# SGEA Banco de Dados de Eventos Acadêmicos

Projeto da disciplina de Banco de Dados (Instituto Federal do Triângulo Mineiro IFTM, Campus Uberlândia Centro).
**Cenário 4:** sistema de eventos acadêmicos: eventos, atividades, palestrantes, inscrições e presença.

## Integrantes

- Luanna Naves de Oliveira
- Marcelo Cabral Rodrigues
- Marianna Peres Castro

## O problema

Uma instituição organiza congressos, seminários e palestras e precisa controlar, em um só lugar, quem se inscreveu em cada evento e quem esteve presente em cada atividade. O banco responde a perguntas como:

- Quantos participantes estão inscritos em cada evento?
- Quais atividades possuem maior participação?
- Quais participantes atingiram determinada frequência?

## Modelo

Sete tabelas: `evento`, `atividade`, `palestrante`, `atividade_palestrante`, `participante`, `inscricao` e `presenca`.
Diagrama e dicionário de dados em [`docs/modelo-de-dados.md`](docs/modelo-de-dados.md).

## Regras de negócio protegidas pelo banco

| Regra | Como é garantida |
| --- | --- |
| Data de fim do evento não anterior à de início | `CHECK` em `evento` |
| Atividade dentro do período do evento | gatilho em `atividade` |
| Uma inscrição por participante em cada evento | `UNIQUE (participante_id, evento_id)` |
| Inscrição com situação definida (Confirmada ou Cancelada) | `CHECK` em `inscricao` |
| Presença só de inscrição Confirmada | gatilho em `presenca` |
| Presença na atividade e inscrição do mesmo evento | chaves estrangeiras compostas por `evento_id` |
| Uma presença por participante em cada atividade | `UNIQUE (atividade_id, inscricao_id)` |
| CPF e e-mail únicos | `UNIQUE` em `participante` |

