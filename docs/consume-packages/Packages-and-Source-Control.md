---
title: Pacotes do NuGet e controle do código-fonte
description: Considerações sobre como tratar pacotes do NuGet nos sistemas de controle de versão e do código-fonte, e como omitir pacotes com git e TFVC.
author: karann-msft
ms.author: karann
ms.date: 03/16/2018
ms.topic: conceptual
ms.openlocfilehash: 0338c3445b2a3d8169158171d97d1e874533a80a
ms.sourcegitcommit: 1d1406764c6af5fb7801d462e0c4afc9092fa569
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43551793"
---
# <a name="omitting-nuget-packages-in-source-control-systems"></a>Omitindo pacotes do NuGet em sistemas de controle do código-fonte

Os desenvolvedores geralmente omitem pacotes do NuGet de seus repositórios de controle do código-fonte e, em vez disso, contam com a [restauração de pacote](package-restore.md) para reinstalar as dependências de um projeto antes de um build.

Alguns dos motivos para contar com a restauração de pacote são:

1. Sistemas de controle de versão distribuídos, como Git, incluem cópias completas de todas as versões de cada arquivo dentro do repositório. Arquivos binários que são atualizados com frequência levam a uma sobrecarga significativa e aumentam o tempo necessário para clonar o repositório.
1. Quando os pacotes são incluídos no repositório, os desenvolvedores são responsáveis por adicionar referências diretamente ao conteúdo do pacote no disco em vez de referenciar os pacotes por meio do NuGet, o que pode levar a nomes de caminho embutido em código no projeto.
1. Fica mais difícil limpar as pastas de pacote não utilizadas da solução, pois pode ser necessário garantir que você não exclua as pastas de pacote ainda em uso.
1. Ao omitir pacotes, você pode manter limites claros de propriedade entre o código e os pacotes de terceiros dos quais você depende. Muitos pacotes do NuGet já são mantidos em seus próprios repositórios de controle do código-fonte.

Embora a restauração de pacote seja o comportamento padrão no NuGet, é necessário certo trabalho manual para omitir pacotes &mdash; ou seja, a pasta `packages` em seu projeto &mdash; do controle do código-fonte, conforme descrito neste artigo.

## <a name="omitting-packages-with-git"></a>Omitir pacotes com Git

Use o [arquivo .gitignore](https://git-scm.com/docs/gitignore) para ignorar pacotes do NuGet (`.nupkg`), a pasta `packages`, `project.assets.json`, entre outros itens. Para referência, veja o [exemplo de `.gitignore` para projetos do Visual Studio](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore):

As partes importantes do arquivo `.gitignore` são:

```gitignore
# Ignore NuGet Packages
*.nupkg

# The packages folder can be ignored because of Package Restore
**/[Pp]ackages/*

# except build/, which is used as an MSBuild target.
!**/[Pp]ackages/build/

# Uncomment if necessary however generally it will be regenerated when needed
#!**/[Pp]ackages/repositories.config

# NuGet v3's project.json files produces more ignorable files
*.nuget.props
*.nuget.targets

# Ignore other intermediate files that NuGet might create. project.lock.json is used in conjunction
# with project.json (NuGet v3); project.assets.json is used in conjunction with the PackageReference
# format (NuGet v4 and .NET Core).
project.lock.json
project.assets.json
```

## <a name="omitting-packages-with-team-foundation-version-control"></a>Omitindo pacotes com o Controle de Versão do Team Foundation

> [!Note]
> Se possível, siga estas instruções *antes* de adicionar o projeto ao controle do código-fonte. Caso contrário, exclua a pasta `packages` manualmente do repositório e confirme essa alteração antes de continuar.

Para desabilitar a integração de controle do código-fonte com TFVC para os arquivos selecionados:

1. Crie uma pasta chamada `.nuget` na pasta da sua solução (no qual o arquivo `.sln` está).
    - Dica: no Windows, para criar essa pasta no Windows Explorer, use o nome `.nuget.` *com* o ponto à direita.

1. Nessa pasta, crie um arquivo chamado `NuGet.Config` e abra-o para edição.

1. Adicione o texto a seguir no mínimo, em que a configuração [disableSourceControlIntegration](../reference/nuget-config-file.md#solution-section) instrui o Visual Studio a ignorar tudo na pasta `packages`:

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <configuration>
       <solution>
           <add key="disableSourceControlIntegration" value="true" />
       </solution>
   </configuration>
   ```

1. Se você estiver usando o TFS 2010 ou anterior, encubra a pasta `packages` nos seus mapeamentos de espaço de trabalho.

1. No TFS 2012 ou posterior, ou com o Visual Studio Team Services, crie um arquivo `.tfignore` conforme descrito em [AddFiles para o servidor](/vsts/tfvc/add-files-server.md?view=vsts#tfignore). Nesse arquivo, inclua o conteúdo abaixo para ignorar explicitamente as modificações na pasta `\packages` no nível do repositório e alguns outros arquivos intermediários. (Você pode criar o arquivo no Windows Explorer usando o nome de um `.tfignore.` com o ponto à direita, mas pode ser necessário desabilitar a opção “Ocultar extensões de arquivos conhecidos” primeiro.):

   ```cli
   # Ignore NuGet Packages
   *.nupkg

   # Ignore the NuGet packages folder in the root of the repository. If needed, prefix 'packages'
   # with additional folder names if it's not in the same folder as .tfignore.   
   packages

   # Exclude package target files which may be required for MSBuild, again prefixing the folder name as needed.
   !packages/*.targets

   # Omit temporary files
   project.lock.json
   project.assets.json
   *.nuget.props
   ```

1. Adicione `NuGet.Config` e `.tfignore` ao controle do código-fonte e confirme suas alterações.
