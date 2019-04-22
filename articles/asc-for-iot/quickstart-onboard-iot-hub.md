---
title: IOT hub'ı önizlemesi hizmetinde IOT için Azure Güvenlik Merkezi'ni etkinleştirin | Microsoft Docs
description: IOT Hub'ınızın IOT hizmeti için Azure Güvenlik Merkezi'ni etkinleştirmeyi öğrenin.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 670e6d2b-e168-4b14-a9bf-51a33c2a9aad
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: mlottner
ms.openlocfilehash: 59021d09f2af9d430b118acdeb8aa977094e683e
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58862395"
---
# <a name="quickstart-enable-service-in-iot-hub"></a>Hızlı Başlangıç: IOT hub hizmetini etkinleştirme

> [!IMPORTANT]
> IOT için Azure Güvenlik Merkezi şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede, IOT Hub'ınızın IOT Önizleme hizmeti için Azure Güvenlik Merkezi (ASC) etkinleştirmek bir açıklama sağlar.  

> [!NOTE]
> Şu anda IOT için Azure Güvenlik Merkezi standart katmanı ve daha yüksek IOT hub'ları yalnızca destekler.
> IOT için Azure Güvenlik Merkezi tek bir hub çözümüdür. Birden çok hub'a ihtiyacınız varsa birden çok çözümü gereklidir. 

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