---
title: Şirket içinde Azure AD parola koruması ile ilgili SSS
description: Şirket içinde Azure AD parola koruması ile ilgili SSS
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 10/30/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: f690625fda07bdbff671567f0292cc8e1cabeda1
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2019
ms.locfileid: "55078704"
---
# <a name="preview-azure-ad-password-protection-on-premises---frequently-asked-questions"></a>Önizleme: Azure AD parola koruması hakkında sık sorulan sorular şirket içinde

|     |
| --- |
| Azure AD parola koruması, Azure Active Directory genel Önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="general-questions"></a>Genel sorular

**S: Ne zaman Azure AD parola koruması genel kullanılabilirlik (GA) ulaşır?**

Henüz bir GA tarih duyurduk değil.

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

Yanıtını burada bulamadığınız bir şirket içi Azure AD parola koruması sorunuz varsa, bir geri bildirim öğesi altında - teşekkür gönderin!

[Azure AD parola korumasını dağıtma](howto-password-ban-bad-on-premises-deploy.md)
