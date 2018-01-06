---
title: "Notas de versão do NuGet 2.2.1 | Microsoft Docs"
author: karann-msft
ms.author: karann-msft
manager: ghogen
ms.date: 11/11/2016
ms.topic: article
ms.prod: nuget
ms.technology: 
ms.assetid: 39ceaeb3-2d33-4b1c-b195-eba36c6cbf9a
description: "Notas de versão do NuGet 2.2.1 incluindo problemas conhecidos, correções de bug, recursos adicionados e DCRs."
keywords: "Notas de versão 2.2.1 o NuGet, correções de bugs, problemas conhecidos, adicionaram recursos, DCRs"
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: c31150572b4b6e066643ebcf0d92be16b25c6e19
ms.sourcegitcommit: d0ba99bfe019b779b75731bafdca8a37e35ef0d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/14/2017
---
# <a name="nuget-221-release-notes"></a>Notas de versão do NuGet 2.2.1

[Notas de versão do NuGet 2.2](../release-notes/nuget-2.2.md) | [notas de versão do NuGet 2.5](../release-notes/nuget-2.5.md)

NuGet 2.2.1 foi lançado em 15 de fevereiro de 2013.  O número de versão da extensão do VS é 2.2.40116.9051.

## <a name="localization-refresh"></a>Atualização de localização
NuGet é fornecido como parte do Visual Studio 2012, ele foi totalmente localizado em inglês + 13 outros idiomas.  Desde então, NuGet 2.1 e 2.2 fornecidos, mas a localização não tinha sido atualizada.  A versão do NuGet 2.2.1 atualiza nossa localização.

Interface do usuário e o Console do PowerShell do NuGet são localizados em idiomas a seguir:

1. Chinês (simplificado)
1. Chinês (tradicional)
1. Tcheco
1. Inglês
1. Francês
1. Alemão
1. Italiano
1. Japonês
1. Coreano
1. Polonês
1. Português (Brasil)
1. Russo
1. Espanhol
1. Turco

## <a name="visual-studio-templates-support-multiple-preinstalled-package-repositories"></a>Modelos do Visual Studio oferecem suporte a vários repositórios de pacote pré-instalado
Se você gerar modelos do Visual Studio, você pode usar o NuGet [pré-instalar pacotes](../visual-studio-extensibility/visual-studio-templates.md) como parte do modelo.  Até agora, esse recurso tinha uma limitação que todos os pacotes necessários para provenientes da mesma origem.  Com o NuGet 2.2.1 no entanto, você pode ter pacotes instalados vários repositórios (dentro do modelo, um VSIX ou uma pasta no disco definida no registro).

O cenário principal para esse recurso é personalizados modelos de projeto do ASP.NET.  Os modelos internos do ASP.NET usam pacotes pré-instalados, extrair pacotes de disco local.  Agora você pode criar um modelo de projeto ASP.NET personalizado que usa os pacotes existentes instalados pelo ASP.NET mas adicionar extras pacotes do NuGet em seu modelo.

## <a name="bug-fixes"></a>Correções de Bug
NuGet 2.2.1 inclui algumas correções de bugs de destino. Para obter uma lista de trabalho itens corrigidos no NuGet 2.2.1, por favor, exibir o [NuGet Issue Tracker para esta versão](http://nuget.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=NuGet%202.2.1&assignedTo=All&component=All&sortField=LastUpdatedDate&sortDirection=Descending&page=0).


## <a name="known-issues"></a>Problemas conhecidos

Se você estiver estendendo modelos de projeto do ASP.NET, todos os repositórios de pacote pré-instalado devem usar o mesmo valor para o `isPreunzipped` atributo.