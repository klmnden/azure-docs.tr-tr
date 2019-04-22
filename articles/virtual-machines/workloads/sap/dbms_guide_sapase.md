---
title: SAP iş yükü için SAP ASE Azure sanal makineleri DBMS dağıtım | Microsoft Docs
description: SAP iş yükü için SAP ASE Azure Sanal Makineler DBMS dağıtımı
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/1/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3f50f013020c704ddc294a59f8c6c5dac24bbd5a
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58850959"
---
# <a name="sap-ase-azure-virtual-machines-dbms-deployment-for-sap-workload"></a>SAP iş yükü için SAP ASE Azure Sanal Makineler DBMS dağıtımı

[767598]:https://launchpad.support.sap.com/#/notes/767598
[773830]:https://launchpad.support.sap.com/#/notes/773830
[826037]:https://launchpad.support.sap.com/#/notes/826037
[965908]:https://launchpad.support.sap.com/#/notes/965908
[1031096]:https://launchpad.support.sap.com/#/notes/1031096
[1114181]:https://launchpad.support.sap.com/#/notes/1114181
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
[2171857]:https://launchpad.support.sap.com/#/notes/2171857
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
[dbms-guide-managed-disks]:dbms-guide.md#f42c6cb5-d563-484d-9667-b07ae51bce29

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
[storage-powershell-guide-full-copy-vhd]:../../../storage/common/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../../windows/disks-types.md
[storage-redundancy]:../../../storage/common/storage-redundancy.md
[storage-scalability-targets]:../../../storage/common/storage-scalability-targets.md
[storage-use-azcopy]:../../../storage/common/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:../../linux/attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:../../../resource-manager-deployment-model.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md 
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md 
[virtual-machines-linux-agent-user-guide]:../../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:../../linux/capture-image.md#step-2-capture-the-vm
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability-linux]:../../linux/manage-availability.md
[virtual-machines-manage-availability-windows]:../../windows/manage-availability.md
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
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/resources/templates/sql-server-2014-alwayson-existing-vnet-and-ad/
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



Bu belgede, SAP ASE Azure Iaas dağıtırken göz önüne almanız gereken birkaç farklı alanları kapsar. Bu belge için bir önkoşul belge okuma [SAP iş yükü Azure sanal makineleri DBMS dağıtım konuları](dbms_guide_general.md) ve diğer kılavuzların [SAP iş yükü Azure Belgeleri'nde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started). 

## <a name="specifics-to-sap-ase-on-windows"></a>Windows üzerinde SAP ASE için özellikleri
Microsoft Azure ile başlayarak, mevcut SAP ASE uygulamalarınızı Azure sanal makinelerine geçiş yapabilirsiniz. SAP ASE bir Azure sanal Makinesi'nde kolayca bu uygulamalar Microsoft azure'a geçirerek toplam sahip olma maliyetini dağıtım, yönetim ve Bakım Kurumsal avantajlarına uygulama azaltmanızı sağlar. Bir Azure sanal Makinesi'nde SAP ASE ile yöneticiler ve geliştiriciler aynı geliştirme ve şirket içi kullanılabilir olan yönetim araçlarını kullanmaya devam edebilirsiniz.

Azure sanal makineler için SLA şurada bulunabilir: <https://azure.microsoft.com/support/legal/sla/virtual-machines>

Microsoft Azure, binlerce kullanıcıya küçük SAP sistemlerini ve en büyük SAP sistemlerini çok ortamlarını ve ortamlarını çalıştırmanıza olanak tanıyan çok sayıda farklı sanal makine türlerini sunar. SAP sayıda farklı SAP boyutlandırma SAP sertifikalı VM SKU'ları SAP Not sağlanan [1928533].

Deyimleri ve öneriler için Azure depolama, SAP VM dağıtımı veya SAP yapılan izleme kullanımı ile ilgili [SAP iş yükü Azure sanal makineleri DBMS dağıtım konuları](dbms_guide_general.md) SAP ASE dağıtımları için de geçerlidir.

### <a name="sap-ase-version-support"></a>SAP ASE sürüm desteği
Şu anda desteklediği SAP ASE sürüm 16,0 SAP Business Suite ürünlerle kullanmak üzere SAP. Tüm güncelleştirmeleri SAP ASE sunucusu veya SAP Business Suite ürünleri ile kullanılacak JDBC ve ODBC sürücüleri için SAP Service Marketplace yalnızca aracılığıyla sağlanır: <https://support.sap.com/swdc>.

JDBC ve ODBC sürücüleri veya SAP ASE sunucunun güncelleştirmeleri doğrudan Sybase sitelerinden yüklemeyin. Düzeltme ekleri hakkında ayrıntılı bilgi için SAP ürünleri şirket içi kullanım için desteklenir ve Azure sanal Makineler'de aşağıdaki SAP notları görebilirsiniz:

* [1590719]
* [1973241]

SAP Business Suite, SAP ASE üzerinde çalıştırma hakkında genel bilgiler bulunabilir [SCN](https://www.sap.com/community/topic/ase.html)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Azure vm'lerde SAP ilgili SAP ASE yüklemeleri için SAP ASE yapılandırma yönergeleri
#### <a name="structure-of-the-sap-ase-deployment"></a>SAP ASE dağıtım yapısı
SAP ASE yürütülebilir dosyaları bulunan veya yüklü sanal makinenin işletim sistemi diski sistem sürücüsüne (c: sürücüsü\). Genellikle, SAP ASE sisteminizi ve araçlarınızı veritabanlarının en yüksek iş yükü yaşamadığınızdan. Bu nedenle (master, model, saptools, sybmgmtdb, sybsystemdb) sistem ve araçları veritabanları C:\ sürücüde kalır. 

Bir özel durum, daha yüksek veri hacmine veya g/ç işlemleri toplu gerektirebilir durumunda bazı SAP ERP ve tüm BW iş yüklerini geçici veritabanı olabilir. Birim veya sanal makinenin işletim sistemi diski tarafından sağlanan IOPS (C: sürücüsü\).

Bağımlı SAPInst/SWPM yüklemek için kullanılan sürümü, SAP ASE örnek yapılandırması gibi görünebilir:

* SAP ASE yüklenirken oluşturulan tek bir SAP ASE tempdb
* SAP ASE ve SAP yükleme yordamı tarafından oluşturulan bir ek saptempdb yükleyerek oluşturulmuş bir SAP ASE tempdb
* SAP ASE ve el ile oluşturulmuş bir ek tempdb yükleyerek oluşturulmuş bir SAP ASE tempdb (örneğin'lu SAP notuna aşağıdaki [1752266]) ERP/BW belirli tempdb gereksinimlerini karşılamak için

Performansla ilgili nedenlerden dolayı belirli ERP veya tüm BW iş yükleri dışında ayrıca oluşturulan tempdb tempdb cihazları C:\ dışında bir sürücüde depolamak için anlamlı olabilir. Hiçbir ek tempdb varsa oluşturmanız önerilir (SAP notu [1752266]).

Bu tür sistemler için ayrıca oluşturulan tempdb için aşağıdaki adımları yürütülmelidir:

* SAP veritabanının ilk cihaza ilk tempdb cihaz Taşı
* Her bir cihaz SAP veritabanının içeren VHD'ler için tempdb cihaz Ekle

Bu yapılandırma etkinleştirir tempdb sistem sürücüsünün sunabileceğinden daha fazla alan kullanır. Bir başvuru olarak bir şirket içi mevcut sistemler, tempdb cihaz boyutları kontrol edebilirsiniz. Ya da bu tür bir yapılandırma sistem sürücüsü ile sağlanan tempdb karşı IOPS sayılarında etkinleştirir. Şirket içinde çalışan sistemler, tempdb karşı g/ç iş yükünü izlemek için kullanılabilir.

Hiçbir zaman herhangi bir SAP ASE cihaza VM D:\ sürücüsüne yerleştirin. Tempdb tutulan nesneleri yalnızca geçici olsa bile, SAP ASE için bu önerileri tempdb için de geçerlidir.

Veri ve işlem günlük dosyası dağıtımlar, ifadeler ve yapılan öneriler için [SAP iş yükü Azure sanal makineleri DBMS dağıtım konuları](dbms_guide_general.md). Windows tabanlı dağıtımlar durumda Windows depolama alanları kullanımını eşlikli yeterli IOPS, üretilen iş ve birim oluşturmak için kullanılması önerilir.  

#### <a name="impact-of-database-compression"></a>Veritabanı sıkıştırması etkisini
Burada, g/ç bant genişliği sınırlayıcı bir etmen haline gelebilir yapılandırmalarında, IOPS azaltır her ölçü bir Azure gibi bir Iaas senaryosu çalıştırabilirsiniz iş yükü esnetme artırmaya yardımcı olabilir. Bu nedenle, SAP ASE sıkıştırma mevcut SAP veritabanını Azure'a yüklemeden önce kullanıldığından emin olmak için önerilir.

Azure'a karşıya yüklemeden önce sıkıştırma uygulamak için öneri dışında çeşitli nedenleri verilmiştir:

* Azure'a karşıya yüklenecek veri miktarını düşük
* Daha fazla CPU veya daha yüksek g/ç bant genişliği veya g/ç gecikme şirket içi daha az daha güçlü donanım kullanabilirsiniz varsayarak sıkıştırma yürütme süresi kısadır
* Küçük veritabanı boyutları için disk ayırmayı daha az maliyetlerine neden olabilir

Veri ve LOB sıkıştırma, şirket içi yaptığı gibi Azure sanal Makineler'de barındırılan bir VM içinde çalışır. Varolan bir SAP ASE veritabanında kullanım sıkıştırma zaten kullanımda olup olmadığını denetlemek nasıl daha fazla bilgi için ' lu SAP notuna denetleyin [1750510].

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Veritabanı örnekleri izlemek için DBACockpit kullanma
SAP ASE veritabanı platform kullanan, SAP sistemlerini için DBACockpit işlem DBACockpit katıştırılmış tarayıcı pencerelerini veya Webdynpro olarak erişilebilir durumdadır. Ancak veritabanı yönetme ve izleme için tam işlevsellik yalnızca DBACockpit Webdynpro uygulamasında kullanılabilir.

Olarak şirket içi sistemler ile birkaç adım DBACockpit Webdynpro uygulaması tarafından kullanılan tüm SAP NetWeaver işlevselliğini etkinleştirmek için gereklidir. SAP notu izleyin [1245200] webdynpros kullanımını etkinleştirin ve gerekli değerler oluşturmak için. Yukarıdaki Notları'ndaki yönergeleri takip ederken, ayrıca Internet iletişimi Yöneticisi'ni (ICM) http ve https bağlantıları için kullanılacak bağlantı noktaları ile birlikte yapılandırırsınız. Varsayılan ayarı http şuna benzer:

> ICM/server_port_0 değerler bağlantı noktası = HTTP bağlantı noktası = 8000 PROCTIMEOUT = zaman AŞIMI 600 = 600 =
> 
> ICM/server_port_1 değerler bağlantı noktası = HTTPS, bağlantı noktası = = 443$ $, PROCTIMEOUT 600, zaman AŞIMI = 600 =
> 
> 

ve işlem DBACockpit benzer oluşturulan bağlantıları:

> https:\//\<fullyqualifiedhostname>:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http:\//\<fullyqualifiedhostname >: sap/8000/bc/sap/webdynpro/dba_cockpit
> 
> 

SAP sistemi barındıran Azure sanal makine için AD nasıl bağlandığını bağlı olarak ve bir DNS ICM burada açmakta gelen DBACockpit makinede çözülebilir tam bir konak adı kullandığınızdan emin olun. ' Lu SAP notuna bakın [773830] ICM profili parametrelerine göre tam ana bilgisayar adını nasıl belirlediğini anlamak ve gerekirse parametresi ICM/host_name_full açıkça ayarlayın.

Şirket içi ile Azure arasında şirketler arası bağlantı olmadan yalnızca bulut senaryosunda VM dağıttıysanız, genel bir IP adresi ve bir domainlabel tanımlamanız gerekir. Genel DNS adı VM'nin biçimi şu şekilde görünür:

> `<custom domainlabel`>.`<azure region`>.cloudapp.azure.com
> 
> 

DNS adına ilgili daha fazla ayrıntı bulunabilir [burada][virtual-machines-azurerm-versus-azuresm].

Azure VM bağlantısı için DNS adı SAP profili parametresi ICM/host_name_full ayarlama şuna benzeyebilir:

> https:\//mydomainlabel.westeurope.cloudapp.net:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http:\//mydomainlabel.westeurope.cloudapp.net:8000/sap/bc/webdynpro/sap/dba_cockpit

Bu durumda için emin olmanız gerekir:

* Azure portalında ICM ile iletişim kurmak için kullanılan TCP/IP bağlantı noktaları için ağ güvenlik grubu için gelen kuralları ekleme
* ICM ile iletişim kurmak için kullanılan TCP/IP bağlantı noktaları için Windows Güvenlik duvarı yapılandırması için gelen kuralları ekleme

Otomatikleştirilmiş için kullanılabilir tüm düzeltmeleri alınan, düzenli aralıklarla SAP notu SAP sürümünüz için geçerli düzeltme koleksiyonu uygulamak için önerilir:

* [1558958]
* [1619967]
* [1882376]

Aşağıdaki SAP notları SAP ASE için DBA Cockpit hakkında daha fazla bilgi bulunabilir:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496]    
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>SAP ASE için yedekleme/kurtarma konuları
SAP ASE Azure'a dağıtırken, yedekleme yönteminize gözden geçirilmesi gerekir. Üretim dışı sistemlere için bile, SAP veritabanlarının düzenli aralıklarla yedeklenmelidir. Azure depolama üç görüntü tutar beri bir yedekleme için bir depolama kilitlenme telafi açısından daha az önemli olabilir. Uygun bir yedekleme ve geri yükleme planı sürdürmek için birincil nedeni, belirli bir noktadaki zaman kurtarma özellikleri sağlayarak mantıksal/el ile hataları dengeleyebilir daha fazla olmasıdır. Bu nedenle yedeklemelerini geri belirli bir noktaya veritabanını geri veya var olan veritabanı kopyalayarak başka bir sistem kaynağını oluşturmak için Azure'da yedeklemelerini kullanın ya da kullanın olmaktır. 

Şirket içi yedekleme ve azure'da bir veritabanı geri yükleme aynı şekilde çalışır. SAP notlara bakın:

* [1588316]
* [1585981]

Döküm yapılandırmaları oluşturma ve zamanlama yedekleme hakkında daha fazla bilgi için. Stratejisi ve yapılandırabileceğiniz gereksinimlerine bağlı olarak veritabanı ve günlük var olan disklerden birini açın disk dökümlerini veya yedekleme için ek bir disk ekleyin. Veri kaybı bir hata durumunda olma tehlikesi azaltmak için hiçbir veritabanı dosyalarının bulunduğu bir disk kullanmak için önerilir.

Veri ve LOB sıkıştırma yanı sıra, SAP ASE yedekleme sıkıştırma de sunar. Daha az alan veritabanı ve günlük dökümleri ile kullanmak için bir yedekleme sıkıştırma kullanmak için önerilir. Daha fazla bilgi için bkz. Not SAP [1588316]. Yedekleme sıkıştırma ayrıca yedekleri ya da şirket içi Azure sanal makinenin yedekleme dökümleri içeren VHD'ler indirmeyi planlıyorsanız aktarılan veri miktarını azaltmak çok önemlidir.

Sürücü D:\ döküm hedef veritabanı veya günlük olarak kullanmayın.

#### <a name="performance-considerations-for-backupsrestores"></a>Yedekleme/geri yükleme işlemleri için performans konuları
Çıplak metal dağıtımlarındaki yedekleme/geri yükleme performansı, paralel olarak kaç birimleri okunabilir ve ne bu birimlerin verimi olabilir bağlıdır. Yedekleme sıkıştırma CPU kaynaklarını tüketir göz önünde bulundurun. Bu CPU tüketimi yedekleme sıkıştırma, önemli bir rol ile CPU iş parçacıklarını az sayıda vm'lerde oynatabilirsiniz. Bu nedenle, kabul edilebilir:

* Daha az veritabanı cihazları depolamak için kullanılan disk sayısını daha küçük genel üretilen işi okuma
* Daha küçük CPU sayısını, VM, daha ciddi yedekleme sıkıştırma etkisini iş parçacıkları
* Yedekleme için daha düşük aktarım hızı yazmak için daha az sayıda hedefle (Şerit dizinleri, diskler)

Kullanılan/birleşik, gereksinimlerinize bağlı olarak iki seçenek yazılacak hedefleri sayısını artırmak için:

* Şeritli birim IOPS aktarım hızını iyileştirmek için birden fazla bağlı diskler üzerinde Yedekleme hedef birimin bölümlemesi
* Döküm yapılandırma düzeyinde SAP ASE oluşturma, birden fazla hedef dizin dökümünü almak için yazılacak kullanır

Bir disk birimi üzerinde birden fazla bağlı disk bölümlemesi ele alınan [SAP iş yükü Azure sanal makineleri DBMS dağıtım konuları](dbms_guide_general.md). Birden çok dizin SAP ASE döküm yapılandırmasında kullanma ile ilgili daha fazla bilgi için döküm yapılandırmasını oluşturmak için kullanılan saklı yordam sp_config_dump belgelerine bakın [Sybase Bilgi Merkezi](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Azure sanal makineler ile olağanüstü durum kurtarma
#### <a name="data-replication-with-sap-sybase-replication-server"></a>Veri çoğaltma ile SAP Sybase çoğaltma sunucusu
SAP Sybase çoğaltma sunucusuna (SRS ile), SAP ASE veritabanı işlemleri zaman uyumsuz olarak uzak bir konuma aktarmak için normal bir bekleme çözümü sağlar. 

Yükleme ve SRS işleyişini çalışır de işlevsel olarak şirket içi yaptığı gibi Azure sanal makine hizmetlerinde barındırılan bir VM.

SAP ASE HADR bir Azure iç yük dengeleyici gerektirmez ve işletim sistemi düzeyinde kümeleme üzerinde bağımlılık yok. Azure Windows ve Linux Vm'leri üzerinde çalışır. SAP ASE HADR üzerinde ayrıntılarını okumak için [SAP ASE HADR Kullanıcı Kılavuzu](https://help.sap.com/viewer/efe56ad3cad0467d837c8ff1ac6ba75c/16.0.3.3/en-US/a6645e28bc2b1014b54b8815a64b87ba.html).

## <a name="specifics-to-sap-ase-on-linux"></a>Linux'ta SAP ASE için özellikleri
Microsoft Azure ile başlayarak, mevcut Azure sanal makinelerinde SAP ASE uygulamalarınızı kolayca geçirebilirsiniz. Bir sanal makinede SAP ASE kolayca bu uygulamalar Microsoft azure'a geçirerek toplam sahip olma maliyetini dağıtım, yönetim ve Bakım Kurumsal avantajlarına uygulama azaltmanızı sağlar. Bir Azure sanal Makinesi'nde SAP ASE ile yöneticiler ve geliştiriciler aynı geliştirme ve şirket içi kullanılabilir olan yönetim araçlarını kullanmaya devam edebilirsiniz.

Azure Vm'leri dağıtmak için burada bulunan resmi SLA'lar bilmeniz önemlidir: <https://azure.microsoft.com/support/legal/sla>

Boyut bilgisini SAP ve SAP sertifikalı sanal makine SKU'ları listesi SAP notu sağlanan [1928533]. Azure sanal makineleri burada bulunabilir belgeleri boyutlandırma ek SAP <https://blogs.msdn.com/b/saponsqlserver/archive/2015/06/19/how-to-size-sap-systems-running-on-azure-vms.aspx> ve burada <https://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

Deyimleri ve öneriler kullanımı ile ilgili Azure depolama, SAP VM dağıtımı veya SAP izleme SAP uygulamaları ile birlikte, SAP ASE dağıtımları için bu belgenin ilk dört bölüm belirtildiği gibi uygulayın.

Aşağıdaki iki SAP notları bulutta Linux ve ASE ASE hakkında genel bilgiler şunları içerir:

* [2134316]
* [1941500]

### <a name="sap-ase-version-support"></a>SAP ASE sürüm desteği
Şu anda desteklediği SAP ASE sürüm 16,0 SAP Business Suite ürünlerle kullanmak üzere SAP. Tüm güncelleştirmeleri SAP ASE sunucusu veya SAP Business Suite ürünleri ile kullanılacak JDBC ve ODBC sürücüleri için SAP Service Marketplace yalnızca aracılığıyla sağlanır: <https://support.sap.com/swdc>.

Yüklemeleri şirket içinde olduğu gibi JDBC ve ODBC sürücüleri veya SAP ASE sunucunun güncelleştirmeleri doğrudan Sybase sitelerinden yüklemeyin. Düzeltme ekleri hakkında ayrıntılı bilgi için SAP Business Suite ürün şirket içi kullanım için desteklenir ve Azure sanal Makineler'de aşağıdaki SAP notları görebilirsiniz:

* [1590719]
* [1973241]

SAP Business Suite, SAP ASE üzerinde çalıştırma hakkında genel bilgiler bulunabilir [SCN](https://www.sap.com/community/topic/ase.html)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Azure vm'lerde SAP ilgili SAP ASE yüklemeleri için SAP ASE yapılandırma yönergeleri
#### <a name="structure-of-the-sap-ase-deployment"></a>SAP ASE dağıtım yapısı
SAP ASE yürütülebilir dosyalar bulunan veya kök dosya sistemine (/sybase) VM'nin yüklenir. Genellikle, SAP ASE sisteminizi ve araçlarınızı veritabanlarının en yüksek iş yükü yaşamadığınızdan. Bu nedenle kök dosya sisteminde (master, model, saptools, sybmgmtdb, sybsystemdb) sistem ve araçları veritabanları depolanabilir. 

Bir özel durum, daha yüksek veri hacmine veya g/ç işlemleri toplu gerektirebilir durumunda bazı SAP ERP ve tüm BW iş yüklerini geçici veritabanı olabilir. Birim veya sanal makinenin işletim sistemi diski tarafından sağlanan IOPS 

SAPInst/SWPM sistemini yüklemek için kullanılan sürümüne bağlı olarak, veritabanı içerebilir:

* SAP ASE yüklenirken oluşturulan tek bir SAP ASE tempdb
* SAP ASE ve SAP yükleme yordamı tarafından oluşturulan bir ek saptempdb yükleyerek oluşturulmuş bir SAP ASE tempdb
* SAP ASE ve el ile oluşturulmuş bir ek tempdb yükleyerek oluşturulmuş bir SAP ASE tempdb (örneğin'lu SAP notuna aşağıdaki [1752266]) ERP/BW belirli tempdb gereksinimlerini karşılamak için

Performans nedenleriyle dışında belirli ERP veya tüm BW iş yükleri, tek bir Azure veri diski ya da Linux m kapsayan bir RAID tarafından temsil edilebilir bir ayrı bir dosya sisteminde tempdb cihazları ayrıca oluşturulan tempdb (SWPM tarafından veya elle) depolamak için anlamlı olabilir birden çok Azure veri diskleri. Hiçbir ek tempdb varsa oluşturmanız önerilir (SAP notu [1752266]).

Bu tür sistemler için ayrıca oluşturulan tempdb için aşağıdaki adımlar gerçekleştirilmelidir:

* İlk tempdb dizini SAP veritabanına ilk dosya sistemine taşıma
* Tempdb dizinler her bir dosya sistemi SAP veritabanının bulunduğu disk ekleme

Bu yapılandırma etkinleştirir tempdb sistem sürücüsünün sunabileceğinden daha fazla alan kullanır. Bir başvuru olarak bir şirket içi mevcut sistemler, tempdb cihaz boyutları kontrol edebilirsiniz. Ya da bu tür bir yapılandırma sistem sürücüsü ile sağlanan tempdb karşı IOPS sayılarında etkinleştirir. Şirket içinde çalışan sistemler, tempdb karşı g/ç iş yükünü izlemek için kullanılabilir.

Hiçbir zaman herhangi bir SAP ASE dizin /mnt veya sanal makinenin /mnt/resource üzerine yerleştirin. Tempdb tutulan nesneleri yalnızca geçici olsa bile, SAP ASE için bu önerileri tempdb için de geçerlidir. Kalıcı olmayan bir varsayılan Azure VM geçici alanı /mnt veya /mnt/resource olduğundan. Azure VM geçici alanı hakkında daha fazla ayrıntı bulunabilir [bu makalede][virtual-machines-linux-how-to-attach-disk]

Veri ve işlem günlük dosyası dağıtımlar, ifadeler ve yapılan öneriler için [SAP iş yükü Azure sanal makineleri DBMS dağıtım konuları](dbms_guide_general.md). Linux tabanlı dağıtımlar durumunda LVM veya MDADM kullanımını eşlikli yeterli IOPS, üretilen iş ve birim oluşturmak için kullanılması önerilir. 

#### <a name="impact-of-database-compression"></a>Veritabanı sıkıştırması etkisini
Burada, g/ç bant genişliği sınırlayıcı bir etmen haline gelebilir yapılandırmalarında, IOPS azaltır her ölçü bir Azure gibi bir Iaas senaryosu çalıştırabilirsiniz iş yükü esnetme artırmaya yardımcı olabilir. Bu nedenle, SAP ASE sıkıştırma mevcut SAP veritabanını Azure'a yüklemeden önce kullanıldığından emin olmak için önerilir.

Azure'a karşıya yüklemeden önce sıkıştırma uygulamak için öneri dışında çeşitli nedenleri verilmiştir:

* Azure'a karşıya yüklenecek veri miktarını düşük
* Daha fazla CPU veya daha yüksek g/ç bant genişliği veya g/ç gecikme şirket içi daha az daha güçlü donanım kullanabilirsiniz varsayarak sıkıştırma yürütme süresi kısadır
* Küçük veritabanı boyutları için disk ayırmayı daha az maliyetlerine neden olabilir

Veri ve LOB sıkıştırma, şirket içi yaptığı gibi Azure sanal Makineler'de barındırılan bir VM içinde çalışır. Varolan bir SAP ASE veritabanında kullanım sıkıştırma zaten kullanımda olup olmadığını denetlemek nasıl daha fazla bilgi için ' lu SAP notuna denetleyin [1750510]. Veritabanı sıkıştırma hakkında daha fazla bilgi için bkz. Not SAP [2121797].

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Veritabanı örnekleri izlemek için DBACockpit kullanma
SAP ASE veritabanı platform kullanan, SAP sistemlerini için DBACockpit işlem DBACockpit katıştırılmış tarayıcı pencerelerini veya Webdynpro olarak erişilebilir durumdadır. Ancak veritabanı yönetme ve izleme için tam işlevsellik yalnızca DBACockpit Webdynpro uygulamasında kullanılabilir.

Olarak şirket içi sistemler ile birkaç adım DBACockpit Webdynpro uygulaması tarafından kullanılan tüm SAP NetWeaver işlevselliğini etkinleştirmek için gereklidir. SAP notu izleyin [1245200] webdynpros kullanımını etkinleştirin ve gerekli değerler oluşturmak için. Yukarıdaki Notları'ndaki yönergeleri takip ederken, ayrıca Internet iletişimi Yöneticisi'ni (ICM) http ve https bağlantıları için kullanılacak bağlantı noktaları ile birlikte yapılandırırsınız. Http için varsayılan ayarı şöyle görünür:

> ICM/server_port_0 değerler bağlantı noktası = HTTP bağlantı noktası = 8000 PROCTIMEOUT = zaman AŞIMI 600 = 600 =
> 
> ICM/server_port_1 değerler bağlantı noktası = HTTPS, bağlantı noktası = = 443$ $, PROCTIMEOUT 600, zaman AŞIMI = 600 =
> 
> 

ve işlem sırasında oluşturulan bağlantılar DBACockpit şuna benzer görünecektir:

> https:\//\<fullyqualifiedhostname>:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http:\//\<fullyqualifiedhostname >: sap/8000/bc/sap/webdynpro/dba_cockpit
> 
> 

SAP sistemi barındıran Azure sanal makine için AD nasıl bağlandığını bağlı olarak ve bir DNS ICM burada açmakta gelen DBACockpit makinede çözülebilir tam bir konak adı kullandığınızdan emin olun. ' Lu SAP notuna bakın [773830] ICM profili parametrelerinden ve kümesi parametre ICM/host_name_full bağlı olarak tam ana bilgisayar adı açıkça gerekirse etiketleneceğini nasıl anlamak için.

Şirket içi ile Azure arasında şirketler arası bağlantı olmadan yalnızca bulut senaryosunda VM dağıttıysanız, genel bir IP adresi ve bir domainlabel tanımlamanız gerekir. Genel DNS adı VM'nin biçimi şu şekilde görünür:

> `<custom domainlabel`>.`<azure region`>.cloudapp.azure.com
> 
> 

DNS adına ilgili daha fazla ayrıntı bulunabilir [burada][virtual-machines-azurerm-versus-azuresm].

Azure VM bağlantısı için DNS adı SAP profili parametresi ICM/host_name_full ayarlama şuna benzeyebilir:

> https:\//mydomainlabel.westeurope.cloudapp.net:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http:\//mydomainlabel.westeurope.cloudapp.net:8000/sap/bc/webdynpro/sap/dba_cockpit

Bu durumda için emin olmanız gerekir:

* Azure portalında ICM ile iletişim kurmak için kullanılan TCP/IP bağlantı noktaları için ağ güvenlik grubu için gelen kuralları ekleme
* ICM ile iletişim kurmak için kullanılan TCP/IP bağlantı noktaları için Windows Güvenlik duvarı yapılandırması için gelen kuralları ekleme

Otomatikleştirilmiş için kullanılabilir tüm düzeltmeleri alınan, düzenli aralıklarla SAP notu SAP sürümünüz için geçerli düzeltme koleksiyonu uygulamak için önerilir:

* [1558958]
* [1619967]
* [1882376]

Aşağıdaki SAP notları SAP ASE için DBA Cockpit hakkında daha fazla bilgi bulunabilir:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496]    
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>SAP ASE için yedekleme/kurtarma konuları
SAP ASE Azure'a dağıtırken, yedekleme yönteminize gözden geçirilmesi gerekir. Üretim dışı sistemlere için bile, SAP veritabanlarının düzenli aralıklarla yedeklenmelidir. Azure depolama üç görüntü tutar beri bir yedekleme için bir depolama kilitlenme telafi açısından daha az önemli olabilir. Uygun bir yedekleme ve geri yükleme planı sürdürmek için birincil nedeni, belirli bir noktadaki zaman kurtarma özellikleri sağlayarak mantıksal/el ile hataları dengeleyebilir daha fazla olmasıdır. Bu nedenle yedeklemelerini geri belirli bir noktaya veritabanını geri veya var olan veritabanı kopyalayarak başka bir sistem kaynağını oluşturmak için Azure'da yedeklemelerini kullanın ya da kullanın olmaktır. 

Şirket içi yedekleme ve azure'da bir veritabanı geri yükleme aynı şekilde çalışır. SAP notlara bakın:

* [1588316]
* [1585981]

Döküm yapılandırmaları oluşturma ve zamanlama yedekleme hakkında daha fazla bilgi için. Stratejisi ve gereksinimlerine bağlı olarak, var olan disklerden birini açın disk yedekleme için ek bir disk eklemek için veritabanı ve günlük dökümleri yapılandırabilirsiniz. Veri kaybı bir hata durumunda olma tehlikesi azaltmak için hiçbir veritabanı dizin/dosya bulunduğu bir disk kullanmak için önerilir.

Veri ve LOB sıkıştırma yanı sıra, SAP ASE yedekleme sıkıştırma de sunar. Daha az alan veritabanı ve günlük dökümleri ile kullanmak için bir yedekleme sıkıştırma kullanmak için önerilir. Daha fazla bilgi için bkz. Not SAP [1588316]. Yedekleme sıkıştırma ayrıca yedekleri ya da şirket içi Azure sanal makinenin yedekleme dökümleri içeren VHD'ler indirmeyi planlıyorsanız aktarılan veri miktarını azaltmak çok önemlidir.

Azure VM geçici alanı /mnt veya /mnt/resource döküm hedef veritabanı veya günlük olarak kullanmayın.

#### <a name="performance-considerations-for-backupsrestores"></a>Yedekleme/geri yükleme işlemleri için performans konuları
Çıplak metal dağıtımlarındaki yedekleme/geri yükleme performansı, paralel olarak kaç birimleri okunabilir ve ne bu birimlerin verimi olabilir bağlıdır. Yedekleme sıkıştırma CPU kaynaklarını tüketir göz önünde bulundurun. Bu CPU tüketimi yedekleme sıkıştırma, önemli bir rol ile CPU iş parçacıklarını az sayıda vm'lerde oynatabilirsiniz.  Bu nedenle, kabul edilebilir:

* Daha az veritabanı cihazları depolamak için kullanılan disk sayısını daha küçük genel üretilen işi okuma
* Daha küçük CPU sayısını, VM, daha ciddi yedekleme sıkıştırma etkisini iş parçacıkları
* Yedekleme için daha düşük aktarım hızı yazmak için daha az sayıda hedefle (Linux yazılım RAID, diskler)

Kullanılan/birleşik, gereksinimlerinize bağlı olarak iki seçenek yazılacak hedefleri sayısını artırmak için:

* Şeritli birim IOPS aktarım hızını iyileştirmek için birden fazla bağlı diskler üzerinde Yedekleme hedef birimin bölümlemesi
* Döküm yapılandırma düzeyinde SAP ASE oluşturma, birden fazla hedef dizin dökümünü almak için yazılacak kullanır

Bir disk birimi üzerinde birden fazla bağlı disk bölümlemesi ele alınan [SAP iş yükü Azure sanal makineleri DBMS dağıtım konuları](dbms_guide_general.md). Birden çok dizin SAP ASE döküm yapılandırmasında kullanma ile ilgili daha fazla bilgi için döküm yapılandırmasını oluşturmak için kullanılan saklı yordam sp_config_dump belgelerine bakın [Sybase Bilgi Merkezi](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Azure sanal makineler ile olağanüstü durum kurtarma
#### <a name="data-replication-with-sap-sybase-replication-server"></a>Veri çoğaltma ile SAP Sybase çoğaltma sunucusu
SAP Sybase çoğaltma sunucusuna (SRS ile), SAP ASE veritabanı işlemleri zaman uyumsuz olarak uzak bir konuma aktarmak için normal bir bekleme çözümü sağlar. 

Yükleme ve SRS işleyişini çalışır de işlevsel olarak şirket içi yaptığı gibi Azure sanal makine hizmetlerinde barındırılan bir VM.

ASE HADR SAP çoğaltma sunucusu aracılığıyla desteklenir. Bu tür bir yapılandırma denemek için SAP ASE 16.03 kullanmak için önerilir. Bu tür yapılandırmaları yüklemek için daha ayrıntılı yönergeler için bu ayrıntılı [blog](https://blogs.msdn.microsoft.com/saponsqlserver/2018/06/18/installation-procedure-for-sybase-16-3-patch-level-3-always-on-dr-on-suse-12-3-recent-customer-proof-of-concept/).
