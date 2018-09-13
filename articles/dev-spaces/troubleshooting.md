---
title: Sorun giderme | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 09/11/2018
ms.topic: article
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Hizmeti, kapsayıcılar
manager: douge
ms.openlocfilehash: c6ca3003c1338f3e057c76d9e04d8b0cbd2210c7
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44721203"
---
# <a name="troubleshooting-guide"></a>Sorun giderme kılavuzu

Bu kılavuz, Azure geliştirme alanları kullanılırken olabilir sık karşılaşılan sorunlar hakkında bilgi içerir.

## <a name="enabling-detailed-logging"></a>Ayrıntılı günlük kaydını etkinleştirme

Sorunları daha etkili bir şekilde gidermek için ayrıntılı günlükleri gözden geçirme oluşturmak için yardımcı.

Visual Studio uzantısı için `MS_VS_AZUREDEVSPACES_TOOLS_LOGGING_ENABLED` ortam değişkeni 1. Visual Studio ortam değişkeni için etkili olması için yeniden emin olun. Etkinleştirildikten sonra ayrıntılı günlükler yazılır, `%TEMP%\Microsoft.VisualStudio.Azure.DevSpaces.Tools` dizin.

CLI, komut yürütme sırasında daha fazla bilgi kullanarak çıkarabilirsiniz `--verbose` geçin.

## <a name="debugging-services-with-multiple-instances"></a>Birden çok örnek ile hata ayıklama Hizmetleri

Şu anda, Azure geliştirme alanları yalnızca tek bir örneği üzerinde (pod) hata ayıklamayı destekler. Bir ayar, hizmetiniz için çalıştırılan örnek sayısını gösteren replicaCount azds.yaml dosya içerir. Belirli bir hizmet için birden çok örneğini çalıştırmak için uygulamanızı yapılandırma replicaCount değiştirirseniz, hata ayıklayıcı davranışını beklendiği gibi olmayabilir.

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

1. Açık **Araçlar > Seçenekler** altında **projeler ve çözümler**, seçin **derleme ve çalıştırma**.
2. Ayarlarını değiştirmek için **MSBuild proje oluşturması çıkış ayrıntısı** için **ayrıntılı** veya **tanılama**.

    ![Ekran Araçlar, Seçenekler iletişim kutusu](media/common/VerbositySetting.PNG)
    
## <a name="dns-name-resolution-fails-for-a-public-url-associated-with-a-dev-spaces-service"></a>Geliştirme alanları hizmeti ile ilişkilendirilen genel bir URL için DNS adı çözümlemesi başarısız olur

DNS ad çözümlemesi başarısız olduğunda, görebileceğiniz bir "Sayfası görüntülenemiyor" veya "Bu site erişilemiyor" hatası web tarayıcınızda genel URL'ye bağlanmaya geliştirme alanları hizmeti ile ilişkili olduğunda.

### <a name="try"></a>Deneyin:

Geliştirme alanları hizmetlerinizle ilişkili tüm URL'lerin listesini aşağıdaki komutu kullanabilirsiniz:

```cmd
azds list-uris
```

Bir URL ise *bekleyen* geliştirme alanları tamamlamak için DNS kaydı için hala bekliyor anlamına gelen durumu. Bazı durumlarda, kayıt tamamlanması birkaç dakika sürer. Geliştirme alanları localhost tünel DNS kaydında beklenirken kullanabileceğiniz her hizmet için de açılır.

Bir URL kalırsa *bekleyen* durum 5 dakikadan fazla, dış DNS pod'u genel bir uç nokta oluşturan ve/veya genel bir uç nokta edinme ngınx giriş denetleyicisine pod ile ilgili bir sorun olduğunu gösteriyor. Bu pod'ları silmek için aşağıdaki komutları kullanabilirsiniz. Bunlar otomatik olarak yeniden oluşturulur.

```cmd
kubectl delete pod -n kube-system -l app=addon-http-application-routing-external-dns
kubectl delete pod -n kube-system -l app=addon-http-application-routing-nginx-ingress
```

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

## <a name="warning-dockerfile-could-not-be-generated-due-to-unsupported-language"></a>Uyarı 'Dockerfile nedeniyle desteklenmeyen dil üretilemedi'
Azure geliştirme alanları, C# ve Node.js için yerel destek sağlar. Çalıştırdığınızda *azds hazırlığı* bu dillerden birinde yazılan kod içeren bir dizinde Azure geliştirme alanları otomatik olarak uygun bir Dockerfile sizin için oluşturur.

Diğer dillerde yazılmış kod ile Azure geliştirme alanları kullanmaya devam edebilirsiniz, ancak Dockerfile çalıştırılmadan önce kendiniz oluşturmanız gerekecektir *yukarı azds* ilk kez.

### <a name="try"></a>Deneyin:
Uygulamanızı Azure geliştirme alanları tarafından yerel olarak desteklenmeyen bir dilde yazılan kodunuzu çalıştıran bir kapsayıcı görüntüsünü oluşturmak için uygun bir Dockerfile sağlamak gerekir. Docker sağlayan bir [dockerfile'ları yazmak için en iyi uygulamalar listesini](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) yanı [Dockerfile başvurusunu](https://docs.docker.com/engine/reference/builder/) yardımcı olan, gereksinimlerinize uyan bir Dockerfile yazma.

Uygun bir Dockerfile sağlandıktan çalışır durumda geçebilirsiniz *yukarı azds* Azure geliştirme alanlarında uygulamanızı çalıştırmak için.

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
VS Code hata ayıklayıcı başlatılıyor, bazen bu hataya neden olabilir.

### <a name="try"></a>Deneyin:
1. VS Code kapatıp yeniden açın.
2. F5'e yeniden'e basın.

## <a name="debugging-error-failed-to-find-debugger-extension-for-typecoreclr"></a>'Hata ayıklama uzantısı türü: coreclr bulmak için başarısız' hata ayıklama
VS Code hata ayıklayıcısı çalıştırma, hata raporları: `Failed to find debugger extension for type:coreclr.`

### <a name="reason"></a>Neden
C# geliştirme makinenizde yüklü VS Code uzantı yoktur. C# uzantısı hata ayıklama için.Net Core desteği içerir (CoreCLR).

### <a name="try"></a>Deneyin:
Yükleme [C# için VS Code uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

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
