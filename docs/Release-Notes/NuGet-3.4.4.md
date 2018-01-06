---
title: "Notas de versão do NuGet 3.4.4 | Microsoft Docs"
author: karann-msft
ms.author: karann-msft
manager: ghogen
ms.date: 11/11/2016
ms.topic: article
ms.prod: nuget
ms.technology: 
ms.assetid: 6f0aa9bb-75e5-429d-9954-3cb41a909c14
description: "Notas de versão do NuGet 3.4.4 incluindo problemas conhecidos, correções de bug, recursos adicionados e DCRs."
keywords: "Notas de versão NuGet 3.4.4, correções de bugs, problemas conhecidos, adicionaram recursos, DCRs"
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: 51ddb918d2269990588e9cba4d15ffeb878de5f3
ms.sourcegitcommit: d0ba99bfe019b779b75731bafdca8a37e35ef0d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/14/2017
---
# <a name="nuget-344-release-notes"></a>Notas de versão do NuGet 3.4.4

[Notas de versão do NuGet 3.4.3](../release-notes/nuget-3.4.3.md) | [notas de versão 3.5 Beta do NuGet](../release-notes/nuget-3.5-Beta.md)

O foco principal desta versão foram as melhorias a qualidade das 3.4.3 versão de nuget.exe com algumas correções à extensão do Visual Studio.

Você pode baixar o VSIX e nuget.exe [aqui](https://dist.nuget.org/index.html).

## <a name="344-rtmhttpsgithubcomnugetnugetclienttree344-rtm-2016-05-19"></a>[3.4.4-RTM](https://github.com/NuGet/NuGet.Client/tree/3.4.4-rtm) (2016-05-19)

[Log de alteração completa](https://github.com/NuGet/NuGet.Client/compare/3.5.0-beta-final...3.4.4-rtm)

[Lista de problemas](https://github.com/NuGet/Home/issues?q=is%3Aissue+milestone%3A3.4.4+is%3Aclosed)

### <a name="changes"></a>Alterações

- Aprimoramentos do pacote: Melhorias Empacotando símbolos, de remessa com `project.json` e mais [ \#606](https://github.com/NuGet/NuGet.Client/pull/606)
- Exibir a exceção quando não houver uma falha ao localizar a projetos no comando update [\#605] (https://github.com/NuGet/NuGet.Client/pull/605
- Ler o tipo de pacote de entrada `.nuspec` e `project.json` quando remessa [ \#603](https://github.com/NuGet/NuGet.Client/pull/603)
- Verifique NuGet.Shared não em um projeto. [\#602](https://github.com/NuGet/NuGet.Client/pull/602)
- Use o tempo limite de envio por push o tempo de limite de resposta HTTP [ \#599](https://github.com/NuGet/NuGet.Client/pull/599)
- Arquivos de pacote com tempos de futuros não terão seus vezes usado [ \#597](https://github.com/NuGet/NuGet.Client/pull/597)
- Atualizando `NuGet.Core.dll` versão 2.12.0 para corrigir o problema XML [ \#594](https://github.com/NuGet/NuGet.Client/pull/594)
- Suporte./NuGet.CommandLine.XPlat - v \<detalhamento\> \<modo\> [ \#593](https://github.com/NuGet/NuGet.Client/pull/593)
- Erro ao exibir restaurar sem `project.json` ou `packages.config` [ \#590](https://github.com/NuGet/NuGet.Client/pull/590)
- Correção de versões de dependência quando diferem versões necessárias [ \#559](https://github.com/NuGet/NuGet.Client/pull/559)