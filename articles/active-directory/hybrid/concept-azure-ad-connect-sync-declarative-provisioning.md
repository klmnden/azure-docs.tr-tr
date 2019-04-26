---
title: 'Azure AD Connect: Bildirim temelli sağlama anlama | Microsoft Docs'
description: Azure AD CONNECT'te bildirim temelli sağlama yapılandırma modeli açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: cfbb870d-be7d-47b3-ba01-9e78121f0067
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 543c1a6706f794b81c4f93fc6fff3a61ed3fb9e3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60246273"
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning"></a>Azure AD Connect eşitleme: Bildirim Temelli Sağlamayı Anlama
Bu konuda, Azure AD CONNECT'te yapılandırma modeli açıklanmaktadır. Bildirim temelli sağlama modeli adı verilir ve bir yapılandırma değişikliği kolayca yapmanızı sağlar. Bu konuda açıklanan pek çok gelişmiş ve çoğu müşteri senaryoları için gerekli değildir.

## <a name="overview"></a>Genel Bakış
Bildirim temelli sağlama nesneleri bir kaynak bağlı dizinden gelen işlem ve nasıl öznitelikleri ve nesne bir kaynaktan bir hedefe dönüştürülmesi gereken belirler. Bir nesneyi eşitleme hattında işlenir ve işlem hattı gelen ve giden kurallar için aynıdır. Bir gelen kuralı bir bağlayıcı alanından için meta veri deposu ve bir giden kuralı meta veri deposu için bir bağlayıcı alanı.

![Eşitleme işlem hattı](./media/concept-azure-ad-connect-sync-declarative-provisioning/sync1.png)  

İşlem hattı, birkaç farklı modüller içerir. Her biri, bir kavram, nesne eşitleme sorumludur.

![Eşitleme işlem hattı](./media/concept-azure-ad-connect-sync-declarative-provisioning/pipeline.png)  

* Kaynak, kaynak nesnesi
* [Kapsam](#scope), kapsam içinde olan tüm eşitleme kuralları bulur
* [Birleştirme](#join), bağlayıcı alanı ve meta veri deposu arasındaki ilişki belirler
* Dönüştürme, öznitelikleri nasıl dönüştürdüğünü hesaplar ve flow
* [Öncelik](#precedence), öznitelik Katkıları çakışan çözümler
* Hedef, hedef nesne

## <a name="scope"></a>Kapsam
Kapsam modülü, bir nesne değerlendirmek ve kapsam içindedir ve işleme dahil edilmesi gereken kurallar belirler. Nesne öznitelikleri değerlerine bağlı olarak, farklı eşitleme kuralları, kapsam içinde yer değerlendirilir. Örneğin, devre dışı bir kullanıcı Exchange posta kutusu ile daha etkin bir kullanıcı bir posta kutusu ile farklı kurallara sahip.  
![Kapsam](./media/concept-azure-ad-connect-sync-declarative-provisioning/scope1.png)  

Kapsam grupları ve yan tümce tanımlanır. Yan tümceleri içinde bir grubudur. Mantıksal AND bir gruptaki tüm koşullar arasında kullanılır. Örneğin, (departman BT ve ülke = Danimarka =). Mantıksal OR grupları arasında kullanılır.

![Kapsam](./media/concept-azure-ad-connect-sync-declarative-provisioning/scope2.png)  
Bu resimdeki kapsamı olarak okunmalıdır (departman BT ve ülke = = Danimarka) veya (ülke İsveç =). Grup 1 veya 2 grup true, sonra kuralın değerlendirildiği taktirde kapsamdadır.

Kapsam Modülü aşağıdaki işlemleri destekler.

| İşlem | Açıklama |
| --- | --- |
| EŞİTTİR, EŞİT DEĞİLDİR |Öznitelik değerine eşit bir değer ise döndüren bir dize karşılaştırma. Birden çok değerli öznitelikler için ISIN ve ISNOTIN bakın. |
| LESSTHAN, LESSTHAN_OR_EQUAL |Değer döndüren bir dize karşılaştırma küçüktür öznitelik değeri. |
| İÇEREN NOTCONTAINS |Değer yere içindeki değeri öznitelikte bulunamazsa döndüren bir dize karşılaştırma. |
| STARTSWITH, NOTSTARTSWITH |Öznitelik değerinin başına değer ise döndüren bir dize karşılaştırma. |
| ENDSWITH, NOTENDSWITH |Öznitelik değerinin son değer ise döndüren bir dize karşılaştırma. |
| GREATERTHAN, GREATERTHAN_OR_EQUAL |Büyük öznitelik değerinin bir değerse değerlendiren bir dize karşılaştırma. |
| ISNULL, ISNOTNULL |Öznitelik yoksa değerlendirir nesnesi. Öznitelik mevcut ve bu nedenle null değilse, kuralı kapsamları dahilinde olması. |
| ISIN, ISNOTIN |İçinde tanımlı öznitelik değeri varsa değerlendirir. Bu işlem eşittir ve eşit değildir birden çok değerli çeşididir. Öznitelik birden çok değerli bir öznitelik olduğu varsayılır ve değeri herhangi bir öznitelik değeri bulunabilir, kural kapsamda olur. |
| ISBITSET, ISNOTBITSET |Belirli bir biti ayarlanmışsa değerlendirir. Örneğin, bir kullanıcı etkin veya devre dışı olduğunu görmek için userAccountControl bitler değerlendirmek için kullanılabilir. |
| ISMEMBEROF, ISNOTMEMBEROF |Bağlayıcı alanında bir gruba bir DN değeri içermelidir. Belirtilen grubun bir üyesiyse, kural kapsamdadır. |

## <a name="join"></a>Birleştir
Eşitleme işlem hattının birleştirme modülü, hedef kaynak nesnesi ve bir nesne arasındaki ilişkiyi bulmak için sorumludur. Bir gelen kuralı üzerinde bu ilişkiyi bir bağlayıcı alanında meta veri deposunda bir nesneye bir ilişki bulma bir nesne olabilir.  
![Mv ile cs arasında katılın](./media/concept-azure-ad-connect-sync-declarative-provisioning/join1.png)  
Başka bir bağlayıcı tarafından oluşturulan zaten meta veri deposu, bir nesne ise, değeriyle ilişkilendirilip ilişkilendirilmeyeceğini görmek için hedeftir. Örneğin, bir hesap-kaynak ormanda kullanıcı hesap ormandan kullanıcıyla kaynak ormandaki alanına eklenmelidir.

Birleşimler, bağlayıcı alanı nesnelerini birlikte aynı meta veri deposu nesnesine katılmak için çoğunlukla gelen kuralları kullanılır.

Birleşimler, bir veya daha fazla grup olarak tanımlanır. Bir grup yan tümceleri vardır. Mantıksal AND bir gruptaki tüm koşullar arasında kullanılır. Mantıksal OR grupları arasında kullanılır. Grupları, yukarıdan aşağıya sırasında işlenir. Bir grup hedef nesne ile tam olarak bir eşleşme karşılaştı, başka bir birleştirme kuralları değerlendirmeye alınır. Sıfır veya bir nesne bulunandan daha fazla işleme kuralları sonraki grubuna devam eder. Bu nedenle, kuralları en açık ilk sırasına göre oluşturulur ve daha uçtaki benzer olmalıdır.  
![Tanımı katılın](./media/concept-azure-ad-connect-sync-declarative-provisioning/join2.png)  
Bu resmi birleşimlerde üstten alta işlenir. İlk eşitleme işlem hattı EmployeeID bir eşleşme olup olmadığını görür. Aksi durumda, ikinci kuralı hesap adı nesneler birbirine birleştirmek için kullanılabilir olmadığını görür. Bu bir eşleşme ya da değilse, üçüncü ve son bir daha benzer eşleştirme kullanıcı adını kullanarak kuralıdır.

Tüm birleştirme kuralları değerlendirilir ve tam olarak bir eşleşme varsa **bağlantı türü** üzerinde **açıklama** sayfa kullanılır. Bu seçenek ayarlanırsa **sağlama**, hedef yeni bir nesne oluşturulur.  
![Sağlama veya birleştirme](./media/concept-azure-ad-connect-sync-declarative-provisioning/join3.png)  

Bir nesne, yalnızca birleştirme kuralları ile bir tek eşitleme kuralı kapsamında olmalıdır. Varsa birden çok eşitleme kuralı birleştirme tanımlandığı bir hata oluşur. Öncelik, birleştirme çakışmaları çözümlemek için kullanılmaz. Bir nesne ile aynı gelen/giden yönde akmasına izin öznitelikleri için kapsamda bir birleşim kuralına sahip olmalıdır. Hem gelen hem de aynı nesneye giden akış öznitelikleri gerekiyorsa, hem bir gelen hem de bir birleşim ile giden eşitleme kuralı olması gerekir.

Bir nesne için bir hedef bağlayıcı alanı sağlamak çalıştığında, giden birleşim özel davranışa sahiptir. DN özniteliği, önce bir ters birleştirme denemek için kullanılır. Nesneleri varsa zaten bir nesneyi hedef bağlayıcı alanında DN aynı ile birleştirilir.

Birleştirme modülü, yalnızca zaman yeni bir eşitleme kuralı kapsamına olduğunda değerlendirilir. Bir nesne katıldı, katılım ölçütleri artık memnun olsanız bile, ayrılma değil. Bir nesne çıkma istiyorsanız, nesneler birleştirilen eşitleme kuralı kapsam dışına gitmeniz gerekir.

### <a name="metaverse-delete"></a>Meta veri deposu Sil
Bir meta veri deposu nesnesi kapsamlı bir eşitleme kuralı olduğu kadar uzun kalır **bağlantı türü** kümesine **sağlama** veya **StickyJoin**. Yeni bir nesne için meta veri sağlamak için bir bağlayıcı alamadığında ancak bunu katıldı bir StickyJoin kullanılır, meta veri deposu nesne silinmeden önce kaynak silinmesi gerekir.

Bir meta veri deposu nesnesi silindiğinde, bir giden eşitleme kuralı ile ilişkilendirilmiş tüm nesneleri için işaretlenmiş **sağlama** bir silme için işaretlenmiş.

## <a name="transformations"></a>Dönüşümler
Dönüştürmeleri nasıl öznitelikleri kaynaktan hedefe akışını tanımlamak için kullanılır. Akışınız aşağıdakilerden biri olabilir **akış türleri**: Doğrudan, sabit veya ifade. Akışları bir öznitelik değeri olarak doğrudan bir akış-ek hiçbir dönüştürme ile olan. Sabit değer belirtilen değere ayarlar. Bir ifade dönüşümü nasıl olması ifade etmek için bildirim temelli sağlama ifade dili kullanır. İfade dili ayrıntılarını bulunabilir [bildirim temelli sağlama ifade dili anlama](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md) konu.

![Sağlama veya birleştirme](./media/concept-azure-ad-connect-sync-declarative-provisioning/transformations1.png)  

**Kez Uygula** onay kutusu, nesne ilk oluşturulduğunda özniteliği yalnızca ayarlanmış olduğunu tanımlar. Örneğin, bu yapılandırma, yeni bir kullanıcı nesnesi için bir başlangıç parolası ayarlamak için kullanılabilir.

### <a name="merging-attribute-values"></a>Öznitelik değerleri birleştirme
Öznitelik akışları birden çok değerli öznitelikler birkaç farklı bağlayıcılardan birleştirilip birleştirilmeyeceğini belirleyen bir ayar yoktur. Varsayılan değer **güncelleştirme**, en yüksek önceliğe sahip eşitleme kuralı win gösterir.

![Birleştirme türleri](./media/concept-azure-ad-connect-sync-declarative-provisioning/mergetype.png)  

Ayrıca **birleştirme** ve **MergeCaseInsensitive**. Bu seçenekler, değerleri farklı kaynaklardan birleştirmeyi sağlar. Örneğin, birkaç farklı ormanlardaki üyesi veya proxyAddresses özniteliği birleştirmek için kullanılabilir. Bu seçeneği kullandığınızda, bir nesne için kapsamdaki tüm eşitleme kuralları aynı birleştirme türü kullanmanız gerekir. Tanımlanamaz **güncelleştirme** bir Bağlayıcıdan ve **birleştirme** başka bir. Denerseniz, bir hata alırsınız.

Arasındaki fark **birleştirme** ve **MergeCaseInsensitive** yinelenen öznitelik değerleri işlemek nasıl. Eşitleme altyapısı, yinelenen değerler alan hedef özniteliğe eklenmiyor emin olur. İle **MergeCaseInsensitive**, yinelenen değerleri yalnızca bir fark dışında bulunması yapmayacağınız durumunda. Örneğin, her ikisi de görmelisiniz değil "SMTP:bob@contoso.com"ve"smtp:bob@contoso.com" Hedef öznitelik. **Birleştirme** yalnızca tam değerlerini ve birden çok değer arama yalnızca bulunduğu bir fark çalışması mevcut olabilir.

Seçenek **değiştirin** aynı **güncelleştirme**, ancak bu kullanılmaz.

### <a name="control-the-attribute-flow-process"></a>Öznitelik akış işlemini denetleme
Birden çok gelen eşitleme kuralı aynı meta veri deposu özniteliği için'katkıda bulunmak için yapılandırıldığında, öncelik, KAZANANI belirlemek için kullanılır. En yüksek önceliğe (düşük sayısal değer) ile eşitleme kuralı değeri katkıda zordur. Aynı giden kuralları için gerçekleşir. Eşitleme en yüksek öncelik WINS kural ve bağlı dizinin değer katkıda.

Bazı durumlarda, bir değer katkıda yerine eşitleme kuralı diğer kuralları nasıl hareket etmesi gerektiğini belirlemeniz gerekir. Bu durumda kullanılan bazı özel değişmez değerleri vardır.

Gelen eşitleme kuralları, değişmez değer için **NULL** akışı katkıda bulunmak için hiçbir değer olduğunu belirtmek için kullanılabilir. Daha düşük önceliğe sahip başka bir kural, bir değer katkıda bulunabilir. Hiçbir kural katkıda bulunan bir değer, meta veri deposu özniteliği sonra kaldırılır. Bir giden kuralı için **NULL** tüm eşitleme kuralları işlendikten sonra değer bağlantılı dizinde kaldırılır sonra son değerdir.

Değişmez değer **AuthoritativeNull** benzer **NULL** ancak farkla hiçbir daha düşük bir öncelik kuralları bir değer katkıda bulunabilir.

Bir öznitelik akışı de kullanabilirsiniz **IgnoreThisFlow**. Katkıda bulunmak için bir şey yok gösteren anlamda NULL olarak benzer. Bunu zaten var olan bir değer hedef kaldırmaz farktır. Öznitelik akışı var. olmamıştı gibidir.

Örnek aşağıda verilmiştir:

İçinde *Out AD - kullanıcı Exchange karma* aşağıdaki akış bulunabilir:  
`IIF([cloudSOAExchMailbox] = True,[cloudMSExchSafeSendersHash],IgnoreThisFlow)`  
Şu ifade olarak okunmalıdır: Azure AD'den AD özniteliği Azure AD'de kullanıcı posta kutusu bulunuyorsa, ardından akış. Aksi durumda, Active Directory dön herhangi bir şey geçmez. Bu durumda, mevcut değerin AD'de tutacak.

### <a name="importedvalue"></a>ImportedValue
' % S'işlevi ImportedValue diğer işlevler farklı olduğu öznitelik adı köşeli ayraç yerine tırnak işaretleri içine alınması gerekir:  
`ImportedValue("proxyAddresses")`.

Eşitleme sırasında genellikle bir öznitelik ("üst") tower, dışarı aktarma sırasında bir hata alındı veya henüz dışarı taşınmadığından bile beklenen değer kullanır. Bir gelen eşitleme henüz bağlı bir dizin sonunda ulaşıldığında edilmemiş öznitelik, ulaştığını varsayar. Bazı durumlarda yalnızca bağlı dizin ("hologramı ve delta tower içe") tarafından onaylanan bir değer eşitlemek önemlidir.

Bu işlev örneği kutusu giden eşitleme kuralı içinde bulunabilir *içinde ad – kullanıcı ortak Exchange'e*. Değer başarıyla dışarı aktarıldı onaylanmıştır, karma Exchange'de Exchange tarafından çevrimiçi katma değer yalnızca eşitlenmesi gerektiğini:  
`proxyAddresses` <- `RemoveDuplicates(Trim(ImportedValue("proxyAddresses")))`

## <a name="precedence"></a>Öncellik
Birçok eşitleme kuralı aynı öznitelik değeri hedef katkıda çalıştığınızda, KAZANANI belirlemek için öncelik değeri kullanılır. Öznitelik, bir çakışma katkıda bulunmak için en düşük sayısal değer olan en yüksek önceliğe sahip kural geçiyor.

![Birleştirme türleri](./media/concept-azure-ad-connect-sync-declarative-provisioning/precedence1.png)  

Bu sıralama küçük bir alt nesne için daha kesin bir öznitelik akışları tanımlamak için kullanılabilir. Örneğin, out-,-kutusu-kuralları etkinleştirilmiş bir hesaptan öznitelikleri sağlayın (**kullanıcı AccountEnabled**) diğer hesaplardan önceliğe sahip olur.

Öncelik arasında bağlayıcıları tanımlanabilir. Bağlayıcılar ile daha iyi veri değerleri ilk katkıda bulunmak için izin verir.

### <a name="multiple-objects-from-the-same-connector-space"></a>Aynı bağlayıcı alanında birden fazla nesneleri
Aynı meta veri deposu nesnesine katılmış aynı bağlayıcı alanında birden fazla nesneniz varsa, öncelik ayarlanmalıdır. Birden çok nesnenin aynı eşitleme kuralı kapsamında olup, eşitleme altyapısı önceliği belirlemek mümkün değildir. Hangi kaynak nesnesi meta veri değerine katkıda bulunmalıdır, belirsiz. Kaynak öznitelikleri aynı değere sahip olsa bile bu yapılandırma belirsiz olarak bildirilir.  
![Birden fazla nesne için aynı mv nesne katıldı](./media/concept-azure-ad-connect-sync-declarative-provisioning/multiple1.png)  

Bu senaryo için kapsam içinde kaynak nesneleri farklı eşitleme kuralları böylece kapsam eşitleme kuralları değiştirmeniz gerekir. Farklı öncelik tanımlamanıza olanak sağlar.  
![Birden fazla nesne için aynı mv nesne katıldı](./media/concept-azure-ad-connect-sync-declarative-provisioning/multiple2.png)  

## <a name="next-steps"></a>Sonraki adımlar
* İfade dili, hakkında daha fazla bilgiyi [bildirim temelli sağlama ifadelerini anlama](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md).
* Bkz: nasıl bildirim temelli sağlama olan kullanılan kullanıma hazır olarak [varsayılan yapılandırmayı anlama](concept-azure-ad-connect-sync-default-configuration.md).
* İçinde bildirim temelli sağlama kullanarak pratik değişiklik yapılacağını görmek [Varsayılan yapılandırmada bir değişiklik yapmak nasıl](how-to-connect-sync-change-the-configuration.md).
* Kullanıcıları ve kişileri birlikte nasıl çalıştığını okumaya devam [kullanıcıları ve kişileri anlama](concept-azure-ad-connect-sync-user-and-contacts.md).

**Genel bakış konuları**

* [Azure AD Connect eşitlemesi: Anlama ve eşitleme özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)

**Başvuru konuları**

* [Azure AD Connect eşitlemesi: İşlevler başvurusu](reference-connect-sync-functions-reference.md)
