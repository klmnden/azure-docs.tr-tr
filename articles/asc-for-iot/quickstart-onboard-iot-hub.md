---
title: IOT hub'ı önizlemesi hizmetinde IOT için Azure Güvenlik Merkezi'ni etkinleştirin | Microsoft Docs
description: IOT Hub'ınızın IOT hizmeti için Azure Güvenlik Merkezi'ni etkinleştirmeyi öğrenin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 670e6d2b-e168-4b14-a9bf-51a33c2a9aad
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/16/2019
ms.author: mlottner
ms.openlocfilehash: f81fb7aeed1b704ebdd82c1f5b83c33a4b05e9ca
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67617997"
---
# <a name="quickstart-enable-service-in-iot-hub"></a>Hızlı Başlangıç: IOT hub hizmetini etkinleştirme

> [!IMPORTANT]
> IOT için Azure Güvenlik Merkezi şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede, IOT Hub'ınızın IOT Önizleme hizmeti için Azure Güvenlik Merkezi (ASC) etkinleştirmek bir açıklama sağlar.  

> [!NOTE]
> Şu anda IOT için Azure Güvenlik Merkezi, yalnızca standart katman IOT hub'larını destekler.
> IOT için Azure Güvenlik Merkezi tek bir hub çözümüdür. Birden çok hub'a ihtiyacınız varsa birden çok çözümü gereklidir. 

## <a name="prerequisites-for-enabling-the-service"></a>Hizmetini etkinleştirmek için Önkoşullar

- Log Analytics çalışma alanı
  - İki tür bilgi varsayılan olarak, IOT için Log Analytics çalışma alanınızda ASC tarafından varsayılan olarak depolanır; **güvenlik uyarıları** ve **önerileri**. 
  - Bir ek bilgi türü depolama alanı eklemek seçebileceğiniz **ham olaylar**. Bu depolama Not **ham olaylar** Log Analytics'e ek depolama maliyetlerini taşır. 
- IOT hub'ı (standart katman)
- Tümüne uyan [hizmet önkoşulları](service-prerequisites.md) 
- Desteklenen hizmet bölgeleri
  - Orta ABD
  - Kuzey Avrupa
  - Güneydoğu Asya

## <a name="enable-asc-for-iot-on-your-iot-hub"></a>IOT Hub'ınızın IOT için ASC etkinleştir 

IOT Hub'ınıza güvenliği etkinleştirmek için aşağıdakileri yapın: 

1. Açık, **IOT hub'ı** Azure portalında. 
2. Altında **güvenlik** menüsünde tıklatın **genel bakış**, ardından **başlangıç Önizleme**. 
3. Seçin **etkinleştirme IOT güvenlik**. 
4. Log Analytics çalışma alanı bilgilerinizi sağlayın. 
   - Depolama kullanmayı **ham olaylar** bırakarak tarafından depolama varsayılan bilgi türlerini yanı sıra **ham olay** geçiş **üzerinde**. 
   - Etkinleştirme kullanmayı **ikizi koleksiyon** bırakarak tarafından **ikizi koleksiyon** geçiş **üzerinde**. 
5. **Kaydet**’e tıklayın. 

Tebrikler! IOT hub'ınızdaki IOT ASC etkinleştirme tamamladınız. 

## <a name="next-steps"></a>Sonraki adımlar

Çözümünüzü yapılandırma hakkında bilgi edinmek için sonraki makaleye ilerleyin...

> [!div class="nextstepaction"]
> [Çözümünüzü yapılandırın](quickstart-configure-your-solution.md)
