---
title: Notas de versão 1.4 do NuGet
description: Notas de versão do NuGet 1.4, incluindo problemas conhecidos, correções de bugs, recursos adicionados e DCRs.
author: karann-msft
ms.author: karann
ms.date: 11/11/2016
ms.topic: conceptual
ms.openlocfilehash: 3f4d712f83ea108a82a1bc4910c2a721a8c24d51
ms.sourcegitcommit: 1d1406764c6af5fb7801d462e0c4afc9092fa569
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43550625"
---
# <a name="nuget-14-release-notes"></a>Notas de versão 1.4 do NuGet

[Notas de versão do NuGet 1.3](../release-notes/nuget-1.3.md) | [notas de versão do NuGet 1.5](../release-notes/nuget-1.5.md)

O NuGet 1.4 foi lançado em 17 de junho de 2011.

## <a name="features"></a>Recursos

### <a name="update-package-improvements"></a>Aprimoramentos do pacote de atualização
O NuGet 1.4 apresenta muitos aprimoramentos ao comando Update-Package, tornando mais fácil de manter os pacotes na mesma versão de vários projetos em uma solução. Por exemplo, ao atualizar um pacote para a versão mais recente, é muito comum que deseja que todos os projetos com que o pacote instalado para ser atualizado para o mesmo verision.

O `Update-Package` comando agora torna mais fácil:

#### <a name="update-all-packages-in-a-single-project"></a>Atualizar todos os pacotes em um único projeto

    Update-Package -Project MvcApplication1

#### <a name="update-a-package-in-all-projects"></a>Atualizar um pacote em todos os projetos

    Update-Package PackageId

#### <a name="update-all-packages-in-all-projects"></a>Atualizar todos os pacotes em todos os projetos

    Update-Package

#### <a name="perform-a-safe-update-on-all-packages"></a>Executar uma atualização "segura" de todos os pacotes
O `-Safe` sinalizador restringe atualizações para versões somente com o mesmo componente de versão principal e secundária. Por exemplo, se versão 1.0.0 de um pacote é instalado, e as versões 1.0.1, 1.0.2 e 1.1 estiverem disponíveis no feed, o `-Safe` sinalizador atualiza o pacote à 1.0.2. Atualizar sem a `-Safe` sinalizador seria atualize o pacote para a versão mais recente, 1.1.

    Update-Package -Safe

### <a name="managing-packages-at-the-solution-level"></a>Gerenciamento de pacotes no nível da solução
Antes do NuGet 1.4, era complicado usando a caixa de diálogo instalar um pacote em vários projetos. Ele necessário iniciar a caixa de diálogo, uma vez por projeto.

O NuGet 1.4 adiciona suporte para instalar/desinstalar/atualização de pacotes em vários projetos ao mesmo tempo. Basta iniciar o clique direito a solução e selecionando o **gerenciar pacotes NuGet** opção de menu.

![Caixa de diálogo de nível Manage NuGet Packages de solução](./media/manage-nuget-packages-solution-dialog.png)

Observe que, na barra de título da caixa de diálogo, o nome da solução é exibido, não o nome de um projeto.
Operações de pacote agora fornecem uma lista de caixas de seleção com a lista de projetos, a que a operação deve ser aplicada.

![Gerenciar a seleção de projeto de pacotes do NuGet](./media/manage-nuget-packages-project-selection.png)

Para obter mais detalhes, consulte o tópico sobre [pacotes de gerenciamento para a solução](../tools/package-manager-ui.md#managing-packages-for-the-solution).

### <a name="constraining-upgrades-to-allowed-versions"></a>Restringindo é atualizado para versões de permissão
Por padrão, ao executar o `Update-Package` comando em um pacote (ou atualizando o pacote usando a caixa de diálogo), ele será atualizado para a versão mais recente no feed. Com o novo suporte para todos os pacotes de atualização, pode haver casos em que você deseja bloquear a um intervalo de versão específica de um pacote. Por exemplo, você pode saber com antecedência que seu aplicativo só funcionará com a versão 2. * de um pacote, mas não 3.0 e superior. Para evitar que acidentalmente a atualização do pacote para 3, o NuGet 1.4 adiciona suporte para restringir o intervalo de versões de pacotes podem ser atualizados para editando manualmente o `packages.config` do arquivo usando o novo `allowedVersions` atributo.

Por exemplo, o exemplo a seguir mostra como bloquear o `SomePackage` pacote da versão 2.0-3.0 (exclusivo) de intervalo.
O `allowedVersions` atributo aceita valores usando o [formato de intervalo de versão](../reference/package-versioning.md#version-ranges-and-wildcards).

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
    <package id="SomePackage" version="2.1.0" allowedVersions="[2.0, 3.0)" />
</packages>
```

Observe que no 1.4, bloqueio de um pacote para um intervalo de versão específico deve ser editado manualmente. No NuGet 1.5 estamos planejando adicionar suporte para o posicionamento desse intervalo por meio de `Install-Package` comando.

### <a name="package-visualizer"></a>Visualizador de pacote
O novo Visualizador de pacote, iniciado por meio de **ferramentas** -> **Library Package Manager** -> **pacote visualizador** opção de menu permite que você visualize facilmente a todos os projetos e suas dependências de pacote em uma solução.

_**Observação importante:** esse recurso se beneficia do suporte DGML no Visual Studio. Criar a visualização só tem suporte no Visual Studio Ultimate. Exibir um diagrama DGML só tem suporte no Visual Studio Premium ou superior._

![Visualizador de pacote](./media/package-visualizer.png)

### <a name="automatic-update-check-for-the-nuget-dialog"></a>Verificação de atualização automática para a caixa de diálogo do NuGet
Algumas versões do NuGet introduzem novos recursos expressados por meio de `.nuspec` arquivo que não são compreendidas por versões mais antigas da caixa de diálogo do NuGet.
Um exemplo é a introdução em NuGet 1.4 para [especificando os assemblies do framework](../release-notes/nuget-1.2.md#framework-assembly-refs).
Por isso, é importante usar a versão mais recente do NuGet para garantir que você pode usar pacotes aproveitando os recursos mais recentes.
Para fazer atualizações para o NuGet mais visível, a caixa de diálogo do NuGet contém a lógica para realçar quando uma versão mais recente está disponível.

_**Observação**: A verificação é feita apenas se o **Online** guia foi selecionada na sessão atual._

![Gerenciar a caixa de diálogo de pacotes do NuGet com a nova versão disponível](./media/manage-nuget-packages-update-notification.png)

Para desativar a verificação automática de atualizações, vá para a caixa de diálogo de configurações do NuGet e desmarque a opção **verifique automaticamente atualizações**.

![Configurações do NuGet](./media/nuget-settings.png)

Esse recurso, na verdade, foi adicionado na versão 1.3 do NuGet, mas não será visível, é claro que, até que uma atualização para 1.3, como o NuGet 1.4, foi disponibilizado.

### <a name="package-manager-dialog-improvements"></a>Aprimoramentos do diálogo do Gerenciador de pacotes
* **Nomes de menu aprimorado**: opções de Menu para iniciar a caixa de diálogo foram renomeadas para maior clareza. A opção de menu é agora **gerenciar pacotes NuGet**.
* **Painel de detalhes mostra a data de atualização mais recente**: caixa de diálogo o NuGet exibe a data da atualização mais recente no painel de detalhes para um pacote quando o **Online** ou **atualiza** guia é selecionada.
* **Lista de marcas exibido**: caixa de diálogo o Nuget exibe marcas.

### <a name="powershell-improvements"></a>Melhorias do PowerShell
* **Scripts do PowerShell assinados**: NuGet inclui scripts assinados do Powershell, permitindo o uso em ambientes mais restritivas.
* **Solicitar suporte**: O Package Manager Console agora dá suporte a solicitar por meio de `$host.ui.Prompt` e `$host.ui.PromptForChoice` comandos.
* **Nomes de origem do pacote**: o nome de uma origem de pacote é suportado ao especificar uma origem de pacote usando o `-Source` sinalizador.

### <a name="nugetexe-command-line-improvements"></a>aprimoramentos de linha de comando NuGet.exe
* **Comandos do NuGet personalizado**: nuget.exe é extensível por meio de comandos personalizados usando o MEF.
* **Mais simples do fluxo de trabalho para a criação de pacotes de símbolos**: O `-Symbols` sinalizador pode ser aplicado a uma estrutura de pastas de convenção normal com base em criação de um pacote de símbolos, incluindo apenas o código-fonte e `.pdb` arquivos dentro da pasta.
* **Especificar várias fontes**: O `NuGet install` comando dá suporte à especificação de várias fontes usando ponto e vírgula como delimitador ou especificando `-Source` várias vezes.
* **Suporte de autenticação de proxy**: NuGet 1.4 adiciona suporte para solicitar as credenciais do usuário ao usar o NuGet atrás de um proxy que exija autenticação.
* **NuGet.exe atualizar alteração significativa**: O `-Self` sinalizador agora é necessário para nuget.exe atualize a mesmo. `nuget.exe Update` agora entra em um caminho para o `packages.config` de arquivo e tentará atualizar pacotes. Observe que essa atualização é limitada em que ele não será: * * atualizar, adicionar, remover o conteúdo no arquivo de projeto.
* * Executar scripts do Powershell dentro do pacote.

### <a name="nuget-server-support-for-pushing-packages-using-nugetexe"></a>Suporte de servidor do NuGet para enviar por push pacotes usando nuget.exe
NuGet inclui uma maneira simples de host uma [web leve com base em repositório NuGet](../hosting-packages/nuget-server.md) por meio de `NuGet.Server` pacote do NuGet. Com o NuGet 1.4, o servidor leve dá suporte a envio por push e excluir pacotes usando nuget.exe.
A versão mais recente do `NuGet.Server` adiciona um novo `appSetting`, denominado `apiKey`. Quando a chave for omitida ou deixada em branco, o push de pacotes para o feed está desabilitado. Configurar o apiKey para um valor (o ideal é que uma senha forte) permite enviar por push pacotes usando nuget.exe.

```xml
<appSettings>
    <!-- Set the value here to allow people to push/delete packages from the server.
            NOTE: This is a shared key (password) for all users. -->
    <add key="apiKey" value="" />
</appSettings>
```

### <a name="support-for-windows-phone-tools-mango-edition"></a>Suporte para Windows Phone ferramentas Mango Edition
A versão release candidate das ferramentas do Windows Phone para Mango agora tem suporte ao NuGet.
Atualmente, as ferramentas do Windows Phone não tem suporte para o Gerenciador de extensão do Visual Studio, então instalar o NuGet para ferramentas do Windows Phone, talvez você precise baixar e executar o VSIX manualmente.

Para desinstalar o NuGet para ferramentas do Windows Phone, execute o comando a seguir.

     vsixinstaller.exe /uninstall:NuPackToolsVsix.Microsoft.67e54e40-0ae3-42c5-a949-fddf5739e7a5

## <a name="bug-fixes"></a>Correções de Bug
O NuGet 1.4 tinha um total de 88 fixos de itens de trabalho. 71 deles foram marcados como bugs.

Para obter uma lista completa de trabalho itens corrigidos no NuGet 1.4, por favor, modo de exibição de [rastreador de problemas do NuGet para esta versão](http://nuget.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=NuGet%201.4&assignedTo=All&component=All&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

## <a name="bug-fixes-worth-noting"></a>Correções de bug, vale a pena observar:

* [Problema 603](http://nuget.codeplex.com/workitem/603): dependências de pacotes em diferentes repositórios resolve corretamente ao especificar uma origem de pacote específico.
* [Problema 1036](http://nuget.codeplex.com/workitem/1036): adicionando `NuGet Pack SomeProject.csproj` como evento pós-compilação não faz com que um loop infinito.
* [Problema 961](http://nuget.codeplex.com/workitem/961): `-Source` sinalizador dá suporte a caminhos relativos.

## <a name="nuget-14-update"></a>Atualização do NuGet 1.4
Logo após o lançamento do NuGet 1.4, descobrimos alguns problemas que foram importantes para corrigir.
O número de versão específico dessa atualização para 1.4 é 1.4.20615.9020.

### <a name="bug-fixes"></a>Correções de Bug
* [Problema 1220](http://nuget.codeplex.com/workitem/1220): não executa o pacote de atualização `install.ps1` / `uninstall.ps1` em todos os projetos quando há mais de um projeto
* [Problema 1156](http://nuget.codeplex.com/workitem/1156): console do Gerenciador de pacotes preso no W2K3/XP (quando 2 do Powershell não está instalado)
