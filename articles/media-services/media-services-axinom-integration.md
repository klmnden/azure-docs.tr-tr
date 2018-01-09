---
title: "Azure Media Services Widevine lisansları teslim için Axinom kullanma | Microsoft Docs"
description: "Bu makalede, PlayReady ve Widevine DRMs AMS tarafından dinamik olarak şifrelenmiş bir akış sağlamak üzere Azure Media Services (AMS) nasıl kullanabileceğinizi açıklar. Medya Hizmetleri PlayReady lisans sunucusundan PlayReady lisans gelir ve Axinom lisans sunucusu tarafından Widevine lisans teslim edilir."
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.assetid: 9c93fa4e-b4da-4774-ab6d-8b12b371631d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: willzhan;Mingfeiy;rajputam;Juliako
ms.openlocfilehash: 9a3aa1680ada03e4472db3a198a3b806511671ed
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2018
---
# <a name="using-axinom-to-deliver-widevine-licenses-to-azure-media-services"></a>Azure Media Services’ta Widevine lisansları vermek için Axinom kullanma
> [!div class="op_single_selector"]
> * [castLabs](media-services-castlabs-integration.md)
> * [Axinom](media-services-axinom-integration.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Azure Media Services (AMS), Google Widevine dinamik koruma ekledi (bkz [Mingfei'nın blogu](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) Ayrıntılar için). Ayrıca, Azure Media Player (AMP) Widevine destek da ekledi (bkz [AMP belge](http://amp.azure.net/libs/amp/latest/docs/) Ayrıntılar için). Bu çok-native DRM ile (PlayReady ve Widevine) CENC tarafından korunan tire içeriği akış ana gerçekleştirmek MSE ve EME ile donatılmış modern tarayıcılar açıktır.

Media Services .NET SDK sürüm 3.5.2'de ile başlayarak, Media Services, Widevine lisans şablonu yapılandırmak ve Widevine lisansı alabilmeniz sağlar. Widevine lisansları teslim etmenize yardımcı olması için şu AMS ortaklarını da kullanabilirsiniz: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

Bu makalede, tümleştirme ve Axinom tarafından yönetilen Widevine lisans sunucusunu test açıklar. Özellikle şunları içerir:  

* Dinamik ortak şifreleme çoklu DRM (PlayReady ve Widevine) karşılık gelen lisans edinme URL'ler ile yapılandırılmasını;
* Lisans sunucusu gereksinimlerini karşılamak için JWT belirteci oluşturma;
* Lisans edinme JWT belirteci kimlik doğrulaması ile işler Azure Media Player uygulama geliştirme;

Tam sistem ve içerik anahtarı akış anahtarı kimlik, anahtar çekirdek, JTW belirteci ve kendi talep en iyi tanımlanabilen tarafından Aşağıdaki diyagramda:

![ÇİZGİ ve CENC](./media/media-services-axinom-integration/media-services-axinom1.png)

## <a name="content-protection"></a>Content Protection
Dinamik koruma ve anahtar teslim ilkesini yapılandırmak için lütfen Mingfei'nın blogu bakın: [Azure Media Services ile Widevine paketlemeyi yapılandırma](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services).

Dinamik CENC koruma çoklu DRM ile DASH şunların ikisini de sahip akış için yapılandırabilirsiniz:

1. MS Edge ve bir belirteç yetkilendirme kısıtlama olabilir IE11, PlayReady koruma. Belirteç kısıtlamalı ilkenin tarafından bir güvenli belirteç hizmeti (STS), Azure Active Directory gibi bir belirteç olarak eklenmelidir;
2. Widevine koruma Chrome için bunu başka bir STS tarafından verilen belirteci ile belirteci kimlik doğrulamasını isteyebilirsiniz. 

Bkz: [JWT belirteci oluşturma](media-services-axinom-integration.md#jwt-token-generation) bölüm için Axinom'ın Widevine lisans sunucusu neden Azure Active Directory bir STS olarak kullanılamaz.

### <a name="considerations"></a>Dikkat edilmesi gerekenler
1. Belirtilen Axinom kullanmalısınız anahtar çekirdek (8888000000000000000000000000000000000000) ve oluşturulan veya seçili anahtar anahtar teslim hizmeti yapılandırmak için içerik anahtarı oluşturmak için kimliği. Axinom lisans sunucusu sınama ve üretim için geçerli olan aynı anahtar çekirdek göre içerik anahtarı içeren tüm lisanslar verir.
2. Test Widevine lisans edinme URL'si: [https://drm-widevine-licensing.axtest.net/AcquireLicense](https://drm-widevine-licensing.axtest.net/AcquireLicense). Hem HTTP hem de HTTS izin verilir.

## <a name="azure-media-player-preparation"></a>Azure Media Player hazırlama
AMP v1.4.0 dinamik olarak PlayReady ve Widevine DRM ile paketlenmiştir AMS içeriği kayıttan yürütmeyi destekler.
Widevine lisans sunucusu belirteci kimlik doğrulaması gerektirmiyorsa, Widevine tarafından korunan bir tire içeriği test etmek için gerçekleştirmeniz gereken ek bir şey yoktur. Örneğin, basit bir AMP takım sağlar [örnek](http://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevine_notoken.html), bu sınır ve IE11 ile PlayReady ve Widevine ile Chrome çalışma burada görebilirsiniz.
Axinom tarafından sağlanan Widevine lisans sunucusunu JWT belirteci kimlik doğrulaması gerektirir. JWT belirteci bir HTTP üstbilgisi "X-AxDRM-ileti" aracılığıyla lisans istekle gönderilmesi gerekiyor. Bu amaç için kaynak ayarlamadan önce AMP barındırma web sayfasında aşağıdaki javascript eklemeniz gerekir:

    <script>AzureHtml5JS.KeySystem.WidevineCustomAuthorizationHeader = "X-AxDRM-Message"</script>

Rest AMP kodunun standart AMP AMP belge olduğu gibi API'dir [burada](http://amp.azure.net/libs/amp/latest/docs/).

AMP resmi uzun vadeli yaklaşım yayımlanmadan önce ayarı özel yetkilendirme üst bilgisi için yukarıdaki javascript hala bir kısa süreli bir yaklaşımdır.

## <a name="jwt-token-generation"></a>JWT belirteci oluşturma
Test Axinom Widevine lisans sunucusu JWT belirteci kimlik doğrulaması gerektirir. Ayrıca, JWT belirteci Taleplerde temel veri türü yerine karmaşık nesne türünün biridir.

Ne yazık ki, Azure AD ile ilkel türler yalnızca JWT belirteçleri verebilir. Benzer şekilde, .NET Framework API (System.IdentityModel.tokens.securitytokenhandler ve JwtPayload) yalnızca talepler olarak karmaşık nesne türü giriş olanak sağlar. Ancak, talep hala dize olarak serileştirilir. Bu nedenle biz herhangi bir JWT belirteci için Widevine lisans isteği oluşturmak için iki kullanamazsınız.

John Sheehan'ın [JWT NuGet paketi](https://www.nuget.org/packages/JWT) Biz bu NuGet paketi kullanacaksanız şekilde gereksinimlerini karşılar.

Test etmek için gerekli taleplerle Axinom Widevine lisans sunucusu gerektirdiği şekilde oluşturma JWT belirteci için kod aşağıdadır:

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.IdentityModel.Tokens;
    using System.IdentityModel.Protocols.WSTrust;
    using System.Security.Claims;

    namespace OpenIdConnectWeb.Utils
    {
        public class JwtUtils
        {
            //using John Sheehan's NuGet JWT library: https://www.nuget.org/packages/JWT/
            public static string CreateJwtSheehan(string symmetricKeyHex, string key_id)
            {
                byte[] symmetricKey = ConvertHexStringToByteArray(symmetricKeyHex);  //hex string to byte[] Note: Note that the key is a hex string, however it must be treated as a series of bytes not a string when encoding.

                var payload = new Dictionary<string, object>()
                             {
                                 { "version", 1 },
                                 { "com_key_id", System.Configuration.ConfigurationManager.AppSettings["ax:com_key_id"] },
                                 { "message", new { type = "entitlement_message", key_ids = new string[] { key_id } }  }
                             };

                string token = JWT.JsonWebToken.Encode(payload, symmetricKey, JWT.JwtHashAlgorithm.HS256);

                return token;
            }

            //convert hex string to byte[]
            public static byte[] ConvertHexStringToByteArray(string hexString)
            {
                if (hexString.Length % 2 != 0)
                {
                    throw new ArgumentException(String.Format(System.Globalization.CultureInfo.InvariantCulture, "The binary key cannot have an odd number of digits: {0}", hexString));
                }

                byte[] HexAsBytes = new byte[hexString.Length / 2];
                for (int index = 0; index < HexAsBytes.Length; index++)
                {
                    string byteValue = hexString.Substring(index * 2, 2);
                    HexAsBytes[index] = byte.Parse(byteValue, System.Globalization.NumberStyles.HexNumber, System.Globalization.CultureInfo.InvariantCulture);
                }

                return HexAsBytes;
            }

        }  

    }  

Axinom Widevine lisans sunucusu

    <add key="ax:laurl" value="http://drm-widevine-licensing.axtest.net/AcquireLicense" />
    <add key="ax:com_key_id" value="69e54088-e9e0-4530-8c1a-1eb6dcd0d14e" />
    <add key="ax:com_key" value="4861292d027e269791093327e62ceefdbea489a4c7e5a4974cc904b840fd7c0f" />
    <add key="ax:keyseed" value="8888000000000000000000000000000000000000" />

### <a name="considerations"></a>Dikkat edilmesi gerekenler
1. AMS PlayReady lisans teslimat hizmetinin gerektirir olsa bile "taşıyıcı =" kimlik doğrulama belirtecini önceki, Axinom Widevine lisans sunucusu onu kullanmaz.
2. Axinom iletişimi anahtar imzalama anahtarı olarak kullanılır. Ancak, bu bir dize değil bir bayt serisi kodlarken Muamele görmelidir onaltılık dize anahtardır. Bu ConvertHexStringToByteArray yöntemi tarafından sağlanır.

## <a name="retrieving-key-id"></a>Anahtar kimliği alma
JWT'nin oluşturmak için kod belirteci, anahtar kimliği gerekli olduğunu fark etmiş olabilirsiniz. Yükleme AMP player önce hazır olmasını JWT belirteci gereksinimlerini anahtar beri kimliği JWT belirteci üretmek için alınması gerekir.

Elbette, anahtarın askıya almak için birden çok yolu vardır kimliği Örneğin, bir depolayabilir içerik meta veri veritabanındaki birlikte anahtar kimliği. Veya, alabilirsiniz anahtar tire MPD (medya sunu açıklaması) dosyasından kimliği. Aşağıdaki kodu ikincisi ' dir.

    //get key_id from DASH MPD
    public static string GetKeyID(string dashUrl)
    {
        if (!dashUrl.EndsWith("(format=mpd-time-csf)"))
        {
            dashUrl += "(format=mpd-time-csf)";
        }

        XPathDocument objXPathDocument = new XPathDocument(dashUrl);
        XPathNavigator objXPathNavigator = objXPathDocument.CreateNavigator();
        XmlNamespaceManager objXmlNamespaceManager = new XmlNamespaceManager(objXPathNavigator.NameTable);
        objXmlNamespaceManager.AddNamespace("",     "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("ns1",  "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("cenc", "urn:mpeg:cenc:2013");
        objXmlNamespaceManager.AddNamespace("ms",   "urn:microsoft");
        objXmlNamespaceManager.AddNamespace("mspr", "urn:microsoft:playready");
        objXmlNamespaceManager.AddNamespace("xsi",  "http://www.w3.org/2001/XMLSchema-instance");
        objXmlNamespaceManager.PushScope();

        XPathNodeIterator objXPathNodeIterator;
        objXPathNodeIterator = objXPathNavigator.Select("//ns1:MPD/ns1:Period/ns1:AdaptationSet/ns1:ContentProtection[@value='cenc']", objXmlNamespaceManager);

        string key_id = string.Empty;
        if (objXPathNodeIterator.MoveNext())
        {
            key_id = objXPathNodeIterator.Current.GetAttribute("default_KID", "urn:mpeg:cenc:2013");
        }

        return key_id;
    }

## <a name="summary"></a>Özet
Hem Azure medya Hizmetleri içerik koruma, hem de Azure Media Player Widevine destek son eklenmesiyle biz AMS ve Widevine lisans sunucusundan Axinom aşağıdaki modern tarayıcılar için her iki PlayReady lisans hizmetiyle tire + çok-native DRM (PlayReady + Widevine) akış uygulayabilirsiniz:

* Chrome
* Windows 10 Microsoft Edge
* Windows 8.1 ve Windows 10 IE 11
* Firefox (Masaüstü) ve Mac (iOS değil) üzerinde Safari de Silverlight ve Azure Media Player ile aynı URL aracılığıyla desteklenir

Aşağıdaki parametreleri Mini çözüm yararlanmayı Axinom Widevine lisans sunucusu gereklidir. Anahtar dışında kimliği, parametre kalan kendi Widevine sunucu kurulumu temel alınarak Axinom tarafından sağlanır.

| Parametre | Nasıl kullanılır |
| --- | --- |
| İletişim anahtar kimliği |JWT belirteci "com_key_id" talep değeri olarak dahil edilmesi gerekir (bkz [bu](media-services-axinom-integration.md#jwt-token-generation) bölümü). |
| İletişim anahtarı |JWT belirteci imzalama anahtarı olarak kullanılan gerekir (bkz [bu](media-services-axinom-integration.md#jwt-token-generation) bölümü). |
| Anahtar çekirdek |Verilen tüm içeriğe sahip içerik anahtarı oluşturmak için kullanılması gereken anahtar kimliği (bkz [bu](media-services-axinom-integration.md#content-protection) bölümü). |
| Widevine lisans edinme URL'si |TİRE akış için varlık teslim ilkesini yapılandırma kullanılması gerekir (bkz [bu](media-services-axinom-integration.md#content-protection) bölümü). |
| İçerik anahtarı kimliği |JWT belirteci yetkilendirme ileti talep değerini bir parçası olarak dahil olması gerekir (bkz [bu](media-services-axinom-integration.md#jwt-token-generation) bölümü). |

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a>İlgili kaynaklar
Bu belge oluşturma doğrultusunda katkıda bulunan aşağıdaki kişilerin onaylamak isteriz: Axinom, Kristjan Jõgi, Mingfei Yan ve Amit Rajput.

