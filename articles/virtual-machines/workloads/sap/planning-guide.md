---
title: "Azure sanal makineleri planlama ve uygulama için SAP NetWeaver | Microsoft Docs"
description: "Azure sanal makineleri planlama ve uygulama SAP NetWeaver için"
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: d7c59cc1-b2d0-4d90-9126-628f9c7a5538
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/08/2016
ms.author: sedusch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 39b5c70c8740bc06beded42e9066e3be196741a1
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="azure-virtual-machines-planning-and-implementation-for-sap-netweaver"></a>Azure sanal makineleri planlama ve uygulama SAP NetWeaver için
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
[2069760]:https://launchpad.support.sap.com/#/notes/2069760
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
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md#subscription-limits

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

[deploy-template-cli]:../../../resource-group-template-deploy-cli.md
[deploy-template-portal]:../../../resource-group-template-deploy-portal.md
[deploy-template-powershell]:../../../resource-group-template-deploy.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md
[getting-started-dbms]:get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:../../virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:../../virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:../../virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:../../virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:../../virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:../../virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

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

[powershell-install-configure]:https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]:../../../storage/common/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../../../storage/common/storage-azure-cli.md#copy-blobs
[storage-introduction]:../../../storage/common/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../../../storage/common/storage-powershell-guide-full.md
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
[virtual-machines-linux-capture-image-resource-manager-capture]:../../linux/capture-image.md#step-2-capture-the-vm
[virtual-machines-windows-capture-image-resource-manager]:../../windows/capture-image.md
[virtual-machines-windows-capture-image]:../../virtual-machines-windows-generalize-vhd.md
[virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture]:../../virtual-machines-windows-generalize-vhd.md
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-create-upload-vhd-oracle]:../../linux/oracle-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability]:../../linux/manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-create-powershell.md
[virtual-machines-sizes-linux]:../../linux/sizes.md
[virtual-machines-sizes-windows]:../../windows/sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:./../../windows/sql/virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:./../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:./../../windows/sql/virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:../../virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:../../virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../../linux/multiple-nics.md
[virtual-network-deploy-multinic-arm-ps]:../../windows/multiple-nics.md
[virtual-network-deploy-multinic-arm-template]:../../../virtual-network/virtual-network-deploy-multinic-arm-template.md
[virtual-networks-configure-vnet-to-vnet-connection]:../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md
[virtual-networks-manage-dns-in-vnet]:../../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics-windows]:../../windows/multiple-nics.md
[virtual-networks-multiple-nics-linux]:../../linux/multiple-nics.md
[virtual-networks-nsg]:../../../virtual-network/virtual-networks-nsg.md
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
[capture-image-linux-step-2-create-vm-image]:../../linux/capture-image.md#step-2-create-vm-image

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Microsoft Azure uzun tedarik döngüleri olmadan en az sürede işlem ve depolama kaynaklarını edinmeye şirketler sağlar. Azure sanal makineler SAP NetWeaver Azure uygulamalara dayalı gibi Klasik uygulamaları dağıtmak şirketlerin izin ve başka kaynaklar kullanılabilir şirket içi gerek kalmadan, güvenilirlik ve kullanılabilirlik genişletir. Azure sanal makine Hizmetleri şirketlerin etkin olarak Azure sanal makineler kendi şirket içi etki alanlarına, kendi özel Bulutlar ve bunların SAP sistem yatay tümleştirmenize olanak tanır şirket içi bağlantılar da destekler.
Bu teknik incelemede Microsoft Azure sanal makinesi temelleri açıklanır ve Azure SAP NetWeaver yüklemeleri için planlama ve uygulama ile ilgili bir kılavuz sağlar ve bu nedenle Azure üzerinde SAP NetWeaver gerçek dağıtımlarına başlamadan önce okumak için belge olmalıdır.
Kağıt yüklemeleri ve SAP yazılım dağıtımları için birincil kaynaklar üzerinde platformları temsil eden SAP notlar ve SAP yükleme belgelerini tamamlar.

## <a name="summary"></a>Özet
Bulut bilgi işlem, BT endüstri içinde daha da fazla önem büyük ve çokuluslu şirketler kadar küçük şirketlerden elde yaygın olarak kullanılan bir terimdir.

Microsoft Azure paylaşılabilen çok sayıda yeni olanakları sunan Microsoft bulut Hizmetleri platformudur. Teknik veya bütçeleme kısıtlamaları sınırlı değildir şimdi müşterileri için hızlı sağlama ve devre dışı bırakma sağlama uygulamaları bulutta bir hizmet olarak olduğundan. Donanım altyapısını zamandan ve yatırım yapmak yerine, şirket uygulama, iş süreçlerini ve onun avantajlarını müşteriler ve kullanıcılar için odaklanabilirsiniz.

Microsoft, Microsoft Azure Sanal Makine Hizmetleri ile kapsamlı bir Hizmet Olarak Altyapı (IaaS) platformu sunmaktadır. SAP NetWeaver tabanlı uygulamalar, Azure Sanal Makinelerinde (IaaS) desteklenmektedir. Bu teknik inceleme, planlamanızı ve Microsoft Azure içindeki SAP NetWeaver tabanlı uygulamalardan seçim platform olarak açıklar.

Kağıt iki ana yönlere odaklanır:

* İlk bölümü Azure üzerinde SAP NetWeaver tabanlı uygulamalar için iki desteklenen dağıtım desenleri açıklar. Ayrıca, Azure genel işleme aklınızda SAP dağıtımlarla anlatır.
* İkinci bölümü, ilk bölümünde açıklanan iki farklı senaryolar uygulama ayrıntıları.

Ek kaynaklar için bölüm bkz [kaynakları] [ planning-guide-1.2] bu belgedeki.

### <a name="definitions-upfront"></a>Önceden tanımları
Belge boyunca aşağıdaki terimleri kullanırız:

* Iaas: Hizmet olarak altyapı
* PaaS: Hizmet olarak Platform
* SaaS: Hizmet olarak yazılım
* ARM: Azure Kaynak Yöneticisi
* SAP bileşen: tek tek SAP gibi bir uygulama ECC, bant genişliği, çözüm Yöneticisi veya EP'deki  SAP bileşenleri geleneksel ABAP veya Java teknolojiler ya da olmayan-tabanlı NetWeaver uygulama iş nesneleri gibi temel alabilir.
* SAP ortamı: bir veya daha fazla SAP bileşenleri geliştirme, QAS, eğitim, DR veya üretim gibi işletme işlevini gerçekleştirmek için mantıksal olarak gruplandırılır.
* SAP yatay: Bu tüm müşteri'nin SAP varlıkları başvurduğu BT yatay. SAP yatay tüm üretim ve üretim dışı ortamlar içerir.
* SAP sistem: Birleşimi DBMS katman ve uygulama katmanı, örneğin, bir SAP ERP geliştirme sistemi, SAP BW test sistemini, SAP CRM üretim sistem vb. Azure dağıtımlarda, bu iki katmanı şirket içi ve Azure arasında bölmek için desteklenmiyor. Şirket içi SAP sistem ya da bu anlamına gelir dağıtılan veya Azure'da dağıtılır. Ancak, Azure veya şirket içi SAP yatay farklı sistemleri dağıtabilirsiniz. Örneğin, SAP CRM geliştirme dağıtmak ve Azure ancak SAP CRM üretim sistem şirket içi sistemleri test.
* Yalnızca bulut dağıtım: Burada Azure aboneliğine bağlı bir siteden siteye veya şirket içi ağ altyapısı için ExpressRoute bağlantısı aracılığıyla bir dağıtım. Ortak Azure belgelerine bu tür dağıtımlar da "Yalnızca bulut" dağıtımları açıklanmıştır. Bu yöntem ile dağıtılan sanal makineleri, internet ve genel bir IP adresi ve/veya azure'da VM için atanan ortak bir DNS adı aracılığıyla erişilir. Microsoft Windows Şirket içi Active Directory (AD) ve DNS Azure dağıtımları bu tür için genişletilmiş değil. Bu nedenle sanal makineleri şirket içi Active Directory'nin parçası değildir. Aynı, örneğin, OpenLDAP + kullanarak Kerberos Linux uygulamaları için geçerlidir.

> [!NOTE]
> Bu belgede yalnızca bulut dağıtımında tam SAP Windows'un özel olarak Azure Active Directory uzantısız çalışıyor olarak tanımlanan / OpenLDAP veya şirket içi genel Buluta ad çözümlemesi. Yalnızca bulut yapılandırmaları, üretim SAP sistemleri veya burada Azure ve şirket içi bulunan kaynaklar üzerinde barındırılan SAP sistemleri arasında kullanılacak SAP STMS veya diğer şirket kaynaklarına gereken yapılandırmalar için desteklenmez.
>
>

* Şirket içi: siteden siteye, çok siteli veya şirket içi inizdeki ve Azure arasında ExpressRoute bağlantısı bulunan bir Azure aboneliği için sanal makineleri dağıtıldığı bir senaryo açıklanmaktadır. Ortak Azure belgelerine dağıtımları bu tür şirket içi senaryoları açıklanmıştır. Bağlantı için şirket içi etki alanları, şirket içi Active Directory/OpenLDAP ve şirket içi DNS Azure'da genişletmek için nedenidir. Şirket içi yatay abonelik Azure varlıklar için genişletilir. Bu uzantı, sanal makineleri şirket içi etki alanının parçası olabilir. Şirket içi etki alanının etki alanı kullanıcıları sunucularına erişebilir ve hizmetler (ör. DBMS Hizmetleri) bu sanal makineleri üzerinde çalışabilir. Dağıtılan VM'ler şirket içi ve Azure dağıtılan VM'ler arasında iletişim ve ad çözümleme mümkündür. Bu, dağıtılacak çoğu SAP varlıklar bekliyoruz senaryodur. Daha fazla bilgi için bkz: [bu] [ vpn-gateway-cross-premises-options] makale ve [bu][vpn-gateway-site-to-site-create].

> [!NOTE]
> Şirket içi dağıtımları SAP sistemleri çalışan Azure sanal makineleri bir şirket içi etki alanının üyesi olduğu SAP sistemlerinin üretim SAP sistemleri için desteklenir. Şirketler arası yapılandırmalar bölümleri dağıtmak için desteklenen veya Azure'da SAP Windows'un tamamlayın. Bile tam SAP yatay Azure'da çalışan, şirket içi etki alanı ve REKLAM/OpenLDAP parçası olan bu VM'lerin olması gerektirir. Belge önceki sürümlerinde, biz karma BT senaryolar hakkında açıklandı burada terimi *karma* bir şirket içi bağlantılar şirket içi ve Azure arasında olduğunu aslında kökü belirtilmiş. Ayrıca, Azure sanal makineleri şirket içi Active Directory'nin parçası bulunmasına / OpenLDAP.
>
>

Bazı Microsoft belgeleri şirketler arası senaryoları özellikle DBMS HA yapılandırmaları için biraz farklı bir şekilde açıklanmıştır. SAP ilgili belgeler söz konusu olduğunda, şirket içi senaryo yalnızca bir site siteye veya özel (ExpressRoute) bağlantısını ve şirket içi ve Azure arasında SAP yatay dağıtıldığını olgu zorunda boils.  

### <a name="e55d1e22-c2c8-460b-9897-64622a34fdff"></a>Kaynakları
Aşağıdaki ek kılavuzlar, SAP dağıtımları Azure ile ilgili konu için kullanılabilir:

* [Azure sanal makineleri planlama ve uygulama SAP NetWeaver (Bu belgenin) için][planning-guide]
* [SAP NetWeaver için Azure sanal makineler dağıtımı][deployment-guide]
* [SAP NetWeaver için Azure sanal makineleri DBMS dağıtımı][dbms-guide]

> [!IMPORTANT]
> Olası başvuran SAP Yükleme Kılavuzu'na bir bağlantı yerde kullanılan (başvuru InstGuide-01, bkz: <http://service.sap.com/instguides>). Önkoşullar ve yükleme işlemini geldiğinde, bu belgede yalnızca bir Microsoft Azure sanal makinede yüklü SAP NetWeaver sistemler için belirli görevler kapsadığından SAP NetWeaver yükleme kılavuzları her zaman dikkatlice okunmalıdır.
>
>

Azure üzerinde SAP konu aşağıdaki SAP notları ilişkili:

| Not numarası | Başlık |
| --- | --- |
| [1928533] |Azure üzerinde SAP uygulamaları: desteklenen ürünler ve boyutlandırma |
| [2015553] |Microsoft Azure üzerinde SAP: Destek önkoşulları |
| [1999351] |Gelişmiş Azure için SAP izleme sorunlarını giderme |
| [2178632] |Microsoft Azure üzerinde SAP için ölçümleri izleme anahtarı |
| [1409604] |Windows sanallaştırma: İzleme Gelişmiş |
| [2191498] |Azure ile Linux üzerinde SAP: İzleme Gelişmiş |
| [2243692] |Microsoft Azure (Iaas) VM Linux'ta: SAP lisans sorunları |
| [1984787] |SUSE LINUX Enterprise Server 12: Yükleme notları |
| [2002167] |Red Hat Enterprise Linux 7.x: yükleme ve yükseltme |
| [2069760] |Oracle Linux 7.x SAP yükleme ve yükseltme |
| [1597355] |Linux takas alanı önerisi |

Ayrıca okuma [SCN Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) , Linux için tüm SAP notlar içerir.

Genel varsayılan sınırlamalar ve Azure abonelikleri maksimum sınırlamaları bulunabilir [bu makalede][azure-subscription-service-limits-subscription].

## <a name="possible-scenarios"></a>Olası senaryolar
SAP çok kuruluşların içindeki en kritik uygulamalardan biri olarak görülür. Mimari ve bu uygulamaların işlemler çoğunlukla çok karmaşık ve kullanılabilirlik ve performans gereksinimlerini karşıladığınızdan emin olma önemlidir.

Bu nedenle kuruluşların dikkatle hakkında uygulamaları genel bulut ortamında, seçilen bulut sağlayıcısı bağımsız çalıştırılabilir düşünmeniz gerekir.

SAP NetWeaver dağıtmak için olası sistem türleri tabanlı uygulamaları içinde genel bulut ortamlarında aşağıda listelenmiştir:

1. Orta ölçekli üretim sistemleri
2. Geliştirme sistemleri
3. Test sistemleri
4. Prototip sistemleri
5. Öğrenme / tanıtım sistemleri

Başarıyla SAP sistemleri Azure Iaas veya Iaas genel olarak dağıtmak için geleneksel outsourcers veya barındırıcılar teklifleri ve Iaas teklifleri arasındaki önemli farkları anlamak önemlidir. Geleneksel barındırma sağlayıcısı veya dış kaynak altyapısı (ağ, depolama ve sunucu türü) barındırmak için bir müşterinin istediği iş yüküne uyum ise, bunun yerine Iaas dağıtımları için doğru iş yükü seçmek için Müşteri'nin sorumluluğundadır.

İlk adım olarak, müşteriler aşağıdaki öğeleri doğrulayın gerekir:

* SAP desteklenen Azure VM türleri
* Azure üzerinde SAP desteklenen ürünler/yayınlar
* Desteklenen işletim sistemi ve DBMS yayımları Azure belirli SAP sürümlerde için
* Farklı Azure SKU'ları tarafından sağlanan SAP işleme

Bu soruların yanıtlarını SAP notta okunabilir [1928533].

İkinci bir adım olarak, Azure kaynak ve bant genişliği sınırlamaları şirket içi sistemlerde gerçek kaynak tüketimi için karşılaştırılması gerekir. Bu nedenle, müşterilerin SAP alanı ile desteklenen Azure türlerinin farklı özellikler hakkında bilgi sahibi olmanız gerekir:

* CPU ve bellek kaynakları farklı VM türler ve
* IOPS bant genişliği farklı VM türler ve
* Farklı VM türler özelliklerini ağ.

Bu verilerin çoğunu bulunabilir [burada (Linux)] [ virtual-machines-sizes-linux] ve [burada (Windows)][virtual-machines-sizes-windows].

Yukarıdaki bağlantıyı sınırları listelenen unutmayın konusunda üst sınırı. Bu, sınırları kaynakların herhangi biri için gelmez, örneğin IOPS tüm koşullar altında sağlanabilir. Ancak seçilen VM türü CPU ve bellek kaynakları durumlardır. SAP tarafından desteklenen VM türleri için CPU ve bellek kaynakları zaman VM dahilinde tüketimi için ayrılmış ve bu nedenle herhangi bir noktada kullanılabilir.

Microsoft Azure platformu diğer Iaas platformlar gibi bir çok kiracılı platformudur. Başka bir deyişle, depolama, ağ ve diğer kaynakları kiracılar arasında paylaşılır. Akıllı azaltma ve kota mantığı kazanmadan ciddi bir şekilde başka bir kiracı (gürültülü komşu) performansını etkileyen bir kiracı önlemek için kullanılır. Bant genişliği yaşadı Varyanslar korumak Azure mantığında çalışır ancak kaynak/bant genişliği kullanılabilirliğini birçok müşteri kendi şirket içi dağıtımda kullanılır daha büyük sapmalar tanıtmak için küçük, yüksek oranda paylaşılan platformları eğilimlidir. Sonuç olarak, farklı düzeylerde bant genişliği ağ veya depolama g/ç (toplu yanı sıra gecikme) ilgili dakika dakika karşılaşabilirsiniz. Azure SAP sistemde bir şirket içi sistemde daha büyük sapmalar karşılaşması emin olma olasılığını göz önünde bulundurulması gerekir.

Son adım, kullanılabilirlik gereksinimlerini değerlendirmek için kullanılır. Bu durum ortaya çıkabilir, temel alınan Azure altyapı güncelleştirilmesi gerekiyor ve yeniden başlatılmasını VM'ler çalıştıran ana gerektirir. Bu durumlarda, bu konaklarda çalışan sanal makineler kapatılması ve de yeniden. Böyle bakım zamanlaması belirli bir bölge için çekirdek olmayan iş saatleri sırasında yapılır, ancak yeniden başlatma gerçekleşir birkaç saatlerde olası görece geniş bir penceredir. Bazılarını veya tümünü bu güncelleştirmeler etkisini azaltmak için yapılandırılabilir Azure platformu içinde çeşitli teknolojiler vardır. Gelecekteki geliştirmeler Azure platformu, DBMS ve SAP uygulama gibi yeniden başlatma etkisini en aza indirmek için tasarlanmıştır.

Azure SAP sisteme başarıyla dağıtmak için sistemleri işletim sistemi, veritabanı, şirket içi SAP ve SAP Azure destek matrisi, SAP uygulamaları görünmelidir Azure kaynakları içinde altyapı sağlayabilir sığacak ve çalışabilirsiniz ile kullanılabilirlik SLA Microsoft Azure sunar. Bu sistemlere tanımlandığı gibi aşağıdaki iki dağıtım senaryoları birinde karar vermeniz gerekir.

### <a name="1625df66-4cc6-4d60-9202-de8a0b77f803"></a>Yalnızca bulut - şirket içi müşteri ağdaki bağımlılıklar olmadan sanal makine dağıtımlarının Azure içine
![SAP demo veya Azure'da dağıtılan eğitim senaryo ile tek VM][planning-guide-figure-100]

Bu senaryo SAP ve SAP olmayan yazılım tüm bileşenleri tek bir VM içinde yüklendiği eğitimleri veya tanıtım sistemleri için genel bir durumdur. Bu dağıtım senaryosunda, üretim SAP sistemleri desteklenmez. Genel olarak, bu senaryo aşağıdaki gereksinimleri karşıladığından:

* Sanal makineleri kendilerini ortak bir ağ üzerinden erişilebilir. Doğrudan ağ bağlantısı gösterileri veya eğitimleri içerik veya müşteri sahibi olan şirket içi ağ için sanal makinelerin içinde çalışan uygulamalar için gerekli değildir.
* Eğitimleri veya tanıtım senaryo temsil eden birden çok VM durumunda ağ iletişimleri ve ad çözümleme VM'ler arasında çalışması gerekir. Ancak VM'ler kümesi arasındaki iletişimi VM'ler birkaç kümesi yan yana girişim dağıtılabilir böylece yalıtılmış olması gerekir.  
* Azure üzerinde barındırılan sanal makineleri için uzak oturum için son kullanıcı için Internet bağlantısı gereklidir. Bağlı olarak Konuk işletim sistemi, Terminal Hizmetleri/RDS veya VNC/ssh eğitim görevleri yerine getirmek veya gösterileri gerçekleştirmek için VM erişmek için kullanılır. SAP gibi 3200, 3300 & 3600 can bağlantı noktaları, aynı zamanda ortaya SAP uygulama örneği herhangi Internet bağlantılı masaüstünden erişilebilir.
* SAP sistemleri (ve yalnızca son kullanıcı erişimi için ortak Internet bağlantısı gerektirir ve diğer VM'ler için Azure'da bağlantı gerektirmez Azure tek başına senaryoda VM(s)) temsil eder.
* SAPGUI ve tarayıcı yüklenir ve VM doğrudan çalıştırılır.
* VM özgün durumuna hızlı sıfırlamanız ve yeni dağıtım, özgün durumuna yeniden gereklidir.
* Tanıtım ve eğitim senaryoları söz konusu olduğunda, gerçekleşen birden çok VM, bir Active Directory / OpenLDAP ve/veya DNS hizmeti VM'ler her kümesi için gereklidir.

![Sanal makinenin bir tanıtım veya bir Azure bulut hizmeti eğitim senaryoda temsil eden, Grup][planning-guide-figure-200]

Sanal makine kümelerinin her VM her kümesinin aynı olduğu paralel olarak dağıtılması gerekiyor göz önünde bulundurmanız önemlidir.

### <a name="f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10"></a>Şirket içi - tek dağıtımını ya da Azure ile şirket içi ağınıza tam olarak tümleştirilmiş zorunluluğu içine birden çok SAP VM
![VPN siteden siteye bağlantı (şirket içi)][planning-guide-figure-300]

Bu senaryo, birçok olası dağıtım desenlerle bir şirket içi senaryodur. SAP bazı bölümlerini çalıştıran yatay şirket içi ve Azure üzerinde SAP diğer bölümleri yatay olarak basit açıklanabilir. SAP bileşenleri bir parçası Azure üzerinde çalışan olgu tüm yönlerini son kullanıcılar için saydam olmalıdır. Bu nedenle SAP aktarım düzeltme sistem (STMS), RFC iletişimi, yazdırma, güvenlik (SSO gibi) vb. sorunsuz bir şekilde Azure üzerinde çalışan SAP sistemler için çalışır. Ancak şirket içi senaryo da burada tam SAP yatay Müşteri'nin etki alanı ile Azure üzerinde çalışır ve Azure'da DNS genişletilmiş bir senaryo açıklanmaktadır.

> [!NOTE]
> Üretken SAP sistemleri çalıştırmak için desteklenen dağıtım senaryosu budur.
>
>

Okuma [bu makalede] [ vpn-gateway-create-site-to-site-rm-powershell] Microsoft Azure için şirket içi ağınıza bağlanma hakkında daha fazla bilgi için

> [!IMPORTANT]
> Azure ve şirket içi müşteri dağıtımlar arasında şirketler arası senaryolar hakkında varsayılır, tüm SAP sistemleri bazda arıyoruz. Olan senaryoları *desteklenmiyor* için şirket içi senaryolar şunlardır:
>
> * SAP uygulamaları farklı katmanlar farklı dağıtım yöntemleri çalışıyor. Örneğin DBMS katman içi ancak SAP uygulama katmanı Azure Vm'leri olarak (veya tersi) dağıtılan VM'ler içinde çalışır.
> * Azure ve bazı şirket içi SAP katmanda bazı bileşenleri. Örneğin, şirket içi ve Azure sanal makineleri arasındaki SAP uygulama katmanı örneklerini bölme.
> * Bir sistem SAP örnekleri birden çok Azure bölgeleri üzerinde çalışan sanal makineler dağıtımını desteklenmiyor.
>
> Bu kısıtlamalar için çok düşük gecikme süresi yüksek performanslı bir ağ özellikle uygulama örnekleri ile bir SAP sisteminin DBMS katman arasındaki bir SAP sistemi içinde gereksinimini nedenidir.
>
>

### <a name="supported-os-and-database-releases"></a>Desteklenen işletim sistemi ve veritabanı sürümleri
* Bu makalede Azure sanal makine Hizmetleri'ni listelenen için desteklenen Microsoft sunucu yazılımı: <http://support.microsoft.com/kb/2721672>.
* İşletim sistemi sürümleri desteklenir, SAP yazılım ile birlikte Azure sanal makine hizmetlerini desteklenen veritabanı sistemi sürümleri SAP notta belgelenen [1928533].
* SAP uygulamaları ve Azure sanal makine hizmetlerini desteklenen sürümler SAP notta belgelenmiştir [1928533].
* Yalnızca 64 Bit görüntüleri SAP senaryoları için azure'da Konuk VM olarak çalıştırmak için desteklenir. Bu, aynı zamanda yalnızca 64-bit SAP uygulamaları ve veritabanları desteklenir anlamına gelir.

## <a name="microsoft-azure-virtual-machine-services"></a>Microsoft Azure sanal makine Hizmetleri
Microsoft Azure platformu barındırılan ve Microsoft veri merkezleri işletilen bir Internet ölçeğinde bulut Hizmetleri platformudur. Platform, Microsoft Azure sanal makine Hizmetleri (bir hizmet veya Iaas olarak altyapı) ve zengin bir Platform olarak hizmet (PaaS) özellikleri kümesi içerir.

Azure platformu teknoloji eylemli gereksinimini azaltır ve altyapı satın alır. Koruma ve isteğe bağlı işlem ve ana bilgisayar, ölçeklendirme ve web uygulaması ve bağlı uygulamaları yönetmek için depolama alanı sağlayarak uygulamaları işletim basitleştirir. Altyapı yönetimi, yüksek kullanılabilirlik için tasarlanmış bir platformuyla otomatik ve dinamik Kullandıkça Öde bir fiyatlandırma modelini seçeneğiyle kullanım ihtiyaçlarına uygun ölçeklendirme.

![Microsoft Azure sanal makine hizmetlerini konumlandırma][planning-guide-figure-400]

Azure sanal makine Hizmetleri ile Microsoft, özel sunucu görüntülerinin (bkz: Şekil 4) Iaas örnekleri Azure'a dağıtmak imkan verir. Azure'da sanal makineler Hyper-V sanal sabit sürücülerde (VHD) temel alır ve konuk işletim sistemi farklı işletim sistemlerini çalıştıramaz.

Azure sanal makine hizmeti, bir işlemsel açısından bakıldığında, şirket içinde dağıtılan sanal makineleri olarak benzer deneyimleri sunar. Ancak, tedarik etmek, yönetmek ve altyapıyı yönetmek için gerek duymadığınız önemli bir avantajı vardır. Geliştiriciler ve Yöneticiler bu sanal makineleri içindeki işletim sistemi görüntüsünün tam denetime sahiptir. Yöneticiler uzaktan, Bakım ve sorun giderme görevleri ve bunun yanı sıra yazılım dağıtım görevlerini gerçekleştirmek için bu sanal makineleri için oturum açabilirsiniz. Dağıtım in regard to yalnızca kısıtlamaları boyutları ve Azure Vm'leri özelliklerini var. Bunlar gibi ince olmayabilir bu şirket içinde yapılabilir gibi ayrıntılı yapılandırma. VM türlerinin bileşimini temsil eden bir seçenek vardır:

* Vcpu'lar sayısı,
* Bellek
* Eklenebilecek VHD'ler sayısı,
* Ağ ve depolama bant genişlikleri.

Çeşitli farklı sanal makinelerin boyutları sınırlamaları ve boyutu sunulan bir tabloda görülme [bu makalede (Linux)] [ virtual-machines-sizes-linux] ve [bu makalede (Windows)] [virtual-machines-sizes-windows].

Anladıkça, farklı aileleri ya da sanal makineleri dizi vardır. Sanal makineleri aşağıdaki aileleri ayırt edebilirsiniz:

* A0 A7 VM türleri: Bu tüm SAP için sertifikalıdır. Azure Iaas ile sunulan ilk VM dizisi.
* A8-A11 VM türleri: yüksek performanslı örnekleri bilgi işlem. Farklı daha iyi gerçekleştirme çalışan diğer A-series VM'ler daha ana işlem.
* Dv2/D-serisi VM türler: A0 A7 daha iyi gerçekleştirme. Tüm VM türleri SAP ile sertifikalı.
* DS/DSv2 serisi VM türleri: Dv2/D-serisi, ancak Azure Premium Storage bağlanabilmek için olan benzer (bölüm bkz [Azure Premium Storage] [ planning-guide-3.3.2] bu belgenin). Yeniden tüm VM türleri SAP ile sertifikalı.
* G-serisi VM türler: yüksek bellek VM türler.
* GS serisi VM türler: G-serisi istiyor ancak Azure Premium Storage kullanma seçeneğiniz de dahil olmak üzere (bölüm bkz [Azure Premium Storage] [ planning-guide-3.3.2] bu belgenin). GS serisi VM'ler veritabanı sunucusu kullanırken, Premium depolama DB veri ve işlem günlük dosyaları için kullanan zorunludur.

Farklı VM serisinin aynı CPU ve bellek yapılandırmalarını bulabilirsiniz. Bununla birlikte, bunlar bu Vm'lere farklı serisi dışında işleme performansını baktığınızda önemli ölçüde değişebilir. Aynı CPU ve bellek yapılandırmasına sahip olmasına rağmen. Farklı VM türler giriş temel alınan ana bilgisayar sunucu donanımı farklı verimlilik vardı nedenidir.  Genellikle verimliliği performansından gösterilen fark farklı VM'ler fiyatına yansıtılır.

Tüm farklı VM dizisi (için Azure bölgeleri sonraki bölüme bakın) Azure bölgeler her birinde sunulan. Ayrıca tüm sanal makineleri veya VM seri için SAP sertifikalı unutmayın.

> [!IMPORTANT]
> SAP NetWeaver tabanlı uygulamaları için kullanılması yalnızca alt VM türleri ve yapılandırmaları listelenen SAP notta [1928533] desteklenir.
>
>

### <a name="be80d1b9-a463-4845-bd35-f4cebdb5424a"></a>Azure bölgeleri
Sanal makinelerine dağıtmak için tanır şekilde adlı Microsoft *Azure bölgeleri*. Bir Azure bölgesi yakınında içinde bulunan bir veya birden çok veri merkezleri olabilir. Çoğu dünyadaki coğrafi bölgeler için en az iki Azure bölgeleri Microsoft sahiptir. Örneğin, Avrupa'da olduğu bir Azure bölgesi *Kuzey Avrupa* ve aşağıdakilerden birini *Batı Avrupa*. Böylece teknik veya doğal afetler hem Azure bölgeleri aynı coğrafi bölgede etkilemez coğrafi bölge içindeki iki tür Azure bölgeleri yeterince önemli uzaklığı tarafından ayrılır. Microsoft sürekli olarak farklı coğrafi bölgeler yeni Azure bölgelerde çıkışı genel oluşturuyor olduğundan, bu bölgeler sayısı sürekli büyüyen ve Ara 2015'ten itibaren ek bölgeler önceden duyurulmuş 20 Azure bölgesiyle sayısına ulaşıldı. İki Azure bölgeleri Çin'de dahil olmak üzere tüm bu bölgeler, halinde, bir müşteri olarak SAP sistemleri dağıtabilirsiniz. Azure bölgeler hakkında güncel güncel bilgiler için bu Web sitesine bakın: <https://azure.microsoft.com/regions/>

### <a name="8d8ad4b8-6093-4b91-ac36-ea56d80dbf77"></a>Microsoft Azure sanal makine kavramı
Microsoft Azure altyapı (Iaas) çözümü olarak benzer işlevler ile sanal makineleri barındırmak için bir şirket içi sanallaştırma çözümü olarak sunar. Hangi dağıtım ve yönetim yetenekleri de sunar sanal makinelerden Azure portalın içinde PowerShell veya CLI, oluşturabilirsiniz.

Azure Resource Manager, uygulamalarınızı bildirim temelli bir şablon aracılığıyla sağlamanıza olanak tanır. Tek bir şablonda birden çok hizmeti bağımlılıklarıyla birlikte dağıtabilirsiniz. Her aşaması sırasında uygulamanızın uygulama yaşam döngüsü veya sürekli olarak dağıtmak için aynı şablonu kullanın.

Resource Manager şablonları kullanma hakkında daha fazla bilgi şurada bulunabilir:

* [Dağıtmak ve Azure Resource Manager şablonları ve Azure CLI kullanarak sanal makineleri yönetme] [../../linux/create-ssh-secured-vm-from-template.md]
* [Azure Resource Manager ve PowerShell kullanarak sanal makineleri yönetme][virtual-machines-deploy-rmtemplates-powershell]
* <https://Azure.microsoft.com/documentation/Templates/>

Başka bir ilginç içinden, gereksinimlerinizi karşılayacak sanal makine örnekleri hızla dağıtamaz belirli depoları hazırlama olanak sağlayan görüntüler sanal makinelerden oluşturma olanağı özelliğidir.

Sanal makinelerden görüntüleri oluşturma hakkında daha fazla bilgi bulunabilir [bu makalede (Linux)] [ virtual-machines-linux-capture-image-resource-manager] ve [bu makalede (Windows)] [ virtual-machines-windows-capture-image-resource-manager].

#### <a name="df49dc09-141b-4f34-a4a2-990913b30358"></a>Hata etki alanları
Veri merkezleri ve fiziksel dikey veya raf hata etki alanı kabul edilebilir sırada yer alan fiziksel altyapı yakından ilişkili fiziksel bir birim hata, çok hata etki alanlarını temsil, ikisi arasında doğrudan bire bir eşleme yoktur.

Microsoft Azure sanal makine Hizmetleri'ndeki bir SAP sisteminin bir parçası olarak birden çok sanal makine dağıttığınızda, farklı hata böylece Microsoft Azure SLA gereksinimleri karşılayan etki alanı, uygulamanıza dağıtmak için Azure yapı denetleyicisi etkileyebilir. Ancak, dağıtım hata etki alanı Azure ölçek birimi (yüzlerce işlem düğümleri veya depolama düğümleri ve ağ koleksiyonu) üzerinden veya belirli bir hata etki alanı için VM'ler atama üzerinde doğrudan denetim elinizde bir şeydir. Sanal makineleri bir dizi farklı hata etki alanları dağıtmak için Azure yapı denetleyicisi yönlendirmek için bir Azure kullanılabilirlik kümesi Vm'lere dağıtım sırasında atamanız gerekir. Bölüm Azure kullanılabilirlik kümeleri hakkında daha fazla bilgi için bkz: [Azure kullanılabilirlik kümeleri] [ planning-guide-3.2.3] bu belgedeki.

#### <a name="fc1ac8b2-e54a-487c-8581-d3cc6625e560"></a>Yükseltme etki alanları
Yükseltme etki alanlarının bir VM içinde birden çok VM çalıştıran SAP örneklerinin oluşan bir SAP sistemi içinde nasıl güncelleştirileceğini belirlemek için yardımcı bir birimi temsil eder. Yükseltme oluştuğunda, bu yükseltme etki alanları tek tek güncelleme işlemi Microsoft Azure gider. Farklı yükseltme etki alanları VM'ler dağıtım sırasında yayarak kısmen potansiyel kesintiler SAP sisteminizi koruyabilirsiniz. Sanal makineleri farklı yükseltme etki alanları yayılan bir SAP sistemi dağıtmak için Azure zorlamak için her bir VM Dağıtım sırasında belirli bir özniteliği ayarlamanız gerekir. Hata etki alanları için benzer bir Azure ölçek birimi birden fazla yükseltme etki alanlarına ayrılmıştır. Sanal makineleri bir dizi farklı yükseltme etki alanları dağıtmak için Azure yapı denetleyicisi yönlendirmek için bir Azure kullanılabilirlik kümesi Vm'lere dağıtım sırasında atamanız gerekir. Bölüm Azure kullanılabilirlik kümeleri hakkında daha fazla bilgi için bkz: [Azure kullanılabilirlik kümeleri] [ planning-guide-3.2.3] aşağıda.

#### <a name="18810088-f9be-4c97-958a-27996255c665"></a>Azure kullanılabilirlik kümeleri
Azure sanal makineleri bir Azure kullanılabilirlik kümesi içinde Azure yapı denetleyicisi tarafından farklı hata ve yükseltme etki alanları dağıtılır. Tüm sanal makineleri bir SAP sisteminin altyapı Bakımı veya bir hata etki alanı içinde bir hata durumunda kapatılmadan engellemek için farklı hata ve yükseltme etki alanı üzerinde dağıtım amacı olan. Varsayılan olarak, sanal makineleri bir kullanılabilirlik kümesinin parçası değildir. Bir VM katılım bir kullanılabilirlik kümesindeki dağıtım zamanında veya daha sonra yeniden yapılandırılması ve bir VM yeniden dağıtımını tarafından tanımlanır.

Azure kullanılabilirlik kümeleri ve kullanılabilirlik kümeleri ile ilgili hata ve yükseltme etki alanları için yol kavramı anlamak için okuyun [bu makalede][virtual-machines-manage-availability]

Tanımlamak için bir json şablonu aracılığıyla ARM için kullanılabilirlik kümesi bkz [rest API belirtimlerin](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2015-06-15/swagger/compute.json) ve "kullanılabilirlik" arayın.

### <a name="a72afa26-4bf4-4a25-8cf7-855d6032157f"></a>Depolama: Microsoft Azure depolama ve veri diskleri
Microsoft Azure sanal makineleri farklı depolama türlerini kullanın. SAP Azure sanal makine hizmetlerini uygularken, bu iki ana depolama türleri arasındaki farkları anlamak önemlidir:

* Kalıcı olmayan, geçici depolama alanı.
* Kalıcı depolama.

Kalıcı olmayan depolama çalışan sanal makineleri doğrudan bağlı olduğu ve işlem düğümlerinde kendileri yerel örnek depolama (geçici depolama) bulunur. Boyutu dağıtım başlatıldığında seçilen sanal makine boyutuna bağlıdır. Bu depolama türü volatile ve disk bir sanal makine örneğini yeniden başlatıldığında, bu nedenle başlatılır. Genellikle, işletim sistemi için disk belleği dosyası bu geçici bir diskte yer alır.

- - -
> ![Windows][Logo_Windows] Windows
>
> Windows Vm'lerinde sürücü D:\ dağıtılan bir VM olarak temp sürücüsü bağlı.
>
> ![Linux][Logo_Linux] Linux
>
> Linux VM'ler üzerinde /mnt/resource veya /mnt takılı. Daha fazla ayrıntıları buraya bakın:
>
> * [Nasıl bir Linux sanal makineye bir veri diski Ekle][virtual-machines-linux-how-to-attach-disk]
> * <https://docs.microsoft.com/Azure/Storage/Storage-About-Disks-and-vhds-Linux#Temporary-disk>
>
>

- - -
Ana bilgisayar sunucusunun kendisi üzerinde depolandığından gerçek volatile sürücüdür. VM bir dağıtımın taşınsa (örneğin nedeniyle Bakım ana bilgisayar veya kapatma ve yeniden başlatma) sürücü içeriğini kaybolur. Bu nedenle, bu sürücüde herhangi bir önemli veri depolamak için bir seçenek değil. Bu depolama türü için kullanılan medya türünü, Haziran 2015'ten itibaren neye benzediğini gösteren çok farklı performans özellikleri olan farklı VM dizisi arasında farklılık gösterir:

* A5-A7: Çok sınırlı performans. Sayfa dosyası dışındaki her şey için önerilmez
* A8-A11: Bazı on bin IOPS ile çok iyi performans özellikleri ve > 1GB/sn verim.
* D-serisi: Bazı sonra bin IOPS ile çok iyi performans özellikleri ve > 1GB/sn verim.
* DS serisi: Bazı on bin IOPS ile çok iyi performans özellikleri ve > 1GB/sn verim.
* G-serisi: Bazı on bin IOPS ile çok iyi performans özellikleri ve > 1GB/sn verim.
* GS serisi: Bazı on bin IOPS ile çok iyi performans özellikleri ve > 1GB/sn verim.

Yukarıdaki deyimleri, SAP ile sertifikalı VM türlerine uygulanıyor. Mükemmel IOPS ve üretilen iş ile VM dizisi Dengeleme için bazı DBMS özellikleri tarafından hakkını kullanmaya devam eder. Daha fazla bilgi için bkz: [DBMS Dağıtım Kılavuzu'na][dbms-guide].

Microsoft Azure Storage kalıcı depolama ve tipik düzeyde koruma ve SAN depolama alanı üzerinde görülen artıklık sağlar. Azure depolama birimine dayalı sanal sabit Azure Storage Hizmetleri bulunan disk (VHD) disklerdir. Yerel işletim sistemi diski (Windows C:\, Linux/dev/sda1) Azure depolama alanında depolanır ve ek birimler/disklere VM'ye takılı depolanır, çok.

Varolan bir VHD'yi şirket içi karşıya yükleme veya Azure içinde boş olanlardan oluşturun ve onlara dağıtılan VM'ler eklemek mümkündür.

Oluşturma veya bir VHD Azure depolama alanına karşıya yükleme sonrasında bağlayın ve bu var olan bir sanal makine ekleyin ve mevcut (takılmamış) VHD kopyalamak için mümkündür.

Bu VHD kalıcı olarak veri ve bu değişikliklerin yeniden başlatmadan ve bir sanal makine örneğini yeniden güvenlidir. Örneği silinse bile bu VHD güvende ve dağıtılması veya işletim sistemi olmayan diskleri durumunda diğer VM'ler için bağlanabilir.

Azure Storage ağ içindeki farklı artıklık düzeyleri yapılandırılabilir:

* Seçilebilir en düşük düzeydir *yerel artıklık*, eşdeğer olan üç-çoğaltma aynı veri merkezinde bir Azure bölgesinin içindeki verilerin (bölüm bkz [Azure bölgeleri] [ planning-guide-3.1]).
* Üç görüntüleri farklı veri yayılan bölge olarak yedekli depolama aynı Azure bölge içinde toplanır.
* Zaman uyumsuz olarak içerik verileri aynı coğrafi bölgede barındırılan başka bir Azure bölgesindeki başka bir üç görüntülerini yarlar coğrafi artıklık varsayılan artıklık düzeydir.

Ayrıca bu makalede farklı artıklık seçenekleri ile ilgili en üstünde tablosuna bakın: <https://azure.microsoft.com/pricing/details/storage/>

Azure Storage hakkında daha fazla bilgi şurada bulunabilir:

* <https://Azure.microsoft.com/documentation/Services/Storage/>
* <https://Azure.microsoft.com/Services/Site-Recovery>
* <https://docs.microsoft.com/REST/api/storageservices/Understanding-Block-BLOBS--Append-BLOBS--and-Page-BLOBS>
* <https://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/Azure-disk-Encryption-for-Linux-and-Windows-Virtual-Machines-Public-Preview.aspx>

#### <a name="azure-standard-storage"></a>Standart Azure depolama
Azure Iaas yayımlandığında azure standart depolama kullanılabilir depolama türündeydi. Tek disk başına zorlanan IOPS kotaları vardı. Şirket içi SAN/NAS cihazları için birinci sınıf SAP sistemleri dağıtıldığı barındırılan olarak gecikme yaşadı aynı sınıfta değildi. Bununla birlikte, Azure Standard Storage yüzlerce için yeterli oluyor uygulamasına yol açıyordu bu sırada Azure üzerinde dağıtılan SAP sistemleri.

Azure standart depolama hesaplarında depolanan diskleri depolanan gerçek verileri, depolama işlemleri, giden veri aktarımları ve seçilen artıklığı seçeneği hacmine göre ücretlendirilen. En fazla 1 TB boyutu en fazla disk oluşturulabilir, ancak bu boş kaldığı sürece ücretsizdir. Bir VHD 100 GB ile sonra doldurduğunuzda ve VHD ile oluşturulan nominal boyutu 100 GB depolama için sizden ücret kesilir.

#### <a name="ff5ad0f9-f7f4-4022-9102-af07aef3bc92"></a>Azure Premium depolama
Nisan 2015'te, Microsoft Azure Premium Storage kullanıma sunmuştur. Premium depolama sağlamak için hedefle sunulan:

* Daha iyi g/ç gecikmesi.
* Daha iyi verimlilik.
* G/ç gecikmesi daha az sonuçlarındaki.

Bu amaç için iki en önemli olan birçok değişikliğin yapılmadığı:

* Azure depolama düğümleri SSD diskleri kullanımı
* Yeni bir Azure işlem düğümü yerel SSD tarafından yedeklenen Önbellek Okuma

Standart depolama burada özellikler değişmemiştir ters yönde boyutu veya disk (VHD), bağımlı Premium depolama şu anda bu makalede gösterilen üç farklı disk kategorisi vardır: <https://azure.microsoft.com/pricing/ Ayrıntılar/depolama/yönetilmeyen-diskleri />

IOP/disk ve disk işleme/disk disk boyutu kategorisine göre bağımlı bakın

Premium depolama durumunda maliyet temel tür diskleri, ancak böyle bir diski, disk içinde depolanan veri miktarını bağımsız boyutu kategorisini depolanan gerçek veri birimi değil.

Ayrıca, diskleri gösterilen boyutu kategoride doğrudan eşleme değil Premium depolama oluşturabilirsiniz. Bu durumda, özellikle diskleri standart depolama biriminden Premium depolama alanına kopyalama olabilir. Böyle durumlarda, sonraki en büyük Premium depolama diski seçeneği için bir eşleme gerçekleştirilir.

Yalnızca belirli VM serisi Azure Premium depolamadan yararlanabilirsiniz unutmayın. DEC 2015'ten itibaren DS - ve GS serisi bunlar. D-serisi DS serisi bağlama Premium depolama yeteneği olan özel VM'ler ek olarak barındırılan sanal diskler için Azure Standard Storage tabanlı olarak DS serisi temelde aynıdır. Aynı şey G serisi GS serisi karşılaştırma için geçerlidir.

DS serisi VM'ler parçası çıkış denetimi varsa [bu makalede (Linux)] [ virtual-machines-sizes-linux] ve [bu makalede (Windows)][virtual-machines-sizes-windows], fark VM düzeyinin ayrıntı Premium depolama disklerde veri birimi sınırlamalar vardır. Ayrıca farklı DS serisi veya GS serisi VM'ler bağlanabilir veri diski sayısı gelince farklı sınırlamaları vardır. Bu sınırlar da yukarıdaki makalesinde belgelenmiştir. Ancak aslında, örneğin, tek bir DS14 VM 32 x P30 disklere bağlamanız halinde P30 diskin en yüksek verimlilik x 32 alamazsınız anlamına gelir. Bunun yerine makalede anlatıldığı şekilde VM düzeyinde en fazla üretilen veri işleme sınırlar.

Premium depolama hakkında daha fazla bilgi şurada bulunabilir: <http://azure.microsoft.com/blog/2015/04/16/azure-premium-storage-now-generally-available-2>

#### <a name="c55b2c6e-3ca1-4476-be16-16c81927550f"></a>Yönetilen diskleri
Yeni bir kaynak türü Azure kaynağı Azure depolama hesaplarında depolanan VHD yerine kullanılan Yöneticisi'nde yönetilen disklerdir. Yönetilen diskleri otomatik olarak bağlı olan sanal makinenin kullanılabilirlik kümesi hizalamak ve bu nedenle, sanal makine ve sanal makinede çalışan hizmetlerin kullanılabilirliğini artırmak. Daha fazla bilgi için okuma [genel bakış makalesi](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview).

İçin önerdiğimiz dağıtım ve sanal makinelerin yönetimini basitleştirmek için yönetilen disk kullanın.
SAP şu anda yalnızca Premium yönetilen diskleri destekler. Daha fazla bilgi için SAP not okuma [1928533].

#### <a name="azure-storage-accounts"></a>Azure Depolama Hesapları
Hizmetleri veya azure'da VM dağıtırken dağıtımı VHD'ler VM görüntüleri ve Azure Storage hesapları olarak adlandırılan birimler halinde düzenlenebilir. Azure dağıtım planlaması yaparken dikkatli bir şekilde Azure kısıtlamalarını dikkate almanız gerekir. Bir yandan, depolama hesapları, sınırlı sayıda Azure abonelik başına yoktur. Her Azure depolama hesabı VHD dosyaları, çok sayıda tutabilir rağmen bir sabit sınır yoktur depolama hesabı başına toplam IOPS üzerinde. SAP VM'ler yüzlerce önemli GÇ çağrı oluşturuluyor DBMS sistemleriyle dağıtırken, birden çok Azure depolama hesapları arasında yüksek IOPS DBMS sanal makineleri dağıtmak için önerilir. Azure Storage hesapları geçerli sınırı abonelik başına aşmayacak şekilde dikkatli olunması gerekir. Depolama SAP sistem veritabanı dağıtım sürecinin hayati bir parçası olduğundan, bu kavram zaten başvurulan bölümlerinde daha ayrıntılı ele alınmıştır [DBMS Dağıtım Kılavuzu'na][dbms-guide].

Azure Storage hesapları hakkında daha fazla bilgi bulunabilir [bu makalede][storage-scalability-targets]. Bu makaleyi okuduktan, sınırlamalar Azure standart depolama hesapları ve Premium depolama hesapları arasındaki farkları olduğunu unutmayın. Bu tür bir depolama hesabında depolanan veri hacmine önemli farklılıklar şunlardır. Standart depolama, Premium Storage ile büyük bir büyüklük birimdir. Diğer tarafta standart depolama hesabı IOP ciddi bir şekilde sınırlı (sütununa bakın **toplam istek oranı**), ancak böyle bir kısıtlama Azure Premium depolama hesabı vardır. Ayrıntıları ve bu farklılıklar sonuçlarını SAP sistemleri, özellikle DBMS sunucularda dağıtımları ele alırken aşağıdakiler ele alınacaktır.

Bir depolama hesabında düzenleme ve farklı VHD'ler kategorilere ayırmak amacıyla farklı kapsayıcılar oluşturmak için olanağına sahip olursunuz. Bu kapsayıcılar, örneğin, ayrı VHD'leri farklı VM'ler için genellikle kullanılır. Yalnızca bir kapsayıcı veya tek bir Azure depolama hesabı altında birden çok kapsayıcı kullanarak hiçbir performans etkileri vardır.

Azure içinde Azure içinde VHD için benzersiz bir ad sağlamak için gereken aşağıdaki adlandırma bağlantısı VHD adı aşağıdaki gibidir:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

Belirtildiği gibi Azure depolama alanında depolanan VHD benzersiz şekilde tanımlamak yukarıdaki dizesi gerekiyor.

### <a name="61678387-8868-435d-9f8c-450b2424f5bd"></a>Microsoft Azure ağ iletişimi
Microsoft Azure SAP yazılımıyla hayata geçirmek için istiyoruz tüm senaryoları eşleme sağlayan bir ağ altyapısı sağlar. Özellikleri şunlardır:

* Erişim dışarıdan, doğrudan Windows Terminal Hizmetleri veya ssh/VNC üzerinden VM'ler
* Hizmetler ve uygulamalar VM içinden tarafından kullanılan belirli bağlantı noktaları erişim
* İç iletişim ve Azure Vm'leri olarak dağıtılan VM'ler grubu arasındaki ad çözümlemesi
* Azure ağı ile müşterinin şirket içi ağ arasındaki şirket içi bağlantılar
* Azure siteler arasında çapraz Azure bölgesini ya da veri merkezi bağlantısı

Daha fazla bilgi şurada bulunabilir: <https://azure.microsoft.com/documentation/services/virtual-network/>

Azure ad ve IP çözümleme yapılandırmak için birçok farklı olasılık vardır. Bu belgede, Azure DNS (kendi bir DNS hizmeti tanımlama aksine) kullanarak varsayılan yalnızca bulut senaryoları bağlıdır. Ayrıca kendi DNS sunucusunu ayarlama yerine kullanılabilecek yeni bir Azure DNS hizmeti vardır. Daha fazla bilgi bulunabilir [bu makalede] [ virtual-networks-manage-dns-in-vnet] ve [bu sayfayı](https://azure.microsoft.com/services/dns/).

Şirket içi senaryolarda, biz olgu üzerinde bağlı, şirket içi AD/OpenLDAP/DNS genişletilmişse VPN veya özel bağlantı Azure'a. Belirli senaryolar aşağıda belirtildiği gibi Azure üzerinde yüklü bir AD/OpenLDAP çoğaltmanın gerekli olabilir.

Ağ iletişimi olduğundan ve ad çözümlemesi SAP sistem veritabanı dağıtım sürecinin hayati bir parçası ise, bu kavram daha ayrıntılı olarak ele alınmıştır [DBMS Dağıtım Kılavuzu'na][dbms-guide].

##### <a name="azure-virtual-networks"></a>Azure Sanal Ağları
Bir Azure sanal ağı oluşturarak Azure DHCP işlevleri tarafından ayrılan özel IP adresleri adres aralığı tanımlayabilirsiniz. Şirket içi senaryolarda tanımlanan IP adresi aralığı hala DHCP kullanarak Azure tarafından ayrılmış. Ancak, etki alanı adı çözümleme (VM'ler bir şirket içi etki alanının bir parçası olduğunu varsayarak) şirket içi gerçekleştirilir ve bu nedenle farklı Azure Cloud Services ötesinde adresleri çözebilirsiniz.

Her sanal makine azure'da bir sanal ağa bağlı olması gerekir.

Daha fazla ayrıntı bulunabilir [bu makalede] [ resource-groups-networking] ve [bu sayfayı](https://azure.microsoft.com/documentation/services/virtual-network/).

[comment]: <> (MShermannd Yapılacaklar bulamadı OpenLDAP konu + ARM içeren bir makale;)
[comment]: <> (MSSedusch < https://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL>)

> [!NOTE]
> Varsayılan olarak, bir VM dağıtıldıktan sonra sanal ağ yapılandırması değiştirilemiyor. TCP/IP'yi ayarları Azure DHCP sunucusuna bırakılmalıdır. Varsayılan, dinamik IP ataması davranıştır.
>
>

Sanal Ağ kartının MAC adresi, örneğin yeniden boyutlandırma ve işletim sistemi yeni ağ kartı seçer ve otomatik olarak bu durumda IP ve DNS adreslerini atamak için DHCP kullanan Windows veya Linux Konuk sonra değiştirebilirsiniz.

##### <a name="static-ip-assignment"></a>Statik IP ataması
Bir Azure sanal ağ içinden vm'lere sabit veya ayrılmış IP adresleri atamak da mümkündür. Bir Azure sanal ağında VM'ler çalıştıran bu işlevsellik gerekli veya bazı senaryolar için gerekli yararlanmak için harika olasılığı açar. IP ataması VM çalışıyor veya kapatma olup bağımsız olarak VM varlığını geçerli kalır. Sonuç olarak, sanal ağ için IP adresi aralığı tanımlarken, sanal makineleri (VM'ler çalıştığından ve durdurulmuş) genel sayısını dikkate almak gerekir. VM ve ağ arabirimiyle silinene veya IP adresi yeniden XML'deki atanmış kadar IP adresi atanmış olarak kalır. Daha fazla bilgi için okuma [bu makalede][virtual-networks-static-private-ip-arm-pportal].

##### <a name="multiple-nics-per-vm"></a>VM başına birden çok NIC
Bir Azure sanal makine için birden çok sanal ağ arabirim kartı (Vnıc) tanımlayabilirsiniz. Ağ trafiği Ayarla başlatmak için birden çok vNICs sahip olanağı, örneğin, istemci trafiğini bir Vnıc ve arka uç trafiğinin yönlendirildiği ayrımı ikinci Vnıc yönlendirilir. Türüne VM var. farklı sınırlamaları gelince vNICs sayısına bağlıdır. Tam Ayrıntılar, işlevsellik ve sınırlamaları aşağıdaki makalelerde bulunabilir:

* [Birden çok NIC ile bir Windows VM oluşturma][virtual-networks-multiple-nics-windows]
* [Birden çok NIC ile bir Linux VM oluşturma][virtual-networks-multiple-nics-linux]
* [Birden çok NIC bir şablon kullanarak sanal makineleri dağıtma][virtual-network-deploy-multinic-arm-template]
* [Birden çok NIC PowerShell kullanarak sanal makineleri dağıtma][virtual-network-deploy-multinic-arm-ps]
* [Birden çok NIC Azure CLI kullanarak sanal makineleri dağıtma][virtual-network-deploy-multinic-arm-cli]

#### <a name="site-to-site-connectivity"></a>Siteden siteye bağlantı
Şirket içi Azure Vm'leri ve şirket içi saydam ve kalıcı bir VPN bağlantısıyla bağlı olur. Azure en yaygın SAP dağıtım modelinde hale gelmesi bekleniyor. İşlem yordamlarını ve Azure SAP durumlarda işlemlerle saydam çalışmalıdır varsayılır. Şirket içi SAP aktarım yönetim sistemi (TMS) aktarım için azure'da geliştirme sistemi olan bir test sisteme değiştirir kullanım yanı sıra bu dışında bu sistemler yazdırma açabilmelisiniz anlamına gelir dağıtıldı. Daha fazla belge siteden siteye geçici bulunabilir [bu makalede][vpn-gateway-create-site-to-site-rm-powershell]

##### <a name="vpn-tunnel-device"></a>VPN tüneli aygıtı
Siteden siteye bağlantı (şirket içi veri merkezine Azure veri merkezine) oluşturmak için almak ve bir VPN cihazı yapılandırma veya Windows Server 2012 ile bir yazılım bileşeni olarak Yönlendirme ve Uzaktan Erişim hizmeti (, tanıtılan RRAS) kullanmak gerekir.

* [PowerShell kullanarak siteden siteye VPN bağlantısı olan bir sanal ağ oluşturma][vpn-gateway-create-site-to-site-rm-powershell]
* [Siteden siteye VPN Gateway bağlantıları için VPN cihazları hakkında][vpn-gateway-about-vpn-devices]
* [VPN ağ geçidi SSS][vpn-gateway-vpn-faq]

![Şirket içi ve Azure arasında siteden siteye bağlantı][planning-guide-figure-600]

Yukarıdaki şekilde, Azure sanal ağlarda IP adresi alt aralıklara kullanımı için ayrılmış iki Azure aboneliğine sahip gösterir. Şirket içi ağ bağlantısını Azure VPN kurulur.

#### <a name="point-to-site-vpn"></a>Noktadan siteye VPN
Noktadan siteye VPN kendi VPN ile Azure'a bağlanmak için her istemci makine gerektirir. Konumundaki arıyoruz SAP senaryoları için noktadan siteye bağlantı pratik değil. Bu nedenle, daha ayrıntılı başvuru noktadan siteye VPN bağlantısı için verilir.

Daha fazla bilgi şurada bulunabilir
* [Azure portal’ı kullanarak bir sanal ağa yönelik Noktadan Siteye bağlantı yapılandırma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)
* [PowerShell'i kullanarak bir sanal ağa yönelik bir Noktadan Siteye bağlantı yapılandırma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)

#### <a name="multi-site-vpn"></a>Çok siteli VPN
Azure ayrıca günümüzde bir Azure aboneliği için çok siteli VPN bağlantısı oluşturmak için olanağı sunar. Daha önce tek bir abonelik için bir siteden siteye VPN bağlantısı sınırlı. Bu sınırlama tek bir abonelik için çok siteli VPN bağlantılarıyla yerine geçti. Bu şirketler arası yapılandırmalar aracılığıyla belirli bir aboneliği için birden fazla Azure bölgesini yararlanan mümkün kılar.

Daha fazla belge için lütfen bkz [bu makalede][vpn-gateway-create-site-to-site-rm-powershell]

[comment]: <> (ARM belge bağlantı MShermannd Yapılacaklar bulundu)

#### <a name="vnet-to-vnet-connection"></a>Vnet'e bağlantı
Çok siteli VPN kullanarak, ayrı bir Azure sanal ağ her bölgeler yapılandırmanız gerekir. Ancak çok sık farklı bölgelerde yazılım bileşenleri birbirleriyle iletişim gereksinimi var. İdeal olarak bu iletişimi, şirket içi bir Azure bölgesinden ve diğer Azure bölgesini buradan yönlendirileceğini değil. Kısayol için başka bir bölgede bir bölgede başka bir Azure sanal ağa ait bir Azure sanal ağ bağlantısını yapılandırmak için olasılığını barındırılan Azure sunar. Bu işlev, VNet-VNet bağlantı olarak adlandırılır. Bu işlevsellik hakkında daha fazla bilgi şurada bulunabilir: <https://azure.microsoft.com/documentation/articles/vpn-gateway-vnet-vnet-rm-ps/>.

#### <a name="private-connection-to-azure-expressroute"></a>Azure ExpressRoute için özel bağlantı
Microsoft Azure ExpressRoute birlikte bulundurma ortamında veya Azure veri merkezleri ve müşterinin şirket içi altyapı arasında özel bağlantılar oluşturulmasına izin verir. ExpressRoute çeşitli MPLS tarafından sunulan (paket anahtarlamalı) VPN sağlayıcılar veya diğer ağ hizmeti sağlayıcıları. ExpressRoute bağlantıları ortak İnternet üzerinden geçmemektedir. ExpressRoute bağlantısı Internet üzerinden daha yüksek güvenlik, daha fazla güvenilirlik birden çok paralel devreler, yüksek hız ve genel bağlantılara daha düşük gecikme sunar.

Azure ExpressRoute ve burada teklifleri hakkında daha fazla ayrıntı bulabilirsiniz:

* <https://Azure.microsoft.com/documentation/Services/expressroute/>
* <https://Azure.microsoft.com/pricing/details/expressroute/>
* <https://Azure.microsoft.com/documentation/articles/expressroute-faqs/>

Burada açıklandığı gibi bir expressroute bağlantı hattı üzerinden birden çok Azure aboneliği hızlı rota sağlar

* <https://Azure.microsoft.com/documentation/articles/expressroute-howto-linkvnet-ARM/>
* <https://Azure.microsoft.com/documentation/articles/expressroute-howto-circuit-ARM/>

#### <a name="forced-tunneling-in-case-of-cross-premises"></a>Zorlanan tünel şirket içi durumunda
Siteden siteye, noktadan siteye veya ExpressRoute aracılığıyla şirket içi etki alanlarına katılma VM'ler için de bu VM'lerin tüm kullanıcılar için Internet proxy ayarlarını dağıtılmasını emin olmanız gerekir. Varsayılan olarak, bu sanal makineleri veya Internet'e erişmek için bir tarayıcı kullanarak kullanıcıların çalışan yazılımı şirket proxy üzerinden geçecek değil, ancak Azure internet üzerinden doğrudan bağlanabilir. Ancak bile proxy ayarı, yazılım ve hizmetlerinin için proxy denetlemek için sorumluluk olduğundan şirket proxy'si aracılığıyla trafiği yönlendirmek için % 100 bir çözüm değildir. VM içinde çalışan yazılım yapmamanın, ya da bir yönetici ayarlarını yönetir, Internet trafiğini yeniden detoured Azure internet üzerinden doğrudan.

Bu durumu önlemek için zorlamalı tünel şirket içi ve Azure arasında siteden siteye bağlantı yapılandırabilirsiniz. Zorlamalı tüneli özelliğinin ayrıntılı açıklaması burada yayımlanan <https://azure.microsoft.com/documentation/articles/vpn-gateway-forced-tunneling-rm/>

Zorlamalı tünel ExpressRoute ile bir varsayılan rota ExpressRoute BGP eşliği oturumlarını üzerinden reklam müşteriler tarafından etkinleştirilir.

#### <a name="summary-of-azure-networking"></a>Azure ağı özeti
Bu bölümde Azure ağ ilgili birçok önemli noktalar yer. Ana noktaları bir özeti aşağıda verilmiştir:

* Ağ kendi gereksinimlerinize göre ayarlamak için Azure sanal ağlar sağlar
* Azure sanal ağlar VM'ler için IP adresi aralıkları atamak veya VM'ler için sabit IP adresleri atamak için de kullanılabilir
* Siteden siteye veya noktadan siteye bağlantı kurmak için bir Azure sanal ağı oluşturmanız gerekir
* Bir sanal makine dağıtıldıktan sonra artık VM'ye atanan sanal ağ değiştirmek mümkün değil

### <a name="quotas-in-azure-virtual-machine-services"></a>Azure sanal makine Hizmetleri kotaları
Depolama ve ağ altyapısı Hizmetleri, çeşitli Azure altyapısı için çalışan sanal makineler arasında paylaşılır olgu hakkında açık olması gerekir. Ve müşterinin kendi veri merkezlerini'olduğu gibi yalnızca altyapı kaynakların bazıları aşırı sağlama için bir derecede gerçekleşir. Microsoft Azure platformu disk, CPU, ağ ve diğer kotaları kaynak tüketimini sınırlamak için ve tutarlı ve belirleyici performansı korumak için kullanır.  Farklı VM türler (A5, A6, vb.) ağ ve CPU, RAM, disk sayısı için farklı kotalar sahip.

> [!NOTE]
> SAP tarafından desteklenen VM türlerinin CPU ve bellek kaynakları ana bilgisayar düğümlerinde önceden ayrılmış. Bu VM dağıtıldığında, konakta kaynaklarını VM türü tarafından tanımlandığı şekilde kullanılabilir anlamına gelir.
>
>

Her sanal makine boyutu kotaları planlarken ve SAP Azure çözümlerini boyutlandırma dikkate alınmalıdır. VM kotaları açıklanan [burada (Linux)] [ virtual-machines-sizes-linux] ve [burada (Windows)][virtual-machines-sizes-windows].

Açıklanan kota teorik en yüksek değerleri temsil eder.  IOPS sınırı disk başına küçük IOs (8 kb) ile elde edilen ancak büyük IOs (1 Mb) büyük olasılıkla elde değil.  IOPS sınırı tek disk ayrıntı düzeyi üzerinde zorlanır.

Bir SAP sistemi Azure sanal makine hizmetlerini ve özelliklerini veya var olan bir sistemi olup Azure sistemde dağıtmak için farklı şekilde yapılandırılması gerekiyor uygun olup olmadığını karar vermek için bir kaba karar ağacı, aşağıdaki karar ağacı kullanılabilir:

![Azure üzerinde SAP dağıtma yeteneği karar vermek için karar ağacı][planning-guide-figure-700]

**1. adım**: en önemli bilgiler ile başlamak SAP verilen SAP sistemi gereksinimdir. SAP Sistem zaten dağıtılmış şirket içi bir katman 2 yapılandırmasında olsa bile SAP gereksinimleri DBMS ve SAP uygulama bölümlerini ayrılması gerekir. Varolan sistemler için donanım kullanımda genellikle ilgili SAP belirlenen veya varolan SAP kıyaslamaları üzerinde göre tahmini. Sonuçları şurada bulunabilir: <http://global.sap.com/campaigns/benchmark/index.epx>.
Yeni dağıtılan SAP sistemleri için sistem SAP gereksinimlerini belirlemelisiniz boyutlandırma alıştırma çalıştınız.
Azure üzerinde bu blog ve ekli belgeyi SAP boyutlandırma için Ayrıca bkz: <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

**2. adım**: varolan sistemler için DBMS sunucusunda saniye başına g/ç işlemleri ve g/ç birim ölçülen. Yeni planlı sistemler için yeni sistem boyutlandırma alıştırma ayrıca g/ç gereksinimlerinin kabaca fikirleri DBMS tarafında vermesi gerekir. Emin değilseniz, sonuçta bir kavram kanıtı yürütmeniz gerekir.

**3. adım**: Azure farklı VM türler sağlayabilir SAP DBMS sunucusuyla SAP gereksinimini karşılaştırın. SAP hakkında bilgi farklı Azure VM türlerinin SAP notta belgelenen [1928533]. Veritabanı katmanı dağıtımların çoğunluğu ölçeğini olmayan bir SAP NetWeaver sistemi katman olduğundan odağı DBMS VM önce olmalıdır. Buna karşılık, SAP uygulama katmanı dışa genişletilebilir. SAP hiçbiri destekleniyorsa Azure VM türleri gerekli SAP dağıtabilir, planlı SAP sistem iş yükünü Azure üzerinde çalıştırılamaz. Ya da sistem dağıtmak için gerekir, şirket içi veya sistem iş yükü birimi değiştirmeniz gerekir.

**Adım 4**: belirtildiği gibi [burada (Linux)] [ virtual-machines-sizes-linux] ve [burada (Windows)][virtual-machines-sizes-windows], Azure disk başına IOPS kota zorlar Standart depolama veya Premium depolama kullanıp bağımsız. VM türü bağımlı, takılabilir veri diski sayısı değişir. Sonuç olarak, her biri farklı VM türler ile elde edilebilir maksimum IOPS birkaç hesaplayabilirsiniz. Veritabanı dosya düzeni bağımlı, bir konuk işletim sistemi biriminde olmasını diskleri şeritler. Ancak, dağıtılan bir SAP sisteminin geçerli IOPS hacmi Azure ve daha fazla belleğe sahip dengelemek için hiçbir fırsat ise en büyük VM türünü hesaplanan sınırları aşarsa, iş yükü SAP sisteminin ciddi bir şekilde etkilenebilir. Böyle durumlarda, burada Azure sistemde dağıtmamanız noktası isabet.

**5. adım**: dağıtılan şirket içi 2 katmanlı yapılandırmaları olan özellikle SAP içinde büyük olasılıkla sistem Azure üzerinde bir 3 katmanlı yapılandırmasında yapılandırılması gerekebilir sistemleridir. Bu adımda, olup olmadığını bir bileşeni olan ölçeği genişletilemez ve hangi Azure VM farklı teklif CPU ve bellek kaynakları uyar değil SAP uygulama katmanında denetlemeniz gerekir. Gerçekten varsa bu tür bir bileşeni, SAP sistem ve iş yükünü Azure'a dağıtılamıyor. Ancak sistem birden fazla Azure VM'yi SAP uygulama bileşenlerini ölçeklendirebilirsiniz, Azure'da dağıtılabilir.

**Adım 6**: DBMS ve SAP uygulama katmanı bileşenleri Azure Vm'lerde çalıştırılabilir, yapılandırma ile regard için tanımlanmış olması gerekir:

* Azure VM sayısı
* Tek tek bileşenleri için VM türleri
* DBMS yeterli IOPS sağlamak için VM içinde VHD'leri sayısı

## <a name="managing-azure-assets"></a>Azure varlıklarını yönetme
### <a name="azure-portal"></a>Azure Portal
Azure portalında Azure VM dağıtımları yönetmek için üç arabirimi biridir. Azure portalı üzerinden görüntülerden sanal makineleri dağıtma gibi temel yönetim görevleri gerçekleştirilebilir. Ayrıca, depolama hesapları, sanal ağlar ve diğer Azure bileşenleri oluşturulmasını var. de Azure portalında çok iyi işleyebilir görevleri Ancak, şirket içinden Azure'a VHD yüklenmesini veya azure'daki bir VHD kopyalama gibi işlevselliği, üçüncü taraf araçları veya PowerShell veya CLI üzerinden Yönetimi gerektiren görevleri değildir.

![Microsoft Azure portal - sanal makineye genel bakış][planning-guide-figure-800]

[comment]: <> (MSSedusch * < https://azure.microsoft.com/documentation/articles/virtual-networks-create-vnet-arm-pportal/>)
[comment]: <> (MSSedusch * < https://azure.microsoft.com/documentation/articles/virtual-machines-windows-tutorial/>)

Sanal makine örneği için yönetim ve yapılandırma görevlerini Azure portalını mümkündür.

Yeniden başlatmak ve bir sanal makineyi kapatmak makinenin yanı sıra, ayrıca ekleme, detach ve veri diskleri için örnek görüntü hazırlık için yakalama ve sanal makine örneğinin boyutunu yapılandırmak için sanal makine örneğini oluşturun.

Azure portalı VM'ler ve diğer birçok Azure Hizmetleri dağıtmak ve yapılandırmak için temel işlevleri sağlar. Ancak tüm kullanılamaz işlevselliği Azure portal tarafından ele alınmıştır. Azure portalında gibi görevleri gerçekleştirmeniz mümkün değil:

* Azure'a VHD yükleme
* Sanal makineleri kopyalama

[comment]: <> (MShermannd Yapılacaklar Otomasyon hakkında neler SAP VM'ler için hizmet?)
[comment]: <> (Bu sırada olası birden çok VM os MSSedusch dağıtımı)
[comment]: <> (Ayrıca MSSedusch Otomasyon dağıtım ile ilgili herhangi bir türde Azure portalıyla mümkün değil. Birden çok VM komut dosyalı dağıtımı gibi görevleri Azure portalı üzerinden mümkün değildir.)

### <a name="management-via-microsoft-azure-powershell-cmdlets"></a>Microsoft Azure PowerShell cmdlet'leri aracılığıyla Yönetimi
Windows PowerShell yaygın sayıda Azure sistemlerinde dağıtma müşteriler tarafından benimsenen güçlü ve Genişletilebilir bir çerçevedir. Bir masaüstü, dizüstü bilgisayar veya özel yönetim istasyon PowerShell cmdlet'leri yüklendikten sonra PowerShell cmdlet'leri uzaktan çalıştırılabilir.

Bir yerel Masaüstü/dizüstü Azure PowerShell cmdlet'leri ve Azure aboneliği ile kullanımını açıklanan olanlar yapılandırmak nasıl kullanımını etkinleştirmek için işlem [bu makalede][powershell-install-configure].

Yükleme, güncelleştirme ve Azure PowerShell cmdlet'leri de bulunabilir yapılandırma adımları ayrıntılı [Bu bölümde Dağıtım Kılavuzu'nun][deployment-guide-4.1].

Müşteri Deneyimi kadarki PowerShell (PS) kesinlikle daha güçlü bir araç sanal makineleri dağıtmak ve özel VM'ler dağıtımında oluşturmak için adımları olduğunu olmuştur. Azure portalında yapın veya hatta özel olarak Azure dağıtımları yönetmek için PS cmdlet'leri kullanarak yönetim görevleri tamamlamak için SAP örnekleri Azure'da çalışan tüm müşteriler PS cmdlet'leri kullanıyor. 2000'den fazla Windows ilişkili cmdlet'leriyle aynı adlandırma kuralı Azure özgü cmdlet'leri paylaşmak olduğundan, bu cmdlet'leri yararlanmak Windows yöneticileri için kolay bir görev kalır.

Örnek buraya bakın: <http://blogs.technet.com/b/keithmayer/archive/2015/07/07/18-steps-for-end-to-end-iaas-provisioning-in-the-cloud-with-azure-resource-manager-arm-powershell-and-desired-state-configuration-dsc.aspx>

[comment]: <> (MShermannd Yapılacaklar açıklamak sınandığında yeni CLI komutu)
Azure Monitoring uzantısı SAP için dağıtım (bölüm bkz [Azure izlemesi çözümünü SAP] [ planning-guide-9.1] bu belgedeki) PowerShell veya CLI aracılığıyla yalnızca mümkündür. Bu nedenle ayarlamak ve PowerShell veya CLI dağıtırken veya Azure SAP NetWeaver sistemde yönetme yapılandırmak için zorunludur.  

Azure daha fazla işlevsellik sağlayan gibi bir güncelleştirme cmdlet'leri gerektiren eklenecek yeni PS cmdlet'leri adımıdır. Bu nedenle Azure karşıdan sitenin en az bir kez ay denetlemek için bir anlam <https://azure.microsoft.com/downloads/> cmdlet'ler yeni bir sürümü için. Yeni sürümün üstünde eski sürümü yüklü.

PowerShell Azure ile ilgili genel bir listesi için komutları burada denetleyin: <https://docs.microsoft.com/powershell/azure/overview>.

### <a name="management-via-microsoft-azure-cli-commands"></a>Microsoft Azure CLI komutları aracılığıyla Yönetimi
Linux kullanın ve Azure yönetmek isteyen müşteriler için bir seçenek kaynakları Powershell olmayabilir. Microsoft Azure CLI alternatif olarak sunar.
Azure CLI, Azure platformu ile çalışmak için platformlar arası komutları açık kaynak kümesi sağlar. Azure CLI Azure Portalı'nda bulunan aynı işlevlerinin çoğunu sağlar.

Yükleme, yapılandırma ve CLI kullanma hakkında bilgi için bkz: Azure görevleri gerçekleştirmek için komutları

* [Azure CLI'yı yükleme][xplat-cli]
* [Dağıtmak ve Azure Resource Manager şablonları ve Azure CLI kullanarak sanal makineleri yönetme] [../../linux/create-ssh-secured-vm-from-template.md]
* [Mac, Linux ve Windows Azure Resource Manager ile Azure CLI kullanma][xplat-cli-azure-resource-manager]

Ayrıca bölümünü okuyun [Linux VM'ler için Azure CLI] [ deployment-guide-4.5.2] içinde [Dağıtım Kılavuzu'na] [ planning-guide] Azure Monitoring dağıtmak için Azure CLI kullanma SAP için uzantısı.

## <a name="different-ways-to-deploy-vms-for-sap-in-azure"></a>SAP Azure için sanal makineleri dağıtmak için farklı yollar
Bu bölümde, azure'da bir VM'yi dağıtmak için kullanılabilecek çeşitli yöntemler öğreneceksiniz. VHD ve VM'ler için Azure'da işlenmesini yanı sıra ek hazırlık yordamlar bu bölümde ele alınmıştır.

### <a name="deployment-of-vms-for-sap"></a>VM dağıtımı için SAP
Microsoft Azure sanal makineleri ve ilişkili diskleri dağıtmak için birden çok yol sunar. Bu nedenle VM'lerin hazırlıklar dağıtım yöntemine bağlı olarak değişebilir beri farkları anlamak çok önemlidir. Genel olarak, biz aşağıdaki senaryoları göz atın:

#### <a name="4d175f1b-7353-4137-9d2f-817683c26e53"></a>Bir VM şirket içinden Azure'a genelleştirilmiş olmayan bir diskle taşıma
Belirli bir SAP sistemi şirket içinden Azure'a taşımak planlayın. Bu, Azure DBMS veri ve günlük dosyaları ile işletim sistemi, SAP ikili dosyaları ve DBMS ikili dosyaları içeren VHD artı VHD'leri yükleyerek yapılabilir. Tersine için [Senaryo #2 aşağıdaki][planning-guide-5.1.2], ana bilgisayar adı, SAP SID tutmak ve şirket içi ortamda yapılandırılmış olarak Azure VM'yi SAP kullanıcı hesapları. Bu nedenle, görüntünün genelleme gerekli değildir. Bölümlere bakın [VM şirket içinden Azure'a genelleştirilmiş olmayan bir diskle taşıma için hazırlık] [ planning-guide-5.2.1] şirket içi hazırlık adımları ve genelleştirilmiş VMs ya da Azure VHD'leri karşıya yükleme için bu belgenin. Okuma bölüm [Senaryo 3: şirket içi SAP ile genelleştirilmiş Azure VHD kullanarak bir VM taşınan] [ deployment-guide-3.4] içinde [Dağıtım Kılavuzu'na] [ deployment-guide] için Bu tür bir görüntü azure'da dağıtmanın ayrıntılı adımlar.

#### <a name="e18f7839-c0e2-4385-b1e6-4538453a285c"></a>Bir müşteriye özgü görüntüsü olan bir VM dağıtma
İşletim sistemi veya DBMS sürümünüzün belirli düzeltme eki gereksinimleri nedeniyle, Azure Marketi sağlanan görüntüleri gereksinimlerinize değil. Bu nedenle, daha sonra birkaç kez dağıtılabilir kendi özel işletim sistemi/DBMS VM görüntüsü kullanarak bir VM oluşturma gerekebilir. Çoğaltma için özel bir görüntüsünü hazırlamak için kabul edilmesi aşağıdaki öğeleri vardır:

- - -
> ![Windows][Logo_Windows] Windows
>
> Burada Ayrıntılar için bkz: <https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed> Windows ayarlarınızı (örneğin, Windows SID ve ana bilgisayar adı) soyutlanır / şirket içi VM üzerinde genelleştirilmiş olması gerekir sysprep komutu.
>
>
> ![Linux][Logo_Linux] Linux
>
> Bu makaleler için açıklanan adımları izleyin [SUSE][virtual-machines-linux-create-upload-vhd-suse], [Red Hat][virtual-machines-linux-redhat-create-upload-vhd], veya [Oracle Linux] [ virtual-machines-linux-create-upload-vhd-oracle], Azure'a karşıya yüklenecek bir VHD hazırlamak için.
>
>

- - -
(Özellikle 2 katmanlı sistemler için), şirket içi VM'deki SAP içerik zaten yüklediyseniz, örnek aracılığıyla Azure VM dağıtımı yeniden adlandırdıktan sonra SAP yazılım sağlama Yöneticisi tarafından desteklenen yordamı SAP sistem ayarlarını uyarlayabilirsiniz (SAP Not [1619720]). Bölümlere bakın [için SAP müşteriye özgü görüntüsü olan bir VM dağıtımı için hazırlık] [ planning-guide-5.2.2] ve [şirket içi bir VHD'den Azure karşıya] [ planning-guide-5.3.2]şirket içi hazırlık adımları ve genelleştirilmiş bir Azure VM'ye karşıya yükleme için bu belgenin. Okuma bölüm [Senaryo 2: özel bir görüntü ile bir VM için SAP dağıtma] [ deployment-guide-3.3] içinde [Dağıtım Kılavuzu'na] [ deployment-guide] dağıtma ayrıntılı adımlar için Bu tür bir görüntüde Azure.

#### <a name="deploying-a-vm-out-of-the-azure-marketplace"></a>Azure Market dışında bir VM dağıtma
Bir Microsoft kullanmak istediğiniz veya üçüncü taraf VM görüntüsü, VM dağıtmak için Azure Marketi'nden sağlanan. Azure VM dağıtımı sonra aynı kılavuzları ve bir şirket içi ortamda yaptığınız gibi SAP yazılım ve/veya VM içinde DBMS yüklemek için Araçlar izleyin. Bölüm daha ayrıntılı dağıtımı açıklaması için lütfen bkz [Senaryo 1: SAP Azure Market dışında bir VM dağıtma] [ deployment-guide-3.2] içinde [Dağıtım Kılavuzu'na][deployment-guide].

### <a name="6ffb9f41-a292-40bf-9e70-8204448559e7"></a>Azure için sanal makineleri SAP ile hazırlama
Sanal makineleri emin olmak için gereken Azure'a karşıya yüklemeden önce sanal makineleri ve VHD belirli gereksinimleri karşılamak. Kullanılan dağıtım yöntemine bağlı olarak küçük farklılıklar vardır.

#### <a name="1b287330-944b-495d-9ea7-94b83aff73ef"></a>Bir VM şirket içinden Azure'a genelleştirilmiş olmayan bir diskle taşıma için hazırlama
Genel dağıtım yöntemi bir SAP sistemi şirket içinden Azure'a çalıştıran mevcut bir VM'yi taşımaktır. Bu VM ve VM SAP sistemde, aynı ana bilgisayar adı ve büyük olasılıkla aynı SAP SID kullanarak Azure'da yalnızca çalıştırmalısınız. Bu durumda konuk işletim sistemi, VM için birden çok dağıtım genelleştirilmiş değil. Şirket içi ağ Azure'da genişlettiyseniz (bölüm bakın [şirket içi - tek dağıtımını ya da Azure ile şirket içi ağınıza tam olarak tümleştirilmiş zorunluluğu içine birden çok SAP VM] [ planning-guide-2.2] bu belgedeki), sonra bile aynı etki alanı hesapları bu şirket içi önce kullanılan VM dahilinde kullanılabilir.

Kendi Azure VM Disk hazırlanırken gereksinimleri şunlardır:

* İlk olarak işletim sistemini içeren VHD yalnızca en büyük boyutu 127 GB olabilir. Bu sınırlama Mart 2015 sonunda ortadan. Artık başka bir Azure depolama VHD de barındırılan olarak işletim sistemini içeren VHD boyutu 1 TB'ye kadar olabilir.
* Sabit VHD biçiminde olması gerekir. Dinamik VHD veya VHDx biçiminde VHD'ler henüz Azure üzerinde desteklenmez. Dinamik VHD VHD PowerShell cmdlet'leri ile veya CLI yüklediğinizde statik VHD'leri dönüştürülür.
* VM'ye bağlı olduğundan ve bir sabit VHD biçiminde de VM gerek Azure'da yeniden takılmasını VHD'ler. Lütfen okuyun [bu makalede (Linux)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-linux) ve [bu makalede (Windows)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-windows) veri diski boyutu sınırları için. Dinamik VHD VHD PowerShell cmdlet'leri ile veya CLI yüklediğinizde statik VHD'leri dönüştürülür.
* Microsoft destek veya hangi hizmetlerin ve uygulamaların VM dağıtılana kadar çalıştırmak için bağlamı olarak atanabilir ve daha uygun kullanıcılar tarafından kullanılan yönetici ayrıcalıklarına sahip başka bir yerel hesap kullanılabilir ekleyin.
* Bir yalnızca bulut dağıtım senaryosu kullanma örneği için (bölüm bakın [salt bulut - şirket içi müşteri ağdaki bağımlılıklar olmadan sanal makine dağıtımlarının Azure içine] [ planning-guide-2.1] bu belgenin) Azure diski Azure'da dağıtıldıktan sonra bu dağıtım yöntemi ile birlikte, etki alanı hesapları çalışmayabilir. Bu, özellikle DBMS veya SAP uygulamaları gibi hizmetlerini çalıştırmak için kullanılan hesaplar için geçerlidir. Bu nedenle bu tür etki alanı hesapları VM yerel hesaplarla değiştirin ve VM şirket içi etki alanı hesaplarında silmek gerekir. Şirket içi etki alanı kullanıcıları VM görüntüsündeki tutma değil bir sorun VM şirketler arası senaryoda bölümde açıklandığı gibi dağıtıldığında [şirket içi - tek dağıtımını ya da Azure ile tam olarak olma gereksinimi içine birden çok SAP VM Şirket içi ağınıza tümleşik] [ planning-guide-2.2] bu belgedeki.
* Yalnızca bulut senaryolarda dağıtılması için sistem şirket içi ve bu sanal makineleri çalıştıran beklenen olduğunda etki alanı hesapları DBMS oturumlar veya kullanıcılar kullanıldıysa, etki alanı kullanıcıları silinmesi gerekir. Yerel yönetici artı başka bir VM yerel kullanıcı eklendiğini bir oturum açma/kullanıcı olarak yöneticileri olarak DBMS içine emin olmanız gerekir.
* Bu belirli dağıtım senaryosu için gerekebilecek gibi diğer yerel hesaplar ekleyin.

- - -
> ![Windows][Logo_Windows] Windows
>
> Bu senaryoda VM hiçbir Genelleştirme (sysprep) karşıya yükleyin ve Azure üzerinde VM dağıtmak için gereklidir.
> D:\ kullanılmıyorsa bu sürücüye emin olun.
> Bölümde açıklandığı gibi bağlı diskler için disk otomatik bağlama ayarlamak [bağlı diskler için otomatik bağlama ayarı] [ planning-guide-5.5.3] bu belgedeki.
>
> ![Linux][Logo_Linux] Linux
>
> Bu senaryoda hiçbir Genelleştirme (waagent-deprovision) VM karşıya yükleyin ve Azure üzerinde VM dağıtmak için gereklidir.
> / Mnt/kaynak kullanılmaz ve tüm diskleri UUID bağlandığından emin olun. İşletim sistemi diski için önyükleme yükleyicisi giriş de UUID tabanlı bağlama gösterdiğinden emin olmalısınız.
>
>

- - -
#### <a name="57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3"></a>SAP için müşteriye özgü görüntüsü olan bir VM dağıtımı için hazırlama
Genelleştirilmiş bir işletim sistemi içeren VHD dosyaları kapsayıcıları Azure depolama hesapları veya yönetilen Disk görüntü olarak depolanır. VHD veya yönetilen Disk görüntü kaynağı dağıtım şablonu dosyalarınızı olarak bölümde açıklandığı gibi başvurarak bu tür bir görüntüden yeni bir VM dağıtabilirsiniz [Senaryo 2: özel bir görüntü ile bir VM için SAP dağıtma] [ deployment-guide-3.3], [Dağıtım Kılavuzu'na][deployment-guide].

Kendi Azure VM görüntüsü hazırlanırken gereksinimleri şunlardır:

* İlk olarak işletim sistemini içeren VHD yalnızca en büyük boyutu 127 GB olabilir. Bu sınırlama Mart 2015 sonunda ortadan. Artık başka bir Azure depolama VHD de barındırılan olarak işletim sistemini içeren VHD boyutu 1 TB'ye kadar olabilir.
* Sabit VHD biçiminde olması gerekir. Dinamik VHD veya VHDx biçiminde VHD'ler henüz Azure üzerinde desteklenmez. Dinamik VHD VHD PowerShell cmdlet'leri ile veya CLI yüklediğinizde statik VHD'leri dönüştürülür.
* VM'ye bağlı olduğundan ve bir sabit VHD biçiminde de VM gerek Azure'da yeniden takılmasını VHD'ler. Lütfen okuyun [bu makalede (Linux)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-linux) ve [bu makalede (Windows)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-windows) veri diski boyutu sınırları için. Dinamik VHD VHD PowerShell cmdlet'leri ile veya CLI yüklediğinizde statik VHD'leri dönüştürülür.
* VM kullanıcılar bir yalnızca bulut senaryosunda var olmayan tüm etki alanı kullanıcıları kayıtlı olduğundan (bölüm bakın [salt bulut - şirket içi müşteri ağdaki bağımlılıklar olmadan sanal makine dağıtımlarının Azure içine] [ planning-guide-2.1] bu belgenin), böyle bir etki alanı hesapları görüntüsünü Azure'da dağıtıldığında çalışmayabilir kullanarak hizmetleri. Bu, özellikle DBMS veya SAP uygulamaları gibi hizmetlerini çalıştırmak için kullanılan hesaplar için geçerlidir. Bu nedenle bu tür etki alanı hesapları VM yerel hesaplarla değiştirin ve VM şirket içi etki alanı hesaplarında silmek gerekir. Şirket içi etki alanı kullanıcıları VM görüntüsündeki tutma olabilir bir sorun VM şirketler arası senaryoda bölümde açıklandığı gibi dağıtıldığında [şirket içi - tek dağıtımını ya da Azure olma gereksinimi ile içine birden çok SAP VM Şirket içi ağınıza tam olarak tümleşiktir] [ planning-guide-2.2] bu belgedeki.
* Microsoft destek sorun araştırmalar veya hangi hizmetlerin ve uygulamaların VM dağıtılana kadar çalıştırmak için bağlamı olarak atanabilir ve daha uygun kullanıcılar tarafından kullanılan yönetici ayrıcalıklarına sahip başka bir yerel hesap kullanılabilir ekleyin.
* Yalnızca bulut dağıtımları ve burada etki alanı hesapları DBMS oturumlar veya kullanıcılar içi sistem çalışırken kullanılan, etki alanı kullanıcıları silinmesi gerekir. Yerel yönetici artı başka bir VM yerel kullanıcı eklendiğini bir oturum açma/kullanıcı yöneticileri olarak DBMS olarak emin olmanız gerekir.
* Bu belirli dağıtım senaryosu için gerekebilecek gibi diğer yerel hesaplar ekleyin.
* SAP NetWeaver yüklemesini ve ana bilgisayar adının Azure dağıtım noktasında orijinal adı yeniden adlandırma görüntüsü içeriyorsa, SAP yazılım sağlama Yöneticisi DVD en son sürümlerini şablonuna eklenecek önerilir, olasıdır. Bu, kolayca değiştirilen hostname uyum ve/veya yeni bir kopyasını kullanmaya hemen dağıtılan VM görüntüsü SAP sisteminde SID'si değiştirmek için sağlanan SAP yeniden adlandırma işlevselliği kullanmanıza olanak sağlar.

- - -
> ![Windows][Logo_Windows] Windows
>
> Sürücü D:\ değil bölümde açıklandığı gibi disklerdeki kümesi disk otomatik bağlama kullanıldığından emin olun [bağlı diskler için otomatik bağlama ayarı] [ planning-guide-5.5.3] bu belgedeki.
>
> ![Linux][Logo_Linux] Linux
>
> / Mnt/kaynak kullanılmaz ve tüm diskleri UUID bağlandığından emin olun. İşletim sistemi diski için önyükleme yükleyicisi giriş de UUID tabanlı bağlama gösterdiğinden emin olun.
>
>

- - -
* SAP GUI (için yönetim ve Kurulum amacıyla) bu tür bir şablonda önceden yüklenebilir.
* Bu yazılım VM yeniden adlandırma ile çalışabilirsiniz sürece sanal makineleri şirket içi senaryolarda başarılı bir şekilde çalıştırmak için gereken diğer yazılım yüklenebilir.

VM yeterince genel ve hesapları/kullanıcılar hedeflenen Azure dağıtım senaryosunda kullanılamaz sonunda bağımsız olarak hazırlanmış varsa, bu tür bir görüntüyü genelleme son hazırlık adımı yürütülür.

##### <a name="generalizing-a-vm"></a>Bir VM genelleme
- - -
> ![Windows][Logo_Windows] Windows
>
> VM bir yönetici hesabıyla oturum açmak için son adım olacaktır. Bir Windows komut penceresinde açın *yönetici*. İçin %Windir%\Windows\System32\sysprep gidin ve sysprep.exe yürütün.
> Küçük bir pencere görüntülenir. Denetlenecek önemlidir **Generalize** seçeneği (varsayılan olarak atanmamış checked) ve 'Kapatma' için 'Yeniden başlatma' varsayılan kapatma seçeneğini değiştirin. Bu yordam, sysprep işleminin yürütülen şirket içi bir VM konuk işletim sistemini olduğunu varsayar.
> Zaten Azure'da çalışan bir VM ile yordamı gerçekleştirmek istiyorsanız, açıklanan adımları izleyin [bu makalede](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/capture-image-resource).
>
> ![Linux][Logo_Linux] Linux
>
> [Resource Manager şablonu olarak kullanmak üzere Linux sanal makinesi yakalama][capture-image-linux-step-2-create-vm-image]
>
>

- - -
### <a name="transferring-vms-and-vhds-between-on-premises-to-azure"></a>Şirket içi Azure arasında sanal makineleri ve VHD aktarma
VM görüntüleri ve diskleri Azure'a yüklenmesini Azure portalı üzerinden mümkün olmadığından, Azure PowerShell cmdlet'lerini veya CLI kullanmanız gerekir. Diğer bir olasılık 'AzCopy' aracının kullanımı içindir. Aracı VHD'leri (her iki yönde) şirket içi ve Azure arasında kopyalayabilirsiniz. Ayrıca, Azure bölgeler arasında VHD'ler de kopyalayabilirsiniz. Lütfen bakın [bu belgeleri] [ storage-use-azcopy] karşıdan yükleme ve AzCopy kullanımı.

Üçüncü bir alternatif çeşitli üçüncü taraf GUI tabanlı araçlar kullanmak olabilir. Bununla birlikte, lütfen bu araçları Azure sayfa Bloblarını destekliyorsanız emin olun. Bizim amacıyla Azure sayfa Blob deposu kullanmamız gerekiyor (farklar aşağıda açıklanmıştır: <https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>). Ayrıca Azure tarafından sağlanan araçları, VM'ler ve karşıya yüklenecek gereken VHD'ler sıkıştırma içinde çok verimlidir. Bu önemlidir, çünkü bu verimliliği sıkıştırma (yine de karşıya yükleme bağlantı bağlı olarak, şirket içi tesis ve hedeflenen Azure dağıtım bölge internet'e değişir) karşıya yükleme süresini azaltır. Bu, VM veya Avrupa konum bir VHD'den merkezleri aynı VM'ler/VHD'ler Avrupa Azure karşıya daha uzun sürer ABD göre Azure verileri karşıya veri merkezleri Orta bir varsayılır.

#### <a name="a43e40e6-1acc-4633-9816-8f095d5a7b6a"></a>Şirket içi bir VHD'den Azure karşıya yükleme
Şirket içi ağ üzerinden bir var olan VM veya VHD yüklemek için bu tür bir VM veya VHD bölümde listelenen gereksinimlerini karşılamak gerekir [VM şirket içinden Azure'a genelleştirilmiş olmayan bir diskle taşıma için hazırlık] [ planning-guide-5.2.1] bu belgenin.

Bu tür bir VM genelleştirilmiş gerekmez ve durum ve şirket içi tarafında kapatma sonrasında sahip şekli yüklenir. Aynı herhangi bir işletim sistemini içermeyen ek VHD'ler için geçerlidir.

##### <a name="uploading-a-vhd-and-making-it-an-azure-disk"></a>Bir VHD yüklemek ve bir Azure diski yapma
Bu durumda olan veya olmayan bir işletim sisteminde, bir VHD yüklemek ve bir veri diski olarak bir VM takmaya veya işletim sistemi diski olarak kullanmak istiyoruz. Bu çok adımlı bir işlemdir

**PowerShell**

* Oturum açtığınızda, aboneliğinizle *Login-AzureRmAccount*
* Bağlamına sahip abonelik *Set-AzureRmContext* ve parametre Subscriptionıd veya varlığıyla SubscriptionName - <https://docs.microsoft.com/powershell/module/azurerm.profile/set-azurermcontext>
* VHD ile karşıya *Ekle AzureRmVhd* bir Azure depolama hesabı - bkz <https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvhd>
* (İsteğe bağlı) VHD ile yönetilen bir Disk oluşturmak *yeni AzureRmDisk* -bkz <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermdisk>
* VHD veya yönetilen diskle yeni bir VM yapılandırma, işletim sistemi diski koymak *kümesi AzureRmVMOSDisk* -bkz <https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk>
* VM yapılandırma ile yeni bir VM oluşturun *New-AzureRmVM* -bkz <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm>
* Yeni bir VM ile bir veri diski Ekle *Ekle AzureRmVMDataDisk* -bkz <https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmdatadisk>

**Azure CLI 2.0**

* Oturum açtığınızda, aboneliğinizle *az oturum açma*
* Aboneliğinizi seçin *az hesabı kümesi--abonelik`<subscription name or id`>*
* VHD ile karşıya *az depolama blob karşıya yükleme* -bkz [Azure Storage ile Azure CLI kullanma][storage-azure-cli]
* (İsteğe bağlı) VHD ile yönetilen bir Disk oluşturmak *az disketi* -https://docs.microsoft.com/cli/azure/disk#az_disk_create bakın
* Karşıya yüklenen VHD veya yönetilen Disk ile işletim sistemi diski olarak belirterek yeni bir VM oluşturmak *az vm oluşturma* ve parametre *--attach-os-disk*
* Yeni bir VM ile bir veri diski Ekle *az vm diskini* ve parametre *--yeni*

**Şablon**

* VHD Powershell veya Azure CLI ile karşıya yükle
* (İsteğe bağlı) VHD Powershell, Azure CLI veya Azure portal ile yönetilen bir Disk oluşturun
* VHD gösterildiği gibi başvuran bir JSON şablonu ile VM dağıtmak [Bu örnek JSON şablonunu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json) ya da gösterildiği gibi yönetilen diskleri kullanarak [Bu örnek JSON şablonunu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/sap-2-tier-user-disk-md/azuredeploy.json).

#### <a name="deployment-of-a-vm-image"></a>Bir VM görüntüsü dağıtımını
Böyle bir VM veya VHD gereksinim bölümde listelenen gereksinimlerini karşılamak bir Azure VM görüntü olarak kullanmak için şirket içi ağ üzerinden bir var olan VM veya VHD yüklemeye [için SAP müşteriye özgü görüntüsü olan bir VM dağıtımı için hazırlık] [ planning-guide-5.2.2] bu belgenin.

* Kullanım *sysprep* Windows veya *waagent-deprovision* , VM - generalize Linux bkz [Sysprep Teknik Başvurusu](https://technet.microsoft.com/library/cc766049.aspx) Windows için veya [yakalama bir Resource Manager şablonu olarak kullanmak üzere Linux sanal makine] [ capture-image-linux-step-2-create-vm-image] Linux için
* Oturum açtığınızda, aboneliğinizle *Login-AzureRmAccount*
* Bağlamına sahip abonelik *Set-AzureRmContext* ve parametre Subscriptionıd veya varlığıyla SubscriptionName - <https://docs.microsoft.com/powershell/module/azurerm.profile/set-azurermcontext>
* VHD ile karşıya *Ekle AzureRmVhd* bir Azure depolama hesabı - bkz <https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvhd>
* (İsteğe bağlı) VHD ile yönetilen bir Disk görüntüsü oluşturmak *yeni AzureRmImage* -bkz <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermimage>
* Yeni bir VM yapılandırma, işletim sistemi diski ayarlayın
  * VHD ile *kümesi AzureRmVMOSDisk - SourceImageUri - CreateOption fromImage* -bkz <https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk>
  * Disk görüntüsü yönetilen *kümesi AzureRmVMSourceImage* -bkz <https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmsourceimage>
* VM yapılandırma ile yeni bir VM oluşturun *New-AzureRmVM* -bkz <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm>

**Azure CLI 2.0**

* Kullanım *sysprep* Windows veya *waagent-deprovision* , VM - generalize Linux bkz [Sysprep Teknik Başvurusu](https://technet.microsoft.com/library/cc766049.aspx) Windows için veya [yakalama bir Resource Manager şablonu olarak kullanmak üzere Linux sanal makine] [ capture-image-linux-step-2-create-vm-image] Linux için
* Oturum açtığınızda, aboneliğinizle *az oturum açma*
* Aboneliğinizi seçin *az hesabı kümesi--abonelik`<subscription name or id`>*
* VHD ile karşıya *az depolama blob karşıya yükleme* -bkz [Azure Storage ile Azure CLI kullanma][storage-azure-cli]
* (İsteğe bağlı) VHD ile yönetilen bir Disk görüntüsü oluşturmak *az görüntü oluşturma* -https://docs.microsoft.com/cli/azure/image#az_image_create bakın
* Karşıya yüklenen VHD veya yönetilen Disk görüntüsü ile işletim sistemi diski olarak belirterek yeni bir VM oluşturmak *az vm oluşturma* ve parametre *--görüntüsü*

**Şablon**

* Kullanım *sysprep* Windows veya *waagent-deprovision* , VM - generalize Linux bkz [Sysprep Teknik Başvurusu](https://technet.microsoft.com/library/cc766049.aspx) Windows için veya [yakalama bir Resource Manager şablonu olarak kullanmak üzere Linux sanal makine] [ capture-image-linux-step-2-create-vm-image] Linux için
* VHD Powershell veya Azure CLI ile karşıya yükle
* (İsteğe bağlı) Powershell, Azure CLI veya Azure portal ile VHD'den yönetilen bir Disk görüntüsü oluşturma
* VHD görüntüsü gösterildiği gibi başvuran bir JSON şablonu ile VM dağıtmak [Bu örnek JSON şablonunu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/sap-2-tier-user-image/azuredeploy.json) veya yönetilen Disk görüntüsü gösterildiği gibi kullanarak [Bu örnek JSON şablonunu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json).

#### <a name="downloading-vhds-or-managed-disks-to-on-premises"></a>İndirme VHD'leri veya şirket içi yönetilen diskleri
Bir hizmet olarak Azure altyapısı yalnızca VHD ve SAP yükleyebildiğini olma tek yönlü bir yol değil sistemler. SAP taşıyabilirsiniz Azure sistemlerden de şirket içi dünyaya yedekleyin.

Karşıdan yükleme süresi sırasında VHD'leri veya yönetilen diskleri etkin olamaz. Takılı diskler Vm'lere yüklerken, bile, VM kapatılır ve serbest gerekiyor. Yalnızca içerik veritabanı karşıdan yüklemek isterseniz, ardından yeni bir sistemi ayarlamak için kullanılması gereken şirket içi ve kabul edilebilir ise, karşıdan yükleme süresini ve Azure sisteminde hala yeni sistem kurulumu sırasında çalışır durumda , bir diske sıkıştırılmış veritabanı yedeklemesi gerçekleştirerek uzun bir kesinti süresini önler ve da işletim sistemi temel VM indirmek yerine bu disk hemen indirin.

#### <a name="powershell"></a>PowerShell

  * Yönetilen bir Disk indirme  
  İlk yönetilen diskin temel blob erişmek gerekir. Ardından yeni bir depolama hesabı için temel alınan blob kopyalama ve bu depolama hesabından blob indirin.

  ```powershell
  $access = Grant-AzureRmDiskAccess -ResourceGroupName <resource group> -DiskName <disk name> -Access Read -DurationInSecond 3600
  $key = (Get-AzureRmStorageAccountKey -ResourceGroupName <resource group> -Name <storage account name>)[0].Value
  $destContext = (New-AzureStorageContext -StorageAccountName <storage account name -StorageAccountKey $key)
  Start-AzureStorageBlobCopy -AbsoluteUri $access.AccessSAS -DestContainer <container name> -DestBlob <blob name> -DestContext $destContext
  # Wait for blob copy to finish
  Get-AzureStorageBlobCopyState -Container <container name> -Blob <blob name> -Context $destContext
  Save-AzureRmVhd -SourceUri <blob in new storage account> -LocalFilePath <local file path> -StorageKey $key
  # Wait for download to finish
  Revoke-AzureRmDiskAccess -ResourceGroupName <resource group> -DiskName <disk name>
  ```

  * Bir VHD indirme  
  SAP sistem durdurulur ve VM'yi kapatın sonra şirket içi world VHD diskleri indirmek için şirket içi hedef kaydetme AzureRmVhd PowerShell cmdlet'ini kullanabilirsiniz. Bunu yapmak için 'depolama alanında bölüm' bulabileceğiniz VHD URL'sini gerekir (depolama hesabı ve VHD oluşturulduğu depolama kapsayıcısı gezinmek için gereklidir) Azure portal ve, VHD'yi burada kopyalanacağı bilmeniz gerekir.

  Ardından yalnızca SourceUri parametre indirmek için VHD ve LocalFilePath URL'sini (adı dahil) VHD fiziksel konumu olarak tanımlayarak komutu yararlanabilirsiniz. Komut gibi görünebilir:

  ```powerhell
  Save-AzureRmVhd -ResourceGroupName <resource group name of storage account> -SourceUri http://<storage account name>.blob.core.windows.net/<container name>/sapidedata.vhd -LocalFilePath E:\Azure_downloads\sapidesdata.vhd
  ```

  Kaydet-AzureRmVhd cmdlet daha fazla ayrıntı için lütfen burayı <https://docs.microsoft.com/powershell/module/azurerm.compute/save-azurermvhd>.

#### <a name="cli-20"></a>CLI 2.0
  * Yönetilen bir Disk indirme  
  İlk yönetilen diskin temel blob erişmek gerekir. Ardından yeni bir depolama hesabı için temel alınan blob kopyalama ve bu depolama hesabından blob indirin.
  ```
  az disk grant-access --ids "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" --duration-in-seconds 3600
  az storage blob download --sas-token "<sas token>" --account-name <account name> --container-name <container name> --name <blob name> --file <local file>
  az disk revoke-access --ids "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>"
  ```

  * Bir VHD indirme   
  SAP sistem durdurulur ve VM'yi kapatın sonra Azure CLI komutunu kullanabilirsiniz _azure depolama blob yükleme_ VHD indirmek için şirket içi hedef diskleri şirket içi dünyasına destekleyen. Adını ve hangi yapabilecekleriniz VHD kapsayıcı, bunu yapabilmek içinde 'depolama bölümünü' Azure Portalı'nı (depolama hesabı ve VHD oluşturulduğu depolama kapsayıcısı gezinmek için gereklidir) ve VHD olması gereken yerde bilmeniz Bul copi Ed için.

  Ardından yalnızca VHD kapsayıcısı ve parametreleri blob yükleme ve hedef (adı dahil) VHD fiziksel hedef konum olarak tanımlayarak komutu yararlanabilirsiniz. Komut gibi görünebilir:

  ```
  az storage blob download --name <name of the VHD to download> --container-name <container of the VHD to download> --account-name <storage account name of the VHD to download> --account-key <storage account key> --file <destination of the VHD to download>
  ```

### <a name="transferring-vms-and-disks-within-azure"></a>VM'ler ve Azure içinde diskleri aktarma
#### <a name="copying-sap-systems-within-azure"></a>Azure içinde SAP sistemleri kopyalama
Büyük olasılıkla bir SAP sistem ya da bir SAP uygulama katmanı destekleyen bile bir adanmış DBMS sunucusu OS ikililerini veya SAP veritabanına veri ve günlük dosyalarını içeren birden çok disk oluşur. Diskleri kopyalama Azure işlevselliğini ne diskleri yerel diske kaydetme Azure işlevselliğini anlık görüntü eşzamanlı olarak birden çok disk misiniz bir eşitleme mekanizması vardır. Bu nedenle, bu aynı VM karşı bağlandığından olsa bile kopyalanan veya kaydedilmiş disklerin durumunu farklı olacaktır. Başka bir deyişle, farklı veri ve farklı diskler bulunan logfile(s) sahip olmanın somut durumda uçtaki veritabanı tutarsız olabilir.

**Sonuç: kopyalama veya SAP sistem yapılandırmasının bir parçası olan diskleri kaydetmek için SAP sistem durdurun ve ayrıca dağıtılan VM kapatma gerek gerekir. Ancak bundan sonra kopyalama veya ya da Azure veya şirket içi SAP sistemine bir kopyasını oluşturmak için bir disk kümesi indirin.**

Veri diskleri Azure depolama hesabınız VHD dosyaları olarak depolanır ve bir sanal makineye doğrudan bağlı veya bir görüntü olarak kullanılabilir. Bu durumda, VHD'yi sanal makineye bağlı önce başka bir konuma kopyalanır. Azure'da VHD dosyasının tam adı Azure içinde benzersiz olmalıdır. Zaten daha önce belirtildiği gibi ad tür benzer bir üç bölümlük adı şudur:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

Veri diskleri yönetilen diskleri de olabilir. Bu durumda, yönetilen Disk sanal makineye bağlı önce yönetilen yeni bir Disk oluşturmak için kullanılır. Yönetilen Disk adı bir kaynak grubu içinde benzersiz olmalıdır.

##### <a name="powershell"></a>PowerShell
Bir VHD gösterildiği gibi kopyalamak için Azure PowerShell cmdlet'lerini kullanabilirsiniz [bu makalede][storage-powershell-guide-full-copy-vhd]. Yeni bir yönetilen Disk oluşturmak için yeni AzureRmDiskConfig ve yeni AzureRmDisk aşağıdaki örnekte gösterildiği gibi kullanın.

```powershell
$config = New-AzureRmDiskConfig -CreateOption Copy -SourceUri "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" -Location <location>
New-AzureRmDisk -ResourceGroupName <resource group name> -DiskName <disk name> -Disk $config
```

##### <a name="cli-20"></a>CLI 2.0
Azure CLI gösterildiği gibi bir VHD kopyalamak için kullanabileceğiniz [bu makalede][storage-azure-cli-copy-blobs]. Yeni bir yönetilen Disk oluşturmak için kullanmak *az disketi* aşağıdaki örnekte gösterildiği gibi.

```
az disk create --source "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" --name <disk name> --resource-group <resource group name> --location <location>
```

##### <a name="azure-storage-tools"></a>Azure Storage araçları
* <http://storageexplorer.com/>

Azure depolama gezginleri Professional sürümleri şurada bulunabilir:

* <http://www.cerebrata.com/>
* <http://clumsyleaf.com/products/cloudxplorer>

Bir VHD'nin depolama hesabındaki kopyasını yalnızca birkaç saniye (SAN donanım anlık görüntüleri yavaş kopyalama ve kopyalama ile yazma oluşturma benzer) alan bir işlemdir. Bir kopyasını oluşturduktan sonra VHD dosyasının bir sanal makineye ekleyebilir veya sanal makinelere VHD kopyalarını eklemek için bir görüntü olarak kullanın.

##### <a name="powershell"></a>PowerShell
```powershell
# attach a vhd to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -VhdUri <path to vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption attach
$vm | Update-AzureRmVM

# attach a managed disk to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -ManagedDiskId <managed disk id> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption attach
$vm | Update-AzureRmVM

# attach a copy of the vhd to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name <disk name> -VhdUri <new path of vhd> -SourceImageUri <path to image vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption fromImage
$vm | Update-AzureRmVM

# attach a copy of the managed disk to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$diskConfig = New-AzureRmDiskConfig -Location $vm.Location -CreateOption Copy -SourceUri <source managed disk id>
$disk = New-AzureRmDisk -DiskName <disk name> -Disk $diskConfig -ResourceGroupName <resource group name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Caching <caching option> -Lun <lun, for example 0> -CreateOption attach -ManagedDiskId $disk.Id
$vm | Update-AzureRmVM
```
##### <a name="cli-20"></a>CLI 2.0
```

# attach a vhd to a vm
az vm unmanaged-disk attach --resource-group <resource group name> --vm-name <vm name> --vhd-uri <path to vhd>

# attach a managed disk to a vm
az vm disk attach --resource-group <resource group name> --vm-name <vm name> --disk <managed disk id>

# attach a copy of the vhd to a vm
# this scenario is currently not possible with Azure CLI. A workaround is to manually copy the vhd to the destination.

# attach a copy of a managed disk to a vm
az disk create --name <new disk name> --resource-group <resource group name> --location <location of target virtual machine> --source <source managed disk id>
az vm disk attach --disk <new disk name or managed disk id> --resource-group <resource group name> --vm-name <vm name> --caching <caching option> --lun <lun, for example 0>
```

#### <a name="9789b076-2011-4afa-b2fe-b07a8aba58a1"></a>Azure depolama hesapları arasında diskleri kopyalama
Bu görev Azure portalı üzerinde gerçekleştirilemiyor. Azure PowerShell cmdlet'leri, Azure CLI veya bir üçüncü taraf depolama tarayıcı kullanabilirsiniz. PowerShell cmdlet'lerini veya CLI komutları oluşturabilir ve zaman uyumsuz olarak BLOB Depolama hesapları ve Azure abonelik içindeki bölgeler arasında kopyalama becerisini dahil BLOB'lar yönetebilirsiniz.

##### <a name="powershell"></a>PowerShell
Bu gibi durumlarda, VHD ayrıca abonelikler arasında kopyalayabilirsiniz. Daha fazla bilgi için okuma [bu makalede][storage-powershell-guide-full-copy-vhd].

PS cmdlet mantığı temel akışı şu şekildedir:

* İçin bir depolama hesabı bağlamını oluşturun **kaynak** depolama hesabıyla *yeni AzureStorageContext* -bkz <https://msdn.microsoft.com/library/dn806380.aspx>
* İçin bir depolama hesabı bağlamını oluşturun **hedef** depolama hesabıyla *yeni AzureStorageContext* -bkz <https://msdn.microsoft.com/library/dn806380.aspx>
* Kopya ile Başlat

```powershell
Start-AzureStorageBlobCopy -SrcBlob <source blob name> -SrcContainer <source container name> -SrcContext <variable containing context of source storage account> -DestBlob <target blob name> -DestContainer <target container name> -DestContext <variable containing context of target storage account>
```

* Döngü ile kopya durumunu denetleme

```powershell
Get-AzureStorageBlobCopyState -Blob <target blob name> -Container <target container name> -Context <variable containing context of target storage account>
```

* Yeni VHD yukarıda açıklandığı gibi bir sanal makineye Ekle.

Örnekler için bkz [bu makalede][storage-powershell-guide-full-copy-vhd].

##### <a name="cli-20"></a>CLI 2.0
* Kopya ile Başlat

```
az storage blob copy start --source-blob <source blob name> --source-container <source container name> --source-account-name <source storage account name> --source-account-key <source storage account key> --destination-container <target container name> --destination-blob <target blob name> --account-name <target storage account name> --account-key <target storage account name>
```

* Durumu kontrol edin döngü ile kopyalama

```
az storage blob show --name <target blob name> --container <target container name> --account-name <target storage account name> --account-key <target storage account name>
```

* Yeni VHD yukarıda açıklandığı gibi bir sanal makineye Ekle.

Örnekler için bkz [bu makalede][storage-azure-cli-copy-blobs].

### <a name="disk-handling"></a>Disk işleme
#### <a name="4efec401-91e0-40c0-8e64-f2dceadff646"></a>SAP dağıtımları için VM/disk yapısı
İdeal olarak bir VM ve ilişkili diskler yapısını işlenmesi çok basit olmalıdır. Şirket içi yüklemelerde sunucu yüklemesi yapılandırılması, birçok yolu müşteriler geliştirmiştir.

* İşletim sistemi ve DBMS ve/veya SAP tüm ikili dosyaları içeren bir temel disk. Mart 2015 tarihinden itibaren bu disk boyutu 127 GB'lık sınırlı önceki kısıtlamaları yerine 1 TB'ye kadar olabilir.
* DBMS içeren bir veya birden çok disk günlük dosyası SAP veritabanının ve DBMS günlük dosyası geçici depolama alanı (DBMS destekliyorsa). Veritabanı günlük IOPS gereksinimleri yüksekse, gerekli IOPS birim ulaşmak için birden çok disk şeritler gerekir.
* SAP veritabanına bir veya iki veritabanı dosyalarının ve DBMS geçici veri dosyalarını da (DBMS destekliyorsa) içeren disk sayısı.

![Azure Iaas VM SAP için başvuru yapılandırması][planning-guide-figure-1300]

[comment]: <> (MShermannd Yapılacaklar açıklamak Linux yapısı)

- - -
> ![Windows][Logo_Windows] Windows
>
> Birçok müşteri ile yapılandırmaları nerede, örneğin, SAP ve DBMS ikili dosyaları işletim Sisteminin yüklendiği c:\ sürücüsü yüklenmedi gördük. Bunun çeşitli nedenleri vardı, ancak biz geri kök oluştu, bunu genellikle sürücüleri küçük ve işletim sistemi yükseltmeleri ek alan 10-15 yıl önce gerekli değil. Bugünlerde artık çok sık koşulların her ikisi de geçerli değildir. Bugün c:\ sürücüsü, büyük hacimli diskleri veya sanal makineleri eşlenebilir. Dağıtımları kendi yapısında basit tutmak için aşağıdaki dağıtım düzeni Azure SAP NetWeaver sistemlerinde için izlemeniz önerilir.
>
> Windows işletim sistemi disk belleği dosyası (kalıcı olmayan disk) D: sürücüde olmalıdır
>
> ![Linux][Logo_Linux] Linux
>
> Linux takas /mnt/mnt/kaynak altında açıklandığı gibi Linux'ta yerleştirin [bu makalede][virtual-machines-linux-agent-user-guide]. Takas dosyası Linux Aracısı /etc/waagent.conf yapılandırma dosyasında yapılandırılabilir. Ekleyin veya aşağıdaki ayarları değiştirebilirsiniz:
>
>

```
ResourceDisk.EnableSwap=y
ResourceDisk.SwapSizeMB=30720
```

Değişiklikleri etkinleştirmek için Linux Aracısı ile yeniden başlatmanız gerekir

```
sudo service waagent restart
```

SAP Not okuyun [1597355] önerilen takas dosyası boyutu hakkında daha fazla ayrıntı için

- - -
DBMS veri dosyaları ve Azure üzerinde barındırılan bu diskleri depolama türü için kullanılan disk sayısı IOPS gereksinimleri ve gerekli gecikme tarafından belirlenmesi. Tam kotaları açıklanmıştır [bu makalede (Linux)] [ virtual-machines-sizes-linux] ve [bu makalede (Windows)][virtual-machines-sizes-windows].

Son 2 yıldır SAP dağıtımlarının deneyimi bize olarak özetlenir bazı dersleri öğrettin:

* Var olan müşteri sistemleri farklı şekilde kendi SAP veritabanları temsil eden veri dosyalarını boyutta beri IOPS trafik farklı veri dosyaları için her zaman aynı değildir. Sonuç olarak, bir RAID yapılandırması LUN olanlar dışında yontulmuş veri dosyaları yerleştirmek için birden çok disk üzerinde daha iyi kullanılmasını dönüştü. Özellikle Azure standart depolama burada DBMS işlem günlüğü karşı tek bir disk kotası isabet bir IOPS oranı ile durumlarda vardı. Bu tür senaryolarda Premium depolama kullanılması önerilir veya alternatif olarak birden çok standart depolama bir araya getirildiği bir yazılım disklerle RAID.

- - -
> ![Windows][Logo_Windows] Windows
>
> * [Azure Virtual Machines'de SQL Server için performans en iyi yöntemler][virtual-machines-sql-server-performance-best-practices]
>
> ![Linux][Logo_Linux] Linux
>
> * [Yazılım RAID Linux üzerinde yapılandırma][virtual-machines-linux-configure-raid]
> * [Azure'da bir Linux VM LVM yapılandırın][virtual-machines-linux-configure-lvm]
> * [Azure depolama gizli bilgiler ve Linux g/ç en iyi duruma getirme](http://blogs.msdn.com/b/igorpag/archive/2014/10/23/azure-storage-secrets-and-linux-i-o-optimizations.aspx)
>
>

- - -
* Premium depolama önemli daha iyi performans, özellikle kritik işlem günlük yazma gösteriliyor. Üretim performans gibi sunmak için beklenen SAP senaryoları için Azure Premium Storage yararlanabilirsiniz VM-serisi kullanmak için önerilir.

Aklınızda işletim sistemi içeren disk ve öneririz, SAP ikili dosyaları ve veritabanı (temel VM) de artık 127 GB ile sınırlı değildir. Boyutu 1 TB'ye kadar artık olabilir. Bu, örneğin, SAP toplu iş günlükleri de dahil olmak üzere tüm gerekli dosyaların tutmak için yeterli alan olmalıdır.

Daha fazla öneri ve özellikle DBMS VM'ler için daha fazla ayrıntı için lütfen bakın [DBMS Dağıtım Kılavuzu][dbms-guide]

#### <a name="disk-handling"></a>Disk işleme
Çoğu senaryoda SAP veritabanına VM dağıtmak için ek disk oluşturmanız gerekir. Biz bölüm diskleri sayısı hakkında dikkat edilecek noktalar açıklandı [SAP dağıtımları için VM/disk yapısının] [ planning-guide-5.5.1] bu belgenin. Ekleme ve temel bir VM dağıtıldıktan sonra Disk ayırma için Azure portal sağlar. Disklerin bağlı/ayrılmış VM yukarı olduğunda ve ne zaman durduruldu yanı sıra çalışan olabilir. Bir disk eklerken, boş bir disk veya bu anda başka bir VM öğesine bağlı olmayan bir mevcut disk eklemek Azure portalı sunar.

**Not**: diskler yalnızca eklenebilir bir VM için belirli bir zamanda.

![Attach / detach Azure Standard Storage disklerle][planning-guide-figure-1400]

Yeni bir sanal makine dağıtımı sırasında yönetilen diskleri kullanın veya Azure depolama hesaplarında disklerinizi yerleştirmek isteyip istemediğinize karar verebilirsiniz. Premium depolama kullanmak istiyorsanız, yönetilen diskleri kullanmanızı öneririz.

Ardından, yeni ve boş bir disk oluşturmak isteyip istemediğinizi ya da daha önce yüklenen ve VM şimdi eklenmelidir varolan bir diski seçin isteyip istemediğinizi karar vermeniz gerekir.

**Önemli**:, **yok** ana bilgisayar önbelleğe alma standart Azure Storage ile kullanmak istediğiniz. Konak önbelleği tercihi hiçbiri varsayılan olarak bırakmanız gerekir. G/ç özelliğini genellikle normal g/ç trafiği veritabanı veri dosyası gibi salt okunur ise Azure Premium Storage ile okuma önbelleği etkinleştirmeniz gerekir. Veritabanı işlem günlüğü dosyasını durumunda, önbelleğe alma önerilir.

- - -
> ![Windows][Logo_Windows] Windows
>
> [Azure portalında bir veri diskini nasıl][virtual-machines-linux-attach-disk-portal]
>
> Diskleri bağlıysa, Windows Disk Yöneticisi'ni açmak için VM'ye oturum açmak gerekir. Otomatik bağlama bölümde önerildiği gibi etkin değilse [bağlı diskler için otomatik bağlama ayarı][planning-guide-5.5.3], yeni eklenen birimin çevrimiçi olması ve alınması gerekir.
>
> ![Linux][Logo_Linux] Linux
>
> Diskleri bağlıysa, VM'ye oturum açın ve diskleri açıklandığı şekilde başlatmak gereken [bu makalede][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]
>
>

- - -
Yeni disk boş disk ise, disk de biçimlendirmeniz gerekir. Biçimlendirme için özellikle DBMS veri ve günlük dosyaları DBMS çıplak dağıtımlar için olduğu gibi aynı önerileri geçerlidir.

Zaten bölümde belirtildiği [Microsoft Azure sanal makine kavram][planning-guide-3.2], bir Azure Storage hesabı g/ç birim bakımından sonsuz kaynaklarını sağlamıyor IOPS ve veri birimi. Genellikle DBMS VM'ler en bu tarafından etkilenir. Birkaç yüksek g/ç birim Azure depolama hesabı toplu sınırı içinde kalmak için dağıtmak için sanal makineleri varsa her VM için ayrı bir depolama hesabı kullanmak en iyi yöntem olabilir. Aksi takdirde, bu sanal makineleri tek her depolama hesabının sınırını basarsa olmadan farklı depolama hesapları arasında nasıl dengeleyebilirsiniz görmek için gerekir. Daha fazla ayrıntı ele alınmıştır [DBMS Dağıtım Kılavuzu'na][dbms-guide]. Ayrıca, saf SAP uygulama server Vm'lerinin veya ek VHD'ler sonunda gerektirebilir diğer VM'ler için aklınızda sınırlamalara tutmanız gerekir. Yönetilen Disk kullanırsanız, bu kısıtlama geçerli değildir. Premium depolama kullanmayı planlıyorsanız, yönetilen Disk kullanmanızı öneririz.

Depolama hesapları için uygun olan başka bir konu, bir depolama hesabı içinde VHD'leri coğrafi olarak çoğaltılmış alma olup olmadığını ' dir. Coğrafi çoğaltma etkin veya depolama hesabı düzeyi ve değil VM düzeyinde devre dışı. Coğrafi çoğaltma etkinleştirilirse, depolama hesabındaki VHD'ler aynı bölge içinde başka bir Azure veri merkezi içinde çoğaltılır. Bu karar vermeden önce aşağıdaki kısıtlama hakkında düşünmek:

Azure coğrafi çoğaltma her VHD bir VM üzerinde yerel olarak çalışır ve kronolojik sıra içinde IOs bir VM'de birden çok VHD arasında çoğaltılmaz. Bu nedenle, VM'ye bağlı ek VHD'lerin yanı sıra temel VM'yi temsil VHD birbirlerinden bağımsız çoğaltılır. Bu, farklı VHD'ler değişiklikleri arasında eşitleme anlamına gelir. IOs anlamına gelir yazılmış sipariş bağımsız olarak çoğaltılır olgu o coğrafi çoğaltma birden çok VHD dağıtılmış kendi veritabanlarını sahip veritabanı sunucuları için değer değil. DBMS yanı sıra da olabilir burada yazma veya işlemler verileri farklı VHD'leri ve değişiklikleri sırasını tutmak önemli olduğu işlemek diğer uygulamaları. Bu gereksinimi varsa, Azure coğrafi çoğaltma etkinleştirilmemelidir. Bağımlı olup gerekir veya bir dizi VM'ler için ancak başka bir küme için coğrafi çoğaltma isterseniz, zaten VM'ler ve bunların ilişkili VHD'leri coğrafi etkin veya devre dışı çoğaltma sahip farklı depolama hesaplarına sınıflandırabilirsiniz.

#### <a name="17e0d543-7e8c-4160-a7da-dd7117a1ad9d"></a>Otomatik bağlama bağlı diskler için ayarlama
- - -
> ![Windows][Logo_Windows] Windows
>
> Kendi görüntüleri veya diskleri oluşturulan VM'ler için denetleyin ve büyük olasılıkla otomatik bağlama parametresi ayarlamak gereklidir. Bu parametre ayarı VM yeniden başlatma veya yeniden dağıtım bağlı ve takılı sürücüleri otomatik olarak yeniden bağlamak için azure'da sonra izin verir.
> Microsoft Azure Marketi tarafından sağlanan görüntüler için parametresini ayarlayın.
>
> Otomatik bağlama ayarlamak için burada komut satırı yürütülebilir diskpart.exe belgeleri gözden geçirin:
>
> * [DiskPart komut satırı seçenekleri](https://technet.microsoft.com/library/bb490893.aspx)
> * [Otomatik bağlama](http://technet.microsoft.com/library/cc753703.aspx)
>
> Windows komut satırı penceresi yönetici olarak açılması gerekir.
>
> Diskleri bağlıysa, Windows Disk Yöneticisi'ni açmak için VM'ye oturum açmak gerekir. Otomatik bağlama bölümde önerildiği gibi etkin değilse [bağlı diskler için otomatik bağlama ayarı][planning-guide-5.5.3], yeni, birim bağlı > çevrimiçi olması ve yapılması gereken.
>
> ![Linux][Logo_Linux] Linux
>
> Yeni eklenen boş disk açıklandığı şekilde başlatmak gereken [bu makalede][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux].
> Ayrıca yeni diskler /etc/fstab eklemeniz gerekir.
>
>

- - -
### <a name="final-deployment"></a>Son dağıtım
Son dağıtım ve dağıtım SAP genişletilmiş izleme, özellikle göre tam adımlar Lütfen için [Dağıtım Kılavuzu'na][deployment-guide].

## <a name="accessing-sap-systems-running-within-azure-vms"></a>Azure VM'ler içinde çalışan SAP sistemler erişme
Yalnızca bulut senaryoları için bu SAP sistemleri SAP GUI kullanarak genel internet üzerinden bağlanan isteyebilirsiniz. Bu durumlarda, aşağıdaki yordamları uygulanması gerekir.

Belgede daha sonra bir siteden siteye (VPN tüneli) veya Azure ExpressRoute bağlantısı şirket içi sistemleri ve Azure sistemleri arasında olan şirket içi dağıtımlarda SAP sistemleri bağlanma diğer ana senaryo aşağıdakiler ele alınacaktır.

### <a name="remote-access-to-sap-systems"></a>SAP sistemleri için uzaktan erişim
Azure Resource Manager ile önceki Klasik modelde varsayılan uç nokta yok artık gibi vardır. Bir Azure ARM VM tüm bağlantı noktalarının açık olduğunu sürece:

1. Hiçbir ağ güvenlik grubu, alt ağ veya ağ arabirimi için tanımlanır. Azure VM'ler için ağ trafiğini sözde "ağ güvenlik grupları" güvenli hale getirilebilir. Daha fazla bilgi için bkz: [bir ağ güvenlik grubu (NSG) nedir?][virtual-networks-nsg]
2. Azure yük dengeleyici ağ arabirimi için tanımlanır   

Bölümünde açıklandığı gibi Klasik modeli ve ARM mimarisi birbirinden bkz [bu makalede][virtual-machines-azure-resource-manager-architecture].

#### <a name="configuration-of-the-sap-system-and-sap-gui-connectivity-for-cloud-only-scenario"></a>Yalnızca bulut senaryosu için SAP sistem ve SAP GUI bağlantısı yapılandırma
Lütfen bu konuya ayrıntıları tanımlayan bu makalesine bakın: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/06/24/sap-gui-connection-closed-when-connecting-to-sap-system-in-azure.aspx>

#### <a name="changing-firewall-settings-within-vm"></a>VM dahilinde güvenlik duvarı ayarlarını değiştirme
SAP sisteminize gelen trafiğe izin vermek için sanal makinelerde güvenlik duvarını yapılandırmanız gerekebilir.

- - -
> ![Windows][Logo_Windows] Windows
>
> Varsayılan olarak, bir Azure dağıtılan VM dahilinde Windows Güvenlik Duvarı etkinleştirilir. Şimdi SAP bağlantı noktasının açılması izin vermeniz, aksi takdirde SAP GUI bağlanmak mümkün olmaz.
> Bunu yapmak için:
>
> * Denetim masası\sistem ve güvenlik\windows Güvenlik Duvarı'nda açmak **Gelişmiş ayarları**.
> * Şimdi gelen kuralları sağ tıklatın ve seçtiğiniz **yeni kural**.
> * Aşağıdaki Sihirbazı'nda seçtiğiniz yeni **bağlantı noktası** kuralı.
> * Sihirbazın sonraki adımda, açmak istediğiniz bağlantı noktası numarası TCP ve türü ayarında bırakın. Bizim SAP örnek kimliği 00 olduğundan, biz 3200 sürdü. Farklı bir örnek numarasını örneğiniz varsa, daha önce örnek sayısına göre tanımlanan bağlantı noktası açık olmalıdır.
> * Sihirbazın sonraki adımına öğesi bırakın gerek **izin bağlantı** işaretli.
> * Sihirbazın sonraki adımda kural etki alanı, özel ve ortak ağ için geçerli olup olmadığını belirlemeniz gerekir. Lütfen gereksinimlerinize gerekirse ayarlayın. Ancak, ortak ağ üzerinden dışarıdan SAP GUI ile bağlanırken, ortak ağa uygulanan kural gerekir.
> * Kuralı Sihirbazı'nın son adımda adlandırın ve tuşlarına basarak Kaydet **son**.
>
> Kural hemen etkin olur.
>
> ![Bağlantı noktası kuralı tanımı][planning-guide-figure-1600]
>
> ![Linux][Logo_Linux] Linux
>
> Azure Market Linux görüntülerinde varsayılan olarak iptables Güvenlik Duvarı'nı etkinleştirme ve SAP sisteminizi bağlantısı çalışması gerekir. İptables veya başka bir güvenlik duvarı etkinleştirilirse, lütfen iptables veya (burada xx SAP sisteminizi sistem sayısıdır) bağlantı noktası 32xx gelen tcp trafiğine izin verecek şekilde kullanılan güvenlik duvarı için belgelerine bakın.
>
>

- - -
#### <a name="security-recommendations"></a>Güvenlik önerileri
SAP GUI, herhangi bir çalışan, ancak ilk SAP ileti sunucu işlemine (bağlantı noktası 36xx) açılan bağlantı noktası üzerinden bağlanır SAP örneklerin (bağlantı noktası 32xx) hemen bağlanmaz. Geçmişte çok aynı bağlantı noktasını iç iletişim uygulama örnekleri için ileti sunucusu tarafından kullanıldı. Önlemek için yanlışlıkla iç iletişim bağlantı noktalarını değiştirilebilir azure'da bir ileti sunucusu ile iletişim kurmasını uygulama sunucuları şirket içi. SAP ileti sunucusu kendi uygulama örnekleri arasında iç iletişimi geliştirme projesi test vb. için bir kopyasını gibi şirket içi sistemlerden klonlanmış sistemlerde farklı bağlantı noktası numarasını değiştirmek için önerilir. Bu varsayılan profili parametresiyle yapılabilir:

> rdisp/msserv_internal
>
>

açıklandığı gibi [SAP ileti sunucusu için güvenlik ayarları](https://help.sap.com/saphelp_nwpi71/helpdata/en/47/c56a6938fb2d65e10000000a42189c/content.htm)

## <a name="96a77628-a05e-475d-9df3-fb82217e8f14"></a>SAP örneklerinin yalnızca bulut dağıtımının kavramları
### <a name="3e9c3690-da67-421a-bc3f-12c520d99a30"></a>SAP demo/senaryo eğitimi NetWeaver tek VM
![Tek başına VM SAP demo sistemler ile aynı VM adları çalıştıran, yalıtılmış Azure bulut Hizmetleri][planning-guide-figure-1700]

Bu senaryoda (bölüm bkz [yalnızca bulut] [ planning-guide-2.1] bu belgenin) tam eğitim/tanıtım senaryo bulunduğu bir tipik eğitim/tanıtım sistem senaryosu tek bir VM'de uyguluyorsanız. Dağıtım VM görüntü şablonları üzerinden yapılır varsayalım. Ayrıca bu demo/eğitimleri, birden fazla sanal makineleri aynı ada sahip VM'ler ile dağıtılması gerekir varsayıyoruz.

Bölüm bazı bölümlerinde açıklandığı gibi bir VM görüntüsü oluşturulan varsayılır [SAP Azure VM'lerin hazırlanıyor] [ planning-guide-5.2] bu belgedeki.

Senaryo uygulama olayların sırası şöyle görünür:

##### <a name="powershell"></a>PowerShell
* Her eğitim/tanıtım yatay için yeni bir kaynak grubu oluştur

```powershell
$rgName = "SAPERPDemo1"
New-AzureRmResourceGroup -Name $rgName -Location "North Europe"
```
* Yönetilen disklerini kullanmak istemiyorsanız, yeni bir depolama hesabı oluşturma

```powershell
$suffix = Get-Random -Minimum 100000 -Maximum 999999
$account = New-AzureRmStorageAccount -ResourceGroupName $rgName -Name "saperpdemo$suffix" -SkuName Standard_LRS -Kind "Storage" -Location "North Europe"
```

* Aynı ana bilgisayar adı ve IP adreslerini kullanımını etkinleştirmek her eğitim/tanıtım yatay için yeni bir sanal ağ oluşturun. Sanal ağ, yalnızca SSH için Uzak Masaüstü erişimi etkinleştirmek için 3389 numaralı bağlantı noktası ve bağlantı noktası 22 trafiğe izin veren bir ağ güvenlik grubu tarafından korunuyor.

```powershell
# Create a new Virtual Network
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGRDP -Protocol * -SourcePortRange * -DestinationPortRange 3389 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 100
$sshRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGSSH -Protocol * -SourcePortRange * -DestinationPortRange 22 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 101
$nsg = New-AzureRmNetworkSecurityGroup -Name SAPERPDemoNSG -ResourceGroupName $rgName -Location  "North Europe" -SecurityRules $rdpRule,$sshRule

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name Subnet1 -AddressPrefix  10.0.1.0/24 -NetworkSecurityGroup $nsg
$vnet = New-AzureRmVirtualNetwork -Name SAPERPDemoVNet -ResourceGroupName $rgName -Location "North Europe"  -AddressPrefix 10.0.1.0/24 -Subnet $subnetConfig
```

* Sanal makine internet'ten erişmek için kullanılan yeni bir ortak IP adresi oluştur

```powershell
# Create a public IP address with a DNS name
$pip = New-AzureRmPublicIpAddress -Name SAPERPDemoPIP -ResourceGroupName $rgName -Location "North Europe" -DomainNameLabel $rgName.ToLower() -AllocationMethod Dynamic
```

* Sanal makine için yeni bir ağ arabirimi oluştur

```powershell
# Create a new Network Interface
$nic = New-AzureRmNetworkInterface -Name SAPERPDemoNIC -ResourceGroupName $rgName -Location "North Europe" -Subnet $vnet.Subnets[0] -PublicIpAddress $pip
```

* Bir sanal makine oluşturun. Yalnızca bulut senaryosu için her VM aynı ada sahip olacaktır. Bu sanal makineleri SAP NetWeaver durumlarda SAP SID'si aynı de olacaktır. Azure kaynak grubu içinde VM adının benzersiz olması gerekir, ancak farklı Azure kaynak grubunda aynı ada sahip VM'ler çalıştırabilirsiniz. Windows veya Linux ' kök' varsayılan 'Yönetici' hesabı geçerli değil. Bu nedenle, yeni bir yönetici kullanıcı adı bir parola ile birlikte tanımlanması gerekiyor. VM boyutu da tanımlanması gerekiyor.

```powershell
#####
# Create a new virtual machine with an official image from the Azure Marketplace
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

# select image
$vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "SUSE" -Offer "SLES-SAP" -Skus "12-SP1" -Version "latest"
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "RedHat" -Offer "RHEL" -Skus "7.2" -Version "latest"
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "Oracle" -Offer "Oracle-Linux" -Skus "7.2" -Version "latest"
# $vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$vmconfig = Set-AzureRmVMBootDiagnostics -Disable -VM $vmconfig
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

```powershell
#####
# Create a new virtual machine with a VHD that contains the private image that you want to use
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$diskName="osfromimage"
$osDiskUri=$account.PrimaryEndpoints.Blob.ToString() + "vhds/" + $diskName  + ".vhd"

$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path to VHD that contains the OS image> -Windows
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred
#$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path to VHD that contains the OS image> -Linux
#$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Set-AzureRmVMBootDiagnostics -Disable -VM $vmconfig
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

```powershell
#####
# Create a new virtual machine with a Managed Disk Image
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -Id <Id of Managed Disk Image>
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred
#$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Set-AzureRmVMBootDiagnostics -Disable -VM $vmconfig
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

* İsteğe bağlı olarak ek disk ekleyin ve gerekli içerik geri yükleyin. Tüm blob adları (URL BLOB'lar için) Azure içinde benzersiz olduğunu unutmayın.

```powershell
# Optional: Attach additional VHD data disks
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name SAPERPDemo
$dataDiskUri = $account.PrimaryEndpoints.Blob.ToString() + "vhds/datadisk.vhd"
Add-AzureRmVMDataDisk -VM $vm -Name datadisk -VhdUri $dataDiskUri -DiskSizeInGB 1023 -CreateOption empty | Update-AzureRmVM

# Optional: Attach additional Managed Disks
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name SAPERPDemo
Add-AzureRmVMDataDisk -VM $vm -Name datadisk -DiskSizeInGB 1023 -CreateOption empty -Lun 0 | Update-AzureRmVM
```

##### <a name="cli"></a>CLI
Aşağıdaki kod örneği, Linux üzerinde kullanılabilir. Windows, lütfen PowerShell yukarıda açıklandığı gibi kullanabilir veya % rgName % yerine $rgName kullanın ve Windows komutunu kullanarak ortam değişkenini ayarlamak için örnekte uyum *ayarlamak*.

* Her eğitim/tanıtım yatay için yeni bir kaynak grubu oluştur

```
rgName=SAPERPDemo1
rgNameLower=saperpdemo1
az group create --name $rgName --location "North Europe"
```

* Yeni depolama hesabı oluşturma

```
az storage account create --resource-group $rgName --location "North Europe" --kind Storage --sku Standard_LRS --name $rgNameLower
```

* Aynı ana bilgisayar adı ve IP adreslerini kullanımını etkinleştirmek her eğitim/tanıtım yatay için yeni bir sanal ağ oluşturun. Sanal ağ, yalnızca SSH için Uzak Masaüstü erişimi etkinleştirmek için 3389 numaralı bağlantı noktası ve bağlantı noktası 22 trafiğe izin veren bir ağ güvenlik grubu tarafından korunuyor.

```
az network nsg create --resource-group $rgName --location "North Europe" --name SAPERPDemoNSG
az network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGRDP --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 3389 --access Allow --priority 100 --direction Inbound
az network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGSSH --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 22 --access Allow --priority 101 --direction Inbound

az network vnet create --resource-group $rgName --name SAPERPDemoVNet --location "North Europe" --address-prefixes 10.0.1.0/24
az network vnet subnet create --resource-group $rgName --vnet-name SAPERPDemoVNet --name Subnet1 --address-prefix 10.0.1.0/24 --network-security-group SAPERPDemoNSG
```

* Sanal makine internet'ten erişmek için kullanılan yeni bir ortak IP adresi oluştur

```
az network public-ip create --resource-group $rgName --name SAPERPDemoPIP --location "North Europe" --dns-name $rgNameLower --allocation-method Dynamic
```

* Sanal makine için yeni bir ağ arabirimi oluştur

```
az network nic create --resource-group $rgName --location "North Europe" --name SAPERPDemoNIC --public-ip-address SAPERPDemoPIP --subnet Subnet1 --vnet-name SAPERPDemoVNet
```

* Bir sanal makine oluşturun. Yalnızca bulut senaryosu için her VM aynı ada sahip olacaktır. Bu sanal makineleri SAP NetWeaver durumlarda SAP SID'si aynı de olacaktır. Azure kaynak grubu içinde VM adının benzersiz olması gerekir, ancak farklı Azure kaynak grubunda aynı ada sahip VM'ler çalıştırabilirsiniz. Windows veya Linux ' kök' varsayılan 'Yönetici' hesabı geçerli değil. Bu nedenle, yeni bir yönetici kullanıcı adı bir parola ile birlikte tanımlanması gerekiyor. VM boyutu da tanımlanması gerekiyor.

```
#####
# Create virtual machines using storage accounts
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image SUSE:SLES-SAP:12-SP1:latest --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image RedHat:RHEL:7.2:latest --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image "Oracle:Oracle-Linux:7.2:latest" --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --authentication-type password

#####
# Create virtual machines using Managed Disks
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image SUSE:SLES-SAP:12-SP1:latest --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image RedHat:RHEL:7.2:latest --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image "Oracle:Oracle-Linux:7.2:latest" --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --authentication-type password
```

```
#####
# Create a new virtual machine with a VHD that contains the private image that you want to use
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --os-type Windows --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --image <path to image vhd>
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --os-type Linux --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --image <path to image vhd> --authentication-type password

#####
# Create a new virtual machine with a Managed Disk Image
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --image <managed disk image id>
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --image <managed disk image id> --authentication-type password
```

* İsteğe bağlı olarak ek disk ekleyin ve gerekli içerik geri yükleyin. Tüm blob adları (URL BLOB'lar için) Azure içinde benzersiz olduğunu unutmayın.

```
# Optional: Attach additional VHD data disks
az vm unmanaged-disk attach --resource-group $rgName --vm-name SAPERPDemo --size-gb 1023 --vhd-uri https://$rgNameLower.blob.core.windows.net/vhds/data.vhd  --new

# Optional: Attach additional Managed Disks
az vm disk attach --resource-group $rgName --vm-name SAPERPDemo --size-gb 1023 --disk datadisk --new
```

##### <a name="template"></a>Şablon
Github'daki azure hızlı başlangıç şablonlarını deposunda örnek şablonları kullanabilirsiniz.

* [Basit Linux VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
* [Basit bir Windows VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
* [Görüntüden VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

### <a name="implement-a-set-of-vms-which-need-to-communicate-within-azure"></a>Azure içinde iletişim kurmak için gereken sanal makineleri kümesini uygular
Bu yalnızca bulut senaryosu eğitim ve tanıtım için tipik bir senaryodur where demo/eğitim temsil eden yazılım amacıyla senaryo birden çok VM yayılır. Birbirleri ile iletişim kurmak farklı VM'ler yüklü farklı bileşenleri gerekir. Yeniden, bu senaryoda, bir şirket içi ağ iletişimi veya şirket içi senaryo gereklidir.

Bu senaryo bir bölümde açıklanan yükleme uzantısıdır [tek VM SAP demo/senaryo eğitimi NetWeaver ile] [ planning-guide-7.1] bu belgenin. Bu durumda daha fazla sanal makine var olan bir kaynak grubuna eklenir. Aşağıdaki örnekte bir SAP ASCS/SCS VM'i, DBMS ve SAP uygulama sunucusu örneği VM çalıştıran VM eğitim yatay oluşur.

Bu senaryo oluşturmadan önce önce senaryoda zaten kullandı temel ayarları hakkında düşünmeniz gerekir.

#### <a name="resource-group-and-virtual-machine-naming"></a>Kaynak grubu ve sanal makine adlandırma
Tüm kaynak grubu adları benzersiz olmalıdır. Kendi adlandırma şeması kaynaklarınızın gibi geliştirmek `<rg-name`>-soneki.

Sanal makine adı kaynak grubun içinde benzersiz olması gerekir.

#### <a name="set-up-network-for-communication-between-the-different-vms"></a>Farklı sanal makineler arasındaki iletişim için ağ ayarlayın
![Bir Azure sanal ağ içindeki VM'ler kümesi][planning-guide-figure-1900]

Önlemek için aynı eğitim/tanıtım Windows'un klonlar ile çakışma adlandırma, her yatay bir Azure sanal ağı oluşturmanız gerekir. DNS ad çözümlemesi Azure tarafından sağlanacak veya Azure (daha fazla burada ele alınacak değil) dışında kendi DNS sunucusunu yapılandırabilirsiniz. Bu senaryoda, biz kendi DNS yapılandırmayın. Bir Azure sanal ağı içindeki tüm sanal makineler için ana bilgisayar adları üzerinden iletişim etkinleştirilecek.

Sanal ağlar ve yalnızca kaynak grupları tarafından eğitim veya tanıtım Windows'un ayırmak için nedenlerden kaynaklanabilir:

* SAP yatay ayarlandığı kendi AD/OpenLDAP gerekir ve bir etki alanı sunucusu her Windows'un bir parçası olması gerekiyor.  
* SAP yatay ayarlandığı sabit IP adresleri ile çalışmanız için gerekli bileşenleri içerir.

Azure sanal ağlar ve bunları tanımlama hakkında daha fazla ayrıntı bulunabilir [bu makalede][virtual-networks-create-vnet-arm-pportal].

## <a name="deploying-sap-vms-with-corporate-network-connectivity-cross-premises"></a>SAP VM'ler şirket ağ bağlantısı (şirket içi) ile dağıtma
Bir SAP yatay çalıştırın ve çıplak için birinci sınıf DBMS sunucuları arasında dağıtım bölmek istiyorsanız, şirket içi sanallaştırılmış ortamlar için uygulama katmanları ve daha küçük 2 katmanlı SAP sistemleri ve Azure Iaas yapılandırılmış. SAP sistemleri bir SAP yatay içinde şirket, kendi dağıtım biçiminde bağımsız dağıtılan birçok diğer yazılım bileşenleri ve birbirleri ile iletişim kurması gereken temel varsayılır. Ayrıca olmamalıdır SAP GUI veya diğer arabirimleri ile bağlanma son kullanıcı için dağıtım form tarafından sunulan herhangi bir fark. Biz şirket içi Active Directory/OpenLDAP ve site-için-site/çok siteler bağlantısını veya Azure ExpressRoute gibi özel bağlantılar aracılığıyla Azure sistemlere genişletilmiş DNS hizmetleri varsa, bu koşullar yalnızca karşılanabilir.

Daha fazla arka plan Azure üzerinde SAP uygulama ayrıntılarını almak için bölüm okumanızı öneririz [kavramları, Cloud-Only dağıtım SAP örneklerinin] [ planning-guide-7] bu belgenin Azure ve nasıl bunlar Azure SAP uygulamaları ile kullanılması gereken temel bilgileri yapıları bazıları açıklanmaktadır.

### <a name="scenario-of-an-sap-landscape"></a>Bir SAP yatay senaryosu
Şirket içi senaryo, kabaca grafiklerde gibi açıklanabilir:

![Şirket içi ve Azure varlıklar arasında siteden siteye bağlantı][planning-guide-figure-2100]

Yukarıda gösterilen senaryo, bir senaryo açıklanmaktadır. burada şirket içi AD/OpenLDAP ve DNS için Azure genişletilmiş. Şirket içi tarafında, belirli bir IP adres aralığı Azure abonelik başına ayrılmıştır. IP adresi aralığı, bir Azure sanal ağı Azure tarafında atanır.

#### <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler
SSL/TLS gibi güvenli iletişim protokolleri tarayıcı erişimi veya VPN tabanlı bağlantılar için sistem Azure hizmetlerine erişime izin için en düşük gereksinim kullanılmasıdır. Şirketler VPN bağlantısı kendi şirket ağınız ve Azure arasında çok farklı işler varsayılır. Bazı şirketler blankly tüm bağlantı noktalarının açık. Diğer bazı şirketler, hangi bağlantı noktalarını açın, vb. için ihtiyaç duydukları çok kesin olarak isteyebilirsiniz.

Tipik SAP aşağıdaki tabloda iletişim bağlantı noktaları listelenir. Temel olarak SAP ağ geçidi bağlantı noktasını açmak yeterli olur.

| Hizmet | Bağlantı noktası adı | Örnek `<nn`> 01 = | Varsayılan aralığı (min-maks.) | Açıklama |
| --- | --- | --- | --- | --- |
| Dağıtıcı |sapdp`<nn>` bakın * |3201 |3200 - 3299 |Windows için SAP GUI ve Java tarafından kullanılan SAP göndericisi |
| İleti sunucusu |sapms`<sid`> bkz ** |3600 |Ücretsiz sapms`<anySID`> |SID SAP sistem kimliği = |
| Ağ geçidi |sapgw`<nn`> bkz * |3301 |Boş |SAP ağ geçidi, CPIC ve RFC iletişim için kullanılan |
| SAP yönlendirici |sapdp99 |3299 |Boş |Yalnızca CI (Merkezi örneği) hizmeti adlarının yüklendikten sonra /etc/services rasgele bir değere atanamaz. |

*) nn = SAP örnek sayısı

*) SID SAP sistem kimliği =

Farklı SAP ürünler için gereken bağlantı noktaları hakkında daha ayrıntılı bilgi veya hizmetleri SAP ürünleri tarafından şurada bulunabilir <http://scn.sap.com/docs/DOC-17124>.
Bu belgeyle belirli SAP ürünleri ve senaryoları için gerekli VPN cihazı ayrılmış bağlantı noktaları açma yapabiliyor olmanız gerekir.

Diğer güvenlik ölçer oluşturmak için sanal makineleri böyle bir senaryoda dağıtma olabilir bir [ağ güvenlik grubu] [ virtual-networks-nsg] erişim kurallarını tanımlamak için.

### <a name="dealing-with-different-virtual-machine-series"></a>Farklı sanal makine serisi postalarla
Son 12 ay sırasında Microsoft ya da Vcpu, bellek sayısı farklı pek çok daha fazla VM türleri eklenmiş veya daha önemli donanım üzerinde çalıştığı. Bu tüm VM'lerin SAP ile desteklenir (bkz: desteklenen VM SAP Not türlerinde [1928533]). Bu VM'lerin bazıları farklı ana bilgisayar donanım nesli üzerinde çalıştırın. Bu konak donanım nesli yer ayrıntı düzeyi, bir Azure ölçek birimi dağıtılır. Burada seçtiğiniz farklı VM boyutları aynı ölçek birimi üzerinde çalıştıramazsınız anlamına gelir durumlarda gerçekleşebilir. Bir kullanılabilirlik kümesi ölçek birimleri farklı donanım tabanlı kapsanacak yeteneği sınırlıdır.  Örneğin DBMS A5 A11 VM'ler ve SAP uygulama katmanı G-serisi vm'lerinde çalıştırmak istiyorsanız, tek bir SAP sistem ya da farklı kullanılabilirlik kümeleri içinde farklı SAP sistemleri dağıtmak için zorunlu.

#### <a name="printing-on-a-local-network-printer-from-sap-instance-in-azure"></a>SAP örneğinden Azure yerel ağ yazıcısı yazdırma
##### <a name="printing-over-tcpip-in-cross-premises-scenario"></a>TCP/IP üzerinden şirket içi senaryoda yazdırma
Şirket içi TCP/IP tabanlı ağ yazıcıları Azure VM'deki ayarlama genel bir VPN siteden siteye tünel veya ExpressRoute bağlantı kuruldu olduğu varsayılarak şirket ağınıza olduğu gibi aynıdır.

- - -
> ![Windows][Logo_Windows] Windows
>
> Bunu yapmak için:
>
> * Bazı ağ yazıcıları yazıcınızın Azure VM'deki ayarlamak kolay hale getiren bir Yapılandırma Sihirbazı ile gelir. Herhangi bir Sihirbazı yazılım yazıcıyla dağıtılmışsa yazıcıyı ayarlamak için el ile yeni bir TCP/IP yazıcı bağlantı noktasını oluşturmak için yoludur.
> * Açık Denetim Masası -> cihazlar ve yazıcılar Yazıcı Ekle ->
> * TCP/IP adresi veya ana bilgisayar adı kullanarak bir yazıcı Ekle'yi seçin
> * Yazıcı IP adresi yazın
> * Yazıcı bağlantı noktasını standart 9100
> * Gerekirse uygun yazıcı sürücüsü el ile yükleyin.
>
> ![Linux][Logo_Linux] Linux
>
> * Ağ yazıcısı yüklemek için standart yordam Windows yalnızca izleme ister
> * yalnızca için ortak Linux kılavuzları izleyin [SUSE](https://www.suse.com/documentation/sles-12/book_sle_deployment/data/sec_y2_hw_print.html) veya [Red Hat ve Oracle Linux](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/sec-Printer_Configuration.html) yazıcı ekleme.
>
>

- - -
![Ağ yazdırma][planning-guide-figure-2200]

##### <a name="host-based-printer-over-smb-shared-printer-in-cross-premises-scenario"></a>Ana bilgisayar tabanlı yazıcıya SMB (paylaşılan yazıcı) üzerinden şirket içi senaryosu
Ana bilgisayar tabanlı yazıcılar tasarım gereği ağ ile uyumlu değildir. Ancak, bir ağdaki bilgisayarlar arasında yazıcının gücü açma bilgisayara bağlı olduğu sürece bir ana bilgisayar tabanlı yazıcı paylaşılabilir. Şirket bağlanmak siteden siteye ya da ExpressRoute ağ ve yerel yazıcınızın paylaşın. SMB protokolü NetBIOS yerine DNS ad hizmeti kullanır. NetBIOS ana bilgisayar adı DNS ana bilgisayar adından farklı olabilir. NetBIOS ana bilgisayar adı ve DNS ana bilgisayar adı özdeş olduğunu standart durumdur. DNS etki alanı NetBIOS adı alanı mantıklı değildir. Buna göre DNS ana bilgisayar adı ve DNS etki alanı oluşan tam DNS ana bilgisayar adı NetBIOS adı alanında kullanılmamalıdır.

Yazıcı Paylaşımı, ağdaki benzersiz bir ad tarafından tanımlanır:

* (Her zaman gereklidir) SMB ana bilgisayarın ana bilgisayar adı.
* (Her zaman gereklidir) paylaşımının adı.
* Yazıcı paylaşıyorsanız etki alanının adını SAP sistem aynı etki alanında değil.
* Ayrıca, bir kullanıcı adı ve parola yazıcı paylaşımına erişmek için gerekli.

Nasıl yapılır:

- - -
> ![Windows][Logo_Windows] Windows
>
> Yerel yazıcı paylaşın.
> Azure VM'de, yazıcı paylaşım adı, türü ve Windows Gezgini'ni açın.
> Bir yazıcı Yükleme Sihirbazı'nı yükleme işleminde size kılavuzluk.
>
> ![Linux][Logo_Linux] Linux
>
> İşte bazı örnekler içinde Linux ağ yazıcıları yapılandırma veya bölüm de dahil olmak üzere ilgili belgelerin Linux içinde yazdırma ilgili. VM VPN parçası olduğu sürece bir Azure Linux VM'de aynı şekilde çalışır:
>
> * SLES <https://en.opensuse.org/SDB:Printing_via_SMB_ (Samba) _Share_or_Windows_Share>
> * RHEL veya Oracle Linux <https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/System_Administrators_Guide/sec-Printer_Configuration.html#s1-printing-smb-printer>
>
>

- - -
##### <a name="usb-printer-printer-forwarding"></a>USB yazıcı (Yazıcı iletme)
Azure'da kullanıcılara uzak bir oturumda yerel yazıcı cihazlarını erişimi sağlamak için Uzak Masaüstü Hizmetleri özelliği kullanılabilir değil.

- - -
> ![Windows][Logo_Windows] Windows
>
> Windows ile yazdırma hakkında daha fazla bilgi şurada bulunabilir: <http://technet.microsoft.com/library/jj590748.aspx>.
>
>

- - -
#### <a name="integration-of-sap-azure-systems-into-correction-and-transport-system-tms-in-cross-premises"></a>SAP tümleştirme Azure sistemlere düzeltme ve şirket içi (TMS) sistemde taşıma
SAP değiştirme ve taşıma sistem (TMS) yatay sistemler arasında taşıma isteği almak ve vermek için yapılandırılmış olması gerekir. Kalite güvence (QA) ve üretken sistemleri (PRD) şirket içi ise bir SAP sistemine (Geliştirme) geliştirme örnekleri Azure içinde bulunduğu varsayılmaktadır. Ayrıca, bir merkezi aktarım dizini olduğunu varsayın.

##### <a name="configuring-the-transport-domain"></a>Aktarım etki alanını yapılandırma
Belirlediğiniz aktarım etki alanı denetleyicisi olarak açıklandığı gibi sistem üzerindeki aktarım etki alanınızı yapılandırmak [aktarım etki alanı denetleyicisi yapılandırma](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm). Sistem kullanıcı tarafından TMSADM oluşturulan ve gerekli RFC hedef oluşturulur. İşlem SM59 kullanarak bu RFC bağlantıları kontrol edebilirsiniz. Ana bilgisayar adı çözümlemesi, aktarım etki alanı genelinde etkinleştirilmesi gerekir.

Nasıl yapılır:

* Senaryomuzda şirket içi QAS sistem CTS etki alanı denetleyicisi olacaktır vermiştir. İşlem STMS çağırın. TMS iletişim kutusu görüntülenir. Bir taşıma etki alanı yapılandırma iletişim kutusu görüntülenir. (Aktarım etki alanı henüz yapılandırmadıysanız yalnızca bu iletişim kutusu görünür.)
* Otomatik olarak oluşturulan kullanıcı TMSADM yetkili olduğundan emin olun (SM59 ABAP bağlantı -> -> TMSADM@E61.DOMAIN_E61 -> ayrıntıları -> Utilities(M) yetkilendirme Test ->). İşlem ilk ekranda STMS bu SAP Sistem şimdi aktarım etki alanı denetleyicisi olarak aşağıda gösterildiği gibi çalışıp çalışmadığını göstermesi gerekir:

![Etki alanı denetleyicisinde STMS işleminin ilk ekran][planning-guide-figure-2300]

#### <a name="including-sap-systems-in-the-transport-domain"></a>Aktarım etki alanında SAP sistemleri de dahil olmak üzere
Bir taşıma etki alanında bir SAP sistemi dahil olmak üzere sırası şu şekilde görünür:

* Azure'da geliştirme sisteminde, aktarım sistemine (istemci 000) gidin ve işlem STMS çağırın. Diğer yapılandırma iletişim kutusundan seçin ve etki alanındaki dahil sistemiyle devam edin. Etki alanı denetleyicisi hedef konak belirtin ([SAP sistemlerle aktarım etki alanındaki](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0c17acc11d1899e0000e829fbbd/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm)). Sistem şimdi aktarım etki alanında dahil edilecek bekliyor.
* Güvenlik nedenleriyle, böylece isteğinizi onaylamak için etki alanı denetleyicisini geri dönmek olur. Sisteme genel bakış ve Onayla seçim bekleme sistemi. İstem ve yapılandırma dağıtılacağı onaylayın.

Bu SAP Sistem şimdi aktarım etki alanındaki tüm diğer SAP sistemleri hakkında gerekli bilgileri içerir. Aynı zamanda tüm diğer SAP sistemlere gönderilen yeni SAP sisteminin adresi veri ve SAP sistem Aktarım Denetim programı aktarım profilinde girilir. RFC'leri ve etki alanının aktarım dizinine erişimi çalışıp çalışmadığını denetleyin.

Aktarım sisteminizin yapılandırması ile her zamanki gibi belgelerinde açıklandığı gibi devam [değiştirme ve taşıma sistemi](http://help.sap.com/saphelp_nw70ehp3/helpdata/en/48/c4300fca5d581ce10000000a42189c/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm).

Nasıl yapılır:

* Şirket içi, STMS doğru yapılandırıldığından emin olun.
* Ana bilgisayar adını taşıma etki alanı denetleyicisi Azure ve tersine visa sanal makinenizde çözülebilir emin olun.
* İşlem STMS diğer yapılandırma -> çağrı -> Sistem etki alanındaki içerir.
* Bağlantı açık şirket içi TMS sistemde doğrulayın.
* Taşıma yolları, grupları ve Katmanlar her zamanki gibi yapılandırın.

Siteden siteye bağlanan şirketler arası içinde senaryoları, şirket içi ve Azure arasında gecikme hala önemli olabilir. Biz geliştirme nesnelerde taşıma sırası izleyin ve üretim sistemlerine test taşımaları uygulama hakkında düşünmek veya paketleri farklı sistemleri için destek, merkezi aktarım dizininin konumu bağımlı, bazı sistemler okuma veya veri merkezi aktarım dizininde yazma yüksek gecikme karşılaşır, unutmayın. Durum veri merkezleri arasında önemli uzaklıklı farklı veri merkezleri aracılığıyla farklı sistemleri burada yayılır SAP yatay yapılandırmalarına benzer.

Bu tür gecikme çalışma ve Hızlı Okuma ya da ya da yazma aktarım dizininden çalışma sistemleri için iki STMS aktarım etki ayarlayabilirsiniz (şirket içi, diğeri Azure sistemleriyle için ve aktarım etki alanları bağlayın. Lütfen bu kavram SAP TMS olarak ardında yatan ilkeler açıklayan bu belgelere bakın: <http://help.sap.com/saphelp_me60/helpdata/en/c4/6045377b52253de10000009b38f889/content.htm?frameset=/en/57/38dd924eb711d182bf0000e829fbfe/frameset.htm>.

Nasıl yapılır:

* Her bir konum (şirket içi ve Azure) aktarım etki alanı ayarlama işlemi STMS kullanarak <http://help.sap.com/saphelp_nw70ehp3/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm>
* Etki alanlarını etki alanı ile bağlantıyı ve iki etki alanı arasındaki bağlantı onaylayın.
  <http://help.SAP.com/saphelp_nw73ehp1/helpdata/en/a3/139838280c4f18e10000009b38f8cf/Content.htm>
* Bağlı sistem yapılandırmaya dağıtın.

#### <a name="rfc-traffic-between-sap-instances-located-in-azure-and-on-premises-cross-premises"></a>Azure ve şirket içi (şirket içi) bulunan SAP örnekleri arasında RFC trafiği
RFC trafiği Azure ve şirket içi sistemlerini arasında çalışması gerekir. Burada bir RFC bağlantısı hedef sistem doğrultusunda tanımlamanız gerekir bağlantı çağrısı hareketi SM59 oluşturan kaynak sistemde ayarlamak için. Yapılandırma standart bir RFC bağlantısı kurulumun benzer.

Şirket içi bir senaryoda, birbirleri ile iletişim kurmak için gereken çalışma SAP sistemleri aynı etki alanında olan VM'ler varsayıyoruz. Bu nedenle SAP sistemleri arasında bir RFC bağlantısı Kurulum kurulum adımlarını ve şirket içi senaryolarda girişleri farklı değildir.

#### <a name="accessing-local-fileshares-from-sap-instances-located-in-azure-or-vice-versa"></a>Azure veya tersi bulunan SAP örneklerinden erişilirken yerel fileshares
Kurumsal şirket içinde olan dosya paylaşımlarına erişmek Azure içinde bulunan SAP örnekleri gerekir. Ayrıca, Azure'da bulunan dosya paylaşımlarına erişmek şirket içi SAP örnekleri gerekir. Dosya paylaşımları etkinleştirmek için izinleri ve yerel sistem paylaşım seçeneklerini yapılandırmanız gerekir. Azure ve veri merkeziniz arasında VPN ya da ExpressRoute bağlantı noktalarına açtığınızdan emin olun.

## <a name="supportability"></a>Desteklenebilirlik
### <a name="6f0a47f3-a289-4090-a053-2521618a28c3"></a>Azure için SAP izleme çözümü
Etkinleştirmek için görev kritik SAP sistemleri Azure ile ilgili izleme araçları SAPOSCOL veya SAP konak Aracısı izlemeyi SAP veri alma SAP için bir Azure izleme uzantısı aracılığıyla Azure sanal makine hizmet ana bilgisayarı kapat. SAP tarafından taleplerini uygulamaları SAP çok özel olduğundan, Microsoft olmayan genel Azure içine gerekli işlevselliği uygulamak, ancak gerekli izleme bileşenleri ve yapılandırmaları Azure'da çalışan sanal makinelerini dağıtmak müşteriler için bırakın karar vermiştir. Ancak, dağıtım ve yaşam döngüsü yönetimi izleme bileşenlerinin çoğunlukla Azure tarafından otomatik olarak yapılır.

#### <a name="solution-design"></a>Çözüm tasarımı
SAP izlemeyi etkinleştirmek için geliştirilen çözüm Azure VM aracısı ve uzantısı çerçevesini mimarisini temel alır. Azure VM aracısı ve uzantısı framework'ün bir Azure VM uzantısı galerisinde kullanılabilir yazılım uygulamaları yüklenmesine izin verecek şekilde olur. Bu kavram arkasında ilkesine fikir (gibi durumlarda Azure Monitoring uzantısı SAP için), dağıtım VM özel işlevsellik ve yazılımla yapılandırma dağıtım zamanında izin vermektir.

'Azure VM VM dahilinde belirli Azure VM uzantıları işlenmesini sağlayan Aracısı' varsayılan olarak, Azure portalında VM oluşturulduktan Windows VM'ler eklenen. SUSE, Red Hat veya Oracle Linux durumunda, VM Aracısı zaten Azure Market görüntüsü parçasıdır. Şirket içi bir Linux VM Azure karşıya durumda VM Aracısı elle yüklenmesi gerekir.

SAP görünümler için azure'da izleme çözümünün temel yapı taşlarının böyle:

![Microsoft Azure uzantısı bileşenleri][planning-guide-figure-2400]

Yukarıdaki blok diyagramda gösterildiği gibi SAP için izleme çözümünün bir parçası Azure VM görüntüsü ve Azure işlemleri tarafından yönetilen bir genel çoğaltılan depo Azure uzantısı galerisinde barındırılır. Azure Monitoring uzantısı SAP için yeni sürümlerini yayımlamak için Azure işlemleri çalışmak için SAP Azure uygulamasının çalıştığı birleşik SAP/MS takım sorumluluğundadır.

Yeni bir Windows VM dağıttığınızda, Azure VM Aracısı VM otomatik olarak eklenir. Bu aracı yükleme ve yapılandırma SAP NetWeaver sistemleri izlemek için Azure uzantıların koordine etmek için işlevidir. Linux VM'ler için Azure VM Aracısı zaten Azure Market işletim sistemi görüntüsü parçasıdır.

Ancak, yine müşteri tarafından yürütülmesi gereken bir adım yoktur. Bu etkinleştirme ve yapılandırma performans koleksiyonunun adıdır. İşlem yapılandırması ile ilgili bir PowerShell komut dosyası veya CLI komutu tarafından otomatik hale getirilmiştir. Bölümünde açıklandığı gibi Microsoft Azure Script Center PowerShell Betiği indirilebilir [Dağıtım Kılavuzu'na][deployment-guide].

SAP için Azure izleme çözümünün Genel mimarisi şuna benzer:

![SAP NetWeaver için izleme çözümü azure][planning-guide-figure-2500]

**Tam nasıl yapılır ve dağıtımları sırasında bu PowerShell cmdlet'lerini veya CLI komutunu kullanarak, ayrıntılı adımlar için verilen yönergeleri izleyin [Dağıtım Kılavuzu'na][deployment-guide].**

### <a name="integration-of-azure-located-sap-instance-into-saprouter"></a>Azure tümleştirilmesi SAProuter SAP örneğine bulunan
Azure'da çalışan SAP örnekleri de SAProuter erişilebilir olması gerekir.

![SAP yönlendirici ağ bağlantısı][planning-guide-figure-2600]

Bir SAProuter TCP/IP'yi sistemleri arasındaki iletişimi doğrudan IP bağlantısı yoksa katılımcı sağlar. Bu, uçtan uca bağlantı iletişim iş ortakları arasında ağ düzeyinde gerekli olduğunu avantajı sağlar. SAProuter varsayılan 3299 numaralı bağlantı noktasında dinliyor.
SAP örnekleri SAProuter bağlanmak için bağlantı girişimi SAProuter dize ve ana bilgisayar adıyla vermeniz gerekir.

## <a name="sap-netweaver-as-java"></a>SAP NetWeaver AS Java
Şu ana kadar belgenin odak SAP NetWeaver genel yanıtlandı veya SAP NetWeaver ABAP yığın. Küçük Bu bölümde, SAP Java yığını için belirli konuları listelenmiştir. En önemli SAP NetWeaver özel olarak tabanlı Java uygulamalarını SAP Enterprise Portal biridir. SAP NetWeaver ABAP ve Java yığınları SAP PI ve SAP çözüm Manager kullanmak gibi diğer SAP NetWeaver tabanlı uygulamaları. Bu nedenle, kesinlikle yoktur gereksinimi de SAP NetWeaver Java yığınıyla ilgili belirli yönlerini göz önünde bulundurun.

### <a name="sap-enterprise-portal"></a>SAP Enterprise Portal
Şirket içi senaryolarda dağıtıyorsanız Kurulum SAP portalı bir Azure sanal makine üzerinde bir şirket içi yüklemesinden farklı değildir. DNS şirket tarafından yapılır olduğundan, bağlantı noktası ayarlarını ayrı ayrı örnekleri yapılandırılmış şirket içi yapılabilir. Şu ana kadar bu belgede açıklanan kısıtlamaları ve öneriler SAP Enterprise Portal veya SAP NetWeaver Java yığını gibi bir uygulama için genel olarak uygulanır.

![Sunulan SAP portalı][planning-guide-figure-2700]

Sanal makine konağına siteden siteye VPN tüneli ya da ExpressRoute aracılığıyla şirket ağına bağlıyken özel dağıtım senaryosu bazı müşteriler tarafından doğrudan maruz SAP Enterprise Portal'ın Internet ' dir. Böyle bir senaryo için belirli bağlantı noktalarını açın ve güvenlik duvarı veya ağ güvenlik grubu tarafından engellenmediğinden emin olmak zorunda. Aynı mekanizması, şirket içi bir yalnızca bulut senaryosunda bir SAP Java örneğine bağlanmak istediğinizde uygulanması gerekir.

İlk URI http (s) portalıdır:`<Portalserver`>: 5XX00/irj burada bağlantı noktası biçimlendirilmiş 50000 tarafından artı (Systemnumber?? 100). Varsayılan portal URI, SAP sistem 00 `<dns name`>.`<azure region` >.Cloudapp.azure.com:PublicPort/irj. Daha fazla ayrıntı için göz atın sahip <http://help.sap.com/saphelp_nw70ehp1/helpdata/de/a2/f9d7fed2adc340ab462ae159d19509/frameset.htm>.

![Uç nokta yapılandırması][planning-guide-figure-2800]

URL ve/veya bağlantı noktaları, SAP Enterprise Portal'ın özelleştirmek istiyorsanız, bu belgeleri gözden geçirin:

* [Değişiklik portalı URL'si](http://wiki.scn.sap.com/wiki/display/EP/Change+Portal+URL)
* [Varsayılan bağlantı noktası numaralarını, Portal bağlantı noktası numaralarını değiştirme](http://wiki.scn.sap.com/wiki/display/NWTech/Change+Default++port+numbers%2C+Portal+port+numbers)

## <a name="high-availability-ha-and-disaster-recovery-dr-for-sap-netweaver-running-on-azure-virtual-machines"></a>Yüksek kullanılabilirlik (HA) ve Azure sanal makinelerde çalışan SAP NetWeaver için olağanüstü durum kurtarma (DR)
### <a name="definition-of-terminologies"></a>Terimler tanımı
Terim **yüksek kullanılabilirlik (HA)** genellikle iş sürekliliği BT hizmetlerinin yoluyla sağlayarak BT kesintilerini en aza indirir teknoloji ilgili yedekli, hataya dayanıklı veya yük devretme korumalı bileşenleri içinde **aynı** veri merkezi. Örneğimizde, içinde bir Azure bölgesi.

**Olağanüstü Durum Kurtarma (DR)** BT Hizmetleri kesintiyi en aza indirmenizi ve bunların kurtarma ayrıca ancak çapraz hedeflediği **farklı** genellikle veri merkezleri, yüzlerce kilometre uzakta bulunur. Örneğimizde genellikle aynı coğrafi bölge içindeki veya bir müşteri olarak sizin tarafınızdan yerleşik olarak farklı Azure bölgeler arasında.

### <a name="overview-of-high-availability"></a>Yüksek kullanılabilirlik genel bakış
Biz SAP iki kısma azure'da yüksek kullanılabilirlik hakkında tartışma ayırabilirsiniz:

* **Azure altyapı yüksek kullanılabilirlik**, örneğin HA (VM) işlem, ağ, depolama vb. ve onun avantajlarını SAP uygulama kullanılabilirliği artırma için.
* **SAP uygulama yüksek kullanılabilirlik**, örneğin HA, SAP yazılım bileşenleri:
  * SAP uygulama sunucuları
  * SAP ASCS/SCS örneği
  * Veritabanı sunucusu

ve Azure altyapı HA nasıl birleştirilebilir.

Azure'da yüksek kullanılabilirlik SAP SAP yüksek kullanılabilirlik için bir şirket içi fiziksel veya sanal ortamda karşılaştırıldığında bazı farklar vardır. Aşağıdaki kağıt SAP kullanılarak Windows sanallaştırılmış ortamlarda standart SAP yüksek kullanılabilirlik yapılandırmaları açıklar: <http://scn.sap.com/docs/DOC-44415>. Windows için mevcut değil gibi sapinst tümleşik SAP-HA için yapılandırmaya Linux yoktur. SAP HA ile ilgili şirket içi Linux için daha fazla bilgi burada bulabilirsiniz: <http://scn.sap.com/docs/DOC-8541>.

### <a name="azure-infrastructure-high-availability"></a>Azure altyapı yüksek kullanılabilirlik
Şu anda bir tek VM SLA % 99,9. Nasıl tek bir VM'ye kullanılabilirliğini görünebilir farklı kullanılabilir Azure SLA ürün basitçe oluşturabilirsiniz gibi bir fikir edinmek için: <https://azure.microsoft.com/support/legal/sla/>.

30 gün içinde her ay veya 43200 dakika hesaplama temelidir. Bu nedenle, %0,05 kapalı kalma süresi 21.6 dakika karşılık gelir. Her zamanki gibi farklı hizmetlerin kullanılabilirliğini aşağıdaki şekilde çarpın:

(Kullanılabilirlik hizmeti #1/100) * (kullanılabilirlik hizmeti #2/100) * (kullanılabilirlik hizmeti #3/100) 

Benzer:

(99,95/100) * (99,9/100) * (99,9/100) = 0.9975 veya %99.75 genel kullanılabilirlik.

#### <a name="virtual-machine-vm-high-availability"></a>Sanal makine (VM) yüksek kullanılabilirlik
İki tür sanal makinelerin kullanılabilirliğini etkileyebilecek Azure platformu olay vardır: planlanan Bakım ve planlanmayan Bakım.

* Planlı bakım, genel güvenilirliği, performansı ve sanal makinelerinizi çalıştıracağınız platform altyapısının güvenliğini artırmak için temel alınan Azure platformu için Microsoft tarafından oluşturulan düzenli güncelleştirmeleri olaylardır.
* Plansız bakım olayları donanım ya da sanal makineniz için temel alınan fiziksel altyapı herhangi bir yolla hatalı oluşur. Buna yerel ağ hataları, yerel disk hataları veya raf düzeyinde diğer hatalar dahildir. Bu tür bir arıza tespit edildiğinde Azure platformu, sanal makinenin sağlıklı bir fiziksel sunucuya barındırma sağlıksız fiziksel sunucudan sanal makineniz otomatik olarak geçirir. Bu tür olaylar nadirdir, ancak sanal makinenizin yeniden başlatılmasına da neden olabilir.

Bu belgede daha fazla ayrıntı bulunabilir: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="azure-storage-redundancy"></a>Azure depolama artıklığı
Microsoft Azure depolama hesabınızdaki veriler dayanıklılık ve yüksek kullanılabilirlik, Azure depolama SLA geçici donanım arızalarında bile karşısında toplantı emin olmak için her zaman çoğaltılır.

Azure Storage üç görüntülerini verileri varsayılan olarak engelliyor olduğundan, RAID5 veya birden çok Azure disklere RAID1 yok gerekli.

Bu makalede daha fazla ayrıntı bulunabilir: <http://azure.microsoft.com/documentation/articles/storage-redundancy/>

#### <a name="utilizing-azure-infrastructure-vm-restart-to-achieve-higher-availability-of-sap-applications"></a>SAP uygulamaları daha yüksek kullanılabilirlik elde etmek için kullanan Azure altyapı VM yeniden başlatma
Linux üzerinde Windows Server Yük Devretme Kümelemesi (WSFC) veya Pacemaker gibi işlevler kullanmamaya karar verirseniz (şu anda SLES 12 ve daha yüksek yalnızca desteklenir), Azure VM yeniden başlatma kullanılan bir SAP sistemi Azure planlanmış ve Planlanmamış kapalı kalma süresi karşı korumak için fiziksel sunucu altyapısı ve genel temel Azure platformu.

> [!NOTE]
> Azure VM yeniden öncelikle VM'ler ve uygulamaları korur, anlatılması önemlidir. VM yeniden SAP uygulamalar için yüksek kullanılabilirlik sağlamaz, ancak belirli bir düzeyde altyapı kullanılabilirliğini ve SAP sistemleri, bu nedenle dolaylı olarak yüksek kullanılabilirlik sağlar. VM konak planlanmış veya planlanmamış kesinti sonra yeniden başlatmak için sürer saat için hiçbir SLA bulunmaktadır. Bu nedenle, bu yöntem, yüksek kullanılabilirlik (A) SCS veya DBMS gibi bir SAP sisteminin kritik bileşenleri için uygun değil.
>
>

Başka bir önemli altyapısı için yüksek kullanılabilirlik depolama öğesidir. Örneğin Azure depolama SLA %99,9 kullanılabilirlik olur. Varsa, disklerle tüm VM'ler tek Azure depolama hesabı, olası Azure Storage, Azure depolama hesabında yerleştirilir tüm sanal makineleri ve bu VM'lerin içinde çalışan aynı zamanda tüm SAP bileşenleri kullanılamama kullanılamazlık neden olacak içine dağıtır.  

Tüm sanal makineleri tek tek Azure Storage hesabınıza yerine koyma, ayrılmış bir depolama de kullanabilirsiniz hesapları her VM için ve bu şekilde, birden çok bağımsız Azure depolama hesapları kullanarak genel VM ve SAP uygulama kullanılabilirliği artırın.

Azure yönetilen diskleri hata etki alanı için bağlı sanal makinenin otomatik olarak yerleştirilir. Yerleştirdiğiniz iki sanal makine bir kullanılabilirlik kümesi ve yönetilen diskleri kullanın, platform farklı hata etki alanlarına da yönetilen diskleri dağıtma ilgilenebilmek. Premium depolama kullanmayı planlıyorsanız, diskleri yönetme de kullanarak öneririz.

Azure altyapı HA ve depolama hesapları kullanan bir SAP NetWeaver sisteminin örnek mimarisi şöyle:

![Azure altyapı SAP uygulama daha yüksek kullanılabilirlik elde etmek için HA kullanma][planning-guide-figure-2900]

Azure altyapı HA ve yönetilen diskleri kullanan bir SAP NetWeaver sisteminin örnek mimarisi şöyle:

![Azure altyapı SAP uygulama daha yüksek kullanılabilirlik elde etmek için HA kullanma][planning-guide-figure-2901]

Kritik SAP bileşenleri için aşağıdaki kadarki elde:

* SAP uygulama sunucuları (AS) yüksek kullanılabilirlik

  SAP uygulama sunucu örnekleri yedekli bileşenleridir. Her SAP örneği farklı bir Azure hata ve yükseltme etki alanında çalışan kendi VM üzerinde dağıtıldığında (bölümlere bakın [hata etki alanlarını] [ planning-guide-3.2.1] ve [yükseltme etki alanları][planning-guide-3.2.2]). Bu Azure kullanılabilirlik kümeleri kullanarak sağlamış (bölüm bkz [Azure kullanılabilirlik kümeleri][planning-guide-3.2.3]). Olası planlanmış veya planlanmamış kullanılamama Azure hatası veya yükseltme etki alanı kendi SAP AS VM'ler sınırlı sayıda kullanılamama neden olacak örnekleri.

  Örnek, kendi Azure depolama hesabında yerleştirilir - bir Azure depolama hesabı potansiyel olarak kullanım dışı kalması kendi SAP AS ile yalnızca bir VM kullanılamama neden olacak şekilde her SAP örneği. Ancak, bir Azure depolama hesapları sınırı içinde bir Azure aboneliği olduğunu unutmayın. (A) SCS örneğinde Autostart parametre kümesini emin olun (A) SCS örneği otomatik olarak Başlat VM yeniden başlatmanın ardından emin olmak için bölümde açıklanan profili Başlat [SAP örnekleri için Otomatik Başlat'ı kullanarak][planning-guide-11.5].
  Lütfen bölüm okuyun [SAP uygulama sunucuları için yüksek kullanılabilirlik] [ planning-guide-11.4.1] daha fazla ayrıntı için.

  Yönetilen diskleri kullansanız bile, bu diskler ayrıca bir Azure depolama hesabında depolanır ve bir depolama kesinti bir olayda kullanılamıyor olabilir.

* *Daha yüksek* SAP kullanılabilirlik (A) SCS örneği

  Burada yüklü SAP (A) SCS örneğiyle VM korumak için Azure VM yeniden kullanır. Azure planlanmış veya planlanmamış kesinti durumunda sunucularının, sanal makineleri başka bir kullanılabilir sunucusunda yeniden başlatılacak. Daha önce belirtildiği gibi Azure VM yeniden öncelikle VM'ler ve uygulamaları, bu durumda (A) SCS örnekte korur. VM yeniden biz SAP (A) SCS örneğinin dolaylı olarak yüksek kullanılabilirlik ulaşması. VM yeniden başlatıldıktan sonra otomatik olarak Başlat (A) SCS örneğinin güvence altına almaya Autostart parametre bölümde açıklanan (A) SCS örneği başlangıç profilinde ayarladığınızdan emin olun [SAP örnekleri için Otomatik Başlat'ı kullanarak][planning-guide-11.5]. Bir tek tek bir VM'de çalıştıran bir hata noktası (SPOF) olarak (A) SCS örnek bunun anlamı tüm SAP yatay kullanılabilirliğini determinative faktör olacaktır.

* *Daha yüksek* DBMS sunucu kullanılabilirliği

  Burada, SAP (A) SCS örnek kullanım örneğine benzer, biz VM yüklü DBMS yazılım ile korumak için Azure VM yeniden kullanmak ve biz DBMS yazılımın VM yeniden aracılığıyla yüksek kullanılabilirlik elde.
  Tek bir VM'de çalıştıran DBMS ayrıca bir SPOF ve tüm SAP yatay kullanılabilirliğini determinative çarpanını şeklindedir.

### <a name="sap-application-high-availability-on-azure-iaas"></a>SAP Azure Iaas uygulama yüksek kullanılabilirlik
Tam SAP sistem yüksek kullanılabilirlik elde etmek için örnek olarak yedekli SAP uygulama sunucularını ve SAP (A) SCS örneği ve DBMS gibi benzersiz bileşenler (örneğin tek hata noktası) için tüm kritik SAP sistem bileşenleri korumak ihtiyacımız.

#### <a name="5d9d36f9-9058-435d-8367-5ad05f00de77"></a>SAP uygulama sunucuları için yüksek kullanılabilirlik
SAP uygulama sunucuları/iletişim örnekleri için belirli yüksek kullanılabilirlik çözümü hakkında düşünmek gerekli değildir. Yüksek kullanılabilirlik artıklık ve böylece yalnızca elde yeterli bunlardan farklı sanal makinelere sahip. Bunların tümü aynı Azure kullanılabilirlik önlemek için kümesinde VM'ler planlanmış bakım kapalı kalma süresi sırasında aynı anda güncelleştirilebilir yerleştirilmelidir. Farklı yükseltme ve bir Azure ölçek birimi hata etki alanlarını temel işlevselliği zaten bölümde sunulan [yükseltme etki alanları][planning-guide-3.2.2]. Azure kullanılabilirlik kümeleri bölümde sunulan [Azure kullanılabilirlik kümeleri] [ planning-guide-3.2.3] bu belgenin.

Hiçbir sonsuz sayıda hata ve yükseltme Azure ölçek birimi içinde Azure kullanılabilirlik kümesi tarafından kullanılan etki alanları yoktur. Bu, bir kullanılabilirlik kümesi, er geç birden fazla VM aynı hatası veya yükseltme etki alanında sona eriyor içine VM'lerin sayısını koyma anlamına gelir.

Birkaç SAP uygulama sunucu örneklerinde kendi özel VM'ler dağıtma ve beş yükseltme etki alanları elde ettiğinizi varsayarak, aşağıdaki resimde sonunda ortaya çıkar. Hataya ve güncelleştirme etki alanı bir kullanılabilirlik kümesi içinde gerçek maksimum sayısı gelecekte değişebilir:

![HA azure'da SAP uygulama sunucuları][planning-guide-figure-3000]

Bu belgede daha fazla ayrıntı bulunabilir: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="high-availability-for-the-sap-ascs-instance-on-windows"></a>Windows SAP (A) SCS örneğinde için yüksek kullanılabilirlik
Windows Server Yük devretme kümesi (WSFC) SAP (A) SCS örneğini korumak için sık kullanılan bir çözümdür. Ayrıca, bir "HA yükleme" biçiminde sapinst içine tümleşiktir. Bu anda Azure altyapı şirket içi yapmıştır gibi gerekli Windows Server Yük devretme aynı şekilde ayarlamak için işlevselliği sağlamak mümkün değil.

Ocak 2016 itibariyle Windows işletim sistemi çalıştıran Azure bulut platformu iki Azure VM'ler arasında paylaşılan bir disk üzerindeki Küme Paylaşılan birimi kullanma olasılığını sağlamaz.

Geçerli bir çözüm ancak WSFC tümleştirilebilir zaman uyumlu ve saydam disk çoğaltma tarafından paylaşılan bir birim sağlayan 3. taraf yazılım kullanımdır. Bu yaklaşım, yalnızca etkin küme düğümünde bir noktada disk kopyaları birini sürede erişebiliyor olduğunu gösterir. Bu HA Ocak 2016 itibariyle yapılandırması SIOS DataKeeper 3. taraf yazılımlarla birlikte Windows konuk işletim sistemi Azure vm'lerinde SAP (A) SCS örneğinde korumak için desteklenir.

SIOS DataKeeper çözüm sağlayarak Windows Yük devretme kümeleri için paylaşılan disk küme kaynağı sağlar:

* Her bir Windows küme yapılandırmasında olan sanal makineler (VM'ler) bağlı bir ek Azure VHD
* Her iki VM düğümler üzerinde çalışan SIOS DataKeeper Cluster Edition
* SIOS DataKeeper Cluster Edition, zaman uyumlu olarak ek VHD içeriğini yansıtan bir şekilde yapılandırılmış olan ek VHD VM'ler kaynağından ekli birime hedef VM hacmi bağlı.
* SIOS DataKeeper kaynak ve hedef yerel birimleri özetleyen ve bunları tek bir paylaşılan disk olarak Windows Yük devretme kümesine sunma.

SIOS DataKeeper ve SAP içinde ile Windows Yük devretme kümesi yükleme hakkında tüm Ayrıntılar bulabilirsiniz [SAP ASCS SIOS DataKeeper ile azure'da Windows Server Yük devretme kullanarak örneğini Kümeleme] [ ha-guide-classic] teknik incelemesinde yanıtlanmıştır.

#### <a name="high-availability-for-the-sap-ascs-instance-on-linux"></a>Linux SAP (A) SCS örneğinde için yüksek kullanılabilirlik
DEC 2015'ten itibaren de paylaşılan disk üzerinde Azure Linux VM'ler için WSFC eşdeğeri yoktur. Alternatif çözümleri için SIOS Windows gibi 3. taraf yazılım kullanarak SAP Linux Azure üzerinde çalışan için henüz doğrulanmaz.

#### <a name="high-availability-for-the-sap-database-instance"></a>SAP veritabanı örneği için yüksek kullanılabilirlik
Normal SAP DBMS HA Kurulum iki DBMS DBMS yüksek kullanılabilirlik işlevselliğini verilerini active DBMS örneğinden ikinci VM'ye pasif DBMS örneğine çoğaltmak için kullanıldığı VM temel alır.

DBMS için yüksek kullanılabilirlik ve olağanüstü durum kurtarma işlevsellik genel de gibi belirli DBMS açıklanmıştır [DBMS Dağıtım Kılavuzu'na][dbms-guide].

#### <a name="end-to-end-high-availability-for-the-complete-sap-system"></a>Tam SAP sistem için uçtan uca yüksek kullanılabilirlik
Bir tam SAP NetWeaver HA mimarisinde Azure - iki örnekleri şunlardır biri Windows ve Linux için bir tane.

Yönetilmeyen yalnızca diskler: aşağıda açıklandığı gibi kavramları ve dağıtılan VM sayısı depolama hesapları abonelik başına en fazla sınırı aşan birçok SAP sistemi dağıttığınızda biraz tehlikeye gerekebilir. Böyle durumlarda, sanal makineleri VHD'ler bir depolama hesabında birleştirilecek gerekir. Genellikle VHD'ler, SAP uygulama katmanı farklı SAP sistemlerinin VM'ler birleştirerek bunu.  Biz de bir Azure depolama hesabında farklı SAP sistemlerinin farklı DBMS VM'ler farklı VHD'leri birleşik. Böylece Azure depolama hesapları IOPS sınırları göz önünde bulundurarak (<https://azure.microsoft.com/documentation/articles/storage-scalability-targets>)


##### <a name="windowslogowindows-ha-on-windows"></a>![Windows][Logo_Windows] HA Windows
![Azure Iaas SQL Server ile SAP NetWeaver uygulama HA mimarisi][planning-guide-figure-3200]

Aşağıdaki Azure yapılarından SAP NetWeaver sistem için altyapı sorunlarından etkisini en aza indirmek ve düzeltme eki uygulamayı barındırmak için kullanılır:

* Tam sistem (gerekli - DBMS katman, (A) SCS örneği ve tam uygulama katmanı aynı konumda çalıştırmanız gerekir) Azure üzerinde dağıtılır.
* Tam sistem (gerekli) bir Azure aboneliği içinde çalışır.
* Tam sistem bir Azure sanal (gerekli) ağ içinde çalışır.
* Bir SAP sistem VM'ler ayrımı üç kullanılabilirlik kümeleri içine, aynı sanal ağa ait bile tüm sanal makineleri ile mümkündür.
* Bir SAP sistem DBMS örneklerini çalışan tüm sanal makineler, bir kullanılabilirlik kümesinde ' dir. SQL Server AlwaysOn veya Oracle Data Guard gibi özellikleri kullanılır, yerel DBMS yüksek kullanılabilirlik bu yana sistem başına DBMS örneği çalıştıran birden fazla VM olduğunu varsayalım.
* DBMS örnekleri çalışan tüm sanal makineler kendi depolama hesabı kullanın. DBMS veri ve günlük dosyaları bir depolama hesabından verileri eşitlemek DBMS yüksek kullanılabilirlik işlevler kullanılarak başka bir depolama hesabına çoğaltılır. Bir depolama hesabı olarak kullanım dışı kalması bir SQL Windows Küme düğümü ancak değil tam SQL Server hizmeti kullanılamama neden olur.
* (A) bir SAP sistem SCS örneğini çalışan tüm sanal makineler, bir kullanılabilirlik kümesinde ' dir. Bir Windows Server Yük devretme kümesi (WSFC) (A) korumak için bu VM'lerin içinde yapılandırılmış SCS örneği.
* (A) SCS örnekleri çalışan tüm sanal makineler kendi depolama hesabı kullanın. (A) SCS örnek dosyaları ve SAP genel klasör bir depolama hesabından SIOS DataKeeper çoğaltma'yı kullanarak başka bir depolama hesabına çoğaltılır. Bir depolama hesabı olarak kullanım dışı kalması kullanılamazlık birinin neden olur (A) SCS Windows Küme düğümü, ancak tüm (A) SCS hizmet.
* SAP uygulama sunucusu katmanı temsil eden tüm sanal makineleri kullanılabilirlik üçüncü ayarlanır.
* SAP uygulama sunucuları çalışan tüm sanal makineler kendi depolama hesabı kullanın. Bir depolama hesabı olarak kullanım dışı kalması burada diğer SAP AS çalışmaya devam edecek bir SAP uygulama sunucusu olarak kullanım dışı kalması neden olur.

Aşağıdaki şekilde yönetilen diskleri kullanarak aynı yatay gösterilmiştir.

![Azure Iaas SQL Server ile SAP NetWeaver uygulama HA mimarisi][planning-guide-figure-3201]

##### <a name="linuxlogolinux-ha-on-linux"></a>![Linux][Logo_Linux] HA Linux
Mimarisi SAP HA Linux Azure üzerinde temel Windows yukarıda açıklandığı gibi aynıdır. Ocak 2016 itibariyle henüz Linux Azure üzerinde desteklenen SAP (A) SCS HA çözümü yoktur

Sonuç olarak Ocak 2016 itibariyle bir SAP Linux Azure sistemi bir SAP Windows Azure sistemi olarak aynı kullanılabilirlik için (A) SCS HA eksik alamayacağınız örneği ve tek örnekli SAP ana veritabanı.

### <a name="4e165b58-74ca-474f-a7f4-5e695a93204f"></a>SAP örnekleri için Otomatik Başlat'ı kullanma
SAP SAP örnekleri VM dahilinde OS başlangıcı hemen sonra başlatmak için bir işlevsellik sunmazdı. Uygulanacak adımlar SAP Bilgi Bankası makalesinde belgelenen [1909114]. Ancak, SAP ayarını kullanacak şekilde öneren değil artık örneği yeniden sırasına göre bir denetim yok, çünkü birden fazla VM varsayarak etkilenen veya VM başına birden çok örneği çalıştı. Bir VM ve sonunda yeniden tek bir VM durumunun bir SAP uygulama sunucusu örneği tipik Azure senaryosunu varsayıldığında, Autostart gerçekten kritik değildir ve bu parametre ekleyerek etkinleştirilebilir:

    Autostart = 1

SAP ABAP ve/veya Java örneğinin başlangıç profiline.

> [!NOTE]
> Autostart parametre bazı downfalls de sahip olabilir. İlgili Windows/Linux hizmet örneğinin başlatıldığında daha ayrıntılı olarak parametresi bir SAP ABAP veya Java örneği başlangıcı tetiklenir. İşletim sistemi önyüklendiğinde, kesinlikle bir durumdur. Ancak, SAP hizmetleri yeniden de SUM gibi SAP yazılım yaşam döngüsü yönetimi işlevleri için ortak bir şey diğer güncelleştirir veya veya yükseltir. Bu işlevler hiç otomatik olarak yeniden başlatılmasını örneği beklenmiyor. Bu nedenle, Autostart parametresi gibi görevleri çalıştırmadan önce devre dışı bırakılması gerekir. Autostart parametresi SCS/ASCS/CI gibi kümelenir SAP örnekleri için de kullanılmamalıdır.
>
>

SAP için autostart'ile ilgili ek bilgiler burada örnekleri bakın:

* [Başlat/Durdur SAP yanı sıra, UNIX sunucusu Başlat/Durdur](http://scn.sap.com/community/unix/blog/2012/08/07/startstop-sap-along-with-your-unix-server-startstop)
* [Başlatma ve durdurma SAP NetWeaver yönetim aracıları](https://help.sap.com/saphelp_nwpi711/helpdata/en/49/9a15525b20423ee10000000a421938/content.htm)
* [Otomatik Başlat, HANA veritabanına etkinleştirme](http://www.freehanatutorials.com/2012/10/how-to-enable-auto-start-of-hana.html)

### <a name="larger-3-tier-sap-systems"></a>Daha büyük 3 katmanlı SAP sistemleri
3 katmanlı SAP yapılandırmaları yüksek kullanılabilirlik yönlerini zaten önceki bölümlerde ele. Ancak sistemleri DBMS sunucu gereksinimleri Azure, ancak SAP uygulama katmanı bulunan için çok büyük olduğu Azure'da ne dağıtılamıyor?

#### <a name="location-of-3-tier-sap-configurations"></a>3 katmanlı SAP yapılandırmaları konumu
Uygulama katmanı bölmek için desteklenmeyen kendisi veya uygulama ve DBMS katmanı şirket içi ve Azure arasında. Ya da tamamen şirket içinde dağıtılabilir bir SAP sistemidir veya Azure'da. Bu da bazı Azure'da şirket içi ve diğerlerinin çalıştıran uygulama sunucuları için desteklenmez. Bu tartışma başlangıç noktasıdır. Biz de bir SAP sistemi ve iki farklı Azure bölgelerde dağıtılan SAP uygulama sunucusu katmanı DBMS bileşenlerinin olmasını desteklemiyor. Örneğin DBMS Orta ABD, Batı ABD ve SAP uygulama katmanında. Bu tür yapılandırmaları desteklemediğinden için SAP NetWeaver mimarisinin gecikme duyarlılık nedenidir.

Ancak, süresince geçen yıl veri merkezi iş ortaklarının geliştirilmiş ortak konumlara Azure bölgeleri. Bu ortak genellikle çok yakın fiziksel Azure veri merkezleri içinde bir Azure bölgesi yerlerdir. Kısa uzaklık ve Azure ExpressRoute aracılığıyla ortak konuma varlıkları bağlantı 2ms'den az kadar olan bir gecikme neden olabilir. Böyle durumlarda, ortak bir konum ve SAP (depolama SAN/NAS dahil) DBMS katman bulmak için Azure uygulama katmanında mümkündür. DEC 2015'ten itibaren biz tüm dağıtımları gibi yok. Ancak SAP olmayan uygulama dağıtımları farklı müşterilerle zaten bu yaklaşımı kullanarak.

### <a name="offline-backup-of-sap-systems"></a>Çevrimdışı Yedekleme, SAP sistemleri
(Katman 2 veya 3 katmanlı) var. seçilen SAP yapılandırma bağımlı yedekleme gerek olabilir. Veritabanının bir yedeğini VM'nin artı kendisini içeriği. DBMS ilgili yedeklemeleri veritabanı yöntemleriyle yapılması beklenir. Farklı veritabanları için ayrıntılı bir açıklama bulunabilir [DBMS Kılavuzu][dbms-guide]. Diğer taraftan, SAP verileri (veritabanı içeriği de dahil) bir çevrimdışı şekilde bu bölümde açıklandığı gibi çevrimiçi veya sonraki bölümde açıklandığı gibi yedeklenebilir.

Çevrimdışı Yedekleme temelde Azure portalı üzerinden VM kapatma ve temel VM disk artı VM tüm bağlı diske bir kopyasını gerektirir. Bu, VM ve onun ilişkili disk zamanı görüntüsündeki noktası korumak. Yedeklemeleri farklı bir Azure Storage hesabınıza kopyalamak için önerilir. Bu nedenle bölümde açıklanan yordamı [Azure depolama hesapları arasında diskleri kopyalama] [ planning-guide-5.4.2] bu belgenin geçerli olur.
Kapatma yanı sıra Azure portalını birini kullanarak da, Powershell veya CLI burada açıklandığı şekilde yapabilirsiniz: <https://azure.microsoft.com/documentation/articles/virtual-machines-deploy-rmtemplates-powershell/>

Bu durumda, bir geri yükleme temel VM özgün disklerinin yanı sıra temel VM silmek oluşur ve geri yönetilen diskleri için özgün depolama hesabı veya kaynak grubu için kaydedilmiş diskleri kopyalama ve sistem yeniden dağıtırken diskleri bağlı.
Bu makalede örnek Powershell bu işlemde komut dosyası gösterilmektedir: <http://www.westerndevs.com/azure-snapshots/>

Yukarıda açıklandığı gibi bir VM yedeğinin geri yeni bir donanım anahtarı oluşturduğundan yeni bir SAP lisans yüklediğinizden emin olun.

### <a name="online-backup-of-an-sap-system"></a>SAP sistemin çevrimiçi yedekleme
DBMS yedeğini açıklandığı gibi DBMS özgü yöntemleriyle gerçekleştirilen [DBMS Kılavuzu][dbms-guide].

Azure sanal makine yedekleme işlevini kullanarak diğer VM'ler SAP sistemi içinden yedeklenebilir. Azure sanal makine yedekleme erken 2015'te tanıtılan ve bu sırada Azure tam bir VM'yi yedeklemek için bir standart bir yöntemdir. Azure yedekleme ile Azure yedeklemeleri depolar ve geri yükleme bir VM'nin yeniden sağlar.

> [!NOTE]
> DEC 2015'ten itibaren VM Yedekleme kullanarak SAP için kullanılan benzersiz VM kimliği korumaz lisans. Başka bir deyişle, geri yüklenen VM yeni bir VM hem de bir değişiklik değil kaydedilmiş olan eski biri olarak kabul edilir gibi VM yedekten yeni bir SAP lisans anahtarı yüklenmesini gerektirir.
>
> ![Windows][Logo_Windows] Windows
>
> Teorik olarak, Windows VSS DBMS sistem destekliyorsa, tutarlı bir şekilde de çalıştırma veritabanları yedeklenebilir VM'ler (birim gölge kopyası hizmeti <https://msdn.microsoft.com/library/windows/desktop/bb968832(v=vs.85).aspx>) Örneğin, SQL Server yapar.
> Ancak, zaman içinde nokta geri yükler veritabanları Azure VM yedeklemelerin dayalı mümkün olmayan unutmayın. Bu nedenle, Azure VM Backup kalmak yerine DBMS işlevsellikle veritabanlarının yedeklerini gerçekleştirmek için önerilir.
>
> İle hakkında bilgi edinmek için Azure sanal makine yedekleme Lütfen başlatın burada: <https://docs.microsoft.com/azure/backup/backup-azure-vms>.
>
> Diğer olasılıklar Microsoft veri koruma yüklü yedekleme/geri yükleme veritabanları için bir Azure VM ve Azure yedekleme Yöneticisi'nin bir birleşimini kullanmak üzeresiniz. Daha fazla bilgi şurada bulunabilir: <https://docs.microsoft.com/azure/backup/backup-azure-dpm-introduction>.  
>
> ![Linux][Logo_Linux] Linux
>
> Linux içindeki Windows VSS eşdeğeri yoktur. Bu nedenle yalnızca dosya tutarlı yedeklemeler olası ancak değil uygulamayla tutarlı yedeklemeler altındadır. SAP DBMS yedekleme yapılmalıdır DBMS işlevselliğini kullanma. SAP ilgili verileri içeren dosya sistemi, örneğin, burada açıklandığı gibi tar kullanarak kaydedilebilir: <http://help.sap.com/saphelp_nw70ehp2/helpdata/en/d3/c0da3ccbb04d35b186041ba6ac301f/content.htm>
>
>

### <a name="azure-as-dr-site-for-production-sap-landscapes"></a>Üretim SAP Windows'un için DR sitesi olarak Azure
Mid 2014 itibaren Hyper-V, System Center ve Azure çevresinde çeşitli bileşenler için Uzantılar şirket üzerinde Hyper-V tabanlı çalışan sanal makineler için DR sitesi olarak Azure kullanımını etkinleştirin.

Bu çözümün nasıl dağıtılacağına ilişkin ayrıntılı bir blog burada belgelenen: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/11/19/protecting-sap-solutions-with-azure-site-recovery.aspx>.

## <a name="summary"></a>Özet
Yüksek kullanılabilirlik Azure SAP sistemleri için önemli noktalar şunlardır:

* Bu anda, şirket içi dağıtımlarda yapılabilir gibi SAP tek hata noktası tamamen aynı şekilde korunamıyor. Bu paylaşılan kümeleri henüz Azure'da 3 taraf yazılımları kullanmadan oluşturulamıyor Disk nedenidir.
* DBMS katmanı için paylaşılan disk küme teknolojisine bağlı değildir DBMS işlevselliğini kullanmanız gerekir. Ayrıntılar bölümünde belgelenmiştir [DBMS Kılavuzu][dbms-guide].
* Hata etki alanları içindeki sorunları Azure altyapı veya ana bilgisayar bakım etkisini en aza indirmek için Azure kullanılabilirlik kümeleri kullanmanız gerekir:
  * SAP uygulama katmanı için bir kullanılabilirlik kümesi olması önerilir.
  * Kullanılabilirlik ayrı bir kümesi SAP DBMS katman olması önerilir.
  * Aynı kullanılabilirlik kümesinde farklı SAP sistemleri VM'ler için uygulamak için önerilmez.
  * Premium yönetilen diskleri kullanılması önerilir.
* SAP DBMS katman yedekleme amaçları doğrultusunda, lütfen denetleyin [DBMS Kılavuzu][dbms-guide].
* Genellikle daha hızlı basit iletişim örnekleri yeniden dağıtmak için olduğundan SAP iletişim örneği yedekleme az mantıklıdır.
* Genel bir dizinde SAP sisteminin ve onunla içeren VM farklı örneklerinin tüm profiller yedeklediğiniz anlamlı ve Windows Yedekleme ile gerçekleştirilmelidir veya, örneğin, hedef Linux üzerinde. Windows Server 2008 (R2) ve Windows Server 2012 (R2) arasındaki farklar olduğundan, daha yeni Windows Server kullanarak yedeklemek daha kolay hale serbest, Windows konuk işletim sistemi olarak Windows Server 2012 (R2) çalıştırmak için öneririz.
