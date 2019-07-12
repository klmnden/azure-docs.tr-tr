---
title: Azure sanal makineler için yüksek kullanılabilirlik için SUSE Linux Enterprise Server SAP NetWeaver SAP uygulamaları | Microsoft Docs
description: Yüksek kullanılabilirlik Kılavuzu SAP NetWeaver için SUSE Linux Enterprise Server üzerinde SAP uygulamaları için
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: mssedusch
manager: gwallace
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/30/2019
ms.author: sedusch
ms.openlocfilehash: 16f88790d96a1e46f60db368f69155b3ad7afbef
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797498"
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms-on-suse-linux-enterprise-server-for-sap-applications"></a>SAP uygulamaları için SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde SAP NetWeaver için yüksek kullanılabilirlik

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

[suse-ha-guide]:https://www.suse.com/products/sles-for-sap/resource-library/sap-best-practices/
[suse-drbd-guide]:https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha_techguides/book_sleha_techguides.html
[suse-ha-12sp3-relnotes]:https://www.suse.com/releasenotes/x86_64/SLE-HA/12-SP3/

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged-md%2Fazuredeploy.json
[template-file-server]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-file-server-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability.md
[nfs-ha]:high-availability-guide-suse-nfs.md

Bu makalede, sanal makineleri dağıtmak, sanal makineleri yapılandırma, küme Framework'ü yüklemek ve yüksek oranda kullanılabilir bir SAP NetWeaver 7.50 sistemi yükleyin açıklar.
Örnek yapılandırmalarında yükleme komutlarını vs. ASCS örnek numarasını 00, 02 numarası Ağıranlar örnek ve SAP sistemi kimliği NW1 kullanılır. Örnekte (örneğin, sanal makineler, sanal ağlar) kaynakların adları, kullandığınız varsayılmıştır [şablon yakınsanmış][template-converged] kaynakları oluşturmak için kimliği NW1 SAP sistemiyle.

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
* [Linux'ta SAP için Azure sanal makineler dağıtımı][deployment-guide]
* [Linux'ta SAP için Azure sanal makineleri DBMS dağıtım][dbms-guide]
* [SUSE SAP HA en iyi uygulama kılavuzları][suse-ha-guide] ve SAP HANA sistem çoğaltması şirket içi kılavuzları Netweaver HA ayarlamak için gerekli tüm bilgileri içerir. Bu kılavuzlar, genel bir temel olarak kullanın. Bunlar çok daha ayrıntılı bilgi sağlar.
* [SUSE yüksek kullanılabilirlik uzantısı 12 SP3 sürüm notları][suse-ha-12sp3-relnotes]

## <a name="overview"></a>Genel Bakış

Yüksek kullanılabilirlik elde etmek için bir NFS sunucusunun SAP NetWeaver'ı gerektirir. NFS sunucusu ayrı bir kümede yapılandırılmış ve birden çok SAP sistemleri tarafından kullanılabilir.

![SAP NetWeaver-yüksek kullanılabilirlik genel bakış](./media/high-availability-guide-suse/ha-suse.png)

NFS sunucusu, SAP NetWeaver ASCS, SAP NetWeaver SCS, SAP NetWeaver Ağıranlar ve SAP HANA veritabanı sanal ana bilgisayar adı ve sanal IP adresleri kullanın. Azure üzerinde bir yük dengeleyici sanal IP adresi kullanmak için gereklidir. Aşağıdaki liste, (A) yapılandırılmasını gösterir SCS ve Ağıranlar yük dengeleyici.

> [!IMPORTANT]
> Azure sanal makinelerinde konuk işletim sistemi gibi çoklu SID, SAP ASCS/Ağıranlar SUSE Linux ile kümeleme **desteklenmiyor**. Çoklu SID kümeleme Pacemaker kümedeki farklı SID'lere sahip birden çok SAP ASCS/Ağıranlar örneklerinin yüklenmesini açıklar.

### <a name="ascs"></a>(A)SCS

* Ön uç yapılandırması
  * IP adresi 10.0.0.7
* Arka uç yapılandırması
  * (A) bir parçası olması gereken tüm sanal makinelerin birincil ağ arabirimlerine bağlı SCS/Ağıranlar küme
* Araştırma bağlantı noktası
  * Port 620<strong>&lt;nr&gt;</strong>
* Yükleme 
* Dengeleme kuralları
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

## <a name="setting-up-a-highly-available-nfs-server"></a>Yüksek oranda kullanılabilir bir NFS sunucusunu ayarlama

SAP NetWeaver taşıma ve profil dizin için paylaşılan depolama gerektirir. Okuma [SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde NFS için yüksek kullanılabilirlik][nfs-ha] bir NFS sunucusu için SAP NetWeaver ayarlama konusunda.

## <a name="setting-up-ascs"></a>(A) SCS ayarlama

Sanal makineler de dahil olmak üzere tüm gerekli Azure kaynakları dağıtmak için bir Azure şablonu github'dan kullanabilirsiniz, kullanılabilirlik kümesi ve yük dengeleyici veya kaynakları el ile dağıtabilirsiniz.

### <a name="deploy-linux-via-azure-template"></a>Azure şablonu aracılığıyla Linux dağıtın

Azure Market görüntü için SUSE Linux Enterprise Server SAP uygulamaları 12 için yeni sanal makineleri dağıtmak için kullanabileceğiniz içerir. Market görüntüsü için SAP NetWeaver kaynak aracı içerir.

Tüm gerekli kaynakları dağıtmak için Github'da hızlı başlangıç şablonlarından birini kullanabilirsiniz. Şablonu, sanal makineler, yük dengeleyici, kullanılabilirlik vb. kümesi dağıtır. Şablonu dağıtmak için aşağıdaki adımları izleyin:

1. Açık [ASCS/SCS çoklu SID şablon][template-multisid-xscs] or the [converged template][template-converged] on the Azure portal. 
   The ASCS/SCS template only creates the load-balancing rules for the SAP NetWeaver ASCS/SCS and ERS (Linux only) instances whereas the converged template also creates the load-balancing rules for a database (for example Microsoft SQL Server or SAP HANA). If you plan to install an SAP NetWeaver based system and you also want to install the database on the same machines, use the [converged template][template-converged].
1. Aşağıdaki parametreleri girin
   1. Kaynak ön eki (ASCS/SCS çoklu SID şablonu)  
      Kullanmak istediğiniz ön eki girin. Değeri, dağıtılan kaynaklar için önek olarak kullanılır.
   3. SAP sistemi kimliği (yalnızca yakınsanmış şablonu)  
      Yüklemek istediğiniz SAP sistemine SAP sistemi Kimliğini girin. Kimlik ön eki olarak dağıtılan kaynaklar için kullanılır.
   4. Yığın türü  
      SAP NetWeaver yığın türü seçin
   5. İşletim sistemi türü  
      Linux dağıtımları birini seçin. Bu örnekte, SLES 12 BYOS seçin
   6. Veritabanı türü  
      HANA seçin
   7. SAP sistemi boyutu.  
      Yeni sisteme sağlar SAP miktarı. Kaç tane SAP sistemi gerektiriyor değil eminseniz, SAP teknoloji iş ortağı veya sistem Entegratörü isteyin
   8. Sistem kullanılabilirliği  
      HA seçin
   9. Yönetici kullanıcı adı ve yönetici parolası  
      Yeni bir kullanıcı oluşturulur makinesinde oturum açma için kullanılabilir.
   10. Alt ağ kimliği  
   Tanımlanan bir alt ağa sahip olduğunuz mevcut bir Vnet'te VM dağıtmak istiyorsanız, VM atanmalıdır belirli bir alt ağ kimliği adı için. Kimliği genellikle /subscriptions/ gibi görünüyor **&lt;abonelik kimliği&gt;** /resourceGroups/ **&lt;kaynak grubu adı&gt;** /providers/ Microsoft.Network/virtualNetworks/ **&lt;sanal ağ adı&gt;** /subnets/ **&lt;alt ağ adı&gt;**

### <a name="deploy-linux-manually-via-azure-portal"></a>Linux Azure Portalı aracılığıyla el ile dağıtma

Önce bu NFS küme için sanal makineler oluşturmak gerekir. Ardından, yük dengeleyici oluşturma ve arka uç havuzlarında sanal makinelerini kullanın.

1. Kaynak Grubu oluşturma
1. Sanal ağ oluşturma
1. Kullanılabilirlik kümesi oluşturma  
   Kümesi en çok güncelleştirme etki alanı
1. 1 sanal makine oluşturma  
   En azından bu örnek SLES4SAP 12 SP1 görüntüsünde SLES4SAP 12 SP1 https://portal.azure.com/#create/SUSE.SUSELinuxEnterpriseServerforSAPApplications12SP1PremiumImage-ARM  
   SLES için SAP uygulamaları 12 SP1 kullanılır  
   Daha önce oluşturduğunuz kullanılabilirlik kümesi seçin  
1. 2 sanal makine oluşturma  
   En azından bu örnek SLES4SAP 12 SP1 görüntüsünde SLES4SAP 12 SP1 https://portal.azure.com/#create/SUSE.SUSELinuxEnterpriseServerforSAPApplications12SP1PremiumImage-ARM  
   SLES için SAP uygulamaları 12 SP1 kullanılır  
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

Bağlantısındaki [SLES azure'daki SUSE Linux Enterprise Server üzerinde Pacemaker ayarlama](high-availability-guide-suse-pacemaker.md) temel Pacemaker küme bu (A) SCS sunucusu.

### <a name="installation"></a>Yükleme

Aşağıdaki öğeler ile önek **[A]** - tüm düğümler için geçerli **[1]** - düğüm 1 yalnızca uygulanabilir veya **[2]** - yalnızca düğüm 2 için geçerlidir.

1. **[A]**  SUSE bağlayıcısını

   <pre><code>sudo zypper install sap-suse-cluster-connector
   </code></pre>

   > [!NOTE]
   > Tire, küme düğümlerinin ana bilgisayar adları kullanmayın. Aksi takdirde, küme çalışmaz. Bu bilinen bir sınırlamadır ve SUSE bir düzeltme üzerinde çalışmaktadır. Sap suse bulut Bağlayıcısı paket bir düzeltme eki bir düzeltme kullanıma sunulacaktır.

   SAP SUSE Küme Bağlayıcısı yeni sürümünün yüklü olduğundan emin olun. Eskisini sap_suse_cluster_connector çağrıldı ve yeni bir tane çağrılır **sap suse Küme Bağlayıcısı**.

   ```
   sudo zypper info sap-suse-cluster-connector
   
   Information for package sap-suse-cluster-connector:
   ---------------------------------------------------
   Repository     : SLE-12-SP3-SAP-Updates
   Name           : sap-suse-cluster-connector
   <b>Version        : 3.0.0-2.2</b>
   Arch           : noarch
   Vendor         : SUSE LLC <https://www.suse.com/>
   Support Level  : Level 3
   Installed Size : 41.6 KiB
   <b>Installed      : Yes</b>
   Status         : up-to-date
   Source package : sap-suse-cluster-connector-3.0.0-2.2.src
   Summary        : SUSE High Availability Setup for SAP Products
   ```

1. **[A]**  Güncelleştirme SAP kaynak aracıları  
   
   Aracılar kaynak paketi için bir düzeltme eki, bu makalede açıklanan yeni yapılandırmayı kullanmak için gereklidir. Aşağıdaki komutla düzeltme eki zaten yüklediyseniz, kontrol edebilirsiniz

   <pre><code>sudo grep 'parameter name="IS_ERS"' /usr/lib/ocf/resource.d/heartbeat/SAPInstance
   </code></pre>

   Çıktı aşağıdakine benzer olmalıdır.

   <pre><code>&lt;parameter name="IS_ERS" unique="0" required="0"&gt;
   </code></pre>

   Grep komut IS_ERS parametresi bulamazsa, listelenen düzeltme ekini yüklemeniz gerekir [SUSE indirme sayfası](https://download.suse.com/patch/finder/#bu=suse&familyId=&productId=&dateRange=&startDate=&endDate=&priority=&architecture=&keywords=resource-agents)

   <pre><code># example for patch for SLES 12 SP1
   sudo zypper in -t patch SUSE-SLE-HA-12-SP1-2017-885=1
   # example for patch for SLES 12 SP2
   sudo zypper in -t patch SUSE-SLE-HA-12-SP2-2017-886=1
   </code></pre>

1. **[A]**  Kurulum ana bilgisayar adı çözümlemesi

   Bir DNS sunucusu kullanabilir veya/etc/hosts tüm düğümlerde değiştirin. Bu örnek/Etc/Hosts dosyasının nasıl kullanılacağını gösterir.
   IP adresi ve aşağıdaki komutlarda bulunan ana bilgisayar adını değiştirin

   <pre><code>sudo vi /etc/hosts
   </code></pre>

   / Etc/hosts aşağıdaki satırları ekleyin. IP adresi ve ana bilgisayar adını, ortamınızla eşleşecek şekilde değiştirin.   

   <pre><code># IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nw1-nfs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS
   <b>10.0.0.7 nw1-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS ERS
   <b>10.0.0.8 nw1-aers</b>
   # IP address of the load balancer frontend configuration for database
   <b>10.0.0.13 nw1-db</b>
   </code></pre>

## <a name="prepare-for-sap-netweaver-installation"></a>SAP NetWeaver yüklemesi için hazırlama

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

1. **[A]**  Autofs yapılandırın

   <pre><code>sudo vi /etc/auto.master
   
   # Add the following line to the file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   Bir dosya oluşturun

   <pre><code>sudo vi /etc/auto.direct
   
   # Add the following lines to the file, save and exit
   /sapmnt/<b>NW1</b> -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/trans
   /usr/sap/<b>NW1</b>/SYS -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/sidsys
   </code></pre>

   Yeni paylaşımlar bağlamak autofs yeniden başlatın

   <pre><code>sudo systemctl enable autofs
   sudo service autofs restart
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


### <a name="installing-sap-netweaver-ascsers"></a>SAP NetWeaver ASCS/Ağıranlar yükleme

1. **[1]**  Sanal IP kaynak ve sistem durumu araştırması ASCS örneği oluşturma

   <pre><code>sudo crm node standby <b>nw1-cl-1</b>
   
   sudo crm configure primitive fs_<b>NW1</b>_ASCS Filesystem device='<b>nw1-nfs</b>:/<b>NW1</b>/ASCS' directory='/usr/sap/<b>NW1</b>/ASCS<b>00</b>' fstype='nfs4' \
     op start timeout=60s interval=0 \
     op stop timeout=60s interval=0 \
     op monitor interval=20s timeout=40s
   
   sudo crm configure primitive vip_<b>NW1</b>_ASCS IPaddr2 \
     params ip=<b>10.0.0.7</b> cidr_netmask=<b>24</b> \
     op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>NW1</b>_ASCS anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k 620<b>00</b>" \
     op monitor timeout=20s interval=10 depth=0
   
   sudo crm configure group g-<b>NW1</b>_ASCS fs_<b>NW1</b>_ASCS nc_<b>NW1</b>_ASCS vip_<b>NW1</b>_ASCS \
      meta resource-stickiness=3000
   </code></pre>

   Küme durumunun Tamam olduğunu ve tüm kaynakları başlatıldığından emin olun. Hangi düğümünde kaynaklarını çalıştıran önemli değildir.

   <pre><code>sudo crm_mon -r
   
   # Node nw1-cl-1: standby
   # <b>Online: [ nw1-cl-0 ]</b>
   # 
   # Full list of resources:
   # 
   # stonith-sbd     (stonith:external/sbd): <b>Started nw1-cl-0</b>
   #  Resource Group: g-NW1_ASCS
   #      fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nw1-cl-0</b>
   #      nc_NW1_ASCS        (ocf::heartbeat:anything):      <b>Started nw1-cl-0</b>
   #      vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-0</b>
   </code></pre>

1. **[1]**  SAP NetWeaver ASCS yükleyin  

   Örneğin IP adresini yük dengeleyici ön uç yapılandırması ASCS için eşleşen bir sanal ana bilgisayar adı kullanarak ilk düğümü üzerinde kök olarak SAP NetWeaver ASCS yükleme <b>nw1 ascs</b>, <b>10.0.0.7</b> ve örnek, örneğin yük dengeleyici araştırması için kullandığınız numarası <b>00</b>.

   SAPINST_REMOTE_ACCESS_USER sapinst parametresi, bir kök olmayan kullanıcı için sapinst bağlanmasına izin vermek için kullanabilirsiniz.

   <pre><code>sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   Bir alt klasör/usr/sap/oluşturmak yükleme başarısız olursa**NW1**/ASCS**00**, sahibi ve ASCS grubu yapmayı deneyin**00** klasörü ve yeniden deneyin.

   <pre><code>chown nw1adm /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   chgrp sapsys /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   </code></pre>

1. **[1]**  Sanal IP kaynak ve sistem durumu araştırması Ağıranlar örneği oluşturma

   <pre><code>sudo crm node online <b>nw1-cl-1</b>
   sudo crm node standby <b>nw1-cl-0</b>
   
   sudo crm configure primitive fs_<b>NW1</b>_ERS Filesystem device='<b>nw1-nfs</b>:/<b>NW1</b>/ASCSERS' directory='/usr/sap/<b>NW1</b>/ERS<b>02</b>' fstype='nfs4' \
     op start timeout=60s interval=0 \
     op stop timeout=60s interval=0 \
     op monitor interval=20s timeout=40s
   
   sudo crm configure primitive vip_<b>NW1</b>_ERS IPaddr2 \
     params ip=<b>10.0.0.8</b> cidr_netmask=<b>24</b> \
     op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>NW1</b>_ERS anything \
    params binfile="/usr/bin/nc" cmdline_options="-l -k 621<b>02</b>" \
    op monitor timeout=20s interval=10 depth=0
   
   # WARNING: Resources nc_NW1_ASCS,nc_NW1_ERS violate uniqueness for parameter "binfile": "/usr/bin/nc"
   # Do you still want to commit (y/n)? y
   
   sudo crm configure group g-<b>NW1</b>_ERS fs_<b>NW1</b>_ERS nc_<b>NW1</b>_ERS vip_<b>NW1</b>_ERS
   </code></pre>

   Küme durumunun Tamam olduğunu ve tüm kaynakları başlatıldığından emin olun. Hangi düğümünde kaynaklarını çalıştıran önemli değildir.

   <pre><code>sudo crm_mon -r
   
   # Node <b>nw1-cl-0: standby</b>
   # <b>Online: [ nw1-cl-1 ]</b>
   # 
   # Full list of resources:
   #
   # stonith-sbd     (stonith:external/sbd): <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ASCS
   #      fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nw1-cl-1</b>
   #      nc_NW1_ASCS        (ocf::heartbeat:anything):      <b>Started nw1-cl-1</b>
   #      vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ERS
   #      fs_NW1_ERS (ocf::heartbeat:Filesystem):    <b>Started nw1-cl-1</b>
   #      nc_NW1_ERS (ocf::heartbeat:anything):      <b>Started nw1-cl-1</b>
   #      vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-1</b>
   </code></pre>

1. **[2]**  SAP NetWeaver Ağıranlar yükleyin

   Örneğin IP adresini yük dengeleyici ön uç yapılandırması Ağıranlar için eşleşen bir sanal ana bilgisayar adı kullanarak ikinci düğümü kök olarak SAP NetWeaver Ağıranlar yükleme <b>nw1 aers</b>, <b>10.0.0.8</b> ve örnek, örneğin yük dengeleyici araştırması için kullandığınız numarası <b>02</b>.

   SAPINST_REMOTE_ACCESS_USER sapinst parametresi, bir kök olmayan kullanıcı için sapinst bağlanmasına izin vermek için kullanabilirsiniz.

   <pre><code>sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   > [!NOTE]
   > SWPM SP 20 PL 05 veya üzerini kullanın. Daha düşük sürümler doğru izinleri ayarlama ve yükleme başarısız olur.

   Bir alt klasör/usr/sap/oluşturmak yükleme başarısız olursa**NW1**/ERS**02**, sahibi ve Ağıranlar grubu yapmayı deneyin**02** klasörü ve yeniden deneyin.

   <pre><code>chown nw1adm /usr/sap/<b>NW1</b>/ERS<b>02</b>
   chgrp sapsys /usr/sap/<b>NW1</b>/ERS<b>02</b>
   </code></pre>


1. **[1]**  Adapt ASCS/SCS ve Ağıranlar örnek profilleri
 
   * ASCS/SCS profili

   <pre><code>sudo vi /sapmnt/<b>NW1</b>/profile/<b>NW1</b>_<b>ASCS00</b>_<b>nw1-ascs</b>
   
   # Change the restart command to a start command
   #Restart_Program_01 = local $(_EN) pf=$(_PF)
   Start_Program_01 = local $(_EN) pf=$(_PF)
   
   # Add the following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector
   
   # Add the keep alive parameter
   enque/encni/set_so_keepalive = true
   </code></pre>

   * Cıların profili

   <pre><code>sudo vi /sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b>
   
   # Change the restart command to a start command
   #Restart_Program_00 = local $(_ER) pf=$(_PFL) NR=$(SCSID)
   Start_Program_00 = local $(_ER) pf=$(_PFL) NR=$(SCSID)
   
   # Add the following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector
   
   # remove Autostart from ERS profile
   # Autostart = 1
   </code></pre>

1. **[A]**  Tutmayı yapılandırma

   SAP NetWeaver uygulama sunucusu ve ASCS/SCS arasındaki iletişimi, yazılım yük dengeleyici üzerinden yönlendirilir. Yük Dengeleyici, yapılandırılabilir bir zaman aşımından sonra etkin olmayan bağlantılarını keser. Bunu önlemek için SAP NetWeaver ASCS/SCS profilinde bir parametre ve Linux sistem ayarlarını değiştirmeniz gerekir. Okuma [SAP notu 1410736][1410736] daha fazla bilgi için.

   ASCS/SCS profili parametresi enque/encni/set_so_keepalive, son adımda zaten eklendi.

   <pre><code># Change the Linux system configuration
   sudo sysctl net.ipv4.tcp_keepalive_time=120
   </code></pre>

1. **[A]**  SAP kullanıcılar yükleme sonrasında yapılandırın

   <pre><code># Add sidadm to the haclient group
   sudo usermod -aG haclient <b>nw1</b>adm
   </code></pre>

1. **[1]**  ASCS ve Ağıranlar SAP Hizmetleri sapservice dosyaya ekleyin

   ASCS ikinci düğüme giriş hizmet ve ilk düğümü Ağıranlar hizmet girişi kopyalama ekleyin.

   <pre><code>cat /usr/sap/sapservices | grep ASCS<b>00</b> | sudo ssh <b>nw1-cl-1</b> "cat >>/usr/sap/sapservices"
   sudo ssh <b>nw1-cl-1</b> "cat /usr/sap/sapservices" | grep ERS<b>02</b> | sudo tee -a /usr/sap/sapservices
   </code></pre>

1. **[1]**  SAP küme kaynaklarını oluşturma

Sıraya alma 1 sunucusu mimarisi (ENSA1) kullanıyorsanız, kaynakları gibi tanımlayın:

   <pre><code>sudo crm configure property maintenance-mode="true"
   
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

  Kuyruğa sunucu çoğaltma, SAP KB 7.52 itibarıyla dahil olmak üzere 2 için sunulan destek SAP. Sıraya alma sunucu 2 ABAP Platform 1809 ile başlayarak, varsayılan olarak yüklenir. SAP bkz Not [2630416](https://launchpad.support.sap.com/#/notes/2630416) kuyruğa sunucu 2 desteği.
Sıraya alma 2 sunucu mimarisi kullanıyorsanız ([ENSA2](https://help.sap.com/viewer/cff8531bc1d9416d91bb6781e628d4e0/1709%20001/en-US/6d655c383abf4c129b0e5c8683e7ecd8.html)), kaynakları aşağıdaki gibi tanımlayın:

<pre><code>sudo crm configure property maintenance-mode="true"
   
   sudo crm configure primitive rsc_sap_<b>NW1</b>_ASCS<b>00</b> SAPInstance \
    operations \$id=rsc_sap_<b>NW1</b>_ASCS<b>00</b>-operations \
    op monitor interval=11 timeout=60 on_fail=restart \
    params InstanceName=<b>NW1</b>_ASCS<b>00</b>_<b>nw1-ascs</b> START_PROFILE="/sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ASCS<b>00</b>_<b>nw1-ascs</b>" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000
   
   sudo crm configure primitive rsc_sap_<b>NW1</b>_ERS<b>02</b> SAPInstance \
    operations \$id=rsc_sap_<b>NW1</b>_ERS<b>02</b>-operations \
    op monitor interval=11 timeout=60 on_fail=restart \
    params InstanceName=<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b> START_PROFILE="/sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b>" AUTOMATIC_RECOVER=false IS_ERS=true 
   
   sudo crm configure modgroup g-<b>NW1</b>_ASCS add rsc_sap_<b>NW1</b>_ASCS<b>00</b>
   sudo crm configure modgroup g-<b>NW1</b>_ERS add rsc_sap_<b>NW1</b>_ERS<b>02</b>
   
   sudo crm configure colocation col_sap_<b>NW1</b>_no_both -5000: g-<b>NW1</b>_ERS g-<b>NW1</b>_ASCS
   sudo crm configure order ord_sap_<b>NW1</b>_first_start_ascs Optional: rsc_sap_<b>NW1</b>_ASCS<b>00</b>:start rsc_sap_<b>NW1</b>_ERS<b>02</b>:stop symmetrical=false
   
   sudo crm node online <b>nw1-cl-0</b>
   sudo crm configure property maintenance-mode="false"
   </code></pre>

  Eski bir sürümden yükseltme ve 2 kuyruğa sunucusuna geçiş SAP bkz Not [2641019](https://launchpad.support.sap.com/#/notes/2641019). 

   Küme durumunun Tamam olduğunu ve tüm kaynakları başlatıldığından emin olun. Hangi düğümünde kaynaklarını çalıştıran önemli değildir.


   <pre><code>sudo crm_mon -r
   
   # Online: <b>[ nw1-cl-0 nw1-cl-1 ]</b>
   #
   # Full list of resources:
   #
   # stonith-sbd     (stonith:external/sbd): <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ASCS
   #      fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nw1-cl-1</b>
   #      nc_NW1_ASCS        (ocf::heartbeat:anything):      <b>Started nw1-cl-1</b>
   #      vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-1</b>
   #      rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ERS
   #      fs_NW1_ERS (ocf::heartbeat:Filesystem):    <b>Started nw1-cl-0</b>
   #      nc_NW1_ERS (ocf::heartbeat:anything):      <b>Started nw1-cl-0</b>
   #      vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-0</b>
   #      rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   <b>Started nw1-cl-0</b>
   </code></pre>

## <a name="2d6008b0-685d-426c-b59e-6cd281fd45d7"></a>SAP NetWeaver uygulama sunucusu hazırlığı

Bazı veritabanları veritabanı örneği yüklemesi bir uygulama sunucusunda yürütülür gerektirir. Bu gibi durumlarda kullanılacak kullanabilmek için uygulama sunucusu sanal makine hazırlayın.

Adımları aşağıdaki varsayılır uygulama sunucusu ASCS/SCS ve HANA sunucularından farklı bir sunucuya yükleyin. Aksi takdirde (örneğin, ana bilgisayar adı çözümlemesi yapılandırma) adımlardan bazıları gerekli değildir.

1. İşletim sistemini Yapılandır

   Kirli önbellek boyutunu küçültün. Daha fazla bilgi için [düşük performans SLES 11/12 üzerinde yazma büyük RAM sunucularıyla](https://www.suse.com/support/kb/doc/?id=7010287).

   <pre><code>sudo vi /etc/sysctl.conf

   # Change/set the following settings
   vm.dirty_bytes = 629145600
   vm.dirty_background_bytes = 314572800
   </code></pre>

1. Ana bilgisayar adı çözümlemesi Kurulumu

   Bir DNS sunucusu kullanabilir veya/etc/hosts tüm düğümlerde değiştirin. Bu örnek/Etc/Hosts dosyasının nasıl kullanılacağını gösterir.
   IP adresi ve aşağıdaki komutlarda bulunan ana bilgisayar adını değiştirin

   ```bash
   sudo vi /etc/hosts
   ```

   / Etc/hosts aşağıdaki satırları ekleyin. IP adresi ve ana bilgisayar adını, ortamınızla eşleşecek şekilde değiştirin.

   <pre><code># IP address of the load balancer frontend configuration for NFS
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

1. Sapmnt dizini oluşturma

   <pre><code>sudo mkdir -p /sapmnt/<b>NW1</b>
   sudo mkdir -p /usr/sap/trans

   sudo chattr +i /sapmnt/<b>NW1</b>
   sudo chattr +i /usr/sap/trans
   </code></pre>

1. AutoFS yapılandırın

   <pre><code>sudo vi /etc/auto.master
   
   # Add the following line to the file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   Yeni bir dosya oluşturun

   <pre><code>sudo vi /etc/auto.direct
   
   # Add the following lines to the file, save and exit
   /sapmnt/<b>NW1</b> -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/trans
   </code></pre>

   Yeni paylaşımlar bağlamak autofs yeniden başlatın

   <pre><code>sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. TAKAS dosyası yapılandırma

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

## <a name="install-database"></a>Veritabanı Yükleme

Bu örnekte, SAP HANA'da SAP NetWeaver yüklenir. Bu yükleme için desteklenen her veritabanı kullanabilirsiniz. Azure'da SAP HANA yükleme hakkında daha fazla bilgi için bkz. [SAP HANA, yüksek kullanılabilirlik Azure Virtual Machines'de (VM'ler)][sap-hana-ha]. For a list of supported databases, see [SAP Note 1928533][1928533].

1. SAP veritabanı örneğini yüklemeyi çalıştırın

   SAP NetWeaver veritabanı örneği örnek veritabanı için yük dengeleyici ön uç yapılandırması IP adresine eşleyen bir sanal ana bilgisayar adı kullanarak kök yükleme <b>nw1 db</b> ve <b>10.0.0.13</b>.

   SAPINST_REMOTE_ACCESS_USER sapinst parametresi, bir kök olmayan kullanıcı için sapinst bağlanmasına izin vermek için kullanabilirsiniz.

   <pre><code>sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

## <a name="sap-netweaver-application-server-installation"></a>SAP NetWeaver uygulama sunucusu yükleme

SAP uygulama sunucusu yüklemek için aşağıdaki adımları izleyin.

1. Uygulama sunucusu hazırlama

   Bu bölümdeki adımları [SAP NetWeaver uygulama sunucusu hazırlığı](high-availability-guide-suse.md#2d6008b0-685d-426c-b59e-6cd281fd45d7) uygulama sunucuyu hazırlamak için yukarıdaki.

1. SAP NetWeaver uygulama sunucusu yükleme

   Bir birincil veya ek SAP NetWeaver uygulamalarını sunucuya yükleyin.

   SAPINST_REMOTE_ACCESS_USER sapinst parametresi, bir kök olmayan kullanıcı için sapinst bağlanmasına izin vermek için kullanabilirsiniz.

   <pre><code>sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. SAP HANA Güvenli Depo güncelleştirme

   SAP HANA Güvenli Depo SAP HANA sistem çoğaltması Kurulumu sanal adına işaret edecek şekilde güncelleştirin.

   Girişleri listelemek için aşağıdaki komutu çalıştırın
   <pre><code>hdbuserstore List
   </code></pre>

   Bu tüm girişleri listeler ve benzer şekilde görünmelidir
   <pre><code>DATA FILE       : /home/nw1adm/.hdb/nw1-di-0/SSFS_HDB.DAT
   KEY FILE        : /home/nw1adm/.hdb/nw1-di-0/SSFS_HDB.KEY
   
   KEY DEFAULT
     ENV : 10.0.0.14:<b>30313</b>
     USER: <b>SAPABAP1</b>
     DATABASE: <b>HN1</b>
   </code></pre>

   Çıktı, varsayılan giriş IP adresini sanal makineye ve load balancer'ın IP adresi işaret ettiğini gösterir. Bu girişi yük dengeleyicinin sanal ana bilgisayar için işaret edecek şekilde değiştirilmesi gerekir. Aynı bağlantı noktasını kullandığınızdan emin olun (**30313** yukarıdaki çıktıda) ve veritabanı adını (**HN1** yukarıdaki çıktıda)!

   <pre><code>su - <b>nw1</b>adm
   hdbuserstore SET DEFAULT <b>nw1-db:30313@HN1</b> <b>SAPABAP1</b> <b>&lt;password of ABAP schema&gt;</b>
   </code></pre>

## <a name="test-the-cluster-setup"></a>Test kümesi Kurulumu

En iyi uygulamalar kılavuzları SUSE, test çalışmalarını bir kopyasını testlerdir. Kolaylık olması için kopyalanırlar. Ayrıca her zaman en iyi uygulamalar Kılavuzları'nı okuyun ve eklenmiş tüm ek testler gerçekleştirin.

1. Test HAGetFailoverConfig ve HACheckConfig HACheckFailoverConfig

   Olarak aşağıdaki komutları çalıştırın \<sapsid > adm ASCS örneği şu anda çalıştığı düğüm üzerinde. Komutları hata ile başarısız olursa: Yetersiz bellek, ana bilgisayar adı tirelerin tarafından kaynaklanabilir. Bu bilinen bir sorundur ve sap suse Küme Bağlayıcısı paketi SUSE göre düzeltilecektir.

   <pre><code>nw1-cl-0:nw1adm 54> sapcontrol -nr <b>00</b> -function HAGetFailoverConfig
   
   # 15.08.2018 13:50:36
   # HAGetFailoverConfig
   # OK
   # HAActive: TRUE
   # HAProductVersion: Toolchain Module
   # HASAPInterfaceVersion: Toolchain Module (sap_suse_cluster_connector 3.0.1)
   # HADocumentation: https://www.suse.com/products/sles-for-sap/resource-library/sap-best-practices/
   # HAActiveNode:
   # HANodes: nw1-cl-0, nw1-cl-1
   
   nw1-cl-0:nw1adm 55> sapcontrol -nr 00 -function HACheckConfig
   
   # 15.08.2018 14:00:04
   # HACheckConfig
   # OK
   # state, category, description, comment
   # SUCCESS, SAP CONFIGURATION, Redundant ABAP instance configuration, 2 ABAP instances detected
   # SUCCESS, SAP CONFIGURATION, Redundant Java instance configuration, 0 Java instances detected
   # SUCCESS, SAP CONFIGURATION, Enqueue separation, All Enqueue server separated from application server
   # SUCCESS, SAP CONFIGURATION, MessageServer separation, All MessageServer separated from application server
   # SUCCESS, SAP CONFIGURATION, ABAP instances on multiple hosts, ABAP instances on multiple hosts detected
   # SUCCESS, SAP CONFIGURATION, Redundant ABAP SPOOL service configuration, 2 ABAP instances with SPOOL service detected
   # SUCCESS, SAP STATE, Redundant ABAP SPOOL service state, 2 ABAP instances with active SPOOL service detected
   # SUCCESS, SAP STATE, ABAP instances with ABAP SPOOL service on multiple hosts, ABAP instances with active ABAP SPOOL service on multiple hosts detected
   # SUCCESS, SAP CONFIGURATION, Redundant ABAP BATCH service configuration, 2 ABAP instances with BATCH service detected
   # SUCCESS, SAP STATE, Redundant ABAP BATCH service state, 2 ABAP instances with active BATCH service detected
   # SUCCESS, SAP STATE, ABAP instances with ABAP BATCH service on multiple hosts, ABAP instances with active ABAP BATCH service on multiple hosts detected
   # SUCCESS, SAP CONFIGURATION, Redundant ABAP DIALOG service configuration, 2 ABAP instances with DIALOG service detected
   # SUCCESS, SAP STATE, Redundant ABAP DIALOG service state, 2 ABAP instances with active DIALOG service detected
   # SUCCESS, SAP STATE, ABAP instances with ABAP DIALOG service on multiple hosts, ABAP instances with active ABAP DIALOG service on multiple hosts detected
   # SUCCESS, SAP CONFIGURATION, Redundant ABAP UPDATE service configuration, 2 ABAP instances with UPDATE service detected
   # SUCCESS, SAP STATE, Redundant ABAP UPDATE service state, 2 ABAP instances with active UPDATE service detected
   # SUCCESS, SAP STATE, ABAP instances with ABAP UPDATE service on multiple hosts, ABAP instances with active ABAP UPDATE service on multiple hosts detected
   # SUCCESS, SAP STATE, SCS instance running, SCS instance status ok
   # SUCCESS, SAP CONFIGURATION, SAPInstance RA sufficient version (nw1-ascs_NW1_00), SAPInstance includes is-ers patch
   # SUCCESS, SAP CONFIGURATION, Enqueue replication (nw1-ascs_NW1_00), Enqueue replication enabled
   # SUCCESS, SAP STATE, Enqueue replication state (nw1-ascs_NW1_00), Enqueue replication active
   
   nw1-cl-0:nw1adm 56> sapcontrol -nr 00 -function HACheckFailoverConfig
   
   # 15.08.2018 14:04:08
   # HACheckFailoverConfig
   # OK
   # state, category, description, comment
   # SUCCESS, SAP CONFIGURATION, SAPInstance RA sufficient version, SAPInstance includes is-ers patch
   </code></pre>

1. ASCS örneği el ile geçirme

   Kaynak durumu, test başlamadan önce:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

   Kök olarak ASCS örneği geçirmek için aşağıdaki komutları çalıştırın.

   <pre><code>nw1-cl-0:~ # crm resource migrate rsc_sap_NW1_ASCS00 force
   # INFO: Move constraint created for rsc_sap_NW1_ASCS00
   
   nw1-cl-0:~ # crm resource unmigrate rsc_sap_NW1_ASCS00
   # INFO: Removed migration constraints for rsc_sap_NW1_ASCS00
   
   # Remove failed actions for the ERS that occurred as part of the migration
   nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   Kaynak durumu test sonra:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

1. Test HAFailoverToNode

   Kaynak durumu, test başlamadan önce:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

   Olarak aşağıdaki komutları çalıştırın \<sapsid > adm ASCS örneği geçirilecek.

   <pre><code>nw1-cl-0:nw1adm 55> sapcontrol -nr 00 -host nw1-ascs -user nw1adm &lt;password&gt; -function HAFailoverToNode ""
   
   # run as root
   # Remove failed actions for the ERS that occurred as part of the migration
   nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ERS02
   # Remove migration constraints
   nw1-cl-0:~ # crm resource clear rsc_sap_NW1_ASCS00
   #INFO: Removed migration constraints for rsc_sap_NW1_ASCS00
   </code></pre>

   Kaynak durumu test sonra:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

1. Düğüm kilitlenmesi benzetimi

   Kaynak durumu, test başlamadan önce:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

   Düğümdeki kök olarak ASCS örneğinin çalıştığı aşağıdaki komutu çalıştırın

   <pre><code>nw1-cl-0:~ # echo b > /proc/sysrq-trigger
   </code></pre>

   SBD kullanırsanız, otomatik olarak sonlandırılan düğüm üzerinde Pacemaker başlamamalıdır. Düğümü yeniden başlatıldıktan sonra durumu yeniden şu şekilde görünmelidir.

   <pre><code>Online: [ nw1-cl-1 ]
   OFFLINE: [ nw1-cl-0 ]
   
   Full list of resources:
   
   stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   
   Failed Actions:
   * rsc_sap_NW1_ERS02_monitor_11000 on nw1-cl-1 'not running' (7): call=219, status=complete, exitreason='none',
       last-rc-change='Wed Aug 15 14:38:38 2018', queued=0ms, exec=0ms
   </code></pre>

   Pacemaker sonlandırılan bir düğümde başlamaya SBD iletileri temizlemek ve başarısız kaynakları temizlemek için aşağıdaki komutları kullanın.

   <pre><code># run as root
   # list the SBD device(s)
   nw1-cl-0:~ # cat /etc/sysconfig/sbd | grep SBD_DEVICE=
   # SBD_DEVICE="/dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116;/dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1;/dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3"
   
   nw1-cl-0:~ # sbd -d /dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116 -d /dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1 -d /dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3 message nw1-cl-0 clear
   
   nw1-cl-0:~ # systemctl start pacemaker
   nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ASCS00
   nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   Kaynak durumu test sonra:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

1. Test ASCS örneğinin el ile yeniden başlatma

   Kaynak durumu, test başlamadan önce:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

   Örnek düzenleme işlem su01 bir kullanıcı tarafından kuyruğa kilit oluşturun. Olarak aşağıdaki komutları çalıştırın \<sapsid > adm ASCS örneğinin çalıştığı düğüm üzerinde. Komutlar ASCS örneğini durdurun ve yeniden başlatın. Sıraya alma 1 sunucusu mimarisi kullanarak, sıraya alma kilidi bu sınamada kaybolur beklenir. Sıraya alma 2 sunucu mimarisi kullanarak, kuyruğa korunur. 

   <pre><code>nw1-cl-1:nw1adm 54> sapcontrol -nr 00 -function StopWait 600 2
   </code></pre>

   ASCS örneği artık Pacemaker içinde devre dışı bırakılmalıdır

   <pre><code>rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Stopped (disabled)
   </code></pre>

   ASCS örneği aynı düğümde yeniden başlatın.

   <pre><code>nw1-cl-1:nw1adm 54> sapcontrol -nr 00 -function StartWait 600 2
   </code></pre>

   İşlem su01, sıraya alma kilidi, kayıp olmalıdır ve arka uç sıfırlamalısınız. Kaynak durumu test sonra:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

1. İleti sunucu işlemi Sonlandır

   Kaynak durumu, test başlamadan önce:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

   Kök olarak ileti sunucusu işlemi belirleyip bunu sonlandırmak için aşağıdaki komutları çalıştırın.

   <pre><code>nw1-cl-1:~ # pgrep ms.sapNW1 | xargs kill -9
   </code></pre>

   Yalnızca ileti sunucusu kez KILL, sapstart tarafından başlatılacak. Bu genellikle yeterli Pacemaker sonunda olacak KILL ASCS örneği başka bir düğüme taşıyın. Kaynak durumunu ASCS ve Ağıranlar örneğinin sonra test temizlemek için kök olarak aşağıdaki komutları çalıştırın.

   <pre><code>nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ASCS00
   nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   Kaynak durumu test sonra:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

1. Sıraya alma sunucu işlemi Sonlandır

   Kaynak durumu, test başlamadan önce:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

   Aşağıdaki komutları kök olarak, kuyruğa sunucunun sonlandırmak için ASCS örneğinin çalıştığı düğüm üzerinde çalıştırın.

   <pre><code>nw1-cl-0:~ # pgrep en.sapNW1 | xargs kill -9
   </code></pre>

   ASCS örneği hemen başka bir düğüme yük devretme. ASCS başlatıldıktan sonra Ağıranlar örneği de yük devretme. Kaynak durumunu ASCS ve Ağıranlar örneğinin sonra test temizlemek için kök olarak aşağıdaki komutları çalıştırın.

   <pre><code>nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ASCS00
   nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   Kaynak durumu test sonra:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

1. Sıraya alma çoğaltma sunucusu işlemini sonlandır

   Kaynak durumu, test başlamadan önce:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

   Aşağıdaki komutu, sıraya alma çoğaltma sunucusu işlemi sonlandırmak için Ağıranlar örneğinin çalıştığı düğüm üzerinde kök olarak çalıştırın.

   <pre><code>nw1-cl-0:~ # pgrep er.sapNW1 | xargs kill -9
   </code></pre>

   Yalnızca komutu bir kez çalıştırırsanız, sapstart işlemini yeniden başlatır. Bunu çalıştırırsanız, genellikle yeterli sapstart işlem yeniden ve kaynak durdurulmuş durumda olacaktır. Kaynak durumunu Ağıranlar örneğinin sonra test temizlemek için kök olarak aşağıdaki komutları çalıştırın.

   <pre><code>nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   Kaynak durumu test sonra:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

1. Sıraya alma sapstartsrv işlemini sonlandır

   Kaynak durumu, test başlamadan önce:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

   Aşağıdaki komutları kök olarak, ASCS çalıştığı düğüm üzerinde çalıştırın.

   <pre><code>nw1-cl-1:~ # pgrep -fl ASCS00.*sapstartsrv
   # 59545 sapstartsrv
   
   nw1-cl-1:~ # kill -9 59545
   </code></pre>

   Sapstartsrv işlemi her zaman Pacemaker kaynak aracı tarafından yeniden başlatılması. Kaynak durumu test sonra:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

## <a name="next-steps"></a>Sonraki adımlar

* [Azure sanal makineleri planlama ve uygulama için SAP][planning-guide]
* [SAP için Azure sanal makineler dağıtımı][deployment-guide]
* [SAP için Azure sanal makineleri DBMS dağıtım][dbms-guide]
* Yüksek kullanılabilirlik ve olağanüstü durum kurtarma SAP hana (büyük örnekler) azure'da planlama oluşturma hakkında bilgi almak için bkz: [SAP HANA (büyük örnekler) azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md).
* Yüksek kullanılabilirlik ve Azure Vm'leri üzerinde SAP hana olağanüstü durum kurtarma planı oluşturma hakkında bilgi almak için bkz: [SAP HANA, yüksek kullanılabilirlik Azure Virtual Machines'de (VM'ler)][sap-hana-ha]
