---
title: "Azure IOT Hub fiyatlandırma anlama | Microsoft Docs"
description: "Geliştirici Kılavuzu - ölçüm ve de dahil olmak üzere IOT Hub ile works fiyatlandırma örnekler nasıl çalıştığı hakkında bilgi."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 1ac90923-1edf-4134-bbd4-77fee9b68d24
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/29/2017
ms.author: elioda
ms.openlocfilehash: 05006a78cc7d82bc048ec5706465f7140eb40e94
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-iot-hub-pricing-information"></a>Azure IOT Hub ile fiyatlandırma bilgileri

[Azure IOT Hub ile fiyatlandırma] [ lnk-pricing] farklı SKU'ları ve IOT Hub için fiyatlandırma hakkında genel bilgiler sağlar. Bu makale nasıl çeşitli IOT hub'ı işlevler iletileri olarak IOT Hub tarafından ölçülen ek ayrıntılar içerir.

## <a name="charges-per-operation"></a>İşlem başına ücret

| İşlem | Faturalama bilgileri | 
| --------- | ------------------- |
| Kimlik kayıt defteri işlemleri <br/> (oluşturma, alma, liste, Güncelleştir, Sil) | Ücret işlenmedi. |
| Cihazdan buluta iletiler | Başarıyla gönderilen iletileri 4 KB öbekler giriş üzerinde IOT Hub'ına örneğin 6-KB ileti 2 mesaj doludur sizden ücret kesilir. |
| Bulut-cihaz iletilerini | Başarıyla gönderilen iletileri 4 KB öbekler ücretlendirilen, 6-KB ileti 2 mesaj örneğin doludur. |
| Dosya yüklemeleri | Azure depolama birimine dosya aktarımı IOT Hub tarafından ölçülen değil. Dosya aktarımı başlatma ve tamamlanma iletilerini 4 KB artımlarla ölçülen messaged gibi ücretlendirilirsiniz. Örneğin, 10 MB'lık dosyası aktarma Azure Storage maliyetinin yanı sıra iki ileti ücret kesilir. |
| Doğrudan yöntemler | Başarılı yöntem isteği 4 KB öbekler ücretlendirilen, boş olmayan gövdeleri yanıtları 4 KB ek iletiler sizden ücret kesilir. Bağlantısı kesilmiş aygıtları isteklerine 4 KB öbekler iletilerinde olarak ücretlendirilirsiniz. Örneği için hiçbir gövde ile bir yanıt aygıttan sonuçlanan 6-KB gövde yöntemiyle seçili iki ileti; bir 1 KB aygıttan yanıt gelmesi sonuçlanan 6-KB gövde ile bir yöntem olarak istek için iki ileti artı başka bir ileti yanıtı için ücret kesilir. |
| Cihaz ikizi okumaları | Cihaz çifti cihaz ve çözüm arka uç 512 baytlık öbekleri iletilerinde olarak ücretlendirilirsiniz okur. Örneğin, 6-KB cihaz çifti okuma 12 iletileri ücret kesilir. |
| Cihaz çifti güncelleştirmeleri (etiketleri ve özellikleri) | Cihaz çifti güncelleştirmeleri cihaz ve çözüm arka ucu olarak 512 baytlık öbekleri iletilerinde sizden ücret kesilir. Örneğin, 6-KB cihaz çifti okuma 12 iletileri ücret kesilir. |
| Cihaz çifti sorguları | Sorgu sonuç boyutunu 512 baytlık parçalar bağlı olarak iletileri olarak ücretlendirilirsiniz. |
| İş işlemleri <br/> (oluşturma, güncelleştirme, listeleme, silme) | Ücret işlenmedi. |
| İşlerini aygıt başına işlem | İşlerini işlemleri (örneğin, cihaz çifti güncelleştirmeleri ve yöntemleri) normal olarak sizden ücret kesilir. Örneğin, 1 KB isteklerini ve yanıtlarını gövdesi boş olan 1000 yöntem çağrılarını sonuçta bir işi 1000 iletileri ücretlendirilir. |

> [!NOTE]
> Tüm boyutları yükü boyutu (Protokolü çerçeveleme göz ardı edilir) bayt dikkate hesaplanır. (Olan özellikleri ve gövde) iletileri durumunda boyutu bir protokol belirsiz şekilde açıklandığı gibi hesaplanır [IOT Hub Geliştirici Kılavuzu Mesajlaşma][lnk-message-size].

## <a name="example-1"></a>Örnek #1

Bir aygıt dakika başına tek bir 1 KB cihaz bulut ileti sonra Azure akış analizi tarafından okunur IOT Hub'ına gönderir. Çözüm arka ucu bir yöntemle (512 baytlık yükü) belirli bir eylemi tetiklemek için on dakikada cihazda çağırır. Cihaz 200 bayt sonucunu yöntemiyle yanıt verir.

Cihaz 1 ileti tüketir * 1440 iletileri cihaz bulut iletilerini için günlük ve 2 istek ve yanıt = 60 dakika * 24 saat * 6 kereye saat başına * 24 saat = 288 iletileri 1728 iletileri gün başına toplam yöntemleri için.

## <a name="example-2"></a>Örnek #2

Bir aygıt her saat bir 100 KB cihaz bulut iletisi gönderir. Ayrıca, cihaz çifti 4 saatte bir 1 KB yükü ile güncelleştirir. Çözüm arka sona kez günde, 14-KB cihaz çifti okur ve yapılandırmaları değiştirmek için 512 baytlık yükü ile güncelleştirir.

Cihaz 25 (100 KB/4 KB) iletileri tüketir * cihaz bulut iletilerini yanı sıra, 2 mesaj (1KB/0,5 KB), 24 saat * 6 kereye günde cihaz çifti güncelleştirmeleri için günde 612 iletilerin toplam.
Çözüm arka ucu, 29 iletilerin toplam cihaz çifti okumak için 28 iletileri (14 KB/0,5 KB) yanı sıra, güncelleştirme 1 ileti tüketir.

Toplam, aygıt ve çözüm arka ucu günde 641 iletileri kullanabilir.


[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-message-size]: iot-hub-devguide-messages-construct.md
