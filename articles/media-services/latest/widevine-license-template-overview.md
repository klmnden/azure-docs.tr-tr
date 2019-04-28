---
title: Azure Media Services aracılığıyla widevine lisans şablonuna genel bakış | Microsoft Docs
description: Bu konu, Widevine lisansları yapılandırmak için kullanılan bir Widevine lisans şablonu genel bir bakış sağlar.
author: juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: c6fc363a7ab9de215647e371a9d3c846f8688bd5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61466332"
---
# <a name="widevine-license-template-overview"></a>Widevine lisans şablonuna genel bakış 

Azure Media Services ile içeriğinizi şifrelemenizi sağlar **Google Widevine**. Media Services, Widevine lisanslarını teslim etmek üzere bir hizmet de sağlar. Widevine lisansları yapılandırmak için Azure Media Services API'leri kullanabilirsiniz. Bir player, Widevine korumalı içeriğini oynatmak çalıştığında, bir istek için lisans teslimat hizmetinin lisansı edinmek için gönderilir. Lisans hizmeti isteği onaylarsa, hizmet lisansı verir. Bu, istemciye gönderilen ve şifresini çözmek ve belirtilen içeriğin yürütmek için kullanılır.

Widevine lisans isteği bir JSON iletisi olarak biçimlendirilir.  

>[!NOTE]
> Hiçbir değer ile yalnızca boş bir ileti oluşturabilirsiniz "{}." Ardından bir lisans şablonu varsayılan değerlerle oluşturulur. Varsayılan, çoğu durumda çalışır. Microsoft tabanlı lisans teslim senaryoları her zaman varsayılan değerleri kullanmanız gerekir. "Sağlayıcı" ve "content_id" değerlerini ayarlamak ihtiyacınız varsa, sağlayıcı Widevine kimlik bilgileriyle eşleşmelidir.

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
| yük |Base64 ile kodlanmış bir dize |Bir istemci tarafından gönderilen lisans isteği. |
| content_id |Base64 ile kodlanmış bir dize |Anahtar kimliği ve içerik türetmek için kullanılan tanımlayıcı her content_key_specs.track_type için anahtar. |
| sağlayıcı |string |İçerik anahtarları ve ilkeleri bakmak için kullanılır. Microsoft anahtar teslim Widevine lisans teslim için kullanılıyorsa, bu parametre yoksayılır. |
| policy_name |string |Önceden kaydedilmiş bir ilke adı. İsteğe bağlı. |
| allowed_track_types |Sabit listesi |SD_ONLY veya SD_HD. Anahtarları içerik denetimleri bir lisans dahil edilir. |
| content_key_specs |JSON dizisi yapıları "İçerik anahtarı özellikleri." bölümüne bakın  |Döndürülecek hangi içerik anahtarı üzerinde daha ayrıntılı denetim. Daha fazla bilgi için "İçerik anahtarı özellikleri." bölümüne bakın. Allowed_track_types ve content_key_specs değerleri yalnızca biri belirtilebilir. |
| use_policy_overrides_exclusively |Boole true veya false |Tüm daha önce depolanan ilke çıkarın ve policy_overrides tarafından belirtilen ilke öznitelikler kullanın. |
| policy_overrides |JSON yapısı, "İlkesini geçersiz kılar." bölümüne bakın |Bu lisans için ilke ayarları.  Bu varlık önceden tanımlanmış bir ilke olması durumunda bu belirtilen değerler kullanılır. |
| session_init |JSON yapısı, "Oturumu başlatma" bölümüne bakın |İsteğe bağlı verileri lisansı geçirilir. |
| parse_only |Boole true veya false |Lisans isteği ayrıştırılır ancak hiçbir lisans verilir. Ancak, değerler lisans istek yanıt olarak döndürülür. |

## <a name="content-key-specs"></a>İçerik anahtarı özellikleri
Önceden var olan bir ilke varsa, içerik anahtarı spec içinde herhangi bir değeri belirtmek için gerek yoktur. Bu içerikle ilişkili ilkeyi önceden mevcut olan yüksek bant genişlikli dijital içerik koruma (HDCP) ve kopyalama genel yönetim sistemi (CGMS) gibi bir çıkış koruması belirlemek için kullanılır. Önceden var olan bir ilke Widevine lisans sunucusuyla kayıtlı değilse içerik sağlayıcısına lisans isteğe değerleri ekleyebilir.   

Her bir content_key_specs değeri use_policy_overrides_exclusively seçeneği bağımsız olarak tüm parçaları için belirtilmelidir. 

| Ad | Değer | Açıklama |
| --- | --- | --- |
| content_key_specs. track_type |string |İzleme türü adı. Content_key_specs lisans istekte belirtilirse, tüm türleri açıkça izlemek belirttiğinizden emin olun. Bunun yapılmaması, 10 saniye kayıttan yürütme hatası sonuçlanır. |
| content_key_specs  <br/> security_level |uint32 |Kayıttan yürütme için istemci sağlamlık gereksinimleri tanımlar. <br/> -Yazılım tabanlı beyaz-box şifrelemesi gereklidir. <br/> -Yazılım şifreleme ve karıştırılmış bir kod çözücü gereklidir. <br/> -Anahtar malzemesi ve şifreleme işlemleri, donanım destekli güvenilir yürütme ortamı içinde gerçekleştirilmelidir. <br/> -Şifreleme ve içeriğini kod çözme, donanım destekli güvenilir yürütme ortamı içinde gerçekleştirilmelidir.  <br/> -Şifreleme, kod çözme ve tüm işleme (sıkıştırılmış ve sıkıştırılmamış) ortam içinde bir donanım destekli güvenilir yürütme ortamı işlenmelidir. |
| content_key_specs <br/> required_output_protection.hdc |dize, HDCP_NONE, HDCP_V1, HDCP_V2 biri |HDCP gerekli olup olmadığını belirtir. |
| content_key_specs <br/>anahtar |Base64-<br/>Kodlanmış dize |Bu izleme için kullanılacak içerik anahtarı. Belirtilmişse track_type veya key_id gereklidir. İçerik sağlayıcısı oluşturmak veya bir anahtarı aramak Widevine lisans sunucusu izin vermek yerine bu izleme için içerik anahtarını eklenmek üzere bu seçeneği kullanabilirsiniz. |
| content_key_specs.key_id |Base64 ile kodlanmış dize ikili, 16 bayt |Anahtar için benzersiz tanımlayıcı. |

## <a name="policy-overrides"></a>İlke geçersiz kılma
| Ad | Değer | Açıklama |
| --- | --- | --- |
| policy_overrides&#46;can_play |Boole true veya false |Gösterir, kayıttan yürütme içeriği izin verilir. Varsayılan değer false’tur. |
| policy_overrides&#46;can_persist |Boole true veya false |Kalıcı depolama için çevrimdışı kullanım için lisans kalıcı gösterir. Varsayılan değer false’tur. |
| policy_overrides&#46;can_renew |Boole true veya false |Bu Lisans yenileme izin verildiğini gösterir. TRUE ise lisansı süresi sinyal tarafından genişletilebilir. Varsayılan değer false’tur. |
| policy_overrides&#46;license_duration_seconds |Int64 |Bu belirli bir lisans için zaman penceresini gösterir. 0 değeri, süre sınırı olduğunu gösterir. Varsayılan 0'dır. |
| policy_overrides&#46;rental_duration_seconds |Int64 |Kayıttan yürütme izin sırada zaman penceresini gösterir. 0 değeri, süre sınırı olduğunu gösterir. Varsayılan 0'dır. |
| policy_overrides&#46;playback_duration_seconds |Int64 |Lisans süresi içinde kayıttan yürütme başladıktan sonra zaman görüntüleme penceresi. 0 değeri, süre sınırı olduğunu gösterir. Varsayılan 0'dır. |
| policy_overrides&#46;renewal_server_url |string |Bu lisans tüm sinyal (yenileme) istekleri belirtilen URL'ye yeniden yönlendirilir. Bu alan yalnızca can_renew doğruysa kullanılır. |
| policy_overrides&#46;renewal_delay_seconds |Int64 |Yenileme ilk denenmeden önce license_start_time kaç saniye. Bu alan yalnızca can_renew doğruysa kullanılır. Varsayılan 0'dır. |
| policy_overrides&#46;renewal_retry_interval_seconds |Int64 |Başarısız olması durumunda sonraki Lisans yenileme istekleri arasındaki saniye cinsinden gecikme süresini belirtir. Bu alan yalnızca can_renew doğruysa kullanılır. |
| policy_overrides&#46;renewal_recovery_duration_seconds |Int64 |Pencerenin zaman yenileme denemesi sırasında hangi kayıttan yürütme devam edebilirsiniz, ancak lisans sunucusu ile arka uç sorunları nedeniyle başarısız. 0 değeri, süre sınırı olduğunu gösterir. Bu alan yalnızca can_renew doğruysa kullanılır. |
| policy_overrides&#46;renew_with_usage |Boole true veya false |Lisans kullanımı başladığında yenilenmesinde gönderilir gösterir. Bu alan yalnızca can_renew doğruysa kullanılır. |

## <a name="session-initialization"></a>Oturum başlatma
| Ad | Değer | Açıklama |
| --- | --- | --- |
| provider_session_token |Base64 ile kodlanmış bir dize |Bu oturum belirteci lisans geri geçirilir ve sonraki yenileme içinde mevcut. Oturum belirteci oturumları sürdürmez. |
| provider_client_token |Base64 ile kodlanmış bir dize |Lisans yanıtta gönderilecek istemci belirteci. Lisans isteği istemci belirteci içeriyorsa bu değer yoksayılır. İstemci belirteci lisans oturumları devam ettirir. |
| override_provider_client_token |Boole true veya false |False ve lisans isteği istemci belirteci içeriyorsa isteği istekten belirteç bile istemci belirteci bu yapıda belirtildi kullanın. TRUE ise, her zaman bu yapıda belirtilen belirteci kullanın. |

## <a name="configure-your-widevine-license-with-net"></a>.NET ile Widevine lisansı yapılandırın 

Media Services, Widevine lisansı yapılandırmanıza olanak sağlayan bir sınıf sağlar. Lisans oluşturmak için JSON olarak geçirmek [WidevineTemplate](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.contentkeypolicywidevineconfiguration.widevinetemplate?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_ContentKeyPolicyWidevineConfiguration_WidevineTemplate).

Şablonu yapılandırmak için şunları yapabilirsiniz:

### <a name="directly-construct-a-json-string"></a>Doğrudan bir JSON dizesi oluşturun

Bu yöntem, hata yapmaya açık olabilir. Açıklanan başka bir yöntem kullanmak için önerilen [tanımlayın, sınıflar gerektiği ve JSON için seri hale getirme](#classes).

```csharp
ContentKeyPolicyWidevineConfiguration objContentKeyPolicyWidevineConfiguration = new ContentKeyPolicyWidevineConfiguration
{
    WidevineTemplate = @"{""allowed_track_types"":""SD_HD"",""content_key_specs"":[{""track_type"":""SD"",""security_level"":1,""required_output_protection"":{""hdcp"":""HDCP_V2""}}],""policy_overrides"":{""can_play"":true,""can_persist"":true,""can_renew"":false}}"
};
```

### <a id="classes"></a> Gerekli sınıfları tanımlayabilir ve JSON için seri hale getirme

#### <a name="define-classes"></a>Sınıfları tanımlama

Aşağıdaki örnek, Widevine JSON şemaya eşleme sınıfların tanımları örneği gösterir. JSON dizesine serileştirmeden önce sınıfları örneği oluşturabilir.  

```csharp
public class PolicyOverrides
{
    public bool CanPlay { get; set; }
    public bool CanPersist { get; set; }
    public bool CanRenew { get; set; }
    public int RentalDurationSeconds { get; set; }    //Indicates the time window while playback is permitted. A value of 0 indicates that there is no limit to the duration. Default is 0.
    public int PlaybackDurationSeconds { get; set; }  //The viewing window of time after playback starts within the license duration. A value of 0 indicates that there is no limit to the duration. Default is 0.
    public int LicenseDurationSeconds { get; set; }   //Indicates the time window for this specific license. A value of 0 indicates that there is no limit to the duration. Default is 0.
}

public class ContentKeySpec
{
    public string TrackType { get; set; }
    public int SecurityLevel { get; set; }
    public OutputProtection RequiredOutputProtection { get; set; }
}

public class OutputProtection
{
    public string HDCP { get; set; }
}

public class WidevineTemplate
{
    public string AllowedTrackTypes { get; set; }
    public ContentKeySpec[] ContentKeySpecs { get; set; }
    public PolicyOverrides PolicyOverrides { get; set; }
}
```

#### <a name="configure-the-license"></a>Lisans'ı yapılandırma

Önceki bölümde tanımlanan sınıflar yapılandırmak için kullanılan JSON oluşturulacağı [WidevineTemplate](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.contentkeypolicywidevineconfiguration.widevinetemplate?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_ContentKeyPolicyWidevineConfiguration_WidevineTemplate):

```csharp
private static ContentKeyPolicyWidevineConfiguration ConfigureWidevineLicenseTempate()
{
    WidevineTemplate template = new WidevineTemplate()
    {
        AllowedTrackTypes = "SD_HD",
        ContentKeySpecs = new ContentKeySpec[]
        {
            new ContentKeySpec()
            {
                TrackType = "SD",
                SecurityLevel = 1,
                RequiredOutputProtection = new OutputProtection()
                {
                    HDCP = "HDCP_V2"
                }
            }
        },
        PolicyOverrides = new PolicyOverrides()
        {
            CanPlay = true,
            CanPersist = true,
            CanRenew = false,
            RentalDurationSeconds = 2592000,
            PlaybackDurationSeconds = 10800,
            LicenseDurationSeconds = 604800,
        }
    };

    ContentKeyPolicyWidevineConfiguration objContentKeyPolicyWidevineConfiguration = new ContentKeyPolicyWidevineConfiguration
    {
        WidevineTemplate = Newtonsoft.Json.JsonConvert.SerializeObject(template)
    };
    return objContentKeyPolicyWidevineConfiguration;
}
```

## <a name="next-steps"></a>Sonraki adımlar

İşlemlerini nasıl gerçekleştirebileceğinizi [DRM ile koruma](protect-with-drm.md)
