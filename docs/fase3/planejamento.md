# Planejamento da Avaliação

A Fase 3 corresponde à etapa de **projetar a avaliação** dentro do Processo de Avaliação de Produto de Software (ISO/IEC 25040). Tendo como entrada a especificação da medição produzida na Fase 2 — os objetivos, questões e métricas definidos pelo método **Goal/Question/Metric (GQM)** para as características de **Confiabilidade** e **Manutenibilidade** — esta fase organiza *como* essas medições serão efetivamente obtidas na Fase 4.

Conforme Solingen e Berghout (1999), o principal artefato da etapa de planejamento de um programa de medição GQM é o **plano de avaliação**, que documenta os procedimentos, os recursos e o cronograma necessários para que a coleta seja executada de forma padronizada, repetível e auditável. É exatamente esse o escopo das seções a seguir.

!!! info "Rastreabilidade entre as fases"
    Este plano não introduz novas métricas. Cada elemento descrito aqui deriva diretamente das métricas e dos critérios de julgamento de **Confiabilidade** e **Manutenibilidade** estabelecidos na Fase 2 e do propósito declarado na Fase 1. A relação explícita é apresentada na [Seção 5 — Consistência entre as Fases 2 e 3](#5-consistencia-entre-as-fases-2-e-3).

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

A avaliação adota uma abordagem **mista**: medições **automáticas/estáticas** sobre o código-fonte (cobertura de testes, acoplamento, tratamento de exceções, logs) combinadas com uma medição **dinâmica e controlada** com usuários (MTBF) e uma medição **de processo** sobre o histórico do repositório (tempo médio de alteração). Essa combinação acompanha a recomendação de Solingen e Berghout (1999) de só recorrer a ferramentas automatizadas quando elas respondem objetivamente à questão, mantendo a coleta manual onde o dado mais valioso vem da observação humana.

---

## 2 - Método de Avaliação

Para cada métrica da Fase 2 são definidos: a **fonte de evidência**, a **ferramenta/instrumento** e as **instruções passo a passo** para o avaliador. As instruções foram escritas de modo a garantir **repetibilidade** (o mesmo avaliador obtém o mesmo resultado) e **reprodutibilidade** (avaliadores diferentes obtêm resultados equivalentes), conforme exigido em processos formais de medição.

!!! note "Sobre o nível de detalhe das ferramentas"
    O **método** de cada coleta segue fielmente os passos definidos na Fase 2 e é independente de ferramenta. As ferramentas citadas são de três tipos: as **já presentes no projeto** (verificadas no repositório `unb-mds/2023-2-SuaGradeUnB` — ex.: o *test runner* do Django com `coverage`/Codecov, e a stack Django 5 + DRF + `requests`/`beautifulsoup4`); as de **apoio opcional** (ex.: `grep`/AST e `pydeps`), que apenas agilizam a inspeção e podem ser substituídas por verificação manual; e a **adição planejada** do **Sentry** com a config `LOGGING` do Django, necessária ao M2.1 (Manutenibilidade), uma vez que o backend ainda não possui instrumentação de logs.

### 2.1 - Tabela GQM do método de avaliação

A tabela abaixo consolida o desdobramento GQM, ligando cada **dimensão** (característica) às suas **questões** e **métricas** da Fase 2 e associando, para cada métrica, a **fonte de dados** e o **plano de coleta**. As instruções detalhadas de cada plano estão nas Seções 2.2 e 2.3.

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: left; vertical-align: top;">
    <tr>
      <th><b>Dimensão</b></th>
      <th><b>Questão</b></th>
      <th><b>Métrica</b></th>
      <th><b>Fonte de dados</b></th>
      <th><b>Plano de Coleta</b></th>
    </tr>
    <tr>
      <td rowspan="2"><b>Confiabilidade</b></td>
      <td>Qual é o tempo médio entre falhas apresentado pelo sistema durante o seu uso em operação normal?</td>
      <td>M1.1: Tempo Médio Entre Falhas (MTBF)</td>
      <td>Instância em execução e relato dos usuários durante a sessão controlada</td>
      <td>Cronometrar uma sessão de uso controlada com usuários finais e contar as falhas observadas. Resultado em <b>problemas/hora = nº de falhas ÷ tempo (horas)</b> (detalhes na Seção 2.2)</td>
    </tr>
    <tr>
      <td>O quanto o sistema consegue sinalizar a ocorrência de falhas?</td>
      <td>M2.1: Percentual de operações críticas tratadas com exceções</td>
      <td>Código-fonte do projeto (backend e frontend)</td>
      <td>Definir a lista de operações críticas (acesso ao banco, scraping do SIGAA, autenticação, manipulação de arquivos). No código-fonte, contar o total dessas operações e quantas estão dentro de um bloco <code>try</code>. Percentual = (operações com <code>try</code> ÷ total) × 100 (detalhes na Seção 2.2)</td>
    </tr>
    <tr>
      <td rowspan="4"><b>Manutenibilidade</b></td>
      <td>O quão forte é a relação entre os componentes do sistema?</td>
      <td>M1.1: Nível de independência dos componentes</td>
      <td>Classes/módulos do sistema e suas dependências</td>
      <td>Levantar as classes/módulos do sistema; identificar as dependências entre eles; classificar como independente o que não depende de outros; aplicar a fórmula (Seção 2.3)</td>
    </tr>
    <tr>
      <td>O quão fácil é identificar a operação específica que causou a falha?</td>
      <td>M2.1: Nível de qualidade dos logs do sistema</td>
      <td>Chamadas de log das operações críticas, centralizadas no Sentry</td>
      <td>Mapear as operações críticas; instrumentar com a config <code>LOGGING</code> do Django + Sentry; verificar no painel do Sentry (ou nas chamadas de log no código) quantas operações registram dados; aplicar a fórmula (Seção 2.3)</td>
    </tr>
    <tr>
      <td>O quão facilmente o mantenedor consegue modificar o software para resolver um problema?</td>
      <td>M3.1: Média do tempo gasto para fazer uma alteração</td>
      <td>Quadro de <i>issues</i> do projeto (GitHub Projects)</td>
      <td>Escolher um período de coleta; medir o tempo entre cada <i>issue</i> passar de "Em Andamento" para "Concluída"; aplicar a média (Seção 2.3)</td>
    </tr>
    <tr>
      <td>O quão completa é a cobertura de testes do sistema?</td>
      <td>M4.1: Porcentagem de cobertura de testes</td>
      <td>Suíte de testes automatizados e especificações do projeto</td>
      <td>Rodar a suíte de testes para contar quantos casos de teste foram criados; levantar nas especificações quantos cenários deveriam existir; aplicar a fórmula (Seção 2.3)</td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 2: Tabela GQM — método de coleta por dimensão, questão e métrica.</figcaption></font>
  </div>
</div>

### 2.2 - Instruções de coleta — Confiabilidade

**M1.1 — Tempo Médio Entre Falhas (MTBF)**

1. Preparar um **roteiro de tarefas** representativas do uso normal: (a) buscar disciplinas por nome de matéria e por professor; (b) selecionar turmas e montar uma grade; (c) gerar a grade horária; (d) exportar a grade em PDF; (e) autenticar com conta Google.
2. Recrutar de 3 a 5 usuários finais (estudantes da UnB) e conduzir uma **sessão de uso controlada** com a instância em produção (`https://suagradeunb.com.br/`).
3. Iniciar um cronômetro no começo da sessão e registrar o **tempo total decorrido em horas**.
4. A cada falha ou comportamento inesperado relatado, registrar a ocorrência no **formulário de registro de falhas** (descrição, tarefa, horário, avaliador responsável) e somar +1 ao total de falhas da sessão.
5. Ao final, calcular a **taxa de falhas** = `(número de falhas) / (tempo da sessão em horas)`, em **problemas/hora**, e comparar com a pontuação de julgamento da Fase 2.

!!! warning "Alinhar com a Fase 2"
    A escala de julgamento da Fase 2 está em **problemas/hora**, portanto a taxa de falhas (`falhas ÷ horas`) é a forma diretamente comparável e robusta ao caso de zero falhas. A fórmula textual da Fase 2 ainda está como `horas ÷ falhas` (MTBF), que é o inverso e indefinido quando não há falhas — recomenda-se uniformizar a Fase 2 para `falhas ÷ horas`.

!!! note "Instrumento de coleta"
    O formulário de registro de falhas segue o modelo de *manual data collection form* de Solingen e Berghout (1999): uma planilha de página única, com campo identificador do avaliador, que torna o dado bruto verificável e rastreável. O modelo em branco e a planilha preenchida serão versionados no repositório.

**M2.1 — Percentual de operações críticas com tratamento de exceção**

1. Fixar a lista de **operações críticas**: conexão/consulta ao banco de dados, manipulação de arquivos, manipulação dos dados de *web scraping* do SIGAA, e autenticação/autorização de usuários.
2. Varrer todo o código-fonte com busca assistida (`grep`/análise de AST com o módulo `ast` do Python) para **contar todas as ocorrências** dessas operações.
3. Classificar manualmente quais ocorrências estão **sintaticamente dentro de um bloco `try { ... }`** (ou `try/except`), descartando falsos positivos.
4. Calcular `(operações críticas com try-catch / total de operações críticas) × 100`.
5. Registrar, para cada ocorrência, o arquivo e a linha, garantindo a rastreabilidade entre o dado bruto e a métrica.

### 2.3 - Instruções de coleta — Manutenibilidade

**M1.1 — Nível de independência dos componentes**

1. Levantar as **classes/módulos** do sistema (apps do backend Django e módulos do frontend Next.js).
2. **Identificar as dependências** entre eles (análise de *imports*; opcionalmente com apoio de `pydeps` no backend para visualizar o grafo).
3. Classificar como **independente** o componente que não depende de outro componente interno do sistema.
4. Calcular `(componentes independentes / total de componentes)` e comparar com a faixa de julgamento.

**M2.1 — Nível de qualidade dos logs**

> Hoje o backend **não possui instrumentação de logs**; a medição pressupõe ativar o `logging` do Django e centralizar os registros no **Sentry** — adição de baixo esforço e diretamente alinhada ao propósito da Analisabilidade (diagnosticar a operação que causou a falha).

1. A partir da lista de operações críticas (mesma do M2.1 de Confiabilidade), montar um **checklist do que deveria ser registrado** para monitorar o status do sistema.
2. Instrumentar o backend com a configuração `LOGGING` do Django e integrar ao **Sentry** (`sentry-sdk[django]`); no frontend, usar `@sentry/nextjs`.
3. Verificar — no painel do Sentry ou inspecionando as chamadas de log no código — quantas operações críticas **efetivamente registram dados**.
4. Calcular `(dados efetivamente registrados / dados planejados para registro)`.

**M3.1 — Tempo médio para realizar uma alteração**

1. Definir o período de coleta (ex.: as *issues* trabalhadas no último marco do projeto).
2. Para cada *issue*, medir o **tempo de ciclo** entre a transição "Em Andamento" → "Concluída" no GitHub Projects, cruzando com as datas dos *commits* (`git log`).
3. Calcular `(soma dos tempos de alteração / número de alterações)` em horas.

**M4.1 — Cobertura de testes**

> A métrica conta **casos de teste automatizados** em relação aos cenários que deveriam existir — não é cobertura de linhas de código.

1. Executar a suíte de testes do **backend** com `coverage run manage.py test` — o *test runner* do Django, já configurado no projeto (`coverage 7.3.2`) e na CI via Codecov — e **contar os casos de teste automatizados** existentes.
2. Verificar a existência de testes automatizados no **frontend** (`web/`) e registrar a cobertura efetiva encontrada.
3. Levantar, nas **especificações/histórias do projeto**, quantos cenários de teste **deveriam** existir.
4. Calcular `(casos de teste automatizados escritos / cenários que deveriam existir)` e comparar com a faixa de julgamento.

!!! warning "Evidências obrigatórias"
    Toda execução de ferramenta deve ser acompanhada de evidência (saída de terminal, relatório HTML de cobertura ou *screenshot*) armazenada no repositório, permitindo a verificação externa exigida na Fase 4.

---

## 3 - Recursos e Ambiente de Avaliação

### 3.1 - Ambiente de execução

O *Sua Grade UnB* é distribuído via Docker, o que permite reproduzir o ambiente de avaliação de forma idêntica entre os avaliadores. São requisitos para executar e analisar o produto:

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: left;">
    <tr>
      <th><b>Categoria</b></th>
      <th><b>Recurso</b></th>
      <th><b>Finalidade</b></th>
    </tr>
    <tr>
      <td rowspan="2"><b>Hardware</b></td>
      <td>Máquina com, no mínimo, 8 GB de RAM e acesso à internet</td>
      <td>Subir os contêineres e executar as suítes de teste</td>
    </tr>
    <tr>
      <td>Dispositivos variados (desktop e celular)</td>
      <td>Sessão de uso controlada do MTBF (abordagem <i>Mobile First</i> do produto)</td>
    </tr>
    <tr>
      <td rowspan="4"><b>Software (execução)</b></td>
      <td>Docker e Docker Compose</td>
      <td>Reproduzir o ambiente do backend e do banco PostgreSQL</td>
    </tr>
    <tr>
      <td>Python 3 + Django / Node.js</td>
      <td>Executar backend e frontend localmente</td>
    </tr>
    <tr>
      <td>Git e acesso ao GitHub (repositório e Projects)</td>
      <td>Coleta de M3.1 (histórico) e versionamento dos dados brutos</td>
    </tr>
    <tr>
      <td>Navegador web moderno</td>
      <td>Acesso à instância em produção na sessão de uso</td>
    </tr>
    <tr>
      <td rowspan="5"><b>Software (medição)</b></td>
      <td>Test runner do Django (<code>coverage run manage.py test</code>) + Codecov</td>
      <td>M4.1 — execução e contagem dos casos de teste (backend)</td>
    </tr>
    <tr>
      <td>Sentry (<code>sentry-sdk[django]</code> / <code>@sentry/nextjs</code>) + config <code>LOGGING</code> do Django</td>
      <td>M2.1 (Manutenibilidade) — instrumentação e centralização dos logs</td>
    </tr>
    <tr>
      <td>Análise de <i>imports</i> (apoio opcional: <code>pydeps</code>)</td>
      <td>M1.1 — Independência dos componentes</td>
    </tr>
    <tr>
      <td><code>grep</code> e módulo <code>ast</code> do Python (apoio à inspeção)</td>
      <td>M2.1 (Confiabilidade) — contagem de operações com <code>try</code></td>
    </tr>
    <tr>
      <td>Planilha (Google Sheets / LibreOffice Calc)</td>
      <td>Formulários de coleta e consolidação dos dados brutos</td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 3: Recursos de hardware e software para a avaliação.</figcaption></font>
  </div>
</div>

### 3.2 - Massa de dados

- **MTBF (M1.1):** a sessão de uso depende de um **conjunto representativo de consultas** (nomes de disciplinas e professores reais ofertados pela UnB). Como os dados são obtidos por *web scraping* periódico do SIGAA, a própria base em produção fornece a massa necessária; o roteiro de tarefas fixa as consultas a serem realizadas para garantir comparabilidade entre os usuários.
- **Cobertura de testes (M4.1):** utiliza a suíte de testes já existente no repositório como massa de entrada, não exigindo geração de dados adicionais.

### 3.3 - Conhecimento exigido dos avaliadores

- **Domínio da aplicação:** conhecimento do processo de montagem de grade horária e matrícula na UnB — pré-requisito para conduzir a sessão de MTBF e interpretar falhas.
- **Conhecimento técnico:** noções de Python/Django e TypeScript/Next.js e familiaridade com Git, Docker e com as ferramentas de medição, necessárias para as métricas estáticas e de processo.
- **Nível de informática dos usuários da sessão:** usuários finais (estudantes) com conhecimento básico de navegação web, refletindo o público-alvo real do produto.

---

## 4 - Cronograma de Avaliação

O cronograma abaixo organiza a execução da Fase 4, distribuindo as atividades entre os integrantes e permitindo a coleta **em paralelo** por característica. As métricas estáticas e de processo independem da sessão com usuários, podendo ocorrer simultaneamente.

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
      <td>Preparação do ambiente (Docker, suítes de teste) e dos instrumentos (roteiro e formulários)</td>
      <td>Todas</td>
      <td>Equipe</td>
      <td>Dias 1–2</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Coleta das métricas estáticas de código</td>
      <td>Conf. M2.1; Manut. M1.1 e M2.1</td>
      <td>Dupla "Código"</td>
      <td>Dias 3–5</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Coleta da métrica de processo (histórico de issues)</td>
      <td>Manut. M3.1</td>
      <td>Responsável "Processo"</td>
      <td>Dias 3–4</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Execução das suítes e coleta de cobertura</td>
      <td>Manut. M4.1</td>
      <td>Responsável "Testes"</td>
      <td>Dias 3–4</td>
    </tr>
    <tr>
      <td>5</td>
      <td>Sessão de uso controlada com usuários finais</td>
      <td>Conf. M1.1 (MTBF)</td>
      <td>Dupla "Usuários"</td>
      <td>Dias 5–6</td>
    </tr>
    <tr>
      <td>6</td>
      <td>Consolidação dos dados brutos no repositório e revisão</td>
      <td>Todas</td>
      <td>Equipe</td>
      <td>Dias 7–8</td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 4: Cronograma de execução da avaliação (Fase 4).</figcaption></font>
  </div>
</div>

!!! note
    A divisão por "duplas/responsáveis" será detalhada na Tabela de Contribuição da equipe. Os períodos são relativos ao início da Fase 4 e serão ajustados às datas oficiais definidas no Moodle.

---

## Bibliografia

- ISO/IEC 25040:2011 — *Systems and software engineering — Systems and software Quality Requirements and Evaluation (SQuaRE) — Evaluation process*. Genebra: ISO, 2011.
- ISO/IEC 25010:2011 — *Systems and software engineering — Systems and software Quality Requirements and Evaluation (SQuaRE) — System and software quality models*. Genebra: ISO, 2011.
- SOLINGEN, Rini van; BERGHOUT, Egon. **The Goal/Question/Metric Method: a practical guide for quality improvement of software development**. Londres: McGraw-Hill, 1999.
- BASILI, Victor R.; CALDIERA, Gianluca; ROMBACH, H. Dieter. The Goal Question Metric Approach. In: *Encyclopedia of Software Engineering*. Nova York: John Wiley & Sons, 1994.
- RAMOS, Cristiane Soares. **Processo de Avaliação de Produto de Software**. Slides da disciplina Qualidade de Software 1, Faculdade de Ciências e Tecnologias em Engenharia (FCTE), Universidade de Brasília, 2025/2.

## Histórico de Versão

| Versão | Data       | Descrição                          | Autor(es) |
|:------:|:-----------|:-----------------------------------|:----------|
| 1.0    | 2026-06-06 | Elaboração do plano de avaliação (método, recursos, cronograma e consistência) | Caio Felipe e Gabriel Magioli |
