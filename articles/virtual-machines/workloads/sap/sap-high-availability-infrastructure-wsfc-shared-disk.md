---
title: Azure altyapı SAP SAP ASCS/SCS için bir Windows Yük devretme kümesi ve paylaşılan disk kullanarak HA hazırlama | Microsoft Docs
description: Azure altyapı SAP HA için bir Windows Yük devretme kümesi ve paylaşılan disk için bir SAP ASCS/SCS örneği kullanarak nasıl hazırlayacağınızı öğrenin.
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
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
ms.openlocfilehash: 25d3d01e12132165cc9e12032ba0f6e7a2f15070
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="prepare-the-azure-infrastructure-for-sap-ha-by-using-a-windows-failover-cluster-and-shared-disk-for-sap-ascsscs"></a>Azure altyapı SAP HA için SAP ASCS/SCS için Windows Yük devretme kümesi ve paylaşılan disk kullanarak hazırlama

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

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

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
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
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

Bu makalede Azure altyapı yükleme ve kullanarak Windows Yük devretme kümesinde yüksek kullanılabilirlik SAP sistem yapılandırma hazırlamak için uygulayacağınız adımlar bir *Küme Paylaşılan disk* kümeleme için bir seçenek olarak bir SAP ASCS örneği.

## <a name="prerequisites"></a>Önkoşullar

Yüklemeye başlamadan önce bu makalede gözden geçirin:

* [Mimari Kılavuzu: Windows Yük devretme kümesinde Küme Paylaşılan bir disk kullanarak bir SAP ASCS/SCS örneği küme][sap-high-availability-guide-wsfc-shared-disk]

## <a name="prepare-the-infrastructure-for-architectural-template-1"></a>Mimari şablonu 1 için altyapıyı hazırlama
Azure Resource Manager şablonları SAP için gerekli kaynakları dağıtımını kolaylaştırır.

Üç katmanlı şablonları Azure Kaynak Yöneticisi'nde, aynı zamanda yüksek kullanılabilirlik senaryolarını destekler. Örneğin, iki küme mimari şablonu 1 vardır. Her küme bir SAP tek hata SAP ASCS/SCS ve DBMS noktasıdır.

İşte burada Biz bu makalede açıklayan örnek senaryo için Azure Resource Manager şablonları elde edebilirsiniz:

* [Azure Market görüntüsü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
* [Azure yönetilen diskleri kullanarak azure Market görüntüsü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-md)  
* [Özel görüntü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)
* [Yönetilen diskleri kullanarak özel görüntü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-md)

Mimari şablonu 1 için altyapıyı hazırlamak için:

- Azure portalında içinde **parametreleri** bölmesi, **SYSTEMAVAILABILITY** kutusunda **HA**.

  ![Şekil 1: Ayarlamak SAP yüksek kullanılabilirlik Azure Resource Manager parametreleri][sap-ha-guide-figure-3000]

_**Şekil 1:** ayarlamak SAP yüksek kullanılabilirlik Azure Resource Manager parametreleri_


  Şablonları oluşturun:

  * **Sanal makineler**:
    * SAP uygulama sunucusu sanal makineleri: \<SAPSystemSID\>- di -\<numarası\>
    * ASCS/SCS küme sanal makineler: \<SAPSystemSID\>- ascs-\<numarası\>
    * DBMS küme: \<SAPSystemSID\>- db-\<numarası\>

  * **Ağ kartları ilişkili IP adresleriyle tüm sanal makineler için**:
    * \<SAPSystemSID\>- NIC-di -\<numarası\>
    * \<SAPSystemSID\>- NIC-ascs -\<numarası\>
    * \<SAPSystemSID\>- NIC-db -\<numarası\>

  * **Azure depolama hesapları (yalnızca yönetilmeyen diskleri)**:

  * **Kullanılabilirlik grupları** için:
    * SAP uygulama sunucusu sanal makineleri: \<SAPSystemSID\>- avset dı
    * SAP ASCS/SCS küme sanal makineler: \<SAPSystemSID\>- avset ascs
    * DBMS küme sanal makineler: \<SAPSystemSID\>- avset-db

  * **Azure iç yük dengeleyici**:
    * IP adresi ve ASCS/SCS örneği için tüm bağlantı noktaları ile \<SAPSystemSID\>- lb ascs
    * SQL Server DBMS ve IP adresi için tüm bağlantı noktaları ile \<SAPSystemSID\>- lb db

  * **Ağ güvenlik grubu**: \<SAPSystemSID\>- nsg ascs 0  
    * Açık bir dış Uzak Masaüstü Protokolü (RDP) bağlantı noktasına sahip \<SAPSystemSID\>- ascs-0 sanal makine

> [!NOTE]
> Tüm IP adresleri ağ kartları ve Azure iç yük dengeleyicileri varsayılan olarak dinamik. Statik IP adresine değiştirin. Biz bu makalenin sonraki bölümlerinde yapılacağı açıklanmaktadır.
>
>

## <a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a> Üretimde kullanılacak kurumsal ağ bağlantısı (şirket içi) ile sanal makineleri dağıtma
Üretim SAP sistemleri için Azure sanal makinelerle dağıtımı [kurumsal ağ bağlantısı (şirket içi)] [ planning-guide-2.2] Azure VPN ağ geçidi veya Azure ExpressRoute kullanarak.

> [!NOTE]
> Azure Virtual Network örneğinizi kullanabilirsiniz. Sanal ağ ve alt ağ zaten oluşturulmuş hazırlanmış ve.
>
>

1.  Azure portalında içinde **parametreleri** bölmesi, **NEWOREXISTINGSUBNET** kutusunda **varolan**.
2.  İçinde **SUBNETID** kutusunda, Azure sanal makinelerinizi dağıtmak planladığınız hazırlanan Azure ağ alt ağı Kimliğiniz tam dizesi ekleyin.
3.  Tüm Azure ağ alt ağlar listesini almak için bu PowerShell komutunu çalıştırın:

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets
  ```

  **Kimliği** alan değeri alt ağ kimliği için gösterir
4. Tüm alt ağ kimliği değerlerinin bir listesini almak için bu PowerShell komutunu çalıştırın:

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets.Id
  ```

  Alt ağ kimliği şöyle görünür:

  ```
  /subscriptions/<subscription ID>/resourceGroups/<VPN name>/providers/Microsoft.Network/virtualNetworks/azureVnet/subnets/<subnet name>
  ```

## <a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a> Test ve demo için yalnızca bulut SAP örnekleri dağıtma
Yüksek kullanılabilirlik SAP sisteminizi bir yalnızca bulut dağıtım modelinde dağıtabilirsiniz. Bu tür bir dağıtım öncelikle tanıtım ve test kullanım durumları için yararlıdır. Üretim kullanım durumları için uygun değildir.

- Azure portalında içinde **parametreleri** bölmesi, **NEWOREXISTINGSUBNET** kutusunda **yeni**. Bırakın **SUBNETID** alanı boş.

  SAP Azure Resource Manager şablonu, Azure sanal ağ ve alt otomatik olarak oluşturur.

> [!NOTE]
> Ayrıca Active Directory ve DNS hizmeti aynı Azure sanal ağ örneğinde için en az bir ayrılmış sanal makine dağıtmak için gerekir. Şablon, bu sanal makineleri oluşturmaz.
>
>


## <a name="prepare-the-infrastructure-for-architectural-template-2"></a>Mimari şablon 2 için altyapıyı hazırlama

Bu Azure Resource Manager şablonu SAP için gerekli altyapı kaynaklarıdır dağıtım SAP mimari şablon 2 basitleştirmeye yardımcı olması için kullanabilirsiniz.

İşte, bu dağıtım senaryosu için Azure Resource Manager şablonları burada alabilirsiniz:

* [Azure Market görüntüsü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged)  
* [Yönetilen diskleri kullanarak azure Market görüntüsü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged-md)  
* [Özel görüntü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged)
* [Yönetilen diskleri kullanarak özel görüntü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged-md)


## <a name="prepare-the-infrastructure-for-architectural-template-3"></a>Mimari şablonu 3 için altyapıyı hazırlama

Altyapıyı hazırlama ve SAP çoklu SID için yapılandırın. Örneğin, ek bir SAP ASCS/SCS örneğine ekleyebileceğiniz bir *varolan* küme yapılandırması. Daha fazla bilgi için bkz: [Azure Kaynak Yöneticisi'nde bir SAP çoklu SID yapılandırması oluşturmak var olan bir küme yapılandırması için ek SAP ASCS/SCS örnek yapılandırma][sap-ha-multi-sid-guide].

Yeni bir SID çoklu küme oluşturmak istiyorsanız, çoklu SID kullanabilirsiniz [GitHub hızlı başlangıç şablonlarında](https://github.com/Azure/azure-quickstart-templates).

Yeni bir SID çoklu küme oluşturmak için aşağıdaki üç şablonları dağıtmanız gerekir:

* [ASCS/SCS şablonu](#ASCS-SCS-template)
* [Veritabanı şablonu](#database-template)
* [Uygulama sunucuları şablonu](#application-servers-template)

Aşağıdaki bölümlerde şablonları ve şablonları sağlamanız gereken parametreleri hakkında daha fazla ayrıntı sahip.

### <a name="ASCS-SCS-template"></a> ASCS/SCS şablonu

ASCS/SCS şablonu birden fazla ASCS/SCS örneği barındıran bir Windows Server Yük devretme kümesi oluşturmak için kullanabileceğiniz iki sanal makine dağıtır.

ASCS/SCS çoklu SID şablonunu, buna ayarlamak için [ASCS/SCS çoklu SID şablonu] [ sap-templates-3-tier-multisid-xscs-marketplace-image] veya [yönetilen diskleri kullanarak ASCS/SCS çoklu SID şablonu] [ sap-templates-3-tier-multisid-xscs-marketplace-image-md], aşağıdaki parametreler için değerler girin:

  - **Kaynak önek**: dağıtım sırasında oluşturulan tüm kaynakları öneki için kullanılan kaynak öneki ayarlayın. Kaynaklar için yalnızca bir SAP sistemine ait olmadığından kaynak önek bir SAP sistem SID'si değil.  Önek üç ve altı karakter arasında olmalıdır.
  - **Yığın türü**: SAP Sistem yığını türünü seçin. Yığın türüne bağlı olarak, Azure yük dengeleyici (ABAP veya yalnızca Java) bir veya iki (ABAP + Java) özel IP adresleri SAP sistem başına var.
  -  **İşletim sistemi türü**: sanal makinelerin işletim sistemini seçin.
  -  **SAP sistem sayısı**: Bu kümede yüklemek istediğiniz SAP sistemleri sayısını seçin.
  -  **Sistem kullanılabilirliğini**: seçin **HA**.
  -  **Yönetici kullanıcı adı ve yönetici parolası**: makinede oturum açmak için kullanılan yeni bir kullanıcı oluşturun.
  -  **Yeni veya mevcut alt**: yeni sanal ağ ve alt ağ oluşturmak veya mevcut bir alt kullanmayı ayarlayın. Şirket içi ağınıza bağlı bir sanal ağ zaten varsa, seçin **varolan**.
  -  **Alt ağ kimliği**: kümesi için sanal makineleri bağlı alt ağ kimliği. Sanal makine şirket içi ağınıza bağlanmak için VPN ya da ExpressRoute sanal ağınızın alt ağ seçin. Kimliği genellikle şu şekildedir:

   /Subscriptions/\<abonelik kimliği\>/resourceGroups/\<kaynak grubu adı\>/providers/Microsoft.Network/virtualNetworks/\<sanal ağ adı\>/subnets/ \<alt ağ adı\>

Şablon birden çok SAP sistemlerini destekleyen bir Azure yük dengeleyici örneği dağıtır:

- ASCS örnekleri örnek numarası 00, 10, 20 yapılandırılmış...
- SCS örnekleri örnek numarası 01, 11, 21 yapılandırılmış...
- ASCS kuyruğa çoğaltma sunucusuna (ERS) (yalnızca Linux) örnekleri örnek numarası 02, 12, 22 yapılandırılmış...
- SCS ERS (yalnızca Linux) örnekleri örneği için 03, 13, 23 sayısı yapılandırılan...

Yük Dengeleyici 1 içeren VIP(s) (Linux için 2), 1 x VIP ASCS/SCS için ve 1 x VIP ERS (yalnızca Linux) için.

#### <a name="0f3ee255-b31e-4b8a-a95a-d9ed6200468b"></a> SAP ASCS/SCS bağlantı noktaları
Aşağıdaki listede tüm Yük Dengeleme kuralları (burada x, SAP sisteminin, örneğin, 1, 2, 3... sayıdır) içerir:
- Her SAP sistemi için Windows özel bağlantı noktaları: 445, 5985
- ASCS bağlantı noktası (örnek numarasını x0): 32 x 0, 36 x 0, 39 x 0, 81 x 0, 5 x 013, 5 x 014, 5 x 016
- SCS bağlantı noktası (örnek numarasını x1): 32 x 1, 33 x 1, 39 x 1, 81 x 1, 5 x 113, 5 x 114, 5 x 116
- ASCS ERS bağlantı noktaları (örnek numarasını x2) Linux'ta: 33 x 2, 5 x 213, 5 x 214, 5 x 216
- SCS ERS bağlantı noktaları (örnek numarasını x3) Linux'ta: 33 x 3, 5 x 313, 5 x 314, 5 x 316

Yük Dengeleyici (burada x, SAP sisteminin, örneğin, 1, 2, 3... sayıdır) aşağıdaki araştırma bağlantı noktalarını kullanacak şekilde yapılandırılır:
- ASCS/SCS iç yük dengeleyici araştırması bağlantı noktası: 620 x 0
- ERS iç yük dengeleyici araştırması bağlantı noktası (yalnızca Linux): 621 x 2

### <a name="database-template"></a> Veritabanı şablonu

Veritabanı şablonu bir veya iki sanal bir SAP sistem ilişkisel veritabanı yönetim sistemi (RDBMS) yüklemek için kullanabileceğiniz makineleri dağıtır. Örneğin, beş SAP sistemleri için bir ASCS/SCS şablonu dağıtırsanız, bu şablonu beş kez dağıtmak için gerekir.

Veritabanı çoklu SID şablonu ayarlamak için [veritabanı çoklu SID şablonu] [ sap-templates-3-tier-multisid-db-marketplace-image] veya [yönetilen diskleri kullanarak veritabanı çoklu SID şablonu] [ sap-templates-3-tier-multisid-db-marketplace-image-md], aşağıdaki parametreler için değerler girin:

  -  **SAP sistem kimliği**: yüklemek istediğiniz SAP sistem SAP sistem Kimliğini girin. Kimliği önek olarak dağıtılan kaynaklar için kullanılır.
  -  **İşletim sistemi türü**: sanal makinelerin işletim sistemini seçin.
  -  **DbType**: kümede yüklemek istediğiniz veritabanının türünü seçin. Seçin **SQL** Microsoft SQL Server yüklemek istiyorsanız. Seçin **HANA** sanal makinelerde SAP HANA yüklemeyi planlıyorsanız. Doğru işletim sistemi türü seçtiğinizden emin olun. Seçin **Windows** HANA için Linux dağıtımı seçin ve SQL için. Sanal makinelere bağlı Azure yük dengeleyici, seçili veritabanı türü destekleyecek şekilde yapılandırılır:
    * **SQL**: yük dengeleyici Yük Dengeleme bağlantı noktası 1433. SQL Server AlwaysOn ayarlarınızı bu bağlantı noktasını kullandığınızdan emin olun.
    * **HANA**: yük dengeleyici Yük Dengeleme ve bağlantı noktaları 35015 35017. SAP HANA ile örnek numarasını yüklediğinizden emin olun **50**.
    Yük Dengeleyici araştırması bağlantı noktası 62550 kullanır.
  -  **SAP sistem boyutu**: yeni sistem sağlar SAP sayısını ayarlayın. Sistem gerektirir kaç SAP değil eminseniz, SAP teknolojisi iş ortağı veya sistem Tümleştirici isteyin.
  -  **Sistem kullanılabilirliğini**: seçin **HA**.
  -  **Yönetici kullanıcı adı ve yönetici parolası**: makinede oturum açmak için kullanılan yeni bir kullanıcı oluşturun.
  -  **Alt ağ kimliği**: ASCS/SCS şablonu dağıtımının bir parçası ASCS/SCS şablon dağıtımı sırasında kullanılan alt ağ kimliği veya oluşturulmuş alt ağ Kimliğini girin.

### <a name="application-servers-template"></a> Uygulama sunucuları şablonu

Uygulama sunucuları şablonu iki veya daha fazla sanal SAP uygulama sunucusu örneklerinin bir SAP sistemi için kullanılabilecek makineler dağıtır. Örneğin, beş SAP sistemleri için bir ASCS/SCS şablonu dağıtırsanız, bu şablonu beş kez dağıtmak için gerekir.

Uygulama sunucuları çoklu SID şablonu ayarlamak için [uygulama sunucuları çoklu SID şablonu] [ sap-templates-3-tier-multisid-apps-marketplace-image] veya [yönetilen disklerikullanarakuygulamasunucularıçokluSIDşablonu] [ sap-templates-3-tier-multisid-apps-marketplace-image-md], aşağıdaki parametreler için değerler girin:

  -  **SAP sistem kimliği**: yüklemek istediğiniz SAP sistem SAP sistem Kimliğini girin. Kimliği önek olarak dağıtılan kaynaklar için kullanılır.
  -  **İşletim sistemi türü**: sanal makinelerin işletim sistemini seçin.
  -  **SAP sistem boyutu**: yeni sistem sağlar SAP sayısı. Sistem gerektirir kaç SAP değil eminseniz, SAP teknolojisi iş ortağı veya sistem Tümleştirici isteyin.
  -  **Sistem kullanılabilirliğini**: seçin **HA**.
  -  **Yönetici kullanıcı adı ve yönetici parolası**: makinede oturum açmak için kullanılan yeni bir kullanıcı oluşturun.
  -  **Alt ağ kimliği**: ASCS/SCS şablonu dağıtımının bir parçası ASCS/SCS şablon dağıtımı sırasında kullanılan alt ağ kimliği veya oluşturulmuş alt ağ Kimliğini girin.


## <a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a> Azure sanal ağı
Bizim örneğimizde, Azure sanal ağı örneği adres alanı 10.0.0.0/16 şeklindedir. Alt ağ, 10.0.0.0/24 bir adres aralığı adı verilen bir alt ağ yok. Bu sanal ağda, tüm sanal makineler ve iç yük dengeleyicileri dağıtılır.

> [!IMPORTANT]
> Ağ ayarları konuk işletim sistemi içinde herhangi bir değişiklik yoktur. Bu IP adresleri, DNS sunucuları ve alt ağ içerir. Azure'da tüm ağ ayarlarını yapılandırın. Dinamik Ana Bilgisayar Yapılandırma Protokolü (DHCP) hizmeti ayarlarınızı yayar.
>
>

## <a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a> DNS IP adresleri

Gerekli DNS IP adreslerini ayarlamak için aşağıdaki adımları tamamlayın:

1.  Azure portalında içinde **DNS sunucuları** bölmesinde, olduğundan emin olun, sanal ağınızı **DNS sunucuları** seçeneği **özel DNS**.
2.  Sahip olduğunuz ağ türüne göre ayarlarınızı seçin. Daha fazla bilgi için aşağıdaki kaynaklara bakın:
    * [Kurumsal ağ bağlantısı (şirket içi)][planning-guide-2.2]: şirket içi DNS sunucularının IP adreslerini ekleyin.  
    Azure'da çalışan sanal makineleri şirket içi DNS sunucularına genişletebilirsiniz. Bu senaryoda DNS hizmeti çalışan Azure sanal makinelerin IP adreslerini ekleyebilirsiniz.
    * [Yalnızca bulut dağıtım][planning-guide-2.1]: bir DNS sunucusu olarak hizmet veren aynı sanal ağ örneğinde ek bir sanal makine dağıtın. DNS hizmeti çalıştırmak kadar ayarladığınızdan Azure sanal makinelerin IP adreslerini ekleyin.

    ![Şekil 2: Azure sanal ağı için DNS sunucularını yapılandırın][sap-ha-guide-figure-3001]

    _**Şekil 2:** Yapılandır DNS sunucuları için Azure sanal ağ_

  > [!NOTE]
  > DNS sunucularının IP adreslerini değiştirirseniz, değişikliği uygulamak ve yeni DNS sunucularını yayılması için Azure sanal makineleri yeniden başlatmanız gerekir.
  >
  >

Bizim örneğimizde, DNS hizmeti yüklenir ve bu Windows sanal makinelerde yapılandırılır:

| Sanal makine rolü | Sanal makine ana bilgisayar adı | Ağ kartı adı | Statik IP adresi |
| --- | --- | --- | --- |
| İlk DNS sunucusu |domcontr-0 |pr1-NIC-domcontr-0 |10.0.0.10 |
| İkinci DNS sunucusu |domcontr-1 |pr1-NIC-domcontr-1 |10.0.0.11 |

## <a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a> Ana bilgisayar adları ve SAP ASCS/SCS kümelenmiş örneği ve DBMS kümelenmiş örneği için statik IP adresleri

Şirket içi dağıtım için bu ayrılmış ana bilgisayar adlarını ve IP adreslerini gerekir:

| Sanal ana bilgisayar adı rolü | Sanal ana bilgisayar adı | Sanal statik IP adresi |
| --- | --- | --- |
| SAP ASCS/SCS ilk küme sanal ana bilgisayar adı (küme yönetimi) |pr1 ascs VIR |10.0.0.42 |
| SAP ASCS/SCS örnek sanal ana bilgisayar adı |pr1 ascs sap |10.0.0.43 |
| SAP DBMS ikinci küme sanal ana bilgisayar adı (küme yönetimi) |pr1-dbms-vir |10.0.0.32 |

Kümeyi oluşturduğunuzda, sanal ana bilgisayar adları pr1-ascs-VIR ve pr1 dbms VIR ve kümeyi yönetmek ilişkili IP adresleri oluşturun. Bunun nasıl yapılacağı hakkında daha fazla bilgi için bkz: [toplamak küme düğümleri bir küme yapılandırmasında][sap-high-availability-infrastructure-wsfc-shared-disk-collect-cluster-config].

DNS sunucusunda diğer iki sanal ana bilgisayar adlarını pr1 ascs sap ve pr1-dbms-sap ve ilişkili IP adreslerini el ile oluşturabilirsiniz. Kümelenmiş SAP ASCS/SCS örneği ve kümelenmiş DBMS örneği bu kaynakları kullanın. Bunun nasıl yapılacağı hakkında daha fazla bilgi için bkz: [kümelenmiş bir SAP ASCS/SCS örneği için bir sanal ana bilgisayar adı oluşturmak][sap-ha-guide-9.1.1].

## <a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a> SAP sanal makineler için statik IP adresi ayarlayın
Kümedeki sanal makinelerin dağıttıktan sonra tüm sanal makineler için statik IP adresi ayarlamak gerekir. Bunu yapmak, Azure sanal ağ yapılandırması ve konuk işletim sistemi içinde değil.

1.  Azure portalında seçin **kaynak grubu** > **ağ kartı** > **ayarları** > **IP adresi**.
2.  İçinde **IP adreslerini** bölmesi altında **atama**seçin **statik**. İçinde **IP adresi** kutusunda, kullanmak istediğiniz IP adresini girin.

  > [!NOTE]
  > Ağ kartı IP adresini değiştirirseniz, değişikliği uygulamak için Azure sanal makineleri yeniden başlatmanız gerekir.  
  >
  >

  ![Şekil 3: statik IP adresleri her sanal makinenin ağ kartı için ayarlama][sap-ha-guide-figure-3002]

  _**Şekil 3:** ayarlamak, her bir sanal makinenin ağ kartı için statik IP adresleri_

  Tüm sanal makineler için Active Directory veya DNS hizmetiniz için kullanmak istediğiniz sanal makineleri dahil olan tüm ağ arabirimleri için bu adımı yineleyin.

Bizim örneğimizde, bu sanal makineleri ve statik IP adresleri vardır:

| Sanal makine rolü | Sanal makine ana bilgisayar adı | Ağ kartı adı | Statik IP adresi |
| --- | --- | --- | --- |
| İlk SAP uygulama sunucusu örneği |pr1 dı 0 |pr1-NIC-dı-0 |10.0.0.50 |
| İkinci SAP uygulama sunucusu örneği |pr1 dı 1 |pr1-NIC-dı-1 |10.0.0.51 |
| ... |... |... |... |
| Son SAP uygulama sunucusu örneği |pr1 dı 5 |pr1-nic-di-5 |10.0.0.55 |
| İlk küme düğümüne ASCS/SCS örneği için |pr1 ascs 0 |pr1-NIC-ascs-0 |10.0.0.40 |
| İkinci küme düğümü ASCS/SCS örneği için |pr1 ascs 1 |pr1-NIC-ascs-1 |10.0.0.41 |
| İlk küme düğümüne DBMS örneği için |pr1-db-0 |pr1-NIC-db-0 |10.0.0.30 |
| DBMS örneği için ikinci küme düğümü |pr1-db-1 |pr1-NIC-db-1 |10.0.0.31 |

## <a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a> Azure iç yük dengeleyici için statik bir IP adresi ayarlayın

SAP Azure Resource Manager şablonu SAP ASCS/SCS örnek küme ve DBMS küme için kullanılan bir Azure iç yük dengeleyici oluşturur.

> [!IMPORTANT]
> Sanal ana bilgisayar adını SAP ASCS/SCS IP adresini SAP ASCS/SCS iç yük dengeleyici IP adresi ile aynıdır: pr1 lb ascs.
> DBMS sanal adını IP adresini DBMS iç yük dengeleyici IP adresi ile aynıdır: pr1 lb dbms.
>
>

Azure iç yük dengeleyici için statik bir IP adresi ayarlamak için:

1.  İç yük dengeleyici IP adresi ilk dağıtım ayarlar **dinamik**. Azure portalında üzerinde **IP adreslerini** bölmesi altında **atama**seçin **statik**.
2.  İç yük dengeleyici IP adresi kümesi **pr1 lb ascs** SAP ASCS/SCS örneğinin sanal ana bilgisayar adı IP adresine.
3.  İç yük dengeleyici IP adresi kümesi **pr1 lb dbms** DBMS örneğinin sanal ana bilgisayar adı IP adresine.

  ![Şekil 4: SAP ASCS/SCS örneği için iç yük dengeleyici için statik IP adresleri ayarlayın.][sap-ha-guide-figure-3003]

  _**Şekil 4:** SAP ASCS/SCS örneği için iç yük dengeleyici için statik IP adresleri ayarlayın_

Bizim örneğimizde, biz bu statik IP adresine sahip iki Azure iç yük dengeleyicileri vardır:

| Azure iç yük dengeleyici rol | Azure iç yük dengeleyicisi adı | Statik IP adresi |
| --- | --- | --- |
| SAP ASCS/SCS iç yük dengeleyici örneği |pr1 lb ascs |10.0.0.43 |
| SAP DBMS iç yük dengeleyici |pr1 lb dbms |10.0.0.33 |


## <a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a> Varsayılan ASCS/SCS Yük Dengeleme kuralları Azure iç yük dengeleyici için

SAP Azure Resource Manager şablonu gereksinim duyduğunuz bağlantı noktalarını oluşturur:
* Varsayılan örnek numarasını 00 ile ABAP ASCS örneği
* Bir Java SCS örneğiyle varsayılan örnek numarasını 01

SAP ASCS/SCS örneğinizi yüklediğinizde, varsayılan örnek numarasını 00 ABAP ASCS örneğinizi ve varsayılan örnek numarasını 01 için Java SCS Örneğiniz için kullanmanız gerekir.

Ardından, gerekli iç Yük Dengeleme SAP NetWeaver bağlantı noktaları için uç noktaları oluşturun.

Gerekli iç Yük Dengeleme uç noktaları oluşturmak için ilk olarak, bu Yük Dengeleme SAP NetWeaver ABAP ASCS bağlantı noktaları için uç nokta oluşturun:

| Hizmet/Yük Dengeleme kuralı adı | Varsayılan bağlantı noktası numaraları | Somut için bağlantı noktalarını (ASCS örneği ile örnek numarasını 00) (ERS 10 ile) |
| --- | --- | --- |
| Sıraya alma sunucu / *lbrule3200* |32\<InstanceNumber\> |3200 |
| ABAP ileti sunucusu / *lbrule3600* |36\<InstanceNumber\> |3600 |
| İç ABAP ileti / *lbrule3900* |39\<InstanceNumber\> |3900 |
| İleti sunucusu HTTP / *Lbrule8100* |81\<InstanceNumber\> |8100 |
| SAP ASCS HTTP hizmeti Başlat / *Lbrule50013* |5\<InstanceNumber\>13 |50013 |
| SAP hizmet ASCS HTTPS Başlat / *Lbrule50014* |5\<InstanceNumber\>14 |50014 |
| Sıraya alma çoğaltma / *Lbrule50016* |5\<InstanceNumber\>16 |50016 |
| SAP ERS HTTP hizmetini başlatın *Lbrule51013* |5\<InstanceNumber\>13 |51013 |
| SAP ERS HTTP hizmetini başlatın *Lbrule51014* |5\<InstanceNumber\>14 |51014 |
| Windows Uzaktan Yönetim (WinRM) *Lbrule5985* | |5985 |
| Dosya Paylaşımı *Lbrule445* | |445 |

**Tablo 1:** bağlantı noktası numaralarını SAP NetWeaver ABAP ASCS örnekleri

Ardından, bu Yük Dengeleme SAP NetWeaver Java SCS bağlantı noktaları için uç nokta oluşturun:

| Hizmet/Yük Dengeleme kuralı adı | Varsayılan bağlantı noktası numaraları | Somut için bağlantı noktalarını (SCS örneği ile örnek numarasını 01) (ERS 11 ile) |
| --- | --- | --- |
| Sıraya alma sunucu / *lbrule3201* |32\<InstanceNumber\> |3201 |
| Ağ Geçidi sunucusu / *lbrule3301* |33\<InstanceNumber\> |3301 |
| Java ileti sunucusu / *lbrule3900* |39\<InstanceNumber\> |3901 |
| İleti sunucusu HTTP / *Lbrule8101* |81\<InstanceNumber\> |8101 |
| SAP SCS HTTP hizmeti Başlat / *Lbrule50113* |5\<InstanceNumber\>13 |50113 |
| SAP hizmet SCS HTTPS Başlat / *Lbrule50114* |5\<InstanceNumber\>14 |50114 |
| Sıraya alma çoğaltma / *Lbrule50116* |5\<InstanceNumber\>16 |50116 |
| SAP ERS HTTP hizmetini başlatın *Lbrule51113* |5\<InstanceNumber\>13 |51113 |
| SAP ERS HTTP hizmetini başlatın *Lbrule51114* |5\<InstanceNumber\>14 |51114 |
| WinRM *Lbrule5985* | |5985 |
| Dosya Paylaşımı *Lbrule445* | |445 |

**Tablo 2:** bağlantı noktası numaralarını SAP NetWeaver Java SCS örnekleri

![Şekil 5: varsayılan ASCS/SCS Yük Dengeleme kuralları Azure iç yük dengeleyici için][sap-ha-guide-figure-3004]

_**Şekil 5:** varsayılan ASCS/SCS Yük Dengeleme kuralları Azure iç yük dengeleyici için_

IP adresini yük dengeleyici pr1 lb-dbms DBMS örneğinin sanal ana bilgisayar adı IP adresine ayarlayın.

### <a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a> ASCS/SCS varsayılan Yük Dengeleme kuralları Azure iç yük dengeleyici için değiştirme

SAP ASCS veya SCS örnekleri için farklı numaraları kullanmak istiyorsanız, adlarını ve değerlerini kendi bağlantı noktalarının varsayılan değerleri değiştirmeniz gerekir.

1.  Azure portalında seçin  **\<SID\>-lb ascs yük dengeleyici** > **Yük Dengeleme kuralları**.
2.  Tüm Yük Dengeleme SAP ASCS veya SCS örneğine ait kuralları için bu değerleri değiştirin:

  * Ad
  * Bağlantı noktası
  * Arka uç bağlantı noktası

  Örneğin, varsayılan ASCS örnek numarasını 00-31 değiştirmek istiyorsanız, Tablo 1'de listelenen tüm bağlantı noktaları için değişiklikler yapmanız gerekir.

  Bağlantı noktası için bir güncelleştirme örneği *lbrule3200*.

  ![Şekil 6: ASCS/SCS varsayılan Yük Dengeleme kuralları Azure iç yük dengeleyici için değiştirme][sap-ha-guide-figure-3005]

  _**Şekil 6:** ASCS/SCS varsayılan Yük Dengeleme kuralları Azure iç yük dengeleyici için değiştirme_

## <a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a> Windows sanal makine etki alanına ekleyin

Sanal makineler için statik bir IP adresi atadıktan sonra sanal makine etki alanına ekleyin.

![Şekil 7: bir sanal makine bir etki alanına ekleyin.][sap-ha-guide-figure-3006]

_**Şekil 7:** bir sanal makine bir etki alanına ekleme_

## <a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a> SAP ASCS/SCS örneği her iki küme düğümleri üzerinde kayıt defteri girdilerini ekleyin

Azure yük dengeleyici bağlantıları ayarlanmış bir süre boyunca boşta olduğunda kapanır bağlantıları (boşta zaman aşımı) zaman bir iç yük dengeleyici sahiptir. İlk sıraya alma/dequeue gönderilmesi gerekiyor isteği hemen SAP iş iletişim örnekleri açık bağlantıları SAP sıraya alma işlemlerinde işleyin. Bu bağlantılar genellikle iş işlemi kadar kurulan kalmasını veya sıraya alma işlemi yeniden başlatır. Ancak, belirlenen bir süre için bağlantı boşta kalırsa Azure iç yük dengeleyicisi bağlantıları kapatır. Artık yoksa SAP iş işlemi sıraya alma işlemi için bağlantıyı yeniden kurar çünkü bu bir sorun değildir. Bu etkinlikler SAP işlemlerini Geliştirici izlerini belgelenen, ancak bunlar büyük miktarda ek içerik bu izlemeler oluşturur. TCP/IP'yi değiştirmek için iyi bir fikirdir `KeepAliveTime` ve `KeepAliveInterval` her iki küme düğümlerinde. Bu değişiklikler makalenin sonraki bölümlerinde açıklanan SAP profili parametreleri TCP/IP'yi parametrelerle ile birleştirin.

Kayıt defteri girdileri SAP ASCS/SCS örneği üzerinde her iki küme düğümü eklemek için ilk olarak, bu Windows kayıt defteri girdileri hem Windows Küme düğümlerinde SAP ASCS/SCS için ekleyin:

| Yol | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Değişken adı |`KeepAliveTime` |
| Değişken türü |REG_DWORD (ondalık) |
| Değer |120000 |
| Belgelere bağlantı |[https://technet.microsoft.com/library/cc957549.aspx](https://technet.microsoft.com/library/cc957549.aspx) |

**Tablo 3:** ilk TCP/IP'yi parametre değiştirme

Daha sonra bu Windows kayıt defteri girdisi SAP ASCS/SCS için hem Windows küme düğümlerine ekleyin:

| Yol | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Değişken adı |`KeepAliveInterval` |
| Değişken türü |REG_DWORD (ondalık) |
| Değer |120000 |
| Belgelere bağlantı |[https://technet.microsoft.com/library/cc957548.aspx](https://technet.microsoft.com/library/cc957548.aspx) |

**Tablo 4:** ikinci TCP/IP'yi parametre değiştirme

Değişiklikleri uygulamak için her iki küme düğümü yeniden başlatın.

## <a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a> Windows Server Yük devretme kümesi bir SAP ASCS/SCS örneği için ayarlama

SAP ASCS/SCS örneği için bir Windows Server Yük devretme ayarlama, bu görevleri içerir:

- Bir küme yapılandırmasında küme düğümleri toplayın.
- Bir küme dosya paylaşımı tanığı yapılandırın.

### <a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a> Bir küme yapılandırmasında küme düğümleri Topla

1.  Rol Ekle ve Özellik Ekleme Sihirbazı, her iki küme düğümü yük devretme ekleyin.
2.  Yük devretme kümesini yedeklerken, yük devretme kümesi Yöneticisi'ni kullanarak ayarlayın. Yük Devretme Kümesi Yöneticisi'nde seçin **küme oluşturma**ve ardından yalnızca ilk küme (a düğümü) adını ekleyin. İkinci düğümü henüz eklemeyin; İkinci düğümü bir sonraki adımda ekleyin.

  ![Şekil 8: ilk küme düğümüne sunucu veya sanal makine adını ekleyin][sap-ha-guide-figure-3007]

  _**Şekil 8:** ilk küme düğümüne sunucu veya sanal makine adını ekleyin_

3.  Küme ağ adı (sanal ana bilgisayar adı) girin.

  ![Şekil 9: küme adını girin][sap-ha-guide-figure-3008]

  _**Şekil 9:** küme adını girin_

4.  Kümeyi oluşturduktan sonra bir küme doğrulama testi çalıştırın.

  ![Şekil 10: küme doğrulama denetimini Çalıştır][sap-ha-guide-figure-3009]

  _**Şekil 10:** küme doğrulama denetimini Çalıştır_

  Bu noktada işleminde disklerle ilgili tüm uyarılar yoksayabilirsiniz. Dosya paylaşım tanığı ve SIOS diskler daha sonra paylaşılan ekleyeceksiniz. Bu aşamada, bir çekirdek sahibi olma hakkında endişelenmeniz gerekmez.

  ![Şekil 11: Çekirdek disk bulunamadı][sap-ha-guide-figure-3010]

  _**Şekil 11:** çekirdek disk bulunamadı_

  ![Şekil 12: Bir çekirdek küme kaynağının yeni bir IP adresi gerekiyor.][sap-ha-guide-figure-3011]

  _**Şekil 12:** çekirdek küme kaynağı yeni bir IP adresi gerekiyor_

5.  Çekirdeği Küme hizmetinin IP adresini değiştirin. Çekirdeği Küme hizmeti, IP adresini değiştirme kadar sanal makine düğümlerinden biri için sunucunun IP adresini işaret ettiğinden küme başlatılamaz. Bunu yapmak **özellikleri** çekirdek Küme hizmeti IP kaynak sayfası.

  Örneğin, kimliğinizi (örneğimizde, 10.0.0.42) bir IP adresi atamak küme sanal ana bilgisayar adı pr1-ascs-VIR için gerekiyor.

  ![Şekil 13: Özellikler iletişim kutusunda, IP adresini değiştirme][sap-ha-guide-figure-3012]

  _**Şekil 13:** içinde **özellikleri** iletişim kutusunda, IP adresini değiştirme_

  ![Şekil 14: küme için ayrılmış bir IP adresi atayın][sap-ha-guide-figure-3013]

  _**Şekil 14:** küme için ayrılmış bir IP adresi atayın_

6.  Küme sanal ana bilgisayar adı çevrimiçi duruma getirin.

  ![Şekil 15: Küme çekirdek hazır ve çalışır, doğru IP adresi ile hizmetidir][sap-ha-guide-figure-3014]

  _**Şekil 15:** küme çekirdeği hazır ve çalışır, doğru IP adresi ile hizmetidir_

7.  İkinci küme düğümünü ekleyin.

  Çekirdeği Küme hizmeti çalışır durumda olduğundan, ikinci küme düğümü ekleyebilirsiniz.

  ![İkinci Küme düğüm Şekil 16 Ekle][sap-ha-guide-figure-3015]

  _**Şekil 16:** ikinci küme düğümü ekleyin._

8.  İkinci küme düğümü ana bilgisayar için bir ad girin.

  ![Şekil 17: ikinci küme düğümü ana bilgisayar adı girin][sap-ha-guide-figure-3016]

  _**Şekil 17:** ikinci küme düğümü ana bilgisayar adını girin_

  > [!IMPORTANT]
  > Olduğundan emin olun **tüm uygun depolamayı kümeye eklemek** onay kutusu *değil* seçili.  
  >
  >

  ![Şekil 18: onay kutusunu seçin][sap-ha-guide-figure-3017]

  _**Şekil 18:** yapmak *değil* onay kutusunu seçin_

  Çekirdek ve diskleri ilgili uyarılar yoksayabilirsiniz. Çekirdek ayarlayın ve diski daha sonra açıklandığı şekilde paylaşma [bir SAP ASCS/SCS küme paylaşım diski SIOS DataKeeper Cluster Edition][sap-high-availability-infrastructure-wsfc-shared-disk-install-sios].

  ![Şekil 19: disk çekirdek ilgili uyarılar yoksay][sap-ha-guide-figure-3018]

  _**Şekil 19:** disk çekirdek ilgili uyarılar yoksay_


#### <a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a> Bir küme dosya paylaşımı tanığı Yapılandır

Bir küme dosya paylaşımı tanığı yapılandırma, bu görevleri içerir:

- Bir dosya paylaşımı oluşturun.
- Dosya paylaşım tanığı çekirdek yük devretme kümesi Yöneticisi'nde ayarlayın.

#### <a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a> Dosya paylaşımı oluşturma

1.  Dosya paylaşım tanığı yerine bir çekirdek diski seçin. Bu seçenek SIOS DataKeeper destekler.

  Bu makaledeki örneklerde, Azure'da çalışan Active Directory veya DNS sunucusu dosya paylaşım tanığı açıktır. Dosya paylaşım tanığı domcontr-0 olarak adlandırılır. Active Directory veya DNS hizmetiniz Azure (aracılığıyla, VPN ağ geçidi veya Azure ExpressRoute) bir VPN bağlantısı yapılandırılmış için şirket içi ve dosya paylaşım tanığı çalıştırmak uygun değil.

  > [!NOTE]
  > Yalnızca şirket içi Active Directory veya DNS hizmeti çalışıyorsa, dosya paylaşım tanığı Active Directory veya DNS Windows işletim şirket içi çalışan sistemde yapılandırmayın. Azure ve Active Directory veya DNS şirket içi çalışan küme düğümleri arasındaki ağ gecikmesi çok büyük ve bağlantı sorunlarına neden olabilir. Dosya paylaşım tanığı, küme düğümü yakın çalıştıran Azure sanal makinede yapılandırdığınızdan emin olun.  
  >
  >

  Çekirdek sürücüde en az 1024 MB boş disk alanı gerekir. 2.048 MB boş disk alanı çekirdek sürücüsü için öneririz.

2.  Küme adı nesnesi ekleyin.

  ![Şekil 20: küme adı nesnesi paylaşımı üzerindeki izinleri atama][sap-ha-guide-figure-3019]

  _**Şekil 20:** küme adı nesnesi paylaşımı üzerindeki izinleri atama_

  İzinler (örneğimizde, pr1 ascs VIR$) küme adı nesnesi için paylaşım uygulamasında verileri değiştirme yetkisi eklediğinizden emin olun.

3.  Küme adı nesnesi listesine eklemek için seçin **Ekle**. Şekil 22'de gösterilen ek olarak bilgisayar nesneleri denetlemek için filtreyi değiştirin.

  ![Şekil 21: Bilgisayarları dahil etme değişiklik nesne türleri][sap-ha-guide-figure-3020]

  _**Şekil 21:** değişiklik **nesne türlerini** bilgisayarları dahil etme_

  ![Şekil 22: bilgisayarlar onay kutusunu seçin][sap-ha-guide-figure-3021]

  _**Şekil 22:** seçin **bilgisayarlar** onay kutusu_

4.  Küme adı nesnesi şekil 21'de gösterildiği gibi girin. Kaydı zaten oluşturulduğundan, Şekil 20'de gösterildiği gibi izinlerini değiştirebilirsiniz.

5.  Seçin **güvenlik** paylaşım ayarlayın ve ardından sekmesinde daha ayrıntılı küme adı nesnesi için izinleri.

  ![Şekil 23: dosya paylaşımı çekirdeği küme adı nesnesi için güvenlik özniteliklerini ayarlama][sap-ha-guide-figure-3022]

  _**Şekil 23:** dosya paylaşımı çekirdeği küme adı nesnesi için güvenlik özniteliklerini ayarla_

#### <a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a> Yük Devretme Kümesi Yöneticisi'nde dosya paylaşım tanığı çekirdek ayarlayın

1.  Açık çekirdek Ayarlama Sihirbazı'nı yapılandırın.

  ![Şekil 24: yapılandırma küme çekirdeği Ayarlama Sihirbazı'nı Başlat][sap-ha-guide-figure-3023]

  _**Şekil 24:** yapılandırma küme çekirdeği Ayarlama Sihirbazı'nı Başlat_

2.  Üzerinde **çekirdek yapılandırma seçeneğini** sayfasında **çekirdek tanığı Seç**.

  ![Şekil 25: aralarından seçim yapabileceğiniz Çekirdek yapılandırmaları][sap-ha-guide-figure-3024]

  _**Şekil 25:** seçim yapabileceğiniz Çekirdek yapılandırmaları_

3.  Üzerinde **çekirdek tanığı Seç** sayfasında **bir dosya paylaşımı tanığı Yapılandır**.

  ![Şekil 26: dosya paylaşım tanığı seçin][sap-ha-guide-figure-3025]

  _**Şekil 26:** dosya paylaşım tanığı seçin_

4.  Dosya Paylaşımı için UNC yolunu girin (örneğimizde \\domcontr 0\FSW). Yapabilirsiniz yapılan değişikliklerin bir listesi görmek için seçin **sonraki**.

  ![Şekil 27: Tanık paylaşımı için dosya paylaşım konumunu tanımlayın][sap-ha-guide-figure-3026]

  _**Şekil 27:** Tanık paylaşımı için dosya paylaşım konumunu tanımlayın_

5.  Seçin ve ardından değişiklikleri **sonraki**. Küme yapılandırmasını şekil 28'de gösterildiği gibi başarılı bir şekilde yeniden yapılandırmanız gerekir:  

  ![Şekil 28: küme yeniden yapılandırılması onayı][sap-ha-guide-figure-3027]

  _**Şekil 28:** kümeye yeniden yapılandırılması onayı_

Windows Yük devretme kümesi başarıyla yükledikten sonra Azure koşullar için yük devretme algılama uyarlamak için bazı eşikleri değiştirmeniz gerekir. Değiştirilecek parametreleri belgelenmiştir [yük devretme kümesi ağ eşiklerini ayarlama][tuning-failover-cluster-network-thresholds]. Varsayarak, iki VM oluşturan ASCS/SCS Windows Küme yapılandırması aynı alt ağdaki, bu değerleri aşağıdaki parametreleri değiştirin:

- SameSubNetDelay = 2
- SameSubNetThreshold = 15

Bu ayarlar müşterilerle test edilmiş ve iyi bir güvenlik açığı sunar. Yeterince esnektir, ancak gerçek hata koşullarda bir SAP yazılım veya bir düğüm veya VM hatası yeterince hızlı yük devretme de sağlar.

### <a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a> SAP ASCS/SCS küme paylaşım diski için SIOS DataKeeper küme Edition'ı yükleme

Artık Azure üzerinde çalışan bir Windows Server Yük Devretme Kümelemesi yapılandırma vardır. SAP ASCS/SCS örneği yüklemek için paylaşılan disk kaynağı gerekir. İhtiyacınız olan paylaşılan disk kaynakları Azure'da oluşturulamıyor. Paylaşılan disk kaynakları oluşturmak için kullanabileceğiniz bir üçüncü taraf çözümü SIOS DataKeeper küme sürümüdür.

SAP ASCS/SCS küme paylaşım diski için SIOS DataKeeper Cluster Edition yüklemek, bu görevleri içerir:

- Microsoft .NET Framework 3.5 ekleyin.
- SIOS DataKeeper yükleyin.
- SIOS DataKeeper ayarlayın.

### <a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a> .NET Framework 3.5 ekleme
.NET framework 3.5 otomatik olarak etkinleştirilmiş veya Windows Server 2012 R2'de yüklü değil. SIOS DataKeeper .NET DataKeeper yüklediğiniz tüm düğümlerde olması gerektiğinden, .NET Framework 3.5, kümedeki tüm sanal makineler konuk işletim sisteminde yüklemeniz gerekir.

.NET Framework 3.5 eklemek için iki yolu vardır:

- Ekle roller ve Özellikler Sihirbazı'nı Windows, Şekil 29 gösterildiği gibi kullanın:

  ![Şekil 29: Ekle roller ve Özellikler Sihirbazı'nı kullanarak yükleme .NET Framework 3.5][sap-ha-guide-figure-3028]

  _**Şekil 29:** Ekle roller ve Özellikler Sihirbazı'nı kullanarak yükleme .NET Framework 3.5_

  ![Şekil 30: yükleme ilerleme çubuğu Ekle roller ve Özellikler Sihirbazı'nı kullanarak .NET Framework 3.5 yüklediğinizde][sap-ha-guide-figure-3029]

  _**Şekil 30:** Ekle roller ve Özellikler Sihirbazı'nı kullanarak .NET Framework 3.5 yüklediğinizde çubuğu yükleme ilerleme durumu_

- Dism.exe komut satırı aracını kullanın. Bu yükleme türü için Windows yükleme medyasında bulunan SxS dizin erişmesi gerekir. Yükseltilmiş bir komut isteminde şu komutu girin:

  ```
  Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
  ```

### <a name="dd41d5a2-8083-415b-9878-839652812102"></a> SIOS DataKeeper yükleyin

Kümedeki her düğümde SIOS DataKeeper Cluster Edition yükleyin. SIOS DataKeeper ile sanal paylaşılan depolama alanı oluşturmak için eşitlenen bir yansıtma oluşturmak ve Küme Paylaşılan depolama benzetimini yapma.

SIOS yazılımı yüklemeden önce DataKeeperSvc etki alanı kullanıcısı oluşturun.

> [!NOTE]
> DataKeeperSvc etki alanı kullanıcı her iki küme düğümlerinde yerel yönetici grubuna ekleyin.
>
>

SIOS DataKeeper yüklemek için:

1.  SIOS yazılım her iki küme düğümlerine yükleyin.

  ![SIOS yükleyici][sap-ha-guide-figure-3030]

  ![Şekil 31: İlk sayfa SIOS DataKeeper yükleme][sap-ha-guide-figure-3031]

  _**Şekil 31:** SIOS DataKeeper yüklemesinin ilk sayfa_

2.  İletişim kutusunda **Evet**.

  ![Şekil 32: Bir hizmeti devre dışı bırakılacak DataKeeper size bildirir][sap-ha-guide-figure-3032]

  _**Şekil 32:** DataKeeper bildirir, bir hizmeti devre dışı bırakılacak_

3.  İletişim kutusunda seçtiğiniz olan öneririz **etki alanı veya sunucu hesabı**.

  ![Şekil 33: Kullanıcı Seçimi SIOS DataKeeper için][sap-ha-guide-figure-3033]

  _**Şekil 33:** SIOS DataKeeper için kullanıcı seçimi_

4.  SIOS DataKeeper için oluşturduğunuz parola ve etki alanı hesabı kullanıcı adı girin.

  ![Şekil 34: SIOS DataKeeper yükleme için etki alanı kullanıcı adı ve parolayı girin][sap-ha-guide-figure-3034]

  _**Şekil 34:** SIOS DataKeeper yükleme için etki alanı kullanıcı adı ve parolayı girin_

5.  Lisans anahtarı SIOS DataKeeper Örneğiniz için Şekil 35 gösterildiği gibi yükleyin.

  ![Şekil 35: SIOS DataKeeper lisans anahtarınızı girin][sap-ha-guide-figure-3035]

  _**Şekil 35:** SIOS DataKeeper lisans anahtarınızı girin_

6.  İstendiğinde, sanal makineyi yeniden başlatın.

### <a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a> SIOS DataKeeper ayarlayın

Her iki düğümde SIOS DataKeeper yükledikten sonra yapılandırma başlatın. Yapılandırma amacı, her sanal makineye bağlı ek diskleri arasında zaman uyumlu veri çoğaltma sağlamaktır.

1.  DataKeeper yönetim ve Yapılandırma Aracı'nı başlatın ve ardından **Connect Server**.

  ![Şekil 36: SIOS DataKeeper yönetim ve yapılandırma aracı][sap-ha-guide-figure-3036]

  _**Şekil 36:** SIOS DataKeeper yönetim ve yapılandırma aracı_

2.  Adını veya yönetim ve yapılandırma aracı için ve ikinci adım, ikinci düğümü bağlanmalısınız ilk düğümü TCP/IP'yi adresini girin.

  ![Şekil 37: ad ekleyin veya TCP/IP adresi ilk düğümün yönetim ve yapılandırma aracı için ve ikinci adım, ikinci düğümü bağlanmanız gerekir][sap-ha-guide-figure-3037]

  _**Şekil 37:** adını veya yönetimi ve yapılandırması aracı bağlanmak için ve ikinci adım, ikinci düğümü ilk düğümde TCP/IP'yi adresini Ekle_

3.  İki düğüm arasında çoğaltma işi oluşturun.

  ![Şekil 38: bir çoğaltma işi oluşturma][sap-ha-guide-figure-3038]

  _**Şekil 38:** çoğaltma işi oluşturma_

  Sihirbaz, bir çoğaltma işi oluşturma işleminde size rehberlik eder.

4.  Çoğaltma işi adını tanımlayın.

  ![Şekil 39: çoğaltma işi adını tanımlayın][sap-ha-guide-figure-3039]

  _**Şekil 39:** çoğaltma işi adını tanımlayın_

  ![Şekil 40: geçerli kaynak düğüm olmalıdır düğümü için temel veri tanımlama][sap-ha-guide-figure-3040]

  _**Şekil 40:** geçerli kaynak düğüm olmalıdır düğümü için temel veri tanımlama_

5.  Adı, TCP/IP adresi ve hedef düğüm disk birimi tanımlayın.

  ![Şekil 41: adı, TCP/IP adresi ve geçerli hedef düğüm disk birimi tanımlayın][sap-ha-guide-figure-3041]

  _**Şekil 41:** adı, TCP/IP adresi ve geçerli hedef düğüm disk birimi tanımlayın_

6.  Sıkıştırma algoritmaları tanımlayın. Bizim örneğimizde, çoğaltma akışını sıkıştırmak öneririz. Özellikle yeniden eşitleme durumlarda çoğaltma akışı sıkıştırma yeniden eşitleme süresini önemli ölçüde azaltır. Sıkıştırma, bir sanal makinenin CPU ve RAM kaynakları kullanır. Sıkıştırma oranı arttıkça, birimin kullanılan CPU kaynaklarının da artar. Bu ayarı daha sonra değiştirebilirsiniz.

7.  Kontrol etmeniz başka bir ayar olup çoğaltma zaman uyumsuz veya zaman uyumlu olarak gerçekleşir. SAP ASCS/SCS yapılandırmaları koruduğunuzda, zaman uyumlu çoğaltma kullanmanız gerekir.  

  ![Şekil 42: çoğaltma ayrıntılarını tanımlayın][sap-ha-guide-figure-3042]

  _**Şekil 42:** çoğaltma ayrıntılarını tanımlayın_

8.  Çoğaltma işlemiyle çoğaltılır birimi paylaşılan bir disk olarak Windows Server Yük devretme kümesi yapılandırması için temsil olup olmadığını tanımlar. SAP ASCS/SCS yapılandırmasını seçin **Evet** Windows Küme çoğaltılan birim küme birimi olarak kullanabileceği bir paylaşılan disk olarak görebilmesi için.

  ![Şekil 43: çoğaltılan birim küme birimi olarak ayarlamak için Evet'i seçin][sap-ha-guide-figure-3043]

  _**Şekil 43:** seçin **Evet** çoğaltılan birim küme birimi olarak ayarlamak için_

  Birim oluşturulduktan sonra DataKeeper yönetim ve yapılandırma aracı çoğaltma işi etkin olduğunu gösterir.

  ![Şekil 44: DataKeeper SAP ASCS/SCS paylaşım disk için zaman uyumlu yansıtma etkindir][sap-ha-guide-figure-3044]

  _**Şekil 44:** disk SAP ASCS/SCS paylaşmak için DataKeeper zaman uyumlu yansıtma etkin olduğu_

  Yük Devretme Kümesi Yöneticisi şimdi şekil 45 gösterildiği gibi diski bir DataKeeper disk olarak gösterilmektedir:

  ![Şekil 45: Yük devretme kümesi Yöneticisi'ni DataKeeper çoğaltılan disk gösterir.][sap-ha-guide-figure-3045]

  _**Şekil 45:** yük devretme kümesi Yöneticisi, çoğaltılan bu DataKeeper disk gösterir_

## <a name="next-steps"></a>Sonraki adımlar

* [SAP NetWeaver HA bir Windows Yük devretme kümesi ve paylaşılan disk için bir SAP ASCS/SCS örneği kullanarak yükleme][sap-high-availability-installation-wsfc-shared-disk]
