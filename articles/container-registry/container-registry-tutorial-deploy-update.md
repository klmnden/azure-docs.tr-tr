---
title: "Azure kapsayıcı kayıt defteri Öğreticisi - itme güncelleştirilmiş görüntünün bölgesel dağıtımları"
description: "Değiştirilmiş bir Docker görüntüyü, coğrafi olarak çoğaltılmış Azure anında kayıt defteri içeren sonra birden çok bölgede çalışan web uygulamaları için otomatik olarak dağıtılan değişiklikleri görebilirsiniz. Bölümü üç üç bölümlük olması gerekir."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 359fdcabd579d277e40f02eba2d4603ebd9f5f1f
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="push-an-updated-image-to-regional-deployments"></a>Güncelleştirilmiş görüntünün bölgesel dağıtımları için anında iletme

Bu üç üç bölümlü öğretici serisinde bir parçasıdır. İçinde [önceki öğretici](container-registry-tutorial-deploy-app.md), coğrafi çoğaltma, iki farklı bölgesel Web uygulama dağıtımları için yapılandırıldı. Bu öğreticide ilk uygulama değiştirebileceğiniz, ardından yeni bir kapsayıcı görüntüsü oluşturma ve coğrafi olarak çoğaltılmış kaydınız gönderme. Son olarak, her iki Web uygulaması durumlarda Azure kapsayıcı kayıt defteri kancalarını tarafından otomatik olarak dağıtılan değişiklik görüntüleyin.

Bu öğreticide, serideki son bölümü:

> [!div class="checklist"]
> * HTML web uygulamasını değiştirme
> * Derleme ve Docker görüntü etiketi
> * Azure kapsayıcı kayıt defterine değişiklik bildirme
> * Güncelleştirilmiş uygulama iki farklı bölgelerde görüntüleyin

İki henüz yapılandırmışsanız *kapsayıcıları için Web uygulaması* bölgesel dağıtımları döndürmek için önceki öğretici serisinde [dağıtma web uygulaması Azure kapsayıcı kayıt defterinden](container-registry-tutorial-deploy-app.md).

## <a name="modify-the-web-application"></a>Web uygulamasını değiştirme

Bu adımda, Azure kapsayıcı kayıt defterine güncelleştirilmiş kapsayıcı görüntü itme sonra yüksek oranda görünür olacak web uygulamalarının değişiklik.

Bul `AcrHelloworld/Views/Home/Index.cshtml` uygulama kaynak dosyası [Github'dan klonlanmış](container-registry-tutorial-prepare-registry.md#get-application-code) önceki öğreticideki ve sık kullandığınız metin düzenleyicisinde açın. Varolan altına aşağıdaki satırı ekleyin `<h1>` satır:

```html
<h1>MODIFIED</h1>
```

Değiştirilmiş `Index.cshtml` şuna benzemelidir:

```html
@{
    ViewData["Title"] = "Azure Container Registry :: Geo-replication";
}
<style>
    body {
        background-image: url('images/azure-regions.png');
        background-size: cover;
    }
    .footer {
        position: fixed;
        bottom: 0px;
        width: 100%;
    }
</style>

<h1 style="text-align:center;color:blue">Hello World from:  @ViewData["REGION"]</h1>
<h1>MODIFIED</h1>
<div class="footer">
    <ul>
        <li>Registry URL: @ViewData["REGISTRYURL"]</li>
        <li>Registry IP: @ViewData["REGISTRYIP"]</li>
        <li>Registry Region: @ViewData["REGION"]</li>
    </ul>
</div>
```

## <a name="rebuild-the-image"></a>Görüntüyü yeniden oluşturma

Web uygulaması güncelleştirdik, kapsayıcı görüntüsünü yeniden oluşturun. Önceki gibi etiket için oturum açma sunucusu URL'si de dahil olmak üzere tam görüntü adı kullanın:

```bash
docker build . -f ./AcrHelloworld/Dockerfile -t <acrName>.azurecr.io/acr-helloworld:v1
```

## <a name="push-image-to-azure-container-registry"></a>Azure kapsayıcı kayıt defterine görüntü gönderme

Şimdi, güncelleştirilmiş anında *acr helloworld* coğrafi olarak çoğaltılmış kaydınız kapsayıcı görüntüye. Burada, tek bir yürütme `docker push` hem de kayıt defteri çoğaltmaları güncelleştirilen görüntüyü dağıtmak için komut *Batı ABD* ve *Doğu ABD* bölgeleri.

```bash
docker push <acrName>.azurecr.io/acr-helloworld:v1
```

Çıktı aşağıdakine benzer görünmelidir:

```bash
The push refers to a repository [uniqueregistryname.azurecr.io/acr-helloworld]
5b9454e91555: Pushed
d6803756744a: Layer already exists
b7b1f3a15779: Layer already exists
a89567dff12d: Layer already exists
59c7b561ff56: Layer already exists
9a2f9413d9e4: Layer already exists
a75caa09eb1f: Layer already exists
v1: digest: sha256:4c3f2211569346fbe2d1006c18cbea2a4a9dcc1eb3a078608cef70d3a186ec7a size: 1792
```

## <a name="view-the-webhook-logs"></a>Web kancası günlükleri görüntüle

Görüntü çoğaltılırken, tetiklenen Azure kapsayıcı kayıt defteri Web kancalarını görebilirsiniz.

Kapsayıcıya dağıtıldığında, oluşturulan bölgesel Web kancalarını görmek için *kapsayıcıları için Web Apps* önceki öğreticide, Azure portalında kapsayıcı kayıt gidin ve ardından **Kancalarını**altında **Hizmetleri**.

![Azure portalında kapsayıcı kayıt defteri Web kancaları][tutorial-portal-01]

Kendi çağrılarını ve yanıtlarını geçmişini görmek için her Web kancası seçin. İçin bir satır görmelisiniz **itme** her iki Web Kancalarını günlüklerine eylem. Web kancası için günlüğü burada bulunan *Batı ABD* bölge gösterir **itme** eylemi tetikleyen tarafından `docker push` önceki adımda:

![Kapsayıcı kayıt defteri Web kancası günlük Azure portalında (Batı ABD)][tutorial-portal-02]

## <a name="view-the-updated-web-app"></a>Görünüm güncelleştirilmiş web uygulaması

Yeni bir görüntü güncelleştirilmiş kapsayıcı iki bölgesel web uygulamaları için otomatik olarak dağıtan kayıt defterine gönderilen Web uygulamalarını Web Kancalarını bildir

Uygulamanın her iki dağıtım web tarayıcınızda bölgesel her iki Web uygulaması dağıtım giderek güncelleştirildiğinden emin olun. Bir anımsatıcı her uygulama hizmeti genel bakış sekmesi sağ üst içinde dağıtılan web uygulamasının URL'sini bulabilirsiniz.

![Azure portalında uygulama hizmetine genel bakış][tutorial-portal-03]

Güncelleştirilmiş uygulamayı görmek için uygulama hizmeti genel bakış bağlantıyı seçin. İçinde çalışan uygulama örnek görünümünü işte *Batı ABD*:

![Batı ABD bölgesi çalıştıran değiştirilmiş web uygulamasının tarayıcı görünümü][deployed-app-westus-modified]

Güncelleştirilmiş kapsayıcı görüntü için de dağıtıldı doğrulayın *Doğu ABD* tarayıcınızda görüntüleyerek dağıtım.

![Doğu ABD bölgesinde çalıştıran değiştirilmiş web uygulamasının tarayıcı görünümü][deployed-app-eastus-modified]

Tek bir `docker push`, bölgesel her iki Web uygulaması dağıtım güncelleştirdik ve Azure kapsayıcı kayıt sunulan ağ Kapat depoları kapsayıcı görüntülerden.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide güncelleştirildi ve web uygulama kapsayıcı yeni bir sürümü coğrafi olarak çoğaltılmış kaydınız gönderilir. Web kancası Azure kapsayıcı kayıt defterindeki kayıt defteri çoğaltmaların yerel çekme tetiklenen güncelleştirme kapsayıcıları için Web Apps bildirim.

Bu serideki son Öğreticisi:

> [!div class="checklist"]
> * Web uygulaması HTML güncelleştirildi
> * Yerleşik ve Docker görüntü etiketli
> * Azure kapsayıcı kayıt defterine değişiklik gönderilir
> * Güncelleştirilmiş uygulama iki farklı bölgelerde görüntülenebilir

<!-- IMAGES -->
[deployed-app-eastus-modified]: ./media/container-registry-tutorial-deploy-update/deployed-app-eastus-modified.png
[deployed-app-westus-modified]: ./media/container-registry-tutorial-deploy-update/deployed-app-westus-modified.png
[local-container-01]: ./media/container-registry-tutorial-deploy-update/local-container-01.png
[tutorial-portal-01]: ./media/container-registry-tutorial-deploy-update/tutorial-portal-01.png
[tutorial-portal-02]: ./media/container-registry-tutorial-deploy-update/tutorial-portal-02.png
[tutorial-portal-03]: ./media/container-registry-tutorial-deploy-update/tutorial-portal-03.png