---
title: Öznitelik tabanlı dinamik grup üyeliği kuralları, Azure Active Directory'de | Microsoft Docs
description: Dinamik grup üyeliği de dahil olmak üzere için Gelişmiş kurallar oluşturmak nasıl ifade kural işleçleri ve parametreleri desteklenmiyor.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 07/24/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: e49da237584a48c01e72552abae01da2514da3c1
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39248898"
---
# <a name="create-dynamic-groups-with-attribute-based-membership-in-azure-active-directory"></a>Azure Active Directory üyeliği öznitelik tabanlı dinamik gruplar oluşturun

Azure Active Directory'de (Azure AD), gruplar için dinamik üyelik etkinleştirmek için karmaşık öznitelik tabanlı kurallar oluşturabilirsiniz. Bu makalede, kullanıcı veya cihaz için dinamik Üyelik kuralları oluşturmak için sözdizimi ve öznitelikleri ayrıntıları. Güvenlik gruplarında veya Office 365 gruplarında dinamik üyelik için bir kural ayarlayabilirsiniz.

Bir kullanıcı veya cihaz herhangi bir özniteliği değiştiğinde sistem, herhangi bir grubu ekler veya kaldırır değişiklik tetikleyecek olmadığını görmek için bir dizindeki tüm dinamik Grup kurallarını değerlendirir. Bir kullanıcı veya cihaz bir grup üzerindeki kuralı karşılıyorsa bu grubun bir üyesi eklenir. Bunlar artık kural karşılıyorsanız, bunlar kaldırılır.

> [!NOTE]
> Bu özellik, bir veya daha fazla dinamik grupların üyesi olan her benzersiz bir kullanıcı için bir Azure AD Premium P1 lisansı gerekir. Dinamik grupların üyesi olmak için bunları kullanıcılara lisans atama gerekmez ancak tüm kullanıcıları kapsayacak kiracıda lisansları en az sayıda olmalıdır. Örneğin, kiracınızda içindeki tüm dinamik gruplar 1.000 benzersiz kullanıcıların toplam olsaydı, lisans gereksinimi karşılamak Azure AD Premium P1 için en az 1000 lisans gerekir.
>
> Kullanıcılar veya cihazlar için dinamik bir grup oluşturabilirsiniz, ancak hem kullanıcılar hem de cihazları içeren bir kural oluşturulamıyor.
> 
> Şu anda sahip olan kullanıcı özniteliklerine dayalı bir cihaz grubu oluşturmak mümkün değildir. Cihaz Üyelik kuralları yalnızca Directory'de cihaz nesnelerini hemen özniteliklerini başvurabilir.

## <a name="to-create-an-advanced-rule"></a>Gelişmiş bir kural oluşturmak için

1. Oturum [Azure AD yönetim merkezini](https://aad.portal.azure.com) bir genel yönetici veya kullanıcı hesabı yöneticisi olan bir hesapla.
2. **Kullanıcı ve gruplar**'ı seçin.
3. Seçin **tüm grupları**seçip **yeni grup**.

   ![Yeni grubu eklemek](./media/groups-dynamic-membership/new-group-creation.png)

4. Üzerinde **grubu** dikey penceresinde, bir ad ve yeni grup için bir açıklama girin. Seçin bir **üyelik türü** herhangi birinin **dinamik kullanıcı** veya **dinamik cihaz**kullanıcı veya cihaz için bir kural oluşturmak ve ardındanisteyipistemediğinizibağlıolarak**Dinamik sorgu Ekle**. Gelişmiş bir kural kendiniz yazmak veya basit bir kural oluşturmak için kural Oluşturucusu'nu kullanın. Bu makale, mevcut kullanıcı ve cihaz öznitelikleri ve bunun yanı sıra gelişmiş kurallar örnekleri hakkında daha fazla bilgi içerir.

   ![Dinamik üyelik kuralı ekle](./media/groups-dynamic-membership/add-dynamic-group-rule.png)

5. Bir kural oluşturduktan sonra seçin **Sorgu Ekle** dikey pencerenin alt kısmındaki.
6. Seçin **Oluştur** üzerinde **grubu** grubu oluşturmak için dikey pencere.

> [!TIP]
> Hatalı oluşturulmuş ya da geçerli girdiğiniz kural grubu oluşturma başarısız olur. Sağ üst köşede portalının kural neden işlenemedi açıklama içeren bir bildirim görüntülenir. Bunu nasıl geçerli hale getirmek için kural ayarlamak gerekir dikkatli bir şekilde anlamak için okuyun.

## <a name="status-of-the-dynamic-rule"></a>Dinamik kural durumu

İşlem durumu ve son güncelleştirme tarihi genel bakış sayfasında, dinamik grup üyeliği görebilirsiniz.
  
  ![dinamik grup durumunu görüntüleme](./media/groups-dynamic-membership/group-status.png)


Aşağıdaki durum iletileri için gösterilen **üyelik işleme** durumu:

* **Değerlendirme**: Grup değişikliği alındı ve güncelleştirmeler değerlendirilir.
* **İşleme**: güncelleştirmeleri işleniyor.
* **Güncelleştirme tamamlandı**: işleme tamamlandı ve geçerli tüm güncelleştirmeleri yapıldı.
* **İşleme hatası**: Üyelik Kuralı değerlendirilirken bir hata oluştu ve işlem tamamlanamadı.
* **Güncelleştirme duraklatıldı**: dinamik üyelik kuralı güncelleştirmeleri yönetici tarafından duraklatıldı. MembershipRuleProcessingState "Paused" olarak ayarlanır.

Aşağıdaki durum iletileri için gösterilen **son güncelleştirme üyelik** durumu:

* &lt;**Tarih ve saat**&gt;: en son ne zaman üyelik güncelleştirildi.
* **Devam eden**: güncelleştirmeleri şu anda sürüyor.
* **Bilinmeyen**: son güncelleştirme zamanı alınamıyor. Yeni oluşturulan grubu nedeniyle olabilir.

Belirli bir grup üyeliği kuralı işlenirken bir hata meydana gelirse, üzerindeki bir uyarı gösterilir **genel bakış sayfasında** grubu için. Hayır dinamik üyelik güncelleştirmeleri Kiracı içindeki tüm gruplar için daha sonra 24 saat için işlenebilir varsa bir uyarı üzerindeki gösterilir **tüm grupları**.

![işleme hata iletisi](./media/groups-dynamic-membership/processing-error.png)

## <a name="constructing-the-body-of-an-advanced-rule"></a>Gelişmiş bir kural gövdesi oluşturmak

Gruplara yönelik dinamik üyelikler için oluşturabileceğiniz Gelişmiş bir kural, üç bölümden oluşan ve true veya false sonucu sonuç aslında bir ikili ifadedir. Üç bölümleri şunlardır:

* Sol parametre
* İkili işleç
* Doğru sabiti

Eksiksiz bir Gelişmiş kural şuna benzer: (leftParameter işlecin "RightConstant"), açılış ve kapanış parantezi tüm ikili ifadesi için isteğe bağlı olduğu durumlarda da çift tırnak işareti de yalnızca doğru sabiti için gerekli isteğe bağlıdır Bu bir dizedir ve user.property sol parametresi için sözdizimi aşağıdaki gibidir. Gelişmiş bir kural ikili ifadeler ayırarak birden oluşabilir ve, - veya ve - olmayan mantıksal işleçler.

Doğru şekilde oluşturulmuş bir Gelişmiş kural örnekleri şunlardır:
```
(user.department -eq "Sales") -or (user.department -eq "Marketing")
(user.department -eq "Sales") -and -not (user.jobTitle -contains "SDE")
```
Desteklenen parametreler ve ifade kural işleçleri tam listesi için aşağıdaki bölümlere bakın. Cihaz kuralları için kullanılan öznitelikleri için [cihaz nesneleri için kuralları oluşturmak için öznitelikleri kullanma](#using-attributes-to-create-rules-for-device-objects).

Gelişmiş kural gövdesi toplam uzunluğu 2048 karakterden uzun olamaz.

> [!NOTE]
> Dize ve normal ifade işlemleri büyük/küçük harfe duyarlı değildir. Kullanarak Null denetimlerini gerçekleştirebilir *null* user.department - eq'gibi bir sabit olarak *null*.
> Tırnak işareti içeren bir dize "kullanılarak atlanması ' karakteri, örneğin, user.department - eq \`"Sales".

## <a name="supported-expression-rule-operators"></a>Desteklenen ifade kural işleçleri

Aşağıdaki tablo, tüm desteklenen ifade kural işleçleri ve sözdizimlerinin Gelişmiş kural gövdesi içinde kullanılacak listeler:

| İşleç | Sözdizimi |
| --- | --- |
| Eşit değildir |-ne |
| Eşittir |-eq |
| İle başlamaz |-notStartsWith |
| Şununla başlar |-startsWith |
| İçermez |-notContains |
| Contains |-içerir |
| Eşleşmiyor |-notMatch |
| Eşleşme |-eşleşmesi |
| İçinde | -içinde |
| İçinde değil | -notIn |

## <a name="operator-precedence"></a>İşleç önceliği

Tüm işleçleri, daha düşük öncelikli büyük başına aşağıda listelenmiştir. Aynı satırda işleçleri eşit önceliği vardır:

````
-any -all
-or
-and
-not
-eq -ne -startsWith -notStartsWith -contains -notContains -match –notMatch -in -notIn
````

Tüm işleçleri ile veya kısa çizgi öneki olmadan kullanılabilir. Yalnızca önceliği gereksinimlerinizi karşılamadığında parantez gereklidir.
Örneğin:

```
   user.department –eq "Marketing" –and user.country –eq "US"
```

eşdeğerdir:

```
   (user.department –eq "Marketing") –and (user.country –eq "US")
```

## <a name="using-the--in-and--notin-operators"></a>Kullanarak içinde ve - notIn işleçleri

Bir dizi farklı değerler karşı bir kullanıcı özniteliğinin değeri karşılaştırmak istiyorsanız kullanabileceğiniz içinde veya - notIn işleçleri. İşte bir örnek kullanarak işlecinde:
```
   user.department -In ["50001","50002","50003",“50005”,“50006”,“50007”,“50008”,“50016”,“50020”,“50024”,“50038”,“50039”,“51100”]
```
Kullanımına dikkat edin "[" ve "]", değerler listesinin sonunda ve başındaki. Bu durum, user.department eşittir listede yer alan değerlerden biri değerinin TRUE değerlendirir.


## <a name="query-error-remediation"></a>Sorgu hata düzeltme

Sık karşılaşılan hataların ve bunların nasıl düzeltileceği aşağıdaki tabloda listelenmiştir.

| Sorgu ayrıştırma hatası | Hata kullanımı | Düzeltilmiş kullanımı |
| --- | --- | --- |
| Hata: özniteliği desteklenmiyor. |(user.invalidProperty - eq "Value") |(user.department - eq "value")<br/><br/>Öznitelik olduğundan emin olun üzerinde [desteklenen özellikler listesinde](#supported-properties). |
| Hata: İşleç özniteliği desteklenmiyor. |(user.accountEnabled-true içerir) |(user.accountEnabled - eq true)<br/><br/>Özellik türü için kullanılan işleci desteklenmiyor (Bu örnekte,-içeren kullanılamaz boolean türü). Doğru operators, özellik türü için kullanın. |
| Hata: Sorgu derleme hatası. |1. (user.department - eq "Sales") (user.department - eq "Pazarlama")<br/><br/>2. (user.userPrincipalName-eşleşen "*@domain.ext") |1. İşleç eksik. Kullanın - veya - veya iki doğrulamaları katılın<br/><br/>(user.department - eq "Sales")- veya (user.department - eq "Pazarlama")<br/><br/>2 hata - ile kullanılan normal ifade ile eşleşen<br/><br/>(user.userPrincipalName-eşleşen ". *@domain.ext"), bunun yerine: (user.userPrincipalName-eşleşen "\@domain.ext$")|

## <a name="supported-properties"></a>Desteklenen özellikler

Gelişmiş kuralınızı kullanabileceğiniz tüm kullanıcı özellikleri şunlardır:

### <a name="properties-of-type-boolean"></a>Özelliklerini boolean türü

İzin verilen işleçleri

* -eq
* -ne

| Özellikler | İzin verilen değerler | Kullanım |
| --- | --- | --- |
| accountEnabled |doğru yanlış |user.accountEnabled - eq true |
| dirSyncEnabled |doğru yanlış |user.dirSyncEnabled - eq true |

### <a name="properties-of-type-string"></a>Dize türündeki özellikleri

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
| city |Herhangi bir dize değeri veya *null* |(user.city - eq "value") |
| Ülke |Herhangi bir dize değeri veya *null* |(Resource.country - eq "value") |
| Şirket adı | Herhangi bir dize değeri veya *null* | (user.companyName - eq "value") |
| Bölüm |Herhangi bir dize değeri veya *null* |(user.department - eq "value") |
| displayName |Herhangi bir dize değeri |(user.displayName - eq "value") |
| EmployeeID |Herhangi bir dize değeri |(user.employeeId - eq "value")<br>(user.employeeId - ne *null*) |
| facsimileTelephoneNumber |Herhangi bir dize değeri veya *null* |(user.facsimileTelephoneNumber - eq "value") |
| givenName |Herhangi bir dize değeri veya *null* |(user.givenName - eq "value") |
| İş Unvanı |Herhangi bir dize değeri veya *null* |(user.jobTitle - eq "value") |
| posta |Herhangi bir dize değeri veya *null* (kullanıcının SMTP adresi) |(user.mail - eq "value") |
| mailNickName |Herhangi bir dize değeri (kullanıcının posta diğer) |(user.mailNickName - eq "value") |
| Mobil |Herhangi bir dize değeri veya *null* |(user.mobile - eq "value") |
| objectId |Kullanıcı nesnesinin GUID |(user.objectId - eq "11111111-1111-1111-1111-111111111111") |
| onPremisesSecurityIdentifier | Güvenlik tanımlayıcısı (SID) şirket içinden buluta eşitlenmiş kullanıcılar için şirket içi. |(user.onPremisesSecurityIdentifier - eq "S-1-1-11-1111111111-1111111111-1111111111-1111111") |
| passwordPolicies |Hiçbiri DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration, DisableStrongPassword |(user.passwordPolicies - eq "DisableStrongPassword") |
| physicalDeliveryOfficeName |Herhangi bir dize değeri veya *null* |(user.physicalDeliveryOfficeName - eq "value") |
| posta kodu |Herhangi bir dize değeri veya *null* |(user.postalCode - eq "value") |
| preferredLanguage |ISO 639-1 kodu |(user.preferredLanguage - eq "en-US") |
| sipProxyAddress |Herhangi bir dize değeri veya *null* |(user.sipProxyAddress - eq "value") |
| durum |Herhangi bir dize değeri veya *null* |(user.state - eq "value") |
| streetAddress |Herhangi bir dize değeri veya *null* |(user.streetAddress - eq "value") |
| Soyadı |Herhangi bir dize değeri veya *null* |(user.surname - eq "value") |
| telephoneNumber |Herhangi bir dize değeri veya *null* |(user.telephoneNumber - eq "value") |
| usageLocation |İki yitirmiş ülke kodu |(user.usageLocation - eq "US") |
| userPrincipalName |Herhangi bir dize değeri |(user.userPrincipalName - eq "alias@domain") |
| UserType |üye Konuk *null* |(user.userType - eq "Üye") |

### <a name="properties-of-type-string-collection"></a>Türü dize koleksiyonunun özellikleri

İzin verilen işleçleri

* -içerir
* -notContains

| Özellikler | İzin verilen değerler | Kullanım |
| --- | --- | --- |
| otherMails |Herhangi bir dize değeri |(user.otherMails-içeren "alias@domain") |
| proxyAddresses |SMTP: alias@domain smtp: alias@domain |(user.proxyAddresses-içeren "SMTP: alias@domain") |

## <a name="multi-value-properties"></a>Birden çok değerli özellikler

İzin verilen işleçleri

* -Tüm (ne zaman en az koleksiyondaki bir öğe koşulu ile eşleşen memnun)
* -(tüm koleksiyondaki tüm öğeler koşulu eşleştiğinde memnun)

| Özellikler | Değerler | Kullanım |
| --- | --- | --- |
| assignedPlans |Koleksiyondaki her nesne aşağıdaki dize özellikleri sunar: capabilityStatus, hizmeti, servicePlanId |user.assignedPlans-tüm (assignedPlan.servicePlanId - eq "efb87545-963c-4e0d-99df-69c6916d9eb0"- ve assignedPlan.capabilityStatus - eq "Etkin") |
| proxyAddresses| SMTP: alias@domain smtp: alias@domain | (user.proxyAddresses-tüm (\_ -"contoso" içeren)) |

Birden çok değerli özellikler aynı türde nesne koleksiyonlarıdır. Kullanabileceğiniz - sırasıyla bir koşul koleksiyondaki öğelerin tümünü veya bir uygulamak için herhangi bir ve - tüm işleçler. Örneğin:

assignedPlans kullanıcıya atanan tüm hizmet planları listeleyen birden çok değerli bir özelliktir. İfade, aynı zamanda etkin durumda olan Exchange Online (Plan 2) hizmet planı olan kullanıcılar seçer:

```
user.assignedPlans -any (assignedPlan.servicePlanId -eq "efb87545-963c-4e0d-99df-69c6916d9eb0" -and assignedPlan.capabilityStatus -eq "Enabled")
```

(Exchange Online (Plan 2) hizmet planı GUID tanımlayıcısı tanımlar.)

> [!NOTE]
> Kendisi için tüm kullanıcıları tanımlamak istiyorsanız kullanışlıdır bir Office 365 (veya diğer Microsoft çevrimiçi hizmet) özelliği etkinleştirilmişse, örneğin bir ilkeleri kümesiyle hedefine.

Aşağıdaki ifade (hizmet adı "SCO" tarafından tanımlanır) Intune hizmeti ile ilişkili olan tüm hizmet planına sahip tüm kullanıcılar'ı seçer:
```
user.assignedPlans -any (assignedPlan.service -eq "SCO" -and assignedPlan.capabilityStatus -eq "Enabled")
```

### <a name="using-the-underscore--syntax"></a>Alt çizgi kullanarak (\_) söz dizimi

Alt çizgi (\_) sözdizimi ile eşleşen bir kullanıcı veya cihaz için dinamik bir grup eklemek için birden çok değerli dize koleksiyon özelliklerini belirli bir değerin. İle kullanılan veya - tüm işleçler.

Aşağıda, alt çizgi kullanarak bir örnek (\_) (çalıştığını user.otherMails aynı) user.proxyAddress alarak üyeleri eklemek için bir kural içinde. Bu kural grubuna "contoso" içeren proxy adresine sahip herhangi bir kullanıcı ekler.

```
(user.proxyAddresses -any (_ -contains "contoso"))
```

## <a name="use-of-null-values"></a>Null değerlerin kullanın

Null değeri bir kuralda belirtmek için kullanabileceğiniz *null* değeri. Sözcüğü tırnak kullanmamaya özen *null* -bunu yaparsanız, değişmez değer dize değeri olarak yorumlanır. Not işleci kullanılamaz bir karşılaştırma işleci olarak null. Bunu kullanırsanız, null kullanıp veya $null hata alırsınız. Bunun yerine, - eq veya - ne kullanın. Null değerine başvurmak üzere doğru şekilde aşağıdaki gibidir:
```
   user.mail –ne $null
```

## <a name="extension-attributes-and-custom-attributes"></a>Uzantı öznitelikleri ve özel öznitelikler
Uzantı öznitelikleri ve özel öznitelikleri dinamik Üyelik kuralları desteklenir.

Uzantı öznitelikleri, şirket içi Windows Server AD eşitlenir ve "burada X, 1-15 eşit ExtensionAttributeX" biçiminin gerçekleştirin.
Uzantı özniteliğine kullanan bir kural, bir örnek olabilir

```
(user.extensionAttribute15 -eq "Marketing")
```

Özel öznitelikler, şirket içi Windows Server AD veya bağlı bir SaaS uygulaması ve biçimini eşitlenen "user.extension_[GUID]\__ [Attribute]", burada [GUID] aad'de oluşturduğunuz uygulamayı benzersiz tanımlayıcısı Azure AD'de öznitelik [Attribute] olup öznitelik adı, oluşturulduğu gibi. Özel bir öznitelik kullanan bir kural örneğidir

```
user.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  
```

Özel öznitelik adını dizinde bir kullanıcının sorgulayarak bulunabilir özniteliği Graph Gezgini kullanarak ve öznitelik adı için arama yapılabilir.

## <a name="direct-reports-rule"></a>"Bağlı çalışanları" kuralı
Bir yönetici, tüm bağlı çalışanları içeren bir grup oluşturabilirsiniz. Yöneticinin çalışanların gelecekteki değiştirdiğinizde, Grup üyeliğini otomatik olarak ayarlanır.

> [!NOTE]
> 1. Kuralın çalışması için emin **Yöneticisi kimliği** özelliği düzgün şekilde ayarlandığını kiracınızdaki kullanıcılar. Bir kullanıcı için geçerli değer denetleyebilirsiniz kendi **profili sekmesinde**.
> 2. Bu kural yalnızca destekler **doğrudan** raporlar. Şu anda bir grubu için iç içe hiyerarşisini oluşturmak mümkün değildir; Örneğin, çalışanların ve raporlarının içeren grup.
> 3. Bu kural, diğer gelişmiş kurallar ile birleştirilemez.

**Grubu yapılandırmak için**

1. İzleme bölümünden 1-5 adımlarını [Gelişmiş bir kural oluşturmak için](#to-create-the-advanced-rule)seçip bir **üyelik türü** , **dinamik kullanıcı**.
2. Üzerinde **dinamik Üyelik kuralları** dikey penceresinde, aşağıdaki söz dizimini kuralla girin:

    *Bağlı çalışanları için "{objectID_of_manager}"*

    Geçerli bir kuralı örneği:
```
                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863"
```
    where “62e19b97-8b3d-4d4a-a106-4ce66896a863” is the objectID of the manager. The object ID can be found on manager's **Profile tab**.
3. Kural kaydettikten sonra belirtilen Yöneticisi kimlik değerine sahip tüm kullanıcılar grubuna eklenecektir.

## <a name="using-attributes-to-create-rules-for-device-objects"></a>Cihaz nesneleri için kuralları oluşturmak için öznitelikleri kullanma
Ayrıca, bir gruptaki üyelik için cihaz nesnelerinin seçen bir kural oluşturabilirsiniz. Aşağıdaki cihaz öznitelikleri kullanılabilir.

 Cihaz özniteliği  | Değerler | Örnek
 ----- | ----- | ----------------
 accountEnabled | doğru yanlış | (device.accountEnabled - eq true)
 displayName | herhangi bir dize değeri |(device.displayName - eq "Rob Iphone")
 deviceOSType | herhangi bir dize değeri | (cihaz.cihazostürü - eq "iPad")- veya (cihaz.cihazostürü - eq "iPhone")
 deviceOSVersion | herhangi bir dize değeri | (cihaz. OSVersion - eq "9.1")
 deviceCategory | Geçerli cihaz kategorisi adı | (device.deviceCategory - eq "KCG")
 deviceManufacturer | herhangi bir dize değeri | (device.deviceManufacturer - eq "Samsung")
 deviceModel | herhangi bir dize değeri | (device.deviceModel - eq "iPad hava")
 deviceOwnership | Kişisel, şirket, bilinmeyen | (device.deviceOwnership - eq "Şirket")
 DomainName | herhangi bir dize değeri | (device.domainName - eq "contoso.com")
 enrollmentProfileName | Apple cihaz kayıt profili ya da Windows Autopilot profili adı | (device.enrollmentProfileName - eq "DEP iPhone")
 isRooted | doğru yanlış | (device.isRooted - eq true)
 managementType | MDM (mobil cihazlar için)<br>Bilgisayar (Intune PC aracısı tarafından yönetilen bilgisayarlar için) | (device.managementType - eq "MDM")
 Kuruluş birimi | Şirket içi Active Directory tarafından ayarlayın kuruluş birimi adı eşleşen herhangi bir dize değeri | (device.organizationalUnit - eq "ABD bilgisayarları")
 deviceId | Geçerli bir Azure AD cihaz kimliği | (device.deviceId - eq "d4fe7726-5966-431c-b3b8-cddc8fdb717d")
 objectId | Geçerli bir Azure AD nesne kimliği |  (device.objectId - eq 76ad43c9-32c5-45e8-a272-7b58b58f596d")



## <a name="changing-dynamic-membership-to-static-and-vice-versa"></a>Dinamik üyelik statik olarak değiştirmeyi ve bunun tersi de geçerlidir
Üyelik bir gruba nasıl yönetilir değiştirmek mümkündür. Bu gruba varolan herhangi bir başvuruyu hala geçerli olduğundan sistemde aynı grubu adını ve Kimliğini tutmak istediğinizde kullanışlıdır. Yeni grup oluşturma, bu başvuruları güncelleştirilmesi gerekir.

Bu işlevsellik desteği eklemek için Azure AD yönetim merkezini güncelleştirdik. Şimdi, atanmış üyelik ve bunun tersini Azure AD yönetim merkezini veya aşağıda gösterildiği gibi PowerShell cmdlet'leri aracılığıyla müşteriler var olan grupları dinamik üyelik dönüştürebilirsiniz.

> [!WARNING]
> Varolan bir statik grup için dinamik bir grup değiştirme, tüm mevcut üyelerin gruptan kaldırılır ve yeni üyeler eklemek için üyelik kuralını sonra işlenir. Grubu, uygulamaları veya kaynaklarına erişimi denetlemek için kullanılıyorsa, özgün üyeleri üyelik kuralı tam olarak işlendiği kadar erişimlerini kaybedebilir.
>
> Yeni grup üyeliği beklenildiği gibi gittiğinden emin olmak için önceden yeni üyelik kuralını test etmenizi öneririz.

### <a name="using-azure-ad-admin-center-to-change-membership-management-on-a-group"></a>Değişiklik grubunda üyelik yönetimi için Azure AD yönetim merkezini kullanma 

1. Oturum [Azure AD yönetim merkezini](https://aad.portal.azure.com) bir genel yönetici veya kiracınızdaki kullanıcı hesabı yöneticisi olan bir hesapla.
2. Seçin **grupları**.
3. Gelen **tüm grupları** listesinde, değiştirmek istediğiniz grubu açın.
4. Seçin **özellikleri**.
5. Üzerinde **özellikleri** grubu seç sayfasını bir **üyelik türü** atanan (statik), dinamik kullanıcı ya da, istediği üyelik türüne bağlı olarak dinamik cihaz. Dinamik üyelik için basit bir kuralı seçeneklerini seçin veya gelişmiş bir kural kendiniz yazmak için kural Oluşturucusu'nu kullanabilirsiniz. 

Bir kullanıcı grubu için dinamik üyeliği statik olan bir grubu değiştirme örneği adımlardır. 

1. Üzerinde **özellikleri** seçili grubunuzun seçin sayfasında bir **üyelik türü** , **dinamik kullanıcı**, gruba değişiklikleri açıklayan iletişim Evet'i seçin devam etmek için üyelik. 
  
   ![dinamik kullanıcı üyelik türünü seçin](./media/groups-dynamic-membership/select-group-to-convert.png)
  
2. Seçin **dinamik sorgu Ekle**ve sonra kural sağlayın.
  
   ![Kural girin](./media/groups-dynamic-membership/enter-rule.png)
  
3. Bir kural oluşturduktan sonra seçin **Sorgu Ekle** sayfanın alt kısmındaki.
4. Seçin **Kaydet** üzerinde **özellikleri** yaptığınız değişiklikleri kaydetmek grup için sayfa. **Üyelik türü** grubunu Grup listesinde hemen güncelleştirilir.

> [!TIP]
> Girdiğiniz Gelişmiş bir kural yanlışsa grubu dönüştürme başarısız olabilir. Kural sistem tarafından neden kabul edilemez bir açıklama içeren portalının sağ üst köşede bir bildirim görüntülenir. Bunu nasıl geçerli hale getirmek için bir kural ayarlayabilirsiniz dikkatli bir şekilde anlamak için okuyun.

### <a name="using-powershell-to-change-membership-management-on-a-group"></a>Değişiklik grubunda üyelik yönetimi için PowerShell kullanma

> [!NOTE]
> Cmdlet'leri kullanmak için gerekir, dinamik Grup özelliklerini değiştirmek için **önizleme sürümünü** [Azure AD PowerShell sürüm 2](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0). Önizlemeyi yükleyebilirsiniz [PowerShell Galerisi](https://www.powershellgallery.com/packages/AzureADPreview).

Var olan bir grubuna üyelik Yönetimi geçiş işlevleri örneği aşağıda verilmiştir. Bu örnekte, bakım doğru GroupTypes özelliği yönetmek ve korumak için dinamik üyelik ilgisiz herhangi bir değeri getirilir.

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
Statik bir grup yapmak için:
```
ConvertDynamicGroupToStatic "a58913b2-eee4-44f9-beb2-e381c375058f"
```
Dinamik bir grup yapmak için:
```
ConvertStaticGroupToDynamic "a58913b2-eee4-44f9-beb2-e381c375058f" "user.displayName -startsWith ""Peter"""
```
## <a name="next-steps"></a>Sonraki adımlar
Bu makaleler, Azure Active Directory içinde grupları hakkında ek bilgi sağlar.

* [Var olan grupları görme](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Yeni grup oluşturma ve üye ekleme](../fundamentals/active-directory-groups-create-azure-portal.md)
* [Bir grubun ayarlarını yönetme](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyeliklerini yönetme](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](groups-dynamic-membership.md)
