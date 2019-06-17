---
title: Azure Media Services Widevine lisanslarını teslim etmek için Axinom kullanma | Microsoft Docs
description: Bu makale, PlayReady ve Widevine benzeri DRM ile AMS tarafından dinamik olarak şifrelenmiş bir akış sunmak için Azure Media Services (AMS) nasıl kullanabileceğinizi açıklar. Media Services PlayReady lisans sunucusundan PlayReady lisans gelir ve Widevine lisans Axinom lisans sunucusu tarafından sağlanır.
services: media-services
documentationcenter: ''
author: willzhan
manager: femila
editor: ''
ms.assetid: 9c93fa4e-b4da-4774-ab6d-8b12b371631d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: willzhan;Mingfeiy;rajputam;Juliako
ms.openlocfilehash: 6714beae690e23c686fc08b88e93044ae3901c89
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61245040"
---
# <a name="using-axinom-to-deliver-widevine-licenses-to-azure-media-services"></a>Azure Media Services’ta Widevine lisansları vermek için Axinom kullanma 
> [!div class="op_single_selector"]
> * [castLabs](media-services-castlabs-integration.md)
> * [Axinom](media-services-axinom-integration.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Azure Media Services (AMS), Google Widevine dinamik koruma ekledi (bkz [Mingfei'nın blog](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) Ayrıntılar için). Ayrıca, Azure Media Player'ı (AMP) Widevine desteği de ekledi (bkz [AMP belge](https://amp.azure.net/libs/amp/latest/docs/) Ayrıntılar için). EME MSE ile donatılmış modern tarayıcılarda akış DASH içerik CENC tarafından çok-native-DRM (PlayReady ve Widevine) korumalı bir ana başarıyı budur.

Media Services .NET SDK sürüm 3.5.2 ile başlayarak, Media Services, Widevine lisans şablonu yapılandırma ve Widevine lisansları almak sağlar. Ayrıca, Widevine lisansları teslim etmenize yardımcı olmak için şu AMS ortaklarını da kullanabilirsiniz: [Axinom](https://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](https://ezdrm.com/), [castLabs](https://castlabs.com/company/partners/azure/).

Bu makalede, tümleştirme ve Widevine lisans sunucusu Axinom tarafından yönetilen test açıklar. Özellikle de kapsar:  

* Dinamik ortak şifreleme çoklu DRM (PlayReady ve Widevine) karşılık gelen lisans edinme URL'lerle ile yapılandırma;
* Lisans sunucusu gereksinimlerini karşılamak için bir JWT belirteci oluşturma;
* JWT belirteci kimlik doğrulaması ile lisans edinme işleyen Azure Media Player uygulaması geliştirme;

Anahtar tam sistem ve içerik anahtarı akışını kimliği, temel çekirdek, JTW belirteci ve kendi talep en iyi tanımlanabilen Aşağıdaki diyagramda tarafından:

![ÇİZGİ ve CENC](./media/media-services-axinom-integration/media-services-axinom1.png)

## <a name="content-protection"></a>Content Protection
Dinamik koruma ve anahtar teslim ilkesini yapılandırmak için lütfen Mingfei'nın bloguna bakın: [Azure Media Services ile Widevine paketlemeyi yapılandırma](https://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services).

Birden çok DRM ile DASH şunların ikisini de sahip akış için dinamik CENC korumayı yapılandırabilirsiniz:

1. Microsoft Edge ve bir belirteç bir yetkilendirme kısıtlaması olabilir ıe11, PlayReady korumalı. Belirteç kısıtlamalı ilkenin beraberinde tarafından bir güvenli belirteç hizmeti (STS), Azure Active Directory gibi verilmiş bir belirteç bulunmalıdır;
2. Widevine koruma Chrome için başka bir STS tarafından verilen belirtecin Bu belirteci kimlik doğrulaması gerektirebilir. 

Bkz: [JWT belirteci oluşturma](media-services-axinom-integration.md#jwt-token-generation) bölümünü Axinom'ın Widevine lisans sunucusu için neden Azure Active Directory STS kullanılamaz.

### <a name="considerations"></a>Dikkat edilmesi gerekenler
1. Belirtilen Axinom kullanmanız gerekir (8888000000000000000000000000000000000000) anahtar çekirdek ve oluşturulan veya seçili anahtar anahtar dağıtımı hizmetiyle yapılandırma içerik anahtarı oluşturmak için kimliği. Test ve üretim için geçerli olan aynı anahtar çekirdek temel içerik anahtarı içeren tüm lisansları Axinom lisans sunucusu verir.
2. Test için Widevine lisans edinme URL'si: [ https://drm-widevine-licensing.axtest.net/AcquireLicense ](https://drm-widevine-licensing.axtest.net/AcquireLicense). Hem HTTP hem de HTTS izin verilir.

## <a name="azure-media-player-preparation"></a>Azure Media Player hazırlama
AMP v1.4.0 dinamik olarak PlayReady ve Widevine DRM ile paketlenmiştir AMS içeriklerin destekler.
Widevine lisans sunucusu belirteci kimlik doğrulaması gerektirmiyorsa, Widevine tarafından korunan bir tire içeriği test etmek için gerçekleştirmeniz gereken ek bir şey yoktur. Örneğin, basit bir AMP takım sağlar [örnek](https://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevineFairPlay_notoken.html), bu Microsoft Edge ve ıe11 ile PlayReady ve Widevine ile Chrome çalışma burada görebilirsiniz.
Widevine lisans sunucusunu Axinom tarafından sağlanan JWT belirteci kimlik doğrulaması gerektirir. JWT belirteci bir HTTP üst bilgisi "X-AxDRM-Message" aracılığıyla lisans istekle gönderilmesi gerekir. Bu amaç için kaynak ayarlamadan önce AMP barındırma web sayfasında aşağıdaki javascript eklemeniz gerekir:

    <script>AzureHtml5JS.KeySystem.WidevineCustomAuthorizationHeader = "X-AxDRM-Message"</script>

AMP belge olduğu gibi standart AMP API'si AMP kodu geri kalanı, [burada](https://amp.azure.net/libs/amp/latest/docs/).

AMP resmi uzun vadeli yaklaşımda yayımlanmadan önce ayarı özel yetkilendirme üst bilgisi için yukarıdaki javascript hala bir kısa süreli bir yaklaşımdır.

## <a name="jwt-token-generation"></a>JWT belirteci oluşturma
JWT belirteci kimlik doğrulamasını test etmek için Axinom Widevine lisans sunucusu gerektirir. Ayrıca, JWT belirteçteki talepleri temel veri türü yerine karmaşık nesne türünün biridir.

Ne yazık ki, Azure AD ile ilkel türler yalnızca JWT belirteçleri verebilir. Benzer şekilde, .NET Framework API (System.IdentityModel.tokens.securitytokenhandler ve JwtPayload) yalnızca talepler olarak karmaşık nesne türü giriş olanak sağlar. Ancak, talepler yine de dize olarak serileştirilir. Bu nedenle size herhangi bir JWT belirteci için Widevine lisans isteği oluşturmak için iki kullanamazsınız.

John Sheehan'ın [JWT NuGet paketini](https://www.nuget.org/packages/JWT) gereksinimlerini karşılayan, böylece bu NuGet paketi kullanmak için kullanacağız.

Test etmek için Axinom Widevine lisans sunucusu gerektirdiği gerekli talep oluşturma JWT belirteci için kod aşağıda verilmiştir:

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

    <add key="ax:laurl" value="https://drm-widevine-licensing.axtest.net/AcquireLicense" />
    <add key="ax:com_key_id" value="69e54088-e9e0-4530-8c1a-1eb6dcd0d14e" />
    <add key="ax:com_key" value="4861292d027e269791093327e62ceefdbea489a4c7e5a4974cc904b840fd7c0f" />
    <add key="ax:keyseed" value="8888000000000000000000000000000000000000" />

### <a name="considerations"></a>Dikkat edilmesi gerekenler
1. AMS PlayReady lisans teslimat hizmetinin gerektiriyor olsa bile "taşıyıcı =" bir kimlik doğrulama belirteci yukarıdaki Axinom Widevine lisans sunucusu bunları kullanmaz.
2. Axinom iletişim anahtar imzalama anahtarı olarak kullanılır. Bir dizeyi bir bayt serisi kodlarken değerlendirilmesi gerekir ancak bir onaltılık dize anahtardır. Bu yöntem ConvertHexStringToByteArray tarafından sağlanır.

## <a name="retrieving-key-id"></a>Anahtar kimliği alınıyor
JWT'nin oluşturmak için kodda belirteci, anahtar kimliği gerekli olduğunu fark etmiş olabilirsiniz. JWT belirteci gereksinimlerini yükleme AMP player önce hazır olmasını anahtar beri JWT belirteci oluşturmak için alınacak kimliği gerekir.

Elbette, anahtarın askıya almak için birden çok yolu vardır kimliği Örneğin, bir depolayabilir içerik meta veri veritabanındaki birlikte anahtar kimliği. Veya, alabileceğiniz anahtar dosyasından DASH MPD (medya sunu açıklama) kimliği. İkincisi için aşağıdaki kodu var.

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
        objXmlNamespaceManager.AddNamespace("xsi",  "https://www.w3.org/2001/XMLSchema-instance");
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
Azure Media Services Content Protection, hem de Azure Media Player, Widevine desteğinin son ekiyle birlikte biz hem de PlayReady lisans hizmeti AMS ve Widevine lisans ile DASH + çok-native çoklu DRM (PlayReady ve Widevine) akış uygulayabilirsiniz Aşağıdaki modern tarayıcılar için Axinom sunucudan:

* Chrome
* Windows 10 için Microsoft Edge
* Windows 8.1 ve Windows 10 üzerinde IE 11
* Firefox (Masaüstü) ve Safari (iOS değil) Mac üzerinde hem de Silverlight ve Azure Media Player ile aynı URL aracılığıyla desteklenir

Aşağıdaki parametreleri Mini çözüm yararlanarak Axinom Widevine lisans sunucusu gerekir. Anahtar dışında kimliği, parametreleri geri kalanı, Widevine sunucusu kurulumuna bağlı Axinom tarafından sağlanır.

| Parametre | Nasıl kullanılır |
| --- | --- |
| İletişim anahtar kimliği |JWT belirteci "com_key_id" talep değeri olarak dahil edilmesi gereken (bkz [bu](media-services-axinom-integration.md#jwt-token-generation) bölümü). |
| İletişim anahtarı |JWT belirteci imzalama anahtarı kullanılmalıdır (bkz [bu](media-services-axinom-integration.md#jwt-token-generation) bölümü). |
| Anahtar kaynağı |Herhangi bir içerik ile içerik anahtarı oluşturmak için kullanılması gereken anahtar kimliği (bkz [bu](media-services-axinom-integration.md#content-protection) bölümü). |
| Widevine lisans edinme URL'si |DASH akış için varlık teslim ilkesini yapılandırma kullanılmalıdır (bkz [bu](media-services-axinom-integration.md#content-protection) bölümü). |
| İçerik anahtarı kimliği |JWT belirtecinin yetkilendirmesini ileti talep değerini bir parçası olarak dahil edilmesi gereken (bkz [bu](media-services-axinom-integration.md#jwt-token-generation) bölümü). |

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a>İlgili kaynaklar
Bu belge oluşturma doğrultusunda katkıda bulunan aşağıdaki kişilerin onaylamak istiyoruz: Kristjan Jõgi of Axinom, Mingfei Yan, and Amit Rajput.

