---
title: "Azure çok faktörlü kimlik doğrulaması - nasıl çalışır?"
description: "Azure Multi-Factor Authentication, kullanıcıların basit bir oturum açma işlemi taleplerini karşılarken, verilere ve uygulamalara erişimi korumaya da yardımcı olur. İkinci bir form kimlik doğrulama isteyerek ek güvenlik sağlar ve bir dizi kolay doğrulama seçeneklerini aracılığıyla güçlü kimlik doğrulaması sunar."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d14db902-9afe-4fca-b3a5-4bd54b3d8ec5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 6fee02885cc76b3a4fdad11e8702f623d6fe6597
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-azure-multi-factor-authentication-works"></a>Azure multi-Factor Authentication nasıl çalışır
İki aşamalı doğrulama güvenlik, katmanlı yaklaşımın arasındadır. Birden çok kimlik doğrulama faktörleri tehlikeye saldırganlar için önemli bir sınama gösterir. Bir saldırgan kullanıcı parolasının öğrenmek bile yönetiliyorsa, bu da güvenilen cihaza sahip gerek kalmadan gereksizdir. 

![Proofup](./media/multi-factor-authentication-how-it-works/howitworks.png)

Azure Multi-Factor Authentication, kullanıcıların basit bir oturum açma işlemi taleplerini karşılarken, verilere ve uygulamalara erişimi korumaya da yardımcı olur.  İkinci bir form kimlik doğrulama isteyerek ek güvenlik sağlar ve bir dizi kolay doğrulama seçeneklerini aracılığıyla güçlü kimlik doğrulaması sunar.


## <a name="methods-available-for-two-step-verification"></a>İki aşamalı doğrulama için kullanılabilen yöntemler
Bir kullanıcı oturum açtığında ek doğrulama kullanıcıya gönderilir.  Bu ikinci doğrulama için kullanılan yöntemleri listesi verilmiştir.

| Doğrulama yöntemi | Açıklama |
| --- | --- |
| Telefon araması |Çağrı, bir kullanıcının kayıtlı telefon yerleştirilir. Gerekli ardından # tuşuna bastığında, kullanıcı bir PIN girer. |
| Kısa mesaj |Kısa mesaj kullanıcı cep telefonu altı rakamlı bir kod ile gönderilir. Kullanıcı oturum açma sayfasında bu kodu girer. |
| Mobil uygulama bildirimi |Bir kullanıcının Akıllı telefonu doğrulama isteği gönderilir. Kullanıcı bir PIN gerekli olduğunda girer sonra seçer **doğrula** mobil uygulama üzerinde. |
| Mobil uygulama doğrulama kodu |Bir kullanıcının Akıllı telefonu üzerinde çalıştığından, mobil uygulama değişiklikleri her 30 saniyede bir doğrulama kodu görüntüler. Kullanıcı en son kod bulur ve oturum açma sayfasında girer. |
| Üçüncü taraf OATH belirteçleri | Azure multi-Factor Authentication sunucusu, üçüncü taraf doğrulaması yöntemlerini kabul edecek şekilde yapılandırılabilir. |

Azure çok faktörlü kimlik doğrulaması, hem bulut hem de sunucu için seçilebilir doğrulama yöntemlerini sağlar. Hangi yöntemler, kullanıcılarınız için kullanılabilir seçebilirsiniz: telefon araması, metin, uygulama bildirimi veya uygulama kodları. Daha fazla bilgi için bkz: [seçilebilir doğrulama yöntemlerini](multi-factor-authentication-whats-next.md#selectable-verification-methods).

## <a name="next-steps"></a>Sonraki adımlar

- Farklı hakkında okuyun [sürümleri ve Azure çok faktörlü kimlik doğrulaması için tüketim yöntemi](multi-factor-authentication-versions-plans.md)

- Azure MFA dağıtmak isteyip istemediğinizi seçin [bulutta veya şirket içi](multi-factor-authentication-get-started.md)

- Okuma yanıtlar için [sık sorulan sorular](multi-factor-authentication-faq.md)