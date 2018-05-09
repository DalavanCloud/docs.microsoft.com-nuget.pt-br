---
title: Resolução de dependências de pacote do NuGet
description: Detalhes sobre o processo por meio do qual as dependências de um pacote do NuGet são resolvidas e instaladas no NuGet 2.x e 3.x ou superior.
author: kraigb
ms.author: kraigb
manager: douge
ms.date: 08/14/2017
ms.topic: conceptual
ms.openlocfilehash: bfe6e348fa9a8f5df7f28509098260128920c528
ms.sourcegitcommit: 3eab9c4dd41ea7ccd2c28bb5ab16f6fbbec13708
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/26/2018
---
# <a name="how-nuget-resolves-package-dependencies"></a><span data-ttu-id="3b257-103">Como o NuGet resolve as dependências do pacote</span><span class="sxs-lookup"><span data-stu-id="3b257-103">How NuGet resolves package dependencies</span></span>

<span data-ttu-id="3b257-104">Sempre que um pacote for instalado ou reinstalado, o que inclui o que está sendo instalado como parte de um processo de [restauração](../consume-packages/package-restore.md), o NuGet também instala todos os pacotes adicionais dos quais o primeiro pacote depende.</span><span class="sxs-lookup"><span data-stu-id="3b257-104">Any time a package is installed or reinstalled, which includes being installed as part of a [restore](../consume-packages/package-restore.md) process, NuGet also installs any additional packages on which that first package depends.</span></span>

<span data-ttu-id="3b257-105">Essas dependências imediatas também podem ter suas próprias dependências, que podem continuar em uma profundidade arbitrária.</span><span class="sxs-lookup"><span data-stu-id="3b257-105">Those immediate dependencies might then also have dependencies on their own, which can continue to an arbitrary depth.</span></span> <span data-ttu-id="3b257-106">Isso produz o que é chamado de um *grafo de dependência* que descreve as relações entre os pacotes em todos os níveis.</span><span class="sxs-lookup"><span data-stu-id="3b257-106">This produces what's called a *dependency graph* that describes the relationships between packages at all levels.</span></span>

<span data-ttu-id="3b257-107">Quando vários pacotes têm a mesma dependência, a mesma ID de pacote pode aparecer no grafo várias vezes, potencialmente com restrições de versão diferentes.</span><span class="sxs-lookup"><span data-stu-id="3b257-107">When multiple packages have the same dependency, then the same package ID can appear in the graph multiple times, potentially with different version constraints.</span></span> <span data-ttu-id="3b257-108">No entanto, somente uma versão de determinado pacote pode ser usada em um projeto, por isso o NuGet precisa escolher qual versão é usada.</span><span class="sxs-lookup"><span data-stu-id="3b257-108">However, only one version of a given package can be used in a project, so NuGet must choose which version is used.</span></span> <span data-ttu-id="3b257-109">O processo exato depende do formato de gerenciamento de pacote em uso.</span><span class="sxs-lookup"><span data-stu-id="3b257-109">The exact process depends on the package management format being used.</span></span>

## <a name="dependency-resolution-with-packagereference"></a><span data-ttu-id="3b257-110">Resolução de dependência com PackageReference</span><span class="sxs-lookup"><span data-stu-id="3b257-110">Dependency resolution with PackageReference</span></span>

<span data-ttu-id="3b257-111">Ao instalar os pacotes em projetos usando o formato PackageReference, o NuGet adiciona referências a um grafo de pacote simples no arquivo apropriado e resolve conflitos antecipadamente.</span><span class="sxs-lookup"><span data-stu-id="3b257-111">When installing packages into projects using the PackageReference format, NuGet adds references to a flat package graph in the appropriate file and resolves conflicts ahead of time.</span></span> <span data-ttu-id="3b257-112">Esse processo é chamado de *restauração transitiva*.</span><span class="sxs-lookup"><span data-stu-id="3b257-112">This process is referred to as *transitive restore*.</span></span> <span data-ttu-id="3b257-113">Reinstalar ou restaurar pacotes é um processo de baixar os pacotes listados no grafo, resultando em builds mais rápidos e mais previsíveis.</span><span class="sxs-lookup"><span data-stu-id="3b257-113">Reinstalling or restoring packages is then a process of downloading the packages listed in the graph, resulting in faster and more predictable builds.</span></span> <span data-ttu-id="3b257-114">Você também pode aproveitar as versões curinga (flutuantes), como 2.8. \*, evitando chamadas caras e propensas a erro para `nuget update` em computadores clientes e servidores de build.</span><span class="sxs-lookup"><span data-stu-id="3b257-114">You can also take advantage of wildcard (floating) versions, such as 2.8.\*, avoiding expensive and error prone calls to `nuget update` on the client machines and build servers.</span></span>

<span data-ttu-id="3b257-115">Quando o processo de restauração do NuGet é executado antes de uma compilação, ele resolve as dependências primeiro na memória, em seguida, grava o grafo resultante em um arquivo chamado `project.assets.json` na pasta `obj` de um projeto usando PackageReference.</span><span class="sxs-lookup"><span data-stu-id="3b257-115">When the NuGet restore process runs prior to a build, it resolves dependencies first in memory, then writes the resulting graph to a file called `project.assets.json` in the `obj` folder of a project using PackageReference.</span></span> <span data-ttu-id="3b257-116">O MSBuild lê este arquivo e converte-o em um conjunto de pastas em que as referências em potencial podem ser encontradas e as adiciona à árvore de projeto na memória.</span><span class="sxs-lookup"><span data-stu-id="3b257-116">MSBuild then reads this file and translates it into a set of folders where potential references can be found, and then adds them to the project tree in memory.</span></span>

<span data-ttu-id="3b257-117">O arquivo de bloqueio é temporário e não deve ser adicionado ao controle do código-fonte.</span><span class="sxs-lookup"><span data-stu-id="3b257-117">The lock file is temporary and should not be added to source control.</span></span> <span data-ttu-id="3b257-118">É listado por padrão em ambos `.gitignore` e `.tfignore`.</span><span class="sxs-lookup"><span data-stu-id="3b257-118">It's listed by default in both `.gitignore` and `.tfignore`.</span></span> <span data-ttu-id="3b257-119">Consulte [Pacotes e controle do código-fonte](packages-and-source-control.md).</span><span class="sxs-lookup"><span data-stu-id="3b257-119">See [Packages and source control](packages-and-source-control.md).</span></span>

### <a name="dependency-resolution-rules"></a><span data-ttu-id="3b257-120">Regras de resolução de dependência</span><span class="sxs-lookup"><span data-stu-id="3b257-120">Dependency resolution rules</span></span>

<span data-ttu-id="3b257-121">A restauração transitiva aplica quatro regras principais para resolver as dependências: versão mais antiga aplicável, versões flutuantes, wins mais próximos e dependências primo.</span><span class="sxs-lookup"><span data-stu-id="3b257-121">Transitive restore applies four main rules to resolve dependencies: lowest applicable version, floating versions, nearest-wins, and cousin dependencies.</span></span>

<a name="lowest-applicable-version"></a>

#### <a name="lowest-applicable-version"></a><span data-ttu-id="3b257-122">Versão mais antiga aplicável</span><span class="sxs-lookup"><span data-stu-id="3b257-122">Lowest applicable version</span></span>

<span data-ttu-id="3b257-123">A regra de versão aplicáveis mais antiga restaura a versão mais antiga possível de um pacote, conforme definido por suas dependências.</span><span class="sxs-lookup"><span data-stu-id="3b257-123">The lowest applicable version rule restores the lowest possible version of a package as defined by its dependencies.</span></span> <span data-ttu-id="3b257-124">Ela também se aplica às dependências do aplicativo ou à biblioteca de classes, a menos que seja declarado como [flutuante](#floating-versions).</span><span class="sxs-lookup"><span data-stu-id="3b257-124">It also applies to dependencies on the application or the class library unless declared as [floating](#floating-versions).</span></span>

<span data-ttu-id="3b257-125">Na figura a seguir, por exemplo, 1.0-beta é considerado inferior a 1.0, por isso o NuGet escolhe a versão 1.0:</span><span class="sxs-lookup"><span data-stu-id="3b257-125">In the following figure, for example, 1.0-beta is considered lower than 1.0 so NuGet chooses the 1.0 version:</span></span>

![Escolher a versão mais antiga aplicável](media/projectJson-dependency-1.png)

<span data-ttu-id="3b257-127">Na figura a seguir, a versão 2.1 não está disponível no feed, mas como a restrição de versão é >=2.1, o NuGet escolhe a próxima versão mais antiga encontrada, neste caso 2.2:</span><span class="sxs-lookup"><span data-stu-id="3b257-127">In the next figure, version 2.1 is not available on the feed but because the version constraint is >= 2.1 NuGet picks the next lowest version it can find, in this case 2.2:</span></span>

![Escolher a próxima versão mais antiga disponível no feed](media/projectJson-dependency-2.png)

<span data-ttu-id="3b257-129">Quando um aplicativo especifica um número de versão exata, como 1.2, que não está disponível no feed, o NuGet falhará com um erro ao tentar instalar ou restaurar o pacote:</span><span class="sxs-lookup"><span data-stu-id="3b257-129">When an application specifies an exact version number, such as 1.2, that is not available on the feed, NuGet fails with an error when attempting to install or restore the package:</span></span>

![O NuGet gera um erro quando uma versão exata do pacote não está disponível](media/projectJson-dependency-3.png)

<a name="floating-versions"></a>

#### <a name="floating-wildcard-versions"></a><span data-ttu-id="3b257-131">Versões flutuante (curinga)</span><span class="sxs-lookup"><span data-stu-id="3b257-131">Floating (wildcard) versions</span></span>

<span data-ttu-id="3b257-132">Uma versão de dependência flutuante ou curinga é especificada com o curinga \*, assim como acontece com o 6.0.\*.</span><span class="sxs-lookup"><span data-stu-id="3b257-132">A floating or wildcard dependency version is specified with the \* wildcard, as with 6.0.\*.</span></span> <span data-ttu-id="3b257-133">Essa especificação de versão diz “use a versão mais recente do 6.0”; 4.\* significa “use a versão mais recente de 4.x”.</span><span class="sxs-lookup"><span data-stu-id="3b257-133">This version specification says "use the latest 6.0.x version"; 4.\* means "use the latest 4.x version."</span></span> <span data-ttu-id="3b257-134">Usar um caractere curinga permite que um pacote de dependência continue a se desenvolver sem a necessidade de uma alteração no aplicativo (ou pacote) consumidor.</span><span class="sxs-lookup"><span data-stu-id="3b257-134">Using a wildcard allows a dependency package to continue evolving without requiring a change to the consuming application (or package).</span></span>

<span data-ttu-id="3b257-135">Ao usar um caractere curinga, o NuGet resolverá a versão mais recente de um pacote que corresponde ao padrão de versão, por exemplo 6.0. \* obtém a versão mais recente de um pacote que começa com 6.0:</span><span class="sxs-lookup"><span data-stu-id="3b257-135">When using a wildcard, NuGet resolves the highest version of a package that matches the version pattern, for example 6.0.\* gets the highest version of a package that starts with 6.0:</span></span>

![Escolher a versão 6.0.1 quando uma versão flutuante 6.0.\* é solicitada](media/projectJson-dependency-4.png)

> [!Note]
> <span data-ttu-id="3b257-137">Para obter informações sobre o comportamento de curingas e as versões de pré-lançamento, consulte [Controle de versão do pacote](../reference/package-versioning.md#version-ranges-and-wildcards).</span><span class="sxs-lookup"><span data-stu-id="3b257-137">For information on the behavior of wildcards and pre-release versions, see [Package versioning](../reference/package-versioning.md#version-ranges-and-wildcards).</span></span>


<a name="nearest-wins"></a>

#### <a name="nearest-wins"></a><span data-ttu-id="3b257-138">Wins mais próximos</span><span class="sxs-lookup"><span data-stu-id="3b257-138">Nearest wins</span></span>

<span data-ttu-id="3b257-139">Quando o grafo de pacote para um aplicativo contém versões diferentes do mesmo pacote, o NuGet escolhe aquele que é mais próximo do aplicativo no grafo e ignora todos os demais.</span><span class="sxs-lookup"><span data-stu-id="3b257-139">When the package graph for an application contains different versions of the same package, NuGet chooses the package that's closest to the application in the graph and ignores all others.</span></span> <span data-ttu-id="3b257-140">Esse comportamento permite que um aplicativo substitua qualquer versão do pacote específico no grafo de dependência.</span><span class="sxs-lookup"><span data-stu-id="3b257-140">This behavior allows an application to override any particular package version in the dependency graph.</span></span>

<span data-ttu-id="3b257-141">No exemplo abaixo, o aplicativo depende diretamente no Pacote B com uma restrição de versão de >=2.0.</span><span class="sxs-lookup"><span data-stu-id="3b257-141">In the example below, the application depends directly on Package B with a version constraint of >=2.0.</span></span> <span data-ttu-id="3b257-142">O aplicativo também depende do Pacote A, que por sua vez também depende do Pacote B, mas com uma restrição >=1.0.</span><span class="sxs-lookup"><span data-stu-id="3b257-142">The application also depends on Package A which in turn also depends on Package B, but with a >=1.0 constraint.</span></span> <span data-ttu-id="3b257-143">Como a dependência do Pacote B 2.0 é mais próxima do aplicativo no grafo, essa versão é usada:</span><span class="sxs-lookup"><span data-stu-id="3b257-143">Because the dependency on Package B 2.0 is nearer to the application in the graph, that version is used:</span></span>

![Aplicativo que usa a regra O mais próximo vence](media/projectJson-dependency-5.png)

>[!Warning]
> <span data-ttu-id="3b257-145">A regra O mais próximo vence pode resultar em um downgrade da versão do pacote, potencialmente interrompendo, dessa forma, outras dependências no grafo.</span><span class="sxs-lookup"><span data-stu-id="3b257-145">The Nearest Wins rule can result in a downgrade of the package version, thus potentially breaking other dependencies in the graph.</span></span> <span data-ttu-id="3b257-146">Portanto, essa regra é aplicada com um aviso para alertar o usuário.</span><span class="sxs-lookup"><span data-stu-id="3b257-146">Hence this rule is applied with a warning to alert the user.</span></span>

<span data-ttu-id="3b257-147">Esta regra também resulta em maior eficiência com um grande grafo de dependência (como aqueles com os pacotes BCL) porque depois que uma determinada dependência é ignorada, o NuGet também ignora todas as dependências restantes no branch do grafo.</span><span class="sxs-lookup"><span data-stu-id="3b257-147">This rule also results in greater efficiency with a large dependency graph (such as those with the BCL packages) because once a given dependency is ignored, NuGet also ignores all remaining dependencies on that branch of the graph.</span></span> <span data-ttu-id="3b257-148">No diagrama abaixo, por exemplo, como o Pacote C 2.0 é usado, o NuGet ignora os branches no grafo que fazem referência a uma versão anterior do Pacote C:</span><span class="sxs-lookup"><span data-stu-id="3b257-148">In the diagram below, for example, because Package C 2.0 is used, NuGet ignores any branches in the graph that refer to an older version of Package C:</span></span>

![Quando o NuGet ignora um pacote no grafo, ele ignora todo o branch](media/projectJson-dependency-6.png)

<a name="cousin-dependencies"></a>

#### <a name="cousin-dependencies"></a><span data-ttu-id="3b257-150">Dependências primo</span><span class="sxs-lookup"><span data-stu-id="3b257-150">Cousin dependencies</span></span>

<span data-ttu-id="3b257-151">Quando versões diferentes do pacote são referidas na mesma distância no grafo do aplicativo, o NuGet usa a versão mais antiga que cumpre todos os requisitos de versão (assim como acontece com as regras [versão mais antiga aplicável](#lowest-applicable-version) e [versões flutuantes](#floating-versions)).</span><span class="sxs-lookup"><span data-stu-id="3b257-151">When different package versions are referred to at the same distance in the graph from the application, NuGet uses the lowest version that satisfies all version requirements (as with the [lowest applicable version](#lowest-applicable-version) and [floating versions](#floating-versions) rules).</span></span> <span data-ttu-id="3b257-152">Na imagem abaixo, por exemplo, a versão 2.0 do Pacote B satisfaz a outra restrição >=1.0 e, portanto, é usada:</span><span class="sxs-lookup"><span data-stu-id="3b257-152">In the image below, for example, version 2.0 of Package B satisfies the other >=1.0 constraint, and is thus used:</span></span>

![Resolvendo as dependências primo usando a versão inferior que atende todas as restrições](media/projectJson-dependency-7.png)

<span data-ttu-id="3b257-154">Em alguns casos, não é possível atender a todos os requisitos de versão.</span><span class="sxs-lookup"><span data-stu-id="3b257-154">In some cases, it's not possible to meet all version requirements.</span></span> <span data-ttu-id="3b257-155">Conforme mostrado abaixo, se o Pacote A requer exatamente o Pacote B 1.0 e o Pacote C requer Pacote B >= 2.0, o NuGet não poderá resolver as dependências e apresentará um erro.</span><span class="sxs-lookup"><span data-stu-id="3b257-155">As shown below, if Package A requires exactly Package B 1.0 and Package C requires Package B >=2.0, then NuGet cannot resolve the dependencies and gives an error.</span></span>

![Dependências não podem ser resolvidas devido a um requisito de versão exata](media/projectJson-dependency-8.png)

<span data-ttu-id="3b257-157">Nessas situações, o consumidor de nível superior (o aplicativo ou pacote) deve adicionar sua própria dependência direta no Pacote B para que a regra [O mais próximo vence](#nearest-wins) se aplique.</span><span class="sxs-lookup"><span data-stu-id="3b257-157">In these situations, the top-level consumer (the application or package) should add its own direct dependency on Package B so that the [Nearest Wins](#nearest-wins) rule applies.</span></span>

## <a name="dependency-resolution-with-packagesconfig"></a><span data-ttu-id="3b257-158">Resolução de dependência com packages.config</span><span class="sxs-lookup"><span data-stu-id="3b257-158">Dependency resolution with packages.config</span></span>

<span data-ttu-id="3b257-159">Com o `packages.config`, as dependências do projeto são gravadas em `packages.config` como uma lista simples.</span><span class="sxs-lookup"><span data-stu-id="3b257-159">With `packages.config`, a project's dependencies are written to `packages.config` as a flat list.</span></span> <span data-ttu-id="3b257-160">Todas as dependências desses pacotes também são gravadas na mesma lista.</span><span class="sxs-lookup"><span data-stu-id="3b257-160">Any dependencies of those packages are also written in the same list.</span></span> <span data-ttu-id="3b257-161">Quando os pacotes são instalados, o NuGet também pode modificar o arquivo `.csproj`, `app.config`, `web.config` e outros arquivos individuais.</span><span class="sxs-lookup"><span data-stu-id="3b257-161">When packages are installed, NuGet might also modify the `.csproj` file, `app.config`, `web.config`, and other individual files.</span></span>

<span data-ttu-id="3b257-162">Com `packages.config`, o NuGet tenta resolver conflitos de dependência durante a instalação de cada pacote individual.</span><span class="sxs-lookup"><span data-stu-id="3b257-162">With `packages.config`, NuGet attempts to resolve dependency conflicts during the installation of each individual package.</span></span> <span data-ttu-id="3b257-163">Ou seja, se o Pacote A está sendo instalado e depende do Pacote B, e este já está listado em `packages.config` como uma dependência de outra coisa, o NuGet compara as versões do Pacote B que está sendo solicitado e tenta localizar uma versão que atenda a todas as restrições de versão.</span><span class="sxs-lookup"><span data-stu-id="3b257-163">That is, if Package A is being installed and depends on Package B, and Package B is already listed in `packages.config` as a dependency of something else, NuGet compares the versions of Package B being requested and attempts to find a version that satisfies all version constraints.</span></span> <span data-ttu-id="3b257-164">Especificamente, o NuGet seleciona a versão *principal.secundária* mais antiga que satisfaça as dependências.</span><span class="sxs-lookup"><span data-stu-id="3b257-164">Specifically, NuGet selects the lower *major.minor* version that satisfies dependencies.</span></span>

<span data-ttu-id="3b257-165">Por padrão, o NuGet 2.8 procura a versão de patch mais baixa (veja [Notas de versão do NuGet 2.8](../release-notes/nuget-2.8.md#patch-resolution-for-dependencies)).</span><span class="sxs-lookup"><span data-stu-id="3b257-165">By default, NuGet 2.8 looks for the lowest patch version (see [NuGet 2.8 release notes](../release-notes/nuget-2.8.md#patch-resolution-for-dependencies)).</span></span> <span data-ttu-id="3b257-166">Você pode controlar essa configuração por meio do atributo `DependencyVersion` em `Nuget.Config` e a opção `-DependencyVersion` na linha de comando.</span><span class="sxs-lookup"><span data-stu-id="3b257-166">You can control this setting through the `DependencyVersion` attribute in `Nuget.Config` and the `-DependencyVersion` switch on the command line.</span></span>  

<span data-ttu-id="3b257-167">O processo `packages.config` para resolver as dependências fica complicado para grandes grafos de dependência.</span><span class="sxs-lookup"><span data-stu-id="3b257-167">The `packages.config` process for resolving dependencies gets complicated for larger dependency graphs.</span></span> <span data-ttu-id="3b257-168">Cada nova instalação do pacote exige uma transversal de todo o grafo e aumenta as chances de conflitos de versão.</span><span class="sxs-lookup"><span data-stu-id="3b257-168">Each new package installation requires a traversal of the whole graph and raises the chance for version conflicts.</span></span> <span data-ttu-id="3b257-169">Quando ocorre um conflito, a instalação é interrompida, deixando o projeto em um estado indeterminado, especialmente com possíveis modificações para o arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="3b257-169">When a conflict occurs, installation is stopped, leaving the project in an indeterminate state, especially with potential modifications to the project file itself.</span></span> <span data-ttu-id="3b257-170">Isso não é um problema ao usar outros formatos de gerenciamento de pacote.</span><span class="sxs-lookup"><span data-stu-id="3b257-170">This is not an issue when using other package management formats.</span></span>

## <a name="managing-dependency-assets"></a><span data-ttu-id="3b257-171">Gerenciamento de ativos de dependência</span><span class="sxs-lookup"><span data-stu-id="3b257-171">Managing dependency assets</span></span>

<span data-ttu-id="3b257-172">Ao usar o formato PackageReference, é possível controlar de quais ativos as dependências fluem no projeto de nível superior.</span><span class="sxs-lookup"><span data-stu-id="3b257-172">When using the PackageReference format, you can control which assets from dependencies flow into the top-level project.</span></span> <span data-ttu-id="3b257-173">Para obter detalhes, veja [PackageReference](package-references-in-project-files.md#controlling-dependency-assets).</span><span class="sxs-lookup"><span data-stu-id="3b257-173">For details, see [PackageReference](package-references-in-project-files.md#controlling-dependency-assets).</span></span>

<span data-ttu-id="3b257-174">Quando o projeto de nível superior for um pacote, você também tem controle sobre esse fluxo usando os atributos `include` e `exclude` com dependências listadas no arquivo `.nuspec`.</span><span class="sxs-lookup"><span data-stu-id="3b257-174">When the top-level project is itself a package, you also have control over this flow by using the `include` and `exclude` attributes with dependencies listed in the `.nuspec` file.</span></span> <span data-ttu-id="3b257-175">Consulte [Referência de .nuspec – Dependências](../reference/nuspec.md#dependencies).</span><span class="sxs-lookup"><span data-stu-id="3b257-175">See [.nuspec Reference - Dependencies](../reference/nuspec.md#dependencies).</span></span>

## <a name="excluding-references"></a><span data-ttu-id="3b257-176">Excluindo referências</span><span class="sxs-lookup"><span data-stu-id="3b257-176">Excluding references</span></span>

<span data-ttu-id="3b257-177">Há cenários em que assemblies com o mesmo nome podem ser referenciados mais de uma vez em um projeto, produzindo erros de tempo de design e de tempo de build.</span><span class="sxs-lookup"><span data-stu-id="3b257-177">There are scenarios in which assemblies with the same name might be referenced more than once in a project, producing design-time and build-time errors.</span></span> <span data-ttu-id="3b257-178">Considere um projeto que contém uma versão personalizada do `C.dll` e faz referência ao Pacote C que também contém `C.dll`.</span><span class="sxs-lookup"><span data-stu-id="3b257-178">Consider a project that contains a custom version of `C.dll`, and references Package C that also contains `C.dll`.</span></span> <span data-ttu-id="3b257-179">Por outro lado, o projeto também depende do Pacote B, que também depende do Pacote C e `C.dll`.</span><span class="sxs-lookup"><span data-stu-id="3b257-179">At the same time, the project also depends on Package B which also depends on Package C and `C.dll`.</span></span> <span data-ttu-id="3b257-180">Como resultado, o NuGet não pode determinar qual `C.dll` usar, mas não é possível você apenas remover a dependência do projeto no pacote C porque o pacote B também depende dele.</span><span class="sxs-lookup"><span data-stu-id="3b257-180">As a result, NuGet can't determine which `C.dll` to use, but you can't just remove the project's dependency on Package C because Package B also depends on it.</span></span>

<span data-ttu-id="3b257-181">Para resolver isso, você precisa referenciar diretamente o `C.dll` que você deseja (ou usar outro pacote que faz referência ao pacote correto) e, em seguida, adiciona uma dependência ao Pacote C que exclui todos os seus ativos.</span><span class="sxs-lookup"><span data-stu-id="3b257-181">To resolve this, you must directly reference the `C.dll` you want (or use another package that references the right one), and then add a dependency on Package C that excludes all its assets.</span></span> <span data-ttu-id="3b257-182">Isso é feito da seguinte maneira dependendo do formato de gerenciamento de pacote em uso:</span><span class="sxs-lookup"><span data-stu-id="3b257-182">This is done as follows depending on the package management format in use:</span></span>

- <span data-ttu-id="3b257-183">[PackageReference](../consume-packages/package-references-in-project-files.md): adicionar `Exclude="All"` à dependência:</span><span class="sxs-lookup"><span data-stu-id="3b257-183">[PackageReference](../consume-packages/package-references-in-project-files.md): add `Exclude="All"` in the dependency:</span></span>

    ```xml
    <PackageReference Include="PackageC" Version="1.0.0" Exclude="All" />
    ```

- <span data-ttu-id="3b257-184">`packages.config`: remova a referência a PackageC do arquivo `.csproj` de forma que ele faça referência somente à versão de `C.dll` que você deseja.</span><span class="sxs-lookup"><span data-stu-id="3b257-184">`packages.config`: remove the reference to PackageC from the `.csproj` file so that it references only the version of `C.dll` that you want.</span></span>
    
## <a name="dependency-updates-during-package-install"></a><span data-ttu-id="3b257-185">Atualizações de dependência durante a instalação do pacote</span><span class="sxs-lookup"><span data-stu-id="3b257-185">Dependency updates during package install</span></span> 

<span data-ttu-id="3b257-186">Se uma versão de dependência já for atendida, a dependência não será atualizada durante outras instalações de pacote.</span><span class="sxs-lookup"><span data-stu-id="3b257-186">If a dependency version is already satisfied, the dependency isn't updated during other package installations.</span></span> <span data-ttu-id="3b257-187">Por exemplo, considere o pacote A que depende do pacote B e especifica 1.0 para o número de versão.</span><span class="sxs-lookup"><span data-stu-id="3b257-187">For example, consider package A that depends on package B and specifies 1.0 for the version number.</span></span> <span data-ttu-id="3b257-188">O repositório de origem contém as versões 1.0, 1.1 e 1.2 do pacote B. Se A estiver instalado em um projeto que já contém a versão 1.0 do B, B 1.0 permanecerá em uso, pois atende à restrição de versão.</span><span class="sxs-lookup"><span data-stu-id="3b257-188">The source repository contains versions 1.0, 1.1, and 1.2 of package B. If A is installed in a project that already contains B version 1.0, then B 1.0 remains in use because it satisfies the version constraint.</span></span> <span data-ttu-id="3b257-189">No entanto, se o pacote A solicitava a versão 1.1 ou superior de B, B 1.2 seria instalado.</span><span class="sxs-lookup"><span data-stu-id="3b257-189">However, if package A had requests version 1.1 or higher of B, then B 1.2 would be installed.</span></span> 

## <a name="resolving-incompatible-package-errors"></a><span data-ttu-id="3b257-190">Resolvendo erros de pacote incompatível</span><span class="sxs-lookup"><span data-stu-id="3b257-190">Resolving incompatible package errors</span></span>

<span data-ttu-id="3b257-191">Durante uma operação de restauração de pacote, você pode vir a encontrar o erro “Um ou mais pacotes não são compatíveis...” ou uma indicação de que um pacote “não é compatível” com a estrutura de destino do projeto.</span><span class="sxs-lookup"><span data-stu-id="3b257-191">During a package restore operation, you may see the error "One or more packages are not compatible..." or that a package "is not compatible" with the project's target framework.</span></span>

<span data-ttu-id="3b257-192">Esse erro ocorre quando um ou mais dos pacotes referenciados em seu projeto não indica que eles dão suporte à estrutura de destino do projeto, ou seja, o pacote não contém uma DLL adequada em sua pasta `lib` para uma estrutura de destino que é compatível com o projeto.</span><span class="sxs-lookup"><span data-stu-id="3b257-192">This error occurs when one or more of the packages referenced in your project do not indicate that they support the project's target framework; that is, the package does not contain a suitable DLL in its `lib` folder for a target framework that is compatible with the project.</span></span> <span data-ttu-id="3b257-193">(Consulte [Estruturas de destino](../reference/target-frameworks.md) para ver uma lista.)</span><span class="sxs-lookup"><span data-stu-id="3b257-193">(See [Target frameworks](../reference/target-frameworks.md) for a list.)</span></span> 

<span data-ttu-id="3b257-194">Por exemplo, se um projeto utiliza `netstandard1.6` e você tentar instalar um pacote que contém as DLLs somente nas pastas `lib\net20` e `\lib\net45`, você verá mensagens com a seguinte para o pacote e, possivelmente, seus dependentes:</span><span class="sxs-lookup"><span data-stu-id="3b257-194">For example, if a project targets `netstandard1.6` and you attempt to install a package that contains DLLs in only the `lib\net20` and `\lib\net45` folders, then you see messages like the following for the package and possibly its dependents:</span></span>

```output
Restoring packages for myproject.csproj...
Package ContosoUtilities 2.1.2.3 is not compatible with netstandard1.6 (.NETStandard,Version=v1.6). Package ContosoUtilities 2.1.2.3 supports:
  - net20 (.NETFramework,Version=v2.0)
  - net45 (.NETFramework,Version=v4.5)
Package ContosoCore 0.86.0 is not compatible with netstandard1.6 (.NETStandard,Version=v1.6). Package ContosoCore 0.86.0 supports:
  - 11 (11,Version=v0.0)
  - net20 (.NETFramework,Version=v2.0)
  - sl3 (Silverlight,Version=v3.0)
  - sl4 (Silverlight,Version=v4.0)
One or more packages are incompatible with .NETStandard,Version=v1.6.
Package restore failed. Rolling back package changes for 'MyProject'.
```

<span data-ttu-id="3b257-195">Para resolver as incompatibilidades, siga um destes procedimentos:</span><span class="sxs-lookup"><span data-stu-id="3b257-195">To resolve incompatibilities, do one of the following:</span></span>

- <span data-ttu-id="3b257-196">Redirecione seu projeto para uma estrutura que é compatível com os pacotes que você deseja usar.</span><span class="sxs-lookup"><span data-stu-id="3b257-196">Retarget your project to a framework that is supported by the packages you want to use.</span></span>
- <span data-ttu-id="3b257-197">Entre em contato com o autor dos pacotes e trabalhe junto a eles para adicionar suporte à estrutura escolhida.</span><span class="sxs-lookup"><span data-stu-id="3b257-197">Contact the author of the packages and work with them to add support for your chosen framework.</span></span> <span data-ttu-id="3b257-198">Cada página de listagem de pacote no [nuget.org](https://www.nuget.org/) tem um link **Contatar os Proprietários** para essa finalidade.</span><span class="sxs-lookup"><span data-stu-id="3b257-198">Each package listing page on [nuget.org](https://www.nuget.org/) has a **Contact Owners** link for this purpose.</span></span>
