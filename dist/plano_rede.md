# Plano de Endereçamento IPv4 e Segmentação — SmartRetail Network (Ana Fashion)

> **Fonte Única da Verdade:** Todos os endereços IPv4, máscaras, gateways, VLANs, servidores DNS e nomes de host definidos neste documento devem ser aplicados exatamente desta forma no Cisco Packet Tracer e replicados integralmente em `docs/RELATORIO.md` e `docs/PITCH.md`.

---

## 1. Visão Geral das Sub-redes IPv4

O endereçamento adota o padrão mnemônico `192.168.<VLAN_ID>.0/24` para a rede interna, evitando conflitos com redes domésticas padrão (`192.168.0.0/24` e `192.168.1.0/24`), facilitando auditoria, criação de ACLs e expansão futura com sobra ampla de endereços por segmento. Para a simulação do enlace externo (WAN), utiliza-se a faixa pública de documentação RFC 5737 (`203.0.113.0/30`) e uma rede pública simulada (`8.8.8.0/24`).

| VLAN / Enlace | Nome do Segmento | Sub-rede / Prefixo | Máscara Decimal | Primeiro IP Útil | Último IP Útil | Broadcast | Gateway Padrão | DNS Primário / Secundário | Estratégia de IP |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **VLAN 10** | `VENDAS_PDV` | `192.168.10.0/24` | `255.255.255.0` | `192.168.10.1` | `192.168.10.254` | `192.168.10.255` | `192.168.10.1` | `192.168.20.10` / `8.8.8.8` | Misto (Estático + DHCP) |
| **VLAN 20** | `SERVIDORES` | `192.168.20.0/24` | `255.255.255.0` | `192.168.20.1` | `192.168.20.254` | `192.168.20.255` | `192.168.20.1` | `192.168.20.10` / `8.8.8.8` | Estático |
| **VLAN 30** | `CFTV` | `192.168.30.0/24` | `255.255.255.0` | `192.168.30.1` | `192.168.30.254` | `192.168.30.255` | `192.168.30.1` | `192.168.20.10` *(apenas interno)* | Estático |
| **VLAN 40** | `WLAN_MOVEL` | `192.168.40.0/24` | `255.255.255.0` | `192.168.40.1` | `192.168.40.254` | `192.168.40.255` | `192.168.40.1` | `192.168.20.10` / `8.8.8.8` | DHCP |
| **VLAN 99** | `NATIVA_GERENCIA` | `192.168.99.0/24` | `255.255.255.0` | `192.168.99.1` | `192.168.99.254` | `192.168.99.255` | `192.168.99.1` | — | Estático (Infraestrutura) |
| **WAN (Link)** | `WAN_ISP` | `203.0.113.0/30` | `255.255.255.252` | `203.0.113.1` | `203.0.113.2` | `203.0.113.3` | `203.0.113.1` | — | Estático (Ponto a Ponto) |
| **WAN (Ext.)** | `INTERNET_TEST` | `8.8.8.0/24` | `255.255.255.0` | `8.8.8.1` | `8.8.8.254` | `8.8.8.255` | `8.8.8.1` | `8.8.8.8` | Estático (Simulação ISP) |

---

## 2. Dispositivos e Interfaces com IP Estático

Os equipamentos de infraestrutura, o servidor, a impressora e as câmeras utilizam **IP estático** para garantir previsibilidade de acesso, estabilidade na gravação NVR, facilidade de mapeamento pelo PDV e possibilidade de exceções pontuais futuras em ACLs (por exemplo, liberar acesso futuro de tablets apenas ao IP fixo da impressora).

### 2.1 Infraestrutura de Rede e WAN
| Hostname / Dispositivo | Modelo no PT | Interface | VLAN | Endereço IPv4 | Máscara | Gateway Padrão | Função |
| :--- | :--- | :--- | :---: | :--- | :--- | :--- | :--- |
| **Roteador Gateway** (`RTR-AnaFashion`) | `Router 2911` | `Gi0/0.10` | 10 | `192.168.10.1` | `255.255.255.0` | — | Gateway da VLAN `VENDAS_PDV` |
| **Roteador Gateway** (`RTR-AnaFashion`) | `Router 2911` | `Gi0/0.20` | 20 | `192.168.20.1` | `255.255.255.0` | — | Gateway da VLAN `SERVIDORES` |
| **Roteador Gateway** (`RTR-AnaFashion`) | `Router 2911` | `Gi0/0.30` | 30 | `192.168.30.1` | `255.255.255.0` | — | Gateway da VLAN `CFTV` |
| **Roteador Gateway** (`RTR-AnaFashion`) | `Router 2911` | `Gi0/0.40` | 40 | `192.168.40.1` | `255.255.255.0` | — | Gateway da VLAN `WLAN_MOVEL` |
| **Roteador Gateway** (`RTR-AnaFashion`) | `Router 2911` | `Gi0/0.99` *(native)* | 99 | `192.168.99.1` | `255.255.255.0` | — | Gateway de Gerência / Native VLAN |
| **Roteador Gateway** (`RTR-AnaFashion`) | `Router 2911` | `Gi0/1` *(outside NAT)* | WAN | `203.0.113.2` | `255.255.255.252` | `203.0.113.1` | Saída WAN para Internet (com PAT/NAT) |
| **Switch Principal L2** (`SW-AnaFashion`) | `Switch 2960-24TT` | `Vlan 99` (SVI) | 99 | `192.168.99.2` | `255.255.255.0` | `192.168.99.1` | Interface de gerência L2 do Switch |
| **Access Point Wi-Fi** (`AP-Loja`) | `AccessPoint-PT` | `Port 0 / Port 1` | 40 | *(Bridge L2 — sem IP)* | — | — | Ponte transparente Wi-Fi (`VLAN 40`) |
| **Roteador Operadora ISP** (`ISP-WAN`) | `Router 2911` | `Gi0/0` | WAN | `203.0.113.1` | `255.255.255.252` | — | Roteador da operadora (simulação WAN) |
| **Roteador Operadora ISP** (`ISP-WAN`) | `Router 2911` | `Gi0/1` | Ext. | `8.8.8.1` | `255.255.255.0` | — | Gateway da rede externa simulada |
| **Servidor Externo Internet** (`Internet-Test`) | `Server-PT` | `FastEthernet0` | Ext. | `8.8.8.8` | `255.255.255.0` | `8.8.8.1` | Servidor externo simulado para teste de `ping` |

### 2.2 Dispositivos Finais Estáticos
| Hostname / Dispositivo | Dispositivo no PT | VLAN | Endereço IPv4 | Máscara | Gateway Padrão | DNS | Justificativa da Escolha Estática |
| :--- | :--- | :---: | :--- | :--- | :--- | :--- | :--- |
| **Servidor Local Estoque/NVR** (`SRV-Estoque-NVR`) | `Server-PT` | 20 | `192.168.20.10` | `255.255.255.0` | `192.168.20.1` | `192.168.20.10` | Servidor central de estoque, DNS interno e gravação NVR; exige endereço fixo imutável. |
| **Impressora de Rede** (`Impressora-Rede`) | `Printer-PT` | 10 | `192.168.10.10` | `255.255.255.0` | `192.168.10.1` | `192.168.20.10` | Periférico compartilhado pelos caixas; IP fixo evita falhas de spool e facilita futura regra de exceção em ACL. |
| **Câmera IP 01** (`Cam-IP-01`) | `PC-PT` *(substituto)* | 30 | `192.168.30.11` | `255.255.255.0` | `192.168.30.1` | `192.168.20.10` | Câmera IP 01 — IP fixo permite identificação contínua pelo NVR e auditoria de segurança. |
| **Câmera IP 02** (`Cam-IP-02`) | `PC-PT` *(substituto)* | 30 | `192.168.30.12` | `255.255.255.0` | `192.168.30.1` | `192.168.20.10` | Câmera IP 02 — IP fixo para gravação contínua no NVR. |
| **Câmera IP 03** (`Cam-IP-03`) | `PC-PT` *(substituto)* | 30 | `192.168.30.13` | `255.255.255.0` | `192.168.30.1` | `192.168.20.10` | Câmera IP 03 — IP fixo para gravação contínua no NVR. |
| **Câmera IP 04** (`Cam-IP-04`) | `PC-PT` *(substituto)* | 30 | `192.168.30.14` | `255.255.255.0` | `192.168.30.1` | `192.168.20.10` | Câmera IP 04 — IP fixo para gravação contínua no NVR. |

---

## 3. Faixas de Pool DHCP e Dispositivos Dinâmicos

O serviço DHCP será centralizado no roteador **`RTR-AnaFashion`**, reservando os endereços `.1` a `.99` de cada sub-rede dinâmica para gateways, impressoras e futuras expansões de infraestrutura estática, liberando a faixa `.100` a `.254` para distribuição automática.

### 3.1 Configuração Lógica dos Pools DHCP
| Nome do Pool (`RTR-AnaFashion`) | VLAN | Rede / Máscara | Endereços Excluídos (Reservados) | Faixa Entregue pelo DHCP | Default-Router (Gateway) | DNS Server |
| :--- | :---: | :--- | :--- | :--- | :--- | :--- |
| `POOL_VLAN10_PDV` | 10 | `192.168.10.0 /24` | `192.168.10.1` – `192.168.10.99` | `192.168.10.100` – `192.168.10.254` *(155 IPs)* | `192.168.10.1` | `192.168.20.10`, `8.8.8.8` |
| `POOL_VLAN40_WLAN` | 40 | `192.168.40.0 /24` | `192.168.40.1` – `192.168.40.99` | `192.168.40.100` – `192.168.40.254` *(155 IPs)* | `192.168.40.1` | `192.168.20.10`, `8.8.8.8` |

> **Observação de Segurança:** As VLANs `20 (SERVIDORES)` e `30 (CFTV)` **não possuem pool DHCP** propositalmente, garantindo que apenas dispositivos explicitamente cadastrados operem nessas sub-redes críticas.

### 3.2 Dispositivos Atendidos via DHCP
| Hostname / Dispositivo | Dispositivo no PT | Meio de Conexão | VLAN | Pool DHCP | Faixa Esperada | Justificativa do DHCP |
| :--- | :--- | :--- | :---: | :--- | :--- | :--- |
| **Caixa Registradora 01 (PDV)** (`Caixa-01`) | `PC-PT` | Cabeado (Ethernet) | 10 | `POOL_VLAN10_PDV` | `192.168.10.100–254` | Facilita substituição rápida de terminal PDV em caso de falha sem reconfiguração manual. |
| **Caixa Registradora 02 (PDV)** (`Caixa-02`) | `PC-PT` | Cabeado (Ethernet) | 10 | `POOL_VLAN10_PDV` | `192.168.10.100–254` | Padronização automática de IP, gateway e DNS nos terminais de caixa. |
| **Caixa Registradora 03 (PDV)** (`Caixa-03`) | `PC-PT` | Cabeado (Ethernet) | 10 | `POOL_VLAN10_PDV` | `192.168.10.100–254` | Permite adicionar novos caixas no futuro conectando apenas o cabo de rede. |
| **Tablet Atendimento 01** (`Tablet-01`) | `TabletPC-PT` | Sem Fio (`AP-Loja`) | 40 | `POOL_VLAN40_WLAN` | `192.168.40.100–254` | Dispositivo móvel de atendimento; requer alocação dinâmica no ingresso ao Wi-Fi. |
| **Tablet Atendimento 02** (`Tablet-02`) | `TabletPC-PT` | Sem Fio (`AP-Loja`) | 40 | `POOL_VLAN40_WLAN` | `192.168.40.100–254` | Escalável para novos tablets de vendedores em datas sazonais. |
| **Maquininha de Cartão POS** (`Maquininha-POS`) | `Smartphone-PT` *(substituto)* | Sem Fio (`AP-Loja`) | 40 | `POOL_VLAN40_WLAN` | `192.168.40.100–254` | Terminal POS inteligente Wi-Fi; recebe IP/gateway/DNS automaticamente para comunicar-se com operadoras via WAN. |

---

## 4. Serviços de Rede e Planejamento de Escalabilidade

### 4.1 Serviços Estruturados
- **Gateway / Roteamento Inter-VLAN (`RTR-AnaFashion`):** Roteamento *Router-on-a-Stick* via encapsulamento 802.1Q, traduzindo endereços internos para a WAN via **NAT Overload (PAT)** na interface `Gi0/1`.
- **DHCP (`RTR-AnaFashion`):** Distribuição automática de IPv4, máscara, gateway e DNS para `VLAN 10` e `VLAN 40`.
- **DNS Interno (`SRV-Estoque-NVR` — `192.168.20.10`) + Secundário (`8.8.8.8`):** Serviço DNS habilitado no servidor local para resolução de nomes corporativos internos (ex.: `estoque.anafashion.local` e `nvr.anafashion.local`), evitando dependência de memorização de IPs pelos usuários.

### 4.2 Reserva para Crescimento Futuro (Escalabilidade)
- **Endereços Estáticos Reservados:** Faixa `.2` a `.99` livre em todas as VLANs para novas impressoras, servidores auxiliares, novas câmeras IP (`192.168.30.15+`) ou novos Access Points.
- **Capacidade DHCP:** 155 endereços disponíveis imediatamente nas VLANs 10 e 40.
- **Faixas Reservadas para Novas VLANs:**
  - `VLAN 50` (`192.168.50.0/24`): reservada para eventual rede **Wi-Fi de Clientes / Visitantes** (com novo AP ou WLC).
  - `VLAN 60` (`192.168.60.0/24`): reservada para eventual setor **Administrativo / Escritório**.
- **Flexibilidade de ACL:** Caso seja necessário permitir no futuro que os tablets imprimam diretamente, basta inserir uma única regra *permit* apontando para o IP estático `192.168.10.10` (`Impressora-Rede`) antes do bloqueio entre `VLAN 40` e `VLAN 10`.
