---
title: Azure Ağ İzleyicisi sorun giderme kaynak giriş | Microsoft Docs
description: Bu sayfa Ağ İzleyicisi kaynak sorun giderme özellikleri genel bir bakış sağlar.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: c1145cd6-d1cf-4770-b1cc-eaf0464cc315
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: jdial
ms.openlocfilehash: 2f8a41834c1451d80c53cfed4bae3b7e36281702
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32779269"
---
# <a name="introduction-to-resource-troubleshooting-in-azure-network-watcher"></a>Azure Ağ İzleyicisi sorun giderme kaynak giriş

Sanal ağ geçitleri, şirket içi kaynakları ile Azure içindeki diğer sanal ağlar arasında bağlantı sağlar. İzleme ağ geçitleri ve bağlantıları için kritik iletişim sağlama bozuk değil. Ağ İzleyicisi ağ geçitleri ve bağlantıları sorun giderme yeteneği sağlar. Özelliği, portal, PowerShell, Azure CLI veya REST API çağrılabilir. Çağrıldığında, Ağ İzleyicisi ağ geçidi veya bağlantı durumunu tanılar ve uygun sonuçları döndürür. İstek uzun süren bir işlemdir. Tanılama tamamlandıktan sonra sonuç döndürülür.

![portal][2]

## <a name="results"></a>Sonuçlar

Döndürülen ön sonuçları kaynak sistem durumu genel bir bakış sağlar. Aşağıdaki bölümde gösterildiği gibi daha ayrıntılı bilgi için kaynaklar sağlanabilir:

Sorun giderme API döndürdüğü değer listesidir:

* **startTime** -sorun giderme API çağrısı başlatıldığında bu değerdir.
* **endTime** -bu değer sorun giderme ne zaman sona saattir.
* **kod** -tek tanılama hatası varsa bu sağlıksız, değerdir.
* **Sonuçları** -sonuçları bağlantı veya sanal ağ geçidi döndürülen sonuçları koleksiyonudur.
    * **Kimliği** -bu değer arıza türüdür.
    * **Özet** -bu değer hataya bir özetidir.
    * **ayrıntılı** -bu değer hatası için ayrıntılı bir açıklama sağlar.
    * **recommendedActions** -bu özellik almak için önerilen eylemleri koleksiyonudur.
      * **actionText** -bu değer, hangi eylemin yapılacağını açıklayan metin içerir.
      * **actionUri** -bu değer belgelerine URI üstlenmesini sağlar.
      * **actionUriText** -bu değer eylem metni kısa açıklamasıdır.

Aşağıdaki tablolar kullanılabilir farklı hata türleri (ID yukarıdaki listede sonuçlarından altında) göster ve hata günlükleri oluşturur.

### <a name="gateway"></a>Ağ geçidi

| Hata türü | Neden | Günlük|
|---|---|---|
| NoFault | Herhangi bir hata algılandığında |Evet|
| GatewayNotFound | Ağ geçidi veya ağ geçidi sağlanmamış bulunamıyor |Hayır|
| PlannedMaintenance |  Ağ geçidi örneği bakımda  |Hayır|
| UserDrivenUpdate | Bu hata, bir kullanıcı güncelleştirme devam ediyor oluşur. Güncelleştirmeyi yeniden boyutlandırma işlemi olabilir. | Hayır |
| VipUnResponsive | Bu hata oluşur. ağ geçidi birincil örneği durum araştırma hatası nedeniyle erişilemiyor. | Hayır |
| PlatformInActive | Platform ile ilgili bir sorun yoktur. | Hayır|
| ServiceNotRunning | Temel alınan hizmet çalışmıyor. | Hayır|
| NoConnectionsFoundForGateway | Hiçbir bağlantı ağ geçidi yok. Bu hata yalnızca bir uyarıdır.| Hayır|
| ConnectionsNotConnected | Bağlantıları bağlı değil. Bu hata yalnızca bir uyarıdır.| Evet|
| GatewayCPUUsageExceeded | Geçerli ağ geçidi CPU kullanımı % > 95 ' dir. | Evet |

### <a name="connection"></a>Bağlantı

| Hata türü | Neden | Günlük|
|---|---|---|
| NoFault | Herhangi bir hata algılandığında |Evet|
| GatewayNotFound | Ağ geçidi veya ağ geçidi sağlanmamış bulunamıyor |Hayır|
| PlannedMaintenance | Ağ geçidi örneği bakımda  |Hayır|
| UserDrivenUpdate | Bu hata, bir kullanıcı güncelleştirme devam ediyor oluşur. Güncelleştirmeyi yeniden boyutlandırma işlemi olabilir.  | Hayır |
| VipUnResponsive | Bu hata oluşur. ağ geçidi birincil örneği durum araştırma hatası nedeniyle erişilemiyor. | Hayır |
| ConnectionEntityNotFound | Bağlantı yapılandırması eksik | Hayır |
| ConnectionIsMarkedDisconnected | Bağlantı "bağlantısız" olarak işaretlenmiş |Hayır|
| ConnectionNotConfiguredOnGateway | Temel alınan hizmet yapılandırılmış bağlantısı yok. | Evet |
| ConnectionMarkedStandy | Temel alınan hizmet yedek olarak işaretlenir.| Evet|
| Kimlik Doğrulaması | Önceden paylaşılmış anahtar uyuşmuyor | Evet|
| PeerReachability | Eş Ağ Geçidi ulaşılabilir değil. | Evet|
| IkePolicyMismatch | Eş Ağ geçidi, Azure tarafından desteklenmez IKE ilkeleri vardır. | Evet|
| WfpParse hata | WFP günlük ayrıştırılırken bir hata oluştu. |Evet|

## <a name="supported-gateway-types"></a>Desteklenen ağ geçidi türleri

Hangi ağ geçitleri ve bağlantıları desteklenen aşağıdaki tabloda listelenmektedir Ağ İzleyicisi sorun giderme:

|  |  |
|---------|---------|
|**Ağ geçidi türleri**   |         |
|VPN      | Desteklenen        |
|ExpressRoute | Desteklenmiyor |
|**VPN türleri** | |
|Rota tabanlı | Desteklenen|
|İlke tabanlı | Desteklenmiyor|
|**Bağlantı türleri**||
|IPSec| Desteklenen|
|VNet2Vnet| Desteklenen|
|ExpressRoute| Desteklenmiyor|
|VPNClient| Desteklenmiyor|

## <a name="log-files"></a>Günlük dosyaları

Kaynak sorun giderme tamamlandıktan sonra kaynak sorun giderme günlük dosyalarının bir depolama hesabında depolanır. Aşağıdaki resimde hatayla sonuçlandı bir çağrı örnek içeriğini gösterir.

![zip dosyası][1]

> [!NOTE]
> Bazı durumlarda, yalnızca bir alt kümesini günlükleri dosyaları depolama alanına yazılır.

Azure depolama hesaplarından dosyaları indirme ile ilgili yönergeler için bkz [.NET kullanarak Azure Blob storage'ı kullanmaya başlama](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Kullanılabilir başka bir Depolama Gezgini aracıdır. Aşağıdaki bağlantıda Depolama Gezgini hakkında daha fazla bilgi şurada bulunabilir: [Depolama Gezgini](http://storageexplorer.com/)

### <a name="connectionstatstxt"></a>ConnectionStats.txt

**ConnectionStats.txt** dosyası bağlantının giriş ve çıkış baytlarını, bağlantı durumu ve bağlantının oluşturulduğu zaman dahil olmak üzere genel istatistikleri içerir.

> [!NOTE]
> Sorun giderme API çağrısı sağlıklı döndürürse, zip dosyasında döndürülen tek şey. bir **ConnectionStats.txt** dosya.

Bu dosyanın içeriğini aşağıdaki örneğe benzer:

```
Connectivity State : Connected
Remote Tunnel Endpoint :
Ingress Bytes (since last connected) : 288 B
Egress Bytes (Since last connected) : 288 B
Connected Since : 2/1/2017 8:22:06 PM
```

### <a name="cpustatstxt"></a>CPUStats.txt

**CPUStats.txt** CPU kullanımı ve test sırasında kullanılabilir bellek dosyası içerir.  Bu dosyanın içeriğini aşağıdaki örneğe benzer:

```
Current CPU Usage : 0 % Current Memory Available : 641 MBs
```

### <a name="ikeerrorstxt"></a>IKEErrors.txt

**IKEErrors.txt** dosyasını içeren IKE hataları izleme sırasında bulunamadı.

Aşağıdaki örnek, bir IKEErrors.txt dosyasının içeriğini gösterir. Hatalarınızı soruna bağlı olarak farklı olabilir.

```
Error: Authentication failed. Check shared key. Check crypto. Check lifetimes. 
     based on log : Peer failed with Windows error 13801(ERROR_IPSEC_IKE_AUTH_FAIL)
Error: On-prem device sent invalid payload. 
     based on log : IkeFindPayloadInPacket failed with Windows error 13843(ERROR_IPSEC_IKE_INVALID_PAYLOAD)
```

### <a name="scrubbed-wfpdiagtxt"></a>İptal etti wfpdiag.txt

**Scrubbed wfpdiag.txt** günlük dosyası wfp günlük içerir. Bu günlük paket bırakma ve IKE/AuthIP hataları günlüğe kaydedilmesini içerir.

Aşağıdaki örnek Scrubbed wfpdiag.txt dosyasının içeriğini gösterir. Bu örnekte, bir bağlantısı paylaşılan anahtarı altındaki üçüncü satırından görüldüğü gibi doğru değildi. Aşağıdaki örnekte yalnızca tüm günlük parçacığıyla aynıdır günlük sorun bağlı olarak uzun olabilir.

```
...
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Deleted ICookie from the high priority thread pool list
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|IKE diagnostic event:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Event Header:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Timestamp: 1601-01-01T00:00:00.000Z
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Flags: 0x00000106
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    Local address field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    Remote address field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    IP version field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  IP version: IPv4
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  IP protocol: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Local address: 13.78.238.92
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Remote address: 52.161.24.36
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Local Port: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Remote Port: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Application ID:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  User SID: <invalid>
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Failure type: IKE/Authip Main Mode Failure
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Type specific info:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Failure error code:0x000035e9
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    IKE authentication credentials are unacceptable
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Failure point: Remote
...
```

### <a name="wfpdiagtxtsum"></a>wfpdiag.txt.Sum

**Wfpdiag.txt.sum** arabellek ve işlenen olayların gösteren bir günlük dosyasıdır.

Aşağıdaki örnek wfpdiag.txt.sum dosyasının içeriğini aynıdır.
```
Files Processed:
    C:\Resources\directory\924336c47dd045d5a246c349b8ae57f2.GatewayTenantWorker.DiagnosticsStorage\2017-02-02T17-34-23\wfpdiag.etl
Total Buffers Processed 8
Total Events  Processed 2169
Total Events  Lost      0
Total Format  Errors    0
Total Formats Unknown   486
Elapsed Time            330 sec
+-----------------------------------------------------------------------------------+
|EventCount    EventName            EventType   TMF                                 |
+-----------------------------------------------------------------------------------+
|        36    ikeext               ike_addr_utils_c844  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|        12    ikeext               ike_addr_utils_c857  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|        96    ikeext               ike_addr_utils_c832  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|         6    ikeext               ike_bfe_callbacks_c133  1dc2d67f-8381-6303-e314-6c1452eeb529|
|         6    ikeext               ike_bfe_callbacks_c61  1dc2d67f-8381-6303-e314-6c1452eeb529|
|        12    ikeext               ike_sa_management_c5698  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|         6    ikeext               ike_sa_management_c8447  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c494  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c642  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|         6    ikeext               ike_sa_management_c3162  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c3307  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
```

## <a name="next-steps"></a>Sonraki adımlar

Bir ağ geçidi veya ağ geçidi bağlantısı olan bir sorunu tanılamak öğrenmek için bkz: [ağlar arasında iletişim sorunları tanılamak](diagnose-communication-problem-between-networks.md).
<!--Image references-->

[1]: ./media/network-watcher-troubleshoot-overview/GatewayTenantWorkerLogs.png
[2]: ./media/network-watcher-troubleshoot-overview/portal.png
