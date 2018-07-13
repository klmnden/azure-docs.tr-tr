---
title: Azure sanal makineleri DBMS dağıtım için SAP NetWeaver | Microsoft Docs
description: Azure sanal makineleri DBMS dağıtım için SAP NetWeaver
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
ms.openlocfilehash: 2caa9a5137edd4e012adf704c01dc5c470e1bb51
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38972453"
---
# <a name="azure-virtual-machines-dbms-deployment-for-sap-netweaver"></a>Azure sanal makineleri DBMS dağıtım için SAP NetWeaver
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

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Bu kılavuz belgeleri Microsoft Azure üzerinde SAP yazılım dağıtma ve uygulama'nın bir parçasıdır. Bu kılavuzu okumadan önce okuma [planlama ve Uygulama Kılavuzu][planning-guide]. Bu belgede, çeşitli ilişkisel veritabanı yönetim sistemleri (RDBMS) ve ilgili ürünler Microsoft Azure Virtual Machines'de (VM'ler) SAP ile birlikte bir hizmet (Iaas) özellikleri Azure altyapısı kullanılarak dağıtımını kapsar.

Kağıt yüklemeleri ve SAP yazılım dağıtımları için birincil kaynakları temsil eden platformları üzerinde SAP notları ve SAP yükleme belgelerine tamamlar.

## <a name="general-considerations"></a>Temel noktalar
Bu bölümde, Azure Vm'lerinde SAP ilgili DBMS sistemleri çalıştırma konuları kullanıma sunulmuştur. Bu bölümde belirli DBMS sistemleri için birkaç başvurular var. Bunun yerine belirli DBMS sistemleri içinde bu yazıda, sonra bu bölümde ele alınır.

### <a name="definitions-upfront"></a>Ön maliyet tanımları
Belgede aşağıdaki terimleri kullanırız:

* Iaas: Hizmet olarak altyapı.
* PaaS: Hizmet olarak Platform.
* SaaS: Hizmet olarak yazılım.
* SAP bileşeni: tek bir SAP uygulamayı ECC, BW, çözüm Yöneticisi veya EP'deki gibi  SAP bileşenleri geleneksel ABAP veya Java teknolojileri ya da bir olmayan-NetWeaver tabanlı uygulama iş nesneleri gibi temel alabilir.
* SAP ortamı: bir veya daha fazla SAP bileşenleri geliştirme, QAS, eğitim, DR veya üretim gibi bir iş işlevi gerçekleştirmek için mantıksal olarak gruplandırılır.
* SAP ortamı: Bu bir müşterinin tüm SAP varlıkları başvuruyor BT yatay. SAP ortamı, tüm üretim ve üretim dışı ortamlar içerir.
* SAP sistemine: DBMS katmanı ve uygulama katmanı, örneğin, bir SAP ERP geliştirme sisteminin, SAP BW test sistemi, SAP CRM üretim sistemine vb. bileşimi. Azure dağıtımında, bu iki katmanı şirket içi ile Azure arasında bölmek için desteklenmiyor. Şirket içi SAP sistemine ya da bu anlamına gelir dağıtılan veya Azure'da dağıtılır. Ancak, Azure'da veya şirket içi bir SAP ortamının farklı sistemleri dağıtabilirsiniz. Örneğin, SAP CRM geliştirme dağıtma ve Azure ancak SAP CRM üretim sistemi şirket içi sistemleri test edin.
* Yalnızca bulutta yer alan dağıtım: Burada Azure aboneliğine bağlı bir siteden siteye veya ExpressRoute bağlantısı şirket içi ağ altyapısına aracılığıyla bir dağıtım. Yaygın olarak kullanılan Azure belgeleri bu tür dağıtımlar da "Salt bulut" dağıtımları açıklanmıştır. Bu yöntem ile dağıtılan sanal makinelerin İnternet'e ve Azure Vm'leri için atanan ortak Internet uç noktaları aracılığıyla erişilir. Şirket içi Active Directory (AD) ve DNS Azure'da bu tür dağıtımlar için genişletilmiş değil. Bu nedenle sanal makineleri şirket içi Active Directory'nin parçası değildir. Not: Yalnızca bulut dağıtımlarına bu belgede, Azure Active Directory veya ad çözümlemesi uzantısı olmadan, yalnızca şirket içinden genel buluta çalıştıran tüm SAP ortamlarını olarak tanımlanır. Yalnızca bulut yapılandırmaları, üretim SAP sistemlerini veya burada barındırılan Azure ve şirket içinde bulunan kaynaklar üzerinde SAP sistemlerini arasında kullanılacak SAP STMS veya diğer şirket içi kaynaklara gerek yapılandırmaları için desteklenmez.
* Şirket içi: siteden siteye, çok siteli veya ExpressRoute bağlantısı şirket içi datacenter(s) ve Azure arasında olan bir Azure aboneliğine VM'ler dağıtıldığı bir senaryo açıklanmaktadır. Azure ortak belgeler, bu tür dağıtımlar şirketler arası senaryoları açıklanmıştır. Bağlantı için şirket içi etki alanları, şirket içi Active Directory ve şirket içi DNS Azure'a genişletmek için nedenidir. Şirket içi yatay aboneliğin Azure varlıkları için genişletilir. Bu uzantı, sanal makineleri şirket içi etki alanının parçası olabilir. Şirket içi etki alanının etki alanı kullanıcıları sunucularına erişebilir ve Hizmetleri (gibi hizmetleri DBMS) bu sanal makineler üzerinde çalıştırabilir. Sanal makineler arasında iletişim ve ad çözümlemesi, şirket içi dağıtılan ve Azure'da dağıtılan Vm'leri mümkündür. Azure üzerinde SAP varlıklarını dağıtmak için en yaygın senaryo olmasını bekliyoruz. Daha fazla bilgi için [bu makalede] [ vpn-gateway-cross-premises-options] ve [bu makalede][vpn-gateway-site-to-site-create].

> [!NOTE]
> Şirket içi dağıtımlarını SAP sistemlerini SAP sistemlerini çalıştıran Azure sanal makineleri şirket içi etki alanının üyesi olduğu, üretim SAP sistemlerini için desteklenir. Şirketler arası yapılandırmalar bölümleri dağıtmak için desteklenen veya SAP ortamlarını Azure'a tamamlayın. Bile tam SAP ortamı Azure'da çalışan şirket içi etki alanı ve REKLAM parçası olan bu VM'nin bulunması gerekir. Hibrit BT senaryolarını hakkında konuştuk belgelerinin önceki sürümleri, burada terimi *karma* bir şirket içi ile Azure arasında şirketler arası bağlantı olduğunu aslında kökü belirtilmemiş. Bu durumda *karma* aynı zamanda Azure sanal makineleri şirket içi Active Directory'nin parçası olduğunu anlamına gelir.
> 
> 

Bazı Microsoft belge içi ve dışı karışık senaryo özellikle DBMS yüksek kullanılabilirlik yapılandırmaları için biraz farklı açıklar. SAP ile ilgili belgeler söz konusu olduğunda, bir siteden siteye veya özel (ExpressRoute) bağlantısı olmayan aşağı ve SAP yatay olgu içi ve dışı karışık senaryo boils dağıtılır şirket içi ile Azure arasında.

### <a name="resources"></a>Kaynaklar
Aşağıdaki kılavuzlar, Azure üzerinde SAP dağıtımları için kullanılabilir:

* [Azure sanal makineleri planlama ve uygulama için SAP NetWeaver][planning-guide]
* [Azure sanal makineler dağıtım için SAP NetWeaver][deployment-guide]
* [Azure sanal makineleri DBMS dağıtım için (Bu belgede) SAP NetWeaver][dbms-guide]

Azure'da SAP aşağıdaki SAP notları ilgili:

| Not numarası | Unvan |
| --- | --- |
| [1928533] |Azure'da SAP uygulamaları: desteklenen ürünler ve Azure VM türleri |
| [2015553] |Microsoft Azure üzerinde SAP: Destek önkoşulları |
| [1999351] |SAP için Gelişmiş Azure izleme sorunlarını giderme |
| [2178632] |Microsoft Azure üzerinde SAP için ölçümleri izleme anahtarı |
| [1409604] |Windows üzerinde sanallaştırma: Gelişmiş izleme |
| [2191498] |Azure ile Linux üzerinde SAP: Gelişmiş izleme |
| [2039619] |Oracle veritabanı'nı kullanarak Microsoft Azure üzerinde SAP uygulamaları: desteklenen ürünler ve sürümler |
| [2233094] |DB6: Linux, UNIX ve Windows - ek bilgi için IBM DB2 kullanarak azure'da SAP uygulamaları |
| [2243692] |Linux üzerinde Microsoft Azure (Iaas) sanal makine: SAP lisans sorunları |
| [1984787] |SUSE LINUX Enterprise Server 12: Yükleme notları |
| [2002167] |Red Hat Enterprise Linux 7.x: yükleme ve yükseltme |
| [2069760] |Oracle Linux 7.x SAP yükleme ve yükseltme |
| [1597355] |Linux için takas alanı önerisi |
| [2171857] |Oracle Database 12c - Linux üzerinde dosya sistemi desteği |
| [1114181] |Oracle veritabanı 11g - Linux üzerinde dosya sistemi desteği |


Ayrıca okuma [SCN Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) , Linux için tüm SAP notları içerir.

Microsoft Azure Mimarisi ve Microsoft Azure sanal makineleri nasıl dağıtılan ve çalıştırılan hakkında bilgiye sahip olmalıdır. Daha fazla bilgi bulabilirsiniz. <https://azure.microsoft.com/documentation/>

> [!NOTE]
> Duyuyoruz **değil** Microsoft Azure platformu Microsoft Azure platformunun hizmet (PaaS) teklifi olarak ele. Bu raporda, şirket içi ortamınızda DBMS çalıştırma gibi bir veritabanı yönetim sistemi (DBMS) Microsoft Azure sanal makinelerinde (Iaas) hakkında çalışıyor. Veritabanı özellikleri ve işlevleri bu iki teklifler arasında çok farklıdır ve birbiriyle karışmasını değil. Ayrıca bkz: <https://azure.microsoft.com/services/sql-database/>
> 
> 

Biz Iaas görüştükten sonra genel olarak Windows, Linux ve DBMS yükleme ve yapılandırma temel olarak herhangi bir sanal makine ya da şirket içi yüklenir çıplak metal makine aynıdır. Ancak, Iaas kullanırken farklı uygulama kararlar bazı mimarisi ve sistem yönetim vardır. Bu belgenin amacı, özel mimari ve, Iaas kullanırken hazırlıklı olmam gereken sistem yönetimi farkları açıklamaktadır sağlamaktır.

Genel olarak, bu belgede incelenen farkı genel şunlardır:

* Uygun veri sağlamak için SAP sistemlerini uygun VM/disk yerleşimini planlama Düzen dosyası ve iş yükünüz için yeterli IOPS değerlerine ulaşabilir.
* Iaas kullanılırken ağ konuları.
* Veritabanı düzenini iyileştirmek için kullanılacak belirli veritabanı özellikleri'ni kullanın.
* Iaas yedekleme ve geri yükleme konuları.
* Farklı dağıtım görüntülerin özelliğinden yararlanma.
* Azure Iaas yüksek kullanılabilirlik.

## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>Bir RDBMS dağıtım yapısı
Bu bölümde takip etmek için hangi sunulup sunulmadığı anlamak için gerekli olduğu [bu] [ deployment-guide-3] bölüm [Dağıtım Kılavuzu][deployment-guide]. Farklı VM serisi ve farkları ve farkları Azure standart ve Premium depolama hakkında bilgi anladım ve bu bölümü okumadan önce bilinen.

Mart 2015 tarihine kadar bir işletim sistemini içeren diskler için 127 GB cinsinden boyutu sınırlı. Bu sınırlama Mart 2015'te yükseltilmiş (daha fazla bilgi olup olmadığını kontrol edin <https://azure.microsoft.com/blog/2015/03/25/azure-vm-os-drive-limit-octupled/>). İşletim sistemini içeren disklerde buradan başka bir disk olarak aynı boyutta olabilir. Bununla birlikte, bir işletim sistemi, DBMS ve nihai SAP ikili dosyaları veritabanı dosyalarından ayrı olduğu dağıtım yapısı tercih devam ederiz. Bu nedenle, yüklü işletim sistemi, veritabanı yönetim sistemi yürütülebilir dosyalarının ve SAP yürütülebilir dosyaları ile temel VM (veya disk), Azure sanal Makineler'de çalışan SAP sistemlerini sahip bekliyoruz. DBMS veri ve günlük dosyalarını ayrı disklerin (standart veya Premium depolama) Azure Depolama'da depolanan ve özgün Azure işletim sistemi görüntüsüne VM bağlı mantıksal diskleri. 

Azure standart veya Premium depolama (örneğin DS serisi veya GS serisi Vm'leri kullanarak) var. yararlanarak üzerinde bağımlıdır belgelenen diğer kotaların azure'da [burada (Linux)] [ virtual-machines-sizes-linux] ve [ Burada (Windows)][virtual-machines-sizes-windows]. Disk düzeninizi planlarken kotaları şu öğeler için en iyi dengeyi bulmanız gerekir:

* Veri dosyalarının sayısını.
* Dosyaları içeren disk sayısı.
* Tek bir diskin IOPS kotalar.
* Disk başına veri aktarım hızı.
* VM boyutu olası ek veri diskleri sayısı.
* VM toplam depolama aktarım hızı sağlar.

Azure, bir veri disk başına IOPS kotası zorlar. Bu kotalar, Azure standart depolama ve Premium depolama üzerinde barındırılan diskleri için farklıdır. G/ç gecikme ayrıca Etkenler daha iyi g/ç gecikme sunan Premium depolama ile iki depolama türleri arasında çok farklı değildir. Her biri farklı VM türleri, sınırlı sayıda iliştirebilir veri diskleri sahiptir. VM türleri yalnızca belirli Azure Premium depolama yararlanabilir başka bir kısıtlaması yoktur. Bu, belirli bir VM türüne karar yalnızca CPU ve bellek gereksinimleri de IOPS tarafından tarafından disk sayısını veya Premium depolama diski türü ile genellikle Ölçeklendirildi gecikme süresi ve disk aktarım hızı gereksinimleri yönlendirebilir değil anlamına gelir. Özellikle Premium depolama ile bir disk boyutu da IOPS ve her disk tarafından elde gereken aktarım hızı sayısı belirlenmesine.

Olgu toplam IOPS hızı bağlı disk sayısı ve VM boyutu tüm bağlı birlikte, kendi şirket içi dağıtım farklı olacak şekilde bir SAP sistemiyle, bir Azure yapılandırması neden olabilir. LUN başına IOPS limitlerine genellikle şirket içi dağıtımlarında yapılandırılamaz. Azure depolama ile Premium depolama disk türüne bağlıdır olduğu gibi sabit olması veya bu sınırları ise. Bu nedenle şirket içi dağıtımları ile SAP DBMS veya geçici veritabanı veya tablo alanları için özel birimleri gibi özel yürütülebilir dosyalar için birçok farklı birimleri kullanılarak veritabanı sunucularına müşteri yapılandırmalarını görüyoruz. Böyle bir şirket içi sistem Azure'a taşındığında, yürütülebilir dosyalar veya ya da çok fazla IOPS değil gerçekleştirmeyin veritabanları için bir disk kaybettikten tarafından olası IOPS bant genişliği kaybı için neden olabilir. Bu nedenle, Azure Vm'lerinde DBMS ve SAP yürütülebilir dosyaları mümkünse işletim sistemi diskinde yüklenmesi önerilir.

Veritabanı dosyalarını ve günlük dosyaları ve Azure kullanılan depolamanın türü yerleşimini tanımlanmalıdır IOPS, gecikme süresi ve aktarım hızı gereksinimleri. İşlem günlüğü için yeterli IOPS sahip olmak için daha büyük bir Premium depolama diski kullanın ya da işlem günlüğü dosyası için birden çok diskler'den yararlanmaya zorlanabilirsiniz. Böyle bir durumda bir ' % s'yazılım RAID (için örnek Windows depolama havuzu Windows veya MDADM ve Linux için (mantıksal birim Yöneticisi) LVM için) işlem günlüğünü içeren disklerle oluşturmak.

- - -
> ![Windows][Logo_Windows] Windows
> 
> Azure VM'de D:\ Azure işlem düğümü üzerinde bazı yerel disk ile desteklenir, kalıcı olmayan sürücü sürücüdür. Kalıcı olmayan olduğundan, bu D:\ sürücüsüne içerikte yapılan tüm değişiklikler kayıp VM yeniden başlatıldığında anlamına gelir. "Herhangi bir değişiklik" demek isteriz kaydedilen dosyalar, oluşturulan dizinleri, yüklü uygulamalar vb.
> 
> ![Linux][Logo_Linux] Linux
> 
> Azure sanal makineleri otomatik olarak bir sürücü üzerinde Azure işlem düğümünün yerel disk ile desteklenir ve kalıcı olmayan bir sürücü /mnt/resource bağlayın. Kalıcı olmayan olduğundan, bu sanal makine yeniden başlatıldığında /mnt/resource içerikte yapılan tüm değişiklikler kaybedilir anlamına gelir. Herhangi bir değişiklik tarafından demek isteriz kaydedilen dosyaları, oluşturulan dizinleri, yüklü uygulamalar vb.
> 
> 

- - -
Azure VM serisi bağımlı işlem düğümünde yerel diskler gibi kategorilere ayrılabilir farklı performans göster:

* A0-A7: Çok sınırlı performans. Windows disk belleği dosyası dışında her şey için kullanılamaz
* A8-A11: Bazı on bin ıops'den çok iyi performans özellikleri ve > 1GB/sn aktarım hızı
* D serisi: Bazı on bin ıops'den çok iyi performans özellikleri ve > 1GB/sn aktarım hızı
* DS serisi: Bazı on bin ıops'den çok iyi performans özellikleri ve > 1GB/sn aktarım hızı
* G serisi: Bazı on bin ıops'den çok iyi performans özellikleri ve > 1GB/sn aktarım hızı
* GS serisi: Bazı on bin ıops'den çok iyi performans özellikleri ve > 1GB/sn aktarım hızı

Yukarıdaki deyimleri ile SAP sertifikalı sanal makine türleri için uygulamaya koyuyor. Mükemmel IOPS ve aktarım hızı ile VM serisi, tempdb veya geçici tablo alanı gibi bazı DBMS özellikleri tarafından yararlanmak için uygun.

### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>VM'ler ve veri diskleri için önbelleğe alma
Veri diskleri portalı üzerinden veya biz vm'lere karşıya yüklenen diskleri bağladığınızda oluşturduğumuzda, biz olmadığını VM ve Azure depolamada bulunan bu diskleri arasındaki g/ç trafiği önbelleğe alınmış seçebilirsiniz. Azure standart ve Premium depolama, iki farklı teknoloji önbellek bu tür için kullanın. Her iki durumda da, önbellek geçici disk VM tarafından (Windows üzerinde D:\) ya da Linux'ta /mnt/resource kullanılan sürücülerde desteklenen disk olacaktır.

Azure standart depolama için olası önbellek türleri şunlardır:

* Önbelleksizlik
* Önbelleğe alma okuyun
* Okuma ve yazma önbelleği

Tutarlı ve belirleyici performansı elde etmek için Azure standart depolama alanında içeren tüm diskler için önbelleğe almayı ayarlamalısınız **DBMS ile ilgili veri dosyaları, günlük dosyalarını ve tablo alanı için 'NONE'**. Sanal makinenin önbelleğe almayı varsayılan kalabilir.

Azure Premium depolama için aşağıdaki önbelleğe alma seçenekleri mevcuttur:

* Önbelleksizlik
* Önbelleğe alma okuyun

Azure Premium depolama önerilir yararlanmak için **veri dosyaları için önbelleğe alma okuma** SAP veritabanı ve seçtiğiniz **günlük dosyalarını şuraya diskler için önbelleğe alma**.

### <a name="c8e566f9-21b7-4457-9f7f-126036971a91"></a>Yazılım RAID
Zaten belirtilen yukarıdaki yapılandırabileceğiniz disk sayısı arasında veritabanı dosyaları için gereken IOPS sayısını dengelemek gereken ve disk veya Premium depolama disk türüne maksimum IOPS Azure VM'deki sağlar. Diskler üzerinde IOPS yüküyle başa çıkmak için kolay bir yolu, farklı diskler üzerinde yazılım RAID oluşturmaktır. Ardından birkaç SAP DBMS veri dosyalarının yazılım RAID dışında gerekmez LUN'ları yerleştirin. Premium depolama kullanımını da iki ve üç beri düşünmek isteyebilirsiniz gereksinimlerine bağımlı farklı Premium depolama diskleri diskleri standart depolama alanına dayalı olarak daha yüksek IOPS kota sağlar. Azure Premium Depolama tarafından sağlanan önemli ölçüde daha iyi g/ç gecikme yanı sıra. 

Aynı işlem günlüğünün farklı DBMS sistemleri için geçerlidir. DBMS sistemleri dosyalardan biri yalnızca teker teker yazma beri kaç tanesinin daha fazla Tlog dosyaları ekleme ile yardımcı olmaz. Tek bir standart tabanlı depolama diskini sunabilir daha yüksek IOPS hızları gerekirse, üzerinde birden çok standart depolama disklerinin stripe veya Etkenler yazma düşük gecikme süreleri de sunar, daha yüksek IOPS oranları ötesinde daha büyük bir Premium depolama disk türüne kullanabilir miyim / İşlem günlüğüne işletim sistemi.

Yazılım RAID kullanarak favor, Azure dağıtımlarında yaşadı durumlar şunlardır:

* İşlem günlüğü/Yinele günlüğü için tek bir diske Azure sağladığından daha yüksek IOPS gerektirir. Yukarıda da belirtildiği gibi bu yazılım RAID kullanarak birden çok disk üzerinde bir LUN oluşturarak çözülebilir.
* G/ç iş yükü dağılımına farklı veri dosyaları üzerinde SAP veritabanı. Bu gibi durumlarda bir kota yerine genellikle ulaşmaktan bir veri dosyası oluşabilir. Diğer veri dosyalarını tek bir disk IOPS kotanız dolmak bile alamıyorsanız ise. Bu gibi durumlarda, bir yazılım RAID kullanarak birden çok disk üzerinde tek bir LUN oluşturun en kolay çözümdür. 
* Ne tam g/ç iş yükü veri dosyası başına olan ve yalnızca yaklaşık genel IOPS iş yükü DBMS karşı ne olduğunu bilmeniz bilmiyorum. Kolay yapmak bir LUN yazılım RAID yardımıyla oluşturmaktır. Bu LUN arkasında birden fazla disk kotaları toplamını ardından bilinen IOPS oranı karşılamak.

- - -
> ![Windows][Logo_Windows] Windows
> 
> Windows depolama alanları Windows Server 2012 veya üzeri çalıştırırsanız kullanmanızı öneririz. Daha önceki Windows sürümlerinde, Windows şeritleme daha etkilidir. Windows depolama havuzları ve depolama alanları, işletim sistemi olarak Windows Server 2012 kullanırken PowerShell komutlarıyla oluşturmanız gerekebilir. PowerShell komutlarını buradan ulaşabilirsiniz <https://technet.microsoft.com/library/jj851254.aspx>
> 
> ![Linux][Logo_Linux] Linux
> 
> Yalnızca MDADM ve LVM (mantıksal birim Yöneticisi) bir Linux'ta yazılım RAID oluşturmak için desteklenir. Daha fazla bilgi için bu makaleleri okuyun:
> 
> * [Linux'ta yazılım RAID yapılandırma] [ virtual-machines-linux-configure-raid] (için MDADM)
> * [Azure'da Linux sanal makinesi üzerinde LVM'yi yapılandırma][virtual-machines-linux-configure-lvm]
> 
> 

- - -
Azure Premium Depolama'yı genellikle çalışabilir durumdaysanız VM-serisi, yararlanarak dikkat edilmesi gereken noktalar şunlardır:

* SAN/NAS cihazlarına teslim yakın olan g/ç gecikme talepleri.
* Azure Standard Storage sunabilir daha Etkenler daha iyi g/ç gecikme süresi için isteğe bağlı.
* VM başına birden çok standart depolama diski belirli bir VM'ye karşı ne elde edilebilir daha yüksek IOPS yazın.

Temel alınan Azure depolama, her disk için en az üç depolama düğümünden, şeritleme kullanılabilir basit RAID 0 Yedeğiyle. RAID5 veya RAID1 uygulamak için gerek yoktur.

### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Microsoft Azure depolama
Microsoft Azure depolama Blobları ve diskleri veya temel VM (OS ile) en az üç ayrı depolama düğümleri için depolar. Bir depolama hesabı veya yönetilen disk oluşturma olduğunda koruma seçenekleri aşağıda gösterildiği gibi:

![Azure depolama hesabı için etkin coğrafi çoğaltma][dbms-guide-figure-100]

Azure depolama yerel çoğaltma (yerel olarak yedekli) dağıtmak için bazı müşterilerin göze bir altyapı hatası nedeniyle veri kaybına karşı koruma düzeyleri sağlar. Burada gösterildiği gibi dört farklı seçeneklerle beşinci bir ilk üç birini çeşitlemesi yükleniyor. Bunları yakın bakarak biz ayırt edebilirsiniz:

* **Premium yerel olarak yedekli depolama (LRS)**: Azure Premium depolama, g/Ç açısından yoğun iş yüklerini çalıştıran sanal makineler için yüksek performanslı, düşük gecikme süreli disk desteği sunar. Bir Azure bölgesi aynı Azure veri merkezinde verilerin üç kopyasını vardır. Farklı hata ve yükseltme etki alanları kopyalarıdır (kavramları görmek için [bu] [ planning-guide-3.2] bölümde [Planlama Kılavuzu][planning-guide]). Bir depolama düğümü hatası veya disk arızası nedeniyle hizmet dışına çıkan veriler çoğaltma durumunda, yeni bir çoğaltma otomatik olarak oluşturulur.
* **Yerel olarak yedekli depolama (LRS)**: Bu durumda, bir Azure bölgesi aynı Azure veri merkezinde verilerin üç kopyasını vardır. Farklı hata ve yükseltme etki alanları kopyalarıdır (kavramları görmek için [bu] [ planning-guide-3.2] bölümde [Planlama Kılavuzu][planning-guide]). Bir depolama düğümü hatası veya disk arızası nedeniyle hizmet dışına çıkan veriler çoğaltma durumunda, yeni bir çoğaltma otomatik olarak oluşturulur. 
* **Coğrafi olarak yedekli depolama (GRS)**: Bu durumda, ek bir üç kopyaya çoğunda aynı coğrafi bölge (Kuzey Avrupa ve Batı Avrupa gibi durumlarda olan ve başka bir Azure bölgesi, veri akışlarını zaman uyumsuz bir çoğaltma yok ). Böylece altı toplamda bu üç ek yinelemede sonuçlanır. Bu çeşitlemesi, burada coğrafi çoğaltılan Azure bölgesindeki verileri (okuma erişimli coğrafi olarak yedekli) okuma amaçları için kullanılabilir bir ektir.
* **Bölge yedekli depolama (ZRS)**: Bu durumda, aynı Azure bölgesinde verilerin üç kopyasını kalır. İçinde anlatıldığı gibi [bu] [ planning-guide-3.1] bölüm [Planlama Kılavuzu] [ planning-guide] bir Azure bölgesi içinde yakın veri merkezlerinden oluşan bir sayı olabilir. Söz konusu olduğunda LRS çoğaltmaları tek Azure bölgesi olun farklı veri merkezlerinde dağıtılması.

Daha fazla bilgi bulunabilir [burada][storage-redundancy].

> [!NOTE]
> Coğrafi olarak yedekli depolama kullanımını DBMS dağıtımları için önerilmez
> 
> Azure depolama coğrafi çoğaltma, zaman uyumsuz. Tek bir VM'ye bağlı tek tek disklerinin çoğaltma kilit adımda eşitlenmez. Bu nedenle, farklı diskler üzerinde dağıtılmış veya birden çok disklerde yazılım tabanlı RAID yazılım karşı dağıtılan DBMS dosyaları çoğaltmak uygun değil. DBMS yazılım kalıcı bir disk depolama farklı LUN'ları ve temel alınan diskler/iğne arasında tam olarak eşleştirilir gerektirir. Dizisi GÇ Yazma etkinlikleri için çeşitli mekanizmalar DBMS yazılımı kullanıyor ve bunlar bile birkaç milisaniye göre farklılık gösteriyorsa çoğaltma tarafından hedeflenen disk depolama alanı bozuk DBMS raporlar. Esnetilmiş birden çok diskte coğrafi çoğaltmalı veritabanı içeren bir veritabanı yapılandırması isterse, bu nedenle böyle bir çoğaltma veritabanı anlamına gelir ve işlevsellik ile gerçekleştirilmesi gerekir. Bir Azure depolama coğrafi bu işlemi gerçekleştirmek için çoğaltma üzerinde güvenmemelisiniz. 
> 
> Bir örnek sistemiyle açıklamak basit bir sorundur. DBMS, veri dosyalarını içeren sekiz diskleri ve işlem günlüğü dosyası içeren bir diskine sahip Azure'a yüklediğiniz bir SAP sistemine sahip varsayalım. Dokuz bu disklerin her biri olup bunları DBMS göre tutarlı bir yöntem içinde yazılan veriler veri veya işlem günlük dosyaları için yazılan veriler.
> 
> Doğru coğrafi olarak çoğaltmak için verileri sıralama ve bir veritabanı tutarlı görüntü Bakımı içeriği tüm dokuz disk g/ç işlemleri dokuz farklı disklerde yürütülen tam sırada coğrafi olarak çoğaltılmış erişilebilmelidir. Ancak, Azure depolama coğrafi çoğaltma diskleri arasındaki bağımlılıkları bildirmek için izin vermez. Bu Microsoft Azure depolama coğrafi çoğaltma hakkında dokuz farklı disklerin içeriğini birbiriyle ilişkili olduğunu ve yalnızca g/ç işlemleri sırada çoğaltma tamamında gerçekleştiği zaman, veri değişikliklerini tutarlı olduğunu bilmez anlamına gelir. dokuz diskler.
> 
> Coğrafi olarak çoğaltılmış görüntüleri senaryoda tutarlı veritabanı resmi sağlamaz yüksek olan, büyük olasılıkla yanı sıra Ayrıca var. performansı önemli ölçüde etkileyebilir, coğrafi olarak yedekli depolama ile görünür bir performans cezası Özet olarak, bu depolama yedekliliği türü DBMS türü iş yükleri için kullanmayın.
> 
> 

#### <a name="mapping-vhds-into-azure-virtual-machine-service-storage-accounts"></a>Azure sanal makine hizmeti depolama hesaplarına veri VHD'leri eşleme
Bu bölüm, yalnızca Azure depolama hesapları için de geçerlidir. Yönetilen diskleri kullanmayı planlıyorsanız, bu bölümde bahsedilen kısıtlamaları geçerli değildir. Yönetilen diskler hakkında daha fazla bilgi için bölüm okuma [yönetilen diskler] [ dbms-guide-managed-disks] bu kılavuzun.

Bir Azure depolama hesabı yalnızca bir yönetim yapısı, aynı zamanda bir konu sınırlamalar ' dir. Sınırlamalar olup olmadığını biz Azure standart depolama hesabı veya bir Azure Premium depolama hesabı hakkında konuşmak üzerinde değişiklik ise. Tam özelliklerini ve sınırlamalarını listelenen [burada][storage-scalability-targets]

Azure standart depolama için depolama hesabı başına IOPS hakkında bir sınır yoktur unutmayın, bu nedenle (satır içeren **toplam istek oranı** içinde [makaleyi][storage-scalability-targets]). Ayrıca, 100 depolama hesapları (itibariyle, Temmuz 2015) Azure abonelik başına ilk bir sınırı yoktur. Bu nedenle, Azure standart depolama kullanırken birden çok depolama hesapları arasında VM IOPS dengelemek için önerilir. Mümkünse, ideal olarak kullanan bir depolama hesabı tek bir VM ise. Bu nedenle biz Burada, Azure standart depolama alanında barındırılan her VHD kota sınırına ulaşabilir DBMS dağıtımları hakkında konuşmak, Azure standart depolama kullanan Azure depolama hesabı başına 30-40 VHD'ler yalnızca dağıtmanız gerekir. Öte yandan, Azure Premium depolama kullanan ve büyük veritabanı birimleri depolamak istiyorsanız, IOPS'ye göre iyi olabilir. Ancak, Azure Premium depolama hesabı bir Azure standart depolama hesabı veri hacmi daha kısıtlayıcıdır. Sonuç olarak, veri birimi limitini aştıktan önce sınırlı sayıda Azure Premium depolama hesabında VHD yalnızca dağıtabilirsiniz. Sonunda, bir Azure depolama hesabı 'Sanal özellikleri IOPS ve/veya kapasitesi sınırlı olan SAN' olarak düşünebilirsiniz. Sonuç olarak, görev, şirket içi dağıtımlarda, Azure depolama hesapları ve farklı 'sanal SAN cihazları' farklı SAP sistemlerini Vhd'lerinin düzenini tanımlamak için olduğu gibi kalır.

Azure standart depolama için mümkünse tek bir VM için depolama birimi farklı depolama hesaplarından sunmak için önerilmez.

DS veya GS serisi Azure sanal makinelerinin kullanırken, Azure standart depolama hesapları ve Premium depolama hesapları bağlama vhd'lere mümkündür. Yedeklemeleri standart depolama alanına yazma yedeklenen VHD'ler ve DBMS verilere sahip olmak ve Premium depolama günlük dosyalarını nerede gibi heterojen depolama havuzlamanızı aklınıza gelebilir gibi durumlarda kullanın. 

Müşteri dağıtımları ve test yaklaşık 30-40 VHD'ler veritabanı veri dosyaları ve günlük dosyalarını içeren bir tek Azure standart depolama hesabı üzerinde kabul edilebilir performansı sağlanabilir temel. Daha önce bahsedildiği gibi Azure Premium depolama hesabı SORUMLULUĞUN içerebileceği veri kapasitesi ve değil IOPS olması olasıdır.

Olarak SAN cihazları şirket içi, paylaşımı bazı sonunda bir Azure depolama hesabındaki performans sorunlarını algılamak izlenmesini gerektirir. SAP ve Azure portalı için Azure izleme uzantısı olan yetersiz g/ç performansı sunan meşgul Azure depolama hesapları algılamak için kullanılan araçları.  Bu durum algılanırsa, başka bir Azure depolama hesabına meşgul Vm'leri taşımak için önerilir. Başvurmak [Dağıtım Kılavuzu] [ deployment-guide] SAP etkinleştirme konusunda ayrıntıları izleme özellikleri barındırmak için.

Azure standart depolama ve Azure standart depolama hesapları ile ilgili en iyi özetleme başka bir makalede buradan ulaşabilirsiniz <https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx>

#### <a name="f42c6cb5-d563-484d-9667-b07ae51bce29"></a>Yönetilen diskler
Yeni bir kaynak türü Azure kaynak Azure depolama hesaplarında depolanan VHD yerine kullanılabilecek Yöneticisi'nde yönetilen disklerdir. Yönetilen diskler, otomatik olarak bağlandıkları sanal makinenin kullanılabilirlik kümesi ile hizalanan ve bu nedenle, sanal makine ve sanal makine üzerinde çalışan hizmetleri kullanılabilirliğini artırın. Daha fazla bilgi edinmek için [genel bakış makalesi](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview).

Şu anda SAP Premium yönetilen diskler yalnızca destekler. Okuma'lu SAP notuna [1928533] daha fazla ayrıntı için.

#### <a name="moving-deployed-dbms-vms-from-azure-standard-storage-to-azure-premium-storage"></a>Azure Premium depolama için Azure Standard Storage DBMS Vm'lerden dağıtılan taşıma
Size müşteri olarak, Azure standart depolama alanından Azure Premium depolama alanına dağıtılmış bir sanal Makineye taşımak istediğiniz oldukça bazı senaryolar karşılaşırsınız. Diskleriniz Azure depolama hesaplarında depolanır, bu verileri fiziksel olarak taşımadan mümkün değildir. Hedefe ulaşmak için birçok yol vardır:

* Yeni bir Azure Premium depolama hesabına tüm VHD'ler, temel VHD yanı sıra veri VHD'leri kopyalanamadı. Genellikle VHD sayısı değil veri hacmi gerekli olgu nedeniyle Azure standart depolama alanında seçtiniz. Ancak, birçok VHD'ler nedeniyle IOPS gerekli. Azure Premium Depolama'ya taşıma göre yolu ile aynı IOPS aktarım hızı elde etmek için daha az VHD gidilemedi. Azure standart kullanılan veriler ve çok az disk boyutu için ödeme depolama alanında, VHD sayısı maliyetleri açısından önemli değil, olgu verilir. Ancak, Azure Premium depolama sayesinde, çok az disk boyutu ödersiniz. Bu nedenle, müşterilerin çoğu Azure VHD'leri sayıda Premium depolama IOPS performans sağlamak için gereken sayı gerekli tutmaya çalışın. Bu nedenle, müşterilerin çoğu 1:1 basit şekilde karşı kopyalama karar verin.
* Henüz bağlı, bir veritabanı yedeği SAP veritabanınızın içerebilir, tek bir VHD bağlayın. Yedeklemeden sonra yedekleme içeren VHD dahil olmak üzere tüm VHD'ler çıkarın ve temel VHD ve VHD yedekleme ile bir Azure Premium depolama hesabına kopyalayın. Ardından temel VHD tabanlı VM'yi dağıtın ve yedekleme ile VHD'nin. Şimdi veritabanına geri yüklemek için kullanılan sanal makine için ek boş Premium depolama diskleri oluşturun. Başka bir varsayar DBMS yolları için veri ve günlük dosyaları geri yükleme işleminin bir parçası değiştirmenize olanak sağlar.
* Diğer bir olasılık önceki işlemi burada VHD yedekleme Azure Premium Depolama'ya kopyalayın ve yeni dağıtılan ve yüklenen bir VM'ye karşı eklemek, bir çeşididir.
* Buna need veritabanınızın veri dosyalarının sayısını değiştirmek için olduğunda, dördüncü olasılığını seçmelisiniz. Böyle bir durumda, içeri/dışarı aktarma kullanarak bir SAP homojen sistem kopyası gerçekleştirmelisiniz. Put bu dosyaları bir Azure Premium depolama hesabına kopyalanır ve içeri aktarma işlemlerini çalıştırmak için kullandığınız bir VM'ye eklemek bir VHD verin. Müşteriler, çoğunlukla veri dosyalarının sayısını azaltmak istediklerinde bu olasılığı kullanır.

Yönetilen diskleri kullanıyorsanız, Premium depolama ile geçirebilirsiniz:

1. Sanal makineyi serbest bırak
1. Gerekirse, sanal makineyi Premium depolama (örneğin, DS veya GS) destekleyen bir boyuta yeniden boyutlandırın.
1. Premium (SSD) için yönetilen Disk hesap türünü değiştirme
1. Veri diskleri bölümde önerildiği şekilde önbelleğe almayı değiştirme [Vm'leri ve veri diskleri için önbelleğe alma][dbms-guide-2.1]
1. Sanal makineyi Başlat

### <a name="deployment-of-vms-for-sap-in-azure"></a>Azure'da SAP VM dağıtımı
Microsoft Azure Vm'leri ve ilişkili diskler dağıtmak için birden çok yol sunar. Böylece VM'lerin hazırlıkları bağımlı dağıtım yolu değişebilir olduğundan farkları anlamak önemlidir. Genel olarak, aşağıdaki bölümlerde açıklanan senaryolardan içine bakacağız.

#### <a name="deploying-a-vm-from-the-azure-marketplace"></a>Azure Market'ten bir VM dağıtma
Bir Microsoft veya üçüncü taraf Azure Market görüntüsünden sağlanan sanal makinenizin dağıtılacağı istiyorsunuz. Sanal makinenizin azure'da dağıtılan sonra aynı yönergeler ve bir şirket içi ortamda yaptığınız gibi sanal Makinenizin içinde SAP yazılımı yüklemek için Araçlar izleyin. Azure VM, SAP ve Microsoft içindeki SAP yazılımı yüklemek için yüklemeniz önerilir ve diskleri ya da 'gerekli tüm SAP yükleme medyasını içeren bir dosya sunucusu olarak', çalışan bir Azure VM oluşturmak için SAP yükleme medyasını depolayın.

#### <a name="deploying-a-vm-with-a-customer-specific-generalized-image"></a>Müşteriye özgü genelleştirilmiş görüntü ile bir VM dağıtma
İşletim sistemi veya DBMS sürümü ile ilgili belirli gereksinimleri nedeniyle, sağlanan görüntüleri Azure Market'te gereksinimlerinize uygun olmayabilir. Bu nedenle, daha sonra birkaç kez dağıtılabilir kendi 'private' işletim sistemi/DBMS VM görüntüsünü kullanarak bir VM oluşturmanız gerekebilir. Çoğaltma için 'private' bir görüntüsünü hazırlamak için işletim sistemi şirket içi VM genelleştirilmiş olmalıdır. Başvurmak [Dağıtım Kılavuzu] [ deployment-guide] VM'yi genelleştirme hakkında ayrıntılar için.

(Özellikle de 2 katmanlı sistemleri için), şirket içi VM'de SAP içerik zaten yüklediyseniz, örnek üzerinden Azure VM dağıtımını yeniden adlandır sonra yordamı SAP yazılım sağlama Yöneticisi (SAP tarafından desteklenen SAP sistem ayarları uyarlayabilirsiniz Not [1619720]). Aksi takdirde, daha sonra Azure sanal makine dağıtıldıktan sonra SAP yazılım yükleyebilirsiniz.

SAP uygulama tarafından kullanılan veritabanı içerik, ya da içerik yeni bir SAP yüklemesi tarafından oluşturabilirsiniz veya içeriğinizi DBMS veritabanı yedeklemesiyle VHD kullanarak veya içine doğrudan yedekleme, DBMS özelliklerinden yararlanarak Azure'a alabilirsiniz  Microsoft Azure depolama. Bu durumda, ayrıca VHD'ler DBMS veri ve günlük dosyaları ile şirket içi hazırlamanız ve ardından bu diskleri Azure'da içeri. Ancak, şirket içinden Azure'a yüklendiğinden DBMS veri aktarımını şirket içi hazırlıklı olmak için gereken VHD diskler üzerinde çalışır.

#### <a name="moving-a-vm-from-on-premises-to-azure-with-a-non-generalized-disk"></a>Bir sanal makine şirket içinden Azure'a genelleştirilmiş olmayan bir diskle taşıma
Şirket içinden belirli bir SAP sistemine (lift and shift ile) Azure'a taşımak planlayın. Bu, veri ve günlük dosyalarını azure'a DBMS ile işletim sistemi, SAP ikili dosyaları ve nihai DBMS ikili dosyaları içeren diskin yanı sıra diskleri karşıya yükleyerek yapılabilir. Şirket içi ortamda yapılandırıldıkları gibi yukarıdaki Senaryo #2'ters yönde, ana bilgisayar adı, SID SAP ve SAP kullanıcı hesapları Azure VM'nin bulundurun. Bu nedenle, görüntüyü Genelleştirme gerekli değildir. Bu durumda, çoğunlukla SAP ortamının bir kısmını şirket içinde ve bölümleri Azure üzerinde çalıştırıldığı içi ve dışı karışık senaryo için geçerlidir.

## <a name="871dfc27-e509-4222-9370-ab1de77021c3"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma ile Azure Vm'leri
Azure, biz SAP ve DBMS dağıtımları için kullanacağınız farklı bileşenleri için uygulanan aşağıdaki yüksek kullanılabilirlik (HA) ve olağanüstü durum kurtarma (DR) işlevlerini, sunar.

### <a name="vms-deployed-on-azure-nodes"></a>Azure düğümlerine dağıtılan VM'ler
Azure platformu, dağıtılan VM'ler için dinamik geçiş gibi özellikler sunmaz. Bu, varsa bakım gerekli bir VM dağıtıldığı bir sunucu kümesinde VM durdurulup yeniden gerek duyduğu anlamına gelir. Azure'da bakım kullanarak, bu nedenle sunucu kümeleri içinde yükseltme etki alanları adlı gerçekleştirilir. Yalnızca bir yükseltme etki alanı aynı anda sağlanıyor. Böyle bir yeniden başlatma sırasında yoktur hizmet VM'yi kapatın, Bakım ve yeniden başlatılır. Çoğu DBMS satıcılar, ancak birincil düğüm kullanılamıyorsa, başka bir düğümde DBMS Hizmetleri hızlı bir şekilde yeniden yüksek kullanılabilirlik ve olağanüstü durum kurtarma işlevler sunar. Azure platformu sanal makineleri, depolama ve diğer Azure Hizmetleri, yükseltme planlı bakım ya da altyapı hataları Vm'leriniz veya hizmetlerinizi küçük bir kısmı yalnızca erişememeleri emin olmak için etki alanları arasında dağıtmak için işlevsellik sunar.  Dikkatli planlama ile şirket içi altyapıyla karşılaştırılabilir kullanılabilirlik seviyelerine ulaşmasını sağlamak mümkündür.

Microsoft Azure kullanılabilirlik kümeleri, VM'lerin mantıksal gruplandırması olan veya yalnızca şekilde bir düğümün kapanması herhangi bir noktada ( OkumazamanındaVm'lerisağlar,hizmetlerivediğerhizmetlerifarklıhataveyükseltmeetkialanlarıiçinbirkümeiçindedağıtılır[bu (Linux)] [ virtual-machines-manage-availability-linux] veya [bu (Windows)] [ virtual-machines-manage-availability-windows] makale daha fazla ayrıntı için).

Burada görüldüğü gibi VM'lerin sıralı olduğunda amacına göre yapılandırılması gerekir:

![DBMS yüksek kullanılabilirlik yapılandırmaları için kullanılabilirlik kümesi tanımı][dbms-guide-figure-200]

DBMS dağıtım yüksek oranda kullanılabilir yapılandırmalara (kullanılan tek DBMS HA işlevler bağımsız) oluşturmak istiyoruz, DBMS Vm'leri gerekir:

* Vm'leri aynı Azure sanal ağa ekleyin (<https://azure.microsoft.com/documentation/services/virtual-network/>)
* HA yapılandırmasının Vm'leri de aynı alt ağda olmalıdır. Farklı alt ağlar arasında ad çözümlemesine salt bulut dağıtımında, yalnızca IP çözümleme works mümkün değildir. Siteden siteye veya ExpressRoute bağlantısı kullanarak şirket içi dağıtımlar için en az bir alt ağla zaten oluşturulur. Ad çözümlemesi, şirket içi göre yapılır AD ilkeleri ve ağ altyapısı. 



#### <a name="ip-addresses"></a>IP Adresleri
VM'ler yüksek kullanılabilirlik yapılandırmaları için esnek bir şekilde ayarlamak için önerilir. IP adresleri HA yapılandırmasındaki HA ortaklarınızın yönelik olarak bağlı, statik IP adresi kullanılmadığı sürece Azure'da güvenilir değil. Azure'da iki "Kapat" kavram vardır:

* Azure portal veya Azure PowerShell cmdlet'ini Stop-AzureRmVM kapatma: Bu durumda, sanal makine kapatma alır ve edilemez. Kullanılan depolama alanı için ücret tek Ücret olacak şekilde Azure hesabınız artık bu VM için ücretlendirilir. Ancak, ağ arabiriminin özel IP adresi statik değil, IP adresi serbest ve bu ağ arabiriminin VM yeniden sonra yeniden atanmış eski IP adresini alır garanti edilmez. Azure portalı üzerinden ya da otomatik olarak Stop-AzureRmVM çağırarak aşağı kapatma gerçekleştirmek ayırmayı kaldırma neden olur. Makine kullanımı Stop-AzureRmVM - StayProvisioned serbest bırakmak istemiyorsanız 
* Bir işletim sistemi düzeyi VM'den kapatırsanız, VM kapatma ve yok edilemez. Ancak, bu durumda, Azure hesabınızı yine de kapatma olduğunu rağmen VM için ücretlendirilir. Böyle bir durumda, durdurulan bir VM için IP adresi atamasının değişmeden kalır. VM içinde kapatma otomatik olarak ayırmayı kaldırma zorlamaz.

Şirket içi senaryolar için bile varsayılan olarak bir kapatma ve ayırmayı kaldırma anlamına gelir IP adreslerinin yinelenenleri atama VM'den DHCP ayarları şirket ilkeleri farklı olsa bile. 

* Özel bir ağ arabirimi için bir statik IP adresi atar, açıklanan [burada][virtual-networks-reserved-private-ip].
* Böyle bir durumda ağ arabirimi silinmez sürece IP adresinin sabit kalır.

> [!IMPORTANT]
> Tüm dağıtım basit ve yönetilebilir tutmak için açık bir DBMS HA veya DR yapılandırması Azure içinde ilgili farklı VM'ler arasında işlevsel bir ad çözümlemesi olan bir şekilde İşbirliği yapmaya Vm'leri kurulum önerilir.
> 
> 

## <a name="deployment-of-host-monitoring"></a>İzleme ana bilgisayarı dağıtımı
Azure sanal Makineler'de SAP uygulamalarını verimli kullanım için SAP, Azure sanal makineleri çalıştıran fiziksel ana bilgisayarlardan verilerin izleme ana bilgisayarı alma olanağı gerektirir. Belirli bir SAP konak Aracısı düzeltme eki düzeyi gereklidir, SAPOSCOL ve SAP konak Aracısı bu yeteneği sağlar. Tam düzeltme eki düzeyi SAP Not belgelenen [1409604].

Ana verileri SAPOSCOL ve SAP konak Aracısı teslim bileşenlerin dağıtımına ilişkin ve bu bileşenlerden birini yaşam döngüsü yönetimi ile ilgili ayrıntılar için başvurmak [Dağıtım Kılavuzu][deployment-guide]

## <a name="3264829e-075e-4d25-966e-a49dad878737"></a>Microsoft SQL Server özellikleri
### <a name="sql-server-iaas"></a>SQL Server IaaS
Microsoft Azure ile başlayarak, Windows Server platformunda Azure sanal makineler için yerleşik mevcut SQL Server uygulamalarınızı kolayca geçirebilirsiniz. Bir sanal Makine'deki SQL Server kolayca bu uygulamalar Microsoft azure'a geçirerek toplam sahip olma maliyetini dağıtım, yönetim ve Bakım Kurumsal avantajlarına uygulama azaltmanızı sağlar. Bir Azure sanal Makine'de SQL Server ile yöneticiler ve geliştiricilerin aynı geliştirme ve şirket içi kullanılabilir olan yönetim araçlarını kullanmaya devam edebilirsiniz. 

> [!IMPORTANT]
> Biz Microsoft Azure SQL veritabanı, Microsoft Azure platformu bir hizmet teklifinin bir Platform olan görüştükten değil. Azure sanal bir hizmet Özelliği Azure altyapısından yararlanarak Makineler'de, şirket içi dağıtımlar için bilinen SQL Server ürün çalıştıran bu yazıdaki bir tartışma bulunmaktadır. Veritabanı özellikleri ve işlevleri bu iki teklifler arasında farklıdır ve birbiriyle karışmasını değil. Ayrıca bkz: <https://azure.microsoft.com/services/sql-database/>
> 
> 

Gözden geçirmek için önerilen [bu] [ virtual-machines-sql-server-infrastructure-services] devam etmeden önce belgeleri.

Aşağıdaki bölümlerde yukarıdaki bağlantıyı altında belge parçalarını parçalarını toplanır ve belirtilen. SAP ilgili detaylar de belirtilmiştir ve bazı kavramları daha ayrıntılı olarak açıklanmıştır. Ancak, SQL Server'a özgü belgelere okuma önce ilgili belgelerde ilk üzerinde çalışmak için önerilir.

Iaas belirli bilgiler devam etmeden önce bilmeniz gereken bazı SQL Server vardır:

* **Sanal makine SLA'sı**: Burada bulunabilir Azure'da çalışan sanal makineler için SLA yoktur: <https://azure.microsoft.com/support/legal/sla/>  
* **SQL sürüm desteği**: SAP müşteriler, SQL Server 2008 R2 destekliyoruz ve Microsoft Azure sanal makinesi üzerinde daha yüksek. Önceki sürümlerinde desteklenmez. Bu genel gözden [destek bildirimiyle](https://support.microsoft.com/kb/956893) daha fazla ayrıntı için. Genel SQL Server 2008 Microsoft tarafından de desteklendiğini unutmayın. Ancak, önemli işlevleri için SQL Server 2008 R2 ile birlikte sunulan, SAP, SQL Server 2008 R2 SAP için en düşük sürüm kaynaklanır. Iaas senaryosunda (örneğin, doğrudan Azure depolama karşı yedekleme) daha derin tümleştirme ile SQL Server 2012 ve 2014 genişletilmiş aklınızda bulundurun. Bu nedenle, ki bu yazıda SQL Server 2012 ve 2014'te, en son düzeltme eki düzeyi için Azure kısıtlayın.
* **SQL özellik desteği**: en SQL Server özelliklerini, bazı özel durumlar ile Microsoft Azure sanal makinelerinde desteklenir. **Paylaşılan diskleri kullanarak SQL Server Yük Devretme Kümelemesi desteklenmiyor**.  Veritabanı yansıtma, AlwaysOn Kullanılabilirlik grupları, çoğaltma, günlük aktarma ve hizmet Aracısı tek bir Azure bölgesinde desteklenmektedir gibi teknolojileri dağıtılmış. SQL Server AlwaysOn ayrıca desteklenen farklı Azure bölgeleri arasında burada açıklandığı gibi: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.  Gözden geçirme [destek bildirimiyle](https://support.microsoft.com/kb/956893) daha fazla ayrıntı için. AlwaysOn yapılandırmasını dağıtma hakkında bir örnek gösterilmiştir [bu] [ virtual-machines-workload-template-sql-alwayson] makalesi. Ayrıca, en iyi belgelenen adımları denetleyin [burada][virtual-machines-sql-server-infrastructure-services] 
* **SQL performansı**: Microsoft Azure barındırılan sanal makineleri gerçekleştirmek çok iyi diğer genel bulut sanallaştırma teklifleri ancak tek tek sonuç kolaylığına değişebilir başarılara duyuyoruz. Kullanıma [bu] [ virtual-machines-sql-server-performance-best-practices] makalesi.
* **Azure marketi'ndeki görüntüleri**: yeni bir Microsoft Azure sanal makine dağıtmak için en hızlı yolu Azure Marketi'nden bir görüntü kullanmaktır. SQL Server içeren görüntülerini Azure Market'te vardır. SQL Server önceden yüklendiği görüntüleri için SAP NetWeaver uygulamalarını hemen kullanılamaz. Varsayılan SQL Server harmanlaması bu görüntülerin ve SAP NetWeaver sistemleri tarafından gerekli harmanlama yüklü nedenidir. Bu görüntüleri kullanmak için bölümde belgelenen adımları denetleyin [Microsoft Azure Marketi dışında bir SQL sunucu görüntüsü kullanarak][dbms-guide-5.6]. 
* Kullanıma [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/) daha fazla bilgi için. [SQL Server 2012 lisanslama Kılavuzu](https://download.microsoft.com/download/7/3/C/73CAD4E0-D0B5-4BE5-AB49-D5B886A5AE00/SQL_Server_2012_Licensing_Reference_Guide.pdf) ve [SQL Server 2014 lisanslama Kılavuzu](https://download.microsoft.com/download/B/4/E/B4E604D9-9D38-4BBA-A927-56E4C872E41C/SQL_Server_2014_Licensing_Guide.pdf) de önemli bir kaynaktır.

### <a name="sql-server-configuration-guidelines-for-sap-related-sql-server-installations-in-azure-vms"></a>Azure sanal makinelerinde SAP ile ilgili SQL Server yüklemeleri için SQL Server yapılandırma yönergeleri
#### <a name="recommendations-on-vmvhd-structure-for-sap-related-sql-server-deployments"></a>VM/VHD yapısına'SAP ile ilgili SQL Server dağıtımları için öneriler
Genel bir açıklama uygun olarak, SQL Server yürütülebilir dosyalar bulunan veya yüklü sanal makinenin işletim sistemi diski sistem sürücüsüne (C: sürücüsü\).  Genellikle, SQL Server sistem veritabanlarının en yüksek düzeyde SAP NetWeaver iş yükü tarafından kullanılmaz. Bu nedenle (master, msdb ve model) SQL Server'ın sistem veritabanları da C:\ sürücüde kalır. Bir özel durum, bazı SAP ERP ve tüm BW iş yükleri söz konusu olduğunda, daha yüksek veri hacmine veya özgün VM'ye sığamıyorsa g/ç işlemleri toplu gerektirebilir, TempDB'de olabilir. Bu tür sistemler için aşağıdaki adımlar gerçekleştirilmelidir:

* Birincil tempdb veri dosyası, SAP veritabanının birincil veri dosyası olarak aynı mantıksal sürücüye taşıyın.
* Ek tempdb veri dosyalarının her bir veri dosyası SAP kullanıcı veritabanının içeren diğer bir mantıksal sürücüler ekleyin.
* Kullanıcı veritabanı günlük dosyası içeren bir mantıksal sürücü tempdb günlük dosyası ekleyin.
* **Yalnızca yerel SSD'ler VM türleri için** işlem düğümü tempdb verilerini ve günlük dosyaları D:\ sürücüsüne yerleştirilmiş olabilir. Bununla birlikte, birden fazla tempdb veri dosyası kullanmayı tavsiye. VM türüne göre D:\ sürücüsüne birimlerin farklı olduğunu unutmayın.

Bu yapılandırmalar, sistem sürücüsünün sağlamak mümkün olandan daha fazla alan kullanmak tempdb etkinleştirin. Uygun tempdb boyutunu belirlemek için bir şirket içi mevcut sistemler, tempdb boyutları kontrol edebilirsiniz. Ayrıca, bu tür bir yapılandırma sistem sürücüsü ile sağlanan tempdb karşı IOPS sayılarında etkinleştirebilirsiniz. Yeniden şirket içinde çalışan sistemler, böylece, tempdb üzerinde görmeyi beklediğiniz IOPS sayılarında türetebilirsiniz tempdb karşı g/ç iş yükünü izlemek için kullanılabilir.

Tempdb verilerini ve tempdb logfile D:\ sürücüsüne yerleştirildiği ve bir SAP veritabanı ile SQL Server çalıştıran bir VM yapılandırması gibi görünür:

![SAP için Azure Iaas VM başvuru yapılandırması][dbms-guide-figure-300]

D:\ sürücüsüne farklı boyutta bir VM türüne bağımlı olduğunu unutmayın. Tempdb boyut gereksinimini bağımlı, D:\ sürücüsüne çok küçük olduğu durumlarda SAP veritabanı veri ve günlük dosyaları ile çifti tempdb veri ve günlük dosyaları için zorlanabilirsiniz.

#### <a name="formatting-the-disks"></a>Diskleri biçimlendirme
SQL Server için NTFS blok boyutu için SQL Server verilerini içeren diskler ve günlük dosyalarını 64 K olmalıdır. D:\ sürücüsüne biçimlendirmek için gerek yoktur. Bu sürücü önceden biçimlendirilmiş gelir.

Geri yükleme ya da veritabanı oluşturulmasını veri dosyalarını dosyaların içeriğini sıfırlama tarafından başlatılıyor değil emin emin olmak için bir SQL Server hizmetini çalıştıran kullanıcı bağlamı belirli bir izni olduğundan emin olmanız gerekir. Genellikle Windows Yönetici grubundaki kullanıcılar bu izinlere sahip olursunuz. SQL Server hizmetini Windows yönetici olmayan kullanıcı kullanıcı bağlamında çalışırsa, kullanıcı hakkı bu kullanıcı atamanız gerekir **Birim bakım görevleri gerçekleştir**.  Bu Microsoft Bilgi Bankası makalesi ayrıntılara bakın: <https://support.microsoft.com/kb/2574695>

#### <a name="impact-of-database-compression"></a>Veritabanı sıkıştırması etkisini
Burada, g/ç bant genişliği sınırlayıcı bir etmen haline gelebilir yapılandırmalarında, IOPS azaltır her ölçü bir Azure gibi bir Iaas senaryosu çalıştırabilirsiniz iş yükü esnetme artırmaya yardımcı olabilir. Bu nedenle, henüz yapıldığında, SQL Server sayfa sıkıştırmayı uygulama SAP ve Microsoft tarafından mevcut SAP veritabanını Azure'a yüklemeden önerilir.

Azure'a yüklemeden önce veritabanı sıkıştırma gerçekleştirmek için öneri dışında iki nedenleri verilmiştir:

* Karşıya yüklenecek veri miktarı düşüktür.
* Daha fazla CPU veya daha yüksek g/ç bant genişliği veya g/ç gecikme şirket içi daha az daha güçlü donanım kullanabilirsiniz varsayarak sıkıştırma yürütme süresi kısadır.
* Küçük veritabanı boyutları için disk ayırmayı daha az maliyetlerine neden olabilir

Şirket içi yaptığı gibi veritabanı sıkıştırma bir Azure sanal Makineler'de de çalışır. Mevcut SAP SQL Server veritabanını sıkıştırma konusunda daha fazla ayrıntı için buraya bakın: <https://blogs.msdn.com/b/saponsqlserver/archive/2010/10/08/compressing-an-sap-database-using-report-msscompress.aspx>

### <a name="sql-server-2014---storing-database-files-directly-on-azure-blob-storage"></a>SQL Server 2014 - Depolama veritabanı dosyalarını doğrudan Azure Blob Depolama
SQL Server 2014 veritabanı dosyalarını doğrudan Azure Blob Store VHD etrafında 'sarmalayıcı' olmadan depolamak için olasılığını açılır. Standart Azure depolama veya daha küçük VM türleri kullanmaya özellikle bu sınırlı sayıda bazı küçük bir VM türleri bağlı diskler tarafından uygulanabiliyordu IOPS sınırları nerede üstesinden gelebilir senaryolarını olanaklı kılar. Bu, ancak SQL Server'ın sistem veritabanları için kullanıcı veritabanları için çalışır. Ayrıca, SQL Server'ın veri ve günlük dosyaları için çalışır. SAP SQL Server veritabanı dağıtmak istiyorsanız bu şekilde 'sarmalama' yerine VHD'ler, içine tutmak aşağıdakileri göz önünde:

* Depolama hesabı gereksinimlerini VM SQL Server'ı dağıtmak için kullanılan bir çalıştırıyorsa aynı Azure bölgesinde olacak şekilde kullanılır.
* VHD dağıtım ile ilgili farklı Azure depolama hesapları daha önce listelenen konular bu dağıtımlar da yöntem için geçerlidir. Azure depolama hesabı sınırları karşı g/ç işlemlerinin sayısı anlamına gelir.

[comment]: <> (MSSedusch TODO ancak değil, bunu ağ bant genişliği ve depolama bant genişliği değil, kullanacaksınız?)

Bu dağıtım türü hakkındaki ayrıntıları aşağıda listelenmiştir: <https://docs.microsoft.com/sql/relational-databases/databases/sql-server-data-files-in-microsoft-azure>

Azure Premium depolama üzerinde doğrudan SQL Server veri dosyaları depolamak için burada belgelenmektedir en az bir SQL Server 2014 düzeltme eki sürümü gerekir: <https://support.microsoft.com/kb/3063054>. Azure standart depolama üzerinde SQL Server veri dosyaları depolamak, yayımlanan sürümü SQL Server 2014 ile çalışır. Ancak, çok aynı düzeltme eklerini başka bir dizi SQL Server veri dosyaları ve yedekler için doğrudan Azure Blob Depolama kullanımını daha güvenilir hale düzeltmeleri içerir. Bu nedenle bu düzeltme ekleri genel kullanmanızı öneririz.

### <a name="sql-server-2014-buffer-pool-extension"></a>SQL Server 2014 arabellek havuzu genişletme
SQL Server 2014 arabellek havuzu uzantısı adlı yeni bir özelliği kullanıma sunuldu. Arabellek havuzu sunucusunun yerel SSD veya VM tarafından desteklenen ikinci bir düzey önbelleği ile bellekte tutulur SQL Server'ın bu işlevselliği genişletir. Bu veri büyük çalışma kümesi 'bellekte' tutmak için sağlar. Azure standart depolama erişimi karşılaştırıldığında Azure VM'deki yerel SSD'ler üzerinde depolanan arabellek havuzu uzantısı Access'e faktörlerden daha hızlıdır.  Bu nedenle, yerel D:\ sürücüsüne mükemmel IOPS ve aktarım hızı olan VM türleri yararlanarak Azure Storage'a karşı IOPS yükü azaltmak ve sorgu yanıt sürelerini önemli ölçüde geliştirmek için çok makul bir şekilde olabilir. Bu, özellikle Premium depolama kullanmadığınız durumlarda geçerlidir. Premium depolama ve işlem düğümünde Premium Azure okuma önbelleği kullanımı olması durumunda, veri dosyaları için önerildiği gibi önemli fark bekleniyor. Her iki önbellekler (SQL Server arabellek havuzu genişletme ve Premium depolama okuma önbelleği) işlem düğümlerinin yerel diskler kullanıyorsanız nedenidir.
Bu işlev hakkında daha fazla ayrıntı için bu belgelere bakın: <https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension> 

### <a name="backuprecovery-considerations-for-sql-server"></a>SQL Server için yedekleme/kurtarma konuları
SQL Server Azure'a dağıtırken yedekleme yönteminize gözden geçirilmesi gerekir. Sistem, üretken bir sistem değil olsa bile, SQL Server tarafından barındırılan SAP veritabanı düzenli aralıklarla yedeklenmelidir. Azure depolama üç görüntü tutar olduğundan, bir yedekleme şimdi daha az depolama kilitlenme telafi için açısından önemlidir. Uygun bir yedekleme ve kurtarma planı sürdürmek için öncelik daha belirli bir noktadaki zaman kurtarma özellikleri sağlayarak mantıksal/el ile hataları dengeleyebilir nedenidir. Bu nedenle yedeklemelerini geri belirli bir noktaya veritabanını geri veya var olan veritabanı kopyalayarak başka bir sistem kaynağını oluşturmak için Azure'da yedeklemelerini kullanın ya da kullanın olmaktır. Örneğin, 2 katmanlı SAP yapılandırmasından aynı sistem bir 3 katmanlı sistem ayarı için bir yedekleme geri yükleyerek aktardığınız.

SQL sunucusunu yedeklemek için Azure depolama için üç farklı yolu vardır:

1. SQL Server 2012 CU4 ve daha yüksek veritabanlarını yedeklemek için bir URL yerel olarak yedekleyebilirsiniz. Bu blogdaki ayrıntılı [SQL Server 2014 - bölüm 5 - yedekleme/geri yükleme iyileştirmeleri yeni işlevler](https://blogs.msdn.com/b/saponsqlserver/archive/2014/02/15/new-functionality-in-sql-server-2014-part-5-backup-restore-enhancements.aspx). Bölüm bakın [SQL Server 2012 SP1 CU4 ve daha sonra][dbms-guide-5.5.1].
2. SQL 2012 CU4 önce SQL Server sürümleri, bir VHD için yedekleme ve temel yazma akış yapılandırılmış bir Azure depolama konumuna taşımak için bir yeniden yönlendirme işlevini kullanabilirsiniz. Bölüm bakın [SQL Server 2012 SP1 CU3'ten ve önceki sürümleri][dbms-guide-5.5.2].
3. Son bir disk cihaza disk komutu için geleneksel SQL Server Yedekleme yöntemidir. Bu, şirket içi dağıtım desen aynıdır ve ayrıntılı bu belgede ele alınmayan.

#### <a name="0fef0e79-d3fe-4ae2-85af-73666a6f7268"></a>SQL Server 2012 SP1 CU4 ve üzeri
Bu işlev, Azure BLOB depolama alanına doğrudan yedekleme sağlar. Bu yöntem disk ve IOPS kapasitesi kullanılmasına neden olur diğer disklere yedeklemeniz gerekir. Temel fikir şu::

 ![Microsoft Azure depolama BLOBU için SQL Server 2012 yedeklemeyi kullanma][dbms-guide-figure-400]

Bu durumda bir SQL Server yedeklemeleri depolamak için diskleri harcayabileceğiniz gerektirmeyeceği avantajlarındandır. Bu nedenle ayrılan daha az sayıda disk ve disk IOPS, veri ve günlük dosyaları için kullanılabilir tüm bant gerekir. Bölümünde açıklandığı gibi yedekleme en büyük boyutu en fazla 1 TB sınırlı olduğunu unutmayın **sınırlamaları** bu makaledeki: <https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url#limitations>. SQL Server Yedekleme sıkıştırma kullanmayı rağmen yedekleme boyutu 1 TB boyutunda aşılmasına, bölümde açıklanan işlevselliğini [SQL Server 2012 SP1 CU3'ten ve önceki sürümleri] [ dbms-guide-5.5.2] bu belgede olması gerekir kullanılır.

[İlgili belgelere](https://docs.microsoft.com/sql/relational-databases/backup-restore/restoring-from-backups-stored-in-microsoft-azure) veritabanlarının Azure Blob Store karşı yedeklerden geri yükleme açıklayan önerilir yedeklemenizse doğrudan Azure BLOB depolama alanından geri yüklememeyi > 25 GB. Bu makalede öneri performansla ilgili önemli noktalar ve işlevsel kısıtlamaları nedeniyle değil temel alır. Bu nedenle, tek olay temelinde farklı koşullar geçerli olabilir.

Bu tür bir yedekleme ayarlamak ve diğer yararlanılarak hakkında belgeler bulunabilir [bu](https://docs.microsoft.com/sql/relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016) Öğreticisi

Bir dizi adımdan örneği okunabilir [burada](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url).

Yedeklemeleri otomatikleştirme, bu her yedekleme için BLOB'ları farklı şekilde adlandırılmış emin olmak için en yüksek önem derecesi olur. Aksi takdirde üzerine yazılır ve restore zinciri kopmuş.

Üç farklı yedekleme türleri arasında şeyler'kurmak karıştırmamaya sırayla yedeklemeler için kullanılan depolama hesabı altında farklı kapsayıcılar oluşturmak için önerilir. Kapsayıcıları VM tarafından yalnızca veya VM ile yedekleme türü olabilir. Şema gibi görünebilir:

 ![Microsoft Azure depolama BLOBU - SQL Server 2012 yedekleme ayrı bir depolama hesabı altında farklı kapsayıcılar kullanma][dbms-guide-figure-500]

Yukarıdaki örnekte, yedeklemeleri VM'ler dağıtıldığı aynı depolama hesabına gerçekleştirilen değil. Özellikle yedekleri için yeni bir depolama hesabı olacaktır. Depolama hesapları içinde oluşturulan bir matris yedekleme ve VM adını yazın, farklı kapsayıcılar olacaktır. Bu tür bir kesimleme farklı VM yedekleri Yönet kolaylaştırır.

BLOB'ları yedeklemeler için bir doğrudan Yazar Koleksiyonlar'a eklemediğinizden veri sayısı için bir sanal Makinenin disklerini. Bu nedenle bir durum, veriler için belirli VM SKU'ların bağlı veri diskleri en fazla en üst düzeye ve işlem günlük dosyası ve bir depolama kapsayıcısı karşı yedek hala yürütün. 

#### <a name="f9071eff-9d72-4f47-9da4-1852d782087b"></a>SQL Server 2012 SP1 CU3'ten ve önceki sürümleri
Bağlantılı MSI indirmek için doğrudan Azure Storage'a karşı yedek elde etmek için gerçekleştirmeniz gereken ilk adım olacaktır [bu](https://www.microsoft.com/download/details.aspx?id=40740) KBA makalesi.

İndirme x64 yükleme dosyasını ve belgeler. Dosya adında bir programı yükler: **Microsoft SQL Server yedeklemesini Microsoft Azure aracına**. Kapsamlı ürün belgelerini okuyun.  Araç, temel olarak şu şekilde çalışır:

* SQL Server tarafında bir disk konumu SQL sunucusu yedeklemesi için tanımlanır (D:\ sürücüsüne konumu olarak kullanmayın).
* Aracı, farklı Azure depolama kapsayıcıları yedeklemeler farklı türde yönlendirmek için kullanılan kuralları tanımlamanıza olanak sağlar.
* Kuralları yerinde olduktan sonra aracı yazma akış yedekleme VHD'leri/diskleri daha önce tanımlanmış olan Azure depolama konumuna birine yönlendirir.
* Araç birkaç KB boyutlu bir küçük saplama dosya diskte VHD/SQL Server için tanımlanmış olan yedekleme bırakır. **Azure Depolama'dan yeniden geri yüklemek için gerekli olduğundan, bu dosya depolama konumunda bırakılmalıdır.**
  * (Örneğin aracılığıyla saplama dosyasında yer alan depolama medyası kaybı) saplama dosyasını kaybettiğiniz ve bir Microsoft Azure depolama hesabına yedekleme seçeneği seçtiniz, Microsoft Azure depolama aracılığıyla saplama dosyası karşıdan yükleyerek kurtarabilir Bunu yerleştirildiği depolama kapsayıcısından. Aracı algılamak ve şifreleme ile özgün kuralı kullanılırsa aynı şifreleme parolasıyla aynı kapsayıcıya karşıya yüklemek için yapılandırıldığı bir klasör yerel makinede saplama dosyası yerleştirin. 

Başka bir deyişle, SQL Server'ın daha yeni sürümleri için yukarıda açıklandığı gibi şema doğrudan adresi bir Azure depolama konumuna izin vermiyor, SQL Server sürümleri için de yere koyabilirsiniz.

Bu yöntem kullanılmamalıdır yedekleme yerel olarak Azure depolama karşı desteklemek daha yeni SQL Server sürümleri ile. Yerel yedekleme sınırlamaları azure'a azure'a yedekleme yerel yürütme burada engelliyor durumlardır.

#### <a name="other-possibilities-to-back-up-sql-server-databases"></a>SQL Server veritabanlarını yedeklemek için diğer olasılıklar
Veritabanlarını yedeklemek için diğer olanaklar var. yedeklemeleri depolamak için kullandığınız bir VM için ek veri diskleri eklemek için Böyle bir durumda, diskleri tam çalışmayan emin olmanız gerekir. Bu durumda, diskleri çıkarma speak kadar çok 'Arşivle' ve yeni bir boş disk ile değiştirmek gerekir. Bu yolu giderseniz, bunlar VHD'lerin ayrı Azure depolama hesaplarında olanlardan tutmak istediğiniz, VHD'leri ile veritabanı dosyaları.

Bağlı, çok sayıda disk içeren büyük bir VM örneğin D14 32VHDs ile kullanma ikinci bir olasılıktır. Ardından farklı DBMS sunucuları için yedekleme hedefi kullanılan paylaşımları burada oluşturabilirsiniz esnek bir ortam oluşturmak için depolama alanları kullanın.

Bazı en iyi belgelenmiş [burada](https://blogs.msdn.com/b/sqlcat/archive/2015/02/26/large-sql-server-database-backup-on-an-azure-vm-and-archiving.aspx) de. 

#### <a name="performance-considerations-for-backupsrestores"></a>Yedekleme/geri yükleme işlemleri için performans konuları
Çıplak metal dağıtımlarındaki yedekleme/geri yükleme performansı, paralel olarak kaç birimleri okunabilir ve ne bu birimlerin verimi olabilir bağlıdır. Yedekleme sıkıştırma tarafından kullanılan CPU tüketimini Vm'lerde ile en çok sekiz adet CPU iş parçacıklarını Ayrıca, önemli bir rol oynayabilir. Bu nedenle, kabul edilebilir:

* Daha az veri depolamak için kullanılan disk sayısı, dosyaları, daha küçük hizmetin genel performansını okuma.
* Daha küçük CPU sayısını, yedekleme sıkıştırma etkisini VM, daha ağır iş parçacıklarını yönlendirebilir.
* Daha az sayıda hedefle (BLOB'lar, VHD'leri veya diskleri) yedekleme için daha düşük aktarım hızı yazmak için.
* Küçük VM boyutu, yazma ve Azure depolama alanından okunmasını daha küçük depolama aktarım hızı kotası. Bağımsız olarak mı yedekleri Azure Blob üzerinde doğrudan depolanır veya yeniden Azure Bloblarında depolanan VHD mi depolanır.

Bir Microsoft Azure depolama BLOBU, daha yeni sürümlerde yedekleme hedefi olarak kullanırken, belirli her yedekleme için yalnızca bir URL hedefi belirlenmesi için kısıtlanır.

Ancak, "Microsoft SQL Server Yedekleme" Microsoft Azure aracına eski sürümlerde kullanırken, birden fazla dosya hedef tanımlayabilirsiniz. Birden fazla hedef yedek ölçeklendirebilirsiniz ve aktarım hızı yedekleme daha yüksektir. Bu, ardından Azure depolama hesabındaki de birden çok dosyalarında sonuçlanır. Testinizde birden çok dosya hedefini kullanarak, kesinlikle, üzerinde SQL Server 2012 SP1 CU4 uygulanan yedekleme uzantılara sahip olabileceğimizden aktarım hızı elde edebilirsiniz. Ayrıca yerel yedekleme olduğu gibi 1 TB sınırını tarafından Azure'a engellenmez.

Ancak, göz önünde bulundurun, aktarım hızı da yedekleme için kullandığınız Azure depolama hesabı konumu bağlıdır. Vm'leri çalıştıran daha farklı bir bölgede depolama hesabı bulunacak bir fikir olabilir. Örneğin, Batı Avrupa'da VM Yapılandırması'nı çalıştırın, ancak yerleştirme için kullandığınız depolama hesabını Kuzey Avrupa'da karşı yedekleme. Kesinlikle yedekleme aktarım hızı üzerinde bir etkisi yoktur ve hedef depolama ve VM'lerin aynı bölgesel veri merkezi içinde çalıştığı durumlarda mümkün göründüğü 150 MB/sn verimini üreteceği değil.

#### <a name="managing-backup-blobs"></a>Yedekleme BLOB'ları yönetme
Kendi yedeklemelerini yönetme gereksinimi yoktur. Beklentisi birçok bloblar sık işlem günlüğü yedeklemeleri yürüterek oluşturulan olduğundan, bu bloblar yönetimini Azure portalı kapasitesini kolayca. Bu nedenle, bir Azure Depolama Gezgini yararlanarak recommendable gereklidir. Bir Azure depolama hesabını yönetmek için yardımcı olabilecek birkaç iyi kullanılabilir olanlarla, vardır.

* Microsoft Visual Studio Azure SDK'sı (<https://azure.microsoft.com/downloads/>)
* Microsoft Azure Depolama Gezgini (<https://azure.microsoft.com/downloads/>)
* Üçüncü taraf araçları

Yedekleme daha ayrıntılı bir irdelemesi ve Azure üzerinde SAP başvurmak [SAP yedekleme Kılavuzu](sap-hana-backup-guide.md) daha fazla bilgi için.

### <a name="1b353e38-21b3-4310-aeb6-a77e7c8e81c8"></a>Microsoft Azure Marketi dışında bir SQL Server görüntüsü kullanma
Microsoft SQL Server sürümlerini içeren sanal makineleri Azure Market'te sunar. SQL Server ve Windows için Lisansı gerektiren SAP müşteriler, bu temel SQL Server'ın zaten yüklü olan Vm'leri getirerek lisans gereksinimi karşılamak için bir fırsat olabilir. SAP için görüntüleri kullanmak için aşağıdaki konuları yapılması gerekir:

* SQL Server değerlendirme olmayan sürümleri, Azure Marketi'nden dağıtılan VM 'Yalnızca Windows' dan daha yüksek maliyetleri alın. Compare prices için şu makalelere bakın: <https://azure.microsoft.com/pricing/details/virtual-machines/windows/> ve <https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-enterprise/>. 
* Yalnızca SQL Server 2012 gibi bir SAP tarafından desteklenen SQL Server sürümleri kullanabilir.
* Azure Marketi'nde sunulan vm'lerde yüklü SQL Server örneğinin harmanlama SAP NetWeaver'ı çalıştırmak için SQL Server örneği gerektirir harmanlama değil. Aşağıdaki bölümde yönde rağmen ile harmanlama değiştirebilirsiniz.

#### <a name="changing-the-sql-server-collation-of-a-microsoft-windowssql-server-vm"></a>Microsoft Windows/SQL Server VM, SQL Server harmanlaması değiştiriliyor
SQL Server görüntüleri Azure Market'te SAP NetWeaver uygulamaları tarafından gerekli harmanlamasını kullanacak şekilde ayarlanmadınız beri dağıtımdan hemen sonra değiştirilmesi gerekiyor. SQL Server 2012 için bu adımlara yönetici dağıtılan VM'de oturum açmaya devam edebilir ve VM dağıtıldıktan sürede gerçekleştirilebilir:

* Yönetici olarak bir Windows komut penceresi açın.
* Dizini için C:\Program Files\Microsoft SQL Server\110\Setup Bootstrap\SQLServer2012 değiştirin.
* Komutu yürütün: Setup.exe/QUIET/eylem = REBUILDDATABASE InstanceName = MSSQLSERVER /SQLSYSADMINACCOUNTS =`<local_admin_account_name`> /SQLCOLLATION SQL_Latin1_General_Cp850_BIN2 =   
  * `<local_admin_account_name`> VM Galeriden ilk kez dağıtırken yönetici hesabı olarak tanımlanmış olan hesap.

İşlem yalnızca birkaç dakika sürer. Adım doğru sonucuyla sonlandı emin olmak için aşağıdaki adımları gerçekleştirin:

* SQL Server Management Studio’yu açın.
* Bir sorgu penceresi açın.
* Komut sp_helpsort SQL Server ana veritabanında yürütün.

İstenen sonucu gibi görünmelidir:

    Latin1-General, binary code point comparison sort for Unicode Data, SQL Server Sort Order 40 on Code Page 850 for non-Unicode Data

Sonuç bu değilse, SAP dağıtmadan DURDURMAK ve neden Kurulum komutu beklendiği gibi çalışmaması inceleyin. SAP NetWeaver uygulamalarının daha yukarıda bahsedilen farklı SQL Server kod sayfaları ile SQL Server örneği dağıtımı **değil** desteklenir.

### <a name="sql-server-high-availability-for-sap-in-azure"></a>SQL Server yüksek kullanılabilirlik için azure'da SAP
Bu belgede daha önce belirtildiği gibi en eski SQL Server yüksek kullanılabilirlik işlevselliğini kullanımı için gerekli olan paylaşılan depolama alanı oluşturmak için kaybetme riski yoktur. Bu işlev, bir Windows Server Yük devretme kümesi (kullanıcı veritabanları (ve tempdb sonunda) paylaşılan disk kullanarak WSFC) iki veya daha fazla SQL Server örneğini yükleyin. Ayrıca SAP tarafından desteklenen uzun zaman standart yüksek kullanılabilirlik yöntem budur. Azure paylaşılan depolama desteklemediğinden, SQL Server yüksek kullanılabilirlik yapılandırmaları paylaşılan disk küme yapılandırması ile gerçekleşen olamaz. Ancak, diğer pek çok yüksek kullanılabilirlik yöntemleri hala yapılabilir ve aşağıdaki bölümlerde açıklanmıştır.

#### <a name="sql-server-log-shipping"></a>SQL Server günlük aktarma
Yüksek kullanılabilirlik (HA) yöntemleri SQL Server günlük aktarma biridir. Yüksek kullanılabilirlik yapılandırmasında katılan Vm'leri ad çözümlemesinin çalışıp varsa, bir sorun yok ve Kurulum azure'da şirket içinde gerçekleştirilen herhangi bir ayar farklı değildir. Yalnızca IP çözünürlüğüne yararlanmayı önerilmez. Günlük aktarma ve günlük aktarma etrafında ilkeler ayarlama bakımından, bu belgelere bakın:

<https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server>

Herhangi bir yüksek kullanılabilirlik elde etmek için aynı Azure kullanılabilirlik kümesi içinde olacak şekilde bu tür bir günlük aktarma yapılandırması içinde olan Vm'leri dağıtmak bir gerekir.

#### <a name="database-mirroring"></a>Veritabanı yansıtma
SAP tarafından desteklenen yansıtma veritabanı (SAP notu bkz [965908]) üzerinde bir yük devretme iş ortağı SAP bağlantı dizesinde tanımlama kullanır. Şirketler arası durumlarda, iki VM aynı etki alanında olduğunu ve kullanıcı bağlamı iki SQL Server örneklerini de bir etki alanı kullanıcı altında çalışan ve ilgili iki SQL Server durumlarda yeterli ayrıcalıklara sahip varsayıyoruz. Bu nedenle, azure'da veritabanı yansıtma kurulumu tipik şirket içi Kurulumu/yapılandırma farklı değildir.

Yalnızca bulut dağıtımlarına itibarıyla en kolay yöntem bir etki alanı içinde başka bir etki alanı Kurulum bu DBMS Vm'leri (ve ideal olarak özel SAP VM'ler) Azure'da zorunda kalmaz.

Bir etki alanı mümkün değilse, bir sertifika için uç noktalar burada açıklandığı şekilde yansıtma veritabanı kullanabilirsiniz: <https://docs.microsoft.com/sql/database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql>

Bir öğretici, azure'da veritabanı yansıtma ayarlamak için burada bulunabilir: <https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server> 

#### <a name="sql-server-always-on"></a>SQL Server her zaman açık
Always On şirket için SAP desteklenmediğinden (lu SAP notuna bakın [1772688]), azure'da SAP ile birlikte kullanılacak desteklenir. Paylaşılan diskler Azure'da oluşturmak mümkün olmadığını olgu bir başka sanal makineler arasında bir her zaman üzerinde Windows Server Yük devretme kümesi (WSFC) yapılandırması oluşturulamıyor anlamına gelmez. Bu, yalnızca bir paylaşılan disk küme yapılandırmasında bir çekirdek olarak kullanma olasılığını olmadığını anlamına gelir. Bu nedenle Azure üzerinde her zaman WSFC yapılandırması oluşturma ve paylaşılan disk yararlanan çekirdek türü seçin. Azure ortamı bu sanal makineler Çözümle ada göre Vm'leri ve Vm'leri aynı etki alanında olmalıdır dağıtılır. Bu seçenek, yalnızca Azure ve şirket içi dağıtımlar için geçerlidir. SQL Server kullanılabilirlik grubu dinleyicisini (Azure kullanılabilirlik kümesi'ile karıştırılır olarak değil) dağıtımı geçici olarak bazı özel durumlar vardır. bu yana Azure bu anda olası şirket içinde olduğu gibi bir AD/DNS nesne oluşturmak için izin vermez. Bu nedenle, bazı farklı bir yükleme adımları Azure belirli davranışını üstesinden gelmek gereklidir.

Bir kullanılabilirlik grubu dinleyicisi kullanarak bazı hususlardır:

* Bir kullanılabilirlik grubu dinleyicisi kullanarak yalnızca Windows Server 2012 veya daha fazla sanal makinenin konuk işletim sistemi mümkündür. Windows Server 2012 için bu düzeltme ekini uygulandığından emin olmanız gerekir: <https://support.microsoft.com/kb/2854082> 
* Windows Server 2008 R2 için bu düzeltme eki yok ve Always On #içeren veritabanı yansıtma ile aynı şekilde belirterek bir yük devretme ortağı bağlantı dizesinde kullanılacak (SAP default.pfl parametresi dbs/mss/sunucu - Bitti luSAPnotunabakın[965908]).
* Bir kullanılabilirlik grubu dinleyicisi kullanırken, veritabanı VM'lerin ayrılmış bir yük dengeleyiciye bağlı gerekir. Ad çözümlemesi, yalnızca bulut dağıtımlarına ya da gerektirir (uygulama sunucuları, DBMS sunucu ve (A) SCS sunucu) bir SAP sistemiyle tüm VM'lerin aynı sanal ağdaki veya bir SAP uygulama katmanından sırayla etc\host dosyasının bakım gerektirir SQL Server Vm'leri çözümlenen VM adlarını almak için. Azure her iki VM kapatma bu arada olduğu durumlarda yeni IP adresleri atama, önlemek için bir statik IP adresleri bu ağ arabirimleri için Vm'leri (statik IP adresi içindeaçıklanantanımlamaAlwaysOnyapılandırmaatamanızgerekir[bu] [ virtual-networks-reserved-private-ip] makale)

[comment]: <> (Eski blogları)
[comment]: <> (<https://blogs.msdn.com/b/alwaysonpro/archive/2014/08/29/recommendations-and-best-practices-when-deploying-sql-server-alwayson-availability-groups-in-windows-azure-iaas.aspx>, <https://blogs.technet.com/b/rmilne/archive/2015/07/27/how-to-set-static-ip-on-azure-vm.aspx>) 
* Küme düğümü üzerinde oluşturulan küme adı aynı IP adresini geçerli işlevselliği ile Azure atadığınız çünkü küme özel bir IP adresi gereken yere WSFC küme yapılandırma atandığında, gerekli ek adımlar vardır. Bu, kümeye farklı bir IP adresi atamak için el ile gerçekleştirilmesi anlamına gelir.
* Kullanılabilirlik grubu dinleyicisi, kullanılabilirlik grubunun birincil ve ikincil çoğaltmalar çalıştıran sanal makinelere atanan TCP/IP'yi uç noktaları sayesinde, Azure'da oluşturulacak geçiyor.
* Bu uç nokta ACL'leri ile güvenli hale getirmek için bir gereksinim olabilir.

[comment]: <> (TODO eski blogu)
[comment]: <> (Ayrıntılı adımlar ve Azure'da AlwaysOn yapılandırmasını yükleme necessities öğreticide kullanılabilir [here][virtual-machines-windows-classic-ps-sql-alwayson-availability-groups] yürüyen olduğunda en iyi karşılaştı)
[comment]: <> (Azure Galerisi yoluyla AlwaysOn kurulumuna önceden yapılandırılmış <https://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx>)
[comment]: <> (Bir kullanılabilirlik grubu dinleyicisi oluşturma [this][virtual-machines-windows-classic-ps-sql-int-listener] öğreticide en iyi şekilde açıklanabilir)
[comment]: <> (En iyi ACL'lerine sahip ağ uç noktaları güvenliğini sağlama aşağıda açıklanmıştır:)
[comment]: <> (* <https://michaelwasham.com/windows-azure-powershell-reference-guide/network-access-control-list-capability-in-windows-azure-powershell/>)
[comment]: <> (* <https://blogs.technet.com/b/heyscriptingguy/archive/2013/08/31/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-1-of-2.aspx> )
[comment]: <> (* <https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/01/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-2-of-2.aspx>)  
[comment]: <> (* <https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/18/creating-acls-for-windows-azure-endpoints.aspx>) 

Farklı Azure bölgelerine de bir SQL Server Always On kullanılabilirlik grubu dağıtmak mümkündür. Bu işlevselliği Azure VNet-Vnet bağlantısı yararlanır ([daha fazla ayrıntı][virtual-networks-configure-vnet-to-vnet-connection]).

[comment]: <> (TODO eski blogu)
[comment]: <> (SQL Server AlwaysOn Kullanılabilirlik grupları kurulumunu böyle bir senaryoda burada açıklanan: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.) 

#### <a name="summary-on-sql-server-high-availability-in-azure"></a>Azure'da SQL Server yüksek kullanılabilirlik özeti
Azure depolama içeriği koruduğu gerçeği göz önünde bulundurulduğunda, daha az bir nedeni bir etkin bekleme görüntüsünde ısrar yoktur. Başka bir deyişle, yalnızca aşağıdaki durumlarda karşı korumak yüksek kullanılabilirlik senaryonuz gerekir:

* VM kullanılamama Azure veya diğer nedenlerle server kümesinde bakım nedeniyle bir bütün olarak
* SQL Server örneğinde yazılım sorunları
* Burada veri silindiğinde ve -belirli bir noktaya kurtarma gerekli el ile hata koruma

Günlük aktarma tarafından üçüncü durumda yalnızca kapsamında olabilir ancak bir veritabanı yansıtma veya her zaman açık, ilk iki durum alınabiliyorsa buna eşleşen teknolojileri bakarak.

Her zaman daha üstün olan yönleri Always On ile karşılaştırıldığında, veritabanı yansıtması için açık, daha karmaşık kurulumunun dengelemeniz gerekir. Bu avantajlar gibi listelenir:

* Okunabilir ikincil çoğaltma.
* İkincil çoğaltma yedeklemelerin.
* Daha yüksek ölçeklenebilirlikten.
* Daha fazla bir ikincil çoğaltma.

### <a name="9053f720-6f3b-4483-904d-15dc54141e30"></a>Genel SQL Server SAP on Azure özeti
Bu kılavuzda çok sayıda öneri ve birden çok kez Azure dağıtımınızı planlama önce okurken önerilir. Genel olarak, ancak genel on DBMS Azure belirli noktalarında takip ettiğinizden emin olun:

[comment]: <> (2.3 daha yüksek üretilen işten ne? Bir VHD?)
1. Azure'da çoğu avantajları olan SQL Server 2014 gibi en son DBMS sürümü kullanın. SQL Server için SQL Server 2012 SP1, Azure depolama karşılaştıklarında yedekleme özelliği içerir CU4 budur. Bununla birlikte, SAP ile birlikte, en az SQL Server 2014 SP1 CU1 veya SQL Server 2012 SP2 ve en son CU kullanmak için önerilir.
2. Dikkatli bir şekilde Azure kısıtlamaları ve veri dosya düzeni dengelemek için azure'da SAP sistemi ortamınızı planlayın:
   * Yoksa çok fazla disk olmalıdır; ancak, gereken IOPS size ulaşabildiğimizden emin olmak için yeterli.
   * Yönetilen diskleri kullanmazsanız, IOPS ayrıca Azure depolama hesabı sınırlı olduğunu ve depolama hesapları her Azure aboneliğinde sınırlı olduğunu unutmayın ([daha fazla ayrıntı][azure-subscription-service-limits]). 
   * Yalnızca stripe disklerde bulunan daha yüksek bir performans sağlamak istiyorsanız.
3. Hiçbir zaman yazılımını yükleyin veya kalıcı olmayan ve herhangi bir şey bu sürücüdeki bir Windows önyükleme sırasında kaybolur D:\ sürücüsüne Kalıcılık gerektiren tüm dosyaları yerleştirin.
4. Azure standart depolama için diski önbelleğe alma işlemi kullanmayın.
5. Coğrafi olarak çoğaltılmış Azure depolama hesapları kullanmayın.  Yerel olarak yedekli DBMS iş yükleri için kullanın.
6. Veritabanı verileri çoğaltmak için DBMS satıcınızın HA/DR çözümü kullanın.
7. Her zaman ad çözümlemesini kullanmasına, IP adreslerini güvenmeyin.
8. Olası en yüksek veritabanı sıkıştırma kullanın. SQL Server için sayfa sıkıştırmayı olduğu.
9. SQL Server görüntüleri Azure Market'te kullanırken dikkatli olun. SQL Server birini kullanıyorsanız, herhangi bir SAP NetWeaver sistemini yüklemeden önce örneği harmanlamasıyla değiştirmeniz gerekir.
10. Yükleme ve yapılandırma bölümünde anlatıldığı gibi SAP izleme ana bilgisayarı için Azure [Dağıtım Kılavuzu][deployment-guide].

## <a name="specifics-to-sap-ase-on-windows"></a>Windows üzerinde SAP ASE için özellikleri
Microsoft Azure ile başlayarak, mevcut Azure sanal makinelerinde SAP ASE uygulamalarınızı kolayca geçirebilirsiniz. Bir sanal makinede SAP ASE kolayca bu uygulamalar Microsoft azure'a geçirerek toplam sahip olma maliyetini dağıtım, yönetim ve Bakım Kurumsal avantajlarına uygulama azaltmanızı sağlar. Bir Azure sanal Makinesi'nde SAP ASE ile yöneticiler ve geliştiriciler aynı geliştirme ve şirket içi kullanılabilir olan yönetim araçlarını kullanmaya devam edebilirsiniz.

Azure sanal burada bulunan makineler için bir SLA yoktur: <https://azure.microsoft.com/support/legal/sla/virtual-machines>

Microsoft Azure da diğer genel bulut sanallaştırma teklifleri, ancak tek sonuçları ile barındırılan sanal makineleri gerçekleştirir değişebilir başarılara duyuyoruz. SAP sayıda farklı SAP boyutlandırma SAP sertifikalı VM SKU'ları ayrı bir SAP Not sağlanan [1928533].

Deyimleri ve Azure depolama, SAP VM dağıtımı veya SAP izleme önerileri kullanım in regard to SAP ASE dağıtımları SAP uygulamaları ile birlikte bu belgenin ilk dört bölüm belirtildiği gibi uygulanır.

### <a name="sap-ase-version-support"></a>SAP ASE sürüm desteği
Şu anda desteklediği SAP ASE sürüm 16,0 SAP Business Suite ürünlerle kullanmak üzere SAP. Tüm güncelleştirmeleri SAP ASE sunucusu veya SAP Business Suite ürünleri ile kullanılacak JDBC ve ODBC sürücüleri için SAP Service Marketplace yalnızca aracılığıyla sağlanır: <https://support.sap.com/swdc>.

Yüklemeleri şirket içinde olduğu gibi JDBC ve ODBC sürücüleri veya SAP ASE sunucunun güncelleştirmeleri doğrudan Sybase sitelerinden yüklemeyin. Düzeltme ekleri hakkında ayrıntılı bilgi için SAP Business Suite ürün şirket içi kullanım için desteklenir ve Azure sanal Makineler'de aşağıdaki SAP notları görebilirsiniz:

* [1590719]
* [1973241]

SAP Business Suite, SAP ASE üzerinde çalıştırma hakkında genel bilgiler bulunabilir [SCN](https://www.sap.com/community/topic/ase.html)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Azure vm'lerde SAP ilgili SAP ASE yüklemeleri için SAP ASE yapılandırma yönergeleri
#### <a name="structure-of-the-sap-ase-deployment"></a>SAP ASE dağıtım yapısı
Genel Açıklama uygun olarak, SAP ASE yürütülebilir dosyaları bulunan veya sanal makinenin işletim sistemi diski sistem sürücüsüne yüklü (c: sürücüsü\). Genellikle, SAP ASE sisteminizi ve araçlarınızı veritabanlarının En zorlu SAP NetWeaver iş yüküne göre kullanılmaz. Bu nedenle sisteminizi ve araçlarınızı veritabanları (master, model, saptools, sybmgmtdb, sybsystemdb) de C:\ sürücüde kalır. 

Bir özel durum, tüm iş tabloları ve daha yüksek veri hacmine veya özgün sanal makinenin işletim sistemi içinde sığamıyorsa g/ç işlemleri toplu gerektirebilir, bazı SAP ERP ve tüm BW iş yükleri durumunda SAP ASE tarafından oluşturulan geçici tablolar içeren bir geçici veritabanı olabilir disk (c: sürücüsü\).

SAPInst/SWPM sistemini yüklemek için kullanılan sürümüne bağlı olarak, veritabanı içerebilir:

* SAP ASE yüklenirken oluşturulan tek bir SAP ASE tempdb
* SAP ASE ve SAP yükleme yordamı tarafından oluşturulan bir ek saptempdb yükleyerek oluşturulmuş bir SAP ASE tempdb
* SAP ASE ve el ile oluşturulmuş bir ek tempdb yükleyerek oluşturulmuş bir SAP ASE tempdb (örneğin'lu SAP notuna aşağıdaki [1752266]) ERP/BW belirli tempdb gereksinimlerini karşılamak için

Belirli bir ERP veya tüm BW iş yükleri olması durumunda, C:\ dışında bir sürücüde (SWPM tarafından veya elle) ek olarak oluşturulan tempdb tempdb cihazları korumak için performansı in regard to mantıklıdır. Hiçbir ek tempdb varsa oluşturmanız önerilir (SAP notu [1752266]).

Bu tür sistemler için ayrıca oluşturulan tempdb için aşağıdaki adımlar gerçekleştirilmelidir:

* SAP veritabanının ilk cihaza ilk tempdb cihaz Taşı
* Her bir cihaz SAP veritabanının içeren VHD'ler için tempdb cihaz Ekle

Bu yapılandırma, tempdb ya da sistem sürücüsünün sağlamak mümkün olandan daha fazla alanı kullanmak üzere etkinleştirir. Bir başvuru olarak bir şirket içi mevcut sistemler, tempdb cihaz boyutları kontrol edebilirsiniz. Veya bu tür bir yapılandırma sistem sürücüsü ile sağlanan tempdb karşı IOPS sayılarında etkinleştirmez. Yeniden şirket içinde çalışan sistemler, tempdb karşı g/ç iş yükünü izlemek için kullanılabilir.

Hiçbir zaman herhangi bir SAP ASE cihaza VM D:\ sürücüsüne yerleştirin. Tempdb tutulan nesneleri yalnızca geçici olsa bile bu tempdb için de geçerlidir.

#### <a name="impact-of-database-compression"></a>Veritabanı sıkıştırması etkisini
Burada, g/ç bant genişliği sınırlayıcı bir etmen haline gelebilir yapılandırmalarında, IOPS azaltır her ölçü bir Azure gibi bir Iaas senaryosu çalıştırabilirsiniz iş yükü esnetme artırmaya yardımcı olabilir. Bu nedenle, SAP ASE sıkıştırma mevcut SAP veritabanını Azure'a yüklemeden önce kullanıldığından emin olmak için önerilir.

Sıkıştırma zaten uygulanmadı, önce Azure'a yükleme gerçekleştirmek için öneri dışında çeşitli nedenleri verilmiştir:

* Azure'a karşıya yüklenecek veri miktarını düşük
* Daha fazla CPU veya daha yüksek g/ç bant genişliği veya g/ç gecikme şirket içi daha az daha güçlü donanım kullanabilirsiniz varsayarak sıkıştırma yürütme süresi kısadır
* Küçük veritabanı boyutları için disk ayırmayı daha az maliyetlerine neden olabilir

Veri ve LOB sıkıştırma, şirket içi yaptığı gibi Azure sanal Makineler'de barındırılan bir VM içinde çalışır. Varolan bir SAP ASE veritabanında kullanım sıkıştırma zaten kullanımda olup olmadığını denetlemek nasıl daha fazla bilgi için ' lu SAP notuna denetleyin [1750510].

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Veritabanı örnekleri izlemek için DBACockpit kullanma
SAP ASE veritabanı platform kullanan, SAP sistemlerini için DBACockpit işlem DBACockpit katıştırılmış tarayıcı pencerelerini veya Webdynpro olarak erişilebilir durumdadır. Ancak veritabanı yönetme ve izleme için tam işlevsellik yalnızca DBACockpit Webdynpro uygulamasında kullanılabilir.

Olarak şirket içi sistemler ile birkaç adım DBACockpit Webdynpro uygulaması tarafından kullanılan tüm SAP NetWeaver işlevselliğini etkinleştirmek için gereklidir. SAP notu izleyin [1245200] webdynpros kullanımını etkinleştirin ve gerekli değerler oluşturmak için. Yukarıdaki Notları'ndaki yönergeleri takip ederken, ayrıca Internet iletişimi Yöneticisi'ni (ICM) http ve https bağlantıları için kullanılacak bağlantı noktaları ile birlikte yapılandırırsınız. Http için varsayılan ayarı şöyle görünür:

> ICM/server_port_0 değerler bağlantı noktası = HTTP bağlantı noktası = 8000 PROCTIMEOUT = zaman AŞIMI 600 = 600 =
> 
> ICM/server_port_1 değerler bağlantı noktası = HTTPS, bağlantı noktası = = 443$ $, PROCTIMEOUT 600, zaman AŞIMI = 600 =
> 
> 

ve işlem sırasında oluşturulan bağlantılar DBACockpit şuna benzer:

> https://`<fullyqualifiedhostname`>: sap/44300/bc/sap/webdynpro/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: sap/8000/bc/sap/webdynpro/dba_cockpit
> 
> 

Bağlı olarak, ve -siteye, SAP sistemine barındıran Azure sanal makinesine bağlı nasıl çok siteli veya ExpressRoute (şirket içi dağıtımı) bu ICM emin olmak için ihtiyacınız çözülebilir tam bir ana bilgisayar makinesinde kullanan burada gelen DBACockpit açmaya çalışıyorsunuz. ' Lu SAP notuna bakın [773830] ICM profili parametrelerinden ve kümesi parametre ICM/host_name_full bağlı olarak tam ana bilgisayar adı açıkça gerekirse etiketleneceğini nasıl anlamak için.

Şirket içi ile Azure arasında şirketler arası bağlantı olmadan yalnızca bulut senaryosunda VM dağıttıysanız, genel bir IP adresi ve bir domainlabel tanımlamanız gerekir. Genel DNS adı VM'nin biçimi şöyle görünür:

> `<custom domainlabel`>.`<azure region`>.cloudapp.azure.com
> 
> 

DNS adına ilgili daha fazla ayrıntı bulunabilir [burada][virtual-machines-azurerm-versus-azuresm].

Azure VM bağlantısı için DNS adı SAP profili parametresi ICM/host_name_full ayarlama şuna benzeyebilir:

> https://mydomainlabel.westeurope.cloudapp.net:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://mydomainlabel.westeurope.cloudapp.net:8000/sap/bc/webdynpro/sap/dba_cockpit
> 
> 

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
SAP ASE Azure'a dağıtırken, yedekleme yönteminize gözden geçirilmesi gerekir. Sistem, üretken bir sistem değil olsa bile, SAP ASE tarafından barındırılan SAP veritabanı düzenli aralıklarla yedeklenmelidir. Azure depolama üç görüntü tutar olduğundan, bir yedekleme şimdi daha az depolama kilitlenme telafi için açısından önemlidir. Uygun bir yedekleme ve geri yükleme planı sürdürmek için birincil nedeni, belirli bir noktadaki zaman kurtarma özellikleri sağlayarak mantıksal/el ile hataları dengeleyebilir daha fazla olmasıdır. Bu nedenle yedeklemelerini geri belirli bir noktaya veritabanını geri veya var olan veritabanı kopyalayarak başka bir sistem kaynağını oluşturmak için Azure'da yedeklemelerini kullanın ya da kullanın olmaktır. Örneğin, 2 katmanlı SAP yapılandırmasından aynı sistem bir 3 katmanlı sistem ayarı için bir yedekleme geri yükleyerek aktardığınız.

Şirket içi yedekleme ve azure'da bir veritabanı geri yükleme aynı şekilde çalışır. SAP notlara bakın:

* [1588316]
* [1585981]

Döküm yapılandırmaları oluşturma ve zamanlama yedekleme hakkında daha fazla bilgi için. Yapılandırabileceğiniz gereksinimlerini ve strateji bağlı olarak veritabanı ve günlük dökümleri var olan disklerden biri disk veya yedekleme için ek bir disk ekleyin. Veri kaybı bir hata durumunda olma tehlikesi azaltmak için hiçbir veritabanı cihazın bulunduğu bir disk kullanmak için önerilir.

Veri ve LOB yanı sıra SAP ASE sıkıştırma, yedekleme sıkıştırma de sunar. Veritabanı ve günlük dökümleri ile daha az alan kaplaması için bunu yedekleme sıkıştırma kullanılması önerilir. Daha fazla bilgi için bkz. Not SAP [1588316]. Yedekleme sıkıştırma ayrıca yedekleri ya da şirket içi Azure sanal makinenin yedekleme dökümleri içeren VHD'ler indirmeyi planlıyorsanız aktarılan veri miktarını azaltmak çok önemlidir.

Sürücü D:\ döküm hedef veritabanı veya günlük olarak kullanmayın.

#### <a name="performance-considerations-for-backupsrestores"></a>Yedekleme/geri yükleme işlemleri için performans konuları
Çıplak metal dağıtımlarındaki yedekleme/geri yükleme performansı, paralel olarak kaç birimleri okunabilir ve ne bu birimlerin verimi olabilir bağlıdır. Yedekleme sıkıştırma tarafından kullanılan CPU tüketimini Vm'lerde ile en çok sekiz adet CPU iş parçacıklarını Ayrıca, önemli bir rol oynayabilir. Bu nedenle, bir kabul edilebilir:

* Daha az veritabanı cihazları depolamak için kullanılan disk sayısını daha küçük genel üretilen işi okuma
* Daha küçük CPU sayısını, VM, daha ciddi yedekleme sıkıştırma etkisini iş parçacıkları
* Yedekleme için daha düşük aktarım hızı yazmak için daha az sayıda hedefle (Şerit dizinleri, diskler)

Kullanılan/birleşik, gereksinimlerinize bağlı olarak iki seçenek yazılacak hedefleri sayısını artırmak için:

* Şeritli birim IOPS aktarım hızını iyileştirmek için birden fazla bağlı diskler üzerinde Yedekleme hedef birimin bölümlemesi
* Döküm yapılandırma düzeyinde SAP ASE oluşturma, birden fazla hedef dizin dökümünü almak için yazılacak kullanır

Bir birim birden çok bağlı diski bölümlenerek daha önce bu kılavuzda ele alınan. Birden çok dizin SAP ASE döküm yapılandırmasında kullanma ile ilgili daha fazla bilgi için döküm yapılandırmasını oluşturmak için kullanılan saklı yordam sp_config_dump belgelerine bakın [Sybase Bilgi Merkezi](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Azure sanal makineler ile olağanüstü durum kurtarma
#### <a name="data-replication-with-sap-sybase-replication-server"></a>Veri çoğaltma ile SAP Sybase çoğaltma sunucusu
SAP Sybase çoğaltma sunucusuna (SRS) SAP ASE ile veritabanı işlemleri zaman uyumsuz olarak uzak bir konuma aktarmak için normal bir bekleme çözümü sağlar. 

Yükleme ve SRS işleyişini çalışır de işlevsel olarak şirket içi yaptığı gibi Azure sanal makine hizmetlerinde barındırılan bir VM.

SAP ASE HADR işletim sistemi düzeyinde kümeleme üzerinde bağımlılıkları olmayan bir Azure iç yük dengeleyici gerektirmez ve Azure Windows ve Linux Vm'leri üzerinde çalışır. SAP ASE HADR üzerinde ayrıntılarını okumak için [SAP ASE HADR Kullanıcı Kılavuzu](https://help.sap.com/viewer/efe56ad3cad0467d837c8ff1ac6ba75c/16.0.3.3/en-US/a6645e28bc2b1014b54b8815a64b87ba.html).

## <a name="specifics-to-sap-ase-on-linux"></a>Linux'ta SAP ASE için özellikleri
Microsoft Azure ile başlayarak, mevcut Azure sanal makinelerinde SAP ASE uygulamalarınızı kolayca geçirebilirsiniz. Bir sanal makinede SAP ASE kolayca bu uygulamalar Microsoft azure'a geçirerek toplam sahip olma maliyetini dağıtım, yönetim ve Bakım Kurumsal avantajlarına uygulama azaltmanızı sağlar. Bir Azure sanal Makinesi'nde SAP ASE ile yöneticiler ve geliştiriciler aynı geliştirme ve şirket içi kullanılabilir olan yönetim araçlarını kullanmaya devam edebilirsiniz.

Azure Vm'leri dağıtmak için burada bulunan resmi SLA'lar bilmeniz önemlidir: <https://azure.microsoft.com/support/legal/sla>

Boyut bilgisini SAP ve SAP sertifikalı sanal makine SKU'ları listesi SAP notu sağlanan [1928533]. Azure sanal makineleri burada bulunabilir belgeleri boyutlandırma ek SAP <http://blogs.msdn.com/b/saponsqlserver/archive/2015/06/19/how-to-size-sap-systems-running-on-azure-vms.aspx> ve burada <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

Deyimleri ve Azure depolama, SAP VM dağıtımı veya SAP izleme önerileri kullanım in regard to SAP ASE dağıtımları SAP uygulamaları ile birlikte bu belgenin ilk dört bölüm belirtildiği gibi uygulanır.

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
Genel bir açıklama uygun olarak, SAP ASE yürütülebilir dosyaları bulunan veya kök dosya sistemine (/sybase) VM'nin yüklenir. Genellikle, SAP ASE sisteminizi ve araçlarınızı veritabanlarının En zorlu SAP NetWeaver iş yüküne göre refs'nin değil. Bu nedenle sisteminizi ve araçlarınızı veritabanları (master, model, saptools, sybmgmtdb, sybsystemdb) de kök dosya sisteminde kalabilir. 

Bir özel durum, tüm iş tabloları ve daha yüksek veri hacmine veya g/ç işlemleri, özgün sanal makinenin işletim sistemi içinde sığamıyorsa birimi gerekebilir, bazı SAP ERP ve tüm BW iş yükleri durumunda SAP ASE tarafından oluşturulan geçici tablolar içeren bir geçici veritabanı olabilir disk.

SAPInst/SWPM sistemini yüklemek için kullanılan sürümüne bağlı olarak, veritabanı içerebilir:

* SAP ASE yüklenirken oluşturulan tek bir SAP ASE tempdb
* SAP ASE ve SAP yükleme yordamı tarafından oluşturulan bir ek saptempdb yükleyerek oluşturulmuş bir SAP ASE tempdb
* SAP ASE ve el ile oluşturulmuş bir ek tempdb yükleyerek oluşturulmuş bir SAP ASE tempdb (örneğin'lu SAP notuna aşağıdaki [1752266]) ERP/BW belirli tempdb gereksinimlerini karşılamak için

Belirli bir ERP veya tüm BW iş yükleri olması durumunda, tek bir Azure veri diski ya da Linux RAID spannin tarafından temsil edilebilir bir ayrı bir dosya sisteminde ayrıca oluşturulan tempdb (SWPM tarafından veya elle) tempdb cihazları korumak için performansı in regard to mantıklıdır g birden çok Azure veri diski. Hiçbir ek tempdb varsa oluşturmanız önerilir (SAP notu [1752266]).

Bu tür sistemler için ayrıca oluşturulan tempdb için aşağıdaki adımlar gerçekleştirilmelidir:

* İlk tempdb dizini SAP veritabanına ilk dosya sistemine taşıma
* Tempdb dizinler her bir dosya sistemi SAP veritabanının bulunduğu disk ekleme

Bu yapılandırma, tempdb ya da sistem sürücüsünün sağlamak mümkün olandan daha fazla alanı kullanmak üzere etkinleştirir. Bir başvuru olarak bir şirket içi mevcut sistemler, tempdb dizini boyutları kontrol edebilirsiniz. Veya bu tür bir yapılandırma sistem sürücüsü ile sağlanan tempdb karşı IOPS sayılarında etkinleştirmez. Yeniden şirket içinde çalışan sistemler, tempdb karşı g/ç iş yükünü izlemek için kullanılabilir.

Hiçbir zaman herhangi bir SAP ASE dizin /mnt veya sanal makinenin /mnt/resource üzerine yerleştirin. Kalıcı olmayan bir varsayılan Azure VM geçici alanı /mnt veya /mnt/resource olduğundan tempdb tutulan nesneleri yalnızca geçici olsa bile bu tempdb için de geçerlidir. Azure VM geçici alanı hakkında daha fazla ayrıntı bulunabilir [bu makalede][virtual-machines-linux-how-to-attach-disk]

#### <a name="impact-of-database-compression"></a>Veritabanı sıkıştırması etkisini
Burada, g/ç bant genişliği sınırlayıcı bir etmen haline gelebilir yapılandırmalarında, IOPS azaltır her ölçü bir Azure gibi bir Iaas senaryosu çalıştırabilirsiniz iş yükü esnetme artırmaya yardımcı olabilir. Bu nedenle, SAP ASE sıkıştırma mevcut SAP veritabanını Azure'a yüklemeden önce kullanıldığından emin olmak için önerilir.

Sıkıştırma zaten uygulanmadı, önce Azure'a yükleme gerçekleştirmek için öneri dışında çeşitli nedenleri verilmiştir:

* Azure'a karşıya yüklenecek veri miktarını düşük
* Daha fazla CPU veya daha yüksek g/ç bant genişliği veya g/ç gecikme şirket içi daha az daha güçlü donanım kullanabilirsiniz varsayarak sıkıştırma yürütme süresi kısadır
* Küçük veritabanı boyutları için disk ayırmayı daha az maliyetlerine neden olabilir

Veri ve LOB sıkıştırma, şirket içi yaptığı gibi Azure sanal Makineler'de barındırılan bir VM içinde çalışır. Varolan bir SAP ASE veritabanında kullanım sıkıştırma zaten kullanımda olup olmadığını denetlemek nasıl daha fazla bilgi için ' lu SAP notuna denetleyin [1750510]. Veritabanı sıkıştırma hakkında ek bilgi için bkz. Not SAP [2121797].

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Veritabanı örnekleri izlemek için DBACockpit kullanma
SAP ASE veritabanı platform kullanan, SAP sistemlerini için DBACockpit işlem DBACockpit katıştırılmış tarayıcı pencerelerini veya Webdynpro olarak erişilebilir durumdadır. Ancak veritabanı yönetme ve izleme için tam işlevsellik yalnızca DBACockpit Webdynpro uygulamasında kullanılabilir.

Olarak şirket içi sistemler ile birkaç adım DBACockpit Webdynpro uygulaması tarafından kullanılan tüm SAP NetWeaver işlevselliğini etkinleştirmek için gereklidir. SAP notu izleyin [1245200] webdynpros kullanımını etkinleştirin ve gerekli değerler oluşturmak için. Yukarıdaki Notları'ndaki yönergeleri takip ederken, ayrıca Internet iletişimi Yöneticisi'ni (ICM) http ve https bağlantıları için kullanılacak bağlantı noktaları ile birlikte yapılandırırsınız. Http için varsayılan ayarı şöyle görünür:

> ICM/server_port_0 değerler bağlantı noktası = HTTP bağlantı noktası = 8000 PROCTIMEOUT = zaman AŞIMI 600 = 600 =
> 
> ICM/server_port_1 değerler bağlantı noktası = HTTPS, bağlantı noktası = = 443$ $, PROCTIMEOUT 600, zaman AŞIMI = 600 =
> 
> 

ve işlem sırasında oluşturulan bağlantılar DBACockpit şuna benzer görünecektir:

> https://`<fullyqualifiedhostname`>: sap/44300/bc/sap/webdynpro/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: sap/8000/bc/sap/webdynpro/dba_cockpit
> 
> 

Bağlı olarak, ve -siteye, SAP sistemine barındıran Azure sanal makinesine bağlı nasıl çok siteli veya ExpressRoute (şirket içi dağıtımı) bu ICM emin olmak için ihtiyacınız çözülebilir tam bir ana bilgisayar makinesinde kullanan burada gelen DBACockpit açmaya çalışıyorsunuz. ' Lu SAP notuna bakın [773830] ICM profili parametrelerinden ve kümesi parametre ICM/host_name_full bağlı olarak tam ana bilgisayar adı açıkça gerekirse etiketleneceğini nasıl anlamak için.

Şirket içi ile Azure arasında şirketler arası bağlantı olmadan yalnızca bulut senaryosunda VM dağıttıysanız, genel bir IP adresi ve bir domainlabel tanımlamanız gerekir. Genel DNS adı VM'nin biçimi şöyle görünür:

> `<custom domainlabel`>.`<azure region`>.cloudapp.azure.com
> 
> 

DNS adına ilgili daha fazla ayrıntı bulunabilir [burada][virtual-machines-azurerm-versus-azuresm].

Azure VM bağlantısı için DNS adı SAP profili parametresi ICM/host_name_full ayarlama şuna benzeyebilir:

> https://mydomainlabel.westeurope.cloudapp.net:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://mydomainlabel.westeurope.cloudapp.net:8000/sap/bc/webdynpro/sap/dba_cockpit
> 
> 

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
SAP ASE Azure'a dağıtırken yedekleme yönteminize gözden geçirilmesi gerekir. Sistem, üretken bir sistem değil olsa bile, SAP ASE tarafından barındırılan SAP veritabanı düzenli aralıklarla yedeklenmelidir. Azure depolama üç görüntü tutar olduğundan, bir yedekleme şimdi daha az depolama kilitlenme telafi için açısından önemlidir. Uygun bir yedekleme ve geri yükleme planı sürdürmek için birincil nedeni, belirli bir noktadaki zaman kurtarma özellikleri sağlayarak mantıksal/el ile hataları dengeleyebilir daha fazla olmasıdır. Bu nedenle yedeklemelerini geri belirli bir noktaya veritabanını geri veya var olan veritabanı kopyalayarak başka bir sistem kaynağını oluşturmak için Azure'da yedeklemelerini kullanın ya da kullanın olmaktır. Örneğin, 2 katmanlı SAP yapılandırmasından aynı sistem bir 3 katmanlı sistem ayarı için bir yedekleme geri yükleyerek aktardığınız.

Şirket içi yedekleme ve azure'da bir veritabanı geri yükleme aynı şekilde çalışır. SAP notlara bakın:

* [1588316]
* [1585981]

Döküm yapılandırmaları oluşturma ve zamanlama yedekleme hakkında daha fazla bilgi için. Yapılandırabileceğiniz gereksinimlerini ve strateji bağlı olarak veritabanı ve günlük dökümleri var olan disklerden biri disk veya yedekleme için ek bir disk ekleyin. Veri kaybı bir hata durumunda olma tehlikesi azaltmak için hiçbir veritabanı dizin/dosya bulunduğu bir disk kullanmak için önerilir.

Veri ve LOB yanı sıra SAP ASE sıkıştırma, yedekleme sıkıştırma de sunar. Veritabanı ve günlük dökümleri ile daha az alan kaplaması için bunu yedekleme sıkıştırma kullanılması önerilir. Daha fazla bilgi için bkz. Not SAP [1588316]. Yedekleme sıkıştırma ayrıca yedekleri ya da şirket içi Azure sanal makinenin yedekleme dökümleri içeren VHD'ler indirmeyi planlıyorsanız aktarılan veri miktarını azaltmak çok önemlidir.

Azure VM geçici alanı /mnt veya /mnt/resource döküm hedef veritabanı veya günlük olarak kullanmayın.

#### <a name="performance-considerations-for-backupsrestores"></a>Yedekleme/geri yükleme işlemleri için performans konuları
Çıplak metal dağıtımlarındaki yedekleme/geri yükleme performansı, paralel olarak kaç birimleri okunabilir ve ne bu birimlerin verimi olabilir bağlıdır. Yedekleme sıkıştırma tarafından kullanılan CPU tüketimini Vm'lerde ile en çok sekiz adet CPU iş parçacıklarını Ayrıca, önemli bir rol oynayabilir. Bu nedenle, bir kabul edilebilir:

* Daha az veritabanı cihazları depolamak için kullanılan disk sayısını daha küçük genel üretilen işi okuma
* Daha küçük CPU sayısını, VM, daha ciddi yedekleme sıkıştırma etkisini iş parçacıkları
* Yedekleme için daha düşük aktarım hızı yazmak için daha az sayıda hedefle (Linux yazılım RAID, diskler)

Kullanılan/birleşik, gereksinimlerinize bağlı olarak iki seçenek yazılacak hedefleri sayısını artırmak için:

* Şeritli birim IOPS aktarım hızını iyileştirmek için birden fazla bağlı diskler üzerinde Yedekleme hedef birimin bölümlemesi
* Döküm yapılandırma düzeyinde SAP ASE oluşturma, birden fazla hedef dizin dökümünü almak için yazılacak kullanır

Bir birim birden çok bağlı diski bölümlenerek daha önce bu kılavuzda ele alınan. Birden çok dizin SAP ASE döküm yapılandırmasında kullanma ile ilgili daha fazla bilgi için döküm yapılandırmasını oluşturmak için kullanılan saklı yordam sp_config_dump belgelerine bakın [Sybase Bilgi Merkezi](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Azure sanal makineler ile olağanüstü durum kurtarma
#### <a name="data-replication-with-sap-sybase-replication-server"></a>Veri çoğaltma ile SAP Sybase çoğaltma sunucusu
SAP Sybase çoğaltma sunucusuna (SRS) SAP ASE ile veritabanı işlemleri zaman uyumsuz olarak uzak bir konuma aktarmak için normal bir bekleme çözümü sağlar. 

Yükleme ve SRS işleyişini çalışır de işlevsel olarak şirket içi yaptığı gibi Azure sanal makine hizmetlerinde barındırılan bir VM.

ASE HADR SAP çoğaltma sunucusu aracılığıyla bu anda desteklenmiyor. Test ve Microsoft Azure platform için gelecekte yayımlanan.

## <a name="specifics-to-oracle-database-on-windows"></a>Windows üzerinde Oracle veritabanı özellikleri
Oracle yazılımları, Microsoft Windows Hyper-V ve Azure'da çalıştırmak için Oracle tarafından desteklenir. Windows Hyper-V ve Azure genel desteği hakkında daha fazla ayrıntı için denetleyin: <https://blogs.oracle.com/cloud/entry/oracle_and_microsoft_join_forces> 

Genel destek, Oracle veritabanları yararlanarak SAP uygulama belirli bir senaryoyu de desteklenir. Belgenin bu bölümünde ayrıntıları adlandırılır.

### <a name="oracle-version-support"></a>Oracle sürüm desteği
Oracle ve SAP Oracle Azure sanal Makineler'de çalışan SAP Not bulunabilir desteklenmeyen karşılık gelen işletim sistemi sürümleri [2039619].

SAP Business Suite üzerinde Oracle çalıştırmayla ilgili genel bilgileri 1DX içinde bulunabilir: <https://www.sap.com/community/topic/oracle.html>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Azure vm'lerde SAP yüklemeleri için Oracle yapılandırma yönergeleri
#### <a name="storage-configuration"></a>Depolama yapılandırması
Yalnızca tek örnek Oracle NTFS olarak biçimlendirilmiş disklerle desteklenir. Tüm veritabanı dosyaları, VHD'leri veya yönetilen diskleri temel alan NTFS dosya sisteminde depolanmış olması gerekir. Bu diskler Azure VM'sine bağlanmış ve Azure sayfa BLOB depolama alanına dayalı (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) veya yönetilen diskler (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Her türlü ağ sürücülerine veya Azure Dosya Hizmetleri gibi uzak paylaşımları:

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx> 
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx>

olan **değil** Oracle veritabanı dosyaları için desteklenen!

Bağlı diskleri kullanarak Azure sayfa BLOB Depolama veya yönetilen diskler, bu belgede bölüm yapılan deyimleri [Vm'leri ve veri diskleri için önbelleğe alma] [ dbms-guide-2.1] ve [Microsoft Azure depolama] [ dbms-guide-2.3] de Oracle Database dağıtımlar için geçerlidir.

Daha önce belgenin Genel bölümünde açıklandığı gibi Azure disklerin IOPS işleme kotalar mevcut. Tam kotalar VM türüne bağlı olarak kullanılır. Kotalarını ile VM türlerinin bir listesini bulunabilir [burada (Linux)] [ virtual-machines-sizes-linux] ve [burada (Windows)][virtual-machines-sizes-windows].

Desteklenen Azure VM türleri tanımlamak için SAP notuna bakın [1928533].

Disk başına geçerli IOPS kota gereksinimlerini karşılamadığı sürece, tek bir bağlı disk üzerindeki tüm veritabanı dosyaları depolamak mümkündür. 

Daha fazla IOPS gerekiyorsa, kullanım penceresi depolama havuzları (yalnızca Windows Server 2012'de kullanılabilir ve daha yüksek) veya Windows 2008 R2 için Windows bölmek için büyük bir mantıksal cihaz üzerinde birden fazla bağlı disk oluşturmak için önerilir (Ayrıca bkz. bölüm [ Yazılım RAID] [ dbms-guide-2.2] bu belgenin). Bu yaklaşım, disk alanını yönetmek için yönetim yükünü basitleştirir ve el ile oluşturulmuş birden çok diskte dosyaları dağıtmak için gereken çabayı önler.

#### <a name="backup--restore"></a>Yedekleme / Geri Yükleme
Yedekleme / geri yükleme işlevi, SAP BR * Oracle için Araçlar, standart Windows Server işletim sistemleri ve Hyper-V gibi aynı şekilde desteklenir. Oracle kurtarma Yöneticisi'ni (RMAN) da disk ve diskten geri yüklemek yedeklemeler için desteklenir.

#### <a name="high-availability"></a>Yüksek Kullanılabilirlik
Oracle Data Guard, yüksek kullanılabilirlik ve olağanüstü durum kurtarma amacıyla desteklenir. Ayrıntıları bulunabilir [bu] [ virtual-machines-windows-classic-configure-oracle-data-guard] belgeleri.

#### <a name="other"></a>Diğer
Azure kullanılabilirlik kümeleri veya SAP izleme gibi diğer tüm genel alanlar da Oracle veritabanı ile VM'lerin dağıtımlar için bu belgenin ilk üç bölümde açıklandığı gibi uygulayın.

## <a name="specifics-to-oracle-database-on-oracle-linux"></a>Oracle Linux'ta Oracle veritabanı özellikleri
Oracle yazılımları, Microsoft Windows Hyper-V ve Azure'da çalıştırmak için Oracle tarafından desteklenir. Windows Hyper-V ve Azure genel desteği hakkında daha fazla ayrıntı için denetleyin: <https://blogs.oracle.com/cloud/entry/oracle_and_microsoft_join_forces> 

Genel destek, Oracle veritabanları yararlanarak SAP uygulama belirli bir senaryoyu de desteklenir. Belgenin bu bölümünde ayrıntıları adlandırılır.

### <a name="oracle-version-support"></a>Oracle sürüm desteği
Oracle ve SAP Oracle Azure sanal Makineler'de çalışan SAP Not bulunabilir desteklenmeyen karşılık gelen işletim sistemi sürümleri [2039619].

SAP Business Suite üzerinde Oracle çalıştırmayla ilgili genel bilgileri 1DX içinde bulunabilir: <https://www.sap.com/community/topic/oracle.html>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Azure vm'lerde SAP yüklemeleri için Oracle yapılandırma yönergeleri
#### <a name="storage-configuration"></a>Depolama yapılandırması
Yalnızca Oracle ext3 kullanarak, diskleri ext4 ve xfs biçimlendirilmiş tek örnek desteklenir. Tüm veritabanı dosyaları, VHD'leri veya yönetilen diskleri temel alan bu dosya sistemlerine depolanmalıdır. Bu diskler Azure VM'sine bağlanmış ve Azure sayfa BLOB depolama alanına dayalı (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) veya yönetilen diskler (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Her türlü ağ sürücülerine veya Azure Dosya Hizmetleri gibi uzak paylaşımları:

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx> 
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx>

olan **değil** Oracle veritabanı dosyaları için desteklenen!

Bağlı diskleri kullanarak Azure sayfa BLOB Depolama veya yönetilen diskler, bu belgede bölüm yapılan deyimleri [Vm'leri ve veri diskleri için önbelleğe alma] [ dbms-guide-2.1] ve [Microsoft Azure depolama] [ dbms-guide-2.3] de Oracle Database dağıtımlar için geçerlidir.

Daha önce belgenin Genel bölümünde açıklandığı gibi Azure disklerin IOPS işleme kotalar mevcut. Tam kotalar VM türüne bağlı olarak kullanılır. Kotalarını ile VM türlerinin bir listesini bulunabilir [burada (Linux)] [ virtual-machines-sizes-linux] ve [burada (Windows)][virtual-machines-sizes-windows].

Desteklenen Azure VM türleri tanımlamak için SAP notuna bakın [1928533]

Disk başına geçerli IOPS kota gereksinimlerini karşılamadığı sürece, tek bir bağlı disk üzerindeki tüm veritabanı dosyaları depolamak mümkündür. 

Daha fazla IOPS gerekiyorsa, birden çok bağlı diskler üzerinde büyük bir mantıksal birim oluşturmak için LVM (mantıksal birim Yöneticisi) veya MDADM kullanmak için önerilir. Ayrıca bkz. bölüm [yazılım RAID] [ dbms-guide-2.2] bu belgenin. Bu yaklaşım, disk alanını yönetmek için yönetim yükünü basitleştirir ve el ile oluşturulmuş birden çok diskte dosyaları dağıtmak için gereken çabayı önler.

#### <a name="backup--restore"></a>Yedekleme / Geri Yükleme
Yedekleme / geri yükleme işlevi, SAP BR * Oracle için Araçlar, çıplak ve Hyper-V gibi aynı şekilde desteklenir. Oracle kurtarma Yöneticisi'ni (RMAN) da disk ve diskten geri yüklemek yedeklemeler için desteklenir.

#### <a name="high-availability"></a>Yüksek Kullanılabilirlik
Oracle Data Guard, yüksek kullanılabilirlik ve olağanüstü durum kurtarma amacıyla desteklenir. Ayrıntıları bulunabilir [bu] [ virtual-machines-windows-classic-configure-oracle-data-guard] belgeleri.

#### <a name="other"></a>Diğer
Azure kullanılabilirlik kümeleri veya SAP izleme gibi diğer tüm genel alanlar da Oracle veritabanı ile VM'lerin dağıtımlar için bu belgenin ilk üç bölümde açıklandığı gibi uygulayın.

## <a name="specifics-for-the-sap-maxdb-database-on-windows"></a>Windows üzerinde SAP MaxDB veritabanı özellikleri
### <a name="sap-maxdb-version-support"></a>SAP MaxDB sürüm desteği
Şu anda desteklediği SAP MaxDB sürüm 7,9 azure'da SAP NetWeaver tabanlı ürünleriyle kullanmak için SAP. SAP MaxDB sunucusu veya SAP NetWeaver tabanlı ürünleriyle kullanılacak JDBC ve ODBC sürücüleri için tüm güncelleştirmeleri yalnızca SAP Service Marketplace aracılığıyla sağlanan <https://support.sap.com/swdc>.
SAP NetWeaver SAP MaxDB üzerinde çalıştırma hakkında genel bilgiler bulunabilir <https://www.sap.com/community/topic/maxdb.html>.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-maxdb-dbms"></a>SAP MaxDB DBMS için desteklenen Microsoft Windows sürümleri ve Azure VM türleri
Azure üzerinde SAP MaxDB DBMS için desteklenen Microsoft Windows sürümü bulmak için bkz:

* [SAP ürünü kullanılabilirlik matris (PAM)][sap-pam]
* SAP notu [1928533]

Microsoft Windows 2012 R2 Microsoft Windows işletim sisteminin en yeni sürümü kullanmak için önerilir.

### <a name="available-sap-maxdb-documentation"></a>Kullanılabilir SAP MaxDB belgeleri
Aşağıdaki SAP Note SAP MaxDB belgeleri güncelleştirilmiş listesini bulabilirsiniz [767598]

### <a name="sap-maxdb-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Azure vm'lerde SAP yüklemeleri için SAP MaxDB yapılandırma yönergeleri
#### <a name="b48cfe3b-48e9-4f5b-a783-1d29155bd573"></a>Depolama yapılandırması
SAP MaxDB için Azure depolama en iyi uygulamaları izleyin bölümde bahsedilen genel öneriler [RDBMS dağıtım yapısı][dbms-guide-2].

> [!IMPORTANT]
> Diğer veritabanları gibi SAP MaxDB veri ve günlük dosyalarını da vardır. Bununla birlikte, SAP MaxDB terminolojisinde doğru "Birim" (değil "dosyası") bir terimdir. Örneğin, SAP MaxDB vardır verileri ve günlük birimler. Bu işletim sistemi disk birimleri ile karıştırmayın. 
> 
> 

Kısacası için gerekenler:

* Azure depolama hesapları kullanıyorsanız SAP MaxDB veri ve günlük birimlerini (yani, dosyaları) bulunduğu Azure depolama hesabı ayarlayın **yerel olarak yedekli depolama (LRS)** bölümde belirtildiği gibi [Microsoft Azure depolama][dbms-guide-2.3].
* Günlük birimleri (yani, dosyaları) için GÇ yolundan SAP MaxDB veri birimlerini (yani, dosyaları) için g/ç yolu ayırın. Bu, bir mantıksal diske yüklenebilmek için SAP MaxDB veri birimlerini (yani, dosyaları) olması ve başka bir mantıksal diske yüklenebilmek için SAP MaxDB günlük birimleri (yani, dosyaları) olması anlamına gelir.
* Bölümde açıklandığı gibi SAP MaxDB veri veya günlük birimleri için (yani, dosyaları) kullanıp ve Azure standart ya da Azure Premium depolama kullanmadığınıza bağlı olarak her disk için uygun önbelleğe alma türünü ayarlayın [Vm'leri ve veri diskleri için önbelleğe alma] [dbms-guide-2.1].
* Disk başına geçerli IOPS kota gereksinimlerini karşılamadığı sürece, tüm veri miktarları tek bir bağlı diskte depolar ve ayrıca başka bir tek bağlı disk üzerinde tüm veritabanı günlük birimleri depolama mümkündür.
* Daha fazla IOPS ve/veya boşluk gerekliyse, Microsoft penceresi depolama havuzları (yalnızca Microsoft Windows Server 2012'de kullanılabilir ve daha yüksek) veya Microsoft Windows 2008 R2 için Microsoft Windows şeritleme birden çok büyük bir mantıksal cihaz oluşturmak için kullanmak için önerilir bağlı diskler. Ayrıca bkz. bölüm [yazılım RAID] [ dbms-guide-2.2] bu belgenin. Bu yaklaşım, disk alanını yönetmek için yönetim yükünü basitleştirir ve el ile oluşturulmuş birden çok diskte dosyaları dağıtma çaba önler.
* En yüksek IOPS gereksinimleri için Azure Premium depolama, DS serisi ve GS serisi VM'ler üzerinde kullanılabilir olduğu kullanabilirsiniz.

![Azure Iaas VM için SAP MaxDB DBMS başvuru yapılandırması][dbms-guide-figure-600]

#### <a name="23c78d3b-ca5a-4e72-8a24-645d141a3f5d"></a>Yedekleme ve geri yükleme
SAP MaxDB Azure'a dağıtırken, yedekleme yönteminize gözden geçirmeniz gerekir. Sistem, üretken bir sistem değil olsa bile, SAP MaxDB tarafından barındırılan SAP veritabanı düzenli aralıklarla yedeklenmelidir. Azure depolama üç görüntü tutar beri bir yedekleme artık depolama hatası ve daha önemli operasyonel veya yönetim hatalarına karşı sisteminizi korumak açısından daha az önemlidir. Belirli bir noktaya kurtarma özellikleri sağlayarak mantıksal veya el ile hataları dengeleyebilirsiniz. böylece uygun yedekleme ve geri yükleme planı sürdürmek için birincil nedenidir. Bu nedenle hedefi ya da yedeklemeler veritabanını zamandaki belirli bir noktaya geri yüklemek veya var olan veritabanı kopyalayarak başka bir sistem kaynağını oluşturmak için Azure'da yedeklemelerini kullanın kullanmaktır. Örneğin, 2 katmanlı SAP yapılandırmasından aynı sistem bir 3 katmanlı sistem ayarı için bir yedekleme geri yükleyerek aktardığınız.

Yedekleme ve azure'da bir veritabanı geri yükleme'lu SAP notuna içinde listelenen SAP MaxDB belgeleri belgeleri biriyle açıklanan standart SAP MaxDB yedekleme/geri yükleme araçlarını kullanabilmeniz için şirket içi sistemler, çalıştığı gibi aynı şekilde çalışır [767598]. 

#### <a name="77cd2fbb-307e-4cbf-a65f-745553f72d2c"></a>Yedekleme ve geri yükleme için performans konuları
Tam kurtarma dağıtımlarındaki kaç birimleri paralel ve bu birimlerin verimi okunabilir üzerinde yedekleme ve geri yükleme performansı bağlıdır. Ayrıca, yedekleme sıkıştırma tarafından kullanılan CPU tüketimi önemli bir rol ile en çok sekiz adet CPU iş parçacıklarını Vm'lerde oynatabilirsiniz. Bu nedenle, bir kabul edilebilir:

* Daha az veritabanı araçları, daha düşük genel okuma verimini depolamak için kullanılan disk sayısı
* Daha küçük CPU sayısını, VM, daha ciddi yedekleme sıkıştırma etkisini iş parçacıkları
* Yedekleme için daha düşük aktarım hızı yazmak için daha az sayıda hedefle (Şerit dizinleri, diskler)

Yazılacak hedefleri sayısını artırmak için birlikte, büyük olasılıkla, gereksinimlerinize bağlı olarak kullanabileceğiniz iki seçenek vardır:

* Yedekleme için ayrı birim ayrılması
* Yedekleme hedef birim, Bölüştürülmüş bir disk birimi IOPS aktarım hızını iyileştirmek için üzerinde birden fazla bağlı disk bölümlemesi
* Ayrı bir adanmış mantıksal disk cihazları için sahip:
  * SAP MaxDB yedekleme birimleri (yani, dosyaları)
  * SAP MaxDB veri birimlerini (yani, dosyaları)
  * SAP MaxDB günlük birimleri (yani, dosyaları)

Bir birim üzerinde birden fazla bağlı disk bölümlemesi ele alınan önceki bölümde [yazılım RAID] [ dbms-guide-2.2] bu belgenin. 

#### <a name="f77c1436-9ad8-44fb-a331-8671342de818"></a>Diğer
Azure kullanılabilirlik kümeleri veya SAP izleme gibi diğer tüm genel alanlar, SAP MaxDB veritabanı VM'lerin dağıtımlar için bu belgenin ilk üç bölümde açıklandığı gibi de geçerlidir.
Diğer SAP MaxDB özgü ayarları Azure Vm'leri için saydamdır ve SAP notu içinde listelenen farklı belgelerinde açıklanan [767598] ve bu SAP notları:

* [826037] 
* [1139904]
* [1173395]

## <a name="specifics-for-sap-livecache-on-windows"></a>Windows üzerinde SAP liveCache için özellikleri
### <a name="sap-livecache-version-support"></a>SAP liveCache sürüm desteği
Azure sanal Makineler'de desteklenen SAP liveCache en düşük sürümü **SAP LC/LCAPPS 10.0 SP 25** dahil olmak üzere **liveCache 7.9.08.31** ve **LCA derleme 25**için kullanıma sunulan **EhP 2 SAP SCM 7.0** ve daha yüksek.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-livecache-dbms"></a>SAP liveCache DBMS için desteklenen Microsoft Windows sürümleri ve Azure VM türleri
Azure üzerinde SAP liveCache için desteklenen Microsoft Windows sürümü bulmak için bkz:

* [SAP ürünü kullanılabilirlik matris (PAM)][sap-pam]
* SAP notu [1928533]

Microsoft Windows Server işletim sisteminin en yeni sürümü kullanmak için önerilir. 

### <a name="sap-livecache-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP liveCache Azure sanal makinelerinde SAP yüklemeleri için yapılandırma yönergeleri
#### <a name="recommended-azure-vm-types"></a>Önerilen Azure VM türleri
SAP liveCache büyük hesaplamalar yapan bir uygulama olduğundan, RAM ve CPU hızı ve miktarı SAP liveCache performansı üzerinde önemli bir etkisi vardır. 

SAP tarafından desteklenen Azure VM türleri (SAP notu [1928533]), sanal Makineye ayrılan tüm sanal CPU kaynakları ayrılmış fiziksel CPU kaynakları hiper yönetici tarafından desteklenir. Herhangi bir açıdan (ve bu nedenle hiçbir CPU kaynak rekabetini) gerçekleşir.

Benzer şekilde, SAP tarafından desteklenen tüm Azure VM örneği türleri için VM belleği 100 (atlayarak taahhüdü), örneğin, açıdan fiziksel bellek - eşlenen % kullanılmadığıdır.

A serisinde % 60 daha hızlı işlemcilere sahip oldukları gibi bu açısından bakıldığında, yeni D serisi veya DS serisi (Azure Premium depolama ile birlikte), Azure VM türü kullanmak için önerilir. En yüksek RAM ve CPU yükü için G serisi ve GS serisi (Azure Premium depolama ile birlikte), VM'ler ile en son Intel kullanabilirsiniz?? Xeon?? İşlemci E5 v3 ailesini iki kez bellek ve dört katı hal sürücü depolaması (SSD'ler) D/DS serisi zaman.

#### <a name="storage-configuration"></a>Depolama Yapılandırması
SAP liveCache SAP MaxDB teknolojisini temel alan gibi tüm Azure depolama en iyi yöntem önerileri için SAP MaxDB bölümde bahsedilen [depolama yapılandırması] [ dbms-guide-8.4.1] SAP liveCache için geçerlidir. 

#### <a name="dedicated-azure-vm-for-livecache"></a>LiveCache için adanmış bir Azure VM
SAP liveCache yoðun işlem gücünü kullandığından, verimli kullanım için ayrılmış olan bir Azure sanal makine dağıtmak için önerilir. 

![Azure VM için liveCache verimli bir kullanım örneği için ayrılmış][dbms-guide-figure-700]

#### <a name="backup-and-restore"></a>Yedekleme ve Geri Yükleme
Yedekleme ve geri yükleme, performans konuları dahil olmak üzere ilgili SAP MaxDB bölümlerde açıklanmıştır zaten [yedekleme ve geri yükleme] [ dbms-guide-8.4.2] ve [yedekleme için performans konuları ve geri yükleme][dbms-guide-8.4.3]. 

#### <a name="other"></a>Diğer
Diğer tüm genel alanlar zaten ilgili SAP MaxDB içinde açıklanan [bu] [ dbms-guide-8.4.4] bölüm. 

## <a name="specifics-for-the-sap-content-server-on-windows"></a>Windows üzerinde SAP içerik sunucusu özellikleri
SAP içerik sunucusu, elektronik belge gibi içerikler farklı biçimlerde depolamak için ayrı, sunucu tabanlı bir bileşendir. SAP içerik sunucusu geliştirme teknoloji tarafından sağlanır ve tüm SAP uygulamaları için kullanılan çapraz uygulama olacak. Ayrı bir sistemde yüklü. Eğitim malzemeleri ve Bilgi Bankası ambarı veya PLM belge yönetim sistemi mySAP kaynaklanan teknik çizimler belgelerinden bunun normal içeriktir. 

### <a name="sap-content-server-version-support"></a>SAP içerik sunucusu sürüm desteği
Şu anda SAP destekler:

* **SAP içerik sunucusu** sürümüyle **6.50 (ve üzeri)**
* **SAP MaxDB 7,9 sürümü**
* **Microsoft IIS (Internet Information Server) sürüm 8.0 (ve üzeri)**

Olan bu belge yazma zamanında SAP içerik sunucusu, en yeni sürümü kullanmak için önemle tavsiye edilir **6.50 SP4**ve en yeni sürümünü **Microsoft IIS 8.5**. 

Desteklenen en son sürümlerini SAP içerik sunucusu ve Microsoft IIS denetleyin [SAP ürünü kullanılabilirlik matris (PAM)][sap-pam].

### <a name="supported-microsoft-windows-and-azure-vm-types-for-sap-content-server"></a>SAP içerik sunucusu için desteklenen Microsoft Windows ile Azure VM türleri
Azure üzerinde SAP içerik sunucusu için desteklenen Windows sürümü bulmak için bkz:

* [SAP ürünü kullanılabilirlik matris (PAM)][sap-pam]
* SAP notu [1928533]

Microsoft Windows Server'ın en yeni sürümü kullanmak için önerilir.

### <a name="sap-content-server-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Azure vm'lerde SAP yüklemeleri için SAP içerik sunucusu yapılandırma yönergeleri
#### <a name="storage-configuration"></a>Depolama Yapılandırması
SAP MaxDB veritabanında depolamak için SAP içerik sunucusu yapılandırırsanız, öneri için SAP MaxDB bölümde bahsedilen tüm Azure depolama en iyi yöntemler [depolama yapılandırması] [ dbms-guide-8.4.1] da geçerlidir SAP içerik sunucusu senaryosu için. 

Dosya sistemindeki dosyaları depolamak için SAP içerik sunucusu yapılandırırsanız, adanmış bir mantıksal sürücü kullanmak için önerilir. Windows depolama alanları kullanarak mantıksal diskin boyut ve IOPS işleme da artırabilirsiniz olanak tanır bölümde açıklandığı gibi [yazılım RAID][dbms-guide-2.2]. 

#### <a name="sap-content-server-location"></a>SAP içerik sunucusu konumu
SAP içerik sunucusu aynı Azure bölgesindeki ve SAP sistemine dağıtıldığı Azure VNET dağıtılması gerekir. Adanmış bir Azure sanal makinesinde veya SAP sistemine çalıştığı aynı VM'de SAP içerik sunucusu bileşenlerini dağıtmak istediğinize karar verin ücretsizdir. 

![Azure VM'de SAP içerik sunucusu için ayrılmış][dbms-guide-figure-800]

#### <a name="sap-cache-server-location"></a>SAP önbellek sunucu konumu
SAP önbellek sunucusuna yerel olarak (önbelleğe alınmış) belgelere erişimi sağlamak için bir ek sunucu tabanlı bileşendir. SAP önbellek sunucusu, SAP içerik sunucusu belgeleri önbelleğe alır. Bu belgeler farklı konumlarda birden çok kez alınacak varsa ağ trafiğini en iyi duruma getirmek için yapılır. Genel kural SAP önbellek sunucusu SAP önbellek sunucusuna erişen istemci yakın fiziksel olarak sahip olur. 

Burada iki seçeneğiniz vardır:

1. **İstemci, bir arka uç SAP sistemidir** SAP içerik sunucusuna erişmek için bir arka uç SAP sistemine yapılandırdıysanız, bu SAP sistemine bir istemcidir. Aynı Azure bölgesinde, aynı Azure veri merkezinde hem SAP sistemi hem de SAP içerik sunucusu dağıtılırken fiziksel olarak birbirlerine yakın oldukları. Bu nedenle, ayrılmış bir SAP önbellek sunucusu olmasını gerek yoktur. SAP UI istemcileri (SAP GUI veya web tarayıcısı) doğrudan SAP sistemine erişim ve SAP sistemine SAP içerik sunucusundan belgelerini alır.
2. **İstemci, bir şirket içi web tarayıcısı olan** SAP içerik sunucusu, doğrudan web tarayıcısı tarafından erişilecek şekilde yapılandırılabilir. Bu durumda, şirket içinde çalışan bir web tarayıcısı olan SAP içeriği sunucusunun bir istemcidir. Şirket içi veri merkezi ve Azure veri merkezi (İdeal olarak yakın olması birbirleriyle) farklı fiziksel konumlarda yerleştirilir. Şirket içi veri merkezinizi Azure siteden siteye VPN veya ExpressRoute aracılığıyla azure'a bağlı. İki seçenek de Azure'da güvenli bir VPN ağ bağlantısı sunmasına rağmen siteden siteye ağ bağlantısı şirket içi veri merkezi ve Azure veri merkezi arasında ağ bant genişliği ve gecikme SLA sağlamaz. Belgeye erişimi hızlandırmak için aşağıdakilerden birini yapın:
   1. Şirket içi SAP önbellek sunucusu yüklemek, kapatmak için şirket içi web tarayıcısında (seçeneğini [bu] [ dbms-guide-900-sap-cache-server-on-premises] Şekil)
   2. Azure şirket içi veri merkeziniz ile Azure veri merkezi arasında yüksek hızlı ve düşük gecikme süreli Adanmış ağ bağlantısı sunan ExpressRoute yapılandırın.

![Şirket içi SAP önbellek sunucusu yüklemek için seçeneği][dbms-guide-figure-900]
<a name="642f746c-e4d4-489d-bf63-73e80177a0a8"></a>

#### <a name="backup--restore"></a>Yedekleme / Geri Yükleme
SAP MaxDB veritabanında depolamak için SAP içerik sunucusu yapılandırırsanız, yedekleme/geri yükleme yordamı ve performans konuları zaten SAP MaxDB bölümde açıklanan [yedekleme ve geri yükleme] [ dbms-guide-8.4.2]ve bölüm [yedekleme ve geri yükleme için başarım düşünceleri][dbms-guide-8.4.3]. 

Dosya sistemindeki dosyaları depolamak için SAP içerik sunucusu yapılandırırsanız, bir el ile yedekleme/geri yükleme tam dosya yapısı yürütmek için belgelerin bulunduğu yere seçenektir. Benzer şekilde, SAP MaxDB yedekleme/geri yükleme, bu yedekleme bir amaç için adanmış bir disk birimi sağlamak için önerilir. 

#### <a name="other"></a>Diğer
Diğer SAP içerik sunucusuna özgü ayarları, Azure Vm'leri için saydamdır ve çeşitli belgeleri ve SAP notları açıklanmıştır:

* <https://service.sap.com/contentserver> 
* SAP notu [1619726]  

## <a name="specifics-to-ibm-db2-for-luw-on-windows"></a>Windows üzerinde LUW için IBM DB2'ye özellikleri
Microsoft Azure ile IBM DB2 üzerinde Linux, UNIX ve Windows (LUW) için Azure sanal makineleri çalıştıran mevcut SAP uygulamanızı kolayca geçiş yapabilirsiniz. IBM DB2 LUW için üzerinde SAP ile yöneticiler ve geliştiriciler aynı geliştirme ve şirket içi kullanılabilir olan yönetim araçları kullanmaya devam edebilirsiniz.
SAP Business Suite, IBM DB2 üzerinde çalışan LUW, SAP topluluk ağ (SCN) içinde bulunabilir hakkında genel bilgi <https://www.sap.com/community/topic/db2-for-linux-unix-and-windows.html>.

Ek bilgi ve azure'da LUW için DB2 üzerinde SAP hakkında güncelleştirmeler için bkz. Not SAP [2233094]. 

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 Linux, UNIX ve Windows sürüm desteği
IBM DB2 için Microsoft Azure sanal makine Hizmetleri LUW üzerinde SAP 10.5 DB2 sürümden itibaren desteklenir.

Desteklenen SAP ürünlerini ve Azure VM türleri hakkında daha fazla bilgi için SAP notuna bakın [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Linux, UNIX ve Windows Azure vm'lerde SAP yüklemeleri için yapılandırma yönergeleri için IBM DB2
#### <a name="storage-configuration"></a>Depolama Yapılandırması
Tüm veritabanı dosyaları doğrudan bağlı diskleri temel alan NTFS dosya sisteminde depolanmış olması gerekir. Bu diskler Azure VM'sine bağlanmış ve Azure sayfa BLOB Storage'da temel alır (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) veya yönetilen diskler (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Her türlü ağ sürücülerine veya aşağıdaki Azure Dosya Hizmetleri gibi uzak paylaşımları **değil** veritabanı dosyaları için desteklenir: 

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx>

Azure sayfa BLOB Depolama veya yönetilen diskleri temel alan diskler kullanıyorsanız, ifadeler bölümde bu belgedeki [RDBMS dağıtım yapısı] [ dbms-guide-2] LUW için IBM DB2 dağıtımlar için de geçerlidir. Veritabanı. 

Daha önce belgenin Genel bölümünde açıklandığı gibi disklerin IOPS işleme kotalar mevcut. Tam kotalar kullanılan VM türüne bağlıdır. Kotalarını ile VM türlerinin bir listesini bulunabilir [burada (Linux)] [ virtual-machines-sizes-linux] ve [burada (Windows)][virtual-machines-sizes-windows].

Disk başına geçerli IOPS kota yeterli olduğu sürece, tek bir bağlı disk üzerindeki tüm veritabanı dosyaları depolamak mümkündür. 

İçin performans konuları da bölüm 'Veri güvenliği ve veritabanı dizinleri performans değerlendirmeleri' SAP yükleme kılavuzlarını içinde bakın.

Alternatif olarak, Windows depolama havuzları (yalnızca Windows Server 2012'de kullanılabilir ve üzeri) kullanabilir veya Windows 2008 R2 Windows şeritleme bölümde açıklanan [yazılım RAID] [ dbms-guide-2.2] için bu belgenin büyük bir mantıksal cihaz üzerinde birden çok disk oluşturun.
Sapdata ve saptmp dizinlerinizi DB2 depolama yollarını içeren diskler için bir fiziksel disk sektör boyutu 512 KB belirtmeniz gerekir. Windows depolama havuzlarını kullanırken depolama havuzları parametresini kullanarak komut satırı arabirimi aracılığıyla el ile oluşturmanız gerekir `-LogicalSectorSizeDefault`. Daha fazla bilgi için <https://technet.microsoft.com/itpro/powershell/windows/storage/new-storagepool>.

#### <a name="backuprestore"></a>Yedekleme/Geri Yükleme
Yedekleme/geri yükleme işlevlerini LUW için IBM DB2 için standart Windows Server işletim sistemleri ve Hyper-V üzerinde aynı şekilde desteklenir.

Geçerli bir veritabanı yedekleme stratejisi mevcut olduğundan emin olmanız gerekir. 

Tam kurtarma dağıtımlarındaki yedekleme/geri yükleme performansı kaç birim paralel olarak okunması ve hangi bu birimlerin verimi olabilir bağlıdır. Yedekleme sıkıştırma tarafından kullanılan CPU tüketimini Vm'lerde ile en çok sekiz adet CPU iş parçacıklarını Ayrıca, önemli bir rol oynayabilir. Bu nedenle, bir kabul edilebilir:

* Daha az veritabanı cihazları depolamak için kullanılan disk sayısını daha küçük genel üretilen işi okuma
* Daha küçük CPU sayısını, VM, daha ciddi yedekleme sıkıştırma etkisini iş parçacıkları
* Yedekleme için daha düşük aktarım hızı yazmak için daha az sayıda hedefle (Şerit dizinleri, diskler)

Yazılacak hedefleri sayısını artırmak için iki seçenek, gereksinimlerinize bağlı olarak kullanılan/birleşik olabilir:

* Şeritli birim IOPS aktarım hızını iyileştirmek için birden çok disk üzerinde Yedekleme hedef birimin bölümlemesi
* Yedeğe yazmak için birden fazla hedef dizin'ı kullanma

#### <a name="high-availability-and-disaster-recovery"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma
Microsoft Küme sunucusu (MSCS) desteklenmiyor.

DB2 yüksek kullanılabilirlik, olağanüstü durum kurtarma (HADR) desteklenir. Ad çözümlemesinin çalışıp HA yapılandırmasının sanal makineler varsa, Kurulum azure'da şirket içinde gerçekleştirilen herhangi bir ayar farklı değildir. Yalnızca IP çözünürlüğüne yararlanmayı önerilmez.

Coğrafi çoğaltma, veritabanı disklerini depolamak için depolama hesapları kullanmayın. Daha fazla bilgi için bölüme başvurun [Microsoft Azure depolama] [ dbms-guide-2.3] ve bölüm [yüksek kullanılabilirlik ve olağanüstü durum kurtarma Azure sanal makineleri ile] [ dbms-guide-3].

#### <a name="other"></a>Diğer
Azure kullanılabilirlik kümeleri veya SAP izleme gibi diğer tüm genel alanlar LUW için IBM DB2 Vm'lerle de dağıtımları için bu belgenin ilk üç bölümde açıklandığı gibi uygulayın. 

Ayrıca bir bölüme başvurmak [Genel SQL Server SAP on Azure özeti][dbms-guide-5.8].

## <a name="specifics-to-ibm-db2-for-luw-on-linux"></a>Linux üzerinde LUW için IBM DB2'ye özellikleri
Microsoft Azure ile IBM DB2 üzerinde Linux, UNIX ve Windows (LUW) için Azure sanal makineleri çalıştıran mevcut SAP uygulamanızı kolayca geçiş yapabilirsiniz. IBM DB2 LUW için üzerinde SAP ile yöneticiler ve geliştiriciler aynı geliştirme ve şirket içi kullanılabilir olan yönetim araçları kullanmaya devam edebilirsiniz. SAP Business Suite, IBM DB2 üzerinde çalışan LUW, SAP topluluk ağ (SCN) içinde bulunabilir hakkında genel bilgi <https://www.sap.com/community/topic/db2-for-linux-unix-and-windows.html>.

Ek bilgi ve azure'da LUW için DB2 üzerinde SAP hakkında güncelleştirmeler için bkz. Not SAP [2233094].

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 Linux, UNIX ve Windows sürüm desteği
IBM DB2 için Microsoft Azure sanal makine Hizmetleri LUW üzerinde SAP 10.5 DB2 sürümden itibaren desteklenir.

Desteklenen SAP ürünlerini ve Azure VM türleri hakkında daha fazla bilgi için SAP notuna bakın [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Linux, UNIX ve Windows Azure vm'lerde SAP yüklemeleri için yapılandırma yönergeleri için IBM DB2
#### <a name="storage-configuration"></a>Depolama Yapılandırması
Tüm veritabanı dosyaları doğrudan bağlı diskleri temel alan bir dosya sisteminde depolanmış olması gerekir. Bu diskler Azure VM'sine bağlanmış ve Azure sayfa BLOB Storage'da temel alır (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) veya yönetilen diskler (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Her türlü ağ sürücülerine veya aşağıdaki Azure Dosya Hizmetleri gibi uzak paylaşımları **değil** veritabanı dosyaları için desteklenir:

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx>

Azure sayfa BLOB Depolama, bölüm bu belgede yapılan deyimleri temel diskleri kullanıyorsanız [RDBMS dağıtım yapısı] [ dbms-guide-2] LUW veritabanı için IBM DB2 dağıtımlar için de geçerlidir.

Daha önce belgenin Genel bölümünde açıklandığı gibi disklerin IOPS işleme kotalar mevcut. Tam kotalar kullanılan VM türüne bağlıdır. Kotalarını ile VM türlerinin bir listesini bulunabilir [burada (Linux)] [ virtual-machines-sizes-linux] ve [burada (Windows)][virtual-machines-sizes-windows].

Disk başına geçerli IOPS kota yeterli olduğu sürece, tüm veritabanı dosyaları tek bir diske depolamak mümkündür.

İçin performans konuları da bölüm 'Veri güvenliği ve veritabanı dizinleri performans değerlendirmeleri' SAP yükleme kılavuzlarını içinde bakın.

Alternatif olarak, LVM (mantıksal birim Yöneticisi) veya MDADM bölümde açıklandığı gibi kullanabileceğiniz [yazılım RAID] [ dbms-guide-2.2] büyük bir mantıksal cihaz üzerinde birden fazla disk oluşturmak için bu belgenin.
Sapdata ve saptmp dizinlerinizi DB2 depolama yollarını içeren diskler için bir fiziksel disk sektör boyutu 512 KB belirtmeniz gerekir.

#### <a name="backuprestore"></a>Yedekleme/Geri Yükleme
Yedekleme/geri yükleme işlevlerini LUW için IBM DB2 için standart Linux yükleme şirket içi aynı şekilde desteklenir.

Geçerli bir veritabanı yedekleme stratejisi mevcut olduğundan emin olmanız gerekir.

Tam kurtarma dağıtımlarındaki yedekleme/geri yükleme performansı kaç birim paralel olarak okunması ve hangi bu birimlerin verimi olabilir bağlıdır. Yedekleme sıkıştırma tarafından kullanılan CPU tüketimini Vm'lerde ile en çok sekiz adet CPU iş parçacıklarını Ayrıca, önemli bir rol oynayabilir. Bu nedenle, bir kabul edilebilir:

* Daha az veritabanı cihazları depolamak için kullanılan disk sayısını daha küçük genel üretilen işi okuma
* Daha küçük CPU sayısını, VM, daha ciddi yedekleme sıkıştırma etkisini iş parçacıkları
* Yedekleme için daha düşük aktarım hızı yazmak için daha az sayıda hedefle (Şerit dizinleri, diskler)

Yazılacak hedefleri sayısını artırmak için iki seçenek, gereksinimlerinize bağlı olarak kullanılan/birleşik olabilir:

* Şeritli birim IOPS aktarım hızını iyileştirmek için birden çok disk üzerinde Yedekleme hedef birimin bölümlemesi
* Yedeğe yazmak için birden fazla hedef dizin'ı kullanma

#### <a name="high-availability-and-disaster-recovery"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma
DB2 yüksek kullanılabilirlik, olağanüstü durum kurtarma (HADR) desteklenir. Ad çözümlemesinin çalışıp HA yapılandırmasının sanal makineler varsa, Kurulum azure'da şirket içinde gerçekleştirilen herhangi bir ayar farklı değildir. Yalnızca IP çözünürlüğüne yararlanmayı önerilmez.

Coğrafi çoğaltma, veritabanı disklerini depolamak için depolama hesapları kullanmayın. Daha fazla bilgi için bölüme başvurun [Microsoft Azure depolama] [ dbms-guide-2.3] ve bölüm [yüksek kullanılabilirlik ve olağanüstü durum kurtarma Azure sanal makineleri ile] [ dbms-guide-3].

#### <a name="other"></a>Diğer
Azure kullanılabilirlik kümeleri veya SAP izleme gibi tüm diğer genel konular LUW için IBM DB2 Vm'lerle de dağıtımları için bu belgenin ilk üç bölümde açıklandığı gibi geçerlidir.

Ayrıca bir bölüme başvurmak [Genel SQL Server SAP on Azure özeti][dbms-guide-5.8].

