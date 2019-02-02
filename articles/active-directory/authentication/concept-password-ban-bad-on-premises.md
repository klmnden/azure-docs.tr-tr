---
title: Azure AD parola koruması önizlemesi
description: Azure AD parola koruması Önizlemesi'ni kullanarak şirket içi Active Directory zayıf parolalarda yasaklama
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/25/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: 816c459ca6edd7204ccdcdf9d402f2d4499d9116
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55662532"
---
# <a name="preview-enforce-azure-ad-password-protection-for-windows-server-active-directory"></a>Önizleme: Windows Server Active Directory için Azure AD parola koruması zorlama

|     |
| --- |
| Azure AD parola koruması ve özel yasaklı parola listesi Azure Active Directory genel Önizleme özellikleri şunlardır. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Azure AD parola koruması Azure Active Directory (parola ilkeleri bir kuruluşta geliştirmek için Azure AD) tarafından desteklenen, genel Önizleme aşamasında olan yeni bir özellik var. Azure AD parola koruması, şirket içi dağıtım iki genel kullanır ve özel Azure AD'de depolanan parola listelerini yasaklanmış ve aynı denetimleri şirket içinde Azure AD bulut tabanlı değişiklikler olarak gerçekleştirir.

Azure AD parola koruması üç yazılım bileşenleri şunlardır:

* Azure AD parola koruması proxy hizmeti geçerli Active Directory ormanındaki tüm etki alanına katılmış makinede çalıştırır. Azure AD etki alanı denetleyicilerinden isteklerini iletir ve etki alanı denetleyicisine Azure AD'den yanıtı döndürür.
* Azure AD parola koruması DC Aracısı hizmeti aracı DC parola filtresi DLL'den parola doğrulama isteklerini alır, geçerli yerel olarak kullanılabilir parola ilkesini kullanarak işleyen ve (pass\fail) sonuç döndürür. Bu hizmeti düzenli aralıklarla (saat bir kez) sorumlu parola ilkesini yeni sürümlerini almak için Azure AD parola koruması proxy hizmeti çağırma. Azure AD parola koruması DC Aracısı hizmeti ve Azure AD parola koruması proxy hizmeti arasındaki iletişim, TCP üzerinden RPC (uzak yordam çağrısı) kullanılarak işlenir. Alma sırasında yeni ilkeler burada diğer etki alanı denetleyicilerine çoğaltabilirsiniz sysvol klasöründe depolanır. Diğer etki alanı denetleyicileri, yeni parola ilkelerini yazmış durumunda DC Aracısı hizmeti de değişiklikler için sysvol klasörünü izler; uygun şekilde yeni bir ilke zaten varsa yeni ilke karşıdan yükleme isteği atlanacak.
* DC aracı parola filtresi DLL'sinin işletim sisteminden parola doğrulama isteklerini alır ve bunları etki alanı denetleyicisinde yerel olarak çalışan Azure AD parola koruması DC aracı hizmetine iletir.

![Azure AD parola koruması bileşenleri birlikte nasıl çalıştığını](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

### <a name="license-requirements"></a>Lisans gereksinimleri

Genel yasaklı parola listesi avantajları, Azure Active Directory (Azure AD) tüm kullanıcılar için geçerlidir.

Özel yasaklı parola listesi, Azure AD temel lisansı gerektirir.

Azure AD parola koruması için Windows Server Active Directory, Azure AD Premium lisansı gerektirir.

Maliyetleri de dahil olmak üzere ek lisans bilgilerini bulunabilir [Azure Active Directory site fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="download"></a>İndirme

Adresinden indirilip Azure AD parola koruması için gerekli iki yükleyiciler vardır [Microsoft İndirme Merkezi](https://www.microsoft.com/download/details.aspx?id=57071)

## <a name="answers-to-common-questions"></a>Sık sorulan sorulara yanıtlar

* Etki alanı denetleyicilerinden gerekli internet bağlantısı yok. Azure AD parola koruması proxy'si hizmeti çalıştıran makineleri internet bağlantısı gerektiren yalnızca makinelerdir.
* Hiçbir ağ bağlantı noktası, etki alanı denetleyicilerinde açılmadı.
* Active Directory şema değişiklik büyük/küçük harf gerekmez.
* Yazılım, mevcut Active Directory kapsayıcısı ve serviceConnectionPoint şema nesneleri kullanır.
* En düşük Active Directory etki alanı veya orman işlevsel düzeyi (DFL\FFL) gereksinimi yoktur.
* Yazılım oluşturmaz veya herhangi bir hesabı koruduğu Active Directory etki alanlarında gerektirir.
* Artımlı dağıtım etki alanı denetleyicisi aracının yüklendiği parola ilkesi yalnızca uygulandığını artırabilen ile desteklenir.
* Parola koruması zorlama emin olmak için tüm DC'leri DC aracıyı yüklemek için önerilir.
* Azure AD parola koruması gerçek zamanlı ilke uygulama altyapısı değil. Bir parola ilkesi yapılandırma değişikliği ve ulaştığında ve tüm etki alanı denetleyicilerinde zorlanan saat arasındaki zaman bir gecikme olabilir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD parola korumasını dağıtma](howto-password-ban-bad-on-premises-deploy.md)
