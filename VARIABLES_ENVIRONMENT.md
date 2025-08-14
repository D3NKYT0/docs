# Variáveis de Ambiente - ProAGE 2

Este documento lista todas as variáveis de ambiente possíveis utilizadas no projeto ProAGE 2.

## 📋 Índice

- [Configurações Gerais](#configurações-gerais)
- [Configurações de Banco de Dados](#configurações-de-banco-de-dados)
- [Configurações de Email](#configurações-de-email)
- [Configurações de Cache/Redis](#configurações-de-cacheredis)
- [Configurações de Segurança](#configurações-de-segurança)
- [Configurações de Auditoria](#configurações-de-auditoria)
- [Configurações de Deploy](#configurações-de-deploy)
- [Configurações de Upload](#configurações-de-upload)
- [Configurações do Docker](#configurações-do-docker)
- [Configurações do Nginx](#configurações-do-nginx)
- [Configurações do Gunicorn](#configurações-do-gunicorn)

---

## 🔧 Configurações Gerais

### `DEBUG`
- **Descrição**: Habilita/desabilita o modo de debug do Django
- **Tipo**: Boolean
- **Valores**: `True` (desenvolvimento), `False` (produção)
- **Padrão**: `True`
- **Arquivo**: `core/settings.py:18`

### `SECRET_KEY`
- **Descrição**: Chave secreta do Django para criptografia
- **Tipo**: String
- **Valores**: Chave de 32 bytes
- **Padrão**: Gerada automaticamente se não fornecida
- **Arquivo**: `core/settings.py:21`

### `SEND_EMAIL_DEGUB`
- **Descrição**: Habilita/desabilita envio de emails em modo debug
- **Tipo**: Boolean
- **Valores**: `True`, `False`
- **Padrão**: `False`
- **Arquivo**: `core/settings.py:230`

---

## 🗄️ Configurações de Banco de Dados

### `DB_ENGINE`
- **Descrição**: Motor do banco de dados
- **Tipo**: String
- **Valores**: `postgresql`, `mysql`, `sqlite3`, etc.
- **Padrão**: `None` (usa SQLite3)
- **Arquivo**: `core/settings.py:118`

### `DB_HOST`
- **Descrição**: Host do servidor de banco de dados
- **Tipo**: String
- **Valores**: Endereço IP ou hostname
- **Padrão**: `None`
- **Arquivo**: `core/settings.py:121`

### `DB_PORT`
- **Descrição**: Porta do servidor de banco de dados
- **Tipo**: Integer
- **Valores**: Número da porta (ex: 5432 para PostgreSQL)
- **Padrão**: `None`
- **Arquivo**: `core/settings.py:122`

### `DB_NAME`
- **Descrição**: Nome do banco de dados
- **Tipo**: String
- **Valores**: Nome do banco
- **Padrão**: `None`
- **Arquivo**: `core/settings.py:123`

### `DB_USERNAME`
- **Descrição**: Usuário do banco de dados
- **Tipo**: String
- **Valores**: Nome do usuário
- **Padrão**: `None`
- **Arquivo**: `core/settings.py:119`

### `DB_PASS`
- **Descrição**: Senha do banco de dados
- **Tipo**: String
- **Valores**: Senha do usuário
- **Padrão**: `None`
- **Arquivo**: `core/settings.py:120`

---

## 📧 Configurações de Email

### `CONFIG_EMAIL_ENABLE`
- **Descrição**: Habilita/desabilita configuração de email
- **Tipo**: Boolean
- **Valores**: `True`, `False`
- **Padrão**: `False`
- **Arquivo**: `core/settings.py:189`

### `CONFIG_EMAIL_USE_TLS`
- **Descrição**: Habilita/desabilita TLS para conexão SMTP
- **Tipo**: Boolean
- **Valores**: `True`, `False`
- **Padrão**: `None`
- **Arquivo**: `core/settings.py:191`

### `CONFIG_EMAIL_HOST`
- **Descrição**: Servidor SMTP
- **Tipo**: String
- **Valores**: Hostname do servidor SMTP
- **Padrão**: `None`
- **Arquivo**: `core/settings.py:192`

### `CONFIG_EMAIL_HOST_USER`
- **Descrição**: Usuário do servidor SMTP
- **Tipo**: String
- **Valores**: Email ou usuário SMTP
- **Padrão**: `None`
- **Arquivo**: `core/settings.py:193`

### `CONFIG_EMAIL_HOST_PASSWORD`
- **Descrição**: Senha do servidor SMTP
- **Tipo**: String
- **Valores**: Senha do usuário SMTP
- **Padrão**: `None`
- **Arquivo**: `core/settings.py:194`

### `CONFIG_EMAIL_PORT`
- **Descrição**: Porta do servidor SMTP
- **Tipo**: Integer
- **Valores**: Porta SMTP (ex: 587, 465)
- **Padrão**: `None`
- **Arquivo**: `core/settings.py:195`

### `CONFIG_DEFAULT_FROM_EMAIL`
- **Descrição**: Email padrão de origem
- **Tipo**: String
- **Valores**: Endereço de email
- **Padrão**: `None`
- **Arquivo**: `core/settings.py:196`

---

## 🗃️ Configurações de Cache/Redis

### `DJANGO_CACHE_REDIS_URI`
- **Descrição**: URI de conexão com Redis para cache
- **Tipo**: String
- **Valores**: `redis://host:port/db`
- **Padrão**: `None` (usa cache local em desenvolvimento)
- **Arquivo**: `core/settings.py:203`

---

## 🔐 Configurações de Segurança

### `ENCRYPTION_KEY`
- **Descrição**: Chave para criptografia de arquivos
- **Tipo**: String
- **Valores**: Chave de 32 bytes
- **Padrão**: `None`
- **Arquivo**: `core/settings.py:219`

### `DATA_UPLOAD_MAX_MEMORY_SIZE`
- **Descrição**: Tamanho máximo de upload em memória (bytes)
- **Tipo**: Integer
- **Valores**: Tamanho em bytes (ex: 10485760 = 10MB)
- **Padrão**: `10485760`
- **Arquivo**: `core/settings.py:220`

### `SERVE_DECRYPTED_FILE_URL_BASE`
- **Descrição**: URL base para servir arquivos descriptografados
- **Tipo**: String
- **Valores**: Caminho base da URL
- **Padrão**: `'decrypted-file'`
- **Arquivo**: `core/settings.py:221`

---

## 📊 Configurações de Auditoria

### `CONFIG_AUDITOR_MIDDLEWARE_ENABLE`
- **Descrição**: Habilita/desabilita middleware de auditoria
- **Tipo**: Boolean
- **Valores**: `True`, `False`
- **Padrão**: `False`
- **Arquivo**: `core/settings.py:225`

---

## 🚀 Configurações de Deploy

### `RENDER_EXTERNAL_HOSTNAME`
- **Descrição**: Hostname externo para Render (produção)
- **Tipo**: String
- **Valores**: Domínio ou hostname
- **Padrão**: `None`
- **Arquivo**: `core/settings.py:44`

### `RENDER_EXTERNAL_FRONTEND`
- **Descrição**: Hostname do frontend para Render (produção)
- **Tipo**: String
- **Valores**: Domínio ou hostname do frontend
- **Padrão**: `None`
- **Arquivo**: `core/settings.py:48`

---

## 📁 Configurações de Upload

### `MEDIA_ROOT`
- **Descrição**: Diretório raiz para arquivos de mídia
- **Tipo**: Path
- **Valores**: Caminho absoluto
- **Padrão**: `BASE_DIR / 'media'`
- **Arquivo**: `core/settings.py:175`

### `STATIC_ROOT`
- **Descrição**: Diretório raiz para arquivos estáticos
- **Tipo**: Path
- **Valores**: Caminho absoluto
- **Padrão**: `BASE_DIR / 'staticfiles'`
- **Arquivo**: `core/settings.py:172`

---

## 🐳 Configurações do Docker

### `PYTHONDONTWRITEBYTECODE`
- **Descrição**: Evita criação de arquivos .pyc
- **Tipo**: Integer
- **Valores**: `1`
- **Padrão**: `1`
- **Arquivo**: `Dockerfile:5`

### `PYTHONUNBUFFERED`
- **Descrição**: Desabilita buffer de saída do Python
- **Tipo**: Integer
- **Valores**: `1`
- **Padrão**: `1`
- **Arquivo**: `Dockerfile:6`

---

## 🌐 Configurações do Nginx

### `client_max_body_size`
- **Descrição**: Tamanho máximo de upload (configurado no nginx)
- **Tipo**: String
- **Valores**: `10M`
- **Padrão**: `10M`
- **Arquivo**: `nginx/django.conf:15`

---

## ⚙️ Configurações do Gunicorn

### `bind`
- **Descrição**: Endereço e porta para binding do servidor
- **Tipo**: String
- **Valores**: `0.0.0.0:5005`
- **Padrão**: `0.0.0.0:5005`
- **Arquivo**: `gunicorn-cfg.py:8`

### `workers`
- **Descrição**: Número de workers (processos)
- **Tipo**: Integer
- **Valores**: Calculado automaticamente
- **Padrão**: `(2 x CPUs) + 1`
- **Arquivo**: `gunicorn-cfg.py:11`

### `worker_class`
- **Descrição**: Classe de worker
- **Tipo**: String
- **Valores**: `sync`, `gevent`, `uvicorn`
- **Padrão**: `sync`
- **Arquivo**: `gunicorn-cfg.py:14`

### `timeout`
- **Descrição**: Timeout para workers (segundos)
- **Tipo**: Integer
- **Valores**: Tempo em segundos
- **Padrão**: `30`
- **Arquivo**: `gunicorn-cfg.py:26`

### `max_requests`
- **Descrição**: Máximo de requisições por worker
- **Tipo**: Integer
- **Valores**: Número de requisições
- **Padrão**: `1000`
- **Arquivo**: `gunicorn-cfg.py:29`

### `max_requests_jitter`
- **Descrição**: Jitter para max_requests
- **Tipo**: Integer
- **Valores**: Número de requisições
- **Padrão**: `50`
- **Arquivo**: `gunicorn-cfg.py:30`

---

## 📝 Exemplo de Arquivo .env

```bash
# Configurações Gerais
DEBUG=True
SEND_EMAIL_DEGUB=True
SECRET_KEY='sua_chave_secreta_de_32_bytes_aqui'

# Banco de Dados
DB_ENGINE=postgresql
DB_HOST=localhost
DB_NAME=proage_suop
DB_USERNAME=postgres
DB_PASS=sua_senha_aqui
DB_PORT=5432

# Email
CONFIG_EMAIL_ENABLE=True
CONFIG_EMAIL_USE_TLS=True
CONFIG_EMAIL_HOST=smtp.gmail.com
CONFIG_EMAIL_HOST_USER=seu_email@gmail.com
CONFIG_DEFAULT_FROM_EMAIL=seu_email@gmail.com
CONFIG_EMAIL_HOST_PASSWORD=sua_senha_de_app
CONFIG_EMAIL_PORT=587

# Cache/Redis
DJANGO_CACHE_REDIS_URI=redis://redis_suop:6379/0

# Segurança
ENCRYPTION_KEY='sua_chave_de_criptografia_de_32_bytes'
DATA_UPLOAD_MAX_MEMORY_SIZE=10485760
SERVE_DECRYPTED_FILE_URL_BASE=decrypted-file

# Auditoria
CONFIG_AUDITOR_MIDDLEWARE_ENABLE=True

# Deploy (produção)
RENDER_EXTERNAL_HOSTNAME=seu-dominio.com
RENDER_EXTERNAL_FRONTEND=seu-frontend.com
```

---

## 🔍 Configurações Opcionais

### Configurações de Logging
- **Arquivo**: `core/logger.py`
- **Descrição**: Configurações de logging personalizadas

### Configurações de CORS
- **Arquivo**: `core/settings.py:35-50`
- **Descrição**: Configurações de Cross-Origin Resource Sharing

### Configurações de Middleware
- **Arquivos**: `middlewares/`
- **Descrição**: Middlewares customizados para controle de acesso e rate limiting

---

## ⚠️ Observações Importantes

1. **Segurança**: Nunca commite arquivos `.env` com senhas reais
2. **Produção**: Sempre use `DEBUG=False` em produção
3. **Chaves**: Use chaves fortes e únicas para `SECRET_KEY` e `ENCRYPTION_KEY`
4. **Banco de Dados**: Configure corretamente as credenciais do banco
5. **Email**: Use senhas de aplicação para serviços como Gmail
6. **Redis**: Configure corretamente a URI do Redis para cache
7. **Upload**: Ajuste `DATA_UPLOAD_MAX_MEMORY_SIZE` conforme necessário

---

## 📚 Referências

- [Django Settings](https://docs.djangoproject.com/en/stable/ref/settings/)
- [Django Environment Variables](https://django-environ.readthedocs.io/)
- [Gunicorn Configuration](https://docs.gunicorn.org/en/stable/configure.html)
- [Nginx Configuration](https://nginx.org/en/docs/)
- [Redis Configuration](https://redis.io/topics/config)
