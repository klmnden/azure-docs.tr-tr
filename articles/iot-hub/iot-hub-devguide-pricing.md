---
title: Azure IOT Hub fiyatlandırma anlama | Microsoft Docs
description: Geliştirici Kılavuzu - ölçüm ve de dahil olmak üzere IOT Hub ile works fiyatlandırma örnekler nasıl çalıştığı hakkında bilgi.
services: iot-hub
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 1ac90923-1edf-4134-bbd4-77fee9b68d24
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: 9b0d2df078c59c7d261fd3231450ddfb2fdcd88e
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="azure-iot-hub-pricing-information"></a>Azure IOT Hub ile fiyatlandırma bilgileri

[Azure IOT Hub ile fiyatlandırma] [ lnk-pricing] farklı SKU'ları ve IOT Hub için fiyatlandırma hakkında genel bilgiler sağlar. Bu makale nasıl çeşitli IOT hub'ı işlevler iletileri olarak IOT Hub tarafından ölçülen ek ayrıntılar içerir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="charges-per-operation"></a>İşlem başına ücret

| İşlem | Faturalama bilgileri | 
| --------- | ------------------- |
| Kimlik kayıt defteri işlemleri <br/> (oluşturma, alma, liste, Güncelleştir, Sil) | Ücret işlenmedi. |
| Cihazdan buluta iletiler | IOT Hub'ına, 4 KB öbekler giriş üzerinde başarıyla gönderilen iletileri ücretlendirilirsiniz. Örneğin, 6-KB ileti 2 mesaj doludur. |
| Bulut-cihaz iletilerini | Başarıyla gönderilen iletileri 4 KB öbekler ücretlendirilen, 6-KB ileti 2 mesaj örneğin doludur. |
| Dosya yüklemeleri | Azure depolama birimine dosya aktarımı IOT Hub tarafından ölçülen değil. Dosya aktarımı başlatma ve tamamlanma iletilerini 4 KB artımlarla ölçülen messaged gibi ücretlendirilirsiniz. Örneğin, 10 MB'lık dosyası aktarma Azure Storage maliyetinin yanı sıra iki ileti doludur. |
| Doğrudan yöntemler | Başarılı yöntem isteği 4 KB öbekler ücretlendirilen, boş olmayan gövdeleri yanıtları 4 KB öbekler ek iletiler sizden ücret kesilir. Bağlantısı kesilmiş aygıtları isteklerine 4 KB öbekler iletilerinde olarak ücretlendirilirsiniz. Örneğin, aygıt hiçbir gövde ile bir yanıt sonuçlanan 6-KB gövde yöntemiyle seçili iki ileti. Bir 1 KB aygıttan yanıt gelmesi sonuçlanan 6-KB gövde ile bir yöntem olarak istek için iki ileti artı başka bir ileti yanıtı için ücret kesilir. |
| Aygıt ve modülü twin okur | Son olarak 512 baytlık öbekleri iletilerinde ücretlendirilirsiniz aygıt ya da modül ve çözüm arka Twin okur. Örneğin, 6-KB twin okuma 12 iletileri doludur. |
| Cihaz ve modül twin güncelleştirmeleri (etiketleri ve özellikleri) | Twin güncelleştirmeleri cihaz veya modülü ve çözüm arka ucu olarak 512 baytlık öbekleri iletilerinde sizden ücret kesilir. Örneğin, 6-KB twin okuma 12 iletileri doludur. |
| Aygıt ve modül twin sorguları | Sorgu sonuç boyutunu 512 baytlık parçalar bağlı olarak iletileri olarak ücretlendirilirsiniz. |
| İş işlemleri <br/> (oluşturma, güncelleştirme, listeleme, silme) | Ücret işlenmedi. |
| İşlerini aygıt başına işlem | İşlerini işlemleri (örneğin, twin güncelleştirmeleri ve yöntemleri) normal olarak sizden ücret kesilir. Örneğin, 1 KB isteklerini ve yanıtlarını gövdesi boş olan 1000 yöntem çağrılarını sonuçta bir işi 1000 iletileri doludur. |

> [!NOTE]
> Tüm boyutları yükü boyutu (Protokolü çerçeveleme göz ardı edilir) bayt dikkate hesaplanır. Özellikler ve gövde sahip, iletilerde boyutu Protokolü belirsiz şekilde hesaplanır. Daha fazla bilgi için bkz: [IOT Hub Geliştirici Kılavuzu Mesajlaşma][lnk-message-size].

## <a name="example-1"></a>Örnek #1

Bir aygıt dakika başına tek bir 1 KB cihaz bulut ileti sonra Azure akış analizi tarafından okunur IOT Hub'ına gönderir. Çözüm arka ucu bir yöntemle (512 baytlık yükü) belirli bir eylemi tetiklemek için 10 dakikada bir aygıtta çağırır. Cihaz 200 bayt sonucunu yöntemiyle yanıt verir.

Aygıt kullanır:

* Bir ileti * 60 dakika * 24 saat = cihaz bulut iletilerini için günlük 1440 iletileri.
* İki istek yanıt * 6 kereye saat başına * 24 saat = 288 iletileri için yöntemleri.

Bu hesaplama 1728 iletileri günde toplamını verir.

## <a name="example-2"></a>Örnek #2

Bir aygıt her saat bir 100 KB cihaz bulut iletisi gönderir. Ayrıca, cihaz çifti dört saatte bir 1 KB yükü ile güncelleştirir. Çözüm arka sona kez günde, 14-KB cihaz çifti okur ve yapılandırmaları değiştirmek için 512 baytlık yükü ile güncelleştirir.

Aygıt kullanır:

* 25 (100 KB/4 KB) iletileri * cihaz bulut iletilerini 24 saattir.
* İki ileti (1 KB/0,5 KB) * altı kat günde cihaz çifti güncelleştirmeleri için.

Bu hesaplama günde 612 iletileri toplamını verir.

Çözüm arka ucu, 29 iletilerin toplam cihaz çifti okumak için 28 iletileri (14 KB/0,5 KB) yanı sıra, güncelleştirmek için bir ileti tüketir.

Toplam, aygıt ve çözüm arka ucu günde 641 iletileri kullanabilir.


[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-message-size]: iot-hub-devguide-messages-construct.md
