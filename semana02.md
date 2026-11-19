# Semana 2 — Diagrama Entidade-Relacionamento (DER) e Modelo Lógico Relacional

## 1. Identificação do grupo e cenário escolhido

| Campo | Informação |
| :--- | :--- |
| **Projeto** | SGEA — Banco de Dados de Eventos Acadêmicos |
| **Cenário escolhido** | Cenário 4: Sistema de eventos acadêmicos |
| **Integrantes** | Luanna Naves de Oliveira, Marcelo Cabral Rodrigues, Marianna Peres Castro |
| **Instituição / Disciplina** | IFTM (Campus Uberlândia Centro) Banco de Dados |
| **Etapa** | Semana 2: DER, Modelo Lógico Relacional e Normalização |

---

## 2. Revisão das entidades e evolução em relação à Semana 1

Na Semana 1, foram levantadas sete entidades preliminares (`Evento`, `Atividade`, `Palestrante`, `Atividade_Palestrante`, `Participante`, `Inscricao` e `Presenca`). Durante a transição para a modelagem conceitual e lógica, foram realizados os seguintes refinamentos técnicos para garantir total aderência às regras de negócio (RN01 a RN10):

1. **Padronização de nomenclatura:** Todas as entidades e atributos foram padronizados em `snake_case`, no singular e sem acentuação (`evento`, `atividade`, `palestrante`, `atividade_palestrante`, `participante`, `inscricao`, `presenca`), mantendo consistência com o esquema físico em PostgreSQL.
2. **Chaves primárias substitutas (`id`) vs. naturais:** Adotou-se chave primária substituta (`id`) nas entidades principais (`evento`, `atividade`, `palestrante`, `participante`, `inscricao`, `presenca`) para simplificar referências entre tabelas, preservando as chaves naturais (`cpf` e `email` em `participante`; `email` em `palestrante`) com restrições `UNIQUE`.
3. **Propagação de `evento_id` para integridade composta em `presenca` (RN08):** Para impedir estruturalmente que uma presença associe uma inscrição de um evento A a uma atividade de um evento B, o atributo `evento_id` foi incluído em `presenca` formando duas chaves estrangeiras compostas: `(atividade_id, evento_id)` referenciando `atividade(id, evento_id)` e `(inscricao_id, evento_id)` referenciando `inscricao(id, evento_id)`.
4. **Detalhamento de tipos e domínios:** Os horários de início e fim das atividades (`data_hora_inicio` e `data_hora_fim`) foram definidos com data e hora completas (`TIMESTAMP`) para permitir validar se a atividade ocorre dentro do período do evento (`data_inicio` a `data_fim`) e evitar intervalos invertidos.

---

## 3. Diagrama Entidade-Relacionamento (DER)

### 3.1. Diagrama visual (Mermaid — renderização nativa no GitHub)

```mermaid
erDiagram
    EVENTO {
        int id PK "Identificador único do evento"
        varchar nome "Nome oficial do evento"
        text descricao "Descrição e objetivos"
        date data_inicio "Data de início"
        date data_fim "Data de encerramento (>= data_inicio)"
        varchar local "Local ou campus de realização"
    }

    ATIVIDADE {
        int id PK "Identificador único da atividade"
        int evento_id FK "Evento ao qual pertence (UK composta com id)"
        varchar titulo "Título da atividade"
        varchar tipo "Palestra, Minicurso, Mesa-redonda, etc."
        timestamp data_hora_inicio "Início da atividade"
        timestamp data_hora_fim "Término da atividade"
        varchar sala "Sala ou auditório"
        int vagas_totais "Capacidade máxima (> 0)"
        numeric carga_horaria "Duração em horas (> 0)"
    }

    PALESTRANTE {
        int id PK "Identificador único do palestrante"
        varchar nome "Nome completo"
        varchar email UK "E-mail único de contato"
        varchar instituicao "Instituição de vínculo"
    }

    ATIVIDADE_PALESTRANTE {
        int atividade_id PK "FK -> atividade(id)"
        int palestrante_id PK "FK -> palestrante(id)"
    }

    PARTICIPANTE {
        int id PK "Identificador único do participante"
        varchar nome "Nome completo"
        varchar email UK "E-mail único"
        char cpf UK "CPF único (11 dígitos)"
        varchar instituicao "Instituição de origem"
    }

    INSCRICAO {
        int id PK "Identificador único da inscrição"
        int participante_id FK "FK -> participante(id)"
        int evento_id FK "FK -> evento(id) (UK composta com id)"
        timestamp data_inscricao "Data/hora do registro"
        varchar situacao "Confirmada ou Cancelada"
    }

    PRESENCA {
        int id PK "Identificador único da presença"
        int atividade_id FK "FK composta -> atividade(id, evento_id)"
        int inscricao_id FK "FK composta -> inscricao(id, evento_id)"
        int evento_id FK "Garante mesmo evento entre atividade e inscrição"
        timestamp data_hora_registro "Momento da validação de presença"
    }

    EVENTO ||--o{ ATIVIDADE : "possui"
    EVENTO ||--o{ INSCRICAO : "recebe"
    PARTICIPANTE ||--o{ INSCRICAO : "efetua"
    ATIVIDADE ||--o{ ATIVIDADE_PALESTRANTE : "tem"
    PALESTRANTE ||--o{ ATIVIDADE_PALESTRANTE : "conduz"
    ATIVIDADE ||--o{ PRESENCA : "registra"
    INSCRICAO ||--o{ PRESENCA : "comprova"
