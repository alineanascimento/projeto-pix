# 💰 Sistema de Pagamentos PIX

API REST para processamento de pagamentos via PIX desenvolvida durante o curso de Python da Rocketseat. Sistema completo com confirmação em tempo real usando WebSockets.

## 🚀 Tecnologias

- Python
- Flask
- Flask-SocketIO
- SQLAlchemy
- SQLite

## 📦 Instalação

1. Clone o repositório:
```bash
git clone https://github.com/alineanascimento/projeto-pix.git
cd projeto-pix
```

2. Crie um ambiente virtual:
```bash
python -m venv venv
```

3. Ative o ambiente virtual:
```bash
# Windows
venv\Scripts\activate

# Linux/Mac
source venv/bin/activate
```

4. Instale as dependências:
```bash
pip install -r requirements.txt
```

5. Inicialize o banco de dados:
```bash
flask shell
>>> db.create_all()
>>> exit()
```

## ▶️ Como usar

1. Execute o servidor:
```bash
python app.py
```

2. A API estará disponível em:
```
http://localhost:5000
```

## 📡 Endpoints da API

### Criar pagamento PIX
```http
POST /payments/pix
Content-Type: application/json

{
  "value": 100.00
}
```

**Resposta:**
```json
{
  "message": "The payment has been created",
  "payment": {
    "id": 1,
    "value": 100.00,
    "qr_code": "qr_code_filename",
    "bank_payment_id": "uuid",
    "expiration_date": "2024-01-01T12:30:00"
  }
}
```

### Obter QR Code
```http
GET /payments/pix/qr_code/{file_name}
```

**Nota:** Os QR Codes são salvos em `static/img/`

### Confirmar pagamento
```http
POST /payments/pix/confirmation
Content-Type: application/json

{
  "bank_payment_id": "uuid",
  "value": 100.00
}
```

### Visualizar página de pagamento
```http
GET /payments/pix/{payment_id}
```

## 🎯 Funcionalidades

- ✅ Criação de pagamentos PIX com expiração de 30 minutos
- ✅ Geração automática de QR Code
- ✅ Confirmação de pagamentos via webhook
- ✅ Notificações em tempo real via WebSocket
- ✅ Interface web para visualização de pagamentos
- ✅ Validação de dados e valores

## 📁 Estrutura do Projeto

```
projeto-pix/
├── app.py                      # Aplicação principal e rotas
├── requirements.txt            # Dependências do projeto
├── .gitignore                  # Arquivos ignorados pelo Git
├── db_models/                  # Modelos do banco de dados
│   └── payment.py             # Modelo Payment
├── payments/                   # Lógica de pagamentos PIX
│   └── pix.py                 # Classe Pix
├── repository/                 # Camada de acesso aos dados
│   └── database.py            # Configuração do SQLAlchemy
├── templates/                  # Templates HTML
│   ├── payment.html           # Página de pagamento
│   ├── confirmed_payment.html # Página de confirmação
│   └── 404.html               # Página de erro
├── static/                     # Arquivos estáticos
│   ├── css/                   # Estilos CSS
│   └── img/                   # QR Codes gerados
└── tests/                      # Testes unitários
    └── test_pix.py            # Testes da classe Pix
```

## 🔄 Fluxo de Pagamento

1. Cliente faz POST em `/payments/pix` com o valor
2. Sistema gera QR Code e retorna dados do pagamento
3. Cliente acessa `/payments/pix/{id}` para visualizar o QR Code
4. Quando o pagamento é confirmado (webhook), o sistema:
   - Marca o pagamento como pago
   - Emite evento via WebSocket
   - Atualiza a página automaticamente

## 🧪 Testes

Execute os testes unitários:
```bash
pytest tests/
```

## ⚙️ Configuração

O projeto usa SQLite por padrão. Para mudar o banco de dados, edite em `app.py`:

```python
app.config['SQLALCHEMY_DATABASE_URI'] = 'sua-connection-string'
```

## 🔒 Segurança

Em produção, certifique-se de:
- Alterar a `SECRET_KEY`
- Usar HTTPS
- Validar webhooks com assinaturas
- Implementar rate limiting

---

Projeto desenvolvido durante o curso de Python da **Rocketseat** 🚀

## 📚 Aprendizados

- Criação de APIs REST com Flask
- Integração com sistema de pagamentos
- WebSockets para comunicação em tempo real
- Boas práticas de arquitetura (Repository Pattern)
- Testes unitários com Pytest
