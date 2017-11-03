---
title: "Azure AD uygulama proxy'si için yükseltme | Microsoft Docs"
description: "Hangi proxy Microsoft Forefront ya da birleşik erişim ağ geçidi yükseltme yapıyorsanız en iyi çözümdür seçin."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 6c9f70493155de6989b26fd4e8bcf1dff01c835c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="compare-remote-access-solutions"></a>Uzaktan erişim çözümlerini karşılaştırın

Azure Active Directory Uygulama proxy'si Microsoft sunar iki uzaktan erişim çözümlerinin biridir. Diğer Web uygulama proxy'si, şirket içi sürümü olan. Bu iki çözümleri Microsoft sunulan önceki ürün değiştirin: Microsoft Forefront Threat Yönetimi ağ geçidi (TMG) ve birleşik erişim ağ geçidi (UAG). Bu dört çözümlerini birbirleriyle nasıl karşılaştırın anlamak için bu makaleyi kullanın. Bu size kullanım dışı TMG veya UAG çözümlerini kullanmaya devam geçişinizi uygulama proxy'si birine planlamaya yardımcı olması için bu makaleyi kullanın. 


## <a name="feature-comparison"></a>Özellik karşılaştırması

Birbirleriyle nasıl tehdit Yönetimi ağ geçidi (TMG), Unified erişim ağ geçidi (UAG), Web uygulaması Ara sunucusu (WAP) ve Azure AD uygulama proxy'si (AP) karşılaştırmak anlamak için bu tabloyu kullanın.

| Özellik | TMG | UAG | WAP | AP |
| ------- | --- | --- | --- | --- |
| Sertifika kimlik doğrulaması | Evet | Evet | - | - |
| Seçmeli olarak tarayıcı uygulama yayımlama | Evet | Evet | Evet | Evet |
| Ön kimlik doğrulaması ve çoklu oturum açma | Evet | Evet | Evet | Evet | 
| Katman 2/3 güvenlik duvarı | Evet | Evet | - | - |
| İleriye doğru proxy özellikleri | Evet | - | - | - |
| VPN özellikleri | Evet | Evet | - | - |
| Zengin protokol desteği | - | Evet | Evet, HTTP üzerinden çalışıyorsa | Evet, HTTP üzerinden veya Uzak Masaüstü Ağ geçidi üzerinden çalışıyorsa |
| ADFS Ara sunucusu olarak görev yapar | - | Evet | Evet | - |
| Uygulama erişimi için bir portal | - | Evet | - | Evet |
| Yanıt gövdesi bağlantı çevirisi | Evet | Evet | - | Evet | 
| Üst bilgileri ile kimlik doğrulaması | - | Evet | - | Evet, PingAccess ile | 
| Bulut ölçekli güvenlik | - | - | - | Evet | 
| Koşullu erişim | - | Evet | - | Evet |
| Sivil bölge (DMZ) herhangi bir bileşeni | - | - | - | Evet |
| Hiçbir gelen bağlantıları | - | - | - | Evet |

Çoğu senaryoda, modern çözümü olarak Azure AD uygulaması öneririz. Web uygulama proxy'si, AD FS için bir proxy sunucu gerektiren, tercih edilen senaryolar ve Azure Active Directory'de özel etki alanları kullanamazsınız. 

Azure AD uygulama proxy'si de dahil olmak üzere benzer ürünlerle karşılaştırıldığında benzersiz avantajları sunar:

- Azure AD şirket içi kaynaklara genişletme
   - Bulut ölçekli güvenlik ve koruma
   - Koşullu erişim ve çok faktörlü kimlik doğrulaması gibi özellikleri etkinleştirmek kolay
- Hiçbir componenet sivil bölge içinde
- Gerekli gelen bağlantı
- O365 dahil olmak üzere tüm kullanıcıların uygulamaları için kullanıcılarınızın gidebilirsiniz, Azure AD SaaS uygulamalarında tümleşik ve şirket içi web uygulamaları bir erişim paneli. 


## <a name="next-steps"></a>Sonraki adımlar

- [Şirket içi uygulamalara güvenli uzaktan erişim sağlamak için Azure AD uygulaması kullanın](active-directory-application-proxy-get-started.md)
- [Geçiş Forefront TMG ve uygulama proxy'si UAG'ye](https://blogs.technet.microsoft.com/isablog/2015/06/30/modernizing-microsoft-application-access-with-web-application-proxy-and-azure-active-directory-application-proxy/).
