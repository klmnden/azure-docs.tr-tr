---
title: SAP iş yükü için SQL Server Azure sanal makineleri DBMS dağıtım | Microsoft Docs
description: SAP iş yükü için SQL Server Azure Sanal Makineler DBMS dağıtımı
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
ms.date: 09/26/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0c12c75bd5c357613d55e04aed67c0cc901135e6
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58881095"
---
# <a name="sql-server-azure-virtual-machines-dbms-deployment-for-sap-netweaver"></a>SAP NetWeaver için SQL Server Azure sanal makineleri DBMS dağıtım

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

[dbms-guide]:dbms-guide_general.md 
[dbms-guide-2.1]:dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f 
[dbms-guide-2.2]:dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 
[dbms-guide-2.3]:dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 
[dbms-guide-2]:dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 
[dbms-guide-3]:dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 
[dbms-guide-5.5.1]:dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 
[dbms-guide-5.5.2]:dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b 
[dbms-guide-5.6]:dbms_guide_sqlserver.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 
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
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/eresources/templates/sql-server-2014-alwayson-existing-vnet-and-ad/
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




Bu belge, Azure Iaas SAP iş yükü için SQL Server'ı dağıtma yaparken dikkate alınması gereken birkaç farklı alanlarını kapsar. Bu belge için bir önkoşul belge okuma [SAP iş yükü Azure sanal makineleri DBMS dağıtım konuları](dbms_guide_general.md) diğer yönergelerinde yanı sıra [AzureBelgeleri'ndeSAPişyükü](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started). 



> [!IMPORTANT]
> Bu belgenin kapsamı SQL Server'da Windows sürümüdür. SAP, SAP yazılımının biriyle SQL Server Linux sürümünü desteklemediğinden. Belge Microsoft Azure SQL veritabanı, Microsoft Azure platformu bir hizmet teklifinin bir Platform olan üzerinde tartışmaktadır değil. Azure sanal bir hizmet Özelliği Azure altyapısından yararlanarak Makineler'de, şirket içi dağıtımlar için bilinen SQL Server ürün çalıştıran bu yazıdaki bir tartışma bulunmaktadır. Veritabanı özellikleri ve işlevleri bu iki teklifler arasında farklıdır ve birbiriyle karışmasını değil. Ayrıca bkz: <https://azure.microsoft.com/services/sql-database/>
> 
>

Genel olarak, en son SQL Server'ı kullanarak Azure Iaas içinde SAP iş yükü çalıştırmak için sürümleri dikkate almanız gerekir. En son SQL Server sürümleri, bazı Azure Hizmetleri ve İşlevler daha iyi tümleştirme sağlar. Veya Azure Iaas altyapısını işlemlerini iyileştirmenize değişiklikler içerir.

Gözden geçirmek için önerilen [bu] [ virtual-machines-sql-server-infrastructure-services] devam etmeden önce belgeleri.

Aşağıdaki bölümlerde yukarıdaki bağlantıyı altında belge parçalarını parçalarını toplanır ve belirtilen. SAP ilgili detaylar de belirtilmiştir ve bazı kavramları daha ayrıntılı olarak açıklanmıştır. Ancak, SQL Server'a özgü belgelere okuma önce ilgili belgelerde ilk üzerinde çalışmak için önerilir.

Iaas belirli bilgiler devam etmeden önce bilmeniz gereken bazı SQL Server vardır:

* **SQL sürüm desteği**: SAP müşteriler, SQL Server 2008 R2 ve Microsoft Azure sanal Makinesi'nde yüksek desteklenir. Önceki sürümlerinde desteklenmez. Bu genel gözden [destek bildirimiyle](https://support.microsoft.com/kb/956893) daha fazla ayrıntı için. Genel olarak, SQL Server 2008'de Microsoft tarafından desteklenir. Ancak, önemli işlevleri için SQL Server 2008 R2 ile birlikte sunulan, SAP, SQL Server 2008 R2 SAP için en düşük sürüm kaynaklanır. Genel olarak, en son SQL Server'ı kullanarak Azure Iaas içinde SAP iş yükü çalıştırmak için sürümleri dikkate almanız gerekir. En son SQL Server sürümleri, bazı Azure Hizmetleri ve İşlevler daha iyi tümleştirme sağlar. Veya Azure Iaas altyapısını işlemlerini iyileştirmenize değişiklikler içerir. Bu nedenle, yazıda SQL Server 2017 ve SQL Server 2016 ile sınırlıdır.
* **SQL performansı**: Microsoft Azure da diğer genel bulut sanallaştırma teklifleri, ancak tek sonuçları ile barındırılan sanal makineleri gerçekleştirmek farklılık gösterebilir. Makaleyi inceleyin [Azure sanal Makineler'de SQL Server için en iyi performans](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance).
* **Azure Marketteki görüntüleri kullanarak**: Yeni bir Microsoft Azure sanal makine dağıtmak için en hızlı yolu, Azure Marketi'nden bir görüntü kullanmaktır. En son SQL Server sürümleriyle içeren görüntüleri Azure Market'te vardır. SQL Server önceden yüklendiği görüntüleri için SAP NetWeaver uygulamalarını hemen kullanılamaz. Varsayılan SQL Server harmanlaması bu görüntülerin ve SAP NetWeaver sistemleri tarafından gerekli harmanlama yüklü nedenidir. Bu görüntüleri kullanmak için bölümde belgelenen adımları denetleyin [Microsoft Azure Marketi dışında bir SQL sunucu görüntüsü kullanarak][dbms-guide-5.6]. 


## <a name="recommendations-on-vmvhd-structure-for-sap-related-sql-server-deployments"></a>VM/VHD yapısına'SAP ile ilgili SQL Server dağıtımları için öneriler
Genel bir açıklama uygun olarak, SQL Server yürütülebilir dosyalar bulunan veya yüklü sanal makinenin işletim sistemi diski sistem sürücüsüne (C: sürücüsü\).  Genellikle, SQL Server sistem veritabanlarının en yüksek düzeyde SAP NetWeaver iş yükü tarafından kullanılmaz. Sonuç olarak (master, msdb ve model) SQL Server'ın sistem veritabanları da C:\ sürücüsü üzerinde kalır. Bir özel durum, SAP iş yükleri, söz konusu olduğunda, daha yüksek veri hacmine veya g/ç işlemleri toplu gerektirebilir, TempDB'de olmalıdır. İşletim sistemi VHD'si için uygulanmamalıdır g/ç iş yükü. Bu tür sistemler için aşağıdaki adımlar gerçekleştirilmelidir:


* İle tüm SAP sertifikalı sanal makine türleri (SAP notu bkz [1928533]), A serisi VM'ler, tempdb verilerini ve günlük dosyaları kalıcı olmayan D:\ sürücüsüne yerleştirilebilir dışında. 
* Bununla birlikte, birden fazla tempdb veri dosyası kullanmak için önerilir. VM türüne göre D:\ sürücüsüne birimlerin farklı olduğunu unutmayın. Farklı sanal makinelerin D:\ sürücüsüne tam boyutları makale onay [boyutları için Windows azure'da sanal makineler](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).

Bu yapılandırmalar, sistem sürücüsünün sağlamak mümkün olandan daha fazla alan kullanmak tempdb etkinleştirin. Kalıcı olmayan D:\ sürücüsüne de daha iyi g/ç gecikme süresi ve aktarım hızı (hariç, A serisi VM'ler) sunar. Uygun tempdb boyutunu belirlemek için var olan sistemler tempdb boyutları kontrol edebilirsiniz. 

>[!NOTE]
> tempdb veri dosyası ve günlük dosyası oluşturduğunuz D:\ sürücüsüne üzerinde bir klasöre yerleştirin durumda klasörü sonra VM'yi yeniden başlatma yok emin olmanız gerekir. VM yeniden başlatıldıktan sonra D:\ sürücüsüne yeni başlatılmış olduğundan tüm dosya ve dizin yapıları temizlendiğinde. SQL Server hizmetinin başlangıç belgelenen önce D:\ sürücüsüne nihai dizin yapıları yeniden oluşturmak için bir olasılık [bu makalede](https://www.sqlserver.co.uk/index.php/using-ssds-in-azure-vms-to-store-sql-server-tempdb-and-buffer-pool-extensions/).

Tempdb verilerini ve tempdb logfile D:\ sürücüsüne yerleştirildiği ve bir SAP veritabanı ile SQL Server çalıştıran bir VM yapılandırması gibi görünür:

![SQL Server için basit VM disk yapılandırması diyagramı](./media/dbms_sqlserver_deployment_guide/Simple_disk_structure.PNG)

Yukarıdaki diyagramda, basit bir örnek gösterir. İçin makalede eluded gibi [SAP iş yükü Azure sanal makineleri DBMS dağıtım konuları](dbms_guide_general.md), sayı, ve Premium depolama disk boyutu farklı faktöre bağlıdır. Ancak genel öneririz:

- Bir veya birkaç küçük birimler oluşturmak için depolama alanları kullanarak, SQL Server veri dosyaları içerir. Bu yapılandırma altında yatan neden gerçek hayatta farklı boyutta veritabanı dosyalarını farklı g/ç iş yükü ile çok sayıda SAP veritabanlarıyla yoktur.
- Yeterli IOPS sağlamak için depolama alanları'nı kullanarak ve SQL Server işlem günlüğü dosyası. Olası IOPS iş yükü genellikle SQL Server işlem hacmi değil olası hacmine ve işlem günlük birimi boyutlandırması için yol gösterici satır değil
- Performans yeterince iyi olduğu sürece D:\drive tempdb için kullanın. Genel iş yükü performansı D:\ sürücüsüne üzerindeymiş tmepdb sınırlıdır, Premium depolama diskleri de önerildiği şekilde ayırmak için tempdb taşımak için dikkate almanız gerekebilir [bu makalede](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance).


### <a name="special-for-m-series-vms"></a>M serisi sanal makineler için özel
Azure M serisi VM için Azure Premium depolama performansı için Azure yazma Hızlandırıcı kullanırken karşılaştırıldığında faktörler tarafından işlem günlüğüne yazma gecikme süresi azaltılabilir. Bu nedenle, SQL Server işlem günlüğü için birim form vhd'sinin için Azure yazma Hızlandırıcı dağıtmanız gerekir. Ayrıntılar belgede okunabilir [yazma hızlandırıcı](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator).
  

### <a name="formatting-the-disks"></a>Diskleri biçimlendirme
SQL Server için SQL Server veri ve günlük dosyalarını içeren diskler için NTFS blok boyutu 64 KB olmalıdır. D:\ sürücüsüne biçimlendirmek için gerek yoktur. Bu sürücü önceden biçimlendirilmiş gelir.

Geri yükleme ya da veritabanı oluşturulmasını veri dosyalarını dosyaların içeriğini sıfırlama tarafından başlatılıyor değil emin emin olmak için SQL Server hizmetini çalıştıran kullanıcı bağlamı belirli bir izni olduğundan emin olmanız gerekir. Genellikle Windows Yönetici grubundaki kullanıcılar bu izinlere sahip olursunuz. SQL Server hizmetini Windows yönetici olmayan kullanıcı kullanıcı bağlamında çalışırsa, kullanıcı hakkı bu kullanıcı atamanız gerekir **Birim bakım görevleri gerçekleştir**.  Bu Microsoft Bilgi Bankası makalesi ayrıntılara bakın: <https://support.microsoft.com/kb/2574695>

### <a name="impact-of-database-compression"></a>Veritabanı sıkıştırması etkisini
Burada, g/ç bant genişliği sınırlayıcı bir etmen haline gelebilir yapılandırmalarında, IOPS azaltır her ölçü bir Azure gibi bir Iaas senaryosu çalıştırabilirsiniz iş yükü esnetme artırmaya yardımcı olabilir. Bu nedenle, henüz yapıldığında, SQL Server sayfa sıkıştırmayı uygulama SAP ve Microsoft tarafından mevcut SAP veritabanını Azure'a yüklemeden önerilir.

Azure'a yüklemeden önce veritabanı sıkıştırma gerçekleştirmek için öneri dışında iki nedenleri verilmiştir:

* Karşıya yüklenecek veri miktarı düşüktür.
* Daha fazla CPU veya daha yüksek g/ç bant genişliği veya g/ç gecikme şirket içi daha az daha güçlü donanım kullanabilirsiniz varsayarak sıkıştırma yürütme süresi kısadır.
* Küçük veritabanı boyutları için disk ayırmayı daha az maliyetlerine neden olabilir

Şirket içi yaptığı gibi veritabanı sıkıştırma bir Azure sanal Makineler'de de çalışır. Mevcut SAP NetWeaver SQL Server veritabanları, sıkıştırma konusunda daha fazla ayrıntı denetlemek için makaleye [SAP geliştirilmiş sıkıştırma aracı MSSCOMPRESS](https://blogs.msdn.microsoft.com/saponsqlserver/2016/11/25/improved-sap-compression-tool-msscompress/). 

## <a name="sql-server-2014-and-more-recent---storing-database-files-directly-on-azure-blob-storage"></a>SQL Server 2014 ve daha yeni - depolama veritabanı dosyalarını doğrudan Azure Blob Depolama üzerinde
SQL Server 2014 ve üzeri sürümlerde açık olasılığını doğrudan Azure Blob Store VHD etrafında 'sarmalayıcı' olmadan veritabanı dosyalarını depolamak için. Standart Azure depolama veya daha küçük VM türleri özellikle kullanımı ile bu tür dağıtımda, sınırlı sayıda bazı küçük bir VM türleri bağlı diskler tarafından uygulanabiliyordu IOPS sınırları nerede üstesinden gelebilir senaryolara olanak sağlar. Ancak, SQL Server'ın sistem veritabanları için kullanıcı veritabanları için dağıtım bu şekilde çalışır. Ayrıca, SQL Server'ın veri ve günlük dosyaları için çalışır. SAP SQL Server veritabanı dağıtmak istiyorsanız 'sarmalama' yerine bu şekilde VHD'ler, içine tutmak unutmayın:

* Depolama hesabı gereksinimlerini VM SQL Server'ı dağıtmak için kullanılan bir çalıştırıyorsa aynı Azure bölgesinde olacak şekilde kullanılır.
* VHD dağıtım ile ilgili farklı Azure depolama hesapları daha önce listelenen konular bu dağıtımlar da yöntem için geçerlidir. Azure depolama hesabı sınırları karşı g/ç işlemlerinin sayısı anlamına gelir.
* Sanal makinenin depolama g/ç kota karşı hesap yerine depolama blobları SQL Server veri ve günlük dosyalarını temsil eden karşı trafiğin muhasebesi sanal makinenin ağ bant genişliği belirli VM türü. Ağ ve depolama için bant genişliği belirli bir VM türünün, makalesine başvurun [boyutları için Windows azure'da sanal makineler](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).
* Dosya g/ç ağ kotası aracılığıyla gönderme, sonucu olarak depolama kotası çoğunlukla stranding ve ile sanal makinenin toplam bant genişliği yalnızca kısmen kullanın.
* Azure Premium depolama için farklı bir disk boyutlarına sahip IOPS ve g/ç aktarım hızı performans hedeflerini artık geçerli değildir. Azure Premium depolama blobları, oluşturduğunuz bile bulunur. Hedefleri belgelenen makaleyi [yüksek performanslı Premium depolama ve VM'ler için yönetilen diskler](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage). SQL Server veri dosyaları ve günlük dosyalarını doğrudan Azure Premium depolama alanında depolanan blobları yerleştirme sonucu olarak performans özellikleri VHD'lerde Azure Premium depolama ile karşılaştırıldığında farklı olabilir.
* Azure Premium depolama diskleri olarak önbelleğe alma tabanlı bir konak, doğrudan Azure BLOB'ları üzerinde SQL Server veri dosyaları yerleştirirken kullanılabilir değil.
* M serisi sanal makineler üzerinde SQL Server işlem günlüğü dosyasına yönelik milisaniyenin yazma desteklemek için Azure yazma Hızlandırıcı kullanılamaz. 

Bu işlevlerin bilgi makalesinde bulunabilir [Microsoft azure'da SQL Server veri dosyaları](https://docs.microsoft.com/sql/relational-databases/databases/sql-server-data-files-in-microsoft-azure?view=sql-server-2017)

Üretim sistemleri için bu yapılandırmayı önlemek ve yerine SQL Server veri yerleşimi seçin ve doğrudan Azure BLOB üzerinde yerine Azure Premium depolama VHD'leri, günlük dosyalarını önerilir.


## <a name="sql-server-2014-buffer-pool-extension"></a>SQL Server 2014 arabellek havuzu genişletme
SQL Server 2014 olarak adlandırılan yeni bir özellik tanıtılan [arabellek havuzu uzantısı](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension?view=sql-server-2017). Arabellek havuzu sunucusunun yerel SSD veya VM tarafından desteklenen bir ikinci düzey önbelleği ile bellekte tutulur SQL Server'ın bu işlevselliği genişletir. Arabellek havuzu uzantısı 'bellek' daha büyük bir çalışma kümesi veri tutma sağlar. Azure standart depolama erişimi karşılaştırıldığında Azure VM'deki yerel SSD'ler üzerinde depolanan arabellek havuzu uzantısı Access'e faktörlerden daha hızlıdır. Arabellek havuzu uzantısı için Azure Premium depolama okuma önbelleği, SQL Server veri dosyaları için önerilen şekilde karşılaştırma, önemli hiçbir avantajı için arabellek havuzu genişletmelerini beklenmektedir. Azure işlem düğümünün yerel diskler hem önbellekler (SQL Server arabellek havuzu genişletme ve Premium depolama okuma önbelleği) kullandığınızdan emin nedenidir.

Sırada SQL Server arabellek havuzu uzantısı ile SAP iş yükü kazanılan deneyimleri, karma ve hala NET öneriler her durumda kullanıp kullanmayacağınızı üzerinde izin vermiyor. SAP uygulama gerektiriyorsa çalışma kümesi ana belleğe uyan ideal durumdur. Bu arada, 4 TB ile bellek gelen Vm'leri sunan Azure ile çalışma kümesi bellekte tut ulaşılabilir olmalıdır. Bu nedenle arabellek havuzu uzantısı kullanımı için bazı nadir durumlarda sınırlıdır ve temel bir durumda olmaması gerekir.  

## <a name="backuprecovery-considerations-for-sql-server"></a>SQL Server için yedekleme/kurtarma konuları
SQL Server'ı Azure'a dağıtma, yedekleme yönteminize gözden geçirilmesi gerekir. Sistem, bir üretim sistemine değil olsa bile, SQL Server tarafından barındırılan SAP veritabanı düzenli aralıklarla yedeklenmelidir. Azure depolama üç görüntü tutar olduğundan, bir yedekleme şimdi daha az depolama kilitlenme telafi için açısından önemlidir. Uygun bir yedekleme ve kurtarma planı sürdürmek için öncelik daha belirli bir noktadaki zaman kurtarma özellikleri sağlayarak mantıksal/el ile hataları dengeleyebilir nedenidir. Bu nedenle yedeklemelerini geri belirli bir noktaya veritabanını geri veya var olan veritabanı kopyalayarak başka bir sistem kaynağını oluşturmak için Azure'da yedeklemelerini kullanın ya da kullanın olmaktır. 

Azure yedekleme olanaklar farklı SQL Server aramak için makaleyi okuyun [yedekleme ve Azure sanal Makineler'de SQL Server için geri yükleme](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery). Makale birkaç farklı olasılık kapsar.

### <a name="manual-backups"></a>El ile yedekleme
'Manual' yedeklemeleri gerçekleştirmek için çeşitli olanaklar var.

1. Doğrudan bağlı Azure disklerde geleneksel SQL Server yedekleme gerçekleştirme. Bu yöntem sistem yenilemeler için hemen kullanılabilir yedeklemelere sahip ve yeni sistemleri mevcut SAP sistemlerini bir kopyasını oluşturmak bir avantajı vardır.
2.  SQL Server 2012 CU4 ve veritabanlarını yedeklemek için bir Azure depolama URL'si yüksek yedekleyebilirsiniz.
3.  Dosya anlık görüntüsü yedekleri Azure Blob depolama alanındaki veritabanı dosyaları için. Bu yöntem yalnızca SQL Server veri ve günlük dosyalarını Azure blob depolama alanında bulunan olduğunda çalışır

İlk yöntem, bilinen ve çoğu durumda şirket içi dünyanın de uygulanır. Ancak yine de, uzun vadeli yedekleme konumu çözmek için görevle bırakır. En az 30 gün içinde yerel olarak bağlı Azure depolama için yedeklemelerinizi tutmak istemediğiniz olduğundan ya da Azure yedekleme hizmeti veya yedeklemeleriniz için erişim ve koruma Yönetimi içeren başka bir üçüncü taraf yedekleme/Kurtarma aracını kullanmaya gerek vardır. Veya, azure'da büyük dosya sunucusu Windows depolama alanları kullanarak oluşturmak.

İkinci yöntem daha yakın makalesinde açıklanan [URL'ye SQL Server Yedekleme](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url?view=sql-server-2017). SQL Server'ın farklı sürümleri, bu işlev bazı farklılıklar vardır. Bu nedenle, belirli SQL Server sürüm denetimi için belgeleri inceleyin denetlemeniz gerekir. Bu makalede kısıtlamaları birçok listelediğine dikkat edin önemlidir. Ya da yedekleme karşı olanağına sahip olursunuz:

- Ardından 1000 GB yedekleme boyutunu sınırlayan tek tek Azure sayfa blobu. Bu ayrıca aktarım hızı elde edebileceğiniz sınırlar.
- Birden çok teorik bir yedekleme boyutu 12 TB etkinleştir (en fazla 64) Azure blok blobları,. En fazla yedekleme boyutu, teorik sınırdan daha küçük olabilir ancak, müşteri veritabanları ile testleri göstermiştir. Bu durumda, bekletme ve erişim o yedeklerin de yönetmekten sorumlu olursunuz.


### <a name="automated-backup-for-sql-server"></a>SQL Server için Otomatik Yedekleme
Otomatik yedekleme, azure'daki bir Windows VM'de çalışan SQL Server Standard ve Enterprise sürümleri için otomatik bir yedekleme hizmeti sağlar. Bu hizmet tarafından sağlanan [SQL Server Iaas Aracısı uzantısı](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension), otomatik olarak yüklendiği SQL Server Windows sanal makine görüntüleri Azure portalında üzerinde. Yüklü SQL Server ile kendi işletim sistemi görüntüleri dağıtmak istiyorsanız VM uzantıları ayrı olarak yüklemeniz gerekir. Gerekli olan adımları, bu konuda belgelenmiştir [makale](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension).

Bu yöntemin özellikleri hakkında daha fazla bilgi aşağıdaki makalelerde bulunabilir:

- SQL Server 2014: [SQL Server 2014 Virtual Machines'de (Resource Manager) için otomatik yedekleme](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-backup)
- SQL Server 2016/2017: [Azure Virtual Machines'de (Resource Manager) için otomatik yedekleme v2](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-backup-v2)

Belgeleme bakarak, işlevselliği daha yeni SQL Server sürümleriyle geliştirilmiş olduğunu görebilirsiniz. Daha fazla ayrıntı otomatik yedekleri makalesinde yayımlanan SQL Server'da [Microsoft azure'da SQL Server yönetilen yedekleme](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure?view=sql-server-2017). Teorik yedekleme boyut sınırını 12 TB'dir.  Otomatik yedeklemeler, 12 TB'ye kadar yedekleme boyutları için iyi bir yöntem olabilir. Birden çok BLOB'ları paralel olarak yazılır olduğundan, 100 MB/sn daha büyük bir işleme bekleyebilirsiniz. 
 

### <a name="azure-backup-for-sql-server-vms"></a>SQL Server Vm'leri için Azure yedekleme
Bu yeni yöntemi SQL Server Yedekleme Haziran 2018'den itibaren Azure yedekleme hizmetleri tarafından genel önizleme olarak sunulur. Diğer üçüncü taraf araçları kullanarak, yani bir hedef konuma stream yedekleri SQL Server VSS/VDI arabirimi yöntemi yedekleme SQL Server için aynıdır. Bu durumda hedef Azure kurtarma Hizmetleri kasası konumdur.

Bu yedekleme yöntemi, birden fazla ayrıntılı bir açıklaması, izleme, yedekleme yapılandırmalarının Merkezi, çok sayıda avantaj ekler ve yönetim kullanılabilir [burada](https://docs.microsoft.com/azure/backup/backup-azure-sql-database). 


### <a name="third-party-backup-solutions"></a>Üçüncü taraf yedekleme çözümleri
SAP müşterilerinin oldukça sayısı için baştan başlayın ve Azure'da çalışan kendi SAP ortamının bir parçası için tam yeni yedekleme çözümleri tanıtacağız olasılığı vardı. Sonuç olarak, var olan yedekleme çözümleri kullanılır ve Azure'a genişletilmiş gerekmiyor. Mevcut yedekleme çözümleri, genellikle ana satıcıların bu alandaki en iyi çalıştığınız azure'a genişletme. 


## <a name="1b353e38-21b3-4310-aeb6-a77e7c8e81c8"></a>Microsoft Azure Marketi dışında bir SQL Server görüntüsü kullanma
Microsoft SQL Server sürümlerini içeren sanal makineleri Azure Market'te sunar. SQL Server ve Windows için Lisansı gerektiren SAP müşteriler, bu görüntüleri kullanarak SQL Server'ın zaten yüklü olan Vm'leri getirerek lisans gereksinimi karşılamak için bir fırsat olabilir. SAP için görüntüleri kullanmak için aşağıdaki konuları yapılması gerekir:

* SQL Server değerlendirme olmayan sürümleri, Azure Marketi'nden dağıtılan VM 'Yalnızca Windows' dan daha yüksek maliyetleri alın. Compare prices için şu makalelere bakın: <https://azure.microsoft.com/pricing/details/virtual-machines/windows/> ve <https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-enterprise/>. 
* SAP tarafından desteklenen SQL Server sürümleri, yalnızca kullanabilirsiniz.
* Azure Marketi'nde sunulan vm'lerde yüklü SQL Server örneğinin harmanlama SAP NetWeaver'ı çalıştırmak için SQL Server örneği gerektirir harmanlama değil. Aşağıdaki bölümde yönde rağmen ile harmanlama değiştirebilirsiniz.

### <a name="changing-the-sql-server-collation-of-a-microsoft-windowssql-server-vm"></a>Microsoft Windows/SQL Server VM, SQL Server harmanlaması değiştiriliyor
SQL Server görüntüleri Azure Market'te SAP NetWeaver uygulamaları tarafından gerekli harmanlamasını kullanacak şekilde ayarlanmadınız beri dağıtımdan hemen sonra değiştirilmesi gerekiyor. SQL Server için bu değişikliği harmanlama aşağıdaki adımlarla yönetici dağıtılan VM'de oturum açmaya devam edebilir ve VM dağıtıldıktan sürede yapılabilir:

* Yönetici olarak bir Windows komut penceresi açın.
* Dizini için C:\Program Files\Microsoft SQL Server\110\Setup Bootstrap\SQLServer2012 değiştirin.
* Komutu yürütün: Setup.exe /QUIET /ACTION=REBUILDDATABASE /INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS=`<local_admin_account_name`> /SQLCOLLATION=SQL_Latin1_General_Cp850_BIN2   
  * `<local_admin_account_name`> VM Galeriden ilk kez dağıtırken yönetici hesabı olarak tanımlanmış olan hesap.

İşlem yalnızca birkaç dakika sürer. Adım doğru sonucuyla sonlandı emin olmak için aşağıdaki adımları gerçekleştirin:

* SQL Server Management Studio’yu açın.
* Bir sorgu penceresi açın.
* Komut sp_helpsort SQL Server ana veritabanında yürütün.

İstenen sonucu gibi görünmelidir:

    Latin1-General, binary code point comparison sort for Unicode Data, SQL Server Sort Order 40 on Code Page 850 for non-Unicode Data

Sonucu farklı ise, SAP dağıtmadan DURDURMAK ve neden Kurulum komutu beklendiği gibi çalışmaması inceleyin. SAP NetWeaver uygulamalarının daha yukarıda bahsedilen farklı SQL Server kod sayfaları ile SQL Server örneği dağıtımı **değil** desteklenir.

## <a name="sql-server-high-availability-for-sap-in-azure"></a>SQL Server yüksek kullanılabilirlik için azure'da SAP
SQL Server SAP için Azure Iaas dağıtımları kullanarak yüksek oranda kullanılabilir DBMS katmanın dağıtılacağı eklemek için birkaç farklı olasılık sahip. Bölümünde açıklandığı gibi [SAP iş yükü Azure sanal makineleri DBMS dağıtım konuları](dbms_guide_general.md) zaten Azure sağlayan farklı süresi SLA'ları için tek bir VM ile bir Azure kullanılabilirlik kümesinde dağıtılan Vm'leri bir çift. Bu, sürücü doğrultusunda Azure kullanılabilirlik kümeleri dağıtımda gerektiren süreli SLA üretim dağıtımlarınıza varsayılır. Böyle bir durumda, bu tür bir kullanılabilirlik kümesinde iki VM en az dağıtmanız gerekir. Bir VM etkin SQL Server örneği çalışır. Diğer VM edilgen örneği çalışır

### <a name="sql-server-clustering-using-windows-scale-out-file-server"></a>Windows ölçek genişletme dosya sunucusu kullanarak SQL Server Kümelemesi
Microsoft Windows Server 2016 ile tanıtılan [depolama alanları doğrudan](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview). Depolama alanları doğrudan dağıtımı bağlı olarak, SQL Server FCI Kümelemesi desteklenir. Bilgi makalesinde bulunabilir [yapılandırma SQL Server Yük devretme kümesi örneği Azure sanal Makineler'de](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-create-failover-cluster). Çözüm, küme kaynaklarını sanal IP adresi ile dağıtılacak Azure yük dengeleyici de gerektirir. SQL Server veritabanı dosyaları, depolama alanları ' depolanır. Bu nedenle, olan bir Azure Premium depolama tabanlı Windows depolama alanları'kurmak oluşturmak için gereken biçimde. Bu çözüm çok uzun henüz için desteklenen olduğundan, SAP üretim senaryolarında bu çözümü kullanan herhangi bir bilinen SAP müşterilerinin vardır.  

### <a name="sql-server-log-shipping"></a>SQL Server günlük aktarma
Yüksek kullanılabilirlik (HA) yöntemleri SQL Server günlük aktarma biridir. Yüksek kullanılabilirlik yapılandırmasında katılan Vm'leri ad çözümlemesinin çalışıp varsa, bir sorun yok ve Kurulum azure'da şirket içinde gerçekleştirilen herhangi bir ayar farklı değildir. Günlük aktarma ve günlük aktarma etrafında ilkeler ayarlama bakımından. SQL Server günlük aktarma ayrıntılarını makalesinde bulunabilir [günlük aktarma (SQL Server) hakkında](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server?view=sql-server-2017).

SQL Server günlük işlevi, bir Azure bölgesi içinde yüksek kullanılabilirlik elde etmek için Azure'da zor kullanıldı. Ancak aşağıdaki senaryolarda SAP müşterilerinin Azure ile birlikte başarılı günlük aktarma kullandığınız:

- Bir Azure bölgesinden başka bir Azure bölgesine içine olağanüstü durum kurtarma senaryoları
- Bir Azure bölgesi içinde şirket içi olağanüstü durum kurtarma yapılandırması
- Şirket içi senaryolarından kesme üzerinden azure'a. Bu gibi durumlarda, günlük aktarma, azure'da yeni DBMS dağıtım devam eden üretim sistemi şirket içi ile eşitlemek için kullanılır. Zaman içinde kesme, üretim kapatılır ve son ve en son işlem günlüğü yedeklemeleri için Azure DBMS dağıtım aktarılan emin yapılır. Ardından Azure DBMS dağıtım için üretim açılır.  



### <a name="database-mirroring"></a>Veritabanı yansıtma
SAP tarafından desteklenen yansıtma veritabanı (SAP notu bkz [965908]) üzerinde bir yük devretme iş ortağı SAP bağlantı dizesinde tanımlama kullanır. Şirketler arası durumlarda, iki VM aynı etki alanında olduğunu ve kullanıcı bağlamı iki SQL Server örneklerini de bir etki alanı kullanıcı altında çalışan ve ilgili iki SQL Server durumlarda yeterli ayrıcalıklara sahip varsayıyoruz. Bu nedenle, azure'da veritabanı yansıtma kurulumu tipik şirket içi Kurulumu/yapılandırma farklı değildir.

Yalnızca bulut dağıtımlarına itibarıyla en kolay yöntem bir etki alanı içinde başka bir etki alanı Kurulum bu DBMS Vm'leri (ve ideal olarak özel SAP VM'ler) Azure'da zorunda kalmaz.

Bir etki alanı mümkün değilse, bir sertifika için uç noktalar burada açıklandığı şekilde yansıtma veritabanı kullanabilirsiniz: <https://docs.microsoft.com/sql/database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql>

Bir öğretici, azure'da veritabanı yansıtma ayarlamak için burada bulunabilir: <https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server> 

### <a name="sql-server-always-on"></a>SQL Server her zaman açık
Always On şirket için SAP desteklenmediğinden (SAP notu bkz [1772688]), azure'da SAP ile birlikte desteklenir. SQL Server kullanılabilirlik grubu dinleyicisini (Azure kullanılabilirlik kümesi'ile karıştırılır olarak değil) dağıtımı geçici olarak bazı özel durumlar vardır. bu yana Azure bu anda olası şirket içinde olduğu gibi bir AD/DNS nesne oluşturmaya izin vermiyor. Bu nedenle, bazı farklı bir yükleme adımları Azure belirli davranışını üstesinden gelmek gereklidir.

Bir kullanılabilirlik grubu dinleyicisi kullanarak bazı hususlardır:

* Bir kullanılabilirlik grubu dinleyicisi kullanarak yalnızca Windows Server 2012 veya daha fazla sanal makinenin konuk işletim sistemi mümkündür. Windows Server 2012 için bu düzeltme ekini uygulandığından emin olmanız gerekir: <https://support.microsoft.com/kb/2854082> 
* Windows Server 2008 R2 için bu düzeltme eki yok ve Always On #içeren veritabanı yansıtma ile aynı şekilde belirterek bir yük devretme ortağı bağlantı dizesinde kullanılacak (SAP default.pfl parametresi dbs/mss/sunucu - Bitti luSAPnotunabakın[965908]).
* Bir kullanılabilirlik grubu dinleyicisi kullanırken, veritabanı VM'lerin ayrılmış bir yük dengeleyiciye bağlı gerekir. Azure burada iki VM'nin de bu arada kapatıldığından durumlarda yeni IP adresleri atama, önlemek için bir statik IP adresleri bu ağ arabirimleri için Vm'leri (statik IP adresi içindeaçıklanantanımlamaAlwaysOnyapılandırmaatamanızgerekir[bu] [ virtual-networks-reserved-private-ip] makale)
* Küme düğümü üzerinde oluşturulan küme adı aynı IP adresini geçerli işlevselliği ile Azure atadığınız çünkü küme özel bir IP adresi gereken yere WSFC küme yapılandırma atandığında, gerekli ek adımlar vardır. Bu, kümeye farklı bir IP adresi atamak için el ile gerçekleştirilmesi anlamına gelir.
* Kullanılabilirlik grubu dinleyicisi, kullanılabilirlik grubunun birincil ve ikincil çoğaltmalar çalıştıran sanal makinelere atanan TCP/IP'yi uç noktaları sayesinde, Azure'da oluşturulacak geçiyor.
* Bu uç nokta ACL'leri ile güvenli hale getirmek için bir gereksinim olabilir.

Azure Vm'lerde SQL Server ile Always On dağıtma ayrıntılı belgeler gibi listeler:

- [Karşınızda SQL Server Always On kullanılabilirlik grupları'Azure sanal makinelerinde](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview).
- [Farklı bölgelerdeki Azure sanal makinelerinde Always On kullanılabilirlik grubu yapılandırma](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-dr).
- [Azure'da bir Always On kullanılabilirlik grubu için bir yük dengeleyici yapılandırma](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener).

>[!NOTE]
> Kullanılabilirlik grubu dinleyicisinin sanal IP adresi için Azure load balancer'ı yapılandırıyorsanız, noktalarının DirectServerReturn yapılandırıldığından emin olun. Bu seçenek yapılandırıldığında, ağ gidiş dönüş gecikmesine SAP uygulama katmanı ve DBMS katmanı arasında azaltır. 

SQL Server Always On en yaygın kullanılan yüksek kullanılabilirlik ve olağanüstü durum kurtarma işlevi Azure'da SAP iş yükü dağıtımları için kullanılan ' dir. Çoğu müşteri her zaman tek bir Azure bölgesi içinde yüksek kullanılabilirlik için kullanabilirsiniz. Dağıtım, yalnızca iki düğüm sınırlı ise, bağlantı için iki seçeneğiniz vardır:

- Kullanılabilirlik grubu dinleyicisi kullanarak. Kullanılabilirlik grubu dinleyicisi ile bir Azure yük dengeleyici dağıtmak için gereklidir. Bu genellikle varsayılan dağıtım yöntemidir. SAP uygulamalarını kullanılabilirlik grubu dinleyicisi tek bir düğüm adlarla değildir ve bağlanmak için yapılandırılmış olması
- SQL Server veritabanı yansıtma bağlantı parametrelerini kullanarak. Bu durumda, SAP uygulama bağlantı burada her iki düğüm adları olarak da adlandırılır bir şekilde yapılandırmanız gerekir. Bu tür bir SAP yan yapılandırma hakkında tam Ayrıntılar, SAP Not belgelenen [#965908](https://launchpad.support.sap.com/#/notes/965908). Bu seçeneği kullanarak, bir kullanılabilirlik grubu dinleyicisi yapılandırma gerek yoktur. Ve ile hiçbir Azure yük dengeleyici için SQL Server yüksek kullanılabilirlik. Sonuç olarak, Azure load balancer gelen trafiği SQL Server örneğine yönlendirilmemesidir olduğundan SAP uygulama katmanı ve DBMS katmanı arasındaki ağ gecikme süresi düşüktür. Ancak geri çağırma, iki örnek yaymasına izin, kullanılabilirlik grubunuzun kısıtlamak isterseniz bu seçeneği yalnızca çalışır. 

Videonuz müşteriler, Azure bölgeleri arasında ek olağanüstü durum kurtarma işlevleri için SQL Server Always On işlevsellik yararlanıyor. Birden fazla müşteri özelliği ikincil çoğaltmasından Yedekleme gerçekleştirmek için de kullanabilirsiniz. 

## <a name="sql-server-transparent-data-encryption"></a>SQL Server saydam veri şifrelemesi
SQL Server kullanan müşteriler sayıda [saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) zaman kendi SAP SQL Server dağıtımı izin ver veritabanları Azure'da. SQL Server TDE işlevselliğini tamamen SAP tarafından desteklenir (SAP notu bkz [#1380493](https://launchpad.support.sap.com/#/notes/1380493)). 

### <a name="applying-sql-server-tde"></a>SQL Server TDE uygulanıyor
Heterojen geçiş şirket içi, Azure üzerinde çalışan Windows/SQL Server çalıştıran başka bir DBMS gelen gerçekleştirmek olduğu durumlarda, SQL Server, boş hedef veritabanında önceden oluşturmanız gerekir. Sonraki adım olarak, SQL Server TDE işlevselliğini uygular. Olsa da, üretim sistemi şirket içi hala çalışıyor. Bu sırayla gerçekleştirmek istediğiniz boş veritabanı şifreleme işleminin oldukça biraz sürebilir nedenidir. SAP alma işlemlerini ardından verilerin şifrelenmiş veritabanına kapalı kalma süresi aşamasında içeri aktarmanıza. Şifrelenmiş bir veritabanına aktarılması yükü daha sonra aşağı dışarı aktarma aşamasında veritabanı şifreleme bir şekilde daha düşük zaman etkisi zaman aşaması. Veritabanı üzerinde çalışan SAP iş yükü ile TDE uygulanmaya çalışılırken yapılan burada negatif karşılaşır. Bu nedenle, öneri TDE dağıtımını belirli veritabanı üzerinde SAP iş yükü olmadan yapılması gereken bir etkinlik olacak.

Burada, SAP SQL Server veritabanlarını şirket içinden Azure'a taşıyın durumlarda, hangi altyapı, hızlı uygulanan şifreleme alabilirsiniz test etmenizi öneririz. Bunun için bu bilgileri göz önünde bulundurun:

- Ne kadar iş parçacığı veritabanına veri şifrelemesi uygulamak için kullanılan tanımlayamazsınız. İş parçacığı sayısını majorly SQL Server veri ve günlük dosyaları, üzerinde dağıtılan disk birimlerinin sayısına bağlıdır. Daha fazla farklı birimler (sürücü harfleri) anlamına gelir. daha fazla iş parçacığı paralel olarak şifreleme gerçekleştirmek için bağlı. Bu tür bir yapılandırma biraz daha önceki disk yapılandırma öneri, bir veya daha az sayıda Azure vm'lerde SQL Server veritabanı dosyaları için depolama alanları oluşturma ile çelişiyor. Az sayıda birimler içeren bir yapılandırma için az sayıda şifreleme çalışan iş parçacıklarının sunulmasını sağlar. Şifreleme tek iş parçacığı 64 KB kapsamlarını okuma, şifreler ve bir kayıt kapsamı şifrelendi belirten işlem günlük dosyasına girilir yazın. Sonuç olarak hareket günlüğü yükü orta.
- Eski SQL Server sürümleri artık SQL Server veritabanınıza şifrelediğinizde yedekleme sıkıştırma verimliliği almadı. SQL Server veritabanını şirket içi şifrelenir ve azure'da veritabanını geri yüklemek için Azure yedekleme kopyalayın planınızı olduğunda bu davranış bir sorunla geliştirebilir. SQL Server Yedekleme sıkıştırma faktörü 4 sıkıştırma oranı genellikle ulaşır.
- SQL Server 2016 ile SQL Server şifreli veritabanlarına verimli bir şekilde de sıkıştırma izin veren yeni işlevler sunulur. Bkz: [bu blogları](https://blogs.msdn.microsoft.com/sqlcat/2016/06/20/sqlsweet16-episode-1-backup-compression-for-tde-enabled-databases/) bazı ayrıntılar için.
 
Uygulama olmadan az SAP iş yüküne yalnızca TDE şifreleme kullanarak, SAP veritabanı şirket içi için TDE uygulamak veya Azure'da Bunu yapmak için daha iyi olduğu belirli yapılandırmanızda test etmeniz gerekir. Azure'da, kesinlikle fazladan sağlama altyapı açısından daha fazla esnekliğe sahip olursunuz ve TDE uygulanan sonra altyapıyı Daralt.

### <a name="using-azure-key-vault"></a>Azure Key Vault kullanma
Azure'un sunduğu hizmet bir [Key Vault](https://azure.microsoft.com/services/key-vault/) şifreleme anahtarlarını depolamak için. SQL Server diğer tarafta TDE sertifika deposu olarak Azure anahtar kasası yararlanmak için bir bağlayıcı sunmaktadır.

Azure Key Vault için SQL Server TDE kullanmak için daha fazla ayrıntı listeler gibi:

- [Azure anahtar kasası (SQL Server) ile Genişletilebilir anahtar yönetimi](https://docs.microsoft.com/sql/relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server?view=sql-server-2017).
- [Azure anahtar kasası - kurulum adımlarını kullanarak SQL Server TDE Genişletilebilir anahtar yönetimi](https://docs.microsoft.com/sql/relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault?view=sql-server-2017).
- [SQL Server Bağlayıcısı Bakım ve sorun giderme](https://docs.microsoft.com/sql/relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting?view=sql-server-2017).
- [Müşteriler SQL Server saydam veri şifrelemesi – TDE + Azure anahtar kasası hakkında daha fazla soruya](https://blogs.msdn.microsoft.com/saponsqlserver/2017/04/04/more-questions-from-customers-about-sql-server-transparent-data-encryption-tde-azure-key-vault/).


>[!IMPORTANT]
>SQL Server TDE, özellikle Azure anahtarla kasası kullanarak, SQL Server 2014, SQL Server 2016 ve SQL Server 2017 en son düzeltme eklerinin kullanmak için önerilir. Neden olan müşteri geri bildirimleri temelinde, iyileştirmeler ve düzeltmeler koda uygulandı. Örnek olarak, kontrol [KBA #4058175](https://support.microsoft.com/help/4058175/tde-enabled-backup-and-restore-slow-if-encryption-key-is-stored-in-ekm).
>  

## <a name="9053f720-6f3b-4483-904d-15dc54141e30"></a>Genel SQL Server SAP on Azure özeti
Bu kılavuzda çok sayıda öneri ve birden çok kez Azure dağıtımınızı planlama önce okurken önerilir. Genel olarak, ancak üst genel DBMS Azure'a özel önerileri izlediğinizden emin olun:

1. Azure'da çoğu avantajları olan SQL Server 2017 gibi en son DBMS sürümü kullanın. 
2. Dikkatli bir şekilde Azure kısıtlamaları ve veri dosya düzeni dengelemek için azure'da SAP sistemi ortamınızı planlayın:
   * Yoksa çok fazla disk olmalıdır; ancak, gereken IOPS size ulaşabildiğimizden emin olmak için yeterli.
   * Yönetilen diskleri kullanmazsanız, IOPS ayrıca Azure depolama hesabı sınırlı olduğunu ve depolama hesapları her Azure aboneliğinde sınırlı olduğunu unutmayın ([daha fazla ayrıntı][azure-subscription-service-limits]). 
   * Yalnızca stripe disklerde bulunan daha yüksek bir performans sağlamak istiyorsanız.
3. Hiçbir zaman yazılımını yükleyin veya kalıcı olmayan ve herhangi bir şey bu sürücüdeki bir Windows önyükleme sırasında kaybolur D:\ sürücüsüne Kalıcılık gerektiren tüm dosyaları yerleştirin.
4. Azure standart depolama için diski önbelleğe alma işlemi kullanmayın.
5. Azure coğrafi olarak çoğaltılmış Azure standart depolama hesapları kullanmayın.  Yerel olarak yedekli DBMS iş yükleri için kullanın.
6. Veritabanı verileri çoğaltmak için DBMS satıcınızın HA/DR çözümü kullanın.
7. Her zaman ad çözümlemesini kullanmasına, IP adreslerini güvenmeyin.
8. SQL Server TDE kullanarak en son SQL Server düzeltme ekleri uygulayın.
9. Olası en yüksek veritabanı sıkıştırma kullanın. SQL Server için sayfa sıkıştırmayı olduğu.
10. SQL Server görüntüleri Azure Market'te kullanırken dikkatli olun. SQL Server birini kullanıyorsanız, herhangi bir SAP NetWeaver sistemini yüklemeden önce örneği harmanlamasıyla değiştirmeniz gerekir.
11. Yükleme ve yapılandırma bölümünde anlatıldığı gibi SAP izleme ana bilgisayarı için Azure [Dağıtım Kılavuzu][deployment-guide].
