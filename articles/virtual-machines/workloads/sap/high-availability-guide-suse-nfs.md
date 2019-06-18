---
title: SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde NFS için yüksek kullanılabilirlik | Microsoft Docs
description: SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde NFS için yüksek kullanılabilirlik
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: mssedusch
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/15/2019
ms.author: sedusch
ms.openlocfilehash: ed92be0c1968d8f8a931d59d2dadefbbb12f2100
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64925734"
---
# <a name="high-availability-for-nfs-on-azure-vms-on-suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde NFS için yüksek kullanılabilirlik

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[2205917]: https://launchpad.support.sap.com/#/notes/2205917
[1944799]: https://launchpad.support.sap.com/#/notes/1944799
[1928533]: https://launchpad.support.sap.com/#/notes/1928533
[2015553]: https://launchpad.support.sap.com/#/notes/2015553
[2178632]: https://launchpad.support.sap.com/#/notes/2178632
[2191498]: https://launchpad.support.sap.com/#/notes/2191498
[2243692]: https://launchpad.support.sap.com/#/notes/2243692
[1984787]: https://launchpad.support.sap.com/#/notes/1984787
[1999351]: https://launchpad.support.sap.com/#/notes/1999351
[1410736]:https://launchpad.support.sap.com/#/notes/1410736

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[sles-hae-guides]:https://www.suse.com/documentation/sle-ha-12/
[sles-for-sap-bp]:https://www.suse.com/documentation/sles-for-sap-12/
[suse-ha-12sp3-relnotes]:https://www.suse.com/releasenotes/x86_64/SLE-HA/12-SP3/

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged-md%2Fazuredeploy.json
[template-file-server]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-file-server-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability.md

Bu makalede, sanal makineleri dağıtmak, sanal makineleri yapılandırma, küme Framework'ü yüklemek ve yüksek oranda kullanılabilir bir SAP sistemine paylaşılan verilerini depolamak için kullanılan yüksek oranda kullanılabilir bir NFS sunucusu yükleme açıklar.
Bu kılavuz iki SAP sistemlerinin NW1 ve NW2 kullanılan yüksek oranda kullanılabilir bir NFS sunucusunu ayarlama işlemi açıklanmaktadır. Örnekte (örneğin, sanal makineler, sanal ağlar) kaynakların adları, kullandığınız varsayılmıştır [SAP dosya sunucusu şablonu] [ template-file-server] kaynak önekiyle **prod**.

Önce aşağıdaki SAP notları ve raporları okuma

* SAP notu [1928533], sahip olduğu:
  * SAP yazılım dağıtımı için desteklenen bir Azure VM boyutlarının listesini
  * Azure VM boyutları için önemli kapasite bilgileri
  * Desteklenen bir SAP yazılım ve işletim sistemi (OS) ve veritabanı birleşimleri
  * Windows ve Linux'ta Microsoft Azure için gerekli SAP çekirdek sürümü

* SAP notu [2015553] azure'da SAP tarafından desteklenen SAP yazılım dağıtımları için önkoşulları listeler.
* SAP notu [2205917] SUSE Linux Enterprise Server işletim sistemi ayarlarını SAP uygulamaları için önerilir
* SAP notu [1944799] SAP HANA kılavuzu için SUSE Linux Enterprise Server SAP uygulamaları için vardır.
* SAP notu [2178632] ayrıntılı azure'da SAP için bildirilen tüm izlenen ölçümler hakkında bilgi içerir.
* SAP notu [2191498] azure'da Linux için gerekli SAP konak Aracısı sürümü vardır.
* SAP notu [2243692] Linux Azure üzerinde SAP lisanslama hakkında bilgi içeriyor.
* SAP notu [1984787] SUSE Linux Enterprise Server 12 ilgili genel bilgiler bulunur.
* SAP notu [1999351] Azure Gelişmiş izleme uzantısı için SAP için ek bilgiler.
* [SAP topluluk WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) tüm SAP notları Linux için zorunludur.
* [Azure sanal makineleri planlama ve uygulama için Linux üzerinde SAP][planning-guide]
* [(Bu makale) Linux'ta SAP için Azure sanal makineler dağıtımı][deployment-guide]
* [Linux'ta SAP için Azure sanal makineleri DBMS dağıtım][dbms-guide]
* [SUSE Linux Enterprise yüksek kullanılabilirlik uzantısı 12 SP3 en iyi uygulamalar kılavuzları][sles-hae-guides]
  * Yüksek oranda kullanılabilir bir NFS depolamada DRBD ve Pacemaker
* [SUSE Linux Enterprise Server SAP uygulamaları 12 SP3 en iyi uygulamalar kılavuzları][sles-for-sap-bp]
* [SUSE yüksek kullanılabilirlik uzantısı 12 SP3 sürüm notları][suse-ha-12sp3-relnotes]

## <a name="overview"></a>Genel Bakış

Yüksek kullanılabilirlik elde etmek için bir NFS sunucusunun SAP NetWeaver'ı gerektirir. NFS sunucusu ayrı bir kümede yapılandırılmış ve birden çok SAP sistemleri tarafından kullanılabilir.

![SAP NetWeaver-yüksek kullanılabilirlik genel bakış](./media/high-availability-guide-nfs/ha-suse-nfs.png)

NFS sunucusu bu NFS sunucusu kullanan her SAP sistemi için ayrılmış bir sanal ana bilgisayar adı ve sanal IP adreslerini kullanır. Azure üzerinde bir yük dengeleyici sanal IP adresi kullanmak için gereklidir. Aşağıdaki liste, yük dengeleyici yapılandırmasını gösterir.        

* Ön uç yapılandırması
  * IP adresi 10.0.0.4 NW1
  * IP adresi 10.0.0.5 NW2
* Arka uç yapılandırması
  * NFS kümesinin parçası olacak tüm sanal makineleri, birincil ağ arabirimlerine bağlı
* Araştırma bağlantı noktası
  * 61000 NW1 için bağlantı noktası
  * 61001 NW2 için bağlantı noktası
* Yük Dengeleme kuralları
  * NW1 2049 TCP
  * 2049 UDP NW1 için
  * NW2 2049 TCP
  * 2049 UDP NW2 için

## <a name="set-up-a-highly-available-nfs-server"></a>Yüksek oranda kullanılabilir bir NFS sunucusunu ayarlama

Sanal makineler de dahil olmak üzere gerekli tüm Azure kaynakları dağıtmak için bir Azure şablonu github'dan kullanabilirsiniz, kullanılabilirlik kümesi ve yük dengeleyici veya kaynakları el ile dağıtabilirsiniz.

### <a name="deploy-linux-via-azure-template"></a>Azure şablonu aracılığıyla Linux dağıtın

Azure Market görüntü için SUSE Linux Enterprise Server SAP uygulamaları 12 için yeni sanal makineleri dağıtmak için kullanabileceğiniz içerir.
Tüm gerekli kaynakları dağıtmak için Github'da hızlı başlangıç şablonlarından birini kullanabilirsiniz. Şablonu, sanal makineler, yük dengeleyici, kullanılabilirlik vb. kümesi dağıtır. Şablonu dağıtmak için aşağıdaki adımları izleyin:

1. Açık [SAP dosya sunucusu şablonu] [ template-file-server] Azure portalında   
1. Aşağıdaki parametreleri girin
   1. Kaynak ön eki  
      Kullanmak istediğiniz ön eki girin. Değeri, dağıtılan kaynaklar için önek olarak kullanılır.
   2. SAP sistemi sayısı  
      Bu dosya sunucusu kullanan SAP sistemlerini sayısını girin. Bu gerekli boyutta dağıtır ön uç yapılandırmaları, Yük Dengeleme kuralları, araştırma bağlantı noktaları, disk vb.
   3. İşletim sistemi türü  
      Linux dağıtımları birini seçin. Bu örnekte, SLES 12 seçin
   4. Yönetici kullanıcı adı ve yönetici parolası  
      Yeni bir kullanıcı oluşturulur makinesinde oturum açma için kullanılabilir.
   5. Alt ağ kimliği  
      Tanımlanan bir alt ağa sahip olduğunuz mevcut bir Vnet'te VM dağıtmak istiyorsanız, VM atanmalıdır belirli bir alt ağ kimliği adı için. Kimliği genellikle /subscriptions/ gibi görünüyor **&lt;abonelik kimliği&gt;** /resourceGroups/ **&lt;kaynak grubu adı&gt;** /providers/ Microsoft.Network/virtualNetworks/ **&lt;sanal ağ adı&gt;** /subnets/ **&lt;alt ağ adı&gt;**

### <a name="deploy-linux-manually-via-azure-portal"></a>Linux Azure Portalı aracılığıyla el ile dağıtma

Önce bu NFS küme için sanal makineler oluşturmak gerekir. Ardından, yük dengeleyici oluşturma ve arka uç havuzlarında sanal makinelerini kullanın.

1. Kaynak Grubu oluşturma
1. Sanal ağ oluşturma
1. Kullanılabilirlik kümesi oluşturma  
   Kümesi en çok güncelleştirme etki alanı
1. Sanal makine 1 kullanmak en az oluşturma SLES4SAP 12 SP3 SLES4SAP 12 SP3 BYOS görüntü SLES için SAP uygulamaları 12 SP3 (BYOS) Bu örnekte kullanılır  
   Daha önce oluşturduğunuz kullanılabilirlik kümesi seçin  
1. Sanal makine 2 kullanmak en az oluşturma, bu örnekte SLES4SAP 12 SP3 BYOS görüntü SLES4SAP 12 SP3  
   SLES için SAP uygulamaları 12 SP3 (BYOS) kullanılır  
   Daha önce oluşturduğunuz kullanılabilirlik kümesi seçin  
1. Her iki sanal makinelere her SAP sistemi için bir veri diski ekleyin.
1. Bir yük dengeleyiciye (dahili) oluşturma  
   1. Ön uç IP adresi oluşturma
      1. IP adresi 10.0.0.4 NW1
         1. Yük Dengeleyici açın, ön uç IP havuzu seçin ve Ekle'ye tıklayın
         1. Yeni ön uç IP havuzunun adını girin (örneğin **nw1 ön uç**)
         1. Atama statik olarak ayarlamanız ve IP adresini girin (örneğin **10.0.0.4**)
         1. Tamam'a tıklayın
      1. IP adresi 10.0.0.5 NW2
         * NW2 için yukarıdaki adımları yineleyin
   1. Arka uç havuzları oluşturma
      1. Birincil ağ arabirimine NW1 NFS kümenin parçası olması gereken tüm sanal makinelerin bağlı
         1. Yük Dengeleyici açın, arka uç havuzlarını seçin ve Ekle'ye tıklayın
         1. Yeni arka uç havuzunun adını girin (örneğin **nw1 arka uç**)
         1. Bir sanal makine Ekle
         1. Daha önce oluşturduğunuz kullanılabilirlik kümesi seçin
         1. NFS küme sanal makineleri seçin
         1. Tamam'a tıklayın
      1. Birincil ağ arabirimine NW2 NFS kümenin parçası olması gereken tüm sanal makinelerin bağlı
         * Arka uç havuzu için NW2 oluşturmak için yukarıdaki adımları yineleyin.
   1. Sistem durumu araştırmaları oluşturma
      1. 61000 NW1 için bağlantı noktası
         1. Yük Dengeleyici açın, sistem durumu araştırmaları seçin ve Ekle'ye tıklayın
         1. Yeni bir sistem durumu araştırma adını girin (örneğin **nw1 hp**)
         1. TCP bağlantı noktası 610 protokolü olarak seçin**00**, aralığı 5 ve sağlıksız durum eşiği 2 tutun
         1. Tamam'a tıklayın
      1. 61001 NW2 için bağlantı noktası
         * NW2 için durum araştırması oluşturmak için yukarıdaki adımları yineleyin.
   1. Yük Dengeleme kuralları
      1. NW1 2049 TCP
         1. Açık yük dengeleyici, Yük Dengeleme kuralları'nı seçin ve Ekle'ye tıklayın
         1. Yeni Yük Dengeleyici kuralı adını girin (örneğin **nw1 lb 2049**)
         1. Ön uç IP adresi, arka uç havuzu ve durum yoklaması, daha önce oluşturduğunuz seçin (örneğin **nw1 ön uç**)
         1. Protokol tutmak **TCP**, bağlantı noktasını girin **2049**
         1. 30 dakika boşta kalma zaman aşımı süresini artırın
         1. **Kayan IP etkinleştirdiğinizden emin olun**
         1. Tamam'a tıklayın
      1. 2049 UDP NW1 için
         * Bağlantı noktası 2049 ve UDP NW1 için yukarıdaki adımları yineleyin
      1. NW2 2049 TCP
         * Bağlantı noktası 2049 ve TCP NW2 için yukarıdaki adımları yineleyin
      1. 2049 UDP NW2 için
         * Bağlantı noktası 2049 ve UDP NW2 için yukarıdaki adımları yineleyin

> [!IMPORTANT]
> Azure vm'lerinde Azure yük dengeleyicinin arkasına yerleştirilen TCP zaman damgaları etkinleştirmeyin. TCP zaman damgaları etkinleştirme, sistem durumu araştırmaları başarısız olmasına neden olur. Parametre kümesi **net.ipv4.tcp_timestamps** için **0**. Ayrıntılar için bkz. [yük dengeleyici sistem durumu araştırmalarının](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview).

### <a name="create-pacemaker-cluster"></a>Pacemaker kümesi oluşturma

Bağlantısındaki [SLES azure'daki SUSE Linux Enterprise Server üzerinde Pacemaker ayarlama](high-availability-guide-suse-pacemaker.md) bu NFS sunucusu için bir temel Pacemaker kümesi oluşturmak için.

### <a name="configure-nfs-server"></a>NFS sunucusunu yapılandırma

Aşağıdaki öğeler ile önek **[A]** - tüm düğümler için geçerli **[1]** - düğüm 1 yalnızca uygulanabilir veya **[2]** - yalnızca düğüm 2 için geçerlidir.

1. **[A]**  Kurulum ana bilgisayar adı çözümlemesi

   Bir DNS sunucusu kullanabilir veya/etc/hosts tüm düğümlerde değiştirin. Bu örnek/Etc/Hosts dosyasının nasıl kullanılacağını gösterir.
   IP adresi ve aşağıdaki komutlarda bulunan ana bilgisayar adını değiştirin

   <pre><code>sudo vi /etc/hosts
   </code></pre>
   
   / Etc/hosts aşağıdaki satırları ekleyin. IP adresi ve ana bilgisayar adını, ortamınızla eşleşecek şekilde değiştirin.
   
   <pre><code># IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nw1-nfs</b>
   <b>10.0.0.5 nw2-nfs</b>
   </code></pre>

1. **[A]**  Etkinleştirme NFS sunucusu

   ' % S'kök NFS dışarı aktarma girişi oluşturma

   <pre><code>sudo sh -c 'echo /srv/nfs/ *\(rw,no_root_squash,fsid=0\)>/etc/exports'
   
   sudo mkdir /srv/nfs/
   </code></pre>

1. **[A]**  Drbd bileşenlerini yükler

   <pre><code>sudo zypper install drbd drbd-kmp-default drbd-utils
   </code></pre>

1. **[A]**  Drbd cihazlar için bir bölüm oluşturun

   Tüm kullanılabilir veri diskleri Listele

   <pre><code>sudo ls /dev/disk/azure/scsi1/
   </code></pre>

   Örnek çıktı
   
   ```
   lun0  lun1
   ```

   Her veri diski için bölüm oluşturma

   <pre><code>sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/disk/azure/scsi1/lun0'
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/disk/azure/scsi1/lun1'
   </code></pre>

1. **[A]**  LVM oluşturma yapılandırmaları

   Kullanılabilir tüm bölümleri listesi

   <pre><code>ls /dev/disk/azure/scsi1/lun*-part*
   </code></pre>

   Örnek çıktı
   
   ```
   /dev/disk/azure/scsi1/lun0-part1  /dev/disk/azure/scsi1/lun1-part1
   ```

   Her bölüm için LVM birimler oluşturun

   <pre><code>sudo pvcreate /dev/disk/azure/scsi1/lun0-part1  
   sudo vgcreate vg-<b>NW1</b>-NFS /dev/disk/azure/scsi1/lun0-part1
   sudo lvcreate -l 100%FREE -n <b>NW1</b> vg-<b>NW1</b>-NFS

   sudo pvcreate /dev/disk/azure/scsi1/lun1-part1
   sudo vgcreate vg-<b>NW2</b>-NFS /dev/disk/azure/scsi1/lun1-part1
   sudo lvcreate -l 100%FREE -n <b>NW2</b> vg-<b>NW2</b>-NFS
   </code></pre>

1. **[A]**  Drbd yapılandırın

   <pre><code>sudo vi /etc/drbd.conf
   </code></pre>

   Drbd.conf dosyası aşağıdaki iki satırı içerdiğinden emin olun

   <pre><code>include "drbd.d/global_common.conf";
   include "drbd.d/*.res";
   </code></pre>

   Genel drbd yapılandırmasını değiştirme

   <pre><code>sudo vi /etc/drbd.d/global_common.conf
   </code></pre>

   İşleyici ve net bölümünde aşağıdaki girişleri ekleyin.

   <pre><code>global {
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
             md-flushes yes;
             disk-flushes yes;
             c-plan-ahead 1;
             c-min-rate 100M;
             c-fill-target 20M;
             c-max-rate 4G;
        }
        net {
             after-sb-0pri discard-younger-primary;
             after-sb-1pri discard-secondary;
             after-sb-2pri call-pri-lost-after-sb;
             protocol     C;
             tcp-cork yes;
             max-buffers 20000;
             max-epoch-size 20000;
             sndbuf-size 0;
             rcvbuf-size 0;
        }
   }
   </code></pre>

1. **[A]**  NFS drbd cihazları oluşturun

   <pre><code>sudo vi /etc/drbd.d/<b>NW1</b>-nfs.res
   </code></pre>

   Çıkış ve yeni drbd cihaz yapılandırması ekleme

   <pre><code>resource <b>NW1</b>-nfs {
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

   <pre><code>sudo vi /etc/drbd.d/<b>NW2</b>-nfs.res
   </code></pre>

   Çıkış ve yeni drbd cihaz yapılandırması ekleme

   <pre><code>resource <b>NW2</b>-nfs {
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

   Drbd cihaz oluşturun ve başlatın

   <pre><code>sudo drbdadm create-md <b>NW1</b>-nfs
   sudo drbdadm create-md <b>NW2</b>-nfs
   sudo drbdadm up <b>NW1</b>-nfs
   sudo drbdadm up <b>NW2</b>-nfs
   </code></pre>

1. **[1]**  İlk eşitlemeyi atla

   <pre><code>sudo drbdadm new-current-uuid --clear-bitmap <b>NW1</b>-nfs
   sudo drbdadm new-current-uuid --clear-bitmap <b>NW2</b>-nfs
   </code></pre>

1. **[1]**  Birincil düğüm kümesi

   <pre><code>sudo drbdadm primary --force <b>NW1</b>-nfs
   sudo drbdadm primary --force <b>NW2</b>-nfs
   </code></pre>

1. **[1]**  Yeni drbd cihazları eşitlenir kadar bekleyin

   <pre><code>sudo drbdsetup wait-sync-resource NW1-nfs
   sudo drbdsetup wait-sync-resource NW2-nfs
   </code></pre>

1. **[1]**  Drbd cihazlarda dosya sistemleri oluşturun

   <pre><code>sudo mkfs.xfs /dev/drbd0
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

   Bir konaktan diğerine verileri eşitlemek için drbd kullanırken, böylece çağrılan bir bölme beyin ortaya çıkabilir. Bölme beyin burada her iki küme düğümünün drbd cihazın birincil olmasını yükseltilmiş ve eşitlenmemiş gittiği bir senaryodur. Nadir bir durum olabilir, ancak yine de işlemek ve mümkün olduğunca hızlı bir bölme beyin çözmek istiyor. Bu nedenle, bir bölme beyin gerçekleştiğinde bildirim almak önemlidir.

   Okuma [resmi drbd belgeleri](https://docs.linbit.com/doc/users-guide-83/s-configure-split-brain-behavior/#s-split-brain-notification) bölme beyin bildirimini ayarlama konusunda.

   Bölme beyin senaryodan otomatik olarak kurtarmaya mümkündür. Daha fazla bilgi için okuma [otomatik bölme beyin kurtarma ilkeleri](https://docs.linbit.com/doc/users-guide-83/s-configure-split-brain-behavior/#s-automatic-split-brain-recovery-configuration)
   
### <a name="configure-cluster-framework"></a>Küme çerçeve Yapılandır

1. **[1]**  SAP sistemine NW1 için NFS drbd cihazlar için Küme Yapılandırması Ekle

   <pre><code>sudo crm configure rsc_defaults resource-stickiness="200"

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
   
   sudo crm configure primitive nfsserver systemd:nfs-server \
     op monitor interval="30s"
   sudo crm configure clone cl-nfsserver nfsserver

   sudo crm configure primitive exportfs_<b>NW1</b> \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW1</b>" \
     options="rw,no_root_squash,crossmnt" clientspec="*" fsid=1 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive vip_<b>NW1</b>_nfs \
     IPaddr2 \
     params ip=<b>10.0.0.4</b> cidr_netmask=<b>24</b> op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>NW1</b>_nfs \
     anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k <b>61000</b>" op monitor timeout=20s interval=10 depth=0
   
   sudo crm configure group g-<b>NW1</b>_nfs \
     fs_<b>NW1</b>_sapmnt exportfs_<b>NW1</b> nc_<b>NW1</b>_nfs vip_<b>NW1</b>_nfs
   
   sudo crm configure order o-<b>NW1</b>_drbd_before_nfs inf: \
     ms-drbd_<b>NW1</b>_nfs:promote g-<b>NW1</b>_nfs:start
   
   sudo crm configure colocation col-<b>NW1</b>_nfs_on_drbd inf: \
     g-<b>NW1</b>_nfs ms-drbd_<b>NW1</b>_nfs:Master
   </code></pre>

1. **[1]**  SAP sistemine NW2 için NFS drbd cihazlar için Küme Yapılandırması Ekle

   <pre><code># Enable maintenance mode
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
     options="rw,no_root_squash" clientspec="*" fsid=2 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive vip_<b>NW2</b>_nfs \
     IPaddr2 \
     params ip=<b>10.0.0.5</b> cidr_netmask=<b>24</b> op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>NW2</b>_nfs \
     anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k <b>61001</b>" op monitor timeout=20s interval=10 depth=0
   
   sudo crm configure group g-<b>NW2</b>_nfs \
     fs_<b>NW2</b>_sapmnt exportfs_<b>NW2</b> nc_<b>NW2</b>_nfs vip_<b>NW2</b>_nfs
   
   sudo crm configure order o-<b>NW2</b>_drbd_before_nfs inf: \
     ms-drbd_<b>NW2</b>_nfs:promote g-<b>NW2</b>_nfs:start
   
   sudo crm configure colocation col-<b>NW2</b>_nfs_on_drbd inf: \
     g-<b>NW2</b>_nfs ms-drbd_<b>NW2</b>_nfs:Master
   </code></pre>

1. **[1]**  Bakım modunu devre dışı bırak
   
   <pre><code>sudo crm configure property maintenance-mode=false
   </code></pre>

## <a name="next-steps"></a>Sonraki adımlar

* [SAP ASCS ve veritabanı yükleme](high-availability-guide-suse.md)
* [Azure sanal makineleri planlama ve uygulama için SAP][planning-guide]
* [SAP için Azure sanal makineler dağıtımı][deployment-guide]
* [SAP için Azure sanal makineleri DBMS dağıtım][dbms-guide]
* Yüksek kullanılabilirlik ve olağanüstü durum kurtarma SAP hana (büyük örnekler) azure'da planlama oluşturma hakkında bilgi almak için bkz: [SAP HANA (büyük örnekler) azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md).
* Yüksek kullanılabilirlik ve Azure Vm'leri üzerinde SAP hana olağanüstü durum kurtarma planı oluşturma hakkında bilgi almak için bkz: [SAP HANA, yüksek kullanılabilirlik Azure Virtual Machines'de (VM'ler)][sap-hana-ha]
