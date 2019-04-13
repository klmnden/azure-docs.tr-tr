---
title: Sorun giderme
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 09/11/2018
ms.topic: conceptual
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, kapsayıcılar, Helm, hizmet kafes, ağ hizmeti Yönlendirme, kubectl, k8s '
ms.openlocfilehash: 16b33203099765633d6bc5992fdc266aa1f28a26
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59548789"
---
# <a name="troubleshooting-guide"></a>Sorun giderme kılavuzu

Bu kılavuz, Azure geliştirme alanları kullanılırken olabilir sık karşılaşılan sorunlar hakkında bilgi içerir.

Azure geliştirme alanları kullanılırken bir sorun varsa, oluşturun bir [sorunu Azure geliştirme alanları GitHub deposunda](https://github.com/Azure/dev-spaces/issues).

## <a name="enabling-detailed-logging"></a>Ayrıntılı günlük kaydını etkinleştirme

Sorunları daha etkili bir şekilde gidermek için bunu ayrıntılı günlükleri gözden geçirme oluşturmak için yardımcı olabilir.

Visual Studio uzantısı için `MS_VS_AZUREDEVSPACES_TOOLS_LOGGING_ENABLED` ortam değişkeni 1. Visual Studio ortam değişkeni için etkili olması için yeniden emin olun. Etkinleştirildikten sonra ayrıntılı günlükler için yazılmıştır, `%TEMP%\Microsoft.VisualStudio.Azure.DevSpaces.Tools` dizin.

CLI, komut yürütme sırasında daha fazla bilgi kullanarak çıkarabilirsiniz `--verbose` geçin. Daha ayrıntılı günlükleri de göz atabilirsiniz `%TEMP%\Azure Dev Spaces`. Mac bilgisayarlarda, çalıştırarak TEMP dizininde bulunabilir `echo $TMPDIR` bir terminal penceresinden. Bir Linux bilgisayarında, TEMP dizini genellikle olan `/tmp`.

## <a name="debugging-services-with-multiple-instances"></a>Birden çok örnek ile hata ayıklama Hizmetleri

Şu anda, Azure geliştirme alanları çalışır, tek bir örneğini hata ayıklama sırasında en iyi veya pod. Bir ayar azds.yaml dosyayı içeren *replicaCount*, hizmetiniz için Kubernetes çalıştıran pod'ların sayısını gösterir. Belirli bir hizmet için birden çok podunuz çalıştırmak için uygulamanızı yapılandırma replicaCount değiştirirseniz, hata ayıklayıcı alfabetik olarak listelenen ilk pod'u ekler. Özgün pod geri dönüştürüldüğünde, hata ayıklayıcı için farklı bir pod muhtemelen beklenmeyen davranışlara neden olur ekler.

## <a name="error-failed-to-create-azure-dev-spaces-controller"></a>'Azure geliştirme alanları denetleyicisi oluşturmak için başarısız' hatası

Bir şeyler denetleyicisi oluşturulmasını yanlış gittiğinde şu hatayla karşılaşabilirsiniz. Geçici bir hatadır, silin ve düzeltmek için denetleyici oluşturun.

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
    
### <a name="multi-stage-dockerfiles"></a>Çok aşamalı dockerfile'ları:
Aldığınız bir *hizmeti başlatılamıyor* çok aşamalı Dockerfile'ı kullanırken bir hata oluştu. Bu durumda, aşağıdaki metni ayrıntılı çıktıyı içerir:

```cmd
$ azds up -v
Using dev space 'default' with target 'AksClusterName'
Synchronizing files...6s
Installing Helm chart...2s
Waiting for container image build...10s
Building container image...
Step 1/12 : FROM [imagename:tag] AS base
Error parsing reference: "[imagename:tag] AS base" is not a valid repository/tag: invalid reference format
Failed to build container image.
Service cannot be started.
```

AKS düğümleri Docker çok aşamalı desteklemeyen eski bir sürümünü oluşturur çünkü bu hata oluşur. Çok aşamalı yapılar önlemek için Dockerfile yeniden yazın.

### <a name="rerunning-a-service-after-controller-re-creation"></a>Hizmet Denetleyicisi yeniden oluşturma işleminden sonra yeniden çalıştırma
Aldığınız bir *hizmeti başlatılamıyor* kaldırılır ve ardından bu kümeyle ilişkili Azure geliştirme alanları denetleyicisi yeniden sonra hizmet yeniden çalışılırken hata oluştu. Bu durumda, aşağıdaki metni ayrıntılı çıktıyı içerir:

```cmd
Installing Helm chart...
Release "azds-33d46b-default-webapp1" does not exist. Installing it now.
Error: release azds-33d46b-default-webapp1 failed: services "webapp1" already exists
Helm install failed with exit code '1': Release "azds-33d46b-default-webapp1" does not exist. Installing it now.
Error: release azds-33d46b-default-webapp1 failed: services "webapp1" already exists
```

Bu hata, geliştirme alanları denetleyicisini kaldırma, denetleyici tarafından önceden yüklenmiş hizmetleri kaldırmaz oluşur. Eski Hizmetleri yerinde olduğundan denetleyicisi yeniden oluşturma ve ardından yeni denetleyicisi kullanarak hizmetlerini çalıştırma denemesi başarısız olur.

Bu sorunu ele almak için `kubectl delete` el ile eski Hizmetleri kümenizden kaldırın, ardından geliştirme yeni hizmetlerin yüklenmesini için boşluk yeniden çalıştırmak için komutu.

## <a name="dns-name-resolution-fails-for-a-public-url-associated-with-a-dev-spaces-service"></a>Geliştirme alanları hizmeti ile ilişkilendirilen genel bir URL için DNS adı çözümlemesi başarısız olur

Belirterek hizmetiniz için genel bir URL uç noktası yapılandırabilirsiniz `--public` geçin `azds prep` komutunu ya da seçerek `Publicly Accessible` Visual Studio onay kutusunu işaretleyin. Genel DNS adı, hizmetinizi geliştirme alanlarında çalıştırdığınızda otomatik olarak kaydedilir. Bu DNS adı kayıtlı değilse, gördüğünüz bir *sayfa görüntülenemiyor* veya *Site ulaşılamıyor* web tarayıcınızda genel URL'ye bağlanırken hata oluştu.

### <a name="try"></a>Deneyin:

Geliştirme alanları hizmetlerinizle ilişkili tüm URL'lerin listesini aşağıdaki komutu kullanabilirsiniz:

```cmd
azds list-uris
```

Bir URL ise *bekleyen* geliştirme alanları tamamlamak için DNS kaydı için hala bekliyor anlamına gelen durumu. Bazı durumlarda, kayıt tamamlanması birkaç dakika sürer. Geliştirme alanları localhost tünel DNS kaydında beklenirken kullanabileceğiniz her hizmet için de açılır.

Bir URL içinde kalırsa *bekleyen* durum 5 dakikadan fazla, genel bir uç nokta oluşturur dış DNS pod veya genel bir uç nokta edinme ngınx giriş denetleyicisine pod ile ilgili bir sorun olduğunu gösteriyor. Bu pod'ları silmek için aşağıdaki komutları kullanabilirsiniz. AKS, silinen pod'ları otomatik olarak yeniden oluşturur.

```cmd
kubectl delete pod -n kube-system -l app=addon-http-application-routing-external-dns
kubectl delete pod -n kube-system -l app=addon-http-application-routing-nginx-ingress
```

## <a name="error-required-tools-and-configurations-are-missing"></a>'Araçlar ve yapılandırmaları eksik gerekli' hatası

VS Code başlatırken bu hata oluşabilir: "[Azure geliştirme alanları] araçları ve derleme ve '[Proje adı]' hata ayıklama yapılandırmaları gerekli eksik."
Hata, o azds.exe yol ortam değişkeninde değil VS Code'da görüldüğü anlamına gelir.

### <a name="try"></a>Deneyin:

VS Code, PATH ortam değişkenine düzgün olarak ayarlandığı bir komut isteminden başlatın.

## <a name="error-required-tools-to-build-and-debug-projectname-are-out-of-date"></a>Hata "oluşturup 'projectname' hata ayıklama için gerekli araçları güncel."

Azure geliştirme alanları, ancak Azure geliştirme alanları CLI'ın eski bir sürümü için VS Code uzantısı'nın daha yeni bir sürümü varsa, bu hatayı Visual Studio code'da görürsünüz.

### <a name="try"></a>Deneme

İndirin ve en son Azure geliştirme alanları CLI sürümünü yükleyin:

* [Windows](https://aka.ms/get-azds-windows)
* [Mac](https://aka.ms/get-azds-mac)
* [Linux](https://aka.ms/get-azds-linux)

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

Diğer dillerde yazılmış kod ile Azure geliştirme alanları kullanmaya devam edebilirsiniz, ancak çalıştırmadan önce Dockerfile el ile oluşturmak için ihtiyacınız *yukarı azds* ilk kez.

### <a name="try"></a>Deneyin:
Uygulamanızı Azure geliştirme alanları yerel olarak desteklemediği bir dili ile yazılmıştır, kodunuzu çalıştıran bir kapsayıcı görüntüsünü oluşturmak için uygun bir Dockerfile'ı sağlamanız gerekir. Docker sağlayan bir [dockerfile'ları yazmak için en iyi uygulamalar listesini](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) ve [Dockerfile başvurusunu](https://docs.docker.com/engine/reference/builder/) yardımcı olan, gereksinimlerinize uyan bir Dockerfile yazma.

Uygun bir Dockerfile sağlandıktan çalışır durumda geçebilirsiniz *yukarı azds* Azure geliştirme alanlarında uygulamanızı çalıştırmak için.

## <a name="error-upstream-connect-error-or-disconnectreset-before-headers"></a>Hata 'yukarı bağlantı hatası veya üst bilgileri önce bağlantıyı kes/reset'
Hizmetinizi erişmeye çalışırken bu hatayı görebilirsiniz. Örneğin, ne zaman, hizmetin URL'sini bir tarayıcıda gidin. 

### <a name="reason"></a>Neden 
Kapsayıcı bağlantı noktası kullanılamaz. Bu sorun nedeniyle oluşabilir: 
* , Yine yerleşik dağıtılan ve sürecinde kapsayıcıdır. Çalıştırırsanız bu sorun ortaya çıkabilecek `azds up` veya hata ayıklayıcıyı başlatın ve ardından başarıyla dağıtıldığını önce kapsayıcı erişmeyi deneyin.
* Bağlantı noktası yapılandırması arasında tutarlı değil, _Dockerfile_, Helm grafiği ve sunucu kodlar bir bağlantı noktası açar.

### <a name="try"></a>Deneyin:
1. Kapsayıcı yerleşik/dağıtılan aşamasında olan 2-3 saniye bekleyin ve hizmete tekrar erişmeyi deneyin. 
1. Bağlantı noktası yapılandırmanızı denetleyin. Belirtilen bağlantı noktası numaralarını olmalıdır **aynı** şu varlıkları tümünde:
    * **Dockerfile:** Tarafından belirtilen `EXPOSE` yönergesi.
    * **[Helm grafiği](https://docs.helm.sh):** Tarafından belirtilen `externalPort` ve `internalPort` değerleri bir hizmet için (genellikle bulunan bir `values.yml` dosyası),
    * Örneğin, Node.js içinde uygulama kodunda açılan tüm bağlantı noktaları: `var server = app.listen(80, function () {...}`


## <a name="config-file-not-found"></a>Yapılandırma dosyası bulunamadı
Çalıştırmadan `azds up` ve aşağıdaki hatayı alıyorsunuz: `Config file not found: .../azds.yaml`

### <a name="reason"></a>Neden
Çalıştırmalısınız `azds up` çalıştırmak istediğiniz kodu kök dizininden ve Azure Dev alanları ile çalıştırmak için kod klasörü başlatmanız gerekir.

### <a name="try"></a>Deneyin:
1. Geçerli dizin hizmeti kodunuzu içeren kök klasöre değiştirin. 
1. Yoksa bir _azds.yaml_ çalıştırın kod klasörü dosyasında `azds prep` Docker, Kubernetes ve Azure Dev alanları varlıklar oluşturmak için.

## <a name="error-the-pipe-program-azds-exited-unexpectedly-with-code-126"></a>Hata: '' Azds' kanal programı 126 koduyla beklenmedik şekilde çıkıldı.'
VS Code hata ayıklayıcı başlatılıyor, bazen bu hataya neden olabilir.

### <a name="try"></a>Deneyin:
1. VS Code kapatıp yeniden açın.
2. F5'e yeniden'e basın.

## <a name="debugging-error-failed-to-find-debugger-extension-for-typecoreclr"></a>'Hata ayıklama uzantısı türü: coreclr bulmak için başarısız' hata ayıklama
VS Code hata ayıklayıcısı çalıştırma, hata raporları: `Failed to find debugger extension for type:coreclr.`

### <a name="reason"></a>Neden
C# geliştirme makinenizde yüklü VS Code uzantı yoktur. C# Uzantısı, hata ayıklama için .NET Core (CoreCLR) desteği içerir.

### <a name="try"></a>Deneyin:
Yükleme [C# için VS Code uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

## <a name="debugging-error-configured-debug-type-coreclr-is-not-supported"></a>'Yapılandırıldı hata ayıklama türü 'coreclr' desteklenmeyen' hata ayıklama
VS Code hata ayıklayıcısı çalıştırma, hata raporları: `Configured debug type 'coreclr' is not supported.`

### <a name="reason"></a>Neden
Azure geliştirme geliştirme makinenizde yüklü alanları için VS Code uzantı yoktur.

### <a name="try"></a>Deneyin:
Yükleme [Azure geliştirme alanları için VS Code uzantısı](get-started-netcore.md).

## <a name="debugging-error-invalid-cwd-value-src-the-system-cannot-find-the-file-specified-or-launch-program-srcpath-to-project-binary-does-not-exist"></a>Hata ayıklama "geçersiz 'cwd' değeri ' / src'. Sistem belirtilen dosyayı bulamıyor." veya "Başlat: '/ src / [Proje ikili dosya yolu]' programı yok"
VS Code hata ayıklayıcısı çalıştırma, hata raporları `Invalid 'cwd' value '/src'. The system cannot find the file specified.` ve/veya `launch: program '/src/[path to project executable]' does not exist`

### <a name="reason"></a>Neden
Varsayılan olarak, VS Code uzantısı kullanan `src` kapsayıcı üzerindeki proje için çalışma dizini olarak. Güncelleştirdiyseniz, `Dockerfile` farklı bir çalışma dizini belirtmek için bu hatayı görebilirsiniz.

### <a name="try"></a>Deneyin:
Güncelleştirme `launch.json` altında dosya `.vscode` proje klasörünüze alt dizinidir. Değişiklik `configurations->cwd` yönergesi ile aynı dizine işaret edecek şekilde `WORKDIR` projenizin tanımlı `Dockerfile`. Güncelleştirmeniz gerekebilir `configurations->program` de yönergesi.

## <a name="the-type-or-namespace-name-mylibrary-could-not-be-found"></a>'Kitaplığım' tür veya ad alanı bulunamadı

### <a name="reason"></a>Neden 
Varsayılan olarak proje/hizmet düzeyinde derleme bağlamıdır, bu nedenle, kullandığınız bir kitaplık projesi bulunamıyor.

### <a name="try"></a>Deneyin:
Yapmanız gerekenler:
1. Değiştirme _azds.yaml_ derleme bağlamı için çözüm düzeyi ayarlamak için dosya.
2. Değiştirme _Dockerfile_ ve _Dockerfile.develop_ projesine başvuruda bulunmak için dosyaları (_.csproj_) dosyaları doğru bir şekilde yeni göre derleme bağlamı.
3. Bir yerde bir _.dockerignore_ dosya .sln dosyasını ve gerektiği gibi değiştirin.

Bir örneğe göz bulabilirsiniz https://github.com/sgreenmsft/buildcontextsample

## <a name="microsoftdevspacesregisteraction-authorization-error"></a>'Microsoft.DevSpaces/register/action' Yetkilendirme hatası
Gereksinim duyduğunuz *sahibi* veya *katkıda bulunan* Azure aboneliğinizdeki Azure geliştirme alanları yönetmek için erişim. Geliştirme alanları yönetme çalıştığınız ve erişiminiz yok, bu hatayı görebilirsiniz *sahibi* veya *katkıda bulunan* ilişkili Azure aboneliğine erişim.
`The client '<User email/Id>' with object id '<Guid>' does not have authorization to perform action 'Microsoft.DevSpaces/register/action' over scope '/subscriptions/<Subscription Id>'.`

### <a name="reason"></a>Neden
Seçilen Azure aboneliği kaydedilmemiş `Microsoft.DevSpaces` ad alanı.

### <a name="try"></a>Deneyin:
Azure aboneliğinin sahibi veya katkıda bulunan erişimi olan el ile kaydetmek için aşağıdaki Azure CLI komutunu çalıştırabilirsiniz `Microsoft.DevSpaces` ad alanı:

```cmd
az provider register --namespace Microsoft.DevSpaces
```

## <a name="dev-spaces-times-out-at-waiting-for-container-image-build-step-with-aks-virtual-nodes"></a>Geliştirme alanları zaman aşımına *kapsayıcı görüntü derlemesi için bekleniyor...*  AKS sanal düğümü adımla

### <a name="reason"></a>Neden
Bu zaman aşımı oluşur çalıştırmak için yapılandırılmış bir hizmeti çalıştırmak için geliştirme alanları kullanma girişimi bir [AKS sanal düğümü](https://docs.microsoft.com/azure/aks/virtual-nodes-portal). Geliştirme alanları, oluşturma veya hata ayıklama Hizmetleri sanal düğümlere şu anda desteklemiyor.

Çalıştırırsanız `azds up` ile `--verbose` anahtar veya Visual Studio'da etkinleştir ayrıntılı günlük kaydı, ek ayrıntı görürsünüz:

```cmd
$ azds up --verbose

Installed chart in 2s
Waiting for container image build...
pods/mywebapi-76cf5f69bb-lgprv: Scheduled: Successfully assigned default/mywebapi-76cf5f69bb-lgprv to virtual-node-aci-linux
Streaming build container logs for service 'mywebapi' failed with: Timed out after 601.3037572 seconds trying to start build logs streaming operation. 10m 1s
Container image build failed
```

Yukarıdaki komut, hizmetin pod atandığı gösterir *sanal düğümü-aci-linux*, sanal bir düğüm.

### <a name="try"></a>Deneyin:
Güncelleştirme hizmeti kaldırmak Helm grafiği *nodeSelector* ve/veya *tolerations* sanal düğüm üzerinde çalışan izin değerleri. Bu değerler, genellikle grafik içinde tanımlanan `values.yaml` dosya.

Derleme/debug geliştirme alanları ile istediğiniz hizmeti bir VM düğüm üzerinde çalışıyorsa, sanal düğümü özelliği etkinleştirilmiş, bir AKS kümesi kullanmaya devam edebilirsiniz. Bu varsayılan yapılandırmadır.

## <a name="error-could-not-find-a-ready-tiller-pod-when-launching-dev-spaces"></a>"Hata: hazır tiller pod bulunamadı" geliştirme alanları başlatırken

### <a name="reason"></a>Neden
Helm istemci artık kümede çalışan Tiller pod konuşabilirsiniz değilse bu hata oluşur.

### <a name="try"></a>Deneyin:
Genellikle, kümenizin aracı düğümleri yeniden başlatılıyor, bu sorunu çözer.

## <a name="azure-dev-spaces-proxy-can-interfere-with-other-pods-running-in-a-dev-space"></a>Bir geliştirme alanında çalışan diğer pod'ları ile Azure geliştirme alanları proxy etkileyebilir

### <a name="reason"></a>Neden
AKS kümenizde bir ad alanı üzerinde geliştirme alanları etkinleştirdiğinizde, ek bir kapsayıcı olarak adlandırılan _mindaro proxy_ her biri, ad alanı içinde çalışan pod'ların yüklenir. Bu kapsayıcı, geliştirme alanları takım geliştirme özellikleri için tam sayı olan pod hizmetlere çağrılarını yakalar; Ancak, bu pod'ların çalışan belirli hizmetleri ile engelleyebilir. Azure önbelleği için Redis çalıştırma, yönetici/bağımlı iletişim bağlantısı hataları neden pod'ları ile etkileşime bilinir.

### <a name="try"></a>Deneyin:
Etkilenen pod'ların yapan küme içinde bir ad alanına taşıyabilirsiniz _değil_ geliştirme etkin boşluk. Uygulamanızın rest geliştirme alanları özellikli bir ad alanı içinde çalıştırmaya devam edebilirsiniz. Geliştirme alanları yüklemez _mindaro proxy_ kapsayıcı geliştirme olmayan alanları içinde etkin ad alanları.

## <a name="azure-dev-spaces-doesnt-seem-to-use-my-existing-dockerfile-to-build-a-container"></a>Azure geliştirme alanları bir kapsayıcı oluşturmak için var olan my Dockerfile kullanmak gibi görünüyor

### <a name="reason"></a>Neden
Azure geliştirme alanları, belirli bir işaret edecek şekilde yapılandırılabilir _Dockerfile_ projenizdeki. Azure geliştirme alanları kullanmıyorsa görünürse _Dockerfile_ kapsayıcılarınızı derleme beklediğiniz, açıkça Azure geliştirme alanları kullanmak için hangi Dockerfile bildirmeniz gerekebilir. 

### <a name="try"></a>Deneyin:
Açık _azds.yaml_ , Azure geliştirme alanları projenizde oluşturulan dosya. Kullanma *yapılandırmaları -> Geliştirme derleme -> dockerfile ->* kullanmak için bir Dockerfile işaret edecek şekilde yönergesi:

```
...
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
```

## <a name="error-internal-watch-failed-watch-enospc-when-attaching-debugging-to-a-nodejs-application"></a>Hata "İç izleme başarısız oldu: ENOSPC izleme" eklerken bir Node.js uygulaması için hata ayıklama

### <a name="reason"></a>Neden

Hata ayıklayıcısı ile iliştirme çalıştığınız Node.js uygulaması ile pod çalıştıran düğümü aştı *fs.inotify.max_user_watches* değeri. Bazı durumlarda, [varsayılan değerini *fs.inotify.max_user_watches* bir Haya ayıklayıcı doğrudan bir pod işlemek için çok küçük olabilir](https://github.com/Azure/AKS/issues/772).

### <a name="try"></a>Deneme
Değerini artırmak için bu sorun için geçici bir çözüm olan *fs.inotify.max_user_watches* kümedeki her düğümde ve değişikliklerin etkili olması bu düğümü yeniden başlatın.

## <a name="new-pods-are-not-starting"></a>Yeni pod'ların başlamıyor

### <a name="reason"></a>Neden

Kubernetes Başlatıcısı için RBAC izni değişiklikleri nedeniyle, yeni pod'ların PodSpec uygulanamıyor *küme yönetim* küme rolü. Yeni pod da geçersiz bir PodSpec olabilir, örneğin pod ile ilişkili hizmet hesabı artık yok. İçinde bulunduğunuz pod'ların görmek için bir *bekleyen* kullanım Başlatıcı sorunu nedeniyle durum `kubectl get pods` komutu:

```bash
kubectl get pods --all-namespaces --include-uninitialized
```

Bu sorunu pod'ların etkileyebilir *tüm ad alanlarını* burada Azure geliştirme alanları etkin değil ad alanları da dahil olmak üzere küme içindeki.

### <a name="try"></a>Deneme

[Geliştirme alanları CLI en son sürüme güncelleştirme](./how-to/upgrade-tools.md#update-the-dev-spaces-cli-extension-and-command-line-tools) ve ardından silme *azds InitializerConfiguration* Azure geliştirme alanları denetleyicisinden:

```bash
az aks get-credentials --resource-group <resource group name> --name <cluster name>
kubectl delete InitializerConfiguration azds
```

Kaldırılan sonra *azds InitializerConfiguration* kullanın Azure geliştirme alanları denetleyicisinden `kubectl delete` herhangi pod'ların kaldırmak için bir *bekleyen* durumu. Tüm bekleyen pod'ların kaldırıldı, podlarınız yeniden dağıtın.

Yeni pod'ların yine de sıkışmış varsa bir *bekleyen* yeniden dağıtım, kullanım sonra durum `kubectl delete` herhangi pod'ların kaldırmak için bir *bekleyen* durumu. Tüm bekleyen pod'ların kaldırıldı, denetleyici kümeden silin ve yeniden yükleyin:

```bash
azds remove -g <resource group name> -n <cluster name>
azds controller create --name <cluster name> -g <resource group name> -tn <cluster name>
```

Denetleyicinizi yeniden yüklendikten sonra pod'ların yeniden dağıtın.