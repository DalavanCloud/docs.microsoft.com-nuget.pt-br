---
title: "Notas de versão do NuGet 3.4 | Microsoft Docs"
author: karann-msft
ms.author: karann-msft
manager: ghogen
ms.date: 11/11/2016
ms.topic: article
ms.prod: nuget
ms.technology: 
ms.assetid: 80a429f5-7569-4349-ad7f-4fe9218eac23
description: "Notas de versão do NuGet 3.4, incluindo correções de bugs, problemas conhecidos, recursos adicionados e DCRs."
keywords: "Notas de versão NuGet 3.4, correções de bugs, problemas conhecidos, adicionaram recursos, DCRs"
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: 911e4377ad9c8b1f865b0721f43ff73b62df6d1e
ms.sourcegitcommit: d0ba99bfe019b779b75731bafdca8a37e35ef0d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/14/2017
---
# <a name="nuget-34-release-notes"></a>Notas de versão 3.4 do NuGet

[Notas de versão 3.4 RC NuGet](../release-notes/nuget-3.4-RC.md) | [NuGet 3.4.1 notas de versão](../release-notes/nuget-3.4.1.md)

NuGet 3.4 foi lançada em 30 de março de 2016 como parte do Visual Studio 2015 atualização 2 e versão de visualização do Visual Studio 15 e foi criado com alguns princípios em mente:

*  Suporte de plataforma cruzada
*  Melhorias de desempenho
*  Pequenos aperfeiçoamentos de interface do usuário

Os seguintes recursos foram adicionados anteriormente no RC e foram atualizados ou concluídos para a versão 3.4:

## <a name="new-features"></a>Novos recursos

* Os clientes do NuGet agora suportam a codificação de conteúdo gzip dos repositórios
* Suporte para PDBs de pacotes em projetos xproj
* Suporte para iOS e Android criar ações no elemento contentFiles
* Suporte para os identificadores de netstandard e netstandardapp monikers framework

## <a name="new-user-interface-features"></a>Novos recursos de Interface do usuário

* Melhorias de desempenho significativas, especialmente nas guias instalados, atualizações e consolidar
* Origem de agregação 'Todas as fontes de pacote' está disponível com a mesclagem de resultados de pesquisa apropriada
* Instalado e guias de atualizações agora são classificados em ordem alfabética
* Adicionar um botão de atualização que permite que uma pesquisa a ser atualizado
* Opções de compilação mais recentes na parte superior da lista de versão

## <a name="updates-and-improvements"></a>Atualizações e aprimoramentos

* Pacotes referenciados em `project.json` que têm um flutuante versão não será atualizado em cada compilação. Em vez disso, elas serão atualizadas somente quando forçada a restaurar, limpar, recriar ou modificar `project.json`.
* origens de repositório NuGet.org não são forçadas em uma configuração de projeto quando você usa a configuração do NuGet da interface do usuário.
* O NuGet não restaura pacotes em projetos compartilhados nem grava um arquivo de bloqueio.
* Nós já aprimorado falha de rede e tente novamente tratamento para servidores inacessíveis ou lento para responder.
* Comportamentos de teclado e mouse são aprimorados na UI do Gerenciador de pacote do Visual Studio.
* Agora temos suporte para a versão mais recente `project.json` esquema DNX.

## <a name="breaking-changes"></a>Alterações significativas

* Números de versão do pacote agora são normalizados para o formato *principais*. *pequenas*. *patch*-*pré-lançamento* cada um dos principais, secundária e patch são tratados como inteiros e descartar qualquer zeros à esquerda.  As informações de pré-lançamento são tratadas como uma cadeia de caracteres e nenhuma alteração será aplicada a ele. Esses números são usados em consultas, os clientes do NuGet e a pesquisa fornecidos pelo serviço do nuget.org.  Mais detalhes podem ser encontrados em documentos do NuGet em [versões de pré-lançamento](../create-packages/prerelease-packages.md).

## <a name="known-issues"></a>Problemas conhecidos

* **Problema:** Windows 10 v1511 usuários podem enfrentar problemas ou até mesmo uma falha Visual Studio com o Powershell no Visual Studio nos seguintes cenários:
    * Instalar / desinstalar os pacotes que têm install.ps1 / uninstall.ps1 scripts
    * Carregando projetos com um script init.ps1 (como EntityFramework)
    * Publicação de conteúdo da web

* **Solução alternativa:** Certifique-se de que a instalação do Windows 10 tem os patches mais recentes aplicados, expecially janeiro de 2016 (3124263 KB) ou uma atualização posterior.  Mais detalhes estão disponíveis em [problema do GitHub &#1638;](http://github.com/nuget/home/issues/1638)

* **Problema:** os redirecionamentos de protocolo do NuGet v2 foram interrompidos.
Os repositórios NuGet personalizados que redirecionam solicitações para um host alternativo não consideram a solicitação de redirecionamento.
* **Solução alternativa:** para contornar esse problema, configure o URI do repositório de pacote nas configurações para apontar para o local do servidor redirecionado.
Para obter mais informações, consulte [solicitação de recepção GitHub &#387;](https://github.com/NuGet/NuGet.Client/pull/387).

Continuamos a rastrear problemas em nossa lista de problemas do GitHub que pode ser encontrada em: [http://github.com/nuget/home/issues](http://github.com/nuget/home/issues)