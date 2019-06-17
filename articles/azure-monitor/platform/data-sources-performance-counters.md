---
title: Toplama ve performans sayaçları Azure İzleyici'de çözümleme | Microsoft Docs
description: Performans sayaçları, Windows ve Linux aracılarını performansını analiz etmek için Azure İzleyici tarafından toplanır.  Bu makalede her iki Windows performans sayaçlarını toplamayı yapılandırma ve Linux aracıları, bunlar ayrıntılarını çalışma ve bunları Azure portalında çözümlemek nasıl depolanır.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 20e145e4-2ace-4cd9-b252-71fb4f94099e
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2018
ms.author: magoedte
ms.openlocfilehash: 76f4061af816c59e644db99913193ed6fcf24d18
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65205747"
---
# <a name="windows-and-linux-performance-data-sources-in-azure-monitor"></a>Azure İzleyici'de Windows ve Linux performans veri kaynakları
Windows ve Linux performans sayaçları, performans donanım bileşenleri, işletim sistemleri ve uygulamalar hakkında bilgi sağlar.  Azure İzleyici, neredeyse gerçek zamanlı (NRT) analiz uzun süreli analiz için performans verilerini toplama ve raporlama yanı sıra sık aralıklarla performans sayaçları toplayabilirsiniz.

![Performans sayaçları](media/data-sources-performance-counters/overview.png)

## <a name="configuring-performance-counters"></a>Performans sayaçları yapılandırılıyor
Performans sayaçlarını Yapılandır [Gelişmiş ayarlar veri menüde](agent-data-sources.md#configuring-data-sources).

Yeni bir çalışma alanı için Windows veya Linux performans sayaçlarını ilk kez yapılandırırken, birkaç ortak sayacı hızlı bir şekilde oluşturma seçenekleri sunulur.  Her birinin yanında bir onay kutusu görüntülenir.  İlk olarak oluşturmak istediğiniz herhangi bir sayaç denetlenir ve ardından olun **Seçili performans sayaçlarını Ekle**.

Windows performans sayaçları için her performans sayacı için belirli bir örneğine seçebilirsiniz. Linux performans sayaçları için seçtiğiniz her bir sayacın örneğini üst sayacı tüm alt sayaçları için geçerlidir. Aşağıdaki tabloda, hem Linux hem de Windows Performans sayaçlarıyla kullanılabilir ortak örnekleri gösterilmektedir.

| Örnek adı | Açıklama |
| --- | --- |
| \_Toplam |Tüm örneklerin toplam |
| \* |Tüm örnekler |
| (/&#124;/var) |Adlandırılmış örneklerle eşleşir: / veya /var |

### <a name="windows-performance-counters"></a>Windows performans sayaçları

![Windows performans sayaçlarını Yapılandır](media/data-sources-performance-counters/configure-windows.png)

Toplanacak yeni bir Windows performans sayacı eklemek için bu yordamı izleyin.

1. Sayaç adı biçimi metin kutusuna yazın *nesne (örnek) \counter*.  Yazmaya başladığınızda, eşleşen bir sık kullanılan sayaçları listesi ile gösterilir.  Bir sayaç, liste veya kendi yazın ya da seçebilirsiniz.  Belirterek belirli bir sayaç için tüm örnekleri döndürebilir *object\counter*.  

    SQL Server performans sayaçları adlandırılmış örneklerden toplarken, tüm örnek sayaçları başlama adlı *MSSQL$* ve ardından örneğinin adı.  Örneğin, tüm veritabanları için adlandırılmış bir SQL veritabanı performans nesnesinden INST2 örneği için günlük önbellek isabet oranını sayacı toplamak için belirtin `MSSQL$INST2:Databases(*)\Log Cache Hit Ratio`.

2. Tıklayın **+** veya basın **Enter** sayaç listesine eklenecek.
3. Bir sayacı eklediğinizde, 10 saniye boyunca varsayılan kullanır, **örnekleme aralığı**.  Toplanan performans verilerini depolama gereksinimlerini azaltmak istiyorsanız bunu 1800 saniyeye kadar (30 dakika) daha yüksek bir değere değiştirebilirsiniz.
4. Ekleme sayaçları işiniz bittiğinde tıklayın **Kaydet** yapılandırmayı kaydetmek için ekranın üstünde düğme.

### <a name="linux-performance-counters"></a>Linux performans sayaçları

![Linux performans sayaçlarını Yapılandır](media/data-sources-performance-counters/configure-linux.png)

Yeni Linux performans sayacı toplamak için eklemek için bu yordamı izleyin.

1. Varsayılan olarak, tüm yapılandırma değişiklikleri tüm aracılara otomatik olarak gönderilir.  Linux aracıları için bir yapılandırma dosyası Fluentd veri toplayıcısı gönderilir.  Her bir Linux aracı bu dosyayı el ile değiştirmek istiyorsanız, kutunun işaretini kaldırın *aşağıdaki yapılandırmayı Linux makinelerime Uygula* ve aşağıdaki yönergeleri izleyin.
2. Sayaç adı biçimi metin kutusuna yazın *nesne (örnek) \counter*.  Yazmaya başladığınızda, eşleşen bir sık kullanılan sayaçları listesi ile gösterilir.  Bir sayaç, liste veya kendi yazın ya da seçebilirsiniz.  
3. Tıklayın **+** veya basın **Enter** nesnesinin diğer sayaçlarının listesi sayaç eklemek için.
4. Bir nesne için tüm sayaçlar kullandığınızdan **örnekleme aralığı**.  Varsayılan değer 10 saniyedir.  Toplanan performans verilerini depolama gereksinimlerini azaltmak istiyorsanız bu 1800 saniyeye kadar (30 dakika) daha yüksek bir değerle değiştirin.
5. Ekleme sayaçları işiniz bittiğinde tıklayın **Kaydet** yapılandırmayı kaydetmek için ekranın üstünde düğme.

#### <a name="configure-linux-performance-counters-in-configuration-file"></a>Linux performans sayaçlarıyla yapılandırma dosyasındaki yapılandırma
Azure portalını kullanarak Linux performans sayaçlarıyla yapılandırmak yerine, yapılandırma dosyalarını Linux aracısı üzerinde düzenleme seçeneğiniz vardır.  Toplanacak performans ölçümleri, yapılandırmada tarafından denetlenen **/etc/opt/microsoft/omsagent/\<çalışma alanı kimliği\>/conf/omsagent.conf**.

Her nesne veya performans ölçümleri toplamak için bir kategori olarak tek bir yapılandırma dosyasında tanımlanmalıdır `<source>` öğesi. Söz dizimi aşağıdaki deseni izler.

    <source>
      type oms_omi  
      object_name "Processor"
      instance_regex ".*"
      counter_name_regex ".*"
      interval 30s
    </source>


Bu öğe içindeki parametreler aşağıdaki tabloda açıklanmıştır.

| Parametreler | Açıklama |
|:--|:--|
| Nesne\_adı | Koleksiyon için nesne adı. |
| örnek\_normal ifade |  A *normal ifade* hangi örneklerinin toplamak için tanımlama. Değer: `.*` tüm örnekleri belirtir. İşlemci ölçümlerimi yalnızca toplamak için \_toplam örnek belirttiğiniz `_Total`. Yalnızca crond veya sshd örnekleri için işlem ölçümleri toplamak için belirtebilirsiniz: `(crond\|sshd)`. |
| Sayaç\_adı\_normal ifade | A *normal ifade* hangi (nesne için) sayaçlarının tanımlama. Nesne için tüm sayaçları toplamak için bu seçeneği belirtin: `.*`. Yalnızca bellek nesne için takas alanı sayaçları toplamak için örneğin, belirtebilirsiniz: `.+Swap.+` |
| interval | Toplanan ve nesnenin sayaçları sıklığı. |


Aşağıdaki tabloda, nesneleri ve yapılandırma dosyasında belirttiğiniz sayaçları listeler.  Kullanılabilir ek sayaçları belirli uygulamalar için açıklandığı [Azure İzleyici'de Linux uygulamaları için performans sayaçları toplamak](data-sources-linux-applications.md).

| Nesne adı | Sayaç adı |
|:--|:--|
| Mantıksal Disk | % Boş Inode'ları |
| Mantıksal Disk | % Boş alan |
| Mantıksal Disk | % Kullanılan Inode |
| Mantıksal Disk | % Kullanılan alan |
| Mantıksal Disk | Disk Okuma Bayt/sn |
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
| Ağ | Aktarılan toplam bayt |
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
| Process | PCT ayrıcalıklı zaman |
| Process | PCT kullanıcı zamanı |
| Process | Kullanılan bellek KB |
| Process | Paylaşılan sanal bellek |
| İşlemci | % DPC Zamanı |
| İşlemci | % Boş zaman |
| İşlemci | % Kesme Zamanı |
| İşlemci | % GÇ bekleme zamanı |
| İşlemci | % İyi olma zamanı |
| İşlemci | % Ayrıcalıklı Zaman |
| İşlemci | % İşlemci zamanı |
| İşlemci | % Kullanıcı Zamanı |
| Sistem | Boş fiziksel bellek |
| Sistem | Disk belleği dosyasındaki boş alan |
| Sistem | Boş sanal bellek |
| Sistem | İşlemler |
| Sistem | Disk belleği dosyalarında depolanan boyut |
| Sistem | Çalışma Süresi |
| Sistem | Kullanıcılar |


Performans ölçümleri için varsayılan yapılandırma aşağıda verilmiştir.

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
Azure İzleyici, sayaç yüklü olan tüm aracıları, belirtilen örnekleme aralığında tüm performans sayaçlarını toplar.  Veriler değil toplanır ve ham veriler, aboneliğinizi tarafından belirtilen süre için tüm günlük sorgu görünümlerde kullanılabilir.

## <a name="performance-record-properties"></a>Performans kayıt özellikleri
Performans kayıtları sahip bir tür **Perf** ve aşağıdaki tabloda gösterilen özelliklere sahiptir.

| Özellik | Açıklama |
|:--- |:--- |
| Bilgisayar |Olay toplandığı bilgisayar. |
| CounterName |Performans sayacı adı |
| Sayaç yolu |Tam yol biçiminde bir sayacın \\ \\ \<bilgisayar >\\nesne(örnek)\\sayacı. |
| Ort |Sayacın sayısal değer. |
| InstanceName |Olay örneğinin adı.  Hiçbir örnek varsa boş. |
| ObjectName |Performans nesnesi adı |
| SourceSystem |Aracı verileri toplandığı türü. <br><br>OpsManager – Windows Aracısı, doğrudan bağlanın veya SCOM <br> Linux – tüm Linux aracıları  <br> AzureStorage – Azure tanılama |
| TimeGenerated |Tarih ve saat verileri örneklenen. |

## <a name="sizing-estimates"></a>Boyutlandırma tahminleri
 Belirli bir sayaç koleksiyonu 10 saniyelik aralıklarla için yaklaşık bir tahmin örnek başına günde yaklaşık 1 MB ' dir.  Aşağıdaki formülü olan belirli bir sayaç depolama gereksinimlerini tahmin edebilirsiniz.

    1 MB x (number of counters) x (number of agents) x (number of instances)

## <a name="log-queries-with-performance-records"></a>Performans kayıtları ile günlük sorguları
Aşağıdaki tabloda farklı performans kayıtları almak günlük sorguları örnekleri sağlar.

| Sorgu | Açıklama |
|:--- |:--- |
| Perf |Tüm performans verileri |
| Perf &#124; nerede bilgisayar "Bilgisayarım" == |Belirli bir bilgisayardaki tüm performans verileri |
| Perf &#124; CounterName burada "Geçerli Disk Sırası Uzunluğu" == |Belirli bir sayaç için tüm performans verileri |
| Perf &#124; nerede ObjectName "İşlemci" ve CounterName == "% işlemci zamanı" ve InstanceName == "_Toplam" == &#124; AVGCPU özetlemek avg(CounterValue) bilgisayar tarafından = |Tüm bilgisayarlardaki ortalama CPU kullanımı |
| Perf &#124; CounterName burada "% işlemci zamanı" == &#124; Summarize aggregatedvalue = max(CounterValue) bilgisayar tarafından |Tüm bilgisayarlardaki en fazla CPU kullanımı |
| Perf &#124; nerede ObjectName "LogicalDisk" ve CounterName == "Geçerli Disk Sırası Uzunluğu" ve bilgisayar == "MyComputerName" == &#124; Summarize aggregatedvalue = avg(CounterValue) InstanceName tarafından |Belirli bir bilgisayarın tüm örneklerdeki ortalama geçerli Disk Sırası Uzunluğu |
| Perf &#124; CounterName burada "Disk aktarımı/sn" == &#124; Summarize aggregatedvalue = bilgisayar tarafından yüzdebirlik (Ort, 95) |95\. yüzdebirlik, Disk aktarımı/sn tüm bilgisayarlardaki |
| Perf &#124; CounterName burada "% işlemci zamanı" ve InstanceName == "_Toplam" == &#124; Summarize aggregatedvalue = avg(CounterValue) bin (TimeGenerated, 1 saat), bilgisayar tarafından |Saatlik tüm bilgisayarlardaki CPU kullanımı ortalaması |
| Perf &#124; nerede bilgisayar "Bilgisayarım" ve CounterName startswith_cs "%" ve InstanceName == "_Toplam" == &#124; Summarize aggregatedvalue göre gruplama (TimeGenerated, 1 saat), CounterName yüzdebirlik (Ort, 70) = | Belirli bir bilgisayar için her % yüzde sayacın saatlik 70 yüzdebirlik |
| Perf &#124; CounterName burada "% işlemci zamanı" ve InstanceName == "_Toplam" ve bilgisayar == "Bilgisayarım" == &#124; ["min(CounterValue)"] özetlemek min(CounterValue), = ["avg(CounterValue)"] avg(CounterValue), = ["percentile75(CounterValue)"] yüzdebirlik (Ort, 75), = ["max(CounterValue)"] max(CounterValue) bin (TimeGenerated, 1 saat), bilgisayar tarafından = |Saatlik ortalama, minimum, maksimum ve 75 yüzdebirlik CPU kullanımı belirli bir bilgisayar için |
| Perf &#124; where ObjectName == "MSSQL$INST2:Databases" and InstanceName == "master" | Adlandırılmış SQL Server örneğinden INST2 ana veritabanı için veritabanı performans nesnesinden tüm performans verileri.  




## <a name="next-steps"></a>Sonraki adımlar
* [Linux uygulamaları performans sayaçlarını Topla](data-sources-linux-applications.md) MySQL ve Apache HTTP Server dahil olmak üzere.
* Hakkında bilgi edinin [oturum sorguları](../log-query/log-query-overview.md) veri kaynakları ve çözümlerinden toplanan verileri analiz etmek için.  
* İçin toplanan verileri dışarı aktarma [Power BI](powerbi.md) ek görselleştirmeler ve analiz için.
