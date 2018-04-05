---
title: Azure IOT hub'ı yükseltme | Microsoft Docs
description: Fiyatlandırma ve ölçek katmanı IOT hub'ın daha fazla ileti ve cihaz Yönetimi işlevlerini edinecek şekilde değiştirin.
services: iot-hub
documentationcenter: ''
author: kgremban
manager: timlt
ms.assetid: ''
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/02/2018
ms.author: kgremban
ms.openlocfilehash: d383d26b406c012b6b76225faf89f4b5dbd6bb9c
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="how-to-upgrade-your-iot-hub"></a>IOT hub'ınızı yükseltme

IOT çözümünüzü büyüdükçe, Azure IOT Hub ölçeği yardımcı olması hazırdır. Azure IOT hub'ı iki katmanı sunar, (B) temel ve standart (S), istediğiniz müşteriler uyum sağlamak için farklı özellikleri kullanın. Her katman içinde her gün gönderilen ileti sayısını belirleyen üç (1, 2 ve 3) boyutlarıdır. 

Daha fazla cihaz varsa ve daha fazla özelliklerine ihtiyaç, gereksinimlerinize uygun olarak, IOT hub'ı ayarlamak için üç yol vardır:

* IOT hub'ı içinde birimleri ekleyin. Örneğin, bir ek 400.000 iletiler günde bir B1 IOT hub'ı ek her birimi sağlar. 
* IOT hub'ı boyutunu değiştirin. Örneğin, günde her birimi destekleyen iletilerin miktarını artırmak için B2 katmanına B1 katmanından geçirin.
* Daha yüksek bir katmana yükseltin. Örneğin, B1 katmanından aynı Mesajlaşma kapasitesi ancak standart katmanındaki gelen Gelişmiş özellikleri ile S1 katmana yükseltin.

Bu değişiklikler, tüm mevcut işlemleri kesintiye uğratmadan oluşabilir.

IOT hub'ınızı düşürmek istiyorsanız, birimleri kaldırın ve IOT hub'ı azaltın. Ancak, daha düşük bir katmana düşürülemiyor. Örneğin, S1 katmanına S2 katmanından ancak S2 katman B1 katmanına taşıyabilirsiniz. 

Bu örnekler, IOT hub'ınızı çözüm değişikliklerinizi ayarlamak nasıl anlamanıza yardımcı olması için yöneliktir. Her katmanın özellikleri hakkında belirli bilgiler için her zaman için başvurmalıdır [Azure IOT Hub fiyatlandırma](https://azure.microsoft.com/pricing/details/iot-hub/). 

## <a name="upgrade-your-existing-iot-hub"></a>Var olan IOT hub'ınızı yükseltme 

1. Oturum [Azure portal](https://portal.azure.com/) ve IOT hub'ına gidin. 
2. Seçin **fiyatlandırma ve ölçek**. 

   ![Fiyatlandırma ve ölçek](./media/iot-hub-upgrade/pricing-scale.png)

3. Hub'ınız için katmanını değiştirmek için seçin **fiyatlandırma ve ölçek katmanı**. Yeni katman seçin ve ardından **seçin**.

   ![Fiyatlandırma ve ölçek](./media/iot-hub-upgrade/select-tier.png)

4. Hub'ınızdaki birim sayısını değiştirmek için altında yeni bir değer girin **IOT Hub birimleri**. 
5. Seçin **kaydetmek** yaptığınız değişiklikleri kaydetmek için. 

IOT hub'ınız şimdi ayarlanır ve yapılandırmalarınızı değiştirilmemiştir. 

## <a name="next-steps"></a>Sonraki adımlar

Hakkında daha fazla ayrıntıyı [sağ IOT hub'ı katmanı seçme](iot-hub-scaling.md). 

