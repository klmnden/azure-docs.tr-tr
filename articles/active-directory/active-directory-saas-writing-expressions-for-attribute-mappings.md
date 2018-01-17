---
title: "Azure Active Directory'de özellik eşlemeleri için ifade yazma | Microsoft Docs"
description: "Otomatik Azure Active Directory'de SaaS uygulama nesnelerinin sağlama sırasında öznitelik değerleri kabul edilebilir bir biçime dönüştürmek için ifade eşlemeleri kullanmayı öğrenin."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: b13c51cd-1bea-4e5e-9791-5d951a518943
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.openlocfilehash: 5549fb8f20ac2eb07b52b3b8e1c418873e467c93
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="writing-expressions-for-attribute-mappings-in-azure-active-directory"></a>Azure Active Directory'de özellik eşlemeleri için ifade yazma
Bir SaaS uygulaması sağlama yapılandırdığınızda belirtebilirsiniz öznitelik eşlemelerini tür bir ifade eşlemesi biridir. Bunlar için kullanıcılarınızın veri SaaS uygulaması için daha kabul edilebilir biçimlere dönüştürme olanak sağlayan bir betik benzeri ifadesi yazmanız gerekir.

## <a name="syntax-overview"></a>Söz dizimi genel bakış
Özellik eşlemeleri için ifade sözdizimi Applications (VBA) işlevleri için Visual Basic reminiscent aşağıdaki gibidir.

* Tüm ifadesi parantez içinde bağımsız değişken adından sonra oluşur işlevleri bakımından tanımlanmış olması gerekir: <br>
  *FunctionName (<< bağımsız değişkeni 1 >>, <<argument N>>)*
* Birbirine içinde işlevleri iç içe. Örneğin: <br> *FunctionOne(FunctionTwo(<<argument1>>))*
* Bağımsız değişkenler üç farklı türde işlevlerini geçirebilirsiniz:
  
  1. Öznitelikleri kare köşeli parantez içine alınmalıdır. Örneğin: [attributeName]
  2. Dize sabitleri çift tırnak içine alınmalıdır. Örneğin: "ABD"
  3. Diğer işlevleri. Örneğin: FunctionOne (<<argument1>>, FunctionTwo (<<argument2>>))
* Bir ters eğik çizgi (\) ya da dizisinde tırnak işareti (") gerekiyorsa dize sabitleri için onu ters eğik çizgi (\) simgesiyle kaçış uygulanmalıdır. Örneğin: "şirket adı: \"Contoso\""

## <a name="list-of-functions"></a>İşlevlerin listesi
[Append](#append) &nbsp; &nbsp; &nbsp; &nbsp; [FormatDateTime](#formatdatetime) &nbsp; &nbsp; &nbsp; &nbsp; [katılın](#join) &nbsp; &nbsp; &nbsp; &nbsp; [Mid](#mid) &nbsp; &nbsp; &nbsp; &nbsp; [değil](#not) &nbsp; &nbsp; &nbsp; &nbsp; [Değiştir](#replace) &nbsp; &nbsp; &nbsp; &nbsp; [SingleAppRoleAssignment](#singleapproleassignment) &nbsp; &nbsp; &nbsp; &nbsp; [StripSpaces](#stripspaces) &nbsp; &nbsp; &nbsp; &nbsp; [anahtarı](#switch)

- - -
### <a name="append"></a>Ekle
**İşlev:**<br> Append(Source, Suffix)

**Açıklama:**<br> Bir kaynak dize değeri alır ve onu sonuna sonek ekler.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gerekli |Dize |Genellikle kaynak nesneden özniteliğinin adı |
| **suffix** |Gerekli |Dize |Kaynak değerin sonuna istediğiniz dize. |

- - -
### <a name="formatdatetime"></a>FormatDateTime
**İşlev:**<br> FormatDateTime (kaynak, inputFormat, outputFormat)

**Açıklama:**<br> Bir tarih dizesi bir biçimden alır ve farklı bir biçime dönüştürür.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gerekli |Dize |Genellikle kaynak nesneden özniteliğinin adı. |
| **inputFormat** |Gerekli |Dize |Kaynak değerin beklenen biçimi. Desteklenen biçimler için bkz: [http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx](http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx). |
| **outputFormat** |Gerekli |Dize |Çıkış tarihi biçimi. |

- - -
### <a name="join"></a>Birleştir
**İşlev:**<br> Birleştirme (ayırıcı kaynak1, kaynak2,...)

**Açıklama:**<br> Birden çok birleştirebilirsiniz dışında join() Append() için benzer **kaynak** dize değerlerini tek bir dize halinde ve her bir değeri ile ayrılmış bir **ayırıcı** dize.

Kaynak değerlerden biri birden çok değerli özniteliği, her değer ise bu özniteliğin birlikte katılacaksa, ayırıcı değeri ayrılmış.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **ayırıcı** |Gerekli |Dize |Bir dizeye birleşir, kaynak değerleri ayırmak için kullanılan dize. Olabilir "" ayırıcı gerekiyorsa. |
| ** kaynak1... kaynakN ** |Gerekli, değişken-sayısı |Dize |Birlikte birleştirilecek değerleri dize. |

- - -
### <a name="mid"></a>Orta
**İşlev:**<br> Mid (kaynak, başlangıç, uzunluk)

**Açıklama:**<br> Kaynak değerin alt dizeyi döndürür. Bir alt dizesi kaynak dizeden karakterleri yalnızca bir bölümünü içeren bir dizedir.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gerekli |Dize |Genellikle özniteliğinin adı. |
| **start** |Gerekli |integer |İçinde dizin **kaynak** dize substring burada başlamalıdır. Dizedeki ilk karakter dizini 1 olacaktır, ikinci karakter dizin 2 sahip ve benzeri. |
| **uzunluğu** |Gerekli |integer |Dizenin uzunluğu. Uzunluk dışında ererse **kaynak** dize işlevi alt dizeyi döndürecektir **Başlat** dizin sonuna kadar **kaynak** dize. |

- - -
### <a name="not"></a>değil
**İşlev:**<br> Not(Source)

**Açıklama:**<br> Boole değeri döndürür **kaynak**. Varsa **kaynak** değer "*True*", döndürür "*False*". Aksi takdirde, döndürür "*True*".

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gerekli |Boole dizesi |Beklenen **kaynak** değerler: "True" veya "False"... |

- - -
### <a name="replace"></a>Değiştir
**İşlev:**<br> ObsoleteReplace (kaynak, oldValue, regexPattern, regexGroupName, replacementValue, replacementAttributeName, şablonu)

**Açıklama:**<br>
Bir dize içindeki değerleri değiştirir. Sağlanan parametre bağlı olarak farklı şekilde çalışır:

* Zaman **oldValue** ve **replacementValue** sağlanır:
  
  * İle replacementValue oldValue kaynağındaki tüm oluşumlarını değiştirir
* Zaman **oldValue** ve **şablonu** sağlanır:
  
  * Tüm oluşumlarını değiştirir **oldValue** içinde **şablonu** ile **kaynak** değeri
* Zaman **oldValueRegexPattern**, **oldValueRegexGroupName**, **replacementValue** sağlanır:
  
  * Kaynak dizesi replacementValue ile oldValueRegexPattern eşleşen tüm değerleri değiştirir
* Zaman **oldValueRegexPattern**, **oldValueRegexGroupName**, **replacementPropertyName** sağlanır:
  
  * Varsa **kaynak** değerine sahip **kaynak** döndürülür
  * Varsa **kaynak** değeri yok, kullanan **oldValueRegexPattern** ve **oldValueRegexGroupName** özelliğiyle değiştirme değeri ayıklamak için  **replacementPropertyName**. Sonuç olarak değiştirme değeri döndürülür

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gerekli |Dize |Genellikle kaynak nesneden özniteliğinin adı. |
| **oldValue** |İsteğe bağlı |Dize |İçinde değiştirilecek değer **kaynak** veya **şablon**. |
| **regexPattern** |İsteğe bağlı |Dize |Regex düzenidir içinde değiştirilecek değer için **kaynak**. Veya replacementPropertyName kullanıldığında, değer değiştirme özelliği ayıklamak için desen. |
| **regexGroupName** |İsteğe bağlı |Dize |İçinde grup adını **regexPattern**. Yalnızca replacementPropertyName kullanıldığında, biz bu grubun değerini değiştirme özelliğinden replacementValue olarak ayıklar. |
| **replacementValue** |İsteğe bağlı |Dize |Eski bir öğe ile değiştirmek için yeni değer. |
| **replacementAttributeName** |İsteğe bağlı |Dize |Kaynak yok değerine sahip olduğunda değiştirme değeri için kullanılacak özniteliğinin adı. |
| **şablonu** |İsteğe bağlı |Dize |Zaman **şablonu** değeri sağlanır, biz görüneceğini **oldValue** şablonu içinde ve kaynak değerle değiştirin. |

- - -
### <a name="singleapproleassignment"></a>SingleAppRoleAssignment
**İşlev:**<br> SingleAppRoleAssignment([appRoleAssignments])

**Açıklama:**<br> Belirli bir uygulamada bir kullanıcıya atanan tüm appRoleAssignments listesinden tek bir appRoleAssignment döndürür. Bu işlev, appRoleAssignments nesneyi bir tek bir rol adı dizeye dönüştürmek için gereklidir. Yalnızca bir appRoleAssignment emin olmak için en iyisi olduğuna dikkat edin tek bir kullanıcıya aynı anda atanır ve birden çok rol döndürülen rol dizesini atanmışsa tahmin edilebilir olmayabilir.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **[appRoleAssignments]** |Gerekli |Dize |**[appRoleAssignments]**  nesnesi. |

- - -
### <a name="stripspaces"></a>StripSpaces
**İşlev:**<br> StripSpaces(source)

**Açıklama:**<br> Tüm alanı kaldırır ("") kaynak dize karakterler.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gerekli |Dize |**Kaynak** güncelleştirmek için değer. |

- - -
### <a name="switch"></a>Anahtar
**İşlev:**<br> Anahtar (kaynak, defaultValue, key1, value1, key2, value2,...)

**Açıklama:**<br> Zaman **kaynak** değer eşleşen bir **anahtar**, döndürür **değeri** söz konusu **anahtar**. Varsa **kaynak** değeri döndürür tüm anahtarları eşleşmiyor **defaultValue**.  **Anahtar** ve **değeri** parametreleri çiftler halinde her zaman gelmesi gerekir. İşlev, her zaman çift sayıda parametre bekliyor.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gerekli |Dize |**Kaynak** güncelleştirmek için değer. |
| **defaultValue** |İsteğe bağlı |Dize |Kaynak herhangi bir anahtarı eşleşmediğinde kullanılacak varsayılan değeri. Boş bir dize olabilir (""). |
| **key** |Gerekli |Dize |**Anahtar** karşılaştırmak için **kaynak** ile değer. |
| **değer** |Gerekli |Dize |Değiştirme değeri için **kaynak** eşleşen. |

## <a name="examples"></a>Örnekler
### <a name="strip-known-domain-name"></a>Şerit bilinen etki alanı adı
Bir kullanıcı adı almak için bir kullanıcının e-posta bilinen etki alanı adından çıkarabilmesi gerekir. <br>
Örneğin, etki alanı "contoso.com" ise, aşağıdaki ifadeyi kullanabilirsiniz:

**İfade:** <br>
`Replace([mail], "@contoso.com", , ,"", ,)`

**Giriş / Çıkış örneği:** <br>

* **Giriş** (posta): "john.doe@contoso.com"
* **OUTPUT**:  "john.doe"

### <a name="append-constant-suffix-to-user-name"></a>Kullanıcı adı için sabit soneki ekleme
Salesforce korumalı alan kullanıyorsanız, ek bir sonek eşitlemeden önce tüm kullanıcı adları eklemek gerekebilir.

**İfade:** <br>
`Append([userPrincipalName], ".test"))`

**Giriş/Çıkış örneği:** <br>

* **Giriş**: (userPrincipalName): "John.Doe@contoso.com"
* **OUTPUT**:  "John.Doe@contoso.com.test"

### <a name="generate-user-alias-by-concatenating-parts-of-first-and-last-name"></a>Birleştirme bölümleri adı ve Soyadı tarafından kullanıcı diğer adı oluştur
Kullanıcının ilk adını, ilk 3 harf ve kullanıcının soyadını ilk 5 harfini gerçekleştirerek kullanıcı diğer adı oluşturmak gerekir.

**İfade:** <br>
`Append(Mid([givenName], 1, 3), Mid([surname], 1, 5))`

**Giriş/Çıkış örneği:** <br>

* **Giriş** (givenName): "John"
* **Giriş** (Soyadı): "Doe"
* **OUTPUT**:  "JohDoe"

### <a name="output-date-as-a-string-in-a-certain-format"></a>Çıkış tarihi belirli biçiminde bir dize olarak
Belirli bir biçimde SaaS uygulamasına tarihleri göndermek istiyor. <br>
Örneğin, ServiceNow için tarihleri biçimlendirmek istediğiniz.

**İfade:** <br>

`FormatDateTime([extensionAttribute1], "yyyyMMddHHmmss.fZ", "yyyy-MM-dd")`

**Giriş/Çıkış örneği:**

* **Giriş** (extensionAttribute1): "20150123105347.1Z"
* **OUTPUT**:  "2015-01-23"

### <a name="replace-a-value-based-on-predefined-set-of-options"></a>Önceden tanımlanmış seçenekleri kümesini temel alan bir değer değiştirin
Azure AD'de depolanan durum kodunu göre kullanıcının saat dilimi tanımlamanız gerekir. <br>
Durum kodu önceden tanımlanmış seçeneklerinden herhangi birini eşleşmiyorsa, "Avustralya/Sidney" varsayılan değeri kullanın.

**İfade:** <br>

`Switch([state], "Australia/Sydney", "NSW", "Australia/Sydney","QLD", "Australia/Brisbane", "SA", "Australia/Adelaide")`

**Giriş/Çıkış örneği:**

* **INPUT** (state): "QLD"
* **Çıktı**: "Avustralya/Brisbane"

## <a name="related-articles"></a>İlgili Makaleler
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
* [Kullanıcı sağlama/sağlamayı SaaS uygulamaları için otomatik hale getirme](active-directory-saas-app-provisioning.md)
* [Kullanıcı sağlama öznitelik eşlemelerini özelleştirme](active-directory-saas-customizing-attribute-mappings.md)
* [Kapsam belirleme filtreleri kullanıcı sağlama](active-directory-saas-scoping-filters.md)
* [Kullanıcıların ve grupların Azure Active Directory'den uygulamalara otomatik olarak hazırlanmasını etkinleştirmek için SCIM'yi kullanma](active-directory-scim-provisioning.md)
* [Hesap sağlama bildirimleri](active-directory-saas-account-provisioning-notifications.md)
* [SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)

