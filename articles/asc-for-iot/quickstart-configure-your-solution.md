---
title: IOT çözümü Önizleme, ASC yapılandırma | Microsoft Docs
description: IOT için ASC kullanarak uçtan uca IOT çözümünüzü yapılandırmayı öğrenin.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: ae2207e8-ac5b-4793-8efc-0517f4661222
ms.service: ascforiot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 3ba8b1a099dc204730813754fdfc4b0b8ce45303
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541683"
---
# <a name="quickstart-configure-your-iot-solution"></a>Hızlı Başlangıç: IOT çözümünüzü yapılandırın

> [!IMPORTANT]
> ASC IOT için şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede, IOT için ASC kullanarak IOT güvenlik çözümünüzü ilk yapılandırmasını gerçekleştirmek bir açıklama sağlar. 

## <a name="asc-for-iot"></a>IOT için ASC

ASC IOT için Azure tabanlı IOT çözümleri için kapsamlı uçtan uca güvenlik sağlar.

IOT için ASC ile. tek bir Panoda tüm IOT çözümünüzü IOT cihazlarınızı, IOT platformları ve Azure arka uç kaynaklarına tümünün görünmesini izleyebilirsiniz.

IOT Hub'ınızda etkinleştirildikten sonra IOT için ASC da IOT hub'ınıza bağlanan ve IOT çözümünüzü ilişkili diğer Azure hizmetleri otomatik olarak tanımlar.

Otomatik ilişki algılamayı yanı sıra, aynı zamanda çekme ve etiket için hangi diğer Azure kaynakları, IOT çözümünüzün bir parçası olarak seçebilirsiniz.
Seçimlerinizi tüm abonelikler, kaynak grubu veya tek kaynaklar eklemenize izin verir.

Tüm kaynak ilişkiler tanımladıktan sonra ASC IOT için Azure Güvenlik Merkezi, bu kaynak için güvenlik önerileri ve uyarılar sağlamak üzere yararlanır.

## <a name="add-azure-resources-to-your-iot-solution"></a>IOT çözümünüzü Azure kaynakları ekleyin

IOT çözümünüzü yeni bir kaynak eklemek için aşağıdakileri yapın: 

1. Açık, **IOT hub'ı** Azure portalında. 
2. Seçin ve açın **kaynakları** altında **güvenlik** sol menüden. 
3. Seçin **kaynak Ekle**.
4. IOT çözümünüze ait kaynakları seçin.
4. **Ekle**'ye tıklayın. 

Tebrikler! IOT çözümünüzü yeni bir kaynak ekledik.

ASC izleyiciler yeni olduğunuz kaynakları ve yüzeyleri ilgili güvenlik önerilerini ve Uyarıları, IOT çözümünüzün bir parçası olarak eklenen artık IOT için.

## <a name="next-steps"></a>Sonraki adımlar

Güvenlik modülleri oluşturma hakkında bilgi edinmek için sonraki makaleye ilerleyin...

> [!div class="nextstepaction"]
> [Güvenlik modülleri oluşturma](quickstart-create-security-twin.md)