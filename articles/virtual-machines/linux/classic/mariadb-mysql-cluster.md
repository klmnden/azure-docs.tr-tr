---
title: Azure üzerinde bir MariaDB (MySQL) küme çalıştırın | Microsoft Docs
description: Bir MariaDB Oluştur + Galera MySQL Azure sanal makineleri küme
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
ms.openlocfilehash: 4a3eede532345f8628af1722a06531571f01afbf
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="mariadb-mysql-cluster-azure-tutorial"></a>MariaDB (MySQL) küme: Azure Öğreticisi
> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Azure Resource Manager](../../../resource-manager-deployment-model.md) ve klasik. Bu makale, klasik dağıtım modelini kapsamaktadır. Microsoft, en yeni dağıtımların Azure Resource Manager modelini kullanmasını önerir.

> [!NOTE]
> MariaDB Kurumsal küme Azure Marketi'nde kullanıma sunulmuştur. Yeni sunum MariaDB Galera küme üzerinde Azure Resource Manager otomatik olarak dağıtacaktır. Yeni sunum gelen kullanması gereken [Azure Marketi](https://azure.microsoft.com/marketplace/partners/mariadb/cluster-maxscale/).
>
>

Bu makalede, birden çok asıl oluşturma gösterilmektedir [Galera](http://galeracluster.com/products/) kümesinin [MariaDBs](https://mariadb.org/en/about/) (MySQL için güçlü, ölçeklenebilir ve güvenilir içeri kayma yenileme) Azure sanal makinelerde yüksek oranda kullanılabilir bir ortamda çalışmak için.

## <a name="architecture-overview"></a>Mimariye genel bakış
Bu makale, aşağıdaki adımları tamamlayın açıklamaktadır:

- Üç düğümlü bir küme oluşturun.
- Veri diskleri işletim sistemi diski ayırın.
- Veri diskleri IOPS artırmak için RAID-0/şeritli ayarı oluşturun.
- Üç düğümü için yükü dengelemek için Azure yük dengeleyici kullanın.
- Yinelenen iş en aza indirmek için MariaDB + Galera içeren bir VM görüntüsü oluşturun ve başka bir küme sanal makineleri oluşturmak için kullanın.

![Sistem Mimarisi](./media/mariadb-mysql-cluster/Setup.png)

> [!NOTE]
> Bu konuda kullanan [Azure CLI](../../../cli-install-nodejs.md) araçları, böylece bunları indirmek ve yönergelere göre Azure aboneliğinize bağlanmak emin olun. Azure CLI kullanılabilir komutları başvuru gerekirse bkz [Azure CLI komut başvurusu](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2). Ayrıca gerekecektir [kimlik doğrulaması için SSH anahtarı oluşturma] ve .pem dosya konumunu not edin.
>
>

## <a name="create-the-template"></a>Şablonu oluşturma
### <a name="infrastructure"></a>Altyapı
1. Kaynakları birlikte tutmak için bir benzeşim grubu oluşturun.

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"
2. Sanal ağ oluşturun.

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet
3. Bizim tüm diskleri barındırmak için bir depolama hesabı oluşturun. 20.000 IOPS depolama hesabı sınırı basarsa önlemek için aynı depolama hesabında birden fazla 40 yoğun olarak kullanılan diskleri yerleştirebilirsiniz döndürmemelidir. Bu durumda, her şeyi kolaylık sağlaması için aynı hesabı depolayacağınız için o sınırın altına iyi demektir.

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster
4. CentOS 7 sanal makine görüntüsü adını bulun.

        azure vm image list | findstr CentOS
   Çıktı aşağıdakine benzer olacaktır `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`.

   Aşağıdaki adımda bu adı kullanın.
5. VM şablonunu oluşturmak ve /path/to/key.pem oluşturulan .pem SSH anahtarı depolandığı yolu ile değiştirin.

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser
6. VM RAID yapılandırması kullanmak için dört 500 GB veri diskleri iliştirin.

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd
7. Mariadbhatemplate.cloudapp.net:22 sırasında oluşturulan VM şablonu oturum açmak için SSH kullanın ve özel anahtarınızı kullanarak bağlanın.

### <a name="software"></a>Yazılım
1. Kök alın.

        sudo su

2. RAID desteği yükleyin:

    a. Mdadm yükleyin.

              yum install mdadm

    b. RADI0/stripe yapılandırmasını EXT4 dosya sistemi ile oluşturun.

              mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
              mdadm --detail --scan >> /etc/mdadm.conf
              mkfs -t ext4 /dev/md0
    c. Bağlama noktası dizini oluşturun.

              mkdir /mnt/data
    d. Yeni oluşturulan RAID aygıtı UUID'si alın.

              blkid | grep /dev/md0
    e. /Etc/fstab düzenleyin.

              vi /etc/fstab
    f. Otomatik olarak bağlama önceki elde edilen değerle UUID değiştirme başlatmada etkinleştirmek için aygıt ekleme **blkid** komutu.

              UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2
    g. Yeni bölüm bağlayın.

              mount /mnt/data

3. MariaDB yükleyin.

    a. MariaDB.repo dosyası oluşturun.

                vi /etc/yum.repos.d/MariaDB.repo

    b. Depodaki dosya aşağıdaki içerik ile doldurun:

              [mariadb]
              name = MariaDB
              baseurl = http://yum.mariadb.org/10.0/centos7-amd64
              gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
              gpgcheck=1
    c. Çakışmaları önlemek için mevcut sonek ve mariadb kitaplıklar kaldırın.

           yum remove postfix mariadb-libs-*
    d. MariaDB Galera ile yükleyin.

           yum install MariaDB-Galera-server MariaDB-client galera

4. MySQL veri dizini RAID blok cihaza taşıyın.

    a. Geçerli MySQL dizin yeni konumuna kopyalayın ve eski dizini kaldırın.

           cp -avr /var/lib/mysql /mnt/data  
           rm -rf /var/lib/mysql
    b. Yeni dizin izinlerini uygun şekilde ayarlayın.

           chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/

    c. Eski dizin RAID bölüm yeni konumuna işaret eden bir simgesel oluşturun.

           ln -s /mnt/data/mysql /var/lib/mysql

5. Çünkü [SELinux uğratan küme işlemleriyle](http://galeracluster.com/documentation-webpages/configuration.html#selinux), geçerli oturum için devre dışı bırakmak gereklidir. Düzen `/etc/selinux/config` sonraki yeniden başlatmalar için devre dışı bırakmak için.

            setenforce 0

            then editing `/etc/selinux/config` to set `SELINUX=permissive`
6. MySQL çalıştırır doğrulayın.

   a. MySQL başlatın.

           service mysql start
   b. MySQL yükleme güvenli, kök parola ayarlama, uzak kök oturum açma devre dışı bırakmak için anonim kullanıcılar kaldırın ve test veritabanını kaldır.

           mysql_secure_installation
   c. Küme işlemleri için ve isteğe bağlı olarak, uygulamalarınız için veritabanında bir kullanıcı oluşturun.

           mysql -u root -p
           GRANT ALL PRIVILEGES ON *.* TO 'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
           exit

   d. MySQL durdurun.

            service mysql stop
7. Bir yapılandırma yer tutucu oluşturun.

   a. Küme ayarları için bir yer tutucu oluşturmak için MySQL yapılandırmasını düzenleyin. Değiştirmez **`<Variables>`** veya artık açıklamadan çıkarın. Bu şablonu kullanarak bir VM oluşturduktan sonra gerçekleşir.

            vi /etc/my.cnf.d/server.cnf
   b. Düzen **[galera]** bölümünde ve temizleyin.

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
8. Gerekli bağlantı noktalarını Güvenlik Duvarı'nda, CentOS 7 FirewallD kullanarak açın.

   * MySQL: `firewall-cmd --zone=public --add-port=3306/tcp --permanent`
   * GALERA: `firewall-cmd --zone=public --add-port=4567/tcp --permanent`
   * GALERA IST: `firewall-cmd --zone=public --add-port=4568/tcp --permanent`
   * RSYNC: `firewall-cmd --zone=public --add-port=4444/tcp --permanent`
   * Güvenlik Duvarı'nı yeniden yükleyin: `firewall-cmd --reload`

9. Sistem performansı en iyi duruma getirme. Daha fazla bilgi için bkz: [performans stratejisi ayarlama](optimize-mysql.md).

   a. MySQL yapılandırma dosyasını yeniden düzenleyin.

            vi /etc/my.cnf.d/server.cnf
   b. Düzen **[mariadb]** bölümünde ve aşağıdaki içeriği ekleyin:

   > [!NOTE]
   > Bu innodb öneririz\_arabellek\_pool_size olan VM bellek yüzde 70 '. Azure VM 3.5 GB RAM'i olan Orta Bu örnekte, 2,45 GB olarak ayarlandı.
   >
   >

           innodb_buffer_pool_size = 2508M # The buffer pool contains buffered data and the index. This is usually set to 70 percent of physical memory.
           innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
           max_connections = 5000 # A larger value will give the server more time to recycle idled connections
           innodb_file_per_table = 1 # Speed up the table space transmission and optimize the debris management performance
           innodb_log_buffer_size = 128M # The log buffer allows transactions to run without having to flush the log to disk before the transactions commit
           innodb_flush_log_at_trx_commit = 2 # The setting of 2 enables the most data integrity and is suitable for Master in MySQL cluster
           query_cache_size = 0
10. MySQL durdurun, kümenin bir düğümü eklenirken kesintiye kaçının başlangıç çalışmasını MySQL hizmetini devre dışı bırakın ve makine sağlamayı sonlandırın.

        service mysql stop
        chkconfig mysql off
        waagent -deprovision
11. Portal üzerinden VM'yi yakalayın. (Şu anda [#1268 Azure CLI araçlarında sorun](https://github.com/Azure/azure-xplat-cli/issues/1268) Azure CLI araçları tarafından yakalanan görüntüleri eklenen veri disklerini yakalamayın olgu açıklanmaktadır.)

    a. Portal üzerinden makineyi kapatın.

    b. Tıklatın **yakalama** ve görüntü adı olarak belirtmeniz **mariadb galera görüntü**. Bir açıklama girin ve denetimi "Waagent Çalıştır."
      
      ![Sanal makine yakalanamadı](./media/mariadb-mysql-cluster/Capture2.PNG)

## <a name="create-the-cluster"></a>Kümeyi oluşturma
Üç VM şablonu oluşturduğunuz ve yapılandırmak ve küme Başlat oluşturun.

1. İlk CentOS 7 VM, aşağıdaki bilgileri sağlayarak oluşturulan mariadb galera görüntü görüntüden oluşturun:

 - Sanal ağ adı: mariadbvnet
 - Alt ağ: mariadb
 - Makine boyutu: Orta
 - Bulut hizmet adı: mariadbha (veya ad istediğiniz mariadbha.cloudapp.net erişilebilmesi)
 - Makine adı: mariadb1
 - Kullanıcı adı: azureuser
 - SSH erişimini: etkin
 - SSH sertifika .pem dosyasını geçirme ve oluşturulan .pem SSH anahtarı depolandığı yolu /path/to/key.pem değiştirme.

   > [!NOTE]
   > Aşağıdaki komutları daha anlaşılır olması için birden çok satır üzerinden bölünür, ancak her bir satır olarak girmeniz gerekir.
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
2. Mariadbha bulut hizmetine bağlanarak iki daha fazla sanal makine oluşturun. VM adı ve SSH bağlantı noktası aynı bulut hizmetindeki diğer vm'lerle çakışan benzersiz bağlantı noktasına değiştirin.

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
3. Her üç VM'ler iç IP adresi bir sonraki adımda almanız gerekir:

    ![IP adresi alınıyor](./media/mariadb-mysql-cluster/IP.png)
4. SSH, üç sanal makineleri için oturum açın ve bunların her biri üzerindeki yapılandırma dosyasını düzenlemek için kullanın.

        sudo vi /etc/my.cnf.d/server.cnf

    Açıklamadan çıkarın **`wsrep_cluster_name`** ve **`wsrep_cluster_address`** kaldırarak **#** satırın başındaki.
    Ayrıca, yerine **`<ServerIP>`** içinde **`wsrep_node_address`** ve **`<NodeName>`** içinde **`wsrep_node_name`** VM'in IP adresiyle ad, sırasıyla ve de bu satırlardaki yorumları kaldırın.
5. Küme üzerinde MariaDB1 başlatmak ve başlangıçta çalışmasına izin verin.

        sudo service mysql bootstrap
        chkconfig mysql on
6. MySQL MariaDB2 ve MariaDB3 başlatmak ve başlangıçta çalışmasına izin verin.

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balance-the-cluster"></a>Yük Dengeleme kümesi
Kümelenmiş sanal makineleri oluşturduğunuzda, koyarlar farklı hata ve güncelleştirme etki alanlarında ve o Azure hiçbir zaman bakım tüm makinelerde aynı anda olmadığından emin olmak için clusteravset adlı bir kullanılabilirlik kümesine eklenir. Bu yapılandırma Azure hizmet düzeyi sözleşmesi (SLA) desteklenmesi için gereksinimleri karşılıyor.

Artık üç düğümler arasında istekleri dengelemek için Azure yük dengeleyici kullanın.

Azure CLI kullanarak makinenizde aşağıdaki komutları çalıştırın.

Komut parametreleri yapıdır: `azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

CLI biraz uzun olabilir 15 saniye olarak, yük dengeleyici yoklama aralığı ayarlar. Portalında altında değiştirme **uç noktaları** herhangi biri sanal makineleri için.

![Uç noktayı düzenle](./media/mariadb-mysql-cluster/Endpoint.PNG)

Seçin **yük dengeli kümesi yeniden**.

![Yük dengeli kümesi yeniden yapılandırın](./media/mariadb-mysql-cluster/Endpoint2.PNG)

Değişiklik **yoklama aralığı** 5 saniyeye ve değişikliklerinizi kaydedin.

![Değiştirme yoklama aralığı](./media/mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validate-the-cluster"></a>Küme doğrulama
Sabit çalışma yapılır. Küme artık adresindeki erişilebilmelidir `mariadbha.cloudapp.net:3306`, sorunsuz ve verimli bir şekilde yük dengeleyici ve rota isteklerini üç sanal makineleri arasındaki trafik.

Sık kullanılan MySQL istemci bağlantısı veya bu küme çalıştığını doğrulamak için sanal makineleri birinden bağlantı için kullanın.

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
Bu makalede, bir üç düğümü MariaDB oluşturulan + Galera yüksek oranda kullanılabilir küme sanal Azure üzerinde çalışan CentOS 7 makineleri. Sanal makineleri Azure yük dengeleyici ile dengeleneceğini.

Bakmak isteyebilirsiniz [Linux MySQL kümede başka bir şekilde](mysql-cluster.md) ve yolları [iyileştirmek ve Azure Linux VM'ler MySQL performansı test](optimize-mysql.md).

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
