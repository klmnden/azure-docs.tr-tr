---
title: SLES 12 SP3 ile Azure sanal makineler'de SAP HANA 2.0 genişleme HSR Pacemaker Kurulumu sorunlarını giderme | Microsoft Docs
description: Denetleyin ve SAP HANA sistem çoğaltması (HSR) ve Azure sanal makineler üzerinde çalışan SLES üzerinde Pacemaker SLES 12 SP3 göre bir karmaşık SAP HANA genişleme yüksek kullanılabilirlik yapılandırmasında sorun giderme kılavuzu
services: virtual-machines-linux
documentationcenter: ''
author: hermannd
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/24/2018
ms.author: hermannd
ms.openlocfilehash: b794b045efa4be20a63e9996425d69f0212ae0d7
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707250"
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


Bu makalede, Azure sanal makinelerinde (VM'ler) çalışan SAP HANA ölçek genişletme Pacemaker küme yapılandırmanızı denetleyin yardımcı olur. SAP HANA sistem çoğaltması (HSR) ile birlikte Küme kurulumu gerçekleştirilebilir ve SUSE RPM paketini SAPHanaSR genişletme. Tüm testleri üzerinde SUSE SLES 12 SP3 yalnızca yapıldığını. Makalenin bölümleri farklı alanlarını kapsar ve örnek komutlar ve alıntıları yapılandırma dosyalarını içerir. Bu örnekler doğrulayın ve tüm küme Kurulumu denetlemek için bir yöntem öneririz.



## <a name="important-notes"></a>Önemli Notlar

SAP HANA ölçeklendirme SAP HANA sistem çoğaltması ve Pacemaker ile birlikte tüm test SAP HANA 2.0 ile yalnızca yapılmadı. İşletim sistemi sürümü, SAP uygulamaları için SUSE Linux Enterprise Server 12 SP3 oluştu. En son RPM paketi, SUSE, gelen SAPHanaSR genişletme Pacemaker kümesini ayarlamak için kullanıldı.
SUSE yayımlanan bir [ayrıntılı açıklamasını bu performans için iyileştirilmiş Kurulum][sles-hana-scale-out-ha-paper].

SAP HANA ölçeklendirme için desteklenen sanal makine türleri için denetleme [SAP HANA sertifikalı ve Iaas dizin][sap-hana-iaas-list].

SAP HANA ölçeklendirme birlikte birden çok alt ağları ve Vnıc'ler ve HSR ayarlama ile ilgili teknik bir sorun oluştu. Burada Bu sorun giderilmiştir en son düzeltme eklerini SAP HANA 2.0 kullanmak için zorunludur. Aşağıdaki SAP HANA sürümleri desteklenir: 

* Rev2.00.024.04 veya üzeri 
* Rev2.00.032 veya üzeri

SUSE desteğine ihtiyacınız varsa, bunu izleyin [Kılavuzu][suse-pacemaker-support-log-files]. Makalesinde açıklandığı gibi SAP HANA yüksek kullanılabilirlik (HA) küme ilgili tüm bilgileri toplayın. SUSE desteği ayrıntılı analiz için bu bilgileri gerekir.

İç test sırasında Küme kurulumu Azure portalından bir normal normal VM kapatma tarafından kafası karışıyordu. Bu nedenle diğer yöntemleri ile bir küme yük devretmesi test etmenizi öneririz. Bir çekirdek Panik zorlama gibi yöntemleri kullanın veya ağları kapatın veya geçirme **msl** kaynak. Aşağıdaki bölümlerde ayrıntılara bakın. Standart bir kapatma niyetle olur varsayılır. En iyi kasıtlı bir kapatma için bakım örneğidir. Ayrıntılara bakın [planlı Bakım](#planned-maintenance).

Küme bakım modunda ederken Ayrıca iç sınama sırasında Küme kurulumu sonra el ile bir SAP HANA devralma kafası karışıyordu. Küme bakım modunu sonlandırmadan önce bu geri el ile yeniden geçmenizi öneririz. Başka bir küme bakım moduna koymadan önce bir yük devretmeyi tetiklemek için bir seçenektir. Daha fazla bilgi için [planlı Bakım](#planned-maintenance). SUSE belgelerinden kullanarak küme bu şekilde nasıl sıfırlayabilirsiniz açıklar **crm** komutu. Ancak belirtilen bir yaklaşım daha önce iç test sırasında sağlam ve hiç beklenmedik yan etkileri gösterdi.

Kullanırken **crm geçirme** komutu, küme yapılandırmayı Temizle emin olun. Bunu, farkında olmadıklarınız konum kısıtlamaları ekler. Bu kısıtlamalar, küme davranışını etkiler. Daha fazla bilgi [planlı Bakım](#planned-maintenance).



## <a name="test-system-description"></a>Test sistem açıklaması

 SAP HANA genişleme HA doğrulama ve sertifika için bir kurulum kullanıldı. Üç SAP HANA düğümü olan her iki sistem, toplamda: bir ana ve iki çalışan. Aşağıdaki tablo listeleri VM adları ve iç IP adresleri. İzleyen tüm doğrulama örnekler, bu Vm'lere yapıldığını. Komut örnekleri kullanarak bu sanal makine adları ve IP adresleri, komutlar ve bunların çıkışları daha iyi anlayabilir:


| Düğüm türü | VM adı | IP adresi |
| --- | --- | --- |
| 1 sitesinde ana düğümü | hso-hana-vm-s1-0 | 10.0.0.30 |
| Çalışan düğümü 1 sitesinde 1 | hso-hana-vm-s1-1 | 10.0.0.31 |
| 1 sitesinde 2 çalışan düğümü | hso-hana-vm-s1-2 | 10.0.0.32 |
| | | |
| Ana düğüm sitesinde 2 | hso-hana-vm-s2-0 | 10.0.0.40 |
| 1 sitesinde 2 çalışan düğümü | hso-hana-vm-s2-1 | 10.0.0.41 |
| 2\. site 2 çalışan düğümü | hso-hana-vm-s2-2  | 10.0.0.42 |
| | | |
| Temel oluşturucu düğüm | hso hana dm | 10.0.0.13 |
| SBD aygıt sunucusu | hso hana sbd | 10.0.0.19 |
| | | |
| NFS Sunucusu 1 | hso-nfs-vm-0 | 10.0.0.15 |
| NFS sunucusu 2 | hso-nfs-vm-1 | 10.0.0.14 |



## <a name="multiple-subnets-and-vnics"></a>Birden çok alt ağları ve Vnıc'ler

SAP HANA ağ önerileri üç alt ağ içinde bir Azure sanal ağı oluşturulur. Azure üzerinde SAP HANA ölçeği genişletilmiş paylaşılmayan modunda yüklü olması gerekir. Her düğüm için yerel disk birimlerini kullanır anlamına **/hana/veri** ve **/hana/günlük**. Düğümler yalnızca yerel diskte birimler kullandığından, depolama için ayrı bir alt ağı tanımlar gerekli değildir:

- SAP HANA düğümler arası iletişim için 10.0.2.0/24
- SAP HANA sistem çoğaltması (HSR) için 10.0.1.0/24
- diğer her şey için 10.0.0.0/24

Birden çok ağ kullanmaya ilişkin SAP HANA yapılandırma hakkında daha fazla bilgi için bkz. [SAP HANA global.ini](#sap-hana-globalini).

Kümedeki her bir VM alt ağ sayısını için karşılık gelen üç Vnıc'ler sahiptir. [Bir Linux sanal makinesini Azure'da birden çok ağ arabirimi kartları oluşturmak nasıl][azure-linux-multiple-nics] describes a potential routing issue on Azure when deploying a Linux VM. This specific routing article applies only for use of multiple vNICs. The problem is solved by SUSE per default in SLES 12 SP3. For more information, see [Multi-NIC with cloud-netconfig in EC2 and Azure][suse-cloud-netconfig].


SAP HANA birden fazla ağ kullanmak için doğru şekilde yapılandırıldığını doğrulamak için aşağıdaki komutları çalıştırın. İlk işletim sistemi düzeyinde, tüm üç iç IP adreslerini üç alt ağ için etkin olup olmadığını denetleyin. Farklı IP adresi aralıklarını alt ağlarla tanımlanmışsa, komutları uymak zorunda:

<pre><code>
ifconfig | grep "inet addr:10\."
</code></pre>

Aşağıdaki örnek çıktıda, ikinci site 2 çalışan düğümü arasındadır. Üç farklı iç IP adreslerinden eth0 eth1 ve eth2 görebilirsiniz:

<pre><code>
inet addr:10.0.0.42  Bcast:10.0.0.255  Mask:255.255.255.0
inet addr:10.0.1.42  Bcast:10.0.1.255  Mask:255.255.255.0
inet addr:10.0.2.42  Bcast:10.0.2.255  Mask:255.255.255.0
</code></pre>


Ardından, SAP HANA bağlantı noktaları için ad sunucusu ve HSR doğrulayın. SAP HANA, karşılık gelen alt ağlar dinlemesi gereken. SAP HANA örneği sayısı bağlı olarak, komutlar uymak zorunda. Test sistemi için örnek sayısı olan **00**. Hangi bağlantı noktalarının kullanıldığını bulmak için farklı yolu vardır. 

Aşağıdaki SQL deyimini örnek kimliği, örnek numarasını ve diğer bilgileri döndürür:

<pre><code>
select * from "SYS"."M_SYSTEM_OVERVIEW"
</code></pre>

Doğru bağlantı noktası numaralarını bulmak için örneğin, HANA Studio altında bakabilirsiniz **yapılandırma** veya bir SQL deyimi ile:

<pre><code>
select * from M_INIFILE_CONTENTS WHERE KEY LIKE 'listen%'
</code></pre>

SAP HANA gibi SAP yazılım yığını kullanılan her bağlantı noktası bulmak için arama [TCP/IP bağlantı noktaları tüm SAP ürünleri][sap-list-port-numbers].

Örnek numarasını verilen **00** SAP HANA 2.0 test sisteminde ad sunucusu bağlantı noktası numarasıdır **30001**. HSR meta veri iletişimi için bağlantı noktası numarası **40002**. Bir çalışan düğümü için oturum açın ve ardından ana düğümü Hizmetleri denetlemek için bir seçenek var. Bu makale için size 2 sitesinde site 2 ana düğüme bağlanmaya 2 çalışan düğümü teslim.

Ad sunucusu bağlantı noktasını denetleyin:

<pre><code>
nc -vz 10.0.0.40 30001
nc -vz 10.0.1.40 30001
nc -vz 10.0.2.40 30001
</code></pre>

Düğümler arası iletişim alt ağ kullanır kanıtlamak için **10.0.2.0/24**, sonuç aşağıdaki örnek çıktı gibi görünmelidir.
Yalnızca alt ağ bağlantısıyla **10.0.2.0/24** başarılı olması gerekir:

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

HSR iletişim alt ağ kullanır kanıtlamak için **10.0.1.0/24**, sonuç aşağıdaki örnek çıktı gibi görünmelidir.
Yalnızca alt ağ bağlantısıyla **10.0.1.0/24** başarılı olması gerekir:

<pre><code>
nc: connect to 10.0.0.40 port 40002 (tcp) failed: Connection refused
Connection to 10.0.1.40 40002 port [tcp/*] succeeded!
nc: connect to 10.0.2.40 port 40002 (tcp) failed: Connection refused
</code></pre>



## <a name="corosync"></a>Corosync


**Corosync** temel oluşturucu düğüm dahil olmak üzere kümedeki her düğümde doğru olması yapılandırma dosyası vardır. Bir düğüm kümesi birleşimi beklendiği gibi çalışmazsa, oluşturma veya kopyalama **/etc/corosync/corosync.conf** el ile üzerine tüm düğümleri ve hizmetini yeniden başlatın. 

İçeriği **corosync.conf** testten sistem örneğidir.

İlk bölüm **totem**anlatılan şekilde [Küme yükleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#cluster-installation), 11. adım. Değeri yoksayabilirsiniz **mcastaddr**. Yalnızca var olan girdiyi sakla. Girişleri **belirteci** ve **fikir birliğine varılmış** göre ayarlanmalıdır [Microsoft Azure SAP HANA belgeleri][sles-pacemaker-ha-guide].

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

İkinci bölüm **günlüğü**, verilen varsayılanlarından değiştirilen değildi:

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

Üçüncü bölümü gösterir **düğüm listesine**. Kümenin tüm düğümleri ile göstermek zorunda kendi **nodeId**:

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

Son bölümdeki **çekirdek**, değeri ayarlamak önemlidir **expected_votes** doğru. Temel oluşturucu düğüm dahil olmak üzere bir düğüm olmalıdır. Ve değeri **two_node** olmak zorundadır **0**. Giriş tamamen kaldırmayın. Değere ayarlamanız yeterlidir **0**.

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

Azure VM'de bir SBD cihazı ayarlama konusunda açıklanan [SBD çitlemek](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#sbd-fencing).

İlk olarak, kümedeki her düğüm için ACL girişleri varsa SBD sunucusunda VM kontrol edin. VM SBD sunucuda aşağıdaki komutu çalıştırın:


<pre><code>
targetcli ls
</code></pre>


Test sisteminde, komut çıktısı aşağıdaki örneğe benzer görünür. ACL adları gibi **iqn.2006-04.hso-db-0.local:hso-db-0** sanal makinelere karşılık gelen Başlatıcı adları olarak girilmelidir. Her VM'nin farklı bir tane gerekir.

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

Ardından, tüm sanal makineler Başlatıcı adları farklı ve daha önce gösterilen girişlere karşılık gelen denetleyin. Çalışan düğümü 1 sitesinde 1'den bu örneği verilmiştir:

<pre><code>
cat /etc/iscsi/initiatorname.iscsi
</code></pre>

Çıktı aşağıdaki örneğe benzer görünür:

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

Ardından, doğrulayın **bulma** düzgün şekilde çalışır. Aşağıdaki komut, VM SBD sunucusunun IP adresini kullanarak her küme düğümünde çalıştırın:

<pre><code>
iscsiadm -m discovery --type=st --portal=10.0.0.19:3260
</code></pre>

Çıktı aşağıdaki örneğe benzer görünmelidir:

<pre><code>
10.0.0.19:3260,1 iqn.2006-04.dbhso.local:dbhso
</code></pre>

Sonraki kavram noktayı düğüm SDB cihaz görür doğrulamaktır. Bu, temel oluşturucu düğüm dahil olmak üzere her bir düğümde kontrol edin:

<pre><code>
lsscsi | grep dbhso
</code></pre>

Çıktı aşağıdaki örneğe benzer görünmelidir. Ancak, adları farklı olabilir. VM yeniden başlatıldıktan sonra cihaz adını da değiştirebilirsiniz:

<pre><code>
[6:0:0:0]    disk    LIO-ORG  sbddbhso         4.0   /dev/sdm
</code></pre>

Sistem durumu bağlı olarak, bu bazen sorunları çözmek için iSCSI hizmetleri yeniden başlatmak için yardımcı olur. Sonra aşağıdaki komutları çalıştırın:

<pre><code>
systemctl restart iscsi
systemctl restart iscsid
</code></pre>


Herhangi bir düğümünden tüm düğümleri olup olmadığını denetleyebilir **Temizle**. Belirli bir düğümde doğru cihaz adını kullandığınızdan emin olun:

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


Başka bir SBD denetleyin **döküm** seçeneği **sbd** komutu. Bu örnek komut ve temel oluşturucu düğüm çıkışı, cihaz adı neydi **sdd**değil **sdm**:

<pre><code>
sbd -d /dev/sdd dump
</code></pre>

Çıktı, cihaz adı dışında aynı tüm düğümlerde görünmesi gerekir:

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

Bir daha fazla onay SBD için başka bir düğüme bir ileti göndermek için olasılıktır. 2\. site 2 çalışan düğümü bir ileti göndermek için sitesinde 2 çalışan düğümü 1 üzerinde aşağıdaki komutu çalıştırın:

<pre><code>
sbd -d /dev/sdm message hso-hana-vm-s2-2 test
</code></pre>

Hedef VM yan **hso-hana-vm-s2-2** Bu örnekte, dosyasında şu girişi bulun **/var/log/messages**:

<pre><code>
/dev/disk/by-id/scsi-36001405e614138d4ec64da09e91aea68:   notice: servant: Received command test from hso-hana-vm-s2-1 on disk /dev/disk/by-id/scsi-36001405e614138d4ec64da09e91aea68
</code></pre>

Kontrol girişleri **/etc/sysconfig/sbd** açıklamasında karşılık [SLES azure'daki SUSE Linux Enterprise Server üzerinde Pacemaker ayarlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#sbd-fencing). Ayarı başlangıç doğrulayın **/etc/iscsi/iscsid.conf** otomatik olarak ayarlanır.

Aşağıdaki önemli girişler **/etc/sysconfig/sbd**. Uyum **kimliği** gerekirse değeri:

<pre><code>
SBD_DEVICE="/dev/disk/by-id/scsi-36001405e614138d4ec64da09e91aea68;"
SBD_PACEMAKER=yes
SBD_STARTMODE=always
SBD_WATCHDOG=yes
</code></pre>


Başlatma ayarı iade **/etc/iscsi/iscsid.conf**. Gerekli ayarı ile aşağıdakileri gerçekleşen **iscsiadm** belgelerinde açıklanan komutu. Doğrulayın ve bunu el ile uyum **VI** farklı olması durumunda.

Bu komut, başlangıç davranışını ayarlar:

<pre><code>
iscsiadm -m node --op=update --name=node.startup --value=automatic
</code></pre>

Bu girdiye olun **/etc/iscsi/iscsid.conf**:

<pre><code>
node.startup = automatic
</code></pre>

Sınama ve doğrulama, bir sanal makine yeniden başlatıldıktan sonra sırasında SBD cihaz artık bazı durumlarda görünür değildi. Başlatma ayarı ve hangi YaST2 gösterdi arasında bir tutarsızlık vardı. Ayarlarını denetlemek için şu adımları uygulayın:

1. YaST2 başlatın.
2. Seçin **Ağ Hizmetleri** sol tarafındaki.
3. Aşağı kaydırın için sağ taraftaki **iSCSI başlatıcısı** ve bu seçeneği belirleyin.
4. Altında sonraki ekranda **hizmet** sekmesi, düğüm için benzersiz bir başlatıcı ad görürsünüz.
5. Başlatıcı adı emin **hizmetini** değeri ayarı **olduğunda önyükleme**.
6. Yüklü değilse, ayarlayabilirsiniz **olduğunda önyükleme** yerine **el ile**.
7. Ardından, üst sekmeye geçin **bağlı hedefleri**.
8. Üzerinde **bağlı hedefleri** ekran, bu örnek gibi SBD cihaz için bir giriş görmeniz gerekir: **10.0.0.19:3260 iqn.2006-04.dbhso.local:dbhso**.
9. Kontrol **başlatma** değeri ayarı **önyüklemede**.
10. Aksi takdirde, seçin **Düzenle** ve değiştirin.
11. Değişiklikleri kaydetmek ve YaST2 çıkın.



## <a name="pacemaker"></a>Pacemaker

Her şeyin doğru şekilde ayarlandıktan sonra her düğüm üzerinde Pacemaker hizmet durumunu denetlemek için aşağıdaki komutu çalıştırabilirsiniz:

<pre><code>
systemctl status pacemaker
</code></pre>

Çıkış en üstüne aşağıdaki örneğe benzer görünmelidir. Önemli olduğu, sonra durum **etkin** olarak gösterilen **yüklenen** ve **etkin (çalışan)** . Sonraki durum **Loaded** olarak gösterilmelidir **etkin**.

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

Ayar hala açık değilse **devre dışı**, aşağıdaki komutu çalıştırın:

<pre><code>
systemctl enable pacemaker
</code></pre>

Tüm yapılandırılmış kaynakların Pacemaker görmek için aşağıdaki komutu çalıştırın:

<pre><code>
crm status
</code></pre>

Çıktı aşağıdaki örneğe benzer görünmelidir. Bunu sahip ince, **cln** ve **msl** kaynakları gösterilen çoğunluğu Oluşturucu VM durduruldu **hso hana dm**. Hiçbir temel oluşturucu düğüm üzerinde SAP HANA yüklemesi yok. Bu nedenle **cln** ve **msl** kaynakları durduruldu olarak gösterilir. VM'ler, doğru toplam sayısını gösteren önemlidir **7**. Kümenin parçası olan tüm sanal makineler durumuyla listelenmiş olmalıdır **çevrimiçi**. Geçerli birincil ana düğüm doğru tanınan gerekir. Bu örnekte, bunun **hso-hana-vm-s1-0**:

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

Bakım modu yer Pacemaker önemli bir özelliğidir. Bu modda, hemen küme eylem provoking olmadan değişiklikler yapabilirsiniz. VM'yi yeniden başlatma buna bir örnektir. Tipik bir kullanım örneği, planlı işletim sistemi ya da Azure altyapı bakım olacaktır. Bkz: [planlı Bakım](#planned-maintenance). Pacemaker bakım moduna almak için aşağıdaki komutu kullanın:

<pre><code>
crm configure property maintenance-mode=true
</code></pre>

İade ile **crm durumu**, tüm kaynaklar olarak işaretlenmiş çıktıda fark **yönetilmeyen**. Bu durumda, başlatma veya durdurma SAP HANA gibi herhangi bir değişiklik kümesi tepki vermezse.
Aşağıdaki örnek çıktısı gösterir **crm durumu** küme bakım modundayken komutu:

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


Bu komut örnek küme bakım modunu sonlandırmak nasıl gösterir:

<pre><code>
crm configure property maintenance-mode=false
</code></pre>


Başka bir **crm** komut bunu düzenleyebilmek için bir düzenleyicisine tam küme yapılandırması alır. Değişiklikleri kaydettikten sonra küme uygun eylemleri başlar:

<pre><code>
crm configure edit
</code></pre>

Tam küme yapılandırmasını aramak için kullanın **crm show** seçeneği:

<pre><code>
crm configure show
</code></pre>



Küme kaynaklarının hatadan sonra **crm durumu** komut listesi gösterilir **başarısız Eylemler**. Bu çıktı aşağıdaki örneğe bakın:


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

Hatasından sonra küme temizleme yapmak gereklidir. Kullanın **crm** komutunu tekrar ve komut seçeneği **Temizleme** bunlardan kurtulmak için eylem girişleri başarısız oldu. Karşılık gelen küme kaynağı şu şekilde adlandırın:

<pre><code>
crm resource cleanup rsc_SAPHanaCon_HSO_HDB00
</code></pre>

Komutu aşağıdaki örnek gibi bir çıktı döndürülmesi gerekir:

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



## <a name="failover-or-takeover"></a>Yük devretme veya devralma

Bölümünde açıklandığı gibi [önemli notlar](#important-notes), küme yük devretmesi veya SAP HANA HSR devralma işlemini test etmek için standart normal şekilde kapatılmasını kullanmamalısınız. Bunun yerine, bir çekirdek Panik tetiklemek, kaynak geçişi zorla veya büyük olasılıkla bir sanal makinenin işletim sistemi düzeyinde tüm ağlar kapatın öneririz. Bir başka yöntem **crm \<düğüm\> bekleme** komutu. Bkz: [SUSE belge][sles-12-ha-paper]. 

Aşağıdaki üç örnek komutlar bir küme yük devretmesi zorlayabilirsiniz:

<pre><code>
echo c &gt /proc/sysrq-trigger

crm resource migrate msl_SAPHanaCon_HSO_HDB00 hso-hana-vm-s2-0 force

wicked ifdown eth0
wicked ifdown eth1
wicked ifdown eth2
......
wicked ifdown eth&ltn&gt
</code></pre>

Bölümünde anlatıldığı gibi [planlı Bakım](#planned-maintenance), küme etkinlikleri izlemek için en iyi yolu çalıştırmaktır **SAPHanaSR showAttr** ile **watch** komutu:

<pre><code>
watch SAPHanaSR-showAttr
</code></pre>

Ayrıca bir SAP Python betiğini yakında SAP HANA landscape durum bakmak yardımcı olur. Küme kurulumu için bu durum değeri arıyor. Bir çalışan düğümü hataları dikkate almamız olduğunda açık hale gelir. Bir çalışan düğümü kalırsa, SAP HANA hemen tüm genişleme sistem durumu için bir hata döndürmez. 

Gereksiz yük devretme işlemleri önlemek için bazı deneme vardır. Küme durumu yalnızca değişiklikleri varsa tepki verdiğini **Tamam**, dönüş değeri **4**, **hata**, dönüş değeri **1**. Doğru Bu nedenle, çıkışı **SAPHanaSR showAttr** durumuna sahip bir VM gösterir **çevrimdışı**. Ancak, birincil ve ikincil henüz geçmek için etkinlik yok. SAP HANA bir hata döndürmez sürece hiçbir küme etkinlik tetiklenen.

Kullanıcı olarak SAP HANA yatay sistem durumunu izleyebilir  **\<HANA SID\>adm** gibi SAP Python betiğini çağırarak. Yolun uyum gerekebilir:

<pre><code>
watch python /hana/shared/HSO/exe/linuxx86_64/HDB_2.00.032.00.1533114046_eeaf4723ec52ed3935ae0dc9769c9411ed73fec5/python_support/landscapeHostConfiguration.py
</code></pre>

Bu komutun çıktısı, aşağıdaki örneğe benzer görünmelidir. **Konak durumu** sütun ve **genel ana bilgisayar durumu** hem de önemlidir. Ek sütunları içeren gerçek çıkış geniştir.
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


Geçerli Küme etkinlikleri denetlemek için başka bir komut yoktur. Birincil sitenin ana düğüm sonlandırıldı sonra aşağıdaki komutu ve çıkış kuyruğunu görürsünüz. Geçiş eylemleri gibi listesini görebilirsiniz **yükseltme** eski ikincil ana düğümü **hso-hana-vm-s2-0**, yeni birincil anahtarı olarak. Her şeyi bir sakınca yoktur ve tüm etkinlikleri tamamlanmış olan, bu **geçiş özeti** listesine sahip boş olmalıdır.

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

Bu planlı bakım için söz konusu olduğunda farklı kullanım örnekleri vardır. Bir soru, yalnızca işletim sistemi düzeyi ve disk yapılandırması veya bir HANA yükseltme değişiklikleri gibi altyapı bakım olup olmadığını ' dir.
SUSE gibi gelen belgelerde ek bilgiler bulabilirsiniz [doğrultusunda sıfır kapalı kalma süresi][sles-zero-downtime-paper] or [SAP HANA SR Performance Optimized Scenario][sles-12-for-sap]. Bu belgeleri nasıl birincil el ile geçirmeyi gösteren örnekler de içerir.

Altyapı bakım kullanım örneği doğrulamak için yoğun iç testi yapılmadı. Birincil geçirmeye ilgili sorunlardan kaçınmak için her zaman bir küme bakım moduna almadan önce birincil geçirmek verdik. Bu şekilde, önceki durumu hakkında unutursanız küme yapmak ise gerekli değildir: hangi tarafta birincil ve ikincil olduğu.

Bu konuda, iki farklı durum vardır:

- **Planlı bakım geçerli ikincil**. Bu durumda, yalnızca küme bakım moduna almak ve iş ikincil küme etkilemeden gerçekleştirebilirsiniz.

- **Planlı bakım geçerli birincil**. Kullanıcıların bakım sırasında çalışmaya devam edebilmesi için bir yük devretme zorlamanız gerekir. Bu yaklaşımda, küme yük devretmesi Pacemaker tarafından ve yalnızca SAP HANA HSR düzeyi tetiklemesi gerekir. Pacemaker Kurulum otomatik olarak SAP HANA devralma işlemini tetikler. Ayrıca, küme, bakım moduna önce yük devretme gerçekleştirmek gerekir.

Geçerli ikincil sitede bakım için yordam şu şekildedir:

1. Küme, bakım moduna almak.
2. İkincil sitede iş gerçekleştirirsiniz. 
3. Küme Bakım modu bitiş olayı.

Geçerli birincil sitede bakım için daha karmaşık bir yordamdır:

1. Bir yük devretme veya SAP HANA devralma Pacemaker kaynak geçişi aracılığıyla el ile tetiklersiniz. Aşağıdaki ayrıntılara bakın.
2. Eski birincil site üzerinde SAP HANA tarafından kümesi Kurulumu kapat.
3. Küme, bakım moduna almak.
4. Bakım işi tamamlandıktan sonra eski birincil yeni ikincil site olarak kaydedin.
5. Küme yapılandırmasını Temizle. Aşağıdaki ayrıntılara bakın.
6. Küme Bakım modu bitiş olayı.


Bir kaynak taşıma küme yapılandırması için bir giriş ekler. Bir örnek, bir yük devretme işlemini zorlayarak. Bakım modunu sonlandırmak önce bu girdilerin temizlenmesini gerekir. Aşağıdaki örneğe bakın.

İlk olarak bir küme yük devretmesi geçiş yaparak zorla **msl** geçerli ikincil ana düğümünün kaynağı. Bu komut bir uyarı verir, bir **kısıtlaması taşıma** oluşturuldu:

<pre><code>
crm resource migrate msl_SAPHanaCon_HSO_HDB00 force

INFO: Move constraint created for msl_SAPHanaCon_HSO_HDB00
</code></pre>


Yük devretme işlemini komutu aracılığıyla denetleme **SAPHanaSR showAttr**. Küme durumunu izlemek için bir adanmış shell penceresini açın ve komutla başlatın **watch**:

<pre><code>
watch SAPHanaSR-showAttr
</code></pre>

Çıktı, el ile yük devretme göstermesi gerekir. Önceki ikincil ana düğüm alındı **yükseltilen**, bu örnekte **hso-hana-vm-s2-0**. Eski birincil site durdurulmuş, **lss** değer **1** eski birincil ana düğüm için **hso-hana-vm-s1-0**: 

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

Küme yük devretmesi ve SAP HANA devralma sonra kümeyi bakım açıklandığı moduna [Pacemaker](#pacemaker).

Komutlar **SAPHanaSR showAttr** ve **crm durumu** kaynak geçiş tarafından oluşturulan kısıtlamaları hakkında herhangi bir şey göstermediği. Bu kısıtlamalar görünür yapmak için bir tam küme kaynak yapılandırması aşağıdaki komutla göstermek için seçenektir:

<pre><code>
crm configure show
</code></pre>

Küme Yapılandırması içinde önceki el ile kaynak geçiş işleminin neden yeni bir konum kısıtlaması bulun. Bu örnek girişi ile başlayan **konumu CLI -** :

<pre><code>
location cli-ban-msl_SAPHanaCon_HSO_HDB00-on-hso-hana-vm-s1-0 msl_SAPHanaCon_HSO_HDB00 role=Started -inf: hso-hana-vm-s1-0
</code></pre>

Ne yazık ki, bu gibi sınırlamalar genel küme davranışını etkileyebilir. Bu nedenle bunları yeniden yedekleme sisteminin tamamını sunmadan önce kaldırmak için zorunludur. İle **unmigrate** komutunu önce oluşturulan konum kısıtlamaları temizlemek olabilir. Adlandırma biraz kafa karıştırıcı. Kaynak içinden geçirilen geri özgün VM geçişi dener. Yalnızca konum kısıtlamaları kaldırır ve komutu çalıştırdığınızda da ilgili bilgileri döndürür:


<pre><code>
crm resource unmigrate msl_SAPHanaCon_HSO_HDB00

INFO: Removed migration constraints for msl_SAPHanaCon_HSO_HDB00
</code></pre>

Bakım iş sonunda küme bakım modunu gösterildiği durdurmanız [Pacemaker](#pacemaker).



## <a name="hbreport-to-collect-log-files"></a>Günlük dosyaları toplamak için hb_report

Pacemaker küme sorunlarını analiz etmenize yardımcı olan ve aynı zamanda çalıştırmak için SUSE desteği tarafından istenen olur **hb_report** yardımcı programı. Bunun ne olduğunu çözümlemek için gerek duyduğunuz tüm önemli günlük dosyalarını toplar. Bu örnek çağrı, belirli bir olaya ilişkin olayın gerçekleştiği başlangıç ve bitiş saatini kullanır. Ayrıca bkz: [önemli notlar](#important-notes):

<pre><code>
hb_report -f "2018/09/13 07:36" -t "2018/09/13 08:00" /tmp/hb_report_log
</code></pre>

Komutu, burada, sıkıştırılmış günlük dosyaları yerleştirdiğiniz bildirir:

<pre><code>
The report is saved in /tmp/hb_report_log.tar.bz2
Report timespan: 09/13/18 07:36:00 - 09/13/18 08:00:00
</code></pre>

Ardından standart aracılığıyla tek tek dosyaları ayıklayın **tar** komutu:

<pre><code>
tar -xvf hb_report_log.tar.bz2
</code></pre>

Ayıklanan dosyaları baktığınızda, tüm günlük dosyalarını bulun. Bunların çoğu, kümedeki her düğüm için ayrı dizinlerde yerleştirilmiştir:

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


Belirtilmiş olan zaman aralığında geçerli ana düğüm **hso-hana-vm-s1-0** sonlandırıldı. Bu olay ile ilgili girdileri bulabilirsiniz **journal.log**:

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

Yeni birincil ana dönüştü ikincil asıl üzerinde Pacemaker günlük dosyası başka bir örnektir. Sonlandırılan Birincil ana düğüm durumu olarak ayarlandı, bu alıntı gösterir **çevrimdışı**:

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


SAP HANA'dan aşağıdaki forumlarından olan **global.ini** dosyayı küme sitesinde 2. Bu örnek, ana bilgisayar adı için SAP HANA düğümler arası iletişim ve HSR farklı ağlarda kullanmak için çözünürlük girişlerini gösterir:

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

Küme çözümü, komutların Kabuk düzeyine sahip olmaya menüleri ve grafik tercih eden kullanıcılar için bir GUI sağlar bir tarayıcı arabirimi sağlar.
Tarayıcı arabirimi kullanmak üzere değiştirin **\<düğüm\>** gerçek bir SAP HANA düğümü aşağıdaki URL'de bulunan. Ardından, küme kimlik bilgilerini girin (kullanıcı **küme**):

<pre><code>
https://&ltnode&gt:7630
</code></pre>

Bu ekran görüntüsünde, küme Panosu gösterilmektedir:


![HAWK küme Panosu](media/hana-vm-scale-out-HA-troubleshooting/hawk-1.png)


Bu örnek, bir küme kaynak geçişi tarafından içinde anlatıldığı gibi neden konum kısıtlamaları gösterir [planlı Bakım](#planned-maintenance):


![HAWK liste kısıtlamaları](media/hana-vm-scale-out-HA-troubleshooting/hawk-2.png)


Da karşıya yükleyebilirsiniz **hb_report** Hawk çıkış **geçmişi**aşağıdaki şekilde gösterilmiştir. Günlük dosyaları toplamak için hb_report bakın: 

![HAWK karşıya yükleme hb_report çıkış](media/hana-vm-scale-out-HA-troubleshooting/hawk-3.png)

İle **geçmişi Gezgini**, ardından dahil tüm küme geçişleri üzerinden gidebilirsiniz **hb_report** çıktı:

![Hb_report çıktıda HAWK geçişleri](media/hana-vm-scale-out-HA-troubleshooting/hawk-4.png)

Son bu ekran görüntüsünde gösterilmiştir **ayrıntıları** tek bir geçiş bölümü. Kümenin birincil ana düğüm çökmeyle tepki verilmiş düğüm **hso-hana-vm-s1-0**. İkincil düğüm olarak yeni ana düzeye yükseltilirken artık olan **hso-hana-vm-s2-0**:

![HAWK tek geçiş](media/hana-vm-scale-out-HA-troubleshooting/hawk-5.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu sorun giderme kılavuzu, bir genişleme yapılandırmasında SAP HANA için yüksek kullanılabilirlik açıklar. Veritabanı ek olarak, SAP NetWeaver yığını bir SAP ortamının içinde başka bir önemli bileşenlerden biridir. Hakkında bilgi edinin [SUSE Enterprise Linux Server kullanan Azure sanal makinelerinde SAP NetWeaver için yüksek kullanılabilirlik][sap-nw-ha-guide-sles].

