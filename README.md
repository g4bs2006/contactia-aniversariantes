# Aniversariantes

App que mostra os pacientes que fazem aniversário no mês e agenda o parabéns pelo
WhatsApp, com modelos de mensagem da própria clínica. Ele abre dentro da
plataforma de mensagens que a clínica já usa.

> Este repositório é uma vitrine: mostra as telas e o desenho do sistema. O código
> é privado. Todos os dados dos prints são fictícios, de uma clínica de demonstração.

![Aniversariantes do mês, com a prévia da mensagem](telas/08-agenda-um-selecionado.png)

## O que ele resolve

Mandar parabéns aos pacientes aproxima a clínica, mas ninguém tinha tempo de
olhar a data de nascimento de cada um no prontuário. O app lista os aniversariantes
do mês, mostra quem ainda não recebeu mensagem e agenda tudo de uma vez, para o
dia do aniversário ou alguns dias antes.

## Telas

### Agenda

Os aniversariantes do mês agrupados por dia, com filtros por situação e busca. O
painel ao lado mostra o que será enviado.

| Sem mensagem | O mês inteiro |
|---|---|
| ![Sem mensagem](telas/01-agenda-sem-mensagem.png) | ![Todos do dia](telas/02-agenda-todos-hoje.png) |

| Com mensagem | Sem telefone |
|---|---|
| ![Com mensagem](telas/04-agenda-com-mensagem.png) | ![Sem telefone](telas/05-agenda-sem-telefone.png) |

| Seleção em lote | Agendamento confirmado |
|---|---|
| ![Seleção em lote](telas/07-agenda-selecao-em-lote.png) | ![Agendamento confirmado](telas/11-agenda-agendamento-confirmado.png) |

| Data e hora | Dezembro |
|---|---|
| ![Seletor de data e hora](telas/09-agenda-data-e-hora.png) | ![Troca de mês](telas/12-agenda-dezembro.png) |

### Modelos

Cada modelo tem as variáveis ligadas aos dados do paciente e a regra de quando
sai. A prévia mostra a mensagem como o paciente vai ver.

![Modelos](telas/40-modelos.png)

| Quando sai | Campo da variável |
|---|---|
| ![Quando sai](telas/41-modelos-quando-sai.png) | ![Campo da variável](telas/43-modelos-campo-da-variavel.png) |

| Modelo com vídeo | Alterações não salvas |
|---|---|
| ![Modelo com vídeo](telas/45-modelos-com-video.png) | ![Alterações não salvas](telas/44-modelos-alteracoes-nao-salvas.png) |

### Histórico

Cada envio e a situação dele, com filtros e cancelamento em lote.

![Histórico](telas/20-historico.png)

| Com falha | Cancelar em lote |
|---|---|
| ![Com falha](telas/23-historico-com-falha.png) | ![Cancelar em lote](telas/26-historico-cancelar-em-lote.png) |

A ficha do envio traz a linha do tempo, a mensagem como saiu e os parabéns dos
anos anteriores.

| Lida | Agendada |
|---|---|
| ![Ficha de envio lido](telas/27-ficha-lida.png) | ![Ficha de envio agendado](telas/28-ficha-agendada.png) |

| Falhou | Não saiu |
|---|---|
| ![Ficha com falha](telas/30-ficha-falhou.png) | ![Ficha de envio que não saiu](telas/31-ficha-nao-saiu.png) |

### Setup

A área da equipe que implanta o app: cadastro da clínica, prontuário, canal de
WhatsApp, sincronização, link do painel e unidades.

| Entrada | Clínicas |
|---|---|
| ![Entrada do setup](telas/60-setup-entrar.png) | ![Lista de clínicas](telas/61-setup-clinicas.png) |

| Clínica | Escolher canal |
|---|---|
| ![Clínica](telas/62-setup-clinica.png) | ![Escolher canal](telas/65-setup-escolher-canal.png) |

| Teste, sincronização e link | Unidades |
|---|---|
| ![Teste e sincronização](telas/66-setup-teste-sincronizacao-e-link.png) | ![Unidades](telas/67-setup-unidades.png) |

### Programa beta

Página de convite para novas clínicas, com a lista de interessados no setup.

| Convite | Pedido preenchido |
|---|---|
| ![Página do beta](telas/80-beta.png) | ![Pedido preenchido](telas/82-beta-pedido-preenchido.png) |

| Pedido recebido | Interessados |
|---|---|
| ![Pedido recebido](telas/83-beta-na-lista.png) | ![Interessados](telas/71-setup-interessados.png) |

## Principais funcionalidades

- Aniversariantes do mês lidos do prontuário, agrupados por dia.
- Parabéns agendado em lote, para o dia do aniversário ou dias antes, com data e
  hora opcionais.
- Em dezembro, os aniversários de janeiro já contam para o ano seguinte.
- Situação real de cada mensagem (enviada, entregue, lida ou com falha),
  conferida a cada 15 minutos.
- Modelos com variáveis e vídeo, e conferência do número de envio de cada unidade.
- Várias unidades por clínica.
- API versionada com contrato OpenAPI para outros sistemas da equipe.

## Integrações

- **Prontuários:** e-Clínica, consultada na hora, e Clinicorp, lido de uma cópia
  renovada toda madrugada ou pelo botão "Sincronizar agora". A API do Clinicorp
  lista um dia por vez e tem limite de chamadas por hora, por isso a cópia.
- **Plataforma de mensagens:** WhatsApp, canais e equipes.
- **Cadastro comum:** pode ler a clínica do cadastro central dos produtos.

## Como foi construído

- **Fatia vertical:** um diretório por caso de uso, um núcleo puro e portas para o
  prontuário e para as mensagens.
- **Acesso sem login:** a clínica entra pela aba da plataforma ou por um link
  assinado (HMAC). O setup tem senha própria.
- **Banco isolado:** esquema próprio no Postgres, com acesso só pelo servidor.
- **Contrato do banco conferido no deploy.**

| Camada | Tecnologias |
|---|---|
| Telas | Next.js 16, React 19, Tailwind 4, Radix, react-day-picker |
| API | Hono, zod, OpenAPI 3.1 |
| Dados | PostgreSQL (Supabase) |
| Qualidade | TypeScript, Vitest, ESLint, CI no GitHub Actions |
| Entrega | Docker, nginx, VPS |
