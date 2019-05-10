---
title: Azure sanal makineleri planlama ve uygulama için SAP NetWeaver | Microsoft Docs
description: Azure sanal makineleri planlama ve uygulama için SAP NetWeaver
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: MSSedusch
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: d7c59cc1-b2d0-4d90-9126-628f9c7a5538
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/07/2019
ms.author: sedusch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2ddcf1f38d3d92f9d9bdd12203ebf99f20600478
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65409780"
---
# <a name="azure-virtual-machines-planning-and-implementation-for-sap-netweaver"></a>Azure sanal makineleri planlama ve uygulama için SAP NetWeaver

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

[dr-guide-classic]:https://go.microsoft.com/fwlink/?LinkID=521971

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

[ha-guide-classic]:https://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-Azvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:planning-guide.md  
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f
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

[powershell-install-configure]:https://docs.microsoft.com/powershell/azure/install-az-ps
[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../../networking/networking-overview.md
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
[storage-premium-storage-preview-portal]:../../windows/disks-types.md
[storage-redundancy]:../../../storage/common/storage-redundancy.md
[storage-scalability-targets]:../../../storage/common/storage-scalability-targets.md
[storage-use-azcopy]:../../../storage/common/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:../../linux/attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../../resource-manager-deployment-model.md
[virtual-machines-Az-versus-azuresm]:virtual-machines-linux-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md
[virtual-machines-linux-agent-user-guide]:../../extensions/agent-linux.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../extensions/agent-windows.md#command-line-options
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
[virtual-network-deploy-multinic-arm-template]:../../../virtual-network/template-samples.md
[virtual-networks-configure-vnet-to-vnet-connection]:../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../../../virtual-network/manage-virtual-network.md#create-a-virtual-network
[virtual-networks-manage-dns-in-vnet]:../../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics-windows]:../../windows/multiple-nics.md
[virtual-networks-multiple-nics-linux]:../../linux/multiple-nics.md
[virtual-networks-nsg]:../../../virtual-network/security-overview.md
[virtual-networks-reserved-private-ip]:../../../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../../../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../../../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../../../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../../../vpn-gateway/vpn-gateway-about-vpngateways.md
[vpn-gateway-site-to-site-create]:../../../vpn-gateway/vpn-gateway-site-to-site-create.md
[vpn-gateway-vpn-faq]:../../../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../../../cli-install-nodejs.md
[xplat-cli-azure-resource-manager]:../../../xplat-cli-azure-resource-manager.md
[capture-image-linux-step-2-create-vm-image]:../../linux/capture-image.md#step-2-create-vm-image

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Microsoft Azure işlem ve depolama kaynaklarını uzun tedarik döngüleri olmadan mümkün olduğunca az zamanda almaya şirketlerinin sağlar. Azure sanal makine hizmeti, SAP NetWeaver tabanlı Azure uygulamaları gibi Klasik uygulamaları dağıtın ve ek kaynaklar kullanılabilir şirket içi zorunda kalmadan, güvenilirlik ve kullanılabilirlik genişletmek şirketlerin sağlar. Azure sanal makine hizmetleri sağlayan Azure sanal makineler, şirket içi etki alanlarına, kendi özel Bulutlar ve kendi SAP sistem ortamı etkin bir şekilde tümleştirmek şirket içi ve dışı karışık bağlantı da destekler.
Bu teknik incelemede, Microsoft Azure sanal makinesi temelleri açıklanır ve azure'da SAP NetWeaver yüklemelerinden planlama ve uygulama konularına bir kılavuz sağlar ve gerçek başlatmadan önce okumak için belgeyi şekilde olmalıdır azure'da SAP NetWeaver dağıtımları.
Kağıt yüklemeleri ve SAP yazılım dağıtımları için birincil kaynakları temsil eden platformları üzerinde SAP notları ve SAP yükleme belgelerine tamamlar.

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="summary"></a>Özet
Bulut bilgi işlem, küçük şirketlerden büyük ve çok uluslu şirketlere kadar daha önemli BT sektör içinde sağlamasını yaygın olarak kullanılan bir terimdir.

Microsoft Azure paylaşılabilen çok sayıda yeni olanaklar sunan Microsoft bulut Hizmetleri platformudur. Artık kısıtlamalara takılmadan ya da teknik olmayan şekilde müşterilerin hızlı bir şekilde sağlama ve devre dışı bırakma sağlama uygulamaları bulutta bir hizmet olarak kullanabilirsiniz. Zamanlarını ve bütçelerini donanım altyapısına yatırım yapmak yerine şirket uygulama, iş süreçlerini ve aboneliğin avantajları müşteriler ve kullanıcılar için odaklanabilirsiniz.

Microsoft, Microsoft Azure Sanal Makine Hizmetleri ile kapsamlı bir Hizmet Olarak Altyapı (IaaS) platformu sunmaktadır. SAP NetWeaver tabanlı uygulamalar, Azure Sanal Makinelerinde (IaaS) desteklenmektedir. Bu teknik incelemede planlama ve tercih ettiğiniz platforma yönelik Microsoft azure'da SAP NetWeaver tabanlı uygulamaların uygulama açıklar.

Kağıt üzerinde iki ana özelliklerini odaklanır:

* İlk bölümü, Azure'da SAP NetWeaver tabanlı uygulamalar için iki desteklenen dağıtım desenleri açıklar. Ayrıca, Azure'un genel işleme aklınızda SAP dağıtımları olan açıklar.
* İkinci bölümü, ilk bölümünde açıklanan iki farklı senaryoları uygulama ayrıntıları.

Ek kaynaklar için bkz: bölüm [kaynakları] [ planning-guide-1.2] bu belgedeki.

### <a name="definitions-upfront"></a>Ön maliyet tanımları
Belgede aşağıdaki terimleri kullanırız:

* Iaas: Hizmet olarak altyapı
* PaaS: Hizmet olarak Platform
* SaaS: Bir hizmet olarak yazılım
* SAP bileşeni: tek bir SAP uygulamayı ECC, BW, çözüm Yöneticisi veya S/4HANA gibi.  SAP bileşenleri geleneksel ABAP veya Java teknolojileri ya da bir olmayan-NetWeaver tabanlı uygulama iş nesneleri gibi temel alabilir.
* SAP ortamı: bir veya daha fazla SAP bileşenleri geliştirme, QAS, eğitim, DR veya üretim gibi bir iş işlevi gerçekleştirmek için mantıksal olarak gruplandırılır.
* SAP ortamı: Bu terim, bir müşterinin tamamı SAP varlıkları başvurduğu BT yatay. SAP ortamı, tüm üretim ve üretim dışı ortamlar içerir.
* SAP sistemi: DBMS katmanı ve uygulama katmanı, örneğin, bir SAP ERP geliştirme sisteminin, SAP BW test sistemi, SAP CRM üretim sistemine vb. birleşimi. Azure dağıtımında, bu iki katmanı şirket içi ile Azure arasında bölmek için desteklenmiyor. Şirket içi anlamına gelir bir SAP sistemiyle ya da dağıtılmış veya Azure'da dağıtılır. Ancak, Azure veya şirket içine bir SAP ortamının farklı sistemleri dağıtabilirsiniz. Örneğin, SAP CRM geliştirme dağıtma ve Azure ancak SAP CRM üretim sistemi şirket içi sistemleri test edin.
* Şirket içi veya karma: Siteden siteye, çok siteli veya ExpressRoute bağlantısı şirket içi datacenter(s) ve Azure arasında olan bir Azure aboneliğine VM'ler dağıtıldığı bir senaryo açıklanır. Yaygın olarak kullanılan Azure belgeleri, bu tür dağıtımlar da açıklandığı gibi şirketler arası veya karma olarak senaryoları. Bağlantı için şirket içi etki alanları, şirket içi Active Directory/OpenLDAP ve şirket içi DNS Azure'a genişletmek için nedenidir. Şirket içi yatay aboneliğin Azure varlıkları için genişletilir. Bu uzantı, sanal makineleri şirket içi etki alanının parçası olabilir. Şirket içi etki alanının etki alanı kullanıcıları sunucularına erişebilir ve Hizmetleri (gibi hizmetleri DBMS) bu sanal makineler üzerinde çalıştırabilir. Dağıtılan VM'lerin şirket içi ve Azure dağıtılan VM'ler arasında iletişimi ve ad çözümlemesini mümkündür. Azure'a SAP varlıklar dağıtma en yaygın ve neredeyse özel durum budur. Daha fazla bilgi için [bu] [ vpn-gateway-cross-premises-options] makale ve [bu][vpn-gateway-site-to-site-create].

> [!NOTE]
> Şirket içi veya karma dağıtımlar SAP sistemlerinin SAP sistemlerini çalıştıran Azure sanal makineler, şirket içi etki alanının üyesi olduğu, üretim SAP sistemlerini için desteklenir. Şirket içi veya karma yapılandırmalar bölümleri dağıtmak için desteklenen veya SAP ortamlarını Azure'a tamamlayın. Bile tam SAP ortamı Azure'da çalışan şirket içi etki alanı ve REKLAM/OpenLDAP parçası olan bu VM'nin bulunması gerekir. 
>
>



### <a name="e55d1e22-c2c8-460b-9897-64622a34fdff"></a>Kaynakları
Azure Belgeleri'nde SAP iş yükü giriş noktası bulunamadı [burada](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started). Bu giriş noktası ile başlangıç konuları kapsayan çok sayıda makaleler bulun:

- SAP NetWeaver ve iş bir on Azure
- Azure'da çeşitli DBMS sistemleri için SAP DBMS kılavuzları
- Azure'da SAP iş yükleri için yüksek kullanılabilirlik ve olağanüstü durum kurtarma
- Azure'da SAP HANA çalıştırmaya yönelik özel yönergeler
- SAP HANA DBMS Azure HANA büyük örnekleri için belirli Kılavuzu 


> [!IMPORTANT]
> Olası başvuran SAP yükleme kılavuzlarını veya diğer SAP belgeler için bir bağlantı yerde kullanılır (başvuru InstGuide-01, bkz: <http://service.sap.com/instguides>). Önkoşullar, yükleme işlemi veya SAP belgeler ve Kılavuzlar her zaman kimler dikkatli bir şekilde belirli SAP işlevleri ayrıntılarını söz konusu olduğunda, Microsoft belgeleri yalnızca kapsar belirli görevler için SAP yazılım yüklü ve içinde çalıştırılan bir Microsoft Azure sanal makinesi.
>
>

Azure'da SAP konuyu aşağıdaki SAP notları ilgili:

| Not numarası | Unvan |
| --- | --- |
| [1928533] |Azure'da SAP uygulamaları: Desteklenen Ürünler ve boyutlandırma |
| [2015553] |Microsoft Azure üzerinde SAP: Destek önkoşulları |
| [1999351] |SAP için Gelişmiş Azure izleme sorunlarını giderme |
| [2178632] |Microsoft Azure üzerinde SAP için ölçümleri izleme anahtarı |
| [1409604] |Sanallaştırma Windows üzerinde: Gelişmiş izleme |
| [2191498] |Azure ile Linux üzerinde SAP: Gelişmiş izleme |
| [2243692] |Linux üzerinde Microsoft Azure (Iaas) sanal makine: SAP lisans sorunları |
| [1984787] |SUSE LINUX Enterprise Server 12: Yükleme notları |
| [2002167] |Red Hat Enterprise Linux 7.x: Yükleme ve yükseltme |
| [2069760] |Oracle Linux 7.x SAP yükleme ve yükseltme |
| [1597355] |Linux için takas alanı önerisi |

Ayrıca okuma [SCN Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) , Linux için tüm SAP notları içerir.

Genel varsayılan sınırlamalar ve Azure abonelikleri maksimum sınırlamaları bulunabilir [bu makalede][azure-subscription-service-limits-subscription].

## <a name="possible-scenarios"></a>Olası senaryolar
SAP çok içindeki kuruluşlara en kritik uygulamalardan biri olarak görülür. Mimari ve işlemleri bu uygulamaların çoğunlukla karmaşık ve kullanılabilirlik ve performans gereksinimlerini karşıladığını sağlanması önemlidir.

Bu nedenle kuruluşların dikkatle böyle bir iş çalıştırmak üzere seçmek için hangi bulut sağlayıcısı hakkında önemli iş üzerinde işler düşünmeniz gerekir. Azure iş kritik SAP uygulamaları ve iş süreçleri için ideal bir genel bulut platformudur. Çeşitli Azure altyapı göz önünde bulundurulduğunda, neredeyse tüm mevcut SAP NetWeaver ve S/4HANA sistemlerini Azure'a bugün barındırılabilir. Azure Vm'leri terabayta kadar bellek ve 200'den fazla CPU sunar. Azure'un sunduğu ötesinde [HANA büyük örnekleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture), en fazla 120 TB dağıtımları 24 TB'a kadar ve ölçek genişletme ANA genişleme HANA dağıtımları izin. 


Başarıyla SAP sistemlerini Azure Iaas veya Iaas genel olarak dağıtmak için geleneksel outsourcers veya barındırıcılar teklifleri ve Iaas tekliflerini arasındaki önemli farklılıkları anlamanız önemlidir. Geleneksel barındırma sağlayıcısı veya dış kaynak altyapısı (ağ, depolama ve sunucu türü) barındırmak için bir müşterinin istediği iş yüküne uyum sağlayan ise, bunun yerine iş yükü derecesinin ve doğru Azure'ı seçin müşteri veya iş ortağının sorumluluğundadır VM'ler, depolama ve ağ Iaas dağıtımları için bileşenleri.

İlk adım, müşterilerin aşağıdakileri doğrulamanız gerekir:

* SAP desteklenen Azure VM türleri
* Azure üzerinde SAP desteklenen ürünler/yayınlar
* Belirli SAP sürümleri azure'da desteklenen DBMS ve işletim sistemi sürümleri
* Farklı Azure SKU'ları tarafından sağlanan SAP aktarım hızı

Bu soruların yanıtlarını SAP Not okunabilir [1928533].

İkinci bir adım olarak, Azure kaynak ve bant genişliği sınırlamaları için şirket içi sistemlerde gerçek kaynak tüketimini karşılaştırılması gerekir. Bu nedenle, müşterilerin alanında SAP ile desteklenen Azure türleri farklı özellikleri tanımanız gerekir:

* CPU ve bellek kaynakları farklı VM türleri ve
* Farklı VM türleri IOPS bant genişliği ve
* Ağ özelliklerini farklı VM türleri.

Bu verilerden en iyi şekilde bulunabilir [burada (Linux)] [ virtual-machines-sizes-linux] ve [burada (Windows)][virtual-machines-sizes-windows].

Yukarıdaki bağlantıda sınırları listelenen etkilenebileceğini olan üst sınırları. Bu, sınırları tüm kaynakların gelmez, örneğin IOPS tüm durumlarda sağlanabilir. Ancak seçilen VM türünün CPU ve bellek kaynakları özel durumlardır. SAP tarafından desteklenen VM türleri için CPU ve bellek kaynakları zamanlı tüketim içinde VM için ayrılmış ve bu nedenle herhangi bir noktada kullanılabilir.

Microsoft Azure platformu, bir çok kiracılı platformudur. Sonuç olarak, depolama, ağ ve diğer kaynakları kiracılar arasında paylaşılır. Akıllı kısıtlama ve kota mantıksal bir kiracının başka bir kiracıda (gürültülü komşu) performansını güçlü bir şekilde etkilemesini önlemek için kullanılır. Azure platformunda SAP HANA için özellikle sertifika için Microsoft burada birden çok VM düzenli olarak aynı konaktaki için SAP çalıştırabilirsiniz çalışmaları için kaynak yalıtımı kanıtlamaları gerekir. Bant genişliği yaşadı farklarını tutmak azure'da mantıksal çalışır ancak kaynak/bant genişliğinin müşteriler, şirket içi dağıtımlarını karşılaşabilirsiniz daha büyük sapmalar tanıtmak için küçük, yüksek oranda paylaşılan platformları eğilimindedir. Azure'da bir SAP sistemiyle bir şirket içi sistemde daha büyük sapmalar karşılaşması olasılığını göz önünde bulundurulması gerekir.

Kullanılabilirlik gereksinimlerini değerlendirmek son bir adımdır. Bu durum oluşabilir, temel Azure altyapısını güncelleştirilmesi gerekir ve başlatılması Vm'leri çalıştıran konaklar gerektirir. Microsoft farklı durumlarda belgeleri [Azure sanal makineleri için bakım](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates). Nadiren azaltmak için burada Vm'leri yeniden başlatmak zorunda, ancak daha da önemli durumlar için düzeltme eki konuk işletim sistemi veya DBMS bileşenleri için ihtiyacınız geçerli yüksek kullanılabilirlik kavramlar için üretim SAP sistemlerini geliştirmek ihtiyacınız. Bu gereksinim, şirket içi yüz gereksinimlerinde farklı değil. Microsoft, kararlı bir şekilde platform değişiklikleri nedeniyle kapalı kalma süresini azaltmak üzere Azure platformunun ilerletmektedir. 

Başarıyla bir SAP sistemiyle azure'a dağıtmak için şirket içi sistemleri işletim sistemi, veritabanı, SAP ve SAP uygulamaları için Azure SAP destek matrisi görünmelidir içindeki kaynakların Azure altyapısını sağlayabilir ve hangi çalışabilir Kullanılabilirlik SLA'ları Microsoft Azure teklifleri. Bu sistemlerin tanımlandığı gibi aşağıdaki iki dağıtım senaryoları biri üzerinde karar vermeniz gerekir.





### <a name="f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10"></a>Şirket içi - dağıtımı tek veya birden çok SAP VM şirket içi ağa tam olarak tümleştirilmiş gereksiniminin azure'a
![VPN siteden siteye bağlantı (şirket içi)][planning-guide-figure-300]

Bu senaryo, birçok olası dağıtım desen ile şirketler arası bir senaryodur. Azure'da SAP yatay şirket bazı bölümlerini ve diğer bölümlerini bir SAP ortamının çalıştırma olarak açıklanabilir. SAP bileşenlerinin bir parçası, Azure üzerinde çalışıyorsa olgu tüm yönlerini, son kullanıcılar için saydam olmalıdır. Bu nedenle SAP aktarım düzeltme sisteminin (STMS), RFC iletişimi, yazdırma, (SSO gibi) güvenlik, vb. sorunsuz bir şekilde Azure üzerinde çalışan SAP sistemlerini için çalışır. Ancak içi ve dışı karışık senaryo da burada tam SAP ortamı Müşteri'nin etki alanı ile Azure'da çalışan ve DNS Azure'a genişletilmiş bir senaryo açıklanmaktadır.

> [!NOTE]
> Üretken SAP sistemlerini çalıştırmak üzere desteklenen dağıtım senaryosunu da budur.
>
>

Okuma [bu makalede] [ vpn-gateway-create-site-to-site-rm-powershell] Microsoft Azure'da şirket içi ağınıza bağlanma hakkında daha fazla bilgi için

> [!IMPORTANT]
> Azure ve şirket içi müşteri dağıtımları arasında şirketler arası senaryolar hakkında daha fazla varsayılır, tamamı SAP sistemlerini ayrıntı düzeyinde arıyoruz. Olan senaryoları *desteklenmiyor* için şirket içi senaryolar şunlardır:
>
> * SAP uygulama farklı katmanlara farklı dağıtım yöntemleri çalışır. Örneğin DBMS katman şirket, ancak SAP uygulama katmanı, Azure Vm'leri olarak veya dağıtılmış Vm'lerde çalışan.
> * Azure ve bazı şirket içi SAP katmanda bazı bileşenleri. Örneğin, şirket içi ve Azure Vm'leri arasında SAP uygulama katmanının örneklerini bölme.
> * SAP örneklerini tek bir sistem üzerinde birden fazla Azure bölgesini çalışan VM'lerin dağıtım desteklenmiyor.
>
> Bu kısıtlamalar özellikle uygulama örnekleri ile SAP sistemine DBMS katmanı arasındaki bir SAP sistemine içinde düşük gecikme süresi yüksek performanslı ağ gereksinimi nedeni.
>
> Özel sistemleri ve bölgeleri planlama, yüksek oranda tümleşik birden çok SAP sistemlerini kullanırken gerçekleşmelidir. Bu sistemler diğer ağ gecikmesini en aza indirmek için mümkün olduğunca Kapat olarak dağıtmak emin olun. Yüksek oranda tümleşik SAP sistemlerini örnekleri şunlardır:
> * SAP BW; ERP, CRM ya da SRM gibi SAP OLTP sistemlerindeki verileri okuma veya
> * SAP ve SAP olmayan sistemler arasında bile veya birden çok SPA sistemleri arasında veri çoğaltmak için kullanılan SAP SLT; veya
> * SAP S/4; bir SAP ERP sisteme bağlı VS.


### <a name="supported-os-and-database-releases"></a>Desteklenen işletim sistemi ve veritabanı yayınlar
* Azure sanal makine Hizmetleri, bu makalede listelenen için desteklenen Microsoft sunucu Yazılımları: <https://support.microsoft.com/kb/2721672>.
* İşletim sistemi sürümleri desteklenir, SAP yazılımı ile birlikte Azure sanal makine Services'ta desteklenen veritabanı sistemi sürümleri SAP notu belgelenen [1928533].
* SAP uygulamalarını ve Azure sanal makine Services'ta desteklenen sürümleri SAP notu belgelenmiştir [1928533].
* Yalnızca 64-Bit görüntüleri, Konuk sanal makineleri azure'da SAP senaryoları için çalışacak şekilde desteklenir. Sonuç olarak, yalnızca 64-bit SAP uygulamalarını ve veritabanlarını desteklenir.

## <a name="microsoft-azure-virtual-machine-services"></a>Microsoft Azure sanal makine Hizmetleri
Microsoft Azure platformu tarafından işletilen Microsoft veri merkezleri içinde ve barındırılan bir internet ölçeğindeki bulut Hizmetleri platformudur. Platformu, Microsoft Azure sanal makine Hizmetleri (bir hizmet veya Iaas olarak altyapı) ve zengin bir Platform olarak hizmet (PaaS) özellikleri kümesi içerir.

Azure platformu ön teknoloji gereksinimini azaltır ve altyapı satın. İsteğe bağlı işlem ve barındırma, ölçeklendirme ve web uygulaması ve bağlı uygulamaları yönetmek için depolama alanı sağlayarak uygulamaları bakımını yapma ve kolaylaştırır. Altyapı yönetimi, otomatik bir platformuyla yüksek kullanılabilirlik için tasarlanmıştır ve dinamik Kullandıkça Öde fiyatlandırma modelinin seçeneğiyle kullanım ihtiyaçlarını karşılamak için ölçekleme olanağı sunar.

![Microsoft Azure sanal makine hizmetlerini konumlandırma][planning-guide-figure-400]

Azure sanal makine Hizmetleri ile Microsoft Azure'da Iaas örnekleri (bkz: Şekil 4) olarak özel sunucu görüntülerini dağıtmayı etkileştirir. Azure'da sanal makineler Hyper-V sanal sabit sürücülerde (VHD) temel alır ve farklı işletim sistemleri çalıştıran konuk işletim sistemi olanağına sahip olursunuz.

Azure sanal makine hizmeti, işletimsel açısından bakıldığında, şirket içi dağıtılan sanal makineler olarak benzer deneyimler sunar. Ancak, tedarik edin, yönetmek ve altyapıyı yönetmek için ihtiyacınız olmayan önemli avantajı vardır. Geliştiriciler ve Yöneticiler, bu sanal makineler içinde işletim sistemi görüntüsünü tam denetime sahiptir. Yöneticiler uzaktan, bu sanal makinelere Bakım ve görevlerin yanı sıra yazılım dağıtım görevlerini sorun giderme gerçekleştirmek için oturum açabilir. Dağıtım in regard to yalnızca kısıtlamaları boyutları ve Azure sanal makinelerinin özellikleri var. Bu boyutları olarak ince olmayabilir yapılandırma ayrıntılı olarak şirket içi yapılabilir. Bir seçimi temsil eden bir birleşimi VM türleri verilmiştir:

* Vcpu sayısı
* Bellek
* Eklenebilecek VHD sayısı
* Ağ ve depolama bant genişlikleri

Boyut ve çeşitli farklı sanal makine boyutları sınırlamaları sunulan görülebilir bir tablodaki [bu makalede (Linux)] [ virtual-machines-sizes-linux] ve [bu makalede (Windows)] [virtual-machines-sizes-windows].

Tüm farklı VM serisi her biri, Azure bölgeleri (Azure bölgeleri sonraki bölüme bakın için) sunulan. Ayrıca tüm VM'ler veya VM serisi SAP için sertifikalı olduğunu unutmayın.

> [!IMPORTANT]
> SAP NetWeaver tabanlı uygulamaların kullanımına yalnızca VM türleri ve yapılandırmaları alt listelenen SAP Not [1928533] desteklenir.
>
>

### <a name="be80d1b9-a463-4845-bd35-f4cebdb5424a"></a>Azure bölgeleri
Sanal makinelerin dağıtıldığı içine yorumun *Azure bölgeleri*. Bir Azure bölgesi içinde olarak yakınında bulunan bir veya birden çok veri merkezleri olabilir. Jeopolitik bölgeler dünyanın çoğu için Microsoft'un en az iki Azure bölgesi vardır. Örneğin, Avrupa'da olduğu bir Azure bölgesine *Kuzey Avrupa* ve biri *Batı Avrupa*. Böylece teknik ya da doğal felaketler, hem Azure bölgeleri aynı jeopolitik bölgede etkilemez coğrafi bölge içindeki iki tür Azure bölgesi tarafından yeterince önemli uzaklık ayrılır. Microsoft kararlı bir şekilde yeni bir Azure bölgesi farklı coğrafi bölgelerde kullanıma genel yapı olduğundan, bu bölge sayısı sürekli olarak büyüyen ve aralık 2015'ten itibaren ek bölge zaten duyurulan ile 20 Azure bölge sayısı üst sınırına. İki Azure bölgesi Çin'de dahil olmak üzere tüm bu bölgeler içinde bir müşteri olarak, SAP sistemlerini dağıtabilirsiniz. Azure bölgeleri hakkında güncel güncel bilgiler için bu Web sitesine bakın: <https://azure.microsoft.com/regions/>

### <a name="8d8ad4b8-6093-4b91-ac36-ea56d80dbf77"></a>Microsoft Azure sanal makine kavramı
Microsoft Azure altyapı (Iaas) çözümü olarak benzer işlevler ile sanal makineleri barındırmak için bir şirket içi sanallaştırma çözümü sunar. Dağıtım ve yönetim özellikleri de sunan Azure portalında, PowerShell veya CLI, sanal makineleri oluşturamaz.

Azure Resource Manager, uygulamalarınızı bildirim temelli bir şablon aracılığıyla sağlamanıza olanak tanır. Tek bir şablonda birden çok hizmeti bağımlılıklarıyla birlikte dağıtabilirsiniz. Uygulama yaşam döngüsünün her aşamasında uygulamanızı tekrar tekrar dağıtmak için aynı şablonu kullanın.

Resource Manager şablonlarını kullanma hakkında daha fazla bilgi burada bulunabilir:

* [Azure Resource Manager şablonları ve Azure CLI kullanarak sanal makineleri yönetme ve dağıtma](../../linux/create-ssh-secured-vm-from-template.md)
* [Azure Resource Manager ve PowerShell kullanarak sanal makineleri yönetme][virtual-machines-deploy-rmtemplates-powershell]
* <https://azure.microsoft.com/documentation/templates/>

Başka bir ilgi çekici içerdiği, kendi gereksinimlerinize göre sanal makine örnekleri, hızlı bir şekilde dağıtmak mümkün olan belirli bir depo hazırlama olanak tanıyan sanal makinelerden görüntüleri oluşturma olanağı özelliğidir.

Sanal makinelerden görüntüleri oluşturma hakkında daha fazla bilgi bulunabilir [bu makalede (Linux)] [ virtual-machines-linux-capture-image-resource-manager] ve [bu makalede (Windows)] [ virtual-machines-windows-capture-image-resource-manager].

#### <a name="df49dc09-141b-4f34-a4a2-990913b30358"></a>Hata etki alanları
Hata etki alanları için veri merkezlerinde bulunan fiziksel altyapı yakından ilgili bir hata, fiziksel bir birimini temsil eder ve bir fiziksel dikey veya raf hata etki alanı kabul edilebilir, ancak ikisi arasındaki doğrudan bire bir eşleme yoktur.

Birden çok sanal makine bir SAP sistemi Microsoft Azure sanal makine Hizmetleri'nde bir parçası olarak dağıttığınızda, böylece gereksinimlerini karşılayan farklı hata etki uygulamanıza dağıtmak için Azure yapı denetleyicisi etkileyebilir. Microsoft Azure SLA. Ancak, Azure ölçek birimi (yüzlerce işlem düğümleri veya depolama düğümleri ve ağ koleksiyonu) üzerinde hata etki alanlarına dağıtılması veya belirli bir hata etki alanı Vm'lere atamasını üzerinde doğrudan denetim sizde değil bir şeydir. Farklı hata etki alanları üzerinde bir VM kümesi dağıtmak için Azure yapı denetleyicisi yönlendirmek için bir Azure kullanılabilirlik kümesine sanal makinelere dağıtım sırasında atamanız gerekir. Azure kullanılabilirlik kümeleri hakkında daha fazla bilgi için bkz: bölüm [Azure kullanılabilirlik kümeleri] [ planning-guide-3.2.3] bu belgedeki.

#### <a name="fc1ac8b2-e54a-487c-8581-d3cc6625e560"></a>Upgrade Domains
Yükseltme etki alanları, bir VM içinde birden çok Vm'lerde çalışan SAP örneklerinin oluşan bir SAP sistemiyle nasıl güncelleştirileceğini belirlemek için yardımcı olan bir mantıksal birimi temsil eder. Yükseltme ortaya çıktığında, Microsoft Azure bu yükseltme etki alanlarında tek tek güncelleştirme işlemi gider. Farklı yükseltme etki alanları üzerinde dağıtım sırasında Vm'leri dağıtılarak, kısmen olası kapalı kalma süresi, SAP sistemine koruyabilirsiniz. Azure sanal makinelerin farklı yükseltme etki alanlarında yayılan bir SAP sistemiyle dağıtılacak zorlamak için her sanal makine dağıtım sırasında belirli bir öznitelik kümesi gerekir. Hata etki alanları için benzer bir Azure ölçek birimi birden fazla yükseltme etki alanlarına ayrılmıştır. Farklı yükseltme etki alanları üzerinde bir VM kümesi dağıtmak için Azure yapı denetleyicisi yönlendirmek için bir Azure kullanılabilirlik kümesine sanal makinelere dağıtım sırasında atamanız gerekir. Azure kullanılabilirlik kümeleri hakkında daha fazla bilgi için bkz: bölüm [Azure kullanılabilirlik kümeleri] [ planning-guide-3.2.3] aşağıda.

#### <a name="18810088-f9be-4c97-958a-27996255c665"></a>Azure kullanılabilirlik kümeleri
Azure sanal makineler bir Azure kullanılabilirlik kümesi içinde Azure yapı denetleyicisi tarafından farklı hata ve yükseltme etki alanları üzerinde dağıtılır. Tüm sanal makinelerin bir SAP sistemiyle altyapı bakım ya da bir hata etki alanı içinde bir hata olması durumunda kapatılmadan önlemek için farklı hata ve yükseltme etki alanları üzerinde dağıtım amacı olan. Varsayılan olarak, Vm'leri bir kullanılabilirlik kümesinin parçası değildir. Bir kullanılabilirlik kümesindeki bir VM'nin katılım, dağıtım sırasında veya daha sonra yeniden yapılandırma ve yeniden dağıtma işlemi, bir VM tarafından tanımlanır.

Azure kullanılabilirlik kümeleri ve kullanılabilirlik kümeleri, hata ve yükseltme etki alanları için ilişkili bir şekilde kavramı anlamak için okuma [bu makalede][virtual-machines-manage-availability]

Kullanılabilirlik kümesi için Azure Resource Manager bir json şablonunu tanımlamak için bkz: [rest API özellikleri](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2015-06-15/swagger/compute.json) ve "kullanılabilirlik" arayın.

### <a name="a72afa26-4bf4-4a25-8cf7-855d6032157f"></a>Depolama: Microsoft Azure depolama ve veri diskleri
Microsoft Azure sanal makineleri farklı depolama türlerini kullanın. Azure sanal makine hizmetleri üzerinde SAP uygulama, bu iki ana depolama türleri arasındaki farkları anlamak önemlidir:

* Kalıcı olmayan, geçici depolama alanı.
* Kalıcı depolama alanı.

VM dağıtıldıktan sonra azure sanal makineleri kalıcı olmayan diskler sunar. VM'yi yeniden başlatma durumunda, bu sürücülerde tüm içeriği temizlenir. Bu nedenle, olan bir biçimde hiçbir koşulda veri dosyaları ve veritabanı günlük/Yinele dosyaları bu kalıcı olmayan sürücüler bulunmalıdır. Burada bu kalıcı olmayan sürücüler tempdb ve geçici açabilmek için uygun olabilir veritabanlarından bazıları için özel durumlar olabilir. Ancak, bu kalıcı olmayan sürücüler aktarım hızının, VM ailesi ile sınırlı olduğundan, A serisi VM'ler için bu sürücüleri kullanmaktan kaçının. Daha fazla ayrıntı için makaleyi okuyun [azure'da Windows VM'ler geçici sürücüyü anlama](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

- - -
> ![Windows][Logo_Windows] Windows
> 
> Azure VM'de D:\ Azure işlem düğümü üzerinde bazı yerel disk ile desteklenir, kalıcı olmayan sürücü sürücüdür. Kalıcı olmayan olduğundan, bu D:\ sürücüsüne içerikte yapılan tüm değişiklikler kayıp VM yeniden başlatıldığında anlamına gelir. "Herhangi bir değişiklik" tutulan dosyaları gibi oluşturulan dizinleri yüklü uygulamalar tarafından vb.
> 
> ![Linux][Logo_Linux] Linux
> 
> Azure sanal makineleri otomatik olarak bir sürücü üzerinde Azure işlem düğümünün yerel disk ile desteklenir ve kalıcı olmayan bir sürücü /mnt/resource bağlayın. Kalıcı olmayan olduğundan, bu sanal makine yeniden başlatıldığında /mnt/resource içerikte yapılan tüm değişiklikler kaybedilir anlamına gelir. Depolanan, dosyaları Dizinler oluşturuldu, yüklü uygulamalar gibi herhangi bir değişiklikten vb.
> 
> 

- - -

Microsoft Azure depolama, kalıcı depolama alanı ve koruma ve yedeklilik SAN depolama görülen tipik düzeyleri sunar. Azure depolama alanına dayalı olarak sanal sabit Azure depolama hizmetlerinde yer alan disk (VHD) disklerdir. Yerel işletim sistemi diski (Windows C:\, Linux/dev/sda1) Azure depolama alanında depolanır ve sanal Makineye bağlı ek birimler/diskleri, çok depolanan.

Şirket içi varolan bir VHD'yi karşıya yükleme veya azure'daki boş olanlardan oluşturup bu dağıtılan VM'ler eklemek mümkündür.

Oluşturma veya Azure Depolama'ya bir VHD'yi karşıya sonra bağlamak ve bu var olan bir sanal makineye eklemek için ve (bağlı olmayan) varolan bir VHD'yi kopyalamak için mümkündür.

Bu VHD kalıcı olarak veri ve bunlar içindeki değişiklikler yeniden başlatma ve sanal makine örneği yeniden güvenlidir. Örneği silinse bile bu Vhd'lere güvende ve dağıtılması veya işletim sistemi olmayan diskler olması durumunda, diğer sanal makinelere bağlanabilir.

Azure depolama hakkında daha fazla bilgi burada bulunabilir:

* <https://azure.microsoft.com/documentation/services/storage/>
* <https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>
* <https://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview.aspx>

#### <a name="azure-standard-storage"></a>Azure standart depolama
Azure Iaas yayımlandığında azure standart depolama kullanılabilir depolama alanı türündeydi. Zorlanan tek disk başına IOPS kotalar vardı. Genellikle yüksek kaliteli SAP sistemlerinde dağıtılan SAN/NAS cihazlarındaki şirket içinde barındırılan olarak karşılaşılan gecikme sürelerini aynı sınıfta değildi. Bununla birlikte, Azure standart depolama için yüzlerce yeterli kanıtlandı SAP sistemlerini Azure'a bu arada dağıtılmış.

Azure standart depolama hesaplarında depolanır diskleri depolanan gerçek veriler, depolama işlemleri ve giden veri aktarımları olarak yedeklilik seçeneği, seçilen hacmi üzerinden ücretlendirilir. En fazla 1 TB boyutu en fazla disk oluşturulabilir, ancak bu kısım boş bırakılır olduğu sürece ücret alınmaz. Daha sonra bir VHD 100 GB ile doldurun, VHD ile oluşturulan nominal boyutu ve 100 GB depolama için ücretlendirilir.

#### <a name="ff5ad0f9-f7f4-4022-9102-af07aef3bc92"></a>Azure Premium depolama
Azure Premium depolama sağlamak için hedef ile sunulan:

* Daha iyi g/ç gecikme süresi.
* Daha iyi aktarım hızı.
* G/ç gecikme sürelerinde daha az değişkenlik.

Bu amaç için iki en önemli olduğu çok sayıda değişikliğin yapılmadığı:

* Azure depolama düğümlerinde SSD diskleri kullanımı
* Yeni bir Azure işlem düğümünün yerel SSD'si tarafından desteklenen Önbellek Okuma

Burada özellikleri değişmedi standart depolama ters yönde bağımlı disk (veya VHD) boyutu Premium depolama şu anda bu makalede gösterilen üç farklı disk kategorisi vardır: <https://azure.microsoft.com/pricing/details/storage/unmanaged-disks/>

IOPS/disk ve disk aktarım hızı/disk disk boyutu kategoriye göre bağımlı olduğunu görürsünüz

Premium depolama söz konusu olduğunda maliyet temel tür diskler, ancak böyle bir diskin, disk içinde depolanan veri miktarı bağımsız boyutu kategori depolanan gerçek veri hacmi değil.

Diskler, Premium depolama, doğrudan gösterilen boyutu kategoriler halinde eşleme değil de oluşturabilirsiniz. Özellikle diskleri standart depolamadan Premium depolama alanına kopyalama işlemi sırasında bu durumda olabilir. Böyle durumlarda, bir sonraki en büyük Premium depolama disk seçeneğine bir eşleme gerçekleştirilir.

İle SAP sertifikalı Azure VM ailelerinden bir karışımını Azure standart ve Premium depolama arasında veya Premium depolama ile çalışabilmek için çoğu.

DS serisi Vm'lerde bulunan parçası kullanıma alınıyor, [bu makalede (Linux)] [ virtual-machines-sizes-linux] ve [bu makalede (Windows)][virtual-machines-sizes-windows], fark VM düzeyi ayrıntı düzeyi Premium depolama disklerini veri birimi sınırlamalar vardır. Ayrıca farklı DS serisi veya GS serisi VM'ler bağlanabilir ve veri diskleri sayısı ilgili farklı sınırlamaları sahip. Bu sınırlar de yukarıdaki makalesinde belirtilmiştir. Ancak temelde, örneğin, tek bir DS14 VM 32 x P30 disklere bağlamanız halinde P30 disk maksimum aktarım hızı x 32 alınamıyor anlamına gelir. Bunun yerine makalesinde belgelendiği gibi VM düzeyi en yüksek aktarım veri çıkışını sınırlandırır.

Premium depolama hakkında daha fazla bilgi burada bulunabilir: <https://azure.microsoft.com/blog/2015/04/16/azure-premium-storage-now-generally-available-2>

#### <a name="azure-storage-accounts"></a>Azure Depolama Hesapları

Hizmetleri veya azure'da sanal makineler dağıtırken dağıtım VHD'ler ve VM görüntüleri Azure depolama hesapları olarak adlandırılan birimler halinde düzenlenebilir. Bir Azure dağıtımındaki planlarken, dikkatli bir şekilde Azure kısıtlamalarını dikkate almanız gerekir. Bir yandan, depolama hesapları sınırlı sayıda Azure aboneliği başına yoktur. Her Azure depolama hesabı, çok sayıda VHD dosyaları içerebilir ancak var. sabit bir sınır üzerindeki depolama hesabı başına toplam IOPS Yüzlerce SAP VM önemli GÇ çağrıları oluşturma DBMS sistemleri ile dağıtım yaparken, birden çok Azure depolama hesapları arasında yüksek IOPS DBMS Vm'leri dağıtmak için önerilir. Azure depolama hesapları her abonelik için geçerli sınır aşmayacak şekilde dikkatli olunması gerekir. Depolama için bir SAP sistemiyle veritabanı dağıtım sürecinin hayati bir parçası olduğundan, bu kavramı zaten başvurulmuş daha ayrıntılı anlatılan [DBMS Dağıtım Kılavuzu][dbms-guide].

Azure depolama hesapları hakkında daha fazla bilgi bulunabilir [bu makalede][storage-scalability-targets]. Bu makaleyi okuduktan, Azure standart depolama hesapları ve Premium depolama hesapları arasındaki farklar ve sınırlamalar, tatilini andıran başlığımız. Başlıca farklar bu tür bir depolama hesabında depolanan veri hacminin ' dir. Standart depolama alanında, Premium depolama ile büyük bir boyuta birimdir. Diğer tarafta standart depolama hesabı ciddi bir şekilde IOP cinsinden sınırlıdır (sütununa bakın **toplam istek oranı**), Azure Premium depolama hesabı herhangi bir kısıtlama sahipken. Ayrıntılarını ve bu farklar sonuçlarını SAP sistemlerini, özellikle DBMS sunucuları dağıtımları tartışırken ele alınacaktır.

Bir depolama hesabı içinde düzenleme ve farklı VHD halinde kategorilere ayrılması amacıyla farklı kapsayıcılar oluşturmak için olanağına sahip olursunuz. Bu kapsayıcılar, örneğin, farklı vm'lere ayrı VHD'ler için kullanılır. Yalnızca bir kapsayıcı ya da tek bir Azure depolama hesabı altında birden çok kapsayıcı kullanarak hiçbir performans etkileri vardır.

Azure içinde Azure içindeki VHD için benzersiz bir ad sağlamanız gereken aşağıdaki adlandırma bağlantısı VHD adı aşağıdaki gibidir:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

Azure depolama alanında depolanan VHD benzersiz olarak tanımlanabilmesi yukarıdaki dize gerekir.

#### <a name="c55b2c6e-3ca1-4476-be16-16c81927550f"></a>Yönetilen diskler

Yeni bir kaynak türü Azure kaynak Azure depolama hesaplarında depolanan VHD yerine kullanılabilecek Yöneticisi'nde yönetilen disklerdir. Yönetilen diskler, otomatik olarak bağlandıkları sanal makinenin kullanılabilirlik kümesi ile hizalanan ve bu nedenle, sanal makine ve sanal makine üzerinde çalışan hizmetleri kullanılabilirliğini artırın. Daha fazla bilgi için okuma [genel bakış makalesi](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview).

Öneririz dağıtım ve Yönetim sanal makinelerinizin basitleştirmek için yönetilen disk kullanın.
Şu anda SAP Premium yönetilen diskler yalnızca destekler. Daha fazla bilgi için ' lu SAP notuna okuma [1928533].

#### <a name="microsoft-azure-storage-resiliency"></a>Microsoft Azure depolama dayanıklılık

Microsoft Azure depolama, en az üç farklı depolama düğümlerinde (OS ile) temel VHD ve bağlı diskleri veya BLOB'ları depolar. Bu durum, yerel olarak yedekli depolama (LRS) olarak adlandırılır. Azure depolama tüm türleri için varsayılan değer lrs'dir. 

Tüm makalesinde açıklanan birkaç daha fazla yedeklilik yöntemleri vardır [Azure depolama çoğaltma](https://docs.microsoft.com/azure/storage/common/storage-redundancy?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).

> [!NOTE]
>Azure Premium depolama DBMS VM'ler ve veritabanı ve günlük/Yinele dosyaları depolama diskleri için önerilen türü olan depolama itibariyle, yalnızca kullanılabilir yöntem lrs'dir. Sonuç olarak, SQL Server Always On, Oracle Data Guard veya gibi HANA sistem çoğaltması veritabanı başka bir Azure bölgesi veya başka bir Azure kullanılabilirlik alanı veri çoğaltmayı etkinleştirmek için veritabanı yöntemlerini yapılandırmak gerekir.


> [!NOTE]
> Ciddi performans etkisi ve yazma sırası arasında bir VM'ye bağlı farklı VHD dikkate almaz olduğundan DBMS dağıtımları için Azure standart depolama ile kullanılabilir olarak coğrafi olarak yedekli depolama kullanımı önerilmez. Yazma sırasını arasında farklı VHD uygularken değil, olgu yüksek olası bir veritabanı ve günlük/Yinele dosyaları (çoğu durumda gibi) birden çok VHD VM yan kaynak arasında yayılır, tutarsız veritabanlarını çoğaltma hedef tarafta düştüğünden taşıyan.


### <a name="61678387-8868-435d-9f8c-450b2424f5bd"></a>Microsoft Azure ağı

Microsoft Azure ile SAP yazılım hayata geçirmek için istediğimiz tüm senaryoları eşleme sağlayan bir ağ altyapısı sağlar. Özellikleri şunlardır:

* Windows Terminal Hizmetleri ya da ssh/VNC Vm'lere için doğrudan, dışarıdan erişim
* Access Hizmetleri ve sanal makinelerin içinde uygulamalar tarafından kullanılan belirli bağlantı noktaları
* İç iletişim ve bir grup Azure Vm'leri olarak dağıtılan VM'ler arasındaki ad çözümlemesi
* Bir müşterinin şirket içi ağınız ve Azure ağı arasında şirketler arası bağlantı
* Azure siteler arasında çapraz Azure bölgesi veya veri merkezi bağlantısı

Buradan daha fazla bilgi bulabilirsiniz: <https://azure.microsoft.com/documentation/services/virtual-network/>

Azure'daki adı ve IP çözümü yapılandırmak için birçok farklı olasılık vardır. Kendi DNS sunucunuzu ayarlamak yerine kullanılabilecek bir Azure DNS hizmeti de mevcuttur. Daha fazla bilgi bulunabilir [bu makalede] [ virtual-networks-manage-dns-in-vnet] ve [bu sayfayı](https://azure.microsoft.com/services/dns/).

Şirket içi veya hibrit senaryoları, biz bağlı olan olgu üzerinde şirket içi AD/OpenLDAP/DNS genişletilmişse VPN veya özel bağlantı Azure'a. Belirli senaryolar aşağıda belirtildiği gibi Azure üzerinde yüklü bir AD/OpenLDAP çoğaltma gerekli olabilir.

Ağ iletişimi olduğundan ve ad çözümlemesi için bir SAP sistemiyle veritabanı dağıtım sürecinin hayati bir parçası olduğundan, bu kavram daha ayrıntılı olarak ele alınan [DBMS Dağıtım Kılavuzu][dbms-guide].

##### <a name="azure-virtual-networks"></a>Azure Sanal Ağları

Bir Azure sanal ağı oluşturarak, özel IP adresleri Azure DHCP işlevselliğe göre ayrılmış adres aralığı tanımlayabilirsiniz. Şirketler arası senaryolarda tanımlanan IP adresi aralığı yine de DHCP kullanarak Azure tarafından ayrılmış. Ancak, etki alanı adı çözümlemesi (VM'ler bir şirket içi etki alanının bir parçası olduğunu varsayarak) şirket yapılır ve bu nedenle Azure Cloud Services'ın farklı ötesinde adresleri çözebilirsiniz.

Her sanal makine azure'da bir sanal ağa bağlı olması gerekir.

Daha fazla ayrıntı bulunabilir [bu makalede] [ resource-groups-networking] ve [bu sayfayı](https://azure.microsoft.com/documentation/services/virtual-network/).


> [!NOTE]
> Bir VM dağıtıldıktan sonra varsayılan olarak, sanal ağ yapılandırması değiştirilemiyor. TCP/IP'yi ayarları, Azure DHCP sunucusuna bırakılmalıdır. Dinamik IP ataması varsayılan davranışıdır.
>
>

MAC adresini sanal ağ kartı, örneğin yeniden boyutlandırma ve işletim sistemi yeni ağ kartı seçer ve otomatik olarak bu durumda IP ve DNS adreslerini atamak için DHCP kullanan Windows veya Linux Konuk sonra değişebilir.

##### <a name="static-ip-assignment"></a>Statik IP ataması
Bir Azure sanal ağ içinden vm'lere sabit veya ayrılmış IP adresleri atamak mümkündür. Vm'leri bir Azure sanal ağında çalışan bu işlevselliğinden gerekli ya da bazı senaryolarda gerekli için harika bir olasılık açılır. IP ataması VM çalışıyor veya kapatma olup bağımsız olarak VM varlığını geçerli kalır. Sonuç olarak, sanal ağ için IP adresi aralığı tanımlarken, sanal makineleri (VM'ler çalışan ve durdurulmuş) genel sayısını dikkate almak gerekir. VM ve ağ arabirimiyle silinene ya da IP adresi yeniden XML'deki atanmış kadar IP adresi atanmış olarak kalır. Daha fazla bilgi için okuma [bu makalede][virtual-networks-static-private-ip-arm-pportal].

> [!NOTE]
> Azure yol aracılığıyla statik IP adresleri atamasını bireysel Vnıc'ler için. Konuk işletim sistemi içinde statik IP adresleri için bir Vnıc atamanız gerekir değil. Olgu üzerinde bazı Azure Hizmetleri gibi Azure Backup hizmeti kullanan, en azından birincil Vnıc DHCP ve statik IP adresleri için ayarlanır. Ayrıca bkz [sanal makine yedekleme sorunlarını giderme Azure](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking).
>
>

##### <a name="multiple-nics-per-vm"></a>VM başına birden çok NIC

Bir Azure sanal makinesi için birden çok sanal ağ arabirim kartları (Vnıc) tanımlayabilirsiniz. Ağ trafiği ayarlamaya başlayabilirsiniz çoklu Vnıcs sahip olanağı, örneğin, istemci trafiğini bir Vnıc ve arka uçta trafiği yönlendirildiği ayrımı ikinci bir Vnıc yönlendirilir. VM üzerinde var. Vnıc'ler sayısını ilgili farklı sınırlamaları bağımlı türüdür. Hakkında tam Ayrıntılar, işlevsellik ve kısıtlamaları aşağıdaki makalelerde bulunabilir:

* [Birden çok NIC içeren bir Windows VM oluşturma][virtual-networks-multiple-nics-windows]
* [Birden çok NIC içeren bir Linux VM oluşturma][virtual-networks-multiple-nics-linux]
* [Birden çok NIC bir şablon kullanarak VM dağıtma][virtual-network-deploy-multinic-arm-template]
* [Birden çok NIC PowerShell kullanarak Vm'leri dağıtma][virtual-network-deploy-multinic-arm-ps]
* [Birden çok NIC Azure CLI kullanarak Vm'leri dağıtma][virtual-network-deploy-multinic-arm-cli]

#### <a name="site-to-site-connectivity"></a>Konumdan Konuma Bağlantı

Azure Vm'leri ve bağlı saydam ve kalıcı bir VPN bağlantısıyla şirket içi ve dışı karışık olur. Azure'da en yaygın SAP dağıtım desen haline beklenir. İşlem yordamlarını ve azure'da SAP örnekleri ile işlemlerini şeffaf bir şekilde çalışmalıdır varsayılır. Şirket içi SAP aktarım yönetim sistemi (TMS) aktarım için azure'da bir geliştirme sistemi olan bir test sisteminizde yapılan değişikliklerin kullanım yanı sıra bu dışında bu sistemler yazdırma olmalıdır anlamına gelir dağıtıldı. Daha fazla belge siteden siteye etrafında bulunabilir [bu makalede][vpn-gateway-create-site-to-site-rm-powershell]

##### <a name="vpn-tunnel-device"></a>VPN tüneli cihaz

Siteden siteye bağlantı (Azure veri merkezi için şirket içi veri merkezi) oluşturmak için almak ve bir VPN cihazı yapılandırma ya da Yönlendirme ve Uzaktan Erişim hizmeti (sunulan RRAS), bir yazılım bileşeni Windows Server 2012 ile kullanmak gerekir.

* [PowerShell kullanarak siteden siteye VPN bağlantısı ile sanal ağ oluşturma][vpn-gateway-create-site-to-site-rm-powershell]
* [Siteden siteye VPN Gateway bağlantıları için VPN cihazları hakkında][vpn-gateway-about-vpn-devices]
* [VPN Gateway hakkında SSS][vpn-gateway-vpn-faq]

![Şirket içi ve Azure arasında siteden siteye bağlantı][planning-guide-figure-600]

Yukarıdaki şekilde, IP adresi alt aralıklara kullanım için ayrılmış iki Azure abonelikleri azure'daki sanal ağlara sahip gösterir. VPN şirket içi ağdan azure'a bağlantısı kurulur.

#### <a name="point-to-site-vpn"></a>Noktadan siteye VPN

Noktadan siteye VPN ile kendi VPN Azure'a bağlanmak için her bir istemci makine gerekir. SAP senaryoları için arıyoruz, noktadan siteye bağlantı pratik değildir. Bu nedenle, daha fazla başvuru noktadan siteye VPN bağlantısı verilir.

Daha fazla bilgi burada bulunabilir
* [Azure portal’ı kullanarak bir sanal ağa yönelik Noktadan Siteye bağlantı yapılandırma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)
* [PowerShell'i kullanarak bir sanal ağa yönelik bir Noktadan Siteye bağlantı yapılandırma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)

#### <a name="multi-site-vpn"></a>Çok siteli VPN

Azure, günümüzde ayrıca bir Azure aboneliği için çok siteli VPN bağlantısı oluşturma olanağı sunar. Tek bir abonelik daha önce bir siteden siteye VPN bağlantısı sınırlı. Bu sınırlama, tek bir abonelik için çok siteli VPN bağlantıları ile yerine geçti. Bu şirketler arası yapılandırmalar aracılığıyla belirli bir aboneliği için birden fazla Azure bölgesi yararlanmasını mümkün kılar.

Daha fazla bilgi için bkz [bu makalede][vpn-gateway-create-site-to-site-rm-powershell]

#### <a name="vnet-to-vnet-connection"></a>Vnet'ten Vnet'e bağlantı

Çok siteli VPN kullanarak, her bölge içinde ayrı bir Azure sanal ağ yapılandırmanız gerekir. Ancak genellikle farklı bölgelerde yazılım bileşenlerinin birbirleriyle iletişim gereksinimi vardır. İdeal olarak bu iletişimi şirket içi bir Azure bölgesinden ve bir Azure bölgesine buradan yönlendirileceğini değil. Kısayol, olası bir bölgede başka bir Azure sanal ağ için bir Azure sanal ağ arasında bağlantı yapılandırmak için başka bir bölgede barındırılan Azure sunar. Bu işlev, VNet-VNet bağlantı olarak adlandırılır. Bu işlev hakkında daha fazla bilgi şurada bulunabilir: <https://azure.microsoft.com/documentation/articles/vpn-gateway-vnet-vnet-rm-ps/>.

#### <a name="private-connection-to-azure-expressroute"></a>Azure ExpressRoute özel bağlantı

Microsoft Azure ExpressRoute bir birlikte bulundurma ortamında bulunan veya Azure veri merkezleri ve müşterinin şirket içi altyapı arasında özel bağlantılar oluşturulmasına izin verir. ExpressRoute, çeşitli MPLS tarafından sunulur (paket anahtarlamalı) VPN sağlayıcıları veya diğer ağ hizmeti sağlayıcıları. ExpressRoute bağlantıları ortak İnternet üzerinden geçmemektedir. ExpressRoute bağlantısı Internet üzerinden daha yüksek güvenlik, birden çok paralel bağlantı hatları, daha yüksek hızlar ve daha düşük gecikme süreleri tipik aracılığıyla daha fazla güvenilirlik sunar.

Azure ExpressRoute ve burada teklifleri hakkında daha fazla ayrıntı bulabilirsiniz:

* <https://azure.microsoft.com/documentation/services/expressroute/>
* <https://azure.microsoft.com/pricing/details/expressroute/>
* <https://azure.microsoft.com/documentation/articles/expressroute-faqs/>

Express Route burada açıklandığı gibi bir ExpressRoute bağlantı hattı üzerinden birden çok Azure abonelikleri etkinleştirir

* <https://azure.microsoft.com/documentation/articles/expressroute-howto-linkvnet-arm/>
* <https://azure.microsoft.com/documentation/articles/expressroute-howto-circuit-arm/>

#### <a name="forced-tunneling-in-case-of-cross-premises"></a>Zorlamalı tünel durumunda şirket içi
Siteden siteye, noktadan siteye veya ExpressRoute aracılığıyla şirket içi etki alanlarına katılma VM'ler için de bu vm'lerdeki tüm kullanıcılar için Internet proxy ayarları dağıtılmasını emin olmanız gerekir. Varsayılan olarak, bu Vm'leri veya kullanıcıların İnternet'e erişmek için bir tarayıcı kullanarak çalışan yazılım şirket proxy üzerinden Git değil, ancak Azure internet üzerinden doğrudan bağlanabilir. Ancak bile proxy ayarı, yazılım ve hizmetlerinin proxy için denetlenecek sorumluluk olduğundan şirket proxy'si üzerinden trafiği yönlendirmek için % 100 çözüm değildir. VM'de çalışan yazılım değil yaptığını, ya da yönetici ayarları yönetir, trafiği İnternet'e yeniden detoured doğrudan Azure internet üzerinden.

Bir tür bir doğrudan internet bağlantısı önlemek için zorlamalı tünel şirket içi ve Azure arasında siteden siteye bağlantı ile yapılandırabilirsiniz. Zorlamalı tünel özellik ayrıntılı açıklamasını buraya yayımlanır <https://azure.microsoft.com/documentation/articles/vpn-gateway-forced-tunneling-rm/>

ExpressRoute ile zorlamalı tünel aracılığıyla ExpressRoute BGP eşliği oturumlarını bir varsayılan yolun tanıtılması müşteriler tarafından etkinleştirilir.

#### <a name="summary-of-azure-networking"></a>Azure ağ özeti

Bu bölümde, Azure ağ ilgili birçok önemli noktaları içeriyordu. Ana noktaları bir özeti aşağıda verilmiştir:

* Azure sanal ağları, ağ yapısının Azure dağıtımınızda yerleştirilmesine olanak sağlar. Sanal ağlar birbirlerine göre yalıtılmış olabilir veya sanal ağlar arasındaki trafik ağ güvenlik grupları'nın yardımıyla denetlenebilir.
* Azure sanal ağları, IP adresi aralıklarını Vm'lere atamak ya da VM'ler için sabit IP adresleri atamak üzere yararlanılabilir
* Siteden siteye veya noktadan siteye bir bağlantı kurmak için öncelikle bir Azure sanal ağ oluşturmanız gerekir
* Bir sanal makine dağıtıldıktan sonra artık VM'ye atanan sanal ağ değiştirmek mümkün değildir

### <a name="quotas-in-azure-virtual-machine-services"></a>Azure sanal makine Hizmetleri kotaları
Depolama ve ağ altyapısını çeşitli hizmetler Azure altyapısında çalışan sanal makineler arasında paylaşılan, olgu hakkında açık olması gerekir. Ve yalnızca müşterinin kendi veri merkezlerini olduğu gibi bazı altyapı kaynakları fazladan sağlama bir dereceye gerçekleşir. Microsoft Azure platformu, kaynak tüketimini sınırlamak için ve tutarlı ve belirleyici performansını korumak için disk, CPU, ağ ve diğer kotalarını kullanır.  Farklı VM türleri (A5, A6 vb.) için CPU, RAM, disk sayısını farklı kotalar ve ağ.

> [!NOTE]
> SAP tarafından desteklenen VM türlerinin CPU ve bellek kaynakları ana düğümler üzerinde önceden ayrılmış. Başka bir deyişle, VM dağıtıldıktan sonra konak üzerindeki kaynaklara VM türü tarafından tanımlandığı şekilde kullanılabilir.
>
>

Planlama ve SAP Azure çözümlerine boyutlandırma, her sanal makine boyutu için kotalar dikkate alınmalıdır. VM kotalar açıklanan [burada (Linux)] [ virtual-machines-sizes-linux] ve [burada (Windows)][virtual-machines-sizes-windows].

Açıklanan kota teorik en yüksek değerleri temsil eder.  Disk başına IOPS sınırını küçük IOs (8 kb) elde edilebilir, ancak muhtemelen büyük IOs (1 Mb) elde edilebilir değil.  IOPS sınırı tek disk ayrıntı düzeyi üzerinde uygulanır.

Bir kaba karar ağacı bir SAP sistemiyle Azure sanal makine hizmetlerini ve özelliklerini veya mevcut bir sistemi olup sistem azure'da dağıtmak için farklı şekilde yapılandırılması gerekir uygun olup olmadığını karar vermek için aşağıdaki karar ağacı kullanılabilir:

![Azure'da SAP dağıtabilme karar vermek için karar ağacı][planning-guide-figure-700]

**1. adım**: En önemli bilgiler ile başlamak için belirli bir SAP sistemine SAP gereksinimidir. SAP sistemine zaten şirket içinde dağıtılması 2 katmanlı yapılandırmasında olsa bile, SAP gereksinimleri DBMS ve SAP uygulama bölümlerini ayrılması gerekir. Mevcut sistemler için genellikle donanım kullanımıyla ilgili SAP belirlenen veya mevcut SAP ölçümlerinde göre tahmini. Sonuçları şurada bulunabilir: <https://sap.com/about/benchmark.html>.
Yeni dağıtılmış SAP sistemlerini için SAP gereklilikleri sisteminin ve boyutlandırma alıştırma çalıştınız.
Ayrıca bu blog ve SAP boyutlandırma için ekli belge, Azure üzerinde bakın: <https://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

**2. adım**: Mevcut sistemler için g/ç hacmi ve saniye başına g/ç işlemleri DBMS sunucuda ölçülen. Yeni planlı sistemler için yeni sistemi için boyutlandırma alıştırma ayrıca DBMS tarafında g/ç gereksinimlerinin kaba fikirleri vermeniz gerekir. Emin değilseniz, sonunda bir kavram kanıtı yürütmek gerekir.

**3. adım**: Azure farklı VM türleri sağlayabilir SAP DBMS sunucusuyla SAP gereksinimini karşılaştırın. SAP bilgileri farklı Azure VM türleri SAP Not belgelenen [1928533]. Veritabanı katmanı, katman dağıtımların çoğunluğu içinde kullanıma ölçeklenmez bir SAP NetWeaver sistemi olduğundan odağı DBMS VM'de önce olmalıdır. Buna karşılık, SAP uygulama katmanı dışa genişletilebilir. SAP hiçbiri destekleniyorsa, Azure VM türleri gerekli SAP sunabilir, planlanan SAP sistemine iş yükünü Azure üzerinde çalıştırılamaz. Ya da sistem dağıtmak için ihtiyacınız şirket içi veya sistem için iş yükü birimi değiştirmeniz gerekir.

**4. adım**: Belirtildiği gibi [burada (Linux)] [ virtual-machines-sizes-linux] ve [burada (Windows)][virtual-machines-sizes-windows], Azure standart kullanmanızdan bağımsız disk başına bir IOPS kotası zorunlu kılar Depolama veya Premium depolama. VM türü bağımlı bağlanabilir, veri diski sayısı değişir. Sonuç olarak, her biri farklı VM türleri ile elde edilebilecek en fazla IOPS birkaç hesaplayabilirsiniz. Veritabanı dosya yerleşimi bağlı, bir konuk işletim sistemi biriminde olacak diskleri stripe. Bununla birlikte, SAP sistemine iş yükü ciddi bir dağıtılmış SAP sistemini geçerli IOPS hacmi Azure'dan ve daha fazla belleğe sahip dengelemek için bir şans varsa en büyük VM türünün hesaplanan limitlerini aşarsam etkilenebilir. Bu gibi durumlarda, burada Azure sistemde dağıtmamanız bir noktası ulaşmasını.

**5. adım**: SAP sistemlerini, özellikle de olan şirket içi 2 katmanlı yapılandırmalarda dağıtılan, büyük olasılıkla sistem Azure'da 3 katmanlı bir yapılandırmada yapılandırılmış olması gerekebilir. Bu adımda, olup olmadığını bir bileşeni olan farklı Azure VM türleri teklif CPU ve bellek kaynakları yerleştirin değil ve hangi ölçeği genişletilemez SAP uygulama katmanında gerekir. Gerçekten varsa bu tür bir bileşeni, SAP sistemine ve iş yükünü Azure'a dağıtılamıyor. Ancak sistemin birden fazla Azure sanal makinelerde oturum SAP uygulama bileşenlerini ölçeklendirilebilir, Azure'a dağıtılabilir.

**6. adım**: DBMS ve SAP uygulama katmanı bileşenlerini Azure Vm'lerde çalıştırılabilir, yapılandırma ile tanımlanması gerekir:

* Azure VM sayısı
* Ayrı ayrı bileşenler için VM türleri
* DBMS sağlamak yeterli IOPS için VM içindeki VHD sayısı

## <a name="managing-azure-assets"></a>Azure varlıklarını yönetme

### <a name="azure-portal"></a>Azure portal

Azure portalında Azure VM dağıtımları yönetmek için üç arabirimi biridir. Azure portalı üzerinden görüntülerden VM dağıtma gibi temel yönetim görevlerini gerçekleştirebilirsiniz. Ayrıca, depolama hesapları, sanal ağlar ve diğer Azure bileşenlerini oluşturulmasını da Azure portalında iyi işleyebilir görevler verilmiştir. Ancak, VHD'leri şirket içinden Azure'a yükleme veya azure'daki bir VHD kopyalama gibi işlevler, üçüncü taraf Araçlar'ı veya PowerShell veya CLI aracılığıyla yönetim gerektiren görevler uygulanır.

![Microsoft Azure portalı - sanal makineye genel bakış][planning-guide-figure-800]


Sanal makine örneği için yönetim ve yapılandırma görevini Azure portalının içinde mümkündür.

Yeniden başlatmak ve bir sanal makine kapatılıyor yanı sıra, ayrıca ekleme, ayırma ve veri diskleri görüntüsü hazırlama, örneğin yakalamak için sanal makine örneği için oluşturma ve sanal makine örneği boyutunu yapılandırın.

Azure portalı, dağıtmak ve sanal makineleri ve diğer birçok Azure hizmetinde yapılandırmak için temel işlevleri sağlar. Ancak değil tüm kullanılabilir İşlevler, Azure portal tarafından ele alınmıştır. Azure portalında gibi görevleri gerçekleştirmek mümkün değildir:

* Azure'a VHD yükleme
* Sanal makineleri kopyalama


### <a name="management-via-microsoft-azure-powershell-cmdlets"></a>Microsoft Azure PowerShell cmdlet'leri aracılığıyla yönetim

Windows PowerShell, yaygın olarak daha fazla sayıda azure'da sistemleri dağıtan müşteriler tarafından benimsenmiştir güçlü ve Genişletilebilir bir çerçevedir. Masaüstü, dizüstü bilgisayar veya özel yönetim istasyon PowerShell cmdlet'leri yüklendikten sonra PowerShell cmdlet'leri uzaktan çalıştırılabilir.

Yerel bir masaüstü/dizüstü kullanımı Azure PowerShell cmdlet'leri ve açıklanan Azure abonelikleri ile kullanım için bu yapılandırma için etkinleştirme işlemini [bu makalede][powershell-install-configure].

Yükleme, güncelleştirme ve Azure PowerShell cmdlet'lerini de bulunabilir yapılandırma adımları ayrıntılı [Bu bölümde Dağıtım Kılavuzu'nun][deployment-guide-4.1].

Müşteri Deneyimini şimdiye PowerShell (PS) kesinlikle Vm'leri dağıtmak ve özel adımlar VM dağıtımı oluşturma için daha güçlü araç olduğunu olmuştur. Azure'da SAP örnekleri çalışan tüm müşteriler, Azure portalından gerçekleştirin veya bile özel olarak azure'da dağıtımlarını yönetmek için PS cmdlet'leri kullanarak yönetim görevleri tamamlamak için PS cmdlet'leri kullanıyor. 2000'den fazla Windows ile ilgili cmdlet'leri ile aynı adlandırma kuralı Azure özgü cmdlet'lerin paylaşmak olduğundan, bu cmdlet'leri yararlanmak Windows yöneticileri için kolay bir görevi var.

Burada örneğe bakın: <https://blogs.technet.com/b/keithmayer/archive/2015/07/07/18-steps-for-end-to-end-iaas-provisioning-in-the-cloud-with-azure-resource-manager-arm-powershell-and-desired-state-configuration-dsc.aspx>


SAP için Azure izleme uzantısı dağıtım (bölüm bakın [SAP için Azure izleme çözümü] [ planning-guide-9.1] bu belgedeki) PowerShell veya CLI aracılığıyla yalnızca mümkündür. Bu nedenle ayarlama ve PowerShell veya CLI dağıtma veya azure'da bir SAP NetWeaver sistemini yönetme, yapılandırma için zorunludur.  

Azure, daha fazla işlevsellik sağlar. gibi yeni PS cmdlet'leri cmdlet'lerinin bir güncelleştirme gerektiren eklenmesi oluşturacaksınız. Bu nedenle en az bir kez ay Azure indirme sitesi denetlemek için mantıklıdır <https://azure.microsoft.com/downloads/> cmdlet'leri yeni bir sürümü için. Yeni sürümün eski sürümün üzerine yüklenir.

PowerShell Azure ile ilgili genel bir listesi için komutları buraya bakın: <https://docs.microsoft.com/powershell/azure/overview>.

### <a name="management-via-microsoft-azure-cli-commands"></a>Microsoft Azure CLI komutları aracılığıyla yönetim

Linux kullanın ve Azure yönetmek isteyen müşteriler için bir seçenek kaynakları Powershell olmayabilir. Alternatif olarak, Microsoft, Azure CLI sunar.
Azure CLI, Azure platformu ile birlikte çalışmaya yönelik platformlar arası komut bir dizi açık kaynaklı sağlar. Azure CLI Azure Portalı'nda bulunan aynı işlevselliğinin sağlar.

Kurulum, yapılandırma ve CLI kullanma hakkında bilgi için bkz: Azure görevleri gerçekleştirmek için komutları

* [Klasik Azure CLI yükleme][xplat-cli]
* [Dağıtmak ve Azure Resource Manager şablonları ve Azure CLI kullanarak sanal makineleri yönetme] [../../linux/create-ssh-secured-vm-from-template.md]
* [Mac, Linux ve Windows Azure Resource Manager ile klasik Azure CLI kullanma][xplat-cli-azure-resource-manager]

Bölüm de okuma [Linux VM'ler için Azure CLI] [ deployment-guide-4.5.2] içinde [Dağıtım Kılavuzu] [ planning-guide] Azure izleme dağıtmak için Azure CLI kullanma hakkında SAP için uzantısı.

## <a name="different-ways-to-deploy-vms-for-sap-in-azure"></a>VM'ler için azure'da SAP dağıtmanın farklı yöntemleri

Bu bölümde, azure'da VM dağıtmanın farklı yöntemleri öğrenin. Bu bölümde, VHD'ler ve azure'da sanal makineler yanı sıra ek hazırlık yordamları ele alınmaktadır.

### <a name="deployment-of-vms-for-sap"></a>SAP için VM dağıtımı

Microsoft Azure Vm'leri ve ilişkili diskler dağıtmak için birden çok yol sunar. Bu nedenle hazırlıkları VM'lerin dağıtım yöntemine bağlı olarak değişebilir olduğundan farkları anlamak önemlidir. Genel olarak, aşağıdaki senaryoları göz atın:

#### <a name="4d175f1b-7353-4137-9d2f-817683c26e53"></a>Bir sanal makine şirket içinden Azure'a genelleştirilmiş olmayan bir diskle taşıma

Belirli bir SAP sistemine şirket içinden Azure'a taşımayı planlayın. Bu, veri ve günlük dosyalarını azure'a DBMS ile işletim sistemi, SAP ikili dosyaları ve DBMS ikili dosyaları içeren VHD artı VHD'lerin yükleyerek yapılabilir. Tersine [senaryo aşağıdaki #2][planning-guide-5.1.2], konak adı, SAP SID tutun ve şirket içi ortamda yapılandırıldıkları gibi Azure VM'de SAP kullanıcı hesapları. Bu nedenle, görüntüyü Genelleştirme gerekli değildir. Bölümlere bakın [VM şirket içinden Azure'a genelleştirilmiş olmayan bir disk ile geçiş hazırlığı] [ planning-guide-5.2.1] şirket içi hazırlık adımları ve genelleştirilmiş olmayan Vm'leri ya da azure'a VHD yükleme için bu belgenin. İlgili bölüm [Senaryo 3: VM genelleştirilmiş olmayan bir Azure VHD'nin ile SAP kullanarak şirket içi taşıma] [ deployment-guide-3.4] içinde [Dağıtım Kılavuzu] [ deployment-guide] dağıtma gibi bir görüntüde ilgili ayrıntılı adımlar Azure.

#### <a name="e18f7839-c0e2-4385-b1e6-4538453a285c"></a>Bir müşteriye özel görüntü ile bir VM dağıtma

İşletim sistemi veya DBMS sürümünüzün belirli gereksinimleri nedeniyle, sağlanan görüntüleri Azure Market'te gereksinimlerinize uygun olmayabilir. Bu nedenle, daha sonra birkaç kez dağıtılabilir kendi özel işletim sistemi/DBMS VM görüntüsünü kullanarak bir VM oluşturmanız gerekebilir. Çoğaltma için özel bir görüntüsünü hazırlamak için aşağıdaki öğelere sahip olarak kabul edilmesi:

- - -
> ![Windows][Logo_Windows] Windows
>
> Daha fazla ayrıntılı bilgiyi burada bulabilirsiniz: <https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed> Windows ayarları (örneğin, Windows SID ve ana bilgisayar adı) soyutlanır/genelleştirilmiş sysprep komutu aracılığıyla şirket içi VM üzerinde olmalıdır.
>
>
> ![Linux][Logo_Linux] Linux
>
> Bu makaleler için açıklanan adımları [SUSE][virtual-machines-linux-create-upload-vhd-suse], [Red Hat][virtual-machines-linux-redhat-create-upload-vhd], veya [Oracle Linux] [ virtual-machines-linux-create-upload-vhd-oracle]Azure'a karşıya yüklenecek VHD'yi hazırlama için.
>
>

- - -
(Özellikle de 2 katmanlı sistemleri için), şirket içi VM'de SAP içerik zaten yüklediyseniz, örnek üzerinden Azure VM dağıtımını yeniden adlandır sonra yordamı SAP yazılım sağlama Yöneticisi (SAP tarafından desteklenen SAP sistem ayarları uyarlayabilirsiniz Not [1619720]). Bölümlere bakın [SAP için bir müşteriye özel görüntü ile bir VM dağıtımı için hazırlık] [ planning-guide-5.2.2] ve [şirket içinden azure'a VHD yükleme] [ planning-guide-5.3.2]şirket içi hazırlık adımları ve azure'da genelleştirilmiş bir VM'yi karşıya yüklenmesi için bu belgenin. İlgili bölüm [Senaryo 2: SAP için özel bir görüntü ile bir VM dağıtma] [ deployment-guide-3.3] içinde [Dağıtım Kılavuzu] [ deployment-guide] ayrıntılı adımlar, bu tür bir görüntüyü azure'da dağıtma.

#### <a name="deploying-a-vm-out-of-the-azure-marketplace"></a>Azure Marketi dışında bir VM dağıtma

Bir Microsoft kullanmak ister misiniz veya VM görüntüsü, VM dağıtmak için Azure Market'teki üçüncü taraf sağlanan. Sanal makinenizin azure'da dağıtılan sonra aynı yönergeler ve bir şirket içi ortamda yaptığınız gibi SAP yazılımı ve/veya sanal makinenizin içinde DBMS yüklemek için Araçlar izleyin. Daha ayrıntılı dağıtım açıklaması için bkz. bölüm [Senaryo 1: SAP için Azure Marketi dışında bir VM dağıtma] [ deployment-guide-3.2] içinde [Dağıtım Kılavuzu][deployment-guide].

### <a name="6ffb9f41-a292-40bf-9e70-8204448559e7"></a>Azure için SAP ile Vm'leri hazırlama

Vm'leri Azure'a yüklemeden önce sanal makineleri ve VHD belirli gereksinimleri karşılayan emin olmanız gerekir. Kullanılan dağıtım yöntemine bağlı olarak küçük farklılıklar vardır.

#### <a name="1b287330-944b-495d-9ea7-94b83aff73ef"></a>Bir VM şirket içinden Azure'a genelleştirilmiş olmayan bir disk ile geçiş için hazırlama

Genel dağıtım yöntemi, bir SAP sistemiyle şirket içinden Azure'a çalıştıran varolan bir VM'yi taşımaktır. Bu VM ve VM SAP sistemde, aynı ana bilgisayar adı ve büyük olasılıkla aynı SAP SID kullanarak Azure'da yalnızca çalıştırmalısınız. Bu durumda, konuk işletim sistemi, VM için birden çok dağıtım genelleştirilerek değil. Şirket içi ağ Azure'a uzatıldıysa (bölüm bakın [şirket içi - dağıtımı tek veya birden çok SAP VM şirket içi ağa tam olarak tümleştirilmiş gereksiniminin azure'a] [ planning-guide-2.2] bu belgedeki), sonra da aynı etki alanı hesapları bu şirket içi önce kullanılan VM içinden kullanılabilir.

Kendi Azure VM Disk hazırlanırken gereksinimleri şunlardır:

* İlk olarak işletim sistemini içeren VHD yalnızca bir en büyük boyutu 127 GB olabilir. Bu sınırlama, Mart 2015 sonunda ortadan. Artık başka bir Azure depolama VHD de barındırılan olarak işletim sistemini içeren VHD boyutu 1 TB'ye kadar olabilir.
* Sabit bir VHD biçiminde olması gerekir. Dinamik VHD veya VHDx biçiminde VHD'lerin Azure'da henüz desteklenmiyor. Dinamik VHD PowerShell commandlet'lerini VHD'yi veya CLI karşıya yüklediğinizde statik Vhd'lere dönüştürülür.
* Bir sabit VHD biçiminde de VM gerek Azure'da yeniden bağlanmalıdır ve sanal Makineye bağlı VHD. Okuma [bu makalede (Linux)](../../linux/managed-disks-overview.md) ve [bu makalede (Windows)](../../windows/managed-disks-overview.md)) için veri disklerinin boyutu sınırları. Dinamik VHD PowerShell commandlet'lerini VHD'yi veya CLI karşıya yüklediğinizde statik Vhd'lere dönüştürülür.
* Microsoft desteği veya hangi hizmetlerin ve uygulamaların VM dağıtılana kadar çalıştırmak için bağlam olarak atanabilir ve daha uygun kullanıcı tarafından kullanılan yönetici ayrıcalıklarıyla başka bir yerel hesabı kullanılabilir ekleyin.
* Bu belirli bir dağıtım senaryosu için gerekebilecek diğer yerel hesapları ekleyin.

- - -
> ![Windows][Logo_Windows] Windows
>
> Bu senaryoda, karşıya yükleme ve Azure üzerinde sanal makine dağıtmak için hiçbir VM genelleştirilmiş (sysprep) gereklidir.
> D:\ kullanılmıyorsa bu sürücüye emin olun.
> Bölümde açıklandığı gibi kullanıma açılan diskler için disk otomatik bağlama kümesi [bağlı diskleri için otomatik bağlama ayarlama] [ planning-guide-5.5.3] bu belgedeki.
>
> ![Linux][Logo_Linux] Linux
>
> Bu senaryoda hiçbir Genelleştirme (waagent-sağlamayı kaldırma) karşıya yükleyin ve azure'da VM dağıtmak için sanal makinenin gereklidir.
> / Mnt/kaynak kullanılmaz ve tüm diskleri UUID bağlı olduğundan emin olun. İşletim sistemi diski için şifresizdir girişi ayrıca UUID tabanlı bağlama gösterdiğinden emin olun.
>
>

- - -
#### <a name="57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3"></a>SAP için bir müşteriye özel görüntü ile bir VM dağıtımı için hazırlama

Genelleştirilmiş bir işletim sistemi içeren VHD dosyaları, kapsayıcılar, Azure depolama hesapları veya yönetilen Disk görüntüleri olarak depolanır. VHD veya yönetilen Disk görüntüsü bir dağıtım şablonu dosyalarınızı kaynağı olarak bölümde açıklandığı gibi başvurarak bu tür bir görüntüden yeni bir VM dağıtabilirsiniz [Senaryo 2: SAP için özel bir görüntü ile bir VM dağıtma] [ deployment-guide-3.3] , [Dağıtım Kılavuzu][deployment-guide].

Kendi Azure VM görüntünüzü hazırlarken gereksinimleri şunlardır:

* İlk olarak işletim sistemini içeren VHD yalnızca bir en büyük boyutu 127 GB olabilir. Bu sınırlama, Mart 2015 sonunda ortadan. Artık başka bir Azure depolama VHD de barındırılan olarak işletim sistemini içeren VHD boyutu 1 TB'ye kadar olabilir.
* Sabit bir VHD biçiminde olması gerekir. Dinamik VHD veya VHDx biçiminde VHD'lerin Azure'da henüz desteklenmiyor. Dinamik VHD PowerShell commandlet'lerini VHD'yi veya CLI karşıya yüklediğinizde statik Vhd'lere dönüştürülür.
* Bir sabit VHD biçiminde de VM gerek Azure'da yeniden bağlanmalıdır ve sanal Makineye bağlı VHD. Lütfen okuma [bu makalede (Linux)](../../windows/managed-disks-overview.md) ve [bu makalede (Windows)](../../linux/managed-disks-overview.md) veri disklerinin boyutu sınırları için. Dinamik VHD PowerShell commandlet'lerini VHD'yi veya CLI karşıya yüklediğinizde statik Vhd'lere dönüştürülür.
* Bu belirli bir dağıtım senaryosu için gerekebilecek diğer yerel hesapları ekleyin.
* Görüntü yüklemesini SAP NetWeaver ve ana bilgisayar adının Azure dağıtım noktasında orijinal adı yeniden adlandırma içeriyorsa, SAP yazılım sağlama Manager DVD en son sürümlerine şablonuna eklenecek önerilir, olasıdır. Bu, kolayca uyum değiştirilen konak adı ve/veya yeni bir kopyasını başlatıldıktan hemen sonra dağıtılan sanal makine görüntüsünü SAP sisteminde SID'si değiştirmek için sağlanan SAP yeniden adlandırma işlevselliği kullanmanıza olanak sağlayacaktır.

- - -
> ![Windows][Logo_Windows] Windows
>
> Sürücü D:\ değil bölümde açıklandığı gibi için kullanıma açılan diskler kümesi disk otomatik bağlama kullanılan emin [bağlı diskleri için otomatik bağlama ayarlama] [ planning-guide-5.5.3] bu belgedeki.
>
> ![Linux][Logo_Linux] Linux
>
> / Mnt/kaynak kullanılmaz ve tüm diskleri UUID bağlı olduğundan emin olun. İşletim sistemi diski için şifresizdir girişi ayrıca UUID tabanlı bağlama gösterdiğinden emin olun.
>
>

- - -
* SAP GUI (için yönetim ve Kurulum amacıyla) bu tür bir şablonda önceden yüklenebilir.
* VM yeniden adlandırma ile bu yazılım çalışabilir sürece şirketler arası senaryolarda Vm'leri başarılı bir şekilde çalıştırılması için gerekli olan diğer yazılımlar yüklenebilir.

VM yeterince genel ve sonunda bağımsız hesapları/kullanıcı hedeflenen Azure dağıtım senaryosunda kullanılabilir olması için hazırlık yaptıysanız, bu tür bir görüntüyü Genelleştirme son hazırlık adımı yürütülür.

##### <a name="generalizing-a-vm"></a>VM'yi Genelleştirme
- - -
> ![Windows][Logo_Windows] Windows
>
> Bir VM'ye bir yönetici hesabıyla oturum açmak için son adımdır bakın. Olarak bir Windows komut penceresi açın *yönetici*. %Windir%\Windows\System32\sysprep için Git ve sysprep.exe yürütün.
> Küçük bir pencere görüntülenir. Gözden geçirmeniz önemlidir **Generalize** seçeneği (varsayılan olarak işaretli değildir) ve Değiştir 'Kapatma' için 'Yeniden başlatma' varsayılan kapatma seçeneği. Bu yordam, uygulamanın sysprep işlemiyle çalıştırılan şirket içi sanal makinenin konuk işletim sisteminde olduğu varsayılır.
> Zaten Azure'da çalışan bir VM ile yordamı uygulamak istiyorsanız, açıklanan adımları izleyin [bu makalede](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource).
>
> ![Linux][Logo_Linux] Linux
>
> [Resource Manager şablonu olarak kullanmak üzere Linux sanal makinesi yakalama][capture-image-linux-step-2-create-vm-image]
>
>

- - -
### <a name="transferring-vms-and-vhds-between-on-premises-to-azure"></a>Şirket içi arasında Azure sanal makineleri ve VHD aktarma
VM görüntüleri ve diskleri Azure'a yükleme Azure portalı üzerinden mümkün olmadığından, Azure PowerShell cmdlet'lerini veya CLI kullanmanız gerekir. ' % S'araç 'AzCopy' kullanımını başka bir olasılıktır. Aracı VHD'ler (her iki yönde) şirket içi ile Azure arasında kopyalayabilirsiniz. Ayrıca, Azure bölgeleri arasında VHD'lerin de kopyalayabilirsiniz. Lütfen [bu belgeleri] [ storage-use-azcopy] indirme ve AzCopy kullanımı için.

GUI tabanlı çeşitli üçüncü taraf araçlarını kullanmak için üçüncü bir seçenek olacaktır. Ancak, bu araçları Azure sayfa Blobları destekleyen emin olun. Azure sayfa blobu deposunu kullanmak üzere ihtiyacımız amaçlarımız doğrultusunda (farklar aşağıda açıklanmıştır: <https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>). Azure tarafından sağlanan araçları da VM'ler ve yüklenmesi gerekir VHD'ler, sıkıştırma verimlidir. Bu önemlidir, çünkü bu sıkıştırma verimliliği (yine de bağlı olarak yükleme bağlantısı, şirket içi tesis ve hedeflenen Azure Dağıtım bölgesi internet'e değişir) karşıya yükleme süresini azaltır. Bu, bir VM veya Avrupa konumu bir VHD'den merkezleri Avrupa Azure Vm'leri/VHD'lerin aynısını karşıya daha uzun sürer ABD tabanlı Azure veri için karşıya veri merkezleri adil bir varsayılır.

#### <a name="a43e40e6-1acc-4633-9816-8f095d5a7b6a"></a>Şirket içinden azure'a VHD yükleme
Şirket içi ağdan bir var olan VM veya VHD'yi karşıya yüklemek için böyle bir VM veya VHD bölümde listelenen gereksinimlerini karşılamak gereken [VM şirket içinden Azure'a genelleştirilmiş olmayan bir disk ile geçiş hazırlığı] [ planning-guide-5.2.1]bu belgenin.

Böyle bir VM genelleştirilmiş gerekmez ve durum ve şirket içi tarafında kapatma sonrasında olan şekil karşıya yüklenebilir. Aynı herhangi bir işletim sistemini içermeyen ek VHD'ler için geçerlidir.

##### <a name="uploading-a-vhd-and-making-it-an-azure-disk"></a>Bir VHD'yi karşıya yüklemek ve bir Azure Disk yapma
Bu durumda veya bilginiz olmaksızın bir işletim sisteminde, bir VHD'yi karşıya yükleme ve bir VM'ye veri diski olarak takmak veya işletim sistemi diski olarak kullanmak istiyoruz. Çok adımlı bir işlemdir

**PowerShell**

* Aboneliğinizde oturum açın *AzAccount Bağlan*
* Bağlamınızın ile aboneliği ayarlamak *kümesi AzContext* ve Subscriptionıd parametresi veya - SubscriptionName bakın <https://docs.microsoft.com/powershell/module/az.accounts/set-Azcontext>
* VHD'yi karşıya yükleme *Ekle AzVhd* bir Azure depolama hesabı - bkz. <https://docs.microsoft.com/powershell/module/az.compute/add-Azvhd>
* (İsteğe bağlı) İle bir VHD'den yönetilen Disk oluşturma *yeni AzDisk* -bakın <https://docs.microsoft.com/powershell/module/az.compute/new-Azdisk>
* VHD veya yönetilen Disk ile yeni bir VM yapılandırması, işletim sistemi diski olarak *kümesi AzVMOSDisk* -bakın <https://docs.microsoft.com/powershell/module/az.compute/set-Azvmosdisk>
* VM yapılandırması ile yeni VM oluşturma *New-AzVM* -bakın <https://docs.microsoft.com/powershell/module/az.compute/new-Azvm>
* İle yeni bir VM'ye veri diski ekleme *Ekle AzVMDataDisk* -bakın <https://docs.microsoft.com/powershell/module/az.compute/add-Azvmdatadisk>

**Azure CLI**

* Aboneliğinizde oturum açın *az oturum açma*
* Aboneliğinizi seçin *az hesabı set--abonelik `<subscription name or id`>*
* VHD'yi karşıya yükleme *az storage blob upload* -bkz [Azure depolama ile Azure CLI kullanma][storage-azure-cli]
* (İsteğe bağlı) İle bir VHD'den yönetilen Disk oluşturma *az disk oluşturma* -bakın https://docs.microsoft.com/cli/azure/disk
* Karşıya yüklenen VHD veya yönetilen Disk ile işletim sistemi diski olarak belirterek yeni bir VM oluşturma *az vm oluşturma* ve parametre *--ekleme-işletim sistemi diski*
* İle yeni bir VM'ye veri diski ekleme *az vm disk ekleme* ve parametre *--yeni*

**Şablon**

* Powershell veya Azure CLI ile bir VHD'yi karşıya yükleme
* (İsteğe bağlı) Powershell, Azure CLI veya Azure portalı ile bir VHD'den yönetilen Disk oluşturma
* VHD gösterildiği başvuran bir JSON şablon ile VM dağıtma [Bu örnek JSON şablonunu](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-specialized-vhd-new-or-existing-vnet/azuredeploy.json) ya da gösterildiği gibi yönetilen Diskler'i kullanarak [Bu örnek JSON şablonunu](https://github.com/Azure/azure-quickstart-templates/blob/master/sap-2-tier-user-image-md/azuredeploy.json).

#### <a name="deployment-of-a-vm-image"></a>Bir VM görüntüsü dağıtımını
Böyle bir VM veya VHD gerekir bölümde listelenen gereksinimlerini karşılamak bir Azure VM görüntüsü olarak kullanmak için şirket içi ağdan bir var olan bir sanal makine veya VHD yüklenecek [SAP için bir müşteriye özel görüntü ile bir VM dağıtımı için hazırlık] [ planning-guide-5.2.2] bu belgenin.

* Kullanım *sysprep* Windows üzerinde veya *waagent-sağlamayı kaldırma* Linux VM'nize - genelleştirmek için bkz. [Sysprep teknik başvuru](https://technet.microsoft.com/library/cc766049.aspx) Windows için veya [yakalama bir Resource Manager şablonu olarak kullanmak üzere Linux sanal makinesi] [ capture-image-linux-step-2-create-vm-image] Linux
* Aboneliğinizde oturum açın *AzAccount Bağlan*
* Bağlamınızın ile aboneliği ayarlamak *kümesi AzContext* ve Subscriptionıd parametresi veya - SubscriptionName bakın <https://docs.microsoft.com/powershell/module/az.accounts/set-Azcontext>
* VHD'yi karşıya yükleme *Ekle AzVhd* bir Azure depolama hesabı - bkz. <https://docs.microsoft.com/powershell/module/az.compute/add-Azvhd>
* (İsteğe bağlı) İle bir VHD'den yönetilen Disk görüntüsü oluşturma *yeni AzImage* -bakın <https://docs.microsoft.com/powershell/module/az.compute/new-Azimage>
* Yeni bir VM yapılandırması için işletim sistemi diskini ayarlayın
  * VHD ile *AzVMOSDisk Set - SourceImageUri - CreateOption Fromımage* -bakın <https://docs.microsoft.com/powershell/module/az.compute/set-Azvmosdisk>
  * Yönetilen disk görüntüsü *kümesi AzVMSourceImage* -bakın <https://docs.microsoft.com/powershell/module/az.compute/set-Azvmsourceimage>
* VM yapılandırması ile yeni VM oluşturma *New-AzVM* -bakın <https://docs.microsoft.com/powershell/module/az.compute/new-Azvm>

**Azure CLI**

* Kullanım *sysprep* Windows üzerinde veya *waagent-sağlamayı kaldırma* Linux VM'nize - genelleştirmek için bkz. [Sysprep teknik başvuru](https://technet.microsoft.com/library/cc766049.aspx) Windows için veya [yakalama bir Resource Manager şablonu olarak kullanmak üzere Linux sanal makinesi] [ capture-image-linux-step-2-create-vm-image] Linux
* Aboneliğinizde oturum açın *az oturum açma*
* Aboneliğinizi seçin *az hesabı set--abonelik `<subscription name or id`>*
* VHD'yi karşıya yükleme *az storage blob upload* -bkz [Azure depolama ile Azure CLI kullanma][storage-azure-cli]
* (İsteğe bağlı) İle bir VHD'den yönetilen Disk görüntüsü oluşturma *az görüntü oluşturma* -bakın https://docs.microsoft.com/cli/azure/image
* Karşıya yüklenen VHD veya yönetilen Disk görüntüsü ile işletim sistemi diski olarak belirterek yeni bir VM oluşturma *az vm oluşturma* ve parametre *--görüntüsü*

**Şablon**

* Kullanım *sysprep* Windows üzerinde veya *waagent-sağlamayı kaldırma* Linux VM'nize - genelleştirmek için bkz. [Sysprep teknik başvuru](https://technet.microsoft.com/library/cc766049.aspx) Windows için veya [yakalama bir Resource Manager şablonu olarak kullanmak üzere Linux sanal makinesi] [ capture-image-linux-step-2-create-vm-image] Linux
* Powershell veya Azure CLI ile bir VHD'yi karşıya yükleme
* (İsteğe bağlı) Powershell, Azure CLI veya Azure portalı ile bir VHD'den yönetilen Disk görüntüsü oluşturma
* VHD görüntüsü gösterildiği başvuran bir JSON şablon ile VM dağıtma [Bu örnek JSON şablonunu](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-specialized-vhd-new-or-existing-vnet/azuredeploy.json) veya yönetilen Disk görüntüsü gösterildiği gibi kullanarak [Bu örnek JSON şablonunu](https://github.com/Azure/azure-quickstart-templates/blob/master/sap-2-tier-user-image-md/azuredeploy.json).

#### <a name="downloading-vhds-or-managed-disks-to-on-premises"></a>Şirket içi VHD'leri veya yönetilen diskler yükleniyor
Azure hizmet olarak altyapı yalnızca VHD'ler ve SAP yükleyebildiğini olmanın tek yönlü bir sokak değil sistemler. SAP taşıyabilirsiniz azure'dan sistemleri de şirket içi dünyaya yedekleme.

Yönetilen diskler ve VHD'ler indirme olduğu süre boyunca etkin olamaz. Vm'lere bağlı diskler, hatta indirirken VM'nin kapatılmasını ve serbest gerekir. Yalnızca, ardından ayarlamak için kullanılması gereken veritabanı içeriği karşıdan yüklemek isterseniz yeni bir sistem şirket içi ve kabul edilebilir düzeydeyse, karşıdan yükleme süresini ve azure'da sistem yine de yeni sisteme kurulumu sırasında çalışır durumda , bir diske sıkıştırılmış veritabanı yedeklemesini gerçekleştirerek uzun bir kapalı kalma süresini önlemek ve aynı zamanda işletim sistemi temel VM indirmek yerine bu diski hemen indirin.

#### <a name="powershell"></a>Powershell

* Yönetilen Disk indiriliyor  
  İlk erişmek için yönetilen Disk, temel alınan blob gerekir. Ardından yeni bir depolama hesabı için temel alınan blob kopyalayıp bu depolama hesabındaki blob indirin.

  ```powershell
  $access = Grant-AzDiskAccess -ResourceGroupName <resource group> -DiskName <disk name> -Access Read -DurationInSecond 3600
  $key = (Get-AzStorageAccountKey -ResourceGroupName <resource group> -Name <storage account name>)[0].Value
  $destContext = (New-AzStorageContext -StorageAccountName <storage account name -StorageAccountKey $key)
  Start-AzStorageBlobCopy -AbsoluteUri $access.AccessSAS -DestContainer <container name> -DestBlob <blob name> -DestContext $destContext
  # Wait for blob copy to finish
  Get-AzStorageBlobCopyState -Container <container name> -Blob <blob name> -Context $destContext
  Save-AzVhd -SourceUri <blob in new storage account> -LocalFilePath <local file path> -StorageKey $key
  # Wait for download to finish
  Revoke-AzDiskAccess -ResourceGroupName <resource group> -DiskName <disk name>
  ```

* VHD indirme  
  SAP sistemine durduruldu sonra VM kapatılırken oluşur, şirket içi dünyasına VHD diskleri indirmek için şirket içi hedef kaydetme AzVhd PowerShell cmdlet'ini kullanabilirsiniz. Bunu yapmak için 'depoda Bölümü' bulabilirsiniz VHD URL'si gerekir (depolama hesabı ve VHD oluşturulduğu depolama kapsayıcısı için gitmek için gereklidir) Azure portalı ve sizin için VHD nereye kopyalanacağını bilmeniz gerekir.

  Ardından SourceUri parametresi indirmek için VHD ve LocalFilePath URL'sini VHD'yi (adı dahil) fiziksel konumu olarak tanımlayarak komutu yararlanabilirsiniz. Komutu gibi görünebilir:

  ```powerhell
  Save-AzVhd -ResourceGroupName <resource group name of storage account> -SourceUri http://<storage account name>.blob.core.windows.net/<container name>/sapidedata.vhd -LocalFilePath E:\Azure_downloads\sapidesdata.vhd
  ```

  Kaydet-AzVhd cmdlet'inin daha fazla ayrıntı için buraya bakın <https://docs.microsoft.com/powershell/module/az.compute/save-Azvhd>.

#### <a name="azure-cli"></a>Azure CLI'si
* Yönetilen Disk indiriliyor  
  İlk erişmek için yönetilen Disk, temel alınan blob gerekir. Ardından yeni bir depolama hesabı için temel alınan blob kopyalayıp bu depolama hesabındaki blob indirin.
  ```
  az disk grant-access --ids "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" --duration-in-seconds 3600
  az storage blob download --sas-token "<sas token>" --account-name <account name> --container-name <container name> --name <blob name> --file <local file>
  az disk revoke-access --ids "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>"
  ```

* VHD indirme   
  SAP sistemine durduruldu sonra VM kapatılırken oluşur, Azure CLI komutunu kullanabilirsiniz _azure depolama blobu indirme_ VHD indirmek için şirket içi hedef diskler için şirket içi dünyanın geri. (Depolama hesabı ve depolama kapsayıcısını VHD oluşturulduğu yere gitmek için gereklidir) Azure portal'ın 'depolama bölümünde' bulabilirsiniz VHD kapsayıcısı ve VHD Kopyala olması gereken yerde bilmeniz gerekir ve bunu yapabilmek için adı gerekir. ied için.

  Ardından VHD kapsayıcısı ve parametreleri blob indirme ve hedef için VHD'yi (adı dahil) fiziksel hedef konumu olarak tanımlayarak komutu yararlanabilirsiniz. Komutu gibi görünebilir:

  ```
  az storage blob download --name <name of the VHD to download> --container-name <container of the VHD to download> --account-name <storage account name of the VHD to download> --account-key <storage account key> --file <destination of the VHD to download>
  ```

### <a name="transferring-vms-and-disks-within-azure"></a>VM'ler ve Azure diskleri aktarma

#### <a name="copying-sap-systems-within-azure"></a>Azure'da SAP sistemlerini kopyalama

Büyük olasılıkla bir SAP sistemiyle veya hatta bir SAP uygulama katmanında destekleyen ayrılmış DBMS sunucusu ikili dosyaları içeren işletim sistemi veya veri ve günlük dosyaları SAP veritabanının içeren birden çok disk oluşur. Diskleri kopyalamanın Azure işlevlerini ne diskleri yerel bir diske kaydedilirken Azure işlevlerini hangi anlık görüntüler bir eşitleme mekanizması tutarlı bir şekilde birden çok disk varsa. Bu nedenle, bu karşı aynı VM oluşturulmuş olsa bile kopyalanan veya kaydedilmiş disklerin durumunu farklı olacaktır. Bu, somut durumunda farklı veri ve farklı disklerin bulunan logfile(s) yaşama uçtaki veritabanı tutarsız olacağı anlamına gelir.

**Sonuç: Kopyalayın veya bir SAP sistem yapılandırmasının bir parçası olan diskleri kaydetmek için SAP sistemine durdurun ve ayrıca dağıtılan sanal makineyi gerek gerekir. Ancak bundan sonra kopyalayabilir veya ya da Azure veya şirket içi SAP sistemine bir kopyasını oluşturmak için disk kümesi indirin.**

Veri diskleri, Azure depolama hesabınız VHD dosyaları olarak depolanır ve doğrudan bir sanal makineye bağlı veya bir görüntü olarak kullanılacak. Bu durumda, VHD'yi sanal makineye bağlı önce başka bir konuma kopyalanır. Azure'da VHD dosyasının tam adı Azure içinde benzersiz olmalıdır. Zaten daha önce belirtildiği gibi tür gibi görünen bir üç bölümü adı adıdır:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

Veri disklerini yönetilen disklere de olabilir. Bu durumda, yönetilen Disk sanal makineye bağlı önce yeni bir yönetilen Disk oluşturmak için kullanılır. Yönetilen Disk adı, bir kaynak grubu içinde benzersiz olmalıdır.

##### <a name="powershell"></a>Powershell

Bir VHD gösterildiği kopyalamak için Azure PowerShell cmdlet'lerini kullanabilirsiniz [bu makalede][storage-powershell-guide-full-copy-vhd]. Yeni bir yönetilen Disk oluşturmak için New-AzDiskConfig ve yeni AzDisk aşağıdaki örnekte gösterildiği gibi kullanın.

```powershell
$config = New-AzDiskConfig -CreateOption Copy -SourceUri "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" -Location <location>
New-AzDisk -ResourceGroupName <resource group name> -DiskName <disk name> -Disk $config
```

##### <a name="azure-cli"></a>Azure CLI'si

Bir VHD gösterildiği kopyalamak için Azure CLI kullanabilirsiniz [bu makalede][storage-azure-cli-copy-blobs]. Yeni bir yönetilen Disk oluşturmak için kullanın *az disk oluşturma* aşağıdaki örnekte gösterildiği gibi.

```
az disk create --source "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" --name <disk name> --resource-group <resource group name> --location <location>
```

##### <a name="azure-storage-tools"></a>Azure depolama araçları

* <https://storageexplorer.com/>

Azure depolama gezginleri Professional sürümlerinde burada bulunabilir:

* <https://www.cerebrata.com/>
* <http://clumsyleaf.com/products/cloudxplorer>

Bir depolama hesabında VHD kendisi yalnızca birkaç saniye (SAN donanım anlık görüntüleri üzerinde yazma yavaş kopyalama ve kopyalama ile oluşturma benzer) alan bir işlem kopyasıdır. VHD dosyasının bir kopyasını oluşturduktan sonra bir sanal makineye ekleyin veya sanal makinelere VHD'nin kopyasını eklemek için bir görüntü olarak kullanın.

##### <a name="powershell"></a>Powershell

```powershell
# attach a vhd to a vm
$vm = Get-AzVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzVMDataDisk -VM $vm -Name newdatadisk -VhdUri <path to vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption attach
$vm | Update-AzVM

# attach a managed disk to a vm
$vm = Get-AzVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzVMDataDisk -VM $vm -Name newdatadisk -ManagedDiskId <managed disk id> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption attach
$vm | Update-AzVM

# attach a copy of the vhd to a vm
$vm = Get-AzVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzVMDataDisk -VM $vm -Name <disk name> -VhdUri <new path of vhd> -SourceImageUri <path to image vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption fromImage
$vm | Update-AzVM

# attach a copy of the managed disk to a vm
$vm = Get-AzVM -ResourceGroupName <resource group name> -Name <vm name>
$diskConfig = New-AzDiskConfig -Location $vm.Location -CreateOption Copy -SourceUri <source managed disk id>
$disk = New-AzDisk -DiskName <disk name> -Disk $diskConfig -ResourceGroupName <resource group name>
$vm = Add-AzVMDataDisk -VM $vm -Caching <caching option> -Lun <lun, for example 0> -CreateOption attach -ManagedDiskId $disk.Id
$vm | Update-AzVM
```
##### <a name="azure-cli"></a>Azure CLI'si

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
Bu görevi Azure portalı üzerinde gerçekleştirilemez. Azure PowerShell cmdlet'lerini, Azure CLI veya bir üçüncü taraf depolama tarayıcı kullanabilirsiniz. PowerShell cmdlet'lerini veya CLI komutları, oluşturun ve BLOB'ları, ama zaman uyumsuz olarak ve bölgeleri Azure aboneliğinde depolama hesapları arasında BLOB kopyalama özelliği dahil yönetin.

##### <a name="powershell"></a>Powershell
Ayrıca, VHD'leri abonelikler arasında kopyalayabilirsiniz. Daha fazla bilgi için okuma [bu makalede][storage-powershell-guide-full-copy-vhd].

PS cmdlet'i mantığı temel akışı şöyle görünür:

* İçin bir depolama hesabı bağlamını oluşturun **kaynak** depolama hesabıyla *yeni AzStorageContext* -bakın <https://docs.microsoft.com/powershell/module/az.storage/new-AzStoragecontext>
* İçin bir depolama hesabı bağlamını oluşturun **hedef** depolama hesabıyla *yeni AzStorageContext* -bakın <https://docs.microsoft.com/powershell/module/az.storage/new-AzStoragecontext>
* Kopyalama işlemiyle başlayın

```powershell
Start-AzStorageBlobCopy -SrcBlob <source blob name> -SrcContainer <source container name> -SrcContext <variable containing context of source storage account> -DestBlob <target blob name> -DestContainer <target container name> -DestContext <variable containing context of target storage account>
```

* Bir döngü ile kopyalama durumunu denetleyin

```powershell
Get-AzStorageBlobCopyState -Blob <target blob name> -Container <target container name> -Context <variable containing context of target storage account>
```

* Yeni VHD, yukarıda açıklanan şekilde bir sanal makineye ekleyin.

Örnekler için bkz [bu makalede][storage-powershell-guide-full-copy-vhd].

##### <a name="azure-cli"></a>Azure CLI'si
* Kopyalama işlemiyle başlayın

```
az storage blob copy start --source-blob <source blob name> --source-container <source container name> --source-account-name <source storage account name> --source-account-key <source storage account key> --destination-container <target container name> --destination-blob <target blob name> --account-name <target storage account name> --account-key <target storage account name>
```

* Kopyalama ile döngüsü aşamasında ise durumunu kontrol edin.

```
az storage blob show --name <target blob name> --container <target container name> --account-name <target storage account name> --account-key <target storage account name>
```

* Yeni VHD, yukarıda açıklanan şekilde bir sanal makineye ekleyin.

Örnekler için bkz [bu makalede][storage-azure-cli-copy-blobs].

### <a name="disk-handling"></a>Disk işleme

#### <a name="4efec401-91e0-40c0-8e64-f2dceadff646"></a>SAP dağıtımları için VM/disk yapısı

İdeal olarak bir VM ile ilişkili diskler yapısını işlenmesini basit olması gerekir. Şirket içi yüklemelerde sunucu yüklemesi yapılandırılması için birçok yolu müşteriler geliştirilmiştir.

* İşletim sistemi ve DBMS ve/veya SAP tüm ikili dosyaları içeren bir temel disk. Mart 2015 tarihinden itibaren bu disk boyutu 127 GB ile sınırlıdır önceki kısıtlamaları yerine 1 TB'ye kadar olabilir.
* (DBMS destekliyorsa) SAP veritabanının DBMS günlük dosyası ve günlük dosyası DBMS geçici depolama alanı içeren bir veya birden çok diskler. Veritabanı günlük IOPS gereksinimlerine yüksekse, gereken IOPS birimi ulaşmak için birden çok disk stripe gerekir.
* SAP veritabanına bir veya iki veritabanı dosyalarının ve DBMS geçici veri dosyalarını da (DBMS destekliyorsa) içeren disk sayısı.

![SAP için Azure Iaas VM başvuru yapılandırması][planning-guide-figure-1300]


- - -
> ![Windows][Logo_Windows] Windows
>
> Birçok müşteri ile yapılandırmaları, örneğin, SAP ve DBMS ikili dosyaları işletim Sisteminin yüklendiği c:\ sürücüde yüklenmedi gördük. Çeşitli nedenlerden dolayı bu vardı, ancak biz geri kök oluştu, bunu genellikle sürücüleri küçük ve işletim sistemi yükseltmelerini ek alan 10-15 yıl önce gerekli oluyordu. Bugünlerde artık çok sık koşulların her ikisi de geçerli değildir. Bugün c:\ sürücüsüne büyük hacim disklerinin veya Vm'lerinde eşlenebilir. Dağıtımları kendi yapısında basit tutmak için aşağıdaki dağıtım desende azure'da SAP NetWeaver sistemleri için önerilir
>
> Windows işletim sistemi disk belleği dosyası D: sürücüsünde (kalıcı disk) olmalıdır
>
> ![Linux][Logo_Linux] Linux
>
> Linux swapfile /mnt/mnt/kaynak altında açıklandığı gibi Linux üzerinde yerleştirmek [bu makalede][virtual-machines-linux-agent-user-guide]. Takas dosyası Linux Aracısı /etc/waagent.conf yapılandırma dosyasında yapılandırılabilir. Ekleme veya aşağıdaki ayarları değiştirebilirsiniz:
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

Lütfen'lu SAP notuna okuyun [1597355] önerilen takas dosyası boyutu hakkında daha fazla bilgi

- - -
DBMS veri dosyaları ve bu disklerin barındırılan Azure depolama türü için kullanılan disk sayısını IOPS gereksinimler ve gereken gecikme tarafından belirlenmesi. Tam kotalar açıklanmıştır [bu makalede (Linux)] [ virtual-machines-sizes-linux] ve [bu makalede (Windows)][virtual-machines-sizes-windows].

SAP dağıtımları son iki yılda bir deneyim bize olarak özetlenebilir bazı dersleri verilen:

* Var olan müşteri sistemleri farklı kendi SAP veritabanlarını temsil eden veri dosyalarını boyutta IOPS trafiğini farklı veri dosyaları için her zaman aynı değildir. Sonuç olarak, bir RAID yapılandırması LUN'ları olanlar dışında gerekmez veri dosyaları yerleştirmek için birden çok disk üzerinde daha iyi kullanılmasını dönüştü. Burada bir IOPS oranı DBMS işlem günlüğü karşı tek bir disk kotası isabet Azure standart depolama ile özellikle durumlarda vardı. Böyle senaryolarda, Premium depolama kullanılması önerilir veya alternatif olarak, birden çok standart depolama toplayarak bir yazılım disklerle stripe.

- - -
> ![Windows][Logo_Windows] Windows
>
> * [Azure sanal Makineler'de SQL Server için en iyi performans][virtual-machines-sql-server-performance-best-practices]
>
> ![Linux][Logo_Linux] Linux
>
> * [Linux'ta yazılım RAID yapılandırma][virtual-machines-linux-configure-raid]
> * [Azure'da Linux sanal makinesi üzerinde LVM'yi yapılandırma][virtual-machines-linux-configure-lvm]
> * [Azure depolama gizli dizileri ve Linux g/ç en iyi duruma getirme](https://blogs.msdn.com/b/igorpag/archive/2014/10/23/azure-storage-secrets-and-linux-i-o-optimizations.aspx)
>
>

- - -
* Premium depolama, özellikle kritik işlem günlüğüne yazma işlemleri için önemli bir daha iyi performans göstermez. Üretim gibisi performans sunmak için beklenen SAP senaryoları için Azure Premium depolama kullanan sanal makine serisi kullanılacak önemle tavsiye edilir.

Aklınızda işletim sistemi içerir, disk ve öneririz, SAP ikili dosyaları ve veritabanı (temel VM) de artık 127 GB ile sınırlı değildir. Boyutu 1 TB'ye kadar artık olabilir. Bu, örneğin, SAP toplu iş günlükleri gibi tüm gerekli dosya tutmak için yeterli alan olmalıdır.

Daha fazla öneri ve özellikle DBMS VM'ler için daha fazla ayrıntı için danışın [DBMS Dağıtım Kılavuzu][dbms-guide]

#### <a name="disk-handling"></a>Disk işleme

Çoğu senaryoda, VM'de oturum SAP veritabanı dağıtmak için ek diskler oluşturmanız gerekir. Bölüm diskleri sayısı konuları hakkında konuştuk [SAP dağıtımları için VM/disk yapısının] [ planning-guide-5.5.1] bu belgenin. Azure portalı, ekleme ve temel VM dağıtıldıktan sonra Disk ayırma için izin verir. Disklerin bağlı/ayrılmış sanal makine çalışır durumda olduğunda ve ne zaman durdurulduğu yanı sıra çalışan olabilir. Bir disk eklerken, boş bir disk veya bu anda başka bir sanal makineye bağlı olmayan var olan bir diski eklemek Azure portalı sunar.

**Not**: Diskler herhangi bir zamanda yalnızca bir VM'ye iliştirilebilir.

![Ekleme / Azure standart depolama ile disk detach][planning-guide-figure-1400]

Yeni bir sanal makine dağıtımı sırasında yönetilen diskleri kullanmanız veya Azure depolama hesaplarında disklerinizi yerleştirmek istediğinize karar verin. Premium depolama kullanmak istiyorsanız, yönetilen diskler kullanılması önerilir.

Ardından, daha önce karşıya yüklendi ve artık VM'ye bağlı var olan bir diski seçmek istediğiniz veya yeni ve boş bir disk oluşturmak istediğinize karar vermeniz gerekir.

**ÖNEMLİ**: **Yok** ana bilgisayar önbelleğe alma, standart Azure Depolama'yı kullanmak istiyorsanız. Konak önbelleği tercihi hiçbiri varsayılan olarak bırakmalısınız. G/ç özelliğini çoğunlukla tipik g/ç trafiğinin veritabanı veri dosyaları gibi salt okunur ise Azure Premium depolama ile okuma önbelleği etkinleştirmeniz gerekir. Veritabanı işlem günlüğü dosyası olması durumunda, önbelleğe alma önerilir.

- - -
> ![Windows][Logo_Windows] Windows
>
> [Azure portalında bir veri diski ekleme][virtual-machines-linux-attach-disk-portal]
>
> Bağlı diskleri, VM'ye Windows Disk Yöneticisi'ni açmak için oturum açmanız gerekir. Otomatik bağlama bölümde önerildiği şekilde etkin değilse [bağlı diskleri için otomatik bağlama ayarlama][planning-guide-5.5.3], yeni eklenen birimin çevrimiçi ve başlatılmış yapılmasına gerek.
>
> ![Linux][Logo_Linux] Linux
>
> Bağlı diskleri sanal Makineye oturum açmak ve açıklandığı gibi diskleri başlatmak gerekirse [bu makalede][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]
>
>

- - -
Yeni disk boş bir diski olduğundan, de diski biçimlendirmek gerekir. Biçimlendirme için özellikle DBMS verilerin ve günlük dosyalarının aynı önerileri DBMS, çıplak dağıtımlar için geçerlidir.

Zaten bölümde bahsedilen [Microsoft Azure sanal makine kavramı][planning-guide-3.2], bir Azure depolama hesabının g/ç hacmi, IOPS ve veri hacmi açısından sonsuz kaynaklarını sağlamıyor. Genellikle DBMS Vm'leri en bu tarafından etkilenir. Birkaç yüksek g/ç toplu VM'ler için Azure depolama hesabı birimin sınırın içinde kalmanızı dağıtmak için varsa, her VM için ayrı bir depolama hesabı kullanmak en iyi olabilir. Aksi takdirde, bu VM'lerin her tek bir depolama hesabının limitini aştıktan olmadan farklı depolama hesapları arasında nasıl dengeleyebilirsiniz görmek gerekir. Daha fazla ayrıntı ele alınmıştır [DBMS Dağıtım Kılavuzu][dbms-guide]. Ayrıca, saf SAP uygulama sunucusu sanal makineleri veya sonunda ek VHD'ler yapmanızı şart koşabileceği diğer Vm'lere aklınızda Bu sınırlamalar tutmanız gerekir. Yönetilen Disk kullanıyorsanız, bu kısıtlama geçerli değildir. Premium depolama kullanmayı planlıyorsanız, yönetilen Disk kullanmanızı öneririz.

Depolama hesapları için uygun olan başka bir konu, coğrafi olarak çoğaltılmış bir depolama hesabında VHD mi alıyorsanız ' dir. Coğrafi çoğaltma etkin veya depolama hesabı düzeyinde ve sanal makine düzeyinde değil, devre dışı. Coğrafi çoğaltma etkinleştirildiğinde, depolama hesabında VHD ile aynı bölgede başka bir Azure veri merkezine çoğaltılır. Bu karar vermeden önce aşağıdaki kısıtlama hakkında almalısınız:

Azure coğrafi çoğaltma, her VHD bir sanal makinede yerel olarak çalışır ve IOs kronolojik sırada bir VM'de birden çok VHD arasında çoğaltılmaz. Bu nedenle, VM'ye ek VHD'lerin yanı sıra temel VM'yi temsil VHD birbirinden bağımsız çoğaltılır. Bu, farklı VHD değişiklikleri arasındaki eşitleme yoktur anlamına gelir. IOs anlamına gelir yazılan sırasını bağımsız olarak çoğaltılır olgu, coğrafi çoğaltma üzerinde birden çok VHD dağıtılmış kendi veritabanına sahip veritabanı sunucuları için değerin altında değil. DBMS yanı sıra, ayrıca olabilir burada işlemleri yazma veya verileri farklı VHD'ler ve değişiklikleri sıralamasını korumak önemli olduğu diğer uygulamalar. Azure'da coğrafi çoğaltma, bir gereksinimi varsa etkinleştirilmemelidir. Bağımlı olup olmadığını size gereken veya coğrafi çoğaltma için bir VM kümesi, ancak başka bir kümesi için değil istediğiniz şirket, zaten Vm'leri ve ilgili Vhdleri geografickou replikaci etkin veya devre dışı olduğu farklı depolama hesaplarında kategorilere ayırabilirsiniz.

#### <a name="17e0d543-7e8c-4160-a7da-dd7117a1ad9d"></a>Otomatik bağlama bağlı diskleri için ayarlama
- - -
> ![Windows][Logo_Windows] Windows
>
> Kendi görüntülerini veya diskleri oluşturulan VM'ler için denetleyin ve büyük olasılıkla otomatik bağlama parametresi gereklidir. Bu parametre ayarı VM yeniden başlatma veya yeniden dağıtım bağlı ve takılı sürücüleri otomatik olarak yeniden bağlamak için azure'de sonra izin verir.
> Azure marketi'ndeki Microsoft tarafından sağlanan görüntüleri için parametresini ayarlayın.
>
> Otomatik bağlama ayarlamak için buraya komut satırı yürütülebilir diskpart.exe belgelerine bakın:
>
> * [DiskPart komut satırı seçenekleri](https://technet.microsoft.com/library/bb490893.aspx)
> * [Otomatik bağlama](https://technet.microsoft.com/library/cc753703.aspx)
>
> Windows komut satırı penceresini yönetici olarak açılması gerekir.
>
> Bağlı diskleri, VM'ye Windows Disk Yöneticisi'ni açmak için oturum açmanız gerekir. Otomatik bağlama bölümde önerildiği şekilde etkin değilse [bağlı diskleri için otomatik bağlama ayarlama][planning-guide-5.5.3], yeni, birimin bağlı > Çevrimiçi ve başlatılmış yapılmasına gerek.
>
> ![Linux][Logo_Linux] Linux
>
> Yeni eklenen bir boş disk açıklandığı başlatmak gereken [bu makalede][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux].
> Ayrıca /etc/fstab için yeni bir disk eklemeniz gerekir.
>
>

- - -
### <a name="final-deployment"></a>Son dağıtım

Son dağıtım ve özellikle SAP, genişletilmiş izleme, dağıtım bakımından tam adımlar için bkz [Dağıtım Kılavuzu][deployment-guide].

## <a name="accessing-sap-systems-running-within-azure-vms"></a>Azure sanal makineler içinde çalışan SAP sistemlerini erişme

SAP GUI kullanarak genel internet bu SAP sistemlere bağlanmak için istediğiniz senaryolar için aşağıdaki yordamları uygulanması gerekir.

Belgede daha sonra diğer ana senaryo bir siteden siteye bağlantı (VPN tüneli) veya Azure ExpressRoute bağlantısı şirket içi sistemler ve Azure sistemleri arasında şirket içi dağıtımlarda, SAP sistemlerini bağlanma ele alınacaktır.

### <a name="remote-access-to-sap-systems"></a>SAP sistemlerini uzaktan erişim

Azure Resource Manager ile önceki Klasik modelde varsayılan uç nokta artık gibi vardır. Bir Azure Resource Manager sanal makinenin tüm bağlantı noktalarının açık olduğundan sürece:

1. Ağ güvenlik grubu, alt ağ veya ağ arabirimi için tanımlanır. Azure Vm'leri için ağ trafiğini sözde "ağ güvenlik grupları" güvenli hale getirilebilir. Daha fazla bilgi için [bir ağ güvenlik grubu (NSG) nedir?][virtual-networks-nsg]
2. Azure Load Balancer olmamasını ağ arabirimi için tanımlanır.   

Klasik modeli ve ARM mimarisi farkı açıklandığı bkz [bu makalede][virtual-machines-azure-resource-manager-architecture].

#### <a name="configuration-of-the-sap-system-and-sap-gui-connectivity-over-the-internet"></a>İnternet üzerinden SAP sistemine ve SAP GUI bağlantı yapılandırma

Lütfen bu konunun ayrıntılarını açıklayan bu makaleye bakın: <https://blogs.msdn.com/b/saponsqlserver/archive/2014/06/24/sap-gui-connection-closed-when-connecting-to-sap-system-in-azure.aspx>

#### <a name="changing-firewall-settings-within-vm"></a>VM Güvenlik Duvarı ayarlarını değiştirme

Sanal makinelerinizde SAP sisteminize gelen trafiğe izin vermek için güvenlik duvarını yapılandırmanız gerekebilir.

- - -
> ![Windows][Logo_Windows] Windows
>
> Varsayılan olarak, Windows Güvenlik Duvarı Azure dağıtılan VM içinde etkinleştirilir. Artık SAP bağlantı noktasının açılması izin vermeniz gerekir, aksi takdirde SAP GUI bağlanmak mümkün olmayacaktır.
> Bunu yapmak için:
>
> * Denetim masası\sistem ve güvenlik\windows Güvenlik Duvarı'nda açın **Gelişmiş ayarlar**.
> * Şimdi gelen kurallar üzerinde sağ tıklayın ve seçtiğiniz **yeni kural**.
> * Aşağıdaki sihirbazda seçtiğiniz yeni bir **bağlantı noktası** kuralı.
> * Sihirbazın bir sonraki adımda, açmak istediğiniz bağlantı noktası numarasını TCP ve türü ayarında bırakın. SAP bizim örnek kimliği 00 olduğundan, 3200 attık. Farklı örnek numarasını örneğiniz varsa, daha önce örnek sayısına göre tanımlanmış bağlantı açılmalıdır.
> * Sihirbazın sonraki kısmında öğesi bırakın gerek **izin bağlantısı** işaretli.
> * Sihirbazın bir sonraki adımında, kuralın etki alanı, özel ve genel ağ için geçerli olacağını tanımlamak gerekir. Lütfen gerekiyorsa ihtiyaçlarınıza ayarlayın. Bununla birlikte, SAP GUI ile dış ortak ağ üzerinden bağlanırken, ortak ağa uygulanan kural olması gerekir.
> * Sihirbazın son adımda, kural adı ve tuşlarına basarak kaydedin **son**.
>
> Kural hemen etkili olur.
>
> ![Bağlantı noktası kural tanımı][planning-guide-figure-1600]
>
> ![Linux][Logo_Linux] Linux
>
> Azure Market'teki Linux görüntüleri iptables Güvenlik Duvarı varsayılan olarak etkinleştirmeyin ve SAP sisteminize bağlantı çalışması gerekir. İptables veya başka bir güvenlik duvarı etkinleştirilirse, iptables (xx SAP sisteminizin sistem numarası olduğu yer) bağlantı noktası 32xx gelen tcp trafiğine izin vermek için kullanılan güvenlik duvarı veya belgelerine başvurun.
>
>

- - -
#### <a name="security-recommendations"></a>Güvenlik önerileri

SAP GUI herhangi biri çalışıyor, ancak ilk SAP ileti sunucusu işlemi (bağlantı noktası 36xx) açılan bağlantı noktası üzerinden bağlanır SAP örneklerin (bağlantı noktası 32xx) hemen bağlanmaz. Geçmişte, aynı bağlantı noktasını iç iletişimi uygulama örneklerine ileti sunucusu tarafından kullanıldı. Şirket içi uygulama sunucuları yanlışlıkla azure'da bir ileti sunucusu ile iletişim kurmasını engellemek için iç iletişim bağlantı noktalarını değiştirilebilir. Farklı bir bağlantı noktası, bir kopyasını geliştirme için Proje test vb. gibi şirket içi sistemlerden kopyalanan sistemlerinde, uygulama örnekleri ile SAP ileti sunucusu arasındaki dahili iletişimi değiştirmek için önerilir. Bu varsayılan profil parametresi ile yapılabilir:

> rdisp/msserv_internal
>
>

açıklandığı gibi [SAP ileti sunucusu için güvenlik ayarları](https://help.sap.com/saphelp_nwpi71/helpdata/en/47/c56a6938fb2d65e10000000a42189c/content.htm)


### <a name="3e9c3690-da67-421a-bc3f-12c520d99a30"></a>Tek tanıtım/senaryo eğitim NetWeaver SAP VM

![Tek başına VM SAP tanıtım sistemler ile aynı VM adlarını çalıştırmak, Azure bulut Hizmetleri'nde yalıtılmış][planning-guide-figure-1700]

Bu senaryoda, biz tek bir VM'de tam bir eğitim/tanıtım senaryo bulunduğu bir tipik eğitim/tanıtım sistem senaryosu uyguluyor. Dağıtım VM görüntüsü şablonları yapılır varsayıyoruz. Ayrıca bu tanıtım/eğitimleri, birden çok VM ile aynı ada sahip Vm'leri dağıtılması gerek varsayıyoruz. Tüm eğitim sistemleri, şirket içi varlıklarınız için bağlantınız ve karma bir dağıtım için bir karşıtı olduğu.

Bölüm bazı bölümlerinde açıklandığı gibi bir VM görüntüsü oluşturdunuz varsayılır [Azure için SAP ile Vm'leri hazırlama] [ planning-guide-5.2] bu belgedeki.

Senaryoyu uygulamak için olaylar dizisini şöyle görünür:

##### <a name="powershell"></a>Powershell

* Her eğitim/tanıtım yatay için yeni bir kaynak grubu oluşturun

```powershell
$rgName = "SAPERPDemo1"
New-AzResourceGroup -Name $rgName -Location "North Europe"
```
* Yönetilen diskleri kullanmayı istemiyorsanız, yeni depolama hesabı oluşturma

```powershell
$suffix = Get-Random -Minimum 100000 -Maximum 999999
$account = New-AzStorageAccount -ResourceGroupName $rgName -Name "saperpdemo$suffix" -SkuName Standard_LRS -Kind "Storage" -Location "North Europe"
```

* Aynı ana bilgisayar adı ve IP adresi kullanımını etkinleştirmek her eğitim/tanıtım yatay için yeni bir sanal ağ oluşturun. Sanal ağ, yalnızca SSH için Uzak Masaüstü erişimi etkinleştirmek için 3389 numaralı bağlantı noktası ve bağlantı noktası 22 trafiğe izin veren bir ağ güvenlik grubu tarafından korunur.

```powershell
# Create a new Virtual Network
$rdpRule = New-AzNetworkSecurityRuleConfig -Name SAPERPDemoNSGRDP -Protocol * -SourcePortRange * -DestinationPortRange 3389 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 100
$sshRule = New-AzNetworkSecurityRuleConfig -Name SAPERPDemoNSGSSH -Protocol * -SourcePortRange * -DestinationPortRange 22 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 101
$nsg = New-AzNetworkSecurityGroup -Name SAPERPDemoNSG -ResourceGroupName $rgName -Location  "North Europe" -SecurityRules $rdpRule,$sshRule

$subnetConfig = New-AzVirtualNetworkSubnetConfig -Name Subnet1 -AddressPrefix  10.0.1.0/24 -NetworkSecurityGroup $nsg
$vnet = New-AzVirtualNetwork -Name SAPERPDemoVNet -ResourceGroupName $rgName -Location "North Europe"  -AddressPrefix 10.0.1.0/24 -Subnet $subnetConfig
```

* İnternet'ten sanal makineye erişmek için kullanılan yeni bir genel IP adresi oluşturma

```powershell
# Create a public IP address with a DNS name
$pip = New-AzPublicIpAddress -Name SAPERPDemoPIP -ResourceGroupName $rgName -Location "North Europe" -DomainNameLabel $rgName.ToLower() -AllocationMethod Dynamic
```

* Sanal makine için yeni bir ağ arabirimi oluşturma

```powershell
# Create a new Network Interface
$nic = New-AzNetworkInterface -Name SAPERPDemoNIC -ResourceGroupName $rgName -Location "North Europe" -Subnet $vnet.Subnets[0] -PublicIpAddress $pip
```

* Sanal makine oluşturur. Bu senaryoda, her VM, aynı ada sahip olacaktır. Bu sanal makineler SAP NetWeaver örneklerde SAP SID'si aynı de olacaktır. Azure kaynak grubunda VM adının benzersiz olması gerekir, ancak farklı Azure kaynak grubunda aynı ada sahip VM'ler çalıştırabilirsiniz. Windows veya Linux için ' root' varsayılan 'Administrator' hesabının geçerli değildir. Bu nedenle, yeni bir yönetici kullanıcı adı ile birlikte bir parola gerekiyor. Ayrıca VM'nin boyutunu tanımlanması gerekir.

```powershell
#####
# Create a new virtual machine with an official image from the Azure Marketplace
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzVMConfig -VMName SAPERPDemo -VMSize Standard_D11

# select image
$vmconfig = Set-AzVMSourceImage -VM $vmconfig -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
$vmconfig = Set-AzVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
# $vmconfig = Set-AzVMSourceImage -VM $vmconfig -PublisherName "SUSE" -Offer "SLES-SAP" -Skus "12-SP1" -Version "latest"
# $vmconfig = Set-AzVMSourceImage -VM $vmconfig -PublisherName "RedHat" -Offer "RHEL" -Skus "7.2" -Version "latest"
# $vmconfig = Set-AzVMSourceImage -VM $vmconfig -PublisherName "Oracle" -Offer "Oracle-Linux" -Skus "7.2" -Version "latest"
# $vmconfig = Set-AzVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Add-AzVMNetworkInterface -VM $vmconfig -Id $nic.Id

$vmconfig = Set-AzVMBootDiagnostics -Disable -VM $vmconfig
$vm = New-AzVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

```powershell
#####
# Create a new virtual machine with a VHD that contains the private image that you want to use
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzVMConfig -VMName SAPERPDemo -VMSize Standard_D11

$vmconfig = Add-AzVMNetworkInterface -VM $vmconfig -Id $nic.Id

$diskName="osfromimage"
$osDiskUri=$account.PrimaryEndpoints.Blob.ToString() + "vhds/" + $diskName  + ".vhd"

$vmconfig = Set-AzVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path to VHD that contains the OS image> -Windows
$vmconfig = Set-AzVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred
#$vmconfig = Set-AzVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path to VHD that contains the OS image> -Linux
#$vmconfig = Set-AzVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Set-AzVMBootDiagnostics -Disable -VM $vmconfig
$vm = New-AzVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

```powershell
#####
# Create a new virtual machine with a Managed Disk Image
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzVMConfig -VMName SAPERPDemo -VMSize Standard_D11

$vmconfig = Add-AzVMNetworkInterface -VM $vmconfig -Id $nic.Id

$vmconfig = Set-AzVMSourceImage -VM $vmconfig -Id <Id of Managed Disk Image>
$vmconfig = Set-AzVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred
#$vmconfig = Set-AzVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Set-AzVMBootDiagnostics -Disable -VM $vmconfig
$vm = New-AzVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

* İsteğe bağlı olarak ek disk ekleyin ve gerekli içeriği geri yükleyin. Tüm blob adları (bloblara URL'ler), Azure içinde benzersiz olmalıdır.

```powershell
# Optional: Attach additional VHD data disks
$vm = Get-AzVM -ResourceGroupName $rgName -Name SAPERPDemo
$dataDiskUri = $account.PrimaryEndpoints.Blob.ToString() + "vhds/datadisk.vhd"
Add-AzVMDataDisk -VM $vm -Name datadisk -VhdUri $dataDiskUri -DiskSizeInGB 1023 -CreateOption empty | Update-AzVM

# Optional: Attach additional Managed Disks
$vm = Get-AzVM -ResourceGroupName $rgName -Name SAPERPDemo
Add-AzVMDataDisk -VM $vm -Name datadisk -DiskSizeInGB 1023 -CreateOption empty -Lun 0 | Update-AzVM
```

##### <a name="cli"></a>CLI

Aşağıdaki kod örneği, Linux üzerinde kullanılabilir. İçin Windows PowerShell yukarıda açıklandığı gibi kullanabilir veya % rgName %$rgName yerine kullanın ve Windows komutunu kullanarak ortam değişkenini ayarlamak için örnekte uyum *ayarlamak*.

* Her eğitim/tanıtım yatay için yeni bir kaynak grubu oluşturun

```
rgName=SAPERPDemo1
rgNameLower=saperpdemo1
az group create --name $rgName --location "North Europe"
```

* Yeni depolama hesabı oluştur

```
az storage account create --resource-group $rgName --location "North Europe" --kind Storage --sku Standard_LRS --name $rgNameLower
```

* Aynı ana bilgisayar adı ve IP adresi kullanımını etkinleştirmek her eğitim/tanıtım yatay için yeni bir sanal ağ oluşturun. Sanal ağ, yalnızca SSH için Uzak Masaüstü erişimi etkinleştirmek için 3389 numaralı bağlantı noktası ve bağlantı noktası 22 trafiğe izin veren bir ağ güvenlik grubu tarafından korunur.

```
az network nsg create --resource-group $rgName --location "North Europe" --name SAPERPDemoNSG
az network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGRDP --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 3389 --access Allow --priority 100 --direction Inbound
az network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGSSH --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 22 --access Allow --priority 101 --direction Inbound

az network vnet create --resource-group $rgName --name SAPERPDemoVNet --location "North Europe" --address-prefixes 10.0.1.0/24
az network vnet subnet create --resource-group $rgName --vnet-name SAPERPDemoVNet --name Subnet1 --address-prefix 10.0.1.0/24 --network-security-group SAPERPDemoNSG
```

* İnternet'ten sanal makineye erişmek için kullanılan yeni bir genel IP adresi oluşturma

```
az network public-ip create --resource-group $rgName --name SAPERPDemoPIP --location "North Europe" --dns-name $rgNameLower --allocation-method Dynamic
```

* Sanal makine için yeni bir ağ arabirimi oluşturma

```
az network nic create --resource-group $rgName --location "North Europe" --name SAPERPDemoNIC --public-ip-address SAPERPDemoPIP --subnet Subnet1 --vnet-name SAPERPDemoVNet
```

* Sanal makine oluşturur. Bu senaryoda, her VM, aynı ada sahip olacaktır. Bu sanal makineler SAP NetWeaver örneklerde SAP SID'si aynı de olacaktır. Azure kaynak grubunda VM adının benzersiz olması gerekir, ancak farklı Azure kaynak grubunda aynı ada sahip VM'ler çalıştırabilirsiniz. Windows veya Linux için ' root' varsayılan 'Administrator' hesabının geçerli değildir. Bu nedenle, yeni bir yönetici kullanıcı adı ile birlikte bir parola gerekiyor. Ayrıca VM'nin boyutunu tanımlanması gerekir.

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

* İsteğe bağlı olarak ek disk ekleyin ve gerekli içeriği geri yükleyin. Tüm blob adları (bloblara URL'ler), Azure içinde benzersiz olmalıdır.

```
# Optional: Attach additional VHD data disks
az vm unmanaged-disk attach --resource-group $rgName --vm-name SAPERPDemo --size-gb 1023 --vhd-uri https://$rgNameLower.blob.core.windows.net/vhds/data.vhd  --new

# Optional: Attach additional Managed Disks
az vm disk attach --resource-group $rgName --vm-name SAPERPDemo --size-gb 1023 --disk datadisk --new
```

##### <a name="template"></a>Şablon

Github'da azure hızlı başlangıç şablonları havuz üzerinde örnek şablonları kullanabilirsiniz.

* [Basit bir Linux VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
* [Basit Windows VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
* [Görüntüden VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

### <a name="implement-a-set-of-vms-that-communicate-within-azure"></a>Azure içinde iletişim kuran VM'ler bir dizi uygulama

Eğitim ve tanıtım için tipik bir senaryo Bu karma olmayan senaryodur nerede tanıtım/eğitim temsil eden yazılım amacıyla senaryo birden çok VM'ye yayılır. Farklı bileşenlerin farklı Vm'lere yüklü birbirleriyle iletişim kurması gerekir. Yeniden, bu senaryoda, iletişimi hiçbir şirket içi ağ veya içi ve dışı karışık senaryo gereklidir.

Bu senaryo bir bölümde açıklanan yükleme uzantısıdır [SAP tanıtım/senaryo eğitim NetWeaver ile tek bir VM] [ planning-guide-7.1] bu belgenin. Bu durumda daha fazla sanal makine mevcut bir kaynak grubuna eklenecektir. Aşağıdaki örnekte, bir SAP ASCS/SCS VM'nin, DBMS ve SAP uygulama sunucusu örneği VM çalıştıran bir VM eğitim yatay oluşur.

Bu senaryo oluşturmadan önce temel ayarları olarak zaten önce senaryoda kullandı dikkat etmeniz gerekir.

#### <a name="resource-group-and-virtual-machine-naming"></a>Kaynak grubunu ve sanal makine adlandırma

Tüm kaynak grubu adları benzersiz olmalıdır. Kendi adlandırma düzeninizi kaynaklarınızın gibi geliştirme `<rg-name`>-soneki.

Sanal makine adı kaynak grubu içinde benzersiz olması gerekir.

#### <a name="set-up-network-for-communication-between-the-different-vms"></a>Farklı makineler arasında kurulan iletişim için ağ kurma ayarlayın

![Bir Azure sanal ağ içindeki sanal makineler kümesi][planning-guide-figure-1900]

Önlemek için aynı eğitim/tanıtım ortamlarını, klonlar çakışmalarla adlandırma, her yatay bir Azure sanal ağ oluşturmanız gerekir. DNS ad çözümlemesi Azure tarafından sağlanır veya kendi DNS sunucunuzu (daha fazla burada tartışılan değil) Azure'a dışında yapılandırabilirsiniz. Bu senaryoda, biz kendi DNS yapılandırmayın. Bir Azure sanal ağ içindeki tüm sanal makineler için konak adları üzerinden iletişim etkinleştirilecektir.

Sanal ağlar ve yalnızca kaynak grupları tarafından eğitimi veya tanıtım ortamlarını ayrı nedeni aşağıdakilerden biri olabilir:

* SAP ortamı ayarlandığı kendi AD/OpenLDAP gerekir ve bir etki alanı sunucusu ortamlarını her bir parçası olması gerekiyor.  
* SAP ortamı kurma olarak sabit IP adresleri ile çalışmak için ihtiyacınız olan bileşenleri vardır.

Azure sanal ağları ve bunları tanımlama hakkında daha fazla ayrıntı bulunabilir [bu makalede][virtual-networks-create-vnet-arm-pportal].

## <a name="deploying-sap-vms-with-corporate-network-connectivity-cross-premises"></a>Kurumsal ağ bağlantısı (şirket içi) ile SAP sanal makineleri dağıtma

Bir SAP ortamının çalıştırma ve çıplak yüksek kaliteli DBMS sunucuları için uygulama katmanları için şirket içinde sanallaştırılmış ortamlar arasında dağıtımı bölmek istiyorsanız ve daha küçük 2 katmanlı SAP sistemlerini ve Azure Iaas yapılandırılmış. Bir SAP ortamının içinde SAP sistemlerini birbirleri ile ve şirket, kendi dağıtım biçiminin bağımsız olarak dağıtılan çok sayıda diğer yazılım bileşenleri ile iletişim kurmak gereken temel varsayılır. Ayrıca bulunmamalıdır SAP GUI veya diğer arabirimleri ile bağlanma son kullanıcı dağıtım formunda sunulan fark. Site-için-site/çok siteler bağlantı veya özel bağlantılar gibi Azure ExpressRoute aracılığıyla Azure sistemlere genişletilmiş DNS hizmetleri ve şirket içi Active Directory/OpenLDAP sahibiz, bu koşullar yalnızca karşılanabilir.



### <a name="scenario-of-an-sap-landscape"></a>Bir SAP ortamının senaryo

Şirket içi veya karma senaryo kabaca tanımlanabilen gibi grafik:

![Şirket içi ve Azure varlıkları arasında siteden siteye bağlantı][planning-guide-figure-2100]

Yukarıda gösterilen senaryo, bir senaryo açıklanmaktadır. burada şirket içi

Tarayıcı erişimini ya da Azure Hizmetleri için sistem erişimi için VPN tabanlı bağlantılar için SSL/TLS gibi güvenli iletişim protokolleri kullanımını en düşük gereksinimdir. Şirketler, kurumsal ağ ve Azure arasında VPN bağlantısı farklı şekilde işlemek varsayılır. Bazı şirketler, tüm bağlantı noktaları blankly açılabilir. Diğer bazı şirketler, hangi bağlantı noktalarını açın, vb. için ihtiyaç duydukları kesin olarak isteyebilirsiniz.

Aşağıdaki tabloda tipik SAP iletişim bağlantı noktaları listelenir. Temel SAP ağ geçidi bağlantı noktasını açmanız yeterlidir.

<!-- sapms is prefix of a SAP service name and not a spelling error -->

| Hizmet | Bağlantı noktası adı | Örnek `<nn`> = 01 | Varsayılan aralık (min-maks.) | Açıklama |
| --- | --- | --- | --- | --- |
| Dağıtıcı |sapdp`<nn>` bakın * |3201 |3200 - 3299 |SAP Dispatcher, SAP GUI Windows, için ve Java tarafından kullanılan |
| İleti sunucusu |sapms`<sid`> bkz ** |3600 |Ücretsiz sapms`<anySID`> |SID SAP sistem kimliği = |
| Ağ geçidi |sapgw`<nn`> bakın * |3301 |ücretsiz |SAP ağ geçidine CPIC ve RFC iletişim için kullanılan, |
| SAP yönlendirici |sapdp99 |3299 |ücretsiz |CI (Merkezi örnek) hizmet adları yalnızca yükleme sonrasında /etc/services rastgele bir değere atanamaz. |

*) nn = SAP örnek sayısı

*) SID SAP sistem kimliği =

SAP ürünleri tarafından Hizmetleri şurada bulunabilir ya da farklı SAP ürünleri için gerekli bağlantı noktaları hakkında daha ayrıntılı bilgi <https://scn.sap.com/docs/DOC-17124>.
Bu belge, belirli SAP ürününü ve senaryosunu için gerekli olan VPN cihaz ayrılmış bağlantı noktaları açmanız mümkün olması gerekir.

Diğer güvenlik önlemleri oluşturmak için böyle bir senaryoda Vm'leri dağıtma olabileceği bir [ağ güvenlik grubu] [ virtual-networks-nsg] erişim kurallarını tanımlamak için.

### <a name="dealing-with-different-virtual-machine-series"></a>Farklı sanal makine serisi uğraşmanızı

Microsoft ya da Vcpu, bellek sayısı farklı birçok daha fazla VM türleri eklenemez veya daha önemli bir donanım üzerinde çalıştığı. Bu tüm sanal makineler ile SAP desteklenir (SAP notu desteklenen bakın VM türleri [1928533]). Bu VM'lerin bazılarını farklı donanım Nesilleri üzerinde çalıştırın. Bu konak donanım Nesilleri ayrıntı düzeyi, bir Azure ölçek birimi içinde dağıtılır. Burada seçtiğiniz farklı VM türleri aynı ölçek birimi üzerinde çalıştırılamaz durumlarda ortaya çıkabilir. Bir kullanılabilirlik kümesi içinde ölçek birimleri farklı bir donanım tabanlı span olanağı sınırlıdır.  Örneğin bir HA yapılandırmasında ikincil DBMS örneği çalışan VM ile birlikte bir kullanılabilirlik kümesinde olan E64s_v3 VM'de SAP DBMS katman çalıştırıyorsanız, yalnızca durdurun ve upg için isteyebilirsiniz, çünkü M serisi VM olarak ikincil VM'yi yeniden olamaz rade VM. M serisi VM'ler ve Ev3 serisi VM'ler, ile farklı donanım açıp farklı ölçek birimi cinsinden çalıştığını nedenidir. Yeni bir kullanılabilirlik kümesi oluşturma, depolama silmeden ikincil Ev3 serisi VM ve VM M serisi VM olarak yeni kullanılabilirlik kümesi içinde dağıtmanız gerekir.

#### <a name="printing-on-a-local-network-printer-from-sap-instance-in-azure"></a>Azure'da SAP örneğinden bir yerel ağ yazıcısına yazdırma

##### <a name="printing-over-tcpip-in-cross-premises-scenario"></a>TCP/IP üzerinde içi ve dışı senaryoda yazdırma

Azure VM'de şirket içi TCP/IP bağlı ağ yazıcıları ayarlamaya genel bir VPN siteden siteye tüneli veya ExpressRoute bağlantı kuruldu olduğunu varsayarsak, şirket ağınız olduğu gibi aynıdır.

- - -
> ![Windows][Logo_Windows] Windows
>
> Bunu yapmak için:
>
> * Bazı ağ yazıcıları, Azure VM'deki yazıcı ayarlamak kolaylaştıran bir Yapılandırma Sihirbazı ile gelir. Sihirbaz yazılım yazıcıyla dağıtılmamışsa, yazıcı kurma şekilde el ile yeni bir TCP/IP yazıcı bağlantı noktasını oluşturmaktır.
> * Açık Denetim Masası -> cihazlar ve yazıcılar için Yazıcı Ekle ->
> * TCP/IP adresi veya ana bilgisayar adı kullanarak bir yazıcı Ekle'yi seçin
> * Yazıcı IP adresi yazın
> * Yazıcı bağlantı noktasını standart 9100
> * Gerekirse uygun yazıcı sürücüsü el ile yükleyin.
>
> ![Linux][Logo_Linux] Linux
>
> * Ağ yazıcısı yüklemek için standart yordam Windows yalnızca izleme istiyor
> * yalnızca genel Linux kılavuzlarını izleyin [SUSE](https://www.suse.com/documentation/sles-12/book_sle_deployment/data/sec_y2_hw_print.html) veya [Red Hat ve Oracle Linux](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/sec-Printer_Configuration.html) yazıcı ekleme.
>
>

- - -
![Ağ yazdırma][planning-guide-figure-2200]

##### <a name="host-based-printer-over-smb-shared-printer-in-cross-premises-scenario"></a>Ana bilgisayar tabanlı yazıcıya SMB (paylaşılan yazıcı) üzerinden içi ve dışı karışık senaryo

Ana bilgisayar tabanlı yazıcılar tasarım gereği ağ ile uyumlu değildir. Ancak, ağdaki bilgisayarlar arasında yazıcının açık bilgisayara bağlı olduğu sürece bir ana bilgisayar tabanlı yazıcı paylaşılabilir. Şirket bağlanmak siteden siteye veya ExpressRoute ağ ve yerel yazıcı paylaşın. SMB protokolü, DNS yerine NetBIOS adı Hizmet olarak kullanır. NetBIOS ana bilgisayar adı DNS ana bilgisayar adından farklı olabilir. NetBIOS ana bilgisayar adı ve DNS ana bilgisayar adı özdeş olduğunu standart durumdur. DNS etki alanı NetBIOS adı alanı mantıklı değildir. Buna göre DNS ana bilgisayar adı ve DNS etki alanı oluşan tam DNS ana bilgisayar adı NetBIOS adı alanı kullanılmamalıdır.

Yazıcı Paylaşımı, ağdaki benzersiz bir ad tarafından tanımlanır:

* (Her zaman gereklidir) SMB konağın konak adı.
* (Her zaman gereklidir) paylaşımının adı.
* Yazıcıyı paylaştırmak etki alanının adını SAP sistemine aynı etki alanında değilse.
* Ayrıca, bir kullanıcı adı ve parola yazıcı paylaşıma erişmek için gerekebilir.

Nasıl yapılır:

- - -
> ![Windows][Logo_Windows] Windows
>
> Yerel yazıcı paylaşın.
> Azure VM'de yazıcı paylaşımı adını yazın ve Windows Gezgini açın.
> Bir yazıcı Yükleme Sihirbazı'nı yükleme süreci boyunca size yol gösterir.
>
> ![Linux][Logo_Linux] Linux
>
> Linux'ta ağ yazıcıları yapılandırma veya bölüm de dahil olmak üzere ilgili belgeleri bazı örnekleri aşağıda verilmiştir yazdırma Linux'ta ilgili. VM'ye bir VPN parçası olduğu sürece bir Azure Linux VM ile aynı şekilde çalışır:
>
> * SLES <https://en.opensuse.org/SDB:Printing_via_SMB_(Samba)_Share_or_Windows_Share>
> * RHEL veya Oracle Linux <https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/System_Administrators_Guide/sec-Printer_Configuration.html#s1-printing-smb-printer>
>
>

- - -
##### <a name="usb-printer-printer-forwarding"></a>USB yazıcı (yazıcısı iletme)

Azure'da Uzak Masaüstü Hizmetleri kullanıcı yerel yazıcı cihazlarını uzaktan bir oturumda erişmeyi olanağı kullanılamıyor.

- - -
> ![Windows][Logo_Windows] Windows
>
> Windows yazdırma hakkında daha fazla bilgi şurada bulunabilir: <https://technet.microsoft.com/library/jj590748.aspx>.
>
>

- - -
#### <a name="integration-of-sap-azure-systems-into-correction-and-transport-system-tms-in-cross-premises"></a>SAP tümleştirme düzeltme ve şirketler arası aktarım sisteminde (TMS) Azure sistemleri

SAP değiştirme ve taşıma sistemi (TMS) aktarım isteği yatay sistemleri arasında içeri ve dışarı aktarmak üzere yapılandırılması gerekir. Oysa kalite güvencesi (kapsayan QA) ve üretken sistemlerine (PRD) şirket içi bir SAP sistemine (Geliştirme) geliştirme örneklerini Azure'da yer alan varsayıyoruz. Ayrıca, bir merkezi aktarım dizini olduğunu varsayar.

##### <a name="configuring-the-transport-domain"></a>Aktarım etki alanını yapılandırma

Belirlediğiniz aktarım etki alanı denetleyicisi olarak açıklandığı gibi sistem üzerindeki aktarım etki alanınızı yapılandırın [aktarım etki alanı denetleyicisini yapılandırmadan](https://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm). Sistem kullanıcısı TMSADM oluşturulur ve gerekli RFC hedef oluşturulur. İşlem SM59 kullanarak bu RFC bağlantıları kontrol edebilirsiniz. Ana bilgisayar adı çözümlemesi arasında aktarım etki alanınızı etkinleştirilmesi gerekir.

Nasıl yapılır:

* Senaryomuzdaki ise şirket içi QAS sistem CTS etki alanı denetleyicisi olacaktır verdik. İşlem STMS çağırın. TMS iletişim kutusu görüntülenir. Bir taşıma etki alanı yapılandırma iletişim kutusu görüntülenir. (Bir aktarım etki alanı henüz yapılandırmadıysanız yalnızca bu iletişim kutusu görüntülenir.)
* Otomatik olarak oluşturulan kullanıcıda TMSADM yetkili emin olun (SM59 ABAP bağlantı -> -> TMSADM@E61.DOMAIN_E61 -> Ayrıntılar -> Utilities(M) yetkilendirme Test ->). İşlem ilk ekranın bu SAP sistemine artık aktarım etki alanı denetleyicisi olarak burada gösterildiği gibi çalışıp çalışmadığını STMS göstermelidir:

![Etki alanı denetleyicisinde STMS işleminin ilk ekran][planning-guide-figure-2300]

#### <a name="including-sap-systems-in-the-transport-domain"></a>SAP sistemlerini aktarım etki alanında da dahil olmak üzere

Aktarım etki alanında bir SAP sistemiyle dahil olmak üzere dizisi şu şekilde görünür:

* Azure'da geliştirme sisteminde, taşıma sistemi (istemci 000) gidin ve işlem STMS çağırın. Diğer yapılandırma iletişim kutusundan seçin ve etki alanı dahil sisteminde devam edin. Etki alanı denetleyicisi hedef konak belirtin ([dahil olmak üzere SAP sistemlerini aktarım etki alanındaki](https://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0c17acc11d1899e0000e829fbbd/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm)). Sistem şimdi aktarım etki alanında dahil edilecek bekliyor.
* Güvenlik nedenleriyle, ardından isteğinizi onaylamak için etki alanı denetleyicisini geri gitmek gerekir. Sisteme genel bakış ve onaylama seçin bekleme sisteminin. Ardından isteme ve yapılandırma dağıtılır onaylayın.

Bu SAP sistemine artık aktarım etki alanındaki tüm diğer SAP sistemlerini hakkında gerekli bilgileri içerir. Aynı zamanda yeni SAP sistemine adresi verilerini diğer tüm SAP sistemlerini için gönderilir ve SAP sistemine taşıma program denetimini taşıma profilinde girilir. RFC ve etki alanının aktarım dizinine erişimi çalışıp çalışmadığını denetleyin.

Aktarım sisteminizin yapılandırmasıyla zamanki belgelerinde açıklanan şekilde devam [değiştirme ve taşıma sistemi](https://help.sap.com/saphelp_nw70ehp3/helpdata/en/48/c4300fca5d581ce10000000a42189c/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm).

Nasıl yapılır:

* Şirket içi, STMS doğru yapılandırıldığından emin olun.
* Ana bilgisayar adını taşıma etki alanı denetleyicisi sanal makinenizde Azure ve Başkan visa çözülebilir emin olun.
* Çağrı işlem STMS diğer yapılandırma -> Ekle-> Sistem-etki alanında.
* Bağlantı, şirket içi TMS sistemde onaylayın.
* Taşıma yolları, grupları ve Katmanlar her zamanki şekilde yapılandırın.

Siteden siteye bağlı şirket içi ve senaryoları, şirket içi ile Azure arasındaki gecikme süresini önemli ölçüde olmaya devam edebilir. Biz geliştirme ve test üretim sistemlerine nesnelerde taşıma sırası izleyin veya düşünme taşımalar veya destek paketleri farklı sistemlere uygulama hakkında fark bağımlı merkezi aktarım dizininin konumu Bazı sistemler, okuma veya veri merkezi aktarım dizine yazma gecikmesi karşılaşır. Durum veri merkezleri arasında önemli ölçüde uzaklığı ile farklı veri merkezleri aracılığıyla farklı sistemler nerede yayılır SAP yatay yapılandırmalarına benzerdir.

Böyle gecikme çalışma ve hızlı okuma veya yazarak aktarım dizinden çalışma sistemleri için iki STMS aktarım etki alanları ayarlayabilirsiniz (şirket içinde ve azure'da sistemlerle ve aktarım etki bağlama. Bu kavram, SAP TMS ardında yatan ilkeler açıklayan bu belgeleri gözden geçirin: <https://help.sap.com/saphelp_me60/helpdata/en/c4/6045377b52253de10000009b38f889/content.htm?frameset=/en/57/38dd924eb711d182bf0000e829fbfe/frameset.htm>.

Nasıl yapılır:

* Her bir konum (şirket içi ve Azure) aktarım etki alanı ayarlama işlem STMS kullanma <https://help.sap.com/saphelp_nw70ehp3/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm>
* Etki alanlarını etki alanı ile bağlantıyı ve iki etki alanı arasındaki bağlantı onaylayın.
  <https://help.sap.com/saphelp_nw73ehp1/helpdata/en/a3/139838280c4f18e10000009b38f8cf/content.htm>
* Bağlı sistem yapılandırmaya dağıtın.

#### <a name="rfc-traffic-between-sap-instances-located-in-azure-and-on-premises-cross-premises"></a>Azure ve şirket içi (şirket içi) bulunan SAP örnekleri arasında RFC trafiği

RFC trafiği şirket içi sistemleri arasındaki ve azure'da çalışması gerekir. Hedef sistemde doğru bir RFC bağlantısı tanımlamak gerek duyduğunuz kaynak sistemde bir bağlantı çağrı işlemi SM59 ayarlamak için. Yapılandırma standart bir RFC bağlantısı kurulumun benzerdir.

Bu şirketler arası bir senaryoda birbirleri ile iletişim kurmak için gereken çalıştırma SAP sistemlerini aynı etki alanındadır VM'lerin varsayıyoruz. Bu nedenle SAP sistemlerini arasında bir RFC bağlantısı kurulumunu kurulum adımları ve şirket içi senaryolar girişlerinde farklı değildir.

#### <a name="accessing-local-fileshares-from-sap-instances-located-in-azure-or-vice-versa"></a>Azure'da ya da tam tersi bulunan SAP örneklerinden erişen yerel fileshares

Şirket içi içinde olan dosya paylaşımlarına erişmek SAP örnekleri Azure'da yer alan gerekir. Ayrıca, Azure'da bulunan dosya paylaşımlarına erişmek şirket içi SAP örneklerini gerekir. Dosya paylaşımları etkinleştirmek için izinleri ve yerel sistem paylaşım seçeneklerini yapılandırmanız gerekir. Merkeziniz ile Azure arasında VPN veya ExpressRoute bağlantı noktalarına açtığınızdan emin olun.

## <a name="supportability"></a>Desteklenebilirlik

### <a name="6f0a47f3-a289-4090-a053-2521618a28c3"></a>Azure için SAP izleme çözümü

Etkinleştirmek için Görev açısından kritik SAP sistemlerini Azure üzerinde izleme araçları SAPOSCOL veya SAP konak Aracısı izlemeyi SAP veri alma SAP için Azure izleme uzantısı aracılığıyla Azure sanal makine hizmet ana bilgisayarı kapat. SAP tarafından taleplerini SAP uygulamalarına özel olduğundan, Microsoft olmayan genel azure'a gerekli işlevselliği uygulamak, ancak müşterilerin kendi sanal için izleme gerekli bileşenleri ve yapılandırmaları dağıtmak bırakın hata verdi. Azure'da çalışan makineler. Ancak, dağıtım ve yaşam döngüsü yönetimi izleme bileşenlerin çoğunlukla Azure tarafından otomatik olarak yapılır.

#### <a name="solution-design"></a>Çözüm tasarımı

SAP izlemeyi etkinleştirmek için geliştirilen çözüm, Azure VM aracısı ve uzantı framework mimarisini temel alır. Azure VM aracısı ve uzantı framework'ün bir VM içinde Azure VM uzantısı galerisinde kullanılabilir yazılım uygulamaların yüklenmesine izin verecek şekilde olur. Bu kavramı arkasında İlkesi fikir, bir VM'ye özel işlevler dağıtımını ve yapılandırmasını söz konusu yazılımın dağıtım zaman (durumlarda SAP için Azure izleme uzantısı gibi), izin vermektir.

'Azure VM VM'de belirli bir Azure VM uzantıları işlenmesini sağlayan Aracısı' Windows Vm'leri varsayılan olarak Azure portalında VM oluşturmayı eklenmiş olur. SUSE, Red Hat veya Oracle Linux olması durumunda, VM Aracısı zaten Azure Market görüntüsü parçasıdır. Şirket içinden azure'a bir Linux VM karşıya durumlarda VM aracısını elle yüklenmesi gerekir.

Azure'da SAP görünümler için izleme çözümünün temel yapı taşlarını benzer:

![Microsoft Azure uzantısı bileşenleri][planning-guide-figure-2400]

Blok Yukarıdaki diyagramda gösterildiği gibi SAP için izleme çözümünün bir parçası Azure VM görüntüsünü ve Azure işlemleri tarafından yönetilen bir genel olarak çoğaltılan depo Azure uzantı Galerisi barındırılır. SAP, SAP için Azure izleme uzantısı yeni sürümlerini yayımlamak için Azure işlemleriyle çalışmak için Azure uygulaması üzerinde birleşik SAP/MS ekibin sorumluluğundadır.

Yeni bir Windows sanal makine dağıttığınızda, Azure VM Aracısı, VM'de oturum otomatik olarak eklenir. İşlev bu aracının, yükleme ve yapılandırma SAP NetWeaver sistemlerinin izlenmesi için Azure Uzantıları'nın işlerini koordine etmektir. Linux VM'ler için Azure VM Aracısı zaten Azure Market işletim sistemi görüntüsü parçasıdır.

Ancak, müşteri tarafından yürütülmesi gereken bir adım yoktur. Etkinleştirme ve performans bilgileri toplama yapılandırması budur. İşlem yapılandırması ile ilgili bir PowerShell Betiği veya CLI komutuyla otomatik hale getirilmiştir. PowerShell Betiği Microsoft Azure Script Center açıklandığı indirilebilir [Dağıtım Kılavuzu][deployment-guide].

SAP için Azure izleme çözümünün Genel mimarisi şuna benzer:

![Azure için SAP NetWeaver izleme çözümü][planning-guide-figure-2500]

**Tam nasıl yapılır ve dağıtımları sırasında bu PowerShell cmdlet'lerini veya CLI komutunu kullanarak ayrıntılı adımlar için verilen yönergeleri izleyin [Dağıtım Kılavuzu][deployment-guide].**

### <a name="integration-of-azure-located-sap-instance-into-saprouter"></a>Azure tümleştirme SAProuter SAP örneğine bulunan

Azure'da çalışan SAP örnekleri de SAProuter erişilebilir olması gerekir.

![SAP yönlendirici ağ bağlantısı][planning-guide-figure-2600]

Bir SAProuter doğrudan IP bağlantısı yoksa katılımcı sistemler arasında TCP/IP iletişim sağlar. Bu, ağ düzeyinde iletişim iş ortakları arasında uçtan uca bağlantı gerekli olduğunu avantajı sağlar. SAProuter varsayılan 3299 numaralı bağlantı noktasında dinliyor.
SAP örnekleri bir SAProuter bağlanmak için bağlantı girişimi SAProuter dize ve ana bilgisayar adıyla vermeniz gerekir.

## <a name="sap-netweaver-as-java"></a>SAP NetWeaver AS Java

Şu ana kadar belgenin odak SAP NetWeaver genel alındı veya SAP NetWeaver ABAP yığın. Küçük Bu bölümde, SAP Java yığını için belirli konuları listelenir. En önemli SAP NetWeaver tabanlı özel Java uygulamalarından biridir; SAP Kurumsal portalıdır. Diğer SAP NetWeaver tabanlı uygulamalar gibi SAP NetWeaver ABAP ve Java yığınları PI SAP ve SAP çözüm Yöneticisi'ni kullanın. Bu nedenle, kesinlikle yoktur gereksinimini de SAP NetWeaver Java yığınıyla ilgili belirli yönlerini göz önünde bulundurun.

### <a name="sap-enterprise-portal"></a>SAP Kurumsal Portal

Şirketler arası senaryolarda dağıtıyorsanız kurulumu bir SAP portalı bir Azure sanal makinesi üzerinde bir şirket içi yüklemesinden farklı değildir. Şirket içi DNS yapılır olduğundan, bağlantı noktası ayarlarını ayrı ayrı örnekleri yapılandırılmış şirket içi yapılabilir. Şu ana kadar bu belgede açıklanan kısıtlamaları ve önerileri SAP Enterprise Portal veya SAP NetWeaver Java yığını gibi bir uygulama için genel olarak uygulanır.

![İfşa edilen SAP portalı][planning-guide-figure-2700]

Sanal makine konağına siteden siteye VPN tüneli veya ExpressRoute aracılığıyla şirket ağına bağlı olduğu sürece bir özel dağıtım bazı müşteriler tarafından SAP Enterprise Portal'da İnternet'e doğrudan erişimini senaryodur. Böyle bir senaryo için belirli bağlantı noktalarını açık ve güvenlik duvarı veya ağ güvenlik grubu tarafından engellenmediğinden emin olmanız gerekir. 

İlk URI http (s) portalıdır:`<Portalserver`>: nerede bağlantı noktası biçimlendirilmiş SAP tarafından belirtildiği gibi 5XX00/irj <https://help.sap.com/saphelp_nw70ehp1/helpdata/de/a2/f9d7fed2adc340ab462ae159d19509/frameset.htm>.

![Uç nokta yapılandırması][planning-guide-figure-2800]

URL ve/veya SAP Enterprise Portal'ınızın bağlantı noktaları özelleştirmek istiyorsanız, bu belgelere bakın:

* [Portal URL'yi Değiştir](https://wiki.scn.sap.com/wiki/display/EP/Change+Portal+URL)
* [Varsayılan bağlantı noktası numaralarını, Portal bağlantı noktası numaralarını değiştirmek](https://wiki.scn.sap.com/wiki/display/NWTech/Change+Default++port+numbers%2C+Portal+port+numbers)

## <a name="high-availability-ha-and-disaster-recovery-dr-for-sap-netweaver-running-on-azure-virtual-machines"></a>Yüksek kullanılabilirlik (HA) ve Azure sanal Makineler'de çalışan SAP NetWeaver için olağanüstü durum kurtarma (DR)

### <a name="definition-of-terminologies"></a>Terimler tanımı

Terim **yüksek kullanılabilirlik (HA)** genellikle BT Hizmetleri iş sürekliliği sağlayarak BT kesintileri en aza indirir teknoloji kümesi ilgili yedekli, hataya dayanıklı veya yük devretme korumalı bileşenleri içinde **aynı** veri merkezi. Bu örnekte, bir Azure bölgesi içinde.

**Olağanüstü Durum Kurtarma (DR)** BT Hizmetleri kesintiye en aza indirmek ve bunların kurtarma ayrıca ancak boyunca hedeflediği **farklı** genellikle veri merkezleri, yüzlerce kilometre uzakta bulunur. Bu örnekte genellikle aynı jeopolitik bölgede veya müşteri olarak sizin tarafınızdan belirlenen olarak farklı Azure bölgeleri arasında.

### <a name="overview-of-high-availability"></a>Yüksek kullanılabilirlik'e genel bakış

SAP iki parçalara azure'da yüksek kullanılabilirlik hakkında bir tartışma ayırabiliriz:

* **Azure altyapı yüksek kullanılabilirlik**ve SAP uygulama kullanılabilirliği artırmak için aboneliğin avantajları işlem (VM'ler), ağ, depolama vb. örneği HA için.
* **SAP uygulama yüksek kullanılabilirlik**, örneğin, yüksek kullanılabilirlik, SAP yazılım bileşenlerini:
  * SAP uygulama sunucuları
  * SAP ASCS/SCS örneği
  * Veritabanı sunucusu

ve Azure altyapı HA nasıl birleştirilebilir.

Azure'da yüksek kullanılabilirlik SAP, SAP yüksek kullanılabilirlik için bir şirket içi fiziksel veya sanal ortamda kıyasla bazı farklar vardır. Windows üzerinde sanallaştırılmış ortamlarda standart SAP yüksek kullanılabilirlik yapılandırmaları aşağıdaki incelemeyi SAP açıklar: <https://scn.sap.com/docs/DOC-44415>. Windows için mevcut gibi sapinst tümleşik SAP-yüksek kullanılabilirlik için bir yapılandırma Linux yoktur. SAP HA Linux için şirket içi bulmak daha fazla bilgi: <https://scn.sap.com/docs/DOC-8541>.

### <a name="azure-infrastructure-high-availability"></a>Azure altyapı yüksek kullanılabilirlik

Şu anda bir tek sanal makine % 99,9 SLA yoktur. Nasıl tek bir VM kullanılabilirliği gibi görünebilir hakkında bir fikir edinmek için farklı kullanılabilir Azure SLA çarpımını oluşturabilirsiniz: <https://azure.microsoft.com/support/legal/sla/>.

Aylık veya 43200 dakika, 30 gün hesaplama temelidir. Bu nedenle, %0,05 kapalı kalma süresi 21.6 dakika karşılık gelir. Her zamanki şekilde farklı Hizmetleri kullanılabilirliğini aşağıdaki şekilde çarpın:

(Kullanılabilirlik hizmet #1/100) * (kullanılabilirlik hizmet #2/100) * (kullanılabilirlik hizmet #3/100) 

Benzer:

(% 99,95/100) * (% 99,9/100) * (% 99,9/100) = 0.9975 veya bir genel kullanılabilirliği %99.75.

#### <a name="virtual-machine-vm-high-availability"></a>Sanal makine (VM) yüksek kullanılabilirlik

Azure platformu olayların sanal makinelerinizin kullanılabilirliğini etkileyebilecek iki tür vardır: planlı Bakım ve Plansız bakım.

* Planlı bakım, Microsoft tarafından genel güvenilirlik, performans ve sanal makinelerinizin çalıştığı platforma altyapısının güvenliğini artırmak için temel alınan Azure platformunda yapılan Periyodik güncelleştirmelerdir olaylardır.
* Plansız bakım olayları, donanım veya sanal makinenizin altında yatan fiziksel altyapı bir şekilde arıza yaptığında oluşur. Buna yerel ağ hataları, yerel disk hataları veya raf düzeyinde diğer hatalar dahildir. Böyle bir arıza tespit edildiğinde, Azure platformu sanal makinenizi sağlıklı bir fiziksel sunucuya barındırma sağlıksız fiziksel sunucudan sanal makinenizi otomatik olarak geçirir. Bu tür olaylar nadirdir, ancak sanal makinenizin yeniden başlatılmasına da neden olabilir.

Bu belgede daha fazla ayrıntı bulunabilir: <https://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="azure-storage-redundancy"></a>Azure depolama Yedekliliği

Microsoft Azure depolama hesabınızdaki veriler, dayanıklılık ve yüksek kullanılabilirlik, geçici donanım hataları karşılaşıldığında bile Azure depolama SLA'sı karşıladığından emin olmak için her zaman çoğaltılır.

Azure depolama verilerinizin üç görüntü varsayılan olarak engelliyor. bu yana RAID5 veya birden çok Azure disklerde RAID1 olmayan gerekli.

Bu makalede daha fazla ayrıntı bulunabilir: <https://azure.microsoft.com/documentation/articles/storage-redundancy/>

#### <a name="utilizing-azure-infrastructure-vm-restart-to-achieve-higher-availability-of-sap-applications"></a>SAP uygulama yüksek kullanılabilirlik elde etmek üzere Azure altyapı VM yeniden kullanma

Linux üzerinde Windows Server Yük Devretme Kümelemesi (WSFC) veya Pacemaker gibi işlevler kullanmamaya karar verirseniz (şu anda SLES 12 ve sonraki yalnızca desteklenir), Azure VM'yi yeniden başlatma kullanılan bir SAP sistemiyle planlanmış ve planlanmamış kesinti süreleri Azure karşı korumak için fiziksel sunucu altyapısı ve genel bir temel alınan Azure platformu.

> [!NOTE]
> Azure VM'yi yeniden başlatın. öncelikle sanal makineleri ve uygulamaları korur, bahsetmek önemlidir. VM yeniden SAP uygulamaları için yüksek kullanılabilirlik sağlamaz, ancak belirli bir düzeyde altyapı kullanılabilirliğini ve bu nedenle dolaylı olarak yüksek kullanılabilirlik, SAP sistemlerini sunar. Bir konak, planlı veya plansız kesinti sonra bir VM'yi yeniden başlatmak için gereken süreyi için SLA bulunmaktadır. Bu nedenle, bu yöntem yüksek kullanılabilirlik (A) SCS veya DBMS gibi bir SAP sistemiyle kritik bileşenleri için uygun değil.
>
>

Yüksek kullanılabilirlik için başka bir önemli altyapı öğesi depolamadır. Örneğin Azure depolama SLA'sı % 99,9 kullanılabilirlik kümesidir. Varsa, tek Azure depolama hesabına, olası Azure depolama, Azure depolama hesabına yerleştirilen tüm sanal makineler ve bu sanal makineler içinde çalışan ayrıca tüm SAP bileşenleri kullanılamama kullanılamazlık neden olur, disklerini bulunduğu tüm VM'ler dağıtır.  

Tüm sanal makineleri tek tek Azure depolama hesabına almak yerine ayrılmış depolama da kullanabilirsiniz hesapları her VM için ve bu şekilde, birden çok bağımsız Azure depolama hesaplarını kullanarak genel VM ve SAP uygulama kullanılabilirliği artırın.

Azure yönetilen diskler, hata etki alanı için bağlı sanal makinenin otomatik olarak yerleştirilir. Yerleştirirseniz, iki sanal makine bir kullanılabilirlik kümesi ve yönetilen diskler, platform farklı hata etki alanlarına da yönetilen diskleri dağıtma ölçeklendirilmesini. Premium depolama kullanmayı planlıyorsanız, diskleri yönetme de kullanarak önerilir.

Azure altyapı HA ve depolama hesaplarını kullandığı bir SAP NetWeaver sisteminin örnek bir mimarisi şöyle görünebilir:

![Azure altyapı SAP uygulama daha yüksek kullanılabilirlik elde etmek için HA faydalanma][planning-guide-figure-2900]

Azure altyapı HA ve yönetilen diskler kullanan bir SAP NetWeaver sisteminin örnek bir mimarisi şöyle görünebilir:

![Azure altyapı SAP uygulama daha yüksek kullanılabilirlik elde etmek için HA faydalanma][planning-guide-figure-2901]

Kritik SAP bileşenleri için aşağıdaki şu ana kadar alanımız:

* SAP uygulama sunucuları (AS) yüksek kullanılabilirlik

  SAP uygulama sunucusu örneklerinin yedekli bileşenlerdir. Her SAP örneği çalıştıran farklı bir Azure hata ve yükseltme etki alanı kendi VM üzerinde dağıtılmış olduğundan (bölümlere bakın [hata etki alanları] [ planning-guide-3.2.1] ve [yükseltme etki alanları] [ planning-guide-3.2.2]). Bu Azure kullanılabilirlik kümeleri kullanarak sağlamış (bkz. bölüm [Azure kullanılabilirlik kümeleri][planning-guide-3.2.3]). Olası planlanmış veya planlanmamış kullanılamazlık Azure arıza ya da yükseltme etki alanı kendi SAP AS ile sınırlı sayıda VM kullanılamama neden olacak örnekleri.

  Her SAP örneği kendi Azure depolama hesabına yerleştirilen - olası kullanım dışı kalması bir Azure depolama hesabı yalnızca bir VM kullanılamama kendi SAP AS neden olacak şekilde örneği. Ancak, bir Azure aboneliğinde Azure depolama hesapları sınırının olmadığını unutmayın. (A) SCS örneği Autostart parametresi ayarladığınızdan emin olun (A) SCS örneği otomatik olarak başlamasını VM'yi yeniden başlatma sonrası emin olmak için profil bölümde açıklanan başlangıç [Autostart kullanarak SAP örnekleri için][planning-guide-11.5].
  Bölüm ayrıca okuyun [SAP uygulama sunucuları için yüksek kullanılabilirlik] [ planning-guide-11.4.1] daha fazla ayrıntı için.

  Yönetilen diskleri kullanıyor olsanız bile, bu diskleri de bir Azure depolama hesabında depolanır ve bir depolama kesintisi olayda kullanılamıyor olabilir.

* *Daha yüksek* SAP kullanılabilirlik (A) SCS örneği

  Burada Azure VM'yi yüklü SAP (A) SCS örneği ile korumak için VM yeniden kullanın. Azure planlı veya plansız kesinti olması durumunda sunucularının, Vm'leri başka bir kullanılabilir sunucu yeniden başlatılır. Daha önce bahsedildiği gibi Azure VM'yi yeniden başlatın. öncelikle VM'ler ve bu durum (A) SCS örneği içinde olmayan uygulamaları korur. VM yeniden, SAP (A) SCS örneği dolaylı olarak daha yüksek kullanılabilirliğini ulaşacağız. (A) SCS örneği otomatik olarak başlamasını VM'yi yeniden başlatma sonrası sağlamak üzere, Autostart parametresi bölümde açıklanan (A) SCS örneği başlangıç profilinde ayarladığınızdan emin olun [Autostart kullanarak SAP örnekleri için][planning-guide-11.5]. Bir tek tek bir VM'de çalışan bir hata noktası (SPOF) olarak (A) SCS örneği bunun anlamı tüm SAP ortamı kullanılabilirliğini determinative faktörü olacaktır.

* *Daha yüksek* DBMS sunucu kullanılabilirliği

  Burada, SAP (A) SCS örneği kullanım örneğine benzer, biz yüklü DBMS yazılım ile bir sanal Makineyi korumak için Azure VM'yi yeniden başlatın kullandığından ve biz DBMS yazılım VM yeniden aracılığıyla yüksek kullanılabilirlik elde etmek.
  DBMS tek bir VM'de çalışan bir SPOF Ayrıca, ve onu tüm SAP ortamı kullanılabilirliğini determinative faktördür.

### <a name="sap-application-high-availability-on-azure-iaas"></a>Azure Iaas üzerinde SAP uygulama yüksek kullanılabilirlik

Tam SAP sistemi yüksek kullanılabilirlik elde etmek için örnek yedekli SAP uygulama sunucuları ve SAP (A) SCS örneği ve DBMS gibi benzersiz bileşenleri (örneğin tek hata noktası) için tüm kritik SAP sistem bileşenleri korumak ihtiyacımız var.

#### <a name="5d9d36f9-9058-435d-8367-5ad05f00de77"></a>SAP uygulama sunucuları için yüksek kullanılabilirlik

SAP uygulama sunucuları/iletişim örnekleri için belirli bir yüksek kullanılabilirlik çözümü hakkında düşünmek gerekli değildir. Yedeklilik ve böylece yüksek kullanılabilirlik elde yeterli bunların farklı sanal makinelere sahip. Bunların tümü aynı Azure kullanılabilirlik önlemek için kümesi'nde Vm'leri aynı anda planlı bakım kapalı kalma süresinde güncelleştirilebilir yerleştirilmelidir. Farklı yükseltme ve hata etki alanları içindeki bir Azure ölçek birimi üzerine inşa edilmiştir temel işlevleri zaten bölümde sunulan [yükseltme etki alanları][planning-guide-3.2.2]. Azure kullanılabilirlik kümeleri bölümde sunulan [Azure kullanılabilirlik kümeleri] [ planning-guide-3.2.3] bu belgenin.

Hiçbir sınırsız sayıda hata ve yükseltme Azure ölçek birimindeki Azure kullanılabilirlik kümesi tarafından kullanılan etki alanlarında yoktur. Bu, içine, bir kullanılabilirlik kümesi, er geç VM sona eriyor, aynı hata veya yükseltme etki alanında birden fazla VM sayısı koyarak anlamına gelir.

Aşağıdaki resimde, birkaç SAP uygulama sunucusu örneklerinde ayrılmış Vm'leri dağıtma ve beş yükseltme etki alanları yapılandırdığımıza varsayarak, sonunda ortaya çıkar. Bir kullanılabilirlik kümesi içinde hata ve güncelleme etki alanları en fazla gerçek sayısını gelecekte değişebilir:

![HA azure'da SAP uygulama sunucuları][planning-guide-figure-3000]

Bu belgede daha fazla ayrıntı bulunabilir: <https://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="high-availability-for-sap-central-services-on-azure"></a>Azure üzerinde SAP Central Services'in için yüksek kullanılabilirlik

Makale için yüksek kullanılabilirlik mimarisi azure'da SAP Central Services'in onay [yüksek kullanılabilirlik mimarisi ve senaryolar için SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios) olarak giriş bilgileri. Makalenin belirli işletim sistemleri için daha detaylı açıklamalar işaret eder.

#### <a name="high-availability-for-the-sap-database-instance"></a>SAP veritabanı örneği için yüksek kullanılabilirlik

Normal SAP DBMS HA Kurulumu DBMS yüksek kullanılabilirlik işlevselliği verileri active DBMS örneğinden bir pasif DBMS örneğine ikinci bir sanal makine için çoğaltmak için kullanıldığı iki DBMS VM temel alır.

Yüksek kullanılabilirlik ve olağanüstü durum kurtarma işlevleri için DBMS genel de gibi belirli DBMS açıklanmıştır [DBMS Dağıtım Kılavuzu][dbms-guide].

#### <a name="end-to-end-high-availability-for-the-complete-sap-system"></a>Uçtan uca tam SAP sistemi için yüksek kullanılabilirlik

İşte bir tam SAP NetWeaver HA mimarisi - azure'da iki örnek bir Windows ve Linux için.

Yalnızca yönetilmeyen diskler: Kavramlar aşağıda açıklandığı gibi çok sayıda SAP sistemlerini dağıtıp depolama hesaplarının üst sınırı abonelik başına dağıtılan VM'lerin sayısını aşıp biraz tehlikeye gerekebilir. Böyle durumlarda, bir depolama hesabında birleştirilecek VHD'lerin VM gerekir. Genellikle VHD'ler, SAP uygulama katmanı farklı SAP sistemlerini VM'lerin birleştirerek bunu.  Biz de farklı VHD farklı DBMS VM'lerin bir Azure depolama hesabında farklı SAP sistemlerinin birleştirilmiş. Böylece Azure depolama hesaplarının IOPS limitlerine göz önünde bulundurarak (<https://azure.microsoft.com/documentation/articles/storage-scalability-targets>)


##### <a name="windowslogowindows-ha-on-windows"></a>![Windows][Logo_Windows] HA Windows üzerinde

![Azure Iaas SQL Server ile SAP NetWeaver uygulama HA mimarisi][planning-guide-figure-3200]

Aşağıdaki Azure yapıları, altyapı sorunları etkiyi en aza indirmek ve düzeltme eki uygulama barındırma SAP NetWeaver sistem için kullanılır:

* Tam sistem (gerekli - DBMS katman, (A) SCS örneği ve tam uygulama katmanı aynı konumda çalıştırmanız gerekir) Azure üzerinde dağıtılır.
* Tam sistem (gerekli) bir Azure aboneliğinde çalıştırır.
* Tam sistem bir Azure sanal (gerekli) ağ içinde çalışır.
* Üç kullanılabilirlik kümelerine tek SAP sistemine sanal makinelerin ayrılması, aynı sanal ağa ait bile tüm sanal makineleri ile mümkündür.
* (Örneğin DBMS, ASCS, uygulama sunucuları) her katman, ayrılmış bir kullanılabilirlik kümesi kullanmanız gerekir.
* Tek SAP sistemine DBMS örneklerini çalışan tüm sanal makineler, bir kullanılabilirlik kümesi'nde ' dir. SQL Server AlwaysOn veya Oracle Data Guard gibi özellikleri kullanılır, yerel DBMS yüksek kullanılabilirlik beri sistem başına DBMS örneği çalıştıran birden fazla VM olduğunu varsayıyoruz.
* DBMS örnekleri çalışan tüm sanal makineler, kendi depolama hesabını kullanırsınız. DBMS veri ve günlük dosyaları bir depolama hesabından diğerine verileri eşitlemek DBMS yüksek kullanılabilirlik işlevleri kullanarak başka bir depolama hesabına çoğaltılır. Bir depolama hesabı kullanım dışı kalması yetersizlik tek SQL Windows Küme düğümü, ancak değil tam SQL Server hizmetinin neden olur.
* Tek SAP sistemine (A) SCS örneği çalışan tüm sanal makineler, bir kullanılabilirlik kümesi'nde ' dir. (A) korumak için bu sanal makineler içinde yapılandırılmış bir Windows Server Yük devretme kümesi (WSFC) SCS örneği.
* (A) SCS örneği çalışan tüm sanal makineler, kendi depolama hesabını kullanırsınız. (A) SCS örneği dosyaları ve SAP genel klasörü bir depolama hesabından diğerine SIOS DataKeeper çoğaltmayı kullanarak başka bir depolama hesabına çoğaltılır. Bir depolama hesabı kullanım dışı kalması kullanılamazlık birinin neden olur (A) SCS Windows Küme düğümü, ancak tam (A) SCS hizmeti.
* SAP uygulama sunucusu katmanı temsil eden tüm VM'lerin kullanılabilirlik üçüncü ayarlanır.
* SAP uygulama sunucuları çalışan tüm sanal makineler, kendi depolama hesabını kullanırsınız. Bir depolama hesabı kullanım dışı kalması burada diğer SAP uygulama sunucuları çalışmaya devam eder, bir SAP uygulama sunucusu olarak kullanım dışı kalması neden olur.

Aşağıdaki şekilde yönetilen Diskler'i kullanarak aynı yatay gösterilmektedir.

![Azure Iaas SQL Server ile SAP NetWeaver uygulama HA mimarisi][planning-guide-figure-3201]

##### <a name="linuxlogolinux-ha-on-linux"></a>![Linux][Logo_Linux] HA Linux'ta

Mimari için SAP HA azure'da Linux üzerinde temel Windows yukarıda açıklanan şekilde aynıdır. SAP notuna bakın [1928533] desteklenen yüksek kullanılabilirlik çözümleri listesi.

### <a name="4e165b58-74ca-474f-a7f4-5e695a93204f"></a>SAP örnekleri için AutoStart'ı kullanma

SAP, SAP örnekleri başlatılmasından VM içindeki işletim sistemi hemen sonra başlatmak için işlevselliğin sunduğu. Uygulanacak adımlar SAP Bilgi Bankası makalesi belgelenen [1909114]. Bununla birlikte, SAP ayarını kullanacak şekilde önerme değil artık olmadığı için örneği yeniden başlatma sırasına göre bir denetim yok birden fazla VM varsayıldığında etkilenen veya VM başına birden çok örneği çalıştı. Bir VM ve sonunda yeniden tek bir VM durumunun bir SAP uygulama sunucusu örneği, Azure tipik bir senaryo varsayıldığında, Autostart kritik değildir ve bu parametre ekleyerek etkinleştirilebilir:

    Autostart = 1

SAP ABAP ve/veya Java örneğinin başlangıç profiline.

> [!NOTE]
> Autostart parametresi bazı downfalls de sahip olabilir. İlgili Windows/Linux hizmet örneği başlatıldığında daha ayrıntılı olarak parametresi bir SAP ABAP veya Java örnek tetiklenir. İşletim sistemi önyükleme yaptığında, kesinlikle durum geçerlidir. Bununla birlikte, SAP hizmetleri yeniden toplamı gibi SAP yazılım yaşam döngüsü yönetimi işlevleri için ortak bir şey de olan veya diğer güncelleştirmeleri veya yükseltmeleri. Bu işlevler, hiç otomatik olarak yeniden başlatılmasını örneği beklenmiyor. Bu nedenle, Autostart parametresi gibi görevleri çalıştırmadan önce devre dışı bırakılmalıdır. Autostart parametresi gibi ASCS/SCS/CI kümelenir SAP örnekleri için de kullanılmamalıdır.
>
>

SAP için autostart'ile ilgili ek bilgiler burada örnekler bakın:

* [Başlat/Durdur SAP birlikte, UNIX sunucusu başlatma/durdurma](https://scn.sap.com/community/unix/blog/2012/08/07/startstop-sap-along-with-your-unix-server-startstop)
* [Başlatma ve durdurma SAP NetWeaver yönetim aracıları](https://help.sap.com/saphelp_nwpi711/helpdata/en/49/9a15525b20423ee10000000a421938/content.htm)
* [Otomatik Başlat, HANA veritabanı etkinleştirme](http://www.freehanatutorials.com/2012/10/how-to-enable-auto-start-of-hana.html)

### <a name="larger-3-tier-sap-systems"></a>3 katmanlı SAP sistemlerini daha büyük
3 katmanlı SAP yapılandırmalarına yönlerini yüksek kullanılabilirlik, önceki bölümlerde zaten ele. Ancak DBMS sunucu gereksinimleri Azure, ancak SAP uygulama katmanı bulunan için çok büyük olduğu sistemler Azure'a dağıtılabilir nedir?

#### <a name="location-of-3-tier-sap-configurations"></a>3 katmanlı SAP yapılandırmalarına konumu
Uygulama katmanı bölmek için desteklenmeyen kendisi veya uygulama ve DBMS şirket içi ile Azure arasında katman. Bir SAP sistemidir ya da tamamen şirket içinde dağıtılabilir veya Azure. Bu ayrıca Azure'da çalıştırmak, şirket içinde ve bazıları uygulama sunucuları bazıları için desteklenmiyor. Bu tartışma başlangıç noktasıdır. Biz de DBMS bileşenleri bir SAP sistemiyle ve iki farklı Azure bölgelerinde dağıtılan SAP uygulama sunucusu katmanı için desteklemiyor. Örneğin, Orta ABD, Batı ABD ve SAP uygulama katmanında DBMS. Bu tür yapılandırmaları desteklemediğinden için SAP NetWeaver mimarisi gecikme duyarlılığına nedenidir.

Ancak, kursunda geçen yıl veri merkezi iş ortaklarının geliştirilmiş ortak konumlarını Azure bölgeleri için. Bu ortak genellikle yakın fiziksel Azure veri merkezleri bir Azure bölgesi içinde konumlardır. Azure ExpressRoute aracılığıyla ortak konuma varlıkları bağlantısı ve kısa bir mesafe 2ms'den küçük olan bir gecikme süresine neden olabilir. Bu gibi durumlarda, ortak bir konum ve SAP DBMS katman (depolama SAN/NAS dahil) bulmak için Azure uygulama katmanında mümkündür. [HANA büyük örnekleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture). 

### <a name="offline-backup-of-sap-systems"></a>Çevrimdışı Yedekleme, SAP sistemlerini
Yedekleme yapmanız (Katman 2 veya 3 katman), burada seçilen SAP yapılandırmasına bağlı olabilir. Veritabanının bir yedeklemesini sağlamak için sanal Makinenin kendisini artı içeriği. DBMS ile ilgili yedekleme veritabanı yöntemleriyle gerçekleştirilmesi beklenir. Ayrıntılı bir açıklaması için farklı veritabanlarını bulunabilir [DBMS Kılavuzu][dbms-guide]. Öte yandan, SAP veri (veritabanı içeriği de dahil) çevrimdışı bir şekilde bu bölümde açıklandığı gibi çevrimiçi veya sonraki bölümde açıklandığı gibi yedeklenebilir.

Çevrimdışı Yedekleme Azure portalından bir VM'nin bir kapatma ve temel VM diskine ve tüm bağlı diskleri olan VM için bir kopyasını temelde gerektirir. Bu görüntü zaman içinde nokta VM ve onun ilişkili disk korumak. Yedekleri farklı bir Azure depolama hesabına kopyalamak için önerilir. Bu bölümde açıklanan yordamı [diskleri Azure depolama hesapları arasında kopyalama] [ planning-guide-5.4.2] bu belgenin uygular.
Kapatma yanı sıra Azure portalı birini kullanarak da Powershell veya CLI burada açıklandığı bunu yapabilirsiniz: <https://azure.microsoft.com/documentation/articles/virtual-machines-deploy-rmtemplates-powershell/>

Bu durumu bir geri yükleme temel sanal makinenin özgün diskleri yanı sıra temel VM silme oluşur ve diskler, kaydedilmiş diskler yönetilen disklerin özgün depolama hesabı veya kaynak grubu geri kopyalamak ve ardından sistemi yeniden dağıtmaya gerek bağlı.
Bu makalede, bu işlemde bir Powershell betiği oluşturmak nasıl bir örnek gösterilmektedir: <http://www.westerndevs.com/azure-snapshots/>

Lütfen yukarıda açıklandığı gibi bir VM yedek geri yeni bir donanım anahtarı oluşturduğundan, yeni bir SAP lisansı yüklediğinizden emin olun.

### <a name="online-backup-of-an-sap-system"></a>Bir SAP sistemiyle, çevrimiçi yedekleme

DBMS yedeğini açıklandığı DBMS özgü yöntemleriyle gerçekleştirilen [DBMS Kılavuzu][dbms-guide].

Azure sanal makine yedekleme işlevini kullanarak SAP sistemine içindeki diğer Vm'lere yedeklenebilir. Azure sanal makine yedekleme, azure'daki tüm VM'yi yedeklemek için standart bir yöntemdir. Azure Backup, Azure'da yedeklemeleri depolayan ve VM'nin bir geri yeniden sağlar.

> [!NOTE]
> Aralık 2015'ten itibaren VM yedeklemesi kullanarak SAP için kullanılan benzersiz VM kimliği korumaz lisanslama. Başka bir deyişle, geri yüklenen VM'ye yeni bir sanal makine ve yenisini kaydedilmiş olan eski biri değil olarak kabul edilir gibi VM yedekten'ın yeni bir SAP lisans anahtarı yüklenmesini gerektirir.
>
> ![Windows][Logo_Windows] Windows
>
> Teorik olarak, Windows VSS DBMS sistemi destekliyorsa, tutarlı bir şekilde de çalışma veritabanları yedeklenebilecek VM'ler (birim gölge kopyası hizmeti <https://msdn.microsoft.com/library/windows/desktop/bb968832(v=vs.85).aspx>), örneğin, SQL Server yapar.
> Ancak, veritabanını zaman içinde nokta geri yükler, Azure VM yedeklemeleri üzerinde alan mümkün olmayan unutmayın. Bu nedenle, Azure VM yedekleme kalmak yerine DBMS işlevselliği olan veritabanlarının yedeklerini gerçekleştirmek için kullanılması önerilir.
>
> Burada Azure sanal makine yedekleme başlama hakkında bilgi edinmek için: <https://docs.microsoft.com/azure/backup/backup-azure-vms>.
>
> Diğer olasılıklar, Microsoft veri koruma bir Azure VM ve Azure Backup, yedekleme/geri yükleme veritabanları için yüklü Yöneticisi'nin bir birleşimini kullanmak üzeresiniz. Daha fazla bilgi burada bulunabilir: <https://docs.microsoft.com/azure/backup/backup-azure-dpm-introduction>.  
>
> ![Linux][Logo_Linux] Linux
>
> Linux'ta Windows VSS eşdeğeri yoktur. Bu nedenle yalnızca dosyayla tutarlı yedekleme olası ancak değil uygulamayla tutarlı yedeklemeler altındadır. SAP DBMS yedekleme yapılmalıdır DBMS işlevini kullanarak. SAP ile ilgili veriler içeren bir dosya sistemi, örneğin, burada açıklandığı gibi hedefi kullanarak kaydedilebilir: <https://help.sap.com/saphelp_nw70ehp2/helpdata/en/d3/c0da3ccbb04d35b186041ba6ac301f/content.htm>
>
>

### <a name="azure-as-dr-site-for-production-sap-landscapes"></a>Üretim SAP ortamlarını için DR site olarak azure'a

Mid 2014'ten itibaren extensions çeşitli bileşenleri Hyper-V, System Center ve Azure ile şirket içi Hyper-V tabanlı çalıştıran VM'ler için DR sitesi olarak Azure kullanımını etkinleştirin.

Bu çözümün nasıl dağıtılacağı gerçekleşen blog burada belgelenmektedir: <https://blogs.msdn.com/b/saponsqlserver/archive/2014/11/19/protecting-sap-solutions-with-azure-site-recovery.aspx>.

## <a name="summary"></a>Özet

Yüksek kullanılabilirlik için azure'da SAP sistemlerini önemli noktaları şunlardır:

* Bu anda, şirket içi dağıtımlarında yapılabilir gibi SAP tek hata noktası tamamen aynı şekilde korunamıyor. Küme henüz Azure'da 3. taraf yazılım kullanmadan oluşturulamıyor, paylaşılan Disk nedenidir.
* DBMS katmanı için paylaşılan disk küme teknolojisine bağımlı kalmayacak DBMS işlevi kullanmanız gerekir. Ayrıntılar bölümünde belgelenmiştir [DBMS Kılavuzu][dbms-guide].
* Hata etki alanları içindeki sorunları Azure altyapı veya ana bilgisayar bakım etkisini en aza indirmek için Azure kullanılabilirlik kümeleri kullanmanız gerekir:
  * SAP uygulama katmanı için bir kullanılabilirlik kümesi olması önerilir.
  * Kullanılabilirlik ayrı bir kümesi için SAP DBMS katmanı sağlamak için önerilir.
  * Aynı kullanılabilirlik kümesindeki VM'ler, farklı SAP sistemlerinin için uygulamak için önerilmez.
  * Premium yönetilen diskleri kullanmanız önerilir.
* SAP DBMS katman yedekleme amacıyla denetleyin [DBMS Kılavuzu][dbms-guide].
* Genellikle daha hızlı basit iletişim örnekleri yeniden dağıtmak için olduğundan SAP iletişim örneklerinizi yedeklemek mantıklı.
* Tüm profiller farklı örnekleri, genel bir dizin SAP sistemine ve onunla içeren VM yedekleme mantıklı ve Windows Yedekleme ile gerçekleştirilmesi gereken veya örneğin, Linux üzerinde he. Windows Server 2008 (R2) ile Windows Server 2012 (R2) arasındaki farklar olduğundan, daha yeni Windows Server'ı kullanarak yedekleme kolaylaştırmak sürümleri, Windows konuk işletim sistemi olarak Windows Server 2012 (R2) çalıştırmanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar
Makaleyi okuyun:

- [Azure sanal makineler dağıtım için SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide)
- [SAP iş yükü Azure sanal makineleri DBMS dağıtım konuları](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)
- [SAP HANA altyapısı yapılandırmaları ve işlemleri Azure üzerinde] (https://docs.microsoft.com/
- Azure/sanal-makineler/iş yüklerini/sap/hana-vm-işlemleri)
