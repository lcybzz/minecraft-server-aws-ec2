# 01 — Configuração da instância EC2

> Criação e configuração da máquina virtual na AWS que hospedará o servidor do Minecraft.

---

## Pré-requisitos

- Conta ativa na AWS ([aws.amazon.com](https://aws.amazon.com))
- Cartão de crédito cadastrado (obrigatório para o cadastro — não há cobrança dentro do Free Tier)

---

## Passo 1 — Acesse o painel do EC2

1. Faça login no [Console da AWS](https://console.aws.amazon.com)
2. Na barra de pesquisa superior, digite **EC2**
3. Clique em **EC2** em Serviços
4. Na barra lateral esquerda, clique em **Instâncias**
5. Clique em **Lançar instâncias**

> 📸 `assets/screenshots/01-ec2-dashboard.png`

---

## Passo 2 — Nomeie a instância

Na seção **Nome e tags**, insira um nome descritivo:

```
minecraft-server
```

---

## Passo 3 — Escolha a AMI (sistema operacional)

Na seção **Imagens de aplicativos e SO**:

| Campo | Valor |
|---|---|
| AMI | **Ubuntu Server 24.04 LTS** |
| Arquitetura | 64 bits (x86) |
| Nível gratuito | ✅ Elegível |

> O Ubuntu 24.04 LTS é a escolha recomendada: estável, amplamente utilizado em servidores de produção e com suporte total por 5 anos.

> 📸 `assets/screenshots/01-ami-selection.png`

---

## Etapa 4 — Escolha o tipo de instância

Na seção **Tipo de instância**:

| Campo | Valor |
|---|---|
| Tipo | `t3.micro` |
| vCPUs | 2 |
| RAM | 1 GB |
| Nível gratuito | ✅ Elegível |

> `t3.micro` oferece poder de computação suficiente para rodar um servidor Minecraft para fins de aprendizagem.

> 📸 `assets/screenshots/01-instance-type.png`

---

## Etapa 5 — Região

Certifique-se de estar operando na região correta da AWS.  
Este projeto usa **Leste dos EUA (Ohio) — `us-east-2`**, visível no canto superior direito do console.

> ⚠️ **Importante:** As instâncias são específicas da região. Se você não conseguir encontrar sua instância mais tarde, verifique se está na região correta.

---

## Próximo passo

➡️ [02 — Par de chaves e grupo de segurança](02-keypair-security-group.md)