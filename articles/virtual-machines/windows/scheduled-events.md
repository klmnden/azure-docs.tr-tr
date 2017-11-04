---
title: "Azure'da Windows sanal makineleri için olayları zamanlanmış | Microsoft Docs"
description: "Azure meta veri hizmeti için Windows sanal makinelerde kullanarak olayları zamanlanan."
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: 
author: zivraf
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
ms.author: zivr
ms.openlocfilehash: 76179b6a8eb7066c90828d33729b557f5e37c17a
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="azure-metadata-service-scheduled-events-preview-for-windows-vms"></a>Azure meta veri hizmeti: Windows VM'ler zamanlanmış olaylar (Önizleme)

> [!NOTE] 
> Kullanım koşullarını kabul ediyorum koşuluyla önizlemeleri için kullanılabilir hale getirilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Microsoft Azure Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

Zamanlanmış olaylar biridir alt Servisleri Azure meta veri hizmeti altında. Yaklaşan olayları ile ilgili bilgiler görünmesini sorumludur (örneğin, yeniden başlatma) uygulamanız için hazırlamak ve sınırlamak için kesintisi. PaaS ve Iaas dahil olmak üzere tüm Azure sanal makine türleri için kullanılabilir. Zamanlanmış olayları olay etkisini en aza indirmek için önleyici görevleri gerçekleştirmek için sanal makine zaman verir. 

Zamanlanmış olaylar hem Linux hem de Windows VM'ler için kullanılabilir. Linux üzerinde zamanlanmış olaylar hakkında bilgi için bkz: [Linux VM'ler için zamanlanmış olayları](../linux/scheduled-events.md).

## <a name="why-scheduled-events"></a>Neden zamanlanmış olayları?

Zamanlanmış olaylarla platform intiated Bakım veya hizmetinizi kullanıcı tarafından başlatılan Eylemler etkisini sınırlamak üzere adım atabilirsiniz. 

Durumunu korumak için çoğaltma tekniklerini kullanın, çok örnekli iş yükleri arasında birden çok örneği gerçekleştiği kesintileri etkilenebilir. Bu tür kayıpları, pahalı görevler (örneğin, yeniden dizinler) veya çoğaltma kaybına neden olabilir. 

Çoğu durumda, genel hizmet kullanılabilirliği geliştirilmiş bir kapama sırası gibi gerçekleştirerek (el ile yük devretme) kümedeki diğer Vm'lerle görevlere yeniden atama ya da sanal kaldırılması Tamamlanıyor (veya iptal etme) yürütülen işlemler, Ağ Yük Dengeleyici havuzuna makineden. 

Burada yöneticinin yaklaşan bir olay hakkında bilgilendirmek veya böyle bir olay günlüğü yardımcı bakım yapılabilirliğini bulutta barındırılan uygulamalar geliştirme durumlar vardır.

Azure meta veri hizmeti zamanlanmış olayları aşağıdaki kullanım örneklerini ortaya çıkarır:
-   Başlatılan platform Bakım (örneğin, ana bilgisayar işletim sistemi dağıtımı)
-   Kullanıcı tarafından başlatılan çağrıları (örneğin, kullanıcı yeniden başlatma veya yeniden dağıtır VM)


## <a name="the-basics"></a>Temel bilgiler  

Azure meta veri hizmeti REST uç noktasını VM içinden erişilebilen kullanarak sanal makineleri çalıştırma hakkında bilgi gösterir. Böylece dışında VM gösterilmeyen bilgileri yönlendirilemeyen bir IP kullanılabilir.

### <a name="scope"></a>Kapsam
Zamanlanmış olaylar, tüm sanal makineler bir bulut hizmetinde veya bir kullanılabilirlik kümesindeki tüm sanal makineleri çıkmış. Sonuç olarak, denetlemelisiniz `Resources` hangi VM'ler etkilenir olacak tanımlamak için olay alanındaki. 

### <a name="discovering-the-endpoint"></a>Uç nokta keşfetme
Bir sanal makine bir sanal ağ (VNet) içinde oluşturulduğu olduğu durumda meta veri hizmeti bir statik yönlendirilemeyen bir IP, kullanılabilir `169.254.169.254`.
Bulut Hizmetleri ve klasik sanal makineleri için varsayılan durumlar bir sanal ağ içinde sanal makine oluşturulmamışsa ek mantık kullanmak için uç nokta bulmak için gereklidir. Bilgi edinmek için bu örneğe bakın nasıl [konak uç noktası bulunamadı](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).

### <a name="versioning"></a>Sürüm oluşturma 
Örnek meta veri sürümü tutulan hizmetidir. Sürümleri zorunludur ve geçerli sürümü `2017-03-01`.

> [!NOTE] 
> Önceki Önizleme sürümleri {son} API sürümü desteklenen zamanlanmış olaylar. Bu biçim artık desteklenmemektedir ve gelecekte kullanım dışı kalacaktır.

### <a name="using-headers"></a>Üst bilgileri kullanma
Meta veri hizmeti sorguladığınızda başlık sağlamalısınız `Metadata: true` istek istemeden yönlendirilmeyen emin olmak için.

### <a name="enabling-scheduled-events"></a>Zamanlanmış olayları etkinleştirme
Zamanlanmış olaylar, istekte ilk kez Azure örtük olarak sanal makinenizde özelliği sağlar. Sonuç olarak, iki dakika içinde ilk çağrıda Gecikmeli yanıt beklemelisiniz.

### <a name="user-initiated-maintenance"></a>Kullanıcı tarafından başlatılan bakım
Sanal makine Bakımı Azure portalı üzerinden, API, CLI, kullanıcı tarafından başlatılan veya PowerShell zamanlanmış bir olayı sonuçlanır. Bu bakım hazırlık mantığı uygulamanıza test etmenizi sağlar ve kullanıcı tarafından başlatılan bakım için hazırlamak uygulamanızı sağlar.

Bir sanal makinenin yeniden başlatılması zamanlar türüne sahip bir olay `Reboot`. Bir sanal makineyi dağıtarak zamanlar türüne sahip bir olay `Redeploy`.

> [!NOTE] 
> Şu anda en fazla 10 kullanıcı tarafından başlatılan bakım işlemleri aynı anda zamanlanabilir. Bu sınır zamanlanmış olayları genel kullanılabilirlik önce rahat olmalıdır.

> [!NOTE] 
> Şu anda zamanlanmış olayları kaynaklanan kullanıcı tarafından başlatılan bakım yapılandırılabilir değildir. Yapılandırılabilirlik gelecekteki bir sürümde planlanmaktadır.

## <a name="using-the-api"></a>API'yi kullanma

### <a name="query-for-events"></a>Olaylar için sorgu
Aşağıdaki çağrıyı yaparak zamanlanmış olaylar için sorgulama yapabilirsiniz:

```
curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
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
| Olay Kimliği | Bu olay için genel benzersiz tanımlayıcı. <br><br> Örnek: <br><ul><li>602d9444-d2cd-49c7-8624-8643e7171297  |
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

### <a name="starting-an-event"></a>Bir olayı başlatılıyor 

Yaklaşan olay öğrenilen ve normal olarak kapatmak için mantığınızı tamamlandı sonra yaparak bekleyen olay onaylayabilirsiniz bir `POST` çağrısı ile meta veri hizmetine `EventId`. Bu, en düşük bildirim kısaltabilir Azure'a gösterir (uygunsa) süresi. 

```
curl -H Metadata:true -X POST -d '{"DocumentIncarnation":"5", "StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

> [!NOTE] 
> Bir olay aktarımının sağlayan tüm devam etmek olay `Resources` olay bildirir yalnızca sanal makine durumunda. Bu nedenle ilk makine olarak basit bildirim koordine etmek için bir kılavuz seçmediğiniz seçebileceği `Resources` alan.


## <a name="powershell-sample"></a>PowerShell örnek 

Aşağıdaki örnek, zamanlanmış olaylar için meta veri hizmeti sorgular ve her bekleyen olay onaylar.

```PowerShell
# How to get scheduled events 
function GetScheduledEvents($uri)
{
    $scheduledEvents = Invoke-RestMethod -Headers @{"Metadata"="true"} -URI $uri -Method get
    $json = ConvertTo-Json $scheduledEvents
    Write-Host "Received following events: `n" $json
    return $scheduledEvents
}

# How to approve a scheduled event
function ApproveScheduledEvent($eventId, $docIncarnation, $uri)
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

function HandleScheduledEvents($scheduledEvents)
{
    # Add logic for handling events here
}

######### Sample Scheduled Events Interaction #########

# Set up the scheduled events URI for a VNET-enabled VM
$localHostIP = "169.254.169.254"
$scheduledEventURI = 'http://{0}/metadata/scheduledevents?api-version=2017-03-01' -f $localHostIP 

# Get events
$scheduledEvents = GetScheduledEvents $scheduledEventURI

# Handle events however is best for your service
HandleScheduledEvents $scheduledEvents

# Approve events when ready (optional)
foreach($event in $scheduledEvents.Events)
{
    Write-Host "Current Event: `n" $event
    $entry = Read-Host "`nApprove event? Y/N"
    if($entry -eq "Y" -or $entry -eq "y")
    {
        ApproveScheduledEvent $event.EventId $scheduledEvents.DocumentIncarnation $scheduledEventURI 
    }
}
``` 


## <a name="c-sample"></a>C\# örnek 

Meta veri hizmeti ile iletişim kuracağını basit bir istemci, aşağıdaki örnek verilmiştir.

```csharp
public class ScheduledEventsClient
{
    private readonly string scheduledEventsEndpoint;
    private readonly string defaultIpAddress = "169.254.169.254"; 

    // Set up the scheduled events URI for a VNET-enabled VM
    public ScheduledEventsClient()
    {
        scheduledEventsEndpoint = string.Format("http://{0}/metadata/scheduledevents?api-version=2017-03-01", defaultIpAddress);
    }

    // Get events
    public string GetScheduledEvents()
    {
        Uri cloudControlUri = new Uri(scheduledEventsEndpoint);
        using (var webClient = new WebClient())
        {
            webClient.Headers.Add("Metadata", "true");
            return webClient.DownloadString(cloudControlUri);
        }   
    }

    // Approve events
    public void ApproveScheduledEvents(string jsonPost)
    {
        using (var webClient = new WebClient())
        {
            webClient.Headers.Add("Content-Type", "application/json");
            webClient.UploadString(scheduledEventsEndpoint, jsonPost);
        }
    }
}
```

Zamanlanmış olaylar aşağıdaki veri yapılarını kullanılarak temsil edilebilir:

```csharp
public class ScheduledEventsDocument
{
    public string DocumentIncarnation;
    public List<CloudControlEvent> Events { get; set; }
}

public class CloudControlEvent
{
    public string EventId { get; set; }
    public string EventStatus { get; set; }
    public string EventType { get; set; }
    public string ResourceType { get; set; }
    public List<string> Resources { get; set; }
    public DateTime? NotBefore { get; set; }
}

public class ScheduledEventsApproval
{
    public string DocumentIncarnation;
    public List<StartRequest> StartRequests = new List<StartRequest>();
}

public class StartRequest
{
    [JsonProperty("EventId")]
    private string eventId;

    public StartRequest(string eventId)
    {
        this.eventId = eventId;
    }
}
```

Aşağıdaki örnek, zamanlanmış olaylar için meta veri hizmeti sorgular ve her bekleyen olay onaylar.

```csharp
public class Program
{
    static ScheduledEventsClient client;

    static void Main(string[] args)
    {
        client = new ScheduledEventsClient();

        while (true)
        {
            string json = client.GetDocument();
            ScheduledEventsDocument scheduledEventsDocument = JsonConvert.DeserializeObject<ScheduledEventsDocument>(json);

            HandleEvents(scheduledEventsDocument.Events);

            // Wait for user response
            Console.WriteLine("Press Enter to approve executing events\n");
            Console.ReadLine();

            // Approve events
            ScheduledEventsApproval scheduledEventsApprovalDocument = new ScheduledEventsApproval()
            {
                DocumentIncarnation = scheduledEventsDocument.DocumentIncarnation
            };
        
            foreach (CloudControlEvent event in scheduledEventsDocument.Events)
            {
                scheduledEventsApprovalDocument.StartRequests.Add(new StartRequest(event.EventId));
            }

            if (scheduledEventsApprovalDocument.StartRequests.Count > 0)
            {
                // Serialize using Newtonsoft.Json
                string approveEventsJsonDocument =
                    JsonConvert.SerializeObject(scheduledEventsApprovalDocument);

                Console.WriteLine($"Approving events with json: {approveEventsJsonDocument}\n");
                client.ApproveScheduledEvents(approveEventsJsonDocument);
            }

            Console.WriteLine("Complete. Press enter to repeat\n\n");
            Console.ReadLine();
            Console.Clear();
        }
    }

    private static void HandleEvents(List<CloudControlEvent> events)
    {
        // Add logic for handling events here
    }
}
```

## <a name="python-sample"></a>Python örneği 

Aşağıdaki örnek, zamanlanmış olaylar için meta veri hizmeti sorgular ve her bekleyen olay onaylar.

```python
#!/usr/bin/python

import json
import urllib2
import socket
import sys

metadata_url = "http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01"
headers = "{Metadata:true}"
this_host = socket.gethostname()

def get_scheduled_events():
   req = urllib2.Request(metadata_url)
   req.add_header('Metadata', 'true')
   resp = urllib2.urlopen(req)
   data = json.loads(resp.read())
   return data

def handle_scheduled_events(data):
    for evt in data['Events']:
        eventid = evt['EventId']
        status = evt['EventStatus']
        resources = evt['Resources']
        eventtype = evt['EventType']
        resourcetype = evt['ResourceType']
        notbefore = evt['NotBefore'].replace(" ","_")
        if this_host in resources:
            print "+ Scheduled Event. This host is scheduled for " + eventype + " not before " + notbefore
            # Add logic for handling events here

def main():
   data = get_scheduled_events()
   handle_scheduled_events(data)
   
if __name__ == '__main__':
  main()
  sys.exit(0)
```

## <a name="next-steps"></a>Sonraki adımlar 

- Kullanılabilen API'leri hakkında daha fazla bilgiyi [örneği meta veri hizmeti](instance-metadata-service.md).
- Hakkında bilgi edinin [planlı bakım azure'da Windows sanal makineler için](planned-maintenance.md).

