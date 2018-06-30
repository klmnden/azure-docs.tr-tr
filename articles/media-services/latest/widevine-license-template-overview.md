---
title: Azure Media Services aracılığıyla widevine lisans şablonu genel bakış | Microsoft Docs
description: Bu konu, Widevine lisansları yapılandırmak için kullanılan Widevine lisans şablonu genel bir bakış sağlar.
author: juliako
manager: cfowler
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: juliako
ms.openlocfilehash: e3af5efd253458401c13f6174d9567f932482eb0
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37133107"
---
# <a name="widevine-license-template-overview"></a>Widevine lisans şablonu genel bakış

Azure Media Services ve Google Widevine lisansları istek yapılandırmak için kullanabilirsiniz. Widevine korumalı içeriği yürütmek player çalıştığında, bir istek lisans edinmeye yönelik lisans teslimat hizmeti gönderilir. Lisans hizmeti isteği onaylarsa, hizmet lisansı verir. İstemciye gönderilen ve şifresini çözmek ve belirtilen içeriği yürütmek için kullanılır.

Widevine lisans isteği bir JSON iletisi olarak biçimlendirilir.  

>[!NOTE]
> Boş bir ileti ile değer yoksa, yalnızca oluşturabileceğiniz "{}." Ardından bir lisans şablonu varsayılan değerlerle oluşturulur. Çoğu durumda varsayılan çalışır. Microsoft tabanlı lisans teslimat senaryoları her zaman varsayılan değerleri kullanmanız gerekir. "Sağlayıcı" ve "content_id" değerlerini ayarlamak gerekiyorsa, bir sağlayıcı Widevine kimlik bilgileriyle eşleşmelidir.

    {  
       "payload":"<license challenge>",
       "content_id": "<content id>"
       "provider": "<provider>"
       "allowed_track_types":"<types>",
       "content_key_specs":[  
          {  
             "track_type":"<track type 1>"
          },
          {  
             "track_type":"<track type 2>"
          },
          …
       ],
       "policy_overrides":{  
          "can_play":<can play>,
          "can persist":<can persist>,
          "can_renew":<can renew>,
          "rental_duration_seconds":<rental duration>,
          "playback_duration_seconds":<playback duration>,
          "license_duration_seconds":<license duration>,
          "renewal_recovery_duration_seconds":<renewal recovery duration>,
          "renewal_server_url":"<renewal server url>",
          "renewal_delay_seconds":<renewal delay>,
          "renewal_retry_interval_seconds":<renewal retry interval>,
          "renew_with_usage”:<renew with usage>
       }
    }

## <a name="json-message"></a>JSON iletisi

| Ad | Değer | Açıklama |
| --- | --- | --- |
| yük |Base64 ile kodlanmış dize |Bir istemci tarafından gönderilen lisans isteği. |
| content_id |Base64 ile kodlanmış dize |Anahtar kimliği ve içerik türetmek için kullanılan tanımlayıcı her content_key_specs.track_type için anahtar. |
| Sağlayıcı |dize |İçerik anahtarı ve ilkeleri ayarladıktan bakmak için kullanılır. Microsoft anahtar teslim Widevine lisans teslim için kullanılırsa, bu parametre yoksayılır. |
| policy_name |dize |Daha önce kaydedilmiş bir ilke adı. İsteğe bağlı. |
| allowed_track_types |Enum |SD_ONLY veya SD_HD. Bir lisans anahtarları içerik denetimleri dahil edilir. |
| content_key_specs |JSON dizisi yapıları "İçerik anahtarı belirtimlerin." bölümüne bakın  |Geçirmiş denetim döndürmek için hangi içerik anahtarlar. Daha fazla bilgi için "İçerik anahtarı belirtimlerin." bölümüne bakın Allowed_track_types ve content_key_specs değerleri yalnızca biri belirtilebilir. |
| use_policy_overrides_exclusively |Boolean, true veya false |Tüm daha önce depolanan ilke atlayın ve policy_overrides tarafından belirtilen ilke öznitelikler kullanın. |
| policy_overrides |JSON yapısı, "İlkesini geçersiz kılar." bölümüne bakın |Bu lisans için ilke ayarları.  Bu varlık önceden tanımlanmış bir ilke olan olayda belirtilen bu değerler kullanılır. |
| session_init |JSON yapısı, "Oturum Başlatma" bölümüne bakın |İsteğe bağlı verileri lisansı geçirilir. |
| parse_only |Boolean, true veya false |Lisans isteği ayrıştırılır, ancak hiçbir lisans verilir. Ancak, lisans isteği değerlerinden yanıt olarak döndürülür. |

## <a name="content-key-specs"></a>İçerik anahtarı özellikleri
Önceden var olan bir ilke varsa, herhangi bir değerin içerik anahtar teknik özellikler belirtmek için gerek yoktur. Bu içerikle ilişkili önceden var olan ilke çıkış koruma, yüksek bant genişliği dijital içerik koruma (HDCP) ve kopyalama genel yönetim sistemi (CGMS) gibi belirlemek için kullanılır. Önceden var olan bir ilke ile Widevine lisans sunucusu kayıtlı değil, içerik sağlayıcısına lisans isteği değerleri ekleyemezsiniz.   

Her bir content_key_specs değeri use_policy_overrides_exclusively seçeneği bağımsız olarak tüm parçaları için belirtilmelidir. 

| Ad | Değer | Açıklama |
| --- | --- | --- |
| content_key_specs. track_type |dize |Bir izleme türü adı. Content_key_specs lisans istekte belirtilmişse, tüm türleri açıkça izlemek belirttiğinizden emin olun. 10 saniye kayıttan durum Sağlanamaması sonuçlanır. |
| content_key_specs  <br/> security_level |uint32 |Kayıttan yürütme için istemci sağlamlık gereksinimlerini tanımlar. <br/> -Yazılım tabanlı beyaz kutusunu şifrelemesi gereklidir. <br/> -Yazılım şifreleme ve karıştırılmış bir kod çözücü gereklidir. <br/> -Anahtar malzeme ve şifreleme işlemleri içinde donanım destekli güvenilir yürütme ortamı gerçekleştirilmesi gerekir. <br/> -Şifreleme ve içeriğini kod çözme içinde donanım destekli güvenilir yürütme ortamı gerçekleştirilmesi gerekir.  <br/> -Şifreleme, kod çözme ve tüm işlenmesini içinde donanım destekli güvenilir Yürütme Ortamı (sıkıştırılmış ve sıkıştırılmamış) medya yapılması gerekir. |
| content_key_specs <br/> required_output_protection.hdc |dize, HDCP_NONE, HDCP_V1, HDCP_V2 birini |HDCP gerekli olup olmadığını gösterir. |
| content_key_specs <br/>anahtar |Base64-<br/>kodlu bir dize |Bu izleme için kullanılacak içerik anahtarı. Belirtilmişse, track_type veya key_id gereklidir. İçerik sağlayıcı için içerik anahtarı oluşturun veya bir anahtarı aramak Widevine lisans sunucusu izin vererek yerine bu izleme için eklemesine bu seçeneği kullanabilirsiniz. |
| content_key_specs.key_id |Base64 kodlu dize ikili, 16 bayt |Anahtar için benzersiz tanımlayıcı. |

## <a name="policy-overrides"></a>İlkeyi geçersiz kılar
| Ad | Değer | Açıklama |
| --- | --- | --- |
| policy_overrides&#46;can_play |Boolean, true veya false |Belirten bu kayıttan yürütme içeriğin izin verilir. Varsayılan değer false’tur. |
| policy_overrides&#46;can_persist |Boolean, true veya false |Çevrimdışı kullanım için kalıcı depolama birimine lisans kalıcı gösterir. Varsayılan değer false’tur. |
| policy_overrides&#46;can_renew |Boolean, true veya false |Bu lisans yenilenmesini izin verildiğini gösterir. TRUE ise, lisans süresi sinyal tarafından genişletilebilir. Varsayılan değer false’tur. |
| policy_overrides&#46;license_duration_seconds |Int64 |Bu özel lisans için zaman penceresini gösterir. 0 değeri sınır süresi gösterir. Varsayılan 0'dır. |
| policy_overrides&#46;rental_duration_seconds |Int64 |Kayıttan yürütme izin sırada zaman penceresini gösterir. 0 değeri sınır süresi gösterir. Varsayılan 0'dır. |
| policy_overrides&#46;playback_duration_seconds |Int64 |Lisans süre içinde kayıttan yürütme başladıktan sonra zaman görüntüleme penceresi. 0 değeri sınır süresi gösterir. Varsayılan 0'dır. |
| policy_overrides&#46;renewal_server_url |dize |Bu lisans tüm sinyal (yenileme) istekleri belirtilen URL'ye yeniden yönlendirilir. Bu alan yalnızca can_renew true ise kullanılır. |
| policy_overrides&#46;renewal_delay_seconds |Int64 |Kaç saniye yenileme ilk denenmeden önce license_start_time sonra. Bu alan yalnızca can_renew true ise kullanılır. Varsayılan 0'dır. |
| policy_overrides&#46;renewal_retry_interval_seconds |Int64 |Hata durumu oluştuysa sonraki Lisans yenileme istekleri arasında saniye cinsinden gecikme süresini belirtir. Bu alan yalnızca can_renew true ise kullanılır. |
| policy_overrides&#46;renewal_recovery_duration_seconds |Int64 |Pencerenin zaman yenileme denemesi sırasında hangi kayıttan yürütme devam edebilirsiniz, ancak lisans sunucusu arka uç sorunlar nedeniyle başarısız. 0 değeri sınır süresi gösterir. Bu alan yalnızca can_renew true ise kullanılır. |
| policy_overrides&#46;renew_with_usage |Boolean, true veya false |Lisans kullanımı başladığında için yenileme gönderilir gösterir. Bu alan yalnızca can_renew true ise kullanılır. |

## <a name="session-initialization"></a>Oturum başlatma
| Ad | Değer | Açıklama |
| --- | --- | --- |
| provider_session_token |Base64 ile kodlanmış dize |Bu oturum belirteci lisans geri gönderilir ve sonraki yenileme içinde yok. Oturum belirteci oturumları kalıcı değildir. |
| provider_client_token |Base64 ile kodlanmış dize |Lisans yanıtta gönderilecek istemci belirteci. Lisans isteği istemci belirteci içeriyorsa, bu değer yoksayılır. İstemci belirteci lisans oturumları devam ettirir. |
| override_provider_client_token |Boolean, true veya false |Bir istemci belirteci false ve lisans isteği içeriyorsa, istemci belirteci bu yapısında belirtilmiş olsa bile istek belirteci kullanın. TRUE ise, her zaman bu yapısı içinde belirtilen belirteci kullanın. |

## <a name="configure-your-widevine-license-with-net"></a>.NET ile Widevine lisansı yapılandırın 

Media Services Widevine lisansı yapılandırmanıza olanak sağlayan bir sınıf sağlar. Lisans oluşturmak için JSON olarak geçirmek [WidevineTemplate](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.contentkeypolicywidevineconfiguration.widevinetemplate?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_ContentKeyPolicyWidevineConfiguration_WidevineTemplate).

Şablonu yapılandırmak için aşağıdakileri yapabilirsiniz:

1.  Doğrudan yapı/stillerinizin (hataya yatkın olabilir) bir JSON dize;

    ```csharp
        ContentKeyPolicyWidevineConfiguration objContentKeyPolicyWidevineConfiguration = new ContentKeyPolicyWidevineConfiguration
    {
        WidevineTemplate = @"{""allowed_track_types"":""SD_HD"",""content_key_specs"":[{""track_type"":""SD"",""security_level"":1,""required_output_protection"":{""hdcp"":""HDCP_V2""}}],""policy_overrides"":{""can_play"":true,""can_persist"":true,""can_renew"":false}}"
    };
    ```

2.  Yapı JSON özniteliklerle eşleme özellikleri ile sınıfları gerekli ve JSON dizeye serileştirme önce örneği. Bu tür sınıflar örneği ve nasıl örneğinin serileştirilmiş ve aşağıdadır.

    ```csharp
    public class policy_overrides
    {
        public bool can_play { get; set; }
        public bool can_persist { get; set; }
        public bool can_renew { get; set; }
        public int rental_duration_seconds { get; set; }    //Indicates the time window while playback is permitted. A value of 0 indicates that there is no limit to the duration. Default is 0.
        public int playback_duration_seconds { get; set; }  //The viewing window of time after playback starts within the license duration. A value of 0 indicates that there is no limit to the duration. Default is 0.
        public int license_duration_seconds { get; set; }   //Indicates the time window for this specific license. A value of 0 indicates that there is no limit to the duration. Default is 0.
    }

    public class content_key_spec
    {
        public string track_type { get; set; }
        public int security_level { get; set; }
        public output_protection required_output_protection { get; set; }
    }

    public class output_protection
    {
        public string hdcp { get; set; }
    }

    public class widevine_template
    {
        public string allowed_track_types { get; set; }
        public content_key_spec[] content_key_specs { get; set; }
        public policy_overrides policy_overrides { get; set; }
    }
    ```

### <a name="configure-the-license"></a>Lisans yapılandırın

Önceki bölümde tanımlanan sınıflar yapılandırmak için kullanılan JSON oluşturulacağı [WidevineTemplate](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.contentkeypolicywidevineconfiguration.widevinetemplate?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_ContentKeyPolicyWidevineConfiguration_WidevineTemplate):

```csharp
void ConfigureLicense()
{
    widevine_template objwidevine_template = new widevine_template()
    {
        allowed_track_types = "SD_HD",
        content_key_specs = new content_key_spec[]
        {
            new content_key_spec()
            {
                track_type = "SD",
                security_level = 1,
                required_output_protection = new output_protection()
                {
                hdcp = "HDCP_V2"
                }
            }
        },
        policy_overrides = new policy_overrides()
        {
            can_play = true,
            can_persist = true,
            can_renew = false,
            license_duration_seconds = 2592000,
            playback_duration_seconds = 10800,
            rental_duration_seconds = 604800,
        }
    };

    ContentKeyPolicyWidevineConfiguration objContentKeyPolicyWidevineConfiguration = new ContentKeyPolicyWidevineConfiguration
    {
        WidevineTemplate = Newtonsoft.Json.JsonConvert.SerializeObject(objwidevine_template)
    };
}
```

## <a name="next-steps"></a>Sonraki adımlar

[Genel Bakış](content-protection-overview.md)