---
title: "Doğrulama - Microsoft tehdit modelleme aracı - Azure giriş | Microsoft Docs"
description: "Azaltıcı Etkenler tehdit modelleme Aracı kullanıma sunulan tehditleri"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: c416ae23565870223abc3f2db1ac460e8bea77f6
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="security-frame-input-validation--mitigations"></a>Güvenlik çerçevesi: Giriş doğrulama | Azaltıcı Etkenler 
| Ürün/hizmet | Makale |
| --------------- | ------- |
| **Web uygulaması** | <ul><li>[XSLT güvenilmeyen stil sayfalarını kullanarak tüm dönüşümler için komut dosyası devre dışı bırak](#disable-xslt)</li><li>[Kullanıcı denetlenebilir içeriği içerebilir her sayfanın MIME otomatik algılaması dışında çevrilir emin olun](#out-sniffing)</li><li>[Sağlamlaştırmak veya XML varlık çözümleme devre dışı bırakma](#xml-resolution)</li><li>[HTTP.sys kullanan uygulamalar URL Standartlaştırma doğrulama gerçekleştirme](#app-verification)</li><li>[Uygun denetimleri dosyaların kullanıcılardan kabul ederken karşılandığından emin olun](#controls-users)</li><li>[Tür kullanımı uyumlu parametreleri veri erişimi için Web uygulamasında kullanıldığından emin olun](#typesafe)</li><li>[Ayrı model bağlama sınıflarını kullanın veya MVC yığın atama güvenlik açığı önlemek için bağlama filtresi listeler](#binding-mvc)</li><li>[Güvenilmeyen web çıkış işleme önce kodlama](#rendering)</li><li>[Giriş doğrulaması ve tüm dize türünde Model özelliklerini filtrelemesine](#typemodel)</li><li>[Tüm karakterleri, örneğin, Zengin Metin Düzenleyicisi'ni kabul eden form alanlarını temizleme işlemi uygulanmalıdır](#richtext)</li><li>[DOM öğeleri yerleşik kodlama olmayan havuzlarını atamayın](#inbuilt-encode)</li><li>[Tüm doğrulama uygulama içinde yeniden yönlendirmeleri kapalı veya güvenli bir şekilde tamamlandı](#redirect-safe)</li><li>[Uygulama giriş doğrulaması denetleyicisi yöntemler tarafından kabul edilen tüm dize türü parametreleri](#string-method)</li><li>[Normal ifade DoS nedeniyle hatalı normal ifadeler önlemek için işleme için üst sınır zaman aşımını ayarlama](#dos-expression)</li><li>[Razor görünümleri Html.Raw kullanmaktan kaçının](#html-razor)</li></ul> | 
| **Veritabanı** | <ul><li>[Dinamik sorgular saklı yordamlarda kullanmayın](#stored-proc)</li></ul> |
| **Web API** | <ul><li>[Model doğrulama Web API yöntemlerini yapıldığından emin olun](#validation-api)</li><li>[Web API yöntemleri tarafından kabul edilen tüm dize tür parametrelerindeki giriş doğrulaması uygulama](#string-api)</li><li>[Tür kullanımı uyumlu parametreleri Web API'si veri erişimi için kullanıldığından emin olun](#typesafe-api)</li></ul> | 
| **Azure belge DB** | <ul><li>[Parametreli SQL sorguları için Azure Cosmos DB kullanın](#sql-docdb)</li></ul> | 
| **WCF** | <ul><li>[Şema bağlama aracılığıyla WCF girişi doğrulama](#schema-binding)</li><li>[Parametre denetçiler aracılığıyla WCF girişi doğrulama](#parameters)</li></ul> |

## <a id="disable-xslt"></a>XSLT güvenilmeyen stil sayfalarını kullanarak tüm dönüşümler için komut dosyası devre dışı bırak

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [XSLT güvenlik](https://msdn.microsoft.com/library/ms763800(v=vs.85).aspx), [XsltSettings.EnableScript özelliği](http://msdn.microsoft.com/library/system.xml.xsl.xsltsettings.enablescript.aspx) |
| **Adımları** | XSLT destekleyen kullanarak stil sayfaları komut dosyası `<msxml:script>` öğesi. Bu, bir XSLT dönüşümü kullanılacak özel işlevler sağlar. Komut dosyası dönüştürme gerçekleştirme işlem bağlamı altında yürütülür. XSLT komut dosyası, güvenilmeyen kodun yürütülmesini engellemek için bir ortamda güvenilmeyen zaman devre dışı bırakılması gerekir. *.NET kullanıyorsanız:* varsayılan olarak XSLT komut dosyası devre dışı; ancak, bunu açıkça aracılığıyla etkinleştirilmemiş olduğundan emin olmalısınız `XsltSettings.EnableScript` özelliği.|

### <a name="example"></a>Örnek 

```csharp
XsltSettings settings = new XsltSettings();
settings.EnableScript = true; // WRONG: THIS SHOULD BE SET TO false
```

### <a name="example"></a>Örnek
XSLT komut dosyası, MSXML 6.0 kullanarak kullanıyorsanız, varsayılan olarak devre dışıdır; Ancak, bunu açıkça AllowXsltScript XML DOM nesnesi özelliği üzerinden etkinleştirilmemiş olduğundan emin olmalısınız. 

```csharp
doc.setProperty("AllowXsltScript", true); // WRONG: THIS SHOULD BE SET TO false
```

### <a name="example"></a>Örnek
MSXML 5 kullanıyorsanız veya aşağıda XSLT komut dosyası varsayılan olarak etkindir ve açıkça gerekir devre dışı bırakın. XML DOM nesnesi özelliğini AllowXsltScript false olarak ayarlayın. 

```csharp
doc.setProperty("AllowXsltScript", false); // CORRECT. Setting to false disables XSLT scripting.
```

## <a id="out-sniffing"></a>Kullanıcı denetlenebilir içeriği içerebilir her sayfanın MIME otomatik algılaması dışında çevrilir emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [IE8 Güvenlik bölümü V - kapsamlı koruma](http://blogs.msdn.com/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx)  |
| **Adımları** | <p>Kullanıcı denetlenebilir içeriği içerebilir her bir sayfa için HTTP üstbilgisi kullanmalısınız `X-Content-Type-Options:nosniff`. Bu gereksinim ile uyum sağlamak için gereken üstbilgi kullanıcı denetlenebilir içerik içerebilecek sayfalar için sayfa tarafından ayarlayabilirsiniz veya genel uygulamadaki tüm sayfalar için ayarlayabilirsiniz.</p><p>Her bir web sunucusundan teslim dosya ilişkili bir türü [MIME türü](http://en.wikipedia.org/wiki/Mime_type) (olarak da adlandırılan bir *içerik türü*) (diğer bir deyişle, görüntü, metin, uygulama, vb.) içerik yapısını açıklar</p><p>Geliştiriciler kendi içerik MIME sniffed olmamalıdır belirtmek izin veren bir HTTP üstbilgisi içerik türü seçenekleri X başlığıdır. Bu üst MIME algılaması saldırıları azaltmak için tasarlanmıştır. Internet Explorer 8 (IE8) bu başlığı için destek eklendi</p><p>Yalnızca Internet Explorer 8 (IE8) kullanıcılarının X-içerik-tür-seçeneklerden yararlanabilir. Internet Explorer'ın önceki sürümlerini şu anda X-içerik-tür-Options üstbilgisi dikkate almaz</p><p>Bir MIME algılaması çevirin özelliği uygulamak için yalnızca ana tarayıcılar Internet Explorer 8 (ve üstü) var. Bu tarayıcılar da söz diziminin dahil etmek için diğer önemli tarayıcılar (Firefox, Safari, Chrome) benzer özellikleri varsa ve uyguladığınızda, bu öneri güncelleştirilir</p>|

### <a name="example"></a>Örnek
Uygulamadaki tüm sayfalar için genel gerekli üstbilgisi etkinleştirmek için aşağıdakilerden birini yapabilirsiniz: 

* Uygulama Internet Information Services (IIS) 7 tarafından barındırılıyorsa web.config dosyasında üstbilgisi Ekle 

```
<system.webServer> 
  <httpProtocol> 
    <customHeaders> 
      <add name=""X-Content-Type-Options"" value=""nosniff""/>
    </customHeaders>
  </httpProtocol>
</system.webServer> 
```

* Genel Uygulama aracılığıyla üstbilgisi eklemek\_BeginRequest 

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

* Yalnızca belirli sayfaları için gerekli üstbilgisi tek tek yanıtlarını ekleyerek etkinleştirebilirsiniz: 

```
this.Response.Headers[""X-Content-Type-Options""] = ""nosniff""; 
``` 

## <a id="xml-resolution"></a>Sağlamlaştırmak veya XML varlık çözümleme devre dışı bırakma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [XML varlık genişletme](http://capec.mitre.org/data/definitions/197.html), [XML hizmet reddi saldırılarına ve savunma](http://msdn.microsoft.com/magazine/ee335713.aspx), [MSXML güvenliğine genel bakış](http://msdn.microsoft.com/library/ms754611(v=VS.85).aspx), [MSXML kod güvenliğini sağlamak için en iyi uygulamalar](http://msdn.microsoft.com/library/ms759188(VS.85).aspx), [NSXMLParserDelegate Protokolü başvurusu](http://developer.apple.com/library/ios/#documentation/cocoa/reference/NSXMLParserDelegate_Protocol/Reference/Reference.html), [dış başvuruları çözme](https://msdn.microsoft.com/library/5fcwybb2.aspx) |
| **Adımları**| <p>Yaygın olarak kullanılmaz rağmen belge içinde veya dış kaynaklardan tanımlanan değerlerle makrosu varlıklar genişletmek XML Ayrıştırıcı sağlayan XML özelliği yoktur. Örneğin, belgeyi bir varlık "Şirket" adı "Microsoft" değeriyle şekilde tanımlayabilir, her zaman metin "&companyname;" belgede bunu otomatik olarak değiştirilir Microsoft metinle görüntülenir. Veya, belgeyi bir varlık "Microsoft hisse senedi geçerli değeri getirmek için bir dış web hizmeti başvuran MSFTStock" tanımlayabilirsiniz.</p><p>Sonra her zaman "&MSFTStock;", otomatik olarak değiştirilir geçerli stok fiyatıyla belgede görüntülenir. Ancak, bu işlev reddi (DoS) hizmet koşulları oluşturmak için kötüye. Bir saldırgan sistem üzerindeki tüm kullanılabilir bellek tüketir üstel genişletme XML Bomba oluşturmak için birden çok varlık yerleştirebilirsiniz. </p><p>Alternatif olarak, kendisinin geri akışları bir dış başvuru sonsuz bir veri miktarı oluşturabilir veya, yalnızca iş parçacığı askıda kalır. Sonuç olarak, tamamen kendi uygulama değil, kullanıyorsanız veya el ile bellek ve bu işlevselliği kesinlikle gerekli değilse, uygulama için varlık çözümlemesi tüketebileceği süre miktarını sınırlamak tüm takımlar iç ve/veya dış XML varlık çözümleme devre dışı bırakmalısınız. Varlık çözümleme, uygulamanız tarafından gerekli değildir, sonra da devre dışı bırakın. </p>|

### <a name="example"></a>Örnek
.NET Framework kodunu aşağıdaki yaklaşımlardan kullanabilirsiniz:

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
Unutmayın varsayılan değerini `ProhibitDtd` içinde `XmlReaderSettings` ancak doğrudur `XmlTextReader` false olur. In kapsama kullanıyorsanız, ProhibitDtd açıkça true olarak ayarlanmış gerekmez, ancak güvenliği artırmak amacıyla için bunu yapmanız önerilir. Ayrıca XmlDocument sınıfı varsayılan varlık çözümleme izin verildiğini unutmayın. 

### <a name="example"></a>Örnek
Varlık çözümleme XML belgelerine uymasıdır için devre dışı bırakmak için `XmlDocument.Load(XmlReader)` aşırı yükleme yöntemi ve uygun özellikleri XmlReader bağımsız değişkeni çözümleme, devre dışı bırakmak için aşağıdaki kodda gösterildiği şekilde ayarlayın: 

```csharp
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = true;
XmlReader reader = XmlReader.Create(stream, settings);
XmlDocument doc = new XmlDocument();
doc.Load(reader);
```

### <a name="example"></a>Örnek
Varlık çözümlemeyi devre dışı bırakma, uygulamanız için mümkün değilse, uygulamanızın gereksinimlerine göre uygun bir değere XmlReaderSettings.MaxCharactersFromEntities özelliğini ayarlayın. Bu olası üstel genişletme DoS saldırıları etkisini sınırlar. Aşağıdaki kod, bu yaklaşımın bir örnek sağlar: 

```csharp
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = false;
settings.MaxCharactersFromEntities = 1000;
XmlReader reader = XmlReader.Create(stream, settings);
```

### <a name="example"></a>Örnek
Satır içi varlıkları çözmek ancak bunu gerekiyorsa dış varlıklar, çözümlemeye gerekmez XmlReaderSettings.XmlResolver özelliği null olarak ayarlayın. Örneğin: 

```csharp
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = false;
settings.MaxCharactersFromEntities = 1000;
settings.XmlResolver = null;
XmlReader reader = XmlReader.Create(stream, settings);
```
Msxml6 içinde ProhibitDTD true (devre dışı bırakma DTD işleme) varsayılan olarak ayarlanır. Apple OSX/iOS kodu için kullanabileceğiniz iki XML ayrıştırıcıları vardır: NSXMLParser ve libXML2. 

## <a id="app-verification"></a>HTTP.sys kullanan uygulamalar URL Standartlaştırma doğrulama gerçekleştirme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Http.sys kullanan herhangi bir uygulama bu yönergeleri izlemelidir:</p><ul><li>URL uzunluğu en fazla 16.384 karakter (ASCII veya Unicode) sınırlayın. Varsayılan Internet Information Services (IIS) 6 ayarına göre mutlak URL uzunluğu üst sınırı budur. Web siteleri bu değerden daha kısa bir süre için mümkünse çaba</li><li>Bunlar kurallı kullanım kuralları .NET FX yararlanır gibi standart .NET Framework dosyası g/ç sınıfları (örneğin, FILESTREAM) kullanın</li><li>Açıkça bilinen dosya adları, bir izin verilenler listesi oluşturma</li><li>Açıkça bilinen dosya türleri, değil hizmet UrlScan reddeder reddetme: exe bat, cmd, com, htw, IDA, IDQ, htr, IDC, shtm [l], stm, yazıcı, INI, pol, verilerinin dosyaları</li><li>Aşağıdaki özel durumları yakalamak:<ul><li>System.ArgumentException (için aygıt adları)</li><li>System.NotSupportedException (için veri akışları)</li><li>System.IO.FileNotFoundException (için geçersiz kaçış karakterli dosya adları)</li><li>System.IO.directorynotfoundexception (için geçersiz kaçış karakterli dizin)</li></ul></li><li>*Sağlamadığı* Win32 dosya g/ç API'leri duyurmak. Geçersiz bir URL, düzgün biçimde 400 hatası kullanıcıya geri götürmek ve gerçek hata oturum.</li></ul>|

## <a id="controls-users"></a>Uygun denetimleri dosyaların kullanıcılardan kabul ederken karşılandığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Kısıtlanmamış karşıya dosya yükleme](https://www.owasp.org/index.php/Unrestricted_File_Upload), [dosya imza tablosu](http://www.garykessler.net/library/file_sigs.html) |
| **Adımları** | <p>Karşıya yüklenen dosyaların uygulamaları için önemli bir riski temsil eder.</p><p>İlk adımda saldırıların çoğu saldırıya için sisteme biraz kod almaktır. Ardından saldırı yürütülen kod almanın bir yolu bulmak yeterlidir. Bir dosyayı karşıya yükleme kullanarak ilk adımı gerçekleştirmek saldırgan yardımcı olur. Sınırsız dosya karşıya yükleme sonuçlarını tam sistem devralma, aşırı yüklenmiş dosya sistemi ya da veritabanı, arka uç sistemleri ve basit tahrifatı saldırılarına iletme de dahil olmak üzere değişebilir.</p><p>Yüklenen dosya ile uygulama yaptığı ve özellikle depolandığı bağlıdır. Sunucu tarafı dosya yüklemeleriyle doğrulanması eksik. Güvenlik denetimleri aşağıdaki dosyayı karşıya yüklemeyi işlevselliği için uygulanması gerekir:</p><ul><li>Dosya uzantısı onay (izin verilen dosya türü için geçerli bir kümesi kabul)</li><li>En büyük dosya boyutu sınırı</li><li>Dosya için webroot yüklenmelidir değil; konum, sistem dışı sürücü üzerinde bir dizin olmalıdır</li><li>Adlandırma kuralı izlenmesi gereken, karşıya yüklenen dosya adına sahip dosyanın önlemek amacıyla bazı rastgele, üzerine yazar</li><li>Dosyaları diske yazmadan önce virüsten koruma için taranmalıdır</li><li>Dosya adı ve diğer meta verileri (örneğin, dosya yolu) için kötü amaçlı karakter doğrulandığından emin olmak</li><li>Dosya biçimi imza işaretlenmelidir, bir kullanıcının masqueraded bir dosyayı karşıya yüklemeyi önlemek için (örneğin, bir exe dosyası için txt uzantısı değiştirerek karşıya yükleme)</li></ul>| 

### <a name="example"></a>Örnek
Dosya biçimi imza doğrulaması ilgili son noktası için Ayrıntılar sınıfına bakın: 

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

## <a id="typesafe"></a>Tür kullanımı uyumlu parametreleri veri erişimi için Web uygulamasında kullanıldığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Kullanırsanız parametreler koleksiyonu, SQL davranır yerine yürütülebilir kod olarak değişmez değer olarak bir giriş olabilir. Parametreler koleksiyonu giriş veri türü ve uzunluğu kısıtlamalar uygulamak için kullanılabilir. Değer aralığının dışında bir özel durum tetikler. Tür kullanımı uyumlu SQL parametre kullanılmazsa, saldırganlar filtrelenmemiş girişinde katıştırılmış ekleme saldırıları yürütmek olabilir.</p><p>Tür güvenli parametreleri SQL sorguları oluşturmak filtrelenmemiş girişle oluşabilecek olası SQL ekleme saldırıları önlemek için kullanın. Saklı yordamlar ve dinamik SQL deyimleri ile tür güvenli parametreleri kullanabilirsiniz. Parametreleri yürütülebilir kod değil de, veritabanı tarafından değişmez değerler olarak kabul edilir. Parametreler, türü ve uzunluğu için de denetlenir.</p>|

### <a name="example"></a>Örnek 
Aşağıdaki kod tür güvenli parametreleri ile SqlParameterCollection bir saklı yordamı çağrılırken kullanmayı gösterir. 

```csharp
using System.Data;
using System.Data.SqlClient;

using (SqlConnection connection = new SqlConnection(connectionString))
{ 
DataSet userDataset = new DataSet(); 
SqlDataAdapter myCommand = new SqlDataAdapter(LoginStoredProcedure", connection); 
myCommand.SelectCommand.CommandType = CommandType.StoredProcedure; 
myCommand.SelectCommand.Parameters.Add("@au_id", SqlDbType.VarChar, 11); 
myCommand.SelectCommand.Parameters["@au_id"].Value = SSN.Text; 
myCommand.Fill(userDataset);
}  
```
Önceki kod örneğinde giriş değeri 11 karakterden uzun olamaz. Veri türü veya parametresi tarafından tanımlanan uzunluk uyuşmadığı SqlParameter sınıfı bir özel durum oluşturur. 

## <a id="binding-mvc"></a>Ayrı model bağlama sınıflarını kullanın veya MVC yığın atama güvenlik açığı önlemek için bağlama filtresi listeler

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC5, MVC6 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Meta veri öznitelikleri](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.metadatatypeattribute), [ortak anahtarı Güvenlik Açığı ve azaltma](https://github.com/blog/1068-public-key-security-vulnerability-and-mitigation), [tam kılavuzu için ASP.NET MVC yığın atamasını](http://odetocode.com/Blogs/scott/archive/2012/03/11/complete-guide-to-mass-assignment-in-asp-net-mvc.aspx), [MVC kullanarak EF ile çalışmaya başlama](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost) |
| **Adımları** | <ul><li>**Güvenlik açıkları aşırı gönderim için ne zaman görmeliyim? -** Güvenlik açıkları aşırı gönderim giriş kullanıcıdan modeli sınıfları bağlamak herhangi bir yeri oluşabilir. MVC gibi çerçeveleri düz eski CLR nesnelerini (POCOs) dahil olmak üzere özel .NET sınıfları kullanıcı verilerini temsil edebilir. MVC kullanıcı girişi ilgilenmek için kullanışlı bir temsilini sağlayarak bu modeli sınıfları isteği verilerle otomatik olarak doldurur. Bu sınıfların kullanıcı tarafından ayarlanmamalıdır özellikleri eklediğinizde, uygulamanın kullanıcı denetimi uygulama hiçbir zaman amaçlı veri sağlayan, aşırı gönderim saldırılarına için saldırılara açık olabilir. MVC model bağlama gibi veritabanına erişmek Entity Framework gibi nesne/ilişkisel mappers gibi teknolojileri genellikle ayrıca veritabanı verileri temsil etmek üzere POCO nesnelerini kullanma desteği. Bu veri modeli sınıflar kullanıcı girişi ile ilgili MVC yaptığı gibi veritabanı verileriyle ilgili olarak aynı kolaylık sağlar. MVC ve veritabanı POCO nesneleri gibi benzer modelleri desteklemediğinden kolayca hem amacıyla aynı sınıfları yeniden gibi görünüyor. Sorunları ayrılması korumak bu yöntem başarısız olur ve istenmeyen özellikleri atlayarak nakil saldırılarını etkinleştirme model bağlama için Burada sunulan tek bir ortak alan olduğunu.</li><li>**Neden ı my filtrelenmemiş veritabanı modeli sınıflarını parametre olarak my MVC Eylemler kullanmamanız gerekir? -** Çünkü MVC model bağlama bağlamak herhangi bir şey bu sınıfta. Verilerin görünümünüzde bile görünmüyorsa, kötü niyetli bir kullanıcının dahil bu verilerle bir HTTP isteği gönderebilir ve eyleminizi veritabanı sınıfı için kullanıcı girişi kabul etmelidir veri şekli olduğunu söylüyor çünkü MVC gladly onu bağlarsınız.</li><li>**Model bağlama için kullanılan şekli hakkında neden önemli? -** Kullanarak ASP.NET MVC model bağlama aşırı geniş kapsamlı modellerle aşırı gönderim saldırılarına uygulamayı gösterir. Atlayarak nakil hedeflenen, bir öğe için bir fiyat veya bir hesap için güvenlik ayrıcalıklarını geçersiz kılma gibi geliştirici ötesinde uygulama verileri değiştirmek saldırganlar sağlayabilir. Uygulamalar, hangi güvenilmeyen girişi model bağlama aracılığıyla izin vermek için eyleme özgü modelleri (veya belirli izin verilen özellik filtresi listeleri) açık bir sağlamak için bağlama sözleşme kullanmalıdır.</li><li>**Kod yalnızca çoğaltma ayrı bağlama modelleri sorun mu yaşıyorsunuz? -** Hayır, sağlasa da, sorunları ayrılması yöneliktir. Eylem yöntemleri veritabanı modellerinde yeniden kullanırsanız, sınıf bir HTTP isteğinin kullanıcı tarafından ayarlanmış herhangi bir özelliği (veya alt özellik) yorumlarını. MVC yapmak istediğinizi değil ise, bir filtre listesi veya hangi verileri, bunun yerine Giriş kullanıcıdan gelebilir MVC göstermek için ayrı sınıf şeklin gerekir.</li><li>**Kullanıcı girişi için ayrı bağlama modelleri varsa, my veri ek açıklaması öznitelikleri yinelenen var mı? -** Gerekmez. Model bağlama sınıfı meta bağlamak için veritabanı model sınıfını MetadataTypeAttribute kullanabilirsiniz. Yalnızca MetadataTypeAttribute tarafından başvurulan türü (daha az özellik ancak fazla sahip olabilir) başvuru türünde bir alt olması gerektiğini unutmayın.</li><li>**Verileri geri ve İleri kullanıcı giriş modelleri ve veritabanı modelleri arasında taşıma yorucu olabilir. Yansıma kullanarak tüm özellikleri kopyalayabilir yalnızca miyim? -** Evet. Bağlama modellerinde görünen yalnızca kullanıcı girişi için güvenli olarak belirlediğiniz olanları özelliklerdir. Bu iki modelleri arasında ortak mevcut tüm özellikleri üzerinden kopyalamak için yansıma kullanarak engelleyen güvenlik neden yoktur.</li><li>**Ne hakkında [bağlayın (dışarıda = "â €¦")]. Ayrı bağlama modelleri sahip olmak yerine kullanabilir miyim? -** Bu yaklaşım önerilmez. Kullanma [bağlayın (dışarıda = "â €¦")] herhangi bir yeni özellik, varsayılan olarak bağlanabilir anlamına gelir. Yeni bir özellik eklendiğinde şeyler güvenli tutmak için fazladan bir adım yoktur yerine varsayılan olarak güvenli olacak tasarım sahip. Geliştirici bağlı olarak bir özellik eklenir her zaman bu liste denetimi risklidir.</li><li>**Olduğundan [bağlayın (içerme = "â €¦")] düzenleme işlemleri için yararlı? -** No [Bağlayın (içerme = "â €¦")] yalnızca (yeni veriler toplayarak) stili ekleme işlemleri için uygundur. (Mevcut verileri düzeltilmesi) stilini güncelleştir işlemleri için ayrı bağlama modelleri olması veya açık bir izin verilen özellikleri listesi UpdateModel veya TryUpdateModel geçirme gibi başka bir yaklaşım kullanın. Ekleme bir [bağlayın (içerme = "â €¦")] düzenleme işlemi öznitelikte anlamına gelir MVC bir nesne örneğini oluşturmak ve diğerlerini varsayılan değerlerinde bırakın yalnızca listelenen özellikleri ayarlayın. Veri kalıcı olduğunda tamamen Atlanan tüm özelliklerinin değerlerini kendi varsayılanlarına sıfırlamak olan bir varlığı değiştirir. IsAdmin atlanırsa, örneğin, bir [bağlamak (içerme = "â €¦")] adı bu eylem düzenlenmiş herhangi bir kullanıcı bir Düzenle işlem özniteliği için IsAdmin sıfırlama = false (herhangi bir düzenlenen kullanıcı yönetici durumu kaybeder). Belirli özellikler için güncelleştirmeleri engellemek istiyorsanız, diğer yaklaşımlardan birini kullanın. MVC araç bazı sürümleri denetleyicisi sınıflarıyla oluşturmak Not [bağlamak (içerme = "â €¦")] düzenleme eylemleri ve bu listeden bir özellik kaldırma atlayarak nakil saldırılarını önlemeye kapsıyor. Ancak, yukarıda açıklandığı gibi bu yaklaşımı beklendiği gibi çalışmaz ve bunun yerine belirtilmemişse özelliklerinde herhangi bir veri varsayılan değerlerine sıfırlar.</li><li>**Oluşturma işlemleri için kullanarak uyarılar vardır [bağlayın (içerme = "â €¦")] ayrı bağlama modelleri yerine? -** Evet. Önce tüm atlayarak nakil güvenlik açıkları Azaltıcı için iki ayrı yaklaşım koruma gerektiren düzenleme senaryolar için bu yaklaşım çalışmaz. İkinci ve ayrı bağlama modelleri zorunlu kullanıcı girişi için kullanılan şekil kalıcılığı için kullanılan şekli arasındaki sorunları ayrılması bir şey [bağlayın (içerme = "â €¦")] yapın. Üçüncü unutmayın [bağlayın (içerme = "â €¦")] yalnızca üst düzey özellikleri; işleyebilir yalnızca alt özellikleri (örneğin, "Details.Name") bölümünü özniteliğinde izin veremez. Son olarak ve belki de en önemlisi, kullanarak [bağlayın (içerme = "â €¦")] sınıfı kurduğunda hatırlanan gerekir fazladan bir adım model bağlama için kullanılan ekler. Yeni bir eylem yöntemi doğrudan veri sınıfına bağlar ve dahil etmek unutması durumunda bir [bağlamak (içerme = "â €¦")] özniteliği olabilir, aşırı gönderim saldırılarına için güvenlik açığı böylece [bağlamak (içerme = "â €¦")] yaklaşımdır varsayılan biraz daha az güvenli. Kullanırsanız [bağlayın (içerme = "â €¦")], her zaman veri sınıfları eylem yöntemi parametrelerine görünen her zaman belirtmek için dikkatli olun.</li><li>**Oluşturma işlemleri için ne koyma [bağlayın (içerme = "â €¦")] modeli sınıfı özniteliği? Bu yaklaşım öznitelik her eylem yöntemi koyma hatırlama gereksinimini önlenir? -** Bazı durumlarda bu yaklaşım çalışır. Kullanma [bağlamak (INCLUDE = "â €¦")] model türündeki (yerine bu sınıfı kullanarak eylem parametrelerini), dahil etmeyi unutmayın gereksinimini ortadan kaldırmak [bağlamak (INCLUDE = "â €¦")] her eylem yöntemi özniteliği. Öznitelik doğrudan sınıf üzerinde etkili bir şekilde kullanarak model bağlama amacıyla bu sınıfın ayrı bir yüzey alanını oluşturur. Ancak, bu yaklaşım, model bağlama bir şeklin model sınıfı başına yalnızca sağlar. Model bağlama bir alanın (kullanıcı rollerini güncelleştiren Örneğin, yalnızca yönetici eylem) izin vermek için bir eylem yönteminin gerekir ve bu alan model bağlama önlemek başka eylemler ihtiyacınız varsa bu yaklaşımı çalışmaz. Her sınıf yalnızca bir model bağlama şeklin olabilir; farklı eylemler farklı model bağlama şekiller gerekiyorsa, her iki ayrı model bağlama sınıflarını kullanarak bu ayrı şekilleri temsil veya ayırmak ihtiyaç duydukları [bağlayın (içerme = "â €¦")] eylem yöntemlerini öznitelikleri.</li><li>**Ne modelleri bağlama? Görünüm model ile aynı şey oldukları? -** İki ilgili kavramları şunlardır. Bir model bağlama modelini başvuran bir eylemi kullanılan sınıf parametre listesi (MVC model bağlamanın dışında eylem yöntemine geçirilen Şekil) terimdir. Terim görünüm modeli eylem yönteminden bir görünüme iletilen bir model sınıfı gösterir. Bir görünüm özgü modeli kullanarak bir eylem yönteminden bir görünüme veri geçirme için ortak bir yaklaşımdır. Genellikle, bu şeklin model bağlama için uygundur ve terim görünüm modeli kullanılan her iki yerde de aynı modelin başvurmak için kullanılabilir. Kesin olarak için bu yordamı ne yığın atama amaçlar için önemli olan eyleme geçirilen şekli odaklanan modeller, özellikle bağlama hakkında alınmaktadır.</li></ul>| 

## <a id="rendering"></a>Güvenilmeyen web çıkış işleme önce kodlama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, Web Forms, MVC5, MVC6 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [ASP.NET siteler arası komut önlemek nasıl](http://msdn.microsoft.com/library/ms998274.aspx), [siteler arası komut dosyası](http://cwe.mitre.org/data/definitions/79.html), [XSS (siteler arası komut dosyası) önleme kopya sayfası](https://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet) |
| **Adımları** | Siteler arası komut dosyası (XSS yaygın olarak kısaltılır) Çevrimiçi hizmetler veya tüm uygulama / web girişten tüketen bileşen için saldırı vektörü olur. XSS güvenlik açıkları, bir saldırganın bir güvenlik açığı web uygulaması aracılığıyla başka bir kullanıcının makinesinde betik yürütmek. Kötü amaçlı komut dosyalarını, tanımlama bilgilerini çalabilir ve aksi halde bir kurbanın makine JavaScript aracılığıyla değiştirmesine için kullanılabilir. Kullanıcı girişini doğrulama, doğru biçimlendirildiğinden olduktan ve bir web sayfasında işlenmeden önce kodlama XSS engelledi. Giriş doğrulama ve çıktı kodlama Web koruma kitaplığı kullanılarak yapılabilir. Yönetilen kod için (C\#, VB.net, vb.), bir veya daha uygun kodlama yöntemlerini nerede kullanıcı girişini bildirilmiş bağlam bağlı olarak Web koruma (Anti-XSS) kitaplığı:| 

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

## <a id="typemodel"></a>Giriş doğrulaması ve tüm dize türünde Model özelliklerini filtrelemesine

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, MVC5, MVC6 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Doğrulama ekleme](http://www.asp.net/mvc/overview/getting-started/introduction/adding-validation), [MVC uygulamasındaki Model verileri doğrulama](http://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), [ilkeleri için ASP.NET MVC uygulamalarınızı yönlendirmede](http://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Adımları** | <p>Uygulama kötü amaçlı kullanıcı girişleri karşı korunması emin olmak için uygulamada kullanılmadan önce giriş parametrelerini doğrulanması gerekir. Sunucu tarafında bir beyaz liste doğrulama stratejisi ile normal ifade doğrulamaları kullanarak giriş değerleri doğrulayın. Kullanıcı girişleri unsanitized / yöntemlere iletilen parametreler yerleştirme güvenlik açıkları kodu neden olabilir.</p><p>Web uygulamaları için giriş noktaları form alanları, QueryStrings, tanımlama bilgileri, HTTP üst bilgilerine ve web hizmeti parametreleri de dahil edebilirsiniz.</p><p>Aşağıdaki giriş doğrulama denetimleri, model bağlama sırasında gerçekleştirilen gerekir:</p><ul><li>Yanıtta normal ifade ek açıklama, izin verilen karakterler ve izin verilen maksimum uzunluğu kabul etmek için model özellikleri açıklama</li><li>Denetleyici yöntemlerine ModelState geçerlilik gerçekleştirmeniz gerekir</li></ul>|

## <a id="richtext"></a>Tüm karakterleri, örneğin, Zengin Metin Düzenleyicisi'ni kabul eden form alanlarını temizleme işlemi uygulanmalıdır

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Güvenli olmayan giriş kodlamak](https://msdn.microsoft.com/library/ff647397.aspx#paght000003_step3), [HTML temizleyici](https://github.com/mganss/HtmlSanitizer) |
| **Adımları** | <p>Kullanmak istediğiniz tüm static işaretleme etiketlerini tanımlayın. Ortak bir uygulama gibi güvenli HTML öğelerine biçimlendirme kısıtlamaktır `<b>` (kalın) ve `<i>` (italik).</p><p>HTML kodlama verileri yazmadan önce onu. Bu kötü amaçlı bir komut dosyası yürütülebilir kod olarak değil metin olarak işlenecek neden tarafından güvenli hale getirir.</p><ol><li>ValidateRequest ekleyerek ASP.NET istek doğrulamayı devre dışı bırak = "false" özniteliği için @ Page yönergesi</li><li>Dize girişi HtmlEncode yöntemi ile kodlayın</li><li>Bir StringBuilder kullanın ve seçmeli olarak izin vermek istediğiniz HTML öğeleri kodlama kaldırmak için değiştirme yöntemini çağırın</li></ol><p>Sayfa bileşenini ayarlayarak başvuruları devre dışı bırakır ASP.NET istek doğrulama `ValidateRequest="false"`. Onu HTML olarak kodlar giriş ve seçmeli olarak tanır `<b>` ve `<i>` alternatif olarak, bir .NET kitaplığı HTML Temizleme için de kullanılabilir.</p><p>HtmlSanitizer HTML parçalarının ve XSS saldırılarını açabilir yapıları belgelerden Temizleme için bir .NET kitaplıktır. Ayrıştırma, işlemek ve HTML ve CSS işlemek için AngleSharp kullanır. Bir NuGet paketi olarak HtmlSanitizer yüklenebilir ve kullanıcı girişini ilgili HTML veya CSS temizleme yöntemi, sunucu tarafında geçerli aracılığıyla geçirilebilir. Lütfen unutmayın, güvenlik denetimi olarak temizleme yalnızca son seçeneği olarak düşünülmelidir.</p><p>Giriş doğrulama ve çıktı kodlama daha iyi güvenlik denetimleri olarak kabul edilir.</p> |

## <a id="inbuilt-encode"></a>DOM öğeleri yerleşik kodlama olmayan havuzlarını atamayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Varsayılan kodlama birçok javascript işlevleri desteklemez. Bu tür işlevler aracılığıyla DOM öğelerine güvenilmeyen giriş atarken site komut dosyası (XSS) yürütmeleri sonuçlanabilir.| 

### <a name="example"></a>Örnek
Güvensiz örnekleri şunlardır: 

```
document.getElementByID("div1").innerHtml = value;
$("#userName").html(res.Name);
return $('<div/>').html(value)
$('body').append(resHTML);   
```
Kullanmayan `innerHtml`; bunun yerine kullanın `innerText`. Benzer şekilde, yerine, `$("#elm").html()`, kullanın`$("#elm").text()` 

## <a id="redirect-safe"></a>Tüm doğrulama uygulama içinde yeniden yönlendirmeleri kapalı veya güvenli bir şekilde tamamlandı

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [OAuth 2.0 yetkilendirme Framework - açık yeniden yönlendiricileri](http://tools.ietf.org/html/rfc6749#section-10.15) |
| **Adımları** | <p>Kullanıcının sağladığı bir konuma yeniden yönlendirme gerektiren uygulama tasarımı önceden tanımlanmış bir "safe" listesi siteler veya etki alanları için olası yeniden yönlendirme hedefleri sınırlamak gerekir. Uygulamadaki tüm yeniden yönlendirmeleri kapalı güvenli olması gerekir.</p><p>Bunu yapmak için:</p><ul><li>Tüm yeniden yönlendirmeleri tanımlayın</li><li>Her yeniden yönlendirme için uygun bir hafifletme. Uygun Azaltıcı Etkenler yeniden yönlendirme beyaz liste veya kullanıcı onayı içerir. Bir web sitesi veya bir açık yeniden yönlendirme güvenlik açığı hizmetiyle Facebook/OAuth/Openıd kimlik sağlayıcıları kullanıyorsa, bir saldırganın bir kullanıcının oturum açma belirteci çalabilir ve o kullanıcının kimliğine bürün. Bu devralınan bir risk RFC 6749 "OAuth 2.0 yetkilendirme Framework" belgelenen OAuth kullanıldığında, 10.15 "açmak yönlendiren" bölümünde benzer şekilde, zıpkınla kimlik avı saldırıları tarafından açık yeniden yönlendirmeleri kullanarak kullanıcıların kimlik bilgilerini tehlikeye girebilir</li></ul>|

## <a id="string-method"></a>Uygulama giriş doğrulaması denetleyicisi yöntemler tarafından kabul edilen tüm dize türü parametreleri

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, MVC5, MVC6 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Model verileri MVC uygulamasındaki doğrulama](http://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), [ilkeleri için ASP.NET MVC uygulamalarınızı yönlendirmede](http://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Adımları** | Yalnızca basit veri türü ve değil modelleri bağımsız değişken olarak kabul yöntemleri için normal ifade kullanarak giriş doğrulaması yapılmalıdır. Burada Regex.IsMatch geçerli regex desenle kullanılmalıdır. Giriş belirtilen normal ifadeyle eşleşmez, denetim daha fazla devam etmemelisiniz ve doğrulama hatası ile ilgili yeterli bir uyarı görüntülenmesi gerekir.| 

## <a id="dos-expression"></a>Normal ifade DoS nedeniyle hatalı normal ifadeler önlemek için işleme için üst sınır zaman aşımını ayarlama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, Web Forms, MVC5, MVC6  |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [DefaultRegexMatchTimeout Property ](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.defaultregexmatchtimeout.aspx) |
| **Adımları** | Hizmet reddi saldırılarına karşı hatalı emin olmak için çok sayıda geri dönüş neden, oluşturulan normal ifadeler, genel varsayılan zaman aşımı ayarlayın. İşleme tanımlanmış üst sınırından daha uzun sürüyorsa, zaman aşımı özel durumu throw. Hiçbir şey yapılandırılmışsa, zaman aşımını sonsuz olacaktır.| 

### <a name="example"></a>Örnek
Örneğin, aşağıdaki yapılandırma işleme 5 saniyeden daha uzun sürerse bir RegexMatchTimeoutException atar: 

```csharp
<httpRuntime targetFramework="4.5" defaultRegexMatchTimeout="00:00:05" />
```

## <a id="html-razor"></a>Razor görünümleri Html.Raw kullanmaktan kaçının

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC5, MVC6 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| Adım | ASP.Net Web sayfaları (Razor) gerçekleştirmek otomatik HTML kodlaması. Katıştırılmış kod nuggets (@ blokları) tarafından yazdırılan tüm otomatik olarak HTML ile kodlanmış dizelerdir. Ancak, ne zaman `HtmlHelper.Raw` yöntemi çağrıldığında, HTML kodlu olmayan biçimlendirme döndürür. Varsa `Html.Raw()` yardımcı yöntemi kullanıldığında, Razor sağladığı otomatik kodlama koruma atlar.|

### <a name="example"></a>Örnek
Güvenli olmayan bir örnek aşağıda verilmiştir: 

```csharp
<div class="form-group">
            @Html.Raw(Model.AccountConfirmText)
        </div>
        <div class="form-group">
            @Html.Raw(Model.PaymentConfirmText)
        </div>
</div>
```
Kullanmayın `Html.Raw()` biçimlendirme görüntülemek gerekli olmadıkça. Bu yöntem örtük olarak kodlama çıkış gerçekleştirmez. Örneğin diğer ASP.NET Yardımcıları kullanın,`@Html.DisplayFor()` 

## <a id="stored-proc"></a>Dinamik sorgular saklı yordamlarda kullanmayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Bir SQL ekleme saldırısı güvenlik açıklarını veritabanında rasgele komutları çalıştırmak için giriş doğrulamada kullanır. Uygulamanız veritabanına erişmek için dinamik SQL deyimlerini oluşturmak için giriş kullandığında oluşabilir. Kodunuzu ham kullanıcı girişi içeren dizeleri geçirilen saklı yordamlar kullanılıyorsa da oluşabilir. Saldırgan, SQL ekleme saldırısında kullanarak, veritabanında rasgele komutları çalıştırabilirsiniz. Tüm SQL deyimleri (SQL deyimlerini saklı yordamlarda dahil) parametreli gerekir. Parametreli SQL deyimlerini-SQL (tek tırnak gibi) özel bir anlamı sorunsuz kesin türü belirtilmiş olduğundan olan karakterleri kabul eder. |

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

## <a id="validation-api"></a>Model doğrulama Web API yöntemlerini yapıldığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC5, MVC6 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [ASP.NET Web API'de model doğrulama](http://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api) |
| **Adımları** | Bir istemci bir web API veri gönderdiğinde, herhangi bir işlem yapmadan önce verileri doğrulamak için zorunludur. ASP.NET Web modelleri girdi olarak kabul için API ' larını veri ek açıklamaları modellerinde doğrulama kuralları modelin özelliklerini ayarlamak için kullanın.|

### <a name="example"></a>Örnek
Aşağıdaki kod, aynı gösterir: 

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
API denetleyicilerinin eylem yöntemi modeli geçerliliğini aşağıda gösterildiği gibi açıkça denetlenmesini sahiptir: 

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

## <a id="string-api"></a>Web API yöntemleri tarafından kabul edilen tüm dize tür parametrelerindeki giriş doğrulaması uygulama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, MVC 5, MVC 6 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Model verileri MVC uygulamasındaki doğrulama](http://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), [ilkeleri için ASP.NET MVC uygulamalarınızı yönlendirmede](http://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Adımları** | Yalnızca basit veri türü ve değil modelleri bağımsız değişken olarak kabul yöntemleri için normal ifade kullanarak giriş doğrulaması yapılmalıdır. Burada Regex.IsMatch geçerli regex desenle kullanılmalıdır. Giriş belirtilen normal ifadeyle eşleşmez, denetim daha fazla devam etmemelisiniz ve doğrulama hatası ile ilgili yeterli bir uyarı görüntülenmesi gerekir.|

## <a id="typesafe-api"></a>Tür kullanımı uyumlu parametreleri Web API'si veri erişimi için kullanıldığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Kullanırsanız parametreler koleksiyonu, SQL davranır yerine yürütülebilir kod olarak değişmez değer olarak bir giriş olabilir. Parametreler koleksiyonu giriş veri türü ve uzunluğu kısıtlamalar uygulamak için kullanılabilir. Değer aralığının dışında bir özel durum tetikler. Tür kullanımı uyumlu SQL parametre kullanılmazsa, saldırganlar filtrelenmemiş girişinde katıştırılmış ekleme saldırıları yürütmek olabilir.</p><p>Tür güvenli parametreleri SQL sorguları oluşturmak filtrelenmemiş girişle oluşabilecek olası SQL ekleme saldırıları önlemek için kullanın. Saklı yordamlar ve dinamik SQL deyimleri ile tür güvenli parametreleri kullanabilirsiniz. Parametreleri yürütülebilir kod değil de, veritabanı tarafından değişmez değerler olarak kabul edilir. Parametreler, türü ve uzunluğu için de denetlenir.</p>|

### <a name="example"></a>Örnek
Aşağıdaki kod tür güvenli parametreleri ile SqlParameterCollection bir saklı yordamı çağrılırken kullanmayı gösterir. 

```csharp
using System.Data;
using System.Data.SqlClient;

using (SqlConnection connection = new SqlConnection(connectionString))
{ 
DataSet userDataset = new DataSet(); 
SqlDataAdapter myCommand = new SqlDataAdapter(LoginStoredProcedure", connection); 
myCommand.SelectCommand.CommandType = CommandType.StoredProcedure; 
myCommand.SelectCommand.Parameters.Add("@au_id", SqlDbType.VarChar, 11); 
myCommand.SelectCommand.Parameters["@au_id"].Value = SSN.Text; 
myCommand.Fill(userDataset);
}  
```
Önceki kod örneğinde giriş değeri 11 karakterden uzun olamaz. Veri türü veya parametresi tarafından tanımlanan uzunluk uyuşmadığı SqlParameter sınıfı bir özel durum oluşturur. 

## <a id="sql-docdb"></a>Cosmos DB için parametreli SQL sorgularını kullan

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure belge DB | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Azure Cosmos DB içinde SQL parametrelemeyi Duyurusu](https://azure.microsoft.com/blog/announcing-sql-parameterization-in-documentdb/) |
| **Adımları** | Azure Cosmos DB yalnızca salt okunur sorguları destekler; ancak, SQL ekleme sorguları kullanıcı girişi ile birleştirerek oluşturulur, yine de mümkündür. Bunlar aynı koleksiyonunda kötü amaçlı SQL sorguları hazırlayın tarafından erişiyor döndürmemelidir verilere erişmek bir kullanıcı için mümkün olabilir. Sorguları oluşturulan parametreli SQL sorguları kullanıcı girişini temel alarak kullanın. |

## <a id="schema-binding"></a>Şema bağlama aracılığıyla WCF girişi doğrulama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, NET Framework 3 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff647820.aspx) |
| **Adımları** | <p>Doğrulama eksikliği farklı türü ekleme saldırıları için yol gösterir.</p><p>İleti doğrulama bir savunma hattı oluşturan WCF uygulamanızın koruma temsil eder. Bu yaklaşımda, WCF Hizmeti işlemlerini kötü amaçlı bir istemci tarafından saldırılara karşı korumak için şemaları kullanarak iletileri doğrulayın. İstemci tarafından kötü amaçlı bir hizmete saldırılardan korumak için istemci tarafından alınan tüm iletileri doğrulayın. İleti doğrulama ileti işlemleri ileti sözleşmeleri veya yapılamaz veri sözleşmeleri kullandığında doğrulamak mümkün kılar parametre doğrulaması kullanarak. İleti doğrulama, böylece daha fazla esneklik sağlayan ve geliştirme süresini azaltarak şemalarını içinde doğrulama mantığını oluşturmanıza olanak sağlar. Şemalar veri temsili standartları oluşturma, kuruluş içindeki farklı uygulamalar arasında yeniden kullanılabilir. Ayrıca, ileti doğrulama özelliği iş mantığı temsil eden sözleşmeleri içeren daha karmaşık veri türleri kullandığında işlemleri korumanıza olanak sağlar.</p><p>İleti doğrulamayı gerçekleştirmek için önce işlemlerini hizmetiniz ve bu işlemler tarafından tüketilen veri türlerini temsil eden bir şema oluşturun. Özel istemci ileti denetçisi ve özel dağıtıcısı ileti denetçisi hizmet denetleyicisinden gönderilen/alınan iletileri doğrulamak için uygulayan bir .NET sınıf oluşturursunuz. Ardından, hem istemci hem de hizmet ileti doğrulamasını etkinleştirmek için bir özel uç noktası davranışı uygular. Son olarak, hizmet veya istemci yapılandırma dosyasında genişletilmiş özel uç noktası davranışı kullanıma sunmak izin veren sınıfındaki özel yapılandırma öğesi uygulamak"</p>|

## <a id="parameters"></a>Parametre denetçiler aracılığıyla WCF girişi doğrulama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, NET Framework 3 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff647875.aspx) |
| **Adımları** | <p>Giriş ve veri doğrulama derinlemesine koruma WCF uygulamanızın önemli bir satırı temsil eder. Hizmet saldırılardan korumak için WCF hizmeti işlemlerinde kötü amaçlı bir istemci tarafından kullanıma sunulan tüm parametreleri doğrulamalıdır. Buna karşılık, aynı zamanda istemci tarafından kötü amaçlı bir hizmete saldırılardan korumak için istemci tarafından alınan tüm dönüş değerleri doğrulamalıdır</p><p>WCF özel uzantılar oluşturarak WCF çalışma zamanı davranışını özelleştirmenizi farklı genişletilebilirlik noktaları sağlar. İleti denetçileri ve parametre denetçiler bir istemci ve hizmet arasında geçen verileri üzerinde daha fazla denetim kazanmak için kullanılan iki genişletilebilirlik mekanizmalardır. Giriş doğrulaması için parametre denetçiler kullanın ve yalnızca bir hizmet ve bu moddan akan tüm ileti incelemek gerektiğinde ileti denetçileri kullanmanız gerekir.</p><p>Giriş doğrulamayı gerçekleştirmek için bir .NET sınıfını oluşturmak ve parametreleri hizmetinizi işlemlerini doğrulamakta özel parametre denetçisi uygulamak. Ardından, hem istemci hem de hizmet doğrulamasını etkinleştirmek için bir özel uç noktası davranışı gerçekleştireceksiniz. Son olarak, hizmet veya istemci yapılandırma dosyasında genişletilmiş özel uç noktası davranışı kullanıma sunmak izin veren sınıfındaki özel yapılandırma öğesi gerçekleştireceksiniz</p>|
