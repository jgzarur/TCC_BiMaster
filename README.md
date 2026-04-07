# 🔍 Auditoria Inteligente de Dados com IA Generativa
### Detecção de Anomalias com LangChain e GPT

> **Trabalho de Conclusão de Curso — PUC-Rio | BI Master**
> Rio de Janeiro, Março de 2026

**Autores:** João Gabriel Lopes Zarur · Thiago de Assis Sobreira
**Orientador:** Leonardo Alfredo Forero Mendoza

---

## 📋 Sobre o Projeto

Este projeto propõe um pipeline completo de **auditoria inteligente de dados** que combina Engenharia de Dados com Inteligência Artificial Generativa para automatizar a detecção de anomalias em bases de pagamentos educacionais.

O sistema não apenas identifica desvios estatísticos, mas também permite que analistas façam **perguntas em linguagem natural** sobre os dados — sem necessidade de conhecimento técnico em SQL.

**Problema central:** Como identificar automaticamente inconsistências em grandes bases de dados de pagamentos e permitir consultas em linguagem natural, sem exigir conhecimento técnico especializado?

---

## 🏗️ Arquitetura

O projeto é estruturado em **6 módulos** em pipeline sequencial:

| Módulo | Função | Tecnologia |
|--------|--------|------------|
| **Parte 1** | Geração de dados sintéticos com anomalias | Python / NumPy / Pandas |
| **Parte 2** | Dashboard de integridade visual no terminal | Python / ANSI Colors |
| **Parte 3** | Persistência em banco de dados relacional | SQLite / SQLAlchemy |
| **Parte 4** | Agente SQL com LLM para consultas naturais | LangChain / OpenAI GPT-4.1-mini |
| **Parte 5** | Guardrail de segurança para comandos SQL | Python / Regex |
| **Parte 6** | Análises em linguagem natural automatizadas | LangChain SQL Agent |

---

## 🛠️ Stack Tecnológica

- **Python 3.x** — linguagem principal
- **Pandas & NumPy** — manipulação e geração de dados
- **SQLite** — banco de dados relacional embutido
- **SQLAlchemy** — ORM para abstração do banco
- **LangChain** — framework de orquestração de LLMs
- **OpenAI GPT-4.1-mini** — modelo de linguagem para o agente SQL

---

## 🚀 Módulos em Detalhe

### Parte 1 — Geração de Dados Sintéticos

Gera uma base realista simulando pagamentos ao longo de dois dias consecutivos com anomalias controladas.

```python
base_auditoria = gerar_dados_dashboard(total_alunos=1000, taxa_anomalia=0.03)
# → 1.000 alunos monitorados | ~30 alunos com anomalias geradas
```

**Lógica:**
- Registros para dois dias: `2025-05-29` (base) e `2025-05-30` (comparação)
- Cada aluno possui três valores: **boleto**, **receita** e **bolsa**
- Alunos normais variam ±1% entre os dias
- Alunos anômalos variam ±20% a ±30%

---

### Parte 2 — Dashboard de Integridade

Apresenta no terminal um panorama da saúde dos dados, calculando variações percentuais e classificando anomalias por gravidade.

**Critérios:**
- Desvio > 10%: classificado como **anomalia**
- Desvio > 15%: classificado como gravidade **ALTA**
- Exibe os top 10 casos mais críticos

---

### Parte 3 — Persistência em SQLite

Persiste o DataFrame em banco SQLite para permitir consultas SQL dinâmicas pelo agente de IA.

**Schema da tabela `auditoria_pagamentos`:**

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| `id_aluno` | TEXT | Identificador único (ex: MATRICULA_0001) |
| `data` | TEXT | Data do registro |
| `boleto` | REAL | Valor do boleto |
| `rob` | REAL | Resultado Operacional Bruto |
| `bolsa` | REAL | Valor da bolsa |

---

### Parte 4 — Agente SQL com LLM (LangChain + GPT)

Núcleo de inovação do projeto: agente de IA que recebe perguntas em linguagem natural, converte automaticamente para SQL, executa no banco e retorna os resultados interpretados.

```python
llm = ChatOpenAI(model='gpt-4.1-mini', temperature=0)
agent = create_sql_agent(llm=llm, db=db, verbose=True)
```

> `temperature=0` garante respostas determinísticas — essencial para auditoria reprodutível.

---

### Parte 5 — Guardrail de Segurança SQL

Camada de proteção que bloqueia comandos SQL destrutivos, garantindo que o agente opere apenas com `SELECT`.

```python
# Comandos proibidos: INSERT, UPDATE, DELETE, DROP, ALTER, CREATE, TRUNCATE, ATTACH, PRAGMA
def somente_select(sql: str) -> bool:
    ...
```

| Comando | Status |
|---------|--------|
| `SELECT * FROM auditoria_pagamentos LIMIT 5` | ✅ PERMITIDO |
| `DROP TABLE auditoria_pagamentos` | 🚫 BLOQUEADO |

---

### Parte 6 — Análises em Linguagem Natural

Demonstração do agente respondendo automaticamente cinco perguntas analíticas complexas, incluindo:

1. Contagem de anomalias com variação > 10%
2. Média e máximo da variação absoluta de boleto e ROB
3. Alunos com desvio crítico acima de 15% (top 20)
4. Comparativo de ocorrências entre variação de boleto e ROB

---

## 📊 Resultados

| Métrica | Resultado |
|---------|-----------|
| Total de alunos monitorados | 1.000 |
| Taxa de anomalias configurada | 3% (~30 alunos) |
| Limiar de detecção | > 10% de variação |
| Perguntas respondidas pelo agente | 7 (5 automatizadas + 2 manuais) |
| Guardrail: bloqueio de comandos destrutivos | 100% dos testes |

---

## 📌 Conclusão

O projeto demonstrou que a combinação de Engenharia de Dados com IA Generativa é uma abordagem **poderosa e acessível** para auditoria de dados em larga escala. O agente LangChain + GPT-4.1-mini provou-se eficiente na geração automática de SQL, eliminando a barreira técnica para analistas não especializados.

### Contribuições
- Framework end-to-end de auditoria com IA Generativa
- Demonstração prática de LLMs em domínios financeiros
- Guardrail de segurança para sistemas de agentes SQL
- Base sintética configurável para testes de detecção de anomalias

### Trabalhos Futuros
- Integração com fontes de dados reais e pipelines ETL em produção
- Interface web para democratizar o acesso ao sistema
- Avaliação de outros modelos LLM (Claude, Gemini) para comparação
- Alertas automáticos via e-mail ou Slack em caso de anomalias

---

## 📚 Referências

- CHASE, H. et al. **LangChain**: Building applications with LLMs through composability. GitHub, 2023. https://github.com/langchain-ai/langchain
- OPENAI. **GPT-4 Technical Report**. arXiv, 2023. https://arxiv.org/abs/2303.08774
- **Pandas** Documentation. https://pandas.pydata.org
- **SQLite** Documentation. https://www.sqlite.org/docs.html
- McKINNEY, W. **Python for Data Analysis**. 3. ed. O'Reilly Media, 2022.

---

*PUC-Rio | BI Master — Rio de Janeiro, Março de 2026*
