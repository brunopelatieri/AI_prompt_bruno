# Workflow XML Architecture for LLM Agents

## Enterprise Deterministic Pattern + Definitive Structural Template

------------------------------------------------------------------------

# 1. Objective

This document defines a production-grade XML workflow architecture for
LLM-based agents, optimized for:

-   Deterministic execution
-   Structured tool calling
-   Explicit state management
-   Multi-step orchestration
-   Enterprise scalability
-   Auditability

This pattern converts probabilistic LLM behavior into a controlled
finite-state-machine execution model.

------------------------------------------------------------------------

# 2. Core Architectural Principles

## 2.1 Determinism Over Interpretation

All branching logic MUST depend on explicit state variables.

Never:

"If it seems to be a clinic..."

Always:

IF lead_type == "clinic"

------------------------------------------------------------------------

## 2.2 Strict Separation of Layers

Each `<step>`{=html} must isolate:

1.  Input layer
2.  Cognitive analysis layer
3.  Classification layer
4.  State mutation layer
5.  Deterministic decision layer
6.  Tool invocation layer
7.  Output (user-facing) layer
8.  Transition control layer

------------------------------------------------------------------------

# 3. State Modeling Strategy

## 3.1 Avoid Multiple Boolean Flags

Problematic:

``` xml
<lead_is_clinic>true|false</lead_is_clinic>
<lead_type_unclear>true|false</lead_type_unclear>
```

Risk: conflicting logical states.

## 3.2 Prefer Enumerated State (Recommended)

``` xml
<lead_type>clinic | non_clinic | unclear</lead_type>
```

Advantages:

-   Single source of truth
-   No conflict risk
-   Easier logging
-   Scalable classification model

------------------------------------------------------------------------

# 4. Canonical Enterprise Step Template

This is the definitive reusable architecture model.

``` xml
<workflow>

  <workflow_state>
    <current_step></current_step>
    <lead_name></lead_name>
    <lead_type></lead_type>
    <last_tool_invoked></last_tool_invoked>
  </workflow_state>


  <step id="2" action="lead_filter_clinic_type">

    <!-- 1️⃣ INPUT -->
    <input>
      Raw user response about business type
    </input>


    <!-- 2️⃣ THOUGHT PROCESS (Internal Cognitive Layer) -->
    <thought_process visibility="internal">
      Analyze semantic indicators in user response.
      Identify if business relates to:
      - medical clinic
      - dental clinic
      - other business
      - ambiguous answer
    </thought_process>


    <!-- 3️⃣ CLASSIFICATION -->
    <classification>
      Classify strictly into one of:
      - clinic
      - non_clinic
      - unclear
    </classification>


    <!-- 4️⃣ STATE UPDATE -->
    <state>
      <lead_type>clinic | non_clinic | unclear</lead_type>
    </state>


    <!-- 5️⃣ DECISION LAYER (Deterministic Only) -->
    <decision>
      IF lead_type == "clinic" THEN go_to_step="3"
      ELSE IF lead_type == "unclear" THEN action="ask_clarification"
      ELSE invoke_tool="attendantSupport"
    </decision>


    <!-- 6️⃣ TOOL INVOCATION -->
    <invoke tool="attendantSupport" silent="true"
            condition="lead_type == 'non_clinic'"/>


    <!-- 7️⃣ OUTPUT LAYER -->
    <output>

      <message condition="lead_type == 'clinic'">
        Perfeito, {{lead_name}}! 😊
        Que ótimo ter uma clínica com a gente.
        Como você conheceu a empresa?
      </message>

      <message condition="lead_type == 'unclear'">
        {{lead_name}}, só para confirmar:
        seu negócio é uma clínica médica ou odontológica?
      </message>

      <message condition="lead_type == 'non_clinic'">
        Entendi, {{lead_name}}!
        Vou te conectar com o especialista certo.
      </message>

    </output>


    <!-- 8️⃣ NEXT STEP CONTROL -->
    <next_step condition="lead_type == 'clinic'">3</next_step>
    <next_step condition="lead_type == 'unclear'">2</next_step>
    <next_step condition="lead_type == 'non_clinic'">END</next_step>

  </step>

</workflow>
```

------------------------------------------------------------------------

# 5. Execution Pipeline Model

Recommended internal execution order:

1.  Read input
2.  Run cognitive analysis
3.  Classify
4.  Update state
5.  Evaluate decision
6.  Execute tool (if applicable)
7.  Produce user message
8.  Transition to next step

This mirrors classical Finite State Machine (FSM) architecture.

------------------------------------------------------------------------

# 6. Tool Invocation Standard

Avoid natural language triggers.

Never:

\[INVOKE: attendantSupport\]

Always:

``` xml
<invoke tool="attendantSupport" silent="true"/>
```

Benefits:

-   Machine-parsable
-   Middleware-safe
-   Lower hallucination risk
-   Enterprise compatible

------------------------------------------------------------------------

# 7. Production Hardening Checklist

✔ Enumerated states\
✔ Decision depends only on state\
✔ No control logic inside `<thought_process>`{=html}\
✔ Structured tool invocation\
✔ Explicit next_step transitions\
✔ No mixing reasoning with execution\
✔ Persistent workflow_state for long sessions

------------------------------------------------------------------------

# 8. Pareto Optimization

20% changes that generate 80% robustness:

-   Replace implicit reasoning with explicit state
-   Use enum instead of multiple booleans
-   Separate cognitive layer from decision layer
-   Make all branching deterministic

------------------------------------------------------------------------

# 9. Conceptual Model

Implicit reasoning agent: Improvisational judge.

Explicit state architecture: Deterministic state machine executing
defined transitions.

------------------------------------------------------------------------

# 10. Final Conclusion

For prototypes: Decision inside `<thought_process>`{=html} may work.

For enterprise production systems: Explicit state + deterministic
decision layer is mandatory.

This architecture transforms LLM behavior from probabilistic text
generation into structured, controllable workflow execution.
