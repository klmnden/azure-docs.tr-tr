---
title: "Azure üzerinde bir dosya paylaşımı kullanarak bir SAP ASCS/SCS örneği Windows Yük devretme kümesinde Küme | Microsoft Docs"
description: "Azure üzerinde bir dosya paylaşımı kullanarak bir SAP ASCS/SCS örneği Windows Yük devretme kümesinde Küme öğrenin."
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8cb339c9ecffbbc711aa6ea55d6f357fe0f4cfd0
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[2492395]:https://launchpad.support.sap.com/#/notes/2492395

[kb4025334]:https://support.microsoft.com/help/4025334/windows-10-update-kb4025334

[dv2-series]:https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dv2-series
[ds-series]:https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#ds-series

[sap-installation-guides]:http://service.sap.com/instguides

[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md

[deployment-guide]:deployment-guide.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md

[ha-guide]:sap-high-availability-guide.md
[sap-high-availability-architecture-scenarios]:sap-high-availability-architecture-scenarios.md
[sap-high-availability-guide-wsfc-shared-disk]:sap-high-availability-guide-wsfc-shared-disk.md
[sap-high-availability-guide-wsfc-file-share]:sap-high-availability-guide-wsfc-file-share.md
[sap-ascs-high-availability-multi-sid-wsfc]:sap-ascs-high-availability-multi-sid-wsfc.md
[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-infrastructure-wsfc-file-share]:sap-high-availability-infrastructure-wsfc-file-share.md
[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md
[sap-hana-ha]:sap-hana-high-availability.md
[sap-suse-ascs-ha]:high-availability-guide-suse.md

[planning-volumes-s2d-choosing-filesystem]:https://docs.microsoft.com/windows-server/storage/storage-spaces/plan-volumes#choosing-the-filesystem
[choosing-the-size-of-volumes-s2d]:https://docs.microsoft.com/windows-server/storage/storage-spaces/plan-volumes#choosing-the-size-of-volumes
[deploy-sofs-s2d-in-azure]:https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-storage-spaces-direct-deployment
[s2d-in-win-2016]:https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/storage-spaces-direct-overview
[deep-dive-volumes-in-s2d]:https://blogs.technet.microsoft.com/filecab/2016/08/29/deep-dive-volumes-in-spaces-direct/

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

[1869038]:https://launchpad.support.sap.com/#/notes/1869038 

# <a name="cluster-an-sap-ascsscs-instance-on-a-windows-failover-cluster-by-using-a-file-share-in-azure"></a>Azure üzerinde bir dosya paylaşımı kullanarak bir SAP ASCS/SCS örneği Windows Yük devretme kümesinde Küme

> ![Windows][Logo_Windows] Windows
>

Windows Server Yük Devretme Kümelemesi yüksek kullanılabilirlik SAP ASCS/SCS yükleme ve Windows'da DBMS oluşturmaktadır.

Bir yük devretme kümesi, uygulamaların ve hizmetlerin kullanılabilirliğini artırmak için birlikte çalışan 1 + n bağımsız sunucular (düğümler) grubudur. Bir düğüm hatası oluşursa, Windows Server Yük Devretme Kümelemesi oluşur ve hala uygulamaları ve hizmetleri sağlamak için iyi bir küme korumak hatalarının sayısını hesaplar. Yük Devretme Kümelemesi elde etmek için farklı bir çekirdek modlarından seçebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede açıklanan görevler başlamadan önce bu makalede gözden geçirin:

* [Azure sanal makineleri yüksek oranda kullanılabilirlik mimarisi ve SAP NetWeaver senaryoları][sap-high-availability-architecture-scenarios]

> [!IMPORTANT]
> Bir dosya paylaşımı kullanarak SAP ASCS/SCS örnekleri kümeleme SAP NetWeaver 7.40 (ve daha sonra), SAP çekirdek 7.49 (ve daha sonra) desteklenir.
>


## <a name="windows-server-failover-clustering-in-azure"></a>Windows Server Yük Devretme Kümelemesi ile Azure

Tam veya özel bulut dağıtımları için ile karşılaştırıldığında, Azure sanal makineleri Windows Server Yük Devretme Kümelemesi yapılandırmak için ek adımlar gerektirir. Bir küme oluşturma sırasında birkaç IP adresleri ve sanal ana bilgisayar adları SAP ASCS/SCS örneği için ayarlamanız gerekir.

### <a name="name-resolution-in-azure-and-the-cluster-virtual-host-name"></a>Ad çözümlemesi Azure ve küme sanal ana bilgisayar adı

Azure bulut platformu kayan IP adresleri gibi sanal IP adreslerini yapılandırma seçeneği sağlamaz. Küme kaynağı bulutta ulaşmak için sanal bir IP adresi ayarlamak için alternatif bir çözüm gerekir. 

Azure Yük Dengeleyici Hizmeti sağlayan bir *iç yük dengeleyici* Azure için. İç yük dengeleyici ile istemcileri kümenin küme sanal IP adresi ulaşabilirsiniz. 

Küme düğümleri içeren kaynak grubunda iç yük dengeleyicisi dağıtın. Ardından, kuralları araştırma kullanarak iç yük dengeleyicisi bağlantı noktaları iletme tüm gerekli bağlantı noktası yapılandırın. İstemcileri sanal ana bilgisayar adını bağlanabilir. DNS sunucusu küme IP adresi çözümler. İç yük dengeleyicisi kümesinin etkin düğümü için bağlantı noktası iletme işler.

![Şekil 1: Windows Server Yük devretme kümeleme yapılandırmasında Azure olmayan paylaşılan bir disk][sap-ha-guide-figure-1001]

_**Şekil 1:** Azure yapılandırmasında bir paylaşılan disk olmayan Windows Server Yük devretme_

## <a name="sap-ascsscs-ha-with-file-share"></a>SAP ASCS/SCS HA dosya paylaşımı

SAP, yeni bir yaklaşım ve SAP ASCS/SCS örneği üzerinde Windows Yük Devretme Kümelemesi için Paylaşılan diskleri, küme için bir alternatif geliştirilmiştir. Küme Paylaşılan diskleri kullanmak yerine, bir SMB dosya paylaşımına SAP genel ana bilgisayar dosyaları dağıtmak için kullanabilirsiniz.

> [!NOTE]
> Bir SMB dosya paylaşımına SAP ASCS/SCS örnekleri kümeleme için Küme Paylaşılan diskleri kullanan bir alternatifidir.  
>

Bu mimari, aşağıdaki yollarla özeldir:

* SAP merkezi hizmetleriyle (kendi dosya yapısı ve ileti ve sıraya alma işlemleri) SAP genel ana bilgisayar dosyalarından ayrıdır.
* SAP merkezi hizmetlerinin SAP ASCS/SCS örneği altında çalıştırın.
* SAP ASCS/SCS örneğinin kümelenmiş olduğunu ve erişilebilir olduğundan kullanarak \<ASCS/SCS sanal ana bilgisayar adını\> sanal ana bilgisayar adı.
* SAP genel dosyaları SMB dosya paylaşımında yerleştirilir ve kullanılarak erişilen \<SAP genel ana bilgisayar\> ana bilgisayar adı: \\ \\ &lt;SAP genel ana bilgisayar&gt;\sapmnt\\ &lt;SID&gt;\SYS\....
* SAP ASCS/SCS örneği, her iki küme düğümünde bir yerel diskte yüklenir.
* \<ASCS/SCS sanal ana bilgisayar adını\> ağ adı farklı &lt;SAP genel ana bilgisayar&gt;.

![Şekil 2: SAP ASCS/SCS HA mimarisiyle SMB dosya paylaşımı][sap-ha-guide-figure-8004]

_**Şekil 2:** yeni SAP ASCS/SCS HA mimarisiyle bir SMB dosya paylaşımı_

SMB dosya paylaşımı için Önkoşullar:

* SMB 3.0 (veya üstü) protokolü.
* Active Directory erişimi denetim listeleri (ACL'ler) Active Directory kullanıcı grupları için ayarlayabilme ve `computer$` bilgisayar nesnesi.
* Dosya Paylaşımı HA etkinleştirilmiş olması gerekir:
    * Dosyaları depolamak için kullanılan diskler tek hata noktası olmaması gerekir.
    * Sunucu veya VM kapalı kalma süresi kapalı kalma süresi dosya paylaşımında neden olmaz.

SAP \<SID\> Küme Paylaşılan diskleri veya genel bir dosya paylaşımı küme kaynağının küme rolünü içermiyor.


![Şekil 3: SAP \<SID\> küme için bir dosya paylaşımı kullanarak rolü kaynaklar][sap-ha-guide-figure-8005]

_**Şekil 3:** SAP &lt;SID&gt; küme için bir dosya paylaşımı kullanarak rolü kaynaklar_


## <a name="scale-out-file-shares-with-storage-spaces-direct-in-azure-as-an-sapmnt-file-share"></a>Depolama alanları doğrudan bir SAPMNT dosya paylaşımı olarak Azure ile genişleme dosya paylaşımları

Bir genişleme dosya paylaşımında barındırabilir ve SAP genel ana bilgisayar dosyaları korumak için kullanabilirsiniz. Bir genişleme dosya paylaşımına da yüksek oranda kullanılabilir bir SAPMNT Dosya Paylaşımı hizmeti sunar.

![Şekil 4: SAP genel ana bilgisayar dosyaları korumak için kullanılan genişleme dosya paylaşımı][sap-ha-guide-figure-8006]

_**Şekil 4:** SAP genel ana bilgisayar dosyaları korumak için kullanılan bir genişleme dosya paylaşımı_

> [!IMPORTANT]
> Genişleme dosya paylaşımları, tam olarak Microsoft Azure Bulut ve şirket içi ortamlarında desteklenir.
>

Bir genişleme dosya paylaşımında bir yüksek oranda kullanılabilir ve ölçeklenebilir yatay SAPMNT dosya paylaşımı sunar.

Depolama alanları doğrudan bir genişleme dosya paylaşımı için paylaşılan bir disk olarak kullanılır. Depolama alanları doğrudan ile yerel depolama sunucuları kullanarak yüksek oranda kullanılabilir ve ölçeklenebilir depolama oluşturmak için kullanabilirsiniz. Paylaşılan bir genişleme dosya paylaşımı için kullanılan depolama alanı, SAP genel ana bilgisayar dosyaları için ister, tek bir hata noktası değil.

> [!IMPORTANT]
>Varsa, *sağlamadığı* olağanüstü durum kurtarma, ayarlamak için plan Azure yüksek oranda kullanılabilir dosya paylaşımı için bir çözüm olarak bir genişleme dosya paylaşımına kullanmanızı öneririz.
>

### <a name="sap-prerequisites-for-scale-out-file-shares-in-azure"></a>Genişleme dosya paylaşımları Azure SAP önkoşulları

Bir genişleme dosya paylaşımını kullanmak için sisteminizin aşağıdaki gereksinimleri karşılamalıdır:

* En az iki küme düğümü bir genişleme dosya paylaşımı için.
* Her düğüm, en az iki yerel disk olması gerekir.
* Performans nedeni kullanmalısınız *dayanıklılık yansıtma*:
    * İki yönlü bir genişleme dosya paylaşımına iki küme düğümü için yansıtma.
    * Üç yönlü yansıtma üç (veya daha fazla) küme düğümleri ile genişleme dosya paylaşımı için.
* Üç yönlü yansıtma ile bir genişleme dosya paylaşımı için üç (veya daha fazla) küme düğümleri öneririz.
    Bu kurulumu daha fazla ölçeklenebilirlik ve iki küme düğümlerini ve iki yönlü yansıtma genişleme dosya paylaşımına Kurulumu'den daha fazla depolama esneklik sunar.
* Azure Premium diskleri kullanmanız gerekir.
* Azure yönetilen diskleri kullanmanızı öneririz.
* Dayanıklı dosya sistemi (ReFS) kullanarak birimleri biçimlendirme öneririz.
    * Daha fazla bilgi için bkz: [SAP Not 1869038 - ReFs dosya sistemi SAP desteği] [ 1869038] ve [dosya sistemi seçme] [ planning-volumes-s2d-choosing-filesystem] bölüm Depolama alanları doğrudan planlama birimlerinde makalesi.
    * Yüklediğiniz mutlaka [Microsoft KB4025334 toplu güncelleştirme][kb4025334].
* DS serisi veya DSv2 serisi Azure VM boyutlarını kullanabilirsiniz.
* İyi bir ağ performansı, depolama alanları doğrudan disk eşitleme için gerekli olan, VM'ler arasında en az bir "Yüksek" ağ bant genişliği olan bir VM türü kullanın.
    Daha fazla bilgi için bkz: [DSv2 serisi] [ dv2-series] ve [DS serisi] [ ds-series] belirtimleri.
* Depolama havuzundaki bazı ayrılmamış kapasite yedek öneririz. Depolama havuzundaki bazı ayrılmamış kapasite bırakarak bir sürücü arızalanırsa "yerinde" onarmak için birimler sunacaktır. Bu, veri güvenliği ve performansı artırır.  Daha fazla bilgi için bkz: [birim boyutu seçme][choosing-the-size-of-volumes-s2d].
* Genişleme dosya paylaşımına Azure VM'ler, kendi Azure kullanılabilirlik kümesine dağıtılmalıdır.
* Genişleme dosya paylaşımı ağ adı için Azure iç yük dengeleyici gibi yapılandırmak gerekmez \<SAP genel ana bilgisayar\>. Bunu yapmanız \<ASCS/SCS sanal ana bilgisayar adını\> SAP ASCS/SCS örneğinin veya DBMS için. Bir genişleme dosya paylaşımında tüm küme düğümleri arasında yük ölçeklendirir. \<SAP genel ana bilgisayar\> tüm küme düğümleri için yerel IP adresini kullanır.


> [!IMPORTANT]
> İşaret SAPMNT dosya paylaşımı yeniden adlandırılamıyor \<SAP genel ana bilgisayar\>. Yalnızca paylaşım adı "sapmnt." SAP destekler

> Daha fazla bilgi için bkz: [paylaşım adı sapmnt değiştirilmesi SAP Not 2492395 - olabilir?][2492395]

### <a name="configure-sap-ascsscs-instances-and-a-scale-out-file-share-in-two-clusters"></a>İki kümelerde SCS örnekleri ve bir genişleme dosya paylaşımı/SAP ASCS yapılandırın

SAP ASCS/SCS örnekleri kendi SAP ile bir kümede dağıtabilirsiniz \<SID\> küme rolünü. Bu durumda, başka bir küme rolüne sahip başka bir küme üzerinde genişleme dosya paylaşımını yapılandırın.

> [!IMPORTANT]
>Bu senaryoda, SAP ASCS/SCS örneği UNC yolu kullanılarak SAP genel ana bilgisayar erişimi için yapılandırılmış \\ \\ &lt;SAP genel ana bilgisayar&gt;\sapmnt\\&lt;SID&gt;\SYS\.
>

![Şekil 5: SAP ASCS/SCS örneği ve iki kümelerinde dağıtılmış bir genişleme dosya paylaşımı][sap-ha-guide-figure-8007]

_**Şekil 5:** SAP ASCS/SCS örneği ve bir genişleme dosya paylaşımına iki kümelerinde dağıtılmış_

> [!IMPORTANT]
> Azure bulutta SAP ve genişleme dosya paylaşımları kendi Azure kullanılabilirlik dağıtılması için kullanılan her küme ayarlayın. Bu, dağıtılmış bir kümede sanal makineleri temel Azure altyapı yerleşimini sağlar.
>

## <a name="generic-file-share-with-sios-datakeeper-as-cluster-shared-disks"></a>Genel dosya paylaşımının SIOS Paylaşılan diskleri küme olarak DataKeeper


> [!IMPORTANT]
> Yüksek oranda kullanılabilir bir dosya paylaşımı için bir genişleme dosya paylaşımına çözüm öneririz.
>
> Ayrıca, yüksek oranda kullanılabilir dosya paylaşımı için olağanüstü durum kurtarma ayarlayın planlıyorsanız, Küme Paylaşılan diskler için genel bir dosya paylaşımı ve SISO DataKeeper kullanmanız gerekir.
>

Genel dosya paylaşımı yüksek oranda kullanılabilir bir dosya paylaşımı elde etmek için başka bir seçenek değil.

Bu durumda, Küme Paylaşılan bir disk olarak bir üçüncü taraf SIOS çözüm kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure altyapı SAP HA için SAP ASCS/SCS örneği için bir Windows Yük devretme kümesi ve dosya paylaşımı kullanarak hazırlama][sap-high-availability-infrastructure-wsfc-file-share]
* [SAP NetWeaver HA SAP ASCS/SCS örneği için bir Windows Yük devretme kümesi ve dosya paylaşım yükleyin.][sap-high-availability-installation-wsfc-shared-disk]
* [Azure'da UPD depolama için iki düğümlü depolama alanları doğrudan genişleme dosya sunucusu dağıtma][deploy-sofs-s2d-in-azure]
* [Depolama alanları doğrudan Windows Server 2016][s2d-in-win-2016]
* [Derin Dalış: depolama alanları doğrudan birimleri][deep-dive-volumes-in-s2d]
