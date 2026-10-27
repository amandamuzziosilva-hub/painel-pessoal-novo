# CLAUDE.md — Sistema Pessoal (painel da Amanda)

## O que é
Painel pessoal em um único arquivo `index.html` (HTML + CSS + JS puro, sem build),
publicado pelo GitHub Pages a partir da branch `main`.
Os dados ficam numa planilha Google Sheets; o `index.html` conversa com ela por um
app da Web do Apps Script (`code.gs`), que NÃO está neste repositório.
Este arquivo (CLAUDE.md) é também o resumo do painel: não existe um resumo separado.

## Sobre a Amanda
- A Amanda não é programadora. Explique tudo em português simples, passo a passo, sem jargão.
- Quando precisar de um termo técnico, explique em uma frase.
- Escreva com "está" em vez de "tá" e sem outras contrações informais.
- Ao explicar uma mudança, descreva o que muda na tela.

## Regras importantes
- **O repositório é público.** Nunca colocar no código: URLs reais do Apps Script,
  senhas/chaves, dados de pacientes, valores financeiros ou dados de saúde reais.
  URL e senha ficam só no localStorage do aparelho (`sp-url`, `sp-token`, `pp_url`, `pp_key`).
  Em qualquer arquivo, use marcadores como SUA_CHAVE_AQUI ou SUA_URL_DO_APPS_SCRIPT_AQUI.
- **Dados iniciais de exemplo devem ser sempre fictícios, nunca reais.**
  Vale para `FIN_DEF`, `finSeed`, `planSeed` e qualquer outra carga inicial.
- Dados de pacientes vêm de OUTRO painel (painel de pacientes), só leitura, via
  `?p=pendencias&k=<chave>`. Nunca gravar dados de paciente na planilha pessoal.
- Não adicionar dependências nem etapa de build. Única lib externa: xlsx 0.18.5 (cdnjs).
- Textos da interface em português do Brasil, tom simples e direto.
- Antes de ler o código, leia este arquivo. Se uma regra não estiver aqui nem no código,
  pergunte à Amanda antes de inventar.
- Teste antes de entregar.

## Mapa do index.html
- `SCHEMA` (~l.572): campos de cada coleção (k=chave, l=rótulo, t=tipo, al=nomes alternativos de coluna).
- `COLS`: coleções = abas da planilha. Adicionar coleção nova exige mudança no `code.gs` também.
- `TABS` / `NAVDEF` / `ARQMAP`: abas do menu, grupos e quais coleções cada aba usa.
  A ordem escolhida pela Amanda (Ajustes › Ordem do menu) fica em `painel_ajustes`, id `menu`,
  e é aplicada por `navDef()`. Início fica sempre no topo e Arquivo no fim.
- Planejamento (`colTasks`): tarefas com prazo vêm primeiro, da mais próxima de vencer para a
  mais distante, e arrastar não muda a posição delas. Tarefas sem prazo vêm depois, na ordem em
  que foram arrastadas (campo `ordem`). "Limpar concluídas" apaga as concluídas de vez.
- `api()`: POST para o Apps Script com `{action, token, ...}`; resposta `{ok, dados, versao}` ou `{ok:false, erro}`.
- `versao` do script libera recursos: ≥3 Planejamento, ≥4 Finanças, ≥5 Arquivo.
- Cache offline: `sp-cache`; alterações não enviadas: `sp-pending`.
- Uma função `viewXxx()` por aba (viewInicio, viewInv, viewDiario, viewSaude, viewRotina,
  viewViagens, viewProjetos, viewFinancas, viewPlan, viewArquivo).
- `diario` e `rotina` são guardados um dia por linha na planilha e agrupados por mês no painel.
- Ícones na raiz: `icon-64.png` (aba do navegador), `icon-180.png` (iPhone), `icon-192.png` e
  `icon-512.png` (Android, via `manifest.webmanifest`). Arte: "am" em Instrument Serif itálico
  sobre o degradê do `.avatar`.

## Apps Script (code.gs)
- O `code.gs` não está neste repositório e não deve ser adicionado sem a Amanda pedir.
- Só proponha mudança no `code.gs` quando for realmente necessário. Antes de propor,
  peça que a Amanda cole o código atual do Apps Script na conversa. Depois, entregue o arquivo
  completo, nunca trechos soltos.
- Quando houver mudança no `code.gs`, avise com destaque. O Code não consegue atualizar o Apps Script.
  A Amanda precisa: (1) abrir a planilha > Extensões > Apps Script; (2) selecionar tudo e colar o código novo;
  (3) salvar; (4) Implantar > Gerenciar implantações > lápis > Versão: Nova versão > Implantar.
  O endereço continua o mesmo.
- Se mudar o formato dos dados, avise que o `code.gs` no Google também precisa ser atualizado.

## Backup
- Antes de mudanças que mexem nos dados (formato das coleções, importações, regravações),
  lembre a Amanda de exportar um backup da planilha.

## Como testar
Abrir o `index.html` no navegador. Sem planilha conectada aparece a tela "Primeiro acesso".
Conferir em largura de celular (barra inferior `#dock`) e de computador (menu lateral).

## Fluxo de trabalho
- Trabalhar em branch e só juntar na `main` (que publica o site) com o OK da Amanda.
- **Como tratar cada pedido de alteração** (para gastar poucos recursos):
  1. **Resuma primeiro.** Antes de mexer em qualquer coisa, escreva em poucas linhas, em português simples, o que você entendeu do pedido.
  2. **Alteração pequena** (um ícone, botão, texto, função simples, ajuste estrutural ou melhoria no código sem mudar a aparência): não precisa de mockup. Depois do resumo, faça direto. Só pare para perguntar se o pedido estiver ambíguo.
  3. **Alteração grande** (principalmente visual: layout, nova tela, mudança de cores ou da estrutura de uma tela): depois do resumo, pergunte se a Amanda prefere ver um mockup ou fazer a alteração direto. Recomende o mockup. Se a Amanda disser que você entendeu certo e pedir para seguir, faça direto.
  4. **Mockup, quando pedido:** use dados fictícios e mostre antes e depois em largura de celular e de computador, ou uma prévia clicável privada. Só altere o código depois do OK.
  5. **Mudança no formato dos dados:** explique em palavras simples o que vai mudar e espere o OK antes de alterar.
- Ao terminar, diga em poucas linhas o que mudou e o que a Amanda precisa fazer para colocar no ar.
- Se uma regra ou parte do mapa mudar, atualize este arquivo.
