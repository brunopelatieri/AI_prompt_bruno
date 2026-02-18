# 🤖 Prompt Engineering & Versioning Guide (SemVer 1.0.0)

Este documento define o padrão de versionamento e a arquitetura de prompts para o ecossistema do **DiretorIA App**. O objetivo é garantir que cada instrução enviada ao modelo de IA seja rastreável, testável e segura para produção.

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
# 🤖 Estratégia de Versionamento de Prompts (Prompt Ops)

Este documento define o padrão de versionamento e a arquitetura de governança para os prompts do ecossistema **DiretorIA App**. O objetivo é garantir **reprodutibilidade**, **rastreabilidade** e **estabilidade** em produção.

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
