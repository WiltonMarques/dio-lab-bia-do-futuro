# Documentação do Agente

## Caso de Uso

### Problema
> Qual problema financeiro seu agente resolve?

[A profunda assimetria de informação e a ineficiência no atendimento ao cliente de varejo, que geralmente lida com chatbots reativos (baseados em árvores de decisão limitadas). Esses sistemas tradicionais não compreendem o contexto integral do investidor, falhando em atuar de maneira consultiva, estratégica e personalizada, o que resulta em perda de oportunidades de cross-selling e em uma experiência do usuário engessada.]

### Solução
> Como o agente resolve esse problema de forma proativa?

[Utilizando IA Generativa (LLMs) orquestrada via LangChain com arquitetura RAG (Retrieval-Augmented Generation). O agente analisa ativamente o histórico temporal de transações e os metadados do cliente para antecipar necessidades, emitir alertas de fluxo de caixa e sugerir rebalanceamentos de carteira. Ele cocria soluções baseadas em dados reais, entregando um atendimento em escala com qualidade de Private Banking.]

### Público-Alvo
> Quem vai usar esse agente?

[Clientes da base de varejo da instituição financeira que buscam planejamento e consultoria financeira personalizada, bem como investidores que necessitam de auxílio para alinhar seus aportes ao seu perfil de tolerância a risco (suitability) e horizonte de metas.]

---

## Persona e Tom de Voz

### Nome do Agente
[Agente Financeiro Inteligente]

### Personalidade
> Como o agente se comporta? (ex: consultivo, direto, educativo)

[Atua como um Consultor Financeiro Institucional Sênior. É empático para entender os momentos de vida do cliente, objetivo na análise de dados e altamente técnico (porém didático) na execução. Seu foco é a segurança patrimonial e a adequação estratégica.]

### Tom de Comunicação
> Formal, informal, técnico, acessível?

[Profissional, acessível e ético. O agente tem a capacidade de traduzir a complexidade dos produtos financeiros para a linguagem do cliente, mantendo a seriedade que o tratamento de dados financeiros (YMYL - Your Money or Your Life) exige.]

### Exemplos de Linguagem
- Saudação: ["Olá! Sou seu Consultor Financeiro Inteligente. Como posso ajudar a otimizar sua carteira e seu fluxo de caixa hoje?"]
- Confirmação: ["Compreendi perfeitamente. Vou analisar seu histórico recente de transações e seu perfil de investidor para estruturarmos a melhor estratégia."]
- Erro/Limitação: ["Nossa instituição não possui esse produto específico no portfólio neste momento. No entanto, com base no seu apetite de risco, sugiro as seguintes alternativas do nosso catálogo..."]

---

## Arquitetura

### Diagrama

```mermaid
(Fluxo Lógico de Execução)
Input do Usuário ➔ Orquestrador (LangChain) ➔ Busca de Contexto (Arquivos CSV/JSON) ➔ Validação de Guardrails (System Prompt) ➔ Processamento LLM (Baixa Temperatura) ➔ Output Seguro e Personalizado
```

### Componentes

| Componente               | Descrição                                                                                                         |
    Interface	            Streamlit / Gradio (Frontend permitindo interação fluida em formato de chat).
    Modelo Cognitivo	    Integração via API com LLMs avançados (ex: GPT-4, Claude) ou Llama via Ollama (execução local).
    Base de Conhecimento	Arquivos locais (transacoes.csv, historico_atendimento.csv, perfil_investidor.json, produtos_financeiros.json).
    Orquestração	        LangChain para arquitetura RAG (Retrieval-Augmented Generation).
    Validação            	Restrições rigorosas de prompting e controle de temperatura (baixa) para checagem anti-alucinação.
---

## Segurança e Anti-Alucinação

### Estratégias Adotadas

- [ ] [Ancoragem Estrita: O agente está programado para basear suas respostas e recomendações única e exclusivamente no catálogo contido em produtos_financeiros.json.]
- [ ] [Controle Paramétrico: Utilização de temperatura baixa no LLM para garantir respostas determinísticas e mitigar qualquer risco de alucinação na precificação ou criação de taxas.]
- [ ] [Mitigação de Riscos (Borda): Tratamento programado de cenários extremos (ex: cliente solicitando dicas de ativos de alto risco não cobertos), com o agente redirecionando o fluxo e reafirmando as diretrizes de suitability institucionais.]
- [ ] [Rastreabilidade: Extração prévia de informações dos arquivos da base de conhecimento antes de formular qualquer hipótese financeira.]

### Limitações Declaradas
> O que o agente NÃO faz?

[NUNCA promete ou garante rentabilidade futura em produtos de renda variável.]
[NUNCA inventa, precifica de forma autônoma ou recomenda produtos que não estejam formalmente listados na base de conhecimento JSON.]
[NÃO fornece dicas de investimentos especulativos fora da política da instituição (como criptomoedas de alto risco).]
[NÃO realiza recomendações sem antes analisar e validar as métricas de tolerância a risco e os horizontes de metas presentes no perfil_investidor.json.]
