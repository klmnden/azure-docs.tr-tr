---
title: Azure'da Windows VM'ler için zamanlanmış olayları | Microsoft Docs
description: Zamanlanmış olaylar Azure meta veri hizmeti için Windows sanal makinelerinde kullanma.
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: ''
author: ericrad
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: 28d8e1f2-8e61-4fbe-bfe8-80a68443baba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2018
ms.author: ericrad
ms.openlocfilehash: ab0aefd5650aada9c301115813a80747ddd1f2ac
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64926312"
---
# <a name="azure-metadata-service-scheduled-events-for-windows-vms"></a>Azure meta veri hizmeti: Windows Vm'leri için zamanlanmış olaylar

Zamanlanmış olaylar, sanal makine bakım için hazırlamak için uygulama süresi sunan Azure meta veri hizmetidir. Yaklaşan bakımın olaylar hakkında bilgi sağlar (örn: yeniden başlatma) uygulamanız için hazırlamak ve sınırlamak kesilme. PaaS ve Iaas hem Windows hem de Linux da dahil olmak üzere tüm Azure sanal makine türleri için kullanılabilir. 

Linux üzerinde zamanlanmış olaylar hakkında bilgi için bkz: [Linux VM'ler için zamanlanmış olaylar](../linux/scheduled-events.md).

> [!Note] 
> Zamanlanmış olaylar tüm Azure bölgelerinde genel kullanıma sunuldu. Bkz: [sürümü ve bölge kullanılabilirliği](#version-and-region-availability) en son sürüm bilgileri için.

## <a name="why-scheduled-events"></a>Neden zamanlanmış olaylar?

Birçok uygulama, sanal makine bakım için hazırlanmanıza zamandan yararlı olabilir. Zaman, uygulama kullanılabilirliği, güvenilirlik ve Bakım yapılabilirliğini dahil olmak üzere geliştirmek belirli görevleri gerçekleştirmek için kullanılabilir: 

- Denetim noktası ve geri yükleme
- Bağlantı boşaltma
- Birincil çoğaltma yük devri 
- Yük Dengeleyici havuzu kaldırma
- Olay günlüğü
- Normal şekilde kapatılmasını 

Zamanlanmış olaylar, uygulamanızın kullanarak bakım zaman ve ortaya etkisini sınırlamak için görevlerini tetikleyin bulabilir. Zamanlanmış olaylar etkinleştirme sanal makinenizin bakım etkinliği gerçekleştiren önce geçmesi gereken süreyi en düşük düzeyde sağlar. Ayrıntılar için olay planlama bölümüne altına bakın.

Zamanlanmış olaylar, olayları aşağıdaki kullanım örnekleri sağlar:
- Platform tarafından başlatılan Bakım (örneğin konak işletim sistemi güncelleştirmesi)
- Düzeyi düşürülmüş donanım
- Kullanıcı tarafından başlatılan Bakım (örneğin kullanıcı yeniden başlatır veya bir sanal makine yeniden dağıtır)
- [Düşük öncelikli VM çıkarma](https://azure.microsoft.com/blog/low-priority-scale-sets) içinde ölçek kümeleri

## <a name="the-basics"></a>Temel bilgileri  

Azure meta veri hizmeti REST uç noktasını VM içinden erişilebilir kullanarak sanal makineleri çalıştırma hakkında bilgi gösterir. Böylece VM dışında gösterilmeyen bilgileri yönlendirilemeyen bir IP kullanılabilir.

### <a name="endpoint-discovery"></a>Uç nokta bulma
Sanal ağ özellikli VM'ler için bir statik yönlendirilemeyen IP, meta veri hizmeti kullanılabilir `169.254.169.254`. Zamanlanmış olaylar en son sürümü için tam uç noktadır: 

 > `http://169.254.169.254/metadata/scheduledevents?api-version=2017-11-01`

Sanal makineyi bir sanal ağdaki bulut Hizmetleri ve klasik VM'ler için varsayılan durumlar oluşturulmazsa ek mantık kullanılacak IP adresini bulmak için gereklidir. Bilgi edinmek için bu örnek başvuru nasıl [konak son noktasını Bul](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).

### <a name="version-and-region-availability"></a>Sürümü ve bölge kullanılabilirliği
Zamanlanmış olaylar tutulan hizmetidir. Sürümleri zorunludur ve geçerli sürümü `2017-11-01`.

| Version | Yayın türü | Bölgeler | Sürüm Notları | 
| - | - | - | - |
| 2017-11-01 | Genel Erişilebilirlik | Tümü | <li> Düşük öncelikli VM çıkarma EventType 'Preempt' desteği eklendi<br> | 
| 2017-08-01 | Genel Erişilebilirlik | Tümü | <li> Iaas Vm'leri için kaynak adları alt çizgi başına kaldırıldı<br><li>Tüm istekler için zorlanan meta veri üst bilgisi gereksinimi | 
| 2017-03-01 | Önizleme | Tümü |<li>İlk yayın

> [!NOTE] 
> Önceki Önizleme sürümlerinde zamanlanmış olaylar {son} api-version desteklenir. Bu biçim, artık desteklenmemektedir ve gelecekte kullanım dışı bırakılacaktır.

### <a name="enabling-and-disabling-scheduled-events"></a>Zamanlanmış olaylar etkinleştirme ve devre dışı bırakma
Zamanlanmış olaylar, olaylar için bir istekte hizmeti ilk zaman için etkindir. İlk iki dakikaya kadar çağrınızda Gecikmeli yanıt beklemelisiniz. Düzenli aralıklarla durum gerçekleştirilmekte olan bakım etkinliklerinin yanı sıra yaklaşan bakım olayları algılamak için uç nokta sorgu.

Zamanlanmış olaylar 24 saat için bir istek yapmadığında hizmetiniz için dışıdır.

### <a name="user-initiated-maintenance"></a>Kullanıcı tarafından başlatılan bakım
Kullanıcı tarafından başlatılan Azure portal, API, CLI aracılığıyla sanal makine Bakımı veya PowerShell zamanlanmış bir olayı oluşur. Bu bakım hazırlık mantıksal uygulamanızı test etmenizi sağlar ve uygulamanızın kullanıcı tarafından başlatılan bakım için hazırlanmanıza sağlar.

Zamanlar bir olay türüne sahip bir sanal makineyi yeniden başlatarak `Reboot`. Bir olay türüne sahip bir sanal makine yeniden dağıtılıyor zamanlar `Redeploy`.

## <a name="using-the-api"></a>API'yi kullanma

### <a name="headers"></a>Üst bilgiler
Meta veri hizmetine sorguladığınızda, başlık sağlamalısınız `Metadata:true` istek istemeden yönlendirilmeyen emin olmak için. `Metadata:true` Üstbilgi tüm zamanlanmış olaylar istekler için gereklidir. İstekte üstbilgisini hatası meta veri hizmeti hatalı istek yanıtından neden olur.

### <a name="query-for-events"></a>Sorgu olayları
Zamanlanmış olaylar için aşağıdaki çağrıyı yaparak sorgulayabilirsiniz:

#### <a name="powershell"></a>PowerShell
```
curl http://169.254.169.254/metadata/scheduledevents?api-version=2017-11-01 -H @{"Metadata"="true"}
```

Yanıt, zamanlanmış olaylar dizisini içerir. Boş bir dizi var. şu anda zamanlanmış bir olayı yok anlamına gelir.
Bu durumda Zamanlanmış olaylar olduğunda, bir dizi olay yanıtı içerir: 
```
{
    "DocumentIncarnation": {IncarnationID},
    "Events": [
        {
            "EventId": {eventID},
            "EventType": "Reboot" | "Redeploy" | "Freeze" | "Preempt",
            "ResourceType": "VirtualMachine",
            "Resources": [{resourceName}],
            "EventStatus": "Scheduled" | "Started",
            "NotBefore": {timeInUTC},
        }
    ]
}
```
DocumentIncarnation ETag ve olayları yükü son sorgu bu yana değişmişse incelemek için kolay bir yol sağlar.

### <a name="event-properties"></a>Olay Özellikleri
|Özellik  |  Açıklama |
| - | - |
| EventID | Bu olay için genel benzersiz tanımlayıcı. <br><br> Örnek: <br><ul><li>602d9444-d2cd-49c7-8624-8643e7171297  |
| EventType | Bu olaya neden olan etkisi. <br><br> Değerler: <br><ul><li> `Freeze`: Sanal makine, birkaç saniye için duraklatır şekilde zamanlanır. CPU askıya alındı, ancak bellek, açık dosyalar ve ağ bağlantıları üzerinde etkisi yoktur. <li>`Reboot`: Sanal makine için yeniden başlatma zamanlanır (kalıcı olmayan bellek kaybolur). <li>`Redeploy`: Sanal makineyi başka bir düğüme taşımak üzere zamanlanmış (kısa ömürlü diskleri kaybolur). <li>`Preempt`: Düşük öncelikli sanal makine siliniyor (kısa ömürlü diskleri kaybolur).|
| ResourceType | Bu olay etkiler kaynak türü. <br><br> Değerler: <ul><li>`VirtualMachine`|
| Kaynaklar| Bu olay etkiler kaynakların listesi. Bu en çok bir makinelerden içeren garanti [güncelleme etki alanı](manage-availability.md), ancak UD içindeki tüm makineler içeremez. <br><br> Örnek: <br><ul><li> ["FrontEnd_IN_0", "BackEnd_IN_0"] |
| Olay durumu | Bu olay durumu. <br><br> Değerler: <ul><li>`Scheduled`: Bu olay, belirtilen süre geçtikten sonra başlatmak için zamanlanmış `NotBefore` özelliği.<li>`Started`: Bu olayı başlatıldı.</ul> Hayır `Completed` veya benzer durum hiç olmadığı kadar sağlanır; olay tamamlandığında, artık olay döndürülür.
| notBefore| Saat sonra bu olay başlayabilir. <br><br> Örnek: <br><ul><li> Pzt, 19 Eylül 2016 18:29:47 GMT  |

### <a name="event-scheduling"></a>Olay planlama
Her olay zamanlanmış bir minimum süre gelecekte olay türüne dayalı. Bu süre, bir olayın içinde yansıtılır `NotBefore` özelliği. 

|EventType  | En düşük bildirimi |
| - | - |
| Dondurma| 15 dakika |
| Yeniden başlatma | 15 dakika |
| Yeniden dağıtım | 10 dakika |
| Etkisiz hale | 30 saniye |

### <a name="event-scope"></a>Olay kapsamı     
Zamanlanmış olaylar teslim edilir:        
 - Bir bulut hizmetindeki tüm sanal makineler      
 - Bir kullanılabilirlik kümesindeki tüm sanal makineler      
 - Bir ölçek kümesi yerleştirme grubundaki tüm sanal makineler.         

Sonuç olarak, denetlemelisiniz `Resources` alanındaki Vm'leri etkilenmiş olacak tanımlamak için olay. 

### <a name="starting-an-event"></a>Olay başlatma 

Yaklaşan olay hakkında bilgi edindiniz ve mantığınızı kapatılmasını için tamamlanan sonra yaparak bekleyen olay onaylayabilirsiniz bir `POST` meta verileri hizmete çağrı `EventId`. Bu en düşük bildirim kısaltabilir Azure'a gösterir (uygun olduğunda) saat. 

Beklenen json verilmiştir `POST` istek gövdesi. İstek bir listesini içermelidir `StartRequests`. Her `StartRequest` içeren `EventId` hızlandırmak istediğiniz olayı için:
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
curl -H @{"Metadata"="true"} -Method POST -Body '{"StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' -Uri http://169.254.169.254/metadata/scheduledevents?api-version=2017-11-01
```

> [!NOTE] 
> Bir olay sıkan sağlar tüm devam etmek olay `Resources` yalnızca sanal makine, olay edilemeyeceğini durumunda. Bu nedenle ilk makine olarak basit bildirim koordine etmek için bir öncü seçmesi seçebilir `Resources` alan.


## <a name="powershell-sample"></a>PowerShell örneği 

Aşağıdaki örnek, zamanlanmış olaylar için meta veri hizmetini sorgular ve her bir bekleyen olay onaylar.

```powershell
# How to get scheduled events 
function Get-ScheduledEvents($uri)
{
    $scheduledEvents = Invoke-RestMethod -Headers @{"Metadata"="true"} -URI $uri -Method get
    $json = ConvertTo-Json $scheduledEvents
    Write-Host "Received following events: `n" $json
    return $scheduledEvents
}

# How to approve a scheduled event
function Approve-ScheduledEvent($eventId, $uri)
{
    # Create the Scheduled Events Approval Document
    $startRequests = [array]@{"EventId" = $eventId}
    $scheduledEventsApproval = @{"StartRequests" = $startRequests} 
    
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
$scheduledEventURI = 'http://{0}/metadata/scheduledevents?api-version=2017-11-01' -f $localHostIP 

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
        Approve-ScheduledEvent $event.EventId $scheduledEventURI 
    }
}
``` 

## <a name="next-steps"></a>Sonraki adımlar 

- İzleme bir [zamanlanmış olayları tanıtım](https://channel9.msdn.com/Shows/Azure-Friday/Using-Azure-Scheduled-Events-to-Prepare-for-VM-Maintenance) Azure Friday'de. 
- Zamanlanmış olaylar kod örnekleri gözden [Azure örnek meta veri zamanlanmış olayları GitHub deposu](https://github.com/Azure-Samples/virtual-machines-scheduled-events-discover-endpoint-for-non-vnet-vm)
- Kullanılabilir API'ler hakkında daha fazla bilgiyi [örnek meta veri hizmetine](instance-metadata-service.md).
- Hakkında bilgi edinin [azure'da Windows sanal makineleri için planlı Bakımı](planned-maintenance.md).
