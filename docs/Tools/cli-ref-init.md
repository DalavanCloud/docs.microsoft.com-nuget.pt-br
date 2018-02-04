---
title: Comando do NuGet CLI init | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 01/18/2018
ms.topic: reference
ms.prod: nuget
ms.technology: 
description: "Referência para o comando de inicialização de nuget.exe"
keywords: "referência de init NuGet, o comando de inicialização"
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: 6d7710cd024e2c2956fb73aa767c3be55b9fb0f9
ms.sourcegitcommit: 4651b16a3a08f6711669fc4577f5d63b600f8f58
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/02/2018
---
# <a name="init-command-nuget-cli"></a><span data-ttu-id="bd550-104">comando init (NuGet CLI)</span><span class="sxs-lookup"><span data-stu-id="bd550-104">init command (NuGet CLI)</span></span>

<span data-ttu-id="bd550-105">**Aplica-se a:** criação do pacote &bullet; **versões com suporte:** 3.3 +</span><span class="sxs-lookup"><span data-stu-id="bd550-105">**Applies to:** package creation &bullet; **Supported versions:** 3.3+</span></span>

<span data-ttu-id="bd550-106">Copia todos os pacotes de uma pasta simples em uma pasta de destino usando um layout hierárquico, conforme descrito para o [adicionar comando](cli-ref-add.md).</span><span class="sxs-lookup"><span data-stu-id="bd550-106">Copies all the packages from a flat folder to a destination folder using a hierarchical layout as described for the [add command](cli-ref-add.md).</span></span> <span data-ttu-id="bd550-107">Ou seja, usando `init` é equivalente a usar o `add` comando em cada pacote na pasta.</span><span class="sxs-lookup"><span data-stu-id="bd550-107">That is, using `init` is equivalent to using the `add` command on each package in the folder.</span></span>

<span data-ttu-id="bd550-108">Assim como acontece com `add`, o destino deve ser uma pasta local ou um caminho UNC; Não há suporte para os repositórios de pacote HTTP como nuget.org ou servidores privadas.</span><span class="sxs-lookup"><span data-stu-id="bd550-108">As with `add`, the destination must be either a local folder or a UNC path; HTTP package repositories such as nuget.org or private servers are not supported.</span></span>

## <a name="usage"></a><span data-ttu-id="bd550-109">Uso</span><span class="sxs-lookup"><span data-stu-id="bd550-109">Usage</span></span>

```cli
nuget init <source> <destination> [options]
```

<span data-ttu-id="bd550-110">onde `<source>` é a pasta que contém pacotes e `<destination>` é a pasta local ou caminho UNC para o qual os pacotes são copiados.</span><span class="sxs-lookup"><span data-stu-id="bd550-110">where `<source>` is the folder containing packages and `<destination>` is the local folder or UNC pathname to which the packages are copied.</span></span>

## <a name="options"></a><span data-ttu-id="bd550-111">Opções</span><span class="sxs-lookup"><span data-stu-id="bd550-111">Options</span></span>

| <span data-ttu-id="bd550-112">Opção</span><span class="sxs-lookup"><span data-stu-id="bd550-112">Option</span></span> | <span data-ttu-id="bd550-113">Descrição</span><span class="sxs-lookup"><span data-stu-id="bd550-113">Description</span></span> |
| --- | --- |
| <span data-ttu-id="bd550-114">ConfigFile</span><span class="sxs-lookup"><span data-stu-id="bd550-114">ConfigFile</span></span> | <span data-ttu-id="bd550-115">O arquivo de configuração do NuGet para aplicar.</span><span class="sxs-lookup"><span data-stu-id="bd550-115">The NuGet configuration file to apply.</span></span> <span data-ttu-id="bd550-116">Se não for especificado, *%AppData%\NuGet\NuGet.Config* é usado.</span><span class="sxs-lookup"><span data-stu-id="bd550-116">If not specified, *%AppData%\NuGet\NuGet.Config* is used.</span></span> |
| <span data-ttu-id="bd550-117">ForceEnglishOutput</span><span class="sxs-lookup"><span data-stu-id="bd550-117">ForceEnglishOutput</span></span> | <span data-ttu-id="bd550-118">*(3.5 +)*  Força nuget.exe para ser executado usando uma cultura invariável, com base em inglês.</span><span class="sxs-lookup"><span data-stu-id="bd550-118">*(3.5+)* Forces nuget.exe to run using an invariant, English-based culture.</span></span> |
| <span data-ttu-id="bd550-119">Expandir</span><span class="sxs-lookup"><span data-stu-id="bd550-119">Expand</span></span> | <span data-ttu-id="bd550-120">Adiciona todos os arquivos em cada pacote que é adicionado à origem do pacote; mesmo que `-Expand` com o `add` comando.</span><span class="sxs-lookup"><span data-stu-id="bd550-120">Adds all files in each package that's added to the package source; same as `-Expand` with the `add` command.</span></span> |
| <span data-ttu-id="bd550-121">Ajuda</span><span class="sxs-lookup"><span data-stu-id="bd550-121">Help</span></span> | <span data-ttu-id="bd550-122">Exibe informações de ajuda para o comando.</span><span class="sxs-lookup"><span data-stu-id="bd550-122">Displays help information for the command.</span></span> |
| <span data-ttu-id="bd550-123">NonInteractive</span><span class="sxs-lookup"><span data-stu-id="bd550-123">NonInteractive</span></span> | <span data-ttu-id="bd550-124">Suprime avisos para a entrada do usuário ou confirmações.</span><span class="sxs-lookup"><span data-stu-id="bd550-124">Suppresses prompts for user input or confirmations.</span></span> |
| <span data-ttu-id="bd550-125">Detalhamento</span><span class="sxs-lookup"><span data-stu-id="bd550-125">Verbosity</span></span> | <span data-ttu-id="bd550-126">Especifica a quantidade de detalhes exibidos na saída: *normal*, *silencioso*, *detalhadas*.</span><span class="sxs-lookup"><span data-stu-id="bd550-126">Specifies the amount of detail displayed in the output: *normal*, *quiet*, *detailed*.</span></span> |

<span data-ttu-id="bd550-127">Consulte também [variáveis de ambiente](cli-ref-environment-variables.md)</span><span class="sxs-lookup"><span data-stu-id="bd550-127">Also see [Environment variables](cli-ref-environment-variables.md)</span></span>

## <a name="examples"></a><span data-ttu-id="bd550-128">Exemplos</span><span class="sxs-lookup"><span data-stu-id="bd550-128">Examples</span></span>

```cli
nuget init c:\foo c:\bar
nuget init \\foo\packages \\bar\packages -Expand
```
