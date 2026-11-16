# SmartRetail Network — Ana Fashion (UniFECAF)

Projeto acadêmico da disciplina **Computer Network (Redes de Computadores)** da **UniFECAF**, com foco no planejamento, implementação, validação e documentação de uma rede corporativa de pequeno porte para a loja de roupas **Ana Fashion**.

O projeto responde à questão central:
> *"Como estruturar uma rede corporativa de pequeno porte capaz de integrar vendas, estoque, pagamentos, dispositivos móveis e CFTV com segurança, disponibilidade e possibilidade de expansão?"*

## Artefatos

Video Pitch: https://youtu.be/LaLK1n3kerc

PDF: [PDF](https://github.com/joaopedro-chaves/Projeto-SmartRetail-Network-UniFECAF/blob/main/dist/SmartRetail%20Network%20(Ana%20Fashion)%20-%20Parte%20Te%C3%B3rica.pdf)

Arquivo Packet Tracer: [Packet Tracer](https://github.com/joaopedro-chaves/Projeto-SmartRetail-Network-UniFECAF/blob/main/dist/main.pkt)

## Visão Geral da Arquitetura

A solução adota topologia física em **Estrela** combinada à arquitetura lógica **Router-on-a-Stick (IEEE 802.1Q)**, garantindo convergência física com isolamento lógico entre setores:

- **Roteador / Gateway (`RTR-AnaFashion` — `Router 2911`):** Roteamento Inter-VLAN por sub-interfaces (`Gi0/0.10`, `.20`, `.30`, `.40` e `.99`), serviço DHCP centralizado, filtragem por Listas de Controle de Acesso Estendidas (ACLs) e **NAT Overload (PAT)** na saída WAN (`Gi0/1` — `203.0.113.2/30`).
- **Switch Principal L2 (`SW-AnaFashion` — `Switch 2960-24TT`):** Portas *Access* segmentadas por VLAN, enlace **Trunk 802.1Q** (`Gi0/1`) com Native VLAN `99`, SVI de gerência (`192.168.99.2`) e 15 portas ociosas desativadas (`shutdown`) na VLAN `99`.
- **Access Point Wi-Fi (`AP-Loja` — `AccessPoint-PT`):** Ponte Camada 2 na VLAN `40`, com SSID `AnaFashion-Corp`, autenticação **WPA2-PSK** e criptografia **AES**.
- **Servidor Convergente (`SRV-Estoque-NVR` — `Server-PT`):** Hospeda o sistema de estoque/vendas, gravação CFTV (NVR) e DNS interno (`estoque.anafashion.local` e `nvr.anafashion.local`) no IP estático `192.168.20.10`.
- **Simulação WAN / Internet:** Cadeia `Modem-WAN` (`Cable-Modem-PT`) -> `Cloud-PT` -> `ISP-WAN` (`Router 2911`) -> `Internet-Test` (`Server-PT`, `8.8.8.8`).

### Dispositivos Finais (12 equipamentos)

| Grupo | Quantidade | Hostnames | Modelo no Packet Tracer | Alocação de IP |
| :--- | :---: | :--- | :--- | :--- |
| Caixas Registradoras (PDV) | 3 | `Caixa-01` a `Caixa-03` | `PC-PT` | DHCP (`VLAN 10`) |
| Impressora de Rede | 1 | `Impressora-Rede` | `Printer-PT` | Estático `192.168.10.10` (`VLAN 10`) |
| Servidor Estoque / NVR / DNS | 1 | `SRV-Estoque-NVR` | `Server-PT` | Estático `192.168.20.10` (`VLAN 20`) |
| Câmeras IP (CFTV) | 4 | `CAM-IP-01` a `CAM-IP-04` | `PC-PT` *(substituto para testes CLI)* | Estático `192.168.30.11` a `.14` (`VLAN 30`) |
| Tablets de Atendimento | 2 | `Tablet-01`, `Tablet-02` | `TabletPC-PT` | DHCP via Wi-Fi (`VLAN 40`) |
| Maquininha de Cartão (POS) | 1 | `Maquininha-POS` | `Smartphone-PT` *(substituto Wi-Fi)* | DHCP via Wi-Fi (`VLAN 40`) |

## Segmentação Lógica (VLANs) e Endereçamento

| VLAN | Nome | Sub-rede | Gateway | Política Principal |
| :---: | :--- | :--- | :--- | :--- |
| **10** | `VENDAS_PDV` | `192.168.10.0/24` | `192.168.10.1` | Acessa `VLAN 20` e Internet; bloqueada para `VLAN 30`, `40` e `99` (`ACL_VLAN10_IN`). |
| **20** | `SERVIDORES` | `192.168.20.0/24` | `192.168.20.1` | Sem DHCP; recebe conexões de PDV, WLAN e gravações do CFTV. |
| **30** | `CFTV` | `192.168.30.0/24` | `192.168.30.1` | Sem DHCP; isolamento estrito apenas para `VLAN 20` e ICMP ao gateway (`ACL_VLAN30_IN`). |
| **40** | `WLAN_MOVEL` | `192.168.40.0/24` | `192.168.40.1` | Acessa `VLAN 20` e Internet; bloqueada para `VLAN 30`, `10` e `99` (`ACL_VLAN40_IN`). |
| **99** | `NATIVA_GERENCIA` | `192.168.99.0/24` | `192.168.99.1` | VLAN nativa do Trunk 802.1Q, gerência do switch (`192.168.99.2`) e portas ociosas em `shutdown`. |

O planejamento prevê ainda as sub-redes **`VLAN 50` (`192.168.50.0/24`)** e **`VLAN 60` (`192.168.60.0/24`)** para futura rede de visitantes e setor administrativo, além de 15 portas livres no switch e 155 endereços disponíveis por pool DHCP (`.100` a `.254`).

## Estrutura do Repositório

- `dist/`
  - `main.pkt` — Topologia completa implementada no Cisco Packet Tracer.
  - `ios_switch.txt`, `ios_router.txt`, `ios_isp.txt`, `ios_acls.txt` — Scripts Cisco IOS aplicados nos equipamentos.
  - `prints/` — Evidências capturadas no Cisco Packet Tracer (topologia, VLANs/Trunk, DHCP/NAT, ACLs, DNS, Wi-Fi e testes de conectividade/bloqueio).
- `docs/`
  - `ORIENTACOES.md` — Requisitos oficiais e critérios de avaliação da UniFECAF.
  - `TASK.md` — Lista de controle das entregas e prazos.
  - `RELATORIO.md` — Relatório técnico completo cobrindo os 9 tópicos obrigatórios e a questão central.


## Política de Uso de Inteligência Artificial

Este projeto adota uso transparente e responsável de ferramentas de Inteligência Artificial como apoio ao desenvolvimento acadêmico. O **único e exclusivo motivo** para a utilização de IA neste trabalho foi o **curto período de tempo disponível para execução e entrega, em razão de motivos pessoais**.

1. **Papel da IA (Copiloto Técnico):** Diante da restrição de tempo, a IA foi utilizada exclusivamente como assistente de produtividade e engenharia de redes para auxiliar no planejamento do endereçamento IPv4, revisar a segmentação em VLANs, gerar blocos de configuração Cisco IOS comentados, auxiliar na auditoria das capturas de tela de testes e apoiar a redação técnica da documentação como rascunho.
2. **Controle por Etapas e Execução Humana:** O trabalho seguiu o protocolo unitário definido em `AGENTS.md` e `docs/WORKFLOW.md`, no qual nenhuma etapa avançou sem revisão e confirmação explícita do autor. Toda a montagem física, aplicação dos scripts no Cisco Packet Tracer (`dist/main.pkt`), execução dos comandos de teste e captura das evidências em `dist/prints/` foram realizadas e verificadas manualmente no simulador.
3. **Rastreabilidade e Fonte Única da Verdade:** Todas as decisões técnicas, correções de topologia e verificações de consistência estão registradas em `docs/HISTORICO_CONVERSA.md`, garantindo correspondência exata entre os scripts em `dist/`, o arquivo `.pkt` e os documentos em `docs/`.
4. **Responsabilidade Acadêmica:** A validação técnica final, a exportação do relatório em PDF, a gravação do vídeo pitch e a entrega oficial no ambiente virtual da disciplina são de inteira responsabilidade do autor do trabalho.
