---
title: Bildirim hub'ları için güvenlik
description: Bu konu Azure bildirim hub'ları için güvenlik açıklar.
services: notification-hubs
documentationcenter: .net
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 6506177c-e25c-4af7-8508-a3ddca9dc07c
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 9f197a85dfad31ce32d0f9c93127b69d8e33c9ee
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="security"></a>Güvenlik
## <a name="overview"></a>Genel Bakış
Bu konuda, Azure Notification Hubs'un güvenlik modeli açıklanır. Bildirim hub'ları Service Bus varlık olduğundan, bunlar aynı güvenlik modeli hizmet veri yolu olarak uygular. Daha fazla bilgi için bkz: [hizmet veri yolu kimlik doğrulaması](https://msdn.microsoft.com/library/azure/dn155925.aspx) Konular.

## <a name="shared-access-signature-security-sas"></a>Paylaşılan erişim imzası güvenlik (SAS)
Bildirim hub'ları bir varlık düzeyinde güvenlik şeması uygulayan SAS (paylaşılan erişim imzası) olarak adlandırılır. Bu düzen varlık hakları kendi açıklaması en fazla 12 yetkilendirme kuralları bildirmek Mesajlaşma varlıkları sağlar.

"Güvenlik taleplerini." bölümünde açıklandığı gibi her bir kural bir ad, bir anahtar değeri (paylaşılan gizliliği) ve hakları, bir dizi içeriyor Bildirim hub'ı oluştururken, iki kuralları otomatik olarak oluşturulur: (yani istemci uygulamanın kullandığı) dinleme haklarıyla diğeri (uygulama arka ucu kullanan) tüm haklarına sahip.

Kayıt Yönetimi aracılığıyla gönderilen bilgiler, istemci uygulamalardan gerçekleştirirken bildirimleri (örneğin, hava durumu güncelleştirmelerini) duyarlı değil, bir bildirim hub'ı erişmek için genel bir kural yalnızca dinleme erişimi anahtar değeri istemci uygulamasına vermek için yoludur, ve kural tam erişim anahtar değeri uygulama arka ucuna vermek için.

Windows mağazası istemci uygulamalarında anahtar değeri katıştırmak önerilmez. Anahtar değeri katıştırma önlemek için bir yol uygulama arka ucu başlangıçta almak istemci uygulaması sağlamaktır.

Anahtar dinleme erişimi olan herhangi bir etiket kaydetmek bir istemci uygulaması izin verdiğini anlamak önemlidir. Uygulamanız için özel etiketler (örneğin, kullanıcı kimliklerini etiketleri temsil ettiğinde) belirli istemcilere kayıtlar kısıtlamanız gerekiyorsa, uygulamanızın arka ucuna kayıtlar gerçekleştirmeniz gerekir. Daha fazla bilgi için bkz: kayıt yönetimi. Bu şekilde, istemci uygulaması bildirim hub'ları doğrudan erişim sahip olmadığını unutmayın.

## <a name="security-claims"></a>Güvenlik taleplerini
Benzer diğer varlıklar için bildirim hub'ı işlemleri için üç güvenlik taleplerini izin verilir: dinleme, Gönder ve Yönet.

| İste | Açıklama | İzin verilen işlemler |
| --- | --- | --- |
| Dinle |Oluştur/güncelleştir, okuma ve tek kayıtları silme |Kayıt oluştur/güncelleştir<br><br>Kayıt okuma<br><br>Tüm kayıtlar için bir tanıtıcı okuma<br><br>Kayıt silme |
| Gönder |Bildirim hub'ına iletileri gönder |İleti gönder |
| Yönetme |Bildirim hub'ları (PNS kimlik bilgileri ve güvenlik anahtarları güncelleştirme dahil) ve etiketlere göre okuma kayıtlar cRUDs |Oluşturma/güncelleştirme/okuma/silme bildirim hub'ları<br><br>Etikete göre kayıtları oku |

Bildirim hub'ları doğrudan bildirim hub'ına yapılandırılan paylaşılan anahtarlar ile oluşturulan imza belirteçleri ve Microsoft Azure erişim denetimi belirteçleri tarafından verilen bir talep kabul edin.

