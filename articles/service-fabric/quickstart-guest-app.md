---
title: Mevcut uygulamayı Azure Service Fabric kümesine hızla dağıtma
description: Mevcut Node.js uygulamasını Visual Studio ile barındırmak için Azure Service Fabric kümesi kullanın.
services: service-fabric
documentationcenter: nodejs
author: msfussell
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/06/2017
ms.author: mfussell
ms.openlocfilehash: 90ecf8a3f6d660c665cf3cdee3e1158bebee9d12
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58499735"
---
# <a name="host-a-nodejs-application-on-azure-service-fabric"></a>Node.js uygulamasını Azure Service Fabric'te barındırma

Bu hızlı başlangıç, mevcut uygulamayı (bu örnekte Node.js) Azure üzerinde çalışan bir Service Fabric kümesine dağıtmanıza yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce [geliştirme ortamınızı ayarladığınızdan](service-fabric-get-started.md) emin olun. Bu, Service Fabric SDK'sını ve Visual Studio 2017 veya 2015'i yüklemeyi de içerir.

Ayrıca dağıtım için bir Node.js uygulamanız da olmalıdır. Bu hızlı başlangıçta, [buradan][download-sample] indirilebilen basit bir Node.js web sitesi kullanılmıştır. Sonraki adımda projeyi oluşturduktan sonra, bu dosyayı `<path-to-project>\ApplicationPackageRoot\<package-name>\Code\` klasörünüze ayıklayın.

Azure aboneliğiniz yoksa [ücretsiz bir hesap][create-account] oluşturun.

## <a name="create-the-service"></a>Hizmeti oluşturma

Visual Studio'yu **yönetici** olarak başlatın.

`CTRL`+`SHIFT`+`N` ile bir proje oluşturun

**Yeni Proje** iletişim kutusunda **Bulut > Service Fabric Uygulaması**'nı seçin.

Uygulamaya **MyGuestApp** adını verin ve **Tamam**'a basın.

>[!IMPORTANT]
>Node.js, Windows'un yollara getirdiği 260 karakter sınırını kolayca aşabilir. Projenin kendisi için kısa bir yol (örneğin, **c:\code\svc1**) kullanın. İsteğe bağlı olarak **[bu yönergeleri](https://stackoverflow.com/a/41687101/1664231)** izleyerek Windows 10'da uzun dosya yollarını etkinleştirebilirsiniz.
   
![Visual Studio'da yeni proje iletişim kutusu][new-project]

Sonraki iletişim kutusunda her türde Service Fabric hizmeti oluşturabilirsiniz. Bu hızlı başlangıç için **Konuk Yürütülebilir Dosyası**'nı seçin.

Hizmeti **MyGuestService** olarak adlandırın ve sağdaki seçenekleri aşağıdaki değerlere ayarlayın:

| Ayar                   | Değer |
| ------------------------- | ------ |
| Kod Paketi Klasörü       | _&lt;Node.js uygulamanızla gelen klasör&gt;_ |
| Kod Paketi Davranışı     | Klasör içeriğini projeye kopyala |
| Program                   | node.exe |
| Bağımsız Değişkenler                 | server.js |
| Çalışma Klasörü            | CodePackage |

**Tamam**'a basın.

![Visual Studio'da yeni hizmet iletişim kutusu][new-service]

Visual Studio uygulama projesini ve aktör hizmeti projesini oluşturup bu projeleri Çözüm Gezgini'nde görüntüler.

Uygulama projesi (**MyGuestApp**) doğrudan kod içermez. Bunun yerine, bir dizi hizmet projesine başvuru sağlar. Ayrıca, bunlardan farklı olarak üç tür içerik daha barındırır:

* **Yayımlama profilleri**  
Farklı ortamlar için araç tercihleri.

* **Betikler**  
Uygulamanızı dağıtmak/yükseltmek için PowerShell betiği.

* **Uygulama tanımı**  
*ApplicationPackageRoot* altındaki uygulama bildirimini içerir. İlişkili uygulama parametre dosyaları, uygulamayı tanımlayan ve belirli bir ortam için özel olarak yapılandırmanıza imkan tanıyan *ApplicationParameters* altında bulunur.
    
Hizmet projesinin içeriklerine genel bakış için bkz. [Reliable Services ile çalışmaya başlama](service-fabric-reliable-services-quick-start.md).

## <a name="set-up-networking"></a>Ağı ayarlama

Dağıttığımız örnek Node.js uygulamasında **80** bağlantı noktası kullanılır ve Service Fabric'e bu bağlantı noktasının ortaya çıkarılmasını bildirmemiz gerekir.

Projedeki **ServiceManifest.xml** dosyasını açın. Bildirimin en altında, girdisi zaten tanımlanmış bir `<Resources> \ <Endpoints>` vardır. Bu girdiyi değiştirerek `Port`, `Protocol` ve `Type` ekleyin. 

```xml
  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="MyGuestAppServiceTypeEndpoint" Port="80" Protocol="http" Type="Input" />
    </Endpoints>
  </Resources>
```

## <a name="deploy-to-azure"></a>Azure’a dağıtma

**F5**'e basar ve projeyi çalıştırırsanız, yerel kümeye dağıtılır. Ama biz bunun yerine Azure'a dağıtalım.

Projeye sağ tıklayın ve Azure yayımlama iletişim kutusunu açan **Yayımla...** komutunu seçin.

![Service Fabric hizmeti için Azure'a yayımlama iletişim kutusu][publish]

**PublishProfiles\Cloud.xml** hedef profilini seçin.

Daha önce yapmadıysanız, dağıtımın yapılacağı Azure hesabını seçin. Henüz hesabınız yoksa, [bir hesap için kaydolun][create-account].

**Bağlantı Uç Noktası**'nın altında, dağıtımın yapılacağı Service Fabric kümesini seçin. Kümeniz yoksa, web tarayıcısı penceresinde Azure portalını açan **&lt;Yeni Küme Oluştur...&gt;** öğesini seçin. Daha fazla bilgi için bkz. [Portalda küme oluşturma](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal). 

Service Fabric kümesini oluştururken, **Özel uç noktalar** ayarını **80** olarak belirleyin.

![Uç noktayla Service Fabric düğüm türü yapılandırması][custom-endpoint]

Yeni Service Fabric kümesi oluşturma işleminin tamamlanması biraz zaman alır. Oluşturulduktan sonra, yayımlama iletişim kutusunu geri dönün ve **&lt;Yenile&gt;**'yi seçin. Yeni küme, açılan kutuna listelenir; yeni kümeyi seçin.

**Yayımla**'ya basın ve dağıtımın bitmesini bekleyin.

Bu birkaç dakika sürebilir. Tamamlandıktan sonra, uygulamanın tümüyle kullanılabilir duruma gelmesi için birkaç dakika daha gerekebilir.

## <a name="test-the-website"></a>Web sitesini test etme

Hizmetiniz yayımlandıktan sonra, hizmeti web tarayıcısında test edin. 

İlk olarak, Azure portalını açın ve Service Fabric hizmetinizi bulun.

Hizmet adresinin genel bakış dikey penceresini denetleyin. _İstemci bağlantısı uç noktası_ özelliğindeki etki alanı adını kullanın. Örneğin, `http://mysvcfab1.westus2.cloudapp.azure.com`.

![Azure portalında Service Fabric genel bakış dikey penceresi][overview]

Bu adrese gidin; `HELLO WORLD` yanıtını göreceksiniz.

## <a name="delete-the-cluster"></a>Küme silme

Bu hızlı başlangıç için oluşturduğunuz kaynakların tümünü silmeyi unutmayın, çünkü bu kaynaklar için ücretlendirilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Konuk yürütülebilir dosyaları](service-fabric-guest-executables-introduction.md) hakkındaki diğer yazıları okuyun.

<!-- Image References -->

[new-project]: ./media/quickstart-guest-app/new-project.png
[new-service]: ./media/quickstart-guest-app/template.png
[solution-exp]: ./media/quickstart-guest-app/solution-explorer.png
[publish]: ./media/quickstart-guest-app/publish.png
[overview]: ./media/quickstart-guest-app/overview.png
[custom-endpoint]: ./media/quickstart-guest-app/custom-endpoint.png

[download-sample]: https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/service-fabric-node-website.zip
[create-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
