# Avaliação — Confiabilidade

Esta página apresenta os resultados da **Fase 4 — Execução da Avaliação** referentes à característica de qualidade **Confiabilidade**, avaliada segundo o modelo de qualidade SQuaRE e a norma ISO/IEC 25010:2011. As métricas executadas nesta fase — M1.1 (Tempo Médio Entre Falhas) e M2.1 (Percentual de operações críticas com tratamento de exceção) — foram planejadas na [Fase 3](../fase3/planejamento.md) e seguem o método, os recursos e o roteiro lá definidos.

## 1. Métrica M1.1 — Tempo Médio Entre Falhas (MTBF)

### 1.1 Descrição da medição

A medição da métrica M1.1 foi realizada por meio de uma sessão de uso controlada na instância em produção do *Sua Grade UnB* (`https://suagradeunb.com.br/`), seguindo o roteiro de tarefas representativo do uso normal definido na Fase 3. A sessão teve duração total de **2min54s**, durante a qual foram executadas as seguintes atividades:

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
      <td>2min47s</td>
      <td>Bug de autenticação: ao autenticar com uma conta Google e, em seguida, tentar trocar de conta, o usuário é redirecionado imediatamente para a página inicial do aplicativo, sem que a tela de seleção de conta do Google seja exibida.</td>
      <td>Autenticação / Troca de conta Google</td>
      <td>Médio — impede a conclusão da tarefa de troca de conta, exigindo contorno (ex.: logout manual ou limpeza de sessão).</td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 4.1 — Falhas observadas durante a sessão de uso.</figcaption></font>
  </div>
</div>

### 1.3 Resultado consolidado

A métrica M1.1 foi calculada com base na fórmula definida na Fase 2 — **MTBF = (tempo da sessão de uso em minutos) / (quantidade de falhas identificadas)** —, convertendo a duração da sessão (2min54s) para minutos decimais:

```
Duração da sessão = 2min54s = 2 + (54/60) = 2,9 minutos

MTBF = (tempo da sessão em minutos) / (número de falhas)
     = 2,9 / 2
     = 1,45 minutos
```

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: left;">
    <tr>
      <th><b>Indicador</b></th>
      <th><b>Valor</b></th>
    </tr>
    <tr>
      <td>Duração da sessão</td>
      <td>2min54s (≈ 2,9 minutos)</td>
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
      <td>2min47s</td>
    </tr>
    <tr>
      <td><b>M1.1 — MTBF</b></td>
      <td><b>1,45 minutos</b></td>
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

Com o valor obtido de **MTBF = 1,45 minutos**, dentro do intervalo de 1 a 4 minutos, a métrica M1.1 é classificada como **Regular**. O resultado indica que, sob uso normal, o sistema apresentou falhas em uma frequência que compromete parcialmente a estabilidade percebida pelos usuários finais, embora não no nível mais crítico da escala.

## 2. Métrica M2.1 — Percentual de operações críticas com tratamento de exceção

### 2.1 Descrição da medição

A medição da métrica M2.1 foi realizada por meio de um script Python que percorreu o código-fonte do repositório `2023-2-SuaGradeUnB`, identificando as operações críticas previamente definidas na Fase 3 (conexão/consulta ao banco de dados, manipulação de arquivos, *web scraping* do SIGAA e autenticação/autorização de usuários) e verificando se cada ocorrência está protegida por um bloco de tratamento de exceção (`try/except` no backend Python ou `try/catch` no frontend TypeScript). O registro completo da medição, com o detalhamento de cada ocorrência por arquivo e linha, está disponível em [Registro da metrica M2.1 - Confiabilidade](m2_1_registro.md)

O escopo analisado foi:

- **Backend:** diretório `api/` (excluindo *migrations* e testes);
- **Frontend:** diretório `web/` (excluindo *imports*, *exports* e comentários).

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

O detalhamento completo de cada ocorrência analisada (arquivo, linha, trecho de código e indicação de tratamento) encontra-se no registro de medição `m2_1_registro.md`, garantindo a rastreabilidade entre o dado bruto coletado e o resultado consolidado apresentado acima.

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

Com o valor obtido de **M2.1 = 17,91%**, abaixo do limite de 40%, a métrica é classificada como **Insatisfatório**. O resultado indica que a grande maioria das operações críticas do sistema (conexões com banco de dados, manipulação de arquivos, *web scraping* do SIGAA e autenticação/autorização) não está protegida por blocos de tratamento de exceção, o que representa um risco relevante de falhas não tratadas em tempo de execução.

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

Há, portanto, uma sobreposição entre os critérios "Parcialmente aceitável" e "Inaceitável" para este conjunto específico de resultados (2 métricas, sendo 1 Regular e 1 Insatisfatório). Adotando a ordem de verificação dos critérios da Fase 2 — Aceitável, Parcialmente aceitável e, por fim, Inaceitável —, a característica **Confiabilidade** do *Sua Grade UnB* é classificada como **Parcialmente aceitável**: o sistema funciona, mas apresenta instabilidades pontuais relevantes, evidenciadas tanto pelas falhas observadas na sessão de uso (M1.1) quanto pela baixa cobertura de tratamento de exceções no código (M2.1).

Recomenda-se priorizar, como ações corretivas, (i) a correção dos defeitos identificados durante a sessão de uso — em especial o bug de autenticação relacionado à troca de conta Google, classificado com impacto médio — e (ii) a ampliação da cobertura de blocos de tratamento de exceção nas operações críticas, sobretudo nas categorias de *Web Scraping* SIGAA e Regras de Negócio/Banco de Dados, que apresentaram as menores porcentagens de cobertura (12,5% e 15,9%, respectivamente).

## Histórico de Versão

| Versão | Data       | Descrição                                          | Autor(es) |
|:------:|:-----------|:----------------------------------------------------|:----------|
| 1.0    | 2026-06-11 | Execução das medições de Confiabilidade (Fase 4)   | Edilson Junior         |