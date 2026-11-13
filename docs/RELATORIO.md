# Relatório Técnico — SmartRetail Network (Ana Fashion)

**Disciplina:** Redes de Computadores (*Computer Network*) — UniFECAF  
**Cliente:** Loja de Roupas Ana Fashion  
**Escopo Atual do Documento:** Documentação completa da **Fase 1 (Arquitetura, Segmentação, Segurança e Endereçamento IPv4)** — Passos 1, 2 e 3.  
*(As evidências práticas de montagem porta-a-porta, comandos Cisco IOS aplicados e capturas de tela dos testes no Cisco Packet Tracer serão inseridas após a conclusão das Fases 2 e 3 — Passos 4 a 8).*

---

## 1. Diagnóstico das Necessidades da Ana Fashion

A **Ana Fashion** é uma loja de vestuário em processo de expansão que enfrenta gargalos operacionais por não possuir uma infraestrutura de rede corporativa estruturada, segura e escalável. A operação atual depende de equipamentos isolados ou conectados sem separação lógica, o que gera riscos à continuidade das vendas e à segurança das informações.

Os principais desafios identificados no cenário são:

- **Criticidade na frente de caixa e estoque:** Os 3 terminais de caixa (PDV) precisam consultar e atualizar simultaneamente o servidor local de estoque sem quedas de comunicação, latência excessiva ou divergências de saldo.
- **Isolamento do sistema de vigilância (CFTV):** As 4 câmeras IP precisam gravar continuamente no servidor/NVR local, gerando tráfego constante de vídeo que não pode competir com os dados de vendas nem ficar acessível aos dispositivos de atendimento ou à Internet.
- **Atendimento móvel e pagamentos sem fio:** Os 2 tablets utilizados pelos vendedores no salão e a maquininha de cartão inteligente demandam conectividade Wi-Fi estável, segura e integrada ao roteamento corporativo.
- **Compartilhamento de periféricos:** A impressora de rede deve atender de forma previsível às estações de caixa sem falhas de endereçamento.
- **Crescimento sustentável:** A loja planeja adicionar novos pontos de venda, câmeras, dispositivos móveis e até uma futura rede para visitantes sem precisar refazer a topologia física ou reendereçar toda a rede.

---

## 2. Proposta Geral da Arquitetura

Para solucionar os problemas diagnosticados com equilíbrio entre custo, desempenho e facilidade de manutenção para uma empresa de pequeno porte, propõe-se uma arquitetura **hierárquica enxuta de duas camadas (Acesso e Roteamento/Borda)** baseada no modelo **Router-on-a-Stick** com segmentação lógica por **VLANs (IEEE 802.1Q)**.

Os pilares da arquitetura definida na Fase 1 são:

- **Núcleo de Comutação em Camada 2 (`Switch 2960-24TT`):** Centraliza todas as conexões cabeadas (caixas, impressora, servidor, câmeras IP e Access Point) em portas de acesso segregadas por VLAN, comunicando-se com o roteador por meio de um único enlace **Trunk 802.1Q**.
- **Roteamento Inter-VLAN, Segurança e Borda (`Router 2911`):** Atua como gateway padrão de todas as sub-redes por meio de sub-interfaces lógicas, centraliza os servidores DHCP dos segmentos dinâmicos, aplica **Listas de Controle de Acesso (ACLs)** para isolar o tráfego sensível e realiza **NAT Overload (PAT)** na saída para a Internet (WAN).
- **Mobilidade Corporativa (`AccessPoint-PT`):** Opera como ponte transparente de Camada 2 vinculada à VLAN sem fio, garantindo que a política de endereçamento, roteamento e firewall continue centralizada no roteador principal.
- **Servidor Convergente Local (`Server-PT`):** Hospeda simultaneamente o sistema de estoque/PDV, o serviço de gravação de câmeras (NVR) e o servidor DNS interno corporativo.

> **[Espaço reservado — Passo 4 / Passo 8]:** Inserir aqui a captura de tela da topologia lógica completa montada no Cisco Packet Tracer (`dist/prints/01_topologia_geral.png`).

---

## 3. Justificativa da Topologia, Equipamentos e Substitutos no Packet Tracer

### 3.1 Equipamentos de Infraestrutura e WAN
A escolha de cada equipamento equilibra recursos corporativos reais com a compatibilidade total de simulação no Cisco Packet Tracer:

| Tipo de Dispositivo | Hostname Definido | Modelo no Packet Tracer | Justificativa Técnica da Escolha |
| :--- | :--- | :--- | :--- |
| **Roteador / Gateway** | `RTR-AnaFashion` | `Router 2911` | Possui 3 interfaces GigabitEthernet nativas (`Gi0/0`, `Gi0/1`, `Gi0/2`), suportando *Router-on-a-Stick*, pools DHCP, ACLs estendidas e saída WAN sem necessidade de módulos físicos extras. |
| **Switch Principal L2** | `SW-AnaFashion` | `Switch 2960-24TT` | Disponibiliza 24 portas FastEthernet e 2 portas GigabitEthernet. Comporta todos os dispositivos cabeados atuais deixando mais de 50% das portas livres para expansão futura. |
| **Access Point Wi-Fi** | `AP-Loja` | `AccessPoint-PT` | Opera em modo *bridge* Camada 2 (ao contrário de roteadores domésticos como `WRT300N`, que criam NAT secundário indesejado), permitindo que o roteador gerencie os IPs e ACLs dos dispositivos sem fio. |
| **Nuvem WAN** | `WAN-Cloud` | `Cloud-PT` | Representa a camada de acesso à rede pública externa exigida no escopo oficial. |
| **Roteador Operadora ISP** *(apoio à simulação)* | `ISP-WAN` | `Router 2911` | Acoplado à nuvem WAN para simular o roteamento externo da operadora de Internet, pois uma `Cloud-PT` isolada não responde a pacotes ICMP no simulador. |
| **Servidor Externo Internet** *(apoio à simulação)* | `Internet-Test` | `Server-PT` | Simula um destino público externo (`8.8.8.8`) atrás da operadora ISP para validação prática de acesso à Internet via comando `ping`. |

### 3.2 Dispositivos Finais e Contornos de Simulação no Packet Tracer
Todos os 12 dispositivos finais previstos nos requisitos oficiais foram contemplados no inventário, utilizando substitutos tecnicamente justificados quando o simulador não dispõe de ícone nativo completo para testes:

| Tipo de Dispositivo (Operação) | Qtd | Hostname / Rótulo | Dispositivo no PT | Justificativa da Representação / Contorno |
| :--- | :---: | :--- | :--- | :--- |
| **Caixas Registradoras (PDV)** | 3 | `Caixa-01`, `Caixa-02`, `Caixa-03` | `PC-PT` *(nativo)* | Terminais de balcão funcionam como computadores desktop cabeados com sistema PDV; oferecem suporte completo a DHCP e terminal `Command Prompt`. |
| **Tablets de Atendimento** | 2 | `Tablet-01`, `Tablet-02` | `TabletPC-PT` *(nativo)* | Equipamento móvel nativo com interface `Wireless0`, suporte a WPA2-PSK, DHCP e `Command Prompt` para validação de acessos e bloqueios. |
| **Impressora de Rede** | 1 | `Impressora-Rede` | `Printer-PT` *(nativo)* | Possui placa `FastEthernet0`, aceita configuração de IP estático, gateway e DNS, respondendo a testes de conectividade dos caixas. |
| **Servidor Local (Estoque + NVR)** | 1 | `SRV-Estoque-NVR` | `Server-PT` *(nativo)* | Centraliza a aplicação de estoque, o armazenamento de CFTV (NVR) e o serviço DNS interno corporativo. |
| **Maquininha de Cartão (POS)** | 1 | `Maquininha-POS` | `Smartphone-PT` *(substituto)* | O Packet Tracer não possui ícone nativo de terminal POS/TEF. As maquininhas atuais (Smart POS) operam de forma equivalente a smartphones conectados por Wi-Fi; o `Smartphone-PT` reproduz com fidelidade o uso sem fio, recebe DHCP via `AP-Loja` e possui `Command Prompt` para testes. |
| **Câmeras IP (CFTV)** | 4 | `Cam-IP-01` a `Cam-IP-04` | `PC-PT` *(substituto)* | Embora algumas versões possuam o objeto IoT `Webcam`, ele não dispõe de `Command Prompt` para executar comandos `ping` diretamente da câmera até o servidor NVR. O uso do `PC-PT` rotulado como Câmera IP garante configuração IP estática completa e validação bidirecional via CLI (conectividade com NVR e prova de bloqueio vindo dos tablets). |

---

## 4. Plano de Endereçamento IPv4 (Sub-redes, Máscaras e Gateways)

Adotou-se o esquema mnemônico **`192.168.<VLAN_ID>.0/24`** para as sub-redes internas. Essa escolha evita colisões com faixas residenciais comuns (`192.168.0.0/24` e `192.168.1.0/24`), torna o diagnóstico visual imediato (o terceiro octeto identifica diretamente a VLAN) e reserva ampla capacidade de crescimento por segmento. Para o enlace WAN com a operadora, utilizou-se uma sub-rede `/30` da faixa de documentação RFC 5737 (`203.0.113.0/30`), e para a simulação externa adotou-se `8.8.8.0/24`.

| VLAN / Enlace | Segmento | Sub-rede / Prefixo | Máscara Decimal | Primeiro IP Útil | Último IP Útil | Broadcast | Gateway Padrão | DNS Primário / Secundário |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **VLAN 10** | `VENDAS_PDV` | `192.168.10.0/24` | `255.255.255.0` | `192.168.10.1` | `192.168.10.254` | `192.168.10.255` | `192.168.10.1` (`Gi0/0.10`) | `192.168.20.10` / `8.8.8.8` |
| **VLAN 20** | `SERVIDORES` | `192.168.20.0/24` | `255.255.255.0` | `192.168.20.1` | `192.168.20.254` | `192.168.20.255` | `192.168.20.1` (`Gi0/0.20`) | `192.168.20.10` / `8.8.8.8` |
| **VLAN 30** | `CFTV` | `192.168.30.0/24` | `255.255.255.0` | `192.168.30.1` | `192.168.30.254` | `192.168.30.255` | `192.168.30.1` (`Gi0/0.30`) | `192.168.20.10` *(apenas interno)* |
| **VLAN 40** | `WLAN_MOVEL` | `192.168.40.0/24` | `255.255.255.0` | `192.168.40.1` | `192.168.40.254` | `192.168.40.255` | `192.168.40.1` (`Gi0/0.40`) | `192.168.20.10` / `8.8.8.8` |
| **VLAN 99** | `NATIVA_GERENCIA` | `192.168.99.0/24` | `255.255.255.0` | `192.168.99.1` | `192.168.99.254` | `192.168.99.255` | `192.168.99.1` (`Gi0/0.99`) | — |
| **WAN (Link)** | `WAN_ISP` | `203.0.113.0/30` | `255.255.255.252` | `203.0.113.1` | `203.0.113.2` | `203.0.113.3` | `203.0.113.1` (`ISP Gi0/0`) | — |
| **WAN (Ext.)** | `INTERNET_TEST` | `8.8.8.0/24` | `255.255.255.0` | `8.8.8.1` | `8.8.8.254` | `8.8.8.255` | `8.8.8.1` (`ISP Gi0/1`) | `8.8.8.8` |

---

## 5. Definição de Dispositivos com IP Estático vs. DHCP

A atribuição de endereços divide-se estrategicamente entre **IP Estático** (para infraestrutura e serviços que exigem localização imutável) e **DHCP** (para estações operacionais e dispositivos móveis, visando agilidade e escalabilidade).

### 5.1 Dispositivos e Interfaces com IP Estático
| Hostname / Dispositivo | Interface / Tipo | VLAN | Endereço IPv4 | Máscara | Gateway | Justificativa da Alocação Estática |
| :--- | :--- | :---: | :--- | :--- | :--- | :--- |
| **Roteador Gateway** (`RTR-AnaFashion`) | `Gi0/0.10` | 10 | `192.168.10.1` | `255.255.255.0` | — | Gateway padrão da rede de Vendas/PDV. |
| **Roteador Gateway** (`RTR-AnaFashion`) | `Gi0/0.20` | 20 | `192.168.20.1` | `255.255.255.0` | — | Gateway padrão da zona de Servidores. |
| **Roteador Gateway** (`RTR-AnaFashion`) | `Gi0/0.30` | 30 | `192.168.30.1` | `255.255.255.0` | — | Gateway padrão do segmento CFTV. |
| **Roteador Gateway** (`RTR-AnaFashion`) | `Gi0/0.40` | 40 | `192.168.40.1` | `255.255.255.0` | — | Gateway padrão da rede sem fio (WLAN). |
| **Roteador Gateway** (`RTR-AnaFashion`) | `Gi0/0.99` *(native)* | 99 | `192.168.99.1` | `255.255.255.0` | — | Gateway da VLAN nativa e de gerência. |
| **Roteador Gateway** (`RTR-AnaFashion`) | `Gi0/1` *(WAN)* | WAN | `203.0.113.2` | `255.255.255.252` | `203.0.113.1` | Interface pública com tradução PAT/NAT para Internet. |
| **Switch Principal L2** (`SW-AnaFashion`) | `Vlan 99` (SVI) | 99 | `192.168.99.2` | `255.255.255.0` | `192.168.99.1` | Endereço de gerência remota administrativa do switch. |
| **Access Point Wi-Fi** (`AP-Loja`) | `Port 0 / Port 1` | 40 | *(Bridge L2 — sem IP)* | — | — | Opera de forma transparente na Camada 2 no Packet Tracer. |
| **Roteador Operadora ISP** (`ISP-WAN`) | `Gi0/0` / `Gi0/1` | WAN / Ext. | `203.0.113.1` / `8.8.8.1` | `/30` / `/24` | — | Simula roteamento externo da operadora de Internet. |
| **Servidor Externo Internet** (`Internet-Test`) | `FastEthernet0` | Ext. | `8.8.8.8` | `255.255.255.0` | `8.8.8.1` | Destino público simulado para testes de conectividade WAN. |
| **Servidor Local Estoque/NVR** (`SRV-Estoque-NVR`) | `Server-PT` | 20 | `192.168.20.10` | `255.255.255.0` | `192.168.20.1` | Endereço fixo obrigatório para consultas de estoque, DNS e destino das gravações NVR. |
| **Impressora de Rede** (`Impressora-Rede`) | `Printer-PT` | 10 | `192.168.10.10` | `255.255.255.0` | `192.168.10.1` | Evita quebra de fila de impressão nos caixas e permite criar exceção pontual futura em ACL. |
| **Câmera IP 01** (`Cam-IP-01`) | `PC-PT` *(substituto)* | 30 | `192.168.30.11` | `255.255.255.0` | `192.168.30.1` | Identificação fixa contínua pelo servidor NVR e auditoria. |
| **Câmera IP 02** (`Cam-IP-02`) | `PC-PT` *(substituto)* | 30 | `192.168.30.12` | `255.255.255.0` | `192.168.30.1` | Identificação fixa contínua pelo servidor NVR. |
| **Câmera IP 03** (`Cam-IP-03`) | `PC-PT` *(substituto)* | 30 | `192.168.30.13` | `255.255.255.0` | `192.168.30.1` | Identificação fixa contínua pelo servidor NVR. |
| **Câmera IP 04** (`Cam-IP-04`) | `PC-PT` *(substituto)* | 30 | `192.168.30.14` | `255.255.255.0` | `192.168.30.1` | Identificação fixa contínua pelo servidor NVR. |

### 5.2 Dispositivos Dinâmicos e Pools DHCP
Em ambas as VLANs dinâmicas, a faixa **`.1` a `.99`** permanece excluída para proteger gateways, impressoras e futuras expansões de infraestrutura estática, liberando **155 endereços úteis (`.100` a `.254`)** por pool:

| Pool DHCP (`RTR-AnaFashion`) | VLAN | Faixa Excluída | Faixa Distribuída | Dispositivos Atendidos | Justificativa do Uso de DHCP |
| :--- | :---: | :--- | :--- | :--- | :--- |
| `POOL_VLAN10_PDV` | 10 | `192.168.10.1` – `192.168.10.99` | `192.168.10.100` – `192.168.10.254` | **Caixa Registradora 01** (`Caixa-01`), **02** (`Caixa-02`), **03** (`Caixa-03`) | Agiliza a substituição ou ampliação de terminais de caixa sem reconfiguração manual de rede. |
| `POOL_VLAN40_WLAN` | 40 | `192.168.40.1` – `192.168.40.99` | `192.168.40.100` – `192.168.40.254` | **Tablet Atendimento 01** (`Tablet-01`), **02** (`Tablet-02`), **Maquininha de Cartão POS** (`Maquininha-POS`) | Dispositivos móveis ingressam via Wi-Fi obtendo IP, máscara, gateway e DNS automaticamente. |

*(As VLANs `20` e `30` não possuem serviço DHCP intencionalmente, impedindo que equipamentos não autorizados recebam endereço nessas zonas restritas).*

---

## 6. Proposta de Segmentação da Rede (VLANs)

A segmentação lógica em 5 VLANs isola domínios de *broadcast*, melhora o desempenho do PDV e viabiliza políticas granulares de controle de acesso:

1. **VLAN 10 (`VENDAS_PDV`):** Agrupa `Caixa-01`, `Caixa-02`, `Caixa-03` e `Impressora-Rede`. Garante que o tráfego de impressão e vendas não sofra interferência de fluxos de vídeo ou dispositivos sem fio.
2. **VLAN 20 (`SERVIDORES`):** Isola exclusivamente o `SRV-Estoque-NVR` em uma zona controlada de servidores, concentrando a aplicação de políticas para acesso ao banco de estoque e às gravações.
3. **VLAN 30 (`CFTV`):** Agrupa `Cam-IP-01` a `Cam-IP-04`. Atende diretamente ao requisito de isolar logicamente o tráfego contínuo de vídeo do restante da operação comercial.
4. **VLAN 40 (`WLAN_MOVEL`):** Reúne os dispositivos conectados ao `AP-Loja` (`Tablet-01`, `Tablet-02` e `Maquininha-POS`). Como o `AccessPoint-PT` opera como ponte de Camada 2 associada a uma única porta de acesso do switch, manter o Wi-Fi em VLAN dedicada isola o meio sem fio da rede cabeada dos caixas.
5. **VLAN 99 (`NATIVA_GERENCIA`):** Adotada como VLAN nativa do enlace *Trunk* entre switch e roteador (substituindo a VLAN 1 padrão) e como VLAN de estacionamento (*blackhole*) para todas as portas não utilizadas do switch.

---

## 7. Principais Serviços de Rede Utilizados

- **Gateway Padrão e Roteamento Inter-VLAN (`RTR-AnaFashion`):** Por meio de sub-interfaces 802.1Q (`Gi0/0.10` a `Gi0/0.99`), o roteador interconecta exclusivamente os fluxos autorizados entre VLANs e encaminha o tráfego externo pela rota padrão (`0.0.0.0/0` via `203.0.113.1`) com tradução de endereços **NAT Overload (PAT)** na interface `Gi0/1`.
- **Serviço DHCP (`RTR-AnaFashion`):** Entrega automática de configuração IPv4 completa (IP, máscara `/24`, gateway padrão e servidores DNS) para as estações da `VLAN 10` e `VLAN 40`.
- **Serviço DNS (`SRV-Estoque-NVR` + Secundário Externo):** O servidor local (`192.168.20.10`) provê resolução de nomes interna para os serviços corporativos (ex.: `estoque.anafashion.local` e `nvr.anafashion.local`), dispensando o uso de IPs numéricos pelos operadores, combinado ao DNS secundário público `8.8.8.8` nas redes com acesso à Internet.

---

## 8. Controles Básicos de Segurança

### 8.1 Matriz de Comunicação Inter-VLAN (Política de ACLs)
Aplicada nas sub-interfaces do roteador seguindo o **princípio do menor privilégio**:

| Origem | Destino | Ação da Política | Finalidade e Justificativa |
| :--- | :--- | :---: | :--- |
| **VLAN 10 (`VENDAS_PDV`)** | **VLAN 20 (`SERVIDORES`)** | **PERMITIDO** | Operação contínua dos caixas junto ao sistema de estoque. |
| **VLAN 10 (`VENDAS_PDV`)** | **WAN / Internet** | **PERMITIDO** | Emissão de documentos fiscais e serviços externos autorizados. |
| **VLAN 10 (`VENDAS_PDV`)** | **VLAN 30 (`CFTV`)** e **VLAN 40 (`WLAN_MOVEL`)** | **BLOQUEADO (ACL)** | Impede acesso direto dos caixas às câmeras e segmenta o balcão cabeado do Wi-Fi. |
| **VLAN 20 (`SERVIDORES`)** | **VLAN 30 (`CFTV`)** e **WAN / Internet** | **PERMITIDO** | Permite ao NVR gerenciar/gravar as câmeras e realizar atualizações/backups. |
| **VLAN 30 (`CFTV`)** | **VLAN 20 (`SERVIDORES`)** | **PERMITIDO** | Envio exclusivo do fluxo contínuo de vídeo das câmeras ao servidor NVR. |
| **VLAN 30 (`CFTV`)** | **VLAN 10, VLAN 40 e WAN / Internet** | **BLOQUEADO (ACL)** | **Isolamento estrito:** impede acesso externo às câmeras ou movimento lateral a partir delas. |
| **VLAN 40 (`WLAN_MOVEL`)** | **VLAN 20 (`SERVIDORES`)** | **PERMITIDO** | Tablets consultam catálogo/estoque em tempo real no servidor local. |
| **VLAN 40 (`WLAN_MOVEL`)** | **WAN / Internet** | **PERMITIDO** | Essencial para a `Maquininha-POS` autorizar transações de cartão junto às operadoras e para o uso comercial dos tablets. |
| **VLAN 40 (`WLAN_MOVEL`)** | **VLAN 30 (`CFTV`)** | **BLOQUEADO (ACL)** | **Bloqueio crítico de segurança:** impede que dispositivos móveis acessem ou interfiram nas câmeras IP. |
| **VLAN 40 (`WLAN_MOVEL`)** | **VLAN 10 (`VENDAS_PDV`)** | **BLOQUEADO (ACL)** | Mantém os terminais de caixa e a impressora protegidos contra eventuais riscos do meio sem fio *(com flexibilidade planejada para exceção futura apenas à impressora, se necessário)*. |

### 8.2 Hardening Adicional de Camada 2, Wireless e Equipamentos
- **Rede Sem Fio (`AP-Loja`):** Autenticação **WPA2-PSK com criptografia AES** e SSID corporativo dedicado (`AnaFashion-Corp`), impedindo conexões não autorizadas na WLAN operacional.
- **Proteção de Portas e VLAN Nativa (`SW-AnaFashion`):** Todas as portas não utilizadas permanecem administrativamente desligadas (`shutdown`) e alocadas na **VLAN 99 (`NATIVA_GERENCIA`)**; a VLAN nativa do Trunk é alterada da VLAN 1 padrão para a VLAN 99 como mitigação básica contra *VLAN Hopping*.
- **Controle de Acesso Administrativo:** Configuração de senha criptografada de modo privilegiado (`enable secret`) e proteção de acesso por console/VTY no switch e no roteador.

> **[Espaço reservado — Passos 5 a 8]:** Inserir aqui o resumo dos scripts IOS aplicados e os prints dos testes de conectividade (`ping` permitidos, bloqueios via ACL e acesso à Internet) salvos em `dist/prints/`.

---

## 9. Escalabilidade, Crescimento Futuro e Resposta à Questão Central

### 9.1 Capacidade de Expansão Planejada
A arquitetura foi desenhada para absorver o crescimento da Ana Fashion sem reconstrução física ou reendereçamento:

- **Portas Livres no Switch:** O `Switch 2960-24TT` utiliza apenas parte de suas 24 portas FastEthernet e mantém portas GigabitEthernet disponíveis para novos dispositivos cabeadas, uplinks adicionais ou um segundo switch de acesso.
- **Endereços IP Reservados e Folga nos Pools DHCP:** A reserva de `.2` a `.99` em cada VLAN permite adicionar novas impressoras (`192.168.10.11+`), servidores auxiliares (`192.168.20.11+`) ou novas câmeras IP (`192.168.30.15+`), enquanto os pools DHCP oferecem **155 IPs livres** cada para novos caixas, tablets ou maquininhas.
- **Sub-redes Reservadas para Novas VLANs:** Já estão previstas novas faixas seguindo o mesmo padrão mnemônico: **`VLAN 50` (`192.168.50.0/24`)** para eventual Wi-Fi de Clientes/Visitantes (via novo Access Point ou Controladora Wireless) e **`VLAN 60` (`192.168.60.0/24`)** para setor Administrativo/Escritório.
- **Evolução Modular das ACLs:** Como a `Impressora-Rede` possui IP fixo conhecido (`192.168.10.10`), caso a loja decida no futuro permitir que tablets imprimam diretamente, basta adicionar uma única regra *permit* pontual antes do bloqueio entre `VLAN 40` e `VLAN 10`, preservando o isolamento dos 3 caixas.

### 9.2 Resposta à Questão Central do Projeto
> *"Como estruturar uma rede corporativa de pequeno porte capaz de integrar vendas, estoque, pagamentos, dispositivos móveis e CFTV com segurança, disponibilidade e possibilidade de expansão?"*

A estruturação eficiente para uma empresa de pequeno porte como a **Ana Fashion** baseia-se em **separar logicamente aquilo que opera em conjunto fisicamente**:

1. **Integração e Disponibilidade:** Centraliza-se a comutação em um switch gerenciável corporativo (`2960-24TT`) conectado a um roteador de borda capaz (*Router-on-a-Stick* `2911`), permitindo que caixas (`VLAN 10`), tablets (`VLAN 40`) e câmeras (`VLAN 30`) acessem de forma previsível e simultânea o servidor local de estoque/NVR (`VLAN 20`), enquanto a maquininha POS Wi-Fi alcança as operadoras externas com estabilidade via NAT/PAT.
2. **Segurança por Segmentação e Menor Privilégio:** Em vez de manter todos os dispositivos em uma única rede plana, o uso de **VLANs dedicadas**, **ACLs restritivas no roteador** (isolando o CFTV exclusivamente para comunicação com o NVR e bloqueando acessos cruzados entre Wi-Fi e PDV), **WPA2-PSK AES** e **hardening de portas ociosas** garante que falhas ou ameaças em um segmento não comprometam o faturamento nem as gravações de segurança.
3. **Expansão Sem Retrabalho:** O planejamento prévio de portas sobressalentes no switch, faixas estáticas reservadas (`.2`–`.99`), pools DHCP amplos (`.100`–`.254`) e sub-redes reservadas (`VLAN 50` e `VLAN 60`) permite que a loja cresça adicionando novos equipamentos, novos Access Points e novos serviços com apenas ajustes incrementais de configuração, preservando toda a base instalada.
