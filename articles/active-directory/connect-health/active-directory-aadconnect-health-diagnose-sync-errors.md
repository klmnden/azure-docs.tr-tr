---
title: Azure AD Connect Health - tanılama özniteliği eşitleme hatalarını yinelenen | Microsoft Docs
description: Bu belgede, yinelenen öznitelik eşitleme hatalarını tanılama işlemi ve olası bir düzeltme doğrudan Azure portalından yalnız bırakılmış nesneye senaryolar açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: zhiweiw
manager: maheshu
editor: billmath
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2018
ms.author: zhiweiw
ms.openlocfilehash: 4a6e0924492c26c9bad4ed0af207ad9764c3cc5c
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34831906"
---
# <a name="diagnose-and-remediate-duplicated-attribute-sync-errors"></a>Tanılama ve yinelenen öznitelik eşitleme hatalarını Düzelt

## <a name="overview"></a>Genel Bakış
Eşitleme hataları vurgulamak için bir adım uzağa alma, Azure Active Directory (Azure AD) Connect Health Self Servis düzeltmeyi sunar. Yinelenen öznitelik Eşitleme hataları giderir ve yalnız bırakılır nesneleri Azure AD'den giderir.
Tanı özelliği aşağıdaki faydaları vardır:
- Yinelenen öznitelik Eşitleme hataları daraltır tanılama bir yordam sağlar. Ve düzeltmeler sağlar.
- Tek bir adımda hatayı gidermek için Azure AD'den bir düzeltme ayrılmış senaryolar için geçerlidir.
- Yükseltme veya yapılandırma yok, bu özelliği etkinleştirmek için gereklidir.
Azure AD hakkında daha fazla bilgi için bkz: [kimlik eşitleme ve yinelenen öznitelik dayanıklılık](https://aka.ms/dupattributeresdocs).

## <a name="problems"></a>Sorunları
### <a name="a-common-scenario"></a>Yaygın bir senaryo
Zaman **QuarantinedAttributeValueMustBeUnique** ve **AttributeValueMustBeUnique** Eşitleme hataları durum, bu yaygın bir **UserPrincipalName** veya **Proxy adreslerini** Azure AD çakışıyor. Şirket içi yan çakışan kaynak nesneden güncelleyerek Eşitleme hataları düzeltebilir. Eşitleme hatası sonraki eşitlemeden sonra çözümlenir. Örneğin, bu görüntüyü iki kullanıcı çakışma sahip olduğunu gösterir. kendi **UserPrincipalName**. Her ikisi de **Joe.J@contoso.com**. Çakışan nesneler Azure AD'de karantinaya alındı durumundadır.

![Eşitleme hatası yaygın bir senaryo tanılama](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixCommonCase.png)

### <a name="orphaned-object-scenario"></a>Yalnız bırakılmış nesneye senaryosu
Bazen, mevcut bir kullanıcının kaybederse bulabileceğiniz **kaynak bağlantısı**. Kaynak nesne silinmesini şirket içi Active Directory'de oldu. Ancak silme sinyal değişiklik hiçbir zaman Azure AD'ye eşitlenen. Eşitleme altyapısı sorunları veya etki alanı geçişi gibi nedenlerle bu kaybı olur. Aynı nesne veya geri yeniden, mantıksal olarak, mevcut bir kullanıcının eşitleme kullanıcıya olmalıdır **kaynak bağlantısı**. 

Mevcut bir kullanıcının bir yalnızca bulut nesne olduğunda, Azure AD'ye eşitlenen çakışan kullanıcı de görebilirsiniz. Kullanıcı, var olan nesneye eşit eşleştirilemiyor. Yeniden eşlemek için doğrudan bir yolu yoktur **kaynak bağlantısı**. Daha fazla gördükleri hakkında [varolan Bilgi Bankası](https://support.microsoft.com/help/2647098). 

Örnek olarak, Azure AD mevcut nesne Joe lisans korur. Farklı bir sahip yeni eşitlenmiş bir nesnenin **kaynak bağlantısı** Azure AD'de bir yinelenen öznitelik durumunda oluşur. Şirket içi Active Directory'de Joe değişiklikleri Can'ın özgün kullanıcı (varolan nesne) Azure AD içinde uygulanmaz.  

![Eşitleme hatası yalnız bırakılmış nesneye senaryosu tanılama](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixOrphanedCase.png)

## <a name="diagnostic-and-troubleshooting-steps-in-connect-health"></a>Tanılama ve sorun giderme adımları Connect Health içinde 
Tanıla özelliği, şu yinelenen öznitelikleri olan kullanıcı nesneleri destekler:

| Öznitelik adı | Eşitleme hata türleri|
| ------------------ | -----------------|
| userPrincipalName | QuarantinedAttributeValueMustBeUnique veya AttributeValueMustBeUnique | 
| ProxyAddresses | QuarantinedAttributeValueMustBeUnique veya AttributeValueMustBeUnique | 
| sipProxyAddress | AttributeValueMustBeUnique | 
| onPremiseSecurityIdentifier |  AttributeValueMustBeUnique |

>[!IMPORTANT]
> Bu özelliğe erişmek için **genel yönetici** izni veya **katkıda bulunan** RBAC ayarlarından izni gereklidir.
>

Eşitleme hata ayrıntılarını daraltmak ve daha belirli çözümleri sağlamak için Azure portalından adımları izleyin:

![Eşitleme hatası tanılama adımları](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixSteps.png)

Azure portalından belirli düzeltilebilir senaryoları tanımlamak için bazı adımları uygulayın:  
1.  Denetleme **durumunu tanılamak** sütun. Durumu eşitleme hatası doğrudan Azure Active Directory'den düzeltmek için olası bir yolu olup olmadığını gösterir. Diğer bir deyişle, sorun giderme akış var hata durumunu daraltmak ve potansiyel olarak düzeltin.
| Durum | Ne anlama geliyor? |
| ------------------ | -----------------|
| Başlatılmadı | Bu tanılama işlemi ziyaret etmediğiniz. Tanılama sonucu bağlı olarak doğrudan portal üzerinden eşitleme hatayı düzeltmek için olası bir yolu yoktur. |
| El ile Düzeltme Gerekli | Hata portalından kullanılabilir düzeltmeler ölçütlerine uymayan. Kullanıcıları, çakışan ya da nesne türleri değil veya zaten tanılama adımlarını gittiğiniz ve düzeltme çözümleme portaldan kullanılabilir. İkinci durumda, bir düzeltme şirket içi taraftaki hala çözümleri biridir. [Okuma hakkında daha fazla bilgi içi düzeltmeleri](https://support.microsoft.com/help/2647098). | 
| Beklenen Eşitleme | Bir düzeltme uygulandı. Portal hatayı temizlemek bir sonraki eşitleme döngüsü için bekliyor. |
  >[!IMPORTANT]
  > Tanı durumu sütun her eşitleme döngüsünden sonra sıfırlanır. 
  >

2.  Seçin **Tanıla** düğmesi hata ayrıntılarını altında. Birkaç soruyu yanıtlamanız ve eşitleme hata ayrıntılarını tanımlayın. Soruların yanıtlarını yalnız bırakılmış nesneye çalışması tanımlamaya yardımcı olur.

3.  Varsa bir **Kapat** tanılama sonunda düğmesi görünür olduğundan hızlı düzeltme verdiğiniz yanıtları temel alarak portalından. Son adımda gösterilen çözümde bakın. Şirket içi düzeltmeleri hala çözümdür. Seçin **Kapat** düğmesi. Geçerli eşitleme hatası durumunu geçer **gerekli el ile düzeltme**. Durumu geçerli eşitleme döngüsü sırasında kalır.

4.  Yalnız bırakılmış nesneye çalışması belirlendikten sonra yinelenen öznitelikler doğrudan portal üzerinden eşitleme hataları düzeltebilir. İşlemi tetiklemek için seçin **düzeltme** düğmesi. Geçerli eşitleme hatası güncelleştirmeler durumunu **eşitleme bekleyen**.

5.  Bir sonraki eşitleme döngüsünden sonra hata listesinden kaldırılması gerekir.

## <a name="how-to-answer-the-diagnosis-questions"></a>Tanılama soruları yanıtlamak nasıl 
### <a name="does-the-user-exist-in-your-on-premises-active-directory"></a>Kullanıcı, şirket içi Active Directory'nizde var mı?

Şirket içi Active Directory'den mevcut kullanıcının kaynak nesnesi tanımlamak bu soruyu çalışır.  
1.  Azure Active Directory sağlanan sahip bir nesne olup olmadığını kontrol **UserPrincipalName**. Aksi durumda, yanıt **Hayır**.
2.  Aşması durumunda nesne eşitleme için hala kapsamında olup olmadığını denetleyin.  
  - DN kullanarak Azure AD bağlayıcı alanı arama.
  - Nesne bulunursa **bekleyen eklemek** durum, yanıt **Hayır**. Azure AD Connect nesneyi sağ Azure AD nesnesine bağlanamıyor.
  - Nesne bulunamazsa, yanıt **Evet**.

Bu örneklerde, soru tanımlamayı dener olup olmadığını **Joe Jackson** hala şirket içi Active Directory'de mevcut.
İçin **yaygın bir senaryo**, her iki kullanıcı **Joe Johnson** ve **Joe Jackson** şirket içi Active Directory'de mevcut. Karantinaya alınan nesneleri iki farklı kullanıcılardır.

![Eşitleme hatası yaygın bir senaryo tanılama](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixCommonCase.png)

İçin **yalnız bırakılmış nesneye senaryo**, yalnızca tek kullanıcı **Joe Johnson** şirket içi Active Directory'de mevcuttur:

![Eşitleme hatası yalnız bırakılmış nesneye tanılamak * kullanıcı mevcut * senaryosu](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixOrphanedCase.png)

### <a name="do-both-of-these-accounts-belong-to-the-same-user"></a>Bu hesapların her ikisi de aynı kullanıcıya ait?
Bu soruyu, aynı kullanıcıya ait görmek için Azure AD'de bir gelen çakışan kullanıcı ve var olan kullanıcı nesnesi denetler.  
1.  Çakışan nesne yeni Azure Active Directory ile eşitlenmiş. Nesnelerin öznitelikleri karşılaştırın:  
  - Görünen Ad
  - Kullanıcı Asıl Adı
  - Nesne kimliği
2.  Bunları karşılaştırmak Azure AD başarısız olursa, Active Directory nesneleri ile sağlanan sahip olup olmadığını kontrol **UserPrincipalNames**. Yanıt **Hayır** her ikisi de fark ederseniz.

Aşağıdaki örnekte, iki nesnenin aynı kullanıcıya ait **Joe Johnson**.

![Eşitleme hatası yalnız bırakılmış nesneye tanılamak * aynı kullanıcı * senaryosu](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixOrphanedCase.png)


## <a name="what-happens-after-the-fix-is-applied-in-the-orphaned-object-scenario"></a>Yalnız bırakılmış nesneye senaryoda düzeltme uygulandıktan sonra ne olur?
Göreceğiniz önceki soruların yanıtlarını bağlı olarak, **düzeltme** düğmesini Azure AD'den kullanılabilir bir düzeltme olduğunda. Beklenmeyen Azure ile şirket içi nesne bu durumda, eşitliyor AD nesnesi. İki nesne kullanarak eşlenmiş **kaynak bağlantısı**. **Düzeltme** değişiklik alır bunlar veya benzer adımlar:
1. Güncelleştirmeleri **kaynak bağlantısı** Azure AD'de doğru nesnesine.
2. Varsa, Azure AD'de çakışan nesneyi siler.

![Eşitleme hatası düzeltme tanılama](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixAfterFix.png)

>[!IMPORTANT]
> **Düzeltme** değişikliği yalnızca yalnız bırakılmış nesneye çalışmaları için geçerlidir.
>

Önceki adımlar, varolan bir nesne için bir bağlantı özgün kaynak kullanıcı erişebilir. **Durumunu tanılamak** değeri liste görünümünde güncelleştirmeleri **bekleyen eşitleme**. Eşitleme hatası sonraki eşitlemeden sonra çözümlenir. Sistem durumu uzun gösterilmiyor liste görünümünde çözümlenmiş eşitleme hatası bağlayın.


## <a name="faq"></a>SSS
**S.** Ne olur yürütülmesi **düzeltme** başarısız oluyor?  
**C.** Yürütme başarısız olursa, Azure AD Connect verme hatayla çalışıyor mümkündür. Portal sayfayı yenileyin ve sonraki eşitlemeden sonra yeniden deneyin. Varsayılan eşitleme döngüsü 30 dakikadır. 


**S.** Ne **varolan nesne** silinecek nesne olmalıdır?  
**C.** Varsa **varolan nesne** olmalıdır silinmiş, işlem değişikliği içermeyen **kaynak bağlantısı**. Genellikle, şirket içi Active Directory'den olarak düzeltebilir. 


**S.** Hangi izni düzeltmeyi uygulamak bir kullanıcı mu?  
**C.** **Genel yönetici**, veya **katkıda bulunan** RBAC ayarlarından tanılama ve sorun giderme işlemi erişmek için izne sahip.


**S.** Bu özellik için Azure AD Connect Health aracısını güncelleştirme veya Azure AD Connect yapılandırma gerekiyor mu?  
**C.** Hayır, tanılama tam bir bulut tabanlı özelliği işlemidir.


**S.** Varolan nesne silinmiş de yumuşak ise, tanılama işlemi nesne yeniden etkin hale getirir?  
**C.** Hayır, düzeltme nesne öznitelikleri dışında güncelleştirme olmaz **kaynak bağlantısı**.
