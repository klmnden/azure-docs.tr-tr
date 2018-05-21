---
title: Azure sanal makineler için yüksek oranda kullanılabilirlik SAP NetWeaver | Microsoft Docs
description: SAP NetWeaver için Azure sanal makineler üzerinde yüksek kullanılabilirlik Kılavuzu
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/07/2016
ms.author: goraco
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4d1088f786bdcfabb3095dfb8276c508f97bc23b
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
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

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

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

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

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

[powershell-install-configure]:https://docs.microsoft.com/powershell/azure/install-azurerm-ps
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
[storage-premium-storage-preview-portal]:../../windows/premium-storage.md
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


Azure sanal makinelerin işlem, depolama ve ağ kaynaklarına, en az sürede ve uzun tedarik döngüleri olmadan gereken kuruluşlar için çözüm şeklindedir. SAP NetWeaver tabanlı ABAP, Java ve ABAP + Java yığını gibi Klasik uygulamaları dağıtmak için Azure sanal makineleri kullanabilirsiniz. Güvenilirlik ve kullanılabilirlik ek şirket içi kaynaklar olmadan genişletir. Azure sanal makineleri şirket içi bağlantılar, Azure sanal makineler, kuruluşunuzun şirket içi etki alanları, özel Bulutlar ve SAP sistem yatay tümleştirebilir şekilde destekler.

Bu makalede, Azure Resource Manager dağıtım modelini kullanarak azure'da yüksek kullanılabilirlik SAP sistemlerini dağıtmak için uygulayabileceğiniz adımlar kapsar. Biz bu ana görevlerinde ilerlemenizde:

* Sağ SAP notlar ve yükleme kılavuzları, listelenen Bul [kaynakları] [ sap-ha-guide-2] bölümü. Bu makalede SAP yükleme belgelerini tamamlar ve yardımcı olabilecek birincil kaynaklardır SAP notları yükleyip belirli platformlar SAP yazılımı dağıtın.
* Azure Resource Manager dağıtım modeli ve Azure Klasik dağıtım modeli arasındaki farklar hakkında bilgi edinin.
* Azure dağıtımınız için doğru modeli seçebilmeniz için Windows Server Yük Devretme Kümelemesi çekirdek modları hakkında bilgi edinin.
* Azure hizmetlerinde paylaşılan Windows Server Yük Devretme Kümelemesi depolama hakkında bilgi edinin.
* Başarısızlık durumunu tek nokta bileşenleri gibi gelişmiş iş uygulama programlama (ABAP) SAP merkezi Hizmetleri (ASCS) korunmasına yardımcı olmak öğrenin / SAP merkezi Hizmetleri (SCS) ve veritabanı yönetim sistemi (DBMS) ve yedek bileşenlerin gibi Azure SAP uygulama sunucusu.
* Adım adım örnek bir yükleme ve bir Windows Server Yük Devretme Kümelemesi küme azure'da yüksek kullanılabilirlik SAP sisteminde yapılandırmasının, Azure Kaynak Yöneticisi'ni kullanarak izleyin.
* Windows Server Yük Devretme Kümelemesi Azure ancak, bir şirket içi dağıtımda gerekli olmayan kullanmak için gerekli ek adımlar hakkında bilgi edinin.

Dağıtım ve yapılandırma, bu makalede, basitleştirmek için SAP üç katmanlı yüksek kullanılabilirlik Resource Manager şablonları kullanırız. Şablonlar bir yüksek kullanılabilirlik SAP sistemi için gereksinim duyduğunuz tüm altyapının dağıtımını otomatik hale getirme. Altyapı SAP uygulama performans standart (SAP) boyutlandırma SAP sisteminizin de destekler.

## <a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a> Önkoşullar
Başlamadan önce aşağıdaki bölümlerde açıklanan önkoşulları karşıladığından emin olun. Ayrıca, listelenen tüm kaynakları kontrol ettiğinizden emin olun [kaynakları] [ sap-ha-guide-2] bölümü.

Bu makalede, Azure Resource Manager şablonları için kullandığımız [üç katmanlı SAP NetWeaver](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image/). Şablonları yararlı bir genel bakış için bkz: [SAP Azure Resource Manager şablonları](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).

## <a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a> Kaynakları
Bu makaleler Azure SAP dağıtımlarda kapsar:

* [Azure sanal makineleri planlama ve uygulama SAP NetWeaver için][planning-guide]
* [SAP NetWeaver için Azure sanal makineler dağıtımı][deployment-guide]
* [SAP NetWeaver için Azure sanal makineleri DBMS dağıtımı][dbms-guide]
* [Azure sanal makineler için yüksek oranda kullanılabilirlik SAP NetWeaver (Bu Kılavuzu)][sap-ha-guide]

> [!NOTE]
> Mümkün olduğunda, bir bağlantıya başvuran SAP Yükleme Kılavuzu'na sunuyoruz (bkz [SAP yükleme kılavuzları][sap-installation-guides]). Önkoşullar ve yükleme işlemiyle ilgili bilgi için SAP NetWeaver yükleme kılavuzları dikkatle okuyun iyi bir fikirdir. Bu makalede yalnızca Azure sanal makineler ile kullanabileceğiniz SAP NetWeaver tabanlı sistemler için belirli görevler yer almaktadır.
>
>

Bu SAP Notlar Azure SAP konu ilişkili:

| Not numarası | Unvan |
| --- | --- |
| [1928533] |Azure üzerinde SAP uygulamaları: desteklenen ürünler ve boyutlandırma |
| [2015553] |Microsoft Azure üzerinde SAP: Destek önkoşulları |
| [1999351] |Azure için SAP izleme Gelişmiş |
| [2178632] |Microsoft Azure üzerinde SAP için ölçümleri izleme anahtarı |
| [1999351] |Windows sanallaştırma: İzleme Gelişmiş |
| [2243692] |SAP DBMS örneği için Azure Premium SSD depolama kullanımı |

Daha fazla bilgi edinmek [Azure abonelikleri sınırlamaları][azure-subscription-service-limits-subscription]genel varsayılan sınırlamalar ve en fazla sınırlamalar dahil olmak üzere.

## <a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>Azure Klasik dağıtım modeli ve Azure Resource Manager ile yüksek kullanılabilirlik SAP
Azure Resource Manager ve Azure Klasik dağıtım modellerine aşağıdaki alanlarda farklıdır:

- Kaynak grupları
- Azure kaynak grubu Azure iç yük dengeleyici bağımlılığı
- SAP çoklu SID senaryolar için destek

### <a name="f76af273-1993-4d83-b12d-65deeae23686"></a> Kaynak grupları
Azure Kaynak Yöneticisi'nde, Azure aboneliğinizde tüm uygulama kaynakları yönetmek için kaynak gruplarını kullanabilirsiniz. Tümleşik bir yaklaşım, aynı yaşam döngüsü tüm kaynaklar bir kaynak grubunda sahiptir. Örneğin, tüm kaynaklar aynı anda oluşturulur ve aynı anda silinir. [Kaynak grupları](../../../azure-resource-manager/resource-group-overview.md#resource-groups) hakkında daha fazla bilgi edinin.

### <a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a> Azure kaynak grubu Azure iç yük dengeleyici bağımlılığı

Azure Klasik dağıtım modelinde Azure iç yük dengeleyici (Azure Yük Dengeleyici Hizmeti) ve bulut hizmeti grubu arasında bir bağımlılık yoktur. Her iç yük dengeleyici bir bulut hizmeti grubu olması gerekiyor.

Azure Kaynak Yöneticisi'nde, Azure yük dengeleyici kullanmak için bir Azure kaynak grubu gerek yoktur. , Daha basit ve daha esnek ortamıdır.

### <a name="support-for-sap-multi-sid-scenarios"></a>SAP çoklu SID senaryolar için destek

Azure Kaynak Yöneticisi'nde, birden çok SAP sistem tanımlayıcısı (SID) ASCS/SCS örnekleri bir küme içinde yükleyebilirsiniz. Çoklu SID örnekleri her Azure iç yük dengeleyici için birden fazla IP adresine yönelik destek nedeniyle mümkündür.

Azure Klasik dağıtım modelini kullanmak için açıklanan yordamları izleyin [SAP NetWeaver azure'da: SIOS DataKeeper ile azure'da Windows Server Yük Devretme Kümelemesi kullanarak SAP ASCS/SCS kümeleme örneklerini](http://go.microsoft.com/fwlink/?LinkId=613056).

> [!IMPORTANT]
> SAP yüklemeleri için Azure Resource Manager dağıtım modeli kullanmanız önerilir. Klasik dağıtım modelinde bulunmayan birçok avantaj sunar. Azure hakkında daha fazla bilgi [dağıtım modellerini][virtual-machines-azure-resource-manager-architecture-benefits-arm].   
>
>

## <a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a> Windows Server Yük Devretme Kümelemesi
Windows Server Yük Devretme Kümelemesi, yüksek kullanılabilirlik SAP ASCS/SCS yükleme ve Windows'da DBMS temelidir.

Bir yük devretme kümesi, uygulamaların ve hizmetlerin kullanılabilirliğini artırmak için birlikte çalışan 1 + n bağımsız sunucular (düğümler) grubudur. Bir düğüm hatası oluşursa, Windows Server Yük Devretme Kümelemesi uygulamaları ve hizmetleri sağlamak için iyi bir küme korurken oluşabilir sayısı hesaplar. Yük Devretme Kümelemesi elde etmek için farklı bir çekirdek modlarından seçebilirsiniz.

### <a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a> Çekirdek modu
Windows Server Yük Devretme Kümelemesi kullanırken dört çekirdek modlarından seçim yapabilirsiniz:

* **Düğüm çoğunluğu**. Kümedeki her düğüm oy kullanabilir. Küme yalnızca oyları, çoğunu ile başka bir deyişle, yarısından fazlası oyları ile çalışır. Bu seçenek eşit sayıda düğüme sahip kümeler için önerilir. Örneğin, üç yedi düğümlü bir küme düğümünde başarısız olabilir ve küme durağan Çoğunluk erişir ve çalışmaya devam eder.  
* **Düğüm ve Disk Çoğunluğu**. Her düğüm ve küme depolama alanındaki ayrılan disk (bir disk tanığı) ne zaman kullanılabilir olduğunu ve iletişim oy kullanabilir. Küme yalnızca oyları çoğunu ile başka bir deyişle, yarısından fazlası oyları ile çalışır. Bu mod, bir küme ortamında düğümlerinin çift sayıda mantıklıdır. Yarı düğüm ve disk çevrimiçi ise, küme sağlam durumda kalır.
* **Düğüm ve dosya paylaşımı çoğunluğu**. Her düğüm artı yönetici oluşturur bir dosya paylaşımı (dosya paylaşım tanığı), düğümler ve dosya paylaşımı kullanılabilir ve iletişimde oy kullanabilir. Küme yalnızca oyları çoğunu ile başka bir deyişle, yarısından fazlası oyları ile çalışır. Bu mod, bir küme ortamında düğümlerinin çift sayıda mantıklıdır. Düğüm ve Disk Çoğunluğu moduna benzer, ancak Tanık diski yerine bir Tanık dosya paylaşımı kullanır. Bu mod uygulanması kolaydır, ancak dosya paylaşımı kendisini yüksek oranda kullanılabilir değil, tek bir hata noktası olabilir.
* **Çoğunluk yok: Yalnızca Disk**. Bir düğüm kullanılabilir ve küme depolama alanındaki belirli bir diskle iletişimi ise küme çekirdeği vardır. Ayrıca, disk ile iletişimi olan düğümler kümeye katılabilir. Bu mod kullanmamanızı öneririz.

## <a name="fdfee875-6e66-483a-a343-14bbaee33275"></a> Şirket içi Windows Server Yük devretme
Şekil 1 iki düğümden oluşan bir küme gösterir. Düğüm başarısız olur ve hem düğümleri Kal yukarı ve çalışan, bir çekirdek disk veya dosya arasında ağ bağlantısı paylaşırsanız, hangi düğümün kümenin uygulamaları ve hizmetleri sağlamaya devam edecek belirler. Çekirdek disk veya dosya paylaşımına erişimi olduğundan düğüm Hizmetleri devam etmenizi sağlayan olandır.

Bu örnek iki düğümlü bir küme kullandığından, düğüm ve dosya paylaşımı çoğunluğu çekirdek modu kullanırız. Düğüm ve Disk Çoğunluğu geçerli bir seçenek de değil. Bir üretim ortamında, bir çekirdek disk kullanmanızı öneririz. Yüksek oranda kullanılabilir yapmak için ağ ve depolama sistemi teknolojisi kullanabilirsiniz.

![Şekil 1: Bir Windows Server Yük Devretme Kümelemesi yapılandırma için SAP ASCS/SCS Azure örneği][sap-ha-guide-figure-1000]

_**Şekil 1:** bir Windows Server Yük Devretme Kümelemesi yapılandırma için SAP ASCS/SCS Azure örneği_

### <a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a> Paylaşılan depolama alanı
Şekil 1 iki düğümlü paylaşılan depolama kümesi ayrıca gösterir. Bir şirket içi paylaşılan depolama Küme Paylaşılan depolama alanı kümedeki tüm düğümlerin algıla. Kilitleme mekanizması veri bozulmaya karşı korur. Tüm düğümler, başka bir düğüm başarısız olursa algılayabilir. Bir düğüm başarısız olursa, kalan düğümü depolama kaynakları aittir ve hizmetlerin kullanılabilirliğini sağlar.

> [!NOTE]
> SQL Server ile gibi bazı DBMS uygulamalarla yüksek kullanılabilirlik için Paylaşılan diskleri gerekmez. SQL Server Always On başka bir küme düğümüne yerel diske bir küme düğümünde yerel diskten DBMS veri ve günlük dosyalarını çoğaltır. Bu durumda, paylaşılan bir disk Windows Küme yapılandırması gerekmez.
>
>

### <a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a> Ağ ve ad çözümlemesi
İstemci bilgisayarlar, bir sanal IP adresi ve DNS sunucusu sağlayan bir sanal ana bilgisayar adı üzerinden küme ulaşabilirsiniz. Şirket içi düğümler ve DNS sunucusu birden çok IP adresi işleyebilir.

Tipik bir kurulumunda iki veya daha fazla ağ bağlantıları kullanın:

* Depolama için ayrılmış bir bağlantı
* Sinyal için bir küme iç ağ bağlantısı
* İstemcilerin kümeye bağlanmak için kullandığı bir ortak ağ

## <a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a> Azure'da Windows Server Yük devretme
Çıplak metal veya özel bulut dağıtımları karşılaştırıldığında, Azure sanal makineleri Windows Server Yük Devretme Kümelemesi yapılandırmak için ek adımlar gerektirir. Paylaşılan bir küme diski oluşturma sırasında birkaç IP adresleri ve sanal ana bilgisayar adları SAP ASCS/SCS örneği için ayarlamanız gerekir.

Bu makalede, temel kavramları ve Azure bir SAP yüksek kullanılabilirlik merkezi Hizmetleri küme oluşturmak için gereken ek adımlar açıklanmaktadır. Üçüncü taraf aracı SIOS DataKeeper ayarlamak nasıl ve Azure iç yük dengeleyici yapılandırmak nasıl gösteriyoruz. Dosya paylaşım tanığı Azure ile Windows Yük devretme kümesi oluşturmak için bu araçları kullanabilirsiniz.

![Şekil 2: Windows Server Yük devretme kümeleme yapılandırmasında Azure olmayan paylaşılan bir disk][sap-ha-guide-figure-1001]

_**Şekil 2:** Windows Server Yük devretme kümeleme yapılandırmasında Azure olmayan paylaşılan bir disk_

### <a name="1a464091-922b-48d7-9d08-7cecf757f341"></a> Paylaşılan disk Azure ile SIOS DataKeeper
Yüksek kullanılabilirlik SAP ASCS/SCS örneği için paylaşılan depolama küme. Eylül 2016 itibariyle Azure paylaşılan depolama küme oluşturmak için kullanabileceğiniz paylaşılan depolama sunmuyor. Üçüncü taraf yazılım SIOS DataKeeper Cluster Edition, Küme Paylaşılan depolama benzetim yansıtılmış bir depolama alanı oluşturmak için kullanabilirsiniz. Gerçek zamanlı zaman uyumlu veri çoğaltma SIOS çözümü sağlar. Bu küme için paylaşılan disk kaynağı nasıl oluşturabilirsiniz.

1. Bir ek Azure sanal sabit disk (VHD) her sanal makine (VM) için bir Windows küme yapılandırmasında ekleyin.
2. SIOS DataKeeper Cluster Edition her iki sanal makine düğümde çalıştırın.
3. Kaynak sanal makinenin ek bağlı VHD birimden hedef sanal makinenin ek bağlı VHD birime içeriğini yansıtan SIOS DataKeeper Cluster Edition yapılandırın. SIOS DataKeeper kaynak ve hedef yerel birimleri soyutlar ve Windows Server Yük Devretme Kümelemesi bir paylaşılan disk için gösterir.

Hakkında daha fazla bilgi almak [SIOS DataKeeper](http://us.sios.com/products/datakeeper-cluster/).

![Şekil 3: Windows Server Yük devretme kümeleme yapılandırmasında SIOS DataKeeper ile Azure][sap-ha-guide-figure-1002]

_**Şekil 3:** SIOS DataKeeper ile azure'da Windows Server Yük Devretme Kümelemesi yapılandırma_

> [!NOTE]
> SQL Server gibi bazı DBMS ürünleri ile yüksek kullanılabilirlik için Paylaşılan diskleri gerekmez. SQL Server Always On başka bir küme düğümüne yerel diske bir küme düğümünde yerel diskten DBMS veri ve günlük dosyalarını çoğaltır. Bu durumda, paylaşılan bir disk Windows Küme yapılandırması gerekmez.
>
>

### <a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a> Azure ad çözümleme
Azure bulut platformu kayan IP adresleri gibi sanal IP adreslerini yapılandırma seçeneği sağlamaz. Küme kaynağı bulutta ulaşmak için sanal bir IP adresi ayarlamak için alternatif bir çözüm gerekir.
Azure Azure yük dengeleyici hizmetinde bir iç yük dengeleyici sahiptir. İç yük dengeleyici ile istemcileri kümenin küme sanal IP adresi ulaşabilirsiniz.
Küme düğümleri içeren kaynak grubunda iç yük dengeleyicisi dağıtmanız gerekir. Ardından, kuralları araştırmasıyla iç yük dengeleyicisi bağlantı noktaları iletme tüm gerekli bağlantı noktası yapılandırın.
İstemcileri sanal ana bilgisayar adını bağlanabilir. DNS sunucusu küme IP adresi ve kümesinin etkin düğümü iletme iç yük dengeleyici tanıtıcıları bağlantı noktası çözümler.

## <a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a> SAP NetWeaver yüksek kullanılabilirlik Azure altyapısı olarak-hizmet (Iaas)
SAP yazılım bileşenleri için SAP uygulama yüksek kullanılabilirlik, gibi elde etmek için aşağıdaki bileşenler korumak gerekir:

* SAP uygulama sunucusu örneği
* SAP ASCS/SCS örneği
* DBMS sunucu

Yüksek kullanılabilirlik senaryolarını SAP bileşenlerinde koruma hakkında daha fazla bilgi için bkz: [Azure sanal makineleri planlama ve uygulama SAP NetWeaver için](planning-guide.md).

### <a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a> Yüksek kullanılabilirlik SAP uygulama sunucusu
SAP uygulama sunucusu ile iletişim örnekleri için belirli bir yüksek kullanılabilirlik çözümü genellikle gerekmez. Artıklık tarafından yüksek kullanılabilirlik elde etmek ve iletişim birden çok başka durumlarda, Azure sanal makineleri yapılandıracaksınız. İki durumlarda, Azure sanal makinelerinde yüklü en az iki SAP uygulama örneğinin olması gerekir.

![Şekil 4: Yüksek oranda kullanılabilirlik SAP uygulama sunucusu][sap-ha-guide-figure-2000]

_**Şekil 4:** yüksek kullanılabilirlik SAP uygulama sunucusu_

Ana bilgisayar SAP uygulama sunucusu örneklerinin aynı Azure kullanılabilirlik kümesi tüm sanal makineler yerleştirmeniz gerekir. Bir Azure kullanılabilirlik kümesi sağlar:

* Tüm sanal makineleri aynı yükseltme etki alanı'nın bir parçasıdır. Bir yükseltme etki alanı, örneğin, sanal makineler planlı bakım kapalı kalma süresi sırasında aynı anda güncelleştirilmemiş emin olur.
* Tüm sanal makineleri aynı hata etki alanı'nın bir parçasıdır. Hata etki alanı, örneğin, böylece hiç tek hata noktası tüm sanal makinelerin kullanılabilirliğini etkiler dağıtılan sanal makineler olduğundan emin olur.

Nasıl yapılır hakkında daha fazla bilgi [sanal makinelerin kullanılabilirliğini yönetme][virtual-machines-manage-availability].

Azure depolama hesabına olası tek hata noktası olduğundan, en az iki sanal makine dağıtılmış en az iki Azure depolama hesabı olması önemlidir. İdeal bir kurulumunda SAP iletişim örneği çalıştıran her bir sanal makine disklerinin farklı depolama hesabında dağıtılması.

### <a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a> Yüksek kullanılabilirlik SAP ASCS/SCS örneği
Şekil 5, yüksek kullanılabilirlik SAP ASCS/SCS örneğinin bir örnektir.

![Şekil 5: Yüksek oranda kullanılabilirlik SAP ASCS/SCS örneği][sap-ha-guide-figure-2001]

_**Şekil 5:** yüksek kullanılabilirlik SAP ASCS/SCS örneği_

#### <a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a> Windows Server Yük Devretme Kümelemesi Azure ile yüksek kullanılabilirlik SAP ASCS/SCS örneği
Çıplak metal veya özel bulut dağıtımları karşılaştırıldığında, Azure sanal makineleri Windows Server Yük Devretme Kümelemesi yapılandırmak için ek adımlar gerektirir. Windows Yük devretme kümesi oluşturmak için paylaşılan bir küme diski, birden fazla IP adresi, birçok sanal ana bilgisayar adlarını ve Azure iç yük dengeleyiciye SAP ASCS/SCS örneği kümeleme için gerekir. Biz bu makalenin sonraki bölümlerinde daha ayrıntılı ele alınmıştır.

![Şekil 6: Windows Server Yük Devretme Kümelemesi için SIOS DataKeeper kullanarak bir SAP ASCS/SCS yapılandırmasında Azure][sap-ha-guide-figure-1002]

_**Şekil 6:** Windows Server Yük Devretme Kümelemesi SIOS DataKeeper ile azure'da SAP ASCS/SCS yapılandırma_

### <a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>Yüksek kullanılabilirlik DBMS örneği
DBMS de tek bir kişinin bir SAP sisteminde noktasıdır. Yüksek kullanılabilirlik çözümü kullanarak korumanız gerekir. Şekil 7, Azure'da bir SQL Server Always On yüksek kullanılabilirlik çözümü gösterir, Windows Server Yük Devretme Kümelemesi ile Azure iç yük dengeleyici. SQL Server Always On kendi DBMS çoğaltma kullanılarak DBMS veri ve günlük dosyalarını çoğaltır. Bu durumda, paylaşılan diskleri, hangi tüm kurulumu basitleştirir küme.

![Şekil 7: SQL Server Always On ile bir yüksek kullanılabilirlik SAP DBMS örneği][sap-ha-guide-figure-2003]

_**Şekil 7:** SQL Server Always On ile bir yüksek kullanılabilirlik SAP DBMS örneği_

Azure Resource Manager dağıtım modelini kullanarak Azure SQL Server Kümelemesi hakkında daha fazla bilgi için bu makalelere bakın:

* [Always On kullanılabilirlik grubu Resource Manager kullanarak Azure sanal makinelerinde el ile yapılandırın.][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]
* [Azure'da Azure iç yük dengeleyiciye Always On kullanılabilirlik grubu için yapılandırın][virtual-machines-windows-portal-sql-alwayson-int-listener]

## <a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a> Uçtan uca yüksek kullanılabilirlik dağıtım senaryoları

### <a name="deployment-scenario-using-architectural-template-1"></a>Mimari şablonu 1'i kullanarak dağıtım senaryosu

Şekil 8 için azure'da bir SAP NetWeaver yüksek kullanılabilirlik mimari örneği gösterilmiştir **bir** SAP sistem. Bu senaryo aşağıdaki gibi kurun:

- Tek bir adanmış küme SAP ASCS/SCS örneği için kullanılır.
- Tek bir adanmış küme DBMS örneği için kullanılır.
- SAP uygulama sunucusu örnekleri kendi özel VM'ler dağıtılır.

![Şekil 8: yüksek oranda kullanılabilirlik mimari şablonu, 1 ile ayrılmış küme ASCS/SCS ve DBMS için SAP][sap-ha-guide-figure-2004]

_**Şekil 8:** ASCS/SCS ve DBMS ayrılmış kümelerine şablonu 1 mimari yüksek kullanılabilirlik, SAP_

### <a name="deployment-scenario-using-architectural-template-2"></a>Mimari şablon 2 kullanarak dağıtım senaryosu

Şekil 9 için azure'da bir SAP NetWeaver yüksek kullanılabilirlik mimari örneği gösterir **bir** SAP sistem. Bu senaryo aşağıdaki gibi kurun:

- Ayrılmış bir küme için kullanıldığından **her ikisi de** SAP ASCS/SCS örneği ve DBMS.
- SAP uygulama sunucusu örnekleri kendi özel VM'ler içinde dağıtılır.

![Şekil 9: yüksek oranda kullanılabilirlik mimari şablon 2 ile ayrılmış bir küme ASCS/SCS için ve ayrılmış bir küme DBMS için SAP][sap-ha-guide-figure-2005]

_**Şekil 9:** SAP yüksek kullanılabilirlik mimari şablon 2 ile ayrılmış bir küme ASCS/SCS için ve DBMS için ayrılmış bir küme_

### <a name="deployment-scenario-using-architectural-template-3"></a>Mimari şablonu 3 kullanan dağıtım senaryosu

Şekil 10 için azure'da bir SAP NetWeaver yüksek kullanılabilirlik mimari örneği gösterilir **iki** ile sistemleri, SAP &lt;SID1&gt; ve &lt;SID2&gt;. Bu senaryo aşağıdaki gibi kurun:

- Ayrılmış bir küme için kullanıldığından **her ikisi de** SAP ASCS/SCS SID1 örneği *ve* SAP ASCS/SCS SID2 örneği (bir küme için).
- Tek bir adanmış küme DBMS SID1 için kullanılır ve başka bir ayrılmış küme DBMS SID2 için kullanılır (iki küme).
- SAP uygulama sunucusu örneklerinin SAP sistemi SID1 için kendi özel VM'ler vardır.
- SAP uygulama sunucusu örneklerinin SAP sistemi SID2 için kendi özel VM'ler vardır.

![Şekil 10: yüksek kullanılabilirlik mimari şablonu 3, ile ayrılmış bir küme farklı ASCS/SCS örnekleri için SAP][sap-ha-guide-figure-6003]

_**Şekil 10:** SAP yüksek kullanılabilirlik mimari şablonu 3, ile ayrılmış bir küme için farklı ASCS/SCS örnekleri_

## <a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a> Altyapıyı hazırlama

### <a name="prepare-the-infrastructure-for-architectural-template-1"></a>Mimari şablonu 1 için altyapıyı hazırlama
Azure Resource Manager şablonları SAP için gerekli kaynakları dağıtımını kolaylaştırır.

Üç katmanlı şablonları Azure Kaynak Yöneticisi'nde de yüksek kullanılabilirlik senaryoları gibi mimari şablon iki küme olan 1'de destekler. Her küme bir SAP tek hata SAP ASCS/SCS ve DBMS noktasıdır.

İşte burada Biz bu makalede açıklayan örnek senaryo için Azure Resource Manager şablonları elde edebilirsiniz:

* [Azure Market görüntüsü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
* [Özel görüntü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)

Mimari şablonu 1 için altyapıyı hazırlamak için:

- Azure portalında üzerinde **parametreleri** dikey penceresindeki **SYSTEMAVAILABILITY** kutusunda **HA**.

  ![Şekil 11: Ayarlamak SAP yüksek kullanılabilirlik Azure Resource Manager parametreleri][sap-ha-guide-figure-3000]

_**Şekil 11:** ayarlamak SAP yüksek kullanılabilirlik Azure Resource Manager parametreleri_


  Şablonları oluşturun:

  * **Sanal makineler**:
    * SAP uygulama sunucusu sanal makineleri: <*SAPSystemSID*> - dı - <*numarası*>
    * ASCS/SCS küme sanal makineler: <*SAPSystemSID*> - ascs - <*numarası*>
    * DBMS küme: <*SAPSystemSID*> - db - <*numarası*>

  * **Ağ kartları ilişkili IP adresleriyle tüm sanal makineler için**:
    * <*SAPSystemSID*> - NIC - dı - <*numarası*>
    * <*SAPSystemSID*> - NIC - ascs - <*numarası*>
    * <*SAPSystemSID*> - NIC - db - <*numarası*>

  * **Azure depolama hesapları**

  * **Kullanılabilirlik grupları** için:
    * SAP uygulama sunucusu sanal makineleri: <*SAPSystemSID*> - avset - dı
    * SAP ASCS/SCS küme sanal makineler: <*SAPSystemSID*> - avset - ascs
    * DBMS küme sanal makineler: <*SAPSystemSID*> - avset - db

  * **Azure iç yük dengeleyici**:
    * IP adresi ve ASCS/SCS örneği için tüm bağlantı noktaları ile <*SAPSystemSID*> - lb - ascs
    * SQL Server DBMS ve IP adresi için tüm bağlantı noktaları ile <*SAPSystemSID*> - lb - db

  * **Ağ güvenlik grubu**: <*SAPSystemSID*> - nsg - ascs-0  
    * Açık bir dış Uzak Masaüstü Protokolü (RDP) bağlantı noktasına sahip <*SAPSystemSID*> - ascs - 0 sanal makine

> [!NOTE]
> Tüm IP adresleri ağ kartları ve Azure iç yük dengeleyicileri **dinamik** varsayılan olarak. Bunları değiştirmek **statik** IP adresleri. Biz bu makalenin sonraki bölümlerinde yapılacağı açıklanmaktadır.
>
>

### <a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a> Üretimde kullanılacak kurumsal ağ bağlantısı (şirket içi) ile sanal makineleri dağıtma
Üretim SAP sistemleri için Azure sanal makinelerle dağıtımı [kurumsal ağ bağlantısı (şirket içi)] [ planning-guide-2.2] Azure siteden siteye VPN veya Azure ExpressRoute kullanarak.

> [!NOTE]
> Azure Virtual Network örneğinizi kullanabilirsiniz. Sanal ağ ve alt ağ zaten oluşturulmuş hazırlanmış ve.
>
>

1.  Azure portalında üzerinde **parametreleri** dikey penceresinde, **NEWOREXISTINGSUBNET** kutusunda **varolan**.
2.  İçinde **SUBNETID** kutusunda, hazırlanan Azure alt ağınızı Azure sanal makinelerinizi dağıtmak planladığınız SubnetID tam dizesi ekleyin.
3.  Tüm Azure ağ alt ağlar listesini almak için bu PowerShell komutunu çalıştırın:

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

### <a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a> Test ve demo için yalnızca bulut SAP örnekleri dağıtma
Yüksek kullanılabilirlik SAP sisteminizi bir yalnızca bulut dağıtım modelinde dağıtabilirsiniz. Bu tür bir dağıtım öncelikle tanıtım ve test kullanım durumları için yararlıdır. Üretim kullanım durumları için uygun değildir.

- Azure portalında üzerinde **parametreleri** dikey penceresindeki **NEWOREXISTINGSUBNET** kutusunda **yeni**. Bırakın **SUBNETID** alanı boş.

  SAP Azure Resource Manager şablonu, Azure sanal ağ ve alt otomatik olarak oluşturur.

> [!NOTE]
> Ayrıca en az bir ayrılmış sanal makine Active Directory ve DNS için aynı Azure sanal ağ örneğinde dağıtmanız gerekir. Şablon, bu sanal makineleri oluşturmaz.
>
>


### <a name="prepare-the-infrastructure-for-architectural-template-2"></a>Mimari şablon 2 için altyapıyı hazırlama

Bu Azure Resource Manager şablonu SAP için gerekli altyapı kaynaklarıdır dağıtım SAP mimari şablon 2 basitleştirmeye yardımcı olması için kullanabilirsiniz.

İşte, bu dağıtım senaryosu için Azure Resource Manager şablonları burada alabilirsiniz:

* [Azure Market görüntüsü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged)  
* [Özel görüntü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged)


### <a name="prepare-the-infrastructure-for-architectural-template-3"></a>Mimari şablonu 3 için altyapıyı hazırlama

Altyapıyı hazırlama ve yapılandırma için SAP **çoklu SID**. Örneğin, ek bir SAP ASCS/SCS örneğine ekleyebileceğiniz bir *varolan* küme yapılandırması. Daha fazla bilgi için bkz: [Azure Kaynak Yöneticisi'nde bir SAP çoklu SID yapılandırması oluşturmak için var olan bir küme yapılandırmasını içine ek SAP ASCS/SCS örnek yapılandırma][sap-ha-multi-sid-guide].

Yeni bir SID çoklu küme oluşturmak istiyorsanız, çoklu SID kullanabilirsiniz [GitHub hızlı başlangıç şablonlarında](https://github.com/Azure/azure-quickstart-templates).
Yeni bir SID çoklu küme oluşturmak için aşağıdaki üç şablonlarını dağıtma gerekir:

* [ASCS/SCS şablonu](#ASCS-SCS-template)
* [Veritabanı şablonu](#database-template)
* [Uygulama sunucuları şablonu](#application-servers-template)

Aşağıdaki bölümlerde, şablonları ve şablonları sağlamanız gereken parametreleri hakkında daha fazla ayrıntı sahip.

#### <a name="ASCS-SCS-template"></a> ASCS/SCS şablonu

ASCS/SCS şablonu birden fazla ASCS/SCS örneği barındıran bir Windows Server Yük devretme kümesi oluşturmak için kullanabileceğiniz iki sanal makine dağıtır.

ASCS/SCS çoklu SID şablonunu, buna ayarlamak için [ASCS/SCS çoklu SID şablonu][sap-templates-3-tier-multisid-xscs-marketplace-image], aşağıdaki parametreler için değerler girin:

  - **Kaynak önek**.  Dağıtım sırasında oluşturulan tüm kaynakları öneki için kullanılan kaynak öneki ayarlayın. Kaynaklar için yalnızca bir SAP sistemine ait olmadığından kaynak önek bir SAP sistem SID'si değil.  Önek arasında olmalıdır **üç ve altı karakter**.
  - **Yığın türü**. SAP Sistem yığını türünü seçin. Yığın türüne bağlı olarak, Azure yük dengeleyici (ABAP veya yalnızca Java) bir veya iki (ABAP + Java) özel IP adresleri SAP sistem başına var.
  -  **İşletim sistemi türü**. Sanal makinelerin işletim sistemini seçin.
  -  **SAP sistem sayısı**. SAP sistemleri bu kümede yüklemek istediğiniz sayısını seçin.
  -  **Sistem kullanılabilirliğini**. Seçin **HA**.
  -  **Yönetici kullanıcı adı ve yönetici parolası**. Makinede oturum açmak için kullanılan yeni bir kullanıcı oluşturun.
  -  **Yeni veya var olan bir alt ağ**. Yeni sanal ağ ve alt oluşturulmalıdır veya mevcut bir alt kullanılmalıdır gerekip gerekmediğini belirleyin. Şirket içi ağınıza bağlı bir sanal ağ zaten varsa, seçin **varolan**.
  -  **Alt ağ kimliği**. Sanal makinelerin bağlanması alt ağ Kimliğini ayarlayın. Sanal özel ağ (VPN) veya sanal makine şirket içi ağınıza bağlamak için ExpressRoute sanal ağ alt ağı seçin. Kimliği genellikle şu şekildedir:

   /Subscriptions/ <*abonelik kimliği*> /resourceGroups/ <*kaynak grubu adı*> /providers/Microsoft.Network/virtualNetworks/ <*sanal ağ adı*> /subnets/ <*alt ağ adı*>

Şablon birden çok SAP sistemlerini destekleyen bir Azure yük dengeleyici örneği dağıtır.

- ASCS örnekleri örnek numarası 00, 10, 20 yapılandırılmış...
- SCS örnekleri örnek numarası 01, 11, 21 yapılandırılmış...
- ASCS kuyruğa çoğaltma sunucusuna (ERS) (yalnızca Linux) örnekleri örnek numarası 02, 12, 22 yapılandırılmış...
- SCS ERS (yalnızca Linux) örnekleri örneği için 03, 13, 23 sayısı yapılandırılan...

Yük Dengeleyici 1 (Linux için 2) içeren VIP(s), ASCS/SCS için 1 x VIP ve 1 x VIP ERS (yalnızca Linux) için.

Aşağıdaki listede tüm Yük Dengeleme kuralları (burada x, SAP sisteminin, örneğin, 1, 2, 3... sayıdır) içerir:
- Her SAP sistemi için Windows özel bağlantı noktaları: 445, 5985
- ASCS bağlantı noktası (örnek numarasını x0): 32 x 0, 36 x 0, 39 x 0, 81 x 0, 5 x 013, 5 x 014, 5 x 016
- SCS bağlantı noktası (örnek numarasını x1): 32 x 1, 33 x 1, 39 x 1, 81 x 1, 5 x 113, 5 x 114, 5 x 116
- ASCS ERS bağlantı noktaları (örnek numarasını x2) Linux'ta: 33 x 2, 5 x 213, 5 x 214, 5 x 216
- SCS ERS bağlantı noktaları (örnek numarasını x3) Linux'ta: 33 x 3, 5 x 313, 5 x 314, 5 x 316

Yük Dengeleyici (burada x, SAP sisteminin, örneğin, 1, 2, 3... sayıdır) aşağıdaki araştırma bağlantı noktalarını kullanacak şekilde yapılandırılır:
- ASCS/SCS iç yük dengeleyici araştırması bağlantı noktası: 620 x 0
- ERS iç yük dengeleyici araştırması bağlantı noktası (yalnızca Linux): 621 x 2

#### <a name="database-template"></a> Veritabanı şablonu

Veritabanı şablonu bir veya iki sanal bir SAP sistem ilişkisel veritabanı yönetim sistemi (RDBMS) yüklemek için kullanabileceğiniz makineleri dağıtır. Örneğin, beş SAP sistemleri için bir ASCS/SCS şablonu dağıtırsanız, bu şablonu beş kez dağıtmak için gerekir.

Veritabanı çoklu SID şablonu ayarlamak için [veritabanı çoklu SID şablonu][sap-templates-3-tier-multisid-db-marketplace-image], aşağıdaki parametreler için değerler girin:

  -  **SAP sistem kimliği**. Yüklemek istediğiniz SAP sistem SAP sistem Kimliğini girin. Kimliği önek olarak dağıtılan kaynaklar için kullanılır.
  -  **İşletim sistemi türü**. Sanal makinelerin işletim sistemini seçin.
  -  **DbType**. Kümede yüklemek istediğiniz veritabanını seçin. Seçin **SQL** Microsoft SQL Server yüklemek istiyorsanız. Seçin **HANA** sanal makinelerde SAP HANA yüklemeyi planlıyorsanız. Doğru işletim sistemi türü seçtiğinizden emin olun: seçin **Windows** HANA için Linux dağıtımı seçin ve SQL için. Sanal makinelere bağlı Azure yük dengeleyici, seçili veritabanı türünü desteklemek üzere yapılandırılır:
    * **SQL**. Yük Dengeleyici Yük Dengeleme bağlantı noktası 1433 olur. SQL Server Always On ayarlarınızı bu bağlantı noktasını kullandığınızdan emin olun.
    * **HANA**. Yük Dengeleyici Yük Dengeleme 35015 ve 35017 bağlantı noktalarını olur. SAP HANA ile örnek numarasını yüklediğinizden emin olun **50**.
    Yük Dengeleyici araştırması bağlantı noktasını 62550 kullanır.
  -  **SAP sistem boyutu**. Yeni Sistem sağlayacak SAP sayısını ayarlayın. Sistem gerektirir kaç SAP değil eminseniz, SAP teknolojisi iş ortağı veya sistem Tümleştirici isteyin.
  -  **Sistem kullanılabilirliğini**. Seçin **HA**.
  -  **Yönetici kullanıcı adı ve yönetici parolası**. Makinede oturum açmak için kullanılan yeni bir kullanıcı oluşturun.
  -  **Alt ağ kimliği**. ASCS/SCS şablon dağıtımı sırasında kullanılan alt ağ kimliği veya oluşturulmuş alt ağ kimliği ASCS/SCS şablonu dağıtımının bir parçası girin.

#### <a name="application-servers-template"></a> Uygulama sunucuları şablonu

Uygulama sunucuları şablonu iki veya daha fazla sanal SAP uygulama sunucusu örneklerinin bir SAP sistemi için kullanılabilecek makineler dağıtır. Örneğin, beş SAP sistemleri için bir ASCS/SCS şablonu dağıtırsanız, bu şablonu beş kez dağıtmak için gerekir.

Uygulama sunucuları çoklu SID şablonu ayarlamak için [uygulama sunucuları çoklu SID şablonu][sap-templates-3-tier-multisid-apps-marketplace-image], aşağıdaki parametreler için değerler girin:

  -  **SAP sistem kimliği**. Yüklemek istediğiniz SAP sistem SAP sistem Kimliğini girin. Kimliği önek olarak dağıtılan kaynaklar için kullanılır.
  -  **İşletim sistemi türü**. Sanal makinelerin işletim sistemini seçin.
  -  **SAP sistem boyutu**. Yeni Sistem sağlayacak SAP sayısı. Sistem gerektirir kaç SAP değil eminseniz, SAP teknolojisi iş ortağı veya sistem Tümleştirici isteyin.
  -  **Sistem kullanılabilirliğini**. Seçin **HA**.
  -  **Yönetici kullanıcı adı ve yönetici parolası**. Makinede oturum açmak için kullanılan yeni bir kullanıcı oluşturun.
  -  **Alt ağ kimliği**. ASCS/SCS şablon dağıtımı sırasında kullanılan alt ağ kimliği veya oluşturulmuş alt ağ kimliği ASCS/SCS şablonu dağıtımının bir parçası girin.


### <a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a> Azure sanal ağı
Bizim örneğimizde, Azure sanal ağ adres alanı 10.0.0.0/16 şeklindedir. Adlı bir alt ağ yok **alt**, 10.0.0.0/24 adres aralığı olan. Bu sanal ağda, tüm sanal makineler ve iç yük dengeleyicileri dağıtılır.

> [!IMPORTANT]
> Ağ ayarları konuk işletim sistemi içinde herhangi bir değişiklik yoktur. Bu IP adresleri, DNS sunucuları ve alt ağ içerir. Azure'da tüm ağ ayarlarını yapılandırın. Dinamik Ana Bilgisayar Yapılandırma Protokolü (DHCP) hizmeti ayarlarınızı yayar.
>
>

### <a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a> DNS IP adresleri

Gerekli DNS IP adreslerini ayarlamak için aşağıdaki adımları uygulayın.

1.  Azure portalında üzerinde **DNS sunucuları** dikey penceresinde olduğundan emin olun, sanal ağınızı **DNS sunucuları** seçeneği **özel DNS**.
2.  Sahip olduğunuz ağ türüne göre ayarlarınızı seçin. Daha fazla bilgi için aşağıdaki kaynaklara bakın:
    * [Kurumsal ağ bağlantısı (şirket içi)][planning-guide-2.2]: şirket içi DNS sunucularının IP adreslerini ekleyin.  
    Azure'da çalışan sanal makineleri şirket içi DNS sunucularına genişletebilirsiniz. Bu senaryoda DNS hizmeti çalışan Azure sanal makinelerin IP adreslerini ekleyebilirsiniz.
    * [Yalnızca bulut dağıtım][planning-guide-2.1]: bir DNS sunucusu olarak hizmet veren aynı sanal ağ örneğinde ek bir sanal makine dağıtın. DNS hizmeti çalıştırmak için ayarladığınız Azure sanal makinelerin IP adreslerini ekleyin.

    ![Şekil 12: Azure sanal ağı için DNS sunucularını yapılandırın][sap-ha-guide-figure-3001]

    _**Şekil 12:** Yapılandır DNS sunucuları için Azure sanal ağ_

  > [!NOTE]
  > DNS sunucularının IP adreslerini değiştirirseniz, değişikliği uygulamak ve yeni DNS sunucularını yayılması için Azure sanal makineleri yeniden başlatmanız gerekir.
  >
  >

Bizim örneğimizde, DNS hizmeti yüklenir ve bu Windows sanal makinelerde yapılandırılır:

| Sanal makine rolü | Sanal makine ana bilgisayar adı | Ağ kartı adı | Statik IP adresi |
| --- | --- | --- | --- |
| İlk DNS sunucusu |domcontr-0 |pr1-NIC-domcontr-0 |10.0.0.10 |
| İkinci DNS sunucusu |domcontr-1 |pr1-NIC-domcontr-1 |10.0.0.11 |

### <a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a> Ana bilgisayar adları ve SAP ASCS/SCS kümelenmiş örneği ve DBMS kümelenmiş örneği için statik IP adresleri

Şirket içi dağıtım için bu ayrılmış ana bilgisayar adlarını ve IP adreslerini gerekir:

| Sanal ana bilgisayar adı rolü | Sanal ana bilgisayar adı | Sanal statik IP adresi |
| --- | --- | --- |
| SAP ASCS/SCS ilk küme sanal ana bilgisayar adı (küme yönetimi) |pr1 ascs VIR |10.0.0.42 |
| SAP ASCS/SCS örnek sanal ana bilgisayar adı |pr1 ascs sap |10.0.0.43 |
| SAP DBMS ikinci küme sanal ana bilgisayar adı (küme yönetimi) |pr1-dbms-vir |10.0.0.32 |

Kümeyi oluşturduğunuzda, sanal ana bilgisayar adlarını oluşturmak **pr1 ascs VIR** ve **pr1 dbms VIR** ve kümeyi yönetmek ilişkili IP adreslerini. Bunun nasıl yapılacağı hakkında daha fazla bilgi için bkz: [toplamak küme düğümleri bir küme yapılandırmasında][sap-ha-guide-8.12.1].

Diğer iki sanal ana bilgisayar adlarını, el ile oluşturabilirsiniz **pr1 ascs sap** ve **pr1 dbms sap**ve ilişkili IP adresleri, DNS sunucusu. Kümelenmiş SAP ASCS/SCS örneği ve kümelenmiş DBMS örneği bu kaynakları kullanın. Bunun nasıl yapılacağı hakkında daha fazla bilgi için bkz: [kümelenmiş bir SAP ASCS/SCS örneği için bir sanal ana bilgisayar adı oluşturmak][sap-ha-guide-9.1.1].

### <a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a> SAP sanal makineler için statik IP adresi ayarlayın
Kümedeki sanal makinelerin dağıttıktan sonra tüm sanal makineler için statik IP adresi ayarlamak gerekir. Bunu yapmak, Azure sanal ağ yapılandırması ve konuk işletim sistemi içinde değil.

1.  Azure portalında seçin **kaynak grubu** > **ağ kartı** > **ayarları** > **IP adresi**.
2.  Üzerinde **IP adreslerini** dikey altında **atama**seçin **statik**. İçinde **IP adresi** kutusunda, kullanmak istediğiniz IP adresini girin.

  > [!NOTE]
  > Ağ kartı IP adresini değiştirirseniz, değişikliği uygulamak için Azure sanal makineleri yeniden başlatmanız gerekir.  
  >
  >

  ![Şekil 13: statik IP adresleri her sanal makinenin ağ kartı için ayarlama][sap-ha-guide-figure-3002]

  _**Şekil 13:** ayarlamak, her bir sanal makinenin ağ kartı için statik IP adresleri_

  Tüm sanal makineler için Active Directory DNS hizmetiniz için kullanmak istediğiniz sanal makineleri dahil olan tüm ağ arabirimleri için bu adımı yineleyin.

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

### <a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a> Azure iç yük dengeleyici için statik bir IP adresi ayarlayın

SAP Azure Resource Manager şablonu SAP ASCS/SCS örnek küme ve DBMS küme için kullanılan bir Azure iç yük dengeleyici oluşturur.

> [!IMPORTANT]
> Sanal ana bilgisayar adını SAP ASCS/SCS IP adresini SAP ASCS/SCS iç yük dengeleyici IP adresi ile aynıdır: **pr1 lb ascs**.
> DBMS sanal adını IP adresini DBMS iç yük dengeleyici IP adresi ile aynıdır: **pr1 lb dbms**.
>
>

Azure iç yük dengeleyici için statik bir IP adresi ayarlamak için:

1.  İç yük dengeleyici IP adresi ilk dağıtım ayarlar **dinamik**. Azure portalında üzerinde **IP adreslerini** dikey altında **atama**seçin **statik**.
2.  İç yük dengeleyici IP adresi kümesi **pr1 lb ascs** SAP ASCS/SCS örneğinin sanal ana bilgisayar adı IP adresine.
3.  İç yük dengeleyici IP adresi kümesi **pr1 lb dbms** DBMS örneğinin sanal ana bilgisayar adı IP adresine.

  ![Şekil 14: SAP ASCS/SCS örneği için iç yük dengeleyici için statik IP adresleri ayarlayın.][sap-ha-guide-figure-3003]

  _**Şekil 14:** SAP ASCS/SCS örneği için iç yük dengeleyici için statik IP adresleri ayarlayın_

Bizim örneğimizde, biz bu statik IP adresine sahip iki Azure iç yük dengeleyicileri vardır:

| Azure iç yük dengeleyici rol | Azure iç yük dengeleyicisi adı | Statik IP adresi |
| --- | --- | --- |
| SAP ASCS/SCS iç yük dengeleyici örneği |pr1 lb ascs |10.0.0.43 |
| SAP DBMS iç yük dengeleyici |pr1 lb dbms |10.0.0.33 |


### <a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a> Varsayılan ASCS/SCS Yük Dengeleme kuralları Azure iç yük dengeleyici için

SAP Azure Resource Manager şablonu gereksinim duyduğunuz bağlantı noktalarını oluşturur:
* Varsayılan örneği numarasıyla ABAP ASCS örneği **00**
* Bir Java SCS örneğiyle varsayılan örnek numarasını **01**

SAP ASCS/SCS örneğinizi yüklediğinizde, varsayılan örnek numarasını kullanmalıdır **00** ABAP ASCS örneğinizi ve varsayılan örnek sayısı için **01** Java SCS Örneğiniz için.

Ardından, gerekli iç Yük Dengeleme SAP NetWeaver bağlantı noktaları için uç noktaları oluşturun.

Gerekli iç Yük Dengeleme uç noktaları oluşturmak için ilk olarak, bu Yük Dengeleme SAP NetWeaver ABAP ASCS bağlantı noktaları için uç nokta oluşturun:

| Hizmet/Yük Dengeleme kuralı adı | Varsayılan bağlantı noktası numaraları | Somut için bağlantı noktalarını (ASCS örneği ile örnek numarasını 00) (ERS 10 ile) |
| --- | --- | --- |
| Sıraya alma sunucu / *lbrule3200* |32 <*InstanceNumber*> |3200 |
| ABAP ileti sunucusu / *lbrule3600* |36 <*InstanceNumber*> |3600 |
| İç ABAP ileti / *lbrule3900* |39 <*InstanceNumber*> |3900 |
| Sunucu HTTP iletisi / *Lbrule8100* |81 <*InstanceNumber*> |8100 |
| SAP başlangıç hizmet ASCS HTTP / *Lbrule50013* |5 <*InstanceNumber*> 13 |50013 |
| SAP başlangıç hizmet ASCS HTTPS / *Lbrule50014* |5 <*InstanceNumber*> 14 |50014 |
| Sıraya alma çoğaltma / *Lbrule50016* |5 <*InstanceNumber*> 16 |50016 |
| SAP başlangıç hizmeti ERS HTTP *Lbrule51013* |5 <*InstanceNumber*> 13 |51013 |
| SAP başlangıç hizmeti ERS HTTP *Lbrule51014* |5 <*InstanceNumber*> 14 |51014 |
| RM win *Lbrule5985* | |5985 |
| Dosya Paylaşımı *Lbrule445* | |445 |

_**Tablo 1:** bağlantı noktası numaralarını SAP NetWeaver ABAP ASCS örnekleri_

Ardından, bu Yük Dengeleme SAP NetWeaver Java SCS bağlantı noktaları için uç nokta oluşturun:

| Hizmet/Yük Dengeleme kuralı adı | Varsayılan bağlantı noktası numaraları | Somut için bağlantı noktalarını (SCS örneği ile örnek numarasını 01) (ERS 11 ile) |
| --- | --- | --- |
| Sıraya alma sunucu / *lbrule3201* |32 <*InstanceNumber*> |3201 |
| Ağ Geçidi sunucusu / *lbrule3301* |33 <*InstanceNumber*> |3301 |
| Java ileti sunucusu / *lbrule3900* |39 <*InstanceNumber*> |3901 |
| Sunucu HTTP iletisi / *Lbrule8101* |81 <*InstanceNumber*> |8101 |
| SAP başlangıç hizmet SCS HTTP / *Lbrule50113* |5 <*InstanceNumber*> 13 |50113 |
| SAP başlangıç hizmet SCS HTTPS / *Lbrule50114* |5 <*InstanceNumber*> 14 |50114 |
| Sıraya alma çoğaltma / *Lbrule50116* |5 <*InstanceNumber*> 16 |50116 |
| SAP başlangıç hizmeti ERS HTTP *Lbrule51113* |5 <*InstanceNumber*> 13 |51113 |
| SAP başlangıç hizmeti ERS HTTP *Lbrule51114* |5 <*InstanceNumber*> 14 |51114 |
| RM win *Lbrule5985* | |5985 |
| Dosya Paylaşımı *Lbrule445* | |445 |

_**Tablo 2:** bağlantı noktası numaralarını SAP NetWeaver Java SCS örnekleri_

![Şekil 15: varsayılan ASCS/SCS Yük Dengeleme kuralları Azure iç yük dengeleyici için][sap-ha-guide-figure-3004]

_**Şekil 15:** varsayılan ASCS/SCS Yük Dengeleme kuralları Azure iç yük dengeleyici için_

Yük Dengeleyici IP adresi kümesi **pr1 lb dbms** DBMS örneğinin sanal ana bilgisayar adı IP adresine.

### <a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a> ASCS/SCS varsayılan Yük Dengeleme kuralları Azure iç yük dengeleyici için değiştirme

SAP ASCS veya SCS örnekleri için farklı numaraları kullanmak istiyorsanız, adlarını ve değerlerini kendi bağlantı noktalarının varsayılan değerleri değiştirmeniz gerekir.

1.  Azure portalında seçin  **< *SID*> - lb - ascs yük dengeleyici** > **Yük Dengeleme kuralları**.
2.  Tüm Yük Dengeleme SAP ASCS veya SCS örneğine ait kuralları için bu değerleri değiştirin:

  * Ad
  * Bağlantı noktası
  * Arka uç bağlantı noktası

  Örneğin, varsayılan ASCS örnek numarasını 00-31 değiştirmek istiyorsanız, Tablo 1'de listelenen tüm bağlantı noktaları için değişiklikler yapmanız gerekir.

  Bağlantı noktası için bir güncelleştirme örneği *lbrule3200*.

  ![Şekil 16: ASCS/SCS varsayılan Yük Dengeleme kuralları Azure iç yük dengeleyici için değiştirme][sap-ha-guide-figure-3005]

  _**Şekil 16:** ASCS/SCS varsayılan Yük Dengeleme kuralları Azure iç yük dengeleyici için değiştirme_

### <a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a> Windows sanal makine etki alanına ekleyin

Sanal makineler için statik bir IP adresi atadıktan sonra sanal makine etki alanına ekleyin.

![Şekil 17: bir sanal makine bir etki alanına ekleyin.][sap-ha-guide-figure-3006]

_**Şekil 17:** bir sanal makine bir etki alanına ekleme_

### <a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a> SAP ASCS/SCS örneği her iki küme düğümleri üzerinde kayıt defteri girdilerini ekleyin

Azure yük dengeleyici bağlantıları ayarlanmış bir süre boyunca boşta olduğunda kapanır bağlantıları (boşta zaman aşımı) zaman bir iç yük dengeleyici sahiptir. İlk sıraya alma/dequeue gönderilmesi gerekiyor isteği hemen SAP iş iletişim örnekleri açık bağlantıları SAP sıraya alma işlemlerinde işleyin. Bu bağlantılar genellikle iş işlemi kadar kurulan kalmasını veya sıraya alma işlemi yeniden başlatır. Ancak, belirlenen bir süre için bağlantı boşta kalırsa Azure iç yük dengeleyicisi bağlantıları kapatır. Artık yoksa SAP iş işlemi sıraya alma işlemi için bağlantıyı yeniden kurar çünkü bu bir sorun değildir. Bu etkinlikler SAP işlemlerini Geliştirici izlerini belgelenen, ancak bunlar büyük miktarda ek içerik bu izlemeler oluşturur. TCP/IP'yi değiştirmek için iyi bir fikirdir `KeepAliveTime` ve `KeepAliveInterval` her iki küme düğümlerinde. Bu değişiklikler makalenin sonraki bölümlerinde açıklanan SAP profili parametreleri TCP/IP'yi parametrelerle ile birleştirin.

Kayıt defteri girdileri SAP ASCS/SCS örneği üzerinde her iki küme düğümü eklemek için ilk olarak, bu Windows kayıt defteri girdileri hem Windows Küme düğümlerinde SAP ASCS/SCS için ekleyin:

| Yol | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Değişken adı |`KeepAliveTime` |
| Değişken türü |REG_DWORD (ondalık) |
| Değer |120000 |
| Belgelere bağlantı |[https://technet.microsoft.com/library/cc957549.aspx](https://technet.microsoft.com/library/cc957549.aspx) |

_**Tablo 3:** ilk TCP/IP'yi parametre değiştirme_

Daha sonra bu Windows kayıt defteri girdileri SAP ASCS/SCS için hem Windows küme düğümlerine ekleyin:

| Yol | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Değişken adı |`KeepAliveInterval` |
| Değişken türü |REG_DWORD (ondalık) |
| Değer |120000 |
| Belgelere bağlantı |[https://technet.microsoft.com/library/cc957548.aspx](https://technet.microsoft.com/library/cc957548.aspx) |

_**Tablo 4:** ikinci TCP/IP'yi parametre değiştirme_

**Değişiklikleri uygulamak için her iki küme düğümlerini yeniden**.

### <a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a> Bir Windows Server Yük Devretme Kümelemesi küme SAP ASCS/SCS örneği için ayarlama

Bir Windows Server Yük Devretme Kümelemesi küme SAP ASCS/SCS örneği için ayarlama, bu görevleri içerir:

- Bir küme yapılandırmasında küme düğümleri toplama
- Bir küme dosya paylaşımı tanığı yapılandırma

#### <a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a> Bir küme yapılandırmasında küme düğümleri Topla

1.  Rol Ekle ve Özellik Ekleme Sihirbazı, her iki küme düğümü yük devretme ekleyin.
2.  Yük devretme kümesini yedeklerken, yük devretme kümesi Yöneticisi'ni kullanarak ayarlayın. Yük Devretme Kümesi Yöneticisi'nde seçin **küme oluşturma**ve ardından yalnızca ilk küme düğüm A. adı ekleyin İkinci düğümü henüz eklemeyin; İkinci düğümü bir sonraki adımda ekleyeceksiniz.

  ![Şekil 18: ilk küme düğümüne sunucu veya sanal makine adını ekleyin][sap-ha-guide-figure-3007]

  _**Şekil 18:** ilk küme düğümüne sunucu veya sanal makine adını ekleyin_

3.  Küme ağ adı (sanal ana bilgisayar adı) girin.

  ![Şekil 19: küme adını girin][sap-ha-guide-figure-3008]

  _**Şekil 19:** küme adını girin_

4.  Kümeyi oluşturduktan sonra bir küme doğrulama testi çalıştırın.

  ![Şekil 20: küme doğrulama denetimini Çalıştır][sap-ha-guide-figure-3009]

  _**Şekil 20:** küme doğrulama denetimini Çalıştır_

  Bu noktada işleminde disklerle ilgili tüm uyarılar yoksayabilirsiniz. Dosya paylaşım tanığı ve SIOS diskler daha sonra paylaşılan ekleyeceksiniz. Bu aşamada, bir çekirdek sahibi olma hakkında endişelenmeniz gerekmez.

  ![Şekil 21: Çekirdek disk bulunamadı][sap-ha-guide-figure-3010]

  _**Şekil 21:** çekirdek disk bulunamadı_

  ![Şekil 22: Çekirdek küme kaynağının yeni bir IP adresi gerekiyor.][sap-ha-guide-figure-3011]

  _**Şekil 22:** çekirdek küme kaynağının yeni bir IP adresi gerekiyor_

5.  Çekirdeği Küme hizmetinin IP adresini değiştirin. Çekirdeği Küme hizmeti, IP adresini değiştirme kadar sanal makine düğümlerinden biri için sunucunun IP adresini işaret ettiğinden küme başlatılamaz. Bunu yapmak **özellikleri** çekirdek Küme hizmeti IP kaynak sayfası.

  Örneğin, bir IP adresi atamak ihtiyacımız (örneğimizde **10.0.0.42**) küme sanal ana bilgisayar adı için **pr1 ascs VIR**.

  ![Şekil 23: Özellikler iletişim kutusunda, IP adresini değiştirme][sap-ha-guide-figure-3012]

  _**Şekil 23:** içinde **özellikleri** iletişim kutusunda, IP adresini değiştirme_

  ![Şekil 24: küme için ayrılmış bir IP adresi atayın][sap-ha-guide-figure-3013]

  _**Şekil 24:** küme için ayrılmış bir IP adresi atayın_

6.  Küme sanal ana bilgisayar adı çevrimiçi duruma getirin.

  ![Şekil 25: Küme çekirdek hizmeti çalışır durumda ve çalıştığından ve doğru IP adresi][sap-ha-guide-figure-3014]

  _**Şekil 25:** küme çekirdek hizmeti çalışır durumda ve çalıştığından ve doğru IP adresi_

7.  İkinci küme düğümünü ekleyin.

  Çekirdeği Küme hizmeti çalışır durumda olduğundan, ikinci küme düğümü ekleyebilirsiniz.

  ![Şekil 26: ikinci küme düğümü ekleyin.][sap-ha-guide-figure-3015]

  _**Şekil 26:** ikinci küme düğümü ekleyin._

8.  İkinci küme düğümü ana bilgisayar için bir ad girin.

  ![Şekil 27: ikinci küme düğümü ana bilgisayar adı girin][sap-ha-guide-figure-3016]

  _**Şekil 27:** ikinci küme düğümü ana bilgisayar adını girin_

  > [!IMPORTANT]
  > Olduğundan emin olun **tüm uygun depolamayı kümeye eklemek** onay kutusu **değil** seçili.  
  >
  >

  ![Şekil 28: onay kutusunu seçin][sap-ha-guide-figure-3017]

  _**Şekil 28:** yapmak **değil** onay kutusunu seçin_

  Çekirdek ve diskleri ilgili uyarılar yoksayabilirsiniz. Çekirdek ayarlayın ve diski daha sonra açıklandığı şekilde paylaşma [SIOS DataKeeper Cluster Edition yükleme SAP ASCS/SCS küme paylaşım diski için][sap-ha-guide-8.12.3].

  ![Şekil 29: disk çekirdek ilgili uyarılar yoksay][sap-ha-guide-figure-3018]

  _**Şekil 29:** disk çekirdek ilgili uyarılar yoksay_


#### <a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a> Bir küme dosya paylaşımı tanığı Yapılandır

Bir küme dosya paylaşımı tanığı yapılandırma, bu görevleri içerir:

- Bir dosya paylaşımı oluşturma
- Yük Devretme Kümesi Yöneticisi'nde dosya paylaşım tanığı çekirdek ayarlama

##### <a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a> Dosya paylaşımı oluşturma

1.  Dosya paylaşım tanığı yerine bir çekirdek diski seçin. Bu seçenek SIOS DataKeeper destekler.

  Bu makaledeki örneklerde, Azure'da çalışan Active Directory/DNS sunucusunun dosya paylaşımı tanığı açıktır. Dosya paylaşım tanığı olarak adlandırılır **domcontr 0**. Azure (aracılığıyla, siteden siteye VPN veya Azure ExpressRoute) bir VPN bağlantısı yapılandırılmış için Active Directory/Hizmeti şirket içi ve bir dosyasını çalıştırmak uygun değil. DNS sunucunuzun Tanık paylaşır.

  > [!NOTE]
  > Yalnızca şirket içi Active Directory DNS hizmeti çalışıyorsa, dosya paylaşım tanığı Active Directory/DNS Windows işletim sisteminde şirket içi çalışan yapılandırmayın. Azure ve Active Directory DNS şirket içi çalışan küme düğümleri arasındaki ağ gecikmesi çok büyük ve bağlantı sorunlarına neden olabilir. Dosya paylaşım tanığı, küme düğümü yakın çalıştıran Azure sanal makinede yapılandırdığınızdan emin olun.  
  >
  >

  Çekirdek sürücüde en az 1024 MB boş disk alanı gerekir. 2.048 MB boş disk alanı çekirdek sürücüsü için öneririz.

2.  Küme adı nesnesi ekleyin.

  ![Şekil 30: küme adı nesnesi paylaşımı üzerindeki izinleri atama][sap-ha-guide-figure-3019]

  _**Şekil 30:** küme adı nesnesi paylaşımı üzerindeki izinleri atama_

  İzinleri küme adı nesnesi için paylaşım uygulamasında verileri değiştirme yetkisi içeren emin olun (örneğimizde **pr1 ascs VIR$**).

3.  Küme adı nesnesi listesine eklemek için seçin **Ekle**. Şekil 31'de gösterilen ek olarak bilgisayar nesneleri denetlemek için filtreyi değiştirin.

  ![Şekil 31: Değişiklik bilgisayarları dahil etme nesne türleri][sap-ha-guide-figure-3020]

  _**Şekil 31:** bilgisayarları dahil etme nesne türlerini değiştirme_

  ![Şekil 32: bilgisayarlar onay kutusunu seçin][sap-ha-guide-figure-3021]

  _**Şekil 32:** seçin **bilgisayarlar** onay kutusu_

4.  Küme adı nesnesi şekil 31'de gösterildiği gibi girin. Kaydı zaten oluşturulduğundan, Şekil 30 gösterildiği gibi izinlerini değiştirebilirsiniz.

5.  Seçin **güvenlik** paylaşım ayarlayın ve ardından sekmesinde daha ayrıntılı küme adı nesnesi için izinleri.

  ![Şekil 33: dosya paylaşımı çekirdeği küme adı nesnesi için güvenlik özniteliklerini ayarlama][sap-ha-guide-figure-3022]

  _**Şekil 33:** dosya paylaşımı çekirdeği küme adı nesnesi için güvenlik özniteliklerini ayarla_

##### <a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a> Yük Devretme Kümesi Yöneticisi'nde dosya paylaşım tanığı çekirdek ayarlayın

1.  Açık çekirdek Ayarlama Sihirbazı'nı yapılandırın.

  ![Şekil 34: yapılandırma küme çekirdeği Ayarlama Sihirbazı'nı Başlat][sap-ha-guide-figure-3023]

  _**Şekil 34:** yapılandırma küme çekirdeği Ayarlama Sihirbazı'nı Başlat_

2.  Üzerinde **seçin Çekirdek yapılandırmasını** sayfasında **çekirdek tanığı Seç**.

  ![Şekil 35: aralarından seçim yapabileceğiniz Çekirdek yapılandırmaları][sap-ha-guide-figure-3024]

  _**Şekil 35:** seçim yapabileceğiniz Çekirdek yapılandırmaları_

3.  Üzerinde **çekirdek tanığı Seç** sayfasında **bir dosya paylaşımı tanığı Yapılandır**.

  ![Şekil 36: dosya paylaşım tanığı seçin][sap-ha-guide-figure-3025]

  _**Şekil 36:** dosya paylaşım tanığı seçin_

4.  Dosya Paylaşımı için UNC yolunu girin (örneğimizde \\domcontr 0\FSW). Yapabilirsiniz yapılan değişikliklerin bir listesi görmek için seçin **sonraki**.

  ![Şekil 37: Tanık paylaşımı için dosya paylaşım konumunu tanımlayın][sap-ha-guide-figure-3026]

  _**Şekil 37:** Tanık paylaşımı için dosya paylaşım konumunu tanımlayın_

5.  Seçin ve ardından değişiklikleri **sonraki**. Küme yapılandırmasını şekil 38 gösterildiği gibi başarılı bir şekilde yeniden yapılandırmanız gerekir.  

  ![Şekil 38: küme yeniden yapılandırılması onayı][sap-ha-guide-figure-3027]

  _**Şekil 38:** kümeye yeniden yapılandırılması onayı_

Windows Yük devretme kümesi başarıyla yükledikten sonra değişiklikler bazı eşikleri Azure koşullar için yük devretme algılama uyarlamak için yapılması gerekir. Değiştirilecek parametreleri bu blog belgelenmiştir: https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/ . Windows Küme yapılandırması ASCS/SCS için derleme, iki VM aynı alt ağda olduğu varsayımıyla, aşağıdaki parametreleri bu değerleri değiştirilmesi gerekebilir:
- SameSubNetDelay = 2
- SameSubNetThreshold = 15

Bu ayarları müşterilerle test ve bir tarafta yeterince dayanıklı olması için iyi bir güvenlik açığı sağlanan. Öte yandan bu ayarları gerçek hata koşulları yeterli yük SAP yazılım veya düğüm/VM hatada sağlama hızlı. 

### <a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a> SAP ASCS/SCS küme paylaşım diski için SIOS DataKeeper küme Edition'ı yükleme

Artık Azure üzerinde çalışan bir Windows Server Yük Devretme Kümelemesi yapılandırma vardır. Ancak, SAP ASCS/SCS örneği yüklemek için paylaşılan disk kaynağı gerekir. İhtiyacınız olan paylaşılan disk kaynakları Azure'da oluşturulamıyor. Paylaşılan disk kaynakları oluşturmak için kullanabileceğiniz bir üçüncü taraf çözümü SIOS DataKeeper küme sürümüdür.

SAP ASCS/SCS küme paylaşım diski için SIOS DataKeeper Cluster Edition yüklemek, bu görevleri içerir:

- .NET Framework 3.5 ekleme
- SIOS DataKeeper yükleme
- SIOS DataKeeper ayarlama

#### <a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a> .NET Framework 3.5 ekleme
Microsoft .NET Framework 3.5 otomatik olarak etkinleştirilmiş veya Windows Server 2012 R2'de yüklü değil. SIOS DataKeeper DataKeeper yüklemek, tüm düğümlerde olması için .NET Framework'ü gerektirdiğinden, .NET Framework 3.5, kümedeki tüm sanal makineler konuk işletim sisteminde yüklemeniz gerekir.

.NET Framework 3.5 eklemek için iki yolu vardır:

- Ekle roller ve Özellikler Sihirbazı'nı Windows şekil 39 gösterildiği gibi kullanın.

  ![Şekil 39: Ekle roller ve Özellikler Sihirbazı'nı kullanarak .NET Framework 3.5 yükleyin.][sap-ha-guide-figure-3028]

  _**Şekil 39:** Ekle roller ve Özellikler Sihirbazı'nı kullanarak .NET Framework 3.5 yükleyin_

  ![Şekil 40: yükleme ilerleme çubuğu Ekle roller ve Özellikler Sihirbazı'nı kullanarak .NET Framework 3.5 yüklediğinizde][sap-ha-guide-figure-3029]

  _**Şekil 40:** Ekle roller ve Özellikler Sihirbazı'nı kullanarak .NET Framework 3.5 yüklediğinizde çubuğu yükleme ilerleme durumu_

- Komut satırı aracı dism.exe kullanın. Bu yükleme türü için Windows yükleme medyasında bulunan SxS dizin erişmesi gerekir. Yükseltilmiş bir komut istemine yazın:

  ```
  Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
  ```

#### <a name="dd41d5a2-8083-415b-9878-839652812102"></a> SIOS DataKeeper yükleyin

Kümedeki her düğümde SIOS DataKeeper Cluster Edition yükleyin. SIOS DataKeeper ile sanal paylaşılan depolama alanı oluşturmak için eşitlenen bir yansıtma oluşturmak ve Küme Paylaşılan depolama benzetimini yapma.

SIOS yazılım yüklemeden önce etki alanı kullanıcısı oluşturun **DataKeeperSvc**.

> [!NOTE]
> Ekleme **DataKeeperSvc** kullanıcıya **yerel yönetici** her iki küme düğümlerinde grup.
>
>

SIOS DataKeeper yüklemek için:

1.  SIOS yazılım her iki küme düğümlerine yükleyin.

  ![SIOS yükleyici][sap-ha-guide-figure-3030]

  ![Şekil 41: İlk sayfa SIOS DataKeeper yükleme][sap-ha-guide-figure-3031]

  _**Şekil 41:** SIOS DataKeeper yüklemesinin ilk sayfa_

2.  Şekil 42 gösterilen iletişim kutusunda seçin **Evet**.

  ![Şekil 42: Bir hizmeti devre dışı bırakılacak DataKeeper size bildirir][sap-ha-guide-figure-3032]

  _**Şekil 42:** DataKeeper bildirir, bir hizmeti devre dışı bırakılacak_

3.  Şekil 43 gösterilen iletişim kutusunda seçtiğiniz olan öneririz **etki alanı veya sunucu hesabı**.

  ![Şekil 43: Kullanıcı Seçimi SIOS DataKeeper için][sap-ha-guide-figure-3033]

  _**Şekil 43:** SIOS DataKeeper için kullanıcı seçimi_

4.  SIOS DataKeeper için oluşturduğunuz parola ve etki alanı hesabı kullanıcı adı girin.

  ![Şekil 44: SIOS DataKeeper yükleme için etki alanı kullanıcı adı ve parolayı girin][sap-ha-guide-figure-3034]

  _**Şekil 44:** SIOS DataKeeper yükleme için etki alanı kullanıcı adı ve parolayı girin_

5.  Lisans anahtarı SIOS DataKeeper Örneğiniz için şekil 45 gösterildiği gibi yükleyin.

  ![Şekil 45: SIOS DataKeeper lisans anahtarınızı girin][sap-ha-guide-figure-3035]

  _**Şekil 45:** SIOS DataKeeper lisans anahtarınızı girin_

6.  İstendiğinde, sanal makineyi yeniden başlatın.

#### <a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a> SIOS DataKeeper ayarlayın

Her iki düğümde SIOS DataKeeper yükledikten sonra yapılandırma başlatmanız gerekir. Yapılandırma amacı, her sanal makineye bağlı ek VHD'ler arasında zaman uyumlu veri çoğaltma sağlamaktır.

1.  DataKeeper yönetim ve Yapılandırma Aracı'nı başlatın ve ardından **Connect Server**. (Şekil 46 bu seçenek kırmızı daire içine alınmıştır.)

  ![Şekil 46: SIOS DataKeeper yönetim ve yapılandırma aracı][sap-ha-guide-figure-3036]

  _**Şekil 46:** SIOS DataKeeper yönetim ve yapılandırma aracı_

2.  Adını veya yönetim ve yapılandırma aracı için ve ikinci adım, ikinci düğümü bağlanmalısınız ilk düğümü TCP/IP'yi adresini girin.

  ![Şekil 47: ad ekleyin veya TCP/IP adresi ilk düğümün yönetim ve yapılandırma aracı için ve ikinci adım, ikinci düğümü bağlanmanız gerekir][sap-ha-guide-figure-3037]

  _**Şekil 47:** adını veya yönetimi ve yapılandırması aracı bağlanmak için ve ikinci adım, ikinci düğümü ilk düğümde TCP/IP'yi adresini Ekle_

3.  İki düğüm arasında çoğaltma işi oluşturun.

  ![Şekil 48: bir çoğaltma işi oluşturma][sap-ha-guide-figure-3038]

  _**Şekil 48:** çoğaltma işi oluşturma_

  Sihirbaz, bir çoğaltma işi oluşturma işleminde size rehberlik eder.
4.  Adı, TCP/IP adresi ve kaynak düğüm disk birimi tanımlayın.

  ![Şekil 49: çoğaltma işi adını tanımlayın][sap-ha-guide-figure-3039]

  _**Şekil 49:** çoğaltma işi adını tanımlayın_

  ![Şekil 50: geçerli kaynak düğüm olmalıdır düğümü için temel veri tanımlama][sap-ha-guide-figure-3040]

  _**Şekil 50:** geçerli kaynak düğüm olmalıdır düğümü için temel veri tanımlama_

5.  Adı, TCP/IP adresi ve hedef düğüm disk birimi tanımlayın.

  ![Şekil 51: geçerli hedef düğümü olmalı düğümü için temel veri tanımlama][sap-ha-guide-figure-3041]

  _**Şekil 51:** geçerli hedef düğüm olmalıdır düğümü için temel veri tanımlama_

6.  Sıkıştırma algoritmaları tanımlayın. Bizim örneğimizde, çoğaltma akışını sıkıştırmak öneririz. Özellikle yeniden eşitleme durumlarda çoğaltma akışı sıkıştırma yeniden eşitleme süresini önemli ölçüde azaltır. Sıkıştırma, bir sanal makinenin CPU ve RAM kaynakları kullandığına dikkat edin. Sıkıştırma oranı arttıkça, birimin kullanılan CPU kaynaklarının da artar. Bu ayarı daha sonra da ayarlayabilirsiniz.

7.  Kontrol etmeniz başka bir ayar olup çoğaltma zaman uyumsuz veya zaman uyumlu olarak gerçekleşir. *SAP ASCS/SCS yapılandırmaları koruduğunuzda, zaman uyumlu çoğaltma kullanmalısınız*.  

  ![Şekil 52: çoğaltma ayrıntılarını tanımlayın][sap-ha-guide-figure-3042]

  _**Şekil 52:** çoğaltma ayrıntılarını tanımlayın_

8.  Çoğaltma işlemiyle çoğaltılır birimi paylaşılan bir disk olarak Windows Server Yük Devretme Kümelemesi küme yapılandırması için temsil olup olmadığını tanımlar. SAP ASCS/SCS yapılandırmasını seçin **Evet** Windows Küme çoğaltılan birim küme birimi olarak kullanabileceği bir paylaşılan disk olarak görebilmesi için.

  ![Şekil 53: çoğaltılan birim küme birimi olarak ayarlamak için Evet'i seçin][sap-ha-guide-figure-3043]

  _**Şekil 53:** seçin **Evet** çoğaltılan birim küme birimi olarak ayarlamak için_

  Birim oluşturulduktan sonra DataKeeper yönetim ve yapılandırma aracı çoğaltma işi etkin olduğunu gösterir.

  ![Şekil 54: DataKeeper SAP ASCS/SCS paylaşım disk için zaman uyumlu yansıtma etkindir][sap-ha-guide-figure-3044]

  _**Şekil 54:** disk SAP ASCS/SCS paylaşmak için DataKeeper zaman uyumlu yansıtma etkin olduğu_

  Yük Devretme Kümesi Yöneticisi şimdi şekil 55 gösterildiği gibi diski bir DataKeeper disk olarak gösterir.

  ![Şekil 55: Yük devretme kümesi Yöneticisi'ni DataKeeper çoğaltılan disk gösterir.][sap-ha-guide-figure-3045]

  _**Şekil 55:** yük devretme kümesi Yöneticisi, çoğaltılan bu DataKeeper disk gösterir_

## <a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a> SAP NetWeaver sistemini yükleyin

Kurulumları DBMS sistemine bağlı olarak farklılık gösterdiğinden biz DBMS kurulumunu açıklayan olmaz. Ancak, farklı DBMS satıcılar için Azure desteği işlevler ile DBMS ile yüksek kullanılabilirlik sorunlarının giderilmesini varsayalım. Örneğin, her zaman açık veya Oracle veritabanları için SQL Server ve Oracle Data Guard için veritabanı yansıtma. Bu makalede kullanırız senaryosunda, size daha fazla koruma DBMS ekleyemiyor.

Bu tür bir Azure kümelenmiş SAP ASCS/SCS yapılandırmasında farklı DBMS Hizmetleri etkileşim, özel durumlar vardır.

> [!NOTE]
> SAP NetWeaver ABAP sistemleri, Java sistemleri ve ABAP + Java sistemleri yükleme yordamları neredeyse aynıdır. En önemli fark, bir SAP ABAP sistemi bir ASCS örneği sahip olur. SAP Java sistem bir SCS örneği vardır. SAP ABAP + Java sistem bir ASCS örneği ve aynı Microsoft yük devretme küme grubunda çalışan bir SCS örneğine sahip. Her SAP NetWeaver yükleme yığınının yükleme farkları açıkça belirtilen. Diğer tüm bölümleri aynı olduğunu kabul edilebilir.  
>
>

### <a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a> Bir yüksek kullanılabilirlik ASCS/SCS örneğiyle SAP yükleyin

> [!IMPORTANT]
> Sayfa dosyanızı DataKeeper yansıtılmış birimler üzerinde değil yerleştirdiğinizden emin olun. Yansıtılmış birimler DataKeeper desteklemez. Varsayılan seçenek geçici D sürücüsündeki bir Azure sanal makine, sayfa dosyası bırakabilirsiniz. Bunu zaten yoksa, Windows disk belleği dosyası Azure sanal makinenizin D sürücüye taşıyın.
>
>

Bir yüksek kullanılabilirlik ASCS/SCS örneğiyle SAP yüklemek, bu görevleri içerir:

- Kümelenmiş SAP ASCS/SCS örneği için bir sanal ana bilgisayar adı oluşturma
- SAP ilk küme düğümüne yükleme
- SAP profili ASCS/SCS örneğinin değiştirme
- Sonda bağlantı noktası ekleme
- Windows Güvenlik Duvarı araştırma bağlantı noktası açma

#### <a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a> Kümelenmiş SAP ASCS/SCS örneği için bir sanal ana bilgisayar adı oluşturma

1.  Windows DNS Yöneticisi'nde ASCS/SCS örneğinin sanal ana bilgisayar adı için bir DNS girişi oluşturun.

  > [!IMPORTANT]
  > ASCS/SCS örneğinin sanal ana bilgisayar adı atamak IP adresi Azure yük dengeleyiciye atanan IP adresi ile aynı olmalıdır (**<*SID*> - lb - ascs**).  
  >
  >

  IP adresi sanal SAP ASCS/SCS ana bilgisayar adının (**pr1 ascs sap**) Azure yük dengeleyici IP adresi ile aynıdır (**pr1 lb ascs**).

  ![Şekil 56: SAP ASCS/SCS küme sanal adı ve TCP/IP adresi için DNS girişini tanımlayın][sap-ha-guide-figure-3046]

  _**Şekil 56:** SAP ASCS/SCS küme sanal adı ve TCP/IP adresi için DNS girişini tanımlayın_

2.  Sanal ana bilgisayar adı atanmış bir IP adresi tanımlamak için seçin **DNS Yöneticisi'ni** > **etki alanı**.

  ![Şekil 57: Yeni sanal adı ve TCP/IP adresi SAP ASCS/SCS kümesi yapılandırması için][sap-ha-guide-figure-3047]

  _**Şekil 57:** yeni sanal adı ve TCP/IP adresi SAP ASCS/SCS küme yapılandırması_

#### <a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a> SAP ilk küme düğümüne yükleyin

1.  İlk küme düğümü seçeneği küme düğümünde A. yürütme Örneğin, **pr1 ascs 0** ana bilgisayar.
2.  Azure iç yük dengeleyicisi için bağlantı noktalarını varsayılan tutmak için seçin:

  * **ABAP sistem**: **ASCS** örnek numarasını **00**
  * **Java sistem**: **SCS** örnek numarasını **01**
  * **ABAP + Java sistem**: **ASCS** örnek numarasını **00** ve **SCS** örnek numarasını **01**

  Örnek sayıları ABAP ASCS örneği ve Java SCS örneğinin 01 00 dışında kullanmak için öncelikle Azure iç yük dengeleyici varsayılan Yük Dengeleme kuralları, açıklanan değiştirmek gerekir [ASCS/SCS varsayılan Yük Dengeleme kuralları Azure iç yük dengeleyici için değiştirme][sap-ha-guide-8.9].

Sonraki birkaç görevleri standart SAP yükleme belgelerinde açıklanan değil.

> [!NOTE]
> SAP yükleme belgelerini ilk ASCS/SCS küme düğümüne yüklemeyi açıklar.
>
>

#### <a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a> SAP profili ASCS/SCS örneğinin değiştirme

Yeni bir profil parametre eklemeniz gerekir. Profil parametre çok uzun süre boşta olduğunda kapanmasını SAP iş süreçlerini ve kuyruğa sunucu arasındaki bağlantıları engeller. Biz sorun senaryoda bahsedilen [SAP ASCS/SCS örneği her iki küme düğümleri üzerinde kayıt defteri girdisini eklemeniz][sap-ha-guide-8.11]. Bu bölümde ayrıca bazı temel TCP/IP bağlantı parametrelerini iki değişiklik sunulmuştur. İkinci adımda, sıraya alma sunucusunu gönderecek şekilde ayarlamanız gerekir. bir `keep_alive` bağlantılar Azure iç yük dengeleyicinin boşta kalma eşiği isabet yok böylece sinyal.

ASCS/SCS örneğinin SAP profilini değiştirmek için:

1.  Bu profil parametre SAP ASCS/SCS örneği profiline ekleyin:

  ```
  enque/encni/set_so_keepalive = true
  ```
  Bizim örneğimizde, yol şudur:

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_ASCS00_pr1-ascs-sap`

  Örneğin, SAP SCS profili ve karşılık gelen yol örneği:

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_SCS01_pr1-ascs-sap`

2.  Değişiklikleri uygulamak için SAP ASCS /SCS örneğini yeniden başlatın.

#### <a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a> Bir araştırma bağlantı noktası ekleme

Azure yük dengeleyici ile çalışma tüm küme yapılandırmasını yapmak için iç yük dengeleyicinin araştırma işlevini kullanın. Azure iç yük dengeleyicisi genellikle gelen iş yükü katılımcı sanal makineler arasında eşit olarak dağıtır. Ancak, tek örnek etkin olmadığından bu bazı küme yapılandırmaları çalışmaz. Diğer örnek pasif ve herhangi bir iş yükü kabul edemez. Azure iç yük dengeleyici yalnızca etkin bir örneği için iş atarken araştırma işlevler yardımcı olur. Araştırma işlevsellikle iç yük dengeleyicisi hangi örnekleri etkin olduğunu ve yalnızca iş yükü örneğiyle hedef algılayabilir.

Sonda bağlantı noktası eklemek için:

1.  Geçerli denetleyin **ProbePort** aşağıdaki PowerShell komutunu çalıştırarak ayarlama. Sanal makineler biri içinde ondan küme yapılandırmasında yürütün.

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter
  ```

2.  Bir araştırma bağlantı noktasını tanımlayın. Varsayılan araştırma bağlantı noktası numarası **0**. Bizim örneğimizde, araştırma bağlantı noktası kullanırız **62000**.

  ![Şekil 58: Küme yapılandırmasını araştırma bağlantı noktası varsayılan olarak 0 olur.][sap-ha-guide-figure-3048]

  _**Şekil 58:** varsayılan küme yapılandırması araştırma bağlantı noktası 0'dır_

  Bağlantı noktası numarasını SAP Azure Resource Manager şablonları tanımlanır. PowerShell'de bağlantı noktası numarası atayabilirsiniz.

  Yeni bir ProbePort değerini ayarlamak için **SAP <*SID*> IP** küme kaynağı, şu PowerShell betiğini çalıştırın. Ortamınız için PowerShell değişkenleri güncelleştirin. Betik çalıştıktan sonra değişiklikleri etkinleştirmek için SAP küme grubu yeniden başlatmanız istenir.

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

  Bunu yaptıktan sonra **SAP <*SID* >**  küme rolünü, doğrulayın **ProbePort** yeni değere ayarlanır.

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter

  ```

  ![Şekil 59: yeni değer ayarlandıktan sonra küme bağlantı noktası araştırma][sap-ha-guide-figure-3049]

  _**Şekil 59:** yeni değer ayarlandıktan sonra küme bağlantı noktası araştırma_

#### <a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a> Windows Güvenlik Duvarı araştırma bağlantı noktasını açın

Her iki küme düğümlerinde Windows Güvenlik Duvarı araştırma bağlantı açmanız gerekir. Bir Windows Güvenlik Duvarı araştırma bağlantı noktasını açmak için aşağıdaki komut dosyasını kullanın. Ortamınız için PowerShell değişkenleri güncelleştirin.

  ```PowerShell
  $ProbePort = 62000   # ProbePort of the Azure Internal Load Balancer

  New-NetFirewallRule -Name AzureProbePort -DisplayName "Rule for Azure Probe Port" -Direction Inbound -Action Allow -Protocol TCP -LocalPort $ProbePort
  ```

**ProbePort** ayarlanır **62000**. Dosya paylaşımına erişebilirsiniz artık  **\\\ascsha-clsap\sapmnt** diğer konakları, gibi kadar **ascsha dbas**.

### <a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a> Veritabanı örneğini yükleyin

Veritabanı örneği yüklemek için SAP yükleme belgelerinde açıklanan işlemi izleyin.

### <a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a> İkinci küme düğümü yükleme

İkinci küme yüklemek için SAP Yükleme Kılavuzu'ndaki adımları izleyin.

### <a name="094bc895-31d4-4471-91cc-1513b64e406a"></a> SAP ERS Windows hizmet örneği başlangıç türünü değiştirme

SAP ERS Windows hizmeti başlangıç türünü değiştirme **otomatik (Gecikmeli Başlatma)** her iki küme düğümlerinde.

![Şekil 60: SAP ERS örneği için hizmet türü Gecikmeli otomatik olarak değiştirin.][sap-ha-guide-figure-3050]

_**Şekil 60:** SAP ERS örneğe otomatik Gecikmeli hizmet türünü değiştir_

### <a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a> SAP birincil uygulama sunucusu yükleme

Birincil uygulama sunucusu (PAS) örneğini yükleyin <*SID*> - dı - 0 PAS barındırmak için belirlediğiniz sanal makinede. Azure veya DataKeeper özgü ayarları bir bağımlılık yoktur.

### <a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a> SAP ek uygulama sunucusu yükleme

Bir SAP ek uygulama sunucusu (AAS) tüm sanal SAP uygulama sunucusu örneği barındırmak için belirlediğiniz makinelere yükleyin. Örneğin, <*SID*> - dı - 1 için <*SID*> - di -&lt;n&gt;.

> [!NOTE]
> Bu, bir yüksek kullanılabilirlik SAP NetWeaver sistemi yüklemesini tamamlar. Ardından, yük devretme testi ile devam edin.
>


## <a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a> SIOS çoğaltma ve SAP ASCS/SCS örneği yük devretme testi
Yük Devretme Kümesi Yöneticisi'ni ve SIOS DataKeeper yönetimi ve yapılandırması aracını kullanarak bir SAP ASCS/SCS örneği yük devretme ve SIOS disk çoğaltması izlemek ve test kolaydır.

### <a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a> SAP ASCS/SCS örneği bir küme düğümünde çalışıyor

**SAP PR1** küme grubu A'daki küme düğümünde çalışıyor Örneğin, **pr1 ascs 0**. Paylaşılan disk bölümü olan sürücü S, ata, **SAP PR1** küme grubu ve a düğümünü kümeye ASCS/SCS örneği kullanan

![Şekil 61: Yük devretme kümesi Yöneticisi: SAP < SID > Küme grubu, bir küme düğümünde çalışıyor][sap-ha-guide-figure-5000]

_**Şekil 61:** yük devretme kümesi Yöneticisi: SAP <*SID*> Küme grubu, bir küme düğümünde çalışıyor_

SIOS DataKeeper yönetim ve Yapılandırma Aracı'nda paylaşılan disk verilerini zaman uyumlu olarak küme düğümü bir kaynak birim sürücüsünden S küme düğümünde B. hedef birim sürücü S için çoğaltıldığından emin görebilirsiniz Örneğin, gelen çoğaltılır **pr1 ascs 0 [10.0.0.40]** için **pr1 ascs 1 [10.0.0.41]**.

![Şekil 62: SIOS DataKeeper içinde yerel birim küme düğümünden bir küme düğümüne B çoğaltma][sap-ha-guide-figure-5001]

_**Şekil 62:** SIOS DataKeeper içinde yerel birim küme düğümünden bir küme düğümüne B çoğaltma_

### <a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a> Bir düğümden yük devretme düğümüne B

1.  Bir yük devretme SAP başlatmak için bu seçeneklerden birini <*SID*> küme grubuna bir küme düğümünden küme düğümü B:
  - Yük Devretme Kümesi Yöneticisi'ni kullanın  
  - Yük devretme kümesi PowerShell kullanma

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPClusterGroup = "SAP $SAPSID"
  Move-ClusterGroup -Name $SAPClusterGroup

  ```
2.  Küme düğümü bir Windows konuk işletim sistemi içinde yeniden (Bu SAP otomatik bir yük devretme başlatır <*SID*> düğümünden bir küme grubu B düğümü için).  
3.  Küme düğümü bir Azure portalından yeniden (Bu SAP otomatik bir yük devretme başlatır <*SID*> düğümünden bir küme grubu B düğümü için).  
4.  Azure PowerShell kullanarak küme düğümü bir yeniden başlatma (Bu SAP otomatik olarak yük devretmesi başlatır <*SID*> düğümünden bir küme grubu B düğümü için).

  SAP yük devretme sonrasında <*SID*> Küme grubu b küme düğümünde çalışıyor Örneğin, üzerinde çalıştırıldığı **pr1 ascs 1**.

  ![Şekil 63: Yük devretme kümesi Yöneticisi'nde SAP < SID > Küme grubu B küme düğümünde çalışıyor][sap-ha-guide-figure-5002]

  _**Şekil 63**: yük devretme kümesi Yöneticisi'nde, SAP <*SID*> Küme grubu B küme düğümünde çalışıyor_

  Paylaşılan disk artık takılı kümede düğüm B. SIOS DataKeeper veri sürücüsünden kaynak birim S B küme düğümünde Küme düğümünde A. hedef birim sürücü S için çoğaltma Örneğin, gelen çoğaltma **pr1 ascs 1 [10.0.0.41]** için **pr1 ascs 0 [10.0.0.40]**.

  ![Şekil 64: Yerel birim SIOS DataKeeper, küme düğümü bir küme düğümünden B çoğaltır.][sap-ha-guide-figure-5003]

  _**Şekil 64:** SIOS DataKeeper çoğaltır yerel birim düğüm bir küme için küme düğümünden B_
