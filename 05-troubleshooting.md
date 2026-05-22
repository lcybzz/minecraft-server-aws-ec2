# 05 — Resolução de problemas

> Erros encontrados durante este projeto, suas causas e soluções.

---

## Erros de SSH e conexão

### `Erro de mapeamento %USERNAME%` no icacls

**Erro:**
```
%luizc%: Não foi realizado nenhum mapeamento entre nomes de conta e IDs de segurança.
Processados 0 arquivos; Falha no processamento de 1 arquivo
```

**Causa:** A variável de ambiente `%USERNAME%` não foi expandida corretamente no comando.

**Solução:** Substitua a variável pelo nome de usuário literal:
```powershell
icacls “C:\Users\luizc\Downloads\minecraft-key.pem” /inheritance:r /grant:r “luizc:R”
```

---

### `Conexão recusada: getsockopt`

**Erro:**
```
Falha ao conectar ao servidor
Conexão recusada: getsockopt
```

**Causa:** O processo do servidor Minecraft não está em execução na instância EC2.

**Solução:** Reconecte-se via SSH e inicie o servidor:
```bash
cd minecraft && java -Xmx900m -Xms512m -jar server.jar nogui
```

---

### `Host desconhecido`

**Erro:**
```
Falha ao conectar ao servidor
Host desconhecido
```

**Causa:** O endereço do servidor inserido no Minecraft está incorreto ou o IP da instância mudou após uma reinicialização.

**Solução:**
1. Acesse **Console da AWS → EC2 → Instâncias**
2. Clique na sua instância e verifique o **Endereço IPv4 público** atual
3. Atualize o endereço do servidor no Minecraft com o novo IP

---

### `Nenhuma instância nesta região`

**Sintoma:** A lista de instâncias EC2 está vazia.

**Causa:** O Console da AWS está configurado para uma região diferente daquela em que a instância foi criada.

**Solução:** Clique no seletor de região no canto superior direito do Console da AWS e alterne para **US East (Ohio) — us-east-2**.

---

## Erros do cliente do Minecraft

### `Cliente incompatível! Use a versão 1.21.5`

**Erro:**
```
Falha ao conectar ao servidor
Cliente incompatível! Use a versão 1.21.5
```

**Causa:** A versão do cliente do Minecraft é mais recente do que a versão do servidor (1.21.5).

**Solução:**
1. Abra o **Minecraft Launcher**
2. Vá para **Instalações → Nova instalação**
3. Selecione a versão **release 1.21.5**
4. Salve e inicie a partir desta instalação

---

### `NetherNet / Falha ao conectar ao modo multijogador`

**Erro:**
```
Seu cliente está tendo dificuldades para se conectar aos serviços multijogador.
Pesquise “NetherNet” em help.Minecraft.net para obter mais informações.
```

**Causa:** Este erro aparece ao usar a **Minecraft Bedrock Edition** em vez da Java Edition. A Bedrock usa um protocolo diferente (RakNet) e **não é compatível** com servidores Java.

**Solução:** No Minecraft Launcher, selecione **Minecraft Java Edition** na barra lateral esquerda — não a Bedrock.

> O detalhe do erro `Transport: RakNet` na tela de erro confirma que a Bedrock Edition está sendo usada.

---

### `Não é possível conectar-se ao servidor` (X vermelho na lista de servidores)

**Causa:** Ou o servidor não está em execução, ou o IP/porta está incorreto.

**Lista de verificação para correção:**
- [ ] Confirme se a instância está **em execução** no painel do EC2
- [ ] Confirme se o **IP público** correto está definido no endereço do servidor do Minecraft
- [ ] Confirme se a porta **25565** está aberta no Grupo de Segurança
- [ ] Confirme se o processo do servidor do Minecraft está em execução (verifique o terminal do PowerShell)

---

## Problemas no Console da AWS

### Instância parada automaticamente

**Causa:** A AWS pode parar instâncias sob certas condições do Nível Gratuito, ou a instância foi parada manualmente.

**Solução:**
1. Vá para **EC2 → Instâncias**
2. Selecione a instância
3. Clique em **Estado da instância → Iniciar**
4. Aguarde até que o status volte para **Em execução**
5. Anote o novo IP público (ele muda a cada inicialização)

---

## Alertas de custo e cobrança

### Evitando cobranças inesperadas

Sempre pare a instância quando não estiver em uso:

```
EC2 → Instâncias → Estado da instância → Parar
```

Configure um alerta de cobrança para receber um e-mail antes que qualquer cobrança ocorra:

```
Console da AWS → Faturamento → Orçamentos → Criar orçamento
Defina o limite: $1,00
```

Monitore o uso do Nível Gratuito:

```
Console da AWS → Faturamento → Uso do Nível Gratuito
```

> A instância t3.micro tem **750 horas gratuitas por mês durante 12 meses** — o suficiente para funcionar 24 horas por dia, 7 dias por semana.

---

## Referência rápida — Comandos do servidor

Depois de conectado via SSH e dentro do diretório `minecraft/`:

| Ação | Comando |
|---|---|
| Iniciar servidor | `java -Xmx900m -Xms512m -jar server.jar nogui` |
| Parar servidor | `Ctrl + C` no terminal |
| Iniciar com screen | `screen -S minecraft` e, em seguida, comando de inicialização |
| Desconectar do screen | `Ctrl + A` e depois `D` |
| Reconectar ao screen | `screen -r minecraft` |
| Verificar versão do Java | `java -version` |
| Verificar processos em execução | `ps aux \| grep java` |

---

*Voltar para [README](../README.md)*
