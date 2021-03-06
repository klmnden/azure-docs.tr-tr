---
title: Azure AD uygulama ara sunucusu yükseltme | Microsoft Docs
description: Microsoft Forefront ya da birleşik erişim ağ geçidi yükseltme yapıyorsanız en iyi hangi Ara sunucu çözümünü seçin.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/17/2019
ms.author: mimart
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4790dc7ebeeee3407e89bcf38d7e3f25699ed328
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67108420"
---
# <a name="compare-remote-access-solutions"></a>Uzaktan erişim çözümlerini karşılaştırın

Azure Active Directory Uygulama proxy'si, Microsoft'un sunduğu iki uzaktan erişim çözümlerinin biridir. Diğer Web uygulaması Ara sunucusu, şirket içi sürümü olan. Bu iki çözümleri Microsoft sunulan önceki ürünlerden değiştirin: Microsoft Forefront Threat Yönetimi ağ geçidi (TMG) ve birleşik erişim ağ geçidi (UAG). Bu dört çözümlerini birbirine nasıl karşılaştırın anlamak için bu makaleyi kullanın. Bu, yine de kullanım dışı TMG veya UAG'den çözümleri kullanarak bir uygulama proxy'si için geçişiniz planlamanıza yardımcı olacak bu makaleyi kullanın. 


## <a name="feature-comparison"></a>Özellik karşılaştırması

Birbirine nasıl tehdit Yönetimi ağ geçidi (TMG), birleşik erişim ağ geçidi (UAG), Web uygulaması Ara sunucusu (WAP) ve Azure AD uygulama ara sunucusu (AP) karşılaştırma anlamak için bu tabloyu kullanın.

| Özellik | TMG | UAG | WAP | AP |
| ------- | --- | --- | --- | --- |
| Sertifika kimlik doğrulaması | Evet | Evet | - | - |
| Tarayıcı uygulamaları seçmeli olarak Yayımla | Evet | Evet | Evet | Evet |
| Ön kimlik doğrulaması ve çoklu oturum açma | Evet | Evet | Evet | Evet | 
| Katman 2/3 güvenlik duvarı | Evet | Evet | - | - |
| İletim proxy'si özellikleri | Evet | - | - | - |
| VPN özellikleri | Evet | Evet | - | - |
| Zengin protokol desteği | - | Evet | Evet, HTTP üzerinden çalışıyorsa | Evet, HTTP üzerinden veya Uzak Masaüstü Ağ Geçidi aracılığıyla çalışıyorsa |
| ADFS Ara sunucu olarak görev yapar | - | Evet | Evet | - |
| Uygulama erişimi için bir portal | - | Evet | - | Evet |
| Yanıt gövdesi bağlantı çeviri | Evet | Evet | - | Evet | 
| Üst bilgileri ile kimlik doğrulaması | - | Evet | - | Evet, pingaccess | 
| Bulut ölçeğinde güvenlik | - | - | - | Evet | 
| Koşullu Erişim | - | Evet | - | Evet |
| Bileşen (DMZ) arındırılmış bölge içinde yok | - | - | - | Evet |
| Gelen bağlantı yok | - | - | - | Evet |

Çoğu senaryo için modern bir çözüm olarak Azure AD uygulama proxy'si öneririz. Web uygulama proxy'si için AD FS proxy sunucusu gerektiren, tercih edilen senaryolar, ve Azure Active Directory'de özel etki alanları kullanamazsınız. 

Azure AD uygulama ara sunucusu da dahil olmak üzere benzer ürünleri karşılaştırıldığında benzersiz avantajları sunar:

- Şirket içi kaynaklarınızı Azure AD'ye genişletme
   - Bulut ölçeğinde güvenlik ve koruma
   - Koşullu erişim ve çok faktörlü kimlik doğrulaması gibi özellikleri etkinleştirmek kolaydır
- Sivil bölge içinde bileşen yok
- Gerekli olan herhangi bir gelen bağlantı
- Bir erişim panelinde kullanıcılarınız O365 dahil olmak üzere tüm uygulamalar için gidebilir, Azure AD tümleşik SaaS uygulamaları ve şirket içi web uygulamaları. 


## <a name="next-steps"></a>Sonraki adımlar

- [Şirket içi uygulamalara güvenli uzaktan erişim sağlamak için Azure AD uygulamasını kullanın](application-proxy.md)
