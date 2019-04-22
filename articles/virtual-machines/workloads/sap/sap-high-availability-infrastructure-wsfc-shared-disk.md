---
title: Azure altyapı SAP SAP ASCS/SCS Windows Yük devretme kümesi ve paylaşılan disk kullanarak HA hazırlanma | Microsoft Docs
description: Azure altyapı SAP yüksek kullanılabilirlik için bir SAP ASCS/SCS örneği için bir Windows Yük devretme kümesi ve paylaşılan disk kullanarak nasıl hazırlanacağını öğrenin.
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: ec976257-396b-42a0-8ea1-01c97f820fa6
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b729327187a52f36d50f8a754f5521527bb07ac6
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58848305"
---
# <a name="prepare-the-azure-infrastructure-for-sap-ha-by-using-a-windows-failover-cluster-and-shared-disk-for-sap-ascsscs"></a>Azure altyapı SAP yüksek kullanılabilirlik için bir Windows Yük devretme kümesi ve paylaşılan disk SAP ASCS/SCS kullanarak hazırlama

[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

[sap-installation-guides]:http://service.sap.com/instguides
[tuning-failover-cluster-network-thresholds]:https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/

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
[sap-ha-guide-9.1.1]:high-availability-guide.md#a97ad604-9094-44fe-a364-f89cb39bf097
[sap-hana-ha]:sap-hana-high-availability.md
[sap-suse-ascs-ha]:high-availability-guide-suse.md

[planning-guide]:planning-guide.md
[planning-guide-11]:planning-guide.md
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10

[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f


[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-infrastructure-wsfc-shared-disk-vpn]:sap-high-availability-infrastructure-wsfc-shared-disk.md#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-high-availability-infrastructure-wsfc-shared-disk-change-def-ilb]:sap-high-availability-infrastructure-wsfc-shared-disk.md#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-high-availability-infrastructure-wsfc-shared-disk-setup-wsfc]:sap-high-availability-infrastructure-wsfc-shared-disk.md#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-high-availability-infrastructure-wsfc-shared-disk-collect-cluster-config]:sap-high-availability-infrastructure-wsfc-shared-disk.md#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-high-availability-infrastructure-wsfc-shared-disk-install-sios]:sap-high-availability-infrastructure-wsfc-shared-disk.md#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-high-availability-infrastructure-wsfc-shared-disk-add-dot-net]:sap-high-availability-infrastructure-wsfc-shared-disk.md#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-high-availability-infrastructure-wsfc-shared-disk-install-sios-both-nodes]:sap-high-availability-infrastructure-wsfc-shared-disk.md#dd41d5a2-8083-415b-9878-839652812102
[sap-high-availability-infrastructure-wsfc-shared-disk-setup-sios]:sap-high-availability-infrastructure-wsfc-shared-disk.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006

[sap-ha-multi-sid-guide]:sap-high-availability-multi-sid.md (SAP çoklu SID yüksek kullanılabilirliği yapılandırma)

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

Bu makalede Azure altyapı yükleyip kullanarak bir Windows Yük devretme kümesinde yüksek kullanılabilirlik SAP sistemine yapılandırmaya yönelik hazırlanmak için uygulayacağınız adımlar bir *Küme Paylaşılan disk* kümeleme seçeneği olarak bir SAP ASCS örneği.

## <a name="prerequisites"></a>Önkoşullar

Yüklemeye başlamadan önce bu makaleyi gözden geçirin:

* [Mimari Kılavuzu: Bir küme paylaşılan disk kullanarak bir Windows Yük devretme kümesinde Küme SAP ASCS/SCS örneği][sap-high-availability-guide-wsfc-shared-disk]

## <a name="prepare-the-infrastructure-for-architectural-template-1"></a>Mimari şablon 1 için altyapıyı hazırlama
SAP için Azure Resource Manager şablonları, gerekli kaynakları dağıtımı kolaylaştırır.

Azure Resource Manager'daki üç katmanlı şablon aynı zamanda yüksek kullanılabilirlik senaryolarını destekler. Örneğin, iki küme mimari şablon 1 vardır. Her kümenin bir SAP tek SAP ASCS/SCS ve DBMS hata noktasıdır.

Bu makalede açıklanmaktadır örnek senaryosu için Azure Resource Manager şablonları edinebileceğiniz aşağıda verilmiştir:

* [Azure Market görüntüsü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
* [Azure yönetilen diskleri kullanarak azure Market görüntüsü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-md)  
* [Özel görüntü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)
* [Yönetilen diskleri kullanarak özel görüntü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-md)

Mimari şablon 1 için altyapıyı hazırlamak için:

- Azure portalında, **parametreleri** bölmesinde, **SYSTEMAVAILABILITY** kutusunda **HA**.

  ![Şekil 1: SAP yüksek kullanılabilirlik Azure Resource Manager parametreleri ayarlayın][sap-ha-guide-figure-3000]

_**Şekil 1:** SAP yüksek kullanılabilirlik Azure Resource Manager parametreleri ayarlayın_


  Şablonları oluşturun:

  * **Sanal makineler**:
    * SAP uygulama sunucusu sanal makineler: \<SAPSystemSID\>- di -\<numarası\>
    * ASCS/SCS sanal makineleri küme: \<SAPSystemSID\>- ascs-\<numarası\>
    * DBMS küme: \<SAPSystemSID\>- db-\<numarası\>

  * **Ağ kartları ilişkili IP adreslerine sahip tüm sanal makineler için**:
    * \<SAPSystemSID\>- NIC-di -\<numarası\>
    * \<SAPSystemSID\>-nic-ascs-\<Number\>
    * \<SAPSystemSID\>- NIC-db -\<numarası\>

  * **Azure depolama hesapları (yalnızca yönetilmeyen diskler)**:

  * **Kullanılabilirlik grupları** için:
    * SAP uygulama sunucusu sanal makineler: \<SAPSystemSID\>- avset dı
    * SAP ASCS/SCS sanal makineleri küme: \<SAPSystemSID\>- avset ascs
    * DBMS sanal makineleri küme: \<SAPSystemSID\>- avset db

  * **Azure iç yük dengeleyici**:
    * IP adresi ve ASCS/SCS örneği için tüm bağlantı \<SAPSystemSID\>- lb ascs
    * SQL Server DBMS ve IP adresi için tüm bağlantı \<SAPSystemSID\>- lb-db

  * **Ağ güvenlik grubu**: \<SAPSystemSID\>- nsg ascs 0  
    * Açık bir dış Uzak Masaüstü Protokolü (RDP) bağlantı noktasına sahip \<SAPSystemSID\>- ascs 0 sanal makine

> [!NOTE]
> Varsayılan olarak Azure iç yük dengeleyicileri ve ağ kartları tüm IP adresleri dinamiktir. Bunları, statik IP adreslerini değiştirin. Biz, makalenin sonraki bölümlerinde bunun nasıl yapılacağını açıklar.
>
>

## <a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a> Üretimde kullanmak için kurumsal ağ bağlantısı (şirket içi) ile sanal makineleri dağıtma
Üretim SAP sistemlerini için Azure sanal makineleri dağıtma [kurumsal ağ bağlantısı (şirket içi)] [ planning-guide-2.2] Azure VPN ağ geçidi veya Azure ExpressRoute kullanarak.

> [!NOTE]
> Azure sanal ağı örneğinizin kullanabilirsiniz. Sanal ağ ve alt ağ zaten oluşturduğunuz hazırlanmış ve.
>
>

1. Azure portalında, **parametreleri** bölmesinde, **NEWOREXISTINGSUBNET** kutusunda **mevcut**.
2. İçinde **SUBNETID** kutusunda, Azure sanal makinelerinizi dağıtmak planladığınız hazırlanan Azure ağ alt ağı Kimliğiniz tam dizesi ekleyin.
3. Tüm Azure ağ alt ağların bir listesini almak için bu PowerShell komutunu çalıştırın:

   ```powershell
   (Get-AzVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets
   ```

   **Kimliği** alan değeri için alt ağ kimliği gösterir
4. Tüm alt ağ kimliği değerlerin bir listesini almak için bu PowerShell komutunu çalıştırın:

   ```powershell
   (Get-AzVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets.Id
   ```

   Alt ağ Kimliğini şöyle görünür:

   ```
   /subscriptions/<subscription ID>/resourceGroups/<VPN name>/providers/Microsoft.Network/virtualNetworks/azureVnet/subnets/<subnet name>
   ```

## <a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a> Test ve tanıtım için yalnızca bulutta yer alan SAP örnek dağıtın
Yalnızca bulutta yer alan dağıtım modelinde, yüksek kullanılabilirlik SAP sistemine dağıtabilirsiniz. Bu tür bir dağıtım, öncelikle tanıtım ve test kullanım durumları için kullanışlıdır. Üretimi kullanım örnekleri için uygun değildir.

- Azure portalında, **parametreleri** bölmesinde, **NEWOREXISTINGSUBNET** kutusunda **yeni**. Bırakın **SUBNETID** boş.

  SAP Azure Resource Manager şablonu otomatik olarak Azure sanal ağını ve alt ağ oluşturur.

> [!NOTE]
> Ayrıca Active Directory ve DNS hizmet in aynı örneğinden Azure sanal ağ için en az bir özel sanal makine dağıtmak gerekir. Şablon, bu sanal makineler oluşturmaz.
>
>


## <a name="prepare-the-infrastructure-for-architectural-template-2"></a>Mimari şablon 2 için altyapıyı hazırlama

Bu Azure Resource Manager şablonu için SAP, SAP mimari şablon 2 için gerekli altyapı kaynaklarını dağıtımını basitleştirmeye yardımcı olması için kullanabilirsiniz.

Bu dağıtım senaryosu için Azure Resource Manager şablonları edinebileceğiniz aşağıda verilmiştir:

* [Azure Market görüntüsü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged)  
* [Yönetilen diskleri kullanarak azure Market görüntüsü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged-md)  
* [Özel görüntü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged)
* [Yönetilen diskleri kullanarak özel görüntü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged-md)


## <a name="prepare-the-infrastructure-for-architectural-template-3"></a>Mimari şablon 3 için altyapıyı hazırlama

Altyapıyı hazırlama ve SAP çoklu SID için yapılandırın. Örneğin, ek bir SAP ASCS/SCS örneğine ekleyebilirsiniz. bir *mevcut* küme yapılandırması. Daha fazla bilgi için [Azure Resource Manager'da bir SAP çoklu SID yapılandırmasını oluşturmak var olan bir küme yapılandırması için ek bir SAP ASCS/SCS örneği yapılandırma][sap-ha-multi-sid-guide].

Yeni bir çoklu SID küme oluşturmak istiyorsanız, çoklu SID kullanabileceğiniz [hızlı başlangıç şablonları GitHub üzerinde](https://github.com/Azure/azure-quickstart-templates).

Yeni bir çoklu SID kümesi oluşturmak için aşağıdaki üç şablonu dağıtmanız gerekir:

* [ASCS/SCS şablonu](#ASCS-SCS-template)
* [Veritabanı şablonu](#database-template)
* [Uygulama sunucuları şablonu](#application-servers-template)

Aşağıdaki bölümlerde şablonları ve şablonlar, sağlamanız gereken parametreler hakkında daha fazla ayrıntı sahip.

### <a name="ASCS-SCS-template"></a> ASCS/SCS şablonu

ASCS/SCS şablonu birden fazla ASCS/SCS örneği barındıran bir Windows Server Yük devretme kümesi oluşturmak için kullanabileceğiniz iki sanal makine dağıtır.

ASCS/SCS çoklu SID şablonu, ayarlamak için [ASCS/SCS çoklu SID şablon] [ sap-templates-3-tier-multisid-xscs-marketplace-image] veya [yönetilen diskleri kullanarak ASCS/SCS çoklu SID şablon] [ sap-templates-3-tier-multisid-xscs-marketplace-image-md], aşağıdaki parametreler için değerleri girin:

- **Kaynak önek**:  Dağıtım sırasında oluşturulan tüm kaynakları önek olarak eklemek için kullanılan kaynak öneki ayarlayın. Kaynakları tek bir SAP sistemine ait olmadığından kaynak öneki tek SAP sistemine SID'si değil.  Önek üç ve altı karakter arasında olmalıdır.
- **Yığın türü**: SAP sistemine yığın türünü seçin. Yığın türüne bağlı olarak, Azure Load Balancer (ABAP ya da yalnızca Java) veya iki (ABAP + Java) özel IP adresi başına SAP sistemine sahiptir.
- **İşletim sistemi türü**: Sanal makinelerin işletim sistemi seçin.
- **SAP sistemi sayısı**: Bu kümede yüklemek istediğiniz SAP sistemlerini sayısını seçin.
- **Sistem kullanılabilirliği**: Seçin **HA**.
- **Yönetici kullanıcı adı ve yönetici parolası**: Makinede oturum açmak için kullanılan yeni bir kullanıcı oluşturun.
- **Yeni veya var olan bir alt ağa**: Var olan bir alt ağ kullanın veya yeni bir sanal ağ ve alt ağı oluşturmak bu seçeneği ayarlayın. Şirket içi ağınıza bağlı bir sanal ağınız zaten varsa, seçin **mevcut**.
- **Alt ağ kimliği**: Tanımlanan bir alt ağa sahip olduğunuz mevcut bir Vnet'te VM dağıtmak istiyorsanız, VM atanmalıdır belirli bir alt ağ kimliği adı için. Kimliği genellikle şu şekilde görünür:

  /Subscriptions/\<abonelik kimliği\>/resourceGroups/\<kaynak grubu adı\>/providers/Microsoft.Network/virtualNetworks/\<sanal ağ adı\>/subnets/ \<alt ağ adı\>

Şablon birden çok SAP sistemlerini destekleyen bir Azure Load Balancer örneği dağıtır:

- ASCS örnekleri, örnek numarası 00, 10, 20 yapılandırılır...
- SCS örnekleri, örnek numarası, 01 11, 21 yapılandırılır...
- ASCS kuyruğa çoğaltma sunucusuna (Ağıranlar) (yalnızca Linux) örnekleri, örnek numarası 02, 12, 22 yapılandırılır...
- SCS Ağıranlar (yalnızca Linux) örnekleri, örnek numarası 03, 13, 23 yapılandırılır...

Yük Dengeleyici içeren 1 VIP(s) (Linux için 2), 1 x VIP ASCS/SCS için ve 1 x VIP için Ağıranlar (yalnızca Linux).

#### <a name="0f3ee255-b31e-4b8a-a95a-d9ed6200468b"></a> SAP ASCS/SCS bağlantı noktaları
Aşağıdaki liste, tüm Yük Dengeleme kuralları (burada x Örneğin, 1, 2, 3... SAP sistemine sayısıdır) içerir:
- Her SAP sistemi için Windows özel bağlantı noktaları: 445, 5985
- ASCS bağlantı noktaları (örnek numarasını x0): 32 x 0, 36 x 0, 39 x 0, 81 x 0, 5 x 013, 5 x 014, 5 x 016
- SCS bağlantı noktaları (örnek numarasını x1): 32 x 1, 33 x 1, 39 x 1, 81 x 1, 5 x 113, 5 x 114, 5 x 116
- (Örnek numarasını x2) Linux'ta ASCS Ağıranlar bağlantı noktaları: 33 x 2, 5 x 213, 5 x 214, 5 x 216
- (Örnek numarasını x3) Linux'ta SCS Ağıranlar bağlantı noktaları: 33 x 3, 5 x 313, 5 x 314, 5 x 316

Yük Dengeleyici (burada x Örneğin, 1, 2, 3... SAP sistemine sayısıdır) aşağıdaki araştırma bağlantı noktalarını kullanacak şekilde yapılandırılır:
- ASCS/SCS iç yük dengeleyici yoklama bağlantı noktası: 620x0
- İç Ağıranlar dengeleyici yoklama bağlantı noktası (yalnızca Linux) yükle: 621 x 2

### <a name="database-template"></a> Veritabanı şablonu

Veritabanı şablonu, bir veya iki tek SAP sistemine ilişkisel veritabanı yönetim sistemi (RDBMS) yüklemek için kullanabileceğiniz sanal makinelerin dağıtır. Örneğin, beş SAP sistemlerini bir ASCS/SCS şablonu dağıtırsanız, beş kez bu şablonu dağıtmak gerekir.

Veritabanı çoklu SID Şablonu ' ayarlamak için [veritabanı çoklu SID şablonu] [ sap-templates-3-tier-multisid-db-marketplace-image] veya [yönetilen diskleri kullanarak veritabanı çoklu SID şablonu] [ sap-templates-3-tier-multisid-db-marketplace-image-md], aşağıdaki parametreler için değerleri girin:

- **SAP sistem kimliği**: Yüklemek istediğiniz SAP sistemine SAP sistemi Kimliğini girin. Kimlik ön eki olarak dağıtılan kaynaklar için kullanılır.
- **İşletim sistemi türü**: Sanal makinelerin işletim sistemi seçin.
- **DbType**: Kümeye yüklemek istediğiniz veritabanı türünü seçin. Seçin **SQL** Microsoft SQL Server yüklemek istiyorsanız. Seçin **HANA** sanal makinelerde SAP HANA yüklemeyi planlıyorsanız. Doğru işletim sistemi türü seçtiğinizden emin olun. Seçin **Windows** SQL ve HANA için bir Linux dağıtımı seçin. Sanal makinelere bağlanan Azure Load Balancer, seçili veritabanı türü destekleyecek şekilde yapılandırılır:
  * **SQL**: Yük Dengeleyici Yük Dengeleme bağlantı noktası 1433'tür. SQL Server AlwaysOn ayarlarınızı bu bağlantı noktasını kullandığınızdan emin olun.
  * **HANA**: Yük Dengeleyici Yük Dengeleme ve bağlantı noktalarını 35015 35017. SAP HANA ile örnek numarasını yüklediğinizden emin olun **50**.
  Yük Dengeleyici yoklama bağlantı noktası 62550 kullanır.
- **SAP sistemi boyutu**: Yeni sisteme sağlar SAP sayısını ayarlayın. Kaç tane SAP sistemi gerektiriyor değil eminseniz, SAP teknoloji iş ortağı veya sistem Entegratörü isteyin.
- **Sistem kullanılabilirliği**: Seçin **HA**.
- **Yönetici kullanıcı adı ve yönetici parolası**: Makinede oturum açmak için kullanılan yeni bir kullanıcı oluşturun.
- **Alt ağ kimliği**: ASCS/SCS şablon dağıtımının bir parçası ASCS/SCS şablon dağıtımı sırasında kullanılan alt ağ kimliği veya oluşturulmuş alt ağ Kimliğini girin.

### <a name="application-servers-template"></a> Uygulama sunucuları şablonu

Uygulama sunucuları şablonu iki veya daha fazla SAP uygulama sunucusu örnekleri olarak bir SAP sistemi için kullanılabilir sanal makinelerin dağıtır. Örneğin, beş SAP sistemlerini bir ASCS/SCS şablonu dağıtırsanız, beş kez bu şablonu dağıtmak gerekir.

Uygulama sunucuları çoklu SID Şablonu ' ayarlamak için [uygulama sunucuları çoklu SID şablonu] [ sap-templates-3-tier-multisid-apps-marketplace-image] veya [yönetilen disklerkullanarakuygulamasunucularıçokluSIDşablonu] [ sap-templates-3-tier-multisid-apps-marketplace-image-md], aşağıdaki parametreler için değerleri girin:

  -  **SAP sistem kimliği**: Yüklemek istediğiniz SAP sistemine SAP sistemi Kimliğini girin. Kimlik ön eki olarak dağıtılan kaynaklar için kullanılır.
  -  **İşletim sistemi türü**: Sanal makinelerin işletim sistemi seçin.
  -  **SAP sistemi boyutu**: Yeni sisteme sağlar SAP sayısı. Kaç tane SAP sistemi gerektiriyor değil eminseniz, SAP teknoloji iş ortağı veya sistem Entegratörü isteyin.
  -  **Sistem kullanılabilirliği**: Seçin **HA**.
  -  **Yönetici kullanıcı adı ve yönetici parolası**: Makinede oturum açmak için kullanılan yeni bir kullanıcı oluşturun.
  -  **Alt ağ kimliği**: ASCS/SCS şablon dağıtımının bir parçası ASCS/SCS şablon dağıtımı sırasında kullanılan alt ağ kimliği veya oluşturulmuş alt ağ Kimliğini girin.


## <a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a> Azure sanal ağı
Bizim örneğimizde 10.0.0.0/16 adres alanı, Azure sanal ağı örneği var. Alt ağ, 10.0.0.0/24 adres aralığına bir alt yoktur. Tüm sanal makineler ve iç yük Dengeleyiciler bu sanal ağ içinde dağıtılmıştır.

> [!IMPORTANT]
> Ağ ayarları konuk işletim sistemi içinde herhangi bir değişiklik yok. Bu IP adresleri, DNS sunucuları ve alt ağ içerir. Azure'da tüm ağ ayarlarını yapılandırın. Dinamik konak Yapılandırma Protokolü (DHCP) hizmeti ayarlarınızı yayar.
>
>

## <a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a> DNS IP adresleri

Gerekli DNS IP adresleri için aşağıdaki adımları tamamlayın:

1. Azure portalında, **DNS sunucuları** bölmesinde emin olun, sanal ağınızın **DNS sunucuları** seçeneği **özel DNS**.
2. Sahip olduğunuz ağ türüne göre ayarlarınızı seçin. Daha fazla bilgi için aşağıdaki kaynaklara bakın:
   * [Kurumsal ağ bağlantısı (şirket içi)][planning-guide-2.2]: Şirket içi DNS sunucularının IP adreslerini ekleyin.  
   Azure'da çalışan sanal makineleri şirket içi DNS sunucularına genişletebilirsiniz. Bu senaryoda DNS hizmeti çalıştıran Azure sanal makinelerin IP adreslerini ekleyebilirsiniz.
   * Azure'da yalıtılmış VM dağıtımları için: Bir DNS sunucusu olarak hizmet veren aynı sanal ağ örneğinde ek bir sanal makine dağıtın. DNS hizmeti çalıştıran Azure sanal makinelerini yedeklemek için ayarladığınız IP adreslerini ekleyin.

   ![Şekil 2: Azure sanal ağı için DNS sunucuları yapılandırma][sap-ha-guide-figure-3001]

   _**Şekil 2:** Azure sanal ağı için DNS sunucuları yapılandırma_

   > [!NOTE]
   > DNS sunucularının IP adreslerini değiştirirseniz, değişikliği uygulamak ve yeni DNS sunucularını yaymak için bir Azure sanal makineleri yeniden başlatmanız gerekir.
   >
   >

Bizim örneğimizde, DNS hizmeti yüklenir ve bu Windows sanal makinelerinde yapılandırılır:

| Sanal makine rolü | Sanal makine konak adı | Ağ kartı adı | Statik IP adresi |
| --- | --- | --- | --- |
| İlk DNS sunucusu |domcontr-0 |pr1 NIC domcontr 0 |10.0.0.10 |
| İkinci bir DNS sunucusu |domcontr-1 |pr1 NIC domcontr 1 |10.0.0.11 |

## <a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a> Ana bilgisayar adları ve DBMS kümelenmiş örneği ve SAP ASCS/SCS kümelenmiş örneği için statik IP adresleri

Şirket içi dağıtımlarda, bu ayrılmış konak adları ve IP adresleri gerekir:

| Sanal ana bilgisayar adı rolü | Sanal ana bilgisayar adı | Sanal bir statik IP adresi |
| --- | --- | --- |
| SAP ASCS/SCS ilk küme sanal ana bilgisayar adı (için küme yönetimi) |pr1 ascs VIR |10.0.0.42 |
| SAP ASCS/SCS örneği sanal ana bilgisayar adı |pr1 ascs sap |10.0.0.43 |
| SAP DBMS ikinci küme sanal ana bilgisayar adı (küme yönetimi) |pr1-dbms-vir |10.0.0.32 |

Kümeyi oluşturduğunuzda, sanal ana bilgisayar adları pr1-ascs-VIR ve pr1 dbms VIR ve kümeyi yönetme ilişkili IP adreslerini oluşturun. Bunun nasıl yapılacağı hakkında daha fazla bilgi için bkz. [toplamak küme düğümleri bir küme yapılandırmasında][sap-high-availability-infrastructure-wsfc-shared-disk-collect-cluster-config].

Diğer iki sanal ana bilgisayar adları, pr1 ascs sap ve pr1-dbms-sap ve ilişkili IP adresleri, DNS sunucusunda el ile oluşturabilirsiniz. Bu kaynaklar kümelenmiş SAP ASCS/SCS örneği ile kümelenmiş DBMS örneği kullanın. Bunun nasıl yapılacağı hakkında daha fazla bilgi için bkz. [kümelenmiş bir SAP ASCS/SCS örneği için bir sanal ana bilgisayar adı oluşturma][sap-ha-guide-9.1.1].

## <a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a> SAP sanal makineleri için statik IP adresi ayarlama
Kümedeki sanal makinelerin dağıttıktan sonra tüm sanal makineler için statik IP adresleri kümesi gerekir. Bunu yapmak, Azure sanal ağ yapılandırması ve konuk işletim sistemi içinde değil.

1. Azure portalında **kaynak grubu** > **ağ kartı** > **ayarları** > **IPadresi**.
2. İçinde **IP adresleri** bölmesi altında **atama**seçin **statik**. İçinde **IP adresi** kutusunda, kullanmak istediğiniz IP adresini girin.

   > [!NOTE]
   > Ağ kartının IP adresini değiştirirseniz, değişikliği uygulamak için Azure sanal makineleri yeniden başlatmanız gerekir.  
   >
   >

   ![Şekil 3: Statik IP adresleri her sanal makinenin ağ kartı için ayarlayın][sap-ha-guide-figure-3002]

   _**Şekil 3:** Statik IP adresleri her sanal makinenin ağ kartı için ayarlayın_

   Tüm sanal makineler için Active Directory ve DNS hizmetiniz için kullanmak istediğiniz sanal makineleri dahil olan tüm ağ arabirimlerinin, bu adımı yineleyin.

Bizim örneğimizde, bu sanal makineler ve statik IP adresleri vardır:

| Sanal makine rolü | Sanal makine konak adı | Ağ kartı adı | Statik IP adresi |
| --- | --- | --- | --- |
| İlk SAP uygulama sunucusu örneği |pr1 di 0 |pr1 NIC di 0 |10.0.0.50 |
| İkinci SAP uygulama sunucusu örneği |pr1 di 1 |pr1 NIC di 1 |10.0.0.51 |
| ... |... |... |... |
| Son SAP uygulama sunucusu örneği |pr1 di 5 |pr1-nic-di-5 |10.0.0.55 |
| İlk küme düğümüne ASCS/SCS örneği için |pr1 ascs 0 |pr1 NIC ascs 0 |10.0.0.40 |
| ASCS/SCS örneği için ikinci küme düğümü |pr1 ascs 1 |pr1 NIC ascs 1 |10.0.0.41 |
| İlk küme düğümüne DBMS örneği için |pr1-db-0 |pr1-NIC-db-0 |10.0.0.30 |
| DBMS örneği için ikinci küme düğümü |pr1-db-1 |pr1-NIC-db-1 |10.0.0.31 |

## <a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a> Azure iç yük dengeleyici için statik bir IP adresi ayarlama

SAP Azure Resource Manager şablonu SAP ASCS/SCS örneği küme ve DBMS küme için kullanılan bir Azure iç yük dengeleyici oluşturur.

> [!IMPORTANT]
> SAP ASCS/SCS sanal ana bilgisayar adı, IP adresi SAP ASCS/SCS iç yük dengeleyici IP adresi ile aynıdır: pr1 lb ascs.
> DBMS adı sanal IP adresi DBMS iç yük dengeleyici IP adresi ile aynıdır: pr1 lb dbms.
>
>

Azure iç yük dengeleyici için statik bir IP adresi ayarlamak için:

1. İç yük dengeleyici IP adresi ilk dağıtım ayarlar **dinamik**. Azure portalında, üzerinde **IP adresleri** bölmesi altında **atama**seçin **statik**.
2. İç yük dengeleyicinin IP adresi ayarlama **pr1 lb ascs** SAP ASCS/SCS örneği sanal ana bilgisayar adını IP adresine.
3. İç yük dengeleyicinin IP adresi ayarlama **pr1 lb dbms** DBMS örneğinin sanal ana bilgisayar adını IP adresine.

   ![Şekil 4: SAP ASCS/SCS örneği için iç yük dengeleyici için statik IP adresi ayarlama][sap-ha-guide-figure-3003]

   _**Şekil 4:** SAP ASCS/SCS örneği için iç yük dengeleyici için statik IP adresi ayarlama_

Bizim örneğimizde, bu statik IP adresine sahip iki Azure iç yük Dengeleyiciler sunuyoruz:

| Azure iç yük dengeleyici rolü | Azure iç yük dengeleyici adı | Statik IP adresi |
| --- | --- | --- |
| SAP ASCS/SCS iç yük dengeleyici örneği |pr1 lb ascs |10.0.0.43 |
| SAP DBMS iç yük dengeleyici |pr1 lb dbms |10.0.0.33 |


## <a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a> ASCS/SCS Yük Dengeleme kuralları Azure iç yük dengeleyici için varsayılan

SAP Azure Resource Manager şablonu ihtiyacınız bağlantı noktaları oluşturur:
* Varsayılan örnek sayısı 00 ile ABAP ASCS örneği
* Varsayılan örnek sayısı 01 ile bir Java SCS örneği

SAP ASCS/SCS örneğinizin yüklediğinizde, varsayılan örnek sayısı 00 ABAP ASCS örneğinizi ve varsayılan örnek sayısı 01 için Java SCS Örneğiniz için kullanmanız gerekir.

Ardından, uç noktaları SAP NetWeaver bağlantı noktaları için gerekli iç yük dengelemeyi oluşturun.

Gerekli iç Yük Dengeleme uç noktaları oluşturmak için ilk olarak, bu Yük Dengeleme uç SAP NetWeaver ABAP ASCS bağlantı noktaları oluşturun:

| Hizmet/Yük Dengeleme kuralı adı | Varsayılan bağlantı noktası numaraları | Somut için bağlantı noktalarını (ASCS örneği ile örnek numarasını 00) (Ağıranlar 10 ile) |
| --- | --- | --- |
| Sıraya alma sunucu / *lbrule3200* |32\<Örneksayısı\> |3200 |
| ABAP ileti sunucusu / *lbrule3600* |36\<Örneksayısı\> |3600 |
| İç ABAP ileti / *lbrule3900* |39\<Örneksayısı\> |3900 |
| HTTP ileti sunucusu / *Lbrule8100* |81\<Örneksayısı\> |8100 |
| SAP ASCS HTTP hizmeti Başlat / *Lbrule50013* |5\<Örneksayısı\>13 |50013 |
| SAP ASCS HTTPS Hizmeti Başlat / *Lbrule50014* |5\<Örneksayısı\>14 |50014 |
| Sıraya alma çoğaltma / *Lbrule50016* |5\<Örneksayısı\>16 |50016 |
| SAP başlangıç hizmeti Ağıranlar HTTP *Lbrule51013* |5\<Örneksayısı\>13 |51013 |
| SAP başlangıç hizmeti Ağıranlar HTTP *Lbrule51014* |5\<Örneksayısı\>14 |51014 |
| Windows Uzaktan Yönetim (WinRM) *Lbrule5985* | |5985 |
| Dosya Paylaşımı *Lbrule445* | |445 |

**Tablo 1:** Bağlantı noktası numaralarını SAP NetWeaver ABAP ASCS örnekleri

Ardından, bu Yük Dengeleme uç SAP NetWeaver Java SCS bağlantı noktaları oluşturun:

| Hizmet/Yük Dengeleme kuralı adı | Varsayılan bağlantı noktası numaraları | Somut için bağlantı noktalarını (SCS örneği ile örnek numarasını 01) (Ağıranlar 11) |
| --- | --- | --- |
| Sıraya alma sunucu / *lbrule3201* |32\<Örneksayısı\> |3201 |
| Ağ Geçidi sunucusu / *lbrule3301* |33\<Örneksayısı\> |3301 |
| Java ileti sunucusu / *lbrule3900* |39\<Örneksayısı\> |3901 |
| HTTP ileti sunucusu / *Lbrule8101* |81\<Örneksayısı\> |8101 |
| SAP SCS HTTP hizmeti Başlat / *Lbrule50113* |5\<Örneksayısı\>13 |50113 |
| SAP SCS HTTPS Hizmeti Başlat / *Lbrule50114* |5\<Örneksayısı\>14 |50114 |
| Sıraya alma çoğaltma / *Lbrule50116* |5\<Örneksayısı\>16 |50116 |
| SAP başlangıç hizmeti Ağıranlar HTTP *Lbrule51113* |5\<Örneksayısı\>13 |51113 |
| SAP başlangıç hizmeti Ağıranlar HTTP *Lbrule51114* |5\<Örneksayısı\>14 |51114 |
| WinRM *Lbrule5985* | |5985 |
| Dosya Paylaşımı *Lbrule445* | |445 |

**Tablo 2:** Bağlantı noktası numaralarını SAP NetWeaver Java SCS örnekleri

![Şekil 5: ASCS/SCS Yük Dengeleme kuralları Azure iç yük dengeleyici için varsayılan][sap-ha-guide-figure-3004]

_**Şekil 5:** ASCS/SCS Yük Dengeleme kuralları Azure iç yük dengeleyici için varsayılan_

IP adresini yük dengeleyici pr1-lb-dbms DBMS örneğinin sanal ana bilgisayar adını IP adresine ayarlayın.

### <a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a> ASCS/SCS varsayılan Yük Dengeleme kuralları Azure iç yük dengeleyici için değiştirin

SAP ASCS veya SCS örneği için farklı bir sayı kullanmak istiyorsanız, varsayılan değerleri adları ve bağlantı noktalarının değerlerini değiştirmeniz gerekir.

1. Azure portalında  **\<SID\>-lb ascs yük dengeleyici** > **Yük Dengeleme kuralları**.
2. Tüm Yük Dengeleme SAP ASCS veya SCS örneğine ait kuralları için bu değerleri değiştirin:

   * Ad
   * Bağlantı noktası
   * Arka uç bağlantı noktası

   Örneğin, varsayılan ASCS örnek numarasını 00-31 olarak değiştirmek isterseniz, Tablo 1'de listelenen tüm bağlantı noktaları için değişiklikler yapmanız gerekir.

   İşte bir örnek bağlantı noktası için bir güncelleştirme *lbrule3200*.

   ![Şekil 6: ASCS/SCS varsayılan Yük Dengeleme kuralları Azure iç yük dengeleyici için değiştirin][sap-ha-guide-figure-3005]

   _**Şekil 6:** ASCS/SCS varsayılan Yük Dengeleme kuralları Azure iç yük dengeleyici için değiştirin_

## <a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a> Windows sanal makineleri etki alanına ekleyin

Sanal makinelere statik IP adresi atadıktan sonra sanal makinelerin etki alanına ekleyin.

![Şekil 7: Bir sanal makine bir etki alanına ekleme][sap-ha-guide-figure-3006]

_**Şekil 7:** Bir sanal makine bir etki alanına ekleme_

## <a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a> SAP ASCS/SCS örneği her iki küme düğümlerinde kayıt defteri girdilerini ekleyin

Azure Load Balancer iç yük dengeleyici bağlantıları için belirlenen bir süre boyunca boşta olduğunda kapanır bağlantılar (boşta zaman aşımı) süresi vardır. SAP iş işlemlerinde SAP sıraya iletişim örnekleri açık bağlantıları ilk sıraya alma/sıradan çıkarma gönderilmesi gerekiyor isteği olarak işleyin. Bu bağlantılar genellikle kadar iş sürecinin yerleşik kalır veya sıraya alma işlemi yeniden başlatır. Ancak, bağlantı belirli bir süre için boşta kalırsa Azure iç yük dengeleyici bağlantıları kapatır. SAP iş işlemi, artık mevcut değilse, sıraya alma işlemi için bağlantıyı yeniden kurar çünkü bu bir sorun değildir. Bu etkinlikler SAP işlemleri Geliştirici izlemelerinde belirtilmiştir, ancak izlemeleri çok miktarda ek içeriği oluştururlar. TCP/IP'yi değiştirmek için iyi bir fikirdir `KeepAliveTime` ve `KeepAliveInterval` her iki küme düğümlerinde. Bu makalenin sonraki bölümlerinde açıklanan SAP profili parametrelerinden TCP/IP'yi parametrelerle değişiklikleri birleştirin.

SAP ASCS/SCS örneği her iki küme düğümlerinde kayıt defteri girdileri eklemek için ilk olarak, bu Windows kayıt defteri girişleri hem Windows Küme düğümlerinde SAP ASCS/SCS ekleyin:

| Yol | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Değişken adı |`KeepAliveTime` |
| Değişken türü |REG_DWORD (ondalık) |
| Değer |120000 |
| Belgelerinin bağlantısı |[https://technet.microsoft.com/library/cc957549.aspx](https://technet.microsoft.com/library/cc957549.aspx) |

**Tablo 3:** İlk TCP/IP'yi parametre değiştirme

Ardından, bu Windows kayıt defteri girdisi SAP ASCS/SCS için hem Windows Küme düğümlerinde ekleyin:

| Yol | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Değişken adı |`KeepAliveInterval` |
| Değişken türü |REG_DWORD (ondalık) |
| Değer |120000 |
| Belgelerinin bağlantısı |[https://technet.microsoft.com/library/cc957548.aspx](https://technet.microsoft.com/library/cc957548.aspx) |

**Tablo 4:** İkinci TCP/IP'yi parametre değiştirme

Değişiklikleri uygulamak için her iki küme düğümü yeniden başlatın.

## <a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a> SAP ASCS/SCS örneği için Windows Server Yük devretme kümesi oluşturma

SAP ASCS/SCS örneği için bir Windows Server Yük devretme kümesi ayarlama, bu görevleri kapsar:

- Küme düğümleri bir küme yapılandırmasında toplayın.
- Küme dosya paylaşım tanığı yapılandırın.

### <a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a> Küme düğümleri bir küme yapılandırmasında Topla

1. Rol Ekle ve Özellikler Sihirbazı'nda Yük Devretme Kümelemesi her iki küme düğümlerine ekleyin.
2. Yük devretme kümesi, yük devretme kümesi Yöneticisi'ni kullanarak ayarlayın. Yük Devretme Kümesi Yöneticisi'nde **küme oluşturma**ve ardından yalnızca ilk kümesi (a düğümü) adını ekleyin. İkinci düğümü henüz eklemeyin; İkinci düğümü daha sonraki bir adımda eklediğiniz.

   ![Şekil 8: İlk küme düğümüne sunucu veya sanal makine adını ekleyin][sap-ha-guide-figure-3007]

   _**Şekil 8:** İlk küme düğümüne sunucu veya sanal makine adını ekleyin_

3. Küme ağ adı (sanal ana bilgisayar adı) girin.

   ![Şekil 9: Küme adı girin][sap-ha-guide-figure-3008]

   _**Şekil 9:** Küme adı girin_

4. Kümeyi oluşturduktan sonra küme doğrulama testi çalıştırın.

   ![Şekil 10: Küme doğrulama denetimini Çalıştır][sap-ha-guide-figure-3009]

   _**Şekil 10:** Küme doğrulama denetimini Çalıştır_

   Bu noktada işleminde disklerle ilgili tüm uyarılar yoksayabilirsiniz. Dosya paylaşım tanığı ve SIOS diskleri daha sonra paylaşılan ekleyeceksiniz. Bu aşamada, bir çekirdek sahip hakkında endişelenmeniz gerekmez.

   ![Şekil 11: Hiçbir çekirdek diski bulundu][sap-ha-guide-figure-3010]

   _**Şekil 11:** Hiçbir çekirdek diski bulundu_

   ![Şekil 12: Küme Çekirdek kaynağı yeni bir IP adresi gerekiyor][sap-ha-guide-figure-3011]

   _**Şekil 12:** Küme Çekirdek kaynağı yeni bir IP adresi gerekiyor_

5. Çekirdeği Küme hizmetinin IP adresini değiştirin. Çekirdeği Küme hizmetinin IP adresini değiştirmek kadar küme sanal makine düğümlerinin sunucusunun IP adresini işaret başlatılamıyor. Bunu yapmak **özellikleri** çekirdek küme hizmetin IP kaynak sayfası.

   Örneğin, bir IP adresi (örneğimizde, 10.0.0.42) atamak küme sanal ana bilgisayar adı pr1-ascs-VIR için ihtiyacımız var.

   ![Şekil 13: Özellikler iletişim kutusunda, IP adresini değiştirme][sap-ha-guide-figure-3012]

   _**Şekil 13:** İçinde **özellikleri** iletişim kutusunda, IP adresini değiştirme_

   ![Şekil 14: Küme için ayrılmış IP adresi atama][sap-ha-guide-figure-3013]

   _**Şekil 14:** Küme için ayrılmış IP adresi atama_

6. Küme sanal ana bilgisayar adı çevrimiçi duruma getirin.

   ![Şekil 15: Küme Çekirdek çalışmaya, doğru IP adresiyle hizmetidir][sap-ha-guide-figure-3014]

   _**Şekil 15:** Küme Çekirdek çalışmaya, doğru IP adresiyle hizmetidir_

7. İkinci küme düğümüne ekleyin.

   Çekirdeği Küme hizmeti çalışır duruma geldikten sonra ikinci küme düğümüne ekleyebilirsiniz.

   ![İkinci küme düğümüne Şekil 16 Ekle][sap-ha-guide-figure-3015]

   _**Şekil 16:** İkinci küme düğümü Ekle_

8. İkinci küme düğümünde konak için bir ad girin.

   ![Şekil 17: İkinci küme düğümü ana bilgisayar adı girin][sap-ha-guide-figure-3016]

   _**Şekil 17:** İkinci küme düğümü ana bilgisayar adı girin_

   > [!IMPORTANT]
   > Olduğundan emin olun **tüm uygun Depolama alanlarını kümeye ekleyin** onay kutusu *değil* seçili.  
   >
   >

   ![Şekil 18: Onay kutusunu seçin][sap-ha-guide-figure-3017]

   _**Şekil 18:** Yapmak *değil* onay kutusunu işaretleyin_

   Çekirdek ve diskler hakkında uyarıları gözardı edebilirsiniz. Çekirdek ayarlayın ve diski daha sonra açıklandığı paylaş [bir SAP ASCS/SCS Küme Paylaşımı diski için SIOS DataKeeper Cluster Edition][sap-high-availability-infrastructure-wsfc-shared-disk-install-sios].

   ![Şekil 19: Disk çekirdek hakkında uyarılar yoksay][sap-ha-guide-figure-3018]

   _**Şekil 19:** Disk çekirdek hakkında uyarılar yoksay_


#### <a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a> Küme dosya paylaşımı tanığını yapılandırma

Bir küme dosya paylaşım tanığı yapılandırma, bu görevleri kapsar:

- Bir dosya paylaşımı oluşturun.
- Dosya paylaşımı tanığı çekirdek yük devretme kümesi Yöneticisi'nde ayarlayın.

#### <a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a> Dosya paylaşımı oluşturma

1. Dosya paylaşım tanığı yerine bir çekirdek disk seçin. Bu seçenek, SIOS DataKeeper destekler.

   Bu makaledeki örneklerde, Azure'da çalışan Active Directory veya DNS sunucusu dosya paylaşımı tanığı açıktır. Dosya paylaşım tanığı domcontr 0 olarak adlandırılır. Active Directory ve DNS hizmetinizin (aracılığıyla veya Azure ExpressRoute VPN ağ geçidi) Azure VPN bağlantısı yapılandırmış olmanız, çünkü şirket içi ve dosya paylaşım tanığı çalıştırmak uygun değildir.

   > [!NOTE]
   > Yalnızca şirket içi Active Directory ve DNS hizmetiniz çalıştırıyorsa, Active Directory veya DNS Windows işletim şirket içinde çalışan sisteminde, dosya paylaşım tanığı yapılandırmayın. Azure ve Active Directory veya DNS şirket içi çalışan küme düğümleri arasındaki ağ gecikme süresi, çok büyük ve bağlantı sorunlarına neden olabilir. Küme düğümü yakın çalıştıran Azure sanal makinesinde dosya paylaşım tanığı yapılandırma emin olun.  
   >
   >

   Çekirdek sürücüde en az 1024 MB boş alan gerekir. 2.048 MB boş alan çekirdek sürücüsü için öneririz.

2. Küme adı nesnesi ekleyin.

   ![Şekil 20: Küme adı nesnesi için paylaşım izinleri atama][sap-ha-guide-figure-3019]

   _**Şekil 20:** Küme adı nesnesi için paylaşım izinleri atama_

   İzinleri, veri paylaşımı için küme adı nesnesi (Bizim örneğimizde, pr1 ascs VIR$) değiştirileceğini yetkilisi eklediğinizden emin olun.

3. Küme adı nesnesi listeye eklemek için seçin **Ekle**. Bilgisayar nesnelerini Şekil 22'de gösterilen ek olarak denetlemek için filtreyi değiştirin.

   ![Şekil 21: Bilgisayarları eklemek için değişiklik nesne türleri][sap-ha-guide-figure-3020]

   _**Şekil 21:** Değişiklik **nesne türlerini** bilgisayarları dahil etme_

   ![Şekil 22: Bilgisayarlar onay kutusunu seçin][sap-ha-guide-figure-3021]

   _**Şekil 22:** Seçin **bilgisayarlar** onay kutusu_

4. Şekil 21'de gösterildiği gibi küme adı nesnesi girin. Kaydı zaten oluşturulduğundan, Şekil 20'de gösterildiği gibi izinleri değiştirebilirsiniz.

5. Seçin **güvenlik** paylaşımı ve ardından sekme daha ayrıntılı küme adı nesnesi için izinleri.

   ![Şekil 23: Dosya Paylaşımı çekirdeği üzerinde Küme adı nesnesi için güvenlik özniteliklerini ayarlayın][sap-ha-guide-figure-3022]

   _**Şekil 23:** Dosya Paylaşımı çekirdeği üzerinde Küme adı nesnesi için güvenlik özniteliklerini ayarlayın_

#### <a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a> Yük Devretme Kümesi Yöneticisi'nde dosya paylaşım tanığı çekirdek ayarlayın

1. Açık çekirdeği Ayarlama Sihirbazı'nı yapılandırın.

   ![Şekil 24: Yapılandırma küme çekirdek Ayarlama Sihirbazı'nı başlatın][sap-ha-guide-figure-3023]

   _**Şekil 24:** Yapılandırma küme çekirdek Ayarlama Sihirbazı'nı başlatın_

2. Üzerinde **çekirdek yapılandırma seçeneğini** sayfasında **çekirdek tanığı Seç**.

   ![Şekil 25: Çekirdek yapılandırmaları arasından seçim yapabilirsiniz][sap-ha-guide-figure-3024]

   _**Şekil 25:** Çekirdek yapılandırmaları arasından seçim yapabilirsiniz_

3. Üzerinde **çekirdek tanığı Seç** sayfasında **dosya paylaşım tanığı yapılandırma**.

   ![Şekil 26: Dosya paylaşım tanığı seçin][sap-ha-guide-figure-3025]

   _**Şekil 26:** Dosya paylaşım tanığı seçin_

4. Dosya Paylaşımı için UNC yolu girin (Bizim örneğimizde \\domcontr 0\FSW). Yapabilirsiniz değişikliklerin bir listesini görmek için seçin **sonraki**.

   ![Şekil 27: Tanık paylaşımı için dosya paylaşım konumunu tanımlayın][sap-ha-guide-figure-3026]

   _**Şekil 27:** Tanık paylaşımı için dosya paylaşım konumunu tanımlayın_

5. Seçin ve ardından değişiklikleri **sonraki**. Şekil 28'de gösterildiği gibi küme yapılandırmasını başarılı bir şekilde yeniden yapılandırmanız gerekir:  

   ![Şekil 28: Küme yeniden yapılandırılması onayı][sap-ha-guide-figure-3027]

   _**Şekil 28:** Küme yeniden yapılandırılması onayı_

Windows Yük devretme kümesi başarıyla yükledikten sonra bunlar koşullara azure'da yük devretme algılama uyum bazı eşiklerini değiştirmek gerekir. Değiştirilecek parametreleri bölümünde belgelendirilen [yük devretme kümesi ağ eşikleri ayarlarken][tuning-failover-cluster-network-thresholds]. İki sanal makinelerinizin oluşturan varsayarak ASCS/SCS Windows Küme yapılandırması aynı alt ağdaki içindir, bu değerleri aşağıdaki parametreleri değiştirin:

- SameSubNetDelay = 2
- SameSubNetThreshold = 15

Bu ayarlar, müşterilerle test edilmiş ve iyi bir seçim sunar. Bunlar yeterince esnektir, ancak ayrıca SAP yazılım veya bir düğüm veya VM hatası gerçek hata koşullarında yeterince hızlı yük devretme sağlarlar.

### <a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a> SAP ASCS/SCS Küme Paylaşımı diski için SIOS DataKeeper Cluster Edition'ı yükleme

Artık Azure'da çalışan bir Windows Server Yük devretme kümeleme yapılandırması vardır. SAP ASCS/SCS örneği yüklemek için paylaşılan disk kaynağı gerekir. Azure'da ihtiyacınız olan paylaşılan disk kaynakları oluşturulamıyor. SIOS DataKeeper Cluster Edition paylaşılan disk kaynakları oluşturmak için kullanabileceğiniz bir üçüncü taraf çözümüdür.

SAP ASCS/SCS Küme Paylaşımı diski için SIOS DataKeeper Cluster Edition yüklemek, bu görevleri içerir:

- Microsoft .NET Framework 3.5 ekleyin.
- SIOS DataKeeper yükleyin.
- SIOS DataKeeper ' ayarlayın.

### <a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a> .NET Framework 3.5 Ekle
.NET framework 3.5 otomatik olarak etkinleştirilmiş veya Windows Server 2012 R2'de yüklü değil. SIOS DataKeeper .NET DataKeeper yüklediğiniz tüm düğümlerinde olması gerektiğinden, kümedeki tüm sanal makinelerin konuk işletim sisteminde .NET Framework 3.5 yüklemeniz gerekir.

.NET Framework 3.5 eklemenin iki yolu vardır:

- Rol ve Özellik Ekleme Sihirbazı Windows, Şekil 29 ' gösterildiği gibi kullanın:

  ![Şekil 29: Rol ve Özellik Ekleme Sihirbazı'nı kullanarak .NET Framework 3.5 yükleyin][sap-ha-guide-figure-3028]

  _**Şekil 29:** Rol ve Özellik Ekleme Sihirbazı'nı kullanarak .NET Framework 3.5 yükleyin_

  ![Şekil 30: Rol ve Özellik Ekleme Sihirbazı'nı kullanarak .NET Framework 3.5 yüklerken çubuğu yükleme ilerleme durumu][sap-ha-guide-figure-3029]

  _**Şekil 30:** Rol ve Özellik Ekleme Sihirbazı'nı kullanarak .NET Framework 3.5 yüklerken çubuğu yükleme ilerleme durumu_

- Dism.exe komut satırı aracını kullanın. Bu yükleme türü için Windows yükleme medyasında bulunan SxS dizin erişmeniz gerekir. Yükseltilmiş bir komut isteminde şu komutu girin:

  ```
  Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
  ```

### <a name="dd41d5a2-8083-415b-9878-839652812102"></a> SIOS DataKeeper yükleyin

Kümedeki her düğümde SIOS DataKeeper Cluster Edition yükleyin. SIOS DataKeeper ile sanal paylaşılan depolama alanı oluşturmak için eşitlenen bir yansıtma oluşturun ve ardından Küme Paylaşılan depolamayı benzetmek.

SIOS yazılımını yüklemeden önce DataKeeperSvc etki alanı kullanıcısı oluşturun.

> [!NOTE]
> DataKeeperSvc etki alanı kullanıcı her iki küme düğümlerinde yerel yönetici grubuna ekleyin.
>
>

SIOS DataKeeper yüklemek için:

1. SIOS yazılımı, her iki küme düğümlerinde yükleyin.

   ![SIOS yükleyici][sap-ha-guide-figure-3030]

   ![Şekil 31: SIOS DataKeeper yüklemenin ilk sayfa][sap-ha-guide-figure-3031]

   _**Şekil 31:** SIOS DataKeeper yüklemenin ilk sayfa_

2. İletişim kutusunda **Evet**.

   ![Şekil 32: Bir hizmet devre dışı bırakılacak DataKeeper bildirir][sap-ha-guide-figure-3032]

   _**Şekil 32:** Bir hizmet devre dışı bırakılacak DataKeeper bildirir_

3. İletişim kutusunda, seçtiğiniz olan öneririz **etki alanı veya sunucu hesabı**.

   ![Şekil 33: SIOS DataKeeper için kullanıcı seçimi][sap-ha-guide-figure-3033]

   _**Şekil 33:** SIOS DataKeeper için kullanıcı seçimi_

4. SIOS DataKeeper için oluşturduğunuz parola ve etki alanı hesabı kullanıcı adı girin.

   ![Şekil 34: Etki alanı kullanıcı adı ve parolayı için SIOS DataKeeper yükleme girin][sap-ha-guide-figure-3034]

   _**Şekil 34:** Etki alanı kullanıcı adı ve parolayı için SIOS DataKeeper yükleme girin_

5. SIOS DataKeeper Örneğiniz için lisans anahtarı Şekil 35 ' gösterildiği gibi yükleyin.

   ![Şekil 35: SIOS DataKeeper lisans anahtarınızı girin][sap-ha-guide-figure-3035]

   _**Şekil 35:** SIOS DataKeeper lisans anahtarınızı girin_

6. İstendiğinde, sanal makineyi yeniden başlatın.

### <a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a> SIOS DataKeeper ' ayarlayın

Her iki düğümde SIOS DataKeeper yükledikten sonra yapılandırmayı başlatın. Yapılandırma amacı, her sanal makineye bağlı ek diskler arasında zaman uyumlu veri çoğaltmayı olmasını sağlamaktır.

1. DataKeeper yönetim ve yapılandırma aracını başlatın ve ardından **Connect Server**.

   ![Şekil 36: SIOS DataKeeper yönetim ve yapılandırma aracı][sap-ha-guide-figure-3036]

   _**Şekil 36:** SIOS DataKeeper yönetim ve yapılandırma aracı_

2. Adını veya ilk düğümü yönetim ve yapılandırma aracını için ve ikinci adım, ikinci düğüme bağlanmak TCP/IP'yi adresini girin.

   ![Şekil 37: TCP/IP adresi ilk düğümü yönetim ve yapılandırma aracı için ve ikinci adım, ikinci düğümü bağlanmanız gerekir ya da adını Ekle][sap-ha-guide-figure-3037]

   _**Şekil 37:** TCP/IP adresi ilk düğümü yönetim ve yapılandırma aracı için ve ikinci adım, ikinci düğümü bağlanmanız gerekir ya da adını Ekle_

3. İki düğüm arasındaki çoğaltma işi oluşturun.

   ![Şekil 38: Çoğaltma işi oluşturma][sap-ha-guide-figure-3038]

   _**Şekil 38:** Çoğaltma işi oluşturma_

   Bir sihirbaz çoğaltma işi oluşturma işleminde size kılavuzluk eder.

4. Çoğaltma işi adı tanımlayın.

   ![Şekil 39: Çoğaltma işi adını tanımlayın][sap-ha-guide-figure-3039]

   _**Şekil 39:** Çoğaltma işi adını tanımlayın_

   ![Şekil 40: Geçerli kaynak düğüm olmalıdır düğümü için temel veri tanımlama][sap-ha-guide-figure-3040]

   _**Şekil 40:** Geçerli kaynak düğüm olmalıdır düğümü için temel veri tanımlama_

5. Adı, TCP/IP adresi ve hedef düğümünün disk birimi tanımlayın.

   ![Şekil 41: Adı, TCP/IP adresi ve geçerli hedef düğümünün disk birimi tanımlayın][sap-ha-guide-figure-3041]

   _**Şekil 41:** Adı, TCP/IP adresi ve geçerli hedef düğümünün disk birimi tanımlayın_

6. Sıkıştırma algoritmaları tanımlar. Bizim örneğimizde, çoğaltma akışını sıkıştırmak öneririz. Çoğaltma akışı sıkıştırma, özellikle yeniden eşitleme durumlarda yeniden eşitleme zamanı önemli ölçüde azaltır. Sıkıştırma, bir sanal makinenin CPU ve RAM kaynaklarını kullanır. Sıkıştırma oranı arttıkça, bu nedenle kullanılan CPU kaynakları hacmi yapar. Bu ayar daha sonra ayarlayabilirsiniz.

7. Başka bir ayarı kontrol etmeniz mi çoğaltma zaman uyumlu veya zaman uyumsuz olarak gerçekleşir. SAP ASCS/SCS yapılandırmaları koruduğunuzda, zaman uyumlu çoğaltma kullanmanız gerekir.  

   ![Şekil 42: Çoğaltma ayrıntılarını tanımlayın][sap-ha-guide-figure-3042]

   _**Şekil 42:** Çoğaltma ayrıntılarını tanımlayın_

8. Çoğaltma işi tarafından çoğaltılmış birimi paylaşılan bir disk olarak bir Windows Server Yük devretme kümesi yapılandırması için gösterilen olup olmadığını tanımlar. SAP ASCS/SCS yapılandırmasını seçin **Evet** böylece küme birimi olarak kullanabileceği paylaşılan bir disk olarak çoğaltılmış bir birimi Windows Küme görür.

   ![Şekil 43: Çoğaltılmış birimi küme birimi olarak ayarlamak için Evet'i seçin][sap-ha-guide-figure-3043]

   _**Şekil 43:** Seçin **Evet** çoğaltılmış birimi küme birimi olarak ayarlamak için_

   Birim oluşturulduktan sonra DataKeeper yönetimini ve yapılandırmasını araç çoğaltma işi etkin olduğunu gösterir.

   ![Şekil 44: SAP ASCS/SCS paylaşımı diski DataKeeper zaman uyumlu yansıtma etkin][sap-ha-guide-figure-3044]

   _**Şekil 44:** SAP ASCS/SCS paylaşımı diski DataKeeper zaman uyumlu yansıtma etkin_

   Yük Devretme Kümesi Yöneticisi şekil 45 gösterildiği DataKeeper disk olarak disk artık gösterir:

   ![Şekil 45: Yük Devretme Kümesi Yöneticisi DataKeeper çoğaltılan disk gösterir.][sap-ha-guide-figure-3045]

   _**Şekil 45:** Yük Devretme Kümesi Yöneticisi DataKeeper çoğaltılan disk gösterir._

## <a name="next-steps"></a>Sonraki adımlar

* [SAP NetWeaver HA SAP ASCS/SCS örneği için bir Windows Yük devretme kümesi ve paylaşılan disk kullanarak yükleme][sap-high-availability-installation-wsfc-shared-disk]
