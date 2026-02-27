# 👽 : 26/02/2026

Você é um especialista em n8n e com as melhores práticas vamos criar um workflow as tool para agentAI com o google calendar.
Crie utilizando as práticas mais avançada para um sistema de agendamento com CRUD completo utilizando o Google Calendar. 
Roteiro sugerido:
1. O Workflow da Ferramenta (A Tool)
Crie um workflow separado que começa com o nó n8n Form Tool ou Execute Workflow Trigger.

Input: Defina variáveis como summary, start_time e action (create, read, delete).

Processo: Use nós IF/Switch para decidir se vai Criar, Ler ou Deletar no Google Calendar com base no input da IA.

Output: Retorne uma confirmação simples para a IA.

2. O Workflow do Agente (O Cérebro)
No seu workflow principal (o do Agente AI):

Adicione o nó AI Agent.

Conecte o nó Workflow Tool na entrada de ferramentas.

Nas configurações da Tool, descreva para a IA: "Use esta ferramenta para gerenciar o Google Calendar. Você pode criar, ler e excluir eventos."

3. Quero usar Supabase para criar um passo extra no workflow para que, toda vez que o agente criar um evento no Google Calendar, ele também salve o event_id no seu banco de dados. Isso facilita muito se você precisar que o app mostre o status do agendamento para o cliente final depois.

Esse é apenas um contexto sugerido. Você é livre para usar usa expertise.

Mas atenção! Obrigatório:
 - Coloque os blocos de comentário no fluxo detalhando todo o funcionamento do fluxo e pontos chaves de configuração se necessário.
 - As credenciais do google calendar já estão cadastradas no n8n e integradas com sucesso. Credenciais ok.
 - A versão do n8n que estamos trabalhando é a versão mais atualizada n8n Version 2.8.3