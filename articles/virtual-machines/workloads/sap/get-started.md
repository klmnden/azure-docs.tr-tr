---
title: Azure vm'lerde SAP kullanmaya başlama | Microsoft Docs
description: Microsoft Azure sanal makineler'de (VM) üzerinde çalışan SAP çözümleri hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: ad8e5c75-0cf6-4564-ae62-ea1246b4e5f2
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/06/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3892b546bb6873a802d85b30cd89801abc9a7424
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162436"
---
# <a name="using-azure-for-hosting-and-running-sap-workload-scenarios"></a>Barındırma ve SAP iş yükü senaryoları çalıştırmak için Azure'ı kullanma
[767598]:https://launchpad.support.sap.com/#/notes/767598
[773830]:https://launchpad.support.sap.com/#/notes/773830
[826037]:https://launchpad.support.sap.com/#/notes/826037
[965908]:https://launchpad.support.sap.com/#/notes/965908
[1031096]:https://launchpad.support.sap.com/#/notes/1031096
[1139904]:https://launchpad.support.sap.com/#/notes/1139904
[1173395]:https://launchpad.support.sap.com/#/notes/1173395
[1245200]:https://launchpad.support.sap.com/#/notes/1245200
[1409604]:https://launchpad.support.sap.com/#/notes/1409604
[1558958]:https://launchpad.support.sap.com/#/notes/1558958
[1585981]:https://launchpad.support.sap.com/#/notes/1585981
[1588316]:https://launchpad.support.sap.com/#/notes/1588316
[1590719]:https://launchpad.support.sap.com/#/notes/1590719
[1597355]:https://launchpad.support.sap.com/#/notes/1597355
[1605680]:https://launchpad.support.sap.com/#/notes/1605680
[1619720]:https://launchpad.support.sap.com/#/notes/1619720
[1619726]:https://launchpad.support.sap.com/#/notes/1619726
[1619967]:https://launchpad.support.sap.com/#/notes/1619967
[1750510]:https://launchpad.support.sap.com/#/notes/1750510
[1752266]:https://launchpad.support.sap.com/#/notes/1752266
[1757924]:https://launchpad.support.sap.com/#/notes/1757924
[1757928]:https://launchpad.support.sap.com/#/notes/1757928
[1758182]:https://launchpad.support.sap.com/#/notes/1758182
[1758496]:https://launchpad.support.sap.com/#/notes/1758496
[1772688]:https://launchpad.support.sap.com/#/notes/1772688
[1814258]:https://launchpad.support.sap.com/#/notes/1814258
[1882376]:https://launchpad.support.sap.com/#/notes/1882376
[1909114]:https://launchpad.support.sap.com/#/notes/1909114
[1922555]:https://launchpad.support.sap.com/#/notes/1922555
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1941500]:https://launchpad.support.sap.com/#/notes/1941500
[1956005]:https://launchpad.support.sap.com/#/notes/1956005
[1973241]:https://launchpad.support.sap.com/#/notes/1973241
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2039619]:https://launchpad.support.sap.com/#/notes/2039619
[2121797]:https://launchpad.support.sap.com/#/notes/2121797
[2134316]:https://launchpad.support.sap.com/#/notes/2134316
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

[azure-cli]:../../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md#subscription

[dbms-guide]:dbms-guide.md
[dbms-guide-2.1]:dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f
[dbms-guide-2.2]:dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91
[dbms-guide-2.3]:dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152
[dbms-guide-2]:dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64
[dbms-guide-3]:dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3
[dbms-guide-5.5.1]:dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268
[dbms-guide-5.5.2]:dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b
[dbms-guide-5.6]:dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8
[dbms-guide-5.8]:dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30
[dbms-guide-5]:dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737
[dbms-guide-8.4.1]:dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573
[dbms-guide-8.4.2]:dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d
[dbms-guide-8.4.3]:dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c
[dbms-guide-8.4.4]:dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818
[dbms-guide-900-sap-cache-server-on-premises]:dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:deployment-guide.md
[deployment-guide-2.2]:deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94
[deployment-guide-3.1.2]:deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab
[deployment-guide-3.2]:deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281
[deployment-guide-3.3]:deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2
[deployment-guide-3.4]:deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e
[deployment-guide-4.1]:deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7
[deployment-guide-4.2]:deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e
[deployment-guide-4.3]:deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc
[deployment-guide-4.4.2]:deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542
[deployment-guide-4.4]:deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d
[deployment-guide-4.5.1]:deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4
[deployment-guide-4.5.2]:deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f
[deployment-guide-4.5]:deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca
[deployment-guide-5.1]:deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2
[deployment-guide-5.2]:deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1
[deployment-guide-5.3]:deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8
[deployment-guide-configure-monitoring-scenario-1]:deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b
[deployment-guide-configure-proxy]:deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d
[deployment-guide-figure-100]:media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:deployment-guide.md#figure-11
[deployment-guide-figure-1100]:media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:deployment-guide.md#figure-14
[deployment-guide-figure-1400]:media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:deployment-guide.md#figure-5
[deployment-guide-figure-50]:media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:deployment-guide.md#figure-6
[deployment-guide-figure-600]:media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:deployment-guide.md#figure-7
[deployment-guide-figure-700]:media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b
[deploy-template-cli]:../../../resource-group-template-deploy.md
[deploy-template-portal]:../../../resource-group-template-deploy.md
[deploy-template-powershell]:../../../resource-group-template-deploy.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md
[getting-started-dbms]:get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c


[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056
[ha-guide]:sap-high-availability-guide.md
[ha-guide-get-started]:sap-high-availability-guide-start.md

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:planning-guide.md
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff
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
[planning-guide-figure-2500]:media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[powershell-install-configure]:https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../../networking/networking-overview.md
[sap-pam]:https://support.sap.com/pam (SAP ürün kullanılabilirliği Matrisi)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]:../../../storage/common/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../../../storage/common/storage-azure-cli.md#copy-blobs
[storage-introduction]:../../../storage/common/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../../../storage/common/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../../windows/premium-storage.md
[storage-redundancy]:../../../storage/common/storage-redundancy.md
[storage-scalability-targets]:../../../storage/common/storage-scalability-targets.md
[storage-use-azcopy]:../../../storage/common/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:../../linux/attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-linux-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md
[virtual-machines-linux-agent-user-guide]:../../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:../../linux/capture-image.md#capture-the-vm
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability]:../../linux/manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-create-powershell.md
[virtual-machines-sizes]:../../linux/sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:./../../windows/sql/virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:./../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:./../../windows/sql/virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:../../virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:../../virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../linux/multiple-nics.md
[virtual-network-deploy-multinic-arm-ps]:../windows/multiple-nics.md
[virtual-network-deploy-multinic-arm-template]:../../../virtual-network/template-samples.md
[virtual-networks-configure-vnet-to-vnet-connection]:../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../../../virtual-network/manage-virtual-network.md#create-a-virtual-network
[virtual-networks-manage-dns-in-vnet]:../../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../../../virtual-network/virtual-network-deploy-multinic-classic-ps.md
[virtual-networks-nsg]:../../../virtual-network/security-overview.md
[virtual-networks-reserved-private-ip]:../../../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../../../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../../../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../../../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../../../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../../../vpn-gateway/vpn-gateway-site-to-site-create.md
[vpn-gateway-vpn-faq]:../../../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../../../cli-install-nodejs.md
[xplat-cli-azure-resource-manager]:../../../xplat-cli-azure-resource-manager.md

Microsoft Azure, SAP kullanılmaya hazır bir bulut iş ortağı olarak seçerek, senaryoları ve görev açısından kritik SAP iş yüklerini ölçeklenebilir, uyumlu ve kurumsal düzeyde kendini kanıtlamış platformunda güvenilir bir şekilde çalıştırmak kullanabilirsiniz.  Azure’un sunduğu ölçeklenebilirlik, esneklik ve maliyet tasarrufu olanaklarından yararlanın. Microsoft ve SAP arasındaki Genişletilmiş iş ortaklığı ile SAP uygulama yelpazesini - azure'da geliştirme/test ve üretim senaryolarında çalıştırabilir ve tam olarak desteklenir. SAP Netweaver'dan SAP S4/HANA, SAP BI, Linux için Windows, SAP HANA'dan SQL'e, karşılıyoruz.

SAP NetWeaver farklı DBMS Azure senaryolarla barındırma yanı sıra, farklı barındırabilirsiniz Azure üzerinde SAP BI gibi diğer SAP iş yükü senaryoları. Azure yerel sanal makinelerinde SAP NetWeaver dağıtımları ile ilgili belgeler, "Azure sanal makinelerinde SAP NetWeaver." bölümünde bulunabilir

Azure'da SAP HANA yararlanır SAP iş yükü karşılamak için CPU ve bellek kaynaklarının boyutu hiç olmadığı kadar büyüyor yerel Azure sanal makine teklifler bulunur. Bu alanda daha fazla bilgi için Azure sanal Makineler'de SAP HANA bölümünde belgeleri arayın."

Benzersiz SAP HANA için Azure, Azure dışında yarışma ayarlar benzersiz bir tekliftir. Daha fazla bellek ve CPU kaynağı zorlu SAP HANA, Azure teklifleri içeren SAP senaryoları barındırma müşteri kullanımını etkinleştirmek için 20 TB'a kadar (60 TB ölçek genişletme) için bellek gerektiren SAP HANA dağıtımlarını yürüten amacıyla çıplak bilgisayar donanım ayrılmış. S/4HANA veya diğer SAP HANA iş yükü. Bu benzersiz Azure çözüm SAP hana (büyük örnekler) Azure üzerinde SAP HANA SAP uygulama katmanında veya yerel Azure sanal Makineler'de barındırılan iş yükü donanımlar orta katman ile özel bir çıplak bilgisayar donanım çalıştırmanıza olanak tanır. Bu çözüm, çeşitli belgelerde "SAP HANA (büyük örnekler) azure'da." bölümünde belgelenmiştir   

Azure'da SAP iş yükü senaryoları barındırma kimlik tümleştirmesi ve çoklu oturum açma Azure Activity Directory farklı SAP bileşenleri ile SAP SaaS gereksinimlerini de oluşturabilir veya PaaS sunar. Söz konusu tümleştirmesi ve çoklu oturum açma senaryoları ile Azure Active Directory (AAD) ve SAP varlık listesini açıklanmış ve bölümünde belgelenen "AAD SAP kimlik tümleştirmesi ve çoklu oturum açma."

## <a name="latest-changes"></a>En son değişiklikleri

SAP HANA dinamik Azure Vm'leri için katmanlama etrafında belgeleri

- [Azure'da SAP HANA altyapısı yapılandırmaları ve işlemleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations#sap-hana-dynamic-tiering-20-for-azure-virtual-machines)

Azure sanal makine M128s üzerinde SAP HANA ölçeği genişletilmiş etrafında belgeleri eklenen:

- [Azure'da SAP HANA altyapısı yapılandırmaları ve işlemleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations#configuring-azure-infrastructure-for-sap-hana-scale-out)
- [Bir Azure bölgesi içinde SAP HANA kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-one-region)


## <a name="sap-hana-on-sap-hana-on-azure-large-instances"></a>SAP HANA (büyük örnekler) Azure üzerinde SAP HANA

Bir dizi belgeleri (büyük örnekler) Azure üzerinde SAP HANA müşteri adayları veya, HANA büyük örnekleri kısa. Belgeler, HANA büyük örnekleri listelenen alanları kapsar:

- [SAP HANA (büyük örnekler) azure'da genel bakış](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [SAP hana (büyük örnekler) azure'da bir mimari](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-architecture)
- [Altyapı ve SAP HANA (büyük örnekler) azure'da bağlantısı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-infrastructure-connectivity)
- [SAP HANA (büyük örnekler) Azure üzerinde SAP HANA yükleyin](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-installation)
- [Yüksek kullanılabilirlik ve olağanüstü durum kurtarma (büyük örnekler) Azure üzerinde SAP hana](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery)
- [Sorun giderme ve SAP hana (büyük örnekler) azure'da izleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/troubleshooting-monitoring)

Sonraki adımlar:

- Okuma [genel bakış ve SAP hana (büyük örnekler) azure'da bir mimari](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)



## <a name="sap-hana-on-azure-virtual-machines"></a>Azure Sanal Makinelerde SAP HANA

### <a name="getting-started-with-sap-hana-on-azure"></a>Azure'da SAP HANA ile çalışmaya başlama
Başlık: Azure vm'lerde SAP hana el ile yükleme Hızlı Başlangıç Kılavuzu

Özet: Bu Hızlı Başlangıç Kılavuzu, bir tek örnek SAP HANA sistemi SAP NetWeaver 7.5 ve SAP HANA SP12 Azure vm'lerde el ile yükleme tarafından ayarlamak için yardımcı olur. Kılavuzu okuyucu Azure Iaas temel bilgileri gibi sanal makine veya sanal ağlar Azure portal veya Powershell/CLI gibi json şablonları kullanma seçeneği aracılığıyla dağıtma hakkında bilgi sahibi olduğunu varsayar. Ayrıca okuyucu SAP HANA, SAP NetWeaver ve şirket içi yükleme ile ilgili bilgi sahibi olduğunu beklenmektedir.


[Bu kılavuza buradan ulaşabilirsiniz](hana-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="s4hana-sap-cal-deployment-on-azure"></a>S/4hana'yı Azure üzerindeki SAP CAL dağıtım
Başlık: SAP S/4HANA veya BW/4hana'yı azure'da dağıtın

Özet: Bu kılavuz, SAP S/4HANA, SAP Cloud Appliance Library kullanarak azure'da dağıtımını göstermek için yardımcı olur. SAP Cloud Appliance Library, Azure üzerinde SAP uygulamaları dağıtmanıza olanak tanıyan SAP tarafından bir hizmettir. Kılavuzda, dağıtım adım adım açıklanmaktadır.


[Bu kılavuza buradan ulaşabilirsiniz](cal-s4h.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="high-availability-of-sap-hana-in-azure-virtual-machines"></a>Azure sanal makineler'de SAP hana yüksek kullanılabilirlik
Başlık: Azure sanal makineler'de SAP hana yüksek kullanılabilirlik

Özet: Bu kılavuzda, HANA sistem çoğaltma otomatik yük devretme ile uyum sağlamak için SUSE 12 işletim sistemi ve SAP HANA yüksek kullanılabilirlik yapılandırması üzerinden yol gösterir. Kılavuzu, SUSE ve Azure sanal makineler için özeldir. Kılavuzu henüz Red Hat veya çıplak veya özel Bulut veya diğer Azure dışı genel bulut dağıtımları geçerli değildir.



[Bu kılavuza buradan ulaşabilirsiniz](sap-hana-high-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="sap-hana-backup-overview-on-azure-vms"></a>Azure vm'lerde SAP HANA yedeklemesine genel bakış
Başlık: Azure sanal Makineler'de SAP HANA yedekleme Kılavuzu

Özet: Bu kılavuzda, Azure sanal Makineler'de SAP HANA çalıştırmayı yedekleme olanaklar hakkında temel bilgiler sağlanır.



[Bu kılavuza buradan ulaşabilirsiniz](sap-hana-backup-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="sap-hana-file-level-backup-on-azure-vms"></a>Azure vm'lerde SAP HANA dosya düzeyi yedekleme
Başlık: depolama anlık görüntülerine dayalı SAP HANA yedeklemesi

Özet: Bu kılavuzda, Azure sanal Makineler'de SAP HANA çalıştırırken anlık görüntü tabanlı yedeklemelerin Azure Vm'lerinde kullanma hakkında bilgi sağlanır.



[Bu kılavuza buradan ulaşabilirsiniz](sap-hana-backup-file-level.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


### <a name="sap-hana-snapshot-based-backups-on-azure-vms"></a>Azure vm'lerde SAP HANA tabanlı anlık görüntü yedeklemeleri
Başlık: SAP HANA dosya düzeyi Azure yedekleme

Özet: Bu kılavuzda Azure sanal Makineler'de SAP HANA çalıştırmayı SAP HANA dosya düzeyi yedekleme kullanma hakkında bilgi sağlar.



[Bu kılavuza buradan ulaşabilirsiniz](sap-hana-backup-storage-snapshots.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


## <a name="sap-netweaver-deployed-on-azure-virtual-machines"></a>Dağıtılan Azure sanal Makineler'de SAP NetWeaver

### <a name="deploy-sap-ides-system-on-windows-and-sql-server-through-sap-cal-on-azure"></a>Windows ve SQL Server ile Azure üzerinde SAP CAL üzerinde SAP IDES sistemi dağıtma
Başlık: Microsoft Azure SUSE Linux vm'lerde SAP NetWeaver test etme

Özet: Bu belgede, Windows ve SQL Server SAP Cloud Appliance Library kullanarak azure'da temel bir SAP IDES sisteminin dağıtımı açıklanmaktadır. SAP bulut Gereci kitaplığı Azure üzerinde SAP ürünleri dağıtımını izin veren bir SAP hizmetidir. Bu belgede, adım adım bir SAP IDES sisteminin dağıtımı gider. Yalnızca Microsoft Azure üzerinde SAP bulut Gereci aracılığıyla dağıtılan birden fazla diğer düzine uygulamalar için örnek IDE'ler sistemidir.


[Bu kılavuza buradan ulaşabilirsiniz](cal-ides-erp6-erp7-sp3-sql.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


### <a name="quickstart-guide-for-netweaver-on-suse-linux-on-azure"></a>Azure'da SUSE Linux'ta NetWeaver için Hızlı Başlangıç Kılavuzu
Başlık: Microsoft Azure SUSE Linux vm'lerde SAP NetWeaver test etme

Özet: Bu makalede, Microsoft Azure SUSE Linux sanal makinelerinde (VM'ler) SAP NetWeaver çalıştırırken dikkate alınması gereken çeşitli şeyler açıklanmaktadır. Resmi Azure SUSE Linux Vm'lerde SAP NetWeaver desteklenir. Linux sürümleri, SAP çekirdek sürümlerini ve diğer ayrıntıları ile ilgili tüm ayrıntıları SAP notu 1928533 bulunabilir "azure'da SAP uygulamaları: desteklenen ürünler ve Azure VM türleri".


[Bu kılavuza buradan ulaşabilirsiniz](suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="3da0389e-708b-4e82-b2a2-e92f132df89c"></a>Planlama ve uygulama
Başlık: Azure sanal makineleri planlama ve uygulama için SAP NetWeaver

Özet: Azure sanal Makineler'de SAP NetWeaver çalıştırma hakkında düşünmek, bu belge ile başlaması kılavuzudur. Bu planlama ve Uygulama Kılavuzu bir mevcut veya planlanan SAP NetWeaver tabanlı sisteminizin bir Azure sanal makineleri ortamına dağıtılıp dağıtılamayacağını değerlendirmenize yardımcı olur. İncelemede birden fazla SAP NetWeaver dağıtım senaryosuna ek olarak Azure’a özgü SAP yapılandırmalarına yer verilmiştir. Kağıt listeler ve size gereken tüm gerekli yapılandırma bilgileri karma SAP ortamı çalıştırmak için SAP/Azure tarafında açıklar. IaaS üzerindeki SAP NetWeaver tabanlı sistemlerde yüksek kullanılabilirliği sağlamanız için gerekli önlemler de incelenmiştir.



[Bu kılavuza buradan ulaşabilirsiniz][planning-guide]

### <a name="high-availability-configurations-of-sap-netweaver-in-azure-vms"></a>Azure vm'lerde SAP NetWeaver'ın yüksek kullanılabilirlik yapılandırmaları
Başlık: Azure sanal makinelerinde SAP NetWeaver için yüksek kullanılabilirlik

Özet: Bu belgede, Azure Resource Manager dağıtım modelini kullanarak yüksek kullanılabilirlik SAP sistemlerini azure'a dağıtmak için attığınız adımlar biz karşılarız. Önemli görevleri inceleyeceğiz. Belgede, biz nasıl tek---bir hata noktası bileşenleri gibi gelişmiş iş uygulaması programlama (ABAP) SAP Central Hizmetleri (ASCS) açıklayan / SAP gibi SAP Central Hizmetleri (SCS) ve veritabanı yönetim sistemi (DBMS) ve yedekli bileşenler Uygulama sunucusu, Azure Vm'lerinde çalışan korunacak seçeceğiz. Yükleme ve yüksek kullanılabilirlik SAP sistemine bir Windows Server Yük Devretme Kümelemesi küme ve azure'daki SUSE Linux Enterprise Server küme çerçevesi yapılandırmasını adım adım bir örnek gösterilmiştir ve bu belgede gösterilen.



[Bu kılavuza buradan ulaşabilirsiniz][ha-guide-get-started]

### <a name="realizing-multi-sid-deployments-of-sap-netweaver-in-azure-vms"></a>Azure vm'lerde SAP NetWeaver gerçekleştirilmiş çoklu SID dağıtımları
Başlık: bir SAP NetWeaver çoklu SID yapılandırması oluştur

Özet: Bu belgede Azure vm'lerinde SAP NetWeaver için yüksek kullanılabilirlik belgeye bir ektir. Eylül 2016'da tanıtılan azure'da yeni işlevsellik nedeniyle, birden çok SAP NetWeaver ASCS/SCS örneği bir çift Azure Vm'leri dağıtmak mümkündür. Bu tür bir yapılandırma ile yüksek oranda kullanılabilir SAP NetWeaver yapılandırmaları faydalanmanın dağıtmak için gereken VM sayısını azaltabilirsiniz. Kılavuzu gibi çoklu SID yapılandırmaları kurulumunu açıklar.



[Bu kılavuza buradan ulaşabilirsiniz](high-availability-multi-sid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="6aadadd2-76b5-46d8-8713-e8d63630e955"></a>Azure vm'lerde SAP NetWeaver'ın dağıtımı
Başlık: Azure sanal makineler dağıtım için SAP NetWeaver

Özet: Bu belgede Azure’daki sanal makinelerde SAP NetWeaver yazılımlarını dağıtmak için gerekli yordam bilgileri verilmiştir. Bu incelemede yer alan üç belirli dağıtım senaryosunda SAP için Azure İnceleme Uzantılarını etkinleştirme talimatlarına ve SAP için Azure İzleme Uzantıları sorun giderme önerilerine yer verilmiştir. Bu incelemede planlama ve uygulama kılavuzunu okuduğunuz varsayılmaktadır.



[Bu kılavuza buradan ulaşabilirsiniz][deployment-guide]

### <a name="1343ffe1-8021-4ce6-a08d-3a1553a4db82"></a>DBMS Dağıtım Kılavuzu
Başlık: Azure sanal makineleri DBMS dağıtım için SAP NetWeaver

Özet: Bu incelemede SAP ile birlikte çalışması gereken DBMS sistemleri için planlama ve uygulama bilgilerine yer verilmiştir. İlk bölümde genel konular listelenmiş ve sunulmuştur. Sonraki bölümler ise Azure’da SAP tarafından desteklenen farklı DBMS dağıtımları ele alınmıştır. SQL Server, SAP ASE ve Oracle yer verilen DBMS var. Bu belirli bölümlerinde, SAP sistemlerini Azure'a DBMS ile birlikte çalıştırılıyorsa için hesabına sahip konuları ele alınmıştır. Farklı DBMS çözümlerinin Azure’da desteklediği yedekleme ve yüksek kullanılabilirlik yöntemleri gibi konular, SAP uygulamalarının kullanımıyla bağlantılı olarak incelenmiştir.



[Bu kılavuza buradan ulaşabilirsiniz][dbms-guide]

### <a name="using-azure-site-recovery-for-sap-workload"></a>Azure Site RECOVERY'yi kullanarak SAP iş yükü için
Başlık: SAP NetWeaver: Azure Site Recovery ile bir olağanüstü durum kurtarma çözümü oluşturma

Özet: Bu belgede, olağanüstü durum kurtarma senaryolarını işleme amacıyla Azure Site Recovery hizmetleri nasıl kullanılabileceğini biçimini tanımlar. Azure Site Recovery Services ile bir şirket içi SAP ortamı için Azure olağanüstü durum kurtarma konumunuz olarak kullanıldığı durumlarda. Başka bir senaryo belgede açıklanan Aure Azure (A2A) olağanüstü durum kurtarma çalışması ve Azure Site Recovery ile nasıl yönetilir ' dir.  



[Bu kılavuza buradan ulaşabilirsiniz](http://aka.ms/asr-sap)
