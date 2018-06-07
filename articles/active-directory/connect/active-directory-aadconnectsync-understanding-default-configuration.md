---
title: 'Azure AD Connect eşitleme: varsayılan yapılandırmayı anlama | Microsoft Docs'
description: Bu makalede, Azure AD Connect eşitleme Varsayılan yapılandırmada açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: ed876f22-6892-4b9d-acbe-6a2d112f1cd1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: f4278dc3af1074b6de299444d2b205396bc0a9c0
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34595317"
---
# <a name="azure-ad-connect-sync-understanding-the-default-configuration"></a>Azure AD Connect Eşitleme: Varsayılan yapılandırmayı anlama
Bu makalede out-of-box yapılandırma kuralları açıklanır. Kurallar ve bu kuralları yapılandırmasını nasıl etkiler belgeler. Bu ayrıca, Azure AD Connect eşitleme varsayılan yapılandırma açıklanmaktadır. Okuyucu bildirim temelli hazırlama, adlı yapılandırma modeli gerçek örnekte nasıl çalıştığını algıladığını hedeftir. Bu makale, zaten yüklediyseniz ve Yükleme Sihirbazı'nı kullanarak Azure AD Connect eşitleme yapılandırma varsayar.

Yapılandırma modeli ayrıntılarını anlamak için okuyun [anlama bildirim temelli hazırlama](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

## <a name="out-of-box-rules-from-on-premises-to-azure-ad"></a>Azure ad şirket içi Out-of-box kuralları
Aşağıdaki ifadeler out-of-box yapılandırmada bulunamadı.

### <a name="user-out-of-box-rules"></a>Kullanıcı out-of-box kuralları
Bu kurallar iNetOrgPerson nesne türü için de geçerlidir.

Bir kullanıcı nesnesi eşitlenecek aşağıdaki karşılaması gerekir:

* Bir sourceAnchor olması gerekir.
* Azure AD içinde nesne oluşturulduktan sonra sourceAnchor değiştiremezsiniz. Değiştirilen şirket içi değerse nesne sourceAnchor önceki değerine değiştirilene kadar eşitleme durdurur.
* AccountEnabled (userAccountControl) öznitelik doldurulmuş gerekir. Bir şirket içi Active Directory ile bu öznitelik her zaman mevcut ve doldurulan durumda.

Aşağıdaki kullanıcı nesneler **değil** Azure AD'ye eşitlenen:

* `IsPresent([isCriticalSystemObject])`. Yerleşik yönetici hesabı gibi birçok out-of-box nesneleri, Active Directory içinde emin olun, eşitlenmez.
* `IsPresent([sAMAccountName]) = False`. Kullanıcı nesneleri sAMAccountName özniteliği ile eşitlenmemiş emin olun. Bu durumda yalnızca pratikte NT4 yükseltilmiş bir etki alanında gerçekleşir.
* `Left([sAMAccountName], 4) = "AAD_"`, `Left([sAMAccountName], 5) = "MSOL_"`. Azure AD Connect eşitleme ve önceki sürümleri tarafından kullanılan hizmet hesabını eşitlemez.
* Exchange Online çalışmayan Exchange hesaplarına eşitlemez.
  * `[sAMAccountName] = "SUPPORT_388945a0"`
  * `Left([mailNickname], 14) = "SystemMailbox{"`
  * `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`
  * `(Left([sAMAccountName], 4) = "CAS_" && (InStr([sAMAccountName], "}")> 0))`
* Exchange Online çalışmayan nesneleri eşitlemez.
  `CBool(IIF(IsPresent([msExchRecipientTypeDetails]),BitAnd([msExchRecipientTypeDetails],&H21C07000) > 0,NULL))`  
  Bu bit maskesi (& H21C07000) aşağıdaki nesnelerini filtrelemek:
  * Posta etkinleştirilmiş ortak klasör (içinde sürüm 1.1.524.0 itibariyle Önizleme)
  * Sistem Katılımcısı posta kutusu
  * Posta kutusu veritabanı posta kutusu (sistem posta)
  * Evrensel güvenlik grubu (bir kullanıcı için geçerli olmayacaktır, ancak eski nedenlerle varsa)
  * Olmayan Evrensel Grup (bir kullanıcı için geçerli olmayacaktır, ancak eski nedenlerle varsa)
  * Posta kutusu planı
  * Bulma posta kutusu
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Herhangi bir çoğaltma kurban nesne eşitlemez.

Aşağıdaki öznitelik kurallar geçerlidir:

* `sourceAnchor <- IIF([msExchRecipientTypeDetails]=2,NULL,..)`. SourceAnchor özniteliği bağlantılı bir posta kutusundan katkıda değil. Bağlı bir posta kutusu bulunursa gerçek daha sonra katılmışsa varsayılır.
* Exchange ilgili öznitelikleri yalnızca, eşitlenen öznitelik **mailNickName** bir değere sahip.
* Birden çok ormanınız olduğunda öznitelikleri aşağıdaki sırayla kullanılır:
  1. Oturum açmak için ilgili öznitelikleri (örneğin userPrincipalName) etkin bir hesapla ormandan katkısı.
  2. Bir Exchange GAL (genel adres listesi) bulunabilir öznitelikler, ormandaki bir Exchange posta kutusu ile katkıda.
  3. Posta kutusu bulunabilir, bu öznitelikler herhangi bir orman gelebilir.
  4. Exchange ilgili öznitelikleri (GAL görünür teknik öznitelikler) ormandan katkıda bulunan burada `mailNickname ISNOTNULL`.
  5. Bu kurallardan biri karşılar birden çok orman varsa, oluşturma sırasını (tarih) (orman) bağlayıcıları hangi ormanın katkıda bulunan öznitelikler belirlemek için kullanılır.

### <a name="contact-out-of-box-rules"></a>Out-of-box kuralları başvurun
Bir kişi nesnesi eşitlenecek aşağıdaki karşılaması gerekir:

* Kişi, posta etkinleştirilmiş olması gerekir. Aşağıdaki kurallar ile doğrulanır:
  * `IsPresent([proxyAddresses]) = True)`. ProxyAddresses özniteliği doldurulmalıdır.
  * Bir birincil e-posta adresi proxyAddresses özniteliği veya posta özniteliğinin bulunabilir. Varlığını bir @ içerik bir e-posta adresi olduğunu doğrulamak için kullanılır. Bu iki kuralın birini True değerlendirilmelidir.
    * `(Contains([proxyAddresses], "SMTP:") > 0) && (InStr(Item([proxyAddresses], Contains([proxyAddresses], "SMTP:")), "@") > 0))`. Olan bir giriş yok "SMTP:" ve varsa, bir dizede bulunan @ olabilir?
    * `(IsPresent([mail]) = True && (InStr([mail], "@") > 0)`. Posta özniteliğinin doldurulur ve değilse, bir dizede bulunan @ olabilir?

Aşağıdaki kişi nesneler **değil** Azure AD'ye eşitlenen:

* `IsPresent([isCriticalSystemObject])`. Kritik olarak işaretlenmelidir herhangi bir kişi nesnenin eşitlenir emin olun. Herhangi bir varsayılan yapılandırmaya sahip olmamalıdır.
* `((InStr([displayName], "(MSOL)") > 0) && (CBool([msExchHideFromAddressLists])))`.
* `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`. Bu nesneler Exchange Online iş olmayacaktır.
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Herhangi bir çoğaltma kurban nesne eşitlemez.

### <a name="group-out-of-box-rules"></a>Grup out-of-box kuralları
Bir grup nesnesi eşitlenecek aşağıdaki karşılaması gerekir:

* 50. 000'den az üyeleri olmalıdır. Bu sayı şirket içi gruptaki üyeler sayısıdır.
  * İlk kez eşitleme başlamadan önce daha fazla üye varsa, Grup eşitlenmemiş.
  * Üye sayısı zaman başlangıçta oluşturulduğu, ardından 50.000 üyeleri ulaştığında büyüme, yeniden üyelik sayısı 50. 000 ' daha düşük olana kadar eşitleme durdurur.
  * Not: 50.000 üyelik sayısı ayrıca Azure AD tarafından zorlanır. Değiştirdiğinizde ya da bu kuralı kaldırmak olsa bile grupları ile daha fazla üye eşitlemek mümkün değildir.
* Grupta bir **dağıtım grubu**, posta etkinleştirilmiş olmalıdır. Bkz: [başvurun out-of-box kuralları](#contact-out-of-box-rules) için bu kural zorlanır.

Aşağıdaki Grup nesneler **değil** Azure AD'ye eşitlenen:

* `IsPresent([isCriticalSystemObject])`. Yerleşik Yöneticiler grubunun gibi birçok out-of-box nesneleri, Active Directory içinde emin olun, eşitlenmez.
* `[sAMAccountName] = "MSOL_AD_Sync_RichCoexistence"`. DirSync tarafından kullanılan eski grubu.
* `BitAnd([msExchRecipientTypeDetails],&amp;H40000000)`. Rol grubu.
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Herhangi bir çoğaltma kurban nesne eşitlemez.

### <a name="foreignsecurityprincipal-out-of-box-rules"></a>ForeignSecurityPrincipal out-of-box kuralları
FSP "herhangi" birleştirilir (\*) meta veri nesnesi. Gerçekte, bu birleştirme yalnızca kullanıcılar ve güvenlik grupları için gerçekleşir. Bu yapılandırma, ormanlar arası üyeliklerini çözümlendi ve Azure AD içinde doğru şekilde temsil sağlar.

### <a name="computer-out-of-box-rules"></a>Bilgisayar out-of-box kuralları
Bir bilgisayar nesnesi eşitlenecek aşağıdaki karşılaması gerekir:

* `userCertificate ISNOTNULL`. Yalnızca Windows 10 bilgisayarlar, bu öznitelik doldurun. Bu öznitelik alanında bir değere sahip tüm bilgisayar nesneleri eşitlenir.

## <a name="understanding-the-out-of-box-rules-scenario"></a>Out-of-box kurallar senaryo anlama
Bu örnekte, bir hesap ormanı (A) olan bir dağıtım, bir kaynak ormanı (R) ve bir Azure AD dizini kullanıyoruz.

![Senaryo açıklaması ile resim](./media/active-directory-aadconnectsync-understanding-default-configuration/scenario.png)

Bu yapılandırmada hesap ormanındaki etkin bir hesap ve kaynak ormanında bağlı bir posta kutusu ile devre dışı bırakılmış bir hesabı olduğu varsayılır.

Amacımız varsayılan yapılandırması şöyledir:

* Oturum açmak için ilgili öznitelikleri ormandaki etkin hesabıyla eşitlenir.
* GAL (genel adres listesi) bulunabilir öznitelikleri ormandaki posta kutusu ile eşitlenir. Posta kutusu bulunabilir, başka bir ormana kullanılır.
* Bağlı bir posta kutusu bulunursa, hesabın bağlantılı etkinleştirilmiş Azure AD'ye aktarılacak nesnesinin bulunması gerekir.

### <a name="synchronization-rule-editor"></a>Eşitleme kuralı Düzenleyicisi
Yapılandırma görüntülenebilir ve eşitleme kuralları Düzenleyicisi'ni (SRE) aracıyla değişti ve Başlat menüsünde kısayolu bulunabilir.

![Eşitleme kuralları Düzenleyicisi simgesi](./media/active-directory-aadconnectsync-understanding-default-configuration/sre.png)

SRE bir Kaynak Seti aracıdır ve Azure AD Connect eşitleme ile yüklenir. Başlatmak kullanabilmek için ADSyncAdmins grubunun bir üyesi olması gerekir. Başlatıldığında, aşağıdakine benzer bakın:

![Gelen eşitleme kuralları](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

Bu bölümde, tüm eşitleme yapılandırmanız için oluşturulan kuralların bakın. Tablodaki her satır bir eşitleme kuralıdır. Kural türleri altında sola iki farklı türleri listelenmiştir: gelen ve giden. Gelen ve giden meta veri görünümünden olur. Bu genel bakışta gelen kurallarında odağı çoğunlukla çağıracaksınız. Eşitleme kuralları gerçek listesi, AD içinde algılanan şemaya göre değişir. Yukarıdaki resimde hesap ormanı (fabrikamonline.com) Exchange ve Lync gibi tüm Hizmetleri yüklü değil ve bu hizmetler için hiçbir eşitleme kuralı oluşturuldu. Ancak, kaynak ormandaki (res.fabrikamonline.com), eşitleme kuralları bu hizmetler için bulun. Kuralları içeriğini sürümü algılandı bağlı olarak farklılık gösterir. Örneğin, Exchange 2013 ile bir dağıtımda vardır yapılandırılmış Exchange 2010/2007'den daha fazla öznitelik akışları.

### <a name="synchronization-rule"></a>Eşitleme Kuralı
Bir koşul yerine getirdiğinizde akan öznitelikleri kümesi içeren bir yapılandırma nesnesi bir eşitleme kuralıdır. Bağlayıcı alanı içinde bir nesne olarak bilinen meta veri nesnesine nasıl ilişkili olduğunu açıklamak için de kullanılır **birleştirme** veya **eşleşen**. Eşitleme kuralları birbirleriyle nasıl ilişkili olduğunu belirten bir öncelik değerine sahip. Daha düşük bir sayısal değer ile bir eşitleme kuralı daha yüksek bir önceliği ve bir öznitelik akışı çakışması çakışma çözümü daha yüksek önceliği WINS.

Örnek olarak, eşitleme kuralını arayın **içinde AD'den – kullanıcı AccountEnabled**. Bu satırı seçin ve SRE işaretlemek **Düzenle**.

Bu kural bir out-of-box kuralı olduğundan, kuralın açtığınızda bir uyarı alırsınız. Herhangi bir yapmamalısınız [değişiklikleri out-of-box kuralları](active-directory-aadconnectsync-best-practices-changing-default-configuration.md), bu nedenle, amaçları nelerdir sorulur. Bu durumda, yalnızca kural görüntülemek istiyorsunuz. Seçin **Hayır**.

![Eşitleme kuralları uyarı](./media/active-directory-aadconnectsync-understanding-default-configuration/warningeditrule.png)

Bir eşitleme kuralı dört yapılandırma bölümü vardır: filtresi, birleştirme kuralları ve dönüştürmeler kapsamlar açıklaması.

#### <a name="description"></a>Açıklama
İlk bölümü, bir ad ve açıklama gibi temel bilgiler sağlar.

![Açıklama sekmesinde eşitleme kural Düzenleyicisi ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruledescription.png)

Ayrıca hangi bağlı sistemi hakkında bu kural, ilgili bilgiler hangi nesne türü için geçerlidir bağlı sistemdeki ve meta veri deposu nesne türü bulunur. Kaynak nesne türü kullanıcı, iNetOrgPerson veya kişi meta veri deposu nesne türü her zaman kişi bağımsız olur. Genel bir tür oluşmasını meta veri deposu nesne türü hiçbir zaman değiştirmeniz gerekir. Bağlantı türü, birleştirme, StickyJoin veya sağlamak için ayarlanabilir. Bu ayar katılma kurallar bölümü ile birlikte çalışır ve daha sonra ele alınmıştır.

Bu eşitleme kuralı için parola eşitlemesi kullanıldığını da görebilirsiniz. Bir kullanıcı bu eşitleme kuralı kapsamına ise, parolayı (parola eşitleme özelliği etkin olduğu varsayılarak) bulut için şirket içi eşitlenir.

#### <a name="scoping-filter"></a>Kapsamı filtresi
Filtre kapsamı bölümünde bir eşitleme kuralı uyguladığınızda yapılandırmak için kullanılır. Kapsam, yalnızca uygulanması için etkin kullanıcılar, arıyor, eşitleme kuralının adını gösterir olduğundan, yapılandırılmış böylece AD özniteliği **userAccountControl** değil bit 2 ayarlamış olmanız gerekir. Eşitleme altyapısı AD içinde bir kullanıcı bulduğunda, bu eşitleme geçerlidir kural **userAccountControl** ondalık değer 512 (etkin normal kullanıcı). Kullanıcı sahip olduğunda kural uygulanmaz **userAccountControl** 514 (devre dışı bırakılmış normal kullanıcı) ayarlayın.

![Kapsam sekmesinde eşitleme kural Düzenleyicisi ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilter.png)

Kapsam filtresi grupları ve iç içe olabilir tümceler var. Bir eşitleme kuralı uygulamak bir grup içindeki tüm koşullar karşılanmalıdır. Birden çok grubu tanımlandığında, en az bir grup kuralı uygulamak karşılanması gerekir. Diğer bir deyişle, mantıksal veya grupları ve bir mantıksal arasında değerlendirilir ve bir grup içinde değerlendirilir. Bu yapılandırma örneği giden eşitleme kuralı bulunabilir **Out AAD'ye – gruba katılma**. Birkaç eşitleme filtre grubu vardır, örneğin bir güvenlik grupları (`securityEnabled EQUAL True`) ve dağıtım grupları için bir tane (`securityEnabled EQUAL False`).

![Kapsam sekmesinde eşitleme kural Düzenleyicisi ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilterout.png)

Bu kural, hangi grupların Azure AD ile sağlanması tanımlamak için kullanılır. Dağıtım grupları posta Azure AD ile eşitlenmesi için etkinleştirilmiş olması gerekir, ancak güvenlik grupları için bir e-posta gerekli değildir.

#### <a name="join-rules"></a>Kuralları katılma
Üçüncü bölüm ne meta veri nesneleri için bağlayıcı alanı nesne ilişkili yapılandırmak için kullanılır. Kural, arama sırasında daha önce herhangi bir yapılandırma kuralları, katılmak için bunun yerine bulacağınızı bakmak için yok **içinde AD'den – kullanıcı katılma**.

![Kurallar sekmesi eşitleme kuralı Düzenleyicisi'nde katılma ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulejoinrules.png)

Birleştirme kuralı içeriği Yükleme Sihirbazı'nda eşleşen seçeneğe göre değişir. Bir gelen kuralı için bir nesne kaynak bağlayıcı alanda değerlendirme başlatır ve birleştirme kuralları her grupta sırayla değerlendirilir. Kaynak nesne, birleşim kuralları birini kullanarak meta veri deposu içinde yalnızca bir nesneye eşleşecek şekilde değerlendirildiğinde nesneleri birleştirilir. Tüm kuralları değerlendirilemiyor ve eşleşmesi yok açıklama ekleyin sayfasında bağlantı türü kullanılır. Bu yapılandırma ayarlanmışsa **sağlama**, yeni bir nesne hedef, meta veri oluşturulur. Meta veri deposu için yeni bir nesne olarak da bilinen olduğu sağlama **proje** meta veri deposu için bir nesne.

Birleşim kuralları yalnızca bir kez değerlendirilir. Bağlayıcı alanı nesne ve meta veri deposu nesnesinin katıldığında, eşitleme kuralı kapsamını hala memnun olduğu sürece birleştirilmiş kalırlar.

Eşitleme kuralları değerlendirilirken tanımlanan birleştirme kurallar ile yalnızca bir eşitleme kuralı kapsamında olması gerekir. Bir nesne için birden çok eşitleme kuralları birleştirme kurallarla bulunursa, bir hata oluşturulur. Bu nedenle, yalnızca bir eşitleme kuralı kapsamında bir nesne için birden çok eşitleme kuralları olduğunda tanımlanan birleştirme sahip bir en iyi uygulamadır. Azure AD Connect eşitleme için out-of-box yapılandırması, bu kurallar adına bakarak bulunamadı ve word olanlar bulmak **katılma** adının sonunda. Başka bir eşitleme kuralı nesneleri birleştirilen veya yeni bir hedef nesne sağlanan tanımlanan herhangi bir birleştirme kuralı olmadan bir eşitleme kuralı öznitelik akışları uygulanır.

Resim yukarıdaki bakarsanız, kural katılmaya çalışıyor görebilirsiniz **objectSID** ile **msExchMasterAccountSid** (Exchange) ve **Msrtcsıp-OriginatorSid** (Lync) olan bir hesap-kaynak ormanı topolojisinde ne bekliyoruz. Tüm ormanlarda aynı kural bulun. Her ormanda bir hesabı veya kaynak ormanı olabilir varsayılır. Tek bir ormanda canlı ve katılması gerekmez hesapları varsa, bu yapılandırma ayrıca çalışır.

#### <a name="transformations"></a>Dönüşümleri
Dönüştürme bölümü nesneleri birleştirilir ve kapsam filtresi sağlanıyorsa hedef nesnesi için geçerli tüm öznitelik akışları tanımlar. Geri gidip **içinde AD'den – kullanıcı AccountEnabled** eşitleme kuralı şu dönüşümleri bulun:

![Eşitleme kuralı Düzenleyicisi dönüşümleri sekmesi ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruletransformations.png)

Bağlamında bir hesap-kaynak ormanı dağıtımı, bu yapılandırma koymak için bunu etkinleştirilmiş bir hesabı hesap ormanı ve devre dışı bırakılmış bir hesap kaynak ormanda Exchange ve Lync ayarlarla bulmak için bekleniyor. Oturum açma için gerekli öznitelikler adresindeki arayan eşitleme kuralı içerir ve bu öznitelikler ormandan akış etkinleştirilmiş bir hesabı olduğu. Bu öznitelik akışları birlikte bir eşitleme kuralı yerleştirilir.

Bir dönüşüm farklı olabilir: sabiti, doğrudan ve ifade.

* Sürekli akışı, her zaman bir sabit kodlanmış değer akar. Yukarıdaki durumda, her zaman değeri ayarlar **True** meta veri deposu özniteliği adlı **accountEnabled**.
* Doğrudan bir akışı her zaman kaynak hedef özniteliğe özniteliğinin değeri akar-değil.
* Üçüncü akış türü ifadesidir ve daha gelişmiş yapılandırmalarını sağlar.

İfade VBA (Visual Basic for Applications), bu nedenle Microsoft Office deneyimiyle kişiler veya VBScript biçimi algılar dilidir. Öznitelikler [attributeName] köşeli parantez içine alınır. Öznitelik adları ve işlev adları büyük küçük harfe duyarlıdır, ancak eşitleme kuralları Düzenleyicisi ifadeleri değerlendirir ve ifade geçerli değilse bir uyarı sağlayın. Tüm ifadeler, iç içe geçmiş işlevler ile tek bir satırda belirtilmiştir. Yapılandırma dil gücünü göstermek için pwdLastSet, ancak eklenen ek açıklamaları olan akış şöyledir:

```
// If-then-else
IIF(
// (The evaluation for IIF) Is the attribute pwdLastSet present in AD?
IsPresent([pwdLastSet]),
// (The True part of IIF) If it is, then from right to left, convert the AD time format to a .Net datetime, change it to the time format used by Azure AD, and finally convert it to a string.
CStr(FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")),
// (The False part of IIF) Nothing to contribute
NULL
)
```

Bkz: [anlama bildirim temelli hazırlama ifadelerini](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) öznitelik akışları için ifade dili hakkında daha fazla bilgi için.

### <a name="precedence"></a>Öncellik
Bazı bireysel eşitleme kurallarında şimdi attıktan ancak kuralları yapılandırmada birlikte çalışır. Bazı durumlarda, bir öznitelik değeri aynı target özniteliği birden çok eşitleme kurallarından katkıda bulunan. Bu durumda, öznitelik önceliği hangi öznitelik WINS belirlemek için kullanılır. Örnek olarak, sourceAnchor özniteliği arayın. Bu öznitelik için Azure AD oturum açabilmesi için önemli bir özniteliktir. İki farklı eşitleme kuralları, bu öznitelik için bir öznitelik akışı bulabilirsiniz **içinde AD'den – kullanıcı AccountEnabled** ve **içinde AD'den – kullanıcı ortak**. Bazı nesneler meta veri deposu nesnesine birleştirilmiş olduğunda eşitleme kuralı önceliği nedeniyle sourceAnchor özniteliği etkin bir hesapla ormandaki ilk katkıda. Hiçbir etkin hesapları sonra catch tüm eşitleme kuralı eşitleme altyapısı kullanan **içinde AD'den – kullanıcı ortak**. Bu yapılandırma, devre dışı bırakılmış bile hesapları için olduğunu hala bir sourceAnchor sağlar.

![Gelen eşitleme kuralları](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

Eşitleme kuralları için öncelik gruplarında Yükleme Sihirbazı tarafından ayarlanır. Bir gruptaki tüm kuralları aynı ada sahip, ancak farklı bağlı dizinlere bağlanır. Yükleme Sihirbazı'nı kural verir **içinde AD'den – kullanıcı katılma** en yüksek öncelik ve onu tekrarlanan üzerinden tüm bağlı AD dizinleri. Ardından kuralları önceden tanımlanmış bir sırada sonraki grupları ile devam eder. Bir grup içinde kurallar bağlayıcıları sihirbazında eklenmiş sırayla eklenir. Başka bir bağlayıcı Sihirbazı eklediyseniz, eşitleme kuralları kaldırılmasında ve yeni bağlayıcı kuralları son her gruba eklenir.

### <a name="putting-it-all-together"></a>Tüm bir araya getirme
Şimdi yeterli eşitleme yapılandırma farklı eşitleme kuralları nasıl çalıştığını anlamak için kuralları hakkında biliyoruz. Bir kullanıcı ve meta veri deposu için katkıda bulunan öznitelikler bakarsanız, kuralları aşağıdaki sırayla uygulanır:

| Ad | Açıklama |
|:--- |:--- |
| İçinde AD'den – kullanıcı birleştirme |Bağlayıcı alanı nesneler meta veri deposu ile birleştirme kuralı. |
| İçinde kullanıcı hesabı AD – etkin |Oturum açma Azure ad için gerekli öznitelikler ve Office 365. Bu öznitelikler etkin hesabından istiyoruz. |
| İçinde AD'den – kullanıcı ortak Exchange'den |Genel adres listesinde bulunan öznitelikler. Veri Kalitesi kullanıcının posta bulduk olduğu ormandaki en iyisidir varsayalım. |
| İçinde AD'den – kullanıcı ortak |Genel adres listesinde bulunan öznitelikler. Bir posta kutusu bulamadık durumda alanına katılmış herhangi bir nesnenin öznitelik değeri katkıda bulunabilir. |
| İçinde AD'den – kullanıcı Exchange |Yalnızca Exchange algıladıysa bulunmaktadır. Tüm altyapı Exchange özniteliklerini akar. |
| İçinde AD'den – kullanıcı Lync |Lync algıladıysa yalnızca bulunmaktadır. Tüm altyapı Lync öznitelikleri akar. |

## <a name="next-steps"></a>Sonraki adımlar
* Yapılandırma modeli hakkında daha fazla bilgiyi [anlama bildirim temelli hazırlama](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* İfade dili hakkında daha fazla bilgiyi [anlama bildirim temelli hazırlama ifadelerini](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).
* Out-of-box yapılandırma şeklini okunurken devam [kullanıcıları ve kişileri anlama](active-directory-aadconnectsync-understanding-users-and-contacts.md)
* Bkz. bildirim temelli sağlamayı kullanarak pratik değişiklik yapmak [varsayılan yapılandırması ile değişiklik yapmak nasıl](active-directory-aadconnectsync-change-the-configuration.md).

**Genel bakış konuları**

* [Azure AD Connect eşitleme: anlamak ve eşitleme özelleştirme](active-directory-aadconnectsync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)

