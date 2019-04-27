---
title: SAP NetWeaver HA Windows Yük devretme kümesi ve azure'da SAP ASCS/SCS örneği için paylaşılan disk yükleyin | Microsoft Docs
description: SAP NetWeaver HA Windows Yük devretme kümesi ve bir SAP ASCS/SCS örneği için paylaşılan disk nasıl yükleneceğini öğrenin.
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 6209bcb3-5b20-4845-aa10-1475c576659f
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1229b7f9e2a430a663a3e78bb457c03cf4a4a590
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60714461"
---
# <a name="install-sap-netweaver-ha-on-a-windows-failover-cluster-and-shared-disk-for-an-sap-ascsscs-instance-in-azure"></a>SAP NetWeaver HA Windows Yük devretme kümesi ve azure'da SAP ASCS/SCS örneği için paylaşılan disk yükleyin

[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

[sap-installation-guides]:http://service.sap.com/instguides

[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md

[deployment-guide]:deployment-guide.md

[dr-guide-classic]:https://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md

[ha-guide]:sap-high-availability-guide.md
[sap-high-availability-architecture-scenarios]:sap-high-availability-architecture-scenarios.md
[sap-high-availability-guide-wsfc-shared-disk]:sap-high-availability-guide-wsfc-shared-disk.md
[sap-high-availability-guide-wsfc-file-share]:sap-high-availability-guide-wsfc-file-share.md
[sap-ascs-high-availability-multi-sid-wsfc]:sap-ascs-high-availability-multi-sid-wsfc.md
[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md
[sap-ha-guide-8.9]:high-availability-guide.md#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-ha-guide-8.11]:high-availability-guide.md#661035b2-4d0f-4d31-86f8-dc0a50d78158
[sap-hana-ha]:sap-hana-high-availability.md
[sap-suse-ascs-ha]:high-availability-guide-suse.md

[planning-guide]:planning-guide.md
[planning-guide-11]:planning-guide.md
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10

[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md



[sap-ha-guide-figure-1000]:./media/virtual-machines-shared-sap-high-availability-guide/1000-wsfc-for-sap-ascs-on-azure.png
[sap-ha-guide-figure-1001]:./media/virtual-machines-shared-sap-high-availability-guide/1001-wsfc-on-azure-ilb.png
[sap-ha-guide-figure-1002]:./media/virtual-machines-shared-sap-high-availability-guide/1002-wsfc-sios-on-azure-ilb.png
[sap-ha-guide-figure-2000]:./media/virtual-machines-shared-sap-high-availability-guide/2000-wsfc-sap-as-ha-on-azure.png
[sap-ha-guide-figure-2001]:./media/virtual-machines-shared-sap-high-availability-guide/2001-wsfc-sap-ascs-ha-on-azure.png
[sap-ha-guide-figure-2003]:./media/virtual-machines-shared-sap-high-availability-guide/2003-wsfc-sap-dbms-ha-on-azure.png
[sap-ha-guide-figure-2004]:./media/virtual-machines-shared-sap-high-availability-guide/2004-wsfc-sap-ha-e2e-archit-template1-on-azure.png
[sap-ha-guide-figure-2005]:./media/virtual-machines-shared-sap-high-availability-guide/2005-wsfc-sap-ha-e2e-arch-template2-on-azure.png

[sap-ha-guide-figure-3000]:./media/virtual-machines-shared-sap-high-availability-guide/3000-template-parameters-sap-ha-arm-on-azure.png
[sap-ha-guide-figure-3001]:./media/virtual-machines-shared-sap-high-availability-guide/3001-configuring-dns-servers-for-Azure-vnet.png
[sap-ha-guide-figure-3002]:./media/virtual-machines-shared-sap-high-availability-guide/3002-configuring-static-IP-address-for-network-card-of-each-vm.png
[sap-ha-guide-figure-3003]:./media/virtual-machines-shared-sap-high-availability-guide/3003-setup-static-ip-address-ilb-for-ascs-instance.png
[sap-ha-guide-figure-3004]:./media/virtual-machines-shared-sap-high-availability-guide/3004-default-ascs-scs-ilb-balancing-rules-for-azure-ilb.png
[sap-ha-guide-figure-3005]:./media/virtual-machines-shared-sap-high-availability-guide/3005-changing-ascs-scs-default-ilb-rules-for-azure-ilb.png
[sap-ha-guide-figure-3006]:./media/virtual-machines-shared-sap-high-availability-guide/3006-adding-vm-to-domain.png
[sap-ha-guide-figure-3007]:./media/virtual-machines-shared-sap-high-availability-guide/3007-config-wsfc-1.png
[sap-ha-guide-figure-3008]:./media/virtual-machines-shared-sap-high-availability-guide/3008-config-wsfc-2.png
[sap-ha-guide-figure-3009]:./media/virtual-machines-shared-sap-high-availability-guide/3009-config-wsfc-3.png
[sap-ha-guide-figure-3010]:./media/virtual-machines-shared-sap-high-availability-guide/3010-config-wsfc-4.png
[sap-ha-guide-figure-3011]:./media/virtual-machines-shared-sap-high-availability-guide/3011-config-wsfc-5.png
[sap-ha-guide-figure-3012]:./media/virtual-machines-shared-sap-high-availability-guide/3012-config-wsfc-6.png
[sap-ha-guide-figure-3013]:./media/virtual-machines-shared-sap-high-availability-guide/3013-config-wsfc-7.png
[sap-ha-guide-figure-3014]:./media/virtual-machines-shared-sap-high-availability-guide/3014-config-wsfc-8.png
[sap-ha-guide-figure-3015]:./media/virtual-machines-shared-sap-high-availability-guide/3015-config-wsfc-9.png
[sap-ha-guide-figure-3016]:./media/virtual-machines-shared-sap-high-availability-guide/3016-config-wsfc-10.png
[sap-ha-guide-figure-3017]:./media/virtual-machines-shared-sap-high-availability-guide/3017-config-wsfc-11.png
[sap-ha-guide-figure-3018]:./media/virtual-machines-shared-sap-high-availability-guide/3018-config-wsfc-12.png
[sap-ha-guide-figure-3019]:./media/virtual-machines-shared-sap-high-availability-guide/3019-assign-permissions-on-share-for-cluster-name-object.png
[sap-ha-guide-figure-3020]:./media/virtual-machines-shared-sap-high-availability-guide/3020-change-object-type-include-computer-objects.png
[sap-ha-guide-figure-3021]:./media/virtual-machines-shared-sap-high-availability-guide/3021-check-box-for-computer-objects.png
[sap-ha-guide-figure-3022]:./media/virtual-machines-shared-sap-high-availability-guide/3022-set-security-attributes-for-cluster-name-object-on-file-share-quorum.png
[sap-ha-guide-figure-3023]:./media/virtual-machines-shared-sap-high-availability-guide/3023-call-configure-cluster-quorum-setting-wizard.png
[sap-ha-guide-figure-3024]:./media/virtual-machines-shared-sap-high-availability-guide/3024-selection-screen-different-quorum-configurations.png
[sap-ha-guide-figure-3025]:./media/virtual-machines-shared-sap-high-availability-guide/3025-selection-screen-file-share-witness.png
[sap-ha-guide-figure-3026]:./media/virtual-machines-shared-sap-high-availability-guide/3026-define-file-share-location-for-witness-share.png
[sap-ha-guide-figure-3027]:./media/virtual-machines-shared-sap-high-availability-guide/3027-successful-reconfiguration-cluster-file-share-witness.png
[sap-ha-guide-figure-3028]:./media/virtual-machines-shared-sap-high-availability-guide/3028-install-dot-net-framework-35.png
[sap-ha-guide-figure-3029]:./media/virtual-machines-shared-sap-high-availability-guide/3029-install-dot-net-framework-35-progress.png
[sap-ha-guide-figure-3030]:./media/virtual-machines-shared-sap-high-availability-guide/3030-sios-installer.png
[sap-ha-guide-figure-3031]:./media/virtual-machines-shared-sap-high-availability-guide/3031-first-screen-sios-data-keeper-installation.png
[sap-ha-guide-figure-3032]:./media/virtual-machines-shared-sap-high-availability-guide/3032-data-keeper-informs-service-be-disabled.png
[sap-ha-guide-figure-3033]:./media/virtual-machines-shared-sap-high-availability-guide/3033-user-selection-sios-data-keeper.png
[sap-ha-guide-figure-3034]:./media/virtual-machines-shared-sap-high-availability-guide/3034-domain-user-sios-data-keeper.png
[sap-ha-guide-figure-3035]:./media/virtual-machines-shared-sap-high-availability-guide/3035-provide-sios-data-keeper-license.png
[sap-ha-guide-figure-3036]:./media/virtual-machines-shared-sap-high-availability-guide/3036-data-keeper-management-config-tool.png
[sap-ha-guide-figure-3037]:./media/virtual-machines-shared-sap-high-availability-guide/3037-tcp-ip-address-first-node-data-keeper.png
[sap-ha-guide-figure-3038]:./media/virtual-machines-shared-sap-high-availability-guide/3038-create-replication-sios-job.png
[sap-ha-guide-figure-3039]:./media/virtual-machines-shared-sap-high-availability-guide/3039-define-sios-replication-job-name.png
[sap-ha-guide-figure-3040]:./media/virtual-machines-shared-sap-high-availability-guide/3040-define-sios-source-node.png
[sap-ha-guide-figure-3041]:./media/virtual-machines-shared-sap-high-availability-guide/3041-define-sios-target-node.png
[sap-ha-guide-figure-3042]:./media/virtual-machines-shared-sap-high-availability-guide/3042-define-sios-synchronous-replication.png
[sap-ha-guide-figure-3043]:./media/virtual-machines-shared-sap-high-availability-guide/3043-enable-sios-replicated-volume-as-cluster-volume.png
[sap-ha-guide-figure-3044]:./media/virtual-machines-shared-sap-high-availability-guide/3044-data-keeper-synchronous-mirroring-for-SAP-gui.png
[sap-ha-guide-figure-3045]:./media/virtual-machines-shared-sap-high-availability-guide/3045-replicated-disk-by-data-keeper-in-wsfc.png
[sap-ha-guide-figure-3046]:./media/virtual-machines-shared-sap-high-availability-guide/3046-dns-entry-sap-ascs-virtual-name-ip.png
[sap-ha-guide-figure-3047]:./media/virtual-machines-shared-sap-high-availability-guide/3047-dns-manager.png
[sap-ha-guide-figure-3048]:./media/virtual-machines-shared-sap-high-availability-guide/3048-default-cluster-probe-port.png
[sap-ha-guide-figure-3049]:./media/virtual-machines-shared-sap-high-availability-guide/3049-cluster-probe-port-after.png
[sap-ha-guide-figure-3050]:./media/virtual-machines-shared-sap-high-availability-guide/3050-service-type-ers-delayed-automatic.png
[sap-ha-guide-figure-5000]:./media/virtual-machines-shared-sap-high-availability-guide/5000-wsfc-sap-sid-node-a.png
[sap-ha-guide-figure-5001]:./media/virtual-machines-shared-sap-high-availability-guide/5001-sios-replicating-local-volume.png
[sap-ha-guide-figure-5002]:./media/virtual-machines-shared-sap-high-availability-guide/5002-wsfc-sap-sid-node-b.png
[sap-ha-guide-figure-5003]:./media/virtual-machines-shared-sap-high-availability-guide/5003-sios-replicating-local-volume-b-to-a.png

[sap-ha-guide-figure-6003]:./media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png

[sap-templates-3-tier-multisid-xscs-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs%2Fazuredeploy.json
[sap-templates-3-tier-multisid-xscs-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps-md%2Fazuredeploy.json

[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../../../azure-resource-manager/resource-group-overview.md#the-benefits-of-using-resource-manager

[virtual-machines-manage-availability]:../../virtual-machines-windows-manage-availability.md

Bu makalede, yükleme ve bir Windows Server Yük devretme kümesi ve Küme Paylaşılan disk kümeleme SAP ASCS/SCS örneği için kullanarak Azure'da yüksek kullanılabilirlik SAP sistemine yapılandırma açıklar.

## <a name="prerequisites"></a>Önkoşullar

Yüklemeye başlamadan önce şu belgeleri gözden geçirin:

* [Mimari Kılavuzu: Bir küme paylaşılan disk kullanarak bir Windows Yük devretme kümesinde Küme SAP ASCS/SCS örneği][sap-high-availability-guide-wsfc-shared-disk]

* [Azure altyapı SAP yüksek kullanılabilirlik için bir SAP ASCS/SCS örneği için bir Windows Yük devretme kümesi ve paylaşılan disk kullanarak hazırlama][sap-high-availability-infrastructure-wsfc-shared-disk]

Ayarlar DBMS sistemine bağlı olarak farklılık gösterdiğinden bu makaledeki DBMS Kurulumu açıklanmaktadır yok. Yüksek kullanılabilirlik sorunları DBMS ile farklı DBMS satıcıları desteklemek için Azure işlevleri ile giderilen varsayıyoruz. AlwaysOn veya SQL Server ve Oracle Data Guard'için Oracle veritabanları için veritabanı yansıtma verilebilir. Bu makalede kullandığımız senaryosunda, size daha fazla koruma için DBMS eklemeyin.

Farklı DBMS Hizmetleri kümelenmiş SAP ASCS veya azure'da SCS yapılandırma ile etkileşim kurduğunuzda özel durumlar vardır.

> [!NOTE]
> SAP NetWeaver ABAP sistemleri, Java sistemleri ve ABAP + Java sistemleri yükleme yordamları neredeyse aynıdır. En önemli fark, bir SAP ABAP sistemi bir ASCS örneğine sahip olur. SAP Java sistem bir SCS örneği vardır. ASCS örneği ve bir SCS örneği aynı Microsoft yük devretme küme grubu içinde çalışan SAP ABAP + Java sistemi vardır. Her SAP NetWeaver yükleme yığınının yükleme farkları açıkça belirtilmiştir. Diğer tüm bölümleri aynı olduğunu varsayabilirsiniz.  
>
>

## <a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a>Bir yüksek kullanılabilirlik ASCS/SCS örneği ile SAP yükleme

> [!IMPORTANT]
> SIOS DataKeeper yansıtılmış birimler üzerinde sayfa dosyanız yerleştirmemeye emin olun. Yansıtılmış birimler DataKeeper desteklemez. Varsayılan sayfa dosyanız bir Azure sanal makine geçici D sürücüsündeki bırakabilirsiniz. Zaten orada değilse, Windows disk belleği dosyası, Azure sanal makinenizin D sürücüye taşıyın.
>
>

Bir yüksek kullanılabilirlik ASCS/SCS örneği ile SAP yüklemek, bu görevleri içerir:

* Kümelenmiş SAP ASCS/SCS örneği için bir sanal ana bilgisayar adı oluşturun.
* SAP ilk küme düğümüne yükleyin.
* SAP ASCS/SCS örneği profilini değiştirin.
* Bir araştırma bağlantı noktasını ekleyin.
* Windows Güvenlik Duvarı araştırma bağlantı noktasını açın.

### <a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a>Kümelenmiş SAP ASCS/SCS örneği için bir sanal ana bilgisayar adı oluşturun

1. Windows DNS Yöneticisi'nde, sanal ana bilgisayar adı ASCS/SCS örneği için bir DNS girişi oluşturabilir.

   > [!IMPORTANT]
   > Sanal ana bilgisayar adını ASCS/SCS örneği için atadığınız IP adresi, Azure yük dengeleyiciye atanan IP adresi ile aynı olmalıdır (\<SID\>- lb ascs).  
   >
   >

   Sanal bir SAP ASCS/SCS konak adı (pr1 ascs sap) IP adresini Azure Load Balancer (pr1-lb-ascs) IP adresi ile aynıdır.

   ![Şekil 1: SAP ASCS/SCS küme sanal adı ve TCP/IP adresi için DNS girişi tanımlayın][sap-ha-guide-figure-3046]

   _**Şekil 1:** SAP ASCS/SCS küme sanal adı ve TCP/IP adresi için DNS girişi tanımlayın_

2. Sanal ana bilgisayar adı için atanan IP adresini tanımlamak için seçin **DNS Yöneticisi** > **etki alanı**.

   ![Şekil 2: Yeni bir sanal ad ve SAP ASCS/SCS kümesi yapılandırması için TCP/IP adresi][sap-ha-guide-figure-3047]

   _**Şekil 2:** Yeni bir sanal ad ve SAP ASCS/SCS kümesi yapılandırması için TCP/IP adresi_

### <a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a> SAP ilk küme düğümüne yükleme

1. İlk küme düğümü seçenek a küme düğümünde yürütün Örneğin, üzerinde pr1 ascs 0 * ana bilgisayar.
2. Azure iç yük dengeleyici için bağlantı noktalarını varsayılan tutmak için bu seçeneği seçin:

   * **ABAP sistem**: **ASCS** örnek numarası **00**
   * **Java sistem**: **SCS** örnek numarası **01**
   * **ABAP + Java sistem**: **ASCS** örnek numarası **00** ve **SCS** örnek numarası **01**

   ABAP ASCS örneği ve 01 Java SCS örneği için örnek sayılar 00 dışında kullanmak için ilk olarak, Azure iç yük dengeleyici varsayılan Yük Dengeleme kurallarını değiştirin. Daha fazla bilgi için [ASCS/SCS varsayılan Yük Dengeleme kuralları Azure iç yük dengeleyici için değiştirme][sap-ha-guide-8.9].

Sonraki birkaç görevi standart SAP yükleme belgelerinde açıklanan değildir.

> [!NOTE]
> SAP yükleme belgelerine ilk ASCS/SCS küme düğümüne yüklemeyi açıklar.
>
>

### <a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a> SAP ASCS/SCS örneği profilini değiştirme

İlk olarak, yeni bir profil parametre ekleyin. Profil parametresi, uzun süre boşta olduklarında kapanmasını SAP iş süreçleri sıraya alma server arasında bağlantıları engeller. Biz sorun senaryoda bahsetmek [SAP ASCS/SCS örneği her iki küme düğümlerinde kayıt defteri girdisini eklemeniz][sap-ha-guide-8.11]. Bu bölümde, biz de bazı temel TCP/IP bağlantı parametreleri için iki değişiklik sunar. İkinci adımda, kuyruğa sunucusunu gönderecek şekilde ayarlamanız gerekir. bir `keep_alive` bağlantıları Azure iç load balancer'ın boşta eşiği isabet yoksa, sinyal.

SAP ASCS/SCS örneği profilini değiştirmek için:

1. Bu profil parametresi SAP ASCS/SCS örneği profiline ekleyin:

   ```
   enque/encni/set_so_keepalive = true
   ```
   Bizim örneğimizde yoludur:

   `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_ASCS00_pr1-ascs-sap`

   Örneğin, SAP SCS örneği profili ve karşılık gelen yolu:

   `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_SCS01_pr1-ascs-sap`

2. Değişiklikleri uygulamak için SAP ASCS/SCS örneği yeniden başlatın.

### <a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a> Bir araştırma bağlantı noktası Ekle

Azure Load Balancer ile çalışma tüm küme yapılandırmasını yapmak için iç load balancer'ın araştırma işlevini kullanın. Azure iç yük dengeleyici genellikle gelen iş yükü katılan sanal makineler arasında eşit olarak dağıtır.

 Ancak, tek örnek etkin olmadığından bu kümenin bazı yapılandırmalarda işe yaramaz. Diğer bir örnek pasif ve herhangi bir iş yükü kabul edemez. Azure iç yük dengeleyici yalnızca etkin bir örneği için iş atarken araştırma işlevselliği yardımcı olur. Araştırma özelliği sayesinde, iç yük dengeleyici hangi örneklerinin etkin olduğunu ve yalnızca iş yüküyle örneği ardından hedef algılayabilir.

Bir araştırma bağlantı noktası eklemek için:

1. Geçerli denetleyin **ProbePort** aşağıdaki PowerShell komutunu çalıştırarak değeri:

   ```powershell
   $SAPSID = "PR1"     # SAP <SID>

   $SAPNetworkIPClusterName = "SAP $SAPSID IP"
   Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter
   ```

   Küme yapılandırmasında sanal makinelerden biri içinde komutu yürütün.

2. Bir araştırma bağlantı noktasını tanımlayın. Varsayılan araştırma bağlantı noktası numarası 0'dır. Bizim örneğimizde, yoklama bağlantı noktası 62000 kullanırız.

   ![Şekil 3: Küme yapılandırması araştırma bağlantı noktası varsayılan olarak 0 olur.][sap-ha-guide-figure-3048]

   _**Şekil 3:** Varsayılan Küme yapılandırma araştırma bağlantı noktası 0'dır_

   Bağlantı noktası numarasını SAP Azure Resource Manager şablonları içinde tanımlanır. PowerShell bağlantı noktası numarası atayabilirsiniz.

   SAP için yeni bir ProbePort değeri ayarlamak için \<SID\> IP küme kaynağı, ortamınızı PowerShell değişkenleri güncelleştirmek için aşağıdaki PowerShell betiğini çalıştırın:

   ```powershell
   $SAPSID = "PR1"      # SAP <SID>
   $ProbePort = 62000   # ProbePort of the Azure internal load balancer

   Clear-Host
   $SAPClusterRoleName = "SAP $SAPSID"
   $SAPIPresourceName = "SAP $SAPSID IP"
   $SAPIPResourceClusterParameters =  Get-ClusterResource $SAPIPresourceName | Get-ClusterParameter
   $IPAddress = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "Address" }).Value
   $NetworkName = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "Network" }).Value
   $SubnetMask = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "SubnetMask" }).Value
   $OverrideAddressMatch = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "OverrideAddressMatch" }).Value
   $EnableDhcp = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "EnableDhcp" }).Value
   $OldProbePort = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "ProbePort" }).Value

   $var = Get-ClusterResource | Where-Object {  $_.name -eq $SAPIPresourceName  }

   Write-Host "Current configuration parameters for SAP IP cluster resource '$SAPIPresourceName' are:" -ForegroundColor Cyan
   Get-ClusterResource -Name $SAPIPresourceName | Get-ClusterParameter

   Write-Host
   Write-Host "Current probe port property of the SAP cluster resource '$SAPIPresourceName' is '$OldProbePort'." -ForegroundColor Cyan
   Write-Host
   Write-Host "Setting the new probe port property of the SAP cluster resource '$SAPIPresourceName' to '$ProbePort' ..." -ForegroundColor Cyan
   Write-Host

   $var | Set-ClusterParameter -Multiple @{"Address"=$IPAddress;"ProbePort"=$ProbePort;"Subnetmask"=$SubnetMask;"Network"=$NetworkName;"OverrideAddressMatch"=$OverrideAddressMatch;"EnableDhcp"=$EnableDhcp}

   Write-Host

   $ActivateChanges = Read-Host "Do you want to take restart SAP cluster role '$SAPClusterRoleName', to activate the changes (yes/no)?"

   if($ActivateChanges -eq "yes"){
   Write-Host
   Write-Host "Activating changes..." -ForegroundColor Cyan

   Write-Host
   write-host "Taking SAP cluster IP resource '$SAPIPresourceName' offline ..." -ForegroundColor Cyan
   Stop-ClusterResource -Name $SAPIPresourceName
   sleep 5

   Write-Host "Starting SAP cluster role '$SAPClusterRoleName' ..." -ForegroundColor Cyan
   Start-ClusterGroup -Name $SAPClusterRoleName

   Write-Host "New ProbePort parameter is active." -ForegroundColor Green
   Write-Host

   Write-Host "New configuration parameters for SAP IP cluster resource '$SAPIPresourceName':" -ForegroundColor Cyan
   Write-Host
   Get-ClusterResource -Name $SAPIPresourceName | Get-ClusterParameter
   }else
   {
   Write-Host "Changes are not activated."
   }
   ```

   SAP getirdikten sonra \<SID\> doğrulayın, küme rolünü **ProbePort** yeni değere ayarlanır.

   ```powershell
   $SAPSID = "PR1"     # SAP <SID>

   $SAPNetworkIPClusterName = "SAP $SAPSID IP"
   Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter

   ```
   Betik çalıştıktan sonra değişiklikleri etkinleştirmek için SAP küme grubu yeniden başlatmanız istenir.

   ![Şekil 4: Yeni değeri ayarladıktan sonra küme bağlantı noktası araştırma][sap-ha-guide-figure-3049]

   _**Şekil 4:** Yeni değeri ayarladıktan sonra küme bağlantı noktası araştırma_

### <a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a> Windows Güvenlik Duvarı araştırma bağlantı noktasını açın

Windows Güvenlik Duvarı araştırma bağlantı noktasını her iki küme düğümlerinde açın. Bir Windows Güvenlik Duvarı araştırma bağlantı noktasını açmak için aşağıdaki betiği kullanın. PowerShell benzeri değişkenleri ortamınız için güncelleştirin.

  ```powershell
  $ProbePort = 62000   # ProbePort of the Azure internal load balancer

  New-NetFirewallRule -Name AzureProbePort -DisplayName "Rule for Azure Probe Port" -Direction Inbound -Action Allow -Protocol TCP -LocalPort $ProbePort
  ```

**ProbePort** ayarlanır **62000**. Şimdi, dosya paylaşımına erişebilirsiniz \\\ascsha-clsap\sapmnt diğer konaklarından ascsha dba'lar gelenler gibi.

## <a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a> Veritabanı örneğini yükleyin

Veritabanı örneği yüklemek için SAP yükleme belgelerinde açıklanan işlemi izleyin.

## <a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a> İkinci küme düğümüne yükleme

İkinci küme yüklemek için SAP Yükleme Kılavuzu'nda açıklanan adımları izleyin.

## <a name="094bc895-31d4-4471-91cc-1513b64e406a"></a> SAP Ağıranlar Windows hizmet örneği başlangıç türünü değiştirme

SAP Ağıranlar Windows hizmetinin başlangıç türünü değiştirin **otomatik (Gecikmeli Başlatma)** her iki küme düğümlerinde.

![Şekil 5: SAP Ağıranlar örneği için hizmet türü Gecikmeli otomatik olarak değiştirin][sap-ha-guide-figure-3050]

_**Şekil 5:** SAP Ağıranlar örneği için hizmet türü Gecikmeli otomatik olarak değiştirin_

## <a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a> SAP birincil uygulama sunucusu yükleme

Birincil uygulama sunucusu (PA'lar) örneğini yüklemeyi \<SID\>-di-Pa'ları barındırmak için belirlediğiniz sanal makinede 0. Azure üzerinde hiçbir bağımlılık vardır. Hiçbir DataKeeper özgü ayarları vardır.

## <a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a> SAP ek uygulama sunucusu yükleme

Bir SAP ek uygulama sunucusu (AAS) tüm SAP uygulama sunucusu örneği barındırmak için belirlediğiniz makinelerde yükleyin. Örneğin, \<SID\>-di-1 olarak \<SID\>- di -&lt;n&gt;.

> [!NOTE]
> Bu, yüksek kullanılabilirlik SAP NetWeaver system yüklemesi tamamlanır. Ardından, yük devretme testiyle devam edin.
>


## <a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a> SIOS çoğaltma ve SAP ASCS/SCS örneği yük devretme testi
Testi ve yük devretme kümesi Yöneticisi SIOS DataKeeper yönetim ve yapılandırma aracını kullanarak bir SAP ASCS/SCS örneği yük devretme ve SIOS disk çoğaltması izlemek kolay bir işlemdir.

### <a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a> SAP ASCS/SCS örneği bir küme düğümünde çalışıyor

SAP PR1 küme grubu A'daki küme düğümünde çalışıyor Örneğin, pr1-ascs-0. Paylaşılan disk SAP PR1 küme grubunun parçası olduğu sürücü S, küme düğümüne A. atama ASCS/SCS örneği disk sürücüsü S. de kullanır. 

![Şekil 6: Yük Devretme Kümesi Yöneticisi: SAP \<SID\> küme grubu, bir küme düğümünde çalışıyor][sap-ha-guide-figure-5000]

_**Şekil 6:** Yük Devretme Kümesi Yöneticisi: SAP \<SID\> küme grubu, bir küme düğümünde çalışıyor_

SIOS DataKeeper yönetim ve Yapılandırma Aracı'nda paylaşılan disk zaman uyumlu olarak bir küme düğümünde kaynak birim sürücüden S b küme düğümünü hedef birimi sürücüsünü S çoğaltılan verileri görebilirsiniz Örneğin, pr1-ascs-0'dan [10.0.0.40] pr1-ascs-1 [10.0.0.41] çoğaltılır.

![Şekil 7: SIOS DataKeeper yerel birim bir küme düğümünden küme düğümüne B çoğaltma][sap-ha-guide-figure-5001]

_**Şekil 7:** SIOS DataKeeper yerel birim bir küme düğümünden küme düğümüne B çoğaltma_

### <a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a> Bir düğümden yük devretme için B düğümü

1. Bir yük devretme SAP başlatmak için bu seçeneklerden birini \<SID\> küme düğümüne B: bir küme düğümünden küme grubu
   - Yük Devretme Kümesi Yöneticisi  
   - Yük devretme kümesi PowerShell

   ```powershell
   $SAPSID = "PR1"     # SAP <SID>

   $SAPClusterGroup = "SAP $SAPSID"
   Move-ClusterGroup -Name $SAPClusterGroup

   ```
2. Küme düğümü bir Windows konuk işletim sistemi içinde yeniden başlatın. Bu SAP otomatik yük devretme başlatır \<SID\> düğüme B'nin düğümünden bir küme grubu  
3. Küme düğümü bir Azure portalından yeniden başlatın. Bu SAP otomatik yük devretme başlatır \<SID\> düğüme B'nin düğümünden bir küme grubu  
4. Küme düğümü bir Azure PowerShell kullanarak yeniden başlatın. Bu SAP otomatik yük devretme başlatır \<SID\> düğüme B'nin düğümünden bir küme grubu

   Yük devretme, SAP sonra \<SID\> küme grubu b küme düğümünde çalışıyor Örneğin, bu ascs pr1 1 üzerinde çalışıyor.

   ![Şekil 8: Yük Devretme Kümesi Yöneticisi'nde, SAP \<SID\> küme grubu B küme düğümünde çalışıyor][sap-ha-guide-figure-5002]

   _**Şekil 8**: Yük Devretme Kümesi Yöneticisi'nde, SAP \<SID\> küme grubu B küme düğümünde çalışıyor_

   Paylaşılan disk artık bağlanmıştır kümede düğüm b SIOS DataKeeper veri kaynak birim sürücüden S B küme düğümünde A. küme düğümünde hedef birimin sürücü S çoğaltma Örneğin, bu pr1-ascs-1'den [10.0.0.41] pr1-ascs-0 için [10.0.0.40] çoğaltılıyor.

   ![Şekil 9: SIOS DataKeeper yerel birim düğümü bir küme için küme düğümlerinin birinden B çoğaltır.][sap-ha-guide-figure-5003]

   _**Şekil 9:** SIOS DataKeeper yerel birim düğümü bir küme için küme düğümlerinin birinden B çoğaltır._
