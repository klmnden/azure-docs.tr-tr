---
title: SAP ASCS/SCS örneği paylaşılan disk azure'da Windows Server Yük Devretme Kümelemesi ve yüksek kullanılabilirlikle çoklu SID | Microsoft Docs
description: Windows Server Yük Devretme Kümelemesi ve Azure üzerinde paylaşılan disk ile SAP ASCS/SCS örneği için yüksek kullanılabilirlik çoklu SID
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: cbf18abe-41cb-44f7-bdec-966f32c89325
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: dbc21922be66c793e76882cbd145f19681684252
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59786905"
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

[dr-guide-classic]:https://go.microsoft.com/fwlink/?LinkID=521971

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

# <a name="sap-ascsscs-instance-multi-sid-high-availability-with-windows-server-failover-clustering-and-shared-disk-on-azure"></a>Paylaşılan disk azure'da Windows Server Yük Devretme Kümelemesi ve yüksek kullanılabilirlikle çoklu SID SAP ASCS/SCS örneği

> ![Windows][Logo_Windows] Windows
>

Eylül 2016'da, Microsoft yönetebileceğiniz birden çok sanal IP adresi kullanarak bir özelliği yayımlanan bir [Azure iç yük dengeleyici][load-balancer-multivip-overview]. Bu işlev Azure dış yük dengeleyicide zaten var. 

SAP dağıtımınızı varsa, Windows Küme yapılandırması için SAP Central Services'in (ASCS/SCS) örnekleri oluşturmak için iç yük dengeleyici kullanmanız gerekir.

Bu makalede, paylaşılan disk ile bir Windows Server Yük Devretme Kümelemesi (WSFC) kümesine ek SAP ASCS/SCS kümelenmiş örneklerini yükleyerek tek bir ASCS/SCS yüklemesinden bir SAP çoklu SID yapılandırmasına taşıma odaklanır. Bu işlem tamamlandığında, bir SAP çoklu SID kümesini yapılandırdınız.

> [!NOTE]
> Bu özellik yalnızca Azure Resource Manager dağıtım modelinde kullanılabilir.
>
>Her Azure iç yük dengeleyici için özel ön uç IP sayısı bir sınırlama yoktur.
>
>SAP ASCS/SCS örneği bir WSFC kümesinde en fazla sayısını özel ön uç IP'ler için her bir Azure iç Yük Dengeleyiciyi maksimum sayısına eşittir.
>

Yük Dengeleyici sınırları hakkında daha fazla bilgi için bkz: "Yük Dengeleyici başına özel ön uç IP" bölümünde [ağ Limitleri: Azure Resource Manager][networking-limits-azure-resource-manager].

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Kullanarak bir SAP ASCS/SCS örneği için kullanılacak bir WSFC kümesi zaten yapılandırmış olmanız **dosya paylaşımı**Bu diyagramda gösterildiği gibi.

![Yüksek kullanılabilirlik SAP ASCS/SCS örneği][sap-ha-guide-figure-6001]

> [!IMPORTANT]
> Kurulum, aşağıdaki koşulları karşılaması gerekir:
> * SAP ASCS/SCS örneği aynı WSFC kümesi paylaşmanız gerekir.
> * Her veritabanı yönetim sistemi (DBMS) SID, kendi özel WSFC kümesine sahip olmalıdır.
> * Bir SAP sistemine SID ait SAP uygulama sunucuları, kendi özel VM'ler olması gerekir.

## <a name="sap-ascsscs-multi-sid-architecture-with-shared-disk"></a>Paylaşılan disk ile SAP ASCS/SCS çoklu SID mimarisi

Hedef birden çok SAP ABAP ASCS yüklemektir veya SAP Java SCS örneği aynı WSFC kümesinde, Resimli olarak burada kümelenmiş:

![Azure'da birden çok SAP ASCS/SCS kümelenmiş örneği][sap-ha-guide-figure-6002]

Yük Dengeleyici sınırları hakkında daha fazla bilgi için bkz: "Yük Dengeleyici başına özel ön uç IP" bölümünde [ağ Limitleri: Azure Resource Manager][networking-limits-azure-resource-manager].

İki yüksek kullanılabilirlik SAP sistemlerini ile tam yatay şöyle görünür:

![SAP çoklu SID yüksek kullanılabilirlik Kurulum iki SAP sistemiyle SID][sap-ha-guide-figure-6003]

## <a name="25e358f8-92e5-4e8d-a1e5-df7580a39cb0"></a> Bir SAP çoklu SID senaryosu için altyapıyı hazırlama

Altyapınızı hazırlamak için aşağıdaki parametrelerle bir ek SAP ASCS/SCS örneği yükleyebilirsiniz:

| Parametre adı | Değer |
| --- | --- |
| SAP ASCS/SCS SID |pr1 lb ascs |
| SAP DBMS iç yük dengeleyici | PR5 |
| SAP sanal ana bilgisayar adı | pr5 sap Temizle |
| SAP ASCS/SCS sanal ana bilgisayar IP adresi (ek Azure yük dengeleyici IP adresi) | 10.0.0.50 |
| SAP ASCS/SCS örneği sayısı | 50 |
| ILB araştırma bağlantı noktası ek SAP ASCS/SCS örneği için | 62350 |

> [!NOTE]
> SAP ASCS/SCS kümesi örnekleri için benzersiz bir araştırma bağlantı noktası her IP adresi gerektirir. Örneğin, herhangi bir IP adresi, yük dengeleyicideki bir Azure iç yük dengeleyici üzerindeki bir IP adresi, yoklama bağlantı noktası 62300 kullanıyorsa, yoklama bağlantı noktası 62300 kullanabilirsiniz.
>
>Araştırma bağlantı noktası 62300 zaten ayrılmış olduğundan, amaçlarımız doğrultusunda, yoklama bağlantı noktası 62350 kullanıyoruz.

Ek SAP ASCS/SCS örnekleri, mevcut WSFC kümesinde iki düğüm ile yükleyebilirsiniz:

| Sanal makine rolü | Sanal makine konak adı | Statik IP adresi |
| --- | --- | --- |
| İlk küme düğümüne ASCS/SCS örneği için |pr1 ascs 0 |10.0.0.10 |
| ASCS/SCS örneği için ikinci küme düğümü |pr1 ascs 1 |10.0.0.9 |

### <a name="create-a-virtual-host-name-for-the-clustered-sap-ascsscs-instance-on-the-dns-server"></a>Kümelenmiş SAP ASCS/SCS örneği için bir sanal ana bilgisayar adı DNS sunucusunda oluşturma

Aşağıdaki parametreleri kullanarak sanal ana bilgisayar adı ASCS/SCS örneği için bir DNS girişi oluşturabilirsiniz:

| Yeni SAP ASCS/SCS sanal ana bilgisayar adı | İlişkili IP adresi |
| --- | --- |
|pr5 sap Temizle |10.0.0.50 |

Yeni ana bilgisayar adı ve IP adresi DNS Yöneticisi'nde, aşağıdaki ekran görüntüsünde gösterildiği gibi görüntülenir:

![Küme sanal adı ve TCP/IP adresi DNS Yöneticisi listesi tanımlanmış DNS girişini yeni SAP ASCS/SCS için vurgulama][sap-ha-guide-figure-6004]

> [!NOTE]
> Sanal ana bilgisayar adını ek ASCS/SCS örneği için atadığınız yeni IP adresini SAP Azure yük dengeleyiciye atanan yeni IP adresiyle aynı olmalıdır.
>
>Senaryomuzdaki ise 10.0.0.50 IP adresidir.

### <a name="add-an-ip-address-to-an-existing-azure-internal-load-balancer-by-using-powershell"></a>PowerShell kullanarak bir IP adresi için var olan bir Azure iç yük dengeleyici Ekle

Birden fazla SAP ASCS/SCS örneği aynı WSFC kümesinde oluşturmak için bir IP adresi için var olan bir Azure iç yük dengeleyici eklemek için PowerShell kullanın. Her IP adresi, kendi Yük Dengeleme kuralları, araştırma bağlantı noktasını, ön uç IP havuzu ve arka uç havuzu gerektirir.

Aşağıdaki komut dosyasını yeni bir IP adresi için var olan bir yük dengeleyici ekler. PowerShell benzeri değişkenleri ortamınız için güncelleştirin. Betik, tüm gerekli Yük Dengeleme kuralları tüm SAP ASCS/SCS bağlantı noktaları oluşturur.

```powershell

# Select-AzSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>
Clear-Host
$ResourceGroupName = "SAP-MULTI-SID-Landscape"      # Existing resource group name
$VNetName = "pr2-vnet"                        # Existing virtual network name
$SubnetName = "Subnet"                        # Existing subnet name
$ILBName = "pr2-lb-ascs"                      # Existing ILB name                      
$ILBIP = "10.0.0.50"                          # New IP address
$VMNames = "pr2-ascs-0","pr2-ascs-1"          # Existing cluster virtual machine names
$SAPInstanceNumber = 50                       # SAP ASCS/SCS instance number: must be a unique value for each cluster
[int]$ProbePort = "623$SAPInstanceNumber"     # Probe port: must be a unique value for each IP and load balancer

$ILB = Get-AzLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName

$count = $ILB.FrontendIpConfigurations.Count + 1
$FrontEndConfigurationName ="lbFrontendASCS$count"
$LBProbeName = "lbProbeASCS$count"

# Get the Azure virtual network and subnet
$VNet = Get-AzVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName
$Subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

# Add a second front-end and probe configuration
Write-Host "Adding new front end IP Pool '$FrontEndConfigurationName' ..." -ForegroundColor Green
$ILB | Add-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id
$ILB | Add-AzLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 10  | Set-AzLoadBalancer

# Get a new updated configuration
$ILB = Get-AzLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

# Get an updated LP FrontendIpConfig
$FEConfig = Get-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

# Add a back-end configuration into an existing ILB
$BackEndConfigurationName  = "backendPoolASCS$count"
Write-Host "Adding new backend Pool '$BackEndConfigurationName' ..." -ForegroundColor Green
$BEConfig = Add-AzLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB | Set-AzLoadBalancer

# Get an updated config
$ILB = Get-AzLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

# Assign VM NICs to the back-end pool
$BEPool = Get-AzLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
foreach($VMName in $VMNames){
        $VM = Get-AzVM -ResourceGroupName $ResourceGroupName -Name $VMName
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)        
        $NIC = Get-AzNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName                
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools += $BEPool
        Write-Host "Assigning network card '$NICName' of the '$VMName' VM to the backend pool '$BackEndConfigurationName' ..." -ForegroundColor Green
        Set-AzNetworkInterface -NetworkInterface $NIC
        #start-AzVM -ResourceGroupName $ResourceGroupName -Name $VM.Name
}


# Create load-balancing rules
$Ports = "445","32$SAPInstanceNumber","33$SAPInstanceNumber","36$SAPInstanceNumber","39$SAPInstanceNumber","5985","81$SAPInstanceNumber","5$SAPInstanceNumber`13","5$SAPInstanceNumber`14","5$SAPInstanceNumber`16"
$ILB = Get-AzLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName
$FEConfig = get-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$BEConfig = Get-AzLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

Write-Host "Creating load balancing rules for the ports: '$Ports' ... " -ForegroundColor Green

foreach ($Port in $Ports) {

        $LBConfigrulename = "lbrule$Port" + "_$count"
        Write-Host "Creating load balancing rule '$LBConfigrulename' for the port '$Port' ..." -ForegroundColor Green

        $ILB | Add-AzLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $HealthProbe -Protocol tcp -FrontendPort  $Port -BackendPort $Port -IdleTimeoutInMinutes 30 -LoadDistribution Default -EnableFloatingIP   
}

$ILB | Set-AzLoadBalancer

Write-Host "Successfully added new IP '$ILBIP' to the internal load balancer '$ILBName'!" -ForegroundColor Green

```
Betiği çalıştırdıktan sonra sonuçları aşağıdaki ekran görüntüsünde gösterildiği gibi Azure Portalı'nda görüntülenir:

![Azure portalında yeni ön uç IP havuzu][sap-ha-guide-figure-6005]

### <a name="add-disks-to-cluster-machines-and-configure-the-sios-cluster-share-disk"></a>Küme makinelere disk ekleyin ve SIOS Küme Paylaşımı disk yapılandırma

Her ek SAP ASCS/SCS örneği için yeni bir küme paylaşımı disk eklemeniz gerekir. Windows Server 2012 R2 için WSFC Küme Paylaşımı disk şu anda kullanımda SIOS DataKeeper yazılım çözümüdür.

Şunları yapın:
1. Ek bir disk veya diskler (gereken stripe) aynı boyutta, her küme düğümlerine ekleyin ve biçimlendirir.
2. Depolama çoğaltma, SIOS DataKeeper ile yapılandırın.

Bu yordam WSFC küme makinelerde SIOS DataKeeper zaten yüklediğinizi varsayar. Yüklediyseniz, çoğaltma makineler arasında şimdi yapılandırmanız gerekir. İşlem, ayrıntılı olarak açıklanan [SAP ASCS/SCS Küme Paylaşımı diski için SIOS DataKeeper Cluster Edition][sap-high-availability-infrastructure-wsfc-shared-disk-install-sios].  

![Yeni SAP ASCS/SCS disk paylaşmak için yansıtma zaman uyumlu DataKeeper][sap-ha-guide-figure-6006]

### <a name="deploy-vms-for-sap-application-servers-and-the-dbms-cluster"></a>SAP uygulama sunucuları ve DBMS küme için Vm'leri dağıtma

İkinci SAP sistemine altyapı hazırlığı tamamlamak için aşağıdakileri yapın:

1. SAP uygulama sunucuları için ayrılmış Vm'leri dağıtma ve kendi özel kullanılabilirlik grubundaki her yerleştirin.
2. DBMS küme için ayrılmış Vm'leri dağıtma ve kendi özel kullanılabilirlik grubundaki her yerleştirin.

## <a name="install-an-sap-netweaver-multi-sid-system"></a>Bir SAP NetWeaver çoklu SID sistemi yükleyin

Tam ikinci bir SAP SID2 sistemi yükleme işleminin bir açıklaması için bkz. [Windows Yük devretme kümesi ve bir SAP ASCS/SCS örneği için paylaşılan disk üzerinde SAP NetWeaver HA yükleme][sap-high-availability-installation-wsfc-shared-disk].

Üst düzey yordam aşağıdaki gibidir:

1. [Bir yüksek kullanılabilirlik ASCS/SCS örneği ile SAP yükleme][sap-high-availability-installation-wsfc-shared-disk-install-ascs].  
 Bu adımda, mevcut WSFC küme düğümü 1 üzerindeki bir yüksek kullanılabilirlik ASCS/SCS örneğiyle SAP yüklüyorsunuz.

2. [SAP ASCS/SCS örneği profilini değiştirmek][sap-high-availability-installation-wsfc-shared-disk-modify-ascs-profile].

3. [Bir araştırma bağlantı noktasını yapılandırma][sap-high-availability-installation-wsfc-shared-disk-add-probe-port].  
 Bu adımda, PowerShell kullanarak SAP küme kaynağı SAP SID2 IP araştırma bağlantı noktası yapılandırmış olursunuz. Bu yapılandırma, SAP ASCS/SCS küme düğümlerinden biri üzerinde çalıştırın.

4. Veritabanı örneğini yükleyin.  
 İkinci küme yüklemek için SAP Yükleme Kılavuzu'ndaki adımları izleyin.

5. İkinci küme düğümüne yükleyin.  
 Bu adımda, bir yüksek kullanılabilirlik ASCS/SCS örneği mevcut WSFC küme düğümünde 2 ile SAP yüklüyorsunuz. İkinci küme yüklemek için SAP Yükleme Kılavuzu'ndaki adımları izleyin.

6. SAP ASCS/SCS örneği ve araştırma bağlantı noktası için Windows Güvenlik Duvarı bağlantı noktalarını açın.  
    SAP ASCS/SCS örneği için kullanılan her iki küme düğümleri üzerinde SAP ASCS/SCS tarafından kullanılan tüm Windows Güvenlik Duvarı bağlantı noktaları açıyoruz. SAP ASCS/SCS örneği bu bağlantı noktaları bölümde listelenen [SAP ASCS / SCS bağlantı noktaları][sap-net-weaver-ports-ascs-scs-ports].

    Diğer tüm SAP bağlantı listesi için bkz. [TCP/IP bağlantı noktaları tüm SAP ürünleri][sap-net-weaver-ports].  

    Ayrıca senaryomuzdaki 62350 olan Azure iç yük dengeleyici araştırma bağlantı noktasını açın. Açıklanmıştır [bu makaledeki][sap-high-availability-installation-wsfc-shared-disk-win-firewall-probe-port].

7. [Değerlendirilen SAP giriş kapatma (Ağıranlar) Windows hizmet örneği başlangıç türünü değiştirme][sap-high-availability-installation-wsfc-shared-disk-change-ers-service-startup-type].

8. SAP birincil uygulama sunucusunda yeni adanmış VM'de SAP Yükleme Kılavuzu'nda açıklandığı gibi yükleyin.  

9. SAP ek uygulama sunucusu yeni adanmış VM'de SAP Yükleme Kılavuzu'nda açıklandığı gibi yükleyin.

10. [SIOS çoğaltma ve SAP ASCS/SCS örneği yük devretme testi][sap-high-availability-installation-wsfc-shared-disk-test-ascs-failover-and-sios-repl].

## <a name="next-steps"></a>Sonraki adımlar

- [Ağ Limitleri: Azure Resource Manager][networking-limits-azure-resource-manager]
- [Azure için birden çok Vıp'de yük dengeleyici][load-balancer-multivip-overview]
