---
title: SLES 12 SP3 ile Azure sanal makineler'de SAP HANA 2.0 genişleme HSR Pacemaker Kurulumu sorunlarını giderme | Microsoft Docs
description: Denetleyin ve SAP HANA sistem çoğaltması (HSR) ve Azure sanal makineler üzerinde çalışan SLES üzerinde Pacemaker SLES 12 SP3 göre karmaşık bir SAP HANA genişletmek kullanılabilirliğinden yapılandırma sorunlarını giderme kılavuzu
services: virtual-machines-linux
documentationcenter: ''
author: hermannd
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/24/2018
ms.author: hermannd
ms.openlocfilehash: 6c0d6397246e8b8db1d59c26229e37a722d49f48
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47184977"
---
# <a name="verify-and-troubleshoot-sap-hana-scale-out-high-availability-setup-on-sles-12-sp3"></a>Doğrulayın ve SLES 12 SP3 üzerinde SAP HANA genişleme yüksek kullanılabilirlik Kurulumu sorunlarını giderme 

[sles-pacemaker-ha-guide]:high-availability-guide-suse-pacemaker.md
[sles-hana-scale-out-ha-paper]:https://www.suse.com/documentation/suse-best-practices/singlehtml/SLES4SAP-hana-scaleOut-PerfOpt-12/SLES4SAP-hana-scaleOut-PerfOpt-12.html
[sap-hana-iaas-list]:https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html
[suse-pacemaker-support-log-files]:https://www.suse.com/support/kb/doc/?id=7022702
[azure-linux-multiple-nics]:https://docs.microsoft.com/azure/virtual-machines/linux/multiple-nics
[suse-cloud-netconfig]:https://www.suse.com/c/multi-nic-cloud-netconfig-ec2-azure/
[sap-list-port-numbers]:https://help.sap.com/viewer/ports
[sles-12-ha-paper]:https://www.suse.com/documentation/sle-ha-12/pdfdoc/book_sleha/book_sleha.pdf
[sles-zero-downtime-paper]:https://www.suse.com/media/presentation/TUT90846_towards_zero_downtime%20_how_to_maintain_sap_hana_system_replication_clusters.pdf
[sap-nw-ha-guide-sles]:high-availability-guide-suse.md
[sles-12-for-sap]:https://www.suse.com/media/white-paper/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf


Bu makale, Azure sanal makineler'de çalışan SAP HANA genişleme Pacemaker küme yapılandırmasını denetlemek için yazılmıştır. SAP HANA sistem çoğaltması (HSR) ile birlikte Küme kurulumu gerçekleştirilebilir ve SUSE RPM paketini SAPHanaSR genişletme. Tüm testleri üzerinde SUSE SLES 12 SP3 yalnızca yapıldığını. Farklı alanlarını kapsar ve örnek komutlar ve yapılandırma dosyalarından forumlarından dahil birkaç bölümü vardır. Bu örnekler, doğrulamak ve tüm küme ayarlarını denetlemek için bir yöntem önerilir.



## <a name="important-notes"></a>Önemli Notlar

SAP HANA ölçeklendirme SAP HANA sistem çoğaltması ve Pacemaker ile birlikte tüm test SAP HANA 2.0 ile yalnızca yapılmadı. İşletim sistemi sürümü, SUSE Linux Enterprise Server 12 SP3 SAP uygulamaları için oluştu. Buna ek olarak en son RPM paketinden SAPHanaSR genişletme SUSE pacemaker kümesini ayarlamak için kullanıldı.
SUSE yayımlanan bulunabilir, bu en iyi duruma getirilmiş performans Kurulum, ayrıntılı bir açıklaması [burada][sles-hana-scale-out-ha-paper]

Sanal makine türleri için SAP HANA genişleme denetle desteklenir [SAP HANA sertifikalı ve Iaas dizini][sap-hana-iaas-list]

SAP HANA ölçeklendirme birlikte birden çok alt ağları ve Vnıc'ler ve HSR ayarlama ile ilgili teknik bir sorun oluştu. Burada Bu sorun düzeltilmiştir en son düzeltme eklerini SAP HANA 2.0 kullanmak için zorunludur. Aşağıdaki SAP HANA sürümleri desteklenir: 

**Rev2.00.024.04 veya üzeri & rev2.00.032 veya üzeri.**

SUSE desteğini gerektirir bir durum olmalıdır durumda izleyin [Kılavuzu][suse-pacemaker-support-log-files]. Makalesinde açıklandığı gibi SAP HANA yüksek kullanılabilirlik kümesi hakkındaki tüm bilgileri toplayın. SUSE desteği ayrıntılı analiz için bu bilgileri gerekir.

İç test sırasında Küme kurulumu Azure portalından bir normal normal VM kapatma tarafından kafası karışıyordu oluştu. Bu nedenle diğer yöntemleri ile bir küme yük devretme testi için önerilir. Bir çekirdek Panik zorlama gibi yöntemleri kullanın veya ağları kapatın veya geçirme **msl** kaynak (aşağıdaki bölümlerde ayrıntılarına bakın). Standart bir kapatma niyetle olur varsayılır. Kasıtlı bir kapatma işlemi için en iyi örnek olan Bakım (Planlı bakım hakkında bölümünde ayrıntılarına bakın).

İç test sırasında küme bakım modunda ederken Küme kurulumu sonra el ile bir SAP HANA devralma kafası karışıyordu oluştu. Bu geri el ile yeniden, küme bakım modunu sonlandırmadan önce geçiş yapmak için önerilir. Küme bakım moduna almadan önce bir yük devretmeyi tetiklemek için başka bir seçenektir (daha fazla ayrıntı için planlı bakımla ilgili bölümüne bakın). Bu konuda, crm komutunu kullanarak küme sıfırlama nasıl SUSE belgelerinden açıklar. Ancak, belirtilen bir yaklaşım iç test sırasında ve bir daha sağlam olması gibi görünüyor önce beklenmeyen yan etkileri gösterdi.

Ne zaman crm kullanarak geçiş yapmaya komut yok kaçırmayın küme yapılandırmasını temizleme. Bunu, farkında olmadıklarınız konum kısıtlamaları, ekler. Bu kısıtlamalar küme davranışını etkiler (bölümünde planlı bakım hakkında daha fazla ayrıntılarına bakın).



## <a name="test-system-description"></a>Test sistem açıklaması

SAP HANA genişleme HA doğrulama ve bir kurulum kullanılan sertifika için SAP HANA üç düğüm ile iki sistem her - bir ana ve iki çalışan oluşan. Sanal makine adları ve iç IP adresleri listesi aşağıda verilmiştir. Daha fazla tüm doğrulama örneklerini varsayılan olarak, bu Vm'lere aşağı yapıldığını. Bu sanal makine adı ile IP adresleri komutu örneklerden komutlar ve bunların çıkışları daha iyi anlamak için yardımcı olmalıdır.


| Düğüm türü | VM adı | IP adresi |
| --- | --- | --- |
| 1 sitesinde ana düğümü | hso-hana-vm-s1-0 | 10.0.0.30 |
| Çalışan düğümü 1 sitesinde 1 | hso-hana-vm-s1-1 | 10.0.0.31 |
| 1 sitesinde 2 çalışan düğümü | hso-hana-vm-s1-2 | 10.0.0.32 |
| | | |
| Ana düğüm sitesinde 2 | hso-hana-vm-s2-0 | 10.0.0.40 |
| 1 sitesinde 2 çalışan düğümü | hso-hana-vm-s2-1 | 10.0.0.41 |
| 2. site 2 çalışan düğümü | hso-hana-vm-s2-2  | 10.0.0.42 |
| | | |
| Temel oluşturucu düğüm | hso hana dm | 10.0.0.13 |
| SBD aygıt sunucusu | hso hana sbd | 10.0.0.19 |
| | | |
| NFS Sunucusu 1 | hso-nfs-vm-0 | 10.0.0.15 |
| NFS sunucusu 2 | hso-nfs-vm-1 | 10.0.0.14 |



## <a name="multiple-subnets-and-vnics"></a>Birden çok alt ağları ve Vnıc'ler

SAP HANA ağ önerileri üç alt ağ içinde bir Azure sanal ağı oluşturulur. Azure üzerinde SAP HANA ölçeği genişletilmiş sahip her düğüm için yerel disk birimleri kullandığı anlamına gelir paylaşılmayan modunda yüklenecek **/hana/veri** ve **/hana/günlük**. Yalnızca yerel disk birimlerinin kullanılması nedeniyle depolama için ayrı bir alt ağı tanımlar gerekli değildir:

- SAP HANA düğümler arası iletişim için 10.0.2.0/24
- SAP HANA sistem çoğaltması HSR 10.0.1.0/24
- diğer her şey için 10.0.0.0/24

Kullanmayla ilgili SAP HANA yapılandırma hakkında bilgi için birden fazla ağ bölümüne bakın **global.ini** birazcık daha indiğimde.

Kümedeki her bir VM alt ağ sayısını için karşılık gelen üç Vnıc'ler sahiptir. [Bu] [ azure-linux-multiple-nics] makalede olası bir yönlendirme sorunu Azure'da bir Linux VM dağıtılırken. Bu belirli yönlendirme konu, yalnızca çoklu Vnıcs kullanımı için geçerlidir. Sorun, SLES 12 SP3'te varsayılan başına tarafından SUSE çözülür. SUSE makaleden bu konuda bulunabilir [burada][suse-cloud-netconfig].


SAP HANA birden fazla ağ kullanmak için düzgün şekilde yapılandırıldığını doğrulamak için temel onay, yalnızca aşağıdaki komutları çalıştırın. İlk adım tüm üç iç IP adreslerini üç alt ağ için etkin olan işletim sistemi düzeyinde sağlayamazsanız teknolojidir. Farklı IP adresi aralıklarını alt ağlarla tanımlanmış durumda komutları uymak zorunda:

<pre><code>
ifconfig | grep "inet addr:10\."
</code></pre>

İkinci site 2 çalışan düğümü örnek çıktısı aşağıdadır. Üç farklı iç IP adreslerinden eth0 eth1 ve eth2 görebilirsiniz:

<pre><code>
inet addr:10.0.0.42  Bcast:10.0.0.255  Mask:255.255.255.0
inet addr:10.0.1.42  Bcast:10.0.1.255  Mask:255.255.255.0
inet addr:10.0.2.42  Bcast:10.0.2.255  Mask:255.255.255.0
</code></pre>


SAP HANA bağlantı noktaları için ad sunucusu ve HSR doğrulama ikinci adımıdır. SAP HANA, karşılık gelen alt ağlar dinlemesi gereken. SAP HANA örneği sayısı bağlı olarak, komutlar uymak zorunda. Test sistemi için örnek sayısı olan **00**. Hangi bağlantı noktalarının kullanıldığını anlamak için farklı yolu vardır. 

Aşağıda, örnek kimliği ve diğer bilgileri arasında örnek numarasını döndüren bir SQL deyimi görürsünüz:

<pre><code>
select * from "SYS"."M_SYSTEM_OVERVIEW"
</code></pre>

Doğru bağlantı noktası numaralarını bulmak için örneğin, HANA Studio altında bakabilirsiniz "**yapılandırma**" ya da bir SQL deyimi ile:

<pre><code>
select * from M_INIFILE_CONTENTS WHERE KEY LIKE 'listen%'
</code></pre>

SAP HANA gibi SAP yazılım yığını kullanılır, her bağlantı noktası bulmak için arama [burada][sap-list-port-numbers].

Örnek numarasını verilen **00** SAP HANA 2.0 test sisteminde ad sunucusu için bağlantı noktası numarasıdır **30001**. HSR meta veri iletişimi için bağlantı noktası numarası **40002**. Bir çalışan düğümü için oturum açın ve ardından ana düğümü Hizmetleri denetlemek için bir seçenek var. Buraya 2 sitesinde site 2 ana düğüme bağlanmaya 2 çalışan düğümü üzerinde onay yapılır.

Ad sunucusu bağlantı noktasını denetleyin:

<pre><code>
nc -vz 10.0.0.40 30001
nc -vz 10.0.1.40 30001
nc -vz 10.0.2.40 30001
</code></pre>

Sonuç çıkış aşağıdaki alt düğümler arası iletişimin kullandığını kanıtlamak için örnek gibi görünmelidir **10.0.2.0/24**.
Yalnızca alt ağ üzerinden Bağlan **10.0.2.0/24** başarılı olması gerekir:

<pre><code>
nc: connect to 10.0.0.40 port 30001 (tcp) failed: Connection refused
nc: connect to 10.0.1.40 port 30001 (tcp) failed: Connection refused
Connection to 10.0.2.40 30001 port [tcp/pago-services1] succeeded!
</code></pre>

Şimdi HSR bağlantı noktasını denetle **40002**:

<pre><code>
nc -vz 10.0.0.40 40002
nc -vz 10.0.1.40 40002
nc -vz 10.0.2.40 40002
</code></pre>

Sonuç HSR iletişim alt kullandığını kanıtlamak için çıktı aşağıdaki örneğe benzer görünmelidir **10.0.1.0/24**.
Yalnızca alt ağ üzerinden Bağlan **10.0.1.0/24** başarılı olması gerekir:

<pre><code>
nc: connect to 10.0.0.40 port 40002 (tcp) failed: Connection refused
Connection to 10.0.1.40 40002 port [tcp/*] succeeded!
nc: connect to 10.0.2.40 port 40002 (tcp) failed: Connection refused
</code></pre>



## <a name="corosync"></a>Corosync


Doğru temel oluşturucu düğüm dahil olmak üzere kümedeki her düğümde corosync yapılandırma dosyası vardır. Bir düğüm kümesi birleşimi beklendiği gibi işe yaramadı durumda ve/veya kopyalama **/etc/corosync/corosync.conf** el ile/tüm düğümlerin ve hizmetini yeniden başlatın.

İçeriği işte **corosync.conf** test sistemi örneği olarak.

İlk bölüm **totem** açıklandığı [belgeleri] [ sles-pacemaker-ha-guide] (bölüm Küme yükleme 11. adım). Değeri yoksayabilirsiniz **mcastaddr**. Yalnızca var olan girdiyi sakla. Girişleri **belirteci** ve **fikir birliğine varılmış** bulabileceğiniz Microsoft Azure SAP HANA belgelerinize göre ayarlanmalıdır [burada][sles-pacemaker-ha-guide]

<pre><code>
totem {
    version:    2
    secauth:    on
    crypto_hash:    sha1
    crypto_cipher:  aes256
    cluster_name:   hacluster
    clear_node_high_bit: yes

    token:      30000
    token_retransmits_before_loss_const: 10
    join:       60
    consensus:  36000
    max_messages:   20

    interface {
        ringnumber:     0
        bindnetaddr: 10.0.0.0
        mcastaddr:  239.170.19.232
        mcastport:  5405

        ttl:        1
    }
    transport:      udpu

}
</code></pre>

İkinci bölüm **günlüğü** verilen varsayılanlarından değiştirilen değildi:

<pre><code>
logging {
    fileline:   off
    to_stderr:  no
    to_logfile:     no
    logfile:    /var/log/cluster/corosync.log
    to_syslog:  yes
    debug:      off
    timestamp:  on
    logger_subsys {
        subsys:     QUORUM
        debug:  off
    }
}
</code></pre>

Üçüncü bölümü gösterir **düğüm listesine**. Kümenin tüm düğümleri kendi düğüm kimliği ile gösterilecek vardır:

<pre><code>
nodelist {
  node {
   ring0_addr:hso-hana-vm-s1-0
   nodeid: 1
   }
  node {
   ring0_addr:hso-hana-vm-s1-1
   nodeid: 2
   }
  node {
   ring0_addr:hso-hana-vm-s1-2
   nodeid: 3
   }
  node {
   ring0_addr:hso-hana-vm-s2-0
   nodeid: 4
   }
  node {
   ring0_addr:hso-hana-vm-s2-1
   nodeid: 5
   }
  node {
   ring0_addr:hso-hana-vm-s2-2
   nodeid: 6
   }
  node {
   ring0_addr:hso-hana-dm
   nodeid: 7
   }
}
</code></pre>

Son bölümde **çekirdek**, değeri ayarlamak önemlidir **expected_votes** doğru. Temel oluşturucu düğüm dahil olmak üzere bir düğüm olmalıdır. Ve değeri **two_node** olmak zorundadır **0**. Giriş tamamen kaldırmayın. Değere ayarlamanız yeterlidir **0**.

<pre><code>
quorum {
    # Enable and configure quorum subsystem (default: off)
    # see also corosync.conf.5 and votequorum.5
    provider: corosync_votequorum
    expected_votes: 7
    two_node: 0
}
</code></pre>


Hizmet aracılığıyla yeniden **systemctl**:

<pre><code>
systemctl restart corosync
</code></pre>




## <a name="sbd-device"></a>SBD cihaz

Belgeleri bir Azure sanal makinesinde SBD cihazı ayarlama konusunda açıklanan [burada] [ sles-pacemaker-ha-guide] (bölüm sbd ayarlayalım).

Bir kez daha denetleyin ilk şey kümedeki her düğüm için ACL girişleri varsa SBD sunucusunda VM aramaktır. VM SBD sunucuda aşağıdaki komutu çalıştırın:


<pre><code>
targetcli ls
</code></pre>


Test sisteminde, komut çıktısı aşağıdaki örneğe benzer arıyordu. ACL adları gibi **iqn.2006-04.hso-db-0.local:hso-db-0** sanal makinelere karşılık gelen Başlatıcı adı olarak girilmelidir. Her VM'nin farklı bir tane gerekir.

<pre><code>
 | | o- sbddbhso ................................................................... [/sbd/sbddbhso (50.0MiB) write-thru activated]
  | |   o- alua ................................................................................................... [ALUA Groups: 1]
  | |     o- default_tg_pt_gp ....................................................................... [ALUA state: Active/optimized]
  | o- pscsi .................................................................................................. [Storage Objects: 0]
  | o- ramdisk ................................................................................................ [Storage Objects: 0]
  o- iscsi ............................................................................................................ [Targets: 1]
  | o- iqn.2006-04.dbhso.local:dbhso ..................................................................................... [TPGs: 1]
  |   o- tpg1 ............................................................................................... [no-gen-acls, no-auth]
  |     o- acls .......................................................................................................... [ACLs: 7]
  |     | o- iqn.2006-04.hso-db-0.local:hso-db-0 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-1.local:hso-db-1 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-2.local:hso-db-2 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-3.local:hso-db-3 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-4.local:hso-db-4 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-5.local:hso-db-5 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-6.local:hso-db-6 .................................................................. [Mapped LUNs: 1]
</code></pre>

Daha sonra tüm VM'lerin Başlatıcı adları farklıdır ve yukarıda gösterilen girişlere karşılık gelen denetleyin. Çalışan düğümü 1 sitesinde 1'den bir örnek aşağıda verilmiştir:

<pre><code>
cat /etc/iscsi/initiatorname.iscsi
</code></pre>

Çıktı aşağıdaki örneğe benzer Aranan:

<pre><code>
##
## /etc/iscsi/iscsi.initiatorname
##
## Default iSCSI Initiatorname.
##
## DO NOT EDIT OR REMOVE THIS FILE!
## If you remove this file, the iSCSI daemon will not start.
## If you change the InitiatorName, existing access control lists
## may reject this initiator.  The InitiatorName must be unique
## for each iSCSI initiator.  Do NOT duplicate iSCSI InitiatorNames.
InitiatorName=iqn.2006-04.hso-db-1.local:hso-db-1
</code></pre>

Sonraki doğrulama **bulma** düzgün şekilde çalışır ve her küme düğümünde VM SBD sunucusunun IP adresini kullanarak aşağıdaki komutu çalıştırın:

<pre><code>
iscsiadm -m discovery --type=st --portal=10.0.0.19:3260
</code></pre>

Çıktı aşağıdaki örneğe benzer görünmelidir:

<pre><code>
10.0.0.19:3260,1 iqn.2006-04.dbhso.local:dbhso
</code></pre>

Sonraki kavram noktası düğümü SDB cihaz görür doğrulamaktır. Bu, temel oluşturucu düğüm dahil olmak üzere her bir düğümde kontrol edin:

<pre><code>
lsscsi | grep dbhso
</code></pre>

Çıktı aşağıdaki örneğe benzer görünmelidir. Adları farklılık gösterebilir göz önünde bulundurun (cihaz adı aynı zamanda değişebilir VM yeniden başlatıldıktan sonra):

<pre><code>
[6:0:0:0]    disk    LIO-ORG  sbddbhso         4.0   /dev/sdm
</code></pre>

Sistem durumu bağlı olarak, sorunu çözmek için iSCSI hizmetleri yeniden başlatmak bazen yardımcı olabilir. Sonra aşağıdaki komutları çalıştırın:

<pre><code>
systemctl restart iscsi
systemctl restart iscsid
</code></pre>


Herhangi bir düğümünden tüm düğümleri olup olmadığını denetleyebilir **Temizle**. Yalnızca belirli bir düğümde doğru cihaz adını kullanmaya dikkat edin:

<pre><code>
sbd -d /dev/sdm list
</code></pre>

Çıkış göstermelidir **Temizle** kümedeki her düğüm için:

<pre><code>
0       hso-hana-vm-s1-0        clear
1       hso-hana-vm-s2-2        clear
2       hso-hana-vm-s2-1        clear
3       hso-hana-dm     clear
4       hso-hana-vm-s1-1        clear
5       hso-hana-vm-s2-0        clear
6       hso-hana-vm-s1-2        clear
</code></pre>


Başka bir SBD denetleyin **döküm** sbd komut seçeneği. İşte bir örnek komut ve burada cihaz adı değil temel oluşturucu düğüm çıktısını **sdm** ancak **sdd**:

<pre><code>
sbd -d /dev/sdd dump
</code></pre>

Çıktı (cihaz adı dışında) tüm düğümlerde aynı görünmelidir:

<pre><code>
==Dumping header on disk /dev/sdd
Header version     : 2.1
UUID               : 9fa6cc49-c294-4f1e-9527-c973f4d5a5b0
Number of slots    : 255
Sector size        : 512
Timeout (watchdog) : 60
Timeout (allocate) : 2
Timeout (loop)     : 1
Timeout (msgwait)  : 120
==Header on disk /dev/sdd is dumped
</code></pre>

Bir daha fazla onay SBD için başka bir düğüme bir ileti göndermek için olasılıktır. Aşağıdaki komutu çalışan düğümündeki 1 sitesinde 2 2. site 2 çalışan düğümü bir ileti göndermek için çalıştırın:

<pre><code>
sbd -d /dev/sdm message hso-hana-vm-s2-2 test
</code></pre>

Hedef VM tarafı - olduğu **hso-hana-vm-s2-2** Bu örnekte - dosyasında şu girişi bulun **/var/log/messages**:

<pre><code>
/dev/disk/by-id/scsi-36001405e614138d4ec64da09e91aea68:   notice: servant: Received command test from hso-hana-vm-s2-1 on disk /dev/disk/by-id/scsi-36001405e614138d4ec64da09e91aea68
</code></pre>

Varsa denetleyin girişleri **/etc/sysconfig/sbd** açıklamasında karşılık bizim [belgeleri] [ sles-pacemaker-ha-guide] (bölüm sbd ayarlayalım). Ayarı başlangıç doğrulayın **/etc/iscsi/iscsid.conf** otomatik olarak ayarlanır.

Önemli girişler **/etc/sysconfig/sbd** (gerekirse kimlik değerini uyum):

<pre><code>
SBD_DEVICE="/dev/disk/by-id/scsi-36001405e614138d4ec64da09e91aea68;"
SBD_PACEMAKER=yes
SBD_STARTMODE=always
SBD_WATCHDOG=yes
</code></pre>


Denetlenecek başka bir öğe başlangıç ayardır **/etc/iscsi/iscsid.conf**. Gerekli ayarı tarafından ne olması gerektiğini **iscsiadm** gösterilen komut aşağıda açıklanan belgelerinde. Doğrulayın ve belki de bunu el ile uyum mantıklıdır **VI** farklı olması durumunda.

Başlangıç davranışını ayarlamak için komut:

<pre><code>
iscsiadm -m node --op=update --name=node.startup --value=automatic
</code></pre>

Giriş **/etc/iscsi/iscsid.conf**:

<pre><code>
node.startup = automatic
</code></pre>

Sınama ve doğrulama sırasında oluşum oldu burada VM yeniden başlatma sonrasında SBD cihaz artık görünür değildi. Başlatma ayarı ve hangi yast2 gösterdi arasında bir tutarsızlık vardı. Ayarları denetleyin için aşağıdaki adımları gerçekleştirin:

1. Yast2 Başlat
2. Seçin **Ağ Hizmetleri** sol tarafındaki
3. Aşağı kaydırın için sağ taraftaki **iSCSI başlatıcısı** ve bunu seçin
4. Sonraki ekranda altında **hizmet** sekmesi, düğüm için benzersiz bir başlatıcı ad görmeniz gerekir
5. Başlatıcı adı emin **hizmetini** değeri ayarı **olduğunda önyükleme**
6. Böyle değilse, ayarlayabilirsiniz **olduğunda önyükleme** yerine **el ile**
7. Sonraki üst sekmeye geçin **bağlı hedefleri**
8. Bağlı hedefleri ekranda Bu örnek gibi SBD cihaz için bir giriş görmeniz gerekir: **10.0.0.19:3260 iqn.2006-04.dbhso.local:dbhso**
9. Başlangıç değeri şuna ayarlı olmadığını denetle "**onboot**"
10. Aksi takdirde, seçin **Düzenle** ve değiştirin
11. Değişiklikleri kaydetmek ve yast2 çıkın



## <a name="pacemaker"></a>Pacemaker

Her şeyin doğru şekilde ayarlandıktan sonra her düğüm üzerinde pacemaker hizmet durumunu denetlemek için aşağıdaki komutu çalıştırabilirsiniz:

<pre><code>
systemctl status pacemaker
</code></pre>

Çıkış üst örnek gibi görünmelidir. Önemli olduğu, sonra durum **etkin** olarak gösterilen **yüklenen** ve **etkin (çalışan)**. Durumu "Yüklü" olarak gösterilmelidir sonra **etkin**.

<pre><code>
  pacemaker.service - Pacemaker High Availability Cluster Manager
   Loaded: loaded (/usr/lib/systemd/system/pacemaker.service; enabled; vendor preset: disabled)
   Active: active (running) since Fri 2018-09-07 05:56:27 UTC; 4 days ago
     Docs: man:pacemakerd
           http://clusterlabs.org/doc/en-US/Pacemaker/1.1-pcs/html/Pacemaker_Explained/index.html
 Main PID: 4496 (pacemakerd)
    Tasks: 7 (limit: 4915)
   CGroup: /system.slice/pacemaker.service
           ├─4496 /usr/sbin/pacemakerd -f
           ├─4499 /usr/lib/pacemaker/cib
           ├─4500 /usr/lib/pacemaker/stonithd
           ├─4501 /usr/lib/pacemaker/lrmd
           ├─4502 /usr/lib/pacemaker/attrd
           ├─4503 /usr/lib/pacemaker/pengine
           └─4504 /usr/lib/pacemaker/crmd
</code></pre>

Ayar hala olması durumunda **devre dışı**, aşağıdaki komutu çalıştırın:

<pre><code>
systemctl enable pacemaker
</code></pre>

Tüm yapılandırılmış kaynakların pacemaker görmek için aşağıdaki komutu çalıştırın:

<pre><code>
crm status
</code></pre>

Çıktı aşağıdaki örneğe benzer görünmelidir. Tamam cln ve msl kaynakları çoğunluğu Oluşturucu VM durduruldu olarak gösterilir (**hso hana dm**). Hiçbir temel oluşturucu düğüm üzerinde SAP HANA yüklemesi yok. Bu nedenle **cln** ve **msl** kaynakları durduruldu olarak gösterilir. VM'lerin doğru toplam sayısını gösteren önemlidir (**7**). Kümenin parçası olan tüm sanal makineler durumuyla listelenmiş olmalıdır **çevrimiçi**. Geçerli birincil ana düğüm doğru tanınan gerekir (Bu örnekte bu **hso-hana-vm-s1-0**).

<pre><code>
Stack: corosync
Current DC: hso-hana-dm (version 1.1.16-4.8-77ea74d) - partition with quorum
Last updated: Tue Sep 11 15:56:40 2018
Last change: Tue Sep 11 15:56:23 2018 by root via crm_attribute on hso-hana-vm-s1-0

7 nodes configured
17 resources configured

Online: [ hso-hana-dm hso-hana-vm-s1-0 hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started hso-hana-dm
 Clone Set: cln_SAPHanaTop_HSO_HDB00 [rsc_SAPHanaTop_HSO_HDB00]
     Started: [ hso-hana-vm-s1-0 hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]
     Stopped: [ hso-hana-dm ]
 Master/Slave Set: msl_SAPHanaCon_HSO_HDB00 [rsc_SAPHanaCon_HSO_HDB00]
     Masters: [ hso-hana-vm-s1-0 ]
     Slaves: [ hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]
     Stopped: [ hso-hana-dm ]
 Resource Group: g_ip_HSO_HDB00
     rsc_ip_HSO_HDB00   (ocf::heartbeat:IPaddr2):       Started hso-hana-vm-s1-0
     rsc_nc_HSO_HDB00   (ocf::heartbeat:anything):      Started hso-hana-vm-s1-0
</code></pre>

Bakım moduna almak için yer pacemaker önemli bir özelliğidir. Bu mod (örneğin bir VM yeniden başlatma) bir değişiklik yapmadan hemen küme eylem provoking olmadan sağlar. Tipik bir kullanım örneği planlı işletim sistemi ya da Azure altyapı bakım olacaktır (Ayrıca planlı bakım hakkında ayrı bölümüne bakın). Pacemaker bakım moduna almak için aşağıdaki komutu kullanın:

<pre><code>
crm configure property maintenance-mode=true
</code></pre>

İle denetlenirken **crm durumu**, tüm kaynaklar olarak işaretlenmiş çıktıda fark **yönetilmeyen**. Bu durumda, küme üzerinde SAP HANA başlatma/durdurma gibi herhangi bir değişiklik tepki vermez.
Örnek Çıktı, işte **crm durumu** küme bakım modundayken komutu:

<pre><code>
Stack: corosync
Current DC: hso-hana-dm (version 1.1.16-4.8-77ea74d) - partition with quorum
Last updated: Wed Sep 12 07:48:10 2018
Last change: Wed Sep 12 07:46:54 2018 by root via cibadmin on hso-hana-vm-s2-1

7 nodes configured
17 resources configured

              *** Resource management is DISABLED ***
  The cluster will not attempt to start, stop or recover services

Online: [ hso-hana-dm hso-hana-vm-s1-0 hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started hso-hana-dm (unmanaged)
 Clone Set: cln_SAPHanaTop_HSO_HDB00 [rsc_SAPHanaTop_HSO_HDB00] (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s1-1 (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s1-0 (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s1-2 (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s2-1 (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s2-2 (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s2-0 (unmanaged)
     Stopped: [ hso-hana-dm ]
 Master/Slave Set: msl_SAPHanaCon_HSO_HDB00 [rsc_SAPHanaCon_HSO_HDB00] (unmanaged)
     rsc_SAPHanaCon_HSO_HDB00   (ocf::suse:SAPHanaController):  Slave hso-hana-vm-s1-1 (unmanaged)
     rsc_SAPHanaCon_HSO_HDB00   (ocf::suse:SAPHanaController):  Slave hso-hana-vm-s1-2 (unmanaged)
     rsc_SAPHanaCon_HSO_HDB00   (ocf::suse:SAPHanaController):  Slave hso-hana-vm-s2-1 (unmanaged)
     rsc_SAPHanaCon_HSO_HDB00   (ocf::suse:SAPHanaController):  Slave hso-hana-vm-s2-2 (unmanaged)
     rsc_SAPHanaCon_HSO_HDB00   (ocf::suse:SAPHanaController):  Master hso-hana-vm-s2-0 (unmanaged)
     Stopped: [ hso-hana-dm hso-hana-vm-s1-0 ]
 Resource Group: g_ip_HSO_HDB00
     rsc_ip_HSO_HDB00   (ocf::heartbeat:IPaddr2):       Started hso-hana-vm-s2-0 (unmanaged)
     rsc_nc_HSO_HDB00   (ocf::heartbeat:anything):      Started hso-hana-vm-s2-0 (unmanaged)
</code></pre>


Ve aşağıdaki komut örnek küme bakım modunu sonlandırmak bkz:

<pre><code>
crm configure property maintenance-mode=false
</code></pre>


Başka bir crm komutu tam küme yapılandırma düzenleyicisine düzenlemek için bir ekrandan alınıyor sağlar. Değişiklikleri kaydettikten sonra küme uygun eylemleri başlar:

<pre><code>
crm configure edit
</code></pre>

Yalnızca tam küme yapılandırmasını aramak için crm kullanın **Göster** seçeneği:

<pre><code>
crm configure show
</code></pre>



Küme kaynaklarının hatasından sonra onu olur, bu **crm durumu** komut listesi gösterilir **başarısız Eylemler**. Bir örnek için bu Çıktıyı aşağıya bakın:


<pre><code>
Stack: corosync
Current DC: hso-hana-dm (version 1.1.16-4.8-77ea74d) - partition with quorum
Last updated: Thu Sep 13 07:30:44 2018
Last change: Thu Sep 13 07:30:20 2018 by root via crm_attribute on hso-hana-vm-s1-0

7 nodes configured
17 resources configured

Online: [ hso-hana-dm hso-hana-vm-s1-0 hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started hso-hana-dm
 Clone Set: cln_SAPHanaTop_HSO_HDB00 [rsc_SAPHanaTop_HSO_HDB00]
     Started: [ hso-hana-vm-s1-0 hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]
     Stopped: [ hso-hana-dm ]
 Master/Slave Set: msl_SAPHanaCon_HSO_HDB00 [rsc_SAPHanaCon_HSO_HDB00]
     Masters: [ hso-hana-vm-s1-0 ]
     Slaves: [ hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]
     Stopped: [ hso-hana-dm hso-hana-vm-s2-0 ]
 Resource Group: g_ip_HSO_HDB00
     rsc_ip_HSO_HDB00   (ocf::heartbeat:IPaddr2):       Started hso-hana-vm-s1-0
     rsc_nc_HSO_HDB00   (ocf::heartbeat:anything):      Started hso-hana-vm-s1-0

Failed Actions:
* rsc_SAPHanaCon_HSO_HDB00_monitor_60000 on hso-hana-vm-s2-0 'unknown error' (1): call=86, status=complete, exitreason='none',
    last-rc-change='Wed Sep 12 17:01:28 2018', queued=0ms, exec=277663ms
</code></pre>

Hatasından sonra küme temizleme yapmak gereklidir. Yalnızca crm komutunu tekrar kullanın ve komut seçeneği **Temizleme** başarısız bunlardan kurtulmak için karşılık gelen adlandırma eylemi girişleri aşağıda gösterildiği gibi küme kaynağı:

<pre><code>
crm resource cleanup rsc_SAPHanaCon_HSO_HDB00
</code></pre>

Komutu aşağıdaki örnekte gibi görünen bir çıktı döndürülmesi gerekir:

<pre><code>
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-dm
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s1-0
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s1-1
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s1-2
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s2-0
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s2-1
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s2-2
Waiting for 7 replies from the CRMd....... OK
</code></pre>



## <a name="failover--takeover"></a>Yük devretme / devralma

Zaten önemli notlar ile ilk bölümde bahsedildiği gibi küme yük devretmesi veya SAP HANA HSR devralma işlemini test etmek için standart normal şekilde kapatılmasını kullanmamalısınız. Bunun yerine kaynak geçişi zorla tetiklemek, örneğin, bir çekirdek Panik veya belki de bir sanal makinenin işletim sistemi düzeyindeki tüm ağlar kapatmanız önerilir. Başka bir yöntem olabilir **crm \<düğüm\> bekleme** komutu. Ayrıca bulunabilir SUSE belge bkz [burada][sles-12-ha-paper]. Aşağıda, üç örnek komutlar bir küme yük devretmeye zorlamak için görürsünüz:

<pre><code>
echo c &gt /proc/sysrq-trigger

crm resource migrate msl_SAPHanaCon_HSO_HDB00 hso-hana-vm-s2-0 force

wicked ifdown eth0
wicked ifdown eth1
wicked ifdown eth2
......
wicked ifdown eth&ltn&gt
</code></pre>

Çalıştırmak için de açıklandığı gibi planlı bakım hakkında bölümünde, küme etkinlikleri izlemek için en iyi yolu olan **SAPHanaSR showAttr** ile **watch** komutu:

<pre><code>
watch SAPHanaSR-showAttr
</code></pre>

Ayrıca, bunu bir SAP python betiğini yakında SAP HANA landscape durum bakmak için yardımcı olur. Küme kurulumu aramak için bir durum değerdir. Bir alt düğümde hata oluştuktan hakkında düşünürken, açık hale gelir. Bir çalışan düğümü kalırsa, SAP HANA hemen tüm genişleme sistem durumu için bir hata döndürmez. Gereksiz yük devretme işlemleri önlemek için bazı deneme vardır. Küme, yalnızca durum hatası (dönüş değeri 1) Tamam (dönüş değeri 4) değişirse tepki verir. Bu nedenle doğru olduğundan, çıkışı **SAPHanaSR showAttr** durumuna sahip bir VM gösterir **çevrimdışı** ancak henüz birincil ve ikincil anahtar etkinliği yok. SAP HANA bir hata döndürmez sürece hiçbir küme etkinlik tetiklenen.

Kullanıcı olarak SAP HANA yatay sistem durumunu izleyebilir \<HANA SID\>SAP python çağırarak adm betik aşağıdaki şekilde (yol uyum gerekebilir):

<pre><code>
watch python /hana/shared/HSO/exe/linuxx86_64/HDB_2.00.032.00.1533114046_eeaf4723ec52ed3935ae0dc9769c9411ed73fec5/python_support/landscapeHostConfiguration.py
</code></pre>

Bu komutun çıktısı, aşağıdaki örnek gibi görünmelidir. Önemli **konak durumu** sütun yanı sıra **genel ana bilgisayar durumu**. Gerçek çıkış aslında için ek sütunlar geniş olması gerekir.
Çıktı tablosu bu belgede daha okunaklı hale getirmek için en fazla sütuna sağ tarafındaki kesilmiş:

<pre><code>
| Host             | Host   | Host   | Failover | Remove | 
|                  | Active | Status | Status   | Status | 
|                  |        |        |          |        | 
| ---------------- | ------ | ------ | -------- | ------ |    .......
| hso-hana-vm-s2-0 | yes    | ok     |          |        |        
| hso-hana-vm-s2-1 | yes    | ok     |          |        |         
| hso-hana-vm-s2-2 | yes    | ok     |          |        |        

overall host status: ok
</code></pre>


Geçerli Küme etkinlikleri denetlemek için başka bir komut yoktur. Birincil sitenin ana düğüm sonlandırıldı sonra komut ve çıkış kuyruğunu bakın. Geçiş eylemleri gibi listesini görebilirsiniz **yükseltme** önceki ikincil ana düğüm (**hso-hana-vm-s2-0**) yeni birincil anahtarı olarak. Her şey Tamam ve tüm etkinlikleri tamamlanmış olan, bu listesi **geçiş özeti** boş olması gerekir.

<pre><code>
 crm_simulate -Ls

...........

Transition Summary:
 * Fence hso-hana-vm-s1-0
 * Stop    rsc_SAPHanaTop_HSO_HDB00:1   (hso-hana-vm-s1-0)
 * Demote  rsc_SAPHanaCon_HSO_HDB00:1   (Master -> Stopped hso-hana-vm-s1-0)
 * Promote rsc_SAPHanaCon_HSO_HDB00:5   (Slave -> Master hso-hana-vm-s2-0)
 * Move    rsc_ip_HSO_HDB00     (Started hso-hana-vm-s1-0 -> hso-hana-vm-s2-0)
 * Move    rsc_nc_HSO_HDB00     (Started hso-hana-vm-s1-0 -> hso-hana-vm-s2-0)
</code></pre>



## <a name="planned-maintenance"></a>Planlı bakım 

Bu planlı bakım için söz konusu olduğunda farklı kullanım örnekleri vardır. Yalnızca işletim sistemi düzeyi ve disk yapılandırması veya bir HANA yükseltme değişiklikleri gibi altyapı bakım ise bir soru, örneğin, olur.
SUSE gibi gelen belgelerde ek bilgiler bulabilirsiniz [burada] [ sles-zero-downtime-paper] veya [başka bir burada][sles-12-for-sap]. Bu belgeler, örnekleri de birincil el ile geçirme.

Altyapı bakım kullanım örneği doğrulamak için yoğun iç testi yapılmadı. Herhangi bir türden birincil geçirmeye ilgili bir sorun önlemek için her zaman bir küme bakım moduna almadan önce birincil geçirmek için karar yapıldı. Bu şekilde önceki durumu hakkında (hangi tarafta birincil ve ikincil hangi tarafta) unutursanız kümeyi gerekli değildir.

Bu konuda, iki farklı durum vardır:

1. Planlı bakım geçerli ikincil. 
   Bu durumda, yalnızca küme bakım moduna almak ve iş ikincil küme etkilemeden gerçekleştirebilirsiniz

2. Planlı bakım geçerli birincil. 
   Bakım sırasında çalışmaya devam etmesine izin vermek için bir yük devretmeye zorlamak gereklidir. Bu yaklaşım ile küme yük devretmesi pacemaker tarafından ve yalnızca SAP HANA HSR düzeyi tetiklemesi gerekir. Pacemaker Kurulum otomatik olarak SAP HANA devralma işlemini tetikler. Ayrıca, küme bakım moduna almadan önce yük devretme gerçekleştirmek gereklidir.

Yordamın geçerli ikincil sitede bakım için aşağıdaki adımları istiyor:

1. Bakım moduna küme
2. İkincil sitede iş gerçekleştirmek 
3. Son küme Bakım modu

Geçerli birincil sitede bakım için daha karmaşık bir yordamdır:

1. El ile bir yük devretmeyi tetiklemek / SAP HANA devralma yoluyla Pacemaker kaynak geçişi (aşağıdaki ayrıntılara bakın)
2. SAP HANA eski birincil sitede Küme kurulumu tarafından kapatıldığında
3. Bakım moduna küme
4. Bakım işi tamamlandıktan sonra eski birincil yeni ikincil site olarak kaydedin.
5. Küme yapılandırmasını Temizle (aşağıdaki ayrıntılara bakın)
6. Son küme Bakım modu


Bir kaynak (örneğin bir yük devretmeye zorlamak) geçiş küme yapılandırması için bir giriş ekler. Bakım modu bitiş önce bu girdilerin temizlenmesini gerekir. Bir örnek aşağıda verilmiştir:

Bir küme yük devretmesi msl kaynak geçerli ikincil ana düğüme geçerek zorlamak ilk adımdır. Aşağıdaki komut, "taşıma kısıtlaması" oluşturulduğunu bir uyarı verir.

<pre><code>
crm resource migrate msl_SAPHanaCon_HSO_HDB00 force

INFO: Move constraint created for msl_SAPHanaCon_HSO_HDB00
</code></pre>


Yük devretme işlemini komutu aracılığıyla denetleme **SAPHanaSR showAttr**. Adanmış shell penceresini açın ve komut ile başlatmak için küme durumunu izlemenize yardımcı olan **watch**:

<pre><code>
watch SAPHanaSR-showAttr
</code></pre>

Çıktı, el ile yük devretme yansıtmalıdır. Önceki ikincil ana düğüm alındı **yükseltilen** (Bu örnekte **hso-hana-vm-s2-0**) ve eski birincil sitenin durduruldu (**lss** değer **1** eski birincil ana düğüm için **hso-hana-vm-s1-0**): 

<pre><code>
Global cib-time                 prim  sec srHook sync_state
------------------------------------------------------------
global Wed Sep 12 07:40:02 2018 HSOS2 -   SFAIL  SFAIL


Sites lpt        lss mns              srr
------------------------------------------
HSOS1 10         1   hso-hana-vm-s1-0 P
HSOS2 1536738002 4   hso-hana-vm-s2-0 P


Hosts            clone_state node_state roles                        score  site
----------------------------------------------------------------------------------
hso-hana-dm                  online
hso-hana-vm-s1-0 UNDEFINED   online     master1::worker:             150    HSOS1
hso-hana-vm-s1-1 DEMOTED     online     slave::worker:               -10000 HSOS1
hso-hana-vm-s1-2 DEMOTED     online     slave::worker:               -10000 HSOS1
hso-hana-vm-s2-0 PROMOTED    online     master1:master:worker:master 150    HSOS2
hso-hana-vm-s2-1 DEMOTED     online     slave:slave:worker:slave     -10000 HSOS2
hso-hana-vm-s2-2 DEMOTED     online     slave:slave:worker:slave     -10000 HSOS2
</code></pre>

Küme yük devretmesi ve SAP HANA devralma sonra kümeyi pacemaker bölümünde açıklandığı gibi bakım moduna yerleştirin.

Komutlar **SAPHanaSR showAttr** veya **crm durumu** kaynak geçiş tarafından oluşturulan kısıtlamaları hakkında herhangi bir şey göstermediği. Bu kısıtlamalar görünür yapmak için bir tam küme kaynak yapılandırması aşağıdaki komutla göstermek için seçenektir:

<pre><code>
crm configure show
</code></pre>

Küme Yapılandırması içinde önceki el ile kaynak geçiş işleminin neden yeni bir konum kısıtlaması bulun. Bir örnek aşağıda verilmiştir (ile başlayan girişi **konumu CLI -**):

<pre><code>
location cli-ban-msl_SAPHanaCon_HSO_HDB00-on-hso-hana-vm-s1-0 msl_SAPHanaCon_HSO_HDB00 role=Started -inf: hso-hana-vm-s1-0
</code></pre>

Ne yazık ki bu tür kısıtlamaları genel küme davranışı üzerinde bir etkisi olabilir. Bu nedenle tüm sistemin geri getiriyor daha önce kaldırmak için zorunludur. İle **unmigrate** konum kısıtlamaları, önce oluşturulan temizlemek mümkün olduğu komutu. Adlandırma biraz kafa karıştırıcı. İçinden geçirilen tekrar kendi özgün VM kaynağı geçirmeyi denemeden anlamına gelmez. Yalnızca konum kısıtlamaları kaldırır ve ayrıca komut çalıştırılırken ilgili bilgileri döndürür:


<pre><code>
crm resource unmigrate msl_SAPHanaCon_HSO_HDB00

INFO: Removed migration constraints for msl_SAPHanaCon_HSO_HDB00
</code></pre>

Bakım işi sonunda pacemaker bölümünde gösterildiği gibi küme bakım modunu durdur.



## <a name="hbreport-to-collect-log-files"></a>Günlük dosyaları toplamak için hb_report

Pacemaker küme sorunlarını analiz etmenize yardımcı olan ve aynı zamanda çalıştırmak için SUSE desteği tarafından istenen olur **hb_report** yardımcı programı. Bunun ne olduğunu analiz izin önemli logfiles toplar. Başlangıç ve bitiş zamanı (Ayrıca bkz: ilk bölümünde hakkında önemli notlar) belirli bir olaya ilişkin olayın gerçekleştiği kullanarak bir örnek çağrısı şu şekildedir:

<pre><code>
hb_report -f "2018/09/13 07:36" -t "2018/09/13 08:00" /tmp/hb_report_log
</code></pre>

Komut sıvı sıçraması sıkıştırılmış günlük dosyaları burada bunu koyun:

<pre><code>
The report is saved in /tmp/hb_report_log.tar.bz2
Report timespan: 09/13/18 07:36:00 - 09/13/18 08:00:00
</code></pre>

Ardından standart aracılığıyla tek tek dosyaları ayıklayın **tar** komutu:

<pre><code>
tar -xvf hb_report_log.tar.bz2
</code></pre>

Ayıklanan dosyaları isteyen tüm günlük dosyalarını bulun. Bunların çoğu, kümedeki her düğüm için ayrı dizinlerde yerleştirilmiştir:

<pre><code>
-rw-r--r-- 1 root root  13655 Sep 13 09:01 analysis.txt
-rw-r--r-- 1 root root  14422 Sep 13 09:01 description.txt
-rw-r--r-- 1 root root      0 Sep 13 09:01 events.txt
-rw-r--r-- 1 root root 275560 Sep 13 09:00 ha-log.txt
-rw-r--r-- 1 root root     26 Sep 13 09:00 ha-log.txt.info
drwxr-xr-x 4 root root   4096 Sep 13 09:01 hso-hana-dm
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s1-0
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s1-1
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s1-2
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s2-0
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s2-1
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s2-2
-rw-r--r-- 1 root root 264726 Sep 13 09:00 journal.log
</code></pre>


Zaman aralığı içinde olduğu geçerli ana düğüm belirtilen **hso-hana-vm-s1-0** sonlandırıldı. İçinde **journal.log** bu olaya ilişkin girişleri bulabilirsiniz:

<pre><code>
2018-09-13T07:38:01+0000 hso-hana-vm-s2-1 su[93494]: (to hsoadm) root on none
2018-09-13T07:38:01+0000 hso-hana-vm-s2-1 su[93494]: pam_unix(su-l:session): session opened for user hsoadm by (uid=0)
2018-09-13T07:38:01+0000 hso-hana-vm-s2-1 systemd[1]: Started Session c44290 of user hsoadm.
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 corosync[28302]:   [TOTEM ] A new membership (10.0.0.13:120996) was formed. Members left: 1
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 corosync[28302]:   [TOTEM ] Failed to receive the leave message. failed: 1
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 attrd[28313]:   notice: Node hso-hana-vm-s1-0 state is now lost
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 attrd[28313]:   notice: Removing all hso-hana-vm-s1-0 attributes for peer loss
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 attrd[28313]:   notice: Purged 1 peer with id=1 and/or uname=hso-hana-vm-s1-0 from the membership cache
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 stonith-ng[28311]:   notice: Node hso-hana-vm-s1-0 state is now lost
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 stonith-ng[28311]:   notice: Purged 1 peer with id=1 and/or uname=hso-hana-vm-s1-0 from the membership cache
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 cib[28310]:   notice: Node hso-hana-vm-s1-0 state is now lost
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 corosync[28302]:   [QUORUM] Members[6]: 7 2 3 4 5 6
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 corosync[28302]:   [MAIN  ] Completed service synchronization, ready to provide service.
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 crmd[28315]:   notice: Node hso-hana-vm-s1-0 state is now lost
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 pacemakerd[28308]:   notice: Node hso-hana-vm-s1-0 state is now lost
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 cib[28310]:   notice: Purged 1 peer with id=1 and/or uname=hso-hana-vm-s1-0 from the membership cache
2018-09-13T07:38:03+0000 hso-hana-vm-s2-1 su[93494]: pam_unix(su-l:session): session closed for user hsoadm
</code></pre>

Yeni birincil ana dönüştü ikincil asıl üzerinde pacemaker günlük dosyası başka bir örnektir. Sonlandırılan Birincil ana düğüm durumu ayarlandı olduğunu gösteren bir alıntı işte **çevrimdışı**.

<pre><code>
Sep 13 07:38:02 [4178] hso-hana-vm-s2-0 stonith-ng:     info: pcmk_cpg_membership:      Node 3 still member of group stonith-ng (peer=hso-hana-vm-s1-2, counter=5.1)
Sep 13 07:38:02 [4178] hso-hana-vm-s2-0 stonith-ng:     info: pcmk_cpg_membership:      Node 4 still member of group stonith-ng (peer=hso-hana-vm-s2-0, counter=5.2)
Sep 13 07:38:02 [4178] hso-hana-vm-s2-0 stonith-ng:     info: pcmk_cpg_membership:      Node 5 still member of group stonith-ng (peer=hso-hana-vm-s2-1, counter=5.3)
Sep 13 07:38:02 [4178] hso-hana-vm-s2-0 stonith-ng:     info: pcmk_cpg_membership:      Node 6 still member of group stonith-ng (peer=hso-hana-vm-s2-2, counter=5.4)
Sep 13 07:38:02 [4178] hso-hana-vm-s2-0 stonith-ng:     info: pcmk_cpg_membership:      Node 7 still member of group stonith-ng (peer=hso-hana-dm, counter=5.5)
Sep 13 07:38:02 [4184] hso-hana-vm-s2-0       crmd:     info: pcmk_cpg_membership:      Node 1 left group crmd (peer=hso-hana-vm-s1-0, counter=5.0)
Sep 13 07:38:02 [4184] hso-hana-vm-s2-0       crmd:     info: crm_update_peer_proc:     pcmk_cpg_membership: Node hso-hana-vm-s1-0[1] - corosync-cpg is now offline
Sep 13 07:38:02 [4184] hso-hana-vm-s2-0       crmd:     info: peer_update_callback:     Client hso-hana-vm-s1-0/peer now has status [offline] (DC=hso-hana-dm, changed=4000000)
Sep 13 07:38:02 [4184] hso-hana-vm-s2-0       crmd:     info: pcmk_cpg_membership:      Node 2 still member of group crmd (peer=hso-hana-vm-s1-1, counter=5.0)
</code></pre>





## <a name="sap-hana-globalini"></a>SAP HANA global.ini


Aşağıda, SAP HANA düğümler arası iletişim ve HSR için farklı ağlarda kullanmanın ana bilgisayar adı çözümlemesi girişleri göstermek için örnek olarak küme site 2 üzerinde SAP HANA global.ini dosyasından forumlarından görürsünüz:

<pre><code>
[communication]
tcp_keepalive_interval = 20
internal_network = 10.0.2/24
listeninterface = .internal
</code></pre>


<pre><code>
[internal_hostname_resolution]
10.0.2.40 = hso-hana-vm-s2-0
10.0.2.42 = hso-hana-vm-s2-2
10.0.2.41 = hso-hana-vm-s2-1
</code></pre>

<pre><code>
[ha_dr_provider_SAPHanaSR]
provider = SAPHanaSR
path = /hana/shared/myHooks
execution_order = 1
</code></pre>

<pre><code>
[system_replication_communication]
listeninterface = .internal

[system_replication_hostname_resolution]
10.0.1.30 = hso-hana-vm-s1-0
10.0.1.31 = hso-hana-vm-s1-1
10.0.1.32 = hso-hana-vm-s1-2
10.0.1.40 = hso-hana-vm-s2-0
10.0.1.41 = hso-hana-vm-s2-1
10.0.1.42 = hso-hana-vm-s2-2
</code></pre>



## <a name="hawk"></a>HAWK

Küme çözümü, ayrıca menüleri ve karşılaştırıldığında grafik Kabuğu düzeyindeki tüm komutları tercih kişiler için iyi bir GUI sağlar bir tarayıcı arabirimi sağlar.
Tarayıcı arabirimi kullanmak için aşağıda gösterilen URL'si ve yerine **\<düğüm\>** gerçek bir SAP HANA düğüm tarafından küme kimlik bilgilerini girin (kullanıcı **hacluster**):

<pre><code>
https://&ltnode&gt:7630
</code></pre>

Aşağıdaki ekran görüntüsünde, küme Panosu gösterilmektedir:


![HAWK küme Panosu](media/hana-vm-scale-out-HA-troubleshooting/hawk-1.png)


İkinci ekran görüntüsünde planlı Bakım bölümünde açıklandığı gibi küme kaynak geçişi neden konum kısıtlamaları örneğini görebilirsiniz:


![HAWK liste kısıtlamaları](media/hana-vm-scale-out-HA-troubleshooting/hawk-2.png)


Başka bir iyi özelliğini yüklemek için olasılıktır bir **hb_report** çıkış (bölümüne bakın **hb_report**) içinde **HAWK** altında **geçmişi** olarak sonraki ekran görüntüsünde gösterilen:

![HAWK karşıya yükleme hb_report çıkış](media/hana-vm-scale-out-HA-troubleshooting/hawk-3.png)

**Geçmişi Gezgini** sonra dahil tüm küme geçişleri aracılığıyla veren **hb_report** çıktı:

![Geçişleri hb_report çıkış içinde HAWK bakma](media/hana-vm-scale-out-HA-troubleshooting/hawk-4.png)

Son ekran üzerinde küme üzerinde bir birincil ana düğüm kilitlenmesi tepki verilmiş gösteren tek bir geçiş Ayrıntılar bölümünde görebilirsiniz (düğüm **hso-hana-vm-s1-0**) ve ikincil düğüme yeni Yöneticisi olarak (artıkyükseltiliyor**hso-hana-vm-s2-0**):

![Tek bir geçiş HAWK bakma](media/hana-vm-scale-out-HA-troubleshooting/hawk-5.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu sorun giderme kılavuzu, SAP HANA için yüksek kullanılabilirlik genişleme yapılandırmasında hakkındadır. Veritabanı yanı sıra bir SAP ortamının içinde başka bir önemli bileşen, SAP NetWeaver yığınıdır. Yüksek kullanılabilirlik hakkında SAP NetWeaver için SUSE Enterprise Linux Server'ı kullanarak Azure sanal makinelerinde İleri okumalıdır [bu] [ sap-nw-ha-guide-sles] makalesi.

