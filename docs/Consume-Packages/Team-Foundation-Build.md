---
title: "Explicação passo a passo da restauração de pacotes do NuGet com o Team Foundation Build | Microsoft Docs"
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 1/9/2017
ms.topic: article
ms.prod: nuget
ms.technology: 
ms.assetid: 3113cccd-35f7-4980-8a6e-fc06556b5064
description: Um passo a passo de como restaurar pacotes do NuGet com o Team Foundation Build (no TFS e no Visual Studio Team Services).
keywords: "Restauração do pacote do NuGet, NuGet e TFS, NuGet e VSTS, sistemas de build do NuGet, build do team foundation, projetos do MSBuild personalizados, build na nuvem, integração contínua"
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: 4be1bb83549958897a15d690439cac073c9683d1
ms.sourcegitcommit: d0ba99bfe019b779b75731bafdca8a37e35ef0d9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/14/2017
---
# <a name="setting-up-package-restore-with-team-foundation-build"></a>Configurando a restauração de pacote com o Team Foundation Build

> Aplica-se a:
>  - Projetos do MSBuild personalizados executados em qualquer versão do TFS
>  - Team Foundation Server 2012 ou superior
>  - Modelos do Team Foundation Build Process personalizados migrados para o TFS 2013 ou posterior
>  - Compilar modelos de processo com a funcionalidade de restauração do Nuget removida
>
> Se você estiver usando o Visual Studio Team Services ou o Team Foundation Server 2013 local com seus modelos de processo de build, a Restauração de Pacote Automática ocorrerá como parte do processo de build.

Esta seção fornecerá instruções passo a passo detalhadas sobre como restaurar os pacotes como parte do [Team Foundation Build](http://msdn.microsoft.com/library/ms181710(v=VS.90).aspx) para [git](http://en.wikipedia.org/wiki/Git_(software)) e para o [Controle de Versão do TF](http://msdn.microsoft.com/library/ms181237(v=vs.120).aspx).

Embora este passo a passo seja específico para o cenário que usa o [Team Foundation Service](http://tfs.visualstudio.com/), os conceitos também se aplicam a outros controle de versão e sistemas de build.

## <a name="the-general-approach"></a>A abordagem geral

Uma vantagem de usar o NuGet é que você pode usá-lo para evitar fazer check in de binários para seu sistema de controle de versão.

Isso é especialmente interessante se você estiver usando um sistema de [controle de versão distribuída](http://en.wikipedia.org/wiki/Distributed_revision_control) como git porque os desenvolvedores precisam clonar o repositório inteiro, incluindo o histórico completo, para começar a trabalhar localmente. Fazer check-in de binários pode causar sobrecarga significativa no repositório, visto que arquivos binários normalmente são armazenados sem compactação delta.

O NuGet tem sido compatível com a [restauração de pacotes](../consume-packages/package-restore.md) como parte do build há bastante tempo. A implementação anterior tinha um problema de ovo e a galinha para os pacotes que desejam estender o processo de build porque os pacotes restaurados do NuGet durante a compilação do projeto. No entanto, o MSBuild não permite estender o build durante o build; poderíamos argumentar que isso é um problema no MSBuild, mas eu diria que se trata de um problema inerente. Dependendo de qual aspecto você precisa estender, pode ser muito tarde registrar no momento em que o pacote é restaurado.

A solução para esse problema é verificar se os pacotes foram restaurados como a primeira etapa no processo de build. O NuGet 2.7 ou superior torna isso fácil por meio de uma linha de comando simplificada:

```
nuget restore path\to\solution.sln
```

Quando o processo de build restaura os pacotes antes de compilar o código, você não precisa fazer check-in de arquivos `.targets`

> [!Note]
> Os pacotes devem ser criados para permitir o carregamento no Visual Studio. Caso contrário, ainda pode ser recomendável fazer check-in em arquivos `.targets` para que outros desenvolvedores possam simplesmente abrir a solução sem a necessidade de restaurar os pacotes.

O seguinte projeto de demonstração mostra como configurar o build de forma que as pastas `packages` os arquivos `.targets` não precisam passar por check-in. Ele também mostra como configurar um build automatizado no Team Foundation Service para este projeto de exemplo.

## <a name="repository-structure"></a>Estrutura do repositório

Nosso projeto de demonstração é uma ferramenta de linha de comando simples que usa o argumento de linha de comando para consultar o Bing. Ele se destina ao .NET Framework 4 e usa muitos dos [pacotes BCL](http://www.nuget.org/profiles/dotnetframework/) ([Microsoft.Net.Http](http://www.nuget.org/packages/Microsoft.Net.Http), [Microsoft.Bcl](http://www.nuget.org/packages/Microsoft.Bcl), [Microsoft.Bcl.Async](http://www.nuget.org/packages/Microsoft.Bcl.Async) e [Microsoft.Bcl.Build](http://www.nuget.org/packages/Microsoft.Bcl.Build)).

A estrutura do repositório será semelhante ao seguinte:

    <Project>
        │   .gitignore
        │   .tfignore
        │   build.proj
        │
        ├───src
        │   │   BingSearcher.sln
        │   │
        │   └───BingSearcher
        │       │   App.config
        │       │   BingSearcher.csproj
        │       │   packages.config
        │       │   Program.cs
        │       │
        │       └───Properties
        │               AssemblyInfo.cs
        │
        └───tools
            └───NuGet
                    nuget.exe

Você pode ver que não fizemos check-in na pasta `packages` nem qualquer arquivo `.targets`.

No entanto, fizemos check-in no `nuget.exe`, pois ele é necessário durante o build. Seguindo as convenções mais usadas, nós o colocamos em uma pasta `tools` compartilhada.

O código-fonte está na pasta `src`. Embora a nossa demonstração use apenas uma única solução, é possível imaginar facilmente que essa pasta contém mais de uma solução.

### <a name="ignore-files"></a>Ignorar arquivos

> [!Note]
> Atualmente, há um [bug conhecido no cliente do NuGet](https://nuget.codeplex.com/workitem/4072) que faz com que o cliente ainda adicione a pasta `packages` ao controle de versão. Uma solução alternativa é desabilitar a integração de controle do código-fonte. Para fazer isso, você precisará de um arquivo `Nuget.Config ` na pasta `.nuget` é paralela à sua solução. Se essa pasta ainda não existir, você precisará criá-la. No [`Nuget.Config`](../consume-packages/configuring-nuget-behavior.md), adicione o conteúdo a seguir:

```xml
<configuration>
    <solution>
        <add key="disableSourceControlIntegration" value="true" />
    </solution>
</configuration>
```


Para se comunicar com o controle de versão que nós não temos intenção de fazer check-in nas pastas **pacotes**, também adicionamos ignorar arquivos ao git (`.gitignore`) e ao controle de versão do TF (`.tfignore`). Esses arquivos descrevem padrões dos arquivos para os quais você não deseja fazer check-in.

O arquivo `.gitignore` se parece com o seguinte:

    syntax: glob
    *.user
    *.suo
    bin
    obj
    packages

O arquivo `.gitignore` é [muito poderoso](https://www.kernel.org/pub/software/scm/git/docs/gitignore.html). Por exemplo, se você geralmente não quiser fazer check-in do conteúdo da pasta `packages`, mas deseja prosseguir com a orientação anterior de fazer check-in nos arquivos `.targets`, você poderia ter a seguinte regra em vez disso:

    packages
    !packages/**/*.targets

Isso excluirá todas as pastas `packages`, mas incluirá novamente todos os arquivos `.targets` contidos. Aliás, você pode encontrar um modelo para arquivos `.gitignore` especificamente projetado para as necessidades de desenvolvedores do Visual Studio [aqui](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).

O controle de versão do TF dá suporte a um mecanismo muito semelhante por meio do arquivo [.tfignore](http://msdn.microsoft.com/library/ms245454.aspx). A sintaxe é praticamente a mesma:

    *.user
    *.suo
    bin
    obj
    packages

## <a name="buildproj"></a>build.proj

Para nossa demonstração, manteremos o processo de build bastante simples. Vamos criar um projeto do MSBuild que cria todas as soluções enquanto verifica se os pacotes são restaurados antes de compilar as soluções.

Este projeto terá os três destinos convencionais `Clean`, `Build` e `Rebuild`, bem como um novo destino `RestorePackages`.

- Os destinos de `Build` e `Rebuild` dependem do `RestorePackages`. Isso garante que você pode tanto executar `Build` e `Rebuild` quanto contar com os pacotes que estão sendo restaurados.
- `Clean`, `Build` e `Rebuild` invocam o destino do MSBuild correspondente em todos os arquivos de solução.
- O destino `RestorePackages` invoca `nuget.exe` para cada arquivo de solução. Isso é feito usando a [funcionalidade de envio em lotes do MSBuild](http://msdn.microsoft.com/library/ms171473.aspx).

O resultado se parece com o seguinte:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project ToolsVersion="4.0"
            DefaultTargets="Build"
            xmlns="http://schemas.microsoft.com/developer/msbuild/2003">

    <PropertyGroup>
    <OutDir Condition=" '$(OutDir)'=='' ">$(MSBuildThisFileDirectory)bin\</OutDir>
    <Configuration Condition=" '$(Configuration)'=='' ">Release</Configuration>
    <SourceHome Condition=" '$(SourceHome)'=='' ">$(MSBuildThisFileDirectory)src\</SourceHome>
    <ToolsHome Condition=" '$(ToolsHome)'=='' ">$(MSBuildThisFileDirectory)tools\</ToolsHome>
    </PropertyGroup>

    <ItemGroup>
    <Solution Include="$(SourceHome)*.sln">
        <AdditionalProperties>OutDir=$(OutDir);Configuration=$(Configuration)</AdditionalProperties>
    </Solution>
    </ItemGroup>

    <Target Name="RestorePackages">
    <Exec Command="&quot;$(ToolsHome)NuGet\nuget.exe&quot; restore &quot;%(Solution.Identity)&quot;" />
    </Target>

    <Target Name="Clean">
    <MSBuild Targets="Clean"
                Projects="@(Solution)" />
    </Target>

    <Target Name="Build" DependsOnTargets="RestorePackages">
    <MSBuild Targets="Build"
                Projects="@(Solution)" />
    </Target>

    <Target Name="Rebuild" DependsOnTargets="RestorePackages">
    <MSBuild Targets="Rebuild"
                Projects="@(Solution)" />
    </Target>
</Project>
```

## <a name="configuring-team-build"></a>Configurar o Team Build

O Team Build oferece vários modelos de processo. Para esta demonstração, estamos usando o Team Foundation Service. Contudo, instalações locais do TFS serão muito semelhantes.

O Git e o Controle de Versão do TF têm diferentes modelos de Team Build, por isso as etapas a seguir variarão dependendo de qual sistema de controle de versão você está usando. Em ambos os casos, tudo o que você precisa é selecionar o build.proj como o projeto que você deseja compilar.

Primeiro, vamos analisar o modelo de processo para o git. No modelo baseado em git, o build é selecionado por meio da propriedade `Solution to build`:

![Processo de build para git](media/PackageRestoreTeamBuildGit.png)

Observe que essa propriedade é um local em seu repositório. Como nosso `build.proj` está na raiz, simplesmente usamos `build.proj`. Se você colocar o arquivo de build em uma pasta chamada `tools`, o valor será `tools\build.proj`.

No modelo de controle de versão do TF, o projeto é selecionado por meio da propriedade `Projects`:

![Processo de build para TFVC](media/PackageRestoreTeamBuildTFVC.png)

Em contraste com modelo baseado em git, o controle de versão do TF é compatível com seletores (o botão do lado direito com três pontos). Para evitar erros de digitação, sugerimos usá-los para selecionar o projeto.