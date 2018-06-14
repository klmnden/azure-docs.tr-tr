---
title: Yük dengeli kümeleri MySQL clusterize | Microsoft Docs
description: Linux MySQL kümesi azure'da Klasik dağıtım modeliyle oluşturulan bir yük dengeli, yüksek kullanılabilirlik ayarlama
services: virtual-machines-linux
documentationcenter: ''
author: bureado
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 6c413a16-e9b5-4ffe-a8a3-ae67046bbdf3
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/14/2015
ms.author: jparrel
ms.openlocfilehash: e2671def47879e3d4eae000c9084cd458e29b933
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
ms.locfileid: "30841273"
---
# <a name="use-load-balanced-sets-to-clusterize-mysql-on-linux"></a>MySQL Linux'ta clusterize için yük dengeli kümeleri kullanın
> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Azure Resource Manager](../../../resource-manager-deployment-model.md) ve klasik. Bu makale klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. A [Resource Manager şablonu](https://azure.microsoft.com/documentation/templates/mysql-replication/) MySQL Küme dağıtımı yapmanız gerekirse kullanılabilir.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Bu makalede inceler ve MySQL Server yüksek kullanılabilirlik öncü olarak keşfetme Microsoft Azure üzerinde yüksek oranda kullanılabilir Linux tabanlı hizmetler dağıtmak kullanılabilir farklı yaklaşımlara göstermektedir. Bu yaklaşım gösteren bir video kullanılabilir [Channel 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL).

Biz DRBD, Corosync ve Pacemaker göre hiçbir şey paylaşılmayan, iki düğümlü ve tek yöneticili MySQL yüksek kullanılabilirlik çözümü özetler. Yalnızca bir düğüm MySQL aynı anda çalıştırır. Aynı zamanda okuma ve yazma DRBD kaynaktan aynı anda yalnızca bir düğüme sınırlıdır.

Yük dengeli kümeleri Microsoft Azure'da hepsini işlevselliği ve uç nokta algılama, kaldırma ve VIP normal kurtarılmasını sağlamak için kullanacağınız çünkü LVS gibi bir VIP çözümü için gerek yoktur. VIP, bulut hizmeti ilk oluşturduğunuzda Microsoft Azure tarafından atanan genel olarak yönlendirilebilir bir IPv4 adresidir.

Diğer olası mimari NBD küme, Percona, Galera ve en az bir de dahil olmak üzere çeşitli ara yazılımı çözümler de dahil olmak üzere MySQL için bir VM olarak kullanılabilir [VM deposu](http://vmdepot.msopentech.com). Bu çözümlerin tek noktaya yayın ve çok noktaya yayın veya yayın üzerinde çoğaltabilir ve paylaşılan depolama ortamı veya birden çok ağ arabirimi güvenmeyin sürece senaryolar Microsoft Azure'da dağıtmak kolay olmalıdır.

Bunlar mimarileri kümeleme PostgreSQL ve OpenLDAP gibi diğer ürünlere yönelik benzer bir şekilde genişletilebilir. Örneğin, bu yük dengeleyici yordamı paylaşılmadığı ile başarıyla çok ana OpenLDAP ile test edilmiştir ve bizim Channel 9 blogunda izleyebilirsiniz.

## <a name="get-ready"></a>Hazırlanma
Aşağıdaki kaynaklar ve yeteneklerini gerekir:

  - Geçerli bir abonelik, en az iki VM oluşturabilmek için bir Microsoft Azure hesabı (Bu örnekte XS kullanılan)
  - Bir ağ ve bir alt ağ
  - Bir benzeşim grubu
  - Bir kullanılabilirlik kümesi
  - Bulut hizmeti ile aynı bölgede VHD oluşturma ve Linux VM'ler ekleme olanağı

### <a name="tested-environment"></a>Sınanan ortamı
* Ubuntu 13.10
  * DRBD
  * MySQL Server
  * Corosync ve Pacemaker

### <a name="affinity-group"></a>Benzeşim grubu
Azure portalında oturum açarak çözüm için benzeşim grubu oluşturmak seçme **ayarları**ve bir benzeşim grubu oluşturma. Daha sonra oluşturulan ayrılan kaynakları bu benzeşim grubuna atanır.

### <a name="networks"></a>Ağlar
Yeni bir ağ oluşturulur ve bir alt ağ içinde oluşturulur. Bu örnek 10.10.10.0/24 ağ içinde yalnızca bir /24 alt ağ ile kullanır.

### <a name="virtual-machines"></a>Sanal makineler
İlk Ubuntu 13.10 VM Endorsed Ubuntu galeri görüntüsü kullanılarak oluşturulur ve adlandırılır `hadb01`. Yeni bir bulut hizmeti hadb adlandırılan işlemde oluşturulur. Bu ad, daha fazla kaynak eklendiğinde, hizmet olacak paylaşılan, yük dengelemeli yapısı gösterilmektedir. Oluşturulmasını `hadb01` portal olaysız ve tamamlanan kullanmaktır. SSH için bir uç nokta otomatik olarak oluşturulur ve yeni bir ağ seçtiniz. Artık bir kullanılabilirlik VM'ler için kümesi oluşturabilirsiniz.

(Teknik olarak, bulut hizmeti oluşturulduğunda) ilk VM oluşturulduktan sonra ikinci VM oluşturma `hadb02`. İkinci VM için Ubuntu 13.10 VM Galeriden Portalı'nı kullanarak, ancak var olan bir bulut hizmetini kullanın `hadb.cloudapp.net`, yeni bir tane oluşturmak yerine. Ağ ve kullanılabilirlik kümesi otomatik olarak seçilmesi gerekir. Bir SSH uç noktası, çok oluşturulur.

Her iki VM oluşturulduktan sonra SSH bağlantı noktasını not edin `hadb01` (TCP 22) ve `hadb02` (otomatik olarak Azure tarafından atanan).

### <a name="attached-storage"></a>Bağlı depolama
Her iki VM için yeni bir disk ekleyin ve işleminde 5 GB disk oluşturabilirsiniz. Disklerin ana işletim sistemi disklerinizi kullanılmak VHD kapsayıcısında barındırılır. Diskleri oluşturup bağlı sonra çekirdek yeni cihaz görürsünüz çünkü Linux yeniden başlatmaya gerek yoktur. Bu aygıt genellikle değil `/dev/sdc`. Denetleme `dmesg` çıktısı için.

Kullanarak her bir VM üzerinde bir bölüm oluşturmak `cfdisk` (birincil, Linux bölüm) ve yeni bölüm tablosuna yazma. Bir dosya sistemi bu bölüme oluşturmayın.

## <a name="set-up-the-cluster"></a>Küme ayarlama
APT Corosync, Pacemaker ve DRBD her iki Ubuntu VM yüklemek için kullanın. İle bunun için `apt-get`, aşağıdaki kodu çalıştırın:

    sudo apt-get install corosync pacemaker drbd8-utils.

MySQL şu anda yüklemeyin. Debian ve Ubuntu yükleme betikleri başlatma MySQL veri dizini üzerinde `/var/lib/mysql`, ancak dizin DRBD dosya sistemi tarafından değiştirilen çünkü MySQL daha sonra yüklemeniz gerekir.

Doğrulayın (kullanarak `/sbin/ifconfig`) her iki VM 10.10.10.0/24 alt ağ adresleri ve bunların birbirlerine ada göre ping atabildiğinizi kullanıyor. Aynı zamanda `ssh-keygen` ve `ssh-copy-id` her iki VM, bir parola gerektirmeden SSH yoluyla kurabilir emin olmak için.

### <a name="set-up-drbd"></a>DRBD ayarlayın
Arka plandaki kullanan DRBD kaynak oluşturma `/dev/sdc1` üretmek için bölüm bir `/dev/drbd1` ext3 kullanılarak biçimlendirilmiş ve birincil ve ikincil düğüm kullanılan kaynak.

1. Açık `/etc/drbd.d/r0.res` ve her iki VM aşağıdaki kaynak tanımında kopyalayın:

        resource r0 {
          on `hadb01` {
            device  /dev/drbd1;
            disk   /dev/sdc1;
            address  10.10.10.4:7789;
            meta-disk internal;
          }
          on `hadb02` {
            device  /dev/drbd1;
            disk   /dev/sdc1;
            address  10.10.10.5:7789;
            meta-disk internal;
          }
        }

2. Kaynak kullanarak başlatmak `drbdadm` her iki VM üzerinde:

        sudo drbdadm -c /etc/drbd.conf role r0
        sudo drbdadm up r0

3. Birincil VM üzerinde (`hadb01`), DRBD kaynak (birincil) sahipliğini zorla:

        sudo drbdadm primary --force r0

/ Proc/drbd içeriğini incelerseniz (`sudo cat /proc/drbd`) her iki VM üzerinde görmelisiniz `Primary/Secondary` üzerinde `hadb01` ve `Secondary/Primary` üzerinde `hadb02`, bu noktada çözümü ile tutarlı. 5 GB disk, müşterilere ücret ödemeden 10.10.10.0/24 ağ üzerinden eşitlenir.

Disk eşitlendikten sonra dosya sistemi üzerinde oluşturabileceğiniz `hadb01`. Test amacıyla, ext2 kullandık, ancak aşağıdaki kodu ext3 dosya sistemi oluşturacak:

    mkfs.ext3 /dev/drbd1

### <a name="mount-the-drbd-resource"></a>DRBD kaynak Bağla
Üzerinde DRBD kaynakları bağlamak artık hazırsınız `hadb01`. Debian ve türevleri kullanım `/var/lib/mysql` MySQL'ın veri dizini olarak. MySQL yüklemediniz olduğundan dizin oluşturun ve DRBD kaynak bağlayın. Bu seçenek gerçekleştirmek için aşağıdaki kodu çalıştırmak `hadb01`:

    sudo mkdir /var/lib/mysql
    sudo mount /dev/drbd1 /var/lib/mysql

## <a name="set-up-mysql"></a>MySQL ayarlayın
MySQL yüklemek hazır artık `hadb01`:

    sudo apt-get install mysql-server

İçin `hadb02`, iki seçeneğiniz vardır. Mysql /var/lib/mysql oluşturmak, yeni bir veri dizin ile doldurun ve ardından içeriği kaldırmak sunuculu, yükleyebilirsiniz. Bu seçenek gerçekleştirmek için aşağıdaki kodu çalıştırmak `hadb02`:

    sudo apt-get install mysql-server
    sudo service mysql stop
    sudo rm –rf /var/lib/mysql/*

Yük devretme için ikinci seçenek, `hadb02` ve orada mysql server yükleyin. Yükleme betikleri mevcut yükleme görürsünüz ve touch olmaz.

Aşağıdaki kod çalıştıracağınız `hadb01`:

    sudo drbdadm secondary –force r0

Aşağıdaki kod çalıştıracağınız `hadb02`:

    sudo drbdadm primary –force r0
    sudo apt-get install mysql-server

Yük devretme DRBD şimdi planlamıyorsanız, ilk seçenek tartışmaya açık bir şekilde daha az rağmen Zarif daha kolay olur. Bu ayarladıktan sonra MySQL veritabanınızı üzerinde çalışmaya başlayabiliriz. Aşağıdaki kod çalıştıracağınız `hadb02` (veya hangi sunucuların birini DRBD göre etkindir):

    mysql –u root –p
    CREATE DATABASE azureha;
    CREATE TABLE things ( id SERIAL, name VARCHAR(255) );
    INSERT INTO things VALUES (1, "Yet another entity");
    GRANT ALL ON things.\* TO root;

> [!WARNING]
> Bu son deyim etkili bir şekilde bu tablodaki kök kullanıcı için kimlik doğrulaması devre dışı bırakır. Bu üretim-sınıf tarafından değiştirilmesi gereken verme deyimleri ve yalnızca tanımlayıcı amaçlarla yer alır.

(Bu kılavuzun amacı olan) VM'ler dışında sorgularından yapmak istiyorsanız, ayrıca ağ MySQL için etkinleştirmeniz gerekir. Her iki VM üzerinde açmak `/etc/mysql/my.cnf` ve Git `bind-address`. Adres 127.0.0.1 0.0.0.0 olarak değiştirin. Dosyayı kaydettikten sonra sorun bir `sudo service mysql restart` geçerli birincil üzerinde.

### <a name="create-the-mysql-load-balanced-set"></a>MySQL yük dengeli kümesi oluşturma
Portalına geri dönün, Git `hadb01`ve seçin **uç noktaları**. Bir uç nokta oluşturmak için MySQL (TCP 3306) aşağı açılan listeden seçip **oluştur Yeni Yük dengeli kümesi**. Yük dengeli uç nokta adı `lb-mysql`. Ayarlama **zaman** 5 saniye, en düşük.

Uç nokta oluşturduktan sonra Git `hadb02`, seçin **uç noktaları**ve bir uç nokta oluşturun. Seçin `lb-mysql`ve ardından MySQL aşağı açılan listeden seçin. Bu adım için Azure CLI de kullanabilirsiniz.

Şimdi küme el ile işlem için gereken her şeyi sahipsiniz.

### <a name="test-the-load-balanced-set"></a>Sınama yük dengeli kümesi
Testleri bir dış makineden herhangi bir MySQL istemcisi kullanarak veya bir Azure Web sitesi olarak çalışan phpMyAdmin gibi belirli uygulamaları kullanarak gerçekleştirilebilir. Bu durumda, başka bir Linux kutuda MySQL'ın komut satırı aracı kullanılır:

    mysql azureha –u root –h hadb.cloudapp.net –e "select * from things;"

### <a name="manually-failing-over"></a>El ile yük devrediliyor
Yük devretme işlemlerini MySQL kapatılıyor DRBD'ın birincil değiştirme ve MySQL yeniden başlatmayı benzetimini yapabilirsiniz.

Bu görevi gerçekleştirmek için aşağıdaki kodu hadb01 üzerinde çalıştırın:

    service mysql stop && umount /var/lib/mysql ; drbdadm secondary r0

Ardından, hadb02 üzerinde:

    drbdadm primary r0 ; mount /dev/drbd1 /var/lib/mysql && service mysql start

El ile yük devri sonra uzak sorgunuzu yineleyebilir ve mükemmel çalışması gerekir.

## <a name="set-up-corosync"></a>Corosync ayarlayın
Corosync çalışmaya Pacemaker için gereken temel küme altyapısıdır. Pacemaker işlevindeki sinyal daha benzer kalırken sinyal (ve Ultramonkey gibi diğer yöntemleri), Corosync CRM işlevleri bölme içindir.

Ana Corosync için Azure üzerinde Corosync çok noktaya yayın üzerinden tek noktaya yayın iletişim tercih eder, ancak Microsoft Azure ağı yalnızca tek noktaya yayın destekler sınırlamadır.

Neyse ki, Corosync çalışma tek noktaya yayın modu vardır. Tüm düğümlerin kendilerini arasında iletişim için IP adresleri de dahil olmak üzere, yapılandırma dosyalarındaki düğümleri tanımlama gerektiğini yalnızca gerçek sınırlamadır. Tek noktaya yayın ve değişiklik adresi, düğüm listeler ve günlük dizinleri bağlamak için biz Corosync örnek dosyalarını kullanabilirsiniz (Ubuntu kullanan `/var/log/corosync` örnek kullanım dosyalar sırada `/var/log/cluster`) ve çekirdek araçları sağlar.

> [!NOTE]
> Aşağıdaki `transport: udpu` yönergesi ve her iki düğüm için el ile tanımlanan IP adresi.

Aşağıdaki kod çalıştıracağınız `/etc/corosync/corosync.conf` iki düğüm için:

    totem {
      version: 2
      crypto_cipher: none
      crypto_hash: none
      interface {
        ringnumber: 0
        bindnetaddr: 10.10.10.0
        mcastport: 5405
        ttl: 1
      }
      transport: udpu
    }

    logging {
      fileline: off
      to_logfile: yes
      to_syslog: yes
      logfile: /var/log/corosync/corosync.log
      debug: off
      timestamp: on
      logger_subsys {
        subsys: QUORUM
        debug: off
        }
      }

    nodelist {
      node {
        ring0_addr: 10.10.10.4
        nodeid: 1
      }

      node {
        ring0_addr: 10.10.10.5
        nodeid: 2
      }
    }

    quorum {
      provider: corosync_votequorum
    }

Her iki VM üzerindeki bu yapılandırma dosyasını kopyalayın ve her iki düğüm Corosync Başlat:

    sudo service start corosync

Hizmeti başlatılıyor, kısa süre sonra küme geçerli halka kurulacaktır ve çekirdek constituted. Bu işlev günlükleri gözden geçirme veya aşağıdaki kodu çalıştırarak kontrol edebilirsiniz:

    sudo corosync-quorumtool –l

Aşağıdaki görüntüye benzer bir çıktı görürsünüz:

![corosync quorumtool - m örnek çıkış](./media/mysql-cluster/image001.png)

## <a name="set-up-pacemaker"></a>Pacemaker ayarlayın
Pacemaker küme kaynakları için izleme, ne zaman ana Git aşağı tanımlamak ve bu kaynakları için ikincil anahtar için kullanır. Kaynakların kullanılabilir komut kümesi ya da LSB'si (Init benzeri) komut dosyaları, diğer seçenekler arasında tanımlanabilir.

"DRBD kaynak, bağlama noktası ve MySQL hizmeti sahip olmasını" Pacemaker istiyoruz. Pacemaker DRBD açıp bağlama ve bunu çıkarın ve sonra durdurup başlatın hatalı bir şey birincil ile olduğunda MySQL doğru sırada, Kurulum işlemi tamamlanmış olur.

Pacemaker ilk kez yüklediğinizde, yapılandırmanızı gösterilene benzer yeteri kadar basit olmalıdır:

    node $id="1" hadb01
      attributes standby="off"
    node $id="2" hadb02
      attributes standby="off"

1. Çalıştırarak yapılandırmasını denetleyin `sudo crm configure show`.
2. Bir dosya oluşturun (gibi `/tmp/cluster.conf`) aşağıdaki kaynaklarla:

        primitive drbd_mysql ocf:linbit:drbd \
              params drbd_resource="r0" \
              op monitor interval="29s" role="Master" \
              op monitor interval="31s" role="Slave"

        ms ms_drbd_mysql drbd_mysql \
              meta master-max="1" master-node-max="1" \
                clone-max="2" clone-node-max="1" \
                notify="true"

        primitive fs_mysql ocf:heartbeat:Filesystem \
              params device="/dev/drbd/by-res/r0" \
              directory="/var/lib/mysql" fstype="ext3"

        primitive mysqld lsb:mysql

        group mysql fs_mysql mysqld

        colocation mysql_on_drbd \
               inf: mysql ms_drbd_mysql:Master

        order mysql_after_drbd \
               inf: ms_drbd_mysql:promote mysql:start

        property stonith-enabled=false

        property no-quorum-policy=ignore

3. Dosya yapılandırmaya yükleyin. Yalnızca bu bir düğümünden yapmanız gerekir.

        sudo crm configure
          load update /tmp/cluster.conf
          commit
          exit

4. Pacemaker her iki düğüm önyüklemede başladığından emin olun:

        sudo update-rc.d pacemaker defaults

5. Kullanarak `sudo crm_mon –L`, düğümlerinden biri için Küme Yöneticisi hale geldi ve tüm kaynakları çalıştıran doğrulayın. Kaynakları çalıştığını denetlemek için bağlama ve ps kullanabilirsiniz.

Aşağıdaki ekran görüntüsü gösterildiği `crm_mon` durdurulmuş bir düğümle (Ctrl + C seçerek çıkış):

![crm_mon düğüm durduruldu](./media/mysql-cluster/image002.png)

Bu ekran, düğüm, bir ana ve bir ikincil gösterir:

![crm_mon işletimsel ana/bağımlı](./media/mysql-cluster/image003.png)

## <a name="testing"></a>Test Etme
Otomatik Yük devretme benzetimi için hazırsınız. Bunu yapmanın iki yolu vardır: yazılım ve donanım.

Kümenin kapatma işlevi yumuşak biçimini kullanır: ``crm_standby -U `uname -n` -v on``. Bu ana kullanırsanız, ikincil devreye girer. Bu geri off olarak ayarlamayı unutmayın. Bunu yapmazsanız, crm_mon bekleme tek bir düğüm gösterir.

Sabit şekilde portalı üzerinden veya çalışma düzeyi (diğer bir deyişle, durdurmak, kapatma) VM'de değiştirerek birincil VM (hadb01) kapatılıyor. Bu Corosync ve ana giderek aşağı sinyal tarafından Pacemaker yardımcı olur. Bu test edebilirsiniz (Bakım pencereleri için yararlıdır), ancak Ayrıca, VM dondurma tarafından senaryo zorlayabilirsiniz.

## <a name="stonith"></a>STONITH
VM kapatma fiziksel bir aygıtı denetleyen STONITH komut dosyası yerine Azure CLI aracılığıyla mümkün olması gerekir. Kullanabileceğiniz `/usr/lib/stonith/plugins/external/ssh` Bankası ve kümenin yapılandırmasında STONITH etkinleştir. Azure CLI genel olarak yüklenmelidir ve yayımlama ayarlarını ve profil kümenin kullanıcı için yüklenen.

Kaynak için örnek kod edinilebilir [GitHub](https://github.com/bureado/aztonith). Aşağıdakileri ekleyerek küme yapılandırmasını değiştirme `sudo crm configure`:

    primitive st-azure stonith:external/azure \
      params hostlist="hadb01 hadb02" \
      clone fencing st-azure \
      property stonith-enabled=true \
      commit

> [!NOTE]
> Komut dosyası denetimleri yukarı/aşağı gerçekleştirmez. 15 ping denetimleri özgün SSH kaynak var, ancak bir Azure VM için kurtarma süresi daha fazla değişken olabilir.

## <a name="limitations"></a>Sınırlamalar
Aşağıdaki sınırlamalar uygulanır:

* Bir kaynak olarak Pacemaker kullandığı DRBD yönetir linbit DRBD kaynak betik `drbdadm down` düğümü yalnızca bekleme durumuna geçiyor olsa bile bir düğüm kapanırken. Asıl yazma alır ancak ikincil DRBD kaynak eşitleme değil çünkü bu ideal değildir. Asıl graciously başarısız olmaz, ikincil daha eski bir dosya sistemi durumu alabilir. Bu çözümü iki olası yolu vardır:
  * Zorunlu bir `drbdadm up r0` tüm küme düğümlerinde yerel (clusterized değil) izleme olaylarını aracılığıyla içinde
  * Dikkat ederek linbit DRBD betik düzenleme `down` çağrılmaz `/usr/lib/ocf/resource.d/linbit/drbd`
* Yük dengeleyicinin en az beş saniyede uygulamaları küme durumunu algılayan ve zaman aşımı süresi daha dayanıklı şekilde yanıt vermesi gerekir. Uygulama sıraları ve sorgu middlewares gibi diğer mimarileri de yardımcı olabilir.
* MySQL ayarlama yazma yönetilebilir hızı yapılır ve bellek kaybını en aza indirmek için mümkün olduğunca sık diske önbellekleri Temizlenen sağlamak gereklidir.
* Yazma performansı VM'de bağımlı bu DRBD tarafından cihaz çoğaltmak için kullanılan mekanizma olduğundan sanal anahtarında birbirine.
