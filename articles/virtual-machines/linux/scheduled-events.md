---
title: "Azure Linux VM'ler için olayları zamanlanmış | Microsoft Docs"
description: "Azure meta veri hizmeti için Linux sanal makineleri kullanarak olayları zamanlanan."
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/14/2017
ms.author: zivr
ms.openlocfilehash: e8e943db5a48f8fbbcd63a448abe34b66f5f987a
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="azure-metadata-service-scheduled-events-preview-for-linux-vms"></a>Azure meta veri hizmeti: Linux VM'ler zamanlanmış olaylar (Önizleme)

> [!NOTE] 
> Kullanım koşullarını kabul ediyorum koşuluyla önizlemeleri için kullanılabilir hale getirilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Microsoft Azure Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

Zamanlanmış olaylar hizmettir bir Azure meta verileri, sanal makine bakımı için hazırlamak için uygulama zaman verir. Yaklaşan Bakımı olaylar hakkında bilgi sağlar (örn. yeniden başlatma) uygulamanız için hazırlamak ve sınırlamak için kesintisi. PaaS ve hem Windows hem de Linux Iaas dahil olmak üzere tüm Azure sanal makine türleri kullanılabilir. 

Windows zamanlanan olaylar hakkında daha fazla bilgi için bkz: [zamanlanmış olaylar için Windows Vm'lerini](../windows/scheduled-events.md).

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

## <a name="the-basics"></a>Temel bilgiler  

Azure meta veri hizmeti REST uç noktasını VM içinden erişilebilen kullanarak sanal makineleri çalıştırma hakkında bilgi gösterir. Böylece dışında VM gösterilmeyen bilgileri yönlendirilemeyen bir IP kullanılabilir.

### <a name="scope"></a>Kapsam
Zamanlanmış olayları teslim edilir:
- Bir bulut hizmetindeki tüm sanal makineler
- Tüm sanal makinelerin bir kullanılabilirlik kümesi
- Bir ölçek kümesi yerleştirme grubundaki tüm sanal makineler. 

Sonuç olarak, denetlemelisiniz `Resources` hangi VM'ler etkilenir olacak tanımlamak için olay alanındaki.

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
- Hakkında bilgi edinin [planlı bakım için Linux sanal makineleri azure'da](planned-maintenance.md).
