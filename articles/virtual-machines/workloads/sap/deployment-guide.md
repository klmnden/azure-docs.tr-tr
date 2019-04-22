---
title: Azure sanal makineler dağıtım için SAP NetWeaver | Microsoft Docs
description: Azure'da Linux sanal makinelerinde SAP yazılım dağıtmayı öğrenin.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: MSSedusch
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 1c4f1951-3613-4a5a-a0af-36b85750c84e
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/26/2018
ms.author: sedusch
ms.openlocfilehash: c93bca14d9385eaf9f79f69d76e9e704796da7a9
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58850879"
---
# <a name="azure-virtual-machines-deployment-for-sap-netweaver"></a>Azure sanal makineler dağıtım için SAP NetWeaver

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
[2367194]:https://launchpad.support.sap.com/#/notes/2367194

[azure-cli]:../../../cli-install-nodejs.md
[azure-cli-2]:https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest
[azure-portal]:https://portal.azure.com
[azure-ps]:/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md#subscription-limits

[dbms-guide]:dbms-guide.md (SAP için Azure sanal makineleri DBMS dağıtım)
[dbms-guide-2.1]:dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (VM'ler ve VHD'ler için önbelleğe alma)
[dbms-guide-2.2]:dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (Yazılım RAID)
[dbms-guide-2.3]:dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (Microsoft Azure depolama)
[dbms-guide-2]:dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (Bir RDBMS dağıtım yapısı)
[dbms-guide-3]:dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (Azure sanal makineleri ile yüksek kullanılabilirlik ve olağanüstü durum kurtarma)
[dbms-guide-5.5.1]:dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 (SQL Server 2012 SP1 CU4 ve üzeri)
[dbms-guide-5.5.2]:dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3'ten ve önceki sürümleri)
[dbms-guide-5.6]:dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (Azure Market'ten bir SQL Server görüntüsü kullanma)
[dbms-guide-5.8]:dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (Genel SQL Server SAP on Azure özeti)
[dbms-guide-5]:dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (SQL Server RDBMS özellikleri)
[dbms-guide-8.4.1]:dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (Depolama yapılandırması)
[dbms-guide-8.4.2]:dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d (Yedekleme ve geri yükleme)
[dbms-guide-8.4.3]:dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (Yedekleme ve geri yükleme için performans konuları)
[dbms-guide-8.4.4]:dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (Diğer)
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

[deployment-guide]:deployment-guide.md (SAP için Azure sanal makineler dağıtımı)
[deployment-guide-2.2]:deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP kaynakları)
[deployment-guide-3.1.2]:deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (Özel bir görüntü kullanarak bir VM dağıtma)
[deployment-guide-3.2]:deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (Senaryo 1: SAP için Azure Market'ten bir VM dağıtma)
[deployment-guide-3.3]:deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (Senaryo 2: SAP için özel bir görüntü ile bir VM dağıtma)
[deployment-guide-3.4]:deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 (Senaryo 3: Azure olmayan genelleştirilmiş VHD ile SAP kullanarak şirket içi VM taşıma)
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (Dağıtım senaryoları için Microsoft Azure üzerinde SAP VM)
[deployment-guide-4.1]:deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (Azure PowerShell cmdlet'lerini dağıtma)
[deployment-guide-4.2]:deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (İndirme ve içeri aktarma SAP ilgili PowerShell cmdlet'leri)
[deployment-guide-4.3]:deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (Bir şirket içi etki - yalnızca Windows VM katılın)
[deployment-guide-4.4.2]:deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux)
[deployment-guide-4.4]:deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d (İndirme, yükleme ve Azure VM aracısını etkinleştirin)
[deployment-guide-4.5.1]:deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure PowerShell)
[deployment-guide-4.5.2]:deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure CLI)
[deployment-guide-4.5]:deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (SAP için Azure Gelişmiş izleme uzantısı yapılandırma)
[deployment-guide-5.1]:deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (Hazır olma denetimi için Azure SAP Gelişmiş izleme)
[deployment-guide-5.2]:deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (Azure izleme altyapısı durumunu denetleme)
[deployment-guide-5.3]:deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (Azure için SAP izleme sorunlarını giderme)

[deployment-guide-configure-monitoring-scenario-1]:deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b (İzlemeyi Yapılandırma)
[deployment-guide-configure-proxy]:deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d (Proxy yapılandırma)
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
[deployment-guide-troubleshooting-chapter]:deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b (Denetimler ve sorun giderme için uçtan uca izlemeyi ayarlama)

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

[msdn-set-Azvmaemextension]:https://docs.microsoft.com/powershell/module/az.compute/set-azvmaemextension

[planning-guide]:planning-guide.md (Azure sanal makineleri planlama ve uygulama için SAP)
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (Kaynakları)
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (Azure sanal Makineler'de çalışan SAP NetWeaver için yüksek kullanılabilirlik ve olağanüstü durum kurtarma)
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (SAP uygulama sunucuları için yüksek kullanılabilirlik)
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (SAP örnekleri için AutoStart'ı kullanma)
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803 (Yalnızca bulut - şirket içi müşteri ağı bağımlılığı olmaksızın azure'da sanal makine dağıtımları)
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (Şirket içi - dağıtımı tek veya birden çok SAP sanal makineleri azure'da şirket içi ağ ile tamamen tümleşiktir.)
[planning-guide-3.1]:planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure bölgeleri)
[planning-guide-3.2.1]:planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (Hata etki alanları)
[planning-guide-3.2.2]:planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (Yükseltme etki alanları)
[planning-guide-3.2.3]:planning-guide.md#18810088-f9be-4c97-958a-27996255c665 (Azure kullanılabilirlik kümeleri)
[planning-guide-3.2]:planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (Microsoft Azure sanal makineleri kavramı)
[planning-guide-3.3.2]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium depolama)
[planning-guide-5.1.1]:planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (Bir sanal makine şirket içinden Azure'a genelleştirilmiş olmayan bir diskle taşıma)
[planning-guide-5.1.2]:planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (Müşteri belirli bir görüntüye sahip bir VM dağıtma)
[planning-guide-5.2.1]:planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (Bir VM şirket içinden Azure'a genelleştirilmiş olmayan bir disk ile geçiş için hazırlama)
[planning-guide-5.2.2]:planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (SAP için müşteri belirli bir görüntüye sahip bir VM dağıtımı için hazırlama)
[planning-guide-5.2]:planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (Azure için SAP ile Vm'leri hazırlama)
[planning-guide-5.3.1]:planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (Azure görüntü ile bir Azure diskinin arasındaki fark)
[planning-guide-5.3.2]:planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (Şirket içinden azure'a VHD yükleme)
[planning-guide-5.4.2]:planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (Azure depolama hesapları arasında diskleri kopyalama)
[planning-guide-5.5.1]:planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646 (SAP dağıtımları için VM/VHD yapısı)
[planning-guide-5.5.3]:planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (Otomatik bağlama bağlı diskleri için ayarlama)
[planning-guide-7.1]:planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (Tek tanıtım/senaryo eğitim NetWeaver SAP VM)
[planning-guide-7]:planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (SAP örnekleri yalnızca bulutta yer alan dağıtım kavramları)
[planning-guide-9.1]:planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure için SAP izleme çözümü)
[planning-guide-azure-premium-storage]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium depolama)
[planning-guide-managed-disks]:planning-guide.md#c55b2c6e-3ca1-4476-be16-16c81927550f (Yönetilen diskler)
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
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure ağı)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Depolama: Microsoft Azure depolama ve veri diskleri)

[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../../networking/network-overview.md
[sap-pam]:https://support.sap.com/pam (SAP ürün kullanılabilirliği Matrisi)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image-md%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-os-disk-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk-md%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-2-tier-user-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image-md%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-md%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image-md%2Fazuredeploy.json
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
[virtual-machines-Az-versus-azuresm]:virtual-machines-linux-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md (Azure Resource Manager şablonları ve Azure CLI kullanarak sanal makineleri yönetme ve dağıtma)
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md (Azure Resource Manager ve PowerShell kullanarak sanal makineleri yönetme)
[virtual-machines-windows-agent-user-guide]:../../extensions/agent-windows.md
[virtual-machines-linux-agent-user-guide]:../../extensions/agent-linux.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../extensions/agent-linux.md#command-line-options
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
[virtual-machines-manage-availability]:../../linux/manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:../../virtual-machines-windows-ps-create.md
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

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Azure sanal makineler, en az sürede ve uzun tedarik döngüleri olmadan işlem ve depolama kaynaklarını gereken kuruluşlar çözümüdür. Azure sanal makineler, azure'da SAP NetWeaver tabanlı uygulamalar gibi Klasik uygulamaları dağıtmak için kullanabilirsiniz. Bir uygulamanın güvenilirlik ve kullanılabilirlik ek şirket içi kaynaklar olmadan genişletin. Böylece Azure sanal makineleri kuruluşunuzun şirket içi etki alanları, özel Bulutlar ve SAP sistem ortamı tümleştirebilirsiniz şirketler arası bağlantı, Azure sanal makineleri destekler.

Bu makalede, azure'da, diğer dağıtım seçenekleri dahil olmak üzere ve sorun giderme sanal makineleri (VM'ler) SAP uygulamaları dağıtma adımları ele. Bu makalede yer alan bilgiler geliştirir [planlama Azure sanal makineleri ve SAP NetWeaver uygulamasını][planning-guide]. Ayrıca SAP yükleme belgelerine ve yükleme ve SAP yazılım dağıtmak için birincil kaynaklardır SAP notları tamamlar.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

Bir Azure sanal makinesi için SAP yazılım dağıtımı ayarlama adımları ve kaynakları birden çok içerir. Başlamadan önce Azure sanal makinelerinde SAP yazılım yüklemeye yönelik önkoşulları karşıladığından emin olun.

### <a name="local-computer"></a>Yerel bilgisayar

Windows veya Linux sanal makineleri yönetmek için bir PowerShell Betiği ve Azure portalını kullanabilirsiniz. Her iki araç için Windows 7 veya sonraki bir Windows sürümü çalıştıran yerel bir bilgisayar gerekir. Yalnızca Linux Vm'leri yönetmek istediğiniz ve bu görev için bir Linux bilgisayar kullanmak istiyorsanız, Azure CLI'yı kullanabilirsiniz.

### <a name="internet-connection"></a>Internet bağlantısı

İndirmek ve SAP yazılım dağıtımı için gerekli olan betikleri ve araçları çalıştırmak için Internet'e bağlı gerekir. Azure Gelişmiş izleme uzantısı için SAP çalıştıran Azure VM'nin de Internet erişimi gerekir. Azure VM'de bir Azure sanal ağı veya şirket içi etki alanının parçasıysa, açıklandığı gibi ilgili proxy ayarlarının belirlendiğini emin [Proxy'yi yapılandırmak][deployment-guide-configure-proxy].

### <a name="microsoft-azure-subscription"></a>Microsoft Azure aboneliği

Etkin bir Azure hesabınız olmalıdır.

### <a name="topology-and-networking"></a>Topoloji ve ağ

Azure'da SAP dağıtımının mimarisini ve topolojisi tanımlayın yapmanız gerekir:

* Kullanılacak azure depolama hesapları
* SAP sistemini dağıtmak için istediğiniz sanal ağ
* SAP sistemine dağıtmak istediğiniz kaynak grubu
* SAP sistemini dağıtmak için istediğiniz azure bölgesi
* SAP yapılandırma (iki katmanlı veya üç katmanlı)
* VM boyutları ve Vm'lere bağlanması ek veri diski sayısı
* SAP düzeltme ve taşıma sistemi (CTS) yapılandırma

Oluşturun ve SAP yazılım dağıtım işlemi başlamadan önce Azure depolama hesapları (gerekliyse) veya Azure sanal ağları yapılandırın. Oluşturma ve bu kaynakları yapılandırma hakkında daha fazla bilgi için bkz. [planlama Azure sanal makineleri ve SAP NetWeaver uygulamasını][planning-guide].

### <a name="sap-sizing"></a>SAP boyutlandırma

SAP boyutlandırma için aşağıdaki bilgileri bildirin:

* SAP uygulama performans standart (SAP) yanı sıra SAP hızlı Boyutlandırıcı aracı'nı kullanarak öngörülen SAP iş yükü, örneğin, numara
* SAP sisteminin gerekli CPU kaynağı ve bellek tüketimi
* Saniye başına giriş/çıkış (g/ç) işlemleri gerekli
* Son iletişim azure'da sanal makineler arasındaki ağ bant genişliği gerekli
* Şirket içi varlıklara ve Azure ile dağıtılmış SAP sistemini arasında gerekli ağ bant genişliği

### <a name="resource-groups"></a>Kaynak grupları

Azure Resource Manager'da Azure aboneliğinizdeki tüm uygulama kaynaklarını yönetmek için kaynak gruplarını kullanabilirsiniz. Daha fazla bilgi için [Azure Resource Manager'a genel bakış][resource-group-overview].

## <a name="resources"></a>Kaynaklar

### <a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>SAP kaynakları

SAP yazılımı dağıtımınızı ayarlarken, aşağıdaki SAP kaynakları gerekir:

* SAP notu [1928533], sahip olduğu:
  * SAP yazılım dağıtımı için desteklenen bir Azure VM boyutlarının listesini
  * Azure VM boyutları için önemli kapasite bilgileri
  * Desteklenen bir SAP yazılım ve işletim sistemi (OS) ve veritabanı birleşimleri
  * Windows ve Linux'ta Microsoft Azure için gerekli SAP çekirdek sürümü

* SAP notu [2015553] azure'da SAP tarafından desteklenen SAP yazılım dağıtımları için önkoşulları listeler.
* SAP notu [2178632] ayrıntılı azure'da SAP için bildirilen tüm izlenen ölçümler hakkında bilgi içerir.
* SAP notu [1409604] Windows Azure için gerekli SAP konak Aracısı sürümü vardır.
* SAP notu [2191498] azure'da Linux için gerekli SAP konak Aracısı sürümü vardır.
* SAP notu [2243692] Linux Azure üzerinde SAP lisanslama hakkında bilgi içeriyor.
* SAP notu [1984787] SUSE Linux Enterprise Server 12 ilgili genel bilgiler bulunur.
* SAP notu [2002167] Red Hat Enterprise Linux hakkında genel bilgi 7.x.
* SAP notu [2069760] Oracle Linux hakkında genel bilgi 7.x.
* SAP notu [1999351] Azure Gelişmiş izleme uzantısı için SAP için ek bilgiler.
* SAP notu [1597355] Linux için takas alanı ilgili genel bilgiler bulunur.
* [Azure SCN sayfasında SAP](https://wiki.scn.sap.com/wiki/x/Pia7Gg) haberleri ve faydalı kaynaklar koleksiyonu vardır.
* [SAP topluluk WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) tüm SAP notları Linux için zorunludur.
* Parçası olan SAP özgü PowerShell cmdlet'leri [Azure PowerShell][azure-ps].
* Parçası olan SAP özgü Azure CLI komutları [Azure CLI][azure-cli].

### <a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>Windows kaynakları

Bu Microsoft makaleler, azure'da SAP dağıtımları kapsar:

* [Azure sanal makineleri planlama ve uygulama için SAP NetWeaver][planning-guide]
* [Azure sanal makineler dağıtım için SAP NetWeaver (Bu makale)][deployment-guide]
* [Azure sanal makineleri DBMS dağıtım için SAP NetWeaver][dbms-guide]

## <a name="b3253ee3-d63b-4d74-a49b-185e76c4088e"></a>Azure vm'lerde SAP yazılım için dağıtım senaryoları

Vm'leri ve ilişkili diskler azure'da dağıtmak için birçok seçeneğiniz vardır. Sanal makinelerinizin seçtiğiniz dağıtım türüne göre dağıtımına hazırlamak için farklı adımlar sürebilir çünkü dağıtım seçenekleri arasındaki farkları anlamak önemlidir.

### <a name="db477013-9060-4602-9ad4-b0316f8bb281"></a>Senaryo 1: SAP için Azure Market'ten bir VM dağıtma

Sanal makinenizin dağıtılacağı bir üçüncü taraf Azure Marketi'nde veya Microsoft tarafından sağlanan bir görüntüyü kullanabilirsiniz. Market bazı standart bir işletim sistemi görüntüleri Windows Server ve Linux dağıtımları sunar. Veritabanı Yönetimi içeren bir görüntüyü de dağıtabilirsiniz, örneğin, Microsoft SQL Server sistemi (DBMS) SKU'ları. Görüntüleri DBMS SKU'ları ile kullanma hakkında daha fazla bilgi için bkz. [SAP NetWeaver için Azure sanal makineleri DBMS dağıtım][dbms-guide].

Aşağıdaki akış çizelgesi, Azure Market'ten bir VM dağıtmak için adımları SAP özgü sırasını göstermektedir:

![VM dağıtımı için Azure Market'ten bir VM görüntüsü kullanarak SAP sistemlerini akış çizelgesi][deployment-guide-figure-100]

#### <a name="create-a-virtual-machine-by-using-the-azure-portal"></a>Azure portalını kullanarak bir sanal makine oluşturun

Azure Market'te bir görüntüden yeni bir sanal makine oluşturmak için en kolay yolu, Azure portalı kullanmaktır.

1.  <https://portal.azure.com/#create/hub> kısmına gidin.  Veya, Azure portal menüsünde seçin **+ yeni**.
1.  Seçin **işlem**ve ardından dağıtmak istediğiniz işletim sistemi türünü seçin. Örneğin, Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12), Red Hat Enterprise Linux 7.2 (RHEL 7.2) veya Oracle Linux 7.2. Varsayılan liste görünümü, tüm desteklenen işletim sistemlerini göstermez. Seçin **tümünü gör** tam listesi için. SAP yazılım dağıtımı için desteklenen işletim sistemleri hakkında daha fazla bilgi için bkz. Not SAP [1928533].
1.  Sonraki sayfada, hüküm ve koşulları gözden geçirin.
1.  İçinde **dağıtım modeli seçin** kutusunda, seçin **Resource Manager**.
1.  **Oluştur**’u seçin.

Sihirbaz, ek olarak sanal makine ağ arabirimleri ve depolama hesapları gibi tüm gerekli kaynakları oluşturmak için gerekli parametreleri ayarlamanız konusunda size yol gösterir. Bu parametrelerden bazıları şunlardır:

1. **Temel**:
   * **Ad**: (Sanal makine adı) kaynağının adı.
   * **VM disk türü**: İşletim sistemi diskinin disk türünü seçin. Premium depolama, veri diskleri için kullanmak istiyorsanız, işletim sistemi diski de Premium depolama kullanmanızı öneririz.
   * **Kullanıcı adı ve parola** veya **SSH ortak anahtarı**: Kullanıcı adı ve sağlama sırasında oluşturulan kullanıcı parolasını girin. Bir Linux sanal makinesi için makineye oturum açmak için kullandığınız genel güvenli Kabuk (SSH) anahtarı girebilirsiniz.
   * **Abonelik**: Yeni sanal makine sağlamak için kullanmak istediğiniz aboneliği seçin.
   * **Kaynak grubu**: Sanal makine için kaynak grubunun adı. Yeni bir kaynak grubu adı veya zaten bir kaynak grubu adını girebilirsiniz.
   * **Konum**: Yeni bir sanal makine dağıtılacağı yeri'ı tıklatın. Sanal makine, şirket içi ağınıza bağlanmak istiyorsanız, Azure şirket içi ağınıza bağlanan sanal ağ konumu seçtiğinizden emin olun. Daha fazla bilgi için [Microsoft Azure ağı] [ planning-guide-microsoft-azure-networking] içinde [planlama Azure sanal makineleri ve SAP NetWeaver uygulamasını] [ planning-guide].
1. **Boyutu**:

     Desteklenen VM türlerinin bir listesi için bkz. Not SAP [1928533]. Azure Premium depolama kullanmak istiyorsanız doğru VM türünün seçtiğinizden emin olun. Tüm VM türleri Premium depolamayı destekler. Daha fazla bilgi için [Depolama: Microsoft Azure depolama ve veri diskleri] [ planning-guide-storage-microsoft-azure-storage-and-data-disks] ve [Azure Premium depolama] [ planning-guide-azure-premium-storage] içinde [Azure sanal makineler planlama ve uygulama SAP NetWeaver için][planning-guide].

1. **Ayarları**:
   * **Depolama**
     * **Disk türü**: İşletim sistemi diskinin disk türünü seçin. Premium depolama, veri diskleri için kullanmak istiyorsanız, işletim sistemi diski de Premium depolama kullanmanızı öneririz.
     * **Yönetilen diskleri kullanma**: Yönetilen diskleri kullanmak istiyorsanız, Evet'i seçin. Yönetilen diskler hakkında daha fazla bilgi için bkz: bölüm [yönetilen diskler] [ planning-guide-managed-disks] Planlama Kılavuzu'nda.
     * **Depolama hesabı**: Mevcut bir depolama hesabını seçin veya yeni bir tane oluşturun. SAP uygulamaları çalıştırmak için tüm depolama türlerinde çalışır. Depolama türleri hakkında daha fazla bilgi için bkz. [RDBMS dağıtımlar için bir VM depolama yapısını](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general#65fa79d6-a85f-47ee-890b-22e794f51a64).
   * **Ağ**
     * **Sanal ağ** ve **alt**: Sanal makine intranetinize ile tümleştirmek için şirket içi ağınıza bağlı sanal ağ'ı seçin.
     * **Genel IP adresi**: Kullanmak istediğiniz genel IP adresi seçin veya yeni bir ortak IP adresi oluşturmak için parametreler girin. Sanal makinenizi Internet üzerinden erişmek için genel bir IP adresi kullanabilirsiniz. Ayrıca, sanal makinenize güvenli erişim yardımcı olmak için ağ güvenlik grubu oluşturduğunuzdan emin olun.
     * **Ağ güvenlik grubu**: Daha fazla bilgi için [denetleyen ağ güvenlik grupları ile ağ trafiği akışını][virtual-networks-nsg].
   * **Uzantıları**: Dağıtımı ekleyerek sanal makine uzantıları yükleyebilirsiniz. Bu adımda uzantıları eklemek gerekmez. SAP destek için gereken uzantılarını daha sonra yüklenir. Bölüm bakın [Azure Gelişmiş izleme uzantısını SAP için yapılandırmak] [ deployment-guide-4.5] bu kılavuzdaki.
   * **Yüksek kullanılabilirlik**: Bir kullanılabilirlik kümesi seçin veya yeni bir kullanılabilirlik kümesi oluşturmak için parametreleri girin. Daha fazla bilgi için [Azure kullanılabilirlik kümeleri][planning-guide-3.2.3].
   * **İzleme**
     * **Önyükleme tanılaması**: Seçebileceğiniz **devre dışı** önyükleme tanılaması için.
     * **Konuk işletim sistemi tanılama**: Seçebileceğiniz **devre dışı** Tanılama izleme.

1. **Özet**:

   Seçimlerinizi gözden geçirin ve ardından **Tamam**.

Sanal makinenizi, seçili kaynak grubunda dağıtılmış.

#### <a name="create-a-virtual-machine-by-using-a-template"></a>Bir şablonu kullanarak bir sanal makine oluşturma

Yayımlanan SAP şablonlardan birini kullanarak bir sanal makine oluşturabilirsiniz [azure hızlı başlangıç şablonları GitHub deposunda][azure-quickstart-templates-github]. Ayrıca el ile bir sanal makine kullanarak oluşturabileceğiniz [Azure portalında][virtual-machines-windows-tutorial], [PowerShell][virtual-machines-ps-create-preconfigure-windows-resource-manager-vms], veya [AzureCLI] [virtual-machines-linux-tutorial].

* [**İki katmanlı yapılandırma (yalnızca bir sanal makine) şablonu** (sap-2-katman-Pazar-görüntü)][sap-templates-2-tier-marketplace-image]

  Yalnızca bir sanal makineyi kullanarak iki katmanlı bir sistem oluşturmak için bu şablonu kullanın.
* [**İki katmanlı yapılandırma (yalnızca bir sanal makine) şablonu - yönetilen diskleri** (sap-2-tier-marketplace-image-md)][sap-templates-2-tier-marketplace-image-md]

  Tek bir sanal makine ve yönetilen diskleri kullanarak iki katmanlı bir sistem oluşturmak için bu şablonu kullanın.
* [**Üç katmanlı yapılandırma (birden çok sanal makine) şablonu** (sap-3-katman-Pazar-görüntü)][sap-templates-3-tier-marketplace-image]

  Birden çok sanal makine kullanarak bir üç katmanlı sistemi oluşturmak için bu şablonu kullanın.
* [**Üç katmanlı yapılandırma (birden çok sanal makine) şablonu - yönetilen diskleri** (sap-3-tier-marketplace-image-md)][sap-templates-3-tier-marketplace-image-md]

  Birden çok sanal makine ve yönetilen diskleri kullanarak bir üç katmanlı sistemi oluşturmak için bu şablonu kullanın.

Azure portalında şablon için aşağıdaki parametreleri girin:

1. **Temel**:
   * **Abonelik**: Şablonu dağıtmak için kullanılacak aboneliği.
   * **Kaynak grubu**: Şablonu dağıtmak için kullanılacak kaynak grubu. Yeni bir kaynak grubu oluşturabilir veya mevcut bir kaynak grubu aboneliği seçin.
   * **Konum**: Şablonu dağıtmak yer. Mevcut bir kaynak grubu seçtiyseniz, bu kaynak grubu konumu kullanılır.

1. **Ayarları**:
   * **SAP sistem kimliği**: SAP sistemi kimliği (SID).
   * **İşletim sistemi türü**: Örneğin, Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12), Red Hat Enterprise Linux 7.2 (RHEL 7.2) veya Oracle Linux 7.2 dağıtmak istediğiniz işletim sistemi.

     Liste görünümü, tüm desteklenen işletim sistemlerini göstermez. SAP yazılım dağıtımı için desteklenen işletim sistemleri hakkında daha fazla bilgi için bkz. Not SAP [1928533].
   * **SAP sistemi boyutu**: SAP sistemine boyutu.

     Yeni sisteme sağlar SAP sayısı. Kaç tane SAP sistemi gerektiriyor değil eminseniz, SAP teknoloji iş ortağı veya sistem Entegratörü isteyin.
   * **Sistem kullanılabilirliği** (yalnızca üç katmanlı şablon): Sistem kullanılabilirliği.

     Seçin **HA** yüksek kullanılabilirlik yüklemesi için uygun bir yapılandırma için. İki veritabanı sunucusu ve iki sunucu için ABAP SAP merkezi Hizmetleri (ASCS) oluşturulur.
   * **Depolama türü** (yalnızca iki katmanlı şablon): Kullanmak için depolama türü.

     Daha büyük sistemler için yüksek oranda Azure Premium depolama kullanmanızı öneririz. Depolama türleri hakkında daha fazla bilgi için şu kaynaklara bakın:
      * [SAP DBMS örneği için Azure Premium SSD depolama kullanımı][2367194]
      * [RDBMS dağıtımlar için bir VM depolama yapısı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general#65fa79d6-a85f-47ee-890b-22e794f51a64)
      * [Premium Depolama: Azure sanal makine iş yükleri için yüksek performanslı depolama][storage-premium-storage-preview-portal]
      * [Microsoft Azure Depolama'ya giriş][storage-introduction]
   * **Yönetici kullanıcı adı** ve **yönetici parolası**: Bir kullanıcı adı ve parola.
     Yeni bir kullanıcı sanal makineye oturum açmak için oluşturulur.
   * **Yeni veya var olan bir alt ağa**: Yeni sanal ağ ve alt ağ oluşturulur veya var olan bir alt ağ kullanılan belirler. Şirket içi ağınıza bağlı bir sanal ağınız zaten varsa, seçin **varolan**.
   * **Alt ağ kimliği**: Tanımlanan bir alt ağa sahip olduğunuz mevcut bir Vnet'te VM dağıtmak istiyorsanız, VM atanmalıdır belirli bir alt ağ kimliği adı için. Kimliği genellikle şöyle görünür: /subscriptions/&lt;abonelik kimliği > /resourceGroups/&lt;kaynak grubu adı > /providers/Microsoft.Network/virtualNetworks/&lt;sanal ağ adı > /subnets/&lt;alt ağ adı >

1. **Hüküm ve koşullar**:  
    Gözden geçirin ve yasal koşulları kabul edin.

1. **Satın al**'ı seçin.

Azure VM Aracısı, Azure Marketi'nden bir görüntü kullandığınızda varsayılan olarak dağıtılır.

#### <a name="configure-proxy-settings"></a>Proxy ayarlarını yapılandırma

Şirket içi ağınıza nasıl yapılandırıldığına bağlı olarak, VM'niz proxy ayarlamak gerekebilir. Sanal makinenize bağlı ise şirket içi ağınıza VPN veya ExpressRoute, bir VM aracılığıyla Internet erişimine sahip olmayabilir ve gerekli Uzantıları'nı indirin ve izleme verilerini toplamak mümkün olmayacaktır. Daha fazla bilgi için [Proxy'yi yapılandırmak][deployment-guide-configure-proxy].

#### <a name="join-a-domain-windows-only"></a>(Yalnızca Windows) etki alanına Katıl

Azure dağıtımınızı bir Azure siteden siteye VPN bağlantısı veya ExpressRoute aracılığıyla şirket içi Active Directory ve DNS örneğine bağlı olup olmadığını (Bu adlandırılır *şirketler arası* içinde [Azure sanal makineler planlama SAP NetWeaver için ve uygulama][planning-guide]), VM bir şirket içi etki alanına katıldığını beklenir. Bu görev için dikkat edilecek noktalar hakkında daha fazla bilgi için bkz. [bir şirket içi etki alanına (yalnızca Windows) için bir VM katılmak][deployment-guide-4.3].

#### <a name="ec323ac3-1de9-4c3a-b770-4ff701def65b"></a>İzlemeyi Yapılandırma

Emin olmak için SAP ortamınızı açıklandığı gibi SAP için Azure izleme uzantısı ayarlamak destekler [Azure Gelişmiş izleme uzantısını SAP için yapılandırmak][deployment-guide-4.5]. SAP izleme için önkoşul denetimi ve çekirdek SAP ve SAP konak Aracısı, gerekli en düşük sürümlerle kaynaklarında listelenen [SAP kaynakları][deployment-guide-2.2].

#### <a name="monitoring-check"></a>İzleme denetimi

İzleme çalışıp çalışmadığını, açıklandığı denetleyin [denetimler ve uçtan uca izleme ayarlamak için sorun giderme][deployment-guide-troubleshooting-chapter].

#### <a name="post-deployment-steps"></a>Dağıtım sonrası adımlar

Sonra VM'yi oluşturmak ve VM dağıtılır, VM'yi gerekli yazılım bileşenleri yüklemeniz gerekir. Bu tür bir VM dağıtımı dağıtım/yazılım yükleme sırası nedeniyle yüklenecek yazılım zaten ya da azure'da, başka bir VM'de veya eklenebilecek disk olarak kullanılabilir olmalıdır. Veya hangi şirket içi bağlantı, varlıklar (yükleme paylaşımları) verildiğinde bir içi ve dışı karışık senaryo kullanmayı düşünün.

Azure VM'nizi dağıttıktan sonra aynı yönergeler ve bir şirket içi ortamda olduğu gibi sanal makinenizde SAP yazılımı yüklemek için Araçlar izleyin. Bir Azure VM'de SAP yazılımı yüklemek için hem SAP ve Microsoft karşıya yükleme ve Azure VHD'leri veya yönetilen diskler üzerinde SAP yükleme medyasında depolamak veya gerekli tüm SAP yükleme medyasını içeren bir dosya sunucusu olarak çalışır, bir Azure sanal makinesi, oluşturmanızı öneririz.

### <a name="54a1fc6d-24fd-4feb-9c57-ac588a55dff2"></a>Senaryo 2: SAP için özel bir görüntü ile bir VM dağıtma

Bir işletim sistemi veya DBMS farklı sürümlerini farklı düzeltme eki gereksinimleri olduğundan, Azure Marketi'nde bulma görüntüleri ihtiyaçlarınızı karşılamayabilir. Bunun yerine, daha sonra yeniden dağıtabilirsiniz, kendi işletim sistemi/DBMS VM görüntüsünü kullanarak bir VM oluşturmak isteyebilirsiniz.
Windows için bir tane oluşturmak için daha Linux için özel bir görüntü oluşturmak için farklı adımlar kullanın.

- - -
> ![Windows][Logo_Windows] Windows
>
> Birden çok sanal makine dağıtmak için kullanabileceğiniz bir Windows görüntüsünü hazırlamak için Windows ayarları (örneğin, Windows SID ve ana bilgisayar adı) soyutlanır veya gerekir şirket içi VM genelleştirilmiş. Kullanabileceğiniz [sysprep](https://msdn.microsoft.com/library/hh825084.aspx) Bunu yapmak için.
>
> ![Linux][Logo_Linux] Linux
>
> Birden çok sanal makine dağıtmak için kullanabileceğiniz bir Linux görüntüsü hazırlamak için bazı Linux ayarları soyutlanır veya gerekir şirket içi VM genelleştirilmiş. Kullanabileceğiniz `waagent -deprovision` Bunu yapmak için. Daha fazla bilgi için [Azure üzerinde çalışan Linux sanal makinesi yakalama] [ virtual-machines-linux-capture-image] ve [Azure Linux Aracısı Kullanım Kılavuzu][virtual-machines-linux-agent-user-guide-command-line-options].
>
>

- - -
Hazırlama ve özel bir görüntü oluşturma ve birden fazla yeni VM oluşturmak için kullanın. Bu açıklanan [planlama Azure sanal makineleri ve SAP NetWeaver uygulamasını][planning-guide]. Veritabanı içeriğinizi (geri yükleyen bir veritabanı yedeği sanal makineye bağlı bir diskten) yeni bir SAP sistemine yüklemek için SAP yazılım sağlama Yöneticisi'ni kullanarak veya doğrudan Azure depolama, veritabanı yedeklemesini geri yükleme verilirse, DBMS Bu destekler. Daha fazla bilgi için [SAP NetWeaver için Azure sanal makineleri DBMS dağıtım][dbms-guide]. (Özellikle de iki katmanlı sistemleri için), şirket içi VM'de SAP sistemine zaten yüklediyseniz, SAP sistem ayarlarını Azure VM dağıtıldıktan sonra SAP yazılım sağlama Yöneticisi (SAP tarafından desteklenen sistem yeniden adlandır yordamı kullanarak uyarlayabilirsiniz Not [1619720]). Aksi takdirde, Azure VM dağıttıktan sonra SAP yazılım yükleyebilirsiniz.

Aşağıdaki akış, bir özel görüntüsünden VM dağıtmak için adımları SAP özgü sırasını göstermektedir:

![VM dağıtımı için bir VM görüntüsü özel Market'te kullanarak SAP sistemlerini akış çizelgesi][deployment-guide-figure-300]

#### <a name="create-a-virtual-machine-by-using-the-azure-portal"></a>Azure portalını kullanarak bir sanal makine oluşturun

Azure portalını kullanarak yönetilen Disk görüntüsünden yeni bir sanal makine oluşturmak için en kolay yolu olan. Bir yönetme Disk görüntüsünün nasıl oluşturulacağı hakkında daha fazla bilgi için okuma [azure'da bir genelleştirilmiş VM'nin yönetilen görüntüsünü yakalama](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource)

1.  <https://ms.portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Compute%2Fimages> kısmına gidin. Veya, Azure portal menüsünde seçin **görüntüleri**.
1.  Tıklayın ve dağıtmak istediğiniz yönetilen Disk görüntüsü seçin **VM oluştur**

Sihirbaz, ek olarak sanal makine ağ arabirimleri ve depolama hesapları gibi tüm gerekli kaynakları oluşturmak için gerekli parametreleri ayarlamanız konusunda size yol gösterir. Bu parametrelerden bazıları şunlardır:

1. **Temel**:
   * **Ad**: (Sanal makine adı) kaynağının adı.
   * **VM disk türü**: İşletim sistemi diskinin disk türünü seçin. Premium depolama, veri diskleri için kullanmak istiyorsanız, işletim sistemi diski de Premium depolama kullanmanızı öneririz.
   * **Kullanıcı adı ve parola** veya **SSH ortak anahtarı**: Kullanıcı adı ve sağlama sırasında oluşturulan kullanıcı parolasını girin. Bir Linux sanal makinesi için makineye oturum açmak için kullandığınız genel güvenli Kabuk (SSH) anahtarı girebilirsiniz.
   * **Abonelik**: Yeni sanal makine sağlamak için kullanmak istediğiniz aboneliği seçin.
   * **Kaynak grubu**: Sanal makine için kaynak grubunun adı. Yeni bir kaynak grubu adı veya zaten bir kaynak grubu adını girebilirsiniz.
   * **Konum**: Yeni bir sanal makine dağıtılacağı yeri'ı tıklatın. Sanal makine, şirket içi ağınıza bağlanmak istiyorsanız, Azure şirket içi ağınıza bağlanan sanal ağ konumu seçtiğinizden emin olun. Daha fazla bilgi için [Microsoft Azure ağı] [ planning-guide-microsoft-azure-networking] içinde [planlama Azure sanal makineleri ve SAP NetWeaver uygulamasını] [ planning-guide].
1. **Boyutu**:

     Desteklenen VM türlerinin bir listesi için bkz. Not SAP [1928533]. Azure Premium depolama kullanmak istiyorsanız doğru VM türünün seçtiğinizden emin olun. Tüm VM türleri Premium depolamayı destekler. Daha fazla bilgi için [Depolama: Microsoft Azure depolama ve veri diskleri] [ planning-guide-storage-microsoft-azure-storage-and-data-disks] ve [Azure Premium depolama] [ planning-guide-azure-premium-storage] içinde [Azure sanal makineler planlama ve uygulama SAP NetWeaver için][planning-guide].

1. **Ayarları**:
   * **Depolama**
     * **Disk türü**: İşletim sistemi diskinin disk türünü seçin. Premium depolama, veri diskleri için kullanmak istiyorsanız, işletim sistemi diski de Premium depolama kullanmanızı öneririz.
     * **Yönetilen diskleri kullanma**: Yönetilen diskleri kullanmak istiyorsanız, Evet'i seçin. Yönetilen diskler hakkında daha fazla bilgi için bkz: bölüm [yönetilen diskler] [ planning-guide-managed-disks] Planlama Kılavuzu'nda.
   * **Ağ**
     * **Sanal ağ** ve **alt**: Sanal makine intranetinize ile tümleştirmek için şirket içi ağınıza bağlı sanal ağ'ı seçin.
     * **Genel IP adresi**: Kullanmak istediğiniz genel IP adresi seçin veya yeni bir ortak IP adresi oluşturmak için parametreler girin. Sanal makinenizi Internet üzerinden erişmek için genel bir IP adresi kullanabilirsiniz. Ayrıca, sanal makinenize güvenli erişim yardımcı olmak için ağ güvenlik grubu oluşturduğunuzdan emin olun.
     * **Ağ güvenlik grubu**: Daha fazla bilgi için [denetleyen ağ güvenlik grupları ile ağ trafiği akışını][virtual-networks-nsg].
   * **Uzantıları**: Dağıtımı ekleyerek sanal makine uzantıları yükleyebilirsiniz. Bu adımda uzantısı eklemek gerekmez. SAP destek için gereken uzantılarını daha sonra yüklenir. Bölüm bakın [Azure Gelişmiş izleme uzantısını SAP için yapılandırmak] [ deployment-guide-4.5] bu kılavuzdaki.
   * **Yüksek kullanılabilirlik**: Bir kullanılabilirlik kümesi seçin veya yeni bir kullanılabilirlik kümesi oluşturmak için parametreleri girin. Daha fazla bilgi için [Azure kullanılabilirlik kümeleri][planning-guide-3.2.3].
   * **İzleme**
     * **Önyükleme tanılaması**: Seçebileceğiniz **devre dışı** önyükleme tanılaması için.
     * **Konuk işletim sistemi tanılama**: Seçebileceğiniz **devre dışı** Tanılama izleme.

1. **Özet**:

   Seçimlerinizi gözden geçirin ve ardından **Tamam**.

Sanal makinenizi, seçili kaynak grubunda dağıtılmış.

#### <a name="create-a-virtual-machine-by-using-a-template"></a>Bir şablonu kullanarak bir sanal makine oluşturma

Azure portalından özel bir işletim sistemi görüntüsü kullanarak bir dağıtım oluşturmak için aşağıdaki SAP şablonlardan birini kullanın. Bu şablonlar yayınlanır [azure hızlı başlangıç şablonları GitHub deposunda][azure-quickstart-templates-github]. Ayrıca el ile bir sanal makine kullanarak oluşturabileceğiniz [PowerShell][virtual-machines-upload-image-windows-resource-manager].

* [**İki katmanlı yapılandırma (yalnızca bir sanal makine) şablonu** (sap-2-katman-kullanıcı-görüntü)][sap-templates-2-tier-user-image]

  Yalnızca bir sanal makineyi kullanarak iki katmanlı bir sistem oluşturmak için bu şablonu kullanın.
* [**İki katmanlı yapılandırma (yalnızca bir sanal makine) şablonu - yönetilen Disk görüntüsü** (sap-2-tier-user-image-md)][sap-templates-2-tier-user-image-md]

  Yalnızca bir sanal makine ve yönetilen Disk görüntüsü kullanarak iki katmanlı bir sistem oluşturmak için bu şablonu kullanın.
* [**Üç katmanlı yapılandırma (birden çok sanal makine) şablonu** (sap-3-katman-kullanıcı-görüntü)][sap-templates-3-tier-user-image]

  Birden çok sanal makine ya da kendi işletim sistemi görüntüsü kullanarak bir üç katmanlı sistemi oluşturmak için bu şablonu kullanın.
* [**Üç katmanlı yapılandırma (birden çok sanal makine) şablonu - yönetilen Disk görüntüsü** (sap-3-tier-user-image-md)][sap-templates-3-tier-user-image-md]

  Birden çok sanal makine veya kendi işletim sistemi görüntüsü ve bir yönetilen Disk görüntüsü kullanarak bir üç katmanlı sistemi oluşturmak için bu şablonu kullanın.

Azure portalında şablon için aşağıdaki parametreleri girin:

1. **Temel**:
   * **Abonelik**: Şablonu dağıtmak için kullanılacak aboneliği.
   * **Kaynak grubu**: Şablonu dağıtmak için kullanılacak kaynak grubu. Yeni bir kaynak grubu oluşturun veya mevcut bir kaynak grubu aboneliği seçin.
   * **Konum**: Şablonu dağıtmak yer. Mevcut bir kaynak grubu seçtiyseniz, bu kaynak grubu konumu kullanılır.
1. **Ayarları**:
   * **SAP sistem kimliği**: SAP sistem kimliği
   * **İşletim sistemi türü**: (Windows veya Linux) dağıtmak istediğiniz işletim sistemi türü.
   * **SAP sistemi boyutu**: SAP sistemine boyutu.

     Yeni sisteme sağlar SAP sayısı. Kaç tane SAP sistemi gerektiriyor değil eminseniz, SAP teknoloji iş ortağı veya sistem Entegratörü isteyin.
   * **Sistem kullanılabilirliği** (yalnızca üç katmanlı şablon): Sistem kullanılabilirliği.

     Seçin **HA** yüksek kullanılabilirlik yüklemesi için uygun bir yapılandırma için. İki veritabanı sunucusu ve iki sunucu ASCS için oluşturulur.
   * **Depolama türü** (yalnızca iki katmanlı şablon): Kullanmak için depolama türü.

     Daha büyük sistemler için yüksek oranda Azure Premium depolama kullanmanızı öneririz. Depolama türleri hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:
      * [SAP DBMS örneği için Azure Premium SSD depolama kullanımı][2367194]
      * [RDBMS dağıtımlar için bir VM depolama yapısı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general#65fa79d6-a85f-47ee-890b-22e794f51a64)
      * [Premium Depolama: Azure sanal makine iş yükleri için yüksek performanslı depolama][storage-premium-storage-preview-portal]
      * [Microsoft Azure Depolama'ya giriş][storage-introduction]
   * **Kullanıcı görüntüsü VHD URİ'si** (yalnızca yönetilmeyen disk görüntüsü şablonu): VHD, örneğin, https:// URI özel işletim sistemi yansımasını&lt;accountname >.blob.core.windows.net/vhds/userimage.vhd.
   * **Kullanıcı görüntüsü depolama hesabı** (yalnızca yönetilmeyen disk görüntüsü şablonu): Özel işletim sistemi görüntüsünün depolandığı, örneğin, depolama hesabının adını &lt;accountname > https:// içinde&lt;accountname >.blob.core.windows.net/vhds/userimage.vhd.
   * **userImageId** (yalnızca yönetilen disk görüntüsü şablonu): Kullanmak istediğiniz yönetilen Disk görüntüsü kimliği
   * **Yönetici kullanıcı adı** ve **yönetici parolası**: Kullanıcı adı ve parola.

     Yeni bir kullanıcı sanal makineye oturum açmak için oluşturulur.
   * **Yeni veya var olan bir alt ağa**: Yeni sanal ağ ve alt ağ oluşturulur veya var olan bir alt ağ kullanılan belirler. Şirket içi ağınıza bağlı bir sanal ağınız zaten varsa, seçin **varolan**.
   * **Alt ağ kimliği**: Tanımlanan bir alt ağa sahip olduğunuz mevcut bir Vnet'te VM dağıtmak istiyorsanız, VM atanmalıdır belirli bir alt ağ kimliği adı için. Kimliği genellikle şöyle görünür: /subscriptions/&lt;abonelik kimliği > /resourceGroups/&lt;kaynak grubu adı > /providers/Microsoft.Network/virtualNetworks/&lt;sanal ağ adı > /subnets/&lt;alt ağ adı >

1. **Hüküm ve koşullar**:  
    Gözden geçirin ve yasal koşulları kabul edin.

1. **Satın al**'ı seçin.

#### <a name="install-the-vm-agent-linux-only"></a>VM Aracısı (yalnızca Linux)

Önceki bölümde açıklanan şablonlarını kullanmak için Linux Aracısı kullanıcı görüntüde yüklü olması gereken veya dağıtım başarısız olur. Karşıdan yükleyip VM Aracısı kullanıcı resimde açıklandığı [indirme, yükleme ve Azure VM aracısını etkinleştirin][deployment-guide-4.4]. Şablonları kullanmazsanız, daha sonra VM Aracısı de yükleyebilirsiniz.

#### <a name="join-a-domain-windows-only"></a>(Yalnızca Windows) etki alanına Katıl

Azure dağıtımınızı bir Azure siteden siteye VPN bağlantısı veya Azure ExpressRoute aracılığıyla şirket içi Active Directory ve DNS örneğine bağlı olup olmadığını (Bu adlandırılır *şirketler arası* içinde [Azure sanal makineler Planlama ve uygulama için SAP NetWeaver][planning-guide]), VM bir şirket içi etki alanına katıldığını beklenir. Bu adım için dikkat edilecek noktalar hakkında daha fazla bilgi için bkz. [bir şirket içi etki alanına (yalnızca Windows) için bir VM katılmak][deployment-guide-4.3].

#### <a name="configure-proxy-settings"></a>Proxy ayarlarını yapılandırma

Şirket içi ağınıza nasıl yapılandırıldığına bağlı olarak, VM'niz proxy ayarlamak gerekebilir. Sanal makinenize bağlı ise şirket içi ağınıza VPN veya ExpressRoute, bir VM aracılığıyla Internet erişimine sahip olmayabilir ve gerekli Uzantıları'nı indirin ve izleme verilerini toplamak mümkün olmayacaktır. Daha fazla bilgi için [Proxy'yi yapılandırmak][deployment-guide-configure-proxy].

#### <a name="configure-monitoring"></a>İzlemeyi yapılandırma

Emin olmak için SAP ortamınızı açıklandığı gibi SAP için Azure izleme uzantısı ayarlamak destekler [Azure Gelişmiş izleme uzantısını SAP için yapılandırmak][deployment-guide-4.5]. SAP izleme için önkoşul denetimi ve çekirdek SAP ve SAP konak Aracısı, gerekli en düşük sürümlerle kaynaklarında listelenen [SAP kaynakları][deployment-guide-2.2].

#### <a name="monitoring-check"></a>İzleme denetimi

İzleme çalışıp çalışmadığını, açıklandığı denetleyin [denetimler ve uçtan uca izleme ayarlamak için sorun giderme][deployment-guide-troubleshooting-chapter].


### <a name="a9a60133-a763-4de8-8986-ac0fa33aa8c1"></a>Senaryo 3: Azure olmayan genelleştirilmiş VHD ile SAP kullanarak bir şirket içi VM taşıma

Bu senaryoda, belirli bir SAP sistemine bir şirket içi ortamdan Azure'a taşımayı planlayın. Veri ve günlük dosyalarını azure'a DBMS ile işletim sistemi, SAP ikili dosyaları ve sonunda DBMS ikili dosyaları olan VHD'nin yanı sıra, VHD'leri yükleyerek bunu yapabilirsiniz. Açıklanan senaryoyu aksine [Senaryo 2: SAP için özel bir görüntü ile bir VM dağıtma][deployment-guide-3.3], bu durumda, konak adı, SAP SID tutun ve şirket içi ortamda yapılandırılmış olan çünkü Azure VM'de SAP kullanıcı hesapları. İşletim sistemi genelleştirmek gerekmez. Bu senaryo genellikle burada şirket içi SAP ortamının bir parçası çalışır ve bir bölümü Azure üzerinde çalışan şirket içi senaryolar için de geçerlidir.

Bu senaryoda, VM Aracısı olduğunu **değil** dağıtımı sırasında otomatik olarak yüklenmiş. VM aracısı ve Azure Gelişmiş izleme uzantısını SAP için Azure üzerinde SAP NetWeaver'ı çalıştırmak için gerekli olduğundan, indirme, yükleme ve sanal makineyi oluşturduktan sonra her iki bileşenin el ile etkinleştirmeniz gerekir.

Azure VM Aracısı hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın.

- - -
> ![Windows][Logo_Windows] Windows
>
> [Azure sanal makine Aracısı genel bakış][virtual-machines-windows-agent-user-guide]
>
> ![Linux][Logo_Linux] Linux
>
> [Azure Linux Aracısı Kullanım Kılavuzu][virtual-machines-linux-agent-user-guide]
>
>

- - -

Aşağıdaki akış bir şirket içi VM genelleştirilmiş olmayan bir Azure VHD'nin kullanarak taşımak için adımlar dizisini gösterir:

![VM dağıtımı için bir VM disk kullanarak SAP sistemlerini akış çizelgesi][deployment-guide-figure-400]

Disk zaten karşıya yüklendi ve Azure'da tanımlanan varsa (bkz [planlama Azure sanal makineleri ve SAP NetWeaver uygulamasını][planning-guide]), yapmak görevleri açıklanan önümüzdeki birkaç bölümler.

#### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Azure Portalı aracılığıyla özel bir işletim sistemi diski kullanarak bir dağıtım oluşturmak için yayımlanan SAP şablonu kullanın. [azure hızlı başlangıç şablonları GitHub deposunda][azure-quickstart-templates-github]. Ayrıca el ile bir sanal makine PowerShell kullanarak oluşturabilirsiniz.

* [**İki katmanlı yapılandırma (yalnızca bir sanal makine) şablonu** (sap-2-katman-kullanıcı-disk)][sap-templates-2-tier-os-disk]

  Yalnızca bir sanal makineyi kullanarak iki katmanlı bir sistem oluşturmak için bu şablonu kullanın.
* [**İki katmanlı yapılandırma (yalnızca bir sanal makine) şablonu - yönetilen Disk** (sap-2-tier-user-disk-md)][sap-templates-2-tier-os-disk-md]

  Yalnızca bir sanal makine ve yönetilen Disk kullanarak iki katmanlı bir sistem oluşturmak için bu şablonu kullanın.

Azure portalında şablon için aşağıdaki parametreleri girin:

1. **Temel**:
   * **Abonelik**: Şablonu dağıtmak için kullanılacak aboneliği.
   * **Kaynak grubu**: Şablonu dağıtmak için kullanılacak kaynak grubu. Yeni bir kaynak grubu oluşturun veya mevcut bir kaynak grubu aboneliği seçin.
   * **Konum**: Şablonu dağıtmak yer. Mevcut bir kaynak grubu seçtiyseniz, bu kaynak grubu konumu kullanılır.
1. **Ayarları**:
   * **SAP sistem kimliği**: SAP sistem kimliği
   * **İşletim sistemi türü**: (Windows veya Linux) dağıtmak istediğiniz işletim sistemi türü.
   * **SAP sistemi boyutu**: SAP sistemine boyutu.

     Yeni sisteme sağlar SAP sayısı. Kaç tane SAP sistemi gerektiriyor değil eminseniz, SAP teknoloji iş ortağı veya sistem Entegratörü isteyin.
   * **Depolama türü** (yalnızca iki katmanlı şablon): Kullanmak için depolama türü.

     Daha büyük sistemler için yüksek oranda Azure Premium depolama kullanmanızı öneririz. Depolama türleri hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:
      * [SAP DBMS örneği için Azure Premium SSD depolama kullanımı][2367194]
      * [RDBMS dağıtımlar için bir VM depolama yapısı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general#65fa79d6-a85f-47ee-890b-22e794f51a64)
      * [Premium Depolama: Azure sanal makine iş yükleri için yüksek performanslı depolama][storage-premium-storage-preview-portal]
      * [Microsoft Azure Depolama'ya giriş][storage-introduction]
   * **İşletim sistemi diski VHD URI'si** (yalnızca yönetilmeyen disk şablonu): Özel işletim sistemi diski, örneğin, https:// URI&lt;accountname >.blob.core.windows.net/vhds/osdisk.vhd.
   * **İşletim sistemi diski Disk kimliği yönetilen** (yalnızca yönetilen disk şablonu): Yönetilen diski işletim sistemi disk kimliği /subscriptions/92d102f7-81a5-4df7-9877-54987ba97dd9/resourceGroups/group/providers/Microsoft.Compute/disks/WIN
   * **Yeni veya var olan bir alt ağa**: Yeni sanal ağ ve alt ağ oluşturulur veya varolan bir alt ağı kullanılır olup olmadığını belirler. Şirket içi ağınıza bağlı bir sanal ağınız zaten varsa, seçin **varolan**.
   * **Alt ağ kimliği**: Tanımlanan bir alt ağa sahip olduğunuz mevcut bir Vnet'te VM dağıtmak istiyorsanız, VM atanmalıdır belirli bir alt ağ kimliği adı için. Kimliği genellikle şöyle görünür: /subscriptions/&lt;abonelik kimliği > /resourceGroups/&lt;kaynak grubu adı > /providers/Microsoft.Network/virtualNetworks/&lt;sanal ağ adı > /subnets/&lt;alt ağ adı >

1. **Hüküm ve koşullar**:  
    Gözden geçirin ve yasal koşulları kabul edin.

1. **Satın al**'ı seçin.

#### <a name="install-the-vm-agent"></a>VM aracısını yükleyin

Önceki bölümde açıklanan şablonlarını kullanmak için işletim sistemi diskinde VM Aracısı yüklenmelidir ve dağıtım başarısız olur. Karşıdan yükleyip VM Aracısı VM içinde anlatıldığı gibi [indirme, yükleme ve Azure VM aracısını etkinleştirin][deployment-guide-4.4].

Önceki bölümde açıklanan şablonları kullanmazsanız, daha sonra VM Aracısı de yükleyebilirsiniz.

#### <a name="join-a-domain-windows-only"></a>(Yalnızca Windows) etki alanına Katıl

Azure dağıtımınızı bir Azure siteden siteye VPN bağlantısı veya ExpressRoute aracılığıyla şirket içi Active Directory ve DNS örneğine bağlı olup olmadığını (Bu adlandırılır *şirketler arası* içinde [Azure sanal makineler planlama SAP NetWeaver için ve uygulama][planning-guide]), VM bir şirket içi etki alanına katıldığını beklenir. Bu görev için dikkat edilecek noktalar hakkında daha fazla bilgi için bkz. [bir şirket içi etki alanına (yalnızca Windows) için bir VM katılmak][deployment-guide-4.3].

#### <a name="configure-proxy-settings"></a>Proxy ayarlarını yapılandırma

Şirket içi ağınıza nasıl yapılandırıldığına bağlı olarak, VM'niz proxy ayarlamak gerekebilir. Sanal makinenize bağlı ise şirket içi ağınıza VPN veya ExpressRoute, bir VM aracılığıyla Internet erişimine sahip olmayabilir ve gerekli Uzantıları'nı indirin ve izleme verilerini toplamak mümkün olmayacaktır. Daha fazla bilgi için [Proxy'yi yapılandırmak][deployment-guide-configure-proxy].

#### <a name="configure-monitoring"></a>İzlemeyi yapılandırma

Emin olmak için SAP ortamınızı açıklandığı gibi SAP için Azure izleme uzantısı ayarlamak destekler [Azure Gelişmiş izleme uzantısını SAP için yapılandırmak][deployment-guide-4.5]. SAP izleme için önkoşul denetimi ve çekirdek SAP ve SAP konak Aracısı, gerekli en düşük sürümlerle kaynaklarında listelenen [SAP kaynakları][deployment-guide-2.2].

#### <a name="monitoring-check"></a>İzleme denetimi

İzleme çalışıp çalışmadığını, açıklandığı denetleyin [denetimler ve uçtan uca izleme ayarlamak için sorun giderme][deployment-guide-troubleshooting-chapter].

## <a name="update-the-monitoring-configuration-for-sap"></a>SAP için izleme yapılandırmasını güncelleştirme

Aşağıdaki senaryolardan birinde SAP İzleme yapılandırmasını güncelleştirme:
* Birleşik Microsoft/SAP ekibi, izleme yeteneklerini genişletir ve daha fazla veya daha az sayaç ister.
* Microsoft izleme verileri sunan temel alınan Azure altyapısının yeni bir sürüm sunar ve bu değişiklikleri uyarlanabilen Azure Gelişmiş izleme uzantısını SAP için gerekiyor.
* Azure VM için ek veri diskleri bağlayın veya veri diski kaldırabilirsiniz. Bu senaryoda depolama ile ilgili veri koleksiyonu güncelleştirin. Ekleyerek veya uç noktalarını silme veya IP adresleri atayarak bir VM yapılandırmanızı değiştirme, İzleme yapılandırmasını etkilemez.
* Azure VM, boyutu gibi herhangi bir VM boyutu A5 boyutuna değiştirin.
* Azure VM için yeni ağ arabirimi ekleyin.

İzleme ayarlarını güncelleştirmek için izleme altyapısının içindeki adımları izleyerek güncelleştirme [Azure Gelişmiş izleme uzantısını SAP için yapılandırmak][deployment-guide-4.5].

## <a name="detailed-tasks-for-sap-software-deployment"></a>Ayrıntılı görevler için SAP yazılım dağıtımı

Bu bölüm yapılandırma ve dağıtım sürecini belirli görevleri yapmak için ayrıntılı adımları verilmiştir.

### <a name="604bcec2-8b6e-48d2-a944-61b0f5dee2f7"></a>Azure PowerShell cmdlet'lerini dağıtma

1. Git [Microsoft Azure indirmeleri](https://azure.microsoft.com/downloads/).
1. Altında **komut satırı araçları**altında **PowerShell**seçin **Windows yükleme**.
1. İndirilen dosyayı (örneğin, WindowsAzurePowershellGet.3f.3f.3fnew.exe) için Microsoft İndirme Yöneticisi iletişim kutusunda seçin **çalıştırma**.
1. Microsoft Web Platformu Yükleyicisi (Microsoft Web PI) çalıştırmak için seçin **Evet**.
1. Bu görünür benzer bir sayfa:

   ![Azure PowerShell cmdlet'lerini yükleme sayfası][deployment-guide-figure-500]<a name="figure-5"></a>

1. Seçin **yükleme**ve ardından Microsoft yazılım lisans koşullarını kabul edin.
1. PowerShell'in yüklendiğinden. Seçin **son** Yükleme Sihirbazı'nı kapatın.

Sık güncelleştirmeler genellikle aylık güncelleştirilir PowerShell Cmdlet'lerine bakın. Güncelleştirmeleri denetlemek için en kolay yolu, önceki yükleme adımları, 5. adımda gösterilen yükleme sayfasında kadar yapmaktır. Cmdlet'ler yayın tarih ve yayın sayısı 5. adımda gösterilen sayfasında bulunur. SAP Not aksi belirtilmedikçe [1928533] veya SAP notu [2015553], Azure PowerShell cmdlet'lerinin en son sürümüyle çalışma öneririz.

Azure PowerShell cmdlet'lerini, bilgisayarınızda yüklü sürümünü denetlemek için bu PowerShell komutunu çalıştırın:
```powershell
(Get-Module Az.Compute).Version
```
Sonucu şöyle görünür:

![Azure PowerShell cmdlet sürüm denetimi sonucu][deployment-guide-figure-600]
<a name="figure-6"></a>

Bilgisayarınızda yüklü Azure cmdlet sürümü geçerli sürümü, yükleme sihirbazının ilk sayfasında bu ekleyerek gösterir **(yüklü)** ürün başlığı için (aşağıdaki ekran görüntüsüne bakın). PowerShell Azure cmdlet'lerinizi güncel. Yükleme Sihirbazı'nı kapatmak için seçin **çıkış**.

![Azure PowerShell cmdlet'lerinin en son sürümünü yüklenip yüklenmediğini gösteren Azure PowerShell cmdlet'lerini yükleme sayfası][deployment-guide-figure-700]
<a name="figure-7"></a>

### <a name="1ded9453-1330-442a-86ea-e0fd8ae8cab3"></a>Azure CLI'yı dağıtma

1. Git [Microsoft Azure indirmeleri](https://azure.microsoft.com/downloads/).
1. Altında **komut satırı araçları**altında **Azure komut satırı arabirimi**seçin **yükleme** işletim sisteminiz için bağlantı.
1. İndirilen dosyayı (örneğin, WindowsAzureXPlatCLI.3f.3f.3fnew.exe) için Microsoft İndirme Yöneticisi iletişim kutusunda seçin **çalıştırma**.
1. Microsoft Web Platformu Yükleyicisi (Microsoft Web PI) çalıştırmak için seçin **Evet**.
1. Bu görünür benzer bir sayfa:

   ![Azure PowerShell cmdlet'lerini yükleme sayfası][deployment-guide-figure-500]<a name="figure-5"></a>

1. Seçin **yükleme**ve ardından Microsoft yazılım lisans koşullarını kabul edin.
1. Azure CLI yüklendi. Seçin **son** Yükleme Sihirbazı'nı kapatın.

Güncelleştirmeleri genellikle her ay güncelleştirilen Azure CLI için sık sık kontrol edin. Güncelleştirmeleri denetlemek için en kolay yolu, önceki yükleme adımları, 5. adımda gösterilen yükleme sayfasında kadar yapmaktır.

Azure CLI, bilgisayarınızda yüklü sürümünü denetlemek için şu komutu çalıştırın:
```
azure --version
```

Sonucu şöyle görünür:

![Azure CLI Sürüm denetimi sonucu][deployment-guide-figure-760]
<a name="0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda"></a>

### <a name="31d9ecd6-b136-4c73-b61e-da4a29bbc9cc"></a>Bir şirket içi etki alanı (yalnızca Windows) için bir ekleme

Burada şirket içi Active Directory ve DNS Azure'da genişletilir, bir şirket içi senaryosunda SAP VM dağıtırsanız, sanal makinelerin bir şirket içi etki alanı katıldığınız bekleniyor. Bir şirket içi etki alanı ve bir şirket içi etki alanının bir üyesi olması gereken ek yazılım için bir VM katılmak için ayrıntılı adımlar, müşteri tarafından değişir. Genellikle, bir VM için bir şirket içi etki alanına katılmak için kötü amaçlı yazılımdan koruma yazılımı gibi ek yazılımlar ve izleme ya da yedekleme yazılımları yüklemeniz gerekir.

Bu senaryoda, ayrıca bir VM ortamınızda bir etki alanına katıldığında Internet proxy ayarları zorlanırsa aynı proxy ayarlarını Windows yerel sistem hesabını (S-1-5-18) Konuk VM içinde olduğundan emin olun gerekir. Etki alanı, etki alanındaki sistemlerine uygulanan Grup İlkesi'ni kullanarak proxy zorlamak için en kolay seçenektir bakın.

### <a name="c7cbb0dc-52a4-49db-8e03-83e7edc2927d"></a>İndirme, yükleme ve Azure VM aracısını etkinleştirin

(Örneğin, sysprep veya Windows Sistem hazırlığı aracı kaynaklanmayan bir görüntü) genelleştirilmiş olmayan bir işletim sistemi görüntü üzerinden dağıtılan sanal makineler için el ile indirme, yükleme ve Azure VM Aracısı'nı etkinleştirmeniz gerekir.

Azure Market'ten bir VM dağıtırsanız, bu adım gerekli değildir. Görüntüleri Azure Market'te Azure VM Aracısı zaten var.

#### <a name="b2db5c9a-a076-42c6-9835-16945868e866"></a>Windows

1. Azure VM Aracısı'nı indirme:
   1.  İndirme [Azure VM Aracısı Yükleyici paketi](https://go.microsoft.com/fwlink/?LinkId=394789).
   1.  VM Aracısı MSI paketi yerel olarak bir kişisel bilgisayar veya sunucuda Store.
1. Azure VM aracısını yükleyin:
   1.  Dağıtılan Azure VM ile Uzak Masaüstü Protokolü (RDP) kullanarak bağlanın.
   1.  VM üzerinde bir Windows Explorer penceresi açın ve hedef dizini için VM Aracısı bir MSI dosyası seçin.
   1.  Azure VM Aracısı yükleyicisi MSI dosyasını yerel bilgisayar/sunucunuzdan VM'de VM aracısının hedef dizine sürükleyin.
   1.  VM'deki MSI dosyasını çift tıklatın.
1. Şirket içi etki alanına katılmış olan VM'ler için açıklandığı nihai Internet proxy ayarları ayrıca VM, Windows yerel sistem hesabına (S-1-5-18) uyguladığınızdan emin olun. [Proxy'yi yapılandırmak][deployment-guide-configure-proxy]. VM Aracısı, bu bağlamda çalışır ve Azure'a bağlanabilmesi gerekir.

Azure VM Aracısı'nı güncelleştirmek için kullanıcı etkileşimi gereklidir. VM Aracısı, otomatik olarak güncelleştirilir ve VM yeniden başlatma gerektirmez.

#### <a name="6889ff12-eaaf-4f3c-97e1-7c9edc7f7542"></a>Linux

Linux için VM aracısı yüklemek için aşağıdaki komutları kullanın:

* **SUSE Linux Enterprise Server (SLES)**

  ```
  sudo zypper install WALinuxAgent
  ```

* **Red Hat Enterprise Linux (RHEL) veya Oracle Linux**

  ```
  sudo yum install WALinuxAgent
  ```

Azure Linux aracısını Güncelleştirme Aracısı'nı zaten yüklediyseniz, açıklanan adımları uygulayın [bir VM'de Azure Linux Aracısı Github'dan en son sürüme güncelleştirme][virtual-machines-linux-update-agent].

### <a name="baccae00-6f79-4307-ade4-40292ce4e02d"></a>Proxy yapılandırma

Windows Proxy'yi yapılandırmak için uygulayacağınız adımlar, Linux proxy yapılandırma şekliniz farklıdır.

#### <a name="windows"></a>Windows

Proxy ayarlarını İnternet'e erişmek yerel sistem hesabı için doğru ayarlanmış olması gerekir. Proxy ayarlarını Grup İlkesi tarafından ayarlanmamışsa, yerel sistem hesabı ayarlarını yapılandırabilirsiniz.

1. Git **Başlat**, girin **gpedit.msc**ve ardından **Enter**.
1. Seçin **Bilgisayar Yapılandırması** > **Yönetim Şablonları** > **Windows bileşenleri**  >   **Internet Explorer**. Emin olun ayarı **proxy ayarları makine başına (yerine kullanıcı başına) olun** devre dışı bırakılmış veya yapılandırılmamış.
1. İçinde **Denetim Masası**Git **ağ ve Paylaşım Merkezi** > **Internet Seçenekleri**.
1. Üzerinde **bağlantıları** sekmesinde **LAN Ayarları** düğmesi.
1. NET **ayarlarını otomatik olarak algıla** onay kutusu.
1. Seçin **AĞINIZ için bir proxy sunucusu kullan** onay kutusunu işaretleyin ve ardından proxy adresi ve bağlantı noktası girin.
1. Seçin **Gelişmiş** düğmesi.
1. İçinde **özel durumları** kutusuna, IP adresi girin **168.63.129.16**. **Tamam**’ı seçin.

#### <a name="linux"></a>Linux

Yapılandırma dosyası şu konumdadır Microsoft Azure Konuk Aracısı'nın doğru ara sunucu yapılandırma \\vb.\\waagent.conf.

Aşağıdaki parametreleri ayarlayın:

1. **HTTP proxy konağını**. Örneğin, kümesine **proxy.corp.local**.
   ```
   HttpProxy.Host=<proxy host>

   ```
1. **HTTP proxy bağlantı noktası**. Örneğin, kümesine **80**.
   ```
   HttpProxy.Port=<port of the proxy host>

   ```
1. Aracıyı yeniden başlatın.

   ```
   sudo service waagent restart
   ```

Proxy ayarları \\vb.\\waagent.conf gerekli VM uzantıları için de geçerlidir. Azure depoları kullanmak istiyorsanız, bu depoları trafiği şirket içi intranetiniz üzerinden olmayacağı emin olun. Kullanıcı tanımlı yollar, zorlamalı tünel etkinleştirmek için bir rota eklediğinizden emin olun oluşturduysanız, doğrudan Internet'e ve siteden siteye VPN bağlantınız üzerinden değil de, trafiği yönlendirir.

* **SLES**

  Ayrıca listelenen IP adresleri için yollar eklemeniz gerekir \\vb.\\regionserverclnt.cfg. Aşağıdaki şekil, bir örnek göstermektedir:

  ![Zorlamalı tünel oluşturma][deployment-guide-figure-50]


* **RHEL**

  Ayrıca listelenen konakların IP adresleri için yollar eklemeniz gerekir \\vb.\\yum.repos.d\\rhuı yük Dengeleyiciler. Örneğin, önceki şekilde bakın.

* **Oracle Linux**

  Azure'da Oracle Linux için depo vardır. Oracle Linux için kendi depolarınızı yapılandırın veya genel depolar kullanmanız gerekir.

Kullanıcı tanımlı yollar hakkında daha fazla bilgi için bkz. [kullanıcı tanımlı yollar ve IP iletme][virtual-networks-udr-overview].

### <a name="d98edcd3-f2a1-49f7-b26a-07448ceb60ca"></a>SAP için Azure Gelişmiş izleme uzantısı yapılandırma

Ne zaman hazırladığınız VM açıklandığı [dağıtım senaryoları için azure'da SAP sanal makinelerinin][deployment-guide-3], Azure VM Aracısı sanal makineye yüklenir. Azure Gelişmiş izleme uzantısını Azure uzantı deposunda küresel Azure veri merkezlerinde kullanılabilir olan, SAP için sonraki adım dağıtmaktır. Daha fazla bilgi için [planlama Azure sanal makineleri ve SAP NetWeaver uygulamasını][planning-guide-9.1].

Yükleme ve yapılandırma Azure Gelişmiş izleme uzantısını SAP için PowerShell veya Azure CLI'yı kullanabilirsiniz. Windows makine kullanarak bir Windows veya Linux VM'i uzantıyı yüklemek için bkz: [Azure PowerShell][deployment-guide-4.5.1]. Bir Linux Masaüstü'nü kullanarak bir Linux VM üzerinde uzantıyı yüklemek için bkz: [Azure CLI][deployment-guide-4.5.2].

#### <a name="987cf279-d713-4b4c-8143-6b11589bb9d4"></a>Linux ve Windows Vm'leri için Azure PowerShell

Azure Gelişmiş izleme uzantısını SAP için PowerShell kullanarak yüklemek için:

1. Azure PowerShell cmdlet'ini en son sürümünü yüklediğinizden emin olun. Daha fazla bilgi için [dağıtma, Azure PowerShell cmdlet'lerini][deployment-guide-4.1].  
1. Aşağıdaki PowerShell cmdlet’ini çalıştırın.
    Kullanılabilir ortamların listesi için çalıştırma `commandlet Get-AzEnvironment`. Küresel Azure kullanmak istiyorsanız, ortamınızın olup **AzureCloud**. Çin'de Azure için seçin **AzureChinaCloud**.

    ```powershell
    $env = Get-AzEnvironment -Name <name of the environment>
    Connect-AzAccount -Environment $env
    Set-AzContext -SubscriptionName <subscription name>

    Set-AzVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
    ```

Hesap verilerinizi girin ve Azure sanal makineyi tanımlamak sonra betik gerekli uzantılarını dağıtır ve gerekli özellikleri sağlar. Bu işlem birkaç dakika sürebilir.
Hakkında daha fazla bilgi için `Set-AzVMAEMExtension`, bkz: [kümesi AzVMAEMExtension][msdn-set-Azvmaemextension].

![SAP özgü aktarılmadığı Azure cmdlet kümesi AzVMAEMExtension][deployment-guide-figure-900]

`Set-AzVMAEMExtension` Yapılandırma, SAP için izleme ana bilgisayarı yapılandırmak için gereken tüm adımları yapar.

Betik çıktısı, aşağıdaki bilgileri içerir:

* İşletim sistemi diski ve tüm ek veri diskleri için izleme yapılandırıldığını onayı.
* Sonraki iki iletileri belirli bir depolama hesabına depolama ölçümleri yapılandırmasını doğrulayın.
* Tek satırlık bir çıkış izleme yapılandırması, gerçek güncelleştirmenin durumunu sağlar.
* Başka bir satır çıktı yapılandırma dağıtılan güncelleştirilmiş veya onaylar.
* Çıkışının son satırında bilgilendirme amaçlıdır. Bu, İzleme yapılandırmasını test etmek için seçenekleri gösterir.
* Azure Gelişmiş izleme tüm adımları başarıyla yürütüldüğünü ve Azure altyapı gerekli verileri sağlar denetlemek için Azure Gelişmiş izleme uzantısı, SAP için hazır olma denetimi içindeanlatıldığıgibidevam[Azure SAP Gelişmiş izleme için hazır olma denetimi][deployment-guide-5.1].
* İlgili verileri toplamak Azure tanılama 15-30 dakika bekleyin.

#### <a name="408f3779-f422-4413-82f8-c57a23b4fc2f"></a>Linux VM'ler için Azure CLI

Azure Gelişmiş izleme uzantısını SAP için Azure CLI kullanarak yüklemek için:

   1. Açıklandığı gibi Klasik Azure CLI yükleme [Klasik Azure CLI yükleme][azure-cli].
   1. Azure hesabınızla oturum açın:

      ```
      azure login
      ```

   1. Azure Resource Manager moduna geçin:

      ```
      azure config mode arm
      ```

   1. Azure Gelişmiş izleme etkinleştir:

      ```
      azure vm enable-aem <resource-group-name> <vm-name>
      ```

1. Azure CLI 2.0 kullanarak yükleme

   1. Azure CLI 2. 0'da, bölümünde anlatılan şekilde yükleyin [Azure CLI 2.0 yükleme][azure-cli-2].
   1. Azure hesabınızla oturum açın:

      ```
      az login
      ```

   1. Azure CLI AEM uzantısını yükle
  
      ```
      az extension add --name aem
      ```
  
   1. Uzantıyı yükleyin
  
      ```
      az vm aem set -g <resource-group-name> -n <vm name>
      ```

1. Azure Gelişmiş izleme uzantısını Azure Linux VM'de etkin olduğunu doğrulayın. Denetleme olup olmadığını dosya \\var\\LIB\\AzureEnhancedMonitor\\PerfCounters bulunmaktadır. Bir komut isteminde varsa Azure Gelişmiş izleme tarafından toplanan bilgiler görüntülemek için şu komutu çalıştırın:

   ```
   cat /var/lib/AzureEnhancedMonitor/PerfCounters
   ```

   Çıktı şuna benzer:
   ```
   ...
   2;cpu;Current Hw Frequency;;0;2194.659;MHz;60;1444036656;saplnxmon;
   2;cpu;Max Hw Frequency;;0;2194.659;MHz;0;1444036656;saplnxmon;
   ...
   ```

## <a name="564adb4f-5c95-4041-9616-6635e83a810b"></a>Denetimler ve uçtan uca izleme için sorun giderme

Azure VM dağıttıktan ve ilgili Azure izleme altyapıyı ayarladıktan sonra Azure Gelişmiş izleme uzantısını tüm bileşenlerinin beklendiği gibi çalışıp çalışmadığını denetleyin.

Azure Gelişmiş izleme uzantısı için SAP için hazır olma denetimi açıklandığı gibi çalıştırın [hazırlık denetimi için Azure Gelişmiş izleme uzantısı için SAP][deployment-guide-5.1]. Tüm hazırlık denetimi sonucu pozitif ve tüm ilgili performans sayaçlarını Tamam görünürse, Azure izleme başarıyla ayarlandığını gösterdiğinde. SAP notları açıklandığı gibi SAP konak Aracısı yükleme işlemine devam etmeden [SAP kaynakları][deployment-guide-2.2]. Hazır olma denetimi sayaçları eksik olduğunu gösteriyorsa, sistem durumu denetimi için Azure izleme altyapısının açıklandığı gibi çalıştırın. [sistem durumu denetimi için Azure izleme altyapı yapılandırmasını] [ deployment-guide-5.2]. Daha fazla sorun giderme seçenekleri için bkz. [SAP için sorun giderme Azure izleme][deployment-guide-5.3].

### <a name="bb61ce92-8c5c-461f-8c53-39f5e5ed91f2"></a>Azure Gelişmiş izleme uzantısı için SAP için hazırlık denetimi

Bu onay SAP uygulamanızın içinde görüntülenen tüm performans ölçümlerinin temel alınan Azure izleme altyapısı tarafından sağlandığından emin olur.

#### <a name="run-the-readiness-check-on-a-windows-vm"></a>Bir Windows VM'de hazırlık denetimi Çalıştır

1. Azure sanal makinede oturum açın (yönetici hesabı kullanarak gerekli değildir).
1. Bir komut istemi penceresi açın.
1. Komut isteminde dizini Azure Gelişmiş izleme uzantısını SAP için yükleme klasörünü değiştirin: C:\\paketleri\\eklentileri\\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;sürüm >\\bırak

   *Sürüm* izleme uzantısı yolunu farklılık gösterebilir. Klasörleri birden fazla sürümünü yükleme klasöründeki izleme uzantısı için görürseniz, AzureEnhancedMonitoring Windows hizmet yapılandırmasını denetleyin ve ardından olarak belirtilen klasöre geçin *yürütülebilir dosyanın yolu* .

   ![Azure Gelişmiş izleme uzantısını SAP için çalışan hizmetin özellikleri][deployment-guide-figure-1000]

1. Komut isteminde çalıştırın **azperflib.exe** hiçbir parametre olmadan.

   > [!NOTE]
   > Azperflib.exe döngüsel olarak çalıştırır ve 60 saniyede toplanan sayaçların güncelleştirir. Döngü sona erdirmek için komut istemi penceresini kapatın.
   >
   >

Azure Gelişmiş izleme uzantısı yüklü değil veya AzureEnhancedMonitoring hizmeti çalışmıyor, uzantının düzgün yapılandırılmadı. Uzantı dağıtma hakkında ayrıntılı bilgi için bkz. [SAP için Azure izleme altyapısının sorun giderme][deployment-guide-5.3].

> [!NOTE]
> Azperflib.exe kendi amacıyla kullanılan bir bileşenidir. Azure VM'de SAP konak aracısı için ilgili verileri izleme sağlayan bir bileşendir.
> 

##### <a name="check-the-output-of-azperflibexe"></a>Onay azperflib.exe çıktısı

Tüm Azure performans sayaçları için SAP doldurulmuş Azperflib.exe çıktısını gösterir. Toplanan sayaçlarının listesi sonunda bir özeti ve sistem durumu göstergesi Azure izleme durumunu gösterir.

![Sistem durumu denetimi, herhangi bir sorun mevcut olduğunu gösteren azperflib.exe yürüterek çıktısı][deployment-guide-figure-1100]
<a name="figure-11"></a>

İçin döndürülen sonuç denetleme **sayaçları toplam** boş olarak ve için bildirilen çıktı **sistem durumu**, önceki şekilde gösterildiği.

Sonuçta elde edilen değerleri şu şekilde yorumlayabilir:

| Azperflib.exe sonuç değerleri | Azure sistem durumu izleme |
| --- | --- |
| **API çağrıları - kullanılamıyor** | Kullanılabilir sayaçları ya olabilir sanal makine yapılandırması, geçerli veya hatalar. Bkz: **sistem durumu**. |
| **Toplam - boş sayaçları** |Aşağıdaki iki Azure depolama sayaçları boş olabilir: <ul><li>Depolama okuma işlem gecikme süresi sunucu milisaniye</li><li>Depolama okuma işlem gecikme süresi E2E milisaniye</li></ul>Diğer tüm sayaçlar değerlere sahip olmalıdır. |
| **Sistem durumu** |Yalnızca Tamam if dönüş durumu gösterir **Tamam**. |
| **Tanılama** |Sistem durumu hakkında ayrıntılı bilgiler. |

Varsa **sistem durumu** değeri değil **Tamam**, yönergeleri [sistem durumu denetimi için Azure izleme altyapı yapılandırmasını] [ deployment-guide-5.2].

#### <a name="run-the-readiness-check-on-a-linux-vm"></a>Bir Linux VM üzerinde hazırlık denetimi Çalıştır

1. Azure sanal makine için SSH kullanarak bağlanın.

1. Azure Gelişmiş izleme uzantısını çıktısını denetleyin.

   a.  `more /var/lib/AzureEnhancedMonitor/PerfCounters` öğesini çalıştırın

   **Beklenen sonuç**: Performans sayaçları listesi döndürür. Dosya boş olmamalıdır.

   b. `cat /var/lib/AzureEnhancedMonitor/PerfCounters | grep Error` öğesini çalıştırın

   **Beklenen sonuç**: Hatanın bulunduğu bir satır döndürür **hiçbiri**, örneğin, **3; yapılandırma; Hata; 0; 0; yok; 0 1456416792; tst servercs;**

   c. `more /var/lib/AzureEnhancedMonitor/LatestErrorRecord` öğesini çalıştırın

   **Beklenen sonuç**: Boş olarak döndürür veya yok.

Önceki denetimi başarılı olmadıysa, bu ek denetimler çalıştırın:

1. Waagent yüklü ve etkin olduğundan emin olun.

   a.  `sudo ls -al /var/lib/waagent/` öğesini çalıştırın

     **Beklenen sonuç**: Waagent dizinin içeriğini listeler.

   b.  `ps -ax | grep waagent` öğesini çalıştırın

   **Beklenen sonuç**: Benzer bir giriş görüntüler: `python /usr/sbin/waagent -daemon`

1. Azure Gelişmiş izleme uzantısı yüklü ve çalışıyor olduğundan emin olun.

   a.  `sudo sh -c 'ls -al /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-*/'` öğesini çalıştırın

   **Beklenen sonuç**: Azure Gelişmiş izleme uzantısını dizinin içeriğini listeler.

   b. `ps -ax | grep AzureEnhanced` öğesini çalıştırın

   **Beklenen sonuç**: Benzer bir giriş görüntüler: `python /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-2.0.0.2/handler.py daemon`

1. SAP Not açıklandığı gibi SAP konak Aracısı yükleme [1031096]ve çıktısını kontrol `saposcol`.

   a.  `/usr/sap/hostctrl/exe/saposcol -d` öğesini çalıştırın

   b.  `dump ccm` öğesini çalıştırın

   c.  Denetleme olmadığını **Virtualization_Configuration\Enhanced erişim izleme** ölçümü **true**.

Yüklü bir SAP NetWeaver ABAP uygulama sunucusu zaten varsa, işlem ST06 açın ve Gelişmiş izleme etkin olup olmadığını denetleyin.

Tüm bu denetimlerin başarısız olursa, ve uzantıyı yeniden dağıtma hakkında ayrıntılı bilgi için bkz. [SAP için Azure izleme altyapısının sorun giderme][deployment-guide-5.3].

### <a name="e2d592ff-b4ea-4a53-a91a-e5521edb6cd1"></a>Sistem durumu için Azure izleme altyapı yapılandırmasını denetleyin

Bazı izleme, veri açıklanan test tarafından belirtildiği şekilde doğru şekilde teslim edilemedi [Azure SAP Gelişmiş izleme için hazır olma denetimi][deployment-guide-5.1]çalıştırın `Test-AzVMAEMExtension` cmdlet'ini olmadığını Azure için SAP altyapı ve izleme uzantısı izleme doğru şekilde yapılandırılır.

1. Açıklandığı gibi Azure PowerShell cmdlet en son sürümünü yüklediğinizden emin olun [dağıtma, Azure PowerShell cmdlet'lerini][deployment-guide-4.1].
1. Aşağıdaki PowerShell cmdlet’ini çalıştırın. Kullanılabilir ortamların listesi için cmdlet'i çalıştırın `Get-AzEnvironment`. Küresel Azure kullanmayı tercih **AzureCloud** ortamı. Çin'de Azure için seçin **AzureChinaCloud**.
   ```powershell
   $env = Get-AzEnvironment -Name <name of the environment>
   Connect-AzAccount -Environment $env
   Set-AzContext -SubscriptionName <subscription name>
   Test-AzVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
   ```

1. Hesap verilerinizi girin ve Azure sanal makine belirleyin.

   ![SAP özgü giriş sayfasında Azure cmdlet Test-VMConfigForSAP_GUI][deployment-guide-figure-1200]

1. Betik, seçtiğiniz sanal makine yapılandırmasını test eder.

   ![SAP için Azure izleme altyapısının başarılı test çıkışı][deployment-guide-figure-1300]

Her sistem durumu denetimi sonucu olduğundan emin olun **Tamam**. Bazı denetimleri görüntüleme, **Tamam**, açıklanan şekilde güncelleştirmeyi cmdlet'i çalıştırın [Azure Gelişmiş izleme uzantısını SAP için yapılandırmak][deployment-guide-4.5]. 15 dakika bekleyin ve açıklanan denetimleri [Azure SAP Gelişmiş izleme için hazır olma denetimi] [ deployment-guide-5.1] ve [Azure izleme altyapısı yapılandırmasınıiçinsistemdurumudenetimi] [deployment-guide-5.2]. Denetimler hala bazı veya tüm sayaçları ile ilgili bir sorun gösteriyorsa, bkz. [SAP için Azure izleme altyapısının sorun giderme][deployment-guide-5.3].

> [!Note]
> Bazı uyarılar oluştu, standart Azure diskler yönetilen kullandığınız durumlarda oluşabilir. "Tamam" döndüren testleri yerine uyarılar görüntülenir. Normal ve amaçlanan durumunda, disk türü budur. Ayrıca bkz: bkz [SAP için Azure izleme altyapı sorunlarını giderme][deployment-guide-5.3]
> 

### <a name="fe25a7da-4e4e-4388-8907-8abc2d33cfd8"></a>SAP için Azure izleme altyapı sorunlarını giderme

#### <a name="windowslogowindows-azure-performance-counters-do-not-show-up-at-all"></a>![Windows][Logo_Windows] Azure performans sayaçları hiç gösterilmez

AzureEnhancedMonitoring Windows hizmeti, azure'da performans ölçümleri toplar. Hizmet doğru şekilde yüklenmedi veya, sanal çalışmıyorsa, performans ölçüm toplanabilir.

##### <a name="the-installation-directory-of-the-azure-enhanced-monitoring-extension-is-empty"></a>Azure Gelişmiş izleme uzantısını yükleme dizinini boştur

###### <a name="issue"></a>Sorun

Yükleme dizini C:\\paketleri\\eklentileri\\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;sürüm >\\bırakma boştur.

###### <a name="solution"></a>Çözüm

Uzantısı yüklü değil. (Daha önce açıklandığı gibi) bu Ara sunucu sorunu olup olmadığını belirler. Makineyi yeniden başlatın veya yeniden çalıştırmak ihtiyacınız olabilecek `Set-AzVMAEMExtension` yapılandırma betiği.

##### <a name="service-for-azure-enhanced-monitoring-does-not-exist"></a>Azure Gelişmiş izleme için hizmet kayıtlı değil

###### <a name="issue"></a>Sorun

AzureEnhancedMonitoring Windows hizmet yok.

Azperflib.exe çıkış bir hata oluşturur:

![SAP için Azure Gelişmiş izleme uzantısını hizmet çalışmıyor azperflib.exe yürütülmesini gösterir.][deployment-guide-figure-1400]
<a name="figure-14"></a>

###### <a name="solution"></a>Çözüm

Azure Gelişmiş izleme uzantısını SAP için hizmetin mevcut değilse, doğru şekilde yüklenmedi. Dağıtım senaryonuz için açıklanan adımları kullanarak uzantıyı yeniden [dağıtım senaryoları için azure'da SAP sanal makinelerinin][deployment-guide-3].

Bir saat sonra uzantı dağıttıktan sonra yeniden Azure performans sayaçlarını Azure VM ile sağlanıp sağlanmadığını denetleyin.

##### <a name="service-for-azure-enhanced-monitoring-exists-but-fails-to-start"></a>Azure Gelişmiş izleme hizmeti var, ancak başlatılamıyor

###### <a name="issue"></a>Sorun

AzureEnhancedMonitoring Windows hizmeti var ve etkinleştirilir, ancak başlatılamıyor. Daha fazla bilgi için uygulama olay günlüğünü denetleyin.

###### <a name="solution"></a>Çözüm

Yapılandırması doğru değil. İzleme uzantısı açıklandığı gibi sanal makine için yeniden [Azure Gelişmiş izleme uzantısını SAP için yapılandırmak][deployment-guide-4.5].

#### <a name="windowslogowindows-some-azure-performance-counters-are-missing"></a>![Windows][Logo_Windows] Bazı Azure performans sayaçları kayboluyor

AzureEnhancedMonitoring Windows hizmeti, azure'da performans ölçümleri toplar. Hizmet, çeşitli kaynaklardan veri alır. Bazı yapılandırma verilerini yerel olarak toplanır ve bazı performans ölçümlerini Azure Tanılama'ya okunur. Depolama sayaçları, oturum depolama abonelik düzeyinde kullanılır.

SAP notu kullanarak sorun giderme olursa [1999351] değil, sorunu yeniden `Set-AzVMAEMExtension` yapılandırma betiği. Hemen etkinleştirdikten sonra depolama analizi veya tanılama sayaçları oluşturulamaz çünkü bir saat beklemeniz gerekebilir. Sorun devam ederse, SAP Müşteri Destek iletisine için bileşen OP NT AZR BC Windows veya BC-işlem-LNX-AZR Linux sanal makinesi için açın.

#### <a name="linuxlogolinux-azure-performance-counters-do-not-show-up-at-all"></a>![Linux][Logo_Linux] Azure performans sayaçları hiç gösterilmez

Azure'da performans ölçümleri, bir arka plan tarafından toplanır. Arka plan programı çalışmıyorsa, performans ölçüm toplanabilir.

##### <a name="the-installation-directory-of-the-azure-enhanced-monitoring-extension-is-empty"></a>Azure Gelişmiş izleme uzantısını yükleme dizinini boştur

###### <a name="issue"></a>Sorun

Dizin \\var\\LIB\\waagent\\ Azure Gelişmiş izleme uzantısı için bir alt dizin yok.

###### <a name="solution"></a>Çözüm

Uzantısı yüklü değil. (Daha önce açıklandığı gibi) bu Ara sunucu sorunu olup olmadığını belirler. Makineyi yeniden başlatın ve/veya yeniden çalıştırmak ihtiyacınız olabilecek `Set-AzVMAEMExtension` yapılandırma betiği.

##### <a name="the-execution-of-set-azvmaemextension-and-test-azvmaemextension-show-warning-messages-stating-that-standard-managed-disks-are-not-supported"></a>Set-AzVMAEMExtension ve Test AzVMAEMExtension yürütülmesini standart yönetilen diskler desteklenmediğini belirten bir uyarı iletilerini göster

###### <a name="issue"></a>Sorun

Ne zaman yürütülen kümesi AzVMAEMExtension veya Test AzVMAEMExtension iletileri bunlar gibi gösterilir:

<pre><code>
WARNING: [WARN] Standard Managed Disks are not supported. Extension will be installed but no disk metrics will be available.
WARNING: [WARN] Standard Managed Disks are not supported. Extension will be installed but no disk metrics will be available.
WARNING: [WARN] Standard Managed Disks are not supported. Extension will be installed but no disk metrics will be available.
</code></pre>

Daha önce açıklandığı gibi azperfli.exe yürütme iyi durumda olmayan bir durumu gösteren bir sonuç elde edebilirsiniz. 

###### <a name="solution"></a>Çözüm

İletileri, standart yönetilen diskler standart Azure depolama hesapları istatistiklerle denetlemek için izleme uzantısı tarafından kullanılan API'ler sunan değil olgu kaynaklanır. Bu, birkaç önemli değildir. Standart Disk Depolama hesapları için izleme ile tanışın nedeni sık oluşan g/ç azaltma. Yönetilen diskler, bu tür bir depolama hesabındaki diskleri sayısını sınırlayarak azaltma kaçınır. Bu nedenle, bu türü izleme verilerinin olmaması önemli değildir.


#### <a name="linuxlogolinux-some-azure-performance-counters-are-missing"></a>![Linux][Logo_Linux] Bazı Azure performans sayaçları kayboluyor

Azure'da performans ölçümleri, çeşitli kaynaklardan veri alır bir arka plan programı tarafından toplanır. Bazı yapılandırma verilerini yerel olarak toplanır ve bazı performans ölçümlerini Azure Tanılama'ya okunur. Depolama sayaçları, depolama aboneliğinizdeki günlüklerinden gelir.

Bilinen sorunların eksiksiz ve güncel listesi için bkz. Not SAP [1999351], SAP için Azure izleme Gelişmiş ek sorun giderme bilgileri içeriyor.

SAP notu kullanarak sorun giderme olursa [1999351] değil sorunu çözün, yeniden `Set-AzVMAEMExtension` açıklandığı gibi yapılandırma betiğini [Azure Gelişmiş izleme uzantısını SAP için yapılandırmak] [deployment-guide-4.5]. Hemen etkinleştirdikten sonra depolama analizi veya tanılama sayaçları oluşturulamaz çünkü bir saat beklemeniz gerekebilir. Sorun devam ederse, SAP Müşteri Destek iletisine için bileşen OP NT AZR BC Windows veya BC-işlem-LNX-AZR Linux sanal makinesi için açın.
