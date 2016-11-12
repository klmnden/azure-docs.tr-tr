# [Genel Bakış](media-services-overview.md)
## [Kavramlar ](media-services-concepts.md)
## [Fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/)
## [Sürüm notları](media-services-release-notes.md)
# Başlarken
## [Hesap oluşturma ve yönetme](media-services-portal-create-account.md)
## [Geliştirme ortamınızı kurma](media-services-set-up-computer.md)
## İsteğe bağlı video
### [Portal](media-services-portal-vod-get-started.md)
### [.NET SDK](media-services-dotnet-get-started.md)
### [Java](media-services-java-how-to-use.md)
### [REST](media-services-rest-get-started.md)
## Canlı akış
### [Portal](media-services-portal-live-passthrough-get-started.md)
### [.NET](media-services-dotnet-live-encode-with-onpremises-encoders.md)

# Nasıl yapılır?
## Yönet
### [Portaldaki akış uç noktalarını yönetme](media-services-portal-manage-streaming-endpoints.md)
### Varlıkları yönetme
#### [.NET](media-services-dotnet-manage-entities.md)
#### [REST](media-services-rest-manage-entities.md)
### [Hesapları PowerShell ile yönetme](media-services-manage-with-powershell.md)
### [Videoları Media Encoder Standard ile kırpma](media-services-crop-video.md)
### [Nasıl Yapılır?: Depolama Erişim Anahtarlarını Değiştirdikten Sonra Media Services’ı Güncelleştirme](media-services-roll-storage-access-keys.md)
### [Kotalar ve sınırlamalar](media-services-quotas-and-limitations.md)
### Filtreler
#### [Azure Media Services .NET SDK ile Filtre Oluşturma](media-services-dotnet-dynamic-manifest.md)
#### [Media Encoder Standard kullanarak bir varlığı kodlama](media-services-rest-encode-asset.md)
### Program aracılığıyla bağlanma
#### [.NET](media-services-dotnet-connect-programmatically.md)
#### [REST](media-services-rest-connect-programmatically.md)

## İçerik yükleme
### Bir hesaba dosya yükleme
#### [Portal ](media-services-portal-upload-files.md)
#### [.NET](media-services-dotnet-upload-files.md)
#### [REST](media-services-rest-upload-files.md)
### [Mevcut blobları kopyalama](media-services-copying-existing-blob.md)

## Kodlama
### [İçerik](media-services-encode-asset.md)
#### Media Encoder Standard kullanarak bir varlığı kodlama
##### [Portal](media-services-portal-encode.md)
##### [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
#### [.NET ile Media Encoder Standard kullanarak küçük resim oluşturma](media-services-dotnet-generate-thumbnail-with-mes.md)
#### [Gelişmiş kodlama](media-services-advanced-encoding-with-mes.md)
##### [Media Encoder Premium İş Akışı](media-services-encode-with-premium-workflow.md)
##### [Media Encoder Premium İş Akışı öğreticileri](media-services-media-encoder-premium-workflow-tutorials.md)
##### [İş Akışı Tasarımcısı ile Gelişmiş Kodlama İş Akışları Oluşturma](media-services-workflow-designer.md)
##### [Birden fazla giriş ile premium iş akışı](media-services-media-encoder-premium-workflow-multiplefilesinput.md)

#### Şemalar 
#####[Media Encoder Standard](media-services-mes-schema.md)
#####[Giriş meta verileri](media-services-input-metadata-schema.md)
#####[Çıkış meta verileri](media-services-output-metadata-schema.md)

#### Eski kodlayıcılar
##### [Azure Media Packager’ı kullanma](media-services-static-packaging.md)

### [Canlı akışlar](media-services-manage-channels-overview.md)
#### [Şirket içi kodlayıcılar](media-services-live-streaming-with-onprem-encoders.md)
#### Şirket içi kodlayıcı öğreticileri
##### [Portal](media-services-portal-live-passthrough-get-started.md)
##### [.NET](media-services-dotnet-live-encode-with-onpremises-encoders.md)
#### [Bulut kodlayıcıyla canlı akış](media-services-manage-live-encoder-enabled-channels.md)
#### Bulut kodlayıcı öğreticileri
##### [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
##### [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
#### [Şirket içi kodlayıcıları bulut kodlayıcıyla kullanılmak üzere yapılandırma](media-services-live-encoders-overview.md)
#### [Uzun süren işlemleri çözme](media-services-dotnet-long-operations.md)
#### [Bölünmüş MP4 canlı içe alma belirtimi](media-services-fmp4-live-ingest-overview.md)
#### [Dinamik paketleme](media-services-dynamic-packaging-overview.md)

### Medya İşleme
#### [.NET](media-services-get-media-processor.md)
#### [REST](media-services-rest-get-media-processor.md)

### Kodlayıcıları tek bit hızı canlı akışı için yapılandırma
#### [Elemental Live kodlayıcı](media-services-configure-elemental-live-encoder.md)
#### [FMLE kodlayıcı](media-services-configure-fmle-live-encoder.md)
#### [NewTek TriCaster kodlayıcı](media-services-configure-tricaster-live-encoder.md)
#### [Wirecast kodlayıcı](media-services-configure-wirecast-live-encoder.md)

## [Koruma](media-services-content-protection-overview.md)
### [Portalda içerik korumayı yapılandırma](media-services-portal-protect-content.md)
### [Akışınız için AES-128 şifresiz anahtarını yapılandırma](media-services-protect-with-aes128.md)
### [AMS REST API kullanarak İçeriğinizi Depolama Şifreleme ile şifreleme](media-services-rest-storage-encryption.md)
### [Media Services PlayReady Lisans Şablonuna Genel Bakış](media-services-playready-license-template-overview.md)
### [DRM lisansı teslimi](media-services-deliver-keys-and-licenses.md)
### [Azure Media Services’ta Widevine lisanslarını teslim etmek için iş ortaklarıyla çalışma](media-services-licenses-partner-integration.md)
### [PlayReady ve/ya Widevine dinamik ortak şifreleme işlemini kullanma](media-services-protect-with-drm.md)
### [Apple FairPlay ile korunan HLS içeriğinizin akışını yapmak için Azure Media Services kullanma](media-services-protect-hls-with-fairplay.md)
### [Çoklu DRM ve Access Control ile CENC: Azure ve Azure Media Services Tasarım ve Uygulama Başvurusu](media-services-cenc-with-multidrm-access-control.md)

### Varlık teslimi
#### Varlık teslim ilkelerini yapılandırma
##### [.NET](media-services-dotnet-configure-asset-delivery-policy.md)
##### [REST](media-services-rest-configure-asset-delivery-policy.md)
### ContentKeys oluşturma
#### [.NET](media-services-dotnet-create-contentkey.md)
#### [REST](media-services-rest-create-contentkey.md)
### İçerik anahtarı yetkilendirme ilkesini yapılandırma
#### [Portal](media-services-portal-configure-content-key-auth-policy.md)
#### [.NET](media-services-dotnet-configure-content-key-auth-policy.md)
#### [REST](media-services-rest-configure-content-key-auth-policy.md)

## [Çözümleme](media-services-analytics-overview.md)
### [Indexer 2 ile işleme](media-services-process-content-with-indexer2.md)
### [Indexer ile işleme](media-services-index-content.md)
### [Hyperlapse ile işleme](media-services-hyperlapse-content.md)
### [Face Detector ile işleme](media-services-face-and-emotion-detection.md)
### [Motion Detector ile işleme](media-services-motion-detection.md)
### [Face Redaction ile işleme](media-services-face-redaction.md)
### [Video Thumbnails ile işleme](media-services-video-summarization.md)
### [OCR ile işleme](media-services-video-optical-character-recognition.md)

## Ölçek
### [Medya İşleme](media-services-scale-media-processing-overview.md)
#### [Portal](media-services-portal-scale-media-processing.md)
#### [.NET](media-services-dotnet-encoding-units.md)
#### [REST](https://msdn.microsoft.com/library/azure/dn859236.aspx)
### Akış Uç Noktaları
#### [Portal](media-services-portal-scale-streaming-endpoints.md)

## [İçerik teslim etme](media-services-deliver-content-overview.md)
### [Filtrelere ve dinamik bildirimlere genel bakış](media-services-dynamic-manifest-overview.md)
### Filtre oluşturma
#### [.NET](media-services-dotnet-dynamic-manifest.md)
#### [REST](media-services-rest-dynamic-manifest.md)
### İçerik yayımlama
#### [Portal](media-services-portal-publish.md)
#### [.NET](media-services-deliver-streaming-content.md)
#### [REST](media-services-rest-deliver-streaming-content.md)
### [İndirme ile teslim etme](media-services-deliver-asset-download.md)
### [Akış yük devretme senaryosu](media-services-implement-failover.md)

## Tüketme
### [Mevcut yürütücüler ile medya kayıttan yürütme](media-services-playback-content-with-existing-players.md)
### [Media Player ile medya kayıttan yürütme](media-services-develop-video-players.md)
### Diğer kayıttan yürütme seçenekleri
#### [Kesintisiz akış Windows Mağazası uygulaması](media-services-build-smooth-streaming-apps.md)
#### [DASH.js ile HTML5 Uygulaması](media-services-embed-mpeg-dash-in-html5.md)
#### [Adobe Open Source Media Framework yürütücüleri](media-services-use-osmf-smooth-streaming-client-plugin.md)
### [İstemci tarafına reklam ekleme](media-services-inserting-ads-on-client-side.md)

## Tümleştirme
### [Media Services Uzantısında CDN Önbelleğe Alma İlkesi](../cdn/cdn-caching-policy.md?toc=%2fazure%2fmedia-services%2ftoc.json)
### [Microsoft†" Kesintisiz Akış İstemci Taşıma Kiti Lisanslama](media-services-sspk.md)
### [Birden çok Depolama hesabında yer alan varlıkları yönetme](meda-services-managing-multiple-storage-accounts.md)
### [Azure Media Services’ta Widevine lisanslarını teslim etmek için Axinom kullanma  ](media-services-axinom-integration.md)
### [Azure Media Services’ta Widevine lisanslarını teslim etmek için castLabs kullanma](media-services-castlabs-integration.md)
### [Widevine Lisans Şablonuna Genel Bakış](media-services-widevine-license-template-overview.md)

## İzleme
### İşin ilerleme durumunu denetleme
#### [REST](media-services-rest-check-job-progress.md)
#### [Portal](media-services-portal-check-job-progress.md)
#### [.NET](media-services-check-job-progress.md)
### [İş bildirimlerini izlemek için Kuyruk Depolama](media-services-dotnet-check-job-progress-with-queues.md)

## Sorun giderme
### [Sık sorulan sorular](media-services-frequently-asked-questions.md)
### [Canlı akış sorun giderme kılavuzu](media-services-troubleshooting-live-streaming.md)
###[Hata kodları](media-services-error-codes.md)
###[Yeniden deneme mantığı](media-services-retry-logic-in-dotnet-sdk.md)

# Başvuru
## [Media Services .NET SDK](media-services-dotnet-how-to-use.md)
## [Media Services REST API'si](media-services-rest-how-to-use.md)
## [Media Encoder Premium İş Akışı Biçimleri ve Kod Çözücüleri](media-services-premium-workflow-encoder-formats.md)
## [Media Encoder Standard Biçimleri ve Kod Çözücüleri](media-services-media-encoder-standard-formats.md)

# İlgili
## [Azure Media Services Topluluğu](media-services-community.md)











<!--HONumber=Nov16_HO2-->


