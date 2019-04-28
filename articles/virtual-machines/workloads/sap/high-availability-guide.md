---
title: Azure sanal makineleri SAP NetWeaver için yüksek kullanılabilirlik | Microsoft Docs
description: SAP NetWeaver için Azure sanal makineler üzerinde yüksek kullanılabilirlik Kılavuzu
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
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
ms.date: 01/24/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b3a4f3b37b0dc4d74b03ffcfa61c97fbb571d57f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61465601"
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms"></a>Azure vm'lerinde SAP NetWeaver için yüksek kullanılabilirlik

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

[sap-installation-guides]:http://service.sap.com/instguides

[azure-cli]:../../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:https://docs.microsoft.com/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md
[dbms-guide-2.1]:../../virtual-machines-windows-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f
[dbms-guide-2.2]:../../virtual-machines-windows-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91
[dbms-guide-2.3]:../../virtual-machines-windows-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152
[dbms-guide-2]:../../virtual-machines-windows-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64
[dbms-guide-3]:../../virtual-machines-windows-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3
[dbms-guide-5.5.1]:../../virtual-machines-windows-sap-dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268
[dbms-guide-5.5.2]:../../virtual-machines-windows-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b
[dbms-guide-5.6]:../../virtual-machines-windows-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8
[dbms-guide-5.8]:../../virtual-machines-windows-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30
[dbms-guide-5]:../../virtual-machines-windows-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737
[dbms-guide-8.4.1]:../../virtual-machines-windows-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573
[dbms-guide-8.4.2]:../../virtual-machines-windows-sap-dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d
[dbms-guide-8.4.3]:../../virtual-machines-windows-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c
[dbms-guide-8.4.4]:../../virtual-machines-windows-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818
[dbms-guide-900-sap-cache-server-on-premises]:../../virtual-machines-windows-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:../../virtual-machines-windows-sap-deployment-guide.md
[deployment-guide-2.2]:../../virtual-machines-windows-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94
[deployment-guide-3.1.2]:../../virtual-machines-windows-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab
[deployment-guide-3.2]:../../virtual-machines-windows-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281
[deployment-guide-3.3]:../../virtual-machines-windows-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2
[deployment-guide-3.4]:../../virtual-machines-windows-sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1
[deployment-guide-3]:../../virtual-machines-windows-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e
[deployment-guide-4.1]:../../virtual-machines-windows-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7
[deployment-guide-4.2]:../../virtual-machines-windows-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e
[deployment-guide-4.3]:../../virtual-machines-windows-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc
[deployment-guide-4.4.2]:../../virtual-machines-windows-sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542
[deployment-guide-4.4]:../../virtual-machines-windows-sap-deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d
[deployment-guide-4.5.1]:../../virtual-machines-windows-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4
[deployment-guide-4.5.2]:../../virtual-machines-windows-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f
[deployment-guide-4.5]:../../virtual-machines-windows-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca
[deployment-guide-5.1]:../../virtual-machines-windows-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2
[deployment-guide-5.2]:../../virtual-machines-windows-sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1
[deployment-guide-5.3]:../../virtual-machines-windows-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8

[deployment-guide-configure-monitoring-scenario-1]:../../virtual-machines-windows-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b
[deployment-guide-configure-proxy]:../../virtual-machines-windows-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d
[deployment-guide-figure-100]:media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:../../virtual-machines-windows-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:../../virtual-machines-windows-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:../../virtual-machines-windows-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:../../virtual-machines-windows-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:../../virtual-machines-windows-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:../../virtual-machines-windows-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:../../virtual-machines-windows-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:../../virtual-machines-windows-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:../../virtual-machines-windows-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b

[deploy-template-cli]:../../../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]:../../../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]:../../../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]:https://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:../../virtual-machines-windows-sap-get-started.md
[getting-started-dbms]:../../virtual-machines-windows-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:../../virtual-machines-windows-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:../../virtual-machines-windows-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:../../virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:../../virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:../../virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:../../virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:../../virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:../../virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:https://go.microsoft.com/fwlink/?LinkId=613056

[ha-guide]:high-availability-guide.md

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

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

[sap-ha-guide]:high-availability-guide.md
[sap-ha-guide-1]:high-availability-guide.md#217c5479-5595-4cd8-870d-15ab00d4f84c
[sap-ha-guide-2]:high-availability-guide.md#42b8f600-7ba3-4606-b8a5-53c4f026da08
[sap-ha-guide-3]:high-availability-guide.md#42156640c6-01cf-45a9-b225-4baa678b24f1
[sap-ha-guide-3.1]:high-availability-guide.md#f76af273-1993-4d83-b12d-65deeae23686
[sap-ha-guide-3.2]:high-availability-guide.md#3e85fbe0-84b1-4892-87af-d9b65ff91860
[sap-ha-guide-4]:high-availability-guide.md#8ecf3ba0-67c0-4495-9c14-feec1a2255b7
[sap-ha-guide-4.1]:high-availability-guide.md#1a3c5408-b168-46d6-99f5-4219ad1b1ff2
[sap-ha-guide-5]:high-availability-guide.md#fdfee875-6e66-483a-a343-14bbaee33275
[sap-ha-guide-5.1]:high-availability-guide.md#be21cf3e-fb01-402b-9955-54fbecf66592
[sap-ha-guide-5.2]:high-availability-guide.md#ff7a9a06-2bc5-4b20-860a-46cdb44669cd
[sap-ha-guide-6]:high-availability-guide.md#2ddba413-a7f5-4e4e-9a51-87908879c10a
[sap-ha-guide-6.1]:high-availability-guide.md#1a464091-922b-48d7-9d08-7cecf757f341
[sap-ha-guide-6.2]:high-availability-guide.md#44641e18-a94e-431f-95ff-303ab65e0bcb
[sap-ha-guide-7]:high-availability-guide.md#2e3fec50-241e-441b-8708-0b1864f66dfa
[sap-ha-guide-7.1]:high-availability-guide.md#93faa747-907e-440a-b00a-1ae0a89b1c0e
[sap-ha-guide-7.2]:high-availability-guide.md#f559c285-ee68-4eec-add1-f60fe7b978db
[sap-ha-guide-7.2.1]:high-availability-guide.md#b5b1fd0b-1db4-4d49-9162-de07a0132a51
[sap-ha-guide-7.3]:high-availability-guide.md#ddd878a0-9c2f-4b8e-8968-26ce60be1027
[sap-ha-guide-7.4]:high-availability-guide.md#045252ed-0277-4fc8-8f46-c5a29694a816
[sap-ha-guide-8]:high-availability-guide.md#78092dbe-165b-454c-92f5-4972bdbef9bf
[sap-ha-guide-8.1]:high-availability-guide.md#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-ha-guide-8.2]:high-availability-guide.md#7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310
[sap-ha-guide-8.3]:high-availability-guide.md#47d5300a-a830-41d4-83dd-1a0d1ffdbe6a
[sap-ha-guide-8.4]:high-availability-guide.md#b22d7b3b-4343-40ff-a319-097e13f62f9e
[sap-ha-guide-8.5]:high-availability-guide.md#9fbd43c0-5850-4965-9726-2a921d85d73f
[sap-ha-guide-8.6]:high-availability-guide.md#84c019fe-8c58-4dac-9e54-173efd4b2c30
[sap-ha-guide-8.7]:high-availability-guide.md#7a8f3e9b-0624-4051-9e41-b73fff816a9e
[sap-ha-guide-8.8]:high-availability-guide.md#f19bd997-154d-4583-a46e-7f5a69d0153c
[sap-ha-guide-8.9]:high-availability-guide.md#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-ha-guide-8.10]:high-availability-guide.md#e69e9a34-4601-47a3-a41c-d2e11c626c0c
[sap-ha-guide-8.11]:high-availability-guide.md#661035b2-4d0f-4d31-86f8-dc0a50d78158
[sap-ha-guide-8.12]:high-availability-guide.md#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-ha-guide-8.12.1]:high-availability-guide.md#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-ha-guide-8.12.2]:high-availability-guide.md#e49a4529-50c9-4dcf-bde7-15a0c21d21ca
[sap-ha-guide-8.12.2.1]:high-availability-guide.md#06260b30-d697-4c4d-b1c9-d22c0bd64855
[sap-ha-guide-8.12.2.2]:high-availability-guide.md#4c08c387-78a0-46b1-9d27-b497b08cac3d
[sap-ha-guide-8.12.3]:high-availability-guide.md#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-ha-guide-8.12.3.1]:high-availability-guide.md#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-ha-guide-8.12.3.2]:high-availability-guide.md#dd41d5a2-8083-415b-9878-839652812102
[sap-ha-guide-8.12.3.3]:high-availability-guide.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006
[sap-ha-guide-9]:high-availability-guide.md#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:high-availability-guide.md#31c6bd4f-51df-4057-9fdf-3fcbc619c170
[sap-ha-guide-9.1.1]:high-availability-guide.md#a97ad604-9094-44fe-a364-f89cb39bf097
[sap-ha-guide-9.1.2]:high-availability-guide.md#eb5af918-b42f-4803-bb50-eff41f84b0b0
[sap-ha-guide-9.1.3]:high-availability-guide.md#e4caaab2-e90f-4f2c-bc84-2cd2e12a9556
[sap-ha-guide-9.1.4]:high-availability-guide.md#10822f4f-32e7-4871-b63a-9b86c76ce761
[sap-ha-guide-9.1.5]:high-availability-guide.md#4498c707-86c0-4cde-9c69-058a7ab8c3ac
[sap-ha-guide-9.2]:high-availability-guide.md#85d78414-b21d-4097-92b6-34d8bcb724b7
[sap-ha-guide-9.3]:high-availability-guide.md#8a276e16-f507-4071-b829-cdc0a4d36748
[sap-ha-guide-9.4]:high-availability-guide.md#094bc895-31d4-4471-91cc-1513b64e406a
[sap-ha-guide-9.5]:high-availability-guide.md#2477e58f-c5a7-4a5d-9ae3-7b91022cafb5
[sap-ha-guide-9.6]:high-availability-guide.md#0ba4a6c1-cc37-4bcf-a8dc-025de4263772
[sap-ha-guide-10]:high-availability-guide.md#18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9
[sap-ha-guide-10.1]:high-availability-guide.md#65fdef0f-9f94-41f9-b314-ea45bbfea445
[sap-ha-guide-10.2]:high-availability-guide.md#5e959fa9-8fcd-49e5-a12c-37f6ba07b916
[sap-ha-guide-10.3]:high-availability-guide.md#755a6b93-0099-4533-9f6d-5c9a613878b5

[sap-ha-multi-sid-guide]:high-availability-multi-sid.md (SAP çoklu SID yüksek kullanılabilirliği yapılandırma)


[sap-ha-guide-figure-1000]:media/virtual-machines-shared-sap-high-availability-guide/1000-wsfc-for-sap-ascs-on-azure.png
[sap-ha-guide-figure-1001]:media/virtual-machines-shared-sap-high-availability-guide/1001-wsfc-on-azure-ilb.png
[sap-ha-guide-figure-1002]:media/virtual-machines-shared-sap-high-availability-guide/1002-wsfc-sios-on-azure-ilb.png
[sap-ha-guide-figure-2000]:media/virtual-machines-shared-sap-high-availability-guide/2000-wsfc-sap-as-ha-on-azure.png
[sap-ha-guide-figure-2001]:media/virtual-machines-shared-sap-high-availability-guide/2001-wsfc-sap-ascs-ha-on-azure.png
[sap-ha-guide-figure-2003]:media/virtual-machines-shared-sap-high-availability-guide/2003-wsfc-sap-dbms-ha-on-azure.png
[sap-ha-guide-figure-2004]:media/virtual-machines-shared-sap-high-availability-guide/2004-wsfc-sap-ha-e2e-archit-template1-on-azure.png
[sap-ha-guide-figure-2005]:media/virtual-machines-shared-sap-high-availability-guide/2005-wsfc-sap-ha-e2e-arch-template2-on-azure.png

[sap-ha-guide-figure-3000]:media/virtual-machines-shared-sap-high-availability-guide/3000-template-parameters-sap-ha-arm-on-azure.png
[sap-ha-guide-figure-3001]:media/virtual-machines-shared-sap-high-availability-guide/3001-configuring-dns-servers-for-Azure-vnet.png
[sap-ha-guide-figure-3002]:media/virtual-machines-shared-sap-high-availability-guide/3002-configuring-static-IP-address-for-network-card-of-each-vm.png
[sap-ha-guide-figure-3003]:media/virtual-machines-shared-sap-high-availability-guide/3003-setup-static-ip-address-ilb-for-ascs-instance.png
[sap-ha-guide-figure-3004]:media/virtual-machines-shared-sap-high-availability-guide/3004-default-ascs-scs-ilb-balancing-rules-for-azure-ilb.png
[sap-ha-guide-figure-3005]:media/virtual-machines-shared-sap-high-availability-guide/3005-changing-ascs-scs-default-ilb-rules-for-azure-ilb.png
[sap-ha-guide-figure-3006]:media/virtual-machines-shared-sap-high-availability-guide/3006-adding-vm-to-domain.png
[sap-ha-guide-figure-3007]:media/virtual-machines-shared-sap-high-availability-guide/3007-config-wsfc-1.png
[sap-ha-guide-figure-3008]:media/virtual-machines-shared-sap-high-availability-guide/3008-config-wsfc-2.png
[sap-ha-guide-figure-3009]:media/virtual-machines-shared-sap-high-availability-guide/3009-config-wsfc-3.png
[sap-ha-guide-figure-3010]:media/virtual-machines-shared-sap-high-availability-guide/3010-config-wsfc-4.png
[sap-ha-guide-figure-3011]:media/virtual-machines-shared-sap-high-availability-guide/3011-config-wsfc-5.png
[sap-ha-guide-figure-3012]:media/virtual-machines-shared-sap-high-availability-guide/3012-config-wsfc-6.png
[sap-ha-guide-figure-3013]:media/virtual-machines-shared-sap-high-availability-guide/3013-config-wsfc-7.png
[sap-ha-guide-figure-3014]:media/virtual-machines-shared-sap-high-availability-guide/3014-config-wsfc-8.png
[sap-ha-guide-figure-3015]:media/virtual-machines-shared-sap-high-availability-guide/3015-config-wsfc-9.png
[sap-ha-guide-figure-3016]:media/virtual-machines-shared-sap-high-availability-guide/3016-config-wsfc-10.png
[sap-ha-guide-figure-3017]:media/virtual-machines-shared-sap-high-availability-guide/3017-config-wsfc-11.png
[sap-ha-guide-figure-3018]:media/virtual-machines-shared-sap-high-availability-guide/3018-config-wsfc-12.png
[sap-ha-guide-figure-3019]:media/virtual-machines-shared-sap-high-availability-guide/3019-assign-permissions-on-share-for-cluster-name-object.png
[sap-ha-guide-figure-3020]:media/virtual-machines-shared-sap-high-availability-guide/3020-change-object-type-include-computer-objects.png
[sap-ha-guide-figure-3021]:media/virtual-machines-shared-sap-high-availability-guide/3021-check-box-for-computer-objects.png
[sap-ha-guide-figure-3022]:media/virtual-machines-shared-sap-high-availability-guide/3022-set-security-attributes-for-cluster-name-object-on-file-share-quorum.png
[sap-ha-guide-figure-3023]:media/virtual-machines-shared-sap-high-availability-guide/3023-call-configure-cluster-quorum-setting-wizard.png
[sap-ha-guide-figure-3024]:media/virtual-machines-shared-sap-high-availability-guide/3024-selection-screen-different-quorum-configurations.png
[sap-ha-guide-figure-3025]:media/virtual-machines-shared-sap-high-availability-guide/3025-selection-screen-file-share-witness.png
[sap-ha-guide-figure-3026]:media/virtual-machines-shared-sap-high-availability-guide/3026-define-file-share-location-for-witness-share.png
[sap-ha-guide-figure-3027]:media/virtual-machines-shared-sap-high-availability-guide/3027-successful-reconfiguration-cluster-file-share-witness.png
[sap-ha-guide-figure-3028]:media/virtual-machines-shared-sap-high-availability-guide/3028-install-dot-net-framework-35.png
[sap-ha-guide-figure-3029]:media/virtual-machines-shared-sap-high-availability-guide/3029-install-dot-net-framework-35-progress.png
[sap-ha-guide-figure-3030]:media/virtual-machines-shared-sap-high-availability-guide/3030-sios-installer.png
[sap-ha-guide-figure-3031]:media/virtual-machines-shared-sap-high-availability-guide/3031-first-screen-sios-data-keeper-installation.png
[sap-ha-guide-figure-3032]:media/virtual-machines-shared-sap-high-availability-guide/3032-data-keeper-informs-service-be-disabled.png
[sap-ha-guide-figure-3033]:media/virtual-machines-shared-sap-high-availability-guide/3033-user-selection-sios-data-keeper.png
[sap-ha-guide-figure-3034]:media/virtual-machines-shared-sap-high-availability-guide/3034-domain-user-sios-data-keeper.png
[sap-ha-guide-figure-3035]:media/virtual-machines-shared-sap-high-availability-guide/3035-provide-sios-data-keeper-license.png
[sap-ha-guide-figure-3036]:media/virtual-machines-shared-sap-high-availability-guide/3036-data-keeper-management-config-tool.png
[sap-ha-guide-figure-3037]:media/virtual-machines-shared-sap-high-availability-guide/3037-tcp-ip-address-first-node-data-keeper.png
[sap-ha-guide-figure-3038]:media/virtual-machines-shared-sap-high-availability-guide/3038-create-replication-sios-job.png
[sap-ha-guide-figure-3039]:media/virtual-machines-shared-sap-high-availability-guide/3039-define-sios-replication-job-name.png
[sap-ha-guide-figure-3040]:media/virtual-machines-shared-sap-high-availability-guide/3040-define-sios-source-node.png
[sap-ha-guide-figure-3041]:media/virtual-machines-shared-sap-high-availability-guide/3041-define-sios-target-node.png
[sap-ha-guide-figure-3042]:media/virtual-machines-shared-sap-high-availability-guide/3042-define-sios-synchronous-replication.png
[sap-ha-guide-figure-3043]:media/virtual-machines-shared-sap-high-availability-guide/3043-enable-sios-replicated-volume-as-cluster-volume.png
[sap-ha-guide-figure-3044]:media/virtual-machines-shared-sap-high-availability-guide/3044-data-keeper-synchronous-mirroring-for-SAP-gui.png
[sap-ha-guide-figure-3045]:media/virtual-machines-shared-sap-high-availability-guide/3045-replicated-disk-by-data-keeper-in-wsfc.png
[sap-ha-guide-figure-3046]:media/virtual-machines-shared-sap-high-availability-guide/3046-dns-entry-sap-ascs-virtual-name-ip.png
[sap-ha-guide-figure-3047]:media/virtual-machines-shared-sap-high-availability-guide/3047-dns-manager.png
[sap-ha-guide-figure-3048]:media/virtual-machines-shared-sap-high-availability-guide/3048-default-cluster-probe-port.png
[sap-ha-guide-figure-3049]:media/virtual-machines-shared-sap-high-availability-guide/3049-cluster-probe-port-after.png
[sap-ha-guide-figure-3050]:media/virtual-machines-shared-sap-high-availability-guide/3050-service-type-ers-delayed-automatic.png
[sap-ha-guide-figure-5000]:media/virtual-machines-shared-sap-high-availability-guide/5000-wsfc-sap-sid-node-a.png
[sap-ha-guide-figure-5001]:media/virtual-machines-shared-sap-high-availability-guide/5001-sios-replicating-local-volume.png
[sap-ha-guide-figure-5002]:media/virtual-machines-shared-sap-high-availability-guide/5002-wsfc-sap-sid-node-b.png
[sap-ha-guide-figure-5003]:media/virtual-machines-shared-sap-high-availability-guide/5003-sios-replicating-local-volume-b-to-a.png

[sap-ha-guide-figure-6003]:media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png

[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../../networking/networking-overview.md
[sap-pam]:https://support.sap.com/pam (SAP ürün kullanılabilirliği Matrisi)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-multisid-xscs-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps%2Fazuredeploy.json
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
[virtual-machines-windows-attach-disk-portal]:../../virtual-machines-windows-attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../../azure-resource-manager/resource-group-overview.md
[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../../../azure-resource-manager/resource-group-overview.md#the-benefits-of-using-resource-manager
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-windows-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md
[virtual-machines-linux-agent-user-guide]:../../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-capture]:../../linux/capture-image.md#capture-the-vm
[virtual-machines-windows-capture-image]:../../virtual-machines-windows-capture-image.md
[virtual-machines-windows-capture-image-capture]:../../virtual-machines-windows-capture-image.md#capture-the-vm
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability]:../../virtual-machines-windows-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:../../virtual-machines-windows-ps-create.md
[virtual-machines-sizes]:../../virtual-machines-windows-sizes.md
[virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]:../../windows/sql/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md
[virtual-machines-windows-portal-sql-alwayson-int-listener]:../../windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md
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
[vpn-gateway-site-to-site-create]:../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md
[vpn-gateway-vpn-faq]:../../../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../../../cli-install-nodejs.md
[xplat-cli-azure-resource-manager]:../../../xplat-cli-azure-resource-manager.md


Azure sanal makineleri, bilgi işlem, depolama ve ağ kaynaklarına, mümkün olduğunca az zamanda ve uzun tedarik döngüleri olmadan gereken kuruluşlar çözümüdür. Azure sanal makinelerinde SAP NetWeaver tabanlı ABAP ve Java ABAP + Java yığını gibi Klasik uygulamaları dağıtmak için kullanabilirsiniz. Güvenilirlik ve kullanılabilirlik ek şirket içi kaynaklar olmadan genişletin. Böylece Azure sanal makineleri kuruluşunuzun şirket içi etki alanları, özel Bulutlar ve SAP sistem ortamı tümleştirebilirsiniz şirketler arası bağlantı, Azure sanal makineleri destekler.

Bu makalede, Azure Resource Manager dağıtım modelini kullanarak yüksek kullanılabilirlik SAP sistemlerini azure'a dağıtmak için uygulayabileceğiniz adımları ele. Biz size, bu önemli görevleri gerçekleştirmeyi açıklamaktadır:

* Doğru SAP notları ve yükleme kılavuzlarını, listede bulmak [kaynakları] [ sap-ha-guide-2] bölümü. Bu makale SAP yükleme belgelerine tamamlar ve yardımcı olabilecek birincil kaynaklardır SAP notları yükleyin ve belirli platformları üzerinde SAP yazılımı dağıtın.
* Azure Resource Manager dağıtım modelini ve Azure Klasik dağıtım modeli arasındaki farkları öğrenin.
* Azure dağıtımınız için uygun olan model seçebilmeniz için Windows Server Yük Devretme Kümelemesi, çekirdek modu hakkında bilgi edinin.
* Azure hizmetlerindeki paylaşılan Windows Server Yük Devretme Kümelemesi depolama hakkında bilgi edinin.
* Hata, tek nokta bileşenleri gibi gelişmiş iş uygulaması programlama (ABAP) SAP Central Hizmetleri (ASCS) korunmasına nasıl yardımcı olacağını öğrenin / SAP Central Hizmetleri (SCS) ve veritabanı yönetim sistemi (DBMS) ve yedekli bileşenler gibi SAP uygulama Azure'da sunucusu.
* Yükleme ve azure'da Windows Server Yük Devretme Kümelemesi bir kümedeki bir yüksek kullanılabilirlik SAP sistemi yapılandırmasını adım adım bir örnek, Azure Resource Manager'ı kullanarak izleyin.
* Windows Server Yük Devretme Kümelemesi Azure, ancak bir şirket içi dağıtımda gerekmeyen kullanmak için gereken ek adımlar hakkında bilgi edinin.

Dağıtım ve yapılandırma, bu makaledeki basitleştirmek için SAP üç katmanlı yüksek kullanılabilirlik Resource Manager şablonları kullanıyoruz. Şablonları bir yüksek kullanılabilirlik SAP sistemine gereksinim duyduğunuz tüm altyapının dağıtımını otomatikleştirin. Altyapı, SAP uygulama performans standart (SAP) boyutlandırma SAP sisteminizin da destekler.

## <a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a> Önkoşullar
Başlamadan önce aşağıdaki bölümlerde açıklanan önkoşulları karşıladığından emin olun. Ayrıca, listelenen tüm kaynakları denetlediğinizden emin olun [kaynakları] [ sap-ha-guide-2] bölümü.

Bu makalede, Azure Resource Manager şablonları kullanıyoruz [üç katmanlı SAP NetWeaver](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image/). Şablonları yararlı bir genel bakış için bkz: [SAP Azure Resource Manager şablonları](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).

## <a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a> Kaynakları
Bu makaleler, azure'da SAP dağıtımları kapsar:

* [Azure sanal makineleri planlama ve uygulama için SAP NetWeaver][planning-guide]
* [Azure sanal makineler dağıtım için SAP NetWeaver][deployment-guide]
* [Azure sanal makineleri DBMS dağıtım için SAP NetWeaver][dbms-guide]
* [Azure sanal makineler (Bu kılavuzda) SAP NetWeaver için yüksek kullanılabilirlik][sap-ha-guide]

> [!NOTE]
> Mümkün olduğunda, bir bağlantıya başvuran SAP Yükleme Kılavuzu sunuyoruz (bkz [SAP yükleme kılavuzlarını][sap-installation-guides]). Önkoşullar ve yükleme süreci hakında bilgi için SAP NetWeaver yükleme kılavuzlarını dikkatle okuyun iyi bir fikirdir. Bu makale, Azure sanal makineler ile kullandığınız SAP NetWeaver tabanlı sistemler için yalnızca belirli görevleri kapsar.
>
>

Azure'da SAP konu bu SAP notları ilgili:

| Not numarası | Unvan |
| --- | --- |
| [1928533] |Azure'da SAP uygulamaları: Desteklenen Ürünler ve boyutlandırma |
| [2015553] |Microsoft Azure üzerinde SAP: Destek önkoşulları |
| [1999351] |SAP için Azure izleme Gelişmiş |
| [2178632] |Microsoft Azure üzerinde SAP için ölçümleri izleme anahtarı |
| [1999351] |Sanallaştırma Windows üzerinde: Gelişmiş izleme |
| [2243692] |SAP DBMS örneği için Azure Premium SSD depolama kullanımı |

Daha fazla bilgi edinin [Azure abonelikleri sınırlamaları][azure-subscription-service-limits-subscription]sınırlamalar genel varsayılan ve en fazla sınırlamalar dahil olmak üzere.

## <a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>Azure Klasik dağıtım modelini ve Azure Resource Manager ile yüksek kullanılabilirlik SAP
Azure Klasik dağıtım modelleri ve Azure Resource Manager aşağıdaki alanlarda farklıdır:

- Kaynak grupları
- Azure iç yük dengeleyici bağımlılık Azure kaynak grubu
- SAP çoklu SID senaryolar için destek

### <a name="f76af273-1993-4d83-b12d-65deeae23686"></a> Kaynak grupları
Azure Resource Manager'da Azure aboneliğinizdeki tüm uygulama kaynaklarını yönetmek için kaynak gruplarını kullanabilirsiniz. Tümleşik bir yaklaşım bir kaynak grubu içinde tüm kaynaklar aynı yaşam döngüsüne sahiptir. Örneğin, tüm kaynaklar aynı anda oluşturulur ve aynı anda bunlar silinir. [Kaynak grupları](../../../azure-resource-manager/resource-group-overview.md#resource-groups) hakkında daha fazla bilgi edinin.

### <a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a> Azure iç yük dengeleyici bağımlılık Azure kaynak grubu

Azure Klasik dağıtım modelinde Azure iç yük dengeleyici (Azure Load Balancer hizmeti) ve bulut hizmeti grubu arasındaki bağımlılık yoktur. Her iç yük dengeleyici, bir bulut hizmeti grubu gerekir.

Azure Resource Manager'da Azure Load Balancer'ı kullanmak için bir Azure kaynak grubu gerekmez. , Basit ve daha esnek ortamıdır.

### <a name="support-for-sap-multi-sid-scenarios"></a>SAP çoklu SID senaryolar için destek

Azure Resource Manager'da birden çok SAP sistem tanımlayıcısı (SID) ASCS/SCS örneği bir kümede yükleyebilirsiniz. Çoklu SID örnekleri, her bir Azure iç yük dengeleyici için birden çok IP adresi için destek nedeniyle mümkündür.

Azure Klasik dağıtım modelini kullanmak için açıklanan yordamları izleyin [azure'da SAP NetWeaver: SIOS DataKeeper ile azure'da Windows Server Yük Devretme Kümelemesi'ı kullanarak SAP ASCS/SCS örnekleri Kümeleme](https://go.microsoft.com/fwlink/?LinkId=613056).

> [!IMPORTANT]
> SAP tesislerinize için Azure Resource Manager dağıtım modeli kullanmanız önerilir. Klasik dağıtım modelinde kullanılabilir olmayan birçok avantaj sunar. Azure hakkında daha fazla bilgi [dağıtım modelleri][virtual-machines-azure-resource-manager-architecture-benefits-arm].   
>
>

## <a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a> Windows Server Yük Devretme Kümelemesi
Windows Server Yük Devretme Kümelemesi yüksek kullanılabilirlik SAP ASCS/SCS yükleme ve Windows DBMS'de temelidir.

Bir yük devretme kümesi, uygulamaların ve hizmetlerin kullanılabilirliğini artırmak için birlikte çalışan 1 + n bağımsız sunucular (düğümler) grubudur. Windows Server Yük Devretme Kümelemesi, bir düğüm arıza durumunda, uygulamaları ve hizmetleri sağlamak için iyi bir küme korurken oluşan hata sayısı hesaplar. Yük devretme elde etmek için farklı bir çekirdek modlarından seçebilirsiniz.

### <a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a> Çekirdek modu
Windows Server Yük Devretme Kümelemesi kullanırken dört çekirdek modlarından birini seçebilirsiniz:

* **Düğüm çoğunluğu**. Kümedeki her düğüm oy verebilirsiniz. Küme yalnızca oy, çoğunu ile diğer bir deyişle, yarısından fazlasına oyları ile çalışır. Bu seçenek eşit sayıda düğüme sahip kümeler için önerilir. Örneğin, bir yedi düğüm kümedeki üç düğüm başarısız olabilir ve küme durağan çalışmaya devam eder ve çoğu başarır.  
* **Düğüm ve Disk Çoğunluğu**. Her düğüm ve küme depolama alanındaki atanan bir disk (bir disk tanığı), kullanılabilir olduğunda ve iletişim oy verebilirsiniz. Küme yalnızca oy çoğunu ile diğer bir deyişle, yarısından fazlasına oyları ile çalışır. Bu mod, bir küme ortamında düğümlerinin bir çift sayı mantıklıdır. Düğümlerin yarısının ve disk çevrimiçi olması halinde kümenin iyi durumda kalır.
* **Düğüm ve dosya paylaşımı çoğunluğu**. Her düğümü ve yönetici oluşturur bir dosya paylaşımı (dosya paylaşım tanığı), düğüm ve dosya paylaşımı kullanılabilir olup bağımsız olarak ve iletişim oy verebilirsiniz. Küme yalnızca oy çoğunu ile diğer bir deyişle, yarısından fazlasına oyları ile çalışır. Bu mod, bir küme ortamında düğümlerinin bir çift sayı mantıklıdır. Düğüm ve Disk Çoğunluğu moduna benzer, ancak Tanık diski yerine bir Tanık dosya paylaşımı kullanır. Bu mod uygulanması kolaydır, ancak dosya paylaşımı, kendisi için yüksek oranda kullanılabilir değil, bir tek hata noktası olabilir.
* **Çoğunluk yok: Yalnızca disk**. Bir düğüm kullanılabilir ve küme depolama alanındaki belirli bir diskle iletişimi ise kümenin çekirdeği vardır. Bu diski ile iletişimi olan düğüm kümesine katılabilirsiniz. Bu mod kullanmamanızı öneririz.

## <a name="fdfee875-6e66-483a-a343-14bbaee33275"></a> Şirket içi Windows Server Yük devretme
Şekil 1 iki düğümden oluşan bir küme gösterilir. Düğümleri başarısız olur ve düğümler Üstünlüğünüzü hem çalışan, bir çekirdek disk veya dosya arasında ağ bağlantısı paylaşırsanız, hangi düğümün kümenin uygulamaları ve hizmetleri sağlamaya devam edecek belirler. Bir çekirdek disk veya dosya paylaşımı erişimi olan hizmetleri devam düğümü düğümüdür.

Bu örnek iki düğümlü bir küme kullandığından, düğüm ve dosya paylaşımı çoğunluğu çekirdek modu kullanırız. Düğüm ve Disk çoğunluğu, geçerli bir seçenek de olan. Bir üretim ortamında, bir çekirdek disk kullanmanızı öneririz. Yüksek oranda kullanılabilir hale getirmek için ağ ve depolama sistemi teknolojisini kullanabilirsiniz.

![Şekil 1: Bir Windows Server Yük Devretme Kümelemesi yapılandırma için SAP ASCS/SCS azure'da örneği][sap-ha-guide-figure-1000]

_**Şekil 1:** Bir Windows Server Yük Devretme Kümelemesi yapılandırma için SAP ASCS/SCS azure'da örneği_

### <a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a> Paylaşılan depolama
Şekil 1 iki düğümlü paylaşılan depolama kümesi ayrıca gösterir. Şirket içi paylaşılan depolama alanı kümede, kümedeki tüm düğümlerin paylaşılan depolama algılayın. Kilitleme mekanizması bozulmaya karşı verileri korur. Tüm düğümleri, başka bir düğüm başarısız olursa algılayabilir. Bir düğüm başarısız olursa, kalan düğümü depolama kaynaklarını sahipliğini ve hizmetlerin kullanılabilirliğini sağlar.

> [!NOTE]
> SQL Server gibi bazı DBMS uygulamalarla, yüksek kullanılabilirlik için paylaşılan diskler gerekmez. SQL Server Always On başka bir küme düğümünün yerel diskteki bir küme düğümünde yerel diskten DBMS veri ve günlük dosyalarını çoğaltır. Bu durumda, Windows Küme yapılandırması, paylaşılan bir disk gerektirmez.
>
>

### <a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a> Ağ ve ad çözümlemesi
İstemci bilgisayarların küme sanal IP adresi ve DNS sunucusu sağlayan bir sanal ana bilgisayar adı üzerinden ulaşın. Şirket içi düğümleri ve DNS sunucusu birden çok IP adresi başa çıkabilir.

Tipik bir Kurulum, iki veya daha fazla ağ bağlantıları kullanın:

* Depolama için ayrılmış bir bağlantı
* Sinyal için küme iç ağ bağlantısı
* İstemcilerin kümeye bağlanmak için kullandığı bir ortak ağ

## <a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a> Azure'da Windows Server Yük devretme
Azure sanal makineler, çıplak metal veya özel bulut dağıtımları için karşılaştırıldığında, Windows Server Yük Devretme Kümelemesi yapılandırmak için ek adımlar gerektirir. Paylaşılan bir küme diski oluştururken birden fazla IP adresleri ve sanal ana bilgisayar adlarını SAP ASCS/SCS örneği için ayarlamanız gerekir.

Bu makalede, temel kavramları ve azure'da bir SAP central Services'in yüksek kullanılabilirlik kümesi oluşturmak için gereken ek adımları ele alır. Üçüncü taraf Aracı'nı SIOS DataKeeper ayarlama ve Azure iç Yük Dengeleyiciyi yapılandırma gösteriyoruz. Azure dosya paylaşım tanığı ile Windows Yük devretme kümesi oluşturmak için bu araçları kullanabilirsiniz.

![Şekil 2: Windows Server Yük Devretme Kümelemesi paylaşılan disk olmadan azure'da yapılandırma][sap-ha-guide-figure-1001]

_**Şekil 2:** Windows Server Yük Devretme Kümelemesi paylaşılan disk olmadan azure'da yapılandırma_

### <a name="1a464091-922b-48d7-9d08-7cecf757f341"></a> Azure'da disk SIOS DataKeeper ile paylaşılan
Yüksek kullanılabilirlik SAP ASCS/SCS örneği için paylaşılan depolama kümesi. Eylül 2016'den itibaren Azure, paylaşılan depolama kümesi oluşturmak için kullanabileceğiniz, paylaşılan depolama sunmaz. Üçüncü taraf yazılım SIOS DataKeeper Cluster Edition, Küme Paylaşılan depolama benzetim yansıtılmış depolama alanı oluşturmak için kullanabilirsiniz. Gerçek zamanlı zaman uyumlu veri çoğaltmayı SIOS çözümüdür. Bu küme için paylaşılan disk kaynağı nasıl oluşturabilirsiniz.

1. Bir ek Azure sanal sabit disk (VHD) her sanal makinelerin (VM'ler) bir Windows küme yapılandırmasında ekleyin.
2. SIOS DataKeeper Cluster Edition, her iki sanal makine düğümde çalıştırın.
3. Kaynak sanal makinenin ek bağlı VHD birimden ek bağlı VHD birime hedef sanal makinenin içeriği yansıtan SIOS DataKeeper Cluster Edition yapılandırın. SIOS DataKeeper kaynak ve hedef yerel birimlere soyutlar ve Windows Server Yük Devretme Kümelemesi bir paylaşılan disk olarak için sunar.

Hakkında daha fazla bilgi edinin [SIOS DataKeeper](http://us.sios.com/products/datakeeper-cluster/).

![Şekil 3: SIOS DataKeeper ile azure'da Windows Server Yük Devretme Kümelemesi yapılandırma][sap-ha-guide-figure-1002]

_**Şekil 3:** SIOS DataKeeper ile azure'da Windows Server Yük Devretme Kümelemesi yapılandırma_

> [!NOTE]
> SQL Server gibi bazı DBMS ürünleri ile yüksek kullanılabilirlik için paylaşılan diskler gerekmez. SQL Server Always On başka bir küme düğümünün yerel diskteki bir küme düğümünde yerel diskten DBMS veri ve günlük dosyalarını çoğaltır. Bu durumda, Windows Küme yapılandırması, paylaşılan bir disk gerekli değildir.
>
>

### <a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a> Azure'da ad çözümlemesi
Azure bulut platformunda sanal IP adresleri, kayan IP adresleri gibi yapılandırma seçeneği sunulmaz. Küme kaynağı bulutta ulaşmak için sanal bir IP adresi ayarlamak için alternatif bir çözüm ihtiyacınız vardır.
Azure Azure Load Balancer hizmetinde bir iç yük dengeleyici sahiptir. İç yük dengeleyiciyle istemciler kümeye küme sanal IP adresi ulaşın.
Küme düğümleri içeren bir kaynak grubu içinde iç yük dengeleyici dağıtmak için ihtiyacınız. Ardından, tüm gerekli bağlantı noktası kuralları araştırma ile iç yük dengeleyici bağlantı noktası iletme yapılandırın.
İstemciler, sanal ana bilgisayar adı bağlanabilir. DNS sunucusu, küme IP adresini ve iletme kümesinin etkin düğümü için iç yük dengeleyici tanıtıcıları bağlantı noktası çözümler.

## <a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a> SAP NetWeaver Azure-olarak-hizmet altyapı (Iaas) yüksek kullanılabilirlik
SAP uygulama yüksek kullanılabilirlik gibi SAP yazılım bileşenleri için elde etmek için aşağıdaki bileşenler korumak yapmanız gerekir:

* SAP uygulama sunucusu örneği
* SAP ASCS/SCS örneği
* DBMS sunucusu

Yüksek kullanılabilirlik senaryolarını SAP bileşenlerde koruma hakkında daha fazla bilgi için bkz. [planlama Azure sanal makineleri ve SAP NetWeaver uygulamasını](planning-guide.md).

### <a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a> Yüksek kullanılabilirlik SAP uygulama sunucusu
Ayrıca SAP uygulama sunucusu ve iletişim örnekleri için genellikle belirli bir yüksek kullanılabilirlik çözümü gerekmez. Yedeklilik ile yüksek kullanılabilirlik elde edin ve farklı durumlarda Azure sanal makinelerinin iletişim birden fazla yapılandıracaksınız. İki durumda Azure sanal makinelerinin yüklü en az iki SAP uygulama örneklerine sahip olmalıdır.

![Şekil 4: Yüksek kullanılabilirlik SAP uygulama sunucusu][sap-ha-guide-figure-2000]

_**Şekil 4:** Yüksek kullanılabilirlik SAP uygulama sunucusu_

Ana bilgisayar SAP uygulama sunucusu örneklerinin aynı Azure kullanılabilirlik kümesi tüm sanal makineler yerleştirmeniz gerekir. Azure kullanılabilirlik kümesine sağlar:

* Tüm sanal makineler aynı yükseltme etki alanı'nın bir parçasıdır. Bir yükseltme etki alanını, örneğin, sanal makineler planlı bakım kapalı kalma süresinde aynı anda güncelleştirmeyen emin olur.
* Tüm sanal makineler aynı hata etki alanı'nın bir parçasıdır. Hata etki alanı, örneğin, böylece hiç tek hata noktası tüm sanal makinelerin kullanılabilirliğini etkileyen dağıtılan sanal makineler, emin olur.

Kullanma hakkında daha fazla bilgi edinin [sanal makinelerin kullanılabilirliğini yönetme][virtual-machines-manage-availability].

Azure depolama hesabı, olası bir tek hata noktası olduğundan, en az iki Azure depolama hesapları, en az iki sanal makine dağıtılmasını sağlamak önemlidir. İdeal bir Kurulum, SAP iletişim örneği çalıştıran her sanal makinenin diskleri farklı depolama hesabında dağıtılabilir.

### <a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a> Yüksek kullanılabilirlik SAP ASCS/SCS örneği
Şekil 5, yüksek kullanılabilirlik SAP ASCS/SCS örneği örneğidir.

![Şekil 5: Yüksek kullanılabilirlik SAP ASCS/SCS örneği][sap-ha-guide-figure-2001]

_**Şekil 5:** Yüksek kullanılabilirlik SAP ASCS/SCS örneği_

#### <a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a> SAP ASCS/SCS örneği ile Windows Server Yük Devretme Kümelemesi azure'da yüksek kullanılabilirlik
Azure sanal makineler, çıplak metal veya özel bulut dağıtımları için karşılaştırıldığında, Windows Server Yük Devretme Kümelemesi yapılandırmak için ek adımlar gerektirir. Windows Yük devretme kümesi oluşturmak için paylaşılan bir küme diski, birden fazla IP adresi, birkaç sanal ana bilgisayar adı ve bir Azure iç yük dengeleyici kümeleme SAP ASCS/SCS örneği için gerekir. Bu makalenin ilerleyen bölümlerinde daha ayrıntılı olarak ele alır.

![Şekil 6: Windows Server Yük Devretme Kümelemesi kullanarak SIOS DataKeeper azure'da SAP ASCS/SCS yapılandırma][sap-ha-guide-figure-1002]

_**Şekil 6:** Windows Server Yük Devretme Kümelemesi ile SIOS DataKeeper azure'da SAP ASCS/SCS yapılandırma_

### <a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>Yüksek kullanılabilirlik DBMS örneği
DBMS ayrıca tek bir iletişim noktası uçtaki bir SAP sistemiyle ' dir. Yüksek kullanılabilirlik çözümü kullanarak korumak gerekir. Şekil 7, Azure'da bir SQL Server Always On yüksek kullanılabilirlik çözümü gösterir, Windows Server Yük Devretme Kümelemesi ve Azure iç yük dengeleyici. SQL Server Always On kendi DBMS çoğaltma kullanılarak DBMS veri ve günlük dosyalarını çoğaltır. Bu durumda, paylaşılan diskler, tüm Kurulum sadeleştiren küme.

![Şekil 7: SQL Server Always On ile yüksek oranda kullanılabilir bir SAP DBMS örneği][sap-ha-guide-figure-2003]

_**Şekil 7:** SQL Server Always On ile yüksek oranda kullanılabilir bir SAP DBMS örneği_

Azure Resource Manager dağıtım modelini kullanarak azure'da SQL Server Kümelemesi hakkında daha fazla bilgi için şu makalelere bakın:

* [Always On kullanılabilirlik grubu Resource Manager'ı kullanarak Azure sanal Makineler'de el ile yapılandırın.][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]
* [Azure'da bir Azure iç yük dengeleyici her zaman açık kullanılabilirlik grubu için yapılandırma][virtual-machines-windows-portal-sql-alwayson-int-listener]

## <a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a> Uçtan uca yüksek kullanılabilirlik dağıtım senaryoları

### <a name="deployment-scenario-using-architectural-template-1"></a>Mimari şablon 1'i kullanarak dağıtım senaryosu

Şekil 8 için azure'da bir SAP NetWeaver-yüksek kullanılabilirlik mimarisi örneği gösterilmiştir **bir** SAP sistemine. Bu senaryo aşağıdaki ayarlamaları yapın:

- SAP ASCS/SCS örneği için ayrılmış bir küme kullanılır.
- Tek bir adanmış küme DBMS örneği için kullanılır.
- SAP uygulama sunucusu örnekleri kendi özel VM dağıtılır.

![Şekil 8: SAP ASCS/SCS ve DBMS için ayrılmış bir küme ile şablon 1, mimari yüksek kullanılabilirlik][sap-ha-guide-figure-2004]

_**Şekil 8:** Şablon 1 mimari yüksek kullanılabilirlik, SAP ASCS/SCS ve DBMS adanmış kümeleri_

### <a name="deployment-scenario-using-architectural-template-2"></a>Mimari şablon 2 kullanarak dağıtım senaryosu

Şekil 9 için azure'da bir SAP NetWeaver-yüksek kullanılabilirlik mimarisi örneği gösterir **bir** SAP sistemine. Bu senaryo aşağıdaki ayarlamaları yapın:

- Ayrılmış bir küme için kullanılan **hem** SAP ASCS/SCS örneği ve DBMS.
- SAP uygulama sunucusu örnekleri kendi özel VM dağıtılır.

![Şekil 9: Yüksek kullanılabilirlik mimari şablon 2 ASCS/SCS için ayrılmış bir küme ve ayrılmış bir küme için DBMS ile SAP][sap-ha-guide-figure-2005]

_**Şekil 9:** Yüksek kullanılabilirlik mimari şablon 2 ASCS/SCS için ayrılmış bir küme ve ayrılmış bir küme için DBMS ile SAP_

### <a name="deployment-scenario-using-architectural-template-3"></a>Mimari şablon 3'ü kullanarak dağıtım senaryosu

Şekil 10 gösteren bir SAP NetWeaver-yüksek kullanılabilirlik mimarisi örneği için azure'da **iki** ile SAP sistemlerini &lt;SID1&gt; ve &lt;SID2&gt;. Bu senaryo aşağıdaki ayarlamaları yapın:

- Ayrılmış bir küme için kullanılan **hem** SAP ASCS/SCS SID1 örneği *ve* SAP ASCS/SCS SID2 örneği (bir küme).
- Tek bir adanmış küme DBMS SID1 için kullanılır ve başka bir adanmış küme DBMS SID2 için kullanılır (iki kümeler).
- SAP uygulama sunucusu örneklerinin SID1 SAP sistemi için kendi özel VM'ler sahip.
- SAP uygulama sunucusu örneklerinin SID2 SAP sistemi için kendi özel VM'ler sahip.

![Şekil 10: Yüksek kullanılabilirlik mimari şablon 3, ayrılmış bir küme ile farklı ASCS/SCS örneği için SAP][sap-ha-guide-figure-6003]

_**Şekil 10:** Yüksek kullanılabilirlik mimari şablon 3, ayrılmış bir küme ile farklı ASCS/SCS örneği için SAP_

## <a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a> Altyapıyı hazırlama

### <a name="prepare-the-infrastructure-for-architectural-template-1"></a>Mimari şablon 1 için altyapıyı hazırlama
SAP için Azure Resource Manager şablonları, gerekli kaynakları dağıtımı kolaylaştırır.

Üç katmanlı şablonları Azure Kaynak Yöneticisi'nde de yüksek kullanılabilirlik senaryolarını gibi mimari şablon iki küme olan 1'de destekler. Her kümenin bir SAP tek SAP ASCS/SCS ve DBMS hata noktasıdır.

Bu makalede açıklanmaktadır örnek senaryosu için Azure Resource Manager şablonları edinebileceğiniz aşağıda verilmiştir:

* [Azure Market görüntüsü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
* [Özel görüntü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)

Mimari şablon 1 için altyapıyı hazırlamak için:

- Azure portalında, üzerinde **parametreleri** dikey penceresinde, **SYSTEMAVAILABILITY** kutusunda **HA**.

  ![Şekil 11: SAP yüksek kullanılabilirlik Azure Resource Manager parametreleri ayarlayın][sap-ha-guide-figure-3000]

_**Şekil 11:** SAP yüksek kullanılabilirlik Azure Resource Manager parametreleri ayarlayın_


  Şablonları oluşturun:

  * **Sanal makineler**:
    * SAP uygulama sunucusu sanal makineleri: <*SAPSystemSID*> - di - <*numarası*>
    * ASCS/SCS sanal makineleri küme: <*SAPSystemSID*> - ascs - <*numarası*>
    * DBMS küme: <*SAPSystemSID*> - db - <*numarası*>

  * **Ağ kartları ilişkili IP adreslerine sahip tüm sanal makineler için**:
    * <*SAPSystemSID*> - NIC - di - <*numarası*>
    * <*SAPSystemSID*> - NIC - ascs - <*numarası*>
    * <*SAPSystemSID*> - NIC - db - <*numarası*>

  * **Azure depolama hesapları**

  * **Kullanılabilirlik grupları** için:
    * SAP uygulama sunucusu sanal makineleri: <*SAPSystemSID*> - avset - dı
    * SAP ASCS/SCS sanal makineleri küme: <*SAPSystemSID*> - avset - ascs
    * DBMS küme sanal makineler: <*SAPSystemSID*> - avset - db

  * **Azure iç yük dengeleyici**:
    * IP adresi ve ASCS/SCS örneği için tüm bağlantı noktaları ile <*SAPSystemSID*> - lb - ascs
    * SQL Server DBMS ve IP adresi için tüm bağlantı noktaları ile <*SAPSystemSID*> - lb - db

  * **Ağ güvenlik grubu**: <*SAPSystemSID*> - nsg - ascs-0  
    * Açık bir dış Uzak Masaüstü Protokolü (RDP) bağlantı noktasına sahip <*SAPSystemSID*> - ascs - 0 sanal makine

> [!NOTE]
> Tüm IP adresleri Azure iç yük dengeleyicileri ve ağ kartları **dinamik** varsayılan olarak. Bunları değiştirmek **statik** IP adresleri. Biz, makalenin sonraki bölümlerinde bunun nasıl yapılacağını açıklar.
>
>

### <a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a> Üretimde kullanmak için kurumsal ağ bağlantısı (şirket içi) ile sanal makineleri dağıtma
Üretim SAP sistemlerini için Azure sanal makineleri dağıtma [kurumsal ağ bağlantısı (şirket içi)] [ planning-guide-2.2] Azure siteden siteye VPN veya Azure ExpressRoute kullanarak.

> [!NOTE]
> Azure sanal ağı örneğinizin kullanabilirsiniz. Sanal ağ ve alt ağ zaten oluşturduğunuz hazırlanmış ve.
>
>

1. Azure portalında, üzerinde **parametreleri** dikey penceresinde, **NEWOREXISTINGSUBNET** kutusunda **mevcut**.
2. İçinde **SUBNETID** kutusunda, hazırlanan Azure alt ağınızı Azure sanal makinelerinizi dağıtmak planladığınız Subnetıd tam dizesi ekleyin.
3. Tüm Azure ağ alt ağların bir listesini almak için bu PowerShell komutunu çalıştırın:

   ```PowerShell
   (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets
   ```

   **Kimliği** alan gösterir **SUBNETID**.
4. Tüm listesini almak için **SUBNETID** değerleri, bu PowerShell komutunu çalıştırın:

   ```PowerShell
   (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets.Id
   ```

   **SUBNETID** şöyle görünür:

   ```
   /subscriptions/<SubscriptionId>/resourceGroups/<VPNName>/providers/Microsoft.Network/virtualNetworks/azureVnet/subnets/<SubnetName>
   ```

### <a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a> Test ve tanıtım için yalnızca bulutta yer alan SAP örnek dağıtın
Yalnızca bulutta yer alan dağıtım modelinde, yüksek kullanılabilirlik SAP sistemine dağıtabilirsiniz. Bu tür bir dağıtım, öncelikle tanıtım ve test kullanım durumları için kullanışlıdır. Üretimi kullanım örnekleri için uygun değildir.

- Azure portalında, üzerinde **parametreleri** dikey penceresinde, **NEWOREXISTINGSUBNET** kutusunda **yeni**. Bırakın **SUBNETID** boş.

  SAP Azure Resource Manager şablonu otomatik olarak Azure sanal ağını ve alt ağ oluşturur.

> [!NOTE]
> Ayrıca Active Directory ve DNS için aynı Azure sanal ağı örneğinde en az bir özel sanal makine dağıtmak gerekir. Şablon, bu sanal makineler oluşturmaz.
>
>


### <a name="prepare-the-infrastructure-for-architectural-template-2"></a>Mimari şablon 2 için altyapıyı hazırlama

Bu Azure Resource Manager şablonu için SAP, SAP mimari şablon 2 için gerekli altyapı kaynaklarının dağıtımını basitleştirin yardımcı olmak için kullanabilirsiniz.

Bu dağıtım senaryosu için Azure Resource Manager şablonları edinebileceğiniz aşağıda verilmiştir:

* [Azure Market görüntüsü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged)  
* [Özel görüntü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged)


### <a name="prepare-the-infrastructure-for-architectural-template-3"></a>Mimari şablon 3 için altyapıyı hazırlama

Altyapıyı hazırlama ve yapılandırma için SAP **çoklu SID**. Örneğin, ek bir SAP ASCS/SCS örneğine ekleyebilirsiniz. bir *mevcut* küme yapılandırması. Daha fazla bilgi için [Azure Resource Manager'da bir SAP çoklu SID yapılandırmasını oluşturmak için var olan bir küme yapılandırmasını ek bir SAP ASCS/SCS örneğine yapılandırma][sap-ha-multi-sid-guide].

Yeni bir çoklu SID küme oluşturmak istiyorsanız, çoklu SID kullanabileceğiniz [hızlı başlangıç şablonları GitHub üzerinde](https://github.com/Azure/azure-quickstart-templates).
Yeni bir çoklu SID kümesi oluşturmak için aşağıdaki üç şablonu dağıtmanız gerekebilir:

* [ASCS/SCS şablonu](#ASCS-SCS-template)
* [Veritabanı şablonu](#database-template)
* [Uygulama sunucuları şablonu](#application-servers-template)

Aşağıdaki bölümlerde, şablonlar ve şablonlar, sağlamanız gereken parametreler hakkında daha fazla ayrıntı sahip.

#### <a name="ASCS-SCS-template"></a> ASCS/SCS şablonu

ASCS/SCS şablonu birden fazla ASCS/SCS örneği barındıran bir Windows Server Yük devretme kümesi oluşturmak için kullanabileceğiniz iki sanal makine dağıtır.

ASCS/SCS çoklu SID şablonu, ayarlamak için [ASCS/SCS çoklu SID şablon][sap-templates-3-tier-multisid-xscs-marketplace-image], aşağıdaki parametreler için değerleri girin:

  - **Kaynak önek**.  Dağıtım sırasında oluşturulan tüm kaynakları önek olarak eklemek için kullanılan kaynak öneki ayarlayın. Kaynakları tek bir SAP sistemine ait olmadığından kaynak öneki tek SAP sistemine SID'si değil.  Önek arasında olmalıdır **üç ve altı karakter**.
  - **Yığın türü**. SAP sistemine yığın türünü seçin. Yığın türüne bağlı olarak, Azure Load Balancer (ABAP ya da yalnızca Java) veya iki (ABAP + Java) özel IP adresi başına SAP sistemine sahiptir.
  -  **İşletim sistemi türü**. Sanal makinelerin işletim sistemi seçin.
  -  **SAP sistemi sayısı**. Bu kümede yüklemek istediğiniz SAP sistemlerini sayısını seçin.
  -  **Sistem kullanılabilirliği**. Seçin **HA**.
  -  **Yönetici kullanıcı adı ve yönetici parolası**. Makinede oturum açmak için kullanılan yeni bir kullanıcı oluşturun.
  -  **Yeni veya var olan bir alt ağa**. Yeni sanal ağ ve alt ağı oluşturulması gereken ya da var olan bir alt ağ kullanılması gereken ayarlayın. Şirket içi ağınıza bağlı bir sanal ağınız zaten varsa, seçin **mevcut**.
  -  **Alt ağ kimliği**. Tanımlanan bir alt ağa sahip olduğunuz mevcut bir Vnet'te VM dağıtmak istiyorsanız, VM atanmalıdır belirli bir alt ağ kimliği adı için. Kimliği genellikle şöyle görünür: /subscriptions/ <*abonelik kimliği*> /resourceGroups/ <*kaynak grubu adı*> /providers/Microsoft.Network/virtualNetworks/ < *sanal ağ adı*> /subnets/ <*alt ağ adı*>

Şablon birden çok SAP sistemlerini destekleyen bir Azure Load Balancer örneği dağıtır.

- ASCS örnekleri, örnek numarası 00, 10, 20 yapılandırılır...
- SCS örnekleri, örnek numarası, 01 11, 21 yapılandırılır...
- ASCS kuyruğa çoğaltma sunucusuna (Ağıranlar) (yalnızca Linux) örnekleri, örnek numarası 02, 12, 22 yapılandırılır...
- SCS Ağıranlar (yalnızca Linux) örnekleri, örnek numarası 03, 13, 23 yapılandırılır...

1 (2 Linux için) yük dengeleyici içeren VIP(s) ASCS/SCS için 1 x VIP ve 1 x VIP için Ağıranlar (yalnızca Linux).

Aşağıdaki liste, tüm Yük Dengeleme kuralları (burada x Örneğin, 1, 2, 3... SAP sistemine sayısıdır) içerir:
- Her SAP sistemi için Windows özel bağlantı noktaları: 445, 5985
- ASCS bağlantı noktaları (örnek numarasını x0): 32 x 0, 36 x 0, 39 x 0, 81 x 0, 5 x 013, 5 x 014, 5 x 016
- SCS bağlantı noktaları (örnek numarasını x1): 32 x 1, 33 x 1, 39 x 1, 81 x 1, 5 x 113, 5 x 114, 5 x 116
- (Örnek numarasını x2) Linux'ta ASCS Ağıranlar bağlantı noktaları: 33 x 2, 5 x 213, 5 x 214, 5 x 216
- (Örnek numarasını x3) Linux'ta SCS Ağıranlar bağlantı noktaları: 33 x 3, 5 x 313, 5 x 314, 5 x 316

Yük Dengeleyici (burada x Örneğin, 1, 2, 3... SAP sistemine sayısıdır) aşağıdaki araştırma bağlantı noktalarını kullanacak şekilde yapılandırılır:
- ASCS/SCS iç yük dengeleyici yoklama bağlantı noktası: 620x0
- İç Ağıranlar dengeleyici yoklama bağlantı noktası (yalnızca Linux) yükle: 621 x 2

#### <a name="database-template"></a> Veritabanı şablonu

Veritabanı şablonu, bir veya iki tek SAP sistemine ilişkisel veritabanı yönetim sistemi (RDBMS) yüklemek için kullanabileceğiniz sanal makinelerin dağıtır. Örneğin, beş SAP sistemlerini bir ASCS/SCS şablonu dağıtırsanız, beş kez bu şablonu dağıtmak gerekir.

Veritabanı çoklu SID Şablonu ' ayarlamak için [veritabanı çoklu SID şablonu][sap-templates-3-tier-multisid-db-marketplace-image], aşağıdaki parametreler için değerleri girin:

- **SAP sistem kimliği**. Yüklemek istediğiniz SAP sistemine SAP sistemi Kimliğini girin. Kimlik ön eki olarak dağıtılan kaynaklar için kullanılır.
- **İşletim sistemi türü**. Sanal makinelerin işletim sistemi seçin.
- **DbType**. Kümeye yüklemek istediğiniz veritabanı türünü seçin. Seçin **SQL** Microsoft SQL Server yüklemek istiyorsanız. Seçin **HANA** sanal makinelerde SAP HANA yüklemeyi planlıyorsanız. Doğru işletim sistemi türü seçtiğinizden emin olun: seçin **Windows** SQL ve HANA için bir Linux dağıtımı seçin. Sanal makinelere bağlanan bir Azure Load Balancer, seçili veritabanı türü destekleyecek şekilde yapılandırılır:
  * **SQL**. Yük Dengeleyici, Yük Dengeleme bağlantı noktası 1433 olur. SQL Server Always On ayarlarınızı bu bağlantı noktasını kullandığınızdan emin olun.
  * **HANA**. Yük Dengeleyici Yük Dengeleme 35015 ve 35017 bağlantı noktaları olur. SAP HANA ile örnek numarasını yüklediğinizden emin olun **50**.
  Yük Dengeleyici, yoklama bağlantı noktası 62550 kullanacaktır.
- **SAP sistemi boyutu**. Yeni sisteme sağlayacak SAP sayısını ayarlayın. Sistem gerektirecek kaç SAP değil eminseniz, SAP teknoloji iş ortağı veya sistem Entegratörü isteyin.
- **Sistem kullanılabilirliği**. Seçin **HA**.
- **Yönetici kullanıcı adı ve yönetici parolası**. Makinede oturum açmak için kullanılan yeni bir kullanıcı oluşturun.
- **Alt ağ kimliği**. ASCS/SCS şablon dağıtımının bir parçası ASCS/SCS şablon dağıtımı sırasında kullanılan alt ağ kimliği veya oluşturulmuş alt ağ Kimliğini girin.

#### <a name="application-servers-template"></a> Uygulama sunucuları şablonu

Uygulama sunucuları şablonu iki veya daha fazla SAP uygulama sunucusu örnekleri olarak bir SAP sistemi için kullanılabilir sanal makinelerin dağıtır. Örneğin, beş SAP sistemlerini bir ASCS/SCS şablonu dağıtırsanız, beş kez bu şablonu dağıtmak gerekir.

Uygulama sunucuları çoklu SID Şablonu ' ayarlamak için [uygulama sunucuları çoklu SID şablonu][sap-templates-3-tier-multisid-apps-marketplace-image], aşağıdaki parametreler için değerleri girin:

  -  **SAP sistem kimliği**. Yüklemek istediğiniz SAP sistemine SAP sistemi Kimliğini girin. Kimlik ön eki olarak dağıtılan kaynaklar için kullanılır.
  -  **İşletim sistemi türü**. Sanal makinelerin işletim sistemi seçin.
  -  **SAP sistemi boyutu**. Yeni sisteme sağlayacak SAP sayısı. Sistem gerektirecek kaç SAP değil eminseniz, SAP teknoloji iş ortağı veya sistem Entegratörü isteyin.
  -  **Sistem kullanılabilirliği**. Seçin **HA**.
  -  **Yönetici kullanıcı adı ve yönetici parolası**. Makinede oturum açmak için kullanılan yeni bir kullanıcı oluşturun.
  -  **Alt ağ kimliği**. ASCS/SCS şablon dağıtımının bir parçası ASCS/SCS şablon dağıtımı sırasında kullanılan alt ağ kimliği veya oluşturulmuş alt ağ Kimliğini girin.


### <a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a> Azure sanal ağı
Bizim örneğimizde, Azure sanal ağının adres alanı 10.0.0.0/16 ' dir. Adlı bir alt ağ yok **alt**, 10.0.0.0/24 adres aralığına sahip. Tüm sanal makineler ve iç yük Dengeleyiciler bu sanal ağ içinde dağıtılmıştır.

> [!IMPORTANT]
> Ağ ayarları konuk işletim sistemi içinde herhangi bir değişiklik yok. Bu IP adresleri, DNS sunucuları ve alt ağ içerir. Azure'da tüm ağ ayarlarını yapılandırın. Dinamik konak Yapılandırma Protokolü (DHCP) hizmeti ayarlarınızı yayar.
>
>

### <a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a> DNS IP adresleri

Gerekli DNS IP adresi ayarlamak için aşağıdaki adımları uygulayın.

1. Azure portalında, üzerinde **DNS sunucuları** dikey penceresinde emin olun, sanal ağınızın **DNS sunucuları** seçeneği **özel DNS**.
2. Sahip olduğunuz ağ türüne göre ayarlarınızı seçin. Daha fazla bilgi için aşağıdaki kaynaklara bakın:
   * [Kurumsal ağ bağlantısı (şirket içi)][planning-guide-2.2]: Şirket içi DNS sunucularının IP adreslerini ekleyin.  
   Azure'da çalışan sanal makineleri şirket içi DNS sunucularına genişletebilirsiniz. Bu senaryoda DNS hizmeti çalıştıran Azure sanal makinelerin IP adreslerini ekleyebilirsiniz.
   * Azure'da yalıtılmış dağıtımlar için: Bir DNS sunucusu olarak hizmet veren aynı sanal ağ örneğinde ek bir sanal makine dağıtın. DNS hizmeti çalıştırmak için ayarladığınız Azure sanal makine IP adreslerini ekleyin.

   ![Şekil 12: Azure sanal ağı için DNS sunucuları yapılandırma][sap-ha-guide-figure-3001]

   _**Şekil 12:** Azure sanal ağı için DNS sunucuları yapılandırma_

   > [!NOTE]
   > DNS sunucularının IP adreslerini değiştirirseniz, değişikliği uygulamak ve yeni DNS sunucularını yaymak için bir Azure sanal makineleri yeniden başlatmanız gerekir.
   >
   >

Bizim örneğimizde, DNS hizmeti yüklenir ve bu Windows sanal makinelerinde yapılandırılır:

| Sanal makine rolü | Sanal makine konak adı | Ağ kartı adı | Statik IP adresi |
| --- | --- | --- | --- |
| İlk DNS sunucusu |domcontr-0 |pr1 NIC domcontr 0 |10.0.0.10 |
| İkinci bir DNS sunucusu |domcontr-1 |pr1 NIC domcontr 1 |10.0.0.11 |

### <a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a> Ana bilgisayar adları ve DBMS kümelenmiş örneği ve SAP ASCS/SCS kümelenmiş örneği için statik IP adresleri

Şirket içi dağıtımlarda, bu ayrılmış konak adları ve IP adresleri gerekir:

| Sanal ana bilgisayar adı rolü | Sanal ana bilgisayar adı | Sanal bir statik IP adresi |
| --- | --- | --- |
| SAP ASCS/SCS ilk küme sanal ana bilgisayar adı (için küme yönetimi) |pr1 ascs VIR |10.0.0.42 |
| SAP ASCS/SCS örneği sanal ana bilgisayar adı |pr1 ascs sap |10.0.0.43 |
| SAP DBMS ikinci küme sanal ana bilgisayar adı (küme yönetimi) |pr1-dbms-vir |10.0.0.32 |

Kümeyi oluşturduğunuzda, sanal ana bilgisayar adlarını oluşturma **pr1 ascs VIR** ve **pr1 dbms VIR** ve ilişkili IP adreslerini kümesini yönetme. Bunun nasıl yapılacağı hakkında daha fazla bilgi için bkz. [toplamak küme düğümleri bir küme yapılandırmasında][sap-ha-guide-8.12.1].

Diğer iki sanal ana bilgisayar adı, el ile oluşturabilirsiniz **pr1 ascs sap** ve **pr1 dbms sap**ve ilişkili IP adresleri, DNS sunucusu. Bu kaynaklar kümelenmiş SAP ASCS/SCS örneği ile kümelenmiş DBMS örneği kullanın. Bunun nasıl yapılacağı hakkında daha fazla bilgi için bkz. [kümelenmiş bir SAP ASCS/SCS örneği için bir sanal ana bilgisayar adı oluşturma][sap-ha-guide-9.1.1].

### <a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a> SAP sanal makineleri için statik IP adresi ayarlama
Kümedeki sanal makinelerin dağıttıktan sonra tüm sanal makineler için statik IP adresleri kümesi gerekir. Bunu yapmak, Azure sanal ağ yapılandırması ve konuk işletim sistemi içinde değil.

1. Azure portalında **kaynak grubu** > **ağ kartı** > **ayarları** > **IPadresi**.
2. Üzerinde **IP adresleri** dikey altında **atama**seçin **statik**. İçinde **IP adresi** kutusunda, kullanmak istediğiniz IP adresini girin.

   > [!NOTE]
   > Ağ kartının IP adresini değiştirirseniz, değişikliği uygulamak için Azure sanal makineleri yeniden başlatmanız gerekir.  
   >
   >

   ![Şekil 13: Statik IP adresleri her sanal makinenin ağ kartı için ayarlayın][sap-ha-guide-figure-3002]

   _**Şekil 13:** Statik IP adresleri her sanal makinenin ağ kartı için ayarlayın_

   Tüm sanal makineler için Active Directory/DNS hizmetiniz için kullanmak istediğiniz sanal makineleri dahil olan tüm ağ arabirimlerinin, bu adımı yineleyin.

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

### <a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a> Azure iç yük dengeleyici için statik bir IP adresi ayarlama

SAP Azure Resource Manager şablonu SAP ASCS/SCS örneği küme ve DBMS küme için kullanılan bir Azure iç yük dengeleyici oluşturur.

> [!IMPORTANT]
> SAP ASCS/SCS sanal ana bilgisayar adı, IP adresi SAP ASCS/SCS iç yük dengeleyici IP adresi ile aynıdır: **pr1 lb ascs**.
> DBMS adı sanal IP adresi DBMS iç yük dengeleyici IP adresi ile aynıdır: **pr1 lb dbms**.
>
>

Azure iç yük dengeleyici için statik bir IP adresi ayarlamak için:

1. İç yük dengeleyici IP adresi ilk dağıtım ayarlar **dinamik**. Azure portalında, üzerinde **IP adresleri** dikey altında **atama**seçin **statik**.
2. İç yük dengeleyicinin IP adresi ayarlama **pr1 lb ascs** SAP ASCS/SCS örneği sanal ana bilgisayar adını IP adresine.
3. İç yük dengeleyicinin IP adresi ayarlama **pr1 lb dbms** DBMS örneğinin sanal ana bilgisayar adını IP adresine.

   ![Şekil 14: SAP ASCS/SCS örneği için iç yük dengeleyici için statik IP adresi ayarlama][sap-ha-guide-figure-3003]

   _**Şekil 14:** SAP ASCS/SCS örneği için iç yük dengeleyici için statik IP adresi ayarlama_

Bizim örneğimizde, bu statik IP adresine sahip iki Azure iç yük Dengeleyiciler sunuyoruz:

| Azure iç yük dengeleyici rolü | Azure iç yük dengeleyici adı | Statik IP adresi |
| --- | --- | --- |
| SAP ASCS/SCS iç yük dengeleyici örneği |pr1 lb ascs |10.0.0.43 |
| SAP DBMS iç yük dengeleyici |pr1 lb dbms |10.0.0.33 |


### <a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a> ASCS/SCS Yük Dengeleme kuralları Azure iç yük dengeleyici için varsayılan

SAP Azure Resource Manager şablonu ihtiyacınız bağlantı noktaları oluşturur:
* Varsayılan örnek sayısı ile ABAP ASCS örneği **00**
* Varsayılan örnek sayısı ile bir Java SCS örneği **01**

SAP ASCS/SCS örneğinizin yüklediğinizde, varsayılan örnek numarasını kullanmalıdır **00** ABAP ASCS örneğinizi ve varsayılan örnek sayısı için **01** , Java SCS örneği için.

Ardından, gerekli iç Yük Dengeleme SAP NetWeaver bağlantı noktaları için uç noktaları oluşturun.

Gerekli iç Yük Dengeleme uç noktaları oluşturmak için ilk olarak, bu Yük Dengeleme uç SAP NetWeaver ABAP ASCS bağlantı noktaları oluşturun:

| Hizmet/Yük Dengeleme kuralı adı | Varsayılan bağlantı noktası numaraları | Somut için bağlantı noktalarını (ASCS örneği ile örnek numarasını 00) (Ağıranlar 10 ile) |
| --- | --- | --- |
| Sıraya alma sunucu / *lbrule3200* |32 <*Örneksayısı*> |3200 |
| ABAP ileti sunucusu / *lbrule3600* |36 <*Örneksayısı*> |3600 |
| İç ABAP ileti / *lbrule3900* |39 <*Örneksayısı*> |3900 |
| Sunucu HTTP ileti / *Lbrule8100* |81 <*Örneksayısı*> |8100 |
| SAP başlangıç hizmeti ASCS HTTP / *Lbrule50013* |5 <*Örneksayısı*> 13 |50013 |
| SAP başlangıç hizmeti ASCS HTTPS / *Lbrule50014* |5 <*Örneksayısı*> 14 |50014 |
| Sıraya alma çoğaltma / *Lbrule50016* |5 <*Örneksayısı*> 16 |50016 |
| SAP başlangıç hizmeti Ağıranlar HTTP *Lbrule51013* |5 <*Örneksayısı*> 13 |51013 |
| SAP başlangıç hizmeti Ağıranlar HTTP *Lbrule51014* |5 <*Örneksayısı*> 14 |51014 |
| RM win *Lbrule5985* | |5985 |
| Dosya Paylaşımı *Lbrule445* | |445 |

_**Tablo 1:** Bağlantı noktası numaralarını SAP NetWeaver ABAP ASCS örnekleri_

Ardından, bu Yük Dengeleme uç SAP NetWeaver Java SCS bağlantı noktaları oluşturun:

| Hizmet/Yük Dengeleme kuralı adı | Varsayılan bağlantı noktası numaraları | Somut için bağlantı noktalarını (SCS örneği ile örnek numarasını 01) (Ağıranlar 11) |
| --- | --- | --- |
| Sıraya alma sunucu / *lbrule3201* |32 <*Örneksayısı*> |3201 |
| Ağ Geçidi sunucusu / *lbrule3301* |33 <*Örneksayısı*> |3301 |
| Java ileti sunucusu / *lbrule3900* |39 <*Örneksayısı*> |3901 |
| Sunucu HTTP ileti / *Lbrule8101* |81 <*Örneksayısı*> |8101 |
| SAP başlangıç hizmeti SCS HTTP / *Lbrule50113* |5 <*Örneksayısı*> 13 |50113 |
| SAP başlangıç hizmeti SCS HTTPS / *Lbrule50114* |5 <*Örneksayısı*> 14 |50114 |
| Sıraya alma çoğaltma / *Lbrule50116* |5 <*Örneksayısı*> 16 |50116 |
| SAP başlangıç hizmeti Ağıranlar HTTP *Lbrule51113* |5 <*Örneksayısı*> 13 |51113 |
| SAP başlangıç hizmeti Ağıranlar HTTP *Lbrule51114* |5 <*Örneksayısı*> 14 |51114 |
| RM win *Lbrule5985* | |5985 |
| Dosya Paylaşımı *Lbrule445* | |445 |

_**Tablo 2:** Bağlantı noktası numaralarını SAP NetWeaver Java SCS örnekleri_

![Şekil 15: ASCS/SCS Yük Dengeleme kuralları Azure iç yük dengeleyici için varsayılan][sap-ha-guide-figure-3004]

_**Şekil 15:** ASCS/SCS Yük Dengeleme kuralları Azure iç yük dengeleyici için varsayılan_

Yük dengeleyicinin IP adresi ayarlama **pr1 lb dbms** DBMS örneğinin sanal ana bilgisayar adını IP adresine.

### <a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a> ASCS/SCS varsayılan Yük Dengeleme kuralları Azure iç yük dengeleyici için değiştirin

SAP ASCS veya SCS örneği için farklı bir sayı kullanmak istiyorsanız, varsayılan değerleri adları ve bağlantı noktalarının değerlerini değiştirmeniz gerekir.

1. Azure portalında  **< *SID*> - lb - ascs yük dengeleyici** > **Yük Dengeleme kuralları**.
2. Tüm Yük Dengeleme SAP ASCS veya SCS örneğine ait kuralları için bu değerleri değiştirin:

   * Ad
   * Bağlantı noktası
   * Arka uç bağlantı noktası

   Örneğin, varsayılan ASCS örnek numarasını 00-31 olarak değiştirmek isterseniz, Tablo 1'de listelenen tüm bağlantı noktaları için değişiklikler yapmanız gerekir.

   İşte bir örnek bağlantı noktası için bir güncelleştirme *lbrule3200*.

   ![Şekil 16: ASCS/SCS varsayılan Yük Dengeleme kuralları Azure iç yük dengeleyici için değiştirin][sap-ha-guide-figure-3005]

   _**Şekil 16:** ASCS/SCS varsayılan Yük Dengeleme kuralları Azure iç yük dengeleyici için değiştirin_

### <a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a> Windows sanal makineleri etki alanına ekleyin

Sanal makinelere statik IP adresi atadıktan sonra sanal makinelerin etki alanına ekleyin.

![Şekil 17: Bir sanal makine bir etki alanına ekleme][sap-ha-guide-figure-3006]

_**Şekil 17:** Bir sanal makine bir etki alanına ekleme_

### <a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a> SAP ASCS/SCS örneği her iki küme düğümlerinde kayıt defteri girdilerini ekleyin

Azure Load Balancer iç yük dengeleyici bağlantıları için belirlenen bir süre boyunca boşta olduğunda kapanır bağlantılar (boşta zaman aşımı) süresi vardır. SAP iş işlemlerinde SAP sıraya iletişim örnekleri açık bağlantıları ilk sıraya alma/sıradan çıkarma gönderilmesi gerekiyor isteği olarak işleyin. Bu bağlantılar genellikle kadar iş sürecinin yerleşik kalır veya sıraya alma işlemi yeniden başlatır. Ancak, bağlantı belirli bir süre için boşta kalırsa Azure iç yük dengeleyici bağlantıları kapatır. SAP iş işlemi, artık mevcut değilse, sıraya alma işlemi için bağlantıyı yeniden kurar çünkü bu bir sorun değildir. Bu etkinlikler SAP işlemleri Geliştirici izlemelerinde belirtilmiştir, ancak izlemeleri çok miktarda ek içeriği oluştururlar. TCP/IP'yi değiştirmek için iyi bir fikirdir `KeepAliveTime` ve `KeepAliveInterval` her iki küme düğümlerinde. Bu makalenin sonraki bölümlerinde açıklanan SAP profili parametrelerinden TCP/IP'yi parametrelerle değişiklikleri birleştirin.

SAP ASCS/SCS örneği her iki küme düğümlerinde kayıt defteri girdileri eklemek için ilk olarak, bu Windows kayıt defteri girişleri hem Windows Küme düğümlerinde SAP ASCS/SCS ekleyin:

| Yol | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Değişken adı |`KeepAliveTime` |
| Değişken türü |REG_DWORD (ondalık) |
| Değer |120000 |
| Belgelerinin bağlantısı |[https://technet.microsoft.com/library/cc957549.aspx](https://technet.microsoft.com/library/cc957549.aspx) |

_**Tablo 3:** İlk TCP/IP'yi parametre değiştirme_

Ardından, bu Windows kayıt defteri girdileri SAP ASCS/SCS için hem Windows Küme düğümlerinde ekleyin:

| Yol | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Değişken adı |`KeepAliveInterval` |
| Değişken türü |REG_DWORD (ondalık) |
| Değer |120000 |
| Belgelerinin bağlantısı |[https://technet.microsoft.com/library/cc957548.aspx](https://technet.microsoft.com/library/cc957548.aspx) |

_**Tablo 4:** İkinci TCP/IP'yi parametre değiştirme_

**Değişiklikleri uygulamak için her iki küme düğümlerini yeniden başlatmanız**.

### <a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a> SAP ASCS/SCS örneği için Windows Server Yük Devretme Kümelemesi kümesi ayarlama

SAP ASCS/SCS örneği için bir Windows Server Yük Devretme Kümelemesi küme ayarlama, bu görevleri kapsar:

- Küme düğümleri bir küme yapılandırmasında toplama
- Bir küme dosya paylaşım tanığı yapılandırma

#### <a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a> Küme düğümleri bir küme yapılandırmasında Topla

1. Rol Ekle ve Özellikler Sihirbazı'nda Yük Devretme Kümelemesi her iki küme düğümlerine ekleyin.
2. Yük devretme kümesi, yük devretme kümesi Yöneticisi'ni kullanarak ayarlayın. Yük Devretme Kümesi Yöneticisi'nde **küme oluşturma**ve ardından yalnızca ilk küme, düğümü A. adını ekleyin İkinci düğümü henüz eklemeyin; İkinci düğümü daha sonraki bir adımda ekleyeceksiniz.

   ![Şekil 18: İlk küme düğümüne sunucu veya sanal makine adını ekleyin][sap-ha-guide-figure-3007]

   _**Şekil 18:** İlk küme düğümüne sunucu veya sanal makine adını ekleyin_

3. Küme ağ adı (sanal ana bilgisayar adı) girin.

   ![Şekil 19: Küme adı girin][sap-ha-guide-figure-3008]

   _**Şekil 19:** Küme adı girin_

4. Kümeyi oluşturduktan sonra küme doğrulama testi çalıştırın.

   ![Şekil 20: Küme doğrulama denetimini Çalıştır][sap-ha-guide-figure-3009]

   _**Şekil 20:** Küme doğrulama denetimini Çalıştır_

   Bu noktada işleminde disklerle ilgili tüm uyarılar yoksayabilirsiniz. Dosya paylaşım tanığı ve SIOS diskleri daha sonra paylaşılan ekleyeceksiniz. Bu aşamada, bir çekirdek sahip hakkında endişelenmeniz gerekmez.

   ![Şekil 21: Hiçbir çekirdek diski bulundu][sap-ha-guide-figure-3010]

   _**Şekil 21:** Hiçbir çekirdek diski bulundu_

   ![Şekil 22: Çekirdeği küme kaynağı yeni bir IP adresi gerekiyor][sap-ha-guide-figure-3011]

   _**Şekil 22:** Çekirdeği küme kaynağı yeni bir IP adresi gerekiyor_

5. Çekirdeği Küme hizmetinin IP adresini değiştirin. Çekirdeği Küme hizmetinin IP adresini değiştirmek kadar küme sanal makine düğümlerinin sunucusunun IP adresini işaret başlatılamıyor. Bunu yapmak **özellikleri** çekirdek küme hizmetin IP kaynak sayfası.

   Örneğin, bir IP adresi atamak ihtiyacımız (Bizim örneğimizde **10.0.0.42**) küme sanal ana bilgisayar adının **pr1 ascs VIR**.

   ![Şekil 23: Özellikler iletişim kutusunda, IP adresini değiştirme][sap-ha-guide-figure-3012]

   _**Şekil 23:** İçinde **özellikleri** iletişim kutusunda, IP adresini değiştirme_

   ![Şekil 24: Küme için ayrılmış IP adresi atama][sap-ha-guide-figure-3013]

   _**Şekil 24:** Küme için ayrılmış IP adresi atama_

6. Küme sanal ana bilgisayar adı çevrimiçi duruma getirin.

   ![Şekil 25: Küme Çekirdek hizmeti çalışır durumda ve çalıştığından ve doğru IP adresi][sap-ha-guide-figure-3014]

   _**Şekil 25:** Küme Çekirdek hizmeti çalışır durumda ve çalıştığından ve doğru IP adresi_

7. İkinci küme düğümüne ekleyin.

   Çekirdeği Küme hizmeti çalışır duruma geldikten sonra ikinci küme düğümüne ekleyebilirsiniz.

   ![Şekil 26: İkinci küme düğümü Ekle][sap-ha-guide-figure-3015]

   _**Şekil 26:** İkinci küme düğümü Ekle_

8. İkinci küme düğümünde konak için bir ad girin.

   ![Şekil 27: İkinci küme düğümü ana bilgisayar adı girin][sap-ha-guide-figure-3016]

   _**Şekil 27:** İkinci küme düğümü ana bilgisayar adı girin_

   > [!IMPORTANT]
   > Olduğundan emin olun **tüm uygun Depolama alanlarını kümeye ekleyin** onay kutusu **değil** seçili.  
   >
   >

   ![Şekil 28: Onay kutusunu seçin][sap-ha-guide-figure-3017]

   _**Şekil 28:** Yapmak **değil** onay kutusunu işaretleyin_

   Çekirdek ve diskler hakkında uyarıları gözardı edebilirsiniz. Çekirdek ayarlayın ve diski daha sonra açıklandığı paylaş [SAP ASCS/SCS Küme Paylaşımı diski için SIOS DataKeeper Cluster Edition yükleme][sap-ha-guide-8.12.3].

   ![Şekil 29: Disk çekirdek hakkında uyarılar yoksay][sap-ha-guide-figure-3018]

   _**Şekil 29:** Disk çekirdek hakkında uyarılar yoksay_


#### <a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a> Küme dosya paylaşımı tanığını yapılandırma

Bir küme dosya paylaşım tanığı yapılandırma, bu görevleri kapsar:

- Bir dosya paylaşımı oluşturma
- Yük Devretme Kümesi Yöneticisi'nde dosya paylaşım tanığı çekirdek ayarlama

##### <a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a> Dosya paylaşımı oluşturma

1. Dosya paylaşım tanığı yerine bir çekirdek disk seçin. Bu seçenek, SIOS DataKeeper destekler.

   Bu makaledeki örneklerde, Azure'da çalışan Active Directory/DNS sunucusunda dosya paylaşımı tanığı açıktır. Dosya paylaşım tanığı olarak adlandırılır **domcontr 0**. Bir VPN bağlantısı (siteden siteye VPN veya Azure ExpressRoute) aracılığıyla azure'a yapılandırmış olmanız çünkü, Active Directory/Hizmet şirket içinde ve bir dosyayı çalıştırmak uygun olmayan DNS Tanık paylaşın.

   > [!NOTE]
   > Yalnızca şirket içi Active Directory/DNS hizmetiniz çalıştırıyorsa, Active Directory/DNS Windows işletim sisteminde şirket içinde çalışan, dosya paylaşım tanığı yapılandırmayın. Azure ve Active Directory/DNS şirket içinde çalışan küme düğümleri arasındaki ağ gecikme süresi, çok büyük ve bağlantı sorunlarına neden olabilir. Küme düğümü yakın çalıştıran Azure sanal makinesinde dosya paylaşım tanığı yapılandırma emin olun.  
   >
   >

   Çekirdek sürücüde en az 1024 MB boş alan gerekir. 2.048 MB boş alan çekirdek sürücüsü için öneririz.

2. Küme adı nesnesi ekleyin.

   ![Şekil 30: Küme adı nesnesi için paylaşım izinleri atama][sap-ha-guide-figure-3019]

   _**Şekil 30:** Küme adı nesnesi için paylaşım izinleri atama_

   İzinleri küme adı nesnesi için paylaşımdaki verilere değiştirme yetkisi eklemeyi unutmayın (Bizim örneğimizde **pr1 ascs VIR$**).

3. Küme adı nesnesi listeye eklemek için seçin **Ekle**. Bilgisayar nesnelerini şekil 31'de gösterilen ek olarak denetlemek için filtreyi değiştirin.

   ![Şekil 31: Değişiklik bilgisayarları eklemek için bu nesne türleri][sap-ha-guide-figure-3020]

   _**Şekil 31:** Değişiklik bilgisayarları eklemek için bu nesne türleri_

   ![Şekil 32: Bilgisayarlar onay kutusunu seçin][sap-ha-guide-figure-3021]

   _**Şekil 32:** Seçin **bilgisayarlar** onay kutusu_

4. Şekil 31'de gösterildiği gibi küme adı nesnesi girin. Kaydı zaten oluşturulduğundan şekil 30 gösterildiği izinleri değiştirebilirsiniz.

5. Seçin **güvenlik** paylaşımı ve ardından sekme daha ayrıntılı küme adı nesnesi için izinleri.

   ![Şekil 33: Dosya Paylaşımı çekirdeği üzerinde Küme adı nesnesi için güvenlik özniteliklerini ayarlayın][sap-ha-guide-figure-3022]

   _**Şekil 33:** Dosya Paylaşımı çekirdeği üzerinde Küme adı nesnesi için güvenlik özniteliklerini ayarlayın_

##### <a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a> Yük Devretme Kümesi Yöneticisi'nde dosya paylaşım tanığı çekirdek ayarlayın

1. Açık çekirdeği Ayarlama Sihirbazı'nı yapılandırın.

   ![Şekil 34: Yapılandırma küme çekirdek Ayarlama Sihirbazı'nı başlatın][sap-ha-guide-figure-3023]

   _**Şekil 34:** Yapılandırma küme çekirdek Ayarlama Sihirbazı'nı başlatın_

2. Üzerinde **Çekirdek yapılandırmasını seçin** sayfasında **çekirdek tanığı Seç**.

   ![Şekil 35: Çekirdek yapılandırmaları arasından seçim yapabilirsiniz][sap-ha-guide-figure-3024]

   _**Şekil 35:** Çekirdek yapılandırmaları arasından seçim yapabilirsiniz_

3. Üzerinde **çekirdek tanığı Seç** sayfasında **dosya paylaşım tanığı yapılandırma**.

   ![Şekil 36: Dosya paylaşım tanığı seçin][sap-ha-guide-figure-3025]

   _**Şekil 36:** Dosya paylaşım tanığı seçin_

4. Dosya Paylaşımı için UNC yolu girin (Bizim örneğimizde \\domcontr 0\FSW). Yapabilirsiniz değişikliklerin bir listesini görmek için seçin **sonraki**.

   ![Şekil 37: Tanık paylaşımı için dosya paylaşım konumunu tanımlayın][sap-ha-guide-figure-3026]

   _**Şekil 37:** Tanık paylaşımı için dosya paylaşım konumunu tanımlayın_

5. Seçin ve ardından değişiklikleri **sonraki**. Şekil 38 ' gösterildiği gibi küme yapılandırması başarıyla yeniden yapılandırmanız gerekir.  

   ![Şekil 38: Küme yeniden yapılandırılması onayı][sap-ha-guide-figure-3027]

   _**Şekil 38:** Küme yeniden yapılandırılması onayı_

Windows Yük devretme kümesi başarıyla yükledikten sonra değişiklikler için bazı eşikleri koşullara azure'da yük devretme algılama uyum sağlamak için yapılması gerekir. Parametreleri değiştirilmesi bu blogda belgelenmiştir: https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/ . Aynı alt ağda ASCS/SCS derleme Windows Küme yapılandırması, iki VM olduğu varsayılırsa, aşağıdaki parametreleri, bu değerleri değiştirilmesi gerekebilir:
- SameSubNetDelay = 2
- SameSubNetThreshold = 15

Bu ayarlar müşterilerle test ve sağlanan bir tarafında yeterince dayanıklı olması için iyi bir güvenlik ihlali. Öte yandan bu ayarlar gerçek hata koşullarında yeterli yük devretme SAP yazılım veya düğüm/VM başarısız sağlama hızlı. 

### <a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a> SAP ASCS/SCS Küme Paylaşımı diski için SIOS DataKeeper Cluster Edition'ı yükleme

Artık Azure'da çalışan bir Windows Server Yük Devretme Kümelemesi yapılandırma vardır. Ancak, bir SAP ASCS/SCS örneği yüklemek için paylaşılan disk kaynağı gerekir. Azure'da ihtiyacınız olan paylaşılan disk kaynakları oluşturulamıyor. SIOS DataKeeper Cluster Edition, paylaşılan disk kaynakları oluşturmak için kullanabileceğiniz bir üçüncü taraf çözümüdür.

SAP ASCS/SCS Küme Paylaşımı diski için SIOS DataKeeper Cluster Edition yüklemek, bu görevleri içerir:

- .NET Framework 3.5 ekleme
- SIOS DataKeeper yükleme
- SIOS DataKeeper ayarlama

#### <a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a> .NET Framework 3.5 Ekle
Microsoft .NET Framework 3.5 otomatik olarak etkinleştirilmiş veya Windows Server 2012 R2'de yüklü değil. SIOS DataKeeper DataKeeper yüklediğiniz tüm düğümlerde olması için .NET Framework'ü gerektirdiğinden, kümedeki tüm sanal makinelerin konuk işletim sisteminde .NET Framework 3.5 yüklemeniz gerekir.

.NET Framework 3.5 eklemenin iki yolu vardır:

- Rol ve Özellik Ekleme Sihirbazı Windows şekil 39 gösterildiği gibi kullanın.

  ![Şekil 39: Rol ve Özellik Ekleme Sihirbazı'nı kullanarak .NET Framework 3.5 yükleme][sap-ha-guide-figure-3028]

  _**Şekil 39:** Rol ve Özellik Ekleme Sihirbazı'nı kullanarak .NET Framework 3.5 yükleme_

  ![Şekil 40: Rol ve Özellik Ekleme Sihirbazı'nı kullanarak .NET Framework 3.5 yüklerken çubuğu yükleme ilerleme durumu][sap-ha-guide-figure-3029]

  _**Şekil 40:** Rol ve Özellik Ekleme Sihirbazı'nı kullanarak .NET Framework 3.5 yüklerken çubuğu yükleme ilerleme durumu_

- Komut satırı aracı dism.exe kullanın. Bu yükleme türü için Windows yükleme medyasında bulunan SxS dizin erişmeniz gerekir. Yükseltilmiş bir komut isteminde aşağıdakini yazın:

  ```
  Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
  ```

#### <a name="dd41d5a2-8083-415b-9878-839652812102"></a> SIOS DataKeeper yükleyin

Kümedeki her düğümde SIOS DataKeeper Cluster Edition yükleyin. SIOS DataKeeper ile sanal paylaşılan depolama alanı oluşturmak için eşitlenen bir yansıtma oluşturun ve ardından Küme Paylaşılan depolamayı benzetmek.

SIOS yazılımını yüklemeden önce etki alanı kullanıcısı oluşturun **DataKeeperSvc**.

> [!NOTE]
> Ekleme **DataKeeperSvc** kullanıcıya **yerel yönetici** her iki küme düğümlerinde grubu.
>
>

SIOS DataKeeper yüklemek için:

1. SIOS yazılımı, her iki küme düğümlerinde yükleyin.

   ![SIOS yükleyici][sap-ha-guide-figure-3030]

   ![Şekil 41: SIOS DataKeeper yüklemenin ilk sayfa][sap-ha-guide-figure-3031]

   _**Şekil 41:** SIOS DataKeeper yüklemenin ilk sayfa_

2. Şekil 42 içinde gösterilen iletişim kutusunda **Evet**.

   ![Şekil 42: Bir hizmet devre dışı bırakılacak DataKeeper bildirir][sap-ha-guide-figure-3032]

   _**Şekil 42:** Bir hizmet devre dışı bırakılacak DataKeeper bildirir_

3. Şekil 43 gösterilen iletişim kutusunda, seçtiğiniz olan öneririz **etki alanı veya sunucu hesabı**.

   ![Şekil 43: SIOS DataKeeper için kullanıcı seçimi][sap-ha-guide-figure-3033]

   _**Şekil 43:** SIOS DataKeeper için kullanıcı seçimi_

4. SIOS DataKeeper için oluşturduğunuz parola ve etki alanı hesabı kullanıcı adı girin.

   ![Şekil 44: Etki alanı kullanıcı adı ve parolayı için SIOS DataKeeper yükleme girin][sap-ha-guide-figure-3034]

   _**Şekil 44:** Etki alanı kullanıcı adı ve parolayı için SIOS DataKeeper yükleme girin_

5. SIOS DataKeeper Örneğiniz için lisans anahtarı şekil 45 gösterildiği gibi yükleyin.

   ![Şekil 45: SIOS DataKeeper lisans anahtarınızı girin][sap-ha-guide-figure-3035]

   _**Şekil 45:** SIOS DataKeeper lisans anahtarınızı girin_

6. İstendiğinde, sanal makineyi yeniden başlatın.

#### <a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a> SIOS DataKeeper ' ayarlayın

Her iki düğümde SIOS DataKeeper yükledikten sonra yapılandırmayı başlatmanız gerekir. Yapılandırma amacı, her sanal makineye bağlı ek VHD'ler arasında zaman uyumlu veri çoğaltmayı olmasını sağlamaktır.

1. DataKeeper yönetim ve yapılandırma aracını başlatın ve ardından **Connect Server**. (Şekil 46 bu seçenek kırmızı daire içine alınmıştır.)

   ![Şekil 46: SIOS DataKeeper yönetim ve yapılandırma aracı][sap-ha-guide-figure-3036]

   _**Şekil 46:** SIOS DataKeeper yönetim ve yapılandırma aracı_

2. Adını veya ilk düğümü yönetim ve yapılandırma aracını için ve ikinci adım, ikinci düğüme bağlanmak TCP/IP'yi adresini girin.

   ![Şekil 47: TCP/IP adresi ilk düğümü yönetim ve yapılandırma aracı için ve ikinci adım, ikinci düğümü bağlanmanız gerekir ya da adını Ekle][sap-ha-guide-figure-3037]

   _**Şekil 47:** TCP/IP adresi ilk düğümü yönetim ve yapılandırma aracı için ve ikinci adım, ikinci düğümü bağlanmanız gerekir ya da adını Ekle_

3. İki düğüm arasındaki çoğaltma işi oluşturun.

   ![Şekil 48: Çoğaltma işi oluşturma][sap-ha-guide-figure-3038]

   _**Şekil 48:** Çoğaltma işi oluşturma_

   Bir sihirbaz çoğaltma işi oluşturma işleminde size kılavuzluk eder.
4. Adı, TCP/IP adresi ve kaynak düğümünün disk birimi tanımlayın.

   ![Şekil 49: Çoğaltma işi adını tanımlayın][sap-ha-guide-figure-3039]

   _**Şekil 49:** Çoğaltma işi adını tanımlayın_

   ![Şekil 50: Geçerli kaynak düğüm olmalıdır düğümü için temel veri tanımlama][sap-ha-guide-figure-3040]

   _**Şekil 50:** Geçerli kaynak düğüm olmalıdır düğümü için temel veri tanımlama_

5. Adı, TCP/IP adresi ve hedef düğümünün disk birimi tanımlayın.

   ![Şekil 51: Geçerli hedef düğüm olmalıdır düğümü için temel veri tanımlama][sap-ha-guide-figure-3041]

   _**Şekil 51:** Geçerli hedef düğüm olmalıdır düğümü için temel veri tanımlama_

6. Sıkıştırma algoritmaları tanımlar. Bizim örneğimizde, çoğaltma akışını sıkıştırmak öneririz. Çoğaltma akışı sıkıştırma, özellikle yeniden eşitleme durumlarda yeniden eşitleme zamanı önemli ölçüde azaltır. Sıkıştırma bir sanal makine CPU ve RAM kaynaklarını kullandığını unutmayın. Sıkıştırma oranı arttıkça, bu nedenle kullanılan CPU kaynakları hacmi yapar. Ayrıca, bu ayar daha sonra ayarlayabilirsiniz.

7. Başka bir ayarı kontrol etmeniz mi çoğaltma zaman uyumlu veya zaman uyumsuz olarak gerçekleşir. *SAP ASCS/SCS yapılandırmaları koruduğunuzda, zaman uyumlu çoğaltma kullanmalısınız*.  

   ![Şekil 52: Çoğaltma ayrıntılarını tanımlayın][sap-ha-guide-figure-3042]

   _**Şekil 52:** Çoğaltma ayrıntılarını tanımlayın_

8. Çoğaltma işi tarafından çoğaltılmış birimi paylaşılan bir disk olarak Windows Server Yük Devretme Kümelemesi küme yapılandırması için gösterilen olup olmadığını tanımlar. SAP ASCS/SCS yapılandırmasını seçin **Evet** böylece küme birimi olarak kullanabileceği paylaşılan bir disk olarak çoğaltılmış bir birimi Windows Küme görür.

   ![Şekil 53: Çoğaltılmış birimi küme birimi olarak ayarlamak için Evet'i seçin][sap-ha-guide-figure-3043]

   _**Şekil 53:** Seçin **Evet** çoğaltılmış birimi küme birimi olarak ayarlamak için_

   Birim oluşturulduktan sonra DataKeeper yönetimini ve yapılandırmasını araç çoğaltma işi etkin olduğunu gösterir.

   ![Şekil 54: SAP ASCS/SCS paylaşımı diski DataKeeper zaman uyumlu yansıtma etkin][sap-ha-guide-figure-3044]

   _**Şekil 54:** SAP ASCS/SCS paylaşımı diski DataKeeper zaman uyumlu yansıtma etkin_

   Yük Devretme Kümesi Yöneticisi, artık şekil 55 gösterildiği gibi bir DataKeeper disk olarak disk gösterir.

   ![Şekil 55: Yük Devretme Kümesi Yöneticisi DataKeeper çoğaltılan disk gösterir.][sap-ha-guide-figure-3045]

   _**Şekil 55:** Yük Devretme Kümesi Yöneticisi DataKeeper çoğaltılan disk gösterir._

## <a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a> SAP NetWeaver sistemini yükleyin

Ayarlar DBMS sistemine bağlı olarak farklılık gösterdiğinden DBMS Kurulumu açıklanmaktadır olmaz. Ancak, farklı DBMS satıcıları desteklemek için Azure işlevleri ile DBMS ile yüksek kullanılabilirlik sorunları ele alınır varsayılır. Örneğin, her zaman açık veya SQL Server ve Oracle Data Guard'için Oracle veritabanları için veritabanı yansıtma. Bu makalede kullandığımız senaryosunda, size daha fazla koruma için DBMS ekleyemiyor.

Bu tür bir kümelenmiş SAP ASCS/SCS yapılandırma azure'da farklı DBMS Hizmetleri etkileşim oluşan özel durumlar vardır.

> [!NOTE]
> SAP NetWeaver ABAP sistemleri, Java sistemleri ve ABAP + Java sistemleri yükleme yordamları neredeyse aynıdır. En önemli fark, bir SAP ABAP sistemi bir ASCS örneğine sahip olur. SAP Java sistem bir SCS örneği vardır. ASCS örneği ve bir SCS örneği aynı Microsoft yük devretme küme grubu içinde çalışan SAP ABAP + Java sistemi vardır. Her SAP NetWeaver yükleme yığınının yükleme farkları açıkça belirtilmiştir. Diğer tüm bölümleri aynı olduğunu varsayabilirsiniz.  
>
>

### <a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a> Bir yüksek kullanılabilirlik ASCS/SCS örneği ile SAP yükleme

> [!IMPORTANT]
> Sayfa dosyanız DataKeeper yansıtılmış birimler üzerinde değil yerleştirdiğinizden emin olun. Yansıtılmış birimler DataKeeper desteklemez. Varsayılan sayfa dosyanız bir Azure sanal makine geçici D sürücüsündeki bırakabilirsiniz. Zaten orada değilse, Windows disk belleği dosyası, Azure sanal makinenizin D sürücüye taşıyın.
>
>

Bir yüksek kullanılabilirlik ASCS/SCS örneği ile SAP yüklemek, bu görevleri içerir:

- Kümelenmiş SAP ASCS/SCS örneği için bir sanal ana bilgisayar adı oluşturma
- SAP ilk küme düğümüne yükleme
- SAP ASCS/SCS örneği profilini değiştirme
- Araştırma bağlantı noktası ekleme
- Windows Güvenlik Duvarı araştırma bağlantı noktası açma

#### <a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a> Kümelenmiş SAP ASCS/SCS örneği için bir sanal ana bilgisayar adı oluşturun

1. Windows DNS Yöneticisi'nde, sanal ana bilgisayar adı ASCS/SCS örneği için bir DNS girişi oluşturabilir.

   > [!IMPORTANT]
   > Sanal ana bilgisayar adını ASCS/SCS örneği için atadığınız IP adresi, Azure yük dengeleyiciye atanan IP adresi ile aynı olmalıdır (**<*SID*> - lb - ascs**).  
   >
   >

   SAP ASCS/SCS sanal ana bilgisayar adı, IP adresi (**pr1 ascs sap**) IP adresini Azure Load Balancer'ın aynı (**pr1 lb ascs**).

   ![Şekil 56: SAP ASCS/SCS küme sanal adı ve TCP/IP adresi için DNS girişi tanımlayın][sap-ha-guide-figure-3046]

   _**Şekil 56:** SAP ASCS/SCS küme sanal adı ve TCP/IP adresi için DNS girişi tanımlayın_

2. Sanal ana bilgisayar adı için atanan IP adresini tanımlamak için seçin **DNS Yöneticisi** > **etki alanı**.

   ![Şekil 57: Yeni bir sanal ad ve SAP ASCS/SCS kümesi yapılandırması için TCP/IP adresi][sap-ha-guide-figure-3047]

   _**Şekil 57:** Yeni bir sanal ad ve SAP ASCS/SCS kümesi yapılandırması için TCP/IP adresi_

#### <a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a> SAP ilk küme düğümüne yükleme

1. İlk küme düğümü seçenek a küme düğümünde yürütün Örneğin, **pr1 ascs 0** ana bilgisayar.
2. Azure iç yük dengeleyici için bağlantı noktalarını varsayılan tutmak için bu seçeneği seçin:

   * **ABAP sistem**: **ASCS** örnek numarası **00**
   * **Java sistem**: **SCS** örnek numarası **01**
   * **ABAP + Java sistem**: **ASCS** örnek numarası **00** ve **SCS** örnek numarası **01**

   ABAP ASCS örneği ve 01 Java SCS örneği için örnek sayılar 00 dışında kullanmak için ilk Azure iç yük dengeleyici varsayılan Yük Dengeleme kuralları, açıklanan değiştirmeye ihtiyacınız [ASCS/SCS varsayılan Yük Dengeleme kuralları için değiştirin Azure iç yük dengeleyici][sap-ha-guide-8.9].

Sonraki birkaç görevi standart SAP yükleme belgelerinde açıklanan değildir.

> [!NOTE]
> SAP yükleme belgelerine ilk ASCS/SCS küme düğümüne yüklemeyi açıklar.
>
>

#### <a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a> SAP ASCS/SCS örneği profilini değiştirme

Yeni bir profil parametresi eklemeniz gerekir. Profil parametresi, uzun süre boşta olduklarında kapanmasını SAP iş süreçleri sıraya alma server arasında bağlantıları engeller. Sorun senaryoda belirttiğimiz [SAP ASCS/SCS örneği her iki küme düğümlerinde kayıt defteri girdisini eklemeniz][sap-ha-guide-8.11]. Bu bölümde Ayrıca iki değişikliği bazı temel TCP/IP bağlantı parametrelerini kullanıma sunduk. İkinci adımda, kuyruğa sunucusunu gönderecek şekilde ayarlamanız gerekir. bir `keep_alive` bağlantıları Azure iç load balancer'ın boşta eşiği isabet yoksa, sinyal.

SAP ASCS/SCS örneği profilini değiştirmek için:

1. Bu profil parametresi SAP ASCS/SCS örneği profiline ekleyin:

   ```
   enque/encni/set_so_keepalive = true
   ```
   Bizim örneğimizde yoludur:

   `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_ASCS00_pr1-ascs-sap`

   Örneğin, SAP SCS örneği profili ve karşılık gelen yolu:

   `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_SCS01_pr1-ascs-sap`

2. Değişiklikleri uygulamak için SAP ASCS /SCS örneğini yeniden başlatın.

#### <a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a> Bir araştırma bağlantı noktası Ekle

Azure Load Balancer ile çalışma tüm küme yapılandırmasını yapmak için iç load balancer'ın araştırma işlevini kullanın. Azure iç yük dengeleyici genellikle gelen iş yükü katılan sanal makineler arasında eşit olarak dağıtır. Ancak, tek örnek etkin olmadığından bu kümenin bazı yapılandırmalarda işe yaramaz. Diğer bir örnek pasif ve herhangi bir iş yükü kabul edemez. Azure iç yük dengeleyici yalnızca etkin bir örneği için iş atarken araştırma işlevselliği yardımcı olur. Araştırma özelliği sayesinde, iç yük dengeleyici hangi örneklerinin etkin olduğunu ve yalnızca iş yüküyle örneği ardından hedef algılayabilir.

Bir araştırma bağlantı noktası eklemek için:

1. Geçerli denetleyin **ProbePort** aşağıdaki PowerShell komutunu çalıştırarak ayarlama. Bu sanal makinelerden biri içinde küme yapılandırmasını yürütün.

   ```PowerShell
   $SAPSID = "PR1"     # SAP <SID>

   $SAPNetworkIPClusterName = "SAP $SAPSID IP"
   Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter
   ```

2. Bir araştırma bağlantı noktasını tanımlayın. Varsayılan araştırma bağlantı noktası numarası **0**. Bizim örneğimizde, yoklama bağlantı noktası kullandığımız **62000**.

   ![Şekil 58: Küme yapılandırması araştırma bağlantı noktası varsayılan olarak 0 olur.][sap-ha-guide-figure-3048]

   _**Şekil 58:** Varsayılan Küme yapılandırma araştırma bağlantı noktası 0'dır_

   Bağlantı noktası numarasını SAP Azure Resource Manager şablonları içinde tanımlanır. PowerShell bağlantı noktası numarası atayabilirsiniz.

   Yeni bir ProbePort değerini ayarlamak için **SAP <*SID*> IP** küme kaynağı için aşağıdaki PowerShell betiğini çalıştırın. PowerShell benzeri değişkenleri ortamınız için güncelleştirin. Betik çalıştıktan sonra değişiklikleri etkinleştirmek için SAP küme grubu yeniden başlatmanız istenir.

   ```PowerShell
   $SAPSID = "PR1"      # SAP <SID>
   $ProbePort = 62000   # ProbePort of the Azure Internal Load Balancer

   Clear-Host
   $SAPClusterRoleName = "SAP $SAPSID"
   $SAPIPresourceName = "SAP $SAPSID IP"
   $SAPIPResourceClusterParameters =  Get-ClusterResource $SAPIPresourceName | Get-ClusterParameter
   $IPAddress = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "Address" }).Value
   $NetworkName = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "Network" }).Value
   $SubnetMask = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "SubnetMask" }).Value
   $OverrideAddressMatch = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "OverrideAddressMatch" }).Value
   $EnableDhcp = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "EnableDhcp" }).Value
   $OldProbePort = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "ProbePort" }).Value

   $var = Get-ClusterResource | Where-Object {  $_.name -eq $SAPIPresourceName  }

   Write-Host "Current configuration parameters for SAP IP cluster resource '$SAPIPresourceName' are:" -ForegroundColor Cyan
   Get-ClusterResource -Name $SAPIPresourceName | Get-ClusterParameter

   Write-Host
   Write-Host "Current probe port property of the SAP cluster resource '$SAPIPresourceName' is '$OldProbePort'." -ForegroundColor Cyan
   Write-Host
   Write-Host "Setting the new probe port property of the SAP cluster resource '$SAPIPresourceName' to '$ProbePort' ..." -ForegroundColor Cyan
   Write-Host

   $var | Set-ClusterParameter -Multiple @{"Address"=$IPAddress;"ProbePort"=$ProbePort;"Subnetmask"=$SubnetMask;"Network"=$NetworkName;"OverrideAddressMatch"=$OverrideAddressMatch;"EnableDhcp"=$EnableDhcp}

   Write-Host

   $ActivateChanges = Read-Host "Do you want to take restart SAP cluster role '$SAPClusterRoleName', to activate the changes (yes/no)?"

   if($ActivateChanges -eq "yes"){
   Write-Host
   Write-Host "Activating changes..." -ForegroundColor Cyan

   Write-Host
   write-host "Taking SAP cluster IP resource '$SAPIPresourceName' offline ..." -ForegroundColor Cyan
   Stop-ClusterResource -Name $SAPIPresourceName
   sleep 5

   Write-Host "Starting SAP cluster role '$SAPClusterRoleName' ..." -ForegroundColor Cyan
   Start-ClusterGroup -Name $SAPClusterRoleName

   Write-Host "New ProbePort parameter is active." -ForegroundColor Green
   Write-Host

   Write-Host "New configuration parameters for SAP IP cluster resource '$SAPIPresourceName':" -ForegroundColor Cyan
   Write-Host
   Get-ClusterResource -Name $SAPIPresourceName | Get-ClusterParameter
   }else
   {
   Write-Host "Changes are not activated."
   }
   ```

   Sonra Getir **SAP <*SID*>** doğrulayın, küme rolünü **ProbePort** yeni değere ayarlanır.

   ```PowerShell
   $SAPSID = "PR1"     # SAP <SID>

   $SAPNetworkIPClusterName = "SAP $SAPSID IP"
   Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter

   ```

   ![Şekil 59: Yeni değeri ayarladıktan sonra küme bağlantı noktası araştırma][sap-ha-guide-figure-3049]

   _**Şekil 59:** Yeni değeri ayarladıktan sonra küme bağlantı noktası araştırma_

#### <a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a> Windows Güvenlik Duvarı araştırma bağlantı noktasını açın

Her iki küme düğümlerinde Windows Güvenlik Duvarı araştırma bağlantı noktası açma gerekir. Bir Windows Güvenlik Duvarı araştırma bağlantı noktasını açmak için aşağıdaki betiği kullanın. PowerShell benzeri değişkenleri ortamınız için güncelleştirin.

  ```PowerShell
  $ProbePort = 62000   # ProbePort of the Azure Internal Load Balancer

  New-NetFirewallRule -Name AzureProbePort -DisplayName "Rule for Azure Probe Port" -Direction Inbound -Action Allow -Protocol TCP -LocalPort $ProbePort
  ```

**ProbePort** ayarlanır **62000**. Dosya paylaşımına erişebilirsiniz artık  **\\\ascsha-clsap\sapmnt** diğer konaklarından gibi kadar **ascsha dba'lar**.

### <a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a> Veritabanı örneğini yükleyin

Veritabanı örneği yüklemek için SAP yükleme belgelerinde açıklanan işlemi izleyin.

### <a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a> İkinci küme düğümüne yükleme

İkinci küme yüklemek için SAP Yükleme Kılavuzu'ndaki adımları izleyin.

### <a name="094bc895-31d4-4471-91cc-1513b64e406a"></a> SAP Ağıranlar Windows hizmet örneği başlangıç türünü değiştirme

SAP Ağıranlar Windows hizmetinin başlangıç türünü değiştirin **otomatik (Gecikmeli Başlatma)** her iki küme düğümlerinde.

![Şekil 60: SAP Ağıranlar örneği için hizmet türü Gecikmeli otomatik olarak değiştirin][sap-ha-guide-figure-3050]

_**Şekil 60:** SAP Ağıranlar örneği için hizmet türü Gecikmeli otomatik olarak değiştirin_

### <a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a> SAP birincil uygulama sunucusu yükleme

Birincil uygulama sunucusu (PA'lar) örneğini yükleyin <*SID*> - di - 0 Pa'ları barındırmak için belirlediğiniz sanal makine üzerinde. Azure veya DataKeeper özgü ayarları üzerinde hiçbir bağımlılık vardır.

### <a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a> SAP ek uygulama sunucusu yükleme

Bir SAP ek uygulama sunucusu (AAS) tüm SAP uygulama sunucusu örneği barındırmak için belirlediğiniz makinelerde yükleyin. Örneğin, <*SID*> - di - 1 için <*SID*> - di -&lt;n&gt;.

> [!NOTE]
> Bu, yüksek kullanılabilirlik SAP NetWeaver system yüklemesi tamamlanır. Ardından, yük devretme testiyle devam edin.
>


## <a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a> SIOS çoğaltma ve SAP ASCS/SCS örneği yük devretme testi
Testi ve yük devretme kümesi Yöneticisi SIOS DataKeeper yönetim ve yapılandırma aracını kullanarak bir SAP ASCS/SCS örneği yük devretme ve SIOS disk çoğaltması izlemek kolay bir işlemdir.

### <a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a> SAP ASCS/SCS örneği bir küme düğümünde çalışıyor

**SAP PR1** küme grubu A'daki küme düğümünde çalışıyor Örneğin, **pr1 ascs 0**. Paylaşılan disk parçası olan sürücü S, atama, **SAP PR1** küme grubu ve küme düğümü A. ASCS/SCS örneği kullanan

![Şekil 61: Yük Devretme Kümesi Yöneticisi: Bir küme düğümünde çalışan SAP < SID > Küme grubu][sap-ha-guide-figure-5000]

_**Şekil 61:** Yük Devretme Kümesi Yöneticisi: SAP <*SID*> Küme grubu, bir küme düğümünde çalışıyor_

SIOS DataKeeper yönetim ve Yapılandırma Aracı'nda paylaşılan disk zaman uyumlu olarak bir küme düğümünde kaynak birim sürücüden S b küme düğümünü hedef birimi sürücüsünü S çoğaltılan verileri görebilirsiniz Örneğin, çoğaltılana **pr1 ascs 0 [10.0.0.40]** için **pr1 ascs 1 [10.0.0.41]**.

![Şekil 62: SIOS DataKeeper yerel birim bir küme düğümünden küme düğümüne B çoğaltma][sap-ha-guide-figure-5001]

_**Şekil 62:** SIOS DataKeeper yerel birim bir küme düğümünden küme düğümüne B çoğaltma_

### <a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a> Bir düğümden yük devretme için B düğümü

1. SAP, bir yük devretmeyi başlatmak için aşağıdaki seçeneklerden birini seçin <*SID*> küme düğümüne B: bir küme düğümünden küme grubu
   - Yük Devretme Kümesi Yöneticisi'ni kullanın  
   - Yük devretme kümesi PowerShell kullanma

   ```PowerShell
   $SAPSID = "PR1"     # SAP <SID>

   $SAPClusterGroup = "SAP $SAPSID"
   Move-ClusterGroup -Name $SAPClusterGroup

   ```
2. Küme düğümü bir Windows konuk işletim sistemi içinde yeniden (Bu SAP otomatik yük devretme başlatır <*SID*> Küme grubu bir düğümden düğüme B).  
3. Azure portalından küme düğümü bir yeniden başlatma (Bu SAP otomatik yük devretme başlatır <*SID*> Küme grubu bir düğümden düğüme B).  
4. Azure PowerShell kullanarak küme düğümü bir yeniden başlatma (Bu SAP otomatik yük devretme başlatır <*SID*> Küme grubu bir düğümden düğüme B).

   Yük devretme, SAP sonra <*SID*> Küme grubu b küme düğümünde çalışıyor Örneğin, üzerinde çalıştığı **pr1 ascs 1**.

   ![Şekil 63: Yük Devretme Kümesi Yöneticisi'nde, SAP < SID > Küme grubu B küme düğümünde çalışıyor][sap-ha-guide-figure-5002]

   _**Şekil 63**: Yük Devretme Kümesi Yöneticisi'nde, SAP <*SID*> Küme grubu B küme düğümünde çalışıyor_

   Paylaşılan disk artık bağlanmıştır kümede düğüm b SIOS DataKeeper veri kaynak birim sürücüden S B küme düğümünde A. küme düğümünde hedef birimin sürücü S çoğaltma Örneğin, gelen çoğaltma **pr1 ascs 1 [10.0.0.41]** için **pr1 ascs 0 [10.0.0.40]**.

   ![Şekil 64: SIOS DataKeeper yerel birim düğümü bir küme için küme düğümlerinin birinden B çoğaltır.][sap-ha-guide-figure-5003]

   _**Şekil 64:** SIOS DataKeeper yerel birim düğümü bir küme için küme düğümlerinin birinden B çoğaltır._
