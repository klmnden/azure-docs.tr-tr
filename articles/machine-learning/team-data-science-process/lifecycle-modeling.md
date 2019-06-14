---
title: Team Data Science Process yaşam döngüsü aşaması modelleme
description: Hedefleri, görevleri ve teslim edilebilirler için veri bilimi projelerinizi modelleme aşaması
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/04/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: c22c75b4fe900ecb96d016251c09e9ad6ec31f7c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60306233"
---
# <a name="modeling-stage-of-the-team-data-science-process-lifecycle"></a>Team Data Science Process yaşam döngüsü aşaması modelleme

Bu makalede, hedeflerinizi, görevleri ve teslim edilebilirler ile Team Data Science işlem (TDSP) modelleme aşama ilişkili özetlenmektedir. Bu işlem, veri bilimi projelerinizi yapısı için kullanabileceğiniz önerilen bir yaşam döngüsü sağlar. Yaşam döngüsü projeleri genellikle genellikle yinelemeli olarak yürütme, önemli aşamalar açıklanmaktadır:

   1. **İşin gereksinimlerini anlama**
   2. **Veri edinme ve anlama**
   3. **Modelleme**
   4. **Dağıtım**
   5. **Müşteri kabulü**

TDSP yaşam döngüsü görsel bir temsilini şu şekildedir:

![TDSP yaşam döngüsü](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Hedefleri
* Machine learning modelinin en iyi veri özelliklerini belirler.
* Hedef en doğru bir şekilde tahmin eden bilgilendirici bir makine öğrenimi modeli oluşturun.
* Üretim için uygun olan machine learning modeli oluşturun.

## <a name="how-to-do-it"></a>Nasıl yapılır
Bu aşamada yönelik üç ana görev vardır:

  * **özellik Mühendisliği**: Veri özellikleri, model eğitiminin kolaylaştırmak için ham verileri oluşturun.
  * **Eğitim modeli**: Kendi başarı ölçümlerini karşılaştırarak en doğru soruyu yanıtlar model bulun.
  * Modelinizi olup olmadığının **üretim için uygundur.**

### <a name="feature-engineering"></a>Özellik mühendisliği
Özellik Mühendisliği, ekleme, toplama ve analizde kullanılan özellikler oluşturmak için ham değişkenleri dönüştürmeyi içerir. Bir model önünü açıyor Öngörüler isterseniz, özellikleri birbirleriyle nasıl ilişki kuracağını ve makine öğrenme algoritmalarını söz konusu özellikleri kullanır şeklini anlamanız gerekir. 

Bu adım veri araştırma adımda elde edilen içgörüleri ve etki alanı uzmanlığı yaratıcı bir birleşimini gerektirir. Özellik Mühendisliği bir bulma ve bilgilendirici değişkenleri dahil, ancak aynı anda çok fazla ilgisiz değişkenleri önlemek çalışırken Dengeleme işidir. Bilgilendirici değişkenleri sonuç artırın; İlişkisiz değişkenleri gereksiz bir gürültü modele tanıtır. Ayrıca, Puanlama sırasında elde edilen tüm yeni veriler için bu özellikleri oluşturmak gerekir. Sonuç olarak, bu özelliklerin oluşturma yalnızca Puanlama sırasında kullanılabilir olan veri bağlı olabilir. 

Teknik rehberlik özelliği için ne zaman mühendislik olun çeşitli Azure veri teknolojilerini kullanmak [özellik Mühendisliği, veri bilimi işlemi](create-features.md). 

### <a name="model-training"></a>Modeli eğitimi
Yanıtlamaya çalıştığınız sorunun türüne bağlı olarak, kullanılabilir birçok modelleme algoritması vardır. Algoritmaları seçme ile ilgili yönergeler için bkz: [Microsoft Azure Machine Learning için algoritma seçme](../studio/algorithm-choice.md). Bu makalede, Azure Machine Learning kullansa da, herhangi bir makine öğrenimi projeleri için sağladığı Kılavuzu yararlıdır. 

İşlem modeli eğitimi için aşağıdaki adımları içerir: 

   * **Giriş verileri bölme** rastgele bir eğitim veri kümesi ve bir test veri kümesini modelleme için.
   * **Model derleme** eğitim veri kümesi kullanarak.
   * **Değerlendirme** eğitim ve sınama veri kümesi. Rakip makine öğrenme algoritmalarını ilişkili çeşitli ayar parametreleri birlikte bir dizi kullanın (olarak bilinen bir *parametre tarama*), sağlamıştır ilgilenilen geçerli verileri ile soru yanıtlama doğru.
   * **"En uygun" çözümü belirlemek** alternatif yöntemler arasında başarı ölçümlerini karşılaştırarak soruyu yanıtlamak için.

> [!NOTE]
> **Sızıntısını önlemek**: Bir model veya makine öğrenimi algoritması unrealistically iyi tahminler elde etmeye olanak sağlayan bir eğitim veri kümesi dışından verileri dahil ettiyseniz veri sızıntısına neden olabilir. Sızıntısına neden veri uzmanları, Tahmine dayalı sonuçları alın, tedirgin doğru olması abartılı yaygın bir nedenidir. Bu bağımlılıklar algılanması zor olabilir. Genellikle sızıntısını önlemek için bir analysis veri kümesi oluşturma, bir model oluşturma ve değerlendirme sonuçları doğruluğunu arasında yineleme gerektirir. 
> 
> 

Sağladığımız bir [modelleme ve raporlama aracıyla Otomatik](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/Modeling) birden çok algoritmaları ve parametre süpürmeleri temel modeli oluşturmak üzere çalıştırabilir TDSP ile. Ayrıca, değişken önem dahil olmak üzere her model ve parametre birleşimi performansını özetleyen rapor modelleme bir temel oluşturur. Bu işlem, ayrıca daha fazla özellik Mühendisliği yönlendirebilirsiniz gibi yinelemelidir. 

## <a name="artifacts"></a>Yapıtlar
Bu aşamada üretilen yapıtları içerir:

   * [Özellik kümeleri](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#feature-sets): Geliştirilen model için özelliklerin açıklanan **özellik kümeleri** bölümünü **veri tanımı** rapor. Bu özellikler ve özellik nasıl oluşturulduğunu tanımını oluşturmak için kod işaretçileri içerir.
   * [Rapor modeli](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Model/Model%201/Model%20Report.md): Denenen, her model için bir standart her deneme hakkında ayrıntılar sağlayan şablon tabanlı bir rapor oluşturulur.
   * **Denetim noktası karar**: Model, bir üretim sistemine dağıtmak için yeterince iyi gerçekleştirir olup olmadığını değerlendirin. Bazı temel sorular sormak için şunlardır:
     * Modeli belirtilen test veri yeterli güvenle sorusunu mu? 
     * Herhangi bir alternatif yaklaşımlar çalışmanız gerekir? Ek veri topla, daha fazla özellik Mühendisliği yapın veya diğer algoritmalar ile denemeler?

## <a name="next-steps"></a>Sonraki adımlar

TDSP yaşam döngüsü içinde her adım için bağlantılar şunlardır:

   1. [İşin gereksinimlerini anlama](lifecycle-business-understanding.md)
   2. [Veri edinme ve anlama](lifecycle-data.md)
   3. [Modelleme](lifecycle-modeling.md)
   4. [Dağıtım](lifecycle-deployment.md)
   5. [Müşteri kabulü](lifecycle-acceptance.md)

İşlemin belirli senaryolar için tüm adımları gösteren uçtan uca tam talimatlara sunuyoruz. [Örnek izlenecek yollar](walkthroughs.md) makale bağlantıları ve küçük resim açıklamaları senaryolarıyla bir listesini sağlar. İzlenecek bir iş akışı veya işlem hattı akıllı bir uygulama oluşturmak için bulut, şirket içi araçları ve Hizmetleri birleştirme işlemini göstermektedir. 

Adımlar Azure Machine Learning Studio'nun TDSPs yürütmek nasıl bir örnekleri için bkz: [Azure Machine Learning ile TDSP kullanma](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/). 
