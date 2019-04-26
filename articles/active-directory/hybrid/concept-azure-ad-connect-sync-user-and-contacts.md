---
title: 'Azure AD Connect eşitleme: Kullanıcıları, grupları ve kişileri anlama | Microsoft Docs'
description: Kullanıcıları, grupları ve kişileri Azure AD Connect Eşitleme'deki açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 8d204647-213a-4519-bd62-49563c421602
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/15/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 661747754369c17ca98ae69d477e04124b6a2942
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60245486"
---
# <a name="azure-ad-connect-sync-understanding-users-groups-and-contacts"></a>Azure AD Connect eşitleme: Kullanıcıları, grupları ve kişileri anlama
Neden birden çok Active Directory ormanı gerekir ve birçok farklı dağıtım topolojileri yoksa birkaç farklı nedeni vardır. Ortak bir hesap-kaynak dağıtımı ve GAL sync'ed ormanları birleşme ve alım sonra modelleridir. Ancak karma modeller de olsa bile saf modelleri, ortaktır. Varsayılan yapılandırma, Azure AD Connect eşitleme herhangi bir model varsaymaz ancak nasıl kullanıcı eşleşen Yükleme Kılavuzu'nda seçilen bağlı olarak, farklı davranışları gösterilebilir.

Bu konu başlığında, varsayılan yapılandırması bazı Topolojilerde nasıl davranacağını üzerinden geçer. Yapılandırma aracılığıyla ele alacağız ve eşitleme kuralları Düzenleyicisi yapılandırmasını aramak için kullanılabilir.

Yapılandırmasını varsaydığından birkaç genel kurallar şunlardır:
* Biz etkin dizinler kaynaktan almak sipariş bağımsız olarak, sonuç her zaman aynı olmalıdır.
* Etkin bir hesap oturum açma bilgileri, her zaman katkıda bulunan dahil olmak üzere **userPrincipalName** ve **sourceAnchor**.
* Bulunacak etkin bir hesabınız yok ise bağlı bir posta kutusu değilse devre dışı bırakılmış hesapla userPrincipalName ve sourceAnchor, katkıda bulunur.
* Bağlı bir posta kutusu olan bir hesabın hiçbir zaman userPrincipalName ve sourceAnchor için kullanılır. Etkin bir hesap daha sonra bulunması varsayılır.
* Bir kişi nesnesini Azure AD'ye bir kişi veya bir kullanıcı olarak sağlanan. Tüm kaynak Active Directory ormanları işlenen kadar gerçekten tanımadığınız.

## <a name="groups"></a>Gruplar
Gruplar Active Directory'den Azure AD'ye eşitleme yaparken dikkat edilmesi gereken önemli noktalar:

* Azure AD Connect dizin eşitlemesini yerleşik güvenlik grupları hariç tutar.

* Azure AD Connect eşitleme desteklemiyor [birincil grup üyeliklerini](https://technet.microsoft.com/library/cc771489(v=ws.11).aspx) Azure AD'ye.

* Azure AD Connect eşitleme desteklemiyor [dinamik dağıtım grup üyeliklerini](https://technet.microsoft.com/library/bb123722(v=exchg.160).aspx) Azure AD'ye.

* Azure AD posta etkin bir grup olarak bir Active Directory grubuna eşitlemek için:

    * Varsa grup *proxyAddress* özniteliktir boş, kendi *posta* öznitelik değerine sahip olmalıdır

    * Varsa grup *proxyAddress* özniteliktir boş olmayan, en az bir SMTP proxy adresi değeri içermelidir. İşte bazı örnekler:
    
      * Olan proxyAddress özniteliği değerine sahip bir Active Directory grubu *{"X500:/0=contoso.com/ou=users/cn=testgroup"}* Azure AD'de posta etkin olmayacaktır. Bir SMTP adresi yok.
      
      * Değerleri olan proxyAddress özniteliği olan bir Active Directory grubu *{"X500:/0=contoso.com/ou=users/cn=testgroup","SMTP:johndoe\@contoso.com"}* Azure AD'de posta etkin olacaktır.
      
      * Değerleri olan proxyAddress özniteliği olan bir Active Directory grubu *{"X500:/0=contoso.com/ou=users/cn=testgroup", "smtp:johndoe\@contoso.com"}* de posta özellikli Azure AD'de olacaktır.

## <a name="contacts"></a>Kişiler
Farklı bir ormanda bir kullanıcıyı temsil eden kişiler sahip bir birleşme ve alım sonra GALSync çözüm iki veya daha fazla Exchange ormanları burada köprüleme yaygındır. Kişi nesnesi, her zaman posta özniteliği kullanılarak meta veri bağlayıcısı alanından katılıyor. Varsa zaten bir kişi nesnesi veya aynı e-posta adresine sahip kullanıcı nesnesi, nesneleri birleştirilir. Bu kuralda yapılandırılır **içinde ad – kişi birleştirme**. Ayrıca adlı bir kural yoktur **içinde ad – kişi ortak** meta veri deposu özniteliği için bir öznitelik akışı ile **sourceObjectType** sabiti ile **kişi**. Bu kural çok düşük önceliğe sahip bunu herhangi bir kullanıcı nesnesi aynı meta veri deposu nesnesi ve ardından kural katılmışsa **içinde ad – kullanıcı ortak** bu öznitelik için değer kullanıcı katkıda bulunur. Bu kural, bu öznitelik en az bir kullanıcı bulunamazsa hiçbir kullanıcı katıldıysanız başvurun ve kullanıcı değerleri gerekir.

Azure AD, bir giden kuralı nesneye sağlama **Out AAD için – başvurun katılın** bir kişi nesnesi varsa oluşturacak meta veri deposu özniteliği **sourceObjectType** ayarlanır **başvurun**. Bu öznitelik ayarlanırsa **kullanıcı**, kural **Out AAD için – kullanıcı katılın** bunun yerine bir kullanıcı nesnesi oluşturur.
Bir nesne kaynak etkin dizin alınan ve eşzamanlı kullanıcıya kişiden yükseltilir mümkündür.

Biz ilk ormanı içeri aktardığınızda Örneğin, bir GALSync topolojisinde biz iletişim nesneleri herkes için ikinci bir ormanda bulabilirsiniz. Bu, AAD bağlayıcı yeni kişi nesneler aşama. Biz daha sonra içeri aktarmak ve ikinci ormanın eşitlemek, biz gerçek kullanıcıları bulmak ve var olan meta veri deposu nesnelere siz de onlara katılın. Ardından aad'de kişi nesnesini silme eder ve bunun yerine yeni bir kullanıcı nesnesi oluşturun.

Kullanıcıların kişiler olarak burada gösterilir bir topoloji varsa, posta özniteliğinin yükleme kılavuzundaki şirket kullanıcıları eşleştirmek için seçtiğinizden emin olun. Daha sonra başka bir seçeneği belirlerseniz, bir sipariş bağımlı yapılandırmaya sahip olur. İlgili nesneler her zaman posta özniteliğini katılacağı ancak Yükleme Kılavuzu'nda bu seçenek seçildiyse kullanıcı, nesneyi yalnızca posta özniteliğini katılın. Kişi nesnesi kullanıcı nesnesi önce aldıysanız, ardından iki farklı aynı posta öznitelikle meta veri deposu nesnesi ile bulunabileceğini. Azure AD'ye dışarı aktarma sırasında bir hata oluşturulur. Bu davranış tasarım gereğidir ve bozuk veriler veya topoloji yükleme sırasında doğru tanımlanmadı gösterir.

## <a name="disabled-accounts"></a>Devre dışı bırakılmış hesapları
Devre dışı bırakılmış hesapları da Azure AD ile eşitlenir. Devre dışı bırakılmış hesapları, Exchange, örneğin Konferans salonu kaynakları temsil etmek için ortaktır. Bağlı bir posta kutusu kullanıcılarla işlemdir; daha önce belirtildiği gibi bunlar hiç bir hesap Azure AD'ye sağlanır.

Bir devre dışı bırakılmış kullanıcı hesabı bulunamadı, sonra başka bir etkin hesap biz bir sonraki bulmaz ve nesne userPrincipalName ve bulunan sourceAnchor Azure AD'ye sağlandığında varsayılır. Başka bir etkin hesap aynı meta veri deposu nesnesine katılacağı durumda, userPrincipalName ve sourceAnchor kullanılacaktır.

## <a name="changing-sourceanchor"></a>SourceAnchor değiştirme
Azure AD'ye bir nesne artık sourceAnchor değiştirilmesine izin verilmez sonra verildi olduğunda. Nesnenin ne zaman yapılmış meta veri deposu özniteliği dışarı **cloudSourceAnchor** ile ayarlanır **sourceAnchor** Azure AD tarafından kabul edilen değer. Varsa **sourceAnchor** değiştirilir ve eşleşmemesi **cloudSourceAnchor**, kural **Out AAD için – kullanıcı katılın** hata atar **sourceAnchor özniteliği Değiştirilen**. Aynı sourceAnchor nesne yeniden eşitlenebilmesi yeniden meta veri deposunda mevcut, bu nedenle, bu durumda, yapılandırma veya veri düzeltilmelidir.

## <a name="additional-resources"></a>Ek Kaynaklar
* [Azure AD Connect eşitleme: Eşitleme seçeneklerini özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)

