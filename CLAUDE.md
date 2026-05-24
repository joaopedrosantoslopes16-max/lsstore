# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

---

# MazyOS — Sistema operacional do negócio

Sua empresa roda em cima desse arquivo. Aqui ficam as regras de operação
do MazyOS — como o Claude lê o contexto, aprende com correções, mantém
tudo atualizado e cria skills novas conforme a operação evolui.

Esse arquivo é editável. Quando o `/instalar` rodar, ele complementa o
final dessa página com as regras específicas do seu negócio.

---

## Contexto do negócio

No início de toda conversa, ler os seguintes arquivos (quando existirem
e estiverem preenchidos):

1. `_memoria/empresa.md` — quem é o usuário, o que faz, como funciona o negócio
2. `_memoria/preferencias.md` — tom de voz, estilo de escrita, o que evitar
3. `_memoria/estrategia.md` — foco atual, prioridades, prazos

Usar essas informações como base pra qualquer resposta ou decisão. Ao
sugerir prioridades, formatos ou abordagens, considerar o foco atual
descrito em `estrategia.md`.

Pra qualquer tarefa visual (carrossel, post, landing page), consultar
`identidade/design-guide.md` como referência de estilo.

Não é necessário listar o que foi lido nem confirmar a leitura. Apenas
usar o contexto naturalmente.

---

## Fluxo de trabalho

Antes de executar qualquer tarefa, verificar se existe skill relevante
em `.claude/skills/`. Se encontrar, seguir as instruções da skill. Se
não encontrar, executar a tarefa normalmente.

Ao concluir uma tarefa que não tinha skill mas parece repetível (o
usuário provavelmente vai pedir de novo no futuro), perguntar:

> "Isso pode virar uma skill pra próxima vez. Quer que eu crie?"

Não perguntar pra tarefas pontuais ou perguntas simples. Só quando o
padrão de repetição for claro.

---

## Aprender com correções

Quando o usuário corrigir algo, melhorar uma resposta ou dar uma
instrução que parece permanente (frases como "na verdade é assim", "não
faça mais isso", "prefiro assim", "sempre que...", "evita...", "da
próxima vez..."), perguntar:

> "Quer que eu salve isso pra não precisar repetir?"

Se sim, identificar onde faz mais sentido salvar:

- **Sobre o negócio** (clientes, serviços, mercado) → `_memoria/empresa.md`
- **Sobre preferências e estilo** (tom de voz, formato, o que evitar) → `_memoria/preferencias.md`
- **Sobre prioridades e foco** (projetos, metas, prazos) → `_memoria/estrategia.md`
- **Regra de comportamento nessa pasta** → próprio `CLAUDE.md`

Salvar com uma linha nova clara, sem reformatar o arquivo inteiro.
Confirmar mostrando a linha adicionada.

Não perguntar se a correção for óbvia de contexto imediato (ex: "na
verdade o arquivo se chama X"). Só perguntar quando a informação tiver
valor duradouro.

---

## Manter contexto atualizado

Ao terminar uma tarefa que mudou algo relevante (cliente novo, skill
nova, mudança de foco, processo novo, ferramenta instalada, estrutura
alterada), perguntar:

> "Isso mudou algo no teu contexto. Quer que eu atualize a memória?"

Se sim, identificar o que atualizar:

- **Cliente, serviço, ferramenta, equipe** → `_memoria/empresa.md`
- **Mudança de prioridade ou foco** → `_memoria/estrategia.md`
- **Tom ou estilo** → `_memoria/preferencias.md`
- **Pasta, regra de organização, skill criada** → `CLAUDE.md`
- **Visual (cores, fontes, logo)** → `identidade/design-guide.md`

Mostrar o que vai mudar antes de salvar. Não reformatar o arquivo
inteiro, só adicionar ou editar a linha relevante.

**Quando NÃO perguntar:**
- Tarefas pontuais sem impacto no contexto (escrever um email avulso, criar um post)
- Perguntas simples ou conversas sem ação
- Mudanças já salvas pelo bloco "Aprender com correções"

**Dica:** rode `/atualizar` pra uma varredura completa quando houver dúvida.

---

## Criação de skills

Quando o usuário pedir skill nova:

1. Verificar se existe template relevante em `templates/skills/`. Se
   existir, usar como base e adaptar pro contexto
2. Perguntar se é específica desse projeto ou útil em qualquer:
   - Específica → `.claude/skills/nome-da-skill/SKILL.md` (local)
   - Universal → `~/.claude/skills/nome-da-skill/SKILL.md` (global)
3. Ler `_memoria/empresa.md` e `_memoria/preferencias.md` pra calibrar
   o conteúdo da skill ao contexto do negócio
4. Se a skill precisar de arquivos de apoio (templates, exemplos),
   criar dentro da pasta da skill
5. Seguir o fluxo da skill-creator nativa do Claude Code

---

## Arquitetura do sistema

### Estrutura de diretórios

```
_memoria/          ← cérebro do negócio (lido antes de qualquer resposta)
  empresa.md       ← nome, o que faz, clientes, equipe, ferramentas
  preferencias.md  ← tom de voz, o que evitar, estilo de escrita
  estrategia.md    ← foco atual, gargalos, prioridades

identidade/
  design-guide.md  ← cores, tipografia, logo — toda skill visual lê aqui antes de gerar

.claude/skills/    ← skills locais do projeto (cada pasta = uma skill com SKILL.md)

marketing/         ← outputs de conteúdo e SEO
saidas/            ← entregáveis finais (propostas, relatórios, carrosséis)
dados/             ← inputs brutos (CSVs, exports, PDFs pra análise)
scripts/           ← automações e scripts de suporte

templates/
  perfis/          ← templates de CLAUDE.md por perfil (solopreneur, freelancer, agência, empresa)
  identidade/      ← exemplos de design-guide.md por perfil
  skills/          ← catálogo de skills externas disponíveis pra instalar
  ferramentas/     ← catálogo de ferramentas externas compatíveis
```

### Skills disponíveis (`.claude/skills/`)

| Skill | Comando | O que faz |
|---|---|---|
| instalar | `/instalar` | Setup inicial: entrevista o usuário, preenche `_memoria/` e adapta o `CLAUDE.md` pro perfil |
| abrir | `/abrir` | Carrega contexto da sessão lendo os 3 arquivos de `_memoria/` e devolve resumo de 5 linhas |
| salvar | `/salvar` | Commit + push no GitHub; na primeira vez configura o remote |
| atualizar | `/atualizar` | Varre o workspace e propõe atualizações nos arquivos de `_memoria/` que ficaram defasados |
| carrossel | `/carrossel` | Gera carrossel 1080×1350 em HTML com identidade visual do `design-guide.md` |
| publicar-tema | `/publicar-tema` | Dado um tema: gera artigo de blog + carrossel + 3 legendas amarradas |
| seo | `/seo` | Fluxo completo de 8 passos (demanda → concorrência → GMB → on-page → conteúdo → ads → monitoramento → GEO); salva outputs em `marketing/seo/` |
| anuncio-google | `/anuncio-google` | Monta campanha de Google Ads em CSV pronto pra importar no Editor |
| relatorio-ads | `/relatorio-ads` | Lê exports de Google Ads + Meta e gera relatório semanal com alertas |
| responder-avaliacoes | `/responder-avaliacoes` | Escreve respostas humanas pras reviews do Google no tom da marca |
| aprovar-post | `/aprovar-post` | Publica blog + Instagram + Facebook num comando |
| analisar-dados | `/analisar-dados` | Lê CSV/XLSX/PDF e gera resumo executivo |
| email-profissional | `/email-profissional` | Rascunha email a partir de contexto livre |
| mapear-rotinas | `/mapear-rotinas` | Descobre o que o usuário repete e transforma em skill personalizada |
| novo-projeto | `/novo-projeto` | Cria pasta isolada pra cada cliente ou iniciativa |

### Como as skills se integram

- Toda skill começa lendo `_memoria/empresa.md` + `_memoria/preferencias.md` pra calibrar tom e contexto
- Skills visuais (`/carrossel`, `/publicar-tema`) leem `identidade/design-guide.md` antes de gerar qualquer HTML
- `/seo` (passo 5) alimenta `/publicar-tema`; `/seo` (passo 6) alimenta `/anuncio-google`
- Outputs persistem em `marketing/`, `saidas/` ou subpastas específicas — nunca sobrescrever sem confirmar
- Skills novas criadas pelo usuário vão em `.claude/skills/<nome>/SKILL.md` (local) ou `~/.claude/skills/<nome>/SKILL.md` (global)

---

## Perfil do operador — João (Freelancer)

**Quem sou:** João, freelancer de criação e venda de sites. Trabalho solo com pequenos negócios e lojas locais.

**O que entrego:** Sites completos em HTML/CSS/JS — do briefing à entrega ao cliente.

**Clientes ativos:**
- LSSTORE — loja de roupas (masculino e feminino, público 20-40 anos). Site pronto em `index.html`.

**Como trabalho:** Crio o site, apresento ao cliente e fecho a venda. Foco em automatizar o que for repetitivo.

**Regras do sistema:**
- Cliente novo → criar pasta `clientes/<Nome>/` com `briefing.md`
- Proposta → `clientes/<Nome>/proposta.html`
- Site entregue → mover para `saidas/<Nome>/`
