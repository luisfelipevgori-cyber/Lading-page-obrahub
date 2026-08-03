# Rotina diária de aviso de vencimento (configurar manualmente uma vez)

Isto NÃO é configurado pela skill automaticamente — é feito uma vez pelo usuário em claude.ai/code/routines (ou pelo comando /schedule no Claude Code).

## Como criar
1. Acessar a área de Routines do Claude Code (link "Rotinas"/"Schedule" na aba Code).
2. Criar nova rotina, apontando para este mesmo repositório.
3. Frequência: diária (sugestão: 8h da manhã, horário de Brasília).
4. Conectar o Gmail (conector já disponível) para que a rotina possa enviar o e-mail de aviso.
5. Colar o prompt abaixo como instrução da rotina.

## Prompt da rotina
```
Leia todos os arquivos em data/obras/*.json deste repositório. Para cada obra, identifique parcelas com status "pendente" cujo vencimento é hoje, já passou, ou está a até 5 dias de distância. Se houver pelo menos uma parcela nessa condição, envie um e-mail para construtoragori@gmail.com com assunto "Parcelas — aviso do dia" e corpo listando, por obra: nome da obra, número da parcela, valor, data de vencimento e se está atrasada ou vencendo. Se não houver nenhuma parcela nessa condição, não envie e-mail. Não altere nenhum arquivo do repositório nesta execução.
```

## Observação
Se o usuário ainda não tiver o Gmail conectado como connector dentro da rotina (separado da conexão do GitHub), a rotina falha silenciosamente ao tentar enviar — nesse caso, sugerir reconectar o Gmail na configuração da rotina.
