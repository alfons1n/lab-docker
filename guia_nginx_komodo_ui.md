# 🚀 Nginx Web Server com Docker Compose & Komodo UI

Este é um guia prático e simplificado para implantação de um servidor web **Nginx** utilizando **Docker Compose** e a interface de gerenciamento **Komodo UI**. Ideal para iniciantes no ecossistema Docker.

---

## 🛠️ Requisitos Prévios

- Servidor Linux com **Docker** e **Docker Compose** instalados.
- **Komodo UI** configurado e acessível.
- Acesso ao terminal do servidor via SSH ou console.

---

## 📂 Estrutura de Diretórios

Antes de começar, a estrutura final de arquivos no servidor será:

```text
/opt/docker/nginx-simples/
├── docker-compose.yml
└── html/
    └── index.html
```

---

## 📄 Arquivo `docker-compose.yml`

Abaixo está o arquivo de configuração simplificado para subir o container Nginx.

```yaml
version: '3.8'

services:
  # Nome do serviço no Docker Compose
  meu-webserver:
    # Imagem oficial do Nginx em versão leve (Alpine)
    image: nginx:alpine

    # Nome amigável do container
    container_name: nginx-simples

    # Reinicia automaticamente caso falhe ou o servidor reinicie
    restart: unless-stopped

    # Mapeamento de portas: [Porta Host]:[Porta Container]
    ports:
      - "8080:80"

    # Mapeamento de volumes (Pasta local : Pasta do Nginx no container)
    volumes:
      - ./html:/usr/share/nginx/html:ro
```

---

## 📋 Passo a Passo para Implantação

### 1. Criar a estrutura de pastas no servidor

Acesse o terminal do servidor e execute:

```bash
mkdir -p /opt/docker/nginx-simples/html
```

### 2. Criar uma página HTML de teste

Gere o arquivo `index.html` básico dentro da pasta criada:

```bash
echo "<h1>Meu primeiro container no Komodo UI!</h1>" > /opt/docker/nginx-simples/html/index.html
```

### 3. Configurar e Deploy no Komodo UI

1. Acesse o painel do **Komodo UI**.
2. Navegue até **Stacks** > **Create Stack**.
3. Defina o campo **Directory** com o caminho:
   ```text
   /opt/docker/nginx-simples
   ```
4. Cole o conteúdo do arquivo `docker-compose.yml` no editor de código.
5. Clique em **Build & Deploy**.

---

## 🔍 Como Verificar o Funcionamento

1. Abra o navegador e acesse a URL:
   ```text
   http://<IP_DO_SEU_SERVIDOR>:8080
   ```
2. Se a configuração estiver correta, você verá a mensagem:  
   > **Meu primeiro container no Komodo UI!**

---

## 🛠️ Comandos Úteis no Terminal (Opcional)

Se preferir gerenciar o container manualmente via linha de comando no servidor:

- **Subir o container:**
  ```bash
  cd /opt/docker/nginx-simples && docker compose up -d
  ```
- **Verificar status:**
  ```bash
  docker ps -f name=nginx-simples
  ```
- **Parar o container:**
  ```bash
  cd /opt/docker/nginx-simples && docker compose down
  ```