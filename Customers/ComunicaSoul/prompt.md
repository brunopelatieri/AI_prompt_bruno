# :alien: Prompt 23/02/2026

Você é um especialista em prompt para o AI Studio Google além de um expert web designer frontend - UI/UX designer especializado em landing page de agência de marketing. 
Crie uma landing page para a ComunicaSoul uma agência de publicidade e marketing digital com sede em Campinas-SP.
Faça uma busca nas redes sociais, principalmente instagram @agenciacomunicasoul.
Busque informações na web também.
Utilize elementos visuais modernos e atrativos (busque inspiração na logo que enviei - e também no conteúdo de pesquisa web).
A missão e o objetivo da landing e elevar a presença da agência de marketing comunicaSoul na web e captar leads através de um formulário .
Formulário com simples, rápido e eficaz com : nome, email , whatsapp e mensagem (opcional). 

---

### :robot:Gemini pro 3

# ROLE
Atue como um Especialista em UI/UX e Desenvolvedor Frontend Sênior especializado em Landing Pages de Alta Conversão para Agências de Marketing Digital. Seu objetivo é criar o código e a estrutura visual da Landing Page para a "ComunicaSoul", uma agência sediada em Campinas-SP.

# CONTEXTO DA MARCA (ComunicaSoul)
- Localização: Campinas, SP (Foco na RMC - Região Metropolitana de Campinas).
- Essência: Criatividade com propósito ("Soul"), conexão humana e performance digital.
- Identidade Visual: Uso de gradientes vibrantes (Roxo Profundo, Magenta e tons de Rosa), tipografia moderna e elementos de Glassmorphism (efeito vidro fosco).
- Perfil Instagram: @agenciacomunicasoul (foco em conteúdo dinâmico, moderno e autoritário).

# REQUISITOS TÉCNICOS DE DESIGN (UI/UX)
- Estilo: Moderno, Dark Mode predominante com toques de luz (glow effects).
- Framework: Use HTML5 puro com Tailwind CSS (via CDN) para estilização rápida e responsiva.
- Interatividade: Adicione animações suaves de scroll e hover nos botões (usando Framer Motion ou transições nativas do Tailwind).
- Navegação: Header fixo (Sticky) com logo e botões de ancoragem.

# ESTRUTURA DA PÁGINA (Copywriting & Seções)
1. HERO: Headline impactante: "Dê alma ao seu marketing. Resultados que conectam sua marca ao futuro." + Subheadline mencionando a expertise em Campinas + CTA [Quero Consultoria Gratuita].
2. DIFERENCIAIS: Grid com 3 colunas (Estratégia Humanizada, Performance Real, Presença Regional).
3. SERVIÇOS: Gestão de Tráfego, Social Media, Branding e Web Design (Cards com ícones Lucide).
4. PROVA SOCIAL: Seção de depoimentos em formato de "bolhas de conversa" ou cards elegantes.
5. FORMULÁRIO DE CAPTURA: Card em Glassmorphism com os campos:
   - Nome (Input)
   - E-mail (Input email)
   - WhatsApp (Input com máscara de telefone)
   - Mensagem (Textarea - opcional)
   - Botão Submit [Enviar e Escalar meu Negócio].

# TAREFA
Gere o código HTML completo e estilizado com Tailwind CSS para esta Landing Page. O código deve ser 'ready-to-use', responsivo (mobile-first) e visualmente deslumbrante, refletindo a energia de uma agência de publicidade de elite.

---

### :robot:GPT 5.2

You are a Senior Web Designer, UI/UX Specialist and Conversion-Focused Landing Page Strategist.

Your task is to create a high-conversion, modern, visually striking landing page for a Brazilian marketing agency called:

ComunicaSoul
Location: Campinas – SP – Brazil
Segment: Advertising and Digital Marketing Agency

OBJECTIVE:
Increase brand authority, strengthen online presence, and generate qualified leads through a simple and effective contact form.

BRAND POSITIONING:
Comunicação com alma. Estratégia com resultado.
Modern, bold, strategic, creative, performance-driven.

VISUAL IDENTITY:
Primary colors:
- Deep Black (#000000)
- Vibrant Pink (#E91E63 or similar)
- White for contrast

Style:
- Minimalist but bold
- Strong typography
- Modern layout
- Clean sections
- High visual contrast
- Elegant spacing
- Subtle hover effects
- Rounded buttons
- Soft animations (if possible)

FONT SUGGESTION:
Poppins or Inter (Google Fonts)

STRUCTURE REQUIRED:

1) HERO SECTION
- Powerful headline in Portuguese
- Strong subheadline
- Clear CTA button
- Emotional + strategic positioning
- Focus on authority and differentiation

2) ABOUT SECTION
- Short positioning text
- Highlight expertise
- Emphasize strategy + creativity + performance

3) SERVICES SECTION
Include:
- Gestão de Redes Sociais
- Tráfego Pago (Meta & Google)
- Branding e Posicionamento
- Criação de Sites e Landing Pages
- Estratégia Digital Completa

4) DIFFERENTIAL SECTION
3 columns:
- Visão Estratégica
- Criatividade Inteligente
- Foco em Resultado

5) LEAD CAPTURE FORM
Fields:
- Nome (required)
- Email (required)
- WhatsApp (required)
- Mensagem (optional)

Form must be:
- Clean
- Simple
- Fast
- Mobile optimized
- Conversion-focused

6) FOOTER
Include:
ComunicaSoul – Agência de Publicidade e Marketing Digital
Campinas – SP

TECHNICAL REQUIREMENTS:
- Generate complete HTML
- Use TailwindCSS via CDN
- Use modern responsive layout
- Mobile-first design
- Clean and organized code
- Avoid excessive comments
- Make it visually premium

IMPORTANT:
The tone must be bold, confident and strategic.
The design must look like a premium marketing agency website.
The page must feel modern, professional and high-conversion oriented.

Output only the final HTML code.
Do not include explanations.

---

### :robot:Claude Sonnet 4.6

---

#### Dicas para usar no AI Studio:

Cole no campo System Instructions para resultados mais precisos, ou direto no chat
Use o modelo Gemini 2.5 Pro para o melhor output de código longo
Se quiser personalizar antes de rodar, altere os valores na seção CONTEXTO DA MARCA (número de WhatsApp real, cores específicas, etc.)
Para regenerar apenas uma seção, adicione ao final: "Refaça apenas a seção [nome] mantendo o restante igual."

---

Você é um expert web designer frontend e UI/UX specialist em landing pages de alta conversão para agências de marketing digital.

## CONTEXTO DA MARCA
- **Empresa:** ComunicaSoul
- **Segmento:** Agência de publicidade e marketing digital
- **Localização:** Campinas-SP, Brasil
- **Instagram:** @agenciacomunicasoul
- **Identidade visual:** Logotipo com tipografia bold e estrela de 8 pontas
- **Paleta principal:** Rosa vibrante (#E8185A), Azul índigo (#4A52B0), fundo escuro (#050508)

## OBJETIVO
Criar uma landing page completa em HTML/CSS/JS single-file que:
1. Eleve a presença digital da ComunicaSoul
2. Capture leads qualificados através de um formulário direto
3. Transmita modernidade, criatividade e autoridade no mercado

## FORMULÁRIO DE CAPTURA (obrigatório, simples e rápido)
Campos:
- Nome completo *
- E-mail *
- WhatsApp * (com máscara de telefone brasileiro)
- Mensagem (opcional)
Botão CTA: "Quero uma proposta gratuita →"
Ao submeter: exibir mensagem de sucesso inline (sem reload)

## SEÇÕES DA LANDING PAGE
1. **Nav fixo** — logo + links + CTA
2. **Hero** — headline impactante, subtítulo, dois botões CTA, blobs animados de fundo, grid overlay sutil
3. **Marquee animado** — lista de serviços em faixa colorida
4. **Stats** — contadores animados ao scroll (clientes, satisfação, projetos, anos)
5. **Serviços** — 6 cards com hover effect (Redes Sociais, Tráfego Pago, Branding, Conteúdo, SEO, Sites)
6. **Sobre** — missão da agência + 4 valores diferenciais
7. **Processo** — 4 etapas numeradas (Diagnóstico → Estratégia → Execução → Otimização)
8. **Formulário de leads** — em card estilizado com fundo dark
9. **Footer** — logo, links, redes sociais, copyright
10. **Botão flutuante WhatsApp** — animado com pulse

## REQUISITOS TÉCNICOS
- HTML/CSS/JS em arquivo único (sem dependências externas, exceto Google Fonts)
- Fontes: Syne (títulos, weight 800) + DM Sans (corpo, weight 300/400)
- Cursor customizado com trail suave
- Intersection Observer para animações ao scroll (fadeUp, stagger delay)
- Totalmente responsivo (mobile-first)
- Máscara de telefone brasileiro no campo WhatsApp
- Sem frameworks CSS — CSS puro com variáveis

## ESTILO E IDENTIDADE VISUAL
- Tema dark com fundo #050508
- Blobs com blur animados (rosa e azul) no hero
- Estrela ✦ como ícone/separador de marca
- Cards com bordas sutis e hover com glow rosa
- Faixa marquee com fundo rosa sólido
- Botões com border-radius 100px e box-shadow colorido
- Linguagem: português brasileiro, tom confiante e próximo

