---
title: Azure API Management ilke ifadeleri | Microsoft Docs
description: Azure API Management ilke ifadeleri hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: ea160028-fc04-4782-aa26-4b8329df3448
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2019
ms.author: apimpm
ms.openlocfilehash: 6446919c80e7e3fd379e6e39eb51712f9736ea7e
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341204"
---
# <a name="api-management-policy-expressions"></a>API Management ilke ifadeleri
İlke ifadeleri söz dizimi anlatılmaktadır C# 7. Her ifadesi örtük olarak sağlanan erişimi olan [bağlam](api-management-policy-expressions.md#ContextVariables) değişkeni ve izin verilen [alt](api-management-policy-expressions.md#CLRTypes) .NET Framework türleri.

Daha fazla bilgi için:

- Arka uç hizmetinize bağlam bilgilerini sağlamak bkz. Kullanım [sorgu dizesi parametresini ayarlayın](api-management-transformation-policies.md#SetQueryStringParameter) ve [ayarlamak HTTP üstbilgisi](api-management-transformation-policies.md#SetHTTPheader) ilkeleri bu bilgileri sağlamak için.
- Kullanma hakkında bilgi [doğrulamak için JWT](api-management-access-restriction-policies.md#ValidateJWT) işlemlerine erişim önceden yetkilendirmek için ilke temelli belirteç talep.
- Kullanma hakkında bilgi bir [API denetçisi](https://azure.microsoft.com/documentation/articles/api-management-howto-api-inspector/) ilkelerinin nasıl değerlendirildiği görmek için izleme ve bu değerlendirme sonuçları.
- İfadelerle kullanma hakkında bilgi [önbellekten alma](api-management-caching-policies.md#GetFromCache) ve [önbelleğine Store](api-management-caching-policies.md#StoreToCache) API Management yanıt önbelleğe almayı yapılandırma ilkeleri. Yanıt belirtildiği gibi arka uç hizmeti tarafından yedeklenmiş hizmetin önbelleğe almayı eşleşen süre ayarlama `Cache-Control` yönergesi.
- İçerik filtreleme işlemleri öğrenin. Arka uç kullanarak alınan yanıtı veri öğelerini kaldırma [denetim akışı](api-management-advanced-policies.md#choose) ve [ayarlamak gövdesi](api-management-transformation-policies.md#SetBody) ilkeleri.
- İlke ifadeleri indirmek için bkz [İlkeleri Yönetimi API örnekleri](https://github.com/Azure/api-management-samples/tree/master/policies) GitHub deposu.


## <a name="Syntax"></a> Söz dizimi
Tek bir deyim ifadeleri içine alınmıştır `@(expression)`burada `expression` iyi biçimlendirilmemiş C# ifade deyimi.

Birden fazla deyim ifadeleri içine alınmıştır `@{expression}`. Birden fazla deyim ifadeleri içindeki tüm kod yolları ile bitmelidir bir `return` deyimi.

## <a name="PolicyExpressionsExamples"></a> Örnekleri

```
@(true)

@((1+1).ToString())

@("Hi There".Length)

@(Regex.Match(context.Response.Headers.GetValueOrDefault("Cache-Control",""), @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value)

@(context.Variables.ContainsKey("maxAge") ? int.Parse((string)context.Variables["maxAge"]) : 3600)

@{
  string value;
  if (context.Request.Headers.TryGetValue("Authorization", out value))
  {
    return Encoding.UTF8.GetString(Convert.FromBase64String(value));
  }
  else
  {
    return null;
  }
}
```

## <a name="PolicyExpressionsUsage"></a>Kullanım
İfadeler, öznitelik değerleri ya da herhangi bir API Management metin değerleri olarak kullanılabilir [ilkeleri](api-management-policies.md) (ilke başvurusu aksini belirtmedikçe).

> [!IMPORTANT]
> İlke ifadeleri kullanma olduğunda yalnızca sınırlı doğrulama İlkesi ifadelerin ilke tanımlandığında. İfadeler, çalışma zamanında, bir çalışma zamanı hatası ilke ifadeleri sonucu tarafından oluşturulan özel durumların ağ geçidi tarafından yürütülür.

## <a name="CLRTypes"></a> .NET framework türleri içinde ilke ifadelere izin veriliyor
Aşağıdaki tablo, .NET Framework türlerini ve ilke ifadelerinde izin üyeleri listeler.

|Type|Desteklenen üyeleri|
|--------------|-----------------------|
|Newtonsoft.Json.Formatting|Tümü|
|Newtonsoft.Json.JsonConvert|SerializeObject, DeserializeObject|
|Newtonsoft.Json.Linq.Extensions|Tümü|
|Newtonsoft.Json.Linq.JArray|Tümü|
|Newtonsoft.Json.Linq.JConstructor|Tümü|
|Newtonsoft.Json.Linq.JContainer|Tümü|
|Newtonsoft.Json.Linq.JObject|Tümü|
|Newtonsoft.Json.Linq.JProperty|Tümü|
|Newtonsoft.Json.Linq.JRaw|Tümü|
|Newtonsoft.Json.Linq.JToken|Tümü|
|Newtonsoft.Json.Linq.JTokenType|Tümü|
|Newtonsoft.Json.Linq.JValue|Tümü|
|System.Array|Tümü|
|System.BitConverter|Tümü|
|System.Boolean|Tümü|
|System.Byte|Tümü|
|System.Char|Tümü|
|System.Collections.Generic.Dictionary < TKey, TValue >|Tümü|
|System.Collections.Generic.HashSet\<T >|Tümü|
|System.Collections.Generic.ICollection\<T >|Tümü|
|System.Collections.Generic.IDictionary < TKey, TValue >|Tümü|
|.IEnumerable örneği\<T >|Tümü|
|System.Collections.Generic.IEnumerator\<T >|Tümü|
|System.Collections.Generic.IList\<T >|Tümü|
|System.Collections.Generic.IReadOnlyCollection\<T >|Tümü|
|System.Collections.Generic.IReadOnlyDictionary < TKey, TValue >|Tümü|
|System.Collections.Generic.ISet\<T >|Tümü|
|System.Collections.Generic.KeyValuePair < TKey, TValue >|Tümü|
|System.Collections.Generic.List\<T >|Tümü|
|Sıra System.Collections.Generic.Queue\<T >|Tümü|
|System.Collections.Generic.Stack\<T >|Tümü|
|System.Convert|Tümü|
|System.DateTime|(Oluşturucu), ekleme, AddDays, AddHours, AddMilliseconds, AddMinutes, AddMonths, saniyeEkle, AddTicks, AddYears, tarih, gün, DayOfWeek, DayOfYear, DaysInMonth, saat, IsDaylightSavingTime, IsLeapYear, MaxValue, milisaniye, dakika, MinValue, ay, artık , Ayrıştırma, ikinci olarak, çıkarma, saat tıklaması, TimeOfDay, bugün, ToString, UtcNow, yıl|
|System.DateTimeKind|UTC|
|System.DateTimeOffset|Tümü|
|System.Decimal|Tümü|
|System.Double|Tümü|
|System.Exception|Tümü|
|System.Guid|Tümü|
|System.Int16|Tümü|
|System.Int32|Tümü|
|System.Int64|Tümü|
|System.IO.StringReader|Tümü|
|System.IO.StringWriter|Tümü|
|System.Linq.Enumerable|Tümü|
|System.Math|Tümü|
|System.MidpointRounding|Tümü|
|System.Net.WebUtility|Tümü|
|System.Nullable|Tümü|
|System.Random|Tümü|
|System.SByte|Tümü|
|System.Security.Cryptography.AsymmetricAlgorithm|Tümü|
|System.Security.Cryptography.CipherMode|Tümü|
|System.Security.Cryptography.HashAlgorithm|Tümü|
|System.Security.Cryptography.HashAlgorithmName|Tümü|
|System.Security.Cryptography.HMAC|Tümü|
|System.Security.Cryptography.HMACMD5|Tümü|
|System.Security.Cryptography.HMACSHA1|Tümü|
|System.Security.Cryptography.HMACSHA256|Tümü|
|System.Security.Cryptography.HMACSHA384|Tümü|
|System.Security.Cryptography.HMACSHA512|Tümü|
|System.Security.Cryptography.KeyedHashAlgorithm|Tümü|
|System.Security.Cryptography.MD5|Tümü|
|System.Security.Cryptography.Oid|Tümü|
|System.Security.Cryptography.PaddingMode|Tümü|
|System.Security.Cryptography.RNGCryptoServiceProvider|Tümü|
|System.Security.Cryptography.RSA|Tümü|
|System.Security.Cryptography.RSAEncryptionPadding|Tümü|
|System.Security.Cryptography.RSASignaturePadding|Tümü|
|System.Security.Cryptography.SHA1|Tümü|
|System.Security.Cryptography.SHA1Managed|Tümü|
|System.Security.Cryptography.SHA256|Tümü|
|System.Security.Cryptography.SHA256Managed|Tümü|
|System.Security.Cryptography.SHA384|Tümü|
|System.Security.Cryptography.SHA384Managed|Tümü|
|System.Security.Cryptography.SHA512|Tümü|
|System.Security.Cryptography.SHA512Managed|Tümü|
|System.Security.Cryptography.SymmetricAlgorithm|Tümü|
|System.Security.Cryptography.X509Certificates.PublicKey|Tümü|
|System.Security.Cryptography.X509Certificates.RSACertificateExtensions|Tümü|
|System.Security.Cryptography.X509Certificates.X500DistinguishedName|Ad|
|System.Security.Cryptography.X509Certificates.X509Certificate|Tümü|
|System.Security.Cryptography.X509Certificates.X509Certificate2|Tümü|
|System.Security.Cryptography.X509Certificates.X509ContentType|Tümü|
|System.Security.Cryptography.X509Certificates.X509NameType|Tümü|
|System.Single|Tümü|
|System.String|Tümü|
|System.StringComparer|Tümü|
|System.StringComparison|Tümü|
|System.StringSplitOptions|Tümü|
|System.Text.Encoding|Tümü|
|System.Text.RegularExpressions.Capture|Dizin, uzunluk değeri|
|System.Text.RegularExpressions.CaptureCollection|Öğe sayısı|
|System.Text.RegularExpressions.Group|Yakalamalar, başarılı|
|System.Text.RegularExpressions.GroupCollection|Öğe sayısı|
|System.Text.RegularExpressions.Match|Boş, gruplar, sonuç|
|System.Text.RegularExpressions.Regex|(Oluşturucu) IsMatch, eşleşmenin bir eşleşme değiştirin, Unescape, Böl|
|System.Text.RegularExpressions.RegexOptions|Tümü|
|System.Text.StringBuilder|Tümü|
|System.TimeSpan|Tümü|
|System.TimeZone|Tümü|
|System.TimeZoneInfo.AdjustmentRule|Tümü|
|System.TimeZoneInfo.TransitionTime|Tümü|
|System.TimeZoneInfo|Tümü|
|System.Tuple|Tümü|
|System.UInt16|Tümü|
|System.UInt32|Tümü|
|System.UInt64|Tümü|
|System.Uri|Tümü|
|System.UriPartial|Tümü|
|System.Xml.Linq.Extensions|Tümü|
|System.Xml.Linq.XAttribute|Tümü|
|System.Xml.Linq.XCData|Tümü|
|System.Xml.Linq.XComment|Tümü|
|System.Xml.Linq.XContainer|Tümü|
|System.Xml.Linq.XDeclaration|Tümü|
|System.Xml.Linq.XDocument|All, except: Yükleme|
|System.Xml.Linq.XDocumentType|Tümü|
|System.Xml.Linq.XElement|Tümü|
|System.Xml.Linq.XName|Tümü|
|System.Xml.Linq.XNamespace|Tümü|
|System.Xml.Linq.XNode|Tümü|
|System.Xml.Linq.XNodeDocumentOrderComparer|Tümü|
|System.Xml.Linq.XNodeEqualityComparer|Tümü|
|System.Xml.Linq.XObject|Tümü|
|System.Xml.Linq.XProcessingInstruction|Tümü|
|System.Xml.Linq.XText|Tümü|
|System.Xml.XmlNodeType|Tümü|

## <a name="ContextVariables"></a> Bağlam değişkeni
Adlı bir değişken `context` her ilkede örtük olarak kullanılabilir [ifade](api-management-policy-expressions.md#Syntax). Üyeleri sağlamak için testlerinizle ilgili olabilecek bilgilere `\request`. Tüm `context` üyeleri salt okunurdur.

|Bağlam değişkeni|Yöntemler, özellikler ve parametre değerlerini izin|
|----------------------|-------------------------------------------------------|
|Bağlam|[API](#ref-context-api): [IApi](#ref-iapi)<br /><br /> [Dağıtım](#ref-context-deployment)<br /><br /> Geçen: TimeSpan - zaman damgası değeri ve geçerli saat arasındaki zaman aralığı<br /><br /> [Son hata](#ref-context-lasterror)<br /><br /> [İşlem](#ref-context-operation)<br /><br /> [Ürün](#ref-context-product)<br /><br /> [İstek](#ref-context-request)<br /><br /> RequestId: GUID - benzersiz istek tanımlayıcısı<br /><br /> [Yanıt](#ref-context-response)<br /><br /> [Abonelik](#ref-context-subscription)<br /><br /> Zaman damgası: DateTime - istek alındığında zaman içinde nokta<br /><br /> İzleme: bool - gösterir izleme açık veya kapalı olma <br /><br /> [Kullanıcı](#ref-context-user)<br /><br /> [Değişkenleri](#ref-context-variables): IReadOnlyDictionary < string, object ><br /><br /> void Trace(message: string)|
|<a id="ref-context-api"></a>bağlamı. API|ID: dize<br /><br /> IsCurrentRevision: bool<br /><br />  Ad: dize<br /><br /> Yol: dize<br /><br /> Düzeltme: dize<br /><br /> ServiceUrl: [IUrl](#ref-iurl)<br /><br /> Sürüm: dize |
|<a id="ref-context-deployment"></a>bağlamı. Dağıtım|Bölge: dize<br /><br /> ServiceName: dize<br /><br /> Sertifikaları: IReadOnlyDictionary < string, X509Certificate2 >|
|<a id="ref-context-lasterror"></a>bağlamı. Son hata|Kaynak: dize<br /><br /> Neden: dize<br /><br /> İleti: dize<br /><br /> Kapsam: dize<br /><br /> Bölüm: dize<br /><br /> Yol: dize<br /><br /> Policyıd: dize<br /><br /> Bağlamı hakkında daha fazla bilgi için. LastError, bkz: [hata işleme](api-management-error-handling-policies.md).|
|<a id="ref-context-operation"></a>bağlamı. İşlemi|ID: dize<br /><br /> Yöntemi: dize<br /><br /> Ad: dize<br /><br /> UrlTemplate: dize|
|<a id="ref-context-product"></a>bağlamı. Ürün|API'ler: IEnumerable <[IApi](#ref-iapi)\><br /><br /> ApprovalRequired: bool<br /><br /> Grupları: IEnumerable <[IGroup](#ref-igroup)\><br /><br /> ID: dize<br /><br /> Ad: dize<br /><br /> Durum: sabit listesi ProductState {NotPublished, yayımlanmış}<br /><br /> SubscriptionLimit: int?<br /><br /> SubscriptionRequired: bool|
|<a id="ref-context-request"></a>bağlamı. İstek|Gövdesi: [IMessageBody](#ref-imessagebody)<br /><br /> Sertifika: System.Security.Cryptography.X509Certificates.X509Certificate2<br /><br /> [Üstbilgileri](#ref-context-request-headers): IReadOnlyDictionary < string, string [] ><br /><br /> IPADDRESS: dize<br /><br /> MatchedParameters: IReadOnlyDictionary < string, string ><br /><br /> Yöntemi: dize<br /><br /> OriginalUrl: [IUrl](#ref-iurl)<br /><br /> URL: [IUrl](#ref-iurl)|
|<a id="ref-context-request-headers"></a>dize içeriği. Request.Headers.GetValueOrDefault (headerName: string, defaultValue: dize)|headerName: dize<br /><br /> defaultValue: dize<br /><br /> Üstbilgi değerlerini virgülle ayrılmış istek döndürür veya `defaultValue` üstbilgi bulunamaması durumunda.|
|<a id="ref-context-response"></a>bağlamı. Yanıt|Gövdesi: [IMessageBody](#ref-imessagebody)<br /><br /> [Üstbilgileri](#ref-context-response-headers): IReadOnlyDictionary < string, string [] ><br /><br /> StatusCode: int<br /><br /> StatusReason: dize|
|<a id="ref-context-response-headers"></a>dize içeriği. Response.Headers.GetValueOrDefault (headerName: string, defaultValue: dize)|headerName: dize<br /><br /> defaultValue: dize<br /><br /> Üstbilgi değerlerini virgülle ayrılmış bir yanıt döndürür veya `defaultValue` üstbilgi bulunamaması durumunda.|
|<a id="ref-context-subscription"></a>bağlamı. Abonelik|Oluşturulma zamanı: DateTime<br /><br /> EndDate: DateTime?<br /><br /> ID: dize<br /><br /> Anahtar: dize<br /><br /> Ad: dize<br /><br /> PrimaryKey: dize<br /><br /> İkincil anahtarı oluşturma: dize<br /><br /> StartDate: DateTime?|
|<a id="ref-context-user"></a>context.User|E-posta: dize<br /><br /> FirstName: dize<br /><br /> Grupları: IEnumerable <[IGroup](#ref-igroup)\><br /><br /> ID: dize<br /><br /> Kimlikleri: IEnumerable <[IUserIdentity](#ref-iuseridentity)\><br /><br /> Soyadı: dize<br /><br /> Not: dize<br /><br /> RegistrationDate: DateTime|
|<a id="ref-iapi"></a>IApi|ID: dize<br /><br /> Ad: dize<br /><br /> Yol: dize<br /><br /> Protokoller: IEnumerable < string\><br /><br /> ServiceUrl: [IUrl](#ref-iurl)<br /><br /> SubscriptionKeyParameterNames: [ISubscriptionKeyParameterNames](#ref-isubscriptionkeyparameternames)|
|<a id="ref-igroup"></a>IGroup|ID: dize<br /><br /> Ad: dize|
|<a id="ref-imessagebody"></a>IMessageBody|Olarak < T\>(preserveContent: bool = false): Burada T: dize, JObject, JToken, JArray, XNode, XElement ve XDocument<br /><br /> `context.Request.Body.As<T>` Ve `context.Response.Body.As<T>` İleti gövdeleri belirtilen türde bir istek ve yanıt okumak için kullanılan yöntemleri `T`. Varsayılan olarak yöntemi özgün iletinin gövde akışını kullanır ve bunu döndürdükten sonra kullanılamaz işler. Gövde akışını bir kopyası üzerinde çalışması yöntemi sağlayarak önlemek için ayarlayın `preserveContent` parametresi `true`. Git [burada](api-management-transformation-policies.md#SetBody) bir örnek görmek için.|
|<a id="ref-iurl"></a>IUrl|Konak: dize<br /><br /> Yol: dize<br /><br /> Bağlantı noktası: int<br /><br /> [Sorgu](#ref-iurl-query): IReadOnlyDictionary < string, string [] ><br /><br /> Sorgu dizesi: dize<br /><br /> Düzen: dize|
|<a id="ref-iuseridentity"></a>IUserIdentity|ID: dize<br /><br /> Sağlayıcı: dize|
|<a id="ref-isubscriptionkeyparameternames"></a>ISubscriptionKeyParameterNames|Üstbilgi: dize<br /><br /> Sorgu: dize|
|<a id="ref-iurl-query"></a>IUrl.Query.GetValueOrDefault dize (queryParameterName: string, defaultValue: dize)|queryParameterName: dize<br /><br /> defaultValue: dize<br /><br /> Virgülle ayrılmış bir sorgu parametresi değerleri döndürür veya `defaultValue` parametresi bulunmazsa.|
|<a id="ref-context-variables"></a>T bağlamı. Variables.GetValueOrDefault < T\>(variableName: string, defaultValue: T)|Değişkenadı: dize<br /><br /> defaultValue: T<br /><br /> Değişken değeri türüne yapılan döndürür `T` veya `defaultValue` değişken bulunmazsa.<br /><br /> Belirtilen türün gerçek tür döndürülen değişkenin eşleşmiyorsa bu yöntemi bir özel durum oluşturur.|
|BasicAuthCredentials AsBasic(input: this string)|Giriş: dize<br /><br /> Giriş parametresi geçerli bir HTTP temel kimlik doğrulaması yetkilendirme isteği üst bilgisi değeri içeriyorsa, yöntem türü bir nesne döndürür. `BasicAuthCredentials`; Aksi takdirde yöntem null değeri döndürür.|
|bool TryParseBasic (giriş: Bu dize, sonuç: BasicAuthCredentials çıkış)|Giriş: dize<br /><br /> Sonuç: BasicAuthCredentials çıkış<br /><br /> Giriş parametresi geçerli bir HTTP temel kimlik doğrulaması yetkilendirme değeri istek üstbilgisindeki içeriyorsa yöntem döndürür `true` ve sonuç parametresinin türü değeri içeren `BasicAuthCredentials`; Aksi takdirde yöntem döndürür `false`.|
|BasicAuthCredentials|Parola: dize<br /><br /> UserId: dize|
|Jwt AsJwt(input: this string)|Giriş: dize<br /><br /> Giriş parametresi geçerli bir JWT belirteci değer içeriyorsa, yöntem türü bir nesne döndürür. `Jwt`; Aksi takdirde yöntem döndürür `null`.|
|bool TryParseJwt (giriş: Bu dize, sonuç: Jwt çıkış)|Giriş: dize<br /><br /> Sonuç: Jwt çıkış<br /><br /> Yöntemi giriş parametresi geçerli bir JWT belirteci değeri içerip içermediğini döndürür `true` ve sonuç parametresinin türü değeri içeren `Jwt`; Aksi takdirde yöntem döndürür `false`.|
|Jwt|Algoritma: dize<br /><br /> Hedef kitle: IEnumerable < string\><br /><br /> Talepleri: IReadOnlyDictionary < string, string [] ><br /><br /> ExpirationTime: DateTime?<br /><br /> ID: dize<br /><br /> Veren: dize<br /><br /> IssuedAt: DateTime?<br /><br /> NotBefore: DateTime?<br /><br /> Konu: dize<br /><br /> Türü: dize|
|Jwt.Claims.GetValueOrDefault dize (claimName: string, defaultValue: dize)|claimName: dize<br /><br /> defaultValue: dize<br /><br /> Talep değerlerini virgülle ayrılmış döndürür veya `defaultValue` üstbilgi bulunamaması durumunda.|
|bayt [] şifrele (giriş: Bu bayt [], algoritma: dize, anahtar: byte [], iv:byte[])|Giriş - şifrelenmiş düz metin<br /><br />algoritma - bir simetrik şifreleme algoritması adı<br /><br />anahtarı - şifreleme anahtarı<br /><br />IV - başlatma vektörü<br /><br />Şifrelenmiş düz metin döndürür.|
|bayt [] şifrele (giriş: Bu bayt [], algoritma: System.Security.Cryptography.SymmetricAlgorithm)|Giriş - şifrelenmiş düz metin<br /><br />algoritma - şifreleme algoritması<br /><br />Şifrelenmiş düz metin döndürür.|
|bayt [] şifrele (giriş: Bu bayt [], algoritma: System.Security.Cryptography.SymmetricAlgorithm, anahtar: byte [], iv:byte[])|Giriş - şifrelenmiş düz metin<br /><br />algoritma - şifreleme algoritması<br /><br />anahtarı - şifreleme anahtarı<br /><br />IV - başlatma vektörü<br /><br />Şifrelenmiş düz metin döndürür.|
|bayt [] şifresini çözme (giriş: Bu bayt [], algoritma: dize, anahtar: byte [], iv:byte[])|Giriş - şifresinin çözülmesi için şifre metni<br /><br />algoritma - bir simetrik şifreleme algoritması adı<br /><br />anahtarı - şifreleme anahtarı<br /><br />IV - başlatma vektörü<br /><br />Düz metin döndürür.|
|bayt [] şifresini çözme (giriş: Bu bayt [], algoritma: System.Security.Cryptography.SymmetricAlgorithm)|Giriş - şifresinin çözülmesi için şifre metni<br /><br />algoritma - şifreleme algoritması<br /><br />Düz metin döndürür.|
|bayt [] şifresini çözme (giriş: Bu bayt [], algoritma: System.Security.Cryptography.SymmetricAlgorithm, anahtar: byte [], iv:byte[])|Giriş - şifresinin çözülmesi için şifre metni<br /><br />algoritma - şifreleme algoritması<br /><br />anahtarı - şifreleme anahtarı<br /><br />IV - başlatma vektörü<br /><br />Düz metin döndürür.|
|bool VerifyNoRevocation (giriş: Bu System.Security.Cryptography.X509Certificates.X509Certificate2)|Sertifika iptal durumunu denetlemeden X.509 zincir doğrulama gerçekleştirir.<br /><br />Giriş - sertifika nesnesi<br /><br />Döndürür `true` doğrulama başarılı olursa; `false` doğrulama başarısız olursa.|


## <a name="next-steps"></a>Sonraki adımlar

İlkeleriyle çalışma hakkında bilgi için bkz:

+ [API Management ilkeleri](api-management-howto-policies.md)
+ [API'leri dönüştürme](transform-api.md)
+ [İlke başvurusu](api-management-policy-reference.md) ilke bildirimlerine ve ayarlarının tam listesi için
+ [İlke örnekleri](policy-samples.md)
