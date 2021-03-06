---
title: IOT için Azure Güvenlik Merkezi Önizleme maliyetlerini anlama | Microsoft Docs
description: IOT ve nasıl kontrol edebilir, Azure Güvenlik Merkezi ile ilişkili maliyetler hakkında bilgi edinin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: ef839708-4574-4a40-bc45-07005f8e9daf
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2019
ms.author: mlottner
ms.openlocfilehash: dd041cdb1608eab60fa2a5fa756f381656a13a46
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67618448"
---
# <a name="pricing-and-associated-costs"></a>Fiyatlandırma ve ilişkili maliyetler

> [!IMPORTANT]
> IOT için Azure Güvenlik Merkezi şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede Azure Güvenlik Merkezi (ASC) IOT fiyatlandırma modeli açıklar, tüm ilişkili maliyetleri özetler ve bunların nasıl yönetileceğini açıklar.

## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma modeli iki oluşan IOT için ASC bölümleri ve bir IOT hub'ı bir kez faturalandırılır [etkin](quickstart-onboard-iot-hub.md) ASC IOT için içinde:

- Cihaz - IOT hub'ı günlüklerinin Analizine göre yerleşik güvenlik özellikleri tarafından maliyeti.

- İleti - güvenlik iletileri IOT Edge veya yaprak cihazlardan dayalı Gelişmiş güvenlik özellikleri tarafından maliyeti.

  >[!Note]
  > Güvenlik iletilerinin kotası tüketim IOT Hub'ında da neden olur.

Daha fazla bilgi için [Güvenlik Merkezi fiyatlandırma](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="associated-costs"></a>İlişkili maliyetleri

IOT için ASC doğrudan fiyatlandırma bir parçası olmayan ilgili maliyetleri, iki tür vardır:

- IOT hub'ı kota tüketim

- Günlük analizi depolama maliyetleri

Ayarlarınızı değiştirerek belirli özellikleri dışında çıkarak ilişkili maliyetleri azaltabilir.

Ayarlarınızı değiştirmek için:

1. IOT hub'ı açın.

2. Altında **güvenlik**, tıklayın **genel bakış**.

3. Tıklayın **ayarları**.

Aşağıdaki tabloda, ilişkili maliyetlerin bir özetini ve her seçeneğin etkilerini sağlar.

|     | Kullanım | Yorum |
| --- | --- | --- |
| **IOT hub'ı kota tüketim** |  |
| [Cihaz dışarı](https://docs.microsoft.com/azure/iot-hub/iot-hub-bulk-identity-mgmt#export-devices) işi (ikizi dışarı aktarma) | Günde bir kez | Devre dışı _ikizi meta veri koleksiyonu_ |
| **Log Analytics depolama** |  |
| Cihaz öneri ve uyarılar| Güvenlik önerisi ve hizmet tarafından oluşturulan uyarılar | İsteğe bağlı değil |
| Ham güvenlik verileri| IOT cihazları, güvenlik aracıların topladığı ham güvenlik verileri | Devre dışı _ham aygıt güvenlik olaylarını depolayın_ |

>[!Important]
> İletişimlerini ciddi etkileri kullanılabilir güvenlik özelliklerine sahiptir.
  
| Çıkma | Etkileri |
| --- | --- |
| _İkiz meta veri koleksiyonu_ | Devre dışı [özel uyarılar](quickstart-create-custom-alerts.md) |
| | IOT Edge bildirim önerileri devre dışı bırak |
| | Cihaz kimliği tabanlı önerileri ve uyarılar devre dışı bırak |
| _Store ham aygıt güvenlik olayları_ | Cihaz işletim sistemi taban çizgisi önerileri hakkında ayrıntılı bilgi kullanılabilir değil |
| | Şirket ayrıntıları [uyarı](concept-security-alerts.md) ve [öneri](concept-recommendations.md) araştırmalar kullanılabilir değil |


## <a name="see-also"></a>Ayrıca bkz.

- Erişim, [ham güvenlik verileri](how-to-security-data-access.md)
- [Bir cihaz araştırın](how-to-investigate-device.md)
- Anlama ve keşfetme [güvenlik önerileri](concept-recommendations.md)
- Anlama ve keşfetme [güvenlik uyarıları](concept-security-alerts.md)
