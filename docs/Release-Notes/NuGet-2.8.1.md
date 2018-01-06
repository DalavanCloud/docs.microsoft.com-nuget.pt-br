---
title: "Notas de versão do NuGet 2.8.1 | Microsoft Docs"
author: karann-msft
ms.author: karann-msft
manager: ghogen
ms.date: 11/11/2016
ms.topic: article
ms.prod: nuget
ms.technology: 
ms.assetid: eebbbae4-fc6a-4c05-9102-4ec66b4c64a4
description: "Notas de versão do NuGet 2.8.1 incluindo problemas conhecidos, correções de bug, recursos adicionados e DCRs."
keywords: "Notas de versão NuGet 2.8.1, correções de bugs, problemas conhecidos, adicionaram recursos, DCRs"
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: ac33369644d312591bfe8c06540935b2c6063c5d
ms.sourcegitcommit: d0ba99bfe019b779b75731bafdca8a37e35ef0d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/14/2017
---
# <a name="nuget-281-release-notes"></a>Notas de versão do NuGet 2.8.1

[Notas de versão do NuGet 2.8](../release-notes/nuget-2.8.md) | [notas de versão do NuGet 2.8.2](../release-notes/nuget-2.8.2.md)

NuGet 2.8.1 foi lançado em 2 de abril de 2014.

## <a name="notable-features-in-the-release"></a>Recursos importantes da versão

### <a name="support-for-windows-phone-81-projects"></a>Suporte para projetos do Windows Phone 8.1
Esta versão agora dá suporte a seguinte nova monikers destino do framework que podem ser usado para projetos de destino Windows Phone 8.1:

* WindowsPhone81 / WP81 (para projetos do Windows Phone baseados no Silverlight)
* WindowsPhoneApp81 / WPA81 (para projetos do WinRT com base no aplicativo do Windows Phone)

### <a name="update-of-the-nuget-webmatrix-extension"></a>Atualização da extensão do WebMatrix NuGet
Esta versão atualiza o cliente do NuGet encontrado no WebMatrix para [Core](https://www.nuget.org/packages/Nuget.Core/2.6.1) 2.6.1 e coloca com recursos como transformações XDT. Mais importante, a versão 2.6.1 atualização core permite que o cliente o WebMatrix instalar os pacotes do NuGet que contêm versões mais recentes do `.nuspec` esquema, que inclui os pacotes do NuGet do ASP.NET.

Para obter mais informações sobre a atualização da extensão do WebMatrix, consulte os específico [notas de versão](../release-notes/nuget-2.6.1-for-WebMatrix.md).

### <a name="bug-fixes"></a>Correções de Bug
Além desses recursos, esta versão do NuGet inclui outras correções de bug. Não havia 16 questões total na versão. Para obter uma lista completa do trabalho de itens corrigidos no NuGet 2.8.1,. exibir o [NuGet Issue Tracker para esta versão](https://nuget.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=NuGet%202.8.1&assignedTo=All&component=All&sortField=LastUpdatedDate&sortDirection=Descending&page=0&reasonClosed=All).

### <a name="reshipping-with-visual-studio-14-ctp"></a>Reshipping com o Visual Studio CTP "14"
No CTP do Visual Studio "14" lançada em 3ª de junho de 2014, NuGet 2.8.1 é fornecido na caixa. Os recursos que ele oferece suporte à permanecem no par com outros 2.8.1 VSIXes como do Visual Studio 2013.