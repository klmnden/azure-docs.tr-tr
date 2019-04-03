---
title: Azure AD parola koruması - Azure Active Directory
description: Azure AD parola koruması kullanarak şirket içi Active Directory zayıf parolalarda yasakla
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
ms.openlocfilehash: 9cd9f6112cbca78b323e0a14818b06f891a3f673
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58862896"
---
# <a name="enforce-azure-ad-password-protection-for-windows-server-active-directory"></a>Windows Server Active Directory için Azure AD parola koruması zorlama

Azure AD parola koruması parola ilkeleri bir kuruluşta geliştiren bir özelliktir. Şirket içi dağıtım parola koruması, Azure AD'de depolanan her iki genel ve özel yasaklanmış parola listelerini kullanır. Bunu, aynı denetimleri şirket içi olarak Azure AD için bulut tabanlı değişiklikler yapar.

## <a name="design-principles"></a>Tasarım ilkeleri

Azure AD parola koruması ile bu ilkeleri göz önünde tasarlanmıştır:

* Etki alanı denetleyicileri hiçbir zaman doğrudan internet ile iletişim kurması gerekir.
* Yeni bir ağ bağlantı noktaları, etki alanı denetleyicilerinde açılmadı.
* Active Directory şema değişiklik büyük/küçük harf gerekmez. Yazılım mevcut Active Directory kullanan **kapsayıcı** ve **serviceConnectionPoint** şema nesneleri.
* En düşük Active Directory etki alanı veya orman işlevsel düzeyi yok (DFL/FFL) gereklidir.
* Yazılım oluşturma değil veya koruduğu Active Directory etki alanı hesaplarında gerektirir.
* Kullanıcı düz metin parolaları, parola doğrulama işlemleri sırasında veya herhangi bir anda etki alanı denetleyicisi bırakmayın.
* Artımlı dağıtım desteklenir, ancak parola ilkesini etki alanı denetleyicisi Aracısı'nı (DC Aracısı) yüklendiği devreye girer. Daha fazla ayrıntı için sonraki bölüme bakın.

## <a name="incremental-deployment"></a>Artımlı dağıtım

Azure AD parola koruması, bir Active Directory etki alanındaki etki alanı denetleyicileri arasında artımlı dağıtımı destekler. ancak bunun gerçekten anlamı ve ödün ne olduğunu anlamak önemlidir.

Bu etki alanı denetleyicisine gönderilen parola değişiklikleri yanı sıra, bir etki alanı denetleyicisine yüklendiğinde Azure AD parola DC koruma Aracısı yazılımı yalnızca parolaları doğrulayabilirsiniz. Hangi etki alanı denetleyicileri Windows istemci makineleri için kullanıcının parola değişiklikleri işlemeye tarafından seçmiş denetlemek için mümkün değildir. Tutarlı bir davranış ve evrensel parola koruması güvenlik zorlama garanti için DC Aracısı yazılımını bir etki alanındaki tüm etki alanı denetleyicilerinde yüklenmelidir.

Birçok kuruluşun bir alt kümesi üzerinde Azure AD parola koruması tam dağıtımını yapmadan önce etki alanı denetleyicilerini dikkatli testi yapmak isteyebilirsiniz. Azure AD parola koruması kısmi dağıtım desteği, bile etki alanındaki diğer DC'leri DC aracı yazılımı yüklü olmadığında IE DC aracı yazılımı belirli bir DC üzerinde etkin bir şekilde parolaları doğrular. Bu tür kısmi dağıtımları olmayan dışında tavsiye edilmez ve güvenli sınama amacıyla.

## <a name="architectural-diagram"></a>Mimari diyagramı

Azure AD parola koruması bir şirket içi Active Directory ortamında dağıtmadan önce temel alınan tasarım ve işlev kavramları anlamak önemlidir. Aşağıdaki diyagramda, parola koruması bileşenlerinin birlikte nasıl çalıştığı gösterilmektedir:

![Azure AD parola koruması bileşenleri birlikte nasıl çalıştığını](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

* Azure AD parola koruması Proxy Hizmeti geçerli Active Directory ormanındaki tüm etki alanına katılmış makinede çalıştırır. Birincil amacı, parola ilkesi indirme istekleri için Azure AD etki alanı denetleyicilerinden iletmek sağlamaktır. Daha sonra yanıtları Azure AD etki alanı denetleyicisine döndürür.
* DC aracısının DLL parola filtresinin işletim sisteminden kullanıcı parolası doğrulama isteklerini alır. Bunları yerel olarak etki alanı denetleyicisinde çalışan DC aracı hizmetine iletir.
* Parola koruma DC Aracı hizmeti DC aracısının DLL parola filtresinden parola doğrulama isteği alır. Geçerli (yerel olarak kullanılabilir) parola ilkesini kullanarak isteği işler ve sonucu döndürür: *geçirmek* veya *başarısız*.

## <a name="how-password-protection-works"></a>Parola koruması nasıl çalışır?

Her Azure AD parola koruması Proxy Hizmeti örneği kendisini ormandaki etki alanı denetleyicilerine oluşturarak tanıtır bir **serviceConnectionPoint** Active Directory'de nesnesi.

Her DC Aracısı hizmeti parola koruması için ayrıca oluşturur bir **serviceConnectionPoint** Active Directory'de nesnesi. Bu nesne, öncelikle raporlama ve tanılama için kullanılır.

DC Aracı hizmeti, Azure AD'de yeni bir parola ilkesi yüklenmesi başlatmaktan sorumludur. Bir Azure AD parola koruması Proxy Hizmeti proxy ormanın sorgulayarak bulmak için ilk adımıdır **serviceConnectionPoint** nesneleri. Mevcut proxy hizmet bulunduğunda, DC aracı proxy hizmeti için bir parola ilkesi indirme isteği gönderir. Proxy hizmeti, Azure AD'ye sırayla isteği gönderir. Proxy hizmeti, ardından DC Aracı hizmeti yanıtı döndürür.

DC aracı hizmetini yeni bir parola ilkesi Azure AD'den aldıktan sonra hizmeti ilke, etki alanı kökünde adanmış bir klasörde depolar. *sysvol* klasör paylaşımı. Diğer etki alanındaki DC aracı hizmetlerden içinde yeni ilkeleri çoğaltma durumunda DC Aracı hizmeti bu klasör de izler.

DC Aracısı her zaman hizmeti başlatma sırasında yeni bir ilke ister. DC Aracı hizmeti başlatıldıktan sonra geçerli yerel olarak kullanılabilir ilke saatlik yaşını denetler. DC aracı İlkesi bir saatten eskiyse yeni bir ilke daha önce açıklandığı gibi Azure AD'den ister. Bir saatten daha eski geçerli ilke yoksa, DC aracı, bu ilkeyi kullanmaya devam eder.

Bir Azure AD parola koruması parola ilkesi indirilir her ilkenin bir kiracınıza özgüdür. Diğer bir deyişle, parola ilkeleri her zaman bir Microsoft Genel yasaklanmış parola listesi ve Kiracı başına özel yasaklanmış parola liste birleşimidir.

DC aracı proxy hizmeti RPC üzerinden ile TCP üzerinden iletişim kurar. Proxy hizmeti bu çağrılar yapılandırmasına bağlı olarak dinamik veya statik RPC bağlantı noktasında dinler.

DC Aracısı'nı hiçbir zaman ağ kullanılabilir bir bağlantı noktasında dinler.

Proxy hizmeti, hiçbir zaman DC aracı hizmetini çağırır.

Durum bilgisi olmayan proxy'si hizmetidir. Hiçbir zaman ilkeleri önbelleğe alır ya da herhangi bir durumu Azure'dan indirilebilir.

DC Aracı hizmeti, bir kullanıcının parolasını değerlendirmek için her zaman en son yerel olarak kullanılabilir parola ilkesi kullanır. Parola, parola ilkesi yok, yerel DC üzerinde kullanılabilir haldeyse, otomatik olarak kabul edilir. Bu durum oluştuğunda, olay iletisi yönetici uyarmak için günlüğe kaydedilir.

Azure AD parola koruması gerçek zamanlı ilke uygulama altyapısı değildir. Ne zaman, Azure AD'de parola ilkesi yapılandırma değişiklik yapıldığında ve ne zaman ulaştığında değiştirmek ve tüm etki alanı denetleyicilerinde zorlanan arasında bir gecikme olabilir.

## <a name="foresttenant-binding-for-password-protection"></a>Parola koruması için orman/Kiracı bağlama

Azure AD parola koruması bir Active Directory ormanındaki dağıtımını söz konusu ormanın Azure AD ile kayıt gerektirir. Dağıtılan her bir proxy hizmeti ayrıca Azure AD ile kayıtlı olması gerekir. Bu orman ve proxy kayıtları belirli bir ile ilişkili kayıt sırasında kullanılan kimlik bilgileri tarafından örtük olarak tanımlandığında, Azure AD kiracısına.

Active Directory ormanı ve bir orman içindeki tüm dağıtılan proxy Hizmetleri ile aynı kiracıda kayıtlı olması gerekir. Farklı Azure AD'ye kayıtlı orman Kiracı, bir Active Directory orman ya da tüm proxy hizmetleri için desteklenmiyor. Yanlış yapılandırılmış bir dağıtım belirtileri parola ilkelerini karşıdan yükleyebilir yeteneğinin içerir.

## <a name="download"></a>İndirme

Azure AD parola koruması için iki gerekli aracı yükleyici kullanılabilir [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=57071).

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD parola korumasını dağıtma](howto-password-ban-bad-on-premises-deploy.md)
