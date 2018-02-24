---
title: "Azure'da Windows sanal makineleri için olayları zamanlanmış | Microsoft Docs"
description: "Azure meta veri hizmeti için Windows sanal makinelerde kullanarak olayları zamanlanan."
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: 
author: ericrad
manager: timlt
editor: 
tags: 
ms.assetid: 28d8e1f2-8e61-4fbe-bfe8-80a68443baba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/14/2017
ms.author: ericrad
ms.openlocfilehash: af860391d3aa810c2b5cd6d498de302d6f53bd3f
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
# <a name="azure-metadata-service-scheduled-events-for-windows-vms"></a>Azure meta veri hizmeti: Windows VM'ler için zamanlanmış olaylar

Zamanlanmış olaylar hizmettir bir Azure meta verileri, sanal makine bakımı için hazırlamak için uygulama zaman verir. Yaklaşan Bakımı olaylar hakkında bilgi sağlar (örn. yeniden başlatma) uygulamanız için hazırlamak ve sınırlamak için kesintisi. PaaS ve hem Windows hem de Linux Iaas dahil olmak üzere tüm Azure sanal makine türleri kullanılabilir. 

Linux üzerinde zamanlanmış olaylar hakkında bilgi için bkz: [Linux VM'ler için zamanlanmış olayları](../linux/scheduled-events.md).

[!IMPORTANT] Zamanlanmış olaylar tüm Azure bölgelerde genel olarak kullanılabilir. Bkz: [sürümü ve bölge kullanılabilirliği](#version-and-region-availability) en son sürüm bilgileri için.

## <a name="why-scheduled-events"></a>Neden zamanlanmış olayları?

Birçok uygulama, sanal makine bakımı için hazırlama zamandan yararlı olabilir. Zamanı kullanılabilirliği, güvenilirlik ve Bakım yapılabilirliğini dahil olmak üzere geliştirmek uygulama belirli görevler gerçekleştirmek üzere kullanılabilir: 

- Denetim noktası ve geri yükleme
- Bağlantı boşaltma
- Birincil çoğaltma yük devri 
- Yük Dengeleyici havuzu kaldırma
- Olay günlüğü
- Kapama 

Zamanlanmış olayları, uygulamanızın kullanarak bakım zaman ve ortaya etkisini sınırlamak için görevlerini tetikleyin bulabilir.  

Zamanlanmış olayları aşağıdaki kullanım durumlarda olayları sağlar:
- Başlatılan platform Bakım (örn. ana bilgisayar işletim sistemi güncelleştirmesi)
- Kullanıcı (örn. kullanıcı yeniden başlatır veya bir VM'yi yeniden dağıtır) bakım başlatılan

## <a name="the-basics"></a>Temel kavramları  

Azure meta veri hizmeti REST uç noktasını VM içinden erişilebilen kullanarak sanal makineleri çalıştırma hakkında bilgi gösterir. Böylece dışında VM gösterilmeyen bilgileri yönlendirilemeyen bir IP kullanılabilir.

### <a name="endpoint-discovery"></a>Uç nokta bulma
VNET VM'ler etkin için meta veri hizmeti bir statik yönlendirilemeyen bir IP, kullanılabilir `169.254.169.254`. Zamanlanmış olayları en son sürümü için tam uç noktadır: 

 > `http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01`

Bulut Hizmetleri ve klasik sanal makineleri için varsayılan durumlar bir sanal ağ içinde sanal makine oluşturulmamışsa ek mantık kullanılacak IP adresini bulmak için gereklidir. Bilgi edinmek için bu örneğe bakın nasıl [konak uç noktası bulunamadı](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).

### <a name="version-and-region-availability"></a>Sürümü ve bölge kullanılabilirliği
Zamanlanmış olayları sürümlü hizmetidir. Sürümleri zorunludur ve geçerli sürümü `2017-08-01`.

| Sürüm | Yayın türü | Bölgeler | Sürüm Notları | 
| - | - | - | - |
| 2017-08-01 | Genel Erişilebilirlik | Tümü | <li> Iaas VM'ler için kaynak adları alt çizgi $a kaldırıldı<br><li>Tüm istekler için zorlanan meta verileri üstbilgi gereksinimi | 
| 2017-03-01 | Önizleme | Tümü |<li>İlk yayın

> [!NOTE] 
> Önceki Önizleme sürümleri {son} API sürümü desteklenen zamanlanmış olaylar. Bu biçim artık desteklenmemektedir ve gelecekte kullanım dışı kalacaktır.

### <a name="enabling-and-disabling-scheduled-events"></a>Olayları zamanlanmış etkinleştirme ve devre dışı bırakma
Zamanlanmış olaylar olayları istekte hizmet ilk zaman için etkindir. Gecikmeli yanıt iki dakika içinde ilk çağrıda beklemelisiniz.

Zamanlanmış olaylar 24 saat için bir istek yapmadığında hizmetiniz için dışıdır.

### <a name="user-initiated-maintenance"></a>Kullanıcı tarafından başlatılan bakım
Sanal makine Bakımı Azure portalı üzerinden, API, CLI, kullanıcı tarafından başlatılan veya PowerShell zamanlanmış bir olayı sonuçlanır. Bu bakım hazırlık mantığı uygulamanıza test etmenizi sağlar ve kullanıcı tarafından başlatılan bakım için hazırlamak uygulamanızı sağlar.

Bir sanal makinenin yeniden başlatılması zamanlar türüne sahip bir olay `Reboot`. Bir sanal makineyi dağıtarak zamanlar türüne sahip bir olay `Redeploy`.

## <a name="using-the-api"></a>API'yi kullanma

### <a name="headers"></a>Üst bilgiler
Meta veri hizmeti sorguladığınızda başlık sağlamalısınız `Metadata:true` istek istemeden yönlendirilmeyen emin olmak için. `Metadata:true` Üstbilgisi tüm zamanlanmış olaylar istekler için gereklidir. Hata istek üstbilgisini meta veri hizmetinden hatalı istek yanıtı neden olur.

### <a name="query-for-events"></a>Olaylar için sorgu
Aşağıdaki çağrıyı yaparak zamanlanmış olaylar için sorgulama yapabilirsiniz:

#### <a name="powershell"></a>PowerShell
```
curl http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01 -H @{"Metadata"="true"}
```

Bir yanıt zamanlanmış olaylar dizisini içerir. Boş bir dizi var. şu anda zamanlanmış bir olay yok demektir.
Durumda Zamanlanmış olaylar olduğu yanıtı olaylar dizisini içerir: 
```
{
    "DocumentIncarnation": {IncarnationID},
    "Events": [
        {
            "EventId": {eventID},
            "EventType": "Reboot" | "Redeploy" | "Freeze",
            "ResourceType": "VirtualMachine",
            "Resources": [{resourceName}],
            "EventStatus": "Scheduled" | "Started",
            "NotBefore": {timeInUTC},              
        }
    ]
}
```

### <a name="event-properties"></a>Olay Özellikleri
|Özellik  |  Açıklama |
| - | - |
| EventId | Bu olay için genel benzersiz tanımlayıcı. <br><br> Örnek: <br><ul><li>602d9444-d2cd-49c7-8624-8643e7171297  |
| Olay türü | Bu olaya neden olan etkisi. <br><br> Değerler: <br><ul><li> `Freeze`: Sanal makine, birkaç saniye duraklatmak için zamanlandı. CPU askıya alındı, ancak bellek, açık dosyalar veya ağ bağlantıları üzerinde hiçbir etkisi yoktur. <li>`Reboot`: Sanal makine yeniden başlatma için planlanmıştır (kalıcı olmayan bellek kaybolur). <li>`Redeploy`: Sanal makine başka bir düğüme taşımak için planlanmıştır (kısa ömürlü diskleri kaybolur). |
| ResourceType | Bu olay etkiler kaynak türü. <br><br> Değerler: <ul><li>`VirtualMachine`|
| Kaynaklar| Bu olay etkiler kaynakların listesi. Bu en fazla bir makinelerden içeren garanti [güncelleştirme etki alanı](manage-availability.md), UD tüm makinelerde içerebilir ancak. <br><br> Örnek: <br><ul><li> ["FrontEnd_IN_0", "BackEnd_IN_0"] |
| Olay durumu | Bu olay durumu. <br><br> Değerler: <ul><li>`Scheduled`: Bu olay belirtilen süre sonra başlayacak şekilde zamanlanırsa `NotBefore` özelliği.<li>`Started`: Bu olay başlatıldı.</ul> Hayır `Completed` veya benzer durumu hiç sağlanır; olay tamamlandığında, artık olay döndürülür.
| NotBefore| Süre geçtikten sonra bu olay başlayabilir. <br><br> Örnek: <br><ul><li> 2016-09-19T18:29:47Z  |

### <a name="event-scheduling"></a>Olay planlama
Her olay zamanlanmış bir minimum süre gelecekte olay türüne bağlı. Bu süre bir olayın içinde yansıtılır `NotBefore` özelliği. 

|Olay türü  | Minimum dikkat edin |
| - | - |
| Dondurma| 15 dakika |
| Yeniden başlatma | 15 dakika |
| Yeniden dağıtım | 10 dakika |

### <a name="event-scope"></a>Olay kapsamı     
Zamanlanmış olayları teslim edilir:        
 - Bir bulut hizmetindeki tüm sanal makineler      
 - Tüm sanal makinelerin bir kullanılabilirlik kümesi      
 - Bir ölçek kümesi yerleştirme grubundaki tüm sanal makineler.         

Sonuç olarak, denetlemelisiniz `Resources` hangi VM'ler etkilenir olacak tanımlamak için olay alanındaki. 

### <a name="starting-an-event"></a>Bir olayı başlatılıyor 

Yaklaşan olay öğrenilen ve normal olarak kapatmak için mantığınızı tamamlandı sonra yaparak bekleyen olay onaylayabilirsiniz bir `POST` çağrısı ile meta veri hizmetine `EventId`. Bu, en düşük bildirim kısaltabilir Azure'a gösterir (uygunsa) süresi. 

Beklenen json aşağıdadır `POST` iste gövde. İstek listesi içermelidir `StartRequests`. Her `StartRequest` içeren `EventId` hızlandırmak istediğiniz olayı için:
```
{
    "StartRequests" : [
        {
            "EventId": {EventId}
        }
    ]
}
```

#### <a name="powershell"></a>PowerShell
```
curl -H @{"Metadata"="true"} -Method POST -Body '{"DocumentIncarnation":"5", "StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' -Uri http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01
```

> [!NOTE] 
> Bir olay aktarımının sağlayan tüm devam etmek olay `Resources` olay bildirir yalnızca sanal makine durumunda. Bu nedenle ilk makine olarak basit bildirim koordine etmek için bir kılavuz seçmediğiniz seçebileceği `Resources` alan.


## <a name="powershell-sample"></a>PowerShell örnek 

Aşağıdaki örnek, zamanlanmış olaylar için meta veri hizmeti sorgular ve her bekleyen olay onaylar.

```PowerShell
# How to get scheduled events 
function Get-ScheduledEvents($uri)
{
    $scheduledEvents = Invoke-RestMethod -Headers @{"Metadata"="true"} -URI $uri -Method get
    $json = ConvertTo-Json $scheduledEvents
    Write-Host "Received following events: `n" $json
    return $scheduledEvents
}

# How to approve a scheduled event
function Approve-ScheduledEvent($eventId, $docIncarnation, $uri)
{    
    # Create the Scheduled Events Approval Document
    $startRequests = [array]@{"EventId" = $eventId}
    $scheduledEventsApproval = @{"StartRequests" = $startRequests; "DocumentIncarnation" = $docIncarnation} 
    
    # Convert to JSON string
    $approvalString = ConvertTo-Json $scheduledEventsApproval

    Write-Host "Approving with the following: `n" $approvalString

    # Post approval string to scheduled events endpoint
    Invoke-RestMethod -Uri $uri -Headers @{"Metadata"="true"} -Method POST -Body $approvalString
}

function Handle-ScheduledEvents($scheduledEvents)
{
    # Add logic for handling events here
}

######### Sample Scheduled Events Interaction #########

# Set up the scheduled events URI for a VNET-enabled VM
$localHostIP = "169.254.169.254"
$scheduledEventURI = 'http://{0}/metadata/scheduledevents?api-version=2017-03-01' -f $localHostIP 

# Get events
$scheduledEvents = Get-ScheduledEvents $scheduledEventURI

# Handle events however is best for your service
Handle-ScheduledEvents $scheduledEvents

# Approve events when ready (optional)
foreach($event in $scheduledEvents.Events)
{
    Write-Host "Current Event: `n" $event
    $entry = Read-Host "`nApprove event? Y/N"
    if($entry -eq "Y" -or $entry -eq "y")
    {
        Approve-ScheduledEvent $event.EventId $scheduledEvents.DocumentIncarnation $scheduledEventURI 
    }
}
``` 

## <a name="next-steps"></a>Sonraki adımlar 

- Gözcü bir [zamanlanmış olayları Demo](https://channel9.msdn.com/Shows/Azure-Friday/Using-Azure-Scheduled-Events-to-Prepare-for-VM-Maintenance) Cuma azure'da. 
- Zamanlanmış olayları kod örnekleri gözden [Azure örneği meta verileri zamanlanmış olayları Github deposuna](https://github.com/Azure-Samples/virtual-machines-scheduled-events-discover-endpoint-for-non-vnet-vm)
- Kullanılabilen API'leri hakkında daha fazla bilgiyi [örneği meta veri hizmeti](instance-metadata-service.md).
- Hakkında bilgi edinin [planlı bakım azure'da Windows sanal makineler için](planned-maintenance.md).
