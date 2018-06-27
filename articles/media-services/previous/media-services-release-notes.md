---
title: Media Services sürüm notları | Microsoft Docs
description: Media Services sürüm notları
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: 3ca2d7af-1cf0-45fa-9585-3b73f3ee057d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: media
ms.devlang: dotnet
ms.topic: article
ms.date: 10/18/2017
ms.author: juliako
ms.openlocfilehash: e2512a2af05ee7101713886c3ae1b5c6c74dd3db
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37018547"
---
# <a name="azure-media-services-release-notes"></a>Azure Media Services sürüm notları
Bu sürüm notlarını Azure Media Services önceki sürümlerden ve bilinen sorunlar değişiklikleri özetlemek için.

> [!NOTE]
> Biz, etkileyen sorunları düzeltmeye odaklanabilmeniz müşterilerimizden aldığımız duymak istiyoruz. Bir sorun bildirmek veya sorular sormak için ileti gönderme [Azure Media Services MSDN Forumu].
> 
> 

## <a name="a-idissuescurrently-known-issues"></a><a id="issues"/>Şu anda bilinen sorunlar
### <a name="a-idgeneralissuesmedia-services-general-issues"></a><a id="general_issues"/>Media Services genel sorunlar

| Sorun | Açıklama |
| --- | --- |
| Birçok ortak HTTP üst bilgilerini REST API çağrısında sağlanan değil. |REST API kullanarak Media Services uygulamalar geliştiriyorsanız, bazı ortak HTTP üstbilgi alanları Bul (CLIENT-REQUEST-ID dahil olmak üzere istek kimliği ve RETURN-CLIENT-REQUEST-ID) desteklenmez. Üstbilgileri gelecek bir güncelleştirmede eklenir. |
| Yüzde kodlama izin verilmiyor. |Media Services URL'leri akış içeriğini (örneğin, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters) oluştururken IAssetFile.Name özelliğinin değeri kullanır. Bu nedenle, yüzde kodlama izin verilmiyor. Name özelliği değeri aşağıdakilerden herhangi birini içeremez [yüzde kodlama-ayrılmış karakterleri](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ". Ayrıca, yalnızca bir olabilir "." dosya adı uzantısı için. |
| Azure depolama SDK'sı sürüm 3.x başarısız parçası olan ListBlobs yöntemi. |Media Services oluşturur SAS göre URL'leri [2012-02-12](https://docs.microsoft.com/rest/api/storageservices/Version-2012-02-12) sürümü. Bir blob kapsayıcısında listesi BLOB'lar için depolama SDK'yı kullanmak istiyorsanız, kullanmak [CloudBlobContainer.ListBlobs](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobs.aspx) depolama SDK'sı sürüm parçası olmayan bir yöntemle 2.x. |
| Mekanizması azaltma Media Services hizmetine aşırı isteklerde uygulamalar için kaynak kullanımını kısıtlar. Hizmet, "Hizmet kullanılamıyor" 503 HTTP durum kodu döndürebilir. |Daha fazla bilgi için bkz: 503 HTTP durum kodunu açıklaması [Media Services hata kodları](media-services-encoding-error-codes.md). |
| Varlıkları sorguladığınızda, ortak REST sürüm 2 1.000 sonuçları için sorgu sonuçları sınırladığından 1.000 varlıkların bir sınır aynı anda döndürülür. |Atla ve alın (.NET) kullanın / açıklandığı gibi (REST) üst [bu .NET örnek](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) ve [bu REST API örnek](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). |
| Bazı istemciler, kesintisiz akış bildiriminde bir yineleme etiketi sorunu arasında gelebilir. |Daha fazla bilgi için bkz: [Bu bölümde](media-services-deliver-content-overview.md#known-issues). |
| Media Services .NET SDK'sı nesneleri seri hale getirilemez ve sonuç olarak Azure Redis önbelleği ile çalışmaz. |Azure Redis önbelleğine eklemek için SDK AssetCollection nesneyi serileştirmek çalışırsanız, bir özel durum oluşur. |


## <a name="a-idrestversionhistoryrest-api-version-history"></a><a id="rest_version_history"/>REST API Sürüm Geçmişi
Media Services REST API sürümü geçmişi hakkında daha fazla bilgi için bkz: [Azure Media Services REST API Başvurusu].

## <a name="may-2018"></a>Mayıs 2018 

12 May 2018 Canlı kanallar başlayarak artık destek RTP/MPEG-2 aktarım akışı alma protokolü. Lütfen RTP/MPEG-2'den RTMP veya parçalanmış MP4'e geçiş (kesintisiz akış) protokolleri alma.

## <a name="october-2017-release"></a>Ekim 2017 sürüm
> [!IMPORTANT] 
> Media Services, Azure erişim denetimi hizmeti kimlik doğrulama anahtarları desteği kaldırmaktadır. 22 Haziran 2018 üzerinde artık kod aracılığıyla Media Services arka ucu ile erişim denetimi hizmeti anahtarları kullanılarak doğrulanabilir. Her Azure Active Directory (Azure AD) kullanmak için kodunuzu güncelleştirin [Azure AD tabanlı kimlik doğrulaması](media-services-use-aad-auth-to-access-ams-api.md). Bu değişikliği Azure portalında ilgili uyarılara dikkat edin.

### <a name="updates-for-october-2017"></a>Ekim 2017 güncelleştirmeleri
#### <a name="sdks"></a>SDK’lar
* .NET SDK'sı, Azure AD kimlik doğrulamayı destekleyecek şekilde güncelleştirilmiştir. Erişim denetimi hizmeti kimlik doğrulama desteği, Azure ad daha hızlı geçiş teşvik eden Nuget.org üzerindeki en son .NET SDK kaldırıldı. 
* JAVA SDK'sı, Azure AD kimlik doğrulamayı destekleyecek şekilde güncelleştirilmiştir. Azure AD kimlik doğrulama desteği için Java SDK'sı eklendi. Java SDK'sı ile Media Services'i kullanma hakkında daha fazla bilgi için bkz: [Azure Media Services için Java istemcisi SDK ile çalışmaya başlama](media-services-java-how-to-use.md)

#### <a name="file-based-encoding"></a>Dosya tabanlı kodlama
* Artık Premium Kodlayıcı (HEVC) video codec kodlama H.265 yüksek verimlilik video içeriğinize kodlamak için de kullanabilirsiniz. H.264 gibi diğer codec bileşenleri üzerinden H.265 seçerseniz fiyatlandırma üzerinde etkisi yoktur. HEVC patent lisansları hakkında daha fazla bilgi için bkz: [çevrimiçi Hizmet Koşulları'nı](https://azure.microsoft.com/support/legal/).
* İOS11 veya GoPro kahramanı 6 kullanılarak yakalanan görüntü gibi H.265 (HEVC) video codec ile kodlanmış kaynak video için artık Premium Kodlayıcı veya standart Kodlayıcı bu videolarınızı kodlamak için kullanabilirsiniz. Patent lisansları hakkında daha fazla bilgi için bkz: [çevrimiçi Hizmet Koşulları'nı](https://azure.microsoft.com/support/legal/).
* Birden çok dil ses izleri içeren içerik için dil değerleri (örneğin, ISO MP4) karşılık gelen dosya biçimi belirtimlerine göre doğru şekilde etiketlenmelidir. Daha sonra akışla içerik kodlama için standart kodlayıcı kullanabilirsiniz. Sonuç akış Bulucusu kullanılabilir ses dilleri listeler.
* Standart Kodlayıcı şimdi iki yeni salt ses sistem önayarlarını, "AAC ses" ve "AAC iyi kaliteli ses." destekler Her ikisi de 128 Kb/sn ve 192 Kbps bit hızlarında (AAC) çıkış sırasıyla kodlama ses Gelişmiş stereo üretir.
* Premium Kodlayıcı şimdi giriş olarak QuickTime/MOV dosya biçimlerini destekler. Görüntü codec biri olmalıdır [Apple ProRes türleri bu GitHub makalesinde listelenen](https://docs.microsoft.com/azure/media-services/media-services-media-encoder-standard-formats). Ses ya da AAC veya kod modülasyon (PCM) darbeli gerekir. Premium Kodlayıcı, örneğin, giriş olarak QuickTime/MOV dosyalarında Sarmalanan DVC/DVCPro video desteklemiyor. Standart Kodlayıcı bu görüntü codec bileşenleri desteklemiyor.
* Aşağıdaki hata düzeltmeleri kodlayıcılara yapıldı:

    * Bir giriş varlık kullanarak işleri şimdi gönderebilirsiniz. Bu işleri tamamladıktan sonra varlık değiştirebilirsiniz (örneğin, eklemek, silmek veya varlık içindeki dosyaları yeniden adlandırma) ve ek göndermek.
    * Standart Kodlayıcı tarafından üretilen JPEG küçük resimleri kalitesini geliştirildi.
    * Standart Kodlayıcı giriş meta verilerini ve küçük resim oluşturma çok kısa süre videoları daha iyi işler.
    * Standart Kodlayıcı kullanılan H.264 decoder geliştirmeleri belirli nadir yapılarını ortadan kaldırır. 

#### <a name="media-analytics"></a>Media Analytics
Azure Media Redactor genel kullanılabilirliğini: Bu medya işlemcisi seçili kişiler yüzeyleri Bulanıklaştırma yoluyla anonymization gerçekleştirir ve ortak güvenlik ve haber medya senaryolarda kullanım için idealdir. 

Bu yeni işlemcide bir genel bakış için bkz: [bu blog gönderisine](https://azure.microsoft.com/blog/azure-media-redactor/). Belgeler ve ayarlar hakkında daha fazla bilgi için bkz: [Redaksiyon yüzeyleri Azure medya Analizi ile](media-services-face-redaction.md).



## <a name="june-2017-release"></a>Haziran 2017 sürüm

Media Services destekler [Azure AD tabanlı kimlik doğrulaması](media-services-use-aad-auth-to-access-ams-api.md).

> [!IMPORTANT]
> Şu anda, Media Services erişim denetimi hizmeti kimlik doğrulama modelini destekler. Erişim denetimi hizmeti yetkilendirme 1 Haziran 2018 kullanım dışı kalacaktır. Azure AD kimlik doğrulaması modeline mümkün olan en kısa sürede geçiş yapmanız önerilir.

## <a name="march-2017-release"></a>Mart 2017 sürüm

Standart Kodlayıcı artık kullanabilirsiniz [bit hızı Merdiveni otomatik oluşturma](media-services-autogen-bitrate-ladder-with-mes.md) "Uyarlamalı akış" Önayar dize kodlama bir görev oluşturduğunuzda belirterek. Media Services ile akış için video kodlamak için "Uyarlamalı akış" hazır kullanın. Belirli senaryonuz için önceden belirlenmiş bir kodlama özelleştirmek için ile başlayabilirsiniz [bu hazır](media-services-mes-presets-overview.md).

Artık Medya Kodlayıcısı standart veya Medya Kodlayıcısı Premium akışına kullanabilir [fMP4 öbekleri oluşturan bir kodlama görev oluşturma](media-services-generate-fmp4-chunks.md). 

## <a name="february-2017-release"></a>Şubat 2017 sürüm

1 Nisan 2017 başlangıç 90 günden daha eski hesabınızda herhangi bir işi kaydının otomatik olarak, ilişkili görev kayıtlarını yanı sıra silindi. Toplam kayıt sayısı en yüksek kota altında olsa bile silme işlemi gerçekleşir. İş/görevi bilgileri arşivlemek için açıklanan kodu kullanabilirsiniz [varlıklar ve Media Services .NET SDK'sı ile ilgili varlıklar yönetme](media-services-dotnet-manage-entities.md).

## <a name="january-2017-release"></a>Ocak 2017 sürüm

Media Services'de, içerik doğrudan bir istemci oynatıcı uygulaması veya başkalarına dağıtım için bir içerik teslim ağı (CDN) teslim akış bir hizmet bir akış uç temsil eder. Media Services, aynı zamanda sorunsuz Azure içerik teslim ağı tümleştirme sağlar. Giden akış StreamingEndpoint hizmetinden bir canlı akış, isteğe bağlı video veya aşamalı indirme Media Services hesabınızı, bir varlığın olabilir. Her Media Services hesabı bir varsayılan akış uç noktası içerir. Ek akış uç noktalarını hesabı altında oluşturulabilir. 

Akış uç noktaları, iki sürümü vardır 1.0 ve 2. 0. 10 Ocak 2017 başlangıç sürüm 2.0 varsayılan akış uç herhangi yeni oluşturulan bir Media Services hesabı ekleyin. Bu hesaba eklediğiniz ek akış uç noktalarını ayrıca sürüm 2.0 değildir. Bu değişiklik, var olan hesapları etkilemez. Varolan akış uç noktalarını sürümü 1.0 ve 2.0 sürümüne yükseltilebilir. Davranış, faturalama ve bu değişikliği özellik değişiklikleri vardır. Daha fazla bilgi için bkz. [Akış uç noktalarına genel bakış](media-services-streaming-endpoints-overview.md).

2,15 sürümünden başlayarak, Media Services akış uç noktası varlığa şu özellikleri ekledi:

* CdnProvider 
* CdnProfile
* FreeTrialEndTime 
* StreamingEndpointVersion 

Bu özellikler hakkında daha fazla bilgi için bkz: [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). 

## <a name="december-2016-release"></a>Aralık 2016 sürüm

 Artık, Media Services telemetri/ölçümleri verilere erişmek için kendi Hizmetleri için kullanabilirsiniz. Akış uç noktası Canlı kanal telemetri verilerini toplamak için Media Services geçerli sürümünü kullanın ve varlıkları arşiv. Daha fazla bilgi için bkz: [Media Services telemetri](media-services-telemetry-overview.md).

## <a name="a-idjulychanges16july-2016-release"></a><a id="july_changes16"/>Temmuz 2016 sürüm
### <a name="updates-to-the-manifest-file-ism-generated-by-encoding-tasks"></a>Bildirim dosyası güncelleştirmeleri (*. ISM) oluşturulan görevleri kodlama
Bir kodlama görev Medya Kodlayıcısı standart veya Medya Kodlayıcısı Premium gönderildiğinde kodlama görev oluşturur bir [akış bildirim dosyası](media-services-deliver-content-overview.md) (* .ism) çıkış varlığına içinde. En son hizmet sürümle birlikte, bu akış bildirim dosyasının söz dizimi güncelleştirildi.

> [!NOTE]
> Akış bildirimi (.ism) dosyasının söz dizimi iç kullanım için ayrılmıştır. Sonraki sürüm değişikliklerine tabidir. Değiştirmeyin veya bu dosyanın içeriğini yönlendirebilir.
> 
> 

### <a name="a-new-client-manifest-ismc-file-is-generated-in-the-output-asset-when-an-encoding-task-outputs-one-or-more-mp4-files"></a>Yeni bir istemci bildirimi (*. Bir veya daha fazla MP4 dosyaları bir kodlama görev çıktıları verilebilir ISMC) dosya çıkış varlığına oluşturulur
Bir veya daha fazla MP4 dosyaları oluşturan bir kodlama görev tamamlandıktan sonra en son hizmet sürümünden başlayarak çıkış varlığına akış istemci bildirimi (*.ismc) dosyası da içerir. .İsmc dosya dinamik akış performansını artırmaya yardımcı olur. 

> [!NOTE]
> İstemci bildirimi (.ismc) dosyasının söz dizimi iç kullanım için ayrılmıştır. Sonraki sürüm değişikliklerine tabidir. Değiştirmeyin veya bu dosyanın içeriğini yönlendirebilir.
> 
> 

Daha fazla bilgi için bkz: [bu blog](https://blogs.msdn.microsoft.com/randomnumber/2016/07/08/encoder-changes-within-azure-media-services-now-create-ismc-file/).

### <a name="known-issues"></a>Bilinen sorunlar
Bazı istemciler, kesintisiz akış bildiriminde bir yineleme etiketi sorunu arasında gelebilir. Daha fazla bilgi için bkz: [Bu bölümde](media-services-deliver-content-overview.md#known-issues).

## <a id="apr_changes16"></a>Nisan 2016 sürüm
### <a name="media-analytics"></a>Media Analytics
 Media Services medya analizi için güçlü video gösterimi kullanıma sunuldu. Daha fazla bilgi için bkz: [medya Hizmetleri analizi genel bakış](media-services-analytics-overview.md).

### <a name="apple-fairplay-preview"></a>Apple FairPlay (Önizleme)
Media Services artık, HTTP canlı akışı (HLS) ile Apple FairPlay içerik dinamik olarak şifrelemek için de kullanabilirsiniz. FairPlay lisansları istemcilere teslim etmek için Media Services lisans teslimat hizmeti de kullanabilirsiniz. Daha fazla bilgi için "Kullanın. Apple FairPlay ile korunan HLS içeriğinizin akışını sağlamak için Azure Media Services" konusuna bakın.

## <a id="feb_changes16"></a>Şubat 2016 sürüm
Bir Google Widevine ile ilgili hata düzeltmesi (3.5.3) .NET için Media Services SDK'sı en son sürümünü içerir. Widevine ile şifrelenmiş birden çok varlıklar için AssetDeliveryPolicy yeniden mümkün. Bu hata düzeltmesi bir parçası olarak, aşağıdaki özellikler SDK'SININ eklendi: WidevineBaseLicenseAcquisitionUrl.

    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
    {
        {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl,"http://testurl"},

    };

## <a id="jan_changes_16"></a>Ocak 2016 sürüm
Kodlama ayrılan birimler Kodlayıcı adlarıyla karışıklığı azaltmak için yeniden adlandırıldı.

Temel, standart ve Premium kodlama ayrılan birimler S1, S2, yeniden adlandırılmış ve S3 ayrılmış birimleri, sırasıyla. Bugün temel kodlama ayrılan birimler kullanan müşteriler, Azure portalında (ve fatura) etiketi olarak S1 bakın. Standart ve Premium kullanan müşteriler, S2 ve S3, etiketlerini sırasıyla bakın. 

## <a id="dec_changes_15"></a>Aralık 2015 sürümü

### <a name="media-encoder-deprecation-announcement"></a>Medya Kodlayıcısı kullanımdan Duyurusu

 Medya Kodlayıcısı yaklaşık olarak 12 ay Medya Kodlayıcısı standart sürümündeki başlayarak kullanım dışı kalacaktır.

### <a name="azure-sdk-for-php"></a>PHP için Azure SDK
Yeni bir sürümü Azure SDK'sı takım yayımlanan [PHP için Azure SDK](http://github.com/Azure/azure-sdk-for-php) güncelleştirmeleri ve yeni özellikleri için Media Services içeren paket. Özellikle, PHP için Media Services SDK'sı en son artık destekliyor [içerik koruma](media-services-content-protection-overview.md) özellikleri. Bu dinamik şifreleme AES ve DRM (PlayReady ve Widevine) ile birlikte ve belirteç kısıtlama olmadan özellikleridir. Ayrıca ölçeklendirmeyi destekler [birimleri kodlama](media-services-dotnet-encoding-units.md).

Daha fazla bilgi için bkz.

* Aşağıdaki [kod örnekleri](http://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices) hızlı şekilde kullanmaya yardımcı olur:
  * **vodworkflow_aes.php**: Bu PHP dosyasını AES-128 dinamik şifreleme ve anahtar teslim hizmeti nasıl kullanılacağını gösterir. Açıklandığı .NET örnek dayanır [kullanım AES-128 dinamik şifreleme ve anahtar teslim hizmeti](media-services-protect-with-aes128.md).
  * **vodworkflow_aes.php**: Bu PHP dosyasını PlayReady dinamik şifreleme ve lisans teslimat hizmetinin nasıl kullanılacağını gösterir. Açıklandığı .NET örnek dayanır [kullanım PlayReady ve/veya Widevine dinamik ortak şifreleme](media-services-protect-with-playready-widevine.md).
  * **scale_encoding_units.php**: Bu PHP dosyasını kodlama ayrılan birimleri ölçeklendirme gösterir.

## <a id="nov_changes_15"></a>Kasım 2015 sürüm
 Media Services artık bulutta Widevine lisans teslim hizmeti sunar. Daha fazla bilgi için bkz: [bu blog](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/). Ayrıca bkz [Bu öğretici](media-services-protect-with-playready-widevine.md) ve [GitHub deposunu](http://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm). 

Media Services tarafından sağlanan Widevine lisans teslim hizmetleri önizlemede. Daha fazla bilgi için bkz: [bu blog](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/).

## <a id="oct_changes_15"></a>Ekim 2015 sürüm
Media Services artık canlı aşağıdaki veri merkezlerinde: Brezilya Güney, Hindistan Batı, Hindistan Güney ve Hindistan orta. Azure portalına artık kullanabilirsiniz [Media Service hesapları oluşturmak](media-services-portal-create-account.md) ve açıklanan çeşitli görevleri [Media Services belgeleri Web sayfası](https://azure.microsoft.com/documentation/services/media-services/). Live Encoding bu veri merkezlerinde etkin değil. Ayrıca, bu veri merkezlerinde ayrılan birimler kodlama tüm türleri kullanılabilir.

* Brezilya Güney: Yalnızca standart ve temel ayrılan birimler kodlama kullanılabilir.
* Hindistan Batı, Hindistan Güney ve Hindistan Orta: yalnızca ayrılan birimler kodlama temel kullanılabilir.

## <a id="september_changes_15"></a>Eylül 2015 sürüm
Media Services artık isteğe bağlı ve canlı akış Widevine modüler DRM teknolojisi ile her iki video koruma olanağı sunar. Widevine lisansları teslim yardımcı olması için aşağıdaki teslimat hizmetlerini ortakları kullanabilirsiniz:
* [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/) 
* [EZDRM](http://ezdrm.com/) 
* [castLabs](http://castlabs.com/company/partners/azure/) 

Daha fazla bilgi için bkz: [bu blog](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).
  
AssetDeliveryConfiguration’ı Widevine kullanacak şekilde yapılandırmak için [Media Services .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (3.5.1 sürümünden başlayarak) veya REST API'yi kullanabilirsiniz. 
* Media Services Apple ProRes videolar desteği eklendi. Artık Apple ProRes veya diğer codec bileşenleri kullanan QuickTime kaynak videoları dosyalarınız karşıya yükleyebilirsiniz. Daha fazla bilgi için bkz: [bu blog](https://azure.microsoft.com/blog/announcing-support-for-apple-prores-videos-in-azure-media-services/).
* Artık Medya Kodlayıcısı standart subclipping ve canlı arşiv ayıklama yapmak için kullanabilirsiniz. Daha fazla bilgi için bkz: [bu blog](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).
* Aşağıdaki filtre güncelleştirmeleri yapıldı: 
  
  * Artık bir salt ses filtresiyle Apple HLS biçimini kullanabilirsiniz. Bir salt ses izleme belirterek kaldırmak için bu güncelleştirmeyi kullanabilirsiniz (yalnızca ses = false) URL.
  * Varlıklarınızı için filtreleri tanımladığınızda, birden çok şimdi birleştirebilirsiniz tek bir URL (en çok üç) filtreleri.
    
    Daha fazla bilgi için bkz: [bu blog](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).
* Media Services, t-çerçeveler HLS sürüm 4 şimdi destekler. T-çerçeve desteği İleri ve geri sarma işlemleri en iyi duruma getirir. Varsayılan olarak, tüm HLS sürüm 4 çıkışları t-çerçeve çalma listesi (EXT-X-I-FRAME-STREAM-INF) içerir.
Daha fazla bilgi için bkz: [bu blog](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).

## <a id="august_changes_15"></a>Ağustos 2015 sürüm
* Media Services SDK'sı yeni örnekler ve Java Sürüm 0.8.0 yayın için hazırdır. Daha fazla bilgi için bkz.
    
* Azure Media Player, birden çok ses akışı destek ile güncelleştirilmiştir. Daha fazla bilgi için bkz: [bu blog gönderisine](https://azure.microsoft.com/blog/2015/08/13/azure-media-player-update-with-multi-audio-stream-support/).

## <a id="july_changes_15"></a>Temmuz 2015 sürüm
* Medya Kodlayıcısı standart genel kullanılabilirliğini tanıtıldı. Daha fazla bilgi için bkz: [bu blog gönderisine](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/).
  
    Medya Kodlayıcısı standart kullanan hazır açıklandığı gibi [Bu bölümde](http://go.microsoft.com/fwlink/?LinkId=618336). 4 K için hazır ayar kullandığınızda kodlar, ayrılmış Premium birim türü. Daha fazla bilgi için bkz: [ölçek kodlama](media-services-scale-media-processing-overview.md).
* Canlı gerçek zamanlı resim yazıları Media Services ile Media Player kullanılmıştır. Daha fazla bilgi için bkz: [bu blog gönderisine](https://azure.microsoft.com/blog/2015/07/08/live-real-time-captions-with-azure-media-services-and-player/).

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK'sı güncelleştirmeleri
Media Services .NET SDK'sı sürüm 3.4.0.0 sunulmuştur. Aşağıdaki güncelleştirmeler yapıldı: 

* Destek için Canlı arşiv uygulanmıştır. Canlı bir arşiv içeren bir varlık karşıdan yükleyemiyor.
* Destek için dinamik filtreler uygulanmıştır.
* Böylece bir varlığını silme sırasında kullanıcıların depolama kapsayıcısı tutabilirsiniz işlevselliği uygulanmıştır.
* Hata düzeltmeleri kanalları ilkelerinde yeniden denemek için ilgili yapıldı.
* Medya Kodlayıcısı Premium iş akışı etkinleştirildi.

## <a id="june_changes_15"></a>Haziran 2015 sürüm
### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK'sı güncelleştirmeleri
Media Services .NET SDK'sı sürüm 3.3.0.0 sunulmuştur. Aşağıdaki güncelleştirmeler yapıldı: 

* Openıd Connect bulma belirtimi için destek eklendi.
* Kimlik sağlayıcısı tarafında anahtarları rollover işlemek için destek eklendi.

Bir Openıd Connect bulma belge kullanıma sunan bir kimlik sağlayıcısı kullanıyorsanız (Azure AD Google ve Salesforce yapın), Media Services'ın Openıd Connect bulma spec JSON Web belirteçleri (Jwt'ler) doğrulanması için imzalama anahtarları edinme söyleyebilirsiniz. 

Daha fazla bilgi için bkz: [JWT kimlik doğrulaması Media Services ile çalışmak için Openıd Connect bulma spec kullanım JSON web anahtarları](http://gtrifonov.com/2015/06/07/using-json-web-keys-from-openid-connect-discovery-spec-to-work-with-jwt-token-authentication-in-azure-media-services/).

## <a id="may_changes_15"></a>Mayıs 2015 sürüm
Aşağıdaki yeni özellikleri duyurdu:

* [Media Services ile gerçek zamanlı kodlama, Önizleme](media-services-manage-live-encoder-enabled-channels.md)
* [Dinamik bildirimi](media-services-dynamic-manifest-overview.md)
* [Azure medya Hyperlapse medya işlemcisi önizlemesi](https://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)

## <a id="april_changes_15"></a>Nisan 2015 güncelleştirmesinden
### <a name="general-media-services-updates"></a>Genel Media Services güncelleştirir
* [Media Player](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/) duyurulan.
* Media Services REST 2.10 ile başlayarak, bir gerçek zamanlı ileti Protocol (RTMP) alma için yapılandırılan kanalları birincil ile oluşturulan ve ikincil URL'leri alma. Daha fazla bilgi için bkz: [kanal alma yapılandırmaları](media-services-live-streaming-with-onprem-encoders.md#channel_input).
* Azure Media Indexer güncelleştirildi.
* İspanyolca dil için destek eklendi.
* XML biçimi için yeni bir yapılandırma eklendi.

Daha fazla bilgi için bkz: [bu blog](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK'sı güncelleştirmeleri
Media Services .NET SDK sürüm 3.2.0.0 sunulmuştur. Aşağıdaki güncelleştirmeler yapıldı:

* Değişiklik sonu: TokenRestrictionTemplate.Issuer ve TokenRestrictionTemplate.Audience değiştirildi dize türünde olmalıdır.
* Güncelleştirmeleri özel yeniden deneme ilkelerini oluşturmak için ilgili yapıldı.
* Hata düzeltmeleri dosya yükleme ve indirme ilgili yapıldı.
* MediaServicesCredentials sınıfı karşı kimlik doğrulaması için birincil ve ikincil erişim denetim uç noktaları artık kabul eder.

## <a id="march_changes_15"></a>Mart 2015 sürüm
### <a name="general-media-services-updates"></a>Genel Media Services güncelleştirir
* Media Services şimdi içerik teslim ağı tümleştirme sağlar. Tümleştirmesini desteklemek için CdnEnabled özelliği için StreamingEndpoint eklendi. CdnEnabled 2.9 sürümünden başlayarak REST API'leri ile kullanılabilir. Daha fazla bilgi için bkz: [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). CdnEnabled 3.1.0.2 sürümünden başlayarak .NET SDK ile kullanılabilir. Daha fazla bilgi için bkz: [StreamingEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.istreamingendpoint\(v=azure.10\).aspx).
* Medya Kodlayıcısı Premium iş akışı tanıtıldı. Daha fazla bilgi için bkz: [Tanıtımı Premium Azure Media Services ile kodlama](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/).

## <a id="february_changes_15"></a>Şubat 2015 sürüm
### <a name="general-media-services-updates"></a>Genel Media Services güncelleştirir
Media Services REST API sürümü 2.9 sunulmuştur. Bu sürümünden başlayarak, akış uç noktaları ile içerik teslim ağı tümleştirme etkinleştirebilirsiniz. Daha fazla bilgi için bkz: [StreamingEndpoint](https://msdn.microsoft.com/library/dn783468.aspx).

## <a id="january_changes_15"></a>Ocak 2015 sürüm
### <a name="general-media-services-updates"></a>Genel Media Services güncelleştirir
Dinamik şifreleme ile içerik koruma genel kullanılabilirlik tanıtıldı. Daha fazla bilgi için bkz: [Media Services akış güvenlik DRM teknoloji genel kullanılabilirliğini geliştiren](https://azure.microsoft.com/blog/2015/01/29/azure-media-services-enhances-streaming-security-with-general-availability-of-drm-technology/).

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK'sı güncelleştirmeleri
Media Services .NET SDK'sı sürüm 3.1.0.1 sunulmuştur.

Bu sürüm, varsayılan Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization.TokenRestrictionTemplate Oluşturucu artık kullanılmayan olarak işaretlenmiş. Yeni Oluşturucusu TokenType bağımsız değişken olarak alır.

    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);


## <a id="december_changes_14"></a>Aralık 2014 sürümü
### <a name="general-media-services-updates"></a>Genel Media Services güncelleştirir
* Bazı güncelleştirmeleri ve yeni özellikleri Media Indexer eklendi. Daha fazla bilgi için bkz: [Azure Media Indexer sürüm 1.1.6.7 Sürüm Notları](https://azure.microsoft.com/blog/2014/12/03/azure-media-indexer-version-1-1-6-7-release-notes/).
* Yeni bir REST API kodlama ayrılan birimler güncelleştirmek için kullanabileceğiniz eklendi. Daha fazla bilgi için bkz: [EncodingReservedUnitType REST ile](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype).
* CORS desteği için anahtar teslim hizmeti eklendi.
* Yetkilendirme İlkesi seçenekleri sorgulama için performans geliştirmeler yapılmıştır.
* Çin veri merkezinde [anahtar teslim URL](https://docs.microsoft.com/rest/api/media/operations/contentkey#get_delivery_service_url) müşteri sunulmuştur (diğer veri merkezlerinde olduğu gibi).
* HLS otomatik hedef süresi eklendi. Canlı akış yaparken HLS her zaman dinamik olarak paketlenir. Varsayılan olarak, Media Services ana kare (KeyFrameInterval) aralığında HLS segment paketleme oranı (FragmentsPerSegment) otomatik olarak hesaplar. Bu yöntem da dinamik kodlayıcıdan alınan grubu resimleri (GOP) olarak bilinir. Daha fazla bilgi için bkz: [iş Media Services ile canlı akış](http://msdn.microsoft.com/library/azure/dn783466.aspx).

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK'sı güncelleştirmeleri
[Media Services .NET SDK'sı](http://www.nuget.org/packages/windowsazure.mediaservices/) şimdi 3.1.0.0 sürümüdür. Aşağıdaki güncelleştirmeler yapıldı:

* .NET SDK bağımlılığı .NET 4.5 Framework yükseltildi.
* Kodlama ayrılan birimler güncelleştirmek için kullanabileceğiniz yeni bir API eklendi. Daha fazla bilgi için bkz: [güncelleştirme ayrılmış birim türü ve .NET kullanarak ayrılan birimler kodlama artış](media-services-dotnet-encoding-units.md).
* JWT belirteci kimlik doğrulaması desteği eklendi. Daha fazla bilgi için bkz: [JWT belirteci kimlik doğrulaması Media Services ve dinamik şifreleme](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).
* PlayReady lisans şablonunda BeginDate ve ExpirationDate için göreli uzaklık eklendi.

## <a id="november_changes_14"></a>Kasım 2014 sürümü
* Media Services artık bir SSL bağlantısı üzerinden Canlı kesintisiz akış (fMP4) içerik almak için de kullanabilirsiniz. SSL üzerinden alma için alma URL'si için HTTPS güncelleştirdiğinizden emin olun. Şu anda, Media Services SSL ile özel etki alanlarını desteklemiyor. Canlı akış hakkında daha fazla bilgi için bkz: [iş Azure Media Services canlı akış ile](http://msdn.microsoft.com/library/azure/dn783466.aspx).
* Şu anda bir SSL bağlantısı üzerinden RTMP canlı akış alma olamaz.
* İçeriğinizi teslim etmek istediğiniz akış uç noktası 10 Eylül 2014 sonra yalnızca oluşturulduysa, SSL üzerinden akışını sağlayabilirsiniz. URL "streaming.mediaservices.windows.net" (yeni biçimde) varsa, akış URL'leri 10 Eylül 2014 sonra oluşturulan akış uç noktalarını dayanır. "Origin.mediaservices.windows.net" (eski biçimde) içeren akış URL'leri SSL desteklemez. URL'niz eski biçimindedir ve SSL üzerinden akış istiyorsanız [yeni bir akış uç noktası oluşturma](media-services-portal-manage-streaming-endpoints.md). SSL üzerinden içeriğinizin akışını sağlamak için yeni akış uç noktasında temel URL kullanın.

## <a id="october_changes_14"></a>Ekim 2014 sürümünden
### <a id="new_encoder_release"></a>Media Services Kodlayıcısı sürüm
 Media Services Azure Medya Kodlayıcısı yeni sürümü tanıtıldı. En son medya Kodlayıcı ile yalnızca için ücret ödersiniz GB çıktı. Aksi takdirde, yeni Kodlayıcı önceki encoder ile uyumlu bir özelliktir. Daha fazla bilgi için bkz: [Media Services fiyatlandırma ayrıntıları].

### <a id="oct_sdk"></a>Media Services .NET SDK'sı
.NET uzantıları için Media Services SDK'sı sürüm 2.0.0.3 sunulmuştur.

.NET için Media Services SDK'sı sürüm 3.0.0.8 sunulmuştur. Aşağıdaki güncelleştirmeler yapıldı:

* Yeniden düzenleme yeniden deneme ilkesi sınıflarda uygulanmıştır.
* Bir kullanıcı aracısı dizesi HTTP istek üstbilgilerinin eklendi.
* NuGet restore derleme adımı eklendi.
* Senaryo testleri x509 kullanılacak giderildi depodan sertifika.
* Kanalı ve akış uç güncelleştirdiğinizde doğrulama ayarları için eklendi.

### <a name="new-github-repository-to-host-media-services-samples"></a>Ana bilgisayar Media Services örnekleri için yeni GitHub deposunu
Örnekleri olan [Media Services örnekleri GitHub deposunu](https://github.com/Azure/Azure-Media-Services-Samples).

## <a id="september_changes_14"></a>Eylül 2014 sürümü
Media Services REST meta veri sürümü 2.7 sunulmuştur. En son KALAN güncelleştirmeleri hakkında daha fazla bilgi için bkz: [Media Services REST API Başvurusu](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).

.NET için Media Services SDK'sı sürüm 3.0.0.7 sunulmuştur

### <a id="sept_14_breaking_changes"></a>Yeni değişiklikler
* Kaynak adlandırıldı [StreamingEndpoint].
* Kodlamak ve MP4 dosyaları yayımlamak için Azure portalı kullanırken varsayılan davranış bir değişiklik yapılmıştır.

### <a id="sept_14_GA_changes"></a>Yeni özellikler/genel kullanılabilirlik sürümünün bir parçası olan senaryolar
* Media Indexer medya işlemcisi sunulmuştur. Daha fazla bilgi için bkz: [dizin medya Dizin Oluşturucu ile medya dosyalarını](http://msdn.microsoft.com/library/azure/dn783455.aspx).
* Kullanabileceğiniz [StreamingEndpoint] (ana) özel etki alanı adları eklemek için varlığı.
  
    Özel etki alanı adı Media Services akış uç nokta adı olarak kullanmak için akış uç noktanız için özel ana bilgisayar adlarını ekleyin. Özel ana bilgisayar adları eklemek için Media Services REST API'leri veya .NET SDK'yı kullanın.
  
    Aşağıdaki maddeler geçerlidir:
  
  * Özel etki alanı adı sahipliğini olması gerekir.
  * Etki alanı adı sahipliğini Media Services tarafından doğrulanması gerekir. Etki alanını doğrulamak için DNS mediaservices dns bölge doğrulamak için MediaServicesAccountId üst etki alanı eşleyen bir CName oluşturun.
  * Medya Hizmetleri StreamingEndpoint ana bilgisayar adı (örneğin, amstest.streaming.mediaservices.windows.net) özel ana bilgisayar adı (örneğin, sports.contoso.com) eşlemeleri başka bir CName oluşturmanız gerekir.

    Daha fazla bilgi için bkz: CustomHostNames özelliğinde [StreamingEndpoint](http://msdn.microsoft.com/library/azure/dn783468.aspx) makalesi.

### <a id="sept_14_preview_changes"></a>Yeni özellikler/genel Önizleme sürümü parçası olan senaryolar
* Canlı akış Önizleme. Daha fazla bilgi için bkz: [iş Media Services ile canlı akış](http://msdn.microsoft.com/library/azure/dn783466.aspx).
* Anahtar teslim hizmeti. Daha fazla bilgi için bkz: [kullanım AES-128 dinamik şifreleme ve anahtar teslim hizmeti](http://msdn.microsoft.com/library/azure/dn783457.aspx).
* AES dinamik şifreleme. Daha fazla bilgi için bkz: [kullanım AES-128 dinamik şifreleme ve anahtar teslim hizmeti](http://msdn.microsoft.com/library/azure/dn783457.aspx).
* PlayReady lisans teslimat hizmeti. 
* PlayReady dinamik şifreleme. 
* Medya Hizmetleri PlayReady lisans şablonu. Daha fazla bilgi için bkz: [Media Services PlayReady lisans şablonuna genel bakış].
* Akış depolama şifrelenmiş varlıklar. Daha fazla bilgi için bkz: [depolama şifrelenmiş içerik akışı](http://msdn.microsoft.com/library/azure/dn783451.aspx).

## <a id="august_changes_14"></a>Ağustos 2014 sürümü
Bir varlık kodlama, kodlama işi tamamlandığında, çıkış varlık oluşturulur. Bu sürüm kadar Media Services Encoder çıkış varlıklar hakkında meta veriler üretti. Bu sürüm ile başlayarak, kodlayıcı de giriş varlıklar hakkında meta veriler üretir. Daha fazla bilgi için bkz: [Giriş meta verileri] ve [çıkış meta verileri].

## <a id="july_changes_14"></a>Temmuz 2014 sürümü
Azure Media Services Paketleyici ve Şifreleyici için aşağıdaki hata düzeltmeleri yapıldı:

* Canlı arşiv varlık HLS için iletilirken yalnızca ses geri oynar: Bu sorun giderilmiştir ve ses ve video şimdi yürütebilirsiniz.
* Bir varlık HLS ve AES 128-bit Zarf şifreleme paketlendiğinde paketlenmiş akışlar Android cihazlarda kayıttan yok: Bu hatanın düzeltildiğini ve paket akışı HLS destek Android cihazlarda geri kazanır.

## <a id="may_changes_14"></a>Mayıs 2014 sürümü
### <a id="may_14_changes"></a>Genel Media Services güncelleştirir
Artık kullanabilirsiniz [dinamik paketleme] akışa HLS sürüm 3. Akış HLS sürüm 3 için aşağıdaki biçimi Kaynak Konum Belirleyicisi yolunu ekleyin: * .ism/manifest(format=m3u8-aapl-v3). Daha fazla bilgi için bkz: [bu blog](http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/).

Dinamik paketleme şimdi de statik olarak PlayReady ile şifrelenmiş kesintisiz akış göre PlayReady ile şifrelenmiş HLS (sürüm 3 ve sürüm 4) teslim destekler. Kesintisiz akış PlayReady ile şifreleme hakkında daha fazla bilgi için bkz: [PlayReady ile kesintisiz akış korumak](http://msdn.microsoft.com/library/azure/dn189154.aspx).

### <a name="may_14_donnet_changes"></a>Media Services .NET SDK'sı güncelleştirmeleri
Media Services .NET SDK'sı sürüm 3.0.0.5 sunulmuştur. Aşağıdaki güncelleştirmeler yapıldı:

* Karşıya yükleme ve ortam varlıkları indirme zaman hızı ve esnekliği iyidir.
* Yeniden deneme mantığı ve geçici özel durum işleme içinde geliştirmeler yapılmıştır: 
  
  * Geçici hata algılama ve yeniden deneme mantığı, sorgu, değişiklikleri kaydetmek ve dosyaları karşıya veya karşıdan yükleyen neden olduğu özel durumlar için geliştirilmiş. 
  * Web özel durumları (örneğin, bir erişim denetimi hizmeti belirteç isteği sırasında) aldığınızda, önemli hataları daha hızlı şimdi başarısız.

Daha fazla bilgi için bkz: [.NET için Media Services SDK'sı mantığı yeniden dene].

## <a id="april_changes_14"></a>Nisan 2014 Kodlayıcı sürümü
### <a name="april_14_enocer_changes"></a>Media Services Kodlayıcısı güncelleştirmeleri
* Çimen vadisi EDIUS doğrusal Düzenleyicisi'ni kullanarak yazılan AVI dosyalarını almak için destek eklendi. Bu işlem, video hafifçe Çimen vadisi denetim merkezini/HQX codec kullanılarak sıkıştırılmış. Daha fazla bilgi için bkz: [EDIUS bulut üzerinden akış 7 Çimen vadisi bildirdi].
*  Medya Hizmetleri Kodlayıcı tarafından üretilen dosyalar için adlandırma kuralı belirtmek için destek eklendi. Daha fazla bilgi için bkz: [denetim Media Services Encoder çıkış dosya adları](http://msdn.microsoft.com/library/azure/dn303341.aspx).
*  Video ve/veya ses yer paylaşımları için destek eklendi. Daha fazla bilgi için bkz: [yer paylaşımları oluşturma](http://msdn.microsoft.com/library/azure/dn640496.aspx).
*  Birden çok video kesimi birleştirmek için destek eklendi. Daha fazla bilgi için bkz: [elde etme video kesimleri](http://msdn.microsoft.com/library/azure/dn640504.aspx).
* Kod ses burada kodlanan MP4s MPEG-1 ses Katman 3 (MP3 olarak da bilinir) ile ilgili bir hata düzeltildi.

## <a id="jan_feb_changes_14"></a>Ocak/Şubat 2014 sürümleri
### <a name="jan_fab_14_donnet_changes"></a>.NET SDK'sı 3.0.0.1, 3.0.0.2 ve 3.0.0.3 medya Hizmetleri
3.0.0.1 ve 3.0.0.2 değişiklikleri şunlardır:

* OrderBy ifadelerle LINQ sorgularını kullanımı ile ilgili sorunlar düzeltilmiştir.
* Test çözümlerinde [GitHub] birim tabanlı testleri ve senaryo tabanlı testleri bölün.

Değişiklikler hakkında daha fazla bilgi için bkz: [Media Services .NET SDK 3.0.0.1 ve 3.0.0.2 serbest](http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/).

Sürüm 3.0.0.3 aşağıdaki değişiklikler yapıldı:

* Azure depolama bağımlılıkları sürümü 3.0.3.0 kullanacak biçimde yükseltilmiştir.
* Geriye dönük uyumluluk sorunu 3.0 için düzeltildi. *.* serbest bırakır.

## <a id="december_changes_13"></a>Aralık 2013 sürüm
### <a name="dec_13_donnet_changes"></a>Media Services .NET SDK'sı 3.0.0.0
> [!NOTE]
> 3.0.x.x sürümleri 2.4.x.x sürümleriyle geriye dönük olarak uyumlu değildir.
> 
> 

Media Services SDK'sı en son sürümünü 3.0.0.0 sunulmuştur. En son paketini Nuget'ten indirin ya da bitten alma [GitHub].

Media Services SDK sürüm 3.0.0.0 ile başlayarak, yeniden kullanabileceğiniz [Azure AD erişim denetimi hizmeti](http://msdn.microsoft.com/library/hh147631.aspx) belirteçleri. Daha fazla bilgi için "Yeniden erişim denetimi hizmeti belirteçler" bölümüne bakın [.NET için Media Services SDK'sı ile Media Services'e bağlanma](http://msdn.microsoft.com/library/azure/jj129571.aspx).

### <a name="dec_13_donnet_ext_changes"></a>Media Services .NET SDK uzantıları 2.0.0.0
 Media Services .NET SDK uzantıları, genişletme yöntemleri ve, kodunuzu basitleştirerek Media Services ile geliştirme yapmayı kolaylaştıran yardımcı işlevleri kümesidir. En son bitten alabilirsiniz [Media Services .NET SDK uzantıları](https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev).

## <a id="november_changes_13"></a>Kasım 2013 sürüm
### <a name="nov_13_donnet_changes"></a>Media Services .NET SDK'sı değişiklikleri
Bu sürüm, Media Services REST API katmanı çağrıları yapıldığında oluşabilecek .NET tanıtıcıları geçici hata hatalar için Media Services SDK'sı ile başlatılıyor.

## <a id="august_changes_13"></a>Ağustos 2013 sürüm
### <a name="aug_13_powershell_changes"></a>Azure SDK Araçları'nın içerdiği medya Hizmetleri PowerShell cmdlet'leri
Aşağıdaki medya Hizmetleri PowerShell cmdlet'leri şimdi dahil edilen [Azure SDK Araçları](https://github.com/Azure/azure-sdk-tools):

* Get-AzureMediaServices 

    Örneğin, `Get-AzureMediaServicesAccount`
* New-AzureMediaServicesAccount 
  
    Örneğin, `New-AzureMediaServicesAccount -Name “MediaAccountName” -Location “Region” -StorageAccountName “StorageAccountName”`
* New-AzureMediaServicesKey 
  
    Örneğin, `New-AzureMediaServicesKey -Name “MediaAccountName” -KeyType Secondary -Force`
* Remove-AzureMediaServicesAccount 
  
    Örneğin, `Remove-AzureMediaServicesAccount -Name “MediaAccountName” -Force`

## <a id="june_changes_13"></a>Haziran 2013 sürüm
### <a name="june_13_general_changes"></a>Media Services değişiklikleri
Bu bölümde belirtilen aşağıdaki değişiklikleri Haziran 2013 Media Services sürümlerde bulunan güncelleştirmeler şunlardır:

* Birden çok depolama hesabı bir medya hizmeti hesabına bağlamak yeteneği. 
    * StorageAccount
    * Asset.StorageAccountName ve Asset.StorageAccount
* Job.Priority güncelleştirme özelliği. 
* Bildirim ilgili varlıklar ve özellikleri: 
    * JobNotificationSubscription
    * NotificationEndPoint
    * İş
* Asset.Uri 
* Locator.Name 

### <a name="june_13_dotnet_changes"></a>Media Services .NET SDK'sı değişiklikleri
Aşağıdaki değişiklikler dahil edilen Haziran 2013'te Media Services SDK'sı serbest bırakır. En son Media Services SDK'sı, GitHub üzerinde kullanılabilir.

* 2.3.0.0 sürümünden başlayarak, birden çok depolama bağlama Media Services SDK'sı destekler hesapları bir Media Services hesabı. Bu özellik aşağıdaki API'leri destekler:
  
    * IStorageAccount türü
    * Microsoft.WindowsAzure.MediaServices.Client.CloudMediaContext.StorageAccounts property
    * StorageAccount özelliği
    * StorageAccountName özelliği
  
    Daha fazla bilgi için bkz: [Media Services'ı Yönet varlıklar birden çok depolama hesapları arasında](http://msdn.microsoft.com/library/azure/dn271889.aspx).
* Bildirim ilgili API'ler. 2.2.0.0 sürümünden başlayarak, Azure kuyruk depolama bildirimleri göndermek için dinleme. Daha fazla bilgi için bkz: [işlemek Media Services iş bildirimleri](http://msdn.microsoft.com/library/azure/dn261241.aspx).
  
    * Microsoft.WindowsAzure.MediaServices.Client.IJob.JobNotificationSubscriptions property
    * Microsoft.WindowsAzure.MediaServices.Client.INotificationEndPoint type
    * Microsoft.WindowsAzure.MediaServices.Client.IJobNotificationSubscription type
    * Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointCollection type
    * Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointType type
* Depolama istemcisi SDK 2.0 (Microsoft.WindowsAzure.StorageClient.dll) bağımlılığı
* OData 5.5 (Microsoft.Data.OData.dll) bağımlılığı

## <a id="december_changes_12"></a>Kasım 2012 sürüm
### <a name="dec_12_dotnet_changes"></a>Media Services .NET SDK'sı değişiklikleri
* IntelliSense: Birçok türü için IntelliSense belgeleri eksik eklendi.
* Microsoft.Practices.TransientFaultHandling.Core: Burada SDK bu derleme eski bir sürümü için bir bağımlılık hala sahip bir sorun giderilmiştir. SDK'sı, artık bu derleme sürümüne 5.1.1209.1 başvuruyor.

Kasım 2012'de SDK bulunan sorunları giderir:

* IAsset.Locators.Count: tüm bulucular silindikten sonra Bu sayaç artık doğru şekilde yeni IAsset arabirimlerde bildirilir.
* IAssetFile.ContentFileSize: Bu değer şimdi düzgün bir şekilde karşıya yükleme IAssetFile.Upload(filepath) tarafından ayarlanır.
* IAssetFile.ContentFileSize: Bu özellik, artık bir varlık dosyası oluşturduğunuzda ayarlanabilir. Daha önce salt okunur.
* IAssetFile.Upload(filepath): Birden çok dosya varlık için yüklenen olduğunda bu zaman uyumlu karşıya yükleme yöntemi aşağıdaki hata burada atma olan bir sorun giderilmiştir. Hata: "Sunucu isteğin kimliğini doğrulayamadı. Authorization Üstbilgisi değeri doğru imza dahil olmak üzere biçimlendirildiğinden emin olun."
* IAssetFile.UploadAsync: Beş dosyaları dosyalarına eşzamanlı karşıya sınırlı bir sorun giderilmiştir.
* IAssetFile.UploadProgressChanged: Bu olay artık SDK'sı tarafından sağlanır.
* IAssetFile.DownloadAsync (dize, BlobTransferClient, ILocator, CancellationToken): Bu yöntem aşırı şimdi sağlanır.
* IAssetFile.DownloadAsync: Beş dosyalara dosyaların eş zamanlı indirme sınırlı bir sorun giderilmiştir.
* IAssetFile.Delete(): Hiçbir dosya için IAssetFile karşıya yüklediyseniz arama silme bir özel durum burada throw bir sorun giderilmiştir.
* İşlerini: Burada bir proje şablonu kullanarak bir "MP4 kesintisiz akışlara görev için" "PlayReady koruma görevini" ile zincirleme herhangi bir görevi hiç oluşturmamışsınızdır bir sorun giderilmiştir.
* EncryptionUtils.GetCertificateFromStore(): Bu yöntem artık sertifika yapılandırma sorunlarını göre sertifikayı bulma hatası nedeniyle bir null başvuru özel durumu oluşturur.

## <a id="november_changes_12"></a>Kasım 2012 sürüm
Bu bölümde belirtilen değişiklikleri Kasım 2012'de (sürüm 2.0.0.0) bulunan güncelleştirmeleri olan SDK. Bu değişiklikler SDK sürüm yeniden yazılmıştır veya değiştirilecek Haziran 2012 Önizleme için yazılmış herhangi bir kod gerektirebilir.

* Varlıklar
  
    * IAsset.Create(assetName) olan *yalnızca* varlık oluşturma işlevi. IAsset.Create yöntem çağrısının bir parçası olarak artık dosyaları karşıya yükler. IAssetFile yüklemek için kullanın.
    * IAsset.Publish yöntemi ve AssetState.Publish numaralandırma değeri Hizmetleri SDK'dan kaldırıldı. Bu değeri kullanır herhangi bir kod yazılması gerekir.
* FileInfo
  
    * Bu sınıf kaldırıldı ve IAssetFile tarafından değiştirildi.
  
* IAssetFiles
  
    * IAssetFile FileInfo değiştirir ve farklı bir davranışı vardır. Kullanmak için ya da Media Services SDK'sı veya depolama SDK'sını kullanarak bir dosyayı karşıya yükleyin ve ardından IAssetFiles nesne örneği oluşturur. Aşağıdaki IAssetFile.Upload aşırı kullanılabilir:
  
        * IAssetFile.Upload(filePath): İş parçacığı bu zaman uyumlu yöntemi engeller ve yalnızca tek bir dosyayı karşıya yüklediğinizde bu önerilir.
        * IAssetFile.UploadAsync (filePath, blobTransferClient, Bulucu, cancellationToken): Bu zaman uyumsuz yöntem tercih edilen karşıya yükleme mekanizmadır. 
    
            Bilinen hata: iptal belirteci kullanırsanız, karşıya yükleme iptal edildi. Görevleri birçok iptal durumlar olabilir. Düzgün catch ve özel durumları işleme gerekir.
* Bulucular
  
    * Kaynak özgü sürümleri kaldırıldı. SAS özgü bağlamı. Locators.CreateSasLocator (varlık, accessPolicy) kullanım dışı bırakılan veya kaldırılan genel kullanılabilirlik ile işaretlenir. Güncelleştirilmiş davranışı için "yeni işlevsellik" altında "Bulucular" bölümüne bakın.

## <a id="june_changes_12"></a>Haziran 2012 Önizleme sürümü
Aşağıdaki işlevleri SDK Kasım sürümündeki yeni:

* Varlıkları silme
  
    * IAsset, IAssetFile, ILocator, IAccessPolicy ve IContentKey nesneler nesne düzeyinde bir silme koleksiyondaki diğer bir deyişle, cloudMediaContext.ObjCollection.Delete(objInstance) gerektirmek yerine diğer bir deyişle, IObject.Delete(), şimdi silinir.
* Bulucular
  
    * Bulucular şimdi CreateLocator yöntemi kullanılarak oluşturulmalıdır. Bunlar, oluşturmak istediğiniz bağımsız değişken olarak Bulucu özel türünün LocatorType.SAS veya LocatorType.OnDemandOrigin enum değerleri kullanmanız gerekir.
    * Yeni özellikler kullanılabilir URI'ler içeriğiniz için elde kolaylaştırmak için bulucular eklenmiştir. Bu yeniden tasarlanması Bulucuyu gelecekteki üçüncü taraf genişletilebilirliği için daha fazla esneklik sağlar ve media istemci uygulamalar için kullanım kolaylığı artırır.
* Zaman uyumsuz yöntem desteği
  
    * Tüm yöntemleri için zaman uyumsuz desteği eklendi.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

<!-- Anchors. -->

<!-- Images. -->

<!--- URLs. --->
[Azure Media Services MSDN Forumu]: http://social.msdn.microsoft.com/forums/azure/home?forum=MediaServices
[Azure Media Services REST API Başvurusu]: https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference
[Media Services fiyatlandırma ayrıntıları]: http://azure.microsoft.com/pricing/details/media-services/
[Giriş meta verileri]: http://msdn.microsoft.com/library/azure/dn783120.aspx
[Çıkış meta verileri]: http://msdn.microsoft.com/library/azure/dn783217.aspx
[Deliver content]: http://msdn.microsoft.com/library/azure/hh973618.aspx
[Index media files with the Azure Media Indexer]: http://msdn.microsoft.com/library/azure/dn783455.aspx
[StreamingEndpoint]: http://msdn.microsoft.com/library/azure/dn783468.aspx
[Work with Media Services live streaming]: http://msdn.microsoft.com/library/azure/dn783466.aspx
[Use AES-128 dynamic encryption and the key delivery service]: http://msdn.microsoft.com/library/azure/dn783457.aspx
[Use PlayReady dynamic encryption and the license delivery service]: http://msdn.microsoft.com/library/azure/dn783467.aspx
[Preview features]: http://azure.microsoft.com/services/preview/
[Media Services PlayReady lisans şablonuna genel bakış]: http://msdn.microsoft.com/library/azure/dn783459.aspx
[Stream storage-encrypted content]: http://msdn.microsoft.com/library/azure/dn783451.aspx
[Azure portal]: https://portal.azure.com
[Dinamik paketleme]: http://msdn.microsoft.com/library/azure/jj889436.aspx
[Nick Drouin's blog]: http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/
[Protect Smooth Streaming with PlayReady]: http://msdn.microsoft.com/library/azure/dn189154.aspx
[.NET için Media Services SDK'sı mantığı yeniden dene]: http://msdn.microsoft.com/library/azure/dn745650.aspx
[EDIUS bulut üzerinden akış 7 Çimen vadisi bildirdi]: http://www.streamingmedia.com/Producer/Articles/ReadArticle.aspx?ArticleID=96351&utm_source=dlvr.it&utm_medium=twitter
[Control Media Services Encoder output file names]: http://msdn.microsoft.com/library/azure/dn303341.aspx
[Create overlays]: http://msdn.microsoft.com/library/azure/dn640496.aspx
[Stitch video segments]: http://msdn.microsoft.com/library/azure/dn640504.aspx
[Azure Media Services .NET SDK 3.0.0.1 and 3.0.0.2 releases]: http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/
[Azure AD Access Control Service]: http://msdn.microsoft.com/library/hh147631.aspx
[Connect to Media Services with the Media Services SDK for .NET]: http://msdn.microsoft.com/library/azure/jj129571.aspx
[Media Services .NET SDK extensions]: https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev
[Azure SDK tools]: https://github.com/Azure/azure-sdk-tools
[GitHub]: https://github.com/Azure/azure-sdk-for-media-services
[Manage Media Services assets across multiple Storage accounts]: http://msdn.microsoft.com/library/azure/dn271889.aspx
[Handle Media Services job notifications]: http://msdn.microsoft.com/library/azure/dn261241.aspx

