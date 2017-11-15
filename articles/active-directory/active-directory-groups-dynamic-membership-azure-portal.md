---
title: "Azure Active Directory öznitelik tabanlı dinamik grup üyeliği | Microsoft Docs"
description: "Dinamik grup üyeliği de dahil olmak üzere için Gelişmiş kurallar oluşturmak nasıl ifade kural işleçleri ve parametreleri desteklenmiyor."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: fb434cc2-9a91-4ebf-9753-dd81e289787e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: H1Hack27Feb2017;it-pro
ms.openlocfilehash: 0bf6177bc34b6f7daf9c14a22c3b381025f0f825
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="create-attribute-based-rules-for-dynamic-group-membership-in-azure-active-directory"></a>Azure Active Directory'de dinamik grup üyeliği için öznitelik tabanlı kurallar oluşturma
Azure Active Directory (Azure AD), karmaşık öznitelik tabanlı gruplara yönelik dinamik üyelikler etkinleştirmek için Gelişmiş kurallar oluşturabilirsiniz. Bu makalede, öznitelikleri ve kullanıcılar veya cihazlar için dinamik Üyelik kuralları oluşturmak için sözdizimi ayrıntıları.

Herhangi bir kullanıcı veya aygıt özniteliklerini değiştirdiğinizde, sistem değişikliği tetikleyecek herhangi bir grup ekler veya kaldırır olup bir dizindeki tüm dinamik Grup kurallarını değerlendirir. Bir kullanıcı veya cihaza bir grupta kuralı uymazsa, bunlar bu grubun bir üyesi olarak eklenir. Bunlar artık kural karşılamak varsa, bunlar kaldırılır.

> [!NOTE]
> - Güvenlik gruplarında veya Office 365 gruplarında dinamik üyelik için bir kural ayarlayabilirsiniz.
>
> - Bu özellik en az bir dinamik grubuna eklenen her kullanıcı üyesi için bir Azure AD Premium P1 lisansı gerektirir. Gerçekte dinamik grupların üyesi olacak şekilde kullanıcılara lisans atamak için zorunlu değildir, ancak tüm kullanıcıları kapsayacak şekilde Kiracı içinde en az sayıda lisansa sahip gerekir. Örneğin: kiracınızda tüm dinamik gruplarında 1.000 benzersiz kullanıcıların toplam varsa, lisans gereksinimleri sağlamak için Azure AD Premium P1 için ya da yukarıdaki 1. 000'en az lisanslara sahip gerekir.
>
> - Aygıtların veya kullanıcıların dinamik bir grup oluşturabilirsiniz, ancak kullanıcı ve aygıt nesneleri içeren bir kuralı oluşturulamıyor.

> - Şu anda kullanıcı özniteliklerine sahip bağlı bir cihaz grubu oluşturmak mümkün değil. Cihaz Üyelik kuralları yalnızca dizinde cihaz nesnelerinin hemen özniteliklerini başvuruda bulunabilir.

## <a name="to-create-an-advanced-rule"></a>Gelişmiş bir kural oluşturmak için
1. Oturum [Azure AD Yönetim Merkezi](https://aad.portal.azure.com) genel bir yönetici veya kullanıcı hesabı yönetici olan bir hesapla.
2. Seçin **kullanıcılar ve gruplar**.
3. Seçin **tüm grupları**seçip **yeni grup**.

   ![Yeni Grup Ekle](./media/active-directory-groups-dynamic-membership-azure-portal/new-group-creation.png)

4. Üzerinde **grup** dikey penceresinde, bir ad ve yeni grup için bir açıklama girin. Seçin bir **üyelik türü** herhangi birinin **dinamik kullanıcı** veya **dinamik cihaz**kullanıcılar veya cihazlar için bir kural oluşturmak ve ardından seçinisteyipistemediğinizibağlıolarak**Ekle dinamik sorgu**. Basit bir kural oluşturmak için kural Oluşturucusu'nu kullanın veya gelişmiş bir kuralı kendiniz yazın. Bu makale kullanılabilir kullanıcı ve cihaz öznitelikleri ve bunun yanı sıra gelişmiş kurallar örnekleri hakkında daha fazla bilgi içerir.

   ![Dinamik Üyelik Kuralı Ekle](./media/active-directory-groups-dynamic-membership-azure-portal/add-dynamic-group-rule.png)

5. Kural oluşturduktan sonra Seç **Ekle sorgu** dikey pencerenin altındaki.
6. Seçin **oluşturma** üzerinde **grup** grubu oluşturmak için dikey.

> [!TIP]
> Girdiğiniz Gelişmiş kural yanlış olduysa grup oluşturma işlemi başarısız olabilir. Sağ üst köşede portalı bir bildirim görüntülenir; neden kural sistem tarafından kabul edilemedi, bir açıklama içerir. Bu kuralın geçerli yapmak için ayarlamak gereken nasıl dikkatle anlamak için okuyun.

## <a name="constructing-the-body-of-an-advanced-rule"></a>Gelişmiş bir kural gövdesi oluşturma
Gruplara yönelik dinamik üyelikler için oluşturabileceğiniz Gelişmiş kural üç bölümden oluşur ve bir true veya false sonucu sonuçları aslında bir ikili ifadesidir. Üç bölümden şunlardır:

* Sol parametre
* İkili işleç
* Sağ sabiti

Tam gelişmiş kural şuna benzer: (leftParameter işlecin "RightConstant"), açma ve kapama parantezi tüm ikili ifade için isteğe bağlı olduğu durumlarda da çift tırnak işareti de sadece sağ sabiti için gerekli isteğe bağlıdır ne zaman dizesinin olduğundan ve user.property sol parametre sözdizimi aşağıdaki gibidir. Gelişmiş bir kuralı ikili ifadeler ayırarak birden oluşabilir ve- ya da ve - değil mantıksal işleçler.

Doğru şekilde oluşturulmuş bir Gelişmiş kuralının örnekleri aşağıda verilmiştir:
```
(user.department -eq "Sales") -or (user.department -eq "Marketing")
(user.department -eq "Sales") -and -not (user.jobTitle -contains "SDE")
```
Desteklenen parametreler ve ifade kural işleçleri tam listesi için aşağıdaki bölümlere bakın. Cihaz kuralları için kullanılan öznitelikler için bkz: [aygıt nesneler için kurallar oluşturmak için öznitelikleri kullanma](#using-attributes-to-create-rules-for-device-objects).

Gelişmiş kural gövdesi toplam uzunluğu 2048 karakterden uzun olamaz.

> [!NOTE]
> Dize ve regex işlemlerinin büyük küçük harfe duyarlı değildir. Ayrıca gerçekleştirebilirsiniz Null denetimleri, örneğin bir sabit değer olarak $null kullanarak, user.department - eq $null.
> Tırnak işareti içeren bir dizeler "kullanarak kaçışlı ' karakter, örneğin, user.department - eq \`"Satış".

## <a name="supported-expression-rule-operators"></a>Desteklenen ifade kural işleçleri
Aşağıdaki tabloda, Gelişmiş kural gövdesindeki kullanılacak tüm desteklenen ifade kural işleçleri ve bunların söz dizimi listelenmektedir:

| İşleç | Sözdizimi |
| --- | --- |
| Eşit değildir |-ne |
| Eşittir |-eq |
| İle başlayan değil |-notStartsWith |
| İle başlar |-startsWith |
| İçermez |-notContains |
| Contains |-içerir |
| Eşleşmiyor |-notMatch |
| eşleşme |-eşleşmesi |
| İçinde | -içinde |
| İçinde değil | -notIn |

## <a name="operator-precedence"></a>İşleç önceliği

Tüm işleçleri, daha düşük öncelikli büyük başına aşağıda listelenmiştir. Aynı satırdaki işleçleri içinde eşit önceliğe şunlardır:
````
-any -all
-or
-and
-not
-eq -ne -startsWith -notStartsWith -contains -notContains -match –notMatch -in -notIn
````
Tüm işleçleri ile veya tire öneki olmadan kullanılabilir. Yalnızca öncelik gereksinimlerinizi karşılamadığında parantez gereklidir.
Örneğin:
```
   user.department –eq "Marketing" –and user.country –eq "US"
```
eşdeğerdir:
```
   (user.department –eq "Marketing") –and (user.country –eq "US")
```
## <a name="using-the--in-and--notin-operators"></a>Kullanarak içinde ve - notIn işleçleri

Bir dizi farklı değerlere karşı bir kullanıcı özniteliğinin değeri karşılaştırma yapmak isterseniz kullanabileceğiniz içinde ya da - notIn işleçler. Örneği kullanarak işlecinde:
```
    user.department -In [ "50001", "50002", "50003", “50005”, “50006”, “50007”, “50008”, “50016”, “50020”, “50024”, “50038”, “50039”, “51100” ]
```
Kullanımına dikkat edin "[" ve "]" başında ve, değerler listesinin sonuna. Bu durum, user.department eşittir listesindeki değerlerin birini değeri true olarak değerlendirilir.


## <a name="query-error-remediation"></a>Sorgu hata düzeltme
Aşağıdaki tabloda yaygın hatalar ve bunların nasıl düzeltileceği listeler

| Sorgu ayrıştırma hatası | Hata kullanımı | Düzeltilmiş kullanımı |
| --- | --- | --- |
| Hata: özniteliği desteklenmiyor. |(user.invalidProperty - eq "Value") |(user.department - eq "value")<br/><br/>Öznitelik olduğundan emin olun üzerinde [özellikleri listesi desteklenen](#supported-properties). |
| Hata: Öznitelikte işleci desteklenmiyor. |(user.accountEnabled-true içerir) |(user.accountEnabled - eq true)<br/><br/>Özellik türü için kullanılan işleci desteklenmiyor (Bu örnekte,-içeren kullanılamaz boolean türü). Doğru işleçler özellik türü için kullanın. |
| Hata: Sorgu derleme hatası oluştu. |1. (user.department - eq "Satış") (user.department - eq "Pazarlama")<br/><br/>2. (user.userPrincipalName-eşleşen "*@domain.ext") |1. İşleç eksik. Kullanın - veya - veya iki koşulları katılın<br/><br/>(user.department - eq "Satış")- veya (user.department - eq "Pazarlama")<br/><br/>2. hata - kullanılan normal ifade ile eşleşir<br/><br/>(user.userPrincipalName-eşleşen ". *@domain.ext"), bunun yerine: (user.userPrincipalName-eşleşen "@domain.ext$")|

## <a name="supported-properties"></a>Desteklenen özellikler
Gelişmiş kuralınız kullanabileceğiniz tüm kullanıcı özellikleri şunlardır:

### <a name="properties-of-type-boolean"></a>Boolean özelliklerini türü
İzin verilen işleçleri

* -eq
* -ne

| Özellikler | İzin verilen değerler | Kullanım |
| --- | --- | --- |
| accountEnabled |doğru false |user.accountEnabled - eq true |
| dirSyncEnabled |doğru false |user.dirSyncEnabled - eq true |

### <a name="properties-of-type-string"></a>Dize türünde özellikleri
İzin verilen işleçleri

* -eq
* -ne
* -notStartsWith
* -StartsWith
* -içerir
* -notContains
* -eşleşmesi
* -notMatch
* -içinde
* -notIn

| Özellikler | İzin verilen değerler | Kullanım |
| --- | --- | --- |
| city |Herhangi bir dize değeri veya $null |(user.city - eq "value") |
| Ülke |Herhangi bir dize değeri veya $null |(Resource.country - eq "value") |
| Şirket adı | Herhangi bir dize değeri veya $null | (user.companyName - eq "value") |
| Bölüm |Herhangi bir dize değeri veya $null |(user.department - eq "value") |
| Görünen adı |Herhangi bir dize değeri |(user.displayName - eq "value") |
| facsimileTelephoneNumber |Herhangi bir dize değeri veya $null |(user.facsimileTelephoneNumber - eq "value") |
| givenName |Herhangi bir dize değeri veya $null |(user.givenName - eq "value") |
| İş Unvanı |Herhangi bir dize değeri veya $null |(user.jobTitle - eq "value") |
| Posta |Herhangi bir dize değeri veya $null (kullanıcının SMTP adresi) |(user.mail - eq "value") |
| mailNickName |Herhangi bir dize değeri (kullanıcı diğer adı posta) |(user.mailNickName - eq "value") |
| Mobil |Herhangi bir dize değeri veya $null |(user.mobile - eq "value") |
| objectID |Kullanıcı nesnesinin GUID |(user.objectId - eq "1111111-1111-1111-1111-111111111111") |
| onPremisesSecurityIdentifier | Güvenlik tanımlayıcısı (SID) şirket içi buluta eşitlenmiş olan kullanıcılar için şirket içi. |(user.onPremisesSecurityIdentifier - eq "S-1-1-11-1111111111-1111111111-1111111111-1111111") |
| passwordPolicies |Hiçbiri DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration, DisableStrongPassword |(user.passwordPolicies - eq "DisableStrongPassword") |
| physicalDeliveryOfficeName |Herhangi bir dize değeri veya $null |(user.physicalDeliveryOfficeName - eq "value") |
| posta kodu |Herhangi bir dize değeri veya $null |(user.postalCode - eq "value") |
| preferredLanguage |ISO 639-1 kodu |(user.preferredLanguage - eq "en-US") |
| sipProxyAddress |Herhangi bir dize değeri veya $null |(user.sipProxyAddress - eq "value") |
| durum |Herhangi bir dize değeri veya $null |(user.state - eq "value") |
| StreetAddress |Herhangi bir dize değeri veya $null |(user.streetAddress - eq "value") |
| Soyadı |Herhangi bir dize değeri veya $null |(user.surname - eq "value") |
| telephoneNumber |Herhangi bir dize değeri veya $null |(user.telephoneNumber - eq "value") |
| usageLocation |İki harflerin ülke kodu |(user.usageLocation - eq "ABD") |
| userPrincipalName |Herhangi bir dize değeri |(user.userPrincipalName - eq "alias@domain") |
| UserType |üye Konuk $null |(user.userType - eq "Üye") |

### <a name="properties-of-type-string-collection"></a>Türü dize koleksiyonunun özellikleri
İzin verilen işleçleri

* -içerir
* -notContains

| Özellikler | İzin verilen değerler | Kullanım |
| --- | --- | --- |
| otherMails |Herhangi bir dize değeri |(user.otherMails-içeren "alias@domain") |
| proxyAddresses |SMTP: alias@domain smtp:alias@domain |(user.proxyAddresses-içeren "SMTP: alias@domain") |

## <a name="multi-value-properties"></a>Birden çok değerli özellikleri
İzin verilen işleçleri

* -herhangi bir (zaman en az bir koleksiyondaki bir öğe koşulu ile eşleşen memnun)
* -(tüm koleksiyondaki tüm öğeler koşulu eşleştiğinde memnun)

| Özellikler | Değerler | Kullanım |
| --- | --- | --- |
| assignedPlans |Koleksiyondaki her nesnenin aşağıdaki dize özellikleri sunar: capabilityStatus, hizmet, servicePlanId |user.assignedPlans-tüm (assignedPlan.servicePlanId - eq "efb87545-963c-4e0d-99df-69c6916d9eb0"- ve assignedPlan.capabilityStatus - eq "Etkin") |

Çoklu değer, aynı türde nesne koleksiyonları özelliklerdir. Kullanabileceğiniz - bir koşul birini veya tümünü koleksiyondaki öğelerin sırasıyla uygulamak için herhangi bir ve - tüm işleçler. Örneğin:

assignedPlans kullanıcıya atanan tüm hizmet planları listeleyen birden çok değerli bir özelliktir. İfade da etkin durumda olan Exchange Online (planı 2) hizmet planı olan kullanıcılar seçer:

```
user.assignedPlans -any (assignedPlan.servicePlanId -eq "efb87545-963c-4e0d-99df-69c6916d9eb0" -and assignedPlan.capabilityStatus -eq "Enabled")
```

(GUID tanımlayıcısı Exchange Online (planı 2) hizmet planı tanımlar.)

> [!NOTE]
> Kendisi için tüm kullanıcıları tanımlamak istiyorsanız kullanışlıdır bir Office 365 (veya diğer Microsoft çevrimiçi hizmet) özelliği etkinleştirildi, örneğin bir ilkeleri kümesiyle hedeflemek.

Aşağıdaki ifade ("SCO" hizmet adı tarafından tanımlanan) Intune hizmeti ile ilişkili herhangi bir hizmet planının olan tüm kullanıcıları seçer:
```
user.assignedPlans -any (assignedPlan.service -eq "SCO" -and assignedPlan.capabilityStatus -eq "Enabled")
```

## <a name="use-of-null-values"></a>Null değerleri kullanımı

Bir kuralda boş bir değer belirtmek için "null" ya da $null kullanabilirsiniz. Örnek:
```
   user.mail –ne null
```
eşdeğerdir
```
   user.mail –ne $null
   ```

## <a name="extension-attributes-and-custom-attributes"></a>Uzantı öznitelikleri ve özel öznitelikler
Uzantı öznitelikleri ve özel öznitelikler dinamik üyelik kurallarında desteklenir.

Uzantı öznitelikleri AD şirket içi penceresi sunucusundan eşitlenen ve "ExtensionAttributeX", burada X 1-15'e eşit biçimi uygulayın.
Bir uzantı özniteliği kullanan bir kural, bir örnek olabilir
```
(user.extensionAttribute15 -eq "Marketing")
```
Özel öznitelikler, şirket içi Windows Server AD veya bağlı bir SaaS uygulaması eşitlenen ve biçimi "user.extension_[GUID]\__ [öznitelik]", burada [GUID'dir] öznitelik AAD'de oluşturduğunuz uygulamanın aad'de benzersiz tanımlayıcısı ve [öznitelik] özniteliğin adını oluşturulduğu gibi.
Özel bir öznitelik kullanan bir kural örneğidir
```
user.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  
```
Özel nitelik adında bir kullanıcı sorgulayarak dizininde bulunabilir özniteliği grafik Gezgini'ni kullanarak ve öznitelik adı için arama.

## <a name="direct-reports-rule"></a>"Doğrudan raporları" kuralı
Yöneticinin tüm doğrudan raporları içeren bir grup oluşturabilirsiniz. Yöneticinin doğrudan raporlar gelecekte değiştirdiğinizde, Grup üyeliğini otomatik olarak ayarlanır.

> [!NOTE]
> 1. Kuralın çalışması için emin olun **Manager kimliği** özelliği ayarlanmış doğru kiracınızda kullanıcıları. Bir kullanıcı için geçerli değer denetleyebilirsiniz kendi **Profil sekmesi**.
> 2. Bu kural yalnızca destekler **doğrudan** raporlar. Şu anda iç içe geçmiş bir hiyerarşide, örneğin doğrudan raporlar ve bunların raporları içeren bir grubu bir grup oluşturmak mümkün değil.

**Grubu yapılandırmak için**

1. 1-5 bölümünden adımlarını [Gelişmiş kuralı oluşturmak için](#to-create-the-advanced-rule)seçip bir **üyelik türü** , **dinamik kullanıcı**.
2. Üzerinde **dinamik Üyelik kuralları** dikey penceresinde, aşağıdaki söz dizimini kuralla girin:

    *"{ObectID_of_manager}" için doğrudan raporları*

    Geçerli bir kural örneği:
```
                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863"
```
    where “62e19b97-8b3d-4d4a-a106-4ce66896a863” is the objectID of the manager. The object ID can be found on manager's **Profile tab**.
3. Kural kaydedildikten sonra belirtilen Yöneticisi kimliği değerine sahip tüm kullanıcılar grubuna eklenir.

## <a name="using-attributes-to-create-rules-for-device-objects"></a>Cihaz nesneler için kurallar oluşturmak için öznitelikleri kullanma
Bir gruptaki üyelik için cihaz nesnelerinin seçen bir kural oluşturabilirsiniz. Aşağıdaki cihaz öznitelikleri kullanılabilir.

 Cihaz özniteliği  | Değerler | Örnek
 ----- | ----- | ----------------
 accountEnabled | doğru false | (device.accountEnabled - eq true)
 Görünen adı | Herhangi bir dize değeri |(device.displayName - eq "Ramiz Iphone")
 deviceOSType | Herhangi bir dize değeri | (device.deviceOSType - eq "iPad")- veya (device.deviceOSType - eq "iPhone")
 DeviceOSVersion | Herhangi bir dize değeri | (cihazı. OSVersion - eq "9.1")
 deviceCategory | Geçerli bir aygıt kategori adı | (device.deviceCategory - eq "KCG")
 DeviceManufacturer | Herhangi bir dize değeri | (device.deviceManufacturer - eq "Samsung")
 DeviceModel | Herhangi bir dize değeri | (device.deviceModel - eq "iPad hava")
 deviceOwnership | Kişisel, şirket, bilinmiyor | (device.deviceOwnership - eq "Şirket")
 domainName | Herhangi bir dize değeri | (device.domainName - eq "contoso.com")
 enrollmentProfileName | Apple cihaz kayıt profilinin adı | (device.enrollmentProfileName - eq "DEP iPhone")
 isRooted | doğru false | (device.isRooted - eq true)
 managementType | MDM (için mobil cihazlar)<br>PC (Intune bilgisayar aracı tarafından yönetilen bilgisayarlar için) | (device.managementType - eq "MDM")
 Kuruluş birimi | bir şirket içi Active Directory tarafından ayarlanmış kuruluş biriminin adı ile eşleşen herhangi bir dize değeri | (device.organizationalUnit - eq "ABD PC'ler")
 deviceId | Geçerli bir Azure AD cihaz kimliği | (device.deviceId - eq "d4fe7726-5966-431c-b3b8-cddc8fdb717d")
 objectID | Geçerli bir Azure AD nesne kimliği |  (device.objectId - eq 76ad43c9-32c5-45e8-a272-7b58b58f596d")



## <a name="changing-dynamic-membership-to-static-and-vice-versa"></a>Statik olarak dinamik üyelik değiştirme ve tersi yönde
Bir gruptaki üyelik nasıl yönetilir değiştirmek mümkündür. Grubuna varolan tüm başvuruların hala geçerli olduğundan için aynı grup adı ve kimliği sistemde tutmak istediğiniz gerektiğinde bu faydalıdır; Yeni grup oluşturma bu başvuruları güncelleştirilmesi gerekir.

Bu işlevleri desteklemek için Azure portal güncelleştirme işlemini gerçekleştiriyoruz. Bu arada, kullanabileceğiniz [Klasik Azure portalı](https://manage.windowsazure.com) (yönergeleri izleyin [burada](active-directory-groups-dynamic-membership-azure-portal.md)) ya da aşağıda gösterildiği gibi PowerShell cmdlet'lerini kullanın.

> [!WARNING]
> Varolan bir statik grup için dinamik bir grup değiştirirken, var olan tüm üyeleri gruptan kaldırılacak ve yeni üye eklemek için üyelik kuralını sonra işlenir. Uygulamaları veya kaynaklara erişimi denetlemek için Grup kullandıysanız, üyelik kuralını tam olarak işlenen kadar özgün üyeleri erişimlerini kaybedebilir.
>
> Yeni grup üyeliği beklendiği gibi olduğundan emin olmak için yeni Üyelik Kuralı önceden test etmek için önerilen bir yöntemdir.

**Bir grup üyeliği yönetimini değiştirmek için PowerShell kullanma**

> [!NOTE]
> Cmdlet'leri kullanması gerekecektir dinamik Grup özelliklerini değiştirmek için **önizleme sürümünü** [Azure AD PowerShell sürüm 2](https://docs.microsoft.com/en-us/powershell/azure/active-directory/install-adv2?view=azureadps-2.0). Önizlemesi'nden yükleyebilirsiniz [burada](https://www.powershellgallery.com/packages/AzureADPreview).

Var olan bir grup üyeliği yönetimi geçiş işlevleri bir örneği burada verilmiştir. Dikkatli doğru GroupTypes özelliği işlemek ve orada bulunabilir herhangi bir değeri korumak için alınır dinamik üyelik ilgisiz unutmayın.

```
#The moniker for dynamic groups as used in the GroupTypes property of a group object
$dynamicGroupTypeString = "DynamicMembership"

function ConvertDynamicGroupToStatic
{
    Param([string]$groupId)

    #existing group types
    [System.Collections.ArrayList]$groupTypes = (Get-AzureAdMsGroup -Id $groupId).GroupTypes

    if($groupTypes -eq $null -or !$groupTypes.Contains($dynamicGroupTypeString))
    {
        throw "This group is already a static group. Aborting conversion.";
    }


    #remove the type for dynamic groups, but keep the other type values
    $groupTypes.Remove($dynamicGroupTypeString)

    #modify the group properties to make it a static group: i) change GroupTypes to remove the dynamic type, ii) pause execution of the current rule
    Set-AzureAdMsGroup -Id $groupId -GroupTypes $groupTypes.ToArray() -MembershipRuleProcessingState "Paused"
}

function ConvertStaticGroupToDynamic
{
    Param([string]$groupId, [string]$dynamicMembershipRule)

    #existing group types
    [System.Collections.ArrayList]$groupTypes = (Get-AzureAdMsGroup -Id $groupId).GroupTypes

    if($groupTypes -ne $null -and $groupTypes.Contains($dynamicGroupTypeString))
    {
        throw "This group is already a dynamic group. Aborting conversion.";
    }
    #add the dynamic group type to existing types
    $groupTypes.Add($dynamicGroupTypeString)

    #modify the group properties to make it a static group: i) change GroupTypes to add the dynamic type, ii) start execution of the rule, iii) set the rule
    Set-AzureAdMsGroup -Id $groupId -GroupTypes $groupTypes.ToArray() -MembershipRuleProcessingState "On" -MembershipRule $dynamicMembershipRule
}
```
Bir grup statik yapmak için:
```
ConvertDynamicGroupToStatic "a58913b2-eee4-44f9-beb2-e381c375058f"
```
Bir grup dinamik hale getirmek için:
```
ConvertStaticGroupToDynamic "a58913b2-eee4-44f9-beb2-e381c375058f" "user.displayName -startsWith ""Peter"""
```
## <a name="next-steps"></a>Sonraki adımlar
Bu makaleler Azure Active Directory'de gruplar hakkında ek bilgiler sağlar.

* [Var olan grupları bakın](active-directory-groups-view-azure-portal.md)
* [Yeni bir grup oluşturun ve üye ekleme](active-directory-groups-create-azure-portal.md)
* [Bir grubu ayarlarını yönetme](active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyeliğini yönetme](active-directory-groups-membership-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kurallarını yönet](active-directory-groups-dynamic-membership-azure-portal.md)
