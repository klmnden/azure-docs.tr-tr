---
title: "PlayReady ve/veya Widevine dinamik ortak şifreleme kullanma | Microsoft Belgeleri"
description: "Microsoft Azure Media Services, Microsoft PlayReady DRM korumalı MPEG-DASH, Kesintisiz Akış ve Http Canlı Akış (HLS) akışlar sunmanıza olanak sağlar. Ayrıca, Widevine DRM ile şifrelenmiş DASH teslim etmenizi sağlar. Bu konuda, PlayReady ve Widevine DRM ile nasıl dinamik olarak şifreleme yapılacağı gösterilmektedir."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 548d1a12-e2cb-45fe-9307-4ec0320567a2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: juliako
ms.translationtype: HT
ms.sourcegitcommit: 26c07d30f9166e0e52cb396cdd0576530939e442
ms.openlocfilehash: 993e0a50e1598dd51969655c94730252a5a9dc42
ms.contentlocale: tr-tr
ms.lasthandoff: 07/19/2017

---
# <a name="using-playready-andor-widevine-dynamic-common-encryption"></a>PlayReady ve/veya Widevine dinamik ortak şifreleme kullanma

> [!div class="op_single_selector"]
> * [.NET](media-services-protect-with-drm.md)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
>
>

Microsoft Azure Media Services, [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/) korumalı MPEG-DASH, Kesintisiz Akış ve HTTP Canlı Akış (HLS) akışlar sunmanıza olanak sağlar. Ayrıca, Widevine DRM lisansına sahip şifrelenmiş DASH akışları teslim etmenizi de sağlar. PlayReady ve Widevine, Ortak Şifreleme (ISO/IEC 23001-7 CENC) belirtimi uyarınca şifrelenir. AssetDeliveryConfiguration’ı Widevine kullanacak şekilde yapılandırmak için [AMS .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (3.5.1 sürümünden başlayarak) veya REST API'sini kullanabilirsiniz.

Media Services, PlayReady ve Widevine DRM lisansları teslim etmek üzere bir hizmet sunar. Media Services, kullanıcı korumalı içeriği kayıttan yürüttüğünde PlayReady veya Widevine DRM çalışma zamanının zorlamasını istediğiniz hakları ve kısıtlamaları yapılandırmanıza olanak tanıyan API'ler de sağlar. Kullanıcı DRM korumalı bir içerik istediğinde, oynatıcı uygulaması AMS lisans hizmetinden bir lisans ister. Oynatıcı yetkiliyse AMS lisans hizmeti oynatıcıya bir lisans verir. PlayReady veya Widevine lisansı, istemci oynatıcısının içeriğin şifresini çözmek ve akışını yapmak için kullanabileceği şifre çözme anahtarını içerir.

Widevine lisansları teslim etmenize yardımcı olması için şu AMS ortaklarını da kullanabilirsiniz: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/). Daha fazla bilgi için [Axinom](media-services-axinom-integration.md) ve [castLabs](media-services-castlabs-integration.md) ile tümleştirme makalelerine bakın.

Media Services, anahtar isteğinde bulunan kullanıcıları yetkilendirmenin birden çok yöntemini destekler. İçerik anahtarı yetkilendirme ilkesinin bir veya daha fazla yetkilendirme kısıtlaması olabilir: açık veya belirteç kısıtlaması. Belirteç kısıtlamalı ilkenin beraberinde bir Güvenli Belirteç Hizmeti (STS) tarafından verilmiş bir belirteç bulunmalıdır. Media Services, [Basit Web Belirteçleri](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) ve [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) biçimlerindeki belirteçleri destekler. Daha fazla bilgi edinmek için, İçerik anahtarının yetkilendirme ilkesini yapılandırma bölümüne bakın.

Dinamik şifrelemeden yararlanmak için, bir grup çoklu bit hızlı MP4 dosyası ya da çoklu bit hızlı Kesintisiz Akış kaynak dosyası içeren bir varlığınız olması gerekir. Aynı zamanda varlığın teslim ilkelerini de yapılandırmalısınız (bu konunun ilerideki bölümlerinde açıklanmaktadır). Ardından, akış URL'sinde belirtilen biçime bağlı olarak, İsteğe Bağlı Akış sunucusu akışın seçtiğiniz protokolde teslim edilmesini sağlar. Bunun sonucunda dosyaları yalnızca tek bir depolama biçiminde depolamanız ve buna göre ödeme yapmanız gerekir. Media Services, istemciden gelen isteklere göre uygun yanıtı derler ve sunar.

Bu konu, PlayReady ve Widevine benzeri birden çok DRM ile korunan medya teslim eden uygulamalar üzerinde çalışan geliştiricilere yararlı olacaktır. Konuda, yalnızca yetkili istemcilerin PlayReady veya Widevine lisansları alabilmesini sağlamak üzere PlayReady lisans teslimat hizmetinin yetkilendirme ilkeleri ile nasıl yapılandırılacağı gösterilmektedir. Ayrıca, PlayReady veya Widevine DRM ile DASH üzerinde dinamik şifrelemenin nasıl kullanılacağı açıklanmaktadır.

>[!NOTE]
>AMS hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 

## <a name="download-sample"></a>Örnek indirme
Bu makalede açıklanan örneği [buradan](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm) indirebilirsiniz.

## <a name="configuring-dynamic-common-encryption-and-drm-license-delivery-services"></a>Dinamik Ortak Şifreleme ve DRM Lisans Teslimat Hizmetlerini Yapılandırma

Media Services lisans teslimat hizmeti ve dinamik şifreleme kullanarak PlayReady ile varlıklarınızı korurken gerçekleştirmeniz gereken genel adımlar aşağıdadır.

1. Bir varlık oluşturun ve dosyaları varlığa yükleyin.
2. Dosyayı içeren varlığı, bit hızı uyarlamalı MP4 kümesine kodlayın.
3. Bir içerik anahtarı oluşturup kodlanmış varlıkla ilişkilendirin. Media Services’de, içerik anahtarı varlığın şifreleme anahtarını içerir.
4. İçerik anahtarının yetkilendirme ilkesini yapılandırın. İçerik anahtarının istemciye teslimi için, içerik anahtarı yetkilendirme ilkesinin tarafınızdan yapılandırılması ve istemci tarafından karşılanması gerekir.

    İçerik anahtarı yetkilendirme ilkesini oluştururken şunları belirtmeniz gerekir: teslim yöntemi (PlayReady veya Widevine), kısıtlamalar (açık veya belirteç) ve anahtarın istemciye nasıl teslim edildiğini tanımlayan anahtar teslim türüne özgü bilgiler ([PlayReady](media-services-playready-license-template-overview.md) veya [Widevine](media-services-widevine-license-template-overview.md) lisans şablonu).

5. Varlıklara ilişkin teslim ilkesini yapılandırın. Teslim ilkesi yapılandırması şunları içerir: teslim protokolü (örneğin MPEG DASH, HLS, Kesintisiz Akış veya tümü), dinamik şifreleme türü (Ortak Şifreleme gibi), PlayReady veya Widevine lisans edinme URL'si.

    Bir varlıktaki her bir protokole farklı birer ilke uygulayabilirsiniz. Örneğin, Kesintisiz/DASH için PlayReady şifreleme ve HLS için AES Zarfı uygulayabilirsiniz. Herhangi bir teslim ilkesinde tanımlanmayan tüm protokollerin (örneğin, protokol olarak yalnızca HLS‘yi belirten tek bir ilke ekliyorsunuz) akışla aktarılması engellenir. Bunun tek istisnası, hiçbir varlık teslim ilkesinin tanımlanmadığı durumdur. Bu halde tüm protokollere açık bir şekilde izin verilir.

6. Akış URL’si almak için bir OnDemand bulucu oluşturun.

Konunun sonunda eksiksiz bir .NET örneği bulabilirsiniz.

Aşağıdaki görüntüde, yukarıda açıklanan iş akışı gösterilmektedir. Burada kimlik doğrulaması için belirteç kullanılmaktadır.

![PlayReady ile koruma](./media/media-services-content-protection-overview/media-services-content-protection-with-drm.png)

Bu konunun geri kalanı, yukarıda açıklanan görevlerin nasıl yerine getirileceğini gösteren ayrıntılı açıklamalar, kod örnekleri ve başka konulara bağlantılar sağlamaktadır.

## <a name="current-limitations"></a>Geçerli sınırlamalar
Bir varlık teslim ilkesi ekler veya ilkeyi güncelleştirirseniz, ilişkili bulucuyu (varsa) silip yeni bir bulucu oluşturmanız gerekir.

Azure Media Services aracılığıyla Widevine ile şifrelerken sınırlama: Şu anda, birden çok içerik anahtarı desteklenmemektedir.

## <a name="create-an-asset-and-upload-files-into-the-asset"></a>Varlık oluşturma ve dosyaları varlığa yükleme
Videolarınızı yönetmek, kodlamak ve akışla aktarmak için önce içeriğinizi Microsoft Azure Media Services’e yüklemeniz gerekir. Yüklenmesinin ardından içeriğiniz, sonraki işleme ve akışla aktarma faaliyetleri için güvenli bir şekilde bulutta depolanır.

Ayrıntılı bilgi için bkz. [Media Services hesabına dosya yükleme](media-services-dotnet-upload-files.md).

## <a name="encode-the-asset-containing-the-file-to-the-adaptive-bitrate-mp4-set"></a>Dosyayı içeren varlığı, bit hızı uyarlamalı MP4 kümesine kodlayın.
Dinamik şifreleme ile tek ihtiyacınız, bir grup çoklu bit hızlı MP4 dosyası ya da çoklu bit hızlı Kesintisiz Akış kaynak dosyası içeren bir varlık oluşturmaktır. Ardından, bildirimde ve parça isteğindeki belirtilen biçime bağlı olarak, İsteğe Bağlı Akış sunucusu akışı seçtiğiniz protokolde almanızı sağlar. Bunu sonucunda, dosyaları yalnızca tek bir depolama biçiminde depolamanız ve buna göre ödeme yapmanız gerekir. Media Services hizmeti, istemciden gelen isteklere göre uygun yanıtı derler ve sunar. Daha fazla bilgi için [Dinamik Paketlemeye Genel Bakış](media-services-dynamic-packaging-overview.md) konusuna bakın.

Kodlama yönergeleri için bkz. [Medya Kodlayıcı Standart kullanarak bir varlık kodlama](media-services-dotnet-encode-with-media-encoder-standard.md).

## <a id="create_contentkey"></a>Bir içerik anahtarı oluşturup kodlanmış varlıkla ilişkilendirme
Media Services’de, içerik anahtarı bir varlığı şifrelerken kullanmak istediğiniz anahtarı içerir.

Ayrıntılı bilgi için bkz. [İçerik anahtarı oluşturma](media-services-dotnet-create-contentkey.md).

## <a id="configure_key_auth_policy"></a>İçerik anahtarının yetkilendirme ilkesini yapılandırma
Media Services, anahtar isteğinde bulunan kullanıcıların kimlik doğrulamasını yapmanın birden çok yöntemini destekler. Anahtarın istemciye teslimi için, içerik anahtarı yetkilendirme ilkesinin tarafınızdan yapılandırılması ve istemci (oynatıcı) tarafından karşılanması gerekir. İçerik anahtarı yetkilendirme ilkesinin bir veya daha fazla yetkilendirme kısıtlaması olabilir: açık veya belirteç kısıtlaması.

Ayrıntılı bilgi için bkz. [İçerik Anahtarı Yetkilendirme İlkesini Yapılandırma](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption).

## <a id="configure_asset_delivery_policy"></a>Varlık teslim ilkesini yapılandırma
Varlığınıza ilişkin teslim ilkesini yapılandırın. Varlık teslim ilkesi yapılandırmasının içerdiklerinden bazıları şunlardır:

* DRM lisans edinme URL'si.
* Varlık teslim protokolü (örneğin MPEG DASH, HLS, Kesintisiz Akış veya tümü).
* Dinamik şifreleme türü (bu durumda, Ortak Şifreleme).

Ayrıntılı bilgi için bkz. [Varlık teslim ilkesini yapılandırma](media-services-rest-configure-asset-delivery-policy.md).

## <a id="create_locator"></a>Akış URL’si almak için bir OnDemand akış bulucusu oluşturma
Kesintisiz, DASH veya HLS için kullanıcınıza akış URL'sini sağlamanız gerekir.

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

## <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma

1. Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun. 
2. App.config dosyanızda tanımlanan **appSettings**’e aşağıdaki öğeleri ekleyin:

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

## <a name="example"></a>Örnek

Aşağıdaki örnek, .Net için Azure Media Services SDK’sı Sürüm 3.5.2’de tanıtılan işlevleri göstermektedir (özellikle, Widevine lisans şablonu tanımlama ve Azure Media Services’den Widevine lisansı isteme yeteneği).

Bu bölümde gösterilen kodu Program.cs dosyanızdaki kodun üzerine yazın.

>[!NOTE]
>Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Uzun süre boyunca kullanılmak için oluşturulan bulucu ilkeleri gibi aynı günleri / erişim izinlerini sürekli olarak kullanıyorsanız, aynı ilke kimliğini kullanmalısınız (karşıya yükleme olmayan ilkeler için). Daha fazla bilgi için [bu](media-services-dotnet-manage-entities.md#limit-access-policies) konu başlığına bakın.

Değişkenleri, giriş dosyalarınızın bulunduğu klasörlere işaret edecek şekilde güncelleştirdiğinizden emin olun.

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Threading;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    using Microsoft.WindowsAzure.MediaServices.Client.Widevine;
    using Newtonsoft.Json;

    namespace DynamicEncryptionWithDRM
    {
        class Program
        {
        // Read values from the App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        private static readonly Uri _sampleIssuer =
            new Uri(ConfigurationManager.AppSettings["Issuer"]);
        private static readonly Uri _sampleAudience =
            new Uri(ConfigurationManager.AppSettings["Audience"]);

        // Field for service context.
        private static CloudMediaContext _context = null;

        private static readonly string _mediaFiles =
            Path.GetFullPath(@"../..\Media");

        private static readonly string _singleMP4File =
            Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            bool tokenRestriction = false;
            string tokenTemplateString = null;

            IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
            Console.WriteLine("Uploaded asset: {0}", asset.Id);

            IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
            Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);

            IContentKey key = CreateCommonTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
            Console.WriteLine("PlayReady License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));
            Console.WriteLine();

            if (tokenRestriction)
            tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
            else
            AddOpenAuthorizationPolicy(key);

            Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
            Console.WriteLine();

            CreateAssetDeliveryPolicy(encodedAsset, key);
            Console.WriteLine("Created asset delivery policy. \n");
            Console.WriteLine();

            if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
            {
            // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
            // back into a TokenRestrictionTemplate class instance.
            TokenRestrictionTemplate tokenTemplate =
                TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

            // Generate a test token based on the the data in the given TokenRestrictionTemplate.
            // Note, you need to pass the key id Guid because we specified
            // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
            Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
            string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey,
                                        DateTime.UtcNow.AddDays(365));
            Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
            Console.WriteLine();
            }

            // You can use the http://amsplayer.azurewebsites.net/azuremediaplayer.html player to test streams.
            // Note that DASH works on IE 11 (via PlayReady), Edge (via PlayReady), Chrome (via Widevine).

            string url = GetStreamingOriginLocator(encodedAsset);
            Console.WriteLine("Encrypted DASH URL: {0}/manifest(format=mpd-time-csf)", url);

            Console.ReadLine();
        }

        static public IAsset UploadFileAndCreateAsset(string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
            Console.WriteLine("File does not exist.");
            return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.None);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
        {
            var encodingPreset = "Adaptive Streaming";

            IJob job = _context.Jobs.Create(String.Format("Encoding into Mp4 {0} to {1}",
                        inputAsset.Name,
                        encodingPreset));

            var mediaProcessors =
            _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder Standard")).ToList();

            var latestMediaProcessor =
            mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();

            ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
            encodeTask.InputAssets.Add(inputAsset);
            encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset), AssetCreationOptions.StorageEncrypted);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }


        static public IContentKey CreateCommonTypeContentKey(IAsset asset)
        {

            Guid keyId = Guid.NewGuid();
            byte[] contentKey = GetRandomBuffer(16);

            IContentKey key = _context.ContentKeys.Create(
                        keyId,
                        contentKey,
                        "ContentKey",
                        ContentKeyType.CommonEncryption);

            // Associate the key with the asset.
            asset.ContentKeys.Add(key);

            return key;
        }

        static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
        {

            // Create ContentKeyAuthorizationPolicy with Open restrictions
            // and create authorization policy          

            List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                {
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Open",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                    Requirements = null
                }
                };

            // Configure PlayReady and Widevine license templates.
            string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

            string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();

            IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                restrictions, PlayReadyLicenseTemplate);

            IContentKeyAuthorizationPolicyOption WidevinePolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.Widevine,
                restrictions, WidevineLicenseTemplate);

            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common Content Key with no restrictions").
                Result;


            contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
            contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
            // Associate the content key authorization policy with the content key.
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;
        }

        public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
        {
            string tokenTemplateString = GenerateTokenRequirements();

            List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                {
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Token Authorization Policy",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                    Requirements = tokenTemplateString,
                }
                };

            // Configure PlayReady and Widevine license templates.
            string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

            string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();

            IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                restrictions, PlayReadyLicenseTemplate);

            IContentKeyAuthorizationPolicyOption WidevinePolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.Widevine,
                restrictions, WidevineLicenseTemplate);

            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common Content Key with token restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
            contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);

            // Associate the content key authorization policy with the content key
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;

            return tokenTemplateString;
        }

        static private string GenerateTokenRequirements()
        {
            TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);

            template.PrimaryVerificationKey = new SymmetricVerificationKey();
            template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();
            template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);

            return TokenRestrictionTemplateSerializer.Serialize(template);
        }

        static private string ConfigurePlayReadyLicenseTemplate()
        {
            // The following code configures PlayReady License Template using .NET classes
            // and returns the XML string.

            //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user.
            //It contains a field for a custom data string between the license server and the application
            //(may be useful for custom app logic) as well as a list of one or more license templates.
            PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();

            // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
            // to be returned to the end users.
            //It contains the data on the content key in the license and any rights or restrictions to be
            //enforced by the PlayReady DRM runtime when using the content key.
            PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
            //Configure whether the license is persistent (saved in persistent storage on the client)
            //or non-persistent (only held in memory while the player is using the license).  
            licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;

            // AllowTestDevices controls whether test devices can use the license or not.  
            // If true, the MinimumSecurityLevel property of the license
            // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
            licenseTemplate.AllowTestDevices = true;

            // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class.
            // It grants the user the ability to playback the content subject to the zero or more restrictions
            // configured in the license and on the PlayRight itself (for playback specific policy).
            // Much of the policy on the PlayRight has to do with output restrictions
            // which control the types of outputs that the content can be played over and
            // any restrictions that must be put in place when using a given output.
            // For example, if the DigitalVideoOnlyContentRestriction is enabled,
            //then the DRM runtime will only allow the video to be displayed over digital outputs
            //(analog video outputs won’t be allowed to pass the content).

            //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience.
            // If the output protections are configured too restrictive,
            // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.

            // For example:
            //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

            responseTemplate.LicenseTemplates.Add(licenseTemplate);

            return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
        }

        private static string ConfigureWidevineLicenseTemplate()
        {
            var template = new WidevineMessage
            {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                    new ContentKeySpecs
                    {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                    }
                },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
            };

            string configuration = JsonConvert.SerializeObject(template);
            return configuration;
        }

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            // Get the PlayReady license service URL.
            Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

            // GetKeyDeliveryUrl for Widevine attaches the KID to the URL.
            // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
            // The WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption
            // to append /? KID =< keyId > to the end of the url when creating the manifest.
            // As a result Widevine license acquisition URL will have KID appended twice,
            // so we need to remove the KID that in the URL when we call GetKeyDeliveryUrl.

            Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
            UriBuilder uriBuilder = new UriBuilder(widevineUrl);
            uriBuilder.Query = String.Empty;
            widevineUrl = uriBuilder.Uri;

            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
                    {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
                    {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl, widevineUrl.ToString()}

            };

            // In this case we only specify Dash streaming protocol in the delivery policy,
            // All other protocols will be blocked from streaming.
            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
            assetDeliveryPolicyConfiguration);


            // Add AssetDelivery Policy to the asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        }

        /// <summary>
        /// Gets the streaming origin locator.
        /// </summary>
        /// <param name="assets"></param>
        /// <returns></returns>
        static public string GetStreamingOriginLocator(IAsset asset)
        {

            // Get a reference to the streaming manifest file from the  
            // collection of files in the asset.

            var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                         EndsWith(".ism")).
                         FirstOrDefault();

            // Create a 30-day readonly access policy.
            IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

            // Create a locator to the streaming content on an origin.
            ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

            // Create a URL to the manifest file.
            return originLocator.Path + assetFile.Name;
        }

        static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
            ((IJob)sender).Name,
            e.CurrentState,
            DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
        }

        static private byte[] GetRandomBuffer(int length)
        {
            var returnValue = new byte[length];

            using (var rng =
            new System.Security.Cryptography.RNGCryptoServiceProvider())
            {
            rng.GetBytes(returnValue);
            }

            return returnValue;
        }
        }
    }


## <a name="next-step"></a>Sonraki adım
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca bkz.
[Çoklu DRM ve Access Control ile CENC](media-services-cenc-with-multidrm-access-control.md)

[AMS ile Widevine paketlemeyi yapılandırma](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services)

[Azure Media Services’ta Google Widevine lisans teslim hizmetleri ile tanışın](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)

