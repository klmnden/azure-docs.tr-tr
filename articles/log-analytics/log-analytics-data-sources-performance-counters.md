---
title: "Toplama ve performans sayaçları Azure günlük analizi çözümleme | Microsoft Docs"
description: "Performans sayaçları, Windows ve Linux aracılarını performansını çözümlemek için günlük analizi tarafından toplanır.  Bu makalede, performans sayaçlarını toplama hem Windows için yapılandırmak açıklar ve Linux aracılarını, bunlar ayrıntılarını OMS depo ve OMS portalında analiz etme depolanır."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 20e145e4-2ace-4cd9-b252-71fb4f94099e
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2017
ms.author: magoedte
ms.openlocfilehash: 953bb453b0a9635627fbbb6c3913d0cd757101c7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="windows-and-linux-performance-data-sources-in-log-analytics"></a>Windows ve Linux performans veri kaynaklarında günlük analizi
Windows ve Linux performans sayaçları donanım bileşenleri, işletim sistemleri ve uygulamaların performansını bir anlayış sağlar.  Günlük analizi, performans sayaçları, uzun vadeli analiz için performans verilerini toplama ve raporlama ek olarak neredeyse gerçek zamanlı (NRT) çözümleme için sık aralıklarla toplayabilirsiniz.

![Performans sayaçları](media/log-analytics-data-sources-performance-counters/overview.png)

## <a name="configuring-performance-counters"></a>Performans sayaçları yapılandırma
OMS Portalı'nda performans sayaçları yapılandırmak [günlük analizi ayarları veri menüde](log-analytics-data-sources.md#configuring-data-sources).

Windows veya Linux performans sayaçları yeni bir OMS çalışma alanı için ilk yapılandırırken hızla birçok ortak sayaçları oluşturma seçeneği verilir.  Bunlar, her yanındaki onay kutusunu ile listelenir.  Başlangıçta oluşturmak istediğiniz herhangi bir sayaç denetlenir ve ardından olun **Seçili performans sayaçlarını Ekle**.

Windows performans sayaçları için her performans sayacı için belirli bir örneği seçebilirsiniz. Linux performans sayaçları için seçtiğiniz her sayaç örneği üst sayacı tüm alt sayaçları geçerlidir. Aşağıdaki tabloda, Linux ve Windows performans sayaçları için kullanılabilen ortak örnekleri gösterilmektedir.

| Örnek adı | Açıklama |
| --- | --- |
| \_Toplam |Tüm örnekleri toplamı |
| \* |Tüm örnekleri |
| (/ &#124; / var) |Eşleşen adlandırılmış örnekleri: / veya /var |

### <a name="windows-performance-counters"></a>Windows performans sayaçları

![Windows performans sayaçlarını yapılandırın](media/log-analytics-data-sources-performance-counters/configure-windows.png)

Toplamak için yeni bir Windows performans sayacı eklemek için bu yordamı izleyin.

1. Sayaç adı metin kutusuna biçimde yazın *nesne (örnek) \counter*.  Yazmaya başladığınızda, ortak sayaçların eşleşen bir listesi sunulur.  Bir sayacı, liste veya kendi yazın ya da seçebilirsiniz.  Belirterek belirli bir sayaç için tüm örnekleri döndürebilir *object\counter*.  

    SQL Server performans sayaçları adlandırılmış örneklerin toplarken, tüm örnek sayaçları Başlarken adlı *MSSQL$* ve ardından örneğinin adı.  Örneğin, tüm veritabanları için adlandırılmış SQL veritabanı performans nesnesinden INST2 örneği için günlük önbelleği isabet oranı sayacı toplamak için belirtin `MSSQL$INST2:Databases(*)\Log Cache Hit Ratio`.

2. Tıklatın  **+**  veya basın **Enter** sayaç listesine eklemek için.
3. Bir sayaç eklediğinizde, varsayılan değer 10 saniye olan kullanan kendi **örnekleme aralığı**.  Toplanan performans veri depolama gereksinimlerini azaltmak istiyorsanız bu 1800 saniye (30 dakika) daha yüksek bir değere değiştirebilirsiniz.
4. Ekleme sayaçları bittiğinde, tıklatın **kaydetmek** yapılandırmayı kaydetmek için ekranın üstündeki düğmesi.

### <a name="linux-performance-counters"></a>Linux performans sayaçları

![Linux performans sayaçları yapılandırmak](media/log-analytics-data-sources-performance-counters/configure-linux.png)

Toplamak için yeni bir Linux performans sayacı eklemek için bu yordamı izleyin.

1. Varsayılan olarak, tüm yapılandırma değişiklikleri otomatik olarak tüm aracıları için gönderilir.  Linux aracıları için bir yapılandırma dosyası için Fluentd veri toplayıcı gönderilir.  Bu dosyayı her Linux aracısında el ile değiştirmek isterseniz, kutunun işaretini *aşağıdaki yapılandırmayı Linux makinelerime Uygula* ve aşağıdaki yönergeleri izleyin.
2. Sayaç adı metin kutusuna biçimde yazın *nesne (örnek) \counter*.  Yazmaya başladığınızda, ortak sayaçların eşleşen bir listesi sunulur.  Bir sayacı, liste veya kendi yazın ya da seçebilirsiniz.  
3. Tıklatın  **+**  veya basın **Enter** nesne için diğer sayaçlarının listesi sayaç eklemek için.
4. Bir nesne için tüm sayaçlar aynı kullanmak **örnekleme aralığı**.  Varsayılan değer 10 saniyedir.  Toplanan performans veri depolama gereksinimlerini azaltmak istiyorsanız bu 1800 saniye (30 dakika) daha yüksek bir değerle değiştirin.
5. Ekleme sayaçları bittiğinde, tıklatın **kaydetmek** yapılandırmayı kaydetmek için ekranın üstündeki düğmesi.

#### <a name="configure-linux-performance-counters-in-configuration-file"></a>Linux performans sayaçları yapılandırma dosyasındaki yapılandırma
OMS Portalı'nı kullanarak Linux performans sayaçları yapılandırmak yerine, Linux Aracısı'nı yapılandırma dosyalarını düzenleme seçeneğiniz vardır.  Performans ölçümleri toplamak için yapılandırmada tarafından denetlenen **/etc/opt/microsoft/omsagent/\<çalışma alanı kimliği\>/conf/omsagent.conf**.

Tek bir yapılandırma dosyasındaki her nesne veya toplamak için performans ölçümleri kategorisi tanımlanmalıdır `<source>` öğesi. Sözdizimi deseni izler.

    <source>
      type oms_omi  
      object_name "Processor"
      instance_regex ".*"
      counter_name_regex ".*"
      interval 30s
    </source>


Bu öğe parametrelerinde aşağıdaki tabloda açıklanmıştır.

| Parametreler | Açıklama |
|:--|:--|
| Nesne\_adı | Koleksiyon için nesne adı. |
| örnek\_regex |  A *normal ifade* toplamak için hangi örnekleri tanımlama. Değer: `.*` tüm örneklerini belirtir. Yalnızca işlemci ölçümleri toplamak için \_toplam örneğini belirtmek `_Total`. Yalnızca crond veya sshd örnekleri için işlem ölçümlerini toplamak için belirtebilirsiniz: ' (crond\|sshd)'. |
| Sayaç\_adı\_regex | A *normal ifade* hangi toplamak için (nesne için) sayaçları tanımlama. Nesne için tüm sayaçlar toplanacak belirtin: `.*`. Yalnızca takas alanı sayaçları için bellek nesnesi toplamak için örneğin, size belirtebilirsiniz:`.+Swap.+` |
| aralığı | Toplanan ve nesnenin sayaçları sıklığı. |


Aşağıdaki tabloda, nesneleri ve yapılandırma dosyasında belirttiğiniz sayaçları listeler.  Ek sayaçları belirli uygulamalar için kullanılabilir olduğundan açıklandığı gibi [toplama günlük analizi Linux uygulamaları için performans sayaçları](log-analytics-data-sources-linux-applications.md).

| Nesne adı | Sayaç adı |
|:--|:--|
| Mantıksal Disk | % Boş Inode'lar |
| Mantıksal Disk | % Boş alan |
| Mantıksal Disk | % Kullanılan Inode'lar |
| Mantıksal Disk | % Kullanılan alan |
| Mantıksal Disk | Disk okuma bayt/sn |
| Mantıksal Disk | Disk Okuma/sn |
| Mantıksal Disk | Disk aktarımı/sn |
| Mantıksal Disk | Disk Yazma Bayt/sn |
| Mantıksal Disk | Disk Yazma/sn |
| Mantıksal Disk | Boş megabayt |
| Mantıksal Disk | Mantıksal Disk Bayt/sn |
| Bellek | % Kullanılabilir bellek |
| Bellek | % Kullanılabilir takas alanı |
| Bellek | % Kullanılan bellek |
| Bellek | % Kullanılan takas alanı |
| Bellek | Kullanılabilir MBayt belleği |
| Bellek | Kullanılabilir MBayt değiştirme |
| Bellek | Sayfa Okuma/sn |
| Bellek | Sayfa Yazma/sn |
| Bellek | Sayfa/sn |
| Bellek | Kullanılan MBayt takas alanı |
| Bellek | Kullanılan bellek MBayt |
| Ağ | Aktarılan toplam bayt sayısı |
| Ağ | Alınan toplam bayt sayısı |
| Ağ | Toplam bayt sayısı |
| Ağ | Aktarılan toplam paket sayısı |
| Ağ | Alınan toplam paket sayısı |
| Ağ | Toplam Rx hataları |
| Ağ | Toplam Tx hataları |
| Ağ | Toplam çakışmaları |
| Fiziksel Disk | Ort. Disk sn/Okuma |
| Fiziksel Disk | Ort. Disk sn/Aktarım |
| Fiziksel Disk | Ort. Disk sn/yazma |
| Fiziksel Disk | Fiziksel Disk Bayt/sn |
| İşlem | PCT ayrıcalıklı zamanı |
| İşlem | PCT kullanıcı zamanı |
| İşlem | Kullanılan bellek KB |
| İşlem | Paylaşılan sanal bellek |
| İşlemci | % DPC Zamanı |
| İşlemci | % Boş zaman |
| İşlemci | % Kesme Zamanı |
| İşlemci | % GÇ bekleme zamanı |
| İşlemci | % İyi zaman |
| İşlemci | % Ayrıcalıklı Zaman |
| İşlemci | % İşlemci zamanı |
| İşlemci | % Kullanıcı Zamanı |
| Sistem | Boş fiziksel bellek |
| Sistem | Disk belleği dosyasındaki boş alan |
| Sistem | Boş sanal bellek |
| Sistem | İşlemler |
| Sistem | Disk belleği dosyalarında depolanan boyut |
| Sistem | Açık kalma süresi |
| Sistem | Kullanıcılar |


Aşağıdaki performans ölçümleri için varsayılan yapılandırmadır.

    <source>
      type oms_omi
      object_name "Physical Disk"
      instance_regex ".*"
      counter_name_regex ".*"
      interval 5m
    </source>

    <source>
      type oms_omi
      object_name "Logical Disk"
      instance_regex ".*
      counter_name_regex ".*"
      interval 5m
    </source>

    <source>
      type oms_omi
      object_name "Processor"
      instance_regex ".*
      counter_name_regex ".*"
      interval 30s
    </source>

    <source>
      type oms_omi
      object_name "Memory"
      instance_regex ".*"
      counter_name_regex ".*"
      interval 30s
    </source>

## <a name="data-collection"></a>Veri toplama
Günlük analizi sayaç yüklü olan tüm aracıları, belirtilen örnek aralıkta tüm belirtilen performans sayaçlarını toplar.  Verileri değil toplanır ve ham verileri, OMS aboneliğiniz ile belirtilen süre için tüm günlük arama görünümlerde kullanılabilir durumdadır.

## <a name="performance-record-properties"></a>Performans kaydı Özellikler
Performans kayıtları sahip bir tür **Perf** ve aşağıdaki tabloda özelliklere sahiptir.

| Özellik | Açıklama |
|:--- |:--- |
| Bilgisayar |Olay toplandığı bilgisayar. |
| CounterName |Performans sayacı adı |
| Sayaç yolu |Sayaç biçiminde tam yolunu \\ \\ \<bilgisayar >\\nesne(örnek)\\sayacı. |
| CounterValue |Sayacın sayısal değer. |
| InstanceName |Olay örneğinin adı.  Hiçbir örnek varsa boş. |
| ObjectName |Performans nesnesi adı |
| SourceSystem |Verileri toplandığı aracı türü. <br><br>OpsManager – Windows aracı, ya da doğrudan bağlanın veya SCOM <br> Linux – tüm Linux aracıları  <br> AzureStorage – Azure tanılama |
| TimeGenerated |Tarih ve saat verileri örneklenen. |

## <a name="sizing-estimates"></a>Boyutlandırma tahmin eder
 Belirli bir sayaç koleksiyonu 10 saniyelik aralıklarda için kabaca bir tahmin örneği başına günde yaklaşık 1 MB ' dir.  Aşağıdaki formülü olan belirli bir sayaç depolama gereksinimlerini tahmin edebilirsiniz.

    1 MB x (number of counters) x (number of agents) x (number of instances)

## <a name="log-searches-with-performance-records"></a>Performans kayıtlarla günlük aramalar
Aşağıdaki tabloda farklı performans kayıtları almak günlük arama örnekleri sağlar.

| Sorgu | Açıklama |
|:--- |:--- |
| Türü Perf = |Tüm performans verileri |
| Türü Perf bilgisayar = "Bilgisayarım" = |Belirli bir bilgisayardaki tüm performans verileri |
| Türü Perf CounterName = "Geçerli Disk Sırası Uzunluğu" = |Belirli bir sayaç için tüm performans verileri |
| Türü Perf = (ObjectName işlemci =) CounterName "% işlemci zamanı" InstanceName = = _Total &#124; Bilgisayar tarafından AVGCPU olarak AVG(Average) ölçme |Tüm bilgisayarlardaki ortalama CPU kullanımı |
| Türü Perf = (CounterName = "% işlemci zamanı") &#124;  Bilgisayar tarafından ölçü max(Max) |Tüm bilgisayarlardaki en fazla CPU kullanımı |
| Türü Perf ObjectName = MantıksalDisk CounterName = = "Geçerli Disk Sırası Uzunluğu" bilgisayar = "MyComputerName" &#124; Ölçü InstanceName tarafından Avg(Average) |Belirli bir bilgisayarın tüm örneklerde ortalama geçerli Disk Sırası Uzunluğu |
| Türü Perf CounterName = = "DiskTransfers/sn" &#124; Bilgisayar tarafından ölçü percentile95(Average) |95 yüzdebirlik, Disk aktarımı/sn tüm bilgisayarlardaki |
| Türü Perf CounterName = "% işlemci zamanı" InstanceName = = "_Toplam" &#124; AVG(CounterValue) 1 saat bilgisayar aralığına göre ölçün |Saatlik tüm bilgisayarlardaki CPU kullanımı ortalaması |
| Türü Perf bilgisayar = "Bilgisayarım" CounterName = % * InstanceName = = _Total &#124; Ölçü percentile70(CounterValue) CounterName aralığı 1 saat |Belirli bir bilgisayar için her % yüzde sayacın saatlik 70 yüzdebirlik |
| Türü Perf CounterName = "% işlemci zamanı" InstanceName = "_Toplam" = (bilgisayar = "Bilgisayarım") &#124; Min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) 1 saat bilgisayar aralığına göre ölçün |Saatlik ortalama, en az, en fazla ve 75-yüzdelik CPU kullanımı belirli bir bilgisayar için |
| Türü Perf ObjectName = = "MSSQL$ INST2: veritabanları" InstanceName ana = | Adlandırılmış SQL Server örneğinden INST2 ana veritabanı için veritabanı performans nesnesinden tüm performans verileri.  

>[!NOTE]
> Çalışma alanınız [yeni Log Analytics sorgu diline](log-analytics-log-search-upgrade.md) yükseltilmişse, yukarıdaki sorguların aşağıdaki gibi değiştirilmesi gerekir.

> | Sorgu | Açıklama |
|:--- |:--- |
| Perf |Tüm performans verileri |
| Perf &#124; Burada bilgisayar "Bilgisayarım" == |Belirli bir bilgisayardaki tüm performans verileri |
| Perf &#124; CounterName burada "Geçerli Disk Sırası Uzunluğu" == |Belirli bir sayaç için tüm performans verileri |
| Perf &#124; Burada ObjectName "İşlemci" ve CounterName == "% işlemci zamanı" ve InstanceName == "_Toplam" &#124; == AVGCPU özetlemek bilgisayar tarafından avg(Average) = |Tüm bilgisayarlardaki ortalama CPU kullanımı |
| Perf &#124; CounterName burada "% işlemci zamanı" & #124 ==; AggregatedValue özetlemek bilgisayar tarafından max(Max) = |Tüm bilgisayarlardaki en fazla CPU kullanımı |
| Perf &#124; Burada ObjectName "MantıksalDisk" ve CounterName == "Geçerli Disk Sırası Uzunluğu" ve bilgisayar == "MyComputerName" &#124; == AggregatedValue özetlemek InstanceName tarafından avg(Average) = |Belirli bir bilgisayarın tüm örneklerde ortalama geçerli Disk Sırası Uzunluğu |
| Perf &#124; CounterName burada "DiskTransfers/sn" & #124 ==; AggregatedValue özetlemek yüzdebirlik (ortalama, 95) bilgisayar tarafından = |95 yüzdebirlik, Disk aktarımı/sn tüm bilgisayarlardaki |
| Perf &#124; "% işlemci zamanı" ve InstanceName CounterName burada == "_Toplam" &#124; == AggregatedValue özetlemek bin (TimeGenerated, 1 h), bilgisayar tarafından avg(CounterValue) = |Saatlik tüm bilgisayarlardaki CPU kullanımı ortalaması |
| Perf &#124; Burada bilgisayar "Bilgisayarım" ve CounterName startswith_cs "%" ve InstanceName == "_Toplam" &#124; == AggregatedValue özetlemek depo tarafından (TimeGenerated, 1 h), CounterName yüzdebirlik (CounterValue, 70) = | Belirli bir bilgisayar için her % yüzde sayacın saatlik 70 yüzdebirlik |
| Perf &#124; "% işlemci zamanı" ve InstanceName CounterName burada == "_Toplam" ve bilgisayar == "Bilgisayarım" &#124; == ["min(CounterValue)"] özetlemek min(CounterValue), = ["avg(CounterValue)"] avg(CounterValue), = ["percentile75(CounterValue)"] yüzdebirlik (CounterValue, 75), = ["max(CounterValue)"] bin (TimeGenerated, 1 h), bilgisayar tarafından max(CounterValue) = |Saatlik ortalama, en az, en fazla ve 75-yüzdelik CPU kullanımı belirli bir bilgisayar için |
| Perf &#124; Burada ObjectName == "MSSQL$ INST2: veritabanları" ve InstanceName "ana" == | Adlandırılmış SQL Server örneğinden INST2 ana veritabanı için veritabanı performans nesnesinden tüm performans verileri.  

## <a name="viewing-performance-data"></a>Performans verileri görüntüleme
Performans verileri günlük Ara çalıştırdığınızda **listesi** görünümü, varsayılan olarak görüntülenir.  Grafik formunda verileri görüntülemek için tıklatın **ölçümleri**.  Ayrıntılı bir grafik görünümü için tıklatın  **+**  bir sayaç yanındaki.  

![Daraltılmış ölçümleri Görünüm](media/log-analytics-data-sources-performance-counters/metricscollapsed.png)

Günlük arama performans verilerini toplama için bkz: [isteğe bağlı ölçüm toplama ve OMS görselleştirme](http://blogs.technet.microsoft.com/msoms/2016/02/26/on-demand-metric-aggregation-and-visualization-in-oms/).


## <a name="next-steps"></a>Sonraki adımlar
* [Linux uygulamalardan performans sayaçlarını Topla](log-analytics-data-sources-linux-applications.md) MySQL ve Apache HTTP Server dahil olmak üzere.
* Hakkında bilgi edinin [oturum aramaları](log-analytics-log-searches.md) veri kaynakları ve çözümleri toplanan verileri çözümlemek için.  
* Toplanan verileri dışarı aktarma [Power BI](log-analytics-powerbi.md) ek görselleştirmeleri ve analiz için.
