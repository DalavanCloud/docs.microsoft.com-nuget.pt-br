---
title: Guia de Introdução para criar e publicar um pacote do NuGet do .NET Framework usando o Visual Studio
description: Tutorial passo a passo sobre como criar e publicar um pacote NuGet do .NET Framework usando o Visual Studio 2017.
author: kraigb
ms.author: kraigb
manager: douge
ms.date: 03/13/2018
ms.topic: quickstart
ms.openlocfilehash: 01760034a121b1ff6f227e006415779898c4cf6d
ms.sourcegitcommit: a6ca160b1e7e5c58b135af4eba0e9463127a59e8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-and-publish-a-package-using-visual-studio-net-framework"></a><span data-ttu-id="0e5c4-103">Início Rápido: Criar e publicar um pacote usando o Visual Studio (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="0e5c4-103">Quickstart: Create and publish a package using Visual Studio (.NET Framework)</span></span>

<span data-ttu-id="0e5c4-104">Para criar um pacote NuGet em uma Biblioteca de Classes .NET Framework, é necessário criar a DLL no Visual Studio e, em seguida, usar a ferramenta de linha de comando nuget.exe para criar e publicar o pacote.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-104">Creating a NuGet package from a .NET Framework Class Library involves creating the DLL in Visual Studio, then using the nuget.exe command line tool to create and publish the package.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0e5c4-105">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="0e5c4-105">Prerequisites</span></span>

1. <span data-ttu-id="0e5c4-106">Instale qualquer edição do Visual Studio 2017 de [visualstudio.com](https://www.visualstudio.com/) usando qualquer carga de trabalho relacionada ao .NET.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-106">Install any edition of Visual Studio 2017 from [visualstudio.com](https://www.visualstudio.com/) with any .NET-related workload.</span></span> <span data-ttu-id="0e5c4-107">O Visual Studio 2017 inclui automaticamente os recursos do NuGet quando uma carga de trabalho do .NET é instalada.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-107">Visual Studio 2017 automatically includes NuGet capabilities when a .NET workload is installed.</span></span>

1. <span data-ttu-id="0e5c4-108">Instale a CLI do `nuget.exe` baixando-a de [nuget.org](https://dist.nuget.org/win-x86-commandline/latest/nuget.exe), salvando o arquivo `.exe` em uma pasta adequada e adicionando pasta a variável de ambiente PATH.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-108">Install the `nuget.exe` CLI by downloading it from [nuget.org](https://dist.nuget.org/win-x86-commandline/latest/nuget.exe), saving that `.exe` file to a suitable folder, and adding that folder to your PATH environment variable.</span></span>

1. <span data-ttu-id="0e5c4-109">[Registre-se em uma conta gratuita em nuget.org](https://www.nuget.org/users/account/LogOn?returnUrl=%2F), se ainda não tiver uma.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-109">[Register for a free account on nuget.org](https://www.nuget.org/users/account/LogOn?returnUrl=%2F) if you don't have one already.</span></span> <span data-ttu-id="0e5c4-110">Criar uma nova conta envia um email de confirmação.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-110">Creating a new account sends a confirmation email.</span></span> <span data-ttu-id="0e5c4-111">Você deve confirmar a conta antes de poder carregar um pacote.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-111">You must confirm the account before you can upload a package.</span></span>

## <a name="create-a-class-library-project"></a><span data-ttu-id="0e5c4-112">Criar um projeto de biblioteca de classes</span><span class="sxs-lookup"><span data-stu-id="0e5c4-112">Create a class library project</span></span>

<span data-ttu-id="0e5c4-113">Você pode usar um projeto existente da Biblioteca de Classes .NET Framework para o código que desejar empacotar ou criar um projeto simples da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="0e5c4-113">You can use an existing .NET Framework Class Library project for the code you want to package, or create a simple one as follows:</span></span>

1. <span data-ttu-id="0e5c4-114">No Visual Studio, escolha **Arquivo > Novo > Projeto**, selecione o nó **Visual C#**, selecione o modelo “Biblioteca de Classes (.NET Framework)”, nomeie o projeto como AppLogger e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-114">In Visual Studio, choose **File > New > Project**, select the **Visual C#** node, select the "Class Library (.NET Framework)" template, name the project AppLogger, and click **OK**.</span></span>

1. <span data-ttu-id="0e5c4-115">Clique com botão direito do mouse no arquivo de projeto resultante e selecione **Build** para verificar se o projeto foi criado corretamente.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-115">Right-click on the resulting project file and select **Build** to make sure the project was created properly.</span></span> <span data-ttu-id="0e5c4-116">A DLL é encontrada dentro da pasta de Depuração (ou Versão, se você compilar essa configuração).</span><span class="sxs-lookup"><span data-stu-id="0e5c4-116">The DLL is found within the Debug folder (or Release if you build that configuration instead).</span></span>

<span data-ttu-id="0e5c4-117">Em um pacote NuGet real, obviamente, você implementa muitos recursos úteis com os quais outras pessoas podem compilar aplicativos.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-117">Within a real NuGet package, of course, you implement many useful features with which others can build applications.</span></span> <span data-ttu-id="0e5c4-118">Você também poderá definir as estruturas de destino da maneira que desejar.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-118">You can also set the target frameworks however you like.</span></span> <span data-ttu-id="0e5c4-119">Por exemplo, veja os guias para a [UWP](../guides/create-uwp-packages.md) e para o [Xamarin](../guides/create-packages-for-xamarin.md).</span><span class="sxs-lookup"><span data-stu-id="0e5c4-119">For example, see the guides for [UWP](../guides/create-uwp-packages.md) and [Xamarin](../guides/create-packages-for-xamarin.md).</span></span>

<span data-ttu-id="0e5c4-120">Para este passo a passo, no entanto, você não escreverá nenhum código adicional porque uma biblioteca de classes do modelo é suficiente para criar um pacote.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-120">For this walkthrough, however, you won't write any additional code because a class library from the template is sufficient to create a package.</span></span> <span data-ttu-id="0e5c4-121">Ainda assim, se você desejar algum código funcional para o pacote, use o seguinte:</span><span class="sxs-lookup"><span data-stu-id="0e5c4-121">Still, if you'd like some functional code for the package, use the following:</span></span>

```cs
using System;

namespace AppLogger
{
    public class Logger
    {
        public void Log(string text)
        {
            Console.WriteLine(text);
        }
    }
}
```

> [!Tip]
> <span data-ttu-id="0e5c4-122">A menos que você tenha um motivo para a escolha de outro jeito, o .NET Standard é o destino preferencial para pacotes NuGet, pois fornece compatibilidade com a mais ampla variedade de projetos de consumo.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-122">Unless you have a reason to choose otherwise, .NET Standard is the preferred target for NuGet packages, as it provides compatibility with the widest range of consuming projects.</span></span> <span data-ttu-id="0e5c4-123">Veja [Criar e publicar um pacote usando o Visual Studio (.NET Standard)](create-and-publish-a-package-using-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="0e5c4-123">See [Create and publish a package using Visual Studio (.NET Standard)](create-and-publish-a-package-using-visual-studio.md).</span></span>

## <a name="configure-project-properties-for-the-package"></a><span data-ttu-id="0e5c4-124">Configurar propriedades do projeto para o pacote</span><span class="sxs-lookup"><span data-stu-id="0e5c4-124">Configure project properties for the package</span></span>

<span data-ttu-id="0e5c4-125">Um pacote NuGet contém um manifesto (um arquivo `.nuspec`), que contém metadados relevantes, como o identificador do pacote, o número de versão, a descrição e muito mais.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-125">A NuGet package contains a manifest (a `.nuspec` file), that contains relevant metadata such as the package identifier, version number, description, and more.</span></span> <span data-ttu-id="0e5c4-126">Alguns deles podem ser obtidos diretamente das propriedades do projeto, o que evita a necessidade de atualizá-los separadamente no projeto e no manifesto.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-126">Some of these can be drawn from the project properties directly, which avoids having to separately update them in both the project and the manifest.</span></span> <span data-ttu-id="0e5c4-127">Esta seção descreve onde definir as propriedades aplicáveis.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-127">This section describes where to set the applicable properties.</span></span>

1. <span data-ttu-id="0e5c4-128">Selecione o comando de menu **Projeto > Propriedades** e, em seguida, selecione a guia **Aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-128">Select the **Project > Properties** menu command, then select the **Application** tab.</span></span>

1. <span data-ttu-id="0e5c4-129">No campo **Nome do assembly**, dê um identificador exclusivo ao pacote.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-129">In the **Assembly name** field, give your package a unique identifier.</span></span>

    > [!Important]
    > <span data-ttu-id="0e5c4-130">Você precisa dar ao pacote um identificador exclusivo em nuget.org ou qualquer host que você esteja usando.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-130">You must give the package an identifier that's unique across nuget.org or whatever host you're using.</span></span> <span data-ttu-id="0e5c4-131">Para este passo a passo, recomendamos incluir o "Sample" ou "Test" no nome, pois a etapa de publicação posterior torna o pacote publicamente visível (embora seja improvável que alguém possa usá-lo).</span><span class="sxs-lookup"><span data-stu-id="0e5c4-131">For this walkthrough we recommend including "Sample" or "Test" in the name as the later publishing step does make the package publicly visible (though it's unlikely anyone will actually use it).</span></span>
    >
    > <span data-ttu-id="0e5c4-132">Se você tentar publicar um pacote com um nome que já existe, você verá um erro.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-132">If you attempt to publish a package with a name that already exists, you see an error.</span></span>

1. <span data-ttu-id="0e5c4-133">Selecione o botão **Informações do Assembly...**, que abrirá uma caixa de diálogo na qual será possível inserir outras propriedades contidas no manifesto (veja [Referência do arquivo .nuspec – tokens de substituição](../reference/nuspec.md#replacement-tokens)).</span><span class="sxs-lookup"><span data-stu-id="0e5c4-133">Select the **Assembly Information...** button, which brings up a dialog box in which you can enter other properties that carry into the manifest (see [.nuspec file reference - replacement tokens](../reference/nuspec.md#replacement-tokens)).</span></span> <span data-ttu-id="0e5c4-134">Os campos mais usados são **Título**, **Descrição**, **Empresa**, **Direitos autorais** e **Versão do assembly**.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-134">The most commonly used fields are **Title**, **Description**, **Company**, **Copyright**, and **Assembly version**.</span></span> <span data-ttu-id="0e5c4-135">Essas propriedades aparecem, por fim, com o pacote em um host, como o nuget.org, portanto deixe-os totalmente descritivos.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-135">These properties ultimately appear with your package on a host like nuget.org, so make sure they're fully descriptive.</span></span>

    ![Informações do assembly em um projeto do .NET Framework no Visual Studio](media/qs_create-vs-01b-project-properties.png)

1. <span data-ttu-id="0e5c4-137">Opcional: para ver e editar as propriedades diretamente, abra o arquivo `Properties/AssemblyInfo.cs` no projeto.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-137">Optional: to see and edit the properties directly, open the `Properties/AssemblyInfo.cs` file in the project.</span></span>

1. <span data-ttu-id="0e5c4-138">Quando as propriedades forem definidas, defina a configuração do projeto como **Versão** e recompile o projeto para gerar a DLL atualizada.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-138">When the properties are set, set the project configuration to **Release** and rebuild the project to generate the updated DLL.</span></span>

## <a name="generate-the-initial-manifest"></a><span data-ttu-id="0e5c4-139">Gerar o manifesto inicial</span><span class="sxs-lookup"><span data-stu-id="0e5c4-139">Generate the initial manifest</span></span>

<span data-ttu-id="0e5c4-140">Com uma DLL disponível e as propriedades do projeto definidas, use o comando `nuget spec` para gerar um arquivo `.nuspec` inicial do projeto.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-140">With a DLL in hand and project properties set, you now use the `nuget spec` command to generate an initial `.nuspec` file from the project.</span></span> <span data-ttu-id="0e5c4-141">Essa etapa inclui os tokens de substituição correspondentes para obter informações do arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-141">This step includes the relevant replacement tokens to draw information from the project file.</span></span>

<span data-ttu-id="0e5c4-142">Execute `nuget spec` apenas uma vez para gerar o manifesto inicial.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-142">You run `nuget spec` only once to generate the initial manifest.</span></span> <span data-ttu-id="0e5c4-143">Ao atualizar o pacote, altere os valores no projeto ou edite o manifesto diretamente.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-143">When updating the package, you either change values in your project or edit the manifest directly.</span></span>

1. <span data-ttu-id="0e5c4-144">Abra um prompt de comando e navegue até a pasta que contém o arquivo de projeto `AppLogger.csproj`.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-144">Open a command prompt and navigate to the project folder containing `AppLogger.csproj` file.</span></span>

1. <span data-ttu-id="0e5c4-145">Execute o seguinte comando: `nuget spec AppLogger.csproj`.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-145">Run the following command: `nuget spec AppLogger.csproj`.</span></span> <span data-ttu-id="0e5c4-146">Ao especificar um projeto, o NuGet cria um manifesto que corresponde ao nome do projeto, nesse caso `AppLogger.nuspec`.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-146">By specifying a project, NuGet creates a manifest that matches the name of the project, in this case `AppLogger.nuspec`.</span></span> <span data-ttu-id="0e5c4-147">Ele também inclui os tokens de substituição no manifesto.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-147">It also include replacement tokens in the manifest.</span></span>

1. <span data-ttu-id="0e5c4-148">Abra `AppLogger.nuspec` em um editor de texto para examinar o conteúdo, que deverá ter a seguinte aparência:</span><span class="sxs-lookup"><span data-stu-id="0e5c4-148">Open `AppLogger.nuspec` in a text editor to examine its contents, which should appear as follows:</span></span>

    ```xml
    <?xml version="1.0"?>
    <package >
      <metadata>
        <id>$id$</id>
        <version>$version$</version>
        <title>$title$</title>
        <authors>$author$</authors>
        <owners>$author$</owners>
        <licenseUrl>http://LICENSE_URL_HERE_OR_DELETE_THIS_LINE</licenseUrl>
        <projectUrl>http://PROJECT_URL_HERE_OR_DELETE_THIS_LINE</projectUrl>
        <iconUrl>http://ICON_URL_HERE_OR_DELETE_THIS_LINE</iconUrl>
        <requireLicenseAcceptance>false</requireLicenseAcceptance>
        <description>$description$</description>
        <releaseNotes>Summary of changes made in this release of the package.</releaseNotes>
        <copyright>Copyright 2018</copyright>
        <tags>Tag1 Tag2</tags>
      </metadata>
    </package>
    ```

## <a name="edit-the-manifest"></a><span data-ttu-id="0e5c4-149">Editar o manifesto</span><span class="sxs-lookup"><span data-stu-id="0e5c4-149">Edit the manifest</span></span>

1. <span data-ttu-id="0e5c4-150">O NuGet gerará erro se você tentar criar um pacote com os valores padrão no arquivo `.nuspec`, portanto edite os campos a seguir antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-150">NuGet produces an error if you try to create a package with default values in your `.nuspec` file, so you must edit the following fields before proceeding.</span></span> <span data-ttu-id="0e5c4-151">Veja [Referência do arquivo .nuspec – elementos únicos](../reference/nuspec.md#single-elements) para obter uma descrição de como eles são usados.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-151">See [.nuspec file reference - single elements](../reference/nuspec.md#single-elements) for a description of how these are used.</span></span>

    - <span data-ttu-id="0e5c4-152">licenseUrl</span><span class="sxs-lookup"><span data-stu-id="0e5c4-152">licenseUrl</span></span>
    - <span data-ttu-id="0e5c4-153">projectUrl</span><span class="sxs-lookup"><span data-stu-id="0e5c4-153">projectUrl</span></span>
    - <span data-ttu-id="0e5c4-154">iconUrl</span><span class="sxs-lookup"><span data-stu-id="0e5c4-154">iconUrl</span></span>
    - <span data-ttu-id="0e5c4-155">releaseNotes</span><span class="sxs-lookup"><span data-stu-id="0e5c4-155">releaseNotes</span></span>
    - <span data-ttu-id="0e5c4-156">marcações</span><span class="sxs-lookup"><span data-stu-id="0e5c4-156">tags</span></span>

1. <span data-ttu-id="0e5c4-157">Para pacotes compilados para consumo público, preste atenção especial à propriedade **Tags**, uma vez que as marcações ajudam outras pessoas a encontrar o pacote em fontes como o nuget.org e a entender o que ele faz.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-157">For packages built for public consumption, pay special attention to the **Tags** property, as tags help others find your package on sources like nuget.org and understand what it does.</span></span>

1. <span data-ttu-id="0e5c4-158">Você também pode adicionar outros elementos ao manifesto nesse momento, conforme descrito em [Referência do arquivo .nuspec](../reference/nuspec.md).</span><span class="sxs-lookup"><span data-stu-id="0e5c4-158">You can also add any other elements to the manifest at this time, as described on [.nuspec file reference](../reference/nuspec.md).</span></span>

1. <span data-ttu-id="0e5c4-159">Salve o arquivo antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-159">Save the file before proceeding.</span></span>

## <a name="run-the-pack-command"></a><span data-ttu-id="0e5c4-160">Executar o comando pack</span><span class="sxs-lookup"><span data-stu-id="0e5c4-160">Run the pack command</span></span>

1. <span data-ttu-id="0e5c4-161">Em um prompt de comando na pasta contendo o arquivo `.nuspec`, execute o comando `nuget pack`.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-161">From a command prompt in the folder containing your `.nuspec` file, run the command `nuget pack`.</span></span>

1. <span data-ttu-id="0e5c4-162">O NuGet gera um arquivo `.nupkg` na forma de *identifier-version.nupkg*, que você encontrará na pasta atual.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-162">NuGet generates a `.nupkg` file in the form of *identifier-version.nupkg*, which you'll find in the current folder.</span></span>

## <a name="publish-the-package"></a><span data-ttu-id="0e5c4-163">Publicar o pacote</span><span class="sxs-lookup"><span data-stu-id="0e5c4-163">Publish the package</span></span>

<span data-ttu-id="0e5c4-164">Depois de ter um arquivo `.nupkg`, publique-o no nuget.org usando `nuget.exe` juntamente com uma chave de API adquirida no nuget.org. Para o nuget.org, você deverá usar o `nuget.exe` 4.1.0 ou superior.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-164">Once you have a `.nupkg` file, you publish it to nuget.org using `nuget.exe` with an API key acquired from nuget.org. For nuget.org you must use `nuget.exe` 4.1.0 or higher.</span></span>

[!INCLUDE [publish-notes](includes/publish-notes.md)]

### <a name="acquire-your-api-key"></a><span data-ttu-id="0e5c4-165">Adquirir a chave de sua API</span><span class="sxs-lookup"><span data-stu-id="0e5c4-165">Acquire your API key</span></span>

[!INCLUDE [publish-api-key](includes/publish-api-key.md)]

### <a name="publish-with-nuget-push"></a><span data-ttu-id="0e5c4-166">Publicar com nuget push</span><span class="sxs-lookup"><span data-stu-id="0e5c4-166">Publish with nuget push</span></span>

1. <span data-ttu-id="0e5c4-167">Altere para a pasta que contém o arquivo `.nupkg`.</span><span class="sxs-lookup"><span data-stu-id="0e5c4-167">Change to the folder containing the `.nupkg` file.</span></span>

1. <span data-ttu-id="0e5c4-168">Execute o seguinte comando, especificando o nome do pacote e substituindo o valor da chave pela chave da API:</span><span class="sxs-lookup"><span data-stu-id="0e5c4-168">Run the following command, specifying your package name and replacing the key value with your API key:</span></span>

    ```cli
    nuget push AppLogger.1.0.0.nupkg qz2jga8pl3dvn2akksyquwcs9ygggg4exypy3bhxy6w6x6 -Source https://api.nuget.org/v3/index.json
    ```

1. <span data-ttu-id="0e5c4-169">O nuget.exe exibe os resultados do processo de publicação:</span><span class="sxs-lookup"><span data-stu-id="0e5c4-169">nuget.exe displays the results of the publishing process:</span></span>

    ```output
    Pushing AppLogger.1.0.0.nupkg to 'https://www.nuget.org/api/v2/package'...
        PUT https://www.nuget.org/api/v2/package/
        Created https://www.nuget.org/api/v2/package/ 6829ms
    Your package was pushed.
    ```

<span data-ttu-id="0e5c4-170">Confira [nuget push](../tools/cli-ref-push.md).</span><span class="sxs-lookup"><span data-stu-id="0e5c4-170">See [nuget push](../tools/cli-ref-push.md).</span></span>

### <a name="publish-errors"></a><span data-ttu-id="0e5c4-171">Erros de publicação</span><span class="sxs-lookup"><span data-stu-id="0e5c4-171">Publish errors</span></span>

[!INCLUDE [publish-errors](includes/publish-errors.md)]

### <a name="manage-the-published-package"></a><span data-ttu-id="0e5c4-172">Gerenciar o pacote publicado</span><span class="sxs-lookup"><span data-stu-id="0e5c4-172">Manage the published package</span></span>

[!INCLUDE [publish-manage](includes/publish-manage.md)]

## <a name="related-topics"></a><span data-ttu-id="0e5c4-173">Tópicos relacionados</span><span class="sxs-lookup"><span data-stu-id="0e5c4-173">Related topics</span></span>

- [<span data-ttu-id="0e5c4-174">Criar um pacote</span><span class="sxs-lookup"><span data-stu-id="0e5c4-174">Create a Package</span></span>](../create-packages/creating-a-package.md)
- [<span data-ttu-id="0e5c4-175">Publicar um pacote</span><span class="sxs-lookup"><span data-stu-id="0e5c4-175">Publish a Package</span></span>](../create-packages/publish-a-package.md)
- [<span data-ttu-id="0e5c4-176">Pacotes de pré-lançamento</span><span class="sxs-lookup"><span data-stu-id="0e5c4-176">Pre-release Packages</span></span>](../create-packages/Prerelease-Packages.md)
- [<span data-ttu-id="0e5c4-177">Suporte a várias estruturas de destino</span><span class="sxs-lookup"><span data-stu-id="0e5c4-177">Support multiple target frameworks</span></span>](../create-packages/supporting-multiple-target-frameworks.md)
- [<span data-ttu-id="0e5c4-178">Controle de versão do pacote</span><span class="sxs-lookup"><span data-stu-id="0e5c4-178">Package versioning</span></span>](../reference/package-versioning.md)
- [<span data-ttu-id="0e5c4-179">Criando pacotes localizados</span><span class="sxs-lookup"><span data-stu-id="0e5c4-179">Creating localized packages</span></span>](../create-packages/creating-localized-packages.md)