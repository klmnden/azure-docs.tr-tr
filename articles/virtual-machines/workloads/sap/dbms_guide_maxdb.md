---
title: MaxDB liveCache ve içerik sunucusu dağıtım Azure vm'lerde SAP | Microsoft Docs
description: SAP MaxDB liveCache ve azure'da içerik sunucusu dağıtımı
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
ms.date: 07/12/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 83319118c778d89749b1eb5d5fd792a5200c19c5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60836067"
---
# <a name="sap-maxdb-livecache-and-content-server-deployment-on-azure-vms"></a>SAP MaxDB liveCache ve Azure Vm'leri üzerinde içerik sunucusu dağıtımı

[767598]: https://launchpad.support.sap.com/#/notes/767598
[773830]:https://launchpad.support.sap.com/#/notes/773830
[826037]: https://launchpad.support.sap.com/#/notes/826037
[965908]:https://launchpad.support.sap.com/#/notes/965908
[1031096]:https://launchpad.support.sap.com/#/notes/1031096
[1114181]:https://launchpad.support.sap.com/#/notes/1114181
[1139904]: https://launchpad.support.sap.com/#/notes/1139904
[1173395]: https://launchpad.support.sap.com/#/notes/1173395
[1245200]:https://launchpad.support.sap.com/#/notes/1245200
[1409604]:https://launchpad.support.sap.com/#/notes/1409604
[1558958]:https://launchpad.support.sap.com/#/notes/1558958
[1585981]:https://launchpad.support.sap.com/#/notes/1585981
[1588316]:https://launchpad.support.sap.com/#/notes/1588316
[1590719]:https://launchpad.support.sap.com/#/notes/1590719
[1597355]:https://launchpad.support.sap.com/#/notes/1597355
[1605680]:https://launchpad.support.sap.com/#/notes/1605680
[1619720]:https://launchpad.support.sap.com/#/notes/1619720
[1619726]: https://launchpad.support.sap.com/#/notes/1619726
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
[1928533]: https://launchpad.support.sap.com/#/notes/1928533
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
[dbms-guide-figure-900]:media/dbms_maxdb_deployment_guide/900-sap-cache-server-on-premises.png

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




Bu belge MaxDB liveCache ve Azure Iaas içerik sunucusu dağıtırken göz önünde bulundurulacak birkaç farklı alanlara kapsar. Bu belge için bir önkoşul belge okuma [SAP iş yükü Azure sanal makineleri DBMS dağıtım konuları](dbms_guide_general.md) diğer yönergelerinde yanı sıra [AzureBelgeleri'ndeSAPişyükü](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started). 

## <a name="specifics-for-the-sap-maxdb-deployments-on-windows"></a>Windows üzerinde SAP MaxDB dağıtımlar için özellikleri
### <a name="sap-maxdb-version-support-on-azure"></a>Azure üzerinde SAP MaxDB sürüm desteği
SAP, SAP MaxDB 7.9 veya üzeri bir sürümü şu anda azure'da SAP NetWeaver tabanlı ürünleriyle kullanmak için destekler. SAP MaxDB sunucusu veya SAP NetWeaver tabanlı ürünleriyle kullanılacak JDBC ve ODBC sürücüleri için tüm güncelleştirmeleri yalnızca SAP Service Marketplace aracılığıyla sağlanan <https://support.sap.com/swdc>.
SAP NetWeaver SAP MaxDB üzerinde çalıştırma hakkında genel bilgiler bulunabilir <https://www.sap.com/community/topic/maxdb.html>.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-maxdb-dbms"></a>SAP MaxDB DBMS için desteklenen Microsoft Windows sürümleri ve Azure VM türleri
Azure üzerinde SAP MaxDB DBMS için desteklenen Microsoft Windows sürümü bulmak için bkz:

* [SAP ürünü kullanılabilirlik matris (PAM)][sap-pam]
* SAP notu [1928533]

Microsoft Windows 2016 Microsoft Windows işletim sisteminin en yeni sürümü kullanmak için önerilir.

### <a name="available-sap-maxdb-documentation-for-maxdb"></a>MaxDB kullanılabilir SAP MaxDB belgeleri
Aşağıdaki SAP Note SAP MaxDB belgeleri güncelleştirilmiş listesini bulabilirsiniz [767598]

### <a name="sap-maxdb-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Azure vm'lerde SAP yüklemeleri için SAP MaxDB yapılandırma yönergeleri
#### <a name="b48cfe3b-48e9-4f5b-a783-1d29155bd573"></a>Depolama yapılandırması
SAP MaxDB için Azure depolama en iyi uygulamaları izleyin bölümde bahsedilen genel öneriler [RDBMS dağıtımlar için bir VM depolama yapısını](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general#65fa79d6-a85f-47ee-890b-22e794f51a64).

> [!IMPORTANT]
> Diğer veritabanları gibi SAP MaxDB veri ve günlük dosyalarını da vardır. Bununla birlikte, SAP MaxDB terminolojisinde doğru "Birim" (değil "dosyası") bir terimdir. Örneğin, SAP MaxDB vardır verileri ve günlük birimler. Bu işletim sistemi disk birimleri ile karıştırmayın. 
> 
> 

Kısacası için gerekenler:

* Azure depolama hesapları kullanıyorsanız SAP MaxDB veri ve günlük birimlerini (veri ve günlük dosyaları) bulunduğu Azure depolama hesabı ayarlayın **yerel olarak yedekli depolama (LRS)** belirtilmiş [konuları için Azure sanal Makineleri SAP iş yükü için DBMS dağıtım](dbms_guide_general.md).
* Günlük birimleri (günlük dosyaları) için GÇ yolundan SAP MaxDB veri birimlerini (veri dosyaları) için g/ç yolu ayırın. Bir mantıksal diske yüklenebilmek için SAP MaxDB veri birimlerini (veri dosyaları) olması ve başka bir mantıksal diske yüklenebilmek için SAP MaxDB günlük birimleri (günlük dosyaları) olması anlamına gelir.
* Bölümünde anlatıldığı gibi SAP MaxDB veri veya günlük birimleri için (veri ve günlük dosyaları) kullanıp ve Azure standart ya da Azure Premium depolama kullanmadığınıza bağlı olarak her disk için uygun önbelleğe alma türünü ayarlayın [Azure sanal makineleri DBMS dikkate alınacak noktalar SAP iş yükü için dağıtım](dbms_guide_general.md).
* Disk başına geçerli IOPS kota gereksinimlerini karşılamadığı sürece, tüm veri miktarları tek bir bağlı diskte depolar ve ayrıca başka bir tek bağlı disk üzerinde tüm veritabanı günlük birimleri depolama mümkündür.
* Daha fazla IOPS ve/veya boşluk gerekliyse, büyük bir mantıksal cihaz üzerinde birden fazla bağlı disk oluşturmak için Microsoft penceresi depolama havuzları (yalnızca Microsoft Windows Server 2012'de kullanılabilir ve üzeri) kullanmak önerilir. Daha fazla ayrıntı için Ayrıca bkz: [SAP iş yükü Azure sanal makineleri DBMS dağıtım konuları](dbms_guide_general.md). Bu yaklaşım, disk alanını yönetmek için yönetim yükünü basitleştirir ve el ile oluşturulmuş birden çok diskte dosyaları dağıtma çaba önler.
* Azure Premium depolama MaxDB dağıtımları için kullanmak üzere önemle tavsiye edilir. 

![Azure Iaas VM için SAP MaxDB DBMS başvuru yapılandırması](./media/dbms_maxdb_deployment_guide/Simple_disk_structure_maxdb.PNG)


#### <a name="23c78d3b-ca5a-4e72-8a24-645d141a3f5d"></a>Yedekleme ve geri yükleme
SAP MaxDB Azure'a dağıtırken, yedekleme yönteminize gözden geçirmeniz gerekir. Sistem, üretken bir sistem değil olsa bile, SAP MaxDB tarafından barındırılan SAP veritabanı düzenli aralıklarla yedeklenmelidir. Azure depolama üç görüntü tutar beri bir yedekleme artık depolama hatası ve daha önemli operasyonel veya yönetim hatalarına karşı sisteminizi korumak açısından daha az önemlidir. Belirli bir noktaya kurtarma özellikleri sağlayarak mantıksal veya el ile hataları dengeleyebilirsiniz. böylece uygun yedekleme ve geri yükleme planı sürdürmek için birincil nedenidir. Bu nedenle hedefi ya da yedeklemeler veritabanını zamandaki belirli bir noktaya geri yüklemek veya var olan veritabanı kopyalayarak başka bir sistem kaynağını oluşturmak için Azure'da yedeklemelerini kullanın kullanmaktır. 

Yedekleme ve azure'da bir veritabanı geri yükleme'lu SAP notuna içinde listelenen SAP MaxDB belgeleri belgeleri biriyle açıklanan standart SAP MaxDB yedekleme/geri yükleme araçlarını kullanabilmeniz için şirket içi sistemler, çalıştığı gibi aynı şekilde çalışır [767598]. 

#### <a name="77cd2fbb-307e-4cbf-a65f-745553f72d2c"></a>Yedekleme ve geri yükleme için performans konuları
Çıplak dağıtımlar olduğu gibi yedekleme ve geri yükleme performansı kaç birimleri paralel ve bu birimlerin verimi okunabilir üzerinde bağımlı. Bu nedenle, bir kabul edilebilir:

* Daha az veritabanı araçları, daha düşük genel okuma verimini depolamak için kullanılan disk sayısı
* Yedekleme için daha düşük aktarım hızı yazmak için daha az sayıda hedefle (Şerit dizinleri, diskler)

Yazılacak hedefleri sayısını artırmak için birlikte, büyük olasılıkla, gereksinimlerinize bağlı olarak kullanabileceğiniz iki seçenek vardır:

* Yedekleme için ayrı birim ayrılması
* Yedekleme hedef birim, Bölüştürülmüş bir disk birimi IOPS aktarım hızını iyileştirmek için üzerinde birden fazla bağlı disk bölümlemesi
* Ayrı bir adanmış mantıksal disk cihazları için sahip:
  * SAP MaxDB yedekleme birimleri (yani, dosyaları)
  * SAP MaxDB veri birimlerini (yani, dosyaları)
  * SAP MaxDB günlük birimleri (yani, dosyaları)

Bir birim üzerinde birden fazla bağlı disk bölümlemesi ele daha önce [SAP iş yükü Azure sanal makineleri DBMS dağıtım konuları](dbms_guide_general.md). 

#### <a name="f77c1436-9ad8-44fb-a331-8671342de818"></a>Dikkat edilecek diğer noktalar
Azure kullanılabilirlik kümeleri veya SAP izleme gibi diğer tüm genel alanlar da açıklandığı gibi geçerli [SAP iş yükü Azure sanal makineleri DBMS dağıtım konuları](dbms_guide_general.md).  SAP MaxDB veritabanı ile VM'lerin'dağıtımları için.
Diğer SAP MaxDB özgü ayarları Azure Vm'leri için saydamdır ve SAP notu içinde listelenen farklı belgelerinde açıklanan [767598] ve bu SAP notları:

* [826037] 
* [1139904]
* [1173395]

## <a name="specifics-for-sap-livecache-deployments-on-windows"></a>Windows üzerinde SAP liveCache dağıtımlar için özellikleri
### <a name="sap-livecache-version-support"></a>SAP liveCache sürüm desteği
Azure sanal Makineler'de desteklenen SAP liveCache en düşük sürümü **SAP LC/LCAPPS 10.0 SP 25** dahil olmak üzere **liveCache 7.9.08.31** ve **LCA derleme 25**için kullanıma sunulan **EhP 2 SAP SCM 7.0** ve sonraki sürümleri.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-livecache-dbms"></a>SAP liveCache DBMS için desteklenen Microsoft Windows sürümleri ve Azure VM türleri
Azure üzerinde SAP liveCache için desteklenen Microsoft Windows sürümü bulmak için bkz:

* [SAP ürünü kullanılabilirlik matris (PAM)][sap-pam]
* SAP notu [1928533]

Microsoft Windows Server işletim sisteminin en yeni sürümü kullanmak için önerilir. 

### <a name="sap-livecache-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP liveCache Azure sanal makinelerinde SAP yüklemeleri için yapılandırma yönergeleri
#### <a name="recommended-azure-vm-types-for-livecache"></a>Azure VM türleri liveCache için önerilen
SAP liveCache büyük hesaplamalar yapan bir uygulama olduğundan, RAM ve CPU hızı ve miktarı SAP liveCache performansı üzerinde önemli bir etkisi vardır. 

SAP tarafından desteklenen Azure VM türleri (SAP notu [1928533]), sanal Makineye ayrılan tüm sanal CPU kaynakları ayrılmış fiziksel CPU kaynakları hiper yönetici tarafından desteklenir. Herhangi bir açıdan (ve bu nedenle hiçbir CPU kaynak rekabetini) gerçekleşir.

Benzer şekilde, SAP tarafından desteklenen tüm Azure VM örneği türleri için VM belleği 100 (atlayarak taahhüdü), örneğin, fazladan sağlama fiziksel bellek - eşlenen % kullanılmadığıdır.

Bu açısından bakıldığında, en son Dv2, Dv3, Ev3 ve M serisi VM'ler kullanılacak önemle tavsiye edilir. Farklı VM türleri seçimi liveCache ve ihtiyacınız olan CPU kaynakları için ihtiyacınız olan bellek bağlıdır. Olarak diğer tüm DBMS dağıtımları ile Azure Premium depolama performansı için kritik birimleri yararlanmak için tavsiye edilir.

#### <a name="storage-configuration-for-livecache-in-azure"></a>Azure'da liveCache için depolama yapılandırması
SAP liveCache SAP MaxDB teknolojisini temel alan gibi SAP bu belgede açıklanan MaxDB için belirtilen tüm Azure depolama en iyi yöntem önerileri SAP liveCache için de geçerlidir. 

#### <a name="dedicated-azure-vm-for-livecache-scenario"></a>LiveCache senaryosu için adanmış bir Azure VM
SAP liveCache yoðun işlem gücünü kullandığından, verimli kullanım için ayrılmış olan bir Azure sanal makine dağıtmak için önerilir. 

![Azure VM için liveCache verimli bir kullanım örneği için ayrılmış](./media/dbms_maxdb_deployment_guide/700-livecach-prod.PNG)


#### <a name="backup-and-restore-for-livecache-in-azure"></a>Yedekleme ve geri yükleme için azure'da liveCache
Yedekleme ve geri yükleme, performans değerlendirmeleri gibi zaten bu belgedeki ilgili SAP MaxDB bölümlerde açıklanmıştır. 

#### <a name="other-considerations"></a>Dikkat edilecek diğer noktalar
Diğer tüm genel alanlar zaten ilgili SAP MaxDB bölümde açıklanmıştır. 

## <a name="specifics-for-the-sap-content-server-deployment-on-windows-in-azure"></a>Windows Azure üzerinde SAP içerik sunucusu dağıtımı için özellikleri
SAP içerik sunucusu, elektronik belge gibi içerikler farklı biçimlerde depolamak için ayrı, sunucu tabanlı bir bileşendir. SAP içerik sunucusu geliştirme teknoloji tarafından sağlanır ve tüm SAP uygulamaları için kullanılan çapraz uygulama olacak. Ayrı bir sistemde yüklü. Eğitim malzemeleri ve Bilgi Bankası ambarı veya PLM belge yönetim sistemi mySAP kaynaklanan teknik çizimler belgelerinden bunun normal içeriktir. 

### <a name="sap-content-server-version-support-for-azure-vms"></a>Azure sanal makineler için SAP içerik sunucusu sürüm desteği
Şu anda SAP destekler:

* **SAP içerik sunucusu** sürümüyle **6.50 (ve üzeri)**
* **SAP MaxDB 7,9 sürümü**
* **Microsoft IIS (Internet Information Server) sürüm 8.0 (ve üzeri)**

En yeni sürümünü SAP içerik sunucusu ve en yeni sürümünü kullanacak şekilde önemle tavsiye edilir **Microsoft IIS**. 

Desteklenen en son sürümlerini SAP içerik sunucusu ve Microsoft IIS denetleyin [SAP ürünü kullanılabilirlik matris (PAM)][sap-pam].

### <a name="supported-microsoft-windows-and-azure-vm-types-for-sap-content-server"></a>SAP içerik sunucusu için desteklenen Microsoft Windows ile Azure VM türleri
Azure üzerinde SAP içerik sunucusu için desteklenen Windows sürümü bulmak için bkz:

* [SAP ürünü kullanılabilirlik matris (PAM)][sap-pam]
* SAP notu [1928533]

Microsoft Windows Server'ın en yeni sürümü kullanmak için önerilir.

### <a name="sap-content-server-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Azure vm'lerde SAP yüklemeleri için SAP içerik sunucusu yapılandırma yönergeleri
#### <a name="storage-configuration-for-content-server-in-azure"></a>Azure'da içerik sunucusu için depolama yapılandırması
SAP MaxDB veritabanında depolamak için SAP içerik sunucusu yapılandırırsanız, SAP MaxDB için bu belgede belirtilen tüm Azure depolama en iyi yöntemler öneri ayrıca SAP içerik sunucusu senaryosu için geçerli. 

Dosya sistemindeki dosyaları depolamak için SAP içerik sunucusu yapılandırırsanız, adanmış bir mantıksal sürücü kullanmak için önerilir. Windows depolama alanları kullanarak mantıksal diskin boyut ve IOPS işleme da artırabilirsiniz olanak tanır açıklandığı [SAP iş yükü Azure sanal makineleri DBMS dağıtım konuları](dbms_guide_general.md). 

#### <a name="sap-content-server-location"></a>SAP içerik sunucusu konumu
SAP içerik sunucusu aynı Azure bölgesindeki ve SAP sistemine dağıtıldığı Azure VNET dağıtılması gerekir. Adanmış bir Azure sanal makinesinde veya SAP sistemine çalıştığı aynı VM'de SAP içerik sunucusu bileşenlerini dağıtmak istediğinize karar verin ücretsizdir. 

![Azure VM'de SAP içerik sunucusu için ayrılmış](./media/dbms_maxdb_deployment_guide/800-azure-vm-sap-content-server.png)


#### <a name="sap-cache-server-location"></a>SAP önbellek sunucu konumu
SAP önbellek sunucusuna yerel olarak (önbelleğe alınmış) belgelere erişimi sağlamak için bir ek sunucu tabanlı bileşendir. SAP önbellek sunucusu, SAP içerik sunucusu belgeleri önbelleğe alır. Bu belgeler farklı konumlarda birden çok kez alınacak varsa ağ trafiğini en iyi duruma getirmek için yapılır. Genel kural SAP önbellek sunucusu SAP önbellek sunucusuna erişen istemci yakın fiziksel olarak sahip olur. 

Burada iki seçeneğiniz vardır:

1. **İstemci, bir arka uç SAP sistemidir** SAP içerik sunucusuna erişmek için bir arka uç SAP sistemine yapılandırdıysanız, bu SAP sistemine bir istemcidir. Aynı Azure bölgesinde, aynı Azure veri merkezinde hem SAP sistemi hem de SAP içerik sunucusu dağıtılırken fiziksel olarak birbirlerine yakın oldukları. Bu nedenle, ayrılmış bir SAP önbellek sunucusu olmasını gerek yoktur. SAP UI istemcileri (SAP GUI veya web tarayıcısı) doğrudan SAP sistemine erişim ve SAP sistemine SAP içerik sunucusundan belgelerini alır.
2. **İstemci, bir şirket içi web tarayıcısı olan** SAP içerik sunucusu, doğrudan web tarayıcısı tarafından erişilecek şekilde yapılandırılabilir. Bu durumda, şirket içinde çalışan bir web tarayıcısı olan SAP içeriği sunucusunun bir istemcidir. Şirket içi veri merkezi ve Azure veri merkezi (İdeal olarak yakın olması birbirleriyle) farklı fiziksel konumlarda yerleştirilir. Şirket içi veri merkezinizi Azure siteden siteye VPN veya ExpressRoute aracılığıyla azure'a bağlı. İki seçenek de Azure'da güvenli bir VPN ağ bağlantısı sunmasına rağmen siteden siteye ağ bağlantısı şirket içi veri merkezi ve Azure veri merkezi arasında ağ bant genişliği ve gecikme SLA sağlamaz. Belgeye erişimi hızlandırmak için aşağıdakilerden birini yapın:
   1. Şirket içi SAP önbellek sunucusu yüklemek, kapatmak için şirket içi web tarayıcısında (aşağıdaki şekilde seçeneği gibi)
   2. Azure şirket içi veri merkeziniz ile Azure veri merkezi arasında yüksek hızlı ve düşük gecikme süreli Adanmış ağ bağlantısı sunan ExpressRoute yapılandırın.

![Şirket içi SAP önbellek sunucusu yüklemek için seçeneği](./media/dbms_maxdb_deployment_guide/900-sap-cache-server-on-premises.png)
<a name="642f746c-e4d4-489d-bf63-73e80177a0a8"></a>

#### <a name="backup--restore"></a>Yedekleme / Geri Yükleme
SAP MaxDB veritabanında depolamak için SAP içerik sunucusu yapılandırırsanız, yedekleme/geri yükleme yordamı ve performans konuları zaten bu belgenin SAP MaxDB bölümlerde açıklanmıştır. 

Dosya sistemindeki dosyaları depolamak için SAP içerik sunucusu yapılandırırsanız, bir el ile yedekleme/geri yükleme tam dosya yapısı yürütmek için belgelerin bulunduğu yere seçenektir. Benzer şekilde, SAP MaxDB yedekleme/geri yükleme, bu yedekleme bir amaç için adanmış bir disk birimi sağlamak için önerilir. 

#### <a name="other"></a>Diğer
Diğer SAP içerik sunucusuna özgü ayarları, Azure Vm'leri için saydamdır ve çeşitli belgeleri ve SAP notları açıklanmıştır:

* <https://service.sap.com/contentserver> 
* SAP notu [1619726]  
