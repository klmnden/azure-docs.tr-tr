---
title: "Takım veri bilimi işlemi yaşam döngüsü - Azure aşaması modelleme | Microsoft Docs"
description: "Hedefler, görevler ve veri bilimi projelerinizi modelleme aşaması için teslim edilebilir"
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: bradsev;
ms.openlocfilehash: a7fc5f2128e9c13182ca5bd7a6bd61ae41ef775c
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2017
---
# <a name="modeling"></a>Modelleme

Bu makalede hedefleri, görevler ve modelleme aşama takım veri bilimi işlem (TDSP) ile ilgili sonuçlara özetlenmektedir. Bu işlem, veri bilimi projelerinizi yapısı için kullanabileceğiniz önerilen bir yaşam döngüsü sunar. Yaşam döngüsü projeleri genellikle genellikle tekrarlayarak yürütme, önemli aşamaları ana hatlarıyla gösterilir:

   1. **İş anlama**
   2. **Veri alımı ve anlama**
   3. **Modelleme**
   4. **Dağıtım**
   5. **Müşteri kabulü**

Görsel bir TDSP yaşam döngüsü şöyledir:

![TDSP yaşam döngüsü](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Hedefleri
* Machine learning modelini için en iyi veri özellikleri belirler.
* Hedef en doğru tahmin bilgilendirici bir machine learning modeli oluşturun.
* Üretim için uygun bir machine learning modelini oluşturun.

## <a name="how-to-do-it"></a>Bunu nasıl
Bu aşamada ele üç ana görevleri şunlardır:

  * **Özellik Mühendisliği**: model eğitim kolaylaştırmak için ham verileri veri özellikleri oluşturun.
  * **Model Eğitimi**: kendi başarı ölçümlerini karşılaştırarak soru en doğru bir şekilde yanıtlar modeli bulunamadı.
  * Modelinizi olup olmadığını belirlemek **üretim için uygundur.**

### <a name="feature-engineering"></a>Özellik mühendisliği
Özellik Mühendisliği ekleme, toplama ve analiz kullanılan özellikler oluşturmak için ham değişkenleri dönüşümü içerir. Bir model yönlendiren içine Insight istiyorsanız, nasıl özellikleri birbirleri ile ve bu özellikleri kullanmak için machine learning algoritmaları şeklini anlamanız gerekir. 

Bu adım, etki alanı uzmanlık yaratıcı bir birleşimini ve veri araştırması adımda elde edilen Öngörüler gerektirir. Özellik Mühendisliği bir bulma ve bilgilendirici değişkenleri dahil, ancak aynı anda çok fazla ilgisiz değişkenler kullanmaktan kaçının çalışılırken Dengeleme işidir. Bilgilendirici değişkenleri Sonuç kümenizi artırmak; İlişkisiz değişkenleri gereksiz bir gürültü modeline tanıtır. Puanlama sırasında elde edilen yeni veriler için bu özellikleri oluşturmak gerekir. Sonuç olarak, bu özellikler nesil yalnızca Puanlama zamanında mevcut olan verileri bağlı. 

İçin teknik kılavuz özelliğini zaman mühendislik hale çeşitli Azure veri teknoloji kullanmak için bkz: [özellik Mühendisliği veri bilimi işleminde](create-features.md). 

### <a name="model-training"></a>Model eğitimi
Yanıtlamaya çalıştığınız sorunun türüne bağlı olarak, kullanılabilir birçok modelleme algoritması vardır. Algoritmalar seçme ile ilgili yönergeler için bkz: [için Microsoft Azure Machine Learning algoritmaları seçme](../studio/algorithm-choice.md). Bu makalede Azure Machine Learning kullansa da, tüm makine öğrenme projeleri için sağladığı Kılavuzu yararlıdır. 

İşlem modeli eğitim için aşağıdaki adımları içerir: 

   * **Giriş verisi bölme** rastgele bir eğitim veri kümesi ve bir sınama veri kümesi modelleme için.
   * **Modelleri oluşturabilir** eğitim veri kümesi kullanarak.
   * **Değerlendirme** eğitim ve sınama veri kümesi. Çeşitli ilişkili ayarlama parametrelerini birlikte rakip machine learning algoritmaları, bir dizi kullanın (olarak bilinen bir *parametresi tarama*), sağlamıştır geçerli verilerle ilgi soruyu yanıtlarken bulunun.
   * **"En iyi" çözümü belirlemenize** alternatif yöntemler arasında başarı ölçümlerini karşılaştırarak soruyu yanıtlamak için.

> [!NOTE]
> **Sızıntısını önlemek**: bir model veya unrealistically iyi tahminlerde için makine öğrenme algoritmasını sağlayan eğitim veri kümesi dışından verileri dahil ederseniz veri sızıntısını neden olabilir. Sızıntısını neden, Tahmine dayalı sonuçlar almak zaman bilimcilerine tedirgin veri doğru olması iyi göründüğü ortak bir nedeni. Bu bağımlılıklar algılamak zor olabilir. Genellikle sızıntısını önlemek için bir çözümleme veri kümesi oluşturma, bir model oluşturma ve sonuçları doğruluğunu değerlendirme arasında yineleme gerektirir. 
> 
> 

Sağladığımız bir [modelleme ve Raporlama Aracı otomatik](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/Modeling) birden çok algoritmaları ve parametre bir taban çizgisi modeli oluşturmak üzere dağılımlarında çalıştırabilir TDSP ile. Ayrıca, değişken önem dahil olmak üzere her modeli ve parametre birleşimi performansını özetler rapor modelleme bir temel oluşturur. Bu işlem ayrıca, daha fazla özellik Mühendisliği sürücüsü olarak yinelemelidir. 

## <a name="artifacts"></a>Yapıtlar
Bu aşamada üretilen yapılar içerir:

   * [Özellik kümeleri](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#feature-sets): modelleme için geliştirilen özellikleri açıklanan **özellik kümeleri** bölümünü **veri tanımı** rapor. Özellikler ve özellik nasıl oluşturulan açıklamasını oluşturmak için kodu işaretçiler içerir.
   * [Rapor modeli](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Model/Model%201/Model%20Report.md): denenir, her model için bir standart her deneme hakkında ayrıntılar sağlayan şablon tabanlı rapor üretilir.
   * **Denetim noktası karar**: model üretim sisteme dağıtmak için yeterince iyi gerçekleştiğini değerlendirmek. Sorulacak anahtar bazı sorular şunlardır:
     * Modeli belirtilen test veri yeterli güvenle soruyu yanıtlamak mu? 
     * Herhangi bir alternatif yaklaşımlar çalışmanız gerekir? , Ek veri toplamak, daha fazla özellik Mühendisliği yapın veya diğer algoritmalarıyla denemeler?

## <a name="next-steps"></a>Sonraki adımlar

Aşağıda, her adımda TDSP yaşam döngüsü bağlantıları verilmiştir:

   1. [İş anlama](lifecycle-business-understanding.md)
   2. [Veri alımı ve anlama](lifecycle-data.md)
   3. [Modelleme](lifecycle-modeling.md)
   4. [Dağıtım](lifecycle-deployment.md)
   5. [Müşteri kabulü](lifecycle-acceptance.md)

Belirli senaryolar için işlemdeki tüm adımlar gösteren baştan sona tam talimatlara sunuyoruz. [Örnek izlenecek yollar](walkthroughs.md) makale bağlantılar ve küçük resim açıklamaları senaryolarla listesini sağlar. İzlenecek yollar bulut, şirket içi araçları ve Hizmetleri bir iş akışı veya akıllı bir uygulama oluşturmak için ardışık düzen birleştirmek nasıl gösterilmektedir. 

Azure Machine Learning Studio kullanan TDSPs adımları yürütmek nasıl örnekleri için bkz: [TDSP Azure Machine Learning ile kullanmak](http://aka.ms/datascienceprocess). 
