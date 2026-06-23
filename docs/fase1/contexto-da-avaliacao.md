# Contexto da Avaliação

## 1 - Propósito e Uso dos Resultados
O desenvolvimento de um plano de avaliação da qualidade do software “SuaGrade” ocorreu a partir de um conjunto de interesses por parte deste grupo, podendo ser descritos como:

* **Medir a frequência** com que o sistema apresenta falhas ou comportamentos inesperados durante sua utilização;
* **Obter um relatório** sobre a qualidade atual do sistema para as características priorizadas, identificando possíveis limitações que possam impactar a estabilidade e continuidade do projeto a longo prazo;
* **Avaliar se a documentação** disponibilizada pelos desenvolvedores é suficiente para apoiar a manutenção e continuidade do projeto;
* **Analisar as possibilidades de evolução** deste produto de software (seja para novas funcionalidades ou para a melhoria das que já estão implementadas);
* **Avaliar a efetividade dos recursos** atuais do sistema em facilitar o planejamento acadêmico dos estudantes;
* **Avaliar a consistência** nas informações exibidas aos usuários, principalmente em relação às disciplinas, horários e professores;
* **Analisar a eficiência** dos recursos implementados atualmente no sistema.

Essa série de interesses decorre, sumariamente, dos relatos de experiência de uso dos usuários do “SuaGrade” e do interesse, por parte dos autores deste plano e dos próprios desenvolvedores, em avaliar sua qualidade sob as características mais importantes (conforme evidenciado em Modelo de Qualidade).

## 2 - Requisitantes e Partes Interessadas
O projeto alvo deste plano é amplamente utilizado por estudantes da Universidade de Brasília semestralmente. Portanto, este grupo tem como expectativas a elaboração de um plano que contribua tanto com a melhoria do escopo atual, quanto com a evolução do mesmo, a fim de atender ainda mais às necessidades da grande comunidade de estudantes que o utiliza.

## 3 - Escopo e Profundidade
Como dito anteriormente, o software “Sua Grade UnB” atende às necessidades de uma ampla comunidade de estudantes da Universidade de Brasília todos os semestres desde sua criação. Com isso, é elementar que ele apresenta indicadores de qualidade satisfatórios. Diante disso, este plano de avaliação será executado sob os seguintes módulos que compõem esse sistema:

**Backend (api)**<br/>
Uma API REST desenvolvida com Python 3.11, Django 4.2 e Django REST Framework, alimentado por um banco de dados com o SGBD PostgreSQL. Esse módulo concentra três grandes responsabilidades:

- Módulo de Usuários, responsável pela autenticação (via Google OAuth) e pelo armazenamento das grades salvas, permitindo que o aluno acesse suas grades de qualquer dispositivo. 
- Módulo de Disciplinas, que oferece a busca de disciplinas por nome ou por código, gerenciando a pesquisa que alimenta a interface.
- Módulo de Geração de Grade, que aplica um algoritmo de produto cartesiano para gerar todas as combinações possíveis de horários e disciplinas. Em seguida, trata os dados para eliminar conflitos de horário e disciplinas repetidas, retornando as cinco melhores grades.

**Web Scraping**<br/>
Módulo ligado ao Backend que realiza web scraping no site do SIGAA da UnB, de onde vem as todas as informações das disciplinas. Os dados coletados passam por um tratamento para ficarem legíveis e organizados, e então são cadastrados no PostgreSQL gerenciado pelo Django.

**Frontend (web)**<br/>
É a interface com o usuário final, construída em TypeScript com Next.js e estilizada com Tailwind CSS, hospedada na Vercel. Ele consome a API REST e conduz o fluxo da aplicação: tela de login (com cadastro/login pela conta Google), busca de disciplinas, seleção das matérias desejadas, geração das grades possíveis e salvamento da grade escolhida (no banco ou no armazenamento local do navegador).


## Histórico de Versão

| Versão | Data       | Descrição                  | Autor(es) |
|:------:|:-----------|:---------------------------|:----------|
| 0.1    | 2026-05-11 | Criação e documentação da página  | Gabriel Magioli |
| 1.0    | 2026-06-23 | Detalhamento dos módulos avaliados do Sua Grade UnB | Marllon Cardoso     |

