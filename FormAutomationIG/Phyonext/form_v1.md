:date: 31/10/2025
:alien: AI Claude

Agora com seu novo perfil de especialista backend php. Crie para o formulário que você elaborou um arquivo php que receberá as requisições ajax e insert em um banco de dados supabase externo.
Crie as variáveis de config para configurar as credenciais para acesso a supabase.
O arquivo php tem que receber as requisições dos campos (nome, whatsapp, email, site e faturamento - requisições após o campo ser preenchido) e inserir na tabela lead_unqualified (todos os campos em uma linha - row) e o campo de controle sempre será o controleU.
Na requisição a partir do botão submit deve inserir os dados na tabela lead_qualified e o campo de controle sempre será o controleU.

:date: 29/10/2025
:alien: AI Claude

Crie um formulário em HTML e bootstrap moderno que remete a tecnologia com tons de azul (analise o tom azul desse site:https://phyonex.com) e com temas de cores que remete a IA. Mescle os tons de cores mas foque para representar o site em referência e que transmita tecnologia com automação IA.
Formulário responsivo - sua maior interação será através de dispositivos móveis (celulares - precisa ser otimizado para esse fim).
Campos do formulário:
    - Nome (obrigatório);
    - WhatsApp (obrigatório com máscara e validação para número de celulares no padrão Brasil);
    - Email (obrigatório com validação de email);
    - Radio button com a label "Faturamento anual" com as opções:
        - Até R$100k/ano
        - R$100K a R$500k/ano
        - R$500K a R$1M/ano
        - R$1M a R$5M/ano
        - +R$10M/ano
Utilize jquery para fazer a máscara para o número de celular e validações necessárias.
Crie um alerta elegante e eficiente para informar sobre as validações.
Uma string de controle será informada na url do formulário para ser capturada via get pelo código ajax que enviará as informações (url_do_fom?u=)
AJAX com jquery para enviar os dados assim que cada campo for preenchido relacionado com a string de controle capturada via get (u) da url.
Mesmo com os dados sendo enviado assim que o campo é preenchido e validado, crie o botão de enviar e que depois de clicar esconde o formulário, reset os campos preenchidos e informe um mensagem de agradecimento.
O objetivo e ter um formulário que seja fácil, intuitivo, responsivo, amigável e que atraia as pessoas a preencherem. 