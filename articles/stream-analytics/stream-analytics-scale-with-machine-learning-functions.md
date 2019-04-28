---
title: Azure Stream Analytics Machine Learning işlevleri ölçeği
description: Bu makalede, bölümlendirme ve akış birimlerinin yapılandırarak Machine Learning işlevlerini kullanan bir Stream Analytics işlerini ölçeklendirme açıklar.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: f11034a4970e3fb95333310af82a6b2a2551f1eb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61479158"
---
# <a name="scale-your-stream-analytics-job-with-azure-machine-learning-functions"></a>Stream Analytics işinizi Azure Machine Learning işlevleriyle ölçeklendirme
Stream Analytics işi ayarlama ve bazı örnek veriler üzerinden çalıştırmak için oldukça kolaydır. Daha yüksek veri birimi ile aynı işi çalıştırmak gerektiğinde ne yapmamız gerekir? Stream Analytics işi, ölçeklendirir şekilde yapılandırmak nasıl anlamak bize gerektirir. Bu belgede, Machine Learning işlevleriyle birlikte Stream Analytics işlerini ölçeklendirme özel yönlerini odaklanıyoruz. Genel olarak Stream Analytics işlerini ölçeklendirme hakkında bilgi için bkz [işleri ölçeklendirme](stream-analytics-scale-jobs.md).

## <a name="what-is-an-azure-machine-learning-function-in-stream-analytics"></a>Bir Azure Machine Learning işlevi Stream Analytics nedir?
Bir Machine Learning işlevi Stream analytics'te bir normal işlev çağrısı Stream Analytics sorgu dili gibi kullanılabilir. Ancak, Sahne işlev çağrıları gerçekte Azure Machine Learning Web hizmeti isteği adı verilir. Machine Learning web Hizmetleri, "birden çok satır kısa batch genel performansını artırmak için aynı web hizmeti API çağrısında adlı toplu işleme" destekler. Daha fazla bilgi için [Azure Machine Learning Web Hizmetleri](../machine-learning/studio/consume-web-services.md).

## <a name="configure-a-stream-analytics-job-with-machine-learning-functions"></a>Machine Learning işlevleri ile bir Stream Analytics işi yapılandırma
Machine Learning işlevi Stream Analytics işine ilişkin yapılandırırken dikkate alınması gereken iki parametre, toplu iş boyutu olan Machine Learning işlev çağrıları ve Stream Analytics işi için sağlanan akış birimleri (su) vardır. SUs için uygun değerleri belirlemek için önce bir karar gecikme süresi ve aktarım hızı, diğer bir deyişle, Stream Analytics işi ve aktarım hızı her SU gecikme yapılması gerekir. Ek SUs iş çalıştırma maliyetini artırır ancak SUs her zaman iyi bölümlenmiş bir Stream Analytics sorgusunun verimliliğini artırmak için işe eklenebilir.

Bu nedenle belirlemek önemli olan *dayanıklılık* bir Stream Analytics işi çalıştıran, gecikme süresi. Azure Machine Learning hizmet istekleri çalışmasını ek gecikme, Stream Analytics işi gecikme süresini compounds toplu iş boyutu ile doğal olarak artar. Öte yandan, toplu iş boyutu işlemek Stream Analytics işi artırır * ile daha fazla olay *aynı sayıda* Machine Learning web hizmeti istekleri. En ekonomik bir toplu iş boyutu herhangi belirli durumda bir Machine Learning web hizmeti için önemlidir bu nedenle genellikle Machine Learning web hizmeti gecikme için toplu iş boyutu artış sublinear artıştır. Web hizmeti için varsayılan toplu iş boyutu 1000'dir ve kullanılarak değiştirilebilir istekleri [Stream Analytics REST API](https://msdn.microsoft.com/library/mt653706.aspx "Stream Analytics REST API") veya [Stream için PowerShell istemcisi Analytics](stream-analytics-monitor-and-manage-jobs-use-powershell.md "Stream Analytics için PowerShell istemcisi").

Bir toplu iş boyutu belirlendikten sonra akış sayısı birimleri (su) belirlenen, işlev gereken olayları üzerinden saniye başına işlem. Akış birimleri hakkında daha fazla bilgi için bkz. [Stream Analytics işlerini ölçeklendirmeyi](stream-analytics-scale-jobs.md).

Genel olarak, bir SU işleri ve 3 SU işleri 20 eş zamanlı bağlantı ayrıca Al dışında 20 eşzamanlı bağlantı her 6 SUs için Machine Learning web hizmeti vardır.  Örneğin, giriş veri hızı saniye başına 200.000 olayları ve toplu iş boyutu 1000 varsayılan bırakılır ortaya çıkan web hizmeti gecikme 1000 olayları Mini batch ile 200 ms olur. Başka bir deyişle, her bağlantı beş istek Machine Learning web hizmetine bir saniye içinde yapabilirsiniz. 20 bağlantılarla, Stream Analytics işi bir saniye içinde 20.000 olayları 200 MS ve bu nedenle, 100.000 olayları işleyebilir. Bu nedenle saniye başına 200.000 olayları işlemek için Stream Analytics işi için 12 SUs gelen 40 eş zamanlı bağlantı gerekir. Aşağıdaki diyagramda Stream Analytics işi gelen istekleri Machine Learning web hizmeti uç noktası için gösterir: her 6 SUs en fazla 20 eş zamanlı bağlantı Machine Learning web hizmetine sahip.

![Stream Analytics ile Machine Learning işlevleri işi iki örnek ölçeklendirme](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-00.png "ölçek Stream Analytics, Machine Learning işlevleri işi iki örnek")

Genel olarak, ***B*** toplu iş boyutu için ***L*** web hizmeti düşük gecikme süresi için milisaniye cinsinden toplu iş boyutu B, aktarım hızı bir Stream Analytics işi ile ***N*** SUs olan:

![Stream Analytics, Machine Learning işlevleri formülle ölçeklendirme](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-02.png "Stream Analytics, Machine Learning işlevleri formül ile ölçeklendirme")

Ek bir husus Machine Learning web hizmeti tarafta 'en fazla eş zamanlı çağrılar' olabilir, bu maksimum değeri (şu anda 200) ayarlamak için önerilir.

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
Aşağıdaki senaryoyu göz önünde bulundurun; saniye başına 10.000 tweetlerin bir aktarım hızı ile bir Stream Analytics işi tweetleri (olaylar), yaklaşım analizi gerçekleştirmek için yeniden oluşturulması gerekir. 1 SU kullanarak, bu Stream Analytics işi trafiği işleyebilir olabilir? 1000 varsayılan toplu iş boyutu kullanarak iş girişle ayak olmalıdır. Daha fazla eklenen Machine Learning işlevi en fazla bir genel olan gecikme saniyesi varsayılan gecikmeyle yaklaşım analizi Machine Learning web hizmeti (varsayılan toplu iş boyutu 1000) oluşturmanız gerekir. Stream Analytics iş **genel** veya uçtan uca gecikme süresi genellikle birkaç saniye olacaktır. Bu Stream Analytics işi, daha ayrıntılı göz *özellikle* Machine Learning işlevi çağırır. Toplu iş boyutu 1000 olan, aktarım hızı 10.000 olay ele yaklaşık 10 istekleri web hizmeti. Hatta bir SU ile giriş trafiğe uyum sağlamak için yeterli sayıda eş zamanlı bağlantı vardır.

Giriş olayı Oranı 100 kat artarsa, Stream Analytics işi, saniye başına 1.000.000 tweetleri işlem gerekir. Artan ölçek gerçekleştirmek için iki seçenek vardır:

1. Toplu iş boyutunu artırın veya
2. Bölüm paralel olayları işlemek için giriş akışı

İlk seçenek, işi **gecikme** artırır.

İkinci seçenek sağlanması ve bu nedenle daha fazla eşzamanlı Machine Learning web hizmeti istekleri oluşturmak daha fazla SUs gerekir. Bu iş anlamına gelir **maliyet** artırır.

Machine Learning web hizmetini yaklaşım analizi gecikme süresini 200 ms 1000 olay toplu işlemler için veya aşağıda olduğu 5.000 olay toplu işlemler için 250 ms, 300 ms 10.000 olay toplu işlemler için ya da 500 ms 25.000 olay toplu işlemler için kabul edilmektedir.

1. İlk seçeneği kullanılarak (**değil** daha fazla SUs sağlama). Toplu iş boyutu için artırılacak **25.000**. Bu sırayla (ile çağrı başına 500 ms gecikme) ile Machine Learning web hizmetine 20 eş zamanlı bağlantı 1.000.000 olayları işlemek iş çalıştırmasına olanak tanır. Stream Analytics işi Machine Learning web hizmeti isteklerinin yaklaşım işlevi istekler nedeniyle ek gecikme süresini yükseltilmesini böylece **200 ms** için **500 ms**. Bununla birlikte, boyutu toplu **olamaz** sonsuz bir istek yükü boyutu 4 MB olması Machine Learning web hizmetleri gerektirir veya daha küçük artırılmasını web hizmeti işlemi 100 saniye sonra zaman aşımı ister.
2. İkinci seçeneği kullanarak, toplu iş boyutu 200 ms'lik bir web hizmeti gecikmeyle 1000'den, kaldı, web hizmetine yapılan her 20 eş zamanlı bağlantı olayları işlemeye 1000 * 20 * 5 seçebiliyor = saniye başına 100.000. Bu nedenle saniyede 1.000.000 olayları işlemek için iş 60 SUs gerekir. İlk seçeneği karşılaştırıldığında, Stream Analytics işi sırayla daha yüksek bir maliyet oluşturma daha fazla web hizmeti toplu istekleri hale getirir.

Stream Analytics işi, aktarım hızı için bir tablo farklı SUs ve batch boyutları (saniye başına olay sayısı) aşağıdadır.

| Toplu iş boyutu (ML gecikme) | 500 (200 ms) | 1.000 (200 ms) | 5.000 (250 ms) | 10.000 (300 ms) | 25.000 (500 ms) |
| --- | --- | --- | --- | --- | --- |
| **1 SU** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **3 SUs** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **6 SUs** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **12 SUs** |5.000 |10,000 |40,000 |60,000 |100.000 |
| **18 SUs** |7.500 |15,000 |60,000 |90,000 |150,000 |
| **24 SUs** |10,000 |20.000 |80,000 |120,000 |200,000 |
| **…** |… |… |… |… |… |
| **60 SUs** |25,000 |50,000 |200,000 |300,000 |500,000 |

Artık, zaten, Stream Analytics, Machine Learning işlevlerini nasıl çalıştığını iyi anlamış olmalıdır. Büyük olasılıkla ayrıca veri kaynaklarından alınan verileri Stream Analytics işleri "pull" ve "pull" her olayları işlemek Stream Analytics işine ilişkin toplu döndürür anlayın. Bu çekme modeli etkiyi Machine Learning, hizmet isteklerini nasıl web?

Normalde, Machine Learning işlevleri için ayarladığımız toplu iş boyutu tam olarak, her "pull" Stream Analytics işi tarafından döndürülen olay sayısı bölünebilir olmayacaktır. Machine Learning web hizmeti, böyle bir durumda, "kısmi" toplu işleri adı verilir. Bu çekme isteği birleştirme olaylarında ek iş gecikme yükünü artırmak değil gerçekleştirilir.

## <a name="new-function-related-monitoring-metrics"></a>İşlevi ile ilgili yeni izleme ölçümleri
Bir Stream Analytics işi İzleyici alanında üç ek işlevi ile ilgili ölçüm eklendi. İŞLEV İSTEKLERİ, işlev olayları ve başarısız işlev İSTEKLERİ grafikte gösterildiği gibi değildirler.

![Stream Analytics, Machine Learning işlevleri ölçümlerle ölçeklendirme](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-01.png "Stream Analytics, Machine Learning işlevleri ölçümlerle ölçeklendirin")

Olduğunuz gibi tanımlanır:

**İŞLEV İSTEKLERİ**: İşlev istekleri sayısı.

**İŞLEV OLAYLARI**: İşlev istekleri olay sayısını.

**BAŞARISIZ İŞLEV İSTEKLERİ**: Başarısız işlev istekleri sayısı.

## <a name="key-takeaways"></a>Önemli dersler
Machine Learning işlevleri ile bir Stream Analytics işi ölçeklendirmek için ana noktaları özetlemek gerekirse, aşağıdakiler dikkate alınmalıdır:

1. Giriş olayı oranı
2. Çalışan bir Stream Analytics işi (ve bu nedenle Machine Learning web hizmeti isteklerin toplu iş boyutu) toleranslı gecikme süresi
3. Sağlanan Stream Analytics SUs ve Machine Learning web hizmeti isteklerinin (ek işlevi ilgili maliyetleri) sayısı

Tam olarak bölümlenmiş bir Stream Analytics sorgusu, örnek olarak kullanılmıştır. Daha karmaşık bir sorgu gerekirse [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) Stream Analytics ekibinden Ek Yardım almak için harika bir kaynaktır.

## <a name="next-steps"></a>Sonraki adımlar
Stream Analytics hakkında daha fazla bilgi için bkz:

* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
