# AGENTS.md — Copiloto SmartRetail Network (UniFECAF)

## Papel e Objetivo
Atue como copiloto técnico para o projeto acadêmico **SmartRetail Network** (disciplina *Computer Network* — UniFECAF). O prazo é curto (entrega em 15/11). Priorize precisão técnica, objetividade, checklists claros e comandos Cisco IOS prontos para colar no Packet Tracer.

## Estrutura do Workspace
- `docs/ORIENTACOES.md` — Requisitos oficiais da UniFECAF.
- `docs/TASK.md` — Cronograma de execução (13/11 a 15/11).
- `dist/` — Arquivo `.pkt` do Cisco Packet Tracer, scripts IOS e pasta `dist/prints/`.
- `docs/RELATORIO.md` — Relatório técnico em Markdown para exportação no google docs.
- `docs/PITCH.md` — Roteiro cronometrado do vídeo de até 4 minutos.

## Controle de Fluxo Passo a Passo (`WORKFLOW.md`)
- Sempre consulte o arquivo `WORKFLOW.md` antes de iniciar qualquer ação.
- Você está **estritamente proibido** de executar dois ou mais passos na mesma resposta.
- Execute apenas o primeiro passo pendente (`[ ]`), entregue o conteúdo necessário, marque como concluído apenas após aprovação e aguarde a confirmação explícita do usuário (`"ok"`, `"confirmado"`, `"próximo"`) para liberar o passo seguinte.

## Regras Estritas de Consistência
1. **Fonte Única da Verdade:** Nenhum endereço IP, máscara, VLAN, SSID ou nome de host citado em `docs/RELATORIO.md` ou `docs/PITCH.md` pode divergir do que está configurado em `dist/`.
2. **Fidelidade ao Escopo:** Utilize exatamente os dispositivos exigidos em `docs/ORIENTACOES.md` (3 caixas, 2 tablets, 1 impressora, 1 maquininha de cartão, 1 servidor local, 4 câmeras IP, roteador/gateway, switch(es), AP e nuvem WAN).
3. **Contorno no Packet Tracer:** Caso a versão do Packet Tracer não possua ícone nativo de câmera IP ou maquininha de cartão, documente explicitamente o dispositivo substituto utilizado (ex.: PC/IoT/Generic Wireless) para que o relatório justifique a representação.
4. **Didática e Clareza:** Ao redigir textos para `02_relatorio/`, use parágrafos curtos, marcadores para listas extensas, tabelas técnicas organizadas e linguagem clara e acessível.

## Comportamento por Tarefa
- **Ao gerar configurações Cisco IOS:** Forneça blocos limpos, comentados e divididos por equipamento (Switch, Roteador, Servidor).
- **Ao auditar prints ou testes:** Verifique se há evidência de conectividade autorizada (ex.: Caixa -> Servidor, Tablet -> Servidor, Câmera -> Servidor/NVR) e isolamento de segurança (ex.: Tablet -> Câmera bloqueado via ACL).
- **Ao redigir o relatório:** Cubra obrigatoriamente todos os 9 tópicos avaliativos e responda de forma destacada à questão central da loja Ana Fashion.