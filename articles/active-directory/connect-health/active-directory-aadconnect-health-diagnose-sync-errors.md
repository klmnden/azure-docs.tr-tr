---
title: Azure AD Connect Health - tanılama özniteliği eşitleme hatalarını yinelenen | Microsoft Docs
description: Bu belgede, yinelenen öznitelik eşitleme hatalarını tanılayın işlemi ve olası düzeltme doğrudan Azure portalından yalnız bırakılmış nesneye senaryolar açıklanmaktadır.
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
ms.openlocfilehash: 0a345fd3443fb33d6f5912c8a04677091e20ecc8
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="duplicate-attribute-sync-errors-diagnosis-and-remediation"></a>Yinelenen öznitelik eşitleme hatalarını tanılama ve düzeltme 

## <a name="overview"></a>Genel Bakış
Eşitleme hataları vurgulamanın bir adım alma, Azure Active Directory Connect Health yinelenen öznitelik eşitleme hatalarında sorun giderme ve yalnız bırakılmış nesneleri Azure AD'den düzeltmek için bir Self Servis düzeltmeyi deneyimi sunmaktadır. Tanılama özelliğini temel avantaj:
- Yinelenen öznitelik eşitleme hatası senaryoları daraltmak ve özel çözümler belirtmek için tanılama yordam sağlayın
- Tek tıklatma hatayı gidermek için Azure AD'den ayrılmış senaryoları için düzeltmeyi Uygula
- Yükseltme veya yapılandırma yok, bu özelliği etkinleştirmek için gereklidir.
Azure AD hakkında daha fazla ayrıntı okuma [esnekliği yinelenen](https://aka.ms/dupattributeresdocs).

## <a name="problems"></a>Sorunları
### <a name="common-scenario"></a>Yaygın bir senaryo
Zaman **QuarantinedAttributeValueMustBeUnique** ve **AttributeValueMustBeUnique** Eşitleme hataları gerçekleşir, Azure AD'de kullanıcı asıl adı veya Proxy adreslerini çakışması yaygın olduğu. Şirket içi tarafında çakışan kaynak nesnesi güncelleyerek Eşitleme hataları çözebilir. Eşitleme hatası aşağıdaki eşitlemeden sonra çözümlenir. Örneğin, aşağıdaki resimde iki kullanıcı kendi UserPrincipalName ihtilaf yaşıyor belirtir *Joe.J@contoso.com*. Çakışan nesneler Azure AD'de karantinaya alındı durumundadır. 

![Eşitleme hatası yaygın bir senaryo tanılama](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixCommonCase.png)

### <a name="orphaned-object-scenario"></a>Yalnız bırakılmış nesneye senaryosu
Bazen, mevcut bir kullanıcının kaynak bağlantısı kaybederse bulabilirsiniz. Şirket içinde AD içinde kaynak nesnesi silinmesini oldu, ancak silme sinyal değişiklik hiçbir zaman Azure AD'ye eşitlenen. Eşitleme altyapısı sorunu veya etki alanı geçişi gibi nedenlerle oluşabilir. Aynı nesne geri veya yeniden, mantıksal olarak mevcut kullanıcının kaynak yer işaretine eşitlemek için kullanıcı olması gerekir. Mevcut kullanıcının bulut yalnızca nesne olarak Azure AD'ye eşitlenen çakışan kullanıcı de görebilirsiniz ve var olan nesneye eşit eşleştirilemiyor. Kaynak bağlantısı yeniden eşlemek için doğrudan bir yolu yoktur. Daha fazla bilgi edinin [varolan KB](https://support.microsoft.com/help/2647098). Örneğin, Azure AD mevcut nesne Joe lisans korur. Farklı kaynak bağlantısı ile eşitlenen nesne Azure AD içinde yinelenen öznitelik durumu oluştu. Değişiklikleri şirket Can'ın AD Can'ın özgün kullanıcı (varolan nesne) Azure AD içinde uygulanması mümkün olmaz.  

![Eşitleme hatası yalnız bırakılmış nesneye senaryosu tanılama](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixOrphanedCase.png)

## <a name="diagnostic-and-troubleshooting-steps-in-connect-health"></a>Tanılama ve sorun giderme adımları Connect Health içinde 
Özellik destekler aşağıdaki yinelenen özelliklere sahip kullanıcı nesneler tanılamak:

| Öznitelik Adı | Eşitleme hata türleri|
| ------------------ | -----------------|
| userPrincipalName | QuarantinedAttributeValueMustBeUnique veya AttributeValueMustBeUnique | 
| ProxyAddresses | QuarantinedAttributeValueMustBeUnique veya AttributeValueMustBeUnique | 
| sipProxyAddress | AttributeValueMustBeUnique | 
| onPremiseSecurityIdentifier |  AttributeValueMustBeUnique |

>[!IMPORTANT]
> Gerektirdiği **genel yönetici** izni veya **katkıda bulunan** RBAC ayarlardan bu özelliği erişebilmelidir. 
>

Adımlar Azure portalından, eşitleme hata ayrıntılarını daraltmak ve daha belirli çözümleri sağlamak mümkün olacaktır:

![Eşitleme tanılamak hatasını tanılayın adımları](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixSteps.png)

Azure portalından belirli düzeltilebilir senaryoları tanımlamak için birkaç adım ile çalışmaya devam edebilirsiniz olacaktır:  
1.  Tanıla Durum sütununda durumu hata durumunu daraltmak ve potansiyel olarak Azure Active Directory'den doğrudan düzeltmek için olası bir sorun giderme akış olup olmadığını gösterir.
| Durum | Ne anlama geliyor? |
| ------------------ | -----------------|
| Başlatılmadı | Bu tanılama işlemi gittiğiniz değil. Bağımlı tanılama sonucuna potansiyel olarak doğrudan portal eşitleme hatayı düzeltmek için bir yolu yoktur. |
| El ile Düzeltme Gerekli | Hata ölçütleri portalından kullanılabilir düzeltme içine uymuyor. Durum türleri (2) zaten tanılama adımları ve no gittiğiniz kullanıcılar çözüm Portalı'ndan kullanılabilen düzeltme değil (1) çakışan nesne olabilir. Bu durumda, şirket içi yan düzeltme çözümlerden birini olmaya devam eder. [Şirket içi hakkında daha fazla gidermek okuma](https://support.microsoft.com/help/2647098) | 
| Beklenen Eşitleme | Düzeltme uygulandı. Hata temizlemek bir sonraki eşitleme döngüsü için bekleniyor. |
>[!IMPORTANT]
> Tanı durumu sütun her eşitleme döngüsünden sonra sıfırlanır. 
>

2.  Tıklayarak **Tanıla** düğmesi hata ayrıntıları dikey penceresinde, birkaç soruları yanıtlayın ve eşitleme hata ayrıntıları tanımlamak gerekir. Sorulara yanıt verilmesi, yalnız bırakılmış nesneye senaryo durumunun tanımlanmasına yardımcı olacak. 

3.  Varsa bir **Kapat** düğmesi tanılama sonunda bu hızlı düzeltme verilen yanıtlara bağlı portalından kullanılabilir anlamına gelir. Son adımda gösterilen çözümde bakın. Şirket içi düzeltme çözümleri olmaya devam eder. Kapat düğmesine tıkladıktan sonra geçerli eşitleme hatası durumunu olacak şekilde değiştirmek bulacaksınız **gerekli el ile düzeltme**. Durumu geçerli eşitleme döngüsü sırasında korur.

4.  Yalnız bırakılmış nesneye çalışması tanımlandıktan sonra yinelenen öznitelikler doğrudan portal üzerinden eşitleme hatalarını düzeltmeyi kuramaz. Tıklayın **düzeltme** işlemi tetiklemek için düğmesi. Geçerli eşitleme hatası durumunu olacak şekilde güncelleştirecektir **eşitleme bekleyen**.

5.  Aşağıdaki eşitleme döngüsünden sonra hata listesinden kaldırılması gerekir.

## <a name="how-to-answer-the-diagnosis-questions"></a>Tanılama soruları yanıtlamak nasıl 
### <a name="does-the-user-exist-in-your-on-premises-active-directory"></a>Kullanıcı, şirket içi Active Directory var mı?

Soru kaynak nesnesi mevcut kullanıcının şirket içi Active Directory'den tanımlamak çalışıyor.  
1.  Active Directory sağlanan UserPrincipalName sahip bir nesne olup olmadığını denetleyin. Öyle değilse, yanıt No
2.  Varsa, nesnenin hala Eşitleme kapsamında olup olmadığını denetleyin.  
  - Azure AD Bağlayıcı Alanında DN ile arayın.
  - Nesne ile bulunursa **bekleyen eklemek** durum, yanıt No Azure AD Connect nesneyi sağ AD nesnesine bağlanabiliyor değil.
  - Nesne bulunmazsa Evet yanıtı.

> Aşağıdaki diyagramda alma, örneğin, soru belirlemek çalışıyor *Joe Jackson* hala şirket içi Active Directory'de yer alır.
İçin **yaygın bir senaryo**, her iki kullanıcı *Joe Johnson* ve *Joe Jackson* , şirket içi Active Directory'de mevcut olacaktır. Karantinaya alınan nesneleri iki farklı kullanıcılardır.

![Eşitleme hatası yaygın bir senaryo tanılama](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixCommonCase.png)

> İçin **yalnız bırakılmış nesneye senaryo**, yalnızca tek kullanıcı – *Joe Johnson* şirket içi Active Directory'den mevcut olacaktır:

![Eşitleme hatası yalnız bırakılmış nesneye senaryosu tanılama](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixOrphanedCase.png)

### <a name="do-both-these-accounts-belong-to-the-same-user"></a>Bu iki hesap aynı kullanıcıya mı ait?
Soru, aynı kullanıcıya ait olmadığını görmek için Azure AD, gelen çakışan kullanıcı ve var olan kullanıcı nesnesi denetliyor.  
1.  Çakışan nesne yeni Azure Active Directory ile eşitlenmiş. Nesneden karşılaştırmak kendi:  
  - Görünen ad
  - Kullanıcı Asıl Adı
  - Nesne kimliği
2.  Bunları karşılaştırmak için başarısız olursa, Active Directory ile sağlanan UserPrincipalNames nesnelere sahip denetleyin. Her ikisi de bulunursa Hayır yanıtı.  

> Durumda iki nesnenin aynı kullanıcıya ait *Joe Johnson*.

![Eşitleme hatası yalnız bırakılmış nesneye senaryosu tanılama](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixOrphanedCase.png)


## <a name="what-happened-after-fix-is-applied-for-orphaned-object-scenario"></a>Yalnız bırakılmış nesneye senaryo için düzeltme uygulandıktan sonra ne oldu
Yükseltilmiş soruların yanıtları temel alarak, görmeye siz olacaksınız **düzeltme** düğmesini Azure AD'den kullanılabilir bir düzeltme olduğunda. Beklenmeyen Azure ile şirket içi nesne bu durumda, eşitliyor AD nesnesi. İki nesne, "Kaynak bağlantısı" kullanarak eşlenir. Uygula değiştirme adımları aşağıdaki gibi gerçekleştirin:
- Kaynak bağlantısı doğru nesnesine Azure AD'de güncelleştirin.
- Bunu sunarsa Azure AD'de çakışan nesnesini silin.

![Eşitleme hatası düzeltme tanılama](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixAfterFix.png)

>[!IMPORTANT]
> Geçerli düzeltme değişiklik yalnızca yalnız bırakılmış nesneye çalışmaları için geçerlidir.
>

Yukarıdaki adımları sonra kullanıcı varolan nesne bağlantıdır özgün kaynak erişebiliyor olacaktır. **Durumunu tanılamak** liste görünümünde değer olacak şekilde güncelleştirilir **bekleyen eşitleme**. Eşitleme hatası aşağıdaki eşitlemeden sonra çözümlenir. Bağlantı durumu çözümlenmiş eşitleme hatası liste görünümündeki artık gösterilmez. 


## <a name="faq"></a>SSS
 -  Uygula yürütülmesi başarısız olursa ne oldu?  
Yürütme başarısız olursa, olası Azure AD olduğu Bağlan verme hatası aynı anda çalışıyor. Portal sayfayı yenileyin ve aşağıdaki eşitlemeden sonra yeniden deneyin. Varsayılan eşitleme döngüsü 30 dakikadır. 

 -  Ne **varolan nesne** silinecek nesne olmalıdır?  
Varolan nesne bu durumda silinmesi gerekir, işlem kaynak bağlantısı, değişiklik gerektirmez. Şirket içinden Düzelt açabilmelisiniz AD.  

 -  Düzeltme uygulayabilmesi kullanıcı izinlerini nedir?  
Genel yönetici veya katkıda bulunan RBAC ayarlarından tanılama ve sorun giderme işlemi erişmek için izne sahip olur.

 -  I yapılandırması için AAD Connect veya bu özellik için Azure AD Connect Health aracısını güncelleştirme?  
Hayır, tam bir bulut tabanlı özelliği tanılama işlemidir.

 -  Varolan nesne silinmiş de yumuşak ise, Tanıla işlem tekrar etkin olması için nesne geri?  
Hayır, düzeltme nesne özniteliği kaynak bağlantısı dışında güncelleştirmez. 
