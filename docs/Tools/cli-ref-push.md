---
title: Comando de envio por push do NuGet CLI | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 10/24/2017
ms.topic: reference
ms.prod: nuget
ms.technology: 
ms.assetid: a9709eee-add2-47fb-98e6-eec0697087f6
description: "Referência para o comando de envio de nuget.exe"
keywords: "referência de push NuGet, o comando de envio"
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: 2828cdc41903d8a948870155b23721724bfa781e
ms.sourcegitcommit: d0ba99bfe019b779b75731bafdca8a37e35ef0d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/14/2017
---
# <a name="push-command-nuget-cli"></a>comando de envio (NuGet CLI)

**Aplica-se a:** pacote publicação &bullet; **versões com suporte:** todos; 4.1.0+ necessário para nuget.org

> [!Important]
> Para enviar pacotes para nuget.org, você deve usar nuget.exe v4.1.0 +, que implementa o necessária [NuGet protocolos](../api/nuget-protocols.md).

Envia um pacote para uma origem de pacote e o publica.

Configuração de padrão do NuGet é obtida Carregando `%AppData%\NuGet\NuGet.Config`, em seguida, carregar qualquer `Nuget.Config` ou `.nuget\Nuget.Config` arquivos a partir da raiz da unidade e terminando no diretório atual (consulte [Configurando o comportamento do NuGet](../consume-packages/configuring-nuget-behavior.md))

## <a name="usage"></a>Uso

```
nuget push <packagePath> [options]
```

onde `<packagePath>` identifica o pacote para enviar por push para o servidor.

## <a name="options"></a>Opções

| Opção | Descrição |
| --- | --- |
| apiKey | A chave de API para o repositório de destino. Se não estiverem presentes, especificada na *%AppData%\NuGet\NuGet.Config* é usado. |
| ConfigFile | *(2.5 +)*  NuGet o arquivo de configuração para aplicar. Se não for especificado, *%AppData%\NuGet\NuGet.Config* é usado. |
| DisableBuffering | Desabilita o buffer de envio por push para um servidor HTTP (s) para diminuir o uso de memória. Cuidado: quando esta opção for usada, a autenticação integrada do Windows pode não funcionar. |
| ForceEnglishOutput | *(3.5 +)*  Força nuget.exe para ser executado usando uma cultura invariável, com base em inglês. |
| Ajuda | Exibe informações de ajuda para o comando. |
| Não interativo | Suprime avisos para a entrada do usuário ou confirmações. |
| NoSymbols | *(3.5 +)*  Se existe um pacote de símbolos, ele não será enviado a um servidor de símbolos. |
| Origem | Especifica a URL do servidor. Com o NuGet 2.5 + NuGet identificará um UNC ou uma fonte de pasta local e simplesmente copiar o arquivo em vez de enviar por push usando HTTP.  Além disso, a partir do NuGet 3.4.2, este parâmetro é obrigatório, a menos que o `NuGet.Config` arquivo Especifica um *DefaultPushSource* valor (consulte [NuGet Configurando comportamento](../Consume-Packages/Configuring-NuGet-Behavior.md)). |
| SymbolSource | *(3.5 +)*  Especifica a URL do servidor de símbolo; nuget.smbsrc.net é usado ao enviar para nuget.org |
| SymbolApiKey | *(3.5 +)*  Especifica a chave de API para a URL especificada em `-SymbolSource`. |
| Tempo limite | Especifica o tempo limite, em segundos, para enviar por push para um servidor. O padrão é 300 segundos (5 minutos). |
| Detalhamento | Especifica a quantidade de detalhes exibidos na saída: *normal*, *silencioso*, *detalhadas (2.5 +)*. |

Consulte também [variáveis de ambiente](cli-ref-environment-variables.md)

## <a name="examples"></a>Exemplos

```
nuget push foo.nupkg

nuget push foo.symbols.nupkg

nuget push foo.nupkg -Timeout 360

nuget push *.nupkg

nuget.exe push -source \\mycompany\repo\ mypackage.1.0.0.nupkg

nuget push foo.nupkg 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -Source https://api.nuget.org/v3/index.json

nuget push foo.nupkg 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a

nuget push foo.nupkg 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -s https://customsource/
```