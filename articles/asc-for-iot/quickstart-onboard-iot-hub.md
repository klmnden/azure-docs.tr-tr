---
title: ASC IOT hub'ı önizlemesi IOT hizmeti için etkinleştirme | Microsoft Docs
description: IOT Hub'ınızın IOT hizmeti için ASC etkinleştirmeyi öğrenin.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 670e6d2b-e168-4b14-a9bf-51a33c2a9aad
ms.service: ascforiot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: mlottner
ms.openlocfilehash: cea7517a99358d41a8ba60a78b4e2bfdbdeaf0e8
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2019
ms.locfileid: "58576233"
---
# <a name="quickstart-enable-service-in-iot-hub"></a>Hızlı Başlangıç: IOT hub hizmetini etkinleştirme

> [!IMPORTANT]
> ASC IOT için şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede, IOT Hub'ınızın IOT Önizleme hizmeti için ASC etkinleştirmek bir açıklama sağlar.  

> [!NOTE]
> ASC IOT şu an için yalnızca standart katmanı ve daha yüksek IOT hub'larını destekler.
>ASC için IOT hub'ı tek bir çözümdür. Birden çok hub'a ihtiyacınız varsa birden çok çözümü gereklidir. 

## <a name="prerequisites-for-enabling-the-service"></a>Hizmetini etkinleştirmek için Önkoşullar

- Log Analytics çalışma alanı
  - İki tür bilgi varsayılan olarak, IOT için Log Analytics çalışma alanınızda ASC tarafından varsayılan olarak depolanır; **güvenlik uyarıları** ve **önerileri**. 
  - Bir ek bilgi türü depolama alanı eklemek seçebileceğiniz **ham olaylar**. Bu depolama Not **ham olaylar** Log Analytics'e ek depolama maliyetlerini taşır. 
- IOT hub'ı (standart katmana veya üzeri)

## <a name="enable-asc-for-iot-on-your-iot-hub"></a>IOT Hub'ınızın IOT için ASC etkinleştir 

IOT Hub'ınıza güvenliği etkinleştirmek için aşağıdakileri yapın: 

1. Açık, **IOT hub'ı** Azure portalında. 
2. Seçin ve açın **güvenlik** sol menüden. 
3. Seçin **etkinleştirme IOT güvenlik**. 
4. Log Analytics çalışma alanı bilgilerinizi sağlayın. 
   - Depolama kullanmayı **ham olaylar** bırakarak tarafından depolama varsayılan bilgi türlerini yanı sıra **ham olay** geçiş **üzerinde**. 
   - Etkinleştirme kullanmayı **ikizi koleksiyon** bırakarak tarafından **ikizi koleksiyon** geçiş **üzerinde**. 
5. **Tamam** düğmesine tıklayın. 
6. **Kaydet**’e tıklayın. 

Tebrikler! IOT hub'ınızdaki IOT ASC etkinleştirme tamamladınız. 

## <a name="next-steps"></a>Sonraki adımlar

Çözümünüzü yapılandırma hakkında bilgi edinmek için sonraki makaleye ilerleyin...

> [!div class="nextstepaction"]
> [Çözümünüzü yapılandırın](quickstart-configure-your-solution.md)