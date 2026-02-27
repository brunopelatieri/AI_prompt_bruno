# Arquitetura de Workflow XML para LLM (GPT-5-mini)

## 📌 Objetivo

Este documento resume as melhores práticas para estruturar fluxos de
decisão em XML dentro de prompts para LLMs, com foco em:

-   Determinismo
-   Clareza estrutural
-   Escalabilidade
-   Compatibilidade com tool calling

------------------------------------------------------------------------

# 1️⃣ Problema Inicial

Modelo simples usando decisão dentro de `<thought_process>`:

``` xml
<thought_process>
  <evaluation>Avaliar se o lead é clínica.</evaluation>
  <decision>
    - Se clínica -> Avançar.
    - Se não clínica -> Acionar suporte.
  </decision>
</thought_process>
```

### Limitações:

-   Decisão implícita
-   Dependente de interpretação semântica
-   Não auditável
-   Pode variar entre execuções

------------------------------------------------------------------------

# 2️⃣ Evolução: Estado Explícito

Separar:

1.  Classificação
2.  Estado
3.  Decisão

Exemplo estruturado:

``` xml
<step id="2" action="lead_filter_clinic_type">

  <input>
    Resposta do lead sobre tipo de negócio
  </input>

  <state>
    <lead_type>clinic | non_clinic | unclear</lead_type>
  </state>

  <classification>
    Classifique a resposta do lead em:
    - clinic
    - non_clinic
    - unclear
  </classification>

  <decision>
    IF lead_type == "clinic" THEN go_to_step="3"
    ELSE IF lead_type == "unclear" THEN ask_clarification="true"
    ELSE invoke_tool="attendantSupport"
  </decision>

</step>
```

------------------------------------------------------------------------

# 3️⃣ Booleanos vs Enum

## ❌ Múltiplos Booleanos

``` xml
<state>
  <lead_is_clinic>true|false</lead_is_clinic>
  <lead_type_unclear>true|false</lead_type_unclear>
</state>
```

Problema: podem gerar estados conflitantes.

## ✅ Enum (Recomendado)

``` xml
<state>
  <lead_type>clinic | non_clinic | unclear</lead_type>
</state>
```

Vantagens:

-   Apenas um valor possível
-   Sem conflito lógico
-   Mais escalável

------------------------------------------------------------------------

# 4️⃣ Local Correto do `<state>`

Colocar dentro do `<step>`, logo após `<input>`.

Fluxo ideal:

1.  Recebe input
2.  Classifica
3.  Atualiza estado
4.  Decide
5.  Executa ação

------------------------------------------------------------------------

# 5️⃣ Arquitetura Global (Opcional)

Para sistemas com backend persistente:

``` xml
<workflow_state>
  <lead_type></lead_type>
  <current_step></current_step>
</workflow_state>
```

Indicado quando:

-   Existe memória externa
-   Sistema multi-step longo
-   Orquestração fora do LLM

------------------------------------------------------------------------

# 6️⃣ Padrão Produção Recomendado

✔ Estado explícito\
✔ Decisão baseada apenas em estado\
✔ Enum em vez de múltiplos booleanos\
✔ Separação clara entre classificação e decisão

------------------------------------------------------------------------

# 🎯 Princípio de Pareto Aplicado

20% que gera 80% da robustez:

-   Substituir interpretação textual por variável explícita
-   Usar enum para classificação
-   Decisão depender apenas do estado

------------------------------------------------------------------------

# 🧠 Analogia Final

Decisão implícita = juiz improvisando\
Estado explícito + regra = juiz seguindo código penal escrito

------------------------------------------------------------------------

# 📌 Conclusão

Para protótipo simples → decisão dentro de `<thought_process>`
funciona.\
Para sistema profissional com tool calling → estado explícito + decisão
estruturada é superior.

------------------------------------------------------------------------
