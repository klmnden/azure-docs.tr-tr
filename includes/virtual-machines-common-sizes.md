


Azure'da birden fazla standart boyut seçeneği vardır. Bu boyutlarla ilgili önemli noktalardan bazıları şunlardır:

* D Serisi VM'ler, daha yüksek işlem gücüne ve geçici süreli disk performansına ihtiyaç duyan uygulamaları çalıştıracak şekilde tasarlanmıştır. D Serisi VM'ler daha hızlı işlemcilere, daha yüksek bellek-çekirdek oranına ve geçici disk için katı hal sürücüsüne (SSD) sahiptir. Ayrıntılı bilgi için Azure blogundaki [Yeni D Serisi Sanal Makine Boyutları](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/) duyurusunu inceleyin.
* Orijinal D Serisinin üzerine geliştirilen Dv2 Serisi, daha güçlü bir CPU'ya sahiptir. Dv2 Serisi CPU, D Serisi CPU'dan yaklaşık %35 daha hızlıdır. Yeni nesil 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) işlemciyi temel alır ve Intel Turbo Boost Technology 2.0 ile 3,1 GHz'e varan hızlara çıkabilir. Dv2 Serisi, D Serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.
* F Serisi, Intel Turbo Boost Technology 2.0 ile 3,1 GHz'e varan saat hızlarına ulaşabilen 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) işlemciyi temel alır. Bu değer Dv2 Serisi VM'lerdeki CPU performansıyla aynıdır.  Daha düşük bir saatlik liste fiyatına sahip olan F Serisi, çekirdek başına Azure İşlem Birimi (ACU) açısından fiyat-performans alanında Azure portföyündeki en iyi seçenektir. 
  
    F Serisi ile ayrıca Azure'daki VM boyutu adlandırmalarında yeni bir standart kullanıma sunulmuştur. Bu seride ve gelecekte yayımlanacak VM boyutlarında aile adını belirten harften sonra gelen sayı değeri, CPU çekirdeği sayısını gösterecektir. İyileştirilmiş premium depolama gibi ek özellikler, CPU çekirdeği sayısını gösteren rakamdan sonra gelen harflerle ifade edilecektir. Bu adlandırma biçimi gelecekte yayımlanacak VM boyutları için kullanılacak ancak önceden yayımlanmış olan VM boyutlarının adında geçmişe dönük bir değişiklik yapılmayacaktır.
* En fazla belleği sunan G Serisi VM'ler, Intel Xeon E5 V3 ailesi işlemcilere sahip ana bilgisayarlarda çalışır.
* DS Serisi, DSv2 Serisi, Fs Serisi ve GS Serisi VM'ler, yoğun G/Ç kullanımlı iş yükleri için yüksek performanslı ve düşük gecikmeli Premium Depolama kullanabilir. Bu VM'ler, sanal makine disklerini barındırmak için katı hal sürücüsü (SSD) kullanmanın yanı sıra yerel SSD disk önbelleğine de sahiptir. Premium Depolama belirli bölgelerde kullanılabilir. Ayrıntılar için bkz. [Premium Depolama: Azure sanal makine iş yükleri için yüksek performanslı depolama](../articles/storage/storage-premium-storage.md).
*   A Serisi ve Av2 Serisi VM'ler, farklı donanım türleri ve işlemciler üzerinde dağıtılabilir. Dağıtıldığı donanımdan bağımsız olarak, çalışan örneğe tutarlı işlemci performansı sunmak için boyut donanıma göre genişletilir. Bu boyutun dağıtıldığı fiziksel donanımı belirlemek için Sanal Makinenin içinden sanal donanımı sorgulayın.
* A0 boyutunun fiziksel donanım üzerindeki abone sayısı planlanandan fazladır. Yalnızca bu boyutta diğer müşteri dağıtımları, çalışan iş yükünüzün performansını etkileyebilir. Göreli performans aşağıda beklenen temel düzey olarak belirtilmiştir ve %15 oranında değişiklik gösterebilir.

Sanal makinenin boyutu, fiyatlandırmayı etkiler. Boyut, sanal makinenin işlem, bellek ve depolama kapasitesini de etkiler. Depolama maliyetleri, depolama hesabında kullanılan sayfa sayısına göre ayrıca hesaplanır. Ayrıntılar için bkz. [Sanal Makine Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/virtual-machines/) ve [Azure Depolama Fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/). 

Aşağıdaki önemli noktalar boyut konusunda karar vermenize yardımcı olabilir:

* A8-A11 ve H Serisi boyutlar *yoğun işlem gücü kullanımlı örnekler* olarak da bilinir. Bu boyutları çalıştıran donanım; yüksek performanslı bilgi işlem (HPC) kümesi uygulamaları, modellemeler ve simülasyonlar gibi yoğun işlem ve ağ kullanımlı uygulamalar için tasarlanmış ve iyileştirilmiştir. A8-A11 Serisinde, Intel Xeon E5-2670 @ 2,6 GHZ, H Serisinde ise Intel Xeon E5-2667 v3 @ 3,2 GHz işlemciler kullanılmaktadır. Ayrıntılı bilgiler ve bu boyutları kullanırken dikkat edilmesi gereken noktalar için bkz. [H Serisi ve yoğun işlem gücü kullanımlı A Serisi VM'ler hakkında](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
* Dv2 Serisi, D Serisi, G Serisi ve bunlara karşılık gelen DS/GS modelleri daha hızlı CPU'ya, daha iyi yerel disk performansına veya daha yüksek belleğe ihtiyaç duyan uygulamalar için idealdir.  Bu seçenekler birçok kurumsal sınıf uygulama için güçlü bir bileşim sunar.
* F Serisi VM'ler, daha hızlı CPU'ya ihtiyaç duyan ancak çok fazla belleğe veya CPU çekirdeği başına yerel SSD'ye ihtiyaç duymayan iş yükleri için harika bir seçenektir.  Analitikler, oyun sunucuları, web sunucuları ve toplu işleme gibi iş yükleri, F Serisinin özelliklerinden en fazla yarar sağlayacak iş yükleridir.
* Azure veri merkezlerindeki fiziksel ana bilgisayarlardan bazıları A5 – A11 gibi daha büyük sanal makine boyutlarını desteklemeyebilir. Bunun sonucunda var olan bir sanal makine için yeni boyut belirlerken, 16 Nisan 2013'dan önce oluşturulmuş bir sanal ağda yeni bir sanal makine oluştururken veya yeni bir sanal makineyi var olan bir bulut hizmetine eklerken **Sanal makine yapılandırılamadı<machine name>** veya **Sanal makine oluşturulamadı<machine name>** hata iletisini görebilirsiniz. Her dağıtım senaryosuna özel geçici çözümler için destek forumundaki [Hata: "Sanal makine yapılandırılamadı"](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) konusunu inceleyin.  
* Ayrıca aboneliğiniz, belirli boyut ailelerinde dağıtabileceğiniz çekirdek sayısını da sınırlıyor olabilir. Artırmak istediğini kotalar için Azure Desteği'ne başvurun.

## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar
Azure SKU'larının işlem (CPU) performansını karşılaştırabilmek için Azure İşlem Birimi (ACU) birimini ortaya çıkardık. Bu birim, performans ihtiyaçlarınızı karşılayabilecek SKU'yu kolayca belirlemenize yardımcı olacak.  ACU şu anda Küçük (Standard_A1) VM'de 100 olarak standart haline getirilmiş ve diğer tüm SKU'lar, standart bir karşılaştırmalı testte sunabilecekleri yaklaşık hıza göre derecelendirilmiştir. 

> [!IMPORTANT]
> ACU yalnızca rehberlik etme amacı taşımaktadır.  İş yükünüzle aldığınız sonuçlar farklılık gösterebilir. 
> 
> 

<br>

| SKU Ailesi | ACU/Çekirdek |
| --- | --- |
| [A0](#a-series) |50 |
| [A1-A4](#a-series) |100 |
| [A5-A7](#a-series) |100 |
| [A1_v2-A8_v2](#av2-series) |100 |
| [A2m_v2-A8m_v2](#av2-series) |100 |
| [A8-A11](#a-series) |225* |
| [D1-D14](#d-series) |160 |
| [D1_v2-D15_v2](#dv2-series) |210-250* |
| [DS1-DS14](#ds-series) |160 |
| [DS1_v2-DS15_v2](#dsv2-series) |210-250* |
| [F1-F16](#f-series) |210-250* |
| [F1s-F16s](#fs-series) |210-250* |
| [G1-G5](#g-series) |180-240* |
| [GS1-GS5](#gs-series) |180-240* |
| [H](#h-series) |290-300* |
| [L4s L32s](#l-series) |180-240* |

* işaretli ACU'lar, CPU frekansını artırmak ve performans artışı sağlamak için Intel® Turbo Boost teknolojisinden faydalanır.  Performans artışının oranı VM boyutuna, iş yüküne ve aynı ana bilgisayarda çalışan iş yüklerine göre değişiklik gösterebilir.

## <a name="size-tables"></a>Boyut tabloları
Aşağıdaki tablolarda boyutlara ve sundukları kapasiteye yer verilmiştir.

* Depolama kapasitesi GiB veya 1024^3 bayt cinsinden gösterilmiştir. GB (1000^3 bayt) ile ölçülen diskleri GiB (1024^3 bayt) ile ölçülen disklerle karşılaştırırken GiB cinsinden verilen kapasite rakamlarının daha küçük görünebileceğini unutmayın. Örneğin: 1023 GiB = 1098,4 GB
* Disk aktarım hızı, saniye başına giriş/çıkış işlemi sayısı (IOPS) ve MB/sn (MB/sn = 10^6 bayt/sn) üzerinden ölçülür.
* Veri diskleri önbelleğe alınmış veya alınmamış modlarda çalışabilir.  Önbelleğe alınmış veri diski işlemi için ana bilgisayar önbelleği **SaltOkunur** veya **OkuYaz** moduna ayarlanır.  Önbelleğe alınmamış veri diski işlemi için ana bilgisayar önbelleği **Yok** moduna ayarlanır.
* Maksimum ağ bant genişliği, VM türü başına ayrılan ve atanan toplam maksimum bant genişliğidir. Maksimum bant genişliği, yeterli ağ kapasitesinin mevcut olduğundan emin olarak doğru VM'i seçme konusunda yardımcı olur. Düşük, Orta, Yüksek ve Çok Yüksek arasında geçiş yapıldığında aktarım hızı da artacaktır. Gerçek ağ performansı, ağ ve uygulama yüklerinin yanı sıra uygulama ağ ayarları gibi birçok faktöre bağlıdır.

## <a name="a-series"></a>A Serisi
| Boyut | CPU çekirdekleri | Bellek: GiB | Yerel HDD: GiB | Maksimum veri diskleri | Maksimum veri diski aktarım hızı: IOPS | Maksimum NIC/Ağ bant genişliği |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A0 |1 |0,768 |20 |1 |1x500 |1/düşük |
| Standard_A1 |1 |1,75 |70 |2 |2x500 |1/orta |
| Standard_A2 |2 |3,5 |135 |4 |4x500 |1/orta |
| Standard_A3 |4 |7 |285 |8 |8x500 |2/yüksek |
| Standard_A4 |8 |14 |605 |16 |16x500 |4/yüksek |
| Standard_A5 |2 |14 |135 |4 |4X500 |1/orta |
| Standard_A6 |4 |28 |285 |8 |8x500 |2/yüksek |
| Standard_A7 |8 |56 |605 |16 |16x500 |4/yüksek |

<br>

## <a name="a-series---compute-intensive-instances"></a>A Serisi - Yoğun işlem gücü kullanımlı örnekler
Bilgi ve bu boyutları kullanırken dikkat edilmesi gereken noktalar için bkz. [H Serisi ve yoğun işlem gücü kullanımlı A Serisi VM'ler hakkında](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

| Boyut | CPU çekirdekleri | Bellek: GiB | Yerel HDD: GiB | Maksimum veri diskleri | Maksimum veri diski aktarım hızı: IOPS | Maksimum NIC/Ağ bant genişliği |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A8* |8 |56 |382 |16 |16x500 |2/yüksek |
| Standard_A9* |16 |112 |382 |16 |16x500 |4/çok yüksek |
| Standard_A10 |8 |56 |382 |16 |16x500 |2/yüksek |
| Standard_A11 |16 |112 |382 |16 |16x500 |4/çok yüksek |

*RDMA özellikli

<br>

## <a name="av2-series"></a>Av2 Serisi

| Boyut        | CPU çekirdekleri | Bellek: GiB | Yerel SSD: GiB | Maksimum veri diskleri | Maksimum veri diski aktarım hızı: IOPS | Maksimum NIC/Ağ bant genişliği |
|-------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A1_v2 | 1         | 2            | 10                   | 2              | 2x500              | 1/orta              |
| Standard_A2_v2 | 2         | 4            | 20                   | 4              | 4x500              | 2/orta              |
| Standard_A4_v2 | 4         | 8            | 40                   | 8              | 8x500              | 4/yüksek                  |
| Standard_A8_v2 | 8         | 16           | 80                   | 16             | 16x500             | 8/yüksek                  |
| Standard_A2m_v2 | 2        | 16           | 20                   | 4              | 4X500              | 2/orta              |
| Standard_A4m_v2 | 4        | 32           | 40                   | 8              | 8x500              | 4/yüksek                  |
| Standard_A8m_v2 | 8        | 64           | 80                   | 16             | 16x500             | 8/yüksek                  |


## <a name="d-series"></a>D Serisi
| Boyut | CPU çekirdekleri | Bellek: GiB | Yerel SSD: GiB | Maksimum veri diskleri | Maksimum veri diski aktarım hızı: IOPS | Maksimum NIC/Ağ bant genişliği |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_D1 |1 |3,5 |50 |2 |2x500 |1/orta |
| Standard_D2 |2 |7 |100 |4 |4x500 |2/yüksek |
| Standard_D3 |4 |14 |200 |8 |8x500 |4/yüksek |
| Standard_D4 |8 |28 |400 |16 |16x500 |8/yüksek |
| Standard_D11 |2 |14 |100 |4 |4x500 |2/yüksek |
| Standard_D12 |4 |28 |200 |8 |8x500 |4/yüksek |
| Standard_D13 |8 |56 |400 |16 |16x500 |8/yüksek |
| Standard_D14 |16 |112 |800 |32 |32x500 |8/çok yüksek |

<br>

## <a name="dv2-series"></a>Dv2 Serisi
| Boyut | CPU çekirdekleri | Bellek: GiB | Yerel SSD: GiB | Maksimum veri diskleri | Maksimum veri diski aktarım hızı: IOPS | Maksimum NIC/Ağ bant genişliği |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_D1_v2 |1 |3,5 |50 |2 |2x500 |1/orta |
| Standard_D2_v2 |2 |7 |100 |4 |4x500 |2/yüksek |
| Standard_D3_v2 |4 |14 |200 |8 |8x500 |4/yüksek |
| Standard_D4_v2 |8 |28 |400 |16 |16x500 |8/yüksek |
| Standard_D5_v2 |16 |56 |800 |32 |32x500 |8/aşırı yüksek |
| Standard_D11_v2 |2 |14 |100 |4 |4x500 |2/yüksek |
| Standard_D12_v2 |4 |28 |200 |8 |8x500 |4/yüksek |
| Standard_D13_v2 |8 |56 |400 |16 |16x500 |8/yüksek |
| Standard_D14_v2 |16 |112 |800 |32 |32x500 |8/aşırı yüksek |
| Standard_D15_v2** |20 |140 |1000 |40 |40x500 |8/aşırı yüksek* |

*Bazı bölgelerde Standard_D15_v2 boyutuyla hızlandırılmış ağ iletişimi kullanılabilir. Bu özelliğin kullanım durumu ve şekli hakkında daha fazla bilgi için bkz. [Hızlandırılmış Ağ İletişimi Önizlemede](https://azure.microsoft.com/updates/accelerated-networking-in-preview/) ve [Sanal makine için Hızlandırılmış Ağ İletişimi](../articles/virtual-network/virtual-network-accelerated-networking-powershell.md).

**Örnek, tek bir müşteriye özel donanımla yalıtılmıştır.

<br>

## <a name="ds-series"></a>DS Serisi*
| Boyut | CPU çekirdekleri | Bellek: GiB | Yerel SSD: GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış disk aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Maksimum NIC/Ağ bant genişliği |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1 |1 |3,5 |7 |2 |4000/32 (43) |3200/32 |1/orta |
| Standard_DS2 |2 |7 |14 |4 |8000/64 (86) |6400/64 |2/yüksek |
| Standard_DS3 |4 |14 |28 |8 |16.000/128 (172) |12.800/128 |4/yüksek |
| Standard_DS4 |8 |28 |56 |16 |32.000/256 (344) |25.600/256 |8/yüksek |
| Standard_DS11 |2 |14 |28 |4 |8000/64 (72) |6400/64 |2/yüksek |
| Standard_DS12 |4 |28 |56 |8 |16.000/128 (144) |12.800/128 |4/yüksek |
| Standard_DS13 |8 |56 |112 |16 |32.000/256 (288) |25.600/256 |8/yüksek |
| Standard_DS14 |16 |112 |224 |32 |64.000/512 (576) |51.200/512 |8/çok yüksek |

MB/sn = 10^6 bayt/saniye ve GiB = 1024^3 bayt.

*DS Serisi VM ile maksimum disk aktarım hızı (IOPS veya MB/sn), ekli disklerin sayısı, boyutu ve bölümleme türüyle sınırlı olabilir.  Ayrıntılar için bkz. [Premium Depolama: Azure sanal makine iş yükleri için yüksek performanslı depolama](../articles/storage/storage-premium-storage.md).

<br>

## <a name="dsv2-series"></a>DSv2 Serisi*
| Boyut | CPU çekirdekleri | Bellek: GiB | Yerel SSD: GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış disk aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Maksimum NIC/Ağ bant genişliği |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1_v2 |1 |3,5 |7 |2 |4000/32 (43) |3200/48 |1 orta |
| Standard_DS2_v2 |2 |7 |14 |4 |8000/64 (86) |6400/96 |2 yüksek |
| Standard_DS3_v2 |4 |14 |28 |8 |16.000/128 (172) |12.800/192 |4 yüksek |
| Standard_DS4_v2 |8 |28 |56 |16 |32.000/256 (344) |25.600/384 |8 yüksek |
| Standard_DS5_v2 |16 |56 |112 |32 |64.000/512 (688) |51.200/768 |8 aşırı yüksek |
| Standard_DS11_v2 |2 |14 |28 |4 |8000/64 (72) |6400/96 |2 yüksek |
| Standard_DS12_v2 |4 |28 |56 |8 |16.000/128 (144) |12.800/192 |4 yüksek |
| Standard_DS13_v2 |8 |56 |112 |16 |32.000/256 (288) |25.600/384 |8 yüksek |
| Standard_DS14_v2 |16 |112 |224 |32 |64.000/512 (576) |51.200/768 |8 aşırı yüksek |
| Standard_DS15_v2*** |20 |140 |280 |40 |80.000/640 (720) |64.000/960 |8 aşırı yüksek** |

MB/sn = 10^6 bayt/saniye ve GiB = 1024^3 bayt.

*DSv2 Serisi VM ile maksimum disk aktarım hızı (IOPS veya MB/sn), ekli disklerin sayısı, boyutu ve bölümleme türüyle sınırlı olabilir.  Ayrıntılar için bkz. [Premium Depolama: Azure sanal makine iş yükleri için yüksek performanslı depolama](../articles/storage/storage-premium-storage.md).

**Bazı bölgelerde Standard_DS15_v2 boyutuyla hızlandırılmış ağ iletişimi kullanılabilir. Bu özelliğin kullanım durumu ve şekli hakkında daha fazla bilgi için bkz. [Hızlandırılmış Ağ İletişimi Önizlemede](https://azure.microsoft.com/updates/accelerated-networking-in-preview/) ve [Sanal makine için Hızlandırılmış Ağ İletişimi](../articles/virtual-network/virtual-network-accelerated-networking-powershell.md).

***Örnek, tek bir müşteriye özel donanımla yalıtılmıştır.
<br>

## <a name="f-series"></a>F Serisi
| Boyut | CPU çekirdekleri | Bellek: GiB | Yerel SSD: GiB | Maksimum veri diskleri | Maksimum diski aktarım hızı: IOPS | Maksimum NIC/Ağ bant genişliği |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_F1 |1 |2 |16 |2 |2x500 |1/orta |
| Standard_F2 |2 |4 |32 |4 |4x500 |2/yüksek |
| Standard_F4 |4 |8 |64 |8 |8x500 |4/yüksek |
| Standard_F8 |8 |16 |128 |16 |16x500 |8/yüksek |
| Standard_F16 |16 |32 |256 |32 |32x500 |8/aşırı yüksek |

<br>

## <a name="fs-series"></a>Fs Serisi*
| Boyut | CPU çekirdekleri | Bellek: GiB | Yerel SSD: GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış disk aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Maksimum NIC/Ağ bant genişliği |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_F1s |1 |2 |4 |2 |4000/32 (12) |3200/48 |1/orta |
| Standard_F2s |2 |4 |8 |4 |8000/64 (24) |6400/96 |2/yüksek |
| Standard_F4s |4 |8 |16 |8 |16.000/128 (48) |12.800/192 |4/yüksek |
| Standard_F8s |8 |16 |32 |16 |32.000/256 (96) |25.600/384 |8/yüksek |
| Standard_F16s |16 |32 |64 |32 |64.000/512 (192) |51.200/768 |8/aşırı yüksek |

MB/sn = 10^6 bayt/saniye ve GiB = 1024^3 bayt.

*Fs Serisi VM ile maksimum disk aktarım hızı (IOPS veya MB/sn), ekli disklerin sayısı, boyutu ve bölümleme türüyle sınırlı olabilir.  Ayrıntılar için bkz. [Premium Depolama: Azure sanal makine iş yükleri için yüksek performanslı depolama](../articles/storage/storage-premium-storage.md).

<br>

## <a name="g-series"></a>G Serisi
| Boyut | CPU çekirdekleri | Bellek: GiB | Yerel SSD: GiB | Maksimum veri diskleri | Maksimum diski aktarım hızı: IOPS | Maksimum NIC/Ağ bant genişliği |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_G1 |2 |28 |384 |4 |4x500 |1/yüksek |
| Standard_G2 |4 |56 |768 |8 |8x500 |2/yüksek |
| Standard_G3 |8 |112 |1536 |16 |16x500 |4/çok yüksek |
| Standard_G4 |16 |224 |3072 |32 |32x500 |8/aşırı yüksek |
| Standard_G5* |32 |448 |6144 |64 |64x500 |8/aşırı yüksek |

*Örnek, tek bir müşteriye özel donanımla yalıtılmıştır.
<br>

## <a name="gs-series"></a>GS Serisi*
| Boyut | CPU çekirdekleri | Bellek: GiB | Yerel SSD: GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış disk aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Maksimum NIC/Ağ bant genişliği |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_GS1 |2 |28 |56 |4 |10.000/100 (264) |5000/125 |1/yüksek |
| Standard_GS2 |4 |56 |112 |8 |20.000/200 (528) |10.000/250 |2/Yüksek |
| Standard_GS3 |8 |112 |224 |16 |40.000/400 (1056) |20.000/500 |4/çok yüksek |
| Standard_GS4 |16 |224 |448 |32 |80.000/800 (2112) |40.000/1000 |8/aşırı yüksek |
| Standard_GS5** |32 |448 |896 |64 |160.000/1600 (4224) |80.000/2000 |8/aşırı yüksek |

MB/sn = 10^6 bayt/saniye ve GiB = 1024^3 bayt.

*GS Serisi VM ile maksimum disk aktarım hızı (IOPS veya MB/sn), ekli disklerin sayısı, boyutu ve bölümleme türüyle sınırlı olabilir. 

**Örnek, tek bir müşteriye özel donanımla yalıtılmıştır.
<br>

## <a name="h-series"></a>H Serisi
Azure H Serisi sanal makineler, moleküler modelleme ve hesaplamalı akışkanlar dinamiği gibi üst düzey işlem hesaplama gereksinimlerine hitap eden yeni nesil yüksek performanslı bilgi işlem VM'leridir. Intel Haswell E5-2667 V3 işlemci teknolojisini kullanan bu 8 ve 16 çekirdekli VM'ler, DDR4 belleğe ve yerel SSD tabanlı depolamaya sahiptir. 

H Serisi önemli miktarda CPU gücünün yanı sıra, FDR InfiniBand ile düşük gecikmeli RDMA ağ iletişimi için farklı seçeneklere ek olarak yoğun bellek kullanımlı işlem gereksinimlerini için çok sayıda bellek yapılandırması sunar.

Bilgi ve bu boyutları kullanırken dikkat edilmesi gereken noktalar için bkz. [H Serisi ve yoğun işlem gücü kullanımlı A Serisi VM'ler hakkında](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

| Boyut | CPU çekirdekleri | Bellek: GiB | Yerel SSD: GiB | Maksimum veri diskleri | Maksimum diski aktarım hızı: IOPS | Maksimum NIC/Ağ bant genişliği |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 |8 |56 |1000 |16 |16x500 |2/yüksek |
| Standard_H16 |16 |112 |2000 |32 |32x500 |4/çok yüksek |
| Standard_H8m |8 |112 |1000 |16 |16x500 |2/yüksek |
| Standard_H16m |16 |224 |2000 |32 |32x500 |4/çok yüksek |
| Standard_H16r* |16 |112 |2000 |32 |32x500 |4/çok yüksek |
| Standard_H16mr* |16 |224 |2000 |32 |32x500 |4/çok yüksek |

*RDMA özellikli

<br>


## <a name="ls-series"></a>Ls serisi 

Ls serisi NoSQL veritabanları (örneğin Cassandra, MongoDB, Cloudera ve Redis) gibi düşük gecikme süresine sahip yerel depolama alanları gerektiren iş yükleri için optimize edilmiştir. Ls serisi, [Intel® Xeon İşlemci E5 v3 ailesi](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html) ile 32’ye kadar CPU çekirdeği kullanım olanağı sunar. Bu, G/GS serisi ile aynı CPU performansı sunar ve her CPU çekirdeği başına 8 GiB bellek içerir.  

 
| Boyut          | CPU çekirdekleri | Bellek: GiB | Yerel SSD: GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış disk aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Maksimum NIC/Ağ bant genişliği | 
|---------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standart_L4s  | 4    | 32   | 678   | 8              | Yok / Yok (0)          | 5000/125                               | 2/yüksek       | 
| Standart_L8s  | 8    | 64   | 1,388 | 16             | Yok / Yok (0)          | 10.000/250                              | 4/çok yüksek  | 
| Standart_L16s | 16   | 128  | 2,807 | 32             | Yok / Yok (0)          | 20.000/500                              | 8/aşırı yüksek | 
| Standart_L32s | 32   | 256  | 5,630 | 64             | Yok / Yok (0)          | 40.000/1000                            | 8/aşırı yüksek | 
 
MB/sn = 10^6 bayt/saniye ve GiB = 1024^3 bayt. 



## <a name="n-series"></a>N Serisi
NC ve NV boyutları, GPU özellikli örnekler olarak da bilinir. NVIDIA GPU kartlarına sahip bu özel amaçlı sanal makineler, farklı senaryolar ve kullanım amaçları için iyileştirilmiştir. NV boyutları uzaktan görselleştirme, içerik akışı, oyun ve kodlamanın yanı sıra OpenGL ve DirectX gibi çerçeveleri kullanan VDI senaryoları için tasarlanmış ve iyileştirilmiştir. NC boyutları ise CUDA ve OpenCL tabanlı uygulama ve simülasyonlar gibi yoğun işlem ve ağ kullanımlı uygulamalar ve algoritmalar için iyileştirilmiştir. 


### <a name="nv-instances"></a>NV Örnekleri
NVIDIA'nın Tesla M60 GPU kartının yanı sıra hızlandırılmış masaüstü uygulamaları ve sanal masaüstü düzenleri için NVIDIA GRID özelliğine sahip olan NV örnekleri, müşterilerin verilerini veya simülasyonlarını görselleştirmek için kullanabileceği sistemlerdir. Kullanıcılar yoğun grafik kullanımlı iş akışlarını NV örneklerinde görselleştirerek üstün grafik özelliğine sahip olurken kodlama ve işleme gibi hassas iş yüklerini de tek başına çalıştırabilir. Tesla M60, çift GPU tasarımda 4096 CUDA çekirdeği ve en fazla 36 adet 1080p H.264 akışı sunar. 


| Boyut | CPU çekirdekleri | Bellek: GiB | Yerel SSD: GiB | GPU |
| --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |380 | 1 |
| Standard_NV12 |12 |112 |680 | 2 |
| Standard_NV24 |24 |224 |1440 | 4 |

1 GPU = M60 kartın yarısı.

**Desteklenen işletim sistemleri**

* Windows Server 2016, Windows Server 2012 R2, bkz. [Windows için N Serisi sürücü kurulumu](../articles/virtual-machines/virtual-machines-windows-n-series-driver-setup.md)

### <a name="nc-instances"></a>NC Örnekleri
NC örnekleri, NVIDIA Tesla K80 kartını kullanmaktadır. Kullanıcılar artık enerji keşif uygulamaları, kaza simülasyonları, ışın izlemeli işleme, derin öğrenme ve çok daha fazlası için CUDA yardımıyla verileri çok daha hızlı işleyebilir. Tesla K80, çift GPU tasarımında 4992 CUDA çekirdeği, 2,91 Teraflop'a kadar çift duyarlıklı ve 8,93 Teraflop'a kadar tek duyarlıklı performans sunar.

| Boyut | CPU çekirdekleri | Bellek: GiB | Yerel SSD: GiB | GPU |
| --- | --- | --- | --- | --- |
| Standard_NC6 |6 |56 | 380 | 1 |
| Standard_NC12 |12 |112 | 680 | 2 |
| Standard_NC24 |24 |224 | 1440 | 4 |
| Standard_NC24r* |24 |224 | 1440 | 4 |

1 GPU = K80 kartın yarısı.

*RDMA özellikli

**Desteklenen işletim sistemleri**

* Windows Server 2016, Windows Server 2012 R2, bkz. [Windows için N Serisi sürücü kurulumu](../articles/virtual-machines/virtual-machines-windows-n-series-driver-setup.md)
* Ubuntu 16.04 LTS, bkz. [Linux için N Serisi sürücü kurulumu](../articles/virtual-machines/virtual-machines-linux-n-series-driver-setup.md)

<br>

## <a name="notes-standard-a0---a4-using-cli-and-powershell"></a>Notlar: Standard A0 - A4, CLI ve PowerShell kullanarak
Klasik dağıtım modelinde bazı VM boyutu adları CLI ve PowerShell'dekilerden biraz farklıdır:

* Standard_A0 = Çok Küçük 
* Standard_A1 = Küçük
* Standard_A2 = Orta
* Standard_A3 = Büyük
* Standard_A4 = Çok Büyük

## <a name="next-steps"></a>Sonraki adımlar
* [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../articles/azure-subscription-service-limits.md) hakkında bilgi edinin.
* Yüksek performanslı bilgi işlem (HPC) gibi iş yükleri için [H Serisi ve yoğun işlemci kullanımlı A Serisi VM'ler](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) hakkında daha fazla bilgi edinin.

