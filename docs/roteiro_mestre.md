# Roteiro-Mestre para Criação de Prompt Mestre

## Objetivo
Transformar uma **ideia de projeto já discutida e refinada** em um **Prompt Mestre executável**, capaz de iniciar automaticamente o fluxo de execução em qualquer LLM.

---

## Estrutura do Roteiro

A LLM deve criar o Prompt Mestre seguindo estas diretrizes:

<pre>
- Execução imediata: o Prompt Mestre, quando gerado, entra no modo assistente automaticamente.
- Perguntas guiadas ao usuário:
    1. O núcleo já foi criado? (SIM / NÃO)
    2. Qual estágio ou parte do núcleo já existe?
    3. Qual ação deseja realizar? (CRIAR / CORRIGIR / ATUALIZAR / INTEGRAR)
    4. Qual módulo ou arquivo específico?
    5. Detalhes precisos da ação ou código mestre, se necessário
- Checklist mental automático antes de qualquer ação:
    - Verificar se o módulo selecionado é o correto
    - Garantir compatibilidade das funções existentes
    - Validar parametrização consistente com a ideia do núcleo
    - Confirmar disponibilidade de testes funcionais
    - Assegurar que nenhuma lógica do núcleo será quebrada
- Entrega de código modular real pronto para copy & paste (`cat`)
- Registro estruturado de erros e logs
</pre>

---

## Diretrizes da LLM

1. O Prompt Mestre gerado deve ser **totalmente autoexecutável e funcional**.  
2. Todas as perguntas, checklist e fluxos devem ser definidos **internamente**, baseados apenas na ideia processada.