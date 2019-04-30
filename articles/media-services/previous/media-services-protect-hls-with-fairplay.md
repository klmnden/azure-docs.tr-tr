---
title: HLS Microsoft PlayReady veya Apple FairPlay - Azure ile içerik koruma | Microsoft Docs
description: Bu konu, genel bir bakış sağlar ve HTTP canlı akışı (HLS) içeriğinizi Apple FairPlay ile dinamik olarak şifrelemek için Azure Media Services'ı kullanma işlemini gösterir. Ayrıca, FairPlay lisansı istemcilere sunmak için Media Services lisans teslimat hizmeti kullanmayı gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 7763338a5c2d5ccb58fe912d9989ab5bb57d5932
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60557478"
---
# <a name="protect-your-hls-content-with-apple-fairplay-or-microsoft-playready"></a>HLS Apple FairPlay veya Microsoft PlayReady ile içerik koruma
Azure Media Services dinamik olarak şu biçimlerden kullanarak HTTP canlı akışı (HLS) içeriğinizi şifrelemenizi sağlar:  

* **Zarf AES-128 şifresiz anahtar**

    Tüm öbek kullanılarak şifrelenir **AES-128 CBC** modu. Şifre çözme akışın iOS ve OS X oyuncu tarafından yerel olarak desteklenir. Daha fazla bilgi için [AES-128 kullanılarak dinamik şifreleme ve anahtar dağıtımı hizmetiyle](media-services-protect-with-aes128.md).
* **Apple FairPlay**

    Tek video ve ses örnekleri kullanılarak şifrelenmiş **AES-128 CBC** modu. **FairPlay Streaming** (FPS), iOS ve Apple TV yerel desteğiyle, cihaz işletim sistemlerinin bütünleştirilmiştir. OS X üzerinde Safari şifreli medya Uzantıları (EME) arabirimi desteğini kullanarak FPS sağlar.
* **Microsoft PlayReady**

Aşağıdaki görüntüde **HLS + FairPlay veya PlayReady dinamik şifreleme** iş akışı.

![Dinamik şifreleme iş akışı diyagramı](./media/media-services-content-protection-overview/media-services-content-protection-with-FairPlay.png)

Bu makalede, Media Services dinamik olarak HLS içeriğinizin Apple FairPlay ile şifrelemek için kullanılacak gösterilmiştir. Ayrıca, FairPlay lisansı istemcilere sunmak için Media Services lisans teslimat hizmeti kullanmayı gösterir.

> [!NOTE]
> HLS içeriğinizin PlayReady ile şifreleme istiyorsanız, ortak bir içerik anahtarı oluşturup, varlıkla ilişkilendirme gerekir. İçerik anahtarının yetkilendirme ilkesini yapılandırma bölümünde anlatıldığı gibi etmeniz [dinamik ortak şifreleme kullanarak PlayReady](media-services-protect-with-playready-widevine.md).
>
>

## <a name="requirements-and-considerations"></a>Gereksinimleri ve konular

Media Services HLS ile FairPlay şifrelenmiş sunmak için ve FairPlay lisansları kullanırken gerekli şunlardır:

  * Bir Azure hesabı. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
  * Bir Media Services hesabı. Oluşturmak için bkz: [Azure portalını kullanarak bir Azure Media Services hesabı oluşturma](media-services-portal-create-account.md).
  * Kaydolmak için [Apple geliştirme programı](https://developer.apple.com/).
  * Apple gerektirir edinmek üzere içerik sahibi [dağıtım paketi](https://developer.apple.com/contact/fps/). Media Services ile anahtar güvenlik modülü (KSM) zaten uygulanmış ve son FPS paket isteyen durumu. Sertifika oluşturma ve uygulama gizli anahtarı (ASK) elde etmek amacıyla son FPS pakete yönergeler de vardır. ASK FairPlay yapılandırmak için kullanın.
  * Azure Media Services .NET SDK sürümü **3.6.0** veya üzeri.

Media Services anahtar teslim tarafında aşağıdakiler ayarlanmalıdır:

  * **App Cert (AC)**: Bu özel anahtarı içeren .pfx dosyasıdır. Bu dosya oluşturur ve bir parola ile şifrelemek.

       Bir anahtar teslim ilkesini yapılandırırken, parola ve Base64 biçiminde .pfx dosyası sağlamanız gerekir.

      Aşağıdaki adımları HLS için FairPlay bir .pfx sertifika dosyası oluşturmayı açıklar:

    1. OpenSSL yüklemek https://slproweb.com/products/Win32OpenSSL.html.

        FairPlay sertifika ve Apple tarafından sunulan diğer dosyaları olduğu klasöre gidin.
    2. Komut satırından aşağıdaki komutu çalıştırın. Bu, bir .pem dosyasına .cer dosyasını dönüştürür.

        "C:\OpenSSL-Win32\bin\openssl.exe" x509-der bildirmek-FairPlay.cer içinde-FairPlay out.pem çıkış
    3. Komut satırından aşağıdaki komutu çalıştırın. Bu .pem dosyası özel anahtara sahip bir .pfx dosyasına dönüştürür. .Pfx dosyası için parolayı ardından OpenSSL tarafından istenir.

        "C:\OpenSSL-Win32\bin\openssl.exe" pkcs12-dışarı aktarma - FairPlay out.pfx-inkey privatekey.pem-FairPlay out.pem - passin file:privatekey-pem-pass.txt içinde
  * **App Cert parola**: .Pfx dosyasını oluşturmak için parola.
  * **App Cert parola kimliği**: Parola, bunlar diğer Media Services anahtarları nasıl yüklemek için benzer yüklemeniz gerekir. Kullanım **ContentKeyType.FairPlayPfxPassword** medya Hizmetleri Kimliği almak için sabit listesi değeri Anahtar teslim İlkesi seçeneği kullanmak gereksinim duydukları şeyleri budur.
  * **IV**: Bu, 16 bayt rastgele bir değerdir. Varlık teslim ilkesini de IV ile eşleşmesi gerekir. IV oluşturma ve her iki yerde de yerleştirin: Varlık teslim ilkesini ve anahtar teslim İlkesi seçeneği.
  * **SORUN**: Apple Geliştirici Portalı'nı kullanarak sertifika oluşturduğunuzda, bu anahtarı alınır. Her geliştirme ekibi bir benzersiz ASK alır. ASK bir kopyasını kaydedin ve güvenli bir yerde saklayın. Daha sonra Media Services'e FairPlayAsk olarak ASK yapılandırmanız gerekir.
  * **SORUN KİMLİĞİ**: Media Services'e ASK karşıya yüklediğinizde bu kimliği elde edilir. ASK kullanarak yüklemelisiniz **ContentKeyType.FairPlayAsk** enum değeri. Sonuç olarak, medya Hizmetleri kimliği döndürülür ve ne anahtar teslim İlkesi seçeneği ayarlarken kullanılması gereken budur.

FPS istemci tarafından aşağıdakiler ayarlanmalıdır:

  * **App Cert (AC)**: Bu işletim sisteminin bazı yükü şifrelemek için kullandığı ortak anahtar içeren bir.cer/.der dosyasıdır. Media Services player tarafından gerekli olduğu için bu konuda bilmesi gerekir. Anahtar dağıtımı hizmetiyle ilişkili özel anahtarı kullanarak şifresini çözer.

FairPlay şifrelenmiş stream kayıttan yürütme için gerçek ASK ilk alın ve ardından gerçek bir sertifika oluşturun. Bu işlem, tüm üç bölümü oluşturur:

  * .DER dosya
  * .pfx dosyası
  * .pfx için parolayı

Aşağıdaki istemciler HLS ile destek **AES-128 CBC** şifreleme: Apple TV OS X, iOS Safari.

## <a name="configure-fairplay-dynamic-encryption-and-license-delivery-services"></a>FairPlay dinamik şifreleme ve lisans teslimat hizmetlerini yapılandırma
Varlıklarınızı FairPlay kullanarak Media Services lisans teslimat hizmeti ve dinamik şifreleme kullanarak koruma için genel adımlar aşağıdadır.

1. Bir varlık oluşturun ve dosyaları varlığa yükleyin.
2. Dosyayı içeren varlığı, bit hızı uyarlamalı MP4 kümesine kodlayın.
3. Bir içerik anahtarı oluşturup kodlanmış varlıkla ilişkilendirin.  
4. İçerik anahtarının yetkilendirme ilkesini yapılandırın. Aşağıdakileri belirtin:

   * Teslim yöntemi (Bu durumda, FairPlay).
   * FairPlay ilkesi seçenekleri yapılandırma. FairPlay yapılandırma hakkında daha fazla ayrıntı için bkz. **ConfigureFairPlayPolicyOptions()** aşağıdaki örnekte yöntemi.

     > [!NOTE]
     > Genellikle, yalnızca bir sertifika ve bir dizi sahip olacağından FairPlay ilkesi seçenekleri yalnızca bir kez yapılandırmak istiyorsunuz.
     >
     >
   * Kısıtlamalar (açık veya belirteç).
   * Anahtarın istemciye nasıl teslim edildiğini tanımlayan anahtar teslim türüne özgü bilgiler.
5. Varlık teslim ilkesini yapılandırın. Teslim ilkesi yapılandırması şunları içerir:

   * Teslim Protokolü (HLS).
   * Dinamik şifreleme türü (ortak CBC şifreleme).
   * Lisans edinme URL'si.

     > [!NOTE]
     > FairPlay ve başka bir Digital Rights Management (DRM) sistemi ile şifrelenmiş bir akış iletmek istiyorsanız, ayrı teslim ilkelerini yapılandırma için gerekenler:
     >
     > * Bir IAssetDeliveryPolicy Dynamic Adaptive Streaming HTTP (DASH) ile ortak şifreleme (CENC) (PlayReady ve Widevine) ve PlayReady ile kesintisiz üzerinden yapılandırmak için
     > * Başka bir IAssetDeliveryPolicy HLS için FairPlay yapılandırmak için
     >
     >
6. Akış URL’si almak için bir OnDemand bulucu oluşturun.

## <a name="use-fairplay-key-delivery-by-player-apps"></a>FairPlay anahtar teslim oynatıcı uygulamaları ile kullanma
İOS SDK'sını kullanarak oynatıcı uygulamaları geliştirebilirsiniz. FairPlay içeriğini oynatmak lisans exchange protokolünü uygulayan gerekir. Bu protokol, Apple tarafından belirtilmedi. Bu anahtar teslim istekleri göndermek nasıl her uygulamanın tercihine bağlı olur. Medya Hizmetleri FairPlay anahtar dağıtımı hizmetiyle aşağıdaki biçimde bir www-form-url kodlanmış posta iletisi gelmesini SPC bekliyor:

    spc=<Base64 encoded SPC>

> [!NOTE]
> Azure Media Player FairPlay kayıttan yürütme destekler. Bkz: [Azure Media Player belgeleri](https://amp.azure.net/libs/amp/latest/docs/index.html) daha fazla bilgi için.
>
>

## <a name="streaming-urls"></a>Akış URL'leri
Varlığınız birden çok DRM ile şifrelenmiş, şifreleme etiketi akış URL'SİNDE kullanmalısınız: (format = 'm3u8-aapl' şifreleme = 'xxx').

Aşağıdaki maddeler geçerlidir:

* Yalnızca sıfır veya bir şifreleme türü belirtilebilir.
* Şifreleme türünün yalnızca varlık için bir şifreleme uygulandı URL'nin belirtilmesi gerekmez.
* Şifreleme türü büyük küçük harfe duyarlı değil.
* Aşağıdaki şifreleme türlerini belirtilebilir:  
  * **cenc**:  Ortak şifreleme (PlayReady veya Widevine)
  * **cbcs-aapl**: FairPlay
  * **CBC**: AES zarfı şifreleme

## <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma

1. Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun. 
2. App.config dosyanızda tanımlanan **appSettings**’e aşağıdaki öğeleri ekleyin:

    ```xml
    <add key="Issuer" value="http://testissuer.com"/>
    <add key="Audience" value="urn:test"/>
    ```

## <a name="example"></a>Örnek

Aşağıdaki örnek, şifrelenmiş HLS ile FairPlay içeriğinizi teslim etmek için Media Services'ı kullanma yeteneğini gösterir. Bu işlev, sürüm 3.6.0 .NET için Azure Media Services SDK sunulmuştur. 

Bu bölümde gösterilen kodu Program.cs dosyanızdaki kodun üzerine yazın.

>[!NOTE]
>Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Uzun süre boyunca kullanılmak için oluşturulan bulucu ilkeleri gibi aynı günleri / erişim izinlerini sürekli olarak kullanıyorsanız, aynı ilke kimliğini kullanmalısınız (karşıya yükleme olmayan ilkeler için). Daha fazla bilgi için [bu makaleye](media-services-dotnet-manage-entities.md#limit-access-policies) bakın.

Değişkenleri, giriş dosyalarınızın bulunduğu klasörlere işaret edecek şekilde güncelleştirdiğinizden emin olun.

```csharp
using System;
using System.Collections.Generic;
using System.Configuration;
using System.IO;
using System.Linq;
using System.Threading;
using Microsoft.WindowsAzure.MediaServices.Client;
using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
using Microsoft.WindowsAzure.MediaServices.Client.FairPlay;
using Newtonsoft.Json;
using System.Security.Cryptography.X509Certificates;

namespace DynamicEncryptionWithFairPlay
{
    class Program
    {
        // Read values from the App.config file.
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AMSAADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["AMSRESTAPIEndpoint"];
        private static readonly string _AMSClientId =
            ConfigurationManager.AppSettings["AMSClientId"];
        private static readonly string _AMSClientSecret =
            ConfigurationManager.AppSettings["AMSClientSecret"];

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
            AzureAdTokenCredentials tokenCredentials =
                new AzureAdTokenCredentials(_AADTenantDomain,
                    new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                    AzureEnvironments.AzureCloudEnvironment);

            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            bool tokenRestriction = false;
            string tokenTemplateString = null;

            IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
            Console.WriteLine("Uploaded asset: {0}", asset.Id);

            IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
            Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);

            IContentKey key = CreateCommonCBCTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
            Console.WriteLine("FairPlay License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay));
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

                // Generate a test token based on the data in the given TokenRestrictionTemplate.
                // Note, you need to pass the key id Guid because we specified
                // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
                string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey,
                                            DateTime.UtcNow.AddDays(365));
                Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                Console.WriteLine();
            }

            string url = GetStreamingOriginLocator(encodedAsset);
            Console.WriteLine("Encrypted HLS URL: {0}/manifest(format=m3u8-aapl)", url);

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

            IJob job = _context.Jobs.Create(String.Format("Encoding {0}", inputAsset.Name));

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

        static public IContentKey CreateCommonCBCTypeContentKey(IAsset asset)
        {
            // Create HLS SAMPLE AES encryption content key
            Guid keyId = Guid.NewGuid();
            byte[] contentKey = GetRandomBuffer(16);

            IContentKey key = _context.ContentKeys.Create(
                        keyId,
                        contentKey,
                        "ContentKey",
                        ContentKeyType.CommonEncryptionCbcs);

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


            // Configure FairPlay policy option.
            string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();

            IContentKeyAuthorizationPolicyOption FairPlayPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
            ContentKeyDeliveryType.FairPlay,
            restrictions,
            FairPlayConfiguration);


            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common CBC Content Key with no restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);

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

            // Configure FairPlay policy option.
            string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();


            IContentKeyAuthorizationPolicyOption FairPlayPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                   ContentKeyDeliveryType.FairPlay,
                   restrictions,
                   FairPlayConfiguration);

            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common CBC Content Key with token restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);

            // Associate the content key authorization policy with the content key
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;

            return tokenTemplateString;
        }

        private static string ConfigureFairPlayPolicyOptions()
        {
            // For testing you can provide all zeroes for ASK bytes together with the cert from Apple FPS SDK.
            // However, for production you must use a real ASK from Apple bound to a real prod certificate.
            byte[] askBytes = Guid.NewGuid().ToByteArray();
            var askId = Guid.NewGuid();
            // Key delivery retrieves askKey by askId and uses this key to generate the response.
            IContentKey askKey = _context.ContentKeys.Create(
                        askId,
                        askBytes,
                        "askKey",
                        ContentKeyType.FairPlayASk);

            //Customer password for creating the .pfx file.
            string pfxPassword = "<customer password for creating the .pfx file>";
            // Key delivery retrieves pfxPasswordKey by pfxPasswordId and uses this key to generate the response.
            var pfxPasswordId = Guid.NewGuid();
            byte[] pfxPasswordBytes = System.Text.Encoding.UTF8.GetBytes(pfxPassword);
            IContentKey pfxPasswordKey = _context.ContentKeys.Create(
                        pfxPasswordId,
                        pfxPasswordBytes,
                        "pfxPasswordKey",
                        ContentKeyType.FairPlayPfxPassword);

            // iv - 16 bytes random value, must match the iv in the asset delivery policy.
            byte[] iv = Guid.NewGuid().ToByteArray();

            //Specify the .pfx file created by the customer.
            var appCert = new X509Certificate2("path to the .pfx file created by the customer", pfxPassword, X509KeyStorageFlags.Exportable);

            string FairPlayConfiguration =
            Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
                appCert,
                pfxPassword,
                pfxPasswordId,
                askId,
                iv);

            return FairPlayConfiguration;
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

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            var kdPolicy = _context.ContentKeyAuthorizationPolicies.Where(p => p.Id == key.AuthorizationPolicyId).Single();

            var kdOption = kdPolicy.Options.Single(o => o.KeyDeliveryType == ContentKeyDeliveryType.FairPlay);

            FairPlayConfiguration configFP = JsonConvert.DeserializeObject<FairPlayConfiguration>(kdOption.KeyDeliveryConfiguration);

            // Get the FairPlay license service URL.
            Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay);

            // The reason the below code replaces "https://" with "skd://" is because
            // in the IOS player sample code which you obtained in Apple developer account,
            // the player only recognizes a Key URL that starts with skd://.
            // However, if you are using a customized player,
            // you can choose whatever protocol you want.
            // For example, "https".

            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
                    {AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl, acquisitionUrl.ToString().Replace("https://", "skd://")},
                    {AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs, configFP.ContentEncryptionIV}
            };

            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
            AssetDeliveryProtocol.HLS,
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
```

## <a name="next-steps-media-services-learning-paths"></a>Sonraki adımlar: Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
