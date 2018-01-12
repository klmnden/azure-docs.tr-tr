---
title: "Azure AD Connect eşitleme: işlevleri başvurusu | Microsoft Docs"
description: "Azure AD Connect eşitleme bildirim temelli hazırlama ifadelerini başvuru."
services: active-directory
documentationcenter: 
author: andkjell
manager: mtillman
editor: 
ms.assetid: 4f525ca0-be0e-4a2e-8da1-09b6b567ed5f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: d84a31e72d3e97ebb12f1747259fcb6e6b8fdcdc
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
---
# <a name="azure-ad-connect-sync-functions-reference"></a>Azure AD Connect eşitleme: işlevleri başvurusu
Azure AD Connect işlevleri eşitleme sırasında bir öznitelik değeri işlemek için kullanılır.  
İşlevler söz dizimi aşağıdaki biçimi kullanarak ifade edilir:  
`<output type> FunctionName(<input type> <position name>, ..)`

İşlev aşırı yüklendi ve birden çok sözdizimleri kabul eder, tüm geçerli sözdizimi listelenir.  
İşlevler kesin türü belirtilmiş ve türü belgelenen türü eşleştiğinden geçirilen doğrulayın.  
Türü eşleşmiyorsa, bir hata oluşturulur.

Türleri, aşağıdaki sözdizimi ile ifade edilir:

* **Depo** – ikili
* **bool** – Boole
* **dt** – UTC tarihi/saati
* **Enum** – bilinen sabitleri numaralandırması
* **exp** – bir Boole değeri değerlendirmek için beklenen ifade
* **mvbin** – birden çok değerli ikili
* **mvstr** – birden çok değerli dize
* **mvref** – birden çok değerli başvurusu
* **NUM** – sayısal
* **Ref** – başvurusu
* **str** – dize
* **var** – bir değişken (neredeyse) herhangi bir tür
* **void** – bir değer döndürmüyor

İşlevler türleriyle **mvbin**, **mvstr**, ve **mvref** birden çok değerli öznitelikleri yalnızca çalışabilirsiniz. İle işlevleri **bin**, **str**, ve **ref** hem tek değerli ve birden çok değerli öznitelikleri üzerinde çalışır.

## <a name="functions-reference"></a>İşlevler Başvurusu
| İşlevlerin listesi |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
| **Sertifika** | | | | |
| [CertExtensionOids](#certextensionoids) |[CertFormat](#certformat) |[CertFriendlyName](#certfriendlyname) |[CertHashString](#certhashstring) | |
| [CertIssuer](#certissuer) |[CertIssuerDN](#certissuerdn) |[CertIssuerOid](#certissueroid) |[CertKeyAlgorithm](#certkeyalgorithm) | |
| [CertKeyAlgorithmParams](#certkeyalgorithmparams) |[CertNameInfo](#certnameinfo) |[CertNotAfter](#certnotafter) |[CertNotBefore](#certnotbefore) | |
| [CertPublicKeyOid](#certpublickeyoid) |[CertPublicKeyParametersOid](#certpublickeyparametersoid) |[CertSerialNumber](#certserialnumber) |[CertSignatureAlgorithmOid](#certsignaturealgorithmoid) | |
| [CertSubject](#certsubject) |[CertSubjectNameDN](#certsubjectnamedn) |[CertSubjectNameOid](#certsubjectnameoid) |[Certthumbprınt](#certthumbprint) | |
[CertVersion](#certversion) |[IsCert](#iscert) | | | |
| **Dönüştürme** | | | | |
| [CBool](#cbool) |[CDate](#cdate) |[CGuid](#cguid) |[ConvertFromBase64](#convertfrombase64) | |
| [ConvertToBase64](#converttobase64) |[ConvertFromUTF8Hex](#convertfromutf8hex) |[ConvertToUTF8Hex](#converttoutf8hex) |[CNum](#cnum) | |
| [CRef](#cref) |[CStr](#cstr) |[StringFromGuid](#StringFromGuid) |[StringFromSid](#stringfromsid) | |
| **Tarih / saat** | | | | |
| [DateAdd](#dateadd) |[DateFromNum](#datefromnum) |[FormatDateTime](#formatdatetime) |[Şimdi](#now) | |
| [NumFromDate](#numfromdate) | | | | |
| **Dizin** | | | | |
| [DNComponent](#dncomponent) |[DNComponentRev](#dncomponentrev) |[EscapeDNComponent](#escapedncomponent) | | |
| **Değerlendirme** | | | | |
| [IsBitSet](#isbitset) |[IsDate](#isdate) |[IsEmpty](#isempty) |[IsGuid](#isguid) | |
| [IsNull](#isnull) |[IsNullOrEmpty](#isnullorempty) |[IsNumeric](#isnumeric) |[Olmasına](#ispresent) | |
| [IsString](#isstring) | | | | |
| **Matematik** | | | | |
| [BitAnd](#bitand) |[BitOr](#bitor) |[RandomNum](#randomnum) | | |
| **Birden çok değerli** | | | | |
| [İçerir](#contains) |[Sayısı](#count) |[Öğesi](#item) |[ItemOrNull](#itemornull) | |
| [Birleştir](#join) |[RemoveDuplicates](#removeduplicates) |[Böl](#split) | | |
| **Program akışı** | | | | |
| [Hata](#error) |[IIF](#iif) |[Seç](#select) |[Anahtar](#switch) | |
| [Burada](#where) |[İle](#with) | | | |
| **Metin** | | | | |
| [GUID](#guid) |[InStr](#instr) |[InStrRev](#instrrev) |[LCase](#lcase) | |
| [Sol](#left) |[Len](#len) |[LTrim](#ltrim) |[Orta](#mid) | |
| [PadLeft](#padleft) |[PadRight](#padright) |[PCase](#pcase) |[Değiştir](#replace) | |
| [ReplaceChars](#replacechars) |[Sağ](#right) |[RTrim](#rtrim) |[Kırpma](#trim) | |
| [UCase](#ucase) |[Word](#word) | | | |

- - -
### <a name="bitand"></a>BitAnd
**Açıklama:**  
BitAnd işlevi belirtilen BITS üzerinde bir değer ayarlar.

**Sözdizimi:**  
`num BitAnd(num value1, num value2)`

* value1, value2: and değerini birlikte olmalıdır sayısal değerler

**Notlar:**  
Bu işlev parametrelerinin her ikisini de ikili gösterimine dönüştürür ve biraz ayarlar:

* 0 - biri veya her ikisini karşılık gelen bitleri *maskesi* ve *bayrağı* 0
* 1 - karşılık gelen bit hem de 1 olması gerekir.

Diğer bir deyişle, her iki parametre karşılık gelen bitleri 1 olduğu durumlar dışında tüm durumlarda 0 döndürür.

**Örnek:**  
`BitAnd(&HF, &HF7)`  
Onaltılık "F" ve "F7" değerlendirmek için bu değer 7 döndürür.

- - -
### <a name="bitor"></a>BitOr
**Açıklama:**  
BitOr işlevi belirtilen BITS üzerinde bir değer ayarlar.

**Sözdizimi:**  
`num BitOr(num value1, num value2)`

* value1, value2: or birlikte olmalıdır sayısal değerler

**Notlar:**  
Bu işlev parametrelerinin her ikisini de ikili gösterimine dönüştürür ve karşılık gelen BITS her ikisi de 0 olduğunda birini veya her ikisini maskesi ve bayrağı karşılık gelen bitleri 1 ise 1 ve 0 biraz ayarlar. Diğer bir deyişle, her iki parametre karşılık gelen bitleri 0 nerede dışındaki tüm durumlarda 1 döndürür.

- - -
### <a name="cbool"></a>CBool
**Açıklama:**  
CBool işlevi değerlendirilen geçen ifadeye göre bir Boole değeri döndürür

**Sözdizimi:**  
`bool CBool(exp Expression)`

**Notlar:**  
CBool True değerini döndürür sonra ifadeyi sıfır olmayan bir değere hesaplar varsa, aksi takdirde, False değerini döndürür.

**Örnek:**  
`CBool([attrib1] = [attrib2])`  

Döndürür True her iki öznitelik aynı değere sahip.

- - -
### <a name="cdate"></a>CDate
**Açıklama:**  
CDate işlevi bir dizeden bir UTC DateTime değeri döndürür. Tarih saat yerel öznitelik türü eşitlenmiş değil ancak bazı işlevler tarafından kullanılır.

**Sözdizimi:**  
`dt CDate(str value)`

* Değer: Bir dizeyi bir tarih, saat ve isteğe bağlı olarak saat dilimi

**Notlar:**  
Döndürülen dize her zaman UTC biçiminde değil.

**Örnek:**  
`CDate([employeeStartTime])`  
Çalışanın üzerinde bir DateTime dayalı döndürür başlangıç zamanı

`CDate("2013-01-10 4:00 PM -8")`  
Döndürür DateTime temsil eden bir "2013-01-11 12: 00'da"


- - -
### <a name="certextensionoids"></a>CertExtensionOids
**Açıklama:**  
Bir sertifika nesnesinin tüm kritik uzantılar OID değerini döndürür.

**Sözdizimi:**  
`mvstr CertExtensionOids(binary certificateRawData)`  
*   certificateRawData: Bayt dizisine bir X.509 sertifikası gösterimi. Bayt dizisi olarak kodlanmış ikili (DER) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certformat"></a>CertFormat
**Açıklama:**  
Bu X.509v3 sertifikasını biçim adını döndürür.

**Sözdizimi:**  
`str CertFormat(binary certificateRawData)`  
*   certificateRawData: Bayt dizisine bir X.509 sertifikası gösterimi. Bayt dizisi olarak kodlanmış ikili (DER) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certfriendlyname"></a>CertFriendlyName
**Açıklama:**  
Bir sertifika için ilişkili diğer adı döndürür.

**Sözdizimi:**  
`str CertFriendlyName(binary certificateRawData)`  
*   certificateRawData: Bayt dizisine bir X.509 sertifikası gösterimi. Bayt dizisi olarak kodlanmış ikili (DER) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certhashstring"></a>CertHashString
**Açıklama:**  
X.509v3 sertifikasını SHA1 karma değeri bir onaltılık dize döndürür.

**Sözdizimi:**  
`str CertHashString(binary certificateRawData)`  
*   certificateRawData: Bayt dizisine bir X.509 sertifikası gösterimi. Bayt dizisi olarak kodlanmış ikili (DER) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certissuer"></a>CertIssuer
**Açıklama:**  
X.509v3 sertifikasını veren sertifika yetkilisinin adını döndürür.

**Sözdizimi:**  
`str CertIssuer(binary certificateRawData)`  
*   certificateRawData: Bayt dizisine bir X.509 sertifikası gösterimi. Bayt dizisi olarak kodlanmış ikili (DER) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certissuerdn"></a>CertIssuerDN
**Açıklama:**  
Sertifikayı verenin ayırt edici adını döndürür.

**Sözdizimi:**  
`str CertIssuerDN(binary certificateRawData)`  
*   certificateRawData: Bayt dizisine bir X.509 sertifikası gösterimi. Bayt dizisi olarak kodlanmış ikili (DER) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certissueroid"></a>CertIssuerOid
**Açıklama:**  
Sertifikayı verenin OID döndürür.

**Sözdizimi:**  
`str CertIssuerOid(binary certificateRawData)`  
*   certificateRawData: Bayt dizisine bir X.509 sertifikası gösterimi. Bayt dizisi olarak kodlanmış ikili (DER) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certkeyalgorithm"></a>CertKeyAlgorithm
**Açıklama:**  
Bu X.509v3 sertifika için anahtar algoritması bilgi dize olarak döndürür.

**Sözdizimi:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   certificateRawData: Bayt dizisine bir X.509 sertifikası gösterimi. Bayt dizisi olarak kodlanmış ikili (DER) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certkeyalgorithmparams"></a>CertKeyAlgorithmParams
**Açıklama:**  
X.509v3 sertifika için anahtar algoritması parametreleri onaltılık dize olarak döndürür.

**Sözdizimi:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   certificateRawData: Bayt dizisine bir X.509 sertifikası gösterimi. Bayt dizisi olarak kodlanmış ikili (DER) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certnameinfo"></a>CertNameInfo
**Açıklama:**  
Konu ve sertifikayı veren adları bir sertifika verir.

**Sözdizimi:**  
`str CertNameInfo(binary certificateRawData, str x509NameType, bool includesIssuerName)`  
*   certificateRawData: Bayt dizisine bir X.509 sertifikası gösterimi. Bayt dizisi olarak kodlanmış ikili (DER) veya Base64 ile kodlanmış X.509 veri olabilir.
*   X509NameType: Konu X509NameType değeri.
*   includesIssuerName: verenin adı; dahil etmek için true Aksi takdirde false.

- - -
### <a name="certnotafter"></a>CertNotAfter
**Açıklama:**  
Yerel saatle sonra bir sertifika artık geçerli olduğu tarihi döndürür.

**Sözdizimi:**  
`dt CertNotAfter(binary certificateRawData)`  
*   certificateRawData: Bayt dizisine bir X.509 sertifikası gösterimi. Bayt dizisi olarak kodlanmış ikili (DER) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certnotbefore"></a>CertNotBefore
**Açıklama:**  
Yerel saatte bir sertifikanın geçerli hale geldiği tarihi döndürür.

**Sözdizimi:**  
`dt CertNotBefore(binary certificateRawData)`  
*   certificateRawData: Bayt dizisine bir X.509 sertifikası gösterimi. Bayt dizisi olarak kodlanmış ikili (DER) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certpublickeyoid"></a>CertPublicKeyOid
**Açıklama:**  
X.509v3 sertifikası için ortak anahtar OID döndürür.

**Sözdizimi:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   certificateRawData: Bayt dizisine bir X.509 sertifikası gösterimi. Bayt dizisi olarak kodlanmış ikili (DER) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certpublickeyparametersoid"></a>CertPublicKeyParametersOid
**Açıklama:**  
OID X.509v3 sertifikasını için bir ortak anahtar parametrelerinin döndürür.

**Sözdizimi:**  
`str CertPublicKeyParametersOid(binary certificateRawData)`  
*   certificateRawData: Bayt dizisine bir X.509 sertifikası gösterimi. Bayt dizisi olarak kodlanmış ikili (DER) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certserialnumber"></a>CertSerialNumber
**Açıklama:**  
X.509v3 sertifika seri numarasını döndürür.

**Sözdizimi:**  
`str CertSerialNumber(binary certificateRawData)`  
*   certificateRawData: Bayt dizisine bir X.509 sertifikası gösterimi. Bayt dizisi olarak kodlanmış ikili (DER) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certsignaturealgorithmoid"></a>CertSignatureAlgorithmOid
**Açıklama:**  
Bir sertifikanın imzasını oluşturmak için kullanılan algoritma OID döndürür.

**Sözdizimi:**  
`str CertSignatureAlgorithmOid(binary certificateRawData)`  
*   certificateRawData: Bayt dizisine bir X.509 sertifikası gösterimi. Bayt dizisi olarak kodlanmış ikili (DER) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certsubject"></a>CertSubject
**Açıklama:**  
Bir sertifikadan konu ayırt edici adını alır.

**Sözdizimi:**  
`str CertSubject(binary certificateRawData)`  
*   certificateRawData: Bayt dizisine bir X.509 sertifikası gösterimi. Bayt dizisi olarak kodlanmış ikili (DER) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certsubjectnamedn"></a>CertSubjectNameDN
**Açıklama:**  
Bir sertifikadan konu ayırt edici adını döndürür.

**Sözdizimi:**  
`str CertSubjectNameDN(binary certificateRawData)`  
*   certificateRawData: Bayt dizisine bir X.509 sertifikası gösterimi. Bayt dizisi olarak kodlanmış ikili (DER) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certsubjectnameoid"></a>CertSubjectNameOid
**Açıklama:**  
Bir sertifikadan konu adı OID döndürür.

**Sözdizimi:**  
`str CertSubjectNameOid(binary certificateRawData)`  
*   certificateRawData: Bayt dizisine bir X.509 sertifikası gösterimi. Bayt dizisi olarak kodlanmış ikili (DER) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certthumbprint"></a>Certthumbprınt
**Açıklama:**  
Bir sertifikanın parmak izini döndürür.

**Sözdizimi:**  
`str CertThumbprint(binary certificateRawData)`  
*   certificateRawData: Bayt dizisine bir X.509 sertifikası gösterimi. Bayt dizisi olarak kodlanmış ikili (DER) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certversion"></a>CertVersion
**Açıklama:**  
Bir sertifikanın X.509 biçimindeki sürümü döndürür.

**Sözdizimi:**  
`str CertThumbprint(binary certificateRawData)`  
*   certificateRawData: Bayt dizisine bir X.509 sertifikası gösterimi. Bayt dizisi olarak kodlanmış ikili (DER) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="cguid"></a>CGuid
**Açıklama:**  
CGuid işlevi bir GUID dize gösterimini ikili gösterimine dönüştürür.

**Sözdizimi:**  
`bin CGuid(str GUID)`

* Bu desen biçimlendirilmiş bir dize: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx veya {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

- - -
### <a name="contains"></a>Contains
**Açıklama:**  
Birden çok değerli bir öznitelik içinde bir dize içerir işlev bulur

**Sözdizimi:**  
`num Contains (mvstring attribute, str search)`-büyük küçük harfe duyarlı  
`num Contains (mvstring attribute, str search, enum Casetype)`  
`num Contains (mvref attribute, str search)`-büyük küçük harfe duyarlı

* Öznitelik: aramak için birden çok değerli özniteliği.
* Arama: öznitelikte bulmak için dizesi.
* Casetype: CaseInsensitive veya CaseSensitive.

Dizin, dize bulunduğu birden çok değerli özniteliğinde döndürür. Dize bulunamazsa, 0 döndürülür.

**Notlar:**  
Birden çok değerli dize öznitelikleri için arama alt dizeler değerleri bulur.  
Başvuru özniteliği için Aranan dize değeri bir eşleşme olarak kabul edilmesi için tam olarak eşleşmelidir.

**Örnek:**  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
Bir birincil e-posta adresi proxyAddresses özniteliğine sahipse, (büyük harf tarafından gösterilen "SMTP:"), proxyAddress özniteliği döndürür, aksi takdirde bir hata döndürür.

- - -
### <a name="convertfrombase64"></a>ConvertFromBase64
**Açıklama:**  
ConvertFromBase64 işlevi belirtilen base64 kodlu değer normal bir dizeye dönüştürür.

**Sözdizimi:**  
`str ConvertFromBase64(str source)`-Unicode kodlama için varsayar.  
`str ConvertFromBase64(str source, enum Encoding)`

* Kaynak: Base64 ile kodlanmış dize  
* Kodlaması: Unicode, ASCII, UTF8

**Örnek**  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

Örneklerin her ikisi de döndürmek "*Merhaba Dünya!*"

- - -
### <a name="convertfromutf8hex"></a>ConvertFromUTF8Hex
**Açıklama:**  
ConvertFromUTF8Hex işlevi belirtilen UTF8 olarak kodlanmış onaltılık değeri dizeye dönüştürür.

**Sözdizimi:**  
`str ConvertFromUTF8Hex(str source)`

* Kaynak: UTF8 2-bayt kodlanmış dizesi

**Notlar:**  
Bu işlevin sonucu DN özniteliği için kolay olduğunu ConvertFromBase64([],UTF8) içinde arasındaki fark.  
Bu biçim DN Azure Active Directory tarafından kullanılır.

**Örnek:**  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
Döndürür "*Merhaba Dünya!*"

- - -
### <a name="converttobase64"></a>ConvertToBase64
**Açıklama:**  
ConvertToBase64 işlev bir dize Unicode base64 dizeye dönüştürür.  
Base-64 basamak ile kodlanmış kendi eşdeğer dize gösterimi dizisi değerine dönüştürür.

**Sözdizimi:**  
`str ConvertToBase64(str source)`

**Örnek:**  
`ConvertToBase64("Hello world!")`  
"SABlAGwAbABvACAAdwBvAHIAbABkACEA" döndürür

- - -
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex
**Açıklama:**  
ConvertToUTF8Hex işlev bir dize UTF8 olarak kodlanmış onaltılık değerine dönüştürür.

**Sözdizimi:**  
`str ConvertToUTF8Hex(str source)`

**Notlar:**  
Bu işlev çıktı biçimi DN özniteliği biçimi olarak Azure Active Directory tarafından kullanılır.

**Örnek:**  
`ConvertToUTF8Hex("Hello world!")`  
Döndürür 48656C6C6F20776F726C6421

- - -
### <a name="count"></a>Sayı
**Açıklama:**  
Count işlevi, birden çok değerli bir öznitelikte öğe sayısını döndürür

**Sözdizimi:**  
`num Count(mvstr attribute)`

- - -
### <a name="cnum"></a>CNum
**Açıklama:**  
CNum işlev bir dize alır ve sayısal veri türü döndürür.

**Sözdizimi:**  
`num CNum(str value)`

- - -
### <a name="cref"></a>CRef
**Açıklama:**  
Bir dizeyi bir başvuru özniteliği dönüştürür

**Sözdizimi:**  
`ref CRef(str value)`

**Örnek:**  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

- - -
### <a name="cstr"></a>CStr
**Açıklama:**  
CStr işlevinin bir dize veri türüne dönüştürür.

**Sözdizimi:**  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

* değer: bir sayısal değer, başvuru özniteliği ya da Boole değeri olabilir.

**Örnek:**  
`CStr([dn])`  
Döndürebilirsiniz "cn = Can, dc = contoso, dc = com"

- - -
### <a name="dateadd"></a>DateAdd
**Açıklama:**  
Belirli bir zaman aralığı eklenmiş olan bir tarih içeren bir tarih döndürür.

**Sözdizimi:**  
`dt DateAdd(str interval, num value, dt date)`

* aralığı: dize eklemek istediğiniz zaman aralığı olan ifade. Dizenin şu değerlerden biri olmalıdır:
  * yyyy yıl
  * q üç aylık dönem
  * m ay
  * yılın y günü
  * d gün
  * w haftanın günü
  * ww hafta
  * h Saat
  * n dakika
  * s ikinci
* değer: eklemek istediğiniz birim sayısı. (Gelecekteki tarihleri almak için) olumlu veya olumsuz (geçmişteki tarihler almak için) olabilir.
* Tarih: aralık eklenir tarihini temsil eden DateTime.

**Örnek:**  
`DateAdd("m", 3, CDate("2001-01-01"))`  
3 ay ekler ve "2001-04-01" temsil eden bir DateTime döndürür.

- - -
### <a name="datefromnum"></a>DateFromNum
**Açıklama:**  
Bir değer Reklamın tarih biçimlendirme bir DateTime türü DateFromNum işlevi dönüştürür.

**Sözdizimi:**  
`dt DateFromNum(num value)`

**Örnek:**  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
2012-01-01 temsil eden bir DateTime döndürür 23:00:00

- - -
### <a name="dncomponent"></a>DNComponent
**Açıklama:**  
DNComponent işlevi soldan giderek belirtilen bir DN bileşen değerini döndürür.

**Sözdizimi:**  
`str DNComponent(ref dn, num ComponentNumber)`

* DN: yorumlamak için başvuru özniteliği
* ComponentNumber: Döndürülecek DN bileşeninde

**Örnek:**  
`DNComponent(CRef([dn]),1)`  
DN ise "CN = Joe, ou = =..." Can döndürür

- - -
### <a name="dncomponentrev"></a>DNComponentRev
**Açıklama:**  
DNComponentRev işlevi (Bitiş) sağdan giderek belirtilen bir DN bileşen değerini döndürür.

**Sözdizimi:**  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

* DN: yorumlamak için başvuru özniteliği
* ComponentNumber - döndürülecek DN bileşeni
* Seçenekler: DC – tüm bileşenleri Yoksay "dc ="

**Örnek:**  
DN ise "CN = Joe, ou = Atlanta, ou = GA, ou = = US, dc = contoso, dc = com" sonra  
`DNComponentRev(CRef([dn]),3)`  
`DNComponentRev(CRef([dn]),1,"DC")`  
Her ikisi de BİZE döndür.

- - -
### <a name="error"></a>Hata
**Açıklama:**  
Hata işlevi bir özel hata döndürmek için kullanılır.

**Sözdizimi:**  
`void Error(str ErrorMessage)`

**Örnek:**  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
Öznitelik accountName mevcut değilse, bir hata nesnede atar.

- - -
### <a name="escapedncomponent"></a>EscapeDNComponent
**Açıklama:**  
EscapeDNComponent işlevi bir DN biri bileşenini alır ve LDAP temsil edilebilir şekilde çıkışları.

**Sözdizimi:**  
`str EscapeDNComponent(str value)`

**Örnek:**  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
DisplayName özniteliği LDAP'de kaçış karakterleri olsa bile, bir LDAP dizininde nesne oluşturulabilir emin olur.

- - -
### <a name="formatdatetime"></a>FormatDateTime
**Açıklama:**  
FormatDateTime işlevi DateTime bir dize olarak belirtilen biçimiyle için kullanılır

**Sözdizimi:**  
`str FormatDateTime(dt value, str format)`

* değer: tarih saat biçiminde bir değer
* Biçim: dönüştürmek için kullanılacak biçimi temsil eden dize.

**Notlar:**  
Biçim için olası değerler şurada bulunabilir: [kullanıcı tanımlı tarih/saat biçimleri (biçim işlevi)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)

**Örnek:**  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
"2007-12-25" sonuçlanır.

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
"İçinde 20140905081453.0Z" neden olabilir

- - -
### <a name="guid"></a>Guid
**Açıklama:**  
Yeni bir rastgele GUID GUID işlevi oluşturur

**Sözdizimi:**  
`str Guid()`

- - -
### <a name="iif"></a>IIF
**Açıklama:**  
IIf işlevi belirtilen bir koşula göre olası değerlerin kümesini döndürür.

**Sözdizimi:**  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

* koşul: herhangi bir değer veya true veya false sonucu verebilen ifade.
* Koşul: koşul true olarak, döndürülen değer değerlendirilirse.
* valueIfFalse: koşul döndürülen değeri false olarak değerlendirilirse.

**Örnek:**  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 Kullanıcı bir stajyer ise "t-", başka başlangıcı eklenen sahip bir kullanıcı diğer adı olduğu gibi kullanıcının diğer adı döndürür.

- - -
### <a name="instr"></a>InStr
**Açıklama:**  
InStr işlevi bir alt dizenin ilk örneğinin bir dizede bulur.

**Sözdizimi:**  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

* stringcheck: aranacak dize
* stringmatch: dize bulunamadı
* Başlat: başlama konumu alt dizeyi bulur
* karşılaştırma: vbTextCompare veya vbBinaryCompare

**Notlar:**  
Burada 0 ise bulunamadı veya alt dizeyi bulunamadı konumunu döndürür.

**Örnek:**  
`InStr("The quick brown fox","quick")`  
Evalues 5

`InStr("repEated","e",3,vbBinaryCompare)`  
7'ye değerlendirir

- - -
### <a name="instrrev"></a>InStrRev
**Açıklama:**  
InStrRev işlevi bir alt dizesi son a geçişi bir dizede bulur.

**Sözdizimi:**  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

* stringcheck: aranacak dize
* stringmatch: dize bulunamadı
* Başlat: başlama konumu alt dizeyi bulur
* karşılaştırma: vbTextCompare veya vbBinaryCompare

**Notlar:**  
Burada 0 ise bulunamadı veya alt dizeyi bulunamadı konumunu döndürür.

**Örnek:**  
`InStrRev("abbcdbbbef","bb")`  
7 döndürür

- - -
### <a name="isbitset"></a>IsBitSet
**Açıklama:**  
Veya set IsBitSet testleri bir bit ise işlevi

**Sözdizimi:**  
`bool IsBitSet(num value, num flag)`

* değer: evaluated.flag bir sayısal değer: değerlendirilecek bit olan sayısal bir değer

**Örnek:**  
`IsBitSet(&HF,4)`  
"4" bit onaltılık değeri "F" olarak ayarlandığından True değerini döndürür

- - -
### <a name="isdate"></a>IsDate
**Açıklama:**  
İfade olabiliyorsa IsDate işlevi True olarak değerlendirilir sonra bir DateTime türü değerlendirir.

**Sözdizimi:**  
`bool IsDate(var Expression)`

**Notlar:**  
CDate() başarılı olup olmadığını belirlemek için kullanılır.

- - -
### <a name="iscert"></a>IsCert
**Açıklama:**  
.NET X509Certificate2 sertifika nesnesine ham verileri seri hale getirilebilir true değerini döndürür.

**Sözdizimi:**  
`bool CertThumbprint(binary certificateRawData)`  
*   certificateRawData: Bayt dizisine bir X.509 sertifikası gösterimi. Bayt dizisi olarak kodlanmış ikili (DER) veya Base64 ile kodlanmış X.509 veri olabilir.
- - -
### <a name="isempty"></a>IsEmpty
**Açıklama:**  
Öznitelik CS veya MV var ancak boş bir dize olarak değerlendirir, IsEmpty işlevi True olarak değerlendirilir.

**Sözdizimi:**  
`bool IsEmpty(var Expression)`

- - -
### <a name="isguid"></a>IsGuid
**Açıklama:**  
Dize için bir GUID dönüştürülebilecek, IsGuid işlevi true olarak değerlendirilir.

**Sözdizimi:**  
`bool IsGuid(str GUID)`

**Notlar:**  
Bir GUID bu desenleri birini izleyen bir dize olarak tanımlanır: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx veya {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

CGuid() başarılı olup olmadığını belirlemek için kullanılır.

**Örnek:**  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
GUID biçimi StrAttribute varsa, bir ikili biçimi döndürür, aksi takdirde null değeri döndürür.

- - -
### <a name="isnull"></a>IsNull
**Açıklama:**  
İfade Null olarak değerlendirilirse, IsNull işlevi true döndürür.

**Sözdizimi:**  
`bool IsNull(var Expression)`

**Notlar:**  
Bir öznitelik için bir Null öznitelik yokluğu tarafından ifade edilir.

**Örnek:**  
`IsNull([displayName])`  
Öznitelik CS veya MV mevcut değilse True değerini döndürür.

- - -
### <a name="isnullorempty"></a>IsNullOrEmpty
**Açıklama:**  
İfade null veya boş bir dize ise, IsNullOrEmpty işlevi true döndürür.

**Sözdizimi:**  
`bool IsNullOrEmpty(var Expression)`

**Notlar:**  
Özniteliği yok veya var ancak boş bir dize için bir öznitelik, bu True olarak değerlendirilmesi.  
Bu işlevinin olmasına adlandırılır.

**Örnek:**  
`IsNullOrEmpty([displayName])`  
Özniteliği mevcut değil ya da boş bir dize CS veya MV varsa True değerini döndürür.

- - -
### <a name="isnumeric"></a>IsNumeric
**Açıklama:**  
IsNumeric işlevi bir ifadenin sayı türü değerlendirilebilir olup olmadığını gösteren bir Boole değeri döndürür.

**Sözdizimi:**  
`bool IsNumeric(var Expression)`

**Notlar:**  
CNum() ifade ayrıştırma başarılı olup olmadığını belirlemek için kullanılır.

- - -
### <a name="isstring"></a>IsString
**Açıklama:**  
İfade bir dize türü değerlendirilebilir, IsString işlevi True olarak değerlendirilir.

**Sözdizimi:**  
`bool IsString(var expression)`

**Notlar:**  
CStr() ifade ayrıştırma başarılı olup olmadığını belirlemek için kullanılır.

- - -
### <a name="ispresent"></a>Olmasına
**Açıklama:**  
İfade boş değil ve Null olmayan bir dize olarak değerlendirilirse, olmasına işlevi true döndürür.

**Sözdizimi:**  
`bool IsPresent(var expression)`

**Notlar:**  
Bu işlevinin IsNullOrEmpty olarak adlandırılır.

**Örnek:**  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

- - -
### <a name="item"></a>Öğe
**Açıklama:**  
Item işlevi, birden çok değerli bir dize/özniteliğinden bir öğeyi döndürür.

**Sözdizimi:**  
`var Item(mvstr attribute, num index)`

* Öznitelik: birden çok değerli özniteliği
* Dizin: birden çok değerli dize içindeki bir öğenin dizini.

**Notlar:**  
İkinci işlevi bir öğede birden çok değerli özniteliği için dizin döndürdüğünden öğesi işlevi içerir işlevi ile birlikte yararlıdır.

Dizin sınırların dışında olması durumunda bir hata oluşturur.

**Örnek:**  
`Mid(Item([proxyAddresses],Contains([proxyAddresses], "SMTP:")),6)`  
Birincil e-posta adresini döndürür.

- - -
### <a name="itemornull"></a>ItemOrNull
**Açıklama:**  
ItemOrNull işlevi birden çok değerli bir dize/özniteliğinden bir öğe döndürür.

**Sözdizimi:**  
`var ItemOrNull(mvstr attribute, num index)`

* Öznitelik: birden çok değerli özniteliği
* Dizin: birden çok değerli dize içindeki bir öğenin dizini.

**Notlar:**  
İkinci işlevi bir öğede birden çok değerli özniteliği için dizin döndürdüğünden ItemOrNull işlevi ile birlikte içerir işlevi yararlıdır.

Dizin sınırların dışında ise, bir Null değeri döndürür.

- - -
### <a name="join"></a>Birleştir
**Açıklama:**  
Birleştirme işlevi, birden çok değerli bir dize alır ve her bir öğe eklenen belirtilen ayırıcı ile tek değerli bir dize döndürür.

**Sözdizimi:**  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

* Öznitelik: birleştirilecek dizeler içeren birden çok değerli özniteliği.
* sınırlayıcı: döndürülen dize içinde alt dizeler ayırmak için kullanılan herhangi bir dize. Atlanırsa, boşluk karakteri ("") kullanılır. Sınırlayıcı sıfır uzunlukta bir dize ise ("") veya hiçbir şey, listedeki tüm öğelerin hiçbir sınırlayıcıları ile birleşir.

**Açıklamalar**  
Katılma ve bölünmüş işlevleri arasında eşlik bulunur. Birleştirme işlevi bir dizeler dizisi alır ve bunları tek bir dize döndürmek için bir sınırlayıcı dize kullanarak birleştirir. Bölünmüş işlev bir dize alır ve bir dizeler dizisi döndürmek için sınırlayıcı ayırır. Ancak, bir anahtar farktır birleştirme sınırlayıcı dizesiyle dizeyi birleştirmek, bölme yalnızca bir tek karakter ayırıcısı kullanarak dizeleri ayırabilirsiniz.

**Örnek:**  
`Join([proxyAddresses],",")`  
Döndürebilirsiniz: "SMTP:john.doe@contoso.com,smtp:jd@contoso.com"

- - -
### <a name="lcase"></a>LCase
**Açıklama:**  
LCase işlev bir dize içindeki tüm karakterleri küçük harflere dönüştürür.

**Sözdizimi:**  
`str LCase(str value)`

**Örnek:**  
`LCase("TeSt")`  
"Test" döndürür.

- - -
### <a name="left"></a>Sol
**Açıklama:**  
Sol işlevi bir dizenin soldan belirtilen sayıda karakteri döndürür.

**Sözdizimi:**  
`str Left(str string, num NumChars)`

* dize: dize karakterlerinden döndürmek için
* NumChars: (sol) dizesi başlangıçtan itibaren döndürülecek karakter sayısını tanımlayan bir numara

**Notlar:**  
Dizedeki ilk numChars karakter içeren bir dize:

* Varsa numChars = 0, boş bir dize döndürür.
* NumChars < 0, döndürmesi durumunda giriş dizesi.
* Dize null ise, boş bir dize döndürür.

Sayı belirtilen numChars'den daha az karakter dizesini içeren bir dize (parametre 1'deki tüm karakterleri içeren) dizeye aynı döndürülür.

**Örnek:**  
`Left("John Doe", 3)`  
"Joh" döndürür.

- - -
### <a name="len"></a>Len
**Açıklama:**  
Len işlevi bir dizedeki karakter sayısını döndürür.

**Sözdizimi:**  
`num Len(str value)`

**Örnek:**  
`Len("John Doe")`  
8 döndürür

- - -
### <a name="ltrim"></a>LTrim
**Açıklama:**  
LTrim işlevi bir dizeden öndeki boşlukları kaldırır.

**Sözdizimi:**  
`str LTrim(str value)`

**Örnek:**  
`LTrim(" Test ")`  
"Test" döndürür

- - -
### <a name="mid"></a>Orta
**Açıklama:**  
PARÇAAL işlevi bir dizedeki belirtilen konumdan belirtilen sayıda karakteri döndürür.

**Sözdizimi:**  
`str Mid(str string, num start, num NumChars)`

* dize: dize karakterlerinden döndürmek için
* Başlat: Başlangıç tanımlayan bir numara getirin dizesindeki karakterlerinden döndürmek için
* NumChars: dizedeki döndürmek için karakter sayısını tanımlayan bir numara

**Notlar:**  
Konumundan başlayan dönüş numChars karakter dizesi içinde başlatın.  
Konumu başlangıç dizesinde numChars karakterler içeren bir dize:

* Varsa numChars = 0, boş bir dize döndürür.
* NumChars < 0, döndürmesi durumunda giriş dizesi.
* Başlat > dize uzunluğu giriş dizesi döndürür.
* Varsa Başlat < = 0, giriş dizesi döndürür.
* Dize null ise, boş bir dize döndürür.

NumChar karakter değilse kadar çok konumu başından dizesinde kalan karakterler mümkün olduğunca döndürülür.

**Örnek:**  
`Mid("John Doe", 3, 5)`  
"Hn" döndürür.

`Mid("John Doe", 6, 999)`  
"Doe" döndürür

- - -
### <a name="now"></a>Şimdi
**Açıklama:**  
Şimdi işlevi geçerli tarih ve saat, bilgisayarınızın sistem tarihi ve saati göre belirten bir DateTime döndürür.

**Sözdizimi:**  
`dt Now()`

- - -
### <a name="numfromdate"></a>NumFromDate
**Açıklama:**  
NumFromDate işlevi bir tarih Reklamın tarih biçiminde döndürür.

**Sözdizimi:**  
`num NumFromDate(dt value)`

**Örnek:**  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
129699324000000000 döndürür

- - -
### <a name="padleft"></a>PadLeft
**Açıklama:**  
PadLeft işlevi sol-klavye takımı sağlanan doldurma karakteri kullanarak belirtilen bir süre için dize.

**Sözdizimi:**  
`str PadLeft(str string, num length, str padCharacter)`

* dize: doldurulacak dize.
* Uzunluk: istenen dize uzunluğu temsil eden bir tamsayı.
* padCharacter: doldurma karakteri olarak kullanmak üzere tek bir karakter içeren bir dize

**Notlar:**

* Dize uzunluğu uzunluğundan az ise, ardından padCharacter art arda (bir uzunluk olana kadar Dizi uzunluğuna eşit sol) başına eklenir.
* padCharacter bir boşluk karakteri olabilir, ancak bir null değer olamaz.
* Dize uzunluğu eşit veya uzunluğundan büyük ise, dize değişmeden döndürülür.
* Dizeye özdeş bir dize, dize uzunluğu eşit veya daha büyük bir uzunluk varsa, döndürülür.
* Dize uzunluğu uzunluğundan az ise, yeni bir dize istenen uzunluğu padCharacter ile doldurulan içeren dize döndürülür.
* Dize null ise, işlevi boş bir dize döndürür.

**Örnek:**  
`PadLeft("User", 10, "0")`  
"000000User" döndürür.

- - -
### <a name="padright"></a>PadRight
**Açıklama:**  
PadRight işlevi sağ-klavye takımı sağlanan doldurma karakteri kullanarak belirtilen bir süre için dize.

**Sözdizimi:**  
`str PadRight(str string, num length, str padCharacter)`

* dize: doldurulacak dize.
* Uzunluk: istenen dize uzunluğu temsil eden bir tamsayı.
* padCharacter: doldurma karakteri olarak kullanmak üzere tek bir karakter içeren bir dize

**Notlar:**

* Dize uzunluğu uzunluğundan az ise, bir uzunluk uzunluğa eşit olana kadar sonra padCharacter art arda sonuna dize (sağdaki) eklenir.
* padCharacter bir boşluk karakteri olabilir, ancak bir null değer olamaz.
* Dize uzunluğu eşit veya uzunluğundan büyük ise, dize değişmeden döndürülür.
* Dizeye özdeş bir dize, dize uzunluğu eşit veya daha büyük bir uzunluk varsa, döndürülür.
* Dize uzunluğu uzunluğundan az ise, yeni bir dize istenen uzunluğu padCharacter ile doldurulan içeren dize döndürülür.
* Dize null ise, işlevi boş bir dize döndürür.

**Örnek:**  
`PadRight("User", 10, "0")`  
"User000000" döndürür.

- - -
### <a name="pcase"></a>PCase
**Açıklama:**  
PCase işlevi her boşlukla sözcüğün bir dizede ilk karakteri büyük harflere dönüştürür ve diğer tüm karakterleri küçük harfe dönüştürülür.

**Sözdizimi:**  
`String PCase(string)`

**Notlar:**

* Bu işlev, şu anda bir kısaltma gibi tamamen büyük bir sözcük dönüştürmek için doğru büyük/küçük harf sağlamaz.

**Örnek:**  
`PCase("TEsT")`  
"Test" döndürür.

`PCase(LCase("TEST"))`  
"Test" döndürür

- - -
### <a name="randomnum"></a>RandomNum
**Açıklama:**  
RandomNum işlevi belirtilen bir zaman aralığı arasında rastgele bir sayı döndürür.

**Sözdizimi:**  
`num RandomNum(num start, num end)`

* Başlat: Oluşturulacak rastgele değer alt sınırı tanımlayan bir numara
* Son: Oluşturulacak rastgele değer sayısı üst sınırı tanımlayan bir numara

**Örnek:**  
`Random(100,999)`  
734 döndürebilir.

- - -
### <a name="removeduplicates"></a>RemoveDuplicates
**Açıklama:**  
RemoveDuplicates işlevi, birden çok değerli bir dize alır ve her değerin benzersiz olduğundan emin olun.

**Sözdizimi:**  
`mvstr RemoveDuplicates(mvstr attribute)`

**Örnek:**  
`RemoveDuplicates([proxyAddresses])`  
Burada tüm yinelenen değerleri kaldırılmış bir ayıklanmış proxyAddress özniteliği döndürür.

- - -
### <a name="replace"></a>Değiştir
**Açıklama:**  
Replace işlevi başka bir dizeye bir dizenin tüm oluşumlarını değiştirir.

**Sözdizimi:**  
`str Replace(str string, str OldValue, str NewValue)`

* dize: bir dize değerleri değiştirin.
* OldValue: Dize aramak ve değiştirmek için.
* NewValue: için değiştirilecek dize.

**Notlar:**  
İşlevi aşağıdaki özel adlar tanır:

* \n – yeni satır
* \r – satır başı
* \t – sekmesi

**Örnek:**  
`Replace([address],"\r\n",", ")`  
CRLF virgül ve boşluk ile değiştirir ve "Bir Microsoft yolu, Redmond, WA, ABD için" neden olabilir

- - -
### <a name="replacechars"></a>ReplaceChars
**Açıklama:**  
ReplaceChars işlevi ReplacePattern dizesinde bulunan karakterleri tüm oluşumlarını değiştirir.

**Sözdizimi:**  
`str ReplaceChars(str string, str ReplacePattern)`

* dize: karakter değiştirmek için bir dize.
* ReplacePattern: değiştirilecek karakterler içeren bir sözlük içeren bir dize.

{Kaynak1} biçimidir: {target1}, {kaynak2}: {target2}, {kaynakN}, {targetN} kaynağı bulmak ve değiştirmek için dize hedeflemek için karakter olduğu.

**Notlar:**

* İşlev her oluşumu tanımlanan kaynakları alır ve bunları hedefleri ile değiştirir.
* Kaynak tam olarak bir (unicode) karakter olmalıdır.
* Kaynak boş veya bir karakter (ayrıştırma hatası) daha uzun olamaz.
* Hedef birden çok karakter, örneğin ö:oe, β:ss olabilir.
* Hedef karakter kaldırılması gerektiğini belirten boş olabilir.
* Kaynak küçük harfe duyarlıdır ve tam bir eşleşme olmalıdır.
* , (Virgül) ve: (iki nokta üst üste) ayrılmış karakterler ve bu işlevi kullanılarak değiştirilemez.
* Alanları ve diğer beyaz karakterleri ReplacePattern dizesinde göz ardı edilir.

**Örnek:**  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
Raksmorgas döndürür

`ReplaceChars("O’Neil",%ReplaceString%)`  
Tek değer çizgilerinin "ONeil" döndürür kaldırılacak tanımlanır.

- - -
### <a name="right"></a>Sağ
**Açıklama:**  
Right işlevi sağdan dizenin (Bitiş) belirtilen sayıda karakteri döndürür.

**Sözdizimi:**  
`str Right(str string, num NumChars)`

* dize: dize karakterlerinden döndürmek için
* NumChars: dizesinin sonundan başlayarak (sağdaki) döndürülecek karakterlerin sayısı tanımlayan bir numara

**Notlar:**  
NumChars karakter dizesi son konumundan döndürülür.

Dizedeki son numChars karakter içeren bir dize:

* Varsa numChars = 0, boş bir dize döndürür.
* NumChars < 0, döndürmesi durumunda giriş dizesi.
* Dize null ise, boş bir dize döndürür.

Dize sayı belirtilen NumChars daha az karakterden içeriyorsa, dizeye özdeş bir dize döndürdü.

**Örnek:**  
`Right("John Doe", 3)`  
"Doe" döndürür.

- - -
### <a name="rtrim"></a>RTrim
**Açıklama:**  
RTrim işlevi bir dizeden sondaki boşluk kaldırır.

**Sözdizimi:**  
`str RTrim(str value)`

**Örnek:**  
`RTrim(" Test ")`  
"Test" döndürür.

- - -
### <a name="select"></a>Şunu seçin:
**Açıklama:**  
Birden çok değerli bir öznitelik (veya bir ifade çıktısını) içindeki tüm değerleri belirtilen işlevine dayalı işlemi.

**Sözdizimi:**  
`mvattr Select(variable item, mvattr attribute, func function)`  
`mvattr Select(variable item, exp expression, func function)`

* Madde: birden çok değerli öznitelik bir öğeyi temsil eder
* Öznitelik: birden çok değerli özniteliği
* ifade: değerler koleksiyonu döndüren bir ifadeye
* koşul: öznitelik bir öğeyi işleyebilir işlevi

**Örnekler:**  
`Select($item,[otherPhone],Replace($item,"-",""))`  
Tire (-) kaldırıldıktan sonra birden çok değerli özniteliği otherPhone tüm değerleri döndürür.

- - -
### <a name="split"></a>Böl
**Açıklama:**  
Bölünmüş işlevi bir sınırlayıcı ile ayrılmış bir dize alır ve birden çok değerli bir dize kolaylaştırır.

**Sözdizimi:**  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

* değer: ayırmak için sınırlayıcı karakter dizesiyle.
* sınırlayıcı: tek bir ayırıcı olarak kullanılan karakter.
* sınır: en yüksek sayıda döndürebilir değeri.

**Örnek:**  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
ProxyAddress özniteliği için yararlı 2 öğeleri birden çok değerli bir dize döndürür.

- - -
### <a name="stringfromguid"></a>StringFromGuid
**Açıklama:**  
StringFromGuid işlev ikili GUID alır ve bir dizeye dönüştürür

**Sözdizimi:**  
`str StringFromGuid(bin GUID)`

- - -
### <a name="stringfromsid"></a>StringFromSid
**Açıklama:**  
StringFromSid işlev bir dize için bir güvenlik tanımlayıcısı içeren bir bayt dizisi dönüştürür.

**Sözdizimi:**  
`str StringFromSid(bin ObjectSID)`  

- - -
### <a name="switch"></a>Anahtar
**Açıklama:**  
Anahtar işlevini değerlendirilen koşullara göre tek bir değer döndürmek için kullanılır.

**Sözdizimi:**  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

* Expr: değerlendirmek istediğiniz değişken ifadesi.
* değer: karşılık gelen ifadesi True ise döndürülecek değer.

**Notlar:**  
Anahtar işlevi bağımsız değişken listesi ifadeleri ve değer çiftlerinden oluşur. İfadeler soldan sağa değerlendirilir ve True değerlendirileceği ilk ifade ile ilişkili değeri döndürülür. Bölümleri düzgün eşleştirilmedi, bir çalışma zamanı hatası oluşur.

Örneğin, Expr1 True ise, anahtar value1 döndürür. Yanlış ifade 1., ancak expr 2 True ise, anahtar değeri 2 vb. döndürür.

Anahtar döndüren bir hiçbir şeyin varsa:

* İfadelerden hiçbiri True olarak ayarlanır.
* İlk True ifade null karşılık gelen bir değer içeriyor.

Bunlardan yalnızca birini döndürür olsa bile anahtar tüm ifadeler değerlendirir. Bu nedenle, istenmeyen yan etkileri için izlemeniz gerekir. Örneğin, herhangi bir ifade Değerlendirme hatası bir bölme sonuçlanırsa, bir hata oluşur.

Değer, özel bir dize döndürür hata işlevi de olabilir.

**Örnek:**  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
Bazı önemli şehirlerde konuşma dilini döndürür, aksi takdirde bir hata döndürür.

- - -
### <a name="trim"></a>Kırpma
**Açıklama:**  
Kırpma işlevi baştaki ve sondaki boşlukları bir dizeden kaldırır.

**Sözdizimi:**  
`str Trim(str value)`  

**Örnek:**  
`Trim(" Test ")`  
"Test" döndürür.

`Trim([proxyAddresses])`  
Baştaki ve sondaki boşlukları proxyAddress özniteliğinde her bir değer için kaldırır.

- - -
### <a name="ucase"></a>UCase
**Açıklama:**  
UCase işlev bir dize içindeki tüm karakterleri büyük harfe dönüştürür.

**Sözdizimi:**  
`str UCase(str string)`

**Örnek:**  
`UCase("TeSt")`  
"TEST" döndürür.

- - -
### <a name="where"></a>Burada

**Açıklama:**  
Belirli bir koşula dayalı birden çok değerli özniteliği (veya bir ifade çıktısını) değerleri kümesini döndürür.

**Sözdizimi:**  
`mvattr Where(variable item, mvattr attribute, exp condition)`  
`mvattr Where(variable item, exp expression, exp condition)`  
* Madde: birden çok değerli öznitelik bir öğeyi temsil eder
* Öznitelik: birden çok değerli özniteliği
* koşul: true veya false sonucu verebilen herhangi bir ifade
* ifade: değerler koleksiyonu döndüren bir ifadeye

**Örnek:**  
`Where($item,[userCertificate],CertNotAfter($item)>Now())`  
Sertifika değerler, süresi dolmuş olmayan birden çok değerli özniteliği userCertificate döndürür.

- - -
### <a name="with"></a>Avantaj ile
**Açıklama:**  
WITH işlevi bir görünen bir alt temsil etmek için bir değişken kullanarak veya birden fazla kez karmaşık ifadesinde karmaşık bir ifade basitleştirmek için bir yol sağlar.

**Sözdizimi:**
`With(var variable, exp subExpression, exp complexExpression)`  
* değişken: alt temsil eder.
* Alt: değişkeni tarafından temsil edilen alt.
* complexExpression: karmaşık bir ifade.

**Örnek:**  
`With($unExpiredCerts,Where($item,[userCertificate],CertNotAfter($item)>Now()),IIF(Count($unExpiredCerts)>0,$unExpiredCerts,NULL))`  
İşlevsel olarak eşdeğerdir:  
`IIF (Count(Where($item,[userCertificate],CertNotAfter($item)>Now()))>0, Where($item,[userCertificate],CertNotAfter($item)>Now()),NULL)`  
Hangi kullanıcı sertifikasını özniteliğinde yalnızca süresi dolmamış sertifika değerleri döndürür.


- - -
### <a name="word"></a>Word
**Açıklama:**  
Word işlevi kullanın ve geri dönmek için word numarası sınırlayıcıları açıklayan parametreleri temel bir dize içindeki bir sözcük döndürür.

**Sözdizimi:**  
`str Word(str string, num WordNumber, str delimiters)`

* dize: bir sözcük döndürmek için dize.
* WordNumber: hangi word numarasını tanımlayan bir sayı döndürmelidir.
* Sınırlayıcılar: sözcükler tanımlamak için kullanılması gereken delimiter(s) temsil eden bir dize

**Notlar:**  
Her bir sınırlayıcı karakter biri ayırarak dizedeki karakter dizesini sözcükler olarak tanımlanır:

* Varsa < 1 sayı, boş dize döndürür.
* Dize null ise, boş bir dize döndürür.

Sözcük sayısı sayısından az dize içeriyor ya da dizesi sınırlayıcıları tarafından tanımlanan herhangi bir sözcük içermiyor, boş bir dize döndürdü.

**Örnek:**  
`Word("The quick brown fox",3," ")`  
"Kahverengi" döndürür

`Word("This,string!has&many separators",3,",!&#")`  
"Var" döndürür

## <a name="additional-resources"></a>Ek Kaynaklar
* [Bildirim temelli hazırlama ifadelerini anlama](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [Azure AD Connect eşitleme: Eşitleme seçeneklerini özelleştirme](active-directory-aadconnectsync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
