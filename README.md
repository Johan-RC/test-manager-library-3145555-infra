# test-manager-library-3145555-infra

> **Technical test · Sheet ADSO 3145555**
> Schedule: **Wednesday 14 October 2026, from 6:50 a. m. to 11:00 a. m.** (Colombia time). Delivery at **11:00 a. m.**

This repository is the **infrastructure** of *Manager Library*: containers, network, volumes and an
empty database engine. **It starts empty on purpose**: it is built on each trainee's fork.

## What this repository is allowed to contain

Per **Article V** of the constitution and **ADR-001**: this repository brings up an **empty** engine.
It creates the container, the user, the volume and the network — **nothing else**.

- **No `CREATE TABLE`.** All DDL belongs to the application migrations in
  [`api`](../test-manager-library-3145555-api).
- **No seed data**, not even the five categories.
- **No domain SQL.** The demo seeder in [`tool`](../test-manager-library-3145555-tool) goes through the
  HTTP API, never through the database.

A schema changed by hand in the engine is a defect, not a variant.

## Run it

```bash
cp .env.example .env      # then fill in JWT_SECRET and ADMIN_PASSWORD
docker compose up
```

That single command is **RNF-5** and the evidence for **T-20**:

| Service | Port | What it is |
|---|---|---|
| `db` | `5432` | PostgreSQL 16, **no tables** until the API migrations run |
| `api` | `8000` | Laravel backend, `http://localhost:8000/api/v1` |
| `app` | `5173` | React frontend, `http://localhost:5173` |

To verify that the engine really starts empty:

```bash
docker compose exec db psql -U library -d library -c "\dt library.*"
```

It must return **no rows** before the migrations run.

## Configuration

Every secret comes from the environment (Article IX). Only `.env.example` is versioned; the real `.env`
is in `.gitignore`.

| Variable | Mandatory | Used by |
|---|---|---|
| `JWT_SECRET` | yes | `api` — signing key for the HS256 token |
| `ADMIN_PASSWORD` | yes | `api` — initial administrator, created on boot |
| `ADMIN_USERNAME` | no (default `admin`) | `api` |

Database credentials are local defaults (`library`/`library`) and are overridable through `DB_*`.

## Instructions

Every trainee must **fork the six project repositories** and **solve the project against the stated spec**.

1. Fork, into their own GitHub account, each repository from the table at the end.
2. Read the spec in [`test-manager-library-3145555-docs`](https://github.com/code-sena/test-manager-library-3145555-docs).
   It is delivered in two versions: `spec-python/` and `spec-.net/`.
3. Develop on the forks.

## The challenge is built with React and PHP (Laravel)

The spec is written for Python and for .NET, but the challenge is **not** done in those languages:

| Layer | Technology of the challenge |
|---|---|
| Frontend | React |
| Backend | PHP with Laravel |

What the spec defines about the business — stories, acceptance criteria, rules, API contract, data model — is respected. What it defines about technology is translated to React and Laravel.

## The test is not about writing code

The main purpose is to assess **performance with SDD** (*Spec-Driven Development*): how a specification is read, interpreted and applied to carry it over to a different stack. The code is the means, not the end.

## Delivery

What counts is **what is pushed to the forks before 11:00 a. m.** Anything that arrives later is not reviewed.
There is no extension: push partial progress with small commits during the test.

## The six repositories

| Repository | What goes there |
|---|---|
| [`test-manager-library-3145555-docs`](https://github.com/code-sena/test-manager-library-3145555-docs) | The spec: `spec-python/` and `spec-.net/` |
| [`test-manager-library-3145555-api`](https://github.com/code-sena/test-manager-library-3145555-api) | Backend in PHP (Laravel) |
| [`test-manager-library-3145555-app`](https://github.com/code-sena/test-manager-library-3145555-app) | Frontend in React |
| [`test-manager-library-3145555-page`](https://github.com/code-sena/test-manager-library-3145555-page) | Static public presentation site |
| [`test-manager-library-3145555-infra`](https://github.com/code-sena/test-manager-library-3145555-infra) | Containers, network, volumes and an empty database engine |
| [`test-manager-library-3145555-tool`](https://github.com/code-sena/test-manager-library-3145555-tool) | Utilities: demo data seeder |