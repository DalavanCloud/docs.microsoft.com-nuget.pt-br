---
title: comandos do NuGet dotNet | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 12/08/2017
ms.topic: article
ms.prod: nuget
ms.technology: 
ms.assetid: 0c81dbc4-2c14-4ec8-b87a-b802a899c3ea
description: "Uma referência curta para comandos relacionados NuGet usando a interface de linha de comando dotnet."
keywords: "comandos do NuGet dotnet, pacote dotnet, restauração dotnet, dotnet nuget locais, dotnet nuget push, dotnet nuget delete"
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: d020e62b8bd04c8f4a75756fb30ebcf13ffdb1b3
ms.sourcegitcommit: a40c1c1cc05a46410f317a72f695ad1d80f39fa2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/05/2018
---
# <a name="dotnet-commands"></a>comandos dotNet

A interface de linha de comando DotNet, que é executado no Windows, Mac OS X e Linux, fornece um número de comandos de nuget.exe essenciais, como listado abaixo. Onde dotnet fornece os comandos desejados, não é necessário fazer o download de nuget.exe.

- [**pacote de dotnet**](/dotnet/core/tools/dotnet-pack?tabs=netcore2x): pacotes de código em um pacote do NuGet. A partir do NuGet 4.0, isso executará o mesmo código que `nuget pack`.
- [**restauração dotnet**](/dotnet/core/tools/dotnet-restore?tabs=netcore2x): restaura as dependências e as ferramentas de um projeto. A partir do NuGet 4.0, isso executará o mesmo código que `nuget restore`.
- [**locais do nuget dotnet**](/dotnet/core/tools/dotnet-nuget-locals): limpa ou lista os recursos locais do NuGet como http-solicitação de cache, cache temporário ou pasta pacotes global de todo o computador.
- [**dotnet nuget push**](/dotnet/core/tools/dotnet-nuget-push): envia um pacote para um servidor e o publica, aplicável nuget.org, Visual Studio Team Services ou todos os servidores NuGet de terceiros.
- [**Excluir dotnet nuget**](/dotnet/core/tools/dotnet-nuget-delete): exclui ou unlists um pacote de um servidor, aplicável nuget.org, Visual Studio Team Services ou todos os servidores NuGet de terceiros.