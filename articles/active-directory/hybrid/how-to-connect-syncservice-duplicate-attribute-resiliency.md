---
title: Kimlik eşitleme ve yinelenen öznitelik dayanıklılığı | Microsoft Docs
description: Azure AD Connect'i kullanarak dizin eşitlemesi sırasında UPN veya ProxyAddress çakışması nesneleriyle işlemek nasıl yeni davranış.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 537a92b7-7a84-4c89-88b0-9bce0eacd931
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/15/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: a65af5a5ea0629b617c4e736d8c110cbb9aa540c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60348816"
---
# <a name="identity-synchronization-and-duplicate-attribute-resiliency"></a>Kimlik eşitleme ve yinelenen öznitelik dayanıklılığı
Yinelenen öznitelik dayanıklılığı kaynaklanan uyuşmazlıkları ortadan kaldıracak Azure Active Directory özelliğidir **UserPrincipalName** ve **ProxyAddress** Microsoft'un birini çalıştırırken çakışıyor Eşitleme araçları.

Bu iki öznitelikler genellikle tüm arasında benzersiz olması gereken **kullanıcı**, **grubu**, veya **kişi** nesneleri belirli bir Azure Active Directory kiracısı.

> [!NOTE]
> Yalnızca kullanıcılar UPN olabilir.
> 
> 

Bu özelliği etkinleştiren yeni davranış eşitleme işlem hattının bulut bölümünde, bu nedenle, belirsiz ve Azure AD Connect, DirSync ve MIM + bağlayıcı dahil olmak üzere herhangi bir Microsoft Eşitleme ürünü için ilgili istemci. Genel bir terim "eşitleme istemcisi", bu belgede, bu ürünlerden birini temsil etmek için kullanılır.

## <a name="current-behavior"></a>Şu anki davranışı
Bu benzersizlik kısıtlamasını ihlal ediyor UPN veya ProxyAddress değeri ile yeni bir nesne sağlama girişimi varsa, Azure Active Directory, nesnesinin oluşturulmasını engeller. Benzer şekilde, bir nesneyi benzersiz olmayan bir UPN veya ProxyAddress güncelleştirildikten sonra güncelleştirme başarısız. Sağlama girişimi veya güncelleştirmeyi her dışarı aktarma döngüsü sırasında eşitleme istemcisi tarafından denenen ve çakışması çözülene kadar başarısız devam eder. Her girişimi sırasında bir hata raporu e-posta oluşturulur ve eşitleme istemcisi tarafından bir hata günlüğe kaydedilir.

## <a name="behavior-with-duplicate-attribute-resiliency"></a>Yinelenen öznitelik dayanıklılığı ile davranışı
Tamamen sağlama veya bir nesne ile yinelenen bir öznitelik güncelleştir başarısız yerine, Azure Active Directory "benzersizlik kısıtlamasını ihlal ediyor yinelenen özniteliği karantinaya". Bu öznitelik gerekliyse, UserPrincipalName gibi sağlamak için hizmeti bir yer tutucu değeri atar. Bu geçici değerleri biçimi  
"***\<OriginalPrefix > +\<4DigitNumber >\@\<InitialTenantDomain >. onmicrosoft.com***".  
Öznitelik gerekli değilse, ister bir **ProxyAddress**, Azure Active Directory yalnızca çakışma öznitelik karantinaya alır ve nesne oluşturma veya güncelleştirme ile devam eder.

Öznitelik karantinaya üzerine çakışması hakkında bilgi eski davranışa kullanılan aynı hata raporu e-posta gönderilir. Karantina gerçekleştiğinde, ancak bu bilgileri, hata raporunda yalnızca bir kez görüntülenir. Bu gelecekteki e-postalarda günlüğe kaydedilecek devam etmez. Ayrıca, bu nesne için dışarı aktarma başarılı oldu eşitleme istemcisi bir hata günlüğe kaydetmez ve oluşturma yeniden denemez / güncelleştirme işlemi sırasında sonraki eşitleme döngüsü.

Bu davranışı destekleyen yeni bir öznitelik kullanıcı, Grup ve iletişim nesnesi sınıflarına eklenmiştir:  
**DirSyncProvisioningErrors**

Bu normalde eklenmelidir benzersizlik kısıtlamasını ihlal ediyor çakışan öznitelikleri depolamak için kullanılan birden çok değerli bir özniteliğidir. Yinelenen öznitelik çakışmaları Çözüldü ve söz konusu öznitelikleri karantinadan otomatik olarak kaldırır aramak için her saat çalışır Azure Active Directory'de bir arka plan Zamanlayıcı görevi etkinleştirildi.

### <a name="enabling-duplicate-attribute-resiliency"></a>Yinelenen öznitelik dayanıklılığı etkinleştirme
Yinelenen öznitelik dayanıklılığı yeni varsayılan davranış, tüm Azure Active Directory kiracılar genelinde olacaktır. Üzerinde varsayılan olarak 22 Ağustos 2016 veya sonraki sürümlerinde ilk kez eşitleme etkin tüm kiracılar için de artar. Bu tarihten önce eşitleme özelliği etkin kiracılar özelliğinin toplu olarak etkin olacaktır. Bu sunum Eylül 2016'da başlar ve her bir kiracının Teknik Bildirim kişisi özelliği etkin olduğunda belirli bir tarih içeren bir e-posta bildirimi gönderilir.

> [!NOTE]
> Yinelenen öznitelik dayanıklılığı açıldı sonra devre dışı bırakılamaz.

Bu özellik, kiracınız için etkin olup olmadığını denetlemek için Azure Active Directory PowerShell modülünün en son sürümü indirme ve çalıştırma bunu yapabilirsiniz:

`Get-MsolDirSyncFeatures -Feature DuplicateUPNResiliency`

`Get-MsolDirSyncFeatures -Feature DuplicateProxyAddressResiliency`

> [!NOTE]
> Artık, kiracınız için açık önce proaktif bir şekilde yinelenen öznitelik dayanıklılığı özelliğini etkinleştirmek için Set-MsolDirSyncFeature cmdlet'ini kullanabilirsiniz. Özelliği test etmek için yeni bir Azure Active Directory Kiracı oluşturmanız gerekir.

## <a name="identifying-objects-with-dirsyncprovisioningerrors"></a>DirSyncProvisioningErrors nesneleriyle tanımlama
Şu anda bu hataları nedeniyle yinelenen özellik çakışmaları, Azure Active Directory PowerShell sahip nesneleri tanımlamak için iki yöntem vardır ve [Microsoft 365 Yönetim merkezini](https://admin.microsoft.com). Ek portal tabanlı gelecekte Raporlama ile genişletmek için bir plan yok.

### <a name="azure-active-directory-powershell"></a>Azure Active Directory PowerShell
PowerShell cmdlet'leri için bu konudaki aşağıdaki geçerlidir:

* Aşağıdaki cmdlet'ler büyük küçük harfe duyarlı değildir.
* **– ErrorCategory PropertyConflict** her zaman dahil edilmesi gerekir. Şu anda başka tür vardır **ErrorCategory**, ancak bu gelecekte uzatılabilir.

İlk olarak çalıştırarak kullanmaya başlama **Connect-MsolService** ve için bir kiracı Yöneticisi kimlik bilgilerini girme.

Ardından farklı şekillerde hataları görüntülemek için aşağıdaki cmdlet'leri ve işleçler kullanın:

1. [Tümünü Göster](#see-all)
2. [Özellik türü tarafından](#by-property-type)
3. [Çakışan değere göre](#by-conflicting-value)
4. [Bir dize aramayı kullanma](#using-a-string-search)
5. Sıralı
6. [Sınırlı bir miktar veya tümü](#in-a-limited-quantity-or-all)

#### <a name="see-all"></a>Tümünü incele
Öznitelik sağlamanın genel bir listesini görmek için hataları kiracıdaki bağlandıktan sonra çalıştırın:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict`

Bu, aşağıdakine benzer bir sonuç üretir:  
 ![Get-MsolDirSyncProvisioningError](./media/how-to-connect-syncservice-duplicate-attribute-resiliency/1.png "Get-MsolDirSyncProvisioningError")  

#### <a name="by-property-type"></a>Özellik türü tarafından
Özellik türü tarafından hataları görmek için Ekle **- PropertyName** ile bayrak **UserPrincipalName** veya **ProxyAddresses** bağımsız değişkeni:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName UserPrincipalName`

Veya

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName ProxyAddresses`

#### <a name="by-conflicting-value"></a>Çakışan değere göre
Eklemek için belirli bir özellik ile ilgili hatalar görmeniz **- PropertyValue** bayrağı (**- PropertyName** bu bayrağı eklerken de kullanılmalıdır):

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyValue User@domain.com -PropertyName UserPrincipalName`

#### <a name="using-a-string-search"></a>Bir dize aramayı kullanma
Yapmak için bir geniş dize arama kullanın **- SearchString** bayrağı. Bu bağımsız olarak tüm yukarıdaki bayrakları dışında kullanılabilir **- ErrorCategory PropertyConflict**, her zaman gerekli olan:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -SearchString User`

#### <a name="in-a-limited-quantity-or-all"></a>Sınırlı bir miktar veya tümü
1. **Maxresults bağımsız değişkenini \<Int >** değerleri belirli sayıda sorguya sınırlamak için kullanılabilir.
2. **Tüm** tüm sonuçları, çok sayıda hata var durumda alınır emin olmak için kullanılabilir.

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -MaxResults 5`

## <a name="microsoft-365-admin-center"></a>Microsoft 365 Yönetim Merkezi
Microsoft 365 Yönetim merkezinde dizin eşitleme hataları görüntüleyebilirsiniz. Raporda Microsoft 365 Yönetim Merkezi yalnızca görüntüler **kullanıcı** bu hataları olan nesne. Arasındaki çakışmalar hakkında bilgi göstermez **grupları** ve **kişiler**.

![Etkin kullanıcılar](./media/how-to-connect-syncservice-duplicate-attribute-resiliency/1234.png "etkin kullanıcılar")

Microsoft 365 Yönetim merkezinde dizin eşitleme hataları görüntülemek yönergeler için bkz: [Office 365'te dizin eşitleme hataları belirlemenize](https://support.office.com/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067).

### <a name="identity-synchronization-error-report"></a>Kimlik eşitleme hata raporu
Yinelenen öznitelik çakışma sahip bir nesne zaman işlenir davranışlarla teknik bildirim gönderilir standart kimlik eşitleme hata raporu e-postadaki bildirim dahil bu Kiracı için iletişime geçin. Ancak, bu davranışı önemli bir değişiklik yoktur. Çakışma çözümlendi kadar her sonraki hata raporu geçmişte, yinelenen öznitelik çakışma hakkında bilgi dahil edilir. Bu yeni davranış ile belirli bir çakışma hata bildirimine yalnızca bir kez - çakışan bir öznitelik karantinaya zaman gibi görünüyor.

E-posta bildirimi ProxyAddress çakışma nasıl göründüğüne ilişkin bir örnek aşağıda verilmiştir:  
    ![Etkin kullanıcılar](./media/how-to-connect-syncservice-duplicate-attribute-resiliency/6.png "etkin kullanıcılar")  

## <a name="resolving-conflicts"></a>Çakışmaları çözümleme
Stratejisi ve çözüm taktikleri bu hataları için sorun giderme yinelenen öznitelik hataları geçmişte işlenen biçimi farklı olmalıdır değil. Tek fark, Zamanlayıcı görevi çakışma giderildikten sonra söz konusu öznitelik için uygun nesne otomatik olarak eklemek için hizmet tarafında Kiracı taramalar ' dir.

Aşağıdaki makalede çeşitli sorun giderme ve çözüm stratejileri özetler: [Yinelenen veya geçersiz öznitelik Office 365'te dizin eşitlemesini engelle](https://support.microsoft.com/kb/2647098).

## <a name="known-issues"></a>Bilinen sorunlar
Bu bilinen sorunlar hiçbiri, veri kaybı veya hizmet düşmesine neden olur. Birkaç tanesinin estetik, diğerleri standart neden "*öncesi dayanıklılık*" çakışma özniteliği ve başka karantinaya yerine oluşturulması için yinelenen öznitelik hataları fazladan el ile düzeltme yukarı gerektirecek şekilde belirli hataları neden olur.

**Çekirdek davranışı:**

1. Nesneleri belirli bir öznitelik yapılandırmaları ile karantinaya alınmış yinelenen öznitelikleri aksine dışarı aktarma hataları almaya devam eder.  
   Örneğin:
   
    a. Yeni kullanıcı bir UPN ile AD'de oluşturulur **ALi\@contoso.com** ve ProxyAddress **smtp:Joe\@contoso.com**
   
    b. Bu nesnenin özelliklerini ProxyAddress olduğu mevcut bir grubu ile çakışan **SMTP:Joe\@contoso.com**.
   
    c. Dışarı aktarma, bağlı bir **ProxyAddress çakışma** karantinaya çakışma özniteliklere sahip yerine hata oluşur. Dayanıklılık özellik etkinleştirilmeden önce olabilirdi gibi işlemi her bir sonraki eşitleme döngüsü sırasında yeniden denenir.
2. İki grup aynı SMTP adresi ile şirket içi oluşturulursa standart yinelenen ile yapılan ilk girişim üzerinde sağlama başarısız **ProxyAddress** hata. Ancak, yinelenen değer sonraki eşitleme döngüsü sırasında düzgün karantinaya alındı.

**Office portalı rapor**:

1. Ayrıntılı hata iletisini UPN çakışma küme içindeki iki nesne için aynıdır. Bu, bunların her ikisi de değiştirilen / karantinaya aslında yalnızca bir tanesi değiştirilen herhangi bir veri varken UPN çalıştırılmış gösterir.
2. Yanlış displayName değiştirildi ve karantinaya UPN oluşmuş bir kullanıcının UPN çakışma için ayrıntılı hata iletisi gösterir. Örneğin:
   
    a. **Kullanıcı A** öncelikle ile eşitlemeler **UPN = kullanıcı\@contoso.com**.
   
    b. **B kullanıcısı** yukarı sonraki ile eşitlenmesi çalışıldı **UPN = kullanıcı\@contoso.com**.
   
    c. **B kullanıcısının** UPN değiştiğinde **User1234\@contoso.onmicrosoft.com** ve **kullanıcı\@contoso.com** eklenir **DirSyncProvisioningErrors** .
   
    d. İçin hata iletisini **B kullanıcısı** gösterilmelidir **kullanıcısı** zaten **kullanıcı\@contoso.com** UPN, ancak gösterildiği gibi **kullanıcı B'nin** kendi displayName.

**Kimlik eşitleme hata raporu**:

Bağlantı için *bu sorunu gidermek adımları* yanlış:  
    ![Etkin kullanıcılar](./media/how-to-connect-syncservice-duplicate-attribute-resiliency/6.png "etkin kullanıcılar")  

İşaret etmelidir [ https://aka.ms/duplicateattributeresiliency ](https://aka.ms/duplicateattributeresiliency).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
* [Office 365'te dizin eşitleme hataları belirleyin](https://support.office.com/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067)

