---
title: Referência do PowerShell NuGet Uninstall-Package
description: Referência de comando do PowerShell de pacote de desinstalação no Console do Gerenciador de pacotes NuGet no Visual Studio.
author: karann-msft
ms.author: karann
ms.date: 06/01/2017
ms.topic: reference
ms.openlocfilehash: ae60473fbb716b23f40b0605be8aaa8515802315
ms.sourcegitcommit: 1d1406764c6af5fb7801d462e0c4afc9092fa569
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43551637"
---
# <a name="uninstall-package-package-manager-console-in-visual-studio"></a>Uninstall-Package (Console do Gerenciador de Pacotes no Visual Studio)

*Este tópico descreve o comando dentro de [NuGet Package Manager Console](package-manager-console.md) no Visual Studio no Windows. Para o comando genérico do pacote de desinstalação do PowerShell, consulte o [referência do PowerShell PackageManagement](/powershell/module/packagemanagement/?view=powershell-6).*

Remove um pacote de um projeto, opcionalmente, removendo suas dependências. Se outros pacotes dependem deste pacote, o comando falhará a menos que – Force a opção for especificada.

## <a name="syntax"></a>Sintaxe

```ps
Uninstall-Package [-Id] <string> [-RemoveDependencies] [-ProjectName <string>] [-Force]
    [-Version <string>] [-WhatIf] [<CommonParameters>]
```

Se outros pacotes dependem deste pacote, o comando falhará a menos que – Force a opção for especificada.

## <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --- | --- |
| Id | (Obrigatório) O identificador do pacote a desinstalar. -Id do comutador em si é opcional. |
| Versão | A versão do pacote para desinstalar, padronizando para a versão atualmente instalada. |
| RemoveDependencies | Desinstale o pacote e suas dependências não utilizadas. Ou seja, se qualquer dependência tiver outro pacote que depende dele, ele será ignorado. |
| ProjectName | O projeto do qual deseja desinstalar o pacote, padronizando para o projeto padrão. |
| Force | Força um pacote a ser desinstalado, mesmo se outros pacotes dependem dele. |
| WhatIf | Mostra o que aconteceria ao executar o comando sem realmente executar a desinstalação. |

Nenhum desses parâmetros aceitam caracteres curinga ou de entrada do pipeline.

## <a name="common-parameters"></a>Parâmetros comuns

`Uninstall-Package` suporta as seguintes [parâmetros comuns do PowerShell](http://go.microsoft.com/fwlink/?LinkID=113216): Debug, ação de erro, ErrorVariable, OutBuffer, OutVariable, PipelineVariable, detalhado, WarningAction e WarningVariable.

## <a name="examples"></a>Exemplos

```ps
# Uninstalls the Elmah package from the default project
Uninstall-Package Elmah

# Uninstalls the Elmah package and all its unused dependencies
Uninstall-Package Elmah -RemoveDependencies 

# Uninstalls the Elmah package even if another package depends on it
Uninstall-Package Elmah -Force
```
