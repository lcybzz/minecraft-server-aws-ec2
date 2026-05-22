# 02 — Par de chaves e grupo de segurança

> Configurando credenciais de autenticação e regras de firewall antes de iniciar a instância.

---

## Parte 1 — Par de chaves (autenticação SSH)

Um par de chaves é um conjunto de credenciais criptográficas usado para autenticar o acesso SSH ao servidor — sem necessidade de senha.

- **Chave pública:** armazenada pela AWS no servidor
- **Chave privada:** baixada como um arquivo `.pem` para sua máquina local

### Criando o par de chaves

1. Na tela de configuração da instância, localize a seção **Par de chaves (login)**
2. Clique em **“Criar novo par de chaves”** — uma janela modal se abre **sobre a tela atual** (sua configuração é preservada)
3. Preencha os campos:

| Campo | Valor |
|---|---|
| Nome | `minecraft-key` |
| Tipo de par de chaves | RSA |
| Formato do arquivo da chave privada | `.pem` (para PowerShell / Mac / Linux) |

4. Clique em **“Criar par de chaves”**
5. O arquivo `minecraft-key.pem` é baixado automaticamente — **salve-o em um local seguro**

> 📸 `assets/screenshots/02-keypair-modal.png`

> ⚠️ **Importante:** O arquivo `.pem` é fornecido **apenas uma vez**. Se for perdido, você deverá criar um novo par de chaves e reassociá-lo à instância. Nunca compartilhe este arquivo.

---

## Parte 2 — Grupo de segurança (firewall)

Um grupo de segurança atua como um firewall virtual, controlando quais portas aceitam conexões de entrada para a instância.

### Configurações de rede

Na seção **Configurações de rede**:

- **VPC:** padrão (deixe como está)
- **Sub-rede:** Sem preferência
- **Atribuir IP público automaticamente:** Ativar
- **Firewall:** Criar grupo de segurança

> 📸 `assets/screenshots/02-network-settings.png`

### Regras de entrada

Configure **duas regras**:

#### Regra 1 — Acesso SSH (conexão de terminal)

| Campo | Valor |
|---|---|
| Tipo | SSH |
| Protocolo | TCP |
| Porta | 22 |
| Tipo de origem | **Meu IP** |

> Restringir a porta 22 ao seu IP é uma prática recomendada de segurança — isso impede o acesso SSH não autorizado a partir de IPs externos.

#### Regra 2 — Minecraft (conexão de jogo)

Clique em **“Adicionar regra de grupo de segurança”** e preencha:

| Campo | Valor |
|---|---|
| Tipo | TCP personalizado |
| Protocolo | TCP |
| Porta | 25565 |
| Tipo de origem | **Qualquer lugar — 0.0.0.0/0** |

> A porta 25565 deve estar aberta para todos os IPs para que qualquer jogador possa se conectar ao servidor.

> 📸 `assets/screenshots/02-security-group-rules.png`

### Configuração final do grupo de segurança

```
Regras de entrada:
┌─────────────┬──────────┬───────────────────────┐
│ Tipo        │ Porta     │ Origem                │
├─────────────┼──────────┼───────────────────────┤
│ SSH         │ 22       │ Meu IP (x.x.x.x/32)   │
│ TCP personalizado  │ 25565    │ 0.0.0.0/0             │
└─────────────┴──────────┴───────────────────────┘
```

---

## Iniciando a instância

Após configurar o par de chaves e o grupo de segurança:

1. Verifique o resumo da configuração no painel direito
2. Clique em **“Iniciar instância”**
3. Aguarde cerca de 30 segundos até que o status mude para **“Em execução”**
4. Clique no ID da instância e anote o **endereço IPv4 público**

> 📸 `assets/screenshots/02-instance-running.png`

> ⚠️ **Importante:** O IP público **muda** toda vez que a instância é parada e reiniciada. Para um IP permanente, use o **Elastic IP** (gratuito enquanto a instância estiver em execução).

---

## Próximo passo

➡️ [03 — Conexão SSH](03-ssh-connection.md)