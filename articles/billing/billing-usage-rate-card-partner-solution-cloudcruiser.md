---
title: Bulut Cruiser ve Microsoft Azure API tümleştirme faturalama | Microsoft Docs
description: Microsoft Azure fatura ortağında bulut Cruiser kendi üründe Azure faturalama API'ları tümleştirme deneyimlerini benzersiz bir perspektif sağlar.  Bu, özellikle kullanarak/bulut Cruiser Microsoft Azure Pack için çalışırken ilginizi çekiyor mu Azure ve bulut Cruiser müşteriler için kullanışlıdır.
services: ''
documentationcenter: ''
author: tonguyen
manager: tonguyen
editor: ''
tags: billing
ms.assetid: b65128cf-5d4d-4cbd-b81e-d3dceab44271
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 10/09/2017
ms.author: mobandyo;sirishap;bryanla
ms.openlocfilehash: 8ddb81078e8019284c0481d4ea8d72253d3f0a5a
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="cloud-cruiser-and-microsoft-azure-billing-api-integration"></a>Cruiser ve Microsoft Azure API tümleştirme faturalama bulut
Bu makalede nasıl yeni Microsoft Azure fatura API'lerden toplanan bilgiler bulut Cruiser iş akışı maliyet benzetimi ve analiz için kullanılabileceği açıklanır.

## <a name="azure-ratecard-api"></a>Azure RateCard API
RateCard API Azure'dan oranı bilgi sağlar. Doğru kimlik bilgileriyle kimlik doğrulandıktan sonra teklif kimliğiyle ilişkili oranları yanı sıra Azure üzerinde kullanılabilir hizmetler hakkındaki meta verileri toplamak için API'yi sorgulayabilir.

Aşağıdaki örnek yanıt için A0 fiyatlar gösteren API değil (Windows) örnek:

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

### <a name="cloud-cruisers-interface-to-azure-ratecard-api"></a>Azure RateCard API Cruiser'ın arabirime bulut
Bulut Cruiser RateCard API bilgilerini farklı yollarla kullanabilirsiniz. Bu makale için nasıl bu Iaas yapmak için kullanılabilir olan gösteriyoruz benzetimi ve analiz maliyetiyle iş yükü.

Bu kullanım örneği göstermek için Microsoft Azure Pack (WAP) üzerinde çalışan çeşitli örneklerinin bir iş yükü düşünün. Bu aynı iş yükünü Azure üzerinde benzetimini gerçekleştirmek ve bu tür geçiş yaparken maliyetlerini tahmin etmek için belirtilir. Bu benzetim oluşturmak için iki ana görevlerinin gerçekleştirilmesi için vardır:

1. **Alma ve işlem hizmet bilgilerini RateCard API'SİNDEN toplanır.** Bu görevi burada RateCard API'sinden Ayıkla dönüştürülen ve yeni bir oran planı yayımlanan çalışma kitaplarını da gerçekleştirilir. Bu yeni oranı plan üzerinde benzetimleri Azure fiyatları tahmin etmek için kullanılır.
2. **WAP Hizmetleri ve Iaas için Azure services normalleştirin.** Varsayılan olarak, WAP Hizmetleri tabanlı kaynakların (CPU, bellek boyutu, Disk boyutu, vb.) Azure Hizmetleri örnek boyutu (A0, A1, A2, vb.) dayanır. Bu ilk görev bulut Cruiser'ın ETL altyapısı tarafından gerçekleştirilen, örnek boyutları, Azure örneği hizmetlerine benzer üzerinde bu kaynakların nerede gönderilebilir çalışma kitaplarını çağrılır.

### <a name="import-data-from-the-ratecard-api"></a>RateCard API'SİNDEN veri alma
Bulut Cruiser çalışma kitaplarını toplamak ve RateCard API bilgilerinden işlemek için otomatik bir yol sağlar.  ETL (ayıklama dönüştürme yük) çalışma kitaplarını toplama, dönüştürme ve veri yayımlama bulut Cruiser veritabanına yapılandırmanıza olanak tanır.

Her çalışma kitabı tamamlar veya kullanım verileri büyütmek için farklı kaynaklardan bilgi ilişkilendirmenize olanak tanıyan bir veya birden çok koleksiyonlar olabilir. Aşağıdaki iki ekran görüntüleri yeni bir oluşturmayı gösterir *koleksiyonu* varolan bir çalışma kitabını ve bilgileri alma *koleksiyonu* RateCard API'sinden:

![Şekil 1 - yeni bir koleksiyon oluşturma][1]

![Şekil 2 - yeni koleksiyon verileri alın][2]

Çalışma kitabına verileri aldıktan sonra birden çok adımları ve dönüştürme işlemlerini, değiştirmek ve veri modeli için oluşturmak mümkündür. Biz yalnızca-olarak-hizmet altyapı (Iaas) dönüştürme adımları biz gereksiz satırları kaldırmak için kullanabilirsiniz veya kayıtları ilgilendiğiniz olduğundan, bu örnekte, Iaas dışında Hizmetleri ilgili.

Aşağıdaki ekran görüntüsünde RateCard API'SİNDEN toplanan verileri işlemek için kullanılan dönüştürme adımlar gösterilmektedir:

![Şekil 3 - RateCard API'sinden toplanan verileri işlemek için dönüştürme adımları][3]

### <a name="defining-new-services-and-rate-plans"></a>Yeni hizmetler ve oranı tanımlama planları
Bulut Cruiser üzerinde hizmetleri tanımlamak için farklı yolu vardır. Seçenekleri Hizmetleri kullanım verilerini almak için biridir. Bu yöntem, genel Bulutlar, sağlayıcı tarafından Hizmetleri zaten tanımlandığı ile çalışırken, yaygın olarak kullanılır.

Ücret planı oranları veya geçerlilik tarihlerini ya da diğer seçenekler arasında müşteri grubunu göre farklı hizmetlerine uygulanabilir fiyatları kümesidir. Oranı planları benzetimi veya değişiklikleri Hizmetleri'nde bir iş yükü toplam maliyetini nasıl etkileyebileceğini anlaması için "What-if" senaryoları oluşturmak için bulut Cruiser üzerinde de kullanılabilir.

Bu örnekte, bulut Cruiser yeni hizmetleri tanımlamak için RateCard API'sinden hizmet bilgileri kullanın. Aynı şekilde, biz hizmetlere ilişkili oranları bulut Cruiser üzerinde yeni bir ücret planı oluşturmak için kullanabilirsiniz.

Dönüştürme işleminin sonunda, yeni bir adımı oluşturmak ve yeni hizmetler ve ücretlerin RateCard API verilerini yayımlamak mümkündür.

![Şekil 4 - Yeni hizmetler ve ücretlerin RateCard API verileri yayımlama][4]

### <a name="verify-azure-services-and-rates"></a>Azure Hizmetleri ve ücretlerin doğrulayın
Hızları ve Hizmetleri yayımlandıktan sonra bulut Cruiser'ın alınan hizmet listesini doğrulayabilirsiniz *Hizmetleri* sekmesi:

![Şekil 5 - yeni hizmetler doğrulanıyor][5]

Üzerinde *oranı planları* sekmesinde hızı "AzureSimulation RateCard API'SİNDEN alındı" olarak adlandırılan yeni oranı planı kontrol edebilirsiniz.

![Şekil 6 - yeni oranı planlamak ve ilişkili ücretleri doğrulanıyor][6]

### <a name="normalize-wap-and-azure-services"></a>WAP ve Azure Hizmetleri normalleştirin
Varsayılan olarak, WAP hesaplama, bellek ve ağ kaynaklarını kullanıma bağlı kullanım bilgileri sağlar. Bulut Cruiser içinde hizmetlerinizin temel tanımlayabilirsiniz doğrudan ayırma ya da bu kaynakların ölçülen kullanımı üzerinde. Örneğin, CPU kullanımı her bir saat için bir temel oranını ayarlamak veya GB'lik bir örneğine ayrılan bellek gider.

Bu örnekte, WAP ve Azure arasında maliyetlerini karşılaştırmak için biz WAP üzerinde kaynak kullanımı Azure hizmetlerine eşlenebilir paketleri içine gerekir. Bu dönüşüm kolayca yer alan çalışma kitaplarına uygulanabilir:

![Şekil 7 - Hizmetleri normalleştirmek için WAP verileri dönüştürme][7]

Çalışma kitabı en son adım, bulut Cruiser veritabanına veri yayımlamaktır. Bu adım sırasında kullanım verileri şimdi (yani Azure hizmetlerini eşleştirme) hizmetlerine paketlenmiş ve ücretleri oluşturmak için varsayılan hızlarıyla bağlanır.

Çalışma kitabı bittikten sonra bir Görev Zamanlayıcısı'ekleme ve çalıştırmak çalışma kitabı için saat ve sıklığı belirterek verilerinin işlenmesini otomatikleştirebilirsiniz.

![Şekil 8 - çalışma kitabı planlama][8]

### <a name="create-reports-for-workload-cost-simulation-analysis"></a>İş yükü maliyet benzetimi analiz için rapor oluşturma
Kullanım toplanır ve ücretleri bulut Cruiser veritabanına yüklenen sonra biz bulut Cruiser Öngörüler modülü biz işlemleriniz benzetimi maliyet iş yükü oluşturmak için kullanabilirsiniz.

Bu senaryoyu göstermek için aşağıdaki rapor oluşturduk:

![Maliyet karşılaştırma][9]

Üst grafik maliyet karşılaştırmasını WAP (mavi) ve Azure (mavi) arasında belirli her hizmet için iş yükü çalıştırmanın fiyat karşılaştırma, hizmetleri tarafından gösterir.

Aynı veri alt grafik gösterir ancak departmanı göre ayrılmış. Aralarındaki fark birlikte WAP ve Azure, iş yükünü çalıştırmak her bölüm için maliyetleri (yeşil) tasarrufları çubuğunda görüntülenir.

## <a name="azure-usage-api"></a>Azure kullanım API'si
### <a name="introduction"></a>Giriş
Microsoft, kendi tüketimi Öngörüler elde etmek üzere kullanım verileri, programlı olarak çekmesini abonelerin Azure kullanım API'si kısa süre önce kullanıma sunmuştur. Bulut Cruiser müşteriler bu API aracılığıyla kullanılabilir daha zengin veri kümesi yararlanabilir.

Bulut Cruiser çeşitli şekillerde kullanım API'si ile tümleştirme özelliğini kullanabilirsiniz. Ayrıntı düzeyi (saatlik kullanım bilgileri) ve kaynak meta veri bilgileri API aracılığıyla kullanılabilir esnek giderleri veya geri ödeme modelleri desteklemek için gerekli veri kümesi sağlar. 

Bu öğretici kapsamında, bulut Cruiser kullanım API'si bilgileri nasıl yararlanabilir bir örnek sunar. Daha açık belirtmek gerekirse, biz Azure üzerinde bir kaynak grubu oluşturun, hesap yapısı için etiketler ilişkilendirin ve ardından çekme ve bulut Cruiser içine etiket bilgilerini işleme işlemi açıklanmaktadır.

Son aşağıdakine benzer raporlar oluşturun ve maliyet ve etiketlere göre doldurulmuş hesap yapısı göre tüketimini çözümlemek kurabilmeleri için belirtilir.

![Şekil 10 - etiketleri kullanarak dökümleri raporla][10]

### <a name="microsoft-azure-tags"></a>Microsoft Azure Tags
Azure kullanım API aracılığıyla kullanılabilir verileri yalnızca tüketim bilgileri, aynı zamanda kendisiyle ilişkilendirilmiş herhangi bir etiket dahil olmak üzere kaynak meta verileri içerir. Etiketleri kaynaklarınızı düzenleme, ancak etkili olması için kolay bir yol sağlar, emin olmanız gerekir:

* Etiketleri doğru sağlama zaman kaynaklarına uygulanır
* Etiketler, kuruluş hesabı yapısına kullanım bağlamanın giderleri/geri ödeme işlemi düzgün bir şekilde kullanılır.

Özellikle sağlamak veya yan şarj manuel bir işlem olduğunda bu gereksinimleri her ikisi de zorlu olabilir. Yanlış yazılan, yanlış ya da hatta eksik etiketleri müşterilerden yaygın şikayetlerinden alındığında etiketler ve bu hatalar kullanarak yaşam doluyor tarafında zorlaştırabilir.

Yeni Azure kullanım API'si ile bulut Cruiser kaynak etiketleme bilgilerine pull ve çalışma kitapları olarak adlandırılan bir karmaşık ETL aracı aracılığıyla ortak bu etiketleme hataları düzeltin. Normal ifadeler ve veri bağıntı kullanarak dönüştürme bulut Cruiser yanlış etiketli kaynakları belirleyin ve tüketici doğru ilişkilendirmesini kaynakların sağlama doğru etiketleri uygulayın.

Şarj tarafında bulut Cruiser giderleri/geri ödeme işlemini otomatikleştirir ve uygun bir tüketici (departman, bölme, proje, vb.) için kullanım bağlamanın etiket bilgilerini kullanabilirsiniz. Bu Otomasyon büyük bir geliştirme sağlar ve tutarlı ve denetlenebilir doluyor işlem emin olabilirsiniz.

### <a name="creating-a-resource-group-with-tags-on-microsoft-azure"></a>Microsoft Azure üzerinde etiketlere sahip bir kaynak grubu oluşturma
Bu öğreticinin ilk adımı Azure portalında bir kaynak grubu oluşturmaktır kaynaklara ilişkilendirmek için yeni etiketler oluşturma. Bu örnek için aşağıdaki etiketlerin oluşturuluyor: departman, ortam, sahibi, proje.

Aşağıdaki ekran görüntüsünde, örnek bir kaynak grubu ilişkilendirilmiş etiketleri gösterir.

![Şekil 11 - Azure portalında ilişkili etiketlerle kaynak grubu][11]

Sonraki adım, bulut Cruiser kullanım API'SİNDEN bilgileri almaktır. Kullanım API'si şu anda JSON biçiminde yanıtlar sağlar. Alınan veri örneği şöyledir:

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


### <a name="import-data-from-the-usage-api-into-cloud-cruiser"></a>Veri kullanımı API'SİNDEN bulut Cruiser aktarın.
Bulut Cruiser çalışma kitaplarını toplama ve kullanım API'si bilgilerinden işlemek için otomatik bir yol sağlar. ETL (ayıklama dönüştürme yük) çalışma kitabı, toplama, dönüştürme ve veri yayımlama bulut Cruiser veritabanına yapılandırmanıza olanak sağlar.

Her çalışma kitabı bir veya birden çok koleksiyon olabilir. Bu bilgileri tamamlar veya kullanım verileri büyütmek için farklı kaynaklardan ilişkilendirmenize olanak sağlar. Bu örnekte, yeni bir sayfa Azure şablonu çalışma kitabında oluşturuyoruz (*UsageAPI)* ve yeni bir küme *koleksiyonu* kullanım API'SİNDEN bilgilerini almak için.

![Şekil 3 - kullanım API'si verileri UsageAPI sayfasına alındı][12]

Bu çalışma kitabı zaten Azure Hizmetleri almak için diğer sayfaları sahip olmadığına dikkat edin (*ImportServices*) ve faturalama API'sinden tüketim bilgi işlem (*PublishData*).

Ardından, kullanım API'si doldurmak için kullanırız *UsageAPI* sayfa ve üzerinde verilere faturalama API'sinden bilgilerle ilişkilendirmek *PublishData* sayfası.

### <a name="processing-the-tag-information-from-the-usage-api"></a>Kullanım API'sinden etiket bilgilerini işleme
Çalışma kitabına verileri aldıktan sonra dönüştürme adımlarda oluşturuyoruz *UsageAPI* API bilgilerinden işlenmesi için sayfa. İlk adım, tek bir alandan etiketler ayıklayın, sonra bunların (departman, proje, sahibi ve ortam) her biri için alanları oluşturmak için "JSON bölme" işlemci kullanmaktır.

![Şekil 4 - etiket bilgileri için yeni alanlar oluşturma][13]

"Ağ" hizmet etiket bilgilerini (sarı kutusu) eksik, ancak biz bakarak aynı kaynak grubunun parçası olduğunu doğrulayabilirsiniz fark *ResourceGroupName* alan. Biz bu kaynak grubundaki diğer kaynaklar için etiketler sahip olduğundan, biz işlemde daha sonra bu kaynak eksik etiketler uygulamak için bu bilgileri kullanabilirsiniz.

Etiketleri bilgilerinden ilişkilendirme arama tablosu oluşturmak için sonraki adımdır *ResourceGroupName*. Bu arama tablosu, sonraki adımla etiketi bilgileriyle Tüketim verileri zenginleştirmek için kullanılır.

### <a name="adding-the-tag-information-to-the-consumption-data"></a>Etiket bilgilerini tüketim veri ekleme
Biz atlayabilirsiniz artık *PublishData* sayfası, hangi faturalama API'sinden tüketim bilgileri işler ve etiketleri ayıklanan alanlarını ekleyin. Bu işlem, önceki adımda oluşturulan arama tablosu bakarak gerçekleştirilir kullanarak *ResourceGroupName* aramalar için anahtar olarak.

![Şekil 5 - arama bilgileriyle hesap yapısı doldurma][14]

"Ağ" hizmeti için uygun hesap yapısı alanları uygulandığını eksik etiketleriyle sorunu düzeltme dikkat edin. Biz de kaynaklar için hesap yapısı alanları "Diğer" bizim hedef kaynak grubu dışında raporları farklılaştırmak için doldurulur.

Şimdi biz kullanım verilerini yayımlamak için bir adım eklemek yeterlidir. Bu adım sırasında bizim ücret planı tanımlanan her hizmet için uygun hızlarını veritabanına yüklenen elde edilen ücret ile kullanım bilgileri için geçerlidir.

Yalnızca bir kez bu süreçte Git elinizde en iyi parçasıdır. Çalışma kitabı tamamlandığında, Zamanlayıcı eklemeniz yeterlidir ve onu saatlik veya günlük zamanlanan saatte çalışır. Ardından, yeni raporları oluşturma veya var olanları bulut kullanımınızı anlamlı bilgiler elde etmek için verileri çözümlemek amacıyla özelleştirme yalnızca bir konudur.

### <a name="next-steps"></a>Sonraki Adımlar
* Bulut Cruiser çalışma kitaplarını ve raporları oluşturma hakkında ayrıntılı yönergeler için bulut Cruiser için bilgisayarın çevrimiçi başvuruda [belgelerine](http://docs.cloudcruiser.com/) (geçerli oturum açması gerekli).  Bulut Cruiser hakkında daha fazla bilgi için ilgili kişi [ info@cloudcruiser.com ](mailto:info@cloudcruiser.com).
* Bkz: [Microsoft Azure kaynak tüketimini Öngörüler elde](billing-usage-rate-card-overview.md) RateCard API'leri ve Azure kaynak kullanımı genel bakış.
* Kullanıma [Azure faturalama REST API Başvurusu](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) hem API'ler hakkında daha fazla bilgi için parçası olan Azure kaynak yöneticisi tarafından sağlanan API kümesi.
* Bizim Microsoft Azure fatura API kod örnekleri sağ örnek koda dalın istiyorsanız kontrol [Azure Kod örnekleri](https://azure.microsoft.com/documentation/samples/?term=billing).

### <a name="learn-more"></a>Daha Fazla Bilgi Edinin
* Azure Kaynak Yöneticisi hakkında daha fazla bilgi edinmek için [Azure Resource Manager'a genel bakış](../azure-resource-manager/resource-group-overview.md) makalesi.

<!--Image references-->

[1]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Create-New-Workbook-Collection.png "Şekil 1 - yeni bir koleksiyon oluşturma"
[2]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Import-Data-From-RateCard.png "Şekil 2 - yeni koleksiyon verileri alın"
[3]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transformation-Steps-Process-RateCard-Data.png "Şekil 3 - RateCard API'sinden toplanan verileri işlemek için dönüştürme adımları"
[4]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Publish-RateCard-Data-New-Services-Rates.png "Şekil 4 - Yeni hizmetler ve ücretlerin RateCard API verileri yayımlama"
[5]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing1.png "Şekil 5 - yeni hizmetler doğrulanıyor"
[6]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing2.png "Şekil 6 - yeni oranı planlamak ve ilişkili ücretleri doğrulanıyor"
[7]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transforming-WAP-Normalize-Services.png "Şekil 7 - Hizmetleri normalleştirmek için WAP verileri dönüştürme"
[8]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workbook-Scheduling.png "Şekil 8 - çalışma kitabı planlama"
[9]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workload-Cost-Simulation-Report.png "Şekil 9 - iş yükü maliyet karşılaştırma senaryo için örnek raporu"
[10]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/1_ReportWithTags.png "Şekil 10 - etiketleri kullanarak dökümleri raporla"
[11]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/2_ResourceGroupsWithTags.png "Şekil 11 - Azure portalında ilişkili etiketlerle kaynak grubu"
[12]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/3_ImportIntoUsageAPISheet.png "Şekil 12 - kullanım API'si verileri UsageAPI sayfasına alındı"
[13]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/4_NewTagField.png "Şekil 13 - etiket bilgileri için yeni alanlar oluşturma"
[14]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/5_PopulateAccountStructure.png "Şekil 14 - arama bilgileriyle hesap yapısı doldurma"
