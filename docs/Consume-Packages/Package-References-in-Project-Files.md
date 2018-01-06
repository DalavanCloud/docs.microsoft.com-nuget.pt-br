---
title: PackageReference do NuGet em arquivos de projeto do Visual Studio | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 7/17/2017
ms.topic: article
ms.prod: nuget
ms.technology: 
ms.assetid: 5a554e9d-1266-48c2-92e8-6dd00b1d6810
description: "Veja detalhes sobre o PackageReference de NuGet em arquivos de projeto compatível com o NuGet 4.0 e posterior e o VS2017"
keywords: "Dependências de pacote do NuGet, referências de pacote, arquivos de projeto, PackageReference, packages.config, project.json, VS2017, Visual Studio 2017, NuGet 4"
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: c8fc9e558557af444d9a35ace36d043a5f6382a7
ms.sourcegitcommit: d0ba99bfe019b779b75731bafdca8a37e35ef0d9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/14/2017
---
# <a name="package-references-packagereference-in-project-files"></a>Referências de pacote (PackageReference) em arquivos de projeto

Pacote de referências, usando o nó `PackageReference`, permitem que você gerencie as dependências do NuGet diretamente nos arquivos de projeto, em vez de precisar de um arquivo `packages.config` ou `project.json` separado. Esse método não afeta outros aspectos do NuGet; por exemplo, as configurações nos arquivos `NuGet.Config` (inclusive origens de pacote) ainda são aplicados conforme explicado em [Configurando o comportamento do NuGet](Configuring-NuGet-Behavior.md).

> [!Important]
> No momento, as referências de pacote são compatíveis somente com o Visual Studio 2017, para projetos .NET Core, projetos .NET Standard e projetos UWP voltados para o Windows 10 Build 15063 (Atualização para Criadores).

A abordagem `PackageReference` permite que você use condições do MSBuild para escolher as referências de pacote por estrutura de destino, configuração, plataforma ou outros agrupamentos. Ele também proporciona um controle refinado sobre as dependências e o fluxo de conteúdo. Em termos de comportamento e [resolução de dependência](Dependency-Resolution.md), é o mesmo que usar `project.json`.

Para obter mais detalhes sobre a integração do MSBuild com referências de pacote em arquivos de projeto, consulte [Empacotamento e restauração do NuGet como destinos do MSBuild](../schema/msbuild-targets.md).

## <a name="adding-a-packagereference"></a>Adicionar um PackageReference

Adicione uma dependência ao arquivo de projeto usando a seguinte sintaxe:

```xml
<ItemGroup>
    <!-- ... -->
    <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0" />    
    <!-- ... -->
</ItemGroup>
```

## <a name="controlling-dependency-version"></a>Controlar a versão de dependência

A convenção para especificar a versão de um pacote é a mesma que usar `packages.config` ou `project.json`:

```xml
<ItemGroup>
    <!-- ... -->
    <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0" />
    <!-- ... -->
</ItemGroup>
```

No exemplo acima, 3.6.0 significa qualquer versão que é >=3.6.0 com preferência para a versão mais antiga, conforme descrito em [Controle de versão do pacote](../reference/package-versioning.md#version-ranges-and-wildcards).

## <a name="floating-versions"></a>Versões flutuantes

[Versões flutuante](../consume-packages/dependency-resolution.md#floating-versions) são compatíveis com `PackageReference`:

```xml
<ItemGroup>
    <!-- ... -->
    <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.*" />
    <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0-beta*" />
    <!-- ... -->
</ItemGroup>
```

## <a name="controlling-dependency-assets"></a>Controlar ativos de dependência

Você pode estar usando uma dependência puramente como uma estrutura de desenvolvimento e pode não querer expor isso aos projetos que consumirão o pacote. Nesse cenário, você pode usar os metadados do `PrivateAssets` para controlar esse comportamento.

```xml
<ItemGroup>
    <!-- ... -->

    <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0">
        <PrivateAssets>all</PrivateAssets>
    </PackageReference>

    <!-- ... -->
</ItemGroup>
```

As seguintes marcas de metadados controlam ativos de dependência:

| Marca | Descrição | Valor padrão |
| --- | --- | --- |
| IncludeAssets | Esses ativos serão consumidos | all |
| ExcludeAssets | Esses ativos não serão consumidos | nenhum | 
| PrivateAssets | Esses ativos serão consumidos, mas não fluem para o projeto pai | contentfiles;analyzers;build |


Os valores permitidos para essas marcas são os seguintes, com vários valores separados por ponto e vírgula, exceto com `all` e `none`, que devem aparecer sozinhos:

| Valor | Descrição |
| --- | ---
| compilar | O conteúdo da pasta `lib` |
| tempo de execução | O conteúdo da pasta `runtime` |
| contentFiles | O conteúdo da pasta `contentfiles` |
| build | Objetos e propriedades na pasta `build` |
| analisadores | Analisadores de .NET |
| nativa | O conteúdo da pasta `native` |
| nenhum | Nenhuma das opções acima é usada. |
| all | Todas as anteriores (exceto `none`) |

No exemplo a seguir, tudo, exceto os arquivos de conteúdo do pacote, poderia ser consumido pelo projeto e tudo, exceto analisadores e arquivos de conteúdo, fluiria para o projeto pai.

```xml
<ItemGroup>
    <!-- ... -->

    <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0">
        <IncludeAssets>all</IncludeAssets>
        <ExcludeAssets>contentFiles</ExcludeAssets>
        <PrivateAssets>contentFiles;analyzers</PrivateAssets>
    </PackageReference>

    <!-- ... -->
</ItemGroup>
```

Observe que, como `build` não está incluído em `PrivateAssets`, destino e objetos *fluirão* para o projeto pai. Considere, por exemplo, que a referência acima é usada em um projeto que compila um pacote do NuGet chamado AppLogger. O AppLogger pode consumir os destinos e objetos de `Contoso.Utility.UsefulStuff`, bem como projetos que consomem AppLogger.

## <a name="adding-a-packagereference-condition"></a>Adicionar uma condição de PackageReference

É possível usar uma condição para controlar se um pacote está incluído, em que as condições podem usar qualquer variável MSBuild ou uma variável definida no arquivo de destinos ou objetos. No entanto, no momento, somente a variável `TargetFramework` é compatível.

Por exemplo, digamos que seu destino é `netstandard1.4` e `net452`, mas tem uma dependência que é aplicável somente para `net452`. Nesse caso, não é aconselhável ter um projeto `netstandard1.4` que está consumindo seu pacote para adicionar essa dependência desnecessária. Para evitar isso, você deve especificar uma condição no `PackageReference` da seguinte maneira:

```xml
<ItemGroup>
    <!-- ... -->
    <PackageReference Include="Newtonsoft.json" Version="9.0.1" Condition="'$(TargetFramework)' == 'net452'" />    
    <!-- ... -->
</ItemGroup>
```

Um pacote compilado usando este projeto mostrará que Newtonsoft.json está incluído como uma dependência somente para um destino `net452`:

![O resultado da aplicação de uma condição em PackageReference com o VS2017](media/PackageReference-Condition.png)

As condições também podem ser aplicadas no nível de `ItemGroup` e serão aplicadas a todos os elementos `PackageReference` filhos:

```xml
<ItemGroup Condition = "'$(TargetFramework)' == 'net452'">
    <!-- ... -->
    <PackageReference Include="Newtonsoft.json" Version="9.0.1" />
    <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0" />
    <!-- ... -->
</ItemGroup>
```