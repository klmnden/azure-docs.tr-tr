---
title: Azure API Management ilke ifadelerini | Microsoft Docs
description: "Azure API Management İlkesi ifadeleri hakkında bilgi edinin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: ea160028-fc04-4782-aa26-4b8329df3448
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: 58f7f71fd619eea2865ed42d2808fe6ae3e75c1f
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="api-management-policy-expressions"></a>API Management ilke ifadeleri
İlke ifadeleri sözdizimi C# 6.0 şeklindedir. Her bir ifadenin örtük olarak sağlanan erişimi [bağlamı](api-management-policy-expressions.md#ContextVariables) değişkeni ve bir izin verilen [alt](api-management-policy-expressions.md#CLRTypes) .NET Framework türü.  
  
> [!TIP]
>  İlke ifadeleri hakkında daha fazla bilgi için bkz: [ilke ifadelerini](https://azure.microsoft.com/documentation/videos/policy-expressions-in-azure-api-management/) video.  
>   
>  İlke ifadelerini kullanarak ilkeleri yapılandırma gösterim için bkz: [bulut kapak bölüm 177: daha Vlad Vinogradsky ile yönetim özelliklerini API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). Bu videoda, aşağıdaki ilke ifade gösterim içerir.  
>   
>  -   10:30 - arka uç hizmeti kullanarak bağlam bilgilerini sağlamak için API düzeyinde ilkeyi uygulamak nasıl görmek [ayarlamak sorgu dizesi parametresi](api-management-transformation-policies.md#SetQueryStringParameter) ve [ayarlamak HTTP üstbilgisi](api-management-transformation-policies.md#SetHTTPheader) ilkeleri. 12:10 İşte bu ilkeler görebileceğiniz bir işlem Geliştirici Portalı'nda arama demo yoktur.  
> -   13:50 - bkz nasıl kullanılacağını [doğrulamak JWT](api-management-access-restriction-policies.md#ValidateJWT) işlemlerine erişim önceden yetkilendirmek için ilke tabanlı belirteç talep. İlke Düzenleyicisi'nde yapılandırılmış ilkeler görmek için 15:00 ve 18:50 hem ile hem de gerekli yetkilendirme belirteci olmadan Geliştirici portalından bir işlem arama tanıtımı için ileri sarma.  
> -   21:00 - bkz nasıl kullanılacağını bir [API denetçisi](https://azure.microsoft.com/documentation/articles/api-management-howto-api-inspector/) ilkelerinin nasıl değerlendirildiği görmek için izleme ve değerlendirme sonuçları.  
> -   25:25 - ilke ifadelerle kullanılması hakkında bilgi [önbellekten alma](api-management-caching-policies.md#GetFromCache) ve [önbellek deposuna](api-management-caching-policies.md#StoreToCache) yanıt önbelleğe alma arka uç eşleşen API Management yanıt önbelleğe alma süresi yapılandırmak için ilkeleri Yedeklenen hizmetin tarafından belirtildiği gibi hizmet `Cache-Control` yönergesi.  
> -   34:30 - veri öğeleri kullanarak arka uç hizmeti alınan yanıt kaldırarak içerik filtreleme gerçekleştirme bkz [kontrol akışı](api-management-advanced-policies.md#choose) ve [ayarlamak gövde](api-management-transformation-policies.md#SetBody) ilkeleri. Genel bir bakış görmek 31:50 Başlat [tahmin koyu Sky API](https://developer.forecast.io/) bu tanıtım için kullanılır.  
> -   Bu videoda kullanılan ilke ifadelerini indirmek için bkz: [ilkelerinin yönetimi API örnekleri](https://github.com/Azure/api-management-samples/tree/master/policies) github depo.  
  
  
##  <a name="Syntax"></a>Sözdizimi  
 Tek bir deyimde ifadeleri içine alınmıştır `@(expression)`, burada `expression` doğru biçimlendirilmiş bir C# ifade ifadesi.  
  
 Birden fazla deyim ifadeleri içine alınmıştır `@{expression}`. Birden fazla deyim ifadeleri içindeki tüm kod yollarının ile bitmelidir bir `return` deyimi.  
  
##  <a name="PolicyExpressionsExamples"></a>Örnekler  
  
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
  
##  <a name="PolicyExpressionsUsage"></a>Kullanım  
 İfadeler, öznitelik değerleri ya da herhangi bir API Management metin değerleri olarak kullanılabilir [ilkeleri](api-management-policies.md), Grup İlkesi başvurusu aksini belirtmedikçe.  
  
> [!IMPORTANT]
>  İlke ifadeleri kullandığınızda, yalnızca sınırlı doğrulama ilke ifadelerini ilke tanımlandığında yoktur. İfadeler gelen veya giden ardışık düzendeki çalışma zamanında ağ geçidiyle çalıştırıldığından, ilke ifadeleri tarafından oluşturulan tüm çalışma zamanı özel durumları API çağrısı bir çalışma zamanı hatası neden olur.  
  
##  <a name="CLRTypes"></a>İlke ifadelerde izin verilen .NET framework türleri  
 Aşağıdaki tabloda, .NET Framework türleri ve ilke ifadelerde izin verilen üyeleri listeler.  
  
|CLR türü|Desteklenen yöntemler|  
|--------------|-----------------------|  
|Newtonsoft.Json.Linq.Extensions|Tüm yöntemleri desteklenir|  
|Newtonsoft.Json.Linq.JArray|Tüm yöntemleri desteklenir|  
|Newtonsoft.Json.Linq.JConstructor|Tüm yöntemleri desteklenir|  
|Newtonsoft.Json.Linq.JContainer|Tüm yöntemleri desteklenir|  
|Newtonsoft.Json.Linq.JObject|Tüm yöntemleri desteklenir|  
|Newtonsoft.Json.Linq.JProperty|Tüm yöntemleri desteklenir|  
|Newtonsoft.Json.Linq.JRaw|Tüm yöntemleri desteklenir|  
|Newtonsoft.Json.Linq.JToken|Tüm yöntemleri desteklenir|  
|Newtonsoft.Json.Linq.JTokenType|Tüm yöntemleri desteklenir|  
|Newtonsoft.Json.Linq.JValue|Tüm yöntemleri desteklenir|  
|System.Collections.Generic.IReadOnlyCollection < T\>|Tümü|  
|System.Collections.Generic.IReadOnlyDictionary < TKey, TValue >|Tümü|  
|System.Collections.Generic.ISet < TKey, TValue >|Tümü|  
|System.Collections.Generic.KeyValuePair < TKey, TValue >|Anahtar değeri|  
|System.Collections.Generic.List < TKey, TValue >|Tümü|  
|System.Collections.Generic.Queue < TKey, TValue >|Tümü|  
|System.Collections.Generic.Stack < TKey, TValue >|Tümü|  
|System.Convert'i|Tümü|  
|System.DateTime|Tümü|  
|System.DateTimeKind|UTC|  
|System.DateTimeOffset|Tümü|  
|System.Decimal|Tümü|  
|System.Double|Tümü|  
|System.Guid|Tümü|  
|System.IEnumerable < T\>|Tümü|  
|System.IEnumerator < T\>|Tümü|  
|System.Int16|Tümü|  
|System.Int32|Tümü|  
|System.Int64|Tümü|  
|System.Linq.Enumerable < T\>|Tüm yöntemleri desteklenir|  
|System.Math|Tümü|  
|System.MidpointRounding|Tümü|  
|System.Nullable < T\>|Tümü|  
|System.Random|Tümü|  
|System.SByte|Tümü|  
|System.Security.Cryptography. HMACSHA384|Tümü|  
|System.Security.Cryptography. HMACSHA512|Tümü|  
|System.Security.Cryptography.HashAlgorithm|Tümü|  
|System.Security.Cryptography.HMAC|Tümü|  
|System.Security.Cryptography.HMACMD5|Tümü|  
|System.Security.Cryptography.HMACSHA1|Tümü|  
|System.Security.Cryptography.HMACSHA256|Tümü|  
|System.Security.Cryptography.KeyedHashAlgorithm|Tümü|  
|System.Security.Cryptography.MD5|Tümü|  
|System.Security.Cryptography.RNGCryptoServiceProvider|Tümü|  
|System.Security.Cryptography.SHA1|Tümü|  
|System.Security.Cryptography.SHA1Managed|Tümü|  
|System.Security.Cryptography.SHA256|Tümü|  
|System.Security.Cryptography.SHA256Managed|Tümü|  
|System.Security.Cryptography.SHA384|Tümü|  
|System.Security.Cryptography.SHA384Managed|Tümü|  
|System.Security.Cryptography.SHA512|Tümü|  
|System.Security.Cryptography.SHA512Managed|Tümü|  
|System.Single|Tümü|  
|System.String|Tümü|  
|System.StringSplitOptions|Tümü|  
|System.Text.Encoding|Tümü|  
|System.Text.RegularExpressions.Capture|Dizin, uzunluk değeri|  
|System.Text.RegularExpressions.CaptureCollection|Öğe sayısı|  
|System.Text.RegularExpressions.Group|Yakalamalar, başarılı|  
|System.Text.RegularExpressions.GroupCollection|Öğe sayısı|  
|System.Text.RegularExpressions.Match|Boş, gruplar, sonuç|  
|System.Text.RegularExpressions.Regex|.ctor, IsMatch, eşleşme eşleşirse, Değiştir|  
|System.Text.RegularExpressions.RegexOptions|Derlenmiş, IgnoreCase, IgnorePatternWhitespace, çok satırlı, None, RightToLeft, Singleline|  
|System.TimeSpan|Tümü|  
|System.Tuple|Tümü|  
|System.UInt16|Tümü|  
|System.UInt32|Tümü|  
|System.UInt64|Tümü|  
|System.Uri|Tümü|  
|System.Xml.Linq.Extensions|Tüm yöntemleri desteklenir|  
|System.Xml.Linq.XAttribute|Tüm yöntemleri desteklenir|  
|System.Xml.Linq.XCData|Tüm yöntemleri desteklenir|  
|System.Xml.Linq.XComment|Tüm yöntemleri desteklenir|  
|System.Xml.Linq.XContainer|Tüm yöntemleri desteklenir|  
|System.Xml.Linq.XDeclaration|Tüm yöntemleri desteklenir|  
|System.Xml.Linq.XDocument|Tüm yöntemleri desteklenir|  
|System.Xml.Linq.XDocumentType|Tüm yöntemleri desteklenir|  
|System.Xml.Linq.XElement|Tüm yöntemleri desteklenir|  
|System.Xml.Linq.XName|Tüm yöntemleri desteklenir|  
|System.Xml.Linq.XNamespace|Tüm yöntemleri desteklenir|  
|System.Xml.Linq.XNode|Tüm yöntemleri desteklenir|  
|System.Xml.Linq.XNodeDocumentOrderComparer|Tüm yöntemleri desteklenir|  
|System.Xml.Linq.XNodeEqualityComparer|Tüm yöntemleri desteklenir|  
|System.Xml.Linq.XObject|Tüm yöntemleri desteklenir|  
|System.Xml.Linq.XProcessingInstruction|Tüm yöntemleri desteklenir|  
|System.Xml.Linq.XText|Tüm yöntemleri desteklenir|  
|System.Xml.XmlNodeType|Tümü|  
  
##  <a name="ContextVariables"></a>Bağlam değişkeni  
 Adlı bir değişkende `context` her ilkesinde örtük olarak kullanılabilir [ifade](api-management-policy-expressions.md#Syntax). Üyeleri için ilgili bilgiler `\request`. Tüm `context` üyeleri salt okunur.  
  
|Bağlam değişkeni|İzin verilen yöntemler, özellikler ve parametre değerleri|  
|----------------------|-------------------------------------------------------|  
|bağlam|API: IApi<br /><br /> Dağıtım<br /><br /> Son hata<br /><br /> İşlem<br /><br /> Ürün<br /><br /> İstek<br /><br /> RequestId: GUID<br /><br /> Yanıt<br /><br /> Abonelik<br /><br /> İzleme: bool<br /><br /> Kullanıcı<br /><br /> Değişkenleri: IReadOnlyDictionary < string, object ><br /><br /> void Trace(message: string)|  
|bağlamı. API|Kimliği: dize<br /><br /> Ad: dize<br /><br /> Yol: dize<br /><br /> ServiceUrl: IUrl|  
|bağlamı. Dağıtım|Bölge: dize<br /><br /> ServiceName: dize<br /><br /> Sertifikalar: IReadOnlyDictionary < string, X509Certificate2 >|  
|bağlamı. Son hata|Kaynak: dize<br /><br /> Neden: dize<br /><br /> İleti: dize<br /><br /> Kapsam: dize<br /><br /> Bölüm: dize<br /><br /> Yol: dize<br /><br /> Policyıd: dize<br /><br /> İçerik hakkında daha fazla bilgi için. Son hata, bkz: [hata işleme](api-management-error-handling-policies.md).|  
|bağlamı. İşlemi|Kimliği: dize<br /><br /> Yöntem: dize<br /><br /> Ad: dize<br /><br /> UrlTemplate: dize|  
|bağlamı. Ürün|API: IEnumerable < IApi\><br /><br /> ApprovalRequired: bool<br /><br /> Gruplar: IEnumerable < IGroup\><br /><br /> Kimliği: dize<br /><br /> Ad: dize<br /><br /> Durum: enum ProductState {NotPublished, yayımlanan}<br /><br /> SubscriptionLimit: int?<br /><br /> SubscriptionRequired: bool|  
|bağlamı. İstek|Gövde: IMessageBody<br /><br /> Sertifika: System.Security.Cryptography.X509Certificates.X509Certificate2<br /><br /> Üstbilgileri: IReadOnlyDictionary < dize, string [] ><br /><br /> IPADDRESS: dize<br /><br /> MatchedParameters: IReadOnlyDictionary < dize, dize ><br /><br /> Yöntem: dize<br /><br /> OriginalUrl:IUrl<br /><br /> URL: IUrl|  
|dize bağlamı. Request.Headers.GetValueOrDefault (headerName: string, defaultValue: dize)|headerName: dize<br /><br /> defaultValue: dize<br /><br /> İstek üstbilgi değerleri döndürür virgülle ayrılmış veya `defaultValue` üstbilgi bulunmazsa.|  
|bağlamı. Yanıt|Gövde: IMessageBody<br /><br /> Üstbilgileri: IReadOnlyDictionary < dize, string [] ><br /><br /> StatusCode: int<br /><br /> StatusReason: dize|  
|dize bağlamı. Response.Headers.GetValueOrDefault (headerName: string, defaultValue: dize)|headerName: dize<br /><br /> defaultValue: dize<br /><br /> Üstbilgi değerlerini virgülle ayrılmış yanıtı döndürür veya `defaultValue` üstbilgi bulunmazsa.|  
|bağlamı. Abonelik|CreatedTime: DateTime<br /><br /> EndDate: DateTime?<br /><br /> Kimliği: dize<br /><br /> Anahtar: dize<br /><br /> Ad: dize<br /><br /> PrimaryKey: dize<br /><br /> SecondaryKey: dize<br /><br /> StartDate: DateTime?|  
|bağlamı. Kullanıcı|E-posta: dize<br /><br /> Ad: dize<br /><br /> Gruplar: IEnumerable < IGroup\><br /><br /> Kimliği: dize<br /><br /> Kimlikler: IEnumerable < IUserIdentity\><br /><br /> Soyadı: dize<br /><br /> Not: dize<br /><br /> RegistrationDate: DateTime|  
|IApi|Kimliği: dize<br /><br /> Ad: dize<br /><br /> Yol: dize<br /><br /> Protokolleri: IEnumerable < dize\><br /><br /> ServiceUrl: IUrl<br /><br /> SubscriptionKeyParameterNames: ISubscriptionKeyParameterNames|  
|IGroup|Kimliği: dize<br /><br /> Ad: dize|  
|IMessageBody|Olarak < T\>(preserveContent: bool = false): Burada T: dize, JObject, JToken, JArray, XNode, XElement, XDocument<br /><br /> `context.Request.Body.As<T>` Ve `context.Response.Body.As<T>` İleti gövdeleri belirtilen türde bir istek ve yanıt okumak için kullanılan yöntemleri `T`. Varsayılan olarak özgün ileti gövdesi akışına ve reneders yöntemi kullanır, bu döndükten sonra kullanılamaz. Gövde akışını bir kopyası üzerinde çalışması yöntemi sağlayarak önlemek için ayarlanmış `preserveContent` parametresi `true`. Git [burada](api-management-transformation-policies.md#SetBody) bir örnek görmek için.|  
|IUrl|Ana bilgisayar: dize<br /><br /> Yol: dize<br /><br /> Bağlantı noktası: int<br /><br /> Sorgu: IReadOnlyDictionary < dize, string [] ><br /><br /> Sorgu dizesi: dize<br /><br /> Düzen: dize|  
|IUserIdentity|Kimliği: dize<br /><br /> Sağlayıcı: dize|  
|ISubscriptionKeyParameterNames|Başlık: dize<br /><br /> Sorgu: dize|  
|IUrl.Query.GetValueOrDefault dize (queryParameterName: string, defaultValue: dize)|queryParameterName: dize<br /><br /> defaultValue: dize<br /><br /> Sorgu parametresi değerleri döndürür virgülle ayrılmış veya `defaultValue` parametresi bulunmazsa.|  
|T bağlamı. Variables.GetValueOrDefault < T\>(variableName: string, defaultValue: T)|variableName: dize<br /><br /> defaultValue: T<br /><br /> Değişken değeri yazmak için cast döndürür `T` veya `defaultValue` değişkeni bulunmazsa.<br /><br /> Belirtilen tür döndürülen değişkeninin gerçek türü eşleşmezse, bu yöntem bir özel durum oluşturur.|  
|BasicAuthCredentials AsBasic(input: this string)|Giriş: dize<br /><br /> Giriş parametresi geçerli bir HTTP temel kimlik doğrulaması yetkilendirme isteği üstbilgisi değer içeriyorsa, yöntem türünde bir nesne döndürür `BasicAuthCredentials`; Aksi halde yöntem null değeri döndürür.|  
|bool TryParseBasic (giriş: Bu dize, sonuç: BasicAuthCredentials Giden)|Giriş: dize<br /><br /> Sonuç: BasicAuthCredentials çıkışı<br /><br /> Yöntem giriş parametresi geçerli bir HTTP temel kimlik doğrulaması yetkilendirme isteği üstbilgi değeri içerip içermediğini döndürür `true` ve sonuç parametresinin türünde bir değer içeren `BasicAuthCredentials`; Aksi takdirde yöntemi döndürür `false`.|  
|BasicAuthCredentials|Parola: dize<br /><br /> UserId: dize|  
|Jwt AsJwt(input: this string)|Giriş: dize<br /><br /> Giriş parametresi geçerli bir JWT belirteci değer içeriyorsa, yöntem türünde bir nesne döndürür `Jwt`; Aksi takdirde yöntemi döndürür `null`.|  
|bool TryParseJwt (giriş: Bu dize, sonuç: Jwt Giden)|Giriş: dize<br /><br /> Sonuç: Jwt çıkışı<br /><br /> Yöntem giriş parametresi geçerli bir JWT belirteci değeri içerip içermediğini döndürür `true` ve sonuç parametresinin türünde bir değer içeren `Jwt`; Aksi takdirde yöntemi döndürür `false`.|  
|Jwt|Algoritma: dize<br /><br /> İzleyici: IEnumerable < dize\><br /><br /> Talepleri: IReadOnlyDictionary < dize, string [] ><br /><br /> ExpirationTime: DateTime?<br /><br /> Kimliği: dize<br /><br /> Sertifikayı veren: dize<br /><br /> NotBefore: DateTime?<br /><br /> Konu: dize<br /><br /> Türü: dize|  
|Jwt.Claims.GetValueOrDefault dize (claimName: string, defaultValue: dize)|claimName: dize<br /><br /> defaultValue: dize<br /><br /> Virgülle ayrılmış talep değerleri döndürür veya `defaultValue` üstbilgi bulunmazsa.|
|Byte [] şifrele (giriş: Bu byte [], algoritma: string, anahtarı: byte [], iv:byte[])|Giriş - şifrelenmesi için düz metin<br /><br />algoritma - bir simetrik şifreleme algoritması adı<br /><br />anahtar - şifreleme anahtarı<br /><br />IV - başlatma vektörü<br /><br />Şifrelenmiş düz metin döndürür.|
|Byte [] şifrele (giriş: Bu byte [], algoritma: System.Security.Cryptography.SymmetricAlgorithm)|Giriş - şifrelenmesi için düz metin<br /><br />algoritma - şifreleme algoritması<br /><br />Şifrelenmiş düz metin döndürür.|
|Byte [] şifrele (giriş: Bu byte [], algoritma: System.Security.Cryptography.SymmetricAlgorithm, anahtarı: byte [], iv:byte[])|Giriş - şifrelenmesi için düz metin<br /><br />algoritma - şifreleme algoritması<br /><br />anahtar - şifreleme anahtarı<br /><br />IV - başlatma vektörü<br /><br />Şifrelenmiş düz metin döndürür.|
|Byte [] şifresini çözme (giriş: Bu byte [], algoritma: string, anahtarı: byte [], iv:byte[])|Giriş - şifresinin çözülmesini cyphertext<br /><br />algoritma - bir simetrik şifreleme algoritması adı<br /><br />anahtar - şifreleme anahtarı<br /><br />IV - başlatma vektörü<br /><br />Düz metin döndürür.|
|Byte [] şifresini çözme (giriş: Bu byte [], algoritma: System.Security.Cryptography.SymmetricAlgorithm)|Giriş - şifresinin çözülmesini cyphertext<br /><br />algoritma - şifreleme algoritması<br /><br />Düz metin döndürür.|
|Byte [] şifresini çözme (giriş: Bu byte [], algoritma: System.Security.Cryptography.SymmetricAlgorithm, anahtarı: byte [], iv:byte[])|Şifresi çözülecek giriş - giriş - cyphertext<br /><br />algoritma - şifreleme algoritması<br /><br />anahtar - şifreleme anahtarı<br /><br />IV - başlatma vektörü<br /><br />Düz metin döndürür.|

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player] 
>
## <a name="next-steps"></a>Sonraki adımlar

İlkeleriyle çalışma daha fazla bilgi için bkz:

+ [API Management ilkeleri](api-management-howto-policies.md)
+ [API dönüştürme](transform-api.md)
+ [Grup İlkesi başvurusu](api-management-policy-reference.md) ilke deyimleri ve ayarlarının tam listesi için
+ [İlke örnekleri](policy-samples.md)   