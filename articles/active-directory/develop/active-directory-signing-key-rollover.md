---
title: Azure AD'de imzalama anahtarı geçişi
description: Bu makalede, Azure Active Directory için imzalama anahtarı geçiş işlemini en iyi yöntemler açıklanmaktadır
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2016
ms.author: celested
ms.reviewer: dastrock
ms.custom: aaddev
ms.openlocfilehash: 02d7cb28411e0baec20d334994b385dcd3b06451
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35293390"
---
# <a name="signing-key-rollover-in-azure-active-directory"></a>Azure Active Directory'de anahtar geçişi imzalama
Bu makalede, Azure Active Directory (Azure AD) güvenlik belirteçleri imzalamak için kullanılan ortak anahtarlar hakkında bilmeniz gerekenler açıklanmaktadır. Bu anahtarları rollover düzenli aralıklarla ve acil bir durumda uzatılabilir olduğunu hemen dikkate almak önemlidir. Azure AD kullanan tüm uygulamalar program aracılığıyla anahtarı geçiş işlemi veya düzenli el ile geçiş işlemi oluşturmak mümkün olması gerekir. Anahtarları nasıl çalıştığını, anlamak için okumaya devam uygulamanıza rollover etkisini değerlendirin ve uygulamanızı güncelleştirmeniz veya gerekiyorsa, anahtar geçişi işlemek için düzenli el ile geçiş işlemi oluşturmak.

## <a name="overview-of-signing-keys-in-azure-ad"></a>Azure AD'de imzalama anahtarlarının genel bakış
Azure AD kendisi ve kullanan uygulamaları arasında güven sağlamak için endüstri standartları üzerine inşa edilen ortak anahtar şifrelemesi kullanır. Bu pratikteki, aşağıdaki şekilde çalışır: Azure AD genel ve özel bir anahtar çiftinden oluşur imzalama bir anahtar kullanır. Azure AD için kimlik doğrulaması kullanan bir uygulama için bir kullanıcı oturum açtığında, Azure AD kullanıcı hakkındaki bilgileri içeren bir güvenlik belirteci oluşturur. Bu belirteç uygulama geri gönderilmeden önce özel anahtarını kullanarak Azure AD tarafından imzalanır. Belirtecin geçerli ve Azure AD'den kaynaklı olduğunu doğrulamak için uygulama kiracının içinde yer alan Azure AD tarafından sunulan ortak anahtarı kullanılarak belirtecinin imzası doğrulamalısınız [Openıd Connect bulma belge](http://openid.net/specs/openid-connect-discovery-1_0.html) veya SAML / WS-Fed [Federasyon meta veri belgesi](active-directory-federation-metadata.md).

Güvenlik nedeniyle, Azure AD anahtar dökümünü düzenli aralıklarla ve Acil, imzalama uzatılabilir hemen. Azure AD ile tümleşir herhangi bir uygulama ne sıklıkta oluşabilir olsun anahtar geçişi olayını işlemek için hazırlıklı olmalıdır. Yoktur ve uygulamanızı bir belirteç imzayı doğrulamak için süresi dolmuş bir anahtarı kullanmayı dener, oturum açma isteği başarısız olur.

Her zaman birden fazla geçerli anahtar Openıd Connect bulma belge ve Federasyon meta veri belgesi mevcut değil. Uygulamanız herhangi bir belgede belirtilen anahtarları kullanmak hazırlıklı olmalıdır, bir anahtar en kısa sürede geri alınması olduğundan, başka bir değişimi olması ve benzeri.

## <a name="how-to-assess-if-your-application-will-be-affected-and-what-to-do-about-it"></a>Uygulamanızı etkilenecek varsa değerlendirmek nasıl ve ne hakkında
Anahtar geçişi, uygulamanızın nasıl işlediğini uygulama veya hangi kimlik protokolü ve kitaplık kullanıldı türünü gibi değişkenleri bağlıdır. Aşağıdaki bölümler, en sık karşılaşılan uygulamalar tarafından anahtar geçişi etkilenen ve anahtarını el ile güncelleştirin veya otomatik geçişi desteklemek üzere uygulamayı güncelleştirme hakkında kılavuzluk olup olmadığını değerlendirin.

* [Yerel istemci uygulamaları kaynaklara erişme](#nativeclient)
* [Web uygulamaları / API'leri kaynaklara erişme](#webclient)
* [Web uygulamaları / API'leri kaynakları koruma ve Azure App Services kullanılarak oluşturulmuş](#appservices)
* [Web uygulamaları / .NET OWIN Openıd Connect, WS-Fed veya WindowsAzureActiveDirectoryBearerAuthentication ara yazılımı kullanarak kaynakları koruma API'leri](#owin)
* [Web uygulamaları / .NET Core Openıd Connect veya JwtBearerAuthentication ara yazılımı kullanarak kaynakları koruma API'leri](#owincore)
* [Web uygulamaları / Node.js passport azure ad modülünü kullanarak kaynakları koruma API'leri](#passport)
* [Web uygulamaları / API'leri kaynakları koruma ve Visual Studio 2015 veya Visual Studio 2017 oluşturulmuş](#vs2015)
* [Kaynakları koruma ve Visual Studio 2013 ile oluşturulan web uygulamaları](#vs2013)
* [Kaynakları koruma ve Visual Studio 2013 ile oluşturulan web API'leri](#vs2013_webapi)
* [Kaynakları koruma ve Visual Studio 2012 ile oluşturulan web uygulamaları](#vs2012)
* [Kaynakları koruma ve Windows Identity Foundation'ı kullanarak 2008 o Visual Studio 2010 ile oluşturulan web uygulamaları](#vs2010)
* [Web uygulamaları / diğer kitaplıkları'nı kullanarak veya el ile desteklenen protokoller hiçbirini uygulama kaynakları koruma API'leri](#other)

Bu kılavuz **değil** için geçerlidir:

* Azure AD uygulama (özel dahil) galerisinden eklenen uygulamalar, anahtarları imzalama göre ayrı Kılavuzu vardır. [Daha fazla bilgi.](../manage-apps/manage-certificates-for-federated-single-sign-on.md)
* Uygulama Ara sunucusu üzerinden yayımlanan uygulamalarla anahtarları imzalama hakkında endişelenmeniz gerekmez şirket içi.

### <a name="nativeclient"></a>Yerel istemci uygulamaları kaynaklara erişme
Yalnızca (yani kaynaklarına erişen uygulamalar Microsoft Graph, KeyVault, Outlook API ve diğer Microsoft APIs) genellikle yalnızca bir belirteç almak ve bunu boyunca kaynak sahibine geçirin. O tüm kaynakları koruma değil, belirteç incelemek değil ve bu nedenle doğru şekilde imzalanmış olmak zorunda değildir.

Yerel istemci uygulamaları, masaüstü veya mobil, bu kategoriye ve böylece rollover tarafından etkilenmez.

### <a name="webclient"></a>Web uygulamaları / API'leri kaynaklara erişme
Yalnızca (yani kaynaklarına erişen uygulamalar Microsoft Graph, KeyVault, Outlook API ve diğer Microsoft APIs) genellikle yalnızca bir belirteç almak ve bunu boyunca kaynak sahibine geçirin. O tüm kaynakları koruma değil, belirteç incelemek değil ve bu nedenle doğru şekilde imzalanmış olmak zorunda değildir.

Web uygulamaları ve yalnızca uygulama akışını kullanarak API web (istemci kimlik bilgileri / istemci sertifikası), bu kategoriye girer ve bu nedenle rollover tarafından etkilenmez.

### <a name="appservices"></a>Web uygulamaları / API'leri kaynakları koruma ve Azure App Services kullanılarak oluşturulmuş
Azure uygulama hizmetleri kimlik doğrulama / yetkilendirme (EasyAuth) işlevselliği, anahtar geçişi otomatik olarak işlemek için gerekli mantığı zaten sahip.

### <a name="owin"></a>Web uygulamaları / .NET OWIN Openıd Connect, WS-Fed veya WindowsAzureActiveDirectoryBearerAuthentication ara yazılımı kullanarak kaynakları koruma API'leri
Uygulamanız .NET OWIN Openıd Connect, WS-Fed veya WindowsAzureActiveDirectoryBearerAuthentication ara yazılımı kullanıyorsa, anahtar geçişi otomatik olarak işlemek için gerekli mantığı zaten içeriyor.

Uygulamanız herhangi birini herhangi bir uygulama haline veya Startup.Auth.cs aşağıdaki kod parçacıkları için bakarak kullandığını doğrulayın

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

### <a name="owincore"></a>Web uygulamaları / .NET Core Openıd Connect veya JwtBearerAuthentication ara yazılımı kullanarak kaynakları koruma API'leri
Uygulamanız .NET Core OWIN Openıd Connect veya JwtBearerAuthentication ara yazılımı kullanıyorsanız, anahtar geçişi otomatik olarak işlemek için gerekli mantığı zaten içeriyor.

Uygulamanız herhangi birini herhangi bir uygulama haline veya Startup.Auth.cs aşağıdaki kod parçacıkları için bakarak kullandığını doğrulayın

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
Uygulamanızı Node.js passport ad modülünü kullanıyorsanız, anahtar geçişi otomatik olarak işlemek için gerekli mantığı zaten içeriyor.

Onaylayın, uygulama passport Uygulamanızın app.js aşağıdaki kod parçacığında arayarak ad

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <a name="vs2015"></a>Web uygulamaları / API'leri kaynakları koruma ve Visual Studio 2015 veya Visual Studio 2017 oluşturulmuş
Uygulamanızı Visual Studio 2015 ya da Visual Studio 2017 bir web uygulaması şablonu kullanılarak oluşturulan ve seçtiyseniz **iş ve Okul hesapları** gelen **kimlik doğrulamayı Değiştir** menüsünde, zaten sahip anahtar geçişi otomatik olarak işlemek için gerekli mantığı. OWIN Openıd Connect Ara katıştırılmış bu mantığı alır ve Openıd Connect bulma belgeden anahtarlarını önbelleğe alır ve bunları düzenli aralıklarla yeniler.

Kimlik doğrulama çözümünüz için el ile eklediyseniz, uygulamanız gereken anahtar geçişi mantığı sahip olmayabilir. Kendiniz yazmak veya adımları gerekecek [Web uygulamaları / diğer kitaplıkları'nı kullanarak veya el ile desteklenen protokoller hiçbirini uygulama API'leri](#other).

### <a name="vs2013"></a>Kaynakları koruma ve Visual Studio 2013 ile oluşturulan web uygulamaları
Uygulamanızı Visual Studio 2013'te bir web uygulaması şablonu kullanılarak oluşturulan ve seçtiyseniz **Kurumsal hesaplar** gelen **kimlik doğrulamayı Değiştir** menüsünde, zaten sahip anahtar geçişi otomatik olarak işlemek için gerekli mantığı. Bu mantık, projeyle ilişkili iki veritabanı tablolarındaki kuruluşunuzun benzersiz tanımlayıcı ve imzalama anahtar bilgileri depolar. Veritabanı için bağlantı dizesi projenin Web.config dosyasında bulabilirsiniz.

Kimlik doğrulama çözümünüz için el ile eklediyseniz, uygulamanız gereken anahtar geçişi mantığı sahip olmayabilir. Kendiniz yazmak veya adımları gerekecek [Web uygulamaları / diğer kitaplıkları'nı kullanarak veya el ile desteklenen protokoller hiçbirini uygulama API'leri](#other).

Aşağıdaki adımlar mantığı uygulamanızda düzgün çalıştığını doğrulamak yardımcı olur.

1. Visual Studio 2013'te çözümü açın ve ardından tıklatın **Sunucu Gezgini** sağ penceresi sekmesinde.
2. Genişletme **veri bağlantıları**, **DefaultConnection**ve ardından **tabloları**. Bulun **IssuingAuthorityKeys** tablo, sağ tıklatın ve ardından **Show Table Data**.
3. İçinde **IssuingAuthorityKeys** tablo, anahtar parmak izi değeri karşılık gelen en az bir satır olacak. Tabloda herhangi bir satır silin.
4. Sağ **kiracılar** tablo ve ardından **Show Table Data**.
5. İçinde **kiracılar** tablo, bir benzersiz dizin Kiracı tanımlayıcısına karşılık en az bir satır olacak. Tabloda herhangi bir satır silin. Hem de satır silmezseniz **kiracılar** tablo ve **IssuingAuthorityKeys** tablo, çalışma zamanında bir hata alırsınız.
6. Derleme ve uygulamayı çalıştırın. Hesabınızda oturum açtıktan sonra uygulama durdurabilirsiniz.
7. Geri dönüp **Sunucu Gezgini** ve değerler bakmak **IssuingAuthorityKeys** ve **kiracılar** tablo. Bunlar otomatik olarak Federasyon meta veri belgesi uygun bilgilerle yeniden olduğunu fark edeceksiniz.

### <a name="vs2013"></a>Kaynakları koruma ve Visual Studio 2013 ile oluşturulan web API'leri
Bir web API uygulaması Web API şablonunu kullanarak Visual Studio 2013'oluşturduysanız ve ardından seçili **Kurumsal hesaplar** gelen **kimlik doğrulamayı Değiştir** menüsünde, önceden uygulamanızda gerekli mantığı vardır.

Kimlik doğrulama el ile yapılandırdıysanız, anahtar bilgilerini otomatik olarak güncelleştirmek için Web API yapılandırma konusunda bilgi edinmek için aşağıdaki yönergeleri izleyin.

Aşağıdaki kod parçacığını Federasyon meta verileri belgeden son anahtarları almak ve daha sonra kullanmak gösterilmiştir [JWT belirteci işleyicisi](https://msdn.microsoft.com/library/dn205065.aspx) belirteci doğrulamak için. Kod parçacığı, kendi önbelleğe alma mekanizması anahtar sürdürmek için Azure AD'den gelecekteki belirteçleri doğrulamak için bir veritabanı, yapılandırma dosyası veya başka bir yerde olması gerekmediğini kullanılacağını varsayar.

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
Uygulamanızı Visual Studio 2012'de oluşturulduysa, büyük olasılıkla kimlik ve erişim aracı Uygulamanızı yapılandırmak için kullanılır. Ayrıca, kullandığınız büyük olasılıkla [doğrulama verenin adı kayıt defteri (VINR)](https://msdn.microsoft.com/library/dn205067.aspx). VINR güvenilen kimlik sağlayıcıları (Azure AD) hakkında bilgi ve onlar tarafından yayınlanan belirteçleri doğrulamak için kullanılan anahtarları sorumludur. VINR Ayrıca, bir dizinle ilişkili en son Federasyon meta veri belgesi yükleyerek bir Web.config dosyasında depolanan anahtar bilgilerini otomatik olarak güncelleştirmek kolaylaştırır yapılandırma ile son belge güncel olup olmadığı denetleniyor ve gerekirse yeni anahtarı kullanmak üzere uygulamayı güncelleştirme.

Uygulamanızı kod örnekleri veya Microsoft tarafından sağlanan izlenecek belgelerine herhangi birini kullanarak oluşturduysanız, anahtar geçişi mantığı zaten projenizde dahil edilir. Aşağıdaki kodu zaten projenizde var. fark edeceksiniz. Uygulamanız bu mantığı zaten yoksa ekleyin ve düzgün çalıştığını doğrulamak için aşağıdaki adımları izleyin.

1. İçinde **Çözüm Gezgini**, bir başvuru ekleyin **System.IdentityModel** uygun proje için derleme.
2. Açık **Global.asax.cs** dosya ve aşağıdaki yönergeleri kullanarak:
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

Bu adımları uyguladıktan sonra uygulamanızın Web.config son anahtarları dahil Federasyon meta veri belgesi en son bilgilerle güncelleştirilir. Bu güncelleştirme, IIS, uygulama havuzu geri dönüştürme sayısı her zaman meydana gelir; Varsayılan olarak IIS uygulamaları 29 saatte geri dönüştürmek için ayarlanır.

Anahtar geçişi mantığı çalıştığını doğrulamak için aşağıdaki adımları izleyin.

1. Yukarıdaki kod, uygulamanızın kullanıyor doğruladıktan sonra açmak **Web.config** dosya ve gidin **<issuerNameRegistry>** bloğu, özellikle için aşağıdakileri arayan birkaç satır:
   ```
   <issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
   ```
2. İçinde **<add thumbprint=””>** ayarı herhangi bir karakter ile farklı bir değiştirerek parmak izi değerini değiştirin. Kaydet **Web.config** dosya.
3. Uygulamayı oluşturun ve ardından çalıştırın. Oturum açma işlemini tamamlamak, uygulamanız başarıyla anahtarı, dizinin Federasyon meta verileri belgeden gerekli bilgileri yükleyerek güncelleştiriyor. Oturum açma sorunları yaşıyorsanız, uygulamanızdaki değişiklikleri doğru okuyarak olduğundan emin olun [ekleme oturum açma Web uygulaması kullanarak Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) makalenin veya indiriliyor ve aşağıdaki kod örneği inceleniyor: [ Azure Active Directory için çok Kiracılı bulut uygulaması](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).

### <a name="vs2010"></a>Kaynakları koruma ve Visual Studio 2008 veya 2010 ile oluşturulan web uygulamaları ve Windows Identity Foundation (WIF) v1.0 .NET 3.5 için
Bir uygulama WIF v1.0 oluşturulduysa, yeni bir anahtar kullanmak için uygulamanızın yapılandırmasını otomatik olarak yenilemek için sağlanan bir mekanizma yoktur.

* *En kolay yolu* WIF son meta veri belgesi almak ve yapılandırmanızı güncelleştirmek SDK içindeki FedUtil araçları kullanın.
* Sistem ad alanında bulunan WIF en yeni sürümünü içeren .NET 4.5, uygulamanıza güncelleştirin. Daha sonra [doğrulama verenin adı kayıt defteri (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) uygulamanın yapılandırma'nın otomatik güncelleştirmelerini gerçekleştirmek için.
* Bu kılavuzu belge sonunda yönergeler doğrultusunda el ile geçiş gerçekleştirin.

Yapılandırmanızı güncelleştirmek için FedUtil kullanmak için yönergeler:

1. WIF v1.0 SDK geliştirme makinenizde Visual Studio 2008 veya 2010 yüklü olduğunu doğrulayın. Yapabilecekleriniz [buradan indirin](https://www.microsoft.com/en-us/download/details.aspx?id=4451) henüz yüklemediyseniz.
2. Visual Studio'da Çözüm açmak ve ardından geçerli projeye sağ tıklayın ve seçin **güncelleştirme Federasyon meta verileri**. Bu seçenek kullanılabilir durumda değilse, FedUtil ve/veya WIF v1.0 SDK yüklenmedi.
3. İsteminden seçin **güncelleştirme** Federasyon meta verilerini güncelleştirme başlamak için. Uygulama barındırıldığı sunucu ortamı erişiminiz varsa, isteğe bağlı olarak FedUtil'ın kullanabilirsiniz [otomatik meta veri güncelleştirme zamanlayıcı](https://msdn.microsoft.com/library/ee517272.aspx).
4. Tıklatın **son** güncelleştirme işlemini tamamlamak için.

### <a name="other"></a>Web uygulamaları / diğer kitaplıkları'nı kullanarak veya el ile desteklenen protokoller hiçbirini uygulama kaynakları koruma API'leri
Desteklenen protokoller birini el ile uygulanan veya başka bir kitaplık kullanıyorsanız, kitaplığı veya Openıd Connect bulma belge veya Federasyon meta veri belgesi anahtar alındığını emin olmak için uygulamanızı gözden geçirmek gerekir. Bunun için bir şekilde kodunuzu veya kitaplığın kod Openıd bulma belge veya Federasyon meta veri belgesi yapılan her çağrı için arama yapmak için denetleyebilirsiniz.

Anahtarı depolanıyor yere veya sabit kodlanmış uygulamanızda el ile anahtarı almak ve buna göre bu kılavuzu belge sonunda yönergeler doğrultusunda el ile bir rollover göre gerçekleştirme güncelleştirin. **Kesinlikle otomatik geçişi desteklemek için uygulamanızın artırmak teşvik edilir** yaklaşımlar anahat birini Azure AD rollover tempoyla artırır veya Acil varsa gelecekteki kesintilerini ve ek yükü önlemek için bu makaledeki kullanarak bant dışı geçişi.

## <a name="how-to-test-your-application-to-determine-if-it-will-be-affected"></a>Bunu etkilenecek varsa belirlemek için uygulamanızı test etme
Komut dosyaları indirip'ndaki yönergeleri izleyen otomatik anahtar geçişi, uygulamanızın destekleyip desteklemediğini doğrulamak için [bu GitHub depo.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)

## <a name="how-to-perform-a-manual-rollover-if-your-application-does-not-support-automatic-rollover"></a>Uygulamanızın otomatik geçişi desteklemiyorsa, el ile geçiş gerçekleştirme
Uygulamanız varsa **değil** otomatik geçişi desteği, düzenli aralıklarla izleyiciler Azure AD imzalama işlemi anahtarları ve el ile bir rollover uygun şekilde gerçekleştirir kurmak gerekir. [Bu GitHub deposunu](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) komut dosyaları ve bunun nasıl yapılacağı hakkında yönergeler içerir.

