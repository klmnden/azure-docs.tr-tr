---
title: MariaDB (MySQL) kümesi Azure'da çalıştırma | Microsoft Docs
description: Bir MariaDB oluşturma + Galera MySQL Azure sanal makinelerinde küme
services: virtual-machines-linux
documentationcenter: ''
author: sabbour
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: d0d21937-7aac-4222-8255-2fdc4f2ea65b
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/15/2015
ms.author: asabbour
ms.openlocfilehash: 2cdc58a827f696d62e6240b90202ee04ce371d07
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39426862"
---
# <a name="mariadb-mysql-cluster-azure-tutorial"></a>MariaDB (MySQL) kümesi: Azure Öğreticisi
> [!IMPORTANT]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Azure Resource Manager](../../../resource-manager-deployment-model.md) ve klasik. Bu makale, klasik dağıtım modelini kapsamaktadır. Microsoft, en yeni dağıtımların Azure Resource Manager modelini kullanmasını önerir.

> [!NOTE]
> MariaDB Kurumsal küme Azure Market'te kullanıma sunuldu. Yeni bir teklif, Azure Resource Manager MariaDB Galera küme otomatik olarak dağıtır. Yeni tekliflerini kullanması gereken [Azure Marketi](https://azure.microsoft.com/marketplace/partners/mariadb/cluster-maxscale/).
>
>

Bu makalede çok yöneticili oluşturma işlemi gösterilmektedir [Galera](http://galeracluster.com/products/) kümesinin [MariaDBs](https://mariadb.org/en/about/) (güçlü, ölçeklenebilir ve güvenilir mongodb'nin MySQL) azure'da yüksek oranda kullanılabilir bir ortamda çalışmak için sanal makineler.

## <a name="architecture-overview"></a>Mimariye genel bakış
Bu makalede, aşağıdaki adımları tamamlamak açıklanır:

- Üç düğümlü bir küme oluşturun.
- Veri diskleri, işletim sistemi diskinden ayırın.
- Veri disklerini IOPS artırmak için RAID-0/şeritli ayarı oluşturun.
- Üç düğümler için yükü dengelemek için Azure Load Balancer'ı kullanın.
- Yinelenen çalışmayı en aza indirmek için MariaDB + Galera içeren bir VM görüntüsü oluşturma ve diğer küme Vm'leri oluşturmak için bunu kullanın.

![Sistem Mimarisi](./media/mariadb-mysql-cluster/Setup.png)

> [!NOTE]
> Bu konuda kullanan [Azure CLI](../../../cli-install-nodejs.md) araçları, bu nedenle bunları indirin ve yönergelere göre Azure aboneliğinize bağlayın emin olun. Azure CLI'da kullanılabilir komutlardan başvuru gerekirse bkz [Azure CLI komut başvurusu](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2). Ayrıca gerekecektir [kimlik doğrulaması için SSH anahtarı oluşturma] ve .pem dosya konumunu not edin.
>
>

## <a name="create-the-template"></a>Şablonu oluşturma
### <a name="infrastructure"></a>Altyapı
1. Kaynakları birlikte tutmak için bir benzeşim grubu oluşturun.

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"
1. Sanal ağ oluşturun.

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet
1. Sunduğumuz tüm diskleri barındırmak için bir depolama hesabı oluşturun. 40'tan fazla yoğun olarak kullanılan diskleri 20.000 IOPS depolama hesabı sınırı ulaşmaktan kaçınmak için aynı depolama hesabında yerleştirebilirsiniz olmamalıdır. Bu durumda, her şeyi kolaylık için aynı hesabı depolayacağınız şekilde, bu sınırın altına iyi demektir.

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster
1. CentOS 7 sanal makine görüntüsünün adı bulun.

        azure vm image list | findstr CentOS
   Çıktı aşağıdakine benzer olacaktır `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`.

   Aşağıdaki adımda bu adı kullanın.
1. VM şablonu oluşturun ve /path/to/key.pem oluşturulan .pem SSH anahtarı depoladığınız yolla değiştirdiğinizden.

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser
1. VM RAID yapılandırması kullanmak için dört 500 GB veri diskleri ekleme.

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd
1. Mariadbhatemplate.cloudapp.net:22 sırasında oluşturulan VM şablonu oturum açmak için SSH kullanın ve özel anahtarınızı kullanarak bağlanın.

### <a name="software"></a>Yazılım
1. Kök alın.

        sudo su

1. RAID destek yükleyin:

    a. Mdadm yükleyin.

              yum install mdadm

    b. RADI0/stripe yapılandırmasını bir EXT4 dosya sistemi ile oluşturun.

              mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
              mdadm --detail --scan >> /etc/mdadm.conf
              mkfs -t ext4 /dev/md0
    c. Bağlama noktası dizini oluşturun.

              mkdir /mnt/data
    d. Yeni oluşturulan bir RAID cihaz UUID'sini alın.

              blkid | grep /dev/md0
    e. /Etc/fstab düzenleyin.

              vi /etc/fstab
    f. Cihaz önceki alınan değer UUID yerine başlatmada otomatik bağlamayı etkinleştirin ekleyin **blkid** komutu.

              UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2
    g. Yeni bölümü bağlayın.

              mount /mnt/data

1. MariaDB yükleyin.

    a. MariaDB.repo dosyası oluşturun.

                vi /etc/yum.repos.d/MariaDB.repo

    b. Depo dosyası aşağıdaki içerikle doldurun:

              [mariadb]
              name = MariaDB
              baseurl = http://yum.mariadb.org/10.0/centos7-amd64
              gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
              gpgcheck=1
    c. Çakışmaları önlemek için mevcut sonek ve mariadb libs kaldırın.

           yum remove postfix mariadb-libs-*
    d. MariaDB Galera ile yükleyin.

           yum install MariaDB-Galera-server MariaDB-client galera

1. MySQL veri dizini RAID blok cihaza taşıyın.

    a. Geçerli MySQL dizin yeni konumuna kopyalayın ve eski dizini kaldırılamıyor.

           cp -avr /var/lib/mysql /mnt/data  
           rm -rf /var/lib/mysql
    b. Buna göre yeni dizini için izinleri ayarlayın.

           chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/

    c. Eski dizin RAID bölüm yeni konumuna işaret eden bir sembolik bağlantısını oluşturun.

           ln -s /mnt/data/mysql /var/lib/mysql

1. Çünkü [SELinux Engellemesi ile küme işlemlerini](http://galeracluster.com/documentation-webpages/configuration.html#selinux), geçerli oturum için devre dışı bırakmak gereklidir. Düzen `/etc/selinux/config` için sonraki yeniden başlatmaları devre dışı bırakmak için.

            setenforce 0

            then editing `/etc/selinux/config` to set `SELINUX=permissive`
1. MySQL çalıştırmaları doğrulayın.

   a. MySQL başlatın.

           service mysql start
   b. MySQL yüklemesini güvenli, kök parola ayarlayın, uzak kök oturum açma devre dışı bırakmak için anonim kullanıcıları kaldırmak ve test veritabanını kaldır.

           mysql_secure_installation
   c. Küme işlemleri için ve isteğe bağlı olarak, uygulamalarınız için veritabanında bir kullanıcı oluşturun.

           mysql -u root -p
           GRANT ALL PRIVILEGES ON *.* TO 'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
           exit

   d. MySQL durdurun.

            service mysql stop
1. Yapılandırma yer tutucu oluşturun.

   a. Küme ayarları için bir yer tutucu oluşturmak için MySQL yapılandırmasını düzenleyin. Değiştirmez **`<Variables>`** veya artık açıklamasını kaldırın. Bu şablondan bir VM oluşturduktan sonra gerçekleşir.

            vi /etc/my.cnf.d/server.cnf
   b. Düzen **[galera]** bölümünde ve onu temizleyin.

   c. Düzen **[mariadb]** bölümü.

           wsrep_provider=/usr/lib64/galera/libgalera_smm.so
           binlog_format=ROW
           wsrep_sst_method=rsync
           bind-address=0.0.0.0 # When set to 0.0.0.0, the server listens to remote connections
           default_storage_engine=InnoDB
           innodb_autoinc_lock_mode=2

           wsrep_sst_auth=cluster:p@ssw0rd # CHANGE: Username and password you created for the SST cluster MySQL user
           #wsrep_cluster_name='mariadbcluster' # CHANGE: Uncomment and set your desired cluster name
           #wsrep_cluster_address="gcomm://mariadb1,mariadb2,mariadb3" # CHANGE: Uncomment and Add all your servers
           #wsrep_node_address='<ServerIP>' # CHANGE: Uncomment and set IP address of this server
           #wsrep_node_name='<NodeName>' # CHANGE: Uncomment and set the node name of this server
1. CentOS 7'de FirewallD kullanarak gerekli güvenlik duvarı bağlantı noktalarını açın.

   * MySQL: `firewall-cmd --zone=public --add-port=3306/tcp --permanent`
   * GALERA: `firewall-cmd --zone=public --add-port=4567/tcp --permanent`
   * GALERA İSTESİ: `firewall-cmd --zone=public --add-port=4568/tcp --permanent`
   * RSYNC: `firewall-cmd --zone=public --add-port=4444/tcp --permanent`
   * Güvenlik Duvarı'nı yeniden yükleyin: `firewall-cmd --reload`

1. Sistem performansı iyileştirin. Daha fazla bilgi için [performans stratejisi ayarlama](optimize-mysql.md).

   a. MySQL yapılandırma dosyasını yeniden düzenleyin.

            vi /etc/my.cnf.d/server.cnf
   b. Düzen **[mariadb]** bölümünde ve aşağıdaki içeriği ekleyin:

   > [!NOTE]
   > Bu ınnodb öneririz\_arabellek\_pool_size olan sanal makinenin bellek yüzde 70. Orta 3,5 GB RAM ile Azure VM için bu örnekte, 2,45 GB olarak ayarlandı.
   >
   >

           innodb_buffer_pool_size = 2508M # The buffer pool contains buffered data and the index. This is usually set to 70 percent of physical memory.
           innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
           max_connections = 5000 # A larger value will give the server more time to recycle idled connections
           innodb_file_per_table = 1 # Speed up the table space transmission and optimize the debris management performance
           innodb_log_buffer_size = 128M # The log buffer allows transactions to run without having to flush the log to disk before the transactions commit
           innodb_flush_log_at_trx_commit = 2 # The setting of 2 enables the most data integrity and is suitable for Master in MySQL cluster
           query_cache_size = 0
1. MySQL durdurun, kümeye düğüm eklerken kesintiye önlemek için başlangıçta çalışan MySQL hizmeti devre dışı bırakın ve makinenin sağlamasını kaldırma.

        service mysql stop
        chkconfig mysql off
        waagent -deprovision
1. Portal üzerinden bir sanal Makineyi yakalamaya. (Şu anda [#1268 Azure CLI Araçları içindeki sorun](https://github.com/Azure/azure-xplat-cli/issues/1268) bağlı veri diskleri için Azure CLI araçları tarafından yakalanan görüntü yakalamayın olgu açıklanmaktadır.)

    a. Portal üzerinden makineyi kapatın.

    b. Tıklayın **yakalama** ve görüntü adı olarak belirtmeniz **mariadb galera görüntü**. Bir açıklama girin ve "Waagent çalıştırmanız gerekir." denetleme
      
      ![Sanal makine yakalanamadı](./media/mariadb-mysql-cluster/Capture2.PNG)

## <a name="create-the-cluster"></a>Kümeyi oluşturma
Oluşturulan ve yapılandırma ve kümeyi başlatın şablonu içeren üç VM oluşturun.

1. İlk CentOS 7 VM, aşağıdaki bilgileri sağlayarak oluşturulan mariadb galera görüntü görüntüden oluşturun:

 - Sanal ağ adı: mariadbvnet
 - Alt ağ: mariadb
 - Makine boyutu: Orta
 - Bulut hizmeti adı: mariadbha (veya ad istediğiniz mariadbha.cloudapp.net erişilecek)
 - Makine adı: mariadb1
 - Kullanıcı adı: azureuser
 - SSH erişimini: etkin
 - SSH sertifikası .pem dosyasını geçirme ve oluşturulan .pem SSH anahtarı depoladığınız yolla /path/to/key.pem değiştirme.

   > [!NOTE]
   > Aşağıdaki komutları açıklık için birden fazla satırı üzerinden ayrılır, ancak her bir satır olarak girmeniz gerekir.
   >
   >
        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 22
        --vm-name mariadb1
        mariadbha mariadb-galera-image azureuser
1. Mariadbha bulut hizmetine bağlanarak iki daha fazla sanal makine oluşturun. VM adı ve SSH bağlantı noktası aynı bulut hizmetindeki diğer vm'lerle çakışan benzersiz bağlantı noktasına değiştirin.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 23
        --vm-name mariadb2
        --connect mariadbha mariadb-galera-image azureuser
  MariaDB3 için:

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 24
        --vm-name mariadb3
        --connect mariadbha mariadb-galera-image azureuser
1. Her üç VM'lerin iç IP adresi bir sonraki adımda almanız gerekir:

    ![IP adresi alınıyor](./media/mariadb-mysql-cluster/IP.png)
1. Oturum açmak için üç VM ve bunların her biri üzerindeki yapılandırma dosyasını düzenlemek için SSH kullanın.

        sudo vi /etc/my.cnf.d/server.cnf

    Açıklamadan çıkarın **`wsrep_cluster_name`** ve **`wsrep_cluster_address`** kaldırarak **#** satırın başında.
    Ayrıca, değiştirin **`<ServerIP>`** içinde **`wsrep_node_address`** ve **`<NodeName>`** içinde **`wsrep_node_name`** ile Sanal makinenin IP adresi ve name, sırasıyla de bu satırlardaki açıklamayı Kaldır.
1. Küme üzerinde MariaDB1 başlatın ve başlangıçta çalışmasına olanak tanır.

        sudo service mysql bootstrap
        chkconfig mysql on
1. MySQL MariaDB2 ve MariaDB3 başlatın ve başlatma sırasında çalışmasına olanak tanır.

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balance-the-cluster"></a>Yük Dengeleme kümesi
Kümelenmiş VM'ler oluşturduğunuzda, koyarlar farklı hata ve güncelleme etki alanlarında ve bu Azure hiçbir zaman bakım tüm makinelerde aynı anda her şeyi sağlamak clusteravset adlı bir kullanılabilirlik kümesine eklenir. Bu yapılandırma, Azure servis düzeyi sözleşmesi (SLA) desteklenmesi için gereksinimleri karşılar.

Artık üç düğümler arasında istekleri dengelemek için Azure Load Balancer'ı kullanın.

Aşağıdaki komutları, Azure CLI kullanarak makinenizde çalıştırın.

Komut parametreleri yapıdır: `azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

CLI'yı biraz uzun olabilir 15 saniye için yük dengeleyici yoklama aralığı ayarlar. Portalında değiştirme **uç noktaları** herhangi bir VM için.

![Uç noktayı düzenle](./media/mariadb-mysql-cluster/Endpoint.PNG)

Seçin **yük dengeli küme yeniden**.

![Yük dengeli küme yeniden yapılandırın](./media/mariadb-mysql-cluster/Endpoint2.PNG)

Değişiklik **araştırma aralığı** 5 saniye ve değişikliklerinizi kaydedin.

![Değişiklik araştırma aralığı](./media/mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validate-the-cluster"></a>Kümeyi doğrula
Sabit işler gerçekleştirilir. Küme artık şu adresten erişilebilir olmalıdır `mariadbha.cloudapp.net:3306`, sorunsuz ve verimli bir şekilde üç sanal makineler arasında yük dengeleyiciden hem de rota istekleri İsabetleri.

Sık kullandığınız MySQL istemcisinden bağlanmak veya bu kümenin çalıştığını doğrulamak için sanal makinelerin birinden bağlanmak için kullanın.

     mysql -u cluster -h mariadbha.cloudapp.net -p

Ardından bir veritabanı oluşturun ve bazı verilerle doldurmak.

    CREATE DATABASE TestDB;
    USE TestDB;
    CREATE TABLE TestTable (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, value VARCHAR(255));
    INSERT INTO TestTable (value)  VALUES ('Value1');
    INSERT INTO TestTable (value)  VALUES ('Value2');
    SELECT * FROM TestTable;

Aşağıdaki tabloda, oluşturduğunuz veritabanını döndürür:

    +----+--------+
    | id | value  |
    +----+--------+
    |  1 | Value1 |
    |  4 | Value2 |
    +----+--------+
    2 rows in set (0.00 sec)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir üç düğümlü MariaDB oluşturduğunuz + çalışan CentOS 7 Galera yüksek oranda kullanılabilir bir kümede Azure sanal makineleri. Vm'leri, Azure Load Balancer ile yük dengelemesi yapılıyor.

Bakmak isteyebilirsiniz [başka bir şekilde Linux üzerinde MySQL kümeye](mysql-cluster.md) ve yolları [en iyi duruma getirmek ve Azure Linux vm'lerde MySQL performansını test etme](optimize-mysql.md).

<!--Anchors-->
[Architecture overview]:#architecture-overview
[Creating the template]:#creating-the-template
[Creating the cluster]:#creating-the-cluster
[Load balancing the cluster]:#load-balancing-the-cluster
[Validating the cluster]:#validating-the-cluster
[Next steps]:#next-steps

<!--Image references-->

<!--Link references-->
[Galera]:http://galeracluster.com/products/
[MariaDBs]:https://mariadb.org/en/about/
[kimlik doğrulaması için SSH anahtarı oluşturma]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/
[issue #1268 in the Azure CLI]:https://github.com/Azure/azure-xplat-cli/issues/1268
