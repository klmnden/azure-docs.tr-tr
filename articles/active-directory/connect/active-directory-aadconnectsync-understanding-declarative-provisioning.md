---
title: 'Azure AD Connect: Bildirim temelli hazırlama anlama | Microsoft Docs'
description: Azure AD CONNECT'te bildirim temelli hazırlama yapılandırma modeli açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: cfbb870d-be7d-47b3-ba01-9e78121f0067
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: bb6a0c16322884afba3d306c491c3cd592fc8595
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34593200"
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning"></a>Azure AD Connect eşitleme: bildirim temelli hazırlama anlama
Bu konuda, Azure AD Connect yapılandırma modelinde açıklanmaktadır. Bildirim temelli hazırlama modeli adı verilir ve bir yapılandırma değişikliği kolayca yapmanızı sağlar. Bu konuda açıklanan pek çok gelişmiş ve çoğu müşteri senaryoları için gerekli değildir.

## <a name="overview"></a>Genel Bakış
Bildirim temelli hazırlama kaynak bağlı dizinden gelen nesneleri işleme ve nasıl nesne ve özniteliklerin bir kaynaktan hedefe dönüştürülmesi gereken belirler. Bir nesne eşitleme ardışık düzeninde işlenir ve ardışık düzen gelen ve giden kuralları için aynıdır. Bir gelen kuralı bağlayıcı alanından için meta veri ve bir giden kuralı meta veri deposu için bir bağlayıcı alanı.

![Eşitleme ardışık düzen](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/sync1.png)  

Ardışık Düzen birkaç farklı modülü vardır. Her biri bir kavram nesne eşitleme olarak sorumludur.

![Eşitleme ardışık düzen](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/pipeline.png)  

* Kaynak, kaynak nesnesi
* [Kapsam](#scope), kapsamındaki tüm eşitleme kuralları bulur
* [Katılma](#join), bağlayıcı alanı ve meta veri deposu arasındaki ilişki belirler
* [Dönüştürme](#transform), nasıl öznitelikleri dönüştürülmüş hesaplar ve akış
* [Öncelik](#precedence), öznitelik Katkıları çakışan çözümler
* Hedef, hedef nesne

## <a name="scope"></a>Kapsam
Kapsam modülü bir nesne değerlendirmek ve kapsamındaki işlenmesinde dahil edilmesi gereken kurallara belirler. Nesne öznitelikleri değerlerine bağlı olarak farklı eşitleme kuralları kapsamında olması için değerlendirilir. Örneğin, Exchange posta kutusu devre dışı bırakılmış bir kullanıcıyla bir posta etkin bir kullanıcıyla daha farklı kuralları vardır.  
![Kapsam](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope1.png)  

Kapsam, grupları ve yan tümceleri tanımlanır. Yan tümceleri içinde bir grubudur. Mantıksal AND bir gruptaki tüm yan tümceleri arasında kullanılır. Örneğin, (departman BT ve ülke = = Danimarka). Mantıksal OR grupları arasında kullanılır.

![Kapsam](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope2.png)  
Bu resmi kapsamda olarak okumalısınız (departman BT ve ülke = = Danimarka) veya (ülke İsveç =). Grup 1 veya 2 grubu true, sonra kural için değerlendirilirse, kapsamında ' dir.

Kapsam Modülü aşağıdaki işlemleri destekler.

| İşlem | Açıklama |
| --- | --- |
| EŞİTTİR, EŞİT DEĞİLDİR |Değeri öznitelikte değerine eşitse veren bir dize karşılaştırın. Birden çok değerli öznitelikler için ISIN ve ISNOTIN bakın. |
| LESSTHAN, LESSTHAN_OR_EQUAL |Değer veren bir dize karşılaştırma satıcısı öznitelik değerinin. |
| İÇEREN NOTCONTAINS |Değer bir yerde içindeki değeri öznitelikte bulunabiliyorsa veren bir dize karşılaştırma. |
| STARTSWITH, NOTSTARTSWITH |Değer başına öznitelik değeri, ise veren bir dize karşılaştırma. |
| ENDSWITH, NOTENDSWITH |Değer öznitelik değeri sonunda ise veren bir dize karşılaştırın. |
| GREATERTHAN, GREATERTHAN_OR_EQUAL |Değer öznitelik değerinin büyükse veren bir dize karşılaştırın. |
| ISNULL, ISNOTNULL |Öznitelik mevcut olup olmadığını değerlendirir nesnesinden. Öznitelik mevcut ve bu nedenle null değilse, kural kapsamda kuralıdır. |
| ISIN, ISNOTIN |Değeri tanımlı özniteliğinde mevcut olup olmadığını değerlendirir. Bu işlem eşittir ve NOTEQUAL birden çok değerli çeşididir. Öznitelik birden çok değerli bir öznitelik olduğu varsayılır ve öznitelik değerlerini hiçbirinde değeri bulunabilir, kural kapsamında değil. |
| ISBITSET, ISNOTBITSET |Belirli bir biti ayarlanmışsa değerlendirir. Örneğin, bir kullanıcı etkinleştirildiğini veya devre dışı bırakıldığını görmek için userAccountControl bitleri değerlendirmek için kullanılabilir. |
| ISMEMBEROF, ISNOTMEMBEROF |Bağlayıcı alanı bir gruba bir DN değeri içermelidir. Belirtilen grubunun bir üyesiyse, kapsam içinde kuralıdır. |

## <a name="join"></a>Birleştir
Eşitleme ardışık düzen birleştirme modülünde hedef kaynak nesne ile bir nesne arasındaki ilişkiyi bulmak için sorumludur. Bir gelen kuralı, bu ilişkiyi bir meta veri deposunda bir nesne için bir ilişki bulma bağlayıcı alanı nesne olacaktır.  
![Mv ile cs arasında katılma](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join1.png)  
Başka bir bağlayıcı tarafından oluşturulan zaten meta veri, bir nesne varsa, ilişkilendirileceği görmek için belirtilir. Örneğin, bir hesap-kaynak ormanı kullanıcı hesap ormandan kullanıcıyla kaynak ormandaki katılması.

Birleştirmeler çoğunlukla gelen kurallarında bağlayıcı alanı nesneleri birlikte aynı meta veri deposu nesnesine katılmak için kullanılır.

Birleşimler, bir veya daha fazla grupları olarak tanımlanır. Bir grup yan tümceleri sahip. Mantıksal AND bir gruptaki tüm yan tümceleri arasında kullanılır. Mantıksal OR grupları arasında kullanılır. Grupları, üstten alta sırada işlenir. Bir grubu hedef nesne ile tam olarak bir eşleşme buldu, diğer bir birleşim kuralları değerlendirilir. Sıfır veya bir nesne bulunandan daha fazla işleme kuralları sonraki grubuna devam eder. Bu nedenle, kuralları en açık ilk sırasına göre oluşturulan ve sonunda daha benzer olmalıdır.  
![Tanımı katılma](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join2.png)  
Bu resmi birleşimlerde üstten alta işlenir. İlk eşitleme ardışık düzen EmployeeID bir eşleşme olup olmadığını görür. Aksi durumda, hesap adı nesnelerin araya katılmak için kullanılan, ikinci kuralı görür. Bu bir eşleşme ya da değilse, üçüncü ve son daha benzer eşleştirme kullanıcı adını kullanarak kuralıdır.

Tüm birleşim kuralları değerlendirilir ve tam bir eşleşme varsa **bağlantı türü** üzerinde **açıklama** sayfa kullanılır. Bu seçenek belirlenirse, **sağlama**, hedef yeni bir nesne oluşturulduktan sonra.  
![Sağlama veya birleştirme](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join3.png)  

Bir nesne, yalnızca birleştirme kurallarla bir tek eşitleme kuralı kapsamında olmalıdır. Varsa birden çok eşitleme kuralları birleştirme tanımlandığı, bir hata oluşur. Öncelik, birleştirme çakışmaları çözümlemek için kullanılmaz. Bir nesne ile aynı gelen/giden yön akışı sağlanacak öznitelikleri kapsamına birleştirme kuralına sahip olmalıdır. Hem gelen hem de aynı nesneye giden akış öznitelikleri gerekiyorsa, bir gelen ve giden eşitleme kuralı birleştirme ile olması gerekir.

Bir hedef bağlayıcı alanı nesneye sağlayacak çalıştığında giden birleşim özel bir davranışı vardır. DN özniteliği ilk Ters Birleştirme denemek için kullanılır. Zaten bir nesne içinde olmadığında aynı DN'si hedef bağlayıcı alanı, nesneleri birleştirilir.

Ne zaman yeni bir eşitleme kuralı kapsamına olduğunda birleştirme modülü yalnızca değerlendirilir. Bir nesne katıldı, birleştirme ölçütlerini artık karşılamadı olsa bile, ayrılma değil. Bir nesne çıkarın istiyorsanız, nesneleri birleştirilmiş eşitleme kuralı kapsamının dışına gitmeniz gerekir.

### <a name="metaverse-delete"></a>Meta veri deposu Sil
Meta veri deposu nesne kapsamlı bir eşitleme kuralı olduğu kadar uzun kalır **bağlantı türü** kümesine **sağlama** veya **StickyJoin**. Bir StickyJoin meta veri deposu için yeni bir nesne sağlamak için bir bağlayıcı alamadığında, ancak alanına olduğunda kullanılır, meta veri deposu nesnesi silinmeden önce kaynak silinmesi gerekir.

Bir meta veri deposu nesnesi silindiğinde, bir giden eşitleme kuralı ile ilişkili tüm nesneler için işaretlenmiş **sağlama** silinmek üzere işaretlenmiş.

## <a name="transformations"></a>Dönüşümleri
Dönüşümler nasıl öznitelikleri kaynaktan hedef akış tanımlamak için kullanılır. Akışlar şunlardan biri olabilir **akış türleri**: doğrudan, sabit değer veya ifade. Doğrudan bir akış akar öznitelik değeri olarak-ile hiçbir ek dönüştürmeler değil. Sabit bir değeri belirtilen değere ayarlar. Dönüştürme nasıl olmalıdır express için bildirim temelli hazırlama ifade dili bir ifade kullanır. İfade dili ayrıntılarını bulunabilir [bildirim temelli hazırlama ifade dili anlama](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) konu.

![Sağlama veya birleştirme](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/transformations1.png)  

**Bir kez Uygula** onay kutusunu tanımlayan nesne başlangıçta oluşturulduğunda özniteliği yalnızca ayarlayın. Örneğin, bu yapılandırma, yeni bir kullanıcı nesnesi için bir başlangıç parolası ayarlamak için kullanılabilir.

### <a name="merging-attribute-values"></a>Öznitelik değerlerini birleştirme
Öznitelik akışları birden çok değerli öznitelikleri birkaç farklı bağlayıcılardan birleştirilmiş olmadığını belirleyen bir ayar yoktur. Varsayılan değer **güncelleştirme**, en yüksek önceliğe sahip eşitleme kuralı win gösterir.

![Türleri birleştirme](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/mergetype.png)  

Ayrıca **birleştirme** ve **MergeCaseInsensitive**. Bu seçenekler, farklı kaynaklardan değerleri birleştirmek izin verir. Örneğin, birkaç farklı ormanlardaki üyesi veya proxyAddresses özniteliği birleştirmek için kullanılabilir. Bu seçeneği kullandığınızda, bir nesne için kapsamdaki tüm eşitleme kuralları aynı birleştirme türü kullanmanız gerekir. Tanımlayamazsınız **güncelleştirme** bir bağlayıcısından alınan ve **birleştirme** başka bir. Çalışırsanız, bir hata alırsınız.

Arasındaki farkı **birleştirme** ve **MergeCaseInsensitive** yinelenen öznitelik değerleri işlemek şeklidir. Eşitleme altyapısı yinelenen değerler target özniteliği eklenmiyor emin olur. İle **MergeCaseInsensitive**, bulunması yapmayacağınız durumda yalnızca bir fark değerlerle yinelenen. Örneğin, her ikisi de görülmemelidir "SMTP:bob@contoso.com"ve"smtp:bob@contoso.com" Hedef öznitelik. **Birleştirme** değerleri tam ve birden çok değerin yalnızca arıyor yalnızca olması durumunda bir fark çalışması mevcut olabilir.

Seçenek **Değiştir** aynı **güncelleştirme**, ancak değil kullanılır.

### <a name="control-the-attribute-flow-process"></a>Öznitelik akışı işlem denetimi
Birden çok gelen eşitleme kuralı aynı meta veri deposu özniteliği katkıda bulunmak için yapılandırıldığında, öncelik kazanan belirlemek için kullanılır. En yüksek öncelik (en düşük sayısal değer) ile eşitleme kuralı değeri katkıda zordur. Giden kuralları için aynı olur. Eşitleme ile en yüksek öncelik WINS kural ve bağlı dizin değerine katkıda.

Bazı durumlarda, bir değer katkıda yerine eşitleme kuralı diğer kuralların nasıl hareket etmesi gerektiğini belirlemeniz gerekir. Bu örnekte kullanılan bazı özel değişmez değerler vardır.

Gelen eşitleme kuralları, sabit için **NULL** akış katkıda bulunmak için herhangi bir değer olduğunu göstermek için kullanılır. Daha düşük önceliğe sahip başka bir kural, bir değer katkıda bulunabilir. Hiçbir kural katkıda bulunan bir değer, meta veri deposu özniteliği kaldırılır. Bir giden kuralı için **NULL** tüm eşitleme kuralları işlendikten sonra değer bağlı dizinde kaldırılır sonra son değerdir.

Sabit **AuthoritativeNull** benzer **NULL** ancak daha düşük bir öncelik kuralları bir değer katkıda bulunabilirsiniz arasındaki fark.

Bir öznitelik akışı da kullanabilirsiniz **IgnoreThisFlow**. Katkıda bulunmak için hiçbir şey gösteren anlamda NULL olarak benzer. Bunu zaten varolan bir değerle hedef kaldırmadığını farktır. Öznitelik akışı hiç var. olmamıştı gibidir.

Örnek aşağıda verilmiştir:

İçinde *Out AD - kullanıcı Exchange karma* aşağıdaki akış bulunabilir:  
`IIF([cloudSOAExchMailbox] = True,[cloudMSExchSafeSendersHash],IgnoreThisFlow)`  
Bu ifade olarak okumalısınız: kullanıcı posta kutusuna Azure AD içinde yer alıyorsa, Azure AD için AD öznitelik akışı. Aksi durumda, Active Directory dön herhangi bir şey geçmez. Bu durumda, bu mevcut değeri AD içinde tutmak.

### <a name="importedvalue"></a>ImportedValue
ImportedValue işlevi diğer işlevleri farklı olduğu öznitelik adı köşeli ayraç yerine tırnak içine alınması gerekir:  
`ImportedValue("proxyAddresses")`.

Genellikle eşitleme sırasında beklenen değer ("üst") kule, dışa aktarma sırasında bir hata alındı veya henüz dışarı kurmadı olsa bile bir özniteliğini kullanır. Gelen eşitleme henüz bağlı bir dizin sonunda üst sınırına kurmadı bir öznitelik bu ulaştığında varsayar. Bazı durumlarda, yalnızca bağlı dizin ("hologramı ve delta kule alma") tarafından onaylanan bir değer eşitlemek önemlidir.

Bu işlev örneği out-of-box eşitleme kuralında bulunabilir *içinde AD'den – kullanıcı ortak Exchange'den*. Değeri başarıyla dışarı aktarıldı onaylandıktan olduğunda karma Exchange'de çevrimiçi Exchange tarafından eklenen değeri yalnızca eşitlenmesi gereken:  
`proxyAddresses` <- `RemoveDuplicates(Trim(ImportedValue("proxyAddresses")))`

## <a name="precedence"></a>Öncellik
Birkaç eşitleme kuralı aynı öznitelik değeri hedefe katkıda çalıştığınızda öncelik değeri kazanan belirlemek için kullanılır. Bir çakışma özniteliğinde katkıda bulunmak için en yüksek öncelik, en düşük sayısal değer kuralla geçiyor.

![Türleri birleştirme](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/precedence1.png)  

Bu sıralama küçük bir alt nesne için daha kesin öznitelik akışları tanımlamak için kullanılabilir. Örneğin, çıkış-in-kutusu-kuralları etkinleştirilmiş bir hesaptan öznitelikleri emin olun (**kullanıcı AccountEnabled**) diğer hesaplarından önceliğe sahiptir.

Öncelik arasında bağlayıcılar tanımlanabilir. Bağlayıcılar değerleri ilk katkıda bulunmak için daha iyi veri sağlar.

### <a name="multiple-objects-from-the-same-connector-space"></a>Birden çok nesneden aynı bağlayıcı alanı
Bazı nesneler aynı meta veri deposu nesnesine birleştirilmiş aynı bağlayıcı alanı varsa, öncelik ayarlanması gerekir. Bazı nesneler aynı eşitleme kuralı kapsamında varsa, eşitleme altyapısı öncelik belirlemek mümkün değil. Hangi kaynak nesnesi meta veri değerine katkıda bulunmalıdır, belirsiz. Kaynak öznitelikte aynı değere sahip olsa bile bu yapılandırmayı belirsiz olarak raporlanır.  
![Birden fazla nesne aynı mv nesne katıldı](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple1.png)  

Bu senaryo için eşitleme kuralların kapsamını kaynak nesneleri farklı eşitleme kuralları kapsamında yer alacak şekilde değiştirmeniz gerekir. Bu, farklı öncelik tanımlamanıza olanak sağlar.  
![Birden fazla nesne aynı mv nesne katıldı](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple2.png)  

## <a name="next-steps"></a>Sonraki adımlar
* İfade dili hakkında daha fazla bilgiyi [anlama bildirim temelli hazırlama ifadelerini](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).
* Bkz: nasıl bildirim temelli hazırlama kullanılan out-of-box içinde olduğu [varsayılan yapılandırmayı anlama](active-directory-aadconnectsync-understanding-default-configuration.md).
* Bkz. bildirim temelli sağlamayı kullanarak pratik değişiklik yapmak [varsayılan yapılandırması ile değişiklik yapmak nasıl](active-directory-aadconnectsync-change-the-configuration.md).
* Kullanıcıları ve kişileri birlikte nasıl çalışır okumaya devam [kullanıcıları ve kişileri anlama](active-directory-aadconnectsync-understanding-users-and-contacts.md).

**Genel bakış konuları**

* [Azure AD Connect eşitleme: anlamak ve eşitleme özelleştirme](active-directory-aadconnectsync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)

**Başvuru konuları**

* [Azure AD Connect eşitleme: işlevleri başvurusu](active-directory-aadconnectsync-functions-reference.md)
