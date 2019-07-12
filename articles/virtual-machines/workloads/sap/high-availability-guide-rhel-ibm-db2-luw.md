---
title: Azure sanal makinelerinde (VM'ler) rhel IBM Db2 HADR ' ayarlama | Microsoft Docs
description: IBM Db2 LUW yüksek kullanılabilirliğini Azure sanal makinelerinde (VM'ler) RHEL kurun.
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: SAP
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/10/2019
ms.author: juergent
ms.openlocfilehash: b26a66eaee3a107c37d64541ec6b832331a3e2c9
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67812135"
---
[1928533]: https://launchpad.support.sap.com/#/notes/1928533
[2015553]: https://launchpad.support.sap.com/#/notes/2015553
[2178632]: https://launchpad.support.sap.com/#/notes/2178632
[2191498]: https://launchpad.support.sap.com/#/notes/2191498
[2243692]: https://launchpad.support.sap.com/#/notes/2243692
[2002167]: https://launchpad.support.sap.com/#/notes/2002167
[1999351]: https://launchpad.support.sap.com/#/notes/1999351
[2233094]: https://launchpad.support.sap.com/#/notes/2233094
[1612105]: https://launchpad.support.sap.com/#/notes/1612105
[2694118]: https://launchpad.support.sap.com/#/notes/2694118


[db2-hadr-11.1]:https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.admin.ha.doc/doc/c0011267.html
[db2-hadr-10.5]:https://www.ibm.com/support/knowledgecenter/en/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0011267.html
[dbms-db2]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_ibm

[sap-instfind]:https://help.sap.com/viewer/9e41ead9f54e44c1ae1a1094b0f80712/ALL/en-US/576f5c1808de4d1abecbd6e503c9ba42.html
[rhel-ha-addon]:https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_overview/index
[rhel-ha-admin]:https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_administration/index
[rhel-ha-ref]:https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/index
[rhel-azr-supp]:https://access.redhat.com/articles/3131341
[rhel-azr-inst]:https://access.redhat.com/articles/3252491
[rhel-db2-supp]:https://access.redhat.com/articles/3144221
[ascs-ha-rhel]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel
[glusterfs]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-glusterfs
[rhel-pcs-azr]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker
[anf-rhel]:(https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files)

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md
[azr-sap-plancheck]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist



# <a name="high-availability-of-ibm-db2-luw-on-azure-vms-on-red-hat-enterprise-linux-server"></a>IBM Db2 LUW üzerindeki Azure Vm'lerinde Red Hat Enterprise Linux Server'ın yüksek kullanılabilirliği

Linux, UNIX ve Windows (LUW) içinde IBM Db2 [yüksek kullanılabilirlik ve olağanüstü durum kurtarma (HADR) yapılandırması](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0011267.html) birincil veritabanı örneği çalıştıran bir düğümü ve bir ikincil veritabanı örneğini çalıştıran en az bir düğüm oluşur. Birincil veritabanı örneğine değişiklikler ikincil veritabanı örneğine yapılandırmanıza bağlı olarak zaman uyumlu veya zaman uyumsuz olarak çoğaltılır. 

Bu makalede, dağıtma ve Azure sanal makineleri (VM'ler) yapılandırma, küme Framework'ü yüklemek ve IBM Db2 LUW HADR yapılandırması ile yükleyecek açıklar. 

Makaleyi yüklemek ve IBM Db2 LUW HADR veya SAP yazılım yüklemesi ile yapılandırmak nasıl ele alınmamıştır. Bu görevleri gerçekleştirmenize yardımcı olmak için SAP ve IBM'den yükleme kılavuzlarını başvuruları sunuyoruz. Bu makalede Azure ortamınıza özgü olan bölümleri ele alınmaktadır. 

Desteklenen IBM Db2 sürümleri 10.5 ve üzeri, SAP Not açıklandığı gibi [1928533].

Yükleme başlamadan önce aşağıdaki SAP notları ve belgeler bakın:

| SAP notuna göz atın | Açıklama |
| --- | --- |
| [1928533] | Azure'da SAP uygulamaları: Desteklenen Ürünler ve Azure VM türleri |
| [2015553] | Azure üzerinde SAP: Destek önkoşulları |
| [2178632] | Azure'da SAP için ölçümleri izleme anahtarı |
| [2191498] | Azure ile Linux üzerinde SAP: Gelişmiş izleme |
| [2243692] | Linux üzerinde Azure (Iaas) sanal makine: SAP lisans sorunları |
| [2002167] | Red Hat Enterprise Linux 7.x: Yükleme ve yükseltme |
| [2694118] | Azure'da Red Hat Enterprise Linux HA eklentisi |
| [1999351] | Gelişmiş Azure için SAP izleme sorunlarını giderme |
| [2233094] | DB6: Linux, UNIX ve Windows - ek bilgiler için IBM Db2 kullanan uygulamalar Azure üzerinde SAP |
| [1612105] | DB6: Db2 HADR ile hakkında SSS |


| Belgeler | 
| --- |
| [SAP topluluk Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes): Linux için sahip tüm gerekli SAP notları |
| [Azure sanal makineleri planlama ve uygulama için Linux üzerinde SAP][planning-guide] Kılavuzu |
| [Azure sanal makineler dağıtım için Linux'ta SAP][deployment-guide] (Bu makale) |
| [Azure sanal makineleri veritabanı yönetim system(DBMS) dağıtım için Linux'ta SAP][dbms-guide] Kılavuzu |
| [SAP iş yüküne Azure planlama ve dağıtım denetim listesi][azr-sap-plancheck] |
| [Red Hat Enterprise Linux 7 için yüksek kullanılabilirlik eklentiye genel bakış][rhel-ha-addon] |
| [Yüksek kullanılabilirlik eklenti Yönetim][rhel-ha-admin] |
| [Yüksek kullanılabilirlik eklenti başvurusu][rhel-ha-ref] |
| [RHEL yüksek kullanılabilirlik kümelerini - Microsoft Azure sanal makineleri küme üyeleri olarak ilkeleri desteği][rhel-azr-supp]
| [Yükleme ve Microsoft Azure'da Red Hat Enterprise Linux 7.4 (ve üzeri) yüksek kullanılabilirlik kümesi yapılandırma][rhel-azr-inst]
| [SAP iş yükü için IBM Db2 Azure sanal makineleri DBMS dağıtım][dbms-db2] |
| [IBM Db2 HADR 11.1][db2-hadr-11.1] |
| [IBM Db2 HADR 10.5][db2-hadr-10.5] |
| [RHEL yüksek kullanılabilirlik kümelerini - IBM Db2 yönetimi için Windows bir küme içinde Linux ve UNIX için destek ilkesi][rhel-db2-supp]



## <a name="overview"></a>Genel Bakış
Yüksek kullanılabilirlik elde etmek için IBM Db2 LUW HADR ile en az iki Azure sanal makinelerinde, dağıtılan, yüklü bir [Azure kullanılabilirlik kümesine](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) veya çapraz [Azure kullanılabilirlik alanları](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ha-availability-zones). 

Aşağıdaki grafik, iki veritabanı Server Azure Vm'leri bir ayarını görüntüleyin. Hem veritabanı sunucusuna Azure Vm'leri bağlı kendi depolama alanına sahip ve çalışıyor olduğundan. HADR içinde birincil örneği rolünü bir Azure sanal makinelerinin bir veritabanı örneği vardır. Tüm istemciler, birincil örneğine bağlanır. Veritabanı işlemleri tüm değişiklikleri yerel olarak Db2 işlem günlüğüne kalıcıdır. Hareket günlüğü değişikliklerini yerel olarak kalıcı olarak kayıtları TCP/IP veritabanı örneği için ikinci bir veritabanı sunucusu, bekleme sunucusunun veya bekleme örneği aktarılır. Bekleme örneği aktarılan işlem günlüğü kayıtlarını İleri sarmadır yerel veritabanına güncelleştirir. Bu şekilde, bekleme sunucusunun birincil sunucu ile eşitlenmiş olarak tutulur.

HADR yalnızca bir çoğaltma işlevdir. Bu, hiçbir hata algılama ve otomatik bir devralma veya yük devretme özellikleri vardır. Devralma veya aktarımı yedek sunucu için veritabanı yöneticisi tarafından el ile başlatılmalıdır. Bir otomatik devralma ve hata algılama elde etmek için Linux Pacemaker kümeleme özelliğini kullanabilirsiniz. Pacemaker iki veritabanı sunucu örnekleri izler. Ne zaman birincil veritabanı sunucusu örneğine kilitlenmeler, Pacemaker başlatırken bir *otomatik* HADR devralma yedek sunucu tarafından. Yeni birincil sunucu için sanal IP adresinin atandığından emin pacemaker de sağlar.

![IBM Db2 yüksek kullanılabilirlik genel bakış](./media/high-availability-guide-rhel-ibm-db2-luw/ha-db2-hadr-lb-rhel.png)

SAP için uygulama sunucuları birincil veritabanına bağlanmak, bir sanal ana bilgisayar adı ve sanal bir IP adresi gerekir. Bir yük devretmesi durumunda, SAP uygulama sunucuları, yeni birincil veritabanı örneğine bağlanır. Azure ortamınızda, bir [Azure yük dengeleyici](https://microsoft.sharepoint.com/teams/WAG/AzureNetworking/Wiki/Load%20Balancing.aspx) sanal bir IP adresi için IBM Db2 HADR gereken şekilde kullanmak için gereklidir. 

Tam olarak IBM Db2 LUW HADR ve Pacemaker ile yüksek oranda kullanılabilir bir SAP sistemi Kurulum nasıl uyduğunu anlamanıza yardımcı olmak için aşağıdaki görüntüde yüksek oranda kullanılabilir bir IBM Db2 veritabanına bağlı bir SAP sistemiyle kurulumu genel bir bakış sunar. Bu makale yalnızca IBM Db2 kapsar, ancak bir SAP sisteminin diğer bileşenleri ayarlamak konusunda diğer makaleler başvuru sağlar.

![IBM DB2 yüksek kullanılabilirlik ortamında tam genel bakış](./media/high-availability-guide-rhel-ibm-db2-luw/end-2-end-ha-rhel.png)


### <a name="high-level-overview-of-the-required-steps"></a>Gerekli adımlara üst düzey genel bakış
Bir IBM Db2 yapılandırmasını dağıtmak için bu adımları izlemeniz gerekir:

  + Ortamınızı planlayın.
  + Vm'leri dağıtın.
  + Linux RHEL güncelleştirme ve dosya sistemleri yapılandırın.
  + Yükleyip Pacemaker yapılandırın.
  + Kurulum [glusterfs küme][glusterfs] or [Azure NetApp Files][anf-rhel]
  + Yükleme [ASCS/Ağıranlar ayrı bir kümede][ascs-ha-rhel].
  + IBM Db2 veritabanı (SWPM) dağıtılmış/yüksek kullanılabilirlik seçeneği ile yükleyin.
  + Yükleyin ve bir ikincil veritabanı düğümü ve örneği oluşturun ve HADR yapılandırın.
  + HADR çalıştığını onaylayın.
  + Pacemaker yapılandırma IBM Db2 denetimi için geçerlidir.
  + Azure yük dengeleyici yapılandırın.
  + Birincil yükleme ve iletişim uygulama sunucuları.
  + Denetleyin ve SAP uygulama sunucuları yapılandırmasını uyarlayabilirsiniz.
  + Yük devretme ve devralma testleri gerçekleştirin.



## <a name="plan-azure-infrastructure-for-hosting-ibm-db2-luw-with-hadr"></a>IBM Db2 LUW HADR ile barındırmak için Azure altyapısını planlama

Dağıtım yürütmeden önce Planlama işlemi tamamlayın. Planlama ile azure'da HADR Db2 yapılandırılmasını dağıtmak için temel oluşturur. IMB Db2 LUW (SAP ortamının parçası veritabanı) için planlama bir parçası olması gereken temel öğeleri aşağıdaki tabloda listelenmiştir:

| Konu | Kısa açıklama |
| --- | --- |
| Azure kaynak gruplarını tanımlama | VM, sanal ağ, Azure Load Balancer ve diğer kaynakların nerede dağıttığınız kaynak grupları. Mevcut veya yeni olabilir. |
| Sanal ağ / alt ağ tanımı | IBM Db2 ve Azure Load Balancer için VM'ler dağıtıldığı. Mevcut veya yeni oluşturulmuş olabilir. |
| IBM Db2 LUW barındıran sanal makineler | VM boyutu, depolama, ağ, IP adresi. |
| Sanal ana bilgisayar adı ve IBM Db2 veritabanı için sanal IP| SAP uygulama sunucuları bir bağlantı için kullanılan sanal IP veya ana bilgisayar adı. **DB vırt hostname**, **db vırt IP**. |
| Azure çitlemek | Bölme beyin gibi durumlarla karşılaşmamak için yöntemi engellenir. |
| Azure Load Balancer | Temel kullanımını veya araştırma Db2 veritabanı (önerimizde 62500) için bağlantı noktası (önerilen) standart **araştırma bağlantı noktasını**. |
| Ad çözümlemesi| Nasıl ad çözümlemesi bir ortamda çalışır. DNS hizmeti önemle tavsiye edilir. Yerel ana bilgisayar dosyası kullanılabilir. |
    
Azure'da Linux Pacemaker hakkında daha fazla bilgi için bkz: [SLES azure'da Red Hat Enterprise Linux üzerinde Pacemaker ayarlama][rhel-pcs-azr].

## <a name="deployment-on-red-hat-enterprise-linux"></a>Red Hat Enterprise Linux üzerinde dağıtım

Red Hat Enterprise Linux Server HA eklentisi içinde IBM Db2 LUW kaynak aracısına dahildir. Bu belgede açıklanan kurulumu için SAP için Red Hat Enterprise Linux kullanmanız gerekir. Azure Market'te Red Hat Enterprise Linux, yeni Azure sanal makinelerini dağıtmak için kullanabileceğiniz 7.4 için SAP ya da üst sürümü bir görüntü içerir. Azure sanal makine Marketi'nde bir VM görüntüsü seçerseniz, Azure Marketi aracılığıyla Red Hat tarafından sunulan çeşitli destek veya hizmet modellerini farkında olun.

### <a name="hosts-dns-updates"></a>Konaklar: DNS güncelleştirmeleri
Sanal ana bilgisayar adları da dahil olmak üzere tüm konak adlarının bir listesini ve ana bilgisayar adı çözümlemesi için uygun IP adresi etkinleştirmek için DNS sunucularınızın güncelleştirin. Bir DNS sunucusu yok veya güncelleştirin ve DNS girişleri oluşturmanızı, bu senaryoda katılan tek tek sanal makinelerin yerel ana bilgisayar dosyalarını kullanmanız gerekir. Konak dosyaları girişleri kullanıyorsanız girişleri SAP sistemi ortamdaki tüm vm'lere uygulanır emin olun. Ancak, ideal olarak, Azure'a genişletir, DNS sunucunuzun kullanmanızı öneririz


### <a name="manual-deployment"></a>El ile dağıtım

Seçilen işletim sistemi için IBM Db2 LUW IBM/SAP tarafından desteklendiğinden emin olun. Azure Vm'leri ve Db2 yayınlar için desteklenen işletim sistemi sürümlerinin listesi SAP notu kullanılabilir [1928533]. Tek tek Db2 sürüm tarafından işletim sistemi sürümleri listesi SAP ürün kullanılabilirliği matriste kullanılabilir. Red Hat Enterprise Linux 7.4 SAP için en az bu veya daha sonra Azure ile ilgili performans iyileştirmeleri nedeniyle öneririz Red Hat Enterprise Linux sürümleri.

1. Oluşturun veya bir kaynak grubu seçin.
1. Oluşturun veya bir sanal ağ ve alt ağ seçin.
1. Azure kullanılabilirlik kümesi oluşturma veya bir kullanılabilirlik bölgesine dağıtın.
    + Kullanılabilirlik kümesi için en fazla bir güncelleme etki alanlarının 2 olarak ayarlayın.
1. 1 sanal makine oluşturun.
    + SAP görüntüsü Azure Market'te Red Hat Enterprise Linux'ı kullanın.
    + 3\. adımda oluşturduğunuz Azure kullanılabilirlik kümesi seçin veya kullanılabilirlik bölgesi seçin.
1.  2 sanal makine oluşturun.
    + SAP görüntüsü Azure Market'te Red Hat Enterprise Linux'ı kullanın.
    + Size 3. adımda oluşturulan veya kullanılabilirlik bölgesi (değil aynı bölge 3. adım olduğu gibi) seçin, Azure kullanılabilirlik kümesi seçin.
1. Vm'lere veri diski ekleyin ve ardından bir dosya sistemi Kurulum makaledeki önerisi [SAP iş yükü için IBM Db2 Azure sanal makineleri DBMS dağıtım][dbms-db2].

## <a name="create-the-pacemaker-cluster"></a>Pacemaker kümesi oluşturma
    
IBM Db2 bu sunucu için temel Pacemaker küme oluşturmak için bkz: [SLES azure'da Red Hat Enterprise Linux üzerinde Pacemaker ayarlama][rhel-pcs-azr]. 

## <a name="install-the-ibm-db2-luw-and-sap-environment"></a>IBM Db2 LUW ve SAP ortamı yükleyin

IBM Db2 LUW üzerinde temel bir SAP ortamının yüklemeye başlamadan önce aşağıdaki belgeleri gözden geçirin:

+ Azure belgeleri
+ SAP belgeleri
+ IBM belgeleri

Bu belgelere bağlantılar, bu makalenin tanıtım bölümünde sağlanır.

NetWeaver tabanlı uygulamalar üzerinde IBM Db2 LUW yükleme hakkında daha fazla SAP yükleme kılavuzlarını denetleyin.
Kullanarak SAP Yardım portalı Kılavuzlar bulabilirsiniz [SAP Yükleme Kılavuzu Bulucu][sap-instfind].

Aşağıdaki filtreler ayarlayarak gösterilmesi kılavuzları sayısını azaltabilirsiniz:
- Yapmak istiyorum: "Yeni bir sistem Yükle"
- Veritabanı: "IBM Db2 Linux, Unix ve Windows için"
- SAP NetWeaver sürümleri, yığın yapılandırması veya işletim sistemi için ek filtreler

#### <a name="red-hat-firewall-rules"></a>Red Hat güvenlik duvarı kuralları
Red Hat Enterprise Linux Güvenlik Duvarı varsayılan olarak etkin sahiptir. 

<pre><code>#Allow access to SWPM tool. Rule is not permanent.
sudo firewall-cmd --add-port=4237/tcp</code></pre>

### <a name="installation-hints-for-setting-up-ibm-db2-luw-with-hadr"></a>Yükleme ipuçlarını IBM Db2 LUW HADR ile ayarlama

Birincil IBM Db2 LUW veritabanı örneği için:

- Yüksek kullanılabilirlik veya dağıtılmış seçeneğini kullanın.
- SAP ASCS/Ağıranlar ve veritabanı örneğini yükleyin.
- Yeni yüklenen veritabanının bir yedeğini alın.

> [!IMPORTANT] 
> "Yüklemesi sırasında ayarladığınız veritabanı iletişimi bağlantı noktasını" yazın. Her iki veritabanı örnekleri için aynı bağlantı noktası numarası olmalıdır.
>![SAP SWPM bağlantı noktası tanımı](./media/high-availability-guide-rhel-ibm-db2-luw/hadr-swpm-db2-port.png)

### <a name="ibm-db2-hadr-settings-for-azure"></a>Azure için IBM Db2 HADR ayarları

   Bir Azure Pacemaker çitlemek agent kullandığınızda, aşağıdaki parametreleri ayarlayın:

   - HADR eş pencere süresi (saniye) (HADR_PEER_WINDOW) 240 =  
   - HADR zaman aşımı değerini (HADR_TIMEOUT) = 45

İlk yük devretme/devralma teste dayanan önceki parametreleri öneririz. Yük devretme ve devralma düzgün çalışması için bu parametreyi ayarlarla test zorunludur. Yapılandırmalar farklılık gösterebileceğinden, parametreleri ayarlama gerektirebilir. 

> [!NOTE]
> IBM Db2 normal başlangıç HADR yapılandırmasıyla ile belirli: Birincil veritabanı örneği başlamadan önce ikincil ya da bekleme veritabanı örneği ve çalışıyor olması gerekir.

   
> [!NOTE]
> Yükleme ve Azure ve Pacemaker özel yapılandırması için: SAP yazılım sağlama Yöneticisi aracılığıyla yükleme yordamı sırasında yüksek kullanılabilirlik için IBM Db2 LUW hakkında açık bir soru vardır:
>+ Seçmeyin **IBM Db2 pureScale**.
>+ Seçmeyin **Multiplatforms için IBM Tivoli sistem Otomasyon**.
>+ Seçmeyin **küme yapılandırma dosyaları oluştur**.
>![SAP SWPM - DB2 HA seçenekleri](./media/high-availability-guide-rhel-ibm-db2-luw/swpm-db2ha-opt.png)


SAP homojen sistem kopyalama yordamı kullanarak bekleme veritabanı sunucusunu ayarlamak için aşağıdaki adımları uygulayın:

1. Seçin **sistem kopyası** seçeneği > **hedef sistemleri** > **dağıtılmış** > **veritabanı örneği**.
1. Bir kopyalama yöntemi seçin **homojen sistem** böylece yedekleme, yedek sunucu örneğinde bir yedeklemeyi geri yüklemek için kullanabilirsiniz.
1. Homojen sistem kopyası için veritabanını geri yüklemek için çıkış adımına geldiğinizde, yükleyici çıkın. Veritabanı birincil ana bilgisayarın bir yedekten geri yükleyin. Tüm sonraki yükleme aşamaları birincil veritabanı sunucusunda zaten yürüttünüz.

#### <a name="red-hat-firewall-rules-for-db2-hadr"></a>Red Hat DB2 HADR için güvenlik duvarı kuralları
DB2 ve DB2 çalışmaya HADR için arasında trafiğe izin vermek için güvenlik duvarı kuralı ekleyin:
+ Veritabanı iletişim bağlantı noktası. Bölümler kullanıyorsanız, bu bağlantı noktalarını çok ekleyin.
+ HADR bağlantı noktası (HADR_LOCAL_SVC DB2 parametresinin değeri)
+ Azure araştırma bağlantı noktası
<pre><code>sudo firewall-cmd --add-port=&lt;port&gt;/tcp --permanent
sudo firewall-cmd --reload</code></pre>

#### <a name="ibm-db2-hadr-check"></a>IBM Db2 HADR denetimi
Tanıtım amaçlı ve bu makalede açıklanan yordamları için SID veritabanıdır **ıd2**.

HADR yapılandırdıktan ve durum eş ile bağlı birincil ve hazır bekleyen düğümlerinde olduktan sonra aşağıdaki denetimi gerçekleştirin:

<pre><code>
Execute command as db2&lt;sid&gt; db2pd -hadr -db &lt;SID&gt;

#Primary output:
Database Member 0 -- Database ID2 -- Active -- Up 1 days 15:45:23 -- Date 2019-06-25-10.55.25.349375

                            <b>HADR_ROLE = PRIMARY
                          REPLAY_TYPE = PHYSICAL
                        HADR_SYNCMODE = NEARSYNC
                           STANDBY_ID = 1
                        LOG_STREAM_ID = 0
                           HADR_STATE = PEER
                           HADR_FLAGS =
                  PRIMARY_MEMBER_HOST = az-idb01
                     PRIMARY_INSTANCE = db2id2
                       PRIMARY_MEMBER = 0
                  STANDBY_MEMBER_HOST = az-idb02
                     STANDBY_INSTANCE = db2id2
                       STANDBY_MEMBER = 0
                  HADR_CONNECT_STATUS = CONNECTED</b>
             HADR_CONNECT_STATUS_TIME = 06/25/2019 10:55:05.076494 (1561460105)
          HEARTBEAT_INTERVAL(seconds) = 7
                     HEARTBEAT_MISSED = 5
                   HEARTBEAT_EXPECTED = 52
                HADR_TIMEOUT(seconds) = 30
        TIME_SINCE_LAST_RECV(seconds) = 5
             PEER_WAIT_LIMIT(seconds) = 0
           LOG_HADR_WAIT_CUR(seconds) = 0.000
    LOG_HADR_WAIT_RECENT_AVG(seconds) = 598.000027
   LOG_HADR_WAIT_ACCUMULATED(seconds) = 598.000
                  LOG_HADR_WAIT_COUNT = 1
SOCK_SEND_BUF_REQUESTED,ACTUAL(bytes) = 0, 46080
SOCK_RECV_BUF_REQUESTED,ACTUAL(bytes) = 0, 369280
            PRIMARY_LOG_FILE,PAGE,POS = S0000012.LOG, 14151, 3685322855
            STANDBY_LOG_FILE,PAGE,POS = S0000012.LOG, 14151, 3685322855
                  HADR_LOG_GAP(bytes) = 132242668
     STANDBY_REPLAY_LOG_FILE,PAGE,POS = S0000012.LOG, 14151, 3685322855
       STANDBY_RECV_REPLAY_GAP(bytes) = 0
                     PRIMARY_LOG_TIME = 06/25/2019 10:45:42.000000 (1561459542)
                     STANDBY_LOG_TIME = 06/25/2019 10:45:42.000000 (1561459542)
              STANDBY_REPLAY_LOG_TIME = 06/25/2019 10:45:42.000000 (1561459542)
         STANDBY_RECV_BUF_SIZE(pages) = 2048
             STANDBY_RECV_BUF_PERCENT = 0
           STANDBY_SPOOL_LIMIT(pages) = 1000
                STANDBY_SPOOL_PERCENT = 0
                   STANDBY_ERROR_TIME = NULL
                 PEER_WINDOW(seconds) = 300
                      PEER_WINDOW_END = 06/25/2019 11:12:03.000000 (1561461123)
             READS_ON_STANDBY_ENABLED = N



#Secondary output:
Database Member 0 -- Database ID2 -- Standby -- Up 1 days 15:45:18 -- Date 2019-06-25-10.56.19.820474

                            <b>HADR_ROLE = STANDBY
                          REPLAY_TYPE = PHYSICAL
                        HADR_SYNCMODE = NEARSYNC
                           STANDBY_ID = 0
                        LOG_STREAM_ID = 0
                           HADR_STATE = PEER
                           HADR_FLAGS =
                  PRIMARY_MEMBER_HOST = az-idb01
                     PRIMARY_INSTANCE = db2id2
                       PRIMARY_MEMBER = 0
                  STANDBY_MEMBER_HOST = az-idb02
                     STANDBY_INSTANCE = db2id2
                       STANDBY_MEMBER = 0
                  HADR_CONNECT_STATUS = CONNECTED</b>
             HADR_CONNECT_STATUS_TIME = 06/25/2019 10:55:05.078116 (1561460105)
          HEARTBEAT_INTERVAL(seconds) = 7
                     HEARTBEAT_MISSED = 0
                   HEARTBEAT_EXPECTED = 10
                HADR_TIMEOUT(seconds) = 30
        TIME_SINCE_LAST_RECV(seconds) = 1
             PEER_WAIT_LIMIT(seconds) = 0
           LOG_HADR_WAIT_CUR(seconds) = 0.000
    LOG_HADR_WAIT_RECENT_AVG(seconds) = 598.000027
   LOG_HADR_WAIT_ACCUMULATED(seconds) = 598.000
                  LOG_HADR_WAIT_COUNT = 1
SOCK_SEND_BUF_REQUESTED,ACTUAL(bytes) = 0, 46080
SOCK_RECV_BUF_REQUESTED,ACTUAL(bytes) = 0, 367360
            PRIMARY_LOG_FILE,PAGE,POS = S0000012.LOG, 14151, 3685322855
            STANDBY_LOG_FILE,PAGE,POS = S0000012.LOG, 14151, 3685322855
                  HADR_LOG_GAP(bytes) = 0
     STANDBY_REPLAY_LOG_FILE,PAGE,POS = S0000012.LOG, 14151, 3685322855
       STANDBY_RECV_REPLAY_GAP(bytes) = 0
                     PRIMARY_LOG_TIME = 06/25/2019 10:45:42.000000 (1561459542)
                     STANDBY_LOG_TIME = 06/25/2019 10:45:42.000000 (1561459542)
              STANDBY_REPLAY_LOG_TIME = 06/25/2019 10:45:42.000000 (1561459542)
         STANDBY_RECV_BUF_SIZE(pages) = 2048
             STANDBY_RECV_BUF_PERCENT = 0
           STANDBY_SPOOL_LIMIT(pages) = 1000
                STANDBY_SPOOL_PERCENT = 0
                   STANDBY_ERROR_TIME = NULL
                 PEER_WINDOW(seconds) = 1000
                      PEER_WINDOW_END = 06/25/2019 11:12:59.000000 (1561461179)
             READS_ON_STANDBY_ENABLED = N

</code></pre>



## <a name="db2-pacemaker-configuration"></a>Db2 Pacemaker yapılandırma

Bir düğüm hatası durumunda otomatik yük devretme için Pacemaker kullandığınızda, Db2 örnekleri ve Pacemaker uygun şekilde yapılandırmak gerekir. Bu bölümde, bu tür bir yapılandırma açıklanmaktadır.

Aşağıdaki öğeler ile önek:

- **[A]** : Tüm düğümler için geçerlidir
- **[1]** : Yalnızca 1 düğümü geçerlidir 
- **[2]** : Yalnızca düğüm 2 için geçerlidir

**[A]**  Pacemaker yapılandırma önkoşulları:
1. Her iki kullanıcı db2 veritabanı sunucularıyla kapatma\<SID > db2stop ile.
1. Db2 için kabuk ortamını değiştirme\<SID > kullanıcıya */bin/ksh*:
<pre><code># Install korn shell:
sudo yum install ksh
# Change users shell:
sudo usermod -s /bin/ksh db2&lt;sid&gt;</code></pre>
   

### <a name="pacemaker-configuration"></a>Pacemaker yapılandırma

**[1]**  IBM Db2 HADR özgü Pacemaker yapılandırma:
<pre><code># Put Pacemaker into maintenance mode
sudo pcs property set maintenance-mode=true 
</code></pre>

**[1]**  IBM Db2 oluşturma kaynakları:
<pre><code># Replace <b>bold strings</b> with your instance name db2sid, database SID, and virtual IP address/Azure Load Balancer.
sudo pcs resource create Db2_HADR_<b>ID2</b> db2 instance='<b>db2id2</b>' dblist='<b>ID2</b>' master meta notify=true resource-stickiness=5000

#Configure resource stickiness and correct cluster notifications for master resoruce
sudo pcs resource update Db2_HADR_<b>ID2</b>-master meta notify=true resource-stickiness=5000

# Configure virtual IP - same as Azure Load Balancer IP
sudo pcs resource create vip_<b>db2id2</b>_<b>ID2</b> IPaddr2 ip='<b>10.100.0.40</b>'

# Configure probe port for Azure load Balancer
sudo pcs resource create nc_<b>db2id2</b>_<b>ID2</b> azure-lb port=<b>62500</b>

#Create a group for ip and Azure loadbalancer probe port
sudo pcs resource group add g_ipnc_<b>db2id2</b>_<b>ID2</b> vip_<b>db2id2</b>_<b>ID2</b> nc_<b>db2id2</b>_<b>ID2</b>

#Create colocation constrain - keep Db2 HADR Master and Group on same node
sudo pcs constraint colocation add g_ipnc_<b>db2id2</b>_<b>ID2</b> with master Db2_HADR_<b>ID2</b>-master

#Create start order constrain
sudo pcs constraint order promote Db2_HADR_<b>ID2</b>-master then g_ipnc_<b>db2id2</b>_<b>ID2</b>
</code></pre>

**[1]**  IBM Db2 başlangıç kaynakları:
* Bakım modundan Pacemaker yerleştirin.
<pre><code># Put Pacemaker out of maintenance-mode - that start IBM Db2
sudo pcs property set maintenance-mode=false</pre></code>

**[1]**  Küme durumunun Tamam olduğunu ve tüm kaynakları başlatıldığından emin olun. Bu kaynaklar üzerinde çalışan hangi düğümün önemli değildir.
<pre><code>sudo pcs status</code>
2 nodes configured
5 resources configured

Çevrimiçi: [az-idb01 az idb02]

Kaynakların tam listesi:

 rsc_st_azure   (stonith:fence_azure_arm):      Başlatılan az idb01 yönetici/bağımlı kümesi: Asıl Db2_HADR_ID2-ana [Db2_HADR_ID2]: [az-idb01] kopyalanmış: [az-idb02] kaynak grubu: g_ipnc_db2id2_ID2 vip_db2id2_ID2 (ocf::heartbeat:IPaddr2):       Az idb01 nc_db2id2_ID2 başlatıldı (ocf::heartbeat:azure-lb):      Az idb01 kullanmaya başlama

Arka plan programı durumu: corosync: etkin/devre dışı pacemaker: etkin/devre dışı pcsd: etkin/etkin
</pre>

> [!IMPORTANT]
> Pacemaker yönetmelidir Pacemaker araçlarını kullanarak kümelenmiş Db2 örneği. Db2 komutları gibi db2stop kullanırsanız, kaynak olarak bir hata eylemi Pacemaker algılar. Bakım modunda bakım gerçekleştiriyorsanız, düğümlere ya da kaynaklar koyabilirsiniz. İzleme kaynaklarını pacemaker askıya alır ve sonra normal db2 yönetim komutları kullanabilirsiniz.


### <a name="configure-azure-load-balancer"></a>Azure Load Balancer'ı yapılandırma
Azure Load Balancer'ı yapılandırmak için kullanmanızı öneririz [Azure standart yük dengeleyici SKU](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview) ve ardından aşağıdakileri yapın;

1. Ön uç IP havuzu oluşturun:

   a. Azure portalında, Azure Load Balancer açın, **ön uç IP havuzu**ve ardından **Ekle**.

   b. Yeni ön uç IP havuzunun adını girin (örneğin, **Db2 bağlantı**).

   c. Ayarlama **atama** için **statik**ve IP adresini girin **sanal IP** başında tanımlanır.

   d. **Tamam**’ı seçin.

   e. Yeni ön uç IP havuzu oluşturulduktan sonra havuzu IP adresini not edin.

1. Arka uç havuzu oluşturun:

   a. Azure portalında, Azure Load Balancer açın, **arka uç havuzları**ve ardından **Ekle**.

   b. Yeni arka uç havuzunun adını girin (örneğin, **Db2 arka uç**).

   c. Seçin **bir sanal makine ekleme**.

   d. Kullanılabilirlik kümesi ya da önceki adımda oluşturduğunuz IBM Db2 veritabanı barındıran sanal makineleri seçin.

   e. IBM Db2 kümedeki sanal makineleri seçin.

   f. **Tamam**’ı seçin.

1. Durum araştırması oluşturun:

   a. Azure portalında, Azure Load Balancer açın, **sistem durumu araştırmaları**seçip **Ekle**.

   b. Yeni bir sistem durumu araştırma adını girin (örneğin, **Db2 hp**).

   c. Seçin **TCP** protokolü ve bağlantı noktası olarak **62500**. Tutun **aralığı** değerine **5**, koruyarak **sağlıksız durum eşiği** değerine **2**.

   d. **Tamam**’ı seçin.

1. Yük Dengeleme kuralları oluşturun:

   a. Azure portalında, Azure Load Balancer açın, **Yük Dengeleme kuralları**ve ardından **Ekle**.

   b. Yeni Yük Dengeleyici kuralı adını girin (örneğin, **Db2 SID**).

   c. Ön uç IP adresi, arka uç havuzu ve durum yoklaması, daha önce oluşturduğunuz seçin (örneğin, **Db2 ön uç**).

   d. Tutmak **Protokolü** kümesine **TCP**, bağlantı noktası girin *veritabanı iletişimi bağlantı noktasını*.

   e. Artırmak **boşta kalma zaman aşımı** 30 dakika.

   f. Emin olun **kayan IP'yi etkinleştirebilirsiniz**.

   g. **Tamam**’ı seçin.

**[A]**  Araştırma bağlantı noktası için Güvenlik Duvarı Kuralı Ekle:
<pre><code>sudo firewall-cmd --add-port=<b><probe-port></b>/tcp --permanent
sudo firewall-cmd --reload</code></pre>

### <a name="make-changes-to-sap-profiles-to-use-virtual-ip-for-connection"></a>Sanal IP bağlantısı için kullanmak üzere SAP profilleri değişiklik
SAP HADR yapılandırması birincil örneğine bağlanmak için uygulama katmanı tanımlanan ve Azure Load Balancer için yapılandırılan sanal IP adresini kullanması gerekir. Aşağıdaki değişiklikler gereklidir:

/sapmnt/\<SID >/profile/varsayılan. PFL
<pre><code>SAPDBHOST = db-virt-hostname
j2ee/dbhost = db-virt-hostname
</code></pre>

/sapmnt/\<SID>/global/db6/db2cli.ini
<pre><code>Hostname=db-virt-hostname
</code></pre>



## <a name="install-primary-and-dialog-application-servers"></a>Birincil yükleme ve iletişim uygulama sunucuları

Birincil yükleyin ve iletişim uygulama sunucuları bir Db2 HADR yapılandırma karşı kullanım sanal ana bilgisayar adı, yapılandırma için seçtiğiniz. 

Db2 HADR yapılandırma oluşturduğunuz önce yükleme gerçekleştirdiyseniz, önceki bölümde ve SAP Java yığınları için aşağıda açıklanan değişiklikleri yapın.

### <a name="abapjava-or-java-stack-systems-jdbc-url-check"></a>ABAP + Java veya Java stack sistemleri JDBC URL'sini denetleyin

İade veya JDBC URL'sini güncelleştirme J2EE Yapılandırma Aracı'nı kullanın. J2EE Yapılandırma Aracı'nı bir grafik aracı olduğundan, X ihtiyacınız yüklü sunucuda:
 
1. J2EE örneğinin birincil uygulama sunucusunda oturum açın ve yürütün:
     <pre><code>sudo /usr/sap/*SID*/*Instance*/j2ee/configtool/configtool.sh</code></pre>
1. Sol çerçevede seçin **güvenlik deposu**.
1. Anahtar jdbc/havuz'a sağ çerçevede seçin / \ <SAPSID> /URL.
1. Sanal ana bilgisayar adı için JDBC URL ana bilgisayar adını değiştirin.
     <pre><code>jdbc:db2://db-virt-hostname:5912/TSP:deferPrepares=0</code></pre>
1. Seçin **ekleme**.
1. Değişikliklerinizi kaydetmek için sol üst disk simgesini seçin.
1. Yapılandırma aracını kapatın.
1. Java örneğini yeniden başlatın.

## <a name="configure-log-archiving-for-hadr-setup"></a>Günlük HADR kurulumu için arşivleme yapılandırın
HADR kurulumu için arşivleme Db2 günlük yapılandırmak için hem birincil hem de bekleme veritabanının tüm günlük arşiv konumlardan otomatik günlük alma özelliğine sahip yapılandırmanızı öneririz. Birincil ve hazır bekleyen veritabanı günlük arşiv dosyaları ya da hangi veritabanı birine örnekleri günlük dosyalarını arşivlemek tüm günlük arşiv konumlarda alamadı olması gerekir. 

Günlük arşivleme, yalnızca birincil veritabanı tarafından gerçekleştirilir. Veritabanı sunucuları HADR rollerini değiştirmek veya bir hata oluşursa, yeni birincil veritabanı günlük Arşivlenmeye sorumludur. Birden çok günlük arşiv konumları ayarladıysanız, günlüklerinizi iki kez arşivlenmiş. Yerel veya uzak yakalama durumunda Arşivlenmiş günlükleri eski birincil sunucudan yeni birincil sunucu etkin günlük konumuna el ile kopyalamak olabilir.

Yaygın bir NFS paylaşımına veya her iki düğümü günlüklerinin yazılacağı GlusterFS, yapılandırma öneririz. NFS paylaşım veya GlusterFS yüksek oranda kullanılabilir olmalıdır. 

Mevcut yüksek oranda kullanılabilir NFS paylaşımlarını veya GlusterFS taşımalar veya profili dizin için kullanabilirsiniz. Daha fazla bilgi için bkz.

- [SAP NetWeaver için Red Hat Enterprise Linux’taki Azure VM’leri üzerinde GLusterFS][glusterfs] 
- [Red Hat Enterprise Linux ile SAP uygulamaları için Azure NetApp dosya çubuğunda Azure vm'lerinde SAP NetWeaver için yüksek kullanılabilirlik][anf-rhel]
- [Azure NetApp dosyaları](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction) (NFS paylaşımlarını oluşturmak için)

## <a name="test-the-cluster-setup"></a>Test kümesi Kurulumu

Bu bölümde, Db2 HADR kurulumunuzu nasıl sınayıp doğrulayabileceğiniz açıklanır. IBM Db2 birincil üzerinde çalışan her test varsayar *az idb01* sanal makine. Sudo ayrıcalıkları veya kök (önerilmez) ile kullanıcı kullanılması gerekir.

Tüm test çalışmalarını ilk durumu aşağıda açıklanmıştır: (crm_mon - r veya bilgisayarları durumu)

- **bilgisayarları durumu** yürütme zamanında Pacemaker durumunun bir anlık görüntü 
- **crm_mon - r** sürekli Pacemaker durumu çıktısı

<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb01
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb01 ]
     Slaves: [ az-idb02 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb01
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb01

Daemon Status:
  corosync: active/disabled
  pacemaker: active/disabled
  pcsd: active/enabled
</code></pre>

Bir SAP sistemiyle özgün durumu işlem DBACOCKPIT belgelenen > yapılandırma > genel bakış, aşağıdaki görüntüde gösterildiği gibi:

![DBACockpit - öncesi geçiş](./media/high-availability-guide-rhel-ibm-db2-luw/hadr-sap-mgr-org-rhel.png)




### <a name="test-takeover-of-ibm-db2"></a>IBM Db2'in test devralma


> [!IMPORTANT] 
> Test başlamadan önce emin olun:
> * Pacemaker başarısız tüm eylemler (bilgisayarlar durumu) sahip değil.
> * Hiçbir konum kısıtlamaları (parçalarla geçiş testi) vardır.
> * IBM Db2 HADR eşitleme çalışmaktadır. Denetleme ile kullanıcı db2\<SID > <pre><code>db2pd -hadr -db \<DBSID></code></pre>


Aşağıdaki komutu yürüterek çalıştıran birincil Db2 veritabanı düğümü Geçir:
<pre><code>sudo pcs resource move Db2_HADR_<b>ID2</b>-master</code></pre>

Geçiş yapıldıktan sonra crm durumu çıktısı şuna benzer:
<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb01
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb02 ]
     Stopped: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb02
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb02

</code></pre>

Bir SAP sistemiyle özgün durumu işlem DBACOCKPIT belgelenen > yapılandırma > genel bakış, aşağıdaki görüntüde gösterildiği gibi:

![DBACockpit - geçiş sonrası](./media/high-availability-guide-rhel-ibm-db2-luw/hadr-sap-mgr-post-rhel.png)

"Bilgisayarlar kaynak taşıma" ile geçiş kaynak konum kısıtlamaları oluşturur. Konum kısıtlamaları az idb01 üzerinde çalışan bir IBM Db2 örneği bu durumda engelliyor. Konum kısıtlamaları silinmezse kaynak yeniden çalışma özelliğini kullanamazsınız.

Konumu kısıtlamak Kaldır ve bekleme düğüm üzerinde az idb01 başlatılacak.
<pre><code>sudo pcs resource clear Db2_HADR_<b>ID2</b>-master</code></pre>
Ve küme durumunu değiştirir:
<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb01
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb02 ]
     Slaves: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb02
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb02</code></pre>

![Kaldırılan konumu DBACockpit - oranları](./media/high-availability-guide-rhel-ibm-db2-luw/hadr-sap-mgr-clear-rhel.png)


Kaynak geçişi geri *az idb01* konum kısıtlamaları temizleyin
<pre><code>sudo pcs resource move Db2_HADR_<b>ID2</b>-master az-idb01
sudo pcs resource clear Db2_HADR_<b>ID2</b>-master
</code></pre>

- **bilgisayarlar kaynak taşıma \<res_name > <host>:** Konum kısıtlamaları oluşturur ve devralma ile sorunlara neden olabilir
- **bilgisayarlar kaynak Temizle \<res_name >** : Konum kısıtlamaları temizler
- **bilgisayarlar kaynak Temizleme \<res_name >** : Kaynağın tüm hataları temizler

### <a name="test-a-manual-takeover"></a>Testi el ile bir devralma

El ile bir devralma üzerinde Pacemaker hizmetini durdurarak sınayabilirsiniz *az idb01* düğüm:
<pre><code>systemctl stop pacemaker</code></pre>

Durum *az ibdb02*
<pre><code>2 nodes configured
5 resources configured

Node az-idb01: pending
Online: [ az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb02 ]
     Stopped: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb02
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb02

Daemon Status:
  corosync: active/disabled
  pacemaker: active/disabled
  pcsd: active/enabled</code></pre>

Yük devretmeden sonra hizmeti yeniden üzerinde başlatabilirsiniz *az idb01*.
<pre><code>systemctl start  pacemaker</code></pre>


### <a name="kill-the-db2-process-on-the-node-that-runs-the-hadr-primary-database"></a>HADR birincil veritabanı çalışan düğümü üzerindeki Db2 işlemini sonlandır

<pre><code>#Kill main db2 process - db2sysc
[sapadmin@az-idb02 ~]$ sudo ps -ef|grep db2sysc
db2ptr    34598  34596  8 14:21 ?        00:00:07 db2sysc 0
[sapadmin@az-idb02 ~]$ sudo kill -9 34598
</code></pre>

Db2 örneği başarısız olur ve Pacemaker ana düğümü ve durumu aşağıdaki rapor taşır:

<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb02 ]
     Stopped: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb02
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb02

Failed Actions:
* Db2_HADR_ID2_demote_0 on az-idb01 'unknown error' (1): call=49, status=complete, exitreason='none',
    last-rc-change='Wed Jun 26 09:57:35 2019', queued=0ms, exec=362ms</code></pre>

Pacemaker Db2 birincil veritabanı örneği aynı düğüm üzerinde yeniden başlatın veya ikincil veritabanı örneğinin çalıştığı düğüm üzerinde başarısız olur ve bir hata bildirilir.

### <a name="kill-the-db2-process-on-the-node-that-runs-the-secondary-database-instance"></a>İkincil veritabanı örneğini çalıştıran düğümde Db2 işlemini sonlandır

<pre><code>[sapadmin@az-idb02 ~]$ sudo ps -ef|grep db2sysc
db2id2    23144  23142  2 09:53 ?        00:00:13 db2sysc 0
[sapadmin@az-idb02 ~]$ sudo kill -9 23144
</code></pre>

Düğüm içinde belirtilen başarısız ve bir hata bildirdi
<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb01 ]
     Slaves: [ az-idb02 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb01
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb01

Failed Actions:
* Db2_HADR_ID2_monitor_20000 on az-idb02 'not running' (7): call=144, status=complete, exitreason='none',
    last-rc-change='Wed Jun 26 10:02:09 2019', queued=0ms, exec=0ms</code></pre>

Db2 örnek önce atanmış ikincil roldeki yeniden.

### <a name="stop-db-via-db2stop-force-on-the-node-that-runs-the-hadr-primary-database-instance"></a>Durdurma DB db2stop üzerinden HADR birincil veritabanı örneğini çalıştıran düğümde zorla

Kullanıcı db2 olarak\<SID > db2stop force komutunu yürütün:
<pre><code>az-idb01:db2ptr> db2stop force</code></pre>

Hatası algılandı:

<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Slaves: [ az-idb02 ]
     Stopped: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Stopped
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Stopped

Failed Actions:
* Db2_HADR_ID2_demote_0 on az-idb01 'unknown error' (1): call=110, status=complete, exitreason='none',
    last-rc-change='Wed Jun 26 14:03:12 2019', queued=0ms, exec=355ms</code></pre>

Db2 HADR ikincil veritabanı birincil rolünde yükseltilen.
<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb02 ]
     Slaves: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb02
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb02

Failed Actions:
* Db2_HADR_ID2_demote_0 on az-idb01 'unknown error' (1): call=110, status=complete, exitreason='none',
    last-rc-change='Wed Jun 26 14:03:12 2019', queued=0ms, exec=355ms</pre></code>


### <a name="crash-the-vm-that-runs-the-hadr-primary-database-instance-with-halt"></a>Çökme ile "durdurmak" HADR birincil veritabanı örneği çalıştıran VM

<pre><code>#Linux kernel panic. 
sudo echo b > /proc/sysrq-trigger</code></pre>

Böyle bir durumda, birincil veritabanı örneğini çalıştıran düğümün yanıt vermiyor Pacemaker algılar.

<pre><code>2 nodes configured
5 resources configured

Node az-idb01: UNCLEAN (online)
Online: [ az-idb02 ]

Full list of resources:

rsc_st_azure    (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb01 ]
     Slaves: [ az-idb02 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb01
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb01</code></pre>

Denetlenecek sonraki adımdır bir *bölme beyin* durumu. Bir yük devretme kaynakların, kalan düğümü en son birincil veritabanı örneğinde çalışan düğümü kapalı olduğunu belirledi sonra yürütülür.

<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb02 ]
OFFLINE: [ az-idb01 ]

Full list of resources:

rsc_st_azure    (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb02 ]
     Stopped: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb02
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb02 </code></pre>


Çekirdek Panik olması durumunda, başarısız olan düğümün çitlemek aracısı tarafından restared. Başarısız olan düğümün yeniden çevrimiçi olduktan sonra pacemaker küme tarafından başlamalıdır
<pre><code>sudo pcs cluster start</code></pre> ikincil rolü Db2 örneğine başlar.

<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

rsc_st_azure    (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb02 ]
     Slaves: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb02
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb02</code></pre>

## <a name="next-steps"></a>Sonraki adımlar
- [Yüksek kullanılabilirlik mimarisi ve senaryolar için SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios)
- [SLES azure'da Red Hat Enterprise Linux üzerinde Pacemaker ayarlama][rhel-pcs-azr]
