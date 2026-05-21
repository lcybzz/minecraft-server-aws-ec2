# 🎮 Servidor Minecraft no AWS EC2

> Projeto prático de computação em nuvem: hospedar um servidor Minecraft Java Edition 1.21.5 no AWS EC2 usando Ubuntu, SSH e Grupos de Segurança — apenas com o Nível Gratuito.

O objetivo deste projeto **não** é jogar Minecraft.  
O objetivo é experimentar conceitos reais de infraestrutura em nuvem utilizados na indústria.

---

## 📐 Arquitetura

```
[Máquina local — Windows 11]
         |
         | SSH — Porta 22 (autenticação por par de chaves .pem)
         |
         ▼
[AWS EC2 — Ubuntu 24.04 LTS — t3.micro — us-east-2]
         |
         | Java 21 Runtime + Minecraft server.jar 1.21.5
         |
         ▼
[Porta 25565 — aberta aos jogadores via Grupo de Segurança]
```

---

## 🧠 Conceitos abordados

| Conceito | Descrição |
|---|---|
| **EC2** | Elastic Compute Cloud — máquinas virtuais na AWS |
| **AMI** | Amazon Machine Image — modelo de sistema operacional (Ubuntu 24.04 LTS) |
| **Par de chaves** | Chave criptográfica `.pem` para autenticação SSH |
| **Grupo de segurança** | Firewall virtual que controla o tráfego de entrada/saída por porta e IP |
| **SSH** | Secure Shell — acesso remoto ao servidor Linux via terminal |
| **Nível gratuito** | 750 horas/mês de t2.micro por 12 meses sem custo |
| **IP público** | Endereço voltado para a Internet atribuído à instância |

---

## 📁 Estrutura do projeto

```
minecraft-server-aws-ec2/
│
├── README.md                          ← Você está aqui
│
├── docs/
│   ├── 01-ec2-instance-setup.md       ← Criação da instância EC2
│   ├── 02-keypair-security-group.md   ← Par de chaves e configuração do firewall
│   ├── 03-ssh-connection.md           ← Conexão via SSH no Windows
│   ├── 04-server-installation.md      ← Instalando Java e Minecraft
│   └── 05-troubleshooting.md          ← Erros comuns e soluções
│
├── assets/
│   └── screenshots/                   ← Capturas de tela passo a passo
│
└── LICENSE                            ← Licença MIT
```

---

## 🚀 Início rápido

Siga os documentos na ordem:

1. [Configuração da instância EC2](docs/01-ec2-instance-setup.md)
2. [Par de chaves e grupo de segurança](docs/02-keypair-security-group.md)
3. [Conexão SSH (Windows)](docs/03-ssh-connection.md)
4. [Instalação do servidor](docs/04-server-installation.md)
5. [Solução de problemas](docs/05-troubleshooting.md)

---

## ⚙️ Pilha

| Camada | Tecnologia |
|---|---|
| Provedor de nuvem | AWS (Amazon Web Services) |
| Computação | EC2 t3.micro |
| SO | Ubuntu 24.04 LTS |
| Runtime | OpenJDK 21 |
| Aplicativo | Minecraft Java Edition 1.21.5 |
| Acesso | SSH via Windows PowerShell |
| Região | Leste dos EUA — Ohio (us-east-2) |

---

## ✅ Competências demonstradas

- Provisionamento e gerenciamento de máquinas virtuais no AWS EC2
- Configuração de segurança de rede com Grupos de Segurança
- Autenticação baseada em chave SSH em servidores Linux
- Administração remota de servidores via linha de comando
- Implantação de aplicativos em infraestrutura de nuvem
- Gerenciamento de custos do AWS Free Tier

---

## Autor

**Luiz Felipe Corbelli**  
Estudante de Segurança da Informação — Fatec Araraquara  
Suporte N1 na Capgemini  

[![LinkedIn](https://img.shields.io/badge/LinkedIn-luizcorbelli-blue?style=flat&logo=linkedin)](https://linktr.ee/luizcorbelli)
[![AWS Builder](https://img.shields.io/badge/AWS-Builder%20Center-orange?style=flat&logo=amazonaws)](https://builder.aws.com)

---

*Projeto desenvolvido como exercício prático de Computação em Nuvem — maio de 2026*
