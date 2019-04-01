---
title: Azure sanal makinelerinde (VM'ler) SAP HANA sistem çoğaltması ayarlama | Microsoft Docs
description: Azure sanal makinelerinde (VM'ler) SAP hana yüksek kullanılabilirlik kurun.
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
ms.openlocfilehash: 1be3c411a208a2a9da1a4f6a319fdf37cc8aa2dd
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58669053"
---
# <a name="high-availability-of-sap-hana-on-azure-vms-on-red-hat-enterprise-linux"></a>Red Hat Enterprise Linux Vm'lerinde Azure üzerinde SAP hana yüksek kullanılabilirlik

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
[2292690]:https://launchpad.support.sap.com/#/notes/2292690
[2455582]:https://launchpad.support.sap.com/#/notes/2455582
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2009879]:https://launchpad.support.sap.com/#/notes/2009879

[sap-swcenter]:https://launchpad.support.sap.com/#/softwarecenter
[template-multisid-db]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json

Şirket içi geliştirme için ya da HANA sistem çoğaltması kullanın veya SAP HANA için yüksek kullanılabilirlik kurmak için paylaşılan depolama kullanmak.
Azure sanal makinelerinde (VM'ler), azure'da HANA sistem çoğaltması şu anda desteklenen tek yüksek kullanılabilirlik işlevi ' dir.
SAP HANA çoğaltması, bir birincil düğüm ve en az bir ikincil düğüm oluşur. Birincil düğümdeki verilerde yapılan değişiklikler ikincil düğüme eşzamanlı veya zaman uyumsuz olarak çoğaltılır.

Bu makalede, dağıtın ve sanal makineleri yapılandırma, küme Framework'ü yüklemek ve yüklemeniz ve SAP HANA sistem çoğaltması yapılandırmanız açıklar.
Örnek yapılandırma, yükleme komutlarını örnek numarası **03**ve HANA sistem kimliği **HN1** kullanılır.

Öncelikle aşağıdaki SAP notları ve incelemeleri okuyun:

* SAP notu [1928533], sahip olduğu:
  * SAP yazılım dağıtımı için desteklenen bir Azure VM boyutlarını listesi.
  * Azure VM boyutları için önemli kapasite bilgileri.
  * Desteklenen bir SAP yazılım ve işletim sistemi (OS) ve veritabanı birleşimleri.
  * Windows ve Linux'ta Microsoft Azure için gerekli SAP çekirdek sürümü.
* SAP notu [2015553] azure'da SAP tarafından desteklenen SAP yazılım dağıtımları için önkoşulları listeler.
* SAP notu [2002167] Red Hat Enterprise Linux işletim sistemi ayarlarını önerilir
* SAP notu [2009879] Red Hat Enterprise Linux için SAP HANA yönergeleri içeriyor
* SAP notu [2178632] ayrıntılı azure'da SAP için bildirilen tüm izlenen ölçümler hakkında bilgi içerir.
* SAP notu [2191498] azure'da Linux için gerekli SAP konak Aracısı sürümü vardır.
* SAP notu [2243692] Linux Azure üzerinde SAP lisanslama hakkında bilgi içeriyor.
* SAP notu [1999351] Azure Gelişmiş izleme uzantısı için SAP için ek bilgiler.
* [SAP topluluk WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) tüm SAP notları Linux için zorunludur.
* [Azure sanal makineleri planlama ve uygulama için Linux üzerinde SAP][planning-guide]
* [(Bu makale) Linux'ta SAP için Azure sanal makineler dağıtımı][deployment-guide]
* [Linux'ta SAP için Azure sanal makineleri DBMS dağıtım][dbms-guide]
* [SAP HANA sistem çoğaltması pacemaker kümedeki](https://access.redhat.com/articles/3004101)
* Genel RHEL belgeleri
  * [Yüksek kullanılabilirlik eklentilere genel bakış](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_overview/index)
  * [Yüksek kullanılabilirlik eklenti Yönetim](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_administration/index)
  * [Yüksek kullanılabilirlik eklenti başvurusu](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/index)
* Azure özel RHEL belgeleri:
  * [RHEL yüksek kullanılabilirlik kümelerini - Microsoft Azure sanal makineleri küme üyeleri olarak ilkeleri desteği](https://access.redhat.com/articles/3131341)
  * [Yükleme ve Microsoft Azure'da Red Hat Enterprise Linux 7.4 (ve üzeri) yüksek kullanılabilirlik kümesi yapılandırma](https://access.redhat.com/articles/3252491)
  * [Kullanılacak Microsoft azure'da Red Hat Enterprise Linux üzerinde SAP HANA yükleyin](https://access.redhat.com/solutions/3193782)

## <a name="overview"></a>Genel Bakış

SAP HANA yüksek kullanılabilirlik elde etmek için iki sanal makinelere yüklenir. Veriler, HANA sistem çoğaltması kullanılarak çoğaltılır.

![SAP HANA yüksek kullanılabilirlik genel bakış](./media/sap-hana-high-availability-rhel/ha-hana.png)

SAP HANA sistem çoğaltması Kurulumu kullanır ayrılmış sanal ana bilgisayar adı ve sanal IP adresleri. Azure üzerinde bir yük dengeleyici sanal IP adresi kullanmak için gereklidir. Aşağıdaki liste, yük dengeleyici yapılandırmasını gösterir:

* Ön uç yapılandırması: IP adresi 10.0.0.13 hn1 db
* Arka uç yapılandırması: HANA sistem çoğaltması parçası olması gereken tüm sanal makinelerin birincil ağ arabirimlerine bağlı
* Araştırma bağlantı noktası: Bağlantı noktası 62503
* Yük Dengeleme kuralları: 30313 TCP, 30315 TCP, 30317 TCP, 30340 TCP, 30341 TCP, 30342 TCP

## <a name="deploy-for-linux"></a>Linux için dağıtma

Azure marketi, SAP HANA için yeni sanal makineleri dağıtmak için kullanabileceğiniz Red Hat Enterprise Linux 7.4 bir görüntü içerir.

### <a name="deploy-with-a-template"></a>Bir şablon ile dağıtım

Github üzerindeki tüm gerekli kaynakları dağıtmak için hızlı başlangıç şablonlarından birini kullanabilirsiniz. Şablonu, sanal makineler, Yük Dengeleyiciyi kullanılabilirlik kümesi ve benzeri dağıtır.
Şablonu dağıtmak için aşağıdaki adımları izleyin:

1. Açık [veritabanı şablonu] [ template-multisid-db] Azure portalında.
1. Aşağıdaki parametreleri girin:
    * **SAP sistem kimliği**: Yüklemek istediğiniz SAP sistemine SAP sistemi Kimliğini girin. Kimlik ön eki olarak dağıtılan kaynaklar için kullanılır.
    * **İşletim sistemi türü**: Linux dağıtımları birini seçin. Bu örnekte, seçin **RHEL 7**.
    * **Veritabanı türü**: Seçin **HANA**.
    * **SAP sistemi boyutu**: Yeni sisteme sağlamak için gittiği SAP sayısını girin. SAP teknoloji iş ortağı veya sistem Entegratörü, emin kaç SAP sistemi gerektiriyor olmadığınız durumlarda isteyin.
    * **Sistem kullanılabilirliği**: Seçin **HA**.
    * **Yönetici kullanıcı adı, yönetici parolası veya SSH anahtarı**: Yeni bir kullanıcı oluşturulur makinede oturum açmak için kullanılabilir.
    * **Alt ağ kimliği**: Tanımlanan bir alt ağa sahip olduğunuz mevcut bir Vnet'te VM dağıtmak istiyorsanız, VM atanmalıdır belirli bir alt ağ kimliği adı için. Kimliği genellikle gibi görünüyor **/subscriptions/\<abonelik kimliği > /resourceGroups/\<kaynak grubu adı > /providers/Microsoft.Network/virtualNetworks/\<sanal ağ adı > /subnets/ \<alt ağ adı >**. Yeni bir sanal ağ oluşturmak istiyorsanız boş bırakın

### <a name="manual-deployment"></a>El ile dağıtım

1. Bir kaynak grubu oluşturun.
1. Sanal ağ oluşturun.
1. Bir kullanılabilirlik kümesi oluşturun.  
   En çok güncelleştirme etki alanı ayarlayın.
1. Bir yük dengeleyiciye (dahili) oluşturun.
   * 2. adımda oluşturduğunuz sanal ağı seçin.
1. 1 sanal makine oluşturun.  
   SAP HANA için en az Red Hat Enterprise Linux 7.4 kullanın. Bu örnekte SAP HANA görüntüsü için Red Hat Enterprise Linux 7.4 <https://portal.azure.com/#create/RedHat.RedHatEnterpriseLinux75forSAP-ARM> kullanılabilirlik 3. adımda oluşturulan kümesini seçin.
1. 2 sanal makine oluşturun.  
   SAP HANA için en az Red Hat Enterprise Linux 7.4 kullanın. Bu örnekte SAP HANA görüntüsü için Red Hat Enterprise Linux 7.4 <https://portal.azure.com/#create/RedHat.RedHatEnterpriseLinux75forSAP-ARM> kullanılabilirlik 3. adımda oluşturulan kümesini seçin.
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

## <a name="install-sap-hana"></a>SAP HANA yükleme

Aşağıdaki ön ekleri bu bölümdeki adımları kullanın:

* **[A]** : Adım tüm düğümler için geçerlidir.
* **[1]**: Bu adım yalnızca düğüm 1 için geçerlidir.
* **[2]**: Bu adım yalnızca Pacemaker kümeye 2 düğüme geçerlidir.

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

1. **[A]**  RHEL HANA yapılandırma

   RHEL SAP Not açıklandığı gibi yapılandırın [2292690] ve [2455582] ve <https://access.redhat.com/solutions/2447641>.

1. **[A]**  SAP HANA yükleyin

   SAP HANA sistem çoğaltması yüklemek için izleyin <https://access.redhat.com/articles/3004101>.

   * Çalıştırma **hdblcm** HANA DVD'den program. Komut isteminde aşağıdaki değerleri girin:
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

1. **[A]**  Güvenlik duvarını yapılandırma

   Azure yük dengeleyici yoklama bağlantı noktası için güvenlik duvarı kuralı oluşturun.

   <pre><code>sudo firewall-cmd --zone=public --add-port=625<b>03</b>/tcp
   sudo firewall-cmd --zone=public --add-port=625<b>03</b>/tcp --permanent
   </code></pre>

## <a name="configure-sap-hana-20-system-replication"></a>SAP HANA 2.0 sistem çoğaltmayı yapılandırma

Aşağıdaki ön ekleri bu bölümdeki adımları kullanın:

* **[A]** : Adım tüm düğümler için geçerlidir.
* **[1]**: Bu adım yalnızca düğüm 1 için geçerlidir.
* **[2]**: Bu adım yalnızca Pacemaker kümeye 2 düğüme geçerlidir.

1. **[A]**  Güvenlik duvarını yapılandırma

   HANA sistem çoğaltması ve istemci trafiğine izin vermek için güvenlik duvarı kuralları oluşturun. Gerekli bağlantı noktaları listelenir [TCP/IP bağlantı noktaları tüm SAP ürünleri](https://help.sap.com/viewer/ports). Aşağıdaki komutları veritabanı SYSTEMDB, HN1 ve NW1 Windows 2.0 HANA sistem çoğaltması ve istemci trafiğine izin vermek için yalnızca bir örnektir.

   <pre><code>sudo firewall-cmd --zone=public --add-port=40302/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=40302/tcp
   sudo firewall-cmd --zone=public --add-port=40301/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=40301/tcp
   sudo firewall-cmd --zone=public --add-port=40307/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=40307/tcp
   sudo firewall-cmd --zone=public --add-port=40303/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=40303/tcp
   sudo firewall-cmd --zone=public --add-port=40340/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=40340/tcp
   sudo firewall-cmd --zone=public --add-port=30340/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=30340/tcp
   sudo firewall-cmd --zone=public --add-port=30341/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=30341/tcp
   sudo firewall-cmd --zone=public --add-port=30342/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=30342/tcp
   </code></pre>

1. **[1]**  Kiracı veritabanı oluşturmak.

   SAP HANA 2.0 veya MDC kullanıyorsanız, SAP NetWeaver sisteminiz için bir kiracı veritabanı oluşturun. Değiştirin **NW1** SAP sisteminizin SID ile.

   Olarak Yürüt < hanasid\>adm aşağıdaki komutu:

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

1. **[1]**  Çoğaltma durumunu denetleme

   Çoğaltma durumunu denetlemek ve tüm veritabanları eşit olana kadar bekleyin. Durumu bilinmeyen kalırsa, güvenlik duvarı ayarlarınızı denetleyin.

   <pre><code>sudo su - <b>hn1</b>adm -c "python /usr/sap/<b>HN1</b>/HDB<b>03</b>/exe/python_support/systemReplicationStatus.py"
   # | Database | Host     | Port  | Service Name | Volume ID | Site ID | Site Name | Secondary | Secondary | Secondary | Secondary | Secondary     | Replication | Replication | Replication    |
   # |          |          |       |              |           |         |           | Host      | Port      | Site ID   | Site Name | Active Status | Mode        | Status      | Status Details |
   # | -------- | -------- | ----- | ------------ | --------- | ------- | --------- | --------- | --------- | --------- | --------- | ------------- | ----------- | ----------- | -------------- |
   # | SYSTEMDB | <b>hn1-db-0</b> | 30301 | nameserver   |         1 |       1 | <b>SITE1</b>     | <b>hn1-db-1</b>  |     30301 |         2 | <b>SITE2</b>     | YES           | SYNC        | ACTIVE      |                |
   # | <b>HN1</b>      | <b>hn1-db-0</b> | 30307 | xsengine     |         2 |       1 | <b>SITE1</b>     | <b>hn1-db-1</b>  |     30307 |         2 | <b>SITE2</b>     | YES           | SYNC        | ACTIVE      |                |
   # | <b>NW1</b>      | <b>hn1-db-0</b> | 30340 | indexserver  |         2 |       1 | <b>SITE1</b>     | <b>hn1-db-1</b>  |     30340 |         2 | <b>SITE2</b>     | YES           | SYNC        | ACTIVE      |                |
   # | <b>HN1</b>      | <b>hn1-db-0</b> | 30303 | indexserver  |         3 |       1 | <b>SITE1</b>     | <b>hn1-db-1</b>  |     30303 |         2 | <b>SITE2</b>     | YES           | SYNC        | ACTIVE      |                |
   #
   # status system replication site "2": ACTIVE
   # overall system replication status: ACTIVE
   #
   # Local System Replication State
   # ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
   #
   # mode: PRIMARY
   # site id: 1
   # site name: <b>SITE1</b>
   </code></pre>

## <a name="configure-sap-hana-10-system-replication"></a>SAP HANA 1.0 sistem çoğaltmayı yapılandırma

Aşağıdaki ön ekleri bu bölümdeki adımları kullanın:

* **[A]** : Adım tüm düğümler için geçerlidir.
* **[1]**: Bu adım yalnızca düğüm 1 için geçerlidir.
* **[2]**: Bu adım yalnızca Pacemaker kümeye 2 düğüme geçerlidir.

1. **[A]**  Güvenlik duvarını yapılandırma

   HANA sistem çoğaltması ve istemci trafiğine izin vermek için güvenlik duvarı kuralları oluşturun. Gerekli bağlantı noktaları listelenir [TCP/IP bağlantı noktaları tüm SAP ürünleri](https://help.sap.com/viewer/ports). Aşağıdaki komutları Windows 2.0 HANA sistem çoğaltması izin vermek için yalnızca bir örnektir. Bu, SAP HANA 1.0 yüklemenizi uyarlayın.

   <pre><code>sudo firewall-cmd --zone=public --add-port=40302/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=40302/tcp
   </code></pre>

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

   <pre><code>HDB stop
   hdbnsutil -sr_register --remoteHost=<b>hn1-db-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b>
   HDB start
   </code></pre>

## <a name="create-a-pacemaker-cluster"></a>Pacemaker küme oluşturma

Bağlantısındaki [SLES azure'da Red Hat Enterprise Linux üzerinde Pacemaker ayarlama](high-availability-guide-rhel-pacemaker.md) HANA bu sunucu için temel Pacemaker küme oluşturmak için.

## <a name="create-sap-hana-cluster-resources"></a>SAP HANA küme kaynaklarını oluşturma

SAP HANA kaynak aracıları yükleyin **tüm düğümleri**. Paketi içeren bir depo etkinleştirdiğinizden emin olun.

<pre><code># Enable repository that contains SAP HANA resource agents
sudo subscription-manager repos --enable="rhel-sap-hana-for-rhel-7-server-rpms"
   
sudo yum install -y resource-agents-sap-hana
</code></pre>

Ardından, HANA topolojisi oluşturun. Pacemaker küme düğümlerinden biri üzerinde aşağıdaki komutları çalıştırın:

<pre><code>sudo pcs property set maintenance-mode=true

# Replace the bold string with your instance number and HANA system ID
sudo pcs resource create SAPHanaTopology_<b>HN1</b>_<b>03</b> SAPHanaTopology SID=<b>HN1</b> InstanceNumber=<b>03</b> --clone clone-max=2 clone-node-max=1 interleave=true
</code></pre>

Ardından, HANA kaynakları oluşturun:

<pre><code># Replace the bold string with your instance number, HANA system ID, and the front-end IP address of the Azure load balancer.

sudo pcs resource create SAPHana_<b>HN1</b>_<b>03</b> SAPHana SID=<b>HN1</b> InstanceNumber=<b>03</b> PREFER_SITE_TAKEOVER=true DUPLICATE_PRIMARY_TIMEOUT=7200 AUTOMATED_REGISTER=false master notify=true clone-max=2 clone-node-max=1 interleave=true

sudo pcs resource create vip_<b>HN1</b>_<b>03</b> IPaddr2 ip="<b>10.0.0.13</b>"

sudo pcs resource create nc_<b>HN1</b>_<b>03</b> azure-lb port=625<b>03</b>

sudo pcs resource group add g_ip_<b>HN1</b>_<b>03</b> nc_<b>HN1</b>_<b>03</b> vip_<b>HN1</b>_<b>03</b>

sudo pcs constraint order SAPHanaTopology_<b>HN1</b>_<b>03</b>-clone then SAPHana_<b>HN1</b>_<b>03</b>-master symmetrical=false

sudo pcs constraint colocation add g_ip_<b>HN1</b>_<b>03</b> with master SAPHana_<b>HN1</b>_<b>03</b>-master 4000

sudo pcs property set maintenance-mode=false
</code></pre>

Küme durumunun Tamam olduğunu ve tüm kaynakları başlatıldığından emin olun. Hangi düğümünde kaynaklarını çalıştıran önemli değildir.

<pre><code>sudo pcs status

# Online: [ hn1-db-0 hn1-db-1 ]
#
# Full list of resources:
#
# azure_fence     (stonith:fence_azure_arm):      Started hn1-db-0
#  Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
#      Started: [ hn1-db-0 hn1-db-1 ]
#  Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
#      Masters: [ hn1-db-0 ]
#      Slaves: [ hn1-db-1 ]
#  Resource Group: g_ip_HN1_03
#      nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-0
#      vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-0
</code></pre>

## <a name="test-the-cluster-setup"></a>Test kümesi Kurulumu

Bu bölümde, kurulumunuzu nasıl sınayıp doğrulayabileceğiniz açıklanır. Bir test başlamadan önce Pacemaker (bilgisayarlar durumu) aracılığıyla başarısız olan bir eylem yok, beklenmeyen konumu kısıtlamalar (örneğin bir geçiş testi parçalarla) vardır ve HANA eşitleme durumunda, örneğin systemReplicationStatus sahip olduğundan emin olun:

<pre><code>[root@hn1-db-0 ~]# sudo su - hn1adm -c "python /usr/sap/HN1/HDB03/exe/python_support/systemReplicationStatus.py"
</code></pre>

### <a name="test-the-migration"></a>Test geçişi

Kaynak durumu, test başlamadan önce:

<pre><code>Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
    Started: [ hn1-db-0 hn1-db-1 ]
Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
    Masters: [ hn1-db-0 ]
    Slaves: [ hn1-db-1 ]
Resource Group: g_ip_HN1_03
    nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-0
    vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-0
</code></pre>

Aşağıdaki komutu yürüterek SAP HANA ana düğüm geçirebilirsiniz:

<pre><code>[root@hn1-db-0 ~]# pcs resource move SAPHana_HN1_03-master
</code></pre>

Ayarlarsanız `AUTOMATED_REGISTER="false"`, bu komut, SAP HANA ana düğümü ve hn1-db-1 sanal IP adresi içeren grubunu geçirmeniz gerekir.

Geçiş yaptıktan sonra 'sudo bilgisayarları status' çıktı şuna benzer

<pre><code>Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
    Started: [ hn1-db-0 hn1-db-1 ]
Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
    Masters: [ hn1-db-1 ]
    Stopped: [ hn1-db-0 ]
Resource Group: g_ip_HN1_03
    nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-1
    vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-1
</code></pre>

SAP HANA kaynak hn1-db-0 durduruldu. Bu durumda, HANA örneği ikincil bu komutunu yürüterek yapılandırın:

<pre><code>[root@hn1-db-0 ~]# su - hn1adm

# Stop the HANA instance just in case it is running
hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> sapcontrol -nr 03 -function StopWait 600 10
hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=hn1-db-1 --remoteInstance=03 --replicationMod
e=sync --name=SITE1
</code></pre>

Geçiş yeniden silinmesi gereken konum kısıtlamaları oluşturur:

<pre><code># Switch back to root
exit
[root@hn1-db-0 ~]# pcs resource clear SAPHana_HN1_03-master
</code></pre>

'Bilgisayarları status' kullanarak HANA kaynak durumunu izleyin. HANA hn1-db-0 başlatıldıktan sonra çıktı aşağıdaki gibi görünmelidir

<pre><code>Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
    Started: [ hn1-db-0 hn1-db-1 ]
Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
    Masters: [ hn1-db-1 ]
    Slaves: [ hn1-db-0 ]
Resource Group: g_ip_HN1_03
    nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-1
    vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-1
</code></pre>

### <a name="test-the-azure-fencing-agent"></a>Azure sınır Aracısı'nı test edin

Kaynak durumu, test başlamadan önce:

<pre><code>Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
    Started: [ hn1-db-0 hn1-db-1 ]
Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
    Masters: [ hn1-db-1 ]
    Slaves: [ hn1-db-0 ]
Resource Group: g_ip_HN1_03
    nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-1
    vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-1
</code></pre>

SAP HANA Yöneticisi olarak çalıştığı düğüm ağ arabiriminde devre dışı bırakarak Azure çitlemek aracı Kurulumu test edebilirsiniz.
Bkz: [Red Hat Bilgi Bankası makalesi 79523](https://access.redhat.com/solutions/79523) ağ hata benzetimi yapma konusunda bir açıklama için. Bu örnekte, tüm ağ erişimi engellemek için net_breaker komut dosyasını kullanın.

<pre><code>[root@hn1-db-1 ~]# sh ./net_breaker.sh BreakCommCmd 10.0.0.6
</code></pre>

Sanal makine şimdi yeniden başlatın veya küme yapılandırmanıza bağlı olarak Durdur gerekir.
Ayarlarsanız `stonith-action` kapalı olarak ayarlama, sanal makinenin durdurulması ve kaynakları çalışan sanal makineye geçirilir.

> [!NOTE]
> Bu sanal makineler olana kadar çevrimiçi yeniden tamamlanması 15 dakika sürebilir.

Sanal makine yeniden başlattıktan sonra ikincil ayarlarsanız olarak başlatmak SAP HANA kaynak başarısız `AUTOMATED_REGISTER="false"`. Bu durumda, HANA örneği ikincil bu komutunu yürüterek yapılandırın:

<pre><code>su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> sapcontrol -nr <b>03</b> -function StopWait 600 10
hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=<b>hn1-db-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b>

# Switch back to root and clean up the failed state
exit
[root@hn1-db-1 ~]# pcs resource cleanup SAPHana_HN1_03-master
</code></pre>

Kaynak durumu test sonra:

<pre><code>Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
    Started: [ hn1-db-0 hn1-db-1 ]
Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
    Masters: [ hn1-db-0 ]
    Slaves: [ hn1-db-1 ]
Resource Group: g_ip_HN1_03
    nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-0
    vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-0
</code></pre>

### <a name="test-a-manual-failover"></a>Elle yük devretme testi

Kaynak durumu, test başlamadan önce:

<pre><code>Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
    Started: [ hn1-db-0 hn1-db-1 ]
Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
    Masters: [ hn1-db-0 ]
    Slaves: [ hn1-db-1 ]
Resource Group: g_ip_HN1_03
    nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-0
    vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-0
</code></pre>

Elle yük devretme hn1-db-0 düğümde Küme durdurarak test edebilirsiniz:

<pre><code>[root@hn1-db-0 ~]# pcs cluster stop
</code></pre>

Yük devretmeden sonra kümeyi yeniden başlatabilirsiniz. Ayarlarsanız `AUTOMATED_REGISTER="false"`, ikincil olarak başlatmak SAP HANA kaynak hn1-db-0 düğüm üzerinde başarısız olur. Bu durumda, HANA örneği ikincil bu komutunu yürüterek yapılandırın:

<pre><code>[root@hn1-db-0 ~]# pcs cluster start
[root@hn1-db-0 ~]# su - hn1adm

# Stop the HANA instance just in case it is running
hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> sapcontrol -nr 03 -function StopWait 600 10
hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>

# Switch back to root and clean up the failed state
hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> exit
[root@hn1-db-1 ~]# pcs resource cleanup SAPHana_HN1_03-master
</code></pre>

Kaynak durumu test sonra:

<pre><code>Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
    Started: [ hn1-db-0 hn1-db-1 ]
Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
    Masters: [ hn1-db-1 ]
     Slaves: [ hn1-db-0 ]
Resource Group: g_ip_HN1_03
    nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-1
    vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-1
</code></pre>

## <a name="next-steps"></a>Sonraki adımlar

* [Azure sanal makineleri planlama ve uygulama için SAP][planning-guide]
* [SAP için Azure sanal makineler dağıtımı][deployment-guide]
* [SAP için Azure sanal makineleri DBMS dağıtım][dbms-guide]
* Yüksek kullanılabilirlik ve olağanüstü durum kurtarma SAP hana (büyük örnekler) azure'da planlama oluşturma hakkında bilgi almak için bkz: [SAP HANA (büyük örnekler) azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md)
