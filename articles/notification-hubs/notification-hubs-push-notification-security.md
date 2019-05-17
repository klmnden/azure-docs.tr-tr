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
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 22494984ca45cde7255fb5e1a30548c859bfad68
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65826508"
---
# <a name="security-model-of-azure-notification-hubs"></a>Azure Notification Hubs'ın güvenlik modeli

## <a name="overview"></a>Genel Bakış

Bu konuda, Azure Notification Hubs'ın güvenlik modeli açıklanır. Notification Hubs, Service Bus varlık olduğundan, bunlar aynı güvenlik modeli olarak hizmet veri yolu uygulayın. Daha fazla bilgi için [hizmet veri yolu kimlik doğrulaması](https://msdn.microsoft.com/library/azure/dn155925.aspx) konuları.

## <a name="shared-access-signature-security-sas"></a>Paylaşılan erişim imzası güvenlik (SAS)

Notification hubs'ı uygulayan bir varlık düzeyinde güvenlik şeması SAS (paylaşılan erişim imzası) çağrılır. Bu düzen, varlık üzerinde hakları 12 adede kadar yetkilendirme kuralları, bunların açıklaması bildirmek Mesajlaşma varlıkları sağlar.

Her kural "Güvenlik talepleri" bölümünde açıklandığı gibi bir ad, bir anahtar değeri (paylaşılan gizli) ve hakları, bir dizi içeriyor Bildirim hub'ı oluştururken, iki kuralları otomatik olarak oluşturulur: biri (istemci uygulamanın kullandığı) dinleme haklarına sahip ve diğeri (uygulama arka ucu kullanan) tüm haklara sahip.

Kayıt Yönetimi aracılığıyla gönderilen bilgiler, istemci uygulamalardan gerçekleştirirken bildirimleri (örneğin, hava durumu güncelleştirmelerini) hassas değil, istemci uygulamaya anahtar değeri Kuralın yalnızca dinleme erişim vermek için bir bildirim hub'ı erişmek için yaygın bir yolu olan, ve uygulama arka ucu için anahtar değeri kural tam erişim vermek için.

Anahtar değerini Windows Store istemci uygulamalara ekleme önerilmez. Anahtar değeri katıştırma önlemek için başlatma sırasında bir uygulama arka ucundan almak istemci uygulaması olmasını sağlamaktır.

Dinleme erişim anahtarıyla kaydetmek için herhangi bir etiket bir istemci uygulaması izin verdiğini anlamak önemlidir. Uygulamanız için belirli istemciler (örneğin, kullanıcı kimliklerini etiketleri göstermek) için belirli etiketlere kayıtları kısıtlamanız gerekiyorsa, uygulamanızın arka ucuna kayıtları gerçekleştirmeniz gerekir. Kayıt yönetimi daha fazla bilgi için bkz. Bu şekilde, istemci uygulaması bildirim hub'ları doğrudan erişimi unutmayın.

## <a name="security-claims"></a>Güvenlik talepleri

Bildirim hub'ı işlemlerine izin için üç güvenlik taleplerini diğer varlıklara benzer şekilde: Dinleme, gönderin ve yönetin.

| Talep   | Açıklama                                          | İzin verilen işlemleri |
| ------- | ---------------------------------------------------- | ------------------ |
| Dinle  | Oluşturma/güncelleştirme, okuma ve tek kayıtları silme | Kayıt oluşturma/güncelleştirme<br><br>Kayıt okuma<br><br>Tüm kayıtlar için bir tanıtıcı okuyun<br><br>Kaydı Sil |
| Gönder    | Bildirim hub'ına ileti gönderme                | İleti gönder |
| Yönetme  | Notification hubs'ı (PNS kimlik bilgilerini ve güvenlik anahtarları güncelleştirme dahil) ve okuma kayıtları etiketlere göre cRUDs |Oluşturma/güncelleştirme/okuma/silme bildirim hub'ları<br><br>Etikete göre kayıtlar okuyun |

Notification hubs'ı Microsoft Azure erişim denetimi belirteçleri ve bildirim Hub'ındaki doğrudan yapılandırılmış paylaşılan anahtar ile oluşturulan imzası belirteçlerinin tarafından verilen bir talep kabul edin.

Birden fazla ad alanı için bir bildirim göndermesini mümkün değildir. Ad alanları bildirim hub'ları için mantıksal kapsayıcı ve bildirimleri gönderme ile söz konusu değildir.
Ad alanı düzeyinde erişim ilkeleri (kimlik) için ad alanı düzeyinde işlemler, örneğin kullanılabilir: bildirim hub'larını listeleme, oluşturma veya bildirim hub'ları silme, vb. Hub'ı düzeyinde erişim ilkeleri yalnızca bildirimleri göndermenizi sağlar.
