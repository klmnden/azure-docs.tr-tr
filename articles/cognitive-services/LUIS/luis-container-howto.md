---
title: Yükleme ve docker kapsayıcılarını - Language Understanding (LUIS) çalıştırın
titleSuffix: Azure Cognitive Services
description: LUIS kapsayıcı eğitilen veya yayımlanmış uygulamanızı bir docker kapsayıcısına yükler ve kapsayıcının API uç noktalardan gelen sorgu tahminler elde etmek için erişim sağlar.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/04/2018
ms.author: diberry
ms.openlocfilehash: 085273bbf16ba7cb3557763dd392a7a7207d30db
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52965590"
---
# <a name="install-and-run-containers"></a>Kapsayıcıları yükleme ve çalıştırma
 
Language Understanding (LUIS) kapsayıcı eğitilen veya yayımlanmış Language Understanding modelinizi yüklendiğinde, ayrıca olarak bilmeniz bir [LUIS uygulaması](https://www.luis.ai), docker kapsayıcısı ve erişim için sorgu Öngörüler kapsayıcının API'SİNDEN sağlar. uç noktaları. Kapsayıcıdan sorgu günlükleri toplayıp, uygulamanın tahmin doğruluğunu artırmak için Azure dil anlama modeli Bu geri yükleyin.

Aşağıdaki video, bu kapsayıcı kullanmayı gösterir.

[![Bilişsel hizmetler için kapsayıcı Tanıtımı](./media/luis-container-how-to/luis-containers-demo-video-still.png)](https://aka.ms/luis-container-demo)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

LUIS kapsayıcıyı çalıştırmak için aşağıdakilere sahip olmanız gerekir: 

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Bu önizleme tamamlamak için Docker altyapısının yüklenmiş olması gerekir. bir [ana bilgisayar](#the-host-computer). Docker üzerinde Docker ortamını yapılandıran paketler sağlar [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Docker ve kapsayıcı temelleri hakkında bilgi için bkz: [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/).<br><br> Docker, kapsayıcılar ile bağlanma ve faturalama verileri Azure'a göndermek izin verecek şekilde yapılandırılmalıdır. <br><br> **Windows üzerinde**, Docker de Linux kapsayıcıları destekler şekilde yapılandırılmalıdır.<br><br>|
|Docker ile aşinalık | Bir temel kavramlarını Docker kayıt defterleri, havuzları, kapsayıcılar ve kapsayıcı görüntülerinin yanı sıra temel bilgi gibi olmalıdır `docker` komutları.| 
|Language Understanding (LUIS) kaynak ve ilişkili uygulama |Kapsayıcı kullanabilmeniz için şunlara sahip olmalısınız:<br><br>* A [ _Language Understanding_ Azure kaynak](luis-how-to-azure-subscription.md), ile ilişkili uç noktası anahtarı ve uç noktası URI'si (fatura uç noktası olarak kullanılır).<br>* Kapsayıcı ile ilişkili uygulama kimliğini bağlı bir giriş olarak paketlenmiş bir eğitilen veya yayımlanan uygulama<br>* Bu API yapıyor, uygulama paketi indirmek için yazma anahtarı.<br><br>Bu gereksinimler, aşağıdaki değişkenleri komut satırı bağımsız değişkenleri geçirmek için kullanılır:<br><br>**{AUTHORING_KEY}** : Bu anahtar, paketlenmiş uygulamayı bulutta LUIS hizmetten alma ve sorgu günlüklerini buluta geri yüklemek için kullanılır. Biçim `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.<br><br>**{APPLICATION_ID}** : Bu kimlik, uygulamayı seçmek üzere kullanılır. Biçim `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`.<br><br>**{ENDPOINT_KEY}** : Bu anahtar kapsayıcısı başlatmak için kullanılır. Uç nokta, iki yerde bulabilirsiniz. Azure portalında ilk olan _Language Understanding_ kaynağın anahtarları listesi. Uç nokta da anahtarları ve uç nokta LUIS Portalı'nda ayarları sayfası. Başlangıç anahtarı kullanmayın.<br><br>**{BILLING_ENDPOINT}** : Fatura uç nokta değerini Azure portalının dil anlama genel bakış sayfasında kullanılabilir. Bir örnek verilmiştir: `https://westus.api.cognitive.microsoft.com/luis/v2.0`.<br><br>[Anahtarını ve uç noktası anahtarı yazma](luis-boundaries.md#key-limits) farklı amaçları olan. Bunları birbirinin yerine kullanmayın. |

### <a name="the-host-computer"></a>Ana bilgisayar

**Konak** , docker kapsayıcısı çalıştıran bilgisayardır. Bu, bir bilgisayara şirket içinde veya Azure dahil olmak üzere hizmeti barındıran bir docker olabilir:

* [Azure Kubernetes Service](/azure/aks/)
* [Azure Container Instances](/azure/container-instances/)
* [Kubernetes](https://kubernetes.io/) kümesi dağıtıldı için [Azure Stack](/azure/azure-stack/). Daha fazla bilgi için [Azure Stack dağıtma Kubernetes](/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy).

### <a name="container-requirements-and-recommendations"></a>Kapsayıcı gereksinimleri ve önerileri

Bu kapsayıcı için ayarları en düşük ve önerilen değerlerini destekler:

|Ayar| Minimum | Önerilen |
|-----------|---------|-------------|
|Çekirdek<BR>`--cpus`|1 çekirdek<BR>en az 2.6 gigahertz (GHz) veya daha hızlı|1 çekirdek|
|Bellek<BR>`--memory`|2 GB|4 GB|
|Saniye başına işlem<BR>(TPS)|20 TPS|40 TPS|

`--cpus` Ve `--memory` ayarları bir parçası olarak kullanılan `docker run` komutu.

## <a name="get-the-container-image-with-docker-pull"></a>İle kapsayıcı görüntüsünü Al `docker pull`

Kullanım [ `docker pull` ](https://docs.docker.com/engine/reference/commandline/pull/) bir kapsayıcı görüntüsünü indirmek için komut `mcr.microsoft.com/azure-cognitive-services/luis` depo:

```Docker
docker pull mcr.microsoft.com/azure-cognitive-services/luis:latest
```

Kullanılabilir etiketler, tam bir açıklaması için gibi `latest` önceki komutta kullanılan bkz [LUIS](https://hub.docker.com/r/microsoft/azure-cognitive-services-luis/) Docker Hub üzerinde.

> [!TIP]
> Kullanabileceğiniz [docker görüntüleri](https://docs.docker.com/engine/reference/commandline/images/) indirilen kapsayıcı görüntülerinizi listelemek için komutu. Örneğin, aşağıdaki komut kimliği, havuz ve tablo olarak biçimlendirilmiş her indirilen kapsayıcı görüntüsünün etiketi listeler:
>
>  ```Docker
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>
>  IMAGE ID            REPOSITORY                                                                TAG
>  ebbee78a6baa        mcr.microsoft.com/azure-cognitive-services/luis                           latest
>  ``` 

## <a name="how-to-use-the-container"></a>Kapsayıcı kullanma

Kapsayıcı açıldığında [ana bilgisayar](#the-host-computer), kapsayıcı ile çalışmak için aşağıdaki işlemi kullanın.

![Language Understanding (LUIS) kapsayıcı kullanma işlemini](./media/luis-container-how-to/luis-flow-with-containers-diagram.jpg)

1. [Paketi dışarı aktar](#export-packaged-app-from-luis) LUIS portalını veya API'lerini LUIS kapsayıcısından için.
1. Paket dosyası gerekli Taşı **giriş** dizininde [ana bilgisayar](#the-host-computer). Yeniden adlandırmayın, alter veya LUIS paket dosyası açılamadı.
1. [Kapsayıcıyı çalıştırmak](##run-the-container-with-docker-run), gerekli olan _giriş bağlama_ ve faturalama ayarları. Daha fazla [örnekler](luis-container-configuration.md#example-docker-run-commands) , `docker run` komutu kullanılabilir. 
1. [Kapsayıcının tahmini uç nokta sorgulama](#query-the-containers-prediction-endpoint). 
1. Kapsayıcıyla işiniz bittiğinde [uç nokta günlükleri içeri aktarma](#import-the-endpoint-logs-for-active-learning) çıktısı LUIS Portalı'nda bağlama ve [Durdur](#stop-the-container) kapsayıcı.
1. Kullanım LUIS portal'ın [etkin olarak öğrenmeye](luis-how-to-review-endoint-utt.md) üzerinde **gözden geçirin, konuşma uç noktası** uygulama geliştirmek için sayfa.

Kapsayıcı içinde çalışan uygulama değiştirilemez. İçinde kapsayıcı uygulamada değişiklik sipariş, hizmet LUIS kullanarak uygulamayı değiştirmeniz gerekir [LUIS](https://www.luis.ai) portalı veya kullanım LUIS [yazma API'leri](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f). Ardından eğitmek ve/veya yayımlama, ardından yeni bir paket indirip kapsayıcı yeniden çalıştırın.

Kapsayıcı içinde LUIS uygulaması LUIS hizmetine dışarı aktarılamaz. Yalnızca sorgu günlüklerini karşıya yüklenebilir. 

## <a name="export-packaged-app-from-luis"></a>Paket uygulama LUIS ' dışarı aktarma

LUIS kapsayıcı kullanıcı konuşma tahmin sorguları yanıtlamak için eğitilen veya yayımlanmış LUIS uygulaması gerektirir. LUIS uygulaması alabilmek için ya da eğitilen veya yayımlanan paket API kullanın. 

Varsayılan konum `input` çalıştırdığınız ile ilgili alt `docker run` komutu.  

Paket dosyası bir dizine yerleştirin ve docker kapsayıcısı'nı çalıştırdığınızda bu dizin giriş bağlama başvurun. 

### <a name="package-types"></a>Paket türleri

Giriş bağlama directory içerebilir **üretim**, **hazırlama**, ve **Trained** uygulamasının sürümleri aynı anda. Tüm paketleri bağlanır. 

|Paket türü|Sorgu uç API'si|Sorgu kullanılabilirlik|Paket dosya adı biçimi|
|--|--|--|--|
|Eğitilen|GET, Post|Kapsayıcı yalnızca|`{APPLICATION_ID}_v{APPLICATION_VERSION}.gz`|
|Hazırlanıyor|GET, Post|Azure ve kapsayıcı|`{APPLICATION_ID}_STAGING.gz`|
|Üretim|GET, Post|Azure ve kapsayıcı|`{APPLICATION_ID}_PRODUCTION.gz`|

>**Önemli:** yeniden adlandırmayın, alter veya LUIS paket dosyaları açılamadı.

### <a name="packaging-prerequisites"></a>Paketleme önkoşulları

Bir LUIS uygulaması paketleme önce aşağıdakilere sahip olmanız gerekir:

|Paketleme gereksinimleri|Ayrıntılar|
|--|--|
|Azure _Language Understanding_ kaynak örneği|Desteklenen bölgeleri içerir<br><br>Batı ABD (```westus```)<br>Batı Avrupa (```westeurope```)<br>Avustralya Doğu (```australiaeast```)|
|Eğitilen veya yayımlanmış LUIS uygulaması|Olmadan [desteklenmeyen bağımlılıkları](#unsupported-dependencies). |
|Erişim [ana bilgisayar](#the-host-computer)ait dosya sistemi |Ana bilgisayar izin vermelidir bir [giriş bağlama](luis-container-configuration.md#mount-settings).|
  
### <a name="export-app-package-from-luis-portal"></a>LUIS Portalı'ndan uygulama paketini Dışarı Aktar

LUIS [portalı](https://www.luis.ai) eğitilen veya yayımlanmış uygulama paketi verme olanağı sağlar. 

### <a name="export-published-apps-package-from-luis-portal"></a>LUIS Portalı'ndan yayımlanan uygulamanın paketini Dışarı Aktar

Yayımlanan uygulamanın paket kullanılabilir **uygulamalarım** listesi sayfası. 

1. LUIS için oturum açma [portalı](https://www.luis.ai).
1. Listeden uygulama adının sol tarafındaki onay kutusunu seçin. 
1. Seçin **dışarı** bağlamsal listesinin üstündeki araç çubuğundan öğesi.
1. Seçin **dışarı aktarmak için kapsayıcı (GZIP)**.
1. Ortamını seçin **üretim yuvasına** veya **hazırlama yuvası**.
1. Paket tarayıcıdan yüklenir.

![Yayımlanan paket kapsayıcısı için uygulama sayfanın verme Menüsü'nden dışarı aktarma](./media/luis-container-how-to/export-published-package-for-container.png)

### <a name="export-trained-apps-package-from-luis-portal"></a>LUIS Portalı'ndan eğitilen uygulamanın paketini Dışarı Aktar

Eğitilen uygulamanın paket kullanılabilir **sürümleri** listesi sayfası. 

1. LUIS için oturum açma [portalı](https://www.luis.ai).
1. Uygulamayı listeden seçin. 
1. Seçin **Yönet** uygulamanın Gezinti çubuğundaki.
1. Seçin **sürümleri** sol gezinti çubuğundaki.
1. Listeden sürüm adının sol tarafındaki onay kutusunu seçin.
1. Seçin **dışarı** bağlamsal listesinin üstündeki araç çubuğundan öğesi.
1. Seçin **dışarı aktarmak için kapsayıcı (GZIP)**.
1. Paket tarayıcıdan yüklenir.

![Kapsayıcı için eğitilen paket sürümleri sayfanın verme Menüsü'nden dışarı aktarma](./media/luis-container-how-to/export-trained-package-for-container.png)


### <a name="export-published-apps-package-from-api"></a>API'SİNDEN yayımlanan uygulamanın paketini Dışarı Aktar

Aşağıdaki REST API yöntemi, seçtiğiniz bir LUIS uygulaması paketi kullanılacağını zaten [yayımlanan](luis-how-to-publish-app.md). İçin HTTP belirtimini aşağıdaki tabloyu kullanarak API çağrısı içindeki yer tutucuları kendi uygun değerleri değiştirerek.

```http
GET /luis/api/v2.0/package/{APPLICATION_ID}/slot/{APPLICATION_ENVIRONMENT}/gzip HTTP/1.1
Host: {AZURE_REGION}.api.cognitive.microsoft.com
Ocp-Apim-Subscription-Key: {AUTHORING_KEY}
```

| Yer tutucu | Değer |
|-------------|-------|
|{APPLICATION_ID} | Yayımlanan LUIS uygulaması uygulama kimliği. |
|{APPLICATION_ENVIRONMENT} | Yayımlanan LUIS uygulaması ortam. Aşağıdaki değerlerden birini kullanın:<br/>```PRODUCTION```<br/>```STAGING``` |
|{AUTHORING_KEY} | LUIS hesabı yayımlanan LUIS uygulaması için geliştirme anahtarı.<br/>Yazma anahtarınızdan alabilirsiniz **kullanıcı ayarları** LUIS portalı sayfasında. |
|{AZURE_REGION} | Uygun Azure bölgesi:<br/><br/>```westus``` -Batı ABD<br/>```westeurope``` -Batı Avrupa<br/>```australiaeast``` -Avustralya Doğu |

Yerine kendi değerlerinizi koyarak yayımlanan paketi indirmek için aşağıdaki CURL komutunu kullanın:

```bash
curl -X GET \
https://{AZURE_REGION}.api.cognitive.microsoft.com/luis/api/v2.0/package/{APPLICATION_ID}/slot/{APPLICATION_ENVIRONMENT}/gzip  \
 -H "Ocp-Apim-Subscription-Key: {AUTHORING_KEY}" \
 -o {APPLICATION_ID}_{APPLICATION_ENVIRONMENT}.gz
```

Başarılı olursa, yanıt bir LUIS paket dosyasıdır. Kapsayıcının giriş bağlaması için belirtilen depolama konumunda dosyayı kaydedin. 

### <a name="export-trained-apps-package-from-api"></a>API'SİNDEN eğitilen uygulamanın paketini Dışarı Aktar

Seçtiğiniz bir LUIS uygulama paketi için aşağıdaki REST API yöntemi kullanın zaten [eğitilen](luis-how-to-train.md). İçin HTTP belirtimini aşağıdaki tabloyu kullanarak API çağrısı içindeki yer tutucuları kendi uygun değerleri değiştirerek.

```http
GET /luis/api/v2.0/package/{APPLICATION_ID}/versions/{APPLICATION_VERSION}/gzip HTTP/1.1
Host: {AZURE_REGION}.api.cognitive.microsoft.com
Ocp-Apim-Subscription-Key: {AUTHORING_KEY}
```

| Yer tutucu | Değer |
|-------------|-------|
|{APPLICATION_ID} | Eğitilen LUIS uygulamasının uygulama kimliği. |
|{APPLICATION_VERSION} | Eğitilen LUIS uygulamasının uygulama sürümü. |
|{AUTHORING_KEY} | LUIS hesabı yayımlanan LUIS uygulaması için geliştirme anahtarı.<br/>Yazma anahtarınızdan alabilirsiniz **kullanıcı ayarları** LUIS portalı sayfasında.  |
|{AZURE_REGION} | Uygun Azure bölgesi:<br/><br/>```westus``` -Batı ABD<br/>```westeurope``` -Batı Avrupa<br/>```australiaeast``` -Avustralya Doğu |

Eğitilen paketi indirmek için aşağıdaki CURL komutunu kullanın:

```bash
curl -X GET \
https://{AZURE_REGION}.api.cognitive.microsoft.com/luis/api/v2.0/package/{APPLICATION_ID}/versions/{APPLICATION_VERSION}/gzip  \
 -H "Ocp-Apim-Subscription-Key: {AUTHORING_KEY}" \
 -o {APPLICATION_ID}_v{APPLICATION_VERSION}.gz
```

Başarılı olursa, yanıt bir LUIS paket dosyasıdır. Kapsayıcının giriş bağlaması için belirtilen depolama konumunda dosyayı kaydedin. 

## <a name="run-the-container-with-docker-run"></a>Kapsayıcı ile çalıştırma `docker run`

Kullanım [docker run](https://docs.docker.com/engine/reference/commandline/run/) kapsayıcıyı çalıştırmak için komutu. Komutu şu parametreleri kullanır:

| Yer tutucu | Değer |
|-------------|-------|
|{ENDPOINT_KEY} | Bu anahtar kapsayıcısı başlatmak için kullanılır. Başlangıç anahtarı kullanmayın. |
|{BILLING_ENDPOINT} | Fatura uç nokta değerini Azure portalının dil anlama genel bakış sayfasında kullanılabilir.|

Bu parametreleri aşağıdaki örnekte kendi değerlerinizle değiştirin `docker run` komutu.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={ENDPOINT_KEY}
```

> [!Note] 
> Yukarıdaki komut dizini aracınızdan `c:` sürücü Windows üzerinde hiçbir izni çakışmalarını önlemek için. Giriş dizini belirli bir dizini kullanmak istiyorsanız, docker vermeniz gerekebilir hizmet izni. Yukarıdaki komut docker ters eğik çizgi kullanır `\`, satır devamı karakteri olarak. Bu temel kaldırın veya değiştirin, [ana bilgisayar](#the-host-computer) işletim sisteminin gereksinimleri. Docker kapsayıcıları ile çok iyi bilmiyorsanız, bağımsız değişkenlerin sırası değiştirmeyin.


Bu komut:

* Bir kapsayıcı LUIS kapsayıcı görüntüsünü çalıştırır.
* LUIS uygulaması c:\input, kapsayıcı konağı üzerinde bulunan, giriş bağlama yükler
* İki CPU çekirdek ve 4 gigabayt (GB) bellek ayırır.
* 5000 numaralı TCP bağlantı noktasını kullanıma sunar ve sahte TTY için kapsayıcı ayırır.
* Kapsayıcı ve LUIS bağlama c:\output, kapsayıcı konağı üzerinde bulunan, çıkış günlüklerini kaydeder.
* Bunu çıktıktan sonra kapsayıcı otomatik olarak kaldırır. Kapsayıcı görüntüsü ana bilgisayarda kullanılabilir durumda kalır. 

Daha fazla [örnekler](luis-container-configuration.md#example-docker-run-commands) , `docker run` komutu kullanılabilir. 

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` kapsayıcıyı çalıştırmak için seçenekler belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](#billing).
> ApiKey değer **anahtar** anahtarları ve uç noktaları sayfasında LUIS portalda ve Azure dil anlama kaynak anahtarlar sayfasında kullanılabilir.  

## <a name="query-the-containers-prediction-endpoint"></a>Sorgu kapsayıcının tahmini uç noktası

Kapsayıcı, REST tabanlı sorgu tahmin uç nokta API'leri sağlar. Uç noktaları (hazırlık veya üretim) yayımlanan uygulamalar için bir _farklı_ daha eğitilen uygulamalar için uç nokta yolu. 

Ana bilgisayarını kullanmak https://localhost:5000, kapsayıcı API'leri için. 

|Paket türü|Yöntem|Yol|Sorgu parametreleri|
|--|--|--|--|
|Yayımlanma|[Alma](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78), [sonrası](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee79)|/ luis/v2.0/apps/{appId}?|q = {q}<br>& hazırlama<br>[& timezoneOffset]<br>[& ayrıntılı]<br>[& günlük]<br>|
|Eğitilen|GET, Post|/ luis/v2.0/apps/{appId}/versions/{versionId}?|q = {q}<br>[& timezoneOffset]<br>[& ayrıntılı]<br>[& günlük]|

Sorgu parametrelerini yapılandırma nasıl ve ne sorgu yanıtına döndürülür:

|Sorgu parametresi|Tür|Amaç|
|--|--|--|
|`q`|dize|Kullanıcının utterance.|
|`timezoneOffset`|number|TimezoneOffset sağlar [saat dilimini değiştirme](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) önceden oluşturulmuş varlık datetimeV2 tarafından kullanılır.|
|`verbose`|boole|Tüm amaçlar ve ayarlandığında puanlarını döndürür true. Yalnızca üst hedefini döndüren varsayılan false değeridir.|
|`staging`|boole|Ortam sonuçları, hazırlama alanından döndürür sorgu ayarlamak true. |
|`log`|boole|Sorgular, daha sonra için kullanılabilir günlükleri [etkin olarak öğrenmeye](luis-how-to-review-endoint-utt.md). Varsayılan değer True'dur.|

### <a name="query-published-app"></a>Sorgu yayımlanan uygulama

Yayımlanan uygulama için kapsayıcı sorgulamak için CURL komutu bir örnek verilmiştir:

```bash
curl -X GET \
"http://localhost:5000/luis/v2.0/apps/{APPLICATION_ID}?q=turn%20on%20the%20lights&staging=false&timezoneOffset=0&verbose=false&log=true" \
-H "accept: application/json"
```
Sorgular için **hazırlama** ortamı, değişiklik **hazırlama** sorgu dizesi parametresinin değerini true: 

`staging=true`

### <a name="query-trained-app"></a>Sorgu eğitilen uygulama

Eğitilen bir uygulaması için kapsayıcı sorgulamak için CURL komutu bir örnek verilmiştir: 

```bash
curl -X GET \
"http://localhost:5000/luis/v2.0/apps/{APPLICATION_ID}/versions/{APPLICATION_VERSION}?q=turn%20on%20the%20lights&timezoneOffset=0&verbose=false&log=true" \
-H "accept: application/json"
```
Sürüm adı en fazla 10 karakter sahiptir ve yalnızca bir URL'de izin verilen karakterler içeriyor. 

## <a name="import-the-endpoint-logs-for-active-learning"></a>Etkin öğrenme için uç nokta günlüklerini alma

LUIS kapsayıcı için çıktı bağlama belirtilirse, uygulama sorgu günlük dosyaları burada kapsayıcı kimliği {INSTANCE_ID}, çıkış dizinine kaydedilir Uygulama sorgu günlüğü sorgu, yanıt ve LUIS kapsayıcıya gönderilen her tahmin sorgusu zaman içerir. 

Şu konuma kapsayıcının günlük dosyaları için iç içe geçmiş dizin yapısını gösterir.
`
/output/luis/{INSTANCE_ID}/
`
 
LUIS Portalı'ndan uygulamanızı seçip **içe uç nokta günlükleri** Bu günlükleri karşıya yüklemek için. 

![Etkin öğrenme kapsayıcının günlük dosyalarını içeri aktarın](./media/luis-container-how-to/upload-endpoint-log-files.png)

Günlük karşıya yüklendikten sonra [uç nokta gözden](https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-review-endpoint-utterances) LUIS portalında konuşma.

## <a name="stop-the-container"></a>Kapsayıcı Durdur

Burada kapsayıcı çalışıyor, komut satırı ortamında kapsayıcı kapatmak için basın **Ctrl + C**.

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcı içeren bir çıktı çalıştırırsanız [bağlama](luis-container-configuration.md#mount-settings) ve günlüğe kaydetme etkin, kapsayıcı başlatma veya kapsayıcı çalıştırma sırasında gerçekleşen sorunları gidermek yararlı olan günlük dosyalarını oluşturur. 

## <a name="containers-api-documentation"></a>Kapsayıcının API belgeleri

Kapsayıcı uç noktaları için belgeleri tam bir dizi sağlar hem de bir `Try it now` özelliği. Bu özellik, web tabanlı bir HTML formuna ayarlarınızı girdikten ve herhangi bir kod yazmak zorunda kalmadan sorgu yapmanıza olanak tanır. Sorgunun döndürdüğü CURL komutu bir örnek sağlanır sonra HTTP üst bilgilerini gösterme ve gerekli biçim gövde. 

> [!TIP]
> Okuma [Openapı belirtimi](https://swagger.io/docs/specification/about/), gelen kapsayıcı tarafından desteklenen API işlemleri açıklayan `/swagger` göreli URI'si. Örneğin:
>
>  ```http
>  http://localhost:5000/swagger
>  ```

## <a name="billing"></a>Faturalandırma

Fatura bilgilerini azure'a, kullanarak LUIS kapsayıcı gönderen bir _Language Understanding_ Azure hesabınız kaynaktaki. 

Bilişsel hizmetler kapsayıcıları, kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor. Bilişsel hizmetler kapsayıcılar, Microsoft müşteri verilerini (utterance) göndermeyin. 

`docker run` Faturalama amacıyla aşağıdaki değişkenleri kullanır:

| Seçenek | Açıklama |
|--------|-------------|
| `ApiKey` | API anahtarı _Language Understanding_ kaynak faturalandırma bilgileri izlemek için kullanılır.<br/>Bu seçeneğin değeri, belirtilen sağlanan LUIS Azure kaynağı için bir API anahtarı ayarlanmalıdır `Billing`. |
| `Billing` | Uç noktası _Language Understanding_ kaynak faturalandırma bilgileri izlemek için kullanılır.<br/>Bu seçeneğin değeri, sağlanan bir LUIS Azure kaynağın URI'sini uç noktasına ayarlamanız gerekir.|
| `Eula` | Kapsayıcı lisansını kabul ettiğiniz gösterir.<br/>Bu seçenek değeri ayarlanmalıdır `accept`. |

> [!IMPORTANT]
> Seçeneklerin üçünü geçerli değerlerle belirtilmiş olmalı veya kapsayıcı başlatılamıyor.

Bu seçenekler hakkında daha fazla bilgi için bkz. [kapsayıcıları yapılandırma](luis-container-configuration.md).

## <a name="unsupported-dependencies"></a>Desteklenmeyen bağımlılıkları

Bir LUIS uygulaması, kullanabilir, **içermez** herhangi birini aşağıdaki bağımlılıkları:

Desteklenmeyen uygulama yapılandırmaları|Ayrıntılar|
|--|--|
|Desteklenmeyen kapsayıcı kültürler| Almanca (de-DE)<br>Felemenkçe (nl-NL)<br>Japonca (ja-JP)<br>|
|Desteklenmeyen etki alanları|Önceden oluşturulmuş etki alanı amaç ve varlıkları dahil olmak üzere, önceden oluşturulmuş etki alanları|
|Tüm kültürler için desteklenmeyen varlıklar|[Anahtar cümlesi](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-prebuilt-keyphrase) tüm kültürler için önceden oluşturulmuş varlık|
|İngilizce (en-US) kültürü için desteklenmeyen varlıklar|[GeographyV2](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-prebuilt-geographyv2) önceden oluşturulmuş varlıklar|
|Konuşma Hazırlama işlemi|Dış bağımlılıklar kapsayıcıda desteklenmez.|
|Yaklaşım analizi|Dış bağımlılıklar kapsayıcıda desteklenmez.|
|Bing yazım denetimi|Dış bağımlılıklar kapsayıcıda desteklenmez.|

## <a name="summary"></a>Özet

Bu makalede, kavramlar ve indirme, yükleme ve Language Understanding (LUIS) kapsayıcıları çalıştırmak için iş akışı öğrendiniz. Özet:

* Language Understanding (LUIS), bir Linux kapsayıcılarını Docker sağlayan uç noktası sorgu tahminler konuşma için sağlar.
* Kapsayıcı görüntülerini Microsoft kapsayıcı kayıt defteri (MCR) alanından indirilir.
* Docker kapsayıcı görüntüleri çalıştırın.
* REST API konak kapsayıcısının URI belirterek kapsayıcı uç noktalarını sorgulamak için kullanabilirsiniz.
* Bir kapsayıcı örneği oluşturulurken, fatura bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
> Bilişsel hizmetler kapsayıcıları, kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor. Bilişsel hizmetler kapsayıcılar, Microsoft müşteri verilerini (örneğin, görüntü veya metin analiz edilen) göndermeyin.

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [kapsayıcıları yapılandırma](luis-container-configuration.md) yapılandırma ayarları
* Başvurmak [sık sorulan sorular (SSS)](luis-resources-faq.md) LUIS işlevselliği ile ilgili sorunları gidermek için.