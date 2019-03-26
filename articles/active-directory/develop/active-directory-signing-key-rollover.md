---
title: Azure AD'de imzalama anahtar geçişi
description: Bu makalede, Azure Active Directory için imzalama anahtar geçişi en iyi uygulamalar açıklanmaktadır
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2018
ms.author: celested
ms.reviewer: paulgarn, hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 82e9941a6c468a3b0ed9d1f22a2970cfa6584617
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58439354"
---
# <a name="signing-key-rollover-in-azure-active-directory"></a>İmzalama anahtarı geçiş işlemi, Azure Active Directory'de
Bu makalede, Azure Active Directory (Azure AD) güvenlik belirteçleri imzalamak için kullanılan ortak anahtarları hakkında bilmeniz gerekenler açıklanmaktadır. Bu anahtarları geçişi düzenli aralıklarla ve acil bir durum uzatılabilir, hemen dikkat edin önemlidir. Azure AD kullanan tüm uygulamalar, program aracılığıyla anahtarı geçiş işlemi ya da bir düzenli el ile geçiş işlemi'kurmak başlatabilmeniz gerekir. Anahtarları nasıl çalıştığını, anlamak için okumaya devam uygulamanıza geçişin etkisini değerlendirmek ve uygulamanızı güncelleştirmeniz veya gerekirse, anahtar geçişi işlemek için bir düzenli el ile geçiş işlemi oluşturmak.

## <a name="overview-of-signing-keys-in-azure-ad"></a>Azure AD'de imzalama anahtarı genel bakış
Azure AD, kendisi ve onu kullanan uygulamalar arasında güven oluşturmak için sektör standartlarında derlenmiş ortak anahtar şifrelemesi kullanır. Pratikte, bunu şu şekilde çalışır: Azure AD, bir ortak ve özel anahtar çiftinden oluşur bir imzalama anahtarı kullanır. Azure AD, bir kullanıcı kimlik doğrulaması için Azure AD kullanan bir uygulama için oturum açtığında, kullanıcı hakkında bilgileri içeren bir güvenlik belirteci oluşturur. Bu belirteç uygulamaya geri göndermeden önce özel anahtarı kullanarak Azure AD tarafından imzalanır. Belirtecin geçerli ve Azure ad kaynaklı olduğunu doğrulamak için uygulamayı kiracının içinde yer alan Azure AD tarafından kullanıma sunulan ortak anahtarı kullanarak belirtecinin imzası doğrulama [Openıd Connect bulma belge](https://openid.net/specs/openid-connect-discovery-1_0.html) veya SAML / WS-Federasyon [Federasyon meta veri belgesi](azure-ad-federation-metadata.md).

Güvenlik nedenleriyle, Azure AD'nin imzalama anahtar pay düzenli aralıklarla ve Acil, Uzatılabilir hemen. Azure AD ile tümleştirilen herhangi bir uygulama ne sıklıkta oluşabilir ne olursa olsun, bir anahtar geçişi olayı işlemek için hazırlıklı olmalıdır. Bu değil ve bir belirteç imzayı doğrulamak için süresi dolmuş bir anahtarı kullanmak uygulamanızı çalışır, oturum açma isteği başarısız olur.

Her zaman birden fazla geçerli anahtar Openıd Connect bulma belge ve Federasyon meta veri belgesi içinde mevcut değildir. Uygulamanızı herhangi bir belgede belirtilen anahtarları kullanmak hazırlıklı olmalıdır, bir anahtar yakında aylarına olduğundan, başka bir değişimi olması vb.

## <a name="how-to-assess-if-your-application-will-be-affected-and-what-to-do-about-it"></a>Uygulamanızı etkileniyorsa değerlendirmek nasıl ve ne gerekenler
Uygulamanızı anahtar geçişi nasıl işlediğini uygulama veya hangi kimlik protokolü ve kitaplık kullanıldı türü gibi değişkenleri bağlıdır. Aşağıdaki bölümlerde, en sık karşılaşılan uygulamalar tarafından anahtar geçişi etkilendiğini ve otomatik geçişi desteği veya anahtarı el ile güncelleştirmek için uygulama konusunda rehberlik olup olmadığını değerlendirin.

* [Yerel istemci uygulamaları, kaynaklara erişme](#nativeclient)
* [Web uygulamaları / API'leri kaynaklara erişme](#webclient)
* [Web uygulamaları / API'leri kaynakları koruma ve Azure uygulama hizmetleri kullanılarak oluşturulan](#appservices)
* [Web uygulamaları / API'ları kullanarak .NET OWIN Openıd Connect, WS-Federasyon veya WindowsAzureActiveDirectoryBearerAuthentication ara yazılım kaynakları koruma](#owin)
* [Web uygulamaları / API'ları kullanarak .NET Core Openıd Connect veya JwtBearerAuthentication ara yazılım kaynakları koruma](#owincore)
* [Web uygulamaları / Node.js passport azure ad modülünü kullanarak kaynakları koruma API'leri](#passport)
* [Web uygulamaları / API'leri kaynakları koruma ve Visual Studio 2015 veya Visual Studio 2017 ile oluşturulmuş](#vs2015)
* [Kaynakları koruma ve Visual Studio 2013 ile oluşturulan web uygulamaları](#vs2013)
* Kaynakları koruma ve Visual Studio 2013 ile oluşturulan web API'leri
* [Kaynakları koruma ve Visual Studio 2012 ile oluşturulan web uygulamaları](#vs2012)
* [Kaynakları koruma ve Windows Identity Foundation'ı kullanarak 2008 o Visual Studio 2010 ile oluşturulan web uygulamaları](#vs2010)
* [Web uygulamaları / API'leri kullanarak tüm diğer kitaplıkları veya desteklenen protokolden herhangi birini el ile uygulanması kaynakları koruma](#other)

Bu kılavuzu **değil** için geçerlidir:

* Azure AD uygulama (özel dahil) galerisinden eklenen uygulamalar, anahtarları imzalama bakımından ayrı Kılavuzlar vardır. [Daha fazla bilgi.](../manage-apps/manage-certificates-for-federated-single-sign-on.md)
* Uygulama Ara sunucusu üzerinden yayımlanan uygulamalarla imzalama anahtarı hakkında endişelenmeniz gerekmez şirket içi.

### <a name="nativeclient"></a>Yerel istemci uygulamaları, kaynaklara erişme
Kaynakları (yani yalnızca erişim uygulamaları Microsoft Graph, anahtar kasası, Outlook API ve diğer Microsoft APIs) genellikle yalnızca belirteç edinme ve boyunca kaynak sahibine geçirin. Tüm kaynakları korumadığınızdan düşünüldüğünde, belirteç incelemeyin ve bu nedenle doğru şekilde imzalanmış olmak gerekmez.

Yerel istemci uygulamaları, masaüstü veya mobil, bu kategoriye ve bu nedenle geçişi tarafından etkilenmez.

### <a name="webclient"></a>Web uygulamaları / API'leri kaynaklara erişme
Kaynakları (yani yalnızca erişim uygulamaları Microsoft Graph, anahtar kasası, Outlook API ve diğer Microsoft APIs) genellikle yalnızca belirteç edinme ve boyunca kaynak sahibine geçirin. Tüm kaynakları korumadığınızdan düşünüldüğünde, belirteç incelemeyin ve bu nedenle doğru şekilde imzalanmış olmak gerekmez.

Web uygulamaları ve web yalnızca uygulama akışını kullanarak API'leri (istemci kimlik bilgileri / istemci sertifikası), bu kategoriye girer ve bu nedenle geçişi tarafından etkilenmez.

### <a name="appservices"></a>Web uygulamaları / API'leri kaynakları koruma ve Azure uygulama hizmetleri kullanılarak oluşturulan
Azure uygulama hizmetleri kimlik doğrulama / yetkilendirme (EasyAuth) işlevselliği, anahtar geçişi otomatik olarak işlemek için gerekli mantığı zaten sahiptir.

### <a name="owin"></a>Web uygulamaları / API'ları kullanarak .NET OWIN Openıd Connect, WS-Federasyon veya WindowsAzureActiveDirectoryBearerAuthentication ara yazılım kaynakları koruma
Uygulamanızı .NET OWIN Openıd Connect, WS-Federasyon veya WindowsAzureActiveDirectoryBearerAuthentication ara yazılım, kullanıyorsa, anahtar geçişi otomatik olarak işlemek için gerekli mantığı zaten sahip.

Uygulamanızı aşağıdakilerden herhangi biri için aşağıdaki kod parçacıkları, uygulamanızın Startup.cs veya Startup.Auth.cs birini arayarak kullandığını doğrulayın

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseWsFederationAuthentication(
    new WsFederationAuthenticationOptions
    {
     // ...
     });
```
```
 app.UseWindowsAzureActiveDirectoryBearerAuthentication(
     new WindowsAzureActiveDirectoryBearerAuthenticationOptions
     {
     // ...
     });
```

### <a name="owincore"></a>Web uygulamaları / API'ları kullanarak .NET Core Openıd Connect veya JwtBearerAuthentication ara yazılım kaynakları koruma
Uygulamanızı .NET Core OWIN Openıd Connect veya JwtBearerAuthentication ara yazılımı kullanıyorsanız, anahtar geçişi otomatik olarak işlemek için gerekli mantığı zaten sahip.

Uygulamanızı aşağıdakilerden herhangi biri için aşağıdaki kod parçacıkları, uygulamanızın Startup.cs veya Startup.Auth.cs birini arayarak kullandığını doğrulayın

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseJwtBearerAuthentication(
    new JwtBearerAuthenticationOptions
    {
     // ...
     });
```

### <a name="passport"></a>Web uygulamaları / Node.js passport azure ad modülünü kullanarak kaynakları koruma API'leri
Uygulamanızı Node.js passport ad modülünü kullanıyorsanız, anahtar geçişi otomatik olarak işlemek için gerekli mantığı zaten sahip.

Olduğunu onaylayın, uygulama passport aşağıdaki kod parçacığında, uygulamanızın app.js aratarak ad

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <a name="vs2015"></a>Web uygulamaları / API'leri kaynakları koruma ve Visual Studio 2015 veya Visual Studio 2017 ile oluşturulmuş
Uygulamanız bir web uygulaması şablonu Visual Studio 2015 veya Visual Studio 2017 kullanılarak oluşturulan ve seçtiyseniz **iş ve Okul hesapları** gelen **kimlik doğrulamayı Değiştir** menüsünde, zaten sahip anahtar geçişi otomatik olarak işlemek için gerekli mantığı. OWIN Openıd Connect Ara yazılımında katıştırılmış bu mantık, Openıd Connect bulma belge anahtarlarını önbelleğe alır ve bunları düzenli aralıklarla yeniler.

Çözümünüz için el ile kimlik doğrulaması eklediyseniz gerekli anahtar geçişi mantığı uygulamanız olmayabilir. Kendiniz yazmak veya adımları gerekecektir [Web uygulamaları / herhangi diğer kitaplıkları'nı kullanarak veya desteklenen protokolden herhangi birini el ile uygulanması API'leri](#other).

### <a name="vs2013"></a>Kaynakları koruma ve Visual Studio 2013 ile oluşturulan web uygulamaları
Uygulamanızı Visual Studio 2013'te bir web uygulaması şablonu kullanılarak oluşturulan ve seçtiyseniz **Kurumsal hesaplar** gelen **kimlik doğrulamayı Değiştir** menüsünde, gerekli mantığı zaten sahip anahtar geçişi otomatik olarak işlemek için. Bu mantık, projeyle ilişkili iki veritabanı tablolarındaki kuruluşunuzun benzersiz tanımlayıcı ve imzalama anahtar bilgileri depolar. Veritabanı için bağlantı dizesini projenin Web.config dosyasında bulabilirsiniz.

Çözümünüz için el ile kimlik doğrulaması eklediyseniz gerekli anahtar geçişi mantığı uygulamanız olmayabilir. Kendiniz yazmak veya adımları gerekecek [Web uygulamaları / diğer kitaplıkları'nı kullanarak veya el ile desteklenen protokoller hiçbirini uygulama API'leri](#other).

Aşağıdaki adımlar mantıksal uygulamanızda düzgün çalıştığından emin olun yardımcı olur.

1. Visual Studio 2013'te çözümü açın ve ardından **Sunucu Gezgini** sağ pencereyi sekmesinde.
2. Genişletin **veri bağlantıları**, **DefaultConnection**, ardından **tabloları**. Bulun **IssuingAuthorityKeys** tablo, sağ tıklayın ve ardından **tablo verilerini Göster**.
3. İçinde **IssuingAuthorityKeys** tablo, en az bir satır anahtarı parmak izi değerine karşılık gelen olacaktır. Tablodaki tüm satırları silin.
4. Sağ **kiracılar** tablosunu ve ardından **tablo verilerini Göster**.
5. İçinde **kiracılar** tablo, bir benzersiz dizin Kiracı tanımlayıcısı için karşılık gelen en az bir satır olacaktır. Tablodaki tüm satırları silin. Her iki satır silmezseniz **kiracılar** tablo ve **IssuingAuthorityKeys** tablo, çalışma zamanında bir hata alırsınız.
6. Derleme ve uygulamayı çalıştırın. Hesabınızda oturum açtıktan sonra uygulama durdurabilirsiniz.
7. Geri dönüp **Sunucu Gezgini** değerleri bakın **IssuingAuthorityKeys** ve **kiracılar** tablo. Bunlar otomatik olarak Federasyon meta veri belgesi uygun bilgilerle katalogunun olduğunu fark edeceksiniz.

### <a name="vs2013"></a>Kaynakları koruma ve Visual Studio 2013 ile oluşturulan web API'leri
Bir web API uygulamasını Visual Studio 2013 Web API şablonu kullanılarak oluşturulan ve ardından seçili **Kurumsal hesaplar** gelen **kimlik doğrulamayı Değiştir** menüsünde, önceden sahip gerekli mantıksal uygulamanızda.

Kimlik doğrulama el ile yapılandırdıysanız, Web API'niz anahtar bilgilerini otomatik olarak güncelleştirmek için yapılandırma hakkında bilgi edinmek için aşağıdaki yönergeleri izleyin.

Aşağıdaki kod parçacığı Federasyon meta veri belge en son anahtarlarını edinmeniz ve daha sonra yapmayı gösteren [JWT belirteci işleyicisi](https://msdn.microsoft.com/library/dn205065.aspx) belirteci doğrulamak için. Kod parçacığı, kendi önbelleğe alma mekanizması anahtar kalıcı hale getirilmesine yönelik Azure AD'den gelecekteki belirteçleri doğrulamak için bir veritabanı, yapılandırma dosyası veya başka bir yerde oluşmasından aynısının kullanılacağını varsayar.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IdentityModel.Tokens;
using System.Configuration;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IdentityModel.Metadata;
using System.ServiceModel.Security;
using System.Threading;

namespace JWTValidation
{
    public class JWTValidator
    {
        private string MetadataAddress = "[Your Federation Metadata document address goes here]";

        // Validates the JWT Token that's part of the Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in the Azure Portal]",
                ValidIssuer = "[The issuer for the token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache the signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from the specified metadata document.
        public List<X509SecurityToken> GetSigningCertificates(string metadataAddress)
        {
            List<X509SecurityToken> tokens = new List<X509SecurityToken>();

            if (metadataAddress == null)
            {
                throw new ArgumentNullException(metadataAddress);
            }

            using (XmlReader metadataReader = XmlReader.Create(metadataAddress))
            {
                MetadataSerializer serializer = new MetadataSerializer()
                {
                    // Do not disable for production code
                    CertificateValidationMode = X509CertificateValidationMode.None
                };

                EntityDescriptor metadata = serializer.ReadMetadata(metadataReader) as EntityDescriptor;

                if (metadata != null)
                {
                    SecurityTokenServiceDescriptor stsd = metadata.RoleDescriptors.OfType<SecurityTokenServiceDescriptor>().First();

                    if (stsd != null)
                    {
                        IEnumerable<X509RawDataKeyIdentifierClause> x509DataClauses = stsd.Keys.Where(key => key.KeyInfo != null && (key.Use == KeyType.Signing || key.Use == KeyType.Unspecified)).
                                                             Select(key => key.KeyInfo.OfType<X509RawDataKeyIdentifierClause>().First());

                        tokens.AddRange(x509DataClauses.Select(token => new X509SecurityToken(new X509Certificate2(token.GetX509RawData()))));
                    }
                    else
                    {
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in the metadata");
                    }
                }
                else
                {
                    throw new Exception("Invalid Federation Metadata document");
                }
            }
            return tokens;
        }
    }
}
```

### <a name="vs2012"></a>Kaynakları koruma ve Visual Studio 2012 ile oluşturulan web uygulamaları
Uygulamanızı Visual Studio 2012'de oluşturulduysa, büyük olasılıkla kimlik ve erişim aracı Uygulamanızı yapılandırmak için kullanılır. Ayrıca, kullandığınız olma olasılığı yüksektir [doğrulama verenin ad kayıt defteri (VINR)](https://msdn.microsoft.com/library/dn205067.aspx). VINR, güvenilen kimlik sağlayıcılar (Azure AD) hakkında bilgi ve onlar tarafından verilen belirteçleri doğrulamak için kullanılan anahtarları bakımından sorumludur. VINR ayrıca otomatik olarak, dizininizle ilişkilendirilen en son Federasyon meta veri belgesi yükleyerek bir Web.config dosyasında saklanan anahtar bilgileri güncelleştirmek yapılandırmanın en son belge ile güncel olup olmadığı denetlenirken kolaylaştırır ve Gerekirse yeni anahtarı kullanmak için uygulamayı güncelleştiriliyor.

Uygulamanızı herhangi bir kod örnekleri veya Microsoft tarafından sağlanan adım adım kılavuz belgeleri kullanarak oluşturduysanız, anahtar geçişi mantığı, projenizde zaten eklenmiştir. Aşağıdaki kod, projenizde zaten olduğunu fark edeceksiniz. Bu mantıksal uygulamanız zaten yoksa ekleyin ve düzgün çalıştığını doğrulamak için aşağıdaki adımları izleyin.

1. İçinde **Çözüm Gezgini**, bir başvuru ekleyin **System.IdentityModel** uygun projesi için derleme.
2. Açık **Global.asax.cs** dosyasını açıp aşağıdaki yönergeleri kullanarak:
   ```
   using System.Configuration;
   using System.IdentityModel.Tokens;
   ```
3. Aşağıdaki yöntemi ekleyin **Global.asax.cs** dosyası:
   ```
   protected void RefreshValidationSettings()
   {
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
   }
   ```
4. Çağırma **RefreshValidationSettings()** yönteminde **Application_Start()** yönteminde **Global.asax.cs** gösterildiği gibi:
   ```
   protected void Application_Start()
   {
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
   }
   ```

Bu adımları uyguladıktan sonra uygulamanızın Web.config son anahtarları gibi Federasyon meta veri belgesi en son bilgilerle güncelleştirilir. Bu güncelleştirme, IIS, uygulama havuzunu geri dönüştüren her zaman meydana gelir; Varsayılan olarak IIS uygulamaları 29 saatte geri dönüştürmek için ayarlanır.

Anahtar geçişi mantığı çalıştığını doğrulamak için aşağıdaki adımları izleyin.

1. Yukarıdaki kod, uygulamanızın kullandığı doğruladıktan sonra açmak **Web.config** gidin ve dosya  **\<issuerNameRegistry >** özellikle örnek kod bloğu birkaç satırı aşağıdaki:
   ```
   <issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
   ```
2. İçinde  **\<parmak izini ekleyin = "" >** ayarlama, farklı bir karakterle değiştirilerek parmak izi değerini değiştirin. Kaydet **Web.config** dosya.
3. Uygulamayı derleyin ve çalıştırın. Oturum açma işlemini tamamladığınızda, uygulamanız başarıyla anahtar dizin Federasyon meta veri belge gerekli bilgileri indirerek güncelleştiriliyor. Oturum açma sorunları yaşıyorsanız, uygulamanızdaki değişiklikleri doğru okuyarak emin [ekleme oturum açma, Web uygulaması kullanarak Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) makalenin veya indiriliyor ve İnceleme aşağıdaki kod örneği: [Azure Active Directory için çok Kiracılı bulut uygulaması](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).

### <a name="vs2010"></a>Kaynakları koruma ve Visual Studio 2008 veya 2010 ile oluşturulan web uygulamaları ve .NET 3.5 için Windows Identity Foundation (WIF) v1.0
Bir uygulamayı WIF v1.0 oluşturulduysa, yeni bir anahtar kullanmak için uygulamanızın yapılandırmasını otomatik olarak yenilemek için sağlanan bir mekanizma yoktur.

* *En kolay yolu* WIF, en son meta veri belgesi alabilir ve yapılandırmanızı güncelleştirmek, SDK'da bulunan FedUtil araçları kullanın.
* WIF System ad alanında bulunan en yeni sürümünü içeren .NET 4.5, uygulamanızı güncelleştirin. Ardından [doğrulama verenin ad kayıt defteri (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) uygulamanın yapılandırmasının otomatik güncelleştirmeler gerçekleştirmek için.
* Yönergeler bu belgenin sonundaki yönergeler doğrultusunda el ile bir geçiş gerçekleştirin.

Yapılandırmanızı güncelleştirmek için FedUtil kullanma yönergeleri:

1. WIF v1.0 SDK'sı, geliştirme makinenizde Visual Studio 2008 veya 2010 için yüklü olduğunu doğrulayın. Yapabilecekleriniz [buradan indirin](https://www.microsoft.com/en-us/download/details.aspx?id=4451) henüz yüklemediyseniz.
2. Visual Studio'da Çözüm açmak geçerli projeye sağ tıklayıp seçin **Federasyon meta verilerini güncelleştirme**. Bu seçenek kullanılabilir değilse, FedUtil ve/veya WIF v1.0 SDK'sı yüklü değil.
3. İsteminden, işaretleyin **güncelleştirme** , Federasyon meta verilerini güncelleştirme başlatmak için. Uygulamanın barındırıldığı sunucu ortamınıza erişimi varsa, isteğe bağlı olarak FedUtil'ın kullanabilirsiniz [otomatik meta veri güncelleştirme zamanlayıcı](https://msdn.microsoft.com/library/ee517272.aspx).
4. Tıklayın **son** güncelleştirme işlemini tamamlamak için.

### <a name="other"></a>Web uygulamaları / API'leri kullanarak tüm diğer kitaplıkları veya desteklenen protokolden herhangi birini el ile uygulanması kaynakları koruma
Desteklenen protokolden herhangi birini el ile uygulanan veya başka bir kitaplık kullanıyorsanız, kitaplık veya uygulamanızın Openıd Connect bulma belge veya Federasyon meta verileri anahtar alındığını emin olmak için gözden geçirmek gerekir Belge. Bunu denetlemek için bir kodunuzu ya da kitaplık kodu Openıd keşif belgesi ya da Federasyon meta veri belgesi çağrıları için arama yapmak için yoludur.

Anahtar depolanıyor yere veya kodlanmış uygulamanızdaki el ile anahtarı almak ve buna uygun olarak bir el ile geçiş yönergeler bu belgenin sonundaki yönergeler doğrultusunda gerçekleştiriliyor güncelleştirin. **Otomatik geçişi desteklemek üzere uygulamanızı geliştirmek kesinlikle önerilir** yaklaşımları ana hattın bu makalede Azure AD, geçişi uyumu artırır veya acil bir durum varsa, gelecekteki kesintileri ve ek yükü önlemek için kullanarak bant dışı geçişi.

## <a name="how-to-test-your-application-to-determine-if-it-will-be-affected"></a>Bunu etkilenecek varsa belirlemek için uygulamanızı test etme
Komut dosyalarını indirerek ve yönergeleri izleyerek otomatik anahtar geçişi, uygulamanızın destekleyip desteklemediğini doğrulayabilirsiniz [bu GitHub deposu.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)

## <a name="how-to-perform-a-manual-rollover-if-your-application-does-not-support-automatic-rollover"></a>Uygulamanızı otomatik geçişi desteklemiyorsa el ile geçişi gerçekleştirme
Uygulamanız varsa **değil** otomatik geçişi desteği, düzenli aralıklarla projenin izleyiciler Azure AD imzalama işlemi anahtarları ve buna göre el ile aktarma gerçekleştirir oluşturmak ihtiyacınız olacak. [Bu GitHub deposundan](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) betikleri ve bunun nasıl yapılacağı hakkında yönergeler içerir.

