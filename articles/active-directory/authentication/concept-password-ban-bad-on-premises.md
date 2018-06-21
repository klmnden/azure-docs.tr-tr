---
title: Azure AD parola koruması Önizleme
description: BBu Zayıf parolalar şirket içi Active Directory'de Azure AD parola koruması Önizlemesi'ni kullanma
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: 0819a947229e633331253923654de56638a6c76a
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296116"
---
# <a name="preview-enforce-azure-ad-password-protection-for-windows-server-active-directory"></a>Önizleme: Windows Server Active Directory için Azure AD parola korumasını zorla

|     |
| --- |
| Azure AD parola koruması ve özel Engellenenler parola listesini Azure Active Directory genel Önizleme özellikleri verilmiştir. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları'nı Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Azure AD parola koruması yeni bir Azure Active Directory (parola ilkeleri bir kuruluşta geliştirmek için Azure AD) tarafından sağlanmıştır genel Önizleme özelliğidir. Azure AD parola koruması şirket içi dağıtımını her iki genel kullanır ve özel Azure AD'de depolanan parola listeleri yasaklanan ve aynı denetimleri şirket içi Azure AD bulut tabanlı değişiklikler olarak gerçekleştirir.

Azure AD parola koruması yapmak üç yazılım bileşenleri şunlardır:

* Azure AD parola koruması proxy hizmeti geçerli Active Directory ormanındaki tüm etki alanına katılmış makinede çalıştırır. Azure AD etki alanı denetleyicilerinden isteklerini iletir ve yanıt Azure AD etki alanı denetleyicisine döndürür.
* Azure AD parola koruması DC Aracısı hizmeti DC Aracısı parola filtresi DLL'den parola doğrulama isteklerini alır, bunları geçerli yerel olarak kullanılabilir Parola İlkesi'ni kullanarak işler ve (pass\fail) sonucunu döndürür. Bu hizmeti düzenli aralıklarla (saatte bir kez) sorumlu olduğu parola ilkesi yeni sürümlerini almak için Azure AD parola koruması proxy hizmeti çağırma. İletişim için ve Azure AD parola koruması proxy hizmeti çağrıları için üzerinden RPC (uzak yordam çağrısı) TCP üzerinden gerçekleştirilir. Alma olduğu diğer etki alanı denetleyicilerine çoğaltabilirsiniz sysvol klasörünü yeni ilkeler depolanır. Diğer etki alanı denetleyicilerine yeni parola ilkeleri yazmıştır durumda DC Aracısı hizmeti de değişiklikleri için sysvol klasörünü izler, uygun yeni bir ilke zaten varsa Azure AD parola koruması proxy hizmeti denetimi atlandı.
* DC Aracısı parola filtresi DLL'sinin işletim sisteminden parola doğrulama isteklerini alır ve etki alanı denetleyicisinde yerel olarak çalışan Azure AD parola koruması DC Aracısı hizmeti iletir.

![Azure AD parola koruması bileşenleri birlikte çalışmasını nasıl](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

## <a name="requirements"></a>Gereksinimler

* Azure AD parola koruması bileşenleri de dahil olmak üzere etki alanı denetleyicileri yüklü olduğu tüm makinelerde Windows Server 2012 veya sonraki sürümünü çalıştırması gerekir.
* Azure AD parola koruması proxy hizmeti barındıran en az bir sunucu ve her etki alanında en az bir etki alanı denetleyicisi arasında ağ bağlantısı mevcut olması gerekir.
* DC çalıştıran herhangi bir Active Directory etki alanı Aracısı hizmeti yazılım DFSR sysvol çoğaltma için kullanmanız gerekir.
* Azure AD parola koruması proxy hizmeti Azure AD ile kaydetmek için genel yönetici hesabı.
* Orman kök etki alanındaki Active Directory etki alanı yönetici ayrıcalıklarına sahip bir hesap.

### <a name="license-requirements"></a>Lisans gereksinimleri

Genel Engellenenler parola listesini yararları Azure Active Directory (Azure AD) tüm kullanıcılara uygulanır.

Özel Engellenenler parola liste Azure AD temel lisansı gerektirir.

Windows Server Active Directory için Azure AD parola koruması Azure AD Premium lisansı gerektirir. 

Maliyetleri de dahil olmak üzere ek lisans bilgilerini, üzerinde bulunabilir [Azure Active Directory sitesi fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="download"></a>İndirme

Yüklenebilir Azure AD parola koruması için iki gerekli yükleyiciler vardır [Microsoft İndirme Merkezi](https://www.microsoft.com/download/details.aspx?id=57071)

## <a name="answers-to-common-questions"></a>Sık sorulan soruların yanıtlarını

* Etki alanı denetleyicilerinden gerekli internet bağlantısı. Azure AD parola koruması proxy'si hizmeti çalıştıran makineler internet bağlantısı gerektiren yalnızca makine yok.
* Hiçbir ağ bağlantı noktaları, etki alanı denetleyicilerinde açıldı.
* Active Directory şema değişiklik gerekmez.
   * Yazılım mevcut Active Directory kapsayıcısı ve serviceConnectionPoint şema nesneleri kullanır.
* Minimum Active Directory etki alanı veya orman işlev düzeyi (DFL\FFL) gereksinimi yoktur.
* Yazılım oluşturmaz veya koruduğu Active Directory etki alanındaki herhangi bir hesabı gerektirir.
* Artımlı dağıtımı parola ilkesi yalnızca etki alanı denetleyicisi Aracısı yüklendiği uygulandığını kolaylığını ile desteklenir.
* Azure AD parola koruması gerçek zamanlı ilke uygulama altyapısı değil. Bir parola ilkesi yapılandırma değişikliği ve ulaştığında ve tüm etki alanı denetleyicilerinde zorlanan saat arasındaki zaman bir gecikme olabilir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD parola korumasını dağıtma](howto-password-ban-bad-on-premises.md)