---
title: "SAP (A) SCS örnek küme kullanarak Windows Yük devretme kümesinde Küme Paylaşılan Azure diskte | Microsoft Docs"
description: "SAP (A) SCS örnek küme kullanarak Windows Yük devretme kümesinde Küme Paylaşılan Disk"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: f6fb85f8-c77a-4af1-bde8-1de7e4425d2e
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b7706b6f0adce89775f1cb3cffb102510772a101
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
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

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md
[ha-guide]:sap-high-availability-guide.md
[sap-high-availability-architecture-scenarios]:sap-high-availability-architecture-scenarios.md
[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md

[planning-guide]:planning-guide.md
[planning-guide-11]:planning-guide.md
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10

[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[sap-ha-guide]:sap-high-availability-guide.md
[sap-ha-guide-2]:#42b8f600-7ba3-4606-b8a5-53c4f026da08
[sap-ha-guide-4]:#8ecf3ba0-67c0-4495-9c14-feec1a2255b7
[sap-ha-guide-8]:#78092dbe-165b-454c-92f5-4972bdbef9bf
[sap-ha-guide-8.1]:#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-ha-guide-8.9]:#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-ha-guide-8.11]:#661035b2-4d0f-4d31-86f8-dc0a50d78158
[sap-ha-guide-8.12]:#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-ha-guide-8.12.1]:#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-ha-guide-8.12.3]:#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-ha-guide-8.12.3.1]:#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-ha-guide-8.12.3.2]:#dd41d5a2-8083-415b-9878-839652812102
[sap-ha-guide-8.12.3.3]:#d9c1fc8e-8710-4dff-bec2-1f535db7b006
[sap-ha-guide-9]:#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:#31c6bd4f-51df-4057-9fdf-3fcbc619c170
[sap-ha-guide-9.1.1]:#a97ad604-9094-44fe-a364-f89cb39bf097

[sap-ha-multi-sid-guide]:sap-high-availability-multi-sid.md (SAP multi-SID high-availability configuration)

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

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


[sap-ha-guide-figure-8001]:./media/virtual-machines-shared-sap-high-availability-guide/8001.png
[sap-ha-guide-figure-8002]:./media/virtual-machines-shared-sap-high-availability-guide/8002.png
[sap-ha-guide-figure-8003]:./media/virtual-machines-shared-sap-high-availability-guide/8003.png
[sap-ha-guide-figure-8004]:./media/virtual-machines-shared-sap-high-availability-guide/8004.png
[sap-ha-guide-figure-8005]:./media/virtual-machines-shared-sap-high-availability-guide/8005.png
[sap-ha-guide-figure-8006]:./media/virtual-machines-shared-sap-high-availability-guide/8006.png
[sap-ha-guide-figure-8007]:./media/virtual-machines-shared-sap-high-availability-guide/8007.png
[sap-ha-guide-figure-8008]:./media/virtual-machines-shared-sap-high-availability-guide/8008.png
[sap-ha-guide-figure-8009]:./media/virtual-machines-shared-sap-high-availability-guide/8009.png
[sap-ha-guide-figure-8010]:./media/virtual-machines-shared-sap-high-availability-guide/8010.png
[sap-ha-guide-figure-8011]:./media/virtual-machines-shared-sap-high-availability-guide/8011.png
[sap-ha-guide-figure-8012]:./media/virtual-machines-shared-sap-high-availability-guide/8012.png
[sap-ha-guide-figure-8013]:./media/virtual-machines-shared-sap-high-availability-guide/8013.png
[sap-ha-guide-figure-8014]:./media/virtual-machines-shared-sap-high-availability-guide/8014.png
[sap-ha-guide-figure-8015]:./media/virtual-machines-shared-sap-high-availability-guide/8015.png
[sap-ha-guide-figure-8016]:./media/virtual-machines-shared-sap-high-availability-guide/8016.png
[sap-ha-guide-figure-8017]:./media/virtual-machines-shared-sap-high-availability-guide/8017.png
[sap-ha-guide-figure-8018]:./media/virtual-machines-shared-sap-high-availability-guide/8018.png
[sap-ha-guide-figure-8019]:./media/virtual-machines-shared-sap-high-availability-guide/8019.png
[sap-ha-guide-figure-8020]:./media/virtual-machines-shared-sap-high-availability-guide/8020.png
[sap-ha-guide-figure-8021]:./media/virtual-machines-shared-sap-high-availability-guide/8021.png
[sap-ha-guide-figure-8022]:./media/virtual-machines-shared-sap-high-availability-guide/8022.png
[sap-ha-guide-figure-8023]:./media/virtual-machines-shared-sap-high-availability-guide/8023.png
[sap-ha-guide-figure-8024]:./media/virtual-machines-shared-sap-high-availability-guide/8024.png
[sap-ha-guide-figure-8025]:./media/virtual-machines-shared-sap-high-availability-guide/8025.png


[sap-templates-3-tier-multisid-xscs-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs%2Fazuredeploy.json
[sap-templates-3-tier-multisid-xscs-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps-md%2Fazuredeploy.json

[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../../../azure-resource-manager/resource-group-overview.md#the-benefits-of-using-resource-manager

[virtual-machines-manage-availability]:../../virtual-machines-windows-manage-availability.md

> ![Windows][Logo_Windows] Windows
>

# <a name="clustering-sap-ascs-instance-on-windows-failover-cluster-using-cluster-shared-disk-on-azure"></a>SAP (A) SCS örnek küme kullanarak Windows Yük devretme kümesinde Küme Paylaşılan Disk Azure ile ilgili

Windows Server Yük Devretme Kümelemesi, yüksek kullanılabilirlik SAP ASCS/SCS yükleme ve Windows'da DBMS temelidir.

Bir yük devretme kümesi, uygulamaların ve hizmetlerin kullanılabilirliğini artırmak için birlikte çalışan 1 + n bağımsız sunucular (düğümler) grubudur. Bir düğüm hatası oluşursa, Windows Server Yük Devretme Kümelemesi uygulamaları ve hizmetleri sağlamak için iyi bir küme korurken oluşabilir sayısı hesaplar. Yük Devretme Kümelemesi elde etmek için farklı bir çekirdek modlarından seçebilirsiniz.

## <a name="prerequisite"></a>Önkoşul
Bu belgeyle başlatmadan önce bu belgeleri gözden geçirdiğinizden emin olun:

* [Azure sanal makinelerini yüksek kullanılabilirlik mimarisi ve SAP NetWeaver senaryoları][sap-high-availability-architecture-scenarios]


## <a name="windows-server-failover-clustering-in-azure"></a>Azure'da Windows Server Yük devretme

Çıplak metal veya özel bulut dağıtımları karşılaştırıldığında, Azure sanal makineleri Windows Server Yük Devretme Kümelemesi yapılandırmak için ek adımlar gerektirir. Bir küme oluşturma sırasında birkaç IP adresleri ve sanal ana bilgisayar adları SAP ASCS/SCS örneği için ayarlamanız gerekir.

### <a name="name-resolution-in-azure-and-cluster-virtual-host-name"></a>Ad çözümlemesi Azure ve küme sanal ana bilgisayar adı

Azure bulut platformu kayan IP adresleri gibi sanal IP adreslerini yapılandırma seçeneği sağlamaz. Küme kaynağı bulutta ulaşmak için sanal bir IP adresi ayarlamak için alternatif bir çözüm gerekir. Azure sahip bir **iç yük dengeleyici** Azure yük dengeleyici hizmet. İç yük dengeleyici ile istemcileri kümenin küme sanal IP adresi ulaşabilirsiniz. Küme düğümleri içeren kaynak grubunda iç yük dengeleyicisi dağıtmanız gerekir. Ardından, kuralları araştırmasıyla iç yük dengeleyicisi bağlantı noktaları iletme tüm gerekli bağlantı noktası yapılandırın. İstemcileri sanal ana bilgisayar adını bağlanabilir. DNS sunucusu küme IP adresi ve kümesinin etkin düğümü iletme iç yük dengeleyici tanıtıcıları bağlantı noktası çözümler.

![Şekil 1: Windows Server Yük devretme kümeleme yapılandırmasında Azure olmayan paylaşılan bir disk][sap-ha-guide-figure-1001]

_**Şekil 1:** Windows Server Yük devretme kümeleme yapılandırmasında Azure olmayan paylaşılan bir disk_

### <a name="sap-ascs-ha-with-cluster-shared-disks"></a>SAP (A) SCS HA Küme Paylaşılan diskleri
Üzerinde **Windows**, bir **SAP (A) SCS** örneğini içeren yalnızca **SAP merkezi hizmetlerinin**, **SAP ileti sunucusu**, ve  **sıraya alma sunucu işlemlerini**, aynı zamanda **SAP genel ana bilgisayar** tüm SAP sistem için merkezi dosyalarını depolamak için kullanılan dosyalar.

Bu nedenle, SAP (A) SCS örneği iki bileşenden aşağıdaki vardır:

* **SAP merkezi hizmetlerinin** ile:
    * İki **işlemleri**, sunucu ileti ve sıraya alma ve **< SCSVirtualHostName (A) >** bu iki işlemler erişmek için kullanılan
    * **Dosya yapısı** S:\usr\sap\\&lt;SID&gt;\(A) SCS<InstanceNumber>


* **SAP genel ana bilgisayar** ile dosyaları **sapmnt dosya paylaşımı**:
    * **Dosya yapısı** S:\usr\sap\\&lt;SID&gt;\SYS\....
    * **sapmnt dosya paylaşımı**, bu genel S:\usr\sap erişim sağlayan\\&lt;SID&gt;\SYS\.. UNC yolu kullanarak dosyaları:

     \\\\< SCSVirtualHostName (a) > \sapmnt\\&lt;SID&gt;\SYS\....


![Şekil 2: İşlemler, dosya yapısı ve genel ana bilgisayar sapmnt dosya paylaşımı SAP (A) SCS örneği][sap-ha-guide-figure-8001]

_**Şekil 2:** işlemleri, dosya yapısı ve genel ana bilgisayar sapmnt dosya paylaşımına SAP (A) SCS örneği_

Yüksek oranda kullanılabilirlik ayarında, biz SAP (A) SCS örnekleri kümeleme. Kullanıyoruz **Paylaşılan diskleri kümelenmiş** (dosyaları SAP (A) SCS ve SAP genel ana bilgisayar dosyaları yerleştirmek için S:\ sürücü örneğimizde).

![Şekil 3: (A) SCS HA mimarisiyle paylaşılan disk SAP][sap-ha-guide-figure-8002]

_**Şekil 3:** (A) SCS HA mimarisiyle paylaşılan disk SAP_

> [!IMPORTANT]
>Bu iki bileşenin aynı SAP (A) SCS örnek altında çalışacak şekilde:
>* Aynı **< SCSVirtualHostName (A) >** erişim SAP iletisi ve sıraya alma sunucu işlemlerini yanı sıra, sapmnt dosya paylaşımı aracılığıyla SAP genel ana bilgisayar dosyaları için kullanılır.
>* Aynı **Küme Paylaşılan disk S:\**  bunlar arasında paylaşılır
>


![Şekil 4: (A) SCS HA mimarisiyle paylaşılan disk SAP][sap-ha-guide-figure-8003]

_**Şekil 4:** (A) SCS HA mimarisiyle paylaşılan disk SAP_

### <a name="shared-disk-in-azure-with-sios-datakeeper"></a>Paylaşılan disk Azure ile SIOS DataKeeper

Yüksek kullanılabilirlik SAP ASCS/SCS örneği için paylaşılan depolama küme.

Üçüncü taraf yazılım SIOS DataKeeper Cluster Edition, Küme Paylaşılan depolama benzetim yansıtılmış bir depolama alanı oluşturmak için kullanabilirsiniz. Gerçek zamanlı zaman uyumlu veri çoğaltma SIOS çözümü sağlar.

Aşağıdaki adımları bir küme için paylaşılan disk kaynağı oluşturabilirsiniz:

1. Windows küme yapılandırmasında sanal makinelerinin (VM'ler) her biri için ek bir disk ekleyin.
2. SIOS DataKeeper Cluster Edition her iki sanal makine düğümde çalıştırın.
3. Hedef sanal makineye bağlı ek disk birimi kaynak sanal makinedeki bağlı ek disk birimi içeriği yansıtan SIOS DataKeeper Cluster Edition yapılandırın. SIOS DataKeeper kaynak ve hedef yerel birimleri soyutlar ve Windows Server Yük Devretme Kümelemesi bir paylaşılan disk için gösterir.

Hakkında daha fazla bilgi almak [SIOS DataKeeper](http://us.sios.com/products/datakeeper-cluster/).

![Şekil 5: Windows Server Yük devretme kümeleme yapılandırmasında SIOS DataKeeper ile Azure][sap-ha-guide-figure-1002]

_**Şekil 5:** SIOS DataKeeper ile azure'da Windows Server Yük Devretme Kümelemesi yapılandırma_

> [!NOTE]
> SQL Server gibi bazı DBMS ürünleri ile yüksek kullanılabilirlik için Paylaşılan diskleri gerekmez. SQL Server Always On başka bir küme düğümüne yerel diske bir küme düğümünde yerel diskten DBMS veri ve günlük dosyalarını çoğaltır. Bu durumda, paylaşılan bir disk Windows Küme yapılandırması gerekmez.
>

## <a name="next-steps"></a>Sonraki Adımlar

* [Azure altyapı hazırlık için SAP Windows Yük devretme kümesi ve paylaşılan Disk için SAP (A) SCS örneği kullanarak HA][sap-high-availability-infrastructure-wsfc-shared-disk]

* [Itanium tabanlı sistemler için SAP NetWeaver HA yükleme Windows Yük devretme kümesi ve paylaşılan Disk SAP (A) SCS örneği için][sap-high-availability-installation-wsfc-shared-disk]
