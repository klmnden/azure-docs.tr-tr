---
title: Azure akış analizi Machine Learning işlevlerini ölçeklendirme
description: Bu makalede, bölümlendirme ve akış birimleri yapılandırarak Machine Learning işlevlerini kullanmak Stream Analytics işlerini ölçeklendirme açıklar.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: 015312ab95d6dd5615a5f5bc62d270d46b795ffa
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30909284"
---
# <a name="scale-your-stream-analytics-job-with-azure-machine-learning-functions"></a>Stream Analytics işiniz Azure Machine Learning işlevlerini ölçeklendirme
Bir akış analizi işi ayarlamak ve bazı örnek veriler üzerinden çalıştırmak için doğrudan ileriye doğru değil. Daha yüksek veri birimi ile aynı işi çalıştırmak gerektiğinde biz neler? Stream Analytics işi, ölçeklenebilir şekilde yapılandırmak nasıl anlamak için bize gerektirir. Bu belgede, şu özel akış analizi işleri Machine Learning işlevlerle ölçeklendirme yönlerini odaklanılmaktadır. Genel Stream Analytics işlerini ölçeklendirme hakkında bilgi için bkz: [işlerini ölçeklendirme](stream-analytics-scale-jobs.md).

## <a name="what-is-an-azure-machine-learning-function-in-stream-analytics"></a>Bir Azure Machine Learning işlevinde Stream Analytics nedir?
Stream Analytics Machine Learning işlevinde, Stream Analytics sorgu dili normal işlev çağrısında gibi kullanılabilir. Ancak, Sahne işlev çağrıları gerçekten Azure Machine Learning Web hizmeti isteklerinin aynıdır. Machine Learning web Hizmetleri "birden çok satır, genel üretilen işi artırmak için aynı web hizmeti API çağrısı içinde Mini toplu olarak adlandırılan yığınlama" destekler. Daha fazla ayrıntı için aşağıdaki makalelere bakın; [Stream Analytics azure Machine Learning işlevlerde](https://blogs.technet.microsoft.com/machinelearning/2015/12/10/azure-ml-now-available-as-a-function-in-azure-stream-analytics/) ve [Azure Machine Learning Web Hizmetleri](../machine-learning/studio/consume-web-services.md).

## <a name="configure-a-stream-analytics-job-with-machine-learning-functions"></a>Akış analizi işi Machine Learning işlevleri ile yapılandırma
Stream Analytics işi için Machine Learning işlevi yapılandırırken dikkate alınması gereken iki parametre, Machine Learning işlev çağrıları akış analizi işi için sağlanan akış birimleri (SUs) ve toplu iş boyutu vardır. Bunlar için uygun değerleri belirlemek için önce bir karar verimlilik, gecikme süresi arasında diğer bir deyişle, Stream Analytics işi ve her SU verimini gecikme yapılması gerekir. Ek SUs işi çalıştırma maliyetini artırır ancak SUs her zaman iyi bölümlenmiş bir akış analizi sorgu verimliliğini artırmak için işe eklenebilir.

Bu nedenle belirlemek önemli olan *dayanıklılık* gecikme Stream Analytics işi çalışır. Azure Machine Learning hizmet istekleri çalışmasını ek gecikme Stream Analytics işi gecikme compounds toplu iş boyutu ile doğal olarak artar. Diğer taraftan, toplu iş boyutu işlemek Stream Analytics işi artırır * ile daha fazla olay *aynı numarası* Machine Learning web hizmeti istekleri. Genellikle en ekonomik toplu iş boyutu verilen bir durumda bir Machine Learning web hizmeti için göz önünde bulundurmanız önemlidir Machine Learning web hizmeti gecikme süresi arttıkça toplu iş boyutunu artırmak için sublinear olur. Web hizmeti için varsayılan toplu iş boyutu istekleri 1000'dir ve kullanarak ya da değiştirilebilir [Stream Analytics REST API](https://msdn.microsoft.com/library/mt653706.aspx "Stream Analytics REST API") veya [akış için PowerShell istemcisi Analytics](stream-analytics-monitor-and-manage-jobs-use-powershell.md "akış analizi için PowerShell istemcisi").

Bir toplu iş boyutu belirlendikten sonra akış sayısı birimler (SUs) belirlendiği, işlevi gerektiğinde olayların sayısının temel saniye başına işlem. Akış birimleri hakkında daha fazla bilgi için bkz: [Stream Analytics işlerini ölçeklendirme](stream-analytics-scale-jobs.md).

Genel olarak, 1 SU işleri ve 3 SU işleri 20 eş zamanlı bağlantı da Al dışında her 6 SUs için Machine Learning web hizmetine 20 eş zamanlı bağlantı vardır.  Örneğin, saniye başına 200.000 etkinlik giriş verileri oranıdır ve toplu iş boyutu 1000 varsayılan olarak sol 1000 olayları Mini toplu elde edilen web hizmeti gecikmeyle 200 ms olur. Başka bir deyişle, her bağlantı beş Machine Learning web hizmetine bir saniyede isteğinde bulunabilir. 20 bağlantıları ile Stream Analytics işi 20.000 olayları 200 MS ve bu nedenle 100.000 olayları bir saniyede işleyebilir. Bu nedenle saniyede 200.000 olayları işlemek için Stream Analytics işi 12 SUs gelen 40 eşzamanlı bağlantı gerekir. Stream Analytics işi gelen istekleri Machine Learning web hizmeti uç noktası için aşağıdaki diyagramda gösterilmektedir – her 6 SUs sırasında en fazla 20 eş zamanlı bağlantı Machine Learning web hizmetine sahip.

![Machine Learning işlevleri iki iş örnekle Stream Analytics ölçeklendirme](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-00.png "Machine Learning işlevleri iki iş örnek ile ölçek akış analizi")

Genel olarak, ***B*** toplu iş boyutu için ***L*** web hizmeti gecikme süresi için milisaniye cinsinden toplu iş boyutu B, Stream Analytics verimini işi ile ***N*** SUs değil:

![Ölçeklendirme Machine Learning işlevleri formülü olan Stream Analytics](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-02.png "ölçeklendirme Machine Learning işlevleri formülü olan akış analizi")

Bir sağlayabildiği Machine Learning web hizmeti tarafında 'en fazla eşzamanlı çağrıları' olabilir, bu en büyük değeri (şu anda 200) ayarlamak için önerilen.

Bu ayar hakkında daha fazla bilgi için lütfen gözden [Machine Learning Web Hizmetleri için ölçeklendirme makalesine](../machine-learning/studio/scaling-webservice.md).

## <a name="example--sentiment-analysis"></a>Örnek – düşünceleri çözümleme
Aşağıdaki örnek, Stream Analytics işi düşünceleri analiz Machine Learning işlevi ile açıklandığı gibi içerir [Stream Analytics Machine Learning tümleştirme Öğreticisi](stream-analytics-machine-learning-integration-tutorial.md).

Basit bir sorgu ve ardından tam olarak bölümlenmiş sorgudur **düşünceleri** gösterilen aşağıdaki gibi işlev:

    WITH subquery AS (
        SELECT text, sentiment(text) as result from input
    )

    Select text, result.[Score]
    Into output
    From subquery

Aşağıdaki senaryoyu ele alalım; saniye başına 10.000 tweet'leri verimi ile akış analizi işi (olayları) tweet'leri düşünceleri analizini gerçekleştirmek için oluşturulmuş olması gerekir. 1 SU kullanarak, bu akış analizi işi trafiği kaldırabilir olabilir? 1000 varsayılan toplu iş boyutu kullanarak iş girişle tutmanız gerekir. Daha fazla eklenen Machine Learning işlevi genel olan gecikme saniyesi birden fazla varsayılan gecikmeyle düşünceleri analiz Machine Learning web hizmeti (varsayılan toplu iş boyutu 1000) oluşturmanız gerekir. Stream Analytics iş **genel** veya uçtan uca gecikme süresi genellikle birkaç saniye olacaktır. Bu akış analizi işi, daha ayrıntılı göz *özellikle* Machine Learning işlev çağrıları. Toplu iş boyutu 1000 sahip olmak, 10.000 olayları verimini ele yaklaşık 10 istekleri için web hizmeti. 1 SU ile bile, bu giriş trafiği karşılamak için yeterli eşzamanlı bağlantı vardır.

Ancak ne 100 x giriş olay hızı artırır ve Stream Analytics işi 1.000.000 tweet'leri saniye başına işlem gerekiyor? İki seçenek vardır:

1. Toplu iş boyutunu artırın veya
2. Bölüm paralel olayları işlemek için giriş akışı

İlk seçenek, iş **gecikme** artırır.

İkinci seçeneğiyle sağlanması ve bu nedenle daha fazla eşzamanlı Machine Learning web hizmeti isteklerinin oluşturmak daha fazla SUs gerekir. Bu iş anlamına gelir **maliyet** artırır.

Düşünceleri analiz Machine Learning web hizmeti gecikmesi 200 ms 1000 olay toplu işlemler için veya aşağıda olduğu 5.000 olay toplu işlemler için 250 ms, 300 ms 10.000 olay toplu işlemler için ya da 25.000 olay toplu işlemler için 500 ms varsayalım.

1. İlk seçeneği kullanılarak (**değil** daha fazla SUs sağlama), toplu iş boyutu için artırılacak **25.000**. Bu sırayla (ile çağrı başına 500 ms gecikme) Machine Learning web hizmeti 20 eş zamanlı bağlantılarla 1.000.000 olayları işlemek üzere işlemini olanak tanır. Machine Learning web hizmeti isteklerinin düşünceleri işlevi istekler nedeniyle Stream Analytics işi ek gecikmesi artan böylece **200 ms** için **500 ms**. Ancak, boyutu toplu **olamaz** sonsuz bir istek yükü boyutu 4 MB olması Machine Learning web hizmetleri gerektirdiği şekilde ya da daha küçük artırılmasını web hizmeti işlemi 100 saniye sonra zaman aşımı ister.
2. İkinci seçeneği kullanarak, toplu iş boyutu 200 ms web hizmeti gecikmeyle 1000 sol, web hizmetine her 20 eş zamanlı bağlantı olayları işlemek 1000 * 20 * 5 gerçekleştirebilir = saniye başına 100.000. Bu nedenle saniye başına 1.000.000 olayları işlemek için iş 60 SUs gerekir. İlk seçeneği ile karşılaştırıldığında, Stream Analytics işi sırayla artan bir maliyet oluşturmak daha fazla web hizmeti toplu isteği olmaması anlamına gelir.

Stream Analytics işi işleme için bir tablo farklı SUs ve toplu boyutları (saniye başına etkinlik sayısı) için aşağıdadır.

| Toplu iş boyutu (ML gecikme) | 500 (200 ms) | 1,000 (200 ms) | 5,000 (250 ms) | 10,000 (300 ms) | 25,000 (500 ms) |
| --- | --- | --- | --- | --- | --- |
| **1 SU** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **3 SUs** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **6 SUs** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **12 SUs** |5.000 |10,000 |40,000 |60,000 |100,000 |
| **18 SUs** |7.500 |15,000 |60,000 |90,000 |150,000 |
| **24 SUs** |10,000 |20,000 |80,000 |120,000 |200,000 |
| **…** |… |… |… |… |… |
| **60 SUs** |25,000 |50,000 |200,000 |300,000 |500,000 |

Şimdi tarafından zaten nasıl akış analizi, Machine Learning işlevleri işe iyi anlamış olmanız. Büyük olasılıkla de veri kaynaklarından veri akış analizi işleri "çekmek" ve her "çekme" olayları işlemek Stream Analytics işi için bir dizi döndürür anlamanız. Bu çekme modeli etkiyi Machine Learning hizmet isteklerinin nasıl web?

Normalde, Machine Learning işlevleri için ayarlarız toplu iş boyutu tam olarak her akış analizi işi "çekme" tarafından döndürülen olay sayısı bölünebilir olmayacaktır. Ne zaman Machine Learning web hizmeti ile "kısmi" toplu olarak adlandırılan gerçekleşir. Bu, ek iş gecikme birleştirmesi olaylarda çekme çekme zahmetine değil için gerçekleştirilir.

## <a name="new-function-related-monitoring-metrics"></a>İşlevi ile ilgili yeni izleme ölçümleri
Akış analizi işi İzleyicisi alanında üç ek işlevi ödemeyle ilgili ölçümlerini eklenmiştir. İŞLEV İSTEKLERİ, işlevi olayları ve başarısız işlev İSTEKLER grafikte gösterildiği gibi olduklarını.

![Ölçeklendirme akış analizi Machine Learning işlevleri ölçümlerle](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-01.png "ölçeklendirme Machine Learning işlevleri ölçümlerle akış analizi")

Yapılan tanımı aşağıdaki gibidir:

**İŞLEV İSTEKLERİ**: işlevi istek sayısı.

**İŞLEV olayları**: işlevi istekleri sayı olayları.

**BAŞARISIZ işlev İSTEKLER**: başarısız işleve istek sayısı.

## <a name="key-takeaways"></a>Anahtar paketler
Stream Analytics işi Machine Learning işlevlerle ölçeklendirmek için ana noktalarını özetlemek için aşağıdaki öğeleri dikkate alınmalıdır:

1. Giriş olay hızı
2. Çalışan bir akış analizi işi (ve dolayısıyla Machine Learning web hizmeti isteklerinin toplu iş boyutu) için toleranslı gecikme süresi
3. Sağlanan akış analizi SUs ve Machine Learning web hizmeti isteklerinin (ek işlevi ilişkili maliyetler) sayısı

Tam olarak bölümlenmiş bir akış analizi sorgu örnek olarak kullanılmıştır. Daha karmaşık bir sorgu gerekirse [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) Stream Analytics ekibinden Ek Yardım almak için mükemmel bir kaynaktır.

## <a name="next-steps"></a>Sonraki adımlar
Stream Analytics hakkında daha fazla bilgi için bkz:

* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
