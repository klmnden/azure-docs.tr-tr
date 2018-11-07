---
title: Linux üzerinde MySQL performansını iyileştirme | Microsoft Docs
description: Bir Azure Linux çalıştıran sanal makine üzerinde (VM) çalıştıran MySQL en iyi duruma getirmeyi öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: NingKuang
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 0c1c7fc5-a528-4d84-b65d-2df225f2233f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: ningk
ms.openlocfilehash: 0ba85e82824bc257869d9801f342bd6dbb0402d2
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51247458"
---
# <a name="optimize-mysql-performance-on-azure-linux-vms"></a>Azure Linux vm'lerde MySQL performansını iyileştirme
Hem sanal donanım seçme ve yazılım yapılandırma, Azure üzerinde MySQL performansını etkileyen pek çok etken vardır. Bu makalede, depolama sistemi ve veritabanı yapılandırmalarını ile performansı iyileştirme odaklanır.

> [!IMPORTANT]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Azure Resource Manager](../../../resource-manager-deployment-model.md) ve klasik. Bu makale klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Resource Manager modeli ile Linux VM iyileştirmeleri hakkında daha fazla bilgi için bkz: [azure'da Linux VM'nizi iyileştirme](../optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

## <a name="utilize-raid-on-an-azure-virtual-machine"></a>Bir Azure sanal makinesinde RAID kullanma
Depolama bulut ortamlarında veritabanı performansını etkileyen önemli bir faktördür. Tek bir diske karşılaştırıldığında, RAID eşzamanlılık aracılığıyla daha hızlı erişim sağlar. Daha fazla bilgi için [standart RAID düzeyleri](http://en.wikipedia.org/wiki/Standard_RAID_levels).   

Disk g/ç aktarım hızı ve g/ç yanıt süresi azure'da RAID artırılabilir. Laboratuvar testlerimizin disk g/ç aktarım hızı iki katına ve RAID disk sayısı iki katına (ikisinden dört, dört için sekiz, vs.), g/ç yanıt süresi yarı göre ortalama azaltılabilir gösterir. Bkz: [ek A](#AppendixA) Ayrıntılar için.  

RAID Düzey artırdığınızda, disk g/ç ek olarak, MySQL performansını artırır.  Bkz: [ek B](#AppendixB) Ayrıntılar için.  

Öbek boyutu düşünmek isteyebilirsiniz. Daha büyük öbek boyutu varsa, genel olarak, ek yükü, özellikle büyük yazma işlemleri için daha düşük sahip olursunuz. Ancak, öbek boyutu çok büyük olduğunda RAID yararlanarak engelleyen ek yükü ekleyebilirsiniz. Geçerli varsayılan en genel üretim ortamları için uygun olacak şekilde kanıtlanmış 512 KB boyutudur. Bkz: [ek C](#AppendixC) Ayrıntılar için.   

Farklı sanal makine türleri için ekleyebilirsiniz kaç disklerde sınırı yoktur. Bu sınırlar içinde ayrıntılı [Azure için sanal makine ve bulut hizmeti boyutları](https://msdn.microsoft.com/library/azure/dn197896.aspx). Daha az disklerle RAID ayarlamayı da seçebilirsiniz ancak bu makalede, RAID örneği takip etmek dört bağlı veri diskleri gerekir.  

Bu makalede, MYSQL tarafından yüklenmiş ve yapılandırılmış bir Linux sanal makinesi oluşturmuş ve varsayılır. Başlama ile ilgili daha fazla bilgi için Azure'da MySQL yükleme bakın.  

### <a name="set-up-raid-on-azure"></a>Azure'da RAID ayarlama
Aşağıdaki adımlar, Azure portalını kullanarak azure'da RAID oluşturma işlemi gösterilmektedir. RAID, Windows PowerShell komut dosyalarını kullanarak da ayarlayabilirsiniz.
Bu örnekte biz dört disklerle RAID 0 yapılandıracaksınız.  

#### <a name="add-a-data-disk-to-your-virtual-machine"></a>Sanal makinenize bir veri diski ekleme
Azure portalında panoya gidin ve bir veri diski eklemek istediğiniz sanal makineyi seçin. Bu örnekte, mysqlnode1 sanal makinedir.  

<!--![Virtual machines][1]-->

Tıklayın **diskleri** ve ardından **ekleme yeni**.

![Sanal makine disk ekleme](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-Disks-option.png)

Yeni bir 500 GB disk oluşturun. Emin olun **konak önbelleği tercihi** ayarlanır **hiçbiri**.  İşlemi tamamladığınızda, tıklayın **Tamam**.

![Boş diski kullanıma açın](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-attach-empty-disk.png)


Bu, sanal makinenize boş bir diski ekler. Bu adımı üç, dört veri diskleri için RAID sahip birden fazla kez yineleyin.  

Sanal makine eklenen sürücüleri çekirdek ileti günlüğüne bakarak görebilirsiniz. Örneğin, Ubuntu'da görmek için aşağıdaki komutu kullanın:  

    sudo grep SCSI /var/log/dmesg

#### <a name="create-raid-with-the-additional-disks"></a>RAID ek diskler ile oluşturma
Aşağıdaki adımlarda açıklanmıştır nasıl [Linux'ta yazılım RAID yapılandırma](../configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

> [!NOTE]
> XFS dosya sistemi kullanıyorsanız, RAID oluşturduktan sonra aşağıdaki adımları uygulayın.
>
>

Debian, Ubuntu veya Linux Naneli XFS yüklemek için aşağıdaki komutu kullanın:  

    apt-get -y install xfsprogs  

XFS Fedora, CentOS veya RHEL yüklemek için aşağıdaki komutu kullanın:  

    yum -y install xfsprogs  xfsdump


#### <a name="set-up-a-new-storage-path"></a>Yeni bir depolama birimi yolu ayarlayın
Yeni bir depolama yol ayarlamak için aşağıdaki komutu kullanın:  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

#### <a name="copy-the-original-data-to-the-new-storage-path"></a>Özgün veriler yeni depolama birimi yolu Kopyala
Yeni depolama birimi yolu veri kopyalamak için aşağıdaki komutu kullanın:  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

#### <a name="modify-permissions-so-mysql-can-access-read-and-write-the-data-disk"></a>MySQL erişebilmesi için izinleri (okuma ve yazma) değiştirme veri diski
İzinlerini değiştirmek için aşağıdaki komutu kullanın:  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


## <a name="adjust-the-disk-io-scheduling-algorithm"></a>Disk g/ç zamanlama algoritması Ayarla
Linux, dört türde algoritmaları zamanlama g/ç uygular:  

* NOOP algoritması (Hayır işlem)
* Son tarih algoritması (son)
* Tamamen adil Sıralama algoritması (CFQ)
* Bütçe dönem algoritması (Anticipatory)  

Performansı iyileştirmek için farklı g/ç zamanlayıcılar farklı senaryolar altında seçebilirsiniz. Bir tamamen rastgele erişim ortamında değil performans CFQ ve son tarih algoritmaları arasında önemli bir fark yoktur. MySQL veritabanı ortam kararlılık tarihi ayarlamanızı öneririz. Sıralı g/ç çok fazla varsa, CFQ disk g/ç performansı düşürebilir.   

SSD ve diğer donanım için NOOP veya son varsayılan Zamanlayıcı daha iyi performans elde edebilirsiniz.   

2.5 çekirdek önce varsayılan g/ç zamanlama son algoritmasıdır. Çekirdek 2.6.18 ile başlayarak, varsayılan zamanlama g/ç algoritması CFQ hale geldi.  Sistem çalışırken dinamik olarak bu ayarı değiştirmek ya da bu ayarı kernel önyükleme zaman belirtin.  

Aşağıdaki örnek, denetleyin ve Debian dağıtım ailesindeki NOOP algoritması varsayılan Zamanlayıcı olarak gösterilmiştir.  

### <a name="view-the-current-io-scheduler"></a>Geçerli g/ç Zamanlayıcısını görüntüleyin
Aşağıdaki komutu çalıştırarak Zamanlayıcısı'nı görüntülemek için:  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

Geçerli Zamanlayıcı gösteren çıkış görürsünüz:  

    noop [deadline] cfq


### <a name="change-the-current-device-devsda-of-the-io-scheduling-algorithm"></a>G/ç zamanlama algoritmanın geçerli cihaz (/ dev/sda) değiştirme
Geçerli cihaz değiştirmek için aşağıdaki komutları çalıştırın:  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

> [!NOTE]
> Bu tek başına/dev/sda için ayarlama kullanışlı değildir. Veritabanının bulunduğu tüm veri disklerinde ayarlanmalıdır.  
>
>

Aşağıdaki çıktı, o grub.cfg başarıyla yeniden belirten ve varsayılan Zamanlayıcı için NOOP güncelleştirildiğini görmeniz gerekir:  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

Red Hat dağıtım ailesi için yalnızca aşağıdaki komutu gerekir:

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="configure-system-file-operations-settings"></a>Sistem dosyası operations ayarlarını yapılandırma
Bir en iyi uygulamadır devre dışı bırakmak için *atime* dosya sistemi günlük kaydı özelliği. Atime son dosya erişim zamanı gelmiştir. Bir dosya erişildiğinde, dosya sistemi zaman damgası günlüğe kaydeder. Ancak, bu bilgileri nadiren kullanılır. Hangi genel disk erişim süresi azaltacak gerekmez, bunu devre dışı bırakabilirsiniz.  

Atime günlüğü devre dışı bırakmak için dosya sistemi yapılandırma dosyası /etc/ fstab değiştirmek ve eklemek gereken **noatime** seçeneği.  

Örneğin, aşağıdaki örnekte gösterildiği gibi noatime ekleyerek vim /etc/fstab dosyasını düzenleyin:  

    # CLOUD_IMG: This file was created/modified by the Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add the “noatime” option below to disable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

Ardından, aşağıdaki komutu kullanarak dosya sistemini yeniden bağlayın:  

    mount -o remount /RAID0

Değiştirilen sonucu test edin. Test dosyasını değiştirdiğinizde, erişim zamanı güncelleştirilmez. Aşağıdaki örnekler, kod önce ve sonra değişiklik nasıl göründüğünü gösterir.

Önce:        

![Erişim değişiklikten önce kod][5]

Sonra:

![Erişimi değiştirme sonrasında kod][6]

## <a name="increase-the-maximum-number-of-system-handles-for-high-concurrency"></a>Sistemi işleyicilerini yüksek eşzamanlılık için en fazla sayısını artırın
MySQL yüksek eşzamanlılık veritabanıdır. Eşzamanlı işler varsayılan sayısı, her zaman yeterli değil Linux için 1024'tür. MySQL, yüksek eşzamanlılığı desteklemek için sistem en yüksek eş zamanlı tutamaçlarını artırmak için aşağıdaki adımları kullanın.

### <a name="modify-the-limitsconf-file"></a>Limits.conf dosyasını değiştirme
İzin verilen en fazla eşzamanlı işler artırmak için aşağıdaki dört satırları /etc/security/limits.conf dosyasına ekleyin. 65536 sistemin destekleyebileceği en yüksek sayısı olduğunu unutmayın.   

    * yazılım nofile 65536
    * Sabit nofile 65536
    * yazılım nproc 65536
    * Sabit nproc 65536

### <a name="update-the-system-for-the-new-limits"></a>Yeni sınırlar için sistem güncelleştirmesi
Sistem güncelleştirmek için aşağıdaki komutları çalıştırın:  

    ulimit -SHn 65536
    ulimit -SHu 65536

### <a name="ensure-that-the-limits-are-updated-at-boot-time"></a>Önyükleme sırasında sınırları güncelleştirildiğinden emin olun
Önyükleme sırasında etkili şekilde aşağıdaki başlatma komutları /etc/rc.local dosyasında yerleştirin.  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

## <a name="mysql-database-optimization"></a>MySQL veritabanı en iyi duruma getirme
Azure üzerinde MySQL yapılandırmak için bir şirket içi makinede kullandığınız aynı performans ayarlama stratejiyi kullanabilirsiniz.  

Ana g/ç iyileştirme kurallar şunlardır:   

* Önbellek boyutunu artırın.
* G/ç yanıt süresini azaltın.  

MySQL sunucusunun ayarları iyileştirmek için hem sunucu hem de istemci bilgisayarlar için varsayılan yapılandırma dosyası my.cnf dosyanın güncelleştirebilirsiniz.  

Aşağıdaki yapılandırma öğelerini MySQL performansını etkileyen ana faktörler şunlardır:  

* **innodb_buffer_pool_size**: arabelleğe alınan verileri ve dizin arabellek havuzu içerir. Bu, genellikle fiziksel bellek yüzde 70 ayarlanır.
* **innodb_log_file_size**: yineleme günlük boyutu budur. Yinele günlüklerini yazma işlemleri kilitlenme sonrasında hızlı, güvenilir ve kurtarılabilir olduğundan emin olmak için kullanın. Bu, yazma işlemleri günlüğe kaydetme için yeterince alan sunacak 512 MB ayarlanır.
* **max_connections**: bazen uygulamalar bağlantıları düzgün bir şekilde kapatmayın. Daha büyük bir değer sunucu idled bağlantıları geri dönüştürmek için daha fazla zaman verir. Bağlantı sayısı 10.000, ancak önerilen en çok 5000.
* **Innodb_file_per_table**: Bu ayar etkinleştirir veya Innodb tablolar ayrı dosyaların depolanacağı yeteneği devre dışı bırakır. Birkaç Gelişmiş yönetim işlemleri verimli bir şekilde uygulanabilir emin olmak için seçeneğini etkinleştirin. Bir performans açısından bakıldığında, bu tablo alanı aktarım hızı ve debris yönetim performansını iyileştirin. Bu seçenek için Önerilen ayar açıktır.</br></br>
Eylem gerekmiyor MySQL 5.6 varsayılan ayar olarak olduğundan. Önceki sürümleri için varsayılan ayarı kapalı'dır. Veriler yüklenmeden önce yalnızca yeni oluşturulan tablolar etkilendiği ayarın değiştirilmesi gerekir.
* **innodb_flush_log_at_trx_commit**: varsayılan değer 1, 0 olarak ayarlayın kapsamlı: ~ 2. Varsayılan değer, tek başına MySQL DB için en uygun seçenektir. Ayar 2'in çoğu veri bütünlüğü sağlar ve ana dalda MySQL kümesi için uygundur. 0 ayarı (daha iyi performans ile bazı durumlarda) güvenilirlik etkileyebilir ve MySQL kümesinde bağımlı için uygun olan veri kaybı sağlar.
* **Innodb_log_buffer_size**: işlemleri göndermeden önce diske günlük temizleme gerek kalmadan çalıştırmak işlem günlüğü arabellek sağlar. Ancak, ikili büyük nesne veya metin alanını ise, önbellek hızla tüketilir ve sık disk g/ç tetiklenir. Innodb_log_waits durum değişkeninin değilse arabellek boyutunu daha iyi artırmak olan 0.
* **query_cache_size**: projenin başından itibaren devre dışı bırakmak için en iyi seçenektir. Query_cache_size 0 (varsayılan ayar MySQL 5.6 budur) olarak ayarlayın ve sorguları hızlandırmak için diğer yöntemleri kullanabilirsiniz.  

Bkz: [Ek D'ye](#AppendixD) performans önce ve sonra iyileştirme bir karşılaştırması.

## <a name="turn-on-the-mysql-slow-query-log-for-analyzing-the-performance-bottleneck"></a>Performans sorunu çözümlemek için MySQL yavaş sorgu günlüğü Aç
MySQL yavaş sorgu günlüğü için MySQL yavaş sorgu belirlemenize yardımcı olabilir. MySQL yavaş sorgu günlüğü etkinleştirdikten sonra MySQL araçlarını gibi kullanabileceğiniz **mysqldumpslow** performans sorunu tanımlamak için.  

Varsayılan olarak, bu etkin değil. Yavaş sorgu oturum kapatma bazı CPU kaynaklarını tüketebilir. Bu geçici olarak performans sorunlarını gidermek için etkinleştirmenizi öneririz. Yavaş sorgu oturum açmak için:

1. My.cnf dosyanın sonuna aşağıdaki satırları ekleyerek değiştirin:

        long_query_time = 2
        slow_query_log = 1
        slow_query_log_file = /RAID0/mysql/mysql-slow.log

2. MySQL sunucusunu yeniden başlatın.

        service  mysql  restart

3. Kullanarak ayarı etkin sürüyor olup olmadığını denetleyin **Göster** komutu.

![Yavaş-query-log ON][7]   

![Yavaş-query-log sonuçları][8]

Bu örnekte, yavaş sorgu özelliği açık durumda olduğunu görebilirsiniz. Ardından **mysqldumpslow** performans sorunlarını belirleyin ve dizinleri ekleme gibi performansı en iyi duruma getirmek için aracı.

## <a name="appendices"></a>Ekler
Hedeflenen bir laboratuar ortamında üretilen örnek performans test verileri aşağıda verilmiştir. Genel arka plan üzerinde performans veri eğilimi yaklaşımları ayarlama farklı performans ile sağlarlar. Sonuçları farklı ortamı veya ürün sürümlerinde değişebilir.

### <a name="AppendixA"></a>Ek A  
**Farklı RAID düzeyleri ile disk performansı (IOPS)**

![Farklı RAID düzeyleri ile disk IOPS][9]

**Test komutları**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

> [!NOTE]
> Bu test iş yükünü RAID sayısı üst sınırı erişmeye 64 iş parçacığı kullanır.
>
>

### <a name="AppendixB"></a>Ek B  
**Farklı RAID düzeyleri ile MySQL performans (performans) karşılaştırması**   
(XFS dosya sistemi)

![Farklı RAID düzeyleri ile MySQL performans karşılaştırması][10]  
![Farklı RAID düzeyleri ile MySQL performans karşılaştırması][11]

**Test komutları**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

**Farklı RAID düzeyleri ile MySQL performans (OLTP) karşılaştırması**  
![Farklı RAID düzeyleri ile MySQL performans (OLTP) karşılaştırması][12]

**Test komutları**

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

### <a name="AppendixC"></a>Ek C   
**Farklı öbek boyutları için disk performansı (IOPS) karşılaştırması**  
(XFS dosya sistemi)

![][13]

**Test komutları**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

Bu test için kullanılan dosya boyutları 30 GB ile 1 GB, sırasıyla, RAID 0 (4 disk) ile XFS dosya sistemi.

### <a name="AppendixD"></a>Ek D  
**MySQL performans (performans) karşılaştırma önce ve sonra en iyi duruma getirme**  
(XFS dosya sistemi)

![MySQL performans (performans) karşılaştırma önce ve sonra en iyi duruma getirme][14]

**Test komutları**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

**Yapılandırma ayarı için varsayılan ve en iyi duruma getirme aşağıdaki gibidir:**

| Parametreler | Varsayılan | İyileştirme |
| --- | --- | --- |
| **innodb_buffer_pool_size** |None |7 GB |
| **innodb_log_file_size** |5 MB |512 MB |
| **max_connections** |100 |5000 |
| **innodb_file_per_table** |0 |1 |
| **innodb_flush_log_at_trx_commit** |1 |2 |
| **innodb_log_buffer_size** |8 MB |128 MB |
| **query_cache_size** |16 MB |0 |

Daha ayrıntılı için [iyileştirme yapılandırma parametrelerini](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html), başvurmak [MySQL resmi yönergeleri](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method).  

  **Test ortamı**  

| Donanım | Ayrıntılar |
| --- | --- |
| CPU |AMD Opteron(tm) işlemci 4171 HE / 4 çekirdek |
| Bellek |14 GB |
| Disk |10 GB/disk |
| İşletim Sistemi |14.04.1 ubuntu LTS |

[1]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-01.png
[2]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-02.png
[3]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-03.png
[4]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-04.png
[5]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-05.png
[6]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-06.png
[7]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-07.png
[8]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-08.png
[9]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-09.png
[10]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-10.png
[11]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-11.png
[12]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-12.png
[13]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-13.png
[14]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-14.png

