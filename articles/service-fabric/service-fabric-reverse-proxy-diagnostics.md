---
title: Azure Service Fabric ters proxy tanılama | Microsoft Docs
description: İzleme ve isteği işlemeye ters proxy Tanılama hakkında bilgi edinin.
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
ms.openlocfilehash: c9c8c649208cff95f4ee515d39cc8cca3e2c64bf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60726851"
---
# <a name="monitor-and-diagnose-request-processing-at-the-reverse-proxy"></a>İzleme ve isteği işlemeye ters proxy tanılama

Service Fabric 5.7 sürümünden itibaren ters proxy olayları koleksiyonu için kullanılabilir. Olayları yalnızca hata olaylarıyla istek işleme hatası ters proxy ve girişi başarılı ve başarısız istekler için Ayrıntılı olayları içeren ikinci bir kanal ile ilgili iki kanalda kullanılabilir.

Başvurmak [ters proxy olayları toplamak](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations) yerel ve Azure Service Fabric kümeleri bu kanallar toplama olayları etkinleştirmek için.

## <a name="troubleshoot-using-diagnostics-logs"></a>Tanılama günlüklerini kullanarak sorun giderme
Bir sınırlamasıyla yaygın hata günlüklerinizi yorumlama hakkında bazı örnekler şunlardır:

1. Ters proxy yanıt durum kodu 504 (zaman aşımı) döndürür.

    Nedenlerinden biri, istek zaman aşımı süresi içinde yanıt Hizmeti'nin nedeniyle olabilir.
   İlk olay günlükleri ters proxy alınan isteği ayrıntılarını aşağıda. 
   İkinci olay istek hizmetine ileten hata nedeniyle başarısız olduğunu gösterir. "İç hata ERROR_WINHTTP_TIMEOUT =" 

    Yükü içerir:

   * **traceId**: Bu GUID, tek bir istek için karşılık gelen tüm olayları ilişkilendirmek için kullanılabilir. İçinde iki olay, traceId aşağıda = **2f87b722-e254-4ac2-a802-fd315c1a0271**, ait oldukları aynı istekle olduğunu belirtmek.
   * **requestUrl**: İsteğin gönderildiği URL (ters proxy URL'si).
   * **Fiili**: HTTP fiili.
   * **remoteAddress**: İstemci isteği gönderen adresi.
   * **resolvedServiceUrl**: Gelen istek çözümlendiği hizmet uç noktası URL'si. 
   * **Hata ayrıntıları**: Hatayla ilgili ek bilgiler.

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
    
    Eşleşen hizmet uç noktasını bulmak başarısız olduğu ters proxy 404 döndüğü bir örnek olay aşağıdadır.
    Burada ilgi yükü girişler:
   * **processRequestPhase**: İstek işleme sırasında hatanın oluştuğu andaki aşamasını gösterir ***TryGetEndpoint*** yani iletmek için hizmet uç noktası getirilmeye çalışılırken. 
   * **Hata ayrıntıları**: Uç nokta arama ölçütlerini listeler. Burada, listenerName belirttiğiniz görebilirsiniz = **FrontEndListener** çoğaltma uç nokta listesi yalnızca ada sahip bir dinleyici içerirken **OldListener**.
    
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
     Ters proxy döndürdüğü 404 başka bir örnek olduğundan bulunamadı: ApplicationGateway\Http yapılandırma parametresi **SecureOnlyMode** ters proxy ile true olarak ayarlanırsa üzerinde dinleme **HTTPS**, ancak yineleme bitiş noktalarının tümü güvenli olmayan (HTTP dinleme).
     Bir uç nokta isteği iletmek için HTTPS dinleme bulunamıyor beri proxy döndürür 404 tersine çevirir. Yük sorunu daraltmak için yardımcı olur olay parametreleri analiz ediliyor:
    
     ```
      "errorDetails": "SecureOnlyMode = true, gateway protocol = https, listenerName = NewListener, replica endpoint = {\"Endpoints\":{\"OldListener\":\"Http:\/\/localhost:8491\/LocationApp\/\", \"NewListener\":\"Http:\/\/localhost:8492\/LocationApp\/\"}}"
     ```

3. İstek ters proxy için bir zaman aşımı hatası ile başarısız olur. 
    Olay günlükleri, olay (burada gösterilmiyor) alınan istek ayrıntılarla içeriyor.
    Sonraki olayı, hizmeti bir 404 durum kodu ile yanıt verdi ve ters proxy yeniden çözümleyebilir başlatır gösterir. 

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
    Tüm olayları toplayan her çözmek ve iletme girişimi gösteren olayların trene görürsünüz.
    İstek işleme başarılı çözümleme girişimi sayısı ile birlikte bir zaman aşımı ile başarısız oldu serideki son olayın gösterir.
    
    > [!NOTE]
    > Varsayılan olarak devre dışı ayrıntılı kanal olay koleksiyonu tutmak ve bir gereksinim olarak sorun giderme için etkinleştirmek için önerilir.

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
    
    Yalnızca kritik/hata olaylarını toplama etkinse, zaman aşımı ve çözümleme girişimi sayısını ayrıntılarını içeren bir olay bakın. 
    
    Kullanıcı için bir 404 durum kodu göndermek istediğiniz hizmetleri, bir "X-ServiceFabric" üst bilgisi yanıtta eklemelidir. Yanıt üst bilgisi eklendikten sonra ters proxy durum kodunu istemciye geri gönderir.  

4. İstemci isteği kesildiğinde durumları.

    Aşağıdaki olay ters Ara sunucu, istemciye yanıt iletme ancak istemci bağlantısını keser kaydedilir:

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

    Hizmet bildirimi içinde hizmet uç noktası için kullanılacak URI düzeni belirtilmediği takdirde FABRIC_E_SERVICE_DOES_NOT_EXIST hata döndürülür.

    ```
    <Endpoint Name="ServiceEndpointHttp" Port="80" Protocol="http" Type="Input"/>
    ```

    Sorunu çözmek için kullanılacak URI düzeni bildiriminde belirtin.
    ```
    <Endpoint Name="ServiceEndpointHttp" UriScheme="http" Port="80" Protocol="http" Type="Input"/>
    ```

> [!NOTE]
> Şu anda websocket isteğini işlemiyle ilgili olayları günlüğe kaydedilmez. Bu, sonraki sürümde eklenecek.

## <a name="next-steps"></a>Sonraki adımlar
* [Olay toplama ve Windows Azure Tanılama'yı kullanarak koleksiyon](service-fabric-diagnostics-event-aggregation-wad.md) Azure kümelerinde günlük toplamayı etkinleştirmek için.
* Visual Studio'da Service Fabric olayları görüntülemek için bkz: [İzleyici yerel olarak ve tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).
* Başvurmak [güvenli hizmetlerine bağlanmak için ters proxy yapılandırma](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) için Azure Resource Manager şablonu örnekleri yapılandırmak için farklı hizmet sertifikası ile ters proxy doğrulama seçenekleri güvenli.
* Okuma [Service Fabric ters proxy'si](service-fabric-reverseproxy.md) daha fazla bilgi için.
