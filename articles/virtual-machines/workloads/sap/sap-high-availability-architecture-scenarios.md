---
title: "Azure sanal makinelerini yüksek kullanılabilirlik mimarisi ve SAP NetWeaver senaryoları | Microsoft Docs"
description: "Yüksek kullanılabilirlik (HA) mimarisi ve Azure sanal makinelerde SAP NetWeaver senaryoları"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 887caaec-02ba-4711-bd4d-204a7d16b32b
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 867fe2835418a48e4e616d8137ba9fa4182c8fb7
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2017
---
# <a name="high-availability-architecture-and-scenarios-for-sap-netweaver"></a>Yüksek kullanılabilirlik mimarisi ve SAP NetWeaver senaryoları

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

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

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

[sap-ha-bc-virtual-env-hyperv-vmware-white-paper]:http://scn.sap.com/docs/DOC-44415
[sap-ha-partner-information]:http://scn.sap.com/docs/DOC-8541
[azure-sla]:https://azure.microsoft.com/support/legal/sla/
[azure-virtual-machines-manage-availability]:http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability
[azure-storage-redundancy]:http://azure.microsoft.com/documentation/articles/storage-redundancy/
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

[sap-ha-multi-sid-guide]:sap-high-availability-multi-sid.md (SAP multi-SID high-availability configuration)


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


## <a name="definition-of-terminologies"></a>Terimler tanımı

Terim **yüksek kullanılabilirlik (HA)** iş sürekliliği BT hizmetlerinin yoluyla sağlayarak BT kesintilerini en aza indirir teknoloji ilgili yedekli, hataya dayanıklı veya yük devretme korumalı içindekibileşenler**aynı** veri merkezi. Örneğimizde, içinde bir Azure bölgesi.

**Olağanüstü Durum Kurtarma (DR)** BT Hizmetleri kesintiyi en aza indirmenizi ve bunların kurtarma ayrıca ancak çapraz hedeflediği **farklı** bulunduğu yüzlerce kilometre uzakta olan veri merkezlerinde. Örneğimizde genellikle aynı coğrafi bölge içindeki veya bir müşteri olarak sizin tarafınızdan yerleşik olarak farklı Azure bölgeler arasında.


## <a name="overview-of-high-availability"></a>Yüksek kullanılabilirlik genel bakış
Biz SAP üç kısma azure'da yüksek kullanılabilirlik hakkında tartışma ayırabilirsiniz:

* **Azure altyapı yüksek kullanılabilirlik**, örneğin HA (VM) işlem, ağ, depolama vb. ve onun avantajlarını SAP uygulama kullanılabilirliği artırma için.

* **"Yüksek" SAP uygulamalarının kullanılabilirliğini elde etmek için kullanan Azure altyapı VM yeniden başlatma**

  Linux üzerinde Windows Server Yük Devretme Kümelemesi (WSFC) veya Pacemaker gibi işlevler kullanmamaya karar verirseniz, Azure VM yeniden SAP sistemi planlanan ve planlanmamış kesinti süreleri Azure fiziksel sunucu altyapısı ve genel karşı korumak için kullanılır temel alınan Azure platformu.


* **SAP uygulama yüksek kullanılabilirlik**

  Tam SAP sistem yüksek kullanılabilirlik elde etmek için tüm kritik SAP sistem bileşenleri, örneğin korumak ihtiyacımız var:
  * Yedek **SAP uygulama sunucuları**, ve
  * Benzersiz bileşenler (örneğin **tek hata noktası (SPOF)**) gibi
    * **(A) SCS örneği SAP** ve
    *  **DBMS**.


Azure'da yüksek kullanılabilirlik SAP SAP yüksek kullanılabilirlik için bir şirket içi fiziksel veya sanal ortamda karşılaştırıldığında bazı farklar vardır. Aşağıdaki kağıt [SAP NetWeaver yüksek kullanılabilirlik ve iş sürekliliği VMware ve Microsoft Windows Hyper-V sanal ortamlarda] [ sap-ha-bc-virtual-env-hyperv-vmware-white-paper] standart SAP yüksek kullanılabilirlik açıklar yapılandırmalar Windows sanallaştırılmış ortamlarda.

Windows için mevcut değil gibi sapinst tümleşik SAP-HA için yapılandırmaya Linux yoktur. SAP HA ile ilgili şirket içi Linux bulma hakkında daha fazla bilgi [yüksek kullanılabilirlik iş ortağı bilgileri][sap-ha-partner-information].

## <a name="azure-infrastructure-high-availability"></a>Azure altyapı yüksek kullanılabilirlik

### <a name="single-instance-virtual-machine-sla"></a>Tek örnek sanal makine SLA

Şu anda bir tek VM SLA premium depolama alanına sahip % 99,9. Nasıl tek bir VM'ye kullanılabilirliğini görünebilir gibi bir fikir edinmek için kullanılabilen ürün farklı oluşturabilirsiniz [Azure hizmet düzeyi sözleşmeleri][azure-sla].

30 gün içinde her ay veya değer 43.200 dakikadır hesaplama temelidir. Bu nedenle, %0,05 kapalı kalma süresi 21.6 dakika karşılık gelir. Her zamanki gibi farklı hizmetlerin kullanılabilirliğini aşağıdaki şekilde çarpın:

(Kullanılabilirlik hizmeti #1/100) * (kullanılabilirlik hizmeti #2/100) * (kullanılabilirlik hizmeti #3/100) \*...

Benzer:

(99,95/100) * (99,9/100) * (99,9/100) = 0.9975 veya %99.75 genel kullanılabilirlik.

### <a name="multiple-instances-of-virtual-machines-in-the-same-availability-set"></a>Birden çok örnekleri, sanal makineler aynı kullanılabilirlik kümesindeki
İki veya daha fazla örneğinin aynı dağıtılan tüm sanal makineler için **kullanılabilirlik kümesi**, sahip olduğunuz sanal makine bağlantısı için en az bir örnek süresinin en az % 99,95 garanti ediyoruz.

Her bir sanal makine kullanılabilirlik kümesindeki iki veya daha fazla sanal makineleri aynı kullanılabilirlik kümesine parçası olduğunda, atanan bir **güncelleştirme etki alanı** ve **hata etki alanı** temel Azure platformu tarafından.

**Hata etki alanları** VM'ler ortak güç kaynağı ve ağ anahtarı paylaşmaz farklı donanım üzerinde dağıtılan güvence altına alır. Planlanmamış kapalı kalma süresi sunucuları, ağ anahtarı veya güç kaynağı, yalnızca bir VM zaman etkilenir.

**Güncelleme etki alanları** farklı VM'ler değil aynı anda Azure altyapısının planlı bakım sırasında yeniden ancak aynı anda yalnızca bir VM yeniden garantiler.

Daha fazla bilgi için bkz: [azure'da Windows sanal makinelerin kullanılabilirliğini yönetme][azure-virtual-machines-manage-availability].

Kullanılabilirlik kümesi, yüksek düzeyde kullanılabilirliğini sağlamak için kullanılır:

* Yedek SAP uygulama sunucuları  

* SAP (A) SCS örneği ve DBMS SPOFs korumak iki veya daha fazla düğüm (örneğin, sanal makineleri) içeren kümeler gibi

### <a name="virtual-machine-vm-planned-and-unplanned-maintenance"></a>Sanal makine (VM) planlanmış ve planlanmamış bakım

İki tür sanal makinelerin kullanılabilirliğini etkileyebilecek Azure platformu olay vardır: planlanan Bakım ve planlanmayan Bakım.

* **Planlı Bakım** genel güvenilirliği, performansı ve sanal makinelerinizi çalıştıracağınız platform altyapısının güvenliğini artırmak için temel alınan Azure platformu için Microsoft tarafından oluşturulan düzenli güncelleştirmeleri olaylardır.

* **Plansız bakım** olayları donanım ya da sanal makineniz için temel alınan fiziksel altyapı herhangi bir yolla hatalı olduğunda oluşur. Buna yerel ağ hataları, yerel disk hataları veya raf düzeyinde diğer hatalar dahildir. Bu tür bir arıza tespit edildiğinde Azure platformu sağlıklı bir fiziksel sunucu, sanal makinenize barındırma sağlıksız fiziksel sunucudan sanal makineniz otomatik olarak geçmektedir. Bu tür olaylar nadirdir, ancak sanal makinenizin yeniden başlatılmasına da neden olabilir.

Daha fazla bilgi için bkz: [azure'da Windows sanal makinelerin kullanılabilirliğini yönetme][azure-virtual-machines-manage-availability].

### <a name="azure-storage-redundancy"></a>Azure depolama artıklığı
Microsoft Azure depolama hesabınızdaki veriler dayanıklılık ve yüksek kullanılabilirlik, Azure depolama SLA geçici donanım arızalarında bile karşısında toplantı emin olmak için her zaman çoğaltılır.

Azure Storage üç görüntülerini verileri varsayılan olarak engelliyor olduğundan, RAID5 veya birden çok Azure disklere RAID1 yok gerekli.

Daha fazla bilgi için bkz: [Azure Storage çoğaltma][azure-storage-redundancy].

### <a name="azure-managed-disks"></a>Azure Yönetilen Diskleri
Yeni bir kaynak türü Azure kaynağı Azure depolama hesaplarında depolanan VHD yerine kullanılan Yöneticisi'nde yönetilen disklerdir. Yönetilen diskleri otomatik olarak bağlı olan sanal makinenin kullanılabilirlik kümesi hizalamak ve bu nedenle, sanal makine ve sanal makinede çalışan hizmetlerin kullanılabilirliğini artırmak.
Daha fazla bilgi için bkz: [Azure yönetilen diskleri genel bakış][azure-storage-managed-disks-overview].

İçin önerdiğimiz dağıtım ve sanal makinelerin yönetimini basitleştirmek için yönetilen disk kullanın.
**SAP şu anda yönetilen Premium diskleri yalnızca destekler**. Daha fazla bilgi için SAP not okuma [1928533].

## <a name="utilizing-azure-infrastructure-ha-to-achieve-sap-application-higher-availability"></a>Azure altyapı SAP uygulama "Daha yüksek" kullanılabilirlik elde etmek için HA kullanma

Linux üzerinde Windows Server Yük Devretme Kümelemesi (WSFC) veya Pacemaker gibi işlevler kullanmamaya karar verirseniz (şu anda SLES 12 ve daha yüksek yalnızca desteklenir), Azure VM yeniden başlatma kullanılan bir SAP sistemi Azure planlanmış ve Planlanmamış kapalı kalma süresi karşı korumak için fiziksel sunucu altyapısı ve genel temel Azure platformu.

Bu yaklaşım daha aşağıdaki belgede açıklanan [kullanılarak Azure altyapı VM yeniden Achieve SAP sisteminin "daha yüksek kullanılabilirlik"][sap-higher-availability].

## <a name="baed0eb3-c662-4405-b114-24c10a62954e"></a>SAP Azure Iaas uygulama yüksek kullanılabilirlik

Tam SAP sistem yüksek kullanılabilirlik elde etmek için tüm kritik SAP sistem bileşenleri, örneğin korumak ihtiyacımız var:

* Yedek **SAP uygulama sunucuları**, ve

* Benzersiz bileşenler (örneğin **tek hata noktası (SPOF)**) gibi
  * **(A) SCS örneği SAP** ve
  *  **DBMS**.

Tüm üç kritik SAP sistem bileşenleri için yüksek kullanılabilirlik elde etmek nasıl aşağıda ayrıntılı ele.

### <a name="high-availability-for-sap-application-servers"></a>SAP uygulama sunucuları için yüksek kullanılabilirlik

> Bu bölümde her ikisi için geçerlidir:
>
> ![Windows][Logo_Windows] Windows ve ![Linux][Logo_Linux] Linux
>

SAP uygulama sunucusu ile iletişim örnekleri için belirli bir yüksek kullanılabilirlik çözümü genellikle gerekmez. Artıklık tarafından yüksek kullanılabilirlik elde etmek ve iletişim birden çok başka durumlarda, Azure sanal makineleri yapılandırın. İki durumlarda, Azure sanal makinelerinde yüklü en az iki SAP uygulama örneğinin olması gerekir.

![Şekil 1: Yüksek oranda kullanılabilirlik SAP uygulama sunucusu][sap-ha-guide-figure-2000]

_**Şekil 1:** yüksek kullanılabilirlik SAP uygulama sunucusu_

Ana bilgisayar SAP uygulama sunucusu örneklerinin aynı Azure kullanılabilirlik kümesi tüm sanal makineler yerleştirmeniz gerekir. Bir Azure kullanılabilirlik kümesi sağlar:

* Tüm sanal makineleri aynı parçası olan **yükseltme etki alanı**. Bir yükseltme etki alanı, örneğin, sanal makineler planlı bakım kapalı kalma süresi sırasında aynı anda güncelleştirilmemiş emin olur.
Farklı yükseltme ve bir Azure ölçek birimi hata etki alanlarını temel işlevleri zaten bölümde sunulan [yükseltme etki alanları][planning-guide-3.2.2].

* Tüm sanal makineleri aynı parçası olan **hata etki alanı**. Hata etki alanı, örneğin, böylece hiç tek hata noktası tüm sanal makinelerin kullanılabilirliğini etkiler dağıtılan sanal makineler olduğundan emin olur.

Hiçbir sonsuz sayıda hata ve yükseltme Azure ölçek birimi içinde Azure kullanılabilirlik kümesi tarafından kullanılan etki alanları yoktur. Bu, bir kullanılabilirlik kümesi, er geç birden fazla VM aynı hatası veya yükseltme etki alanında sona eriyor içine VM'lerin sayısını koyma anlamına gelir.

Birkaç SAP uygulama sunucu örneklerinde kendi özel VM'ler dağıtma ve beş yükseltme etki alanları elde ettiğinizi varsayarak, aşağıdaki resimde sonunda ortaya çıkar. Hataya ve güncelleştirme etki alanı bir kullanılabilirlik kümesi içinde gerçek maksimum sayısı gelecekte değişebilir:

![Şekil 2: HA SAP uygulama sunucuları Azure kullanılabilirlik kümesindeki][planning-guide-figure-3000]
_**Şekil 2:** HA, SAP uygulama sunucuları Azure kullanılabilirlik kümesindeki_ bu belgede daha fazla ayrıntı bulunabilir: [sanal makinelerin kullanılabilirliğini yönetme][virtual-machines-manage-availability].


Azure kullanılabilirlik kümeleri bölümde sunulan [Azure kullanılabilirlik kümeleri] [ planning-guide-3.2.3] Azure sanal makineleri planlama ve uygulama SAP NetWeaver belge için.


**Yalnızca yönetilmeyen disk:** Azure depolama hesabına olası tek hata noktası olduğundan, en az iki sanal makineye dağıtılır, en az iki Azure depolama hesabınızın olması önemlidir. İdeal bir kurulumunda SAP iletişim örneği çalıştıran her bir sanal makine disklerinin farklı depolama hesabında dağıtılması.

> [!IMPORTANT]
>
> Öneririz, SAP HA yüklemelerinde yönetilen Azure diskleri kullanın, bunlar otomatik olarak sanal makinenin kullanılabilirlik kümesi ile hizalamak için bağlı olan ve bu nedenle, sanal makine kullanılabilirliğini artırmak ve sanal makine üzerinde çalışan hizmetleri.  
>


### <a name="high-availability-architecture-for-the-sap-ascs-instance"></a>SAP (A) SCS örneği için yüksek kullanılabilirlik mimarisi

### <a name="high-availability-architecture-for-the-sap-ascs-instance-on-windows"></a>Windows SAP (A) SCS örneğinde için yüksek kullanılabilirlik mimarisi


> ![Windows][Logo_Windows] Windows
>

Kullanabileceğiniz **Windows Server Yük Devretme Kümelemesi** (**WSFC**) SAP (A) SCS örneğini korumak için çözüm. Çözüm iki çeşidini vardır:

* SAP (A) SCS örneği kullanarak Kümeleme **Paylaşılan diskleri kümelenmiş**

   Bu belgede kümelenmiş paylaşılan diskler ile HA mimarisi hakkında daha fazla bilgi bulabilirsiniz: [kümeleme SAP (A) SCS Windows Yük devretme kümesi kullanarak küme paylaşılan Disk örneğinde][sap-high-availability-guide-wsfc-shared-disk].   

* SAP (A) SCS örneği kullanarak Kümeleme **dosya paylaşımı**

  Bu belgede dosya paylaşımı ile HA mimarisi hakkında daha fazla bilgi bulabilirsiniz: [kümeleme SAP (A) SCS örneğinde Windows Yük devretme kümesi kullanılarak dosya paylaşımı][sap-high-availability-guide-wsfc-file-share].

### <a name="high-availability-for-the-sap-ascs-instance-on-linux"></a>Linux SAP (A) SCS örneğinde için yüksek kullanılabilirlik


> ![Linux][Logo_Linux] Linux
>

Bu belgede SUSE Linux Enterprise Server küme çerçevesi kullanarak SAP (A) SCS örneğini Kümelemesi hakkında daha fazla bilgi bulabilirsiniz: [SAPuygulamalarıiçinyüksekkullanılabilirlikiçinSAPNetWeaverSUSELinuxEnterpriseServerüzerindeAzurevm'lerinde][sap-suse-ascs-ha].

### <a name="sap-netweaver-multi-sid-configuration-for-clustered-sap-ascs-instance"></a>SAP NetWeaver çoklu SID yapılandırmasını kümelenmiş SAP (A) SCS örnek

> ![Windows][Logo_Windows] Windows
>
>Şu anda çoklu SID yalnızca ile desteklenir **Windows Server Yük devretme kümesi (WSFC)**. Çoklu SID kullanarak desteklenir **dosya paylaşımı** ve **paylaşılan disk**.
>

Bu mimari belgelerde çoklu SID HA mimarisi hakkında daha fazla bilgi bulabilirsiniz:

* [SAP (A) SCS Windows Server Yük Devretme Kümelemesi ile çoklu SID yüksek kullanılabilirlik için örnek ve dosya paylaşımını][sap-ascs-ha-multi-sid-wsfc-file-share]

* [SAP (A) SCS Windows Server Yük Devretme Kümelemesi ile çoklu SID yüksek kullanılabilirlik için örnek ve paylaşılan Disk][sap-ascs-ha-multi-sid-wsfc-shared-disk]

### <a name="high-availability-dbms-instance"></a>Yüksek kullanılabilirlik DBMS örneği

DBMS de tek bir kişinin bir SAP sisteminde noktasıdır. Yüksek kullanılabilirlik çözümü kullanarak korumanız gerekir. Aşağıdaki şekil, Azure'da bir SQL Server Always On yüksek kullanılabilirlik çözümü gösterir, Windows Server Yük Devretme Kümelemesi ile Azure iç yük dengeleyici. SQL Server Always On kendi DBMS çoğaltma kullanılarak DBMS veri ve günlük dosyalarını çoğaltır. Bu durumda, tüm Kurulum basitleştirir Küme Paylaşılan disk gerekmez.

![Şekil 3: SQL Server Always On ile bir yüksek kullanılabilirlik SAP DBMS örneği][sap-ha-guide-figure-2003]

_**Şekil 3:** SQL Server Always On ile bir yüksek kullanılabilirlik SAP DBMS örneği_

Kümeleme hakkında daha fazla bilgi için **SQL Server DBMS** Azure'da, Azure Resource Manager dağıtım modeli kullanarak, bu makalelere bakın:

* [Always On kullanılabilirlik grubu Resource Manager kullanarak Azure sanal makinelerinde el ile yapılandırın.][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]

* [Azure'da Azure iç yük dengeleyiciye Always On kullanılabilirlik grubu için yapılandırın][virtual-machines-windows-portal-sql-alwayson-int-listener]

Kümeleme hakkında daha fazla bilgi için **SAP HANA DBMS** bu makalede Azure Resource Manager dağıtım modelini kullanarak Azure'da bakın:

* [SAP HANA Azure sanal makinelerde (VM), yüksek kullanılabilirlik][sap-hana-ha]
