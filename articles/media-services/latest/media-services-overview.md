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
ms.date: 03/20/2019
ms.author: juliako
ms.custom: mvc
ms.openlocfilehash: 88113fee64251344bd84085caedc9dfccfa10933
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58351465"
---
# <a name="what-is-azure-media-services-v3"></a>Azure Media Services v3 nedir?

Azure Media Services, yayın kalitesinde video akışı elde etmenizi, erişilebilirlik ve dağıtımı iyileştirmenizi, içerikleri analiz etmenizi ve daha fazlasını yapmanızı sağlayan çözümler derlemenize olanak tanıyan bulut tabanlı bir platformdur. İster uygulama geliştiricisi, çağrı merkezi, devlet dairesi, isterse bir eğlence şirketi olun, Media Services günümüzün en popüler mobil cihazlarında ve tarayıcılarında büyük kitlelere olağanüstü kalitede medya deneyimi sunan uygulamalar oluşturmanıza yardımcı olur. 

> [!NOTE]
> Şu anda Azure portalında v3 kaynakları yönetmek için kullanamazsınız. Kullanım [REST API](https://aka.ms/ams-v3-rest-ref), [CLI](https://aka.ms/ams-v3-cli-ref), veya desteklenen biri [SDK'ları](developers-guide.md).

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
* **Bildirimleri** işin ilerleme durumunu veya durumları ya da Canlı olayları başlatma/durdurma ve hatası olaylarını izleyin. Bildirimler, Azure Event Grid bildirim sisteminde tümleşiktir. Azure Media Services’ta birçok kaynaktaki olaya kolayca abone olabilirsiniz. 
* **Azure kaynak yönetimi** şablonları dönüşümleri, akış uç noktalarını, Canlı olayları ve daha fazlasını oluşturmak ve dağıtmak için kullanılabilir.
* **Rol tabanlı erişim denetimi** dönüşümleri, Canlı olayları ve diğer gibi belirli kaynaklara erişim kilitlemenize olanak tanıyan kaynak düzeyinde ayarlanabilir.
* Birçok dilde **İstemci SDK’ları**: .NET, .NET core, Python, Go, Java ve Node.js.

## <a name="naming-conventions"></a>Adlandırma kuralları

Azure Media Services v3 kaynaklarının adları (Varlıklar, İşler, Dönüşümler gibi), Azure Resource Manager adlandırma kısıtlamalarına tabidir. Azure Resource Manager uyarınca kaynak adları her zaman benzersizdir. Bu nedenle kaynaklarınızda benzersiz tanıtıcı dizeleri (GUID gibi) kullanabilirsiniz. 

Media Services kaynak adları şu karakterleri içeremez: '<', '>', '%', '&', ':', '&#92;', '?', '/', '*', '+', '.', tek tırnak karakteri veya kontrol karakterleri. Diğer tüm karakterlere izin verilir. Bir kaynağın adı en fazla 260 karakter olabilir. 

Azure Resource Manager adlandırma hakkında daha fazla bilgi için bkz: [Adlandırma gereksinimlerini](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/resource-api-reference.md#arguments-for-crud-on-resource) ve [adlandırma kuralları](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).

## <a name="v3-api-design-principles"></a>V3 API tasarım ilkeleri

v3 API’nin temel tasarım ilkelerinden biri API’yi daha güvenli hale getirmektir. v3 API’ler **Get** veya **List** işlemlerinde gizli diziler ve kimlik bilgileri döndürmez. Anahtarlar her zaman null, boş veya yanıttan ayıklanmış olur. Gizli dizileri ve kimlik bilgilerini almak için ayrı bir eylem yöntemi çağırmanız gerekir. Bazı API’ler gizli dizileri alır ve görüntülerken diğer API'lerin bunu yapmaması durumunda, ayrı eylemler farklı RBAC güvenlik izinleri ayarlamanızı sağlar. RBAC kullanarak erişimi yönetme hakkında daha fazla bilgi için bkz. [Erişimi yönetmek için RBAC kullanma](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest).

Bunun örnekleri: 

* StreamingLocator’un Get isteği ContentKey değerleri döndürmüyor, 
* ContentKeyPolicy’nin get isteği kısıtlama anahtarları döndürmüyor, 
* İşlerin HTTP Giriş URL’lerinde URL’nin sorgu dizesi bölümünü döndürmüyor (imzayı kaldırmak için).

Bkz: [içerik anahtarı ilkesi - .NET edinme](get-content-key-policy-dotnet-howto.md) örnek.

## <a name="next-steps"></a>Sonraki adımlar

v3’ü kullanmaya nasıl başlayabilirim? 

> [!div class="nextstepaction"]
> [Temel kavramlar hakkında bilgi edinin](concepts-overview.md)<br/>
> [SDK'larını kullanarak Media Services v3 API ile geliştirme](developers-guide.md) 

