---
title: Giriş doğrulama - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs
description: Tehdit modelleme Aracı kullanıma sunulan tehdit azaltma
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: jegeib
ms.openlocfilehash: 0803ade7613480621a0cd87f9944ee5f55bf432c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60586314"
---
# <a name="security-frame-input-validation--mitigations"></a>Güvenlik çerçevesi: Giriş doğrulama | Risk azaltma işlemleri 
| Ürün/hizmet | Makale |
| --------------- | ------- |
| **Web uygulaması** | <ul><li>[XSLT güvenilmeyen stil sayfaları kullanarak tüm dönüştürmeler için betik oluşturma devre dışı bırak](#disable-xslt)</li><li>[Kullanıcı denetlenebilir içerik içerebilen her sayfanın MIME otomatik algılaması dışında bölgedeyse emin olun.](#out-sniffing)</li><li>[Sağlamlaştırma veya XML varlık çözümleme devre dışı bırakma](#xml-resolution)</li><li>[HTTP.sys kullanan uygulamalar URL Standartlaştırma doğrulama gerçekleştirme](#app-verification)</li><li>[Kullanıcıların dosyaları kabul ederken uygun denetimleri yerinde olduğundan emin olun](#controls-users)</li><li>[Tür kullanımı uyumlu parametre veri erişimi için Web uygulamasında kullanıldığından emin olun](#typesafe)</li><li>[Ayrı model bağlama sınıfları kullanın veya MVC yığın atama güvenlik açığını önlemek için bağlama filtresi listeler](#binding-mvc)</li><li>[Güvenilmeyen web çıkış işleme önce kodlayın](#rendering)</li><li>[Giriş doğrulaması ve Model özellikleri tüm dize türü üzerinde filtreleme yapmak](#typemodel)</li><li>[Tüm karakterler, örn, Zengin Metin Düzenleyicisi'ni kabul edin. form alanlarını temizleme uygulanmalıdır](#richtext)</li><li>[DOM öğeleri yerleşik kodlama olmayan havuzlarından atamayın](#inbuilt-encode)</li><li>[Tüm doğrulama yeniden yönlendirmeleri uygulama içinde kapalı veya güvenli bir şekilde tamamlandı](#redirect-safe)</li><li>[Denetleyici yöntemleri tarafından kabul edilen tüm dize türü parametrelerinin giriş doğrulamalarındaki gerçekleştir](#string-method)</li><li>[Normal ifade hatalı normal ifadeler nedeniyle DoS önlemek için işleme için üst sınır zaman aşımını ayarlayın](#dos-expression)</li><li>[Razor görünümleri Html.Raw kullanmaktan kaçının](#html-razor)</li></ul> | 
| **Veritabanı** | <ul><li>[Dinamik sorgular saklı yordamları kullanma](#stored-proc)</li></ul> |
| **Web API** | <ul><li>[Model doğrulama Web API yöntemleri üzerinde yapıldığından emin olun](#validation-api)</li><li>[Web API yöntemleri tarafından kabul edilen tüm dize türü parametrelerinin giriş doğrulamalarındaki gerçekleştir](#string-api)</li><li>[Tür kullanımı uyumlu parametreleri Web API'si veri erişimi için kullanıldığından emin olun](#typesafe-api)</li></ul> | 
| **Azure belge veritabanı** | <ul><li>[Azure Cosmos DB için parametreli SQL sorguları kullanın](#sql-docdb)</li></ul> | 
| **WCF** | <ul><li>[Şema bağlama aracılığıyla WCF girişi doğrulama](#schema-binding)</li><li>[Parametre denetçiler aracılığıyla WCF girişi doğrulama](#parameters)</li></ul> |

## <a id="disable-xslt"></a>XSLT güvenilmeyen stil sayfaları kullanarak tüm dönüştürmeler için betik oluşturma devre dışı bırak

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [XSLT güvenlik](https://msdn.microsoft.com/library/ms763800(v=vs.85).aspx), [XsltSettings.EnableScript özelliği](https://msdn.microsoft.com/library/system.xml.xsl.xsltsettings.enablescript.aspx) |
| **Adımları** | XSLT destekleyen stil sayfaları kullanarak betik `<msxml:script>` öğesi. Bu, bir XSLT dönüşümünde kullanılacak özel işlevler sağlar. Komut dosyası, dönüşüm gerçekleştirme işlem bağlamı altında yürütülür. XSLT betik güvenilmeyen kodun yürütülmesini önlemek için bir ortamda güvenilmeyen zaman devre dışı bırakılmalıdır. *.NET kullanıyorsanız:* XSLT betik, varsayılan olarak devre dışıdır; Ancak, bunu açıkça aracılığıyla nebyl povolen emin emin olmalısınız `XsltSettings.EnableScript` özelliği.|

### <a name="example"></a>Örnek 

```csharp
XsltSettings settings = new XsltSettings();
settings.EnableScript = true; // WRONG: THIS SHOULD BE SET TO false
```

### <a name="example"></a>Örnek
XSLT betik, MSXML 6.0 kullanıyorsanız, varsayılan olarak devre dışıdır; Ancak, bunu açıkça XML DOM nesne özelliği AllowXsltScript nebyl povolen emin emin olmanız gerekir. 

```csharp
doc.setProperty("AllowXsltScript", true); // WRONG: THIS SHOULD BE SET TO false
```

### <a name="example"></a>Örnek
MSXML 5 kullanıyorsanız, aşağıdaki XSLT betik varsayılan olarak etkindir ve açıkça gerekir durumunda bunu devre dışı. AllowXsltScript XML DOM nesne özelliği false olarak ayarlayın. 

```csharp
doc.setProperty("AllowXsltScript", false); // CORRECT. Setting to false disables XSLT scripting.
```

## <a id="out-sniffing"></a>Kullanıcı denetlenebilir içerik içerebilen her sayfanın MIME otomatik algılaması dışında bölgedeyse emin olun.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [IE8 Güvenlik bölümü V - kapsamlı koruma](https://blogs.msdn.com/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx)  |
| **Adımları** | <p>Kullanıcı denetlenebilir içerik içerebilen her sayfa için HTTP üst bilgisi kullanmalısınız `X-Content-Type-Options:nosniff`. Bu gereksinimle birlikte uymak için gereken üst bilgi içeriği denetlenebilir kullanıcı içerebilecek sayfaları için sayfa tarafından ayarlayabilirsiniz veya genel olarak uygulamadaki tüm sayfalar için ayarlayabilirsiniz.</p><p>Her bir web sunucusundan teslim dosya türünün ilişkili bir sahip [MIME türü](https://en.wikipedia.org/wiki/Mime_type) (olarak da adlandırılan bir *içerik türü*), içeriği (diğer bir deyişle, resim, metin, uygulama, vb.) yapısını açıklar</p><p>X-içerik-türü-Options üstbilgisi içeriklerini MIME sızılmasını olmamalıdır belirtmek geliştiricilerinin sağlayan bir HTTP üstbilgisi ' dir. Bu üstbilginin MIME algılaması saldırıları azaltmak için tasarlanmıştır. Internet Explorer 8 (IE8) bu üst bilgisi için destek eklendi</p><p>Yalnızca Internet Explorer 8 (IE8) kullanıcıları, X-içerik-türü-seçeneklerinden faydalanır. Internet Explorer'ın önceki sürümleri şu anda X-içerik-türü-Options üstbilgisi dikkate almaz</p><p>Bir MIME algılaması çevirme özelliği uygulamak için yalnızca bilinen tarayıcılar olan Internet Explorer 8 (ve üzeri). Bu tarayıcılar da için söz dizimi dahil etmek için bilinen diğer tarayıcılar (Firefox, Safari, Chrome) benzer özellikleri uygulamak durumunda bu öneriyi güncelleştirilir</p>|

### <a name="example"></a>Örnek
Genel olarak, uygulamadaki tüm sayfalar için gereken üst bilgi etkinleştirmek için aşağıdakilerden birini yapın: 

* Uygulama Internet Information Services (IIS) 7 tarafından barındırılıyorsa, web.config dosyasında üst bilgi Ekle 

```
<system.webServer> 
  <httpProtocol> 
    <customHeaders> 
      <add name=""X-Content-Type-Options"" value=""nosniff""/>
    </customHeaders>
  </httpProtocol>
</system.webServer> 
```

* Genel Uygulama üstbilgisi Ekle\_BeginRequest 

``` 
void Application_BeginRequest(object sender, EventArgs e)
{
  this.Response.Headers[""X-Content-Type-Options""] = ""nosniff"";
} 
```

* Uygulama özel HTTP modülü 

``` 
public class XContentTypeOptionsModule : IHttpModule 
  {
    #region IHttpModule Members 
    public void Dispose() 
    { 

    } 
    public void Init(HttpApplication context)
    { 
      context.PreSendRequestHeaders += newEventHandler(context_PreSendRequestHeaders); 
    } 
    #endregion 
    void context_PreSendRequestHeaders(object sender, EventArgs e) 
      { 
        HttpApplication application = sender as HttpApplication; 
        if (application == null) 
          return; 
        if (application.Response.Headers[""X-Content-Type-Options ""] != null) 
          return; 
        application.Response.Headers.Add(""X-Content-Type-Options "", ""nosniff""); 
      } 
  } 

``` 

* Gerekli başlık yalnızca belirli bir sayfa için bireysel yanıtlarını ekleyerek etkinleştirebilirsiniz: 

```
this.Response.Headers[""X-Content-Type-Options""] = ""nosniff""; 
``` 

## <a id="xml-resolution"></a>Sağlamlaştırma veya XML varlık çözümleme devre dışı bırakma

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [XML varlık genişletme](https://capec.mitre.org/data/definitions/197.html), [XML hizmet reddi saldırılarına ve savunma](https://msdn.microsoft.com/magazine/ee335713.aspx), [MSXML güvenliğine genel bakış](https://msdn.microsoft.com/library/ms754611(v=VS.85).aspx), [MSXML kod güvenliğini sağlamak için en iyi uygulamalar](https://msdn.microsoft.com/library/ms759188(VS.85).aspx), [ NSXMLParserDelegate Protokolü başvurusu](https://developer.apple.com/library/ios/#documentation/cocoa/reference/NSXMLParserDelegate_Protocol/Reference/Reference.html), [dış başvuruları çözümleme](https://msdn.microsoft.com/library/5fcwybb2.aspx) |
| **Adımları**| <p>Yaygın olarak kullanılmaz olsa da, XML ayrıştırıcının belge içinde veya dış kaynaklardan tanımlanan değerlerle makrosu varlıkları genişletme sağlayan XML özelliği yoktur. Örneğin, belgeyi bir varlık ile "Microsoft" değeri "companyname" şekilde tanımlayabilir, her seferinde metni "&companyname;" belgede bunu otomatik olarak değiştirilir Microsoft metinle görüntülenir. Veya belge varlık "Microsoft hisse senedi geçerli değerini almak için bir dış web hizmetini başvuran MSFTStock" tanımlayabilir.</p><p>Her zaman "&MSFTStock;" belgede bunu otomatik olarak değiştirilir geçerli hisse senedi fiyatı ile görünür. Ancak, bu işlev reddi (DoS) hizmet koşulları oluşturmak için kötüye kullanılabilecek. Bir saldırgan, sistemdeki tüm kullanılabilir bellek tüketen bir üstel genişletme XML bombasını oluşturmak için birden çok varlık yerleştirebilirsiniz. </p><p>Alternatif olarak, kendisinin geri akışı dış başvuru bir sonsuz veri miktarı oluşturabilir veya, yalnızca iş parçacığı askıda kalır. Tamamen uygulamasına değil kullanın veya el ile bellek ve bu işlev ise, uygulama varlık çözümlemesi için kullanabileceği süre miktarını sınırlamak sonuç olarak, tüm takımlar iç ve/veya dış varlık çözümleme XML devre dışı bırakmanız gerekir kesinlikle gerekli. Varlık çözümleme, uygulamanız tarafından gerekli değildir, devre dışı. </p>|

### <a name="example"></a>Örnek
.NET Framework kodu için aşağıdaki yaklaşımları kullanabilirsiniz:

```csharp
XmlTextReader reader = new XmlTextReader(stream);
reader.ProhibitDtd = true;

XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = true;
XmlReader reader = XmlReader.Create(stream, settings);

// for .NET 4
XmlReaderSettings settings = new XmlReaderSettings();
settings.DtdProcessing = DtdProcessing.Prohibit;
XmlReader reader = XmlReader.Create(stream, settings);
```
Unutmayın varsayılan değeri `ProhibitDtd` içinde `XmlReaderSettings` ancak true ise `XmlTextReader` false olur. Değerine kullanıyorsanız ProhibitDtd açıkça true olarak ayarlanmış gerekmez, ancak için güvenliği artırmak amacıyla, yapmanız önerilir. Ayrıca XmlDocument sınıfı varsayılan olarak varlık çözümleme izin verdiğini unutmayın. 

### <a name="example"></a>Örnek
Varlık çözümleme için XML belgelerine uymasıdır devre dışı bırakma, `XmlDocument.Load(XmlReader)` aşırı yükleme yöntemi ve aşağıdaki kodda gösterildiği gibi XmlReader bağımsız değişkeninde çözünürlüğünü devre dışı bırakmak için uygun özellikleri ayarlayın: 

```csharp
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = true;
XmlReader reader = XmlReader.Create(stream, settings);
XmlDocument doc = new XmlDocument();
doc.Load(reader);
```

### <a name="example"></a>Örnek
Varlık çözümlemeyi devre dışı bırakma, uygulamanız için mümkün değilse, uygulamanızın ihtiyaçlarına göre farklı makul bir değer XmlReaderSettings.MaxCharactersFromEntities özelliğini ayarlayın. Bu, üstel genişletme DoS saldırıları olası etkisini sınırlar. Aşağıdaki kod, bu yaklaşımın bir örnek sağlar: 

```csharp
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = false;
settings.MaxCharactersFromEntities = 1000;
XmlReader reader = XmlReader.Create(stream, settings);
```

### <a name="example"></a>Örnek
Satır içi varlıkları çözmek ancak bunu yapmanız durumunda dış varlıklar, çözmek için gerekmez XmlReaderSettings.XmlResolver özelliği null olarak ayarlayın. Örneğin: 

```csharp
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = false;
settings.MaxCharactersFromEntities = 1000;
settings.XmlResolver = null;
XmlReader reader = XmlReader.Create(stream, settings);
```
Msxml6 içinde varsayılan olarak true (devre dışı bırakma DTD işleme) ProhibitDTD ayarlandığına dikkat edin. Apple OSX/iOS kod için kullanabileceğiniz iki XML Çözümleyicileri vardır: NSXMLParser ve libXML2. 

## <a id="app-verification"></a>HTTP.sys kullanan uygulamalar URL Standartlaştırma doğrulama gerçekleştirme

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Http.sys kullanan tüm uygulamaları bu yönergeleri izlemelidir:</p><ul><li>URL uzunluğu (ASCII veya Unicode) en fazla 16.384 karakterle sınırlayın. Varsayılan Internet Information Services (IIS) 6 ayarına göre mutlak URL uzunluğu üst sınırı budur. Web siteleri bu değerden daha kısa bir süre için mümkünse çaba göstermelisiniz</li><li>Bu avantajı Standartlaştırma kuralların içinde .NET FX olacağından (örneğin, FILESTREAM) standart .NET Framework dosyası g/ç sınıfları kullanın</li><li>Açıkça bilinen dosya adları içeren bir izin verilenler listesi oluşturun</li><li>Açıkça bilinen dosya türleri, olmayan hizmet edecektir UrlScan reddeder Reddet: exe, bat, cmd, com, htw, ida, idq, htr, IDC, shtm [m], stm, yazıcı, INI, pol, dat dosyaları</li><li>Aşağıdaki özel durumları yakalayabilirsiniz:<ul><li>System.ArgumentException (için cihaz adları)</li><li>(İçin veri akışları) System.NotSupportedException</li><li>(Geçersiz kaçış dosya adları için) System.IO.FileNotFoundException</li><li>System.IO.directorynotfoundexception (için geçersiz kaçış dizin)</li></ul></li><li>*Sağlamadığı* Win32 dosya g/ç API'leri için duyurmak. Geçersiz bir URL, düzgün bir şekilde 400 hatası kullanıcıya döndürür ve gerçek bir hata oturum.</li></ul>|

## <a id="controls-users"></a>Kullanıcıların dosyaları kabul ederken uygun denetimleri yerinde olduğundan emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Karşıya dosya yükleme Kısıtlanmamış](https://www.owasp.org/index.php/Unrestricted_File_Upload), [dosya imza tablosu](https://www.garykessler.net/library/file_sigs.html) |
| **Adımları** | <p>Karşıya yüklenen dosya, uygulamalar için önemli bir risk temsil eder.</p><p>İlk adımda sayıda saldırı Saldırıya uğrayan için sisteme biraz kod almaktır. Ardından saldırı yalnızca bir şekilde yürütülen kodu alma bulması gerekir. Dosyayı karşıya yükleme kullanarak ilk adımı gerçekleştirmek saldırgan yardımcı olur. Sınırsız dosya karşıya yükleme sonuçlarını tam sistem devralma, aşırı yüklenmiş bir dosya sistemi ya da veritabanı, arka uç sistemleri ve basit tahrifatı saldırıları iletme de dahil olmak üzere farklılık gösterebilir.</p><p>Bu uygulama ile karşıya yüklenen dosya yapar ve özellikle depolandığı bağlıdır. Dosya yüklemeleri, sunucu tarafı doğrulama eksik. Güvenlik denetimleri şu dosya karşıya yükleme işlevselliği için uygulanması gereken:</p><ul><li>Dosya uzantısı onay (izin verilen dosya türü için geçerli bir ayar kabul)</li><li>En büyük dosya boyutu sınırı</li><li>Dosya için webroot yüklenmelidir değil; konumun sistem dışı sürücü bir dizin olmalıdır</li><li>Adlandırma kuralını takip edilmelidir, karşıya yüklenen dosya adına sahip dosyanın engellemek için bazı doğrulukla, üzerine yazar</li><li>Dosyaları diske yazmadan önce virüsten koruma için taranmalıdır</li><li>Dosya adı ve diğer meta verileri (örneğin, dosya yolu) için kötü amaçlı karakter doğrulandığından emin olun.</li><li>Dosya biçimi imzası iade edilmelidir, bir kullanıcının masqueraded dosya karşıya yükleme önlemek için (örneğin, bir exe dosyası için txt uzantısı değiştirerek karşıya yükleme)</li></ul>| 

### <a name="example"></a>Örnek
Dosya biçimi imza doğrulaması ile ilgili son noktası için Ayrıntılar için sınıfına bakın: 

```csharp
        private static Dictionary<string, List<byte[]>> fileSignature = new Dictionary<string, List<byte[]>>
                    {
                    { ".DOC", new List<byte[]> { new byte[] { 0xD0, 0xCF, 0x11, 0xE0, 0xA1, 0xB1, 0x1A, 0xE1 } } },
                    { ".DOCX", new List<byte[]> { new byte[] { 0x50, 0x4B, 0x03, 0x04 } } },
                    { ".PDF", new List<byte[]> { new byte[] { 0x25, 0x50, 0x44, 0x46 } } },
                    { ".ZIP", new List<byte[]> 
                                            {
                                              new byte[] { 0x50, 0x4B, 0x03, 0x04 },
                                              new byte[] { 0x50, 0x4B, 0x4C, 0x49, 0x54, 0x55 },
                                              new byte[] { 0x50, 0x4B, 0x53, 0x70, 0x58 },
                                              new byte[] { 0x50, 0x4B, 0x05, 0x06 },
                                              new byte[] { 0x50, 0x4B, 0x07, 0x08 },
                                              new byte[] { 0x57, 0x69, 0x6E, 0x5A, 0x69, 0x70 }
                                                }
                                            },
                    { ".PNG", new List<byte[]> { new byte[] { 0x89, 0x50, 0x4E, 0x47, 0x0D, 0x0A, 0x1A, 0x0A } } },
                    { ".JPG", new List<byte[]>
                                    {
                                              new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
                                              new byte[] { 0xFF, 0xD8, 0xFF, 0xE1 },
                                              new byte[] { 0xFF, 0xD8, 0xFF, 0xE8 }
                                    }
                                    },
                    { ".JPEG", new List<byte[]>
                                        { 
                                            new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
                                            new byte[] { 0xFF, 0xD8, 0xFF, 0xE2 },
                                            new byte[] { 0xFF, 0xD8, 0xFF, 0xE3 }
                                        }
                                        },
                    { ".XLS", new List<byte[]>
                                            {
                                              new byte[] { 0xD0, 0xCF, 0x11, 0xE0, 0xA1, 0xB1, 0x1A, 0xE1 },
                                              new byte[] { 0x09, 0x08, 0x10, 0x00, 0x00, 0x06, 0x05, 0x00 },
                                              new byte[] { 0xFD, 0xFF, 0xFF, 0xFF }
                                            }
                                            },
                    { ".XLSX", new List<byte[]> { new byte[] { 0x50, 0x4B, 0x03, 0x04 } } },
                    { ".GIF", new List<byte[]> { new byte[] { 0x47, 0x49, 0x46, 0x38 } } }
                };

        public static bool IsValidFileExtension(string fileName, byte[] fileData, byte[] allowedChars)
        {
            if (string.IsNullOrEmpty(fileName) || fileData == null || fileData.Length == 0)
            {
                return false;
            }

            bool flag = false;
            string ext = Path.GetExtension(fileName);
            if (string.IsNullOrEmpty(ext))
            {
                return false;
            }

            ext = ext.ToUpperInvariant();

            if (ext.Equals(".TXT") || ext.Equals(".CSV") || ext.Equals(".PRN"))
            {
                foreach (byte b in fileData)
                {
                    if (b > 0x7F)
                    {
                        if (allowedChars != null)
                        {
                            if (!allowedChars.Contains(b))
                            {
                                return false;
                            }
                        }
                        else
                        {
                            return false;
                        }
                    }
                }

                return true;
            }

            if (!fileSignature.ContainsKey(ext))
            {
                return true;
            }

            List<byte[]> sig = fileSignature[ext];
            foreach (byte[] b in sig)
            {
                var curFileSig = new byte[b.Length];
                Array.Copy(fileData, curFileSig, b.Length);
                if (curFileSig.SequenceEqual(b))
                {
                    flag = true;
                    break;
                }
            }

            return flag;
        }
```

## <a id="typesafe"></a>Tür kullanımı uyumlu parametre veri erişimi için Web uygulamasında kullanıldığından emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>SQL değerlendirir, parametre koleksiyonunu kullanırsanız, yürütülebilir kod olarak yerine değişmez değer olarak bir giriş olabilir. Parametreler koleksiyonu, giriş veri türü ve uzunluğu kısıtlamaları uygulamak için kullanılabilir. Değerler aralığının dışında bir özel durum tetikleyin. Saldırganlar, tür kullanımı uyumlu SQL parametreler kullanılmazsa, filtrelenmemiş girişinde katıştırılmış ekleme saldırılarını yürütebilmek için olabilir.</p><p>Güvenli tür parametreleri, filtrelenmemiş giriş ile ortaya çıkabilecek olası SQL ekleme saldırıları önlemek için SQL sorguları oluştururken kullanın. Saklı yordamlar ve dinamik SQL deyimleri, tür güvenli parametreleri kullanabilirsiniz. Parametreleri yürütülebilir kodu değil de, veritabanı tarafından değişmez değerler olarak kabul edilir. Parametreler, türünü ve uzunluğu için de denetlenir.</p>|

### <a name="example"></a>Örnek 
Aşağıdaki kod bir saklı yordam çağrılırken güvenli tür parametreleri ile SqlParameterCollection kullanma işlemini gösterir. 

```csharp
using System.Data;
using System.Data.SqlClient;

using (SqlConnection connection = new SqlConnection(connectionString))
{ 
DataSet userDataset = new DataSet(); 
SqlDataAdapter myCommand = new SqlDataAdapter("LoginStoredProcedure", connection); 
myCommand.SelectCommand.CommandType = CommandType.StoredProcedure; 
myCommand.SelectCommand.Parameters.Add("@au_id", SqlDbType.VarChar, 11); 
myCommand.SelectCommand.Parameters["@au_id"].Value = SSN.Text; 
myCommand.Fill(userDataset);
}  
```
Önceki kod örneğinde, giriş değeri 11 karakterden uzun olamaz. Veri türü veya parametresi tarafından tanımlanan uzunluk uymayan SqlParameter sınıfı bir özel durum oluşturur. 

## <a id="binding-mvc"></a>Ayrı model bağlama sınıfları kullanın veya MVC yığın atama güvenlik açığını önlemek için bağlama filtresi listeler

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC5, MVC6 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Meta veri öznitelikleri](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.metadatatypeattribute), [ortak anahtarı Güvenlik Açığı ve risk azaltma](https://github.com/blog/1068-public-key-security-vulnerability-and-mitigation), [yığın atama ASP.NET mvc'de Kılavuzu](https://odetocode.com/Blogs/scott/archive/2012/03/11/complete-guide-to-mass-assignment-in-asp-net-mvc.aspx), [MVC kullanarak EF ile çalışmaya başlama](https://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost) |
| **Adımları** | <ul><li>**Güvenlik açıklarını göndermek için aşırı zaman görünmelidir? -** Fazladan güvenlik açıklarını yayınlayarak giriş kullanıcıdan model sınıfları bağlama herhangi bir yerde meydana gelebilir. MVC gibi çerçeveleri düz eski CLR nesnelerini (POCOs) dahil olmak üzere, .NET sınıf özel kullanıcı verilerini temsil edebilir. MVC kullanıcı girişi başa çıkmak için kullanışlı bir gösterim sağlayarak bu istek verileri ile model sınıfları otomatik olarak doldurur. Bu sınıfların kullanıcı tarafından ayarlanmamalıdır özellikleri eklerken, uygulamanın uygulama hiçbir zaman hedeflenen veri kullanıcı denetimi sağlayan saldırıları aşırı kayıtlarına savunmasız olabilir. MVC model bağlama gibi veritabanına erişim Entity Framework gibi nesne/ilişkisel azaltıcının gibi teknolojileri genellikle ayrıca veritabanı verileri temsil etmek için POCO nesnelerini kullanma desteği. Bu veri modeli sınıfları kullanıcı girişi ile ilgili MVC gibi veritabanı verilerle ilgili de aynı kolaylık sağlar. MVC hem veritabanı POCO nesneleri gibi benzer modelleri desteklediğinden kolayca iki amaçları için aynı sınıf yeniden gibi görünüyor. Görev ayrımı nettir korumak bu yöntem başarısız olur ve bu istenmeyen özellikleri fazla posta saldırılar yaşanabileceğini gösterir, model bağlama için Burada sunulan tek bir ortak alan.</li><li>**Neden benim filtrelenmemiş veritabanı modeli sınıfları parametreler my MVC eylemleri kullanabilir olmaması gerekir? -** Çünkü MVC model bağlama bağlama herhangi bir şey bu sınıfta. Eylem veritabanı sınıfı şekli için kullanıcı girişi kabul edeceği veri olduğunu söylüyor çünkü veri görünümü'nde görünmez, kötü niyetli bir kullanıcı bu verileri kullanarak bir HTTP isteği gönderebilir ve MVC gladly olacak olsa bile bağlayın.</li><li>**Model bağlama için kullanılan şekli hakkında neden dikkat etmelisiniz? -** Aşırı geniş kapsamlı modelleri kullanarak ASP.NET MVC model bağlamayla saldırıları aşırı yayınlayarak uygulamanın kullanıma sunar. Uygulama verileri bir öğe için fiyat ya da bir hesap için güvenlik ayrıcalıkları geçersiz kılma gibi yönelik Geliştirici ötesinde değiştirmek saldırganlar fazla posta sağlayabilir. Uygulamaların hangi güvenilir olmayan girişlere model bağlama aracılığıyla izin vermek için eyleme özgü modelleri (veya belirli bir izin verilen özellik filtresi listeleri) açık bir sağlamak için bağlama sözleşme kullanmanız gerekir.</li><li>**Yalnızca kod çoğaltma ayrı bir bağlama modelleri yaşıyor? -** Hayır, bu görev ayrımı nettir sağlasa da kullanılabilir. Eylem yöntemleri veritabanı modellerinde yeniden kullanırsanız, bir HTTP isteği kullanıcı tarafından sınıf ayarlanmış herhangi bir özelliği (veya alt özellik) dediğini. MVC yapmak için istediğinize değil ise, bir filtre listesi veya hangi verileri, bunun yerine Giriş kullanıcıdan gelebilir MVC göstermek için ayrı bir sınıf şeklinin gerekir.</li><li>**Kullanıcı girişi için ayrı bir bağlama modelleri sahipsem my veri ek açıklama öznitelikleri yinelenen sahip? -** Gerekmez. Meta model bağlama sınıfı bağlamak için veritabanı model sınıfını MetadataTypeAttribute kullanabilirsiniz. Yalnızca MetadataTypeAttribute tarafından başvurulan türü bir alt kümesini (daha az özellik ancak fazla sağlayabilirsiniz) başvuru türü olması gerektiğini unutmayın.</li><li>**Kullanıcı Giriş modelleri ve veritabanı modelleri arasında veri ve geriye taşıma yorucu olabilir. Yalnızca de yansıma kullanarak tüm özelliklerini kopyalayabilir miyim? -** Evet. Bağlama modellerinde görüntülenen özellikler yalnızca kullanıcı girişi için güvenli olarak belirlediğiniz olanlardır. Yaygın olarak kullanılan bu iki modeli arasında bulunan tüm özellikler üzerinde kopyalamak için yansıma kullanarak engelleyen güvenlik neden yoktur.</li><li>**Peki [bağlama (hariç = "â €¦")]. Ayrı bir bağlama modelleri yerine kullanabilir miyim? -** Bu yaklaşım önerilmez. Kullanma [bağlama (hariç = "â €¦")] herhangi bir yeni özelliği varsayılan olarak bağlanabilir olduğu anlamına gelir. Yeni bir özellik eklendiğinde, güvenli bir anlatım gözetildiği basitçe hatırlanan fazladan bir adım yoktur. yerine varsayılan olarak güvenli tasarım sahip. Geliştirici bağlı olarak, bir özelliği her eklendiğinde, bu liste denetimi risklidir.</li><li>**Olduğundan [bağlama (içerme = "â €¦")] düzenleme işlemleri için kullanışlı? -** No [Bağlama (içerme = "â €¦")] yalnızca ekleme stili işlemleri (yeni veriler ekleme) için uygundur. (Mevcut verilerin düzeltilmesi) güncelleştirme stili işlemleri için ayrı bir bağlama modelleri olması veya izin verilen özellikleri açık bir listesi için UpdateModel veya TryUpdateModel geçirme gibi başka bir yaklaşım kullanın. Ekleme bir [bağlama (içerme = "â €¦")] özniteliği bir düzenleme işlemi anlamına gelir MVC nesnesi örneği oluşturun ve diğer tüm varsayılan değerlerinde bırakın yalnızca listelenen özellikleri ayarlayın. Verileri kalıcı hale getirilir, tamamen belirtilmemiş tüm özellikler için değerleri varsayılan değerlerine sıfırlama var olan bir varlığa yerini alır. Örneğin IsAdmin ihmal edildiğini, bir [bağlama (içerme = "â €¦")] bir düzenleme işlemi, adı bu eylem düzenlenmiş herhangi bir kullanıcı özniteliği için IsAdmin sıfırlamak = false (düzenlenen herhangi bir kullanıcının yönetici durumu kaybeder). Belirli özellikleri için güncelleştirmeler engellemek istiyorsanız, diğer yaklaşımlardan birini kullanın. MVC araçları bazı sürümleri ile denetleyici sınıflarına oluşturmak, Not [bağlama (içerme = "â €¦")] eylemleri düzenlemek ve yaptığından bu listeden bir özelliğini kaldırmayı fazla posta saldırıları engeller. Ancak, yukarıda açıklandığı gibi bu yaklaşımı beklendiği gibi çalışmaz ve bunun yerine belirtilmemiş özelliklerinde herhangi bir veri varsayılan değerlerine sıfırlar.</li><li>**Kullanarak tüm uyarılar oluşturma işlemlerinde vardır [bağlama (içerme = "â €¦")] ayrı bağlama modelleri yerine? -** Evet. İlk iki ayrı yaklaşım tüm fazla posta güvenlik açıklarını azaltmaya koruma gerektiren, düzenleme senaryoları için bu yaklaşım çalışmaz. İkinci ve ayrı bir bağlama modelleri zorlamak için kullanıcı girişi kullanılan şeklini Kalıcılık için kullanılan şeklini arasındaki ayrımı bir şey [bağlama (içerme = "â €¦")] yapın. Üçüncü unutmayın [bağlama (içerme = "â €¦")] yalnızca üst düzey özellikler; işleyebilir öznitelik, yalnızca alt özellikler (örneğin, "Details.Name") bölümleri izin veremez. Son olarak ve belki de en önemlisi de kullanarak [bağlama (içerme = "â €¦")] sınıfı için istediğiniz zaman anımsanacak gerekir fazladan bir adım model bağlama için kullanılan ekler. Yeni bir eylem yöntemini doğrudan veri sınıfına bağlar ve dahil etmek unutması durumunda bir [bağlama (Ekle = "â €¦")] özniteliği olabilir saldırıları, aşırı gönderme için güvenlik açığı böylece [bağlama (Ekle = "â €¦")] yaklaşım varsayılan olarak biraz daha az güvenlidir. Kullanırsanız [bağlama (içerme = "â €¦")], her zaman, veri sınıfları eylem metodu parametreleriyle görünen her zaman belirtmeden hatırlamak için dikkatli olun.</li><li>**Oluşturma işlemleri için ne koyarak [bağlama (içerme = "â €¦")] model sınıfı özniteliği? Bu yaklaşım her eylem yöntemi hakkında öznitelik koyarak unutmayın gerek önlenir? -** Bu yaklaşım bazı durumlarda çalışır. Kullanarak [bağlama (içerme = "â €¦")] model türü (yerine bu sınıfı kullanarak eylem parametreleri), belirtilmesinin gerekmemesi dahil etmeyi unutmayın [bağlama (Ekle = "â €¦")] özniteliği her eylem yöntemi. Öznitelik doğrudan sınıf üzerinde etkili bir şekilde kullanarak, model bağlama amacıyla bu sınıfın ayrı bir yüzey oluşturur. Ancak, bu yaklaşım, model bağlama bir şeklin model sınıfı başına yalnızca sağlar. Model bağlama bir alanın (kullanıcı rolleri güncelleştiren Örneğin, yalnızca yönetici eylem) izin vermek bir eylem yöntemi gerekir ve bu alanın model bağlama önlemek diğer eylemler ihtiyacınız varsa, bu yaklaşım işe yaramaz. Her sınıf, yalnızca bir model bağlama şekil olabilir; farklı eylemler farklı model bağlama şekiller gerekiyorsa, iki ayrı model bağlama sınıfları kullanarak bu ayrı şekilleri gösterir veya ayırmak ihtiyaç duydukları [bağlama (içerme = "â €¦")] özniteliklerinde eylem yöntemleri.</li><li>**Model bağlama ne? Bunlar görünüm modeli olarak aynı şey midir? -** Bu iki ilişkili kavramlardır. (Şekil MVC model bağlamanın dışında eylem yöntemine geçirilen) bir eylemin parametre listesinde kullanılan bir model sınıfı terimi bağlama modelini ifade eder. Terim görünüm modeli eylem yönteminden bir görünüme iletilen bir model sınıfı ifade eder. Bir özel görünüm modeli kullanarak bir görünüm için bir eylem yöntemindeki verileri geçirmek için yaygın bir yaklaşımdır. Genellikle bu şeklin model bağlama için uygundur ve terimi görünüm modeli, her iki yerde de kullanılan aynı modelin başvurmak için kullanılabilir. Kesin olarak için bu yordamı yığın atama amacıyla şeylere olduğu eyleme geçirilen şekli odaklanan modelleri, özellikle bağlama hakkında konuşuyor.</li></ul>| 

## <a id="rendering"></a>Güvenilmeyen web çıkış işleme önce kodlayın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Generic, Web Forms, MVC5, MVC6 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [ASP.NET'te siteler arası betik kullanmayı önleme nasıl](https://msdn.microsoft.com/library/ms998274.aspx), [siteler arası komut dosyası](https://cwe.mitre.org/data/definitions/79.html), [XSS (siteler arası komut dosyası) önleme kural sayfası](https://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet) |
| **Adımları** | Siteler arası komut dosyası (XSS yaygın olarak kısaltılır), bir saldırı vektörüdür çevrimiçi hizmetler veya tüm uygulama/Web giriş tüketen bileşeni olur. XSS güvenlik açıkları, bir saldırganın bir güvenlik açığı bulunan web uygulaması aracılığıyla başka bir kullanıcının makinede komut dosyası yürütme. Kötü amaçlı komut dosyalarını, tanımlama bilgilerini çalabilir ve JavaScript aracılığıyla bir kurbanın makine aksi değiştirmesine için kullanılabilir. Kullanıcı girişini doğrulama, iyi biçimlendirilmiş sağlamak ve bir web sayfasında işlenmeden önce kodlama XSS engelledi. Giriş doğrulama ve çıkış kodlama Web koruma kitaplığı kullanılarak yapılabilir. Yönetilen kod için (C\#, VB.NET, vb.), bir veya daha uygun olduğu kullanıcı girişini bildirilen bağlama bağlı olarak Web koruma (Anti-XSS) Kitaplığı'ndan kodlama yöntemleri:| 

### <a name="example"></a>Örnek

```csharp
* Encoder.HtmlEncode 
* Encoder.HtmlAttributeEncode 
* Encoder.JavaScriptEncode 
* Encoder.UrlEncode
* Encoder.VisualBasicScriptEncode 
* Encoder.XmlEncode 
* Encoder.XmlAttributeEncode 
* Encoder.CssEncode 
* Encoder.LdapEncode 
```

## <a id="typemodel"></a>Giriş doğrulaması ve Model özellikleri tüm dize türü üzerinde filtreleme yapmak

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, MVC5, MVC6 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Doğrulama ekleme](https://www.asp.net/mvc/overview/getting-started/introduction/adding-validation), [bir MVC uygulamasında Model verileri doğrulama](https://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), [ilkeler, ASP.NET MVC uygulamaları için kılavuzluk](https://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Adımları** | <p>Uygulamayı kötü amaçlı kullanıcı girişleri karşı korunması emin olmak için uygulamada kullanılmadan önce tüm giriş parametrelerini doğrulanması gerekir. Sunucu tarafında bir beyaz liste doğrulama stratejisi ile normal ifade doğrulamaları kullanarak giriş değerleri doğrulayın. Kullanıcı girişleri unsanitized / yöntemlere geçilen parametreleri kod ekleme güvenlik açıklarına neden olabilir.</p><p>Web uygulamaları için giriş noktaları form alanlarını, QueryStrings, tanımlama bilgisi, HTTP üst bilgileri ve web hizmeti parametreleri de dahil edebilirsiniz.</p><p>Model bağlama sırasında aşağıdaki giriş doğrulama denetimleri gerçekleştirilmesi gerekir:</p><ul><li>Yanıtta normal ifade ek açıklama, izin verilen karakterler ve izin verilen maksimum uzunluğu kabul etmek için model özellikleri açıklanmalıdır.</li><li>Denetleyici yöntemlerinde ModelState geçerlilik gerçekleştirmeniz gerekir</li></ul>|

## <a id="richtext"></a>Tüm karakterler, örn, Zengin Metin Düzenleyicisi'ni kabul edin. form alanlarını temizleme uygulanmalıdır

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Güvenli olmayan giriş kodlama](https://msdn.microsoft.com/library/ff647397.aspx#paght000003_step3), [HTML temizleyici](https://github.com/mganss/HtmlSanitizer) |
| **Adımları** | <p>Kullanmak istediğiniz tüm static işaretleme etiketlerini tanımlayın. Güvenli HTML öğeleri için aşağıdakiler gibi biçimlendirme sınırlandırmak için ortak bir uygulama olan `<b>` (kalın) ve `<i>` (italik).</p><p>HTML kodlama verileri yazmadan önce. Bu kötü amaçlı bir komut dosyası yürütülebilir kod olarak değil bir metin olarak işlenmesi için neden tarafından güvenli hale getirir.</p><ol><li>ValidateRequest ekleyerek ASP.NET istek doğrulamayı devre dışı bırak = "false" özniteliği için \@ sayfa yönergesi</li><li>Bir dize girişi HtmlEncode yöntemi ile kodlayın</li><li>Bir StringBuilder kullanın ve izin vermek istediğiniz HTML öğelerinde kodlama seçmeli olarak kaldırmak için kendi Değiştir yöntemini çağırın</li></ol><p>Sayfa açma ayarlayarak başvuruları devre dışı bırakır ASP.NET istek doğrulamayı `ValidateRequest="false"`. Bu HTML olarak kodlar giriş ve seçmeli olarak tanır `<b>` ve `<i>` alternatif olarak, bir .NET kitaplığı HTML Temizleme için de kullanılabilir.</p><p>HtmlSanitizer HTML parçaları ve XSS saldırılarına yol açabilecek yapıları belgelerden temizlemek için bir .NET Kitaplığı ' dir. AngleSharp ayrıştırma, işlemek ve HTML ve CSS işlemek için kullanır. Bir NuGet paketi olarak HtmlSanitizer yüklenebilir ve kullanıcı girişi ile ilgili HTML veya CSS temizleme yöntemi, sunucu tarafında uygulanabilir olduğu durumlarda geçirilebilir. Lütfen unutmayın yalnızca son bir seçenek düşünülmesi gereken bir güvenlik denetimi olarak temizleme.</p><p>Giriş doğrulama ve çıkış kodlama daha iyi güvenlik denetimleri olarak kabul edilir.</p> |

## <a id="inbuilt-encode"></a>DOM öğeleri yerleşik kodlama olmayan havuzlarından atamayın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Çok sayıda javascript işlevleri varsayılan kodlamayı desteklemez. Bu tür işlevleri aracılığıyla DOM öğeleri için güvenilir olmayan girişlere atarken, site betik (XSS) yürütme sonuçlanabilir.| 

### <a name="example"></a>Örnek
Güvensiz örnekleri aşağıda verilmiştir: 

```
document.getElementByID("div1").innerHtml = value;
$("#userName").html(res.Name);
return $('<div/>').html(value)
$('body').append(resHTML);   
```
Kullanmayın `innerHtml`; bunun yerine kullanın `innerText`. Benzer şekilde, yerine, `$("#elm").html()`, kullanın `$("#elm").text()` 

## <a id="redirect-safe"></a>Tüm doğrulama yeniden yönlendirmeleri uygulama içinde kapalı veya güvenli bir şekilde tamamlandı

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [OAuth 2.0 yetkilendirme Framework - yeniden Aç](https://tools.ietf.org/html/rfc6749#section-10.15) |
| **Adımları** | <p>Kullanıcının sağladığı bir konuma yeniden yönlendirme gerektiren uygulama tasarımı, olası yeniden yönlendirme hedeflerini siteler veya etki alanları, önceden tanımlanmış "güvenli" listesini sınırlamak gerekir. Uygulamadaki tüm yeniden yönlendirmeleri kapalı güvenli olması gerekir.</p><p>Bunu yapmak için:</p><ul><li>Tüm yeniden yönlendirmeleri tanımlayın</li><li>Her bir yeniden yönlendirme için uygun bir risk azaltma uygular. Uygun bir risk azaltma işlemleri yeniden yönlendirme beyaz liste veya kullanıcı onayı içerir. Web sitesi veya bir açık yeniden yönlendirme güvenlik açığı hizmetiyle Facebook/OAuth/Openıd kimlik sağlayıcıları kullanıyorsa, bir saldırganın bir kullanıcının oturum açma belirtecinin çalabilir ve bu kullanıcının kimliğine bürün. Bu devralınan bir risk RFC 6749 "OAuth 2.0 yetkilendirme Framework" belgelenen OAuth, kullanıldığında, 10.15 "açın yönlendiren" bölümüne benzer şekilde, zıpkınla kimlik avı saldırıları tarafından açık yeniden yönlendirme kullanarak kullanıcıların kimlik bilgilerini tehlikeye</li></ul>|

## <a id="string-method"></a>Denetleyici yöntemleri tarafından kabul edilen tüm dize türü parametrelerinin giriş doğrulamalarındaki gerçekleştir

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, MVC5, MVC6 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Bir MVC uygulamasında model verileri doğrulama](https://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), [ilkeler, ASP.NET MVC uygulamaları için kılavuzluk](https://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Adımları** | Bağımsız değişken olarak yalnızca ilkel veri türü ve değil modelleri kabul yöntemleri, normal ifade kullanılırken, giriş doğrulaması yapılmalıdır. Burada bir geçerli bir normal ifade deseniyle Regex.IsMatch kullanılmalıdır. Giriş, belirtilen normal ifadeyle eşleşmez, denetimi daha fazla devam etmemelisiniz ve doğrulama hatası ile ilgili yeterli bir uyarı görüntülenir.| 

## <a id="dos-expression"></a>Normal ifade hatalı normal ifadeler nedeniyle DoS önlemek için işleme için üst sınır zaman aşımını ayarlayın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Generic, Web Forms, MVC5, MVC6  |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [DefaultRegexMatchTimeout Property](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.defaultregexmatchtimeout.aspx) |
| **Adımları** | Hizmet reddi saldırılarına karşı hatalı emin olmak için genel varsayılan zaman aşımı geri izlemenin çok neden, oluşturulan normal ifadeler ayarlayın. İşlem süresi tanımlı üst sınırından daha uzun sürerse, zaman aşımı özel durumlar oluşturan. Hiçbir şey yapılandırılmışsa, zaman aşımı sonsuz olacaktır.| 

### <a name="example"></a>Örnek
Örneğin, aşağıdaki yapılandırmayı işleme 5 saniyeden daha uzun sürerse bir RegexMatchTimeoutException atar: 

```csharp
<httpRuntime targetFramework="4.5" defaultRegexMatchTimeout="00:00:05" />
```

## <a id="html-razor"></a>Razor görünümleri Html.Raw kullanmaktan kaçının

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC5, MVC6 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| Adım | ASP.NET Web sayfaları (Razor) gerçekleştirmek otomatik HTML kodlaması. Otomatik olarak HTML ile kodlanmış tüm dizeleri gömülü kod nuggets (@ blokları) tarafından yazdırılan. Ancak, `HtmlHelper.Raw` yöntemi çağrıldığında, HTML kodlu olmayan biçimlendirme döndürür. Varsa `Html.Raw()` yardımcı yöntemi kullanılır, Razor sağladığı otomatik kodlama koruma atlar.|

### <a name="example"></a>Örnek
Güvenli olmayan bir örneği verilmiştir: 

```csharp
<div class="form-group">
            @Html.Raw(Model.AccountConfirmText)
        </div>
        <div class="form-group">
            @Html.Raw(Model.PaymentConfirmText)
        </div>
</div>
```
Kullanmayın `Html.Raw()` biçimlendirme görüntülenecek gerekmedikçe. Bu yöntem, dolaylı olarak kodlama çıkış gerçekleştirmez. Örneğin diğer ASP.NET Yardımcıları kullan `@Html.DisplayFor()` 

## <a id="stored-proc"></a>Dinamik sorgular saklı yordamları kullanma

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>SQL ekleme saldırısına veritabanında rasgele komutları çalıştırmak için giriş doğrulamasında güvenlik açıklarını. Uygulamanız veritabanına erişmek için dinamik SQL deyimlerini oluşturmak için giriş kullandığında oluşabilir. Kodunuzu ham kullanıcı girişi içeren dizelerle geçirilen saklı yordamlar kullanıyorsa da meydana gelebilir. Saldırgan, SQL ekleme saldırısına kullanarak veritabanına rasgele komutları yürütebilir. (Saklı yordamlarda SQL deyimleri dahil) tüm SQL deyimleri parametreli gerekir. Parametreli SQL deyimleri, türü kesin belirlenmiş olduğundan SQL (örneğin, tek tırnak işareti) özel bir anlamı sorunsuz sahip karakterleri kabul eder. |

### <a name="example"></a>Örnek
Güvenli olmayan dinamik saklı yordam örneği aşağıdadır: 

```csharp
CREATE PROCEDURE [dbo].[uspGetProductsByCriteria]
(
  @productName nvarchar(200) = NULL,
  @startPrice float = NULL,
  @endPrice float = NULL
)
AS
 BEGIN
  DECLARE @sql nvarchar(max)
  SELECT @sql = ' SELECT ProductID, ProductName, Description, UnitPrice, ImagePath' +
       ' FROM dbo.Products WHERE 1 = 1 '
       PRINT @sql
  IF @productName IS NOT NULL
     SELECT @sql = @sql + ' AND ProductName LIKE ''%' + @productName + '%'''
  IF @startPrice IS NOT NULL
     SELECT @sql = @sql + ' AND UnitPrice > ''' + CONVERT(VARCHAR(10),@startPrice) + ''''
  IF @endPrice IS NOT NULL
     SELECT @sql = @sql + ' AND UnitPrice < ''' + CONVERT(VARCHAR(10),@endPrice) + ''''

  PRINT @sql
  EXEC(@sql)
 END
```

### <a name="example"></a>Örnek
Güvenli bir şekilde uygulanan aynı saklı yordam aşağıda verilmiştir: 
```csharp
CREATE PROCEDURE [dbo].[uspGetProductsByCriteriaSecure]
(
             @productName nvarchar(200) = NULL,
             @startPrice float = NULL,
             @endPrice float = NULL
)
AS
       BEGIN
             SELECT ProductID, ProductName, Description, UnitPrice, ImagePath
             FROM dbo.Products where
             (@productName IS NULL or ProductName like '%'+ @productName +'%')
             AND
             (@startPrice IS NULL or UnitPrice > @startPrice)
             AND
             (@endPrice IS NULL or UnitPrice < @endPrice)         
       END
```

## <a id="validation-api"></a>Model doğrulama Web API yöntemleri üzerinde yapıldığından emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC5, MVC6 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [ASP.NET Web API'de model doğrulama](https://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api) |
| **Adımları** | Bir istemci bir web API'sine veri gönderdiğinde, herhangi bir işlem gerçekleştirmeden önce verileri doğrulamak için zorunludur. ASP.NET Web API'leri için kabul modelleri, giriş olarak veri ek açıklamaları modeller üzerinde doğrulama kuralları modelin özellikleri ayarlamak için kullanın.|

### <a name="example"></a>Örnek
Aşağıdaki kod aynı gösterir: 

```csharp
using System.ComponentModel.DataAnnotations;

namespace MyApi.Models
{
    public class Product
    {
        public int Id { get; set; }
        [Required]
        [RegularExpression(@"^[a-zA-Z0-9]*$", ErrorMessage="Only alphanumeric characters are allowed.")]
        public string Name { get; set; }
        public decimal Price { get; set; }
        [Range(0, 999)]
        public double Weight { get; set; }
    }
}
```

### <a name="example"></a>Örnek
API denetleyici eylem yöntemine, aşağıda gösterildiği gibi açıkça denetlenmesi modelinin geçerliliği vardır: 

```csharp
namespace MyApi.Controllers
{
    public class ProductsController : ApiController
    {
        public HttpResponseMessage Post(Product product)
        {
            if (ModelState.IsValid)
            {
                // Do something with the product (not shown).

                return new HttpResponseMessage(HttpStatusCode.OK);
            }
            else
            {
                return Request.CreateErrorResponse(HttpStatusCode.BadRequest, ModelState);
            }
        }
    }
}
```

## <a id="string-api"></a>Web API yöntemleri tarafından kabul edilen tüm dize türü parametrelerinin giriş doğrulamalarındaki gerçekleştir

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Generic, MVC 5, MVC 6 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Bir MVC uygulamasında model verileri doğrulama](https://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), [ilkeler, ASP.NET MVC uygulamaları için kılavuzluk](https://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Adımları** | Bağımsız değişken olarak yalnızca ilkel veri türü ve değil modelleri kabul yöntemleri, normal ifade kullanılırken, giriş doğrulaması yapılmalıdır. Burada bir geçerli bir normal ifade deseniyle Regex.IsMatch kullanılmalıdır. Giriş, belirtilen normal ifadeyle eşleşmez, denetimi daha fazla devam etmemelisiniz ve doğrulama hatası ile ilgili yeterli bir uyarı görüntülenir.|

## <a id="typesafe-api"></a>Tür kullanımı uyumlu parametreleri Web API'si veri erişimi için kullanıldığından emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>SQL değerlendirir, parametre koleksiyonunu kullanırsanız, yürütülebilir kod olarak yerine değişmez değer olarak bir giriş olabilir. Parametreler koleksiyonu, giriş veri türü ve uzunluğu kısıtlamaları uygulamak için kullanılabilir. Değerler aralığının dışında bir özel durum tetikleyin. Saldırganlar, tür kullanımı uyumlu SQL parametreler kullanılmazsa, filtrelenmemiş girişinde katıştırılmış ekleme saldırılarını yürütebilmek için olabilir.</p><p>Güvenli tür parametreleri, filtrelenmemiş giriş ile ortaya çıkabilecek olası SQL ekleme saldırıları önlemek için SQL sorguları oluştururken kullanın. Saklı yordamlar ve dinamik SQL deyimleri, tür güvenli parametreleri kullanabilirsiniz. Parametreleri yürütülebilir kodu değil de, veritabanı tarafından değişmez değerler olarak kabul edilir. Parametreler, türünü ve uzunluğu için de denetlenir.</p>|

### <a name="example"></a>Örnek
Aşağıdaki kod bir saklı yordam çağrılırken güvenli tür parametreleri ile SqlParameterCollection kullanma işlemini gösterir. 

```csharp
using System.Data;
using System.Data.SqlClient;

using (SqlConnection connection = new SqlConnection(connectionString))
{ 
DataSet userDataset = new DataSet(); 
SqlDataAdapter myCommand = new SqlDataAdapter("LoginStoredProcedure", connection); 
myCommand.SelectCommand.CommandType = CommandType.StoredProcedure; 
myCommand.SelectCommand.Parameters.Add("@au_id", SqlDbType.VarChar, 11); 
myCommand.SelectCommand.Parameters["@au_id"].Value = SSN.Text; 
myCommand.Fill(userDataset);
}  
```
Önceki kod örneğinde, giriş değeri 11 karakterden uzun olamaz. Veri türü veya parametresi tarafından tanımlanan uzunluk uymayan SqlParameter sınıfı bir özel durum oluşturur. 

## <a id="sql-docdb"></a>Cosmos DB için parametreli SQL sorguları kullanın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure belge veritabanı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Azure cosmos DB SQL Parametreleştirme Duyurusu](https://azure.microsoft.com/blog/announcing-sql-parameterization-in-documentdb/) |
| **Adımları** | Azure Cosmos DB yalnızca salt okunur sorguların destekler; ancak, SQL ekleme sorguları kullanıcı girişi ile birleştirerek oluşturulur, yine de mümkündür. Bunlar aynı koleksiyon içinde kötü amaçlı SQL sorguları kaynaklı tarafından erişim olmamalıdır verilere erişim elde etmek bir kullanıcı için olası olabilir. Sorguları oluşturulur, kullanıcı girişini temel alarak parametrelenmiş SQL sorgularını kullanın. |

## <a id="schema-binding"></a>Şema bağlama aracılığıyla WCF girişi doğrulama

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, NET Framework 3 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff647820.aspx) |
| **Adımları** | <p>Doğrulama eksikliği farklı tür enjeksiyon saldırılarına karşı yol açar.</p><p>İleti doğrulama WCF uygulamanızı korumasını savunma bir satırı temsil eder. Bu yaklaşımda, WCF Hizmeti işlemlerini kötü amaçlı bir istemci tarafından saldırılara karşı korumak için şemalar kullanarak iletileri doğrulayın. İstemci tarafından kötü amaçlı bir hizmete saldırılardan korumak için istemci tarafından alınan tüm iletileri doğrulayın. İleti doğrulama işlemleri ileti sözleşmeleri veya yapılamaz verileri sözleşmelerini tükettiğinizde iletileri doğrulamak mümkün kılar parametre doğrulaması kullanarak. İleti doğrulama şemaları, böylece daha fazla esneklik ve geliştirme süresini azaltmayı içinde Doğrulama mantığı oluşturmanızı sağlar. Şemaları veri temsilini standartları oluşturma kuruluşun içindeki farklı uygulamalar arasında yeniden kullanılabilir. Ayrıca, ileti doğrulama, bunlar iş mantığı temsil eden sözleşmeleri içeren daha karmaşık veri türleri tükettiğinizde işlemleri korumak sağlar.</p><p>İleti doğrulama gerçekleştirmek için önce hizmetiniz ve bu işlemler tarafından tüketilen veri türleri işlemlerini temsil eden bir şema oluşturun. Daha sonra bir özel istemci ileti denetçisi ve özel dağıtıcı ileti denetçisi hizmet içine/dışına gönderilen/alınan iletileri doğrulamak için uygulayan bir .NET sınıf oluşturursunuz. Ardından, hem istemci hem de hizmet ileti doğrulamasını etkinleştirmek için bir özel uç nokta davranışı uygular. Son olarak, hizmet veya istemci yapılandırma dosyasında genişletilmiş özel uç nokta davranışı kullanıma sunmanıza olanak tanıyan sınıfında özel bir yapılandırma öğesi uygulama"</p>|

## <a id="parameters"></a>Parametre denetçiler aracılığıyla WCF girişi doğrulama

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, NET Framework 3 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff647875.aspx) |
| **Adımları** | <p>Giriş ve veri doğrulama WCF uygulamanızı korumasını savunma önemli bir satırı temsil eder. WCF hizmet işlemlerinde hizmet saldırılardan korumak için kötü amaçlı bir istemci tarafından kullanıma sunulan tüm parametreleri doğrulamalıdır. Buna karşılık, ayrıca istemcinin kötü amaçlı bir hizmet tarafından saldırılardan korumak için istemci tarafından alınan tüm dönüş değerleri doğrulamalıdır</p><p>WCF özel uzantıları oluşturarak WCF çalışma zamanı davranışını özelleştirmenizi farklı genişletilebilirlik noktaları sağlar. İleti denetçileri ve parametre denetçiler bir istemci ve hizmet arasında geçen verileri üzerinde daha fazla denetim elde etmek için kullanılan iki genişletilebilirlik mekanizmasıdır. Parametre denetçiler giriş doğrulama yapmak için kullanılan ve hizmet içine ve dışına akan tüm ileti incelemek yalnızca ihtiyacınız olduğunda ileti denetçileri kullanmalısınız.</p><p>Giriş doğrulamayı gerçekleştirmek için bir .NET sınıfı oluşturmak ve hizmet işlemleri üzerinde parametreleri doğrulamak için bir özel parametre denetçisi uygulamak. Ardından, hem istemci hem de hizmet doğrulamasını etkinleştirmek için bir özel uç nokta davranışı uygular. Son olarak, hizmet veya istemci yapılandırma dosyasında genişletilmiş özel uç nokta davranışı kullanıma sunmanıza olanak tanıyan sınıfında özel bir yapılandırma öğesi uygular</p>|
