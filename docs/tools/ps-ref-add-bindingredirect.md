---
title: Referência do PowerShell do NuGet BindingRedirect adicionar
description: Referência de comando do PowerShell Add-BindingRedirect no Console do Gerenciador de pacotes do NuGet no Visual Studio.
author: kraigb
ms.author: kraigb
manager: douge
ms.date: 12/07/2017
ms.topic: reference
ms.openlocfilehash: 7f1f2ef23e54ee48b577a2796b7f7b5f4c7eb284
ms.sourcegitcommit: 3eab9c4dd41ea7ccd2c28bb5ab16f6fbbec13708
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/26/2018
---
# <a name="add-bindingredirect-package-manager-console-in-visual-studio"></a>Add-BindingRedirect (Console do Gerenciador de Pacotes no Visual Studio)

*Disponível apenas dentro de [NuGet Package Manager Console](package-manager-console.md) no Visual Studio no Windows.*

Examina todos os assemblies no caminho de saída para um projeto e adiciona redirecionamentos de associação para o arquivo de configuração do aplicativo ou da web, quando necessário. Esse comando é executado automaticamente quando a instalação de um pacote.

Para obter detalhes sobre associação redirecionamentos e por que eles são usados, consulte [Redirecting Assembly Versions](/dotnet/framework/configure-apps/redirect-assembly-versions) na documentação do .NET.

## <a name="syntax"></a>Sintaxe

```ps
Add-BindingRedirect [-ProjectName] <string> [<CommonParameters>]
```

## <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --- | --- |
| ProjectName | (Obrigatório) O projeto ao qual adicionar redirecionamentos de associação. A opção - ProjectName em si é opcional. |

Nenhum desses parâmetros aceitar caracteres curinga ou de entrada do pipeline.

## <a name="common-parameters"></a>Parâmetros comuns

`Add-BindingRedirect` suporta as seguintes [parâmetros comuns do PowerShell](http://go.microsoft.com/fwlink/?LinkID=113216): depuração, ação de erro, ErrorVariable, OutBuffer, OutVariable, PipelineVariable, WarningAction, detalhado e WarningVariable.

## <a name="examples"></a>Exemplos

```ps
Add-BindingRedirect MyProject

Add-BindingRedirect -ProjectName MyProject
```