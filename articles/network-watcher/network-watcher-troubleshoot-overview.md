---
title: Azure ağ izleyicisinde sorunları giderme kaynak giriş | Microsoft Docs
description: Bu sayfada Ağ İzleyicisi sorun giderme kaynak özelliklerine genel bakış sunulmaktadır.
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.assetid: c1145cd6-d1cf-4770-b1cc-eaf0464cc315
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: kumud
ms.openlocfilehash: aa7fce21228d4413dc4964d6e828bf60478aee27
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60682961"
---
# <a name="introduction-to-resource-troubleshooting-in-azure-network-watcher"></a>Azure ağ izleyicisinde sorunları giderme kaynak giriş

Sanal ağ geçitleri, şirket içi kaynaklara ve diğer Azure içindeki sanal ağlar arasında bağlantı sağlar. Ağ geçitlerinin ve bağlantılarının izlenmesi, iletişimin kesilmemesini güvence altına alma açısından kritik önem taşır. Ağ İzleyicisi, ağ geçitleri ve bağlantı sorunlarını giderme özelliği sağlar. Özelliği, portal, PowerShell, Azure CLI veya REST API çağrılabilir. Çağrıldığında, Ağ İzleyicisi ağ geçidi veya bağlantı durumunu tanılar ve uygun sonuçlarını döndürür. İstek uzun süren bir işlemdir. Tanılama tamamlandıktan sonra sonuçlar döndürülür.

![portal][2]

## <a name="results"></a>Sonuçlar

Döndürülen ilk sonuçlar kaynak durumunun genel bir resmini verir. Aşağıdaki bölümde gösterildiği gibi daha ayrıntılı bilgi için kaynaklar sağlanabilir:

Sorun giderme API döndürülen değerleri aşağıdaki listede verilmiştir:

* **startTime** -sorun giderme API çağrısı başlattığınızda bu değerdir.
* **endTime** -bu değer sorun giderme bittiğinde saattir.
* **kod** -tek tanılama hatası olursa uygun değil, değerdir.
* **Sonuçları** -sonuçlar, bağlantı veya sanal ağ geçidi üzerinde döndürülen sonuç koleksiyonudur.
    * **Kimliği** -bu değeri hata türüdür.
    * **Özet** -bu değeri hatasının bir özetidir.
    * **ayrıntılı** -bu değeri, hatanın ayrıntılı bir açıklamasını sağlar.
    * **recommendedActions** -bu özellik almak için önerilen eylemleri koleksiyonudur.
      * **actionText** -bu değer, hangi eylemin yapılacağını açıklayan metni içerir.
      * **actionUri** -bu değeri URI belgelerine nasıl davranmasını sağlar.
      * **actionUriText** -bu değer eylem metninin kısa bir açıklamasıdır.

Aşağıdaki tablolar kullanılabilir olan farklı hata türleri (yukarıdaki listede sonuçlardan kimliğinizle) gösterir ve Hata günlüklerini oluşturur.

### <a name="gateway"></a>Ağ geçidi

| Hata türü | Neden | Günlük|
|---|---|---|
| NoFault | Herhangi bir hata algılandığında |Evet|
| GatewayNotFound | Ağ geçidi veya ağ geçidi sağlanmamış bulunamıyor |Hayır|
| PlannedMaintenance |  Ağ geçidinin bakımda  |Hayır|
| UserDrivenUpdate | Bir kullanıcı güncelleştirme devam ederken bu hata oluşur. Güncelleştirme, yeniden boyutlandırma işlemi olabilir. | Hayır |
| VipUnResponsive | Birincil ağ geçidi örneğini bir sistem durumu araştırma hatası nedeniyle erişildiğinde bu hata oluşur. | Hayır |
| PlatformInActive | Platform ile ilgili bir sorun yoktur. | Hayır|
| ServiceNotRunning | Temel alınan hizmet çalışmıyor. | Hayır|
| NoConnectionsFoundForGateway | Ağ geçidi üzerinde bağlantı yok. Bu hata yalnızca bir uyarıdır.| Hayır|
| ConnectionsNotConnected | Bağlantı bağlı değildir. Bu hata yalnızca bir uyarıdır.| Evet|
| GatewayCPUUsageExceeded | Geçerli CPU kullanımı > %95 ağ geçididir. | Evet |

### <a name="connection"></a>Bağlantı

| Hata türü | Neden | Günlük|
|---|---|---|
| NoFault | Herhangi bir hata algılandığında |Evet|
| GatewayNotFound | Ağ geçidi veya ağ geçidi sağlanmamış bulunamıyor |Hayır|
| PlannedMaintenance | Ağ geçidinin bakımda  |Hayır|
| UserDrivenUpdate | Bir kullanıcı güncelleştirme devam ederken bu hata oluşur. Güncelleştirme, yeniden boyutlandırma işlemi olabilir.  | Hayır |
| VipUnResponsive | Birincil ağ geçidi örneğini bir sistem durumu araştırma hatası nedeniyle erişildiğinde bu hata oluşur. | Hayır |
| ConnectionEntityNotFound | Bağlantı yapılandırması eksik | Hayır |
| ConnectionIsMarkedDisconnected | Bağlantı "bağlantısız" olarak işaretlendi |Hayır|
| ConnectionNotConfiguredOnGateway | Temel alınan hizmete yapılandırılmış bağlantı yok. | Evet |
| ConnectionMarkedStandby | Temel alınan hizmete yedek olarak işaretlenir.| Evet|
| Authentication | Önceden paylaşılan anahtar uyumsuzluğu | Evet|
| PeerReachability | Eş Ağ Geçidi erişilebilir değil. | Evet|
| IkePolicyMismatch | Eş Ağ geçidi, Azure tarafından desteklenen IKE ilkeleri vardır. | Evet|
| WfpParse Error | WFP günlük ayrıştırılırken bir hata oluştu. |Evet|

## <a name="supported-gateway-types"></a>Desteklenen ağ geçidi türleri

Aşağıdaki tablo, hangi ağ geçitleriniz ve bağlantılarınızı desteklenen listeler Ağ İzleyicisi sorun giderme:

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

Kaynak sorun giderme işlemi tamamlandıktan sonra kaynak sorun giderme günlük dosyalarının bir depolama hesabında depolanır. Aşağıdaki görüntüde hatayla sonuçlanan çağrı örnek içeriğini gösterir.

![zip dosyası][1]

> [!NOTE]
> Bazı durumlarda, yalnızca bir alt kümesini günlük dosyaları için depolama yazılır.

Azure depolama hesaplarından dosyaları indirme ile ilgili yönergeler için başvurmak [.NET kullanarak Azure Blob depolamayı kullanmaya başlama](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Kullanılabilen başka bir Depolama Gezgini aracıdır. Depolama Gezgini hakkında daha fazla bilgi aşağıdaki bağlantıda burada bulunabilir: [Depolama Gezgini](https://storageexplorer.com/)

### <a name="connectionstatstxt"></a>ConnectionStats.txt

**ConnectionStats.txt** genel istatistikleri giriş ve çıkış baytlarını, bağlantı durumu ve bağlantı kuruldu saat bağlantı dosyası içerir.

> [!NOTE]
> Sorun giderme API çağrısı sağlıklı döndürürse, zip dosyasında verilen gereken tek şey olduğunu bir **ConnectionStats.txt** dosya.

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

Aşağıdaki örnek, bir IKEErrors.txt dosyanın içeriğini gösterir. Hatalarınızı soruna bağlı olarak farklı olabilir.

```
Error: Authentication failed. Check shared key. Check crypto. Check lifetimes. 
     based on log : Peer failed with Windows error 13801(ERROR_IPSEC_IKE_AUTH_FAIL)
Error: On-prem device sent invalid payload. 
     based on log : IkeFindPayloadInPacket failed with Windows error 13843(ERROR_IPSEC_IKE_INVALID_PAYLOAD)
```

### <a name="scrubbed-wfpdiagtxt"></a>Wfpdiag.txt iptal etti

**Scrubbed wfpdiag.txt** wfp günlük günlük dosyası içerir. Bu günlük, paket bırakma ve IKE/AuthIP hata günlük kaydı içerir.

Aşağıdaki örnek Scrubbed wfpdiag.txt dosyanın içeriğini gösterir. Bu örnekte, bağlantısı paylaşılan anahtarı altındaki üçüncü satırından görüldüğü gibi doğru değildi. Aşağıdaki örnek günlük soruna bağlı olabileceği tüm günlük yalnızca bir parçacığı aynıdır.

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

**Wfpdiag.txt.sum** arabellekleri ve işlenen olayların gösteren bir günlük dosyasıdır.

Aşağıdaki örnekte wfpdiag.txt.sum dosyasının içeriği bulunur.
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

Bir ağ geçidi veya ağ geçidi bağlantısı ile ilgili bir sorun Tanıla öğrenmek için bkz. [ağlar arasındaki iletişim sorunlarını tanılama](diagnose-communication-problem-between-networks.md).
<!--Image references-->

[1]: ./media/network-watcher-troubleshoot-overview/GatewayTenantWorkerLogs.png
[2]: ./media/network-watcher-troubleshoot-overview/portal.png
