---
title: Azure AD parola koruması önizlemesi
description: Azure AD parola koruması Önizlemesi'ni kullanarak şirket içi Active Directory zayıf parolalarda yasaklama
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/25/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: 286f8e560ec653ed4f4f1cad5a2ae27b940f8d15
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43781789"
---
# <a name="preview-enforce-azure-ad-password-protection-for-windows-server-active-directory"></a>Önizleme: Azure AD parola koruması için Windows Server Active Directory zorla

|     |
| --- |
| Azure AD parola koruması ve özel yasaklı parola listesi Azure Active Directory genel Önizleme özellikleri şunlardır. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Azure AD parola koruması Azure Active Directory (parola ilkeleri bir kuruluşta geliştirmek için Azure AD) tarafından desteklenen, genel Önizleme aşamasında olan yeni bir özellik var. Azure AD parola koruması, şirket içi dağıtım iki genel kullanır ve özel Azure AD'de depolanan parola listelerini yasaklanmış ve aynı denetimleri şirket içinde Azure AD bulut tabanlı değişiklikler olarak gerçekleştirir.

Azure AD parola koruması üç yazılım bileşenleri şunlardır:

* Azure AD parola koruması proxy hizmeti geçerli Active Directory ormanındaki tüm etki alanına katılmış makinede çalıştırır. Azure AD etki alanı denetleyicilerinden isteklerini iletir ve etki alanı denetleyicisine Azure AD'den yanıtı döndürür.
* Azure AD parola koruması DC Aracısı hizmeti aracı DC parola filtresi DLL'den parola doğrulama isteklerini alır, geçerli yerel olarak kullanılabilir parola ilkesini kullanarak işleyen ve (pass\fail) sonuç döndürür. Bu hizmeti düzenli aralıklarla (saat bir kez) sorumlu parola ilkesini yeni sürümlerini almak için Azure AD parola koruması proxy hizmeti çağırma. Azure AD parola koruması proxy hizmeti gelen ve giden çağrıları için iletişim, TCP üzerinden üzerinden RPC (uzak yordam çağrısı) işlenir. Alma sırasında yeni ilkeler burada diğer etki alanı denetleyicilerine çoğaltabilirsiniz sysvol klasöründe depolanır. Diğer etki alanı denetleyicileri, yeni parola ilkelerini yazmış durumunda DC Aracısı hizmeti de değişiklikler için sysvol klasörünü izler, uygun şekilde yeni bir ilke zaten varsa Azure AD parola koruması proxy hizmeti denetimi atlandı.
* DC aracı parola filtresi DLL'sinin işletim sisteminden parola doğrulama isteklerini alır ve bunları etki alanı denetleyicisinde yerel olarak çalışan Azure AD parola koruması DC aracı hizmetine iletir.

![Azure AD parola koruması bileşenleri birlikte nasıl çalıştığını](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

## <a name="requirements"></a>Gereksinimler

* Etki alanı denetleyicileri de dahil olmak üzere Azure AD parola koruması bileşenlerinin yüklendiği tüm makinelerde Windows Server 2012 veya sonraki sürümünü çalıştırmalıdır.
* Etki alanı denetleyicileri de dahil olmak üzere Azure AD parola koruması bileşenlerinin yüklendiği tüm makinelerde yüklü olan evrensel C çalışma zamanı olması gerekir. Bu tercihen Windows Update aracılığıyla makine tam olarak düzeltme eki uygulama tarafından gerçekleştirilir. Aksi takdirde uygun işletim sistemine özgü güncelleştirme paketi olabilir yüklü - [Windows Evrensel C çalışma zamanı güncelleştirmesi](https://support.microsoft.com/help/2999226/update-for-universal-c-runtime-in-windows)
* En az bir sunucu Azure AD parola koruması proxy hizmetini barındıran her etki alanında en az bir etki alanı denetleyicisi arasında ağ bağlantısı olmalıdır.
* Parola koruması işlevsellikten yararlanan herhangi bir Active Directory etki alanı denetleyicisi DC aracısının yüklü olması gerekir.
* DC çalıştıran herhangi bir Active Directory etki alanı Aracı hizmeti yazılımı DFSR sysvol çoğaltma için kullanmanız gerekir.
* Azure AD ile Azure AD parola koruması proxy hizmeti kaydetmek için bir genel yönetici hesabı.
* Orman kök etki alanındaki Active Directory etki alanı yönetici ayrıcalıklarına sahip bir hesap.

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

[Azure AD parola korumasını dağıtma](howto-password-ban-bad-on-premises.md)
