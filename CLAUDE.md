# Apuração TSE 2026 — instruções do projeto

Projeto pessoal: painel ao vivo da apuração do 2º turno de 2026 (Presidente e Governador do DF), com os dados
do TSE em tempo real. **Não é jogo.**
Valem as preferências globais do `~/.claude/CLAUDE.md`. Aqui ficam só o que é específico deste projeto
ou o que muda em relação a elas.

## Commits e repositório

- **Com prefixo** `[APURACAO-2026] - ` em toda mensagem (a branch é `main`, mas o histórico inteiro usa esse
  identificador). Descrição no imperativo, em português, primeira linha curta; corpo quando precisar explicar.
  Exemplo: `[APURACAO-2026] - Esconde a projeção quando o resultado já está decidido`.
- Nenhuma menção a IA ou assistente em commit, código, comentário ou documento (sem `Co-Authored-By`).
- Repositório no **GitHub** (`lecoibr/apuracao-tse-2026`), publicado pelo GitHub Pages. É a exceção à
  preferência geral de usar só o Bitbucket.

## Processo antes de todo commit

1. Varreduras completas em ciclo (agente `qa-jogos-senior`, que cobre página no navegador real e falhas de
   rede): cada varredura vai até o fim, anota todos os erros, corrige todos; se houve erro, nova varredura da
   página inteira; para só quando uma sair limpa. Conferir sempre o comportamento com a leitura do TSE falhando.
2. Gate do `analista-sonar-senior`. Se ele alterar código, a varredura recomeça do zero.
3. Subir a versão (em `#versao` e em `versao.json`), commit e push.

Agentes que cabem aqui: `designer-ui-layout-jogos-senior` (telas, textos, celular, acessibilidade) e
`arquiteto-engenharia-jogos-senior` (leitura dos dados ao vivo, polling, publicação). Os agentes de gameplay e
de arte/áudio não se aplicam. Apito (`[console]::beep(880,300)`) ao terminar ou ao precisar de decisão.

## Stack

- Página única `index.html` (HTML, CSS e JavaScript inline), sem build, sem dependências e sem `package.json`.
  Arquivos ao lado: `versao.json`, `favicon.svg` e `apple-touch-icon.png`.
- A consulta ao TSE roda a cada 5 segundos, resiliente a falhas: o laço continua vivo após erro, espaça as
  tentativas em falha, ignora cópia antiga do CDN e sinaliza painel desatualizado. Status de falha: "Falha TSE".
- **Versão:** `#versao` no `index.html` e `versao.json` têm de ser **sempre iguais**. A página compara as duas
  e mostra o aviso com botão de atualização quando há versão nova publicada.

## Decisões já tomadas (não reabrir)

- **Tela limpa acima de tudo**: nada de quadro extra nem texto explicativo longo; informação secundária em uma
  linha curta ou texto pequeno.
- Projeção para Presidente (estado por estado, usando o 1º turno onde ainda não há urnas apuradas) e para
  Governador do DF (proporção atual). Na tela fica só a frase "A projeção indica vitória de X.", sem título,
  percentuais nem nota.
- "Matematicamente decidido" só quando nem o teto de votos que ainda podem entrar alcança o líder; campo
  ausente ou ilegível nunca declara resultado. Com o resultado decidido, a projeção some e quem está decidido
  é tratado como eleito; o quadro verde mostra só o nome do eleito.
- Sem animação que gire sem parar (causou vertigem): indicador estático ou pulso discreto, respeitando
  `prefers-reduced-motion`. O círculo animado virou o horário da última leitura.
- Horários sempre no fuso de Brasília (offset `-03:00` e `timeZone: "America/Sao_Paulo"`).
- Botão "Compartilhar no WhatsApp" fixo no rodapé da tela, com espaço inferior para não cobrir o último painel.
- Regiões ordenadas da menor para a maior apuração; símbolo indica quando chegam números novos do TSE.
- Removidos: contador de pessoas on-line e aviso de votação estendida no rodapé.
