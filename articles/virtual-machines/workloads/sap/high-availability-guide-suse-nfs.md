---
title: Yüksek kullanılabilirlik için NFS SUSE Linux Enterprise Server üzerinde Azure vm'lerinde | Microsoft Docs
description: SUSE Linux Enterprise Server üzerinde Azure vm'lerinde NFS yüksek kullanılabilirlik
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: mssedusch
manager: timlt
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/21/2018
ms.author: sedusch
ms.openlocfilehash: b1a7b962d07b64aaa662aab937feed1782851a7b
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="high-availability-for-nfs-on-azure-vms-on-suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server üzerinde Azure vm'lerinde NFS yüksek kullanılabilirlik

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[2205917]:https://launchpad.support.sap.com/#/notes/2205917
[1944799]:https://launchpad.support.sap.com/#/notes/1944799
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[1410736]:https://launchpad.support.sap.com/#/notes/1410736

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[suse-drbd-guide]:https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha_techguides/book_sleha_techguides.html

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged-md%2Fazuredeploy.json
[template-file-server]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-file-server-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability.md

Bu makalede, sanal makineler dağıtmak, sanal makineleri yapılandırma, küme Framework'ü yüklemek ve yüksek oranda kullanılabilir bir SAP sisteminin paylaşılan veri depolamak için kullanılan yüksek oranda kullanılabilir bir NFS sunucusu yüklemek açıklar.
Bu kılavuz, iki SAP sistemleri tarafından NW1 ve NW2 kullanılan yüksek oranda kullanılabilir bir NFS sunucusunu ayarlama açıklar. Örnekte kaynakların (örneğin, sanal makineler, sanal ağlar) adları, kullandığınız varsayılmıştır [SAP dosya sunucusu şablonu] [ template-file-server] kaynak önekiyle **Üretim**.

Aşağıdaki SAP notlar ve raporları ilk okuma

* SAP Not [1928533], sahip olduğu:
  * SAP yazılım dağıtımı için desteklenen Azure VM boyutlarını listesi
  * Azure VM boyutlarını önemli kapasite bilgilerini
  * Desteklenen bir SAP yazılım ve işletim sistemi (OS) ve veritabanı birleşimler
  * Windows ve Microsoft Azure Linux için gerekli SAP çekirdek sürümü

* SAP Not [2015553] azure'da SAP desteklenen SAP yazılım dağıtımları için önkoşulları listeler.
* SAP Not [2205917] SUSE Linux Enterprise Server işletim sistemi ayarlarını SAP uygulamaları için önerilir
* SAP Not [1944799] SAP HANA yönergeleri SUSE Linux Enterprise Server için SAP uygulamaları için vardır.
* SAP Not [2178632] ayrıntılı Azure SAP için bildirilen tüm izleme ölçümleri hakkında bilgi sağlanmıştır.
* SAP Not [2191498] Azure Linux için gerekli SAP konak Aracısı sürümü vardır.
* SAP Not [2243692] Linux Azure üzerinde SAP lisanslama hakkında bilgi vardır.
* SAP Not [1984787] SUSE Linux Enterprise Server 12 hakkında genel bilgi yer almaktadır.
* SAP Not [1999351] Azure Gelişmiş izleme uzantısı SAP için ek sorun giderme bilgileri içeriyor.
* [SAP topluluk WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) SAP notları Linux için gerekli tüm sahiptir.
* [Azure sanal makineleri planlama ve uygulama Linux üzerinde SAP için][planning-guide]
* [(Bu makalede) Linux'ta SAP için Azure sanal makineler dağıtımı][deployment-guide]
* [Linux üzerinde SAP için Azure sanal makineleri DBMS dağıtımı][dbms-guide]
* [Senaryo SAP HANA SR performansı en iyi duruma getirilmiş][suse-hana-ha-guide]  
  Kılavuz SAP HANA sistem çoğaltma şirket içi yedekleme ayarlamak için gereken tüm bilgileri içerir. Bu kılavuz, bir taban çizgisi olarak kullanın.
* [Yüksek oranda kullanılabilir NFS depolama DRBD ve Pacemaker] [ suse-drbd-guide] kılavuz yüksek oranda kullanılabilir bir NFS sunucusu kurmak için gerekli tüm bilgileri içerir. Bu kılavuz, bir taban çizgisi olarak kullanın.


## <a name="overview"></a>Genel Bakış

Yüksek kullanılabilirlik elde etmek için bir NFS sunucusunun SAP NetWeaver gerektirir. NFS sunucusu ayrı bir kümede yapılandırılmış ve birden çok SAP sistemleri tarafından kullanılabilir.

![SAP NetWeaver yüksek kullanılabilirlik genel bakış](./media/high-availability-guide-nfs/ha-suse-nfs.png)

NFS sunucusu bu NFS sunucusu kullanan her SAP sistemi için ayrılmış bir sanal ana bilgisayar adı ve sanal IP adreslerini kullanır. Azure üzerinde bir yük dengeleyici sanal IP adresi kullanmak için gereklidir. Aşağıdaki listede, yük dengeleyici yapılandırması gösterilmektedir.        

* Ön uç yapılandırma
  * IP adresi 10.0.0.4 NW1 için
  * IP adresi 10.0.0.5 NW2 için
* Arka uç yapılandırması
  * NFS Küme parçası olması gereken tüm sanal makinelerin birincil ağ arabirimlerine bağlı
* Sonda bağlantı noktası
  * Bağlantı noktası 61000 NW1 için
  * 61001 NW2 için bağlantı noktası
* Loadbalancing kuralları
  * NW1 2049 TCP
  * NW1 için 2049 UDP
  * NW2 2049 TCP
  * NW2 için 2049 UDP

## <a name="set-up-a-highly-available-nfs-server"></a>Yüksek oranda kullanılabilir bir NFS sunucusunu ayarlama

Sanal makineler de dahil olmak üzere tüm gerekli Azure kaynaklarını dağıtmak için bir Azure şablonu github'dan kullanabilirsiniz, kullanılabilirlik kümesi ve yük dengeleyici veya kaynakları el ile dağıtabilirsiniz.

### <a name="deploy-linux-via-azure-template"></a>Linux Azure şablonu aracılığıyla dağıtma

Azure Market SAP uygulamaları 12 için yeni sanal makineleri dağıtmak için kullanabileceğiniz SUSE Linux Enterprise Server için bir görüntü içerir.
Tüm gerekli kaynakları dağıtmak için github'da hızlı başlangıç şablonlarından birini kullanabilirsiniz. Şablonun sanal makineler, yük dengeleyici, kullanılabilirlik vb. kümesi dağıtır. Şablonu dağıtmak için aşağıdaki adımları izleyin:

1. Açık [SAP dosya sunucusu şablonu] [ template-file-server] Azure portalında   
1. Aşağıdaki parametreleri girin
   1. Kaynak öneki  
      Kullanmak istediğiniz ön eki girin. Değer, dağıtılan kaynaklar için önek olarak kullanılır.
   2. SAP sistem sayısı bu dosya Sunucusu'nu kullanacak SAP sistemleri sayısını girin. Bu gerekli boyutta dağıtacağınız ön uç yapılandırmaları, Yük Dengeleme kuralları, araştırma bağlantı noktaları, disk vb.
   3. İşletim sistemi türü  
      Linux dağıtımları birini seçin. Bu örnekte, SLES 12 seçin
   4. Yönetici kullanıcı adı ve yönetici parolası  
      Yeni bir kullanıcı oluşturulur makineye oturum açmak için kullanılabilir.
   5. Alt ağ kimliği  
      Sanal makineler için bağlanması alt ağ kimliği. Yeni bir sanal ağ oluşturmak veya şirket içi ağınıza sanal makineye bağlanmak için VPN veya hızlı rota sanal ağınızın alt seçmek istiyorsanız boş bırakın. Kimliği genellikle /subscriptions/ gibi görünüyor**&lt;abonelik kimliği&gt;**/resourceGroups/**&lt;kaynak grubu adı&gt;**/providers/ Microsoft.Network/virtualNetworks/**&lt;sanal ağ adı&gt;**/subnets/**&lt;alt ağ adı&gt;**

### <a name="deploy-linux-manually-via-azure-portal"></a>Linux Azure portalı üzerinden el ile dağıtma

Önce bu NFS küme için sanal makineleri oluşturmanız gerekir. Daha sonra bir yük dengeleyicisi oluşturun ve sanal makineleri arka uç havuzlarını kullanın.

1. Kaynak Grubu oluşturma
1. Sanal Ağ Oluştur
1. Kullanılabilirlik kümesi oluştur  
   Set max güncelleştirme etki alanı
1. Sanal makine 1 oluşturun   
   En azından bu örnekte SLES4SAP 12 SP1 BYOS görüntü SLES4SAP 12 SP1 https://portal.azure.com/#create/suse-byos.sles-for-sap-byos12-sp1  
   SLES için SAP uygulamaları 12 SP1 (BYOS) kullanılır  
   Daha önce oluşturduğunuz kullanılabilirlik kümesi seçin  
1. Sanal makine 2 oluşturun   
   En azından bu örnekte SLES4SAP 12 SP1 BYOS görüntü SLES4SAP 12 SP1 https://portal.azure.com/#create/suse-byos.sles-for-sap-byos12-sp1  
   SLES için SAP uygulamaları 12 SP1 (BYOS) kullanılır  
   Daha önce oluşturduğunuz kullanılabilirlik kümesi seçin  
1. Her SAP sistem için bir veri diski hem sanal makineleri ekleyin.
1. Bir yük dengeleyiciye (dahili) oluşturma  
   1. Ön uç IP adreslerini oluşturun
      1. IP adresi 10.0.0.4 NW1 için
         1. Yük Dengeleyici açın, ön uç IP havuzu seçin ve Ekle'yi tıklatın
         1. Yeni ön uç IP havuzunun adını girin (örneğin **nw1 ön uç**)
         1. Atama statik olarak ayarlamanız ve IP adresini girin (örneğin **10.0.0.4**)
         1. Tamam'ı tıklatın         
      1. IP adresi 10.0.0.5 NW2 için
         * NW2 için yukarıdaki adımları yineleyin
   1. Arka uç havuzları oluşturma
      1. Birincil Ağ arabirimleri NW1 NFS kümenin parçası olması gereken tüm sanal makinelerin bağlı
         1. Yük Dengeleyici açın, arka uç havuzlarını seçin ve Ekle'yi tıklatın
         1. Yeni arka uç havuzunun adını girin (örneğin **nw1 arka uç**)
         1. Bir sanal makine Ekle
         1. Daha önce oluşturduğunuz kullanılabilirlik kümesi seçin
         1. NFS küme sanal makineleri seçin
         1. Tamam'ı tıklatın
      1. Birincil Ağ arabirimleri NW2 NFS kümenin parçası olması gereken tüm sanal makinelerin bağlı
         * NW2 için bir arka uç havuzu oluşturmak için yukarıdaki adımları yineleyin
   1. Sistem durumu araştırmalarının oluşturma
      1. Bağlantı noktası 61000 NW1 için
         1. Yük Dengeleyici açın, sistem durumu araştırmalarının seçin ve Ekle'yi tıklatın
         1. Yeni durum araştırması adını girin (örneğin **nw1 hp**)
         1. TCP bağlantı noktası 610 protokolü olarak seçin**00**, aralığı 5 ile sağlıksız durum eşiği 2 tutun
         1. Tamam'ı tıklatın
      1. 61001 NW2 için bağlantı noktası
         * Bir sistem durumu araştırması için NW2 oluşturmak için yukarıdaki adımları yineleyin
   1. Loadbalancing kuralları
      1. NW1 2049 TCP
         1. Yük Dengeleyici açın, Yük Dengeleme kuralları seçin ve Ekle'yi tıklatın
         1. Yeni Yük Dengeleyici kuralı adını girin (örneğin **nw1 lb 2049**)
         1. Ön uç IP adresi, arka uç havuzu ve daha önce oluşturduğunuz durumu araştırması seçin (örneğin **nw1 ön uç**)
         1. Protokol tutmak **TCP**, bağlantı noktası girin **2049**
         1. -30 dakika boşta kalma zaman aşımı süresini artırın
         1. **Kayan IP etkinleştirdiğinizden emin olun**
         1. Tamam'ı tıklatın    
      1. NW1 için 2049 UDP
         * Yukarıdaki adımları NW1 için bağlantı noktası 2049 ve UDP için yineleyin
      1. NW2 2049 TCP
         * Yukarıdaki adımları NW2 için bağlantı noktası 2049 ve TCP için yineleyin
      1. NW2 için 2049 UDP
         * Yukarıdaki adımları NW2 için bağlantı noktası 2049 ve UDP için yineleyin

### <a name="create-pacemaker-cluster"></a>Pacemaker kümesi oluşturma

Adımları [Pacemaker SUSE Linux Enterprise Server Azure üzerinde ayarlama](high-availability-guide-suse-pacemaker.md) bu NFS sunucusu için temel Pacemaker küme oluşturmak için.

### <a name="configure-nfs-server"></a>NFS sunucusunu yapılandırın

Aşağıdaki öğeler ile ya da önek **[A]** - tüm düğümleri için geçerli **[1]** - 1 düğümü yalnızca uygulanabilir veya **[2]** - yalnızca düğüm 2 için geçerlidir.

1. **[A]**  Kurulum ana bilgisayar adı çözümlemesi   

   Bir DNS sunucusu kullanın veya tüm düğümlerde/etc/hosts değiştirin. Bu örnek/Etc/Hosts dosyasının nasıl kullanılacağını gösterir.
   IP adresi ve aşağıdaki komutlarda ana bilgisayar adını değiştirin

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   / Etc/hosts aşağıdaki satırları ekleyin. Ortamınıza uyum sağlaması için ana bilgisayar adı ve IP adresini değiştirme   
   
   <pre><code>
   # IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nw1-nfs</b>
   <b>10.0.0.5 nw2-nfs</b>
   </code></pre>

1. **[A]**  Etkinleştirmek NFS sunucusu

   Kök dışa aktarma girişi oluşturmak, NFS Server başlatıldığında ve AutoStart'ı etkinleştirmek

   <pre><code>
   sudo sh -c 'echo /srv/nfs/ *\(rw,no_root_squash,fsid=0\)>/etc/exports'
   
   sudo mkdir /srv/nfs/

   sudo systemctl enable nfsserver
   sudo service nfsserver restart
   </code></pre>

1. **[A]**  Drbd bileşenlerini yükleme

   <pre><code>
   sudo zypper install drbd drbd-kmp-default drbd-utils
   </code></pre>

1. **[A]**  Drbd cihazlar için bir bölüm oluşturun

   Tüm kullanılabilir veri diskleri Listele

   <pre><code>
   sudo ls /dev/disk/azure/scsi1/
   </code></pre>

   Örnek çıktı
   
   ```
   lun0  lun1
   ```

   Her veri diski bölümleri oluşturma

   <pre><code>
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/disk/azure/scsi1/lun0'
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/disk/azure/scsi1/lun1'
   </code></pre>

1. **[A]**  LVM oluşturma yapılandırmaları

   Tüm kullanılabilir bölümler listesi

   <pre><code>
   ls /dev/disk/azure/scsi1/lun*-part*
   </code></pre>

   Örnek çıktı
   
   ```
   /dev/disk/azure/scsi1/lun0-part1  /dev/disk/azure/scsi1/lun1-part1
   ```

   Her bölüm için LVM birim oluşturun

   <pre><code>
   sudo pvcreate /dev/disk/azure/scsi1/lun0-part1  
   sudo vgcreate vg-<b>NW1</b>-NFS /dev/disk/azure/scsi1/lun0-part1
   sudo lvcreate -l 100%FREE -n <b>NW1</b> vg-<b>NW1</b>-NFS

   sudo pvcreate /dev/disk/azure/scsi1/lun1-part1
   sudo vgcreate vg-<b>NW2</b>-NFS /dev/disk/azure/scsi1/lun1-part1
   sudo lvcreate -l 100%FREE -n <b>NW2</b> vg-<b>NW2</b>-NFS
   </code></pre>

1. **[A]**  Drbd yapılandırın

   <pre><code>
   sudo vi /etc/drbd.conf
   </code></pre>

   Drbd.conf dosyası aşağıdaki iki satırı içerdiğinden emin olun

   <pre><code>
   include "drbd.d/global_common.conf";
   include "drbd.d/*.res";
   </code></pre>

   Genel drbd yapılandırmasını değiştirme

   <pre><code>
   sudo vi /etc/drbd.d/global_common.conf
   </code></pre>

   Aşağıdaki girişleri işleyici ve ağ bölümü ekleyin.

   <pre><code>
   global {
        usage-count no;
   }
   common {
        handlers {
             fence-peer "/usr/lib/drbd/crm-fence-peer.sh";
             after-resync-target "/usr/lib/drbd/crm-unfence-peer.sh";
             split-brain "/usr/lib/drbd/notify-split-brain.sh root";
             pri-lost-after-sb "/usr/lib/drbd/notify-pri-lost-after-sb.sh; /usr/lib/drbd/notify-emergency-reboot.sh; echo b > /proc/sysrq-trigger ; reboot -f";
        }
        startup {
             wfc-timeout 0;
        }
        options {
        }
        disk {
             resync-rate 50M;
        }
        net {
             after-sb-0pri discard-younger-primary;
             after-sb-1pri discard-secondary;
             after-sb-2pri call-pri-lost-after-sb;
        }
   }
   </code></pre>

1. **[A]**  NFS drbd aygıtları oluşturun

   <pre><code>
   sudo vi /etc/drbd.d/<b>NW1</b>-nfs.res
   </code></pre>

   Çıkış ve yeni drbd Aygıt Yapılandırması Ekle

   <pre><code>
   resource <b>NW1</b>-nfs {
        protocol     C;
        disk {
             on-io-error       detach;
        }
        on <b>prod-nfs-0</b> {
             address   <b>10.0.0.6:7790</b>;
             device    /dev/drbd<b>0</b>;
             disk      /dev/<b>vg-NW1-NFS</b>/<b>NW1</b>;
             meta-disk internal;
        }
        on <b>prod-nfs-1</b> {
             address   <b>10.0.0.7:7790</b>;
             device    /dev/drbd<b>0</b>;
             disk      /dev/<b>vg-NW1-NFS</b>/<b>NW1</b>;
             meta-disk internal;
        }
   }
   </code></pre>

   <pre><code>
   sudo vi /etc/drbd.d/<b>NW2</b>-nfs.res
   </code></pre>

   Çıkış ve yeni drbd Aygıt Yapılandırması Ekle

   <pre><code>
   resource <b>NW2</b>-nfs {
        protocol     C;
        disk {
             on-io-error       detach;
        }
        on <b>prod-nfs-0</b> {
             address   <b>10.0.0.6:7791</b>;
             device    /dev/drbd<b>1</b>;
             disk      /dev/<b>vg-NW2-NFS</b>/<b>NW2</b>;
             meta-disk internal;
        }
        on <b>prod-nfs-1</b> {
             address   <b>10.0.0.7:7791</b>;
             device    /dev/drbd<b>1</b>;
             disk      /dev/<b>vg-NW2-NFS</b>/<b>NW2</b>;
             meta-disk internal;
        }
   }
   </code></pre>

   Drbd aygıtı oluşturun ve başlatın

   <pre><code>
   sudo drbdadm create-md <b>NW1</b>-nfs
   sudo drbdadm create-md <b>NW2</b>-nfs
   sudo drbdadm up <b>NW1</b>-nfs
   sudo drbdadm up <b>NW2</b>-nfs
   </code></pre>

1. **[1]**  Atla ilk eşitleme

   <pre><code>
   sudo drbdadm new-current-uuid --clear-bitmap <b>NW1</b>-nfs
   sudo drbdadm new-current-uuid --clear-bitmap <b>NW2</b>-nfs
   </code></pre>

1. **[1]**  Birincil düğüm kümesi

   <pre><code>
   sudo drbdadm primary --force <b>NW1</b>-nfs
   sudo drbdadm primary --force <b>NW2</b>-nfs
   </code></pre>

1. **[1]**  Yeni drbd aygıtları eşitlenir kadar bekleyin

   <pre><code>
   sudo drbdsetup wait-sync-resource NW1-nfs
   sudo drbdsetup wait-sync-resource NW2-nfs
   </code></pre>

1. **[1]**  Drbd cihazlarda dosya sistemleri oluşturma

   <pre><code>
   sudo mkfs.xfs /dev/drbd0
   sudo mkdir /srv/nfs/NW1
   sudo chattr +i /srv/nfs/NW1
   sudo mount -t xfs /dev/drbd0 /srv/nfs/NW1
   sudo mkdir /srv/nfs/NW1/sidsys
   sudo mkdir /srv/nfs/NW1/sapmntsid
   sudo mkdir /srv/nfs/NW1/trans
   sudo mkdir /srv/nfs/NW1/ASCS
   sudo mkdir /srv/nfs/NW1/ASCSERS
   sudo mkdir /srv/nfs/NW1/SCS
   sudo mkdir /srv/nfs/NW1/SCSERS
   sudo umount /srv/nfs/NW1

   sudo mkfs.xfs /dev/drbd1
   sudo mkdir /srv/nfs/NW2
   sudo chattr +i /srv/nfs/NW2
   sudo mount -t xfs /dev/drbd1 /srv/nfs/NW2
   sudo mkdir /srv/nfs/NW2/sidsys
   sudo mkdir /srv/nfs/NW2/sapmntsid
   sudo mkdir /srv/nfs/NW2/trans
   sudo mkdir /srv/nfs/NW2/ASCS
   sudo mkdir /srv/nfs/NW2/ASCSERS
   sudo mkdir /srv/nfs/NW2/SCS
   sudo mkdir /srv/nfs/NW2/SCSERS
   sudo umount /srv/nfs/NW2
   </code></pre>

1. **[A]**  Kurulum drbd ayrık beyinli algılama

   Bir ana bilgisayardan diğerine verileri eşitlemek için drbd kullanarak, böylece çağrılan beyinli oluşabilir. Bölme beyin burada her iki küme düğümünün drbd cihazın birincil olmasını yükseltilir ve eşitlenmemiş gittiğini bir senaryodur. Çok nadir bir durum olabilir, ancak yine işlemek ve kadar hızlı bir bölme beyin çözmek istiyor. Bu nedenle, bir bölme beyin meydana geldiğinde bildirim almak önemlidir.

   Okuma [resmi drbd belgelerine](http://docs.linbit.com/doc/users-guide-83/s-configure-split-brain-behavior/#s-split-brain-notification) bir bölme beyin bildirimini ayarlama konusunda.

   Bölme beyin senaryodan otomatik olarak kurtarmaya mümkündür. Daha fazla bilgi için okuma [otomatik bölme beyin kurtarma ilkeleri](http://docs.linbit.com/doc/users-guide-83/s-configure-split-brain-behavior/#s-automatic-split-brain-recovery-configuration)
   
### <a name="configure-cluster-framework"></a>Küme çerçeve Yapılandır

1. **[1]**  Küme yapılandırmasında SAP sistemi NW1 için NFS drbd aygıtı Ekle

   <pre><code>
   # Enable maintenance mode
   sudo crm configure property maintenance-mode=true
   
   sudo crm configure primitive drbd_<b>NW1</b>_nfs \
     ocf:linbit:drbd \
     params drbd_resource="<b>NW1</b>-nfs" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"
   
   sudo crm configure ms ms-drbd_<b>NW1</b>_nfs drbd_<b>NW1</b>_nfs \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true" interleave="true"
   
   sudo crm configure primitive fs_<b>NW1</b>_sapmnt \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd0 \
     directory=/srv/nfs/<b>NW1</b>  \
     fstype=xfs \
     op monitor interval="10s"
   
   sudo crm configure primitive exportfs_<b>NW1</b> \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW1</b>" \
     options="rw,no_root_squash" clientspec="*" fsid=1 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW1</b>_sidsys \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW1</b>/sidsys" \
     options="rw,no_root_squash" clientspec="*" fsid=2 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW1</b>_sapmntsid \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW1</b>/sapmntsid" \
     options="rw,no_root_squash" clientspec="*" fsid=3 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW1</b>_trans \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW1</b>/trans" \
     options="rw,no_root_squash" clientspec="*" fsid=4 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW1</b>_ASCS \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW1</b>/ASCS" \
     options="rw,no_root_squash" clientspec="*" fsid=5 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW1</b>_ASCSERS \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW1</b>/ASCSERS" \
     options="rw,no_root_squash" clientspec="*" fsid=6 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW1</b>_SCS \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW1</b>/SCS" \
     options="rw,no_root_squash" clientspec="*" fsid=7 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW1</b>_SCSERS \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW1</b>/SCSERS" \
     options="rw,no_root_squash" clientspec="*" fsid=8 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive vip_<b>NW1</b>_nfs \
     IPaddr2 \
     params ip=<b>10.0.0.4</b> cidr_netmask=<b>24</b> op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>NW1</b>_nfs \
     anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k <b>61000</b>" op monitor timeout=20s interval=10 depth=0
   
   sudo crm configure group g-<b>NW1</b>_nfs \
     fs_<b>NW1</b>_sapmnt exportfs_<b>NW1</b> exportfs_<b>NW1</b>_sidsys \
     exportfs_<b>NW1</b>_sapmntsid exportfs_<b>NW1</b>_trans exportfs_<b>NW1</b>_ASCS \
     exportfs_<b>NW1</b>_ASCSERS exportfs_<b>NW1</b>_SCS exportfs_<b>NW1</b>_SCSERS \
     nc_<b>NW1</b>_nfs vip_<b>NW1</b>_nfs
   
   sudo crm configure order o-<b>NW1</b>_drbd_before_nfs inf: \
     ms-drbd_<b>NW1</b>_nfs:promote g-<b>NW1</b>_nfs:start
   
   sudo crm configure colocation col-<b>NW1</b>_nfs_on_drbd inf: \
     g-<b>NW1</b>_nfs ms-drbd_<b>NW1</b>_nfs:Master
   </code></pre>

1. **[1]**  Küme yapılandırmasında SAP sistemi NW2 için NFS drbd aygıtı Ekle

   <pre><code>
   # Enable maintenance mode
   sudo crm configure property maintenance-mode=true   
   
   sudo crm configure primitive drbd_<b>NW2</b>_nfs \
     ocf:linbit:drbd \
     params drbd_resource="<b>NW2</b>-nfs" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"
   
   sudo crm configure ms ms-drbd_<b>NW2</b>_nfs drbd_<b>NW2</b>_nfs \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true" interleave="true"
   
   sudo crm configure primitive fs_<b>NW2</b>_sapmnt \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd1 \
     directory=/srv/nfs/<b>NW2</b>  \
     fstype=xfs \
     op monitor interval="10s"
   
   sudo crm configure primitive exportfs_<b>NW2</b> \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW2</b>" \
     options="rw,no_root_squash" clientspec="*" fsid=9 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW2</b>_sidsys \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW2</b>/sidsys" \
     options="rw,no_root_squash" clientspec="*" fsid=10 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW2</b>_sapmntsid \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW2</b>/sapmntsid" \
     options="rw,no_root_squash" clientspec="*" fsid=11 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW2</b>_trans \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW2</b>/trans" \
     options="rw,no_root_squash" clientspec="*" fsid=12 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW2</b>_ASCS \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW2</b>/ASCS" \
     options="rw,no_root_squash" clientspec="*" fsid=13 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW2</b>_ASCSERS \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW2</b>/ASCSERS" \
     options="rw,no_root_squash" clientspec="*" fsid=14 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW2</b>_SCS \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW2</b>/SCS" \
     options="rw,no_root_squash" clientspec="*" fsid=15 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW2</b>_SCSERS \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW2</b>/SCSERS" \
     options="rw,no_root_squash" clientspec="*" fsid=16 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive vip_<b>NW2</b>_nfs \
     IPaddr2 \
     params ip=<b>10.0.0.5</b> cidr_netmask=<b>24</b> op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>NW2</b>_nfs \
     anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k <b>61001</b>" op monitor timeout=20s interval=10 depth=0
   
   sudo crm configure group g-<b>NW2</b>_nfs \
     fs_<b>NW2</b>_sapmnt exportfs_<b>NW2</b> exportfs_<b>NW2</b>_sidsys \
     exportfs_<b>NW2</b>_sapmntsid exportfs_<b>NW2</b>_trans exportfs_<b>NW2</b>_ASCS \
     exportfs_<b>NW2</b>_ASCSERS exportfs_<b>NW2</b>_SCS exportfs_<b>NW2</b>_SCSERS \
     nc_<b>NW2</b>_nfs vip_<b>NW2</b>_nfs
   
   sudo crm configure order o-<b>NW2</b>_drbd_before_nfs inf: \
     ms-drbd_<b>NW2</b>_nfs:promote g-<b>NW2</b>_nfs:start
   
   sudo crm configure colocation col-<b>NW2</b>_nfs_on_drbd inf: \
     g-<b>NW2</b>_nfs ms-drbd_<b>NW2</b>_nfs:Master
   </code></pre>

1. **[1]**  Devre dışı bırak Bakım modu
   
   <pre><code>
   sudo crm configure property maintenance-mode=false
   </code></pre>

## <a name="next-steps"></a>Sonraki adımlar
* [SAP ASCS ve veritabanını yükleme](high-availability-guide-suse.md)
* [Azure sanal makineleri planlama ve uygulama SAP için][planning-guide]
* [SAP için Azure sanal makineler dağıtımı][deployment-guide]
* [SAP için Azure sanal makineleri DBMS dağıtımı][dbms-guide]
* Yüksek kullanılabilirlik ve olağanüstü durum kurtarma SAP HANA planlama azure'da (büyük örnekler) oluşturmayı öğrenmek için bkz: [SAP HANA (büyük örnekler) Azure üzerinde yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md).
* Yüksek kullanılabilirlik ve Azure vm'lerinde SAP HANA olağanüstü durum kurtarma planı oluşturmak öğrenmek için bkz: [SAP HANA, yüksek kullanılabilirlik Azure Virtual Machines'de (VM'ler)][sap-hana-ha]
