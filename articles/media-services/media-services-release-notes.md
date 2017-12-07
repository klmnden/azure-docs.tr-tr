---
title: "Media Services sürüm notları | Microsoft Docs"
description: "Sürüm Notları medya Hizmetleri"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 3ca2d7af-1cf0-45fa-9585-3b73f3ee057d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: media
ms.devlang: dotnet
ms.topic: article
ms.date: 10/18/2017
ms.author: juliako
ms.openlocfilehash: 358b3701773e6cd61b4a3dfddf4bb092741ff713
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="azure-media-services-release-notes"></a>Azure Media Services sürüm notları
Bu sürüm notları değişikliklerden önceki sürümlerden ve bilinen sorunlar özetler.

> [!NOTE]
> Müşterilerimizden aldığımız duymak ve, etkileyen sorunları düzeltmeye odaklanabilirsiniz istiyoruz. Bir sorun bildirmek veya soru sormak için lütfen nakledebilirsiniz [Azure Media Services MSDN Forumu].
> 
> 

## <a id="issues"></a>Şu anda bilinen sorunlar
### <a id="general_issues"></a>Media Services genel sorunları
| Sorun | Açıklama |
| --- | --- |
| REST API birkaç ortak HTTP üst bilgilerini sağlanmadı. |REST API kullanarak Media Services uygulamalar geliştiriyorsanız, bazı ortak HTTP üstbilgi alanları Bul (CLIENT-REQUEST-ID dahil olmak üzere istek kimliği ve RETURN-CLIENT-REQUEST-ID) desteklenmez. Üstbilgileri gelecek bir güncelleştirmede eklenir. |
| Yüzde kodlama izin verilmiyor. |Media Services IAssetFile.Name özelliğinin değeri, URL akış içeriğini (örneğin, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) oluştururken kullanır. Bu nedenle, yüzde kodlama izin verilmiyor. Değeri **adı** özelliği aşağıdakilerden herhangi birini içeremez [yüzde kodlama-ayrılmış karakterleri](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ". Ayrıca, yalnızca bir olabilir '.' Dosya adı uzantısı. |
| Azure depolama SDK'sı sürüm 3.x başarısız parçası olan ListBlobs yöntemi. |Media Services oluşturur SAS göre URL'leri [2012-02-12](https://docs.microsoft.com/rest/api/storageservices/Version-2012-02-12) sürümü. Bir blob kapsayıcısında listesi BLOB'lar için Azure depolama SDK'yı kullanmak istiyorsanız, kullanmak [CloudBlobContainer.ListBlobs](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobs.aspx) Azure depolama SDK'sı sürüm parçası olmayan bir yöntemle 2.x. Parçasıdır ListBlobs yöntemi Azure depolama SDK sürümü 3.x başarısız olur. |
| Media Services mekanizması azaltma hizmete aşırı istekte uygulamalar için kaynak kullanımını kısıtlar. Hizmet, hizmet kullanılamıyor (503) HTTP durum kodu döndürebilir. |Daha fazla bilgi için bkz: 503 HTTP durum kodunu açıklaması [Azure Media Services hata kodları](media-services-encoding-error-codes.md) makalesi. |
| Varlıkları sorgulanırken ortak REST v2 1000 sonuçları için sorgu sonuçları sınırladığından aynı anda döndürülen 1000 varlıkların bir sınırı yoktur. |Kullanmanız gereken **atla** ve **ele** (.NET) / **üst** (açıklandığı gibi REST) [bu .NET örnek](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) ve [bu REST API örnek](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). |
| Bazı istemciler, kesintisiz akış bildiriminde bir yineleme etiketi sorunu arasında gelebilir. |Daha fazla bilgi için [bu](media-services-deliver-content-overview.md#known-issues) bölüme bakın. |
| Azure Media Services .NET SDK'sı nesneleri seri hale getirilemez ve sonuç olarak Azure önbelleğe alma ile çalışmaz. |Azure önbelleği için eklemek için SDK AssetCollection nesneyi serileştirmek çalışırsanız, bir özel durum oluşur. |


## <a id="rest_version_history"></a>REST API sürümü geçmişi
Media Services REST API sürümü geçmişi hakkında daha fazla bilgi için bkz: [Azure Media Services REST API Başvurusu].

## <a name="october-2017-release"></a>Ekim 2017 sürüm
> [!IMPORTANT] 
> Anımsatıcı: Azure Media Services ACS kimlik doğrulama anahtarları desteği onaysız kılınmadan zordur.  1 Haziran 2018, artık ACS anahtarları kullanarak kod aracılığıyla AMS arka ucu ile kimlik doğrulaması için siz olacaksınız. Makale Azure Active Directory (AAD) kullanmak için kodunuzu güncelleştirin [Azure Active Directory (Azure AD)-tabanlı kimlik doğrulaması](media-services-use-aad-auth-to-access-ams-api.md). Ayrıca, bu değişikliği Azure portalında uyarılar alırsınız.

### <a name="updates-for-october-2017-include"></a>Ekim 2017 için güncelleştirmeleri içerir:
#### <a name="sdks"></a>SDK’lar
* .NET SDK AAD kimlik doğrulamayı destekleyecek şekilde güncelleştirilmiştir.  AAD daha hızlı geçiş teşvik eden Nuget.org üzerindeki en son .NET SDK biz ACS kimlik doğrulaması desteğini kaldırdınız. 
* JAVA SDK AAD kimlik doğrulamayı destekleyecek şekilde güncelleştirilmiştir.  Bizim Java SDK'sına AAD kimlik doğrulaması için destek ekledik. Java SDK'yı makalesinde AMS ile kullanma hakkında ayrıntılar okuyabilirsiniz [Azure Media Services için Java istemcisi SDK ile çalışmaya başlama](media-services-java-how-to-use.md)

#### <a name="file-based-encoding"></a>Dosya tabanlı kodlaması
1.  Premium Kodlayıcı artık içeriğinize H.265(HEVC) video codec kodlamak için de kullanabilirsiniz. H.264 gibi diğer codec bileşenleri üzerinden H.265 seçmeye yönelik fiyatlandırma üzerinde etkisi yoktur. Lütfen [çevrimiçi Hizmet Koşulları'nı](https://azure.microsoft.com/support/legal/) HEVC ilgili önemli bir not için patent lisansları.
2.  İOS11 veya GoPro kahramanı 6 kullanılarak yakalanan görüntü gibi H.265(HEVC) video codec ile kodlanmış kaynak video varsa bu videolarınızı kodlamak için Premium Kodlayıcı veya standart Kodlayıcı şimdi kullanabilirsiniz. Lütfen [çevrimiçi Hizmet Koşulları'nı](https://azure.microsoft.com/support/legal/) patent lisansları hakkında önemli bir not için.
3.  Ardından dil değerler doğru (örneğin, ISO MP4) karşılık gelen dosya biçimi belirtimlerine göre etiketlenir sürece içeren birden çok dil ses izleri, ardından içeriğiniz varsa, bu içerik için kodlanması için standart kodlayıcı kullanabilirsiniz Akış. Sonuç akış Bulucusu kullanılabilir ses dilleri listeler.
4.  Standart Kodlayıcı şimdi iki yeni salt ses sistem önayarlarını, "AAC ses" ve "AAC iyi kaliteli ses" destekler. Her ikisi de sırasıyla 128 Kb/sn ve 192 kbps bit hızlarında stereo AAC çıktı üretir.
5.  Görüntü codec birini olduğu sürece Premium Kodlayıcı QuickTime/MOV dosya biçimlerini artık giriş olarak destekler. [Apple ProRes özellikleri listelenen burada](https://docs.microsoft.com/en-us/azure/media-services/media-services-media-encoder-standard-formats), ve ses AAC ya da PCM değil.

> [!NOTE]
> Premium Kodlayıcı, örneğin, giriş olarak QuickTime/MOV dosyalarında Sarmalanan DVC/DVCPro videosunu desteklemiyor.  Ancak, standart Kodlayıcı bu görüntü codec bileşenleri destekler.
>
>

6.  Kodlayıcılar içinde hata düzeltmeleri:
    * Artık bir giriş varlık kullanarak iş göndermek ve bu tamamlandıktan sonra (örneğin göre varlık içindeki dosyaların ekleme/silme/yeniden adlandırma) varlık değiştirebilir ve ek göndermek. 
    * Standart Kodlayıcı tarafından üretilen JPEG küçük resimleri geliştirilmiş kalitesi
    * Kısa süreli videolar için standart Kodlayıcı geliştirmeleri. Daha iyi işleme giriş meta verileri ve çok kısa süre videolar küçük resim oluşturma.
    * Standart Kodlayıcı kullanılan H.264 decoder geliştirmeleri belirli nadir yapılarını ortadan kaldırır. 

#### <a name="media-analytics"></a>Medya Analizi
* Azure Media Redactor - GA bu medya işlemci (MP) anonymization seçili kişiler yüzeyleri Bulanıklaştırma yoluyla gerçekleştirir ve ortak güvenlik ve haber medya senaryolarda kullanım için idealdir. Bu yeni işlemci genel bir bakış için blog gönderisine bakın [burada](https://azure.microsoft.com/blog/azure-media-redactor/). Ayrıntılı belgeler ve ayarlar için bkz: [Redaksiyon yüzeyleri Azure medya Analizi ile](media-services-face-redaction.md).



## <a name="june-2017-release"></a>Haziran 2017 sürüm

Media Services destekler [Azure Active Directory (Azure AD)-tabanlı kimlik doğrulaması](media-services-use-aad-auth-to-access-ams-api.md).

> [!IMPORTANT]
> Şu anda, Media Services, Azure erişim denetimi hizmeti kimlik doğrulama modelini destekler. Ancak, erişim denetimi yetkilendirme 1 Haziran 2018 kullanım dışı kalacaktır. Azure AD kimlik doğrulaması modeline mümkün olan en kısa sürede geçiş yapmanız önerilir.

## <a name="march-2017-release"></a>Mart 2017 sürüm

Artık Azure medya standart kullanabilirsiniz [bit hızı Merdiveni otomatik oluşturma](media-services-autogen-bitrate-ladder-with-mes.md) "Uyarlamalı akış" Önayar dize kodlama bir görev oluştururken belirterek. "Uyarlamalı akış" Media Services ile akış için video kodlamak istediğiniz önerilen hazır olur. Belirli senaryonuz için önceden belirlenmiş bir kodlama özelleştirmeniz gerekiyorsa, ile başlayabilirsiniz [bu](media-services-mes-presets-overview.md) hazır.

Artık Azure medya standart veya Medya Kodlayıcısı Premium akışına kullanabilir [fMP4 öbekleri oluşturan bir kodlama görev oluşturma](media-services-generate-fmp4-chunks.md). 


## <a name="february-2017-release"></a>Şubat 2017 sürüm

1 Nisan 2017’den itibaren, hesabınızdaki 90 günden eski olan tüm İş kayıtları, toplam kayıt sayısı üst kota sınırının altında olsa bile ilişkili Görev kayıtlarıyla birlikte otomatik olarak silinecektir. İş/görev bilgilerini arşivlemeniz gerekiyorsa, [burada](media-services-dotnet-manage-entities.md) açıklanan kodu kullanabilirsiniz.

## <a name="january-2017-release"></a>Ocak 2017 sürüm

Microsoft Azure Media Services (AMS) içinde bir **akış uç noktası** istemci oynatıcı uygulaması için doğrudan veya bir içerik teslim ağı (CDN) için daha fazla dağıtım için içerik ileten bir akış hizmetini temsil eder. Media Services, ayrıca Azure CDN entegrasyon sağlar. Giden akış StreamingEndpoint hizmetinden bir canlı akış, isteğe bağlı veya aşamalı indirme Media Services hesabınızda, varlık, bir video olabilir. Her Azure Media Services hesabı varsayılan StreamingEndpoint içerir. Ek akış hesabı altında oluşturulabilir. Akış, 1.0 ve 2. 0'ın iki sürümü vardır. Yeni oluşturulan tüm AMS 10 Ocak 2017'ile başlayan hesapları sürüm 2.0 içerecektir **varsayılan** StreamingEndpoint. Bu hesaba eklediğiniz ek akış uç noktalarını de sürüm 2.0. Bu değişiklik, var olan hesapları etkilemez; Varolan akış sürüm 1.0 ve 2.0 sürümüne yükseltilebilir. Bu değişiklikle olacaktır davranışı, faturalama ve özellik değişiklikleri (daha fazla bilgi için bkz: [bu](media-services-streaming-endpoints-overview.md) makale).

Ayrıca, 2,15 sürümünden başlayarak, Azure Media Services aşağıdaki özellikleri akış uç noktası varlığa eklenen: **CdnProvider**, **CdnProfile**, **FreeTrialEndTime** , **StreamingEndpointVersion**. Bu özellikleri ayrıntılı bakış için bkz: [bu](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). 

## <a name="december-2016-release"></a>Aralık 2016 sürüm

Azure Media Services şimdi hizmetlerinin telemetri/ölçümleri verilerine erişmek etkinleştirir. AMS geçerli sürümü, Canlı kanal, StreamingEndpoint, telemetri verilerini toplamak ve Arşiv varlıklar Canlı sağlar. Daha fazla bilgi için bkz: [bu](media-services-telemetry-overview.md) makalesi.

## <a id="july_changes16"></a>Temmuz 2016 sürüm
### <a name="updates-to-manifest-file-ism-generated-by-encoding-tasks"></a>Bildirim dosyası için güncelleştirmeleri (*. ISM) oluşturulan görevleri kodlama
Bir kodlama görev Medya Kodlayıcısı standart veya Azure medya Kodlayıcı gönderildiğinde kodlama görev oluşturur bir [akış bildirim dosyası](media-services-deliver-content-overview.md) (* .ism) çıktı dosyasında varlık. En son hizmet sürümle birlikte, bu akış bildirim dosyasının söz dizimi güncelleştirildi.

> [!NOTE]
> Akış bildirimi (.ism) dosyasının söz dizimi iç kullanım için ayrılmıştır ve sonraki sürümlerde değiştirilebilir. Değiştirmeyin veya bu dosyanın içeriğini yönlendirebilir.
> 
> 

### <a name="a-new-client-manifest-ismc-file-is-generated-in-the-output-asset-when-an-encoding-task-outputs-one-or-more-mp4-files"></a>Yeni bir istemci bildirimi (*. ISMC) dosya çıktıda oluşturulan bir veya daha fazla MP4 dosyaları bir kodlama görev çıktısını alır, varlık
Daha fazla MP4 dosyası, çıktı bir oluşturan bir kodlama görev tamamlandıktan sonra en son hizmet sürümünden başlayarak varlık da akış istemci bildirimi (*.ismc) dosyası içerir. .İsmc dosya dinamik akış performansını artırmaya yardımcı olur. 

> [!NOTE]
> İstemci bildirimi (.ismc) dosyasının söz dizimi iç kullanım için ayrılmıştır ve sonraki sürümlerde değiştirilebilir. Değiştirmeyin veya bu dosyanın içeriğini yönlendirebilir.
> 
> 

Daha fazla bilgi için bkz: [bu](https://blogs.msdn.microsoft.com/randomnumber/2016/07/08/encoder-changes-within-azure-media-services-now-create-ismc-file/) blogu.

### <a name="known-issues"></a>Bilinen sorunlar
Bazı istemciler, kesintisiz akış bildiriminde bir yineleme etiketi sorunu arasında gelebilir. Daha fazla bilgi için [bu](media-services-deliver-content-overview.md#known-issues) bölüme bakın.

## <a id="apr_changes16"></a>Nisan 2016 sürüm
### <a name="azure-media-analytics"></a>Azure medya analizi
Azure Media Services, Azure medya analizi için güçlü video gösterimi sunmuştur. Ayrıntılı bilgi için bkz: [Azure Media Services Analizi'ne genel bakış](media-services-analytics-overview.md).

### <a name="apple-fairplay-preview"></a>Apple FairPlay (Önizleme)
Azure Media Services artık, dinamik olarak, HTTP canlı akışı (HLS) ile Apple FairPlay içerik şifrelemenizi sağlar. FairPlay lisansları istemcilere teslim etmek için AMS lisans teslimat hizmeti de kullanabilirsiniz. [Azure medya HLS içerik korumalı Apple FairPlay ile akışı Hizmetleri'ni kullanın. daha ayrıntılı bilgi için bkz.

## <a id="feb_changes16"></a>Şubat 2016 sürüm
Azure Media Services SDK .NET (3.5.3) için en son sürümünü Widevine ilgili hata düzeltmesi içerir. Sorun: AssetDeliveryPolicy uygulanamadı yeniden kullanılabilir Widevine ile şifrelenmiş birden çok varlıklar. Bu hata düzeltmesi bir parçası olarak aşağıdaki özellikler SDK'SININ eklendi: **WidevineBaseLicenseAcquisitionUrl**.

    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
    {
        {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl,"http://testurl"},

    };

## <a id="jan_changes_16"></a>Ocak 2016 sürüm
Kodlayıcı adlarıyla karışıklığı azaltmak için ayrılan birimler kodlama yeniden adlandırıldı.

S1, S2, temel, standart ve Premium kodlama ayrılan birimler yeniden adlandırıldıktan ve S3 ayrılmış birimleri, sırasıyla.  Temel kodlama RUs bugün kullanan müşteriler S1 standart sırasında etiket Azure portalında (ve fatura), olarak görür ve Premium S2 ve S3 etiketlerini sırasıyla görürsünüz. 

## <a id="dec_changes_15"></a>Aralık 2015 sürümü

### <a name="azure-media-encoder-deprecation-announcement"></a>Azure medya Kodlayıcı kullanımdan Duyurusu

Azure medya Kodlayıcı yaklaşık olarak 12 ay Medya Kodlayıcısı standart sürümündeki başlayarak kullanım dışı kalacaktır.

### <a name="azure-sdk-for-php"></a>PHP için Azure SDK
Yeni bir sürümü Azure SDK'sı takım yayımlanan [PHP için Azure SDK](http://github.com/Azure/azure-sdk-for-php) güncelleştirmeleri ve yeni özellikleri için Microsoft Azure Media Services içeren paket. Özellikle, PHP için Azure Media Services SDK'sı en son artık destekliyor [içerik koruma](media-services-content-protection-overview.md) özellikleri: dinamik şifreleme AES ve DRM (PlayReady ve Widevine) ile birlikte ve belirteç kısıtlama olmadan. Ayrıca ölçeklendirmeyi destekler [kodlama birimleri](media-services-dotnet-encoding-units.md).

Daha fazla bilgi için bkz.

* [PHP için Microsoft Azure Media Services SDK](http://southworks.com/blog/2015/12/09/new-microsoft-azure-media-services-sdk-for-php-release-available-with-new-features-and-samples/) blogu.
* Aşağıdaki [kod örnekleri](http://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices) hızla başlamanıza yardımcı olması için:
  * **vodworkflow_aes.php**: Bu AES-128 dinamik şifreleme ve anahtar teslim hizmeti nasıl kullanılacağını gösteren bir PHP dosyasıdır. Açıklandığı .NET örnek dayanır [bu](media-services-protect-with-aes128.md) makalesi.
  * **vodworkflow_aes.php**: Bu PlayReady dinamik şifreleme ve lisans teslimat hizmetinin nasıl kullanılacağını gösteren bir PHP dosyasıdır. Açıklandığı .NET örnek dayanır [bu](media-services-protect-with-playready-widevine.md) makalesi.
  * **scale_encoding_units.php**: Bu kodlama ayrılmış birimi ölçeklendirmek üzere nasıl oluşturulduğunu gösteren bir PHP dosyasıdır.

## <a id="nov_changes_15"></a>Kasım 2015 sürüm
Azure Media Services artık bulutta Google Widevine lisans teslim hizmeti sunar. Daha fazla bilgi için bkz: [Bu duyuru blog](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/). Ayrıca bkz [Bu öğretici](media-services-protect-with-playready-widevine.md) ve [GitHub deposunu](http://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm). 

Azure Media izlemesi tarafından sağlanan Widevine lisans teslim hizmetleri önizlemede. Daha fazla bilgi için bkz: [bu blog](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/).

## <a id="oct_changes_15"></a>Ekim 2015 sürüm
Azure Media Services (AMS) artık canlı aşağıdaki veri merkezlerinde: Brezilya Güney, Hindistan Batı, Hindistan Güney ve Hindistan orta. Azure portalına artık kullanabilirsiniz [Media Service hesapları oluşturmak](media-services-portal-create-account.md) ve açıklanan çeşitli görevleri gerçekleştirmek [burada](https://azure.microsoft.com/documentation/services/media-services/). Ancak, Live Encoding bu veri merkezlerinde etkin değildir. Ayrıca, bu veri merkezlerinde Kodlamaya Ayrılan Birimlerin tüm türleri kullanılabilir değildir.

* Brezilya Güney:                                          Yalnızca Standart ve Temel Kodlamaya Ayrılan Birimler kullanılabilir
* Hindistan Batı, Hindistan Güney ve Hindistan Orta: yalnızca temel kodlamaya ayrılan birimler kullanılabilir

## <a id="september_changes_15"></a>Eylül 2015 sürüm
* AMS artık isteğe bağlı video (VOD) ve modüler Widevine DRM teknolojisi ile canlı akışlar koruma olanağı sunar. Widevine lisansları teslim yardımcı olması için aşağıdaki teslimat hizmetlerini ortakları kullanabilirsiniz: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/). Daha fazla bilgi için bkz: [bu blog](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).
  
    AssetDeliveryConfiguration’ı Widevine kullanacak şekilde yapılandırmak için [AMS .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (3.5.1 sürümünden başlayarak) veya REST API'sini kullanabilirsiniz.  
* AMS Apple ProRes videolar desteği eklendi. Artık Apple ProRes veya diğer codec bileşenleri kullanan QuickTime kaynak videoları dosyalarınız karşıya yükleyebilirsiniz. Daha fazla bilgi için bkz: [bu blog](https://azure.microsoft.com/blog/announcing-support-for-apple-prores-videos-in-azure-media-services/).
* Artık Medya Kodlayıcısı standart subclipping ve canlı arşiv ayıklama yapmak için kullanabilirsiniz. Daha fazla bilgi için bkz: [bu blog](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).
* Aşağıdaki filtre güncelleştirmeleri yapıldı: 
  
  * Apple HTTP canlı akışı (HLS) biçimi yalnızca ses filtresiyle artık kullanabilirsiniz. Bu güncelleştirme, yalnızca ses izleme belirterek kaldırmak sağlar (yalnızca ses = false) URL.
  * Varlıklarınızı filtrelerini tanımlarken, birden fazla birleştirme özelliği şimdi sahip tek bir URL (en fazla 3) filtreleri.
    
    Daha fazla bilgi için bkz: [bu](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogu.
* AMS t-çerçeveler HLS v4 artık destekler. T-çerçeve desteği İleri ve geri sarma işlemleri en iyi duruma getirir. Varsayılan olarak, tüm HLS v4 çıkışları t-çerçeve çalma listesi (EXT-X-I-FRAME-STREAM-INF) içerir.
  
    Daha fazla bilgi için bkz: [bu](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogu.

## <a id="august_changes_15"></a>Ağustos 2015 sürüm
* Azure Media Services SDK Java V0.8.0 sürüm ve yeni örnekler için kullanıma sunulmuştur. Daha fazla bilgi için bkz.
  
  * [Blog gönderisi](http://southworks.com/blog/2015/08/25/microsoft-azure-media-services-sdk-for-java-v0-8-0-released-and-new-samples-available/)
  * [Java örnekleri deposu](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
* Birden çok ses akışı desteğiyle Azure Media Player güncelleştirme. Daha fazla bilgi için bkz.
  * [Blog gönderisi](https://azure.microsoft.com/blog/2015/08/13/azure-media-player-update-with-multi-audio-stream-support/)

## <a id="july_changes_15"></a>Temmuz 2015 sürüm
* Medya Kodlayıcısı standart genel kullanıma sunuyoruz. Daha fazla bilgi için bkz: [bu blog gönderisine](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/).
  
    Medya Kodlayıcısı standart kullanan açıklanan hazır [bu](http://go.microsoft.com/fwlink/?LinkId=618336) bölümü. 4 k kodlar için bir hazır kullanırken, alması gereken **Premium** ayrılmış birim türü. Daha fazla bilgi için bkz: [ölçek kodlama nasıl](media-services-scale-media-processing-overview.md).
* Azure Media Services ve oyuncu ile gerçek zamanlı resim yazıları dinamik. Daha fazla bilgi için bkz: [bu blog gönderisine](https://azure.microsoft.com/blog/2015/07/08/live-real-time-captions-with-azure-media-services-and-player/)

### <a name="media-services-net-sdk-updates"></a>.NET SDK güncelleştirmeleri medya Hizmetleri
Azure Media Services .NET SDK'sı sürüm 3.4.0.0 sunulmuştur. Bu sürümde aşağıdaki işlevselliği eklendi:  

* Canlı arşiv uygulanan desteği. Canlı bir arşiv içeren bir varlık karşıdan yükleyemiyor.
* Dinamik filtre uygulanmış desteği.
* Depolama kapsayıcısı varlık silinirken tutmak kullanıcılara uygulanan işlevselliği.
* Kanallar ilkelerinde yeniden denemek için ilgili hata düzeltmeleri.
* Etkin **Medya Kodlayıcısı Premium iş akışı**.

## <a id="june_changes_15"></a>Haziran 2015 sürüm
### <a name="media-services-net-sdk-updates"></a>.NET SDK güncelleştirmeleri medya Hizmetleri
Azure Media Services .NET SDK'sı sürüm 3.3.0.0 sunulmuştur. Bu sürümde aşağıdaki işlevselliği eklendi:  

* Openıd Connect bulma spec desteği,
* Kimlik sağlayıcısı tarafında anahtarları rollover işlemek için destek. 

Openıd Connect bulma belge kullanıma sunan bir kimlik sağlayıcısı kullanıyorsanız (aşağıdaki sağlayıcıları gibi: Azure Active Directory, Google, Salesforce), Openıd JWT belirteci doğrulaması için imzalama anahtarları almak için Azure Media Services isteyin bulma spec bağlayın. 

Daha fazla bilgi için bkz: [kullanarak Json Web JWT ile çalışmak için anahtarları bulma spec Openıd Connect Azure Media Services kimlik doğrulama belirteci](http://gtrifonov.com/2015/06/07/using-json-web-keys-from-openid-connect-discovery-spec-to-work-with-jwt-token-authentication-in-azure-media-services/).

## <a id="may_changes_15"></a>Mayıs 2015 sürüm
Aşağıdaki yeni özellikleri tanışın:

* [Media Services ile gerçek zamanlı kodlama önizlemesi](media-services-manage-live-encoder-enabled-channels.md)
* [Dinamik bildirimi](media-services-dynamic-manifest-overview.md)
* [Azure medya Hyperlapse medya işlemcisi önizlemesi](https://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)

## <a id="april_changes_15"></a>Nisan 2015 güncelleştirmesinden
### <a name="general-media-services-updates"></a>Güncelleştirmeleri genel medya Hizmetleri
* [Azure Media Player Duyurusu](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/).
* Bir RTMP protokolüyle için yapılandırılan kanalları Media Services REST 2.10 ile başlayan birincil ile oluşturulur ve ikincil URL'leri alma. Daha fazla bilgi için bkz: [kanal yapılandırmaları alma](media-services-live-streaming-with-onprem-encoders.md#channel_input)
* Azure Media Indexer güncelleştirir
* İspanyolca dil desteği
* Yeni yapılandırma xml biçimi

Daha fazla bilgi için bkz: [bu blog](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).

### <a name="media-services-net-sdk-updates"></a>.NET SDK güncelleştirmeleri medya Hizmetleri
Azure Media Services .NET SDK sürüm 3.2.0.0 sunulmuştur.

Güncelleştirmeleri karşılıklı müşteri bazıları şunlardır:

* **Değişiklik çiğnemekten**: değiştirilen **TokenRestrictionTemplate.Issuer** ve **TokenRestrictionTemplate.Audience** dize türünde olmalıdır.
* Özel oluşturulmasıyla ilgili güncelleştirmeleri ilkeleri yeniden deneyin.
* Karşıya yükleme/dosyaları indirmek üzere ilgili hata düzeltmeleri.
* **MediaServicesCredentials** sınıfı şimdi karşı kimlik doğrulaması için birincil ve ikincil erişim denetim uç noktası kabul eder.

## <a id="march_changes_15"></a>Mart 2015 sürüm
### <a name="general-media-services-updates"></a>Güncelleştirmeleri genel medya Hizmetleri
* Media Services, artık Azure CDN tümleştirme sağlar. Tümleştirmesini desteklemek için **CdnEnabled** özelliği eklenmiş **StreamingEndpoint**.  **CdnEnabled** 2.9 sürümünden başlayarak REST API'leri ile kullanılabilir (daha fazla bilgi için bkz: [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint)).  **CdnEnabled** .NET SDK'sı 3.1.0.2 sürümünden başlayarak kullanılabilir (daha fazla bilgi için bkz: [StreamingEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.istreamingendpoint\(v=azure.10\).aspx)).
* Duyuru, **Medya Kodlayıcısı Premium iş akışı**. Daha fazla bilgi için bkz: [Azure Media Services kodlama giriş Premium](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/).

## <a id="february_changes_15"></a>Şubat 2015 sürüm
### <a name="general-media-services-updates"></a>Güncelleştirmeleri genel medya Hizmetleri
Media Services REST API sürümü 2.9 sunulmuştur. Bu sürümünden başlayarak, akış uç noktaları ile Azure CDN tümleştirme etkinleştirebilirsiniz. Daha fazla bilgi için bkz: [StreamingEndpoint](https://msdn.microsoft.com/library/dn783468.aspx).

## <a id="january_changes_15"></a>Ocak 2015 sürüm
### <a name="general-media-services-updates"></a>Güncelleştirmeleri genel medya Hizmetleri
Duyuru, genel kullanılabilirlik (GA) dinamik şifreleme ile içerik koruma. Daha fazla bilgi için bkz: [Azure Media Services Genel kullanılabilirlik DRM teknolojisi ile akış güvenliği artırır](https://azure.microsoft.com/blog/2015/01/29/azure-media-services-enhances-streaming-security-with-general-availability-of-drm-technology/).

### <a name="media-services-net-sdk-updates"></a>.NET SDK güncelleştirmeleri medya Hizmetleri
Azure Media Services .NET SDK'sı sürüm 3.1.0.1 sunulmuştur.

Bu sürüm, varsayılan Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization.TokenRestrictionTemplate Oluşturucu artık kullanılmayan olarak işaretlenmiş. Yeni Oluşturucusu TokenType bağımsız değişken olarak alır.

    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);


## <a id="december_changes_14"></a>Aralık 2014 sürümü
### <a name="general-media-services-updates"></a>Güncelleştirmeleri genel medya Hizmetleri
* Bazı güncelleştirmeleri ve yeni özellikleri Azure dizin oluşturucu medya işlemcisi eklendi. Daha fazla bilgi için bkz: [Azure Media Indexer sürüm 1.1.6.7 Sürüm Notları](https://azure.microsoft.com/blog/2014/12/03/azure-media-indexer-version-1-1-6-7-release-notes/).
* Kodlama güncelleştirmenizi sağlayan yeni bir REST API ayrılan birimleri eklenen: [EncodingReservedUnitType REST ile](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype).
* Eklenen CORS anahtar teslim hizmeti için destek.
* Yetkilendirme İlkesi seçenekleri sorgulama performans iyileştirmeleri yapıldığını.
* Çin veri merkezindeki [anahtar teslim URL'si](https://docs.microsoft.com/rest/api/media/operations/contentkey#get_delivery_service_url) müşteri sunulmuştur (diğer veri merkezlerinde olduğu gibi).
* Eklenen HLS otomatik hedef süresi. Canlı akış yaparken HLS her zaman dinamik olarak paketlenir. Varsayılan olarak, Media Services ana kare (KeyFrameInterval), Grup, resimleri – Canlı kodlayıcıdan alınan GOP olarak da adlandırılan aralığında HLS segment paketleme oranı (FragmentsPerSegment) otomatik olarak hesaplar. Daha fazla bilgi için bkz: [Azure Media Services canlı akış ile çalışma].

### <a name="media-services-net-sdk-updates"></a>.NET SDK güncelleştirmeleri medya Hizmetleri
* [Azure Media Services .NET SDK'sı](http://www.nuget.org/packages/windowsazure.mediaservices/) şimdi 3.1.0.0 sürümüdür.
* .Net SDK bağımlılığı .NET 4.5 Framework yükseltilmiştir.
* Kodlama ayrılan birimler güncelleştirmenizi sağlayan yeni bir API eklendi. Daha fazla bilgi için bkz: [güncelleştirme ayrılmış birim türü ve kodlama .NET kullanarak RUs artırma](media-services-dotnet-encoding-units.md).
* Eklenen JWT (JSON Web belirteci) belirteci kimlik doğrulaması için destek. Daha fazla bilgi için bkz: [JWT belirteci kimlik doğrulaması Azure Media Services ve dinamik şifreleme](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).
* PlayReady lisans şablonunda BeginDate ve ExpirationDate için eklenen göreli uzaklık.

## <a id="november_changes_14"></a>Kasım 2014 sürümü
* Media Services şimdi bir SSL bağlantısı üzerinden bir canlı kesintisiz akış (FMP4) içerik alma sağlar. SSL üzerinden alma için alma URL'si için HTTPS güncelleştirdiğinizden emin olun.  Şu anda AMS SSL ile özel etki alanlarını desteklemiyor.  Canlı akış hakkında daha fazla bilgi için bkz: [Azure Media Services canlı akış ile çalışma].
* Şu anda bir SSL bağlantısı üzerinden RTMP canlı akış alma olamaz.
* İçeriğinizi teslim etmek istediğiniz akış uç noktası 10 Eylül 2014 sonra oluşturduysanız yalnızca SSL üzerinden akışını sağlayabilirsiniz. Akış URL'leri 10 Eylül sonra oluşturulan akış uç noktalarını dayanır, URL "streaming.mediaservices.windows.net" (yeni biçimde) içerir. "Origin.mediaservices.windows.net" (eski biçimde) içeren akış URL'leri SSL desteklemez. URL'niz eski biçimindedir ve SSL üzerinden akış yapabilmek istiyorsanız [yeni bir akış uç noktası oluşturma](media-services-portal-manage-streaming-endpoints.md). SSL üzerinden içeriğinizin akışını sağlamak için yeni akış uç noktasında göre oluşturulan URL kullanın.

## <a id="october_changes_14"></a>Ekim 2014 sürümü
### <a id="new_encoder_release"></a>Medya Kodlayıcısı yayın Hizmetleri
Media Services Azure Medya Kodlayıcısı yeni sürümü sunuyoruz. En son Azure medya Kodlayıcı ile yalnızca için ücretlendirilirsiniz GB çıktı ancak Aksi halde yeni Kodlayıcı önceki encoder ile uyumlu bir özelliktir. Daha fazla bilgi için [Media Services fiyatlandırma ayrıntıları]).

### <a id="oct_sdk"></a>Media Services .NET SDK'sı
.NET uzantıları için Media Services SDK'sı sürüm 2.0.0.3 sunulmuştur.

.NET için Media Services SDK'sı sürüm 3.0.0.8 sunulmuştur.

Aşağıdaki değişiklikler yapıldı:

* Yeniden deneme ilkesi sınıfları yeniden düzenleme.
* Http istek üstbilgilerinin ekleme kullanıcı aracısı dizesi.
* NuGet restore derleme adımı ekleme.
* Senaryo testleri x509 kullanmak için düzeltme depodan sertifika.
* Kanal güncelleştirme ve akış sonlandırdığınızda, ayarlar doğrulanıyor.

### <a name="new-github-repository-to-host-media-services-samples"></a>Ana bilgisayar Media Services örnekleri için yeni GitHub deposunu
Örnekleri bulunur [Azure Media Services örnekleri GitHub deposunu](https://github.com/Azure/Azure-Media-Services-Samples).

## <a id="september_changes_14"></a>Eylül 2014 sürümü
Media Services REST meta veri sürümü 2.7 sunulmuştur. En son KALAN güncelleştirmeleri hakkında daha fazla bilgi için bkz: [Azure Media Services REST API Başvurusu].

.NET için Media Services SDK'sı sürüm 3.0.0.7 sunulmuştur

### <a id="sept_14_breaking_changes"></a>Yeni değişiklikler
* **Kaynak** yeniden adlandırıldı [StreamingEndpoint].
* Kullanırken varsayılan davranış değişikliği **Azure portal** kodlamak ve MP4 dosyaları yayımlayın.

Daha önce ne zaman tek dosya MP4 varlığı bir SAS URL'si yayımlamak için Klasik Azure Portalı'nı kullanarak oluşturulacaktır (SAS URL'leri izin, bir blob depolama alanından video indirmek). Şu anda, kodlama ve tek dosya MP4 varlığı yayımlamak için Klasik Azure Portalı'nı kullandığınızda, oluşturulan URL akış uç noktası bir Azure Media Services işaret eder.  Bu değişiklik, doğrudan Media Services'e ve Azure Media Services tarafından kodlanmış olmadan yayımlanan MP4 videolar etkilemez.

Şu anda, sorunu çözmek için aşağıdaki iki seçeneğiniz vardır.

* Akış birimleri etkinleştirin ve dinamik paketleme kesintisiz akış sunu olarak .mp4 varlığı akışla aktarmak için kullanın.
* .mp4 indirin (veya aşamalı olarak yürütmek için) bir SAS URL'si oluşturun. Bir SAS Bulucu oluşturma hakkında daha fazla bilgi için bkz: [teslim içerik].

### <a id="sept_14_GA_changes"></a>Yeni özellikler/GA sürümü parçası olan senaryolar
* **Dizin Oluşturucu medya işlemcisi**. Daha fazla bilgi için bkz: [ortam dosyalarıyla dizin Azure Media Indexer].
* [StreamingEndpoint] varlık şimdi (ana) özel etki alanı adlarını Ekle olanak sağlar.
  
    Özel etki alanı adı Media Services akış uç nokta adı olarak kullanılacak bir akış uç noktanız için özel ana bilgisayar adlarını eklemeniz gerekir. Özel ana bilgisayar adları eklemek için Media Services REST API'leri veya .NET SDK'yı kullanın.
  
    Aşağıdaki maddeler geçerlidir:
  
  * Özel etki alanı adı sahipliğini olması gerekir.
  * Etki alanı adı sahipliğini Azure Media Services tarafından doğrulanması gerekir. Etki alanını doğrulamak için eşleşen bir CName oluşturun <MediaServicesAccountId> <parent domain> DNS. < mediaservices dns bölge > doğrulanamadı. 
  * Özel ana bilgisayar adı eşlemeleri başka bir CName oluşturmanız gerekir (örn: sports.contoso.com), Media Services StreamingEndpont'ın ana bilgisayar adı (ör: amstest.streaming.mediaservices.windows.net).

    Daha fazla bilgi için bkz: **CustomHostNames** özelliğinde [StreamingEndpoint] makalesi.

### <a id="sept_14_preview_changes"></a>Yeni özellikler/genel Önizleme sürümü parçası olan senaryolar
* Canlı akış Önizleme. Daha fazla bilgi için bkz: [Azure Media Services canlı akış ile çalışma].
* Anahtar teslim hizmeti. Daha fazla bilgi için bkz: [AES-128 dinamik şifreleme kullanarak ve anahtar teslim hizmeti].
* AES dinamik şifreleme. Daha fazla bilgi için bkz: [AES-128 dinamik şifreleme kullanarak ve anahtar teslim hizmeti].
* PlayReady lisans teslimat hizmeti. Daha fazla bilgi için bkz: [dinamik şifreleme kullanarak PlayReady ve lisans teslimat hizmeti].
* PlayReady dinamik şifreleme. Daha fazla bilgi için bkz: [dinamik şifreleme kullanarak PlayReady ve lisans teslimat hizmeti].
* Medya PlayReady lisans şablonu Hizmetleri. Daha fazla bilgi için bkz: [Media Services PlayReady lisans şablonu genel bakış].
* Depolama akış varlıklarını şifrelenir. Daha fazla bilgi için bkz: [akış depolama şifrelenmiş içerik].

## <a id="august_changes_14"></a>Ağustos 2014 sürümü
Bir varlık kodlama, çıkış varlık kodlama işi tamamlandıktan sonra oluşturulur. Bu sürüm kadar Azure Media Services Encoder çıkış varlıklar hakkında meta veriler üretti. Kodlayıcı bu sürümünden başlayarak, giriş varlıklar hakkında meta veriler üretir. Daha fazla bilgi için bkz: [giriş meta veri] ve [çıkış meta veri] makaleleri.

## <a id="july_changes_14"></a>Temmuz 2014 sürümü
Azure Media Services Paketleyici ve Şifreleyici için aşağıdaki hata düzeltmeleri yapıldı:

* Yalnızca ses geri çoğullama dönüşümü Canlı arşiv varlık HTTP canlı akış – bu düzeltilmiştir ve ses ve video şimdi oynatılır yürütülür.
* Bir varlık olarak HTTP canlı akış ve AES 128 bit Zarf şifreleme paketleme, paketlenmiş akışlar Android cihazlarda kayıttan değil – Bu hatanın düzeltildiğini ve paketlenmiş akış HTTP canlı akış destek Android cihazlarda geri oynar.

## <a id="may_changes_14"></a>Mayıs 2014 sürümü
### <a id="may_14_changes"></a>Güncelleştirmeleri genel medya Hizmetleri
Artık kullanabilirsiniz [dinamik paketleme] akış HTTP canlı akışı (HLS) v3 için. HLS v3 akış, aşağıdaki biçimde Kaynak Konum Belirleyicisi yolunu eklemek için: * .ism/manifest(format=m3u8-aapl-v3). Daha fazla bilgi için bkz: [Nick Drouin'ın blogu].

Ayrıca PlayReady ile şifrelenmiş HLS (v3 hem de v4) teslim destekler statik olarak PlayReady ile şifrelenmiş kesintisiz akış dayalı artık dinamik paketleme. Kesintisiz akış PlayReady ile şifreleme hakkında daha fazla bilgi için bkz: [PlayReady ile koruma kesintisiz akış].

### <a name="may_14_donnet_changes"></a>.NET SDK güncelleştirmeleri medya Hizmetleri
Aşağıdaki geliştirmeler eklenmiştir Media Services .NET SDK'sı 3.0.0.5 sürümdeki:

* Daha iyi hızı ve karşıya yükleme/indirme ortam varlıkları esnekliği.
* Yeniden deneme mantığı ve geçici özel durum işleme geliştirmeleri: 
  
  * Geçici hata algılama ve yeniden deneme mantığı gelişmiş sorgulama, değişiklikleri kaydetmeden, karşıya yükleme veya dosyaları indirme neden özel durumlar için. 
  * Web özel durumları (örneğin, bir ACS belirteci isteği sırasında) alırken, önemli hataları daha hızlı şimdi çözümleyemiyor dikkat edin.

Daha fazla bilgi için bkz: [.NET için Media Services SDK'sını yeniden deneme mantığı].

## <a id="april_changes_14"></a>Nisan 2014 Kodlayıcı sürümü
### <a name="april_14_enocer_changes"></a>Kodlayıcı güncelleştirmeleri medya Hizmetleri
* Video hafifçe olduğu Çimen vadisi EDIUS doğrusal Düzenleyicisi kullanılarak yazılan AVI dosyaları alma için destek eklendi Çimen vadisi denetim merkezini/HQX codec kullanılarak sıkıştırılmış. Daha fazla bilgi için bkz: [Çimen vadisi bildirdi EDIUS 7 akış aracılığıyla bulut].
* Medya Kodlayıcı tarafından üretilen dosyalar için adlandırma kuralı belirtmek için destek eklendi. Daha fazla bilgi için bkz: [denetleme medya hizmeti Kodlayıcı çıkışı dosya adları].
* Video ve/veya ses yer paylaşımları desteği eklendi. Daha fazla bilgi için bkz: [oluşturma yer paylaşımları].
* Birden çok video kesimi birlikte Dikiş için destek eklendi. Daha fazla bilgi için bkz: [Dikiş Video kesimleri].
* Ses MPEG-1 ses Katman 3 (diğer adıyla MP3) burada kodlandıktan MP4s kod ilgili bir hata sabit.

## <a id="jan_feb_changes_14"></a>Ocak/Şubat 2014 sürümleri
### <a name="jan_fab_14_donnet_changes"></a>Azure Media Services .NET SDK'sı 3.0.0.1, 3.0.0.2 ve 3.0.0.3
3.0.0.1 ve 3.0.0.2 değişiklikleri şunlardır:

* OrderBy ifadelerle LINQ sorgularını kullanımını sabit sorunları.
* Test çözümlerinde bölme [GitHub] birim tabanlı testleri ve senaryo tabanlı testleri.

Değişiklikler hakkında daha fazla bilgi için bkz: [Azure Media Services .NET SDK 3.0.0.1 ve 3.0.0.2 serbest].

İçinde 3.0.0.3 aşağıdaki değişiklikler yapıldı:

* Sürüm 3.0.3.0 kullanılacak Azure depolama bağımlılıkları yükseltilmiştir. 
* Geriye dönük uyumluluk sorunu 3.0 için sabit. *.* serbest bırakır. 

## <a id="december_changes_13"></a>Aralık 2013'ün yayın
### <a name="dec_13_donnet_changes"></a>Azure Media Services .NET SDK'sı 3.0.0.0
> [!NOTE]
> 3.0.x.x sürümleri 2.4.x.x sürümleriyle geriye dönük olarak uyumlu değildir.
> 
> 

Media Services SDK'sı en son sürümünü 3.0.0.0 sunulmuştur. En son paketini Nuget'ten indirin ya da bitten alma [GitHub].

Media Services SDK sürüm 3.0.0.0 ile başlayarak, yeniden kullanabileceğiniz [Azure Active Directory erişim denetimi Hizmeti'nden (ACS)] belirteçleri. Daha fazla bilgi için "Erişim denetimi hizmeti belirteçleri yeniden kullanma" bölümüne bakın [.NET için Media Services SDK'sı ile Media Services'e bağlanma] makalesi.

### <a name="dec_13_donnet_ext_changes"></a>Azure Media Services .NET SDK uzantıları 2.0.0.0
Azure Media Services .NET SDK uzantıları, genişletme yöntemleri ve kodunuzu basitleştiren ve daha kolay Azure Media Services ile geliştirmek için yardımcı işlevleri kümesidir. En son bitten alabilirsiniz [Azure Media Services .NET SDK uzantıları].

## <a id="november_changes_13"></a>Kasım 2013 sürümü
### <a name="nov_13_donnet_changes"></a>Azure Media Services .NET SDK değişiklikleri
Bu sürümünden başlayarak, .NET için Media Services SDK'sı, Media Services REST API katmanını çağrıları yapılırken oluşabilir geçici hata hatalarını ele alır.

## <a id="august_changes_13"></a>Ağustos 2013'ün yayın
### <a name="aug_13_powershell_changes"></a>Azure SDK Araçları'nın içerdiği Media Services PowerShell cmdlet'leri
Aşağıdaki medya Hizmetleri PowerShell cmdlet'leri şimdi dahil edilen [azure sdk Araçları].

* Get-AzureMediaServices 
  
    Örneğin, `Get-AzureMediaServicesAccount`.
* New-AzureMediaServicesAccount 
  
    Örneğin, `New-AzureMediaServicesAccount -Name “MediaAccountName” -Location “Region” -StorageAccountName “StorageAccountName”`.
* New-AzureMediaServicesKey 
  
    Örneğin, `New-AzureMediaServicesKey -Name “MediaAccountName” -KeyType Secondary -Force`.
* Remove-AzureMediaServicesAccount 
  
    Örneğin, `Remove-AzureMediaServicesAccount -Name “MediaAccountName” -Force`.

## <a id="june_changes_13"></a>Haziran 2013'ün yayın
### <a name="june_13_general_changes"></a>Azure Media Services değişiklikleri
Bu bölümde belirtilen değişiklikleri Haziran 2013 Media Services sürümlerinde bulunan güncelleştirmelerdir.

* Birden çok depolama hesabı bir medya hizmeti hesabına bağlamak yeteneği. 
  
    StorageAccount
  
    Asset.StorageAccountName ve Asset.StorageAccount
* Job.Priority güncelleştirme özelliği. 
* Bildirim ilgili varlıklar ve özellikleri: 
  
    JobNotificationSubscription
  
    NotificationEndPoint
  
    İş
* Asset.Uri 
* Locator.Name 

### <a name="june_13_dotnet_changes"></a>Azure Media Services .NET SDK'sı değişiklikleri
Aşağıdaki değişiklikler dahil edilen Haziran 2013'te Media Services SDK'sı serbest bırakır. En son Media Services SDK'sı, GitHub üzerinde kullanılabilir.

* 2.3.0.0 sürümünden başlayarak, birden çok depolama bağlama Media Services SDK'sı destekler hesapları bir Media Services hesabı. Bu özellik aşağıdaki API'leri destekler:
  
    IStorageAccount türü.
  
    Microsoft.WindowsAzure.MediaServices.Client.CloudMediaContext.StorageAccounts özelliği.
  
    StorageAccount özelliği.
  
    StorageAccountName özelliği.
  
    Daha fazla bilgi için bkz: [yönetme Media Services varlıklar birden çok depolama hesapları arasında].
* Bildirim ilgili API'ler. Azure kuyruk depolama bildirimleri göndermek için dinleme olanağına sahip sürümle 2.2.0.0 başlatılıyor. Daha fazla bilgi için bkz: [Media Services işi bildirimlerini işleme].
  
    Microsoft.WindowsAzure.MediaServices.Client.IJob.JobNotificationSubscriptions özelliği.
  
    Microsoft.WindowsAzure.MediaServices.Client.INotificationEndPoint türü.
  
    Microsoft.WindowsAzure.MediaServices.Client.IJobNotificationSubscription türü.
  
    Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointCollection türü.
  
    Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointType türü.
  
    Microsoft.WindowsAzure.MediaServices.Client.NotificationJobState türü.
* Azure Storage istemci SDK 2.0 (Microsoft.WindowsAzure.StorageClient.dll) bağımlılığı.
* OData 5.5 (Microsoft.Data.OData.dll) bağımlılığı.

## <a id="december_changes_12"></a>Kasım 2012 sürüm
### <a name="dec_12_dotnet_changes"></a>Azure Media Services .NET SDK'sı değişiklikleri
* IntelliSense: pek çok türü eksik IntelliSense belgeleri eklendi.
* Microsoft.Practices.TransientFaultHandling.Core: Burada SDK bu derleme eski bir sürümü için bir bağımlılık hala sahip bir sorun düzeltilmiştir. SDK'sı, artık bu derleme sürümüne 5.1.1209.1 başvuruyor.

Kasım 2012'de SDK bulunan sorunları giderir:

* IAsset.Locators.Count: tüm bulucular silinen sonra Bu sayaç artık doğru yeni IAsset arabirimlerde bildirilir.
* IAssetFile.ContentFileSize: Bu değer şimdi düzgün bir şekilde karşıya yükleme IAssetFile.Upload (filepath) tarafından ayarlanır.
* IAssetFile.ContentFileSize: Bu özellik artık bir varlık dosyası oluşturulurken ayarlanabilir. Daha önce salt okunurdu.
* IAssetFile.Upload (filepath): Burada bu zaman uyumlu karşıya yükleme yöntemi şu hata varlık için birden çok dosya karşıya yüklenirken atma olan bir sorun düzeltilmiştir. Hata: "Sunucu isteğin kimliğini doğrulayamadı. Authorization Üstbilgisi değeri doğru imza dahil olmak üzere biçimlendirildiğinden emin olun."
* IAssetFile.UploadAsync: Burada beşten fazla dosyaları aynı anda karşıya yüklenemedi bir sorun düzeltilmiştir.
* IAssetFile.UploadProgressChanged: Bu olay artık SDK'sı tarafından sağlanır.
* IAssetFile.DownloadAsync (dize, BlobTransferClient, ILocator, CancellationToken): Bu yöntem aşırı şimdi sağlanır.
* IAssetFile.DownloadAsync: Burada beşten fazla dosyaları aynı anda karşıdan yüklenemedi bir sorun düzeltilmiştir.
* IAssetFile.Delete(): hiçbir dosya için IAssetFile karşıya yüklediyseniz arama silme bir özel durum burada atabilir bir sorun düzeltilmiştir.
* İşleri: bir sorun olduğu bir "MP4 kesintisiz akışlara görev için" "PlayReady koruma proje şablonunu kullanarak görevle ilgili" zincirleme değil oluşturacak herhangi bir görevi hiç sabit.
* EncryptionUtils.GetCertificateFromStore(): Bu yöntem artık sertifika yapılandırma sorunlarını göre sertifikayı bulma hatası nedeniyle bir null başvuru özel durumu oluşturur.

## <a id="november_changes_12"></a>Kasım 2012 sürüm
Bu bölümde belirtilen değişiklikleri Kasım 2012'de (sürüm 2.0.0.0) bulunan güncelleştirmeleri olan SDK. Bu değişiklikler SDK sürüm yeniden yazılmıştır veya değiştirilecek Haziran 2012 Önizleme için yazılmış herhangi bir kod gerektirebilir.

* Varlıklar
  
    IAsset.Create(assetName) yalnızca varlık oluşturma işlevdir. IAsset.Create yöntem çağrısının bir parçası olarak artık dosyaları karşıya yükler. IAssetFile yüklemek için kullanın.
  
    IAsset.Publish yöntemi ve AssetState.Publish numaralandırma değeri Hizmetleri SDK'dan kaldırılmıştır. Bu değeri kullanır herhangi bir kod yazılması gerekir.
* FileInfo
  
    Bu sınıf kaldırılır ve IAssetFile tarafından değiştirilir.
  
    IAssetFiles
  
    IAssetFile FileInfo değiştirir ve farklı bir davranışı vardır. Kullanmak için ya kullanarak Media Services SDK'sı ya da Azure depolama SDK'sı bir dosyayı karşıya yükleyin ve ardından IAssetFiles nesne örneği oluşturur. Aşağıdaki IAssetFile.Upload aşırı kullanılabilir:
  
  * IAssetFile.Upload(filePath): iş parçacığı engeller ve yalnızca tek bir dosyayı karşıya yüklenirken önerilen bir zaman uyumlu yöntemi.
  * IAssetFile.UploadAsync (filePath, blobTransferClient, Bulucu, cancellationToken): zaman uyumsuz bir yöntem. Tercih edilen karşıya yükleme mekanizması budur. 
    
    Bilinen hata: cancellationToken kullanarak gerçekten iptal eder; karşıya yükleme Ancak, görevleri iptal durumunu durumlardan birini olabilir. Düzgün catch ve özel durumları işleme gerekir.
* Bulucular
  
    Kaynak özgü sürümlerinde kaldırılmıştır. SAS özgü bağlamı. Locators.CreateSasLocator (varlık, accessPolicy) kullanım dışı bırakılan veya kaldırılan İST tarafından işaretlenir Güncelleştirilmiş davranışı için yeni işlevsellik altında bulunan Bulucular bölümüne bakın.

## <a id="june_changes_12"></a>Haziran 2012 Önizleme sürümü
Aşağıdaki işlevleri SDK Kasım sürümündeki yeni.

* Varlıkları silme
  
    IAsset, IAssetFile, ILocator, IAccessPolicy, IContentKey nesneleri nesneyi düzeyinde cloudMediaContext.ObjCollection.Delete(objInstance) olan koleksiyondaki bir silme gerektirmek yerine diğer bir deyişle, IObject.Delete(), şimdi silinir.
* Bulucular
  
    Bulucular CreateLocator yöntemi kullanılarak oluşturulmalıdır ve LocatorType.SAS veya LocatorType.OnDemandOrigin enum değerleri oluşturmak istediğiniz için bağımsız değişken Bulucu türünü kullanın.
  
    Yeni özellikler kullanılabilir URI'ler içeriğiniz için elde kolaylaştırmak için Bulucular eklenmiştir. Daha fazla esneklik için gelecekteki üçüncü taraf genişletilebilirlik sağlar ve kolaylığı kullanım medya istemci uygulamaları için artırmak için bu yeniden tasarlanması Bulucuyu gerektiği.
* Zaman uyumsuz yöntem desteği
  
    Tüm yöntemleri için zaman uyumsuz desteği eklendi.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<!-- Anchors. -->

<!-- Images. -->

<!--- URLs. --->
[Azure Media Services MSDN Forumu]: http://social.msdn.microsoft.com/forums/azure/home?forum=MediaServices
[Azure Media Services REST API Başvurusu]: https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference
[Media Services fiyatlandırma ayrıntıları]: http://azure.microsoft.com/pricing/details/media-services/
[giriş meta veri]: http://msdn.microsoft.com/library/azure/dn783120.aspx
[çıkış meta veri]: http://msdn.microsoft.com/library/azure/dn783217.aspx
[teslim içerik]: http://msdn.microsoft.com/library/azure/hh973618.aspx
[ortam dosyalarıyla dizin Azure Media Indexer]: http://msdn.microsoft.com/library/azure/dn783455.aspx
[StreamingEndpoint]: http://msdn.microsoft.com/library/azure/dn783468.aspx
[Azure Media Services canlı akış ile çalışma]: http://msdn.microsoft.com/library/azure/dn783466.aspx
[AES-128 dinamik şifreleme kullanarak ve anahtar teslim hizmeti]: http://msdn.microsoft.com/library/azure/dn783457.aspx
[dinamik şifreleme kullanarak PlayReady ve lisans teslimat hizmeti]: http://msdn.microsoft.com/library/azure/dn783467.aspx
[Preview features]: http://azure.microsoft.com/services/preview/
[Media Services PlayReady lisans şablonu genel bakış]: http://msdn.microsoft.com/library/azure/dn783459.aspx
[akış depolama şifrelenmiş içerik]: http://msdn.microsoft.com/library/azure/dn783451.aspx
[Azure portal]: https://manage.windowsazure.com
[dinamik paketleme]: http://msdn.microsoft.com/library/azure/jj889436.aspx
[Nick Drouin'ın blogu]: http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/
[PlayReady ile koruma kesintisiz akış]: http://msdn.microsoft.com/library/azure/dn189154.aspx
[.NET için Media Services SDK'sını yeniden deneme mantığı]: http://msdn.microsoft.com/library/azure/dn745650.aspx
[Çimen vadisi bildirdi EDIUS 7 akış aracılığıyla bulut]: http://www.streamingmedia.com/Producer/Articles/ReadArticle.aspx?ArticleID=96351&utm_source=dlvr.it&utm_medium=twitter
[denetleme medya hizmeti Kodlayıcı çıkışı dosya adları]: http://msdn.microsoft.com/library/azure/dn303341.aspx
[oluşturma yer paylaşımları]: http://msdn.microsoft.com/library/azure/dn640496.aspx
[Dikiş Video kesimleri]: http://msdn.microsoft.com/library/azure/dn640504.aspx
[Azure Media Services .NET SDK 3.0.0.1 ve 3.0.0.2 serbest]: http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/
[Azure Active Directory erişim denetimi Hizmeti'nden (ACS)]: http://msdn.microsoft.com/library/hh147631.aspx
[.NET için Media Services SDK'sı ile Media Services'e bağlanma]: http://msdn.microsoft.com/library/azure/jj129571.aspx
[Azure Media Services .NET SDK uzantıları]: https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev
[azure sdk Araçları]: https://github.com/Azure/azure-sdk-tools
[GitHub]: https://github.com/Azure/azure-sdk-for-media-services
[yönetme Media Services varlıklar birden çok depolama hesapları arasında]: http://msdn.microsoft.com/library/azure/dn271889.aspx
[Media Services işi bildirimlerini işleme]: http://msdn.microsoft.com/library/azure/dn261241.aspx

