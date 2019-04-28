---
author: WenJason
ms.service: dns
ms.topic: include
origin.date: 11/25/2018
ms.date: 12/24/2018
ms.author: v-jay
ms.openlocfilehash: 9cc650cea17acb8d89933c819c4ca60e2c459d1c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60563373"
---
Gönderen ilke framework (SPF) kayıtları hangi e-posta sunucuları bir etki alanı adı adına e-posta gönderebilir belirtmek için kullanılır. Alıcılara e-postanızı önemsiz olarak işaretlemenizi önlemek SPF kayıtlarının doğru yapılandırılması önemlidir.

DNS RFC'ler ilk olarak bu senaryoyu desteklemek için yeni bir SPF kaydını türü kullanıma sunuldu. Eski ad sunucularını desteklemek için bunlar da SPF kayıtlarının TXT kaydı türü kullanımını izin. Bu belirsizlik işlemler sırasında LED'i tarafından çözümlendi Karışıklığı önlemek için [RFC 7208](http://tools.ietf.org/html/rfc7208#section-3.1). Bu, SPF kayıtlarının TXT kaydı türü kullanılarak oluşturulması gerektiğini belirtir. SPF kayıt türü kullanım dışı olduğunu belirtir.

**SPF kayıtlarının TXT kaydı türü kullanılarak oluşturulması gerekir ve Azure DNS tarafından desteklenir.** Eski bir SPF kaydını türü desteklenmiyor. Olduğunda, [DNS bölge dosyasını içeri aktarma](../articles/dns/dns-import-export.md), SPF kaydı türü kullanmaktadır herhangi bir SPF kayıtlarının TXT kayıt türüne dönüştürülür.
