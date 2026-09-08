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
- [MVP](#-mvp)
- [Status do projeto](#-status-do-projeto)
- [Funcionalidades](#-funcionalidades)
- [Perfis de usuário](#-perfis-de-usuário)
- [Regras de negócio](#-regras-de-negócio-principais)
- [Modelo de dados](#-modelo-de-dados)
- [Tecnologias utilizadas](#️-tecnologias-utilizadas)
- [Acesso ao projeto](#-acesso-ao-projeto)
- [Abrir e rodar o projeto](#-abrir-e-rodar-o-projeto)
- [Fluxo de desenvolvimento](#-fluxo-de-desenvolvimento)
- [Roadmap](#️-roadmap)
- [Pessoas desenvolvedoras](#-pessoas-desenvolvedoras)
- [Licença](#-licença)

---

## 📋 Descrição do projeto

O **Loteiro** é uma plataforma para organizar o transporte de passageiros entre cidades,
substituindo a gestão manual realizada atualmente por meio de grupos do WhatsApp.

A plataforma foi pensada para centralizar:

- viagens;
- veículos;
- motoristas;
- passageiros;
- reservas;
- assentos;
- posteriormente, encomendas;
- pagamentos;
- localização em tempo real;
- notificações;
- gestão operacional.

O projeto será desenvolvido de forma incremental, começando por um **MVP (Minimum Viable Product)**
com o fluxo principal de uma viagem e suas reservas.

---

# 🚀 MVP

## Objetivo

O primeiro objetivo do projeto é validar o fluxo principal do transporte antes de implementar
funcionalidades mais complexas.

### Fluxo principal

```text
Gerente
   ↓
Cria a viagem
   ↓
Define veículo, motorista e rota
   ↓
Passageiro consulta as viagens
   ↓
Escolhe origem e destino
   ↓
Escolhe um assento disponível
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
   ↓
Realiza a reserva
   ↓
Motorista visualiza a lista de passageiros
