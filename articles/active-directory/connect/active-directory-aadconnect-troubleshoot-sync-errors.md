---
title: 'Azure AD Connect: Eşitleme sırasında sorun giderme | Microsoft Docs'
description: Azure AD Connect ile eşitleme sırasında karşılaşılan hataların nasıl giderileceği açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: curtand
ms.assetid: 2209d5ce-0a64-447b-be3a-6f06d47995f8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: billmath
ms.openlocfilehash: aaa374d5a11ef5b5860f83a87386ff981319189f
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="troubleshooting-errors-during-synchronization"></a>Eşitleme sırasında sorun giderme
Kimlik verilerini Windows Server Active Directory'den (AD DS) Azure Active Directory (Azure AD) eşitlenen hataları oluşabilir. Bu makalede, farklı türdeki eşitleme hatalar, bu hataları ve hataları düzeltmek için olası yollar neden olası senaryolardan bazıları genel bakış sağlar. Bu makalede genel hata türlerini içerir ve tüm olası hatalar kapak değil.

 Bu makalede okuyucu arka plandaki ile hakkında bilgi sahibi olduğunu varsayar [tasarım kavramları Azure AD ve Azure AD Connect](active-directory-aadconnect-design-concepts.md).

Azure AD Connect'in en son sürümle \(Ağustos 2016 veya daha yüksek\), eşitleme hatalarını raporunu kullanılabilir [Azure Portal](https://aka.ms/aadconnecthealth) Azure AD Connect Health eşitleme için bir parçası olarak.

1 Eylül 2016'dan başlayarak [Azure Active Directory yinelenen öznitelik dayanıklılık](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) özellik etkinleştirilecek varsayılan olarak tüm *yeni* Azure Active Directory kiracılar. Bu özellik, gelecek aylarda var olan kiracılar için otomatik olarak etkinleştirilir.

Azure AD Connect eşitleme sürdürür dizinlerden 3 tür işlemler gerçekleştiren: alma, eşitleme ve dışarı aktarma. Tüm işlemlerde hatalar gerçekleşebilir. Bu makalede çoğunlukla hataları Azure ad dışarı aktarılırken ele alınmaktadır.

## <a name="errors-during-export-to-azure-ad"></a>Azure ad dışarı aktarma sırasında hatalar
Bölüm aşağıdaki Azure AD Bağlayıcısı'nı kullanarak Azure ad dışarı aktarma işlemi sırasında oluşabilecek Eşitleme hataları farklı türleri açıklanmaktadır. Bu bağlayıcı "contoso. olan ad biçimi tarafından belirlenebilir. *onmicrosoft.com*".
Azure ad dışarı aktarma sırasında hatalar belirtmek işlemi \(Ekle, Güncelleştir, Sil vb.\) Azure AD Connect tarafından çalıştı \(eşitleme altyapısı\) Azure Active Directory'ye başarısız oldu.

![Dışarı Aktarma hataları genel bakış](./media/active-directory-aadconnect-troubleshoot-sync-errors/Export_Errors_Overview_01.png)

## <a name="data-mismatch-errors"></a>Veri uyuşmazlığı hataları
### <a name="invalidsoftmatch"></a>InvalidSoftMatch
#### <a name="description"></a>Açıklama
* Ne zaman Azure AD Connect \(eşitleme altyapısı\) eklemek veya nesneleri güncelleştirmek için Azure Active Directory bildirir, Azure AD ile eşleşen gelen nesnesini kullanarak **sourceAnchor** özniteliğini **İmmutableıd** Azure AD'de nesnelerin öznitelik. Bu eşleşme olarak adlandırılan bir **sabit eşleşen**.
* Zaman Azure AD **bulamadı** eşleşen herhangi bir nesne **İmmutableıd** ile öznitelik **sourceAnchor** özniteliği gelen nesnesinin yeni bir nesne sağlamadan önce onu geri ProxyAddresses ve UserPrincipalName öznitelikleri bir eşleşme bulmak için kullanmak üzere döner. Bu eşleşme olarak adlandırılan bir **yumuşak eşleşen**. Yazılım eşleşen nesneler (yani Azure AD'de elde edilir) Azure AD içinde zaten mevcut şirket içi aynı varlık (kullanıcılar, gruplar) temsil eden yeni nesneler eşitleme sırasında eklenen/güncellenen olan ile eşleşmesi için tasarlanmıştır.
* **InvalidSoftMatch** hata oluşuyor sabit eşleşme eşleşen hiçbir nesne bulamazsa **ve** yumuşak eşleşme eşleşen bir nesne bulur, ancak bu nesne farklı değerine sahip *İmmutableıd* daha gelen nesnenin *SourceAnchor*, şirket içi Active Directory eşleşen nesne üzerinde başka bir nesneden eşitlendiği önerme.

Diğer bir deyişle, çalışmaya yumuşak eşleşme sırayla yazılımla eşleşen sahip olacak şekilde nesne için herhangi bir değer olmamalıdır *İmmutableıd*. Herhangi bir nesnesi ile varsa *İmmutableıd* eşleştirme sabit bir değer kümesiyle başarısız olan ancak soft-match ölçütlerine işlemi InvalidSoftMatch eşitleme hataya neden olur.

Azure Active Directory Şeması, aşağıdaki özniteliklerin aynı değere sahip iki veya daha fazla nesnelere izin vermiyor. \(Bu kapsamlı bir liste değildir.\)

* ProxyAddresses
* UserPrincipalName
* onPremisesSecurityIdentifier
* ObjectId

> [!NOTE]
> [Azure AD özniteliği yinelenen öznitelik dayanıklılık](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) özelliği ayrıca alınıyor Azure Active Directory varsayılan davranış olarak.  Azure AD daha esnektir, yinelenen ProxyAddresses ve UserPrincipalName öznitelikleri mevcut şirket içi AD ortamda işleme şekilde yaparak bu Azure AD Connect (yanı sıra diğer eşitleme istemciler) görülen eşitleme hatalarının sayısını azaltır. Bu özellik, çoğaltma hataları düzeltin değil. Bu nedenle veri hala düzeltilmesi gerekiyor. Ancak, Azure AD içinde yinelenen değerler nedeniyle sağlanan Aksi takdirde engellenir yeni nesneler sağlanmasına olanak tanır. Ayrıca, eşitleme istemciye döndürülen eşitleme hatalarının sayısını da azaltır.
> Bu özellik Kiracınız için etkinleştirilirse, yeni nesnelerin sağlama sırasında görülen InvalidSoftMatch eşitleme hatalarını görmezsiniz.
>
>

#### <a name="example-scenarios-for-invalidsoftmatch"></a>InvalidSoftMatch için örnek senaryolar
1. İki veya daha fazla nesne ProxyAddresses özniteliğinin aynı değere sahip mevcut şirket içi Active Directory üzerinde. Tek bir Azure AD içinde sağlanır.
2. UserPrincipalName aynı değere sahip iki veya daha fazla nesne mevcut şirket içi Active Directory üzerinde. Tek bir Azure AD içinde sağlanır.
3. Bir nesne içinde içi ProxyAddresses özniteliğinin aynı değere sahip Active Directory, Azure Active Directory'de mevcut nesnesinin olarak eklendi. Şirket içinde eklenen nesne Azure Active Directory'de sağlanmamış.
4. Bir nesne şirket içinde userPrincipalName özniteliğinin aynı değere sahip Active Directory, Azure Active Directory'de bir hesap olarak eklenmiştir. Nesne Azure Active Directory'de sağlanmamış.
5. Eşitlenen hesap ormandan taşındı orman b Azure AD Connect (eşitleme altyapısı) için kullanan bir objectGUID özniteliği SourceAnchor hesaplamak için. Orman taşıma sonrasında SourceAnchor değerini farklıdır. Azure AD içinde varolan bir nesneyle eşitlemek yeni nesneden (orman B) başarısız.
6. Eşitlenen nesne yanlışlıkla şirket içi Active Directory yeni bir nesne oluşturulduğu ve Active Directory'de (örneğin, kullanıcı) aynı varlık için Azure Active Directory hesabında silmeden sildi. Yeni hesap, mevcut Azure AD nesne ile eşitlemek başarısız olur.
7. Azure AD Connect kaldırılır ve yeniden yüklendi. Yeniden yükleme sırasında farklı bir öznitelik SourceAnchor seçildi. Önceden eşitlenmiş tüm nesneleri InvalidSoftMatch hata ile eşitleniyor durduruldu.

#### <a name="example-case"></a>Örnek durum:
1. **Bob Smith** eşitlenen kullanıcının Azure Active Directory'de üzerinde şirket içi Active Directory olduğu *contoso.com*
2. Bob Smith'in **UserPrincipalName** olarak ayarlanmış olan **bobs@contoso.com**.
3. **"abcdefghijklmnopqrstuv=="** is the **SourceAnchor** calculated by Azure AD Connect using Bob Smith's **objectGUID** from on premises Active Directory, which is the **immutableId** for Bob Smith in Azure Active Directory.
4. Bob de sahip için değerleri aşağıdaki **proxyAddresses** özniteliği:
   * smtp:bobs@contoso.com
   * smtp:bob.smith@contoso.com
   * **smtp:bob@contoso.com**
5. Yeni bir kullanıcı **Bob Taylor**, şirket içi Active Directory eklenir.
6. Bob Taylor'ın **UserPrincipalName** olarak ayarlanmış olan **bobt@contoso.com**.
7. **"abcdefghijkl0123456789 ==" "** olan **sourceAnchor** Bob Taylor'ın kullanarak Azure AD Connect tarafından hesaplanan **objectGUID** gelen üzerinde Active Directory şirket içi. Bob Taylor'ın nesne Azure Active Directory'ye henüz eşitlenmedi.
8. Bob Taylor proxyAddresses özniteliği için aşağıdaki değerleri sahip
   * smtp:bobt@contoso.com
   * smtp:bob.taylor@contoso.com
   * **smtp:bob@contoso.com**
9. Eşitleme sırasında Azure AD Connect şirket içi Active Directory içinde Bob Taylor eklenmesi tanıyan ve aynı değişikliği yapmak için Azure AD isteyin.
10. Azure AD ilk sabit eşleşme gerçekleştirir. Eşit İmmutableıd ile herhangi bir nesne ise başka bir deyişle, arayacaktır "abcdefghijkl0123456789 ==". Azure AD'de başka bir nesneyi o İmmutableıd olacağından sabit eşleşme başarısız olur.
11. Azure AD sonra Bob Taylor soft-match dener. Dahil olmak üzere üç değerlere eşit proxyAddresses ile herhangi bir nesne ise başka bir deyişle, arar smtp:bob@contoso.com
12. Azure AD Bob Smith'in soft-match ölçütle eşleşen nesnesine bulur. Ancak bu nesne İmmutableıd değerini = "abcdefghijklmnopqrstuv ==". Bu nesne belirten nesnesinden başka bir şirket içi Active Directory eşitlenen. Bu nedenle, Azure AD yumuşak bu nesneler ve sonuçları eşleştirme olamaz bir **InvalidSoftMatch** eşitleme hatası.

#### <a name="how-to-fix-invalidsoftmatch-error"></a>InvalidSoftMatch hatayı düzeltmek nasıl
En yaygın InvalidSoftMatch hata iki farklı SourceAnchor nesneleriyle nedeni \(İmmutableıd\) üzerinde Azure AD soft-match işlemi sırasında kullanılan ProxyAddresses ve/veya UserPrincipalName öznitelikleri için aynı değere sahip. Geçersiz yazılım eşleşme düzeltmek için

1. Yinelenen proxyAddresses, userPrincipalName veya hataya neden olan başka bir öznitelik değer belirleyin. Ayrıca, iki tanımlamak \(veya daha fazla\) nesneleri çakışmayı söz konusu. Tarafından oluşturulan rapor [Azure AD Connect Health eşitleme](https://aka.ms/aadchsyncerrors) iki nesne belirlemenize yardımcı olabilir.
2. Hangi nesne yinelenen değerine sahip olacak şekilde devam etmesi gerektiğini ve hangi nesne vermemelisiniz belirleyin.
3. Yinelenen değer bu değere sahip olmaması gereken nesnesinden kaldırın. Nesne öğesinden burada kaynaklanan dizininde değişikliği yapmak zorunda unutmayın. Bazı durumlarda, çakışan nesnelerden birini silmeniz gerekebilir.
4. Şirket içi AD değişikliği yaptıysanız, Azure AD Connect eşitleme ayarlarını değiştir olanak tanır.

Eşitleme hata raporu içinde Azure AD Connect Health eşitleme için 30 dakikada bir güncellenir ve en son eşitleme girişimi hatalarından içerir unutmayın.

> [!NOTE]
> İmmutableıd, tanım olarak, nesne yaşam süresi değiştirmemelisiniz. Yukarıdaki listeden aklınızda senaryolardan bazıları ile Azure AD Connect yapılandırılmadıysa, burada Azure AD Connect hesaplar SourceAnchor aynı temsil eden AD nesne için farklı bir değer durumda şunun varolan Azure AD kullanmaya devam etmek istediğiniz nesneye sahip bir varlık (aynı kullanıcı/grup/kişisi vb.).
>
>

#### <a name="related-articles"></a>İlgili Makaleler
* [Office 365'te dizin eşitleme yinelenen ya da geçersiz öznitelikler engelle](https://support.microsoft.com/en-us/kb/2647098)

### <a name="objecttypemismatch"></a>ObjectTypeMismatch
#### <a name="description"></a>Açıklama
Azure AD yumuşak iki nesne eşleşen girişiminde bulunduğunda, farklı iki nesnelerin "nesne türü," (gibi kullanıcı, Grup, ilgili kişi vb.) olası yazılım eşleştirme gerçekleştirmek için kullanılan öznitelikler için aynı değerlere sahip. Çoğaltma bu özniteliklerin Azure AD içinde izin verilmiyor gibi işlemi "ObjectTypeMismatch" eşitleme hatası neden olabilir.

#### <a name="example-scenarios-for-objecttypemismatch-error"></a>ObjectTypeMismatch hata için örnek senaryolar
* Office 365'te posta etkin güvenlik grubu oluşturulur. Yönetici, şirket içi (yani henüz Azure AD ile eşitlenmiş değil) AD ProxyAddresses özniteliği için aynı değeri ile Office 365 grubundan olarak yeni bir kullanıcı veya ilgili kişi olarak ekler.

#### <a name="example-case"></a>Örnek durumu
1. Yönetim için vergi bölüm Office 365'te yeni bir posta etkin güvenlik grubu oluşturur ve bir e-posta adresi olarak sağlar tax@contoso.com. Bu değeri bu gruba ProxyAddresses özniteliğini atar **smtp:tax@contoso.com**
2. Yeni bir kullanıcı Contoso.com ve şirket içi proxyAddress ile kullanıcı için bir hesap oluşturulur **smtp:tax@contoso.com**
3. Yeni kullanıcı hesabı Azure AD Connect eşitleme yapar, "ObjectTypeMismatch" hatası alırsınız.

#### <a name="how-to-fix-objecttypemismatch-error"></a>ObjectTypeMismatch hatayı düzeltmek nasıl
ObjectTypeMismatch hata en yaygın nedeni, (kullanıcı, Grup, ilgili kişi vb.) farklı türde iki nesne ProxyAddresses özniteliği için aynı değere sahip olmasıdır. ObjectTypeMismatch düzeltmek için:

1. Yinelenen proxyAddresses (veya başka bir öznitelik) tanımlayan hataya neden olan değer. Ayrıca, iki tanımlamak \(veya daha fazla\) nesneleri çakışmayı söz konusu. Tarafından oluşturulan rapor [Azure AD Connect Health eşitleme](https://aka.ms/aadchsyncerrors) iki nesne belirlemenize yardımcı olabilir.
2. Hangi nesne yinelenen değerine sahip olacak şekilde devam etmesi gerektiğini ve hangi nesne vermemelisiniz belirleyin.
3. Yinelenen değer bu değere sahip olmaması gereken nesnesinden kaldırın. Nesne öğesinden burada kaynaklanan dizininde değişikliği yapmak zorunda unutmayın. Bazı durumlarda, çakışan nesnelerden birini silmeniz gerekebilir.
4. Şirket içi AD değişikliği yaptıysanız, Azure AD Connect eşitleme ayarlarını değiştir olanak tanır. Eşitleme hata raporu içinde Azure AD Connect Health eşitleme için 30 dakikada bir güncelleştirilir ve en son eşitleme girişimi hatalarından içerir.

## <a name="duplicate-attributes"></a>Yinelenen öznitelikler
### <a name="attributevaluemustbeunique"></a>AttributeValueMustBeUnique
#### <a name="description"></a>Açıklama
Azure Active Directory Şeması, aşağıdaki özniteliklerin aynı değere sahip iki veya daha fazla nesnelere izin vermiyor. Azure AD'de her nesne belirli bir örneğe bu özniteliklerin benzersiz bir değere sahip zorunda olmasıdır.

* ProxyAddresses
* UserPrincipalName

Yeni bir nesne eklemek veya var olan bir nesne zaten başka bir nesne Azure Active Directory'de atanır yukarıdaki öznitelikleri için bir değer ile güncelleştirmek Azure AD Connect çalışırsa, işlemi "AttributeValueMustBeUnique" eşitleme hatası oluşur.

#### <a name="possible-scenarios"></a>Olası senaryolar:
1. Yinelenen değer eşitlenmiş başka bir nesneyle çakışıyor zaten eşitlenmiş bir nesnenin atanır.

#### <a name="example-case"></a>Örnek durum:
1. **Bob Smith** eşitlenen kullanıcının Azure Active Directory'de üzerinde şirket içi Active Directory contoso.com olan
2. Bob Smith'in **UserPrincipalName** şirket içinde olarak ayarlanmış olan **bobs@contoso.com**.
3. Bob de sahip için değerleri aşağıdaki **proxyAddresses** özniteliği:
   * smtp:bobs@contoso.com
   * smtp:bob.smith@contoso.com
   * **smtp:bob@contoso.com**
4. Yeni bir kullanıcı **Bob Taylor**, şirket içi Active Directory eklenir.
5. Bob Taylor'ın **UserPrincipalName** olarak ayarlanmış olan **bobt@contoso.com**.
6. **Bob Taylor** için aşağıdaki değerleri sahip **ProxyAddresses** özniteliği i. smtp:bobt@contoso.com ii. smtp:bob.taylor@contoso.com
7. Bob Taylor'ın nesnesi başarıyla Azure AD ile eşitlenir.
8. Yönetici karar Bob Taylor'ın güncelleştirmek **ProxyAddresses** aşağıdaki değerli özniteliği: ediyorum. **smtp:bob@contoso.com**
9. İşlem olarak ProxyAddresses değeri zaten Bob Smith için oluşan "AttributeValueMustBeUnique" hata atandığını başarısız olur ancak bu azure AD yukarıdaki değerine sahip Azure AD'de Bob Taylor'ın nesneyi güncelleştirme dener.

#### <a name="how-to-fix-attributevaluemustbeunique-error"></a>AttributeValueMustBeUnique hatayı düzeltmek nasıl
En yaygın AttributeValueMustBeUnique hata iki farklı SourceAnchor nesneleriyle nedeni \(İmmutableıd\) ProxyAddresses ve/veya UserPrincipalName öznitelikleri için aynı değere sahip. AttributeValueMustBeUnique hatayı düzeltmek için

1. Yinelenen proxyAddresses, userPrincipalName veya hataya neden olan başka bir öznitelik değer belirleyin. Ayrıca, iki tanımlamak \(veya daha fazla\) nesneleri çakışmayı söz konusu. Tarafından oluşturulan rapor [Azure AD Connect Health eşitleme](https://aka.ms/aadchsyncerrors) iki nesne belirlemenize yardımcı olabilir.
2. Hangi nesne yinelenen değerine sahip olacak şekilde devam etmesi gerektiğini ve hangi nesne vermemelisiniz belirleyin.
3. Yinelenen değer bu değere sahip olmaması gereken nesnesinden kaldırın. Nesne öğesinden burada kaynaklanan dizininde değişikliği yapmak zorunda unutmayın. Bazı durumlarda, çakışan nesnelerden birini silmeniz gerekebilir.
4. Şirket içi AD değişikliği yaptıysanız, Azure AD Connect eşitleme ayarlarını değiştir sabit için hata için olanak sağlar.

#### <a name="related-articles"></a>İlgili Makaleler
-[Office 365'te dizin eşitleme yinelenen ya da geçersiz öznitelikler engelle](https://support.microsoft.com/en-us/kb/2647098)

## <a name="data-validation-failures"></a>Veri doğrulama hataları
### <a name="identitydatavalidationfailed"></a>IdentityDataValidationFailed
#### <a name="description"></a>Açıklama
Azure Active Directory dizinine yazılacak verilerin izin vermeden önce verilerin kendisini çeşitli kısıtlamalar zorlar. Son kullanıcılar bu veriler üzerinde bağımlı olan uygulamalar kullanırken en iyi olası deneyimleri aldığından emin olmak için budur.

#### <a name="scenarios"></a>Senaryolar
a. UserPrincipalName özniteliği değeri geçersiz karakterler içeriyor.
b. UserPrincipalName özniteliği gerekli biçime izlemez.

#### <a name="how-to-fix-identitydatavalidationfailed-error"></a>IdentityDataValidationFailed hatayı düzeltmek nasıl
a. UserPrincipalName özniteliğinin karakterler ve gerekli biçime desteklenen olduğundan emin olun.

#### <a name="related-articles"></a>İlgili Makaleler
* [Office 365 için dizin eşitleme aracılığıyla kullanıcılara sağlamak hazırlama](https://support.office.com/en-us/article/Prepare-to-provision-users-through-directory-synchronization-to-Office-365-01920974-9e6f-4331-a370-13aea4e82b3e)

### <a name="federateddomainchangeerror"></a>FederatedDomainChangeError
#### <a name="description"></a>Açıklama
Bu sonuçlanan, belirli bir durumdur bir **"FederatedDomainChangeError"** kullanıcının UserPrincipalName sonekiyle başka bir Federasyon etki alanına bir Federasyon etki alanından diğerine değiştirildiğinde eşitleme hatası.

#### <a name="scenarios"></a>Senaryolar
Eşitlenen bir kullanıcı için başka bir Federasyon etki alanına şirket içi bir Federasyon etki alanından diğerine UserPrincipalName soneki değiştirildi. Örneğin, *UserPrincipalName = bob@contoso.com*  şekilde değiştirilmiştir *UserPrincipalName = bob@fabrikam.com* .

#### <a name="example"></a>Örnek
1. Bob Smith, Contoso.com için bir hesap ile UserPrincipalName Active Directory'de yeni bir kullanıcı olarak eklenmiş bob@contoso.com
2. Bob için contoso.com Fabrikam.com olarak adlandırılan farklı bölme taşır ve kendi UserPrincipalName değiştirilir bob@fabrikam.com
3. Azure Active Directory ile Federasyon etki alanı contoso.com ve fabrikam.com etki alanlarıdır.
4. Bob'un userPrincipalName güncelleştirilmemiş ve bir "FederatedDomainChangeError" eşitleme hatayla sonuçlanır.

#### <a name="how-to-fix"></a>Düzeltme yapma
Bir kullanıcının UserPrincipalName soneki @ bob gelen güncelleştirildi,**contoso.com** Kemal için**fabrikam.com**, burada her ikisi de **contoso.com** ve **fabrikam.com** olan **Federasyon etki alanları**, eşitleme hatayı düzeltmek için aşağıdaki adımları izleyin

1. Kullanıcının UserPrincipalName Azure AD'den güncelleştirmek bob@contoso.com için bob@contoso.onmicrosoft.com. Azure AD PowerShell modülü ile aşağıdaki PowerShell komutunu kullanabilirsiniz: `Set-MsolUserPrincipalName -UserPrincipalName bob@contoso.com -NewUserPrincipalName bob@contoso.onmicrosoft.com`
2. Eşitleme girişiminde sonraki eşitleme döngüsü izin verir. Bu zaman eşitleme başarılı olur ve, UserPrincipalName Bob güncelleştirecektir bob@fabrikam.com beklendiği gibi.

#### <a name="related-articles"></a>İlgili Makaleler
* [Farklı bir Federasyon etki alanını kullanmak için bir kullanıcı hesabının UPN değiştirdikten sonra değişiklikleri Azure Active Directory eşitleme aracı tarafından eşitlenen değil](https://support.microsoft.com/en-us/help/2669550/changes-aren-t-synced-by-the-azure-active-directory-sync-tool-after-you-change-the-upn-of-a-user-account-to-use-a-different-federated-domain)

## <a name="largeobject"></a>LargeObject
### <a name="description"></a>Açıklama
Bir öznitelik izin verilen boyut sınırı, uzunluk sınırı veya Azure Active Directory şemasına göre sayısı sınırını aşarsa, eşitleme işlemi sonuçlanır **LargeObject** veya **ExceededAllowedLength** eşitleme hatası. Genellikle bu hata için aşağıdaki öznitelikler oluşur.

* userCertificate
* userSMIMECertificate
* thumbnailPhoto
* proxyAddresses

### <a name="possible-scenarios"></a>Olası senaryolar
1. Bob'un userCertificate özniteliği Bob için atanan çok fazla sertifika depolama. Bunlar daha eski, süresi dolan sertifikaları olabilir. Sabit sınırı 15 sertifikaları olur. Kullanıcı sertifikasını özniteliğiyle LargeObject hataların nasıl işleneceğini hakkında daha fazla bilgi için lütfen makalesine başvurun [userCertificate özniteliği tarafından işleme LargeObject hatalardır](active-directory-aadconnectsync-largeobjecterror-usercertificate.md).
2. Bob'un userSMIMECertificate özniteliği Bob için atanan çok fazla sertifika depolama. Bunlar daha eski, süresi dolan sertifikaları olabilir. Sabit sınırı 15 sertifikaları olur.
3. Active Directory'de ayarlanan Bob'un thumbnailPhoto Azure AD'de senkronize çok büyük.
4. Active Directory'de ProxyAddresses öznitelik otomatik doldurma sırasında bir nesne atanan çok fazla ProxyAddresses sahiptir.

### <a name="how-to-fix"></a>Düzeltme yapma
1. Hataya neden olan öznitelik içinde izin verilen sınırlaması olduğundan emin olun.

## <a name="related-links"></a>İlgili bağlantılar
* [Active Directory Yönetim Merkezi'nde Active Directory nesneleri bulun](https://technet.microsoft.com/library/dd560661.aspx)
* [Azure Active Directory PowerShell kullanarak bir nesne için Azure Active Directory sorgulama](https://msdn.microsoft.com/library/azure/jj151815.aspx)
