---
title: Azure sanal makineler Red Hat Enterprise Linux üzerinde SAP NetWeaver için yüksek kullanılabilirlik | Microsoft Docs
description: Azure sanal makineler Red Hat Enterprise Linux üzerinde SAP NetWeaver için yüksek kullanılabilirlik
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
ms.date: 04/30/2019
ms.author: sedusch
ms.openlocfilehash: 4e224a1abf72bfa068bebaf971e34c492b15d7c0
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65143004"
---
# <a name="azure-virtual-machines-high-availability-for-sap-netweaver-on-red-hat-enterprise-linux"></a>Azure sanal makineler Red Hat Enterprise Linux üzerinde SAP NetWeaver için yüksek kullanılabilirlik

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2009879]:https://launchpad.support.sap.com/#/notes/2009879
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[1410736]:https://launchpad.support.sap.com/#/notes/1410736

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability-rhel.md
[glusterfs-ha]:high-availability-guide-rhel-glusterfs.md

Bu makalede, sanal makineleri dağıtmak, sanal makineleri yapılandırma, küme Framework'ü yüklemek ve yüksek oranda kullanılabilir bir SAP NetWeaver 7.50 sistemi yükleyin açıklar.
Örnek yapılandırmalarında yükleme komutlarını vs. ASCS örnek numarasını 00, 02 numarası Ağıranlar örnek ve SAP sistemi kimliği NW1 kullanılır. Örnekte (örneğin, sanal makineler, sanal ağlar) kaynakların adları, kullandığınız varsayılmıştır [ASCS/SCS şablon] [ template-multisid-xscs] kaynakları oluşturmak için kaynak NW1 önek ile.

Önce aşağıdaki SAP notları ve raporları okuma

* SAP notu [1928533], sahip olduğu:
  * SAP yazılım dağıtımı için desteklenen bir Azure VM boyutlarının listesini
  * Azure VM boyutları için önemli kapasite bilgileri
  * Desteklenen bir SAP yazılım ve işletim sistemi (OS) ve veritabanı birleşimleri
  * Windows ve Linux'ta Microsoft Azure için gerekli SAP çekirdek sürümü

* SAP notu [2015553] azure'da SAP tarafından desteklenen SAP yazılım dağıtımları için önkoşulları listeler.
* SAP notu [2002167] Red Hat Enterprise Linux işletim sistemi ayarlarını önerilir
* SAP notu [2009879] Red Hat Enterprise Linux için SAP HANA yönergeleri içeriyor
* SAP notu [2178632] ayrıntılı azure'da SAP için bildirilen tüm izlenen ölçümler hakkında bilgi içerir.
* SAP notu [2191498] azure'da Linux için gerekli SAP konak Aracısı sürümü vardır.
* SAP notu [2243692] Linux Azure üzerinde SAP lisanslama hakkında bilgi içeriyor.
* SAP notu [1999351] Azure Gelişmiş izleme uzantısı için SAP için ek bilgiler.
* [SAP topluluk WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) tüm SAP notları Linux için zorunludur.
* [Azure sanal makineleri planlama ve uygulama için Linux üzerinde SAP][planning-guide]
* [Linux'ta SAP için Azure sanal makineler dağıtımı][deployment-guide]
* [Linux'ta SAP için Azure sanal makineleri DBMS dağıtım][dbms-guide]
* [Red Hat Gluster depolaması için ürün belgeleri](https://access.redhat.com/documentation/red_hat_gluster_storage/)
* [SAP Netweaver pacemaker kümedeki](https://access.redhat.com/articles/3150081)
* Genel RHEL belgeleri
  * [Yüksek kullanılabilirlik eklentilere genel bakış](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_overview/index)
  * [Yüksek kullanılabilirlik eklenti Yönetim](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_administration/index)
  * [Yüksek kullanılabilirlik eklenti başvurusu](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/index)
  * [ASCS/Ağıranlar RHEL 7.5 tek başına kaynaklar ile SAP Netweaver için yapılandırma](https://access.redhat.com/articles/3569681)
  * [2 (ENSA2) tek başına kuyruğa sunucuyla SLES RHEL üzerinde Pacemaker ASCS/Ağıranlar SAP S/4hana'yı yapılandırma ](https://access.redhat.com/articles/3974941)
* Azure özel RHEL belgeleri:
  * [RHEL yüksek kullanılabilirlik kümelerini - Microsoft Azure sanal makineleri küme üyeleri olarak ilkeleri desteği](https://access.redhat.com/articles/3131341)
  * [Yükleme ve Microsoft Azure'da Red Hat Enterprise Linux 7.4 (ve üzeri) yüksek kullanılabilirlik kümesi yapılandırma](https://access.redhat.com/articles/3252491)

## <a name="overview"></a>Genel Bakış

Yüksek kullanılabilirlik elde etmek için SAP NetWeaver paylaşılan depolama gerektirir. GlusterFS ayrı bir kümede yapılandırılmış ve birden çok SAP sistemleri tarafından kullanılabilir.

![SAP NetWeaver-yüksek kullanılabilirlik genel bakış](./media/high-availability-guide-rhel/ha-rhel.png)

SAP NetWeaver ASCS, SAP NetWeaver SCS, SAP NetWeaver Ağıranlar ve SAP HANA veritabanı sanal ana bilgisayar adı ve sanal IP adresleri kullanın. Azure üzerinde bir yük dengeleyici sanal IP adresi kullanmak için gereklidir. Aşağıdaki liste, (A) yapılandırılmasını gösterir SCS ve Ağıranlar yük dengeleyici.

> [!IMPORTANT]
> Azure sanal makinelerinde konuk işletim sistemi gibi çoklu SID, SAP ASCS/Ağıranlar Red Hat Linux ile kümeleme **desteklenmiyor**. Çoklu SID kümeleme Pacemaker kümedeki farklı SID'lere sahip birden çok SAP ASCS/Ağıranlar örneklerinin yüklenmesini açıklar.

### <a name="ascs"></a>(A)SCS

* Ön uç yapılandırması
  * IP adresi 10.0.0.7
* Arka uç yapılandırması
  * (A) bir parçası olması gereken tüm sanal makinelerin birincil ağ arabirimlerine bağlı SCS/Ağıranlar küme
* Araştırma bağlantı noktası
  * Port 620<strong>&lt;nr&gt;</strong>
* Yük Dengeleme kuralları
  * 32<strong>&lt;nr&gt;</strong> TCP
  * 36<strong>&lt;nr&gt;</strong> TCP
  * 39<strong>&lt;nr&gt;</strong> TCP
  * 81<strong>&lt;nr&gt;</strong> TCP
  * 5<strong>&lt;nr&gt;</strong>13 TCP
  * 5<strong>&lt;nr&gt;</strong>14 TCP
  * 5<strong>&lt;nr&gt;</strong>16 TCP

### <a name="ers"></a>CILARIN

* Ön uç yapılandırması
  * IP adresi 10.0.0.8
* Arka uç yapılandırması
  * (A) bir parçası olması gereken tüm sanal makinelerin birincil ağ arabirimlerine bağlı SCS/Ağıranlar küme
* Araştırma bağlantı noktası
  * Port 621<strong>&lt;nr&gt;</strong>
* Yük Dengeleme kuralları
  * 32<strong>&lt;nr&gt;</strong> TCP
  * 33<strong>&lt;nr&gt;</strong> TCP
  * 5<strong>&lt;nr&gt;</strong>13 TCP
  * 5<strong>&lt;nr&gt;</strong>14 TCP
  * 5<strong>&lt;nr&gt;</strong>16 TCP

## <a name="setting-up-glusterfs"></a>GlusterFS ayarlama

SAP NetWeaver taşıma ve profil dizin için paylaşılan depolama gerektirir. Okuma [üzerindeki Azure Vm'lerinde SAP NetWeaver için Red Hat Enterprise Linux GlusterFS] [ glusterfs-ha] GlusterFS ' için SAP NetWeaver ayarlama.

## <a name="setting-up-ascs"></a>(A) SCS ayarlama

Sanal makineler de dahil olmak üzere tüm gerekli Azure kaynakları dağıtmak için bir Azure şablonu github'dan kullanabilirsiniz, kullanılabilirlik kümesi ve yük dengeleyici veya kaynakları el ile dağıtabilirsiniz.

### <a name="deploy-linux-via-azure-template"></a>Azure şablonu aracılığıyla Linux dağıtın

Azure Market'te Red Hat Enterprise Linux için yeni sanal makineleri dağıtmak için kullanabileceğiniz bir görüntü içerir. Tüm gerekli kaynakları dağıtmak için Github'da hızlı başlangıç şablonlarından birini kullanabilirsiniz. Şablonu, sanal makineler, yük dengeleyici, kullanılabilirlik vb. kümesi dağıtır. Şablonu dağıtmak için aşağıdaki adımları izleyin:

1. Açık [ASCS/SCS şablon] [ template-multisid-xscs] Azure portalında  
1. Aşağıdaki parametreleri girin
   1. Kaynak ön eki  
      Kullanmak istediğiniz ön eki girin. Değeri, dağıtılan kaynaklar için önek olarak kullanılır.
   1. Yığın türü  
      SAP NetWeaver yığın türü seçin
   1. İşletim sistemi türü  
      Linux dağıtımları birini seçin. Bu örnekte, RHEL 7'yi seçin.
   1. Veritabanı türü  
      HANA seçin
   1. SAP sistemi sayısı  
      Bu kümede çalışan SAP sistemine sayısı. 1'i seçin.
   1. Sistem kullanılabilirliği  
      HA seçin
   1. Yönetici kullanıcı adı, yönetici parolası veya SSH anahtarı  
      Yeni bir kullanıcı oluşturulur makinede oturum açmak için kullanılabilir.
   1. Alt ağ kimliği  
   Tanımlanan bir alt ağa sahip olduğunuz mevcut bir Vnet'te VM dağıtmak istiyorsanız, VM atanmalıdır belirli bir alt ağ kimliği adı için. Kimliği genellikle /subscriptions/ gibi görünüyor**&lt;abonelik kimliği&gt;**/resourceGroups/**&lt;kaynak grubu adı&gt;**/providers/ Microsoft.Network/virtualNetworks/**&lt;sanal ağ adı&gt;**/subnets/**&lt;alt ağ adı&gt;**

### <a name="deploy-linux-manually-via-azure-portal"></a>Linux Azure Portalı aracılığıyla el ile dağıtma

Bu küme için sanal makineler oluşturmak önce gerekir. Ardından, yük dengeleyici oluşturma ve arka uç havuzlarında sanal makinelerini kullanın.

1. Kaynak Grubu oluşturma
1. Sanal Ağ Oluştur
1. Kullanılabilirlik kümesi oluşturma  
   Kümesi en çok güncelleştirme etki alanı
1. 1 sanal makine oluşturma  
   En azından bu Red Hat Enterprise Linux 7.4 örnek görüntüsünde, RHEL 7 <https://portal.azure.com/#create/RedHat.RedHatEnterpriseLinux74-ARM>  
   Daha önce oluşturduğunuz kullanılabilirlik kümesi seçin  
1. 2 sanal makine oluşturma  
   En azından bu Red Hat Enterprise Linux 7.4 örnek görüntüsünde, RHEL 7 <https://portal.azure.com/#create/RedHat.RedHatEnterpriseLinux74-ARM>  
   Daha önce oluşturduğunuz kullanılabilirlik kümesi seçin  
1. Her iki sanal makine için en az bir veri diski ekleme  
   Veri disklerini / usr/sap/için kullanılan`<SAPSID`> dizini
1. Bir yük dengeleyiciye (dahili) oluşturma  
   1. Ön uç IP adresi oluşturma
      1. IP adresi 10.0.0.7 ASCS
         1. Yük Dengeleyici açın, ön uç IP havuzu seçin ve Ekle'ye tıklayın
         1. Yeni ön uç IP havuzunun adını girin (örneğin **nw1 ascs frontend**)
         1. Atama statik olarak ayarlamanız ve IP adresini girin (örneğin **10.0.0.7**)
         1. Tamam'a tıklayın
      1. ASCS Ağıranlar için IP adresi 10.0.0.8
         * Bir IP adresi için Ağıranlar oluşturmak için yukarıdaki adımları yineleyin (örneğin **10.0.0.8** ve **nw1 aers arka**)
   1. Arka uç havuzları oluşturma
      1. ASCS için arka uç havuzu oluşturma
         1. Yük Dengeleyici açın, arka uç havuzlarını seçin ve Ekle'ye tıklayın
         1. Yeni arka uç havuzunun adını girin (örneğin **nw1 ascs arka**)
         1. Bir sanal makine Ekle seçeneğine tıklayın.
         1. Daha önce oluşturduğunuz kullanılabilirlik kümesi seçin
         1. (A) sanal makineleri seçersiniz SCS küme
         1. Tamam'a tıklayın
      1. ASCS Ağıranlar için arka uç havuzu oluşturma
         * Arka uç havuzu için Ağıranlar oluşturmak için yukarıdaki adımları yineleyin (örneğin **nw1 aers arka**)
   1. Sistem durumu araştırmaları oluşturma
      1. Bağlantı noktası 620**00** ASCS için
         1. Yük Dengeleyici açın, sistem durumu araştırmaları seçin ve Ekle'ye tıklayın
         1. Yeni bir sistem durumu araştırma adını girin (örneğin **nw1 ascs hp**)
         1. TCP bağlantı noktası 620 protokolü olarak seçin**00**, aralığı 5 ve sağlıksız durum eşiği 2 tutun
         1. Tamam'a tıklayın
      1. Bağlantı noktası 621**02** ASCS Ağıranlar için
         * Cıların için durum araştırması oluşturmak için yukarıdaki adımları yineleyin (örneğin 621**02** ve **nw1 aers hp**)
   1. Yük Dengeleme kuralları
      1. 32**00** ASCS TCP
         1. Açık yük dengeleyici, Yük Dengeleme kuralları'nı seçin ve Ekle'ye tıklayın
         1. Yeni Yük Dengeleyici kuralı adını girin (örneğin **nw1 lb 3200**)
         1. Ön uç IP adresi, arka uç havuzu ve durum yoklaması, daha önce oluşturduğunuz seçin (örneğin **nw1 ascs frontend**)
         1. Protokol tutmak **TCP**, bağlantı noktasını girin **3200**
         1. 30 dakika boşta kalma zaman aşımı süresini artırın
         1. **Kayan IP etkinleştirdiğinizden emin olun**
         1. Tamam'a tıklayın
      1. ASCS için ek bağlantı noktaları
         * 36 bağlantı noktaları için yukarıdaki adımları yineleyin**00**, 39**00**, 81**00**, 5**00**13, 5**00**14, 5**00**16 ve ASCS TCP
      1. ASCS Ağıranlar için ek bağlantı noktaları
         * 33 bağlantı noktaları için yukarıdaki adımları yineleyin**02**, 5**02**13, 5**02**14, 5**02**16 ve ASCS Ağıranlar TCP

> [!IMPORTANT]
> Azure vm'lerinde Azure yük dengeleyicinin arkasına yerleştirilen TCP zaman damgaları etkinleştirmeyin. TCP zaman damgaları etkinleştirme, sistem durumu araştırmaları başarısız olmasına neden olur. Parametre kümesi **net.ipv4.tcp_timestamps** için **0**. Ayrıntılar için bkz. [yük dengeleyici sistem durumu araştırmalarının](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview).

### <a name="create-pacemaker-cluster"></a>Pacemaker kümesi oluşturma

Bağlantısındaki [SLES azure'da Red Hat Enterprise Linux üzerinde Pacemaker ayarlama](high-availability-guide-rhel-pacemaker.md) temel Pacemaker küme bu (A) SCS sunucusu.

### <a name="prepare-for-sap-netweaver-installation"></a>SAP NetWeaver yüklemesi için hazırlama

Aşağıdaki öğeler ile önek **[A]** - tüm düğümler için geçerli **[1]** - düğüm 1 yalnızca uygulanabilir veya **[2]** - yalnızca düğüm 2 için geçerlidir.

1. **[A]**  Kurulum ana bilgisayar adı çözümlemesi

   Bir DNS sunucusu kullanabilir veya/etc/hosts tüm düğümlerde değiştirin. Bu örnek/Etc/Hosts dosyasının nasıl kullanılacağını gösterir.
   IP adresi ve aşağıdaki komutlarda bulunan ana bilgisayar adını değiştirin

   <pre><code>sudo vi /etc/hosts
   </code></pre>

   / Etc/hosts aşağıdaki satırları ekleyin. IP adresi ve ana bilgisayar adını, ortamınızla eşleşecek şekilde değiştirin.

   <pre><code># IP addresses of the GlusterFS nodes
   <b>10.0.0.40 glust-0</b>
   <b>10.0.0.41 glust-1</b>
   <b>10.0.0.42 glust-2</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS
   <b>10.0.0.7 nw1-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS ERS
   <b>10.0.0.8 nw1-aers</b>
   </code></pre>

1. **[A]**  Paylaşılan dizinler oluşturma

   <pre><code>sudo mkdir -p /sapmnt/<b>NW1</b>
   sudo mkdir -p /usr/sap/trans
   sudo mkdir -p /usr/sap/<b>NW1</b>/SYS
   sudo mkdir -p /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   sudo mkdir -p /usr/sap/<b>NW1</b>/ERS<b>02</b>

   sudo chattr +i /sapmnt/<b>NW1</b>
   sudo chattr +i /usr/sap/trans
   sudo chattr +i /usr/sap/<b>NW1</b>/SYS
   sudo chattr +i /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   sudo chattr +i /usr/sap/<b>NW1</b>/ERS<b>02</b>
   </code></pre>

1. **[A]**  Yükleme GlusterFS istemci ve diğer gereksinimler

   <pre><code>sudo yum -y install glusterfs-fuse resource-agents resource-agents-sap
   </code></pre>

1. **[A]**  Kaynak aracıları sap sürümü denetimi

   Yüklü kaynak aracıları sap paketin sürümü en az olduğundan emin olun 3.9.5-124.el7
   <pre><code>sudo yum info resource-agents-sap
   
   # Loaded plugins: langpacks, product-id, search-disabled-repos
   # Repodata is over 2 weeks old. Install yum-cron? Or run: yum makecache fast
   # Installed Packages
   # Name        : resource-agents-sap
   # Arch        : x86_64
   # Version     : <b>3.9.5</b>
   # Release     : <b>124.el7</b>
   # Size        : 100 k
   # Repo        : installed
   # From repo   : rhel-sap-for-rhel-7-server-rpms
   # Summary     : SAP cluster resource agents and connector script
   # URL         : https://github.com/ClusterLabs/resource-agents
   # License     : GPLv2+
   # Description : The SAP resource agents and connector script interface with
   #          : Pacemaker to allow SAP instances to be managed in a cluster
   #          : environment.
   </code></pre>


1. **[A]**  Girdileri bağlama

   <pre><code>sudo vi /etc/fstab
   
   # Add the following lines to fstab, save and exit
   <b>glust-0</b>:/<b>NW1</b>-sapmnt /sapmnt/<b>NW1</b> glusterfs backup-volfile-servers=<b>glust-1:glust-2</b> 0 0
   <b>glust-0</b>:/<b>NW1</b>-trans /usr/sap/trans glusterfs backup-volfile-servers=<b>glust-1:glust-2</b> 0 0
   <b>glust-0</b>:/<b>NW1</b>-sys /usr/sap/<b>NW1</b>/SYS glusterfs backup-volfile-servers=<b>glust-1:glust-2</b> 0 0
   </code></pre>

   Yeni paylaşımları bağlayın

   <pre><code>sudo mount -a
   </code></pre>

1. **[A]**  Yapılandırma TAKAS dosyası

   <pre><code>sudo vi /etc/waagent.conf
   
   # Set the property ResourceDisk.EnableSwap to y
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>
   
   # Set the size of the SWAP file with property ResourceDisk.SwapSizeMB
   # The free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check the SWAP space with command swapon
   # Size of the swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   Değişikliği etkinleştirmek için aracıyı yeniden başlatın

   <pre><code>sudo service waagent restart
   </code></pre>

1. **[A]**  RHEL yapılandırma

   RHEL SAP Not açıklandığı gibi yapılandırın [2002167]

### <a name="installing-sap-netweaver-ascsers"></a>SAP NetWeaver ASCS/Ağıranlar yükleme

1. **[1]**  Sanal IP kaynak ve sistem durumu araştırması ASCS örneği oluşturma

   <pre><code>sudo pcs node standby <b>nw1-cl-1</b>
   
   sudo pcs resource create fs_<b>NW1</b>_ASCS Filesystem device='<b>glust-0</b>:/<b>NW1</b>-ascs' \
     directory='/usr/sap/<b>NW1</b>/ASCS<b>00</b>' fstype='glusterfs' \
     options='backup-volfile-servers=<b>glust-1:glust-2</b>' \
     --group g-<b>NW1</b>_ASCS
   
   sudo pcs resource create vip_<b>NW1</b>_ASCS IPaddr2 \
     ip=<b>10.0.0.7</b> cidr_netmask=<b>24</b> \
     --group g-<b>NW1</b>_ASCS
   
   sudo pcs resource create nc_<b>NW1</b>_ASCS azure-lb port=620<b>00</b> \
     --group g-<b>NW1</b>_ASCS
   </code></pre>

   Küme durumunun Tamam olduğunu ve tüm kaynakları başlatıldığından emin olun. Hangi düğümünde kaynaklarını çalıştıran önemli değildir.

   <pre><code>sudo pcs status
   
   # Node <b>nw1-cl-1</b>: standby
   # Online: [ <b>nw1-cl-0</b> ]
   #
   # Full list of resources:
   #
   # rsc_st_azure    (stonith:fence_azure_arm):      Started <b>nw1-cl-0</b>
   #  Resource Group: g-<b>NW1</b>_ASCS
   #      fs_<b>NW1</b>_ASCS        (ocf::heartbeat:Filesystem):    Started <b>nw1-cl-0</b>
   #      nc_<b>NW1</b>_ASCS        (ocf::heartbeat:azure-lb):      Started <b>nw1-cl-0</b>
   #      vip_<b>NW1</b>_ASCS       (ocf::heartbeat:IPaddr2):       Started <b>nw1-cl-0</b>
   </code></pre>

1. **[1]**  SAP NetWeaver ASCS yükleyin  

   Örneğin IP adresini yük dengeleyici ön uç yapılandırması ASCS için eşleşen bir sanal ana bilgisayar adı kullanarak ilk düğümü üzerinde kök olarak SAP NetWeaver ASCS yükleme <b>nw1 ascs</b>, <b>10.0.0.7</b> ve örnek, örneğin yük dengeleyici araştırması için kullandığınız numarası <b>00</b>.

   SAPINST_REMOTE_ACCESS_USER sapinst parametresi, bir kök olmayan kullanıcı için sapinst bağlanmasına izin vermek için kullanabilirsiniz.

   <pre><code># Allow access to SWPM. This rule is not permanent. If you reboot the machine, you have to run the command again.
   sudo firewall-cmd --zone=public  --add-port=4237/tcp
   
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   Bir alt klasör/usr/sap/oluşturmak yükleme başarısız olursa**NW1**/ASCS**00**, sahibi ve ASCS grubu yapmayı deneyin**00** klasörü ve yeniden deneyin.

   <pre><code>sudo chown nw1adm /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   sudo chgrp sapsys /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   </code></pre>

1. **[1]**  Sanal IP kaynak ve sistem durumu araştırması Ağıranlar örneği oluşturma

   <pre><code>sudo pcs node unstandby <b>nw1-cl-1</b>
   sudo pcs node standby <b>nw1-cl-0</b>
   
   sudo pcs resource create fs_<b>NW1</b>_AERS Filesystem device='<b>glust-0</b>:/<b>NW1</b>-aers' \
     directory='/usr/sap/<b>NW1</b>/ERS<b>02</b>' fstype='glusterfs' \
     options='backup-volfile-servers=<b>glust-1:glust-2</b>' \
    --group g-<b>NW1</b>_AERS
   
   sudo pcs resource create vip_<b>NW1</b>_AERS IPaddr2 \
     ip=<b>10.0.0.8</b> cidr_netmask=<b>24</b> \
    --group g-<b>NW1</b>_AERS
   
   sudo pcs resource create nc_<b>NW1</b>_AERS azure-lb port=621<b>02</b> \
    --group g-<b>NW1</b>_AERS
   </code></pre>
 
   Küme durumunun Tamam olduğunu ve tüm kaynakları başlatıldığından emin olun. Hangi düğümünde kaynaklarını çalıştıran önemli değildir.

   <pre><code>sudo pcs status
   
   # Node <b>nw1-cl-0</b>: standby
   # Online: [ <b>nw1-cl-1</b> ]
   #
   # Full list of resources:
   #
   # rsc_st_azure    (stonith:fence_azure_arm):      Started <b>nw1-cl-1</b>
   #  Resource Group: g-<b>NW1</b>_ASCS
   #      fs_<b>NW1</b>_ASCS        (ocf::heartbeat:Filesystem):    Started <b>nw1-cl-1</b>
   #      nc_<b>NW1</b>_ASCS        (ocf::heartbeat:azure-lb):      Started <b>nw1-cl-1</b>
   #      vip_<b>NW1</b>_ASCS       (ocf::heartbeat:IPaddr2):       Started <b>nw1-cl-1</b>
   #  Resource Group: g-<b>NW1</b>_AERS
   #      fs_<b>NW1</b>_AERS        (ocf::heartbeat:Filesystem):    Started <b>nw1-cl-1</b>
   #      nc_<b>NW1</b>_AERS        (ocf::heartbeat:azure-lb):      Started <b>nw1-cl-1</b>
   #      vip_<b>NW1</b>_AERS       (ocf::heartbeat:IPaddr2):       Started <b>nw1-cl-1</b>
   </code></pre>

1. **[2]**  SAP NetWeaver Ağıranlar yükleyin  

   Örneğin IP adresini yük dengeleyici ön uç yapılandırması Ağıranlar için eşleşen bir sanal ana bilgisayar adı kullanarak ikinci düğümü kök olarak SAP NetWeaver Ağıranlar yükleme <b>nw1 aers</b>, <b>10.0.0.8</b> ve örnek, örneğin yük dengeleyici araştırması için kullandığınız numarası <b>02</b>.

   SAPINST_REMOTE_ACCESS_USER sapinst parametresi, bir kök olmayan kullanıcı için sapinst bağlanmasına izin vermek için kullanabilirsiniz.

   <pre><code># Allow access to SWPM. This rule is not permanent. If you reboot the machine, you have to run the command again.
   sudo firewall-cmd --zone=public  --add-port=4237/tcp

   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   Bir alt klasör/usr/sap/oluşturmak yükleme başarısız olursa**NW1**/ERS**02**, sahibi ve Ağıranlar grubu yapmayı deneyin**02** klasörü ve yeniden deneyin.

   <pre><code>sudo chown nw1adm /usr/sap/<b>NW1</b>/ERS<b>02</b>
   sudo chgrp sapsys /usr/sap/<b>NW1</b>/ERS<b>02</b>
   </code></pre>

1. **[1]**  Adapt ASCS/SCS ve Ağıranlar örnek profilleri

   * ASCS/SCS profili

   <pre><code>sudo vi /sapmnt/<b>NW1</b>/profile/<b>NW1</b>_<b>ASCS00</b>_<b>nw1-ascs</b>
   
   # Change the restart command to a start command
   #Restart_Program_01 = local $(_EN) pf=$(_PF)
   Start_Program_01 = local $(_EN) pf=$(_PF)
   
   # Add the keep alive parameter
   enque/encni/set_so_keepalive = true
   </code></pre>

   * Cıların profili

   <pre><code>sudo vi /sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b>
   
   # Change the restart command to a start command
   #Restart_Program_00 = local $(_ER) pf=$(_PFL) NR=$(SCSID)
   Start_Program_00 = local $(_ER) pf=$(_PFL) NR=$(SCSID)
   
   # remove Autostart from ERS profile
   # Autostart = 1
   </code></pre>


1. **[A]**  Tutmayı yapılandırma

   SAP NetWeaver uygulama sunucusu ve ASCS/SCS arasındaki iletişimi, yazılım yük dengeleyici üzerinden yönlendirilir. Yük Dengeleyici, yapılandırılabilir bir zaman aşımından sonra etkin olmayan bağlantılarını keser. Bunu önlemek için SAP NetWeaver ASCS/SCS profilinde bir parametre ve Linux sistem ayarlarını değiştirmeniz gerekir. Okuma [SAP notu 1410736] [ 1410736] daha fazla bilgi için.

   ASCS/SCS profili parametresi enque/encni/set_so_keepalive, son adımda zaten eklendi.

   <pre><code># Change the Linux system configuration
   sudo sysctl net.ipv4.tcp_keepalive_time=120
   </code></pre>

1. **[A]**  /Usr/sap/sapservices dosyasını güncelleştirme

   Başlangıç örneklerinin sapinit başlangıç betiği tarafından önlemek için Pacemaker tarafından yönetilen tüm örnekler /usr/sap/sapservices dosyasından yorum gerekir. HANA üst düzey ile kullanılacaksa, SAP HANA örneği açıklama yok

   <pre><code>
   sudo vi /usr/sap/sapservices
   
   # On the node where you installed the ASCS, comment out the following line
   # LD_LIBRARY_PATH=/usr/sap/<b>NW1</b>/ASCS<b>00</b>/exe:$LD_LIBRARY_PATH; export LD_LIBRARY_PATH; /usr/sap/<b>NW1</b>/ASCS<b>00</b>/exe/sapstartsrv pf=/usr/sap/<b>NW1</b>/SYS/profile/<b>NW1</b>_ASCS<b>00</b>_<b>nw1-ascs</b> -D -u <b>nw1</b>adm
   
   # On the node where you installed the ERS, comment out the following line
   # LD_LIBRARY_PATH=/usr/sap/<b>NW1</b>/ERS<b>02</b>/exe:$LD_LIBRARY_PATH; export LD_LIBRARY_PATH; /usr/sap/<b>NW1</b>/ERS<b>02</b>/exe/sapstartsrv pf=/usr/sap/<b>NW1</b>/ERS<b>02</b>/profile/<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b> -D -u <b>nw1</b>adm
   </code></pre>

1. **[1]**  SAP küme kaynaklarını oluşturma

  Sıraya alma 1 sunucusu mimarisi (ENSA1) kullanıyorsanız, kaynakları gibi tanımlayın:

   <pre><code>sudo pcs property set maintenance-mode=true
   
   sudo pcs resource create rsc_sap_<b>NW1</b>_ASCS00 SAPInstance \
    InstanceName=<b>NW1</b>_ASCS00_<b>nw1-ascs</b> START_PROFILE="/sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ASCS00_<b>nw1-ascs</b>" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 migration-threshold=1 \
    --group g-<b>NW1</b>_ASCS
   
   sudo pcs resource create rsc_sap_<b>NW1</b>_ERS<b>02</b> SAPInstance \
    InstanceName=<b>NW1</b>_ERS02_<b>nw1-aers</b> START_PROFILE="/sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ERS02_<b>nw1-aers</b>" \
    AUTOMATIC_RECOVER=false IS_ERS=true \
    --group g-<b>NW1</b>_AERS
      
   sudo pcs constraint colocation add g-<b>NW1</b>_AERS with g-<b>NW1</b>_ASCS -5000
   sudo pcs constraint location rsc_sap_<b>NW1</b>_ASCS<b>00</b> rule score=2000 runs_ers_<b>NW1</b> eq 1
   sudo pcs constraint order g-<b>NW1</b>_ASCS then g-<b>NW1</b>_AERS kind=Optional symmetrical=false
   
   sudo pcs node unstandby <b>nw1-cl-0</b>
   sudo pcs property set maintenance-mode=false
   </code></pre>

   Kuyruğa sunucu çoğaltma, SAP KB 7.52 itibarıyla dahil olmak üzere 2 için sunulan destek SAP. Sıraya alma sunucu 2 ABAP Platform 1809 ile başlayarak, varsayılan olarak yüklenir. SAP bkz Not [2630416](https://launchpad.support.sap.com/#/notes/2630416) kuyruğa sunucu 2 desteği.
   Sıraya alma 2 sunucu mimarisi kullanıyorsanız ([ENSA2](https://help.sap.com/viewer/cff8531bc1d9416d91bb6781e628d4e0/1709%20001/en-US/6d655c383abf4c129b0e5c8683e7ecd8.html)), kaynak-Aracısı-sap-4.1.1-12.el7.x86_64 ya da daha yeni kaynak aracısını yükleyin ve kaynakları aşağıdaki gibi tanımlayın:

<pre><code>sudo pcs property set maintenance-mode=true
   
   sudo pcs resource create rsc_sap_<b>NW1</b>_ASCS00 SAPInstance \
    InstanceName=<b>NW1</b>_ASCS00_<b>nw1-ascs</b> START_PROFILE="/sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ASCS00_<b>nw1-ascs</b>" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 \
    --group g-<b>NW1</b>_ASCS
   
   sudo pcs resource create rsc_sap_<b>NW1</b>_ERS<b>02</b> SAPInstance \
    InstanceName=<b>NW1</b>_ERS02_<b>nw1-aers</b> START_PROFILE="/sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ERS02_<b>nw1-aers</b>" \
    AUTOMATIC_RECOVER=false IS_ERS=true \
    --group g-<b>NW1</b>_AERS
      
   sudo pcs constraint colocation add g-<b>NW1</b>_AERS with g-<b>NW1</b>_ASCS -5000
   sudo pcs constraint order g-<b>NW1</b>_ASCS then g-<b>NW1</b>_AERS kind=Optional symmetrical=false
   
   sudo pcs node unstandby <b>nw1-cl-0</b>
   sudo pcs property set maintenance-mode=false
   </code></pre>

   Eski bir sürümden yükseltme ve 2 kuyruğa sunucusuna geçiş SAP bkz Not [2641322](https://launchpad.support.sap.com/#/notes/2641322). 

   Küme durumunun Tamam olduğunu ve tüm kaynakları başlatıldığından emin olun. Hangi düğümünde kaynaklarını çalıştıran önemli değildir.

   <pre><code>sudo pcs status
   
   # Online: [ <b>nw1-cl-0</b> <b>nw1-cl-1</b> ]
   #
   # Full list of resources:
   #
   # rsc_st_azure    (stonith:fence_azure_arm):      Started <b>nw1-cl-0</b>
   #  Resource Group: g-<b>NW1</b>_ASCS
   #      fs_<b>NW1</b>_ASCS        (ocf::heartbeat:Filesystem):    Started <b>nw1-cl-1</b>
   #      nc_<b>NW1</b>_ASCS        (ocf::heartbeat:azure-lb):      Started <b>nw1-cl-1</b>
   #      vip_<b>NW1</b>_ASCS       (ocf::heartbeat:IPaddr2):       Started <b>nw1-cl-1</b>
   #      rsc_sap_<b>NW1</b>_ASCS00 (ocf::heartbeat:SAPInstance):   Started <b>nw1-cl-1</b>
   #  Resource Group: g-<b>NW1</b>_AERS
   #      fs_<b>NW1</b>_AERS        (ocf::heartbeat:Filesystem):    Started <b>nw1-cl-0</b>
   #      nc_<b>NW1</b>_AERS        (ocf::heartbeat:azure-lb):      Started <b>nw1-cl-0</b>
   #      vip_<b>NW1</b>_AERS       (ocf::heartbeat:IPaddr2):       Started <b>nw1-cl-0</b>
   #      rsc_sap_<b>NW1</b>_ERS02  (ocf::heartbeat:SAPInstance):   Started <b>nw1-cl-0</b>
   </code></pre>

1. **[A]**  Her iki düğümde ASCS ve Ağıranlar güvenlik duvarı kuralları ekleme

   <pre><code># Probe Port of ASCS
   sudo firewall-cmd --zone=public --add-port=620<b>00</b>/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=620<b>00</b>/tcp
   sudo firewall-cmd --zone=public --add-port=32<b>00</b>/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=32<b>00</b>/tcp
   sudo firewall-cmd --zone=public --add-port=36<b>00</b>/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=36<b>00</b>/tcp
   sudo firewall-cmd --zone=public --add-port=39<b>00</b>/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=39<b>00</b>/tcp
   sudo firewall-cmd --zone=public --add-port=81<b>00</b>/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=81<b>00</b>/tcp
   sudo firewall-cmd --zone=public --add-port=5<b>00</b>13/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=5<b>00</b>13/tcp
   sudo firewall-cmd --zone=public --add-port=5<b>00</b>14/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=5<b>00</b>14/tcp
   sudo firewall-cmd --zone=public --add-port=5<b>00</b>16/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=5<b>00</b>16/tcp
   # Probe Port of ERS
   sudo firewall-cmd --zone=public --add-port=621<b>02</b>/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=621<b>02</b>/tcp
   sudo firewall-cmd --zone=public --add-port=33<b>02</b>/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=33<b>02</b>/tcp
   sudo firewall-cmd --zone=public --add-port=5<b>02</b>13/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=5<b>02</b>13/tcp
   sudo firewall-cmd --zone=public --add-port=5<b>02</b>14/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=5<b>02</b>14/tcp
   sudo firewall-cmd --zone=public --add-port=5<b>02</b>16/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=5<b>02</b>16/tcp
   </code></pre>

## <a name="2d6008b0-685d-426c-b59e-6cd281fd45d7"></a>SAP NetWeaver uygulama sunucusu hazırlığı

Bazı veritabanları veritabanı örneği yüklemesi bir uygulama sunucusunda yürütülür gerektirir. Bu gibi durumlarda kullanılacak kullanabilmek için uygulama sunucusu sanal makine hazırlayın.

Adımları aşağıdaki varsayılır uygulama sunucusu ASCS/SCS ve HANA sunucularından farklı bir sunucuya yükleyin. Aksi takdirde (örneğin, ana bilgisayar adı çözümlemesi yapılandırma) adımlardan bazıları gerekli değildir.

1. Ana bilgisayar adı çözümlemesi Kurulumu

   Bir DNS sunucusu kullanabilir veya/etc/hosts tüm düğümlerde değiştirin. Bu örnek/Etc/Hosts dosyasının nasıl kullanılacağını gösterir.
   IP adresi ve aşağıdaki komutlarda bulunan ana bilgisayar adını değiştirin

   <pre><code>sudo vi /etc/hosts
   </code></pre>

   / Etc/hosts aşağıdaki satırları ekleyin. IP adresi ve ana bilgisayar adını, ortamınızla eşleşecek şekilde değiştirin.

   <pre><code># IP addresses of the GlusterFS nodes
   <b>10.0.0.40 glust-0</b>
   <b>10.0.0.41 glust-1</b>
   <b>10.0.0.42 glust-2</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS
   <b>10.0.0.7 nw1-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS ERS
   <b>10.0.0.8 nw1-aers</b>
   # IP address of the load balancer frontend configuration for database
   <b>10.0.0.13 nw1-db</b>
   </code></pre>

1. Sapmnt dizini oluşturma

   <pre><code>sudo mkdir -p /sapmnt/<b>NW1</b>
   sudo mkdir -p /usr/sap/trans

   sudo chattr +i /sapmnt/<b>NW1</b>
   sudo chattr +i /usr/sap/trans
   </code></pre>

1. GlusterFS istemci ve diğer gereksinimler

   <pre><code>sudo yum -y install glusterfs-fuse uuidd
   </code></pre>

1. Bağlama girişler ekleyin

   <pre><code>sudo vi /etc/fstab
   
   # Add the following lines to fstab, save and exit
   <b>glust-0</b>:/<b>NW1</b>-sapmnt /sapmnt/<b>NW1</b> glusterfs backup-volfile-servers=<b>glust-1:glust-2</b> 0 0
   <b>glust-0</b>:/<b>NW1</b>-trans /usr/sap/trans glusterfs backup-volfile-servers=<b>glust-1:glust-2</b> 0 0
   </code></pre>

   Yeni paylaşımları bağlayın

   <pre><code>sudo mount -a
   </code></pre>

1. TAKAS dosyası yapılandırma
 
   <pre><code>
   sudo vi /etc/waagent.conf
   
   # Set the property ResourceDisk.EnableSwap to y
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>
   
   # Set the size of the SWAP file with property ResourceDisk.SwapSizeMB
   # The free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check the SWAP space with command swapon
   # Size of the swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   Değişikliği etkinleştirmek için aracıyı yeniden başlatın

   <pre><code>
   sudo service waagent restart
   </code></pre>

## <a name="install-database"></a>Veritabanı Yükleme

Bu örnekte, SAP HANA'da SAP NetWeaver yüklenir. Bu yükleme için desteklenen her veritabanı kullanabilirsiniz. Azure'da SAP HANA yükleme hakkında daha fazla bilgi için bkz. [Red Hat Enterprise Linux Vm'lerinde Azure üzerinde SAP HANA yüksek kullanılabilirliğini][sap-hana-ha]. Desteklenen veritabanlarının bir listesi için bkz. [SAP notu 1928533][1928533].

1. SAP veritabanı örneğini yüklemeyi çalıştırın

   SAP NetWeaver veritabanı örneği örnek veritabanı için yük dengeleyici ön uç yapılandırması IP adresine eşleyen bir sanal ana bilgisayar adı kullanarak kök yükleme <b>nw1 db</b> ve <b>10.0.0.13</b>.

   SAPINST_REMOTE_ACCESS_USER sapinst parametresi, bir kök olmayan kullanıcı için sapinst bağlanmasına izin vermek için kullanabilirsiniz.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

## <a name="sap-netweaver-application-server-installation"></a>SAP NetWeaver uygulama sunucusu yükleme

SAP uygulama sunucusu yüklemek için aşağıdaki adımları izleyin.

1. Uygulama sunucusu hazırlama

   Bu bölümdeki adımları [SAP NetWeaver uygulama sunucusu hazırlığı](high-availability-guide-rhel.md#2d6008b0-685d-426c-b59e-6cd281fd45d7) uygulama sunucuyu hazırlamak için yukarıdaki.

1. SAP NetWeaver uygulama sunucusu yükleme

   Bir birincil veya ek SAP NetWeaver uygulamalarını sunucuya yükleyin.

   SAPINST_REMOTE_ACCESS_USER sapinst parametresi, bir kök olmayan kullanıcı için sapinst bağlanmasına izin vermek için kullanabilirsiniz.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. SAP HANA Güvenli Depo güncelleştirme

   SAP HANA Güvenli Depo SAP HANA sistem çoğaltması Kurulumu sanal adına işaret edecek şekilde güncelleştirin.

   Girişleri olarak listelemek için aşağıdaki komutu çalıştırın \<sapsid > adm

   <pre><code>hdbuserstore List
   </code></pre>

   Bu tüm girişleri listeler ve benzer şekilde görünmelidir
   <pre><code>
   DATA FILE       : /home/nw1adm/.hdb/nw1-di-0/SSFS_HDB.DAT
   KEY FILE        : /home/nw1adm/.hdb/nw1-di-0/SSFS_HDB.KEY
   
   KEY DEFAULT
     ENV : 10.0.0.14:<b>30313</b>
     USER: <b>SAPABAP1</b>
     DATABASE: <b>NW1</b>
   </code></pre>

   Çıktı, varsayılan giriş IP adresini sanal makineye ve load balancer'ın IP adresi işaret ettiğini gösterir. Bu girişi yük dengeleyicinin sanal ana bilgisayar için işaret edecek şekilde değiştirilmesi gerekir. Aynı bağlantı noktasını kullandığınızdan emin olun (**30313** yukarıdaki çıktıda) ve veritabanı adını (**HN1** yukarıdaki çıktıda)!

   <pre><code>su - <b>nw1</b>adm
   hdbuserstore SET DEFAULT <b>nw1-db</b>:<b>30313@NW1</b> <b>SAPABAP1</b> <b>&lt;password of ABAP schema&gt;</b>
   </code></pre>

## <a name="test-the-cluster-setup"></a>Test kümesi Kurulumu

1. ASCS örneği el ile geçirme

   Kaynak durumu, test başlamadan önce:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

   Kök olarak ASCS örneği geçirmek için aşağıdaki komutları çalıştırın.

   <pre><code>[root@nw1-cl-0 ~]# pcs resource move rsc_sap_NW1_ASCS00
   
   [root@nw1-cl-0 ~]# pcs resource clear rsc_sap_NW1_ASCS00
   
   # Remove failed actions for the ERS that occurred as part of the migration
   [root@nw1-cl-0 ~]# pcs resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   Kaynak durumu test sonra:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

1. Düğüm kilitlenmesi benzetimi

   Kaynak durumu, test başlamadan önce:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

   Düğümdeki kök olarak ASCS örneğinin çalıştığı aşağıdaki komutu çalıştırın

   <pre><code>[root@nw1-cl-1 ~]# echo b > /proc/sysrq-trigger
   </code></pre>

   Düğümü yeniden başlatıldıktan sonra durumu yeniden şu şekilde görünmelidir.

   <pre><code>Online: [ nw1-cl-0 nw1-cl-1 ]
   
   Full list of resources:
   
   rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   
   Failed Actions:
   * rsc_sap_NW1_ERS02_monitor_11000 on nw1-cl-0 'not running' (7): call=45, status=complete, exitreason='',
       last-rc-change='Tue Aug 21 13:52:39 2018', queued=0ms, exec=0ms
   </code></pre>

   Başarısız kaynakları temizlemek için aşağıdaki komutu kullanın.

   <pre><code>[root@nw1-cl-0 ~]# pcs resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   Kaynak durumu test sonra:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

1. İleti sunucu işlemi Sonlandır

   Kaynak durumu, test başlamadan önce:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

   Kök olarak ileti sunucusu işlemi belirleyip bunu sonlandırmak için aşağıdaki komutları çalıştırın.

   <pre><code>[root@nw1-cl-0 ~]# pgrep ms.sapNW1 | xargs kill -9
   </code></pre>

   Yalnızca ileti sunucusu kez KILL, sapstart tarafından başlatılacak. Bu genellikle yeterli Pacemaker sonunda olacak KILL ASCS örneği başka bir düğüme taşıyın. Kaynak durumunu ASCS ve Ağıranlar örneğinin sonra test temizlemek için kök olarak aşağıdaki komutları çalıştırın.

   <pre><code>[root@nw1-cl-0 ~]# pcs resource cleanup rsc_sap_NW1_ASCS00
   [root@nw1-cl-0 ~]# pcs resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   Kaynak durumu test sonra:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

1. Sıraya alma sunucu işlemi Sonlandır

   Kaynak durumu, test başlamadan önce:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

   Aşağıdaki komutları kök olarak, kuyruğa sunucunun sonlandırmak için ASCS örneğinin çalıştığı düğüm üzerinde çalıştırın.

   <pre><code>[root@nw1-cl-1 ~]# pgrep en.sapNW1 | xargs kill -9
   </code></pre>

   ASCS örneği hemen başka bir düğüme yük devretme. ASCS başlatıldıktan sonra Ağıranlar örneği de yük devretme. Kaynak durumunu ASCS ve Ağıranlar örneğinin sonra test temizlemek için kök olarak aşağıdaki komutları çalıştırın.

   <pre><code>[root@nw1-cl-0 ~]# pcs resource cleanup rsc_sap_NW1_ASCS00
   [root@nw1-cl-0 ~]# pcs resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   Kaynak durumu test sonra:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

1. Sıraya alma çoğaltma sunucusu işlemini sonlandır

   Kaynak durumu, test başlamadan önce:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

   Aşağıdaki komutu, sıraya alma çoğaltma sunucusu işlemi sonlandırmak için Ağıranlar örneğinin çalıştığı düğüm üzerinde kök olarak çalıştırın.

   <pre><code>[root@nw1-cl-1 ~]# pgrep er.sapNW1 | xargs kill -9
   </code></pre>

   Yalnızca komutu bir kez çalıştırırsanız, sapstart işlemini yeniden başlatır. Bunu çalıştırırsanız, genellikle yeterli sapstart işlem yeniden ve kaynak durdurulmuş durumda olacaktır. Kaynak durumunu Ağıranlar örneğinin sonra test temizlemek için kök olarak aşağıdaki komutları çalıştırın.

   <pre><code>[root@nw1-cl-0 ~]# pcs resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   Kaynak durumu test sonra:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

1. Sıraya alma sapstartsrv işlemini sonlandır

   Kaynak durumu, test başlamadan önce:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

   Aşağıdaki komutları kök olarak, ASCS çalıştığı düğüm üzerinde çalıştırın.

   <pre><code>[root@nw1-cl-0 ~]# pgrep -fl ASCS00.*sapstartsrv
   # 59545 sapstartsrv
   
   [root@nw1-cl-0 ~]# kill -9 59545
   </code></pre>

   İzleme işleminin parçası olarak Pacemaker kaynak aracısı tarafından sapstartsrv işlemi her zaman yeniden başlatılması gerekiyor. Kaynak durumu test sonra:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

## <a name="next-steps"></a>Sonraki adımlar

* [Azure sanal makineleri planlama ve uygulama için SAP][planning-guide]
* [SAP için Azure sanal makineler dağıtımı][deployment-guide]
* [SAP için Azure sanal makineleri DBMS dağıtım][dbms-guide]
* Yüksek kullanılabilirlik ve olağanüstü durum kurtarma SAP hana (büyük örnekler) azure'da planlama oluşturma hakkında bilgi almak için bkz: [SAP HANA (büyük örnekler) azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md).
* Yüksek kullanılabilirlik ve Azure Vm'leri üzerinde SAP hana olağanüstü durum kurtarma planı oluşturma hakkında bilgi almak için bkz: [SAP HANA, yüksek kullanılabilirlik Azure Virtual Machines'de (VM'ler)][sap-hana-ha]
