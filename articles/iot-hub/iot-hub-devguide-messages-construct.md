---
title: "Azure IOT Hub ileti biçimi anlama | Microsoft Docs"
description: "Geliştirici Kılavuzu - descibes biçimi ve IOT hub'ı iletilerinin beklenen içerik."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3fc5f1a3-3711-4611-9897-d4db079b4250
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: b88567616e0a8c46494ae0af367f4deb4506be43
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="create-and-read-iot-hub-messages"></a>Oluşturun ve IOT hub'ı iletileri okur

Protokoller kullanılarak sorunsuz birlikte çalışabilirlik desteklemek için tüm cihaz dönük protokoller için ortak bir ileti biçimi IOT hub'ı tanımlar. Bu ileti biçimi her ikisi için kullanılan [cihaz bulut] [ lnk-d2c] ve [bulut cihaz] [ lnk-c2d] iletileri. Bir [IOT hub'ı ileti] [ lnk-messaging] oluşur:

* Bir dizi *Sistem Özellikleri*. IOT hub'ı yorumlar veya ayarlayan özellikleri. Bu önceden belirlenmiş kümesidir.
* Bir dizi *uygulama özellikleri*. Uygulama tanımlayabilirsiniz dize özellikleri ve ileti gövdesi seri durumdan gerek olmadan erişim, sözlüğü. IOT hub'ı hiçbir zaman bu özellikleri değiştirir.
* Donuk bir ikili gövdesi.

Özellik adları ve değerleri yalnızca ASCII alfasayısal karakterler içerebilirler, artı ``{'!', '#', '$', '%, '&', "'", '*', '+', '-', '.', '^', '_', '`', '|', '~'}`` olduğunda:  

* HTTPS protokolünü kullanarak cihaz-bulut iletileri gönderir.
* Bulut cihaz iletileri gönderir.

Kodlamak ve farklı protokoller kullanılarak gönderilen iletileri kod çözme hakkında daha fazla bilgi için bkz: [Azure IOT SDK'ları][lnk-sdks].

Aşağıdaki tabloda, IOT hub'ı iletileri Sistem özelliklerinde kümesini listeler.

| Özellik | Açıklama |
| --- | --- |
| MessageID |İstek-yanıt desenler için kullanılan ileti için kullanıcı ayarlanabilir bir tanımlayıcı. Biçimi: Büyük küçük harfe duyarlı dizesi (en fazla 128 karakter uzunluğunda) ASCII 7 bit alfasayısal karakter + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| Sıra numarası |Her bulut cihaz iletiye IOT Hub tarafından atanan bir sayı (aygıt sırası benzersiz). |
| Alıcı |Belirtilen bir hedef [bulut cihaz] [ lnk-c2d] iletileri. |
| ExpiryTimeUtc |Tarih ve saat iletisi süre sonu. |
| EnqueuedTime |Tarih ve saat [bulut cihaz] [ lnk-c2d] ileti IOT Hub tarafından alındı. |
| CorrelationId |Bir dize özelliği genellikle istek, istek-yanıt desenleri MessageID içeren bir yanıt iletisi. |
| Kullanıcı Kimliği |İletileri kökeni belirtmek için kullanılan bir kimliği. IOT Hub tarafından iletileri oluşturulduğunda ayarlanır `{iot hub name}`. |
| ACK |Geri bildirim iletisi üreteci. Bu özellik bulut cihaz iletilerinde aygıt tarafından tüketilen iletinin sonucunda geri bildirim iletileri oluşturmak için IOT Hub istemek için kullanılır. Olası değerler: **hiçbiri** (varsayılan): hiçbir geri bildirim iletisi oluşturulur, **pozitif**: ileti tamamlanmışsa bir geri bildirim iletisi **negatif**: alma bir iletinin zaman aşımına (veya en yüksek teslimat sayısı ulaşıldı varsa) aygıtıyla tamamlandığı olmadan geri bildirim iletisi veya **tam**: pozitif ve negatif. Daha fazla bilgi için bkz: [ileti geri bildirim][lnk-feedback]. |
| ConnectionDeviceId |IOT Hub tarafından cihaz bulut iletilerini üzerinde ayarlanmış bir kimliği. İçerdiği **DeviceID** iletiyi gönderen cihaz. |
| ConnectionDeviceGenerationId |IOT Hub tarafından cihaz bulut iletilerini üzerinde ayarlanmış bir kimliği. İçerdiği **Generationıd** (göre [aygıt kimlik özellikleri][lnk-device-properties]) iletiyi gönderen cihaz. |
| ConnectionAuthMethod |IOT Hub tarafından cihaz bulut iletilerini üzerinde ayarlanmış bir kimlik doğrulama yöntemi. Bu özellik, iletiyi göndermeyi cihazın kimliğini doğrulamak için kullanılan kimlik doğrulama yöntemi hakkında bilgi içerir. Daha fazla bilgi için bkz: [yanıltma bulut aygıta][lnk-antispoofing]. |

## <a name="message-size"></a>İleti boyutu

IOT hub'ı yalnızca gerçek yükü dikkate Protokolü belirsiz şekilde, ileti boyutu ölçer. Bayt cinsinden boyutu toplamı aşağıdaki olarak hesaplanır:

* Gövde boyutu bayt.
* İleti sistemi özelliklerini tüm değerlerin bayt cinsinden boyutu.
* Tüm kullanıcı özellik adları ve değerleri bayt cinsinden boyutu.

Dolayısıyla bayt cinsinden boyutu dize uzunluğu eşittir özellik adları ve değerleri ASCII karakter ile sınırlıdır.

## <a name="next-steps"></a>Sonraki adımlar

IOT Hub'ındaki ileti boyutu sınırları hakkında daha fazla bilgi için bkz: [IOT hub'ı kotalar ve azaltma][lnk-quotas].

Oluşturun ve IOT hub'ı çeşitli programlama dillerinde iletileri okumak öğrenmek için bkz: [başlama] [ lnk-get-started] öğreticileri.

[lnk-messaging]: iot-hub-devguide-messaging.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-get-started]: iot-hub-get-started.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-feedback]: iot-hub-devguide-messages-c2d.md#message-feedback
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-antispoofing]: iot-hub-devguide-messages-d2c.md#anti-spoofing-properties
