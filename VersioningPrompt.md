# 🤖 Prompt Engineering & Versioning Guide (SemVer 1.0.0)

Este documento define o padrão de versionamento e a arquitetura de prompts para um ecossistema. O objetivo é garantir que cada instrução enviada ao modelo de IA seja rastreável, testável e segura para produção.

---

## 📌 1. Estratégia de Versionamento Semântico (SemVer)

Adotamos o formato `MAJOR.MINOR.PATCH` para o ciclo de vida dos prompts:

| Versão | Tipo | Descrição | Exemplo |
| :--- | :--- | :--- | :--- |
| **MAJOR** | `2.0.0` | Mudança de arquitetura (ex: migrar de Texto para XML) ou alteração drástica no comportamento. | Alteração no objetivo principal da IA. |
| **MINOR** | `1.1.0` | Adição de novos exemplos (*few-shot*), novas variáveis ou refinamento de instruções. | Inclusão de uma nova regra de negócio. |
| **PATCH** | `1.0.1` | Correções de ortografia, gramática ou ajustes finos de tom de voz. | Ajuste de um adjetivo ou clareza de frase. |

---

## 🏗️ 2. Estrutura Padrão (Template XML)

Utilizamos **XML** para delimitar contextos, pois os modelos modernos (como Claude e Gemini) interpretam melhor blocos estruturados.

```xml
<prompt_definition>
    <metadata>
        <version>1.0.0</version>
        <author>Bruno</author>
        <last_update>2026-02-17</last_update>
        <changelog>Lançamento inicial da estrutura base para integração n8n.</changelog>
    </metadata>

    <system_role>
        Descreva aqui a persona da IA (Ex: Você é um Diretor Criativo...).
    </system_role>

    <context>
        Informações sobre o cenário, dados do usuário e objetivos da tarefa.
    </context>

    <instructions>
        1. Siga o formato de saída JSON.
        2. Não utilize saudações ou introduções.
        3. Priorize a concisão.
    </instructions>

    <input_data>
        {{user_message}}
    </input_data>

    <output_format>
        A saída deve ser estritamente em JSON: {"status": "success", "response": "..."}
    </output_format>
</prompt_definition>

```

---

## 📂 3. Organização do Repositório (Git)

A estrutura de pastas reflete a maturidade e o histórico de cada prompt:

```text
/prompts
  ├── /projects-name
  │   ├── /archive             # Versões obsoletas para histórico histórico
  │   │   ├── v0.8.0-beta.txt
  │   │   └── v0.9.0-beta.txt
  │   ├── main.txt             # Versão estável (v1.0.0)
  │   └── experimental.txt     # Versão em testes (Sandbox)
  └── README.md                # Este arquivo de documentação

```

---

## ⚙️ 4. Fluxo de Integração (Stack Tecnológica)

O versionamento é espelhado entre o código e o banco de dados para automação via **n8n**, garantindo que a IA utilize sempre a instrução homologada:

1.  **GitHub**: *Single Source of Truth* (Fonte da Verdade). O prompt é editado, revisado e versionado aqui.
2.  **Supabase**: Tabela `prompts_registry` armazena o histórico de versões e o status de ativação (`is_active`).
3.  **n8n**: O workflow consome a API do Supabase buscando o prompt por `slug` e filtrando por `version` ou `is_active: true`.
4.  **Evolution API**: Recebe o prompt final processado pelo n8n para realizar a interação com o usuário final.

---

## 📊 5. Estrutura da Tabela (Supabase)

Para gerenciar o ciclo de vida dos prompts diretamente no banco de dados, utilize a seguinte estrutura de colunas:

| Coluna | Tipo | Descrição |
| :--- | :--- | :--- |
| `slug` | `text` | Identificador único do prompt (ex: `atendimento_vendas`). |
| `version` | `varchar` | String seguindo o padrão SemVer (ex: `1.0.0`). |
| `content` | `text` | O corpo completo do prompt (instruções, contexto e regras). |
| `is_active` | `boolean` | Flag booleana para indicar qual versão o n8n deve consumir por padrão. |
| `created_at` | `timestamp` | Registro automático de data/hora da criação da versão. |

---

## 🚀 6. Comandos de Versionamento (Git)

Sempre que uma versão estável for atingida no repositório, utilize **Tags** para facilitar o rastreamento e eventuais *rollbacks*:

```bash
# 1. Criar tag da versão estável localmente
git tag -a v1.0.0 -m "Lançamento inicial estável - App"

# 2. Enviar a tag para o repositório remoto (GitHub)
git push origin v1.0.0

# 3. Listar todas as versões de prompts existentes
git tag -l

```

---
