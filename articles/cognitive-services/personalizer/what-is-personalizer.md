---
title: Personalizer nedir
titleSuffix: Azure Cognitive Services
description: Azure Personalizer, gerçek zamanlı davranışından öğrenme kullanıcılarınıza göstermek için en iyi deneyimi seçmenize olanak tanıyan bulut tabanlı bir API hizmetidir.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: c969029bcc0412267507efe81549ec6f8b2988ce
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027120"
---
# <a name="what-is-personalizer"></a>Personalizer nedir?

Azure Personalizer, gerçek zamanlı davranışından öğrenme kullanıcılarınıza göstermek için en iyi deneyimi seçmenize olanak tanıyan bulut tabanlı bir API hizmetidir.

* Kullanıcılar ve içeriği hakkında bilgi sağlar ve kullanıcılarınızın göstermek için en iyi eylemi alır. 
* Temizleme ve veri Personalizer kullanmadan önce etiketi gerek yoktur.
* Sizin için uygun olduğunda Personalizer için geri bildirim sağlayın. 
* Gerçek zamanlı analiz görüntüleyin. 
* Personalizer, var olan deneyimleri doğrulamak için daha büyük bir veri bilimi çaba bir parçası olarak kullanın.

## <a name="how-does-personalizer-work"></a>Personalizer nasıl çalışır?

Personalizer hangi eylemin bir bağlamda en yüksek boyut için keşfetmek için makine öğrenimi modelleri kullanır. İstemci uygulamanızı bunlarla ilgili bilgilerle eylemlerinin listesini sağlar. ve kullanıcı, cihaz, vb. hakkında bilgi içerebilecek bağlam hakkında bilgiler. Personalizer gerçekleştirilecek eylemi belirler. Seçilen eylem istemci uygulamanızın kullandığı sonra Personalizer biçiminde bir ödül puanı geri bildirim sağlar. Geri bildirim döngüsü tamamlandıktan sonra Personalizer kendi modeli gelecekteki sıralamalara sahip için kullanılan otomatik olarak güncelleştirir.

## <a name="how-do-i-use-the-personalizer"></a>Personalizer nasıl kullanabilirim?

![Bir kullanıcıya göstermek için video seçmek için Personalizer kullanma](media/what-is-personalizer/personalizer-example-highlevel.png)

1. Bir deneyiminizi kişiselleştirmek için uygulamanızı seçin.
1. Oluşturma ve kişiselleştirme hizmeti, Azure portalında yapılandırma
1. Personalizer bilgilerle çağırmak için SDK'sını kullanma (_özellikleri_) kullanıcılarınız ve içerik hakkında (_eylemleri_). Veri Personalizer kullanmadan önce etiketlenmiş temizleyen, girmeniz gerekmez. 
1. İstemci uygulamasında kullanıcı Personalizer tarafından seçilen eylem gösterir.
1. SDK, kullanıcının Personalizer'ın eylem seçtiyseniz belirten Personalizer için geri bildirim sağlamak için kullanın. Bu bir _puanı ödüllendirin_genellikle -1 ile 1 arasında.
1. Analytics sistem nasıl çalıştığını ve kişiselleştirme verilerinizi nasıl yardımcı olduğunu değerlendirmek için Azure Portal'da görüntüleyin.

## <a name="where-can-i-use-personalizer"></a>Burada Personalizer kullanabilir miyim?

Örneğin, istemci uygulamanız için Personalizer ekleyebilirsiniz:

* Hangi makale haber Web sitesinde vurgulanır kişiselleştirin.    
* Bir Web sitesinde ad yerleştirme iyileştirin.
* Bir kişiselleştirilmiş "önerilen öğesi" bir alışveriş Web sitesinde görüntüler.
* Belirli bir fotoğraf uygulanacak filtreler gibi kullanıcı arabirimi öğeleri önerin.
* Kullanıcının amacını açıklamak veya bir eylem önermek için bir sohbet Robotu kişinin yanıt'ı seçin.
* Bir kullanıcı bir iş işlemi bir sonraki adım olarak neler önerileri öncelik verin.

## <a name="personalization-for-developers"></a>Geliştiriciler için kişiselleştirme

İki API personalizer hizmet vardır:

* Bilgileri Gönder (_özellikleri_) kullanıcılarınız ve içerik hakkında (_eylemleri_) kişiselleştirmek için. Personalizer üst eylem ile yanıt verir.
* Ne kadar iyi sıralama genellikle 0 ile 1 arasında bir sayı olarak çalışan hakkında Personalizer için geri bildirim gönder (önceki bölümde söylediğiniz -1 ve 1). 

![Kişiselleştirme için olayların temel sırası](media/what-is-personalizer/personalization-intro.png)

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı Başlangıç: Bir geri bildirim döngüsü içinde oluşturmaC#](csharp-quickstart-commandline-feedback-loop.md)
