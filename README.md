# Apuração 2º Turno 2026

Painel ao vivo da apuração do 2º turno das eleições de 2026, com os números oficiais do TSE em tempo real.
Acompanha duas disputas: **Presidente** e **Governador do Distrito Federal**.

## O que o painel mostra

- Votos e percentuais de cada candidato e a porcentagem de urnas apuradas, atualizados a cada 5 segundos.
- Urnas apuradas por região no painel de Presidente, da região menos apurada para a mais apurada.
- Projeção do resultado em uma frase curta ("A projeção indica vitória de X."), a partir de 30% das urnas apuradas.
  Para Presidente, a projeção é feita estado por estado, usando o 1º turno onde ainda não há urnas apuradas.
  Para Governador do DF, segue a proporção atual.
- Aviso de "Matematicamente decidido" quando nem todos os votos que ainda faltam alcançam o líder.
  A partir daí, a projeção some e o quadro verde mostra o nome do eleito.
- Um símbolo indica quando chegam números novos do TSE, e o horário da última leitura fica sempre visível.
- Botão "Compartilhar no WhatsApp" fixo no rodapé.

Os horários são sempre exibidos no fuso de Brasília.

## Dados e robustez

Os dados vêm do site oficial de resultados do TSE (`resultados.tse.jus.br/oficial/ele2026`), lidos direto pelo navegador.

- Se o TSE falhar, a página continua tentando e espaça as tentativas, sem travar.
- Leituras antigas vindas do cache do TSE são ignoradas.
- Quando a leitura falha seguidamente, o painel avisa que os números podem estar desatualizados.

## Versão e aviso de atualização

A versão aparece no topo da página, ao lado do status da conexão (`#versao` no `index.html`), e também fica em `versao.json`.
Os dois valores precisam ser sempre iguais.
A página consulta o `versao.json` a cada minuto e mostra um aviso com botão de atualizar quando há versão nova publicada.

## Como executar

É uma página única, sem build e sem dependências. Basta abrir o `index.html` no navegador ou servir a pasta:

```
python -m http.server 8000
```

e acessar `http://localhost:8000`.

## Estrutura

```
index.html             página completa (HTML, CSS e JavaScript no mesmo arquivo)
versao.json            versão publicada, igual à do index.html
favicon.svg            ícone da aba
apple-touch-icon.png   ícone para a tela inicial do celular
```

## Código-fonte e publicação

- **Código-fonte:** repositório no GitHub, https://github.com/lecoibr/apuracao-tse-2026
- **Publicação:** GitHub Pages, em https://lecoibr.github.io/apuracao-tse-2026/
