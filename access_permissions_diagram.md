# Diagramas do Fluxo de Permissões

## 1. Fluxo de Autenticação

```mermaid
graph TD
    A[Usuário] --> B{Está Autenticado?}
    B -->|Não| C[Web Interface]
    B -->|Não| D[API]
    C --> E[Login Django]
    D --> F[Login JWT]
    E --> G{Autenticação Válida?}
    F --> H{Token Válido?}
    G -->|Sim| I[Acesso Concedido]
    G -->|Não| J[Acesso Negado]
    H -->|Sim| I
    H -->|Não| J
```

## 2. Níveis de Acesso

```mermaid
graph TD
    A[Usuário] --> B{Verificação de Nível}
    B --> C[Usuário Público]
    B --> D[Usuário Autenticado]
    B --> E[Staff Member]
    B --> F[Superusuário]
    
    C --> G[Rate Limit: 50/min]
    D --> H[Rate Limit: 100/min]
    E --> I[Acesso Admin]
    F --> J[Acesso Total]
    
    G --> K[Recursos Públicos]
    H --> L[Recursos Autenticados]
    I --> M[Recursos Admin]
    J --> N[Todos Recursos]
```

## 3. Fluxo de Verificação de Permissões

```mermaid
sequenceDiagram
    participant U as Usuário
    participant A as API/Web
    participant M as Middleware
    participant P as Permissões
    participant R as Recurso

    U->>A: Requisição
    A->>M: Verifica Autenticação
    M->>P: Verifica Permissões
    P->>R: Verifica Acesso ao Recurso
    
    alt Acesso Permitido
        R-->>U: Concede Acesso
    else Acesso Negado
        R-->>U: Retorna Erro
    end
```

## 4. Hierarquia de Permissões

```mermaid
graph TD
    A[Superusuário] --> B[Staff Member]
    B --> C[Usuário Autenticado]
    C --> D[Usuário Público]
    
    A --> E[Todas as Permissões]
    B --> F[Permissões Admin]
    C --> G[Permissões Básicas]
    D --> H[Permissões Públicas]
```

## 5. Fluxo de Tratamento de Erros

```mermaid
graph TD
    A[Erro de Acesso] --> B{Tipo de Erro}
    B --> C[Token Inválido]
    B --> D[Permissão Insuficiente]
    B --> E[Rate Limit Excedido]
    
    C --> F[Retorna 401]
    D --> G[Retorna 403]
    E --> H[Retorna 429]
    
    F --> I[Redireciona para Login]
    G --> J[Mostra Erro de Permissão]
    H --> K[Retorna Mensagem de Limite]
``` 

# Fluxo de Permissões de Acesso (explicação)

## 1. Autenticação

### 1.1 Autenticação Web (Django Admin)
- Utiliza o sistema de autenticação padrão do Django
- Requer login com usuário e senha
- Redireciona para página de login quando não autenticado
- Suporta autenticação de superusuários (is_superuser)

### 1.2 Autenticação API (JWT)
- Utiliza JWT (JSON Web Tokens) para autenticação
- Endpoints de autenticação:
  - `/api/auth/token/` - Geração de token JWT
  - `/api/auth/register/` - Registro de novos usuários
- Token é enviado no header como `Bearer <token>`

## 2. Níveis de Acesso

### 2.1 Usuários Públicos
- Acesso básico à API
- Podem se registrar e autenticar
- Acesso limitado aos próprios dados
- Rate limiting: 50 requisições/minuto

### 2.2 Usuários Autenticados
- Acesso completo à API
- Rate limiting: 100 requisições/minuto
- Podem acessar recursos protegidos
- Acesso aos próprios dados e recursos autorizados

### 2.3 Staff Members
- Acesso ao painel administrativo
- Podem gerenciar conteúdo e usuários
- Acesso a funcionalidades administrativas
- Verificação via `is_staff` flag

### 2.4 Superusuários
- Acesso total ao sistema
- Podem acessar todas as áreas administrativas
- Podem gerenciar todos os recursos
- Verificação via `is_superuser` flag

## 3. Permissões Específicas

### 3.1 Módulos
- **Notícias**
  - `can_view_index` - Visualizar lista de notícias
  - `can_view_detail` - Visualizar detalhes de notícias

- **FAQ**
  - `can_view_index` - Visualizar lista de FAQs

### 3.2 Aplicações Especiais
- **Auditor**
  - Requer autenticação específica
  - Acesso controlado via middleware

## 4. Middleware de Controle de Acesso

### 4.1 LoginRequiredAccess
- Controla acesso a URLs específicas
- Redireciona para login quando necessário
- Verifica autenticação do usuário
- Aplica restrições por aplicação

## 5. Proteção de Recursos

### 5.1 API Endpoints
- Autenticação via JWT
- Verificação de permissões por endpoint
- Rate limiting por tipo de usuário
- Validação de tokens

### 5.2 Web Interface
- Proteção de rotas administrativas
- Verificação de permissões por view
- Redirecionamento para login
- Mensagens de erro personalizadas

## 6. Fluxo de Verificação

1. Requisição recebida
2. Verificação de autenticação
   - Token JWT (API)
   - Sessão Django (Web)
3. Verificação de permissões
   - Nível de acesso do usuário
   - Permissões específicas
4. Acesso concedido/negado
   - Redirecionamento (Web)
   - Resposta HTTP (API)

## 7. Tratamento de Erros

### 7.1 Acesso Negado
- Redirecionamento para página de login
- Mensagens de erro personalizadas
- Logs de tentativas de acesso
- Proteção contra ataques de força bruta

### 7.2 API Errors
- Respostas HTTP apropriadas
- Mensagens de erro em JSON
- Rate limiting excedido
- Token inválido/expirado 
