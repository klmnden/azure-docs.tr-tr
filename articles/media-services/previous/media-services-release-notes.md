---
title: Media Services sürüm notları | Microsoft Docs
description: Media Services sürüm notları
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: media
ms.devlang: dotnet
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 25da9fd787c467bdddb7c8dcd68b9df518d018b7
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64728043"
---
# <a name="azure-media-services-release-notes"></a>Azure Media Services sürüm notları

Bu sürüm notlarını Azure Media Services önceki sürümleri ve bilinen sorunlar değişiklikleri özetlemek için.

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürüm olan [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/)’ü inceleyin. Ayrıca bkz [geçiş kılavuzuna v2'den v3](../latest/migrate-from-v2-to-v3.md)

Şu sorunlardan etkilendiğiniz sorunlarını düzeltmeye odaklandık müşterilerimizden duymak istiyoruz. Sorun bildirin veya soru sormak için bir gönderiyi gönderme [Azure Media Services MSDN Forumu]. 

## <a name="a-idissuescurrently-known-issues"></a><a id="issues"/>Şu anda bilinen sorunlar
### <a name="a-idgeneralissuesmedia-services-general-issues"></a><a id="general_issues"/>Media Services genel sorunlar

| Sorun | Açıklama |
| --- | --- |
| Birçok ortak HTTP üst bilgilerini REST API'SİNDE sağlanmayan. |REST API kullanarak Media Services uygulamalar geliştiriyorsanız, bazı ortak HTTP üst bilgi alanları bulabilirsiniz (dahil CLIENT-REQUEST-ID REQUEST-ID yanı sıra, RETURN-CLIENT-REQUEST-ID) desteklenmez. Üst bilgiler, gelecek güncelleştirmelerden birinde eklenecektir. |
| Yüzde kodlama izin verilmiyor. |Media Services akış içeriğini URL'ler oluşturulurken IAssetFile.Name özelliğinin değeri kullanır (örneğin, `http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters`). Bu nedenle, yüzde kodlama izin verilmiyor. Name özelliği değeri aşağıdakilerden herhangi birini olamaz [yüzde kodlama-ayrılmış karakterleri](https://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ". Ayrıca, yalnızca bir olabilir "." dosya adı uzantısı için. |
| Azure depolama SDK'sı sürüm 3.x başarısız parçasıdır ListBlobs yöntem. |Media Services oluşturur göre tüm SAS URL'lerini [2012-02-12](https://docs.microsoft.com/rest/api/storageservices/Version-2012-02-12) sürümü. Bir blob kapsayıcı içindeki blobları listeleme için depolama SDK'sını kullanmak istiyorsanız, kullanın [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobs.aspx) depolama SDK'sı sürüm parçası olan yöntemin 2.x. |
| Azaltma mekanizması Media Services, hizmete aşırı isteklerde uygulamalar için kaynak kullanımını kısıtlıyor. Hizmet, "Hizmet kullanılamıyor" 503 HTTP durum kodunu döndürebilir. |Daha fazla bilgi için bkz: 503 HTTP durum kodu açıklamasını [Media Services hata kodları](media-services-encoding-error-codes.md). |
| Varlıkları sorgulama yaptığında, sürüm 2 genel REST sorgu sonuçlarını 1.000 sonuçları sınırladığı için 1000 varlıkların bir sınır aynı anda döndürülür. |Atla ve Al (.NET) / açıklandığı (REST) üst [bu .NET örnek](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) ve [bu REST API örnek](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). |
| Bazı istemciler, kesintisiz akış bildiriminde bir yineleme etiketi sorunu arasında gelebilir. |Daha fazla bilgi için [Bu bölümde](media-services-deliver-content-overview.md#known-issues). |
| Media Services .NET SDK'sı nesneleri seri hale getirilemiyor ve ile Azure önbelleği için Redis sonucunda çalışmaz. |Azure önbelleği için Redis eklemek için SDK'sı AssetCollection nesneyi serileştirmek denerseniz, bir özel durum oluşturulur. |

## <a name="a-idrestversionhistoryrest-api-version-history"></a><a id="rest_version_history"/>REST API Sürüm Geçmişi
Media Services REST API sürüm geçmişi hakkında daha fazla bilgi için bkz. [Azure Media Services REST API'si başvurusu].

## <a name="december-2018"></a>Aralık 2018

Azure Media Services medya Hyperlapse önizleme özelliği yakında kullanımdan kaldırılacaktır. 19 Aralık 2018 tarihinden itibaren Media Services artık değişiklikleri veya geliştirmeleri için medya Hyperlapse hale getirir. 29 Mart 2019, kullanımdan kaldırılmış ve artık kullanılabilir olacaktır.

## <a name="october-2018"></a>Ekim 2018

### <a name="cmaf-support"></a>CMAF desteği

(İOS 11 +) Apple HLS ve MPEG-DASH için CMAF ve 'cbcs' şifreleme desteği CMAF destekleyen oynatıcılar.

### <a name="web-vtt-thumbnail-sprites"></a>Web VTT küçük resim zıplamasını sağlayın

Media Services şimdi v2 Apı'lerimizi kullanarak Web VTT küçük resim hareketli grafikler oluşturmak için de kullanabilirsiniz. Daha fazla bilgi için [bir küçük resim sprite oluşturmak](generate-thumbnail-sprite.md).

## <a name="july-2018"></a>Temmuz 2018

En son hizmet sürümüyle var. küçük değişiklikler nasıl, iki veya daha fazla satırlara ayrılmış olup göre bir işi başarısız olduğunda, hizmet tarafından döndürülen hata iletileri için biçimlendirme

## <a name="may-2018"></a>Mayıs 2018 

Canlı kanallar 12 Mayıs 2018 tarihinden itibaren artık RTP/MPEG-2 aktarım akışı destek alma protokolü. Lütfen RTP/MPEG-2'den RTMP veya parçalanmış MP4'e geçiş (kesintisiz akış) alma protokolleri.

## <a name="october-2017-release"></a>Ekim 2017 sürümü
> [!IMPORTANT] 
> Media Services, Azure Access Control Service kimlik doğrulaması anahtarları desteği kaldırmaktadır. 22 Haziran 2018'de, artık kod aracılığıyla Media Services arka ucu ile Access Control Service tuşlarını kullanarak kimlik doğrulaması yapabilir. Her Azure Active Directory (Azure AD) kullanmak için kodunuzu güncelleştirmeniz gerekir [Azure AD tabanlı kimlik doğrulaması](media-services-use-aad-auth-to-access-ams-api.md). Azure portalında bu değişiklik hakkında uyarılar izleyin.

### <a name="updates-for-october-2017"></a>Ekim 2017 güncelleştirmeleri
#### <a name="sdks"></a>SDK’lar
* .NET SDK'sı, Azure AD kimlik doğrulamasını desteklemek için güncelleştirildi. Access Control Service kimlik doğrulaması için destek nuget.org Azure AD'ye hızlı geçiş teşvik etmek için en son .NET SDK'sı kaldırıldı. 
* JAVA SDK'sı, Azure AD kimlik doğrulamasını desteklemek için güncelleştirildi. Java SDK'sı için Azure AD kimlik doğrulaması için destek eklendi. Media Services ile Java SDK'sını kullanma hakkında daha fazla bilgi için bkz: [Azure Media Services için Java istemci SDK'sı ile çalışmaya başlama](media-services-java-how-to-use.md)

#### <a name="file-based-encoding"></a>Dosya tabanlı kodlama
* Artık Premium kodlayıcı video codec bileşeni (HEVC) kodlama H.265 yüksek verimlilik video içeriğinizi kodlamak için de kullanabilirsiniz. H.264 gibi diğer codec üzerinden H.265 seçerseniz fiyatlandırma etkisi yoktur. HEVC patent lisansları hakkında daha fazla bilgi için bkz: [çevrimiçi hizmet koşulları](https://azure.microsoft.com/support/legal/).
* İOS11 veya GoPro Hero 6 kullanılarak yakalanan görüntü gibi H.265 (HEVC) video codec ile kodlanmış kaynak video için artık Premium Kodlayıcı veya standart Kodlayıcı bu video kodlamak için kullanabilirsiniz. Patent lisansları hakkında daha fazla bilgi için bkz: [çevrimiçi hizmet koşulları](https://azure.microsoft.com/support/legal/).
* Birden çok dil ses parçalarını içeren içerik için dil değerleri (örneğin, ISO MP4) karşılık gelen dosya biçim belirtimine göre doğru şekilde etiketlenmelidir. Ardından, akış içeriği kodlamak için standart kodlayıcı kullanabilirsiniz. Sonuç akış Bulucusu ses diller listelenmiştir.
* Standart Kodlayıcı iki yeni yalnızca ses sistem önayarlarını, "AAC ses" ve "AAC iyi kalite ses." artık desteklemektedir. Her ikisi de (AAC) çıktı, 128 Kb/sn ve 192 Kbps bit hızlarında sırasıyla kodlama ses Gelişmiş stereo üretir.
* Premium Kodlayıcı, giriş olarak artık QuickTime/MOV dosya biçimlerini destekler. Video codec olmalıdır [Apple ProRes türleri bu GitHub makalesinde listelenen](https://docs.microsoft.com/azure/media-services/media-services-media-encoder-standard-formats). Ses ya da AAC veya kod modülasyon (PCM) darbe gerekir. Premium Kodlayıcı, örneğin, giriş olarak QuickTime/MOV dosyalarında sarmalanmış zamanlı kullanılan Cihazlar/DVCPro video desteklemiyor. Standart Kodlayıcı, bu video çözümleyicilerini desteklemiyor.
* Kodlayıcılara aşağıdaki hata düzeltmeleri yapıldı:

    * Artık, bir giriş varlığı kullanarak işleri gönderebilirsiniz. Bu işleri tamamladığınızda, varlık değiştirebilirsiniz (örneğin, eklemek, silmek veya varlık içindeki dosyaları yeniden adlandırma) ve ek göndermek.
    * Standart Kodlayıcı tarafından üretilen JPEG küçük resimleri kalitesini geliştirildi.
    * Standart Kodlayıcı giriş meta verileri ve küçük resim oluşturma süresi çok kısa videoları daha iyi işler.
    * Standart Kodlayıcı kullanılan H.264 kod çözücü geliştirmeleri bazı nadir yapılar ortadan kaldırır. 

#### <a name="media-analytics"></a>Media Analytics
Azure Media Redactor genel olarak kullanılabilir: Bu medya işlemci seçilen kişilerin yüzlerini bulanıklaştırarak anonimleştirme gerçekleştirir ve kamu güvenliği ve haber medya senaryolarında kullanım için idealdir. 

Bu yeni işlemci üzerinde bir genel bakış için bkz [bu blog gönderisini](https://azure.microsoft.com/blog/azure-media-redactor/). Belgeler ve ayarlar hakkında daha fazla bilgi için bkz: [özgürlüğü Azure medya Analizi ile yüzleri](media-services-face-redaction.md).



## <a name="june-2017-release"></a>Haziran 2017 sürümü

Media Services şimdi destekler [Azure AD tabanlı kimlik doğrulaması](media-services-use-aad-auth-to-access-ams-api.md).

> [!IMPORTANT]
> Şu anda Media Services, Access Control Service kimlik doğrulama modeli destekler. Access Control Service yetkilendirme 1 Haziran 2018'de kullanımdan kaldırılacaktır. Azure AD kimlik doğrulaması modeline mümkün olan en kısa sürede geçiş yapmanız önerilir.

## <a name="march-2017-release"></a>Mart 2017 sürümü

Standart Kodlayıcı için artık kullanabilirsiniz [hızı Merdivenini otomatik oluşturma](media-services-autogen-bitrate-ladder-with-mes.md) "Uyarlamalı akış" önceden dize bir kodlama görevi oluşturduğunuzda belirterek. Media Services ile akış için bir video kodlamak için "Uyarlamalı akış" ön ayarını kullanın. Belirli senaryonuz için önceden belirlenmiş bir kodlama özelleştirmek için ile başlayabilirsiniz [bu hazır](media-services-mes-presets-overview.md).

Media Encoder Standard veya Media Encoder Premium iş akışı için artık kullanabilirsiniz [fMP4 öbekleri oluşturan bir kodlama görevi oluşturma](media-services-generate-fmp4-chunks.md). 

## <a name="february-2017-release"></a>Şubat 2017 sürümü

1 Nisan 2017'den itibaren hesabınızdaki 90 günden eski olan tüm iş kayıtları otomatik olarak kendi ilişkili görev kayıtlarıyla birlikte silinir. Toplam kayıt sayısı üst kota sınırının altında olsa bile silme işlemi gerçekleşir. İş/görev bilgilerini arşivlemeniz için açıklanan kodu kullanabilirsiniz [yönetmek, varlıkları ve Media Services .NET SDK'sı ile ilgili öğeleri](media-services-dotnet-manage-entities.md).

## <a name="january-2017-release"></a>Ocak 2017 sürümü

Media Services'de bir akış uç noktası, içeriği doğrudan bir istemci Yürütücü uygulamasına veya başkalarına dağıtım için bir içerik teslim ağı (CDN) teslim eden bir akış hizmetini temsil eder. Media Services, Azure Content Delivery Network sorunsuz tümleştirme de sağlar. Canlı akış, isteğe bağlı bir video veya Media Services hesabı, varlığı bir aşamalı indirme StreamingEndpoint hizmetinin giden akıştan olabilir. Her bir Media Services hesabı bir varsayılan akış uç noktası içerir. Ek akış uç noktaları hesap altında oluşturulabilir. 

Akış uç noktaları, iki sürüm 1.0 ve 2.0. 10 Ocak 2017'den itibaren yeni oluşturulan herhangi bir Media Services hesabı sürüm 2.0 varsayılan akış uç noktası içerir. Bu hesaba eklediğiniz ek akış uç noktaları, sürüm 2.0 de olur. Bu değişiklik, mevcut hesaplarını etkilemez. Mevcut akış uç noktaları, sürüm 1.0 ve 2.0 sürümüne yükseltilebilir. Davranış, faturalama ve bu değişiklik ile özellik değişiklikleri vardır. Daha fazla bilgi için bkz. [Akış uç noktalarına genel bakış](media-services-streaming-endpoints-overview.md).

2,15 sürümünden itibaren Media Services akış uç noktası varlığa aşağıdaki özellikleri eklemiştir:

* CdnProvider 
* CdnProfile
* FreeTrialEndTime 
* StreamingEndpointVersion 

Bu özellikler hakkında daha fazla bilgi için bkz. [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). 

## <a name="december-2016-release"></a>Aralık 2016 sürümü

 Artık, Media Services telemetri/ölçümleri verilerine erişmek için kendi Hizmetleri için kullanabilirsiniz. Akış uç noktası geçerli sürümü, Media Services Canlı kanal telemetri verilerini toplamak için kullanın ve varlıkları arşivleyin. Daha fazla bilgi için [Media Services telemetri](media-services-telemetry-overview.md).

## <a name="a-idjulychanges16july-2016-release"></a><a id="july_changes16"/>Temmuz 2016 sürümü
### <a name="updates-to-the-manifest-file-ism-generated-by-encoding-tasks"></a>Güncelleştirmeleri için bildirim dosyası (*. ISM), kodlama görevlerine tarafından oluşturulan
Bir kodlama görevi Media Encoder Standard veya Medya Kodlayıcısı Premium gönderildiğinde kodlama görevinin oluşturduğu bir [akış bildirim dosyası](media-services-deliver-content-overview.md) (* .ism) çıktı varlığının içinde. En son hizmet sürümle birlikte, bu akış bildirimi dosyasının söz dizimi güncelleştirildi.

> [!NOTE]
> Akış bildirimi (.ism) dosyasında söz dizimi, iç kullanım için ayrılmıştır. Sonraki sürümlerde tabidir. Değiştirmeyin veya bu dosyanın içeriğini düzenleme.
> 
> 

### <a name="a-new-client-manifest-ismc-file-is-generated-in-the-output-asset-when-an-encoding-task-outputs-one-or-more-mp4-files"></a>Yeni bir istemci bildirimi (*. Bir kodlama görevi bir veya daha fazla MP4 dosyaları yaptığında çıktı varlığı ISMC) dosyası oluşturulur
Bir veya daha fazla MP4 dosyaları oluşturan bir kodlama görevi tamamlandıktan sonra en son hizmet sürümünden başlayarak çıktı varlığına bir akış istemci bildirimi (*.ismc) dosyası da içerir. .İsmc dosya dinamik akış performansını artırır. 

> [!NOTE]
> İstemci bildirimi (.ismc) dosyası sözdizimi, iç kullanım için ayrılmıştır. Sonraki sürümlerde tabidir. Değiştirmeyin veya bu dosyanın içeriğini düzenleme.
> 
> 

Daha fazla bilgi için [bu bloga](https://blogs.msdn.microsoft.com/randomnumber/2016/07/08/encoder-changes-within-azure-media-services-now-create-ismc-file/) bakın.

### <a name="known-issues"></a>Bilinen sorunlar
Bazı istemciler, kesintisiz akış bildiriminde bir yineleme etiketi sorunu arasında gelebilir. Daha fazla bilgi için [Bu bölümde](media-services-deliver-content-overview.md#known-issues).

## <a id="apr_changes16"></a>Nisan 2016 sürümü
### <a name="media-analytics"></a>Media Analytics
 Media Services medya analizi için güçlü bir video zeka kullanıma sunuldu. Daha fazla bilgi için [Media Services Analizi'ne genel bakış](media-services-analytics-overview.md).

### <a name="apple-fairplay-preview"></a>Apple FairPlay (Önizleme)
Media Services şimdi dinamik olarak HTTP canlı akış (HLS) Apple FairPlay ile içerik şifrelemek için de kullanabilirsiniz. FairPlay lisansları istemcilere sunmak için Media Services lisans teslimat hizmeti de kullanabilirsiniz. Daha fazla bilgi için "Kullanın. Apple FairPlay ile korunan HLS içeriğinizin akışını sağlamak için Azure Media Services" konusuna bakın.

## <a id="feb_changes16"></a>Şubat 2016 sürümü
Google Widevine ile ilgili bir hata düzeltmesi (3.5.3) .NET için Media Services SDK'sını en son sürümünü içerir. Widevine ile şifrelenmiş birden çok varlığı için AssetDeliveryPolicy yeniden mümkün oldu. Bu hata düzeltmenin bir parçası olarak, aşağıdaki özellik için SDK'sı eklendi: WidevineBaseLicenseAcquisitionUrl.

    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
    {
        {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl,"http://testurl"},

    };

## <a id="jan_changes_16"></a>Ocak 2016 sürümü
Encoding'e ayrılan birimleri Kodlayıcı adları karıştırılıyor azaltmak için değiştirilmiştir.

Temel, standart ve Premium encoding'e ayrılan birimleri yeniden adlandırılan S1, S2, ve S3 ayrılmış birimleri, sırasıyla. Temel encoding'e ayrılan birimleri bugün kullanan müşteriler, Azure portalında (ve fatura) etiketi olarak S1 bakın. Standart ve Premium kullanan müşteriler, S2 ve S3, etiketlerin sırasıyla bakın. 

## <a id="dec_changes_15"></a>Aralık 2015 sürümü

### <a name="media-encoder-deprecation-announcement"></a>Medya Kodlayıcısı kullanım dışı bırakma Duyurusu

 Medya Kodlayıcısı yaklaşık 12 ay sonra Media Encoder Standard'ın sürümünden itibaren kullanımdan kaldırılacaktır.

### <a name="azure-sdk-for-php"></a>PHP için Azure SDK
Azure SDK'sı ekibi, yeni bir sürümü yayımlanan [PHP için Azure SDK'sı](https://github.com/Azure/azure-sdk-for-php) Media Services için güncelleştirmeleri ve yeni özellikleri içeren paket. Özellikle, PHP için Media Services SDK'sını en son artık destekliyor [içerik koruma](media-services-content-protection-overview.md) özellikleri. Bu özellikler, belirteç kısıtlamaları olmadan ve dinamik şifreleme ile AES ve DRM (PlayReady ve Widevine) özelliklerdir. Ayrıca ölçeklendirme destekler [birimleri kodlama](media-services-dotnet-encoding-units.md).

Daha fazla bilgi için bkz.

* Aşağıdaki [kod örnekleri](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices) hızlıca çalışmaya başlamanıza yardımcı olur:
  * **vodworkflow_aes.php**: Bu PHP dosya AES-128 dinamik şifreleme ve anahtar dağıtımı hizmetiyle nasıl kullanılacağını gösterir. .NET örnek bölümünde açıklanan dayanır [kullanım AES-128 dinamik şifreleme ve anahtar dağıtımı hizmetiyle](media-services-protect-with-aes128.md).
  * **vodworkflow_aes.php**: Bu PHP dosya PlayReady dinamik şifreleme ve lisans teslimat hizmetinin nasıl kullanılacağını gösterir. .NET örnek bölümünde açıklanan dayanır [kullanım PlayReady ve/veya Widevine dinamik ortak şifreleme](media-services-protect-with-playready-widevine.md).
  * **scale_encoding_units.php**: Bu PHP dosya encoding'e ayrılan birimleri ölçeklendirme işlemi gösterilmektedir.

## <a id="nov_changes_15"></a>Kasım 2015 sürümü
 Media Services, artık bulutta Widevine lisans teslim hizmeti sunar. Daha fazla bilgi için [bu bloga](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/) bakın. Ayrıca bkz [Bu öğreticide](media-services-protect-with-playready-widevine.md) ve [GitHub deposu](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm). 

Media Services tarafından sağlanan Widevine lisans teslim hizmetleri Önizleme aşamasındadır. Daha fazla bilgi için [bu bloga](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/) bakın.

## <a id="oct_changes_15"></a>Ekim 2015 sürümü
Media Services şu veri merkezlerinde çalışıyor: Brezilya Güney, Hindistan Batı, Hindistan Güney ve Hindistan orta. Artık Azure portalında kullanabilirsiniz [Media Service hesapları oluşturmak](media-services-portal-create-account.md) ve açıklanan çeşitli görevleri [Media Services belgeleri Web sayfasını](https://azure.microsoft.com/documentation/services/media-services/). Live Encoding bu veri merkezlerinde etkin değil. Ayrıca, bu veri merkezlerinde kodlama ayrılmış birimlerimizden tüm türleri kullanılabilir.

* Brezilya Güney:                                          Yalnızca standart ve temel kodlamaya ayrılan birimler kullanılabilir.
* Hindistan Batı, Hindistan Güney ve Hindistan Orta:             Yalnızca temel encoding'e ayrılan birimleri kullanılabilir.

## <a id="september_changes_15"></a>Eylül 2015 sürümü
Media Services şimdi her iki video isteğe bağlı ve canlı akışlarınızdan Widevine modüler DRM teknolojisi ile koruma olanağı sunar. Widevine lisansları teslim etmenize yardımcı olması için aşağıdaki teslimat Hizmetleri iş ortakları kullanabilirsiniz:
* [Axinom](https://www.axinom.com/press/ibc-axinom-drm-6/) 
* [EZDRM](https://ezdrm.com/) 
* [castLabs](https://castlabs.com/company/partners/azure/) 

Daha fazla bilgi için [bu bloga](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) bakın.
  
AssetDeliveryConfiguration’ı Widevine kullanacak şekilde yapılandırmak için [Media Services .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (3.5.1 sürümünden başlayarak) veya REST API'yi kullanabilirsiniz. 
* Media Services, Apple ProRes videolar için destek eklendi. Artık Apple ProRes veya diğer codec bileşenleri kullanan QuickTime kaynak videoları dosyalarınızı karşıya yükleyebilirsiniz. Daha fazla bilgi için [bu bloga](https://azure.microsoft.com/blog/announcing-support-for-apple-prores-videos-in-azure-media-services/) bakın.
* Artık Media Encoder Standard subclipping ve canlı arşiv ayıklama yapmak için kullanabilirsiniz. Daha fazla bilgi için [bu bloga](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/) bakın.
* Aşağıdaki filtreleme güncelleştirmeler yapılmıştır: 
  
  * Bu gibi durumlarda, Apple HLS biçimi artık yalnızca ses Filtresi ile kullanabilirsiniz. Bu güncelleştirme belirterek bir yalnızca ses parçası kaldırmak için kullanabilirsiniz (yalnızca ses = false) bir URL.
  * Varlıklarınız için filtreler tanımladığınızda, artık birden çok birleştirebilirsiniz tek bir URL (en fazla üç) filtreleri.
    
    Daha fazla bilgi için [bu bloga](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) bakın.
* Media Services şimdi HLS sürüm 4 ' ı-çerçeveleri de destekler. Ben-çerçeve destek, İleri ve geri sarma operations iyileştirir. Varsayılan olarak, tüm HLS sürüm 4 çıkışları ben-çerçeve çalma listesi (EXT-X-I-FRAME-STREAM-INF) içerir.
Daha fazla bilgi için [bu bloga](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) bakın.

## <a id="august_changes_15"></a>Ağustos 2015 sürümü
* Java Sürüm 0.8.0 ve yeni örnekler için Media Services SDK'sını kullanıma sunuldu. Daha fazla bilgi için bkz.
    
* Azure Media Player, birden çok ses akışı desteğiyle güncelleştirildi. Daha fazla bilgi için [bu blog gönderisini](https://azure.microsoft.com/blog/2015/08/13/azure-media-player-update-with-multi-audio-stream-support/).

## <a id="july_changes_15"></a>Temmuz 2015 sürümü
* Media Encoder Standard genel kullanılabilirliğini Duyuruldu. Daha fazla bilgi için [bu blog gönderisini](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/).
  
    Media Encoder Standard kullandığı hazır açıklandığı [Bu bölümde](https://go.microsoft.com/fwlink/?LinkId=618336). 4 K için önceden ayarlanmış kullanırken kodlar, Premium ayrılmış birim türünü Al. Daha fazla bilgi için [ölçek kodlama](media-services-scale-media-processing-overview.md).
* Canlı gerçek zamanlı açıklamalı alt yazılar, Media Services Media Player ile kullanıldı. Daha fazla bilgi için [bu blog gönderisini](https://azure.microsoft.com/blog/2015/07/08/live-real-time-captions-with-azure-media-services-and-player/).

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK'sını güncelleştirme
Media Services .NET SDK'sı sürüm 3.4.0.0 sunulmuştur. Aşağıdaki güncelleştirmeler yapılmıştır: 

* Destek için Canlı arşiv uygulanmıştır. Canlı arşiv içeren bir varlık karşıdan yükleyemiyor.
* Destek için dinamik filtre uygulanmıştır.
* Bunlar bir varlığı silme sırada kullanıcılar bir depolama kapsayıcısı olan işlevler uygulanmıştır.
* Hata düzeltmeleri yeniden deneme ilkelerine kanalda ilgili yapıldı.
* Media Encoder Premium iş akışı etkinleştirildi.

## <a id="june_changes_15"></a>Haziran 2015 sürümü
### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK'sını güncelleştirme
Media Services .NET SDK'sı sürüm 3.3.0.0 sunulmuştur. Aşağıdaki güncelleştirmeler yapılmıştır: 

* Openıd Connect bulma özelliği için destek eklendi.
* Kimlik sağlayıcısı tarafında anahtarları geçişi işlemek için destek eklendi.

Bir Openıd Connect bulma belgesi kullanıma sunan bir kimlik sağlayıcısı kullanıyorsanız (Azure AD, Google ve Salesforce'da yapın), Media Services'ın Openıd Connect bulma spec JSON Web belirteçleri (Jwt'ler) doğrulama için imzalama anahtarları edinme bildirebilirsiniz. 

Daha fazla bilgi için [JWT kimlik doğrulaması Media Services ile çalışmak için Openıd Connect bulma özelliği kullanım JSON web anahtarları](http://gtrifonov.com/2015/06/07/using-json-web-keys-from-openid-connect-discovery-spec-to-work-with-jwt-token-authentication-in-azure-media-services/).

## <a id="may_changes_15"></a>Mayıs 2015 sürümü
Aşağıdaki yeni özellikler duyurduk:

* [Media Services ile gerçek zamanlı kodlama, Önizleme](media-services-manage-live-encoder-enabled-channels.md)
* [Dinamik bildirimi](media-services-dynamic-manifest-overview.md)

## <a id="april_changes_15"></a>Nisan 2015 sürümü
### <a name="general-media-services-updates"></a>Genel medya Hizmetleri güncelleştirmeleri
* [Media Player](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/) Duyuruldu.
* Media Services REST 2.10 ile başlayarak, bir gerçek zamanlı Mesajlaşma Protokolü (RTMP) alacak şekilde yapılandırılan kanallar birincil ile oluşturulur ve ikincil URL'lerini alın. Daha fazla bilgi için [kanal yapılandırmalarını alma](media-services-live-streaming-with-onprem-encoders.md#channel_input).
* Azure Media Indexer'ın güncelleştirildi.
* İspanyolca dil desteği eklendi.
* XML biçimi için yeni bir yapılandırma eklendi.

Daha fazla bilgi için [bu bloga](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/) bakın.

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK'sını güncelleştirme
Media Services .NET SDK sürüm 3.2.0.0 sunulmuştur. Aşağıdaki güncelleştirmeler yapılmıştır:

* Yeni değişiklik: Bir dize türünde olmasını TokenRestrictionTemplate.Issuer ve TokenRestrictionTemplate.Audience değiştirildi.
* Güncelleştirmeleri, özel bir yeniden deneme ilkeleri oluşturmak için ilgili yapıldı.
* Hata düzeltmeleri dosya yükleme ve indirme için ilgili yapıldı.
* MediaServicesCredentials sınıfı artık karşı kimlik doğrulaması yapmak için birincil ve ikincil access control uç noktaların kabul eder.

## <a id="march_changes_15"></a>Mart 2015 sürümü
### <a name="general-media-services-updates"></a>Genel medya Hizmetleri güncelleştirmeleri
* Media Services şimdi Content Delivery Network tümleştirme sağlar. Tümleştirmeyi desteklemek için için StreamingEndpoint CdnEnabled özelliği eklendi. CdnEnabled 2.9 sürümünden başlayarak REST API'leri ile kullanılabilir. Daha fazla bilgi için [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). CdnEnabled 3.1.0.2 sürümünden itibaren .NET SDK ile kullanılabilir. Daha fazla bilgi için [StreamingEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.istreamingendpoint\(v=azure.10\).aspx).
* Media Encoder Premium iş akışı Duyuruldu. Daha fazla bilgi için [Premium Karşınızda Azure Media Services kodlama](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/).

## <a id="february_changes_15"></a>Şubat 2015 sürümü
### <a name="general-media-services-updates"></a>Genel medya Hizmetleri güncelleştirmeleri
Media Services REST API sürüm 2.9 sunulmuştur. Bu sürüm ile başlayarak, akış uç noktaları ile Content Delivery Network tümleştirmesini etkinleştirebilirsiniz. Daha fazla bilgi için [StreamingEndpoint](https://msdn.microsoft.com/library/dn783468.aspx).

## <a id="january_changes_15"></a>Ocak 2015 sürümü
### <a name="general-media-services-updates"></a>Genel medya Hizmetleri güncelleştirmeleri
Content protection dinamik şifreleme ile genel kullanılabilirliğini Duyuruldu. Daha fazla bilgi için [Media Services akış güvenlik DRM teknolojisi genel kullanılabilirliğini geliştiren](https://azure.microsoft.com/blog/2015/01/29/azure-media-services-enhances-streaming-security-with-general-availability-of-drm-technology/).

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK'sını güncelleştirme
Media Services .NET SDK'sı sürüm 3.1.0.1 sunulmuştur.

Bu sürüm, varsayılan Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization.TokenRestrictionTemplate Oluşturucu artık kullanılmıyor olarak işaretlenmiş. Yeni oluşturucuyu TokenType bağımsız değişken olarak alır.

    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);


## <a id="december_changes_14"></a>Aralık 2014 sürümü
### <a name="general-media-services-updates"></a>Genel medya Hizmetleri güncelleştirmeleri
* Media Indexer için bazı güncelleştirmeleri ve yeni özellikler eklendi. Daha fazla bilgi için [Azure Media Indexer'ın sürüm 1.1.6.7 Sürüm Notları](https://azure.microsoft.com/blog/2014/12/03/azure-media-indexer-version-1-1-6-7-release-notes/).
* Yeni bir REST API encoding'e ayrılan birimleri güncelleştirmek için kullanabileceğiniz eklendi. Daha fazla bilgi için [REST ile EncodingReservedUnitType](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype).
* Anahtar teslim hizmeti için CORS desteği eklendi.
* Yetkilendirme İlkesi seçenekleri sorgulama için performans geliştirmeleri yapıldı.
* Çin veri merkezindeki [anahtar teslim URL](https://docs.microsoft.com/rest/api/media/operations/contentkey#get_delivery_service_url) müşteri başına sunulmuştur (diğer veri merkezlerinde olduğu gibi).
* HLS otomatik hedef süresi eklendi. Canlı akış yaparken HLS her zaman dinamik olarak paketlenir. Media Services, varsayılan olarak, ana kare aralığına (KeyFrameInterval) dayalı HLS segment paketleme oranı (FragmentsPerSegment) otomatik olarak hesaplar. Bu yöntem aynı zamanda gerçek zamanlı Kodlayıcı alınan bir grubu (GOP) resimlerin verilir. Daha fazla bilgi için [iş Media Services ile canlı akış](https://msdn.microsoft.com/library/azure/dn783466.aspx).

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK'sını güncelleştirme
[Media Services .NET SDK'sı](https://www.nuget.org/packages/windowsazure.mediaservices/) artık 3.1.0.0 sürümüdür. Aşağıdaki güncelleştirmeler yapılmıştır:

* .NET SDK'sı bağımlılık, .NET 4.5 Framework yükseltildi.
* Encoding'e ayrılan birimleri güncelleştirmek için kullanabileceğiniz yeni bir API eklendi. Daha fazla bilgi için [güncelleştirme ayrılmış birim türünü ve .NET kullanarak kodlama ayrılmış birimlerimizden artışı](media-services-dotnet-encoding-units.md).
* JWT belirteci kimlik doğrulaması desteği eklendi. Daha fazla bilgi için [JWT belirteci kimlik doğrulaması Media Services ve dinamik şifreleme](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).
* PlayReady lisans şablonu BeginDate ve ExpirationDate göreli farklarını eklendi.

## <a id="november_changes_14"></a>Kasım 2014 sürümü
* Media Services şimdi bir SSL bağlantısı üzerinden Canlı kesintisiz akış (fMP4) içerik almak için de kullanabilirsiniz. SSL üzerinden alımı için alma URL'si için HTTPS güncelleştirdiğinizden emin olun. Şu anda, Media Services, SSL ile özel etki alanlarını desteklemiyor. Canlı akış hakkında daha fazla bilgi için bkz. [iş Azure Media Services canlı akış ile](https://msdn.microsoft.com/library/azure/dn783466.aspx).
* Şu anda bir SSL bağlantısı üzerinden RTMP canlı akış içe olamaz.
* İçeriğinizi teslim etmek istediğiniz akış uç noktası 10 Eylül 2014'ten sonra yalnızca oluşturulduysa, SSL üzerinden akışını yapabilirsiniz. URL "streaming.mediaservices.windows.net" (yeni biçimde) varsa, akış URL'leri 10 Eylül 2014'ten sonra oluşturulan akış uç noktalarını dayanır. "Origin.mediaservices.windows.net" (eski biçimde) içeren akış URL'leri SSL desteklemez. URL'NİZİN biçimi eski olduğundan ve SSL üzerinden akışla aktarmak istiyorsanız [yeni bir akış uç noktası oluşturma](media-services-portal-manage-streaming-endpoints.md). SSL üzerinden içeriğinizin akışını sağlamak için yeni akış uç noktasına göre URL'leri kullanın.

### <a id="oct_sdk"></a>Media Services .NET SDK'sı
.NET uzantıları için Media Services SDK'sı sürüm 2.0.0.3 sunulmuştur.

.NET için Media Services SDK'sı sürüm 3.0.0.8 sunulmuştur. Aşağıdaki güncelleştirmeler yapılmıştır:

* Yeniden düzenleme, yeniden deneme ilkesi sınıflarda uygulanmıştır.
* Bir kullanıcı aracısı dizesi için HTTP istek üstbilgilerinin eklendi.
* Bir NuGet geri yükleme derleme adımı eklendi.
* Senaryo testleri sorunlar giderildi x509 kullanmak için sertifika deposundan.
* Kanalı ve akış uç güncelleştirdiğinizde doğrulama ayarları için eklendi.

### <a name="new-github-repository-to-host-media-services-samples"></a>Yeni GitHub deposuna konak Media Services örnekleri
Örnekleri olan [Media Services örnekleri GitHub deposunda](https://github.com/Azure/Azure-Media-Services-Samples).

## <a id="september_changes_14"></a>Eylül 2014 sürümü
Media Services REST meta verileri sürüm 2.7 sunulmuştur. KALAN güncelleştirmeleri hakkında daha fazla bilgi için bkz. [Media Services REST API Başvurusu](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).

.NET için Media Services SDK'sı sürüm 3.0.0.7 sunulmuştur

### <a id="sept_14_breaking_changes"></a>Bozucu değişiklikler
* Kaynak adlandırıldı [StreamingEndpoint].
* Kodlamak ve ardından yayımlama MP4 dosyaları için Azure portalını kullanırken varsayılan davranış bir değişiklik yapılmıştır.

### <a id="sept_14_GA_changes"></a>Yeni özellikler/genel kullanım sürümünde parçası olan senaryoları
* Media Indexer medya işleyicisini kullanıma sunulmuştur. Daha fazla bilgi için [Media Indexer ile medya dosyalarının dizinini](https://msdn.microsoft.com/library/azure/dn783455.aspx).
* Kullanabileceğiniz [StreamingEndpoint] (ana) özel etki alanı adlarını eklenecek varlık.
  
    Media Services akış uç noktası adı bir özel etki alanını kullanmak için akış uç noktanızı özel konak adlarını ekleyin. Özel ana bilgisayar adları eklemek için Media Services REST API'leri veya .NET SDK'sını kullanın.
  
    Aşağıdaki maddeler geçerlidir:
  
  * Özel etki alanı adı sahipliğini olması gerekir.
  * Etki alanı sahipliğini Media Services tarafından doğrulanması gerekir. Etki alanını doğrulamak için DNS mediaservices dns bölgesi doğrulamak için MediaServicesAccountId üst etki alanına eşleyen bir CName oluşturun.
  * Medya Hizmetleri StreamingEndpoint ana bilgisayar adı (örneğin, amstest.streaming.mediaservices.windows.net) özel bir ana bilgisayar adı (örneğin, sports.contoso.com) eşleşen başka bir CName oluşturmanız gerekir.

    Daha fazla bilgi için bkz: CustomHostNames özelliğinde [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx) makalesi.

### <a id="sept_14_preview_changes"></a>Yeni özellikler/genel Önizleme sürümü parçası olan senaryoları
* Canlı akış Önizleme. Daha fazla bilgi için [iş Media Services ile canlı akış](https://msdn.microsoft.com/library/azure/dn783466.aspx).
* Anahtar teslim hizmeti. Daha fazla bilgi için [kullanım AES-128 dinamik şifreleme ve anahtar dağıtımı hizmetiyle](https://msdn.microsoft.com/library/azure/dn783457.aspx).
* AES dinamik şifreleme. Daha fazla bilgi için [kullanım AES-128 dinamik şifreleme ve anahtar dağıtımı hizmetiyle](https://msdn.microsoft.com/library/azure/dn783457.aspx).
* PlayReady lisans teslimat hizmeti. 
* PlayReady dinamik şifreleme. 
* Media Services PlayReady lisans şablonu. Daha fazla bilgi için [Media Services PlayReady lisans şablonuna genel bakış].
* Stream varlıklar depolama şifrelenir. Daha fazla bilgi için [Stream depolama ile şifrelenmiş içeriklerin](https://msdn.microsoft.com/library/azure/dn783451.aspx).

## <a id="august_changes_14"></a>Ağustos 2014 sürümü
Bir varlığı kodlama, kodlama işi tamamlandığında, çıktı varlık oluşturulur. Bu sürüm kadar medya Hizmetleri Kodlayıcı çıktı varlığı hakkındaki meta verileri oluşturdu. Bu sürüm ile başlayarak, kodlayıcı giriş varlıklar hakkındaki meta verileri de oluşturur. Daha fazla bilgi için [giriş meta verileri] ve [çıkış meta verileri].

## <a id="july_changes_14"></a>Temmuz 2014 sürümü
Azure Media Services Paketleyici ve Şifreleyici için aşağıdaki hata düzeltmeleri yapıldı:

* Canlı arşiv varlık HLS için iletilirken yalnızca ses geri yürütülür: Bu sorunu düzeltildi ve artık hem ses ve video yürütebilirsiniz.
* Bir varlık HLS ve AES 128 bit Zarf şifreleme paketlendiğinde paketlenmiş akışları Android cihazlarda kayıttan yürütme yok: Bu hatanın düzeltilip ve paketlenmiş akış HLS destekleyen Android cihazlarda geri çalar.

## <a id="may_changes_14"></a>Mayıs 2014 sürümü
### <a id="may_14_changes"></a>Genel medya Hizmetleri güncelleştirmeleri
Artık [dinamik paketleme] akışına HLS sürüm 3. Akış HLS sürüm 3 için aşağıdaki biçimi için Kaynak Konum Belirleyicisi yolu ekleyin: * .ism/manifest(format=m3u8-aapl-v3). Daha fazla bilgi için [bu Forumu](https://social.msdn.microsoft.com/Forums/en-US/13b8a776-9519-4145-b9ed-d2b632861fde/dynamic-packaging-to-hls-v3).

Dinamik paketleme şimdi de statik olarak PlayReady şifreli kesintisiz akış göre PlayReady ile şifrelenmiş HLS (sürüm 3 ve sürüm 4) destekler. Kesintisiz akış PlayReady ile şifreleme hakkında daha fazla bilgi için bkz: [PlayReady ile kesintisiz akış korumak](https://msdn.microsoft.com/library/azure/dn189154.aspx).

### <a name="may_14_donnet_changes"></a>Media Services .NET SDK'sını güncelleştirme
Media Services .NET SDK'sı sürüm 3.0.0.5 sunulmuştur. Aşağıdaki güncelleştirmeler yapılmıştır:

* Karşıya yükleyin ve medya varlıklarının indirme hızı ve dayanıklılığı iyidir.
* Yeniden deneme mantığı ve geçici özel durum işlemesi iyileştirmeler yapılmıştır: 
  
  * Geçici hata algılama ve yeniden deneme mantığı, sorgu, değişiklikleri kaydetmek ve dosyaları karşıya veya karşıdan yükleyen oluşan özel durumlar için geliştirilmiş. 
  * Web özel durumlar (örneğin, bir Access Control Service belirteç isteği sırasında) aldığınızda, önemli hataları daha hızlı artık başarısız.

Daha fazla bilgi için [.NET için Media Services SDK'sı mantığı yeniden deneyin].

## <a id="jan_feb_changes_14"></a>Ocak/Şubat 2014 sürümleri
### <a name="jan_fab_14_donnet_changes"></a>Media Services .NET SDK'sı 3.0.0.1 ve 3.0.0.2 3.0.0.3
3.0.0.1 ve 3.0.0.2 değişiklikleri içerir:

* OrderBy ifadelerle LINQ sorgularını kullanımı ile ilgili sorunlar düzeltilmiştir.
* Çözümleri test [GitHub] birim tabanlı testler ve senaryo tabanlı testler'larına.

Değişiklikler hakkında daha fazla bilgi için bkz. [3.0.0.1 ve 3.0.0.2 Media Services .NET SDK sürümleri](http://gtrifonov.com/2014/02/07/windows-azure-media-services-net-sdk-3-0-0-2-release/index.html).

3.0.0.3 sürümünde aşağıdaki değişiklikler yapılmıştır:

* Azure depolama bağımlılıklarını 3.0.3.0 sürümünü kullanacak şekilde yükseltildi.
* 3.0 için geriye dönük uyumluluk sorunu düzeltildi. *.* serbest bırakır.

## <a id="december_changes_13"></a>Aralık 2013 sürümü
### <a name="dec_13_donnet_changes"></a>Media Services .NET SDK 3.0.0.0
> [!NOTE]
> 3.0.x.x yayınlar 2.4.x.x sürümleriyle geriye dönük olarak uyumlu değildir.
> 
> 

Media Services SDK'sını en son sürümünü 3.0.0.0 sunulmuştur. Nuget'ten en son paketini indirin veya bitten alma [GitHub].

Media Services SDK sürüm 3.0.0.0 başlatma, yeniden kullanabileceğiniz [Azure AD Access Control Service](https://msdn.microsoft.com/library/hh147631.aspx) belirteçleri. Daha fazla bilgi için bkz: "Yeniden Access Control Service belirteçler" bölümünde [.NET için Media Services SDK'sı ile Media Services'e bağlanma](https://msdn.microsoft.com/library/azure/jj129571.aspx).

### <a name="dec_13_donnet_ext_changes"></a>Media Services .NET SDK uzantıları 2.0.0.0
 Media Services .NET SDK uzantıları, genişletme yöntemleri ve kodunuzu basitleştirin ve Media Services ile geliştirmek daha kolay hale getirmek için yardımcı işlevleri kümesidir. En son bitten alabilirsiniz [Media Services .NET SDK uzantıları](https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev).

## <a id="november_changes_13"></a>Kasım 2013 sürümü
### <a name="nov_13_donnet_changes"></a>Media Services .NET SDK değişiklikleri
Bu sürüm, Media Services REST API'si katmanına çağrı yapıldığında oluşabilecek .NET tanıtıcıları geçici hata hatalar için Media Services SDK ile başlatılıyor.

## <a id="august_changes_13"></a>Ağustos 2013 sürümü
### <a name="aug_13_powershell_changes"></a>Medya Hizmetleri PowerShell cmdlet'leri dahil Azure SDK Araçları
Aşağıdaki medya Hizmetleri PowerShell cmdlet'leri artık dahil edilen [Azure SDK Araçları](https://github.com/Azure/azure-sdk-tools):

* Get-AzureMediaServices 

    Örneğin, `Get-AzureMediaServicesAccount`
* New-AzureMediaServicesAccount 
  
    Örneğin, `New-AzureMediaServicesAccount -Name “MediaAccountName” -Location “Region” -StorageAccountName “StorageAccountName”`
* New-AzureMediaServicesKey 
  
    Örneğin, `New-AzureMediaServicesKey -Name “MediaAccountName” -KeyType Secondary -Force`
* Remove-AzureMediaServicesAccount 
  
    Örneğin, `Remove-AzureMediaServicesAccount -Name “MediaAccountName” -Force`

## <a id="june_changes_13"></a>Haziran 2013 sürümü
### <a name="june_13_general_changes"></a>Media Services değişiklikleri
Bu bölümde bahsedilen aşağıdaki değişiklikler Haziran 2013 Media Services sürümlerinde bulunan güncelleştirmeler şunlardır:

* Birden çok depolama hesabında bir medya hizmeti hesabı için bağlantı olanağı. 
    * StorageAccount
    * Asset.StorageAccountName ve Asset.StorageAccount
* Job.Priority güncelleştirme olanağı. 
* Bildirim ilgili varlıkları ve özellikleri: 
    * JobNotificationSubscription
    * NotificationEndPoint
    * İş
* Asset.Uri 
* Locator.Name 

### <a name="june_13_dotnet_changes"></a>Media Services .NET SDK değişiklikleri
Aşağıdaki değişiklikleri dahil edilen Haziran 2013'te, Media Services SDK'sı serbest bırakır. En son Media Services SDK'sını Github'da kullanılabilir.

* Sürüm 2.3.0.0 ile başlayarak, birden çok depolama bağlama Media Services SDK'sını destekleyen bir Media Services hesabına hesaplar. Bu özellik aşağıdaki API'leri destekler:
  
    * IStorageAccount türü
    * Microsoft.WindowsAzure.MediaServices.Client.CloudMediaContext.StorageAccounts property
    * Depolama hesabı özelliği
    * StorageAccountName özelliği
  
      Daha fazla bilgi için [birden çok depolama hesapları arasında Media Services'ı Yönet varlıklar](https://msdn.microsoft.com/library/azure/dn271889.aspx).
* Bildirim ile ilgili API'ler. 2.2.0.0 sürümünden itibaren Azure kuyruk depolama bildirimleri dinleyebilirsiniz. Daha fazla bilgi için [işleyecek medya Hizmetleri iş bildirimleri](https://msdn.microsoft.com/library/azure/dn261241.aspx).
  
    * Microsoft.WindowsAzure.MediaServices.Client.IJob.JobNotificationSubscriptions property
    * Microsoft.WindowsAzure.MediaServices.Client.INotificationEndPoint type
    * Microsoft.WindowsAzure.MediaServices.Client.IJobNotificationSubscription type
    * Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointCollection type
    * Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointType type
* Depolama istemci SDK 2.0 (Microsoft.WindowsAzure.StorageClient.dll) üzerinde bağımlılık
* OData 5.5 (Microsoft.Data.OData.dll) bağımlılığı

## <a id="december_changes_12"></a>Kasım 2012 yayın
### <a name="dec_12_dotnet_changes"></a>Media Services .NET SDK değişiklikleri
* IntelliSense: IntelliSense belgelerini eksik birçok türü için eklendi.
* Microsoft.Practices.TransientFaultHandling.Core: Hala eski bir sürümü bu derleme için bir bağımlılık burada SDK b bir sorun düzeltildi. SDK'sı, artık bu derlemenin sürüm 5.1.1209.1 başvuruyor.

Kasım 2012'de SDK bulundu sorunlarını giderir:

* IAsset.Locators.Count: Tüm bulucular silindikten sonra bu sayı artık yeni IAsset arabirimleri üzerinde doğru şekilde bildirilir.
* IAssetFile.ContentFileSize: Bu değer artık düzgün bir şekilde karşıya yükleme IAssetFile.Upload(filepath) tarafından ayarlanır.
* IAssetFile.ContentFileSize: Bir varlık dosyası oluşturduğunuzda bu özellik artık ayarlayabilirsiniz. Daha önce yalnızca okunduktan.
* IAssetFile.Upload(filepath): Varlık için birden çok dosya karşıya yüklendi, bu zaman uyumlu karşıya yükleme yöntemi aşağıdaki hata nerede atma olan bir sorun düzeltildi. Hata: "Sunucu isteğin kimliğini doğrulayamadı. Yetkilendirme üst bilgisi değeri doğru imza dahil olmak üzere oluşturulmuş emin olun."
* IAssetFile.UploadAsync: Bir sorun dosyalarını beş dosyalarına eşzamanlı karşıya yüklenmesi sınırlı düzeltildi.
* IAssetFile.UploadProgressChanged: Bu olay artık SDK'sı tarafından sağlanır.
* IAssetFile.DownloadAsync (dize, BlobTransferClient, ILocator, CancellationToken): Bu yöntem aşırı yüklemesi artık sağlanmaktadır.
* IAssetFile.DownloadAsync: Bir sorun dosyalarını beş dosyalarına eşzamanlı indirilmesini sınırlı düzeltildi.
* IAssetFile.Delete(): Burada hiçbir dosya için IAssetFile yüklendi, arama silme bir özel durum oluşturabilecek bir sorun düzeltildi.
* İşler: Burada bir proje şablonu kullanarak bir "MP4'e kesintisiz akışlara görev" ile "PlayReady koruma görevi" zincirleme görev hiç oluşturmamışsınızdır bir sorun düzeltildi.
* EncryptionUtils.GetCertificateFromStore(): Bu yöntem artık sertifika yapılandırma sorunlarını dayanan sertifikayı bulma hatası nedeniyle bir null başvurusu özel durumu oluşturur.

## <a id="november_changes_12"></a>Kasım 2012 yayın
Bu bölümde belirtilen değişiklikleri güncelleştirmeler Kasım 2012'de (sürüm 2.0.0.0) dahil olan SDK. SDK sürüm yazılan veya değiştirilecek Haziran 2012 Önizleme için yazılan kodlar bu değişiklikler gerektirebilir.

* Varlıklar
  
    * IAsset.Create(assetName) olduğu *yalnızca* varlık oluşturma işlevi. IAsset.Create, artık dosyaları yöntem çağrısının bir parçası olarak yükler. IAssetFile yüklemek için kullanın.
    * Hizmetleri SDK'sından IAsset.Publish yöntemi ve AssetState.Publish numaralandırma değeri kaldırıldı. Bu değeri kullanır. herhangi bir kod yazılması gerekir.
* FileInfo
  
    * Bu sınıf kaldırıldı ve IAssetFile tarafından değiştirildi.
  
* IAssetFiles
  
    * IAssetFile FileInfo değiştirir ve farklı bir davranışı vardır. Bunu kullanmak için ya da Media Services SDK'sı veya depolama SDK'sını kullanarak bir dosyayı karşıya yükleme tarafından izlenen IAssetFiles nesne örneği. Aşağıdaki IAssetFile.Upload aşırı kullanılabilir:
  
        * IAssetFile.Upload(filePath): Bu zaman uyumlu yöntem iş parçacığını engeller ve yalnızca tek bir dosyayı karşıya yüklediğinizde bu önerilir.
        * IAssetFile.UploadAsync (filePath, blobTransferClient, Bulucu, cancellationToken): Bu zaman uyumsuz yöntem tercih edilen yükleme mekanizmasıdır. 
    
            Bilinen hata: İptal belirteci kullanırsanız, karşıya yükleme iptal edildi. Görevleri iptal izlediğini olabilir. Doğru catch ve özel durumları işleme gerekir.
* Bulucular
  
    * Özel kaynak sürümleri kaldırıldı. SAS özgü bağlamı. Locators.CreateSasLocator (varlık, accessPolicy) kullanım dışı bırakılan veya kaldırılan genel olarak işaretlenir. "Yeni işlevsellik" güncelleştirilmiş davranışı altında "Konum Belirleyicisi" bölümüne bakın.

## <a id="june_changes_12"></a>Önizleme sürümü Haziran 2012
Aşağıdaki işlevler Kasım SDK sürümünde yeni:

* Varlıkları silme
  
    * IAsset, IAssetFile ILocator IAccessPolicy ve IContentKey nesneler artık delete koleksiyonundaki diğer bir deyişle, cloudMediaContext.ObjCollection.Delete(objInstance) yerine diğer bir deyişle, IObject.Delete(), nesne düzeyinde silinir.
* Bulucular
  
    * Bulucular artık CreateLocator yöntemi kullanılarak oluşturulmalıdır. Bunlar, oluşturmak istediğiniz bağımsız değişken olarak belirli bir Bulucu türünün LocatorType.SAS veya LocatorType.OnDemandOrigin enum değerlerinden kullanmanız gerekir.
    * Bulucular içeriğiniz için kullanışlı bir URI'leri elde daha kolay hale getirmek için yeni özellikler eklendi. Bu Bulucuyu tasarlanması, gelecekteki üçüncü taraf genişletilebilirliği için daha fazla esneklik sağlar ve media istemci uygulamaları için kullanım kolaylığı artırır.
* Zaman uyumsuz yöntem desteği
  
    * Tüm yöntemleri için zaman uyumsuz desteği eklendi.

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

<!-- Anchors. -->

<!-- Images. -->

<!--- URLs. --->
[Azure Media Services MSDN Forumu]: https://social.msdn.microsoft.com/forums/azure/home?forum=MediaServices
[Azure Media Services REST API'si başvurusu]: https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference
[Media Services pricing details]: https://azure.microsoft.com/pricing/details/media-services/
[Giriş meta verileri]: https://msdn.microsoft.com/library/azure/dn783120.aspx
[Çıkış meta verileri]: https://msdn.microsoft.com/library/azure/dn783217.aspx
[Deliver content]: https://msdn.microsoft.com/library/azure/hh973618.aspx
[Index media files with the Azure Media Indexer]: https://msdn.microsoft.com/library/azure/dn783455.aspx
[StreamingEndpoint]: https://msdn.microsoft.com/library/azure/dn783468.aspx
[Work with Media Services live streaming]: https://msdn.microsoft.com/library/azure/dn783466.aspx
[Use AES-128 dynamic encryption and the key delivery service]: https://msdn.microsoft.com/library/azure/dn783457.aspx
[Use PlayReady dynamic encryption and the license delivery service]: https://msdn.microsoft.com/library/azure/dn783467.aspx
[Preview features]: https://azure.microsoft.com/services/preview/
[Media Services PlayReady lisans şablonuna genel bakış]: https://msdn.microsoft.com/library/azure/dn783459.aspx
[Stream storage-encrypted content]: https://msdn.microsoft.com/library/azure/dn783451.aspx
[Azure portal]: https://portal.azure.com
[Dinamik paketleme]: https://msdn.microsoft.com/library/azure/jj889436.aspx
[Nick Drouin's blog]: http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/
[Protect Smooth Streaming with PlayReady]: https://msdn.microsoft.com/library/azure/dn189154.aspx
[.NET için Media Services SDK'sı mantığı yeniden deneyin]: https://msdn.microsoft.com/library/azure/dn745650.aspx
[Grass Valley announces EDIUS 7 streaming through the cloud]: https://www.streamingmedia.com/Producer/Articles/ReadArticle.aspx?ArticleID=96351&utm_source=dlvr.it&utm_medium=twitter
[Control Media Services Encoder output file names]: https://msdn.microsoft.com/library/azure/dn303341.aspx
[Create overlays]: https://msdn.microsoft.com/library/azure/dn640496.aspx
[Stitch video segments]: https://msdn.microsoft.com/library/azure/dn640504.aspx
[Azure Media Services .NET SDK 3.0.0.1 and 3.0.0.2 releases]: http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/
[Azure AD Access Control Service]: https://msdn.microsoft.com/library/hh147631.aspx
[Connect to Media Services with the Media Services SDK for .NET]: https://msdn.microsoft.com/library/azure/jj129571.aspx
[Media Services .NET SDK extensions]: https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev
[Azure SDK tools]: https://github.com/Azure/azure-sdk-tools
[GitHub]: https://github.com/Azure/azure-sdk-for-media-services
[Manage Media Services assets across multiple Storage accounts]: https://msdn.microsoft.com/library/azure/dn271889.aspx
[Handle Media Services job notifications]: https://msdn.microsoft.com/library/azure/dn261241.aspx

