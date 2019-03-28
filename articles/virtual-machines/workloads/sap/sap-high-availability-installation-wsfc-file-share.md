---
title: SAP NetWeaver yüksek kullanılabilirlik yüklemesi Azure üzerinde SAP ASCS/SCS örneği için bir Windows Yük devretme kümesi ve dosya paylaşım | Microsoft Docs
description: SAP NetWeaver SAP ASCS/SCS örneği için bir Windows Yük devretme kümesi ve dosya paylaşımı üzerinde yüksek kullanılabilirlik yüklemesi
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 71296618-673b-4093-ab17-b7a80df6e9ac
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 04490abb8b7f3f4c39e4134a314429e190db5174
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58540797"
---
# <a name="install-sap-netweaver-high-availability-on-a-windows-failover-cluster-and-file-share-for-sap-ascsscs-instances-on-azure"></a>Azure üzerinde SAP ASCS/SCS örneği için bir Windows Yük devretme kümesi ve dosya paylaşım SAP NetWeaver-yüksek kullanılabilirlik yükleyin

[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1596496]:https://launchpad.support.sap.com/#/notes/1596496

[sap-installation-guides]:http://service.sap.com/instguides

[sap-powershell-scrips]:https://github.com/Azure-Samples/sap-powershell

[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md

[s2d-in-win-2016]:https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview
[sofs-overview]:https://technet.microsoft.com/library/hh831349(v=ws.11).aspx
[new-in-win-2016-storage]:https://docs.microsoft.com/windows-server/storage/whats-new-in-storage

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md

[deployment-guide]:deployment-guide.md

[dr-guide-classic]:https://go.microsoft.com/fwlink/?LinkID=521971

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

Bu makalede, yükleme ve Windows Server Yük devretme kümesi (WSFC) ve genişleme dosya sunucusu kümesi SAP ASCS/SCS örneği için bir seçenek olarak ile azure'da yüksek kullanılabilirlik SAP sistemine yapılandırma açıklar.

## <a name="prerequisites"></a>Önkoşullar

Yükleme başlamadan önce aşağıdaki makaleleri inceleyin:

* [Mimari Kılavuzu: Dosya paylaşımı kullanarak bir Windows Yük devretme kümesinde Küme: SAP ASCS/SCS örneği][sap-high-availability-guide-wsfc-file-share]

* [Azure altyapı SAP yüksek kullanılabilirlik, SAP ASCS/SCS örneği için bir Windows Yük devretme kümesi ve dosya paylaşımı kullanarak hazırlama][sap-high-availability-infrastructure-wsfc-file-share]

Aşağıdaki yürütülebilir dosyaları ve SAP DLL'lerden ihtiyacınız vardır:
* SAP yazılım sağlama Yöneticisi (SWPM) yükleme aracı sürümü SPS21 veya üzeri.
* En son NTCLUST indirin. Yeni SAP ÖİB arşiviyle küme kaynağı DLL. Yeni SAP küme DLL'leri Windows Server Yük devretme kümesinde dosya paylaşımı ile SAP ASCS/SCS yüksek kullanılabilirliği destekler.

  Yeni SAP küme kaynağı dll dosyası hakkında daha fazla bilgi için bu Web günlüğüne bakın: [Yeni SAP küme kaynağı DLL kullanıma sunuldu! ][sap-blog-new-sap-cluster-resource-dll].

Ayarlar kullandığınız DBMS bağlı olarak farklılık gösterdiğinden biz veritabanı Yönetim Sistemi'nin (DBMS) Kurulum tanımlamaz. Ancak, çeşitli DBMS satıcıları desteklemek için Azure işlevleri ile DBMS ile yüksek kullanılabilirlik sorunları ele alınır varsayılır. Bu tür işlevleri AlwaysOn ya da SQL Server ve Oracle Data Guard'için Oracle veritabanları için veritabanı yansıtma içerir. Bu makalede kullandığımız senaryosunda, size daha fazla koruma için DBMS ekleyemiyor.

Bu tür bir kümelenmiş SAP ASCS/SCS yapılandırma azure'daki çeşitli DBMS Hizmetleri etkileşim oluşan özel durumlar vardır.

> [!NOTE]
> SAP NetWeaver ABAP sistemleri, Java sistemleri ve ABAP + Java sistemleri yükleme yordamları neredeyse aynıdır. En önemli fark, bir SAP ABAP sistemi bir ASCS örneğine sahip olur. SAP Java sistem bir SCS örneği vardır. ASCS örneği ve bir SCS örneği aynı Microsoft yük devretme küme grubu içinde çalışan SAP ABAP + Java sistemi vardır. Her SAP NetWeaver yükleme yığınının yükleme farkları açıkça belirtilmiştir. Diğer tüm bölümleri aynı olduğunu varsayabilirsiniz.  
>
>

## <a name="install-an-ascsscs-instance-on-an-ascsscs-cluster"></a>ASCS/SCS örneği bir ASCS/SCS kümede yüklemek

> [!IMPORTANT]
>
> Şu anda, bir yüksek kullanılabilirlik ayarı dosyası paylaşım yapılandırması ile SAP SWPM yükleme aracı tarafından desteklenmiyor. Bu nedenle, bazı el ile benimseme (örneğin, yükleme ve SAP ASCS/SCS örneği küme ve ayrı bir SAP genel ana yapılandırma) bir SAP sistemini yüklemek gereklidir.  
>
> DBMS yükleme (ve küme için) diğer yükleme adımları bir değişiklik yok örneği ve SAP uygulama sunucuları.
>

### <a name="install-an-ascsscs-instance-on-your-local-drive"></a>ASCS/SCS örneği yerel sürücünüzde yükleyin

SAP ASCS/SCS örneği için yükleyin *hem* ASCS/SCS kümesinin düğümleri. Yerel diske yükleyin. Bizim örneğimizde, yerel C: sürücüdür\\, ancak herhangi bir yerel sürücüye seçebilirsiniz.  

SAP SWPM yükleme aracı örneği yüklemek için şuraya gidin:

**\<Ürün >** > **\<DBMS >** > **yükleme** > **ABAP uygulama sunucusu** () veya **Java**) > **dağıtılmış sistemi** > **ASCS/SCS örneği**

> [!IMPORTANT]
> Şu anda, dosya paylaşımı senaryo SAP SWPM yükleme aracı tarafından desteklenmiyor. *Kullanamazsınız* aşağıdaki yükleme yolu:
>
> **\<Ürün >** > **\<DBMS >** > **yükleme** > **ABAP uygulama sunucusu** () veya **Java**) > **yüksek kullanılabilirlik sistem** >...
>

### <a name="remove-sapmnt-and-create-an-saploc-file-share"></a>SAPMNT kaldırın ve bir SAPLOC dosya paylaşımı oluşturma

C: SWMP oluşturulan SAPMNT Yerel paylaşım\\usr\\sap klasör.

SAPMNT dosya paylaşımını kaldırmak *hem* ASCS/SCS küme düğümleri.

Aşağıdaki PowerShell betiğini çalıştırın:

```powershell
Remove-SmbShare sapmnt -ScopeName * -Force
 ```

SAPLOC paylaşımı mevcut değilse oluşturun *hem* ASCS/SCS küme düğümleri.

Aşağıdaki PowerShell betiğini çalıştırın:

```powershell
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

## <a name="prepare-an-sap-global-host-on-the-sofs-cluster"></a>SOFS kümesine bir SAP genel konağında hazırlama

SOFS küme üzerinde aşağıdaki birim ve dosya paylaşımını oluşturun:

* SAP GLOBALHOST dosya `C:\ClusterStorage\Volume1\usr\sap\<SID>\SYS\` SOFS kümesi yapısına paylaşılan birimi (CSV)

* SAPMNT dosya paylaşımı

* SAPMNT dosya paylaşımı ve klasörü için tam denetim ile güvenlik ayarlayın:
    * \<Etki alanı > \SAP_\<SID > _GlobalAdmin kullanıcı grubu
    * SAP ASCS/SCS küme düğümü bilgisayar nesneleri \<etki alanı > \ClusterNode1$ ve \<etki alanı > \ClusterNode2$

Bir CSV birimi yansıtma dayanıklılığı ile oluşturmak için SOFS küme düğümlerinden biri üzerinde aşağıdaki PowerShell cmdlet'ini yürütün:


```powershell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName SAPPR1 -FileSystem CSVFS_ReFS -Size 5GB -ResiliencySettingName Mirror
```
SAPMNT oluşturup klasörü ve paylaşımı güvenlik için SOFS küme düğümlerinden biri üzerinde aşağıdaki PowerShell betiğini çalıştırın:

```powershell
# Create SAPMNT on file share
$SAPSID = "PR1"
$DomainName = "SAPCLUSTER"
$SAPSIDGlobalAdminGroupName = "$DomainName\SAP_" + $SAPSID + "_GlobalAdmin"

# SAP ASCS/SCS cluster nodes
$ASCSClusterNode1 = "ascs-1"
$ASCSClusterNode2 = "ascs-2"

# Define SAP ASCS/SCS cluster node computer objects
$ASCSClusterObjectNode1 = "$DomainName\$ASCSClusterNode1$"
$ASCSClusterObjectNode2 = "$DomainName\$ASCSClusterNode2$"

# Create usr\sap\.. folders on CSV
$SAPGlobalFolder = "C:\ClusterStorage\Volume1\usr\sap\$SAPSID\SYS"
New-Item -Path $SAPGlobalFOlder -ItemType Directory

$UsrSAPFolder = "C:\ClusterStorage\Volume1\usr\sap\"

# Create a SAPMNT file share and set share security
New-SmbShare -Name sapmnt -Path $UsrSAPFolder -FullAccess "BUILTIN\Administrators", $SAPSIDGlobalAdminGroupName, $ASCSClusterObjectNode1, $ASCSClusterObjectNode2 -ContinuouslyAvailable $false -CachingMode None -Verbose

# Get SAPMNT file share security settings
Get-SmbShareAccess sapmnt

# Set file and folder security
$Acl = Get-Acl $UsrSAPFolder

# Add a file security object of SAP_<sid>_GlobalAdmin group
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($SAPSIDGlobalAdminGroupName,"FullControl", 'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Add  a security object of the clusternode1$ computer object
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($ASCSClusterObjectNode1,"FullControl",'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Add a security object of the clusternode2$ computer object
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($ASCSClusterObjectNode2,"FullControl",'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Set security
Set-Acl $UsrSAPFolder $Acl -Verbose
 ```
## <a name="stop-ascsscs-instances-and-sap-services"></a>ASCS/SCS örnekleri ve SAP hizmetlerini Durdur

Aşağıdaki adımları uygulayın:
1. SAP ASCS/SCS örneği her iki ASCS/SCS küme düğümlerinde durdurun.
2. SAP ASCS/SCS Windows hizmetlerini durdurma **SAP\<SID > _\<Örneksayısı >** her iki küme düğümlerinde.

## <a name="move-the-sys-folder-to-the-sofs-cluster"></a>\SYS taşıma\... SOFS kümesine klasörü

Aşağıdaki adımları uygulayın:
1. SYS klasörünü kopyalayın (örneğin, `C:\usr\sap\<SID>\SYS`) SOFS kümesine ASCS/SCS birinden küme düğümleri (örneğin, `C:\ClusterStorage\Volume1\usr\sap\<SID>\SYS`).
2. Silme `C:\usr\sap\<SID>\SYS` ASCS/SCS küme düğümlerinden iki klasör.

## <a name="update-the-cluster-security-setting-on-the-sap-ascsscs-cluster"></a>SAP ASCS/SCS kümesinde Küme güvenlik ayarını güncelleştirme

SAP ASCS/SCS küme düğümlerinden biri üzerinde aşağıdaki PowerShell betiğini yürütün:

```powershell
# Grant <DOMAIN>\SAP_<SID>_GlobalAdmin group access to the cluster

$SAPSID = "PR1"
$DomainName = "SAPCLUSTER"
$SAPSIDGlobalAdminGroupName = "$DomainName\SAP_" + $SAPSID + "_GlobalAdmin"

# Set full access for the <DOMAIN>\SAP_<SID>_GlobalAdmin group
Grant-ClusterAccess -User $SAPSIDGlobalAdminGroupName -Full

#Check security settings
Get-ClusterAccess
```

## <a name="create-a-virtual-host-name-for-the-clustered-sap-ascsscs-instance"></a>Kümelenmiş SAP ASCS/SCS örneği için bir sanal ana bilgisayar adı oluşturun

Bir SAP ASCS/SCS küme ağ adı oluşturun (örneğin, **pr1-ascs [10.0.6.7]**) anlatılan şekilde [kümelenmiş SAP ASCS/SCS örneği için bir sanal ana bilgisayar adı oluşturma] [ sap-high-availability-installation-wsfc-shared-disk-create-ascs-virt-host] .

## <a name="update-the-default-and-sap-ascsscs-instance-profile"></a>Varsayılan ve SAP ASCS/SCS örneği profilini güncelleştir

Yeni SAP ASCS/SCS sanal ana bilgisayar adını kullanın ve SAP genel ana bilgisayar adı için varsayılan hem de SAP ASCS/SCS örneği profili güncelleştirme \<SID >_ASCS/SCS\<Nr >_\<konak >.


| Eski değer |  |
| --- | --- |
| SAP ASCS/SCS ana bilgisayar adı SAP genel host = | ascs-1 |
| SAP ASCS/SCS örneği profil adı | PR1_ASCS00_ascs-1 |

| Yeni değerleri |  |
| --- | --- |
| SAP ASCS/SCS ana bilgisayar adı | **pr1 ascs** |
| SAP genel ana bilgisayar | **sapglobal** |
| SAP ASCS/SCS örneği profil adı | PR1\_ASCS00\_**pr1 ascs** |

### <a name="update-sap-default-profile"></a>SAP varsayılan profili güncelleştir


| Parametre adı | Parametre değeri |
| --- | --- |
| SAPGLOBALHOST | **sapglobal** |
| rdisp/mshost | **pr1 ascs** |
| enque/serverhost | **pr1 ascs** |

### <a name="update-the-sap-ascsscs-instance-profile"></a>SAP ASCS/SCS örneği profilini güncelleştir

| Parametre adı | Parametre değeri |
| --- | --- |
| SAPGLOBALHOST | **sapglobal** |
| DIR_PROFILE | \\\sapglobal\sapmnt\PR1\SYS\profile |
| _PF | $(DIR_PROFILE) \PR1\_ASCS00_ pr1-ascs |
| Restart_Program_02 local$(_MS) pf=$(_PF) = | **Başlangıç**_Program_02 local$(_MS) pf=$(_PF) = |
| SAPLOCALHOST | **pr1 ascs** |
| Restart_Program_03 local$(_EN) pf=$(_PF) = | **Başlangıç**_Program_03 local$(_EN) pf=$(_PF) = |
| GW/netstat_once | **0** |
| enque/encni/set_so_keepalive  | **TRUE** |
| service/ha_check_node | **1** |

> [!IMPORTANT]
>Kullanabileceğiniz **güncelleştirme SAPASCSSCSProfile** profili güncelleştirme otomatikleştirmek için PowerShell cmdlet'i.
>
>PowerShell cmdlet örnekleri hem SAP ABAP ASCS hem de SAP Java SCS destekler.
>

Kopyalama [ **SAPScripts.psm1** ] [ sap-powershell-scrips] C:\tmp yerel sürücü ve aşağıdaki PowerShell cmdlet'ini çalıştırın:

```powershell
Import-Module C:\tmp\SAPScripts.psm1

Update-SAPASCSSCSProfile -PathToAscsScsInstanceProfile \\sapglobal\sapmnt\PR1\SYS\profile\PR1_ASCS00_ascs-1 -NewASCSHostName pr1-ascs -NewSAPGlobalHostName sapglobal -Verbose  
```

![Şekil 1: SAPScripts.psm1 çıkış][sap-ha-guide-figure-8012]

_**Şekil 1**: SAPScripts.psm1 çıkış_

## <a name="update-the-sidadm-user-environment-variable"></a>Güncelleştirme \<SID > adm kullanıcı ortam değişkeni

1. Güncelleştirme \<SID > adm kullanıcı ortamını yeni GLOBALHOST UNC yolu *hem* ASCS/SCS küme düğümleri.
2. Oturum açma \<SID > adm kullanıcı ve Regedit.exe Aracı'nı başlatın.
3. Git **HKEY_CURRENT_USER** > **ortam**ve ardından değişkenlerinin yeni değere güncelleştirin:

| Değişken | Değer |
| --- | --- |
| RSEC_SSFS_DATAPATH | \\\\**sapglobal**\sapmnt\PR1\SYS\global\security\rsecssfs\data |
| RSEC_SSFS_KEYPATH | \\\\**sapglobal**\sapmnt\PR1\SYS\global\security\rsecssfs\key |
| SAPEXE | \\\\**sapglobal**\sapmnt\PR1\SYS\exe\uc\NTAMD64 |
| SAPLOCALHOST  | **pr1 ascs** |


## <a name="install-a-new-saprcdll-file"></a>Yeni bir saprc.dll dosya yükleme

1. Dosya Paylaşımı senaryoyu destekler SAP küme kaynağı yeni bir sürümünü yükleyin.

2. En son NTCLUST indirin. SAP Service Marketplace ÖİB paketi.

3. NTCLUS ayıklayın. ASCS/SCS birinde ÖİB, küme düğümleri ve ardından yeni saprc.dll dosya yüklemek için komut isteminden aşağıdaki komutu çalıştırın:

```
.\NTCLUST\insaprct.exe -yes -install
```

Yeni saprc.dll dosyanın her iki ASCS/SCS küme düğümlerine yüklenir.

Daha fazla bilgi için [SAP notu 1596496 - SAP kaynak türü'nı DLL'ler Küme Kaynak İzleyicisi için güncelleştirme][1596496].

## <a name="create-a-sap-sid-cluster-group-network-name-and-ip"></a>SAP oluşturma \<SID > Küme grubu, ağ adı ve IP

SAP oluşturmak için \<SID > Küme grubu, bir ASCS/SCS ağ adı ve karşılık gelen bir IP adresi, aşağıdaki PowerShell cmdlet'ini çalıştırın:

```powershell
# Create SAP Cluster Group
$SAPSID = "PR1"
$SAPClusterGroupName = "SAP $SAPSID"
$SAPIPClusterResourceName = "SAP $SAPSID IP"
$SAPASCSNetworkName = "pr1-ascs"
$SAPASCSIPAddress = "10.0.6.7"
$SAPASCSSubnetMask = "255.255.255.0"

# Create an SAP ASCS instance virtual IP cluster resource
Add-ClusterGroup -Name $SAPClusterGroupName -Verbose

#Create an SAP ASCS virtual IP address
$SAPIPClusterResource = Add-ClusterResource -Name $SAPIPClusterResourceName -ResourceType "IP Address" -Group $SAPClusterGroupName -Verbose

# Set a static IP address
$param1 = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $SAPIPClusterResource,Address,$SAPASCSIPAddress
$param2 = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $SAPIPClusterResource,SubnetMask,$SAPASCSSubnetMask
$params = $param1,$param2
$params | Set-ClusterParameter

# Create a corresponding network name
$SAPNetworkNameClusterResourceName = $SAPASCSNetworkName
Add-ClusterResource -Name $SAPNetworkNameClusterResourceName -ResourceType "Network Name" -Group $SAPClusterGroupName -Verbose

# Set a network DNS name
$SAPNetworkNameClusterResource = Get-ClusterResource $SAPNetworkNameClusterResourceName
$SAPNetworkNameClusterResource | Set-ClusterParameter -Name Name -Value $SAPASCSNetworkName

#Check the updated values
$SAPNetworkNameClusterResource | Get-ClusterParameter

#Set resource dependencies
Set-ClusterResourceDependency -Resource $SAPNetworkNameClusterResourceName -Dependency "[$SAPIPClusterResourceName]" -Verbose

#Start an SAP <SID> cluster group
Start-ClusterGroup -Name $SAPClusterGroupName -Verbose
```

## <a name="register-the-sap-start-service-on-both-nodes"></a>Her iki düğüm üzerinde SAP başlangıç hizmeti kaydedin

SAP ASCS/SCS başlangıç hizmeti, yeni profili ve profil yolu işaret edecek şekilde yeniden kaydedin.

Bu yeniden kayıt yürütülmesi gereken *hem* ASCS/SCS küme düğümleri.

Yükseltilmiş komut isteminde aşağıdaki komutu çalıştırın:

```
C:\usr\sap\PR1\ASCS00\exe\sapstartsrv.exe -r -p \\sapglobal\sapmnt\PR1\SYS\profile\PR1_ASCS00_pr1-ascs -s PR1 -n 00 -U SAPCLUSTER\SAPServicePR1 -P mypasswd12 -e SAPCLUSTER\pr1adm
```

![Şekil 2: SAP hizmetini yeniden yükle][sap-ha-guide-figure-8013]

_**Şekil 2**: SAP hizmetini yeniden yükle_

Parametrelerin doğru olduğundan ve ardından emin **el ile** olarak **başlangıç türü**.

## <a name="stop-the-ascsscs-service"></a>ASCS/SCS Hizmeti Durdur

SAP ASCS/SCS SAP Durdur'u\<SID > _\<Örneksayısı > hem ASCS/SCS üzerinde küme düğümleri.

## <a name="create-a-new-sap-service-and-sap-instance-resources"></a>Yeni bir SAP hizmet ve SAP örneği kaynakları oluşturma

SAP SAP kaynakların oluşturma işlemini sonlandırmak için\<SID > Küme grubu, aşağıdaki kaynakları oluşturur:

* SAP \<SID > \<Örneksayısı > hizmeti
* SAP \<SID > \<Örneksayısı > örneği

Aşağıdaki PowerShell cmdlet'ini çalıştırın:

```powershell
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

# Create SAP instance cluster resource
$SAPASCSServiceClusterResource = Add-ClusterResource -Name $SAPInstanceClusterResourceName -Group $SAPClusterGroupName -ResourceType "SAP Resource" -SeparateMonitor -Verbose

#Set SAP instance cluster resource parameters
$SAPASCSServiceClusterResource  | Set-ClusterParameter  -Name SAPSystemName -Value $SAPSID -Verbose
$SAPASCSServiceClusterResource  | Set-ClusterParameter  -Name SAPSystem -Value $SAPInstanceNumber -Verbose

#Set resource dependencies
Set-ClusterResourceDependency -Resource $SAPASCSServiceClusterResource  -Dependency "[$SAPServiceClusterResourceName]" -Verbose
```

## <a name="add-a-probe-port"></a>Bir araştırma bağlantı noktası Ekle

PowerShell kullanarak bir SAP küme kaynağı SAP SID IP araştırma bağlantı noktasını yapılandırın. Açıklandığı gibi bu yapılandırma SAP ASCS/SCS küme düğümlerinden biri üzerinde yürütme [bu makaledeki][sap-high-availability-installation-wsfc-shared-disk-add-probe-port].

## <a name="install-an-ers-instance-on-both-cluster-nodes"></a>Her iki küme düğümlerine Ağıranlar örneğini yükleyin

Sıraya alma çoğaltma sunucusuna (Ağıranlar) örneğini yükleyin *hem* ASCS/SCS kümesinin düğümleri. Bu yükleme yolunda SWPM menüsünde izleyin:

**\<Ürün >** > **\<DBMS >** > **yükleme** > **ek SAP sistemine örnekleri**  >  **Kuyruğa çoğaltma sunucusu örneği**

## <a name="install-a-dbms-instance-and-sap-application-servers"></a>DBMS örneği ve SAP uygulama sunucuları yükleme

SAP sistemi yüklemenizin yükleyerek son haline getir:
* Bir DBMS örneği.
* Birincil bir SAP uygulama sunucusu.
* Ek bir SAP uygulama sunucusu.

## <a name="next-steps"></a>Sonraki adımlar

* [ASCS/SCS örneği yük devretme kümesinde paylaşılan diskleri - yüksek kullanılabilirlik dosya paylaşımı için resmi SAP yönergeleri ile yükleme][sap-official-ha-file-share-document]

* [Windows Server 2016 depolama alanları doğrudan][s2d-in-win-2016]

* [Uygulama verileri genel bakışı için genişleme dosya sunucusu][sofs-overview]

* [Windows Server 2016'da depolamadaki yenilikler][new-in-win-2016-storage]
