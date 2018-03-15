---
title: "Assinado pacotes referência | Microsoft Docs"
author: rido-min
ms.author: rido-min
manager: unniravindranathan
ms.date: 03/06/2018
ms.topic: reference
ms.prod: nuget
ms.technology: 
description: "Descrição do recurso de pacotes de entrada."
keywords: Entrada de pacote do NuGet, assinatura de certificado
ms.reviewer:
- ananguar
- karann-msft
- unniravindranathan
ms.openlocfilehash: 9bf9885aaf42bedb681a5d916202fa8b26749a0c
ms.sourcegitcommit: 74c21b406302288c158e8ae26057132b12960be8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
---
# <a name="signed-packages"></a><span data-ttu-id="8dfb1-104">Pacotes assinados</span><span class="sxs-lookup"><span data-stu-id="8dfb1-104">Signed packages</span></span>

<span data-ttu-id="8dfb1-105">*NuGet 4.6.0+ e Visual Studio 2017 versão 15.6 e posterior*</span><span class="sxs-lookup"><span data-stu-id="8dfb1-105">*NuGet 4.6.0+ and Visual Studio 2017 version 15.6 and later*</span></span>

<span data-ttu-id="8dfb1-106">Pacotes do NuGet podem incluir uma assinatura digital que fornece proteção contra conteúdo violada.</span><span class="sxs-lookup"><span data-stu-id="8dfb1-106">NuGet packages can include a digital signature that provides protection against tampered content.</span></span> <span data-ttu-id="8dfb1-107">Esta assinatura é gerada de um certificado x. 509 que também adiciona provas autenticidade à origem do pacote real.</span><span class="sxs-lookup"><span data-stu-id="8dfb1-107">This signature is produced from an X.509 certificate that also adds authenticity proofs to the actual origin of the package.</span></span>

<span data-ttu-id="8dfb1-108">Pacotes assinados fornecem a validação de ponta a ponta mais forte.</span><span class="sxs-lookup"><span data-stu-id="8dfb1-108">Signed packages provide the strongest end-to-end validation.</span></span> <span data-ttu-id="8dfb1-109">Uma assinatura de autor garante que o pacote não foi modificado desde que o autor entrada o pacote, não importa qual repositório ou o que o pacote é entregue de método de transporte.</span><span class="sxs-lookup"><span data-stu-id="8dfb1-109">An author signature guarantees that the package has not been modified since the author signed the package, no matter from which repository or what transport method the package is delivered.</span></span>

<span data-ttu-id="8dfb1-110">Os consumidores que exigem um ambiente de bloqueio podem exigir pacotes assinados com um certificado de autor específico.</span><span class="sxs-lookup"><span data-stu-id="8dfb1-110">Consumers who demand a locked-down environment can require packages signed with a specific author certificate.</span></span>

<span data-ttu-id="8dfb1-111">Além disso, os pacotes assinados pelo autor fornecem um mecanismo de autenticação extra para o pipeline de publicação nuget.org porque o certificado de autenticação deve ser registrado antes do tempo.</span><span class="sxs-lookup"><span data-stu-id="8dfb1-111">Additionally, author-signed packages provide an extra authentication mechanism to the nuget.org publishing pipeline because the signing certificate must be registered ahead of time.</span></span>

<span data-ttu-id="8dfb1-112">Para obter detalhes sobre como criar um pacote assinado, consulte [assinatura pacotes](../create-packages/Sign-a-package.md) e [comando de entrada do nuget](../tools/cli-ref-sign.md).</span><span class="sxs-lookup"><span data-stu-id="8dfb1-112">For details on creating a signed package, see [Signing Packages](../create-packages/Sign-a-package.md) and the [nuget sign command](../tools/cli-ref-sign.md).</span></span>

> [!Important]
> <span data-ttu-id="8dfb1-113">Atualmente, o NuGet.org não aceita pacotes assinados.</span><span class="sxs-lookup"><span data-stu-id="8dfb1-113">nuget.org does not presently accept signed packages.</span></span> <span data-ttu-id="8dfb1-114">Você pode assinar pacotes para publicar feeds personalizados.</span><span class="sxs-lookup"><span data-stu-id="8dfb1-114">You can sign packages for publishing to custom feeds.</span></span>

## <a name="certificate-requirements"></a><span data-ttu-id="8dfb1-115">Requisitos de certificado</span><span class="sxs-lookup"><span data-stu-id="8dfb1-115">Certificate requirements</span></span>

<span data-ttu-id="8dfb1-116">Assinatura do pacote requer um código de assinatura de certificado, que é um tipo especial de certificado é válido para o `id-kp-codeSigning` finalidade [[RFC 5280 seção 4.2.1.12](https://tools.ietf.org/html/rfc5280#section-4.2.1.12)].</span><span class="sxs-lookup"><span data-stu-id="8dfb1-116">Package signing requires a code signing certificate, which is a special type of certificate that is valid for the `id-kp-codeSigning` purpose [[RFC 5280 section 4.2.1.12](https://tools.ietf.org/html/rfc5280#section-4.2.1.12)].</span></span> <span data-ttu-id="8dfb1-117">Além disso, o certificado deve ter um público comprimento da chave RSA de 2048 bits ou superior.</span><span class="sxs-lookup"><span data-stu-id="8dfb1-117">Additionally, the certificate must have an RSA public key length of 2048 bits or higher.</span></span>

## <a name="get-a-code-signing-certificate"></a><span data-ttu-id="8dfb1-118">Obter um certificado de assinatura de código</span><span class="sxs-lookup"><span data-stu-id="8dfb1-118">Get a code signing certificate</span></span>

<span data-ttu-id="8dfb1-119">Certificados válidos podem ser obtidos em autoridades de certificação pública como:</span><span class="sxs-lookup"><span data-stu-id="8dfb1-119">Valid certificates may be obtained from public certificate authorities like:</span></span>

- [<span data-ttu-id="8dfb1-120">Symantec</span><span class="sxs-lookup"><span data-stu-id="8dfb1-120">Symantec</span></span>](https://trustcenter.websecurity.symantec.com/process/trust/productOptions?productType=SoftwareValidationClass3)
- [<span data-ttu-id="8dfb1-121">DigiCert</span><span class="sxs-lookup"><span data-stu-id="8dfb1-121">DigiCert</span></span>](https://www.digicert.com/code-signing/)
- [<span data-ttu-id="8dfb1-122">Go Daddy</span><span class="sxs-lookup"><span data-stu-id="8dfb1-122">Go Daddy</span></span>](https://www.godaddy.com/web-security/code-signing-certificate)
- [<span data-ttu-id="8dfb1-123">Entrada global</span><span class="sxs-lookup"><span data-stu-id="8dfb1-123">Global Sign</span></span>](https://www.globalsign.com/en/code-signing-certificate/)
- [<span data-ttu-id="8dfb1-124">Comodo</span><span class="sxs-lookup"><span data-stu-id="8dfb1-124">Comodo</span></span>](https://www.comodo.com/e-commerce/code-signing/code-signing-certificate.php)
- [<span data-ttu-id="8dfb1-125">Certum</span><span class="sxs-lookup"><span data-stu-id="8dfb1-125">Certum</span></span>](https://www.certum.eu/certum/cert,offer_en_open_source_cs.xml) 

<span data-ttu-id="8dfb1-126">A lista completa de autoridades de certificação confiada pelo Windows pode ser obtida [ http://aka.ms/trustcertpartners ](http://aka.ms/trustcertpartners).</span><span class="sxs-lookup"><span data-stu-id="8dfb1-126">The complete list of certification authorities trusted by Windows can be obtained from [http://aka.ms/trustcertpartners](http://aka.ms/trustcertpartners).</span></span>

## <a name="create-a-test-certificate"></a><span data-ttu-id="8dfb1-127">Criar um certificado de teste</span><span class="sxs-lookup"><span data-stu-id="8dfb1-127">Create a test certificate</span></span>

<span data-ttu-id="8dfb1-128">Você pode usar certificados emitidos por conta própria para fins de teste.</span><span class="sxs-lookup"><span data-stu-id="8dfb1-128">You can use self-issued certificates for testing purposes.</span></span> <span data-ttu-id="8dfb1-129">Para criar um certificado emitido por conta própria, use o [New-SelfSignedCertificate](https://docs.microsoft.com/en-us/powershell/module/pkiclient/new-selfsignedcertificate) comando do PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8dfb1-129">To create a self-issued certificate, use the [New-SelfSignedCertificate](https://docs.microsoft.com/en-us/powershell/module/pkiclient/new-selfsignedcertificate) PowerShell command.</span></span>

```ps
New-SelfSignedCertificate -Subject "CN=NuGet Test Developer, OU=Use for testing purposes ONLY" `
                          -FriendlyName "NuGetTestDeveloper" `
                          -Type CodeSigning `
                          -KeyUsage DigitalSignature `
                          -KeyLength 2048 `
                          -KeyAlgorithm RSA `
                          -HashAlgorithm SHA256 `
                          -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
                          -CertStoreLocation "Cert:\CurrentUser\My" 
```

<span data-ttu-id="8dfb1-130">Este comando cria um certificado de teste disponível no repositório de certificados pessoais do usuário atual.</span><span class="sxs-lookup"><span data-stu-id="8dfb1-130">This command creates a testing certificate available in the current user's personal certificate store.</span></span> <span data-ttu-id="8dfb1-131">Você pode abrir o repositório de certificados executando `certmgr.msc` para ver o certificado recém-criado.</span><span class="sxs-lookup"><span data-stu-id="8dfb1-131">You can open the certificate store by running `certmgr.msc` to see the newly created certificate.</span></span>

## <a name="timestamp-requirements"></a><span data-ttu-id="8dfb1-132">Requisitos de carimbo de hora</span><span class="sxs-lookup"><span data-stu-id="8dfb1-132">Timestamp requirements</span></span>

<span data-ttu-id="8dfb1-133">Pacotes assinados devem incluir um carimbo de hora do RFC 3161 para garantir a validade da assinatura além do período de validade do certificado de assinatura de pacote.</span><span class="sxs-lookup"><span data-stu-id="8dfb1-133">Signed packages should include an RFC 3161 timestamp to ensure signature validity beyond the package signing certificate's validity period.</span></span> <span data-ttu-id="8dfb1-134">O certificado usado para assinar o carimbo de hora deve ser válido para o `id-kp-timeStamping` finalidade [[RFC 5280 seção 4.2.1.12](https://tools.ietf.org/html/rfc5280#section-4.2.1.12)].</span><span class="sxs-lookup"><span data-stu-id="8dfb1-134">The certificate used to sign the timestamp must be valid for the `id-kp-timeStamping` purpose [[RFC 5280 section 4.2.1.12](https://tools.ietf.org/html/rfc5280#section-4.2.1.12)].</span></span> <span data-ttu-id="8dfb1-135">Além disso, o certificado deve ter um público comprimento da chave RSA de 2048 bits ou superior.</span><span class="sxs-lookup"><span data-stu-id="8dfb1-135">Additionally, the certificate must have an RSA public key length of 2048 bits or higher.</span></span>

<span data-ttu-id="8dfb1-136">Detalhes técnicos adicionais podem ser encontrados no [especificações técnicas de assinatura do pacote](https://github.com/NuGet/Home/wiki/Package-Signatures-Technical-Details) (GitHub).</span><span class="sxs-lookup"><span data-stu-id="8dfb1-136">Additional technical details can be found in the [package signature technical specs](https://github.com/NuGet/Home/wiki/Package-Signatures-Technical-Details) (GitHub).</span></span>