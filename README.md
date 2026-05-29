# ⚖️ Sistema Multiagente — Processo Judicial Brasileiro (1ª Instância)

> Simulação automatizada do fluxo processual civil e penal brasileiro por meio de agentes especializados, orquestrados via framework **OpenCLAWN**.

---

## Visão Geral

Este projeto implementa um **sistema multiagente** que modela o fluxo completo de um processo judicial de primeira instância no Brasil, respeitando as disposições do **CPC/2015**, do **CPP** e da **CF/1988**.

Cada agente representa um ator institucional do sistema judiciário e possui autonomia para executar as ações que lhe competem, comunicando-se com os demais conforme regras de roteamento predefinidas.

---

## Agentes do Sistema

| Agente | Arquivo | Função Principal |
|--------|---------|-----------------|
| **Magistrado** | `agente_magistrado.md` | Direção do processo e prolação de pronunciamentos judiciais |
| **Secretaria Judicial** | `agente_secretaria.md` | Fluxo cartorário, atos ordinatórios e expedição |
| **Oficial de Justiça** | `agente_oficial_justica.md` | Execução material das ordens judiciais em campo |
| **Perito Judicial** | `agente_perito_judicial.md` | Prova técnica especializada e laudo pericial |
| **Ministério Público** | `agente_ministerio_publico.md` | Ação penal (órgão agente) e fiscal da ordem jurídica |
| **Partes e Representantes** | `agente_partes.md` | Postulação, contestação, reconvenção e recursos |

---

## Fluxo Processual

O fluxo segue a lógica do processo civil ordinário, desde a petição inicial até a sentença:

```
Petição Inicial (Partes)
    └─► Autuação e Distribuição (Secretaria)
            └─► Admissibilidade (Magistrado)
                    ├─► [se necessário] Emenda da Inicial (Partes)
                    └─► Expedição de Mandado de Citação
                                └─► Citação/Diligência (Oficial de Justiça)
                                            └─► Devolução do Mandado (Secretaria)
                                                        └─► Contestação (Réu / Partes)
                                                                    └─► Réplica → Saneamento (Magistrado)
                                                                                ├─► Parecer MP (art. 178 CPC)
                                                                                ├─► Julgamento Antecipado
                                                                                └─► Instrução Probatória
                                                                                            └─► Laudo Pericial
                                                                                                    └─► AIJ → Sentença
```

---

## Alertas de Sistema

O orquestrador monitora condições críticas em tempo real e pode bloquear o fluxo ou acionar agentes específicos:

| ID | Severidade | Condição |
|----|-----------|----------|
| `NULIDADE_MP_NAO_INTIMADO` | 🔴 CRÍTICO | MP não intimado em hipótese do art. 178 CPC antes da sentença |
| `PRAZO_PRESO_EXPIRADO` | 🔴 CRÍTICO | Indiciado preso sem manifestação do MP por mais de 5 dias |
| `LAUDO_OMISSO_BLOQUEANTE` | 🟠 ALTO | Laudo pericial com quesitos sem resposta |
| `MANDADO_MOROSO` | 🟡 MÉDIO | Mandado vencido ainda em posse do Oficial de Justiça |

---

## Estrutura de Arquivos

```
agentes_judiciais/
├── 00_orquestrador.md          ← ponto de entrada e configuração do sistema
├── agente_magistrado.md
├── agente_secretaria.md
├── agente_oficial_justica.md
├── agente_perito_judicial.md
├── agente_ministerio_publico.md
└── agente_partes.md
```

---

## Integrações Previstas

O sistema foi projetado para operar integrado com as principais plataformas e bases do Poder Judiciário brasileiro:

| Sistema | Descrição |
|---------|-----------|
| **PJe / e-SAJ / e-Proc / Projudi** | Sistemas processuais eletrônicos |
| **CEMAN** | Central de Mandados |
| **DJE** | Diário de Justiça Eletrônico |
| **SISBAJUD** | Bloqueio de ativos financeiros |
| **RENAJUD** | Constrição de veículos |
| **INFOJUD** | Informações fiscais e patrimoniais |

---

## Base Legal

- **CPC/2015** — Lei nº 13.105/2015 (Código de Processo Civil)
- **CPP** — Decreto-Lei nº 3.689/1941 (Código de Processo Penal)
- **CF/1988** — Constituição Federal da República Federativa do Brasil

---

## Framework

Este sistema é implementado sobre o framework **[OpenCLAWN](https://github.com/openclaw)**, que provê a infraestrutura de orquestração multiagente, roteamento por triggers e controle de estados.

---

## Versão

`v1.0` — Sistema: `processo_judicial_br_1a_instancia`
