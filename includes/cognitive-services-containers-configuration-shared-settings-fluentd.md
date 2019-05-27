---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/02/2019
ms.openlocfilehash: ffd17f7a641e1481aa4c88f8b2eb12ec11fa7d8b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66116705"
---
Bir açık kaynak veri toplayıcı birleşik günlük kaydı için Fluentd olur. `Fluentd` Ayarlarını yönetmek için kapsayıcının bağlantı bir [Fluentd](https://www.fluentd.org) sunucusu. Kapsayıcı günlüklerini yazma izni veren bir Fluentd oturum açma sağlayıcısı kapsayıcı içerir ve isteğe bağlı olarak ölçüm verileri Fluentd sunucusuna.

Aşağıdaki tabloda altında desteklenen yapılandırma ayarları açıklanmaktadır `Fluentd` bölümü.

| Ad | Veri türü | Açıklama |
|------|-----------|-------------|
| `Host` | Dize | Fluentd sunucusunun DNS ana bilgisayar adını veya IP adresi. |
| `Port` | Tamsayı | Fluentd sunucusunun bağlantı noktası.<br/> 24224 varsayılan değerdir. |
| `HeartbeatMs` | Tamsayı | Milisaniye cinsinden sinyal aralığı. Bu aralığı süresi dolmadan önce olay trafik gönderildi, bir sinyal Fluentd sunucuya gönderilir. 60000 milisaniye (1 dakika) varsayılan değerdir. |
| `SendBufferSize` | Tamsayı | Gönderme işlemleri için ayrılan bayt cinsinden ağ arabellek alanı. 32768 bayt (32 kilobayt) varsayılan değerdir. |
| `TlsConnectionEstablishmentTimeoutMs` | Tamsayı | Zaman aşımı, Fluentd sunucusuyla bir SSL/TLS bağlantı kurmak için milisaniye cinsinden. 10000 milisaniye (10 saniye) varsayılan değerdir.<br/> Varsa `UseTLS` yanlış olarak bu değeri yok sayıldı ayarlanmış. |
| `UseTLS` | Boole | Kapsayıcı Fluentd sunucusu ile iletişim kurmak için SSL/TLS kullanıp kullanmayacağını belirtir. Varsayılan değer false'tur. |