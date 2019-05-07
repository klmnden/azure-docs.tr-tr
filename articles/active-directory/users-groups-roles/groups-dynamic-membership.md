---
title: Otomatik dinamik grup üyeliği kuralları - Azure Active Directory | Microsoft Docs
description: Gruplar ve bir kural başvuru otomatik olarak doldurmak için üyelik kuralları oluşturmak nasıl.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: eebb68218fd6f9cbda229aae3d9e544e87441562
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65192443"
---
# <a name="dynamic-membership-rules-for-groups-in-azure-active-directory"></a>Azure Active Directory'de gruplar için dinamik Üyelik kuralları

Azure Active Directory'de (Azure AD), gruplar için dinamik üyelik etkinleştirmek için karmaşık öznitelik tabanlı kurallar oluşturabilirsiniz. Dinamik grup üyeliği ekleme ve kaldırma kullanıcılara yönelik yönetim yükünü azaltır. Bu makalede kullanıcı veya cihaz için dinamik Üyelik kuralları oluşturmak için sözdizimi ve özellikleri ayrıntılı olarak açıklanmaktadır. Güvenlik gruplarında veya Office 365 gruplarında dinamik üyelik için bir kural ayarlayabilirsiniz.

Bir kullanıcı veya cihaz herhangi bir özniteliği değiştiğinde sistem, herhangi bir grubu ekler veya kaldırır değişiklik tetikleyecek olmadığını görmek için bir dizindeki tüm dinamik Grup kurallarını değerlendirir. Bir kullanıcı veya cihaz bir grup üzerindeki kuralı karşılıyorsa bu grubun bir üyesi eklenir. Bunlar artık kural karşılıyorsanız, bunlar kaldırılır. El ile ekleyemez veya dinamik bir grup üyesi kaldırın.

* Kullanıcılar veya cihazlar için dinamik bir grup oluşturabilirsiniz, ancak hem kullanıcılar hem de cihazları içeren bir kural oluşturulamıyor.
* Cihaz sahipleri özniteliklerine dayalı bir cihaz grubu oluşturulamıyor. Cihaz Üyelik kuralları yalnızca cihaz özniteliklerine başvurabilir.

> [!NOTE]
> Bu özellik, bir veya daha fazla dinamik grupların üyesi olan her benzersiz bir kullanıcı için bir Azure AD Premium P1 lisansı gerekir. Dinamik grupların üyesi olmak için bunları kullanıcılara lisans atama gerekmez ancak tüm kullanıcıları kapsayacak kiracıda lisansları en az sayıda olmalıdır. Örneğin, kiracınızda içindeki tüm dinamik gruplar 1.000 benzersiz kullanıcıların toplam olsaydı, lisans gereksinimi karşılamak Azure AD Premium P1 için en az 1000 lisans gerekir.
>

## <a name="constructing-the-body-of-a-membership-rule"></a>Gövdesi bir üyelik kuralı oluşturma

Bir kullanıcı veya cihaz grubuyla otomatik olarak dolduran bir üyelik kuralı, bir true veya false sonucu sonuçları ikili bir ifadedir. Üç basit bir kuralı bölümleri şunlardır:

* Özellik
* İşleç
* Değer

Söz dizimi hatalarını önlemek, bir ifade içinde bölümleri sırası önemlidir.

### <a name="rules-with-a-single-expression"></a>Tek bir ifade ile kuralları

Tek bir ifade, bir üyelik kuralının en basit biçimidir ve yalnızca yukarıda açıklanan üç parça içerir. Tek bir ifadeye sahip bir kural şuna benzer: `Property Operator Value`burada özelliği için söz dizimi nesne.özellik adıdır.

Tek bir ifade doğru şekilde oluşturulmuş üyelik kuralıyla bir örneği verilmiştir:

```
user.department -eq "Sales"
```

Parantezler için tek bir ifade isteğe bağlıdır. Üyeliklerini kuralınız gövdesi toplam uzunluğu 2048 karakterden uzun olamaz.

## <a name="supported-properties"></a>Desteklenen özellikler

Üç türde bir üyelik kuralı oluşturmak için kullanılan özellikleri vardır.

* Boolean
* String
* Dize koleksiyonu

Tek bir ifade oluşturmak için kullanabileceğiniz kullanıcı özellikleri aşağıda verilmiştir.

### <a name="properties-of-type-boolean"></a>Özelliklerini boolean türü

| Özellikler | İzin verilen değerler | Kullanım |
| --- | --- | --- |
| accountEnabled |doğru yanlış |user.accountEnabled -eq true |
| dirSyncEnabled |doğru yanlış |user.dirSyncEnabled -eq true |

### <a name="properties-of-type-string"></a>Dize türündeki özellikleri

| Özellikler | İzin verilen değerler | Kullanım |
| --- | --- | --- |
| city |Herhangi bir dize değeri veya *null* |(user.city - eq "value") |
| Ülke |Herhangi bir dize değeri veya *null* |(Resource.country - eq "value") |
| Şirket adı | Herhangi bir dize değeri veya *null* | (user.companyName - eq "value") |
| Bölüm |Herhangi bir dize değeri veya *null* |(user.department - eq "value") |
| displayName |herhangi bir dize değeri |(user.displayName - eq "value") |
| EmployeeID |herhangi bir dize değeri |(user.employeeId - eq "value")<br>(user.employeeId - ne *null*) |
| facsimileTelephoneNumber |Herhangi bir dize değeri veya *null* |(user.facsimileTelephoneNumber - eq "value") |
| givenName |Herhangi bir dize değeri veya *null* |(user.givenName - eq "value") |
| İş Unvanı |Herhangi bir dize değeri veya *null* |(user.jobTitle - eq "value") |
| posta |Herhangi bir dize değeri veya *null* (kullanıcının SMTP adresi) |(user.mail - eq "value") |
| mailNickName |Herhangi bir dize değeri (kullanıcının posta diğer) |(user.mailNickName - eq "value") |
| Mobil |Herhangi bir dize değeri veya *null* |(user.mobile - eq "value") |
| objectId |Kullanıcı nesnesinin GUID |(user.objectId -eq "11111111-1111-1111-1111-111111111111") |
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
| userPrincipalName |herhangi bir dize değeri |(user.userPrincipalName - eq "alias@domain") |
| UserType |üye Konuk *null* |(user.userType - eq "Üye") |

### <a name="properties-of-type-string-collection"></a>Türü dize koleksiyonunun özellikleri

| Özellikler | İzin verilen değerler | Kullanım |
| --- | --- | --- |
| otherMails |herhangi bir dize değeri |(user.otherMails-içeren "alias@domain") |
| proxyAddresses |SMTP: alias@domain smtp: alias@domain |(user.proxyAddresses-içeren "SMTP: alias@domain") |

Cihaz kuralları için kullanılan özellikleri için bkz: [cihazlar için kuralları](#rules-for-devices).

## <a name="supported-operators"></a>Desteklenen işleçleri

Aşağıdaki tabloda, desteklenen tüm işleçler ve tek bir ifade için kendi sözdizimini listeler. İşleçleri ile veya kısa çizgi (-) öneki olmadan kullanılabilir.

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

### <a name="using-the--in-and--notin-operators"></a>Kullanarak içinde ve - notIn işleçleri

Bir dizi farklı değerler karşı bir kullanıcı özniteliğinin değeri karşılaştırmak istiyorsanız kullanabileceğiniz içinde veya - notIn işleçleri. Köşeli ayraç simgelerini kullanın "[" ve "]", değerler listesinin başlayıp.

 Aşağıdaki örnekte, ifade listesindeki değerlerin hiçbirini user.department değeri eşitse true olarak değerlendirilir:

```
   user.department -in ["50001","50002","50003","50005","50006","50007","50008","50016","50020","50024","50038","50039","51100"]
```


### <a name="using-the--match-operator"></a>Kullanarak eşleştirme işleci 
**-Eşleşen** işleci, herhangi bir normal ifade eşleştirmesi için kullanılır. Örnekler:

```
user.displayName -match "Da.*"   
```
True olarak da, Dav, David değerlendirmek, aDa yanlış olarak değerlendirilir.

```
user.displayName -match ".*vid"
```
David true olarak değerlendirilen, bu Da yanlış olarak değerlendirilir.

## <a name="supported-values"></a>Desteklenen değerler

Bir ifadede kullanılan değerleri dahil olmak üzere birkaç türde oluşabilir:

* Dizeler
* Boole: true, false
* Sayılar
* Diziler – sayı dizisi, dize dizisi

Bir ifade içinde bir değer belirtmek için doğru sözdizimi hataları önlemek önemlidir. Bazı söz dizimi ipuçları şunlardır:

* Değer bir dize olmadığı sürece tırnak isteğe bağlıdır.
* Dize ve normal ifade işlemleri büyük/küçük harfe duyarlı değildir.
* Bir dize değeri çift tırnak işareti içeriyorsa, her iki kullanarak kaçış karakterli tırnak işaretleri \` örneğin user.department - eq karakter \`"Sales\`" değeri "Sales" olduğunda doğru sözdizimi şöyledir.
* Null bir değer olarak, örneğin, kullanarak Null denetimlerini gerçekleştirebilir `user.department -eq null`.

### <a name="use-of-null-values"></a>Null değerlerin kullanın

Null değeri bir kuralda belirtmek için kullanabileceğiniz *null* değeri. 

* -Eq veya kullanın - ne karşılaştırılırken *null* değeri bir ifade.
* Sözcüğü tırnak kullanın *null* yalnızca bir değişmez dize değeri yorumlanması için istiyorsanız.
* Not işleci kullanılamaz bir karşılaştırma işleci olarak null. Bunu kullanırsanız, null kullanıp veya $null hata alırsınız.

Null değerine başvurmak üzere doğru şekilde aşağıdaki gibidir:

```
   user.mail –ne null
```

## <a name="rules-with-multiple-expressions"></a>Birden çok ifadelerle kuralları

Bir grup üyeliği kuralı tarafından bağlanmış birden fazla tek ifade oluşabilir ve, - veya ve - olmayan mantıksal işleçler. Mantıksal işleçler de birlikte kullanılabilir. 

Birden fazla ifade ile doğru şekilde oluşturulmuş Üyelik kuralları örnekleri şunlardır:

```
(user.department -eq "Sales") -or (user.department -eq "Marketing")
(user.department -eq "Sales") -and -not (user.jobTitle -contains "SDE")
```

### <a name="operator-precedence"></a>İşleç önceliği

Tüm işleçler, öncelik en yüksekten en düşüğe sırayla aşağıda listelenmiştir. Aynı satırda işleçleri eşit önceliği vardır:

```
-eq -ne -startsWith -notStartsWith -contains -notContains -match –notMatch -in -notIn
-not
-and
-or
-any -all
```

İşleç önceliği örneği kullanıcı için iki ifadeden değerlendirilen burada verilmiştir:

```
   user.department –eq "Marketing" –and user.country –eq "US"
```

Yalnızca önceliği gereksinimlerinizi karşılamadığında parantez gereklidir. İlk değerlendirilecek departmanı istiyorsanız, örneğin, aşağıdaki parantez sırasını belirlemek için nasıl kullanılabileceğini gösterir:

```
   user.country –eq "US" –and (user.department –eq "Marketing" –or user.department –eq "Sales")
```

## <a name="rules-with-complex-expressions"></a>Karmaşık ifadeleri ile kuralları

Bir üyelik kuralı burada daha karmaşık formlarında özelliklerini, işleçlerini ve değerlerini almak karmaşık ifadeleri oluşabilir. Aşağıdakilerden herhangi biri doğru olduğunda karmaşık ifadeleri olarak değerlendirilir:

* Özellik değerlerinin koleksiyonunu oluşur; Özellikle, birden çok değerli özellikler
* İfadeleri kullanma tüm ve - tüm işleçleri
* İfadenin değerini kendi bir veya daha fazla ifade olabilir

## <a name="multi-value-properties"></a>Birden çok değerli özellikler

Birden çok değerli özellikler aynı türde nesne koleksiyonlarıdır. Kullanarak Üyelik kuralları oluşturmak için kullanılabilir tüm ve - tüm mantıksal işleçler.

| Özellikler | Değerler | Kullanım |
| --- | --- | --- |
| assignedPlans | Koleksiyondaki her nesne aşağıdaki dize özellikleri sunar: capabilityStatus, hizmeti, servicePlanId |user.assignedPlans-tüm (assignedPlan.servicePlanId - eq "efb87545-963c-4e0d-99df-69c6916d9eb0"- ve assignedPlan.capabilityStatus - eq "Etkin") |
| proxyAddresses| SMTP: alias@domain smtp: alias@domain | (user.proxyAddresses-tüm (\_ -"contoso" içeren)) |

### <a name="using-the--any-and--all-operators"></a>Kullanarak tüm ve - tüm işleçleri

Kullanabileceğiniz - sırasıyla bir koşul koleksiyondaki öğelerin tümünü veya bir uygulamak için herhangi bir ve - tüm işleçler.

* -Tüm (ne zaman en az koleksiyondaki bir öğe koşulu ile eşleşen memnun)
* -(tüm koleksiyondaki tüm öğeler koşulu eşleştiğinde memnun)

#### <a name="example-1"></a>Örnek 1

assignedPlans kullanıcıya atanan tüm hizmet planları listeleyen birden çok değerli bir özelliktir. Aşağıdaki ifade, aynı zamanda etkin durumda olan Exchange Online (Plan 2) hizmet planına (bir GUID değeri) sahip kullanıcılar seçer:

```
user.assignedPlans -any (assignedPlan.servicePlanId -eq "efb87545-963c-4e0d-99df-69c6916d9eb0" -and assignedPlan.capabilityStatus -eq "Enabled")
```

Bunun gibi bir kural, tüm kullanıcılar için Grup için kullanılabilir bir Office 365 (veya diğer Microsoft çevrimiçi hizmet) özelliği etkinleştirilir. Ardından, bir ilke kümesi ile grubuna uygulayabilirsiniz.

#### <a name="example-2"></a>Örnek 2

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

## <a name="other-properties-and-common-rules"></a>Diğer özellikleri ve genel kuralları

### <a name="create-a-direct-reports-rule"></a>"Bağlı çalışanları" kuralı oluşturma

Bir yönetici, tüm bağlı çalışanları içeren bir grup oluşturabilirsiniz. Yöneticinin çalışanların gelecekteki değiştirdiğinizde, Grup üyeliğini otomatik olarak ayarlanır.

Bağlı çalışanları kuralı aşağıdaki sözdizimi kullanılarak oluşturulur:

```
Direct Reports for "{objectID_of_manager}"
```

"62e19b97-8b3d-4d4a-a106-4ce66896a863" objectID Yöneticisi olduğu geçerli bir kural, bir örnek aşağıda verilmiştir:

```
Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863"
```

Aşağıdaki ipuçları, kuralın düzgün şekilde kullanmanıza yardımcı olabilir.

* **Yöneticisi kimliği** Yöneticisi nesne kimliği. Yöneticinin içinde bulunabilir **profili**.
* İş kuralı için emin **Manager** özelliği düzgün şekilde ayarlandığını kiracınızdaki kullanıcılar için. Kullanıcının geçerli değerinin denetleyebilirsiniz **profili**.
* Bu kural yalnızca doğrudan Rapor Yöneticisi'nin destekler. Diğer bir deyişle, yöneticinin bağlı çalışanları ile bir grubu oluşturulamıyor. *ve* raporlarının.
* Bu kural, herhangi bir üyelik kuralları ile birleştirilemez.

### <a name="create-an-all-users-rule"></a>"Tüm kullanıcılar" kuralı oluşturma

Bir üyelik kuralı kullanılarak bir kiracı içindeki tüm kullanıcıları içeren bir grup oluşturabilirsiniz. Kullanıcılar eklendiğinde veya kiracıdan gelecekte kaldırıldığında, Grup üyeliğini otomatik olarak ayarlanır.

"Tüm kullanıcılar" kuralı, - ne işleci ve null değerini kullanarak tek bir ifade kullanılarak oluşturulur. Bu kural, B2B konuk kullanıcıların yanı sıra üye kullanıcıları gruba ekler.

```
user.objectid -ne null
```

### <a name="create-an-all-devices-rule"></a>"Tüm cihazlar" kuralı oluşturma

Bir üyelik kuralı kullanılarak bir kiracı içindeki tüm cihazları içeren bir grup oluşturabilirsiniz. Aygıtlar eklendiğinde veya kiracıdan gelecekte kaldırıldığında, Grup üyeliğini otomatik olarak ayarlanır.

"Tüm cihazlar" kuralı, - ne işleci ve null değerini kullanarak tek bir ifade kullanılarak oluşturulur:

```
device.objectid -ne null
```

## <a name="extension-properties-and-custom-extension-properties"></a>Uzantı özellikleri ve özel uzantı özellikleri

Uzantı öznitelikleri ve özel uzantı özellikleri, dinamik Üyelik kuralları dize özellikleri olarak desteklenir. Uzantı öznitelikleri, şirket içi Windows Server AD eşitlenir ve "burada X, 1-15 eşit ExtensionAttributeX" biçiminin gerçekleştirin. Bir özellik olarak uzantısı özniteliği kullanan bir kural, bir örnek aşağıda verilmiştir:

```
(user.extensionAttribute15 -eq "Marketing")
```

Özel uzantı özellikleri şirket içi Windows Server AD veya bağlı bir SaaS uygulama eşitlenir ve biçimi olan `user.extension_[GUID]__[Attribute]`burada:

* [GUID], Özelliği Azure AD'de oluşturulan uygulama için Azure AD'de benzersiz tanımlayıcısı değil.
* [Attribute] oluşturulduğu özelliğin adını aynıdır

Özel uzantı özelliği kullanan bir kural örneğidir:

```
user.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber -eq "123"
```

Özel özellik adını bir kullanıcı sorgulayarak dizininde bulunabilir özelliği Graph Gezgini kullanarak ve arama özellik adı. Ayrıca, artık seçebilirsiniz **özel uzantı özelliklerini alma** bağlantı benzersiz uygulama Kimliğini girin ve bir dinamik üyelik kuralı oluşturulurken kullanılacak özel uzantı özelliklerinin tam listesini almak için dinamik kullanıcı grubu kuralı Oluşturucusu. Bu liste, bu uygulama için tüm yeni özel uzantı özellikleri almak için aynı zamanda yenilenebilir.

## <a name="rules-for-devices"></a>Cihazlar için kuralları

Ayrıca, bir gruptaki üyelik için cihaz nesnelerinin seçen bir kural oluşturabilirsiniz. Grup üyeleri hem kullanıcılar hem de cihazlara sahip olamaz. **OrganizationalUnit** özniteliği artık listelenir ve kullanılmamalıdır. Bu dize, Intune tarafından belirli durumlarda ayarlanır ancak hiçbir cihaz Bu öznitelikte göre gruplara eklenir, böylece Azure AD tarafından tanınmıyor.

Aşağıdaki cihaz öznitelikleri kullanılabilir.

 Cihaz özniteliği  | Değerler | Örnek
 ----- | ----- | ----------------
 accountEnabled | doğru yanlış | (device.accountEnabled - eq true)
 displayName | herhangi bir dize değeri |(device.displayName -eq "Rob iPhone")
 deviceOSType | herhangi bir dize değeri | (cihaz.cihazostürü - eq "iPad")- veya (cihaz.cihazostürü - eq "iPhone")<br>(cihaz.cihazostürü-"AndroidEnterprise" içerir)<br>(cihaz.cihazostürü - eq "AndroidForWork")
 deviceOSVersion | herhangi bir dize değeri | (device.deviceOSVersion - eq "9.1")
 deviceCategory | Geçerli cihaz kategorisi adı | (device.deviceCategory - eq "KCG")
 deviceManufacturer | herhangi bir dize değeri | (device.deviceManufacturer -eq "Samsung")
 deviceModel | herhangi bir dize değeri | (device.deviceModel - eq "iPad hava")
 deviceOwnership | Kişisel, şirket, bilinmeyen | (device.deviceOwnership - eq "Şirket")
 DomainName | herhangi bir dize değeri | (device.domainName - eq "contoso.com")
 enrollmentProfileName | Apple cihaz kayıt profili ya da Windows Autopilot profili adı | (device.enrollmentProfileName - eq "DEP iPhone")
 isRooted | doğru yanlış | (device.isRooted - eq true)
 managementType | MDM (mobil cihazlar için)<br>Bilgisayar (Intune PC aracısı tarafından yönetilen bilgisayarlar için) | (device.managementType - eq "MDM")
 deviceId | Geçerli bir Azure AD cihaz kimliği | (device.deviceId - eq "d4fe7726-5966-431c-b3b8-cddc8fdb717d")
 objectId | Geçerli bir Azure AD nesne kimliği |  (device.objectId - eq 76ad43c9-32c5-45e8-a272-7b58b58f596d")
 systemLabels | Modern iş yeri cihazları etiketleme için Intune cihaz özelliği eşleşen herhangi bir dize | (device.systemLabels-"M365Managed" içerir)

> [!Note]  
> Dinamik gruplar için cihazları oluştururken deviceOwnership için değer "Şirket" eşit ayarlamanız gerekir. Intune cihaz sahipliğini şirket bunun yerine gösterilir. Başvurmak [OwnerTypes](https://docs.microsoft.com/intune/reports-ref-devices#ownertypes) daha fazla ayrıntı için. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makaleler, Azure Active Directory içinde grupları hakkında ek bilgi sağlar.

* [Var olan grupları görme](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Yeni grup oluşturma ve üye ekleme](../fundamentals/active-directory-groups-create-azure-portal.md)
* [Bir grubun ayarlarını yönetme](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyeliklerini yönetme](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](groups-dynamic-membership.md)
