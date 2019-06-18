---
title: Azure için SAP LaMa Bağlayıcısı | Microsoft Docs
description: SAP LaMa kullanarak azure'da SAP sistemlerini yönetme
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: MSSedusch
manager: timlt
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/17/2018
ms.author: sedusch
ms.openlocfilehash: f09f66e81ec4878aedebfee9be4c0c67b75c8ad6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61463030"
---
# <a name="sap-lama-connector-for-azure"></a>Azure için SAP LaMa bağlayıcısı

[1877727]: https://launchpad.support.sap.com/#/notes/1877727
[2343511]: https://launchpad.support.sap.com/#/notes/2343511
[2350235]: https://launchpad.support.sap.com/#/notes/2350235
[2562184]: https://launchpad.support.sap.com/#/notes/2562184
[2628497]: https://launchpad.support.sap.com/#/notes/2628497
[2445033]: https://launchpad.support.sap.com/#/notes/2445033
[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png
[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md
[hana-ops-guide]:hana-vm-operations.md

> [!NOTE]
> Genel destek bildirimi: SAP LaMa veya Azure Bağlayıcısı için desteğe ihtiyacınız varsa lütfen her zaman bir olay ile SAP VCM LVM HYPERV BC bileşende açın.

SAP LaMa çoğu, çalıştırmak ve bunların SAP ortamı izlemek için kullanılır. SAP LaMa 3.0 SP05 beri varsayılan olarak Azure için bir bağlayıcı ile birlikte gelir. Bu bağlayıcı, serbest bırakın ve sanal makineleri başlatmanız, kopyalayın ve yönetilen diskler dışında yeniden konumlandırmakta ve yönetilen diskleri silmek için kullanabilirsiniz. Bu temel işlemleri ile dışında yeniden konumlandırmakta, kopyalama, kopyalama ve SAP LaMa kullanarak SAP sistemlerini yenileyin.

Bu kılavuz için SAP LaMa Azure Bağlayıcısı'nı nasıl ayarlanacağını açıklar, Uyarlamalı SAP sistemlerini ve nasıl yapılandırılacakları yüklemek için kullanılan sanal makineleri oluşturun.

> [!NOTE]
> Bağlayıcı yalnızca SAP LaMa Enterprise Edition'da kullanılabilir

## <a name="resources"></a>Kaynaklar

Aşağıdaki SAP notları konu, Azure üzerinde SAP LaMa ilgili:

| Not numarası | Unvan |
| --- | --- |
| [2343511] |SAP yatay Yönetim (LaMa) için Microsoft Azure Bağlayıcısı |
| [2350235] |Landscape yönetim 3.0 - Enterprise edition SAP |

Ayrıca okuma [SAP Yardım portalı için SAP LaMa](https://help.sap.com/viewer/p/SAP_LANDSCAPE_MANAGEMENT_ENTERPRISE).

## <a name="general-remarks"></a>Genel açıklamalar

* Etkinleştirdiğinizden emin olun *otomatik Mountpoint oluşturma* kurulumunda -> Ayarlar -> altyapısı  
  SAP LaMa SAP Uyarlamalı Uzantıları'nı kullanarak bir sanal makinede birimleri bağlar, bu ayar etkin değilse, bağlama noktasının mevcut olması gerekir.

* Ayrı bir alt ağ kullanın ve SAP örnekleri hazırlıksız IP önlemenin kullanımı dinamik IP adresleri "Yeni sanal makineler dağıtırken çalmak" adres yok  
  Ayrıca SAP LaMa tarafından kullanılan alt ağdaki dinamik IP adresi ayırma kullanırsanız, bir SAP sistemiyle ile SAP LaMa hazırlama başarısız olabilir. Bir SAP sistemiyle hazır değil ise, IP adreslerinin ayrılmış değildir ve diğer sanal makinelere ayrılan.

* Yönetilen konaklar için oturum açın, dosya sistemlerinin bağlı olmayan engellemediğinizden emin olun.  
  Bir Linux sanal makinelerinde oturum açın ve değişiklik bir dizinde bağlama noktası, örneğin /usr/sap/AH1/ASCS00/exe, birim için çalışma dizini kaldırılan olamaz ve yeniden Yerleştir veya unprepare başarısız olur.

## <a name="set-up-azure-connector-for-sap-lama"></a>SAP LaMa için Azure Bağlayıcısı'nı Ayarla

Azure bağlayıcısı SAP LaMa 3.0 SP05 itibarıyla sevk edilir. Her zaman SAP LaMa 3.0 için düzeltme eki ve en son destek paketini yüklemeniz önerilir. Azure bağlayıcı, Microsoft Azure karşı korunmasına yetki vermek için bir hizmet sorumlusu kullanır. SAP yatay Yönetim (LaMa) için bir hizmet sorumlusu oluşturmak için aşağıdaki adımları izleyin.

1. Şuraya gidin: https://portal.azure.com
1. Azure Active Directory dikey penceresini açın
1. Uygulama kayıtları tıklayın
1. Ekle'ye tıklayın
1. Bir ad girin, "Web uygulaması/API'si" uygulama türünü seçin, bir oturum açma URL'sini girin (örneğin http:\//localhost) ve üzerinde oluştur
1. Oturum açma URL'si kullanılmaz ve geçerli bir URL olabilir
1. Yeni uygulamayı seçin ve ayarları sekmesini anahtarları
1. Yeni bir anahtar için bir açıklama girin, "Her zaman geçerli olsun"'i seçin ve tıklayın kaydederken
1. Değeri yazın. Hizmet sorumlusu parolası olarak kullanılır
1. Uygulama Kimliği yazma Hizmet sorumlusu, kullanıcı adı olarak kullanılır

Hizmet sorumlusu kullanarak Azure kaynaklarınızı varsayılan olarak erişim izni yok. Bunlara erişmek için hizmet sorumlusu izinleri vermeniz gerekir.

1. Şuraya gidin: https://portal.azure.com
1. Kaynak grupları dikey penceresini açın
1. Kullanmak istediğiniz kaynak grubunu seçin
1. Erişim denetimi (IAM)'ye tıklayın.
1. Rol ataması Ekle'e tıklayın
1. Katkıda bulunan rolü seçin
1. Yukarıda oluşturduğunuz uygulamanın adını girin
1. Kaydet’e tıklayın.
1. Adım 3'ten 8 SAP LaMa içinde kullanmak istediğiniz tüm kaynak grupları için yineleyin

SAP LaMa Web sitesini açın ve altyapı için gidin. Bulut yöneticileri sekmesine gidin ve Ekle'ye tıklayın. Microsoft Azure bulut bağdaştırıcıyı seçin ve İleri'ye tıklayın. Aşağıdaki bilgileri girin:

* Etiket: Bağlayıcı örneği için bir ad seçin
* Kullanıcı adı: Hizmet Sorumlusu Uygulama Kimliği
* Parola: Hizmet sorumlusu anahtarı/parolası
* URL: Varsayılan koruyun https://management.azure.com/
* İzleme aralığı (saniye): En az 300 olmalıdır
* Abonelik kimliği: Azure abonelik kimliği
* Azure Active Directory Kiracı kimliği: Active Directory kiracısının kimliği
* Proxy ana bilgisayarı: SAP LaMa internet'e bağlanmak için Ara sunucu gerekiyorsa proxy konak adı
* Proxy bağlantı noktası: Proxy TCP bağlantı noktası

Girişinizi doğrulamak için test yapılandırmasına tıklayın. Görmeniz gerekir

Bağlantı başarılı: Microsoft bulut bağlantı başarılı oldu. (yalnızca 10 grupları istenen) 7 kaynak grupları bulunamadı

Web sitesinin en altında.

## <a name="provision-a-new-adaptive-sap-system"></a>Yeni bir Uyarlamalı SAP sistemi sağlayın

El ile yeni bir sanal makine dağıtma veya Azure şablonlarında birini [hızlı depo](https://github.com/Azure/azure-quickstart-templates). Şablonları içerdiği [SAP NetWeaver ASCS](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-lama-ascs), [SAP NetWeaver uygulama sunucuları](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-lama-apps)ve [veritabanı](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-lama-database). Bu şablonlar, sistem kopyalama/kopya vb. bir parçası olarak yeni konaklarını sağlamak için de kullanabilirsiniz.

Ayrı bir alt ağ için tüm sanal SAP LaMa ile yönetmek istediğiniz ve IP adresi "Yeni sanal makineler dağıtırken çalarak" önlemek için dinamik IP adresi kullanmayın ve SAP örnekleri hazırlıksız başladığınız makineleri kullanmanızı öneririz.

> [!NOTE]
> Disk bir sanal makineden ayırmak için uzun çalışma zamanları neden olabileceğinden, mümkün olduğunda, tüm sanal makine uzantıları kaldırın.

Bu kullanıcı emin \<hanasid > adm, \<sapsid > adm ve Grup sapsys aynı kimliği ve GID hedef makinede mevcut veya LDAP kullanın. Etkinleştirir ve SAP NetWeaver (A) SCS çalıştırmak için kullanılması gereken sanal makineleri NFS sunucusu başlatır.

### <a name="manual-deployment"></a>El ile dağıtım

SAP LaMa SAP konak Aracısı'nı kullanarak sanal makine ile iletişim kurar. El ile veya değil, hızlı başlangıç depodan Azure Resource Manager şablonu kullanarak sanal makineler dağıtırsanız, en son SAP konak aracısı ve SAP Uyarlamalı uzantıları yüklemek emin olun. Azure için gerekli bir düzeltme eki düzeyleri hakkında daha fazla bilgi için bkz. Not SAP [2343511].

#### <a name="manual-deployment-of-a-linux-virtual-machine"></a>El ile dağıtımı, bir Linux sanal makinesi

SAP notu içinde listelenen desteklenen işletim sistemi biri ile yeni bir sanal makine oluşturma [2343511]. SAP örnekleri için ek IP yapılandırmaları ekleyin. Her örneği, IP adresi üzerinde en az ve bir sanal ana bilgisayar adı kullanılarak yüklenmesi gerekir.

SAP NetWeaver ASCS örneği için /sapmnt/ diske ihtiyacı\<SAPSID >, / usr/sap/\<SAPSID >, /usr/sap/transve/usr/sap/\<sapsid > adm. SAP NetWeaver uygulama sunucularına ek diskler gerekmez. SAP örneğine ilgili her şeyi gerekir ASCS üzerinde depolanabilir ve NFS dışarı aktarılan. Aksi takdirde, şu anda SAP LaMa kullanarak ek uygulama sunucuları eklemek mümkün değildir.

![Linux'ta SAP NetWeaver ASCS](media/lama/sap-lama-ascs-app-linux.png)

#### <a name="manual-deployment-for-sap-hana"></a>SAP HANA için el ile dağıtım

Yeni bir sanal makine desteklenmeyen bir işlem sistemlerinden biri ile SAP HANA için SAP Not listelendiği gibi oluşturma [2343511]. SAP HANA için ek bir IP yapılandırmasına ve HANA Kiracı başına ekleyin.

SAP HANA diskleri /hana/shared, /hana/backup, /hana/data ve /hana/log gerekiyor

![Linux üzerinde SAP HANA](media/lama/sap-lama-db-hana.png)

#### <a name="manual-deployment-for-oracle-database-on-linux"></a>Linux üzerinde Oracle veritabanı için el ile dağıtım

SAP Not listelendiği gibi Oracle veritabanları için desteklenen işletim sistemi biri ile yeni bir sanal makine oluşturma [2343511]. Oracle veritabanı için ek bir IP yapılandırmasına ekleyin.

Oracle veritabanı diskleri /oracle, /home/oraod1 ve /home/oracle gerekiyor

![Linux üzerinde Oracle veritabanı](media/lama/sap-lama-db-ora-lnx.png)

#### <a name="manual-deployment-for-microsoft-sql-server"></a>Microsoft SQL Server için el ile dağıtım

Yeni bir sanal makine desteklenmeyen bir işlem sistemlerinden biri ile Microsoft SQL Server için SAP Not listelendiği gibi oluşturma [2343511]. SQL Server örneği için ek bir IP yapılandırmasına ekleyin.

SQL Server veritabanı sunucusu, veritabanı veri ve günlük dosyaları ve c:\usr\sap diskleri için diskler gerekir.

![Linux üzerinde Oracle veritabanı](media/lama/sap-lama-db-sql.png)

SAP NetWeaver uygulama sunucusu veya bir sistem kopyalama/kopyalama hedefi olarak yeniden konumlandırmak için kullanmak istediğiniz bir sanal makinede SQL Server için desteklenen bir Microsoft ODBC sürücüsü yüklediğinizden emin olun.

Önceden SQL Server veritabanı örneği için veya bir sistem kopyalama/kopyalama hedefi olarak yeniden konumlandırmak için kullanmak istediğiniz bir sanal makine gerekir SAP LaMa SQL sunucusunun kendisi yerini değiştiremezsiniz.

### <a name="deploy-virtual-machine-using-an-azure-template"></a>Azure şablonu kullanarak sanal makine dağıtma

Aşağıdaki en son kullanılabilir arşivler öğesinden indirme [SAP yazılım Market](https://support.sap.com/swdc) sanal makinelerin işletim sistemi için:

1. SAPCAR 7.21
1. SAP HOST AGENT 7.21
1. SAP UYARLAMALI UZANTISI 1.0 EXT

Ayrıca aşağıdaki bileşenlerden indirme [Microsoft Download Center](https://www.microsoft.com/download)

1. Microsoft Visual C++ 2010 yeniden dağıtılabilir paketi (x64) (yalnızca Windows)
1. SQL Server (yalnızca SQL Server) için Microsoft ODBC sürücüsü

Bileşenler, şablonu dağıtmak için gereklidir. Şablonu kullanılabilir hale getirmek için en kolay yolu, bunları bir Azure depolama hesabına yükleyin ve bir paylaşılan erişim imzası (SAS) oluşturmaktır.

Şablon aşağıdaki parametreleri vardır:

* sapSystemId: SAP sistem kimliği Disk düzeni oluşturmak için kullanılır (örneğin/usr/sap/\<sapsid >).

* computerName: Yeni sanal makinenin bilgisayar adı. Bu parametre, SAP LaMa tarafından da kullanılır. Bir sistem kopyalama işleminin bir parçası olarak yeni bir sanal makine sağlamak için bu şablonu kullandığınızda, bu bilgisayar adı olan ana bilgisayara ulaşılana kadar SAP LaMa bekler.

* osType: Dağıtmak istediğiniz işletim sistemi türü.

* DbType: Veritabanı türü. Bu parametre, ne kadar ek IP yapılandırmaları eklenmesi gerekir ve nasıl disk düzeni görünmesi gerektiğini belirlemek için kullanılır.

* sapSystemSize: Dağıtmak istediğiniz SAP sistemine boyutu. Sanal makine örneği türünü ve boyutunu belirlemek için kullanılır.

* adminUsername: Sanal makine için kullanıcı adı.

* adminPassword: Sanal makinenin parolası. SSH için bir ortak anahtar de sağlayabilirsiniz.

* sshKeyData: Sanal makineler için SSH ortak anahtarı. Yalnızca Linux işletim sistemleri için desteklenir.

* subnetId: Kullanmak istediğiniz alt ağ kimliği.

* deployEmptyTarget: Sanal makine veya benzer bir örneği yeniden Yerleştir için yalnızca bir hedef olarak kullanmak istiyorsanız, boş bir hedef dağıtabilirsiniz. Bu durumda, hiçbir ek diskler veya IP yapılandırmalarına eklenir.

* sapcarLocation: Dağıttığınız işletim sistemiyle eşleşen sapcar uygulaması için konum. sapcar, diğer parametre sağladığınız arşivleri ayıklamak için kullanılır.

* sapHostAgentArchiveLocation: SAP konak Aracısı Arşiv dosyasının konumu. SAP konak Aracısı, bu şablon dağıtımının bir parçası dağıtılır.

* sapacExtLocation: SAP Uyarlamalı uzantıları konumu. SAP notu [2343511] Azure için gerekli en düşük düzeltme eki düzeyi listeler.

* vcRedistLocation: SAP Uyarlamalı uzantıları yüklemek için gerekli VC çalışma zamanının konumu. Bu parametre yalnızca Windows için gereklidir.

* odbcDriverLocation: ODBC sürücüsü yüklemek istediğiniz konumu. Yalnızca SQL Server için Microsoft ODBC sürücüsü desteklenir.

* sapadmPassword: Sapadm kullanıcı parolası.

* sapadmId: Linux kullanıcı sapadm kullanıcının kimliği. Windows için gerekli değildir.

* sapsysGid: Linux grubu sapsys grubun kimliği. Windows için gerekli değildir.

* _artifactsLocation: Bu şablon için gerekli yapıtları bulunduğu yere taban URI. Eşlik eden komut dosyalarını kullanarak şablon dağıtıldığında, abonelik özel bir konumda kullanılır ve bu değeri otomatik olarak oluşturulur. Github'dan şablon dağıtma değil, yalnızca gerekli.

* _artifactsLocationSasToken: _ArtifactsLocation erişmek için gerekli sasToken. Eşlik eden komut dosyalarını kullanarak şablon dağıtıldığında bir sasToken otomatik olarak oluşturulur. Github'dan şablon dağıtma değil, yalnızca gerekli.

### <a name="sap-hana"></a>SAP HANA

Aşağıdaki örneklerde, kimliği HN1 ve sistem kimliği AH1 ile SAP NetWeaver sistemi ile SAP HANA yüklediğiniz varsayılır. Sanal ana bilgisayar adları hn1-db ah1-db SAP NetWeaver sistemi ah1 ascs, SAP NetWeaver ASCS için ve di ah1 0 için ilk SAP NetWeaver uygulama sunucusu tarafından kullanılan HANA Kiracı için HANA örneği için ' dir.

#### <a name="install-sap-netweaver-ascs-for-sap-hana"></a>SAP HANA için SAP NetWeaver ASCS yükleyin

SAP yazılım sağlama Yöneticisi (SWPM) başlamadan önce sanal ana bilgisayar adını ASCS IP adresini bağlamak gerekir. Önerilen yöntem sapacext kullanmaktır. Sapacext kullanarak IP adresini bağlarsanız, IP adresi yeniden başlatma sonrası yeniden bağlamaya yönelik emin olun.

![Linux][Logo_Linux] Linux

```bash
# /usr/sap/hostctrl/exe/sapacext -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
/usr/sap/hostctrl/exe/sapacext -a ifup -i eth0 -h ah1-ascs -n 255.255.255.128
```

![Windows][Logo_Windows] Windows

```bash
# C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i "Ethernet 3" -h ah1-ascs -n 255.255.255.128
```

SWPM çalıştırın ve *ah1 ascs* için *ASCS örneği ana bilgisayar adı*.

![Linux][Logo_Linux] Linux  
Aşağıdaki profil parametresi /usr/sap/hostctrl/exe/host_profile bulunduğu SAP konak Aracısı profili ekleyin. Daha fazla bilgi için bkz. Not SAP [2628497].
```
acosprep/nfs_paths=/home/ah1adm,/usr/sap/trans,/sapmnt/AH1,/usr/sap/AH1
```

#### <a name="install-sap-hana"></a>SAP HANA yükleme

Komut satırı aracı hdblcm kullanılarak SAP HANA'ya yüklerseniz, bir sanal ana bilgisayar adı sağlamak için parametre--ana bilgisayar adı kullanın. Bir ağ arabirimi için IP adresini sanal ana bilgisayar veritabanı adını eklemek gerekir. Önerilen yöntem sapacext kullanmaktır. Sapacext kullanarak IP adresini bağlarsanız, IP adresi yeniden başlatma sonrası yeniden bağlamaya yönelik emin olun.

Başka bir sanal ana bilgisayar adı ve IP adresi uygulama sunucuları tarafından HANA kiracıya bağlanmak için kullanılan ad.

```bash
# /usr/sap/hostctrl/exe/sapacext -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
/usr/sap/hostctrl/exe/sapacext -a ifup -i eth0 -h hn1-db -n 255.255.255.128
/usr/sap/hostctrl/exe/sapacext -a ifup -i eth0 -h ah1-db -n 255.255.255.128
```

Veritabanı örneğinin yüklenmesini SWPM HANA sanal makine değil, uygulama sunucusu sanal makinede çalıştırın. Kullanım *ah1 db* için *veritabanı konak* iletişim kutusunda *SAP sistemine için veritabanı*.

#### <a name="install-sap-netweaver-application-server-for-sap-hana"></a>SAP HANA için SAP NetWeaver uygulama sunucusu yükleme

SAP yazılım sağlama Yöneticisi (SWPM) başlamadan önce sanal ana bilgisayar adı uygulama sunucusunun IP adresini bağlamak gerekir. Önerilen yöntem sapacext kullanmaktır. Sapacext kullanarak IP adresini bağlarsanız, IP adresi yeniden başlatma sonrası yeniden bağlamaya yönelik emin olun.

![Linux][Logo_Linux] Linux

```bash
# /usr/sap/hostctrl/exe/sapacext -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
/usr/sap/hostctrl/exe/sapacext -a ifup -i eth0 -h ah1-di-0 -n 255.255.255.128
```

![Windows][Logo_Windows] Windows

```bash
# C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i "Ethernet 3" -h ah1-di-0 -n 255.255.255.128
```

SAP NetWeaver profili parametresi dbs/hdb/hdb_use_ident içinde HDB userstore anahtarını bulmak için kullanılan kimlik kullanmak için önerilir. Bu parametre, SWPM ile veritabanı örneği yüklemeden sonra el ile ekleyebilir veya SWPM ile çalıştırma

```bash
# from https://blogs.sap.com/2015/04/14/sap-hana-client-software-different-ways-to-set-the-connectivity-data/
/sapdb/DVDs/IM_LINUX_X86_64/sapinst HDB_USE_IDENT=SYSTEM_COO
```

El ile ayarlarsanız, ayrıca yeni HDB userstore girdileri oluşturmanız gerekir.

```bash
# run as <sapsid>adm
/usr/sap/AH1/hdbclient/hdbuserstore LIST
# reuse the port that was listed from the command above, in this example 35041
/usr/sap/AH1/hdbclient/hdbuserstore SET DEFAULT ah1-db:35041@AH1 SAPABAP1 <password>
```

Kullanım *ah1 di 0* için *PA'lar örneği ana bilgisayar adı* iletişim kutusunda *birincil uygulama sunucusu örneği*.

#### <a name="post-installation-steps-for-sap-hana"></a>SAP HANA için yükleme sonrası adımlar

Kiracı kopyalamayı yapmak için taşıma Kiracı veya sistem çoğaltma oluşturma denemeden önce SYSTEMDB ve tüm Kiracı veritabanlarında yedekleme emin olun.

### <a name="microsoft-sql-server"></a>Microsoft SQL Server

Aşağıdaki örneklerde, SAP NetWeaver sistem kimliği AS1 sistemiyle yükleme varsayıyoruz. Sanal ana bilgisayar adları as1-db SAP NetWeaver sistemi as1 ascs, SAP NetWeaver ASCS için ve di as1 0 için ilk SAP NetWeaver uygulama sunucusu tarafından kullanılan SQL Server örneği için ' dir.

#### <a name="install-sap-netweaver-ascs-for-sql-server"></a>SQL Server için SAP NetWeaver ASCS yükleyin

SAP yazılım sağlama Yöneticisi (SWPM) başlamadan önce sanal ana bilgisayar adını ASCS IP adresini bağlamak gerekir. Önerilen yöntem sapacext kullanmaktır. Sapacext kullanarak IP adresini bağlarsanız, IP adresi yeniden başlatma sonrası yeniden bağlamaya yönelik emin olun.

```bash
# C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i "Ethernet 3" -h as1-ascs -n 255.255.255.128
```

SWPM çalıştırın ve *as1 ascs* için *ASCS örneği ana bilgisayar adı*.

#### <a name="install-sql-server"></a>SQL Server yükleme

Bir ağ arabirimi için IP adresini sanal ana bilgisayar veritabanı adını eklemek gerekir. Önerilen yöntem sapacext kullanmaktır. Sapacext kullanarak IP adresini bağlarsanız, IP adresi yeniden başlatma sonrası yeniden bağlamaya yönelik emin olun.

```bash
# C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i "Ethernet 3" -h as1-db -n 255.255.255.128
```

SQL server sanal makinesinde SWPM veritabanı örneği yüklemesini çalıştırın. SAPINST_USE_HOSTNAME kullanın =*as1 db* SQL Server'a bağlanmak için kullanılan ana bilgisayar adı geçersiz kılmak için. Azure Resource Manager şablonu kullanarak sanal makineyi dağıttığınız, için veritabanı veri dosyaları için kullanılacak dizini ayarladığınızdan emin olun *C:\sql\data* ve veritabanı günlük dosyasına *C:\sql\log*.

Emin olun kullanıcı *NT AUTHORITY\SYSTEM* SQL Server'a erişimi olan ve sunucu rolü *sysadmin*. Daha fazla bilgi için bkz. Not SAP [1877727] ve [2562184].

#### <a name="install-sap-netweaver-application-server"></a>SAP NetWeaver uygulama sunucusu yükleme

SAP yazılım sağlama Yöneticisi (SWPM) başlamadan önce sanal ana bilgisayar adı uygulama sunucusunun IP adresini bağlamak gerekir. Önerilen yöntem sapacext kullanmaktır. Sapacext kullanarak IP adresini bağlarsanız, IP adresi yeniden başlatma sonrası yeniden bağlamaya yönelik emin olun.

```bash
# C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i "Ethernet 3" -h as1-di-0 -n 255.255.255.128
```

Kullanım *as1 di 0* için *PA'lar örneği ana bilgisayar adı* iletişim kutusunda *birincil uygulama sunucusu örneği*.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="errors-and-warnings-during-discover"></a>Hataları ve Uyarıları bulma sırasında

* SELECT izni reddedildi
  * [Microsoft] [ODBC SQL Server sürücüsü] [SQL Server] SELECT izni reddedildi; nesne 'log_shipping_primary_databases', 'msdb' veritabanı, şema 'dbo'. [SOAPFaultException]  
  SELECT izni reddedildi; nesne 'log_shipping_primary_databases', 'msdb' veritabanı, şema 'dbo'.
  * Çözüm  
    Emin olun *NT AUTHORITY\SYSTEM* SQL Server erişebilirsiniz. ' Lu SAP notuna bakın [2562184]


### <a name="errors-and-warnings-for-instance-validation"></a>Hatalar ve Uyarılar için örnek doğrulama

* HDB userstore doğrulamada bir özel durum oluştu  
  * Günlük Görüntüleyici bakın  
    com.sap.nw.lm.aci.monitor.api.validation.RuntimeValidationException: Özel durum 'RuntimeHDBConnectionValidator' kimlikli doğrulayıcı (doğrulama: 'VALIDATION_HDB_USERSTORE'): Hdbuserstore alınamadı  
    HANA userstore doğru konumda değil
  * Çözüm  
    Bu /usr/sap/AH1/hdbclient/install/installation.ini doğru olduğundan emin olun

### <a name="errors-and-warnings-during-a-system-copy"></a>Hataları ve Uyarıları System kopyalama sırasında

* Adım sağlama sistem doğrulanırken bir hata oluştu
  * Neden: com.sap.nw.lm.aci.engine.base.api.util.exception.HAOperationException çağırma ' / usr/sap/hostctrl/exe/sapacext - bir ShowHanaBackups -m HN1 -f 50 - h hn1-db - o düzeyi = 0\;durumu = 5\;bağlantı noktası 35013 pf = / usr/sap/hostctrl = / exe/host_profile -R -T dev_lvminfo -u SYSTEM -p kanca - r' | /usr/SAP/hostctrl/exe/sapacext - bir ShowHanaBackups -m HN1 -f 50 - h hn1-db - o düzeyi = 0\;durumu = 5\;bağlantı noktası 35013 pf = / usr/sap/hostctrl/exe/host_profile = -R -T dev_lvminfo -u SYSTEM -p kanca - r
  * Çözüm  
    HANA sistem kaynağında tüm veritabanlarının yedek Al

* Sistem kopyalama adımı *Başlat* veritabanı örneği
  * Konak aracısı işlemini '000D3A282BC91EE8A1D76CF1F92E2944' başarısız oldu (OperationException. FaultCode: '127', ileti: ' Komutu yürütülemedi. : [Microsoft] [ODBC SQL Server sürücüsünü] [SQL Server] kullanıcı, 'AS2' veritabanını değiştirme izni yok, veritabanı yok veya veritabanı erişim denetimlerine izin veren bir durumda değil.')
  * Çözüm  
    Emin olun *NT AUTHORITY\SYSTEM* SQL Server erişebilirsiniz. ' Lu SAP notuna bakın [2562184]

### <a name="errors-and-warnings-during-a-system-clone"></a>Hataları ve Uyarıları System kopyalama sırasında

* Adımda örneği Aracısı kaydedilmeye çalışılırken hata oluştu *zorunlu kaydetmek ve örnek Aracısı başlangıç* ASCS veya uygulama sunucusu
  * Örnek Aracısı kaydedilmeye çalışılırken hata oluştu. (RemoteException: 'Profilinden örnek verileri yüklemek için başarısız oldu'\\as1 ascs\sapmnt\AS1\SYS\profile\AS1_D00_as1 di 0':  Profiline erişilemiyor '\\as1 ascs\sapmnt\AS1\SYS\profile\AS1_D00_as1 di 0': Böyle dosya veya dizin yok.')
  * Çözüm  
   ASCS/SCS sapmnt paylaşımında SAP_AS1_GlobalAdmin için tam erişime sahip olduğundan emin olun

* Adımda hata *kopya için başlangıç korumayı etkinleştir*
  * Dosya açılamadı '\\as1 ascs\sapmnt\AS1\SYS\profile\AS1_D00_as1 di 0' neden: Böyle bir dosya veya dizin yok
  * Çözüm  
    Uygulama sunucusunun bilgisayar hesabı profili yazma erişimi olmalıdır

### <a name="errors-and-warnings-during-create-system-replication"></a>Hataları ve Uyarıları sırasında sistem çoğaltma oluşturma

* Sistem çoğaltma oluşturma tıklandığında özel durumu
  * Neden: com.sap.nw.lm.aci.engine.base.api.util.exception.HAOperationException çağırma ' / usr/sap/hostctrl/exe/sapacext - bir ShowHanaBackups -m HN1 -f 50 - h hn1-db - o düzeyi = 0\;durumu = 5\;bağlantı noktası 35013 pf = / usr/sap/hostctrl = / exe/host_profile -R -T dev_lvminfo -u SYSTEM -p kanca - r' | /usr/SAP/hostctrl/exe/sapacext - bir ShowHanaBackups -m HN1 -f 50 - h hn1-db - o düzeyi = 0\;durumu = 5\;bağlantı noktası 35013 pf = / usr/sap/hostctrl/exe/host_profile = -R -T dev_lvminfo -u SYSTEM -p kanca - r
  * Çözüm  
    Sapacext olarak yürütülen test `<hanasid`> adm

* Tam kopyalama depolama adımda etkinleştirilmediğinde hata
  * Bir bağlam özniteliği ileti yolu IStorageCopyData.storageVolumeCopyList:1 ve alan targetStorageSystemId bildirirken bir hata oluştu
  * Çözüm  
    Adımda uyarılarını gözardı et ve tekrar deneyin. Yeni bir destek paketi/düzeltme eki içinde SAP LaMa Bu sorun çözülecektir.

### <a name="errors-and-warnings-during-relocate"></a>Hataları ve Uyarıları sırasında yeniden Yerleştir

* Yol '/ usr/sap/AH1' nfs reexports için izin verilmiyor.
  * SAP notu denetleyin [2628497] Ayrıntılar için.
  * Çözüm  
    ASCS ASCS HostAgent profiline dışarı aktaran ekleyin. ' Lu SAP notuna bakın [2628497]

* ASCS yeniden konumlandırma, uygulanmadı işlevi
  * Komut çıktısı: exportfs: ana bilgisayar: / usr/sap/AX1: Uygulanmadı işlevi
  * Çözüm  
    NFS Sunucusu hizmeti yeniden Yerleştir hedef sanal makinede etkin olduğundan emin olun

### <a name="errors-and-warnings-during-application-server-installation"></a>Hataları ve Uyarıları uygulama sunucusu yüklemesi sırasında

* SAPinst adımı yürütülürken hata oluştu: getProfileDir
  * HATA: (Son adımı tarafından bildirilen hata: Yakalanan ESAPinstException modülü çağrısında: Doğrulayıcı adımının ' | NW_DI | ul | ul | ul | ul | 0 | 0 | NW_GetSidFromProfiles | ul | ul | ul | ul | getSid | 0 | NW_readProfileDir | ul | ul | ul | ul | readProfile | 0 | getProfileDir' bir hata bildirdi: Düğüm \\\as1-ascs\sapmnt\AS1\SYS\profile mevcut değil. Bu sorunu çözmek için etkileşimli modda SAPinst Başlangıç)
  * Çözüm  
    SWPM profili erişimi olan bir kullanıcı ile çalıştığından emin olun. Bu kullanıcı Uygulama Sunucusu Kurulum Sihirbazı'nda yapılandırılabilir

* SAPinst adımı yürütülürken hata oluştu: askUnicode
  * HATA: (Son adımı tarafından bildirilen hata: Yakalanan ESAPinstException modülü çağrısında: Doğrulayıcı adımının ' | NW_DI | ul | ul | ul | ul | 0 | 0 | NW_GetSidFromProfiles | ul | ul | ul | ul | getSid | 0 | NW_getUnicode | ul | ul | ul | ul | unicode | 0 | askUnicode' bir hata bildirdi: Bu sorunu çözmek için etkileşimli modda SAPinst Başlangıç)
  * Çözüm  
    Yeni bir SAP çekirdek kullanırsanız SWPM sistem artık ASCS ileti sunucusu kullanılarak bir unicode sistem olup olmadığını belirleyemiyor. ' Lu SAP notuna bakın [2445033] daha fazla ayrıntı için.  
    Yeni bir destek paketi/düzeltme eki içinde SAP LaMa Bu sorun çözülecektir.  
    Profil parametre OS_UNICODE SAP sisteminizin bu sorunu çözmek için varsayılan profilde uc =.

* SAPinst adımı yürütülürken hata oluştu: dCheckGivenServer
  * SAPinst adımı yürütülürken hata oluştu: dCheckGivenServer "Sürüm"1.0"hata =: (Son adımı tarafından bildirilen hata: \<p > yükleme kullanıcı tarafından iptal edildi. \</p >
  * Çözüm  
    SWPM profili erişimi olan bir kullanıcı ile çalıştığından emin olun. Bu kullanıcı Uygulama Sunucusu Kurulum Sihirbazı'nda yapılandırılabilir

* SAPinst adımı yürütülürken hata oluştu: checkClient
  * SAPinst adımı yürütülürken hata oluştu: checkClient "Sürüm"1.0"hata =: (Son adımı tarafından bildirilen hata: \<p > yükleme kullanıcı tarafından iptal edildi. \</p >)
  * Çözüm  
    Sanal uygulama sunucusu yüklemek istediğiniz makinede SQL Server için Microsoft ODBC sürücüsü yüklü olduğundan emin olun

* SAPinst adımı yürütülürken hata oluştu: copyScripts
  * Adımı tarafından bildirilen son hata: Sistem çağrısı başarısız oldu. AYRINTILAR: Hata 13 (0x0000000d) (izni reddedildi) yürütme sisteminin çağrı parametresi ' fopenU' (\\\as1-ascs/sapmnt/AS1/SYS/exe/uc/NTAMD64/strdbs.cmd, w), dosya (\bas/bas/749_REL/bc_749_REL/src/ins/SAPINST/impl/src/syslib satır (494) / filesystem/syxxcfstrm2.cpp), yığın izleme:  
  CThrThread.cpp: 85: CThrThread::threadFunction()  
  CSiServiceSet.cpp: 63: CSiServiceSet::executeService()  
  CSiStepExecute.cpp: 913: CSiStepExecute::execute()  
  EJSController.cpp: 179: EJSControllerImpl::executeScript()  
  JSExtension.hpp: 1136: CallFunctionBase::call()  
  iaxxcfile.cpp: 183: iastring CIaOsFileConnect::callMemberFunction (iastring const & adına args_t const ve args)  
  iaxxcfile.cpp: 1849: iastring CIaOsFileConnect::newFileStream (const args_t & _args)  
  iaxxbfile.cpp: 773: CIaOsFile::newFileStream_impl(4)  
  syxxcfile.cpp: 233: CSyFileImpl::openStream(ISyFile::eFileOpenMode)  
  syxxcfstrm.cpp: 29: CSyFileStreamImpl::CSyFileStreamImpl(CSyFileStream*,iastring,ISyFile::eFileOpenMode)  
  syxxcfstrm.cpp: 265: CSyFileStreamImpl::open()  
  syxxcfstrm2.cpp: 58: CSyFileStream2Impl::CSyFileStream2Impl (const CSyPath & \\\aw1-ascs/sapmnt/AW1/SYS/exe/uc/NTAMD64/strdbs.cmd, 0x4)  
  syxxcfstrm2.cpp: 456: CSyFileStream2Impl::open()
  * Çözüm  
    SWPM profili erişimi olan bir kullanıcı ile çalıştığından emin olun. Bu kullanıcı Uygulama Sunucusu Kurulum Sihirbazı'nda yapılandırılabilir

* SAPinst adımı yürütülürken hata oluştu: askPasswords
  * Adımı tarafından bildirilen son hata: Sistem çağrısı başarısız oldu. AYRINTILAR: Hata (0x00000005) 5 (erişim engellendi.) yığın izlemesini sistem çağrısı 'NetValidatePasswordPolicy' parametresi (...) (\bas/bas/749_REL/bc_749_REL/src/ins/SAPINST/impl/src/syslib/account/synxcaccmg.cpp) dosyasında (359) satırı ile yürütülmesi içinde:  
  CThrThread.cpp: 85: CThrThread::threadFunction()  
  CSiServiceSet.cpp: 63: CSiServiceSet::executeService()  
  CSiStepExecute.cpp: 913: CSiStepExecute::execute()  
  EJSController.cpp: 179: EJSControllerImpl::executeScript()  
  JSExtension.hpp: 1136: CallFunctionBase::call()  
  CSiStepExecute.cpp: 764: CSiStepExecute::invokeDialog()  
  DarkModeGuiEngine.cpp: 56: DarkModeGuiEngine::showDialogCalledByJs()  
  DarkModeDialog.cpp: 85: DarkModeDialog::submit()  
  EJSController.cpp: 179: EJSControllerImpl::executeScript()  
  JSExtension.hpp: 1136: CallFunctionBase::call()  
  iaxxcaccount.cpp: 107: iastring CIaOsAccountConnect::callMemberFunction (iastring const & adına args_t const ve args)  
  iaxxcaccount.cpp: 1186: iastring CIaOsAccountConnect::validatePasswordPolicy (const args_t & _args)  
  iaxxbaccount.cpp: 430: CIaOsAccount::validatePasswordPolicy_impl()  
  synxcaccmg.cpp: 297: Const ISyAccountMgt::PasswordValidationMessage CSyAccountMgtImpl::validatePasswordPolicy(saponazure,***))
  * Çözüm  
    Adımda bir konak kural eklediğinizden emin olun *yalıtım* etki alanı denetleyicisini VM'den iletişime izin vermek

## <a name="next-steps"></a>Sonraki adımlar
* [Azure işlemler kılavuzu üzerinde SAP HANA][hana-ops-guide]
* [Azure sanal makineleri planlama ve uygulama için SAP][planning-guide]
* [SAP için Azure sanal makineler dağıtımı][deployment-guide]
* [SAP için Azure sanal makineleri DBMS dağıtım][dbms-guide]
