---
title: AES-128 dinamik şifreleme ve anahtar dağıtımı hizmetiyle | Microsoft Docs
description: İçeriğinizi AES 128 bit şifreleme anahtarları ile Microsoft Azure Media Services'ı kullanarak şifreli sunun. Media Services, şifreleme anahtarlarını yetkili kullanıcıların sunan anahtar dağıtımı hizmetiyle de sağlar. Bu konu nasıl dinamik olarak AES-128 ile şifrelemek ve anahtar dağıtımı hizmetiyle nasıl kullanılacağını gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 4d2c10af-9ee0-408f-899b-33fa4c1d89b9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: 6f76d6aed8dc5eed3dbf673b265c404f27b0536d
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59526835"
---
# <a name="use-aes-128-dynamic-encryption-and-the-key-delivery-service"></a>AES-128 dinamik şifreleme ve anahtar teslim hizmetini kullanma
> [!div class="op_single_selector"]
> * [.NET](media-services-protect-with-aes128.md)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
>  

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürüm olan [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/)’ü inceleyin. Ayrıca bkz [geçiş kılavuzuna v2'den v3](../latest/migrate-from-v2-to-v3.md)

HTTP canlı akışı (HLS) ve kesintisiz akış ile AES 128 bit şifreleme anahtarları kullanılarak şifrelenmiş sunmak için Media Services'ı kullanabilirsiniz. Media Services, şifreleme anahtarlarını yetkili kullanıcıların sunan anahtar dağıtımı hizmetiyle de sağlar. Media Services'ın bir varlık şifrelemek isterseniz, bir şifreleme anahtarı varlıkla ilişkilendirme ve anahtar için yetkilendirme ilkelerini de yapılandırmanız gerekir. Bir akışa bir oynatıcı tarafından istendiğinde Media Services dinamik olarak içeriğinizi AES şifreleme kullanarak şifrelemek için belirtilen anahtar kullanır. Oynatıcı, akışın şifresini çözmek için anahtar teslim hizmetinden anahtarı ister. Kullanıcı anahtarı almak için yetki verilip verilmediğini belirlemek için anahtar için belirtilen Yetkilendirme İlkeleri hizmet tarafından değerlendirilir.

Media Services, anahtar isteğinde bulunan kullanıcıların kimlik doğrulamasını yapmanın birden çok yöntemini destekler. İçerik anahtarı yetkilendirme ilkesinin açık veya belirteç kısıtlaması şeklinde bir veya daha fazla yetkilendirme kısıtlaması olabilir. Belirteç kısıtlamalı ilkenin beraberinde bir güvenlik belirteci hizmeti (STS) tarafından verilmiş bir belirteç bulunmalıdır. Media Services, [basit web belirteci](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) ve [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) biçimlerindeki belirteçleri destekler. Daha fazla bilgi edinmek için bkz. [İçerik anahtarının yetkilendirme ilkesini yapılandırma](media-services-protect-with-aes128.md#configure_key_auth_policy).

Dinamik şifrelemeden yararlanmak için, bir grup çoklu bit hızlı MP4 dosyası ya da çoklu bit hızlı Kesintisiz Akış kaynak dosyası içeren bir varlığınız olması gerekir. (Bu makalenin sonraki bölümlerinde açıklanmıştır) varlık teslim ilkesini yapılandırmanız gerekir. Ardından, akış URL'sinde belirtilen biçime bağlı olarak, isteğe bağlı akış sunucusu akışın seçtiğiniz protokolde teslim edilmesini sağlar. Sonuç olarak, depolama yalnızca tek bir depolama biçimindeki dosyaları buna göre ödeme yapmanız gerekir. Media Services, bir istemciden alınan isteklere göre uygun yanıtı oluşturur ve sunar.

Bu makalede, korunan medya teslim eden uygulamalar üzerinde çalışan geliştiriciler için yararlıdır. Makalede, yalnızca yetkili istemcilerin şifreleme anahtarları alabilmesi anahtar teslim hizmetinin Yetkilendirme İlkeleri ile nasıl yapılandırılacağı gösterilmektedir. Ayrıca, dinamik şifreleme kullanmayı gösterir.

İçerik ile Gelişmiş Şifreleme Standardı (AES) teslim Safari için macOS üzerinde şifreleme hakkında daha fazla bilgi için bkz: [bu blog gönderisini](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).
Medyanızı AES şifreleme ile içerik koruma genel bakış için bkz. [bu videoyu](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption).


## <a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a>AES-128 dinamik şifreleme ve anahtar teslim hizmeti iş akışı

Media Services anahtar teslim hizmeti ve dinamik şifreleme kullanarak varlıklarınızı AES şifreleme uygulanırken aşağıdaki genel adımları uygulayın:

1. [Bir varlık oluşturun ve dosyaları varlığa yükleyin](media-services-protect-with-aes128.md#create_asset).

2. [Bit hızı Uyarlamalı MP4 kümesine dosyayı içeren varlığı kodlama](media-services-protect-with-aes128.md#encode_asset).

3. [Bir içerik anahtarı oluşturup kodlanmış varlıkla ilişkilendirme](media-services-protect-with-aes128.md#create_contentkey). Media Services’de, içerik anahtarı varlığın şifreleme anahtarını içerir.

4. [İçerik anahtarının yetkilendirme ilkesini yapılandırma](media-services-protect-with-aes128.md#configure_key_auth_policy). İçerik anahtarı yetkilendirme ilkesini yapılandırmanız gerekir. İçerik anahtarının istemciye teslim edilebilmesi için önce istemcinin ilkeyi karşılaması gerekir.

5. [Bir varlık teslim ilkesini yapılandırma](media-services-protect-with-aes128.md#configure_asset_delivery_policy). Teslim ilkesi yapılandırması anahtarı edinme URL'si ve başlangıç vektörünün (IV) içerir. (AES-128 aynı IV şifreleme ve şifre çözme için gerekir.) Yapılandırma, teslim Protokolü (örneğin, MPEG-DASH, HLS, kesintisiz akış veya tümü) ve dinamik şifreleme (örneğin, zarfın ya da dinamik şifreleme) türünü de içerir.

    Bir varlıktaki her bir protokole farklı birer ilke uygulayabilirsiniz. Örneğin, Kesintisiz/DASH için PlayReady şifreleme uygularken HLS için bir AES zarfı uygulayabilirsiniz. Bir teslim ilkesinde tanımlanmayan tüm protokollerin akışla aktarılması engellenir. (Tek bir ilke eklerseniz protokol olarak yalnızca HLS belirten örnek verilebilir.) Bunun tek istisnası, hiçbir varlık teslim ilkesinin tanımlanmadığı durumdur. Bu halde tüm protokollere açık bir şekilde izin verilir.

6. [Bir OnDemand Bulucu](media-services-protect-with-aes128.md#create_locator) akış URL'si almak için.

Ayrıca makale [nasıl anahtar anahtar teslim hizmetinden bir istemci uygulaması isteyebilirsiniz](media-services-protect-with-aes128.md#client_request).

Eksiksiz bir bulabilirsiniz [.NET örnek](media-services-protect-with-aes128.md#example) makalenin sonunda.

Aşağıdaki görüntüde, daha önce açıklanan iş akışı gösterilmektedir. Burada kimlik doğrulaması için belirteç kullanılmaktadır.

![AES-128 ile koruma](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

Bu makalenin geri kalanında, açıklamalar, kod örnekleri ve daha önce açıklanan görevlerin elde etmek nasıl gösteren konulara bağlantılar sağlar.

## <a name="current-limitations"></a>Geçerli sınırlamalar
Varlığınızın teslim ilkesini ekler veya güncelleştirirseniz, varsa mevcut bulucuyu silip yeni bir bulucu oluşturmanız gerekir.

## <a id="create_asset"></a>Bir varlık oluşturun ve dosyaları varlığa yükleyin.
Videolarınızı yönetmek, kodlamak ve akışla aktarmak için önce içeriğinizi Media Services’a yüklemeniz gerekir. Dosyanın karşıya yüklenmesinin ardından içeriğiniz, sonraki işleme ve akışla aktarma faaliyetleri için güvenli bir şekilde bulutta depolanır. 

Daha fazla bilgi için bkz. [Media Services hesabına dosya yükleme](media-services-dotnet-upload-files.md).

## <a id="encode_asset"></a>Hızı Uyarlamalı MP4 kümesine dosyayı içeren varlığı kodlama
Dinamik şifreleme ile bir grup çoklu bit hızlı MP4 dosyası ya da çoklu bit hızlı Kesintisiz Akış kaynak dosyası içeren bir varlık oluşturursunuz. Ardından, bildirimi veya parça isteğindeki belirtilen biçime bağlı olarak, isteğe bağlı Akış sunucusu, akışın seçtiğiniz protokolde almanızı sağlar. Ardından, yalnızca depolama tek bir depolama biçimindeki dosyaları buna göre ödeme yapmanız gerekir. Media Services, bir istemciden alınan isteklere göre uygun yanıtı oluşturur ve sunar. Daha fazla bilgi için bkz. [Dinamik paketlemeye genel bakış](media-services-dynamic-packaging-overview.md).

>[!NOTE]
>Media Services hesabınız oluşturulduğunda hesabınıza “Durdurulmuş” durumda bir varsayılan akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının “Çalışıyor” durumda olması gerekir. 
>
>Ayrıca, dinamik paketleme ile dinamik şifrelemeden kullanmak için varlığınız bir dizi hızı Uyarlamalı MP4 veya uyarlamalı bit hızlı kesintisiz akış dosyaları içermelidir.

Kodlama yönergeleri için bkz. [Media Encoder Standard kullanarak varlık kodlama](media-services-dotnet-encode-with-media-encoder-standard.md).

## <a id="create_contentkey"></a>Bir içerik anahtarı oluşturup kodlanmış varlıkla ilişkilendirme
Media Services’de, içerik anahtarı bir varlığı şifrelerken kullanmak istediğiniz anahtarı içerir.

Daha fazla bilgi için bkz. [İçerik anahtarı oluşturma](media-services-dotnet-create-contentkey.md).

## <a id="configure_key_auth_policy"></a>İçerik anahtarının yetkilendirme ilkesini yapılandırma
Media Services, anahtar isteğinde bulunan kullanıcıların kimlik doğrulamasını yapmanın birden çok yöntemini destekler. İçerik anahtarı yetkilendirme ilkesini yapılandırmanız gerekir. Anahtarın istemciye teslim edilebilmesi için istemci (oynatıcı) ilkeyi karşılaması gerekir. İçerik anahtarı yetkilendirme ilkesinin bir veya daha fazla yetkilendirme kısıtlaması, ya da açın, kısıtlama veya IP kısıtlaması belirteci olabilir.

Daha fazla bilgi edinmek için bkz. [İçerik anahtarı yetkilendirme ilkesi yapılandırma](media-services-dotnet-configure-content-key-auth-policy.md).

## <a id="configure_asset_delivery_policy"></a>Varlık teslim ilkesi yapılandırma
Varlığınıza ilişkin teslim ilkesini yapılandırın. Varlık teslim ilkesi yapılandırmasının içerdiklerinden bazıları şunlardır:

* Anahtar edinme URL'si. 
* Başlatma vektörü (IV) Zarf şifreleme için kullanılacak. AES-128, şifreleme ve şifre çözme için aynı IV gerektirir. 
* Varlık teslim protokolü (örneğin MPEG DASH, HLS, Kesintisiz Akış veya tümü).
* Dinamik şifreleme (örneğin, AES zarfı) türü veya dinamik şifreleme yok. 

Daha fazla bilgi için bkz. [Varlık teslim ilkesi yapılandırma](media-services-dotnet-configure-asset-delivery-policy.md).

## <a id="create_locator"></a>Akış URL’si almak için bir OnDemand akış bulucusu oluşturma
Kullanıcınıza Kesintisiz Akış, DASH veya HLS için akış URL’sini sağlamanız gerekir.

> [!NOTE]
> Varlığınızın teslim ilkesini ekler veya güncelleştirirseniz, varsa mevcut bulucuyu silip yeni bir bulucu oluşturmanız gerekir.
> 
> 

Varlık yayımlama ve akış URL'si oluşturma yönergeleri için bkz. [Akış URL'si oluşturma](media-services-deliver-streaming-content.md).

## <a name="get-a-test-token"></a>Test belirteci alma
Anahtar yetkilendirme ilkesi için kullanılan belirteç kısıtlamasına dayalı olarak bir test belirteci alın.

```csharp
    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word "Bearer" in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
```

Akışınızı test etmek için [Azure Media Services Oynatıcısı](https://amsplayer.azurewebsites.net/azuremediaplayer.html)’nı kullanabilirsiniz.

## <a id="client_request"></a>Nasıl istemcinizi bir anahtarı anahtar teslim hizmetinden isteyebilir miyim?
Önceki adımda, bir bildirim dosyasına işaret eden URL oluşturulur. Anahtar teslim hizmetine bir istekte bulunmak için akış bildirim dosyaları için gerekli bilgileri ayıklamak istemcinizi gerekir.

### <a name="manifest-files"></a>Bildirim dosyaları
URL ayıklamak istemcinin gerekir (içerik de içeren anahtar kimliği [çocuk]) bildirim dosyası değeri. İstemci ardından anahtar teslim hizmetinden şifreleme anahtarı alma dener. İstemci ayrıca IV değerini ayıklayın ve akış şifresini çözmek için kullanmanız gerekir. Aşağıdaki kod parçacığında gösterildiği `<Protection>` kesintisiz akış bildirimin öğesi:

```xml
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
```

HLS söz konusu olduğunda kök bildirim segment dosyaya ayrıldığı. 

Örneğin, kök bildirimidir: http:\//test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl). Bu, segment dosya adlarının bir listesini içerir.

    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

Segment dosyalardan biri bir metin Düzenleyicisi'nde açtığınız varsa (örneğin, http:\//test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels(514369)/Manifest(video,format=m3u8-aapl), #EXT-X-dosya şifrelenmiş olduğunu gösteren KEY, içeriyor.

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
>Safari'de AES şifreli HLS yürütme planlıyorsanız bkz [bu blog](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

### <a name="request-the-key-from-the-key-delivery-service"></a>Anahtar teslim hizmeti anahtarı iste

Aşağıdaki kod bir anahtar teslim (bildiriminden ayıklanan) URI kullanarak Media Services anahtar teslim hizmeti için bir istek göndermek nasıl gösterir ve bir belirteç. (Bu makalede bir sts'yi SWTs yapılacağını açıklayan değil.)

```csharp
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
```

## <a name="protect-your-content-with-aes-128-by-using-net"></a>.NET kullanarak içeriğinizi AES-128 ile koruma

### <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma

1. Geliştirme ortamınızı ayarlayın ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun.

2. Aşağıdaki öğeleri appSettings için app.config dosyanızda tanımlanan ekleyin:

    ```xml
    <add key="Issuer" value="http://testissuer.com"/>
    <add key="Audience" value="urn:test"/>
    ```

### <a id="example"></a>Örnek

Bu bölümde gösterilen kodu Program.cs dosyanızdaki kodun üzerine yazın.
 
>[!NOTE]
>Bir sınır 1.000.000 farklı Media Services ilkeleri için (örneğin, Bulucu İlkesi veya ContentKeyAuthorizationPolicy için) yoktur. Her zaman aynı günleri/erişim izinlerini kullanıyorsanız aynı ilke Kimliğini kullanın. Örnek olarak uzun süre olduğu gibi kalması amaçlanan bulucu ilkeleri (karşıya yükleme olmayan ilkeler) verilebilir. Daha fazla bilgi için "Sınırı erişim ilkeleri" bölümüne bakın. [yönetmek, varlıkları ve Media Services .NET SDK'sı ile ilgili öğeleri](media-services-dotnet-manage-entities.md#limit-access-policies).

Değişkenleri, giriş dosyalarınızın bulunduğu klasörlere işaret edecek şekilde güncelleştirdiğinizden emin olun.

[!code-csharp[Main](../../../samples-mediaservices-encryptionaes/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs)]

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
