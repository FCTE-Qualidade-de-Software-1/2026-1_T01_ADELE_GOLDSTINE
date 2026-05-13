# Objeto de Estudo (Software)

## **Aplicação Escolhida**

O software Sua Grade UnB, é um projeto desenvolvido na disciplina de métodos de desenvolvimento de software, onde seu objetivo principal é auxiliar alunos no planejamento de suas grades horárias para o semestre seguinte, centralizando de forma intuitiva em um só lugar, as matérias, horários e professores, para fornecer previsibilidade aos estudantes para o semestre seguinte. 

**Acesse o SuaGrade [aqui](https://suagradeunb.com.br/)!**

## **Funcionalidades do Projeto**

O projeto conta com as seguintes funcionalidades:
1. Autenticação de usuários:  A autenticação dos usuários é opcional, permitindo que o aluno faça uso da plataforma mesmo sem ter cadastrado sua conta. A autenticação é feita utilizando a conta do Google, dessa forma, dados do usuário são buscados diretamente de sua conta utilizada.

2. Busca de Disciplinas: A plataforma permite buscar disciplinas ofertadas pela Universidade de Brasília através do nome das matérias ou nome de professores, retornando uma lista dos resultados obtidos com base nos parâmetros fornecidos junto aos horários das aulas.

3. Seleção de disciplinas: O sistema permite que usuários façam a seleção de turmas para incluir em sua grade, permitindo que os estudantes montem seus horários.

4. Geração de grade horária: O aluno pode fazer a geração de uma grade horária, incluindo todas as turmas selecionadas em um arquivo para visualização.

5. Exportação de Grades: O aluno possui a alternativa de fazer download de um PDF em sua máquina das grades horárias construídas.

## **Principais tarefas do Sua grade UnB**

O projeto Sua Grade UnB possui como tarefa, a facilitação na elaboração e planejamento dos alunos em relação aos seus horários de aulas, disciplinas e professores selecionados, buscando garantir previsibilidade aos estudantes ao longo do semestre seguinte. É permitido o cadastro de usuários ou uso de forma anônima, sem se identificar, possibilitando a criação de diferentes combinações de grades horárias para um mesmo aluno em um mesmo semestre.

## **Quem são os usuários do projeto**

Os principais usuários do sistema são alunos ativos da Universidade de Brasília matriculados em qualquer curso disponibilizado na universidade.

## **Contexto de uso**

O projeto está inserido dentro de um contexto acadêmico onde alunos fazem o uso dele como facilitador para planejar suas matrículas e rotina, em contextos educacionais, onde é utilizado como objeto de estudo em relação a sua usabilidade, desempenho, e outros tópicos referente a sua qualidade.

## **O que é possível medir**

Devido à natureza open source do projeto, diversos aspectos podem ser analisados com o auxílio de ferramentas especializadas, como o SonarQube, utilizadas para avaliar características relacionadas à manutenibilidade do software. Além disso, essas análises podem ser complementadas por testes realizados pela própria equipe, com o objetivo de verificar a portabilidade do “Sua Grade UnB”. Por outro lado, algumas métricas são mais difíceis de serem avaliadas dentro da realidade atual do projeto, principalmente aquelas ligadas à usabilidade. Isso se deve ao fato de a equipe não possuir recursos específicos, como heatmaps ou testes com usuários, que permitiriam uma análise mais precisa da experiência de uso do sistema. Assim, as medições realizadas neste trabalho estarão concentradas, principalmente, nos aspectos relacionados ao código-fonte e à documentação disponibilizada pelos desenvolvedores do “Sua Grade UnB”.

## **Tecnologia Usadas**

Na versão 1.00 de Sua grade UnB, as seguintes tecnologias foram usadas:

* **Ambiente**
    * Docker
    * Docker Compose
* **Backend**
    * Python
    * Django
    * Django REST Framework
    * PostgreSQL
    * Heroku
* **Frontend**
    * TypeScript
    * Next.js
    * Tailwind CSS
    * Vercel

## **Links úteis do projeto**

**[Acesso ao Projeto](https://suagradeunb.com.br/)**

**[Documentação do projeto](https://unb-mds.github.io/2023-2-SuaGradeUnB/)**

**[Repositorio](https://github.com/unb-mds/2023-2-SuaGradeUnB)**

## **Histórico de Versão**

| Versão | Data       | Descrição                  | Autor(es) | Autor(es) |
|:------:|:-----------|:---------------------------|:----------|:----------|
| 0.1    | 2026-05-12 | Criação inicial da página  | Marllon Cardoso | NULL |
