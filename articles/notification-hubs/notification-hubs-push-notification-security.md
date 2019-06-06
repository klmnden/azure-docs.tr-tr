---
title: Bildirim hub'ları güvenlik
description: Bu konuda Azure bildirim hub'ları için güvenlik açıklanmaktadır.
services: notification-hubs
documentationcenter: .net
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 6506177c-e25c-4af7-8508-a3ddca9dc07c
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 05/31/2019
ms.author: jowargo
ms.openlocfilehash: 3f5b23028094b545262e9c01640890f2c0b989ca
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66431243"
---
# <a name="notification-hubs-security"></a>Bildirim hub'ları güvenlik

## <a name="overview"></a>Genel Bakış

Bu konuda, Azure Notification Hubs'ın güvenlik modeli açıklanır.

## <a name="shared-access-signature-security-sas"></a>Paylaşılan erişim imzası güvenlik (SAS)

Notification hubs'ı uygulayan bir varlık düzeyinde güvenlik şeması SAS (paylaşılan erişim imzası) çağrılır. Bu düzen, varlık üzerinde hakları 12 adede kadar yetkilendirme kuralları, bunların açıklaması bildirmek Mesajlaşma varlıkları sağlar.

Her kural açıklandığı gibi bir ad, bir anahtar değeri (paylaşılan gizli) ve hakları, bir dizi içeren [güvenlik talepleri](#security-claims). Bildirim hub'ı oluştururken, iki kuralları otomatik olarak oluşturulur: biriyle **dinleme** (istemci uygulamanın kullandığı) hakları ve bir **tüm** (uygulama arka ucu kullanan) hakları.

Kayıt Yönetimi aracılığıyla gönderilen bilgiler, istemci uygulamalardan gerçekleştirirken bildirimleri (örneğin, hava durumu güncelleştirmelerini) hassas değil, istemci uygulamaya anahtar değeri Kuralın yalnızca dinleme erişim vermek için bir bildirim hub'ı erişmek için yaygın bir yolu olan, ve uygulama arka ucu için anahtar değeri kural tam erişim vermek için.

Uygulamaları değil Windows Store istemci uygulamalara anahtar değeri ekleme, bunun yerine bir uygulama arka ucundan başlangıçta almak istemci uygulamanız.

Anahtar ile **dinleme** erişim için herhangi bir etiket kaydetmek bir istemci uygulaması sağlar. Uygulamanız için belirli istemciler (örneğin, kullanıcı kimliklerini etiketleri göstermek) için belirli etiketlere kayıtları kısıtlamanız gerekiyorsa, uygulamanızın arka ucunu kayıtları gerçekleştirmeniz gerekir. Daha fazla bilgi için [kayıt yönetimi](notification-hubs-push-notification-registration-management.md). Bu şekilde, istemci uygulaması bildirim hub'ları doğrudan erişimi unutmayın.

## <a name="security-claims"></a>Güvenlik talepleri

Bildirim hub'ı işlemlerine izin için üç güvenlik taleplerini diğer varlıklara benzer şekilde: **Dinleme**, **Gönder**, ve **yönetme**.

| İste   | Açıklama                                          | İzin verilen işlemleri |
| ------- | ---------------------------------------------------- | ------------------ |
| Dinle  | Oluşturma/güncelleştirme, okuma ve tek kayıtları silme | Kayıt oluşturma/güncelleştirme<br><br>Kayıt okuma<br><br>Tüm kayıtlar için bir tanıtıcı okuyun<br><br>Kaydı Sil |
| Gönder    | Bildirim hub'ına ileti gönderme                | İleti Gönder |
| Yönetme  | Notification hubs'ı (PNS kimlik bilgilerini ve güvenlik anahtarları güncelleştirme dahil) ve okuma kayıtları etiketlere göre cRUDs |Oluşturma/güncelleştirme/okuma/silme bildirim hub'ları<br><br>Etikete göre kayıtlar okuyun |

Paylaşılan doğrudan bildirim Hub'ındaki yapılandırılmış anahtarlar notification Hubs ile belirteçleri oluşturan imza kabul eder.

Birden fazla ad alanı için bir bildirim göndermesini mümkün değildir. Ad alanları bildirim hub'ları için mantıksal kapsayıcı ve bildirimleri gönderme ile söz konusu değildir.
Ad alanı düzeyinde erişim ilkeleri (kimlik) için ad alanı düzeyinde işlemler, örneğin kullanılabilir: bildirim hub'larını listeleme, oluşturma veya bildirim hub'ları silme, vb. Hub'ı düzeyinde erişim ilkeleri yalnızca bildirimleri göndermenizi sağlar.
