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
