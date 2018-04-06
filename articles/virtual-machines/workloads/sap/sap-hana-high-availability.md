---
title: SAP HANA sistem çoğaltma Azure sanal makinelerde (VM'ler) Kurulumu | Microsoft Docs
description: SAP HANA Azure sanal makinelerde (VM'ler) yüksek kullanılabilirliğini kurun.
services: virtual-machines-linux
documentationcenter: ''
author: MSSedusch
manager: timlt
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/24/2018
ms.author: sedusch
ms.openlocfilehash: e3fb06309dabd7f66d5873e4c5faa48b468854f6
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="high-availability-of-sap-hana-on-azure-virtual-machines-vms"></a>SAP HANA Azure sanal makinelerde (VM), yüksek kullanılabilirlik

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
[2388694]:https://launchpad.support.sap.com/#/notes/2388694

[hana-ha-guide-replication]:sap-hana-high-availability.md#14c19f65-b5aa-4856-9594-b81c7e4df73d
[hana-ha-guide-shared-storage]:sap-hana-high-availability.md#498de331-fa04-490b-997c-b078de457c9d

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[sap-swcenter]:https://launchpad.support.sap.com/#/softwarecenter
[template-multisid-db]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged%2Fazuredeploy.json

Şirket içi ya da HANA sistemi çoğaltması kullanmak veya yüksek kullanılabilirlik için SAP HANA kurmak için paylaşılan depolama alanı kullanın.
Yalnızca yüksek oranda kullanılabilirlik işlevi şu ana kadar desteklenen Azure VM'ler HANA sistem çoğaltma azure'da açıktır. SAP HANA çoğaltma, bir birincil düğüm ve en az bir ikincil düğümü oluşur. Birincil düğümdeki verilerde yapılan değişiklikleri ikincil düğüme eşzamanlı veya zaman uyumsuz olarak çoğaltılır.

Bu makalede, sanal makineleri dağıtmak, sanal makineleri yapılandırma, küme Framework'ü yüklemek, yükleme ve SAP HANA sistem çoğaltma yapılandırma açıklar.
Örnek yapılandırmaları, vb. örnek numarasını 03 yükleme komutları ve HANA sistem kimliği HN1 kullanılır.

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
* [SAP HANA SR performansı en iyi duruma getirilmiş senaryo] [ suse-hana-ha-guide] kılavuz SAP HANA sistem çoğaltma şirket içi yedekleme ayarlamak için gerekli tüm bilgileri içerir. Bu kılavuz, bir taban çizgisi olarak kullanın.

## <a name="overview"></a>Genel Bakış

Yüksek kullanılabilirlik elde etmek için SAP HANA iki sanal makinelerde yüklü. Veriler HANA sistem çoğaltma kullanarak çoğaltılır.

![SAP HANA yüksek kullanılabilirlik genel bakış](./media/sap-hana-high-availability/ha-suse-hana.png)

Ayrılmış sanal ana bilgisayar adı ve sanal IP adresleri SAP HANA SR Kurulum kullanır. Azure üzerinde bir yük dengeleyici sanal IP adresi kullanmak için gereklidir. Aşağıdaki listede, yük dengeleyici yapılandırması gösterilmektedir.

* Ön uç yapılandırma
  * IP adresi 10.0.0.13 hn1 db
* Arka uç yapılandırması
  * HANA sistem çoğaltma parçası olması gereken tüm sanal makinelerin birincil ağ arabirimlerine bağlı
* Sonda bağlantı noktası
  * Bağlantı noktası 62503
* Loadbalancing kuralları
  * 30313 TCP
  * 30315 TCP
  * 30317 TCP

## <a name="deploying-linux"></a>Linux dağıtma

SAP HANA kaynak Aracısı SUSE Linux Enterprise Server SAP uygulamaları için dahil edilir.
Azure Market SAP uygulamaları 12 için yeni sanal makineleri dağıtmak için kullanabileceğiniz SUSE Linux Enterprise Server için bir görüntü içerir.

### <a name="deploy-with-template"></a>Şablon ile dağıtım
Tüm gerekli kaynakları dağıtmak için github'da hızlı başlangıç şablonlarından birini kullanabilirsiniz. Şablonun sanal makineler, yük dengeleyici, kullanılabilirlik vb. kümesi dağıtır. Şablonu dağıtmak için aşağıdaki adımları izleyin:

1. Açık [veritabanı şablonu] [ template-multisid-db] veya [şablonu Yakınsanan] [ template-converged] Azure portalındaki. 
   Yakınsanmış şablon ayrıca ASCS/SCS ve ERS (yalnızca Linux) örneği için Yük Dengeleme kuralları oluşturur ancak veritabanı şablonu yalnızca bir veritabanı için Yük Dengeleme kuralları oluşturur. SAP NetWeaver temel sistem yüklemeyi planladığınız ve ayrıca istiyorsanız ASCS/SCS örneği aynı makinelere yüklemeniz, kullanın [şablonu Yakınsanan][template-converged].
1. Aşağıdaki parametreleri girin
    1. SAP sistem kimliği  
       Yüklemek istediğiniz SAP sistem SAP sistem Kimliğini girin. Dağıtılan kaynaklar için önek olarak kullanılacak kimliği geçiyor.
    1. Yığın türü (yalnızca yakınsanmış şablonunu kullanıyorsanız geçerlidir)   
       SAP NetWeaver yığın türünü seçin
    1. İşletim sistemi türü  
       Linux dağıtımları birini seçin. Bu örnekte, SLES 12 seçin
    1. Db Türü  
       HANA seçin
    1. SAP sistemi boyutu  
       SAP miktarını sağlamak için yeni sistem adımıdır. Sistem gerektirir kaç SAP değil eminseniz, SAP teknolojisi iş ortağı veya sistem Tümleştirici isteyin
    1. Sistem kullanılabilirliği  
       HA seçin
    1. Yönetici kullanıcı adı ve yönetici parolası  
       Yeni bir kullanıcı oluşturulur makineye oturum açmak için kullanılabilir.
    1. Yeni veya var olan bir alt ağ  
       Yeni sanal ağ ve alt oluşturulmalıdır ya da mevcut bir alt kullanılması gerektiğini belirler. Şirket içi ağınıza bağlı bir sanal ağ zaten varsa, varolan seçin.
    1. Alt ağ kimliği  
    Sanal makineler için bağlanması alt ağ kimliği. Sanal makine, şirket içi ağınıza bağlanmak için VPN veya hızlı rota sanal ağınızın alt ağ seçin. Kimliği genellikle /subscriptions/ gibi görünüyor`<subscription ID`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`>

### <a name="manual-deployment"></a>El ile dağıtımı

1. Kaynak Grubu oluşturma
1. Sanal Ağ Oluştur
1. Kullanılabilirlik kümesi oluştur  
   Set max güncelleştirme etki alanı
1. Bir yük dengeleyiciye (dahili) oluşturma  
   İkinci oluşturulan sanal ağ seçin
1. Sanal makine 1 oluşturun  
   En azından SLES4SAP 12 SP2 görüntü kullanacağız Bu örnekte SLES4SAP 12 SP1 https://ms.portal.azure.com/#create/SUSE.SUSELinuxEnterpriseServerforSAPApplications12SP2PremiumImage-ARM  
   SLES için SAP 12 SP2 (Premium)  
   Daha önce oluşturduğunuz kullanılabilirlik kümesi seçin  
1. Sanal makine 2 oluşturun  
   En azından SLES4SAP 12 SP1 BYOS görüntü kullanacağız Bu örnekte SLES4SAP 12 SP1 https://ms.portal.azure.com/#create/SUSE.SUSELinuxEnterpriseServerforSAPApplications12SP2PremiumImage-ARM  
   SLES için SAP 12 SP2 (Premium)  
   Daha önce oluşturduğunuz kullanılabilirlik kümesi seçin  
1. Veri diski Ekle
1. Yük Dengeleyici yapılandırma
    1. Bir ön uç IP havuzu oluşturma
        1. Yük Dengeleyici açın, ön uç IP havuzu seçin ve Ekle'yi tıklatın
        1. Yeni ön uç IP havuzu (örneğin hana-ön uç) adını girin
        1. Atama statik olarak ayarlamanız ve IP adresini girin (örneğin **10.0.0.13**)
        1. Tamam'ı tıklatın
        1. Yeni ön uç IP havuzu oluşturulduktan sonra IP adresini yazma
    1. Arka uç havuzu oluşturma
        1. Yük Dengeleyici açın, arka uç havuzlarını seçin ve Ekle'yi tıklatın
        1. Yeni arka uç havuzu (örneğin hana-arka uç) adını girin
        1. Bir sanal makine Ekle
        1. Daha önce oluşturduğunuz kullanılabilirlik kümesi seçin
        1. SAP HANA kümedeki sanal makineleri seçin
        1. Tamam'ı tıklatın
    1. Durum araştırması oluşturma
        1. Yük Dengeleyici açın, sistem durumu araştırmalarının seçin ve Ekle'yi tıklatın
        1. Yeni durum araştırması (örneğin hana-hp) adını girin
        1. TCP bağlantı noktası 625 protokolü olarak seçin**03**, aralığı 5 ile sağlıksız durum eşiği 2 tutun
        1. Tamam'ı tıklatın
    1. SAP HANA 1.0: Yük Dengeleme kuralları oluşturma
        1. Yük Dengeleyici açın, Yük Dengeleme kuralları seçin ve Ekle'yi tıklatın
        1. Yeni Yük Dengeleyici kuralı adını girin (örneğin hana-lb-3**03**15)
        1. Ön uç IP adresi, arka uç havuzu ve oluşturduğunuz durumu araştırması için daha önce (örnek hana-ön uç) seçin
        1. Protokol TCP tutmak için bağlantı noktası 3 girin**03**15
        1. -30 dakika boşta kalma zaman aşımı süresini artırın
        1. **Kayan IP etkinleştirdiğinizden emin olun**
        1. Tamam'ı tıklatın
        1. Bağlantı noktası 3 için yukarıdaki adımları yineleyin**03**17
    1. SAP HANA 2.0: Yük Dengeleme kuralları sistem veritabanı için oluşturma
        1. Yük Dengeleyici açın, Yük Dengeleme kuralları seçin ve Ekle'yi tıklatın
        1. Yeni Yük Dengeleyici kuralı adını girin (örneğin hana-lb-3**03**13)
        1. Ön uç IP adresi, arka uç havuzu ve oluşturduğunuz durumu araştırması için daha önce (örnek hana-ön uç) seçin
        1. Protokol TCP tutmak için bağlantı noktası 3 girin**03**13
        1. -30 dakika boşta kalma zaman aşımı süresini artırın
        1. **Kayan IP etkinleştirdiğinizden emin olun**
        1. Tamam'ı tıklatın
        1. Bağlantı noktası 3 için yukarıdaki adımları yineleyin**03**14
    1. SAP HANA 2.0: Yük Dengeleme kuralları için ilk Kiracı veritabanı oluşturun.
        1. Yük Dengeleyici açın, Yük Dengeleme kuralları seçin ve Ekle'yi tıklatın
        1. Yeni Yük Dengeleyici kuralı adını girin (örneğin hana-lb-3**03**40)
        1. Ön uç IP adresi, arka uç havuzu ve oluşturduğunuz durumu araştırması için daha önce (örnek hana-ön uç) seçin
        1. Protokol TCP tutmak için bağlantı noktası 3 girin**03**40
        1. -30 dakika boşta kalma zaman aşımı süresini artırın
        1. **Kayan IP etkinleştirdiğinizden emin olun**
        1. Tamam'ı tıklatın
        1. Bağlantı noktası 3 için yukarıdaki adımları yineleyin**03**41 ve 3**03**42

SAP HANA için gerekli bağlantı noktaları hakkında daha fazla bilgi için bölüm okuma [Kiracı veritabanı bağlantılarını](https://help.sap.com/viewer/78209c1d3a9b41cd8624338e42a12bf6/latest/en-US/7a9343c9f2a2436faa3cfdb5ca00c052.html) , [SAP HANA Kiracı veritabanları](https://help.sap.com/viewer/78209c1d3a9b41cd8624338e42a12bf6) kılavuzu veya [SAP Not 2388694] [2388694].


## <a name="create-pacemaker-cluster"></a>Pacemaker kümesi oluşturma

Adımları [Pacemaker SUSE Linux Enterprise Server Azure üzerinde ayarlama](high-availability-guide-suse-pacemaker.md) bu HANA sunucu için temel Pacemaker küme oluşturmak için. SAP HANA ve SAP NetWeaver (A) SCS için de aynı Pacemaker küme kullanabilirsiniz.

## <a name="installing-sap-hana"></a>SAP HANA yükleme

Aşağıdaki öğeler ile ya da önek **[A]** - tüm düğümleri için geçerli **[1]** - 1 düğümü yalnızca uygulanabilir veya **[2]** - Pacemaker kümedeki düğüm 2 yalnızca uygulanabilir.

1. **[A]**  Kurulum disk düzeni
    1. LVM  
       
       Genellikle, veri ve günlük dosyalarını birimleri için LVM kullanmanızı öneririz. Aşağıdaki örneğe sanal makineleri iki birim oluşturmak için kullanılması gereken bağlı dört veri diskleri sahip olduğunuzu varsayar.

       Kullanılabilir tüm diskleri Listele
       
       <pre><code>
       ls /dev/disk/azure/scsi1/lun*
       </code></pre>
       
       Örnek çıktı
       
       ```
       /dev/disk/azure/scsi1/lun0  /dev/disk/azure/scsi1/lun1  /dev/disk/azure/scsi1/lun2  /dev/disk/azure/scsi1/lun3
       ```
       
       Kullanmak istediğiniz tüm disklerin fiziksel birimler oluşturursunuz.    
       <pre><code>
       sudo pvcreate /dev/disk/azure/scsi1/lun0
       sudo pvcreate /dev/disk/azure/scsi1/lun1
       sudo pvcreate /dev/disk/azure/scsi1/lun2
       sudo pvcreate /dev/disk/azure/scsi1/lun3
       </code></pre>

       Veri dosyaları için bir birim grubu, günlük dosyaları için bir birim grubu ve SAP HANA paylaşılan dizini için bir tane oluşturun

       <pre><code>
       sudo vgcreate vg_hana_data_<b>HN1</b> /dev/disk/azure/scsi1/lun0 /dev/disk/azure/scsi1/lun1
       sudo vgcreate vg_hana_log_<b>HN1</b> /dev/disk/azure/scsi1/lun2
       sudo vgcreate vg_hana_shared_<b>HN1</b> /dev/disk/azure/scsi1/lun3
       </code></pre>
       
       Mantıksal birim oluşturun

       <pre><code>
       sudo lvcreate -l 100%FREE -n hana_data vg_hana_data_<b>HN1</b>
       sudo lvcreate -l 100%FREE -n hana_log vg_hana_log_<b>HN1</b>
       sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared_<b>HN1</b>
       sudo mkfs.xfs /dev/vg_hana_data_<b>HN1</b>/hana_data
       sudo mkfs.xfs /dev/vg_hana_log_<b>HN1</b>/hana_log
       sudo mkfs.xfs /dev/vg_hana_shared_<b>HN1</b>/hana_shared
       </code></pre>
       
       Bağlama dizinlerini oluşturun ve tüm mantıksal birimleri UUID'si kopyalayın
       
       <pre><code>
       sudo mkdir -p /hana/data/<b>HN1</b>
       sudo mkdir -p /hana/log/<b>HN1</b>
       sudo mkdir -p /hana/shared/<b>HN1</b>
       # write down the ID of /dev/vg_hana_data_<b>HN1</b>/hana_data, /dev/vg_hana_log_<b>HN1</b>/hana_log and /dev/vg_hana_shared_<b>HN1</b>/hana_shared
       sudo blkid
       </code></pre>
       
       Üç mantıksal birimler için fstab girişleri oluşturmak
       
       <pre><code>
       sudo vi /etc/fstab
       </code></pre>
       
       Bu satırla /etc/fstab Ekle
       
       <pre><code>
       /dev/disk/by-uuid/<b>&lt;UUID of /dev/mapper/vg_hana_data_<b>HN1</b>-hana_data&gt;</b> /hana/data/<b>HN1</b> xfs  defaults,nofail  0  2
       /dev/disk/by-uuid/<b>&lt;UUID of /dev/mapper/vg_hana_log_<b>HN1</b>-hana_log&gt;</b> /hana/log/<b>HN1</b> xfs  defaults,nofail  0  2
       /dev/disk/by-uuid/<b>&lt;UUID of /dev/mapper/vg_hana_shared_<b>HN1</b>-hana_shared&gt;</b> /hana/shared/<b>HN1</b> xfs  defaults,nofail  0  2
       </code></pre>
       
       Yeni birimlere bağlama
       
       <pre><code>
       sudo mount -a
       </code></pre>
    
    1. Düz diskleri  
       Tanıtım sistemler için HANA veri ve günlük dosyalarını bir diske yerleştirebilirsiniz. Aşağıdaki komutları /dev/disk/azure/scsi1/lun0 üzerinde bir bölüm oluşturun ve xfs ile biçimlendirin.

       <pre><code>
       sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/disk/azure/scsi1/lun0'
       sudo mkfs.xfs /dev/disk/azure/scsi1/lun0-part1
       
       # write down the ID of /dev/disk/azure/scsi1/lun0-part1
       sudo /sbin/blkid
       sudo vi /etc/fstab
       </code></pre>

       Bu satırla /etc/fstab Ekle
       <pre><code>
       /dev/disk/by-uuid/<b>&lt;UUID&gt;</b> /hana xfs  defaults,nofail  0  2
       </code></pre>

       Hedef dizin oluşturun ve diski bağlayabilir.

       <pre><code>
       sudo mkdir /hana
       sudo mount -a
       </code></pre>

1. **[A]**  Tüm ana bilgisayarlar için ana bilgisayar adı çözümlemesi Kurulumu  
    Bir DNS sunucusu kullanın veya tüm düğümlerde/etc/hosts değiştirin. Bu örnek/Etc/Hosts dosyasının nasıl kullanılacağını gösterir.
   IP adresi ve aşağıdaki komutlarda ana bilgisayar adını değiştirin
    ```bash
    sudo vi /etc/hosts
    ```
    / Etc/hosts aşağıdaki satırları ekleyin. Ortamınıza uyum sağlaması için ana bilgisayar adı ve IP adresini değiştirme    
    
   <pre><code>
   <b>10.0.0.5 hn1-db-0</b>
   <b>10.0.0.6 hn1-db-1</b>
   </code></pre>

1. **[A]**  Yükleme HANA HA paketleri  
    ```bash
    sudo zypper install SAPHanaSR
    
    ```

SAP HANA sistem çoğaltma yüklemek için Bölüm 4 SAP HANA SR performansı en iyi duruma getirilmiş senaryo Kılavuzu'nun en izleyin https://www.suse.com/products/sles-for-sap/resource-library/sap-best-practices/

1. **[A]**  Hdblcm HANA DVD'SİNDEN çalıştırın
    * Yüklemeyi seçin 1 ->
    * Yükleme -> için 1 ek bileşenleri seçin
    * Yükleme yolu [paylaşılan / hana /] girin: ENTER ->
    * Yerel ana bilgisayar adı [.] girin: ENTER ->
    * Ek ana bilgisayar sistemine eklemek istiyor musunuz? (y/n) [n]: -> ENTER
    * SAP HANA sistem Kimliğini girin: <SID of HANA e.g. HN1>
    * Örnek [00] girin:   
  HANA örneği sayısı. Azure şablonu kullanılan ya da el ile dağıtım ve ardından 03 kullanın
    * Veritabanı modunu seçin / dizin [1] girin: ENTER ->
    * Sistem kullanımı seçin / dizin [4] girin:  
  Sistem kullanımı seçin
    * Veri birimlerinin [/ data/hana/HN1] konumu girin: ENTER ->
    * Günlük birim [/ günlük/hana/HN1] konumu girin: ENTER ->
    * En fazla bellek ayırma kısıtlama? [n]: -> ENTER
    * '...' Ana bilgisayar için sertifika ana bilgisayar adını girin [...]: ENTER ->
    * SAP konak aracısı kullanıcısı (sapadm) parola girin:
    * SAP konak aracısı kullanıcısı (sapadm) parolayı onaylayın:
    * Sistem Yöneticisi (hdbadm) parola girin:
    * Sistem Yöneticisi (hdbadm) parolayı onaylayın:
    * Sistem Yöneticisi giriş dizini girin [/ usr/sap/HN1/giriş]: ENTER ->
    * Sistem Yöneticisi oturum açma kabuğu girin [/ bin/Paylaş]: ENTER ->
    * Sistem yöneticisinin kullanıcı kimliği [1001] girin: ENTER ->
    * Girin kimliği, kullanıcı grubu (sapsys) [79]: ENTER ->
    * Veritabanı kullanıcı (Sistem) parolasını girin:
    * Veritabanı kullanıcı (Sistem) parolayı onaylayın:
    * Sistem makine yeniden başlatıldıktan sonra yeniden başlatılsın mı? [n]: -> ENTER
    * Devam etmek istiyor musunuz? (e/h):   
  Özet doğrulamak ve devam etmek için Y'ye girin

1. **[A]**  Yükseltme SAP konak Aracısı  
  En son SAP konak Aracısı arşivinden karşıdan [SAP Softwarecenter] [ sap-swcenter] ve aracıyı yükseltmek için aşağıdaki komutu çalıştırın. İndirdiğiniz dosyasına işaret edecek şekilde arşivi yolu değiştirin.
    ```bash
    sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <path to SAP Host Agent SAR>
    ```

## <a name="configure-sap-hana-20-system-replication"></a>SAP HANA 2.0 sistem çoğaltmayı yapılandırmak için

Aşağıdaki öğeler ile ya da önek **[A]** - tüm düğümleri için geçerli **[1]** - 1 düğümü yalnızca uygulanabilir veya **[2]** - Pacemaker kümedeki düğüm 2 yalnızca uygulanabilir.

1. **[1]**  Kiracı veritabanı oluşturmak

   SAP HANA 2.0 veya MDC kullanıyorsanız, SAP NetWeaver sisteminiz için bir kiracı veritabanı oluşturun. NW1 SAP sisteminizi SID ile değiştirin.

   Olarak oturum `<hanasid`> adm ve aşağıdaki komutu yürütün

   <pre><code>
   hdbsql -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> -d SYSTEMDB 'CREATE DATABASE <b>NW1</b> SYSTEM USER PASSWORD "<b>passwd</b>"'
   </code></pre>

1. **[1]**  Sistem çoğaltma ilk düğümde yapılandırın
   
   Olarak oturum `<hanasid`> adm ve veritabanlarını yedekleyin

   <pre><code>
   hdbsql -d SYSTEMDB -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackupSYS</b>')"
   hdbsql -d <b>HN1</b> -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackupHN1</b>')"
   hdbsql -d <b>NW1</b> -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackupNW1</b>')"
   </code></pre>

   İkincil sistem PKI dosyaları kopyalama

   <pre><code>
   scp /usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/data/SSFS_<b>HN1</b>.DAT <b>hn1-db-1</b>:/usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/data/
   scp /usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/key/SSFS_<b>HN1</b>.KEY <b>hn1-db-1</b>:/usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/key/
   </code></pre>

   Birincil site oluşturun.

   <pre><code>
   hdbnsutil -sr_enable –-name=<b>SITE1</b>
   </code></pre>

1. **[2]**  Sistem çoğaltma İkinci düğüm üzerinde yapılandırma
    
    İkinci düğümü sistemi çoğaltması başlatmak için kaydolun. Olarak oturum `<hanasid`> adm ve aşağıdaki komutu çalıştırın

    <pre><code>
    sapcontrol -nr <b>03</b> -function StopWait 600 10
    hdbnsutil -sr_register --remoteHost=<b>hn1-db-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
    </code></pre>

## <a name="configure-sap-hana-10-system-replication"></a>SAP HANA 1.0 sistem çoğaltmayı yapılandırmak için

1. **[1]**  Gerekli kullanıcılar oluşturma

    Kök olarak oturum açın ve aşağıdaki komutu çalıştırın. Kalın dizeleri (HANA sistem kimliği HN1 ile örnek numarasını 03) SAP HANA yüklemenizi değerlerle değiştirdiğinizden emin olun.

    <pre><code>
    PATH="$PATH:/usr/sap/<b>HN1</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
    hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN TO <b>hdb</b>hasync' 
    hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
    </code></pre>

1. **[A]**  Bir anahtar girişi oluşturun
   
    Kök olarak oturum açın ve yeni bir anahtar girişi oluşturmak için aşağıdaki komutu çalıştırın.

    <pre><code>
    PATH="$PATH:/usr/sap/<b>HN1</b>/HDB<b>03</b>/exe"
    hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>passwd</b>
    </code></pre>

1. **[1]**  Yedekleme veritabanı

   Kök olarak oturum açın ve veritabanlarını yedekleyin

   <pre><code>
   PATH="$PATH:/usr/sap/<b>HN1</b>/HDB<b>03</b>/exe"
   hdbsql -d SYSTEMDB -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')"
   </code></pre>

   Ayrıca çok kiracılı yükleme kullanırsanız, Kiracı veritabanı yedekleme

   <pre><code>   
   hdbsql -d <b>HN1</b> -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')"
   </code></pre>

1. **[1]**  Sistem çoğaltma ilk düğümde yapılandırın
    
    Olarak oturum `<hanasid`> adm ve birincil site oluşturun.

    <pre><code>
    su - <b>hdb</b>adm
    hdbnsutil -sr_enable –-name=<b>SITE1</b>
    </code></pre>

1. **[2]**  Sistemi çoğaltması ikincil düğümde yapılandırın.

    Olarak oturum `<hanasid`> adm ve ikincil site kaydedin.

    <pre><code>
    sapcontrol -nr <b>03</b> -function StopWait 600 10
    hdbnsutil -sr_register --remoteHost=<b>hn1-db-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
    </code></pre>

## <a name="create-sap-hana-cluster-resources"></a>SAP HANA küme kaynakları oluşturun

   İlk olarak HANA topoloji oluşturun. Pacemaker küme düğümlerinden biri üzerinde aşağıdaki komutları çalıştırın.
   
   <pre><code>
   sudo crm configure property maintenance-mode=true

   # replace the bold string with your instance number and HANA system ID
   
   sudo crm configure primitive rsc_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> ocf:suse:SAPHanaTopology \
     operations \$id="rsc_sap2_<b>HN1</b>_HDB<b>03</b>-operations" \
     op monitor interval="10" timeout="600" \
     op start interval="0" timeout="600" \
     op stop interval="0" timeout="300" \
     params SID="<b>HN1</b>" InstanceNumber="<b>03</b>"
   
   sudo crm configure clone cln_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> rsc_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> \
     meta is-managed="true" clone-node-max="1" target-role="Started" interleave="true"
   </code></pre>
   
   Ardından, HANA kaynakları oluşturun.
   
   <pre><code>
   # replace the bold string with your instance number, HANA system ID and the frontend IP address of the Azure load balancer. 
      
   sudo crm configure primitive rsc_SAPHana_<b>HN1</b>_HDB<b>03</b> ocf:suse:SAPHana \
     operations \$id="rsc_sap_<b>HN1</b>_HDB<b>03</b>-operations" \
     op start interval="0" timeout="3600" \
     op stop interval="0" timeout="3600" \
     op promote interval="0" timeout="3600" \
     op monitor interval="60" role="Master" timeout="700" \
     op monitor interval="61" role="Slave" timeout="700" \
     params SID="<b>HN1</b>" InstanceNumber="<b>03</b>" PREFER_SITE_TAKEOVER="true" \
     DUPLICATE_PRIMARY_TIMEOUT="7200" AUTOMATED_REGISTER="false"
   
   sudo crm configure ms msl_SAPHana_<b>HN1</b>_HDB<b>03</b> rsc_SAPHana_<b>HN1</b>_HDB<b>03</b> \
     meta is-managed="true" notify="true" clone-max="2" clone-node-max="1" \
     target-role="Started" interleave="true"
   
   sudo crm configure primitive rsc_ip_<b>HN1</b>_HDB<b>03</b> ocf:heartbeat:IPaddr2 \
     meta target-role="Started" is-managed="true" \
     operations \$id="rsc_ip_<b>HN1</b>_HDB<b>03</b>-operations" \
     op monitor interval="10s" timeout="20s" \
     params ip="<b>10.0.0.13</b>"
   
   sudo crm configure primitive rsc_nc_<b>HN1</b>_HDB<b>03</b> anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k 625<b>03</b>" \
     op monitor timeout=20s interval=10 depth=0
   
   sudo crm configure group g_ip_<b>HN1</b>_HDB<b>03</b> rsc_ip_<b>HN1</b>_HDB<b>03</b> rsc_nc_<b>HN1</b>_HDB<b>03</b>
   
   sudo crm configure colocation col_saphana_ip_<b>HN1</b>_HDB<b>03</b> 2000: g_ip_<b>HN1</b>_HDB<b>03</b>:Started \
     msl_SAPHana_<b>HN1</b>_HDB<b>03</b>:Master  
   
   sudo crm configure order ord_SAPHana_<b>HN1</b>_HDB<b>03</b> 2000: cln_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> \
     msl_SAPHana_<b>HN1</b>_HDB<b>03</b>
   
   # Cleanup the HANA resources. The HANA resources might have failed because of a known issue.
   sudo crm resource cleanup rsc_SAPHana_<b>HN1</b>_HDB<b>03</b>

   sudo crm configure property maintenance-mode=false
   </code></pre>

   Küme durumunun Tamam olduğunu ve tüm kaynakları başlatıldığından emin olun. Kaynakları çalıştıran hangi düğümde önemli değildir.

   <pre><code>
   sudo crm_mon -r
   
   # Online: [ hn1-db-0 hn1-db-1 ]
   #
   # Full list of resources:
   #
   # stonith-sbd     (stonith:external/sbd): Started hn1-db-0
   # rsc_st_azure    (stonith:fence_azure_arm):      Started hn1-db-1
   # Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
   #     Started: [ hn1-db-0 hn1-db-1 ]
   # Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
   #     Masters: [ hn1-db-0 ]
   #     Slaves: [ hn1-db-1 ]
   # Resource Group: g_ip_HN1_HDB03
   #     rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
   #     rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

### <a name="test-cluster-setup"></a>Test kümesi Kurulumu
Bu bölümde, kurulumunuz test nasıl açıklanmaktadır. Her test kök olduğundan ve SAP HANA asıl sanal makine hn1-db-0 çalışır durumda olduğunu varsayar.

#### <a name="fencing-test"></a>Yalıtma Test

Düğüm hn1-db-0 ağ arabiriminde devre dışı bırakarak yalıtma Aracısı Kurulum test edebilirsiniz.

<pre><code>
sudo ifdown eth0
</code></pre>

Sanal makine şimdi yeniden veya küme yapılandırmanıza bağlı olarak durduruldu.
Kapalı stonith eylem ayarlarsanız, sanal makinenin durdurulması gittiği ve kaynakları çalışan sanal makineyi geçirilir.

Sanal makine yeniden başlattıktan sonra SAP HANA kaynak olarak ikincil başlatılamadığında = AUTOMATED_REGISTER ayarlarsanız, "false". Bu durumda, HANA örneği ikincil bu komutunu yürüterek yapılandırın:

<pre><code>
su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>

# switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-0</b>
</code></pre>

#### <a name="testing-a-manual-failover"></a>El ile bir yük devretme sınaması

Düğüm hn1-db-0 pacemaker hizmetini durdurarak el ile bir yük devretme test edebilirsiniz.
<pre><code>
service pacemaker stop
</code></pre>

Yük devretme sonrasında hizmetini yeniden başlatabilirsiniz. AUTOMATED_REGISTER ayarlarsanız = "false", ikincil olarak başlatmak için hn1-db-0 başarısız SAP HANA kaynak. Bu durumda, HANA örneği ikincil bu komutunu yürüterek yapılandırın:

<pre><code>
service pacemaker start
su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 


# Switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-0</b>
</code></pre>

#### <a name="testing-a-migration"></a>Bir geçiş test etme

Aşağıdaki komutu çalıştırarak SAP HANA ana düğüm geçirebilirsiniz
<pre><code>
crm resource migrate msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-1</b>
crm resource migrate g_ip_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-1</b>
</code></pre>

= AUTOMATED_REGISTER ayarlarsanız, "false", bu komutları dizisini SAP HANA ana düğüm ve sanal IP adresi hn1 db 1 içeren grubun geçirmeniz gerekir.
SAP HANA kaynak hn1 db 0 üzerinde ikincil olarak başlatılamaz. Bu durumda, HANA örneği ikincil bu komutunu yürüterek yapılandırın:

<pre><code>
su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 
</code></pre>

Geçiş yeniden silinmesi gereken konum kısıtlamaları oluşturur.

<pre><code>
crm configure edited

# Delete location constraints that are named like the following contraint. You should have two constraints, one for the SAP HANA resource and one for the IP address group.
location cli-prefer-g_ip_<b>HN1</b>_HDB<b>03</b> g_ip_<b>HN1</b>_HDB<b>03</b> role=Started inf: <b>hn1-db-1</b>
</code></pre>

İkincil düğüm kaynağı durumunun dökümünü temiz gerekir

<pre><code>
# Switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-0</b>
</code></pre>

## <a name="next-steps"></a>Sonraki adımlar
* [Azure sanal makineleri planlama ve uygulama SAP için][planning-guide]
* [SAP için Azure sanal makineler dağıtımı][deployment-guide]
* [SAP için Azure sanal makineleri DBMS dağıtımı][dbms-guide]
* Yüksek kullanılabilirlik ve olağanüstü durum kurtarma SAP HANA planlama azure'da (büyük örnekler) oluşturmayı öğrenmek için bkz: [SAP HANA (büyük örnekler) Azure üzerinde yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md). 
