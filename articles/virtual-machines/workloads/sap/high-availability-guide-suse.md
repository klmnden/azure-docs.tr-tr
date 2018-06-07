---
title: Azure sanal makineleri yüksek kullanılabilirlik için SAP NetWeaver SUSE Linux Enterprise Server üzerinde SAP uygulamaları için | Microsoft Docs
description: Yüksek kullanılabilirlik için Kılavuzu SAP NetWeaver SUSE Linux Enterprise Server üzerinde SAP uygulamaları için
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: mssedusch
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: sedusch
ms.openlocfilehash: 12efeba68f30aa8723acc32449ae05ffac4c1ac4
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34658766"
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms-on-suse-linux-enterprise-server-for-sap-applications"></a>SUSE Linux Enterprise Server üzerinde Azure vm'lerinde SAP NetWeaver SAP uygulamalar için yüksek kullanılabilirlik

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
[nfs-ha]:high-availability-guide-suse-nfs.md

Bu makalede, sanal makineleri dağıtmak, sanal makineleri yapılandırma, küme Framework'ü yüklemek ve yüksek oranda kullanılabilir bir SAP NetWeaver 7.50 sistemi yükleyin açıklar.
Örnek yapılandırmalarında yükleme komutlarını vs. Sayı 02 ERS ASCS örnek numarasını 00, örneği ve SAP sistem kimliği NW1 kullanılır. Örnekte kaynakların (örneğin, sanal makineler, sanal ağlar) adları, kullandığınız varsayılmıştır [şablonu Yakınsanan] [ template-converged] kimliği kaynak oluşturmak için NW1 SAP sistemiyle.

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

![SAP NetWeaver yüksek kullanılabilirlik genel bakış](./media/high-availability-guide-suse/img_001.png)

NFS sunucusu, SAP NetWeaver ASCS, SAP NetWeaver SCS, SAP NetWeaver ERS ve SAP HANA veritabanına sanal ana bilgisayar adı ve sanal IP adresleri kullanın. Azure üzerinde bir yük dengeleyici sanal IP adresi kullanmak için gereklidir. Aşağıdaki liste, (A) yapılandırmasını gösterir SCS ve ERS yük dengeleyici.

### <a name="ascs"></a>(A)SCS

* Ön uç yapılandırma
  * IP adresi 10.0.0.7
* Arka uç yapılandırması
  * (A) bir parçası olması gereken tüm sanal makinelerin birincil ağ arabirimlerine bağlı SCS/ERS küme
* Sonda bağlantı noktası
  * Bağlantı noktası 620**&lt;n&gt;**
* Loadbalancing kuralları
  * 32**&lt;n&gt;**  TCP
  * 36**&lt;n&gt;**  TCP
  * 39**&lt;n&gt;**  TCP
  * 81**&lt;n&gt;**  TCP
  * 5**&lt;n&gt;** 13 TCP
  * 5**&lt;n&gt;** 14 TCP
  * 5**&lt;n&gt;** 16 TCP

### <a name="ers"></a>ERS

* Ön uç yapılandırma
  * IP adresi 10.0.0.8
* Arka uç yapılandırması
  * (A) bir parçası olması gereken tüm sanal makinelerin birincil ağ arabirimlerine bağlı SCS/ERS küme
* Sonda bağlantı noktası
  * Bağlantı noktası 621**&lt;n&gt;**
* Loadbalancing kuralları
  * 33**&lt;n&gt;**  TCP
  * 5**&lt;n&gt;** 13 TCP
  * 5**&lt;n&gt;** 14 TCP
  * 5**&lt;n&gt;** 16 TCP

## <a name="setting-up-a-highly-available-nfs-server"></a>Yüksek oranda kullanılabilir bir NFS sunucusu kurma

SAP NetWeaver aktarım ve profil dizini için paylaşılan depolama gerektirir. Okuma [yüksek kullanılabilirlik için NFS SUSE Linux Enterprise Server üzerinde Azure vm'lerinde] [ nfs-ha] SAP NetWeaver için bir NFS sunucusunu ayarlama konusunda.

## <a name="setting-up-ascs"></a>(A) SCS ayarlama

Sanal makineler de dahil olmak üzere tüm gerekli Azure kaynaklarını dağıtmak için bir Azure şablonu github'dan kullanabilirsiniz, kullanılabilirlik kümesi ve yük dengeleyici veya kaynakları el ile dağıtabilirsiniz.

### <a name="deploy-linux-via-azure-template"></a>Linux Azure şablonu aracılığıyla dağıtma

Azure Market SAP uygulamaları 12 için yeni sanal makineleri dağıtmak için kullanabileceğiniz SUSE Linux Enterprise Server için bir görüntü içerir. Market görüntüsü SAP NetWeaver kaynak aracı içerir.

Tüm gerekli kaynakları dağıtmak için github'da hızlı başlangıç şablonlarından birini kullanabilirsiniz. Şablonun sanal makineler, yük dengeleyici, kullanılabilirlik vb. kümesi dağıtır. Şablonu dağıtmak için aşağıdaki adımları izleyin:

1. Açık [ASCS/SCS çoklu SID şablonu] [ template-multisid-xscs] veya [şablonu Yakınsanan] [ template-converged] yakınsanmış şablon de bir veritabanı (örneğin, Microsoft SQL Server veya SAP HANA) için Yük Dengeleme kuralları oluşturur ancak Azure üzerinde portal ASCS/SCS şablonu yalnızca Yük Dengeleme kuralları SAP NetWeaver ASCS/SCS ve ERS (yalnızca Linux) örnekleri için oluşturur. SAP NetWeaver temel sistem yüklemeyi planladığınız ve ayrıca istiyorsanız aynı makinelerde veritabanını yüklemek, kullanmak [şablonu Yakınsanan][template-converged].
1. Aşağıdaki parametreleri girin
   1. Kaynak önek (yalnızca ASCS/SCS çoklu SID şablonu)  
      Kullanmak istediğiniz ön eki girin. Değer, dağıtılan kaynaklar için önek olarak kullanılır.
   3. SAP sistem kimliği (yalnızca yakınsanmış şablonu)  
      Yüklemek istediğiniz SAP sistem SAP sistem Kimliğini girin. Kimliği önek olarak dağıtılan kaynaklar için kullanılır.
   4. Yığın türü  
      SAP NetWeaver yığın türünü seçin
   5. İşletim sistemi türü  
      Linux dağıtımları birini seçin. Bu örnekte, SLES 12 BYOS seçin
   6. DB türü  
      HANA seçin
   7. SAP sistemi boyutu  
      Yeni sistem sağlar SAP miktarı. Sistem gerektirir kaç SAP değil eminseniz, SAP teknolojisi iş ortağı veya sistem Tümleştirici isteyin
   8. Sistem kullanılabilirliği  
      HA seçin
   9. Yönetici kullanıcı adı ve yönetici parolası  
      Yeni bir kullanıcı oluşturulur makineye oturum açmak için kullanılabilir.
   10. Alt ağ kimliği  
   Sanal makineler için bağlanması alt ağ kimliği.  Yeni bir sanal ağ oluşturmak veya NFS sunucu dağıtımının bir parçası olarak kullanılan ya da oluşturduğunuz aynı alt seçmek istiyorsanız boş bırakın. Kimliği genellikle /subscriptions/ gibi görünüyor**&lt;abonelik kimliği&gt;**/resourceGroups/**&lt;kaynak grubu adı&gt;**/providers/ Microsoft.Network/virtualNetworks/**&lt;sanal ağ adı&gt;**/subnets/**&lt;alt ağ adı&gt;**

### <a name="deploy-linux-manually-via-azure-portal"></a>Linux Azure portalı üzerinden el ile dağıtma

Önce bu NFS küme için sanal makineleri oluşturmanız gerekir. Daha sonra bir yük dengeleyicisi oluşturun ve sanal makineleri arka uç havuzlarını kullanın.

1. Kaynak Grubu oluşturma
1. Sanal Ağ Oluştur
1. Kullanılabilirlik kümesi oluştur  
   Set max güncelleştirme etki alanı
1. Sanal makine 1 oluşturun   
   En azından SLES4SAP 12 SP1, bu örnek SLES4SAP 12 SP1 resmi https://portal.azure.com/#create/SUSE.SUSELinuxEnterpriseServerforSAPApplications12SP1PremiumImage-ARM  
   SAP uygulamaları 12 SP1 ile için SLES kullanılır  
   Daha önce oluşturduğunuz kullanılabilirlik kümesi seçin  
1. Sanal makine 2 oluşturun   
   En azından SLES4SAP 12 SP1, bu örnek SLES4SAP 12 SP1 resmi https://portal.azure.com/#create/SUSE.SUSELinuxEnterpriseServerforSAPApplications12SP1PremiumImage-ARM  
   SAP uygulamaları 12 SP1 ile için SLES kullanılır  
   Daha önce oluşturduğunuz kullanılabilirlik kümesi seçin  
1. Her iki sanal makine için en az bir veri diski Ekle  
   Veri diskleri / usr/sap/için kullanılan`<SAPSID`> Dizin
1. Bir yük dengeleyiciye (dahili) oluşturma  
   1. Ön uç IP adreslerini oluşturun
      1. IP adresi 10.0.0.7 ASCS için
         1. Yük Dengeleyici açın, ön uç IP havuzu seçin ve Ekle'yi tıklatın
         1. Yeni ön uç IP havuzunun adını girin (örneğin **nw1 ascs frontend**)
         1. Atama statik olarak ayarlamanız ve IP adresini girin (örneğin **10.0.0.7**)
         1. Tamam'ı tıklatın
      1. ASCS ERS için IP adresi 10.0.0.8
         * Bir IP adresi için ERS oluşturmak için yukarıdaki adımları yineleyin (örneğin **10.0.0.8** ve **nw1 aers arka**)
   1. Arka uç havuzları oluşturma
      1. ASCS için bir arka uç havuzu oluşturma
         1. Yük Dengeleyici açın, arka uç havuzlarını seçin ve Ekle'yi tıklatın
         1. Yeni arka uç havuzunun adını girin (örneğin **nw1 ascs arka**)
         1. Bir sanal makine Ekle seçeneğine tıklayın.
         1. Daha önce oluşturduğunuz kullanılabilirlik kümesi seçin
         1. (A) sanal makinelerin seçin SCS küme
         1. Tamam'ı tıklatın
      1. ASCS ERS için bir arka uç havuzu oluşturma
         * ERS için bir arka uç havuzu oluşturmak için yukarıdaki adımları yineleyin (örneğin **nw1 aers arka**)
   1. Sistem durumu araştırmalarının oluşturma
      1. Bağlantı noktası 620**00** ASCS için
         1. Yük Dengeleyici açın, sistem durumu araştırmalarının seçin ve Ekle'yi tıklatın
         1. Yeni durum araştırması adını girin (örneğin **nw1 ascs hp**)
         1. TCP bağlantı noktası 620 protokolü olarak seçin**00**, aralığı 5 ile sağlıksız durum eşiği 2 tutun
         1. Tamam'ı tıklatın
      1. Bağlantı noktası 621**02** ASCS ERS için
         * Bir sistem durumu araştırması için ERS oluşturmak için yukarıdaki adımları yineleyin (örneğin 621**02** ve **nw1 aers hp**)
   1. Loadbalancing kuralları
      1. 32**00** ASCS TCP
         1. Yük Dengeleyici açın, Yük Dengeleme kuralları seçin ve Ekle'yi tıklatın
         1. Yeni Yük Dengeleyici kuralı adını girin (örneğin **nw1 lb 3200**)
         1. Ön uç IP adresi, arka uç havuzu ve daha önce oluşturduğunuz durumu araştırması seçin (örneğin **nw1 ascs frontend**)
         1. Protokol tutmak **TCP**, bağlantı noktası girin **3200**
         1. -30 dakika boşta kalma zaman aşımı süresini artırın
         1. **Kayan IP etkinleştirdiğinizden emin olun**
         1. Tamam'ı tıklatın    
      1. ASCS için ek bağlantı noktaları
         * 36 bağlantı noktaları için yukarıdaki adımları yineleyin**00**, 39**00**, 81**00**, 5**00**13, 5**00**14, 5**00**16 ve ASCS TCP
      1. ASCS ERS için ek bağlantı noktaları
         * 33 bağlantı noktaları için yukarıdaki adımları yineleyin**02**, 5**02**13, 5**02**14, 5**02**16 ve ASCS ERS TCP

### <a name="create-pacemaker-cluster"></a>Pacemaker kümesi oluşturma

Adımları [Pacemaker SUSE Linux Enterprise Server Azure üzerinde ayarlama](high-availability-guide-suse-pacemaker.md) temel oluşturmak için Pacemaker küme bu (A) SCS sunucu.

### <a name="installation"></a>Yükleme

Aşağıdaki öğeler ile ya da önek **[A]** - tüm düğümleri için geçerli **[1]** - 1 düğümü yalnızca uygulanabilir veya **[2]** - yalnızca düğüm 2 için geçerlidir.

1. **[A]**  SUSE bağlayıcısını
   
   <pre><code>
   sudo zypper install sap_suse_cluster_connector
   </code></pre>

1. **[A]**  Güncelleştirme SAP kaynak aracıları  
   
   Kaynak aracıları paket için bir düzeltme eki, bu makalede açıklanan yeni yapılandırmayı kullanmak için gereklidir. Düzeltme eki aşağıdaki komutla zaten yüklüyse, kontrol edebilirsiniz

   <pre><code>
   sudo grep 'parameter name="IS_ERS"' /usr/lib/ocf/resource.d/heartbeat/SAPInstance
   </code></pre>

   Çıktısı benzer olmalıdır

   <pre><code>
   &lt;parameter name="IS_ERS" unique="0" required="0"&gt;
   </code></pre>

   Grep komutu IS_ERS parametresi bulamazsa, listelenen düzeltme ekini yüklemek gereken [SUSE indirme sayfası](https://download.suse.com/patch/finder/#bu=suse&familyId=&productId=&dateRange=&startDate=&endDate=&priority=&architecture=&keywords=resource-agents)

   <pre><code>
   # example for patch for SLES 12 SP1
   sudo zypper in -t patch SUSE-SLE-HA-12-SP1-2017-885=1
   # example for patch for SLES 12 SP2
   sudo zypper in -t patch SUSE-SLE-HA-12-SP2-2017-886=1
   </code></pre>

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
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS
   <b>10.0.0.7 nw1-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS ERS
   <b>10.0.0.8 nw1-aers</b>
   # IP address of the load balancer frontend configuration for database
   <b>10.0.0.13 nw1-db</b>
   </code></pre>

## <a name="prepare-for-sap-netweaver-installation"></a>SAP NetWeaver yükleme için hazırlama

1. **[A]**  Paylaşılan dizinler oluşturma

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NW1</b>
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

1. **[A]**  Autofs yapılandırın
 
   <pre><code>
   sudo vi /etc/auto.master
   
   # Add the following line to the file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   Dosya Oluştur

   <pre><code>
   sudo vi /etc/auto.direct
   
   # Add the following lines to the file, save and exit
   /sapmnt/<b>NW1</b> -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/trans
   /usr/sap/<b>NW1</b>/SYS -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/sidsys
   /usr/sap/<b>NW1</b>/ASCS<b>00</b> -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/ASCS
   /usr/sap/<b>NW1</b>/ERS<b>02</b> -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/ASCSERS
   </code></pre>

   Yeni paylaşımlar bağlamak için autofs yeniden başlatın

   <pre><code>
   sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. **[A]**  Yapılandırma TAKAS dosyası

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

   Değişiklik etkinleştirmek için aracıyı yeniden başlatın

   <pre><code>
   sudo service waagent restart
   </code></pre>


### <a name="installing-sap-netweaver-ascsers"></a>SAP NetWeaver ASCS/ERS yükleme

1. **[1]**  Bir sanal IP kaynağı oluşturup ASCS örneği için sistem durumu araştırması

   <pre><code>
   sudo crm node standby <b>nw1-cl-1</b>
   
   sudo crm configure primitive vip_<b>NW1</b>_ASCS IPaddr2 \
     params ip=<b>10.0.0.7</b> cidr_netmask=<b>24</b> \
     op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>NW1</b>_ASCS anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k 620<b>00</b>" \
     op monitor timeout=20s interval=10 depth=0
   
   sudo crm configure group g-<b>NW1</b>_ASCS nc_<b>NW1</b>_ASCS vip_<b>NW1</b>_ASCS \
      meta resource-stickiness=3000
   </code></pre>

   Küme durumunun Tamam olduğunu ve tüm kaynakları başlatıldığından emin olun. Kaynakları çalıştıran hangi düğümde önemli değildir.

   <pre><code>
   sudo crm_mon -r
   
   # Node nw1-cl-1: standby
   # <b>Online: [ nw1-cl-0 ]</b>
   # 
   # Full list of resources:
   # 
   # stonith-sbd     (stonith:external/sbd): <b>Started nw1-cl-0</b>
   # rsc_st_azure    (stonith:fence_azure_arm):      <b>Started nw1-cl-0</b>
   #  Resource Group: g-NW1_ASCS
   #      nc_NW1_ASCS        (ocf::heartbeat:anything):      <b>Started nw1-cl-0</b>
   #      vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-0</b>
   </code></pre>

1. **[1]**  SAP NetWeaver ASCS yükleyin  

   Örneğin ASCS yük dengeleyici ön uç yapılandırmasında IP adresine eşleyen bir sanal ana bilgisayar adı kullanarak ilk düğümde kök olarak SAP NetWeaver ASCS yükleme <b>nw1 ascs</b>, <b>10.0.0.7</b> ve örnek, örneğin yük dengeleyici araştırması için kullandığınız sayı <b>00</b>.

   Kök olmayan kullanıcının sapinst için bağlanmasına izin vermek için sapinst parametresini SAPINST_REMOTE_ACCESS_USER kullanabilirsiniz.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   Bir alt klasör/usr/sap/oluşturmak yükleme başarısız olursa**NW1**/ASCS**00**, sahibi ve ASCS grubu ayarlamayı deneyin**00** klasörü ve yeniden deneyin.

   <pre><code>
   chown nw1adm /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   chgrp sapsys /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   </code></pre>

1. **[1]**  Bir sanal IP kaynağı oluşturup ERS örneği için sistem durumu araştırması

   <pre><code>
   sudo crm node online <b>nw1-cl-1</b>
   sudo crm node standby <b>nw1-cl-0</b>
   
   sudo crm configure primitive vip_<b>NW1</b>_ERS IPaddr2 \
     params ip=<b>10.0.0.8</b> cidr_netmask=<b>24</b> \
     op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>NW1</b>_ERS anything \
    params binfile="/usr/bin/nc" cmdline_options="-l -k 621<b>02</b>" \
    op monitor timeout=20s interval=10 depth=0
   
   # WARNING: Resources nc_NW1_ASCS,nc_NW1_ERS violate uniqueness for parameter "binfile": "/usr/bin/nc"
   # Do you still want to commit (y/n)? y
   
   sudo crm configure group g-<b>NW1</b>_ERS nc_<b>NW1</b>_ERS vip_<b>NW1</b>_ERS
   </code></pre>
 
   Küme durumunun Tamam olduğunu ve tüm kaynakları başlatıldığından emin olun. Kaynakları çalıştıran hangi düğümde önemli değildir.

   <pre><code>
   sudo crm_mon -r
   
   # Node <b>nw1-cl-0: standby</b>
   # <b>Online: [ nw1-cl-1 ]</b>
   # 
   # Full list of resources:
   #
   # stonith-sbd     (stonith:external/sbd): <b>Started nw1-cl-1</b>
   # rsc_st_azure    (stonith:fence_azure_arm):      <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ASCS
   #      nc_NW1_ASCS        (ocf::heartbeat:anything):      <b>Started nw1-cl-1</b>
   #      vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ERS
   #      nc_NW1_ERS (ocf::heartbeat:anything):      <b>Started nw1-cl-1</b>
   #      vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-1</b>
   </code></pre>

1. **[2]**  SAP NetWeaver ERS yükleyin  

   Örneğin ERS yük dengeleyici ön uç yapılandırmasında IP adresine eşleyen bir sanal ana bilgisayar adı kullanarak ikinci düğümde kök olarak SAP NetWeaver ERS yükleme <b>nw1 aers</b>, <b>10.0.0.8</b> ve örnek, örneğin yük dengeleyici araştırması için kullandığınız sayı <b>02</b>.

   Kök olmayan kullanıcının sapinst için bağlanmasına izin vermek için sapinst parametresini SAPINST_REMOTE_ACCESS_USER kullanabilirsiniz.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   > [!NOTE]
   > SWPM SP 20 PL 05 ya da daha yüksek kullanın. Daha düşük sürümler izinleri düzgün ayarlanmamış ve yükleme başarısız olur.

   Bir alt klasör/usr/sap/oluşturmak yükleme başarısız olursa**NW1**/ERS**02**, sahibi ve ERS grubu ayarlamayı deneyin**02** klasörü ve yeniden deneyin.

   <pre><code>
   chown nw1adm /usr/sap/<b>NW1</b>/ERS<b>02</b>
   chgrp sapsys /usr/sap/<b>NW1</b>/ERS<b>02</b>
   </code></pre>
   > 

1. **[1]**  Adapt ASCS/SCS ve ERS örnek profilleri
 
   * ASCS/SCS profili

   <pre><code> 
   sudo vi /sapmnt/<b>NW1</b>/profile/<b>NW1</b>_<b>ASCS00</b>_<b>nw1-ascs</b>
   
   # Change the restart command to a start command
   #Restart_Program_01 = local $(_EN) pf=$(_PF)
   Start_Program_01 = local $(_EN) pf=$(_PF)
   
   # Add the following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector
   
   # Add the keep alive parameter
   enque/encni/set_so_keepalive = true
   </code></pre>

   * ERS profili

   <pre><code> 
   sudo vi /sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b>
   
   # Add the following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector
   </code></pre>


1. **[A]**  Tutmayı yapılandırma

   SAP NetWeaver uygulama sunucusu ve ASCS/SCS arasındaki iletişimi yazılım yük dengeleyici yönlendirilir. Yük Dengeleyici yapılandırılabilir bir zaman aşımından sonra etkin olmayan bağlantıları bağlantısını keser. Bunu önlemek için SAP NetWeaver ASCS/SCS profilinde parametre ve Linux sistem ayarlarını değiştirmeniz gerekir. Okuma [SAP Not 1410736] [ 1410736] daha fazla bilgi için.
   
   ASCS/SCS profili parametre CLR'yi/encni/set_so_keepalive son adımda zaten eklendi.

   <pre><code> 
   # Change the Linux system configuration
   sudo sysctl net.ipv4.tcp_keepalive_time=120
   </code></pre>

1. **[A]**  Yükleme sonrasında SAP kullanıcıları yapılandırın
 
   <pre><code>
   # Add sidadm to the haclient group
   sudo usermod -aG haclient <b>nw1</b>adm   
   </code></pre>

1. **[1]**  ASCS ve ERS SAP Hizmetleri sapservice dosyasına Ekle

   ASCS ERS hizmet girişi ilk düğüme kopyalayın ve giriş ikinci düğüme hizmeti ekleyin.

   <pre><code>
   cat /usr/sap/sapservices | grep ASCS<b>00</b> | sudo ssh <b>nw1-cl-1</b> "cat >>/usr/sap/sapservices"
   sudo ssh <b>nw1-cl-1</b> "cat /usr/sap/sapservices" | grep ERS<b>02</b> | sudo tee -a /usr/sap/sapservices
   </code></pre>

1. **[1]**  SAP küme kaynakları oluşturun

   <pre><code>
   sudo crm configure property maintenance-mode="true"   
   
   sudo crm configure primitive rsc_sap_<b>NW1</b>_ASCS<b>00</b> SAPInstance \
    operations \$id=rsc_sap_<b>NW1</b>_ASCS<b>00</b>-operations \
    op monitor interval=11 timeout=60 on_fail=restart \
    params InstanceName=<b>NW1</b>_ASCS<b>00</b>_<b>nw1-ascs</b> START_PROFILE="/sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ASCS<b>00</b>_<b>nw1-ascs</b>" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 failure-timeout=60 migration-threshold=1 priority=10
   
   sudo crm configure primitive rsc_sap_<b>NW1</b>_ERS<b>02</b> SAPInstance \
    operations \$id=rsc_sap_<b>NW1</b>_ERS<b>02</b>-operations \
    op monitor interval=11 timeout=60 on_fail=restart \
    params InstanceName=<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b> START_PROFILE="/sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b>" AUTOMATIC_RECOVER=false IS_ERS=true \
    meta priority=1000
   
   sudo crm configure modgroup g-<b>NW1</b>_ASCS add rsc_sap_<b>NW1</b>_ASCS<b>00</b>
   sudo crm configure modgroup g-<b>NW1</b>_ERS add rsc_sap_<b>NW1</b>_ERS<b>02</b>
   
   sudo crm configure colocation col_sap_<b>NW1</b>_no_both -5000: g-<b>NW1</b>_ERS g-<b>NW1</b>_ASCS
   sudo crm configure location loc_sap_<b>NW1</b>_failover_to_ers rsc_sap_<b>NW1</b>_ASCS<b>00</b> rule 2000: runs_ers_<b>NW1</b> eq 1
   sudo crm configure order ord_sap_<b>NW1</b>_first_start_ascs Optional: rsc_sap_<b>NW1</b>_ASCS<b>00</b>:start rsc_sap_<b>NW1</b>_ERS<b>02</b>:stop symmetrical=false
   
   sudo crm node online <b>nw1-cl-0</b>
   sudo crm configure property maintenance-mode="false"
   </code></pre>

   Küme durumunun Tamam olduğunu ve tüm kaynakları başlatıldığından emin olun. Kaynakları çalıştıran hangi düğümde önemli değildir.

   <pre><code>
   sudo crm_mon -r
   
   # Online: <b>[ nw1-cl-0 nw1-cl-1 ]</b>
   #
   # Full list of resources:
   #
   # stonith-sbd     (stonith:external/sbd): <b>Started nw1-cl-1</b>
   # rsc_st_azure    (stonith:fence_azure_arm):      <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ASCS
   #      nc_NW1_ASCS        (ocf::heartbeat:anything):      <b>Started nw1-cl-1</b>
   #      vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-1</b>
   #      rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ERS
   #      nc_NW1_ERS (ocf::heartbeat:anything):      <b>Started nw1-cl-0</b>
   #      vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-0</b>
   #      rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   <b>Started nw1-cl-0</b>
   </code></pre>

## <a name="2d6008b0-685d-426c-b59e-6cd281fd45d7"></a>SAP NetWeaver uygulama sunucusu hazırlığı

Bazı veritabanları veritabanı örneği yüklemesi bir uygulama sunucusunda yürütülür gerektirir. Bu durumlarda kullanmak için uygulama sunucusu sanal makineleri hazırlayın.

Adımları aşağıdaki varsayalım ASCS/SCS ve HANA sunuculardan farklı bir sunucu uygulama sunucusu yükleyin. Aksi takdirde (örneğin, ana bilgisayar adı çözümlemesi yapılandırma) aşağıdaki adımları bazıları gerekli değildir.

1. Ana bilgisayar adı çözümlemesi Kurulumu

   Bir DNS sunucusu kullanın veya tüm düğümlerde/etc/hosts değiştirin. Bu örnek/Etc/Hosts dosyasının nasıl kullanılacağını gösterir.
   IP adresi ve aşağıdaki komutlarda ana bilgisayar adını değiştirin
   ```bash
   sudo vi /etc/hosts
   ```
   / Etc/hosts aşağıdaki satırları ekleyin. Ortamınıza uyum sağlaması için ana bilgisayar adı ve IP adresini değiştirme    
    
   <pre><code>
   # IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nw1-nfs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS/SCS
   <b>10.0.0.7 nw1-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ERS
   <b>10.0.0.8 nw1-aers</b>
   # IP address of the load balancer frontend configuration for database
   <b>10.0.0.13 nw1-db</b>
   # IP address of all application servers
   <b>10.0.0.20 nw1-di-0</b>
   <b>10.0.0.21 nw1-di-1</b>
   </code></pre>

1. Sapmnt dizin oluşturun

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NW1</b>
   sudo mkdir -p /usr/sap/trans

   sudo chattr +i /sapmnt/<b>NW1</b>
   sudo chattr +i /usr/sap/trans
   </code></pre>

1. AutoFS yapılandırın
 
   <pre><code>
   sudo vi /etc/auto.master
   
   # Add the following line to the file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   Yeni dosya oluştur

   <pre><code>
   sudo vi /etc/auto.direct
   
   # Add the following lines to the file, save and exit
   /sapmnt/<b>NW1</b> -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/trans
   </code></pre>

   Yeni paylaşımlar bağlamak için autofs yeniden başlatın

   <pre><code>
   sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. TAKAS dosyasını yapılandırma
 
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

   Değişiklik etkinleştirmek için aracıyı yeniden başlatın

   <pre><code>
   sudo service waagent restart
   </code></pre>

## <a name="install-database"></a>Veritabanını yükleme

Bu örnekte, SAP NetWeaver SAP HANA yüklenir. Desteklenen her veritabanı için bu yüklemeyi kullanabilirsiniz. SAP HANA Azure'da yükleme hakkında daha fazla bilgi için bkz: [SAP HANA, yüksek kullanılabilirlik'Azure sanal makineler (VM'ler) üzerinde][sap-hana-ha]. Desteklenen veritabanlarının bir listesi için bkz: [SAP Not 1928533][1928533].

1. SAP veritabanı örneği yüklemesini çalıştırın

   Örneğin IP adresi için yük dengeleyici ön uç yapılandırma veritabanı için eşleşen bir sanal ana bilgisayar adı kullanarak kök olarak SAP NetWeaver veritabanı örneğini yükleyin <b>nw1 db</b> ve <b>10.0.0.13</b>.

   Kök olmayan kullanıcının sapinst için bağlanmasına izin vermek için sapinst parametresini SAPINST_REMOTE_ACCESS_USER kullanabilirsiniz.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

## <a name="sap-netweaver-application-server-installation"></a>SAP NetWeaver uygulama sunucusu yükleme

SAP uygulama sunucusu yüklemek için aşağıdaki adımları izleyin. 

1. Uygulama sunucusu hazırlayın

Bölüm adımları [SAP NetWeaver uygulama sunucusu hazırlığı](high-availability-guide-suse.md#2d6008b0-685d-426c-b59e-6cd281fd45d7) uygulama sunucusu hazırlamak için yukarıdaki.

1. SAP NetWeaver uygulama sunucusu yükleme

   Bir birincil ya da ek SAP NetWeaver uygulamalar sunucusu yükleyin.

   Kök olmayan kullanıcının sapinst için bağlanmasına izin vermek için sapinst parametresini SAPINST_REMOTE_ACCESS_USER kullanabilirsiniz.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. SAP HANA güvenli depolama güncelleştir

   SAP HANA güvenli depolama SAP HANA sistem çoğaltma Kurulum sanal adına işaret edecek şekilde güncelleştirin.

   Girişleri listelemek için aşağıdaki komutu çalıştırın
   <pre><code>
   hdbuserstore List
   </code></pre>

   Bu tüm girişleri listelenmelidir ve şuna benzemelidir
   <pre><code>
   DATA FILE       : /home/nw1adm/.hdb/nw1-di-0/SSFS_HDB.DAT
   KEY FILE        : /home/nw1adm/.hdb/nw1-di-0/SSFS_HDB.KEY
   
   KEY DEFAULT
     ENV : 10.0.0.14:<b>30313</b>
     USER: <b>SAPABAP1</b>
     DATABASE: HN1
   </code></pre>

   Çıkış varsayılan giriş IP adresi yük dengeleyicinin IP adresi değil de, sanal makineye işaret ettiğinden gösterir. Bu girişi yük dengeleyici sanal konak adına işaret edecek şekilde değiştirilmesi gerekir. Aynı bağlantı noktasını kullandığınızdan emin olun (**30313** yukarıdaki çıktıda)!

   <pre><code>
   su - <b>nw1</b>adm
   hdbuserstore SET DEFAULT <b>nw1-db</b>:<b>30313</b> <b>SAPABAP1</b> <b>&lt;password of ABAP schema&gt;</b>
   </code></pre>

## <a name="next-steps"></a>Sonraki adımlar
* [Azure sanal makineleri planlama ve uygulama SAP için][planning-guide]
* [SAP için Azure sanal makineler dağıtımı][deployment-guide]
* [SAP için Azure sanal makineleri DBMS dağıtımı][dbms-guide]
* Yüksek kullanılabilirlik ve olağanüstü durum kurtarma SAP HANA planlama azure'da (büyük örnekler) oluşturmayı öğrenmek için bkz: [SAP HANA (büyük örnekler) Azure üzerinde yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md).
* Yüksek kullanılabilirlik ve Azure vm'lerinde SAP HANA olağanüstü durum kurtarma planı oluşturmak öğrenmek için bkz: [SAP HANA, yüksek kullanılabilirlik Azure Virtual Machines'de (VM'ler)][sap-hana-ha]
