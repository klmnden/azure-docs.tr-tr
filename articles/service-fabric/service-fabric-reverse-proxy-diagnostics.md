---
title: Azure Service Fabric ters proxy tanılama | Microsoft Docs
description: İzleme ve tanılama ters proxy isteği işlemeyi öğrenin.
services: service-fabric
documentationcenter: .net
author: kavyako
manager: vipulm
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 08/08/2017
ms.author: kavyako
ms.openlocfilehash: 662fc124af71c1ce976037a3544f59e3cea54ef0
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="monitor-and-diagnose-request-processing-at-the-reverse-proxy"></a>İzleme ve istek işleme ters proxy tanılama

Service Fabric 5.7 sürümünden itibaren ters proxy olayları koleksiyonu için kullanılabilir. Olaylar, isteği işleme hatası ters proxy ve girişleri hem başarılı hem başarısız istekler için ayrıntılı olaylarla içeren ikinci kanal ilgili yalnızca hata olaylarıyla iki kanalda kullanılabilir.

Başvurmak [ters proxy olayları toplamak](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations) bu kanalları yerel ve Azure Service Fabric kümeleri toplama olaylarından etkinleştirmek için.

## <a name="troubleshoot-using-diagnostics-logs"></a>Tanılama günlükleri kullanarak sorun giderme
Bir karşılaşabilirsiniz ortak başarısız günlüklerinizi yorumlama hakkında bazı örnekler şunlardır:

1. Ters proxy yanıt durum kodu 504 (zaman aşımı) döndürür.

    Nedenlerinden biri, istek zaman aşımı süresi içinde yanıt vermek başarısız olan hizmet nedeni olabilir.
İlk olay günlüklerini aşağıda isteğinin ayrıntılarını ters proxy aldı. İkinci olay isteği hizmetine iletme sırasında nedeniyle başarısız olduğunu gösterir "İç hata ERROR_WINHTTP_TIMEOUT =" 

    Yükü içerir:

    *  **traceId**: Bu GUID, tek bir isteğine karşılık gelen tüm olayları ilişkilendirmek için kullanılabilir. İçinde iki olay traceId aşağıda = **2f87b722-e254-4ac2-a802-fd315c1a0271**, aynı isteğine ait oldukları olduğunu belirtmek.
    *  **requestUrl**: istek gönderildi URL (ters proxy URL).
    *  **Fiili**: HTTP fiili.
    *  **remoteAddress**: istemci isteği gönderilirken adresidir.
    *  **resolvedServiceUrl**: hizmet uç noktası URL'si için gelen istek çözülmüş. 
    *  **errorDetails**: hata hakkında ek bilgi.

    ```
    {
      "Timestamp": "2017-07-20T15:57:59.9871163-07:00",
      "ProviderName": "Microsoft-ServiceFabric",
      "Id": 51477,
      "Message": "2f87b722-e254-4ac2-a802-fd315c1a0271 Request url = https://localhost:19081/LocationApp/LocationFEService?zipcode=98052, verb = GET, remote (client) address = ::1, resolved service url = Https://localhost:8491/LocationApp/?zipcode=98052, request processing start time =     15:58:00.074114 (745,608.196 MSec) ",
      "ProcessId": 57696,
      "Level": "Informational",
      "Keywords": "0x1000000000000021",
      "EventName": "ReverseProxy",
      "ActivityID": null,
      "RelatedActivityID": null,
      "Payload": {
        "traceId": "2f87b722-e254-4ac2-a802-fd315c1a0271",
        "requestUrl": "https://localhost:19081/LocationApp/LocationFEService?zipcode=98052",
        "verb": "GET",
        "remoteAddress": "::1",
        "resolvedServiceUrl": "Https://localhost:8491/LocationApp/?zipcode=98052",
        "requestStartTime": "2017-07-20T15:58:00.0741142-07:00"
      }
    }

    {
      "Timestamp": "2017-07-20T16:00:01.3173605-07:00",
      ...
      "Message": "2f87b722-e254-4ac2-a802-fd315c1a0271 Error while forwarding request to service: response status code = 504, description = Reverse proxy Timeout, phase = FinishSendRequest, internal error = ERROR_WINHTTP_TIMEOUT ",
      ...
      "Payload": {
        "traceId": "2f87b722-e254-4ac2-a802-fd315c1a0271",
        "statusCode": 504,
        "description": "Reverse Proxy Timeout",
        "sendRequestPhase": "FinishSendRequest",
        "errorDetails": "internal error = ERROR_WINHTTP_TIMEOUT"
      }
    }
    ```

2. Ters proxy yanıt durum kodu 404 (bulunamadı) döndürür. 
    
    Eşleşen hizmet uç noktası bulmak başarısız olduğundan ters proxy 404 döndüğü bir örnek olay aşağıdadır.
    Burada ilgi yükü girişler:
    *  **processRequestPhase**: hatanın oluştuğu sırada isteği işleme sırasında aşamasını gösterir ***TryGetEndpoint*** yani iletmek için hizmet uç noktası getirilmeye çalışılırken. 
    *  **errorDetails**: uç noktası arama ölçütlerini listeler. Burada listenerName belirttiğiniz görebilirsiniz = **FrontEndListener** içerirken çoğaltma uç nokta listesi yalnızca ada sahip bir dinleyici **OldListener**.
    
    ```
    {
      ...
      "Message": "c1cca3b7-f85d-4fef-a162-88af23604343 Error while processing request, cannot forward to service: request url = https://localhost:19081/LocationApp/LocationFEService?ListenerName=FrontEndListener&zipcode=98052, verb = GET, remote (client) address = ::1, request processing start time = 16:43:02.686271 (3,448,220.353 MSec), error = FABRIC_E_ENDPOINT_NOT_FOUND, message = , phase = TryGetEndoint, SecureOnlyMode = false, gateway protocol = https, listenerName = FrontEndListener, replica endpoint = {\"Endpoints\":{\"\":\"Https:\/\/localhost:8491\/LocationApp\/\"}} ",
      "ProcessId": 57696,
      "Level": "Warning",
      "EventName": "ReverseProxy",
      "Payload": {
        "traceId": "c1cca3b7-f85d-4fef-a162-88af23604343",
        "requestUrl": "https://localhost:19081/LocationApp/LocationFEService?ListenerName=NewListener&zipcode=98052",
        ...
        "processRequestPhase": "TryGetEndoint",
        "errorDetails": "SecureOnlyMode = false, gateway protocol = https, listenerName = FrontEndListener, replica endpoint = {\"Endpoints\":{\"OldListener\":\"Https:\/\/localhost:8491\/LocationApp\/\"}}"
      }
    }
    ```
    Başka bir örneği burada ters proxy dönebilirsiniz 404 bulunamıyorsa: ApplicationGateway\Http yapılandırma parametresi **SecureOnlyMode** ters proxy ile true olarak ayarlandığında dinliyor **HTTPS**, ancak çoğaltma bitiş noktalarının tümü güvenli olmayan (HTTP dinleme).
    İstek iletmek HTTPS dinleme bir uç nokta bulunamadı bu yana proxy döndürür 404 ters çevrilir. Olay yükü daraltmak sorunu belirlemelerine yardımcı olur. parametreleri çözümleme:
    
    ```
        "errorDetails": "SecureOnlyMode = true, gateway protocol = https, listenerName = NewListener, replica endpoint = {\"Endpoints\":{\"OldListener\":\"Http:\/\/localhost:8491\/LocationApp\/\", \"NewListener\":\"Http:\/\/localhost:8492\/LocationApp\/\"}}"
    ```

3. Ters proxy isteği zaman aşımı hatasıyla başarısız oluyor. 
    Olay günlükleri (burada gösterilmiyor) alınan istek ayrıntılarını bir olay içeriyor.
    Sonraki olay hizmeti 404 durum koduyla yanıt verdi ve yeniden çözümlemek ters proxy başlatır gösterir. 

    ```
    {
      ...
      "Message": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e Request to service returned: status code = 404, status description = , Reresolving ",
      "Payload": {
        "traceId": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e",
        "statusCode": 404,
        "statusDescription": ""
      }
    }
    {
      ...
      "Message": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e Re-resolved service url = Https://localhost:8491/LocationApp/?zipcode=98052 ",
      "Payload": {
        "traceId": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e",
        "requestUrl": "Https://localhost:8491/LocationApp/?zipcode=98052"
      }
    }
    ```
    Tüm olayları toplanırken her çözümleyin ve iletme girişimi gösteren olayların tren bakın.
    Serideki son olay istek işleme başarılı Çöz girişimlerine yanı sıra zaman aşımı ile başarısız oldu gösterir.
    
    > [!NOTE]
    > Varsayılan olarak devre dışı ayrıntılı kanal olay toplama tutmak ve bir gereksinim temelinde sorun giderme için etkinleştirmek için önerilir.

    ```
    {
      ...
      "Message": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e Error while processing request: number of successful resolve attempts = 12, error = FABRIC_E_TIMEOUT, message = , phase = ResolveServicePartition,  ",
      "EventName": "ReverseProxy",
      ...
      "Payload": {
        "traceId": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e",
        "resolveCount": 12,
        "errorval": -2147017729,
        "errorMessage": "",
        "processRequestPhase": "ResolveServicePartition",
        "errorDetails": ""
      }
    }
    ```
    
    Koleksiyon yalnızca kritik/hata olayları etkinleştirilirse, zaman aşımı ve çözümleme girişimlerine ilişkin ayrıntıları içeren bir olay bakın. 
    
    404 durum kodu kullanıcıya geri göndermek istediğiniz hizmetleri eklemek bir "X-ServiceFabric" üstbilgisi yanıtta. Üst bilgi yanıtı eklendikten sonra ters proxy durum kodunu istemciye iletir.  

4. İstemci isteği kesildiğinde durumları.

    Aşağıdaki olay ters proxy istemci yanıtı iletme ancak istemci kesilene kaydedilir:

    ```
    {
      ...
      "Message": "6e2571a3-14a8-4fc7-93bb-c202c23b50b8 Unable to send response to client: phase = SendResponseHeaders, error = -805306367, internal error = ERROR_SUCCESS ",
      "ProcessId": 57696,
      "Level": "Warning",
      ...
      "EventName": "ReverseProxy",
      "Payload": {
        "traceId": "6e2571a3-14a8-4fc7-93bb-c202c23b50b8",
        "sendResponsePhase": "SendResponseHeaders",
        "errorval": -805306367,
        "winHttpError": "ERROR_SUCCESS"
      }
    }
    ```
5. Ters Proxy 404 FABRIC_E_SERVICE_DOES_NOT_EXIST döndürür

    Hizmet bildirimi içinde hizmet uç noktası için kullanılacak URI düzeni belirtilmezse FABRIC_E_SERVICE_DOES_NOT_EXIST hata döndürülür.

    ```
    <Endpoint Name="ServiceEndpointHttp" Port="80" Protocol="http" Type="Input"/>
    ```

    Sorunu gidermek için bildirimde URI düzeni belirtin.
    ```
    <Endpoint Name="ServiceEndpointHttp" UriScheme="http" Port="80" Protocol="http" Type="Input"/>
    ```

> [!NOTE]
> Şu anda Web yuvası isteği işlemiyle ilgili olaylar günlüğe kaydedilmez. Bu bir sonraki sürümde eklenecek.

## <a name="next-steps"></a>Sonraki adımlar
* [Olay toplama ve Windows Azure Tanılama'yı kullanarak koleksiyon](service-fabric-diagnostics-event-aggregation-wad.md) günlük toplama Azure kümelerinde etkinleştirme.
* Visual Studio'da Service Fabric olayları görüntülemek için bkz: [İzleyici ve yerel olarak tanılamak](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).
* Başvurmak [güvenli hizmetlerine bağlanmak için ters proxy yapılandırma](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) için Azure Resource Manager şablonu örnekleri yapılandırmak için farklı hizmet sertifikası ile ters proxy doğrulama seçenekleri güvenli.
* Okuma [Service Fabric ters proxy](service-fabric-reverseproxy.md) daha fazla bilgi için.
