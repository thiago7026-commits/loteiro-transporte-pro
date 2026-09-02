<h1 align="center">🚐 Loteiro</h1>

<p align="center">
  <img loading="lazy" src="http://img.shields.io/static/v1?label=STATUS&message=EM%20DESENVOLVIMENTO&color=GREEN&style=for-the-badge" alt="Status: Em desenvolvimento">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.11-blue?style=flat-square&logo=python" alt="Python">
  <img src="https://img.shields.io/badge/Django-REST%20Framework-092E20?style=flat-square&logo=django" alt="Django REST Framework">
  <img src="https://img.shields.io/badge/PostgreSQL-database-336791?style=flat-square&logo=postgresql" alt="PostgreSQL">
  <img src="https://img.shields.io/badge/Docker-container-2496ED?style=flat-square&logo=docker" alt="Docker">
  <img src="https://img.shields.io/badge/license-not%20defined-lightgrey?style=flat-square" alt="Licença não definida">
</p>

<p align="center">
  Plataforma para organizar o transporte entre <strong>Cavalcante-GO</strong> e <strong>Brasília-DF</strong>,
  substituindo a gestão manual feita hoje via WhatsApp.
</p>

---

## 📑 Índice

- [Descrição do projeto](#-descrição-do-projeto)
- [Status do projeto](#-status-do-projeto)
- [Funcionalidades](#-funcionalidades)
- [Perfis de usuário](#-perfis-de-usuário)
- [Regras de negócio](#-regras-de-negócio-principais)
- [Tecnologias utilizadas](#️-tecnologias-utilizadas)
- [Acesso ao projeto](#-acesso-ao-projeto)
- [Abrir e rodar o projeto](#-abrir-e-rodar-o-projeto)
- [Roadmap](#️-roadmap)
- [Pessoas desenvolvedoras](#-pessoas-desenvolvedoras)
- [Licença](#-licença)

---

## 📋 Descrição do projeto

O **Loteiro** é um sistema que controla motoristas, veículos, passageiros, encomendas, reservas,
pagamentos e localização em tempo real, permitindo que os passageiros façam todo o processo de
consulta, reserva e pagamento de forma autônoma — sem depender de mensagens no grupo do WhatsApp.

O projeto conta com 3 perfis de usuário (Passageiro, Motorista e Gerente/Dono do carro), cada um
com um conjunto próprio de permissões e funcionalidades.

---

## 🚧 Status do projeto

> 🚧 Projeto em construção 🚧

---

## 🔨 Funcionalidades

- 🔐 `Autenticação`: cadastro e login de passageiros via telefone/SMS, com fluxo de recuperação de acesso
- 🎫 `Reservas`: consulta de viagens, escolha de assento e confirmação em tempo real, com suporte a necessidades especiais (ex: cadeirinha para crianças)
- 🚗 `Veículos e viagens`: cadastro de carros, motoristas e status de viagem (Saindo, Em viagem, Parada, Almoço, Abastecimento, Banheiro, Chegando, Finalizado)
- 📍 `Localização em tempo real`: acompanhamento do veículo no mapa com previsão de chegada e distância restante
- 📦 `Encomendas`: registro e rastreamento de envios entre origem e destino
- 💰 `Módulo financeiro`: controle de pendências, pagamento pela plataforma e bloqueio automático por inadimplência
- 📊 `Painel do gerente`: visão consolidada de veículos, motoristas, ocupação, encomendas e status das viagens

---

## 👥 Perfis de usuário

| Perfil | Principais ações |
|---|---|
| **Passageiro** | Reservar viagens, acompanhar localização, ver histórico, pagar pendências |
| **Motorista** | Ver viagens e passageiros atribuídos, compartilhar localização, atualizar status |
| **Gerente / Dono do carro** | Cadastrar motoristas e veículos, criar viagens, organizar assentos e encomendas, acompanhar pagamentos |

---

## 📐 Regras de negócio principais

- **Bloqueio por inadimplência** — passageiro com pendência não pode fazer nova reserva até regularizar o pagamento
- **Liberação automática** — pagamento confirmado libera o passageiro sem intervenção do gerente
- **Controle de motoristas** — cadastro de motorista feito exclusivamente pelo gerente
- **Prevenção de concorrência** — o sistema impede que dois passageiros ocupem a mesma vaga/assento
- **Necessidades especiais** — passageiro pode indicar na reserva a necessidade de cadeirinha infantil ou outro tipo de assento especial *(regra a detalhar)*

---

## 🛠️ Tecnologias utilizadas

- [Python](https://www.python.org/)
- [Django](https://www.djangoproject.com/) + [Django REST Framework](https://www.django-rest-framework.org/)
- [PostgreSQL](https://www.postgresql.org/)
- [Docker](https://www.docker.com/)
- [Git](https://git-scm.com/) / [GitHub](https://github.com/)

---

## 📁 Acesso ao projeto

Você pode acessar o código-fonte deste repositório ou baixá-lo via clone:

```bash
git clone https://github.com/seu-usuario/loteiro.git
```

---

## 🛠 Abrir e rodar o projeto

Após clonar o repositório:

```bash
cd loteiro

# Suba os containers
docker-compose up --build

# Rode as migrations
docker-compose exec web python manage.py migrate

# Crie um superusuário
docker-compose exec web python manage.py createsuperuser
```

A aplicação estará disponível em `http://localhost:8000`.

> ⚠️ Ajuste os comandos acima conforme a configuração real do `docker-compose.yml` do projeto.

---

## 🗺️ Roadmap

- [ ] Módulo de autenticação e gestão de acessos
- [ ] Módulo de veículos e viagens
- [ ] Módulo de reservas
- [ ] Módulo de localização em tempo real
- [ ] Módulo de encomendas
- [ ] Painel do gerente
- [ ] Notificações (viagem confirmada, motorista chegando, pagamento confirmado, etc.)

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
  </tr>
</table>

---

## 📄 Licença

Defina aqui a licença do projeto (ex: MIT, proprietária, etc.).
