---
title: Şirket içinde Azure AD parola koruması Aracı sürüm yayınlama geçmişi
description: Belgeler sürüm ve davranışı değişiklik geçmişi
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 11/01/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: ccfe62e0002e3420303130840f1a0d393efb3420
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2019
ms.locfileid: "55078772"
---
# <a name="preview--azure-ad-password-protection-agent-version-history"></a>Önizleme:  Azure AD parola koruması Aracı sürüm geçmişi

|     |
| --- |
| Azure AD parola koruması, Azure Active Directory genel Önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="12250"></a>1.2.25.0

Yayın Tarihi: 11/01/2018

Düzeltmeleri:

* DC aracı ve proxy hizmeti artık sertifika güven hataları nedeniyle başarısız olması.
* DC aracı ve proxy hizmeti FIPS uyumlu makineler için ek düzeltmelere sahip.
* Proxy hizmeti artık düzgün şekilde TLS 1.2 yalnızca ağ ortamında çalışır.
* Küçük bir performans ve sağlamlık düzeltmeleri
* Gelişmiş günlük kaydı

Değişiklikler:

* Gereken en düşük işletim sistemi düzeyinde Proxy Hizmeti için artık Windows Server 2012 R2. DC Aracı hizmeti için en düşük gerekli işletim sistemi düzeyi Windows Server 2012 kalır.
* Parola doğrulama algoritması bir Genişletilmiş karakter normalleştirme tablosunu kullanır. Bu, önceki sürümlerde kabul edildi reddediliyor parolalarda neden olabilir.

## <a name="12100"></a>1.2.10.0

Yayın Tarihi: 8/17/2018

Düzeltmeleri:

* Register-AzureADPasswordProtectionProxy ve kayıt AzureADPasswordProtectionForest artık çok faktörlü kimlik doğrulaması desteği
* WS2012 veya üzeri etki alanı denetleyicisi etki alanında şifreleme hataları önlemek için kayıt AzureADPasswordProtectionProxy gerektirir.
* DC Aracısı hizmetini yeni bir parola ilkesi Azure'dan başlangıçta isteme hakkında daha güvenilir oldu.
* DC Aracısı hizmetini yeni bir parola ilkesi Azure'dan saatte gerekirse ister, ancak bir rastgele seçilen başlangıç zamanında artık bunu yapar.
* DC Aracısı hizmeti artık yeni DC tanıtım bir kopya olarak, yükseltmeden önce bir sunucu üzerinde yüklü olduğunda, belirsiz bir gecikme neden olur.
* DC Aracısı hizmeti, "Windows Server Active Directory parola korumasını etkinleştir" yapılandırma ayarı artık dokunmaz
* Sonraki sürümlere yükseltme yapılırken, her iki DC aracı ve proxy yükleyici yerinde yükseltme artık destekleyecektir.

> [!WARNING]
> 1.1.10.3 sürümünden yerinde yükseltme desteklenmez ve bir yükleme hataya neden olur. Çok 1.2.10 sürüme yükseltin veya sonraki sürümlerde, önce tamamen DC aracı ve proxy hizmeti yazılımı kaldırın, ardından gerekir sıfırdan yeni sürümünü yükleyin. Azure AD parola koruması Proxy Hizmeti yeniden kayıt gereklidir.  Orman yeniden kaydolmak için gerekli değildir.

> [!NOTE]
> Yerinde yükseltme DC Aracısı yazılım yeniden başlatma gerektirir.

* DC aracı ve proxy hizmet desteği yalnızca FIPS uyumlu algoritmalar kullanmak için yapılandırılmış bir sunucuda çalıştırmak.
* Küçük bir performans ve sağlamlık düzeltmeleri
* Gelişmiş günlük kaydı

## <a name="11103"></a>1.1.10.3

Yayın Tarihi: 6/15/2018

İlk genel Önizleme sürümü

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD parola korumasını dağıtma](howto-password-ban-bad-on-premises-deploy.md)
