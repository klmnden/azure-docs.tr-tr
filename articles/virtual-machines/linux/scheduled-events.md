---
title: Azure'da Linux VM'ler için zamanlanmış olayları | Microsoft Docs
description: Linux sanal makinelerinizi Azure meta veri hizmeti kullanarak olayları zamanlayın.
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: ''
author: ericrad
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2018
ms.author: ericrad
ms.openlocfilehash: 0831f08eaa3e8e6f6a0d3f68bc50cd927167b7ba
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65507920"
---
# <a name="azure-metadata-service-scheduled-events-for-linux-vms"></a>Azure meta veri hizmeti: Linux VM'ler için zamanlanmış olaylar

Zamanlanmış olaylar, sanal makine (VM) bakım için hazırlamak için uygulama süresi sunan Azure meta veri hizmetidir. Yaklaşan bakımın olaylar hakkında bilgi sağlar (örneğin, yeniden başlatma) böylece uygulamanız için hazırlama ve sınırlamak kesilme. PaaS ve Iaas hem Windows hem de Linux da dahil olmak üzere tüm Azure sanal makine türleri için kullanılabilir. 

Windows üzerinde zamanlanmış olaylar hakkında daha fazla bilgi için bkz. [zamanlanmış olayları için Windows Vm'leri](../windows/scheduled-events.md).

> [!Note] 
> Zamanlanmış olaylar tüm Azure bölgelerinde genel kullanıma sunuldu. Bkz: [sürümü ve bölge kullanılabilirliği](#version-and-region-availability) en son sürüm bilgileri için.

## <a name="why-use-scheduled-events"></a>Zamanlanmış olaylar neden kullanmalısınız?

Birçok uygulama, sanal makine bakım için hazırlanmanıza zamandan yararlı olabilir. Zaman kullanılabilirliği, güvenilirlik ve Bakım yapılabilirliğini, geliştirmek, uygulamaya özgü görevleri gerçekleştirmek için kullanılabilir dahil olmak üzere: 

- Denetim noktası ve geri yükleme.
- Bağlantı boşaltma.
- Birincil çoğaltma yük devretme.
- Bir yük dengeleyici havuzu kaldırma.
- Olay günlüğü.
- Normal şekilde kapatılmasını.

Zamanlanmış olaylar ile uygulamanız, bakım zaman ve ortaya etkisini sınırlamak için görevlerini tetikleyin bulabilir.  

Zamanlanmış olaylar, olayları aşağıdaki kullanım örnekleri sağlar:

- [Platform tarafından başlatılan Bakım](https://docs.microsoft.com/azure/virtual-machines/linux/maintenance-and-updates) (örneğin, VM yeniden başlatma, dinamik geçiş veya ana bilgisayar güncelleştirmeleri koruma bellek)
- Düzeyi düşürülmüş donanım
- Kullanıcı tarafından başlatılan Bakım (örneğin, bir kullanıcı yeniden başlatır veya bir sanal makine yeniden dağıtır)
- [Düşük öncelikli VM çıkarma](https://azure.microsoft.com/blog/low-priority-scale-sets) içinde ölçek kümeleri

## <a name="the-basics"></a>Temel bilgileri  

  Meta veri hizmeti VM içinden erişilebilir bir REST uç noktasını kullanarak Vm'leri çalıştırma hakkında bilgi gösterir. Böylece VM dışında gösterilmez, bilgileri bir nonroutable IP kullanılabilir.

### <a name="scope"></a>`Scope`
Zamanlanmış olaylar teslim edilir:

- Tek başına sanal makineler.
- Tüm VM'lerin bir bulut hizmetinde.
- Tüm VM'lerin bir kullanılabilirlik kümesinde.
- Tüm VM'lerin bir ölçek kümesi yerleştirme grubu. 

Sonuç olarak, kontrol `Resources` hangi sanal makinelerin etkilendiğini belirlemek için olay alan.

### <a name="endpoint-discovery"></a>Uç nokta bulma
VNET özellikli VM'ler için bir statik nonroutable IP'den meta veri hizmeti kullanılabilir `169.254.169.254`. Zamanlanmış olaylar en son sürümü için tam uç noktadır: 

 > `http://169.254.169.254/metadata/scheduledevents?api-version=2017-11-01`

Bir sanal ağdaki bulut Hizmetleri ve klasik VM'ler, varsayılan durumlarda VM oluşturulmamışsa ek mantık kullanılacak IP adresini bulmak için gereklidir. Bilgi edinmek için nasıl [konak son noktasını Bul](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm), bu örneğe bakın.

### <a name="version-and-region-availability"></a>Sürümü ve bölge kullanılabilirliği
Zamanlanmış olaylar olarak tutulan hizmetidir. Sürümleri zorunludur; geçerli sürümü `2017-11-01`.

| Version | Yayın türü | Bölgeler | Sürüm Notları | 
| - | - | - | - | 
| 2017-11-01 | Genel Erişilebilirlik | Tümü | <li> Düşük öncelikli VM çıkarma EventType 'Preempt' desteği eklendi<br> | 
| 2017-08-01 | Genel Erişilebilirlik | Tümü | <li> Iaas Vm'leri için kaynak adları alt çizgi başına kaldırıldı<br><li>Tüm istekler için zorlanan meta veri üst bilgisi gereksinimi | 
| 2017-03-01 | Preview | Tümü | <li>İlk yayın


> [!NOTE] 
> Önceki Önizleme sürümlerinde zamanlanmış api-version {son} desteklenen olaylar. Bu biçim, artık desteklenmemektedir ve gelecekte kullanım dışı bırakılacaktır.

### <a name="enabling-and-disabling-scheduled-events"></a>Zamanlanmış olaylar etkinleştirme ve devre dışı bırakma
Zamanlanmış olaylar, olaylar için bir istekte hizmeti ilk zaman için etkindir. İlk iki dakikaya kadar çağrınızda Gecikmeli yanıt beklemelisiniz.

Zamanlanmış olaylar 24 saat için bir istek yapmadığında hizmetiniz için dışıdır.

### <a name="user-initiated-maintenance"></a>Kullanıcı tarafından başlatılan bakım
Azure portalı, API, CLI veya PowerShell aracılığıyla kullanıcı tarafından başlatılan VM bakımını zamanlanmış bir olayı oluşur. Bakım hazırlık mantıksal uygulamanızı sınayabilir ve uygulamanız için kullanıcı tarafından başlatılan bakım hazırlayabilirsiniz.

Bir olay türüne sahip bir VM'yi yeniden başlatın, `Reboot` zamanlandı. Bir olay türüne sahip bir VM varsa yeniden `Redeploy` zamanlandı.

## <a name="use-the-api"></a>API kullanın

### <a name="headers"></a>Üst bilgiler
Meta veri hizmetini sorgulama, başlık sağlamalısınız `Metadata:true` istek değildi yanlışlıkla yeniden yönlendirilen emin olmak için. `Metadata:true` Üstbilgi tüm zamanlanmış olaylar istekler için gereklidir. Meta veri hizmetinden "Hatalı istek" yanıt üstbilgisini istekte hatası sonuçlanır.

### <a name="query-for-events"></a>Sorgu olayları
Zamanlanmış olaylar için aşağıdaki çağrıyı yaparak sorgulayabilirsiniz:

#### <a name="bash"></a>Bash
```
curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01
```

Yanıt, zamanlanmış olaylar dizisini içerir. Boş bir dizi, şu anda hiçbir olay zamanlandığını anlamına gelir.
Bu durumda Zamanlanmış olaylar olduğunda, bir dizi olay yanıtı içerir. 
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

### <a name="event-properties"></a>Olay Özellikleri
|Özellik  |  Açıklama |
| - | - |
| EventID | Bu olay için genel benzersiz tanımlayıcı. <br><br> Örnek: <br><ul><li>602d9444-d2cd-49c7-8624-8643e7171297  |
| EventType | Bu olaya neden olan etkisi. <br><br> Değerler: <br><ul><li> `Freeze`: Sanal makine, birkaç saniye için duraklatır şekilde zamanlanır. CPU ve ağ bağlantısı askıya alınabilir, ancak bellek veya açık dosyaları üzerinde etkisi yoktur.<li>`Reboot`: Sanal makine için yeniden başlatma zamanlanır (kalıcı olmayan bellek kaybolur). <li>`Redeploy`: Sanal makineyi başka bir düğüme taşımak üzere zamanlanmış (kısa ömürlü diskleri kaybolur). <li>`Preempt`: Düşük öncelikli sanal makine siliniyor (kısa ömürlü diskleri kaybolur).|
| KaynakTürü | Bu olayın etkileyen kaynak türü. <br><br> Değerler: <ul><li>`VirtualMachine`|
| Kaynaklar| Bu olayın etkileyen kaynakların listesi. Listenin en fazla bir makinelerden içeren garanti [güncelleme etki alanı](manage-availability.md), ancak tüm makinelerde UD içermeyebilir. <br><br> Örnek: <br><ul><li> ["FrontEnd_IN_0", "BackEnd_IN_0"] |
| EventStatus | Bu olay durumu. <br><br> Değerler: <ul><li>`Scheduled`: Bu olay, belirtilen süre geçtikten sonra başlatmak için zamanlanmış `NotBefore` özelliği.<li>`Started`: Bu olayı başlatıldı.</ul> Hayır `Completed` veya benzer durum hiç olmadığı kadar sağlanır. Olay, olay sona erdiğinde artık döndürülür.
| notBefore| Saat sonra bu olay başlayabilirsiniz. <br><br> Örnek: <br><ul><li> Pzt, 19 Eylül 2016 18:29:47 GMT  |

### <a name="event-scheduling"></a>Olay planlama
Her olay zamanlanmış bir minimum süre gelecekte olay türüne dayalı. Bu süre, bir olayın içinde yansıtılır `NotBefore` özelliği. 

|EventType  | En düşük bildirimi |
| - | - |
| Dondurma| 15 dakika |
| Yeniden başlatma | 15 dakika |
| Yeniden dağıtın | 10 dakika |
| Etkisiz hale | 30 saniye |

### <a name="start-an-event"></a>Bir olayı Başlat 

Yaklaşan bir etkinlik öğrenin ve mantığınızı kapatılmasını için son sonra yaparak bekleyen olay onaylayabilirsiniz bir `POST` ile meta veri hizmetine çağrı `EventId`. En düşük bildirim kısaltabilir Azure'a bu çağrıyı gösterir (uygun olduğunda) saat. 

Aşağıdaki JSON örneği bekleniyor `POST` istek gövdesi. İstek bir listesini içermelidir `StartRequests`. Her `StartRequest` içeren `EventId` hızlandırmak istediğiniz olayı için:
```
{
    "StartRequests" : [
        {
            "EventId": {EventId}
        }
    ]
}
```

#### <a name="bash-sample"></a>Örnek bash
```
curl -H Metadata:true -X POST -d '{"StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' http://169.254.169.254/metadata/scheduledevents?api-version=2017-11-01
```

> [!NOTE] 
> Bir olay sıkan sağlar tüm devam etmek olay `Resources` değil yalnızca sanal Makinenin, olay edilemeyeceğini durumunda. Bu nedenle, ilk makine olarak basit olabilir bildirim koordine etmek için bir öncü seçmesi seçebilirsiniz `Resources` alan.

## <a name="python-sample"></a>Python örneği 

Aşağıdaki örnek, zamanlanmış olaylar için meta veri hizmeti sorguları ve her bir bekleyen olay onaylar:

```python
#!/usr/bin/python

import json
import urllib2
import socket
import sys

metadata_url = "http://169.254.169.254/metadata/scheduledevents?api-version=2017-11-01"
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
            print "+ Scheduled Event. This host " + this_host + " is scheduled for " + eventtype + " not before " + notbefore
            # Add logic for handling events here


def main():
   data = get_scheduled_events()
   handle_scheduled_events(data)

if __name__ == '__main__':
  main()
  sys.exit(0)
```

## <a name="next-steps"></a>Sonraki adımlar 
- İzleme [zamanlanmış olaylar Azure Friday'de](https://channel9.msdn.com/Shows/Azure-Friday/Using-Azure-Scheduled-Events-to-Prepare-for-VM-Maintenance) tanıtımını görmek için. 
- Zamanlanmış olaylar kod örnekleri gözden [Azure örnek meta veri zamanlanmış olayları GitHub deposu](https://github.com/Azure-Samples/virtual-machines-scheduled-events-discover-endpoint-for-non-vnet-vm).
- Daha fazla kullanılabilir API'leri hakkında bilgi edinin [örnek meta veri hizmeti](instance-metadata-service.md).
- Hakkında bilgi edinin [azure'da Linux sanal makineleri için planlı Bakımı](planned-maintenance.md).
