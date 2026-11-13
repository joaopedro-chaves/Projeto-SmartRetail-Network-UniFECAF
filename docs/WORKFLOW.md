# WORKFLOW.md — Roteiro Guiado com Confirmação (SmartRetail Network)

## Protocolo de Execução (Regra de Parada Obrigatória)
- **Execução Unitária:** Execute **APENAS UM PASSO** por vez.
- **Trava de Avanço:** Ao concluir o passo atual, pare imediatamente, apresente o resultado/entregável daquela etapa e pergunte: *"Posso avançar para o próximo passo?"*
- **Gatilho de Continuidade:** Só inicie o passo seguinte após confirmação explícita do usuário (ex.: `"confirmado"`, `"ok"`, `"pode seguir"`, `"próximo"`). Se o usuário pedir ajustes, refaça o passo atual antes de avançar.
- **Controle de Estado:** Marque `[x]` no passo concluído assim que o usuário confirmar.

## Fase 1: Arquitetura e Planejamento (13/11)

- [x] **Passo 1 — Levantamento e Inventário de Dispositivos**
  - **Ação da IA:** Listar todos os equipamentos exigidos em `docs/ORIENTACOES.md` (3 caixas, 2 tablets, 1 impressora, 1 máquina de cartão, 1 servidor de estoque/gravação, 4 câmeras IP, roteador/gateway, switch(es), AP e Internet) e definir quais dispositivos substitutos serão usados no Cisco Packet Tracer caso algum ícone não exista nativamente.
  - **Critério de Confirmação:** Usuário aprova a lista de dispositivos e os substitutos escolhidos.

- [x] **Passo 2 — Definição de Segmentação (VLANs) e Regras de Segurança**
  - **Ação da IA:** Propor a divisão lógica da rede (ex.: VLAN 10 Vendas/PDV, VLAN 20 Servidores, VLAN 30 CFTV, VLAN 40 Wi-Fi/Tablets) e definir a matriz de comunicação (quem acessa quem e o que será bloqueado via ACL, garantindo isolamento das câmeras).
  - **Critério de Confirmação:** Usuário valida as VLANs e a política de isolamento.

- [x] **Passo 3 — Plano de Endereçamento IPv4 (Estático vs. DHCP)**
  - **Ação da IA:** Criar o arquivo `dist/plano_rede.md` com a tabela completa de sub-redes, máscaras, gateways, DNS, lista de dispositivos com IP estático (servidor, câmeras, impressora, interfaces) e faixas de pool DHCP (caixas, tablets, maquininha).
  - **Critério de Confirmação:** Usuário confirma que os IPs e faixas estão prontos para serem aplicados.


## Fase 2: Implementação no Cisco Packet Tracer (13/11)

- [ ] **Passo 4 — Guia de Montagem Física da Topologia**
  - **Ação da IA:** Fornecer o mapa exato de conexões porta-a-porta (qual porta do Switch liga em qual equipamento, porta Trunk para o Roteador e ligação do Access Point) para o usuário montar o desenho no Packet Tracer.
  - **Critério de Confirmação:** Usuário confirma que montou os equipamentos e conectou os cabos no Packet Tracer.
  - Caso necessaario pode usar imagens para auxiliar o usuario na montagem da topologia. Prompt para o gemini para gerar as imagens ou use o nano banana.

- [ ] **Passo 5 — Configuração do Switch (VLANs e Portas Access/Trunk)**
  - **Ação da IA:** Gerar o bloco de comandos Cisco IOS limpo para criação das VLANs e atribuição das interfaces no Switch, pronto para colar no terminal CLI.
  - **Critério de Confirmação:** Usuário confirma que aplicou os comandos no Switch sem erros.

- [ ] **Passo 6 — Configuração do Roteador/Gateway (Roteamento, DHCP e DNS)**
  - **Ação da IA:** Gerar o bloco de comandos Cisco IOS para sub-interfaces (Router-on-a-Stick), pools DHCP por VLAN, configuração de DNS e saída para a Internet (WAN).
  - **Critério de Confirmação:** Usuário confirma que aplicou os comandos no Roteador e os dispositivos dinâmicos pegaram IP via DHCP.

- [ ] **Passo 7 — Configuração de IPs Estáticos, Wi-Fi e Segurança (ACLs)**
  - **Ação da IA:** Listar os valores exatos para preencher manualmente nos dispositivos com IP estático (Servidor, Câmeras, Impressora), SSID/senha WPA2 do Access Point/Tablets e o bloco de comandos ACL no Roteador para isolar a VLAN de CFTV.
  - **Critério de Confirmação:** Usuário confirma que toda a topologia está configurada e o arquivo `.pkt` foi salvo.

## Fase 3: Testes e Evidências (14/11)

- [ ] **Passo 8 — Roteiro de Testes de Conectividade e Checklist de Prints**
  - **Ação da IA:** Entregar a lista exata de comandos `ping` que o usuário deve rodar (caixas -> servidor, tablets -> servidor, câmeras -> servidor, bloqueio tablet -> câmeras, acesso Internet) e os nomes exatos dos prints para salvar em `dist/prints/`.
  - **Critério de Confirmação:** Usuário confirma que rodou os testes e salvou todos os prints na pasta.

## Fase 4: Relatório Técnico e Vídeo Pitch (14/11 e 15/11)

- [ ] **Passo 9 — Redação do Relatório Técnico (`dist/relatorio.md`)**
  - **Ação da IA:** Escrever o relatório completo cobrindo os 9 tópicos obrigatórios da UniFECAF + resposta à questão central, utilizando exatamente os IPs, VLANs e equipamentos validados nos passos anteriores, indicando onde inserir cada print antes de exportar para PDF.
  - **Critério de Confirmação:** Usuário revisa o texto, insere os prints e confirma a exportação do PDF.

- [ ] **Passo 10 — Roteiro do Vídeo Pitch (`dist/pitch.md`) e Checklist Final**
  - **Ação da IA:** Gerar o roteiro cronometrado (3m30s a 3m50s, dentro do limite de 4 minutos) guiando o que mostrar na tela do Packet Tracer em cada segundo, mais o checklist final de entrega no AVA.
  - **Critério de Confirmação:** Trabalho finalizado e pronto para gravação e envio.