---
title: Sorun giderme | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: article
description: Kapsayıcılar ve Azure üzerinde mikro ile hızlı Kubernetes geliştirme
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcıları
manager: douge
ms.openlocfilehash: 371bb9195266f3511d115de2532e6b64f49ef26f
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="troubleshooting-guide"></a>Sorun giderme kılavuzu

Bu kılavuz Azure Dev alanları kullanılırken olabilir ortak sorunlar hakkında bilgi içerir.

## <a name="error-upstream-connect-error-or-disconnectreset-before-headers"></a>Hata 'upstream bağlantı hatası veya kesin/reset önce üstbilgileri'
Hizmetinize erişmek çalışırken, bu hatayı görebilirsiniz. Örneğin, ne zaman, bir tarayıcıda hizmetin URL'sine gidin. 

### <a name="reason"></a>Neden 
Kapsayıcı bağlantı noktası kullanılamaz. Bunun nedeni olabilir: 
* , Yine yerleşik dağıtılan ve sürecinde kapsayıcıdır. Çalıştırıyorsanız bu durum, `azds up` veya hata ayıklayıcısını başlatın ve sonra başarıyla dağıtıldığını önce kapsayıcı erişmeyi deneyin.
* Bağlantı noktası yapılandırmasını, Dockerfile, Helm grafik ve bağlantı noktası açar. herhangi bir sunucu kodu arasında tutarlı değil.

### <a name="try"></a>Deneyin:
1. Kapsayıcı yerleşik/dağıtılan sürecinde ise, 2-3 saniye bekleyin ve hizmet erişimi yeniden deneyin. 
1. Bağlantı noktası yapılandırmanızı denetleyin. Belirtilen bağlantı noktası numaralarını olmalıdır **aynı** aşağıdaki tüm varlıklar içinde:
    * **Dockerfile:** tarafından belirtilen `EXPOSE` yönergesi.
    * **[Helm grafik](https://docs.helm.sh):** tarafından belirtilen `externalPort` ve `internalPort` bir hizmet için değerleri (genellikle bulunan bir `values.yml` dosyası),
    * Örneğin Node.js içinde uygulama kodundaki açılmakta tüm bağlantı noktaları: `var server = app.listen(80, function () {...}`


## <a name="config-file-not-found"></a>Yapılandırma dosyası bulunamadı
Çalıştırmadan `azds up` ve aşağıdaki hatayı alıyorsunuz: `Config file not found: .../azds.yaml`

### <a name="reason"></a>Neden
Çalıştırmalısınız `azds up` çalıştırmak istediğiniz kodu kök dizininden ve Azure Dev alanları ile çalıştırmak için kod klasörü başlatması gerekir.

### <a name="try"></a>Deneyin:
1. Geçerli dizin hizmeti kodunuzu içeren kök klasör olarak değiştirin. 
1. Kod klasöründe azds.yaml dosya yoksa çalıştırmak `azds prep` Docker, Kubernetes ve Azure Dev alanları varlıklar oluşturmak için.

## <a name="error-the-pipe-program-azds-exited-unexpectedly-with-code-126"></a>Hata: 'kanal programı '126 koduyla beklenmedik biçimde çıktı azds'.'
VS Code hata ayıklayıcıyı başlatma bazen bu hatasına neden olabilir. Bu bilinen bir sorundur.

### <a name="try"></a>Deneyin:
1. VS Code kapatıp yeniden açın.
2. F5 yeniden ulaştı.


## <a name="debugging-error-configured-debug-type-coreclr-is-not-supported"></a>'Yapılandırıldı hata ayıklama türü 'coreclr' desteklenmiyor' hata ayıklama
Hata raporlarını VS Code hata ayıklayıcı çalıştırma: `Configured debug type 'coreclr' is not supported.`

### <a name="reason"></a>Neden
Azure Dev geliştirme makinenizde yüklü alanları VS Code uzantısına sahip değil.

### <a name="try"></a>Deneyin:
Yükleme [VS Code uzantısı Azure Dev alanları](get-started-netcore.md).

## <a name="the-type-or-namespace-name-mylibrary-could-not-be-found"></a>'Kitaplığım' türü veya ad alanı adı bulunamadı

### <a name="reason"></a>Neden 
Varsayılan proje/hizmet düzeyinde yapı bağlamdır, bu nedenle, kullanmakta olduğunuz bir kitaplık projesine bulunamadığından olmaz.

### <a name="try"></a>Deneyin:
Yapılması gerekenler:
1. Yapı bağlamı için çözüm düzeyini ayarlamak için azds.yaml dosyasını değiştirin.
2. Csproj dosyaları yeni derleme bağlam göre doğru şekilde başvurmak için Dockerfile ve Dockerfile.develop dosyaları değiştirin.
3. .Dockerignore dosya .sln dosyasını yerleştirin ve gerektiği gibi değiştirin.

Bir örneğe bulabilirsiniz https://github.com/sgreenmsft/buildcontextsample

## <a name="microsoftconnectedenvironmentregisteraction-authorization-error"></a>'Microsoft.ConnectedEnvironment/register/action' Yetkilendirme hatası
Bir Azure Dev alanı yönetme ve bir Azure aboneliğinin sahibi veya katkıda erişim sahip olduğunuz değil çalıştığınız aşağıdaki hatayı görebilirsiniz.
`The client '<User email/Id>' with object id '<Guid>' does not have authorization to perform action 'Microsoft.ConnectedEnvironment/register/action' over scope '/subscriptions/<Subscription Id>'.`

### <a name="reason"></a>Neden
Seçili Azure aboneliği Microsoft.ConnectedEnvironment ad alanı kayıtlı değil.

### <a name="try"></a>Deneyin:
Azure aboneliğinin sahibi veya katkıda erişim biriyle el ile Microsoft.ConnectedEnvironment ad alanı kaydetmek için aşağıdaki Azure CLI komutu çalıştırabilirsiniz:

```cmd
az provider register --namespace Microsoft.ConnectedEnvironment
```

## <a name="azure-dev-spaces-doesnt-seem-to-use-my-existing-dockerfile-to-build-a-container"></a>Azure Dev alanları bir kapsayıcı oluşturmak için varolan my Dockerfile kullanma gibi görünüyor 

### <a name="reason"></a>Neden
Azure Dev alanları projenizdeki belirli bir Dockerfile işaret edecek şekilde yapılandırılabilir. Azure Dev alanları kapsayıcılarınızı yapı beklediğiniz Dockerfile kullanmıyor görünürse, açıkça Azure Dev olduğu alanları bildirmeniz gerekebilir. 

### <a name="try"></a>Deneyin:
Açık `azds.yaml` Azure Dev boşluklarla projenizde oluşturulan dosya. Kullanmak `configurations->develop->build->dockerfile` yönergesi Dockerfile işaret etmek için kullanmak istediğiniz:

```
...
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
```