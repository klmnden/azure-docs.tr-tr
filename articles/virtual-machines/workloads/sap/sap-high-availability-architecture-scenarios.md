---
title: Azure sanal makineler yüksek kullanılabilirlik mimarisi ve senaryolar için SAP NetWeaver | Microsoft Docs
description: Yüksek kullanılabilirlik mimarisi ve senaryolar için Azure sanal Makineler'de SAP NetWeaver
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 887caaec-02ba-4711-bd4d-204a7d16b32b
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/21/2019
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 37f5040585681a53743fb3426b7f7ffac36de51c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60936285"
---
# <a name="high-availability-architecture-and-scenarios-for-sap-netweaver"></a>Yüksek kullanılabilirlik mimarisi ve senaryolar için SAP NetWeaver

[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png


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
[sap-ascs-ha-multi-sid-wsfc-file-share]:sap-ascs-ha-multi-sid-wsfc-file-share.md
[sap-ascs-ha-multi-sid-wsfc-shared-disk]:sap-ascs-ha-multi-sid-wsfc-shared-disk.md
[sap-hana-ha]:sap-hana-high-availability.md
[sap-suse-ascs-ha]:high-availability-guide-suse.md
[sap-suse-ascs-ha-anf]:high-availability-guide-suse-netapp-files.md
[sap-higher-availability]:sap-higher-availability-architecture-scenarios.md

[planning-guide]:planning-guide.md  
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10
[planning-guide-3.1]:planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a
[planning-guide-3.2.1]:planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358
[planning-guide-3.2.2]:planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560
[planning-guide-3.2.3]:planning-guide.md#18810088-f9be-4c97-958a-27996255c665
[planning-guide-3.2]:planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77
[planning-guide-3.3.2]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92
[planning-guide-5.1.1]:planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53
[planning-guide-5.1.2]:planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c
[planning-guide-5.2.1]:planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef
[planning-guide-5.2.2]:planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3
[planning-guide-5.2]:planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7
[planning-guide-5.3.1]:planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79
[planning-guide-5.3.2]:planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a
[planning-guide-5.4.2]:planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1
[planning-guide-5.5.1]:planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646
[planning-guide-5.5.3]:planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d
[planning-guide-7.1]:planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30
[planning-guide-7]:planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14
[planning-guide-9.1]:planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3
[planning-guide-azure-premium-storage]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92

[virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]:../../windows/sql/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md
[virtual-machines-windows-portal-sql-alwayson-int-listener]:../../windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md

[sap-ha-bc-virtual-env-hyperv-vmware-white-paper]:https://scn.sap.com/docs/DOC-44415
[sap-ha-partner-information]:https://scn.sap.com/docs/DOC-8541
[azure-sla]:https://azure.microsoft.com/support/legal/sla/
[azure-virtual-machines-manage-availability]:https://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability
[azure-storage-redundancy]:https://azure.microsoft.com/documentation/articles/storage-redundancy/
[azure-storage-managed-disks-overview]:https://docs.microsoft.com/azure/storage/storage-managed-disks-overview

[planning-guide-figure-100]:media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:media/virtual-machines-shared-sap-planning-guide/planning-monitoring-overview-2502.png
[planning-guide-figure-2600]:media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-2901]:media/virtual-machines-shared-sap-planning-guide/2901-azure-ha-sap-ha-md.png
[planning-guide-figure-300]:media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-3201]:media/virtual-machines-shared-sap-planning-guide/3201-sap-ha-with-sql-md.png
[planning-guide-figure-400]:media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

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

[sap-ha-multi-sid-guide]:sap-high-availability-multi-sid.md (SAP çoklu SID yüksek kullanılabilirliği yapılandırma)


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


## <a name="terminology-definitions"></a>Terim tanımları

**Yüksek kullanılabilirlik**: Yedek, hataya dayanıklı veya yük devretme korumalı bileşenleri içinde BT Hizmetleri iş sürekliliği sağlayarak BT kesintileri en aza teknoloji kümesi başvurduğu *aynı* veri merkezi. Bu örnekte, bir Azure bölgesi içinde veri merkezinde yer alıyor.

**Olağanüstü durum kurtarma**: Ayrıca BT Hizmetleri kesintiye ve bunların kurtarma ancak boyunca en aza indirmek için başvuran *çeşitli* veri merkezleri, yüzlerce mil uzaktaki başka bir olabilir. Bu örnekte, veri merkezleri aynı coğrafi bölge içindeki çeşitli Azure bölgelerinde veya sizin tarafınızdan bir müşteri olarak belirlenen bir konumda bulunan.


## <a name="overview-of-high-availability"></a>Yüksek kullanılabilirlik'e genel bakış
Azure'da SAP yüksek kullanılabilirlik, üç tür olarak ayrılabilir:

* **Azure altyapı yüksek kullanılabilirlik**: 

    Örneğin, yüksek kullanılabilirlik (VM'ler) işlem, ağ veya depolama ve SAP uygulamaların kullanılabilirliğini artırmaya yönelik faydalarını içerebilir.

* **Azure altyapı VM kullanarak yeniden elde etmek için *daha yüksek kullanılabilirlik* SAP uygulama**: 

    Linux üzerinde Windows Server Yük Devretme Kümelemesi (WSFC) veya Pacemaker gibi işlevler kullanmamaya karar verirseniz, Azure VM yeniden başlatma için kullanılır. Bu, Azure fiziksel sunucu altyapısı ve genel bir temel alınan Azure platformunda SAP sistemlerini planlanmış ve planlanmamış kesinti süreleri karşı korur.

* **SAP uygulama yüksek kullanılabilirlik**: 

    Tam SAP sistemi yüksek kullanılabilirlik elde etmek için tüm kritik SAP sistemi bileşenlerini korumanız gerekir. Örneğin:
    * Yedekli SAP uygulama sunucuları.
    * Benzersiz bileşenleri. Bir örnek, tek bir SAP ASCS/SCS örneği veya veritabanı yönetim sistemi (DBMS) gibi başarısız (SPOF) bileşeni noktası olabilir.

Azure'da yüksek kullanılabilirlik SAP, SAP yüksek kullanılabilirlik, bir şirket içi fiziksel veya sanal ortamda farklıdır. Aşağıdaki incelemeyi [SAP NetWeaver-yüksek kullanılabilirlik ve VMware ve Microsoft Windows Hyper-V sanal ortamları'nda iş sürekliliği] [ sap-ha-bc-virtual-env-hyperv-vmware-white-paper] standart SAP yüksek kullanılabilirlik açıklar Windows üzerinde sanallaştırılmış ortamlarda yapılandırmalar.

Windows için de Linux için sapinst tümleşik SAP yüksek kullanılabilirlik yapılandırma yoktur. Linux ile SAP yüksek oranda kullanılabilir şirket içi hakkında bilgi için bkz. [yüksek kullanılabilirlik iş ortağı bilgileri][sap-ha-partner-information].

## <a name="azure-infrastructure-high-availability"></a>Azure altyapı yüksek kullanılabilirlik

### <a name="sla-for-single-instance-virtual-machines"></a>Tek Örnekli sanal makineler için SLA

Tek VM SLA'sı % 99,9 düzeyinde ile premium depolama şu anda yoktur. Tek bir sanal makine kullanılabilirliğini ne olabilir hakkında bir fikir edinmek için çeşitli kullanılabilir ürün oluşturabilirsiniz [Azure hizmet düzeyi sözleşmeleri][azure-sla].

Hesaplama için 30 gün başına aylık veya değer 43.200 dakikadır temelidir. Örneğin, %0,05 kapalı kalma süresi 21.6 dakika karşılık gelir. Her zamanki şekilde çeşitli Hizmetleri kullanılabilirliğini aşağıdaki şekilde hesaplanır:

(Kullanılabilirlik hizmet #1/100) * (kullanılabilirlik hizmet #2/100) * (kullanılabilirlik hizmet #3/100) \*...

Örneğin:

(% 99,95/100) * (% 99,9/100) * (% 99,9/100) = 0.9975 veya bir genel kullanılabilirliği %99.75.

### <a name="multiple-instances-of-virtual-machines-in-the-same-availability-set"></a>Sanal makineler aynı kullanılabilirlik kümesindeki birden çok örneği
Aynı şekilde dağıtılan iki veya daha fazla örneğe sahip tüm sanal makineler için *kullanılabilirlik kümesi*, sanal makine bağlantısı en az bir örnek için en az % 99,95 oranında bir süre için olacağını garanti ediyoruz.

Her bir sanal makine kullanılabilirlik kümesinde iki veya daha fazla sanal makine aynı kullanılabilirlik kümesinin parçası olduğunda, atanan bir *güncelleme etki alanı* ve *hata etki alanı* temel alınan Azure platformu tarafından.

* **Güncelleme etki alanları** garanti birden çok VM aynı zamanda Azure altyapısının planlı bakım sırasında yeniden değil. Yalnızca bir sanal makine bir anda yeniden başlatılır.

* **Hata etki alanları** garanti VM'ler, ortak bir güç kaynağı ve ağ anahtarını paylaşan olmayan donanım bileşenleri dağıtılır. Yalnızca bir VM, sunucular, ağ anahtarı veya güç kaynağı geçmeleri, planlanmamış bir kesinti etkilenir.

Daha fazla bilgi için [azure'daki Windows sanal makinelerin kullanılabilirliğini yönetme][azure-virtual-machines-manage-availability].

Bir kullanılabilirlik kümesi, yüksek oranda kullanılabilirliğini elde etmek için kullanılır:

* Yedekli SAP uygulama sunucuları.  
* SAP ASCS/SCS örneği veya DBMS gibi SPOFs korumak iki veya daha fazla düğüme (örneğin, VM'ler) sahip kümeler.


### <a name="azure-availability-zones"></a>Azure Kullanılabilirlik Alanları
Azure, kavramlarını çalışırken aşamasındayız [Azure kullanılabilirlik alanları](https://docs.microsoft.com/azure/availability-zones/az-overview) farklı boyunca [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/). Kullanılabilirlik alanları Burada sunulan Azure bölgelerinde Azure bölgelerinden bağımsız güç kaynağına, soğutma ve ağ kaynağı olan birden çok veri merkezine sahip. Tek bir Azure bölgesi içinde farklı bölgelere sunan bir nedeni, iki veya üç sunulan kullanılabilirlik alanında uygulamaları dağıtmanıza olanak sağlamaktır. Güç kaynağı ve/veya ağ sorunları, yalnızca bir kullanılabilirlik alanı altyapısını etkileyecek varsayılarak, bir Azure bölgesi içinde uygulama dağıtımınızı hala tamamen çalışır durumdadır. Sonuçta bir bölgedeki Vm'leri bu yana bazı sınırlı kapasite ile kaybolmuş olabilir. Ancak, diğer iki bölgeleri VM hala çalışıyor olduğundan. Bölgeleri sunan Azure bölgeleri içinde listelenen [Azure kullanılabilirlik alanları](https://docs.microsoft.com/azure/availability-zones/az-overview).

Kullanılabilirlik alanları kullanarak, dikkate alınması gereken bazı şeyler vardır. Dikkat edilecek noktalar listesi gibi:

- Bir kullanılabilirlik bölgesi içinde Azure kullanılabilirlik kümeleri dağıtamazsınız. Bir kullanılabilirlik alanı veya bir kullanılabilirlik kümesi için bir VM dağıtımı çerçevesi olarak seçmeniz gerekebilir.
- Kullanamazsınız [temel yük dengeleyici](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview#skus) yük devretme kümesi çözümleri Windows Yük devretme Küme Hizmetleri veya Linux Pacemaker göre oluşturmak için. Bunun yerine kullanmanız gereken [Azure standart yük dengeleyici SKU](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones)
- Azure kullanılabilirlik alanları tek bir bölgede farklı bölgeler arasında belirli bir mesafenin konusunda garanti verir değil
- Farklı Azure bölgeleri içinde farklı Azure kullanılabilirlik alanları arasındaki ağ gecikmesini bölgeden bölgeye Azure'dan farklı olabilir. Burada, bir müşteri makul bir şekilde dağıtılan SAP uygulama katmanı farklı bölgeler arasında ağ gecikme süresi bir bölgeden etkin DBMS VM'ye çalıştırabilirsiniz gibi bir iş işlemi etkisini hala kabul edilebilir durumlarda, olacaktır. Olacaktır ancak müşteri senaryoları burada etkin DBMS sanal makine bir bölgede ve başka bir bölgedeki bir sanal makinede bir SAP uygulama örneği arasında bir gecikme çok zorlayıcı ve SAP iş süreçleri için kabul edilemez olabilir. Sonuç olarak, dağıtım mimarisi gecikme süresi yüksek ise, uygulama için bir etkin/etkin mimari veya Aktif/Pasif mimari ile farklı olması gerekir.
- Kullanarak [Azure yönetilen diskler](https://azure.microsoft.com/services/managed-disks/) Azure kullanılabilirlik alanına dağıtmak için zorunludur 


### <a name="planned-and-unplanned-maintenance-of-virtual-machines"></a>Sanal makineler planlı ve Plansız bakım

Azure platformu olayları iki tür sanal makinelerinizin kullanılabilirliğini etkileyebilir:

* **Planlı Bakım** olaylar, Microsoft tarafından temel alınan Azure platformunda yapılan Periyodik güncelleştirmelerdir. Güncelleştirmeleri genel güvenilirlik, performans ve sanal makinelerinizin çalıştığı platforma altyapısının güvenliğini artırın.

* **Plansız bakım** olaylar, donanım veya sanal makinenizin altında yatan fiziksel altyapı bir şekilde başarısız olduğunda gerçekleşir. Yerel ağ hatalarında, yerel disk hataları veya raf düzeyinde diğer hatalar içerebilir. Böyle bir arıza tespit edildiğinde, Azure platformu sanal makinenizi sağlıklı bir fiziksel sunucuya barındıran sağlıksız fiziksel sunucudan sanal makinenizi otomatik olarak geçirir. Bu tür olaylar nadirdir, ancak sanal makinenizin yeniden başlatılmasına da neden.

Daha fazla bilgi için [azure'daki Windows sanal makinelerin kullanılabilirliğini yönetme][azure-virtual-machines-manage-availability].

### <a name="azure-storage-redundancy"></a>Azure depolama yedekliliği
Depolama hesabınızdaki veriler, dayanıklılık ve yüksek kullanılabilirlik, geçici donanım hataları karşılaşıldığında bile Azure depolama SLA'sı karşıladığından emin olmak için her zaman çoğaltılır.

Azure depolama, varsayılan olarak verilerin üç görüntü tuttuğundan, birden çok Azure disklerde RAID 5 ya da RAID 1 kullanımı gerekli değildir.

Daha fazla bilgi için [Azure depolama çoğaltma][azure-storage-redundancy].

### <a name="azure-managed-disks"></a>Azure Yönetilen Diskleri
Yönetilen diskler, bir Azure kaynak sanal sabit Azure depolama hesaplarında depolanan diskleri (VHD'ler) yerine kullanılacak önerilen Yöneticisi'nde kaynak türü. Yönetilen diskler ile Azure kullanılabilirlik kümesine bağlı sanal makinenin otomatik olarak hizalayın. Bunlar, sanal makinenizi ve üzerinde çalışan hizmetleri kullanılabilirliğini artırın.

Daha fazla bilgi için [Azure yönetilen disklere genel bakış][azure-storage-managed-disks-overview].

Dağıtım ve sanal makinelerinizin yönetimini basitleştirmek için yönetilen diskler kullanmanızı öneririz.



## <a name="utilizing-azure-infrastructure-high-availability-to-achieve-higher-availability-of-sap-applications"></a>Yüksek kullanılabilirlik elde etmek için Azure altyapı kullanan *daha yüksek kullanılabilirlik* SAP uygulama

(Şu anda yalnızca için SUSE Linux Enterprise Server [SLES] 12 ve sonrasında desteklenir) Linux üzerinde işlevler WSFC veya Pacemaker gibi kullanmamaya karar verirseniz, Azure VM yeniden başlatma için kullanılır. Bu, Azure fiziksel sunucu altyapısı ve genel bir temel alınan Azure platformunda SAP sistemlerini planlanmış ve planlanmamış kesinti süreleri karşı korur.

Bu yaklaşımı hakkında daha fazla bilgi için bkz: [kullanan Azure altyapı VM yeniden SAP sistemin daha yüksek kullanılabilirlik elde etmek için][sap-higher-availability].

## <a name="baed0eb3-c662-4405-b114-24c10a62954e"></a> Azure Iaas üzerindeki SAP uygulama yüksek kullanılabilirlik

Tam SAP sistemi yüksek kullanılabilirlik elde etmek için tüm kritik SAP sistemi bileşenlerini korumanız gerekir. Örneğin:
  * Yedekli SAP uygulama sunucuları.
  * Benzersiz bileşenleri. Bir örnek, tek bir SAP ASCS/SCS örneği veya veritabanı yönetim sistemi (DBMS) gibi başarısız (SPOF) bileşeni noktası olabilir.

Sonraki bölümlerde, tüm üç kritik SAP sistem bileşenleri için yüksek kullanılabilirlik elde etmek nasıl ele almaktadır.

### <a name="high-availability-architecture-for-sap-application-servers"></a>SAP uygulama sunucuları için yüksek kullanılabilirlik mimarisi

> Bu bölüm için geçerlidir:
>
> ![Windows][Logo_Windows] Windows ve ![Linux][Logo_Linux] Linux
>

Ayrıca SAP uygulama sunucusu ve iletişim örnekleri için genellikle belirli bir yüksek kullanılabilirlik çözümü gerekmez. Yedeklilik ile yüksek kullanılabilirlik elde edin ve iletişim birden fazla Azure sanal makineleri çeşitli örneklerini yapılandırın. Azure sanal makineleri iki örneğini en az iki SAP uygulama örneklerine sahip olmalıdır.

![Şekil 1: Yüksek kullanılabilirlik SAP uygulama sunucusu][sap-ha-guide-figure-2000]

_**Şekil 1:** Yüksek kullanılabilirlik SAP uygulama sunucusu_

Konak SAP uygulama sunucusu örneklerinin aynı Azure kullanılabilirlik kümesi tüm sanal makineler yerleştirmeniz gerekir. Azure kullanılabilirlik kümesine sağlar:

* Tüm sanal makineler aynı güncelleme etki alanı'nın bir parçasıdır.  
    Sanal makineler planlı bakım kapalı kalma süresinde aynı zamanda güncelleştirilmemiş bir güncelleme etki alanı sağlar.

    Farklı güncelleştirme ve hata etki alanları içindeki bir Azure ölçek birimi oluşturur, temel işlevleri de tanıtılan [güncelleştirme etki alanı] [ planning-guide-3.2.2] bölümü.

* Tüm sanal makineler aynı hata etki alanı'nın bir parçasıdır.  
    Hata etki alanı, böylece hiç tek hata noktası tüm sanal makinelerin kullanılabilirliğini etkileyen dağıtılan sanal makineler, sağlar.

Azure kullanılabilirlik kümesi içinde bir Azure ölçek birimi kullandığı güncelleştirme ve hata etki alanlarının sayısı sınırlıdır. Vm'leri tek bir kullanılabilirlik kümesine ekleme tutmak, iki veya daha fazla sanal makine aynı hata veya güncelleştirme etki alanında sonunda ulaşır.

Birkaç SAP uygulama sunucusu örneklerinde adanmış Vm'lerini dağıtma, biz beş güncelleştirme etki alanları olduğunu varsayarsak, aşağıdaki resimde ortaya çıkar. Gerçek sayısı bir kullanılabilirlik kümesi içinde güncelleştirme ve hata etki alanları gelecekte değişebilir:

![Şekil 2: SAP uygulama sunucuları bir Azure kullanılabilirlik kümesinde yüksek kullanılabilirliği][planning-guide-figure-3000]
_**Şekil 2:** Yüksek kullanılabilirlik, SAP uygulama sunucuları bir Azure kullanılabilirlik kümesi_

Daha fazla bilgi için [azure'daki Windows sanal makinelerin kullanılabilirliğini yönetme][azure-virtual-machines-manage-availability].

Daha fazla bilgi için [Azure kullanılabilirlik kümeleri] [ planning-guide-3.2.3] planlama Azure sanal makineleri ve SAP NetWeaver belge için uygulama bölümü.

**Yalnızca yönetilmeyen diskler:** Azure depolama hesabı, olası bir tek hata noktası olduğundan, en az iki Azure depolama hesapları, en az iki sanal makine dağıtılmasını sağlamak önemlidir. İdeal bir Kurulum, SAP iletişim örneği çalıştıran her sanal makinenin diskleri farklı depolama hesabında dağıtılabilir.

> [!IMPORTANT]
> Azure kullanmanızı öneririz yönetilen diskler, SAP yüksek kullanılabilirliğe sahip yüklemeleri için. Yönetilen diskleri otomatik olarak bağlandıkları sanal makinenin kullanılabilirlik kümesini hizalamak için bunlar, sanal makinenizin ve üzerinde çalışan hizmetleri kullanılabilirliğini artırın.  
>

### <a name="high-availability-architecture-for-an-sap-ascsscs-instance-on-windows"></a>Windows üzerinde SAP ASCS/SCS örneği için yüksek kullanılabilirlik mimarisi

> ![Windows][Logo_Windows] Windows
>

SAP ASCS/SCS örneği korumak için bir WSFC çözümü kullanabilirsiniz. Çözümü iki çeşidi vardır:

* **SAP ASCS/SCS örneği kümelenmiş Paylaşılan diskleri kullanarak küme**: Bu mimari hakkında daha fazla bilgi için bkz: [SAP ASCS/SCS örneği, bir küme paylaşılan disk kullanarak bir Windows Yük devretme kümesinde Küme][sap-high-availability-guide-wsfc-shared-disk].   

* **Dosya paylaşımı kullanarak SAP ASCS/SCS örneği küme**: Bu mimari hakkında daha fazla bilgi için bkz: [SAP ASCS/SCS örneği dosya paylaşımı kullanarak bir Windows Yük devretme kümesinde Küme][sap-high-availability-guide-wsfc-file-share].

### <a name="high-availability-architecture-for-an-sap-ascsscs-instance-on-linux"></a>Linux'ta SAP ASCS/SCS örneği için yüksek kullanılabilirlik mimarisi

> ![Linux][Logo_Linux] Linux
> 
> SLES küme çerçevesini kullanarak SAP ASCS/SCS örneği Kümelemesi hakkında daha fazla bilgi için bkz. [uygulamalar SAP için SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde SAP NetWeaver için yüksek kullanılabilirlik] [ sap-suse-ascs-ha]. SLES alternatif HA mimarisi için hangi gerektirmez yüksek oranda kullanılabilir bir NFS bkz [SUSE Linux Enterprise Server, SAP uygulamalarını Azure NetApp dosyaları üzerinde SAP NetWeaver için yüksek kullanılabilirlik Kılavuzu] [ sap-suse-ascs-ha-anf].

Red Hat küme çerçevesini kullanarak SAP ASCS/SCS örneği Kümelemesi hakkında daha fazla bilgi için bkz. [Azure sanal makineler Red Hat Enterprise Linux üzerinde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel)


### <a name="sap-netweaver-multi-sid-configuration-for-a-clustered-sap-ascsscs-instance"></a>Kümelenmiş bir SAP ASCS/SCS örneği için SAP NetWeaver çoklu SID yapılandırma

> ![Windows][Logo_Windows] Windows
> 
> Şu anda çoklu SID yalnızca WSFC ile desteklenir. Çoklu SID, dosya paylaşımı paylaşılan disk ile desteklenir.
> 
> Çoklu SID yüksek kullanılabilirlik mimarisi hakkında daha fazla bilgi için bkz:

* [SAP ASCS/SCS çoklu SID yüksek kullanılabilirlik, Windows Server Yük Devretme Kümelemesi ve dosya paylaşımı için örnek.][sap-ascs-ha-multi-sid-wsfc-file-share]

* [SAP ASCS/SCS çoklu SID yüksek kullanılabilirlik için paylaşılan disk Windows Server Yük Devretme Kümelemesi ve örnek][sap-ascs-ha-multi-sid-wsfc-shared-disk]

### <a name="high-availability-dbms-instance"></a>Yüksek kullanılabilirlik DBMS örneği

DBMS ayrıca tek bir iletişim noktası uçtaki bir SAP sistemiyle ' dir. Yüksek kullanılabilirlik çözümü kullanarak korumak gerekir. Azure'da bir SQL Server AlwaysOn yüksek kullanılabilirlik çözümü aşağıdaki şekilde gösterilmiştir, Windows Server Yük Devretme Kümelemesi ve Azure iç yük dengeleyici. SQL Server AlwaysOn, kendi DBMS çoğaltma kullanılarak DBMS veri ve günlük dosyalarını çoğaltır. Bu durumda, tüm kurulumu kolaylaştırır Küme Paylaşılan disk gerekmez.

![Şekil 3: SQL Server AlwaysOn ile yüksek oranda kullanılabilir bir SAP DBMS örneği][sap-ha-guide-figure-2003]

_**Şekil 3:** SQL Server AlwaysOn ile yüksek oranda kullanılabilir bir SAP DBMS örneği_

Azure Resource Manager dağıtım modelini kullanarak azure'da SQL Server DBMS Kümelemesi hakkında daha fazla bilgi için şu makalelere bakın:

* [AlwaysOn Kullanılabilirlik grubu Resource Manager'ı kullanarak Azure sanal makineler'de el ile yapılandırın.][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]

* [Azure'da AlwaysOn Kullanılabilirlik grubu için bir Azure iç yük dengeleyici yapılandırma][virtual-machines-windows-portal-sql-alwayson-int-listener]

Azure Resource Manager dağıtım modelini kullanarak azure'da SAP HANA DBMS Kümelemesi hakkında daha fazla bilgi için bkz. [Azure sanal makinelerinde (VM'ler) SAP hana yüksek kullanılabilirlik][sap-hana-ha].
