# Lab Docker

Guia passo a passo para preparação do ambiente Debian e instalação do Docker Engine.

---

## 1. Criar Usuário no Debian

### Passo 1: Criar o novo usuário

O comando `adduser` é o mais recomendado no Debian, pois cria a pasta `/home`, configura o shell e solicita a senha interativamente:

```bash
adduser nome_do_usuario
```

*(Substitua `nome_do_usuario` pelo nome desejado)*

### Passo 2: Dar permissões de administrador (Sudo) — *Opcional*

1. Instale o pacote `sudo` caso ainda não esteja instalado:

```bash
apt update && apt install -y sudo
```

2. Adicione o novo usuário ao grupo `sudo`:

```bash
usermod -aG sudo nome_do_usuario
```

### Passo 3: Testar o acesso

Para alternar para o novo usuário criado:

```bash
su - nome_do_usuario
```

---

## 2. Instalação do Docker Engine

### Passo 1: Adicionar a chave GPG oficial do Docker

```bash
sudo apt update
sudo apt install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

### Passo 2: Adicionar o repositório oficial às fontes do Apt

```bash
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/debian
Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF
```

> **Nota para distribuições Testing ou Derivadas:**
> Se você utiliza Debian *testing* ou uma distribuição derivada (como o Kali Linux), pode ser necessário substituir o trecho do comando que imprime o codinome da versão: `(. /etc/os-release && echo "$VERSION_CODENAME")` pelo codinome da versão correspondente do Debian (por exemplo, `trixie` ou `bookworm`).

### Passo 3: Atualizar o índice de pacotes

```bash
sudo apt update
```

### Passo 4: Instalar os pacotes do Docker

Para instalar a versão estável mais recente do Docker Engine, CLI, Containerd e os plugins (Buildx e Compose):

```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Passo 5: Verificar o status do serviço

Após a instalação, verifique se o Docker está em execução:

```bash
sudo systemctl status docker
```

Se o serviço não estiver rodando, inicie-o manualmente:

```bash
sudo systemctl start docker
```

*(Opcional) Para garantir que o serviço inicie junto com o sistema:*

```bash
sudo systemctl enable docker
```

### Passo 6: Validar a instalação

Confirme que a instalação foi concluída com sucesso executando a imagem `hello-world`:

```bash
sudo docker run hello-world
```

*Este comando baixa uma imagem de teste, executa-a em um contêiner, exibe uma mensagem de confirmação e é encerrado.*

---

## 3. Pós-Instalação: Executar o Docker sem `sudo` *(Recomendado)*

Por padrão, o daemon do Docker pertence ao usuário `root`. Para não precisar utilizar `sudo` em todos os comandos do Docker:

1. **Crie o grupo `docker`** *(caso ainda não exista)*:
   ```bash
   sudo groupadd docker
   ```

2. **Adicione o seu usuário ao grupo `docker`**:
   ```bash
   sudo usermod -aG docker $USER
   ```

3. **Atualize as permissões de grupo da sessão atual**:
   ```bash
   newgrp docker
   ```
   *(Alternativamente, faça logout e login novamente na sua conta)*

4. **Teste a execução do Docker sem `sudo`**:
   ```bash
   docker run hello-world


## 2. Instalação do Komodo

estrutura centralizada em /opt/docker/.

Estrutura de Diretórios da Aplicação

/opt/docker/
└── elastic-monitoring/
    ├── docker-compose.yml
    ├── .env
    └── config/
        ├── logstash.yml
        └── pipeline/
            └── logstash.conf

   
https://komo.do/docs/setup/mongo

