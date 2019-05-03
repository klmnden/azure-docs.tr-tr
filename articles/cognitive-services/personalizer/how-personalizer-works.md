---
title: Nasıl çalışır Personalizer - Personalizer
titleSuffix: Azure Cognitive Services
description: Personalizer bir bağlamda kullanmak için hangi eylemin bulmak için makine öğrenimini kullanıyor. Her öğrenme döngüsü, yalnızca kendisine derecesini gönderdiğiniz ve ödül çağıran veriler üzerine geliştirilen bir modeli vardır. Her öğrenme döngüsü birbirinden tamamen bağımsızdır.
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: 6b2237f27fba5eaf952932cd6592052649400b96
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027134"
---
# <a name="how-personalizer-works"></a>Personalizer nasıl çalışır?

Personalizer bir bağlamda kullanmak için hangi eylemin bulmak için makine öğrenimini kullanıyor. Özel olarak kendisine gönderilen veriler üzerinde eğitilmiş bir modeli öğrenme döngüsü her sahip **derece** ve **ödül** çağırır. Her öğrenme döngüsü birbirinden tamamen bağımsızdır. Her bir bölümü veya kişiselleştirmek isteyip uygulamanızın davranışını bir öğrenme döngüsü oluşturun.

Her bir döngü için **ile derece API'ye çağrı** geçerli bağlam üzerinde alan:

* Olası eylemler listesi: içerik öğeleri, üst eylemi seçin.
* Listesi [bağlam özellikleri](concepts-features.md): kullanıcı, içerik ve bağlam gibi ilgili bağlamsal veriler.

**Derece** API karar ya da kullanılacak:

* _Exploit_: Geçmiş verilere dayanan en iyi eylem karar vermek için geçerli model.
* _Keşfedin_: Üst eylem yerine farklı bir eylem seçin.

**Ödül** API:

* Her bir derece görüşmesinin Ödül puanları ve özellikleri kaydederek modeli eğitmek için verileri toplar.
* Belirtilen ayarlara dayanan modeli güncelleştirmek için bu verileri kullanan _öğrenme ilke_.

## <a name="architecture"></a>Mimari

Aşağıdaki görüntüde sıralama ve ödül çağrıları çağırma mimari akışı gösterilmektedir:

![Alternatif metin](./media/how-personalizer-works/personalization-how-it-works.png "kişiselleştirme nasıl çalışır")

1. Personalizer eylemin boyut belirlemek için bir iç yapay ZEKA modeli kullanır.
1. Hizmet geçerli model yararlanma ya da yeni seçenekleri modeli için keşfetmek karar verir.  
1. Derecelendirme sonucu, Event Hubs'a gönderilir.
1. Personalizer ödül aldığında, Event Hubs'a ödül gönderilir. 
1. Boyut sayısı ve ödül ilişkilendirilir.
1. Yapay ZEKA modeli bağıntı sonuçlarına göre güncelleştirilir.
1. Çıkarımı altyapısının, yeni modeli ile güncelleştirilir. 

## <a name="research-behind-personalizer"></a>Personalizer arkasında araştırma

Personalizer, üstün Bilim ve araştırma alanında üzerinden hesaplanmıştır [pekiştirmeye dayalı öğrenme](concepts-reinforcement-learning.md) raporlar, araştırma etkinlikleri ve devam eden bir araştırma alanlarının Microsoft Research dahil olmak üzere.

## <a name="terminology"></a>Terminoloji

* **Döngü öğrenme**: Kişiselleştirme özelliğinden faydalanabilir, uygulamanızın her bir bölümü için öğrenme döngüsü oluşturabilirsiniz. Birden fazla deneyimini kişiselleştirmek için varsa, her biri için bir döngü oluşturun. 

* **Eylemler**: Ürün ve promosyonlar, aralarından seçim yapabileceğiniz gibi içerik öğeleri eylemlerdir. Personalizer olarak bilinen, kullanıcılarınıza göstermek için en iyi eylemi seçer _ödüllendirin eylem_, derece API aracılığıyla. Her eylem, özellikleri derece istekle beraber gönderilen olabilir.

* **Bağlam**: Daha doğru bir sıra sağlamak için örneğin, bağlamıyla ilgili bilgileri sağlayın:
    * Kullanıcı.
    * Cihaz öğrenebilir. 
    * Geçerli saat.
    * Diğer veriler geçerli durumu hakkında.
    * Kullanıcı veya bağlam ilgili geçmiş verileri.

    Özel uygulamanızın farklı bağlam bilgileri olabilir. 

* **[Özellikleri](concepts-features.md)**: Bir içerik öğesi veya kullanıcı bağlamı hakkında bilgi birimidir.

* **Ödül**: Kullanıcı sıralama API'sine nasıl yanıt verdiğini ölçü eylemi, 0 ile 1 arasında bir puan döndürdü. 0-1 değeri, kişiselleştirme iş hedeflerinize ulaşmak istediğiniz nasıl Yardım üzerinde temel iş mantığınızı tarafından ayarlanır. 

* **Araştırma**: En iyi eylem döndürmek yerine, bu kullanıcı için farklı bir eylem seçtiğinde Personalizer hizmet araştırmaktadır. Personalizer hizmet değişikliklerini, stagnation, önler ve devam eden kullanıcı davranışlarına yönelik inceleyerek uyarlayabilirsiniz. 

* **Deneme süresi**: Personalizer hizmeti bir ödül için bekleyeceği süreyi, derece çağrı andan itibaren başlayarak, olay için oluştu.

* **Etkin olmayan olaylar**: Etkin olmayan bir olayın nerede derece çağrıldı, ancak kullanıcı hiç sonuç, istemci uygulama kararları nedeniyle görürsünüz emin değilseniz olan biridir. Etkin olmayan olaylar oluşturma ve kişiselleştirme sonuçlarını depolamak için izin verin ve sonra bunları daha sonra machine learning modeli etkilemeden atmak mı karar verin.

* **Model**: Personalizer modeli, tüm verileri sıralama ve ödül çağrıları ve öğrenme İlkesi tarafından belirlenen bir eğitim davranışı gönderdiğiniz bağımsız değişkenler birleşimi eğitim verilerini almaya kullanıcı davranışı hakkında bilgi edindiniz yakalar. 

## <a name="next-steps"></a>Sonraki adımlar

Anlamak [Personalizer kullanabileceğiniz](where-can-you-use-personalizer.md).