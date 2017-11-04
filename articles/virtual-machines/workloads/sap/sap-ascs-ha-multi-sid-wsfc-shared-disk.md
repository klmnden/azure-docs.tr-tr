---
title: "SAP (A) SCS örneği Windows Server Yük Devretme Kümelemesi ile çoklu SID yüksek kullanılabilirlik ve Azure diskte paylaşılan | Microsoft Docs"
description: "Çoklu SID yüksek kullanılabilirlik için Windows Server Yük Devretme Kümelemesi (A) SCS örneğiyle SAP ve Azure diskte paylaşılan"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: cbf18abe-41cb-44f7-bdec-966f32c89325
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 31892e334d649c66e86b6c9812ffb18b069f718b
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2017
---
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1869038]:https://launchpad.support.sap.com/#/notes/1869038
[2287140]:https://launchpad.support.sap.com/#/notes/2287140
[2492395]:https://launchpad.support.sap.com/#/notes/2492395

[sap-installation-guides]:http://service.sap.com/instguides

[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md
[networking-limits-azure-resource-manager]:../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits
[load-balancer-multivip-overview]:../../../load-balancer/load-balancer-multivip-overview.md


[sap-net-weaver-ports]:https://help.sap.com/viewer/ports
[sap-high-availability-architecture-scenarios]:sap-high-availability-architecture-scenarios.md
[sap-high-availability-guide-wsfc-shared-disk]:sap-high-availability-guide-wsfc-shared-disk.md
[sap-high-availability-guide-wsfc-file-share]:sap-high-availability-guide-wsfc-file-share.md
[sap-ascs-high-availability-multi-sid-wsfc]:sap-ascs-high-availability-multi-sid-wsfc.md
[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md
[sap-hana-ha]:sap-hana-high-availability.md
[sap-suse-ascs-ha]:high-availability-guide-suse.md
[sap-net-weaver-ports-ascs-scs-ports]:sap-high-availability-infrastructure-wsfc-shared-disk.md#0f3ee255-b31e-4b8a-a95a-d9ed6200468b

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md

[deployment-guide]:deployment-guide.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md

[ha-guide]:sap-high-availability-guide.md

[planning-guide]:planning-guide.md
[planning-guide-11]:planning-guide.md
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10

[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[sap-high-availability-architecture-scenarios]:sap-high-availability-architecture-scenarios.md
[sap-high-availability-guide-wsfc-shared-disk]:sap-high-availability-guide-wsfc-shared-disk.md
[sap-high-availability-guide-wsfc-file-share]:sap-high-availability-guide-wsfc-file-share.md
[sap-ascs-high-availability-multi-sid-wsfc]:sap-ascs-high-availability-multi-sid-wsfc.md
[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-infrastructure-wsfc-file-share]:sap-high-availability-infrastructure-wsfc-file-share.md

[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md
[sap-high-availability-installation-wsfc-shared-disk-install-ascs]:sap-high-availability-installation-wsfc-shared-disk.md#31c6bd4f-51df-4057-9fdf-3fcbc619c170
[sap-high-availability-installation-wsfc-shared-disk-modify-ascs-profile]:sap-high-availability-installation-wsfc-shared-disk.md#e4caaab2-e90f-4f2c-bc84-2cd2e12a9556
[sap-high-availability-installation-wsfc-shared-disk-add-probe-port]:sap-high-availability-installation-wsfc-shared-disk.md#10822f4f-32e7-4871-b63a-9b86c76ce761
[sap-high-availability-installation-wsfc-shared-disk-win-firewall-probe-port]:sap-high-availability-installation-wsfc-shared-disk.md#4498c707-86c0-4cde-9c69-058a7ab8c3ac
[sap-high-availability-installation-wsfc-shared-disk-change-ers-service-startup-type]:sap-high-availability-installation-wsfc-shared-disk.md#094bc895-31d4-4471-91cc-1513b64e406a
[sap-high-availability-installation-wsfc-shared-disk-test-ascs-failover-and-sios-repl]:sap-high-availability-installation-wsfc-shared-disk.md#18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9


[sap-high-availability-installation-wsfc-file-share]:sap-high-availability-installation-wsfc-file-share.md
[sap-high-availability-infrastructure-wsfc-shared-disk-install-sios]:sap-high-availability-infrastructure-wsfc-shared-disk.md#5c8e5482-841e-45e1-a89d-a05c0907c868

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

[sap-ha-guide-figure-6001]:media/virtual-machines-shared-sap-high-availability-guide/6001-sap-multi-sid-ascs-scs-sid1.png
[sap-ha-guide-figure-6002]:media/virtual-machines-shared-sap-high-availability-guide/6002-sap-multi-sid-ascs-scs.png
[sap-ha-guide-figure-6003]:media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png
[sap-ha-guide-figure-6004]:media/virtual-machines-shared-sap-high-availability-guide/6004-sap-multi-sid-dns.png
[sap-ha-guide-figure-6005]:media/virtual-machines-shared-sap-high-availability-guide/6005-sap-multi-sid-azure-portal.png
[sap-ha-guide-figure-6006]:media/virtual-machines-shared-sap-high-availability-guide/6006-sap-multi-sid-sios-replication.png



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

# <a name="sap-ascs-instance-multi-sid-high-availability-with-windows-server-failover-clustering-and-shared-disk-on-azure"></a>SAP (A) SCS Windows Server Yük Devretme Kümelemesi ile çoklu SID yüksek kullanılabilirlik örneği ve Azure diskte paylaşılan

> ![Windows][Logo_Windows] Windows
>

Eylül 2016'da, Microsoft yönetebileceğiniz birden çok sanal IP adresi kullanarak bir özelliği yayımlanan bir [Azure iç yük dengeleyici][load-balancer-multivip-overview]. Bu işlev Azure dış yük dengeleyicide zaten var.

Bir SAP dağıtımınız varsa, SAP ASCS/SCS için bir Windows Küme yapılandırması oluşturmak için bir iç yük dengeleyici kullanmak zorunda.

Bu makalede ek SAP ASCS/kümelenmiş örneklerini içine bir mevcut Windows Server Yük Devretme Kümelemesi (WSFC) küme ile SCS yükleyerek tek bir ASCS/SCS yüklemesinden bir SAP çoklu SID yapılandırmasına taşıma odaklanır **paylaşılan disk**. Bu işlem tamamlandığında, bir SAP çoklu SID küme yapılandırdınız.

> [!NOTE]
>
> Bu özellik yalnızca kullanılabilir **Azure Resource Manager** dağıtım modeli.
>
>Her Azure iç yük dengeleyici için özel ön uç IP sayısı için bir sınır yoktur.
>
>Bir WSFC kümesinin SAP ASCS/SCS örneği maksimum sayısı üst sınırını her Azure iç yük dengeleyici için özel ön uç IP eşittir.
>

Yük Dengeleyici sınırları hakkında daha fazla bilgi için bkz: "Özel ön uç IP yük dengeleyici başına" [ağ sınırları: Azure Resource Manager][networking-limits-azure-resource-manager].

## <a name="prerequisites"></a>Ön koşullar

İçin kullanılan bir WSFC kümesi zaten yapılandırmış olduğunuz **bir** SAP ASCS/SCS örneğini kullanarak **dosya paylaşımı**Bu diyagramda gösterildiği gibi.

![Yüksek kullanılabilirlik SAP ASCS/SCS örneği][sap-ha-guide-figure-6001]

> [!IMPORTANT]
> Kurulum, aşağıdaki koşulları karşılaması gerekir:
> * SAP ASCS/SCS örnekleri aynı WSFC küme paylaşması gerekir.
> * Her DBMS SID kendi adanmış WSFC kümesi olması gerekir.
> * Bir SAP sistem SID'si ait SAP uygulama sunucuları, kendi özel VM'ler olması gerekir.

## <a name="sap-ascs-multi-sid-architecture-with-shared-disk"></a>(A) paylaşılan Disk SCS çoklu SID mimarisiyle SAP

Hedef birden çok SAP ABAP ASCS yüklemektir veya SAP Java SCS burada Resimli olarak aynı WSFC kümesinde örneklerinin kümelenmiş:

![Azure birden çok SAP ASCS/SCS kümelenmiş örneğinde][sap-ha-guide-figure-6002]

Yük Dengeleyici sınırları hakkında daha fazla bilgi için bkz: "Özel ön uç IP yük dengeleyici başına" [ağ sınırları: Azure Resource Manager][networking-limits-azure-resource-manager].

İki yüksek kullanılabilirlik SAP sistemleriyle tam yatay şöyle olabilir:

![SAP yüksek kullanılabilirlik multi-SID Kurulum iki SAP sistemiyle SID][sap-ha-guide-figure-6003]

## <a name="25e358f8-92e5-4e8d-a1e5-df7580a39cb0"></a>SAP çoklu SID senaryosu için altyapı Hazırlık

Altyapınızı hazırlamak için aşağıdaki parametrelerle ek SAP ASCS/SCS örneğini yükleyebilirsiniz:

| Parametre adı | Değer |
| --- | --- |
| SAP ASCS/SCS SID |pr1 lb ascs |
| SAP DBMS iç yük dengeleyici | PR5 |
| SAP sanal ana bilgisayar adı | pr5 sap cl |
| SAP ASCS/SCS sanal ana bilgisayar IP adresi (ek Azure yük dengeleyici IP adresi) | 10.0.0.50 |
| SAP ASCS/SCS örnek sayısı | 50 |
| Ek SAP ASCS/SCS örnek için ILB araştırmasını bağlantı noktası | 62350 |

> [!NOTE]
> SAP ASCS/SCS küme örnekleri için her IP adresi benzersiz sonda bağlantı noktası gerektirir. Örneğin, bir IP adresinde bir Azure iç yük dengeleyici araştırması bağlantı noktası 62300 kullanıyorsa, herhangi bir IP adresi, yük dengeleyicide sonda bağlantı noktası 62300 kullanabilirsiniz.
>
>Sonda bağlantı noktası 62300 zaten ayrılmış olduğundan bizim amaçlar için araştırma bağlantı noktası 62350 kullanıyoruz.

Mevcut WSFC kümesinde iki düğümü ek SAP ASCS/SCS örnekleri yükleyebilirsiniz:

| Sanal makine rolü | Sanal makine ana bilgisayar adı | Statik IP adresi |
| --- | --- | --- |
| İlk küme düğümüne ASCS/SCS örneği için |pr1 ascs 0 |10.0.0.10 |
| İkinci küme düğümü ASCS/SCS örneği için |pr1 ascs 1 |10.0.0.9 |

### <a name="create-a-virtual-host-name-for-the-clustered-sap-ascsscs-instance-on-the-dns-server"></a>Kümelenmiş SAP ASCS/SCS örneği için bir sanal ana bilgisayar adı DNS sunucusunda oluşturun

Aşağıdaki parametreleri kullanarak ASCS/SCS örneğinin sanal ana bilgisayar adı için bir DNS girişi oluşturabilirsiniz:

| Yeni SAP ASCS/SCS sanal ana bilgisayar adı | İlişkili IP adresi |
| --- | --- | --- |
|pr5 sap cl |10.0.0.50 |

Yeni ana bilgisayar adı ve IP adresi DNS Yöneticisi'nde, aşağıdaki ekran görüntüsünde gösterildiği gibi görüntülenir:

![DNS Yöneticisi listesini yeni SAP ASCS/SCS için tanımlanmış DNS girişi vurgulama küme sanal adı ve TCP/IP adresi][sap-ha-guide-figure-6004]

Bir DNS girişi oluşturma yordamı da ana [Windows sanal makineleri üzerinde yüksek kullanılabilirlik SAP NetWeaver için Kılavuzu] ayrıntılı açıklanan [sap-ha-Kılavuzu-9.1.1].

> [!NOTE]
> Ek ASCS/SCS örneğinin sanal ana bilgisayar adı atamak yeni IP adresi SAP Azure yük dengeleyiciye atanan yeni IP adresi ile aynı olması gerekir.
>
>Senaryomuzda 10.0.0.50 IP adresidir.

### <a name="add-an-ip-address-to-an-existing-azure-internal-load-balancer-by-using-powershell"></a>PowerShell kullanarak bir IP adresi var olan bir Azure iç yük dengeleyici ekleme

Aynı WSFC kümesinde birden fazla SAP ASCS/SCS örneği oluşturmak için bir IP adresi var olan bir Azure iç yük dengeleyici eklemek için PowerShell kullanın. Her IP adresi, kendi Yük Dengeleme kuralları, araştırma bağlantı noktası, ön uç IP havuzu ve arka uç havuzu gerektirir.

Aşağıdaki komut dosyasını yeni bir IP adresi için var olan bir yük dengeleyici ekler. Ortamınız için PowerShell değişkenleri güncelleştirin. Komut dosyası tüm SAP ASCS/SCS bağlantı noktaları için gereken tüm Yük Dengeleme kuralları oluşturur.

```powershell

# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>
Clear-Host
$ResourceGroupName = "SAP-MULTI-SID-Landscape"      # Existing resource group name
$VNetName = "pr2-vnet"                        # Existing virtual network name
$SubnetName = "Subnet"                        # Existing subnet name
$ILBName = "pr2-lb-ascs"                      # Existing ILB name                      
$ILBIP = "10.0.0.50"                          # New IP address
$VMNames = "pr2-ascs-0","pr2-ascs-1"          # Existing cluster virtual machine names
$SAPInstanceNumber = 50                       # SAP ASCS/SCS instance number: must be a unique value for each cluster
[int]$ProbePort = "623$SAPInstanceNumber"     # Probe port: must be a unique value for each IP and load balancer

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName

$count = $ILB.FrontendIpConfigurations.Count + 1
$FrontEndConfigurationName ="lbFrontendASCS$count"
$LBProbeName = "lbProbeASCS$count"

# Get the Azure VNet and subnet
$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

# Add second front-end and probe configuration
Write-Host "Adding new front end IP Pool '$FrontEndConfigurationName' ..." -ForegroundColor Green
$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id
$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 10  | Set-AzureRmLoadBalancer

# Get new updated configuration
$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName
# Get new updated LP FrontendIP COnfig
$FEConfig = Get-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

# Add new back-end configuration into existing ILB
$BackEndConfigurationName  = "backendPoolASCS$count"
Write-Host "Adding new backend Pool '$BackEndConfigurationName' ..." -ForegroundColor Green
$BEConfig = Add-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB | Set-AzureRmLoadBalancer

# Get new updated config
$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

# Assign VM NICs to backend pool
$BEPool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
foreach($VMName in $VMNames){
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)        
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName                
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools += $BEPool
        Write-Host "Assigning network card '$NICName' of the '$VMName' VM to the backend pool '$BackEndConfigurationName' ..." -ForegroundColor Green
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        #start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name
}


# Create load-balancing rules
$Ports = "445","32$SAPInstanceNumber","33$SAPInstanceNumber","36$SAPInstanceNumber","39$SAPInstanceNumber","5985","81$SAPInstanceNumber","5$SAPInstanceNumber`13","5$SAPInstanceNumber`14","5$SAPInstanceNumber`16"
$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName
$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

Write-Host "Creating load balancing rules for the ports: '$Ports' ... " -ForegroundColor Green

foreach ($Port in $Ports) {

        $LBConfigrulename = "lbrule$Port" + "_$count"
        Write-Host "Creating load balancing rule '$LBConfigrulename' for the port '$Port' ..." -ForegroundColor Green

        $ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $HealthProbe -Protocol tcp -FrontendPort  $Port -BackendPort $Port -IdleTimeoutInMinutes 30 -LoadDistribution Default -EnableFloatingIP   
}

$ILB | Set-AzureRmLoadBalancer

Write-Host "Succesfully added new IP '$ILBIP' to the internal load balancer '$ILBName'!" -ForegroundColor Green

```
Betik çalıştıktan sonra sonuçları aşağıdaki ekran görüntüsünde gösterildiği gibi Azure portalında görüntülenir:

![Azure portalında yeni ön uç IP havuzu][sap-ha-guide-figure-6005]

### <a name="add-disks-to-cluster-machines-and-configure-the-sios-cluster-share-disk"></a>Makineleri kümelemek eklemek ve SIOS küme paylaşım Disk yapılandırma

Her ek SAP ASCS/SCS örneği için yeni bir küme paylaşım disk eklemeniz gerekir. Windows Server 2012 R2 için WSFC küme paylaşım disk şu anda kullanımda SIOS DataKeeper yazılım çözümüdür.

Şunları yapın:
1. Bir ek disk veya diskler (hangi şeritler gerek) aynı boyutta her küme düğümleri eklemek ve biçimlendirir.
2. Depolama çoğaltma SIOS DataKeeper ile yapılandırın.

Bu yordam WSFC küme makinelerde SIOS DataKeeper zaten yüklediyseniz varsayar. Yüklediyseniz, makineler arası çoğaltma şimdi yapılandırmanız gerekir. İşlem ayrıntılı bölümde açıklanan [SAP ASCS/SCS küme paylaşım diski için SIOS DataKeeper Cluster Edition][sap-high-availability-infrastructure-wsfc-shared-disk-install-sios].  

![DataKeeper eş zamanlı disk yeni SAP ASCS/SCS paylaşmak için yansıtma][sap-ha-guide-figure-6006]

### <a name="deploy-vms-for-sap-application-servers-and-dbms-cluster"></a>SAP uygulama sunucuları ve DBMS küme için sanal makineleri dağıtma

Altyapı hazırlık ikinci SAP sistemi için tamamlamak için aşağıdakileri yapın:

1. SAP uygulama sunucuları için özel VM'ler dağıtın ve bunları kendi özel kullanılabilirlik grubunda yerleştirin.
2. DBMS küme için özel VM'ler dağıtın ve bunları kendi özel kullanılabilirlik grubunda yerleştirin.

## <a name="sap-netweaver-multi-sid-installation"></a>SAP NetWeaver Multi-SID yükleme

İkinci bir SAP SID2 sistemi yükleme tam İşlem Kılavuzu'nda açıklanan [SAP NetWeaver HA yüklemesinde Windows Yük devretme kümesi ve SAP (A) SCS örnek için paylaşılan Disk][sap-high-availability-installation-wsfc-shared-disk].

Üst düzey yordam aşağıdaki gibidir:

1. [Bir yüksek kullanılabilirlik ASCS/SCS örneğiyle SAP yüklemek][sap-high-availability-installation-wsfc-shared-disk-install-ascs].  
 Bu adımda, SAP bir yüksek kullanılabilirlik ASCS/SCS örneğiyle üzerinde yüklemekte olduğunuz **mevcut WSFC küme düğümü 1**.

2. [ASCS/SCS örneğinin SAP profilini değiştirmek][sap-high-availability-installation-wsfc-shared-disk-modify-ascs-profile].

3. [Bir araştırma bağlantı noktası yapılandırın][sap-high-availability-installation-wsfc-shared-disk-add-probe-port].  
 Bu adımda, PowerShell kullanarak bir SAP küme kaynağı SAP SID2 IP sonda bağlantı noktası yapılandırmış olursunuz. Bu yapılandırma SAP ASCS/SCS küme düğümlerinden birinin yürütün.

4. Veritabanı örneğini yükleyin.  
 İkinci küme yüklemek için SAP Yükleme Kılavuzu'ndaki adımları izleyin.

5. İkinci küme düğümüne yükleyin.  
 Bu adımda, mevcut WSFC Küme düğüm 2 üzerinde bir yüksek kullanılabilirlik ASCS/SCS örneği SAP yüklüyorsunuz. İkinci küme yüklemek için SAP Yükleme Kılavuzu'ndaki adımları izleyin.

6. SAP ASCS/SCS örneği ve ProbePort için Windows Güvenlik Duvarı bağlantı noktalarını açın.  

 SAP ASCS/SCS örnekleri için kullanılan her iki küme düğümlerinde SAP ASCS/SCS tarafından kullanılan tüm Windows Güvenlik Duvarı bağlantı noktaları açıyorsunuz. Bu SAP ASCS / SCS örneği bağlantı noktalarını bölümde listelenen [SAP ASCS / SCS bağlantı noktalarını][sap-net-weaver-ports-ascs-scs-ports].

 Tüm diğer SAP bağlantı noktalarının listesini burada listelenir: [tüm SAP ürünlerin TCP/IP bağlantı noktalarını][sap-net-weaver-ports].  

 Ayrıca bizim senaryosunda 62350 açıklandığı gibi olduğundan Azure iç yük dengeleyici araştırması bağlantı açmak [burada][sap-high-availability-installation-wsfc-shared-disk-win-firewall-probe-port].

7. [SAP ERS Windows hizmet örneği başlangıç türünü değiştirme][sap-high-availability-installation-wsfc-shared-disk-change-ers-service-startup-type].

8. SAP birincil uygulama sunucusu yükleme

   SAP birincil uygulama sunucusu yeni adanmış VM SAP Yükleme Kılavuzu'nda açıklandığı şekilde yükleyin.  

9. SAP ek uygulama sunucusu yükleme

   SAP ek uygulama sunucusu yeni adanmış VM SAP Yükleme Kılavuzu'nda açıklandığı şekilde yükleyin.

10. [SIOS çoğaltma ve SAP ASCS/SCS örneği yük devretme testi][sap-high-availability-installation-wsfc-shared-disk-test-ascs-failover-and-sios-repl].

## <a name="next-steps"></a>Sonraki Adımlar

- [Ağ sınırları: Azure Resource Manager][networking-limits-azure-resource-manager]
- [Azure için birden çok Vip yük dengeleyici][load-balancer-multivip-overview]
