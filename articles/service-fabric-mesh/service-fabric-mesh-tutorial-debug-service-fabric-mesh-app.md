---
title: Öğretici- Azure Service Fabric Mesh web uygulamasında hata ayıklama | Microsoft Docs
description: Bu öğreticide, yerel kümenizde çalışan bir Azure Service Fabric Mesh uygulamasında hata ayıklaması yaparsınız.
services: service-fabric-mesh
documentationcenter: .net
author: TylerMSFT
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/17/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: e39e5861c0606c43a869fe02a7de2dc9b6f489ea
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125662"
---
# <a name="tutorial-debug-a-service-fabric-mesh-web-application"></a>Öğretici: Service Fabric Mesh web uygulamasında hata ayıklama

Bir serinin ikinci kısmı olan bu öğreticide, yerel geliştirme kümenizdeki bir Azure Service Fabric Mesh web uygulamasında hata ayıklama işleminin nasıl yapıldığı gösterilir.

Bu öğreticide şunları öğreneceksiniz:

> [!div class="checklist"]
> * Azure Service Fabric Mesh uygulaması derlediğinizde ne olur?
> * Hizmetten hizmete çağrıyı gözlemlemek için kesme noktası nasıl ayarlanır

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [Service Fabric Mesh uygulaması oluşturma](service-fabric-mesh-tutorial-create-dotnetcore.md)
> * Uygulamada yerel olarak hata ayıklama
> * [Uygulamayı Azure’da yayımlama](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturabilirsiniz.

* [Geliştirme ortamınızı ayarladığınızdan](service-fabric-mesh-howto-setup-developer-environment-sdk.md) ve Service Fabric çalışma zamanı, SDK, Docker ve Visual Studio 2017'yi yüklediğinizden emin olun.

## <a name="download-the-to-do-sample-application"></a>Yapılacaklar örnek uygulamasını indirme

[Bu öğretici serisinin birinci kısmında](service-fabric-mesh-tutorial-create-dotnetcore.md) yapılacaklar örnek uygulamasını oluşturmadıysanız, uygulamayı indirebilirsiniz. Komut penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın.

```
git clone https://github.com/azure-samples/service-fabric-mesh
```

Uygulama `src\todolistapp` dizininin altındadır.

## <a name="build-and-debug-on-your-local-cluster"></a>Yerel kümenizde derleme ve hata ayıklama

Docker görüntüsü, projeniz yüklenir yüklenmez yerel kümenizde otomatik olarak derlenir ve dağıtılır. Bu işlem biraz zaman alabilir. Visual Studio'da ilerleme durumunu izlemek için, **Çıkış** bölmesinde Çıkış bölmesi **Çıkışı göster:** açılan listesini **Service Fabric Araçları**'na ayarlayın.

Hizmetinizi yerel olarak derlemek ve çalıştırmak için **F5** tuşuna basın. Proje yerel olarak çalıştırılıp hataları ayıklandığında Visual Studio şunları yapar:

* Docker for Windows’un çalıştırıldığından ve kapsayıcı işletim sistemi olarak Windows'u kullanacak şekilde ayarlandığından emin olur.
* Eksik Docker temel görüntülerini indirir. Bu bölüm biraz zaman alabilir
* Kod projenizi barındırmak için kullanılan Docker görüntüsünü derler (veya yeniden derler).
* Yerel Service Fabric geliştirme kümesinde kapsayıcıyı dağıtır ve çalıştırır.
* Hizmetlerinizi çalıştırır ve ayarladığınız tüm kesme noktalarına gelir.

Yerel dağıtım bittikten ve Visual Studio uygulamanızı çalıştırdıktan sonra, bir tarayıcı penceresinde varsayılan örnek web sayfası açılır.

**Hata ayıklama ipuçları**

* **Service Fabric yerel kümesi çalışmıyor** hatasını alırsanız, Hizmet Yerel Küme Yöneticisi'nin (SLCM) çalıştığından emin olun, sonra görev çubuğunda SLCM simgesine tıklayın ve **Yerel Kümeyi Başlat**'a tıklayın. Başlatıldıktan sonra Visual Studio’ya dönün ve **F5** tuşuna basın.
* Uygulama başlatıldığında **404** hatası alırsanız, **service.yaml** içindeki ortam değişkenleriniz yanlış olabilir. `AppName`, `ApiHostPort` ve `ServiceName` değerlerinin [Ortam değişkenlerini ayarlama](#set-environment-variables) yönergelerine uygun olarak doğru ayarlandığından emin olun.
* **Service.yaml** içinde derleme hataları alırsanız, satırlarda girinti yapmak için sekme yerine boşlukların kullanıldığından emin olun.

### <a name="debug-in-visual-studio"></a>Visual Studio'da hata ayıklama

Visual Studio'da Service Fabric Mesh uygulamasında hata ayıkladığınızda yerel bir Service Fabric geliştirme kümesi kullanırsınız. Arka uç hizmetinden yapılacaklar öğelerinin nasıl alındığını görmek için OnGet() yönteminde hata ayıklaması yapın.
1. **WebFrontEnd** projesinde **Pages** > **Index.cshtml** > **Index.cshtml.cs** öğesini açın ve **Get** yönteminde bir kesme noktası ayarlayın (17. satır).
2. **ToDoService** projesinde **TodoController.cs** öğesini açın ve **OnGet** yönteminde bir kesme noktası ayarlayın (15. satır).
3. Tarayıcınıza geri dönüp sayfayı yenileyin. Web ön ucu `OnGet()` yönteminde kesme noktasına ulaşırsınız. `backendUrl` değişkenini inceleyerek **service.yaml** dosyasında tanımladığınız ortam değişkenlerinin arka uç hizmetiyle bağlantı kurmak için kullanılan URL'yle nasıl birleştirildiği görebilirsiniz.
4. `client.GetAsync(backendUrl).GetAwaiter().GetResult())` çağrısından ilerleyin (F10); denetleyicinin `Get()` kesme noktasına ulaşırsınız. Bu yöntemde, yapılacaklar öğesi listesinin bellek içi listeden nasıl alındığını görebilirsiniz.
5. Bitirdiğinizde, **Shift+F5** tuşlarına basarak Visual Studio’da projenizin hata ayıklamasını durdurun.
 
## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde şunları öğrendiniz:

> [!div class="checklist"]
> * Azure Service Fabric Mesh uygulaması derlediğinizde ne olur?
> * Hizmetten hizmete çağrıyı gözlemlemek için kesme noktası nasıl ayarlanır

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Service Fabric Mesh web uygulamasını yayımlama](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)