# SGEA Banco de Dados de Eventos Acadêmicos

Projeto da disciplina de Banco de Dados (Instituto Federal do Triângulo Mineiro — IFTM, Campus Uberlândia Centro).
**Cenário 4:** sistema de eventos acadêmicos — eventos, atividades, palestrantes, inscrições e presença.

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

## Estrutura do repositório

```
.
├── README.md
├── docs/
│   ├── semana-1-levantamento-de-requisitos.md   # cenário, requisitos, regras de negócio, entidades, perguntas
│   ├── modelo-de-dados.md                       # diagrama ER (Mermaid) e dicionário de dados
│   └── referencias/                             # especificação funcional original do SGEA (PDF)
└── sql/
    ├── 01_schema.sql                            # tabelas, restrições e gatilhos
    ├── 02_dados_exemplo.sql                     # dados fictícios para teste
    ├── 03_consultas.sql                         # as 6 consultas gerenciais
    └── 04_testes_regras.sql                     # comandos que devem falhar (provam as regras)
```

## Como executar (Supabase ou PostgreSQL)

1. Crie um projeto no [Supabase](https://supabase.com) (ou um banco PostgreSQL local).
2. No **SQL Editor**, rode os arquivos nesta ordem:
   1. `sql/01_schema.sql` — cria as tabelas (apaga e recria o esquema, então use em banco de teste);
   2. `sql/02_dados_exemplo.sql` — carrega os dados de exemplo (rode uma única vez, em banco recém-criado);
   3. `sql/03_consultas.sql` — executa as consultas, uma por vez;
   4. `sql/04_testes_regras.sql` — opcional; cada comando deve retornar erro.

Com `psql`:

```bash
psql "$DATABASE_URL" -f sql/01_schema.sql -f sql/02_dados_exemplo.sql
psql "$DATABASE_URL" -f sql/03_consultas.sql
```

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

## Andamento

- [x] Semana 1 — levantamento de requisitos e análise do problema
- [ ] Esquema físico e consultas em SQL (PostgreSQL)
- [ ] Modelagem no drawDB (exportar imagem para `docs/`)
- [ ] Implementação no Supabase
- [ ] Documentação final

- `main` fica sempre funcionando; cada mudança vai em uma branch (`git checkout -b feature/nome-da-mudanca`);
- abra um Pull Request e peça a revisão de outro integrante antes de juntar;
- mensagens de commit curtas e no imperativo: `Adiciona consulta de frequência`.
