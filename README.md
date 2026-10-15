# lab-docker


# Criar usuario
1. Criar o usuário
O comando adduser é o mais recomendado no Debian, pois cria a pasta /home, configura o shell e solicita a senha interativamente:

Bash
adduser nome_do_usuario
(Substitua nome_do_usuario pelo nome desejado)

2. Dar permissões de administrador (Sudo) — Opcional
Instale o pacote sudo caso ainda não esteja instalado:

Bash
apt update && apt install -y sudo
Adicione o novo usuário ao grupo sudo:

Bash
usermod -aG sudo nome_do_usuario
3. Testar o acesso
Para alternar para o novo usuário criado:

Bash
su - nome_do_usuario

# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/debian
Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update