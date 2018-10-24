# [Media Services Belgeleri](index.md)

# [Genel Bakış](media-services-overview.md)
## [Senaryolar ve kullanılabilirlik](scenarios-and-availability.md)
## [Kavramlar ](media-services-concepts.md)

# başlarken
## [Hesap oluşturma ve yönetme](media-services-portal-create-account.md)
## [Geliştirme ortamınızı kurma](media-services-set-up-computer.md)
### [.NET](media-services-dotnet-how-to-use.md)
### [REST](media-services-rest-how-to-use.md)  
## [API’ye erişmek için Azure AD kimlik doğrulaması kullanma](media-services-use-aad-auth-to-access-ams-api.md)
### [Azure AD kimlik doğrulamasını yönetmek için portal kullanma](media-services-portal-get-started-with-aad.md)
### [API’ye .NET ile erişme](media-services-dotnet-get-started-with-aad.md)
### [API’ye REST ile erişme](media-services-rest-connect-with-aad.md)
### [Azure AD uygulaması oluşturmak ve yapılandırmak için Azure CLI kullanma](media-services-cli-create-and-configure-aad-app.md)
### [Azure AD uygulaması oluşturmak ve yapılandırmak için Azure PowerShell kullanma](media-services-powershell-create-and-configure-aad-app.md)

## İsteğe bağlı video gönderme
### [Azure Portal](media-services-portal-vod-get-started.md)
### [.NET SDK](media-services-dotnet-get-started.md)
### [Java](media-services-java-how-to-use.md)
### [REST](media-services-rest-get-started.md)
## Canlı akış gerçekleştirme
### [Azure Portal](media-services-portal-live-passthrough-get-started.md)
### [.NET](media-services-dotnet-live-encode-with-onpremises-encoders.md)

# Nasıl yapılır
## Yönetme
### Varlıklar
#### [.NET](media-services-dotnet-manage-entities.md)
#### [REST](media-services-rest-manage-entities.md)
### [Akış uç noktaları](media-services-streaming-endpoints-overview.md)
#### [Azure Portal](media-services-portal-manage-streaming-endpoints.md)
#### [.NET](media-services-dotnet-manage-streaming-endpoints.md)
### Depolama
#### [Depolama erişim anahtarlarını değiştirdikten Sonra Media Services’ı güncelleştirme](media-services-roll-storage-access-keys.md)
#### [Birden çok depolama hesabında yer alan varlıkları yönetme](meda-services-managing-multiple-storage-accounts.md)
### [Kotalar ve sınırlamalar](media-services-quotas-and-limitations.md)
## [Postman yapılandırma](media-rest-apis-with-postman.md)
### [İsteğe bağlı akış koleksiyonu](postman-collection.md)
### [Canlı akış koleksiyonu](postman-live-streaming-collection.md)
### [Ortam](postman-environment.md)
## İçerik yükleme
### Bir hesaba dosya yükleme
#### [Azure Portal](media-services-portal-upload-files.md)
#### [.NET](media-services-dotnet-upload-files.md)
#### [REST](media-services-rest-upload-files.md)
### [Aspera ile büyük dosyaları karşıya yükleme](media-services-upload-files-with-aspera.md)
### [StorSimple ile dosyaları karşıya yükleme](media-services-upload-files-from-storsimple.md)
### [Mevcut blobları kopyalama](media-services-copying-existing-blob.md)

## [İçerik kodlama](media-services-encode-asset.md)
### [Kodlayıcıları karşılaştır](media-services-compare-encoders.md)
### [Kodlamanızın hızını ve eşzamanlılığını yönetme](media-services-manage-encoding-speed.md)
### Media Encoder Standard (MES)
#### [Media Encoder Standard Biçimleri ve Kod Çözücüleri](media-services-media-encoder-standard-formats.md)
#### [Otomatik olarak bit hızı merdiveni oluşturmak için MES kullanma](media-services-autogen-bitrate-ladder-with-mes.md)
#### Media Encoder Standard ile kodlama
##### [Azure Portal](media-services-portal-encode.md)
##### [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
##### [REST](media-services-rest-encode-asset.md)
#### [MES ile gelişmiş kodlama](media-services-advanced-encoding-with-mes.md)
##### [Media Encoder Standard hazır ayarlarını özelleştirme](media-services-custom-mes-presets-with-dotnet.md)
##### [.NET ile Media Encoder Standard kullanarak küçük resim oluşturma](media-services-dotnet-generate-thumbnail-with-mes.md)
##### [Videoları Media Encoder Standard ile kırpma](media-services-crop-video.md)
##### [Küçük resim sprite’ı oluşturma](generate-thumbnail-sprite.md)
#### MES Şemaları
##### [Media Encoder Standard şeması](media-services-mes-schema.md)
##### [Giriş meta verileri](media-services-input-metadata-schema.md)
##### [Çıkış meta verileri](media-services-output-metadata-schema.md)
#### [MES Ön Ayarları](media-services-mes-presets-overview.md) 
##### [H264 Çoklu Bit Hızı 1080p Ses 5.1](media-services-mes-preset-H264-Multiple-Bitrate-1080p-Audio-5.1.md)
##### [H264 Çoklu Bit Hızı 1080p](media-services-mes-preset-H264-Multiple-Bitrate-1080p.md)
##### [H264 Çoklu Bit Hızı 16x9 SD Ses 5.1](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD-Audio-5.1.md)
##### [H264 Çoklu Bit Hızı 16x9 SD](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD.md)
##### [iOS için H264 Çoklu Bit Hızı 16x9](media-services-mes-preset-H264-Multiple-Bitrate-16x9-for-iOS.md)
##### [H264 Çoklu Bit Hızı 4K Ses 5.1](media-services-mes-preset-H264-Multiple-Bitrate-4K-Audio-5.1.md)
##### [H264 Çoklu Bit Hızı 4K](media-services-mes-preset-H264-Multiple-Bitrate-4K.md)
##### [H264 Çoklu Bit Hızı 4x3 SD Ses 5.1](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD-Audio-5.1.md)
##### [H264 Çoklu Bit Hızı 4x3 SD](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD.md)
##### [iOS için H264 Çoklu Bit Hızı 4x3](media-services-mes-preset-H264-Multiple-Bitrate-4x3-for-iOS.md)
##### [H264 Çoklu Bit Hızı 720p Ses 5.1](media-services-mes-preset-H264-Multiple-Bitrate-720p-Audio-5.1.md)
##### [H264 Çoklu Bit Hızı 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md)
##### [H264 Tekli Bit Hızı 1080p Ses 5.1](media-services-mes-preset-H264-Single-Bitrate-1080p-Audio-5.1.md)
##### [H264 Tekli Bit Hızı 1080p](media-services-mes-preset-H264-Single-Bitrate-1080p.md)
##### [H264 Tekli Bit Hızı 16x9 SD Ses 5.1](media-services-mes-preset-H264-Single-Bitrate-16x9-SD-Audio-5.1.md)
##### [H264 Tekli Bit Hızı 16x9 SD](media-services-mes-preset-H264-Single-Bitrate-16x9-SD.md)
##### [H264 Tekli Bit Hızı 4K Ses 5.1](media-services-mes-preset-H264-Single-Bitrate-4K-Audio-5.1.md)
##### [H264 Tekli Bit Hızı 4K](media-services-mes-preset-H264-Single-Bitrate-4K.md)
##### [H264 Tekli Bit Hızı 4x3 SD Ses 5.1](media-services-mes-preset-H264-Single-Bitrate-4x3-SD-Audio-5.1.md)
##### [H264 Tekli Bit Hızı 4x3 SD](media-services-mes-preset-H264-Single-Bitrate-4x3-SD.md)
##### [H264 Tekli Bit Hızı 720p Ses 5.1](media-services-mes-preset-H264-Single-Bitrate-720p-Audio-5.1.md)
##### [H264 Tekli Bit Hızı 720p](media-services-mes-preset-H264-Single-Bitrate-720p.md)
##### [Android için H264 Tekli Bit Hızı 720p](media-services-mes-preset-H264-Single-Bitrate-720p-for-Android.md)
##### [Android için H264 Tekli Bit Hızı Yüksek Kaliteli SD](media-services-mes-preset-H264-Single-Bitrate-High-Quality-SD-for-Android.md)
##### [Android için H264 Tekli Bit Hızı Düşük Kaliteli SD](media-services-mes-preset-H264-Single-Bitrate-Low-Quality-SD-for-Android.md)
### Media Encoder Premium İş Akışı
#### [Media Encoder Premium İş Akışı Biçimleri ve Kod Çözücüleri](media-services-premium-workflow-encoder-formats.md)
#### Media Encoder Premium İş Akışı ile kodlama
##### [Media Encoder Premium İş Akışı](media-services-encode-with-premium-workflow.md)
##### [Media Encoder Premium İş Akışı öğreticileri](media-services-media-encoder-premium-workflow-tutorials.md)
##### [İş Akışı Tasarımcısı ile Gelişmiş Kodlama İş Akışları Oluşturma](media-services-workflow-designer.md)
##### [Birden fazla giriş ile premium iş akışı](media-services-media-encoder-premium-workflow-multiplefilesinput.md)
### [fMP4 öbekleri oluşturan bir görev oluşturma](media-services-generate-fmp4-chunks.md)
### Medya işlemcileri
#### [.NET](media-services-get-media-processor.md)
#### [REST](media-services-rest-get-media-processor.md)
### [Hata kodları](media-services-encoding-error-codes.md)
### Kullanım Dışı
#### [Statik paketleme ve şifreleme](media-services-static-packaging.md)

## [Canlı akış](media-services-manage-channels-overview.md)
### [Şirket içi kodlayıcılar](media-services-live-streaming-with-onprem-encoders.md)
#### [Önerilen şirket içi kodlayıcılar](media-services-recommended-encoders.md)
#### [Azure Portal](media-services-portal-live-passthrough-get-started.md)
#### [.NET](media-services-dotnet-live-encode-with-onpremises-encoders.md)
### [Bulut kodlayıcıyla canlı akış](media-services-manage-live-encoder-enabled-channels.md)
#### [Azure Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
#### [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
### [Şirket içi kodlayıcıları bulut kodlayıcıyla kullanılmak üzere yapılandırma](media-services-live-encoders-overview.md)
#### [FMLE kodlayıcı](media-services-configure-fmle-live-encoder.md)
#### [Haivision KB kodlayıcı](media-services-configure-kb-live-encoder.md)
#### [NewTek TriCaster kodlayıcı](media-services-configure-tricaster-live-encoder.md)
#### [Wirecast kodlayıcı](media-services-configure-wirecast-live-encoder.md)
### [Uzun süren işlemleri çözme](media-services-dotnet-long-operations.md)
### [Bölünmüş MP4 canlı alma belirtimi](media-services-fmp4-live-ingest-overview.md)

## [İçeriği kırpma](media-services-azure-media-clipper-overview.md)
### [Başlarken](media-services-azure-media-clipper-getting-started.md)
### [Videoları yükleme](media-services-azure-media-clipper-load-assets.md)
### [Klavye kısayollarını yapılandırma](media-services-azure-media-clipper-keyboard-shortcuts.md)
### [Yerelleştirmeyi yapılandırma](media-services-azure-media-clipper-localization.md)
### [Kırpma işlerini gönderme](media-services-azure-media-clipper-submit-job.md)
### [Azure Portal](media-services-azure-media-clipper-portal.md)

## [İçerik koruma](media-services-content-protection-overview.md)
### [Depolama şifrelemesi](media-services-rest-storage-encryption.md)
### [AES-128 şifrelemesi](media-services-protect-with-aes128.md)
### [Akış için PlayReady/Widevine](media-services-protect-with-playready-widevine.md)
### [Akış için FairPlay](media-services-protect-hls-with-fairplay.md)
### [Windows 10 için çevrimdışı PlayReady](https://blogs.msdn.microsoft.com/playready4/2016/10/26/does-azure-media-services-support-offline-mode/)
### [iOS için çevrimdışı FairPlay](media-services-protect-hls-with-offline-fairplay.md)
### [Android için çevrimdışı Widevine](offline-widevine-for-android.md)
### [Azure portalında yapılandırma](media-services-portal-protect-content.md)
### [DRM lisanslarını teslim etme](media-services-deliver-keys-and-licenses.md)
### ContentKeys oluşturma
#### [.NET](media-services-dotnet-create-contentkey.md)
#### [REST](media-services-rest-create-contentkey.md)
### Lisans şablonlarına genel bakış
#### [PlayReady lisans şablonu](media-services-playready-license-template-overview.md)
#### [Widevine lisans şablonu](media-services-widevine-license-template-overview.md)
### Varlık teslim ilkelerini yapılandırma
#### [.NET](media-services-dotnet-configure-asset-delivery-policy.md)
#### [REST](media-services-rest-configure-asset-delivery-policy.md)
### İçerik anahtarı yetkilendirme ilkesini yapılandırma
#### [Azure Portal](media-services-portal-configure-content-key-auth-policy.md)
#### [.NET](media-services-dotnet-configure-content-key-auth-policy.md)
#### [REST](media-services-rest-configure-content-key-auth-policy.md)
### [Kimlik doğrulama belirteçlerini AMS’ye geçirme](media-services-pass-authentication-tokens.md)
### Başvuru tasarımları
#### [Hibrit DRM sistem tasarımı](hybrid-design-drm-sybsystem.md)
#### [Başvuru çoklu DRM tasarımı](media-services-cenc-with-multidrm-access-control.md)

## [Çözümleme](media-services-analytics-overview.md)
### [Azure portalını kullanarak medya çözümleme](media-services-portal-analyze.md)
### [Indexer 2 ile işleme](media-services-process-content-with-indexer2.md)
### [Indexer ile işleme](media-services-index-content.md)
#### [Hazır görev](indexer-task-preset.md)
### [Hyperlapse ile işleme](media-services-hyperlapse-content.md)
### [Face Detector ile işleme](media-services-face-and-emotion-detection.md)
### [Motion Detector ile işleme](media-services-motion-detection.md)
### [Face Redactor ile İşleme](media-services-face-redaction.md)
#### [Face Redactor kılavuzu](media-services-redactor-walkthrough.md)
### [Video Thumbnails ile işleme](media-services-video-summarization.md)
### [OCR ile işleme](media-services-video-optical-character-recognition.md)
### [Content Moderator ile işleme](media-services-content-moderation.md)

## [Telemetri yapılandırma](media-services-telemetry-overview.md)
###[.NET](media-services-dotnet-telemetry.md)
###[REST](media-services-rest-telemetry.md)

## Ölçek
### [Medya İşleme](media-services-scale-media-processing-overview.md)
#### [Azure Portal](media-services-portal-scale-media-processing.md)
#### [.NET](media-services-dotnet-encoding-units.md)
### Akış Uç Noktaları
#### [Azure Portal](media-services-portal-scale-streaming-endpoints.md)

## [İçerik teslim etme](media-services-deliver-content-overview.md)
### [Dinamik paketleme](media-services-dynamic-packaging-overview.md)
### [Filtrelere ve dinamik bildirimlere genel bakış](media-services-dynamic-manifest-overview.md)
#### [.NET ile filtre oluşturma](media-services-dotnet-dynamic-manifest.md)
#### [REST ile filtre oluşturma](media-services-rest-dynamic-manifest.md)
### [Media Services Uzantısında CDN Önbelleğe Alma İlkesi](../../cdn/cdn-caching-policy.md?toc=%2fazure%2fmedia-services%2ftoc.json)
### İçerik yayımlama
#### [Azure Portal](media-services-portal-publish.md)
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
### [Microsoft† Kesintisiz Akış İstemci Taşıma Kitini Lisanslama](media-services-sspk.md)

## Tümleştirme
### [Azure İşlevleri’ni Media Services ile kullanma](media-services-dotnet-how-to-use-azure-functions.md)
### [Azure İşlevleri ile Media Services örnekleri](https://github.com/Azure-Samples/media-services-dotnet-functions-integration)

## İzleme
### İşin ilerleme durumunu denetleme
#### [REST](media-services-rest-check-job-progress.md)
#### [Azure Portal](media-services-portal-check-job-progress.md)
#### [.NET](media-services-check-job-progress.md)
### [Kuyruk depolama ile iş bildirimlerini izleme](media-services-dotnet-check-job-progress-with-queues.md)
### [Web kancaları ile iş bildirimlerini izleme](media-services-dotnet-check-job-progress-with-webhooks.md)

## Sorun giderme
### [Sık sorulan sorular](media-services-frequently-asked-questions.md)
### [Canlı akış sorun giderme kılavuzu](media-services-troubleshooting-live-streaming.md)
### [Hata kodları](media-services-error-codes.md)
### [Yeniden deneme mantığı](media-services-retry-logic-in-dotnet-sdk.md)

# Başvuru
## [Kod örnekleri](https://azure.microsoft.com/resources/samples/?service=media-services)
## [Azure PowerShell (Resource Manager)](/powershell/module/azurerm.media)
## [Azure PowerShell (Hizmet Yönetimi)](/powershell/module/servicemanagement/azure/?view=azuresmps-3.7.0)
## [.NET](/dotnet/api/microsoft.windowsazure.mediaservices.client)
## [REST](/rest/api/media/mediaservice)
## Belirtimler
### [Canlı Alma - Bölünmüş MP4 canlı alma belirtimi](media-services-fmp4-live-ingest-overview.md)
### [Canlı Alma - Canlı Akışta Zamanlanmış Meta Verilere Sinyal Gönderme](media-services-specifications-live-timed-metadata.md)
### [Kesintisiz Akış HEVC](media-services-specifications-ms-sstr-amendment-hevc.md)

# Kaynaklar
## [Azure Media Services topluluğu](media-services-community.md)
## [Azure yol haritası](https://azure.microsoft.com/roadmap/?category=web-mobile)
## [Fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/)
## [Fiyatlandırma hesaplayıcı](https://azure.microsoft.com/pricing/calculator/)
## [Sürüm notları](media-services-release-notes.md)
## [Videolar](https://azure.microsoft.com/resources/videos/index/?services=media-services)
