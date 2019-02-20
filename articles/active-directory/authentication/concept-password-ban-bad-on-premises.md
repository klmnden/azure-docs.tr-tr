---
title: Azure AD parola koruması önizlemesi
description: Azure AD parola koruması Önizlemesi'ni kullanarak şirket içi Active Directory zayıf parolalarda yasaklama
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/18/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: f1beae186f6eb276b9aa302d3d51f0ba8688e591
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56415757"
---
# <a name="preview-enforce-azure-ad-password-protection-for-windows-server-active-directory"></a>Önizleme: Windows Server Active Directory için Azure AD parola koruması zorlama

|     |
| --- |
| Azure AD parola koruması ve özel yasaklı parola listesi Azure Active Directory genel Önizleme özellikleri şunlardır. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Azure AD parola koruması Azure Active Directory (parola ilkeleri bir kuruluşta geliştirmek için Azure AD) tarafından desteklenen, genel Önizleme aşamasında olan yeni bir özellik var. Azure AD parola koruması, şirket içi dağıtım iki genel kullanır ve özel Azure AD'de depolanan parola listelerini yasaklanmış ve aynı denetimleri şirket içinde Azure AD bulut tabanlı değişiklikler olarak gerçekleştirir.

## <a name="design-principles"></a>Tasarım ilkeleri

Active Directory için Azure AD parola koruması ile aşağıdaki ilkeleri göz önünde tasarlanmıştır:

* Etki alanı denetleyicilerine asla doğrudan Internet ile iletişim kurmak için gereklidir
* Yeni bir ağ bağlantı noktaları, etki alanı denetleyicilerinde açılmadı.
* Active Directory şema değişiklik büyük/küçük harf gerekmez. Yazılım, mevcut Active Directory kapsayıcısı ve serviceConnectionPoint şema nesneleri kullanır.
* En düşük Active Directory etki alanı veya orman işlevsel düzeyi yok (DFL\FFL) gereklidir.
* Yazılım oluşturmaz veya herhangi bir hesabı koruduğu Active Directory etki alanlarında gerektirir.
* Düz metin parolalar kullanıcı etki alanı denetleyicisi (olmadığını parola doğrulama işlemleri sırasında veya başka bir zaman) hiçbir zaman ayrılmaz.
* Artımlı dağıtım etki alanı denetleyicisi aracının yüklendiği parola ilkesi yalnızca uygulandığını artırabilen ile desteklenir.
* Bulunabilen parola koruması güvenlik zorlama emin olmak için tüm DC'leri DC aracıyı yüklemek için önerilir.

## <a name="architectural-diagram"></a>Mimari diyagramı

Azure AD parola koruması bir şirket içi Active Directory ortamında dağıtmadan önce temel alınan tasarım ve işlevsel kavramları bilginizin olması önemlidir. Aşağıdaki diyagramda, Azure AD parola koruması bileşenlerinin birlikte nasıl çalıştığı gösterilmektedir:

![Azure AD parola koruması bileşenleri birlikte nasıl çalıştığını](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

Yukarıdaki diyagramda, Azure AD parola koruması üç temel yazılım bileşenleri gösterilmektedir:

* Azure AD parola koruması Proxy Hizmeti geçerli Active Directory ormanındaki tüm etki alanına katılmış makinede çalıştırır. Birincil amacı, parola ilkesi indirme istekleri için Azure AD etki alanı denetleyicilerinden iletmek ve yanıt Azure AD'den etki alanı denetleyicisini geri dönüş oluşturmaktır.
* Azure AD parola koruması DC aracı parola filtresi DLL'sinin işletim sisteminden kullanıcı parolası doğrulama isteklerini alır ve bunları yerel olarak etki alanı denetleyicisinde çalışan Azure AD parola koruması DC aracı hizmetine iletir.
* Azure AD parola koruması DC Aracı hizmeti DC aracı parola filtresi DLL'den parola doğrulama isteklerini alır, geçerli (yerel olarak kullanılabilir) parola ilkesini kullanarak işleyen ve (pass\fail) sonuç döndürür.

## <a name="theory-of-operations"></a>Teorik işlemleri

Bu nedenle Yukarıdaki diyagramda ve tasarım ilkelerini verilen nasıl Azure AD parola koruması gerçekten çalışır?

Her Azure AD parola koruması Proxy hizmetini kendisini ormandaki etki alanı denetleyicileri için Active Directory'de serviceConnectionPoint nesne oluşturarak bildirir.

Her Azure AD parola koruması DC Aracısı hizmetini, aynı zamanda Active Directory'de bir serviceConnectionPoint nesnesi oluşturur. Ancak bu öncelikle raporlama ve tanılama için kullanılır.

Azure AD parola koruması DC Aracı hizmeti, Azure AD'de yeni bir parola ilkesi yüklenmesi başlatmaktan sorumludur. İlk adım, proxy serviceConnectionPoint nesneleri ormanın sorgulayarak bir Azure AD parola koruması Proxy Hizmeti bulmaktır. Kullanılabilir proxy hizmeti bulunduktan sonra bir parola ilkesi indirme isteği buna karşılık, Azure AD'ye gönderir ve ardından DC Aracı hizmeti yanıtı döndürür proxy hizmeti DC aracı hizmetten gönderilir. Yeni bir parola ilkesi Azure AD'den aldıktan sonra DC Aracı hizmeti ilke kendi etki alanı sysvol paylaşımı kökünde adanmış bir klasörde depolar. Etki alanındaki diğer DC aracı hizmetlerden içinde yeni ilkeleri çoğaltma durumunda DC Aracısı hizmeti de bu klasör izler.

Azure AD parola koruması DC Aracısı her zaman yeni bir ilke hizmet başlangıç ister. DC Aracı hizmeti başlatıldıktan sonra düzenli aralıklarla olur (saatte bir kez), geçerli yerel olarak kullanılabilir ilke; yaşını denetleyin Aksi takdirde geçerli ilkeyi bir saat DC aracı hizmetini yeni bir ilke Azure AD'den yukarıda açıklanan şekilde ister daha eski ise, geçerli ilkeyi kullanmak DC aracı devam eder.

Azure AD parola koruması DC Aracı hizmeti TCP üzerinden RPC (uzak yordam çağrısı) kullanarak Azure AD parola koruması Proxy Hizmeti ile iletişim kurar. Proxy hizmeti çağrıları ya da dinamik veya statik RPC bağlantı noktası (yapılandırıldığı gibi) dinler.

Azure AD parola koruması DC Aracısı'nı hiçbir zaman ağ kullanılabilir bir bağlantı noktasını dinler ve Proxy Hizmeti DC aracı hizmetini çağırmak hiçbir zaman çalışır.

Durum bilgisi olmayan Azure AD parola koruması Proxy hizmetidir; hiçbir zaman ilkeleri önbelleğe alır ya da herhangi bir durumu Azure'dan indirilebilir.

Azure AD parola koruması DC Aracısı her zaman sadece en son yerel olarak kullanılabilir parola ilkesini kullanarak bir kullanıcının parolasını değerlendirir. Yerel DC üzerinde hiçbir parola ilkesi varsa, parolayı otomatik olarak kabul edilir ve yönetici uyarmak için olay günlüğü iletisi kaydedilir.

Azure AD parola koruması gerçek zamanlı ilke uygulama altyapısı değil. Olabilir bir gecikme süresi arasında bir parola ilkesi yapılandırma değişikliği, Azure AD ve ulaştığında ve tüm etki alanı denetleyicilerinde zorlanır zaman yapılır.

## <a name="foresttenant-binding-for-azure-ad-password-protection"></a>Azure AD parola koruması için Forest\tenant bağlama

Azure AD parola koruması bir Active Directory ormanındaki dağıtımını Active Directory ormanı ve Azure AD ile dağıtılan bir Azure AD parola koruması Proxy Hizmetleri, kaydı gerektirir. Her iki tür kayıtları (orman ve proxy'ler) belirli bir ile ilişkili Azure AD kiracısı, bir kayıt sırasında kullanılan kimlik bilgileri aracılığıyla örtük olarak tanımlanır. Bir Azure AD parola koruması parola ilkesi indirilir her zaman, her zaman bu kiracıya özel olan (yani, ilke her zaman genel Microsoft birleşimi olacaktır parola listesi ve Kiracı başına özel yasaklı parola listesi yasaklanmış). Farklı Azure AD'ye bağlanması için bir ormandaki farklı etki alanları veya proxy'ler yapılandırmak için desteklenmeyen kiracılar.

## <a name="license-requirements"></a>Lisans gereksinimleri

Genel yasaklı parola listesi avantajları, Azure Active Directory (Azure AD) tüm kullanıcılar için geçerlidir.

Özel yasaklı parola listesi, Azure AD temel lisansı gerektirir.

Azure AD parola koruması için Windows Server Active Directory, Azure AD Premium lisansı gerektirir.

Maliyetleri de dahil olmak üzere ek lisans bilgilerini bulunabilir [Azure Active Directory site fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="download"></a>İndirme

İki aracı yükleyicileri indirilebileceğini Azure AD parola koruması için gerekli [Microsoft İndirme Merkezi](https://www.microsoft.com/download/details.aspx?id=57071)

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD parola korumasını dağıtma](howto-password-ban-bad-on-premises-deploy.md)
