---
title: Azure Active Directory'de öznitelik eşlemeleri için ifadeler yazma | Microsoft Docs
description: İfade eşlemeleri otomatik Azure Active Directory'de SaaS uygulama nesnelerin sağlama sırasında öznitelik değerleri kabul edilebilir bir biçime dönüştürmek için kullanmayı öğrenin.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/21/2019
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 12b75c2df7d11b0e90c5dccc3bc2aae4e0fb0c1e
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204480"
---
# <a name="writing-expressions-for-attribute-mappings-in-azure-active-directory"></a>Azure Active Directory'de öznitelik eşlemeleri için ifadeler yazma
Bir SaaS uygulaması için sağlama yapılandırdığınızda, belirtebilmeniz için öznitelik eşlemelerini türdeki bir ifade eşleme biridir. Bu, kullanıcılarınızın verileri fazla SaaS uygulaması için kabul edilebilir biçimlere dönüştürme olanak tanıyan bir betik gibi ifade yazmanız gerekir.

## <a name="syntax-overview"></a>Söz dizimi genel bakış
Öznitelik eşlemeleri için ifadeler sözdizimi Applications (VBA) işlevleri için Visual Basic reminiscent aşağıdaki gibidir.

* Tüm ifade İşlevler, bağımsız değişkenleri parantez içinde bir adından oluşur bakımından tanımlanmış olması gerekir: <br>
  *FunctionName (`<<argument 1>>`,`<<argument N>>`)*
* İçindeki diğer işlevleri iç içe. Örneğin: <br> *FunctionOne(FunctionTwo(`<<argument1>>`))*
* İşlevlere üç farklı türde bağımsız değişkenler geçirebilirsiniz:
  
  1. Öznitelik, köşeli ayraçlar içine alınmalıdır. Örneğin: [attributeName]
  2. Dize sabitleri çift tırnak içine alınmalıdır. Örneğin: "ABD"
  3. Diğer işlevler. Örneğin: FunctionOne (`<<argument1>>`, FunctionTwo (`<<argument2>>`))
* Dize sabitleri için bir ters eğik çizgi (\) veya tırnak işareti (") dizedeki gerekiyorsa, eğik çizgi (\) simgesiyle kaçınılmalıdır. Örneğin: "Şirket adı: \\"Contoso\\" "

## <a name="list-of-functions"></a>İşlevlerin listesi
[Append](#append) &nbsp; &nbsp; &nbsp; &nbsp; [FormatDateTime](#formatdatetime) &nbsp; &nbsp; &nbsp; &nbsp; [katılın](#join) &nbsp; &nbsp; &nbsp; &nbsp; [Mid](#mid) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; [NormalizeDiacritics](#normalizediacritics) [değil](#not) &nbsp; &nbsp; &nbsp; &nbsp; [değiştirin](#replace) &nbsp; &nbsp; &nbsp; &nbsp; [SelectUniqueValue](#selectuniquevalue) &nbsp; &nbsp; &nbsp; &nbsp; [SingleAppRoleAssignment](#singleapproleassignment) &nbsp; &nbsp; &nbsp; &nbsp; [Bölünmüş](#split) &nbsp; &nbsp; &nbsp; &nbsp; [ StripSpaces](#stripspaces) &nbsp; &nbsp; &nbsp; &nbsp; [anahtar](#switch) &nbsp; &nbsp; &nbsp; &nbsp; [ToLower](#tolower) &nbsp; &nbsp; &nbsp; &nbsp; [ToUpper](#toupper)

---
### <a name="append"></a>Ekle
**İşlev:**<br> Append(Source, Suffix)

**Açıklama:**<br> Kaynak dize değerini alır ve soneki, sonuna ekler.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gerekli |Dize |Genellikle kaynak nesneden özniteliğin adı. |
| **suffix** |Gerekli |Dize |Kaynak değerin sonuna istediğiniz dize. |

---
### <a name="formatdatetime"></a>formatDateTime
**İşlev:**<br> FormatDateTime (kaynak, inputFormat, outputFormat)

**Açıklama:**<br> Bir tarih dizesini bir biçimden alır ve farklı bir biçime dönüştürür.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gerekli |Dize |Genellikle kaynak nesneden özniteliğin adı. |
| **inputFormat** |Gerekli |Dize |Kaynak değeri beklenen biçimi. Desteklenen biçimler için bkz: [ https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx ](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx). |
| **outputFormat** |Gerekli |Dize |Çıkış tarih biçimi. |

---
### <a name="join"></a>Birleştir
**İşlev:**<br> Birleştirme (ayırıcı, kaynak1, kaynak2,...)

**Açıklama:**<br> Birden çok birleştirebilirsiniz dışında join() Append() için benzer **kaynak** dize değerleri tek bir dize olarak ve her bir değeri ile ayrılmış bir **ayırıcı** dize.

Kaynak değerlerden biri çok değerli öznitelik ise, ardından bu öznitelik her değer birlikte ayırıcı değeri tarafından ayrılmış katılır.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Ayırıcı** |Gerekli |Dize |Kaynak değerler bir dizeye birleştirilmiş, ayırmak için kullanılan dize. Olabilir "" ayırıcı gerekiyorsa. |
| **... kaynak1 kaynakN** |Değişken-sayısı gerekli |Dize |Değerleri birlikte birleştirilecek dize. |

---
### <a name="mid"></a>Orta
**İşlev:**<br> Mid (kaynak, başlangıç, uzunluk)

**Açıklama:**<br> Kaynak değerin bir alt dizeyi döndürür. Bir alt dizenin yalnızca bazı kaynak dizeden karakterleri içeren bir dizedir.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gerekli |Dize |Genellikle özniteliğin adı. |
| **start** |Gerekli |integer |İçinde dizin **kaynak** alt dizenin başladığı dize. Dizedeki ilk karakter dizini 1 gerekir, ikinci karakter 2 dizine sahip ve benzeri. |
| **Uzunluğu** |Gerekli |integer |Alt dizenin uzunluğu. Uzunluğu dışında sona ererse **kaynak** dize işlevi alt dizeyi döndürecektir **Başlat** sonuna kadar dizin **kaynak** dize. |

---
### <a name="normalizediacritics"></a>NormalizeDiacritics
**İşlev:**<br> NormalizeDiacritics(source)

**Açıklama:**<br> Bir dize bağımsız değişkeni gerektirir. Dizeyi döndürür, ancak tüm Aksan karakteriyle değiştirildiğini aksanlı eşdeğer karakterlerle. Genellikle kullanıcı asıl adı, SAM hesap adlarını ve e-posta adresleri gibi çeşitli kullanıcı tanımlayıcılarını kullanılabilir yasal değerlerine adlarını ve soyadlarını aksanlı karakterleri (vurgu işaretleri) içeren dönüştürmek için kullanılır.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gerekli |String | Genellikle bir ad veya son name özniteliği. |

---
### <a name="not"></a>değil
**İşlev:**<br> Not(Source)

**Açıklama:**<br> Boole değeri çevirir **kaynak**. Varsa **kaynak** değeridir "*True*", döndürür "*False*". Aksi halde döndürür "*True*".

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gerekli |Boole dizesi |Beklenen **kaynak** değerler "True" veya "False". |

---
### <a name="replace"></a>Değiştir
**İşlev:**<br> Değiştir (kaynak, oldValue, regexPattern, regexGroupName, replacementValue, replacementAttributeName, şablon)

**Açıklama:**<br>
Bir dize içindeki değerleri değiştirir. Sağlanan parametreler bağlı olarak farklı şekilde çalışır:

* Zaman **oldValue** ve **replacementValue** sağlanır:
  
  * İle replacementValue oldValue kaynaktaki tüm oluşumlarını değiştirir
* Zaman **oldValue** ve **şablon** sağlanır:
  
  * Tüm oluşumlarını değiştirir **oldValue** içinde **şablon** ile **kaynak** değeri
* Zaman **regexPattern**, **regexGroupName**, **replacementValue** sağlanır:
  
  * OldValueRegexPattern kaynak dizede replacementValue ile eşleşen tüm değerleri değiştirir
* Zaman **regexPattern**, **regexGroupName**, **replacementPropertyName** sağlanır:
  
  * Varsa **kaynak** değeri **kaynak** döndürülür
  * Varsa **kaynak** bir değere sahip, kullandığı **regexPattern** ve **regexGroupName** özelliğiyle değiştirme değeri ayıklamak için **replacementPropertyName** . Sonuç olarak değiştirme değeri döndürülür

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gerekli |Dize |Genellikle kaynak nesneden özniteliğin adı. |
| **oldValue** |İsteğe bağlı |Dize |İçinde değiştirilecek değer **kaynak** veya **şablon**. |
| **regexPattern** |İsteğe bağlı |Dize |Regex deseni olarak değiştirilecek değeri **kaynak**. Veya replacementPropertyName kullanıldığında değiştirme özelliği değerini ayıklamak için desen. |
| **regexGroupName** |İsteğe bağlı |Dize |Grup içinde adını **regexPattern**. Yalnızca replacementPropertyName kullanıldığında Biz bu grubun değeri olarak değiştirme özelliğinden replacementValue ayıklayacaksınız. |
| **replacementValue** |İsteğe bağlı |Dize |Eski değiştirmek için yeni değeri. |
| **replacementAttributeName** |İsteğe bağlı |Dize |Kaynak yok değerine sahip olduğunda değiştirme değeri için kullanılacak özniteliğin adı. |
| **Şablonu** |İsteğe bağlı |Dize |Zaman **şablon** değeri sağlanır, biz arar **oldValue** şablonu içinde ve kaynak değeriyle değiştirin. |

---
### <a name="selectuniquevalue"></a>SelectUniqueValue
**İşlev:**<br> SelectUniqueValue (uniqueValueRule1, uniqueValueRule2, uniqueValueRule3,...)

**Açıklama:**<br> En az benzersiz değer oluşturma kuralları ifadeleri kullanılarak tanımlanmış olan iki bağımsız değişken gerektirir. İşlev, her kural değerlendirir ve ardından hedef uygulama/dizinindeki benzersizlik için oluşturulan değeri denetler. Bir döndürülen ilk benzersiz değeri bulundu. Tüm değerleri hedefte zaten varsa, giriş kalacakları ve nedeni denetim günlüklerinde günlüğe. Sağlanan bağımsız değişken sayısı için üst sınır yoktur.

> [!NOTE]
>1. Bu üst düzey bir işlev, iç içe olamaz.
>2. Bu işlev yalnızca girişi oluşturma için kullanılmak üzere tasarlanmıştır. Bir öznitelik ile kullanırken, ayarlayın **uygulamak eşleme** özelliğini **yalnızca nesne oluşturma sırasında**.


**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **... uniqueValueRule1 uniqueValueRuleN** |En az 2 gerekli, en üst sınır. |String | Değerlendirmek için benzersiz bir değer oluşturma kuralları listesi. |


---
### <a name="singleapproleassignment"></a>SingleAppRoleAssignment
**İşlev:**<br> SingleAppRoleAssignment([appRoleAssignments])

**Açıklama:**<br> Belirli bir uygulamanın bir kullanıcıya atanan tüm appRoleAssignments listesinden tek bir appRoleAssignment döndürür. Bu işlev, appRoleAssignments nesne tek bir rol adı dizeye dönüştürme için gereklidir. Tek appRoleAssignment emin olmak için en iyi olduğuna dikkat edin, aynı anda tek bir kullanıcıya atanır ve birden çok rol döndürülen rol dize atanmışsa tahmin edilebilir olmayabilir. 

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **[appRoleAssignments]** |Gerekli |Dize |**[appRoleAssignments]**  nesne. |

---
### <a name="split"></a>Böl
**İşlev:**<br> Böl (kaynağı, sınırlayıcı)

**Açıklama:**<br> Belirtilen sınırlayıcı karakteri kullanarak bir mulit değerli diziye, bir dizeyi böler.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gerekli |Dize |**Kaynak** güncelleştirmek için değer. |
| **Sınırlayıcı** |Gerekli |String |Dizeyi bölmek için kullanılan karakteri belirtir (örnek: ",") |

---
### <a name="stripspaces"></a>StripSpaces
**İşlev:**<br> StripSpaces(source)

**Açıklama:**<br> Tüm alan kaldırır ("") karakter kaynak dizesi.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gerekli |Dize |**Kaynak** güncelleştirmek için değer. |

---
### <a name="switch"></a>Anahtar
**İşlev:**<br> Anahtar (kaynak, defaultValue, key1, value1, key2, value2,...)

**Açıklama:**<br> Zaman **kaynak** değeri eşleşen bir **anahtarı**, döndürür **değer** söz konusu **anahtar**. Varsa **kaynak** değeri döndürür bir anahtarı eşleşmiyor **defaultValue**.  **Anahtar** ve **değer** parametreleri her zaman çiftler halinde gelmelidir. İşlev her zaman bir çift sayı parametre bekliyor.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gerekli |Dize |**Kaynak** güncelleştirmek için değer. |
| **defaultValue** |İsteğe bağlı |Dize |Tüm anahtarları kaynak eşleşmediğinde kullanılacak varsayılan değeri. Boş bir dize olabilir (""). |
| **anahtar** |Gerekli |Dize |**Anahtar** Karşılaştırılacak **kaynak** ile değeri. |
| **value** |Gerekli |Dize |İçin değiştirme değeri **kaynak** anahtarıyla eşleşen. |

---
### <a name="tolower"></a>toLower
**İşlev:**<br> ToLower (kaynak, kültür)

**Açıklama:**<br> Alan bir *kaynak* dize değeri ve kültür kullanarak küçük harfe kurallar, belirtilen dönüştürür. Yoksa hiçbir *kültür* bilgisi belirtilen sabit kültürü kullanır.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gerekli |Dize |Genellikle kaynak nesneden özniteliğin adı |
| **Kültür** |İsteğe bağlı |String |RFC 4646 üzerinde temel kültür adı biçimi *languagecode2-ülke/regioncode2*burada *languagecode2* iki harfli dil kodu ve *ülke/regioncode2*iki harfli alt koddur. Ja-JP Japonca (Japonya) ve en-US için İngilizce (ABD) için verilebilir. Burada iki harfli dil kodunu kullanılabilir olmadığı durumlarda, ISO 639-2 ' türetilmiş bir üç harfli kod kullanılır.|

---
### <a name="toupper"></a>toUpper
**İşlev:**<br> ToUpper (kaynak, kültür)

**Açıklama:**<br> Alan bir *kaynak* dize değeri ve kültürü kullanarak büyük için kuralları, belirtilen dönüştürür. Yoksa hiçbir *kültür* bilgisi belirtilen sabit kültürü kullanır.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gerekli |Dize |Genellikle kaynak nesneden özniteliğin adı. |
| **Kültür** |İsteğe bağlı |String |RFC 4646 üzerinde temel kültür adı biçimi *languagecode2-ülke/regioncode2*burada *languagecode2* iki harfli dil kodu ve *ülke/regioncode2*iki harfli alt koddur. Ja-JP Japonca (Japonya) ve en-US için İngilizce (ABD) için verilebilir. Burada iki harfli dil kodunu kullanılabilir olmadığı durumlarda, ISO 639-2 ' türetilmiş bir üç harfli kod kullanılır.|

## <a name="examples"></a>Örnekler
### <a name="strip-known-domain-name"></a>Şerit bilinen etki alanı adı
Bir kullanıcı adı almak için bir kullanıcının e-posta bilinen etki alanı adından çıkarmanız gerekir. <br>
Örneğin, "contoso.com" etki alanı varsa, aşağıdaki ifade kullanabilirsiniz:

**İfade:** <br>
`Replace([mail], "@contoso.com", , ,"", ,)`

**Giriş / Çıkış örneği:** <br>

* **Giriş** (posta): "john.doe@contoso.com"
* **Çıkış**: "john.doe"

### <a name="append-constant-suffix-to-user-name"></a>Kullanıcı adı için sabit bir sonek ekleme
Salesforce korumalı alan kullanıyorsanız, ek bir sonek eşitlemeden önce tüm kullanıcı adları eklemek gerekebilir.

**İfade:** <br>
`Append([userPrincipalName], ".test")`

**Örnek giriş/çıkış:** <br>

* **Giriş**: (userPrincipalName): "John.Doe@contoso.com"
* **ÇIKIŞ**: "John.Doe@contoso.com.test"

### <a name="generate-user-alias-by-concatenating-parts-of-first-and-last-name"></a>Birleştirme bölümleri adının ilk ve son tarafından kullanıcı diğer adı oluştur
Bir kullanıcı diğer kullanıcının adını, ilk 3 harf ve kullanıcının soyadını ilk 5 harfini yararlanarak oluşturmanız gerekiyor.

**İfade:** <br>
`Append(Mid([givenName], 1, 3), Mid([surname], 1, 5))`

**Örnek giriş/çıkış:** <br>

* **Giriş** (givenName): "John"
* **Giriş** (Soyadı): "Doe"
* **ÇIKIŞ**:  "JohDoe"

### <a name="remove-diacritics-from-a-string"></a>Bir dizeden Aksanları Kaldır
Vurgu işaretlerinin içermeyen eşdeğer karakterlerle vurgu işaretleri içeren karakter değiştirmeniz gerekir.

**İfade:** <br>
NormalizeDiacritics([givenName])

**Örnek giriş/çıkış:** <br>

* **Giriş** (givenName): "Zoë"
* **ÇIKIŞ**:  "Zoe"

### <a name="split-a-string-into-a-multi-valued-array"></a>Birden çok değerli bir diziye bir dizeyi Böl
Dizelerin virgülle ayrılmış bir listesini almak ve bunları bir Salesforce'nın PermissionSets öznitelik gibi birden çok değerli öznitelik takılı bir dizi bölün gerekir. Bu örnekte, Azure AD'de extensionAttribute5 içinde izin kümeleri listesini doldurulmadı.

**İfade:** <br>
Böl ([extensionAttribute5] ",")

**Örnek giriş/çıkış:** <br>

* **Giriş** (extensionAttribute5): "PermissionSetOne, PermisionSetTwo"
* **Çıkış**: ["PermissionSetOne", "PermissionSetTwo"]

### <a name="output-date-as-a-string-in-a-certain-format"></a>Belirli bir biçimde bir dize olarak çıkış tarihi
Belirli bir biçimdeki bir SaaS uygulamasına tarihleri göndermek istediğiniz. <br>
Örneğin, tarihleri biçimlendirmek için ServiceNow istiyorsunuz.

**İfade:** <br>

`FormatDateTime([extensionAttribute1], "yyyyMMddHHmmss.fZ", "yyyy-MM-dd")`

**Örnek giriş/çıkış:**

* **Giriş** (extensionAttribute1): "20150123105347.1Z"
* **ÇIKIŞ**:  "2015-01-23"

### <a name="replace-a-value-based-on-predefined-set-of-options"></a>Önceden tanımlanmış seçenekleri kümesi temel alınarak bir değeri değiştirin

Azure AD'de depolanan eyalet koduna göre kullanıcının saat dilimi tanımlamak gerekir. <br>
Önceden tanımlanmış seçeneklerden herhangi biri durum kodu eşleşmiyorsa, "Avustralya/Sidney" varsayılan değeri kullanın.

**İfade:** <br>
`Switch([state], "Australia/Sydney", "NSW", "Australia/Sydney","QLD", "Australia/Brisbane", "SA", "Australia/Adelaide")`

**Örnek giriş/çıkış:**

* **Giriş** (durum): "QLD"
* **ÇIKIŞ**: "Avustralya/Brisbane"

### <a name="replace-characters-using-a-regular-expression"></a>Bir normal ifade kullanarak karakterler değiştirin
Eşleşen bir normal ifade değeri ve bunları kaldırmak karakterler bulmak gerekir.

**İfade:** <br>

Değiştir ([mailNickname], "[a-zA-Z_] *", "",)

**Örnek giriş/çıkış:**

* **Giriş** (mailNickname: "john_doe72"
* **ÇIKIŞ**: "72"

### <a name="convert-generated-userprincipalname-upn-value-to-lower-case"></a>Oluşturulan userPrincipalName (UPN) değeri küçük harfe Dönüştür
Aşağıdaki örnekte, UPN değerini PreferredFirstName ve PreferredLastName kaynak alanları ile birleştirerek oluşturulur ve tüm karakterleri küçük harfe dönüştürmek için oluşturulan dizesini ToLower işlevi çalışır. 

`ToLower(Join("@", NormalizeDiacritics(StripSpaces(Join(".",  [PreferredFirstName], [PreferredLastName]))), "contoso.com"))`

**Örnek giriş/çıkış:**

* **Giriş** (PreferredFirstName): "John"
* **Giriş** (PreferredLastName): "Smith"
* **ÇIKIŞ**: "john.smith@contoso.com"

### <a name="generate-unique-value-for-userprincipalname-upn-attribute"></a>UserPrincipalName (UPN) özniteliği için benzersiz bir değer oluşturur
Bağlı kullanıcının ilk adını, ikinci adı ve Soyadı, UPN özniteliği için değer atamadan önce hedef AD dizininde UPN özniteliği için bir değer ve kendi benzersizlik denetimi oluşturmanız gerekiyor.

**İfade:** <br>

    SelectUniqueValue( 
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  [PreferredFirstName], [PreferredLastName]))), "contoso.com"), 
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  Mid([PreferredFirstName], 1, 1), [PreferredLastName]))), "contoso.com")
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  Mid([PreferredFirstName], 1, 2), [PreferredLastName]))), "contoso.com")
    )

**Örnek giriş/çıkış:**

* **Giriş** (PreferredFirstName): "John"
* **Giriş** (PreferredLastName): "Smith"
* **Çıkış**: "John.Smith@contoso.com" değilse UPN değerini John.Smith@contoso.com dizininde zaten mevcut değil
* **Çıkış**: "J.Smith@contoso.com" değilse UPN değerini John.Smith@contoso.com dizinde zaten var.
* **Çıkış**: "Jo.Smith@contoso.com" Yukarıdaki iki UPN değeri dizinde zaten mevcutsa

## <a name="related-articles"></a>İlgili makaleler
* [Kullanıcı sağlama/sağlamayı kaldırma SaaS uygulamaları için otomatik hale getirin](user-provisioning.md)
* [Kullanıcı sağlama için öznitelik eşlemelerini özelleştirme](customize-application-attributes.md)
* [Kullanıcı sağlama için kapsam oluşturma filtresi](define-conditional-rules-for-provisioning-user-accounts.md)
* [Kullanıcıların ve grupların Azure Active Directory'den uygulamalara otomatik olarak hazırlanmasını etkinleştirmek için SCIM'yi kullanma](use-scim-to-provision-users-and-groups.md)
* [Hesap sağlama bildirimleri](user-provisioning.md)
* [SaaS uygulamalarını tümleştirme hakkında öğreticiler listesi](../saas-apps/tutorial-list.md)
