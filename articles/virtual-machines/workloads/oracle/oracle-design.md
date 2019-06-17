---
title: Bir Oracle veritabanına Azure'da tasarlayıp | Microsoft Docs
description: Tasarlayın ve Azure ortamınızda bir Oracle veritabanına uygulayın.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: romitgirdhar
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/02/2018
ms.author: rogirdh
ms.openlocfilehash: c5a76b9cee8fd6eb09ee4d24c1380202fd17cc6d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60836354"
---
# <a name="design-and-implement-an-oracle-database-in-azure"></a>Azure’da Oracle veritabanı tasarlama ve dağıtma

## <a name="assumptions"></a>Varsayımlar

- Bir Oracle veritabanına şirket içinden Azure'a geçirmeyi planlamanız durumunda.
- Oracle AWR raporları çeşitli ölçümleri'nın bilinmesini var.
- Uygulama performansı ve platform kullanımı temel bir anlayışa sahip.

## <a name="goals"></a>Hedefleri

- Azure'da Oracle dağıtımınızı en iyi duruma getirme hakkında bilgi edinin.
- Performans ayarlama Azure ortamınızda bir Oracle veritabanına seçeneklerini keşfedin.

## <a name="the-differences-between-an-on-premises-and-azure-implementation"></a>Şirket içi arasındaki farklar ve Azure uygulama 

Şirket içi uygulamaları azure'a geçirirken göz önünde bulundurmanız gereken bazı önemli noktalar aşağıda verilmiştir. 

Bir önemli fark, bir Azure uygulamasında, VM'ler, diskler ve sanal ağlar gibi kaynakları diğer istemciler arasında paylaşılır. Ayrıca, gereksinimlerine göre bu kaynakları kısıtlanabilir. Başarısız (MTBF) önleme üzerinde odaklanarak yerine, Azure daha (MTTR) hatası geri kalan odaklanır.

Aşağıdaki tabloda şirket içi uygulama ve bir Oracle veritabanına Azure uygulaması arasındaki farklılıklar listelenmektedir.

> 
> |  | **Şirket içi uygulama** | **Azure uygulaması** |
> | --- | --- | --- |
> | **Ağ** |LAN VE WAN  |SDN (yazılım tanımlı ağ iletişimi)|
> | **Güvenlik grubu** |IP/bağlantı noktası kısıtlama araçları |[Ağ güvenlik grubu (NSG)](https://azure.microsoft.com/blog/network-security-groups) |
> | **Esnekliği** |MTBF (hatalar arasındaki ortalama süreyi) |MTTR (kurtarma için ortalama zamanı)|
> | **Planlı bakım** |Düzeltme eki uygulama yükseltmeleri|[Kullanılabilirlik kümeleri](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) (düzeltme eki uygulama/Azure tarafından yönetilen yükseltme) |
> | **Kaynak** |Adanmış  |Diğer istemcilerle paylaşılan|
> | **Bölgeler** |Veri merkezleri |[Bölge çiftleri](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability)|
> | **Depolama** |SAN/fiziksel diskleri |[Azure tarafından yönetilen depolama](https://azure.microsoft.com/pricing/details/managed-disks/?v=17.23h)|
> | **Ölçeklendirme** |Dikey ölçeklendirme |Yatay ölçeklendirme|


### <a name="requirements"></a>Gereksinimler

- Veritabanı boyutu ve büyüme oranını belirler.
- Oracle AWR raporları veya diğer ağ izleme araçları göre tahmin IOPS gereksinimlerini belirleyin.

## <a name="configuration-options"></a>Yapılandırma seçenekleri

Azure ortamınızda performansını ayarlamak, olası dört alan vardır:

- Sanal makine boyutu
- Ağ aktarım hızı
- Disk türleri ve yapılandırmaları
- Disk önbellek ayarları

### <a name="generate-an-awr-report"></a>AWR rapor oluşturma

Varolan bir Oracle veritabanına sahip ve Azure'a geçirmeyi planlama, birkaç seçeneğiniz vardır. Ölçümler (IOPS, MB/sn, GiBs ve benzeri) almak için Oracle AWR rapor çalıştırabilirsiniz. Topladığınız ölçümlere göre sanal makine seçin. Veya benzer bilgileri almak için altyapı takımınızın başvurabilirsiniz.

Karşılaştırabilmeniz AWR raporunuz hem normal hem de yoğun saatler iş yükleri sırasında çalışan göz önünde bulundurabilirsiniz. Bu raporlara bağlı olarak, ortalama iş yükü veya en fazla iş yüküne göre sanal makineleri boyutlandırabilirsiniz.

AWR raporu oluşturmak nasıl bir örnek aşağıda verilmiştir:

```bash
$ sqlplus / as sysdba
SQL> EXEC DBMS_WORKLOAD_REPOSITORY.CREATE_SNAPSHOT;
SQL> @?/rdbms/admin/awrrpt.sql
```

### <a name="key-metrics"></a>Ana ölçümler

AWR rapordan edinebilirsiniz ölçümler şunlardır:

- Toplam çekirdek sayısı
- CPU saat hızı
- Toplam bellek (GB)
- CPU kullanımı
- En yüksek veri aktarım hızı
- G/ç değişiklikleri (okuma/yazma) oranı
- Günlük oran (MBPs) Yinele
- Ağ aktarım hızı
- Ağ gecikme oranı (düşük/yüksek)
- Veritabanı boyutu GB
- SQL aracılığıyla alınan bayt sayısı * / istemci için Net

### <a name="virtual-machine-size"></a>Sanal makine boyutu

#### <a name="1-estimate-vm-size-based-on-cpu-memory-and-io-usage-from-the-awr-report"></a>1. CPU, bellek ve g/ç kullanımını AWR rapordan temel tahmin VM boyutu

Sistem darboğazların olduğu yerler belirten en çok beş zamanlanmış ön plan olayları bakmak bir şey var.

Örneğin, aşağıdaki diyagramda, günlük dosya eşitleme en üstünde ' dir. Bu, LGWR günlük arabellek Yinele günlük dosyasına yazmaya başlamadan önce gerekli olan bekler sayısını gösterir. Bu sonuçları, daha iyi performans gösteren depolama veya diskleri gerekli olduğunu gösterir. Ayrıca, diyagram CPU (çekirdekler) sayısı ve bellek miktarını gösterir.

![AWR rapor sayfasının ekran görüntüsü](./media/oracle-design/cpu_memory_info.png)

Aşağıdaki diyagramda, toplam g/ç okuma ve yazma gösterilmektedir. 59 okuma GB ve rapor bir süre boyunca yazılan 247.3 GB vardı.

![AWR rapor sayfasının ekran görüntüsü](./media/oracle-design/io_info.png)

#### <a name="2-choose-a-vm"></a>2. Bir VM seçin

AWR rapordan toplanan bilgilere göre sonraki adıma gereksinimlerinizi karşılayan benzer boyutta bir VM için seçmektir. Kullanılabilir VM'lerin listesini makaleyi bulabilirsiniz [bellek için iyileştirilmiş](../../linux/sizes-memory.md).

#### <a name="3-fine-tune-the-vm-sizing-with-a-similar-vm-series-based-on-the-acu"></a>3. ACU üzerinde temel benzer VM serisi ile VM boyutlandırma ince ayar yapma

VM seçtikten sonra ACU VM için dikkat edin. Gereksinimlerinizi daha iyi uyan ACU değerine göre farklı bir VM seçebilir. Daha fazla bilgi için [Azure işlem birimi](https://docs.microsoft.com/azure/virtual-machines/windows/acu).

![ACU birimleri sayfasının ekran görüntüsü](./media/oracle-design/acu_units.png)

### <a name="network-throughput"></a>Ağ aktarım hızı

Aşağıdaki diyagramda, aktarım hızı ve IOPS arasındaki ilişki gösterilmektedir:

![Aktarım hızının ekran görüntüsü](./media/oracle-design/throughput.png)

Toplam ağ verimi, aşağıdaki bilgilere dayanarak tahmin edilir:
- SQL * Net trafiği
- MB/sn x sunucuları (giden akış Oracle Data Guard gibi) sayısı
- Uygulama çoğaltma gibi diğer faktörlere

![Ekran görüntüsü SQL * Net aktarım hızı](./media/oracle-design/sqlnet_info.png)

Ağ bant genişliği gereksinimlerinize göre aralarından seçim yapabileceğiniz çeşitli ağ geçidi türü vardır. Bunlar, temel, VpnGw ve Azure ExpressRoute içerir. Daha fazla bilgi için [VPN gateway fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/vpn-gateway/?v=17.23h).

**Öneriler**

- Ağ gecikme süresi, daha yüksek bir şirket içi dağıtımına karşılaştırılır. Ağ gidiş gelişlerin can önemli ölçüde azaltan performansı artırır.
- Gidiş dönüş azaltmak için yüksek işlem ya da "geveze" uygulamalar aynı sanal makineye sahip uygulamalar birleştirin.

### <a name="disk-types-and-configurations"></a>Disk türleri ve yapılandırmaları

- *Varsayılan işletim sistemi diskleri*: Bu disk türlerini kalıcı veri ve önbelleğe alma sunar. Başlatma sırasında işletim sistemi erişimi için optimize edilmiş ve için tasarlanmamış işlemsel veya veri ambarı (analiz) iş yükleri.

- *Yönetilmeyen diskler*: Şu disk türleri ile sanal makine disklerinizi karşılık gelen sanal sabit disk (VHD) dosyaları depolayan depolama hesapları yönetin. VHD dosyaları, Azure depolama hesaplarındaki sayfa blobları olarak depolanır.

- *Yönetilen diskler*: Azure sanal makine disklerinizi için kullandığınız depolama hesapları yönetir. Disk türünü (premium veya standart) ve ihtiyacınız olan diskin boyutunu belirtin. Azure, oluşturur ve disk sizin yerinize yönetir.

- *Premium depolama disklerini*: Bu disk türlerini üretim iş yükleri için uygundur. Premium depolama DS, DSv2, GS ve F serisi VM'ler gibi belirli boyut serisi VM'ler iliştirilebilir VM disklerini destekler. Premium disk farklı boyutlarda gelir ve 32 GB ile 4096 GB arasında değişen diskler arasında seçim yapabilirsiniz. Her disk boyutu, kendi performans özellikleri vardır. Uygulama gereksinimlerinize bağlı olarak, sanal makinenizde bir veya daha fazla disk ekleyebilirsiniz.

Portaldan yeni bir yönetilen disk oluşturduğunuzda, seçebileceğiniz **hesap türü** için kullanmak istediğiniz diskin türü. Açılan menüde tüm kullanılabilir diskleri gösterilmiştir aklınızda bulundurun. Belirli bir VM boyutu seçin sonra menü yalnızca kullanılabilir premium depolama, sanal makine boyutuna göre SKU'ları gösterir.

![Yönetilen disk sayfasının ekran görüntüsü](./media/oracle-design/premium_disk01.png)

Bir VM'de depolama alanınızı yapılandırdıktan sonra Yük isteyebilirsiniz diskleri veritabanı oluşturmadan önce test edin. G/ç hızı hem gecikme süresi ve aktarım hızı açısından bilmek, beklenen aktarım hızıyla gecikme hedefleri ile Vm'leri destekliyorsa belirlemenize yardımcı olabilir.

Uygulama yük, Oracle Orion Sysbench ve Fio gibi test etme için araçlar vardır.

Bir Oracle veritabanına dağıttıktan sonra Yük testi yeniden çalıştırın. Normal ve yoğun saatler iş yüklerinizi başlayın ve sonuçlarını ortamınızın taban çizgisini göster.

Depolama boyutu yerine IOPS oranı tabanlı depolama boyutu daha önemli olabilir. Birden fazla ile 200 GB depolama alanı gelse de Örneğin, gereken IOPS 5.000 olmakla yalnızca 200 GB ihtiyacınız olursa, hala P30 sınıfı premium disk alabilirsiniz.

IOPS oranı AWR rapordan elde edilebilir. Yinele günlük, fiziksel okuma ve yazma oranı tarafından belirlenir.

![AWR rapor sayfasının ekran görüntüsü](./media/oracle-design/awr_report.png)

Örneğin, yineleme boyut 12,200,000 11.63 MB/sn olarak eşit olan, saniye başına bayttır.
IOPS 12,200,000 olan / 2,358 = 5,174.

G/ç gereksinimleri NET bir görüntüsünü oluşturduktan sonra bu gereksinimleri karşılamak için en uygun sürücüleri bir birleşimini seçebilirsiniz.

**Öneriler**

- Veri tablo alanı için g/ç iş yükü, yönetilen depolama veya Oracle ASM kullanarak disk sayısı arasında yayılabilir.
- Yoğun okuma ve yazma yoğunluklu işlemleri için g/ç blok boyutu arttıkça, daha fazla veri diski ekleyin.
- Sıralı büyük işlemler için blok boyutunu artırın.
- G/ç (hem veri ve dizin) azaltmak için veri sıkıştırma kullanın.
- Yinele günlükler, sistem ve temps ayırın ve TS ayrı veri disklerini geri alın.
- Hiçbir uygulama dosyasında varsayılan işletim sistemi diskleri (/ dev/sda) koymayın. Bu diskleri için optimize edilmediğinden hızlı sanal makine için önyükleme zamanları ve uygulamanız için iyi bir performans sağlamayabilir.

### <a name="disk-cache-settings"></a>Disk önbellek ayarları

Konak önbelleği için üç seçenek vardır:

- *Salt okunur*: Tüm istekler için gelecekteki okuma önbelleğe alınır. Tüm yazma işlemlerini doğrudan Azure Blob depolama alanına kalıcıdır.

- *Okuma-yazma*: "İleri okuma" algoritması budur. Okuma ve yazma işlemleri için gelecekteki okuma önbelleğe alınır. Yazma olmayan yazma aracılığıyla yerel önbelleğe ilk kalıcıdır. Anında yazma kullandığından SQL Server için Azure depolama alanına yazma kalıcıdır. Ayrıca hafif iş yükleri için düşük disk gecikme sağlar.

- *Hiçbiri* (devre dışı): Bu seçeneği kullanarak, önbellek devre dışı bırakabilir. Tüm veriler diske aktarma ve Azure Depolama'da kalıcı hale. Bu yöntem, g/ç kullanımlı iş yükleri için yüksek g/ç hızı sunar. "İşlem Maliyet" önünde bulundurmanız gerekir.

**Öneriler**

Aktarım hızını en üst düzeye çıkarmak için ile başlamanızı öneririz **hiçbiri** için konak önbelleği. Premium depolama dosya sistemi ile bağladığınızda "engelleri" mamtelemetrydisabled aklınızda bulundurun **salt okunur** veya **hiçbiri** seçenekleri. Disklere UUID'sine sahip /etc/fstab dosyasını güncelleştirin.

![Yönetilen disk sayfasının ekran görüntüsü](./media/oracle-design/premium_disk02.png)

- Varsayılan işletim sistemi disklerinde kullanmak **okuma/yazma** önbelleğe alma.
- Sistem, TEMP ve geri alma için **hiçbiri** önbelleğe alma.
- VERİLERİ için kullanın **hiçbiri** önbelleğe alma. Ancak, veritabanı salt okunur veya okuma açısından yoğun ise, **salt okunur** önbelleğe alma.

Veri diski ayarınızı kaydedildikten sonra sürücü işletim sistemi düzeyinde çıkarın ve değişiklikleri yaptıktan sonra sonra bunu yeniden bağlamak sürece konak önbelleği ayarı değiştiremezsiniz.


## <a name="security"></a>Güvenlik

Ayarlama ve Azure ortamınızda sonra sonraki adım, ağınızın güvenliğini sağlamaktır. Bazı öneriler şunlardır:

- *NSG ilke*: NSG bir alt ağ veya NIC tarafından tanımlanabilir Güvenlik ve uygulama güvenlik duvarları gibi şeyler için zorla yönlendirme erişimi denetlemek alt ağ düzeyinde hem kolaydır.

- *Sıçrama kutusu*: Daha güvenli erişim için Yöneticiler doğrudan uygulama hizmeti veya veritabanı için bağlantı. Bir Sıçrama kutusu, yönetici makine ve Azure kaynakları arasında bir medya olarak kullanılır.
![Sıçrama kutusu topolojisi sayfasının ekran görüntüsü](./media/oracle-design/jumpbox.png)

    IP kısıtlı erişim Sıçrama kutusu yalnızca yönetici makine sunmalıdır. Sıçrama kutusu, uygulama ve veritabanına erişimi olmalıdır.

- *Özel ağ* (alt ağlar): Daha iyi denetim NSG İlkesi tarafından ayarlanabilir bu nedenle, veritabanı ve uygulama hizmeti farklı alt ağlarda olmasını öneririz.


## <a name="additional-reading"></a>Ek okuma

- [Oracle ASM’yi yapılandırma](configure-oracle-asm.md)
- [Oracle Data Guard'ı yapılandırma](configure-oracle-dataguard.md)
- [Oracle Golden ağ geçidi yapılandırma](configure-oracle-golden-gate.md)
- [Oracle yedekleme ve kurtarma](oracle-backup-recovery.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Yüksek oranda kullanılabilir VM'ler oluşturma](../../linux/create-cli-complete.md)
- [VM dağıtımı Azure CLI örneklerini keşfedin](../../linux/cli-samples.md)
