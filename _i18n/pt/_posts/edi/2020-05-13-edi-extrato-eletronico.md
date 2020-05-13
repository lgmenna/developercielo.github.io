---
layout: tutorial
title:  EDI - Extrato Eletrônico
description: Instruções EDI
toc_footers: true
categories: tutorial
sort_order: 7
tags:
  - EDI Cielo
---

# Introdução

# Ambiente

|Ambiente|Endereço|
|---|---|
|Sandbox|https://api2.cielo.com.br/sandbox/edi-api/v2/|
|Homologação|api2.cielo.com.br|https://apihom-cielo.sensedia.com/edi-api/v2/|

# Operações

## **POST** Registrar merchantID

Registre o ID do lojista(apenas um, uma lista ou todos), com base no número da entidade.

### Request

> **POST** {{host}}/edi/registers
>
> **Headers**
>
>| Key | Value |
>|---|---|
>| **externalID** |Valor gerado pelo cliente para evitar solicitação duplicada. Esse valor pode ser usado para obter solicitação. Não use o mesmo valor para novas solicitações.|

```

{
  "mainMerchantId": "9999111222",
  "merchants": [
    "9999111111",
    "9999111333"
  ],
  "merchantEMail": "customer@customer.com",
  "type": [
    "SELL"
  ]
}

```

### Response

```

{
  "legalEntityNumber": 1234567890,
  "mainMerchantId": 2008983,
  "registerID": 12345,
  "merchants": [
    823958412384701,
    679809436576210
  ],
  "type": [
    "SELL"
  ],
  "status": "PROCESSING"
}

```

## **GET** Consultar o Status do Registro

Recupere o status de registro do EDI.

### Response

> **GET** {{host}}/edi/registers/{registerID}
>
> **Headers**
>
>| Key | Value |
>|---|---|
>| **registerID** |Valor usado para recuperar o status do registro.|

```

{
  "legalEntityNumber": "01234567890",
  "registerID": 12345,
  "merchants": [
    9999222111,
    9999222222
  ],
  "status": "PROCESSING"
}

```

## **GET** Consulta o Registro do merchantID

Consulte o MerchantID com base no registerID ou no mainMerchantID.

### Response

> **GET** {{host}}/edi
>
> **Headers**
>
>| Key | Value |
>|---|---|
>| **externalID** |Valor gerado pelo cliente para evitar solicitação duplicada. Esse valor pode ser usado para obter solicitação. Não use o mesmo valor para novas solicitações.|
>| **registerID** |O registerID é fornecido pela operação /edi/registers ao se registrar. Pode ser usado no lugar de mainMerchantID, se preferir (apenas um precisa ser informado).|
>| **mainMerchantID** |A maneira mais comum de recuperar um registro EDI na empresa. Pode ser usado em vez de registerID, principalmente em casos de registro não realizados por /edi/registers.|

```

{
  "legalEntityNumer": "12314314",
  "mainMerchantId": 9999111222,
  "registerID": 12345,
  "merchants": [
    9999111111,
    9999111333
  ],
  "type": [
    "SELL"
  ],
  "acknowledge": "COMPLETED"
}

```

## **PUT** Atualizar o Registro do merchantID

Atualize o merchantID com base no registerID ou mainMerchantID.

### Request

> **PUT** {{host}}/edi
>
> **Headers**
>
>| Key | Value |
>|---|---|
>| **externalID** |Valor gerado pelo cliente para evitar solicitação duplicada. Esse valor pode ser usado para obter solicitação. Não use o mesmo valor para novas solicitações.|

```

{
  "registerID": "string",
  "mainMerchantId": "9999222333",
  "merchants": [
    "9999222111",
    "9999222222"
  ],
  "type": [
    "SELL"
  ]
}

```

### Response

```

{
  "legalEntityNumber": "01234567890",
  "mainMerchantId": 9999111222,
  "registerID": 12345,
  "merchants": [
    9999222333,
    9999111222
  ],
  "type": [
    "SELL"
  ]
}

```

## **DELETE** Excluir o Registro do merchantID

Consulte o MerchantID com base no registerID ou no mainMerchantID.

### Response

> **DELETE** {{host}}/edi
>
> **Headers**
>
>| Key | Value |
>|---|---|
>| **externalID** |Valor gerado pelo cliente para evitar solicitação duplicada. Esse valor pode ser usado para obter solicitação. Não use o mesmo valor para novas solicitações.|
>| **registerID** |O registerID é fornecido pela operação /edi/registers ao se registrar. Pode ser usado no lugar de mainMerchantID, se preferir (apenas um precisa ser informado).|
>| **mainMerchantID** |A maneira mais comum de recuperar um registro EDI na empresa. Pode ser usado em vez de registerID, principalmente em casos de registro não realizados por /edi/registers.|

```

{
  "legalEntityNumer": "12314314",
  "mainMerchantId": 9999111222,
  "registerID": 12345,
  "acknowledge": "COMPLETED"
}

```

## **GET** Consulta a Lista de Filiais

Executa a lista de clientes abaixo do access_token informado. O filtro pode ser usado para listar apenas aqueles disponíveis ou indisponíveis. Indisponível significa que a filial já participa de outro registro. Nos serviços de registro e edição, há validação para permitir que apenas aqueles disponíveis sejam informados.

### Response

> **GET** {{host}}/edi/merchantgroup
>
> **Headers**
>
>| Key | Value |
>|---|---|
>| **externalID** |Valor gerado pelo cliente para evitar solicitação duplicada. Esse valor pode ser usado para obter solicitação. Não use o mesmo valor para novas solicitações.|
>| **registerID** |O registerID é fornecido pela operação /edi/registers ao se registrar. Pode ser usado no lugar de mainMerchantID, se preferir (apenas um precisa ser informado).|
>| **mainMerchantID** |A maneira mais comum de recuperar um registro EDI na empresa. Pode ser usado em vez de registerID, principalmente em casos de registro não realizados por /edi/registers.|

```

{
  "legalEntityNumber": "string",
  "branches": [
    {
      "merchantID": "9999111222",
      "legalEntityNumber": "01234567890",
      "businessName": "V",
      "status": "UNAVAILABLE"
    }
  ]
}

```