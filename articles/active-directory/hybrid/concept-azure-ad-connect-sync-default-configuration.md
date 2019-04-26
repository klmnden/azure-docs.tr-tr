---
title: 'Azure AD Connect eşitleme: Varsayılan yapılandırmayı anlama | Microsoft Docs'
description: Bu makalede, Azure AD Connect eşitleme varsayılan yapılandırmasında açıklanır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ed876f22-6892-4b9d-acbe-6a2d112f1cd1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: b42a6b667a8708aeb2edeb0c80a5ab747b6c60a9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60246117"
---
# <a name="azure-ad-connect-sync-understanding-the-default-configuration"></a>Azure AD Connect eşitleme: Varsayılan yapılandırmayı anlama
Bu makalede, out-of-box Yapılandırması kuralları açıklanır. Bu belgeleri, kuralları ve bu kurallar yapılandırmanın nasıl etkiler. Ayrıca, Azure AD Connect eşitleme yapılandırmasını varsayılan rehberlik sağlar. Okuyucu adlı bildirim temelli sağlama, yapılandırma modeli, gerçek hayatta kullanılan örnekte nasıl çalıştığını anladığını hedeftir. Bu makalede, zaten yüklediyseniz ve Azure AD Connect eşitlemeyi Yükleme Sihirbazı'nı kullanarak yapılandırma varsayılır.

Yapılandırma modeli ayrıntılarını anlamak için okuma [anlama bildirim temelli sağlama](concept-azure-ad-connect-sync-declarative-provisioning.md).

## <a name="out-of-box-rules-from-on-premises-to-azure-ad"></a>Azure ad şirket içi kullanıma hazır kuralları
Aşağıdaki ifadeler out-of-box yapılandırması içinde bulunabilir.

### <a name="user-out-of-box-rules"></a>Kullanıcı out-of-box kuralları
Bu kurallar, iNetOrgPerson nesne türü için de geçerlidir.

Bir kullanıcı nesnesi eşitlenmesi için aşağıdakileri karşılamanız gerekir:

* Bir sourceAnchor olması gerekir.
* Azure AD'de oluşturulduktan sonra sourceAnchor değişiklik yapamazsınız. Değer değiştirilen şirket içinde ise, nesne sourceAnchor önceki değerine geri değiştirilinceye kadar eşitleme durdurur.
* AccountEnabled (userAccountControl) öznitelik doldurulmuş gerekir. Şirket içi Active Directory ile bu öznitelik her zaman mevcut ve doldurulmuş bağlantıdır.

Aşağıdaki kullanıcı nesneler **değil** Azure AD'ye eşitlenen:

* `IsPresent([isCriticalSystemObject])`. Yerleşik yönetici hesabı gibi çok sayıda kullanıma hazır nesne Active Directory'de olun, eşitlenmez.
* `IsPresent([sAMAccountName]) = False`. Kullanıcı nesneleri yok sAMAccountName özniteliğinin ile eşitlenmemiş emin olun. Bu durumda yalnızca bulundurmanızı NT4 ' yükseltilmiş bir etki alanında gerçekleşir.
* `Left([sAMAccountName], 4) = "AAD_"`, `Left([sAMAccountName], 5) = "MSOL_"`. Azure AD Connect eşitleme ve kendi önceki sürümleri tarafından kullanılan hizmet hesabı eşitlenmez.
* Exchange Online çalışmıyordu Exchange hesaplarına eşitlenmez.
  * `[sAMAccountName] = "SUPPORT_388945a0"`
  * `Left([mailNickname], 14) = "SystemMailbox{"`
  * `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`
  * `(Left([sAMAccountName], 4) = "CAS_" && (InStr([sAMAccountName], "}")> 0))`
* Exchange Online çalışmıyordu nesneleri eşitlenmez.
  `CBool(IIF(IsPresent([msExchRecipientTypeDetails]),BitAnd([msExchRecipientTypeDetails],&H21C07000) > 0,NULL))`  
  Bu bit maskesi (& H21C07000) aşağıdaki nesnelerini filtrelemek:
  * Posta özellikli ortak klasör (içinde sürüm 1.1.524.0'ten itibaren Önizleme)
  * Sistem Katılımcısı posta kutusu
  * Posta kutusu veritabanı posta kutusu (sistem posta)
  * Evrensel güvenlik grubu (bir kullanıcı için geçerli mıydı, ancak eski nedenlerle yok)
  * Olmayan Evrensel Grup (bir kullanıcı için geçerli mıydı, ancak eski nedenlerle yok)
  * Posta kutusu planı
  * Bulma posta kutusu
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Herhangi bir çoğaltma victim nesne eşitlenmez.

Aşağıdaki özniteliği kurallar geçerlidir:

* `sourceAnchor <- IIF([msExchRecipientTypeDetails]=2,NULL,..)`. SourceAnchor özniteliği, bağlı bir posta kutusundan katkıda değil. Bağlı bir posta kutusu bulunursa, gerçek hesap daha sonra katıldığı varsayılır.
* Exchange ilgili öznitelikleri yalnızca, eşitlenen öznitelik **mailNickName** bir değere sahip.
* Birden çok ormanınız olduğunda, öznitelikleri şu sırayla tüketiliyor diyelim:
  1. Ormandaki etkin bir hesapla oturum açma için ilgili öznitelikleri (örneğin, userPrincipalName) katkıda.
  2. Bir Exchange posta kutusu olan bir ormandaki bir Exchange GAL (genel adres listesi) bulunabilir öznitelikleri katkıda.
  3. Ardından, posta kutusu bulunabilir, bu öznitelikler herhangi bir orman gelebilir.
  4. Exchange ilgili öznitelikleri (GAL içinde görünür değil teknik öznitelikler) ormandan katkıda burada `mailNickname ISNOTNULL`.
  5. Bu kurallardan biri karşılayacaktır birden çok ormanı varsa, oluşturma sırası (tarih) (orman) bağlayıcıları hangi ormanın öznitelikleri katkıda belirlemek için kullanılır.

### <a name="contact-out-of-box-rules"></a>İletişim kutusuna giden kuralları
Bir kişi nesnesi eşitlenmesi için aşağıdakileri karşılamanız gerekir:

* İlgili kişi, posta özellikli olmalıdır. Aşağıdaki kurallar ile doğrulandı:
  * `IsPresent([proxyAddresses]) = True)`. ProxyAddresses özniteliği doldurulmalıdır.
  * Bir birincil e-posta adresi proxyAddresses özniteliği veya posta özniteliği bulunamadı. Varlığı bir \@ içeriğin bir e-posta adresi olduğunu doğrulamak için kullanılır. Bu iki kurallardan biri True olarak değerlendirilmesi gerekir.
    * `(Contains([proxyAddresses], "SMTP:") > 0) && (InStr(Item([proxyAddresses], Contains([proxyAddresses], "SMTP:")), "@") > 0))`. Sahip bir girdi yok "SMTP:" ve varsa, bir \@ dizesinde bulunabilir?
    * `(IsPresent([mail]) = True && (InStr([mail], "@") > 0)`. Posta özniteliği doldurulur ve değilse, bir \@ dizesinde bulunabilir?

Aşağıdaki kişi nesneler **değil** Azure AD'ye eşitlenen:

* `IsPresent([isCriticalSystemObject])`. Kritik olarak işaretlenmelidir herhangi bir kişi nesnenin eşitlendiğinden emin olun. Herhangi bir varsayılan yapılandırmaya sahip olmamalıdır.
* `((InStr([displayName], "(MSOL)") > 0) && (CBool([msExchHideFromAddressLists])))`.
* `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`. Bu nesneler, Exchange Online çalışmaz.
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Herhangi bir çoğaltma victim nesne eşitlenmez.

### <a name="group-out-of-box-rules"></a>Grup kutusu giden kuralları
Bir grup nesnesi eşitlenmesi için aşağıdakileri karşılamanız gerekir:

* 50. 000'den az üyelerinin olması gerekir. Bu sayı şirket içi Grup üyeleri sayısıdır.
  * Eşitleme ilk kez başlatılmadan önce daha fazla üye varsa, Grup eşitlenmez.
  * Üye sayısı ne zaman başlangıçta oluşturulduğu, ardından 50.000 üyeleri ulaştığında geçerseniz yeniden üyeliği sayısı 50. 000 ' daha düşük olana kadar eşitleme durdurur.
  * Not: 50.000 üyeliği sayısı ayrıca Azure AD tarafından zorlanır. Değiştirmek veya bu kuralı kaldırmak bile grupları ile daha fazla üye eşitlenebilir değildir.
* Grubu olup olmadığının bir **dağıtım grubu**, posta etkinleştirilmiş olmalıdır. Bkz: [iletişim kutusuna giden kuralları](#contact-out-of-box-rules) için bu kural zorlanır.

Aşağıdaki Grup nesneler **değil** Azure AD'ye eşitlenen:

* `IsPresent([isCriticalSystemObject])`. Yerleşik Yöneticiler grubunun gibi çok sayıda kullanıma hazır nesne Active Directory'de olun, eşitlenmez.
* `[sAMAccountName] = "MSOL_AD_Sync_RichCoexistence"`. DirSync tarafından kullanılan eski grubu.
* `BitAnd([msExchRecipientTypeDetails],&amp;H40000000)`. Rol grubu.
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Herhangi bir çoğaltma victim nesne eşitlenmez.

### <a name="foreignsecurityprincipal-out-of-box-rules"></a>ForeignSecurityPrincipal out-of-box kuralları
FSP "tümü" birleştirilir (\*) meta veri nesnesi. Gerçekte, bu katılımı yalnızca kullanıcılar ve güvenlik grupları için gerçekleşir. Bu yapılandırma, ormanlar arası üyeliklerini çözümlendi ve Azure AD'de doğru şekilde temsil sağlar.

### <a name="computer-out-of-box-rules"></a>Bilgisayar out-of-box kuralları
Bir bilgisayar nesnesi eşitlenmesi için aşağıdakileri karşılamanız gerekir:

* `userCertificate ISNOTNULL`. Bu öznitelik yalnızca Windows 10 bilgisayarlarını doldurun. Tüm bilgisayar nesneleri bu öznitelikte bir değer ile eşitlenir.

## <a name="understanding-the-out-of-box-rules-scenario"></a>Kullanıma hazır kuralları senaryo anlama
Bu örnekte, bir dağıtım ile bir hesap ormanı (A), bir kaynak ormanı (R) ve bir Azure AD dizinindeki kullanıyoruz.

![Senaryo açıklaması ile resim](./media/concept-azure-ad-connect-sync-default-configuration/scenario.png)

Bu yapılandırmada, hesap ormanındaki etkin bir hesap ve kaynak ormanında bağlı bir posta kutusu ile bir devre dışı bırakılmış hesapla varsayılır.

Varsayılan yapılandırma ile Amacımız şöyledir:

* Hesabın etkinleştirilmiş olan bir ormandaki eşitlenen öznitelikler ilgili oturum açma.
* GAL (genel adres listesi) içinde bulunan öznitelikler, posta kutusu olan bir ormandaki eşitlenir. Posta kutusu bulunamıyorsa, başka bir ormana kullanılır.
* Bağlı bir posta kutusu bulunursa, hesabın bağlantılı etkinleştirilmiş Azure AD'ye aktarılacak nesnesinin bulunması gerekir.

### <a name="synchronization-rule-editor"></a>Eşitleme kuralı Düzenleyicisi
Yapılandırma görüntülenebilir ve eşitleme kuralları Düzenleyicisi (SRE) aracı ile değiştirildi ve bir kısayol, Başlat menüsünde bulunabilir.

![Eşitleme kuralları Düzenleyicisi simgesi](./media/concept-azure-ad-connect-sync-default-configuration/sre.png)

SRE Kaynak Seti aracıdır ve Azure AD Connect eşitlemesi ile yüklenir. Başlatabilmek için için ADSyncAdmins grubunun bir üyesi olmanız gerekir. Başladığında, aşağıdakine benzer bakın:

![Gelen eşitleme kuralları](./media/concept-azure-ad-connect-sync-default-configuration/syncrulesinbound.png)

Bu bölümde, yapılandırmanız için oluşturulan tüm eşitleme kuralları bakın. Tablodaki her satır bir eşitleme kuralıdır. Kural Türü altında sola, iki farklı türleri listelenmiştir: Gelen ve giden. Gelen ve giden meta veri görünümünden olur. Bu genel bakışta gelen kuralları odağı çoğunlukla seçeceksiniz. Eşitleme kuralları gerçek listesini, algılanan şemaya AD'de bağlıdır. Yukarıdaki resimde, hesap ormanı (fabrikamonline.com) Exchange ve Lync gibi tüm hizmetlere sahip olmadığı ve bu hizmetler için oluşturulmuş eşitleme kuralı yok. Ancak, kaynak ormanı (res.fabrikamonline.com), eşitleme kuralları için bu hizmetlerin bulabilirsiniz. Kuralları içeriğini algılanan sürümüne göre farklılık gösterir. Örneğin, Exchange 2013 ile bir dağıtımda var. daha Exchange 2010/2007'de yapılandırılmış. daha fazla öznitelik akışları

### <a name="synchronization-rule"></a>Eşitleme Kuralı
Bir yapılandırma nesnesi bir koşul karşılandığında akan öznitelikleri kümesi ile bir eşitleme kuralıdır. Bir bağlayıcı alanında bir nesne olarak bilinen meta veri içinde bir nesneye nasıl ilişkili olduğunu açıklamak için de kullanılır **birleştirme** veya **eşleşen**. Eşitleme kuralları birbirleriyle nasıl ilişkili olduğunu belirten bir öncelik değerine sahip. Daha düşük bir sayısal değere sahip bir eşitleme kuralı daha yüksek bir önceliği ve bir öznitelik akışı çakışması daha yüksek önceliği çakışma WINS.

Eşitleme kuralı örnek olarak, konum **içinde ad – kullanıcı AccountEnabled**. Bu satırı SRE seçip işaretlemek **Düzenle**.

Bu kural, bir out-of-box kural olduğundan, kuralın açtığınızda, bir uyarı alırsınız. Tüm yapmamalısınız [değişiklikleri kullanıma hazır kuralları](how-to-connect-sync-best-practices-changing-default-configuration.md), amacınızı nelerdir istenir. Bu durumda, yalnızca kural görüntülemek istiyorsunuz. Seçin **Hayır**.

![Eşitleme kuralları Uyarısı](./media/concept-azure-ad-connect-sync-default-configuration/warningeditrule.png)

Eşitleme kuralı dört yapılandırma bölümü vardır: Açıklama, Scoping filtre, birleştirme kuralları ve dönüşümler.

#### <a name="description"></a>Açıklama
İlk bölüm, bir ad ve açıklama gibi temel bilgiler sağlar.

![Açıklama sekmesindeki eşitleme kuralı Düzenleyicisi](./media/concept-azure-ad-connect-sync-default-configuration/syncruledescription.png)

Ayrıca bağlı hangi sistem bu kural, ilgili bilgiler, geçerli bağlı sistem türü ve meta veri deposu nesne türü nesnesi bulabilirsiniz. Kaynak nesne türü bir kullanıcı, iNetOrgPerson veya kişi olduğunda meta veri deposu nesne türü her zaman bakılmaksızın kişidir. Genel bir tür oluşmasını meta veri deposu nesne türü hiçbir zaman değiştirmeniz gerekir. Bağlantı türü, birleştirme, StickyJoin veya sağlama için ayarlanabilir. Bu ayar, birleştirme kuralları bölümünde ile birlikte çalışır ve daha sonra ele alınmıştır.

Ayrıca, bu eşitleme kuralı için parola eşitleme kullanıldığını görebilirsiniz. Bir kullanıcı bu eşitleme kuralı için kapsamdaki ise, parola (parola eşitleme özelliği etkin varsayılarak) buluta şirket içinden eşitlenir.

#### <a name="scoping-filter"></a>Kapsam belirleme filtresi
Kapsam belirleme filtresi bölümü, bir eşitleme kuralının uygulanacağı yapılandırmak için kullanılır. Kapsam, yalnızca uygulanması için etkin kullanıcıları Baktığınız eşitleme kuralının adını belirtir. bu yana yapılandırılır böylece AD özniteliği **userAccountControl** değil 2 bit ayarlamış olmanız gerekir. Eşitleme altyapısı, bir kullanıcı, AD'de bulduğunda, bu eşitleme uygular. kural **userAccountControl** ondalık değeri 512 (etkin normal kullanıcı) ayarlanır. Kullanıcı sahip olduğunda kuralı uygulanmaz **userAccountControl** 514 (devre dışı bırakılmış normal kullanıcı) olarak ayarlayın.

![Kapsam sekmesinde eşitleme kuralı Düzenleyicisi](./media/concept-azure-ad-connect-sync-default-configuration/syncrulescopingfilter.png)

Kapsam belirleme filtresi, grupları ve iç içe yan tümceleri vardır. Bir eşitleme kuralı uygulamak bir grup içindeki tüm koşullar karşılanmalıdır. Birden çok grup tanımlandığında, en az bir grup kuralı uygulamak karşılanması gereken. Diğer bir deyişle, mantıksal OR grupları arasında mantıksal değerlendirilir ve içinde bir grup değerlendirilir. Bu yapılandırmanın bir örneği giden eşitleme kuralı bulunabilir **Out AAD için – gruba katılma**. Birden çok eşitleme filtre grubu vardır, örneğin bir güvenlik grupları (`securityEnabled EQUAL True`) dağıtım grupları için ve biri (`securityEnabled EQUAL False`).

![Kapsam sekmesinde eşitleme kuralı Düzenleyicisi](./media/concept-azure-ad-connect-sync-default-configuration/syncrulescopingfilterout.png)

Bu kural grupları Azure AD ile sağlanacak tanımlamak için kullanılır. Dağıtım grupları Azure AD ile eşitlenmesi için etkin bir posta olmalıdır, ancak güvenlik grupları için bir e-posta gerekli değildir.

#### <a name="join-rules"></a>Kural birleştirme
Üçüncü bölüm nesneleri bağlayıcı alanında meta veri nesnelerine nasıl ilişki kuracağını yapılandırmak için kullanılır. Kural, baktığı, daha önce herhangi bir yapılandırma kuralları, katılmak için bunun yerine bakmak için seçeceğiz yok **içinde ad – kullanıcı katılın**.

![Kurallar sekmesi eşitleme kuralı Düzenleyicisi'nde katılın](./media/concept-azure-ad-connect-sync-default-configuration/syncrulejoinrules.png)

Birleştirme kuralı içeriğini Yükleme Sihirbazı'nda eşleşen seçeneğe göre değişir. Bir gelen kuralı için bir nesne kaynak bağlayıcı alanında değerlendirme başlar ve her gruba katılma kuralları sırayla değerlendirilir. Meta veri birleştirme kuralları kullanarak tam olarak bir nesne ile eşleştirilecek kaynak nesnesi değerlendirildiğinde nesneleri birleştirilir. Tüm kuralları değerlendirilir ve eşleşme açıklama sayfasında bağlantı türü kullanılır. Bu yapılandırma ayarlanırsa **sağlama**, hedef, meta veri yeni bir nesne oluşturulur. Yeni bir nesne için meta veri deposu olarak da bilinen olan sağlama **proje** meta veri deposu için bir nesne.

Birleştirme kuralları yalnızca bir kez değerlendirilir. Bağlayıcı alanı nesnesi ve bir meta veri deposu nesnesi katıldığında, eşitleme kuralı kapsamına hala memnun olduğu sürece birleştirilmiş kalırlar.

Eşitleme kuralları değerlendirilirken tanımlanan birleştirme kuralları ile yalnızca bir eşitleme kuralı kapsamında olması gerekir. Birleştirme kuralları ile birden çok eşitleme kuralı için bir nesne bulunursa, bir hata oluşturulur. Bu nedenle, yalnızca bir eşitleme kuralı kapsamında bir nesne için birden çok eşitleme kuralı olduğunda tanımlanan birleştirme sahip en iyi uygulamadır. Azure AD Connect eşitlemesi için out-of-box yapılandırması, bu kurallar adına bakarak bulunması ve unvanında Bul **katılın** adının sonunda. Başka bir eşitleme kuralı nesneleri birleştirilen ya da yeni bir hedef nesne sağlanan öznitelik akışları tanımlanan herhangi bir birleştirme kuralları olmadan bir eşitleme kuralı uygulanır.

Resim yukarıdaki bakarsanız, kural katılmaya çalışıyor görebilirsiniz **objectSID** ile **msExchMasterAccountSID** (Exchange) ve **Msrtcsıp-OriginatorSid** (Lync) bir hesap-kaynak orman topolojisini beklediğimiz olduğu. Tüm ormanlar aynı kural bulabilirsiniz. Her ormanda bir hesap veya kaynak orman olabileceğini varsayılır. Bu yapılandırma, ayrıca tek bir ormana canlı ve katılması gerekmez hesabınız varsa çalışır.

#### <a name="transformations"></a>Dönüşümler
Dönüştürme bölüm nesneleri birleştirilir ve kapsam filtresi sağlanırsa, hedef nesneye uygulanan tüm öznitelik akışları tanımlar. Geri giderek **içinde ad – kullanıcı AccountEnabled** eşitleme kuralı, aşağıdaki dönüştürmeleri bulun:

![Eşitleme kuralı Düzenleyicisi dönüşümleri sekmesi](./media/concept-azure-ad-connect-sync-default-configuration/syncruletransformations.png)

Bağlamda bir hesap-kaynak orman dağıtımı bu configuration koymak, etkinleştirilmiş bir hesabı hesap ormanı ve devre dışı bırakılan hesabın kaynak ormanda Exchange ve Lync ayarlarla bulmak için bekleniyor. Oturum açma için gerekli öznitelikler Baktığınız eşitleme kuralı içerir ve bu öznitelikler ormandan akışını etkinleştirilmiş bir hesabı olduğu. Bu öznitelik akışları birlikte bir eşitleme kuralı yerleştirilir.

Bir dönüştürme farklı türlere sahip olabilen: Sabit, doğrudan ve ifade.

* Akışı, bir kodlanmış değeri her zaman akar. Yukarıdaki durumda, her zaman değeri ayarlar **True** meta veri deposu özniteliği adlı **accountEnabled**.
* Doğrudan bir akış hedef özniteliğe kaynak özniteliğinin değeri her zaman akışları-olduğu.
* İfade üçüncü akış türüdür ve daha gelişmiş yapılandırmalar için sağlar.

İfade VBA (Visual Basic uygulamalar için), bu nedenle Microsoft Office deneyimiyle kişiler veya VBScript biçimi tanıyacağınız dilidir. Öznitelikler [attributeName], köşeli ayraç içine alınır. Öznitelik adları ve işlev adlarını büyük küçük harfe duyarlıdır, ancak eşitleme kuralları Düzenleyicisi ifadeleri değerlendirir ve ifade geçerli değilse bir uyarı sağlayın. Tüm ifadeler, iç içe geçmiş işlevler ile tek bir satırda gösterilir. Yapılandırma dilinin gücünü göstermek için akış pwdLastSet, ancak eklenen ek açıklamaları olan şöyledir:

```
// If-then-else
IIF(
// (The evaluation for IIF) Is the attribute pwdLastSet present in AD?
IsPresent([pwdLastSet]),
// (The True part of IIF) If it is, then from right to left, convert the AD time format to a .NET datetime, change it to the time format used by Azure AD, and finally convert it to a string.
CStr(FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")),
// (The False part of IIF) Nothing to contribute
NULL
)
```

Bkz: [bildirim temelli sağlama ifadelerini anlama](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md) öznitelik akışları için ifade dili hakkında daha fazla bilgi için.

### <a name="precedence"></a>Öncellik
Şimdi bazı bireysel eşitleme kurallarına attıktan, ancak kuralları yapılandırmada birlikte çalışır. Bazı durumlarda, bir öznitelik değeri aynı hedef öznitelik için birden çok eşitleme kuralları katkıda. Bu durumda, öznitelik önceliği, hangi öznitelik WINS belirlemek için kullanılır. Örneğin, sourceAnchor özniteliği bakın. Bu öznitelik, Azure AD'de oturum açabilmesi için önemli bir özniteliktir. Bu öznitelik iki farklı eşitleme kuralı için bir öznitelik akışı bulabilirsiniz **içinde ad – kullanıcı AccountEnabled** ve **içinde ad – kullanıcı ortak**. Birçok nesne için meta veri deposu nesne katılmış olduğunda eşitleme kuralının önceliğini nedeniyle sourceAnchor özniteliği etkin bir hesapla ormandaki ilk katkıda. Etkin hesapları yok, bu durumda eşitleme altyapısı catch tüm eşitleme kuralı **içinde ad – kullanıcı ortak**. Bu yapılandırma, devre dışı bırakılmış bile hesapları için olduğunu hala bir sourceAnchor sağlar.

![Gelen eşitleme kuralları](./media/concept-azure-ad-connect-sync-default-configuration/syncrulesinbound.png)

Eşitleme kuralları için öncelik gruplarında Yükleme Sihirbazı tarafından ayarlanır. Bir gruptaki tüm kuralları aynı adı taşıyan ancak farklı bağlı dizinlere bağlı. Yükleme Sihirbazı'nı kural verir **içinde ad – kullanıcı katılın** en yüksek öncelik ve yineleme üzerinden tüm bağlı AD dizinleri. Önceden tanımlanmış bir sırada kurallarının sonraki gruplarıyla sonra sürdürür. Bir grubun içine, bağlayıcı Sihirbazı'nda eklenen sırada kuralları eklenir. Başka bir bağlayıcı Sihirbazı eklenirse, eşitleme kuralları yeniden sıralanır ve yeni bağlayıcının kuralları son her gruba eklenir.

### <a name="putting-it-all-together"></a>Hepsini bir araya getirme
Artık yapılandırma farklı eşitleme kuralları ile nasıl çalıştığını anlamak için eşitleme kuralları hakkında yeterli biliyoruz. Bir kullanıcı ve meta veri deposu için katkıda öznitelikleri bakarsanız, kuralları aşağıdaki sırayla uygulanır:

| Ad | Açıklama |
|:--- |:--- |
| İçinde ad – kullanıcı birleştirme |Bağlayıcı alanı nesne meta veri deposu ile katılmak için kural'ı tıklatın. |
| İçinde UserAccount AD'den – etkin |Oturum açma için Azure AD için gerekli öznitelikler ve Office 365. Bu öznitelikler etkin hesabından istiyoruz. |
| İçinde ad – değişiminden kullanıcı genel |Öznitelik, genel adres listesinde bulunamadı. Veri Kalitesi burada kullanıcının posta kutusuna bulduk ormandaki en iyisidir varsayıyoruz. |
| İçinde ad – kullanıcı ortak |Öznitelik, genel adres listesinde bulunamadı. Bir posta kutusu bulamadık durumda birleştirilen herhangi bir nesne öznitelik değeri katkıda bulunabilir. |
| İçinde ad – kullanıcı Exchange |Yalnızca Exchange algılanırsa bulunmaktadır. Bu, tüm altyapı Exchange öznitelikleri akar. |
| İçinde ad – kullanıcı Lync |Lync algılanırsa yalnızca var. Bu, tüm altyapı Lync öznitelikleri akar. |

## <a name="next-steps"></a>Sonraki adımlar
* Yapılandırma modeli hakkında daha fazla bilgiyi [anlama bildirim temelli sağlama](concept-azure-ad-connect-sync-declarative-provisioning.md).
* İfade dili, hakkında daha fazla bilgiyi [bildirim temelli sağlama ifadelerini anlama](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md).
* Out-of-box yapılandırması işleyişi okumaya devam edin [kullanıcıları ve kişileri anlama](concept-azure-ad-connect-sync-user-and-contacts.md)
* İçinde bildirim temelli sağlama kullanarak pratik değişiklik yapılacağını görmek [Varsayılan yapılandırmada bir değişiklik yapmak nasıl](how-to-connect-sync-change-the-configuration.md).

**Genel bakış konuları**

* [Azure AD Connect eşitlemesi: Anlama ve eşitleme özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)

