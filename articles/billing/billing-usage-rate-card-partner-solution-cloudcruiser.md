---
title: Cloud Cruiser ve API tümleştirmesi faturalandırma Microsoft Azure | Microsoft Docs
description: Microsoft Azure faturalama ortağında Cloud Cruiser, üründe Azure faturalandırma API'lerini tümleştirme deneyimlerini benzersiz bir perspektif sağlar.  Bu, özellikle kullanarak/Cloud Cruiser Microsoft Azure Pack için çalışırken, ilgileniyor Azure ve Cloud Cruiser müşterileri için kullanışlıdır.
services: ''
documentationcenter: ''
author: tonguyen
manager: tonguyen
editor: ''
tags: billing
ms.assetid: b65128cf-5d4d-4cbd-b81e-d3dceab44271
ms.service: billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 10/09/2017
ms.author: erikre
ms.openlocfilehash: 95d90e898ddc8766cf96a5a72c315407cd596393
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47393868"
---
# <a name="cloud-cruiser-and-microsoft-azure-billing-api-integration"></a>Cloud Cruiser ve API tümleştirmesi faturalandırma Microsoft Azure
Bu makalede nasıl yeni Microsoft Azure faturalama API'lerinden toplanan bilgiler Cloud Cruiser iş akışı maliyet simülasyon ve analiz için kullanılabileceğini açıklar.

## <a name="azure-ratecard-api"></a>Azure RateCard API'si
RateCard API'si Azure'dan oranı bilgi sağlar. Doğru kimlik bilgileriyle kimlik doğrulandıktan sonra teklif kimliği ile ilişkili ücretler yanı sıra, Azure'da kullanılabilen hizmetler hakkındaki meta verileri toplamak için API'yi sorgulayabilir.

Aşağıdaki örnek yanıt fiyatlar için A0 gösteren API'sinden. (Windows) örneği:

    {
        "MeterId": "0e59ad56-03e5-4c3d-90d4-6670874d7e29",
        "MeterName": "Compute Hours",
        "MeterCategory": "Virtual Machines",
        "MeterSubCategory": "A0 VM (Windows)",
        "Unit": "Hours",
        "MeterRates":
        {
            "0": 0.029
        },
        "EffectiveDate": "2014-08-01T00:00:00Z",
        "IncludedQuantity": 0.0,
        "MeterStatus": "Active"
    },

### <a name="cloud-cruisers-interface-to-azure-ratecard-api"></a>Cloud Cruiser'ın Azure RateCard API Arabirimi
Cloud Cruiser RateCard API'si bilgileri farklı şekillerde kullanabilirsiniz. Bu makale için nasıl, Iaas sağlamak için kullanılabilir olan göstereceğiz maliyet simülasyon ve analiz iş yükü.

Bu kullanım örneği göstermek için Microsoft Azure Pack (WAP) üzerinde çalışan çeşitli örneklerinin bir iş yükünü düşünün. Bu aynı iş yükünü azure'da benzetimini yapar ve bu geçiş işlemi gerçekleştirirken maliyetini tahmin etmek için kullanılan hedeftir. Bu simülasyonda oluşturabilmek için gerçekleştirilmesi gereken iki ana görevi vardır:

1. **İçeri aktarma ve işlem hizmet bilgilerini RateCard API toplanır.** Bu görev ayrıca burada RateCard API'si Ayıkla dönüştürülür ve yeni bir oran plana yayımlanan çalışma kitapları, gerçekleştirilir. Bu yeni fiyatları ücret planına simülasyonlar üzerinde Azure fiyatları tahmin etmek için kullanılır.
2. **WAP hizmetler ve Azure Iaas Hizmetleri Normalleştir.** Varsayılan olarak, WAP Hizmetleri dayalı tek tek kaynakları (CPU, bellek boyutu, Disk boyutu, vb.) Azure Hizmetleri örnek boyutu (A0, A1, A2, vb.) temel alır. Bu ilk görev Cloud Cruiser'ın ETL altyapısı tarafından gerçekleştirilir, çalışma kitapları, örnek boyutları, Azure örneği hizmetlerine benzer üzerinde bu kaynakları burada gönderilebilir çağrılır.

### <a name="import-data-from-the-ratecard-api"></a>Veri Al RateCard API
Cloud Cruiser çalışma kitapları, toplamak ve bilgileri RateCard API'si işlemek için otomatik bir yol sağlar.  ETL (ayıklama-dönüştürme-yükleme) çalışma kitapları koleksiyonu, dönüştürme ve veri yayımlama Cloud Cruiser veritabanına yapılandırmanıza olanak sağlar.

Her çalışma kitabı tamamlar veya kullanım verileri genişletmek için farklı kaynaklardan bilgi bağıntısını kurmanızı sağlayan bir veya birden çok koleksiyonlar olabilir. Aşağıdaki iki ekran görüntüleri, yeni bir oluşturma işlemi gösterilmektedir *koleksiyon* mevcut bir çalışma kitabını ve bilgilerini alma *koleksiyon* RateCard API'si:

![Şekil 1 - yeni bir koleksiyon oluşturma][1]

![Şekil 2 - yeni koleksiyondaki verileri alma][2]

Çalışma kitabına verileri içeri aktardıktan sonra birden çok adımları ve veri modeli ve değiştirmek için dönüştürme işlemleri oluşturmak mümkündür. Biz yalnızca-olarak-hizmet altyapı (Iaas) dönüştürme adımı gereksiz satırları kaldırmak için kullanabiliriz veya kayıtları ilgilendiğiniz olduğundan, bu örnekte, Iaas dışındaki hizmetlere ilgili.

Aşağıdaki ekran görüntüsünde RateCard API toplanan verileri işlemek için kullanılan dönüştürme adımı gösterir:

![Şekil 3 - RateCard API'si toplanan veriyi işlemek için dönüştürme adımı][3]

### <a name="defining-new-services-and-rate-plans"></a>Yeni hizmetler ve oranı tanımlama planları
Cloud Cruiser üzerinde hizmetleri tanımlamak için farklı yolu vardır. Seçeneklerden birini Hizmetleri kullanım verilerini içe aktarılmamasıdır. Bu yöntem, genel Bulutlar, sağlayıcı tarafından Hizmetleri zaten tanımlandığı ile çalışırken yaygın olarak kullanılır.

Ücret planı oranları veya geçerlilik tarihlerini veya diğer seçenekler arasında bir müşteri grubu göre farklı hizmetlerde uygulanan fiyatlar kümesidir. Tarifeler benzetimi veya Hizmetleri'ndeki değişiklikleri bir iş yükü'nın toplam maliyeti nasıl etkileyebileceğini anlaması için "What-if" senaryolarını oluşturmak için Cloud Cruiser üzerinde de kullanılabilir.

Bu örnekte, yeni hizmetler Cloud Cruiser tanımlamak için hizmet bilgileri RateCard API'si kullanın. Aynı şekilde, yeni bir ücret planı üzerinde Cloud Cruiser oluşturmak için hizmetlere ilişkili ücretler kullanabiliriz.

Dönüştürme işleminin sonunda, yeni bir adım oluşturmak ve yeni hizmetler ve ücretleri RateCard API'si verileri yayımlamak mümkündür.

![Şekil 4 - Yeni hizmetler ve oranları RateCard API'si verileri yayımlama][4]

### <a name="verify-azure-services-and-rates"></a>Azure Hizmetleri ve fiyatları doğrulayın
Hizmet ve fiyatlar yayımladıktan sonra içeri aktarılan Cloud Cruiser'ın hizmet listesinde doğrulayabilirsiniz *Hizmetleri* sekmesinde:

![Şekil 5 - yeni hizmetleri doğrulanıyor][5]

Üzerinde *fiyat planları* sekmesinde hızına "AzureSimulation RateCard API'SİNDEN alınan" adlı yeni fiyat planına kontrol edebilirsiniz.

![Şekil 6 - yeni ücret planı ve ilişkili ücretleri doğrulanıyor][6]

### <a name="normalize-wap-and-azure-services"></a>WAP ve Azure Hizmetleri normalleştirin
Varsayılan olarak, WAP tabanlı işlem, bellek ve ağ kaynaklarını kullanma hakkında kullanım bilgileri sağlar. Cloud Cruiser hizmetlerinizi göre tanımlayabilirsiniz. Bu kaynak tarifeli kullanımı ve ayırma üzerinde doğrudan. Örneğin, CPU kullanımı, her saat için temel fiyatı ayarlayabilir veya GB'ın bir örneğine ayrılan bellek ücret alınır.

Bu örnekte, WAP'ı ve Azure arasında maliyetleri karşılaştırmak için WAP üzerindeki kaynak kullanımının ardından Azure Hizmetleri için eşleşen paketleri içine toplamak ihtiyacımız var. Bu dönüştürme, çalışma kitaplarını kolayca uygulanabilir:

![Şekil 7 - Hizmetleri'leri normalleştirmek için WAP verileri dönüştürme][7]

Çalışma kitabı son adım, verileri Cloud Cruiser veritabanına yayımlamaktır. Bu adım sırasında kullanım verilerini artık (yani Azure hizmetlerine harita) Hizmetleri dizinimize ve ücretleri oluşturmak için varsayılan hızlarıyla bağlanır.

Çalışma kitabı bittikten sonra bir Görev zamanlayıcıda ekleyerek ve sıklığını ve zamanını çalıştırmak çalışma kitabı için belirtme verilerin işlenmesini otomatikleştirebilirsiniz.

![Şekil 8 - çalışma kitabı zamanlama][8]

### <a name="create-reports-for-workload-cost-simulation-analysis"></a>İş yükü için maliyet benzetim analizi raporları oluşturma
Kullanım toplandığı ve bu ücretleri Cloud Cruiser veritabanı'na yüklenen sonra Cloud Cruiser Insights modülü istediğimiz benzetimi maliyet iş yükü oluşturmak için kullanabiliriz.

Bu senaryoyu göstermek için aşağıdaki rapor oluşturduk:

![Maliyet karşılaştırması][9]

Üst grafik maliyet karşılaştırması, WAP (mavi) ve Azure (mavi) arasında belirli her hizmet için bir iş yükü çalıştırmaya fiyat karşılaştırma, hizmetleri tarafından gösterir.

Alt grafik aynı verileri gösterir ancak bölüm göre ayrılmış. WAP ve Azure, bir bunların arasındaki farkı yanı sıra iş yükünü çalıştırmak her bölüm için maliyetleri (yeşil) tasarruf çubuğunda görüntülenir.

## <a name="azure-usage-api"></a>Azure kullanım API'si
### <a name="introduction"></a>Giriş
Microsoft kısa süre önce tüketimi için Öngörüler edinmek için kullanım verilerindeki programlı olarak çekmek abonelerin Azure kullanım API'si tanıtımını. Cloud Cruiser müşteriler, bu API üzerinden mevcut daha zengin veri kümesi avantajlarından yararlanabilirsiniz.

Cloud Cruiser çeşitli şekillerde kullanım API'si ile tümleştirme özelliğini kullanabilirsiniz. Ayrıntı düzeyi (saatlik kullanım bilgileri) ve kaynak meta veri bilgilerini API üzerinden mevcut esnek hesaplama ve Ücret yansıtma modellerini desteklemek için gerekli veri kümesi sağlar. 

Bu öğreticide, biz Cloud Cruiser kullanım API'si bilgileri nasıl avantaj elde edebileceği bir örneği yok. Daha açık belirtmek gerekirse biz Azure'da bir kaynak grubu oluşturun, hesap yapısı için etiketler ilişkilendirin ve ardından çekme ve Cloud Cruiser etiketi bilgileri işleme sürecini açıklar.

Nihai amaç, aşağıdakine benzer raporlar oluşturabilir ve maliyet ve etiketlere göre doldurulur hesabı yapısına göre tüketimi analiz çalıştırabilmesi için sağlamaktır.

![Şekil 10 - dökümü etiketleri kullanarak raporla][10]

### <a name="microsoft-azure-tags"></a>Microsoft Azure etiketleri
Azure kullanım API üzerinden mevcut veriler yalnızca tüketimi bilgilerini, aynı zamanda ilişkili herhangi bir etiket dahil olmak üzere kaynak meta verileri içerir. Etiketleri kaynaklarınızı düzenlemek için ancak etkili olması için kolay bir yol sağlar, emin olmanız gerekir:

* Etiketler doğru sağlama zamanında kaynaklara uygulanır
* Etiketler, kuruluş hesabı yapısına kullanım bağlamak için hesaplama/yansıtma işlemi düzgün şekilde kullanılır.

Özellikle, hazırlama veya yan şarj manuel bir işlem olduğunda bu gereksinimlerin ikisini de zorlu olabilir. Yanlış yazılan, hatalı veya eksik bile etiketleri müşterilerden yaygın şikayetlerinden alındığında etiketler ve bu hataları kullanarak yaşam şarj tarafında zorlaştırabilir.

Yeni Azure kullanım API'si ile Cloud Cruiser kaynak etiketleme bilgileri çekmek ve çalışma kitapları olarak adlandırılan bir karmaşık ETL aracı aracılığıyla ortak bu etiketleme hataları düzeltin. Normal ifadeler ile veri korelasyonunda dönüştürme Cloud Cruiser yanlış etiketli kaynakları tanımlayabilir ve tüketici doğru ilişkilendirmesini kaynakları sağlama doğru etiketleri uygulayın.

Şarj tarafında Cloud Cruiser hesaplama/yansıtma işlemini otomatikleştiren ve etiket bilgileri kullanım için uygun bir tüketici (departman, bölme, proje, vb.) bağlamak için kullanabilirsiniz. Bu Otomasyon büyük bir geliştirme sağlar ve tutarlı ve denetlenebilir bir şarj işlem emin olabilirsiniz.

### <a name="creating-a-resource-group-with-tags-on-microsoft-azure"></a>Microsoft Azure üzerinde etiketler ile bir kaynak grubu oluşturma
Azure portalında bir kaynak grubu oluşturmak için Bu öğreticide ilk adım olan kaynaklarla ilişkilendirmek için yeni etiketler oluşturup. Bu örnek için aşağıdaki etiketlerin oluşturmakta: departmanı, ortam, sahibi, proje.

Aşağıdaki ekran görüntüsünde, bir ' % s'örneği kaynak grubu ile ilişkili etiket gösterilmektedir.

![Şekil 11 - Azure portalında ilgili etiketler ile kaynak grubu][11]

Sonraki adım, Cloud Cruiser kullanım API'den bilgileri almaktır. Kullanım API'si şu anda yanıtları JSON biçiminde sağlar. Alınan veri örneği aşağıdadır:

    {
      "id": "/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXX/providers/Microsoft.Commerce/UsageAggregates/Daily_BRSDT_20150623_0000",
      "name": "Daily_BRSDT_20150623_0000",
      "type": "Microsoft.Commerce/UsageAggregate",
      "properties":
      {
        "subscriptionId": "bb678b04-0e48-4b44-XXXX-XXXXXXXXX",
        "usageStartTime": "2015-06-22T00:00:00+00:00",
        "usageEndTime": "2015-06-23T00:00:00+00:00",
        "meterName": "Compute Hours",
        "meterRegion": "",
        "meterCategory": "Virtual Machines",
        "meterSubCategory": "Standard_D1 VM (Non-Windows)",
        "unit": "Hours",
        "instanceData": "{\"Microsoft.Resources\":{\"resourceUri\":\"/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXXX/resourceGroups/DEMOUSAGEAPI/providers/Microsoft.Compute/virtualMachines/MyDockerVM\",\"location\":\"eastus\",\"tags\":{\"Department\":\"Sales\",\"Project\":\"Demo Usage API\",\"Environment\":\"Test\",\"Owner\":\"RSE\"},\"additionalInfo\":{\"ImageType\":\"Canonical\",\"ServiceType\":\"Standard_D1\"}}}",
        "meterId": "e60caf26-9ba0-413d-a422-6141f58081d6",
        "infoFields": {},
        "quantity": 8

      },
    },


### <a name="import-data-from-the-usage-api-into-cloud-cruiser"></a>Veri kullanımı API'den Cloud Cruiser ' aktarın.
Cloud Cruiser çalışma kitapları, toplamak ve bilgileri kullanım API'si işlemek için otomatik bir yol sağlar. Bir ETL (ayıklama-dönüştürme-yükleme) çalışma kitabı koleksiyonu, dönüştürme ve veri yayımlama Cloud Cruiser veritabanına yapılandırmanıza olanak sağlar.

Her çalışma kitabı, bir veya birden çok koleksiyon olabilir. Bu bilgileri tamamlar veya kullanım verileri genişletmek için farklı kaynaklardan bağıntısını kurmanızı sağlar. Bu örnekte, yeni bir sayfa Azure şablonu çalışma kitabında oluşturacağız (*UsageAPI)* ve yeni bir *koleksiyon* kullanım API'den bilgilerini almak için.

![Şekil 3 - kullanım API'si veri UsageAPI sayfasına içeri aktarıldı][12]

Bu çalışma kitabı zaten Azure tarafından sağlanan hizmetleri almak için diğer sayfaları olduğunu göreceksiniz (*ImportServices*) ve faturalandırma API'si tüketim bilgileri işleme (*PublishData*).

Ardından, kullanım API'si doldurmak için kullanırız *UsageAPI* sayfası ve hakkında bilgi faturalandırma API'si tüketim verilerle ilişkilendirmek *PublishData* sayfası.

### <a name="processing-the-tag-information-from-the-usage-api"></a>Etiket bilgileri kullanım API'si işlem
Çalışma kitabına verileri içeri aktardıktan sonra dönüştürme adımlarda oluşturduğumuz *UsageAPI* API'sinden bilgileri işlemek için sayfa. İlk adım, tek bir alan etiketleri ayıklayın, ardından bunların (departman, proje, sahibi ve ortam) her biri için alanları oluşturmak için bir "Böl JSON" işlemci kullanmaktır.

![Şekil 4 - etiket bilgileri için yeni alanlar oluşturun][13]

"Ağ" hizmet (sarı kutusu) etiketi bilgileri eksik, ancak biz bakarak aynı kaynak grubunun parçası olduğunu doğrulayabilirsiniz fark *ResourceGroupName* alan. Biz bu kaynak grubundaki diğer kaynaklar için etiketler olduğundan, bu bilgi işlemi daha sonra bu kaynağın eksik etiketleri uygulamak için kullanabiliriz.

Etiket bilgileri ilişkilendirerek arama tablosu oluşturma için sonraki adımdır *ResourceGroupName*. Bu arama tablosu sonraki etiket bilgilerle tüketim verilerini zenginleştirmek için kullanılır.

### <a name="adding-the-tag-information-to-the-consumption-data"></a>Etiket bilgileri tüketim veri ekleme
Şimdi biz atlayabilirsiniz *PublishData* sayfası, hangi faturalandırma API'si tüketim bilgileri işler ve etiketleri ayıklanan alanları ekleyin. Bu işlem, önceki adımda oluşturulan arama tablosuna bakarak gerçekleştirilir kullanarak *ResourceGroupName* aramaları için anahtar olarak.

![Şekil 5 - hesap yapısı aramaları bilgilerle doldurma][14]

"Ağ" hizmeti için uygun hesap yapı alanı uygulandığını eksik etiketlerle sorunu düzeltme dikkat edin. Biz de kaynaklar için hesap yapı alanı bizim ' % s'hedef kaynak grubu "Diğer" dışında raporlarda ayrılmaları için doldurulur.

Artık şu kullanım verilerini yayımlamak için bir adım eklemek yeterlidir. Bu adım sırasında veritabanı'na yüklenen elde edilen ücret ile kullanım bilgilerini bizim ücret planı üzerinde tanımlanan her hizmet için uygun fiyatları uygulanır.

En iyi tarafı, yalnızca bir kez bu süreçte Git olmasıdır. Çalışma kitabı tamamlandığında, Zamanlayıcı eklemeniz yeterlidir ve saatlik veya günlük zamanlanan saatte çalıştırır. Ardından, yeni rapor oluşturma veya var olanları bulut kullanımınızı anlamlı bilgiler almak için verileri analiz etmek için özelleştirme adımlarından oluşur.

### <a name="next-steps"></a>Sonraki Adımlar
* Cloud Cruiser çalışma kitapları ve raporlarınızı oluşturmaya ilişkin ayrıntılı yönergeler için Cloud Cruiser için kullanıcının çevrimiçi başvuruda [belgeleri](http://docs.cloudcruiser.com/) (gerekli geçerli oturum açma).  Cloud Cruiser hakkında daha fazla bilgi için [ info@cloudcruiser.com ](mailto:info@cloudcruiser.com).
* Bkz: [Microsoft Azure kaynak tüketiminize öngörü](billing-usage-rate-card-overview.md) Azure kaynak kullanım ve RateCard API'leri genel bakış.
* Kullanıma [Azure faturalandırma REST API Başvurusu](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) hem API'leri hakkında daha fazla bilgi için Azure Resource Manager tarafından sağlanan API'lerden oluşan bir kümenin parçası olan.
* Örnek kodun içine doğrudan içine dalmak istiyorsanız, Microsoft Azure faturalama API kod Örneklerimize atın [Azure Kod örnekleri](https://azure.microsoft.com/documentation/samples/?term=billing).

### <a name="learn-more"></a>Daha Fazla Bilgi Edinin
* Azure Resource Manager hakkında daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](../azure-resource-manager/resource-group-overview.md) makalesi.

<!--Image references-->

[1]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Create-New-Workbook-Collection.png "Şekil 1 - yeni bir koleksiyon oluşturma"
[2]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Import-Data-From-RateCard.png "Şekil 2 - yeni koleksiyondaki verileri alma"
[3]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transformation-Steps-Process-RateCard-Data.png "Şekil 3 - RateCard API'si toplanan veriyi işlemek için dönüştürme adımı"
[4]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Publish-RateCard-Data-New-Services-Rates.png "Şekil 4 - Yeni hizmetler ve oranları RateCard API'si verileri yayımlama"
[5]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing1.png "Şekil 5 - yeni hizmetleri doğrulanıyor"
[6]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing2.png "Şekil 6 - yeni ücret planı ve ilişkili ücretleri doğrulanıyor"
[7]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transforming-WAP-Normalize-Services.png "Şekil 7 - Hizmetleri'leri normalleştirmek için WAP verileri dönüştürme"
[8]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workbook-Scheduling.png "Şekil 8 - çalışma kitabı zamanlama"
[9]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workload-Cost-Simulation-Report.png "Şekil 9 - iş yükü maliyet karşılaştırması senaryo için örnek rapor"
[10]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/1_ReportWithTags.png "Şekil 10 - dökümü etiketleri kullanarak raporla"
[11]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/2_ResourceGroupsWithTags.png "Şekil 11 - Azure portalında ilgili etiketler ile kaynak grubu"
[12]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/3_ImportIntoUsageAPISheet.png "Şekil 12 - kullanım API'si veri UsageAPI sayfasına içeri aktarıldı"
[13]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/4_NewTagField.png "Şekil 13 - etiket bilgileri için yeni alanlar oluşturun"
[14]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/5_PopulateAccountStructure.png "Şekil 14 - hesap yapısı aramaları bilgilerle doldurma"
