# Especificação do front do novo Central

Documento de handoff para construir o front em Flutter a partir do protótipo. Autor: Diego Loureiro, Gerente de Inovação. Versão 1.0, 17/11/2026.

| Artefato | Link | Papel |
|---|---|---|
| Protótipo navegável | https://diegoloureiro84.github.io/prototipo-central/ | Fonte da verdade de layout, comportamento e texto |
| Design System 2.0 | https://diegoloureiro84.github.io/coplan-design-system/ | Tokens, componentes, padrões e regras |
| Este documento | `ESPECIFICACAO-FRONT.md` neste repositório | Roteiro, regras de negócio, critérios de aceite e dados |

Ordem de precedência quando houver conflito: **protótipo, depois Design System, depois este documento**. Divergência encontrada deve ser registrada como pergunta, não resolvida por conta própria.

## 1. Escopo

**Navegável e dentro do escopo desta entrega**

- Login, escolha de sistema, Início de cada sistema.
- Estrutura comum: cabeçalho, seletor de unidade gestora e exercício, troca de sistema, menu lateral aberto, recolhido e flutuante, busca (Ctrl+K), notificações, favoritos, título da aba.
- Contábil, 2.10 Empenho completo: lista com indicadores, inclusão a partir de NAD, inclusão avulsa, edição, detalhe, linha do tempo com documentos, extrato de liquidações.
- Planejamento, Especificação da Receita: lista em árvore, inclusão, edição, visualização e validações. É o **modelo para todos os cadastros básicos em hierarquia**.

**Fora do escopo**: as demais telas do menu existem só como navegação (placeholder). Administrativo, Servidor, Tributos e os outros sistemas mostram aviso de que não são navegáveis no protótipo.

## 2. Como usar este material com IA

1. Dê à IA o código-fonte do protótipo (no navegador: Exibir código-fonte, ou o `index.html` deste repositório). É um arquivo único com CSS, JS e dados de exemplo.
2. Os tokens estão no `:root` do CSS e na seção Tokens do Design System, já com a versão Flutter (`CoplanColors`, `CoplanRadius`, `CoplanLayout`).
3. A fonte é Exo 2, embutida no HTML. No Flutter, use o pacote da fonte em arquivo local, não Google Fonts em tempo de execução.
4. Os ícones são Lucide. No Flutter, use o pacote `lucide_icons` ou equivalente com os mesmos nomes.
5. Peça à IA para gerar tela por tela, comparando com o protótipo aberto lado a lado, na ordem do roteiro abaixo.

## 3. Roteiro passo a passo

Base dos links: `https://diegoloureiro84.github.io/prototipo-central/`. Todos os links abrem direto na tela, sem login.

Para o fluxo completo desde o login, use o CPF de teste `529.982.247-25` e qualquer senha com 4 ou mais caracteres.

| # | Ação | Link | Resultado esperado |
|---|---|---|---|
| 1 | Entrar com o CPF de teste | `#login` | Vai para a escolha de sistema. Aba: `Sistemas · Coplan` |
| 2 | Clicar no cartão Contábil | `#sistemas` | Abre o Início do Contábil. Aba: `Início · Contábil · Coplan` |
| 3 | Clicar no nome da unidade no cabeçalho | `#central` | Abre o seletor com 4 unidades na ordem do cadastro, a principal primeiro, e o subtítulo igual ao Tipo de Entidade do Central |
| 4 | No seletor, clicar em `2025` | `#central` | Toast "Exercício 2025, exercício anterior: somente consulta". A pílula do cabeçalho mostra 2025 |
| 5 | Clicar no ícone de grade do cabeçalho | qualquer tela | Abre a troca de sistema no mesmo padrão do seletor de unidade |
| 6 | Recolher o menu e passar o mouse num grupo | qualquer tela | Menu com 60px. O painel flutuante lista as telas do grupo, sem ícones |
| 7 | Pressionar `Ctrl+K` | qualquer tela de sistema | Abre a busca de telas |
| 8 | Abrir Empenho | `#tela/k-2.10` | Aba `Empenho · Contábil · Coplan`. Indicadores: A empenhar (7 NADs, 2 prioritárias), Empenhado, Liquidado, Pago, A liquidar |
| 9 | Clicar no indicador A empenhar | `#tela/k-2.10` | Painel de foco "NADs aguardando empenho", da mais antiga para a mais recente. A tabela principal some |
| 10 | Clicar em Empenhar na NAD 1532 | `#tela/k-2.10/novo?pre=R-318` | Formulário com marca Prioritária, dados carregados da NAD e as abas "Detalhes do empenho" e "Linha do tempo e documentos" |
| 11 | Abrir a aba Linha do tempo | mesmo link | Mostra a linha do tempo da NAD 1532: Solicitação, Deferimento, Autorização, Processo de compra, Emissão da NAD |
| 12 | Digitar `4148` no Número | mesmo link | Erro: "O número 4148/2026 já é do empenho de Zeta Gráfica e Editora Ltda. Deixe vazio ou use uma reserva." Salvar fica bloqueado |
| 13 | Apagar o número e clicar em Salvar empenho | mesmo link | Vai para o detalhe do empenho 4149. Toast: "Empenho 4149/2026 de R$ 52.400,00 salvo. NAD atendida." |
| 14 | Abrir o detalhe de um empenho com liquidações | `#tela/k-2.10/4141?detalhes` | Faixa "Próximo passo: assinar a nota de empenho" com Nota de empenho, Anular, Liquidar e Assinar. Extrato com 2 liquidações |
| 15 | Clicar numa linha do extrato | mesmo link | Abre a linha do tempo com o documento da liquidação no visualizador |
| 16 | Abrir a linha do tempo | `#tela/k-2.10/4141` | Totais (Empenhado, Liquidado, Pago, A liquidar), eventos com documento e visualizador ao lado com assinatura |
| 17 | Editar um empenho sem assinatura | `#tela/k-2.10/4147?editar` | Abre a edição. Aba: `Editar · Empenho 4147 · Contábil · Coplan` |
| 18 | Tentar editar um empenho já assinado | `#tela/k-2.10/4148?editar` | Redireciona para o detalhe (`?detalhes`): empenho assinado não é editável |
| 19 | Alterar a descrição e clicar em Cancelar | `#tela/p-receita/37930?editar` | Barra inline "Descartar as alterações?" com Continuar editando e Descartar. Sem janela modal |
| 20 | Abrir Especificação da Receita | `#tela/p-receita` | Lista em árvore com Agrupar, filtros, colunas configuráveis e paginação |
| 21 | Editar uma conta e informar um código já existente | `#tela/p-receita/37930?editar` | Bloqueia com "Já existe a conta 1.0.0.0.00.0.0.00.00.00: RECEITAS CORRENTES." |
| 22 | Incluir uma conta | `#tela/p-receita/novo` | Formulário vazio. Aba: `Incluir · Especificação da Receita · Planeja · Coplan` |

## 4. Regras de negócio que o front precisa respeitar

**Unidade gestora e exercício**
- A lista de unidades vem do cadastro do Central, só as ativas, com a principal primeiro.
- Nome e Tipo de Entidade aparecem exatamente como estão cadastrados.
- Exercícios anteriores são somente consulta.
- Ordem do seletor de exercício: Outro, 2024, 2025, 2026.

**Empenho**
- O empenho pode nascer de uma NAD. Os dados da NAD vêm preenchidos e a NAD fica atendida ao salvar.
- A prioridade é definida pelo Administrativo na NAD. O Contábil só exibe.
- Número vazio significa numeração automática. O próximo número pula as reservas.
  - Números reservados no protótipo: 4150, 4151 e 4155.
  - Número já usado bloqueia o salvamento, com o nome do credor do empenho dono do número.
- Empenho assinado não é editável: a edição redireciona para o detalhe.
- O Resumo lateral mostra o saldo da dotação, o valor do empenho, o percentual do saldo usado e o checklist dos obrigatórios.
- Tipos: Ordinário, Estimativo e Global. O tipo Global tem parcelas.
- Certidão vencida:
  - é alerta neutro no empenho;
  - é bloqueio (vermelho) no pagamento.

**Especificação da Receita** (modelo para cadastros em árvore)
- Validações que bloqueiam:
  - código duplicado;
  - conta superior inexistente ou analítica;
  - conta sintética que tem contas abaixo dela.
- A estrutura do código segue a máscara do exercício (segmentos 1.1.1.1.2.1.1.2.2.2).
- O painel "Na hierarquia" mostra os ascendentes e os filhos diretos.

**Gerais**
- Filtros aplicam ao mudar, sem botão Aplicar.
- "Limpar filtro" só aparece com filtro ativo.
- Preferências por usuário ficam salvas:
  - colunas visíveis e ordem;
  - indicadores ocultos;
  - menu recolhido;
  - favoritos.
- Toda tela tem deep link. O sistema é inferido pela tela: `k-` é Contábil, `p-` é Planejamento.
- Título da aba no formato `Tela · Sistema · Coplan`.

## 5. Critérios de aceite: o que significa "idêntico"

Uma tela está aceita quando cumpre todos os itens abaixo:

1. **Tokens**: cores, raios, alturas (cabeçalho 65px, menu 280/60px) e tipografia exatamente como no Design System. Nenhuma cor fora da paleta.
2. **Comparação visual**: print da tela em Flutter e do protótipo, os dois em 1440×900, sobrepostos. Sem diferença perceptível de posição, tamanho, cor ou texto.
3. **Comportamento**: o passo correspondente do roteiro da seção 3 produz exatamente o resultado esperado, inclusive os textos de toast e de erro.
4. **Responsivo**: as quebras de layout seguem as do protótipo (599, 899, 1199 e 1599px).
5. **Regras visuais do DS**: vermelho só em bloqueio, sem janela modal, pílula de classificação neutra, menu aberto sem ícones, seletor padrão em todo select.
6. **Deep link e título da aba** funcionando em todas as telas da entrega.

A aceitação é feita por mim, tela a tela, contra este roteiro.

## 6. Dados que cada tela consome

Lista levantada a partir do protótipo. **Precisa ser validada contra o banco convertido** antes de virar contrato de API.

| Entidade | Campos exibidos |
|---|---|
| Unidade gestora | descrição, tipo de entidade, brasão, principal, ativa, município, UF |
| Exercício | ano, situação (corrente ou consulta) |
| NAD | número, data de recebimento, órgão e unidade, dotação, natureza da despesa, fonte, credor, processo, contrato, emenda, tipo, valor, descrição, prioridade, linha do tempo de documentos |
| Empenho | número e ano, data, tipo (Ordinário, Estimativo, Global), natureza (Normal), tipo de processo, dotação, credor, contrato, descrição, valor, nota fiscal, situação, assinaturas, valores empenhado, anulado, liquidado e pago |
| Dotação | código, órgão e unidade, funcional-programática, despesa, fonte de recurso, subelemento, saldo, vínculos exigidos (contrato) |
| Credor | nome, CPF ou CNPJ, situação das certidões |
| Liquidação | número, data, documento fiscal, bruto, anulado, líquido, situação do pagamento, ordem de pagamento |
| Reserva de número | número, ano |
| Especificação da Receita | id, código, descrição, abreviatura, tipo (Analítica, Sintética), grau, conta anterior, status no orçamento, nível TC, código TC, existe no TCE, legislação, dedução, função |

## 7. Lacunas e pendências conhecidas

1. **Nome de exibição da unidade gestora.** O Central não tem esse campo. O nome longo é truncado no cabeçalho. Proposta: criar o campo no cadastro.
2. **Grafia no cadastro.** O tipo de entidade da Câmara está cadastrado como "Pode Legislativo". O front exibe como está; a correção deve ser feita no cadastro.
3. **Exercício anterior.** O protótipo avisa "somente consulta", mas ainda permite abrir a inclusão. O front deve bloquear inclusão e edição em exercício anterior.
4. **Telas fora do escopo** do item 1 são placeholders e não devem ser inferidas pelo menu.

## 8. Perguntas em aberto

1. Qual é a stack e o formato da API do backend (linguagem, REST ou outro, autenticação)?
2. O front é Flutter web, mobile ou os dois na primeira entrega?
3. Login: CPF e senha, certificado digital e gov.br entram nesta fase?
4. Em qual repositório o front vai ficar, e qual é o fluxo de revisão?
5. Qual é o prazo por bloco: estrutura comum, Empenho, Especificação da Receita?

## 9. Governança

- O layout, os componentes e as regras visuais são definidos pelo Design System. Mudança visual entra primeiro no DS e no protótipo, depois no front.
- Dúvida de comportamento é respondida pelo protótipo. Se ele não cobrir o caso, a pergunta vem para mim antes de implementar.
- Contato: Diego Loureiro, diego.loureiro@coplan.srv.br
