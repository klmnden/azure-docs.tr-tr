---
title: 'Azure AD Connect: Eşitleme sırasında karşılaşılan hataları giderme | Microsoft Docs'
description: Azure AD Connect ile eşitleme sırasında karşılaşılan hataların nasıl giderileceği açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 2209d5ce-0a64-447b-be3a-6f06d47995f8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/29/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: f63aebb9a9bbefe84ac36b92cd69e0d93de0ab76
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66298749"
---
# <a name="troubleshooting-errors-during-synchronization"></a>Eşitleme sırasında karşılaşılan hataları giderme
Windows Server Active Directory'den (AD DS) Azure Active Directory (Azure AD) kimlik verilerini eşitlendiğinde hataları oluşabilir. Bu makalede, bu hataları ve hataları düzeltmek için olası yolları neden olası senaryolar bazıları eşitleme hatalarını farklı türde genel bir bakış sağlar. Bu makalede, sık karşılaşılan hata türlerini içerir ve tüm olası hataları kapsamayabilir.

 Bu makalede, okuyucu temel alınan ile ilgili bilgi sahibi olduğunu varsayar [tasarım kavramlarını Azure AD ile Azure AD Connect](plan-connect-design-concepts.md).

Azure AD Connect'in en son sürümle \(Ağustos 2016 veya daha yüksek\), eşitleme hata raporu kullanılabilir [Azure portalında](https://aka.ms/aadconnecthealth) Azure AD Connect Health eşitleme için bir parçası olarak.

1 Eylül 2016'dan itibaren [Azure Active Directory yinelenen öznitelik dayanıklılığı](how-to-connect-syncservice-duplicate-attribute-resiliency.md) özelliği etkinleştirilir varsayılan olarak tüm *yeni* Azure Active Directory kiracıları. Bu özellik gelecek ay içinde var olan kiracılar için otomatik olarak etkinleştirilecektir.

Azure AD Connect eşitlenmiş tutar dizinlerden üç işlem gerçekleştirir: İçeri aktarma, eşitleme ve dışarı aktarma. Hataları tüm işlemler gerçekleştirilebilir. Bu makalede, Azure AD'ye dışarı aktarma sırasında hataları çoğunlukla odaklanır.

## <a name="errors-during-export-to-azure-ad"></a>Azure AD'ye dışarı aktarma sırasında hatalar
Aşağıdaki bölümde, Azure AD Bağlayıcısı'nı kullanarak Azure AD'ye dışarı aktarma işlemi sırasında oluşabilecek Eşitleme hataları farklı türleri açıklanmaktadır. Bu bağlayıcı, "contoso. olan adı biçimi tarafından tanımlanabilir *onmicrosoft.com*".
Azure AD'ye dışarı aktarma sırasında hatalar belirtmek işlemi \(ekleme, güncelleştirme ve silme vb.\) Azure AD Connect tarafından çalıştı \(eşitleme altyapısı\) Azure Active Directory'de başarısız oldu.

![Dışarı Aktarma hataları genel bakış](./media/tshoot-connect-sync-errors/Export_Errors_Overview_01.png)

## <a name="data-mismatch-errors"></a>Veri uyumsuzluğu hatası
### <a name="invalidsoftmatch"></a>InvalidSoftMatch
#### <a name="description"></a>Açıklama
* Azure AD bağladığınızda \(eşitleme altyapısı\) eklenecek veya güncelleştirilecek nesneler Azure Active Directory bildirir, Azure AD ile eşleşen gelen nesnesini kullanarak **sourceAnchor** özniteliğini **Immutableıd**  özniteliği Azure AD'de nesne. Bu eşleşme olarak adlandırılan bir **sabit eşleşen**.
* Zaman Azure AD **bulunamadı** eşleşen herhangi bir nesne **Immutableıd** özniteliğini **sourceAnchor** yeni sağlamadan önce gelen nesnesinin özniteliği nesnesi, bir eşleştirme bulmak üzere ProxyAddresses ve UserPrincipalName özniteliklerini kullanmaya geri döner. Bu eşleşme olarak adlandırılan bir **yazılımla eşleşen**. Yazılım eşleşen nesneleri (Azure AD'de elde edilir) Azure AD'de zaten mevcut şirket içi aynı varlık (kullanıcılar, gruplar) temsil eden yeni nesneleri ekledik/güncelleştirdik eşitleme sırasında aktarılan ile eşleşecek şekilde tasarlanmıştır.
* **InvalidSoftMatch** hata oluşuyor eşleşen herhangi bir nesne sabit eşleşme bulamazsa **ve** eşleşen bir nesne geçici eşleşme bulur, ancak bu nesne farklı değerine sahiptir *Immutableıd* daha gelen nesnenin *SourceAnchor*, şirket içi Active Directory eşleşen nesne üzerinde başka bir nesneden eşitlendiği önerme.

Diğer bir deyişle, sırayla çalışmak üzere yazılım eşleşme ile geçici olarak eşleştirilen alınacak nesneyi herhangi bir değer olmamalıdır *Immutableıd*. Tüm ile nesnesinin *Immutableıd* eşleşme sabit bir değer kümesiyle başarısız olan ancak geçici eşleştirme ölçütlerine, işlemi bir InvalidSoftMatch eşitleme hataya neden olur.

Azure Active Directory Şeması aşağıdaki özniteliklerin aynı değere sahip iki veya daha fazla nesnelere izin vermez. \(Bu kapsamlı bir liste değildir.\)

* ProxyAddresses
* userPrincipalName
* onPremisesSecurityIdentifier
* ObjectId

> [!NOTE]
> [Azure AD özniteliği yinelenen öznitelik dayanıklılığı](how-to-connect-syncservice-duplicate-attribute-resiliency.md) özelliği de piyasaya sunuluyor Azure Active Directory varsayılan davranış olarak.  Azure AD karşı daha dayanıklı yinelenen ProxyAddresses ve UserPrincipalName özniteliklerini AD şirket içi ortamda bulunan işler şekilde yaparak bu Azure AD Connect (aynı zamanda tarafından diğer eşitleme istemciler) görülen eşitleme hatalarının sayısını azaltır. Bu özellik, çoğaltma hataları düzeltin değil. Bu nedenle veri hala düzeltilmesi gerekir. Ancak Azure AD'de yinelenen değerler nedeniyle sağlanan Aksi takdirde engellenir yeni nesnelerin sağlanmasına olanak tanır. Ayrıca, eşitleme istemciye döndürülen eşitleme hatalarının sayısını da azaltır.
> Kiracınız için bu özellik etkinleştirilirse, yeni nesnelerin sağlama sırasında görülen InvalidSoftMatch eşitleme hatalarını görmezsiniz.
>
>

#### <a name="example-scenarios-for-invalidsoftmatch"></a>InvalidSoftMatch için örnek senaryolar
1. ProxyAddresses özniteliği için aynı değere sahip iki veya daha fazla nesneyi, şirket içi Active Directory'de yer alır. Yalnızca biri Azure AD'de sağlanır.
2. Mevcut şirket içi Active Directory'de iki veya daha fazla nesne userPrincipalName özniteliği için aynı değere sahip. Yalnızca biri Azure AD'de sağlanır.
3. Bir nesne, Azure Active Directory'de mevcut bir nesnenin aynı değeri ProxyAddresses özniteliği ile Active Directory şirket içi eklendi. Şirket içi eklenen nesne Azure Active Directory'de sağlanmayan.
4. Bir nesne şirket içi Active Directory ile aynı değeri userPrincipalName özniteliği, Azure Active Directory'de bir hesap olarak eklenmiştir. Nesne Azure Active Directory'de, sağlanmamış.
5. Eşitlenen bir hesap ormandan taşındı orman b Azure AD Connect'i (eşitleme altyapısı) için kullanarak objectGUID özniteliği SourceAnchor hesaplamak için. Orman taşımadan sonra SourceAnchor değerine farklıdır. Yeni nesneden (orman B), Azure AD'de mevcut nesnesi ile eşitlemek başarısız oluyor.
6. Eşitlenen nesne yanlışlıkla gelen şirket içi Active Directory ve yeni bir nesne oluşturulduğu Active Directory'de aynı varlığa (örneğin, kullanıcı) Azure Active Directory hesabı silmeden silindi. Yeni hesap, mevcut Azure AD nesnesi ile eşitlemek başarısız olur.
7. Azure AD Connect kaldırılıp yeniden. Yeniden yükleme sırasında farklı bir öznitelik SourceAnchor seçildi. Eşitleme InvalidSoftMatch hatası ile önceden eşitlenmiş tüm nesneleri durduruldu.

#### <a name="example-case"></a>Örnek durum:
1. **Bob Smith** olan Azure Active Directory'de eşitlenen bir kullanıcı şirket tarafından şirket içi Active Directory *contoso.com*
2. Bob Smith'in **UserPrincipalName** olarak ayarlandığından **ebrunun\@contoso.com**.
3. **"abcdefghijklmnopqrstuv =="** olan **SourceAnchor** Bob Smith'in kullanarak Azure AD Connect tarafından hesaplanan **objectGUID** gelen Active Directory, şirket içinde olduğu **İmmutableıd** Azure Active Directory'de Bob Smith için.
4. Bob de sahip için değerleri aşağıdaki **proxyAddresses** özniteliği:
   * SMTP: bobs@contoso.com
   * SMTP: bob.smith@contoso.com
   * **SMTP: bob\@contoso.com**
5. Yeni bir kullanıcı **Bob Taylor**, şirket içi Active Directory eklenir.
6. Bob Taylor'ın **UserPrincipalName** olarak ayarlandığından **bobt\@contoso.com**.
7. **"abcdefghijkl0123456789 ==" "** olduğu **sourceAnchor** Bob Taylor'ın kullanarak Azure AD Connect tarafından hesaplanan **objectGUID** gelen üzerinde Active Directory şirket içi. Bob Taylor'ın nesne Azure Active Directory'ye henüz eşitlenmedi.
8. Bob Taylor proxyAddresses özniteliği için aşağıdaki değerlere sahip
   * SMTP: bobt@contoso.com
   * SMTP: bob.taylor@contoso.com
   * **SMTP: bob\@contoso.com**
9. Eşitleme sırasında Azure AD Connect Bob Taylor şirket içi Active Directory eklenmesini tanıması ve aynı değişikliği yapmak için Azure AD isteyin.
10. Azure AD, ilk sabit eşleşme gerçekleştirir. Eşittir Immutableıd ile herhangi bir nesne ise diğer bir deyişle, arar "abcdefghijkl0123456789 ==". Azure AD'de başka bir nesne yok, Immutableıd olumsuz etkileyeceğinden sabit eşleşme başarısız olur.
11. Azure AD ardından geçici eşleştirme Bob Taylor dener. Diğer bir deyişle, smtp dahil olmak üzere üç değerden herhangi bir nesne proxyAddresses ile eşitse arar: bob@contoso.com
12. Azure AD Bob Smith'in geçici eşleştirme ölçütlerine uyan nesnesine bulabilirsiniz. Ancak bu nesne değeri Immutableıd = "abcdefghijklmnopqrstuv ==". Bu nesne gösteren nesnesinden başka bir şirket içi Active Directory eşitlendi. Bu nedenle, Azure AD geçici bu nesneleri ve sonuçları eşleştirme edilemez bir **InvalidSoftMatch** eşitleme hatası.

#### <a name="how-to-fix-invalidsoftmatch-error"></a>Nasıl InvalidSoftMatch hatayı düzeltmek için
Farklı SourceAnchor ile iki nesneleri InvalidSoftMatch hatanın en yaygın nedeni, \(Immutableıd\) soft-match sırasında kullanılan ProxyAddresses ve/veya UserPrincipalName öznitelikler için aynı değere sahip Azure AD'de işlem. Geçersiz geçici eşleştirme düzeltmek için

1. Yinelenen proxyAddresses, userPrincipalName veya hataya neden olan diğer öznitelik değeri belirleyin. Ayrıca, iki tanımlar \(veya daha fazla\) nesneleri çakışmayı söz konusu. Tarafından oluşturulan raporu [Azure AD Connect Health eşitleme](https://aka.ms/aadchsyncerrors) iki nesne belirlemenize yardımcı olabilir.
2. Hangi nesne yinelenen değerine sahip olacak şekilde çalışmaya devam etmesi ve hangi nesne barındırmamalıdır belirleyin.
3. Yinelenen değer, bu değer olmaması gereken nesneden kaldırın. Burada nesne kaynağı dizin değişikliği yapmanız gerekir. Bazı durumlarda, çakışan nesnelerinden birini silmeniz gerekebilir.
4. Şirket içi AD değişikliği yaptıysanız, Azure AD Connect eşitleme ayarlarını değiştir olanak tanır.

Eşitleme hata raporu içinde Azure AD Connect Health eşitleme için 30 dakikada bir güncelleştirilir ve en son eşitleme denemesi hataları içerir.

> [!NOTE]
> Immutableıd, tanımı gereği, nesnenin yaşam süresini değiştirmemelisiniz. Azure AD Connect ile bazı senaryolar Yukarıdaki listeden aklınızda yapılandırılmamışsa, bir durumda, Azure AD Connect SourceAnchor aynı varlığı temsil eden AD nesne için farklı bir değerini hesaplar nerede bulunabileceğini (aynı kullanıcı/Grup / kişi vb.) var olan Azure AD kullanmaya devam etmek için istediğiniz bir nesneyi sahiptir.
>
>

#### <a name="related-articles"></a>İlgili makaleler
* [Yinelenen veya geçersiz öznitelik Office 365'te dizin eşitlemesini engelle](https://support.microsoft.com/kb/2647098)

### <a name="objecttypemismatch"></a>ObjectTypeMismatch
#### <a name="description"></a>Açıklama
Azure AD, yumuşak iki nesnenin aynı girişiminde bulunduğunda, farklı iki nesne "nesne türü," (kullanıcı, grup vb. kişi gibi) bir olası geçici eşleştirme uygulamak için kullanılan öznitelikler için aynı değerlere sahip olur. Çoğaltma bu öznitelikler Azure AD'de izin verilmiyor gibi işlemi "ObjectTypeMismatch" eşitleme hataya neden.

#### <a name="example-scenarios-for-objecttypemismatch-error"></a>ObjectTypeMismatch hata için örnek senaryolar
* Office 365'te bir posta etkin güvenlik grubu oluşturulur. Yönetici, şirket içinde (Bu, henüz Azure AD ile eşitlenmemesi) AD ProxyAddresses özniteliği için aynı değere sahip olan Office 365 grubu yeni bir kullanıcı veya kişi ekler.

#### <a name="example-case"></a>Örnek durum
1. Yönetici yeni bir posta etkin güvenlik grubu için vergi bölümü Office 365'te oluşturur ve bir e-posta adresi olarak sağlar tax@contoso.com. Bu grup ProxyAddresses özniteliği değeri atanır **smtp: vergi\@contoso.com**
2. Yeni bir kullanıcı Contoso.com birleştirir ve şirket içi proxyAddress ile kullanıcı için bir hesap oluşturulduktan **smtp: vergi\@contoso.com**
3. Yeni kullanıcı hesabının Azure AD Connect eşitleme yapar "ObjectTypeMismatch" hatasını alırsınız.

#### <a name="how-to-fix-objecttypemismatch-error"></a>Nasıl ObjectTypeMismatch hatayı düzeltmek için
ObjectTypeMismatch hatanın en yaygın nedeni, (kullanıcı, Grup, ilgili kişi vb.) farklı türde iki nesne ProxyAddresses özniteliği için aynı değere sahip olmasıdır. ObjectTypeMismatch çözmek için:

1. Yinelenen proxyAddresses (veya başka bir öznitelik) tanımlamak bir hataya neden olan değer. Ayrıca, iki tanımlar \(veya daha fazla\) nesneleri çakışmayı söz konusu. Tarafından oluşturulan raporu [Azure AD Connect Health eşitleme](https://aka.ms/aadchsyncerrors) iki nesne belirlemenize yardımcı olabilir.
2. Hangi nesne yinelenen değerine sahip olacak şekilde çalışmaya devam etmesi ve hangi nesne barındırmamalıdır belirleyin.
3. Yinelenen değer, bu değer olmaması gereken nesneden kaldırın. Burada nesne kaynağı dizinde değişiklik yapmadan unutmayın. Bazı durumlarda, çakışan nesnelerinden birini silmeniz gerekebilir.
4. Şirket içi AD değişikliği yaptıysanız, Azure AD Connect eşitleme ayarlarını değiştir olanak tanır. Eşitleme hata raporu içinde Azure AD Connect Health eşitleme için 30 dakikada bir güncelleştirilir ve en son eşitleme denemesi hataları içerir.

## <a name="duplicate-attributes"></a>Yinelenen öznitelikler
### <a name="attributevaluemustbeunique"></a>AttributeValueMustBeUnique
#### <a name="description"></a>Açıklama
Azure Active Directory Şeması aşağıdaki özniteliklerin aynı değere sahip iki veya daha fazla nesnelere izin vermez. Azure AD'de her nesne belirli bir örneği bu özniteliklerin benzersiz bir değere sahip zorunda olmasıdır.

* ProxyAddresses
* userPrincipalName

Azure AD Connect'i yeni bir nesne eklemek veya var olan bir nesne zaten başka bir nesne Azure Active Directory'de atanan yukarıdaki öznitelikler için bir değer ile güncelleştirmek çalışırsa, işlemi "AttributeValueMustBeUnique" eşitleme hatası oluşur.

#### <a name="possible-scenarios"></a>Olası senaryolar:
1. Yinelenen değer eşitlenmiş başka bir nesneyle çakışıyor zaten eşitlenmiş bir nesneye atanır.

#### <a name="example-case"></a>Örnek durum:
1. **Bob Smith** Azure Active Directory'de eşitlenen bir kullanıcı şirket tarafından şirket içi Active Directory contoso.com olan
2. Bob Smith'in **UserPrincipalName** şirket içi olarak ayarlandığından **ebrunun\@contoso.com**.
3. Bob de sahip için değerleri aşağıdaki **proxyAddresses** özniteliği:
   * SMTP: bobs@contoso.com
   * SMTP: bob.smith@contoso.com
   * **SMTP: bob\@contoso.com**
4. Yeni bir kullanıcı **Bob Taylor**, şirket içi Active Directory eklenir.
5. Bob Taylor'ın **UserPrincipalName** olarak ayarlandığından **bobt\@contoso.com**.
6. **Bob Taylor** için şu değerleri **ProxyAddresses** i özniteliği. SMTP: bobt@contoso.com ii. SMTP: bob.taylor@contoso.com
7. Bob Taylor'ın nesnesini Azure AD'ye başarıyla eşitlendi.
8. Yönetici kararı Bob Taylor'ın güncelleştirilecek **ProxyAddresses** şu değere sahip bir öznitelik: ediyorum. **SMTP: bob\@contoso.com**
9. İşlem olarak ProxyAddresses değeri zaten Bob Smith için oluşan "AttributeValueMustBeUnique" hata atandığını başarısız olur, ancak azure AD'ye yukarıdaki değerle Azure AD'de Bob Taylor'ın nesne güncelleştirme dener.

#### <a name="how-to-fix-attributevaluemustbeunique-error"></a>Nasıl AttributeValueMustBeUnique hatayı düzeltmek için
Farklı SourceAnchor ile iki nesneleri AttributeValueMustBeUnique hatanın en yaygın nedeni, \(Immutableıd\) ProxyAddresses ve/veya UserPrincipalName öznitelikler için aynı değere sahip. AttributeValueMustBeUnique hatayı düzeltmek için

1. Yinelenen proxyAddresses, userPrincipalName veya hataya neden olan diğer öznitelik değeri belirleyin. Ayrıca, iki tanımlar \(veya daha fazla\) nesneleri çakışmayı söz konusu. Tarafından oluşturulan raporu [Azure AD Connect Health eşitleme](https://aka.ms/aadchsyncerrors) iki nesne belirlemenize yardımcı olabilir.
2. Hangi nesne yinelenen değerine sahip olacak şekilde çalışmaya devam etmesi ve hangi nesne barındırmamalıdır belirleyin.
3. Yinelenen değer, bu değer olmaması gereken nesneden kaldırın. Burada nesne kaynağı dizinde değişiklik yapmadan unutmayın. Bazı durumlarda, çakışan nesnelerinden birini silmeniz gerekebilir.
4. Şirket içi AD değişikliği yaptıysanız, Azure AD Connect'i eşitleme hatası düzeltilecek değişikliğin olanak tanır.

#### <a name="related-articles"></a>İlgili makaleler
-[Yinelenen veya geçersiz öznitelik Office 365'te dizin eşitlemesini engelle](https://support.microsoft.com/kb/2647098)

## <a name="data-validation-failures"></a>Veri doğrulama hataları
### <a name="identitydatavalidationfailed"></a>IdentityDataValidationFailed
#### <a name="description"></a>Açıklama
Azure Active Directory dizinine yazılacak verilerin izin vermeden önce verileri çeşitli kısıtlamalar uygular. Bu kısıtlamalar, son kullanıcılar bu verilerine bağlı uygulamaları kullanırken en iyi olası deneyimleri sunun üzeresiniz.

#### <a name="scenarios"></a>Senaryolar
a. UserPrincipalName özniteliği değeri geçersiz/desteklenmeyen karakterler içeriyor.
b. UserPrincipalName özniteliği, gerekli biçime uymuyor.

#### <a name="how-to-fix-identitydatavalidationfailed-error"></a>Nasıl IdentityDataValidationFailed hatayı düzeltmek için
a. UserPrincipalName özniteliği, karakterleri ve gerekli biçime desteklenen olduğundan emin olun.

#### <a name="related-articles"></a>İlgili makaleler
* [Kullanıcıların Office 365 için dizin eşitleme yoluyla sağlamak hazırlama](https://support.office.com/article/Prepare-to-provision-users-through-directory-synchronization-to-Office-365-01920974-9e6f-4331-a370-13aea4e82b3e)

### <a name="federateddomainchangeerror"></a>FederatedDomainChangeError
#### <a name="description"></a>Açıklama
Bu durumda sonuçlanır bir **"FederatedDomainChangeError"** kullanıcının UserPrincipalName sonekiyle başka bir Federasyon etki alanına bir Federasyon etki alanından değiştirildiğinde eşitleme hatası.

#### <a name="scenarios"></a>Senaryolar
Eşitlenen bir kullanıcı için bir Federasyon etki alanından başka bir Federasyon etki alanına şirket içi UserPrincipalName soneki değiştirildi. Örneğin, *UserPrincipalName bob =\@contoso.com* değiştirilmiştir *UserPrincipalName bob =\@fabrikam.com*.

#### <a name="example"></a>Örnek
1. Bob Smith, Contoso.com için bir hesap ile UserPrincipalName Active Directory'de yeni bir kullanıcı olarak eklenmiş bob@contoso.com
2. Bob contoso.com, Fabrikam.com olarak adlandırılan farklı bir bölüm taşır ve bunların UserPrincipalName değiştirildi bob@fabrikam.com
3. Hem contoso.com ve fabrikam.com etki alanları, Azure Active Directory ile Federasyon etki alanları bulunur.
4. Bob'ın userPrincipalName güncelleştirilmez ve "FederatedDomainChangeError" eşitleme hatasına neden olur.

#### <a name="how-to-fix"></a>Nasıl düzeltileceğini
Bir kullanıcının UserPrincipalName soneki bob ' ' nden güncelleştirildi,**contoso.com** Kemal için\@**fabrikam.com**, burada her ikisi de **contoso.com** ve  **Fabrikam.com** olan **Federasyon etki alanları**, eşitleme hatayı düzeltmek için aşağıdaki adımları izleyin

1. Azure AD'den kullanıcının UserPrincipalName güncelleştirmek bob@contoso.com için bob@contoso.onmicrosoft.com. Azure AD PowerShell modülü ile aşağıdaki PowerShell komutunu kullanabilirsiniz: `Set-MsolUserPrincipalName -UserPrincipalName bob@contoso.com -NewUserPrincipalName bob@contoso.onmicrosoft.com`
2. Eşitleme girişiminde sonraki eşitleme döngüsü sağlar. Bu zaman eşitleme başarılı olur ve, UserPrincipalName Bob güncelleştirecektir bob@fabrikam.com beklendiği gibi.

#### <a name="related-articles"></a>İlgili makaleler
* [Farklı bir Federasyon etki alanı kullanmak için bir kullanıcı hesabının UPN'sini değiştirdikten sonra değişiklikleri Azure Active Directory Sync aracının eşitlenmiş değil](https://support.microsoft.com/help/2669550/changes-aren-t-synced-by-the-azure-active-directory-sync-tool-after-you-change-the-upn-of-a-user-account-to-use-a-different-federated-domain)

## <a name="largeobject"></a>LargeObject
### <a name="description"></a>Açıklama
Bir öznitelik izin verilen boyut sınırı, uzunluk sınırı veya Azure Active Directory şeması tarafından ayarlanan sayı limitini aşarsa, eşitleme işlemi sonuçlanır **LargeObject** veya **ExceededAllowedLength**eşitleme hatası. Bu hata genellikle aşağıdaki özniteliklerde oluşur

* userCertificate
* userSMIMECertificate
* thumbnailPhoto
* proxyAddresses

### <a name="possible-scenarios"></a>Olası senaryolar
1. Bob için atanmış çok fazla sertifika Bob'un userCertificate özniteliğinin yol açtığı depoladığı. Bunlar eski, süresi dolmuş sertifikaları olabilir. Sabit sınırı 15 sertifikaları ' dir. UserCertificate özniteliğinin yol açtığı LargeObject hataları işlemek nasıl hakkında daha fazla bilgi için lütfen makaleye başvurun [neden userCertificate özniteliğinin yol açtığı LargeObject hatalarını işleme](tshoot-connect-largeobjecterror-usercertificate.md).
2. Bob için atanmış çok fazla sertifika depoladığı Bob'un Usersmımecertificate özniteliği. Bunlar eski, süresi dolmuş sertifikaları olabilir. Sabit sınırı 15 sertifikaları ' dir.
3. Active Directory'de ayarlanan Bob'un thumbnailPhoto Azure AD'ye eşitlenmesi için çok büyük.
4. Active Directory ProxyAddresses özniteliğindeki otomatik popülasyonu sırasında bir nesnenin atanmış çok fazla ProxyAddresses vardır.

### <a name="how-to-fix"></a>Nasıl düzeltileceğini
1. Hataya neden olan öznitelik içinde izin verilen sınırlaması olduğundan emin olun.

## <a name="existing-admin-role-conflict"></a>Mevcut Yönetici rolü çakışması

### <a name="description"></a>Açıklama
Bir **mevcut Yönetici rolü çakışma** bu kullanıcı nesnesi olan bir kullanıcı nesnesinde eşitleme sırasında ortaya çıkar:

- Yönetici izinleri ve
- Mevcut bir Azure AD nesnesi olarak aynı UserPrincipalName

Azure AD Connect izin verilmiyor geçici olarak eşleşen bir kullanıcı nesnesi şirket içinden Azure AD'de Yönetici rolü atanmış olan bir kullanıcı nesnesi ile AD.  Daha fazla bilgi için [Azure AD UserPrincipalName popülasyon](plan-connect-userprincipalname.md)

![Mevcut yönetici](media/tshoot-connect-sync-errors/existingadmin.png)


### <a name="how-to-fix"></a>Nasıl düzeltileceğini
Bu sorunu aşağıdakilerden birini yapın çözmek için:


- UserPrincipalName, bir yönetici kullanıcının Azure AD'de - eşleşen UserPrincipalName ile Azure AD'de yeni kullanıcı oluşturacak eşleşmeyen bir değere değiştirin
- Yönetim rolü, şirket içi kullanıcı nesnesi ve mevcut Azure AD kullanıcı nesnesi arasında yumuşak eşleşme sağlayan Azure AD'de yönetici kullanıcıdan kaldırın.

>[!NOTE]
>Şirket içi kullanıcı nesnesi ve Azure AD kullanıcı nesnesi arasında yumuşak eşleşme tamamlandıktan sonra Yönetim rolü için mevcut kullanıcı nesnesi yeniden atayabilirsiniz.

## <a name="related-links"></a>İlgili bağlantılar
* [Active Directory Yönetim Merkezi'nde Active Directory nesneleri bulun](https://technet.microsoft.com/library/dd560661.aspx)
* [Azure Active Directory PowerShell kullanarak bir nesne için Azure Active Directory sorgulama](https://msdn.microsoft.com/library/azure/jj151815.aspx)
