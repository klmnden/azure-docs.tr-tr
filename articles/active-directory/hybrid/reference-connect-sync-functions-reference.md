---
title: 'Azure AD Connect eşitleme: İşlevler başvurusu | Microsoft Docs'
description: Azure AD Connect eşitleme, bildirim temelli sağlama ifadelerini başvurusu.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 4f525ca0-be0e-4a2e-8da1-09b6b567ed5f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 07/12/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: b33e993dbddc9c1567a1a6f7d3dca28af240a000
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60381156"
---
# <a name="azure-ad-connect-sync-functions-reference"></a>Azure AD Connect eşitleme: İşlevler Başvurusu
Azure AD Connect, işlevleri, bir öznitelik değeri, eşitleme sırasında işlemek için kullanılır.  
İşlevler söz dizimi aşağıdaki biçimi kullanarak ifade edilir:  
`<output type> FunctionName(<input type> <position name>, ..)`

Bir işlev aşırı yüklendi ve birden çok sözdizimleri kabul eder, tüm geçerli sözdizimleri listelenir.  
İşlevleri kesin olarak belirlenmiştir ve türü belgelenmiş türü eşleşmelerin geçtiğini doğrulayın.  
Türü ile eşleşmiyorsa, bir hata oluşturulur.

Türleri aşağıdaki sözdizimiyle belirtilir:

* **Depo** – ikili
* **bool** : Boole
* **dt** – UTC tarihi/saati
* **Enum** – bilinen sabitleri, sabit listesi
* **exp** – bir Boole değeri değerlendirmek için beklenen ifadesi
* **mvbin** – birden çok değerli ikili
* **mvstr** – birden çok değerli dize
* **mvref** – birden çok değerli başvurusu
* **NUM** : sayısal
* **Ref** – başvurusu
* **str** – dize
* **Varyasyon** – (yaklaşık) herhangi bir başka tür bir değişken
* **void** – bir değer döndürmüyor

İşlevleri türleriyle **mvbin**, **mvstr**, ve **mvref** yalnızca birden çok değerli öznitelikler üzerinde çalışabilir. İçeren işlevler **bin**, **str**, ve **ref** tek değerli hem birden çok değerli öznitelikler üzerinde çalışır.

## <a name="functions-reference"></a>İşlevler Başvurusu

| İşlevlerin listesi |  |  |  |  |
| --- | --- | --- | --- | --- |
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
| [CRef](#cref) |[CStr](#cstr) |[StringFromGuid](#stringfromguid) |[StringFromSid](#stringfromsid) | |
| **Tarih / saat** | | | | |
| [DateAdd](#dateadd) |[DateFromNum](#datefromnum) |[formatDateTime](#formatdatetime) |[Şimdi](#now) | |
| [NumFromDate](#numfromdate) | | | | |
| **Dizin** | | | | |
| [DNComponent](#dncomponent) |[DNComponentRev](#dncomponentrev) |[EscapeDNComponent](#escapedncomponent) | | |
| **Değerlendirme** | | | | |
| [IsBitSet](#isbitset) |[IsDate](#isdate) |[IsEmpty](#isempty) |[IsGuid](#isguid) | |
| [IsNull](#isnull) |[IsNullOrEmpty](#isnullorempty) |[IsNumeric](#isnumeric) |[Olmasına](#ispresent) | |
| [IsString](#isstring) | | | | |
| **Math** | | | | |
| [BitAnd](#bitand) |[BitOr](#bitor) |[RandomNum](#randomnum) | | |
| **Birden çok değerli** | | | | |
| [içerir](#contains) |[Sayısı](#count) |[Öğesi](#item) |[ItemOrNull](#itemornull) | |
| [Birleştir](#join) |[RemoveDuplicates](#removeduplicates) |[Böl](#split) | | |
| **Program akışı** | | | | |
| [Hata:](#error) |[IIF](#iif) |[Seç](#select) |[Anahtar](#switch) | |
| [Burada](#where) |[ile](#with) | | | |
| **Text** | | | | |
| [GUID](#guid) |[InStr](#instr) |[InStrRev](#instrrev) |[LCase](#lcase) | |
| [Sol](#left) |[Len](#len) |[LTrim](#ltrim) |[Orta](#mid) | |
| [padLeft](#padleft) |[PadRight](#padright) |[PCase](#pcase) |[Değiştir](#replace) | |
| [ReplaceChars](#replacechars) |[sağ](#right) |[RTrim](#rtrim) |[Kırpma](#trim) | |
| [UCase](#ucase) |[Word](#word) | | | |

- - -
### <a name="bitand"></a>BitAnd
**Açıklama:**  
BitAnd işlevi, belirtilen BITS bir değere ayarlar.

**Sözdizimi:**  
`num BitAnd(num value1, num value2)`

* value1, value2: and ile bağlanan birlikte olmalıdır sayısal değerler

**Notlar:**  
Bu işlev, her iki parametre ikili gösterimine dönüştürür ve bir bit ayarlar:

* 0 - biri veya her ikisini karşılık gelen bitler *maskesi* ve *bayrağı* 0
* 1 - karşılık gelen bit hem de 1 olması gerekir.

Diğer bir deyişle, tüm durumlarda, her iki parametre karşılık gelen bitleri 1 olduğu durumlar dışında 0 döndürür.

**Örnek:**  
`BitAnd(&HF, &HF7)`  
7 onaltılık "F" ve "F7" değerlendirmek için bu değeri döndürür.

- - -
### <a name="bitor"></a>BitOr
**Açıklama:**  
BitOr işlevi, belirtilen BITS bir değere ayarlar.

**Sözdizimi:**  
`num BitOr(num value1, num value2)`

* value1, value2: or birlikte olmalıdır sayısal değerler

**Notlar:**  
Bu işlev, her iki parametre ikili gösterimine dönüştürür ve karşılık gelen bit hem de 0 ise birini veya her ikisini karşılık gelen bit maskesi ve bayrağı 1 ise 1 ve 0 için bir bit ayarlar. Diğer bir deyişle, her iki parametre karşılık gelen bitleri 0 olduğu dışındaki tüm durumlarda 1 döndürür.

- - -
### <a name="cbool"></a>CBool
**Açıklama:**  
CBool işlevi değerlendirilen ifadeye göre bir Boole değeri döndürür.

**Sözdizimi:**  
`bool CBool(exp Expression)`

**Notlar:**  
CBool True değerini döndürür, ardından ifade sıfır olmayan bir değer değerlendirilirse, başka False döndürür.

**Örnek:**  
`CBool([attrib1] = [attrib2])`  

Geri dönüş True her iki öznitelikleri aynı değere sahip.

- - -
### <a name="cdate"></a>CDate
**Açıklama:**  
CDate işlevi bir dizedeki bir UTC tarih/saati döndürür. DateTime eşitlenmiş bir yerel öznitelik türü değildir, ancak bazı işlevler tarafından kullanılır.

**Sözdizimi:**  
`dt CDate(str value)`

* Değer: Bir tarih, saat ve isteğe bağlı saat dilimi içeren bir dize

**Notlar:**  
Döndürülen dizeyi her zaman UTC ' dir.

**Örnek:**  
`CDate([employeeStartTime])`  
Başlangıç saati bir tarih/saat çalışanın tabanlı döndürür

`CDate("2013-01-10 4:00 PM -8")`  
Döndürür bir DateTime temsil eden "2013-01-11 12: 00'da"


- - -
### <a name="certextensionoids"></a>CertExtensionOids
**Açıklama:**  
Bir sertifika nesnesinin tüm kritik uzantılar OID değerlerini döndürür.

**Sözdizimi:**  
`mvstr CertExtensionOids(binary certificateRawData)`  
*   certificateRawData: X.509 sertifikası bayt dizisi gösterimi. Bayt dizisinin kodlanmış ikili (DER ile) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certformat"></a>CertFormat
**Açıklama:**  
Bu X.509v3 sertifikasını biçiminin adını döndürür.

**Sözdizimi:**  
`str CertFormat(binary certificateRawData)`  
*   certificateRawData: X.509 sertifikası bayt dizisi gösterimi. Bayt dizisinin kodlanmış ikili (DER ile) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certfriendlyname"></a>CertFriendlyName
**Açıklama:**  
Bir sertifika ilişkili diğer döndürür.

**Sözdizimi:**  
`str CertFriendlyName(binary certificateRawData)`  
*   certificateRawData: X.509 sertifikası bayt dizisi gösterimi. Bayt dizisinin kodlanmış ikili (DER ile) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certhashstring"></a>CertHashString
**Açıklama:**  
X.509v3 sertifikasını SHA1 karma değeri bir onaltılık dize olarak döndürür.

**Sözdizimi:**  
`str CertHashString(binary certificateRawData)`  
*   certificateRawData: X.509 sertifikası bayt dizisi gösterimi. Bayt dizisinin kodlanmış ikili (DER ile) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certissuer"></a>CertIssuer
**Açıklama:**  
X.509v3 sertifikasını veren sertifika yetkilisinin adını döndürür.

**Sözdizimi:**  
`str CertIssuer(binary certificateRawData)`  
*   certificateRawData: X.509 sertifikası bayt dizisi gösterimi. Bayt dizisinin kodlanmış ikili (DER ile) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certissuerdn"></a>CertIssuerDN
**Açıklama:**  
Sertifikayı verenin ayırt edici adını döndürür.

**Sözdizimi:**  
`str CertIssuerDN(binary certificateRawData)`  
*   certificateRawData: X.509 sertifikası bayt dizisi gösterimi. Bayt dizisinin kodlanmış ikili (DER ile) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certissueroid"></a>CertIssuerOid
**Açıklama:**  
OID, sertifikayı veren döndürür.

**Sözdizimi:**  
`str CertIssuerOid(binary certificateRawData)`  
*   certificateRawData: X.509 sertifikası bayt dizisi gösterimi. Bayt dizisinin kodlanmış ikili (DER ile) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certkeyalgorithm"></a>CertKeyAlgorithm
**Açıklama:**  
Bu X.509v3 sertifikasını anahtar algoritması bilgilerini bir dize olarak döndürür.

**Sözdizimi:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   certificateRawData: X.509 sertifikası bayt dizisi gösterimi. Bayt dizisinin kodlanmış ikili (DER ile) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certkeyalgorithmparams"></a>CertKeyAlgorithmParams
**Açıklama:**  
Anahtar algoritması parametreleri X.509v3 sertifikasını için bir onaltılık dize olarak döndürür.

**Sözdizimi:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   certificateRawData: X.509 sertifikası bayt dizisi gösterimi. Bayt dizisinin kodlanmış ikili (DER ile) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certnameinfo"></a>CertNameInfo
**Açıklama:**  
Konu ve sertifikayı veren sertifika adlarını döndürür.

**Sözdizimi:**  
`str CertNameInfo(binary certificateRawData, str x509NameType, bool includesIssuerName)`  
*   certificateRawData: X.509 sertifikası bayt dizisi gösterimi. Bayt dizisinin kodlanmış ikili (DER ile) veya Base64 ile kodlanmış X.509 veri olabilir.
*   X509NameType: Konu X509NameType değeri.
*   includesIssuerName: verenin adı; eklemek için true Aksi takdirde false.

- - -
### <a name="certnotafter"></a>CertNotAfter
**Açıklama:**  
Sonra sertifika artık geçerli olmayan yerel saatle tarihi döndürür.

**Sözdizimi:**  
`dt CertNotAfter(binary certificateRawData)`  
*   certificateRawData: X.509 sertifikası bayt dizisi gösterimi. Bayt dizisinin kodlanmış ikili (DER ile) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certnotbefore"></a>CertNotBefore
**Açıklama:**  
Yerel saatle bir sertifikanın geçerli hale geldiği tarihi döndürür.

**Sözdizimi:**  
`dt CertNotBefore(binary certificateRawData)`  
*   certificateRawData: X.509 sertifikası bayt dizisi gösterimi. Bayt dizisinin kodlanmış ikili (DER ile) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certpublickeyoid"></a>CertPublicKeyOid
**Açıklama:**  
Ortak anahtarın X.509v3 sertifikasını için OID döndürür.

**Sözdizimi:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   certificateRawData: X.509 sertifikası bayt dizisi gösterimi. Bayt dizisinin kodlanmış ikili (DER ile) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certpublickeyparametersoid"></a>CertPublicKeyParametersOid
**Açıklama:**  
Ortak anahtar parametreleri X.509v3 sertifikasını OID döndürür.

**Sözdizimi:**  
`str CertPublicKeyParametersOid(binary certificateRawData)`  
*   certificateRawData: X.509 sertifikası bayt dizisi gösterimi. Bayt dizisinin kodlanmış ikili (DER ile) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certserialnumber"></a>CertSerialNumber
**Açıklama:**  
X.509v3 sertifikasını seri numarasını döndürür.

**Sözdizimi:**  
`str CertSerialNumber(binary certificateRawData)`  
*   certificateRawData: X.509 sertifikası bayt dizisi gösterimi. Bayt dizisinin kodlanmış ikili (DER ile) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certsignaturealgorithmoid"></a>CertSignatureAlgorithmOid
**Açıklama:**  
Bir sertifikanın imzasını oluşturmak için kullanılan algoritma OID döndürür.

**Sözdizimi:**  
`str CertSignatureAlgorithmOid(binary certificateRawData)`  
*   certificateRawData: X.509 sertifikası bayt dizisi gösterimi. Bayt dizisinin kodlanmış ikili (DER ile) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certsubject"></a>CertSubject
**Açıklama:**  
Bir sertifikadan konu ayırt edici adını alır.

**Sözdizimi:**  
`str CertSubject(binary certificateRawData)`  
*   certificateRawData: X.509 sertifikası bayt dizisi gösterimi. Bayt dizisinin kodlanmış ikili (DER ile) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certsubjectnamedn"></a>CertSubjectNameDN
**Açıklama:**  
Bir sertifikadan konu ayırt edici adını döndürür.

**Sözdizimi:**  
`str CertSubjectNameDN(binary certificateRawData)`  
*   certificateRawData: X.509 sertifikası bayt dizisi gösterimi. Bayt dizisinin kodlanmış ikili (DER ile) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certsubjectnameoid"></a>CertSubjectNameOid
**Açıklama:**  
Bir sertifika konu adının OID döndürür.

**Sözdizimi:**  
`str CertSubjectNameOid(binary certificateRawData)`  
*   certificateRawData: X.509 sertifikası bayt dizisi gösterimi. Bayt dizisinin kodlanmış ikili (DER ile) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certthumbprint"></a>Certthumbprınt
**Açıklama:**  
Bir sertifikanın parmak izini döndürür.

**Sözdizimi:**  
`str CertThumbprint(binary certificateRawData)`  
*   certificateRawData: X.509 sertifikası bayt dizisi gösterimi. Bayt dizisinin kodlanmış ikili (DER ile) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="certversion"></a>CertVersion
**Açıklama:**  
Bir sertifikanın X.509 biçimindeki sürümü döndürür.

**Sözdizimi:**  
`str CertThumbprint(binary certificateRawData)`  
*   certificateRawData: X.509 sertifikası bayt dizisi gösterimi. Bayt dizisinin kodlanmış ikili (DER ile) veya Base64 ile kodlanmış X.509 veri olabilir.

- - -
### <a name="cguid"></a>CGuid
**Açıklama:**  
CGuid işlevi bir GUID dize gösterimini ikili gösterimine dönüştürür.

**Sözdizimi:**  
`bin CGuid(str GUID)`

* Bu düzende biçimlendirilmiş bir dize: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx veya {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

- - -
### <a name="contains"></a>İçerir
**Açıklama:**  
Contains işlevi, birden çok değerli bir özniteliği içindeki bir dizeyle bulur.

**Sözdizimi:**  
`num Contains (mvstring attribute, str search)` -büyük küçük harfe duyarlı  
`num Contains (mvstring attribute, str search, enum Casetype)`  
`num Contains (mvref attribute, str search)` -büyük küçük harfe duyarlı

* Öznitelik: aramak için birden çok değerli öznitelik.
* Arama: öznitelik bulunacak dize.
* Casetype: CaseInsensitive veya CaseSensitive.

Birden çok değerli öznitelik dize bulunduğu dizin değerini döndürür. Dize bulunamazsa 0 döndürülür.

**Notlar:**  
Birden çok değerli dize öznitelikleri için arama alt dize değerleri bulur.  
Başvuru öznitelikleri için Aranan dize değeri bir eşleşme olarak kabul edilmesi için tam olarak eşleşmelidir.

**Örnek:**  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
Bir birincil e-posta adresi proxyAddresses özniteliğine sahipse, (büyük harf tarafından gösterilen "SMTP:"), ardından proxyAddress özniteliği döndürür, aksi takdirde bir hata döndürebilir.

- - -
### <a name="convertfrombase64"></a>ConvertFromBase64
**Açıklama:**  
ConvertFromBase64 işlevi, belirtilen base64 kodlu değer normal bir dizeye dönüştürür.

**Sözdizimi:**  
`str ConvertFromBase64(str source)` -Unicode kodlama için varsayar.  
`str ConvertFromBase64(str source, enum Encoding)`

* Kaynak: Base64 ile kodlanmış dize  
* Kodlama: Unicode, ASCII, UTF8

**Örnek**  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

Örneklerin her ikisi de döndürür "*Merhaba Dünya!* "

- - -
### <a name="convertfromutf8hex"></a>ConvertFromUTF8Hex
**Açıklama:**  
ConvertFromUTF8Hex işlevi, belirtilen UTF8 kodlu onaltılık değerin bir dizeye dönüştürür.

**Sözdizimi:**  
`str ConvertFromUTF8Hex(str source)`

* Kaynak: UTF8 2 baytlık kodlu dizesi

**Notlar:**  
Bu işlev ve sonucu DN özniteliği için kolay olduğunu ConvertFromBase64([],UTF8) içinde arasındaki fark.  
Bu biçim, Azure Active Directory tarafından DN kullanılır.

**Örnek:**  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
Döndürür "*Merhaba Dünya!* "

- - -
### <a name="converttobase64"></a>ConvertToBase64
**Açıklama:**  
ConvertToBase64 işlevi bir dize Unicode base64 dizesine dönüştürür.  
Tamsayı dizisi değeri taban 64 basamak ile kodlanmış eşdeğer dize gösterimine dönüştürür.

**Sözdizimi:**  
`str ConvertToBase64(str source)`

**Örnek:**  
`ConvertToBase64("Hello world!")`  
Returns "SABlAGwAbABvACAAdwBvAHIAbABkACEA"

- - -
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex
**Açıklama:**  
ConvertToUTF8Hex işlevi bir dize UTF8 kodlu onaltılı değerine dönüştürür.

**Sözdizimi:**  
`str ConvertToUTF8Hex(str source)`

**Notlar:**  
Çıktı biçimine göre bu işlev, Azure Active Directory tarafından DN öznitelik biçimi kullanılır.

**Örnek:**  
`ConvertToUTF8Hex("Hello world!")`  
48656C6C6F20776F726C6421 döndürür

- - -
### <a name="count"></a>Sayı
**Açıklama:**  
Sayım işlevi birden çok değerli bir özniteliği öğelerin sayısını döndürür.

**Sözdizimi:**  
`num Count(mvstr attribute)`

- - -
### <a name="cnum"></a>CNum
**Açıklama:**  
CNum işlevi bir dize alır ve bir sayısal veri türü döndürür.

**Sözdizimi:**  
`num CNum(str value)`

- - -
### <a name="cref"></a>CRef
**Açıklama:**  
Bir başvuru özniteliği için bir dize dönüştürür

**Sözdizimi:**  
`ref CRef(str value)`

**Örnek:**  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

- - -
### <a name="cstr"></a>CStr
**Açıklama:**  
CStr işlevi bir dize veri türüne dönüştürür.

**Sözdizimi:**  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

* Değer: Bir sayısal değer, başvuru özniteliği ya da Boole değeri olabilir.

**Örnek:**  
`CStr([dn])`  
Döndürebilir "cn = Joe, dc = contoso, dc = com"

- - -
### <a name="dateadd"></a>DateAdd
**Açıklama:**  
Belirtilen bir zaman aralığına eklenmiş olan bir tarih içeren bir tarih döndürür.

**Sözdizimi:**  
`dt DateAdd(str interval, num value, dt date)`

* aralığı: Eklemek istediğiniz zaman aralığı dize ifade. Dizenin şu değerlerden birine sahip olmalıdır:
  * yyyy yıl
  * q Çeyrek
  * milyon ay
  * y yılın günü
  * Günlük d
  * w haftanın günü
  * ww hafta
  * h Saat
  * n dakika
  * s ikinci
* Değer: Eklemek istediğiniz birim sayısı. (Gelecek tarihler almak için) artı veya eksi (geçmişteki tarihler almak için) olabilir.
* Tarihi: Aralık ekleneceği tarihi temsil eden tarih ve saat.

**Örnek:**  
`DateAdd("m", 3, CDate("2001-01-01"))`  
3 ay ekler ve bir DateTime temsil eden "2001-04-01" döndürür.

- - -
### <a name="datefromnum"></a>DateFromNum
**Açıklama:**  
AD'ın tarih değeri, DateTime türüne biçimlendirmek DateFromNum işlevi dönüştürür.

**Sözdizimi:**  
`dt DateFromNum(num value)`

**Örnek:**  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
2012-01-01 temsil eden bir DateTime döndürür 23:00:00

- - -
### <a name="dncomponent"></a>DNComponent
**Açıklama:**  
DNComponent işlevi soldan giderek belirtilen DN bileşen değerini döndürür.

**Sözdizimi:**  
`str DNComponent(ref dn, num ComponentNumber)`

* DN: yorumlamak için başvuru özniteliği
* ComponentNumber: Döndürülecek DN bileşeni

**Örnek:**  
`DNComponent(CRef([dn]),1)`  
DN ise "CN = Joe, ou = =..." ALi döndürür

- - -
### <a name="dncomponentrev"></a>DNComponentRev
**Açıklama:**  
DNComponentRev işlevi, belirtilen DN bileşen gidip sağ taraftan (Bitiş) değerini döndürür.

**Sözdizimi:**  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

* DN: yorumlamak için başvuru özniteliği
* ComponentNumber - döndürülecek DN bileşeni
* Seçenekler: DC – tüm bileşenlerle Yoksay "dc ="

**Örnek:**  
DN ise "CN = Joe, ou = Atlanta, ou = GA, ou = = US, dc = contoso, dc = com" ardından  
`DNComponentRev(CRef([dn]),3)`  
`DNComponentRev(CRef([dn]),1,"DC")`  
Her ikisi de BİZE döndürür.

- - -
### <a name="error"></a>Hata
**Açıklama:**  
Hata işlevi, özel bir hata döndürmek için kullanılır.

**Sözdizimi:**  
`void Error(str ErrorMessage)`

**Örnek:**  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
Öznitelik accountName mevcut değilse, nesne üzerinde bir hata atar.

- - -
### <a name="escapedncomponent"></a>EscapeDNComponent
**Açıklama:**  
EscapeDNComponent işlevi, tek bir DN bileşeninin alır ve LDAP'de temsil edilmesi için bu çıkışları.

**Sözdizimi:**  
`str EscapeDNComponent(str value)`

**Örnek:**  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
DisplayName özniteliğini LDAP'de atlanması gereken karakterler olsa bile, bir LDAP dizininde nesne oluşturulabilir emin olur.

- - -
### <a name="formatdatetime"></a>formatDateTime
**Açıklama:**  
FormatDateTime işlevi DateTime bir dize olarak belirtilen biçimiyle için kullanılır

**Sözdizimi:**  
`str FormatDateTime(dt value, str format)`

* değer: tarih saat biçiminde bir değer
* Format: biçim dönüştürme temsil eden bir dize.

**Notlar:**  
Biçim için olası değerler burada bulunabilir: [Özel tarih ve saat biçimleri için biçimlendirme işlevinde](https://docs.microsoft.com/dax/custom-date-and-time-formats-for-the-format-function).

**Örnek:**  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
"2007-12-25" sonuçlanır.

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
"İçinde 20140905081453.0Z" neden olabilir

- - -
### <a name="guid"></a>Guid
**Açıklama:**  
' % S'işlevi GUID yeni, rastgele bir GUID oluşturur

**Sözdizimi:**  
`str Guid()`

- - -
### <a name="iif"></a>IIF
**Açıklama:**  
IIf işlevi olası değerleri belirtilen bir koşulu temel alarak bir dizi döndürür.

**Sözdizimi:**  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

* koşul: herhangi bir değer veya true veya false sonucu verebilen bir ifade.
* Koşul: Döndürülen değeri true olarak değerlendirilen koşul yoksa.
* valueIfFalse: Koşul false, döndürülen değer olarak değerlendirilirse.

**Örnek:**  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 Kullanıcı bir stajyeri ise, "t-", başka başlangıcını eklenen sahip bir kullanıcı diğer adı olduğu gibi kullanıcının diğer adı döndürür.

- - -
### <a name="instr"></a>InStr
**Açıklama:**  
InStr işlevi bir dizedeki bir alt dizenin ilk geçtiği yeri bulur

**Sözdizimi:**  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

* stringcheck: aranacak dize
* stringmatch: bulunacak dize
* Başlat: başlama konumu alt dizeyi bulmak için
* karşılaştırma: vbTextCompare veya vbBinaryCompare

**Notlar:**  
Burada alt dizeyi bulunamadı veya 0 ise nebyl nalezen konumunu döndürür.

**Örnek:**  
`InStr("The quick brown fox","quick")`  
Evalues 5

`InStr("repEated","e",3,vbBinaryCompare)`  
7 olarak değerlendirir.

- - -
### <a name="instrrev"></a>InStrRev
**Açıklama:**  
InStrRev işlevi bir dizedeki bir alt dizenin son oluşumunu bulur

**Sözdizimi:**  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

* stringcheck: aranacak dize
* stringmatch: bulunacak dize
* Başlat: başlama konumu alt dizeyi bulmak için
* karşılaştırma: vbTextCompare veya vbBinaryCompare

**Notlar:**  
Burada alt dizeyi bulunamadı veya 0 ise nebyl nalezen konumunu döndürür.

**Örnek:**  
`InStrRev("abbcdbbbef","bb")`  
7'yi döndürür

- - -
### <a name="isbitset"></a>IsBitSet
**Açıklama:**  
' % S'işlevi bir bit varsa IsBitSet testleri ayarlanıp

**Sözdizimi:**  
`bool IsBitSet(num value, num flag)`

* değer: evaluated.flag bir sayısal değer: bit uyumluluğunun değerlendirilebilmesi için sahip bir sayısal değer

**Örnek:**  
`IsBitSet(&HF,4)`  
"4" bit onaltılık değer "F" olarak ayarlandığından sahipse True değerini döndürür

- - -
### <a name="isdate"></a>IsDate
**Açıklama:**  
Ardından IsDate işlev True olarak değerlendirilen bir ifade olabilir bir DateTime türü olarak değerlendirir.

**Sözdizimi:**  
`bool IsDate(var Expression)`

**Notlar:**  
CDate() başarılı olup olmadığını belirlemek için kullanılır.

- - -
### <a name="iscert"></a>IsCert
**Açıklama:**  
Ham verileri .NET X509Certificate2 sertifika nesnede seri hale getirilebilir true değerini döndürür.

**Sözdizimi:**  
`bool CertThumbprint(binary certificateRawData)`  
*   certificateRawData: X.509 sertifikası bayt dizisi gösterimi. Bayt dizisinin kodlanmış ikili (DER ile) veya Base64 ile kodlanmış X.509 veri olabilir.
- - -
### <a name="isempty"></a>IsEmpty
**Açıklama:**  
Öznitelik CS veya MV var ancak boş bir dize olarak değerlendirir, IsEmpty işlevi True olarak değerlendirilir.

**Sözdizimi:**  
`bool IsEmpty(var Expression)`

- - -
### <a name="isguid"></a>IsGuid
**Açıklama:**  
Dize GUİD'e dönüştürülemiyor, IsGuid işlev true olarak değerlendirdi.

**Sözdizimi:**  
`bool IsGuid(str GUID)`

**Notlar:**  
Bir GUID şu desenlerden birini uygulayarak bir dize olarak tanımlanır: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx veya {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

CGuid() başarılı olup olmadığını belirlemek için kullanılır.

**Örnek:**  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
GUID biçimi StrAttribute varsa bir ikili biçimi döndürür, aksi halde bir Null değer döndürür.

- - -
### <a name="isnull"></a>IsNull
**Açıklama:**  
İfade Null olarak değerlendirilirse, IsNull işlev true döndürür.

**Sözdizimi:**  
`bool IsNull(var Expression)`

**Notlar:**  
Bir öznitelik için bir Null öznitelik olmaması tarafından gösterilir.

**Örnek:**  
`IsNull([displayName])`  
Öznitelik CS veya MV mevcut değilse true değer döndürür.

- - -
### <a name="isnullorempty"></a>IsNullOrEmpty
**Açıklama:**  
İfade, null veya boş bir dize ise, IsNullOrEmpty işlev true döndürür.

**Sözdizimi:**  
`bool IsNullOrEmpty(var Expression)`

**Notlar:**  
Özniteliği eksik veya var ancak boş bir dizedir bir özniteliği için bunu True olarak değerlendirilmesi.  
Bu işlev tersini olmasına olarak adlandırılır.

**Örnek:**  
`IsNullOrEmpty([displayName])`  
Öznitelik mevcut değil veya boş bir dize CS veya MV varsa True değerini döndürür.

- - -
### <a name="isnumeric"></a>IsNumeric
**Açıklama:**  
IsNumeric işlevine bir ifade bir sayı türü olarak değerlendirilip değerlendirilmediğini gösteren bir Boole değeri döndürür.

**Sözdizimi:**  
`bool IsNumeric(var Expression)`

**Notlar:**  
CNum() ifadesi ayrıştırılamıyor başarılı olup olmadığını belirlemek için kullanılır.

- - -
### <a name="isstring"></a>IsString
**Açıklama:**  
İfade için bir dize türü değerlendirilmeden, IsString işlev True olarak değerlendirilir.

**Sözdizimi:**  
`bool IsString(var expression)`

**Notlar:**  
CStr() ifadesi ayrıştırılamıyor başarılı olup olmadığını belirlemek için kullanılır.

- - -
### <a name="ispresent"></a>Olmasına
**Açıklama:**  
İfade Null olmayan ve boş olmayan bir dize olarak değerlendirilirse, olmasına işlev true döndürür.

**Sözdizimi:**  
`bool IsPresent(var expression)`

**Notlar:**  
Bu işlev tersini IsNullOrEmpty olarak adlandırılır.

**Örnek:**  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

- - -
### <a name="item"></a>Öğe
**Açıklama:**  
Item işlevi, birden çok değerli bir dizeyi/özniteliği bir öğeyi döndürür.

**Sözdizimi:**  
`var Item(mvstr attribute, num index)`

* Öznitelik: birden çok değerli özniteliği
* Dizin: birden çok değerli dize içindeki bir öğenin dizini.

**Notlar:**  
Item işlevi, ikinci işlevi, birden çok değerli öznitelik içindeki bir öğeyi dizinini döndürür. bu yana Contains işlevi ile birlikte yararlı olur.

Dizin sınırların dışında ise bir hata oluşturur.

**Örnek:**  
`Mid(Item([proxyAddresses],Contains([proxyAddresses], "SMTP:")),6)`  
Birincil e-posta adresini döndürür.

- - -
### <a name="itemornull"></a>ItemOrNull
**Açıklama:**  
ItemOrNull işlevi, birden çok değerli bir dizeyi/özniteliği bir öğeyi döndürür.

**Sözdizimi:**  
`var ItemOrNull(mvstr attribute, num index)`

* Öznitelik: birden çok değerli özniteliği
* Dizin: birden çok değerli dize içindeki bir öğenin dizini.

**Notlar:**  
İkinci işlevi, birden çok değerli öznitelik içindeki bir öğeyi dizinini döndürür. bu yana ItemOrNull işlevi ile birlikte Contains işlevi yararlı olur.

Dizin sınırların dışında ise, ardından bir Null değer döndürür.

- - -
### <a name="join"></a>Katıl
**Açıklama:**  
Birleştirme işlevi, birden çok değerli bir dize alır ve her bir öğe eklenen belirtilen ayırıcı ile tek değerli bir dize döndürür.

**Sözdizimi:**  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

* Öznitelik: Birleştirilecek dizeleri içeren birden çok değerli öznitelik.
* sınırlayıcı: Döndürülen dize içindeki alt dizelerin ayırmak için kullanılan bir dize. Atlanırsa, boşluk karakteri ("") kullanılır. Sınırlayıcı sıfır uzunlukta bir dize ise ("") veya hiçbir şey, listedeki tüm öğeleri hiçbir sınırlayıcıları ile bitiştirilir.

**Açıklamalar**  
Katılma ve bölünmüş işlevler arasında eşlik yoktur. Birleştirme işlevi, bir dize dizisi alır ve bunları tek bir dize döndürecek şekilde bir sınırlayıcı dizesi kullanarak birleştirir. Split işlevine bir dize alır ve bir dizisini döndürmek için sınırlayıcı ayırır. Ancak, önemli bir fark birleştirme herhangi bir ayırıcı dize ile dizeyi bitiştirebilirsiniz, bölünmüş yalnızca bir tek karakter sınırlayıcıyı kullanarak dizeleri ayırabilirsiniz olur.

**Örnek:**  
`Join([proxyAddresses],",")`  
Döndürebilir: "SMTP:john.doe@contoso.com,smtp:jd@contoso.com"

- - -
### <a name="lcase"></a>LCase
**Açıklama:**  
LCase işlevi bir dizedeki tüm karakterleri küçük harfe dönüştürür.

**Sözdizimi:**  
`str LCase(str value)`

**Örnek:**  
`LCase("TeSt")`  
"Test" döndürür.

- - -
### <a name="left"></a>Sol
**Açıklama:**  
Left işlevi, bir dizenin soldan belirtilen sayıda karakteri döndürür.

**Sözdizimi:**  
`str Left(str string, num NumChars)`

* dize: dize karakteri döndürür
* NumChars: (solda) başından dize döndürülecek karakterlerin sayısı tanımlayan bir numara

**Notlar:**  
Dizedeki ilk numChars karakter içeren bir dize:

* Varsa numChars = 0 ise, boş bir dize döndürür.
* NumChars < 0, giriş dizesinde dönerseniz.
* Dize null ise, boş bir dize döndürür.

Dize, sayı, belirtilen numChars daha az karakterden içeriyorsa, (yani parametre 1'deki tüm karakterleri içeren) dize özdeş bir dize döndürülür.

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
LTrim işlevi bir dizeden baştaki beyaz boşlukları kaldırır.

**Sözdizimi:**  
`str LTrim(str value)`

**Örnek:**  
`LTrim(" Test ")`  
Döndürür "Test"

- - -
### <a name="mid"></a>Orta
**Açıklama:**  
PARÇAAL işlevi bir dizedeki belirtilen bir konumdan belirtilen sayıda karakteri döndürür.

**Sözdizimi:**  
`str Mid(str string, num start, num NumChars)`

* dize: dize karakteri döndürür
* Başlat: Başlangıç tanımlayan numara konumlandırma dizesindeki karakterlerin döndürmek için
* NumChars: döndürüleceğini dizedeki karakter sayısını tanımlayan numara

**Notlar:**  
Konumundan başlayan dönüş numChars karakter dizesindeki başlatın.  
Dizedeki başlangıç konumunu numChars karakterler içeren bir dize:

* Varsa numChars = 0 ise, boş bir dize döndürür.
* NumChars < 0, giriş dizesinde dönerseniz.
* Başlat > Giriş dizesinin dize uzunluğunu döndürür.
* Başlat < = 0, giriş dizesi döndürür.
* Dize null ise, boş bir dize döndürür.

NumChar karakter değilse, olabildiğince fazla konum başından dizesindeki kalan karakter mümkün olduğunca döndürülür.

**Örnek:**  
`Mid("John Doe", 3, 5)`  
"Hn" döndürür.

`Mid("John Doe", 6, 999)`  
Döndürür "Doe"

- - -
### <a name="now"></a>Şimdi
**Açıklama:**  
Şimdi işlevi, geçerli tarih ve saat, bilgisayarınızın sistem tarihi ve saati göre belirten bir DateTime döndürür.

**Sözdizimi:**  
`dt Now()`

- - -
### <a name="numfromdate"></a>NumFromDate
**Açıklama:**  
NumFromDate işlevi AD'nin tarih biçiminde bir tarih döndürür.

**Sözdizimi:**  
`num NumFromDate(dt value)`

**Örnek:**  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
129699324000000000 döndürür

- - -
### <a name="padleft"></a>padLeft
**Açıklama:**  
PadLeft işlevi sol-kullanamamaktadır sağlanan doldurma karakteri kullanılarak belirtilen bir süre için dize.

**Sözdizimi:**  
`str PadLeft(str string, num length, str padCharacter)`

* dize: dize doldurur.
* Uzunluk: İstenen dizenin uzunluğunu temsil eden bir tamsayı.
* padCharacter: Doldurma karakteri olarak kullanmak için tek bir karakter içeren bir dize

**Notlar:**

* Ardından dizenin uzunluğunu uzunluğundan küçükse, padCharacter sürekli olarak (bir uzunluğu olup kadar dizenin uzunluğa eşit solda) başına eklenir.
* Bir boşluk karakteri PadCharacter olabilir, ancak bir null değer olamaz.
* Dizenin uzunluğunu uzunluğundan büyük veya eşit ise, dize değiştirilmeden döndürülür.
* Dize değerinden büyük veya eşit uzunlukta uzunluğunda bir dize için aynı dize döndürülür.
* Dizenin uzunluğunu uzunluğundan küçükse, istenilen yeni bir dize ile bir padCharacter azsa içeren bir dize döndürdü.
* Dize null ise, işlev boş bir dize döndürür.

**Örnek:**  
`PadLeft("User", 10, "0")`  
"000000User" döndürür.

- - -
### <a name="padright"></a>PadRight
**Açıklama:**  
PadRight işlevi sağ-kullanamamaktadır sağlanan doldurma karakteri kullanılarak belirtilen bir süre için dize.

**Sözdizimi:**  
`str PadRight(str string, num length, str padCharacter)`

* dize: dize doldurur.
* Uzunluk: İstenen dizenin uzunluğunu temsil eden bir tamsayı.
* padCharacter: Doldurma karakteri olarak kullanmak için tek bir karakter içeren bir dize

**Notlar:**

* Dizenin uzunluğunu uzunluğundan küçükse, uzunluğu uzunluğa eşit olana kadar ardından padCharacter art arda dizenin sonuna (sağdaki) eklenir.
* Bir boşluk karakteri PadCharacter olabilir, ancak bir null değer olamaz.
* Dizenin uzunluğunu uzunluğundan büyük veya eşit ise, dize değiştirilmeden döndürülür.
* Dize değerinden büyük veya eşit uzunlukta uzunluğunda bir dize için aynı dize döndürülür.
* Dizenin uzunluğunu uzunluğundan küçükse, istenilen yeni bir dize ile bir padCharacter azsa içeren bir dize döndürdü.
* Dize null ise, işlev boş bir dize döndürür.

**Örnek:**  
`PadRight("User", 10, "0")`  
"User000000" döndürür.

- - -
### <a name="pcase"></a>PCase
**Açıklama:**  
PCase işlevi her boşlukla sözcüğün bir dizede ilk karakteri büyük harfe dönüştürür ve diğer tüm karakterleri küçük harfe dönüştürülür.

**Sözdizimi:**  
`String PCase(string)`

**Notlar:**

* Bu işlev, şu anda gibi bir kısaltması tamamen büyük harfli bir sözcük dönüştürmek için uygun büyük/küçük harf sağlamaz.

**Örnek:**  
`PCase("TEsT")`  
"Test" döndürür.

`PCase(LCase("TEST"))`  
Döndürür "Test"

- - -
### <a name="randomnum"></a>RandomNum
**Açıklama:**  
RandomNum işlevi, belirtilen bir zaman aralığı arasında rastgele bir sayı döndürür.

**Sözdizimi:**  
`num RandomNum(num start, num end)`

* Başlat: Oluşturulacak rastgele değer alt sınırı tanımlayan numara
* Son: Oluşturulacak rastgele değer sayısı üst sınırı tanımlayan numara

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
Replace işlevi başka bir dize için bir dize tüm oluşumlarını değiştirir.

**Sözdizimi:**  
`str Replace(str string, str OldValue, str NewValue)`

* Dize: Değerleri değiştirmek için bir dize.
* OldValue: Dize için arama yapın ve değiştirmek için.
* NewValue: İçin değiştirilecek dize.

**Notlar:**  
İşlev, aşağıdaki özel adlar tanır:

* \n – yeni satır
* \r – başı
* \t – sekmesi

**Örnek:**  
`Replace([address],"\r\n",", ")`  
Bir virgül ve boşluk ile CRLF değiştirir ve "Bir Microsoft yolu, Redmond, WA, ABD için" neden olabilir

- - -
### <a name="replacechars"></a>ReplaceChars
**Açıklama:**  
ReplaceChars işlevi ReplacePattern dizesinde bulunan karakter tüm oluşumlarını değiştirir.

**Sözdizimi:**  
`str ReplaceChars(str string, str ReplacePattern)`

* Dize: İçindeki karakterleri değiştirmek için bir dize.
* ReplacePattern: değiştirilecek karakterler içeren bir sözlük içeren bir dize.

{Kaynak1} biçimi şu şekildedir: {hedef1}, {kaynak2}: {hedef2}, {kaynakN}, {targetN} kaynağı bulun ve hedef dize ile değiştirilecek karakter olduğu.

**Notlar:**

* İşlev, her oluşumu tanımlanmış kaynakları alır ve bunları hedefleri ile değiştirir.
* Kaynak (unicode) tam olarak bir karakter olmalıdır.
* Kaynak boş ya da bir karakter (ayrıştırma hatası) daha uzun olamaz.
* Hedef örneğin ö:oe, β:ss birden çok karakter olabilir.
* Hedef karakter kaldırılması gerektiğini belirten boş olabilir.
* Kaynak, büyük küçük harfe duyarlıdır ve tam bir eşleşme olması gerekir.
* , (Virgül) ve: (virgül) ayrılmış karakterler ve bu işlevi kullanılarak değiştirilemez.
* Boşluk ve beyaz diğer karakterler ReplacePattern dizedeki göz ardı edilir.

**Örnek:**  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
Returns Raksmorgas

`ReplaceChars("O’Neil",%ReplaceString%)`  
Döndürür "ONeil", tek bir tick kaldırılacak tanımlanır.

- - -
### <a name="right"></a>Sağ
**Açıklama:**  
Right işlevi bir dize sağ (Bitiş) belirtilen sayıda karakteri döndürür.

**Sözdizimi:**  
`str Right(str string, num NumChars)`

* dize: dize karakteri döndürür
* NumChars: dizesinin sonundan başlayarak (sağdaki) döndürülecek karakterlerin sayısı tanımlayan bir numara

**Notlar:**  
NumChars karakter dizesinin son konumu döndürülür.

Dizedeki son numChars karakter içeren bir dize:

* Varsa numChars = 0 ise, boş bir dize döndürür.
* NumChars < 0, giriş dizesinde dönerseniz.
* Dize null ise, boş bir dize döndürür.

Dize, sayı, belirtilen NumChars daha az karakterden içeriyorsa, aynı dize için bir dize döndürülür.

**Örnek:**  
`Right("John Doe", 3)`  
"Doe" döndürür.

- - -
### <a name="rtrim"></a>RTrim
**Açıklama:**  
RTrim işlevi bir dizedeki sondaki boşlukları kaldırır.

**Sözdizimi:**  
`str RTrim(str value)`

**Örnek:**  
`RTrim(" Test ")`  
"Test" döndürür.

- - -
### <a name="select"></a>Seçim
**Açıklama:**  
Belirtilen işlev üzerinde birden çok değerli bir öznitelik (veya bir ifadenin çıkış) tüm değerler temel işlemi.

**Sözdizimi:**  
`mvattr Select(variable item, mvattr attribute, func function)`  
`mvattr Select(variable item, exp expression, func function)`

* öğe: Bir öğeyi birden çok değerli bir özniteliği temsil eder
* Öznitelik: birden çok değerli öznitelik
* ifade: değerlerin bir koleksiyonunu döndüren bir ifade
* koşul: öznitelik bir öğedeki işleyebilir herhangi işlevi

**Örnekler:**  
`Select($item,[otherPhone],Replace($item,"-",""))`  
Tireler (-) kaldırdıktan sonra birden çok değerli öznitelik otherPhone tüm değerleri döndürür.

- - -
### <a name="split"></a>Böl
**Açıklama:**  
Split işlevine bir sınırlayıcı ile ayrılmış bir dize alır ve birden çok değerli dize kolaylaştırır.

**Sözdizimi:**  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

* değer: ayırmak için sınırlayıcı karakter içeren dize.
* sınırlayıcı: tek bir ayırıcı olarak kullanılacak karakter.
* sınırı: en yüksek sayıda döndürebilir değeri.

**Örnek:**  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
Yararlı proxyAddress özniteliği için 2 öğe ile birden çok değerli bir dize döndürür.

- - -
### <a name="stringfromguid"></a>StringFromGuid
**Açıklama:**  
StringFromGuid işlevi ikili bir GUID alır ve bir dizeye dönüştürür

**Sözdizimi:**  
`str StringFromGuid(bin GUID)`

- - -
### <a name="stringfromsid"></a>StringFromSid
**Açıklama:**  
Bir güvenlik tanımlayıcısı için bir dize içeren bir bayt dizisi StringFromSid işlevi dönüştürür.

**Sözdizimi:**  
`str StringFromSid(bin ObjectSID)`  

- - -
### <a name="switch"></a>Anahtar
**Açıklama:**  
Anahtar işlev, değerlendirilen koşullara göre tek bir değer döndürmek için kullanılır.

**Sözdizimi:**  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

* ifade: Değişken ifade değerlendirmek istiyorsunuz.
* Değer: Karşılık gelen ifadenin True olması durumunda döndürülecek değer.

**Notlar:**  
Anahtarın işlevi bağımsız değişken listesi ifadeleri ve değer çiftlerinden oluşur. İfadeleri soldan sağa doğru değerlendirilir ve ilk ifade True olarak değerlendirilmesi için ilişkili değer döndürülür. Bölümleri düzgün eşleştirilmedi, bir çalışma zamanı hatası oluşur.

Örneğin, Deyim1 True ise, anahtar value1 döndürür. İfade-1, False, ancak 2 ifade doğru ise, anahtar değeri 2 vb. döndürür.

Anahtar döndürür bir hiçbir şey yapmayın varsa:

* Bir ifadenin True yok.
* İlk True ifade null karşılık gelen bir değer var.

Bunlardan yalnızca birini döndürür olsa bile anahtar tüm ifadeleri değerlendirir. Bu nedenle, istenmeyen yan etkileri için izlemeniz gerekir. Örneğin, herhangi bir ifade değerlendirmesi sıfıra bölme sıfır hata ile sonuçlanırsa hata oluşur.

Değer, özel bir dize döndürür hata işlevini de olabilir.

**Örnek:**  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
Bazı büyük şehirlerin konuşulan dili döndürür, aksi takdirde bir hata döndürür.

- - -
### <a name="trim"></a>Kırpma
**Açıklama:**  
Kırpma işlevi, baştaki ve sondaki beyaz boşlukları bir dizeden kaldırır.

**Sözdizimi:**  
`str Trim(str value)`  

**Örnek:**  
`Trim(" Test ")`  
"Test" döndürür.

`Trim([proxyAddresses])`  
Baştaki ve sondaki boşlukları proxyAddress özniteliği her bir değer kaldırır.

- - -
### <a name="ucase"></a>UCase
**Açıklama:**  
UCase işlevi bir dizedeki tüm karakterleri büyük harfe dönüştürür.

**Sözdizimi:**  
`str UCase(str string)`

**Örnek:**  
`UCase("TeSt")`  
"TEST" döndürür.

- - -
### <a name="where"></a>Konum

**Açıklama:**  
Belirli bir koşula dayalı birden çok değerli öznitelik (veya bir ifadenin çıkış) değerlerin bir alt kümesi döndürür.

**Sözdizimi:**  
`mvattr Where(variable item, mvattr attribute, exp condition)`  
`mvattr Where(variable item, exp expression, exp condition)`  
* öğe: Bir öğeyi birden çok değerli bir özniteliği temsil eder
* Öznitelik: birden çok değerli öznitelik
* koşul: true veya false sonucu verebilen herhangi bir ifade
* ifade: değerlerin bir koleksiyonunu döndüren bir ifade

**Örnek:**  
`Where($item,[userCertificate],CertNotAfter($item)>Now())`  
Sertifika değerleri, süresi dolmuş olmayan birden çok değerli öznitelik userCertificate döndürür.

- - -
### <a name="with"></a>Avantaj ile
**Açıklama:**  
WITH işlevi karmaşık ifadeyi temsil eden bir görünen bir alt ifade bir değişken kullanarak veya birden fazla kez karmaşık ifadenin basitleştirmesini sağlar.

**Sözdizimi:** 
`With(var variable, exp subExpression, exp complexExpression)`  
* değişkeni: Alt ifade temsil eder.
* Alt: alt ifade değişkeni tarafından temsil edilir.
* complexExpression: Karmaşık bir ifade.

**Örnek:**  
`With($unExpiredCerts,Where($item,[userCertificate],CertNotAfter($item)>Now()),IIF(Count($unExpiredCerts)>0,$unExpiredCerts,NULL))`  
İşlevsel olarak eşdeğerdir:  
`IIF (Count(Where($item,[userCertificate],CertNotAfter($item)>Now()))>0, Where($item,[userCertificate],CertNotAfter($item)>Now()),NULL)`  
UserCertificate özniteliği yalnızca süresi dolmamış sertifika değerlerini döndürür.


- - -
### <a name="word"></a>Word
**Açıklama:**  
Word işlev parametreleri ve sözcük sayısını döndürmek için sınırlayıcılar açıklayan göre bir dize içinde yer alan bir word döndürür.

**Sözdizimi:**  
`str Word(str string, num WordNumber, str delimiters)`

* dize: bir sözcük döndürmek için bir dize.
* WordNumber: hangi word numarasını tanımlayan numara döndürmelidir.
* Sınırlayıcılar: kelimeleri tanımlamak için kullanılması gereken delimiter(s) temsil eden bir dize

**Notlar:**  
Her bir dizenin karakter dizesindeki bir sınırlayıcı karakter ile ayrılmış sözcükler olarak tanımlanır:

* < 1 numara, boş dize döndürür.
* Dize null ise, boş bir dize döndürür.

Daha azını sözcük sayısı dize içeriyor ya da dize sınırlayıcı tarafından tanımlanan herhangi bir sözcük içermez, boş bir dize döndürülür.

**Örnek:**  
`Word("The quick brown fox",3," ")`  
Döndürür "brown"

`Word("This,string!has&many separators",3,",!&#")`  
"Sahip" döndürür

## <a name="additional-resources"></a>Ek Kaynaklar
* [Bildirim temelli sağlama ifadelerini anlama](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md)
* [Azure AD Connect eşitleme: Eşitleme seçeneklerini özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
