---
title: Azure İzleyici (Önizleme) VM'ler için sorgu günlüklerini nasıl | Microsoft Docs
description: VM çözümü için Azure İzleyici ölçümleri toplar ve günlük verilerini ve bu makalede kayıtları açıklar ve örnek sorgular içerir.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/10/2019
ms.author: magoedte
ms.openlocfilehash: bca1b96e7dc5673cabef26fe6b2cfb8daa41fbf5
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64702516"
---
# <a name="how-to-query-logs-from-azure-monitor-for-vms-preview"></a>Azure İzleyici günlüklerinden VM'ler (Önizleme) için sorgulama
VM'ler için Azure İzleyici, performans ve bağlantı ölçümü, bilgisayar ve envanter verileri işlemek ve sistem durumu bilgilerini toplar ve Log Analytics çalışma alanına Azure İzleyici'de iletir.  Bu veriler için kullanılabilir [sorgu](../../azure-monitor/log-query/log-query-overview.md) Azure İzleyici'de. Geçiş planlaması kapasite analizi, bulma ve isteğe bağlı performans sorunlarını giderme senaryoları için bu verileri uygulayabilirsiniz.

## <a name="map-records"></a>Harita kayıtları
Her bir benzersiz bilgisayar ve işlem, bir işlem veya bilgisayar başlatıldığında veya Azure İzleyici için Vm'leri eşleme özelliğini eklendiğinden, oluşturulan kayıtlarına ek olarak saat başına tek bir kayıt oluşturulur. Bu kayıtları aşağıdaki tablolarda özelliklere sahiptir. Alanları ve değerleri ServiceMapComputer_CL olaylar ServiceMap Azure Resource Manager API'si makine kaynak alanları eşleyin. Alanları ve değerleri ServiceMapProcess_CL olaylar ServiceMap Azure Resource Manager API'si işlem kaynak alanlarını eşleyin. ResourceName_s alanın, karşılık gelen Resource Manager kaynak adı alanında eşleşir. 

Benzersiz işlemleri ve bilgisayarları tanımlamak için kullanabileceğiniz dahili olarak oluşturulan özellikler vardır:

- Bilgisayar: Kullanım *ResourceId* veya *ResourceName_s* Log Analytics çalışma alanındaki bir bilgisayarı benzersiz olarak tanımlanabilmesi için.
- İşlem: Kullanım *ResourceId* bir işlem bir Log Analytics çalışma alanı içinde benzersiz olarak tanımlanabilmesi için. *ResourceName_s* işlemi çalıştığı (MachineResourceName_s) makine bağlamında benzersizdir 

Sorgular, birden fazla kayıt aynı bilgisayarda veya işlem için belirtilen işlem ve belirtilen zaman aralığında bilgisayar için birden çok kayıt var olabileceğinden döndürebilir. Yalnızca en son kayıt eklemek için Ekle "| Yinelenenleri kaldırma ResourceId"sorgulanamıyor.

### <a name="connections-and-ports"></a>Bağlantılar ve bağlantı noktaları
Azure İzleyici günlüklerine - VMConnection ve VMBoundPort iki yeni tablolar bağlantı ölçümleri özelliği sunar. Bu tablolar üzerinde Aç/etkin bağlantı noktalarını sunucu yanı sıra, bir makine (gelen ve giden) için bağlantıları hakkında bilgi sağlar. Ayrıca, ConnectionMetrics zaman penceresi boyunca belirli bir ölçüyü elde bulunmasını sağlayan API'ler aracılığıyla kullanıma sunulur. TCP bağlantıları kaynaklanan *kabul* dinleme bir yuvada tarafından oluşturulanlar çalışırken gelen olan *bağlanma* giden olan, belirli bir IP ve bağlantı noktası. Bir bağlantı yönünü olarak ayarlanabilir Direction özelliği tarafından temsil edilen **gelen** veya **giden**. 

Bu tablolarındaki kayıtlara bağımlılık aracısı tarafından bildirilen verilerden oluşturulur. Tüm kayıtların gözlemi 1 dakikalık zaman aralığında temsil eder. TimeGenerated özelliği zaman aralığını başlangıcını gösterir. Her kayıt, diğer bir deyişle ilgili varlığı tanımlayan bilgiler, bağlantı veya bağlantı noktası yanı sıra söz konusu varlıkla ilişkili ölçümleri içerir. Şu anda, TCP IPv4 kullanarak oluşan ağ etkinliği bildirilir. 

#### <a name="common-fields-and-conventions"></a>Ortak alanları ve kuralları 
Aşağıdaki alanlar ve kuralları VMConnection hem VMBoundPort için geçerlidir: 

- Bilgisayar: Makine raporlama tam etki alanı adı 
- Agentıd: Log Analytics aracısını sahip bir makine için benzersiz tanımlayıcı  
- Makine: ServiceMap tarafından sunulan makine için Azure Resource Manager kaynak adı. Formu şöyledir *m-{GUID}* burada *GUID* Agentıd olarak aynı GUID  
- İşlem: ServiceMap tarafından kullanıma sunulan işlem için Azure Resource Manager kaynak adı. Formu şöyledir *p-{onaltılık dize}*. Eşsiz işlem kimliği makinelerde üretmek için makine ve işlem alanları birleştirmek ve makine kapsamında benzersiz işlemidir. 
- ProcessName: Raporlama işlemi yürütülebilir adı.
- Tüm IP adresleri IPv4 kurallı biçimde örneğin dizelerdir *13.107.3.160* 

Maliyetini ve karmaşıklığını yönetmek için tek bir fiziksel ağ bağlantıları bağlantısı kayıtlarını göstermez. Birden fazla fiziksel ağ bağlantıları, ardından ilgili tablodaki yansıtılır mantıksal bir bağlantı içinde gruplandırılır.  Yani kayıt içinde *VMConnection* mantıksal bir gruplandırmasını ve uyulması gereken değil ayrı ayrı fiziksel bağlantılar tablosunu temsil eder. Aşağıdaki öznitelikler için aynı değeri belirli bir dakikalık aralık sırasında paylaşımı fiziksel ağ bağlantısı tek bir mantıksal kayıt içine toplanan *VMConnection*. 

| Özellik | Description |
|:--|:--|
|Direction |Yön bağlantının değerdir *gelen* veya *giden* |
|Makine |FQDN bilgisayar |
|İşlem |İşlem ya da işlemleri, bağlantıyı başlatan/kabul grupları kimliği |
|Sourceıp |Kaynak IP adresi |
|DestinationIp |Hedef IP adresi |
|Trafficdirection |Hedef bağlantı noktası numarası |
|Protokol |Bağlantı için kullanılan protokol.  Değerler *tcp*. |

Gruplandırma etkisini için hesap için kaydın şu özelliklerde gruplanmış bir fiziksel bağlantı sayısı hakkında bilgi sağlanır:

| Özellik | Description |
|:--|:--|
|LinksEstablished |Raporlama zaman penceresi boyunca kurulmuş fiziksel ağ bağlantısı sayısı |
|LinksTerminated |Raporlama zaman penceresi boyunca sonlandırıldı fiziksel ağ bağlantısı sayısı |
|LinksFailed |Raporlama zaman penceresi sırasında başarısız olan fiziksel ağ bağlantılarının sayısı. Bu bilgiler, şu anda yalnızca giden bağlantıları için kullanılabilir. |
|LinksLive |Raporlama zaman penceresi sonunda açık olan fiziksel ağ bağlantısı sayısı|

#### <a name="metrics"></a>Ölçümler

Bağlantı sayısı ölçümü yanı sıra alınıp verilen bir mantıksal bağlantı üzerinde gönderilen veri hacmi hakkında bilgi veya ağ bağlantı noktası kaydın şu özelliklerde de dahil edilir:

| Özellik | Description |
|:--|:--|
|BytesSent |Raporlama zaman penceresi boyunca gönderilen bayt sayısı |
|BytesReceived |Raporlama zaman penceresi boyunca alınan bayt sayısı |
|Yanıtlar |Raporlama zaman penceresi boyunca gözlemlenen yanıtlarının sayısı. 
|ResponseTimeMax |Raporlama zaman penceresi boyunca gözlemlenen en büyük yanıt süresi (milisaniye). Değer, boş bir özelliktir.|
|ResponseTimeMin |Raporlama zaman penceresi boyunca gözlemlenen en küçük yanıt süresi (milisaniye). Değer, boş bir özelliktir.|
|ResponseTimeSum |Tüm yanıt süreleri toplamı gözlemlenen Raporlama zaman penceresi boyunca (milisaniye). Değer, boş bir özelliktir.|

Yanıt süresi üçüncü bildirilen veri türü, - ne kadar süreyle çağıran işlenmesi ve uzak uç tarafından yanıt için bir bağlantı üzerinden gönderilen bir istek için bekleniyor harcama. Bildirilen yanıt süresi, temel alınan uygulama protokolü doğru yanıt süresinin bir tahmindir. İlk olarak bir fiziksel ağ bağlantısının kaynak ve hedef sonu arasındaki veri akışını göre buluşsal yöntemler kullanılarak hesaplanır. Kavramsal olarak, bu son bayt bir isteği gönderen çıkışında ve yanıtın son baytını geri geldiğinde saat arasındaki farktır. Bu iki zaman damgaları, belirli bir fiziksel bağlantı istek ve yanıt olaylarına ayırmak için kullanılır. Aralarındaki fark, tek bir istek yanıt süresini temsil eder. 

Bu ilk sürümünde bu özellik, bizim başarı belirli ağ bağlantı için kullanılan gerçek uygulama protokolü bağlı olarak değişen dereceli çalışabilir bir yaklaştırma algoritmasıdır. Örneğin, gibi HTTP (S), istek-yanıt tabanlı protokoller için de geçerli yaklaşımın çalışır ancak tek yönlü çalışmak veya kuyruk tabanlı protokoller iletisi.

Dikkate alınması gereken bazı önemli noktalar şunlardır:

1. Her arabirim için ayrı bir kayıt işlemi aynı IP adresinde ancak birden çok ağ arabirimi üzerinden bağlantı kabul ederse bildirilir. 
2. Joker karakter IP kayıtlarla hiçbir etkinlik içerir. Bunlar, makinede bir bağlantı noktası açık gelen trafiği olduğunu göstermek için dahil edilir.
3. Eşleşen bir kayıt (için aynı işlem, bağlantı noktası ve protokol) olduğunda ayrıntı ve veri hacmini azaltmak için joker karakter IP kayıtlarla belirli bir IP adresi ile atlanacak. Bir joker karakter IP kaydı atlandığında, belirli bir IP adresi IsWildcardBind kayıt özelliğiyle ayarlanır "True" bağlantı noktası raporlama makinenin her bir arabirim üzerinden kullanıma sunulduğunu belirtir.
4. Yalnızca belirli arabirime bağlı bağlantı noktaları IsWildcardBind kümesine sahip *False*.

#### <a name="naming-and-classification"></a>Adlandırma ve sınıflandırma
Kolaylık olması için bir bağlantı uzak bitiş IP adresi RemoteIp özelliği eklenmiştir. Gelen bağlantılar için RemoteIp aynıdır Sourceıp giden bağlantılara karşın, bu DestinationIp aynıdır. RemoteDnsCanonicalNames özelliği RemoteIp için makine tarafından bildirilen DNS kurallı adlarını temsil eder. RemoteDnsQuestions ve RemoteClassification özellikler gelecekte kullanılmak üzere ayrılmıştır. 

#### <a name="geolocation"></a>Coğrafi Konum
*VMConnection* coğrafi konum bilgilerini ve uzak uç her bağlantı kaydın kaydın şu özelliklerde de içerir: 

| Özellik | Description |
|:--|:--|
|RemoteCountry |RemoteIp barındırma ülke adı.  Örneğin, *Amerika Birleşik Devletleri* |
|RemoteLatitude |Coğrafi konum enlem. Örneğin, *47.68* |
|RemoteLongitude |Coğrafi konum boylam. Örneğin, *-122.12* |

#### <a name="malicious-ip"></a>Kötü amaçlı IP
Her RemoteIp özelliğinde *VMConnection* tablo bilinen kötü amaçlı etkinliği ile bir dizi IP'ler karşı denetlenir. Aşağıdaki özellikler RemoteIp kötü amaçlı olarak tanımlanması durumunda doldurulur (IP kötü amaçlı olarak kabul edilmez, boş oldukları) kaydın aşağıdaki özellikleri:

| Özellik | Description |
|:--|:--|
|MaliciousIp |Uzak IP adresi |
|IndicatorThreadType |Algılanan tehdit göstergesidir şu değerlerden birini *Botnet*, *C2*, *CryptoMining*, *Darknet*, *DDos* , *MaliciousUrl*, *kötü amaçlı yazılım*, *kimlik avı*, *Proxy*, *PUA*, *İzleme*.   |
|Description |Gözlemlenen tehdit açıklaması. |
|TLPLevel |Trafik ışığı Protokolü (TLP) düzeyi tanımlanmış değerlerden biridir *beyaz*, *yeşil*, *Amber*, *kırmızı*. |
|Confidence |Değerler *0-100*. |
|Severity |Değerler *0 – 5*burada *5* en ciddi ve *0* hiç önemli değil. Varsayılan değer *3*.  |
|FirstReportedDateTime |İlk kez sağlayıcısı göstergesi bildirdi. |
|LastReportedDateTime |Son zaman göstergesi tarafından Interflow görüldü. |
|Isactive |Göstergeleri ile devre dışı gösteren *True* veya *False* değeri. |
|ReportReferenceLink |Belirli bir observable için ilgili raporları bağlar. |
|AdditionalInformation |Uygunsa, gözlemlenen tehdit hakkında ek bilgi sağlar. |

### <a name="ports"></a>Bağlantı Noktaları 
Etkin olarak gelen trafiği kabul veya potansiyel olarak trafiği kabul edebilecek, ancak Raporlama zaman penceresi boyunca boşta bağlantı noktaları bir makinede VMBoundPort tabloya yazılır.  

>[!NOTE]
>VM'ler için Azure İzleyici, toplama ve bağlantı noktası verileri aşağıdaki bölgelerde bir Log Analytics çalışma alanında kayıt desteklemez:  
>- Doğu ABD  
>- Batı Avrupa
>
> Bu verilerin toplanması etkin diğerinde [desteklenen bölgeler](vminsights-onboard.md#log-analytics) VM'ler için Azure İzleyici için. 

Her kayıtta VMBoundPort aşağıdaki alanlara göre tanımlanır: 

| Özellik | Description |
|:--|:--|
|Process | Bağlantı noktası ile ilişkili olduğu işlem (veya gruplar işlemlerin) kimliği.|
|IP | Bağlantı noktası, IP adresi (joker karakter IP olabilir *0.0.0.0*) |
|Bağlantı noktası |Bağlantı noktası numarası |
|Protokol | Protokol.  Örneğin, *tcp* veya *udp* (yalnızca *tcp* desteklenmektedir).|
 
Kimlik bir bağlantı noktası yukarıdaki beş alanları türetilir ve Portıd özelliğinde depolanıyor. Bu özellik, kayıtları için belirli bir bağlantı noktası zaman hızlı bir şekilde bulmak için kullanılabilir. 

#### <a name="metrics"></a>Ölçümler 
Bağlantı noktası kayıtları temsil eden ilişkili bağlantılar ölçümlerini dahil et. Şu anda aşağıdaki ölçümleri raporlanır (her ölçüm ayrıntılarını önceki bölümde açıklanmıştır): 

- BytesSent ve BytesReceived 
- LinksEstablished, LinksTerminated, LinksLive 
- ResposeTime, ResponseTimeMin, ResponseTimeMax, ResponseTimeSum 

Dikkate alınması gereken bazı önemli noktalar şunlardır:

- Her arabirim için ayrı bir kayıt işlemi aynı IP adresinde ancak birden çok ağ arabirimi üzerinden bağlantı kabul ederse bildirilir.  
- Joker karakter IP kayıtlarla hiçbir etkinlik içerir. Bunlar, makinede bir bağlantı noktası açık gelen trafiği olduğunu göstermek için dahil edilir. 
- Eşleşen bir kayıt (için aynı işlem, bağlantı noktası ve protokol) olduğunda ayrıntı ve veri hacmini azaltmak için joker karakter IP kayıtlarla belirli bir IP adresi ile atlanacak. Joker karakter IP kaydını atlandığında *IsWildcardBind* kaydın belirli bir IP adresine sahip özellik ayarlanacak *True*.  Bu bağlantı noktası raporlama makinenin her bir arabirim üzerinden Internet'e gösterir. 
- Yalnızca belirli arabirime bağlı bağlantı noktaları IsWildcardBind kümesine sahip *False*. 

### <a name="servicemapcomputercl-records"></a>ServiceMapComputer_CL kayıtları
Kayıt türü ile *ServiceMapComputer_CL* Envanter verileri için bağımlılık Aracısı'nı sunucularıyla sahip. Bu kayıtlar aşağıdaki tabloda özelliklere sahiptir:

| Özellik | Description |
|:--|:--|
| Tür | *ServiceMapComputer_CL* |
| SourceSystem | *OpsManager* |
| ResourceId | Çalışma alanı içindeki bir makine için benzersiz tanımlayıcı |
| ResourceName_s | Çalışma alanı içindeki bir makine için benzersiz tanımlayıcı |
| ComputerName_s | FQDN bilgisayar |
| Ipv4Addresses_s | Bir listesi sunucunun IPv4 adresleri |
| Ipv6Addresses_s | Bir listesini sunucu IPv6 adresleri |
| DnsNames_s | Bir dizi DNS adları |
| OperatingSystemFamily_s | Windows veya Linux |
| OperatingSystemFullName_s | İşletim sisteminin tam adı  |
| Bitness_s | Bit genişliği makinenin (32 bit veya 64-bit)  |
| PhysicalMemory_d | MB fiziksel bellek |
| Cpus_d | CPU sayısı |
| CpuSpeed_d | CPU hızı MHz|
| VirtualizationState_s | *Bilinmeyen*, *fiziksel*, *sanal*, *hiper yönetici* |
| VirtualMachineType_s | *hyperv*, *vmware*, vb. |
| VirtualMachineNativeMachineId_g | VM, hiper yönetici tarafından atanan kimliği |
| VirtualMachineName_s | VM adı |
| BootTime_t | Önyükleme zamanı |

### <a name="servicemapprocesscl-type-records"></a>ServiceMapProcess_CL türü kayıtları
Kayıt türü ile *ServiceMapProcess_CL* Envanter verileri TCP bağlantılı işlemleri için bağımlılık Aracısı'nı sunucularıyla vardır. Bu kayıtlar aşağıdaki tabloda özelliklere sahiptir:

| Özellik | Description |
|:--|:--|
| Tür | *ServiceMapProcess_CL* |
| SourceSystem | *OpsManager* |
| ResourceId | Çalışma alanı içinde bir işlem için benzersiz tanımlayıcı |
| ResourceName_s | Üzerinde çalıştığı makinenin içinde bir işlem için benzersiz tanımlayıcı|
| MachineResourceName_s | Makinenin kaynak adı |
| ExecutableName_s | İşlem yürütülebilir dosyası adı |
| StartTime_t | İşlem havuzu başlangıç saati |
| FirstPid_d | İşlem havuzunda ilk PID |
| Description_s | İşlem açıklaması |
| CompanyName_s | Şirketin adı |
| InternalName_s | İç adı |
| ProductName_s | Ürün adı |
| ProductVersion_s | Ürün sürümü |
| FileVersion_s | Dosya sürümü |
| CommandLine_s | Komut satırı |
| ExecutablePath_s | Yürütülebilir dosya yolu |
| WorkingDirectory_s | Çalışma dizini |
| UserName | Hesabın altında işlemi yürütülüyor |
| USERDOMAIN | Etki alanı altında işlemi yürütülüyor |

## <a name="sample-log-searches"></a>Örnek günlük aramaları

### <a name="list-all-known-machines"></a>Bilinen tüm makinelerin listesi
```kusto
ServiceMapComputer_CL | summarize arg_max(TimeGenerated, *) by ResourceId`
```

### <a name="when-was-the-vm-last-rebooted"></a>Ne zaman VM son yeniden başlatıldı
```kusto
let Today = now(); ServiceMapComputer_CL | extend DaysSinceBoot = Today - BootTime_t | summarize by Computer, DaysSinceBoot, BootTime_t | sort by BootTime_t asc`
```

### <a name="summary-of-azure-vms-by-image-location-and-sku"></a>Azure Vm'leri bir özetini resim, konum ve SKU
```kusto
ServiceMapComputer_CL | where AzureLocation_s != "" | summarize by ComputerName_s, AzureImageOffering_s, AzureLocation_s, AzureImageSku_s`
```

### <a name="list-the-physical-memory-capacity-of-all-managed-computers"></a>Tüm yönetilen bilgisayarların fiziksel bellek kapasitesi listeleyin.
```kusto
ServiceMapComputer_CL | summarize arg_max(TimeGenerated, *) by ResourceId | project PhysicalMemory_d, ComputerName_s`
```

### <a name="list-computer-name-dns-ip-and-os"></a>Liste bilgisayar adı, DNS, IP ve işletim sistemi.
```kusto
ServiceMapComputer_CL | summarize arg_max(TimeGenerated, *) by ResourceId | project ComputerName_s, OperatingSystemFullName_s, DnsNames_s, Ipv4Addresses_s`
```

### <a name="find-all-processes-with-sql-in-the-command-line"></a>Komut satırında "sql" ile tüm işlemler bulun
```kusto
ServiceMapProcess_CL | where CommandLine_s contains_cs "sql" | summarize arg_max(TimeGenerated, *) by ResourceId`
```

### <a name="find-a-machine-most-recent-record-by-resource-name"></a>Bir makine (en son kayıt) kaynak adına göre bulma
```kusto
search in (ServiceMapComputer_CL) "m-4b9c93f9-bc37-46df-b43c-899ba829e07b" | summarize arg_max(TimeGenerated, *) by ResourceId`
```

### <a name="find-a-machine-most-recent-record-by-ip-address"></a>(En son kayıt) bir makine IP adresine göre Bul
```kusto
search in (ServiceMapComputer_CL) "10.229.243.232" | summarize arg_max(TimeGenerated, *) by ResourceId`
```

### <a name="list-all-known-processes-on-a-specified-machine"></a>Belirtilen bir makinedeki tüm bilinen işlemlere listesi
```kusto
ServiceMapProcess_CL | where MachineResourceName_s == "m-559dbcd8-3130-454d-8d1d-f624e57961bc" | summarize arg_max(TimeGenerated, *) by ResourceId`
```

### <a name="list-all-computers-running-sql-server"></a>SQL Server çalıştıran tüm bilgisayarları listeleyin
```kusto
ServiceMapComputer_CL | where ResourceName_s in ((search in (ServiceMapProcess_CL) "\*sql\*" | distinct MachineResourceName_s)) | distinct ComputerName_s`
```

### <a name="list-all-unique-product-versions-of-curl-in-my-datacenter"></a>Curl my veri merkezinde tüm benzersiz ürün sürümlerini listeleme
```kusto
ServiceMapProcess_CL | where ExecutableName_s == "curl" | distinct ProductVersion_s`
```

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>CentOS çalıştıran tüm bilgisayarların bir bilgisayar grubu oluşturun
```kusto
ServiceMapComputer_CL | where OperatingSystemFullName_s contains_cs "CentOS" | distinct ComputerName_s`
```

### <a name="bytes-sent-and-received-trends"></a>Bayt gönderilen ve alınan eğilimleri
```kusto
VMConnection | summarize sum(BytesSent), sum(BytesReceived) by bin(TimeGenerated,1hr), Computer | order by Computer desc | render timechart`
```

### <a name="which-azure-vms-are-transmitting-the-most-bytes"></a>Azure Vm'lerinin en fazla bayt iletmesini durdurabilirsiniz
```kusto
VMConnection | join kind=fullouter(ServiceMapComputer_CL) on $left.Computer == $right.ComputerName_s | summarize count(BytesSent) by Computer, AzureVMSize_s | sort by count_BytesSent desc`
```

### <a name="link-status-trends"></a>Bağlantı durumu eğilimleri
```kusto
VMConnection | where TimeGenerated >= ago(24hr) | where Computer == "acme-demo" | summarize  dcount(LinksEstablished), dcount(LinksLive), dcount(LinksFailed), dcount(LinksTerminated) by bin(TimeGenerated, 1h) | render timechart`
```

### <a name="connection-failures-trend"></a>Bağlantı hataları eğilimi
```kusto
VMConnection | where Computer == "acme-demo" | extend bythehour = datetime_part("hour", TimeGenerated) | project bythehour, LinksFailed | summarize failCount = count() by bythehour | sort by bythehour asc | render timechart`
```

### <a name="bound-ports"></a>Bağlı bağlantı noktaları
```kusto
VMBoundPort
| where TimeGenerated >= ago(24hr)
| where Computer == 'admdemo-appsvr'
| distinct Port, ProcessName
```

### <a name="number-of-open-ports-across-machines"></a>Makineler arasında bağlantı noktalarını açma sayısı
```kusto
VMBoundPort
| where Ip != "127.0.0.1"
| summarize by Computer, Machine, Port, Protocol
| summarize OpenPorts=count() by Computer, Machine
| order by OpenPorts desc
```

### <a name="score-processes-in-your-workspace-by-the-number-of-ports-they-have-open"></a>Çalışma alanınızdaki işlemleri sahip oldukları bağlantı noktalarının sayısı tarafından açık Puanlama
```kusto
VMBoundPort
| where Ip != "127.0.0.1"
| summarize by ProcessName, Port, Protocol
| summarize OpenPorts=count() by ProcessName
| order by OpenPorts desc
```

### <a name="aggregate-behavior-for-each-port"></a>Her bağlantı noktası için toplama davranışı
Bu sorgu daha sonra bağlantı noktaları puanlamak için etkinlik, örneğin, en fazla gelen/giden trafiğe sahip bağlantı noktalarını, çoğu bağlantısı ile bağlantı noktalarına tarafından kullanılabilir
```kusto
// 
VMBoundPort
| where Ip != "127.0.0.1"
| summarize BytesSent=sum(BytesSent), BytesReceived=sum(BytesReceived), LinksEstablished=sum(LinksEstablished), LinksTerminated=sum(LinksTerminated), arg_max(TimeGenerated, LinksLive) by Machine, Computer, ProcessName, Ip, Port, IsWildcardBind
| project-away TimeGenerated
| order by Machine, Computer, Port, Ip, ProcessName
```

### <a name="summarize-the-outbound-connections-from-a-group-of-machines"></a>Bir gruptaki makinelerin giden bağlantılar özetleme
```kusto
// the machines of interest
let machines = datatable(m: string) ["m-82412a7a-6a32-45a9-a8d6-538354224a25"];
// map of ip to monitored machine in the environment
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
// all connections to/from the machines of interest
let out=materialize(VMConnection
| where Machine in (machines)
| summarize arg_max(TimeGenerated, *) by ConnectionId);
// connections to localhost augmented with RemoteMachine
let local=out
| where RemoteIp startswith "127."
| project ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine=Machine;
// connections not to localhost augmented with RemoteMachine
let remote=materialize(out
| where RemoteIp !startswith "127."
| join kind=leftouter (ips) on $left.RemoteIp == $right.ips
| summarize by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine=MonitoredMachine);
// the remote machines to/from which we have connections
let remoteMachines = remote | summarize by RemoteMachine;
// all augmented connections
(local)
| union (remote)
//Take all outbound records but only inbound records that come from either //unmonitored machines or monitored machines not in the set for which we are computing dependencies.
| where Direction == 'outbound' or (Direction == 'inbound' and RemoteMachine !in (machines))
| summarize by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine
// identify the remote port
| extend RemotePort=iff(Direction == 'outbound', DestinationPort, 0)
// construct the join key we'll use to find a matching port
| extend JoinKey=strcat_delim(':', RemoteMachine, RemoteIp, RemotePort, Protocol)
// find a matching port
| join kind=leftouter (VMBoundPort 
| where Machine in (remoteMachines) 
| summarize arg_max(TimeGenerated, *) by PortId 
| extend JoinKey=strcat_delim(':', Machine, Ip, Port, Protocol)) on JoinKey
// aggregate the remote information
| summarize Remote=makeset(iff(isempty(RemoteMachine), todynamic('{}'), pack('Machine', RemoteMachine, 'Process', Process1, 'ProcessName', ProcessName1))) by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol
```

## <a name="next-steps"></a>Sonraki adımlar
* Azure İzleyici'de günlük sorgu yazmaya yeni başladıysanız, gözden [Log Analytics'i kullanmak nasıl](../../azure-monitor/log-query/get-started-portal.md) Azure portalında günlük sorguları yazma için.
* Hakkında bilgi edinin [arama sorguları yazma](../../azure-monitor/log-query/search-queries.md).
