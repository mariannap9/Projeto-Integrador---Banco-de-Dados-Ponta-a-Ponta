# Semana 1 Levantamento de requisitos e análise do problema

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

## 3. Informações que precisam ser armazenadas

- Eventos: nome, descrição, período e local.
- Atividades de cada evento: título, tipo (palestra, minicurso, mesa-redonda), horário, sala, vagas e carga horária.
- Palestrantes e quais atividades cada um conduz.
- Participantes: nome, e-mail, CPF e instituição.
- Inscrições: participante, evento, data e situação.
- Presenças: participante, atividade e momento do registro.

## 4. Requisitos funcionais

| Id. | Requisito |
| --- | --- |
| RF01 | O sistema deverá permitir cadastrar, consultar e alterar eventos, com nome, descrição, datas e local. |
| RF02 | O sistema deverá permitir cadastrar as atividades de um evento, com título, tipo, horário, sala, vagas e carga horária. |
| RF03 | O sistema deverá permitir cadastrar palestrantes. |
| RF04 | O sistema deverá permitir vincular um ou mais palestrantes a cada atividade. |
| RF05 | O sistema deverá permitir cadastrar participantes com nome, e-mail, CPF e instituição. |
| RF06 | O sistema deverá permitir inscrever um participante em um evento, registrando a
