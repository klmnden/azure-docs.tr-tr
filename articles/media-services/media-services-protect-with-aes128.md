---
title: "AES-128 dinamik şifreleme ve anahtar teslim hizmeti kullanarak | Microsoft Docs"
description: "Microsoft Azure Media Services AES 128 bit şifreleme anahtarları ile şifrelenmiş içeriğinizi teslim etmenizi sağlar. Media Services, yetkili kullanıcıların şifreleme anahtarları sunan anahtar teslim hizmeti de sağlar. Bu konu anahtar teslim hizmeti dinamik olarak AES-128 ile şifrelemek ve nasıl kullanılacağını gösterir."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 4d2c10af-9ee0-408f-899b-33fa4c1d89b9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: juliako
ms.openlocfilehash: fd90c63baaf254f5086cbc99a2a22d61587ee365
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="using-aes-128-dynamic-encryption-and-key-delivery-service"></a>AES-128 dinamik şifreleme ve anahtar teslim hizmeti kullanma
> [!div class="op_single_selector"]
> * [.NET](media-services-protect-with-aes128.md)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 

> [!NOTE]
> Java SDK'ın en son sürümünü alın ve Java ile geliştirmeye başlamak için bkz: [Media Services için Java istemcisi SDK ile çalışmaya başlama](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use). <br/>
> Media Services için en yeni PHP SDK'yi karşıdan yüklemek için sürümünde 0.5.7 Microsoft/WindowAzure paket Ara [Packagist depo](https://packagist.org/packages/microsoft/windowsazure#v0.5.7).  

## <a name="overview"></a>Genel Bakış
> [!NOTE]
> Bu bkz [blog gönderisi](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/) AES ile bir içerik teslim etmek için şifreleme **macOS üzerinde Safari**.
> Bkz: [bu](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) , medya içerik AES şifreleme ile korumak nasıl bir genel bakış için video.
> 
> 

Microsoft Azure Media Services Http canlı-akış (HLS) ve Gelişmiş Şifreleme Standardı ((128-bit şifreleme anahtarları kullanılarak) AES ile) şifrelenmiş kesintisiz akışlara teslim etmenizi sağlar. Media Services, yetkili kullanıcıların şifreleme anahtarları sunan anahtar teslim hizmeti de sağlar. Media Services'ın bir varlık şifrelemek istiyorsanız, bir şifreleme anahtarı varlıkla ilişkilendirin ve anahtarı için yetkilendirme ilkelerini de yapılandırmanız gerekir. Bir akış player tarafından istendiğinde Media Services belirtilen anahtarı dinamik olarak içeriğinizi AES şifreleme kullanarak şifrelemek için kullanır. Akış şifresini çözmek için player anahtar anahtar teslim hizmetinden ister. Kullanıcının anahtarını almak için yetkili olup olmadığına karar vermek için anahtar için belirtilen Yetkilendirme İlkeleri hizmet değerlendirir.

Media Services, anahtar isteğinde bulunan kullanıcıların kimlik doğrulamasını yapmanın birden çok yöntemini destekler. İçerik anahtarı yetkilendirme ilkesinin bir veya daha fazla yetkilendirme kısıtlaması olabilir: açık veya belirteç kısıtlaması. Belirteç kısıtlamalı ilkenin beraberinde bir Güvenli Belirteç Hizmeti (STS) tarafından verilmiş bir belirteç bulunmalıdır. Media Services, [Basit Web Belirteçleri](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) ve [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) biçimlerindeki belirteçleri destekler. Daha fazla bilgi için bkz: [içerik anahtarının yetkilendirme ilkesini yapılandırma](media-services-protect-with-aes128.md#configure_key_auth_policy).

Dinamik şifrelemeden yararlanmak için, bir grup çoklu bit hızlı MP4 dosyası ya da çoklu bit hızlı Kesintisiz Akış kaynak dosyası içeren bir varlığınız olması gerekir. Ayrıca, (Bu makalenin sonraki bölümlerinde açıklanan) varlık teslim ilkesini yapılandırmanız gerekir. Sonra akış URL'SİNDE belirtilen biçime bağlı olarak, isteğe bağlı Akış sunucusu akışı seçtiğiniz protokolde teslim edilmesini sağlar. Sonuç olarak, yalnızca depolama tek bir depolama biçiminde dosyaları için ödeme yapmanız gerekir ve Media Services hizmeti oluşturur ve istemciden gelen isteklere göre uygun yanıtı işlevi görür.

Bu makalede, korunan medya teslim eden uygulamalar üzerinde çalışan geliştiricilere yararlı olacaktır. Makalede, böylece yalnızca yetkili istemcilerin şifreleme anahtarları alabilir anahtar teslimat hizmetinin Yetkilendirme İlkeleri ile nasıl yapılandırılacağı gösterilmektedir. Ayrıca, dinamik şifreleme kullanabilmek nasıl gösterir.


## <a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a>AES-128 dinamik şifreleme ve anahtar teslim hizmeti iş akışı

Media Services anahtar teslimat hizmeti ve dinamik şifreleme kullanarak AES ile varlıklarınızı şifrelerken gerçekleştirmeniz gereken genel adımlar verilmiştir.

1. [Bir varlık oluşturun ve dosyaları varlığa yükleme](media-services-protect-with-aes128.md#create_asset).
2. [Bit hızı Uyarlamalı MP4 kümesine dosyayı içeren varlığı kodlayın](media-services-protect-with-aes128.md#encode_asset).
3. [Bir içerik anahtarı oluşturup kodlanmış varlıkla ilişkilendirin](media-services-protect-with-aes128.md#create_contentkey). Media Services’de, içerik anahtarı varlığın şifreleme anahtarını içerir.
4. [İçerik anahtarının yetkilendirme ilkesini yapılandırma](media-services-protect-with-aes128.md#configure_key_auth_policy). İçerik anahtarının istemciye teslimi için, içerik anahtarı yetkilendirme ilkesinin tarafınızdan yapılandırılması ve istemci tarafından karşılanması gerekir.
5. [Bir varlık teslim ilkesini yapılandırın](media-services-protect-with-aes128.md#configure_asset_delivery_policy). Teslim ilkesi yapılandırması içerir: anahtar edinme URL'si ve başlatma vektörü (IV) (AES 128 şifreleme ve şifre çözme sağlanacak aynı IV gerektirir), teslim Protokolü (örneğin, MPEG DASH, HLS, kesintisiz akış veya tümü), türü dinamik şifreleme (örneğin, Zarf ya da dinamik şifreleme).

    Bir varlıktaki her bir protokole farklı birer ilke uygulayabilirsiniz. Örneğin, Kesintisiz/DASH için PlayReady şifreleme ve HLS için AES Zarfı uygulayabilirsiniz. Herhangi bir teslim ilkesinde tanımlanmayan tüm protokollerin (örneğin, protokol olarak yalnızca HLS‘yi belirten tek bir ilke ekliyorsunuz) akışla aktarılması engellenir. Bunun tek istisnası, hiçbir varlık teslim ilkesinin tanımlanmadığı durumdur. Ardından, tüm protokoller, açık bir şekilde izin verilir.

6. [Bir OnDemand Bulucu oluşturmanız](media-services-protect-with-aes128.md#create_locator) bir akış URL'si almak için.

Makale ayrıca gösterir [bir istemci uygulaması anahtar teslim hizmetinden bir anahtar nasıl isteyebilir](media-services-protect-with-aes128.md#client_request).

Tam .NET Bul [örnek](media-services-protect-with-aes128.md#example) makalenin sonunda.

Aşağıdaki görüntüde, yukarıda açıklanan iş akışı gösterilmektedir. Burada kimlik doğrulaması için belirteç kullanılmaktadır.

![AES-128 ile koruma](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

Bu makalenin geri kalanı, yukarıda açıklanan görevlerin nasıl yerine getirileceğini gösteren ayrıntılı açıklamalar, kod örnekleri ve başka konulara bağlantılar sağlamaktadır.

## <a name="current-limitations"></a>Geçerli sınırlamalar
Varlığınızın teslim ilkesini ekler veya güncelleştirirseniz, mevcut bulucuyu (varsa) silip yeni bir bulucu oluşturmanız gerekir.

## <a id="create_asset"></a>Bir varlık oluşturun ve dosyaları varlığa yükleme
Videolarınızı yönetmek, kodlamak ve akışla aktarmak için önce içeriğinizi Microsoft Azure Media Services’e yüklemeniz gerekir. Yüklenmesinin ardından içeriğiniz, sonraki işleme ve akışla aktarma faaliyetleri için güvenli bir şekilde bulutta depolanır. 

Ayrıntılı bilgi için bkz. [Media Services hesabına dosya yükleme](media-services-dotnet-upload-files.md).

## <a id="encode_asset"></a>Uyarlamalı bit hızı MP4 kümesine dosyayı içeren varlığı, kodlama
Dinamik şifreleme ile tek ihtiyacınız, bir grup çoklu bit hızlı MP4 dosyası ya da çoklu bit hızlı Kesintisiz Akış kaynak dosyası içeren bir varlık oluşturmaktır. Ardından, istekteki bildirimini veya parça, isteğe bağlı Akış belirtilen biçime bağlı sunucusu akışı seçtiğiniz protokolde almak sağlar. Bunu sonucunda, dosyaları yalnızca tek bir depolama biçiminde depolamanız ve buna göre ödeme yapmanız gerekir. Media Services hizmeti, istemciden gelen isteklere göre uygun yanıtı derler ve sunar. Daha fazla bilgi için [Dinamik Paketlemeye Genel Bakış](media-services-dynamic-packaging-overview.md) makalesine bakın.

>[!NOTE]
>AMS hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 
>
>Ayrıca, dinamik paketleme ve dinamik şifreleme kullanabilmek için varlığınız Uyarlamalı bit hızlı MP4s ya da Uyarlamalı bit hızlı kesintisiz akış dosyaları içermelidir.

Kodlama yönergeleri için bkz. [Medya Kodlayıcı Standart kullanarak bir varlık kodlama](media-services-dotnet-encode-with-media-encoder-standard.md).

## <a id="create_contentkey"></a>Bir içerik anahtarı oluşturup kodlanmış varlıkla ilişkilendirme
Media Services’de, içerik anahtarı bir varlığı şifrelerken kullanmak istediğiniz anahtarı içerir.

Ayrıntılı bilgi için bkz. [İçerik anahtarı oluşturma](media-services-dotnet-create-contentkey.md).

## <a id="configure_key_auth_policy"></a>İçerik anahtarının yetkilendirme ilkesini yapılandırma
Media Services, anahtar isteğinde bulunan kullanıcıların kimlik doğrulamasını yapmanın birden çok yöntemini destekler. Anahtarın istemciye teslimi için, içerik anahtarı yetkilendirme ilkesinin tarafınızdan yapılandırılması ve istemci (oynatıcı) tarafından karşılanması gerekir. İçerik anahtarı yetkilendirme ilkesini bir veya daha fazla yetkilendirme kısıtlamaları olabilir: açın, simge restriction ya da IP kısıtlaması.

Ayrıntılı bilgi için bkz. [İçerik Anahtarı Yetkilendirme İlkesini Yapılandırma](media-services-dotnet-configure-content-key-auth-policy.md).

## <a id="configure_asset_delivery_policy"></a>Varlık teslim ilkesini yapılandırma
Varlığınıza ilişkin teslim ilkesini yapılandırın. Varlık teslim ilkesi yapılandırmasının içerdiklerinden bazıları şunlardır:

* Anahtarı edinme URL'si. 
* Başlatma vektörü (Zarf şifreleme için kullanılacak IV). AES 128, şifreleme ve şifre çözme sağlanacak aynı IV gerektirir. 
* Varlık teslim protokolü (örneğin MPEG DASH, HLS, Kesintisiz Akış veya tümü).
* Dinamik şifreleme (örneğin, AES zarfı) türü veya dinamik şifreleme yok. 

Ayrıntılı bilgi için bkz: [varlık teslim ilkesini yapılandırma](media-services-dotnet-configure-asset-delivery-policy.md).

## <a id="create_locator"></a>Akış URL’si almak için bir OnDemand akış bulucusu oluşturma
Kullanıcı kesintisiz, DASH veya HLS akış URL'sini sağlamanız gerekir.

> [!NOTE]
> Varlığınızın teslim ilkesini ekler veya güncelleştirirseniz, mevcut bulucuyu (varsa) silip yeni bir bulucu oluşturmanız gerekir.
> 
> 

Varlık yayımlama ve akış URL'si oluşturma yönergeleri için bkz. [Akış URL'si oluşturma](media-services-deliver-streaming-content.md).

## <a name="get-a-test-token"></a>Test belirteci alma
Anahtar yetkilendirme ilkesi için kullanılan belirteç kısıtlamasına dayalı olarak bir test belirteci alın.

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);

Akışınızı test etmek için [AMS Oynatıcısı](http://amsplayer.azurewebsites.net/azuremediaplayer.html)’nı kullanabilirsiniz.

## <a id="client_request"></a>Nasıl istemciniz bir anahtar anahtar teslim hizmetinden isteyebilir miyim?
Önceki adımda, bir bildirim dosyası işaret eden URL oluşturulur. Anahtar teslim hizmetine bir istek yapmak için gerekli bilgileri akış bildirim dosyalarını ayıklamak istemci gerekir.

### <a name="manifest-files"></a>Bildirim dosyası
URL ayıklamak istemcinin gerekir (Ayrıca içeriği içeren anahtar kimliği (çocuk)) bildirim dosyası değeri. İstemci anahtar teslim hizmetinden şifreleme anahtarı alma çalışacaktır. İstemcinin IV değerini ve akış şifresini kullanım ayıklamak de gerekir. Aşağıdaki kod parçacığında gösterildiği <Protection> kesintisiz akış bildiriminin öğesi.

    <Protection>
      <ProtectionHeader SystemID="B47B251A-2409-4B42-958E-08DBAE7B4EE9">
        <ContentProtection xmlns:sea="urn:mpeg:dash:schema:sea:2012" schemeIdUri="urn:mpeg:dash:sea:2012">
          <sea:SegmentEncryption schemeIdUri="urn:mpeg:dash:sea:aes128-cbc:2013"/>
          <sea:KeySystem keySystemUri="urn:mpeg:dash:sea:keysys:http:2013"/>
          <sea:CryptoPeriod IV="0xD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7" 
                            keyUriTemplate="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
                                            kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d"/>
        </ContentProtection>
      </ProtectionHeader>
    </Protection>

HLS söz konusu olduğunda, kök bildirim segment dosyalarıyla ayrılır. 

Örneğin, kök bildirimidir: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl) ve segment dosya adlarının bir listesini içerir.

    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

Segment dosyalardan birini (örneğin, http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels(514369)/Manifest(video,format=m3u8-aapl), it should contain metin düzenleyicide açın #EXT-X-dosyanın şifrelenmiş olduğunu belirten anahtar.

    #EXTM3U
    #EXT-X-VERSION:4
    #EXT-X-ALLOW-CACHE:NO
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-TARGETDURATION:9
    #EXT-X-KEY:METHOD=AES-128,
    URI="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
         kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d",
            IV=0XD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7
    #EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
    #EXTINF:8.425708,no-desc
    Fragments(video=0,format=m3u8-aapl)
    #EXT-X-ENDLIST

>[!NOTE] 
>Yürüt planlıyorsanız, bir AES HLS Safari'de şifrelenmiş bkz [bu blog](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

### <a name="request-the-key-from-the-key-delivery-service"></a>Anahtar teslim hizmetinden anahtarı iste

Aşağıdaki kod bir anahtar teslimi (bildirimden ayıklandı) URI kullanarak Media Services anahtar teslim hizmeti bir istek göndermek nasıl gösterir ve bir belirteç (Bu makalede değil konuşun basit Web belirteçleri güvenli bir belirteci Hizmeti'nden alma hakkında).

    private byte[] GetDeliveryKey(Uri keyDeliveryUri, string token)
    {
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(keyDeliveryUri);

        request.Method = "POST";
        request.ContentType = "text/xml";
        if (!string.IsNullOrEmpty(token))
        {
            request.Headers[AuthorizationHeader] = token;
        }
        request.ContentLength = 0;

        var response = request.GetResponse();

        var stream = response.GetResponseStream();
        if (stream == null)
        {
            throw new NullReferenceException("Response stream is null");
        }

        var buffer = new byte[256];
        var length = 0;
        while (stream.CanRead && length <= buffer.Length)
        {
            var nexByte = stream.ReadByte();
            if (nexByte == -1)
            {
                break;
            }
            buffer[length] = (byte)nexByte;
            length++;
        }
        response.Close();

        // AES keys must be exactly 16 bytes (128 bits).
        var key = new byte[length];
        Array.Copy(buffer, key, length);
        return key;
    }

## <a name="protect-your-content-with-aes-128-using-net"></a>İçeriğinizi AES-128 ile korumak .NET kullanma

### <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma

1. Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun. 
2. App.config dosyanızda tanımlanan **appSettings**’e aşağıdaki öğeleri ekleyin:

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

### <a id="example"></a>Örnek

Bu bölümde gösterilen kodu Program.cs dosyanızdaki kodun üzerine yazın.
 
>[!NOTE]
>Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Uzun süre boyunca kullanılmak için oluşturulan bulucu ilkeleri gibi aynı günleri / erişim izinlerini sürekli olarak kullanıyorsanız, aynı ilke kimliğini kullanmalısınız (karşıya yükleme olmayan ilkeler için). Daha fazla bilgi için [bu makaleye](media-services-dotnet-manage-entities.md#limit-access-policies) bakın.

Değişkenleri, giriş dosyalarınızın bulunduğu klasörlere işaret edecek şekilde güncelleştirdiğinizden emin olun.

[!code-csharp[Main](../../samples-mediaservices-encryptionaes/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs)]

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

