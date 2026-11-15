# Roteiro do Vídeo Pitch e Checklist Final — SmartRetail Network (Ana Fashion)

> **Duração alvo:** **3m30s a 3m50s** (tempo sugerido do roteiro: **3m45s**, respeitando rigorosamente o limite máximo de **4 minutos**).  
> **Artefato demonstrado na tela:** [main.pkt](file:///home/radiant/DevHub/Projeto-SmartRetail-Network-UniFECAF/dist/main.pkt) aberto no **Cisco Packet Tracer**.  
> **Fonte Única da Verdade:** Todos os IPs, VLANs, SSIDs, portas e comandos citados abaixo seguem exatamente [plano_rede.md](file:///home/radiant/DevHub/Projeto-SmartRetail-Network-UniFECAF/dist/plano_rede.md) e os scripts aplicados em `dist/`.

---

## 1. Preparação da Tela Antes de Gravar (para ganhar tempo no vídeo)

Antes de apertar **REC** (OBS Studio / gravador de tela), deixe o Packet Tracer preparado para evitar perder segundos procurando menus:

1. **Zoom ideal da topologia:** Ajuste o zoom de [main.pkt](file:///home/radiant/DevHub/Projeto-SmartRetail-Network-UniFECAF/dist/main.pkt) para que todos os setores (`RTR-AnaFashion`, `SW-AnaFashion`, `SRV-Estoque-NVR`, Caixas, Impressora, Câmeras, `AP-Loja`, Tablets, `Maquininha-POS` e área `WAN / INTERNET ZONE`) caibam inteiros e legíveis na tela.
2. **Janelas pré-abertas (minimizadas ou em barra inferior):**
   - `SW-AnaFashion` (CLI) já logado no modo privilegiado (`#`) e pronto para `show vlan brief` / `show interfaces trunk`.
   - `RTR-AnaFashion` (CLI) já logado (`#`) pronto para `show ip dhcp binding` e `show access-lists`.
   - `AP-Loja` na aba `Config → Port 1` mostrando SSID `AnaFashion-Corp` e `WPA2-PSK`.
   - `SRV-Estoque-NVR` na aba `Services → DNS` mostrando os registros `estoque.anafashion.local` e `nvr.anafashion.local`.
   - Prompt de comando (`Desktop → Command Prompt`) pré-aberto em **`Caixa-01`**, **`Tablet-01`**, **`Maquininha-POS`** e **`CAM-IP-02`**.
3. **Dica de ouro (seta para cima `↑`):** Antes de gravar, execute uma vez cada `ping` nos 4 terminais para que os comandos fiquem no histórico; durante a gravação, use a **seta para cima (`↑`)** e pressione **Enter** para reexecutar na hora sem erros de digitação. *(Opcional no `Caixa-01`: deixe o `Desktop → Web Browser` aberto em `http://8.8.8.8` se quiser mostrar `show ip nat translations` ao vivo).*

---

## 2. Roteiro Cronometrado (Total: 03m45s)

| Tempo | Bloco (Obrigatório UniFECAF) | O que mostrar na tela (Packet Tracer) | Duração |
| :---: | :--- | :--- | :---: |
| `00:00 – 00:25` | 1. Abertura e Diagnóstico do Problema | Visão geral completa da topologia (`main.pkt`) | `25s` |
| `00:25 – 00:55` | 2. Arquitetura, Equipamentos e Substitutos | Zoom visual nos blocos: Core, WAN e 12 dispositivos finais | `30s` |
| `00:55 – 01:40` | 3. Endereçamento IPv4, Segmentação (VLANs) e Serviços | CLI `SW-AnaFashion`, CLI `RTR-AnaFashion` e DNS do `SRV-Estoque-NVR` | `45s` |
| `01:40 – 02:15` | 4. Integração dos Dispositivos | Aba Wi-Fi do `AP-Loja` + indicação dos fluxos na topologia | `35s` |
| `02:15 – 03:10` | 5. Demonstração ao Vivo dos Testes e Bloqueios | Prompts de `Caixa-01`, `Tablet-01`, `Maquininha-POS`, `CAM-IP-02` e `show access-lists` | `55s` |
| `03:10 – 03:45` | 6. Segurança, Disponibilidade, Expansão e Fechamento | Retorno à topologia geral destacando portas/VLANs livres | `35s` |

---

### Bloco 1 — Abertura e Problema (00:00 – 00:25)
- **Na tela:** Topologia geral de [main.pkt](file:///home/radiant/DevHub/Projeto-SmartRetail-Network-UniFECAF/dist/main.pkt) centralizada, apontando com o mouse o conjunto de vendas, servidor, câmeras e dispositivos móveis.
- **Fala sugerida:**
  > "Olá! Este é o projeto **SmartRetail Network**, desenvolvido para modernizar a rede da loja **Ana Fashion**. O negócio precisava integrar vendas, estoque, pagamentos, atendimento móvel e vigilância CFTV sem sofrer com instabilidade, lentidão ou riscos de segurança. Nosso objetivo foi responder como estruturar uma rede de pequeno porte que integre tudo isso com segurança, disponibilidade e espaço para crescer."

---

### Bloco 2 — Proposta de Arquitetura e Equipamentos (00:25 – 00:55)
- **Na tela:** Passe o cursor sobre `SW-AnaFashion` (`Switch 2960-24TT`), `RTR-AnaFashion` (`Router 2911`), `AP-Loja` (`AccessPoint-PT`), `SRV-Estoque-NVR` (`Server-PT`), os 12 dispositivos finais e o trecho WAN (`Modem-WAN → Cloud-PT → ISP-WAN → Internet-Test`).
- **Fala sugerida:**
  > "Adotamos uma topologia física em **Estrela** com arquitetura lógica **Router-on-a-Stick**. O switch `SW-AnaFashion` 2960-24TT interliga toda a rede cabeada por um tronco 802.1Q Gigabit até o roteador `RTR-AnaFashion` 2911. Temos 3 caixas, 1 impressora de rede, 1 servidor local de estoque e NVR, 1 Access Point dedicado operando em ponte Camada 2 com 2 tablets, além dos substitutos técnicos do Packet Tracer: `Smartphone-PT` representando a maquininha POS via Wi-Fi, 4 `PC-PT` para as câmeras IP com suporte completo a testes em CLI, e um `Cable-Modem` como ponte física para a nuvem WAN."

---

### Bloco 3 — Endereçamento IPv4, Segmentação (VLANs) e Serviços (00:55 – 01:40)
- **Na tela:**
  1. (`00:55`) Abra o CLI do `SW-AnaFashion` e rode `show vlan brief` (e se já estiver visível, aponte rapidamente as portas).
  2. (`01:12`) Alterne para o CLI do `RTR-AnaFashion` e mostre `show ip dhcp binding`.
  3. (`01:28`) Mostre rapidamente a aba `Services → DNS` do `SRV-Estoque-NVR`.
- **Fala sugerida:**
  > "Para isolar domínios de broadcast e facilitar a auditoria, adotamos o padrão `192.168.<VLAN>.0/24` com cinco VLANs: **VLAN 10** para Vendas/PDV, **VLAN 20** para Servidores, **VLAN 30** para CFTV, **VLAN 40** para a rede sem fio móvel e **VLAN 99** como nativa e de gerência, mitigando *VLAN Hopping*. Infraestrutura, servidor (`192.168.20.10`), impressora (`192.168.10.10`) e as quatro câmeras (`192.168.30.11` a `.14`) possuem **IP estático** pela previsibilidade crítica. Já os 3 caixas, os 2 tablets e a maquininha usam **DHCP** centralizado no roteador na faixa `.100` a `.254`, mantendo `.1` a `.99` livres. O roteador também faz **NAT Overload** na saída `203.0.113.2`, enquanto o servidor local provê **DNS interno** para `estoque.anafashion.local` e `nvr.anafashion.local`."

---

### Bloco 4 — Integração dos Dispositivos (01:40 – 02:15)
- **Na tela:**
  1. (`01:40`) Mostre a aba `Config → Port 1` do `AP-Loja` (`SSID: AnaFashion-Corp`, `WPA2-PSK` com `AES`).
  2. (`01:55`) Volte para a topologia geral e aponte o caminho de cada grupo até o servidor `192.168.20.10` e a Internet (`8.8.8.8`).
- **Fala sugerida:**
  > "Na prática, todos os setores trabalham integrados pelas sub-interfaces do roteador sem abrir brechas laterais. No salão, o `AP-Loja` opera com SSID `AnaFashion-Corp`, autenticação **WPA2-PSK** e criptografia **AES**, conectando os tablets e a maquininha à VLAN 40. Os caixas acessam continuamente o estoque e a impressora na VLAN 10; os tablets consultam o estoque no servidor; a maquininha chega às operadoras via Internet; e as câmeras gravam diretamente no servidor NVR sem exposição externa."

---

### Bloco 5 — Demonstração Prática dos Testes ao Vivo (02:15 – 03:10)
- **Na tela:** Use as janelas já abertas em cascata (tecle `↑` + `Enter` em cada uma):
  1. (`02:15`) **`Caixa-01` (`192.168.10.100`):** rode `ping estoque.anafashion.local` e `ping 192.168.10.10` (ambos **Reply** com `0% loss`).
  2. (`02:30`) **`Tablet-01` (`192.168.40.100`):** rode `ping 192.168.20.10` (**Reply** OK) e em seguida `ping 192.168.30.11` (mostrando **Destination host unreachable** pelo bloqueio da `ACL_VLAN40_IN`).
  3. (`02:45`) **`Maquininha-POS` (`192.168.40.102`):** rode `ping 8.8.8.8` (**Reply from 8.8.8.8** — saída WAN + NAT funcionando).
  4. (`02:54`) **`CAM-IP-02` (`192.168.30.12`):** rode `ping 192.168.20.10` (**Reply** no NVR) e `ping 8.8.8.8` (**Destination host unreachable** pela `ACL_VLAN30_IN`).
  5. (`03:03`) **`RTR-AnaFashion`:** mostre `show access-lists` destacando os contadores `(matches)` em `permit` e `deny`.
- **Fala sugerida:**
  > "Vamos validar isso ao vivo. No **Caixa-01**, o ping para `estoque.anafashion.local` resolve via DNS interno para `192.168.20.10` e alcança a impressora `192.168.10.10` sem perdas. No **Tablet-01**, há acesso ao servidor de estoque, mas ao tentar pingar a câmera `192.168.30.11`, o roteador bloqueia na hora por ACL. Já a **Maquininha-POS** navega normalmente até o servidor externo `8.8.8.8` via NAT. Por fim, a **CAM-IP-02** grava no NVR em `192.168.20.10`, mas é totalmente bloqueada para a Internet e para os caixas — e o comando `show access-lists` no roteador confirma todos esses pacotes filtrados em tempo real."

---

### Bloco 6 — Segurança, Disponibilidade, Expansão e Encerramento (03:10 – 03:45)
- **Na tela:** Volte para a visão panorâmica da topologia (`dist/main.pkt`), destacando a organização dos cabos verdes e a área livre no switch.
- **Fala sugerida:**
  > "Além das **ACLs estendidas** com menor privilégio, aplicamos **hardening** desativando as **15 portas ociosas** do switch na VLAN 99 e protegendo o acesso CLI com senhas criptografadas. Pensando no futuro da Ana Fashion, já deixamos essas 15 portas reservadas por setor, **155 IPs disponíveis** nos pools DHCP, exceção modular pronta caso os tablets precisem usar a impressora fixa, e as sub-redes **VLAN 50** e **VLAN 60** planejadas para Wi-Fi de visitantes e escritório administrativo. Assim, entregamos uma rede segura, estável e pronta para escalar sem retrabalho. Muito obrigado!"

---

## 3. Checklist Final de Entrega no AVA (UniFECAF)

Confira cada item antes de enviar no portal do aluno:

### Entregável 1 — Relatório Técnico em PDF (Peso: 25%)
- [ ] Revisar [RELATORIO.md](file:///home/radiant/DevHub/Projeto-SmartRetail-Network-UniFECAF/docs/RELATORIO.md) garantindo que todos os **10 prints** de `dist/prints/` apareçam corretamente no documento final.
- [ ] Exportar o relatório completo para **PDF** (via Google Docs, LibreOffice ou conversor Markdown mantendo as tabelas e figuras nítidas).
- [ ] Conferir o PDF gerado página por página:
  - [ ] Os 9 tópicos obrigatórios + resposta destacada à questão central estão completos e legíveis.
  - [ ] Nenhuma imagem cortada e nenhuma tabela quebrada de forma ilegível.
  - [ ] Todos os IPs, VLANs, SSIDs e hostnames do PDF batem 100% com [main.pkt](file:///home/radiant/DevHub/Projeto-SmartRetail-Network-UniFECAF/dist/main.pkt).

### Entregável 2 — Arquivo Cisco Packet Tracer e Evidências (Peso: 50%)
- [ ] Confirmar que [main.pkt](file:///home/radiant/DevHub/Projeto-SmartRetail-Network-UniFECAF/dist/main.pkt) foi salvo após todas as configurações (`write memory` no switch e roteadores + `Ctrl+S` no Packet Tracer).
- [ ] Manter uma cópia de backup segura do `.pkt` (e conferir se todos os links na topologia estão verdes e os 6 dispositivos DHCP ativos).
- [ ] Verificar se os 10 prints de `dist/prints/` estão preservados caso o envio exija também os anexos de evidência separadamente.

### Entregável 3 — Vídeo Pitch no YouTube (Peso: 25%)
- [ ] Gravar o vídeo seguindo o roteiro acima com duração **menor ou igual a 4m00s** (ideal entre **3m30s e 3m50s**) e áudio claro.
- [ ] Publicar o vídeo no YouTube como **Não Listado** (ou **Público** — nunca *Privado*, senão o professor não consegue assistir).
- [ ] **Teste obrigatório em aba anônima:** Abrir o link final do YouTube em uma janela anônima/deslogada do navegador e confirmar que o vídeo reproduz normalmente com som e nitidez de tela.

### Conferência Final no Portal (AVA)
- [ ] Anexar o **PDF do Relatório Técnico**.
- [ ] Anexar o arquivo **`.pkt`** ([main.pkt](file:///home/radiant/DevHub/Projeto-SmartRetail-Network-UniFECAF/dist/main.pkt)).
- [ ] Informar/verificar o **link do vídeo no YouTube** (sem espaços extras ou URL quebrada).
- [ ] Confirmar o protocolo/comprovante de envio no AVA antes do prazo final (**15/11**).
