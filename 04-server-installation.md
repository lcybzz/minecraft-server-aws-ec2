# 04 — Instalação do servidor

> Instalação do Java, download do servidor do Minecraft e execução na instância EC2.

---

## Pré-requisitos

- Sessão SSH ativa na instância EC2
- Prompt exibindo `ubuntu@ip-...:~$`

---

## Passo 1 — Atualizar o sistema

Sempre atualize a lista de pacotes antes de instalar qualquer coisa em um novo servidor:

```bash
sudo apt update && sudo apt upgrade -y
```

> Isso pode levar alguns minutos na primeira execução.

---

## Passo 2 — Instalar o Java 21

O Minecraft 1.21.5 requer o Java 21. Instale o JRE headless (sem componentes GUI — ideal para servidores):

```bash
sudo apt install openjdk-21-jre-headless -y
```

**Verifique a instalação:**

```bash
java -version
```

Saída esperada:
```
openjdk version “21.x.x” 2024-xx-xx
OpenJDK Runtime Environment ...
```

---

## Etapa 3 — Crie o diretório do servidor

Organize os arquivos do servidor em um diretório dedicado:

```bash
mkdir minecraft && cd minecraft
```

---

## Passo 4 — Baixe o JAR do servidor do Minecraft

```bash
wget https://piston-data.mojang.com/v1/objects/e6ec2f64e6080b9b5d9b471b291c33cc7f509733/server.jar -O server.jar
```

> Este é o `server.jar` oficial da Mojang para o Minecraft Java Edition **1.21.5**.

---

## Passo 5 — Aceite o EULA

O Minecraft exige a aceitação explícita do Contrato de Licença de Usuário Final antes de iniciar:

```bash
echo “eula=true” > eula.txt
```

---

## Passo 6 — Inicie o servidor (primeira inicialização)

```bash
java -Xmx900m -Xms512m -jar server.jar nogui
```

| Sinalizador | Descrição |
|---|---|
| `-Xmx900m` | Memória heap máxima: 900 MB (limite seguro para t3.micro com 1 GB de RAM) |
| `-Xms512m` | Memória heap inicial: 512 MB |
| `nogui` | Executa sem interface gráfica (necessário para servidores sem interface gráfica) |

### Saída esperada

A primeira inicialização gera arquivos de configuração e cria o mundo:

```
[ServerMain/ERROR]: Falha ao carregar propriedades do arquivo: server.properties
```
> ✅ Esse erro na primeira inicialização é **normal** — o arquivo ainda não existia e é gerado automaticamente.

```
[Thread do servidor/INFO]: Criando novos dados do mundo
[Thread do servidor/INFO]: Iniciando o servidor Minecraft versão 1.21.5
[Thread do servidor/INFO]: Iniciando o servidor Minecraft em *:25565
[Thread do servidor/INFO]: Concluído (8,843 s)! Para obter ajuda, digite “help”
```

> 📸 `assets/screenshots/04-server-done.png`

A mensagem `Concluído!` confirma que o servidor está em execução e aceitando conexões na porta 25565.

---

## Passo 7 — Configurar o modo online

Por padrão, os servidores do Minecraft exigem uma conta premium (paga) para autenticação. Para permitir conexões do Game Pass ou de outros lançadores, desative o modo online:

Pressione `Ctrl + C` para parar o servidor e, em seguida:

```bash
echo “online-mode=false” >> server.properties
```

Reinicie o servidor:

```bash
java -Xmx900m -Xms512m -jar server.jar nogui
```

---

## Passo 8 — Conecte-se a partir do Minecraft Java Edition

1. Abra o **Minecraft Launcher**
2. Selecione **Minecraft Java Edition** na barra lateral esquerda
3. Defina a versão como **release 1.21.5** (deve corresponder à versão do servidor)
4. Clique em **Jogar**
5. No jogo: **Multijogador → Adicionar servidor**

| Campo | Valor |
|---|---|
| Nome do servidor | `minecraft-server-aws` |
| Endereço do servidor | `SEU_IP_PÚBLICO` |
| Porta | `25565` |

6. Clique em **Concluído → Entrar no servidor**

> 📸 `assets/screenshots/04-minecraft-connected.png`

---

## Referência de status do servidor

| Mensagem de log | Significado |
|---|---|
| `Concluído (Xs)! Para obter ajuda, digite “help”` | ✅ Servidor em execução, pronto para conexões |
| `Servidor vazio por 60 segundos, pausando` | ✅ Normal — o servidor pausa quando ocioso e retoma ao ser conectado |
| `Jogador entrou no jogo` | ✅ Jogador conectado com sucesso |

---

## Mantendo o servidor em execução após fechar o terminal

Por padrão, fechar o PowerShell interrompe o servidor. Para mantê-lo em execução em segundo plano, use `screen`:

```bash
# Instalar o screen
sudo apt install screen -y

# Iniciar uma sessão nomeada
screen -S minecraft

# Executar o servidor dentro da sessão
java -Xmx900m -Xms512m -jar server.jar nogui

# Desconecte-se do screen sem parar o servidor
# Pressione: Ctrl + A, depois D

# Reconecte-se mais tarde
screen -r minecraft
```

---

## Próximo passo

➡️ [05 — Solução de problemas](05-troubleshooting.md)
