---
title: confiabilidade

---

## Objetivo de Medição 1: Confiabilidade

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: left;">
    <tr>
      <th><b>Analisar</b></th>
      <td>Sua Grade UnB</td>
    </tr>
    <tr>
      <th><b>Para o propósito de</b></th>
      <td>Avaliar</td>
    </tr>
    <tr>
      <th><b>Com respeito a</b></th>
      <td>Confiabilidade</td>
    </tr>
    <tr>
      <th><b>Do ponto de vista da</b></th>
      <td>Equipe do projeto</td>
    </tr>
    <tr>
      <th><b>No contexto da</b></th>
      <td>Disciplina de Qualidade de Software 1 (FCTE - UnB)</td>
    </tr>
  </table>
</div>

<div align="center">
  <font size="4"><figcaption>Tabela 1: Objetivo de Medição: Confiabilidade</figcaption></font>
</div>

---

### Perguntas e Hipóteses de Medição

Para alcançar o objetivo de medição para a característica de Confiabilidade, abaixo foram elaboradas questões com suas respectivas hipóteses (as quais estabelecem metas que ilustram a qualidade atual do sistema a partir das métricas que serão estabelecidas posteriormente neste fragmento).

> Confiabilidade: Grau em que um sistema, produto ou componente executa as funções especificadas sob condições especificadas por um período de tempo especificado


**Questão 1: Maturidade** (Grau em que um sistema, produto ou componente atende às necessidades de confiabilidade sob operação normal)
> Qual é o tempo médio entre falhas apresentado pelo sistema durante o seu uso em operação normal?

* **Hipótese 1.1 (H1.1):** O sistema apresentará um Tempo Médio Entre Falhas (MTBF) superior a 10 horas ou nenhuma falha encontrada.


**Questão 2: Tolerância a falhas** (Grau em que um sistema, produto ou componente opera conforme o pretendido, apesar da presença de falhas de hardware ou software)
> O quanto o sistema consegue sinalizar a ocorrência de falhas?

* **Hipótese 2.1 (H2.1):** 90% ou mais das operações críticas, como conexões com banco de dados, manipulação de arquivos e autenticação e autorização de usuários, possuirão um mecanismo adequado de tratamento de exceções (try-catch).

---

### Seleção das Métricas

**Questão 1: Maturidade**

* **Métrica 1.1: Tempo Médio Entre Falhas (Mean time between failures - MTBF)**
    * **Definição:** Tempo decorrido de sessão de uso controlado do sistemas dividido pelo número de falhas detectadas durante essa sessão de uso.
    * **Fórmula:** `(Tempo da sessão de uso em horas) / (quantidade de falhas identificadas)`
    * **Coleta:** 
        1. Iniciar uma sessão de uso com usuários finais;
        2. Incrementar 1 em um contador iniciado em 0 a cada falha encontrado pelos usuários;
        3. Ao final da sessão, coletar o tempo decorrido da mesma em horas;
        4.  Aplicar a fórmula apontada acima.

    * **Pontuação de Julgamento:** 

    | **Excelente** | **Bom** | **Regular** | **Insatisfatório** |
    |:--------------:|:--------:|:-------------:|:-------------------:|
    | MTBF > 10 horas ou nenhuma falha detectada | MTBF entre 4 e 10 horas | MTBF entre 1 e 4 horas | MTBF < 1 hora |

    * **Propósito:** Avaliar o nível de estabilidade alcançado pelo produto sob a perspectiva dos usuários finais.


**Questão 2: Tolerância a falhas**

* **Métrica 2.1: Percentual de operações críticas tratadas com exceções.**
    * **Definição:** Percentual de operações críticas que estão contidas dentro de um bloco de exceções (`try-catch`).
    * **Fórmula:** `(Número de operações críticas com try-catch / Número total de operações críticas) * 100`
    * **Coleta:** 
        1. Definir a lista de "operações críticas": chamadas de função/método para conexão com banco de dados, manipulação de arquivos, manipulação de dados do web scraping, autenticação e autorização.
        2. Em **todo o código-fonte do projeto**, contar o número total de ocorrências dessas operações.
        3. Contar quantas dessas ocorrências estão sintaticamente dentro de um bloco `try { ... }`.
        4. Aplicar a fórmula acima.
        
    * **Pontuação de Julgamento:** 

    | **Excelente** | **Bom** | **Regular** | **Insatisfatório** |
    |:--------------:|:--------:|:-------------:|:-------------------:|
    | ≥ 90% | 70%–89% | 40%–69% | < 40% |

    * **Propósito:** Medir a robustez do código contra erros em tempo de execução.

### Critérios para Julgamento

* **Aceitável:** ≥ 70% das métricas classificadas como "Bom" ou "Excelente". O sistema demonstra robustez e previsibilidade.
* **Parcialmente aceitável:** Entre 40% e 69% das métricas com nível "Regular" ou superior. O sistema funciona, mas pode apresentar instabilidades pontuais.
* **Inaceitável:** > 30% das métricas atingindo o nível "Insatisfatório". A estabilidade do sistema é considerada crítica e propensa a falhas.

---

### Relação entre a Confiabilidade, Perguntas e Métricas


<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: left;">
    <tr>
      <th><b>Questão</b></th>
      <th><b>Métricas Simplificadas</b></th>
      <th><b>Tipo de Coleta</b></th>
    </tr>
    <tr>
      <td><b>Maturidade</b><br>Qual é o tempo médio entre falhas apresentado pelo sistema durante o seu uso em operação normal?</td>
      <td>
        M1.1: Métrica 1.1: Tempo Médio Entre Falhas (Mean time between failures - MTBF)
      </td>
      <td>
        Tempo decorrido de sessão de uso controlado do sistemas dividido pelo número de falhas detectadas durante essa sessão de uso.
      </td>
    </tr>
      <tr>
      <td><b>Tolerância a falhas</b><br>O quanto o sistema consegue sinalizar a ocorrência de falhas?</td>
      <td>
        M2.1: Métrica 2.1: Percentual de operações críticas tratadas com exceções.
      </td>
      <td>
        Percentual de operações críticas que estão contidas dentro de um bloco de exceções (`try-catch`).
      </td>
    </tr>
  </table>

  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 2: Questões e Métricas Simplificadas</figcaption></font>
  </div>
</div>

---

### Diagrama GQM - Confiabilidade (Representação Estrutural)

!!! info
    Dê zoom com o scroll do mouse no diagrama para ver melhor. Caso prefira, abra em tela cheia.<br/>
    Você também mode mover o diagrama com o mouse.

``` mermaid

graph TB
  BusinessObjective["Objetivo: Avaliar Confiabilidade do Sua Grade UnB"]
  
  BusinessObjective --> Question1["Q1: Qual é o tempo médio entre falhas apresentado pelo sistema durante o seu uso em operação normal?"]
  BusinessObjective --> Question2["Q2: O quanto o sistema consegue sinalizar a ocorrência de falhas?"]
  
  Question1 --> MetricDefects["M1.1: Tempo Médio Entre Falhas (Mean time between failures - MTBF)"]
  
  Question2 --> MetricExceptions["M2.1: Percentual de operações críticas tratadas com exceções. "]

```

<div style="margin-top: 8px; text-align: center;">
  <font size="4"><figcaption>Figura 1: Diagrama GQM da Confiabilidade. Feito por <a href="http://github.com/m4rllon">Marllon Fausto Cardoso</a></figcaption></font>
</div>