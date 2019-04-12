---
title: Azure sanal makinelerinde (VM'ler) IBM Db2 HADR ' ayarlama | Microsoft Docs
description: IBM Db2 LUW yüksek kullanılabilirliğini Azure sanal makinelerinde (VM) oluşturun.
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: SAP
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/10/2019
ms.author: juergent
ms.openlocfilehash: d47c64fb8b148e39eab09a33c3241ac7dd3ea537
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59502136"
---
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[1612105]:https://launchpad.support.sap.com/#/notes/1612105

[sles-for-sap-bp]:https://www.suse.com/documentation/sles-for-sap-12/
[db2-hadr-11.1]:https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.admin.ha.doc/doc/c0011267.html
[db2-hadr-10.5]:https://www.ibm.com/support/knowledgecenter/en/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0011267.html
[dbms-db2]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_ibm
[sles-pacemaker]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker
[sap-instfind]:https://help.sap.com/viewer/9e41ead9f54e44c1ae1a1094b0f80712/ALL/en-US/576f5c1808de4d1abecbd6e503c9ba42.html
[nfs-ha]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs
[sles-ha-guide]:https://www.suse.com/releasenotes/x86_64/SLE-HA/12-SP3/
[ascs-ha]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md
[azr-sap-plancheck]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist



# <a name="high-availability-of-ibm-db2-luw-on-azure-vms-on-suse-linux-enterprise-server-with-pacemaker"></a>IBM Db2 LUW üzerinde Pacemaker SUSE Linux Enterprise Server Azure Vm'leri üzerinde yüksek kullanılabilirliği

IBM Db2 LUW (Linux, Unix ve Windows) içinde [HADR yapılandırma](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0011267.html) birincil veritabanı örneği çalıştıran bir düğümü ve bir ikincil veritabanı örneğini çalıştıran en az bir düğüm oluşur. Değişiklikler birincil veritabanı örneğine ikincil veritabanı örneği için zaman uyumlu veya zaman uyumsuz olarak yapılandırmanızı bağımlı çoğaltılır. 

Bu makalede, dağıtın ve sanal makineleri yapılandırma, küme Framework'ü yüklemek ve yüklemek ve IBM Db2 LUW HADR yapılandırmasında yapılandırmak açıklar. Makaleyi yükleyin ve IBM Db2 LUW HADR veya SAP yazılım yükleme yapılandırma açıklayacak değil. SAP ve IBM'den yükleme kılavuzlarını başvurular, bu görevleri elde etmek için sağlanır. Azure ortamınıza özgü olan bölümleri biridir. 

Desteklenen IBM Db2 sürümleri 10.5 ve SAP notu #'ta açıklandığı gibi daha yüksek[1928533].

Aşağıdaki SAP notları ve yüklemenin yaklaşan ilk önce belgeleri okuyun:

| SAP notuna göz atın | Açıklama |
| --- | --- |
| [1928533] | Azure'da SAP uygulamaları: Desteklenen Ürünler ve Azure VM türleri |
| [2015553] | Microsoft Azure üzerinde SAP: Destek önkoşulları |
| [2178632] | Microsoft Azure üzerinde SAP için ölçümleri izleme anahtarı |
| [2191498] | Azure ile Linux üzerinde SAP: Gelişmiş izleme |
| [2243692] | Linux üzerinde Microsoft Azure (Iaas) sanal makine: SAP lisans sorunları |
| [1984787] |SUSE LINUX Enterprise Server 12: Yükleme notları |
| [1999351] |SAP için Gelişmiş Azure izleme sorunlarını giderme |
| [2233094] |DB6: Linux, UNIX ve Windows - ek bilgi için IBM Db2 kullanarak azure'da SAP uygulamaları |
| [1612105] |DB6: Db2 yüksek kullanılabilirlik, olağanüstü durum kurtarma (HADR) hakkında SSS |


| Belgeler | 
| --- |
| [SAP topluluk WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) Linux için sahip tüm gerekli SAP notları |
| [Azure sanal makineleri planlama ve uygulama için Linux üzerinde SAP] [ planning-guide] Kılavuzu |
| [Azure sanal makineler dağıtım için Linux'ta SAP] [ deployment-guide] (Bu makale) |
| [Linux'ta SAP için Azure sanal makineleri DBMS dağıtım] [ dbms-guide] Kılavuzu |
| [SAP iş yüküne Azure planlama ve dağıtım denetim listesi][azr-sap-plancheck] |
| [SUSE Linux Enterprise Server SAP uygulamaları 12 SP3 en iyi uygulamalar kılavuzları][sles-for-sap-bp] |
| [SUSE Linux Enterprise yüksek kullanılabilirlik uzantısı 12 SP3][sles-ha-guide] |
| [SAP iş yükü için IBM Db2 Azure sanal makineleri DBMS dağıtım][dbms-db2] |
| [IBM Db2 yüksek kullanılabilirlik olağanüstü durum kurtarma 11.1][db2-hadr-11.1] |
| [IBM Db2 yüksek kullanılabilirlik olağanüstü durum kurtarma R 10.5][db2-hadr-10.5] |

## <a name="overview"></a>Genel Bakış
Yüksek kullanılabilirlik elde etmek için IBM Db2 LUW HADR ile en az iki Azure sanal makinelerinde, dağıtılan, yüklü bir [Azure kullanılabilirlik kümesine](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) veya çapraz [Azure kullanılabilirlik alanları](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ha-availability-zones). Aşağıdaki grafik, iki veritabanı Server Azure Vm'leri bir kurulum gösterilmektedir. Hem veritabanı sunucusuna Azure Vm'leri bağlı kendi depolama alanına sahip ve çalışıyor olduğundan. HADR içinde birincil örneği rolünü bir Azure sanal makinelerinin bir veritabanı örneği vardır. Tüm istemcilerin bu birincil bir örneğine bağlanır. Veritabanı işlemleri tüm değişiklikleri yerel olarak Db2 işlem günlüğüne kalıcıdır. Hareket günlüğü değişikliklerini yerel olarak kalıcı olarak kayıtları TCP/IP veritabanı örneği için ikinci bir veritabanı sunucusu, bekleme sunucusunun veya bekleme örneği aktarılır. Bekleme örneği aktarılan işlem günlüğü kayıtlarını İleri sarmadır yerel veritabanına güncelleştirir. Bu nedenle bekleme sunucusunun birincil sunucu ile eşitlenmiş durumda tutulur.

HADR yalnızca bir çoğaltma işlevdir. Bu, hiçbir hata algılama ve otomatik bir devralma veya yük devretme özellikleri vardır. Devralma veya aktarım bekleme için bir veritabanı yöneticisi tarafından el ile başlatılmalıdır. Bir otomatik devralma ve hata algılama elde etmek için Linux Pacemaker Kümelemesi özelliğini kullanabilirsiniz. İki veritabanı sunucuları/örnek pacemaker izler. Ne zaman birincil veritabanı server/örneği kilitlenmeler, Pacemaker başlatırken bir **otomatik** HADR devralma yedek sunucu tarafından ve ayrıca birincil sunucuya yeni IP adresinin atandığından emin olmasını sağlar.

![IBM Db2 yüksek kullanılabilirliğine genel bakış](./media/dbms-guide-ha-ibm/ha-db2-hadr-lb.png)

İçin SAP uygulama sunucuları, birincil veritabanına bağlanmak için bir sanal ana bilgisayar adı ve sanal bir IP adresi gerekir. Bir yük devretme durumda, SAP uygulama sunucuları, yeni birincil veritabanı örneğine bağlanır. Azure ortamınızda, bir [Azure Load Balancer](https://microsoft.sharepoint.com/teams/WAG/AzureNetworking/Wiki/Load%20Balancing.aspx) sanal bir IP adresi için IBM Db2 HADR gereken yöntemini kullanmak için gereklidir. 

Tam olarak anlamak, IBM Db2 LUW HADR ve Pacemaker ile yüksek oranda kullanılabilir bir SAP sistemi Kurulum'da nasıl uyduğunu için aşağıdaki resimde, yüksek oranda kullanılabilir bir IBM Db2 veritabanına bağlı bir SAP sistemiyle kurulumu genel bir bakış sunar. Bu makalede, IBM Db2 kapsar ve diğer makaleler için SAP sisteminin diğer bileşenleri ayarlamak nasıl başvurur.

![IBM DB2 HA tam ortama genel bakış](.//media/dbms-guide-ha-ibm/end-2-end-ha.png)


### <a name="high-level-overview-of-steps-needed"></a>Gerekli adımlara üst düzey genel bakış
Bir IBM Db2 yapılandırmasını dağıtmak için aşağıdaki adımları ele alınacak gerekir:

  + Ortamını planlama
  + Vm'leri dağıtma
  + SUSE Linux güncelleştirme ve dosya sistemleri yapılandırma
  + Yükleme ve yapılandırma Pacemaker
  + Yükleme [NFS yüksek oranda kullanılabilir][nfs-ha]
  + Yükleme [ASCS/Ağıranlar ayrı kümede][ascs-ha] 
  + IBM Db2 veritabanı (SWPM) dağıtılmış/yüksek kullanılabilirlik seçeneği ile yükleyin.
  + Yükleme/ikincil veritabanı düğüm ve örnek oluşturma ve HADR yapılandırma
  + HADR çalıştığını onaylayın
  + IBM Db2 denetimine Pacemaker yapılandırmasını Uygula
  + Azure Load Balancer'ı yapılandırma 
  + Yükleme birincil + iletişim uygulama sunucuları
  + SAP uygulama sunucuları için yapılandırma denetimi ve uyum
  + Yük devretme gerçekleştirmek / devralma testleri



## <a name="planning-azure-infrastructure-for-hosting-ibm-db2-luw-with-hadr"></a>IBM Db2 LUW HADR ile barındırmak için Azure Altyapı planlama

Dağıtım yürütmeden önce planlaması aracılığıyla gidin. Db2 azure'da HADR ile yapılandırılmasını dağıtmak için temel oluşturuyor. Anahtar IMB Db2 LUW (SAP ortamının parçası veritabanı) için planlama bir parçası olması gereken öğeler.

| Konu | Kısa açıklama |
| --- | --- |
| Azure kaynak grupları tanımlama | VM, sanal ağ, Azure Load Balancer ve diğer kaynakların nerede dağıttığınız kaynak grupları. Mevcut veya yeni olabilir. |
| Sanal ağ / alt ağ tanımı | Burada Vm'leri IBM Db2 ve Azure Load Balancer için dağıtılacak hazırlanıyoruz. Mevcut veya yeni oluşturulmuş olabilir. |
| IBM Db2 LUW barındıran sanal makineler | VM boyutu, depolama, ağ, IP adresi |
| Sanal ana bilgisayar adı & IBM Db2 veritabanı için sanal IP| Sanal IP/bağlantı SAP uygulama sunucuları için kullanılan konak adı. **DB vırt hostname**, **db vırt IP** |
| Azure çitlemek | Azure çitlemek veya SBD çitlemek (kesinlikle önerilir). Bölme beyin gibi durumlarla karşılaşmamak için yöntemi engellenir |
| SBD VM | SBD sanal makine boyutu, depolama, ağ |
| Azure Load Balancer | Temel kullanımını veya araştırma Db2 veritabanı (önerimizde 62500) için bağlantı noktası (önerilen) standart **araştırma bağlantı noktası** |
| Ad çözümlemesi| Nasıl ad çözümlemesi bir ortamda çalışır. DNS hizmeti önemle tavsiye edilir. Yerel ana bilgisayar dosyası kullanılabilir |
    
Azure'da Linux Pacemaker kullanımı hakkında daha fazla bilgi aşağıdaki makalelerde bulunabilir:

- [SLES azure'daki SUSE Linux Enterprise Server üzerinde Pacemaker ayarlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker)



## <a name="deployment-on-suse-linux"></a>SUSE Linux üzerinde dağıtım

IBM Db2 LUW için kaynak aracıyı SUSE Linux Enterprise Server SAP uygulamaları için dahil edilir. Bu belgede açıklanan kurulumu için SAP uygulamaları için SUSE Linux sunucusu kullanmanız zorunludur. Azure marketi, SAP uygulamaları 12 için yeni bir Azure sanal makineleri dağıtmak için kullanabileceğiniz SUSE Enterprise sunucusunun görüntü içerir. Sanal makine görüntüsünü Azure VM galerisinde seçerken Azure galerileri SUSE tarafından sunulan farklı destek/service modelleri farkında olun. 

### <a name="hosts---dns-updates"></a>Ana - DNS güncelleştirmeleri
Sanal ana bilgisayar adları da dahil olmak üzere tüm ana bilgisayar adları listesini oluşturmak ve ana bilgisayar adı çözümlemesi için uygun IP adresi etkinleştirmek için DNS sunucularınızın güncelleştirin. Durumda, bir DNS sunucusu yok veya DNS girişleri oluşturmanızı ve güncelleştirmek mümkün değildir, bu senaryoda katılan tek tek sanal makinelerin yerel ana bilgisayar dosyaları yararlanarak gerekir. Ana bilgisayar dosyaları girişleri kullanarak söz konusu olduğunda girişleri SAP sistemi ortamında tüm vm'lere uygulanır emin olmak gerekir. Ancak, ideal olarak Azure'a genişletilir, DNS kullanmak için önerilir


### <a name="manual-deployment"></a>El ile dağıtım

Seçilen işletim sistemi için IBM Db2 LUW IBM/SAP tarafından desteklendiğinden emin olun. Azure Vm'leri ve Db2 yayınlar için desteklenen işletim sistemi sürümlerinin listesi SAP notu kullanılabilir [1928533]. Tek tek Db2 sürümleri tarafından işletim sistemi sürümleri listesi SAP ürün kullanılabilirliği matriste kullanılabilir. SLES 12 SP3 en az bu veya daha sonra Azure ilgili performans iyileştirmeleri nedeniyle önerilir SUSE Linux sürümleri.

1. Bir kaynak grubu oluşturma/seçin
2. Bir sanal ağ ve alt ağ oluşturma/seçin
3. Azure kullanılabilirlik kümesi oluşturma veya kullanılabilirlik alanında dağıtın
    + Kullanılabilirlik ayarlama - max güncelleştirme etki alanları için iki ayarlayın
4. 1 sanal makine oluşturun.
    + SAP görüntü, Azure Galerisi'nde bulunan SLES kullanın
    + Adım 3 veya select kullanılabilirlik alanında oluşturulan kümesinde Azure kullanılabilirlik seçin
5.  2 sanal makine oluşturun.
    + SAP görüntü, Azure Galerisi'nde bulunan SLES kullanın
    + Azure kullanılabilirlik kümesi içinde oluşturulan adım 3'ı seçin. veya değil aynı bölge gibi 3. adım - kullanılabilirlik bölgesi seçin.
6. Vm'lere veri diskleri ekleme - öneri makaledeki dosya sistemi kurulumunun denetleyin [SAP iş yükü için IBM Db2 Azure sanal makineleri DBMS dağıtım][dbms-db2]

## <a name="create-the-pacemaker-cluster"></a>Pacemaker kümesi oluşturma
    
Bağlantısındaki [SLES azure'daki SUSE Linux Enterprise Server üzerinde Pacemaker ayarlama][sles-pacemaker] IBM Db2 bu sunucu için temel Pacemaker küme oluşturmak için. 

## <a name="install-ibm-db2-luw-and-sap-environment"></a>IBM Db2 LUW ve SAP ortamı yükleyin

IBM Db2 LUW üzerinde temel bir SAP ortamının yüklemeye başlamadan önce (makalenin başında verilen bağlantılar) gözden geçirin:

+ Azure belgeleri
+ SAP belgeleri
+ IBM belgeleri

Yükleme el SAP NetWeaver tabanlı uygulamalar üzerinde IBM Db2 LUW yükleme hakkında denetleyin.

Portalı kullanarak SAP yardımcı Kılavuzlar bulabilirsiniz [SAP Yükleme Kılavuzu Bulucu][sap-instfind]

Filtreleri ayarladıktan ile kullanılabilir kılavuzları sayısını azaltmak için arama filtreleyebilirsiniz:

+ Yapmak istiyorum: "Yeni bir sistem Yükle"
+ Veritabanı: "IBM Db2 Linux, Unix ve Windows için"
+ SAP Netweaver sürümleri, yığın yapılandırması veya işletim sistemi için ek filtreler.

### <a name="installation-hints-for-setting-up-ibm-db2-luw-with-hadr"></a>Yükleme ipuçlarını IBM Db2 LUW HADR ile ayarlama

Birincil IBM Db2 LUW veritabanı örneği ayarlayın:

- Yüksek kullanılabilirlik veya dağıtılmış seçeneğini kullanın
- SAP ASCS/Ağıranlar ve veritabanı örneğini yükleyin
- Yeni yüklenen veritabanını yedekleyin


> [!IMPORTANT] 
> "Kurulum sırasında veritabanı iletişim bağlantı noktası" yazın. Her iki veritabanı örnekleri için aynı bağlantı noktası numarası olmalıdır

SAP'nin homojen sistem kopyalama yordamını kullanarak bekleme veritabanı sunucusunu ayarlamak için aşağıdaki adımları uygulayın:

  - Sistem Kopyala seçeneğini kullanın - hedef sistemleri - dağıtılmış - veritabanı örneği.
  - Yedekleme bekleme server örneğinde bir yedeklemeyi geri yüklemek için kullanabileceğiniz homojen sistem kopyası kopyalama yöntemi olarak seçin
  - Homojen sistem kopyası için veritabanını geri yüklemek için çıkış adımına geldiğinizde, yükleyici çıkın. Veritabanı birincil ana bilgisayarın bir yedekten geri yükleyin. Birincil veritabanı sunucusunda, tüm sonraki yükleme aşamaları zaten yürütülmüş
- IBM Db2 için HADR ayarlayın

> [!NOTE]
> Yükleme/yapılandırma için Azure ve Pacemaker belirli. SAP yazılım sağlama Yöneticisi aracılığıyla yükleme yordamı sırasında yüksek kullanılabilirlik için IBM Db2 LUW açık bir soru vardır:
>+ IBM Db2 pureScale seçin
>+ "Yükleme IBM Tivoli sistem Otomasyon için Multiplatforms seçin
>+ "Küme yapılandırma dosyaları oluştur" seçeneğini belirlemeyin

> [!NOTE]
>SBD cihaz için Linux Pacemaker kullanırken, Db2 HADR parametrelerini ayarlayın
>+ HADR eş pencere süresi (saniye) (HADR_PEER_WINDOW) = 300  
>+ HADR zaman aşımı değerini (HADR_TIMEOUT) 60 =

> [!NOTE]
>Azure Pacemaker sınır Aracısı'nı kullanma:
>+ HADR eş pencere süresi (saniye) (HADR_PEER_WINDOW) 900 =  
>+ HADR zaman aşımı değerini (HADR_TIMEOUT) 60 =

İlk yük devretme/devralma teste dayanan parametreleri önerilir. Devralma ve yük devretme Bu parametre ayarlarla uygun işlevselliğini test etmek için zorunludur. Tek tek yapılandırmaları değişebilir olduğundan, bu parametreleri ayarlama gerektirebilir. 

> [!IMPORTANT]
> Birincil veritabanı örneği başlatmak önce HADR yapılandırmasında normal başlangıç - ikincil/bekleme veritabanı örneği ile IBM Db2 için özel ve çalışıyor olması gerekir.

Tanıtım amaçlı ve bu belgesinde belgelenen yordamları için SID veritabanıdır **PTR**.

##### <a name="ibm-db2-hadr-check"></a>IBM Db2 HADR Check
HADR yapılandırıldıktan ve durum eş ve bağlı birincil ve hazır bekleyen düğümler üzerinde çalışıyor.

<pre><code>
Execute command as db2&lt;sid&gt; db2pd -hadr -db &lt;SID&gt;

#Primary output:
# Database Member 0 -- Database PTR -- Active -- Up 1 days 01:51:38 -- Date 2019-02-06-15.35.28.505451
# 
#                             <b>HADR_ROLE = PRIMARY
#                           REPLAY_TYPE = PHYSICAL
#                         HADR_SYNCMODE = NEARSYNC
#                            STANDBY_ID = 1
#                         LOG_STREAM_ID = 0
#                            HADR_STATE = PEER
#                            HADR_FLAGS = TCP_PROTOCOL
#                   PRIMARY_MEMBER_HOST = azibmdb02
#                      PRIMARY_INSTANCE = db2ptr
#                        PRIMARY_MEMBER = 0
#                   STANDBY_MEMBER_HOST = azibmdb01
#                      STANDBY_INSTANCE = db2ptr
#                        STANDBY_MEMBER = 0
#                   HADR_CONNECT_STATUS = CONNECTED</b>
#              HADR_CONNECT_STATUS_TIME = 02/05/2019 13:51:47.170561 (1549374707)
#           HEARTBEAT_INTERVAL(seconds) = 15
#                      HEARTBEAT_MISSED = 0
#                    HEARTBEAT_EXPECTED = 6137
#                 HADR_TIMEOUT(seconds) = 60
#         TIME_SINCE_LAST_RECV(seconds) = 13
#              PEER_WAIT_LIMIT(seconds) = 0
#            LOG_HADR_WAIT_CUR(seconds) = 0.000
#     LOG_HADR_WAIT_RECENT_AVG(seconds) = 0.000025
#    LOG_HADR_WAIT_ACCUMULATED(seconds) = 434.595
#                   LOG_HADR_WAIT_COUNT = 223713
# SOCK_SEND_BUF_REQUESTED,ACTUAL(bytes) = 0, 46080
# SOCK_RECV_BUF_REQUESTED,ACTUAL(bytes) = 0, 374400
#             PRIMARY_LOG_FILE,PAGE,POS = S0000280.LOG, 15571, 27902548040
#             STANDBY_LOG_FILE,PAGE,POS = S0000280.LOG, 15571, 27902548040
#                   HADR_LOG_GAP(bytes) = 0
#      STANDBY_REPLAY_LOG_FILE,PAGE,POS = S0000280.LOG, 15571, 27902548040
#        STANDBY_RECV_REPLAY_GAP(bytes) = 0
#                      PRIMARY_LOG_TIME = 02/06/2019 15:34:39.000000 (1549467279)
#                      STANDBY_LOG_TIME = 02/06/2019 15:34:39.000000 (1549467279)
#               STANDBY_REPLAY_LOG_TIME = 02/06/2019 15:34:39.000000 (1549467279)
#          STANDBY_RECV_BUF_SIZE(pages) = 2048
#              STANDBY_RECV_BUF_PERCENT = 0
#            STANDBY_SPOOL_LIMIT(pages) = 0
#                 STANDBY_SPOOL_PERCENT = NULL
#                    STANDBY_ERROR_TIME = NULL
#                  PEER_WINDOW(seconds) = 300
#                       PEER_WINDOW_END = 02/06/2019 15:40:25.000000 (1549467625)
#              READS_ON_STANDBY_ENABLED = N

#Secondary output:
# Database Member 0 -- Database PTR -- Standby -- Up 1 days 01:46:43 -- Date 2019-02-06-15.38.25.644168
# 
#                             <b>HADR_ROLE = STANDBY
#                           REPLAY_TYPE = PHYSICAL
#                         HADR_SYNCMODE = NEARSYNC
#                            STANDBY_ID = 0
#                         LOG_STREAM_ID = 0
#                            HADR_STATE = PEER
#                            HADR_FLAGS = TCP_PROTOCOL
#                   PRIMARY_MEMBER_HOST = azibmdb02
#                      PRIMARY_INSTANCE = db2ptr
#                        PRIMARY_MEMBER = 0
#                   STANDBY_MEMBER_HOST = azibmdb01
#                      STANDBY_INSTANCE = db2ptr
#                        STANDBY_MEMBER = 0
#                   HADR_CONNECT_STATUS = CONNECTED</b>
#              HADR_CONNECT_STATUS_TIME = 02/05/2019 13:51:47.205067 (1549374707)
#           HEARTBEAT_INTERVAL(seconds) = 15
#                      HEARTBEAT_MISSED = 0
#                    HEARTBEAT_EXPECTED = 6186
#                 HADR_TIMEOUT(seconds) = 60
#         TIME_SINCE_LAST_RECV(seconds) = 5
#              PEER_WAIT_LIMIT(seconds) = 0
#            LOG_HADR_WAIT_CUR(seconds) = 0.000
#     LOG_HADR_WAIT_RECENT_AVG(seconds) = 0.000023
#    LOG_HADR_WAIT_ACCUMULATED(seconds) = 434.595
#                   LOG_HADR_WAIT_COUNT = 223725
# SOCK_SEND_BUF_REQUESTED,ACTUAL(bytes) = 0, 46080
# SOCK_RECV_BUF_REQUESTED,ACTUAL(bytes) = 0, 372480
#             PRIMARY_LOG_FILE,PAGE,POS = S0000280.LOG, 15574, 27902562173
#             STANDBY_LOG_FILE,PAGE,POS = S0000280.LOG, 15574, 27902562173
#                   HADR_LOG_GAP(bytes) = 0
#      STANDBY_REPLAY_LOG_FILE,PAGE,POS = S0000280.LOG, 15574, 27902562173
#        STANDBY_RECV_REPLAY_GAP(bytes) = 155
#                      PRIMARY_LOG_TIME = 02/06/2019 15:37:34.000000 (1549467454)
#                      STANDBY_LOG_TIME = 02/06/2019 15:37:34.000000 (1549467454)
#               STANDBY_REPLAY_LOG_TIME = 02/06/2019 15:37:34.000000 (1549467454)
#          STANDBY_RECV_BUF_SIZE(pages) = 2048
#              STANDBY_RECV_BUF_PERCENT = 0
#            STANDBY_SPOOL_LIMIT(pages) = 0
#                 STANDBY_SPOOL_PERCENT = NULL
#                    STANDBY_ERROR_TIME = NULL
#                  PEER_WINDOW(seconds) = 300
#                       PEER_WINDOW_END = 02/06/2019 15:43:19.000000 (1549467799)
#              READS_ON_STANDBY_ENABLED = N
</code></pre>



## <a name="db2-pacemaker-configuration"></a>Db2 Pacemaker yapılandırma

Bir düğümün başarısız olması durumunda otomatik yük devretme Pacemaker kullandığınızda, Db2 örnekleri ve Pacemaker uygun şekilde yapılandırmanız gerekir. Bu bölümde, bu tür bir yapılandırma anlatmaktadır.

Aşağıdaki öğeler ile önek:

- **[A]**  - tüm düğümler için geçerlidir
- **[1]**  - düğüm 1 yalnızca uygulanabilir 
- **[2]**  - yalnızca düğüm 2 için geçerlidir.

<!-- Yast is a fixed term in Linux and not a spelling error -->

**[A]**  Pacemaker yapılandırma önkoşulları:
1. Her iki kullanıcı db2 veritabanı sunucusuyla kapatma\<SID > db2stop ile
2. Db2 için değişiklik Kabuk ortamını\<SID > "/ bin/ksh" - kullanıcıya önerilerek Yast aracını kullanma 


### <a name="pacemaker-configuration"></a>Pacemaker yapılandırması:

**[1]**  IBM Db2 HADR belirli Pacemaker yapılandırma
<pre><code># Put Pacemaker into maintenance mode
sudo crm configure property maintenance-mode=true
</code></pre>

**[1]**  IBM Db2 oluşturma kaynakları
<pre><code># Replace **bold strings** with your instance name db2sid, database SID and virtual IP address/Azure Load Balancer

sudo crm configure primitive rsc_Db2_db2ptr_<b>PTR</b> db2 \
        params instance="<b>db2ptr</b>" dblist="<b>PTR</b>" \
        op start interval="0" timeout="130" \
        op stop interval="0" timeout="120" \
        op promote interval="0" timeout="120" \
        op demote interval="0" timeout="120" \
        op monitor interval="30" timeout="60" \
        op monitor interval="31" role="Master" timeout="60"

# Configure virutal IP - same as Azure Load Balancer IP
sudo crm configure primitive rsc_ip_db2ptr_<b>PTR</b> IPaddr2 \
        op monitor interval="10s" timeout="20s" \
        params ip="<b>10.100.0.10</b>"

# Configure probe port for Azure load Balancer
sudo crm configure primitive rsc_nc_db2ptr_<b>PTR</b> anything \
        params binfile="/usr/bin/nc" cmdline_options="-l -k <b>62500</b>" \
        op monitor timeout="20s" interval="10" depth="0"

sudo crm configure group g_ip_db2ptr_<b>PTR</b> rsc_ip_db2ptr_<b>PTR</b> rsc_nc_db2ptr_<b>PTR</b>

sudo crm configure ms msl_Db2_db2ptr_<b>PTR</b> rsc_Db2_db2ptr_<b>PTR</b> \
        meta target-role="Started" notify="true"

sudo crm configure colocation col_db2_db2ptr_<b>PTR</b> inf: g_ip_db2ptr_<b>PTR</b>:Started msl_Db2_db2ptr_<b>PTR</b>:Master

sudo crm configure order ord_db2_ip_db2ptr_<b>PTR</b> inf: msl_Db2_db2ptr_<b>PTR</b>:promote g_ip_db2ptr_<b>PTR</b>:start

sudo crm configure rsc_defaults resource-stickiness=1000
sudo crm configure rsc_defaults migration-threshold=5000
</code></pre>

**[1]**  Başlat IBM Db2 kaynaklar - Pacemaker bakım modundan yerleştirin
<pre><code># Put Pacemaker out of maintenance-mode - that start IBM Db2
sudo crm configure property maintenance-mode=false</pre></code>

**[1]**  Küme durumunun Tamam olduğunu ve tüm kaynakları başlatıldığından emin olun. Hangi düğümünde kaynaklarını çalıştıran önemli değildir.
<pre><code>sudo crm status</code>

# <a name="2-nodes-configured"></a>yapılandırılmış 2 düğüm
# <a name="5-resources-configured"></a>yapılandırılmış 5 kaynakları

# <a name="online--azibmdb01-azibmdb02-"></a>Çevrimiçi: [azibmdb01 azibmdb02]

# <a name="full-list-of-resources"></a>Kaynakların tam listesi:

#  <a name="stonith-sbd----stonithexternalsbd-started-azibmdb02"></a>stonith-sbd    (stonith:external/sbd): Başlatılan azibmdb02
#  <a name="resource-group-gipdb2ptrptr"></a>Kaynak grubu: g_ip_db2ptr_PTR
#      <a name="rscipdb2ptrptr--ocfheartbeatipaddr2-------started-azibmdb02"></a>rsc_ip_db2ptr_PTR (ocf::heartbeat:IPaddr2):       Başlatılan azibmdb02
#      <a name="rscncdb2ptrptr--ocfheartbeatanything------started-azibmdb02"></a>rsc_nc_db2ptr_PTR (ocf::heartbeat: herhangi bir şey):      Başlatılan azibmdb02
#  <a name="masterslave-set-msldb2db2ptrptr-rscdb2db2ptrptr"></a>Yönetici/bağımlı kümesi: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
#      <a name="masters--azibmdb02-"></a>Masters: [ azibmdb02 ]
#      <a name="slaves--azibmdb01-"></a>Kopyalanmış: [azibmdb01]
</pre>

> [!IMPORTANT]
> Kümelenmiş Db2 örnek Pacemaker araçları yapılmalıdır Pacemaker yönetme. DB2 komutları (gibi db2stop) kullanarak Pacemaker tarafından kaynak hatası olarak algılanır. Bakım, durumunda düğümleri ve kaynakların bakım moduna yerleştirebilir ve Pacemaker izleme kaynaklarını askıya alma ve normal db2 yönetim komutları kullanılabilir.


### <a name="configure-azure-load-balancer"></a>Azure Load Balancer'ı yapılandırma
Kullanılacak önerilen [Azure standart yük dengeleyici SKU](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview).

1. Azure Load Balancer (Azure portalı) aracılığıyla yapılandırın. İlk olarak, ön uç IP havuzu oluşturun:

   1. Azure portalında Azure Load Balancer'ı açın, **ön uç IP havuzu**seçip **Ekle**.
   2. Yeni ön uç IP havuzunun adını girin (örneğin, **Db2 bağlantı**).
   3. Ayarlama **atama** için **statik** ve IP adresini girin **sanal IP** başında tanımlanır.
   4. **Tamam**’ı seçin.
   5. Yeni ön uç IP havuzu oluşturulduktan sonra havuzu IP adresini not edin.

2. Sonraki adım, bir arka uç havuzu oluşturmaktır:

   1. Azure portalında Azure Load Balancer'ı açın, **arka uç havuzları**seçip **Ekle**.
   2. Yeni arka uç havuzunun adını girin (örneğin, **Db2 arka uç**).
   3. Seçin **bir sanal makine ekleme**.
   4. 3. adımda oluşturduğunuz IBM Db2 veritabanı barındırma kullanılabilirlik kümesi/sanal makineleri seçin.
   5. IBM Db2 kümedeki sanal makineleri seçin.
   6. **Tamam**’ı seçin.

3. Üçüncü adım bir durum araştırması oluşturmaktır:

   1. Azure portalında Azure Load Balancer'ı açın, **sistem durumu araştırmaları**seçip **Ekle**.
   2. Yeni bir sistem durumu araştırma adını girin (örneğin, **Db2 hp**).
   3. Seçin **TCP** protokolü ve bağlantı noktası olarak **62500**. Tutun **aralığı** 5 olarak ayarlandıysa değer ve **sağlıksız durum eşiği** değerini 2 olarak ayarlayın.
   4. **Tamam**’ı seçin.

4. Yük Dengeleme kuralları oluşturun:

   1. Azure portalında Azure Load Balancer'ı açın, **Yük Dengeleme kuralları**seçip **Ekle**.
   2. Yeni Yük Dengeleyici kuralı adını girin (örneğin, **Db2 SID**).
   3. Ön uç IP adresi, arka uç havuzu ve durum yoklaması, daha önce oluşturduğunuz seçin (örneğin, **Db2 ön uç**).
   4. Tutmak **Protokolü** kümesine **TCP**, bağlantı noktası girin *veritabanı iletişimi bağlantı noktasını*.
   5. Artırmak **boşta kalma zaman aşımı** 30 dakika.
   6. Emin olun **kayan IP'yi etkinleştirebilirsiniz**.
   7. **Tamam**’ı seçin.


### <a name="make-changes-to-sap-profiles-to-use-virtual-ip-for-connection"></a>Sanal IP bağlantısı için kullanmak üzere SAP profilleri değişiklik
SAP uygulama katmanı tanımlanan ve birincil HADR yapılandırma örneğine bağlanmak Azure Load Balancer için yapılandırılan sanal IP adresi kullanılmalıdır. Aşağıdaki değişiklikler gereklidir.

/sapmnt/\<SID >/profile/varsayılan. PFL
<pre><code>SAPDBHOST = db-virt-hostname
j2ee/dbhost = db-virt-hostname
</code></pre>

/sapmnt/\<SID>/global/db6/db2cli.ini
<pre><code>Hostname=db-virt-hostname
</code></pre>



## <a name="install-primary-and-dialog-application-servers"></a>Birincil yükleme ve iletişim uygulama sunucuları

Yükleme birincil ve Db2 HADR yapılandırma karşı iletişim uygulama sunucuları, kullandığınızda, seçtiğiniz sanal ana bilgisayar adı yapılandırması. 

Db2 HADR yapılandırma oluşturmadan önce yüklemeyi gerçekleştirdiğiniz durumunda, önceki paragrafta ve SAP Java yığınları için aşağıda açıklandığı gibi değişiklikler yapmanız gerekir.

### <a name="abapjava-or-java-stack-systems-jdbc-url-check"></a>ABAP + Java veya Java stack sistemleri JDBC URL'sini denetleyin

İade veya JDBC URL'sini güncelleştirme J2EE Yapılandırma Aracı'nı kullanın. Grafik araç J2EE yapılandırma aracı, sonuç olarak ihtiyacınız **sunucu X** yüklü:
 
1. J2EE örneğinin birincil uygulama sunucusunda oturum açın ve yürütün:
    <pre><code>sudo /usr/sap/*SID*/*Instance*/j2ee/configtool/configtool.sh</code></pre>
2. Sol çerçevede güvenlik deposu seçin.
2. Anahtar jdbc/havuz'a sağ çerçevedeseçin/ <SAPSID> /URL.
2. Sanal ana bilgisayar adı için JDBC URL ana bilgisayar adını değiştirin <pre><code>jdbc:db2://db-virt-hostname:5912/TSP:deferPrepares=0</code></pre>
5. Seçin ekleyin.
5. Değişikliklerinizi kaydetmek için sol üst köşedeki disk simgesini tıklayın.
5. Yapılandırma aracını kapatın.
5. Java örneğini yeniden başlatın.

## <a name="configuration-of-log-archiving-for-hadr-setup"></a>Günlük HADR kurulumu için arşivleme, yapılandırma
HADR kurulumu için arşivleme Db2 günlük yapılandırmak için hem birincil hem de bekleme veritabanının tüm günlük arşiv konumlardan otomatik günlük alma özelliğine sahip yapılandırmanızı öneririz. Hem birincil hem de bekleme veritabanı günlük arşiv dosyaları ya da hangi veritabanı birine örnekleri günlük dosyalarını arşivlemek tüm günlük arşiv konumlarda alamadı olmalıdır. 

Günlük arşivleme, yalnızca birincil veritabanı tarafından gerçekleştirilir. Veritabanı sunucuları HADR rollerini değiştirmek veya bir hata oluşursa, yeni birincil veritabanı günlük Arşivlenmeye sorumludur. Farklı günlük arşiv konumları ayarlarsanız, günlüklerinizi iki kez arşivlenmiş ve yerel veya uzak yakalama söz konusu olduğunda, arşivlenen günlükleri eski birincil sunucudan yeni birincil sunucu etkin günlük konumuna el ile kopyalamanız gerekebilir.

Her iki düğümü günlüklerinin yazılacağı ortak NFS paylaşımını yapılandırma öneririz. NFS yüksek oranda kullanılabilir olması gerekir. 

Profil dizini aktarımı için kullanılan mevcut yüksek oranda kullanılabilir NFS kullanabilirsiniz. Okuma:

- [SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde NFS için yüksek kullanılabilirlik][nfs-ha] 
- [SUSE Linux Enterprise Server SAP uygulamaları için Azure NetApp dosya çubuğunda Azure vm'lerinde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files) nasıl kullanılacağı hakkında [Azure NetApp dosyaları](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction) NFS paylaşımlarını oluşturmak için


## <a name="test-the-cluster-setup"></a>Test kümesi Kurulumu

Bu bölümde, Db2 HADR kurulumunuzu nasıl sınayıp doğrulayabileceğiniz açıklanır. **Her test kullanıcı kök olarak oturum açtığınız varsayılır** ve IBM Db2 birincil üzerinde çalışan **azibmdb01** sanal makine.

Tüm test çalışmalarını ilk durumu aşağıda açıklanmıştır: (crm_mon - r veya crm durumu)

- **CRM durumu** yürütme zamanında bir anlık görüntü Pacemaker durumu 
- **crm_mon - r** sürekli Pacemaker durumu çıktısı

<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Stopped
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Stopped
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     rsc_Db2_db2ptr_PTR      (ocf::heartbeat:db2):   Promoting azibmdb01
     Slaves: [ azibmdb02 ]
</code></pre>

Bir SAP sistemiyle özgün durumu işlem DBACOCKPIT belgelenen yapılandırması-->--> gibi genel bakış:

![DBACockpit - öncesi geçiş](./media/dbms-guide-ha-ibm/hadr-sap-mgr-org.png)




### <a name="test-takeover-of-ibm-db2"></a>IBM Db2'in test devralın.


> [!IMPORTANT] 
> Test başlamadan önce Pacemaker başarısız tüm eylemler (crm durumu) sahip değil ve hiçbir konum kısıtlamaları (parçalarla geçiş testi) vardır ve IBM Db2 HADR eşitleme çalıştığından emin olun. Kullanıcı db2 ile kontrol ile\<SID > <pre><code>db2pd -hadr -db \<DBSID></code></pre>


Aşağıdaki komutu yürüterek birincil Db2 veritabanı çalıştıran düğümü Geçir:
<pre><code>crm resource migrate msl_<b>Db2_db2ptr_PTR</b> azibmdb02</code></pre>

Geçiş yaptıktan sonra crm durumu çıktısı şuna benzer:
<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb02
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb02
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb02 ]
     Slaves: [ azibmdb01 ]
</code></pre>

Bir SAP sistemiyle özgün durumu işlem DBACOCKPIT belgelenen yapılandırması-->--> gibi genel bakış: ![DBACockpit - geçiş sonrası](./media/dbms-guide-ha-ibm/hadr-sap-mgr-post.png)

"Crm kaynak geçişi" ile geçiş kaynak konum kısıtlamaları oluşturur. Konum kısıtlamaları silinmesi gerekir. Konum kısıtlamaları silinmez, kaynak yeniden çalışma özelliğini kullanamazsınız veya istenmeyen takeovers oluşabilir. 

Kaynak geçişi geri **azibmdb01** konum kısıtlamaları temizleyin
<pre><code>crm resource migrate msl_<b>Db2_db2ptr_PTR</b> azibmdb01
crm resource clear msl_<b>Db2_db2ptr_PTR</b>
</code></pre>


- CRM kaynağını geçir < res_name > <host> - konum kısıtlamaları oluşturur ve devralma ile sorunlara neden olabilir
- CRM açık kaynak < res_name > - konum kısıtlamaları temizler
- CRM kaynak temizleme < res_name > - kaynağın tüm hataları temizler


### <a name="test-the-fencing-agent"></a>Sınır Aracısı'nı test edin

Bu durumda, SUSE Linux ile kullanım için önerilen SBD çitlemek test ederiz.

<pre><code>
azibmdb01:~ # ps -ef|grep sbd
root       2374      1  0 Feb05 ?        00:00:17 sbd: inquisitor
root       2378   2374  0 Feb05 ?        00:00:40 sbd: watcher: /dev/disk/by-id/scsi-36001405fbbaab35ee77412dacb77ae36 - slot: 0 - uuid: 27cad13a-0bce-4115-891f-43b22cfabe65
root       2379   2374  0 Feb05 ?        00:01:51 sbd: watcher: Pacemaker
root       2380   2374  0 Feb05 ?        00:00:18 sbd: watcher: Cluster

azibmdb01:~ # kill -9 2374
</code></pre>

Küme düğümü **azibmdb01** başlatılması. IBM Db2 birincil HADR rol taşınması geçmeye **azibmdb02**. Zaman **azibmdb01** olduğundan yeniden çevrimiçi, örneği bir ikincil veritabanı rolünde taşıma geçmeye Db2. 

Birincil yeniden başlatılan eski üzerinde Pacemaker hizmeti otomatik olarak başlamaz çalışması için bunu el ile başlatmak emin olun:

<code><pre>sudo service pacemaker start</code></pre>

### <a name="test-a-manual-takeover"></a>Testi el ile bir devralma

El ile bir devralma üzerinde Pacemaker hizmetini durdurarak sınayabilirsiniz **azibmdb01** düğüm:
<pre><code>service pacemaker stop</code></pre>

Durum **azibmdb02**
<pre><code>
2 nodes configured
5 resources configured

Online: [ azibmdb02 ]
OFFLINE: [ azibmdb01 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb02
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb02
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb02 ]
     Stopped: [ azibmdb01 ]
</code></pre>

Yük devretmeden sonra hizmeti yeniden üzerinde başlatabilirsiniz **azibmdb01**.
<pre><code>service pacemaker start</code></pre>


### <a name="kill-db2-process-on-the-node-running-the-hadr-primary-database"></a>HADR birincil veritabanı çalışan düğümü üzerindeki Db2 işlemini sonlandır

<pre><code>#Kill main db2 process - db2sysc
azibmdb01:~ # ps -ef|grep db2s
db2ptr    34598  34596  8 14:21 ?        00:00:07 db2sysc 0

azibmdb01:~ # kill -9 34598
</code></pre>

Db2 örneği başarısız olur ve durumu aşağıdaki Pacemaker bildirir:

<pre><code>
2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Stopped
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Stopped
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Slaves: [ azibmdb02 ]
     Stopped: [ azibmdb01 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_demote_0 on azibmdb01 'unknown error' (1): call=157, status=complete, exitreason='',
    last-rc-change='Tue Feb 12 14:28:19 2019', queued=40ms, exec=223ms


</code></pre>

Pacemaker Db2 birincil veritabanı örneğinde aynı düğüm veya yük devretme için örnekli ikincil veritabanı çalıştıran düğümü yeniden başlayacak ve bir hata bildirilir.

<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb01 ]
     Slaves: [ azibmdb02 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_demote_0 on azibmdb01 'unknown error' (1): call=157, status=complete, exitreason='',
    last-rc-change='Tue Feb 12 14:28:19 2019', queued=40ms, exec=223ms
</code></pre>


### <a name="kill-db2-process-on-node-that-runs-the-secondary-database-instance"></a>İkincil veritabanı örneğini çalıştıran düğümde Db2 işlemini sonlandır

<pre><code>azibmdb02:~ # ps -ef|grep db2s
db2ptr    65250  65248  0 Feb11 ?        00:09:27 db2sysc 0

azibmdb02:~ # kill -9</code></pre>

Düğüm içinde belirtilen başarısız ve bir hata bildirdi
<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     rsc_Db2_db2ptr_PTR      (ocf::heartbeat:db2):   FAILED azibmdb02
     Masters: [ azibmdb01 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_monitor_30000 on azibmdb02 'not running' (7): call=144, status=complete, exitreason='',
last-rc-change='Tue Feb 12 14:36:59 2019', queued=0ms, exec=0ms</code></pre>

Db2 örneği Al'ın içine önce atanmış ikincil rolü yeniden

<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb01 ]
     Slaves: [ azibmdb02 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_monitor_30000 on azibmdb02 'not running' (7): call=144, status=complete, exitreason='',
    last-rc-change='Tue Feb 12 14:36:59 2019', queued=0ms, exec=0ms</code></pre>



### <a name="stop-db-via-db2stop-force-on-node-running-the-hadr-primary-database-instance"></a>Durdurma DB db2stop üzerinden HADR birincil veritabanı örneğini çalıştıran düğümde zorla

<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb01 ]
     Slaves: [ azibmdb02 ]</code></pre>

Kullanıcı db2 olarak\<SID > db2stop force komutunu yürütün:
<pre><code>azibmdb01:~ # su - db2ptr
azibmdb01:db2ptr> db2stop force</code></pre>

Hatası algılandı
<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Stopped
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Stopped
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     rsc_Db2_db2ptr_PTR      (ocf::heartbeat:db2):   FAILED azibmdb01
     Slaves: [ azibmdb02 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_demote_0 on azibmdb01 'unknown error' (1): call=201, status=complete, exitreason='',
    last-rc-change='Tue Feb 12 14:45:25 2019', queued=1ms, exec=150ms</code></pre>

Birincil rolünde Db2 HADR ikincil veritabanı örneğinde yükseltilmiş
<pre><code> nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb02
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb02
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb02 ]
     Stopped: [ azibmdb01 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_start_0 on azibmdb01 'unknown error' (1): call=205, stat
us=complete, exitreason='',
    last-rc-change='Tue Feb 12 14:45:27 2019', queued=0ms, exec=865ms</pre></code>


### <a name="crash-vm-with-restart-on-node-running-the-hadr-primary-database-instance"></a>Kilitlenme VM ile HADR birincil veritabanı örneğini çalıştıran düğümü yeniden başlatma

<pre><code>#Linux kernel panic - with OS restart
azibmdb01:~ # echo b > /proc/sysrq-trigger</code></pre>

İkincil birincil örnek rol örneğine pacemaker yükseltmez. Eski birincil örneğinin ikincil rolde sonra VM'ye taşınır ve VM yeniden başlatıldıktan sonra tüm hizmetleri tam olarak geri yüklenir:

<pre><code> nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb01 ]
     Slaves: [ azibmdb02 ]</code></pre>



### <a name="crash-the-vm-running-the-hadr-primary-database-instance-with-halt"></a>Kilitlenme "durdurmak" ile HADR birincil veritabanı örneğini çalıştıran VM

<pre><code>#Linux kernel panic - halts OS
azibmdb01:~ # echo b > /proc/sysrq-trigger</code></pre>

Böyle bir durumda, birincil veritabanı örneğini çalıştıran düğümün vermiyor Pacemaker algılar.

<pre><code>2 nodes configured
5 resources configured

Node azibmdb01: UNCLEAN (online)
Online: [ azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb01 ]
     Slaves: [ azibmdb02 ]</code></pre>

Denetlenecek sonraki adımdır bir **bölme beyin** durumu. Kalan düğüm birincil veritabanı örneği son çalıştırma, düğüm kapalı olduğundan emin olduktan sonra yürütülecek bir yük devretme kaynakların geçiyor
<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb02 ]
OFFLINE: [ azibmdb01 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb02
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb02
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb02 ]
     Stopped: [ azibmdb01 ] </code></pre>


"Düğümü durdurma durumunda", Azure Yönetim Araçları (Portal, kullanabileceğiniz, Azureclı,...) başlatılması başarısız düğümü vardır. Başarısız olan düğümün yeniden çevrimiçi olduğunda ikincil rolü Db2 örneğine başlatıyorum.

<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb02
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb02
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb02 ]
     Slaves: [ azibmdb01 ]</code></pre>

## <a name="next-steps"></a>Sonraki adımlar
Bu belgelere bakın:

- [Yüksek kullanılabilirlik mimarisi ve senaryolar için SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios)
- [SLES SUSE Linux girin üzerinde Pacemaker ayarlama
- PRI
- azure'da sunucusu SE])https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker)

     

