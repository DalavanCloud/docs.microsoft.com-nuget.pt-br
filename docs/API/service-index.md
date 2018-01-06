---
title: "Índice de serviço, o NuGet API | Microsoft Docs"
author:
- joelverhagen
- kraigb
ms.author:
- joelverhagen
- kraigb
manager: skofman
ms.date: 10/26/2017
ms.topic: reference
ms.prod: nuget
ms.technology: 
ms.assetid: 2f6d6cf2-53fb-417a-b1d8-e0ac591c1699
description: "O índice de serviço é o ponto de entrada da API de HTTP do NuGet e enumera os recursos do servidor."
keywords: Ponto de entrada de API do NuGet, descoberta de ponto de extremidade NuGetA PI
ms.reviewer:
- karann
- unnir
ms.openlocfilehash: 0c43a09d8564964bd0140b9ac5deb9d3063e4dc5
ms.sourcegitcommit: d0ba99bfe019b779b75731bafdca8a37e35ef0d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/14/2017
---
# <a name="service-index"></a>Índice de serviço

O índice de serviço é um documento JSON que é o ponto de entrada para uma origem de pacote do NuGet e permite uma implementação de cliente descobrir os recursos da origem do pacote. O índice de serviço é um objeto JSON com duas propriedades obrigatórias: `version` (a versão do esquema do índice de serviço) e `resources` (pontos de extremidade ou recursos da origem do pacote).

índice de serviço do NuGet.org está localizado aqui:
```
https://api.nuget.org/v3/index.json
```

## <a name="versioning"></a>Controle de versão

O `version` valor é uma cadeia de caracteres de versão pode ser analisado SemVer 2.0.0 que indica a versão do esquema do índice de serviço.
A API exige que a cadeia de caracteres de versão tem um número de versão principal do `3`. Como as alterações recentes não são feitas para o esquema de índice de serviço, versão secundária da cadeia de caracteres de versão será aumentado.

Cada recurso no índice de serviço é com controle de versão independentemente do serviço de versão de esquema de índice.

A versão atual do esquema é `3.0.0-beta.1`.

## <a name="http-methods"></a>Métodos HTTP

O índice de serviço está acessível por meio de métodos HTTP `GET` e `HEAD`.

## <a name="resources"></a>Recursos

O `resources` propriedade contém uma matriz de recursos com suporte por essa origem de pacote.

### <a name="resource"></a>Recurso

Um recurso é um objeto na `resources` matriz. Representa um recurso com versão de uma origem do pacote. Um recurso tem as seguintes propriedades:

Nome          | Tipo   | Necessária | Observações
------------- | ------ | -------- | -----
@id           | cadeia de caracteres | sim      | A URL para o recurso
@type         | cadeia de caracteres | sim      | Uma constante de cadeia de caracteres que representa o tipo de recurso
comment       | cadeia de caracteres | no       | Uma descrição legível do recurso

O `@id` é uma URL que deve ser absoluto e deve ter o esquema HTTP ou HTTPS.

O `@type` é usado para identificar o protocolo específico a ser usado ao interagir com o recurso. O tipo do recurso é uma cadeia de caracteres opaca mas geralmente tem o formato:

```
{RESOURCE_NAME}/{RESOURCE_VERSION}
```

Os clientes devem codificar o `@type` valores que compreendem e observá-los no índice de serviço da origem do pacote. Exato `@type` valores em uso hoje em dia são enumerados nos documentos de referência de recursos individuais listados no [visão geral da API](overview.md#resources-and-schema).

Para manter esta documentação, a documentação sobre recursos diferentes essencialmente será agrupada pelo `{RESOURCE_NAME}` encontrado no índice de serviço que é análogo à agrupamento por cenário. 

Não há nenhum requisito de que cada recurso tem uma única `@id` ou `@type`. Cabe a implementação do cliente para determinar qual recurso preferir em detrimento de outro. Uma possível implementação é que recursos de idênticos ou compatíveis `@type` pode ser usado em um modo round robin no caso de erro de servidor ou de falha de conexão.

### <a name="sample-request"></a>Solicitação de amostra

```
GET https://api.nuget.org/v3/index.json
```

### <a name="sample-response"></a>Resposta de exemplo

[!code-JSON [service-index.json](./_data/service-index.json)]