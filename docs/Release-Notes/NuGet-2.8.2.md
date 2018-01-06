---
title: "Notas de versão do NuGet 2.8.2 | Microsoft Docs"
author: karann-msft
ms.author: karann-msft
manager: ghogen
ms.date: 11/11/2016
ms.topic: article
ms.prod: nuget
ms.technology: 
ms.assetid: bb547f5d-3c0e-4721-b2c7-3fc7e09c34de
description: "Notas de versão do NuGet 2.8.2 incluindo problemas conhecidos, correções de bug, recursos adicionados e DCRs."
keywords: "Notas de versão NuGet 2.8.2, correções de bugs, problemas conhecidos, adicionaram recursos, DCRs"
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: 221b8970663ca80a986fc3ee542b99971c5e2018
ms.sourcegitcommit: d0ba99bfe019b779b75731bafdca8a37e35ef0d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/14/2017
---
# <a name="nuget-282-release-notes"></a>Notas de versão do NuGet 2.8.2

[Notas de versão do NuGet 2.8.1](../release-notes/nuget-2.8.1.md) | [NuGet 2.8.3 notas de versão](../release-notes/nuget-2.8.3.md)

NuGet 2.8.2 foi lançado em 22 de maio de 2014.  Esta versão incluído somente as alterações para o nuget.exe de linha de comando, o pacote de NuGet.Server e outros pacotes do NuGet.  A versão não incluiu uma extensão atualizada do Visual Studio ou a extensão do WebMatrix.

## <a name="notable-updates"></a>Atualizações importantes

As atualizações mais importantes foram no nuget.exe de linha de comando e o pacote de NuGet.Server (para feeds do NuGet auto-hospedado).

### <a name="important-nugetexe-bug-fixes"></a>Correções de bugs importantes nuget.exe

1. [NuGet.exe Push falha e mantém a tentar novamente](https://nuget.codeplex.com/workitem/4000)
1. [NuGet.exe por Push não envia credenciais de autenticação básica corretamente](https://nuget.codeplex.com/workitem/4109)
1. [Envio de NuGet.exe não siga redirecionamento temporário](https://nuget.codeplex.com/workitem/4050)

### <a name="important-nugetserver-bug-fix"></a>Correção de Bug NuGet.Server importantes

1. [Valor incorreto de IsAbsoluteLatestVersion retornado por NuGet.Server](https://nuget.codeplex.com/workitem/4147)

## <a name="packages-updated"></a>Pacotes atualizados

O nuget.exe de linha de comando e NuGet.Server correções são entregues como atualizações de pacote do NuGet.  Não havia outros pacotes atualizados com 2.8.2 também.

Aqui está a lista de pacotes atualizados:

1. [Core](https://www.nuget.org/packages/NuGet.Core/)
1. [NuGet.CommandLine](https://www.nuget.org/packages/NuGet.CommandLine/)
1. [NuGet.Server](https://www.nuget.org/packages/NuGet.Server/)
1. [NuGet.Build](https://www.nuget.org/packages/NuGet.Build/)
1. [NuGet. VisualStudio](https://www.nuget.org/packages/NuGet.VisualStudio/) (o pacote, não a extensão)

## <a name="all-changes"></a>Todas as alterações
Não havia 10 problemas abordados na versão. Para obter uma lista completa do trabalho de itens corrigidos no NuGet 2.8.2,. exibir o [NuGet Issue Tracker para esta versão](https://nuget.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=NuGet%202.8.2&assignedTo=All&component=All&sortField=LastUpdatedDate&sortDirection=Descending&page=0&reasonClosed=All).