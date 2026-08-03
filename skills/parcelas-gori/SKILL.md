---
name: parcelas-gori
description: Controla parcelas, vencimentos e saldo de cada obra da Construtora Gori, mantendo um arquivo de dados por obra e um painel consolidado, e sinalizando parcelas vencendo, atrasadas ou notas fiscais pendentes. USE SEMPRE que o usuário mencionar parcela, vencimento, saldo, pagamento de obra/condomínio, pedir para cadastrar uma obra nova, atualizar um pagamento recebido, ver o painel de parcelas, ou perguntar "como está a obra X" / "quanto falta da obra X" no contexto da Construtora Gori. Não use para orçamentos comerciais nem para o gerador de contratos (skills separadas).
---

# Parcelas Gori

Controle financeiro de parcelas por obra da Construtora Gori. Cada obra tem um arquivo de dados próprio; o painel consolidado é gerado a partir desses arquivos.

## Onde os dados ficam
- Um arquivo por obra em data/obras/<slug-da-obra>.json (slug = nome da obra/condomínio em minúsculas, sem espaço, ex: solar-das-palmeiras.json).
- Ver references/schema.md para o formato exato do JSON.
- O painel consolidado é gerado em data/painel-parcelas.md — sempre regenerado a partir dos arquivos de obra, nunca editado manualmente.

## Processo — cadastrar obra nova
1. Perguntar: nome da obra/condomínio, síndico, valor total, número de parcelas, valor e data de vencimento de cada parcela, forma de pagamento da entrada (se houver), e se a NF de cada parcela já foi/será emitida.
2. Criar data/obras/<slug>.json seguindo references/schema.md.
3. Regenerar data/painel-parcelas.md.
4. Confirmar ao usuário o resumo do que foi cadastrado.

## Processo — registrar pagamento recebido
1. Usuário informa qual obra e qual parcela foi paga (e a data, se diferente de hoje).
2. Atualizar o campo status da parcela correspondente no JSON da obra para "pago" e preencher data_pagamento.
3. Regenerar data/painel-parcelas.md.
4. Confirmar o novo saldo restante da obra.

## Processo — consultar (ex: "como está a obra X")
1. Ler data/obras/<slug>.json da obra perguntada.
2. Responder direto: parcela atual, valor pago até agora, saldo restante, próxima data de vencimento, e se há parcela atrasada ou NF pendente.
3. Não é necessário regenerar o painel só para uma consulta.

## Regras de alerta (usar tanto no painel quanto em rotinas automáticas)
- Vencendo: parcela com status: "pendente" e vencimento em até 5 dias a partir de hoje.
- Atrasada: parcela com status: "pendente" e vencimento já passado.
- NF pendente: parcela com status: "pago" mas nf_emitida: false — sinalizar como pendência administrativa (lembrar que a emissão da NF em si não pode ser feita por aqui, só o alerta).

## Painel consolidado
Gerar/atualizar data/painel-parcelas.md com uma tabela por obra: obra, parcela atual (ex: "3/6"), saldo restante, próximo vencimento, status (em dia / vencendo / atrasada), e uma seção no topo "⚠ Atenção esta semana" listando toda parcela vencendo ou atrasada em qualquer obra.

## Rotina automática (aviso proativo)
Este skill assume que existe uma Claude Code Routine diária configurada separadamente (ver references/rotina-diaria.md) que roda este mesmo processo de consulta em todas as obras e envia um resumo por e-mail quando há algo vencendo ou atrasado. A skill aqui só precisa manter os dados corretos para que a rotina funcione — não precisa configurar a rotina sozinha.
