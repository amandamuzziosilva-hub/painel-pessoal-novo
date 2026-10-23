# Sistema Pessoal — painel da Amanda

## O que é
Painel pessoal em um único arquivo `index.html` (HTML + CSS + JS puro, sem build),
publicado pelo GitHub Pages a partir da branch `main`.
Os dados ficam numa planilha Google Sheets; o `index.html` conversa com ela por um
app da Web do Apps Script (`code.gs`), que NÃO está neste repositório.

## Regras importantes
- **O repositório é público.** Nunca colocar no código: URLs reais do Apps Script,
  senhas/chaves, dados de pacientes, valores financeiros ou dados de saúde reais.
  URL e senha ficam só no localStorage do aparelho (`sp-url`, `sp-token`, `pp_url`, `pp_key`).
- **Dados iniciais de exemplo devem ser sempre fictícios, nunca reais.**
  Vale para `FIN_DEF`, `finSeed`, `planSeed` e qualquer outra carga inicial.
- Dados de pacientes vêm de OUTRO painel (painel de pacientes), só leitura, via
  `?p=pendencias&k=<chave>`. Nunca gravar dados de paciente na planilha pessoal.
- Não adicionar dependências nem etapa de build. Única lib externa: xlsx 0.18.5 (cdnjs).
- Textos da interface em português do Brasil, tom simples e direto.
- A Amanda não é programadora: explicar mudanças em linguagem simples e
  descrever o que muda na tela.

## Mapa do index.html
- `SCHEMA` (~l.572): campos de cada coleção (k=chave, l=rótulo, t=tipo, al=nomes alternativos de coluna).
- `COLS`: coleções = abas da planilha. Adicionar coleção nova exige mudança no `code.gs` também.
- `TABS` / `NAVDEF` / `ARQMAP`: abas do menu, grupos e quais coleções cada aba usa.
- `api()`: POST para o Apps Script com `{action, token, ...}`; resposta `{ok, dados, versao}` ou `{ok:false, erro}`.
- `versao` do script libera recursos: ≥3 Planejamento, ≥4 Finanças, ≥5 Arquivo.
- Cache offline: `sp-cache`; alterações não enviadas: `sp-pending`.
- Uma função `viewXxx()` por aba (viewInicio, viewInv, viewDiario, viewSaude, viewRotina,
  viewViagens, viewProjetos, viewFinancas, viewPlan, viewArquivo).
- `diario` e `rotina` são guardados um dia por linha na planilha e agrupados por mês no painel.
- Ícones na raiz: `icon-64.png` (aba do navegador), `icon-180.png` (iPhone), `icon-192.png` e
  `icon-512.png` (Android, via `manifest.webmanifest`). Arte: "am" em Instrument Serif itálico
  sobre o degradê do `.avatar`.

## Como testar
Abrir o `index.html` no navegador. Sem planilha conectada aparece a tela "Primeiro acesso".
Conferir em largura de celular (barra inferior `#dock`) e de computador (menu lateral).

## Fluxo de trabalho
- Trabalhar em branch e só juntar na `main` (que publica o site) com o OK da Amanda.
- Mudanças visuais (design, layout, cores, novos elementos de tela): mostrar antes um
  mockup com dados fictícios (imagens antes/depois em celular e computador, ou prévia
  clicável privada) e só alterar o código depois do OK da Amanda.
- Se mudar o formato dos dados, avisar que o `code.gs` no Google também precisa ser atualizado.
