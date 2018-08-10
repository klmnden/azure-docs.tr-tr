---
title: Azure IOT hub'ı yükseltme | Microsoft Docs
description: Fiyatlandırma ve ölçek katmanı için IOT Hub'ı daha fazla ileti ve cihaz Yönetimi işlevlerini edinecek şekilde değiştirin.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/02/2018
ms.author: kgremban
ms.openlocfilehash: e1342ed574d84ed5b4edd5060c2d6d3ec8bca1a8
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003120"
---
# <a name="how-to-upgrade-your-iot-hub"></a>IOT hub'ınıza yükseltme

IOT çözümünüzü büyüdükçe, Azure IOT hub'ı ölçek büyütmenize yardımcı olmak hazırdır. Azure IOT Hub'ı iki katmanı sunar, (B) temel ve standart (S), uyum sağlamak istediğiniz müşterileri için farklı özellikleri kullanın. Her bir katman içinde olan her gün gönderilen ileti sayısını belirlemek üç boyutları (1, 2 ve 3). 

Daha fazla cihaz sahip ve daha fazla özelliğe ihtiyacınız olduğunda, IOT hub, gereksinimlerinize uyacak şekilde ayarlamak için üç yolu vardır:

* IOT hub dahilindeki birimleri ekleyin. Örneğin, B1 IOT hub'ındaki ek her birim bir ek 400.000 günlük ileti olanak tanır. 
* IOT hub'ı boyutunu değiştirin. Örneğin, günde her birimi destekleyen iletilerin miktarını artırmak için B2 katman B1 katmanı geçirin.
* Daha yüksek bir katmana yükseltin. Örneğin, B1 katmandan aynı ileti kapasite ancak standart katmanda gelen Gelişmiş Özellikler ile S1 katmanına yükseltin.

Bu değişiklikleri tüm mevcut işlemleri kesintiye uğratmadan ortaya çıkabilir.

IOT hub'ınıza düşürmek istiyorsanız, birimleri kaldırın ve IOT hub'ı azaltın. Ancak, daha düşük bir katmana inemezsiniz. Örneğin, S1 katmanına S2 katmanından ancak B1 katmanı için S2 katmanı taşıyabilirsiniz. Ayrıca, bu yalnızca bir tür unutmayın [edition](https://azure.microsoft.com/pricing/details/iot-hub/) IOT hub'ı bir katman içinde seçilebilir. Örneğin, birden çok S1 birimi olan, ancak bir karışımını birimleri S1 ve B3 ya da S1 ve S2 gibi farklı sürümleri ile değil, bir IOT hub'ı oluşturabilirsiniz.

Bu örnekler, IOT hub'ınıza çözüm değişikliklerinizi ayarlamak nasıl anlamanıza yardımcı olması için yöneliktir. Her katmanın özellikleri hakkında belirli bilgiler için her zaman başvurmanız gerekir [Azure IOT Hub fiyatlandırması](https://azure.microsoft.com/pricing/details/iot-hub/). 

## <a name="upgrade-your-existing-iot-hub"></a>Var olan IOT hub'ı yükseltme 

1. Oturum [Azure portalında](https://portal.azure.com/) ve IOT hub'ınıza gidin. 
2. Seçin **fiyatlandırma ve ölçek**. 

   ![Fiyatlandırma ve ölçek](./media/iot-hub-upgrade/pricing-scale.png)

3. Hub'ınız için katmanını değiştirmek üzere seçin **fiyatlandırma ve ölçek katmanı**. Yeni katmanı seçin ve ardından tıklayın **seçin**.

   ![Fiyatlandırma ve ölçek](./media/iot-hub-upgrade/select-tier.png)

4. Hub'ınızdaki birim sayısını değiştirmek için altında yeni bir değer girin. **IOT Hub birimlerinin**. 
5. Seçin **Kaydet** yaptığınız değişiklikleri kaydedin. 

IOT hub'ınız şimdi ayarlanır ve yapılandırmalarınızı değiştirilmez. Temel katman IOT hub'ı için en yüksek bölüm sınırı 8'dir ve standart katman için 32'dir unutmayın. Çoğu IOT hub'ları yalnızca 4 bölüm gerekir. IOT hub'ı oluşturulduğunda ve CİHAZDAN buluta iletileri bu iletileri eşzamanlı okuyucu sayısıyla ilgilidir'ün bölüm sınırından seçilir. Bu değer, Temel katmandan standart katmana geçiş yaptığınızda değişmeden kalır. 

## <a name="next-steps"></a>Sonraki adımlar

Hakkında daha fazla ayrıntıyı [doğru IOT Hub katmanını seçme](iot-hub-scaling.md). 

