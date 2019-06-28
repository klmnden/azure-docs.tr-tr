---
title: Azure Stream Analytics Machine Learning işlevleri ölçeği
description: Bu makalede, bölümlendirme ve akış birimlerinin yapılandırarak Machine Learning işlevlerini kullanan bir Stream Analytics işlerini ölçeklendirme açıklar.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: 5da09d705246ffd5002a1a21daab2266525f579e
ms.sourcegitcommit: a7ea412ca4411fc28431cbe7d2cc399900267585
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2019
ms.locfileid: "67357505"
---
# <a name="scale-your-stream-analytics-job-with-azure-machine-learning-studio-functions"></a>Stream Analytics işinizi Azure Machine Learning Studio işlevleriyle ölçeklendirme

Bu makalede, verimli bir şekilde Azure Machine Learning işlevlerini kullanan Azure Stream Analytics işlerini ölçeklendirme anlatılmaktadır. Genel olarak Stream Analytics işlerini ölçeklendirme hakkında bilgi için bkz [işleri ölçeklendirme](stream-analytics-scale-jobs.md).

## <a name="what-is-an-azure-machine-learning-function-in-stream-analytics"></a>Bir Azure Machine Learning işlevi Stream Analytics nedir?

Bir Machine Learning işlevi Stream analytics'te bir normal işlev çağrısı Stream Analytics sorgu dili gibi kullanılabilir. Arka planda, ancak gerçekte Azure Machine Learning Web hizmeti istekleri bu işlev çağrıları aynıdır.

"Birden çok satırı birlikte aynı web hizmeti API çağrısı toplu olarak" Machine Learning web hizmeti isteklerinin aktarım hızını artırabilir. Bu gruplandırma, kısa bir toplu iş olarak adlandırılır. Daha fazla bilgi için [Azure Machine Learning Studio Web Hizmetleri](../machine-learning/studio/consume-web-services.md). Azure Machine Learning Studio'da, Stream Analytics desteği Önizleme aşamasındadır.

## <a name="configure-a-stream-analytics-job-with-machine-learning-functions"></a>Machine Learning işlevleri ile bir Stream Analytics işi yapılandırma

Stream Analytics işinizi tarafından kullanılan Machine Learning işlevi yapılandırmak için iki parametre vardır:

* Machine Learning işlev çağrılarının toplu iş boyutu.
* Akış Stream Analytics işi için sağlanan birimleri (su) sayısı.

SUs için uygun değerleri belirlemek için Stream Analytics işi, gecikme süresi veya her SU, aktarım hızını en iyi duruma getirmek isteyip karar verin. SUs her zaman iyi bölümlenmiş bir Stream Analytics sorgusunun verimliliğini artırmak için işe eklenebilir. Ek SUs iş çalıştırma maliyetini artırır.

Gecikme süresini belirleme *dayanıklılık* Stream Analytics işiniz için. Toplu iş boyutu artırmayı, Azure Machine Learning hizmet isteklerinizi gecikme süresini ve Stream Analytics işi gecikme süresini artırır.

Toplu iş boyutu artırır işlemek Stream Analytics işi **daha fazla olay** ile **aynı sayıda** Machine Learning web hizmeti istekleri. Toplu iş boyutu sayısında artış için genellikle sublinear Machine Learning web hizmeti gecikme artıştır. 

En ekonomik bir toplu iş boyutu herhangi belirli durumda bir Machine Learning web hizmeti için göz önünde bulundurmanız önemlidir. Web hizmeti istekleri için varsayılan toplu iş boyutu 1000'dir. Bu varsayılan boyutu kullanarak değiştirebileceğiniz [Stream Analytics REST API](https://docs.microsoft.com/previous-versions/azure/mt653706(v=azure.100) "Stream Analytics REST API") veya [Stream Analytics için PowerShell istemcisi](stream-analytics-monitor-and-manage-jobs-use-powershell.md).

Bir toplu iş boyutu üzerinde verdikten işlevi gereken olayların sayısına dayalı olarak akış birimleri (su) sayısını ayarlayabilirsiniz saniye başına işlem. Akış birimleri hakkında daha fazla bilgi için bkz. [Stream Analytics işlerini ölçeklendirmeyi](stream-analytics-scale-jobs.md).

Her 6 SUs Machine Learning web hizmetine 20 eş zamanlı bağlantı alın. Ancak, SU iş 1 ve 3 SU işleri 20 eş zamanlı bağlantı alın.  

Saniye başına 200.000 olayları uygulamanızı oluşturur ve toplu iş boyutu 1000'dir, sonuçta elde edilen web hizmeti gecikmesi 200 ms olur. Bu oran, her bağlantı saniyede Machine Learning web hizmetine beş istekleri yapabileceğiniz anlamına gelir. 20 bağlantılarla, Stream Analytics işi bir saniye içinde 20.000 olayları 200 MS ve 100.000 olayları işleyebilir.

Saniye başına 200.000 olayları işlemek için Stream Analytics işi için 12 SUs gelen 40 eş zamanlı bağlantı gerekir. Aşağıdaki diyagramda Stream Analytics işi gelen istekleri Machine Learning web hizmeti uç noktası için gösterir: her 6 SUs en fazla 20 eş zamanlı bağlantı Machine Learning web hizmetine sahip.

![Stream Analytics ile Machine Learning işlevleri işi iki örnek ölçeklendirme](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-00.png "ölçek Stream Analytics, Machine Learning işlevleri işi iki örnek")

Genel olarak, ***B*** toplu iş boyutu için ***L*** web hizmeti düşük gecikme süresi için milisaniye cinsinden toplu iş boyutu B, aktarım hızı bir Stream Analytics işi ile ***N*** SUs olan:

![Stream Analytics, Machine Learning işlevleri formülle ölçeklendirme](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-02.png "Stream Analytics, Machine Learning işlevleri formül ile ölçeklendirme")

'En fazla eş zamanlı çağrılar' Machine Learning web hizmeti de yapılandırabilirsiniz. Bu parametre maksimum değerine (şu anda 200) ayarlamak için önerilir.

Bu ayar hakkında daha fazla bilgi için gözden [Machine Learning Web Hizmetleri için ölçeklendirme makale](../machine-learning/studio/scaling-webservice.md).

## <a name="example--sentiment-analysis"></a>Örneğin, yaklaşım analizi
Aşağıdaki örnekte açıklanan şekilde yaklaşım analizi, Machine Learning işlevi ile bir Stream Analytics işi içerir [Stream Analytics, Machine Learning tümleştirme Öğreticisi](stream-analytics-machine-learning-integration-tutorial.md).

Tam olarak bölümlenmiş ardından sorgu basit bir sorgudur **yaklaşım** aşağıdaki örnekte gösterildiği gibi işlev:

```SQL
    WITH subquery AS (
        SELECT text, sentiment(text) as result from input
    )

    Select text, result.[Score]
    Into output
    From subquery
```

Hangi tweetlerin 10.000 tweetlerin bir hızda mu yaklaşım analizi ikinci bir Stream Analytics işi oluşturmak için gerekli yapılandırmayı inceleyelim.

1 SU kullanarak, bu Stream Analytics işi ve trafiğin işlenmesi? İş, 1000 varsayılan toplu iş boyutu kullanarak giriş ile tutabilirsiniz. Yaklaşım analizi Machine Learning web hizmeti varsayılan gecikme süresini (varsayılan toplu iş boyutu 1000 ile) artık bir gecikme süresi saniyeden kısa oluşturur.

Stream Analytics iş **genel** veya uçtan uca gecikme süresi genellikle birkaç saniye olacaktır. Bu Stream Analytics işi, daha ayrıntılı göz *özellikle* Machine Learning işlevi çağırır. 1000 ile bir toplu iş boyutu, aktarım hızı 10.000 olay yaklaşık 10 istekleri web hizmetine alır. Hatta bir SU ile giriş trafiğe uyum sağlamak için yeterli sayıda eş zamanlı bağlantı vardır.

Giriş olayı Oranı 100 kat artarsa, Stream Analytics işi, saniye başına 1.000.000 tweetleri işlem gerekir. Artan ölçek gerçekleştirmek için iki seçenek vardır:

1. Toplu iş boyutunu artırın.
2. Bölüm paralel olayları işlemek için giriş akışı.

İlk seçenek, işi **gecikme** artırır.

İkinci seçenek, daha fazla eşzamanlı Machine Learning web hizmeti istekleri için daha fazla SUs sağlamanız gerekecektir. SUs, daha fazla bu sayıda işi artırır **maliyet**.

Ölçeklendirme aşağıdaki gecikmesi ölçümlerinin için her toplu iş boyutu kullanarak bakalım:

| Gecikme süresi | Toplu işlem boyutu |
| --- | --- |
| 200 ms | 1000 olay toplu veya aşağıdaki |
| 250 ms | 5\.000 olay toplu işlemler |
| 300 ms | 10.000 olay toplu işlemler |
| 500 ms | 25.000 olay toplu işlemler |

1. İlk seçeneği kullanılarak (**değil** daha fazla SUs sağlama). Toplu iş boyutu için artırılacak **25.000**. Bu şekilde toplu iş boyutu artırmayı (ile çağrı başına 500 ms gecikme) ile Machine Learning web hizmetine 20 eş zamanlı bağlantı 1.000.000 olayları işlemek iş izin verir. Stream Analytics işi Machine Learning web hizmeti isteklerinin yaklaşım işlevi istekler nedeniyle ek gecikme süresini yükseltilmesini böylece **200 ms** için **500 ms**. Bununla birlikte, boyutu toplu **olamaz** sonsuz bir istek yükü boyutu 4 MB olması Machine Learning web hizmetleri gerektirir veya daha küçük artırılması ve işlemin 100 saniye sonra web hizmeti isteği zaman aşımı.
1. İkinci seçeneği kullanarak, toplu iş boyutu 200 ms'lik bir web hizmeti gecikmeyle 1000'den, kaldı, web hizmetine yapılan her 20 eş zamanlı bağlantı olayları işlemeye 1000 * 20 * 5 seçebiliyor = saniye başına 100.000. Bu nedenle saniyede 1.000.000 olayları işlemek için iş 60 SUs gerekir. İlk seçeneği karşılaştırıldığında, Stream Analytics işi sırayla daha yüksek bir maliyet oluşturma daha fazla web hizmeti toplu istekleri hale getirir.

Stream Analytics işi, aktarım hızı için bir tablo farklı SUs ve batch boyutları (saniye başına olay sayısı) aşağıdadır.

| Toplu iş boyutu (ML gecikme) | 500 (200 ms) | 1\.000 (200 ms) | 5\.000 (250 ms) | 10.000 (300 ms) | 25.000 (500 ms) |
| --- | --- | --- | --- | --- | --- |
| **1 SU** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **3 SUs** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **6 SUs** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **12 SUs** |5,000 |10,000 |40,000 |60,000 |100,000 |
| **18 SUs** |7,500 |15,000 |60,000 |90,000 |150,000 |
| **24 SUs** |10,000 |20.000 |80,000 |120,000 |200,000 |
| **…** |… |… |… |… |… |
| **60 SUs** |25,000 |50,000 |200,000 |300,000 |500,000 |

Artık, zaten, Stream Analytics, Machine Learning işlevlerini nasıl çalıştığını iyi anlamış olmalıdır. Büyük olasılıkla ayrıca veri kaynaklarından alınan verileri Stream Analytics işleri "pull" ve "pull" her olayları işlemek Stream Analytics işine ilişkin toplu döndürür anlayın. Bu çekme modeli etkiyi Machine Learning, hizmet isteklerini nasıl web?

Normalde, Machine Learning işlevleri için ayarladığımız toplu iş boyutu tam olarak, her "pull" Stream Analytics işi tarafından döndürülen olay sayısı bölünebilir olmayacaktır. Machine Learning web hizmeti, böyle bir durumda, "kısmi" toplu işleri adı verilir. Kısmi toplu kullanarak, çekme isteği birleştirme olaylarında ek iş gecikme yükü yansıtılmasını önler.

## <a name="new-function-related-monitoring-metrics"></a>İşlevi ile ilgili yeni izleme ölçümleri
Bir Stream Analytics işi İzleyici alanında üç ek işlevi ile ilgili ölçüm eklendi. Bunlar **işlev İSTEKLERİ**, **işlev olayları** ve **başarısız işlev İSTEKLERİ**grafikte gösterildiği gibi.

![Stream Analytics, Machine Learning işlevleri ölçümlerle ölçeklendirme](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-01.png "Stream Analytics, Machine Learning işlevleri ölçümlerle ölçeklendirin")

Olduğunuz gibi tanımlanır:

**İŞLEV İSTEKLERİ**: İşlev istekleri sayısı.

**İŞLEV OLAYLARI**: İşlev istekleri olay sayısını.

**BAŞARISIZ İŞLEV İSTEKLERİ**: Başarısız işlev istekleri sayısı.

## <a name="key-takeaways"></a>Önemli dersler

Machine Learning işlevleri ile bir Stream Analytics işi ölçeklendirmek için aşağıdaki faktörleri göz önünde bulundurun:

1. Giriş olayı oranı.
2. Çalışan bir Stream Analytics işi (ve bu nedenle Machine Learning web hizmeti isteklerin toplu iş boyutu) toleranslı gecikme süresi.
3. Sağlanan Stream Analytics SUs ve Machine Learning web hizmeti isteklerinin (ek işlevi ilgili maliyetleri) sayısı.

Tam olarak bölümlenmiş bir Stream Analytics sorgusu, örnek olarak kullanılmıştır. Daha karmaşık bir sorgu gerekirse [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) Stream Analytics ekibinden Ek Yardım almak için harika bir kaynaktır.

## <a name="next-steps"></a>Sonraki adımlar
Stream Analytics hakkında daha fazla bilgi için bkz:

* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
