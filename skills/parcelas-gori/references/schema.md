# Formato de dados — data/obras/<slug>.json

```json
{
  "obra": "Condomínio Solar das Palmeiras",
  "sindico": "João",
  "endereco": "",
  "valor_total": 45000.00,
  "parcelas": [
    {
      "numero": 1,
      "descricao": "Entrada",
      "valor": 13500.00,
      "vencimento": "2026-08-10",
      "status": "pago",
      "data_pagamento": "2026-08-09",
      "nf_emitida": true
    },
    {
      "numero": 2,
      "descricao": "2/3",
      "valor": 15750.00,
      "vencimento": "2026-09-10",
      "status": "pendente",
      "data_pagamento": null,
      "nf_emitida": false
    }
  ]
}
```

- status: sempre "pago" ou "pendente" (nunca outro valor).
- Datas sempre em formato AAAA-MM-DD.
- nf_emitida só é relevante quando status é "pago".
- Ao calcular saldo restante da obra: soma de todas as parcelas com status: "pendente".
