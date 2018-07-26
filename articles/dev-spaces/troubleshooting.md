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
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Hizmeti, kapsayıcılar
manager: douge
ms.openlocfilehash: b2ef450a429b26843cf770a6243c6f4de932de43
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247339"
---
# <a name="troubleshooting-guide"></a>Sorun giderme kılavuzu

Bu kılavuz, Azure geliştirme alanları kullanılırken olabilir sık karşılaşılan sorunlar hakkında bilgi içerir.

## <a name="error-failed-to-create-azure-dev-spaces-controller"></a>'Azure geliştirme alanları denetleyicisi oluşturmak için başarısız' hatası

Bir şeyler denetleyicisi oluşturulmasını yanlış gittiğinde şu hatayla karşılaşabilirsiniz. Geçici bir hatadır, geçemezsiniz denetleyicisine düzeltir.

### <a name="try"></a>Deneyin:

Denetleyici silmek için Azure geliştirme alanları CLI'yı kullanın. Visual Studio veya Cloud Shell içinde Bunu yapmak mümkün değildir. AZDS CLI'yı yüklemek için öncelikle Azure CLI'yı yükleyin ve ardından bu komutu çalıştırın:

```cmd
az aks use-dev-spaces -g <resource group name> -n <cluster name>
```

Ve ardından denetleyicisini silmek için şu komutu çalıştırın:

```cmd
azds remove -g <resource group name> -n <cluster name>
```

Denetleyici yeniden CLI veya Visual Studio'dan yapılabilir. İlk kez başlatılıyorsa gibi öğreticilerde yer yönergeleri izleyin.


## <a name="error-service-cannot-be-started"></a>Hata 'hizmeti başlatılamıyor.'

Başlatmak hizmet kodunuzu başarısız olduğunda bu hatayı görebilirsiniz. Genellikle kullanıcı kodunda nedenidir. Daha fazla tanı bilgilerini almak için komutlar ve ayarlar aşağıdaki değişiklikleri yapın:

### <a name="try"></a>Deneyin:

Komut satırında:

Kullanırken _azds.exe_, kullanın verbose komut satırı seçeneğini kullanıp çıkış biçimini belirtmek için output komut satırı seçeneği.
 
    ```cmd
    azds up --verbose --output json
    ```

Visual Studio'da:

1. Açık **Araçlar > Seçenekler** altında **projeler ve çözümler**, seçin ve **derleme ve çalıştırma**.
2. Ayarlarını değiştirmek için **MSBuild proje oluşturması çıkış ayrıntısı** için **ayrıntılı** veya **tanılama**.

    ![Ekran Araçlar, Seçenekler iletişim kutusu](media/common/VerbositySetting.PNG)

## <a name="error-required-tools-and-configurations-are-missing"></a>'Araçlar ve yapılandırmaları eksik gerekli' hatası

VS Code başlatırken bu hata oluşabilir: "[Azure geliştirme alanları] araçları ve derleme ve '[Proje adı]' hata ayıklama yapılandırmaları gerekli eksik."
Hata, o azds.exe yol ortam değişkeninde değil VS Code'da görüldüğü anlamına gelir.

### <a name="try"></a>Deneyin:

VS Code, PATH ortam değişkenine düzgün olarak ayarlandığı bir komut isteminden başlatın.

## <a name="error-azds-is-not-recognized-as-an-internal-or-external-command-operable-program-or-batch-file"></a>Hata 'azds' iç ya da dış komut, çalıştırılabilir program veya toplu iş dosyası tanınmıyor
 
Bu hata azds.exe yüklü değil veya doğru bir şekilde yapılandırıldığını görebilirsiniz.

### <a name="try"></a>Deneyin:

1. Azds.exe için konum %ProgramFiles%/Microsoft SDKs\Azure\Azure geliştirme alanları CLI (Önizleme) denetleyin. Yoktur, bu konuma PATH ortam değişkenine ekleyin.
2. Azds.exe yüklü değilse aşağıdaki komutu çalıştırın:

    ```cmd
    az aks use-dev-spaces -n <cluster-name> -g <resource-group>
    ```

## <a name="error-upstream-connect-error-or-disconnectreset-before-headers"></a>Hata 'yukarı bağlantı hatası veya üst bilgileri önce bağlantıyı kes/reset'
Hizmetinizi erişmeye çalışırken bu hatayı görebilirsiniz. Örneğin, ne zaman, hizmetin URL'sini bir tarayıcıda gidin. 

### <a name="reason"></a>Neden 
Kapsayıcı bağlantı noktası kullanılamaz. Bu sorun nedeniyle oluşabilir: 
* , Yine yerleşik dağıtılan ve sürecinde kapsayıcıdır. Çalıştırırsanız bu sorun ortaya çıkabilecek `azds up` veya hata ayıklayıcıyı başlatın ve ardından başarıyla dağıtıldığını önce kapsayıcı erişmeyi deneyin.
* Bağlantı noktası yapılandırması arasında tutarlı değil, _Dockerfile_, Helm grafiği ve sunucu kodlar bir bağlantı noktası açar.

### <a name="try"></a>Deneyin:
1. Kapsayıcı yerleşik/dağıtılan aşamasında olan 2-3 saniye bekleyin ve hizmete tekrar erişmeyi deneyin. 
1. Bağlantı noktası yapılandırmanızı denetleyin. Belirtilen bağlantı noktası numaralarını olmalıdır **aynı** aşağıdaki tüm varlıkları içinde:
    * **Dockerfile:** tarafından belirtilen `EXPOSE` yönergesi.
    * **[Helm grafiği](https://docs.helm.sh):** tarafından belirtilen `externalPort` ve `internalPort` değerleri bir hizmet için (genellikle bulunan bir `values.yml` dosyası),
    * Örneğin, Node.js içinde uygulama kodunda açılan tüm bağlantı noktaları: `var server = app.listen(80, function () {...}`


## <a name="config-file-not-found"></a>Yapılandırma dosyası bulunamadı
Çalıştırmadan `azds up` ve aşağıdaki hatayı alıyorsunuz: `Config file not found: .../azds.yaml`

### <a name="reason"></a>Neden
Çalıştırmalısınız `azds up` çalıştırmak istediğiniz kodu kök dizininden ve Azure Dev alanları ile çalıştırmak için kod klasörü başlatmanız gerekir.

### <a name="try"></a>Deneyin:
1. Geçerli dizin hizmeti kodunuzu içeren kök klasöre değiştirin. 
1. Yoksa bir _azds.yaml_ çalıştırın kod klasörü dosyasında `azds prep` Docker, Kubernetes ve Azure Dev alanları varlıklar oluşturmak için.

## <a name="error-the-pipe-program-azds-exited-unexpectedly-with-code-126"></a>Hata: 'kanal programına '126 koduyla beklenmedik bir şekilde çıkıldı azds'.'
VS Code hata ayıklayıcı başlatılıyor, bazen bu hataya neden olabilir. Bu bilinen bir sorundur.

### <a name="try"></a>Deneyin:
1. VS Code kapatıp yeniden açın.
2. F5'e yeniden'e basın.


## <a name="debugging-error-configured-debug-type-coreclr-is-not-supported"></a>'Yapılandırıldı hata ayıklama türü 'coreclr' desteklenmeyen' hata ayıklama
VS Code hata ayıklayıcısı çalıştırma, hata raporları: `Configured debug type 'coreclr' is not supported.`

### <a name="reason"></a>Neden
Azure geliştirme geliştirme makinenizde yüklü alanları için VS Code uzantı yoktur.

### <a name="try"></a>Deneyin:
Yükleme [Azure geliştirme alanları için VS Code uzantısı](get-started-netcore.md).

## <a name="the-type-or-namespace-name-mylibrary-could-not-be-found"></a>'Kitaplığım' tür veya ad alanı bulunamadı

### <a name="reason"></a>Neden 
Varsayılan olarak proje/hizmet düzeyinde derleme bağlamıdır, bu nedenle, kullanmakta olduğunuz bir kitaplık projesi bulunamadığından olmaz.

### <a name="try"></a>Deneyin:
Yapmanız gerekenler:
1. Değiştirme _azds.yaml_ derleme bağlamı için çözüm düzeyi ayarlamak için dosya.
2. Değiştirme _Dockerfile_ ve _Dockerfile.develop_ projesine başvuruda bulunmak için dosyaları (_.csproj_) dosyaları doğru bir şekilde yeni göre derleme bağlamı.
3. Bir yerde bir _.dockerignore_ dosya .sln dosyasını ve gerektiği gibi değiştirin.

Bir örneğe göz bulabilirsiniz https://github.com/sgreenmsft/buildcontextsample

## <a name="microsoftdevspacesregisteraction-authorization-error"></a>'Microsoft.DevSpaces/register/action' Yetkilendirme hatası
Bir Azure geliştirme alanı yönettiğiniz ve bir Azure aboneliği sahibi veya katkıda bulunan erişimi olan değil çalışıyorsanız, aşağıdaki hatayı görebilirsiniz.
`The client '<User email/Id>' with object id '<Guid>' does not have authorization to perform action 'Microsoft.DevSpaces/register/action' over scope '/subscriptions/<Subscription Id>'.`

### <a name="reason"></a>Neden
Seçilen Azure aboneliği kaydedilmemiş `Microsoft.DevSpaces` ad alanı.

### <a name="try"></a>Deneyin:
Azure aboneliğinin sahibi veya katkıda bulunan erişimi olan el ile kaydetmek için aşağıdaki Azure CLI komutunu çalıştırabilirsiniz `Microsoft.DevSpaces` ad alanı:

```cmd
az provider register --namespace Microsoft.DevSpaces
```

## <a name="azure-dev-spaces-doesnt-seem-to-use-my-existing-dockerfile-to-build-a-container"></a>Azure geliştirme alanları bir kapsayıcı oluşturmak için var olan my Dockerfile kullanmak gibi görünüyor 

### <a name="reason"></a>Neden
Azure geliştirme alanları, belirli bir işaret edecek şekilde yapılandırılabilir _Dockerfile_ projenizdeki. Azure geliştirme alanları kullanmıyorsa görünürse _Dockerfile_ kapsayıcılarınızı derleme beklediğiniz, Azure geliştirme alanları açıkça olduğu bildirmeniz gerekebilir. 

### <a name="try"></a>Deneyin:
Açık _azds.yaml_ projenizde Azure geliştirme alanları tarafından oluşturulan dosya. Kullanma `configurations->develop->build->dockerfile` kullanmak için bir Dockerfile işaret edecek şekilde yönergesi:

```
...
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
```