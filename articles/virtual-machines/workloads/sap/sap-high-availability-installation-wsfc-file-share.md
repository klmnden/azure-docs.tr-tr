---
title: "SAP NetWeaver HA yüklemesinde Windows Yük devretme kümesi ve dosya paylaşımı için SAP (A) Azure SCS örneğinde | Microsoft Docs"
description: "Itanium tabanlı sistemler için SAP NetWeaver HA yükleme Windows Yük devretme kümesi ve dosya paylaşımı için SAP (A) SCS örneği"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 71296618-673b-4093-ab17-b7a80df6e9ac
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ec7888cfb9b0d288b5b25f580c852ee32306684c
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2017
---
# <a name="sap-netweaver-ha-installation-on-windows-failover-cluster-and-file-share-for-sap-ascs-instance-on-azure"></a>Itanium tabanlı sistemler için SAP NetWeaver HA yükleme Windows Yük devretme kümesi ve dosya paylaşımı için Azure SCS örneğinde SAP (A)

[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1596496]:https://launchpad.support.sap.com/#/notes/1596496

[sap-installation-guides]:http://service.sap.com/instguides

[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md

[s2d-in-win-2016]:https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview
[sofs-overview]:https://technet.microsoft.com/library/hh831349(v=ws.11).aspx
[new-in-win-2016-storage]:https://docs.microsoft.com/windows-server/storage/whats-new-in-storage

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md

[deployment-guide]:deployment-guide.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md

[sap-blog-new-sap-cluster-resource-dll]:https://blogs.sap.com/2017/08/28/new-sap-cluster-resource-dll-is-available/
[sap-high-availability-architecture-scenarios]:sap-high-availability-architecture-scenarios.md
[sap-high-availability-guide-wsfc-shared-disk]:sap-high-availability-guide-wsfc-shared-disk.md
[sap-high-availability-guide-wsfc-file-share]:sap-high-availability-guide-wsfc-file-share.md
[sap-ascs-high-availability-multi-sid-wsfc]:sap-ascs-high-availability-multi-sid-wsfc.md
[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-infrastructure-wsfc-file-share]:sap-high-availability-infrastructure-wsfc-file-share.md
[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md
[sap-hana-ha]:sap-hana-high-availability.md
[sap-suse-ascs-ha]:high-availability-guide-suse.md

[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md
[sap-high-availability-installation-wsfc-shared-disk-create-ascs-virt-host]:sap-high-availability-installation-wsfc-shared-disk.md#a97ad604-9094-44fe-a364-f89cb39bf097
[sap-high-availability-installation-wsfc-shared-disk-add-probe-port]:sap-high-availability-installation-wsfc-shared-disk.md#10822f4f-32e7-4871-b63a-9b86c76ce761

[planning-guide]:planning-guide.md
[planning-guide-11]:planning-guide.md
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10

[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md

[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-infrastructure-wsfc-shared-disk-vpn]:sap-high-availability-infrastructure-wsfc-shared-disk.md#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-high-availability-infrastructure-wsfc-shared-disk-change-def-ilb]:sap-high-availability-infrastructure-wsfc-shared-disk.md#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-high-availability-infrastructure-wsfc-shared-disk-setup-wsfc]:sap-high-availability-infrastructure-wsfc-shared-disk.md#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-high-availability-infrastructure-wsfc-shared-disk-collect-cluster-config]:sap-high-availability-infrastructure-wsfc-shared-disk.md#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-high-availability-infrastructure-wsfc-shared-disk-install-sios]:sap-high-availability-infrastructure-wsfc-shared-disk.md#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-high-availability-infrastructure-wsfc-shared-disk-add-dot-net]:sap-high-availability-infrastructure-wsfc-shared-disk.md#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-high-availability-infrastructure-wsfc-shared-disk-install-sios-both-nodes]:sap-high-availability-infrastructure-wsfc-shared-disk.md#dd41d5a2-8083-415b-9878-839652812102
[sap-high-availability-infrastructure-wsfc-shared-disk-setup-sios]:sap-high-availability-infrastructure-wsfc-shared-disk.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006

[sap-official-ha-file-share-document]:https://www.sap.com/documents/2017/07/f453332f-c97c-0010-82c7-eda71af511fa.html

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

Bu belge yüklemek ve Azure, yüksek kullanılabilir SAP sistemde yapılandırmak nasıl açıklayan **Windows Yük devretme kümesi (WSFC)** ve **ölçek genişletme dosya paylaşımı** SAP (A) SCS kümeleme için bir seçenek olarak örneği.

## <a name="prerequisites"></a>Ön koşullar

Yükleme işlemine başlamadan önce bu belgeleri gözden geçirdiğinizden emin olun:

* [Kümeleme Mimarisi Kılavuzu - SAP (A) SCS örneği üzerinde **Windows Yük devretme kümesi** kullanarak **dosya paylaşımı**][sap-high-availability-guide-wsfc-file-share]

* [SAP HA kullanmak için Azure altyapı hazırlık **Windows Yük devretme kümesi** ve **paylaşılan dosya** SAP (A) SCS örneği için][sap-high-availability-infrastructure-wsfc-file-share]


Yürütülebilir dosyalar aşağıdaki gereksinim / SAP DLL'lerden:
* SAP **yazılım sağlama Yöneticisi** (**SWPM**) yükleme Aracı sürüm **SPS21 (veya sonrası)**.
* Karşıdan **son NTCLUST. ÖİB** yeni SAP küme kaynağı dll dosyası arşive. Yeni SAP küme DLL'leri SAP (A) SCS yüksek kullanılabilirlik dosya paylaşımı ile Windows Server Yük devretme kümesinde destekler.

  Yeni SAP küme kaynağı DLL üzerinde daha fazla oluşturulması için bu Web Günlüğü denetleyin: [yeni SAP küme kaynağı DLL kullanılabilir!][sap-blog-new-sap-cluster-resource-dll]

Kurulumları DBMS sistemine bağlı olarak farklılık gösterdiğinden biz DBMS Kurulum tanımlamaz. Ancak, farklı DBMS satıcılar için Azure desteği işlevler ile DBMS ile yüksek kullanılabilirlik sorunlarının giderilmesini varsayalım. Örneğin, her zaman açık veya Oracle veritabanları için SQL Server ve Oracle Data Guard için veritabanı yansıtma. Bu makalede kullanırız senaryosunda, size daha fazla koruma DBMS ekleyemiyor.

Bu tür bir Azure kümelenmiş SAP ASCS/SCS yapılandırmasında farklı DBMS Hizmetleri etkileşim, özel durumlar vardır.

> [!NOTE]
> SAP NetWeaver ABAP sistemleri, Java sistemleri ve ABAP + Java sistemleri yükleme yordamları neredeyse aynıdır. En önemli fark, bir SAP ABAP sistemi bir ASCS örneği sahip olur. SAP Java sistem bir SCS örneği vardır. SAP ABAP + Java sistem bir ASCS örneği ve aynı Microsoft yük devretme küme grubunda çalışan bir SCS örneğine sahip. Her SAP NetWeaver yükleme yığınının yükleme farkları açıkça belirtilen. Diğer tüm bölümleri aynı olduğunu kabul edilebilir.  
>
>

## <a name="install-ascs-instance-on-ascs-cluster"></a>(A) SCS kümede (A) SCS örneğini yükleyin

> [!IMPORTANT]
>
>Şu anda bir dosya paylaşımı yapılandırması HA ayarıyla SAP yükleme aracı yazılım sağlama Yöneticisi (SWPM) tarafından desteklenmiyor. Bu nedenle, bazı el ile benimseme bir SAP sistemi örneğin yüklemek için yüklemek ve SAP (A) SCS örnek küme ve ayrı SAP GLOBALHOST yapılandırmak gereklidir.  
>
>Diğer yükleme adımlarını yükleme (ve küme) DBMS örneği ve SAP uygulama sunucuları için herhangi bir değişiklik yoktur.
>

### <a name="install-ascs-instance-on-local-drive"></a>(A) SCS örneği yerel sürücüsüne yükleyin

Yükleme SAP (A) SCS örnek **her ikisi de** (A) SCS küme düğümlerinin. Yüklemeniz **yerel** sürücü. Bizim örneğimizde, yerel sürücüdür C: olan seçtik\\. Herhangi bir yerel sürücüye seçebilirsiniz.  

Yüklemek için SAP yüklemede gezinmek için SWPM aracı:

&lt;Ürün&gt; -> &lt;DBMS&gt; yükleme -> Uygulama -> sunucu ABAP (veya Java) -> Dağıtılmış Sistem -> (A) SCS örneği

> [!IMPORTANT]
>Şu anda, dosya paylaşımı senaryo değil henüz SAP yükleme aracı SWPM tarafından desteklenen **kullanamazsınız** yükleme yolu:
>
>&lt;Ürün&gt; -> &lt;DBMS&gt; yükleme -> Uygulama -> sunucu ABAP (veya Java) yüksek kullanılabilirlik-> Sistem ->...
>

### <a name="remove-sapmnt-and-create-saploc-file-share"></a>SAPMNT kaldırın ve SAPLOC dosya paylaşımı oluşturma

C'de SWMP oluşturulan SAPMNT yerel paylaşıma\\usr\\sap klasör.

SAPMNT dosya paylaşımı kaldırmak **her ikisi de** (A) SCS küme düğümleri:

PowerShell betiğini yürütün:

```PowerShell
Remove-SmbShare sapmnt -ScopeName * -Force
 ```

SAPLOC paylaşımı yoksa, HEM ASCS küme düğümleri üzerinde bir tane oluşturun.

PowerShell betiğini yürütün:

```PowerShell
#Create SAPLOC share and set security
$SAPSID = "PR1"
$DomainName = "SAPCLUSTER"
$SAPSIDGlobalAdminGroupName = "$DomainName\SAP_" + $SAPSID + "_GlobalAdmin"
$HostName = $env:computername
$SAPLocalAdminGroupName = "$HostName\SAP_LocalAdmin"
$SAPDisk = "C:"
$SAPusrSapPath = "$SAPDisk\usr\sap"

New-SmbShare -Name saploc -Path c:\usr\sap -FullAccess "BUILTIN\Administrators", $SAPSIDGlobalAdminGroupName , $SAPLocalAdminGroupName  
 ```

## <a name="prepare-sap-global-host-on-sofs-cluster"></a>SAP genel SOFS küme KONAKTA hazırlama

Bu adımda, SOFS kümede aşağıdaki birim ve dosya paylaşımı oluştur:

* SAP GLOBALHOST dosya C:\ClusterStorage\Volume1\usr\sap\\&lt;SID&gt;\SYS\ yapısına SOFS Küme Paylaşılan birimi (CSV)

* SAPMNT dosya paylaşımı oluşturma

* Güvenlik SAPMNT dosya paylaşım ve klasör için tam denetim ayarlayın:
    * **&lt;Etki alanı&gt;\SAP_&lt;SID&gt;_GlobalAdmin** kullanıcı grubu
    * (A) SCS küme düğümleri bilgisayar SAP **nesneleri &lt;etki alanı&gt;\ClusterNode1$ ve &lt;etki alanı&gt;\ClusterNode2$**

Bölümde tanımlanan bir yansıtma dayanıklılığı ile CSV birim oluşturmak için **SOFS azure'da - Bağlantı Ekle SAP önkoşulları**, aşağıdaki SOFS küme düğümlerinden birinin PowerShell cmdlet'ini yürütün:


```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName SAPPR1 -FileSystem CSVFS_ReFS -Size 5GB -ResiliencySettingName Mirror
```
SAPMNT oluşturmak ve klasörü ve paylaşımı güvenlik ayarlamak için PowerShell Betiği SOFS küme düğümlerinden biri üzerinde aşağıdaki yürütün:

```PowerShell
# Create SAPMNT on file share
$SAPSID = "PR1"
$DomainName = "SAPCLUSTER"
$SAPSIDGlobalAdminGroupName = "$DomainName\SAP_" + $SAPSID + "_GlobalAdmin"

# SAP (A)SCS cluster nodes
$ASCSClusterNode1 = "ascs-1"
$ASCSClusterNode2 = "ascs-2"

# Define SAP (A)SCS cluster node computer objects
$ASCSClusterObjectNode1 = "$DomainName\$ASCSClusterNode1$"
$ASCSClusterObjectNode2 = "$DomainName\$ASCSClusterNode2$"

# Create usr\sap\.. folders on CSV
$SAPGlobalFolder = "C:\ClusterStorage\Volume1\usr\sap\$SAPSID\SYS"
New-Item -Path $SAPGlobalFOlder -ItemType Directory

$UsrSAPFolder = "C:\ClusterStorage\Volume1\usr\sap\"

# Create SAPMNT file share and set share security
New-SmbShare -Name sapmnt -Path $UsrSAPFolder -FullAccess "BUILTIN\Administrators", $SAPSIDGlobalAdminGroupName, $ASCSClusterObjectNode1, $ASCSClusterObjectNode2 -ContinuouslyAvailable $false -CachingMode None -Verbose

# Get SAPMNT file share security settings
Get-SmbShareAccess sapmnt

# Set files & folder security
$Acl = Get-Acl $UsrSAPFolder

# Add file security object of SAP_<sid>_GlobalAdmin group
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($SAPSIDGlobalAdminGroupName,"FullControl", 'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Add security object of clusternode1$ computer object
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($ASCSClusterObjectNode1,"FullControl",'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Add security object of clusternode2$ computer object
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($ASCSClusterObjectNode2,"FullControl",'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Set security
Set-Acl $UsrSAPFolder $Acl -Verbose
 ```
## <a name="stop-ascs-instances-and-sap-services"></a>Durdur (A) SCS örnekleri ve SAP Hizmetleri

Aşağıdaki adımları yürütün:
* SAP (A) SCS örnekleri hem (A) SCS küme düğümleri üzerinde Durdur
* SAP (A) SCS Windows hizmetlerini durdurmak **SAP&lt;SID&gt;_&lt;InstanceNumber&gt;**  her iki küme düğümlerinde

## <a name="move-sys-folder-to-sofs-cluster"></a>\SYS taşıma\.... SOFS küme klasöre

Aşağıdaki adımları yürütün:
* SYS Klasör Kopyala (örneğin C:\usr\sap\\&lt;SID&gt;\SYS) (A) SCS birinden küme düğümleri için SOFS kümeye örn C:\ClusterStorage\Volume1\usr\sap\\&lt;SID&gt;\SYS
* C:\usr\sap Sil\\&lt;SID&gt;(A) SCS küme düğümlerinin her ikisi de \SYS klasöründen

## <a name="update-cluster-security-setting-on-sap-ascs-cluster"></a>SAP (A) SCS kümede küme güvenlik ayarını güncelleştirin

Aşağıdaki PowerShell betiğini SAP (A) SCS küme düğümlerinden birinin yürütün:

```PowerShell
# Grant <DOMAIN>\SAP_<SID>_GlobalAdmin group access to cluster

$SAPSID = "PR1"
$DomainName = "SAPCLUSTER"
$SAPSIDGlobalAdminGroupName = "$DomainName\SAP_" + $SAPSID + "_GlobalAdmin"

# Set full access for <DOMAIn>\SAP_<SID>_GlobalAdmin group
Grant-ClusterAccess -User $SAPSIDGlobalAdminGroupName -Full

#Check security settings
Get-ClusterAccess
```

## <a name="create-a-virtual-host-name-for-the-clustered-sap-ascs-instance"></a>Kümelenmiş SAP (A) SCS örneği için bir sanal ana bilgisayar adı oluşturma

Bölümde açıklandığı gibi [kümelenmiş SAP ASCS/SCS örneği için bir sanal ana bilgisayar adı oluşturmak] [ sap-high-availability-installation-wsfc-shared-disk-create-ascs-virt-host] , örn. SAP (A) SCS küme ağ adı oluşturma **pr1 ascs [10.0.6.7]**

## <a name="update-default-and-sap-ascs-instance-profile"></a>Varsayılan güncelleştirin ve (A) SCS örneği profili SAP

Varsayılan güncelleştirmeniz gerekir ve SAP (A) SCS örneği profili &lt;SID&gt;_(A) SCS<Nr>_  <Host> kullanmak için:

* Yeni SAP (A) SCS sanal ana bilgisayar adı

* Yeni SAP genel ana bilgisayar adı


| Eski değerleri |  |
| --- | --- |
| (A) SCS hostname SAP SAP genel ana bilgisayar = | ascs-1 |
| (A) SCS örnek profil adı SAP | PR1_ASCS00_ascs-1 |

| Yeni değerler |  |
| --- | --- |
| (A) SCS hostname SAP | **pr1 ascs** |
| SAP genel ana bilgisayar | **sapglobal** |
| (A) SCS örnek profil adı SAP | PR1\_ASCS00\_**pr1 ascs** |

### <a name="update-sap-default-profile"></a>SAP varsayılan profilini güncelleştir


| Parametre Adı | Parametre değeri |
| --- | --- |
| SAPGLOBALHOST | **sapglobal** |
| rdisp/mshost | **pr1 ascs** |
| CLR'yi/serverhost | **pr1 ascs** |

### <a name="update-sap-ascs-instance-profile"></a>SAP (A) SCS örneği profilini güncelleştir

| Parametre Adı | Parametre değeri |
| --- | --- |
| SAPGLOBALHOST | **sapglobal** |
| DIR_PROFILE | \\\\**sapglobal**\sapmnt\PR1\SYS\profile |
| _PF | $(DIR_PROFILE) \PR1\_ASCS00_ pr1-ascs |
| Restart_Program_02 local$(_MS) pf=$(_PF) = | **Başlat**_Program_02 local$(_MS) pf=$(_PF) = |
| SAPLOCALHOST | **pr1 ascs** |
| Restart_Program_03 local$(_EN) pf=$(_PF) = | **Başlat**_Program_03 local$(_EN) pf=$(_PF) = |
| GW/netstat_once | **0** |
| encni/CLR'yi/set_so_keepalive  | **TRUE** |
| Hizmet/ha_check_node | **1** |

> [!IMPORTANT]
>Kullanabileceğiniz **güncelleştirme SAPASCSSCSProfile** profil güncelleştirme otomatikleştirmek için PowerShell cmdlet
>
>SAP ABAP ASCS ve SAP Java SCS örnek PowerShell cmdlet'ini destekler.
>

Kopya **SAPScripts.ps1** C:\tmp yerel sürücü ve PowerShell cmdlet'ini çalıştırın:

```PowerShell
Import-Module C:\tmp\SAPScripts.ps1

Update-SAPASCSSCSProfile -PathToAscsScsInstanceProfile \\sapglobal\sapmnt\PR1\SYS\profile\PR1_ASCS00_ascs-1 -NewASCSHostName pr1-ascs -NewSAPGlobalHostName sapglobal -Verbose  
```

![Şekil 1: SAPScripts.ps1 çıkış][sap-ha-guide-figure-8012]

_**Şekil 1:** SAPScripts.ps1 çıkış_

## <a name="update-ltsidgtadm-user-environment-variable"></a>Güncelleştirme &lt;SID&gt;adm kullanıcı ortam değişkeni

Güncelleştirme &lt;SID&gt;adm kullanıcı ortamı yeni GLOBALHOST UNC yolu HEM (A) SCS küme düğümlerinde.
Oturum açma &lt;SID&gt;adm kullanıcı ve başlangıç Regedit.exe aracı.
Git **HKEY_CURRENT_USER** -> **ortam** ve değişkenleri yeni değere güncelleştirin:

| Değişken | Değer |
| --- | --- |
| RSEC_SSFS_DATAPATH | \\\\**sapglobal**\sapmnt\PR1\SYS\global\security\rsecssfs\data |
| RSEC_SSFS_KEYPATH | \\\\**sapglobal**\sapmnt\PR1\SYS\global\security\rsecssfs\key |
| SAPEXE | \\\\**sapglobal**\sapmnt\PR1\SYS\exe\uc\NTAMD64 |
| SAPLOCALHOST  | **pr1 ascs** |


## <a name="install-new-saprcdll"></a>Yeni SAPRC yükleyin. DLL

Dosya Paylaşımı senaryoyu destekler SAP küme kaynağı yeni bir sürümünü yüklemeniz gerekir.

İndirme son **NTCLUST. ÖİB** paket SAP gelen hizmet markette.

NTCLUS ayıklayın. (A) SCS birinde ÖİB küme düğümleri ve komut yeni saprc.dll yüklemek için komut isteminden aşağıdaki çalıştırın:

```
.\NTCLUST\insaprct.exe -yes -install
```

Yeni saprc.dll hem (A) SCS küme düğümlerine yüklenir.

Daha fazla bilgi için bkz: [SAP Not 1596496 - SAP kaynak türü DLL'ler küme kaynağı İzleyicisi için güncelleştirme konusunda][1596496].

## <a name="create-sap-sid-cluster-group-network-name-and-ip"></a>SAP oluşturmak <SID> küme grubu, ağ adı ve IP

Oluşturmanız gerekir:

* SAP &lt;SID&gt; küme grubu
* < SCSNetworkName (a) >
* ve karşılık gelen IP adresi

PowerShell cmdlet'ini çalıştırın:

```PowerShell
# Create SAP Cluster Group
$SAPSID = "PR1"
$SAPClusterGroupName = "SAP $SAPSID"
$SAPIPClusterResourceName = "SAP $SAPSID IP"
$SAPASCSNetworkName = "pr1-ascs"
$SAPASCSIPAddress = "10.0.6.7"
$SAPASCSSubnetMask = "255.255.255.0"

# Create SAP ASCS instance Virtual IP cluster resource
Add-ClusterGroup -Name $SAPClusterGroupName -Verbose

#Create SAP ASCS Virtual IP Address
$SAPIPClusterResource = Add-ClusterResource -Name $SAPIPClusterResourceName -ResourceType "IP Address" -Group $SAPClusterGroupName -Verbose

# Set static IP Address
$param1 = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $SAPIPClusterResource,Address,$SAPASCSIPAddress
$param2 = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $SAPIPClusterResource,SubnetMask,$SAPASCSSubnetMask
$params = $param1,$param2
$params | Set-ClusterParameter

# Create corresponding network name
$SAPNetworkNameClusterResourceName = $SAPASCSNetworkName
Add-ClusterResource -Name $SAPNetworkNameClusterResourceName -ResourceType "Network Name" -Group $SAPClusterGroupName -Verbose

# Set Network DNS Name
$SAPNetworkNameClusterResource = Get-ClusterResource $SAPNetworkNameClusterResourceName
$SAPNetworkNameClusterResource | Set-ClusterParameter -Name Name -Value $SAPASCSNetworkName

#Check the updated values
$SAPNetworkNameClusterResource | Get-ClusterParameter

#Set resource dependencies
Set-ClusterResourceDependency -Resource $SAPNetworkNameClusterResourceName -Dependency "[$SAPIPClusterResourceName]" -Verbose

#Start SAP <SID> Cluster Group
Start-ClusterGroup -Name $SAPClusterGroupName -Verbose
```

## <a name="register-sap-start-service-on-both-nodes"></a>Her iki düğüm SAP başlangıç hizmet kaydı

Yeni bir profil ve profil yolu pint SAP (A) SCS sapstart hizmetine yeniden kaydetmeniz gerekir.

HIS HEM (A) SCS küme düğümlerinde yürütmeniz gerekir.

Çalışma form yükseltilmiş komut istemi aşağıdaki komutu:

```
C:\usr\sap\PR1\ASCS00\exe\sapstartsrv.exe -r -p \\sapglobal\sapmnt\PR1\SYS\profile\PR1_ASCS00_pr1-ascs -s PR1 -n 00 -U SAPCLUSTER\SAPServicePR1 -P mypasswd12 -e SAPCLUSTER\pr1adm
```

![Şekil 2: SAP hizmetini yeniden yükle][sap-ha-guide-figure-8013]

_**Şekil 2:** REINSTALL SAP hizmeti_

Parametreleri doğru olduğundan ve seçin emin yapar **el ile** başlangıç türü olarak.

## <a name="stop-ascs-service"></a>(A) SCS hizmetini durdurun

SAP (A) SCS hizmetini durdurun **SAP&lt;SID&gt;_ &lt;InstanceNumber&gt;**  (A) hem SCS üzerinde küme düğümleri.

## <a name="create-new-sap-service-and-sap-instance-resources"></a>Yeni SAP hizmet oluşturup SAP örnek kaynaklar

SAP SAP kaynakların oluşturulmasını Sonlandır artık&lt;SID&gt; küme grubu, örneğin kaynakları oluşturmanız gerekir:

* **SAP &lt;SID&gt; &lt;InstanceNumber&gt; hizmet** ve
* **SAP &lt;SID&gt; &lt;InstanceNumber&gt; örneği**

Aşağıdaki PowerShell cmdlet'ini çalıştırın:

```PowerShell
$SAPSID = "PR1"
$SAPInstanceNumber = "00"
$SAPNetworkNameClusterResourceName = "pr1-ascs"

$SAPServiceName = "SAP$SAPSID"+ "_" + $SAPInstanceNumber

$SAPClusterGroupName = "SAP $SAPSID"
$SAPServiceClusterResourceName = "SAP $SAPSID $SAPInstanceNumber Service"

$SAPASCSServiceClusterResource = Add-ClusterResource -Name $SAPServiceClusterResourceName -Group $SAPClusterGroupName -ResourceType "SAP Service" -SeparateMonitor -Verbose
$SAPASCSServiceClusterResource  | Set-ClusterParameter  -Name ServiceName -Value $SAPServiceName

#Set resource dependencies
Set-ClusterResourceDependency -Resource $SAPASCSServiceClusterResource  -Dependency "[$SAPNetworkNameClusterResourceName]" -Verbose

$SAPInstanceClusterResourceName = "SAP $SAPSID $SAPInstanceNumber Instance"

# Create SAP Instance cluster resource
$SAPASCSServiceClusterResource = Add-ClusterResource -Name $SAPInstanceClusterResourceName -Group $SAPClusterGroupName -ResourceType "SAP Resource" -SeparateMonitor -Verbose

#Set SAP Instance cluster resource parameters
$SAPASCSServiceClusterResource  | Set-ClusterParameter  -Name SAPSystemName -Value $SAPSID -Verbose
$SAPASCSServiceClusterResource  | Set-ClusterParameter  -Name SAPSystem -Value $SAPInstanceNumber -Verbose

#Set resource dependencies
Set-ClusterResourceDependency -Resource $SAPASCSServiceClusterResource  -Dependency "[$SAPServiceClusterResourceName]" -Verbose
```

## <a name="add-a-probe-port"></a>Bir araştırma bağlantı noktası ekleme

Bu adımda, PowerShell kullanarak bir SAP küme kaynağı SAP SID IP sonda bağlantı noktası yapılandırmış olursunuz. Açıklandığı gibi bu yapılandırmayı SAP ASCS/SCS küme düğümlerinden biri üzerinde yürütme [burada][sap-high-availability-installation-wsfc-shared-disk-add-probe-port].

## <a name="install-ers-instance-on-both-cluster-nodes"></a>Her iki küme düğümlerinde ERS örneğini yükleyin

Sonraki adımda, (A) her iki düğümde ERS (kuyruğa çoğaltma sunucusu) örneği yüklemelisiniz SCS küme.
Yükleme seçeneği SWPM menüde bulunabilir:

&lt;Ürün&gt; -> &lt;DBMS&gt; yükleme -> ek SAP örnekleri -> Sistem -> **kuyruğa çoğaltma sunucusu örneği**

## <a name="install-dbms-instance-and-sap-application-servers"></a>Yükleme DBMS örneği ve SAP uygulama sunucuları

SAP sistem yüklemenizi yükleyerek son şeklini verin:
* DBMS örneği
* Birincil SAP uygulama sunucusu
* Ek SAP uygulama sunucusu

## <a name="next-steps"></a>Sonraki Adımlar

* [Resmi SAP yönergeleri HA dosya paylaşımı için bir yük devretme kümesindeki hiçbir (A) SCS örneği yüklemesini Paylaşılan diskleri -][sap-official-ha-file-share-document]:

* [Depolama alanları doğrudan Windows Server 2016][s2d-in-win-2016]

* [Uygulama verileri genel bakışı için genişleme dosya sunucusu][sofs-overview]

* [Depolama Windows Server 2016 yenilikleri][new-in-win-2016-storage]
