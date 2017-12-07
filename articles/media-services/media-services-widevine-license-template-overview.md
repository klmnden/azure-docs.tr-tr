---
title: "Widevine lisans şablonu genel bakış | Microsoft Docs"
description: "Bu konu, Widevine lisansları yapılandırmak için kullanılan Widevine lisans şablonu genel bir bakış sağlar."
author: juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 0e6f1f05-7ed6-4ed6-82a0-0cc2182b075a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 68d519cd36d41728f57419cd6cecd2a79d65a4af
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="widevine-license-template-overview"></a>Widevine lisans şablonu genel bakış
Azure Media Services ve Widevine lisansları istek yapılandırmanıza olanak sağlar. Son kullanıcı player korumalı Widevine içeriğinizi oynatma denediğinde, bir istek lisans edinmeye yönelik lisans teslimat hizmeti gönderilir. Lisans hizmeti isteği onaylarsa, istemciye gönderilen ve şifresini çözmek ve belirtilen içeriği yürütmek için kullanılan lisans verir.

Widevine lisans isteği bir JSON iletisi olarak biçimlendirilir.  

>[!NOTE]
> Hiçbir değerlerle boş bir ileti oluşturmak yalnızca "{}" seçebilirsiniz ve bir lisans şablonu ile tüm varsayılanları oluşturulur. Çoğu durumda varsayılan çalışır. Örneğin, temel MS lisans teslimat senaryoları için her zaman varsayılan olmalıdır. "Sağlayıcı" ve "content_id" değerlerini ayarlamak gerekiyorsa bir sağlayıcı Google Widevine kimlik bilgileriyle eşleşmelidir.

    {  
       “payload”:“<license challenge>”,
       “content_id”: “<content id>” 
       “provider”: ”<provider>”
       “allowed_track_types”:“<types>”,
       “content_key_specs”:[  
          {  
             “track_type”:“<track type 1>”
          },
          {  
             “track_type”:“<track type 2>”
          },
          …
       ],
       “policy_overrides”:{  
          “can_play”:<can play>,
          “can persist”:<can persist>,
          “can_renew”:<can renew>,
          “rental_duration_seconds”:<rental duration>,
          “playback_duration_seconds”:<playback duration>,
          “license_duration_seconds”:<license duration>,
          “renewal_recovery_duration_seconds”:<renewal recovery duration>,
          “renewal_server_url”:”<renewal server url>”,
          “renewal_delay_seconds”:<renewal delay>,
          “renewal_retry_interval_seconds”:<renewal retry interval>,
          “renew_with_usage”:<renew with usage>
       }
    }

## <a name="json-message"></a>JSON iletisi
| Ad | Değer | Açıklama |
| --- | --- | --- |
| Yükü |Base64 kodlu dize |Bir istemci tarafından gönderilen lisans isteği. |
| content_id |Base64 kodlu dize |KeyId(s) ve içerik anahtarları için her content_key_specs.track_type türetilen için kullanılan tanımlayıcı. |
| Sağlayıcı |Dize |İçerik anahtarı ve ilkeleri ayarladıktan bakmak için kullanılır. MS anahtar teslim Widevine lisans teslim için kullanılırsa, bu parametre yoksayılır. |
| policy_name |Dize |Daha önce kaydedilmiş bir ilke adı. İsteğe bağlı |
| allowed_track_types |Enum |SD_ONLY veya SD_HD. Bir lisans anahtarları içerik denetimleri eklenmelidir |
| content_key_specs |dizi JSON yapısını bkz **içerik anahtarı belirtimlerin** aşağıda |Döndürülecek hangi içerik anahtarları hakkında daha ayrıntılı denetim. İçerik anahtarı Spec aşağıda ayrıntılı bilgi için bkz.  Allowed_track_types ve content_key_specs yalnızca biri belirtilebilir. |
| use_policy_overrides_exclusively |Boole değeri. TRUE veya false |Policy_overrides tarafından belirtilen ilke öznitelikler kullanın ve tüm daha önce depolanan ilke atlayabilirsiniz. |
| policy_overrides |JSON yapısındaki bkz **ilkesini geçersiz kılar** aşağıda |Bu lisans için ilke ayarları.  Bu varlık önceden tanımlanmış bir ilke olan olayda belirtilen bu değerler kullanılır. |
| session_init |JSON yapısındaki bkz **oturum başlatma** aşağıda |İsteğe bağlı verileri lisansı geçirildi. |
| parse_only |Boole değeri. TRUE veya false |Lisans isteği ayrıştırılır, ancak hiçbir lisans verilir. Ancak, lisans isteği yanıtta döndürülen form değerleri. |

## <a name="content-key-specs"></a>İçerik anahtarı özellikleri
Önceden var olan bir ilke yoksa, herhangi bir değeri içerik anahtarı Spec belirtmek için gerek yoktur.  Bu içerikle ilişkili önceden varolan ilke HDCP ve CGMS gibi çıkış koruma belirlemek için kullanılır.  Önceden var olan bir ilke Widevine lisans sunucusuyla kayıtlı değilse, içerik sağlayıcısına lisans isteği değerleri ekleyemezsiniz.   

Her content_key_specs seçeneği use_policy_overrides_exclusively bağımsız olarak tüm parçaları için belirtilmelidir. 

| Ad | Değer | Açıklama |
| --- | --- | --- |
| content_key_specs. track_type |Dize |Bir izleme türü adı. Content_key_specs lisans istekte belirtilmişse, tüm türleri açıkça izlemek belirttiğinizden emin olun. Bunun Sağlanamaması, son 10 saniye kayıttan yürütmek üzere hatasına neden olur. |
| content_key_specs  <br/> security_level |uint32 |Kayıttan yürütme için istemci sağlamlık gereksinimlerini tanımlar. <br/> 1 - yazılım tabanlı whitebox crypto gereklidir. <br/> 2 - yazılım şifreleme ve karıştırılmış bir kod çözücü gereklidir. <br/> 3 - anahtar malzemesi ve şifreleme işlemleri için bir donanım yedeklenmiş güvenilir yürütme ortamında gerçekleştirilmesi gerekir. <br/> 4 - şifreleme ve içeriği çözülmesi için bir donanım yedeklenmiş güvenilir yürütme ortamında gerçekleştirilmesi gerekir.  <br/> 5 - şifreleme, kod çözme ve (sıkıştırılmış ve sıkıştırılmamış) ortamının tüm işleme gerekir işlenecek bir donanım yedeklenmiş güvenilir yürütme ortamında. |
| content_key_specs <br/> required_output_protection.hdc |dize - şunlardan biri: HDCP_NONE, HDCP_V1, HDCP_V2 |HDCP gerekip gerekmediğini belirtir |
| content_key_specs <br/>anahtar |Base64 <br/>kodlu bir dize |Bu izleme için kullanılacak içerik anahtarı. Belirtilmişse, track_type veya key_id gereklidir.  Bu seçenek, içerik anahtarı oluşturun veya bir anahtarı aramak Widevine lisans sunucusu izin vererek yerine bu izleme için eklemesine içerik sağlayıcı sağlar. |
| content_key_specs.key_id |Base64 ile kodlanmış ikili, dizesi 16 bayt |Anahtar için benzersiz tanımlayıcı. |

## <a name="policy-overrides"></a>İlkeyi geçersiz kılar
| Ad | Değer | Açıklama |
| --- | --- | --- |
| policy_overrides. can_play |Boole değeri. TRUE veya false |Belirten bu kayıttan yürütme içeriğin izin verilir. Varsayılan false olur. |
| policy_overrides. can_persist |Boole değeri. TRUE veya false |Lisans çevrimdışı kullanım için geçici olmayan depolama için kalıcı gösterir. Varsayılan false olur. |
| policy_overrides. can_renew |Boolean true veya false |Bu lisans yenilenmesini izin verildiğini gösterir. TRUE ise, lisans süresi sinyal tarafından genişletilebilir. Varsayılan false olur. |
| policy_overrides. license_duration_seconds |Int64 |Bu özel lisans için zaman penceresini gösterir. 0 değeri sınır süresi gösterir. Varsayılan 0'dır. |
| policy_overrides. rental_duration_seconds |Int64 |Kayıttan yürütme izin sırada zaman penceresini gösterir. 0 değeri sınır süresi gösterir. Varsayılan 0'dır. |
| policy_overrides. playback_duration_seconds |Int64 |Lisans süre içinde kayıttan yürütme başladıktan sonra süreyi görüntüleme penceresi. 0 değeri sınır süresi gösterir. Varsayılan 0'dır. |
| policy_overrides. renewal_server_url |Dize |Bu lisans tüm sinyal (yenileme) istekleri belirtilen URL'ye yeniden yönlendirilen. Bu alan yalnızca can_renew doğru olduğunda kullanılır. |
| policy_overrides. renewal_delay_seconds |Int64 |Kaç saniye yenileme ilk denenmeden önce license_start_time sonra. Bu alan yalnızca can_renew doğru olduğunda kullanılır. Varsayılan değer 0'dır |
| policy_overrides. renewal_retry_interval_seconds |Int64 |Hata durumu oluştuysa sonraki Lisans yenileme istekleri arasında saniye cinsinden gecikme süresini belirtir. Bu alan yalnızca can_renew doğru olduğunda kullanılır. |
| policy_overrides. renewal_recovery_duration_seconds |Int64 |Hangi kayıttan yürütme sırasında yenileme devam izin verilen süreyi penceredir yapılmaya çalışılan, lisans sunucusu arka uç sorunlar nedeniyle henüz başarısız. 0 değeri sınır süresi gösterir. Bu alan yalnızca can_renew doğru olduğunda kullanılır. |
| policy_overrides. renew_with_usage |Boolean true veya false |Kullanım başlatıldığında, Lisans yenileme için gönderilen gösterir. Bu alan yalnızca can_renew doğru olduğunda kullanılır. |

## <a name="session-initialization"></a>Oturum başlatma
| Ad | Değer | Açıklama |
| --- | --- | --- |
| provider_session_token |Base64 kodlu dize |Bu oturum belirteci lisans geri gönderilir ve sonraki yenileme içinde yer alır.  Oturum belirteci oturumları kalıcı değildir. |
| provider_client_token |Base64 kodlu dize |Lisans yanıtta gönderilecek istemci belirteci.  Lisans isteği istemci belirteci içeriyorsa, bu değer yoksayılır. İstemci belirteci lisans oturumları korunur. |
| override_provider_client_token |Boole değeri. TRUE veya false |Bir istemci belirteci false ve lisans isteği içeriyorsa, istemci belirteci bu yapısında belirtilmiş olsa bile istek belirteci kullanın.  TRUE ise, her zaman bu yapısı içinde belirtilen belirteci kullanın. |

## <a name="configure-your-widevine-licenses-using-net-types"></a>.NET türlerini kullanarak, Widevine lisansları yapılandırma
Media Services .NET Widevine lisansları yapılandırmanıza olanak tanıyan API'ları sağlar. 

### <a name="classes-as-defined-in-the-media-services-net-sdk"></a>Media Services .NET SDK'ın tanımlanan sınıflar
Bu tür tanımları verilmiştir.

    public class WidevineMessage
    {
        public WidevineMessage();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public AllowedTrackTypes? allowed_track_types { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public ContentKeySpecs[] content_key_specs { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public object policy_overrides { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum AllowedTrackTypes
    {
        SD_ONLY = 0,
        SD_HD = 1
    }
    public class ContentKeySpecs
    {
        public ContentKeySpecs();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string key_id { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public RequiredOutputProtection required_output_protection { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int? security_level { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string track_type { get; set; }
    }

    public class RequiredOutputProtection
    {
        public RequiredOutputProtection();

        public Hdcp hdcp { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum Hdcp
    {
        HDCP_NONE = 0,
        HDCP_V1 = 1,
        HDCP_V2 = 2
    }

### <a name="example"></a>Örnek
Aşağıdaki örnekte basit Widevine lisansı yapılandırmak için .NET API'lerini kullanmayı gösterir.

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


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca bkz.
[PlayReady ve/veya Widevine dinamik ortak şifreleme kullanma](media-services-protect-with-playready-widevine.md)

