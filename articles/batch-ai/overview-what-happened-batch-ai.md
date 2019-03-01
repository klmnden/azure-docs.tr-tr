---
title: Azure Batch AI neler oluyor? | Microsoft Docs
description: Azure Batch AI ve Azure Machine Learning hizmetinin işlem seçeneği için neler olduğu hakkında bilgi edinin.
ms.service: batch-ai
services: batch-ai
ms.topic: overview
ms.author: jmartens
author: j-martens
ms.date: 2/28/2019
ms.openlocfilehash: edd6a7e5f385d766d51631d77f5889233af2469a
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57194511"
---
# <a name="whats-happening-to-azure-batch-ai"></a>Azure Batch AI neler oluyor?

**Azure Batch AI hizmeti Mart ayında devre dışı bırakılıyor.** Ölçekli eğitim ve puanlama Batch AI yeteneklerini de kullanıma sunulmuştur [Azure Machine Learning hizmeti](../machine-learning/service/overview-what-is-azure-ml.md), hangi genel olarak kullanılabilir hale 4 Aralık 2018'de.

Makine öğrenimi özellikleri birçok diğer yanı sıra, Azure Machine Learning hizmeti, eğitim, dağıtma ve makine öğrenimi modellerini Puanlama için yönetilen bir bulut tabanlı bilgi işlem hedef içerir. Bu işlem hedef adlı [Azure Machine Learning işlem](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute). [Geçiş ve bugün kullanmaya başlayın](#migrate). Azure Machine Learning hizmeti aracılığıyla etkileşim kurabilir, [Python SDK'ları](../machine-learning/service/quickstart-create-workspace-with-python.md), komut satırı arabirimi ve [Azure portalında](../machine-learning/service/quickstart-get-started.md).

## <a name="support-timeline"></a>Destek zaman çizelgesi

Şu anda, önce mevcut Azure Batch AI abonelikler olarak kullanabilirsiniz. Ancak, hiçbir **yeni abonelikler** mümkün olan ve hiçbir yeni yatırım yapılan.

Mart başlangıç&nbsp;31&#x2c;&nbsp;2019'da, Batch AI abonelikleri kullanılmayan artık çalışmayacaktır.

## <a name="compare-to-azure-machine-learning"></a>Azure Machine Learning ile Karşılaştır
Bu eğitim, dağıtma, otomatikleştirin ve tüm bulut sunuyor Geniş ölçekte makine öğrenimi modelleri yönetmek için kullandığınız bir bulut hizmetidir. Bir üst düzey anlayın [bu genel bakışta Azure Machine Learning hizmetinde](../machine-learning/service/overview-what-is-azure-ml.md).
 

Veri hazırlama, eğitim ve deneme ve dağıtım aşaması tipik model geliştirme yaşam döngüsünü kapsar. Machine Learning işlem hatlarını kullanarak bu uçtan uca döngüsü düzenlenmiş.

![Akış Diyagramı](./media/overview-what-happened-batch-ai/lifecycle.png)


[Hizmetinin nasıl çalıştığı ve ana kavramları hakkında daha fazla bilgi](../machine-learning/service/concept-azure-machine-learning-architecture.md). Batch AI mevcut kavramları model eğitim iş akışında kavramlardan bir çoğunu benzerdir. 

Özellikle, bunlar hakkında nasıl almalısınız, eşleme şu şekildedir:
 
|Batch AI hizmeti|  Azure Machine Learning hizmeti|
|:--:|:---:|
|Çalışma alanı|Çalışma alanı|
|Küme|   İşlem türü `AmlCompute`|
|Dosya sunucuları|Veri depoları|
|Denemeler|Denemeler|
|İşler|Çalıştıran (iç içe geçmiş çalıştırmaları izin verir)|
 
Daha fazla şey görselleştirmenize yardımcı olacak aynı tablosunun başka bir görünümü aşağıda verilmiştir:
 
### <a name="batch-ai-hierarchy"></a>Batch AI hiyerarşisi
![Akış Diyagramı](./media/overview-what-happened-batch-ai/batchai-heirarchy.png) 
 
### <a name="azure-machine-learning-service-hierarchy"></a>Azure Machine Learning hizmeti hiyerarşisi
![Akış Diyagramı](./media/overview-what-happened-batch-ai/azure-machine-learning-service-heirarchy.png) 

## <a name="platform-capabilities"></a>Platform özellikleri
Azure Machine Learning hizmeti harika bir uçtan uca eğitim dahil olmak üzere yeni işlevler ilişin -> Azure kaynaklarını yönetmek zorunda kalmadan, yapay ZEKA geliştirme için kullanabileceğiniz dağıtım yığını. Bu tablo iki hizmet arasında eğitim özellik desteği karşılaştırır.

|Özellik|Batch AI hizmeti|Azure Machine Learning hizmeti|
|-------|:-------:|:-------:|
|VM boyutu seçeneği |CPU/GPU    |CPU/GPU. Ayrıca FPGA için çıkarım destekler|
|AI hazır kümesi (sürücüleri, Docker, vb.)|  Evet |Evet|
|Düğüm hazırlığı| Evet|    Hayır|
|İşletim sistemi ailesi seçim   |Kısmi    |Hayır|
|Ayrılmış ve LowPriority VM'ler  |Evet    |Evet|
|Otomatik Ölçeklendirme   |Evet    |Evet (varsayılan)|
|Bekleme süresi için otomatik ölçeklendirme  |Hayır |Evet|
|SSH    |Evet|   Evet|
|Küme düzeyi bağlama |Evet (FileShares, Bloblar, NFS, özel)   |Evet (bağlayın veya kendi veri deposu indirin)|
|Dağıtılmış eğitimi|  Evet |Evet|
|İş yürütme modu|    VM veya kapsayıcı|    Kapsayıcı|
|Özel bir kapsayıcı görüntüsü|    Evet |Evet|
|Herhangi bir araç seti    |Evet    |Evet (çalışma Python betiğini)|
|JobPreparation|    Evet |Henüz değil|
|İş düzeyi bağlama |Evet (FileShares, Bloblar, NFS, özel)   |Evet (FileShares, BLOB'ları)|
|İş izleme     |GetJob|    Çalıştırma geçmişi (daha zengin bilgi, daha fazla ölçüm göndermek için özel çalışma zamanı)|
|İş günlükleri ve dosyaları/modelleri alma |   ListFiles ve depolama API'leri  |Yapı Hizmeti|
 |Tensorboard desteği   |Hayır|    Evet|
|VM ailesi düzeyi kotaları |Evet    |(İlet taşınan Evet önceki kapasite ile)|
 
Yukarıdaki tabloda ek olarak, Azure Machine Learning hizmetinde BatchAI içinde desteklenen geleneksel değil özellikler mevcuttur.

|Özellik|Batch AI hizmeti|Azure Machine Learning hizmeti|
|-------|:-------:|:-------:|
|Ortamı hazırlama    |Hayır |Evet (Conda hazırlamak ve ACR yükleme)|
|Hiper parametre ayarı  |Hayır|    Evet|
|Model yönetimi   |Hayır |Evet|
|Kullanıma hazır hale getirme/dağıtım| Hayır  |ACI ve AKS|
|Veri hazırlama   |Hayır |Evet|
|Hedef işlem    |Azure VM’leri  |Yerel, BatchAI (olarak AmlCompute), DataBricks, HDInsight|
|Otomatik makine öğrenimi |Hayır|    Evet|
|İşlem hatları  |Hayır |Evet|
|Toplu Puanlama  |Evet    |Evet|
|Portal/CLI desteği|    Evet |Evet|


## <a name="programming-interfaces"></a>Programlama arabirimleri

Bu tabloda, çeşitli programlama arabirimlerinde için her bir hizmet sunar.
    
|Özellik|BatchAI hizmeti|Azure Machine Learning hizmeti|
|-------|:-------:|:-------:|
|SDK    |Java, C#, Python, Nodejs   |Python (temel çalıştırma yapılandırma ve tahmin ortak çerçeveleri için temel)|
|CLI    |Evet    |Henüz değil|
|Azure portal   |Evet    |Evet (iş gönderme dışında)|
|REST API   |Evet    |Mikro hizmetler arasında ancak dağıtılmış Evet|


GA'ed Azure Machine Learning hizmeti ile Önizleme Batch AI ' yükseltme Estimators ve veri depoları gibi kullanmak daha kolay olan kavramları aracılığıyla daha iyi bir deneyim sunar. Ayrıca Azure hizmet düzeyi SLA'lar genel kullanım ve müşteri desteği garanti eder.

Azure Machine Learning Hizmeti ayrıca yeni işlevler getirir, machine learning, Hiper parametre ayarı ve en büyük ölçekli yapay ZEKA iş yüklerini yararlı olan, ML işlem hatları otomatik. Eğitilen bir modelin ayrı bir hizmet arasında geçiş yapmadan dağıtma yeteneği, verilerin hazırlanmasından (Data Prep SDK'sını kullanarak) veri bilimi döngüyü tamamlamak tamamen kullanıma hazır hale getirme ve model izleme için yardımcı olur.

<a name="migrate"></a>
## <a name="migrate"></a>Geçiş

Nasıl geçirileceği ve Azure Machine Learning hizmetinde kodunda kullandığınız kod eşlemelerini nasıl bilgi [Azure Machine Learning hizmetine geçiş](how-to-migrate.md) makalesi.

## <a name="get-support"></a>Destek alın

Batch AI, 31 Mart devre dışı bırakmak için planlanmıştır ve desteği ile bir özel durum oluşturularak izin verilenler listesinde olmadığı sürece, hizmete kaydediliyor gelen yeni abonelikler zaten engelliyor.  Adresinden bize ulaşın [Azure Batch AI eğitimi önizlemesi](mailto:AzureBatchAITrainingPreview@service.microsoft.com) ile herhangi bir sorunuz veya Azure Machine Learning hizmeti ile geçiş yaparken geri bildirimde bulunmak istiyorsanız.

Azure Machine Learning hizmeti genel kullanıma sunulan bir hizmettir. Bu, taahhüt SLA'sı ve aralarından seçim yapabileceğiniz çeşitli destek planları ile geldiğini anlamına gelir.

Yalnızca her iki durumda yalnızca temel işlem fiyatı alırız olarak Azure altyapısı Batch AI hizmeti aracılığıyla veya Azure Machine Learning hizmeti ile kullanmak için fiyatlandırma, değiştirilmemelidir. Daha fazla bilgi için [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/details/machine-learning-service/).

Bölgesel kullanılabilirlik iki hizmet arasında görünümünde [Azure portalında](https://azure.microsoft.com/global-infrastructure/services/?products=batch-ai,machine-learning-service&regions=all).


## <a name="next-steps"></a>Sonraki adımlar

+ Bilgi [geçirme](how-to-migrate.md) ve Azure Machine Learning hizmetinde kodu nasıl kullandığınız artık kod eşlenir.

+ Okuma [Azure Machine Learning hizmetine genel bakış](../machine-learning/service/overview-what-is-azure-ml.md).

+ [Bir işlem hedefine modeli eğitimi için yapılandırma](../machine-learning/service/how-to-set-up-training-targets.md) Azure Machine Learning hizmeti ile.

+ Gözden geçirme [Azure yol haritası](https://azure.microsoft.com/updates/) diğer Azure hizmet güncelleştirmeleri öğrenin.
