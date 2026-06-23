# Planejamento da Avaliação

Avaliar a **Confiabilidade** e a **Manutenibilidade** do sistema *Sua Grade UnB*, com base no plano de medição GQM (Goal-Question-Metric) definido na Fase 2. O objetivo é executar a coleta de dados de forma sistemática para identificar pontos de vulnerabilidade, débito técnico e riscos operacionais, validando as hipóteses formuladas.

Esta fase organiza *como* as medições especificadas na Fase 2 serão obtidas na prática, documentando o método, os recursos e o cronograma necessários para que a coleta seja feita de forma padronizada e repetível na Fase 4.

---

## 1 - Plano de Avaliação

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: left;">
    <tr>
      <th><b>Objetivo do plano</b></th>
      <td>Definir o método, os recursos, o ambiente e o cronograma para a coleta das medidas de Confiabilidade e Manutenibilidade do <i>Sua Grade UnB</i>.</td>
    </tr>
    <tr>
      <th><b>Objeto de avaliação</b></th>
      <td>Código-fonte, documentação e instância em execução do <i>Sua Grade UnB</i> (repositório <a href="https://github.com/unb-mds/2023-2-SuaGradeUnB">unb-mds/2023-2-SuaGradeUnB</a>).</td>
    </tr>
    <tr>
      <th><b>Características avaliadas</b></th>
      <td>Confiabilidade (Maturidade e Tolerância a falhas) e Manutenibilidade (Modularidade, Analisabilidade, Modificabilidade e Testabilidade).</td>
    </tr>
    <tr>
      <th><b>Avaliadores</b></th>
      <td>Equipe do projeto (disciplina de Qualidade de Software 1 — FCTE/UnB).</td>
    </tr>
    <tr>
      <th><b>Saída esperada</b></th>
      <td>Dados brutos e consolidados, armazenados no repositório, prontos para a análise e o julgamento da Fase 4.</td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 1: Visão geral do plano de avaliação.</figcaption></font>
  </div>
</div>

### Método de Avaliação

Será utilizada uma abordagem mista (quantitativa e qualitativa) baseada no plano GQM. A coleta de dados combina ferramentas de análise estática de código, análise do histórico do repositório e inspeção manual de código para as métricas qualitativas. As medições automáticas garantem objetividade onde o dado é numérico (cobertura de testes, acoplamento, tratamento de exceções, logs), enquanto uma sessão de uso controlada com usuários cobre o que só a observação direta revela.

Essa sessão de uso serve para medir o **MTBF** (*Mean Time Between Failures*, ou Tempo Médio Entre Falhas), um indicador de confiabilidade que expressa quanto tempo, em média, o sistema opera sob uso normal antes de apresentar uma falha. Quanto maior o MTBF, mais estável é o sistema. Esse é o foco da métrica M1.1 de Confiabilidade, detalhada na Seção 2.2.

---

## 2 - Recursos Necessários e Métodos de Avaliação

### 2.1 - Recursos Necessários para a Avaliação

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: left; vertical-align: top;">
    <tr>
      <th><b>Categoria</b></th>
      <th><b>Recurso</b></th>
      <th><b>Finalidade</b></th>
    </tr>
    <tr>
      <td rowspan="2"><b>Ambiente</b></td>
      <td>Repositório local do <i>Sua Grade UnB</i> (código-fonte clonado)</td>
      <td>Base para todas as métricas de análise estática de código (M2.1 — Conf.; M1.1, M2.1 e M4.1 — Manut.)</td>
    </tr>
    <tr>
      <td>Instância em produção (<code>suagradeunb.com.br</code>)</td>
      <td>Ambiente da sessão controlada de uso para medição do MTBF (M1.1 — Conf.)</td>
    </tr>
    <tr>
      <td rowspan="5"><b>Ferramentas</b></td>
      <td><code>grep</code> e módulo <code>ast</code> do Python</td>
      <td>Varredura e contagem de operações críticas cobertas por <code>try/except</code> no backend (M2.1 — Conf.)</td>
    </tr>
    <tr>
      <td><code>pydeps</code></td>
      <td>Geração do grafo de dependências entre módulos Python para análise de acoplamento (M1.1 — Manut.)</td>
    </tr>
    <tr>
      <td><code>coverage</code> + <code>manage.py test</code> (Django test runner)</td>
      <td>Execução da suíte de testes automatizados e contagem de casos de teste do backend (M4.1 — Manut.)</td>
    </tr>
    <tr>
      <td><code>git log</code></td>
      <td>Extração de datas de commits para cruzamento com histórico de <i>issues</i> (M3.1 — Manut.)</td>
    </tr>
    <tr>
      <td>Sentry (<code>sentry-sdk[django]</code> / <code>@sentry/nextjs</code>)</td>
      <td>Verificação e centralização dos registros de log das operações críticas (M2.1 — Manut.)</td>
    </tr>
    <tr>
      <td><b>Plataformas</b></td>
      <td>GitHub Projects</td>
      <td>Coleta de dados de transição de estado das <i>issues</i> para cálculo do tempo de ciclo (M3.1 — Manut.)</td>
    </tr>
    <tr>
      <td><b>Instrumento de coleta</b></td>
      <td>Planilha</td>
      <td>Formulários de coleta e consolidação dos dados brutos de todas as métricas</td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 2: Recursos necessários para a avaliação.</figcaption></font>
  </div>
</div>

### 2.2 - Instruções de coleta — Confiabilidade

**M1.1 — Tempo Médio Entre Falhas (MTBF)**

1. Preparar um **roteiro de tarefas** representativas do uso normal: (a) buscar disciplinas por nome de matéria e por professor; (b) selecionar turmas e montar uma grade; (c) gerar a grade horária; (d) exportar a grade em PDF; (e) autenticar com conta Google.
2. Acessar a instância em produção (`https://suagradeunb.com.br/`) e executar cada fluxo do roteiro, iniciando um cronômetro no início da execução e registrando o **tempo total decorrido em horas**.
3. A cada falha ou comportamento inesperado observado, registrar a ocorrência no **formulário de registro de falhas** (descrição, fluxo, horário, avaliador responsável) e somar +1 ao total de falhas.
4. Ao final, calcular a **taxa de falhas** = `(número de falhas) / (tempo da sessão em horas)`, em **problemas/hora**, e comparar com a pontuação de julgamento da Fase 2.

**M2.1 — Percentual de operações críticas com tratamento de exceção**

1. Fixar a lista de **operações críticas**: conexão/consulta ao banco de dados, manipulação de arquivos, manipulação dos dados de *web scraping* do SIGAA, e autenticação/autorização de usuários.
2. Varrer o código-fonte do backend com `grep` e análise de AST via módulo `ast` do Python para **contar todas as ocorrências** dessas operações; no frontend TypeScript, aplicar `grep` de forma equivalente.
3. Classificar manualmente quais ocorrências estão **dentro de um bloco de tratamento de exceção** — `try/except` no backend Python ou `try/catch` no frontend TypeScript — descartando falsos positivos.
4. Calcular `(operações críticas com tratamento de exceção / total de operações críticas) × 100`.
5. Registrar, para cada ocorrência, o arquivo e a linha, garantindo a rastreabilidade entre o dado bruto e a métrica.

### 2.3 - Instruções de coleta — Manutenibilidade

**M1.1 — Nível de independência dos componentes**

1. Levantar as **classes/módulos** do sistema (apps do backend Django e módulos do frontend Next.js).
2. **Identificar as dependências** entre eles por análise dos `import`s; utilizar `pydeps` no backend para gerar o grafo de dependências e apoiar a classificação visual.
3. Classificar como **independente** o componente que não depende de outro componente interno do sistema.
4. Calcular `(componentes independentes / total de componentes)` e comparar com a faixa de julgamento.

**M2.1 — Nível de qualidade dos logs**

1. A partir da lista de operações críticas (mesma do M2.1 de Confiabilidade), montar um **checklist do que deveria ser registrado** para monitorar o status do sistema.
2. Verificar no código-fonte do backend — pelas chamadas de `logger.*` (configuração `LOGGING` do Django) ou pelas integrações com o Sentry — e no frontend — pelas integrações com `@sentry/nextjs` — quantas operações críticas **efetivamente emitem registros**.
3. Calcular `(operações críticas com registro de log / total de operações críticas planejadas para registro)`.

**M3.1 — Tempo médio para realizar uma alteração**

1. Adotar uma **abordagem longitudinal** para o período de coleta: selecionar amostras de *issues* distribuídas ao longo de diferentes fases do ciclo de vida do software, evitando o viés de complexidade de um único sprint isolado.
2. Para cada *issue*, medir o **tempo de ciclo** entre a transição "Em Andamento" → "Concluída" no GitHub Projects; cruzar com as datas dos *commits* relacionados via `git log` para validar a data efetiva de conclusão do código.
3. Calcular `(soma dos tempos de alteração / número de alterações)` em horas.

**M4.1 — Cobertura de testes**

> A métrica conta **casos de teste automatizados** em relação aos cenários que deveriam existir — não é cobertura de linhas de código.

1. Executar a suíte de testes do **backend** com `coverage run manage.py test` — o *test runner* do Django, já configurado no projeto (`coverage 7.3.2`) e na CI via Codecov — e **contar os casos de teste automatizados** existentes.
2. Verificar a existência de testes automatizados no **frontend** (`web/`) e registrar a cobertura efetiva encontrada.
3. Levantar, nas **especificações/histórias do projeto**, quantos cenários de teste **deveriam** existir.
4. Calcular `(casos de teste automatizados escritos / cenários que deveriam existir)` e comparar com a faixa de julgamento.

---

## 3 - Recursos e Ambiente de Avaliação

### 3.1 - Ambiente de execução

O *Sua Grade UnB* é distribuído via Docker, o que permite reproduzir o ambiente de avaliação de forma idêntica entre os avaliadores.

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: left;">
    <tr>
      <th><b>Recurso</b></th>
      <th><b>Descrição</b></th>
    </tr>
    <tr>
      <td><b>Hardware</b></td>
      <td>Máquina com no mínimo 8 GB de RAM e acesso à internet (contêineres e suítes de teste); dispositivos desktop e celular para a sessão de MTBF (abordagem <i>Mobile First</i>).</td>
    </tr>
    <tr>
      <td><b>Ambiente</b></td>
      <td>Docker e Docker Compose (backend + PostgreSQL), Python 3 + Django e Node.js (execução local), Git/GitHub (histórico e versionamento) e navegador web moderno (instância em produção).</td>
    </tr>
    <tr>
      <td><b>Ferramentas</b></td>
      <td>As já listadas na Tabela 2 — <code>coverage</code> + Django test runner, Sentry, <code>pydeps</code>, <code>grep</code>/<code>ast</code> e planilha — usadas conforme a coleta de cada métrica.</td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 3: Recursos e ambiente de execução.</figcaption></font>
  </div>
</div>

---

## 4 - Cronograma de Avaliação

O cronograma abaixo organiza a execução da Fase 4, distribuindo as atividades entre os integrantes e permitindo a coleta **em paralelo** por característica. As métricas estáticas e de processo são independentes entre si e podem ser conduzidas simultaneamente.

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: left;">
    <tr>
      <th><b>Etapa</b></th>
      <th><b>Atividade</b></th>
      <th><b>Métricas</b></th>
      <th><b>Responsáveis</b></th>
      <th><b>Período</b></th>
    </tr>
    <tr>
      <td>1</td>
      <td>Preparação do ambiente (clone do repositório, suítes de teste) e dos instrumentos (formulários)</td>
      <td>Todas</td>
      <td>Responsável: Equipe</td>
      <td>Dias 1–2</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Coleta das métricas estáticas de código</td>
      <td>Conf. M2.1; Manut. M1.1 e M2.1</td>
      <td>Responsável: Anne de Capdeville e Edilson Júnior</td>
      <td>Dias 1–3</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Coleta da métrica de processo (histórico de <i>issues</i>)</td>
      <td>Manut. M3.1</td>
      <td>Responsável: Anne de Capdeville </td>
      <td>Dias 1–2</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Execução das suítes e coleta de cobertura de testes</td>
      <td>Manut. M4.1</td>
      <td>Responsável: Anne de Capdeville </td>
      <td>Dias 1–3</td>
    </tr>
    <tr>
      <td>5</td>
      <td>Consolidação dos dados brutos no repositório e revisão</td>
      <td>Todas</td>
      <td>Responsável: Equipe</td>
      <td>Dias 4–5</td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 4: Cronograma de execução da avaliação (Fase 4).</figcaption></font>
  </div>
</div>

---

## Histórico de Versão

| Versão | Data       | Descrição                          | Autor(es) |
|:------:|:-----------|:-----------------------------------|:----------|
| 1.0    | 2026-06-06 | Elaboração do plano de avaliação (método, recursos, cronograma e consistência) | Caio Felipe e Gabriel Magioli |
