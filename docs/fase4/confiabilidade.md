# Avaliação — Confiabilidade

Esta página apresenta os resultados da **Fase 4 — Execução da Avaliação** referentes à característica de qualidade **Confiabilidade**, avaliada segundo o modelo de qualidade SQuaRE e a norma ISO/IEC 25010:2011. As métricas executadas nesta fase — M1.1 (Tempo Médio Entre Falhas) e M2.1 (Percentual de operações críticas com tratamento de exceção) — foram planejadas na Fase 3, disponível em [Fase 3 - planejamento](planejamento.md), e seguem o método, os recursos e o roteiro lá definidos.

## Propósito e Contexto GQM

De acordo com o propósito estabelecido na **Fase 1**, esta avaliação tem como objetivo determinar se o _Sua Grade UnB_ apresenta nível de Confiabilidade adequado para uso pelos estudantes da UnB, identificando pontos de vulnerabilidade e riscos operacionais que possam comprometer a experiência do usuário final e a robustez do código em produção.

No plano GQM adotado, a característica Confiabilidade é decomposta da seguinte forma:

- **Objetivo (G):** Avaliar o nível de estabilidade e robustez do _Sua Grade UnB_ sob a perspectiva dos usuários finais e da qualidade interna do código.
- **Questão 1 (Q1 — Maturidade):** O sistema apresenta falhas com frequência aceitável durante o uso normal pelos usuários finais?
  - Métrica de resposta: **M1.1 — MTBF**
- **Questão 2 (Q2 — Tolerância a falhas):** O código do sistema está adequadamente preparado para tolerar erros em tempo de execução?
  - Métrica de resposta: **M2.1 — Percentual de operações críticas com tratamento de exceção**

Os resultados obtidos na Fase 4 respondem a cada uma dessas questões, conforme detalhado nas seções seguintes.

---

## 1. Métrica M1.1 — Tempo Médio Entre Falhas (MTBF)

### 1.1 Descrição da medição

A medição da métrica M1.1 foi realizada por meio de uma sessão de uso controlada na instância em produção do _Sua Grade UnB_ (`https://suagradeunb.com.br/`), seguindo o roteiro de tarefas representativo do uso normal definido na Fase 3. A sessão teve duração total de **5min12s**, durante a qual foram executadas as seguintes atividades:

```markdown
### Roteiro de tarefas

- Autenticar via Google
- Criar duas grades da seguinte forma:
- Buscar até 6 disciplinas, 3 por nome da disciplina e 3 por professor
- Gerar as grades em PDF e salvar
- Deletar uma grade da nuvem
- Visualizar grade salva na aba
- Trocar de conta
```

### 1.2 Falhas observadas

Durante a execução do roteiro, foram registradas **2 falhas**, conforme detalhado a seguir.

> O vídeo abaixo registra a sessão de uso controlada na íntegra. Os timestamps das falhas observadas estão marcados na Tabela 1.

<div align="center">
  <iframe width="560" height="315" src="https://www.youtube.com/embed/4qz93pTRBQg" title="Sessão de uso controlada — Métrica M1.1 MTBF (Confiabilidade)" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Vídeo 1 — Sessão de uso controlada para medição do MTBF (M1.1 — Confiabilidade). As falhas observadas estão identificadas em 1min30s (Falha 1 — bug visual na barra de navegação) e 4min30s (Falha 2 — bug de autenticação Google).</figcaption></font>
  </div>
</div>

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: left;">
    <tr>
      <th><b>#</b></th>
      <th><b>Timestamp</b></th>
      <th><b>Descrição da falha</b></th>
      <th><b>Fluxo afetado</b></th>
      <th><b>Impacto</b></th>
    </tr>
    <tr>
      <td>1</td>
      <td>1min30s</td>
      <td>Bug visual na barra de navegação inferior: o ícone da seção ativa fica preso entre as duas opções de navegação, sem se posicionar corretamente sobre a opção selecionada.</td>
      <td>Navegação geral do aplicativo</td>
      <td>Muito leve — não impede o uso, apenas afeta a apresentação visual.</td>
    </tr>
    <tr>
      <td>2</td>
      <td>4min30s</td>
      <td>Bug de autenticação: ao autenticar com uma conta Google e, em seguida, tentar trocar de conta, o usuário é redirecionado imediatamente para a página inicial do aplicativo, sem que a tela de seleção de conta do Google seja exibida.</td>
      <td>Autenticação / Troca de conta Google</td>
      <td>Médio — impede a conclusão da tarefa de troca de conta, exigindo contorno (ex.: logout manual ou limpeza de sessão).</td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 4.1 — Falhas observadas durante a sessão de uso.</figcaption></font>
  </div>
</div>

#### 1.2.1 Limitação de design identificada

Além das falhas observadas durante a sessão, identificou-se uma **limitação inerente ao approach de raspagem de dados** (_web scraping_) adotado pela equipe no desenvolvimento do _Sua Grade UnB_: quando o SIGAA não possui dados de matrícula disponíveis — seja por período fora de matrícula, ausência de oferta de disciplinas ou indisponibilidade temporária da fonte , o sistema retorna uma listagem vazia **sem exibir nenhuma notificação ao usuário** explicando a ausência de resultados.

Essa ausência de _feedback_ compromete diretamente a principal função do produto: o usuário não consegue distinguir se a lista vazia é consequência de uma limitação de dados na fonte ou de um problema no próprio sistema. Por ser uma consequência direta da estratégia de coleta de dados escolhida — e não um defeito de implementação isolado —, essa limitação requer uma **decisão de design** para ser endereçada, como a adição de uma validação que detecte respostas vazias do SIGAA e exiba ao usuário uma mensagem explicativa adequada (ex.: _"Nenhuma disciplina encontrada. Os dados do SIGAA podem estar indisponíveis ou fora do período de matrícula."_).

### 1.3 Resultado consolidado

A métrica M1.1 foi calculada com base na fórmula definida na Fase 2 — **MTBF = (tempo da sessão de uso em minutos) / (quantidade de falhas identificadas)** —, convertendo a duração da sessão (5min12s) para minutos decimais:

```
Duração da sessão = 5min12s = 5 + (12/60) = 5,2 minutos

MTBF = (tempo da sessão em minutos) / (número de falhas)
     = 5,2 / 2
     = 2,6 minutos
```

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: left;">
    <tr>
      <th><b>Indicador</b></th>
      <th><b>Valor</b></th>
    </tr>
    <tr>
      <td>Duração da sessão</td>
      <td>5min12s (≈ 5,2 minutos)</td>
    </tr>
    <tr>
      <td>Total de falhas observadas</td>
      <td>2</td>
    </tr>
    <tr>
      <td>Timestamp da Falha 1</td>
      <td>1min30s</td>
    </tr>
    <tr>
      <td>Timestamp da Falha 2</td>
      <td>4min30s</td>
    </tr>
    <tr>
      <td><b>M1.1 — MTBF</b></td>
      <td><b>2,6 minutos</b></td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 4.2 — Resultado consolidado da métrica M1.1.</figcaption></font>
  </div>
</div>

### 1.4 Julgamento da métrica M1.1

A Fase 2 estabelece a seguinte faixa de pontuação para a métrica M1.1:

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: left;">
    <tr>
      <th><b>Excelente</b></th>
      <th><b>Bom</b></th>
      <th><b>Regular</b></th>
      <th><b>Insatisfatório</b></th>
    </tr>
    <tr>
      <td>MTBF > 10 minutos ou nenhuma falha detectada</td>
      <td>MTBF entre 4 e 10 minutos</td>
      <td>MTBF entre 1 e 4 minutos</td>
      <td>MTBF < 1 hora</td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 4.3 — Pontuação de julgamento da métrica M1.1 (Fase 2).</figcaption></font>
  </div>
</div>

Com o valor obtido de **MTBF = 2,6 minutos**, dentro do intervalo de 1 a 4 minutos, a métrica M1.1 é classificada como **Regular**. O resultado indica que, sob uso normal, o sistema apresentou falhas em uma frequência que compromete parcialmente a estabilidade percebida pelos usuários finais, embora não no nível mais crítico da escala.

### 1.5 Resposta à Questão GQM (Q1)

**Q1: O sistema apresenta falhas com frequência aceitável durante o uso normal pelos usuários finais?**

**Não plenamente.** Com um MTBF de 2,6 minutos — classificado como **Regular** — o sistema apresentou, na sessão de uso controlado, uma falha a cada 2 minutos e 36 segundos em média. Embora o nível Regular não seja o mais crítico da escala, o resultado ainda não atinge o nível mínimo considerado **Bom** (MTBF ≥ 4 minutos), o que significa que o sistema requer melhorias de estabilidade para oferecer uma experiência confiável ao usuário final.

Além das duas falhas mensuradas pelo MTBF, a limitação de design identificada na seção 1.2.1 — retorno silencioso de lista vazia quando o SIGAA não possui dados de matrícula — representa um risco adicional à percepção de qualidade pelo usuário, pois compromete diretamente a função principal do produto sem qualquer sinalização de erro. Por ser uma limitação estrutural e não uma falha pontual, ela não foi contabilizada no cálculo do MTBF, mas deve ser considerada no julgamento geral da confiabilidade do sistema.

## 2. Métrica M2.1 — Percentual de operações críticas com tratamento de exceção

### 2.1 Descrição da medição

A medição da métrica M2.1 foi realizada por meio de um script Python que percorreu o código-fonte do repositório `2023-2-SuaGradeUnB`, identificando as operações críticas previamente definidas na Fase 3 (conexão/consulta ao banco de dados, manipulação de arquivos, _web scraping_ do SIGAA e autenticação/autorização de usuários) e verificando se cada ocorrência está protegida por um bloco de tratamento de exceção (`try/except` no backend Python ou `try/catch` no frontend TypeScript). O registro completo da medição, com o detalhamento de cada ocorrência por arquivo e linha, está disponível em [Registro da Metrica 2.1 - confiabilidade](../fase4/m2_1_registro.md).

O escopo analisado foi:

- **Backend:** diretório `api/` (excluindo _migrations_ e testes);
- **Frontend:** diretório `web/` (excluindo _imports_, _exports_ e comentários).

> O script Python utilizado para a varredura e o arquivo de registro bruto com todos os resultados estão disponíveis em:
>
> - Script de análise: [Codigo da Metrica 2.1 - confiabilidade](../fase4/codigo_analise_metrica_2.1.md)
> - Registro completo das ocorrências: [Registro da Metrica 2.1 - confiabilidade](../fase4/m2_1_registro.md)

O vídeo abaixo registra a execução do script de análise sobre o repositório do _Sua Grade UnB_, demonstrando a coleta dos dados brutos que originaram os resultados apresentados nesta seção.

<div align="center">
  <iframe width="560" height="315" src="https://www.youtube.com/embed/-url25nATps" title="Execução do script de análise grep e AST — Métrica M2.1 (Confiabilidade)" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Vídeo 2 — Execução do script de análise com <code>grep</code> e módulo <code>ast</code> do Python no repositório do <em>Sua Grade UnB</em> para coleta dos dados da métrica M2.1 (Confiabilidade — Tolerância a Falhas).</figcaption></font>
  </div>
</div>

### 2.2 Resultado consolidado

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: left;">
    <tr>
      <th><b>Métrica</b></th>
      <th><b>Valor</b></th>
    </tr>
    <tr>
      <td>Total de operações críticas</td>
      <td>134</td>
    </tr>
    <tr>
      <td>Operações críticas com tratamento de exceção</td>
      <td>24</td>
    </tr>
    <tr>
      <td>Operações críticas sem tratamento de exceção</td>
      <td>110</td>
    </tr>
    <tr>
      <td>Falsos positivos excluídos</td>
      <td>9</td>
    </tr>
    <tr>
      <td><b>M2.1 — Percentual de cobertura</b></td>
      <td><b>17,91%</b></td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 4.4 — Resultado consolidado da métrica M2.1.</figcaption></font>
  </div>
</div>

```
M2.1 = (operações críticas com tratamento de exceção / total de operações críticas) × 100
     = (24 / 134) × 100
     = 17,91%
```

### 2.3 Resultado por categoria de operação crítica

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: left;">
    <tr>
      <th><b>Categoria</b></th>
      <th><b>Total</b></th>
      <th><b>Com tratamento</b></th>
      <th><b>Sem tratamento</b></th>
      <th><b>% coberta</b></th>
    </tr>
    <tr>
      <td>Autenticação e Segurança</td>
      <td>28</td>
      <td>5</td>
      <td>23</td>
      <td>17,9%</td>
    </tr>
    <tr>
      <td>Web Scraping SIGAA</td>
      <td>24</td>
      <td>3</td>
      <td>21</td>
      <td>12,5%</td>
    </tr>
    <tr>
      <td>Regras de Negócio e Banco de Dados</td>
      <td>44</td>
      <td>7</td>
      <td>37</td>
      <td>15,9%</td>
    </tr>
    <tr>
      <td>Infraestrutura e Integração</td>
      <td>38</td>
      <td>9</td>
      <td>29</td>
      <td>23,7%</td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 4.5 — Resultado da métrica M2.1 por categoria de operação crítica.</figcaption></font>
  </div>
</div>

O detalhamento completo de cada ocorrência analisada (arquivo, linha, trecho de código e indicação de tratamento) encontra-se no registro de medição []m2_1_registro.md`, garantindo a rastreabilidade entre o dado bruto coletado e o resultado consolidado apresentado acima.

### 2.4 Julgamento da métrica M2.1

A Fase 2 estabelece a seguinte faixa de pontuação para a métrica M2.1:

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: left;">
    <tr>
      <th><b>Excelente</b></th>
      <th><b>Bom</b></th>
      <th><b>Regular</b></th>
      <th><b>Insatisfatório</b></th>
    </tr>
    <tr>
      <td>≥ 90%</td>
      <td>70%–89%</td>
      <td>40%–69%</td>
      <td>< 40%</td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 4.6 — Pontuação de julgamento da métrica M2.1 (Fase 2).</figcaption></font>
  </div>
</div>

Com o valor obtido de **M2.1 = 17,91%**, abaixo do limite de 40%, a métrica é classificada como **Insatisfatório**. O resultado indica que a grande maioria das operações críticas do sistema (conexões com banco de dados, manipulação de arquivos, _web scraping_ do SIGAA e autenticação/autorização) não está protegida por blocos de tratamento de exceção, o que representa um risco relevante de falhas não tratadas em tempo de execução.

### 2.5 Resposta à Questão GQM (Q2)

**Q2: O código do sistema está adequadamente preparado para tolerar erros em tempo de execução?**

**Não.** Com apenas 17,91% das operações críticas protegidas por blocos de tratamento de exceção — classificação **Insatisfatório** — o sistema está significativamente despreparado para lidar com falhas em tempo de execução. A categoria mais vulnerável é o _Web Scraping_ do SIGAA, com 12,5% de cobertura (3 de 24 operações), seguida de Regras de Negócio e Banco de Dados com 15,9% (7 de 44 operações). Isso significa que erros em pontos críticos como consultas ao banco de dados ou falhas na raspagem de dados do SIGAA tendem a se propagar sem tratamento, podendo resultar em comportamentos imprevisíveis e falhas silenciosas para o usuário final.

## 3. Julgamento Geral da Confiabilidade

A Fase 2 define os seguintes critérios para o julgamento geral da característica Confiabilidade:

- **Aceitável:** ≥ 70% das métricas classificadas como "Bom" ou "Excelente". O sistema demonstra robustez e previsibilidade.
- **Parcialmente aceitável:** entre 40% e 69% das métricas com nível "Regular" ou superior. O sistema funciona, mas pode apresentar instabilidades pontuais.
- **Inaceitável:** > 30% das métricas atingindo o nível "Insatisfatório". A estabilidade do sistema é considerada crítica e propensa a falhas.

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: left;">
    <tr>
      <th><b>Métrica</b></th>
      <th><b>Resultado</b></th>
      <th><b>Classificação</b></th>
    </tr>
    <tr>
      <td>M1.1 — MTBF</td>
      <td>1,45 minutos</td>
      <td>Regular</td>
    </tr>
    <tr>
      <td>M2.1 — % de operações críticas com tratamento de exceção</td>
      <td>17,91%</td>
      <td>Insatisfatório</td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 4.7 — Resumo das classificações das métricas de Confiabilidade.</figcaption></font>
  </div>
</div>

Das duas métricas avaliadas, **1 de 2 (50%)** atingiu o nível **Regular** ou superior (M1.1), e **1 de 2 (50%)** foi classificada como **Insatisfatório** (M2.1). Aplicando os critérios da Fase 2:

- O percentual de métricas em nível "Regular" ou superior (50%) está dentro da faixa de **40% a 69%**, o que caracteriza a Confiabilidade como **Parcialmente aceitável**;
- Ao mesmo tempo, o percentual de métricas em nível "Insatisfatório" (50%) também excede o limite de **30%** estabelecido para o nível "Inaceitável".

Há, portanto, uma sobreposição entre os critérios "Parcialmente aceitável" e "Inaceitável" para este conjunto específico de resultados (2 métricas, sendo 1 Regular e 1 Insatisfatório). Adotando a ordem de verificação dos critérios da Fase 2 — Aceitável, Parcialmente aceitável e, por fim, Inaceitável —, a característica **Confiabilidade** do _Sua Grade UnB_ é classificada como **Parcialmente aceitável**: o sistema funciona, mas apresenta instabilidades pontuais relevantes, evidenciadas tanto pelas falhas observadas na sessão de uso (M1.1) quanto pela baixa cobertura de tratamento de exceções no código (M2.1).

Retomando o propósito da Fase 1 — avaliar se o _Sua Grade UnB_ apresenta nível de Confiabilidade adequado para uso pelos estudantes da UnB —, os resultados indicam que **o sistema ainda não atende plenamente a esse propósito**. A estabilidade percebida pelo usuário final (Q1) é parcialmente comprometida por falhas observáveis durante o uso normal, e a robustez interna do código (Q2) está muito aquém do mínimo esperado, expondo o sistema a riscos de falhas silenciosas em produção.

### 3.1 Recomendações de melhoria

As ações a seguir estão ordenadas por prioridade, com base no impacto identificado na avaliação:

**Prioridade Alta — Corrigir o bug de autenticação (Falha 2 — impacto médio)**

O fluxo de troca de conta Google redireciona o usuário para a página inicial sem abrir o seletor de contas do OAuth. A causa provável é que a sessão ativa não está sendo encerrada antes do redirecionamento para o provedor de identidade. Recomenda-se inspecionar o módulo de autenticação do backend (diretório `api/`) e os componentes de login do frontend (diretório `web/`), verificando se a chamada de logout da sessão Google precede corretamente o fluxo de re-autenticação. Esta é a ação mais urgente por impedir a conclusão de uma tarefa do roteiro de uso.

**Prioridade Média — Ampliar a cobertura de `try/except` no módulo de _Web Scraping_ do SIGAA**

Com apenas 12,5% de cobertura (3 de 24 operações), o módulo responsável pela raspagem de dados do SIGAA é o mais vulnerável do sistema. Recomenda-se revisar todos os pontos de chamada de rede e parsing de HTML do scraper, adicionando blocos `try/except` que capturem ao menos `requests.exceptions.RequestException`, `AttributeError` e `ValueError`, com registro de log em cada ponto de falha. Priorizar as operações de consulta de disciplinas por nome e por professor, que são os fluxos principais do produto.

**Prioridade Média — Ampliar a cobertura de `try/except` nas operações de Banco de Dados**

Com 15,9% de cobertura (7 de 44 operações), as operações de acesso ao banco de dados representam o maior volume absoluto de operações desprotegidas (37 ocorrências). Recomenda-se envolver as operações de CRUD no backend Django em blocos `try/except` que capturem `django.db.DatabaseError` e `django.db.IntegrityError`, garantindo que erros de banco não se propaguem como exceções não tratadas para a camada de apresentação.

**Prioridade Baixa — Corrigir o bug visual na barra de navegação (Falha 1 — impacto muito leve)**

O ícone de seção ativa fica preso entre as duas opções da barra de navegação inferior. Por não impedir nenhuma funcionalidade, este item pode ser tratado em uma sprint de polimento de interface, após a resolução dos itens de maior impacto acima.

## Histórico de Versão

| Versão | Data       | Descrição                                        | Autor(es)                                          |
| :----: | :--------- | :----------------------------------------------- | :------------------------------------------------- |
|  1.0   | 2026-06-12 | Execução das medições de Confiabilidade (Fase 4) | [Edilson Ribeiro](https://github.com/Edilson-r-jr) |
|  2.0   | 2026-06-23 | Aplicação de correções e feedbacks               | [Edilson Ribeiro](https://github.com/Edilson-r-jr) |
