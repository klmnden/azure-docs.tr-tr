---
title: Ödül puan - Personalizer
titleSuffix: Azure Cognitive Services
description: Ödül puanı, kullanıcının sonuçlandı ne kadar iyi kişiselleştirme tercih ettiğiniz RewardActionID, gösterir. Ödül puanı değeri gözlemlere kullanıcı davranışına dayalı iş mantığınızı tarafından belirlenir. Kendi makine öğrenimi modellerini ödül değerlendirerek personalizer eğitir.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 06/07/2019
ms.author: edjez
ms.openlocfilehash: c64d43301fd173203bd1625b8d37120b71c22805
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67077408"
---
# <a name="reward-scores-indicate-success-of-personalization"></a>Kişiselleştirme başarısını ödül puanlar

Ödül puan ne kadar iyi kişiselleştirme seçimi belirtir [RewardActionID](https://docs.microsoft.com/rest/api/cognitiveservices/personalizer/rank/rank#response), kullanıcı ile sonuçlandı. Ödül puanı değeri gözlemlere kullanıcı davranışına dayalı iş mantığınızı tarafından belirlenir.

Kendi makine öğrenimi modellerini ödül değerlendirerek personalizer eğitir. 

## <a name="use-reward-api-to-send-reward-score-to-personalizer"></a>Ödül için Personalizer ödül puan göndermek için API kullanma

Ödül Personalizer tarafından gönderilen [ödül API](https://docs.microsoft.com/rest/api/cognitiveservices/personalizer/events/reward). Bir ödül, -1 ile 1 arasında bir sayıdır. Personalizer ödül en yüksek olası toplamını zamanla elde etmek için modeli eğitir.

Ödül kullanıcı davranışı, gün sonra olabilecek oluştuktan sonra gönderilir. Bir olay yok ödül sahip olduğu kabul edildiği veya varsayılan ödül yapılandırılmış kadar Personalizer bekleyeceği en uzun süreyi [ödül bekleme süresi](#reward-wait-time) Azure portalında.

İçinde bir olay için ödül puanı alınan edilmemiş varsa **ödül bekleme süresi**, ardından **varsayılan ödül** uygulanır. Genellikle, **[varsayılan ödül](how-to-settings.md#configure-reward-settings-for-the-feedback-loop-based-on-use-case)** sıfır olarak yapılandırılır.


## <a name="behaviors-and-data-to-consider-for-rewards"></a>Davranışları ve ödüller için dikkate alınması gereken veri

Bu sinyalleri ve davranışları ödül score bağlamı için göz önünde bulundurun:

* Öneriler kullanıcı girişini seçenekleri söz konusu olduğunda doğrudan ("X şunu mu demek musunuz?").
* Oturum uzunluğu.
* Oturumlar arasındaki süre.
* Yaklaşım analizi kullanıcının etkileşimler dizesi.
* Doğrudan sorular ve mini anketler doğruluğu fayda hakkında burada yakmasını kullanıcı için geri bildirim ister.
* Uyarıları veya gecikmesi uyarılara yanıt olarak verilen yanıt.

## <a name="composing-reward-scores"></a>Ödül puanları oluşturma

İş mantığınızı bir ödül puan hesaplanması gerekir. Sonuç olarak gösterilebilir:

* Bir kez gönderilen tek bir sayı 
* Hemen (0,8 gibi) gönderilen bir puan ve daha sonra gönderilen bir ek puanı (genellikle 0.2).

## <a name="default-rewards"></a>Varsayılan ödül

İçinde herhangi bir ödül aldıysanız [ödül bekleme süresi](#reward-wait-time)çağrı süresi boyut beri örtük olarak Personalizer geçerlidir **varsayılan ödül** derece o olaya.

## <a name="building-up-rewards-with-multiple-factors"></a>Ödül çoklu faktörlerle ile yedekleme oluşturma  

Etkili kişiselleştirme için ödül puan oluşturabilirsiniz (-1 ile 1 arasında herhangi bir sayı) birden çok unsura bağlı. 

Örneğin, video içerik listesini kişiselleştirmek için bu kuralları uygulayabilirsiniz:

|Kullanıcı davranışı|Kısmi puan değeri|
|--|--|
|Kullanıcı üst öğede tıkladı.|+0.5 ödül|
|Kullanıcı bu öğesinin gerçek içeriği açılır.|+0.3 ödül|
|Kullanıcı 5 dakika içeriği veya % 30 izlenen, hangisinin daha uzun olduğuna.|+0.2 ödül|
|||

Ardından, API için toplam ödül gönderebilirsiniz.

## <a name="calling-the-reward-api-multiple-times"></a>Birden çok kez ödül API çağırma

Ödül farklı Ödül puanları gönderme aynı olay Kimliğini kullanarak API çağrısı. Bu ödül Personalizer alır, bu olay için son ödül Personalizer ayarlarında belirtilen toplayarak belirler.

Toplama ayarları:

*  **İlk**: Olay için alınan ilk ödül puanı alır ve rest atar.
* **Sum**: EventID için toplanan tüm Ödül puanları alır ve bunları bir araya getirir.

Sonra alınan bir olay için tüm ödül **ödül bekleme süresi**atılır ve modellerin eğitimi etkilemez.

Ödül puanları ' ekleyerek, son ödül 1'den daha yüksek veya -1'den daha düşük olabilir. Bu, başarısız hizmet yapmaz.

<!--
@edjez - is the number ignored if it is outside the acceptable range?
-->

## <a name="best-practices-for-calculating-reward-score"></a>Ödül puanı hesaplamaya yönelik en iyi uygulamalar

* **Başarılı kişiselleştirme true göstergeleri göz önünde bulundurun**: Tıklama açısından düşünmeniz kolaydır, ancak iyi bir ödül kullanıcılarınız için istediğinize bağlı *elde* kişiler için istediğinize yerine *yapmak*.  Örneğin, tıklar ödüllendiriyoruz clickbait potansiyeli olan içerik seçmeye neden olabilir.

* **Bir ödül puanlamak için ne kadar iyi kişiselleştirme çalışılan kullanım**: Bir film öneri kişiselleştirme Umarım kullanıcı filmi izlemek ve yüksek bir derecelendirme vererek neden olur. Film derecelendirme büyük olasılıkla birçok şey (kullanıcının ruh acting kalitesini) bağlı olduğundan, bunu nasıl iyi ödül sinyal de değil *kişiselleştirme* çalıştı. İlk birkaç dakika film izlemeyi kullanıcı kişiselleştirme verimlilik ve daha iyi bir sinyal 5 dakika sonra bir ödül 1 gönderme daha iyi bir sinyal ancak olabilir.

* **Ödül yalnızca uygulamak için RewardActionID**: Personalizer ödül RewardActionID içinde belirtilen eylemle çalışıp çalışmadığını anlamak için geçerlidir. Diğer Eylemler ve kullanıcı bunlar üzerinde görüntülemek isterseniz, ödül sıfır olmalıdır.

* **İstenmeyen sonuçlara göz önünde bulundurun**: Sorumlu sonuçları ile neden ödül işlevler Oluştur [etik kurallara ve sorumlu kullanımı](ethics-responsible-use.md).

* **Artımlı ödül kullanın**: Daha iyi ödül ulaşmak için daha küçük kullanıcı davranışları için kısmi ödül ekleme Personalizer yardımcı olur. Bu artımlı bir ödül son istenen davranışı kullanıcının ilgi çekici için daha yakından alma bilmek algoritması sağlar.
    * Daha fazla bilgi, bir süredir ilk öğe üzerinde getirirse filmler listesi gösterildiğinden, bazı kullanıcı katılım gerçekleştiğini belirleyebilirsiniz. Davranış 0,1 bir ödül puanıyla güvenebilirsiniz. 
    * Kullanıcı sayfası açılır ve ardından çıkıldı ödül puan 0.2 olabilir. 

## <a name="reward-wait-time"></a>Ödül bekleme süresi

Personalizer bağıntı kurmak bir sıralama bilgilerini modeli eğitmek için ödül çağrılarında gönderilen ödül ile çağrısı. Bu, farklı zamanlarda ortaya çıkabilir. Personalizer sınırlı bir süre için derece çağrı oldu derece çağrısı etkin olmayan bir olay olarak yapılan ve daha sonra etkin olsa bile başlatılmasını bekler.

Varsa **ödül bekleme süresi** süresi sonu ve ödül bilgi olmuştur, varsayılan ödül, olay eğitim için uygulanır. En fazla bekleme süresi 6 gün olur.

## <a name="best-practices-for-setting-reward-wait-time"></a>Ödül ayarlamaya yönelik en iyi bekleme süresi

Daha iyi sonuçlar için bu önerileri izleyin.

* Ödül bekleme süresi, kullanıcı geri bildirimi almak için yeterli zaman bırakarak, olabildiğince kısa yapın. 

<!--@Edjez - storage quota? -->

* Geri bildirim almak için gereken süreden daha kısa bir süre seçmeyin. Bir kullanıcı 1 dakika video izlenen sonra ödül bazıları vardır, örneğin, deneme uzunluğu en az bir çift olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* [Pekiştirmeye dayalı öğrenme](concepts-reinforcement-learning.md) 
* [API boyut deneyin](https://westus2.dev.cognitive.microsoft.com/docs/services/personalizer-api/operations/Rank/console)
* [Ödül API'sini deneyin](https://westus2.dev.cognitive.microsoft.com/docs/services/personalizer-api/operations/Reward)
