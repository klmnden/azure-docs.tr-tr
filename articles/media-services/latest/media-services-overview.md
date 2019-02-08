---
title: Azure Media Services v3’e genel bakış | Microsoft Docs
description: Bu makalede, Media Services’ın üst düzey genel bakışı ve daha fazla ayrıntı için makalelerin bağlantıları sağlanmaktadır.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: azure media services, akış, yayın, canlı, çevrimdışı
ms.service: media-services
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 02/07/2019
ms.author: juliako
ms.custom: mvc
ms.openlocfilehash: 1911b851e4e219ec4c6d2d4872b75e9c18706feb
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55893332"
---
# <a name="what-is-azure-media-services-v3"></a>Azure Media Services v3 nedir?

Azure Media Services, yayın kalitesinde video akışı elde etmenizi, erişilebilirlik ve dağıtımı iyileştirmenizi, içerikleri analiz etmenizi ve daha fazlasını yapmanızı sağlayan çözümler derlemenize olanak tanıyan bulut tabanlı bir platformdur. İster uygulama geliştiricisi, çağrı merkezi, devlet dairesi, isterse bir eğlence şirketi olun, Media Services günümüzün en popüler mobil cihazlarında ve tarayıcılarında büyük kitlelere olağanüstü kalitede medya deneyimi sunan uygulamalar oluşturmanıza yardımcı olur. 

## <a name="what-can-i-do-with-media-services"></a>Media Services ile ne yapabilirim?

Media Services, bulutta çeşitli medya iş akışı derlemenize olanak sağlar. Aşağıda, Media Services ile gerçekleştirilebileceklerin bazı örnekleri verilmiştir.  

* Videoları, çeşitli tarayıcılarda ve cihazlarda oynatılabilmesi için çeşitli biçimlerde sunma. Çeşitli istemcilere (mobil cihazlar, TV, PC vb.) hem isteğe bağlı hem de canlı akış sunmak için video ve ses içeriğinin uygun şekilde kodlanması ve paketlenmesi gerekir. Teslim etmek ve bu tür içerik akışı hakkında bilgi için bkz: [hızlı başlangıç: Kodlama ve akışını dosyaları](stream-files-dotnet-quickstart.md).
* Futbol, beyzbol, lise ve üniversite takım sporları vb. gibi spor etkinliklerini büyük bir çevrimiçi kitleye canlı akışa alın. 
* Belediye sarayı, şehir meclisi toplantıları ve yasama organları gibi kamu toplantılarını ve etkinliklerini yayınlayın.
* Kaydedilen videoları veya ses içeriğini analiz edin. Örneğin, daha yüksek müşteri memnuniyeti elde etmek için kuruluşlar, konuşmayı metne dönüştürebilir ve arama dizinleri ve panolar derleyebilir. Daha sonra genel şikayetlerden, şikayet kaynaklarından ve diğer ilişkili verilerden istihbarat çıkarabilir.
* Bir müşterinin (örneğin, bir filmi stüdyosu), telif hakkıyla korunan bir çalışmaya erişimi ve kullanım yetkisini kısıtlaması gerektiğinde bir abonelik video hizmeti oluşturun ve DRM korumalı içeriği akışa alın.
* Uçak, tren ve otomobillerde kayıttan yürütülmesi için çevrimdışı içerik sunun. Bir müşterinin, ağ bağlantısının kesileceğini tahmin ettiğinde kayıttan yürütülmesi için içeriği telefonuna veya tabletine indirmesi gerekebilir.
* Konuşmayı metne dönüştürme, birden fazla dile çevirme vb. için Azure Media Services ve [Azure Bilişsel Hizmetler API’leri](https://docs.microsoft.com/azure/#pivot=products&panel=ai) ile eğitim amaçlı bir e-öğrenme video platformu uygulayın. 
* Azure Media Services ile birlikte kullanmak [Azure Bilişsel hizmetler API'leri](https://docs.microsoft.com/azure/#pivot=products&panel=ai) videoları daha geniş bir kitleye (örneğin, işitme engelli kişiler veya isteyenler boyunca farklı bir okumak için uygun alt yazı ve açıklamalı alt yazı eklemek için Dil).
* Azure CDN, anlık yüksek yükleri (örneğin, bir ürün sunumu etkinliğinin başlangıcını) daha iyi işlemek için büyük ölçeklendirme elde etmek etkinleştirin. 

## <a name="v3-capabilities"></a>v3 özellikleri

v3, Azure Resource Manager'da yerleşik olan yönetim ve işlem işlevselliğini kullanıma sunan, birleşik bir API yüzeyini temel alır. 

Bu sürüm aşağıdaki özellikleri sağlar:  

* Medya işleme veya analiz görevlerinin basit iş akışlarını tanımlamanıza yardımcı olan **dönüştürmeler**. Dönüştürme, video ve ses dosyalarınızı işlemeye yönelik bir tariftir. İşleri Dönüştürmeye göndererek içerik kitaplığınızdaki tüm dosyaları işlemek için art arda bunu uygulayabilirsiniz.
* Videolarınızı işleme (kodlama veya analiz etme) **İşleri**. Azure Blob depolamada bulunan dosyaların yolları, SAS URL’leri veya HTTPS URL’leri kullanılarak bir işte girdi içeriği belirtilebilir. AMS v3 şu anda HTTPS URL'leri üzerinden yığın halinde aktarım kodlamasını desteklememektedir.
* İş ilerlemesini veya iş durumlarını ya da Canlı Kanal başlatma/durdurma ve hata olaylarını izleyen **Bildirimler**. Bildirimler, Azure Event Grid bildirim sisteminde tümleşiktir. Azure Media Services’ta birçok kaynaktaki olaya kolayca abone olabilirsiniz. 
* Dönüştürmeler, Akış Uç Noktaları, Kanallar vb. oluşturmak ve dağıtmak için **Azure Kaynak Yönetimi** şablonları kullanılabilir.
* Kaynak düzeyinde **rol tabanlı erişim denetimi** ayarlanabilir ve böylece Dönüştürmeler, Kanallar vb. gibi belirli kaynaklara erişimi kilitlemeniz sağlanır.
* Birçok dilde **İstemci SDK’ları**: .NET, .NET core, Python, Go, Java ve Node.js.

## <a name="naming-conventions"></a>Adlandırma kuralları

Azure Media Services v3 kaynaklarının adları (Varlıklar, İşler, Dönüşümler gibi), Azure Resource Manager adlandırma kısıtlamalarına tabidir. Azure Resource Manager uyarınca kaynak adları her zaman benzersizdir. Bu nedenle kaynaklarınızda benzersiz tanıtıcı dizeleri (GUID gibi) kullanabilirsiniz. 

Media Services kaynak adları şu karakterleri içeremez: '<', '>', '%', '&', ':', '&#92;', '?', '/', '*', '+', '.', tek tırnak karakteri veya kontrol karakterleri. Diğer tüm karakterlere izin verilir. Bir kaynağın adı en fazla 260 karakter olabilir. 

Azure Resource Manager adlandırma hakkında daha fazla bilgi için bkz: [Adlandırma gereksinimlerini](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/resource-api-reference.md#arguments-for-crud-on-resource) ve [adlandırma kuralları](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).

## <a name="media-services-v3-api-design-principles"></a>Media Services v3 API tasarım ilkeleri

v3 API’nin temel tasarım ilkelerinden biri API’yi daha güvenli hale getirmektir. v3 API’ler **Get** veya **List** işlemlerinde gizli diziler ve kimlik bilgileri döndürmez. Anahtarlar her zaman null, boş veya yanıttan ayıklanmış olur. Gizli dizileri ve kimlik bilgilerini almak için ayrı bir eylem yöntemi çağırmanız gerekir. Bazı API’ler gizli dizileri alır ve görüntülerken diğer API'lerin bunu yapmaması durumunda, ayrı eylemler farklı RBAC güvenlik izinleri ayarlamanızı sağlar. RBAC kullanarak erişimi yönetme hakkında daha fazla bilgi için bkz. [Erişimi yönetmek için RBAC kullanma](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest).

Bunun örnekleri: 

* StreamingLocator’un Get isteği ContentKey değerleri döndürmüyor, 
* ContentKeyPolicy’nin get isteği kısıtlama anahtarları döndürmüyor, 
* İşlerin HTTP Giriş URL’lerinde URL’nin sorgu dizesi bölümünü döndürmüyor (imzayı kaldırmak için).

Bkz: [içerik anahtarı ilkesi - .NET edinme](get-content-key-policy-dotnet-howto.md) örnek.

## <a name="how-can-i-get-started-with-v3"></a>v3’ü kullanmaya nasıl başlayabilirim?

Bir geliştirici olarak, Media Services kullanabileceğiniz [REST API](https://go.microsoft.com/fwlink/p/?linkid=873030) veya kolayca oluşturmak için REST API ile etkileşimde bulunmanızı sağlayan istemci kütüphaneleri, yönetmek ve özel medya iş akışları korumak. API temel Media Services v3 [Openapı belirtimi](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media) (eski adıyla bir swagger dosyası da bilinir).

[Azure Media Services Gezgini](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE), Media Services hakkında bilgi edinmek istiyorsanız Windows müşteriler için kullanılabilir bir araçtır. AMSE olduğu bir Winforms /C# karşıya yükleyin, indirin, kodlama, VOD akışı ve Media Services ile içerik Canlı uygulama. Media Services, herhangi bir kod yazmadan test etmek istediğiniz istemciler için AMSE aracıdır. AMSE kod, Media Services ile geliştirmek isteyen müşteriler için bir kaynak olarak sağlanır.

AMSE açık kaynak bir proje, destek, topluluk tarafından sağlanır (sorunları rapor https://github.com/Azure/Azure-Media-Services-Explorer/issues). Bu proje [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Microsoft Açık Kaynak Kullanım Kuralları) belgesinde listelenen kurallara uygundur. Daha fazla bilgi için [kullanım kuralları SSS](https://opensource.microsoft.com/codeofconduct/faq/) veya başvurun opencode@microsoft.com herhangi bir ek sorularınız ya da yorumlarınız ile.
 
Azure Media Services, aşağıdaki istemci kitaplıklardan destekler: 

|API başvuruları|SDK'lar/Araçlar|Örnekler|
|---|---|---|---|
|[REST başvurusu](https://aka.ms/ams-v3-rest-ref)|[REST SDK'sı](https://aka.ms/ams-v3-rest-sdk)|[REST Postman örnekleri](https://github.com/Azure-Samples/media-services-v3-rest-postman)<br/>[Azure Resource Manager tabanlı REST API](https://github.com/Azure-Samples/media-services-v3-arm-templates)|
|[Azure CLI başvurusu](https://aka.ms/ams-v3-cli-ref)|[Azure CLI](https://aka.ms/ams-v3-cli)|[Azure CLI örnekleri](https://github.com/Azure/azure-docs-cli-python-samples/tree/master/media-services)||
|[.NET başvurusu](https://aka.ms/ams-v3-dotnet-ref)|[.NET SDK](https://aka.ms/ams-v3-dotnet-sdk)|[.NET örnekleri](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials)||
||[.NET Core SDK'sı](https://aka.ms/ams-v3-dotnet-sdk) (**.NET CLI** sekmesini seçin)|[.NET Core örnekleri](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials)||
|[Java başvurusu](https://aka.ms/ams-v3-java-ref)|[Java SDK](https://aka.ms/ams-v3-java-sdk)||
|[Node.js başvurusu](https://aka.ms/ams-v3-nodejs-ref)|[Node.js SDK’sı](https://aka.ms/ams-v3-nodejs-sdk)|[Node.js örnekleri](https://github.com/Azure-Samples/media-services-v3-node-tutorials)||
|[Python başvurusu](https://aka.ms/ams-v3-python-ref)|[Python SDK'sı](https://aka.ms/ams-v3-python-sdk)||
|[Go başvurusu](https://aka.ms/ams-v3-go-ref)|[Go SDK'sı](https://aka.ms/ams-v3-go-sdk)||
|Ruby|[Ruby SDK](https://aka.ms/ams-v3-ruby-sdk)||

## <a name="next-steps"></a>Sonraki adımlar

Video dosyalarını kodlamaya ve akışa almaya başlamanın ne kadar kolay olduğunu görmek için [Dosyaları akışa alma](stream-files-dotnet-quickstart.md) bölümüne göz atın. 

