<h1 align="center">🚐 Loteiro</h1>

<p align="center">
  <img loading="lazy" src="http://img.shields.io/static/v1?label=STATUS&message=EM%20DESENVOLVIMENTO&color=GREEN&style=for-the-badge" alt="Status: Em desenvolvimento">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.11-blue?style=flat-square&logo=python" alt="Python">
  <img src="https://img.shields.io/badge/Django-REST%20Framework-092E20?style=flat-square&logo=django" alt="Django REST Framework">
  <img src="https://img.shields.io/badge/PostgreSQL-16-336791?style=flat-square&logo=postgresql" alt="PostgreSQL 16">
  <img src="https://img.shields.io/badge/Docker-container-2496ED?style=flat-square&logo=docker" alt="Docker">
  <img src="https://img.shields.io/badge/license-not%20defined-lightgrey?style=flat-square" alt="Licença não definida">
</p>

<p align="center">
  Plataforma para organizar o transporte de passageiros entre <strong>Cavalcante-GO</strong> e <strong>Brasília-DF</strong>,
  substituindo o controle manual feito hoje via WhatsApp.
</p>

---

## 📑 Índice

- [Descrição do projeto](#-descrição-do-projeto)
- [Status do projeto](#-status-do-projeto)
- [Fluxo principal](#-fluxo-principal)
- [Modelagem de dados](#-modelagem-de-dados)
- [Regras de negócio críticas](#-regras-de-negócio-críticas)
- [Fora do escopo do MVP](#-fora-do-escopo-do-mvp)
- [Permissões por perfil](#-permissões-por-perfil)
- [Tecnologias utilizadas](#️-tecnologias-utilizadas)
- [API](#-api-endpoints-principais)
- [Estrutura de branches](#-estrutura-de-branches)
- [Fluxo de Git](#-fluxo-de-git)
- [Testes obrigatórios](#-testes-obrigatórios)
- [Acesso e ambiente local](#-acesso-e-ambiente-local)
- [Ordem de prioridade](#-ordem-de-prioridade-regra-de-ouro)
- [Pessoas desenvolvedoras](#-pessoas-desenvolvedoras)
- [Licença](#-licença)

---

## 📋 Descrição do projeto

O **Loteiro** controla motoristas, veículos, viagens e reservas de passageiros na rota
Cavalcante-GO ↔ Brasília-DF. O MVP prioriza **simplicidade com correção**: o maior desafio técnico
não é a interface, e sim garantir que a disponibilidade de assentos por trecho da viagem seja
sempre consistente, mesmo sob concorrência.

---

## 🚧 Status do projeto

> 🚧 Projeto em construção — fase de MVP 🚧

---

## 🔄 Fluxo principal

```
GERENTE cria Trip
  → define motorista, veículo e rota
    → PASSAGEIRO visualiza viagens disponíveis
      → escolhe origem e destino
        → escolhe assento
          → confirma reserva (Reservation + SeatAllocations)
            → MOTORISTA visualiza seus passageiros
```

---

## 🗂 Modelagem de dados

### User (customizado, único modelo para todos os papéis)
| Campo | Observação |
|---|---|
| `id` | — |
| `name` | — |
| `phone` | único — usado como identificador de login |
| `role` | `MANAGER`, `DRIVER` ou `PASSENGER` |

> ⚠️ Não criar tabelas separadas `Passenger`, `Driver` e `Manager` no MVP.

### Vehicle
| Campo | Observação |
|---|---|
| `id` | — |
| `model` | pode se repetir |
| `plate` | único |
| `active` | veículos inativos permanecem no banco para histórico, mas não entram em novas viagens |

> ⚠️ Sem substituição de veículo no MVP.

### Trip
Representa **toda a operação de um dia** (ida **e** volta juntas — não criar Trips separadas).

| Campo | Observação |
|---|---|
| `id` | — |
| `date` | — |
| `departure_time` / `return_time` | — |
| `driver` | 1 motorista por Trip |
| `vehicle` | 1 veículo por Trip |
| `status` | `SCHEDULED`, `IN_PROGRESS`, `COMPLETED`, `CANCELLED` |

> ⚠️ O mesmo motorista não pode ter duas viagens no mesmo dia. **Não** criar UNIQUE global em
> `date + departure_time` — dois carros podem sair no mesmo horário.

### Capacidade
Cada veículo comporta **5 pessoas**: 1 motorista + 4 passageiros. Assentos de passageiro numerados de **1 a 4**.

### RouteStop
Pertence a uma Trip. Campos: `city`, `order`, `direction` (`OUTBOUND` ou `RETURN`).

**Ida:** 1 Cavalcante → 2 Teresina → 3 Alto Paraíso → 4 São João d'Aliança → 5 São Gabriel → 6 Brasília
**Volta:** 1 Brasília → 2 São Gabriel → 3 São João d'Aliança → 4 Alto Paraíso → 5 Teresina → 6 Cavalcante

> ⚠️ `order` é único por Trip + direção.

### TripSegment
Cada par consecutivo de `RouteStop` forma um segmento (ex: Cavalcante→Teresina, Teresina→Alto Paraíso...).

### Reservation
| Campo | Observação |
|---|---|
| `passenger`, `trip`, `boarding_stop`, `destination_stop`, `seat`, `status` | `status`: `CONFIRMED` ou `CANCELLED` |

> ⚠️ A origem precisa estar antes do destino na sequência da direção escolhida.
> ⚠️ Não assumir `UNIQUE(trip, passenger)` — decisão ainda em aberto pela squad.

### SeatAllocation
Relaciona `reservation + segment + seat`.

> 🔑 **Regra central:** um mesmo `TripSegment` não pode ter duas alocações ativas para o mesmo assento.
> Uma reserva Cavalcante→Brasília no assento 2 ocupa os 5 segmentos da ida com o assento 2.
> A criação das alocações deve ocorrer **em uma única transação** (tudo ou nada).

---

## ⚠️ Regras de negócio críticas

- **Reuso de assento por trecho** — o mesmo número de assento pode ser usado em trechos não sobrepostos.
  - ✅ Permitido: João (Cavalcante→Alto Paraíso, assento 1) + Maria (Alto Paraíso→Brasília, assento 1)
  - ❌ Proibido: João (Cavalcante→São Gabriel, assento 1) + Maria (Alto Paraíso→Brasília, assento 1) — trechos se sobrepõem
- **Concorrência** — duas reservas simultâneas do mesmo assento/segmento só podem resultar em **uma** vencedora. Proteção obrigatória no **backend + banco** (transação + unicidade), nunca só no frontend. Cenário precisa de teste automatizado.
- **Cancelamento** — libera o assento sem apagar o histórico da Reservation. Estratégia de `SeatAllocation` cancelada (remoção vs. inativação com constraint parcial) ainda será definida na implementação.
- **Necessidade especial de assento** — passageiro pode indicar cadeirinha infantil ou assento especial na reserva *(regra a detalhar)*.

---

## 🚫 Fora do escopo do MVP

GPS em tempo real, rastreamento, pagamento online, SMS, pacotes/encomendas, avaliações,
dashboard avançado, notificações avançadas, substituição automática de motorista/veículo,
sistema complexo de manutenção.

---

## 🔐 Permissões por perfil

| Perfil | Pode fazer |
|---|---|
| **MANAGER** | Criar/alterar/cancelar viagens; definir motorista, veículo e rota |
| **DRIVER** | Visualizar suas viagens e passageiros |
| **PASSENGER** | Visualizar viagens; escolher origem/destino; reservar; consultar e cancelar suas próprias reservas |

> Todas as regras de negócio devem ser validadas no **backend**.

---

## 🛠️ Tecnologias utilizadas

- [Python](https://www.python.org/)
- [Django](https://www.djangoproject.com/) + [Django REST Framework](https://www.django-rest-framework.org/)
- [PostgreSQL 16](https://www.postgresql.org/)
- [Docker](https://www.docker.com/)
- [Git](https://git-scm.com/) / [GitHub](https://github.com/)
- Testes automatizados

---

## 🌐 API (endpoints principais)

```
POST   /api/auth/login/
POST   /api/auth/register/
GET    /api/trips/
POST   /api/trips/
GET    /api/trips/{id}/
POST   /api/reservations/
GET    /api/reservations/
GET    /api/reservations/{id}/
POST   /api/reservations/{id}/cancel/
```

> A convenção REST final deve ser consolidada pela squad.

---

## 🌿 Estrutura de branches

| Branch | Responsabilidade |
|---|---|
| `feature/database` | User, Vehicle, Trip, RouteStop, TripSegment, Reservation, SeatAllocation, migrations e constraints |
| `feature/trips` | CRUD de viagens, motorista, veículo, rota, paradas e segmentos |
| `feature/reservations` | Disponibilidade, criação/cancelamento, origem/destino, assento, SeatAllocation e concorrência |
| `feature/auth` | Custom User, login, autenticação, roles e permissões |
| `feature/frontend` | Login, lista/detalhes de viagens, origem/destino, assentos, confirmação, tela do motorista |

---

## 🔀 Fluxo de Git

Não desenvolver diretamente na `main`. Fluxo obrigatório:

```
branch → commit → push → Pull Request → Code Review → testes → aprovação → merge
```

`main` deve ser protegida contra push direto. Cada branch tem responsabilidade única e clara.

---

## ✅ Testes obrigatórios

- **User** — telefone único, roles e autenticação
- **Trip** — motorista, veículo, data e status
- **RouteStop** — ordem, direção e validação de origem/destino
- **Reservation** — assento 1–4, origem/destino, criação e cancelamento
- **SeatAllocation** — não duplicar o mesmo assento no mesmo segmento
- **Concorrência** — duas tentativas simultâneas para o mesmo assento/segmento resultam em apenas uma reserva bem-sucedida

---

## 📁 Acesso e ambiente local

```bash
git clone https://github.com/seu-usuario/loteiro.git
cd loteiro
```

Ambiente de referência atual da squad: Django rodando localmente e PostgreSQL 16 em Docker
(porta local `5434`).

```bash
# Suba o banco em Docker
docker-compose up -d

# Rode as migrations
python manage.py migrate

# Crie um superusuário
python manage.py createsuperuser
```

> ⚠️ Ajuste os comandos conforme o `docker-compose.yml` final do projeto.

---

## 🥇 Ordem de prioridade (regra de ouro)

> O MVP deve ser **simples, mas correto**.

```
Banco → Autenticação → Viagens → Reservas → Frontend → Testes → Integração
```

O ponto mais sensível é a disponibilidade de assentos por segmento — permitir reutilizar o mesmo
assento em trechos diferentes sem permitir reservas conflitantes.

---

## 👨‍💻 Pessoas desenvolvedoras

<table>
  <tr>
    <td align="center">
      <a href="https://github.com/alisson1017-stack">
        <img loading="lazy" src="https://github.com/alisson1017-stack.png" width="100px;" alt="Foto de Álisson no GitHub"/><br />
        <sub><b>Álisson</b></sub>
      </a>
    </td>
    <td align="center">
      <a href="https://github.com/MiguelMDias">
        <img loading="lazy" src="https://github.com/MiguelMDias.png" width="100px;" alt="Foto de Miguel no GitHub"/><br />
        <sub><b>Miguel</b></sub>
      </a>
    </td>
    <td align="center">
      <a href="https://github.com/MarcosSouza-dev">
        <img loading="lazy" src="https://github.com/MarcosSouza-dev.png" width="100px;" alt="Foto de Marcos no GitHub"/><br />
        <sub><b>Marcos</b></sub>
      </a>
    </td>
    <td align="center">
      <a href="https://github.com/thiago7026-commits">
        <img loading="lazy" src="https://github.com/thiago7026-commits.png" width="100px;" alt="Foto de Thiago no GitHub"/><br />
        <sub><b>Thiago</b></sub>
      </a>
    </td>
    <td align="center">
      <a href="https://github.com/LeoPreviato">
        <img loading="lazy" src="https://github.com/LeoPreviato.png" width="100px;" alt="Foto de Leo no GitHub"/><br />
        <sub><b>Leo</b></sub>
      </a>
    </td>
  </tr>
</table>

---

## 📄 Licença

Defina aqui a licença do projeto (ex: privado/proprietário, MIT, etc.).
