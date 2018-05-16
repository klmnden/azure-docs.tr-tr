---
title: Azure Media Services v3’e genel bakış | Microsoft Docs
description: Bu makalede, Media Services’ın üst düzey genel bakışı ve daha fazla ayrıntı için makalelerin bağlantıları sağlanmaktadır.
services: media-services
documentationcenter: na
author: Juliako
manager: cfowler
editor: ''
tags: ''
keywords: azure media services, akış, yayın, canlı, çevrimdışı
ms.service: media-services
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 03/27/2018
ms.author: juliako
ms.custom: mvc
ms.openlocfilehash: 33afe5fb24309547a7aacfcbe3b799ed413594ac
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="what-is-azure-media-services-v3"></a>Azure Media Services v3 nedir?

> [!div class="op_single_selector" title1="Select the version of Media Services that you are using:"]
> * [Sürüm 2 - Genel Kullanım](../previous/media-services-overview.md)
> * [Sürüm 3 - Önizleme](media-services-overview.md)

> [!NOTE]
> En son Azure Media Services sürümü Önizleme aşamasındadır ve v3 olarak adlandırılabilir.

Azure Media Services, yayın kalitesinde video akışı elde etmenizi, erişilebilirlik ve dağıtımı iyileştirmenizi, içerikleri analiz etmenizi ve daha fazlasını yapmanızı sağlayan çözümler derlemenize olanak tanıyan bulut tabanlı bir platformdur. İster uygulama geliştiricisi, çağrı merkezi, devlet dairesi, isterse bir eğlence şirketi olun, Media Services günümüzün en popüler mobil cihazlarında ve tarayıcılarında büyük kitlelere olağanüstü kalitede medya deneyimi sunan uygulamalar oluşturmanıza yardımcı olur. 

## <a name="what-can-i-do-with-media-services"></a>Media Services ile ne yapabilirim?

Media Services, bulutta çeşitli medya iş akışı derlemenize olanak sağlar. Aşağıda, Media Services ile gerçekleştirilebileceklerin bazı örnekleri verilmiştir.  

* Videoları, çeşitli tarayıcılarda ve cihazlarda oynatılabilmesi için çeşitli biçimlerde sunma. Çeşitli istemcilere (mobil cihazlar, TV, PC vb.) hem isteğe bağlı hem de canlı akış sunmak için video ve ses içeriğinin uygun şekilde kodlanması ve paketlenmesi gerekir. Bu tür içeriklerin nasıl sunulacağını ve akışa alınacağını görmek için bkz. [Hızlı Başlangıç: Dosyaları kodlama ve akışa alma](stream-files-dotnet-quickstart.md).
* Futbol, beyzbol, lise ve üniversite takım sporları vb. gibi spor etkinliklerini büyük bir çevrimiçi kitleye canlı akışa alın. 
* Belediye sarayı, şehir meclisi toplantıları ve yasama organları gibi kamu toplantılarını ve etkinliklerini yayınlayın.
* Kaydedilen videoları veya ses içeriğini analiz edin. Örneğin, daha yüksek müşteri memnuniyeti elde etmek için kuruluşlar, konuşmayı metne dönüştürebilir ve arama dizinleri ve panolar derleyebilir. Daha sonra genel şikayetlerden, şikayet kaynaklarından ve diğer ilişkili verilerden istihbarat çıkarabilir. 
* Bir müşterinin (örneğin, bir filmi stüdyosu), telif hakkıyla korunan bir çalışmaya erişimi ve kullanım yetkisini kısıtlaması gerektiğinde bir abonelik video hizmeti oluşturun ve DRM korumalı içeriği akışa alın.
* Uçak, tren ve otomobillerde kayıttan yürütülmesi için çevrimdışı içerik sunun. Bir müşterinin, ağ bağlantısının kesileceğini tahmin ettiğinde kayıttan yürütülmesi için içeriği telefonuna veya tabletine indirmesi gerekebilir.
* Daha geniş bir kitleye (örneğin, işitme engelli kişilere veya farklı bir dilde okumak isteyen kişilere) sunulacak videolara altyazılar ve açıklamalı alt yazılar ekleyin. 
* Konuşmayı metne dönüştürme, birden fazla dile çevirme vb. için Azure Media Services ve [Azure Bilişsel Hizmetler API’leri](https://docs.microsoft.com/en-us/azure/#pivot=products&panel=ai) ile eğitim amaçlı bir e-öğrenme video platformu uygulayın.
* Azure CDN’nin, anlık yüksek düzeyde yükü (örneğin, bir ürün sunumu etkinliğinin başlangıcında) daha iyi işleyebilmek için büyük ölçeklendirme elde etmesini sağlayın. 

## <a name="v3-capabilities"></a>v3 özellikleri

v3, **Azure Resource Manager**’da yerleşik olan yönetim ve işlem işlevselliğini kullanıma sunan, birleşik bir API yüzeyini temel alır. Bu sürüm aşağıdaki özellikleri sağlar:  

* Medya işleme veya analiz görevlerinin basit iş akışlarını tanımlamanıza yardımcı olan **dönüştürmeler**. Dönüştürme, video ve ses dosyalarınızı işlemeye yönelik bir tariftir. İşleri Dönüştürmeye göndererek içerik kitaplığınızdaki tüm dosyaları işlemek için art arda bunu uygulayabilirsiniz.
* Videolarınızı işleme (kodlama veya analiz etme) **İşleri**. Azure Blob depolamada bulunan dosyaların yolları, SAS URL’leri veya HTTP(s) URL’leri kullanılarak bir işte girdi içeriği belirtilebilir. 
* İş ilerlemesini veya iş durumlarını ya da Canlı Kanal başlatma/durdurma ve hata olaylarını izleyen **Bildirimler**. Bildirimler, Azure Event Grid bildirim sisteminde tümleşiktir. Azure Media Services’ta birçok kaynaktaki olaya kolayca abone olabilirsiniz. 
* Dönüştürmeler, Akış Uç Noktaları, Kanallar vb. oluşturmak ve dağıtmak için **Azure Kaynak Yönetimi** şablonları kullanılabilir.
* Kaynak düzeyinde **rol tabanlı erişim denetimi** ayarlanabilir ve böylece Dönüştürmeler, Kanallar vb. gibi belirli kaynaklara erişimi kilitlemeniz sağlanır.
* Birçok dilde **İstemci SDK’ları**: .NET, .NET core, Python, Go, Java ve Node.js.

## <a name="how-can-i-get-started-with-v3"></a>v3’ü kullanmaya nasıl başlayabilirim?

Geliştirici olarak, özel medya iş akışlarını kolayca oluşturmak, yönetmek ve korumak için REST API ile etkileşim kurmanıza olanak sağlayan Media Services [REST API](https://go.microsoft.com/fwlink/p/?linkid=873030) veya istemci kitaplıklarını kullanabilirsiniz. Microsoft aşağıdaki istemci kitaplıklarını oluşturur ve destekler: 

* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
* [.NET dilleri](https://www.nuget.org/packages/Microsoft.Azure.Management.Media/1.0.0)
* .NET Core 
* Java

  Projenize aşağıdaki bağımlılığı ekleyin:
  
  ```
  <dependency>
    <groupId>com.microsoft.azure.media-2018-03-30-preview</groupId>
    <artifactId>azure-mgmt- media</artifactId>
    <version>0.0.1-beta</version>
  </dependency> 
  ```
* Node.js 

  Aşağıdaki komutu kullanın:
  
  ```
  npm install azure-arm-mediaservices
  ```
  
* [Python](https://pypi.org/project/azure-mgmt-media/1.0.0rc1/)
* [Go](https://github.com/Azure/azure-sdk-for-go/tree/master/services/preview/mediaservices/mgmt/2018-03-30-preview/media)

Media Services, tercih ettiğiniz dil/teknolojiye yönelik SDK’lar oluşturmak için kullanabileceğiniz [Swagger dosyaları](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media) sağlar.  

## <a name="next-steps"></a>Sonraki adımlar

Video dosyalarını kodlamaya ve akışa almaya başlamanın ne kadar kolay olduğunu görmek için [Dosyaları akışa alma](stream-files-dotnet-quickstart.md) bölümüne göz atın. 

