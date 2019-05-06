---
title: Azure Media Services paketleme ve kaynak hataları | Microsoft Docs
description: Bu konuda Azure Media Services paketleme hizmetinden alabileceğiniz hataları açıklanır.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/28/2019
ms.author: juliako
ms.openlocfilehash: e30c51ff3526bb5ed193b65b3f36a64c552024ff
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65141145"
---
# <a name="media-services-packaging-errors"></a>Media Services paketleme hataları 

Bu konuda, Azure Media Services'den alabilirsiniz hataları açıklanır [paketleme hizmeti](streaming-endpoint-concept.md).

## <a name="400-bad-request"></a>400 Hatalı istek

İstek geçersiz bilgi içeriyor ve bu hata kodları ve aşağıdakilerden biri nedeniyle reddetti:

|Hata kodu|Onaltılık değer |Hata açıklaması|
|---|---|---|
|MPE_BAD_URL_SYNTAX |0x80890201|Bir URL söz dizimi veya biçim hatası. Geçersiz bir tür, geçersiz bir parça veya geçersiz bir parça istekleri örneklerindendir. |
|MPE_ENC_ENCRYPTION_NOT_SPECIFIED_IN_URL |0x8088024C|İstek URL'SİNDE şifreleme etiketi yok sahiptir. CMAF istekleri bir şifreleme etiketinin URL gerektirir. Birden fazla şifreleme türü ile yapılandırılan diğer protokolleri, ayrıca Kesinleştirme için şifreleme etiketi gerektirir. |
|MPE_STORAGE_BAD_URL_SYNTAX |0x808900E9|Hatalı istek hata vererek başarısız oldu isteği gerçekleştirmek için depolama isteği. |

## <a name="403-forbidden"></a>403 Yasak

İstek aşağıdaki nedenlerden biri nedeniyle izin verilmez:

|Hata kodu|Onaltılık değer |Hata açıklaması|
|---|---|---|
|MPE_STORAGE_AUTHENTICATION_FAILED |0x808900EA|Depolama isteği isteği gerçekleştirmek için bir kimlik doğrulama hatası ile başarısız oldu. Depolama anahtarlarını döndürülmüş ve hizmet depolama anahtarlarını eşitlenemiyor. Bu durum meydana gelebilir. <br/><br/>Bağlantı kurun, Azure destek giderek [Yardım + Destek](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) Azure portalında.|
|MPE_STORAGE_INSUFFICIENT_ACCOUNT_PERMISSIONS |0x808900EB |Depolama işlemi hatası erişim yetersiz hesap izinleri nedeniyle başarısız oldu. |
|MPE_STORAGE_ACCOUNT_IS_DISABLED |0x808900EC |Depolama hesabı olduğundan devre dışı olduğundan isteği gerçekleştirmek için depolama isteği başarısız oldu. |
|MPE_STORAGE_AUTHENTICATION_FAILURE |0x808900F3 |Depolama işlemi hatası erişim genel hatalar nedeniyle başarısız oldu. |
|MPE_OUTPUT_FORMAT_BLOCKED |0x80890207 |Çıkış biçimi StreamingPolicy yapılandırmada nedeniyle engellendi. |
|MPE_ENC_ENCRYPTION_REQUIRED |0x8088021E |Şifreleme için içerik gereklidir, teslim ilkesini için çıkış biçimini gereklidir. |
|MPE_ENC_ENCRYPTION_NOT_SET_IN_DELIVERY_POLICY |0x8088024D |Teslim İlkesi ayarlarında şifreleme ayarlı değil. |

## <a name="404-not-found"></a>404 Bulunamadı

Artık bir kaynakta yapacak işlem çalışıyor. Örneğin, bir kaynak zaten silinmiş olabilir.

|Hata kodu|Onaltılık değer |Hata açıklaması|
|---|---|---|
|MPE_EGRESS_TRACK_NOT_FOUND |0x80890209 |İstenen izleme bulunamadı. |
|MPE_RESOURCE_NOT_FOUND |0x808901F9 |İstenen kaynak bulunamadı. |
|MPE_UNAUTHORIZED |0x80890244 |Yetkisiz erişim. |
|MPE_EGRESS_TIMESTAMP_NOT_FOUND |0x8089020A |İstenen zaman damgası bulunamadı. |
|MPE_EGRESS_FILTER_NOT_FOUND |0x8089020C |İstenen dinamik bildirim filtresi bulunamadı. |
|MPE_FRAGMENT_BY_INDEX_NOT_FOUND |0x80890252 |Geçerli aralık istenen parça dizinidir. |
|MPE_LIVE_MEDIA_ENTRIES_NOT_FOUND |0x80890254 |Canlı. medya moov arabelleğe almak için girdileri bulunamıyor. |
|MPE_FRAGMENT_TIMESTAMP_NOT_FOUND |0x80890255 |İstenen zaman belirli bir parça parça bulmak yüklenemiyor.<br/><br/>Parça depolamada olmadığından emin olabilir. Farklı bir parçası olabilir sunu katmanı deneyin. |
|MPE_MANIFEST_MEDIA_ENTRY_NOT_FOUND |0x80890256 |Medya girdisi bildirimde istenen bit hızını bulmak yüklenemiyor. <br/><br/>Oyuncu bildiriminde değildi belirli bir hızı bir video izlemek için sorulan olabilir.|
|MPE_METADATA_NOT_FOUND |0x80890257 |Bildirimde belirli meta verileri bulunamadı veya yeniden Temellendirme depolamadan bulunamıyor. |
|MPE_STORAGE_RESOURCE_NOT_FOUND |0x808900ED |Depolama işlemi hatası, kaynak bulunamadı. |

## <a name="409-conflict"></a>409 çakışma

Kimliği üzerinde bir kaynak için sağlanan bir `PUT` veya `POST` işlemi var olan bir kaynak tarafından alınmış. Bu sorunu çözmek için kaynak için farklı bir kimlik kullanın.

|Hata kodu|Onaltılık değer |Hata açıklaması|
|---|---|---|
|MPE_STORAGE_CONFLICT  |0x808900EE  |Depolama işlemi hatası, çakışma hatası.  |

## <a name="410"></a>410

|Hata kodu|Onaltılık değer |Hata açıklaması|
|---|---|---|
|MPE_FILTER_FORCE_END_LEFT_EDGE_CROSSED_DVR_WINDOW|0x80890263|Filtre olan canlı akış için true, başlangıç veya bitiş zaman damgası ayarlamak forceEndTimestamp geçerli DVR penceresi dışında andır.|

## <a name="412-precondition-failure"></a>412 önkoşul hatası

İşlem sunucusunda, diğer bir deyişle, bir iyimser eşzamanlılık hatası kullanılabilir sürümünden farklı bir eTag belirtilen. Kaynağın en son sürümünü okuma ve istekte eTag güncelleştirme sonra isteği yeniden deneyin.

|Hata kodu|Onaltılık değer |Hata açıklaması|
|---|---|---|
|MPE_FRAGMENT_NOT_READY |0x80890200 |İstenen parça hazır değil.|
|MPE_STORAGE_PRECONDITION_FAILED| 0x808900EF|Depolama işlemi hatası, bir önkoşul hatası.|

## <a name="415-unsupported-media-type"></a>415 Desteklenmeyen medya türü

İstemci tarafından gönderilen yük biçimi desteklenmeyen bir biçimindedir.

|Hata kodu|Onaltılık değer |Hata açıklaması|
|---|---|---|
|MPE_ENC_ALREADY_ENCRYPTED| 0x8088021F| Şifreleme zaten şifrelenmiş içeriği geçerli değil.|
|MPE_ENC_INVALID_INPUT_ENCRYPTION_FORMAT|0x8088021D |Şifreleme için giriş biçimi geçersiz.|
|MPE_INVALID_ASSET_DELIVERY_POLICY_TYPE|0x8088021C| Teslim İlkesi türü geçersiz.|
|MPE_ENC_MULTIPLE_SAME_DELIVERY_TYPE|0x8088024E |Birden çok çıkış biçimleri tarafından paylaşılan özgün ayarları.|
|MPE_FORMAT_NOT_SUPPORTED|0x80890205|Medya biçimi veya türü desteklenmiyor. Örneğin, Media Services, 64'ten kalite düzeyi sayısı desteklemez. FLV video etiketinde Media Services ile birden çok SPS ve birden çok pps'dir. video çerçeve desteklemiyor|
|MPE_INPUT_FORMAT_NOT_SUPPORTED|0x80890218| İstenen varlık giriş biçimi desteklenmiyor. Media Services destekler (dinamik), kesintisiz MP4 (VoD) ve aşamalı indirme biçimleri.|
|MPE_OUTPUT_FORMAT_NOT_SUPPORTED|0x8089020D|İstenen çıktı biçimi desteklenmiyor. Media Services destekler kesintisiz, DASH (CSF, CMAF), (v3, v4, CMAF), HLS ve aşamalı biçimleri indirin.|
|MPE_ENCRYPTION_NOT_SUPPORTED|0x80890208|Desteklenmeyen şifreleme türü ile karşılaşıldı.|
|MPE_MEDIA_TYPE_NOT_SUPPORTED|0x8089020E|İstenen medya türüne göre çıktı biçimi desteklenmiyor. Video, ses veya "SUBT" alt konu başlığı, desteklenen türler şunlardır.|
|MPE_MEDIA_ENCODING_NOT_SUPPORTED|0x8089020F|Kaynak varlık medya çıkış biçimiyle uyumlu olmayan bir medya biçimini ile kodlanan.|
|MPE_VIDEO_ENCODING_NOT_SUPPORTED|0x80890210|Kaynak varlık çıkış biçimiyle uyumlu olmayan bir video biçimi ile kodlanan. H.264 AVC, H.265 (HEVC, hev1 veya hvc1) desteklenir.|
|MPE_AUDIO_ENCODING_NOT_SUPPORTED|0x80890211|Kaynak varlık çıkış biçimiyle uyumlu olmayan bir ses biçimi ile kodlanan. Desteklenen ses biçimleri şunlardır: AAC, E-AC3 (gg +), Dolby DTS.|
|MPE_SOURCE_PROTECTION_CONVERSION_NOT_SUPPORTED|0x80890212|Kaynak Korunan varlık çıkış biçimine dönüştürülemiyor.|
|MPE_OUTPUT_PROTECTION_FORMAT_NOT_SUPPORTED|0x80890213|Koruma biçimi tarafından çıktı biçimi desteklenmiyor.|
|MPE_INPUT_PROTECTION_FORMAT_NOT_SUPPORTED|0x80890219|Koruma biçimi giriş biçimi tarafından desteklenmiyor.|
|MPE_INVALID_VIDEO_NAL_UNIT|0x80890231|Geçersiz görüntü NAL birim, örneğin, yalnızca ilk NAL örneğindeki bir AUD. olabilir|
|MPE_INVALID_NALU_SIZE|0x80890260|NAL birim boyutu geçersiz.|
|MPE_INVALID_NALU_LENGTH_FIELD|0x80890261|NAL birim uzunluk değeri geçersiz.|
|MPE_FILTER_INVALID|0x80890236|Geçersiz dinamik bildirim filtreleri.|
|MPE_FILTER_VERSION_INVALID|0x80890237|Geçersiz veya desteklenmeyen filtre sürümleri.|
|MPE_FILTER_TYPE_INVALID|0x80890238|Geçersiz filtre türü.|
|MPE_FILTER_RANGE_ATTRIBUTE_INVALID|0x80890239|Filtre tarafından geçersiz aralığı belirtildi.|
|MPE_FILTER_TRACK_ATTRIBUTE_INVALID|0x8089023A|Geçersiz izleme özniteliği filtre tarafından belirtilir.|
|MPE_FILTER_PRESENTATION_WINDOW_INVALID|0x8089023B|Geçersiz sunu penceresi uzunluğu filtre tarafından belirtilir.|
|MPE_FILTER_LIVE_BACKOFF_INVALID|0x8089023C|Geçersiz geri alma dinamik filtre tarafından belirtilir.|
|MPE_FILTER_MULTIPLE_SAME_TYPE_FILTERS|0x8089023D|Yalnızca bir absTimeInHNS öğesi eski filtreleri desteklenir.|
|MPE_FILTER_REMOVED_ALL_STREAMS|0x8089023E|Var. daha fazla akış tüm filtreler uygulandıktan sonra|
|MPE_FILTER_LIVE_BACKOFF_OVER_DVRWINDOW|0x8089023F|Canlı geri alma DVR penceredir.|
|MPE_FILTER_LIVE_BACKOFF_OVER_PRESENTATION_WINDOW|0x80890262|Canlı geri alma sunu penceresinden daha büyüktür.|
|MPE_FILTER_COMPOSITION_FILTER_COUNT_OVER_LIMIT|0x80890246|On (10) izin verilen en fazla varsayılan filtreler aştı.|
|MPE_FILTER_COMPOSITION_MULTIPLE_FIRST_QUALITY_OPERATOR_NOT_ALLOWED|0x80890248|Toplam istek filtreleri birden çok ilk video kalitesi işlecine izin verilmiyor.|
|MPE_FILTER_FIRST_QUALITY_ATTRIBUTE_INVALID|0x80890249|İlk kalite hızı öznitelik sayısı (1) olmalıdır.|
|MPE_HLS_SEGMENT_TOO_LARGE|0x80890243|HLS segment süresini üçte DVR penceresinin ve HLS vazgeçin daha küçük olmalıdır.|
|MPE_KEY_FRAME_INTERVAL_TOO_LARGE|0x808901FE|Parça süreleri yaklaşık 20 saniye veya daha az olmalıdır, veya giriş kalite düzeylerine zaman hizalı değil.|
|MPE_DTS_RESERVEDBOX_EXPECTED|0x80890105|DTS özgü hata sırasında ayrıştırma DTS kutusu içinde DTSSpecficBox sunmalıdır olduğunda ReservedBox bulamıyor.|
|MPE_DTS_INVALID_CHANNEL_COUNT|0x80890106|DTS özgü hata, DTS kutusu Ayrıştırma sırasında DTSSpecficBox içinde bulunan bir kanal yok.|
|MPE_DTS_SAMPLETYPE_MISMATCH|0x80890107|DTS özgü hata, DTSSpecficBox örnek türü eşleşmiyor.|
|MPE_DTS_MULTIASSET_DTSH_MISMATCH|0x80890108|DTS özgü hata DTSH örnek tür uyuşmazlığı çok varlık ayarlanır.|
|MPE_DTS_INVALID_CORESTREAM_SIZE|0x80890109|DTS özgü hata çekirdek akış boyutu geçersiz.|
|MPE_DTS_INVALID_SAMPLE_RESOLUTION|0x8089010A|Örnek çözüm DTS özgü hatası, geçersiz.|
|MPE_DTS_INVALID_SUBSTREAM_INDEX|0x8089010B|DTS özgü hata alt akış uzantı dizini geçersiz.|
|MPE_DTS_INVALID_BLOCK_NUM|0x8089010C|DTS özgü hata alt akış blok numarası geçersiz.|
|MPE_DTS_INVALID_SAMPLING_FREQUENCE|0x8089010D|DTS özgü hata örnekleme sıklığı geçersiz.|
|MPE_DTS_INVALID_REFCLOCKCODE|0x8089010E|DTS özgü hata alt akış uzantısı başvuru saati kodu geçersiz.|
|MPE_DTS_INVALID_SPEAKERS_REMAP|0x8089010F|DTS özgü hata konuşmacıları remap kümesi sayısı geçersiz.|

Şifreleme makaleler ve örnekler için bkz:

- [Kavram: içerik koruma](content-protection-overview.md)
- [Kavram: İçerik anahtar ilkeleri](content-key-policy-concept.md)
- [Kavram: Akış ilkeleri](streaming-policy-concept.md)
- [Örnek: AES şifreleme ile koruma](protect-with-aes128.md)
- [Örnek: DRM ile koruma](protect-with-drm.md)

Filtre yönergeler için bkz:

- [Kavram: olan dinamik bildirimler](filters-dynamic-manifest-overview.md)
- [Kavram: filtreleri](filters-concept.md)
- [Örnek: REST API ile filtre oluşturma](filters-dynamic-manifest-rest-howto.md)
- [Örnek: .NET ile filtre oluşturma](filters-dynamic-manifest-dotnet-howto.md)
- [Örnek: CLI ile filtre oluşturma](filters-dynamic-manifest-cli-howto.md)

Canlı makaleler ve örnekler için bkz:

- [Kavram: canlı akış genel bakış](live-streaming-overview.md)
- [Kavram: Canlı etkinlikler ve canlı çıkışları](live-events-outputs-concept.md)
- [Örnek: canlı akış Öğreticisi](stream-live-tutorial-with-api.md)

## <a name="416-range-not-satisfiable"></a>416 yeterli aralık yok

|Hata kodu|Onaltılık değer |Hata açıklaması|
|---|---|---|
|MPE_STORAGE_INVALID_RANGE|0x808900F1|Depolama işlemi hatası, geçersiz aralık HTTP 416 hata döndürdü.|

## <a name="500-internal-server-error"></a>500 İç Sunucu Hatası

İsteğin işlenmesi sırasında Media Services işleme devam etmesini engelleyen bazı hatayla karşılaşır.  

|Hata kodu|Onaltılık değer |Hata açıklaması|
|---|---|---|
|MPE_STORAGE_SOCKET_TIMEOUT|0x808900F4|Alınan ve çevrilmiş gelen Winhttp hata kodu ERROR_WINHTTP_TIMEOUT (0x00002ee2).|
|MPE_STORAGE_SOCKET_CONNECTION_ERROR|0x808900F5|Alınan ve çevrilmiş gelen Winhttp hata kodu ERROR_WINHTTP_CONNECTION_ERROR (0x00002efe).|
|MPE_STORAGE_SOCKET_NAME_NOT_RESOLVED|0x808900F6|Alınan ve çevrilmiş gelen Winhttp hata kodu error_wınhttp_name_not_resolved (0x00002ee7).|
|MPE_STORAGE_INTERNAL_ERROR|0x808900E6|Depolama işlemi hatası, HTTP 500 hataları birinin genel Internalerror.|
|MPE_STORAGE_OPERATION_TIMED_OUT|0x808900E7|Depolama işlemi hatası, HTTP 500 hataları birinin genel OperationTimedOut.|
|MPE_STORAGE_FAILURE|0x808900F2|Depolama işlemi hatası Internalerror veya OperationTimedOut diğer HTTP 500 hataları.|

## <a name="503-service-unavailable"></a>503 Hizmet Kullanılamıyor

Sunucu isteklerini almak şu anda belirlenemiyor. Bu hatanın nedeni aşırı isteklerine hizmet. Media Services: azaltma mekanizması, aşırı uzun istek hizmetinize uygulamalar için kaynak kullanımını kısıtlıyor.

> [!NOTE]
> Hata iletisi ve hata kodu dizesi nedeni hakkında daha ayrıntılı bilgi 503 hatası aldığınız almak için denetleyin. Bu hata, azaltma her zaman gelmez.
> 

|Hata kodu|Onaltılık değer |Hata açıklaması|
|---|---|---|
|MPE_STORAGE_SERVER_BUSY|0x808900E8|Depolama işlemi hatası, HTTP sunucu meşgul hatası 503 aldı.|

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, görüşlerinizi, güncelleştirmeleri alın

Kullanıma [Azure Media Services topluluğu](media-services-community.md) soru sorun, görüşlerinizi ve medya hizmetleri hakkında güncelleştirmeler almak farklı yollarını görmek için makaleyi.

## <a name="see-also"></a>Ayrıca bkz.

- [Kodlama hata kodları](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode)
- [Azure Media Services kavramları](concepts-overview.md)
- [Kotalar ve sınırlamalar](limits-quotas-constraints.md)

## <a name="next-steps"></a>Sonraki adımlar

[Örnek: Hata kodu ve ileti .NET ile ApiException erişmek](configure-connect-dotnet-howto.md#connect-to-the-net-client)
