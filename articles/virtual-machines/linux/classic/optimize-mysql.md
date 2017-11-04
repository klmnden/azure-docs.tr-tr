---
title: "Linux MySQL performansı en iyi duruma getirme | Microsoft Docs"
description: "Bir Azure Linux çalıştıran sanal makine üzerinde (VM) çalıştıran MySQL en iyi duruma getirme hakkında bilgi edinin."
services: virtual-machines-linux
documentationcenter: 
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0c1c7fc5-a528-4d84-b65d-2df225f2233f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: ningk
ms.openlocfilehash: 8f2ec884fa98e989448ac11675e71f39aa21fa7f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="optimize-mysql-performance-on-azure-linux-vms"></a>Azure Linux VM'ler MySQL performansı en iyi duruma getirme
Hem sanal donanım seçimi ve yazılım yapılandırmasını Azure üzerinde MySQL performansı etkileyen pek çok etken vardır. Bu makalede, depolama, sistem ve veritabanı yapılandırmalarını en iyi duruma getirme performans odaklanır.

> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Azure Resource Manager](../../../resource-manager-deployment-model.md) ve klasik. Bu makale klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Resource Manager modeli ile Linux VM en iyi duruma getirme hakkında daha fazla bilgi için bkz: [Linux VM'NİZDE Azure ile ilgili en iyi duruma getirme](../optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="utilize-raid-on-an-azure-virtual-machine"></a>Bir Azure sanal makinesi üzerinde RAID kullanma
Depolama bulut ortamlarında veritabanı performansını etkileyen önemli bir faktördür. Tek bir diske ile karşılaştırıldığında, RAID eşzamanlılık üzerinden daha hızlı erişim sağlar. Daha fazla bilgi için bkz: [standart RAID düzeyleri](http://en.wikipedia.org/wiki/Standard_RAID_levels).   

Disk g/ç işleme ve g/ç yanıt süresi Azure RAID artırılabilir. Bizim Laboratuvar testleri disk g/ç işleme iki katına ve RAID disk sayısı (iki'den dört, dört-sekiz, vs.) iki katına, g/ç yanıt süresi tarafından yarım ortalama azaltılabilir gösterir. Bkz: [ek A](#AppendixA) Ayrıntılar için.  

RAID Düzey artırın, disk g/ç yanı sıra MySQL performansı artırır.  Bkz: [ek B](#AppendixB) Ayrıntılar için.  

Öbek boyutu düşünmek isteyebilirsiniz. Daha büyük bir öbek boyutu varsa, genel olarak, ek yükü, özellikle büyük yazmalar için daha düşük sahip olursunuz. Ancak, öbek boyutu çok büyük olduğunda RAID yararlanarak engeller ek yükü ekleyebilirsiniz. Geçerli varsayılan en genel üretim ortamları için en uygun olacak şekilde kanıtlanmış 512 KB boyutudur. Bkz: [ek C](#AppendixC) Ayrıntılar için.   

Başka bir sanal makine türleri için ekleyebilirsiniz kaç disklerde sınırları vardır. Bu sınırlar içinde ayrıntılı [Azure için sanal makine ve bulut hizmeti boyutları](http://msdn.microsoft.com/library/azure/dn197896.aspx). Daha az disklerle RAID ayarlamak seçebilmenize rağmen bu makalede, RAID örnek izlemeniz gereken dört eklenen veri disklerini gerekir.  

Bu makalede, Linux sanal makine oluşturmuş ve MYSQL yüklenmiş ve yapılandırılmış varsayar. Kullanmaya başlama hakkında daha fazla bilgi için Azure üzerinde MySQL yüklemek bkz.  

### <a name="set-up-raid-on-azure"></a>Azure üzerinde RAID ayarlama
Aşağıdaki adımlar, Azure portal kullanarak azure'da RAID oluşturulacağını gösterir. Windows PowerShell komut dosyalarını kullanarak RAID de ayarlayabilirsiniz.
Bu örnekte, şu dört disklerle RAID 0 yapılandırır.  

#### <a name="add-a-data-disk-to-your-virtual-machine"></a>Sanal makinenize bir veri diski Ekle
Azure portalında panoya gidin ve bir veri diski eklemek istediğiniz sanal makineyi seçin. Bu örnekte, sanal makine mysqlnode1 bulunuyor.  

<!--![Virtual machines][1]-->

Tıklatın **diskleri** ve ardından **Attach yeni**.

![Sanal makineler disk ekleme](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-Disks-option.png)

Yeni bir 500 GB disk oluşturun. Olduğundan emin olun **konak önbelleği tercihi** ayarlanır **hiçbiri**.  İşiniz bittiğinde tıklatın **Tamam**.

![Boş diski kullanıma açın](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-attach-empty-disk.png)


Bu bir boş disk, sanal makineye ekler. Böylece dört veri diskleri için RAID sahip bu üç kez tekrarlayın.  

Çekirdek ileti günlüğüne bakarak, sanal makine eklenen sürücüleri görebilirsiniz. Örneğin, bu üzerinde Ubuntu görmek için aşağıdaki komutu kullanın:  

    sudo grep SCSI /var/log/dmesg

#### <a name="create-raid-with-the-additional-disks"></a>RAID ek disklerle oluşturma
Aşağıdaki adımlarda açıklanmaktadır nasıl [yazılım RAID Linux üzerinde yapılandırma](../configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

> [!NOTE]
> XFS dosya sistemi kullanıyorsanız, RAID oluşturduktan sonra aşağıdaki adımları yürütün.
>
>

Debian, Ubuntu ya da Linux Naneli XFS yüklemek için aşağıdaki komutu kullanın:  

    apt-get -y install xfsprogs  

XFS Fedora, CentOS veya RHEL yüklemek için aşağıdaki komutu kullanın:  

    yum -y install xfsprogs  xfsdump


#### <a name="set-up-a-new-storage-path"></a>Yeni bir depolama yol ayarla
Yeni bir depolama yol ayarlamak için aşağıdaki komutu kullanın:  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

#### <a name="copy-the-original-data-to-the-new-storage-path"></a>Yeni depolama birimi yolu özgün veri kopyalama
Yeni depolama birimi yolu veri kopyalamak için aşağıdaki komutu kullanın:  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

#### <a name="modify-permissions-so-mysql-can-access-read-and-write-the-data-disk"></a>MySQL erişebilmesi için (okuma ve yazma) değiştirme izinleri veri diski
İzinlerini değiştirmek için aşağıdaki komutu kullanın:  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


## <a name="adjust-the-disk-io-scheduling-algorithm"></a>Disk g/ç zamanlama algoritması Ayarla
Linux algoritmaları zamanlama g/ç dört tür uygular:  

* SEKMEYİ algoritması (No işlem)
* Son tarih algoritması (son)
* Tamamen Orta Sıralama algoritması (CFQ)
* Bütçe dönem algoritması (Anticipatory)  

Performansı iyileştirmek için farklı g/ç zamanlayıcılar farklı senaryolar altında seçebilirsiniz. Tamamen rastgele erişim ortamında değil performans için CFQ ve son tarih algoritmaları arasında önemli bir fark yoktur. MySQL veritabanı ortamı kararlılık için son tarihi ayarlamanızı öneririz. Çok sayıda sıralı g/ç ise CFQ disk g/ç performansını düşürebilir.   

SSD ve diğer donanım için SEKMEYİ veya son tarih varsayılan Zamanlayıcı daha iyi performans elde edebilirsiniz.   

2.5 çekirdek önce varsayılan g/ç zamanlama son algoritmasıdır. 2.6.18 çekirdek ile başlayarak, CFQ varsayılan g/ç zamanlama algoritma hale geldi.  Bu ayarı çekirdek önyükleme zaman belirtin veya sistem çalışırken dinamik olarak bu ayarı değiştirin.  

Aşağıdaki örnek, denetleyin ve varsayılan Zamanlayıcı Debian dağıtım ailesindeki SEKMEYİ algoritması ayarlamanız gösterilmiştir.  

### <a name="view-the-current-io-scheduler"></a>Geçerli g/ç Zamanlayıcı görüntüleyin
Aşağıdaki komutu çalıştırın Zamanlayıcısı'nı görüntülemek için:  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

Geçerli Zamanlayıcı gösteren çıktı görürsünüz:  

    noop [deadline] cfq


### <a name="change-the-current-device-devsda-of-the-io-scheduling-algorithm"></a>G/ç zamanlama algoritması geçerli cihaz (/ dev/sda) değiştirme
Geçerli cihaz değiştirmek için aşağıdaki komutları çalıştırın:  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

> [!NOTE]
> Bu/dev/sda için tek başına ayarı kullanışlı değildir. Veritabanının bulunduğu tüm veri disklerde ayarlanmalıdır.  
>
>

Aşağıdaki çıkış, bu grub.cfg başarıyla yeniden belirten ve varsayılan Zamanlayıcı için SEKMEYİ güncelleştirildi görmeniz gerekir:  

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

## <a name="configure-system-file-operations-settings"></a>Sistem dosyası işlemleri ayarlarını yapılandırma
Bir en iyi uygulamadır devre dışı bırakmak için *atime* dosya sistemi günlük kaydı özelliği. Atime son dosya erişim zamanı gelmiştir. Bir dosya erişildiğinde, dosya sistemi zaman damgası günlüğe kaydeder. Ancak, bu bilgileri nadiren kullanılır. Hangi genel disk erişim süresini azaltır, onu gereksiniminiz yoksa devre dışı bırakabilirsiniz.  

Atime günlüğü devre dışı bırakmak için dosya sistemi yapılandırma dosyası /etc/ fstab değiştirmek ve eklemek gereken **noatime** seçeneği.  

Örneğin, aşağıdaki örnekte gösterildiği gibi noatime ekleyerek VIM /etc/fstab dosyasını düzenleyin:  

    # CLOUD_IMG: This file was created/modified by the Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add the “noatime” option below to disable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

Ardından, aşağıdaki komutu kullanarak dosya sistemini yeniden bağlayın:  

    mount -o remount /RAID0

Değiştirilen sonucunu sınayın. Test dosyası değiştirdiğinizde, erişim zamanı güncelleştirilmez. Aşağıdaki örnekler, kodu nasıl önce ve sonra değişiklik göründüğünü gösterir.

Önce:        

![Erişim değişiklikten önce kod][5]

Sonra:

![Erişimi değiştirme sonrasında kod][6]

## <a name="increase-the-maximum-number-of-system-handles-for-high-concurrency"></a>Sistem tanıtıcıları yüksek eşzamanlılık için en fazla sayısını artırın
MySQL yüksek eşzamanlılık veritabanıdır. Eşzamanlı işler varsayılan sayısı, her zaman yeterli olmayan Linux için 1024'tür. Maksimum eşzamanlı tanıtıcıları MySQL yüksek eşzamanlılığı desteklemek için sistemin artırmak için aşağıdaki adımları kullanın.

### <a name="modify-the-limitsconf-file"></a>Limits.conf dosyasını değiştirme
İzin verilen en fazla eş zamanlı tanıtıcıları artırmak için /etc/security/limits.conf dosyasında aşağıdaki dört satırları ekleyin. 65536 sistemin destekleyebileceği en fazla olduğuna dikkat edin.   

    * yazılım nofile 65536
    * Sabit nofile 65536
    * yazılım nproc 65536
    * Sabit nproc 65536

### <a name="update-the-system-for-the-new-limits"></a>Yeni sınırları için sistemi güncelleştirme
Sistemi güncelleştirmek için aşağıdaki komutları çalıştırın:  

    ulimit -SHn 65536
    ulimit -SHu 65536

### <a name="ensure-that-the-limits-are-updated-at-boot-time"></a>Önyükleme sırasında sınırları güncelleştirildiğinden emin olun
Bu önyükleme zamanında etkili şekilde aşağıdaki başlatma komutları /etc/rc.local dosyasında yerleştirin.  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

## <a name="mysql-database-optimization"></a>MySQL veritabanı en iyi duruma getirme
Azure üzerinde MySQL yapılandırmak için bir şirket içi makinede kullandığınız aynı performans ayarlama stratejiyi kullanabilirsiniz.  

Ana g/ç iyileştirme kurallar şunlardır:   

* Önbellek boyutunu artırın.
* G/ç yanıt süresini azaltın.  

MySQL sunucu ayarlarını en iyi duruma getirmek için sunucu ve istemci bilgisayarlar için varsayılan yapılandırma dosyası my.cnf dosya güncelleştirebilirsiniz.  

Aşağıdaki yapılandırma öğelerini MySQL performansı etkileyen ana faktörler şunlardır:  

* **innodb_buffer_pool_size**: arabelleğe alınan veri ve dizin arabellek havuzu içerir. Bu, genellikle fiziksel belleğin % 70 yüzdesi ayarlanır.
* **innodb_log_file_size**: Yinele günlük boyutu budur. Yinele günlüklerini yazma işlemleri hızlı, güvenilir ve kurtarılabilir kilitlenme sonrasında olduğundan emin olmak için kullanın. Bu yazma işlemleri günlüğe kaydetme için yeterince alan verecektir 512 MB ayarlanır.
* **max_connections**: bazen uygulamaları bağlantıları düzgün kapatmayın. Daha büyük bir değer sunucu boşa alınmazsa bağlantıları geri dönüştürmek için daha fazla zaman verir. 10.000 en fazla bağlantı sayısı, ancak önerilen en çok 5000 gelir.
* **Innodb_file_per_table**: Bu ayar etkinleştirir veya InnoDB tabloları ayrı dosyaların depolanacağı yeteneği devre dışı bırakır. Birçok gelişmiş yönetim işlemleri verimli bir şekilde uygulanabilir emin olmak için seçeneğini açın. Bir performans açısından bakıldığında, bu tablo alanı aktarım hızı ve debris yönetim performansı en iyi duruma getirme. Bu seçenek için Önerilen ayar açık'tır.</br></br>
Hiçbir eylem gerekli olacak şekilde MySQL 5.6 ON, varsayılandır. Önceki sürümler için varsayılan ayar kapalı'dır. Veriler yüklenmeden önce yalnızca yeni oluşturulan tabloları etkilendiği ayarı değiştirilmelidir.
* **innodb_flush_log_at_trx_commit**: varsayılan değer 0 olarak ayarlayın kapsamıyla 1'dir ~ 2. Varsayılan değer, tek başına MySQL veritabanı için en uygun seçenektir. Ayar 2 çoğu veri bütünlüğü sağlar ve MySQL kümesi yöneticisinde için uygundur. 0 ayarı güvenilirlik (daha iyi performans ile bazı durumlarda) etkileyebilir ve MySQL kümedeki ikincil için uygun veri kaybı sağlar.
* **Innodb_log_buffer_size**: işlemleri yürütmek önce diske günlük temizleme gerek kalmadan çalıştırmak işlemleri günlük arabellek sağlar. Ancak, ikili büyük nesne veya metin alanını ise, önbellek hızla dolacaktır ve sık disk g/ç tetiklenir. Innodb_log_waits durumu değişken değilse arabellek boyutunu daha iyi artırmak olan 0.
* **query_cache_size**: projenin başından itibaren devre dışı bırakmak için en iyi seçenektir. Query_cache_size 0 (varsayılan MySQL 5.6 ayar budur) olarak ayarlayın ve sorguları hızlandırmak için diğer yöntemleri kullanabilirsiniz.  

Bkz: [ek D](#AppendixD) önce ve sonra en iyi duruma getirme performans karşılaştırması.

## <a name="turn-on-the-mysql-slow-query-log-for-analyzing-the-performance-bottleneck"></a>Performans düşüklüğü çözümlemek için MySQL yavaş sorgu oturum aç
MySQL yavaş sorgu günlüğü yavaş sorgular için MySQL belirlemenize yardımcı olabilir. MySQL yavaş sorgu günlüğü etkinleştirdikten sonra MySQL araçları gibi kullanabilirsiniz **mysqldumpslow** performans düşüklüğü tanımlamak için.  

Varsayılan olarak, bu etkin değil. Yavaş sorgu oturum kapatma bazı CPU kaynaklarını tüketebilir. Bu geçici olarak performans sorunları gidermek için etkinleştirmenizi öneririz. Yavaş sorgu oturum açmak için:

1. My.cnf dosyanın sonuna şu satırları ekleyerek değiştirin:

        long_query_time = 2
        slow_query_log = 1
        slow_query_log_file = /RAID0/mysql/mysql-slow.log

2. MySQL sunucusunu yeniden başlatın.

        service  mysql  restart

3. Kullanarak ayarı etkisi sürüyor olup olmadığını denetleyin **Göster** komutu.

![Yavaş-query-log ON][7]   

![Yavaş-query-günlük sonuçları][8]

Bu örnekte, yavaş sorgu özelliği açık durumda olduğunu görebilirsiniz. Daha sonra **mysqldumpslow** performans sorunları belirlemek ve dizinleri ekleme gibi performansını iyileştirmek için aracı.

## <a name="appendices"></a>Ekler
Aşağıdaki örnek performansı test verileri hedeflenen laboratuvar ortamında üretilen geçerlidir. Genel performans verileri eğilim farklı performans yaklaşımlar ayarlama ile sağlarlar. Sonuçları altında farklı ortamı veya ürün sürümleri değişebilir.

### <a name="AppendixA"></a>Ek A  
**Farklı RAID düzeyleri ile disk performansı (IOPS)**

![IOPS diskle farklı RAID düzeyleri][9]

**Test komutları**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

> [!NOTE]
> Bu test iş yükünü RAID sayısı üst sınırı erişmeye 64 iş parçacığı kullanır.
>
>

### <a name="AppendixB"></a>Ek B  
**Farklı RAID düzeyleri ile MySQL performans (performans) karşılaştırma**   
(XFS dosya sistemi)

![Farklı RAID düzeyleri ile MySQL performans karşılaştırma][10]  
![Farklı RAID düzeyleri ile MySQL performans karşılaştırma][11]

**Test komutları**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

**Farklı RAID düzeyleri ile MySQL performans (OLTP) karşılaştırma**  
![Farklı RAID düzeyleri ile MySQL performans (OLTP) karşılaştırma][12]

**Test komutları**

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

### <a name="AppendixC"></a>Ek C   
**Farklı öbek boyutları için disk performans (IOPS) karşılaştırması**  
(XFS dosya sistemi)

![][13]

**Test komutları**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

Bu test için kullanılan dosya boyutları 30 GB ve 1 GB, sırasıyla, RAID 0 (4 disk) ile XFS dosya sistemi.

### <a name="AppendixD"></a>Ek D  
**MySQL performans (performans) karşılaştırma önce ve sonra en iyi duruma getirme**  
(XFS dosya sistemi)

![MySQL performans (performans) karşılaştırma önce ve sonra en iyi duruma getirme][14]

**Test komutları**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

**Varsayılan ve en iyi duruma getirme için yapılandırma ayarı aşağıdaki gibidir:**

| Parametreler | Varsayılan | İyileştirme |
| --- | --- | --- |
| **innodb_buffer_pool_size** |None |7 GB |
| **innodb_log_file_size** |5 MB |512 MB |
| **max_connections** |100 |5000 |
| **innodb_file_per_table** |0 |1 |
| **innodb_flush_log_at_trx_commit** |1 |2 |
| **innodb_log_buffer_size** |8 MB |128 MB |
| **query_cache_size** |16 MB |0 |

Daha ayrıntılı için [en iyi duruma getirme yapılandırma parametrelerini](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html), başvurmak [MySQL resmi yönergeleri](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method).  

  **Test ortamı**  

| Donanım | Ayrıntılar |
| --- | --- |
| CPU |AMD Opteron(tm) işlemci 4171 HE / 4 çekirdek |
| Bellek |14 GB |
| Disk |10 GB/disk |
| İşletim Sistemi |Ubuntu 14.04.1 LTS |

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

