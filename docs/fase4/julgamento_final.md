# Julgamento Consoliado Final — Fase 4

Esta página consolida os resultados das avaliações de **Confiabilidade** e **Manutenibilidade** do _Sua Grade UnB_, executadas na Fase 4 do processo de avaliação de qualidade, e emite o **julgamento final** do produto com base nos critérios estabelecidos na Fase 2. Os dados aqui reunidos são extraídos das páginas de avaliação individuais de cada característica ([Avaliação — Confiabilidade](../fase4/confiabilidade.md) e [Avaliação — Manutenibilidade](../fase4/manutenibilidade.md)).

---

## 1. Consolidação dos Resultados

### 1.1 — Confiabilidade

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: left;">
    <tr>
      <th><b>Cód.</b></th>
      <th><b>Métrica</b></th>
      <th><b>Subcaracterística</b></th>
      <th><b>Resultado</b></th>
      <th><b>Classificação</b></th>
    </tr>
    <tr>
      <td>M1.1</td>
      <td>Tempo Médio Entre Falhas (MTBF)</td>
      <td>Maturidade</td>
      <td>2,6 minutos</td>
      <td>Regular</td>
    </tr>
    <tr>
      <td>M2.1</td>
      <td>% de operações críticas com tratamento de exceção</td>
      <td>Tolerância a falhas</td>
      <td>17,91%</td>
      <td>Insatisfatório</td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 5.1 — Resultados consolidados das métricas de Confiabilidade.</figcaption></font>
  </div>
</div>

### 1.2 — Manutenibilidade

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: left;">
    <tr>
      <th><b>Cód.</b></th>
      <th><b>Métrica</b></th>
      <th><b>Subcaracterística</b></th>
      <th><b>Resultado</b></th>
      <th><b>Classificação</b></th>
    </tr>
    <tr>
      <td>M1.1</td>
      <td>Nível de independência dos componentes</td>
      <td>Modularidade</td>
      <td>57,14%</td>
      <td>Regular</td>
    </tr>
    <tr>
      <td>M2.1</td>
      <td>Nível de qualidade dos logs do sistema</td>
      <td>Analisabilidade</td>
      <td>0%</td>
      <td>Insatisfatório</td>
    </tr>
    <tr>
      <td>M3.1</td>
      <td>Tempo médio para realizar uma alteração</td>
      <td>Modificabilidade</td>
      <td>36 horas</td>
      <td>Insatisfatório</td>
    </tr>
    <tr>
      <td>M4.1</td>
      <td>Porcentagem de cobertura de testes</td>
      <td>Testabilidade</td>
      <td>80,8%</td>
      <td>Bom</td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 5.2 — Resultados consolidados das métricas de Manutenibilidade.</figcaption></font>
  </div>
</div>

---

## 2. Julgamento por Característica

Os critérios de julgamento aplicados são os definidos na Fase 2 para a característica Confiabilidade e adotados por analogia para a Manutenibilidade:

- **Aceitável:** ≥ 70% das métricas classificadas como "Bom" ou "Excelente".
- **Parcialmente aceitável:** entre 40% e 69% das métricas com nível "Regular" ou superior.
- **Inaceitável:** > 30% das métricas atingindo o nível "Insatisfatório".

### 2.1 — Confiabilidade

Das 2 métricas avaliadas:

- 1 métrica atingiu nível **Regular** ou superior (M1.1 — MTBF): **50%**
- 1 métrica atingiu nível **Insatisfatório** (M2.1 — Cobertura de exceções): **50%**

O percentual de métricas Regular ou superior (50%) enquadra-se na faixa "Parcialmente aceitável" (40%–69%). Simultaneamente, o percentual de métricas Insatisfatório (50%) ultrapassa o limite de 30% do critério "Inaceitável". Há, portanto, sobreposição entre os dois critérios. Adotando a ordem de verificação — Aceitável → Parcialmente aceitável → Inaceitável —, a Confiabilidade é julgada como:

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: center;">
    <tr>
      <th><b>Característica</b></th>
      <th><b>Métricas Bom/Excelente</b></th>
      <th><b>Métricas Regular ou superior</b></th>
      <th><b>Métricas Insatisfatório</b></th>
      <th><b>Julgamento</b></th>
    </tr>
    <tr>
      <td>Confiabilidade</td>
      <td>0 / 2 (0%)</td>
      <td>1 / 2 (50%)</td>
      <td>1 / 2 (50%)</td>
      <td><b>Parcialmente aceitável</b></td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 5.3 — Julgamento da Confiabilidade.</figcaption></font>
  </div>
</div>

O sistema funciona, mas exibe instabilidades pontuais perceptíveis ao usuário final (MTBF de 2,6 minutos) e apresenta robustez interna muito aquém do esperado, com apenas 17,91% das operações críticas protegidas por tratamento de exceção. Esse cenário expõe o sistema a falhas silenciosas em produção.

### 2.2 — Manutenibilidade

Das 4 métricas avaliadas:

- 1 métrica atingiu nível **Bom** (M4.1 — Cobertura de testes): **25%**
- 1 métrica atingiu nível **Regular** (M1.1 — Independência dos componentes): **25%**
- 2 métricas atingiram nível **Insatisfatório** (M2.1 e M3.1): **50%**

O percentual de métricas Regular ou superior (M1.1 + M4.1 = 50%) está na faixa "Parcialmente aceitável" (40%–69%). O percentual Insatisfatório (50%) também ultrapassa o limite do critério "Inaceitável". Mesma situação de sobreposição; pelo mesmo critério de ordem de verificação:

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: center;">
    <tr>
      <th><b>Característica</b></th>
      <th><b>Métricas Bom/Excelente</b></th>
      <th><b>Métricas Regular ou superior</b></th>
      <th><b>Métricas Insatisfatório</b></th>
      <th><b>Julgamento</b></th>
    </tr>
    <tr>
      <td>Manutenibilidade</td>
      <td>1 / 4 (25%)</td>
      <td>2 / 4 (50%)</td>
      <td>2 / 4 (50%)</td>
      <td><b>Parcialmente aceitável</b></td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 5.4 — Julgamento da Manutenibilidade.</figcaption></font>
  </div>
</div>

O sistema demonstra boa cobertura de testes no backend (M4.1) e modularidade razoável (M1.1), indicando uma base arquitetural funcional. No entanto, a ausência total de logs operacionais (M2.1 = 0%) e o tempo médio de alteração elevado (M3.1 = 36 horas) comprometem a capacidade da equipe de monitorar, diagnosticar e corrigir falhas de forma proativa.

---

## 3. Julgamento Final Consolidado

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: left;">
    <tr>
      <th><b>Métrica</b></th>
      <th><b>Característica</b></th>
      <th><b>Resultado</b></th>
      <th><b>Classificação</b></th>
    </tr>
    <tr>
      <td>M1.1 — MTBF</td>
      <td>Confiabilidade</td>
      <td>2,6 min</td>
      <td>Regular</td>
    </tr>
    <tr>
      <td>M2.1 — % try/except</td>
      <td>Confiabilidade</td>
      <td>17,91%</td>
      <td>Insatisfatório</td>
    </tr>
    <tr>
      <td>M1.1 — Independência</td>
      <td>Manutenibilidade</td>
      <td>57,14%</td>
      <td>Regular</td>
    </tr>
    <tr>
      <td>M2.1 — Qualidade de logs</td>
      <td>Manutenibilidade</td>
      <td>0%</td>
      <td>Insatisfatório</td>
    </tr>
    <tr>
      <td>M3.1 — Tempo de alteração</td>
      <td>Manutenibilidade</td>
      <td>36 horas</td>
      <td>Insatisfatório</td>
    </tr>
    <tr>
      <td>M4.1 — Cobertura de testes</td>
      <td>Manutenibilidade</td>
      <td>80,8%</td>
      <td>Bom</td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 5.5 — Resumo geral de todas as métricas da Fase 4.</figcaption></font>
  </div>
</div>

Das 6 métricas avaliadas na Fase 4:

- **1 métrica** classificada como **Bom** — 17% do total
- **2 métricas** classificadas como **Regular** — 33% do total
- **3 métricas** classificadas como **Insatisfatório** — 50% do total

Aplicando os critérios gerais de julgamento ao conjunto completo:

- Critério "Aceitável" (≥70% Bom/Excelente): não atingido — apenas 17%.
- Critério "Parcialmente aceitável" (40%–69% Regular ou superior): atingido — 50% das métricas são Regular ou superiores (3 de 6).
- Critério "Inaceitável" (>30% Insatisfatório): também atingido — 50% das métricas são Insatisfatório (3 de 6).

Diante da sobreposição entre os critérios "Parcialmente aceitável" e "Inaceitável" — situação que ocorre quando o conjunto de métricas é pequeno e os resultados são polarizados —, a aplicação literal da ordem de verificação definida na Fase 2 classifica o produto como **Parcialmente aceitável**. Ainda assim, a alta concentração de métricas Insatisfatório (50%) é um sinal de alerta relevante que não deve ser negligenciado: o sistema está na fronteira com o nível Inaceitável, e qualquer nova falha ou deterioração das métricas avaliadas pode reclassificá-lo para esse patamar.

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: center;">
    <tr>
      <th><b>Confiabilidade</b></th>
      <th><b>Manutenibilidade</b></th>
      <th><b>Julgamento Final da Fase 4</b></th>
    </tr>
    <tr>
      <td>Parcialmente aceitável</td>
      <td>Parcialmente aceitável</td>
      <td><b>Parcialmente aceitável</b></td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 5.6 — Julgamento final consolidado da Fase 4.</figcaption></font>
  </div>
</div>

Retomando o propósito estabelecido na Fase 1 — avaliar se o _Sua Grade UnB_ apresenta nível de qualidade adequado para uso pelos estudantes da UnB e para continuidade do projeto como software open source —, os resultados indicam que **o sistema atende parcialmente a esse propósito**. O produto é utilizável e possui pontos técnicos positivos (cobertura de testes do backend e modularidade razoável), mas apresenta vulnerabilidades relevantes em sua robustez operacional e na capacidade da equipe mantenedora de identificar e corrigir problemas proativamente.

---

## 4. Recomendações Priorizadas

As recomendações a seguir consolidam as ações identificadas em ambas as avaliações e estão ordenadas por impacto estimado sobre a qualidade do produto.

**Prioridade Alta — Implementar instrumentação de logs (M2.1 — Manutenibilidade, 0%)**

A ausência de qualquer log operacional nos 12 eventos críticos mapeados é a vulnerabilidade mais grave identificada na Fase 4. Sem rastreabilidade, falhas em produção — incluindo indisponibilidade do SIGAA, erros de banco de dados e falhas de autenticação — são invisíveis para a equipe até que um usuário as reporte. Recomenda-se integrar o Django `LOGGING` nativo no backend e o `@sentry/nextjs` no frontend, cobrindo os eventos de autenticação, web scraping, geração de grade e erros 500 como ponto de partida.

**Prioridade Alta — Ampliar a cobertura de `try/except` (M2.1 — Confiabilidade, 17,91%)**

Apenas 17,91% das operações críticas estão protegidas por tratamento de exceção, com as categorias Web Scraping SIGAA (12,5%) e Banco de Dados (15,9%) como as mais vulneráveis. Recomenda-se adicionar blocos `try/except` nas chamadas de rede e parsing do scraper (capturando `requests.exceptions.RequestException`, `AttributeError` e `ValueError`) e nas operações de CRUD do Django (capturando `django.db.DatabaseError` e `django.db.IntegrityError`).

**Prioridade Alta — Corrigir o bug de autenticação (M1.1 — Confiabilidade, Falha 2)**

O fluxo de troca de conta Google redireciona o usuário para a página inicial sem exibir o seletor de contas do OAuth. Recomenda-se inspecionar o módulo `api/users/` e os componentes de login em `web/`, verificando se o encerramento da sessão ativa precede corretamente o redirecionamento ao provedor de identidade. Esta é a única falha com impacto médio identificada na sessão de uso.

**Prioridade Média — Instrumentar testes no Frontend (M4.1 — Manutenibilidade, frontend 0%)**

O frontend não possui nenhum cenário de teste automatizado, expondo os fluxos de interface, gerenciamento de estado local e interações do usuário a regressões não detectadas. Recomenda-se introduzir um framework de testes no Next.js — como _Jest_ com _React Testing Library_ para testes de componente, ou _Cypress_ para testes de fluxo ponta a ponta — cobrindo prioritariamente os fluxos de autenticação, busca de disciplinas e geração de grade.

**Prioridade Média — Revisar a metodologia de cálculo do M3.1 (Manutenibilidade)**

O resultado de 36 horas para o tempo médio de alteração é fortemente influenciado pelo piso de 24 horas adotado para resoluções no mesmo dia — uma decisão metodológica não prevista na Fase 2. Das 6 issues analisadas, 5 foram resolvidas no mesmo dia, e apenas 1 (Issue #99) levou 96 horas. Uma análise com granularidade em horas efetivas de trabalho provavelmente produziria um resultado mais próximo das faixas Bom ou Excelente e representaria melhor a agilidade real da equipe.

**Prioridade Baixa — Corrigir o bug visual na barra de navegação (M1.1 — Confiabilidade, Falha 1)**

O ícone de seção ativa fica preso entre as duas opções da barra de navegação inferior. Por não impedir nenhuma funcionalidade, este item pode ser tratado em uma sprint de polimento de interface, após a resolução dos itens de maior impacto.

---

## 5. Considerações Finais

O _Sua Grade UnB_ demonstra uma arquitetura funcional, com base de código modular e suíte de testes de backend bem estruturada. Esses aspectos evidenciam que o projeto tem condições técnicas de evoluir para níveis mais altos de qualidade com investimento focalizado.

No entanto, a avaliação da Fase 4 identificou dois padrões de fragilidade que, se não endereçados, comprometem a sustentabilidade operacional do produto: a ausência de resiliência a falhas no código (baixa cobertura de `try/except` e ausência de logs) e a existência de um bug funcional no fluxo de autenticação. Ambos impactam diretamente a experiência do usuário final e a capacidade da equipe mantenedora de operar o sistema com confiança.

A priorização das recomendações de Prioridade Alta — instrumentação de logs, ampliação do tratamento de exceções e correção do bug de autenticação — é suficiente para mover o sistema de "Parcialmente aceitável" para um patamar consistentemente aceitável nas próximas avaliações.

---

## Histórico de Versão

| Versão | Data       | Descrição                                | Autor(es)                                          |
| :----: | :--------- | :--------------------------------------- | :------------------------------------------------- |
|  1.0   | 2026-06-23 | Elaboração do julgamento final da Fase 4 | [Edilson Ribeiro](https://github.com/Edilson-r-jr) |
