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
ms.date: 06/14/2018
ms.author: juliako
ms.custom: mvc
ms.openlocfilehash: 489801852202163ef40d57da0082e39793196d85
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264103"
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

v3, Azure Resource Manager'da yerleşik olan yönetim ve işlem işlevselliğini kullanıma sunan, birleşik bir API yüzeyini temel alır. 

Bu sürüm aşağıdaki özellikleri sağlar:  

* Medya işleme veya analiz görevlerinin basit iş akışlarını tanımlamanıza yardımcı olan **dönüştürmeler**. Dönüştürme, video ve ses dosyalarınızı işlemeye yönelik bir tariftir. İşleri Dönüştürmeye göndererek içerik kitaplığınızdaki tüm dosyaları işlemek için art arda bunu uygulayabilirsiniz.
* Videolarınızı işleme (kodlama veya analiz etme) **İşleri**. Azure Blob depolamada bulunan dosyaların yolları, SAS URL’leri veya HTTP(s) URL’leri kullanılarak bir işte girdi içeriği belirtilebilir. 
* İş ilerlemesini veya iş durumlarını ya da Canlı Kanal başlatma/durdurma ve hata olaylarını izleyen **Bildirimler**. Bildirimler, Azure Event Grid bildirim sisteminde tümleşiktir. Azure Media Services’ta birçok kaynaktaki olaya kolayca abone olabilirsiniz. 
* Dönüştürmeler, Akış Uç Noktaları, Kanallar vb. oluşturmak ve dağıtmak için **Azure Kaynak Yönetimi** şablonları kullanılabilir.
* Kaynak düzeyinde **rol tabanlı erişim denetimi** ayarlanabilir ve böylece Dönüştürmeler, Kanallar vb. gibi belirli kaynaklara erişimi kilitlemeniz sağlanır.
* Birçok dilde **İstemci SDK’ları**: .NET, .NET core, Python, Go, Java ve Node.js.

## <a name="naming-conventions"></a>Adlandırma kuralları

Azure Media Services v3 kaynaklarının adları (Varlıklar, İşler, Dönüşümler gibi), Azure Resource Manager adlandırma kısıtlamalarına tabidir. Azure Resource Manager uyarınca kaynak adları her zaman benzersizdir. Bu nedenle kaynaklarınızda benzersiz tanıtıcı dizeleri (GUID gibi) kullanabilirsiniz. 

Media Services kaynak adları şu karakterleri içeremez: '<', '>', '%', '&', ':', '&#92;', '?', '/', '*', '+', '.', tek tırnak karakteri veya kontrol karakterleri. Diğer tüm karakterlere izin verilir. Bir kaynağın adı en fazla 260 karakter olabilir. 

Azure Resource Manager adlandırma kuralları hakkında daha fazla bilgi için bkz. [Adlandırma gereksinimleri](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/resource-api-reference.md#arguments-for-crud-on-resource) ve [Adlandırma kuralları](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).

## <a name="how-can-i-get-started-with-v3"></a>v3’ü kullanmaya nasıl başlayabilirim?

Geliştirici olarak, özel medya iş akışlarını kolayca oluşturmak, yönetmek ve korumak için REST API ile etkileşim kurmanıza olanak sağlayan Media Services [REST API](https://go.microsoft.com/fwlink/p/?linkid=873030) veya istemci kitaplıklarını kullanabilirsiniz. REST Postman örneğine [buradan](https://github.com/Azure-Samples/media-services-v3-rest-postman) ulaşabilirsiniz. [Azure Resource Manager tabanlı REST API'sini](https://github.com/Azure-Samples/media-services-v3-arm-templates) de kullanabilirsiniz.

Microsoft aşağıdaki istemci kitaplıklarını oluşturur ve destekler: 

|İstemci kitaplığı|Örnekler|
|---|---|
|[Azure CLI SDK'sı](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)|[Azure CLI örnekleri](https://github.com/Azure/azure-docs-cli-python-samples/tree/master/media-services)|
|[.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Media/1.0.0)|[.NET örnekleri](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials)|
|[.NET Core SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.Management.Media/1.0.0) (**.NET CLI** sekmesini seçin)|[.NET Core örnekleri](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials)|
|[Java SDK](https://docs.microsoft.com/java/api/overview/azure/mediaservices)||
|[Node.js SDK’sı](https://docs.microsoft.com/javascript/api/azure-arm-mediaservices/index?view=azure-node-latest)|[Node.js örnekleri](https://github.com/Azure-Samples/media-services-v3-node-tutorials)|
|[Python SDK'sı](https://pypi.org/project/azure-mgmt-media/1.0.0rc1/)||
|[Go SDK'sı](https://github.com/Azure/azure-sdk-for-go/tree/master/services/preview/mediaservices/mgmt/2018-03-30-preview/media)||

Media Services, tercih ettiğiniz dil/teknolojiye yönelik SDK’lar oluşturmak için kullanabileceğiniz [Swagger dosyaları](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media) sağlar.  

## <a name="next-steps"></a>Sonraki adımlar

Video dosyalarını kodlamaya ve akışa almaya başlamanın ne kadar kolay olduğunu görmek için [Dosyaları akışa alma](stream-files-dotnet-quickstart.md) bölümüne göz atın. 

