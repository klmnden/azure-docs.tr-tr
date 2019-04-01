---
title: SUSE Linux Enterprise Server Vm'lerinde Azure üzerinde SAP HANA yüksek kullanılabilirliği | Microsoft Docs
description: SUSE Linux Enterprise Server Vm'lerinde Azure üzerinde SAP hana yüksek kullanılabilirlik
services: virtual-machines-linux
documentationcenter: ''
author: MSSedusch
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/15/2019
ms.author: sedusch
ms.openlocfilehash: 2121cd661f5f1c2c14dc32eb2a4cbf717c966c67
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58668966"
---
# <a name="high-availability-of-sap-hana-on-azure-vms-on-suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server Vm'lerinde Azure üzerinde SAP hana yüksek kullanılabilirlik

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
[401162]:https://launchpad.support.sap.com/#/notes/401162

[hana-ha-guide-replication]:sap-hana-high-availability.md#14c19f65-b5aa-4856-9594-b81c7e4df73d
[hana-ha-guide-shared-storage]:sap-hana-high-availability.md#498de331-fa04-490b-997c-b078de457c9d
[sles-for-sap-bp]:https://www.suse.com/documentation/sles-for-sap-12/

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[sap-swcenter]:https://launchpad.support.sap.com/#/softwarecenter
[template-multisid-db]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged-md%2Fazuredeploy.json

Şirket içi geliştirme için ya da HANA sistem çoğaltması kullanın veya SAP HANA için yüksek kullanılabilirlik kurmak için paylaşılan depolama kullanmak.
Azure sanal makinelerinde (VM'ler), azure'da HANA sistem çoğaltması şu anda desteklenen tek yüksek kullanılabilirlik işlevi ' dir. SAP HANA çoğaltması, bir birincil düğüm ve en az bir ikincil düğüm oluşur. Birincil düğümdeki verilerde yapılan değişiklikler ikincil düğüme eşzamanlı veya zaman uyumsuz olarak çoğaltılır.

Bu makalede, dağıtın ve sanal makineleri yapılandırma, küme Framework'ü yüklemek ve yüklemeniz ve SAP HANA sistem çoğaltması yapılandırmanız açıklar.
Örnek yapılandırma, yükleme komutlarını örnek numarası **03**ve HANA sistem kimliği **HN1** kullanılır.

Öncelikle aşağıdaki SAP notları ve incelemeleri okuyun:

* SAP notu [1928533], sahip olduğu:
  * SAP yazılım dağıtımı için desteklenen bir Azure VM boyutlarını listesi.
  * Azure VM boyutları için önemli kapasite bilgileri.
  * Desteklenen bir SAP yazılım ve işletim sistemi (OS) ve veritabanı birleşimleri.
  * Windows ve Linux'ta Microsoft Azure için gerekli SAP çekirdek sürümü.
* SAP notu [2015553] azure'da SAP tarafından desteklenen SAP yazılım dağıtımları için önkoşulları listeler.
* SAP notu [2205917] SAP uygulamaları için SUSE Linux Enterprise Server işletim sistemi ayarlarını önerilir.
* SAP notu [1944799] SAP HANA kılavuzu için SUSE Linux Enterprise Server SAP uygulamaları için vardır.
* SAP notu [2178632] ayrıntılı tüm azure'da SAP için bildirilen izleme ölçümleri hakkında bilgiler içerir.
* SAP notu [2191498] azure'da Linux için gerekli SAP konak Aracısı sürümü vardır.
* SAP notu [2243692] Linux Azure üzerinde SAP lisanslama hakkında bilgi içeriyor.
* SAP notu [1984787] SUSE Linux Enterprise Server 12 ilgili genel bilgiler bulunur.
* SAP notu [1999351] Azure Gelişmiş izleme uzantısı için SAP için ek bilgiler.
* SAP notu [401162] "adresi zaten kullanımda" HANA sistem çoğaltması ' ayarlarken kaçınılması hakkında bilgi içeriyor.
* [SAP topluluk WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) tüm gerekli SAP notları için Linux sahiptir.
* [SAP HANA sertifikalı Iaas platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)
* [Azure sanal makineleri planlama ve uygulama için Linux üzerinde SAP] [ planning-guide] Kılavuzu.
* [Azure sanal makineler dağıtım için Linux'ta SAP] [ deployment-guide] (Bu makale).
* [Linux'ta SAP için Azure sanal makineleri DBMS dağıtım] [ dbms-guide] Kılavuzu.
* [SUSE Linux Enterprise Server SAP uygulamaları 12 SP3 en iyi uygulamalar kılavuzları][sles-for-sap-bp]
  * SAP HANA SR performans için iyileştirilmiş altyapı (SLES SAP uygulamaları 12 SP1'de) ayarlama. Kılavuz tüm SAP HANA sistem çoğaltması ' için şirket içi geliştirme ayarlamak için gerekli bilgileri içerir. Bu kılavuzu temel olarak kullanın.
  * Bir SAP HANA SR en iyi duruma getirilmiş altyapısı maliyetini (SLES SAP uygulamaları 12 SP1'de) ayarlama

## <a name="overview"></a>Genel Bakış

SAP HANA yüksek kullanılabilirlik elde etmek için iki sanal makinelere yüklenir. Veriler, HANA sistem çoğaltması kullanılarak çoğaltılır.

![SAP HANA yüksek kullanılabilirlik genel bakış](./media/sap-hana-high-availability/ha-suse-hana.png)

SAP HANA sistem çoğaltması Kurulumu kullanır ayrılmış sanal ana bilgisayar adı ve sanal IP adresleri. Azure üzerinde bir yük dengeleyici sanal IP adresi kullanmak için gereklidir. Aşağıdaki liste, yük dengeleyici yapılandırmasını gösterir:

* Ön uç yapılandırması: IP adresi 10.0.0.13 hn1 db
* Arka uç yapılandırması: HANA sistem çoğaltması parçası olması gereken tüm sanal makinelerin birincil ağ arabirimlerine bağlı
* Araştırma bağlantı noktası: Bağlantı noktası 62503
* Yük Dengeleme kuralları: 30313 TCP, 30315 TCP 30317 TCP

## <a name="deploy-for-linux"></a>Linux için dağıtma

SAP HANA için kaynak aracıyı SUSE Linux Enterprise Server SAP uygulamaları için dahil edilir.
Azure Market görüntü için SUSE Linux Enterprise Server SAP uygulamaları 12 için yeni sanal makineleri dağıtmak için kullanabileceğiniz içerir.

### <a name="deploy-with-a-template"></a>Bir şablon ile dağıtım

Github üzerindeki tüm gerekli kaynakları dağıtmak için hızlı başlangıç şablonlarından birini kullanabilirsiniz. Şablonu, sanal makineler, Yük Dengeleyiciyi kullanılabilirlik kümesi ve benzeri dağıtır.
Şablonu dağıtmak için aşağıdaki adımları izleyin:

1. Açık [veritabanı şablonu] [ template-multisid-db] veya [şablon yakınsanmış] [ template-converged] Azure portalında. 
    Yük Dengeleme kuralları yalnızca bir veritabanı için veritabanı şablon oluşturur. Yakınsanmış şablonu ayrıca bir ASCS/SCS ve Ağıranlar (yalnızca Linux) örneği için Yük Dengeleme kuralları oluşturur. SAP NetWeaver tabanlı bir sistemin yüklemeyi planladığınız ve ASCS/SCS örneği aynı makinelerde yüklemek kullanmak istiyorsanız [şablon yakınsanmış][template-converged].

1. Aşağıdaki parametreleri girin:
    - **SAP sistem kimliği**: Yüklemek istediğiniz SAP sistemine SAP sistemi Kimliğini girin. Kimlik ön eki olarak dağıtılan kaynaklar için kullanılır.
    - **Yığın türü**: (Bu parametre yalnızca yakınsanmış şablonunu kullanıyorsanız geçerlidir.) SAP NetWeaver yığın türünü seçin.
    - **İşletim sistemi türü**: Linux dağıtımları birini seçin. Bu örnekte, seçin **SLES 12**.
    - **Veritabanı türü**: Seçin **HANA**.
    - **SAP sistemi boyutu**: Yeni sisteme sağlamak için gittiği SAP sayısını girin. SAP teknoloji iş ortağı veya sistem Entegratörü, emin kaç SAP sistemi gerektiriyor olmadığınız durumlarda isteyin.
    - **Sistem kullanılabilirliği**: Seçin **HA**.
    - **Yönetici kullanıcı adı ve yönetici parolası**: Yeni bir kullanıcı oluşturulur makinede oturum açmak için kullanılabilir.
    - **Yeni veya var olan bir alt ağa**: Yeni sanal ağ ve alt ağ oluşturulmalıdır veya kullanılan var olan bir alt ağ belirler. Şirket içi ağınıza bağlı bir sanal ağınız zaten varsa, seçin **varolan**.
    - **Alt ağ kimliği**: Tanımlanan bir alt ağa sahip olduğunuz mevcut bir Vnet'te VM dağıtmak istiyorsanız, VM atanmalıdır belirli bir alt ağ kimliği adı için. Kimliği genellikle gibi görünüyor **/subscriptions/\<abonelik kimliği > /resourceGroups/\<kaynak grubu adı > /providers/Microsoft.Network/virtualNetworks/\<sanal ağ adı > /subnets/ \<alt ağ adı >**.

### <a name="manual-deployment"></a>El ile dağıtım

> [!IMPORTANT]
> Seçtiğiniz işletim sistemi kullanmakta olduğunuz belirli VM türleri üzerinde SAP HANA için sertifikalıdır SAP olduğundan emin olun. Bu, aranabilir için VM türleri ve işletim sistemi sürümleri listesi, SAP HANA sertifikalı [SAP HANA sertifikalı Iaas platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure). SAP HANA tam listesini almak için listelenen VM türü ayrıntılarına tıkladığınızdan emin olun belirli bir VM türü için işletim sistemi sürümleri desteklenir.
>  

1. Bir kaynak grubu oluşturun.
1. Sanal ağ oluşturun.
1. Bir kullanılabilirlik kümesi oluşturun.
   - En çok güncelleştirme etki alanı ayarlayın.
1. Bir yük dengeleyiciye (dahili) oluşturun.
   - 2. adımda oluşturduğunuz sanal ağı seçin.
1. 1 sanal makine oluşturun.
   - Seçtiğiniz VM türü üzerinde SAP HANA için desteklenen Azure galerisinde SLES4SAP görüntüsünü kullanın.
   - 3. adımda oluşturduğunuz kullanılabilirlik kümesi seçin.
1. 2 sanal makine oluşturun.
   - Seçtiğiniz VM türü üzerinde SAP HANA için desteklenen Azure galerisinde SLES4SAP görüntüsünü kullanın.
   - 3. adımda oluşturduğunuz kullanılabilirlik kümesi seçin. 
1. Veri diski ekleyin.
1. Yük Dengeleyici yapılandırın. İlk olarak, ön uç IP havuzu oluşturun:

   1. Yük Dengeleyici açın, **ön uç IP havuzu**seçip **Ekle**.
   1. Yeni ön uç IP havuzunun adını girin (örneğin, **hana ön uç**).
   1. Ayarlama **atama** için **statik** ve IP adresini girin (örneğin, **10.0.0.13**).
   1. **Tamam**’ı seçin.
   1. Yeni ön uç IP havuzu oluşturulduktan sonra havuzu IP adresini not edin.

1. Ardından, bir arka uç havuzu oluşturun:

   1. Yük Dengeleyici açın, **arka uç havuzları**seçip **Ekle**.
   1. Yeni arka uç havuzunun adını girin (örneğin, **hana arka uç**).
   1. Seçin **bir sanal makine ekleme**.
   1. 3. adımda oluşturduğunuz kullanılabilirlik kümesi seçin.
   1. SAP HANA kümedeki sanal makineleri seçin.
   1. **Tamam**’ı seçin.

1. Ardından, bir durum araştırması oluşturun:

   1. Yük Dengeleyici açın, **sistem durumu araştırmaları**seçip **Ekle**.
   1. Yeni bir sistem durumu araştırma adını girin (örneğin, **hana hp**).
   1. Seçin **TCP** protokolü ve bağlantı noktası 625**03**. Tutun **aralığı** 5 olarak ayarlandıysa değer ve **sağlıksız durum eşiği** değerini 2 olarak ayarlayın.
   1. **Tamam**’ı seçin.

1. SAP HANA 1.0 için Yük Dengeleme kuralları oluşturun:

   1. Yük Dengeleyici açın, **Yük Dengeleme kuralları**seçip **Ekle**.
   1. Yeni Yük Dengeleyici kuralı adını girin (örneğin, hana lb 3**03**15).
   1. Ön uç IP adresi, arka uç havuzu ve durum yoklaması, daha önce oluşturduğunuz seçin (örneğin, **hana ön uç**).
   1. Tutun **Protokolü** kümesine **TCP**, bağlantı noktası 3 girin**03**15.
   1. Artırmak **boşta kalma zaman aşımı** 30 dakika.
   1. Emin olun **kayan IP'yi etkinleştirebilirsiniz**.
   1. **Tamam**’ı seçin.
   1. 3 bağlantı noktası için bu adımları tekrarlayarak**03**17.

1. SAP HANA 2.0 için sistem veritabanı için Yük Dengeleme kuralları oluşturun:

   1. Yük Dengeleyici açın, **Yük Dengeleme kuralları**seçip **Ekle**.
   1. Yeni Yük Dengeleyici kuralı adını girin (örneğin, hana lb 3**03**13).
   1. Ön uç IP adresi, arka uç havuzu ve durum yoklaması, daha önce oluşturduğunuz seçin (örneğin, **hana ön uç**).
   1. Tutun **Protokolü** kümesine **TCP**, bağlantı noktası 3 girin**03**13.
   1. Artırmak **boşta kalma zaman aşımı** 30 dakika.
   1. Emin olun **kayan IP'yi etkinleştirebilirsiniz**.
   1. **Tamam**’ı seçin.
   1. 3 bağlantı noktası için bu adımları tekrarlayarak**03**14.

1. SAP HANA 2.0 için ilk Kiracı veritabanı için Yük Dengeleme kuralları oluşturun:

   1. Yük Dengeleyici açın, **Yük Dengeleme kuralları**seçip **Ekle**.
   1. Yeni Yük Dengeleyici kuralı adını girin (örneğin, hana lb 3**03**40).
   1. Ön uç IP adresi, arka uç havuzu ve durum yoklaması, daha önce oluşturduğunuz seçin (örneğin, **hana ön uç**).
   1. Tutun **Protokolü** kümesine **TCP**, bağlantı noktası 3 girin**03**40.
   1. Artırmak **boşta kalma zaman aşımı** 30 dakika.
   1. Emin olun **kayan IP'yi etkinleştirebilirsiniz**.
   1. **Tamam**’ı seçin.
   1. 3 bağlantı noktaları için bu adımları tekrarlayarak**03**41 ve 3**03**42.

SAP HANA için gerekli bağlantı noktaları hakkında daha fazla bilgi için bu bölümde okuma [Kiracı veritabanlarına bağlantı](https://help.sap.com/viewer/78209c1d3a9b41cd8624338e42a12bf6/latest/en-US/7a9343c9f2a2436faa3cfdb5ca00c052.html) içinde [SAP HANA Kiracı veritabanlarını](https://help.sap.com/viewer/78209c1d3a9b41cd8624338e42a12bf6) kılavuzu veya [SAP notu 2388694][2388694].

> [!IMPORTANT]
> Azure vm'lerinde Azure yük dengeleyicinin arkasına yerleştirilen TCP zaman damgaları etkinleştirmeyin. TCP zaman damgaları etkinleştirme, sistem durumu araştırmaları başarısız olmasına neden olur. Parametre kümesi **net.ipv4.tcp_timestamps** için **0**. Ayrıntılar için bkz. [yük dengeleyici sistem durumu araştırmalarının](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-custom-probe-overview).
> Ayrıca SAP bkz Not [2382421](https://launchpad.support.sap.com/#/notes/2382421). 

## <a name="create-a-pacemaker-cluster"></a>Pacemaker küme oluşturma

Bağlantısındaki [SLES azure'daki SUSE Linux Enterprise Server üzerinde Pacemaker ayarlama](high-availability-guide-suse-pacemaker.md) HANA bu sunucu için temel Pacemaker küme oluşturmak için. SAP HANA ve SAP NetWeaver (A) SCS için aynı Pacemaker kümesini kullanabilirsiniz.

## <a name="install-sap-hana"></a>SAP HANA yükleme

Aşağıdaki ön ekleri bu bölümdeki adımları kullanın:
- **[A]** : Adım tüm düğümler için geçerlidir.
- **[1]**: Bu adım yalnızca düğüm 1 için geçerlidir.
- **[2]**: Bu adım yalnızca Pacemaker kümeye 2 düğüme geçerlidir.

1. **[A]**  Disk düzenini ayarla: **Mantıksal birim Yöneticisi (LVM)**.

   LVM'yi veri depolayan ve günlük dosyaları birimleri için kullanmanızı öneririz. Aşağıdaki örnek, sanal makineler iki birim oluşturmak için kullanılan bağlı dört veri diskleri olduğunu varsayar.

   Tüm kullanılabilir diskleri listelenmektedir:

   <pre><code>ls /dev/disk/azure/scsi1/lun*
   </code></pre>

   Örnek çıktı:

   <pre><code>
   /dev/disk/azure/scsi1/lun0  /dev/disk/azure/scsi1/lun1  /dev/disk/azure/scsi1/lun2  /dev/disk/azure/scsi1/lun3
   </code></pre>

   Kullanmak istediğiniz disklerin tümü için fiziksel birimler oluşturun:

   <pre><code>sudo pvcreate /dev/disk/azure/scsi1/lun0
   sudo pvcreate /dev/disk/azure/scsi1/lun1
   sudo pvcreate /dev/disk/azure/scsi1/lun2
   sudo pvcreate /dev/disk/azure/scsi1/lun3
   </code></pre>

   Veri dosyaları için bir birim grubu oluşturun. Bir birim grubu günlük dosyaları ve paylaşılan dizine SAP hana için kullanın:

   <pre><code>sudo vgcreate vg_hana_data_<b>HN1</b> /dev/disk/azure/scsi1/lun0 /dev/disk/azure/scsi1/lun1
   sudo vgcreate vg_hana_log_<b>HN1</b> /dev/disk/azure/scsi1/lun2
   sudo vgcreate vg_hana_shared_<b>HN1</b> /dev/disk/azure/scsi1/lun3
   </code></pre>

   Mantıksal birimler oluşturun. Kullandığınız doğrusal bir birim oluşturulduğunda `lvcreate` olmadan `-i` geçin. Şeritli birim daha iyi g/ç performansı için oluşturmanızı öneririz burada `-i` bağımsız değişkeni, temel alınan fiziksel birimi sayısı olmalıdır. Bu belgede, iki fiziksel birime veri hacmi için kullanılır. Bu nedenle `-i` anahtar bağımsız değişkeni ayarlanır **2**. Bir fiziksel birime kullanılan günlük birimi için böylece `-i` anahtar açıkça kullanılır. Kullanma `-i` geçin ve her bir veri birden fazla fiziksel birimi kullandığınızda, temel alınan fiziksel birimi sayısını ayarlayın. günlük veya paylaşılan birimler.

   <pre><code>sudo lvcreate <b>-i 2</b> -l 100%FREE -n hana_data vg_hana_data_<b>HN1</b>
   sudo lvcreate -l 100%FREE -n hana_log vg_hana_log_<b>HN1</b>
   sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared_<b>HN1</b>
   sudo mkfs.xfs /dev/vg_hana_data_<b>HN1</b>/hana_data
   sudo mkfs.xfs /dev/vg_hana_log_<b>HN1</b>/hana_log
   sudo mkfs.xfs /dev/vg_hana_shared_<b>HN1</b>/hana_shared
   </code></pre>

   Bağlama dizini oluşturmak ve tüm mantıksal birimler UUID'si kopyalayın:

   <pre><code>sudo mkdir -p /hana/data/<b>HN1</b>
   sudo mkdir -p /hana/log/<b>HN1</b>
   sudo mkdir -p /hana/shared/<b>HN1</b>
   # Write down the ID of /dev/vg_hana_data_<b>HN1</b>/hana_data, /dev/vg_hana_log_<b>HN1</b>/hana_log, and /dev/vg_hana_shared_<b>HN1</b>/hana_shared
   sudo blkid
   </code></pre>

   Oluşturma `fstab` üç mantıksal birimler için girişler:       

   <pre><code>sudo vi /etc/fstab
   </code></pre>

   Aşağıdaki satırda Ekle `/etc/fstab` dosyası:      

   <pre><code>/dev/disk/by-uuid/<b>&lt;UUID of /dev/mapper/vg_hana_data_<b>HN1</b>-hana_data&gt;</b> /hana/data/<b>HN1</b> xfs  defaults,nofail  0  2
   /dev/disk/by-uuid/<b>&lt;UUID of /dev/mapper/vg_hana_log_<b>HN1</b>-hana_log&gt;</b> /hana/log/<b>HN1</b> xfs  defaults,nofail  0  2
   /dev/disk/by-uuid/<b>&lt;UUID of /dev/mapper/vg_hana_shared_<b>HN1</b>-hana_shared&gt;</b> /hana/shared/<b>HN1</b> xfs  defaults,nofail  0  2
   </code></pre>

   Yeni birimleri bağlayın:

   <pre><code>sudo mount -a
   </code></pre>

1. **[A]**  Disk düzenini ayarla: **Düz diskleri**.

   Tanıtım sistemler için HANA verilerin ve günlük dosyalarının bir diskte yerleştirebilirsiniz. /Dev/disk/azure/scsi1/lun0 üzerinde bir bölüm oluşturun ve xfs ile biçimlendirin:

   <pre><code>sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/disk/azure/scsi1/lun0'
   sudo mkfs.xfs /dev/disk/azure/scsi1/lun0-part1
   
   # Write down the ID of /dev/disk/azure/scsi1/lun0-part1
   sudo /sbin/blkid
   sudo vi /etc/fstab
   </code></pre>

   Bu satırı /etc/fstab dosyaya ekleyin:

   <pre><code>/dev/disk/by-uuid/<b>&lt;UUID&gt;</b> /hana xfs  defaults,nofail  0  2
   </code></pre>

   Hedef dizin oluşturun ve diski bağlayın:

   <pre><code>sudo mkdir /hana
   sudo mount -a
   </code></pre>

1. **[A]**  Tüm konaklar için konak adı çözümlemesinin ayarlayın.

   Bir DNS sunucusu kullanabilir veya tüm düğümlerde/etc/hosts dosyasını değiştirin. Bu örnek/Etc/Hosts dosyasının nasıl kullanılacağını gösterir.
   IP adresi ve konak adı aşağıdaki komutlarda değiştirin:

   <pre><code>sudo vi /etc/hosts
   </code></pre>

   / Etc/Hosts dosyasında aşağıdaki satırları ekleyin. IP adresi ve ana bilgisayar adını, ortamınızla eşleşecek şekilde değiştirin:

   <pre><code><b>10.0.0.5 hn1-db-0</b>
   <b>10.0.0.6 hn1-db-1</b>
   </code></pre>

1. **[A]**  SAP HANA yüksek kullanılabilirlik paketleri yükleyin:

   <pre><code>sudo zypper install SAPHanaSR
   </code></pre>

SAP HANA sistem çoğaltması yüklemek için Bölüm 4 izleyin [SAP HANA SR performans için iyileştirilmiş senaryo Kılavuzu](https://www.suse.com/products/sles-for-sap/resource-library/sap-best-practices/).

1. **[A]**  Çalıştırma **hdblcm** HANA DVD'den program. Komut isteminde aşağıdaki değerleri girin:
   * Yükleme seçin: Girin **1**.
   * Ek bileşenler yüklemesi için seçin: Girin **1**.
   * Yükleme yolu [hana paylaşılan /] girin: Select girin.
   * [.] Yerel ana bilgisayar adı girin: Select girin.
   * Sisteme ek konakları eklemek istiyor musunuz? (e/h) [n]: Select girin.
   * SAP HANA sistem kimliği girin: Örneğin, SID HANA girin: **HN1**.
   * [00] Örnek numarasını girin: HANA örneği sayısını girin. Girin **03** Azure şablonu kullanılan veya bu makalede el ile dağıtım bölümünü izleyen.
   * Veritabanı modunu seçin / [1]. dizin girin: Select girin.
   * Sistem kullanımını seçin / [4]. dizin girin: Sistem kullanım değerini seçin.
   * Veri birimleri [/ data/hana/HN1] konumu girin: Select girin.
   * Günlük birimleri [/ hana/log/HN1] konumu girin: Select girin.
   * Maksimum bellek ayırma kısıtlayın? [n]: Select girin.
   * '...' Konak için sertifika ana bilgisayar adı girin [...]: Select girin.
   * SAP konak aracısı kullanıcısı (sapadm) parola girin: Konak Aracısı kullanıcının parolasını girin.
   * SAP konak aracısı kullanıcısı (sapadm) parolayı onaylayın: Onaylamak için yeniden konak Aracısı kullanıcının parolasını girin.
   * Sistem Yöneticisi (hdbadm) parola girin: Sistem Yöneticisi parolasını girin.
   * Sistem Yöneticisi (hdbadm) parolayı onaylayın: Onaylamak için yeniden sistem yöneticisi parolasını girin.
   * Sistem Yöneticisi giriş dizini girin [/ usr/sap/HN1/giriş]: Select girin.
   * Sistem Yöneticisi oturum açma Kabuğu'nu girin [/ bin/sh]: Select girin.
   * Sistem yöneticisinin kullanıcı kimliği [1001] girin: Select girin.
   * Girin kimliği kullanıcı grubuna (sapsys) [79]: Select girin.
   * Veritabanı (Sistem) kullanıcının parolasını girin: Veritabanı kullanıcı parolasını girin.
   * Veritabanı (Sistem) kullanıcı parolayı onaylayın: Onaylamak için yeniden veritabanı kullanıcı parolasını girin.
   * Sistem yeniden başlatıldıktan sonra makinenin yeniden başlatılmasını? [n]: Select girin.
   * Devam etmek istiyor musunuz? (e/h): Özet doğrulayın. Girin **y** devam etmek için.

1. **[A]**  SAP konak aracısını yükseltin.

   En son SAP konak Aracısı arşivden indirme [SAP Software Center] [ sap-swcenter] ve aracıyı yükseltmek için aşağıdaki komutu çalıştırın. İndirdiğiniz dosyaya işaret edecek şekilde arşiv yolunu değiştirin:

   <pre><code>sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive &lt;path to SAP Host Agent SAR&gt;
   </code></pre>

## <a name="configure-sap-hana-20-system-replication"></a>SAP HANA 2.0 sistem çoğaltmayı yapılandırma

Aşağıdaki ön ekleri bu bölümdeki adımları kullanın:

* **[A]** : Adım tüm düğümler için geçerlidir.
* **[1]**: Bu adım yalnızca düğüm 1 için geçerlidir.
* **[2]**: Bu adım yalnızca Pacemaker kümeye 2 düğüme geçerlidir.

1. **[1]**  Kiracı veritabanı oluşturmak.

   SAP HANA 2.0 veya MDC kullanıyorsanız, SAP NetWeaver sisteminiz için bir kiracı veritabanı oluşturun. Değiştirin **NW1** SAP sisteminizin SID ile.

   Olarak aşağıdaki komutu yürütün < hanasid\>adm:

   <pre><code>hdbsql -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> -d SYSTEMDB 'CREATE DATABASE <b>NW1</b> SYSTEM USER PASSWORD "<b>passwd</b>"'
   </code></pre>

1. **[1]**  İlk düğümü üzerinde sistem çoğaltma yapılandırın:

   Veritabanları Yedekleme < hanasid\>adm:

   <pre><code>hdbsql -d SYSTEMDB -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackupSYS</b>')"
   hdbsql -d <b>HN1</b> -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackupHN1</b>')"
   hdbsql -d <b>NW1</b> -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackupNW1</b>')"
   </code></pre>

   İkincil siteye sistem PKI dosyalarını kopyalayın:

   <pre><code>scp /usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/data/SSFS_<b>HN1</b>.DAT   <b>hn1-db-1</b>:/usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/data/
   scp /usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/key/SSFS_<b>HN1</b>.KEY  <b>hn1-db-1</b>:/usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/key/
   </code></pre>

   Birincil siteyi oluşturun:

   <pre><code>hdbnsutil -sr_enable --name=<b>SITE1</b>
   </code></pre>

1. **[2]**  İkinci düğümü sistemi çoğaltmayı yapılandırın:
    
   Sistem çoğaltması başlatmak için ikinci düğümü kaydettirin. Aşağıdaki komut olarak çalıştırılmalıdır < hanasid\>adm:

   <pre><code>sapcontrol -nr <b>03</b> -function StopWait 600 10
   hdbnsutil -sr_register --remoteHost=<b>hn1-db-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
   </code></pre>

## <a name="configure-sap-hana-10-system-replication"></a>SAP HANA 1.0 sistem çoğaltmayı yapılandırma

Aşağıdaki ön ekleri bu bölümdeki adımları kullanın:

* **[A]** : Adım tüm düğümler için geçerlidir.
* **[1]**: Bu adım yalnızca düğüm 1 için geçerlidir.
* **[2]**: Bu adım yalnızca Pacemaker kümeye 2 düğüme geçerlidir.

1. **[1]**  Gerekli kullanıcıları oluşturun.

   Kök olarak aşağıdaki komutu çalıştırın. Kalın dizeleri değiştirdiğinizden emin olun (HANA sistem kimliği **HN1** ve örnek numarası **03**) SAP HANA yüklemenizin değerleriyle:

   <pre><code>PATH="$PATH:/usr/sap/<b>HN1</b>/HDB<b>03</b>/exe"
   hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"'
   hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN TO <b>hdb</b>hasync'
   hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME'
   </code></pre>

1. **[A]**  Anahtar deposu girdisi oluşturma.

   Kök olarak yeni bir anahtar deposu girdisi oluşturmak için aşağıdaki komutu çalıştırın:

   <pre><code>PATH="$PATH:/usr/sap/<b>HN1</b>/HDB<b>03</b>/exe"
   hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>passwd</b>
   </code></pre>

1. **[1]**  Veritabanını yedekleyin.

   Kök olarak veritabanlarını yedekleyin:

   <pre><code>PATH="$PATH:/usr/sap/<b>HN1</b>/HDB<b>03</b>/exe"
   hdbsql -d SYSTEMDB -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')"
   </code></pre>

   Ayrıca, çok kiracılı bir yüklemesini kullanıyorsanız Kiracı veritabanını yedekleyin:

   <pre><code>hdbsql -d <b>HN1</b> -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')"
   </code></pre>

1. **[1]**  İlk düğümü üzerinde sistem çoğaltması yapılandırın.

   Birincil site olarak oluşturma < hanasid\>adm:

   <pre><code>su - <b>hdb</b>adm
   hdbnsutil -sr_enable –-name=<b>SITE1</b>
   </code></pre>

1. **[2]**  İkincil düğüme sistem çoğaltmayı yapılandırın.

   İkincil site olarak kaydolun < hanasid\>adm:

   <pre><code>sapcontrol -nr <b>03</b> -function StopWait 600 10
   hdbnsutil -sr_register --remoteHost=<b>hn1-db-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
   </code></pre>

## <a name="create-sap-hana-cluster-resources"></a>SAP HANA küme kaynaklarını oluşturma

İlk olarak, HANA topolojisi oluşturun. Pacemaker küme düğümlerinden biri üzerinde aşağıdaki komutları çalıştırın:

<pre><code>sudo crm configure property maintenance-mode=true

# Replace the bold string with your instance number and HANA system ID

sudo crm configure primitive rsc_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> ocf:suse:SAPHanaTopology \
  operations \$id="rsc_sap2_<b>HN1</b>_HDB<b>03</b>-operations" \
  op monitor interval="10" timeout="600" \
  op start interval="0" timeout="600" \
  op stop interval="0" timeout="300" \
  params SID="<b>HN1</b>" InstanceNumber="<b>03</b>"

sudo crm configure clone cln_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> rsc_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> \
  meta is-managed="true" clone-node-max="1" target-role="Started" interleave="true"
</code></pre>

Ardından, HANA kaynakları oluşturun:

<pre><code># Replace the bold string with your instance number, HANA system ID, and the front-end IP address of the Azure load balancer. 

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

sudo crm configure colocation col_saphana_ip_<b>HN1</b>_HDB<b>03</b> 4000: g_ip_<b>HN1</b>_HDB<b>03</b>:Started \
  msl_SAPHana_<b>HN1</b>_HDB<b>03</b>:Master  

sudo crm configure order ord_SAPHana_<b>HN1</b>_HDB<b>03</b> Optional: cln_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> \
  msl_SAPHana_<b>HN1</b>_HDB<b>03</b>

# Clean up the HANA resources. The HANA resources might have failed because of a known issue.
sudo crm resource cleanup rsc_SAPHana_<b>HN1</b>_HDB<b>03</b>

sudo crm configure property maintenance-mode=false
sudo crm configure rsc_defaults resource-stickiness=1000
sudo crm configure rsc_defaults migration-threshold=5000
</code></pre>

Küme durumunun Tamam olduğunu ve tüm kaynakları başlatıldığından emin olun. Hangi düğümünde kaynaklarını çalıştıran önemli değildir.

<pre><code>sudo crm_mon -r

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

## <a name="test-the-cluster-setup"></a>Test kümesi Kurulumu

Bu bölümde, kurulumunuzu nasıl sınayıp doğrulayabileceğiniz açıklanır. Her test kök olan ve SAP HANA ana çalışır durumda olduğunu varsayar **hn1-db-0** sanal makine.

### <a name="test-the-migration"></a>Test geçişi

Test başlamadan önce Pacemaker (aracılığıyla crm_mon - r) başarısız olan bir eylem yok, beklenmeyen konumu kısıtlamalar (örneğin bir geçiş testi parçalarla) vardır ve HANA eşitleme durumunda, örneğin SAPHanaSR showAttr sahip olduğundan emin olun:

<pre><code>hn1-db-0:~ # SAPHanaSR-showAttr

Global cib-time
--------------------------------
global Mon Aug 13 11:26:04 2018

Hosts    clone_state lpa_hn1_lpt node_state op_mode   remoteHost    roles                            score site  srmode sync_state version                vhost
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
hn1-db-0 PROMOTED    1534159564  online     logreplay nws-hana-vm-1 4:P:master1:master:worker:master 150   SITE1 sync   PRIM       2.00.030.00.1522209842 nws-hana-vm-0
hn1-db-1 DEMOTED     30          online     logreplay nws-hana-vm-0 4:S:master1:master:worker:master 100   SITE2 sync   SOK        2.00.030.00.1522209842 nws-hana-vm-1
</code></pre>

Aşağıdaki komutu yürüterek SAP HANA ana düğüm geçirebilirsiniz:

<pre><code>crm resource migrate msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-1</b>
</code></pre>

Ayarlarsanız `AUTOMATED_REGISTER="false"`, SAP HANA ana düğümü ve hn1-db-1 sanal IP adresi içeren bir grubu bu komutları dizisini geçirmeniz gerekir.

Geçiş yaptıktan sonra crm_mon - r çıktı şuna benzer

<pre><code>Online: [ hn1-db-0 hn1-db-1 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started hn1-db-1
 Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
     Started: [ hn1-db-0 hn1-db-1 ]
 Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
     Masters: [ hn1-db-1 ]
     Stopped: [ hn1-db-0 ]
 Resource Group: g_ip_HN1_HDB03
     rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
     rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-1

Failed Actions:
* rsc_SAPHana_HN1_HDB03_start_0 on hn1-db-0 'not running' (7): call=84, status=complete, exitreason='none',
    last-rc-change='Mon Aug 13 11:31:37 2018', queued=0ms, exec=2095ms
</code></pre>

SAP HANA kaynak hn1-db-0, ikincil olarak başlatılamıyor. Bu durumda, HANA örneği ikincil bu komutunu yürüterek yapılandırın:

<pre><code>su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> sapcontrol -nr <b>03</b> -function StopWait 600 10
hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>
</code></pre>

Geçiş yeniden silinmesi gereken konum kısıtlamaları oluşturur:

<pre><code># Switch back to root and clean up the failed state
exit
hn1-db-0:~ # crm resource unmigrate msl_SAPHana_<b>HN1</b>_HDB<b>03</b>
</code></pre>

İkincil düğüm kaynak durumunun dökümünü temizlemek gerekir:

<pre><code>hn1-db-0:~ # crm resource cleanup msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-0</b>
</code></pre>

Crm_mon - r kullanarak HANA kaynak durumunu izleyin. HANA hn1-db-0 başlatıldıktan sonra çıktı aşağıdaki gibi görünmelidir

<pre><code>Online: [ hn1-db-0 hn1-db-1 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started hn1-db-1
 Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
     Started: [ hn1-db-0 hn1-db-1 ]
 Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
     Masters: [ hn1-db-1 ]
     Slaves: [ hn1-db-0 ]
 Resource Group: g_ip_HN1_HDB03
     rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
     rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-1
</code></pre>

### <a name="test-the-azure-fencing-agent-not-sbd"></a>Test Azure çitlemek Aracısı (SBD değil)

Ağ arabirimi hn1-db-0 düğüm üzerinde devre dışı bırakarak Azure çitlemek aracı Kurulumu test edebilirsiniz:

<pre><code>sudo ifdown eth0
</code></pre>

Sanal makine şimdi yeniden başlatın veya küme yapılandırmanıza bağlı olarak Durdur gerekir.
Ayarlarsanız `stonith-action` kapalı olarak ayarlama, sanal makinenin durdurulması ve kaynakları çalışan sanal makineye geçirilir.

Sanal makine yeniden başlattıktan sonra ikincil ayarlarsanız olarak başlatmak SAP HANA kaynak başarısız `AUTOMATED_REGISTER="false"`. Bu durumda, HANA örneği ikincil bu komutunu yürüterek yapılandırın:

<pre><code>su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>

# Switch back to root and clean up the failed state
exit
crm resource cleanup msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-0</b>
</code></pre>

### <a name="test-sbd-fencing"></a>Test SBD çitlemek

İnquisitor işlemi sonlandırılıyor tarafından SBD Kurulumu test edebilirsiniz.

<pre><code>hn1-db-0:~ # ps aux | grep sbd
root       1912  0.0  0.0  85420 11740 ?        SL   12:25   0:00 sbd: inquisitor
root       1929  0.0  0.0  85456 11776 ?        SL   12:25   0:00 sbd: watcher: /dev/disk/by-id/scsi-360014056f268462316e4681b704a9f73 - slot: 0 - uuid: 7b862dba-e7f7-4800-92ed-f76a4e3978c8
root       1930  0.0  0.0  85456 11776 ?        SL   12:25   0:00 sbd: watcher: /dev/disk/by-id/scsi-360014059bc9ea4e4bac4b18808299aaf - slot: 0 - uuid: 5813ee04-b75c-482e-805e-3b1e22ba16cd
root       1931  0.0  0.0  85456 11776 ?        SL   12:25   0:00 sbd: watcher: /dev/disk/by-id/scsi-36001405b8dddd44eb3647908def6621c - slot: 0 - uuid: 986ed8f8-947d-4396-8aec-b933b75e904c
root       1932  0.0  0.0  90524 16656 ?        SL   12:25   0:00 sbd: watcher: Pacemaker
root       1933  0.0  0.0 102708 28260 ?        SL   12:25   0:00 sbd: watcher: Cluster
root      13877  0.0  0.0   9292  1572 pts/0    S+   12:27   0:00 grep sbd

hn1-db-0:~ # kill -9 1912
</code></pre>

Küme düğümü hn1-db-0 yeniden başlatılması. Pacemaker hizmeti daha sonra başlatılmamış. Yeniden başlatmak istediğinizden emin olun.

### <a name="test-a-manual-failover"></a>Elle yük devretme testi

Elle yük devretme durdurarak sınayabilirsiniz `pacemaker` hizmet hn1-db-0 düğümde:

<pre><code>service pacemaker stop
</code></pre>

Yük devretmeden sonra hizmeti yeniden başlatabilirsiniz. Ayarlarsanız `AUTOMATED_REGISTER="false"`, ikincil olarak başlatmak SAP HANA kaynak hn1-db-0 düğüm üzerinde başarısız olur. Bu durumda, HANA örneği ikincil bu komutunu yürüterek yapılandırın:

<pre><code>service pacemaker start
su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 

# Switch back to root and clean up the failed state
exit
crm resource cleanup msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-0</b>
</code></pre>

### <a name="suse-tests"></a>SUSE testleri

> [!IMPORTANT]
> Seçtiğiniz işletim sistemi kullanmakta olduğunuz belirli VM türleri üzerinde SAP HANA için sertifikalıdır SAP olduğundan emin olun. Bu, aranabilir için VM türleri ve işletim sistemi sürümleri listesi, SAP HANA sertifikalı [SAP HANA sertifikalı Iaas platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure). SAP HANA tam listesini almak için listelenen VM türü ayrıntılarına tıkladığınızdan emin olun belirli bir VM türü için işletim sistemi sürümleri desteklenir.

Kullanım Örneğinize bağlı olarak SAP HANA SR performans için iyileştirilmiş senaryonuz ya da SAP HANA SR maliyetini en iyi duruma getirilmiş senaryo Kılavuzu'nda listelenen tüm test çalışmalarını Çalıştır. Kılavuzlar bulabilirsiniz [SAP için SLES en iyi yöntemler sayfa][sles-for-sap-bp].

SAP HANA SR performans için iyileştirilmiş senaryo SUSE Linux Enterprise Server SAP uygulamaları 12 SP1 kılavuzu için test açıklamalarını bir kopyasını testlerdir. Güncel bir sürüm için her zaman aynı zamanda Kılavuzu okuyun. Her zaman test başlamadan önce HANA eşitlenmiş olduğundan emin olun ve ayrıca Pacemaker yapılandırmasının doğru olduğundan emin olun.

Aşağıdaki test açıklamalarda PREFER_SITE_TAKEOVER varsayıyoruz = "true" ve AUTOMATED_REGISTER = "false".
NOT: Aşağıdaki testler sırayla çalıştırılması ve önceki testleri çıkış durumuna bağlı şekilde tasarlanmıştır.

1. TEST 1: DURDURMA BİRİNCİL VERİTABANI DÜĞÜMÜNE 1

   Kaynak durumu, test başlamadan önce:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

   Olarak aşağıdaki komutları çalıştırın < hanasid\>adm düğüm hn1-db-0:

   <pre><code>hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> HDB stop
   </code></pre>

   Pacemaker durduruldu HANA örneği ve başka bir düğüme yük devretme algılanmalıdır. Yük devretme tamamlandığında, düğüm hn1-db-0 HANA örneğinde Pacemaker düğüme otomatik olarak HANA ikincil olarak kaydetmez nedeniyle durduruldu.

   Düğüm hn1-db-0 olarak ikincil ve temizleme başarısız kaynak kaydetmek için aşağıdaki komutları çalıştırın.

   <pre><code>hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=hn1-db-1 --remoteInstance=03 --replicationMode=sync --name=SITE1
   
   # run as root
   hn1-db-0:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-0
   </code></pre>

   Kaynak durumu test sonra:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-1 ]
      Slaves: [ hn1-db-0 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-1
   </code></pre>

1. TEST 2: DURDURMA BİRİNCİL DÜĞÜM 2 ÜZERİNDE VERİTABANI

   Kaynak durumu, test başlamadan önce:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-1 ]
      Slaves: [ hn1-db-0 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-1
   </code></pre>

   Olarak aşağıdaki komutları çalıştırın < hanasid\>adm düğümü hn1-db-1:

   <pre><code>hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> HDB stop
   </code></pre>

   Pacemaker durduruldu HANA örneği ve başka bir düğüme yük devretme algılanmalıdır. Yük devretme tamamlandığında, düğüm hn1-db-1 HANA örneğinde Pacemaker düğüme otomatik olarak HANA ikincil olarak kaydetmez nedeniyle durduruldu.

   Düğüm hn1-db-1 olarak ikincil ve temizleme başarısız kaynak kaydetmek için aşağıdaki komutları çalıştırın.

   <pre><code>hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=hn1-db-0 --remoteInstance=03 --replicationMode=sync --name=SITE2
   
   # run as root
   hn1-db-1:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-1
   </code></pre>

   Kaynak durumu test sonra:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

1. TEST 3: ÇÖKME BİRİNCİL VERİTABANI DÜĞÜMDE

   Kaynak durumu, test başlamadan önce:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

   Olarak aşağıdaki komutları çalıştırın < hanasid\>adm düğüm hn1-db-0:

   <pre><code>hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> HDB kill-9
   </code></pre>
   
   Pacemaker kli HANA örneği ve başka bir düğüme yük devretme algılanmalıdır. Yük devretme tamamlandığında, düğüm hn1-db-0 HANA örneğinde Pacemaker düğüme otomatik olarak HANA ikincil olarak kaydetmez nedeniyle durduruldu.

   Düğüm hn1-db-0 olarak ikincil ve temizleme başarısız kaynak kaydetmek için aşağıdaki komutları çalıştırın.

   <pre><code>hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=hn1-db-1 --remoteInstance=03 --replicationMode=sync --name=SITE1
   
   # run as root
   hn1-db-0:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-0
   </code></pre>

   Kaynak durumu test sonra:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-1 ]
      Slaves: [ hn1-db-0 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-1
   </code></pre>

1. TEST 4: ÇÖKME BİRİNCİL DÜĞÜM 2 ÜZERİNDE VERİTABANI

   Kaynak durumu, test başlamadan önce:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-1 ]
      Slaves: [ hn1-db-0 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-1
   </code></pre>

   Olarak aşağıdaki komutları çalıştırın < hanasid\>adm düğümü hn1-db-1:

   <pre><code>hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> HDB kill-9
   </code></pre>

   Pacemaker kli HANA örneği ve başka bir düğüme yük devretme algılanmalıdır. Yük devretme tamamlandığında, düğüm hn1-db-1 HANA örneğinde Pacemaker düğüme otomatik olarak HANA ikincil olarak kaydetmez nedeniyle durduruldu.

   Düğüm hn1-db-1 olarak ikincil ve temizleme başarısız kaynak kaydetmek için aşağıdaki komutları çalıştırın.

   <pre><code>hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=hn1-db-0 --remoteInstance=03 --replicationMode=sync --name=SITE2
   
   # run as root
   hn1-db-1:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-1
   </code></pre>

   Kaynak durumu test sonra:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

1. TEST 5: ÇÖKME BİRİNCİL SITE DÜĞÜM (DÜĞÜM 1)

   Kaynak durumu, test başlamadan önce:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

   Aşağıdaki komutları kök olarak düğüm hn1-db-0 çalıştırın:

   <pre><code>hn1-db-0:~ #  echo 'b' > /proc/sysrq-trigger
   </code></pre>

   Pacemaker kli küme düğümü algılamak ve düğüm Çit gerekir. Düğüm çevrelenmiş olduktan sonra Pacemaker HANA örneğinin bir devralma işlemini tetikler. Çevrelenmiş düğüm yeniden başlatıldığında, Pacemaker otomatik olarak başlatılmaz.

   Pacemaker başlatmak için aşağıdaki komutları çalıştırın, temiz düğüm hn1-db-0 SBD iletileri kaydetme düğüm hn1-db-0 ikincil ve temizleme başarısız kaynağı.

   <pre><code># run as root
   # list the SBD device(s)
   hn1-db-0:~ # cat /etc/sysconfig/sbd | grep SBD_DEVICE=
   # SBD_DEVICE="/dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116;/dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1;/dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3"
   
   hn1-db-0:~ # sbd -d /dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116 -d /dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1 -d /dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3 message hn1-db-0 clear
   
   hn1-db-0:~ # systemctl start pacemaker
   
   # run as &lt;hanasid&gt;adm
   hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=hn1-db-1 --remoteInstance=03 --replicationMode=sync --name=SITE1
   
   # run as root
   hn1-db-0:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-0
   </code></pre>

   Kaynak durumu test sonra:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-1 ]
      Slaves: [ hn1-db-0 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-1
   </code></pre>

1. TEST 6: ÇÖKME İKİNCİL SITE DÜĞÜMÜ (2 DÜĞÜM)

   Kaynak durumu, test başlamadan önce:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-1 ]
      Slaves: [ hn1-db-0 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-1
   </code></pre>

   Aşağıdaki komutları kök olarak düğüm hn1-db-1 üzerinde çalıştırın:

   <pre><code>hn1-db-1:~ #  echo 'b' > /proc/sysrq-trigger
   </code></pre>

   Pacemaker kli küme düğümü algılamak ve düğüm Çit gerekir. Düğüm çevrelenmiş olduktan sonra Pacemaker HANA örneğinin bir devralma işlemini tetikler. Çevrelenmiş düğüm yeniden başlatıldığında, Pacemaker otomatik olarak başlatılmaz.

   Pacemaker başlatmak için aşağıdaki komutları çalıştırın, temiz düğüm hn1-db-1 SBD iletileri kaydetme düğüm hn1-db-1 ikincil ve temizleme başarısız kaynağı.

   <pre><code># run as root
   # list the SBD device(s)
   hn1-db-1:~ # cat /etc/sysconfig/sbd | grep SBD_DEVICE=
   # SBD_DEVICE="/dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116;/dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1;/dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3"
   
   hn1-db-1:~ # sbd -d /dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116 -d /dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1 -d /dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3 message hn1-db-1 clear
   
   hn1-db-1:~ # systemctl start pacemaker
   
   # run as &lt;hanasid&gt;adm
   hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=hn1-db-0 --remoteInstance=03 --replicationMode=sync --name=SITE2
   
   # run as root
   hn1-db-1:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-1
   </code></pre>

   Kaynak durumu test sonra:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

1. TEST 7: İKİNCİL DÜĞÜM 2 ÜZERİNDE VERİTABANI DURDUR

   Kaynak durumu, test başlamadan önce:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

   Olarak aşağıdaki komutları çalıştırın < hanasid\>adm düğümü hn1-db-1:

   <pre><code>hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> HDB stop
   </code></pre>

   Pacemaker durdurulmuş HANA örneğinde algılar ve kaynak düğüm hn1-db-1'başarısız olarak işaretleyin. Pacemaker HANA örneği otomatik olarak yeniden başlatmanız gerekir. Başarısız durumunu temizlemek için aşağıdaki komutu çalıştırın.

   <pre><code># run as root
   hn1-db-1:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-1
   </code></pre>

   Kaynak durumu test sonra:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

1. TEST 8: İKİNCİL DÜĞÜM 2 ÜZERİNDE VERİTABANI KİLİTLENME

   Kaynak durumu, test başlamadan önce:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

   Olarak aşağıdaki komutları çalıştırın < hanasid\>adm düğümü hn1-db-1:

   <pre><code>hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> HDB kill-9
   </code></pre>

   Pacemaker kli HANA örneği algılar ve kaynak düğüm hn1-db-1'başarısız olarak işaretleyin. Başarısız durumunu temizlemek için aşağıdaki komutu çalıştırın. Pacemaker sonra otomatik olarak HANA örneğini yeniden başlatmanız.

   <pre><code># run as root
   hn1-db-1:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-1
   </code></pre>

   Kaynak durumu test sonra:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

1. 9 TEST EDİN: İKİNCİL SITE DÜĞÜMÜ (2 DÜĞÜM) ÇALIŞAN İKİNCİL HANA VERİTABANI KİLİTLENME

   Kaynak durumu, test başlamadan önce:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

   Aşağıdaki komutları kök olarak düğüm hn1-db-1 üzerinde çalıştırın:

   <pre><code>hn1-db-1:~ # echo b > /proc/sysrq-trigger
   </code></pre>

   Pacemaker kli küme düğümü algılamak ve düğüm Çit gerekir. Çevrelenmiş düğüm yeniden başlatıldığında, Pacemaker otomatik olarak başlatılmaz.

   Pacemaker başlatın, düğüm hn1-db-1 ve temizleme başarısız kaynak SBD iletileri temizlemek için aşağıdaki komutları çalıştırın.

   <pre><code># run as root
   # list the SBD device(s)
   hn1-db-1:~ # cat /etc/sysconfig/sbd | grep SBD_DEVICE=
   # SBD_DEVICE="/dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116;/dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1;/dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3"
   
   hn1-db-1:~ # sbd -d /dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116 -d /dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1 -d /dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3 message hn1-db-1 clear
   
   hn1-db-1:~ # systemctl start pacemaker  
   
   hn1-db-1:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-1
   </code></pre>

   Kaynak durumu test sonra:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

## <a name="next-steps"></a>Sonraki adımlar

* [Azure sanal makineleri planlama ve uygulama için SAP][planning-guide]
* [SAP için Azure sanal makineler dağıtımı][deployment-guide]
* [SAP için Azure sanal makineleri DBMS dağıtım][dbms-guide]
* Yüksek kullanılabilirlik ve olağanüstü durum kurtarma SAP hana (büyük örnekler) azure'da planlama oluşturma hakkında bilgi almak için bkz: [SAP HANA (büyük örnekler) azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md)
