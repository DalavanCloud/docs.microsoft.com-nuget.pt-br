---
title: "Referência de PowerShell do NuGet Uninstall-Package | Microsoft Docs"
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 6/1/2017
ms.topic: reference
ms.prod: nuget
ms.technology: 
ms.assetid: f4f5dc79-8e8e-4012-8986-873a5d9283d9
description: "Referência de comando do PowerShell do pacote de desinstalação no Console do Gerenciador de pacotes do NuGet no Visual Studio."
keywords: "Console do Gerenciador, comandos do Powershell do NuGet, referência do Powershell do NuGet, pacote de desinstalação do pacote NuGet"
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: 0899198a354a56615a48a1f7f428674a205f386b
ms.sourcegitcommit: a40c1c1cc05a46410f317a72f695ad1d80f39fa2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/05/2018
---
# <a name="uninstall-package-package-manager-console-in-visual-studio"></a>Uninstall-Package (Console de Gerenciador de pacotes no Visual Studio)

*Este tópico descreve o comando dentro de [NuGet Package Manager Console](Package-Manager-Console.md) no Visual Studio no Windows. Para o comando genérico do pacote de desinstalação do PowerShell, consulte o [referência do PowerShell PackageManagement](/powershell/module/packagemanagement/?view=powershell-6).*

Remove um pacote de um projeto, opcionalmente, a remoção de suas dependências. Se outros pacotes dependerem deste pacote, o comando falhará, a menos que – Force opção é especificada.

## <a name="syntax"></a>Sintaxe

```ps
Uninstall-Package [-Id] <string> [-RemoveDependencies] [-ProjectName <string>] [-Force]
    [-Version <string>] [-WhatIf] [<CommonParameters>]
```

Se outros pacotes dependerem deste pacote, o comando falhará, a menos que – Force opção é especificada.

## <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --- | --- |
| Id | (Obrigatório) O identificador do pacote a ser desinstalado. -Id switch é opcional. |
| Versão | A versão do pacote para desinstalar, padronizando para a versão atualmente instalada. |
| RemoveDependencies | Desinstale o pacote e suas dependências não usadas. Ou seja, se qualquer dependência tiver outro pacote que depende dele, ele é ignorado. |
| ProjectName | O projeto do qual deseja desinstalar o pacote, o padrão para o projeto padrão. |
| Force | Força um pacote a ser desinstalado, mesmo se outros pacotes dependem dele. |
| WhatIf | Mostra o que aconteceria quando a execução do comando sem realmente executar a desinstalação. |

Nenhum desses parâmetros aceitar caracteres curinga ou de entrada do pipeline.

## <a name="common-parameters"></a>Parâmetros comuns

`Uninstall-Package`suporta as seguintes [parâmetros comuns do PowerShell](http://go.microsoft.com/fwlink/?LinkID=113216): depuração, ação de erro, ErrorVariable, OutBuffer, OutVariable, PipelineVariable, WarningAction, detalhado e WarningVariable.

## <a name="examples"></a>Exemplos

```ps
# Uninstalls the Elmah package from the default project
Uninstall-Package Elmah

# Uninstalls the Elmah package and all its unused dependencies
Uninstall-Package Elmah -RemoveDependencies 

# Uninstalls the Elmah package even if another package depends on it
Uninstall-Package Elmah -Force
```