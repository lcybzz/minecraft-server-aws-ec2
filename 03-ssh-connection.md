# 03 — Conexão SSH (Windows)

> Conectando-se à instância EC2 via SSH usando o Windows PowerShell e o arquivo de chave `.pem`.

---

## Pré-requisitos

- Status da instância: **Em execução**
- Endereço IPv4 público anotado no painel do EC2
- Arquivo `minecraft-key.pem` baixado e salvo localmente

---

## Passo 1 — Abrir o PowerShell

Pressione `Win + X` e selecione **“Terminal do Windows”** ou **“PowerShell”**.

> O Windows 11 inclui um cliente SSH nativo no PowerShell — não é necessário nenhum software adicional.

---

## Passo 2 — Corrigir permissões do arquivo de chave (se necessário)

Se o SSH retornar um erro de permissão, restrinja o acesso ao arquivo `.pem`:

```powershell
icacls “C:\Users\YOUR_USERNAME\Downloads\minecraft-key.pem” /inheritance:r /grant:r “YOUR_USERNAME:R”
```

**Exemplo:**
```powershell
icacls “C:\Users\luizc\Downloads\minecraft-key.pem” /inheritance:r /grant:r “luizc:R”
```

> Esta etapa pode ser ignorada no Windows 11 — o PowerShell geralmente aceita a chave sem necessidade de ajuste de permissões.

---

## Etapa 3 — Conecte-se via SSH

```powershell
ssh -i “C:\Users\YOUR_USERNAME\Downloads\minecraft-key.pem” ubuntu@YOUR_PUBLIC_IP
```

**Exemplo:**
```powershell
ssh -i “C:\Users\luizc\Downloads\minecraft-key.pem” ubuntu@18.225.185.252
```

### Solicitação na primeira conexão

Na primeira conexão, o SSH perguntará:

```
Não é possível estabelecer a autenticidade do host ‘18.225.185.252’.
Tem certeza de que deseja continuar a conexão? (sim/não/[impressão digital])?
```

Digite `sim` e pressione Enter. Isso adiciona o servidor à sua lista de hosts conhecidos.

---

## Passo 4 — Confirme a conexão bem-sucedida

Uma conexão bem-sucedida exibe a mensagem de boas-vindas do Ubuntu e altera o prompt do terminal para:

```
ubuntu@ip-172-31-44-146:~$
```

> 📸 `assets/screenshots/03-ssh-connected.png`

O formato `ubuntu@ip-<private-ip>:~$` confirma que você agora está operando dentro da instância EC2 remota.

---

## Referência de comandos SSH

| Ação | Comando |
|---|---|
| Conectar | `ssh -i “key.pem” ubuntu@IP` |
| Desconectar | `exit` |
| Reconectar após reinicialização da instância | O mesmo comando com o IP atualizado, se houver alteração |

---

## Solução de problemas

| Erro | Causa | Solução |
|---|---|---|
| `Permissão negada (chave pública)` | Arquivo de chave ou permissões incorretos | Verifique o caminho do `.pem` e execute novamente o `icacls` |
| `Tempo de espera da conexão expirado` | Instância não em execução ou IP incorreto | Verifique o status da instância e o IP no painel do EC2 |
| `Falha na verificação da chave do host` | IP reutilizado para uma nova instância | Execute `ssh-keygen -R OLD_IP` e reconecte-se |

---

## Próximo passo

➡️ [04 — Instalação do servidor](04-server-installation.md)