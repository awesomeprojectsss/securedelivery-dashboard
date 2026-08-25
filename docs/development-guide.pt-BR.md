# Guia de Desenvolvimento — SecureDelivery Dashboard

## Objetivo

Este documento orienta desenvolvedores do painel web do SecureDelivery.

Antes de desenvolver, leia:

1. `../docs/project.md`
2. `AGENTS.md`
3. `docs/architecture.md`
4. ADRs relevantes em `docs/decisions/`
5. `docs/git-workflow.pt-BR.md`

## Stack oficial

- Next.js
- TypeScript

O backend NestJS continua sendo a fonte de verdade de negócio e autorização.

## Filosofia

O painel é uma aplicação operacional B2B.

Prioridades:

- clareza;
- consistência;
- acessibilidade;
- performance;
- estados operacionais explícitos;
- experiências distintas por role.

Não transforme o Next.js em uma SPA client-side genérica sem necessidade.

## Next.js

Se o projeto usar App Router:

- prefira Server Components por padrão;
- use Client Components apenas quando necessário;
- mantenha `"use client"` no menor boundary possível;
- não coloque `"use client"` automaticamente em páginas/layouts;
- use recursos nativos do framework com intenção.

O padrão real do repositório e ADRs prevalece.

## Backend como autoridade

Não replique no frontend regras como:

- quem pode inativar usuário;
- tenant isolation;
- acesso a SmartBox;
- permissão de alterar roles.

O frontend deve:

- esconder/desabilitar ações para UX;
- tratar 401/403;
- renderizar experiência coerente.

O backend deve validar de verdade.

## Perfis

Experiências:

```text
SUPER_ADMIN
ADMIN
CUSTOMER
```

Não tratar como "mesma tela com alguns botões escondidos" quando a arquitetura de informação for diferente.

### Super Admin

Foco em:

- governança;
- clientes;
- usuários/admins;
- visão global;
- suporte.

### Admin

Foco em:

- operação;
- clientes;
- SmartBoxes;
- eventos;
- suporte.

### Customer

Foco em:

- suas SmartBoxes;
- seus eventos;
- solicitações;
- suporte.

## Device no código, SmartBox na interface

O contrato técnico usa:

```text
Device
deviceId
/api/v1/devices
```

A interface mostra:

```text
SmartBox
SmartBoxes
Minhas SmartBoxes
```

Não crie contrato paralelo `/smartboxes`.

Não renomeie `deviceId` para `smartBoxId` nos DTOs compartilhados.

Quando necessário, crie View Models de apresentação.

## Eventos e sensores extensíveis

O Dashboard deve suportar eventos ainda desconhecidos.

Para tipos conhecidos, pode existir componente especializado.

Para evento desconhecido válido, mostrar fallback genérico:

- tipo;
- severidade;
- data/hora;
- SmartBox;
- localização;
- atributos.

O frontend não deve usar enum fechado contendo todos os eventos possíveis.

Consulte `docs/contracts/` na raiz do workspace antes de implementar consumo de API.

## KPIs de velocidade

O painel pode apresentar:

- velocidade média em movimento;
- velocidade máxima;
- distância monitorada;
- tempo em movimento;
- tempo parado;
- eventos por 100 km;
- eventos por faixa de velocidade.

A API utiliza unidades SI.

Converter `m/s` para `km/h` apenas na camada de apresentação.

Não calcule velocidade média fazendo média simples de valores por minuto.

Prefira o valor derivado pelo servidor a partir de distância e tempo em movimento.

Ao relacionar eventos e velocidade, use linguagem como:

```text
ocorreu a
associado a
eventos nessa faixa de velocidade
```

Evite afirmar causalidade sem método analítico validado.

## Server state e UI state

Diferencie:

- dados vindos do backend;
- estado local de interface;
- eventos realtime;
- autenticação.

Evite copiar toda resposta da API para um store global sem necessidade.

## WebSocket

Realtime não substitui fetch/API.

Fluxo recomendado:

```text
API -> estado autoritativo
WebSocket -> sinal de atualização
Reconexão -> reconciliar com API
```

Evite múltiplas subscriptions duplicadas.

Limpe subscriptions corretamente.

## TypeScript

- strict typing;
- evitar `any`;
- evitar casts inseguros;
- preferir unions discriminadas para estados complexos;
- tipar contratos de API;
- representar loading/error/data de forma explícita.

## Componentes

Prefira:

- componentes pequenos e reutilizáveis;
- componentes por feature;
- composição;
- design system consistente.

Evite:

- página de milhares de linhas;
- lógica de API dentro de componente visual;
- duplicação de tabela/modal/formulário.

## UX operacional

Sempre prever:

- loading;
- skeleton;
- vazio;
- erro;
- offline/desconectado;
- retry;
- sucesso;
- confirmação para ação destrutiva.

Estados críticos devem chamar atenção sem transformar tudo em vermelho.

## Acessibilidade

- HTML semântico;
- labels;
- teclado;
- foco;
- contraste;
- mensagens de erro compreensíveis;
- botões e links com propósito claro.

## Segurança

Nunca colocar secret em:

```text
NEXT_PUBLIC_*
```

Tudo que é público para o browser deve ser considerado público.

Evite expor dados de clientes sem necessidade.

## Testes

Priorize:

- RBAC de UI;
- navegação por perfil;
- restrições de Super Admin;
- telas Customer isoladas;
- SmartBox states;
- reconexão WebSocket;
- tickets/chat;
- loading/error/empty.

Use e2e para jornadas críticas quando a stack de testes for definida.

## Dependências

Antes de adicionar pacote:

- verifique se Next.js/React já resolve;
- confira manutenção;
- bundle impact;
- segurança;
- compatibilidade com SSR/RSC;
- licença.

Mudanças arquiteturais relevantes devem gerar ADR.

## Antes de abrir PR

Execute os scripts reais do projeto.

Normalmente:

```bash
npm install
npm run lint
npm run test
npm run build
```

ou os equivalentes definidos no gerenciador adotado pelo repositório.

Não invente um package manager se o repositório já tiver lockfile.

Consulte `docs/git-workflow.pt-BR.md`.
