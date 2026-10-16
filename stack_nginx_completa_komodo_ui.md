# 🚀 Stack Nginx Completa com Docker Compose & Komodo UI

Este repositório contém uma estrutura completa e comentada para implantação do **Nginx** utilizando **Docker Compose** e o **Komodo UI**. Foi desenvolvido especialmente para fins pedagógicos, explicando detalhadamente o papel de cada parâmetro do Docker.

---

## 📂 Estrutura de Diretórios no Host

Antes de executar a stack no Komodo UI, a seguinte estrutura de diretórios e arquivos deve ser criada no servidor host (exemplo em `/opt/docker/nginx-demo`):

```
/opt/docker/nginx-demo/
├── docker-compose.yml
├── config/
│   └── default.conf
├── html/
│   └── index.html
└── logs/
```

---

## 📄 1. Arquivo `docker-compose.yml`

Este arquivo define os serviços, redes, volumes e verificações de integridade. Copie e cole este conteúdo no editor de Stacks do Komodo UI:

```yaml
version: '3.8'

services:
  # Nome do serviço interno dentro do ecossistema do Compose
  webserver:
    # Imagem oficial do Nginx. A tag 'alpine' é usada por ser extremamente leve (~10MB) e segura.
    image: nginx:alpine
    
    # Nome fixo atribuído ao container em execução (facilita comandos no terminal como 'docker logs nginx_demo')
    container_name: nginx_demo
    
    # Política de reinicialização: garante que o container volte a subir automaticamente se falhar ou se o host reiniciar
    restart: unless-stopped
    
    # Mapeamento de portas: [Porta do Servidor Host]:[Porta Interna do Container]
    # O uso de ${VAR:-padrão} permite configurar portas via variáveis no Komodo UI com um valor fallback
    ports:
      - "${HTTP_PORT:-80}:80"     # Expõe a porta HTTP principal
      - "${HTTPS_PORT:-443}:443"  # Expõe a porta HTTPS para tráfego encriptado
    
    # Variáveis de ambiente injetadas dentro do container
    environment:
      - TZ=Europe/Lisbon # Define o fuso horário interno do container para sincronização de logs
      
    # Mapeamento de volumes (Bind Mounts): liga pastas do seu servidor host às pastas do container
    # Sintaxe: [Caminho no Host]:[Caminho no Container]:[Permissão]
    volumes:
      # Arquivos do site (HTML/CSS/JS). 'ro' (read-only) impede que o Nginx altere os ficheiros no host
      - ./html:/usr/share/nginx/html:ro
      
      # Ficheiro de configuração de virtual hosts do Nginx
      - ./config/default.conf:/etc/nginx/conf.d/default.conf:ro
      
      # Armazenamento persistente de logs de acesso e erros fora do container
      - ./logs:/var/log/nginx
      
    # Verificação de integridade do serviço (Healthcheck)
    # Permite ao Komodo UI e ao Docker monitorizar se o servidor web está realmente a responder a requisições
    healthcheck:
      test: ["CMD-SHELL", "wget --quiet --tries=1 --spider http://localhost/ || exit 1"]
      interval: 15s     # Frequência de verificação
      timeout: 5s       # Tempo limite de resposta de cada teste
      retries: 3        # Número de falhas consecutivas para marcar o container como 'unhealthy'
      start_period: 5s  # Período de tolerância durante o arranque inicial
      
    # Associação do container a uma rede isolada
    networks:
      - app_net

# Definição das redes virtuais do Docker
networks:
  app_net:
    name: nginx_app_network
    driver: bridge # O driver 'bridge' cria uma rede privada isolada no host para os containers
```

---

## 🛠️ 2. Arquivos de Suporte no Host

Execute os passos abaixo no terminal do seu servidor para criar as pastas e arquivos necessários.

### Passos de Criação:

1. **Criar os diretórios:**
   ```bash
   mkdir -p /opt/docker/nginx-demo/html /opt/docker/nginx-demo/config /opt/docker/nginx-demo/logs
   ```

2. **Criar o arquivo de configuração `config/default.conf`:**
   ```nginx
   server {
       listen 80;
       server_name localhost;

       location / {
           root   /usr/share/nginx/html;
           index  index.html index.htm;
       }

       # Redirecionamento de páginas de erro personalizadas
       error_page   500 502 503 504  /50x.html;
       location = /50x.html {
           root   /usr/share/nginx/html;
       }
   }
   ```

3. **Criar a página HTML de teste `html/index.html`:**
   ```html
   <!DOCTYPE html>
   <html lang="pt">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Nginx no Komodo UI</title>
       <style>
           body { font-family: Arial, sans-serif; text-align: center; margin-top: 50px; background-color: #f4f4f9; }
           h1 { color: #2c3e50; }
           p { color: #7f8c8d; }
       </style>
   </head>
   <body>
       <h1>Stack Nginx Ativa!</h1>
       <p>Container implementado com sucesso via <strong>Komodo UI</strong>.</p>
   </body>
   </html>
   ```

---

## ⚙️ 3. Variáveis de Ambiente no Komodo UI (`.env`)

No Komodo UI, você pode personalizar as portas de escuta do container sem alterar o arquivo `docker-compose.yml`. 

Na seção **Environment** da Stack no Komodo, adicione as variáveis:

```env
HTTP_PORT=8080
HTTPS_PORT=8443
```

---

## 📋 4. Passo a Passo de Implantação no Komodo UI

1. Acesse a interface do **Komodo UI**.
2. No menu lateral, navegue até **Stacks** > **Create Stack**.
3. Defina o parâmetro **Directory** para o caminho criado no servidor:
   ```text
   /opt/docker/nginx-demo
   ```
4. Cole o código do arquivo `docker-compose.yml` no editor da Stack.
5. Acesse a aba **Environment** e configure as variáveis de porta (caso queira alterar os valores padrão `80`/`443`).
6. Clique no botão **Build & Deploy**.

---

## 🔍 Como Validar a Implantação

* **Acesso pelo Navegador:** Acesse `http://<IP_DO_SERVIDOR>:8080` (ou porta `80` se não tiver alterado as variáveis).
* **Verificação pelo Terminal:**
  ```bash
  # Verificar container em execução
  docker ps -f name=nginx_demo

  # Visualizar os logs em tempo real
  docker logs -f nginx_demo
  ```