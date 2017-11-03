---
title: "Bir Oracle veritabanına Azure üzerinde tasarlayıp | Microsoft Docs"
description: "Tasarım ve Azure ortamınızda bir Oracle veritabanına uygulayın."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: v-shiuma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 6/22/2017
ms.author: rclaus
ms.openlocfilehash: c8f858bf249c4b56ad4fe60654ab489676eceb1f
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2017
---
# <a name="design-and-implement-an-oracle-database-in-azure"></a>Bir Oracle veritabanına Azure'da tasarlayıp

## <a name="assumptions"></a>Varsayımlar

- Bir Oracle veritabanına şirket içinden Azure'a geçirmek planlama.
- Oracle AWR raporlarda çeşitli ölçümleri bir anlama sahip.
- Uygulama performansı ve platform kullanımı bir temel bilgilere sahipsiniz.

## <a name="goals"></a>Hedefleri

- Oracle dağıtımınızda Azure en iyi duruma getirme anlayın.
- Bir Oracle veritabanına bir Azure ortamı için seçenekleri ayarlama performans keşfedin.

## <a name="the-differences-between-an-on-premises-and-azure-implementation"></a>Şirket içi arasındaki farklar ve Azure uygulaması 

Şirket içi uygulamalar Azure geçirilirken dikkate almanız gereken bazı önemli noktalar şunlardır. 

Bir önemli fark, bir Azure uygulamasında VM'ler, diskler ve sanal ağlar gibi kaynakları diğer istemciler arasında paylaşılır kaynaklanır. Ayrıca, kaynak gereksinimlerine göre kısıtlanan. (MTBF) başarısız önleme odaklanan yerine Azure daha (MTTR) hatası geri kalan odaklanmıştır.

Aşağıdaki tabloda bazı bir şirket içi uygulama ve Oracle veritabanına Azure uygulaması arasındaki farklar listelenmektedir.

> 
> |  | **Şirket içi uygulama** | **Azure uygulama** |
> | --- | --- | --- |
> | **Ağ** |LAN VE WAN  |SDN (yazılım tanımlı ağ)|
> | **Güvenlik grubu** |IP/bağlantı noktasına kısıtlama araçları |[Ağ güvenlik grubu (NSG)](https://azure.microsoft.com/blog/network-security-groups) |
> | **Esnekliği** |MTBF (hataları arasındaki ortalama süre) |MTTR (kurtarma için ortalama zamanı)|
> | **Planlı bakım** |Düzeltme eki uygulama yükseltme|[Kullanılabilirlik kümeleri](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) (düzeltme eki uygulama/Azure tarafından yönetilen yükseltme) |
> | **Kaynak** |Adanmış  |Diğer istemcilerle paylaşılan|
> | **Bölgeler** |Veri merkezleri |[Bölge çiftleri](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability)|
> | **Depolama** |SAN/fiziksel diskleri |[Azure tarafından yönetilen depolama](https://azure.microsoft.com/pricing/details/managed-disks/?v=17.23h)|
> | **Ölçeklendirme** |Dikey Ölçek |Yatay ölçeklendirme|


### <a name="requirements"></a>Gereksinimler

- Veritabanı boyutu ve büyüme oranını belirler.
- Oracle AWR raporları veya diğer ağ izleme araçları göre tahmin IOPS gereksinimlerini belirleyin.

## <a name="configuration-options"></a>Yapılandırma seçenekleri

Bir Azure ortamı performansını artırmak için ayarlayabilirsiniz dört olası alanları şunlardır:

- Sanal makine boyutu
- Ağ verimliliği
- Disk türleri ve yapılandırmalar
- Disk önbelleği ayarları

### <a name="generate-an-awr-report"></a>Bir AWR raporu oluşturun

Varolan bir Oracle veritabanına varsa ve Azure'a geçirmek planlama, birkaç seçeneğiniz vardır. Ölçümleri (IOPS, MB/sn, GiBs ve benzeri) almak için Oracle AWR rapor çalıştırabilirsiniz. Ardından, toplanan ölçümleri temel VM seçin. Veya benzer bilgi almak için altyapı ekibinizin başvurun.

Karşılaştırabileceğiniz normal ve yoğun saatler iş yükleri sırasında AWR raporunuzu çalıştıran düşünebilirsiniz. Bu raporlara göre ortalama iş yükü veya en fazla iş yükünü dayanarak VM'ler boyutlandırabilirsiniz.

Bir AWR raporu oluşturmak nasıl bir örnek verilmiştir:

```bash
$ sqlplus / as sysdba
SQL> EXEC DBMS_WORKLOAD_REPOSITORY.CREATE_SNAPSHOT;
SQL> @?/rdbms/admin/awrrpt.sql
```

### <a name="key-metrics"></a>Anahtar ölçümleri

AWR raporundan elde edebilirsiniz ölçümler şunlardır:

- Çekirdek toplam sayısı
- CPU saat hızı
- GB cinsinden toplam bellek
- CPU kullanımı
- En yüksek veri aktarım hızı
- G/ç değişiklikleri (okuma/yazma) oranı
- Günlük oranı (MBPs) Yinele
- Ağ verimliliği
- Ağ gecikmesi hızı (düşük/yüksek)
- Veritabanı boyutu GB
- SQL ile alınan bayt sayısı * / istemciye Net

### <a name="virtual-machine-size"></a>Sanal makine boyutu

#### <a name="1-estimate-vm-size-based-on-cpu-memory-and-io-usage-from-the-awr-report"></a>1. CPU, bellek ve g/ç kullanımını AWR raporundan temel tahmin VM boyutu

Bakmak bir sistem performans sorunlarının nerede belirten üst beş zamanlanmış ön plan olayları şeydir.

Örneğin, aşağıdaki diyagramda, günlük dosyası eşitleme üstündeki ' dir. LGWR günlük arabellek Yinele günlük dosyasına yazmaya başlamadan önce gerekli olan bekler sayısını gösterir. Bu sonuçları, daha iyi performanslı depolama veya diskleri gerekli olduğunu gösterir. Ayrıca, diyagram CPU (çekirdekler) sayısı ve bellek miktarını gösterir.

![AWR raporu sayfasının ekran görüntüsü](./media/oracle-design/cpu_memory_info.png)

Aşağıdaki diyagramda, toplam g/ç okuma ve yazma gösterir. 59 okuma GB ve 247.3 rapor süresi sırasında yazılmış GB vardı.

![AWR raporu sayfasının ekran görüntüsü](./media/oracle-design/io_info.png)

#### <a name="2-choose-a-vm"></a>2. Bir VM seçin

AWR rapordan toplanan bilgilere göre sonraki adıma gereksinimlerinizi karşılayan benzer bir boyutu VM'i seçmektir. Kullanılabilir sanal makineleri listesini makalesinde bulabilirsiniz [bellek için iyileştirilmiş](../../linux/sizes-memory.md).

#### <a name="3-fine-tune-the-vm-sizing-with-a-similar-vm-series-based-on-the-acu"></a>3. Üzerinde ACU dayalı benzer bir VM dizisi ile VM boyutlandırma ince ayar yapma

VM seçtikten sonra ACU VM için dikkat edin. Gereksinimlerinize daha iyi uyacak ACU değere göre farklı bir VM seçebilirsiniz. Daha fazla bilgi için bkz: [Azure işlem birimi](https://docs.microsoft.com/azure/virtual-machines/windows/acu).

![ACU birimleri sayfasının ekran görüntüsü](./media/oracle-design/acu_units.png)

### <a name="network-throughput"></a>Ağ verimliliği

Aşağıdaki diyagramda, üretilen iş ve IOPS arasındaki ilişki gösterilmektedir:

![Üretilen iş ekran görüntüsü](./media/oracle-design/throughput.png)

Toplam ağ verimliliği aşağıdaki bilgilere dayanarak tahmin edilen:
- SQL * Net trafiği
- MB/sn x sunucularını (Oracle Data Guard gibi giden akış) sayısı
- Uygulama çoğaltma gibi diğer faktörlere

![Ekran görüntüsü SQL * Net işleme](./media/oracle-design/sqlnet_info.png)

Ağ bant genişliği gereksinimlerine bağlı olarak, aralarından seçim yapabileceğiniz çeşitli ağ geçidi türü vardır. Bunlar, temel, VpnGw ve Azure ExpressRoute içerir. Daha fazla bilgi için bkz: [VPN ağ geçidi fiyatlandırma sayfası](https://azure.microsoft.com/en-us/pricing/details/vpn-gateway/?v=17.23h).

**Öneriler**

- Ağ gecikmesi daha yüksek bir şirket içi dağıtıma karşılaştırılır. Ağ dönüşleri can round önemli ölçüde azaltan performansı artırır.
- Gidiş dönüş azaltmak için aynı sanal makineye yüksek işlemler veya "chatty" uygulamaları yüklü uygulamalar birleştirin.

### <a name="disk-types-and-configurations"></a>Disk türleri ve yapılandırmalar

- *Varsayılan işletim sistemi disk*: kalıcı veri ve önbelleğe alma bu disk türleri sunar. Başlangıçta OS erişimi için optimize ve için tasarlanmamış işlemsel veya veri ambarı (analitik) iş yükleri.

- *Yönetilmeyen diskleri*: Bu disk türleri ile VM disklerinizi karşılık gelen sanal sabit disk (VHD) dosyaları depolamak depolama hesaplarını yönetin. VHD dosyaları, Azure storage hesapları sayfa bloblarını olarak depolanır.

- *Yönetilen diskleri*: Azure VM diskleriniz için kullandığınız depolama hesapları yönetir. Disk türünü (premium veya standart) ve ihtiyacınız diskin boyutunu belirtin. Azure oluşturur ve disk tarafından yönetilir.

- *Premium depolama diskleri*: Bu disk türleri üretim iş yükleri için uygundur. Premium depolama belirli boyutu-serisi VM, DS, DSv2, GS ve F serisi VM'ler gibi iliştirilmiş VM diskleri destekler. Premium disk ile farklı boyutlarda gelir ve 32 GB ile 4.096 GB arasında diskleri arasından seçim yapabilirsiniz. Her disk boyutu, kendi performans özellikleri vardır. Uygulama gereksinimlerinize bağlı olarak, VM için bir veya daha fazla disk ekleyebilirsiniz.

Portaldan yeni bir yönetilen disk oluşturduğunuzda, seçebileceğiniz **hesap türünü** için kullanmak istediğiniz disk türü. Aşağı açılır menüde tüm kullanılabilir diskleri gösterilmiştir aklınızda bulundurun. Belirli bir VM boyutu seçtikten sonra menü yalnızca kullanılabilir premium storage bu VM boyutuna göre SKU'ları gösterir.

![Yönetilen disk sayfasının ekran görüntüsü](./media/oracle-design/premium_disk01.png)

Daha fazla bilgi için bkz: [yüksek performanslı Premium depolama ve VM'ler için yönetilen diskleri](https://docs.microsoft.com/azure/storage/storage-premium-storage).

Depolama alanınızın bir VM üzerinde yapılandırdıktan sonra yüklemek istediğiniz diskleri bir veritabanı oluşturmadan önce test. Gecikme süresi ve Verimlilik açısından g/ç oranı bilmek VM'ler beklenen işleme gecikmesi hedefleri ile destekliyorsa belirlemenize yardımcı olabilir.

Uygulama yükleme, Oracle Orion, Sysbench ve Fio gibi test araçları mevcuttur.

Bir Oracle veritabanına dağıttıktan sonra Yük testi yeniden çalıştırın. Normal ve yoğun saatler iş yükleri başlatın ve sonuçları ortamınızın temel gösterir.

Depolama boyutu yerine IOPS oranı tabanlı depolama boyutu daha önemli olabilir. Birden fazla ile 200 GB depolama gelse de Örneğin, gerekli IOPS 5.000 olmakla birlikte, yalnızca 200 GB ihtiyacınız varsa, hala P30 sınıfı premium disk alabilirsiniz.

IOPS oranı AWR raporundan elde edilebilir. Yinele günlük, fiziksel okuma ve yazma hızı tarafından belirlenir.

![AWR raporu sayfasının ekran görüntüsü](./media/oracle-design/awr_report.png)

Örneğin, Yinele boyutu 12,200,000 11.63 MBPs eşittir saniye başına bayttır.
IOPS 12,200,000 olduğu / 2,358 = 5,174.

G/ç gereksinimlerinin NET görüntüsünü oluşturduktan sonra bu gereksinimleri karşılamak için en uygun sürücüleri bir birleşimini seçebilirsiniz.

**Öneriler**

- Veri tablo alanı için g/ç iş yükü disk sayısı yönetilen depolama veya Oracle ASM kullanarak yayılır.
- G/ç blok boyutu yoğun okuma ve yazma yoğunluklu işlemleri için arttıkça, daha fazla veri diski ekleyin.
- Büyük sıralı işlemler için blok boyutunu artırın.
- G/ç (veri ve dizinler için) azaltmak için veri sıkıştırma kullanın.
- Yinele günlükleri, sistem ve temps ayrı ve farklı veri disklerinde TS geri.
- Varsayılan işletim sistemi diskleri (/ dev/sda) tüm uygulama dosyalarını koymayın. Bu diskleri en iyi duruma getirilmiş olmayan için hızlı VM önyükleme zamanları ve uygulamanız için iyi bir performans sağlamayabilir.

### <a name="disk-cache-settings"></a>Disk önbelleği ayarları

Ana bilgisayar önbelleğe almayı için üç seçenek vardır:

- *Salt okunur*: gelecekteki okuma için tüm istekleri önbelleğe alınır. Tüm yazma işlemlerini doğrudan Azure Blob depolama alanına kalıcıdır.

- *Okuma-yazma*: "İleri okuma" algoritma budur. Okuma ve yazma işlemleri için gelecekteki okuma önbelleğe alınır. Yazma olmayan yazma aracılığıyla yerel önbelleğine ilk kalıcıdır. Yazma aracılığıyla kullandığı için SQL Server için Azure depolama alanına yazma kalıcıdır. Hafif iş yükleri için düşük disk gecikme süresi de sağlar.

- *Hiçbiri* (devre dışı): Bu seçeneği kullanarak, önbellek atlayabilir. Tüm verileri diske transfer ve Azure depolama alanına kalıcı. Bu yöntem, g/ç yoğun iş yükleri için yüksek g/ç hızı sunar. Ayrıca "Maliyet" dikkate gerekir.

**Öneriler**

Üretilen işi en üst düzeye çıkarmak için ile başlamanızı öneririz **hiçbiri** ana bilgisayar önbelleğe almayı için. Premium depolama için dosya sistemi ile bağladığınızda, "engelleri" devre dışı bırakmalısınız olduğunu aklınızda bulundurun **ReadOnly** veya **hiçbiri** seçenekleri. Disklerde UUID DEĞERİNE sahip /etc/fstab dosyasını güncelleştirin.

Daha fazla bilgi için bkz: [Linux VM'ler için Premium depolama](https://docs.microsoft.com/azure/storage/storage-premium-storage#premium-storage-for-linux-vms).

![Yönetilen disk sayfasının ekran görüntüsü](./media/oracle-design/premium_disk02.png)

- Varsayılan işletim sistemi disklerinde kullanmak **okuma/yazma** önbelleğe alma.
- Sistem, TEMP ve geri alma için kullanmak **hiçbiri** önbelleğe alma için.
- VERİLERİN kullanmanız **hiçbiri** önbelleğe alma için. Ancak, veritabanı salt okunur veya okuma açısından yoğun ise, **salt okunur** önbelleğe alma.

Veri diski ayarınız kaydedildikten sonra sürücü işletim sistemi düzeyinde çıkarın ve değişikliği yaptıktan sonra yeniden sürece konağı önbellek ayarı değiştiremezsiniz.


## <a name="security"></a>Güvenlik

Ayarlama ve Azure ortamınıza yapılandırdıktan sonra ağınızın güvenliğini sağlamak için sonraki adım olacaktır. Bazı öneriler şunlardır:

- *NSG İlkesi*: NSG bir alt ağ veya NIC tarafından tanımlanabilir Güvenlik ve uygulama güvenlik duvarları gibi şeyler için zorla yönlendirme denetim erişimi hem alt ağ düzeyinde basittir.

- *Jumpbox*: daha güvenli erişim için Yöneticiler doğrudan uygulama hizmeti veya veritabanına bağlanmalısınız değil. Bir jumpbox, yönetici makine ve Azure kaynakları arasında bir ortam olarak kullanılır.
![Jumpbox topoloji sayfasının ekran görüntüsü](./media/oracle-design/jumpbox.png)

    Yönetici makine jumpbox yalnızca IP kısıtlı erişim sunmaktadır. Jumpbox uygulama ve veritabanı erişimi olmalıdır.

- *Özel ağ* (alt ağlar): daha iyi denetim NSG İlkesi tarafından ayarlanabilir böylece, uygulama hizmeti ve veritabanı farklı alt ağlarda sahip olmasını öneririz.


## <a name="additional-reading"></a>Ek kaynaklar

- [Oracle ASM’yi yapılandırma](configure-oracle-asm.md)
- [Oracle Data Guard yapılandırın](configure-oracle-dataguard.md)
- [Oracle Golden kapısı yapılandırın](configure-oracle-golden-gate.md)
- [Oracle yedekleme ve kurtarma](oracle-backup-recovery.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: yüksek oranda kullanılabilir sanal makineleri oluşturma](../../linux/create-cli-complete.md)
- [VM dağıtımı Azure CLI örnekleri keşfedin](../../linux/cli-samples.md)
