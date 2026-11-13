# **13/11 - Planejamento**
- [ ] Ler o PDF de orientações até o fim e anotar os requisitos
- [ ] Listar os equipamentos: 3 caixas, 2 tablets, 1 impressora, 1 máquina de cartão, 1 servidor de estoque, 4 câmeras IP
- [ ] Definir os equipamentos de rede: roteador/gateway, switch(es), access point
- [ ] Decidir a segmentação (VLANs). Sugestão de grupos:
    - [ ] Vendas/PDV (caixas, máquina de cartão, impressora)
    - [ ] Servidor de estoque
    - [ ] CFTV (câmeras, isoladas do resto)
    - [ ] Wi-Fi dos tablets
- [ ] Montar o plano de endereçamento IPv4 (rede, máscara, gateway, faixa DHCP por VLAN)
- [ ] Definir quais dispositivos terão **IP estático** (servidor, câmeras, impressora, gateways) e quais usarão **DHCP**
- [ ] Definir as regras de segurança (quem acessa o servidor, CFTV isolado, Wi-Fi com WPA2/WPA3)

## **Packet Tracer**
- [ ] Montar a topologia com todos os dispositivos
- [ ] Ligar os cabos (cabeados) e o access point (sem fio)
- [ ] Configurar as VLANs e as portas do switch (access/trunk)
- [ ] Configurar o gateway/roteamento entre VLANs
- [ ] Configurar o DHCP e o DNS
- [ ] Configurar IPs estáticos nos dispositivos definidos
- [ ] Configurar o Wi-Fi dos tablets (SSID + senha)
- [ ] Aplicar a segurança básica (ACLs, isolamento do CFTV)
- [ ] Conectar a saída para a Internet (nuvem/ISP)
- [ ] Salvar o arquivo `.pkt` e fazer uma cópia de backup

> [!tip] Câmeras no Packet Tracer
> Se não houver ícone de câmera IP na sua versão, use um dispositivo que a represente (PC/Server/IoT) e explique isso no relatório.

# 14/11 - Testes + Relatório + Roteiro
**Testes e evidências**
- [ ] **Testes de conectividade** (ping e outros):
    - [ ] Caixas → servidor de estoque
    - [ ] Tablets → servidor de estoque
    - [ ] Câmeras → servidor/gravação
    - [ ] Acesso bloqueado onde deve ser bloqueado (ex.: tablet → CFTV)
    - [ ] Acesso à Internet
- [ ] Tirar **prints**: topologia, configurações, tabela de IPs, resultados dos testes

## **Relatório técnico em PDF (25%)**
- [ ] Diagnóstico das necessidades da Ana Fashion
- [ ] Proposta geral da arquitetura
- [ ] Justificativa da topologia e dos equipamentos
- [ ] Plano de endereçamento IPv4 (tabela)
- [ ] Dispositivos com IP estático vs DHCP
- [ ] Proposta de segmentação
- [ ] Principais serviços de rede (DHCP, DNS, gateway...)
- [ ] Controles básicos de segurança
- [ ] Justificativa de crescimento futuro (VLANs/faixas reservadas, portas livres no switch, novo AP)
- [ ] Responder à pergunta final: *"Como estruturar uma rede de pequeno porte que integre vendas, estoque, pagamentos, móveis e CFTV com segurança, disponibilidade e expansão?"*
- [ ] Inserir o diagrama e os prints
- [ ] Conferir se o relatório bate com a rede do Packet Tracer
- [ ] Exportar em **PDF**

## **Vídeo (preparação)**
- [ ] Escrever o roteiro (problema → arquitetura → equipamentos → endereçamento/segmentação → integração → testes → benefícios)
- [ ] Ensaiar cronometrando (< 4 min)

# 15/11: Vídeo + Revisão + Entrega
- [ ] Gravar o vídeo com a tela do Packet Tracer (até 4 min)
- [ ] Subir no YouTube (pode ser **não listado**)
- [ ] Abrir o link em aba anônima e confirmar que funciona
- [ ] Reler o PDF e conferir os 3 entregáveis
- [ ] Entregar no AVA ✅ (de preferência antes do meio-dia)

> [!warning] Antes de enviar
> O PDF de orientações pede para verificar se o link do vídeo está correto e acessível.

> [!tip] Se o tempo apertar
> Priorize nesta ordem: **Packet Tracer → relatório → vídeo**. Se sobrar tempo no sábado, grave o vídeo já nesse dia para o domingo ser só de entrega.