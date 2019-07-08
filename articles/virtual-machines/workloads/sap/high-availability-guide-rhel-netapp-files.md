---
title: Azure sanal makineler Red Hat Enterprise Linux Azure NetApp dosyaları ile SAP NetWeaver için yüksek kullanılabilirlik | Microsoft Docs
description: Azure sanal makineler Red Hat Enterprise Linux üzerinde SAP NetWeaver için yüksek kullanılabilirlik
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/14/2019
ms.author: radeltch
ms.openlocfilehash: 8679cfe54c8fb2c88b312f67ea9b2d7115cc479e
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503584"
---
# <a name="azure-virtual-machines-high-availability-for-sap-netweaver-on-red-hat-enterprise-linux-with-azure-netapp-files-for-sap-applications"></a>Azure sanal makineler Red Hat Enterprise Linux ile SAP uygulamaları için Azure NetApp dosya çubuğunda SAP NetWeaver için yüksek kullanılabilirlik

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[anf-azure-doc]:https://docs.microsoft.com/azure/azure-netapp-files/
[anf-avail-matrix]:https://azure.microsoft.com/global-infrastructure/services/?products=storage&regions=all
[anf-register]:https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-register
[anf-sap-applications-azure]:https://www.netapp.com/us/media/tr-4746.pdf

[2002167]: https://launchpad.support.sap.com/#/notes/2002167
[2009879]: https://launchpad.support.sap.com/#/notes/2009879
[1928533]: https://launchpad.support.sap.com/#/notes/1928533
[2015553]: https://launchpad.support.sap.com/#/notes/2015553
[2178632]: https://launchpad.support.sap.com/#/notes/2178632
[2191498]: https://launchpad.support.sap.com/#/notes/2191498
[2243692]: https://launchpad.support.sap.com/#/notes/2243692
[1999351]: https://launchpad.support.sap.com/#/notes/1999351
[1410736]:https://launchpad.support.sap.com/#/notes/1410736

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability-rhel.md
[glusterfs-ha]:high-availability-guide-rhel-glusterfs.md

Bu makalede sanal makineleri dağıtmak, sanal makineleri yapılandırma, küme Framework'ü yüklemek ve yüksek oranda kullanılabilir bir SAP NetWeaver 7.50 sistemini yüklemek nasıl kullanarak [Azure NetApp dosyaları](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction/).
Örnek yapılandırmalarında yükleme komutlarını vs. ASCS, sayı, 00, 01, birincil uygulama örneği (Pa'ları) 02 sayıdır (AAS) uygulama örneği 03 ise Ağıranlar örneği örneğidir. SAP sistemi kimliği QAS kullanılır. 

Veritabanı katmanı, bu makalede ayrıntılı kapsamında değildir.  

Öncelikle aşağıdaki SAP notları ve incelemeleri okuyun:

* [Azure dosyaları NetApp belgeleri][anf-azure-doc] 
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
* [NetApp Microsoft Azure NetApp dosyaları kullanarak Azure üzerinde SAP uygulamaları][anf-sap-applications-azure]

## <a name="overview"></a>Genel Bakış

Yüksek availability(HA) SAP Netweaver merkezi Hizmetleri için paylaşılan depolama gerektirir.
Red Hat Linux üzerinde elde etmek için şu ana kadar yüksek oranda kullanılabilir ayrı GlusterFS küme oluşturmak gerekli. 

Artık SAP Netweaver HA dağıtılan Azure NetApp dosya paylaşılan depolamayı kullanarak elde etmek mümkündür. Azure NetApp dosyaları için paylaşılan depolama ihtiyacını ortadan kaldırır. kullanarak ek [GlusterFS küme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-glusterfs). Pacemaker SAP Netweaver merkezi services(ASCS/SCS) yüksek kullanılabilirlik için hala gereklidir.

![SAP NetWeaver-yüksek kullanılabilirlik genel bakış](./media/high-availability-guide-rhel/high-availability-guide-rhel-anf.png)

SAP NetWeaver ASCS, SAP NetWeaver SCS, SAP NetWeaver Ağıranlar ve SAP HANA veritabanı sanal ana bilgisayar adı ve sanal IP adresleri kullanın. Azure üzerinde bir yük dengeleyici sanal IP adresi kullanmak için gereklidir. Aşağıdaki liste, (A) SCS ve Ağıranlar IP'ler ayrı ön yük dengeleyici yapılandırmasını gösterir.

> [!IMPORTANT]
> Azure sanal makinelerinde konuk işletim sistemi gibi çoklu SID, SAP ASCS/Ağıranlar Red Hat Linux ile kümeleme **desteklenmiyor**. Çoklu SID kümeleme Pacemaker kümedeki farklı SID'lere sahip birden çok SAP ASCS/Ağıranlar örneklerinin yüklenmesini açıklar.

### <a name="ascs"></a>(A)SCS

* Ön uç yapılandırması
  * IP adresi 192.168.14.9
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
  * IP adresi 192.168.14.10
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

## <a name="setting-up-the-azure-netapp-files-infrastructure"></a>Azure NetApp dosyaları altyapı Kurulumu 

SAP NetWeaver taşıma ve profil dizin için paylaşılan depolama gerektirir.  Azure NetApp dosya altyapısı için kurulum devam etmeden önce ile kendinizi alıştırın [Azure NetApp dosyaları belgeleri][anf-azure-doc]. Seçili Azure bölgeniz Azure NetApp dosyaları sağlayıp denetleyin. Aşağıdaki bağlantıda Azure NetApp dosyaları Azure bölgelere göre kullanılabilirliğini gösterir: [Azure bölgesi tarafından Azure NetApp dosyaları kullanılabilirlik][anf-avail-matrix].

Azure NetApp dosyaları kullanılabilir çeşitli [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/services/?products=netapp). Azure NetApp dosyaları dağıtmadan önce aşağıdaki Azure NetApp dosyaları ekleme isteği [kaydetmek için Azure NetApp dosya yönergeleri][anf-register]. 

### <a name="deploy-azure-netapp-files-resources"></a>NetApp dosya Azure kaynaklarını dağıtma  

Zaten dağıttıysanız, adımlarda varsayılır [Azure sanal ağı](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview). NetApp dosya Azure kaynakları ve Azure NetApp dosyaları kaynakları burada bağlanır sanal makinelerin aynı Azure sanal ağdaki veya eşlenmiş Azure sanal ağlarda bulunan dağıtılması gerekir.  

1. Bunu, zaten yapmadıysanız, istek [Azure NetApp dosyaları ekleme](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-register).  
2. Aşağıdaki seçili Azure bölgesinde, NetApp hesabı oluşturma [NetApp hesap oluşturmaya ilişkin yönergeler](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-netapp-account).  
3. Aşağıdaki Azure NetApp dosyaları kapasitesi havuzu oluşturmak [Azure NetApp dosyaları kapasitesi havuzu oluşturmak yönergeler](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-set-up-capacity-pool).  
Bu makalede sunulan SAP Netweaver mimari, tek Azure NetApp dosyaları kapasitesi havuzu, Premium SKU kullanır. SAP Netweaver uygulaması iş yükü azure'da Azure NetApp dosya Premium SKU öneririz.  
4. Bölümünde anlatıldığı gibi bir alt ağ Azure NetApp dosyaları için temsilci [yönergeleri temsilci bir alt ağ Azure NetApp dosyaları](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-delegate-subnet).  

5. Aşağıdaki Azure NetApp dosyaları birimleri dağıtma [Azure NetApp dosyaları için bir birim oluşturmak için yönergeler](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-volumes). Belirtilen Azure NetApp dosyaları birimlerin dağıtma [alt](https://docs.microsoft.com/rest/api/virtualnetwork/subnets). NetApp dosya Azure kaynakları ve Azure Vm'leri aynı Azure sanal ağdaki veya eşlenmiş Azure sanal ağlarda bulunan olmalıdır aklınızda bulundurun. Bu örnekte, iki Azure NetApp dosyaları birim kullanıyoruz: sap<b>QAS</b> ve transSAP. Karşılık gelen bir bağlama noktası bağlı dosya yolları /usrsap olan<b>qas</b>/sapmnt<b>QAS</b>, /usrsap<b>qas</b>/usrsap<b>QAS</b>sys VS.  

   1. Birim sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/sapmnt<b>QAS</b>)
   2. Birim sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/usrsap<b>QAS</b>ascs)
   3. Birim sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/usrsap<b>QAS</b>sys)
   4. Birim sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/usrsap<b>QAS</b>ağıranlar)
   5. Birim transSAP (nfs://192.168.24.4/transSAP)
   6. Birim sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/usrsap<b>QAS</b>pa'lar)
   7. Birim sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/usrsap<b>QAS</b>aas)
  
Bu örnekte, Azure NetApp dosyaları nasıl kullanılabileceğini göstermek için Azure NetApp dosyaları tüm SAP Netweaver dosya sistemleri için kullanılır. NFS bağlanması gerekmez SAP dosya sistemleri gibi da dağıtılabilir [Azure disk depolama](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#premium-ssd) . Bu örnekte <b>a-e</b> Azure NetApp dosya olmalıdır ve <b>f-g</b> (diğer bir deyişle, / usr/sap/<b>QAS</b>/D<b>02</b>, /usr/sap/<b>QAS </b>/D<b>03</b>) Azure disk depolama alanı olarak dağıtılabilir. 

### <a name="important-considerations"></a>Önemli noktalar

SAP Netweaver SUSE yüksek kullanılabilirlik mimarisi için Azure NetApp dosyaları dikkate alındığında, aşağıdaki önemli hususlara dikkat edin:

- 4 TiB kapasite alt sınırı havuzudur. Kapasitesi havuzu boyutu 4 TiB'ın katları şeklinde olmalıdır.
- En küçük birimdir 100 GiB
- Azure NetApp dosyaları ve burada Azure NetApp dosyaları birimleri bağlanır, tüm sanal makineler, aynı Azure sanal ağı veya olmalıdır [sanal ağlar eşlenmiş](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) aynı bölgede. VNET eşlemesi aynı bölgedeki üzerinden Azure NetApp dosya erişim artık desteklenmektedir. Genel eşleme üzerinden azure NetApp erişim henüz desteklenmiyor.
- Seçilen sanal ağ, Azure için NetApp dosyaları temsilcisi, bir alt ağ olması gerekir.
- Azure NetApp dosyaları şu anda yalnızca NFSv3 destekler 
- Azure NetApp dosyaları sunar [ilkeyi dışarı](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-configure-export-policy): izin verilen istemciler (okuma ve yazma, salt okunur, vs.) erişim türü denetleyebilirsiniz. 
- Azure NetApp dosyaları özelliği henüz bölge farkında değildir. Şu anda Azure NetApp dosyaları özelliği, bir Azure bölgesindeki tüm kullanılabilirlik alanlarında dağıtılan değil. Bazı Azure bölgelerinde olası gecikme etkileri farkında olun. 

## <a name="setting-up-ascs"></a>(A) SCS ayarlama

Bu örnekte, kaynakları el ile aracılığıyla dağıtılan [Azure portalında](https://portal.azure.com/#home).

### <a name="deploy-linux-manually-via-azure-portal"></a>Linux Azure Portalı aracılığıyla el ile dağıtma

İlk Azure NetApp dosyaları birimler oluşturmak gerekir. Vm'leri dağıtın. Ardından, yük dengeleyici oluşturma ve arka uç havuzlarında sanal makinelerini kullanın.

1. Bir yük dengeleyiciye (dahili) oluşturma  
   1. Ön uç IP adresi oluşturma
      1. IP adresi 192.168.14.9 ASCS
         1. Yük Dengeleyici açın, ön uç IP havuzu seçin ve Ekle'ye tıklayın
         1. Yeni ön uç IP havuzunun adını girin (örneğin **ön uç. QAS. ASCS**)
         1. Atama statik olarak ayarlamanız ve IP adresini girin (örneğin **192.168.14.9**)
         1. Tamam'a tıklayın
      1. IP adresi 192.168.14.10 ASCS Ağıranlar
         * Yukarıdaki adımları altında Ağıranlar için bir IP adresi oluşturmak için "a" (örneğin **192.168.14.10** ve **ön uç. QAS. Cıların**)
   1. Arka uç havuzları oluşturma
      1. ASCS için arka uç havuzu oluşturma
         1. Yük Dengeleyici açın, arka uç havuzlarını seçin ve Ekle'ye tıklayın
         1. Yeni arka uç havuzunun adını girin (örneğin **arka uç. QAS**)
         1. Bir sanal makine Ekle seçeneğine tıklayın.
         1. ASCS için daha önce oluşturduğunuz kullanılabilirlik kümesi seçin 
         1. (A) sanal makineleri seçersiniz SCS küme
         1. Tamam'a tıklayın
   1. Sistem durumu araştırmaları oluşturma
      1. Bağlantı noktası 620**00** ASCS için
         1. Yük Dengeleyici açın, sistem durumu araştırmaları seçin ve Ekle'ye tıklayın
         1. Yeni bir sistem durumu araştırma adını girin (örneğin **sistem durumu. QAS. ASCS**)
         1. TCP bağlantı noktası 620 protokolü olarak seçin**00**, aralığı 5 ve sağlıksız durum eşiği 2 tutun
         1. Tamam'a tıklayın
      1. Bağlantı noktası 621**01** ASCS Ağıranlar için
            * Yukarıdaki adımları Ağıranlar için durum araştırması oluşturmak için "c" altında (örneğin 621**01** ve **sistem durumu. QAS. Cıların**)
   1. Yük Dengeleme kuralları
      1. 32**00** ASCS TCP
         1. Açık yük dengeleyici, Yük Dengeleme kuralları'nı seçin ve Ekle'ye tıklayın
         1. Yeni Yük Dengeleyici kuralı adını girin (örneğin **lb. QAS. ASCS.3200**)
         1. ASCS, arka uç havuzu ve önceden oluşturduğunuz sistem durumu araştırması için ön uç IP adresi seçin (örneğin **ön uç. QAS. ASCS**)
         1. Protokol tutmak **TCP**, bağlantı noktasını girin **3200**
         1. 30 dakika boşta kalma zaman aşımı süresini artırın
         1. **Kayan IP etkinleştirdiğinizden emin olun**
         1. Tamam'a tıklayın
      1. ASCS için ek bağlantı noktaları
         * 36 bağlantı noktaları için "d" altında yukarıdaki adımları yineleyin**00**, 39**00**, 81**00**, 5**00**13, 5**00**145**00**16 ve ASCS TCP
      1. ASCS Ağıranlar için ek bağlantı noktaları
         * Yukarıdaki adımları "d" altında 32 bağlantı noktaları için yineleyin**01**, 33**01**, 5**01**13, 5**01**14, 5**01**16 ve TCP ASCS Ağıranlar için


> [!IMPORTANT]
> Azure vm'lerinde Azure yük dengeleyicinin arkasına yerleştirilen TCP zaman damgaları etkinleştirmeyin. TCP zaman damgaları etkinleştirme, sistem durumu araştırmaları başarısız olmasına neden olur. Parametre kümesi **net.ipv4.tcp_timestamps** için **0**. Ayrıntılar için bkz. [yük dengeleyici sistem durumu araştırmalarının](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview).

### <a name="create-pacemaker-cluster"></a>Pacemaker kümesi oluşturma

Bağlantısındaki [SLES azure'da Red Hat Enterprise Linux üzerinde Pacemaker ayarlama](high-availability-guide-rhel-pacemaker.md) temel Pacemaker küme bu (A) SCS sunucusu.

### <a name="prepare-for-sap-netweaver-installation"></a>SAP NetWeaver yüklemesi için hazırlama

Aşağıdaki öğeler ile önek **[A]** - tüm düğümler için geçerli **[1]** - düğüm 1 yalnızca uygulanabilir veya **[2]** - yalnızca düğüm 2 için geçerlidir.

1. **[A]**  Kurulum ana bilgisayar adı çözümlemesi

   Bir DNS sunucusu kullanabilir veya/etc/hosts tüm düğümlerde değiştirin. Bu örnek/Etc/Hosts dosyasının nasıl kullanılacağını gösterir.
   IP adresi ve aşağıdaki komutlarda bulunan ana bilgisayar adını değiştirin

    ```
    sudo vi /etc/hosts
    ```

   / Etc/hosts aşağıdaki satırları ekleyin. IP adresi ve ana bilgisayar adını, ortamınızla eşleşecek şekilde değiştirin.

    ```
    # IP address of cluster node 1
    192.168.14.5    anftstsapcl1
    # IP address of cluster node 2
    192.168.14.6     anftstsapcl2
    # IP address of the load balancer frontend configuration for SAP Netweaver ASCS
    192.168.14.9    anftstsapvh
    # IP address of the load balancer frontend configuration for SAP Netweaver ERS
    192.168.14.10    anftstsapers
    ```

1. **[1]**  Azure NetApp dosyaları birimindeki oluşturma SAP dizinleri.  
   Geçici olarak Azure NetApp dosyaları birimi Vm'lerden birinin bağlamak ve SAP dizinleri (dosya yolları) oluşturun.  

    ```
     #mount temporarily the volume
     sudo mkdir -p /saptmp
     sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=3,tcp 192.168.24.5:/sapQAS /saptmp
     # create the SAP directories
     sudo cd /saptmp
     sudo mkdir -p sapmntQAS
     sudo mkdir -p usrsapQASascs
     sudo mkdir -p usrsapQASers
     sudo mkdir -p usrsapQASsys
     sudo mkdir -p usrsapQASpas
     sudo mkdir -p usrsapQASaas
     # unmount the volume and delete the temporary directory
     sudo cd ..
     sudo umount /saptmp
     sudo rmdir /saptmp

1. **[A]** Create the shared directories

   ```
   sudo mkdir -p/sapmnt/QAS sudo mkdir -p /usr/sap/trans sudo mkdir -p /usr/sap/QAS/SYS sudo mkdir -p /usr/sap/QAS/ASCS00 sudo mkdir -p /usr/sap/QAS/ERS01
   
   sudo chattr + miyim/sapmnt/QAS sudo chattr + ı /usr/sap/trans sudo chattr + ı /usr/sap/QAS/SYS sudo chattr + ı /usr/sap/QAS/ASCS00 sudo chattr + miyim /usr/sap/QAS/ERS01
   ```

1. **[A]** Install NFS client and other requirements

   ```
   sudo yum -y nfs-utils kaynak aracıları kaynak-Aracısı-sap yükleme
   ```

1. **[A]** Check version of resource-agents-sap

   Make sure that the version of the installed resource-agents-sap package is at least 3.9.5-124.el7
   ```
   sudo yum bilgisi kaynak-Aracısı-sap
   
   # <a name="loaded-plugins-langpacks-product-id-search-disabled-repos"></a>Yüklü eklentiler: langpacks, ürün kimliği, arama devre dışı depoları
   # <a name="repodata-is-over-2-weeks-old-install-yum-cron-or-run-yum-makecache-fast"></a>Repodata üzerinde 2 hafta eski olur. Yum cron yüklensin mi? Veya çalıştırın: yum makecache hızlı
   # <a name="installed-packages"></a>Yüklü paketler
   # <a name="name---------resource-agents-sap"></a>Adı: kaynak aracıları sap
   # <a name="arch---------x8664"></a>Arch: x86_64
   # <a name="version------395"></a>Sürüm: 3.9.5
   # <a name="release------124el7"></a>Sürüm: 124.el7
   # <a name="size---------100-k"></a>Boyut: 100 k
   # <a name="repo---------installed"></a>Depo: yüklü
   # <a name="from-repo----rhel-sap-for-rhel-7-server-rpms"></a>Deposundan: rhel-sap-for-rhel-7-server-rpms
   # <a name="summary------sap-cluster-resource-agents-and-connector-script"></a>Özet: SAP küme kaynak aracılar ve Bağlayıcısı betiğiniz
   # <a name="url----------httpsgithubcomclusterlabsresource-agents"></a>URL: https://github.com/ClusterLabs/resource-agents
   # <a name="license------gplv2"></a>Lisans: GPLv2 +
   # <a name="description--the-sap-resource-agents-and-connector-script-interface-with"></a>Açıklama: Bağlayıcı komut arabirimi ve SAP kaynak aracıları
   #          <a name="-pacemaker-to-allow-sap-instances-to-be-managed-in-a-cluster"></a>: Bir kümede yönetilmek üzere SAP örnekleri izin vermek için pacemaker
   #          <a name="-environment"></a>: ortam.
   ```


1. **[A]** Add mount entries

   ```
   sudo VI/etc/fstab
   
   # <a name="add-the-following-lines-to-fstab-save-and-exit"></a>Fstab için aşağıdaki satırları ekleyin, Kaydet ve Çık
    192.168.24.5:/sapQAS/sapmntQAS/sapmnt/QAS nfs rw, sabit, rsize 65536, wsize = 65536, = vers = 3 192.168.24.5:/sapQAS/usrsapQASsys /usr/sap/QAS/SYS nfs rw, sabit, rsize 65536, wsize = 65536, = vers = 3 192.168.24.4:/transSAP /usr/sap/trans nfs rw, sabit, rsize = 65536, wsize 65536, = vers = 3
   ```

   Mount the new shares

   ```
   sudo mount - a  
   ```

1. **[A]** Configure SWAP file

   ```
   sudo VI /etc/waagent.conf
   
   # <a name="set-the-property-resourcediskenableswap-to-y"></a>' % S'özelliği ResourceDisk.EnableSwap y ayarlayın
   # <a name="create-and-use-swapfile-on-resource-disk"></a>Oluşturun ve kaynak diski üzerinde takas dosyası kullanın.
   ResourceDisk.EnableSwap=y
   
   # <a name="set-the-size-of-the-swap-file-with-property-resourcediskswapsizemb"></a>TAKAS dosyası ResourceDisk.SwapSizeMB özelliğiyle boyutunu ayarlama
   # <a name="the-free-space-of-resource-disk-varies-by-virtual-machine-size-make-sure-that-you-do-not-set-a-value-that-is-too-big-you-can-check-the-swap-space-with-command-swapon"></a>Kaynak disk boş alanı, sanal makine boyutuna göre değişir. Çok büyük bir değere ayarlı değil emin olun. Komut swapon ile TAKAS alanı kontrol edebilirsiniz
   # <a name="size-of-the-swapfile"></a>Takas dosyası boyutu.
   ResourceDisk.SwapSizeMB=2000
   ```

   Restart the Agent to activate the change

   ```
   sudo waagent yeniden başlatma
   ```

1. **[A]** RHEL configuration

   Configure RHEL as described in SAP Note [2002167]

### Installing SAP NetWeaver ASCS/ERS

1. **[1]** Create a virtual IP resource and health-probe for the ASCS instance

   ```
   sudo bilgisayarları düğüm bekleme anftstsapcl2
   
   sudo bilgisayarlar kaynak oluşturma fs_QAS_ASCS dosya sistemi device='192.168.24.5:/sapQAS/usrsapQASascs' \
     Dizin = '/ usr/sap/QAS/ASCS00' fstype 'nfs' = \
     --QAS_ASCS g grubu
   
   sudo bilgisayarlar kaynak oluşturma vip_QAS_ASCS IPaddr2 \
     ip=192.168.14.9 cidr_netmask=24 \
     --QAS_ASCS g grubu
   
   sudo bilgisayarlar kaynak oluşturma nc_QAS_ASCS azure lb bağlantı noktası 62000 = \
     --QAS_ASCS g grubu
   ```

   Make sure that the cluster status is ok and that all resources are started. It is not important on which node the resources are running.

   ```
   sudo bilgisayarları durumu
   
   # <a name="node-anftstsapcl2-standby"></a>Düğüm anftstsapcl2: bekleme
   # <a name="online--anftstsapcl1-"></a>Çevrimiçi: [anftstsapcl1]
   #
   # <a name="full-list-of-resources"></a>Kaynakların tam listesi:
   #
   # <a name="rscstazure----stonithfenceazurearm------started-anftstsapcl1"></a>rsc_st_azure    (stonith:fence_azure_arm):      Başlatılan anftstsapcl1
   #  <a name="resource-group-g-qasascs"></a>Kaynak grubu: g-QAS_ASCS
   #      <a name="fsqasascs--------ocfheartbeatfilesystem----started-anftstsapcl1"></a>fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Başlatılan anftstsapcl1
   #      <a name="ncqasascs--------ocfheartbeatazure-lb------started-anftstsapcl1"></a>nc_QAS_ASCS (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl1
   #      <a name="vipqasascs-------ocfheartbeatipaddr2-------started-anftstsapcl1"></a>vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Başlatılan anftstsapcl1
   ```

1. **[1]** Install SAP NetWeaver ASCS  

   Install SAP NetWeaver ASCS as root on the first node using a virtual hostname that maps to the IP address of the load balancer frontend configuration for the ASCS, for example <b>anftstsapvh</b>, <b>192.168.14.9</b> and the instance number that you used for the probe of the load balancer, for example <b>00</b>.

   You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.

   ```
   # <a name="allow-access-to-swpm-this-rule-is-not-permanent-if-you-reboot-the-machine-you-have-to-run-the-command-again"></a>SWPM erişime izin verin. Bu kural, kalıcı değildir. Makineyi yeniden başlatın, komutu yeniden çalıştırın gerekir.
   sudo güvenlik duvarı-cmd--bölge ortak---port = 4237/tcp =
   
   sudo <swpm>/sapinst SAPINST_REMOTE_ACCESS_USER sapadmin SAPINST_USE_HOSTNAME = < virtual_hostname > =
   ```

   If the installation fails to create a subfolder in /usr/sap/**QAS**/ASCS**00**, try setting the owner and group of the ASCS**00** folder and retry.

   ```
   sudo chown qasadm /usr/sap/QAS/ASCS00 sudo chgrp sapsys /usr/sap/QAS/ASCS00
   ```

1. **[1]** Create a virtual IP resource and health-probe for the ERS instance

   ```
   sudo bilgisayarları düğüm unstandby anftstsapcl2 sudo bilgisayarları düğüm bekleme anftstsapcl1
   
   sudo bilgisayarlar kaynak oluşturma fs_QAS_AERS dosya sistemi device='192.168.24.5:/sapQAS/usrsapQASers' \
     Dizin = '/ usr/sap/QAS/ERS01' fstype 'nfs' = \
    --QAS_AERS g grubu

   sudo pcs resource create vip_QAS_AERS IPaddr2 \
     IP 192.168.14.10 = cidr_netmask = 24 \
    --QAS_AERS g grubu
   
   sudo bilgisayarlar kaynak nc_QAS_AERS azure lb bağlantı noktası oluşturma 62101 = \
    --QAS_AERS g grubu
   ```
 
   Make sure that the cluster status is ok and that all resources are started. It is not important on which node the resources are running.

   ```
   sudo bilgisayarları durumu
   
   # <a name="node-anftstsapcl1-standby"></a>Düğüm anftstsapcl1: bekleme
   # <a name="online--anftstsapcl2-"></a>Çevrimiçi: [anftstsapcl2]
   #
   # <a name="full-list-of-resources"></a>Kaynakların tam listesi:
   #
   # <a name="rscstazure----stonithfenceazurearm------started-anftstsapcl2"></a>rsc_st_azure    (stonith:fence_azure_arm):      Başlatılan anftstsapcl2
   #  <a name="resource-group-g-qasascs"></a>Kaynak grubu: g-QAS_ASCS
   #      <a name="fsqasascs--------ocfheartbeatfilesystem----started-anftstsapcl2"></a>fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Başlatılan anftstsapcl2
   #      <a name="ncqasascs--------ocfheartbeatazure-lb------started-anftstsapcl2"></a>nc_QAS_ASCS (ocf::heartbeat:azure-lb):      Anftstsapcl2 çalışmaya <
   #      <a name="vipqasascs-------ocfheartbeatipaddr2-------started-anftstsapcl2"></a>vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Başlatılan anftstsapcl2
   #  <a name="resource-group-g-qasaers"></a>Kaynak grubu: g-QAS_AERS
   #      <a name="fsqasaers--------ocfheartbeatfilesystem----started-anftstsapcl2"></a>fs_QAS_AERS (ocf::heartbeat:Filesystem):    Başlatılan anftstsapcl2
   #      <a name="ncqasaers--------ocfheartbeatazure-lb------started-anftstsapcl2"></a>nc_QAS_AERS (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl2
   #      <a name="vipqasaers-------ocfheartbeatipaddr2-------started-anftstsapcl2"></a>vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Başlatılan anftstsapcl2
   ```

1. **[2]** Install SAP NetWeaver ERS  

   Install SAP NetWeaver ERS as root on the second node using a virtual hostname that maps to the IP address of the load balancer frontend configuration for the ERS, for example <b>anftstsapers</b>, <b>192.168.14.10</b> and the instance number that you used for the probe of the load balancer, for example <b>01</b>.

   You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.

   ```
   # <a name="allow-access-to-swpm-this-rule-is-not-permanent-if-you-reboot-the-machine-you-have-to-run-the-command-again"></a>SWPM erişime izin verin. Bu kural, kalıcı değildir. Makineyi yeniden başlatın, komutu yeniden çalıştırın gerekir.
   sudo güvenlik duvarı-cmd--bölge ortak---port = 4237/tcp =

   sudo <swpm>/sapinst SAPINST_REMOTE_ACCESS_USER sapadmin SAPINST_USE_HOSTNAME = < virtual_hostname > =
   ```

   If the installation fails to create a subfolder in /usr/sap/**QAS**/ERS**01**, try setting the owner and group of the ERS**01** folder and retry.

   ```
   sudo chown qaadm /usr/sap/QAS/ERS01 sudo chgrp sapsys /usr/sap/QAS/ERS01
   ```

1. **[1]** Adapt the ASCS/SCS and ERS instance profiles

   * ASCS/SCS profile

   ```
   sudo VI /sapmnt/QAS/profile/QAS_ASCS00_anftstsapvh
   
   # <a name="change-the-restart-command-to-a-start-command"></a>Yeniden başlatma komutu değiştirmek için bir başlangıç komutu
   #<a name="restartprogram01--local-en-pfpf"></a>Restart_Program_01 yerel $(_EN) pf=$(_PF) =
   Start_Program_01 yerel $(_EN) pf=$(_PF) =
   
   # <a name="add-the-keep-alive-parameter"></a>Canlı tutma parametre Ekle
   encni/enque/set_so_keepalive = true
   ```

   * ERS profile

   ```
   sudo VI /sapmnt/QAS/profile/QAS_ERS01_anftstsapers
   
   # <a name="change-the-restart-command-to-a-start-command"></a>Yeniden başlatma komutu değiştirmek için bir başlangıç komutu
   #<a name="restartprogram00--local-er-pfpfl-nrscsid"></a>Restart_Program_00 yerel $(_ER) pf=$(_PFL) NR=$(SCSID) =
   Start_Program_00 yerel $(_ER) pf=$(_PFL) NR=$(SCSID) =
   
   # <a name="remove-autostart-from-ers-profile"></a>AutoStart Ağıranlar profilinden kaldırın
   # <a name="autostart--1"></a>AutoStart = 1
   ```


1. **[A]** Configure Keep Alive

   The communication between the SAP NetWeaver application server and the ASCS/SCS is routed through a software load balancer. The load balancer disconnects inactive connections after a configurable timeout. To prevent this, you need to set a parameter in the SAP NetWeaver ASCS/SCS profile and change the Linux system settings. Read [SAP Note 1410736][1410736] for more information.

   The ASCS/SCS profile parameter enque/encni/set_so_keepalive was already added in the last step.

   ```
   # <a name="change-the-linux-system-configuration"></a>Linux sistem yapılandırmasını değiştirmek
   sudo sysctl net.ipv4.tcp_keepalive_time=120
   ```

1. **[A]** Update the /usr/sap/sapservices file

   To prevent the start of the instances by the sapinit startup script, all instances managed by Pacemaker must be commented out from /usr/sap/sapservices file. Do not comment out the SAP HANA instance if it will be used with HANA SR.

   ```
   sudo VI /usr/sap/sapservices
   
   # <a name="on-the-node-where-you-installed-the-ascs-comment-out-the-following-line"></a>ASCS yüklediğiniz düğümde aşağıdaki satırı açıklama satırı yapın.
   # <a name="ldlibrarypathusrsapqasascs00exeldlibrarypath-export-ldlibrarypath-usrsapqasascs00exesapstartsrv-pfusrsapqassysprofileqasascs00anftstsapvh--d--u-qasadm"></a>LD_LIBRARY_PATH = / usr/sap/QAS/ASCS00/exe: $LD_LIBRARY_PATH; dışarı aktarma LD_LIBRARY_PATH; /usr/SAP/QAS/ASCS00/exe/sapstartsrv pf = / usr/sap/QAS/SYS/profile/QAS_ASCS00_anftstsapvh - D -u qasadm
   
   # <a name="on-the-node-where-you-installed-the-ers-comment-out-the-following-line"></a>Ağıranlar yüklediğiniz düğümde aşağıdaki satırı açıklama satırı yapın.
   # <a name="ldlibrarypathusrsapqasers01exeldlibrarypath-export-ldlibrarypath-usrsapqasers01exesapstartsrv-pfusrsapqasers01profileqasers01anftstsapers--d--u-qasadm"></a>LD_LIBRARY_PATH = / usr/sap/QAS/ERS01/exe: $LD_LIBRARY_PATH; dışarı aktarma LD_LIBRARY_PATH; /usr/SAP/QAS/ERS01/exe/sapstartsrv pf = / usr/sap/QAS/ERS01/profile/QAS_ERS01_anftstsapers - D -u qasadm
   ```

1. **[1]** Create the SAP cluster resources

   If using enqueue server 1 architecture (ENSA1), define the resources as follows:

   ```
   sudo bilgisayarları özelliğini ayarlayın Bakım modu = true
   
    sudo pcs resource create rsc_sap_QAS_ASCS00 SAPInstance \
    InstanceName QAS_ASCS00_anftstsapvh START_PROFILE = = "/ sapmnt/QAS/profile/QAS_ASCS00_anftstsapvh" \
    AUTOMATIC_RECOVER=false \
    Kaynak meta-süreklilik 5000 geçiş eşiği = 1 = \
    --QAS_ASCS g grubu
   
    sudo pcs resource create rsc_sap_QAS_ERS01 SAPInstance \
    InstanceName QAS_ERS01_anftstsapers START_PROFILE = = "/ sapmnt/QAS/profile/QAS_ERS01_anftstsapers" \
    AUTOMATIC_RECOVER false IS_ERS = true = \
    --QAS_AERS g grubu
      
    sudo bilgisayarları kısıtlaması birlikte bulundurma ekleme g-QAS_AERS g-QAS_ASCS-5000 sudo bilgisayarları kısıtlaması konumu rsc_sap_QAS_ASCS00 kural puanı 2000 runs_ers_QAS eq 1 sudo bilgisayarları kısıtlama sipariş g QAS_ASCS sonra g QAS_AERS türü = isteğe bağlı simetrik = = false
    
    sudo bilgisayarları düğüm unstandby anftstsapcl1 sudo bilgisayarları özelliği ayarlayın Bakım modu = false
    ```

   SAP introduced support for enqueue server 2, including replication, as of SAP NW 7.52. Starting with ABAP Platform 1809, enqueue server 2 is installed by default. See SAP note [2630416](https://launchpad.support.sap.com/#/notes/2630416) for enqueue server 2 support.
   If using enqueue server 2 architecture ([ENSA2](https://help.sap.com/viewer/cff8531bc1d9416d91bb6781e628d4e0/1709%20001/en-US/6d655c383abf4c129b0e5c8683e7ecd8.html)), install resource agent resource-agents-sap-4.1.1-12.el7.x86_64 or newer and define the resources as follows:

    ```
    sudo bilgisayarları özelliğini ayarlayın Bakım modu = true
    
    sudo pcs resource create rsc_sap_QAS_ASCS00 SAPInstance \
    InstanceName QAS_ASCS00_anftstsapvh START_PROFILE = = "/ sapmnt/QAS/profile/QAS_ASCS00_anftstsapvh" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 \
    --QAS_ASCS g grubu
   
    sudo pcs resource create rsc_sap_QAS_ERS01 SAPInstance \
    InstanceName QAS_ERS01_anftstsapers START_PROFILE = = "/ sapmnt/QAS/profile/QAS_ERS01_anftstsapers" \
    AUTOMATIC_RECOVER false IS_ERS = true = \
    --QAS_AERS g grubu
      
    sudo bilgisayarları kısıtlaması birlikte bulundurma ekleme g-QAS_AERS g QAS_ASCS sonra g QAS_AERS g-QAS_ASCS-5000 sudo bilgisayarları kısıtlama siparişi türü ile isteğe bağlı simetrik = = false
   
    sudo bilgisayarları düğüm unstandby anftstsapcl1 sudo bilgisayarları özelliği ayarlayın Bakım modu = false
    ```

   If you are upgrading from an older version and switching to enqueue server 2, see SAP note [2641322](https://launchpad.support.sap.com/#/notes/2641322). 

   Make sure that the cluster status is ok and that all resources are started. It is not important on which node the resources are running.

    ```
    sudo bilgisayarları durumu
    
    # <a name="online--anftstsapcl1-anftstsapcl2-"></a>Çevrimiçi: [anftstsapcl1 anftstsapcl2]
    #
    # <a name="full-list-of-resources"></a>Kaynakların tam listesi:
    #
    # <a name="rscstazure----stonithfenceazurearm------started-anftstsapcl2"></a>rsc_st_azure    (stonith:fence_azure_arm):      Başlatılan anftstsapcl2
    #  <a name="resource-group-g-qasascs"></a>Kaynak grubu: g-QAS_ASCS
    #      <a name="fsqasascs--------ocfheartbeatfilesystem----started-anftstsapcl2"></a>fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Başlatılan anftstsapcl2
    #      <a name="ncqasascs--------ocfheartbeatazure-lb------started-anftstsapcl2"></a>nc_QAS_ASCS (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl2
    #      <a name="vipqasascs-------ocfheartbeatipaddr2-------started-anftstsapcl2"></a>vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Başlatılan anftstsapcl2
    #      <a name="rscsapqasascs00-ocfheartbeatsapinstance---started-anftstsapcl2"></a>rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Başlatılan anftstsapcl2
    #  <a name="resource-group-g-qasaers"></a>Kaynak grubu: g-QAS_AERS
    #      <a name="fsqasaers--------ocfheartbeatfilesystem----started-anftstsapcl1"></a>fs_QAS_AERS (ocf::heartbeat:Filesystem):    Başlatılan anftstsapcl1
    #      <a name="ncqasaers--------ocfheartbeatazure-lb------started-anftstsapcl1"></a>nc_QAS_AERS (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl1
    #      <a name="vipqasaers-------ocfheartbeatipaddr2-------started-anftstsapcl1"></a>vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Başlatılan anftstsapcl1
    #      <a name="rscsapqasers01--ocfheartbeatsapinstance---started-anftstsapcl1"></a>rsc_sap_QAS_ERS01 (ocf::heartbeat:SAPInstance):   Başlatılan anftstsapcl1
   ```

1. **[A]** Add firewall rules for ASCS and ERS on both nodes
   Add the firewall rules for ASCS and ERS on both nodes.
   ```
   # <a name="probe-port-of-ascs"></a>ASCS, yoklama bağlantı noktası
   sudo güvenlik duvarı-cmd--bölge ortak---port = 62000/tcp--= kalıcı sudo güvenlik duvarı-cmd--bölge ortak---port = 62000/tcp sudo güvenlik duvarı-cmd--= bölge ortak---port = 3200/tcp--= kalıcı sudo güvenlik duvarı-cmd--bölge ortak---port = 3200/tcp sudo = Güvenlik Duvarı-cmd--bölge ortak---port = 3600/tcp--= kalıcı sudo güvenlik duvarı-cmd--bölge ortak---port = 3600/tcp sudo güvenlik duvarı-cmd--= bölge ortak---port = 3900/tcp--= kalıcı sudo güvenlik duvarı-cmd--bölge ortak---port = 3900/tcp sudo = Güvenlik Duvarı-cmd--bölge ortak---port = 8100/tcp--= kalıcı sudo güvenlik duvarı-cmd--bölge ortak---port = 8100/tcp sudo güvenlik duvarı-cmd--= bölge ortak---port = 50013/tcp--= kalıcı sudo güvenlik duvarı-cmd--bölge ortak---port = 50013/tcp sudo = Güvenlik Duvarı-cmd--bölge ortak---port = 50014/tcp--= kalıcı sudo güvenlik duvarı-cmd--bölge ortak---port = 50014/tcp sudo güvenlik duvarı-cmd--= bölge ortak---port = 50016/tcp--= kalıcı sudo güvenlik duvarı-cmd--bölge ortak---port = 50016/tcp =
   # <a name="probe-port-of-ers"></a>Ağıranlar araştırma bağlantı noktası
   sudo güvenlik duvarı-cmd--bölge ortak---port = 62101/tcp--= kalıcı sudo güvenlik duvarı-cmd--bölge ortak---port = 62101/tcp sudo güvenlik duvarı-cmd--= bölge ortak---port = 3301/tcp--= kalıcı sudo güvenlik duvarı-cmd--bölge ortak---port = 3301/tcp sudo = Güvenlik Duvarı-cmd--bölge ortak---port = 50113/tcp--= kalıcı sudo güvenlik duvarı-cmd--bölge ortak---port = 50113/tcp sudo güvenlik duvarı-cmd--= bölge ortak---port = 50114/tcp--= kalıcı sudo güvenlik duvarı-cmd--bölge ortak---port = 50114/tcp sudo = Güvenlik Duvarı-cmd--bölge ortak---port = 50116/tcp--= kalıcı sudo güvenlik duvarı-cmd--bölge ortak---port = 50116/tcp =
   ```

## <a name="2d6008b0-685d-426c-b59e-6cd281fd45d7"></a>SAP NetWeaver application server preparation

   Some databases require that the database instance installation is executed on an application server. Prepare the application server virtual machines to be able to use them in these cases.  

   The steps bellow assume that you install the application server on a server different from the ASCS/SCS and HANA servers. Otherwise some of the steps below (like configuring host name resolution) are not needed.  

   The following items are prefixed with either **[A]** - applicable to both PAS and AAS, **[P]** - only applicable to PAS or **[S]** - only applicable to AAS.  

1. **[A]** Setup host name resolution
   You can either use a DNS server or modify the /etc/hosts on all nodes. This example shows how to use the /etc/hosts file.
   Replace the IP address and the hostname in the following commands:  

   ```
   sudo VI/etc/hosts
   ```

   Insert the following lines to /etc/hosts. Change the IP address and hostname to match your environment.

   ```
   # <a name="ip-address-of-the-load-balancer-frontend-configuration-for-sap-netweaver-ascs"></a>IP adresi SAP NetWeaver ASCS yük dengeleyici ön uç yapılandırması
   192.168.14.9 anftstsapvh
   # <a name="ip-address-of-the-load-balancer-frontend-configuration-for-sap-netweaver-ascs-ers"></a>IP adresi SAP NetWeaver ASCS Ağıranlar yük dengeleyici ön uç yapılandırması
   192.168.14.10 anftstsapers 192.168.14.7 anftstsapa01 192.168.14.8 anftstsapa02
   ```

1. **[A]** Create the sapmnt directory
   Create the sapmnt directory.
   ```
   sudo mkdir -p/sapmnt/QAS sudo mkdir -p /usr/sap/trans

   sudo chattr + ı/sapmnt/QAS sudo chattr + ı /usr/sap/trans
   ```

1. **[A]** Install NFS client and other requirements  

   ```
   sudo yum -y nfs-utils uuidd yükleyin
   ```

1. **[A]** Add mount entries  

   ```
   sudo VI/etc/fstab
   
   # <a name="add-the-following-lines-to-fstab-save-and-exit"></a>Fstab için aşağıdaki satırları ekleyin, Kaydet ve Çık
   192.168.24.5:/sapQAS/sapmntQAS /sapmnt/QAS nfs rw,hard,rsize=65536,wsize=65536,vers=3 192.168.24.4:/transSAP /usr/sap/trans nfs rw,hard,rsize=65536,wsize=65536,vers=3
   ```

   Mount the new shares

   ```
   sudo mount - a
   ```

1. **[P]** Create and mount the PAS directory  

   ```
   sudo mkdir -p /usr/sap/QAS/D02 sudo chattr + ı /usr/sap/QAS/D02
   
   sudo VI/etc/fstab
   # <a name="add-the-following-line-to-fstab"></a>Fstab için aşağıdaki satırı ekleyin
   92.168.24.5:/sapQAS/usrsapQASpas /usr/sap/QAS/D02 nfs rw,hard,rsize=65536,wsize=65536,vers=3
   
   # <a name="mount"></a>Bağlama
   sudo mount - a
   ```

1. **[S]** Create and mount the AAS directory  

   ```
   sudo mkdir -p /usr/sap/QAS/D03 sudo chattr +i /usr/sap/QAS/D03
   
   sudo VI/etc/fstab
   # <a name="add-the-following-line-to-fstab"></a>Fstab için aşağıdaki satırı ekleyin
   92.168.24.5:/sapQAS/usrsapQASaas /usr/sap/QAS/D03 nfs rw,hard,rsize=65536,wsize=65536,vers=3
   
   # <a name="mount"></a>Bağlama
   sudo mount - a
   ```


1. **[A]** Configure SWAP file
 
   ```
   sudo VI /etc/waagent.conf
   
   # <a name="set-the-property-resourcediskenableswap-to-y"></a>' % S'özelliği ResourceDisk.EnableSwap y ayarlayın
   # <a name="create-and-use-swapfile-on-resource-disk"></a>Oluşturun ve kaynak diski üzerinde takas dosyası kullanın.
   ResourceDisk.EnableSwap=y
   
   # <a name="set-the-size-of-the-swap-file-with-property-resourcediskswapsizemb"></a>TAKAS dosyası ResourceDisk.SwapSizeMB özelliğiyle boyutunu ayarlama
   # <a name="the-free-space-of-resource-disk-varies-by-virtual-machine-size-make-sure-that-you-do-not-set-a-value-that-is-too-big-you-can-check-the-swap-space-with-command-swapon"></a>Kaynak disk boş alanı, sanal makine boyutuna göre değişir. Çok büyük bir değere ayarlı değil emin olun. Komut swapon ile TAKAS alanı kontrol edebilirsiniz
   # <a name="size-of-the-swapfile"></a>Takas dosyası boyutu.
   ResourceDisk.SwapSizeMB=2000
   ```

   Restart the Agent to activate the change

   ```
   sudo waagent yeniden başlatma
   ```

## Install database

In this example, SAP NetWeaver is installed on SAP HANA. You can use every supported database for this installation. For more information on how to install SAP HANA in Azure, see [High availability of SAP HANA on Azure VMs on Red Hat Enterprise Linux][sap-hana-ha]. For a list of supported databases, see [SAP Note 1928533][1928533].

1. Run the SAP database instance installation

   Install the SAP NetWeaver database instance as root using a virtual hostname that maps to the IP address of the load balancer frontend configuration for the database.

   You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.

   ```
   sudo <swpm>/sapinst SAPINST_REMOTE_ACCESS_USER sapadmin =
   ```

## SAP NetWeaver application server installation

Follow these steps to install an SAP application server.

1. Prepare application server

   Follow the steps in the chapter [SAP NetWeaver application server preparation](high-availability-guide-rhel.md#2d6008b0-685d-426c-b59e-6cd281fd45d7) above to prepare the application server.

1. Install SAP NetWeaver application server

   Install a primary or additional SAP NetWeaver applications server.

   You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.

   ```
   sudo <swpm>/sapinst SAPINST_REMOTE_ACCESS_USER sapadmin =
   ```

1. Update SAP HANA secure store

   Update the SAP HANA secure store to point to the virtual name of the SAP HANA System Replication setup.

   Run the following command to list the entries as \<sapsid>adm

   ```
   hdbuserstore listesi
   ```

   This should list all entries and should look similar to
   ```
   VERİ dosyası: /home/qasadm/.hdb/anftstsapa01/SSFS_HDB. ANAHTAR dosyası DAT: /home/qasadm/.hdb/anftstsapa01/SSFS_HDB. ANAHTARI
   
   ANAHTAR VARSAYILAN ENV: 192.168.14.4:30313 KULLANICI: SAPABAP1 VERİTABANI: QAS
   ```

   The output shows that the IP address of the default entry is pointing to the virtual machine and not to the load balancer's IP address. This entry needs to be changed to point to the virtual hostname of the load balancer. Make sure to use the same port (**30313** in the output above) and database name (**QAS** in the output above)!

   ```
   Su - qasadm hdbuserstore varsayılan YAP qasdb:30313@QAS SAPABAP1 <password of ABAP schema>
   ```

## Test the cluster setup

1. Manually migrate the ASCS instance

   Resource state before starting the test:

   ```
    rsc_st_azure    (stonith:fence_azure_arm):      Anftstsapcl1 kaynak grubu başlatıldı: g QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Anftstsapcl1 nc_QAS_ASCS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl1 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Anftstsapcl1 rsc_sap_QAS_ASCS00 başlatıldı (ocf::heartbeat:SAPInstance):   Anftstsapcl1 kaynak grubu başlatıldı: g QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Anftstsapcl2 nc_QAS_AERS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl2 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Anftstsapcl2 rsc_sap_QAS_ERS01 başlatıldı (ocf::heartbeat:SAPInstance):   Başlatılan anftstsapcl2
   ```

   Run the following commands as root to migrate the ASCS instance.

   ```
   [root@anftstsapcl1 ~]# pcs resource move rsc_sap_QAS_ASCS00
   
   [root@anftstsapcl1 ~]# pcs resource clear rsc_sap_QAS_ASCS00
   
   # <a name="remove-failed-actions-for-the-ers-that-occurred-as-part-of-the-migration"></a>Başarısız eylemler için geçişin bir parçası oluştu Ağıranlar Kaldır
   [root@anftstsapcl1 ~]# pcs resource cleanup rsc_sap_QAS_ERS01
   ```

   Resource state after the test:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Anftstsapcl1 kaynak grubu başlatıldı: g QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Anftstsapcl2 nc_QAS_ASCS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl2 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Anftstsapcl2 rsc_sap_QAS_ASCS00 başlatıldı (ocf::heartbeat:SAPInstance):   Anftstsapcl2 kaynak grubu başlatıldı: g QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Anftstsapcl1 nc_QAS_AERS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl1 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Anftstsapcl1 rsc_sap_QAS_ERS01 başlatıldı (ocf::heartbeat:SAPInstance):   Başlatılan anftstsapcl1
   ```

1. Simulate node crash

   Resource state before starting the test:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Anftstsapcl1 kaynak grubu başlatıldı: g QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Anftstsapcl2 nc_QAS_ASCS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl2 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Anftstsapcl2 rsc_sap_QAS_ASCS00 başlatıldı (ocf::heartbeat:SAPInstance):   Anftstsapcl2 kaynak grubu başlatıldı: g QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Anftstsapcl1 nc_QAS_AERS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl1 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Anftstsapcl1 rsc_sap_QAS_ERS01 başlatıldı (ocf::heartbeat:SAPInstance):   Başlatılan anftstsapcl1
   ```

   Run the following command as root on the node where the ASCS instance is running

   ```
   [root@anftstsapcl2 ~] # echo b > /proc/sysrq-trigger
   ```

   The status after the node is started again should look like this.

   ```
   Çevrimiçi: [anftstsapcl1 anftstsapcl2]
   
   Kaynakların tam listesi:
   
   rsc_st_azure    (stonith:fence_azure_arm):      Anftstsapcl1 kaynak grubu başlatıldı: g QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Anftstsapcl1 nc_QAS_ASCS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl1 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Anftstsapcl1 rsc_sap_QAS_ASCS00 başlatıldı (ocf::heartbeat:SAPInstance):   Anftstsapcl1 kaynak grubu başlatıldı: g QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Anftstsapcl2 nc_QAS_AERS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl2 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Anftstsapcl2 rsc_sap_QAS_ERS01 başlatıldı (ocf::heartbeat:SAPInstance):   Başlatılan anftstsapcl2
   
   Başarısız eylemler:
   * anftstsapcl1 üzerinde rsc_sap_QAS_ERS01_monitor_11000 'çalışır' (7): çağrı = 45, durum = tam exitreason ='',
   ```

   Use the following command to clean the failed resources.

   ```
   [root@anftstsapcl1 ~]# pcs resource cleanup rsc_sap_QAS_ERS01
   ```

   Resource state after the test:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Anftstsapcl1 kaynak grubu başlatıldı: g QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Anftstsapcl1 nc_QAS_ASCS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl1 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Anftstsapcl1 rsc_sap_QAS_ASCS00 başlatıldı (ocf::heartbeat:SAPInstance):   Anftstsapcl1 kaynak grubu başlatıldı: g QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Anftstsapcl2 nc_QAS_AERS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl2 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Anftstsapcl2 rsc_sap_QAS_ERS01 başlatıldı (ocf::heartbeat:SAPInstance):   Başlatılan anftstsapcl2
   ```

1. Kill message server process

   Resource state before starting the test:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Anftstsapcl1 kaynak grubu başlatıldı: g QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Anftstsapcl1 nc_QAS_ASCS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl1 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Anftstsapcl1 rsc_sap_QAS_ASCS00 başlatıldı (ocf::heartbeat:SAPInstance):   Anftstsapcl1 kaynak grubu başlatıldı: g QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Anftstsapcl2 nc_QAS_AERS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl2 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Anftstsapcl2 rsc_sap_QAS_ERS01 başlatıldı (ocf::heartbeat:SAPInstance):   Başlatılan anftstsapcl2
   ```
   
   Run the following commands as root to identify the process of the message server and kill it.

   ```
   [root@anftstsapcl1 ~]# pgrep ms.sapQAS | xargs kill -9
   ```

   If you only kill the message server once, it will be restarted by `sapstart`. If you kill it often enough, Pacemaker will eventually move the ASCS instance to the other node. Run the following commands as root to clean up the resource state of the ASCS and ERS instance after the test.

   ```
   [root@anftstsapcl1 ~]# pcs resource cleanup rsc_sap_QAS_ASCS00 [root@anftstsapcl1 ~]# pcs resource cleanup rsc_sap_QAS_ERS01
   ```

   Resource state after the test:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Anftstsapcl1 kaynak grubu başlatıldı: g QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Anftstsapcl2 nc_QAS_ASCS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl2 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Anftstsapcl2 rsc_sap_QAS_ASCS00 başlatıldı (ocf::heartbeat:SAPInstance):   Anftstsapcl2 kaynak grubu başlatıldı: g QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Anftstsapcl1 nc_QAS_AERS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl1 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Anftstsapcl1 rsc_sap_QAS_ERS01 başlatıldı (ocf::heartbeat:SAPInstance):   Başlatılan anftstsapcl1
   ```

1. Kill enqueue server process

   Resource state before starting the test:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Anftstsapcl1 kaynak grubu başlatıldı: g QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Anftstsapcl2 nc_QAS_ASCS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl2 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Anftstsapcl2 rsc_sap_QAS_ASCS00 başlatıldı (ocf::heartbeat:SAPInstance):   Anftstsapcl2 kaynak grubu başlatıldı: g QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Anftstsapcl1 nc_QAS_AERS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl1 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Anftstsapcl1 rsc_sap_QAS_ERS01 başlatıldı (ocf::heartbeat:SAPInstance):   Başlatılan anftstsapcl1
   ```

   Run the following commands as root on the node where the ASCS instance is running to kill the enqueue server.

   ```
   [root@anftstsapcl2 ~] # omsagent en.sapQAS | xargs-9 Sonlandır
   ```

   The ASCS instance should immediately fail over to the other node. The ERS instance should also fail over after the ASCS instance is started. Run the following commands as root to clean up the resource state of the ASCS and ERS instance after the test.

   ```
   [root@anftstsapcl2 ~]# pcs resource cleanup rsc_sap_QAS_ASCS00 [root@anftstsapcl2 ~]# pcs resource cleanup rsc_sap_QAS_ERS01
   ```

   Resource state after the test:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Anftstsapcl1 kaynak grubu başlatıldı: g QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Anftstsapcl1 nc_QAS_ASCS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl1 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Anftstsapcl1 rsc_sap_QAS_ASCS00 başlatıldı (ocf::heartbeat:SAPInstance):   Anftstsapcl1 kaynak grubu başlatıldı: g QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Anftstsapcl2 nc_QAS_AERS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl2 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Anftstsapcl2 rsc_sap_QAS_ERS01 başlatıldı (ocf::heartbeat:SAPInstance):   Başlatılan anftstsapcl2
   ```

1. Kill enqueue replication server process

   Resource state before starting the test:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Anftstsapcl1 kaynak grubu başlatıldı: g QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Anftstsapcl1 nc_QAS_ASCS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl1 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Anftstsapcl1 rsc_sap_QAS_ASCS00 başlatıldı (ocf::heartbeat:SAPInstance):   Anftstsapcl1 kaynak grubu başlatıldı: g QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Anftstsapcl2 nc_QAS_AERS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl2 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Anftstsapcl2 rsc_sap_QAS_ERS01 başlatıldı (ocf::heartbeat:SAPInstance):   Başlatılan anftstsapcl2
   ```

   Run the following command as root on the node where the ERS instance is running to kill the enqueue replication server process.

   ```
   [root@anftstsapcl2 ~] # omsagent er.sapQAS | xargs-9 Sonlandır
   ```

   If you only run the command once, `sapstart` will restart the process. If you run it often enough, `sapstart` will not restart the process and the resource will be in a stopped state. Run the following commands as root to clean up the resource state of the ERS instance after the test.

   ```
   [root@anftstsapcl2 ~]# pcs resource cleanup rsc_sap_QAS_ERS01
   ```

   Resource state after the test:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Anftstsapcl1 kaynak grubu başlatıldı: g QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Anftstsapcl1 nc_QAS_ASCS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl1 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Anftstsapcl1 rsc_sap_QAS_ASCS00 başlatıldı (ocf::heartbeat:SAPInstance):   Anftstsapcl1 kaynak grubu başlatıldı: g QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Anftstsapcl2 nc_QAS_AERS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl2 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Anftstsapcl2 rsc_sap_QAS_ERS01 başlatıldı (ocf::heartbeat:SAPInstance):   Başlatılan anftstsapcl2
   ```

1. Kill enqueue sapstartsrv process

   Resource state before starting the test:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Anftstsapcl1 kaynak grubu başlatıldı: g QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Anftstsapcl1 nc_QAS_ASCS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl1 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Anftstsapcl1 rsc_sap_QAS_ASCS00 başlatıldı (ocf::heartbeat:SAPInstance):   Anftstsapcl1 kaynak grubu başlatıldı: g QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Anftstsapcl2 nc_QAS_AERS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl2 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Anftstsapcl2 rsc_sap_QAS_ERS01 başlatıldı (ocf::heartbeat:SAPInstance):   Başlatılan anftstsapcl2
   ```

   Run the following commands as root on the node where the ASCS is running.

   ```
   [root@anftstsapcl1 ~] # omsagent -fl ASCS00.*sapstartsrv
   # <a name="59545-sapstartsrv"></a>59545 sapstartsrv
   
   [root@anftstsapcl1 ~] # sonlandırma-9 59545
   ```

   The sapstartsrv process should always be restarted by the Pacemaker resource agent as part of the monitoring. Resource state after the test:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Anftstsapcl1 kaynak grubu başlatıldı: g QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Anftstsapcl1 nc_QAS_ASCS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl1 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Anftstsapcl1 rsc_sap_QAS_ASCS00 başlatıldı (ocf::heartbeat:SAPInstance):   Anftstsapcl1 kaynak grubu başlatıldı: g QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Anftstsapcl2 nc_QAS_AERS başlatıldı (ocf::heartbeat:azure-lb):      Başlatılan anftstsapcl2 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Anftstsapcl2 rsc_sap_QAS_ERS01 başlatıldı (ocf::heartbeat:SAPInstance):   Başlatılan anftstsapcl2
   ```

## Next steps

* [Azure Virtual Machines planning and implementation for SAP][planning-guide]
* [Azure Virtual Machines deployment for SAP][deployment-guide]
* [Azure Virtual Machines DBMS deployment for SAP][dbms-guide]
* To learn how to establish high availability and plan for disaster recovery of SAP HANA on Azure (large instances), see [SAP HANA (large instances) high availability and disaster recovery on Azure](hana-overview-high-availability-disaster-recovery.md).
* To learn how to establish high availability and plan for disaster recovery of SAP HANA on Azure VMs, see [High Availability of SAP HANA on Azure Virtual Machines (VMs)][sap-hana-ha]
