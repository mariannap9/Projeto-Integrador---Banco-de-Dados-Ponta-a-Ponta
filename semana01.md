# Semana 1: Levantamento de requisitos e análise do problema

## 1. Identificação do grupo e cenário escolhido

| Campo | Informação |
| --- | --- |
| Cenário escolhido | Cenário 4 — Sistema de eventos acadêmicos |
| Integrantes | Luanna Naves de Oliveira, Marcelo Cabral Rodrigues, Marianna Peres Castro |
| Disciplina | Banco de Dados — Semana 1: levantamento de requisitos e análise do problema |

## 2. Contextualização e descrição do problema

### Organização fictícia

O Instituto Acadêmico Horizonte (IAH) é uma instituição de ensino superior fictícia que promove, a cada ano, congressos, seminários e palestras abertos a alunos, professores e público externo. A Coordenação de Eventos do IAH planeja a programação, cadastra os palestrantes e recebe as inscrições.

### Problema

Hoje a Coordenação controla tudo em planilhas separadas: uma para inscritos, outra para a programação e listas de papel para a presença. Esses dados ficam duplicados e se contradizem, e não há como saber com segurança quantos participantes se inscreveram em cada evento, quais atividades tiveram mais público ou quem cumpriu a frequência mínima.

### O que o banco de dados deve resolver

O banco deve guardar, em um só lugar, os eventos e suas atividades, os palestrantes de cada atividade, os participantes, as inscrições com sua situação e a presença de cada participante em cada atividade. Com isso, a Coordenação poderá consultar totais de inscritos, a participação por atividade e a frequência de cada participante, sem refazer contagens à mão.

### Informações que precisam ser armazenadas

- Eventos: nome, descrição, período e local.
- Atividades de cada evento: título, tipo (palestra, minicurso, mesa-redonda), horário, sala, vagas e carga horária.
- Palestrantes e quais atividades cada um conduz.
- Participantes: nome, e-mail, CPF e instituição.
- Inscrições: participante, evento, data e situação.
- Presenças: participante, atividade e momento do registro.

## 3. Requisitos funcionais

| Id. | Requisito |
| --- | --- |
| RF01 | O sistema deverá permitir cadastrar, consultar e alterar eventos, com nome, descrição, datas e local. |
| RF02 | O sistema deverá permitir cadastrar as atividades de um evento, com título, tipo, horário, sala, vagas e carga horária, e listá-las com seus palestrantes. |
| RF03 | O sistema deverá permitir cadastrar palestrantes e vincular um ou mais deles a cada atividade. |
| RF04 | O sistema deverá permitir cadastrar participantes com nome, e-mail, CPF e instituição. |
| RF05 | O sistema deverá permitir inscrever um participante em um evento, registrando a data e a situação da inscrição. |
| RF06 | O sistema deverá permitir alterar a situação de uma inscrição, inclusive cancelá-la. |
| RF07 | O sistema deverá permitir registrar a presença de um participante em uma atividade. |
| RF08 | O sistema deverá permitir consultar o total de inscritos por evento, a participação por atividade e a frequência de cada participante. |

## 4. Regras de negócio

| Id. | Regra |
| --- | --- |
| RN01 | Todo evento possui nome, data de início e data de fim, e a data de fim não pode ser anterior à de início. |
| RN02 | Um evento pode possuir várias atividades, e toda atividade pertence a um único evento. |
| RN03 | O horário de uma atividade deve estar dentro do período do evento a que pertence. |
| RN04 | Uma atividade pode ter vários palestrantes, e um palestrante pode conduzir várias atividades. |
| RN05 | Um participante pode se inscrever em vários eventos, e um evento pode receber vários participantes. |
| RN06 | Um participante só pode ter uma inscrição por evento. |
| RN07 | Toda inscrição deve ter uma situação definida: Confirmada ou Cancelada. |
| RN08 | A presença deve estar vinculada a uma atividade e a um participante com inscrição Confirmada no evento dessa atividade. |
| RN09 | Um participante só pode ter uma presença por atividade. |
| RN10 | O CPF e o e-mail de cada participante são únicos no sistema. |

## 5. Lista preliminar de entidades e atributos

| Entidade | Principais atributos | Relacionamentos |
| --- | --- | --- |
| Evento | id, nome, descrição, data de início, data de fim, local | 1 evento tem N atividades e N inscrições |
| Atividade | id, título, tipo, data e hora de início e fim, sala, vagas totais, carga horária | Pertence a 1 evento; tem N palestrantes e N presenças |
| Palestrante | id, nome, e-mail, instituição | Conduz N atividades, por Atividade_Palestrante |
| Atividade_Palestrante | id da atividade, id do palestrante | Liga Atividade e Palestrante (N:N) |
| Participante | id, nome, e-mail, CPF, instituição | Tem N inscrições |
| Inscrição | id, data da inscrição, situação | Liga 1 participante a 1 evento; tem N presenças |
| Presença | id, data e hora do registro | Liga 1 atividade a 1 inscrição |

## 6. Perguntas que o banco deverá responder

1. Quantos participantes estão inscritos em cada evento?
2. Quais atividades possuem maior participação?
3. Quais participantes atingiram determinada frequência em um evento?
4. Quais palestrantes conduzem mais atividades?
5. Qual a taxa de ocupação das vagas de cada atividade?

As consultas correspondentes estão em [`sql/03_consultas.sql`](sql/03_consultas.sql), na mesma ordem (o arquivo traz ainda uma sexta consulta, sobre inscrições canceladas, que é um extra).
