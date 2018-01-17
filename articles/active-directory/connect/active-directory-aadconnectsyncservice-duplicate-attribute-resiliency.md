---
title: "Kimlik eşitleme ve yinelenen öznitelik dayanıklılık | Microsoft Docs"
description: "Azure AD Connect'i kullanarak dizin eşitleme sırasında UPN veya ProxyAddress çakışan nesneleri işlemek nasıl yeni davranışı."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 537a92b7-7a84-4c89-88b0-9bce0eacd931
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.openlocfilehash: 7953e218614ba259db3cd45220de6b6c880608ad
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="identity-synchronization-and-duplicate-attribute-resiliency"></a>Kimlik eşitleme ve yinelenen öznitelik dayanıklılığı
Yinelenen öznitelik dayanıklılık nedeni uyuşmazlık giderilecektir Azure Active Directory'de bir özelliktir **UserPrincipalName** ve **ProxyAddress** Microsoft'un eşitleme araçlardan birini çalıştırırken çakışıyor.

Bu iki özellik genellikle tüm arasında benzersiz olması gereken **kullanıcı**, **grup**, veya **kişi** nesneleri belirli bir Azure Active Directory kiracısı.

> [!NOTE]
> Yalnızca kullanıcıların UPN olabilir.
> 
> 

Bu özelliği etkinleştiren yeni davranışı eşitleme ardışık düzen bulut bölümünde, bu nedenle, istemci belirsiz ve Azure AD Connect, DirSync ve MIM + bağlayıcı dahil olmak üzere herhangi bir Microsoft Eşitleme ürünü için ilgili. Genel bir terim "eşitleme istemcisi" Bu belgede, bu ürünlerden birini temsil etmek için kullanılır.

## <a name="current-behavior"></a>Şu anki davranışı
Bu benzersizlik kısıtlamasını ihlal eden bir UPN veya ProxyAddress değer ile yeni bir nesne sağlamak girişimi varsa, Azure Active Directory bu nesnesinin oluşturulmasını engeller. Benzer şekilde, bir nesne bir benzersiz olmayan UPN veya ProxyAddress ile güncelleştirdiyseniz güncelleştirme başarısız. Sağlama denemesi veya güncelleştirme her dışarı aktarma döngüsü sırasında eşitleme istemci tarafından yeniden denenir ve çakışması çözülene kadar başarısız olmaya devam eder. Her girişimi sırasında bir hata raporu e-posta oluşturulur ve eşitleme istemci tarafından bir hata günlüğe kaydedilir.

## <a name="behavior-with-duplicate-attribute-resiliency"></a>Yinelenen öznitelik dayanıklılığına sahip davranışı
Tamamen sağlamak veya yinelenen bir öznitelik bir nesne güncelleştirmek başarısız yerine, Azure Active Directory "hangi benzersizlik kısıtlamasını ihlal ediyor yinelenen öznitelik karantinaya". Bu öznitelik, UserPrincipalName gibi sağlamak için gerekliyse, hizmetin bir yer tutucu değerini atar. Bu geçici değerleri biçimi  
"***<OriginalPrefix>+ < 4DigitNumber > @<InitialTenantDomain>. onmicrosoft.com***".  
Öznitelik gerekli değilse, ister bir **ProxyAddress**, Azure Active Directory sadece çakışma özniteliği karantinaya ve nesne oluşturma veya güncelleştirme işlemi devam eder.

Öznitelik karantinaya alma sırasında çakışması hakkında bilgi eski davranış kullanılan aynı hata raporu e-posta gönderilir. Karantina gerçekleştiğinde, ancak, bu bilgileri, hata raporunda yalnızca bir kez görüntülenir. Bu gelecekteki postalarda tutulacak devam etmez. Ayrıca, bu nesne için dışarı aktarma başarılı olduktan sonra eşitleme istemcisi bir hata günlüğe kaydetmez ve oluşturma yeniden denemez / güncelleştirmesi sonraki eşitleme döngüsü sırasında işlemi.

Bu davranış desteklemek için yeni bir öznitelik kullanıcı, Grup ve iletişim nesne sınıfları için eklendi:  
**DirSyncProvisioningErrors**

Normal olarak eklenmelidir benzersizlik kısıtlamasını ihlal ediyor çakışan öznitelikleri depolamak için kullanılan birden çok değerli bir özniteliktir. Bir arka plan Zamanlayıcı görevi çözümlendi ve söz konusu öznitelikleri karantinadan otomatik olarak kaldırır yinelenen öznitelik çakışmaları aramak için her saat çalışır Azure Active Directory'de etkinleştirildi.

### <a name="enabling-duplicate-attribute-resiliency"></a>Yinelenen öznitelik dayanıklılık etkinleştirme
Yinelenen öznitelik dayanıklılık yeni varsayılan davranış tüm Azure Active Directory kiracılar arasında olacaktır. Bu üzerinde ilk kez 22 Ağustos 2016 veya sonraki sürümlerde eşitlemenin etkinleştirilmiş tüm kiracılar için varsayılan olarak olacaktır. Bu tarihten önce eşitlemenin etkin kiracılar özelliğinin toplu olarak etkin olacaktır. Bu sunum Eylül 2016'da başlayacak ve her bir kiracının Teknik Bildirim kişi özelliği etkin olduğunda belirli bir tarih ile bir e-posta bildirimi gönderilir.

> [!NOTE]
> Yinelenen öznitelik dayanıklılık açık durumda sonra devre dışı bırakılamaz.

Özellik kiracınız için etkin olup olmadığını denetlemek için Azure Active Directory PowerShell modülü en son sürümünü indirme ve çalıştırma bunu yapabilirsiniz:

`Get-MsolDirSyncFeatures -Feature DuplicateUPNResiliency`

`Get-MsolDirSyncFeatures -Feature DuplicateProxyAddressResiliency`

> [!NOTE]
> Artık, kiracınız için açık önce proaktif olarak yinelenen öznitelik dayanıklılık özelliğini etkinleştirmek için Set-MsolDirSyncFeature cmdlet'ini kullanabilirsiniz. Özellik test edebilmek için yeni bir Azure Active Directory Kiracı oluşturmanız gerekir.

## <a name="identifying-objects-with-dirsyncprovisioningerrors"></a>DirSyncProvisioningErrors nesneleriyle tanımlama
Bu hatalar nedeniyle yinelenen özellik çakışmaları, Azure Active Directory PowerShell ve Office 365 Yönetici portalı sahip nesneleri tanımlamak için şu anda iki yöntem vardır. Ek portal tabanlı gelecekte Raporlama ile genişletmek için bir plan yok.

### <a name="azure-active-directory-powershell"></a>Azure Active Directory PowerShell
İçin PowerShell cmdlet'leri bu konuda aşağıdaki geçerlidir:

* Aşağıdaki cmdlet'leri büyük küçük harfe duyarlı değildir.
* **– ErrorCategory PropertyConflict** her zaman dahil edilmelidir. Şu anda başka tür olan **ErrorCategory**, ancak bu gelecekte uzatılabilir.

İlk olarak, çalıştırarak kullanmaya başlama **Bağlan MsolService** ve için Kiracı Yöneticisi kimlik bilgilerini girme.

Ardından, farklı şekillerde hataları görüntülemek için aşağıdaki cmdlet'leri ve işleçleri kullanın:

1. [Tüm bakın](#see-all)
2. [Özellik türü tarafından](#by-property-type)
3. [Çakışan değerine göre](#by-conflicting-value)
4. [Bir dize arama kullanma](#using-a-string-search)
5. [Sıralanmış](#sorted)
6. [Sınırlı bir miktar veya tümü](#in-a-limited-quantity-or-all)

#### <a name="see-all"></a>Tümünü incele
Öznitelik sağlama genel listesini görmek için bağlandıktan sonra Kiracı hataları çalıştırın:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict`

Bu, aşağıdaki gibi bir sonuç oluşturur:  
 ![Get-MsolDirSyncProvisioningError](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1.png "Get-MsolDirSyncProvisioningError")  

#### <a name="by-property-type"></a>Özellik türü tarafından
Özellik türü hataları görmek için Ekle'ye **- PropertyName** ile bayrak **UserPrincipalName** veya **ProxyAddresses** bağımsız değişkeni:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName UserPrincipalName`

Veya

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName ProxyAddresses`

#### <a name="by-conflicting-value"></a>Çakışan değerine göre
Eklemek için belirli bir özellik ile ilgili hataları görmek için **- PropertyValue** bayrağı (**- PropertyName** de bu bayrak eklerken kullanılması gerekir):

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyValue User@domain.com -PropertyName UserPrincipalName`

#### <a name="using-a-string-search"></a>Bir dize arama kullanma
Bir geniş dize arama kullanımı yapmak için **- AramaDizesi** bayrağı. Bu bağımsız olarak tüm yukarıdaki bayrakları dışında kullanılabilir **- ErrorCategory PropertyConflict**, olduğu her zaman gerekli:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -SearchString User`

#### <a name="in-a-limited-quantity-or-all"></a>Sınırlı bir miktar veya tümü
1. **MaxResults <Int>**  değerleri belirli sayıda sorguya sınırlamak için kullanılabilir.
2. **Tüm** tüm sonuçları, çok sayıda hataları mevcut durumda alınır emin olmak için kullanılabilir.

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -MaxResults 5`

## <a name="office-365-admin-portal"></a>Office 365 Yönetici portalı
Dizin eşitleme hatalarına Office 365 Yönetim Merkezi'nde görüntüleyebilirsiniz. Office 365 portalında rapor yalnızca görüntüler **kullanıcı** bu hataları olan nesne. Arasındaki çakışmaları hakkında bilgi göstermez **grupları** ve **kişiler**.

![Etkin kullanıcılar](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1234.png "etkin kullanıcılar")

Office 365 Yönetim Merkezi'nde dizin eşitleme hataları görüntüleme hakkında daha fazla yönerge için bkz: [Office 365'te dizin eşitleme hatalarına tanımlamak](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067).

### <a name="identity-synchronization-error-report"></a>Kimlik eşitleme hata raporu
Yinelenen öznitelik çakışma sahip bir nesne zaman işlenir Kiracı için bir bildirim Teknik Bildirim gönderilen standart kimlik eşitleme hata raporu e-posta dahil bu yeni davranış kişiyle. Ancak, bu davranış önemli bir değişiklik yoktur. Çakışması çözülene kadar her sonraki hata raporu geçmişte, yinelenen öznitelik çakışma hakkında bilgi dahil edilir. Bu yeni davranış ile belirli bir çakışma için hata bildirimi yalnızca bir kez - çakışan öznitelik karantinaya aynı anda gibi görünüyor.

E-posta bildirimi nasıl ProxyAddress çakışma göründüğünü örneği şöyledir:  
    ![Etkin kullanıcılar](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "etkin kullanıcılar")  

## <a name="resolving-conflicts"></a>Çakışmalarını çözme
Strateji ve çözüm taktiği bu hataları için sorun giderme yinelenen öznitelik hataları geçmişte işlenen biçimi farklı. Tek fark, Zamanlayıcı görevi çakışma çözüldükten sonra söz konusu öznitelik uygun nesnesine otomatik olarak eklemek için hizmet tarafı üzerinde Kiracı taramalar ' dir.

Aşağıdaki makalede çeşitli sorun giderme ve çözümleme stratejileri özetlenmektedir: [yinelenen ya da geçersiz öznitelikler önlemek Office 365'te dizin eşitleme](https://support.microsoft.com/kb/2647098).

## <a name="known-issues"></a>Bilinen sorunlar
Bu bilinen sorunlar hiçbiri veri kaybı veya hizmet düşüşüne neden olur. Birkaç estetik, diğerleri standart neden "*öncesi dayanıklılık*" çakışma özniteliği ve başka karantinaya alma yerine durum için yinelenen öznitelik hataları fazladan el ile düzeltme yukarı gerektirecek şekilde belirli hataları neden olur.

**Çekirdek davranışı:**

1. Özel öznitelik yapılandırmaları nesneleriyle karantinaya alınmış yinelenen öznitelikler aksine verme hataları almaya devam eder.  
   Örneğin:
   
    a. Yeni kullanıcı AD UPN ile oluşturulur  **Joe@contoso.com**  ve ProxyAddress**smtp:Joe@contoso.com**
   
    b. Bu nesnenin özelliklerini ProxyAddress olduğu varolan bir grupla çakışma  **SMTP:Joe@contoso.com** .
   
    c. Dışa aktarma, üzerine bir **ProxyAddress çakışma** hata karantinaya çakışma özniteliklere sahip yerine oluşur. Dayanıklılık özellik etkinleştirilmeden önce olabilirdi gibi işlemi her sonraki eşitleme döngüsü sırasında denenir.
2. İki grup aynı SMTP adresi ile şirket içi oluşturulursa sağlama standart yinelenen ile ilk denemede başarısız **ProxyAddress** hata. Ancak, yinelenen değer düzgün sonraki eşitleme döngüsü sırasında karantinaya alınır.

**Office portalı rapor**:

1. Ayrıntılı hata iletisi UPN çakışma kümesinde iki nesneler için aynıdır. Bu, bunların her ikisi de değiştirilmiş / karantinaya gerçekte yalnızca bir tanesi değiştirilen veri varken UPN Değerlerinin beklendiğinden gösterir.
2. Değiştirilen/karantinaya UPN Değerlerinin oluşmuş bir kullanıcı için yanlış displayName UPN çakışma için ayrıntılı hata iletisi gösterir. Örneğin:
   
    a. **Kullanıcı A** eşitlenir ilk yukarı **UPN = User@contoso.com** .
   
    b. **Kullanıcı B** sonraki ile yukarı senkronize girişiminde bulunuldu **UPN = User@contoso.com** .
   
    c. **B kullanıcısının** UPN değiştirildi  **User1234@contoso.onmicrosoft.com**  ve  **User@contoso.com**  eklenen **DirSyncProvisioningErrors**.
   
    d. İçin hata iletisini **kullanıcı B** bildiren **kullanıcısı** zaten  **User@contoso.com**  UPN, ancak gösterildiği gibi **kullanıcı B'nin** kendi görünen adı.

**Kimlik eşitleme hata raporu**:

Bağlantı için *adımlar bu sorunu gidermek nasıl* yanlış:  
    ![Etkin kullanıcılar](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "etkin kullanıcılar")  

İşaret etmelidir [https://aka.ms/duplicateattributeresiliency](https://aka.ms/duplicateattributeresiliency).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure AD Connect eşitleme](active-directory-aadconnectsync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
* [Office 365'te dizin eşitleme hataları tanımlar](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067)

