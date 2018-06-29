---
title: SAP NetWeaver için Azure sanal makineleri DBMS dağıtımı | Microsoft Docs
description: SAP NetWeaver için Azure sanal makineleri DBMS dağıtımı
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: MSSedusch
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 5654dac7-4204-4387-b312-3d8b2898eb3a
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/26/2018
ms.author: sedusch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 79e77aa067cbb7262a945d94ce8ac1750e80b2d5
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37054798"
---
# <a name="azure-virtual-machines-dbms-deployment-for-sap-netweaver"></a>SAP NetWeaver için Azure sanal makineleri DBMS dağıtımı
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
[storage-premium-storage-preview-portal]:../../windows/premium-storage.md
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
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/en-us/resources/templates/sql-server-2014-alwayson-existing-vnet-and-ad/
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

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Bu kılavuz uygulama ve Microsoft Azure üzerinde SAP yazılım dağıtma konusunda belgelerine bir parçasıdır. Bu kılavuzu okumadan önce okuma [planlama ve Uygulama Kılavuzu][planning-guide]. Bu belge, çeşitli ilişkisel veritabanı yönetim sistemi (RDBMS) ve hizmet (Iaas) özellikleri Azure altyapısı kullanan ilgili ürünler SAP Microsoft Azure Virtual Machines'de (VM'ler) ile birlikte dağıtımını kapsar.

Kağıt yüklemeleri ve SAP yazılım dağıtımları için birincil kaynaklar üzerinde platformları temsil eden SAP notlar ve SAP yükleme belgelerini tamamlar.

## <a name="general-considerations"></a>Genel konular
Bu bölümde, SAP ilgili DBMS sistemleri Azure Vm'lerde çalışan konuları sunulur. Bu bölümde belirli DBMS sistemlere birkaç başvurular var. Bunun yerine belirli DBMS sistemlere sonra bu bölümde, bu makale içinde işlenir.

### <a name="definitions-upfront"></a>Önceden tanımları
Belge boyunca aşağıdaki terimleri kullanırız:

* Iaas: Hizmet olarak altyapı.
* PaaS: Hizmet olarak Platform.
* SaaS: Hizmet olarak yazılım.
* SAP bileşen: tek tek SAP gibi bir uygulama ECC, bant genişliği, çözüm Yöneticisi veya EP'deki  SAP bileşenleri geleneksel ABAP veya Java teknolojiler ya da olmayan-tabanlı NetWeaver uygulama iş nesneleri gibi temel alabilir.
* SAP ortamı: bir veya daha fazla SAP bileşenleri geliştirme, QAS, eğitim, DR veya üretim gibi işletme işlevini gerçekleştirmek için mantıksal olarak gruplandırılır.
* SAP yatay: Bu tüm müşteri'nin SAP varlıkları başvurduğu BT yatay. SAP yatay tüm üretim ve üretim dışı ortamlar içerir.
* SAP sistem: DBMS katman ve uygulama katmanı, örneğin, bir SAP ERP geliştirme sistemi, SAP BW test sistemini, SAP CRM üretim sistem vb. birleşimi. Azure dağıtımlarda, bu iki katmanı şirket içi ve Azure arasında bölmek için desteklenmiyor. Şirket içi SAP sistem ya da bu anlamına gelir dağıtılan veya Azure'da dağıtılır. Ancak, Azure veya şirket içi SAP yatay farklı sistemleri dağıtabilirsiniz. Örneğin, SAP CRM geliştirme dağıtmak ve Azure ancak SAP CRM üretim sistem şirket içi sistemleri test.
* Yalnızca bulut dağıtım: Burada Azure aboneliğine bağlı bir siteden siteye veya şirket içi ağ altyapısı için ExpressRoute bağlantısı aracılığıyla bir dağıtım. Ortak Azure belgelerine bu tür dağıtımlar da "Yalnızca bulut" dağıtımları açıklanmıştır. Bu yöntem ile dağıtılan sanal makineleri Internet'e ve Azure VM'ler için atanan ortak Internet uç noktaları aracılığıyla erişilir. Şirket içi Active Directory (AD) ve DNS Azure dağıtımları bu tür için genişletilmiş değil. Bu nedenle sanal makineleri şirket içi Active Directory'nin parçası değildir. Not: Bu belgede yalnızca bulut dağıtımları yalnızca Azure Active Directory veya ad çözümlemesi uzantısı olmadan şirket içi genel buluta çalıştıran tam SAP Windows'un olarak tanımlanır. Yalnızca bulut yapılandırmaları, üretim SAP sistemleri veya burada Azure ve şirket içi bulunan kaynaklar üzerinde barındırılan SAP sistemleri arasında kullanılacak SAP STMS veya diğer şirket kaynaklarına gereken yapılandırmalar için desteklenmez.
* Şirket içi: siteden siteye, çok siteli veya şirket içi inizdeki ve Azure arasında ExpressRoute bağlantısı bulunan bir Azure aboneliği için sanal makineleri dağıtıldığı bir senaryo açıklanmaktadır. Ortak Azure belgelerine dağıtımları bu tür şirket içi senaryoları açıklanmıştır. Bağlantı için Azure'da şirket içi etki alanları, şirket içi Active Directory ve şirket içi DNS genişletmek için nedenidir. Şirket içi yatay abonelik Azure varlıklar için genişletilir. Bu uzantı, sanal makineleri şirket içi etki alanının parçası olabilir. Şirket içi etki alanının etki alanı kullanıcıları sunucularına erişebilir ve hizmetler (ör. DBMS Hizmetleri) bu sanal makineleri üzerinde çalışabilir. Şirket içi iletişim ve ad çözümleme VM'ler arasında dağıtılır ve Azure'da dağıtılan VM'ler mümkündür. SAP varlıklar azure'da dağıtmak için en yaygın senaryo olması için bunu bekliyoruz. Daha fazla bilgi için bkz: [bu makalede] [ vpn-gateway-cross-premises-options] ve [bu makalede][vpn-gateway-site-to-site-create].

> [!NOTE]
> Şirket içi dağıtımları SAP sistemleri çalışan Azure sanal makineleri bir şirket içi etki alanının üyesi olduğu SAP sistemlerinin üretim SAP sistemleri için desteklenir. Şirketler arası yapılandırmalar bölümleri dağıtmak için desteklenen veya Azure'da SAP Windows'un tamamlayın. Bile tam SAP yatay Azure'da çalışan, şirket içi etki alanı ve REKLAM parçası olan bu VM'lerin olması gerektirir. Belge önceki sürümlerinde, biz karma BT senaryolar hakkında açıklandı burada terimi *karma* bir şirket içi bağlantılar şirket içi ve Azure arasında olduğunu aslında kökü belirtilmiş. Bu durumda *karma* da VM'ler için Azure'da şirket içi Active Directory'nin parçası olmadığı anlamına gelir.
> 
> 

Bazı Microsoft belgeleri şirketler arası senaryoları özellikle DBMS HA yapılandırmaları için biraz farklı bir şekilde açıklanmıştır. SAP ilgili belgeler söz konusu olduğunda, şirket içi senaryo boils bir siteden siteye veya özel (ExpressRoute) bağlantı aşağı kaydırın ve SAP yatay olgu dağıtıldığı şirket içi ve Azure arasında.

### <a name="resources"></a>Kaynaklar
Azure üzerinde SAP dağıtımları için aşağıdaki Kılavuzlar mevcuttur:

* [Azure sanal makineleri planlama ve uygulama SAP NetWeaver için][planning-guide]
* [SAP NetWeaver için Azure sanal makineler dağıtımı][deployment-guide]
* [SAP NetWeaver (Bu belgenin) için Azure sanal makineleri DBMS dağıtımı][dbms-guide]

Aşağıdaki SAP notları Azure üzerinde SAP ilgili:

| Not numarası | Unvan |
| --- | --- |
| [1928533] |Azure üzerinde SAP uygulamaları: desteklenen ürünler ve Azure VM türleri |
| [2015553] |Microsoft Azure üzerinde SAP: Destek önkoşulları |
| [1999351] |Gelişmiş Azure için SAP izleme sorunlarını giderme |
| [2178632] |Microsoft Azure üzerinde SAP için ölçümleri izleme anahtarı |
| [1409604] |Windows sanallaştırma: İzleme Gelişmiş |
| [2191498] |Azure ile Linux üzerinde SAP: İzleme Gelişmiş |
| [2039619] |Microsoft Oracle veritabanına kullanarak Azure SAP uygulamaları: desteklenen ürünleri ve sürümleri |
| [2233094] |DB6: Linux, UNIX ve Windows - ek bilgi için IBM DB2 kullanarak Azure SAP uygulamaları |
| [2243692] |Microsoft Azure (Iaas) VM Linux'ta: SAP lisans sorunları |
| [1984787] |SUSE LINUX Enterprise Server 12: Yükleme notları |
| [2002167] |Red Hat Enterprise Linux 7.x: yükleme ve yükseltme |
| [2069760] |Oracle Linux 7.x SAP yükleme ve yükseltme |
| [1597355] |Linux takas alanı önerisi |
| [2171857] |Oracle veritabanı 12c - Linux dosya sistemi desteği |
| [1114181] |Oracle veritabanı 11g - Linux dosya sistemi desteği |


Ayrıca okuma [SCN Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) , Linux için tüm SAP notlar içerir.

Microsoft Azure Mimarisi ve Microsoft Azure sanal makineleri nasıl dağıtılır ve işletilen hakkında bilgiye sahip olmalıdır. Daha fazla bilgi bulabilirsiniz <https://azure.microsoft.com/documentation/>

> [!NOTE]
> Ki **değil** Microsoft Azure platformu Microsoft Azure platformu bir hizmet (PaaS) teklifleri tartışma. Bu raporda, şirket içi ortamınızda DBMS çalışacak şekilde bir veritabanı yönetim sistemi (DBMS) Microsoft Azure sanal makineleri (Iaas) hakkında çalışıyor. Veritabanı özellikleri ve İşlevler bu iki teklifler arasında çok farklı olan ve birbirleri ile mixed olmalıdır değildir. Ayrıca bkz.: <https://azure.microsoft.com/services/sql-database/>
> 
> 

Biz Iaas ele olduğundan, genel olarak Windows, Linux ve DBMS yükleme ve yapılandırma temelde herhangi bir sanal makine veya şirket içi yüklemek çıplak metal makine aynıdır. Ancak, Iaas kullanırken farklı uygulama kararlarını bazı mimarisi ve sistem yönetimi vardır. Özel mimari ve sizin için Iaas kullanırken hazırlanmalıdır sistem yönetimi farklılıkları açıklamak için bu belgenin amacı budur.

Genel olarak, bu yazıda ele fark, genel alanlar şunlardır:

* SAP sistemleri uygun verileri sahip emin olmak için uygun VM/diskin yerleşimini planlama düzeni dosya ve işleminizi iş yükü için yeterli IOPS elde edebilirsiniz.
* Iaas kullanırken ağ durumları.
* Veritabanı düzeni en iyi duruma getirmek için kullanılacak belirli veritabanı özellikleri'ni kullanın.
* Iaas yedekleme ve geri yükleme konuları.
* Görüntü dağıtımı için farklı türleri kullanma.
* Azure Iaas yüksek kullanılabilirlik.

## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>Bir RDBMS dağıtım yapısı
Bu bölümde takip etmek için ne de sunulan anlamak ise gerekli [bu] [ deployment-guide-3] bölüm [Dağıtım Kılavuzu'na][deployment-guide]. Farklı bir VM dizisi ve farklar ve Azure standart ve Premium depolama farklar hakkında bilgi anlamış ve bu bölümü okumadan önce bilinen.

Mart 2015 kadar bir işletim sistemini içeren diskleri için 127 GB boyutunda sınırlı. Bu sınırlama Mart 2015'te kaldırılmış (daha fazla bilgi onay için <https://azure.microsoft.com/blog/2015/03/25/azure-vm-os-drive-limit-octupled/>). İşletim sistemini içeren disklerde buradan başka bir diske aynı boyutta olabilir. Bununla birlikte, biz yine bir işletim sistemi, DBMS ve nihai SAP ikili dosyaları veritabanı dosyalarından ayrı olduğu dağıtım yapısı tercih eder. Bu nedenle, SAP sistemleri Azure sanal makinelerde çalışan işletim sistemi, veritabanı yönetim sistemi yürütülebilir dosyalarının ve SAP yürütülebilir yüklü temel VM (veya disk) sahip bekliyoruz. DBMS veri ve günlük dosyalarını ayrı diskleri (standart veya Premium Storage) Azure depolama alanına depolanır ve mantıksal diskleri olarak özgün Azure işletim sistemi görüntüsü VM bağlı. 

(Örneğin DS serisi veya GS serisi VM'ler kullanarak) ve Azure standart veya Premium Storage yok yararlanarak üzerinde bağımlıdır belgelenen diğer Azure, kotalarda [burada (Linux)] [ virtual-machines-sizes-linux] ve [ Burada (Windows)][virtual-machines-sizes-windows]. Disk düzeni planlarken, kotaları şu öğeler için en iyi dengeyi bulmak gerekir:

* Veri dosyaları sayısı.
* Dosyaları içeren diskleri sayısı.
* Tek bir disk IOPS kotalar.
* Disk başına veri işleme.
* Ek veri disklerinin VM boyutu olası sayısı.
* Genel depolama üretilen işi, VM sağlayabilir.

Azure veri diski başına IOPS kota zorlar. Bu kotalar Azure Standard Storage ve Premium depolama üzerinde barındırılan diskler için farklıdır. G/ç gecikmeleri ayrıca Premium Etkenler daha iyi g/ç gecikmeleri teslim depolama ile iki depolama türleri arasında çok farklı değildir. Her biri farklı VM türler ekleyebileceği veri diskleri sınırlı sayıda sahiptir. Yalnızca belirli VM türleri Azure Premium Storage yararlanabilirsiniz başka bir kısıtlamadır. Başka bir deyişle, belirli bir VM türüne karar yalnızca aynı zamanda IOPS göre CPU ve bellek gereksinimlerini genellikle disk sayısı veya Premium depolama diskleri türüyle ölçeklenir gecikme süresi ve disk işleme gereksinimleri yönlendirilir değil. Özellikle Premium depolama ile bir disk boyutu da IOPS ve her disk tarafından elde edilebilir gerekiyor üretilen iş sayısına göre belirlenmesine.

Genel IOPS hızı, disk sayısını bağlanır ve VM boyutu tüm bağlı birlikte olgu bir Azure yapılandırması bir SAP sistemi kendi şirket içi dağıtım farklı olması neden olabilir. LUN başına IOPS sınırları şirket içi dağıtımlarda genellikle yapılandırılabilir. Azure Storage ile bu sınırları sabit veya disk türünün Premium depolama bağımlı olduğu gibi ise. Bu nedenle şirket içi dağıtımlar ile biz SAP ve DBMS veya geçici veritabanları veya tablo alanları için özel birimler gibi özel yürütülebilir dosyaları için birçok farklı birimler kullanarak veritabanı sunucuları müşteri yapılandırmalarını konusuna bakın. Bu tür bir şirket içi sistem Azure'a taşındığında, yürütülebilir dosyalar veya ya da çok fazla IOPS değil gerçekleştirmeyin veritabanları için bir disk israfı tarafından olası IOPS bant genişliği kaybı için neden olabilir. Bu nedenle, Azure Vm'lerde DBMS ve SAP yürütülebilir dosyaları işletim sistemi disk üzerinde mümkünse yüklü olmasını öneririz.

Veritabanı dosyalarını ve günlük dosyaları ve Azure kullanılan depolama türünü yerleşimini tanımlanmalıdır IOPS, gecikme ve verimlilik gereksinimleri tarafından. İşlem günlüğü için yeterli IOPS olması için işlem günlük dosyası için birden çok disk kullanın veya daha büyük bir Premium depolama diski kullanmak için zorunlu. Böyle bir durumda bir işlem günlüğü içeren disklerle yazılım RAID (için örnek Windows depolama havuzu için Windows veya MDADM ve LVM (mantıksal birim Yöneticisi) Linux için) oluşturmayı tercih.

- - -
> ![Windows][Logo_Windows] Windows
> 
> Bir Azure VM'ye D:\ Azure işlem düğümü üzerinde bazı yerel diskler tarafından yedeklenen bir kalıcı olmayan sürücü, sürücüdür. Kalıcı olmayan olduğundan, bu içerik D:\ sürücüsüne üzerinde yapılan değişiklikler kayıp VM yeniden başlatıldığında anlamına gelir. "Herhangi bir değişiklik" demek isteriz kaydedilen dosyalar, oluşturulan dizinleri, yüklü uygulamalar vb.
> 
> ![Linux][Logo_Linux] Linux
> 
> Linux Azure VM'ler, otomatik olarak Azure işlem düğümünde yerel disk ile desteklenir ve kalıcı olmayan bir sürücü /mnt/resource bir sürücü bağlayın. Kalıcı olmayan olduğundan bu VM yeniden başlatıldığında /mnt/resource içeriğinde yapılan tüm değişiklikler kaybedilir anlamına gelir. Herhangi bir değişiklik tarafından demek isteriz kaydedilen dosyaları, oluşturulan dizinleri, yüklü uygulamalar vb.
> 
> 

- - -
Azure VM dizisi bağımlı, hesaplama düğümünde yerel diskler gibi kategorilere farklı performans göster:

* A0-A7: Çok sınırlı performans. Windows disk belleği dosyası dışındaki her şey için kullanılabilir değil
* A8-A11: Bazı on bin IOPS ile çok iyi performans özellikleri ve > 1GB/sn verim
* D-serisi: Bazı on bin IOPS ile çok iyi performans özellikleri ve > 1GB/sn verim
* DS serisi: Bazı on bin IOPS ile çok iyi performans özellikleri ve > 1GB/sn verim
* G-serisi: Bazı on bin IOPS ile çok iyi performans özellikleri ve > 1GB/sn verim
* GS serisi: Bazı on bin IOPS ile çok iyi performans özellikleri ve > 1GB/sn verim

Yukarıdaki deyimleri, SAP ile sertifikalı VM türlerine uygulanıyor. Mükemmel IOPS ve üretilen iş ile VM dizisi, tempdb veya geçici tablo alanı gibi bazı DBMS özellikleri tarafından Dengeleme için uygun.

### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>VM'ler ve veri diskleri için önbelleğe alma
Veri diskleri portalı üzerinden veya biz VM'ler karşıya yüklenen disklere bağladığınızda oluşturuyoruz, biz olup olmadığını VM ve Azure depolama alanında bulunan bu disklerin arasındaki g/ç trafiği önbelleğe alınmış seçebilirsiniz. Azure standart ve Premium depolama önbellek bu tür için iki farklı teknoloji kullanın. Her iki durumda da, geçici disk VM tarafından (D:\ Windows) veya Linux'ta /mnt/resource kullanılan aynı sürücüler üzerinde yedeği disk önbelleği olacaktır.

İçin Azure Standard Storage olası önbellek türleri şunlardır:

* Önbelleğe alma
* Okuma
* Okuma ve yazma önbelleği

Tutarlı ve belirleyici performansı elde için Azure Standard Storage içeren tüm diskler için önbelleğe alma ayarlamanız **DBMS ilgili verileri dosyaları, günlük dosyalarını ve tablo alanı için 'NONE'**. VM önbelleğe almayı varsayılan kalabilir.

Azure Premium Storage için aşağıdaki önbelleğe alma seçenekler mevcuttur:

* Önbelleğe alma
* Okuma

Azure Premium Storage için önerilir yararlanmak için **veri dosyaları için okuma** SAP veritabanı ve seçtiğiniz **günlük dosyalarını diskler için önbelleğe alma**.

### <a name="c8e566f9-21b7-4457-9f7f-126036971a91"></a>Yazılım RAID
Zaten belirtilen yukarıdaki yapılandırabileceğiniz disk sayısı arasında veritabanı dosyaları için gerekli IOPS sayısını dengelemek için gerekir ve disk veya Premium depolama disk maksimum IOPS bir Azure VM sağlar. Diskler üzerinde IOPS yük ile ilgilenmenin en kolay yolu, farklı diskler üzerinde yazılım RAID oluşturmaktır. Ardından, SAP DBMS veri dosyalarının sayısını RAID yazılım dışında yontulmuş LUN'ları yerleştirir. Premium depolama alanı kullanımını da üç iki yana düşünmek isteyebilirsiniz gereksinimleri bağımlı farklı Premium depolama diskleri diskleri üzerinde standart depolama göre daha yüksek IOPS kota sağlar. Azure Premium Storage tarafından sağlanan önemli daha iyi g/ç gecikmesi yanı sıra. 

Aynı, farklı DBMS sistemleri işlem günlüğü için geçerlidir. DBMS sistemleri dosyalardan biri aynı anda yalnızca yazma beri bunların çoğu, daha fazla Tlog dosyaları ekleme ile korumaz. Tek bir standart tabanlı depolama disk sunabilir daha yüksek IOPS hızları gerekirse, birden çok standart depolama disk üzerinde şeritler veya Etkenler yazmak için daha düşük gecikme süresi de sağlar, daha yüksek IOPS oranları ötesinde daha büyük bir Premium depolama disk türü kullanabilirsiniz ı / İşlem günlüğüne işletim sistemi.

Yazılım RAID kullanarak favor, Azure dağıtımlarda yaşadı durumlar şunlardır:

* İşlem günlüğü/Yinele günlüğü Azure için tek bir disk sağladığından daha fazla IOPS gerektirir. Yukarıda belirtildiği gibi bu yazılım RAID kullanarak birden çok disk üzerinde bir LUN oluşturarak çözülebilir.
* Düzensiz g/ç iş yükü dağıtımı farklı veri dosyalarını üzerinden SAP veritabanı. Bu gibi durumlarda bir kota yerine genellikle basarsa bir veri dosyası yaşayabilirsiniz. Diğer veri dosyalarını tek bir disk IOPS kotanız dolmak bile alamıyorsanız ise. Bu gibi durumlarda en kolay çözüm bir LUN yazılım RAID kullanarak birden çok disk üzerinde oluşturmaktır. 
* Ne tam g/ç iş yükü her veri dosyası olduğunu ve yalnızca kabaca ne DBMS karşı genel IOPS iş yükü biliyor bilinmiyor. Yazılım RAID yardımıyla bir LUN oluşturmak için yapmak en kolay yoludur. Bu LUN'u arkasında birden çok disk kotaları toplamını sonra bilinen IOPS oranı karşılamak.

- - -
> ![Windows][Logo_Windows] Windows
> 
> Windows Server 2012 veya sonraki sürümünü çalıştırması durumunda Windows depolama alanları kullanmanızı öneririz. Daha önceki Windows sürümlerinde Windows şeritleme daha etkilidir. Windows depolama havuzlarını ve depolama alanları, işletim sistemi olarak Windows Server 2012 kullanırken, PowerShell komutlarıyla oluşturmanız gerekebilir. PowerShell komutları burada bulunabilir. <https://technet.microsoft.com/library/jj851254.aspx>
> 
> ![Linux][Logo_Linux] Linux
> 
> Yalnızca MDADM ve LVM (mantıksal birim Yöneticisi), bir yazılım RAID Linux'ta oluşturmak için desteklenir. Daha fazla bilgi için aşağıdaki makaleler okuyun:
> 
> * [Yazılım RAID Linux üzerinde yapılandırma] [ virtual-machines-linux-configure-raid] (için MDADM)
> * [Azure'da bir Linux VM LVM yapılandırın][virtual-machines-linux-configure-lvm]
> 
> 

- - -
Azure Premium Storage ile genellikle çalışabilmek için VM-serisi, yararlanarak dikkat edilmesi gereken noktalar şunlardır:

* SAN/NAS cihazları teslim yakın olan g/ç gecikmeleri talepleri.
* Azure Standard Storage sunabilir daha Etkenler daha iyi g/ç gecikmesi için isteğe bağlı.
* Belirli bir VM'ye karşı birden çok standart depolama disklerle ne elde edilebilir daha yüksek IOPS VM başına yazın.

Temel alınan Azure Storage, her disk en az üç depolama düğümlerine şeritleme kullanılabilir basit RAID 0 Yedeğiyle. RAID5 veya RAID1 uygulamak için gerek yoktur.

### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Microsoft Azure depolama
Microsoft Azure Storage (OS ile) temel VM ve diskleri veya BLOB'lar için en az üç ayrı depolama düğümleri depolar. Bir depolama hesabı veya yönetilen bir disk oluştururken yoktur koruma seçimine aşağıda gösterildiği gibi:

![Azure depolama hesabı için etkin coğrafi çoğaltma][dbms-guide-figure-100]

Azure Storage yerel çoğaltma (yerel olarak yedekli) dağıtmak için birkaç müşteri göze altyapı hatası nedeniyle veri kaybına karşı koruma düzeyi sağlar. Burada gösterildiği gibi bir beşinci dört farklı seçeneklerle ilk üç birini çeşitlemesi yükleniyor. Bunları yakın arayan biz ayırt edebilirsiniz:

* **Premium yerel olarak yedekli depolama (LRS)**: Azure Premium depolama g/Ç kullanımı yoğun iş yükleri çalıştıran sanal makineler için yüksek performanslı, düşük gecikmeli disk desteği sunar. Bir Azure bölgesi aynı Azure veri merkezi içinde verilerin üç çoğaltmalarının vardır. Farklı hata ve yükseltme etki alanları kopyalarıdır (kavramları görmek için [bu] [ planning-guide-3.2] bölümde [Planlama Kılavuzu][planning-guide]). Bir depolama düğüm hatası veya disk arızası nedeniyle hizmet dışına giderek veri çoğaltma olması durumunda, yeni bir çoğaltma otomatik olarak oluşturulur.
* **Yerel olarak yedekli depolama (LRS)**: Bu durumda, aynı Azure veri merkezinde bir Azure bölgesinin içindeki verilerin üç çoğaltmaları vardır. Farklı hata ve yükseltme etki alanları kopyalarıdır (kavramları görmek için [bu] [ planning-guide-3.2] bölümde [Planlama Kılavuzu][planning-guide]). Bir depolama düğüm hatası veya disk arızası nedeniyle hizmet dışına giderek veri çoğaltma olması durumunda, yeni bir çoğaltma otomatik olarak oluşturulur. 
* **Coğrafi olarak yedekli depolama (GRS)**: Bu durumda, üç çoğaltmalarını ek (örneğin, Kuzey Avrupa ve Batı Avrupa aynı coğrafi bölgede çalışmalarının çoğu olan başka bir Azure bölgesi veri akışlarını zaman uyumsuz bir çoğaltma yok ). Vardır; böylece altı çoğaltmaları toplamda bu üç ek yinelemede sonuçlanır. Bu bir çeşitlemesi, coğrafi çoğaltılan Azure bölgesinde veri okuma amacıyla (okuma erişimli coğrafi olarak yedekli) kullanıldığı bir ektir.
* **Bölge olarak yedekli depolama (ZRS)**: Bu durumda, aynı Azure bölgesinde verilerin üç çoğaltmalarının kalır. İçinde anlatıldığı gibi [bu] [ planning-guide-3.1] bölüm [Planlama Kılavuzu] [ planning-guide] bir Azure bölgesinin bir sayı veri merkezleri içinde yakın bir yerde konumlandırıldığında olabilir. LRS söz konusu olduğunda çoğaltmaları bir Azure bölgesi olun farklı veri merkezlerinde dağıtılması.

Daha fazla bilgi bulunabilir [burada][storage-redundancy].

> [!NOTE]
> DBMS dağıtımları için coğrafi olarak yedekli depolama kullanımı önerilmez.
> 
> Azure depolama coğrafi çoğaltma zaman uyumsuz olarak çağrılır. Tek bir VM'ye takılı tek tek disklerin çoğaltması kilit adımda eşitlenmemiş. Bu nedenle, farklı diskler üzerinde dağıtılmış veya bir yazılım üzerinde birden çok disk tabanlı RAID karşı dağıtılan DBMS dosyaları çoğaltmak uygun değil. DBMS yazılım kalıcı disk depolama farklı LUN'ları ve temel diskleri/dağılımı arasında tam olarak eşitlenir gerektirir. Sıralı g/ç yazma etkinlikleri için çeşitli mekanizmalar DBMS yazılım kullanır ve bu bile birkaç milisaniye göre farklılık gösterir, çoğaltma tarafından hedeflenen disk depolama bozuk bir DBMS raporlar. Birden çok diskte coğrafi olarak çoğaltılmış uzatılmış veritabanına sahip bir veritabanı yapılandırması isterse, bu nedenle bu tür bir çoğaltma veritabanı anlamına gelir ve işlevsellik ile gerçekleştirilmesi gerekir. Bir Azure depolama coğrafi bu işlemi gerçekleştirmek için çoğaltma üzerinde güvenmemelisiniz. 
> 
> Bir örnek sistemiyle açıklamak basit sorunudur. DBMS veri dosyalarını içeren sekiz diskleri ve işlem günlük dosyası içeren bir disk olan Azure karşıya bir SAP sistemine sahip varsayalım. Bu dokuz disklerin her biri olup onlara DBMS göre tutarlı bir yöntemi, yazılan veriler veri veri veya işlem günlük dosyalarına yazılır.
> 
> Verilerin düzgün coğrafi-çoğaltılması için sipariş ve tutarlı veritabanı görüntüsü korumak tüm dokuz diskleri içeriğini g/ç işlemleri dokuz farklı diskler karşı yürütülen tam sırayla coğrafi olarak çoğaltılmış olması gerekir. Ancak, Azure Storage coğrafi çoğaltma diskler arasındaki bağımlılıkları bildirmek için izin vermez. Bu Microsoft Azure Storage coğrafi çoğaltma bu dokuz farklı diskler içeriği birbiriyle ilişkili olduğunu ve yalnızca g/ç işlemleri sırada çoğaltma tüm oluştuğunda veri değişikliklerini tutarlı tanımadığı anlamına gelir. dokuz diskler.
> 
> Coğrafi olarak çoğaltılmış görüntüleri senaryoda tutarlı veritabanı görüntüsü sağlamaz yüksek olması olasılığını yanı sıra, aynı zamanda var. performansı önemli ölçüde etkileyebilir coğrafi olarak yedekli depolama ile görünür bir performans cezası Özet olarak, bu tür bir depolama artıklığı DBMS türü iş yükleri için kullanmayın.
> 
> 

#### <a name="mapping-vhds-into-azure-virtual-machine-service-storage-accounts"></a>Azure sanal makinesi hizmeti depolama hesaplarına veri VHD'ler eşleme
Bu bölüm, yalnızca Azure depolama hesapları için geçerlidir. Yönetilen diskleri kullanmayı planlıyorsanız, bu bölümde belirtildiği sınırlamaları uygulanmaz. Bölüm yönetilen diskler hakkında daha fazla bilgi için okuma [yönetilen diskleri] [ dbms-guide-managed-disks] bu kılavuzun.

Bir Azure depolama hesabı yalnızca bir yönetici yapısıyla, aynı zamanda bir konu sınırlamalar ' dir. Olup olmadığını biz Azure Standard Storage hesabını veya bir Azure Premium depolama hesabı hakkında konuşun üzerindeki kısıtlamaları değişir ancak. Tam özelliklerini ve sınırlamalarını listelenen [burada][storage-scalability-targets]

Azure Standard Storage için depolama hesabı başına IOPS üzerinde bir sınır olmadığını unutmayın önemlidir; böylece (satır içeren **toplam istek oranı** içinde [makaleyi][storage-scalability-targets]). Ayrıca, 100 depolama hesaplarının (itibariyle, Temmuz 2015) Azure abonelik başına ilk bir sınırı yoktur. Bu nedenle, VM IOPS Azure Standard Storage kullanırken birden çok depolama hesapları arasında dengelemek için önerilir. Mümkünse, ideal olarak kullanan bir depolama hesabı tek bir VM ise. Böylece biz Azure Standard Storage üzerinde barındırılan her VHD, kota sınırına burada ulaşabilir DBMS dağıtımları hakkında konuşun varsa, 30-40 VHD'ler Azure Standard Storage kullanır Azure depolama hesabı başına yalnızca dağıtmanız gerekir. Diğer taraftan, Azure Premium Storage yararlanır ve büyük veritabanı birimler depolamak istiyorsanız IOPS'ye göre daha iyi olabilir. Ancak Azure Premium Storage hesabını bir Azure standart depolama hesabı veri hacmi şekilde daha kısıtlayıcı. Sonuç olarak, veri birimi sınırı basarsa önce Azure Premium Storage hesabını içinde VHD'leri sınırlı sayıda yalnızca dağıtabilirsiniz. Sonunda, bir Azure depolama hesabı 'Sanal IOPS ve/veya kapasite özellikleri sınırlı sahip SAN' olarak düşünün. Sonuç olarak, görev, şirket içi dağıtımlar, Azure depolama hesapları ve farklı 'sanal SAN cihazları' farklı SAP sistemleri VHD'ler düzenini tanımlamak için olduğu gibi kalır.

Azure Standard Storage için mümkünse tek bir VM farklı depolama hesaplarından depolama sunmak için önerilmez.

DS veya GS serisi Azure VM'lerin kullanırken, Azure standart depolama hesapları ve Premium depolama hesapları dışında bağlama VHD'ler için mümkündür. Yedeklemeleri standart depolama alanına yazma yedeklenen VHD'leri ve DBMS verilere sahip olmak ve Premium depolama günlük dosyalarını gibi heterojen depolama burada işlevden aklınıza gelen gibi durumlarda kullanın. 

Müşteri dağıtımları ve test tek Azure standart depolama hesabı kabul edilebilir performans üzerinde veritabanı veri dosyalarını ve günlük dosyalarını içeren VHD sağlanabilir yaklaşık 30-40 göre. Daha önce belirtildiği gibi Azure Premium Storage hesabını içerebileceğinden veri kapasitesi ve değil IOPS büyük olasılıkla kısıtlamasıdır.

Olarak SAN cihazlar ile şirket içi, paylaşımı sonunda bir Azure depolama hesabı üzerinde performans sorunlarını algılamak üzere bazı izleme gerektirir. Azure Monitoring uzantısı SAP ve Azure portalı GÇ performansın teslim meşgul Azure depolama hesapları algılamak için kullanılan araçlar şunlardır.  Bu durum algılanırsa, başka bir Azure depolama hesabı meşgul sanal makineleri taşımak için önerilir. Başvurmak [Dağıtım Kılavuzu'na] [ deployment-guide] izleme yeteneklerini SAP etkinleştirme hakkında ayrıntılar barındırmak için.

En iyi uygulamalar Azure Standard Storage ve Azure standart depolama hesapları etrafında özetlemeye başka bir makaleye burada bulunabilir. <https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx>

#### <a name="f42c6cb5-d563-484d-9667-b07ae51bce29"></a>Yönetilen diskleri
Yeni bir kaynak türü Azure kaynağı Azure depolama hesaplarında depolanan VHD yerine kullanılan Yöneticisi'nde yönetilen disklerdir. Yönetilen diskleri otomatik olarak bağlı olan sanal makinenin kullanılabilirlik kümesi hizalamak ve bu nedenle, sanal makine ve sanal makinede çalışan hizmetlerin kullanılabilirliğini artırmak. Daha fazla bilgi edinmek için okuma [genel bakış makalesi](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview).

SAP şu anda yalnızca Premium yönetilen diskleri destekler. Okuma SAP Not [1928533] daha fazla ayrıntı için.

#### <a name="moving-deployed-dbms-vms-from-azure-standard-storage-to-azure-premium-storage"></a>Azure Premium Storage için Azure Standard Storage DBMS VM'lerin dağıtılan taşıma
Biz müşteri olarak dağıtılan VM Azure standart depolama alanından Azure Premium depolama alanına taşımak istediğiniz oldukça bazı senaryolar karşılaşırsınız. Azure depolama hesaplarında disklerinizi depolanırsa, bu verileri fiziksel olarak taşıyarak olmadan mümkün değildir. Hedefe ulaşmak için birçok yolu vardır:

* Yeni bir Azure Premium Storage hesabınıza tüm VHD'ler, temel VHD yanı sıra veri VHD'ler kopyalanamadı. Genellikle sayısı VHD'leri, veri hacmi gerekli değil olgu nedeniyle Azure Standard Storage seçtiniz. Ancak bu kadar sayıda VHD'ler nedeniyle IOPS gerekli. Azure Premium depolama birimine taşımak göre aynı IOPS verimliliği elde etmek için daha az VHD'ler işlemleriyle gidin. Azure standart kullanılan veri ve nominal disk boyutu için ödeme depolama biriminde, VHD sayısı bakımından maliyetleri önemli değil, olgu verilir. Ancak, Azure Premium Storage ile nominal disk boyutu ödeme. Bu nedenle, müşterilerin çoğu Azure VHD'ler sayısı Premium depolama IOPS verime ulaşmak için gereken numaradan gerekli tutmaya çalışın. Bu nedenle, müşterilerin çoğu 1:1 basit yol karşı kopyalama karar verin.
* Henüz olup, SAP veritabanınızın veritabanı yedeği içeren tek bir VHD bağlayın. Yedeklemeden sonra yedeği içeren VHD dahil olmak üzere tüm VHD'leri çıkarın ve temel VHD ve VHD yedekleme ile bir Azure Premium Storage hesabınıza kopyalayın. Ardından üzerinde temel VHD tabanlı VM dağıtmak ve yedekleme ile VHD'nin. Şimdi veritabanına geri yüklemek için kullanılan VM için ek boş Premium depolama diskleri oluşturun. Başka bir varsayar DBMS yolları veri ve günlük dosyaları geri yükleme işleminin bir parçası değiştirmenize olanak sağlar.
* Diğer bir olasılık, burada yedek VHD Azure Premium depolama alanına kopyalayın ve yeni dağıtılan ve yüklenen bir VM'ye karşı ekleme önceki işlem çeşididir.
* Dördüncü olasılığını içinde need veritabanınızın veri dosyalarının sayısını değiştirmek için olduğunda seçmeniz gerekir. Böyle bir durumda içeri/dışarı aktarma kullanarak bir SAP homojen sistem kopyası gerçekleştirmelisiniz. Put bu dosyaları bir Azure Premium Storage hesabına kopyalanır ve içeri aktarma işlemlerini çalıştırmak için kullandığınız bir VM'e ekleyin bir VHD'yi verin. Esas olarak veri dosyalarının sayısını azaltmak istedikleri zaman müşteriler bu olasılığı kullanın.

Yönetilen diskleri kullanırsanız, Premium Depolama tarafından geçirebilirsiniz:

1. Sanal makine serbest bırakma
1. Gerekirse, Premium Storage (örneğin DS veya GS) destekleyen bir boyut sanal makine yeniden boyutlandırma
1. Premium (SSD) Disk yönetilen hesap türünü değiştirme
1. Veri disklerinin bölümde önerildiği önbelleğe alma değiştirmek [VM'ler ve veri diskleri için önbelleğe alma][dbms-guide-2.1]
1. Sanal makineyi Başlat

### <a name="deployment-of-vms-for-sap-in-azure"></a>VM dağıtımı Azure SAP için
Microsoft Azure sanal makineleri ve ilişkili diskleri dağıtmak için birden çok yol sunar. Böylece VM'lerin hazırlıklar dağıtım yol bağımlı değişebilir beri farkları anlamak önemlidir. Genel olarak, biz aşağıdaki bölümlerde açıklanan senaryoları içine bakın.

#### <a name="deploying-a-vm-from-the-azure-marketplace"></a>Azure Marketi'nden VM dağıtma
VM dağıtmak üzere Microsoft veya üçüncü taraf Azure Market görüntüsünden sağlanan yapmak. Azure VM dağıtımı sonra aynı kılavuzları ve bir şirket içi ortamda yaptığınız gibi VM içinde SAP yazılımı yüklemek için Araçlar izleyin. Azure VM, SAP ve Microsoft içinde SAP yazılımını yüklemek için yüklemeyi önerilir ve SAP yükleme medyasını diskler veya 'tüm gerekli SAP yükleme medyasını içeren bir dosya sunucusu olarak', çalışan bir Azure VM oluşturmak için saklayın.

#### <a name="deploying-a-vm-with-a-customer-specific-generalized-image"></a>VM genelleştirilmiş bir müşteriye özgü yansıma ile dağıtma
İşletim sistemi veya DBMS sürümünüz ile ilgili belirli düzeltme eki gereksinimleri nedeniyle Azure Marketi sağlanan görüntüleri gereksinimlerinize değil. Bu nedenle, daha sonra birkaç kez dağıtılabilir kendi 'özel' işletim sistemi/DBMS VM görüntüsü kullanarak bir VM oluşturma gerekebilir. 'Özel' bir görüntüsü çoğaltma için hazırlamak üzere işletim sistemi şirket içi VM genelleştirilmiş gerekir. Başvurmak [Dağıtım Kılavuzu'na] [ deployment-guide] VM generalize hakkında ayrıntılar için.

(Özellikle 2 katmanlı sistemler için), şirket içi VM'deki SAP içerik zaten yüklediyseniz, örnek aracılığıyla Azure VM dağıtımı yeniden adlandırdıktan sonra SAP yazılım sağlama Yöneticisi (SAP tarafından desteklenen yordamı SAP sistem ayarlarını uyarlayabilirsiniz Not [1619720]). Aksi takdirde, daha sonra Azure VM Dağıtım sonrasında SAP yazılım yükleyebilirsiniz.

SAP uygulama tarafından kullanılan veritabanı içerik, ya da içerik yeni bir SAP yüklemesiyle oluşturabileceğiniz veya içeriğinizi VHD bir DBMS veritabanı yedeğini kullanarak veya içine doğrudan yedeklemek için DBMS özelliklerini yararlanarak Azure aktarabilirsiniz  Microsoft Azure depolama. Bu durumda, ayrıca VHD'ler DBMS veri ve günlük dosyaları ile şirket içi hazırlamak ve bu diskleri olarak Azure'a aktarabilir. Ancak şirket içinden Azure'a yüklendiğinden DBMS veri aktarımını şirket içi hazırlıklı olmak için gereken VHD diskler üzerinde çalışır.

#### <a name="moving-a-vm-from-on-premises-to-azure-with-a-non-generalized-disk"></a>Bir VM şirket içinden Azure'a genelleştirilmiş olmayan bir diskle taşıma
Belirli bir SAP sistemi şirket içinden Azure'a (yükseltme ve shift) taşımak planlayın. Bu, Azure DBMS veri ve günlük dosyaları ile işletim sistemi, SAP ikili dosyaları ve nihai DBMS ikili dosyaları içeren disk artı diskleri yükleyerek yapılabilir. Şirket içi ortamda yapılandırılmış gibi yukarıda Senaryo #2 ters yönde, ana bilgisayar adı, SAP SID ve SAP kullanıcı hesapları Azure VM'yi bulundurun. Bu nedenle, görüntünün genelleme gerekli değildir. Bu durumda, çoğunlukla SAP yatay parçası şirket içi ve bölümleri Azure üzerinde çalıştırıldığı şirketler arası senaryolar için geçerlidir.

## <a name="871dfc27-e509-4222-9370-ab1de77021c3"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma Azure VM'ler ile
SAP ve DBMS dağıtımları için kullanırız farklı bileşenler için uygulanacak aşağıdaki yüksek kullanılabilirlik (HA) ve olağanüstü durum kurtarma (DR) işlevlerini, Azure sunar

### <a name="vms-deployed-on-azure-nodes"></a>Azure düğümleri üzerinde dağıtılan VM'ler
Azure platformu, dağıtılan VM'ler için dinamik geçiş gibi özellikler sunmaz. Başka bir deyişle, varsa bakım gerekli bir VM dağıtıldığı bir sunucu kümesinde, VM durdurulur ve yeniden gerekir. Azure bakım kullanarak, bu nedenle sunucuları kümeleri içinde yükseltme etki alanları olarak adlandırılan gerçekleştirilir. Bir seferde yalnızca bir yükseltme etki sağlanıyor. Bu tür bir yeniden başlatma sırasında var. bir hizmet kesintisi VM'yi kapatın, bakım gerçekleştirilir ve VM yeniden Çoğu DBMS satıcısı ancak birincil düğüm kullanılamıyorsa, hızlı bir şekilde başka bir düğümde DBMS hizmetleri yeniden başlatır yüksek kullanılabilirlik ve olağanüstü durum kurtarma işlevsellik sağlar. Azure platformu VM'ler, depolama ve diğer Azure hizmetleriyle yükseltme planlanan Bakım veya altyapı hataları yalnızca küçük bir alt kümesini sanal makineleri veya hizmetleri etkileyebilecek emin olmak için etki alanları arasında dağıtmak için işlevsellik sağlar.  Dikkatli planlama ile şirket içi altyapılar karşılaştırılabilir kullanılabilirlik düzeyleri elde etmek mümkündür.

Microsoft Azure kullanılabilirlik kümeleri VM'ler mantıksal bir gruplandırması olan veya yalnızca sağlayacak şekilde bir düğümün kapanması herhangi bir noktada ( OkumazamanındaVM'lersağlarHizmetlerivediğerhizmetlerifarklıhataveyükseltmeetkialanlarıiçinbirkümeiçindedağıtılır[bu (Linux)] [ virtual-machines-manage-availability-linux] veya [bu (Windows)] [ virtual-machines-manage-availability-windows] daha fazla ayrıntı için makale).

Amacına göre burada görüldüğü gibi VM'ler sunulurken yapılandırılmış olması gerekir:

![Kullanılabilirlik kümesi'nin bir tanımı DBMS HA yapılandırmaları için][dbms-guide-figure-200]

Biz DBMS dağıtımlarını yüksek oranda kullanılabilir yapılandırmaları (bağımsız olarak kullanılan tek tek DBMS HA işlevleri) oluşturmak istiyorsanız, DBMS VM'ler gerekir:

* Aynı Azure sanal ağa VM'ler ekleyin (<https://azure.microsoft.com/documentation/services/virtual-network/>)
* HA yapılandırma VM'ler de aynı alt ağda olmalıdır. Ad çözümlemesi farklı alt ağlar arasında yalnızca bulut dağıtımında, yalnızca IP çözümleme works mümkün değildir. Şirket içi dağıtımlar için siteden siteye veya ExpressRoute bağlantısı kullanarak en az bir alt ağla zaten oluşturulur. Ad çözümlemesi, şirket içi göre yapılır AD ilkeleri ve ağ altyapısı. 



#### <a name="ip-addresses"></a>IP Adresleri
VM'ler HA yapılandırmaları için esnek bir şekilde ayarlamak için önerilir. HA yapılandırma içinde HA ortaklarınızın adres için IP adreslerini güvenmek, statik IP adresleri kullanılmadığı sürece Azure'da güvenilir değil. Azure'da iki "Kapat" kavram vardır:

* Azure portalı üzerinden veya Azure PowerShell cmdlet'ini Stop-AzureRmVM kapatma: Bu durumda, sanal makine kapatma alır ve XML'deki ayrılmış. Kullanılan depolama alanı kullanan yalnızca ücretleri; bu nedenle Azure hesabınız artık bu VM için ücretlendirilir. Ancak, özel IP adresi ağ arabiriminin statik olduysa, IP adresi serbest ve bu ağ arabirimini yeniden VM yeniden sonra atanan eski IP adresi alır garanti edilmez. Azure portalı üzerinden veya Stop-AzureRmVM otomatik olarak çağırarak aşağı kapatma gerçekleştirmek ayırmayı neden olur. Stop-AzureRmVM - StayProvisioned makinesi kullanımı ayırması istemiyorsanız 
* Bir işletim sistemi düzeyi VM'yi kapatın, VM kapatılır ve XML'deki tahsis. Ancak, bu durumda, Azure hesabınız hala kapatma olduğunu olgusuna karşın VM için ücretlendirilir. Böyle bir durumda, durmuş bir VM'yi için IP adresi ataması değişmeden kalır. İçinde VM'den kapatma otomatik olarak ayırmayı zorlamaz.

Şirketler arası senaryolar için bile, varsayılan olarak bir kapatma ayırmayı anlamına gelir ve IP adreslerinin devre dışı bırakma atama sanal makineden DHCP ayarları şirket içi ilkelerinde farklı olsa bile. 

* Özel durum bir ağ arabirimi olarak için statik bir IP adresi atarsa açıklanan [burada][virtual-networks-reserved-private-ip].
* Ağ arabirimi silinmez sürece böyle bir durumda, IP adresi sabit kalır.

> [!IMPORTANT]
> Tüm dağıtım basit ve yönetilebilir tutmak için açık bir DBMS HA veya DR yapılandırmasında Azure içinde ilgili farklı VM'ler arasında işlevsel bir ad çözümlemesi olacak şekilde ortaklık VM'ler kurulum önerilir.
> 
> 

## <a name="deployment-of-host-monitoring"></a>İzleme ana bilgisayarı dağıtımı
SAP uygulamaları Azure Virtual Machines'de verimli kullanımı SAP Azure sanal makineleri çalıştıran fiziksel ana verileri izleme ana bilgisayarı alma yeteneğini gerektirir. Belirli bir SAP konak Aracısı düzeltme eki düzeyi gereklidir SAPOSCOL ve SAP konak Aracısı bu yeteneği sağlar. Tam düzeltme eki düzeyine SAP notta belgelenen [1409604].

Ana verileri SAPOSCOL ve SAP konak Aracısı teslim bileşenleri dağıtımını ve bu bileşenlerin yaşam döngüsü yönetimi ile ilgili ayrıntılar için başvurmak [Dağıtım Kılavuzu][deployment-guide]

## <a name="3264829e-075e-4d25-966e-a49dad878737"></a>Microsoft SQL Server özellikleri
### <a name="sql-server-iaas"></a>SQL Server IaaS
Microsoft Azure ile başlayarak, var olan SQL Server uygulamalarınızı Azure sanal makineler için Windows Server platformunda yerleşik kolayca geçirebilirsiniz. Bir sanal makinede SQL Server, Microsoft Azure bu uygulamaları kolayca geçiş yaparak toplam sahip olma dağıtım, yönetim ve Bakım Kurumsal avantajlarına uygulama maliyetini azaltmak sağlar. Bir Azure sanal makinesinde SQL Server ile yöneticiler ve geliştiriciler aynı geliştirme ve mevcut şirket içi yönetim araçlarını kullanmaya devam edebilirsiniz. 

> [!IMPORTANT]
> Biz Microsoft Azure SQL veritabanı, Microsoft Azure platformu bir hizmet teklifi bir Platform olduğu ele değil. Bu yazıdaki tartışma Azure sanal altyapı Azure bir hizmet özelliği yararlanarak Machines'de, şirket içi dağıtımlar için bilinen SQL Server ürün hakkında çalışıyor. Veritabanı özellikleri ve İşlevler bu iki teklifler arasında farklı ve birbirleri ile mixed olmalıdır değildir. Ayrıca bkz.: <https://azure.microsoft.com/services/sql-database/>
> 
> 

Gözden geçirmek için önerilen [bu] [ virtual-machines-sql-server-infrastructure-services] devam etmeden önce belgeleri.

Aşağıdaki bölümlerde yukarıdaki bağlantıyı altında belge bölümlerini parçaları bir araya getirilir ve belirtilen. SAP ile ilgili detaylar da açıklanan ve bazı kavramları daha ayrıntılı olarak açıklanmıştır. Ancak, SQL Server'a özgü belgeleri okuma önce ilk yukarıda belgeleri ile çalışmak için önerilir.

Iaas belirli bilgiler devam etmeden önce bilmeniz gereken bazı SQL Server vardır:

* **Sanal makine SLA**: Burada bulunabilir Azure'da çalışan sanal makineler için bir SLA vardır: <https://azure.microsoft.com/support/legal/sla/>  
* **SQL sürüm desteği**: SAP müşteriler için desteklediğimiz SQL Server 2008 R2 ve Microsoft Azure sanal makinesi üzerinde daha yüksek. Önceki sürümleri desteklenmez. Bu genel gözden [destek deyimi](https://support.microsoft.com/kb/956893) daha fazla ayrıntı için. Genel SQL Server 2008 Microsoft tarafından da desteklendiğini unutmayın. SQL Server 2008 R2 ile sunulan, SAP için önemli işlevsellikler nedeniyle SQL Server 2008 R2'in en düşük sürüm SAP için ancak. SQL Server 2012 ve 2014 (doğrudan Azure Storage karşı yedekleme gibi) Iaas senaryosu içine daha derin Tümleştirmesi ile genişletilmiş aklınızda bulundurun. Bu nedenle, biz Bu raporda SQL Server 2012 ve en son düzeltme eki düzeyiyle 2014 Azure için kısıtlayın.
* **SQL özellik desteği**: en SQL Server özellikleri, bazı özel durumlar ile Microsoft Azure sanal makinelerinde desteklenir. **SQL Server Paylaşılan diskleri kullanarak Yük Devretme Kümelemesi desteklenmiyor**.  Veritabanı yansıtma, AlwaysOn Kullanılabilirlik grupları, çoğaltma, günlük aktarma ve hizmet Aracısı tek bir Azure bölge içinde desteklenen gibi teknolojileri dağıtılmış. SQL Server AlwaysOn de desteklenir farklı Azure bölgeler arasında burada açıklandığı gibi: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.  Gözden geçirme [destek deyimi](https://support.microsoft.com/kb/956893) daha fazla ayrıntı için. AlwaysOn yapılandırmasını dağıtma konusunda bir örnek gösterilen [bu] [ virtual-machines-workload-template-sql-alwayson] makalesi. Ayrıca, en iyi belgelenen uygulamaları denetleyin [burada][virtual-machines-sql-server-infrastructure-services] 
* **SQL performans**: Microsoft Azure barındırılan sanal makinelerin gerçekleştirmek çok iyi diğer genel bulut sanallaştırma teklifleri ancak tek tek sonuç kıyasla değişebilir emin duyuyoruz. Kullanıma [bu] [ virtual-machines-sql-server-performance-best-practices] makalesi.
* **Azure Marketi'nden görüntüleri kullanarak**: yeni bir Microsoft Azure VM dağıtmak için en hızlı yolu Azure Market'teki bir görüntü kullanmaktır. SQL Server içeren Azure Market görüntülerini vardır. SQL Server zaten yüklü görüntüleri hemen SAP NetWeaver uygulamaları için kullanılamaz. Varsayılan SQL Server Harmanlama bu görüntüleri ve SAP NetWeaver sistemleri tarafından gerekli harmanlama içinde yüklü nedenidir. Bu tür görüntüleri kullanmak için bölümde belgelenen adımları denetleyin [Microsoft Azure Market dışında bir SQL sunucu görüntüsü kullanarak][dbms-guide-5.6]. 
* Kullanıma [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/) daha fazla bilgi için. [SQL Server 2012 lisans Kılavuzu](https://download.microsoft.com/download/7/3/C/73CAD4E0-D0B5-4BE5-AB49-D5B886A5AE00/SQL_Server_2012_Licensing_Reference_Guide.pdf) ve [SQL Server 2014 lisans Kılavuzu](https://download.microsoft.com/download/B/4/E/B4E604D9-9D38-4BBA-A927-56E4C872E41C/SQL_Server_2014_Licensing_Guide.pdf) de önemli bir kaynaktır.

### <a name="sql-server-configuration-guidelines-for-sap-related-sql-server-installations-in-azure-vms"></a>SAP ilgili SQL Server yüklemelerini Azure VM'ler için SQL Server yapılandırma yönergeleri
#### <a name="recommendations-on-vmvhd-structure-for-sap-related-sql-server-deployments"></a>SAP ilgili SQL Server dağıtımları için VM/VHD yapısı hakkında öneriler
Genel Açıklama uygun olarak SQL Server yürütülebilir dosyalar bulunabilir veya sanal makinenin işletim sistemi diski sistem sürücüsüne yüklü (C: sürücüsü\).  Genellikle, SQL Server sistem veritabanlarının en yüksek düzeyde SAP NetWeaver iş yükü tarafından kullanılmaz. Bu nedenle (master, msdb ve model) SQL Server'ın sistem veritabanları C:\ sürücüsünü de kalabilir. Bir özel durum, bazı SAP ERP ve tüm BW iş yükleri, söz konusu olduğunda yüksek veri birimi veya özgün VM'de oturum sığamıyorsa g/ç işlemleri birim gerektirebilir tempdb olabilir. Bu tür sistemler için aşağıdaki adımlar gerçekleştirilmelidir:

* Birincil tempdb veri dosyaları SAP veritabanının birincil veri dosyaları aynı mantıksal sürücüye taşıyın.
* Tüm ek tempdb veri dosyaları her bir veri dosyası SAP kullanıcı veritabanının içeren diğer mantıksal sürücüler ekleyin.
* Kullanıcı veritabanı günlük dosyası içeren bir mantıksal sürücü tempdb günlük dosyası ekleyin.
* **Yalnızca yerel SSD kullanmak VM türleri için** işlem düğümü tempdb veri ve günlük dosyaları D:\ sürücüsüne yerleştirilmiş olabilir. Bununla birlikte, birden çok tempdb veri dosyalarını kullanmak için önerilir. VM türüne göre D:\ sürücüsüne birimlerin farklı unutmayın.

Bu yapılandırmalar, sistem sürücüsünün sağlamak mümkün olandan daha fazla alanı kullanmak üzere tempdb etkinleştirin. Uygun tempdb boyutunu belirlemek için bir şirket içi varolan sistemler tempdb boyutlarına kontrol edebilirsiniz. Ayrıca, bu tür bir yapılandırma ile sistem sürücüsünün sağlanamaz tempdb karşı IOPS numaraları etkinleştirir. Yeniden, şirket içi çalıştıran sistemler, böylece, tempdb üzerinde görmeyi beklediğiniz IOPS numaraları türetilemeyeceğini tempdb karşı g/ç iş yükünü izlemek için kullanılabilir.

Bir SAP veritabanı ile SQL Server çalıştığı ve tempdb veri ve günlük dosyası tempdb D:\ sürücüsüne nereye yerleştirileceğini bir VM yapılandırması gibi görünür:

![Azure Iaas VM SAP için başvuru yapılandırması][dbms-guide-figure-300]

D:\ sürücüsüne farklı boyutlarda VM türüne bağımlı olduğunu unutmayın. Tempdb, boyut gereksinimini bağımlı, D:\ sürücüsüne çok küçük olduğu durumlarda SAP veritabanı veri ve günlük dosyaları ile çifti tempdb veri ve günlük dosyalarını işlem.

#### <a name="formatting-the-disks"></a>Diskleri biçimlendirme
SQL Server için NTFS blok boyutu SQL Server verilerini içeren disklerin ve günlük dosyalarını 64 K olmalıdır. D:\ sürücüsüne biçimlendirmek için gerek yoktur. Bu sürücü önceden biçimlendirilmiş gelir.

Geri yükleme veya veritabanları oluşturmayı veri dosyalarını dosyaların içeriğini sıfırlama tarafından başlatılıyor değil, emin olmak için bir SQL Server hizmetini çalıştıran kullanıcı bağlamı belirli bir iznine sahip olduğunu emin olmanız gerekir. Genellikle Windows Yönetici grubundaki kullanıcılar bu izinlere sahip. SQL Server hizmeti Windows yönetici olmayan bir kullanıcı kullanıcı bağlamında çalıştırırsanız, kullanıcı hakkı bu kullanıcı atamanız gerekir **Birim bakım görevlerini gerçekleştirme**.  Bu Microsoft Bilgi Bankası makalesi ayrıntıları bakın: <https://support.microsoft.com/kb/2574695>

#### <a name="impact-of-database-compression"></a>Veritabanı Sıkıştırma etkisi
Burada g/ç bant genişliği sınırlayıcı bir etmen haline gelebilir yapılandırmalarında IOPS azaltır her ölçü bir Azure gibi bir Iaas senaryosu çalıştırabilirsiniz iş yükü uzatmak için yardımcı olabilir. Bu nedenle, henüz yapıldığında, SQL Server sayfasında sıkıştırma uygulama SAP ve Microsoft tarafından var olan bir SAP veritabanı için Azure karşıya yüklemeden önce önerilir.

Azure için karşıya yüklemeden önce veritabanı sıkıştırma gerçekleştirmek için öneri dışında iki nedenleri verilmiştir:

* Karşıya yüklenecek veri miktarını düşüktür.
* G/ç gecikmesi şirket içi daha az veya daha fazla CPU veya daha yüksek g/ç bant genişliği ile daha güçlü donanım kullanabilirsiniz varsayılarak sıkıştırma yürütme süresi kısadır.
* Daha küçük veritabanı boyutları disk ayırma için daha az maliyetlere neden olabilir

Şirket içi yaptığı gibi veritabanı sıkıştırma de bir Azure sanal makinelerde çalışır. Varolan bir SAP SQL Server veritabanını Sıkıştır konusunda daha fazla ayrıntı için burayı tıklatın: <https://blogs.msdn.com/b/saponsqlserver/archive/2010/10/08/compressing-an-sap-database-using-report-msscompress.aspx>

### <a name="sql-server-2014---storing-database-files-directly-on-azure-blob-storage"></a>SQL Server 2014 - depolanmasını veritabanı dosyalarını doğrudan Azure Blob Depolama
SQL Server 2014 doğrudan Azure Blob deposu VHD etrafında 'sarmalayıcı' olmadan veritabanı dosyaları depolamak için olasılığını açılır. Standart Azure depolama veya daha küçük VM türleri kullanmaya özellikle bu bazı küçük VM türlerine bağlı diskler, sınırlı sayıda tarafından uygulanabiliyordu IOPS sınırları burada üstesinden senaryolara olanak sağlar. Bu, ancak SQL Server'ın sistem veritabanları için kullanıcı veritabanlarını için çalışır. Ayrıca, SQL Server'ın veri ve günlük dosyaları için çalışır. Bir SAP SQL Server veritabanını dağıtmak isteyip istemediğinizi 'kaydırma' yerine bu şekilde VHD'ler, içine aşağıdakileri göz önünde bulundurun:

* Depolama hesabı gereksinimlerini VM SQL Server dağıtmak için kullanılan bir çalışan aynı Azure bölgesinde olması için kullanılır.
* VHD dağıtım ile ilgili farklı Azure depolama hesapları daha önce listelenen konular dağıtımları da bu yöntem için geçerlidir. Azure depolama hesabı sınırları karşı g/ç işlemlerinin sayısı anlamına gelir.

[comment]: <> (MSSedusch Yapılacaklar ancak yoktur bunu ağ bant genişliği ve depolama bant genişliği değil, kullanacaksınız?)

Bu dağıtım türü hakkındaki ayrıntıları aşağıdadır: <https://docs.microsoft.com/sql/relational-databases/databases/sql-server-data-files-in-microsoft-azure>

SQL Server doğrudan Azure Premium Storage veri dosyalarını depolamak için burada belgelenen en az bir SQL Server 2014 düzeltme eki sürümü gerekir: <https://support.microsoft.com/kb/3063054>. SQL Server veri dosyalarını Azure Standard Storage depolama yayımlanmış sürümü SQL Server 2014 ile çalışır. Ancak, çok aynı düzeltme eklerini başka bir dizi SQL Server veri dosyaları ve yedekler için Azure Blob Storage'nın doğrudan kullanım daha güvenilir hale düzeltmeleri içerir. Bu nedenle bu düzeltme ekleri genel kullanmanızı öneririz.

### <a name="sql-server-2014-buffer-pool-extension"></a>SQL Server 2014 arabellek havuzu genişletme
SQL Server 2014 arabellek havuzu genişletme olarak adlandırılan yeni bir özellik sunulmuştur. Bu işlev bir sunucunun yerel SSD veya VM tarafından yedeklenen bir ikinci düzey önbellek ile bellekte tutulur SQL Server'ın arabellek havuzu genişletir. Bu, verileri daha büyük bir çalışma kümesine 'bellekte' tutmak için sağlar. Azure Standard Storage erişimi karşılaştırıldığında bir Azure VM yerel SSD'ler üzerinde depolanan arabellek havuzu uzantısı Access'e birçok faktöre daha hızlıdır.  Bu nedenle, yerel D:\ sürücüsüne mükemmel IOPS ve üretilen iş sahip VM türlerinden yararlanarak, Azure Storage'a karşı IOPS yükü azaltmak ve sorguların yanıt sürelerini önemli ölçüde artırmak için çok makul bir yolu olabilir. Bu, özellikle Premium depolama kullanıldığında geçerlidir. Premium depolama ve hesaplama düğümünde Premium Azure okuma önbelleği kullanımını durumunda, veri dosyaları için önerilen gibi herhangi bir önemli fark beklenir. Her iki önbellekleri (SQL Server arabellek havuzu genişletme ve Premium depolama okuma önbelleği) işlem düğümünün yerel diskler kullanıyorsanız nedenidir.
Bu işlevsellik hakkında daha fazla ayrıntı için bu belgelere bakın: <https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension> 

### <a name="backuprecovery-considerations-for-sql-server"></a>SQL Server için yedekleme/kurtarma konuları
SQL Server Azure'a dağıtırken yedekleme yönteminize gözden geçirilmesi gerekir. Sistem üretken sistem olsa bile, SQL Server tarafından barındırılan SAP veritabanında düzenli aralıklarla yedeklenmelidir. Azure Storage üç görüntüleri tutar, bir yedekleme şimdi daha az depolama çökmeyle karşılayan için açısından önemlidir. Uygun bir yedekleme ve kurtarma planı korumak için öncelik zaman kurtarma özellikleri noktasında sağlayarak mantıksal/el ile hataları dengeleyebilirsiniz daha fazla nedenidir. Bu nedenle ya da kullanım yedeklemeleri veritabanını zaman içinde geri belirli bir noktaya geri yüklemenizi veya varolan bir veritabanını kopyalayarak başka bir sistem oluşturmak için Azure içinde yedekler kullanmaya devam etmek için belirtilir. Örneğin, bir katman 2 SAP yapılandırmasından aynı sistem için bir 3 katmanlı sistemi Kurulum yedeklemesini geri yükleyerek aktaramıyor.

Azure depolama birimine SQL sunucusunu yedeklemek için üç farklı yolu vardır:

1. SQL Server 2012 CU4 ve daha yüksek veritabanlarını yedeklemek için bir URL yerel yedekleyebilirsiniz. Bu Web Günlüğü'ndeki ayrıntılı [SQL Server 2014 - bölümü 5 - yedekleme/geri yükleme geliştirmeleri yeni işlevselliği](https://blogs.msdn.com/b/saponsqlserver/archive/2014/02/15/new-functionality-in-sql-server-2014-part-5-backup-restore-enhancements.aspx). Bölüm bkz [SQL Server 2012 SP1 CU4 ve daha sonra][dbms-guide-5.5.1].
2. SQL 2012 CU4 önce SQL Server sürümleri bir VHD'ye yedekleme ve temel olarak yapılandırılmış bir Azure depolama konumu yazma akışı taşımak için bir yeniden yönlendirme işlevini kullanabilirsiniz. Bölüm bkz [SQL Server 2012 SP1 CU3 ve önceki sürümler][dbms-guide-5.5.2].
3. Son bir disk cihaza diski komut için geleneksel SQL Server Yedekleme yöntemidir. Bu, şirket içi dağıtım modeli için aynıdır ve bu belgede ayrıntılı ele alınmamıştır.

#### <a name="0fef0e79-d3fe-4ae2-85af-73666a6f7268"></a>SQL Server 2012 SP1 CU4 ve sonraki sürümler
Bu işlev, doğrudan Azure BLOB Depolama yedekleme olanak tanır. Bu yöntem disk ve IOPS kapasite kullanılmasına neden olur diğer disklere yedeklemeniz gerekir. Temel olarak bu olur:

 ![Microsoft Azure depolama BLOBU için SQL Server 2012 yedeklemeyi kullanma][dbms-guide-figure-400]

Bir SQL Server yedekleri depolamak için disk harcamanız gerekmez avantajı bu durumda olur. Bu nedenle, ayrılan daha az sayıda disk ve disk IOPS veri ve günlük dosyaları için kullanılabilir tüm bant genişliğini vardır. Bölümünde açıklandığı gibi yedekleme en büyük boyutu en fazla 1 TB için sınırlı olduğunu unutmayın **sınırlamalar** bu makalede: <https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url#limitations>. SQL Server Yedekleme sıkıştırma kullanılarak rağmen yedekleme boyutu 1 TB boyutunda aşacak, işlevsellik açıklanan bölümde [SQL Server 2012 SP1 CU3 ve önceki sürümler] [ dbms-guide-5.5.2] bu belgede olması gerekir kullanılır.

[İlgili belgelere](https://docs.microsoft.com/sql/relational-databases/backup-restore/restoring-from-backups-stored-in-microsoft-azure) veritabanlarının Azure Blob depolamaya karşılık yedeklerden geri yükleme açıklayan önerilir yedekleme ise doğrudan Azure BLOB depolama alanından geri yüklememeyi > 25 GB. Bu makalede öneri performans değerlendirmeleri ve işlevsel kısıtlamaları nedeniyle değil temel alır. Bu nedenle, farklı koşullar bir olay temelinde uygulanabilir.

Bu yedekleme türünü ayarlamak ve diğer işlevden nasıl belgelerine bulunabilir [bu](https://docs.microsoft.com/sql/relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016) Öğreticisi

Adımların sırasını örneği okunabilir [burada](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url).

Yedeklemeleri otomatikleştirme, her yedekleme için BLOB'ları farklı adlandırılmış emin olmak için en yüksek önem derecesi olur. Aksi takdirde üzerine yazılır ve geri yükleme zinciri bozulur.

Üç farklı yedekleme türleri arasında şeyler yukarı karışık değil için sırayla yedeklemeler için kullandığınız depolama hesabı altında farklı kapsayıcılar oluşturmak önerilir. Kapsayıcılar VM tarafından yalnızca veya VM ve yedekleme türü tarafından olabilir. Şema gibi görünebilir:

 ![Microsoft Azure Storage BLOBUNA - SQL Server 2012 yedekleme ayrı depolama hesabı altında farklı kapsayıcılar kullanma][dbms-guide-figure-500]

Yukarıdaki örnekte, yedeklemeleri VM'ler dağıtıldığı storage hesabınıza gerçekleştirilen değil. Özellikle yedekleri için yeni bir depolama hesabı olacaktır. İçindeki depolama hesaplarını, yedekleme ve VM adı türünün bir matris oluşturulan farklı kapsayıcılar olacaktır. Bu tür bir kesimlere ayırma farklı VM'ler yedeklerini yönetmek kolaylaştırır.

Biri, yedeklemeleri doğrudan Yazar BLOB'ları değil eklemekte olduğunuz veri sayısı için bir VM diskleri. Bu nedenle bir durum, veriler için belirli VM sku'sunun takılı veri diski maksimum en üst düzeye ve işlem günlük dosyası ve bir depolama kapsayıcısı karşı yedek çalıştırmaya devam. 

#### <a name="f9071eff-9d72-4f47-9da4-1852d782087b"></a>SQL Server 2012 SP1 CU3 ve önceki sürümleri
Bağlantılı MSI karşıdan yüklemek için gerçekleştirmeniz gerekir doğrudan Azure Storage'a karşı yedek elde etmek için ilk adım olacaktır [bu](https://www.microsoft.com/download/details.aspx?id=40740) KBA makalesi.

İndirme x64 yükleme dosyasını ve belgeler. Dosya adında bir program yükler: **Microsoft Azure aracı için Microsoft SQL Server Yedekleme**. Ürün belgeleri baştan sona okuyun.  Aracı temelde aşağıdaki şekilde çalışır:

* SQL Server taraftan SQL Server Yedekleme için bir disk konumu tanımlanır (D:\ sürücüsüne konumu olarak kullanmayın).
* Aracı, farklı Azure Storage kapsayıcıları yedeklemeleri farklı türde yönlendirmek için kullanılan kuralları tanımlamanıza olanak sağlar.
* Kuralları yerinde olduktan sonra aracı yazma akışı yedekleme VHD'leri/disklerden daha önce tanımlanan Azure depolama konumuna yeniden yönlendirir.
* Aracı birkaç KB boyutunda küçük saplama dosyası VHD/SQL Server için tanımlandı Disk üzerindeki yedekleme bırakır. **Yeniden Azure depolama biriminden geri yüklemek için gerekli olduğundan, bu dosya depolama konumunda bırakılmalıdır.**
  * Saplama dosyası (örneğin aracılığıyla saplama dosyası bulunan depolama medyası kaybı) kaybetmiş ve bir Microsoft Azure depolama hesabına yedekleme seçeneği seçtiniz, karşıdan yükleyerek saplama dosyası aracılığıyla Microsoft Azure Storage kurtarabilir Depolama kapsayıcıyı içine yerleştirildi. Saplama dosyası aracı algılamak ve şifreleme özgün kuralla kullandıysanız aynı şifreleme parolası ile aynı kapsayıcı karşıya yüklemek için yapılandırıldığı yerel makine üzerinde bir klasör yerleştirin. 

Bu SQL Server'ın daha yeni sürümleri için yukarıda açıklandığı gibi şema yerinde de doğrudan adresi bir Azure depolama konumu sağlanmıyor SQL Server sürümleri için koyabilirsiniz anlamına gelir.

Bu yöntem kullanılmamalıdır yedekleme yukarı yerel olarak Azure Storage karşı destekleyen daha yeni SQL Server sürümleri ile. Yerel yedekleme sınırlamaları Azure içine Azure içine yerel yedekleme yürütme burada engelliyor durumlardır.

#### <a name="other-possibilities-to-back-up-sql-server-databases"></a>SQL Server veritabanlarını yedeklemek için diğer olasılıklar
Veritabanlarını yedeklemek için diğer olasılıklar olan yedekleri depolamak için kullandığınız bir VM için ek veri disklerinin eklenecek. Böyle bir durumda diskleri tam çalıştırmayan emin olmak gerekir. Bu durumda, diskleri çıkarın speak kadar çok 'arşivlemek' ve yeni bir boş disk ile değiştirmek gerekir. Bu yolun giderseniz, bu VHD ayrı Azure depolama hesaplarında olanlardan tutmak istediğiniz, veritabanı dosyalarını içeren VHD.

İkinci olasılığı bağlı, çok sayıda disk olan büyük VM örneğin D14 32VHDs ile kullanmaktır. Burada yedekleme hedefleri olarak farklı DBMS sunucuları için kullanılan sonra paylaşımları oluşturabilir esnek bir ortam oluşturmak için depolama alanları kullanın.

Bazı en iyi uygulamalar belgelenen [burada](https://blogs.msdn.com/b/sqlcat/archive/2015/02/26/large-sql-server-database-backup-on-an-azure-vm-and-archiving.aspx) de. 

#### <a name="performance-considerations-for-backupsrestores"></a>Yedekleme/geri yüklemeler için başarım düşünceleri
Çıplak dağıtımlar olduğu gibi yedekleme/geri yükleme performans paralel şekilde kaç tane birimleri okunabilir ve ne bu birimlerin verimini olabilir bağlıdır. Yedekleme sıkıştırma tarafından kullanılan CPU tüketimi Vm'lerinde olan en fazla sekiz CPU iş parçacıklarının Ayrıca, önemli bir rol oynayabilir. Bu nedenle, kabul edilebilir:

* Daha az veri depolamak için kullanılan diskler dosyalarının sayısını, küçük genel üretilen işi okuma.
* Daha küçük CPU sayısı yedekleme sıkıştırma etkisini VM, daha önemli iş parçacıkları.
* Daha az hedefleri (BLOB'ları, VHD'leri veya diskleri) yedekleme verimlilik küçük için yazılacak.
* Daha küçük VM boyutu, yazma ve Azure depolama alanından okunmasını küçük depolama üretilen iş kotası. Bağımsız olup yedeklemeler Azure Blob doğrudan depolanır veya Azure BLOB'ları yeniden depolanan VHD olup depolanır.

Microsoft Azure Storage BLOBUNA daha yeni sürümlerde yedekleme hedefi olarak kullanırken, belirli her yedekleme için yalnızca bir URL hedefi belirlenmesi için kısıtlanır.

Ancak, eski sürümlerde "Microsoft SQL Server Yedekleme" Microsoft Azure aracını kullanırken, birden fazla dosya hedef tanımlayabilirsiniz. Birden fazla hedef yedekleme ölçeklendirebilirsiniz ve yedekleme verimini daha yüksektir. Bundan sonra Azure depolama hesabındaki de birden çok dosya sonuçlanacaktır. Sınama sırasında birden çok dosya hedefleri kullanarak, kesinlikle SQL Server 2012 SP1 CU4 üzerinde uygulanan yedekleme uzantılı elde verimlilik elde edebilirsiniz. Ayrıca yerel yedekleme olduğu gibi 1 TB sınır ile Azure'da engellenmez.

Ancak, göz önünde bulundurmanız, üretilen iş ayrıca yedekleme için kullandığınız Azure depolama hesabı konumu bağlı. Sanal makineleri çalıştıran daha farklı bir bölgede depolama hesabını bulmak için bir fikir olabilir. Örneğin, Batı Avrupa'da VM yapılandırması çalıştırın, ancak için kullandığınız depolama hesabı put Kuzey Avrupa karşı yedek. Kesinlikle yedekleme verimlilik üzerinde bir etkisi ve hedef depolama ve sanal makineleri aynı bölgesel veri merkezine burada çalıştıran durumlarda mümkün göründüğü 150 MB/sn verimde oluşturmak olası değil.

#### <a name="managing-backup-blobs"></a>Yedekleme BLOB'ları yönetme
Kendi yedeklerini yönetmek için bir gereksinim yoktur. Beklenti birçok BLOB'lar sık işlem günlüğü yedeklemeleri yürüterek oluşturulan olduğundan, bu BLOB yönetim kolayca Azure portalı bırakacak. Bu nedenle, bir Azure storage Gezgini yararlanan recommendable. Bir Azure depolama hesabını yönetmek için yardımcı olan çeşitli iyi kullanılabilir olanlarla, vardır

* Microsoft Visual Studio Azure SDK'sı (<https://azure.microsoft.com/downloads/>)
* Microsoft Azure Depolama Gezgini (<https://azure.microsoft.com/downloads/>)
* Üçüncü taraf araçları

Yedekleme daha ayrıntılı bir irdelemesi ve Azure üzerinde SAP başvurmak [SAP yedekleme Kılavuzu](sap-hana-backup-guide.md) daha fazla bilgi için.

### <a name="1b353e38-21b3-4310-aeb6-a77e7c8e81c8"></a>Microsoft Azure Market dışında bir SQL Server görüntüsü kullanma
Microsoft SQL Server sürümleri zaten içeren sanal makineleri Azure markette sunar. SQL Server ve Windows için lisans isteyen SAP müşteriler için bu temel SQL Server zaten yüklü olan VM'ler yukarı dönen tarafından lisansları gereksinimini karşılamak için bir fırsat olabilir. SAP için bu tür görüntüleri kullanmak için aşağıdaki noktaları yapılması gerekir:

* SQL Server olmayan değerlendirme sürümleri, Azure Marketi'nden dağıtılan VM 'Yalnızca Windows' dan daha yüksek maliyetleri alın. Fiyatlar karşılaştırmak için aşağıdaki makalelere bakın: <https://azure.microsoft.com/pricing/details/virtual-machines/windows/> ve <https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-enterprise/>. 
* Yalnızca SQL Server 2012 gibi SAP tarafından desteklenen SQL Server sürümleri de kullanabilirsiniz.
* Azure Marketi'nde sunulan VM'ler yüklü olan SQL Server örneğinin harmanlaması SAP NetWeaver çalıştırmak için SQL Server örneği gerektirir harmanlama değil. Şu bölümdeki yönergeleri olsa ile alfabe düzenini değiştirebilirsiniz.

#### <a name="changing-the-sql-server-collation-of-a-microsoft-windowssql-server-vm"></a>Microsoft Windows/SQL Server VM'ın SQL Server harmanlaması değiştirme
Azure Market SQL Server görüntülerinde SAP NetWeaver uygulamalar tarafından gerekli olan harmanlamasını kullanacak şekilde ayarlanmayan beri dağıtımdan hemen sonra değiştirilmesi gerekiyor. SQL Server 2012 için bu adımlara VM dağıtılır ve yönetici dağıtılan VM oturum açabilecek olan hemen yapılabilir:

* Yönetici olarak bir Windows komut penceresi açın.
* Dizin C:\Program Files\Microsoft SQL Server\110\Setup Bootstrap\SQLServer2012 için değiştirin.
* Komutu yürütün: Setup.exe/QUIET/eylem REBUILDDATABASE/InstanceName = MSSQLSERVER /SQLSYSADMINACCOUNTS = =`<local_admin_account_name`> /SQLCOLLATION SQL_Latin1_General_Cp850_BIN2 =   
  * `<local_admin_account_name`> yönetici hesabı olarak VM Galerisine ilk kez dağıtırken tanımlanan hesap.

İşlem yalnızca birkaç dakika sürer. Adım doğru sonucu ile sona emin olmak için aşağıdaki adımları gerçekleştirin:

* SQL Server Management Studio’yu açın.
* Bir sorgu penceresi açın.
* Komut sp_helpsort SQL Server ana veritabanında yürütün.

İstenen sonucu gibi görünmelidir:

    Latin1-General, binary code point comparison sort for Unicode Data, SQL Server Sort Order 40 on Code Page 850 for non-Unicode Data

Bu sonuç değilse, SAP dağıtma DURDURUN ve neden Kurulum komutu beklendiği gibi çalışmadı araştırın. Yukarıda belirtilen olandan farklı SQL Server kod sayfaları ile SQL Server örneği üzerine SAP NetWeaver uygulamalarının dağıtımı **değil** desteklenir.

### <a name="sql-server-high-availability-for-sap-in-azure"></a>SQL Server yüksek kullanılabilirlik için SAP Azure
Bu makalede daha önce belirtildiği gibi eski SQL Server yüksek kullanılabilirlik işlevselliği kullanım için gerekli olan paylaşılan depolama alanı oluşturmak için olası yoktur. Bu işlev, bir Windows Server Yük devretme kümesi (kullanıcı veritabanlarını (ve tempdb sonunda) paylaşılan bir disk kullanarak WSFC) iki veya daha fazla SQL Server örnekleri yüklenir. SAP tarafından da desteklenir uzun zaman standart yüksek kullanılabilirlik yöntem budur. Azure paylaşılan depolama desteklemediğinden, SQL Server yüksek kullanılabilirlik yapılandırmaları bir paylaşılan disk küme yapılandırmasına sahip gerçekleşen alamazsınız. Ancak, diğer birçok yüksek kullanılabilirlik yöntemi hala mümkündür ve aşağıdaki bölümlerde açıklanmıştır.

#### <a name="sql-server-log-shipping"></a>SQL Server günlük aktarma
Yüksek kullanılabilirlik (HA) yöntemlerinin SQL Server günlük aktarma biridir. HA yapılandırmasında katılan VM'ler ad çözümlemesi çalışma varsa, herhangi bir sorun yoktur ve Azure kurulumunda şirket içi yapılan herhangi bir ayar farklı değildir. Yalnızca IP çözümlemesine dayanan için önerilmez. Günlük aktarma ve günlük aktarma geçici ilkeler ayarlama göre bu belgelere bakın:

<https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server>

Herhangi bir yüksek oranda kullanılabilirlik elde etmek için bir aynı Azure kullanılabilirlik kümesi içinde olacak şekilde bu tür bir günlük aktarma yapılandırma içinde olan sanal makineleri dağıtmak gerekir.

#### <a name="database-mirroring"></a>Veritabanı yansıtma
SAP tarafından desteklenen gibi veritabanı yansıtma (SAP nota bakın [965908]) bir yük devretme ortağı SAP bağlantı dizesinde tanımlama kullanır. Şirket içi durumlarda, iki VM aynı etki alanında olduğunu ve kullanıcı bağlamı iki SQL Server örnekleri de etki alanı kullanıcısı altında çalışan ve söz konusu iki SQL Server durumlarda yeterli ayrıcalıklara sahip varsayıyoruz. Bu nedenle, veritabanı yansıtma Azure kurulumunun tipik şirket içi kurulum/yapılandırma farklı değildir.

Yalnızca bulut dağıtımları itibariyle en kolay yöntem başka bir etki alanı Kurulumu bu DBMS VM'ler (ve ideal özel SAP VM'ler) sahip Azure'da bir etki alanı içinde sağlamaktır.

Bir etki alanı mümkün değilse, bir sertifika uç noktalar burada açıklandığı şekilde yansıtma veritabanı için de kullanabilirsiniz: <https://docs.microsoft.com/sql/database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql>

Veritabanı yansıtma Azure ayarlamak için bir öğretici şurada bulunabilir: <https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server> 

#### <a name="sql-server-always-on"></a>SQL Server her zaman açık
SAP şirket içi için her zaman açık desteklendiğinden (SAP nota bakın [1772688]), SAP Azure ile birlikte kullanılacak desteklenir. Paylaşılan diskler oluşturmak mümkün olmadığını olgu biri farklı VM'ler arasında bir her zaman üzerinde Windows Server Yük devretme kümesi (WSFC) yapılandırması oluşturulamıyor anlamına gelmez. Bu, yalnızca paylaşılan disk küme yapılandırmasında bir çekirdek olarak kullanma olasılığını olmadığını anlamına gelir. Bu nedenle Azure üzerinde her zaman WSFC yapılandırmasında oluşturun ve paylaşılan disk yararlanan çekirdek türü seçin. Azure ortamı bu VM'lerin Çöz ada göre VM'ler ve sanal makineleri aynı etki alanında olmalıdır dağıtılır. Bu seçenek, yalnızca Azure ve şirket içi dağıtımları için geçerlidir. SQL Server kullanılabilirlik grubu dinleyicisini (Azure kullanılabilirlik kümesi ile kafası olarak değil) dağıtma geçici bazı özel durumlar vardır bu yana Azure bu anda içi mümkün olduğu gibi bir AD/DNS nesnesi oluşturmak için izin vermez. Bu nedenle, bazı farklı yükleme adımlarını Azure belirli davranışını üstesinden gelmek gereklidir.

Bir kullanılabilirlik grubu dinleyicisi kullanarak bazı noktalar şunlardır:

* Bir kullanılabilirlik grubu dinleyicisi kullanarak yalnızca Windows Server 2012 veya daha fazla VM konuk işletim sistemi mümkündür. Windows Server 2012 için bu düzeltme eki uygulandığından emin olmanız gerekir: <https://support.microsoft.com/kb/2854082> 
* Windows Server 2008 R2 için bu düzeltme eki yok ve her zaman açık gerekir veritabanı yansıtma aynı şekilde bir yük devretme ortağı bağlantı dizesinde belirterek kullanılması (SAP default.pfl parametresi dbs/mss/sunucu aracılığıyla - bitti SAP Not bakın[965908]).
* Bir kullanılabilirlik grubu dinleyicisi kullanırken, veritabanı VM'ler adanmış bir yük dengeleyiciye bağlı olması gerekir. Ad çözümlemesi yalnızca bulut dağıtımlarında da duyar SAP sistemin (uygulama sunucuları, DBMS sunucu ve (A) SCS sunucu) tüm sanal makineleri aynı sanal ağdaki veya bir SAP uygulama katmanından sırayla etc\host dosyasının bakım gerektirir SQL Server Vm'lerinin çözülmüş VM adını almak için. Azure her iki VM arada kapatma olduğu durumlarda yeni IP adresleri atama olmasını önlemek için bir statik IP adresleri bu ağ arabirimleri VM'ler yapılandırmasında (statik IP adresi içindeaçıklanantanımlamaherzamanaçıkatamanızgerekir[bu] [ virtual-networks-reserved-private-ip] makale)

[comment]: <> (Eski blogları)
[comment]: <> (<https://blogs.msdn.com/b/alwaysonpro/archive/2014/08/29/recommendations-and-best-practices-when-deploying-sql-server-alwayson-availability-groups-in-windows-azure-iaas.aspx>, <https://blogs.technet.com/b/rmilne/archive/2015/07/27/how-to-set-static-ip-on-azure-vm.aspx>) 
* Burada küme özel bir IP adresi gerekiyor WSFC küme yapılandırma atandığında, küme düğümü üzerinde oluşturulan Azure geçerli işlevselliği ile küme adı aynı IP adresini atamanız gerekir çünkü gerekli özel adımlar vardır. Bu küme için farklı bir IP adresi atamak için el ile adım gerçekleştirilmesi anlamına gelir.
* Kullanılabilirlik grubu dinleyicisi, kullanılabilirlik grubunun birincil ve ikincil çoğaltmaları çalıştıran VM'ler için atanan TCP/IP'yi uç ile Azure'da oluşturulacak zordur.
* Bu uç noktalar ACL ile güvenli gerek olabilir.

[comment]: <> (Yapılacaklar eski blogu)
[comment]: <> (Ayrıntılı adımlar ve Azure üzerinde bir AlwaysOn yapılandırmasını yükleme necessities öğreticide kullanılabilir [here][virtual-machines-windows-classic-ps-sql-alwayson-availability-groups] taramasını olduğunda en iyi karşılaştı)
[comment]: <> (AlwaysOn kurulumuna Azure galerisinde üzerinden önceden yapılandırılmış <https://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx>)
[comment]: <> (Bir kullanılabilirlik grubu dinleyicisi oluşturma [this][virtual-machines-windows-classic-ps-sql-int-listener] öğreticide en iyi açıklanan)
[comment]: <> (Güvenliğini sağlama ağ uç nokta ACL'leri ile en iyi burada açıklanmıştır:)
[comment]: <> (* <https://michaelwasham.com/windows-azure-powershell-reference-guide/network-access-control-list-capability-in-windows-azure-powershell/>)
[comment]: <> (* <https://blogs.technet.com/b/heyscriptingguy/archive/2013/08/31/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-1-of-2.aspx> )
[comment]: <> (* <https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/01/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-2-of-2.aspx>)  
[comment]: <> (* <https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/18/creating-acls-for-windows-azure-endpoints.aspx>) 

Farklı Azure bölgeleri de bir SQL Server her zaman üzerinde kullanılabilirlik grubu dağıtmak mümkündür. Bu işlev Azure VNet-Vnet bağlantısı yararlanır ([daha fazla ayrıntı][virtual-networks-configure-vnet-to-vnet-connection]).

[comment]: <> (Yapılacaklar eski blogu)
[comment]: <> (SQL Server AlwaysOn Kullanılabilirlik grupları Kurulum böyle bir senaryoda burada açıklanan: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.) 

#### <a name="summary-on-sql-server-high-availability-in-azure"></a>SQL Server yüksek kullanılabilirlik Azure özeti
Azure Storage içeriği koruduğu gerçeği göz önüne alındığında, etkin bekleme görüntüde ısrar için daha az bir neden yoktur. Başka bir deyişle, yalnızca aşağıdaki durumlarda karşı korumak yüksek kullanılabilirlik senaryonuz gerekir:

* VM kullanılamazlık Azure veya diğer nedenlerle Sunucu kümesinde bakım nedeniyle bir bütün olarak
* SQL Server örneğinde yazılım sorunları
* Burada verileri silinir ve zaman içinde nokta kurtarma gerekli el ile hatadan koruma

Günlük aktarma tarafından üçüncü durumda yalnızca kapsamında olabilir ancak bir veritabanı yansıtma veya her zaman açık ilk iki durumlarda konulabilecek karşıdır eşleşen teknolojileri bakarak.

Her zaman avantajları Always On ile karşılaştırıldığında, veritabanı yansıtması için açık, daha karmaşık kurulumunun dengelemeniz gerekir. Bu avantajlar gibi listelenir:

* Okunabilir ikincil kopya.
* İkincil çoğaltmaları yedeklemelerden.
* Daha iyi ölçeklenebilirlik.
* Daha çok bir ikincil çoğaltma.

### <a name="9053f720-6f3b-4483-904d-15dc54141e30"></a>SAP Azure özeti için genel SQL Server
Bu kılavuzdaki birçok önerileri vardır ve bu birden çok kez Azure dağıtımınızı planlama önce okumanız önerilir. Genel olarak, yine de üst on genel DBMS Azure belirli noktalarında takip ettiğinizden emin olun:

[comment]: <> (2.3 daha yüksek verimlilik miktardan? Bir VHD?)
1. Azure'da çoğu avantajları olan SQL Server 2014 gibi en son DBMS sürümü kullanın. SQL Server için SQL Server 2012 SP1, Azure Storage sunmayı yedekleme özelliği içerir CU4 budur. Ancak, SAP birlikte en az SQL Server 2014 SP1 CU1 veya SQL Server 2012 SP2 ve en son CU kullanmak için önerilir.
2. Veri dosyası düzenini ve Azure kısıtlamaları dengelemek için azure'da, SAP sistem yatay dikkatle planlayın:
   * Verme çok sayıda disk sahip, ancak gerekli IOPS ulaşabildiğimizden emin olmak için yeterli.
   * Yönetilen diskleri kullanmıyorsanız, IOPS de Azure depolama hesabı sınırlı olduğunu ve depolama hesapları her Azure aboneliği sınırlı olduğunu unutmayın ([daha fazla ayrıntı][azure-subscription-service-limits]). 
   * Yalnızca daha yüksek verimlilik elde etmek gerekiyorsa diskler boyunca Şerit.
3. Kalıcı olmayan ve bu sürücüde herhangi bir şey Windows yeniden başlatma sırasında kaybolur D:\ sürücüde Kalıcılık gerektiren herhangi bir dosya put veya hiçbir zaman yazılımını yükleyin.
4. Disk önbelleği için Azure Standard Storage kullanmayın.
5. Coğrafi olarak çoğaltılmış Azure depolama hesapları kullanmayın.  Yerel olarak yedekli DBMS iş yükleri için kullanın.
6. Veritabanı veri çoğaltmak için DBMS satıcınızın HA/DR çözümü kullanın.
7. Her zaman ad çözümlemesi kullanın, IP adreslerini güvenmeyin.
8. Olası en yüksek veritabanı sıkıştırma kullanın. SQL Server için sayfa sıkıştırma olduğu.
9. SQL Server görüntülerinin Azure Marketi'nden kullanırken dikkatli olun. SQL Server birini kullanıyorsanız, herhangi bir SAP NetWeaver sistemini yüklemeden önce örneği harmanlaması değiştirmeniz gerekir.
10. Yükleme ve yapılandırma bölümünde açıklandığı gibi SAP izleme ana bilgisayarı Azure [Dağıtım Kılavuzu'na][deployment-guide].

## <a name="specifics-to-sap-ase-on-windows"></a>SAP ana Windows özellikleri
Microsoft Azure ile başlayarak, var olan SAP ana uygulamalarınız için Azure sanal makineleri kolayca geçirebilirsiniz. SAP ana bir sanal makinede Microsoft Azure bu uygulamaları kolayca geçiş yaparak toplam sahip olma dağıtım, yönetim ve Bakım Kurumsal avantajlarına uygulama maliyetini azaltmak etkinleştirir. SAP ana bir Azure sanal makine ile yöneticiler ve geliştiriciler aynı geliştirme ve mevcut şirket içi yönetim araçlarını kullanmaya devam edebilirsiniz.

Bir SLA için Azure sanal burada bulunan makineler, vardır: <https://azure.microsoft.com/support/legal/sla/virtual-machines>

Microsoft Azure barındırılan sanal makinelerin gerçekleştirir iyi diğer genel bulut sanallaştırma teklifleri, ancak tek tek sonuç kıyasla değişebilir emin duyuyoruz. SAP sayıda farklı SAP boyutlandırma SAP sertifikalı VM SKU'ları ayrı SAP Not içinde sağlanan [1928533].

SAP ana dağıtımları SAP uygulamaları ile birlikte bu belge ilk dört bölüm belirtildiği gibi deyimleri ve Azure Storage, SAP dağıtım VM'ler veya SAP izleme kullanım in regard to önerileri uygulayın.

### <a name="sap-ase-version-support"></a>SAP ana sürüm desteği
Şu anda destekler SAP ana sürüm 16,0 SAP Business Suite ürünlerle kullanmak üzere SAP. SAP ana sunucu veya SAP Business Suite ürünleri ile kullanılacak JDBC ve ODBC sürücüleri için tüm güncelleştirmeleri yalnızca SAP hizmet Market'ten aracılığıyla sağlanır: <https://support.sap.com/swdc>.

Yüklemeleri şirket içi için olduğu gibi JDBC ve ODBC sürücüleri veya SAP ana sunucu için güncelleştirmeleri doğrudan Sybase sitelerinden yüklemeyin. Düzeltme ekleri hakkında ayrıntılı bilgi için içi ürünleri SAP Business Suite ile kullanım için desteklenir ve Azure sanal makinelerinde aşağıdaki SAP notlara bakın:

* [1590719]
* [1973241]

SAP Business Suite SAP ana üzerinde çalıştırma hakkında genel bilgiler bulunabilir [SCN](https://www.sap.com/community/topic/ase.html)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>SAP ilgili SAP ana yüklemelerini Azure VM'ler için SAP ana yapılandırma yönergeleri
#### <a name="structure-of-the-sap-ase-deployment"></a>SAP ana dağıtım yapısı
Genel Açıklama uygun olarak SAP ana yürütülebilir dosyalar bulunabilir veya sanal makinenin işletim sistemi diski sistem sürücüsüne yüklü (c: sürücüsü\). Genellikle, SAP ana sistem ve araçları veritabanlarının çoğu sabit SAP NetWeaver iş yükü tarafından kullanılmaz. Bu nedenle sistem ve araçları veritabanları (master, model, saptools, sybmgmtdb, sybsystemdb) C:\ sürücüsünü de kalabilir. 

İstisna tüm iş tabloları ve daha yüksek veri birimi veya özgün sanal makinenin işletim sistemi sığamıyorsa g/ç işlemleri birim gerektirebilir, bazı SAP ERP ve tüm BW iş yükleri durumunda SAP ana tarafından oluşturulan geçici tabloları içeren geçici veritabanı olabilir disk (c: sürücüsü\).

SAPInst/SWPM sistemini yüklemek için kullanılan sürümüne bağlı olarak, veritabanı içerebilir:

* SAP ana yüklenirken oluşturulan tek bir SAP ana tempdb
* SAP ana ve SAP yükleme yordamı tarafından oluşturulan bir ek saptempdb yükleyerek oluşturulan bir SAP ana tempdb
* SAP ana ve el ile oluşturulan bir ek tempdb yükleyerek oluşturulan bir SAP ana tempdb (Örneğin SAP Not aşağıdaki [1752266]) ERP/BW belirli tempdb gereksinimlerini karşılamak için

Belirli ERP veya tüm BW iş yükleri durumunda, C:\ dışında bir sürücüde (SWPM veya el ile) ek olarak oluşturulan tempdb tempdb aygıtların tutmak için performans, in regard to mantıklıdır. Hiçbir ek tempdb varsa oluşturmak için önerilir (SAP Not [1752266]).

Böyle sistemler için ayrıca oluşturulan tempdb için aşağıdaki adımlar gerçekleştirilmelidir:

* SAP veritabanına ilk cihaza ilk tempdb aygıtı taşıyın
* Her bir aygıtı SAP veritabanının içeren VHD tempdb cihaz Ekle

Bu yapılandırma ya da sistem sürücüsünün sağlamak mümkün olandan daha fazla alanı kullanmak üzere tempdb etkinleştirir. Bir başvuru olarak bir şirket içi varolan sistemler tempdb aygıt boyutlarına kontrol edebilirsiniz. Veya bu tür bir yapılandırma ile sistem sürücüsünün sağlanamaz tempdb karşı IOPS numaraları olanak sağlar. Yeniden şirket içi çalışan sistemlere tempdb karşı g/ç iş yükünü izlemek için kullanılabilir.

Hiçbir zaman herhangi bir SAP ana aygıtı VM'ye D:\ sürücüye yerleştirin. Tempdb tutulan nesneleri yalnızca geçici olsa bile, bu da tempdb için geçerlidir.

#### <a name="impact-of-database-compression"></a>Veritabanı Sıkıştırma etkisi
Burada g/ç bant genişliği sınırlayıcı bir etmen haline gelebilir yapılandırmalarında IOPS azaltır her ölçü bir Azure gibi bir Iaas senaryosu çalıştırabilirsiniz iş yükü uzatmak için yardımcı olabilir. Bu nedenle, var olan bir SAP veritabanı için Azure karşıya yüklemeden önce SAP ana sıkıştırma kullandığınız emin olmak için önerilir.

Öneri zaten uygulanmadı, Azure'a karşıya yüklemeden önce sıkıştırma gerçekleştirmek için çeşitli nedenlerden dışında verilmiştir:

* Azure için yüklenecek veri miktarını düşük olduğu
* G/ç gecikmesi şirket içi daha az veya daha fazla CPU veya daha yüksek g/ç bant genişliği ile daha güçlü donanım kullanabilirsiniz varsayılarak sıkıştırma yürütme süresi kısadır
* Daha küçük veritabanı boyutları disk ayırma için daha az maliyetlere neden olabilir

Veri ve LOB sıkıştırma şirket içi yaptığı gibi Azure sanal makinelerinde barındırılan bir VM'nin çalışır. Varolan bir SAP ana veritabanında sıkıştırma zaten içinde olup olmadığını denetlemek nasıl daha fazla ayrıntı kullanmak için SAP Not denetleyin [1750510].

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Veritabanı örneklerini izlemek için DBACockpit kullanma
SAP ana veritabanı platform olarak kullanıyorsanız, SAP sistemleri için DBACockpit işlem DBACockpit katıştırılmış tarayıcı pencerelerini veya Webdynpro olarak erişilebilir. Ancak veritabanını yönetme ve izleme için tam işlevsellik DBACockpit yalnızca Webdynpro uygulamasında kullanılabilir.

Olarak şirket içi sistemlerle birkaç adım DBACockpit Webdynpro uygulaması tarafından kullanılan tüm SAP NetWeaver işlevselliğini etkinleştirmek için gereklidir. SAP Not uygulayın [1245200] webdynpros kullanımını etkinleştirin ve gerekli olanları oluşturmak için. Yukarıdaki notları bölümündeki yönergeleri izleyerek verilirken, ayrıca Internet iletişimi Yöneticisi'ni (ICM) http ve https bağlantıları için kullanılacak bağlantı noktaları ile birlikte yapılandırılır. Http varsayılan ayarı şöyle görünür:

> ICM/server_port_0 KOR = HTTP bağlantı noktası = 8000, PROCTIMEOUT = 600, zaman AŞIMI = 600 =
> 
> ICM/server_port_1 KOR = = HTTPS, bağlantı noktası 443$, PROCTIMEOUT = 600, zaman AŞIMI = 600 =
> 
> 

ve işlem içinde oluşturulan bağlantılar DBACockpit şuna benzer:

> https://`<fullyqualifiedhostname`>: sap/44300/bc/sap/webdynpro/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: sap/8000/bc/sap/webdynpro/dba_cockpit
> 
> 

Bağlı olarak varsa ve SAP sistem barındırma Azure sanal makine için siteden, bağlı nasıl çok siteli veya bu ICM emin olmak için gereken ExpressRoute (şirket içi dağıtım) çözülebilir tam bir ana bilgisayar makinesinde kullanıyor nerede gelen DBACockpit açmaya çalışıyorsunuz. SAP nota bakın [773830] nasıl ICM profili parametreleri ve kümesi parametre ICM/host_name_full bağlı olarak tam ana bilgisayar adı açıkça gerekirse belirler anlamak için.

Şirket içi bağlantılar şirket içi ve Azure arasında olmadan yalnızca bulut senaryosunda VM dağıtılmışsa, bir ortak IP adresi ve bir domainlabel tanımlamanız gerekir. VM ortak DNS adının biçimi şöyle görünür:

> `<custom domainlabel`>.`<azure region`>.cloudapp.azure.com
> 
> 

DNS adı ile ilgili daha fazla ayrıntı bulunabilir [burada][virtual-machines-azurerm-versus-azuresm].

Azure VM bağlantıyı DNS adını SAP profili parametre ICM/host_name_full ayarı benzer şekilde görünebilir:

> https://mydomainlabel.westeurope.cloudapp.net:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://mydomainlabel.westeurope.cloudapp.net:8000/sap/bc/webdynpro/sap/dba_cockpit
> 
> 

Bu durumda için emin olmanız gerekir:

* Azure portalında ICM ile iletişim kurmak için kullanılan TCP/IP bağlantı noktaları için ağ güvenlik grubu için gelen kuralları ekleme
* Windows Güvenlik Duvarı yapılandırmasını ICM ile iletişim kurmak için kullanılan TCP/IP bağlantı noktaları için gelen kuralları ekleme

Bir otomatik için kullanılabilir tüm düzeltmeler alınan, düzenli aralıklarla SAP Not SAP sürümünüz için geçerli düzeltme koleksiyonu uygulamak için önerilir:

* [1558958]
* [1619967]
* [1882376]

Aşağıdaki SAP notları SAP ana DBA Pilot hakkında daha fazla bilgi bulunabilir:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496]    
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>SAP ana yedekleme/kurtarma değerlendirmeleri
SAP ana Azure'a dağıtırken, yedekleme yönteminize gözden geçirilmesi gerekir. Sistem üretken sistem olsa bile, SAP ana tarafından barındırılan SAP veritabanında düzenli aralıklarla yedeklenmelidir. Azure Storage üç görüntüleri tutar, bir yedekleme şimdi daha az depolama çökmeyle karşılayan için açısından önemlidir. Uygun bir yedekleme ve geri yükleme planı korumak için birincil nedeni zaman kurtarma özellikleri noktasında sağlayarak mantıksal/el ile hataları dengeleyebilirsiniz daha büyük. Bu nedenle ya da kullanım yedeklemeleri veritabanını zaman içinde geri belirli bir noktaya geri yüklemenizi veya varolan bir veritabanını kopyalayarak başka bir sistem oluşturmak için Azure içinde yedekler kullanmaya devam etmek için belirtilir. Örneğin, bir katman 2 SAP yapılandırmasından aynı sistem için bir 3 katmanlı sistemi Kurulum yedeklemesini geri yükleyerek aktaramıyor.

Şirket içi yaptığı gibi yedekleme ve geri yükleme Azure veritabanında aynı şekilde çalışır. SAP notlara bakın:

* [1588316]
* [1585981]

Döküm yapılandırmaları oluşturma ve zamanlama yedeklemeler daha fazla ayrıntı için. Stratejisi ve yapılandırabileceğiniz gereksinimlerine bağlı olarak veritabanı ve günlük dökümleri bir var olan disklerin disk veya yedekleme için ek bir disk ekleyin. Bir hata durumunda veri kaybını tehlike azaltmak için hiçbir veritabanı aygıtı bulunduğu bir disk kullanmak için önerilir.

Veri ve LOB yanı sıra sıkıştırma SAP ana yedekleme sıkıştırma da sağlar. Veritabanı ve günlük dökümleri ile daha az alan kaplar için yedekleme sıkıştırma kullanmak için önerilir. Daha fazla bilgi için bkz. SAP Not [1588316]. Yedekleme sıkıştırma ayrıca yedekleri ya da şirket içi yedek dökümleri Azure sanal makinenin içeren VHD indirmeyi planlıyorsanız aktarılacak veri miktarını azaltmak çok önemlidir.

Sürücü D:\ döküm hedef veritabanı veya günlük olarak kullanmayın.

#### <a name="performance-considerations-for-backupsrestores"></a>Yedekleme/geri yüklemeler için başarım düşünceleri
Çıplak dağıtımlar olduğu gibi yedekleme/geri yükleme performans paralel şekilde kaç tane birimleri okunabilir ve ne bu birimlerin verimini olabilir bağlıdır. Yedekleme sıkıştırma tarafından kullanılan CPU tüketimi Vm'lerinde olan en fazla sekiz CPU iş parçacıklarının Ayrıca, önemli bir rol oynayabilir. Bu nedenle, bir kabul edebilirsiniz:

* Daha az veritabanı aygıtları depolamak için kullanılan disk sayısı daha küçük genel üretilen işi okuma
* Daha küçük CPU sayısı yedekleme sıkıştırma etkisini VM, daha önemli iş parçacıkları
* Yedekleme işleme için daha az yazmak için daha az hedefleri (Stripe dizinleri, diskler)

Gereksinimlerinize bağlı olarak kullanılan/birleştirilmiş iki seçenek vardır yazmak için hedefleri sayısını artırmak için:

* Şeritli birim üzerinde IOPS üretilen işi artırmak için birden fazla bağlı diskler üzerinde Yedekleme hedef birimi bölümlemesine
* SAP ana düzeyinde bir döküm yapılandırması oluşturma, birden fazla hedef dizin döküm yazmak için kullanır

Bir birim üzerinde birden çok bağlama diskleri bölümlemesine bu kılavuzun önceki bölümlerinde ele. SAP ana döküm yapılandırmasında birden çok dizin kullanma ile ilgili daha fazla bilgi için döküm yapılandırması oluşturmak için kullanılan saklı yordam sp_config_dump üzerinde belgelerine başvurun [Sybase Bilgi Merkezi](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Azure VM'ler ile olağanüstü durum kurtarma
#### <a name="data-replication-with-sap-sybase-replication-server"></a>SAP Sybase çoğaltma sunucusu ile veri çoğaltma
SAP Sybase çoğaltma sunucusuna (SRS) SAP ana ile veritabanı işlemleri uzak bir konuma zaman uyumsuz olarak aktarması sıcak bir bekleme çözümü sağlar. 

Yükleme ve SRS işlemi çalışır de işlevsel olarak şirket içi yaptığı gibi Azure sanal makine Hizmetleri'nde barındırılan bir VM'nin.

SAP ana HADR Azure iç yük dengeleyici gerektirmez ve işletim sistemi düzeyinde kümeleme üzerinde bağımlılıklar yok ve Azure Windows ve Linux VM'ler üzerinde çalışır. SAP ana HADR ayrıntıları okumak için [SAP ana HADR Kullanıcı Kılavuzu](https://help.sap.com/viewer/efe56ad3cad0467d837c8ff1ac6ba75c/16.0.3.3/en-US/a6645e28bc2b1014b54b8815a64b87ba.html).

## <a name="specifics-to-sap-ase-on-linux"></a>SAP ana Linux'ta yazıp yazmayacağını
Microsoft Azure ile başlayarak, var olan SAP ana uygulamalarınız için Azure sanal makineleri kolayca geçirebilirsiniz. SAP ana bir sanal makinede Microsoft Azure bu uygulamaları kolayca geçiş yaparak toplam sahip olma dağıtım, yönetim ve Bakım Kurumsal avantajlarına uygulama maliyetini azaltmak etkinleştirir. SAP ana bir Azure sanal makine ile yöneticiler ve geliştiriciler aynı geliştirme ve mevcut şirket içi yönetim araçlarını kullanmaya devam edebilirsiniz.

Azure sanal makineleri dağıtmak için burada bulunabilir resmi SLA bilmeniz önemlidir: <https://azure.microsoft.com/support/legal/sla>

SAP boyutlandırma bilgileri ve SAP VM SKU'ları sertifikalı listesini SAP notta sağlanan [1928533]. Azure sanal makineleri burada bulunabilir belgeleri boyutlandırma ek SAP <http://blogs.msdn.com/b/saponsqlserver/archive/2015/06/19/how-to-size-sap-systems-running-on-azure-vms.aspx> ve burada <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

SAP ana dağıtımları SAP uygulamaları ile birlikte bu belge ilk dört bölüm belirtildiği gibi deyimleri ve Azure Storage, SAP dağıtım VM'ler veya SAP izleme kullanım in regard to önerileri uygulayın.

Aşağıdaki iki SAP notları bulutta Linux ve ana ana hakkında genel bilgi içerir:

* [2134316]
* [1941500]

### <a name="sap-ase-version-support"></a>SAP ana sürüm desteği
Şu anda destekler SAP ana sürüm 16,0 SAP Business Suite ürünlerle kullanmak üzere SAP. SAP ana sunucu veya SAP Business Suite ürünleri ile kullanılacak JDBC ve ODBC sürücüleri için tüm güncelleştirmeleri yalnızca SAP hizmet Market'ten aracılığıyla sağlanır: <https://support.sap.com/swdc>.

Yüklemeleri şirket içi için olduğu gibi JDBC ve ODBC sürücüleri veya SAP ana sunucu için güncelleştirmeleri doğrudan Sybase sitelerinden yüklemeyin. Düzeltme ekleri hakkında ayrıntılı bilgi için içi ürünleri SAP Business Suite ile kullanım için desteklenir ve Azure sanal makinelerinde aşağıdaki SAP notlara bakın:

* [1590719]
* [1973241]

SAP Business Suite SAP ana üzerinde çalıştırma hakkında genel bilgiler bulunabilir [SCN](https://www.sap.com/community/topic/ase.html)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>SAP ilgili SAP ana yüklemelerini Azure VM'ler için SAP ana yapılandırma yönergeleri
#### <a name="structure-of-the-sap-ase-deployment"></a>SAP ana dağıtım yapısı
Genel Açıklama uygun olarak SAP ana yürütülebilir dosyalar bulunabilir veya kök dosya sistemine (/sybase) VM yüklenir. Genellikle, SAP ana sistem ve araçları veritabanlarının çoğu sabit SAP NetWeaver iş yükü tarafından yararlanılabilir değil. Bu nedenle sistem ve araçları veritabanları (master, model, saptools, sybmgmtdb, sybsystemdb) de kök dosya sisteminde kalabilir. 

İstisna tüm iş tabloları ve daha yüksek veri birimi veya g/ç işlemleri, özgün sanal makinenin işletim sistemi sığamıyorsa birimi gerektirebilir, bazı SAP ERP ve tüm BW iş yükleri durumunda SAP ana tarafından oluşturulan geçici tabloları içeren geçici veritabanı olabilir disk.

SAPInst/SWPM sistemini yüklemek için kullanılan sürümüne bağlı olarak, veritabanı içerebilir:

* SAP ana yüklenirken oluşturulan tek bir SAP ana tempdb
* SAP ana ve SAP yükleme yordamı tarafından oluşturulan bir ek saptempdb yükleyerek oluşturulan bir SAP ana tempdb
* SAP ana ve el ile oluşturulan bir ek tempdb yükleyerek oluşturulan bir SAP ana tempdb (Örneğin SAP Not aşağıdaki [1752266]) ERP/BW belirli tempdb gereksinimlerini karşılamak için

Belirli ERP veya tüm BW iş yükleri söz konusu olduğunda, tek bir Azure veri diski veya Linux RAID spannin tarafından temsil edilebilir bir ayrı bir dosya sistemi (SWPM veya el ile) ek olarak oluşturulan tempdb tempdb aygıtların tutmak için performans, in regard to mantıklıdır g birden çok Azure veri diski. Hiçbir ek tempdb varsa oluşturmak için önerilir (SAP Not [1752266]).

Böyle sistemler için ayrıca oluşturulan tempdb için aşağıdaki adımlar gerçekleştirilmelidir:

* İlk tempdb dizini SAP veritabanına ilk dosya sistemine taşıma
* Tempdb dizinler her bir dosya sistemi SAP veritabanının içeren diskleri ekleme

Bu yapılandırma ya da sistem sürücüsünün sağlamak mümkün olandan daha fazla alanı kullanmak üzere tempdb etkinleştirir. Bir başvuru olarak bir şirket içi varolan sistemler tempdb dizini boyutlarına kontrol edebilirsiniz. Veya bu tür bir yapılandırma ile sistem sürücüsünün sağlanamaz tempdb karşı IOPS numaraları olanak sağlar. Yeniden şirket içi çalışan sistemlere tempdb karşı g/ç iş yükünü izlemek için kullanılabilir.

Hiçbir zaman herhangi bir SAP ana dizin /mnt veya /mnt/resource VM üzerine yerleştirin. Kalıcı olmayan bir varsayılan Azure VM geçici alanı, /mnt veya /mnt/resource olduğundan tempdb tutulan nesneleri yalnızca geçici olsa bile bu tempdb için de geçerlidir. Azure VM geçici alanı hakkında daha fazla ayrıntı bulunabilir [bu makalede][virtual-machines-linux-how-to-attach-disk]

#### <a name="impact-of-database-compression"></a>Veritabanı Sıkıştırma etkisi
Burada g/ç bant genişliği sınırlayıcı bir etmen haline gelebilir yapılandırmalarında IOPS azaltır her ölçü bir Azure gibi bir Iaas senaryosu çalıştırabilirsiniz iş yükü uzatmak için yardımcı olabilir. Bu nedenle, var olan bir SAP veritabanı için Azure karşıya yüklemeden önce SAP ana sıkıştırma kullandığınız emin olmak için önerilir.

Öneri zaten uygulanmadı, Azure'a karşıya yüklemeden önce sıkıştırma gerçekleştirmek için çeşitli nedenlerden dışında verilmiştir:

* Azure için yüklenecek veri miktarını düşük olduğu
* G/ç gecikmesi şirket içi daha az veya daha fazla CPU veya daha yüksek g/ç bant genişliği ile daha güçlü donanım kullanabilirsiniz varsayılarak sıkıştırma yürütme süresi kısadır
* Daha küçük veritabanı boyutları disk ayırma için daha az maliyetlere neden olabilir

Veri ve LOB sıkıştırma şirket içi yaptığı gibi Azure sanal makinelerinde barındırılan bir VM'nin çalışır. Varolan bir SAP ana veritabanında sıkıştırma zaten içinde olup olmadığını denetlemek nasıl daha fazla ayrıntı kullanmak için SAP Not denetleyin [1750510]. Veritabanı Sıkıştırma ilgili ek bilgi için bkz. SAP Not [2121797].

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Veritabanı örneklerini izlemek için DBACockpit kullanma
SAP ana veritabanı platform olarak kullanıyorsanız, SAP sistemleri için DBACockpit işlem DBACockpit katıştırılmış tarayıcı pencerelerini veya Webdynpro olarak erişilebilir. Ancak veritabanını yönetme ve izleme için tam işlevsellik DBACockpit yalnızca Webdynpro uygulamasında kullanılabilir.

Olarak şirket içi sistemlerle birkaç adım DBACockpit Webdynpro uygulaması tarafından kullanılan tüm SAP NetWeaver işlevselliğini etkinleştirmek için gereklidir. SAP Not uygulayın [1245200] webdynpros kullanımını etkinleştirin ve gerekli olanları oluşturmak için. Yukarıdaki notları bölümündeki yönergeleri izleyerek verilirken, ayrıca Internet iletişimi Yöneticisi'ni (ICM) http ve https bağlantıları için kullanılacak bağlantı noktaları ile birlikte yapılandırılır. Http varsayılan ayarı şöyle görünür:

> ICM/server_port_0 KOR = HTTP bağlantı noktası = 8000, PROCTIMEOUT = 600, zaman AŞIMI = 600 =
> 
> ICM/server_port_1 KOR = = HTTPS, bağlantı noktası 443$, PROCTIMEOUT = 600, zaman AŞIMI = 600 =
> 
> 

ve işlem içinde oluşturulan bağlantılar DBACockpit için şuna benzer:

> https://`<fullyqualifiedhostname`>: sap/44300/bc/sap/webdynpro/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: sap/8000/bc/sap/webdynpro/dba_cockpit
> 
> 

Bağlı olarak varsa ve SAP sistem barındırma Azure sanal makine için siteden, bağlı nasıl çok siteli veya bu ICM emin olmak için gereken ExpressRoute (şirket içi dağıtım) çözülebilir tam bir ana bilgisayar makinesinde kullanıyor nerede gelen DBACockpit açmaya çalışıyorsunuz. SAP nota bakın [773830] nasıl ICM profili parametreleri ve kümesi parametre ICM/host_name_full bağlı olarak tam ana bilgisayar adı açıkça gerekirse belirler anlamak için.

Şirket içi bağlantılar şirket içi ve Azure arasında olmadan yalnızca bulut senaryosunda VM dağıtılmışsa, bir ortak IP adresi ve bir domainlabel tanımlamanız gerekir. VM ortak DNS adının biçimi şöyle görünür:

> `<custom domainlabel`>.`<azure region`>.cloudapp.azure.com
> 
> 

DNS adı ile ilgili daha fazla ayrıntı bulunabilir [burada][virtual-machines-azurerm-versus-azuresm].

Azure VM bağlantıyı DNS adını SAP profili parametre ICM/host_name_full ayarı benzer şekilde görünebilir:

> https://mydomainlabel.westeurope.cloudapp.net:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://mydomainlabel.westeurope.cloudapp.net:8000/sap/bc/webdynpro/sap/dba_cockpit
> 
> 

Bu durumda için emin olmanız gerekir:

* Azure portalında ICM ile iletişim kurmak için kullanılan TCP/IP bağlantı noktaları için ağ güvenlik grubu için gelen kuralları ekleme
* Windows Güvenlik Duvarı yapılandırmasını ICM ile iletişim kurmak için kullanılan TCP/IP bağlantı noktaları için gelen kuralları ekleme

Bir otomatik için kullanılabilir tüm düzeltmeler alınan, düzenli aralıklarla SAP Not SAP sürümünüz için geçerli düzeltme koleksiyonu uygulamak için önerilir:

* [1558958]
* [1619967]
* [1882376]

Aşağıdaki SAP notları SAP ana DBA Pilot hakkında daha fazla bilgi bulunabilir:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496]    
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>SAP ana yedekleme/kurtarma değerlendirmeleri
SAP ana Azure'a dağıtırken yedekleme yönteminize gözden geçirilmesi gerekir. Sistem üretken sistem olsa bile, SAP ana tarafından barındırılan SAP veritabanında düzenli aralıklarla yedeklenmelidir. Azure Storage üç görüntüleri tutar, bir yedekleme şimdi daha az depolama çökmeyle karşılayan için açısından önemlidir. Uygun bir yedekleme ve geri yükleme planı korumak için birincil nedeni zaman kurtarma özellikleri noktasında sağlayarak mantıksal/el ile hataları dengeleyebilirsiniz daha büyük. Bu nedenle ya da kullanım yedeklemeleri veritabanını zaman içinde geri belirli bir noktaya geri yüklemenizi veya varolan bir veritabanını kopyalayarak başka bir sistem oluşturmak için Azure içinde yedekler kullanmaya devam etmek için belirtilir. Örneğin, bir katman 2 SAP yapılandırmasından aynı sistem için bir 3 katmanlı sistemi Kurulum yedeklemesini geri yükleyerek aktaramıyor.

Şirket içi yaptığı gibi yedekleme ve geri yükleme Azure veritabanında aynı şekilde çalışır. SAP notlara bakın:

* [1588316]
* [1585981]

Döküm yapılandırmaları oluşturma ve zamanlama yedeklemeler daha fazla ayrıntı için. Stratejisi ve yapılandırabileceğiniz gereksinimlerine bağlı olarak veritabanı ve günlük dökümleri bir var olan disklerin disk veya yedekleme için ek bir disk ekleyin. Bir hata durumunda veri kaybını tehlike azaltmak için hiçbir veritabanı dizin/dosya bulunduğu bir disk kullanmak için önerilir.

Veri ve LOB yanı sıra sıkıştırma SAP ana yedekleme sıkıştırma da sağlar. Veritabanı ve günlük dökümleri ile daha az alan kaplar için yedekleme sıkıştırma kullanmak için önerilir. Daha fazla bilgi için bkz. SAP Not [1588316]. Yedekleme sıkıştırma ayrıca yedekleri ya da şirket içi yedek dökümleri Azure sanal makinenin içeren VHD indirmeyi planlıyorsanız aktarılacak veri miktarını azaltmak çok önemlidir.

Azure VM geçici alanı /mnt veya /mnt/resource döküm hedef veritabanı veya günlük olarak kullanmayın.

#### <a name="performance-considerations-for-backupsrestores"></a>Yedekleme/geri yüklemeler için başarım düşünceleri
Çıplak dağıtımlar olduğu gibi yedekleme/geri yükleme performans paralel şekilde kaç tane birimleri okunabilir ve ne bu birimlerin verimini olabilir bağlıdır. Yedekleme sıkıştırma tarafından kullanılan CPU tüketimi Vm'lerinde olan en fazla sekiz CPU iş parçacıklarının Ayrıca, önemli bir rol oynayabilir. Bu nedenle, bir kabul edebilirsiniz:

* Daha az veritabanı aygıtları depolamak için kullanılan disk sayısı daha küçük genel üretilen işi okuma
* Daha küçük CPU sayısı yedekleme sıkıştırma etkisini VM, daha önemli iş parçacıkları
* Yedekleme işleme için daha az yazmak için daha az hedefleri (Linux yazılım RAID, diskler)

Gereksinimlerinize bağlı olarak kullanılan/birleştirilmiş iki seçenek vardır yazmak için hedefleri sayısını artırmak için:

* Şeritli birim üzerinde IOPS üretilen işi artırmak için birden fazla bağlı diskler üzerinde Yedekleme hedef birimi bölümlemesine
* SAP ana düzeyinde bir döküm yapılandırması oluşturma, birden fazla hedef dizin döküm yazmak için kullanır

Bir birim üzerinde birden çok bağlama diskleri bölümlemesine bu kılavuzun önceki bölümlerinde ele. SAP ana döküm yapılandırmasında birden çok dizin kullanma ile ilgili daha fazla bilgi için döküm yapılandırması oluşturmak için kullanılan saklı yordam sp_config_dump üzerinde belgelerine başvurun [Sybase Bilgi Merkezi](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Azure VM'ler ile olağanüstü durum kurtarma
#### <a name="data-replication-with-sap-sybase-replication-server"></a>SAP Sybase çoğaltma sunucusu ile veri çoğaltma
SAP Sybase çoğaltma sunucusuna (SRS) SAP ana ile veritabanı işlemleri uzak bir konuma zaman uyumsuz olarak aktarması sıcak bir bekleme çözümü sağlar. 

Yükleme ve SRS işlemi çalışır de işlevsel olarak şirket içi yaptığı gibi Azure sanal makine Hizmetleri'nde barındırılan bir VM'nin.

SAP çoğaltma sunucusu aracılığıyla ana HADR bu anda desteklenmez. İle test ve Microsoft Azure platform için gelecekte yayımlanan olabilir.

## <a name="specifics-to-oracle-database-on-windows"></a>Oracle veritabanına Windows özellikleri
Oracle yazılım, Microsoft Windows Hyper-V ve Azure üzerinde çalıştırmak için Oracle tarafından desteklenir. Windows Hyper-V ve Azure genel destek hakkında daha fazla bilgi için kontrol edin: <https://blogs.oracle.com/cloud/entry/oracle_and_microsoft_join_forces> 

Genel destek, belirli bir senaryoyu Oracle veritabanları yararlanarak SAP uygulamalarının de desteklenir. Ayrıntılar belgenin bu bölümü adlandırılır.

### <a name="oracle-version-support"></a>Oracle sürüm desteği
Oracle ve SAP Oracle Azure sanal makineler üzerinde çalışan SAP notta bulunabilir, desteklenen karşılık gelen işletim sistemi sürümleri [2039619].

SAP Business Suite Oracle üzerinde çalıştırma hakkında genel bilgi 1DX içinde bulunabilir: <https://www.sap.com/community/topic/oracle.html>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP yüklemelerde Azure VM'ler için Oracle yapılandırma yönergeleri
#### <a name="storage-configuration"></a>Depolama yapılandırması
Yalnızca tek bir örneği NTFS biçimli diskler kullanarak Oracle desteklenir. Tüm veritabanı dosyaları, VHD'leri veya yönetilen diskleri göre NTFS dosya sistemi depolanmalıdır. Bu diskleri Azure VM'ye bağlanır ve Azure sayfa BLOB Depolama birimine dayalı (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) veya yönetilen diskleri (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Her türlü ağ sürücülerine veya Azure dosya services gibi uzak paylaşımları:

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx> 
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx>

olan **değil** Oracle veritabanı dosyaları için desteklenen!

Diskler kullanarak temel Azure sayfa BLOB Storage veya yönetilen diskleri, bu belgede bölüm yapılan deyimleri [VM'ler ve veri diskleri için önbelleğe alma] [ dbms-guide-2.1] ve [Microsoft Azure depolama] [ dbms-guide-2.3] Oracle veritabanı da dağıtımlar için geçerlidir.

Önceki belge Genel bölümünde açıklandığı gibi Azure disklerin IOPS verimlilik kotalar mevcut. Tam kotaları VM türüne bağlı olarak kullanılır. Kendi kotaları ile VM türlerinin bir listesini bulunabilir [burada (Linux)] [ virtual-machines-sizes-linux] ve [burada (Windows)][virtual-machines-sizes-windows].

Desteklenen Azure VM türler tanımlamak için SAP nota bakın [1928533].

Disk başına geçerli IOPS kota gereksinimlerini karşılayan sürece, tek tek bağlı disk üzerindeki tüm veritabanı dosyaları depolamak mümkündür. 

Daha fazla IOPS gerekirse, kullanım penceresi depolama havuzları (yalnızca Windows Server 2012'de kullanılabilir ve üzeri) veya Windows 2008 R2 için Windows bölümlemesine bir büyük mantıksal aygıt birden çok bağlı diskler oluşturmak için önerilir (Ayrıca bkz. bölüm [ Yazılım RAID] [ dbms-guide-2.2] bu belgenin). Bu yaklaşım, disk alanını yönetmek için yönetim yükünü basitleştirir ve takılı birden çok diskte dosyaları el ile dağıtmak için çaba önler.

#### <a name="backup--restore"></a>Yedekleme / Geri Yükleme
Yedekleme / geri yükleme işlevselliği, SAP BR * Oracle için Araçlar, standart Windows Server işletim sistemleri ve Hyper-V gibi aynı şekilde desteklenir. Oracle kurtarma Yöneticisi'ni (RMAN), disk ve disk, geri yüklemek yedeklemeler için de desteklenir.

#### <a name="high-availability"></a>Yüksek Kullanılabilirlik
Oracle Data Guard, yüksek kullanılabilirlik ve olağanüstü durum kurtarma amacıyla desteklenir. Ayrıntılar bulunabilir [bu] [ virtual-machines-windows-classic-configure-oracle-data-guard] belgeleri.

#### <a name="other"></a>Diğer
Azure kullanılabilirlik kümeleri veya SAP izleme gibi diğer tüm genel alanlarında de Oracle veritabanı içeren sanal makineleri dağıtımları için bu belgenin ilk üç bölümde açıklandığı gibi uygulayın.

## <a name="specifics-to-oracle-database-on-oracle-linux"></a>Oracle veritabanına Oracle Linux üzerinde özellikleri
Oracle yazılım, Microsoft Windows Hyper-V ve Azure üzerinde çalıştırmak için Oracle tarafından desteklenir. Windows Hyper-V ve Azure genel destek hakkında daha fazla bilgi için kontrol edin: <https://blogs.oracle.com/cloud/entry/oracle_and_microsoft_join_forces> 

Genel destek, belirli bir senaryoyu Oracle veritabanları yararlanarak SAP uygulamalarının de desteklenir. Ayrıntılar belgenin bu bölümü adlandırılır.

### <a name="oracle-version-support"></a>Oracle sürüm desteği
Oracle ve SAP Oracle Azure sanal makineler üzerinde çalışan SAP notta bulunabilir, desteklenen karşılık gelen işletim sistemi sürümleri [2039619].

SAP Business Suite Oracle üzerinde çalıştırma hakkında genel bilgi 1DX içinde bulunabilir: <https://www.sap.com/community/topic/oracle.html>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP yüklemelerde Azure VM'ler için Oracle yapılandırma yönergeleri
#### <a name="storage-configuration"></a>Depolama yapılandırması
Yalnızca Oracle ext3 kullanarak, diskleri ext4 ve xfs biçimlendirilmiş tek örnek desteklenir. Tüm veritabanı dosyaları, VHD'leri veya yönetilen diskleri göre bu dosya sistemlerine depolanmalıdır. Bu diskleri Azure VM'ye bağlanır ve Azure sayfa BLOB Depolama birimine dayalı (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) veya yönetilen diskleri (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Her türlü ağ sürücülerine veya Azure dosya services gibi uzak paylaşımları:

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx> 
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx>

olan **değil** Oracle veritabanı dosyaları için desteklenen!

Diskler kullanarak temel Azure sayfa BLOB Storage veya yönetilen diskleri, bu belgede bölüm yapılan deyimleri [VM'ler ve veri diskleri için önbelleğe alma] [ dbms-guide-2.1] ve [Microsoft Azure depolama] [ dbms-guide-2.3] Oracle veritabanı da dağıtımlar için geçerlidir.

Önceki belge Genel bölümünde açıklandığı gibi Azure disklerin IOPS verimlilik kotalar mevcut. Tam kotaları VM türüne bağlı olarak kullanılır. Kendi kotaları ile VM türlerinin bir listesini bulunabilir [burada (Linux)] [ virtual-machines-sizes-linux] ve [burada (Windows)][virtual-machines-sizes-windows].

Desteklenen Azure VM türler tanımlamak için SAP nota bakın [1928533]

Disk başına geçerli IOPS kota gereksinimlerini karşılayan sürece, tek tek bağlı disk üzerindeki tüm veritabanı dosyaları depolamak mümkündür. 

Daha fazla IOPS gerekirse, birden çok bağlı diskler üzerinde tek bir büyük mantıksal birim oluşturmak için LVM (mantıksal birim Yöneticisi) veya MDADM kullanmak için önerilir. Ayrıca bkz. bölüm [yazılım RAID] [ dbms-guide-2.2] bu belgenin. Bu yaklaşım, disk alanını yönetmek için yönetim yükünü basitleştirir ve takılı birden çok diskte dosyaları el ile dağıtmak için çaba önler.

#### <a name="backup--restore"></a>Yedekleme / Geri Yükleme
Yedekleme / geri yükleme işlevselliği, SAP BR * Oracle için Araçlar, çıplak metal ve Hyper-V gibi aynı şekilde desteklenir. Oracle kurtarma Yöneticisi'ni (RMAN), disk ve disk, geri yüklemek yedeklemeler için de desteklenir.

#### <a name="high-availability"></a>Yüksek Kullanılabilirlik
Oracle Data Guard, yüksek kullanılabilirlik ve olağanüstü durum kurtarma amacıyla desteklenir. Ayrıntılar bulunabilir [bu] [ virtual-machines-windows-classic-configure-oracle-data-guard] belgeleri.

#### <a name="other"></a>Diğer
Azure kullanılabilirlik kümeleri veya SAP izleme gibi diğer tüm genel alanlarında de Oracle veritabanı içeren sanal makineleri dağıtımları için bu belgenin ilk üç bölümde açıklandığı gibi uygulayın.

## <a name="specifics-for-the-sap-maxdb-database-on-windows"></a>Windows SAP MaxDB veritabanı için özellikleri
### <a name="sap-maxdb-version-support"></a>SAP MaxDB sürüm desteği
Şu anda destekler SAP MaxDB sürüm 7,9 Azure SAP NetWeaver tabanlı ürünleriyle kullanmak için SAP. SAP MaxDB sunucu veya SAP NetWeaver tabanlı ürünleri ile kullanılacak JDBC ve ODBC sürücüleri için tüm güncelleştirmeleri yalnızca SAP hizmet Market'ten aracılığıyla sağlanan <https://support.sap.com/swdc>.
SAP NetWeaver SAP MaxDB üzerinde çalıştırma hakkında genel bilgi adresinde bulunabilir <https://www.sap.com/community/topic/maxdb.html>.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-maxdb-dbms"></a>SAP MaxDB DBMS için desteklenen Microsoft Windows sürümleri ve Azure VM türleri
Azure üzerinde SAP MaxDB DBMS için desteklenen Microsoft Windows sürümünü bulmak için bkz:

* [SAP ürün kullanılabilirliği matrisi (PAM)][sap-pam]
* SAP Not [1928533]

Microsoft Windows 2012 R2 Microsoft Windows işletim sisteminin en yeni sürümü kullanmak için önerilir.

### <a name="available-sap-maxdb-documentation"></a>Kullanılabilir SAP MaxDB belgeleri
Aşağıdaki SAP notta SAP MaxDB belgelerine güncelleştirilmiş listesini bulabilirsiniz [767598]

### <a name="sap-maxdb-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP yüklemelerde Azure VM'ler için SAP MaxDB yapılandırma yönergeleri
#### <a name="b48cfe3b-48e9-4f5b-a783-1d29155bd573"></a>Depolama yapılandırması
SAP MaxDB için Azure depolama en iyi uygulamaları izleyin bölümde belirtildiği genel öneriler [RDBMS dağıtım yapısı][dbms-guide-2].

> [!IMPORTANT]
> Diğer veritabanlarında olduğu gibi SAP MaxDB veri ve günlük dosyalarını da sahiptir. Ancak, SAP MaxDB terminolojisinde doğru "Birim" ("dosyası değil") bir terimdir. Örneğin, SAP MaxDB vardır veri ve günlük birimler. Bu işletim sistemi disk birimleri ile karıştırmayın. 
> 
> 

Kısacası, gerekir:

* Azure depolama hesapları kullanırsanız, SAP MaxDB veri ve günlük birimler (yani dosyaları) tutan Azure depolama hesabı ayarlamak **yerel olarak yedekli depolama (LRS)** bölümde belirtildiği gibi [Microsoft Azure depolama][dbms-guide-2.3].
* Günlük birimler (yani dosyaları) için g/ç yolundan SAP MaxDB veri birimler (yani dosyaları) için g/ç yolu ayırın. Bunun anlamı SAP MaxDB veri birimler (yani dosyaları) bir mantıksal diske yüklenmesi gereken ve SAP MaxDB günlük birimler (yani dosyaları) başka bir mantıksal sürücüye yüklenmesi gerekir.
* SAP MaxDB veri veya günlük birimleri için (yani dosyaları) kullanıp ve Azure standart ya da Azure Premium depolama kullanmadığınıza bağlı olarak her disk için uygun önbelleğe alma türü bölümde açıklandığı gibi ayarlayın [VM'ler ve veri diskleri için önbelleğe alma] [dbms-guide-2.1].
* Disk başına geçerli IOPS kota gereksinimlerini karşılayan sürece, tüm veri miktarları tek bir bağlı diskte depolar ve ayrıca tüm veritabanı günlük birimleri başka bir tek takılı diskte depolar mümkündür.
* Daha fazla IOPS ve/veya boşluk gerekirse, bir büyük mantıksal aygıt birden çok oluşturmak üzere Microsoft penceresi depolama havuzları (yalnızca Microsoft Windows Server 2012'de kullanılabilir ve üzeri) veya Microsoft Windows 2008 R2 için Microsoft Windows şeritleme kullanmak için önerilir. bağlı diskler. Ayrıca bkz. bölüm [yazılım RAID] [ dbms-guide-2.2] bu belgenin. Bu yaklaşım, disk alanını yönetmek için yönetim yükünü basitleştirir ve takılı birden çok diskte dosyaları el ile dağıtma çaba önler.
* En yüksek IOPS gereksinimleri için DS serisi ve GS serisi Vm'lerde kullanılabilir olduğu Azure Premium Storage kullanabilirsiniz.

![Azure Iaas VM SAP MaxDB DBMS için başvuru yapılandırması][dbms-guide-figure-600]

#### <a name="23c78d3b-ca5a-4e72-8a24-645d141a3f5d"></a>Yedekleme ve geri yükleme
SAP MaxDB Azure'a dağıtırken, yedekleme yönteminize gözden geçirmeniz gerekir. Sistem üretken sistem olsa bile, SAP MaxDB tarafından barındırılan SAP veritabanında düzenli aralıklarla yedeklenmelidir. Azure Storage üç görüntüleri tutar, bir yedekleme şimdi sisteminizi depolama hatası ve daha önemli işlemsel veya yönetim hatalarına karşı koruma açısından daha az önemlidir. Böylece zaman içinde nokta kurtarma özellikleri sağlayarak mantıksal veya el ile hataları dengeleyebilirsiniz uygun yedekleme ve geri yükleme planı korumak için birincil nedenidir. Bu nedenle hedef ya da yedeklemeleri veritabanını zaman içinde belirli bir noktaya geri yüklemenizi veya varolan bir veritabanını kopyalayarak başka bir sistem oluşturmak için Azure'da yedekleri kullanmaya kullanmaktır. Örneğin, bir katman 2 SAP yapılandırmasından aynı sistem için bir 3 katmanlı sistemi Kurulum yedeklemesini geri yükleyerek aktaramıyor.

Yedekleme ve geri yükleme Azure veritabanında SAP notta listelenen SAP MaxDB belgelerine belgeleri birinde açıklanan standart SAP MaxDB yedekleme/geri yükleme araçlarını kullanabilmek için şirket içi sistemler için yaptığı gibi aynı şekilde çalışır [767598]. 

#### <a name="77cd2fbb-307e-4cbf-a65f-745553f72d2c"></a>Yedekleme ve geri yükleme için başarım düşünceleri
Çıplak dağıtımlar olduğu gibi yedekleme ve geri yükleme performans kaç birimleri paralel ve bu birimlerin verimini okunabilir üzerinde bağlıdır. Ayrıca, yedekleme sıkıştırma tarafından kullanılan CPU tüketimi önemli bir rol ile en fazla sekiz CPU iş parçacıkları Vm'lerinde yürütebilirsiniz. Bu nedenle, bir kabul edebilirsiniz:

* Daha az veritabanı aygıtlar, alt genel okuma verimini depolamak için kullanılan disk sayısı
* Daha küçük CPU sayısı yedekleme sıkıştırma etkisini VM, daha önemli iş parçacıkları
* Yedekleme için alt işleme yazmak için daha az hedefleri (Stripe dizinleri, diskler)

Yazılacak hedefleri sayısını artırmak için birlikte, büyük olasılıkla gereksinimlerinize bağlı olarak kullanabileceğiniz iki seçenek vardır:

* Yedekleme için ayrı birimlerin ayrılması
* Şeritli disk birimde IOPS verimini artırmak için birden fazla bağlı diskler üzerinde Yedekleme hedef birimi bölümlemesine
* Ayrı bir adanmış mantıksal disk aygıtları için sahip:
  * SAP MaxDB yedekleme birimler (yani dosyaları)
  * SAP MaxDB veri birimler (yani dosyaları)
  * SAP MaxDB günlük birimler (yani dosyaları)

Bir birim üzerinde birden çok bağlama diskleri bölümlemesine ele alınan önceki bölümde [yazılım RAID] [ dbms-guide-2.2] bu belgenin. 

#### <a name="f77c1436-9ad8-44fb-a331-8671342de818"></a>Diğer
Azure kullanılabilirlik kümeleri veya SAP izleme gibi diğer tüm genel alanlarında SAP MaxDB veritabanı VM'lerin dağıtımlar için bu belgenin ilk üç bölümde açıklandığı gibi de geçerlidir.
Diğer SAP MaxDB özgü ayarları Azure VM'ler için saydamdır ve SAP notta listelenen farklı belgelerinde açıklanan [767598] ve bu SAP Notlar:

* [826037] 
* [1139904]
* [1173395]

## <a name="specifics-for-sap-livecache-on-windows"></a>SAP liveCache Windows özellikleri
### <a name="sap-livecache-version-support"></a>SAP liveCache sürüm desteği
SAP liveCache Azure sanal makinelerinde desteklenen en düşük sürümü **SAP LC/LCAPPS 10.0 SP 25** dahil olmak üzere **liveCache 7.9.08.31** ve **LCA yapı 25**, için serbest bırakıldı **EhP 2 SAP SCM 7.0** ve daha yüksek.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-livecache-dbms"></a>SAP liveCache DBMS desteklenen Microsoft Windows sürümleri ve Azure VM türleri
Azure üzerinde SAP liveCache için desteklenen Microsoft Windows sürümünü bulmak için bkz:

* [SAP ürün kullanılabilirliği matrisi (PAM)][sap-pam]
* SAP Not [1928533]

Microsoft Windows Server işletim sisteminin en yeni sürümü kullanmak için önerilir. 

### <a name="sap-livecache-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP liveCache SAP yüklemelerde Azure VM'ler için yapılandırma yönergeleri
#### <a name="recommended-azure-vm-types"></a>Azure VM türleri önerilir
SAP liveCache büyük hesaplamaları gerçekleştiren bir uygulama olarak, RAM ve CPU hızı ve miktarı SAP liveCache performans üzerinde büyük bir etkisi vardır. 

SAP tarafından desteklenen Azure VM türleri için (SAP Not [1928533]), VM için ayrılan tüm sanal CPU kaynaklarını ayrılmış fiziksel CPU kaynaklarına göre hiper yönetici tarafından desteklenir. Hiçbir işleminin (ve bu nedenle hiçbir rekabet CPU kaynakları için) yerini alır.

Benzer şekilde, SAP tarafından desteklenen tüm Azure VM örneği türleri için VM bellek (atlayarak taahhüt), örneğin, işleminin fiziksel belleği - eşlenen % 100 değil kullanılır.

A-series daha % 60 daha hızlı işlemcilere sahip oldukları gibi bu açısından bakıldığında, yeni D-serisi veya (kombinasyondaki Azure Premium Storage ile) DS serisi Azure VM türünü kullanmak için önerilir. En yüksek RAM ve CPU yükü G-serisi ve GS serisi (kombinasyondaki Azure Premium Storage ile) VM'ler ile en son Intel kullanabileceğiniz?? Xeon?? İşlemci iki kez bellek ve dört E5 v3 ailesi D/DS serisi katı hal sürücüsü depolama (SSD) zaman.

#### <a name="storage-configuration"></a>Depolama Yapılandırması
SAP liveCache SAP MaxDB teknolojisine dayalı olarak, tüm Azure depolama en iyi yöntem önerileri için SAP MaxDB bölümde belirtildiği [depolama yapılandırması] [ dbms-guide-8.4.1] da SAP liveCache için geçerlidir. 

#### <a name="dedicated-azure-vm-for-livecache"></a>Ayrılmış Azure VM liveCache için
SAP liveCache yoðun hesaplama gücüne kullandıkça verimli kullanım için ayrılmış bir Azure sanal makine dağıtmak için önerilir. 

![Azure VM verimli kullanım örneği için liveCache için ayrılmış][dbms-guide-figure-700]

#### <a name="backup-and-restore"></a>Yedekleme ve Geri Yükleme
Yedekleme ve geri yükleme, performans konuları dahil olmak üzere ilgili SAP MaxDB bölümlerde açıklanmıştır zaten [yedekleme ve geri yükleme] [ dbms-guide-8.4.2] ve [yedekleme için başarım düşünceleri ve geri yükleme][dbms-guide-8.4.3]. 

#### <a name="other"></a>Diğer
Tüm genel alana zaten ilgili SAP MaxDB içinde açıklanan [bu] [ dbms-guide-8.4.4] bölüm. 

## <a name="specifics-for-the-sap-content-server-on-windows"></a>Windows üzerinde SAP içerik sunucusu özellikleri
SAP içerik sunucusu, elektronik belgeler gibi içerik farklı biçimlerde depolamak için ayrı, sunucu tabanlı bir bileşendir. SAP içerik sunucusu teknolojisi geliştirme tarafından sağlanır ve herhangi bir SAP uygulaması için kullanılan çapraz uygulama olacak. Ayrı bir sistemde yüklü. Tipik içerik eğitim malzemesi ve Bilgi Bankası ambarı veya mySAP PLM belge yönetim sistemi kaynaklanan teknik çizimler belgelerinden ' dir. 

### <a name="sap-content-server-version-support"></a>SAP içerik Server sürüm desteği
SAP şu anda destekler:

* **SAP içerik sunucusu** sürümüyle **6.50 (ve üzeri)**
* **SAP MaxDB sürüm 7,9**
* **Microsoft IIS (Internet Information Server) sürüm 8.0 (ve üzeri)**

SAP içerik bu belgenin yazıldığı zaman sunucu en yeni sürümünü kullanmak için önerilir **6.50 SP4**ve en yeni sürümünü **Microsoft IIS 8.5**. 

Desteklenen en son sürümlerini SAP içerik sunucusu ve Microsoft IIS denetleyin [SAP ürün kullanılabilirlik matris (PAM)][sap-pam].

### <a name="supported-microsoft-windows-and-azure-vm-types-for-sap-content-server"></a>SAP içerik sunucusu için desteklenen Microsoft Windows ve Azure VM türleri
Azure üzerinde SAP içerik sunucusu için desteklenen Windows sürümü bulmak için bkz:

* [SAP ürün kullanılabilirliği matrisi (PAM)][sap-pam]
* SAP Not [1928533]

Microsoft Windows Server'ın en yeni sürümü kullanmak için önerilir.

### <a name="sap-content-server-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP yüklemelerde Azure VM'ler için SAP içerik sunucusu yapılandırma yönergeleri
#### <a name="storage-configuration"></a>Depolama Yapılandırması
SAP MaxDB veritabanında dosyaları depolamak için SAP içerik sunucusu yapılandırırsanız, SAP MaxDB bölümde belirtildiği öneri tüm Azure depolama en iyi yöntemler [depolama yapılandırması] [ dbms-guide-8.4.1] da geçerlidir SAP içerik sunucusu senaryosu için. 

Dosya sistemindeki dosyaları depolamak için SAP içerik sunucusu yapılandırırsanız, adanmış bir mantıksal sürücü kullanmak için önerilir. Windows depolama alanları'nı kullanarak sağlar, ayrıca mantıksal disk boyutu ve IOPS verimliliği artırmak bölümde açıklandığı gibi [yazılım RAID][dbms-guide-2.2]. 

#### <a name="sap-content-server-location"></a>SAP içerik sunucusu konumu
SAP içerik sunucusu aynı Azure bölgesinde ve Azure SAP sisteminin dağıtıldığı sanal dağıtılması gerekir. SAP içerik sunucusu bileşenlerini adanmış bir Azure VM veya SAP sistem çalıştığı aynı VM dağıtmak isteyip istemediğinize karar vermek boş. 

![Azure VM SAP içerik sunucusu için ayrılmış][dbms-guide-figure-800]

#### <a name="sap-cache-server-location"></a>SAP önbellek sunucusu konumu
SAP önbellek sunucusu yerel olarak (önbelleğe alınmış) belgelere erişimi sağlamak için bir ek sunucu tabanlı bileşendir. SAP önbellek sunucusu bir SAP içerik sunucusu belgeleri önbelleğe alır. Farklı konumlardan birden çok kez alınacak belgeler varsa, ağ trafiğini en iyi duruma getirme budur. Genel kural SAP önbellek sunucusu SAP önbellek sunucusu erişen istemci yakın fiziksel olarak sahip olur. 

Burada, iki seçeneğiniz vardır:

1. **İstemci olan bir arka uç SAP sistemi** SAP içerik sunucusuna erişmek için bir arka uç SAP sistemi yapılandırdıysanız, bu SAP sistem bir istemcidir. SAP sistem ve SAP içerik sunucusu aynı Azure bölgesinde aynı Azure veri merkezinde dağıtılan fiziksel olarak diğer yakın olan. Bu nedenle, adanmış bir SAP önbellek sunucusu sağlamak için gerek yoktur. SAP UI istemcileri (SAP GUI ya da web tarayıcısı) SAP sistem doğrudan erişmek ve SAP sistem belgeleri SAP içerik sunucusundan alır.
2. **İstemci bir şirket içi web tarayıcısı olan** SAP içerik sunucusu, web tarayıcısı tarafından doğrudan erişilebilmesi için yapılandırılabilir. Bu durumda, şirket içi çalışan bir web tarayıcısı SAP içerik sunucusunun bir istemcidir. Şirket içi veri merkezi ve Azure veri merkezi (İdeal olarak yakın birbirine) farklı fiziksel konumlarda yerleştirilir. Şirket içi veri merkeziniz Azure Azure siteden siteye VPN ya da ExpressRoute üzerinden bağlanır. Her iki seçenek güvenli VPN ağ bağlantısı için Azure sunmasına rağmen siteden siteye ağ bağlantısı şirket içi veri merkezi ve Azure veri merkezi arasında ağ bant genişliği ve gecikme süresi SLA sağlamaz. Belgelere erişimi hızlandırmak için aşağıdakilerden birini yapabilirsiniz:
   1. Şirket içi SAP önbellek Server yükleyin, kapatmak için şirket içi web tarayıcısında (seçeneği [bu] [ dbms-guide-900-sap-cache-server-on-premises] Şekil)
   2. Şirket içi veri merkezi ve Azure veri merkezi arasında yüksek hızda ve düşük gecikme süreli Adanmış ağ bağlantısı sağlayan Azure ExpressRoute yapılandırın.

![Şirket içi SAP önbellek Server yüklemek için seçeneği][dbms-guide-figure-900]
<a name="642f746c-e4d4-489d-bf63-73e80177a0a8"></a>

#### <a name="backup--restore"></a>Yedekleme / Geri Yükleme
SAP MaxDB veritabanında dosyaları depolamak için SAP içerik sunucusu yapılandırırsanız, yedekleme/geri yükleme yordamı ve performans konuları zaten SAP MaxDB bölümde açıklanan [yedekleme ve geri yükleme] [ dbms-guide-8.4.2]ve bölüm [yedekleme ve geri yükleme için başarım düşünceleri][dbms-guide-8.4.3]. 

Dosya sistemindeki dosyaları depolamak için SAP içerik sunucusu yapılandırırsanız, bir el ile yedekleme/geri yükleme dosyanın tamamı yapısının yürütmek için belgeleri bulunduğu seçenektir. Benzer şekilde SAP MaxDB yedekleme/geri yükleme, yedekleme amaç için ayrılmış disk birimi sağlamak için önerilir. 

#### <a name="other"></a>Diğer
Diğer SAP içerik sunucusuna özgü ayarları Azure VM'ler için saydamdır ve çeşitli belgeler ve SAP notları açıklanmıştır:

* <https://service.sap.com/contentserver> 
* SAP Not [1619726]  

## <a name="specifics-to-ibm-db2-for-luw-on-windows"></a>IBM DB2 yazıp yazmayacağını LUW Windows için
Microsoft Azure ile varolan SAP uygulamanız üzerindeki IBM DB2 Linux, UNIX ve Windows (LUW) çalışan Azure sanal makinelerini kolayca geçirebilirsiniz. IBM DB2 LUW için üzerinde SAP ile yöneticiler ve geliştiriciler mevcut şirket içi yönetim araçları ve aynı geliştirme kullanmaya devam edebilirsiniz.
LUW adresindeki SAP topluluk ağ (SCN) içinde bulunabilir SAP Business Suite IBM DB2 üzerinde çalışan hakkında genel bilgi <https://www.sap.com/community/topic/db2-for-linux-unix-and-windows.html>.

Ek bilgi ve Azure üzerinde LUW DB2 üzerinde SAP ilgili güncelleştirmeler için bkz. SAP Not [2233094]. 

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 Linux, UNIX ve Windows sürüm desteği
IBM DB2 için Microsoft Azure sanal makine hizmetleri üzerinde LUW üzerinde SAP DB2 sürüm 10.5 sürümünden itibaren desteklenir.

Desteklenen SAP ürünler ve Azure VM türleri hakkında daha fazla bilgi için SAP nota bakın [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 Linux, UNIX ve SAP yüklemelerde Azure VM'ler için Windows yapılandırma yönergeleri
#### <a name="storage-configuration"></a>Depolama Yapılandırması
Tüm veritabanı dosyaları, doğrudan bağlı disklerde dayalı NTFS dosya sistemi depolanmalıdır. Bu diskleri Azure VM'ye bağlanır ve Azure sayfa BLOB depolama alanına bağlı (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) veya yönetilen diskleri (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Her türlü ağ sürücülerine veya aşağıdaki Azure Dosya Hizmetleri gibi uzak paylaşımlara **değil** veritabanı dosyaları için desteklenir: 

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx>

Azure sayfa BLOB Storage veya yönetilen diskleri temel diskleri kullanıyorsanız, ifadeler bölümde bu belgedeki [RDBMS dağıtım yapısı] [ dbms-guide-2] LUW IBM DB2 dağıtımlar için de geçerlidir Veritabanı. 

Önceki belge Genel bölümünde açıklandığı gibi diskleri için IOPS işleme kotalar mevcut. Tam kotaları kullanılan VM türüne bağlıdır. Kendi kotaları ile VM türlerinin bir listesini bulunabilir [burada (Linux)] [ virtual-machines-sizes-linux] ve [burada (Windows)][virtual-machines-sizes-windows].

Disk başına geçerli IOPS kota yeterli olduğu sürece, tek tek bağlı disk üzerindeki tüm veritabanı dosyalarını depolamak mümkündür. 

İçin performans konuları da bölüm 'Veri güvenliği ve veritabanı dizinler için başarım düşünceleri' SAP yükleme kılavuzlarında bakın.

Alternatif olarak, Windows depolama havuzları (yalnızca Windows Server 2012'de kullanılabilir ve üzeri) kullanabilir veya Windows 2008 R2 Windows şeritleme bölümde açıklanan [yazılım RAID] [ dbms-guide-2.2] bu belgenin büyük bir mantıksal aygıt birden çok disk oluşturun.
Sapdata ve saptmp dizinlerinizi DB2 depolama yollarını içeren diskler için bir fiziksel disk sektör boyutu 512 KB belirtmeniz gerekir. Windows depolama havuzlarını kullanırken depolama havuzu parametresini kullanarak komut satırı arabirimi kullanarak el ile oluşturmanız gerekir `-LogicalSectorSizeDefault`. Daha fazla bilgi için bkz: <https://technet.microsoft.com/itpro/powershell/windows/storage/new-storagepool>.

#### <a name="backuprestore"></a>Yedekleme/Geri Yükleme
Yedekleme/geri yükleme işlevselliği LUW IBM DB2 için aynı şekilde standart Windows Server işletim sistemi ve Hyper-V üzerinde desteklenir.

Geçerli bir veritabanı yedekleme stratejisi yerinde olduğundan emin olmanız gerekir. 

Çıplak dağıtımlar olduğu gibi yedekleme/geri yükleme performans paralel şekilde kaç tane birimleri okunabilir ve ne bu birimlerin verimini olabilir bağlıdır. Yedekleme sıkıştırma tarafından kullanılan CPU tüketimi Vm'lerinde olan en fazla sekiz CPU iş parçacıklarının Ayrıca, önemli bir rol oynayabilir. Bu nedenle, bir kabul edebilirsiniz:

* Daha az veritabanı aygıtları depolamak için kullanılan disk sayısı daha küçük genel üretilen işi okuma
* Daha küçük CPU sayısı yedekleme sıkıştırma etkisini VM, daha önemli iş parçacıkları
* Yedekleme için alt işleme yazmak için daha az hedefleri (Stripe dizinleri, diskler)

Yazılacak hedefleri sayısını artırmak için iki seçenek, gereksinimlerinize bağlı olarak kullanılan/birleşik olabilir:

* Şeritli birim üzerinde IOPS üretilen işi artırmak için birden çok disk üzerinde Yedekleme hedef birimi bölümlemesine
* Yedekleme yazmak için birden fazla hedef dizini kullanma

#### <a name="high-availability-and-disaster-recovery"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma
Microsoft Küme sunucusu (MSCS) desteklenmiyor.

DB2 yüksek kullanılabilirlik olağanüstü durum kurtarma (HADR) desteklenir. HA yapılandırma sanal makinelerin ad çözümlemesi çalışma varsa, Azure kurulumunda şirket içi yapılan herhangi bir ayar farklı. Yalnızca IP çözümlemesine dayanan için önerilmez.

Coğrafi çoğaltma veritabanı disklerini depolamak depolama hesapları için kullanmayın. Daha fazla bilgi için bölüme başvuran [Microsoft Azure depolama] [ dbms-guide-2.3] ve bölüm [yüksek kullanılabilirlik ve olağanüstü durum kurtarma Azure vm'lerle] [ dbms-guide-3].

#### <a name="other"></a>Diğer
IBM DB2 LUW için olan VM'ler de dağıtımları için bu belgenin ilk üç bölümde açıklandığı gibi Azure kullanılabilirlik kümeleri veya SAP izleme gibi diğer tüm genel alanlarında uygulayın. 

Ayrıca bir bölüme başvurmak [Genel SQL Server için Azure özeti SAP][dbms-guide-5.8].

## <a name="specifics-to-ibm-db2-for-luw-on-linux"></a>IBM DB2 yazıp yazmayacağını Linux'ta LUW için
Microsoft Azure ile varolan SAP uygulamanız üzerindeki IBM DB2 Linux, UNIX ve Windows (LUW) çalışan Azure sanal makinelerini kolayca geçirebilirsiniz. IBM DB2 LUW için üzerinde SAP ile yöneticiler ve geliştiriciler mevcut şirket içi yönetim araçları ve aynı geliştirme kullanmaya devam edebilirsiniz. LUW adresindeki SAP topluluk ağ (SCN) içinde bulunabilir SAP Business Suite IBM DB2 üzerinde çalışan hakkında genel bilgi <https://www.sap.com/community/topic/db2-for-linux-unix-and-windows.html>.

Ek bilgi ve Azure üzerinde LUW DB2 üzerinde SAP ilgili güncelleştirmeler için bkz. SAP Not [2233094].

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 Linux, UNIX ve Windows sürüm desteği
IBM DB2 için Microsoft Azure sanal makine hizmetleri üzerinde LUW üzerinde SAP DB2 sürüm 10.5 sürümünden itibaren desteklenir.

Desteklenen SAP ürünler ve Azure VM türleri hakkında daha fazla bilgi için SAP nota bakın [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 Linux, UNIX ve SAP yüklemelerde Azure VM'ler için Windows yapılandırma yönergeleri
#### <a name="storage-configuration"></a>Depolama Yapılandırması
Tüm veritabanı dosyaları, doğrudan bağlı disklerde tabanlı bir dosya sisteminde depolanmalıdır. Bu diskleri Azure VM'ye bağlanır ve Azure sayfa BLOB depolama alanına bağlı (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) veya yönetilen diskleri (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Her türlü ağ sürücülerine veya aşağıdaki Azure Dosya Hizmetleri gibi uzak paylaşımlara **değil** veritabanı dosyaları için desteklenir:

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx>

Azure sayfa BLOB Storage, bu belgede bölüm yapılan deyimleri temel diskleri kullanıyorsanız [RDBMS dağıtım yapısı] [ dbms-guide-2] LUW veritabanı için IBM DB2 dağıtımlar için de geçerlidir.

Önceki belge Genel bölümünde açıklandığı gibi diskleri için IOPS işleme kotalar mevcut. Tam kotaları kullanılan VM türüne bağlıdır. Kendi kotaları ile VM türlerinin bir listesini bulunabilir [burada (Linux)] [ virtual-machines-sizes-linux] ve [burada (Windows)][virtual-machines-sizes-windows].

Disk başına geçerli IOPS kota yeterli olduğu sürece, tek tek disk üzerindeki tüm veritabanı dosyalarını depolamak mümkündür.

İçin performans konuları da bölüm 'Veri güvenliği ve veritabanı dizinler için başarım düşünceleri' SAP yükleme kılavuzlarında bakın.

Alternatif olarak, LVM (mantıksal birim Yöneticisi) veya MDADM bölümde açıklandığı gibi kullanabileceğiniz [yazılım RAID] [ dbms-guide-2.2] bir büyük mantıksal aygıt birden çok disk oluşturmak için bu belgenin.
Sapdata ve saptmp dizinlerinizi DB2 depolama yollarını içeren diskler için bir fiziksel disk sektör boyutu 512 KB belirtmeniz gerekir.

#### <a name="backuprestore"></a>Yedekleme/Geri Yükleme
Yedekleme/geri yükleme işlevselliği LUW IBM DB2 için aynı şekilde standart Linux yükleme şirket içi üzerinde desteklenir.

Geçerli bir veritabanı yedekleme stratejisi yerinde olduğundan emin olmanız gerekir.

Çıplak dağıtımlar olduğu gibi yedekleme/geri yükleme performans paralel şekilde kaç tane birimleri okunabilir ve ne bu birimlerin verimini olabilir bağlıdır. Yedekleme sıkıştırma tarafından kullanılan CPU tüketimi Vm'lerinde olan en fazla sekiz CPU iş parçacıklarının Ayrıca, önemli bir rol oynayabilir. Bu nedenle, bir kabul edebilirsiniz:

* Daha az veritabanı aygıtları depolamak için kullanılan disk sayısı daha küçük genel üretilen işi okuma
* Daha küçük CPU sayısı yedekleme sıkıştırma etkisini VM, daha önemli iş parçacıkları
* Yedekleme için alt işleme yazmak için daha az hedefleri (Stripe dizinleri, diskler)

Yazılacak hedefleri sayısını artırmak için iki seçenek, gereksinimlerinize bağlı olarak kullanılan/birleşik olabilir:

* Şeritli birim üzerinde IOPS üretilen işi artırmak için birden çok disk üzerinde Yedekleme hedef birimi bölümlemesine
* Yedekleme yazmak için birden fazla hedef dizini kullanma

#### <a name="high-availability-and-disaster-recovery"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma
DB2 yüksek kullanılabilirlik olağanüstü durum kurtarma (HADR) desteklenir. HA yapılandırma sanal makinelerin ad çözümlemesi çalışma varsa, Azure kurulumunda şirket içi yapılan herhangi bir ayar farklı. Yalnızca IP çözümlemesine dayanan için önerilmez.

Coğrafi çoğaltma veritabanı disklerini depolamak depolama hesapları için kullanmayın. Daha fazla bilgi için bölüme başvuran [Microsoft Azure depolama] [ dbms-guide-2.3] ve bölüm [yüksek kullanılabilirlik ve olağanüstü durum kurtarma Azure vm'lerle] [ dbms-guide-3].

#### <a name="other"></a>Diğer
Azure kullanılabilirlik kümeleri veya SAP izleme gibi tüm diğer genel konular, IBM DB2 LUW için olan VM'ler de dağıtımları için bu belgenin ilk üç bölümde açıklandığı gibi uygulayın.

Ayrıca bir bölüme başvurmak [Genel SQL Server için Azure özeti SAP][dbms-guide-5.8].

