---
title: Şirket içi Azure AD parola koruması ile ilgili SSS
description: Şirket içi Azure AD parola koruması ile ilgili SSS
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0a4528cde92c5f738fa3fa7f4a649d84b1e79431
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56175957"
---
# <a name="preview-azure-ad-password-protection-on-premises---frequently-asked-questions"></a>Önizleme: Azure AD parola koruması hakkında sık sorulan sorular şirket içinde

|     |
| --- |
| Azure AD parola koruması, Azure Active Directory genel Önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="general-questions"></a>Genel sorular

**S: Ne zaman Azure AD parola koruması genel kullanılabilirlik (GA) ulaşır?**

Genel kullanım için S1 CY2019 (Mart 2019 sonu önce) planlanmaktadır. Geri bildirim özelliğini - bugüne yayımlamış herkese teşekkür ederiz, değer veriyoruz!

**S: Şirket içi Azure AD parola koruması genel olmayan bulutlarda desteklenir mi?**

Hayır - şirket içi Azure AD parola koruması yalnızca genel bulut ortamında desteklenir.

**S: Nasıl ben Azure AD parola koruması avantajları bir şirket içi kullanıcılar alt kümesine uygulayabilir miyim?**

Desteklenmiyor. Dağıtılan ve etkinleştirilen sonra Azure AD parola koruması olmayan ayrım - tüm kullanıcılar eşit güvenlik avantajlardan yararlanabilir.

**S: Azure AD parola koruması parola-filtre tabanlı diğer ürünleri ile yan yana yüklenecek destekleniyor mu?**

Evet. Birden çok kayıtlı parola filtresi DLL'leri çekirdek Windows özelliği için destek ve Azure AD parola koruması özgüdür. Bir parola kabul edilmeden önce tüm kayıtlı parola filtresi DLL'leri kabul etmesi gerekir.

**S: DFSR, sysvol çoğaltma için neden gereklidir?**

FRS (DFSR için öncül teknolojisi) birçok bilinen sorunlar ve Windows Server Active Directory daha yeni sürümlerinde tamamen desteklenmez. Azure AD parola koruması sıfır test etki alanlarında FRS yapılandırılmış gerçekleştirilir.

Daha fazla bilgi için lütfen aşağıdaki makalelere bakın:

[Durum taşıma sysvol çoğaltması DFSR için](https://blogs.technet.microsoft.com/askds/2010/04/22/the-case-for-migrating-sysvol-to-dfsr)

[Sonu Nigh için FRS](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs)

**S: Neden yüklemek veya DC Aracısı yazılımını yükseltmek için bir yeniden başlatma gerekiyor?**

Bu gereksinim, Windows davranışı core tarafından neden olur.

**S: Belirli bir proxy sunucuyu kullanmak için bir DC aracısını yapılandırmak için herhangi bir yolu var mı?**

Hayır.

## <a name="next-steps"></a>Sonraki adımlar

Burada bulamadığınız bir şirket içi Azure AD parola koruması sorunuz varsa, bir geri bildirim öğesi altında - teşekkür gönderin!

[Azure AD parola korumasını dağıtma](howto-password-ban-bad-on-premises-deploy.md)
