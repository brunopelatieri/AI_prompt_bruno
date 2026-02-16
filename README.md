# 🏗️ |Padronização :: Arquitetura do Prompt DiretorIA

Utilizamos uma hierarquia XML para separar as instruções de sistema, as regras de negócio brasileiras e os mecanismos de tratamento de erro.

## 📋 Tags e Lógica de Processamento

| Tag | Função no Contexto Gastronômico |
| :--- | :--- |
| `<prompt>` | Tag raiz que encapsula todo o conteúdo. | Obrigatório (inclui atributo `version`). |
| `<system_instructions>` | Define o agente como um sistema especializado em culinária brasileira. |
| `<output_format>` | Garante que a saída seja estritamente um JSON válido para o n8n/Supabase. |
| `<few_shot_examples>` | Exemplos práticos de lousas de giz, PDFs e cadernos manuscritos. |
| `<error_handling>` | Define condutas para textos ilegíveis ou preços ausentes. |
| `<execution_parameters>` | Parâmetros técnicos (temperature, top_p).	Documentação interna para o nó do n8n. |
| `<thought_process>` | Raciocínio Interno da IA **(Ver Exemplo)** |
| `<workflow>` | Fluxo Executável de Ações **(Ver Exemplo)** |
| `<brazilian_context_rules>` | Regras específicas para o mercado brasileiro.	Moeda (R$), ABNT, fuso horário e regionalismos. |
| `<normalization>` | Uma etapa do workflow onde a IA aplica as regras de normalização. Executar a transformação dos dados usando os patterns definidos. |
| `<normalization_patterns>` | Um dicionário de regras que define como transformar dados brutos em formato padronizado. Regras de limpeza de dados.	Regex para CPFs, limpeza de strings e trim. |

---

## 🛠️ EXEMPLO :: Prompt Estruturado (Versão 1.0.0)

Este código deve ser utilizado como o `System Prompt` nas suas automações.

```xml
<prompt version="1.0.0">
  <system_instructions lang="en">
    <role>Specialized Brazilian culinary data extractor. Output ONLY STRICTLY VALID JSON ARRAY [].</role>
    <critical_requirements>- MANDATORY: produto, descricao, categoria, preco.</critical_requirements>
    <constraints>- JSON ONLY. No prose. Root element: [].</constraints>
    <output_format>
      <json_schema>
        [{"produto": "str","descricao": "str","categoria": "str","preco": num,"porcao": "str|null","unidade_medida": "str|null","ingredientes_identificados": ["str"],"observacoes": "str|null"}]
      </json_schema>
      <error_schema>
        [{"error": "insufficient_data","message": "str","missing_fields": ["str"]}]
      </error_schema>
      <categories>Entradas, Pratos Principais, Prato Feito, Porções, Marmitas, Bebidas, Sobremesas, Cafés, Lanches, Saladas.</categories>
    </output_format>
  </system_instructions>

  <brazilian_context lang="pt-BR">
    <normalization>- Preço: "R$ 2,50"| "2,5" -> 2.50.</normalization>
  </brazilian_context>

  <few_shot_examples>
    <example id="1">
      <i t="image">PF Frango à Milanesa c/ Arroz e Feijão R$ 28,50</i>
      <o>[{"produto":"Frango à Milanesa","descricao":"Frango empanado e frito com arroz e feijão","categoria":"Prato Feito","preco":28.50,"porcao":"individual","unidade_medida":"porção","ingredientes_identificados":["frango","arroz","feijão"],"observacoes":null}]</o>
    </example>
    <example id="2">
      <i t="blur">Suco de... R$...</i>
      <o>[{"error":"insufficient_data","message":"Dados obrigatórios ausentes.","missing_fields":["produto","preco"]}]</o>
    </example>
  </few_shot_examples>

  <execution_parameters>
    <temperature>0.0</temperature>
    <model>gemini-2.5-flash</model>
    <max_output_tokens>1024</max_output_tokens>
  </execution_parameters>
</prompt>

```

## 🧠 Guia de Tags: `<thought_process>` vs `<workflow>`

---

## 📑 Índice

- [Visão Geral](#visão-geral)
- [thought_process - Raciocínio Interno](#1️⃣-thought_process---raciocínio-interno-da-ia)
- [workflow - Fluxo Executável](#2️⃣-workflow---fluxo-executável-de-ações)
- [Comparação Direta](#-comparação-direta)
- [Quando Usar Cada Um](#-quando-usar-cada-um)
- [Versão Híbrida](#-versão-híbrida-recomendada)

---

## Visão Geral

Estas tags estruturam a lógica de processamento em prompts para LLMs, cada uma com propósitos distintos:

| Tag | Propósito | Visibilidade | Uso Principal |
|-----|-----------|--------------|---------------|
| `<thought_process>` | Raciocínio interno | Implícito | Validação e análise |
| `<workflow>` | Processo executável | Rastreável | Automação multi-etapas |

---

## 1️⃣ `<thought_process>` - Raciocínio Interno da IA

### 📌 Definição

Tag que guia o **raciocínio passo-a-passo** da IA antes de gerar a resposta final. O processo ocorre internamente e não aparece na saída.

### 🎯 Quando Usar

- ✅ Problemas que exigem análise complexa
- ✅ Validação de qualidade de dados
- ✅ Tomada de decisões condicionais
- ✅ Decomposição de problemas em etapas lógicas

### 💻 Exemplo de Implementação
```xml
<system_instructions>
  Before generating the final JSON output, you MUST follow this thought process:
  
  <thought_process>
    <step1>
      Identify all visible text in the image
      - Is the text readable?
      - Is it handwritten or printed?
      - Are there any ambiguous characters?
    </step1>
    
    <step2>
      Extract the four REQUIRED fields:
      - produto: Can I identify a dish name?
      - descricao: Are ingredients or preparation method visible?
      - categoria: What type of dish is this?
      - preco: Is there a clear price in R$?
    </step2>
    
    <step3>
      Quality validation:
      - Do I have ALL four required fields?
      - If NO → Return error JSON requesting better image
      - If YES → Proceed to normalization
    </step3>
    
    <step4>
      Normalize data:
      - Convert price format (R$ 29,90 → 29.90)
      - Standardize category name
      - Identify allergens from ingredients
    </step4>
    
    <step5>
      Generate final JSON output
    </step5>
  </thought_process>
  
  After completing this thought process internally, output ONLY the final JSON.
</system_instructions>
```

### 🔍 Exemplo de Resultado

**Input:** Imagem borrada de cardápio

**Raciocínio Interno (invisível):**
```
Step 1: Text detection → "Fr... Grelh... R$ ..."
Step 2: Required fields → produto: unclear, descricao: missing
Step 3: Validation → Missing fields → RETURN ERROR
```

---

## 2️⃣ `<workflow>` - Fluxo Executável de Ações

### 📌 Definição

Tag que define uma **sequência de ações práticas** com entradas, processamento e saídas em cada etapa. Pode ser rastreado e logado.

### 🎯 Quando Usar

- ✅ Processos multi-etapas com outputs intermediários
- ✅ Integração com ferramentas externas (APIs, funções)
- ✅ Pipelines de automação
- ✅ Quando precisa debugar cada step

### 💻 Exemplo de Implementação
```xml
<system_instructions>
  Follow this workflow for processing menu images:
  
  <workflow>
    <step id="1" action="image_analysis">
      <input>Raw image from user</input>
      <process>
        - Run OCR to extract text
        - Identify text orientation and language
        - Detect handwriting vs printed text
      </process>
      <output>Extracted text string</output>
      <next_step>2</next_step>
    </step>
    
    <step id="2" action="data_extraction">
      <input>Extracted text from step 1</input>
      <process>
        - Parse text for dish names
        - Extract prices using regex pattern
        - Identify category keywords
        - Extract ingredient lists
      </process>
      <output>Raw data dictionary</output>
      <next_step>3</next_step>
    </step>
    
    <step id="3" action="validation">
      <input>Raw data dictionary from step 2</input>
      <process>
        - Check required fields existence
        - Validate data types
        - Check business rules
      </process>
      <decision>
        IF all_required_fields_valid THEN go to step 4
        ELSE go to step 5
      </decision>
    </step>
    
    <step id="4" action="normalization">
      <input>Validated data from step 3</input>
      <process>
        - Convert price format to decimal
        - Standardize category name
        - Detect allergens from ingredients
      </process>
      <output>Final JSON object</output>
      <next_step>END</next_step>
    </step>
    
    <step id="5" action="error_handling">
      <input>Missing fields list from step 3</input>
      <process>
        - Generate error JSON
        - List missing required fields
        - Provide user-friendly message
      </process>
      <output>Error JSON object</output>
      <next_step>END</next_step>
    </step>
  </workflow>
</system_instructions>
```

---

## 🔄 Comparação Direta

| Aspecto | `<thought_process>` | `<workflow>` |
|---------|---------------------|--------------|
| **Natureza** | Raciocínio cognitivo | Processo executável |
| **Visibilidade** | Implícito (interno) | Explícito (rastreável) |
| **Outputs** | Apenas output final | Output por step |
| **Logging** | Não logável | Logável/debugável |
| **Decisões** | Lógica simples | Fluxos condicionais complexos |
| **Uso ideal** | Qualidade de resposta | Automação e integração |
| **Analogia** | "Pensar antes de falar" | "Seguir uma receita" |

---

## 🎯 Quando Usar Cada Um?

### Use `<thought_process>` para:
```xml
✅ Validar qualidade de dados de entrada
✅ Decidir entre múltiplas estratégias de resposta
✅ Raciocinar sobre categorização/classificação
✅ Inferir informações implícitas
✅ Avaliar confiança antes de responder
```

**Exemplo de caso:** "A imagem está legível o suficiente para extrair os dados?"

### Use `<workflow>` para:
```xml
✅ Integração com sistemas externos (n8n, APIs)
✅ Pipelines de processamento (OCR → Parse → Validate)
✅ Quando precisa debugar etapas específicas
✅ Processos que chamam múltiplas ferramentas
✅ Automações com checkpoints intermediários
```

**Exemplo de caso:** "Pipeline de extração de cardápio em 5 etapas com validações"

---

## 💎 Versão Híbrida (Recomendada)

Para máxima eficiência, combine ambas as tags:
```xml
<system_instructions>
  <workflow>
    <step id="1" action="ocr_extraction">
      <input>Image/PDF from user</input>
      
      <!-- Raciocínio dentro do workflow -->
      <thought_process>
        - Analyze image quality (sharp vs blurry)
        - Detect text orientation
        - Identify handwritten vs printed text
        - Assess OCR confidence level
      </thought_process>
      
      <output>Extracted text + confidence score</output>
      <next_step>2</next_step>
    </step>
    
    <step id="2" action="data_parsing">
      <input>Extracted text</input>
      
      <!-- Validação cognitiva antes de decidir -->
      <thought_process>
        - Can I identify produto clearly?
        - Is there enough context to generate descricao?
        - Which categoria matches best?
        - Is the preco format parseable?
      </thought_process>
      
      <decision>
        IF confidence < 70% OR missing_required_fields THEN step 4
        ELSE step 3
      </decision>
    </step>
    
    <step id="3" action="normalization">
      <input>Raw parsed data</input>
      <process>Normalize and generate final JSON</process>
      <output>Valid JSON object</output>
    </step>
    
    <step id="4" action="error_response">
      <input>Missing fields / low confidence</input>
      <output>Error JSON requesting better image</output>
    </step>
  </workflow>
</system_instructions>
```

### Vantagens da Abordagem Híbrida

| Vantagem | Descrição |
|----------|-----------|
| 🧠 **Qualidade** | `<thought_process>` garante decisões corretas |
| 🔧 **Rastreabilidade** | `<workflow>` permite debug de cada etapa |
| ⚡ **Eficiência** | Combina validação cognitiva com execução estruturada |
| 🎯 **Precisão** | Reduz erros de interpretação |

---

## 📊 Diagrama de Fluxo
```
┌─────────────────────────────────────────────────────────────┐
│                    FLUXO COMPLETO                           │
└─────────────────────────────────────────────────────────────┘

INPUT (Imagem de cardápio)
  ↓
[WORKFLOW STEP 1: OCR]
  ├─ <thought_process>: Avaliar qualidade da imagem
  ├─ Process: Extrair texto
  └─ Output: "Frango Grelhado R$25"
  ↓
[WORKFLOW STEP 2: Parsing]
  ├─ <thought_process>: Posso extrair todos campos obrigatórios?
  ├─ Decision: SIM/NÃO
  └─ Output: Raw data
  ↓
[WORKFLOW STEP 3: Normalization]
  ├─ Process: Aplicar regras
  └─ Output: JSON normalizado
  ↓
FINAL OUTPUT
```

---

## 🚀 Boas Práticas

### ✅ DO (Faça)
```xml
<!-- Bom: Thought process com objetivos claros -->
<thought_process>
  <validation>Check if all required fields are present</validation>
  <decision>If missing → error, else → proceed</decision>
</thought_process>

<!-- Bom: Workflow com steps bem definidos -->
<workflow>
  <step id="1" action="extract">
    <input>Clear input definition</input>
    <output>Clear output definition</output>
  </step>
</workflow>
```

### ❌ DON'T (Evite)
```xml
<!-- Ruim: Misturar conceitos -->
<thought_process>
  <step id="1">Extract data</step> <!-- Isso é workflow, não thought -->
</thought_process>

<!-- Ruim: Workflow sem estrutura -->
<workflow>
  Do something and then do something else <!-- Sem steps claros -->
</workflow>
```
---

## 📚 Recursos Adicionais

- [Prompt Engineering Guide](https://www.promptingguide.ai/)
- [Anthropic Prompt Library](https://docs.anthropic.com/claude/prompt-library)
- [OpenAI Best Practices](https://platform.openai.com/docs/guides/prompt-engineering)

---