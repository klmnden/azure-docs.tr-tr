---
title: "Azure AD Connect eşitleme: kullanıcıları, grupları ve kişileri anlama | Microsoft Docs"
description: "Kullanıcılar, gruplar ve kişiler Azure AD Connect Eşitleme'de açıklanmaktadır."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 8d204647-213a-4519-bd62-49563c421602
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: markvi;andkjell
ms.openlocfilehash: 7bb7bdba21d83817cf5579e779a6a4d509753c01
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="azure-ad-connect-sync-understanding-users-groups-and-contacts"></a>Azure AD Connect eşitleme: kullanıcıları, grupları ve kişileri anlama
Neden birden çok Active Directory ormanına gerekir ve birçok farklı dağıtım topolojileri vardır birkaç farklı nedenleri vardır. Ortak modelleri birleşme & edinme sonra bir hesap-kaynak dağıtımı ve GAL sync'ed ormanları içerir. Ancak karma modeller de olsa bile saf modelleri, yaygındır. Azure AD Connect eşitleme Varsayılan yapılandırmada herhangi belirli bir modeli varsaymaz ancak nasıl kullanıcı eşleştirme Yükleme Kılavuzu'nda seçilen bağlı olarak, farklı davranışlar gösterilebilir.

Bu konuda, varsayılan yapılandırması bazı topolojide nasıl davranacağını aracılığıyla ele alacağız. Yapılandırma aracılığıyla ele alacağız ve eşitleme kuralları Düzenleyicisi yapılandırmasını aramak için kullanılabilir.

Yapılandırma varsayar birkaç genel kurallar şunlardır:
* Biz etkin dizinleri kaynağından almak sırasını bağımsız olarak, sonuç her zaman aynı olmalıdır.
* Etkin bir hesap oturum açma bilgileri, her zaman katkıda dahil olmak üzere **userPrincipalName** ve **sourceAnchor**.
* Bulunacak etkin hesap ise bağlı bir posta kutusu olmadığı sürece devre dışı bırakılmış bir hesaba userPrincipalName ve sourceAnchor, katkıda bulunur.
* Bağlantılı bir posta kutusuna sahip bir hesap hiçbir zaman userPrincipalName ve sourceAnchor için kullanılır. Etkin bir hesabı daha sonra bulunamadı varsayılır.
* Bir kişi nesnesi Azure AD ile bir kişi veya bir kullanıcı olarak sağlanan. Tüm kaynak Active Directory ormanları işlenen kadar gerçekten tanımadığınız.

## <a name="groups"></a>Gruplar
Active Directory gruplarına Azure AD eşitleme yaparken dikkat edilmesi gereken önemli noktalar:

* Azure AD Connect dizin eşitlemesini yerleşik güvenlik grupları dışlar.

* Azure AD Connect eşitleme desteklemiyor [birincil grup üyeliklerini](https://technet.microsoft.com/library/cc771489(v=ws.11).aspx) Azure ad.

* Azure AD Connect eşitleme desteklemiyor [dinamik dağıtım grup üyeliklerini](https://technet.microsoft.com/library/bb123722(v=exchg.160).aspx) Azure ad.

* Bir Active Directory grubu posta etkin bir grup olarak Azure ad ile eşitlemek için:

    * Varsa grubun *proxyAddress* özniteliği boş olduğundan, kendi *posta* özniteliği değerine sahip olmalıdır

    * Varsa grubun *proxyAddress* özniteliği boş olmayan, en az bir SMTP proxy adresi değeri içermelidir. İşte bazı örnekler:
    
      * ProxyAddress öznitelik değeri olan bir Active Directory grubu *{"X500:/0=contoso.com/ou=users/cn=testgroup"}* Azure AD'de posta etkin olmaz. Bir SMTP adresi yok.
      
      * Bir Active Directory grubu olan proxyAddress özniteliğine sahip değerleri *{"X500:/0=contoso.com/ou=users/cn=testgroup","SMTP:johndoe@contoso.com"}* Azure AD'de posta etkin olacaktır.
      
      * Bir Active Directory grubu olan proxyAddress özniteliğine sahip değerleri *{"X500:/0=contoso.com/ou=users/cn=testgroup", "smtp:johndoe@contoso.com"}* de Azure AD'de posta etkin olur.

## <a name="contacts"></a>Kişiler
Farklı bir ormanda bir kullanıcıyı temsil eden kişiler sahip bir birleşme & edinme sonra GALSync çözüm iki veya daha fazla ormanlarında Exchange burada köprüleme yaygındır. Kişi nesnesi, her zaman posta özniteliğini kullanarak meta veri bağlayıcısı alanından katılıyor. Zaten varsa bir kişi nesnesi veya kullanıcı nesnesi aynı posta adresiyle, nesnelerin araya birleştirilir. Bu kuralda yapılandırılmış **içinde AD'den – kişi katılma**. Adlı bir kural bulunmaktadır **içinde AD'den – kişi ortak** meta veri deposu özniteliği için bir öznitelik akışı ile **sourceObjectType** sabiti ile **kişi**. Bu kural çok düşük önceliğe sahip bunu herhangi bir kullanıcı nesnesi aynı meta veri deposu nesnesi, sonra kural için katılmışsa **içinde AD'den – kullanıcı ortak** bu öznitelik için değer kullanıcı katkıda bulunur. En az bir kullanıcı bulunamazsa, bu kural, bu öznitelik değeri hiçbir kullanıcı birleştirdiğinizde başvurun ve değer kullanıcı sahiptir.

Azure AD, giden kuralı nesneye sağlama **Out AAD'ye – başvurun katılma** , bir kişi nesnesi oluşturacak meta veri deposu özniteliği **sourceObjectType** ayarlanır **başvurun**. Bu öznitelik ayarlanırsa **kullanıcı**, sonra kural **Out AAD'ye – kullanıcı katılma** bunun yerine bir kullanıcı nesnesi oluşturacak.
Bir nesne daha fazla kaynak etkin dizinler aktarılması ve eşitlenmiş kullanıcı için bir kişiden yükseltilir mümkündür.

Biz ilk ormanı içeri aktardığınızda Örneğin, bir GALSync topolojisinde biz kişi nesneleri herkes için ikinci ormanda bulur. Bu, AAD bağlayıcı yeni kişi nesneler aşama. Biz daha sonra almak ve ikinci ormanın eşitlemek, biz gerçek kullanıcıları bulmak ve var olan meta veri deposu nesnelere ekleyin. Biz sonra AAD kişi nesnesinde silin ve yeni bir kullanıcı nesnesi oluşturun.

Kullanıcılar, kişiler olarak burada sunulur bir topoloji varsa, kurulum kılavuzunda posta öznitelikte kullanıcıları eşleştirmek için seçtiğinizden emin olun. Başka bir seçeneği belirlerseniz, bir siparişi bağımlı yapılandırmaya sahip olur. Kişi nesneler her zaman posta özniteliği katılacak, ancak Yükleme Kılavuzu'nda bu seçenek seçildiyse kullanıcı nesneleri yalnızca posta özniteliği katılın. Kişi nesnesi önce kullanıcı nesnesi aldıysanız, sonra iki farklı aynı posta özniteliği ile meta veri nesnesi şunun. Azure ad dışa aktarma sırasında bir hata oluşturulur. Bu davranış tasarım gereğidir ve hatalı veri veya topoloji yükleme sırasında doğru tanımlanmadı gösterir.

## <a name="disabled-accounts"></a>Devre dışı bırakılan hesapları
Devre dışı bırakılan hesapları da Azure AD ile eşitlenir. Devre dışı bırakılmış Exchange, örneğin konferans odaları kaynaklarında temsil etmek için ortak hesaplarıdır. Bağlı bir posta kutusu kullanıcılarla istisnadır; daha önce belirtildiği gibi bunlar hiç Azure AD için bir hesap hazırlayacağınız.

Devre dışı bırakılmış kullanıcı hesabı bulunamadı, daha sonra başka bir etkin hesabı sizi bir sonraki bulamaz ve nesne sourceAnchor bulunamadı ve userPrincipalName ile Azure ad sağlandığında olduğu varsayılır. Başka bir etkin hesabı aynı meta veri deposu nesnesine katılacak durumunda, userPrincipalName ve sourceAnchor kullanılır.

## <a name="changing-sourceanchor"></a>SourceAnchor değiştirme
SourceAnchor artık değiştirmek için izin verilmiyor sonra nesneyi Azure AD'ye aktarılan olduğunda. Nesnenin ne zaman yapılmış meta veri deposu özniteliği dışarı **cloudSourceAnchor** ile ayarlamak **sourceAnchor** Azure AD tarafından kabul edilen değer. Varsa **sourceAnchor** değiştirilir ve eşleşmiyor **cloudSourceAnchor**, kural **Out AAD'ye – kullanıcı katılma** hata atar **sourceAnchor özniteliği değişti**. Aynı sourceAnchor nesne yeniden eşitlenebilmesi yeniden meta veri deposunda mevcut olacak şekilde, bu durumda, yapılandırma veya veri düzeltilmelidir.

## <a name="additional-resources"></a>Ek Kaynaklar
* [Azure AD Connect eşitleme: Eşitleme seçeneklerini özelleştirme](active-directory-aadconnectsync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)

