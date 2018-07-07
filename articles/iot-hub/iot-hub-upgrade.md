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
ms.openlocfilehash: 1f60b7d30c073c49d5e0a7d35e7263c2181ed744
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37903073"
---
# <a name="how-to-upgrade-your-iot-hub"></a>IOT hub'ınıza yükseltme

IOT çözümünüzü büyüdükçe, Azure IOT hub'ı ölçek büyütmenize yardımcı olmak hazırdır. Azure IOT Hub'ı iki katmanı sunar, (B) temel ve standart (S), uyum sağlamak istediğiniz müşterileri için farklı özellikleri kullanın. Her bir katman içinde olan her gün gönderilen ileti sayısını belirlemek üç boyutları (1, 2 ve 3). 

Daha fazla cihaz sahip ve daha fazla özelliğe ihtiyacınız olduğunda, IOT hub, gereksinimlerinize uyacak şekilde ayarlamak için üç yolu vardır:

* IOT hub dahilindeki birimleri ekleyin. Örneğin, B1 IOT hub'ındaki ek her birim bir ek 400.000 günlük ileti olanak tanır. 
* IOT hub'ı boyutunu değiştirin. Örneğin, günde her birimi destekleyen iletilerin miktarını artırmak için B2 katman B1 katmanı geçirin.
* Daha yüksek bir katmana yükseltin. Örneğin, B1 katmandan aynı ileti kapasite ancak standart katmanda gelen Gelişmiş Özellikler ile S1 katmanına yükseltin.

Bu değişiklikleri tüm mevcut işlemleri kesintiye uğratmadan ortaya çıkabilir.

IOT hub'ınıza düşürmek istiyorsanız, birimleri kaldırın ve IOT hub'ı azaltın. Ancak, daha düşük bir katmana inemezsiniz. Örneğin, S1 katmanına S2 katmanından ancak B1 katmanı için S2 katmanı taşıyabilirsiniz. 

Bu örnekler, IOT hub'ınıza çözüm değişikliklerinizi ayarlamak nasıl anlamanıza yardımcı olması için yöneliktir. Her katmanın özellikleri hakkında belirli bilgiler için her zaman başvurmanız gerekir [Azure IOT Hub fiyatlandırması](https://azure.microsoft.com/pricing/details/iot-hub/). 

## <a name="upgrade-your-existing-iot-hub"></a>Var olan IOT hub'ı yükseltme 

1. Oturum [Azure portalında](https://portal.azure.com/) ve IOT hub'ınıza gidin. 
2. Seçin **fiyatlandırma ve ölçek**. 

   ![Fiyatlandırma ve ölçek](./media/iot-hub-upgrade/pricing-scale.png)

3. Hub'ınız için katmanını değiştirmek üzere seçin **fiyatlandırma ve ölçek katmanı**. Yeni katmanı seçin ve ardından tıklayın **seçin**.

   ![Fiyatlandırma ve ölçek](./media/iot-hub-upgrade/select-tier.png)

4. Hub'ınızdaki birim sayısını değiştirmek için altında yeni bir değer girin. **IOT Hub birimlerinin**. 
5. Seçin **Kaydet** yaptığınız değişiklikleri kaydedin. 

IOT hub'ınız şimdi ayarlanır ve yapılandırmalarınızı değiştirilmez. IOT hub'ı bölüm sınırından temel katmanı Not 8'dir. Bu sınırı kaldığından Temel katmandan standart katmana geçiş yaptığınızda.

## <a name="next-steps"></a>Sonraki adımlar

Hakkında daha fazla ayrıntıyı [doğru IOT Hub katmanını seçme](iot-hub-scaling.md). 

