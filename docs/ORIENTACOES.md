# Orientações Oficiais — SmartRetail Network (UniFECAF)

## 1. Cenário e Problema (Cliente: Ana Fashion)
- **Negócio:** Loja de roupas em expansão precisando modernizar e estruturar sua rede LAN/WLAN.
- **Dor principal:** Falta de integração organizada, segura e escalável entre vendas, estoque, pagamentos, atendimento móvel e CFTV.
- **Questão central a responder no projeto:** *"Como estruturar uma rede corporativa de pequeno porte capaz de integrar vendas, estoque, pagamentos, dispositivos móveis e CFTV com segurança, disponibilidade e possibilidade de expansão?"*

## 2. Requisitos Funcionais e de Segurança
1. **Continuidade Vendas/Estoque (Crítico):** 3 caixas registradoras acessando simultaneamente o servidor local de estoque sem interrupções ou divergências.
2. **CFTV Isolado e Contínuo:** 4 câmeras IP gravando continuamente no servidor/NVR local, mas **segmentadas logicamente** do tráfego administrativo e de vendas.
3. **Mobilidade (WLAN):** 2 tablets operando em rede Wi-Fi estável para consulta de produtos e apoio às vendas.
4. **Periféricos:** 1 impressora de rede e 1 maquininha de cartão integradas à operação.
5. **Escalabilidade:** Capacidade de adicionar novos dispositivos futuramente sem reconstruir a topologia.

## 3. Entregáveis Obrigatórios e Pesos
- **Parte Teórica (25% — PDF):**
  1. Diagnóstico das necessidades da Ana Fashion;
  2. Proposta geral da arquitetura;
  3. Justificativa da topologia e dos equipamentos escolhidos;
  4. Plano de endereçamento IPv4 (IP, máscara, gateway);
  5. Definição de dispositivos com IP estático vs. DHCP;
  6. Proposta de segmentação da rede;
  7. Principais serviços de rede utilizados (DHCP, DNS, Gateway);
  8. Controles básicos de segurança;
  9. Justificativa de como a solução poderá crescer futuramente (incluindo resposta à questão central).
- **Parte Prática (50% — Cisco Packet Tracer):**
  - Topologia funcional com: Roteador/Gateway, Switch(es), Access Point, Internet (WAN), 3 caixas, 2 tablets, 1 impressora, 1 maquininha de cartão, 1 servidor de estoque/gravação e 4 câmeras IP.
  - Configuração real de IPv4, DHCP, segmentação lógica (VLANs), conexões cabeadas e sem fio.
  - Evidências em prints: topologia, configurações, endereçamento e testes de conectividade (`ping` bem-sucedidos e bloqueios de segurança).
- **Vídeo Pitch (25% — até 4 minutos):**
  - Publicado no YouTube (pode ser *Não Listado*) ou plataforma acessível com link verificado.
  - Roteiro: Problema → Arquitetura → Equipamentos → Endereçamento/Segmentação → Integração dos dispositivos → Demonstração dos testes → Segurança, disponibilidade e expansão.