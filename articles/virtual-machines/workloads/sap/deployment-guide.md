---
title: SAP NetWeaver için Azure sanal makineler dağıtımı | Microsoft Docs
description: Linux sanal makineleri Azure üzerinde SAP yazılım dağıtmak öğrenin.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: MSSedusch
manager: timlt
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 1c4f1951-3613-4a5a-a0af-36b85750c84e
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/08/2016
ms.author: sedusch
ms.openlocfilehash: 2b141c96ad3f71cf35ddfbd08ce1eff14e36d8d0
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="azure-virtual-machines-deployment-for-sap-netweaver"></a>SAP NetWeaver için Azure sanal makineler dağıtımı
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
[azure-portal]:https://portal.azure.com
[azure-ps]:/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md#subscription-limits

[dbms-guide]:dbms-guide.md (SAP için Azure sanal makineleri DBMS dağıtımı)
[dbms-guide-2.1]:dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (Sanal makineleri ve VHD için önbelleğe alma)
[dbms-guide-2.2]:dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (Yazılım RAID)
[dbms-guide-2.3]:dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (Microsoft Azure depolama)
[dbms-guide-2]:dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (RDBMS dağıtım yapısı)
[dbms-guide-3]:dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (Azure VM'ler ile yüksek kullanılabilirlik ve olağanüstü durum kurtarma)
[dbms-guide-5.5.1]:dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 (SQL Server 2012 SP1 CU4 ve sonraki sürümler)
[dbms-guide-5.5.2]:dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3 ve önceki sürümleri)
[dbms-guide-5.6]:dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (Azure Marketi'nde bir SQL Server görüntüsü kullanma)
[dbms-guide-5.8]:dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (SAP Azure özeti için genel SQL Server)
[dbms-guide-5]:dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (SQL Server RDBMS özellikleri)
[dbms-guide-8.4.1]:dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (Depolama yapılandırması)
[dbms-guide-8.4.2]:dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d (Yedekleme ve geri yükleme)
[dbms-guide-8.4.3]:dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (Yedekleme ve geri yükleme için başarım düşünceleri)
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
[deployment-guide-3.1.2]:deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (Özel görüntü kullanarak bir VM dağıtma)
[deployment-guide-3.2]:deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (Senaryo 1: Azure Marketi'nden bir VM için SAP dağıtma)
[deployment-guide-3.3]:deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (2. Senaryo: özel bir görüntü ile bir VM için SAP dağıtma)
[deployment-guide-3.4]:deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 (Senaryo 3: şirket içi SAP ile genelleştirilmiş Azure VHD kullanarak bir VM taşıma)
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (VM'ler için Microsoft Azure üzerinde SAP dağıtım senaryoları)
[deployment-guide-4.1]:deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (Azure PowerShell cmdlet'lerini dağıtma)
[deployment-guide-4.2]:deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (Karşıdan yükle ve SAP ilgili PowerShell cmdlet'lerini alma)
[deployment-guide-4.3]:deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (VM bir şirket içi etki alanına - yalnızca Windows ekleyin)
[deployment-guide-4.4.2]:deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux)
[deployment-guide-4.4]:deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d (İndirme, yükleme ve Azure VM Aracısı'nı etkinleştir)
[deployment-guide-4.5.1]:deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure PowerShell)
[deployment-guide-4.5.2]:deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure CLI)
[deployment-guide-4.5]:deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (SAP için Azure Gelişmiş izleme uzantısı yapılandırma)
[deployment-guide-5.1]:deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (Gelişmiş Azure Monitoring SAP için hazır olma durumunu denetle)
[deployment-guide-5.2]:deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (Sistem durumu denetimi için Azure izleme altyapı)
[deployment-guide-5.3]:deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (SAP için izleme Azure sorun giderme)

[deployment-guide-configure-monitoring-scenario-1]:deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b (İzlemeyi Yapılandır)
[deployment-guide-configure-proxy]:deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d (Proxy ayarlarını yapılandır)
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
[deployment-guide-troubleshooting-chapter]:deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b (Denetim ve uçtan uca izleme ayarlamak için sorun giderme)

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

[planning-guide]:planning-guide.md (Azure sanal makineleri planlama ve uygulama SAP için)
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (Kaynakları)
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (Azure sanal makinelerde çalışan SAP NetWeaver için yüksek kullanılabilirlik ve olağanüstü durum kurtarma)
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (SAP uygulama sunucuları için yüksek kullanılabilirlik)
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (SAP örnekleri için Otomatik Başlat'ı kullanma)
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803 (Yalnızca bulut - şirket içi müşteri ağdaki bağımlılıklar olmadan Azure sanal makine dağıtımları)
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (Şirket içi - dağıtımı tek veya birden çok SAP VM'ler için Azure'da şirket içi ağ ile tam olarak tümleşiktir.)
[planning-guide-3.1]:planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure bölgeleri)
[planning-guide-3.2.1]:planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (Hata etki alanları)
[planning-guide-3.2.2]:planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (Yükseltme etki alanları)
[planning-guide-3.2.3]:planning-guide.md#18810088-f9be-4c97-958a-27996255c665 (Azure kullanılabilirlik kümeleri)
[planning-guide-3.2]:planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (Microsoft Azure sanal makineleri kavramı)
[planning-guide-3.3.2]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium depolama)
[planning-guide-5.1.1]:planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (Bir VM şirket içinden Azure'a genelleştirilmiş olmayan bir diskle taşıma)
[planning-guide-5.1.2]:planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (Bir müşteri belirli görüntüsü olan bir VM dağıtma)
[planning-guide-5.2.1]:planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (Bir VM şirket içinden Azure'a genelleştirilmiş olmayan bir diskle taşıma için hazırlama)
[planning-guide-5.2.2]:planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (SAP için belirli bir müşteri resim'yle bir VM'yi dağıtmak için hazırlama)
[planning-guide-5.2]:planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (Azure için sanal makineleri SAP ile hazırlama)
[planning-guide-5.3.1]:planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (Azure disk ve Azure görüntünün arasındaki farkı)
[planning-guide-5.3.2]:planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (Şirket içi bir VHD'den Azure karşıya yükleme)
[planning-guide-5.4.2]:planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (Azure depolama hesapları arasında diskleri kopyalama)
[planning-guide-5.5.1]:planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646 (SAP dağıtımları için VM/VHD yapısı)
[planning-guide-5.5.3]:planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (Otomatik bağlama bağlı diskler için ayarlama)
[planning-guide-7.1]:planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (SAP demo/senaryo eğitimi NetWeaver tek VM)
[planning-guide-7]:planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (SAP örneklerinin yalnızca bulut dağıtımının kavramları)
[planning-guide-9.1]:planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure için SAP izleme çözümü)
[planning-guide-azure-premium-storage]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium depolama)
[planning-guide-managed-disks]:planning-guide.md#c55b2c6e-3ca1-4476-be16-16c81927550f (Yönetilen diskleri)
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
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure ağ iletişimi)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Depolama: Microsoft Azure depolama ve veri diskleri)

[powershell-install-configure]:https://docs.microsoft.com/powershell/azure/install-azurerm-ps
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
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md (Dağıtma ve Azure Resource Manager şablonları ve Azure CLI kullanarak sanal makineleri yönetme)
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md (Azure Resource Manager ve PowerShell kullanarak sanal makineleri yönetme)
[virtual-machines-windows-agent-user-guide]:../../windows/agent-user-guide.md
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

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Azure sanal makinelerin olası en kısa süre içinde ve uzun tedarik döngüleri olmadan işlem ve depolama kaynaklarını gereken kuruluşlar için çözüm şeklindedir. Azure sanal makineleri azure'da SAP NetWeaver tabanlı uygulamalar gibi Klasik uygulamaları dağıtmak için kullanabilirsiniz. Bir uygulamanın güvenilirlik ve kullanılabilirlik ek şirket içi kaynaklar olmadan genişletir. Azure sanal makineleri şirket içi bağlantılar, Azure sanal makineler, kuruluşunuzun şirket içi etki alanları, özel Bulutlar ve SAP sistem yatay tümleştirebilir şekilde destekler.

Bu makalede, sorun giderme ve diğer dağıtım seçenekleri dahil olmak üzere Azure sanal makinelerle (VM'ler) SAP uygulamaları dağıtma adımları kapsar. Bu makalede bilgileri derlemeler [Azure sanal makineleri planlama ve uygulama SAP NetWeaver için][planning-guide]. Aynı zamanda, SAP yükleme belgelerini ve yükleme ve SAP yazılım dağıtma birincil kaynaklar SAP Notlar tamamlar.

## <a name="prerequisites"></a>Önkoşullar
SAP yazılım dağıtımı için Azure sanal makinesi ayarlanıyor birden çok adımlar ve kaynaklar içerir. Başlamadan önce Azure sanal makinelerde SAP yazılım yüklemeye yönelik önkoşulları karşıladığından emin olun.

### <a name="local-computer"></a>Yerel bilgisayar
Windows veya Linux sanal makineleri yönetmek için bir PowerShell Betiği ve Azure portalını kullanabilirsiniz. Her iki Araçlar, Windows 7 veya sonraki bir Windows sürümü çalıştıran yerel bir bilgisayar gerekir. Yalnızca Linux sanal makineleri yönetmek istediğiniz ve bu görev için bir Linux bilgisayar kullanmak istiyorsanız, Azure CLI kullanabilirsiniz.

### <a name="internet-connection"></a>Internet bağlantısı
Karşıdan yükle ve SAP yazılım dağıtımı için gerekli olan komut dosyaları ve araçları çalıştırmak için Internet'e bağlı olmalıdır. Azure Gelişmiş izleme uzantısı SAP çalışan Azure VM ayrıca Internet erişimi gerekir. Bölümünde açıklandığı gibi Azure VM'de bir Azure sanal ağı veya şirket içi etki alanının parçasıysa, ilgili proxy ayarlarının belirlendiğini emin olun [proxy yapılandırma][deployment-guide-configure-proxy].

### <a name="microsoft-azure-subscription"></a>Microsoft Azure aboneliği
Etkin bir Azure hesabı gerekir.

### <a name="topology-and-networking"></a>Topoloji ve ağ iletişimi
Topoloji ve SAP dağıtımının mimarisini Azure'da tanımlamanız gerekir:

* Kullanılacak azure depolama hesapları
* SAP sistem dağıtmak istediğiniz sanal ağ
* SAP sistem dağıtmak istediğiniz kaynak grubu
* SAP sistem dağıtmak istediğiniz azure bölgesi
* SAP yapılandırma (iki katmanlı veya üç katmanlı)
* VM boyutları ve VM'ler için takılması ek veri diski sayısı
* SAP düzeltme ve aktarım sistem (CTS) yapılandırma

Oluşturun ve SAP yazılım dağıtım işlemine başlamadan önce (gerekliyse) Azure depolama hesapları veya Azure sanal ağını yapılandırın. Oluşturun ve bu kaynakları yapılandırma hakkında daha fazla bilgi için bkz: [Azure sanal makineleri planlama ve uygulama SAP NetWeaver için][planning-guide].

### <a name="sap-sizing"></a>SAP boyutlandırma
SAP boyutlandırma için aşağıdaki bilgileri bildirin:

* SAP hızlı Boyutlandırıcı aracı ve SAP uygulama performans standart (SAP) kullanarak tahmini SAP iş yükü, örneğin, numara
* SAP sisteminin gerekli CPU ve bellek kaynak tüketimi
* Saniye başına gerekli giriş/çıkış (g/ç) işlemleri
* Azure VM'ler arasında nihai iletişimin ağ bant genişliği gerekli
* Şirket içi varlıklar ve Azure tarafından dağıtılan SAP sistemi arasındaki gerekli ağ bant genişliği

### <a name="resource-groups"></a>Kaynak grupları
Azure Kaynak Yöneticisi'nde, Azure aboneliğinizde tüm uygulama kaynakları yönetmek için kaynak gruplarını kullanabilirsiniz. Daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış][resource-group-overview].

## <a name="resources"></a>Kaynaklar

### <a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>SAP kaynakları
SAP yazılım dağıtımınızı ayarlarken, aşağıdaki SAP kaynaklara gerekir:

* SAP Not [1928533], sahip olduğu:
  * SAP yazılım dağıtımı için desteklenen Azure VM boyutlarını listesi
  * Azure VM boyutlarını önemli kapasite bilgilerini
  * Desteklenen bir SAP yazılım ve işletim sistemi (OS) ve veritabanı birleşimler
  * Windows ve Microsoft Azure Linux için gerekli SAP çekirdek sürümü

* SAP Not [2015553] azure'da SAP desteklenen SAP yazılım dağıtımları için önkoşulları listeler.
* SAP Not [2178632] ayrıntılı Azure SAP için bildirilen tüm izleme ölçümleri hakkında bilgi sağlanmıştır.
* SAP Not [1409604] Windows Azure için gerekli SAP konak Aracısı sürümü vardır.
* SAP Not [2191498] Azure Linux için gerekli SAP konak Aracısı sürümü vardır.
* SAP Not [2243692] Linux Azure üzerinde SAP lisanslama hakkında bilgi vardır.
* SAP Not [1984787] SUSE Linux Enterprise Server 12 hakkında genel bilgi yer almaktadır.
* SAP Not [2002167] Red Hat Enterprise Linux ilgili genel bilgilerin 7.x.
* SAP Not [2069760] Oracle Linux ilgili genel bilgilerin 7.x.
* SAP Not [1999351] Azure Gelişmiş izleme uzantısı SAP için ek sorun giderme bilgileri içeriyor.
* SAP Not [1597355] Linux takas alanı hakkında genel bilgi.
* [SAP Azure SCN sayfasında](https://wiki.scn.sap.com/wiki/x/Pia7Gg) Haberler ve kullanışlı kaynaklar koleksiyonu vardır.
* [SAP topluluk WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) SAP notları Linux için gerekli tüm sahiptir.
* Parçası olan SAP özgü PowerShell cmdlet'leri [Azure PowerShell][azure-ps].
* Parçası olan SAP özgü Azure CLI komutları [Azure CLI][azure-cli].

### <a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>Windows kaynakları
Bu Microsoft makaleler Azure SAP dağıtımlarda kapsar:

* [Azure sanal makineleri planlama ve uygulama SAP NetWeaver için][planning-guide]
* [SAP NetWeaver (Bu makalede) için Azure sanal makineler dağıtımı][deployment-guide]
* [SAP NetWeaver için Azure sanal makineleri DBMS dağıtımı][dbms-guide]

## <a name="b3253ee3-d63b-4d74-a49b-185e76c4088e"></a>Azure vm'lerinde SAP yazılım için dağıtım senaryoları
Sanal makineleri ve ilişkili diskler azure'da dağıtmak için birçok seçeneğiniz vardır. Vm'leriniz seçtiğiniz dağıtım türüne göre dağıtımına hazırlamak için farklı adımlar sürebilir çünkü dağıtım seçenekleri arasındaki farkları anlamak önemlidir.

### <a name="db477013-9060-4602-9ad4-b0316f8bb281"></a>Senaryo 1: Azure Marketi'nden bir VM için SAP dağıtma
VM dağıtmak için Microsoft veya üçüncü taraf Azure markette tarafından sağlanan bir görüntüyü kullanabilirsiniz. Market bazı standart bir işletim sistemi görüntüleri Windows Server ve farklı Linux dağıtımları sunar. Ayrıca, veritabanı yönetimi içeren bir görüntü dağıtabilirsiniz sistemi (DBMS) SKU'ları, örneğin, Microsoft SQL Server. Görüntüleri DBMS SKU'ları ile kullanma hakkında daha fazla bilgi için bkz: [SAP NetWeaver için Azure sanal makineleri DBMS dağıtım][dbms-guide].

Aşağıdaki akış çizelgesi Azure Marketi'nden VM dağıtma adımları SAP özgü sırasını göstermektedir:

![VM dağıtımı Azure Market'teki bir VM görüntüsü kullanarak SAP sistemleri için akış çizelgesi][deployment-guide-figure-100]

#### <a name="create-a-virtual-machine-by-using-the-azure-portal"></a>Azure portalı kullanarak bir sanal makine oluşturun
Azure Marketi'nden bir görüntü içeren yeni bir sanal makine oluşturmak için en kolay yolu, Azure Portalı'nı kullanmaktır.

1.  <https://portal.azure.com/#create/hub> kısmına gidin.  Veya, Azure portal menüsünde seçin **+ yeni**.
2.  Seçin **işlem**ve ardından dağıtmak istediğiniz işletim sisteminin türünü seçin. Örneğin, Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12), Red Hat Enterprise Linux 7.2 (RHEL 7.2) veya Oracle Linux 7.2. Varsayılan liste görünümü, tüm desteklenen işletim sistemlerini göstermez. Seçin **tümünü görmek** tam listesi için. SAP yazılım dağıtımı için desteklenen işletim sistemleri hakkında daha fazla bilgi için bkz. SAP Not [1928533].
3.  Sonraki sayfada, hüküm ve koşulları gözden geçirin.
4.  İçinde **dağıtım modeli seçin** kutusunda, seçin **Resource Manager**.
5.  **Oluştur**’u seçin.

Sihirbaz, ağ arabirimleri ve depolama hesapları gibi tüm gerekli kaynaklara ek olarak sanal makine oluşturmak için gerekli parametreleri size yol gösterir. Bu parametreler bazıları şunlardır:

1. **Temel kavramları**:
 * **Ad**: (sanal makine adı) kaynağın adı.
 * **VM disk türü**: işletim sistemi diski, disk türünü seçin. Premium depolama, veri diskleri için kullanmak istiyorsanız, Premium depolama da işletim sistemi diski için kullanmanızı öneririz.
 * **Kullanıcı adı ve parola** veya **SSH ortak anahtarını**: kullanıcı adı ve sağlama işlemi sırasında oluşturulan kullanıcı parolasını girin. Bir Linux sanal makine için makinede oturum açmak için kullandığınız genel güvenli Kabuk (SSH) anahtarını girebilirsiniz.
 * **Abonelik**: yeni bir sanal makine sağlamak için kullanmak istediğiniz aboneliği seçin.
 * **Kaynak grubu**: VM kaynak grubunun adı. Yeni bir kaynak grubu adı veya zaten mevcut bir kaynak grubu adını girebilirsiniz.
 * **Konum**: yeni bir sanal makine dağıtmak nereye. Sanal makine, şirket içi ağınıza bağlanmak istiyorsanız, Azure, şirket içi ağınıza bağlanan sanal ağ konumu seçtiğinizden emin olun. Daha fazla bilgi için bkz: [Microsoft Azure ağ] [ planning-guide-microsoft-azure-networking] içinde [Azure sanal makineleri planlama ve uygulama SAP NetWeaver için][planning-guide].
2. **Boyutu**:

     Desteklenen VM türlerinin listesi için bkz. SAP Not [1928533]. Azure Premium Storage kullanmak istiyorsanız, doğru VM türü seçtiğinizden emin olun. Premium depolama tüm VM türleri desteklemez. Daha fazla bilgi için bkz: [Depolama: Microsoft Azure depolama ve veri diskleri] [ planning-guide-storage-microsoft-azure-storage-and-data-disks] ve [Azure Premium Storage] [ planning-guide-azure-premium-storage] içinde [Azure sanal makineleri planlama ve uygulama SAP NetWeaver için][planning-guide].

3. **Ayarları**:
  * **Depolama**
    * **Disk türü**: işletim sistemi diski, disk türünü seçin. Premium depolama, veri diskleri için kullanmak istiyorsanız, Premium depolama da işletim sistemi diski için kullanmanızı öneririz.
    * **Yönetilen diskler kullanan**: yönetilen diskleri kullanmak istiyorsanız, Evet'i seçin. Bölüm yönetilen diskler hakkında daha fazla bilgi için bkz: [yönetilen diskleri] [ planning-guide-managed-disks] Planlama Kılavuzu'nda.
    * **Depolama hesabı**: mevcut bir depolama hesabını seçin veya yeni bir tane oluşturun. Tüm depolama türlerini, SAP uygulamaları çalıştırmak için çalışır. Depolama türleri hakkında daha fazla bilgi için bkz: [Microsoft Azure depolama] [ dbms-guide-2.3] içinde [SAP NetWeaver için Azure sanal makineleri DBMS dağıtım][dbms-guide].
  * **Ağ**
    * **Sanal ağ** ve **alt**: sanal makine intranetinize ile tümleştirmek için şirket içi ağınıza bağlı sanal ağ seçin.
    * **Genel IP adresi**: kullanmak istediğiniz genel IP adresi seçin veya yeni bir ortak IP adresi oluşturma için parametre sayısı girin. Bir ortak IP adresi, sanal makinenize Internet üzerinden erişmek için kullanabilirsiniz. Ayrıca, sanal makine için güvenli erişim yardımcı olacak bir ağ güvenlik grubu oluşturduğunuzdan emin olun.
    * **Ağ güvenlik grubu**: daha fazla bilgi için bkz: [kontrol ağ trafiği akışını ağ güvenlik gruplarıyla][virtual-networks-nsg].
  * **Uzantıları**: dağıtımına ekleyerek sanal makine uzantıları yükleyebilirsiniz. Bu adımda uzantıları eklemeniz gerekmez. SAP desteği için gerekli uzantıları daha sonra yüklenir. Bölüm bkz [Azure Gelişmiş izleme uzantısı SAP yapılandırma] [ deployment-guide-4.5] bu kılavuzdaki.
  * **Yüksek kullanılabilirlik**: bir kullanılabilirlik kümesi seçin veya yeni bir kullanılabilirlik kümesi oluşturmak için parametreleri girin. Daha fazla bilgi için bkz: [Azure kullanılabilirlik kümeleri][planning-guide-3.2.3].
  * **İzleme**
    * **Önyükleme tanılama**: seçebileceğiniz **devre dışı** önyükleme tanılaması için.
    * **Konuk işletim sistemi tanılama**: seçebileceğiniz **devre dışı** Tanılama izleme.

4. **Özet**:

  Seçimlerinizi gözden geçirin ve ardından **Tamam**.

Sanal makineniz, seçtiğiniz kaynak grubunda dağıtılır.

#### <a name="create-a-virtual-machine-by-using-a-template"></a>Bir şablonu kullanarak bir sanal makine oluşturma
Yayımlanacak SAP şablonlarından birini kullanarak bir sanal makine oluşturabilirsiniz [azure hızlı başlangıç şablonlarını GitHub deposunu][azure-quickstart-templates-github]. Ayrıca el ile bir sanal makine kullanarak oluşturabileceğiniz [Azure portal][virtual-machines-windows-tutorial], [PowerShell][virtual-machines-ps-create-preconfigure-windows-resource-manager-vms], veya [Azure CLI][virtual-machines-linux-tutorial].

* [**İki katmanlı yapılandırma (yalnızca bir sanal makine) şablonu** (sap-2-katman-Market-görüntü)][sap-templates-2-tier-marketplace-image]

  İki katmanlı sistemi yalnızca bir sanal makine kullanarak oluşturmak için bu şablonu kullanın.
* [**İki katmanlı yapılandırma (yalnızca bir sanal makine) şablonu - yönetilen diskleri** (sap-2-tier-marketplace-image-md)][sap-templates-2-tier-marketplace-image-md]

  Yalnızca bir sanal makine ve yönetilen diskleri kullanarak iki katmanlı sistemi oluşturmak için bu şablonu kullanın.
* [**Üç katmanlı yapılandırma (birden çok sanal makine) şablonu** (sap-3-katman-Market-görüntü)][sap-templates-3-tier-marketplace-image]

  Üç katmanlı sistem birden çok sanal makine kullanarak oluşturmak için bu şablonu kullanın.
* [**Üç katmanlı yapılandırma (birden çok sanal makine) şablonu - yönetilen diskleri** (sap-3-tier-marketplace-image-md)][sap-templates-3-tier-marketplace-image-md]

  Birden çok sanal makineler ve yönetilen diskleri kullanarak üç katmanlı sistem oluşturmak için bu şablonu kullanın.

Azure portalında şablon için aşağıdaki parametreleri girin:

1. **Temel kavramları**:
  * **Abonelik**: şablonu dağıtmak için kullanılacak aboneliği.
  * **Kaynak grubu**: şablonu dağıtmak için kullanılacak kaynak grubu. Yeni bir kaynak grubu oluşturabilir veya varolan bir kaynak grubu abonelikte seçebilirsiniz.
  * **Konum**: şablonu dağıtmak nereye. Varolan bir kaynak grubu seçtiyseniz, bu kaynak grubu konumu kullanılır.

2. **Ayarları**:
  * **SAP sistem kimliği**: SAP sistem kimliği (SID).
  * **İşletim sistemi türü**: Örneğin, dağıtmak istediğiniz işletim sistemi, Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12), Red Hat Enterprise Linux 7.2 (RHEL 7.2) veya Oracle Linux 7.2.

    Liste görünümü, tüm desteklenen işletim sistemlerini göstermez. SAP yazılım dağıtımı için desteklenen işletim sistemleri hakkında daha fazla bilgi için bkz. SAP Not [1928533].
  * **SAP sistem boyutu**: SAP sistem boyutu.

    Yeni sistem sağlar SAP sayısı. Sistem gerektirir kaç SAP değil eminseniz, SAP teknolojisi iş ortağı veya sistem Tümleştirici isteyin.
  * **Sistem kullanılabilirliğini** (yalnızca üç katmanlı şablon): Sistem kullanılabilirlik.

    Seçin **HA** yüksek kullanılabilirlik yüklemesi için uygun bir yapılandırma için. İki veritabanı sunucuları ve iki sunucu için ABAP SAP merkezi Hizmetleri (ASCS) oluşturulur.
  * **Depolama türü** (yalnızca iki katmanlı şablon): kullanmak için depolama türü.

    Daha büyük sistemler için yüksek oranda Azure Premium Storage kullanmanızı öneririz. Depolama türleri hakkında daha fazla bilgi için şu kaynaklara bakın:
      * [SAP DBMS örneği için Azure Premium SSD depolama kullanımı][2367194]
      * [Microsoft Azure depolama] [ dbms-guide-2.3] içinde [SAP NetWeaver için Azure sanal makineleri DBMS dağıtımı][dbms-guide]
      * [Premium Storage: Azure sanal makine iş yükleri için yüksek performanslı depolama][storage-premium-storage-preview-portal]
      * [Microsoft Azure Storage'a giriş][storage-introduction]
  * **Yönetici kullanıcı adı** ve **yönetici parolası**: bir kullanıcı adı ve parola.
    Yeni bir kullanıcı sanal makineye oturum açmak için oluşturulur.
  * **Yeni veya var olan bir alt ağ**: yeni bir sanal ağ ve alt ağ oluşturulur veya mevcut bir alt kullanılan belirler. Şirket içi ağınıza bağlı bir sanal ağ zaten varsa, seçin **varolan**.
  * **Alt ağ kimliği**: alt ağ kimliği sanal makinelerin bağlanması için. Sanal özel ağ (VPN) veya Azure ExpressRoute, şirket içi ağınıza sanal makineye bağlanmak için kullanılacak sanal ağ alt ağı seçin. Kimliği genellikle şöyle görünür: /subscriptions/&lt;abonelik kimliği > /resourceGroups/&lt;kaynak grubu adı > /providers/Microsoft.Network/virtualNetworks/&lt;sanal ağ adı > /subnets/&lt;alt ağ adı >

3. **Hüküm ve koşullar**:  
    Gözden geçirin ve yasal koşulları kabul edin.

4.  Seçin **satın alma**.

Azure Marketi'nde bir görüntü kullandığınızda Azure VM Aracısı varsayılan olarak dağıtılır.

#### <a name="configure-proxy-settings"></a>Proxy ayarlarını yapılandır
Şirket içi ağınıza nasıl yapılandırıldığına bağlı olarak, VM proxy ayarlamak gerekebilir. VM için bağlıysa, VPN veya ExpressRoute, VM aracılığıyla şirket içi ağınıza Internet erişimine sahip olmayabilir ve gerekli uzantıları indirmesi ve izleme verileri toplamak yükleyemezsiniz. Daha fazla bilgi için bkz: [proxy yapılandırma][deployment-guide-configure-proxy].

#### <a name="join-a-domain-windows-only"></a>(Yalnızca Windows) etki alanına Katıl
Azure dağıtımınız için bir şirket içi Active Directory veya DNS örneğine bir Azure siteden siteye VPN bağlantısı veya ExpressRoute aracılığıyla bağlıysa, (Bu adlı *şirketler arası* içinde [Azure sanal makineleri planlama ve uygulama SAP NetWeaver için][planning-guide]), VM bir şirket içi etki alanına katılma, beklenir. Bu görev için konuları hakkında daha fazla bilgi için bkz: [bir şirket içi etki alanına (yalnızca Windows) için bir VM katılmak][deployment-guide-4.3].

#### <a name="ec323ac3-1de9-4c3a-b770-4ff701def65b"></a>İzlemeyi Yapılandır
SAP açıklandığı gibi Azure Monitoring uzantısı SAP için ayarlamak, ortamınıza emin olmak için destekleyen [Azure Gelişmiş izleme uzantısı SAP yapılandırma][deployment-guide-4.5]. SAP izleme önkoşulları denetleyin ve SAP çekirdek ve SAP konak Aracısı, gerekli en düşük sürümlerle kaynaklarında listelenen [SAP kaynakları][deployment-guide-2.2].

#### <a name="monitoring-check"></a>Onay izleme
İzleme çalışıp çalışmadığını, açıklandığı gibi denetleyin [denetler ve uçtan uca izleme ayarlamak için sorun giderme][deployment-guide-troubleshooting-chapter].

#### <a name="post-deployment-steps"></a>Dağıtım sonrası adımlar
Sonra VM oluşturma ve VM dağıtılır, VM'yi gerekli yazılım bileşenleri yüklemeniz gerekir. Bu tür VM dağıtımı bir dağıtım/yazılım yükleme sırası nedeniyle yüklenecek yazılımı zaten ya da Azure, başka bir VM üzerinde veya kullanılabilir bir disk olarak kullanılabilir olmalıdır. Ya da şirket içi hangi bağlantılar varlıklar (yükleme paylaşımları) verilen bir şirket içi senaryo kullanmayı düşünün.

Azure'da VM dağıttıktan sonra aynı kılavuzları ve bir şirket içi ortamda olduğu gibi VM SAP yazılımını yüklemek için Araçlar izleyin. Azure VM temelinde SAP yazılım yüklemek için SAP ve Microsoft karşıya yükleme ve SAP yükleme medyasını Azure VHD'leri veya yönetilen diskleri depolamak veya bir Azure VM, oluşturduğunuz tüm gerekli SAP yükleme medyasında bulunan dosya sunucusu olarak çalışan önerilir.

### <a name="54a1fc6d-24fd-4feb-9c57-ac588a55dff2"></a>2. Senaryo: özel bir görüntü ile bir VM için SAP dağıtma
Bir işletim sistemi veya DBMS farklı sürümlerini farklı düzeltme eki gereksinimleri olduğundan, Azure Marketi'nde bulma görüntüleri ihtiyaçlarınızı karşılamayabilir. Bunun yerine, daha sonra yeniden dağıtabilirsiniz kendi işletim sistemi/DBMS VM görüntüsünü kullanarak bir VM oluşturmak isteyebilirsiniz.
Windows için bir tane oluşturmak için daha Linux için özel bir görüntü oluşturmak için farklı adımları kullanın.

- - -
> ![Windows][Logo_Windows] Windows
>
> Birden çok sanal makine dağıtmak için kullanabileceğiniz bir Windows görüntüsünü hazırlamak için Windows ayarlarını (örneğin, Windows SID ve ana bilgisayar adı) soyutlanır veya gerekir şirket içi VM genelleştirilmiş. Kullanabileceğiniz [sysprep](https://msdn.microsoft.com/library/hh825084.aspx) Bunu yapmak için.
>
> ![Linux][Logo_Linux] Linux
>
> Birden çok sanal makine dağıtmak için kullanabileceğiniz bir Linux görüntüsünü hazırlamak için bazı Linux ayarları soyutlanır veya gerekir şirket içi VM genelleştirilmiş. Kullanabileceğiniz `waagent -deprovision` Bunu yapmak için. Daha fazla bilgi için bkz: [Azure üzerinde çalışan Linux sanal makine yakalama] [ virtual-machines-linux-capture-image] ve [Azure Linux Aracısı Kullanıcı Kılavuzu'na][virtual-machines-linux-agent-user-guide-command-line-options].
>
>

- - -
Hazırlama ve özel bir görüntü oluşturun ve birden çok yeni VM oluşturmak için kullanın. Bu açıklanan [Azure sanal makineleri planlama ve uygulama SAP NetWeaver için][planning-guide]. Destekliyorsa, DBMS, veritabanı içeriğinizi SAP yazılım sağlama yeni SAP sistemi yüklemek için (bir veritabanı yedeği sanal makineye bağlı bir diskten geri yükler) Yöneticisi'ni kullanarak veya doğrudan Azure depolama biriminden bir veritabanı yedeklemesini geri yükleme ayarlayın. Daha fazla bilgi için bkz: [SAP NetWeaver için Azure sanal makineleri DBMS dağıtım][dbms-guide]. (Özellikle iki katmanlı sistemler için), şirket içi VM üzerinde bir SAP sistemi zaten yüklü değilse, SAP sistem ayarlarını Azure VM Dağıtım sonrasında sağlama SAP yazılım Yöneticisi tarafından desteklenen sistem yeniden adlandırmak yordamı kullanarak uyarlayabilirsiniz (SAP Not [1619720]). Aksi takdirde, Azure VM dağıttıktan sonra SAP yazılım yükleyebilirsiniz.

Aşağıdaki akış çizelgesi, bir VM'den özel bir görüntü dağıtmak için adımları SAP özgü sırasını göstermektedir:

![VM dağıtımı için bir VM görüntüsü özel Marketi'ndeki kullanarak SAP sistemleri akış çizelgesi][deployment-guide-figure-300]

#### <a name="create-a-virtual-machine-by-using-the-azure-portal"></a>Azure portalı kullanarak bir sanal makine oluşturun
Bir Disk yönetilen görüntüsünden yeni bir sanal makine oluşturmak için en kolay yolu, Azure Portalı'nı kullanmaktır. Bir yönetmek Disk görüntüsünün nasıl oluşturulacağı hakkında daha fazla bilgi için okuma [genelleştirilmiş VM Azure ile yönetilen bir görüntüsünü yakalama](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource)

1.  <https://ms.portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Compute%2Fimages> kısmına gidin. Veya, Azure portal menüsünde seçin **görüntüleri**.
2.  İstediğiniz dağıtıp tıklayın yönetilen Disk görüntüsü seçin **VM oluştur**

Sihirbaz, ağ arabirimleri ve depolama hesapları gibi tüm gerekli kaynaklara ek olarak sanal makine oluşturmak için gerekli parametreleri size yol gösterir. Bu parametreler bazıları şunlardır:

1. **Temel kavramları**:
 * **Ad**: (sanal makine adı) kaynağın adı.
 * **VM disk türü**: işletim sistemi diski, disk türünü seçin. Premium depolama, veri diskleri için kullanmak istiyorsanız, Premium depolama da işletim sistemi diski için kullanmanızı öneririz.
 * **Kullanıcı adı ve parola** veya **SSH ortak anahtarını**: kullanıcı adı ve sağlama işlemi sırasında oluşturulan kullanıcı parolasını girin. Bir Linux sanal makine için makinede oturum açmak için kullandığınız genel güvenli Kabuk (SSH) anahtarını girebilirsiniz.
 * **Abonelik**: yeni bir sanal makine sağlamak için kullanmak istediğiniz aboneliği seçin.
 * **Kaynak grubu**: VM kaynak grubunun adı. Yeni bir kaynak grubu adı veya zaten mevcut bir kaynak grubu adını girebilirsiniz.
 * **Konum**: yeni bir sanal makine dağıtmak nereye. Sanal makine, şirket içi ağınıza bağlanmak istiyorsanız, Azure, şirket içi ağınıza bağlanan sanal ağ konumu seçtiğinizden emin olun. Daha fazla bilgi için bkz: [Microsoft Azure ağ] [ planning-guide-microsoft-azure-networking] içinde [Azure sanal makineleri planlama ve uygulama SAP NetWeaver için][planning-guide].
2. **Boyutu**:

     Desteklenen VM türlerinin listesi için bkz. SAP Not [1928533]. Azure Premium Storage kullanmak istiyorsanız, doğru VM türü seçtiğinizden emin olun. Premium depolama tüm VM türleri desteklemez. Daha fazla bilgi için bkz: [Depolama: Microsoft Azure depolama ve veri diskleri] [ planning-guide-storage-microsoft-azure-storage-and-data-disks] ve [Azure Premium Storage] [ planning-guide-azure-premium-storage] içinde [Azure sanal makineleri planlama ve uygulama SAP NetWeaver için][planning-guide].

3. **Ayarları**:
  * **Depolama**
    * **Disk türü**: işletim sistemi diski, disk türünü seçin. Premium depolama, veri diskleri için kullanmak istiyorsanız, Premium depolama da işletim sistemi diski için kullanmanızı öneririz.
    * **Yönetilen diskler kullanan**: yönetilen diskleri kullanmak istiyorsanız, Evet'i seçin. Bölüm yönetilen diskler hakkında daha fazla bilgi için bkz: [yönetilen diskleri] [ planning-guide-managed-disks] Planlama Kılavuzu'nda.
  * **Ağ**
    * **Sanal ağ** ve **alt**: sanal makine intranetinize ile tümleştirmek için şirket içi ağınıza bağlı sanal ağ seçin.
    * **Genel IP adresi**: kullanmak istediğiniz genel IP adresi seçin veya yeni bir ortak IP adresi oluşturma için parametre sayısı girin. Bir ortak IP adresi, sanal makinenize Internet üzerinden erişmek için kullanabilirsiniz. Ayrıca, sanal makine için güvenli erişim yardımcı olacak bir ağ güvenlik grubu oluşturduğunuzdan emin olun.
    * **Ağ güvenlik grubu**: daha fazla bilgi için bkz: [kontrol ağ trafiği akışını ağ güvenlik gruplarıyla][virtual-networks-nsg].
  * **Uzantıları**: dağıtımına ekleyerek sanal makine uzantıları yükleyebilirsiniz. Bu adımda uzantısı eklemek gerekmez. SAP desteği için gerekli uzantıları daha sonra yüklenir. Bölüm bkz [Azure Gelişmiş izleme uzantısı SAP yapılandırma] [ deployment-guide-4.5] bu kılavuzdaki.
  * **Yüksek kullanılabilirlik**: bir kullanılabilirlik kümesi seçin veya yeni bir kullanılabilirlik kümesi oluşturmak için parametreleri girin. Daha fazla bilgi için bkz: [Azure kullanılabilirlik kümeleri][planning-guide-3.2.3].
  * **İzleme**
    * **Önyükleme tanılama**: seçebileceğiniz **devre dışı** önyükleme tanılaması için.
    * **Konuk işletim sistemi tanılama**: seçebileceğiniz **devre dışı** Tanılama izleme.

4. **Özet**:

  Seçimlerinizi gözden geçirin ve ardından **Tamam**.

Sanal makineniz, seçtiğiniz kaynak grubunda dağıtılır.
#### <a name="create-a-virtual-machine-by-using-a-template"></a>Bir şablonu kullanarak bir sanal makine oluşturma
Azure portalından özel bir işletim sistemi görüntüsü kullanarak bir dağıtım oluşturmak için aşağıdaki SAP şablonlarından birini kullanın. Bu şablonlar yayımlanacak [azure hızlı başlangıç şablonlarını GitHub deposunu][azure-quickstart-templates-github]. Ayrıca el ile bir sanal makine kullanarak oluşturabileceğiniz [PowerShell][virtual-machines-upload-image-windows-resource-manager].

* [**İki katmanlı yapılandırma (yalnızca bir sanal makine) şablonu** (sap-2-katman-kullanıcı-görüntü)][sap-templates-2-tier-user-image]

  İki katmanlı sistemi yalnızca bir sanal makine kullanarak oluşturmak için bu şablonu kullanın.
* [**İki katmanlı yapılandırma (yalnızca bir sanal makine) şablonu - yönetilen Disk görüntüsü** (sap-2-tier-user-image-md)][sap-templates-2-tier-user-image-md]

  Yalnızca bir sanal makine ve bir yönetilen Disk görüntüsü kullanarak iki katmanlı sistemi oluşturmak için bu şablonu kullanın.
* [**Üç katmanlı yapılandırma (birden çok sanal makine) şablonu** (sap-3-katman-kullanıcı-görüntü)][sap-templates-3-tier-user-image]

  Birden çok sanal makine ya da kendi işletim sistemi görüntüsü kullanarak üç katmanlı sistem oluşturmak için bu şablonu kullanın.
* [**Üç katmanlı yapılandırma (birden çok sanal makine) şablonu - yönetilen Disk görüntüsü** (sap-3-tier-user-image-md)][sap-templates-3-tier-user-image-md]

  Birden çok sanal makine ya da kendi işletim sistemi görüntüsü ve bir yönetilen Disk görüntüsü kullanarak üç katmanlı sistem oluşturmak için bu şablonu kullanın.

Azure portalında şablon için aşağıdaki parametreleri girin:

1. **Temel kavramları**:
  * **Abonelik**: şablonu dağıtmak için kullanılacak aboneliği.
  * **Kaynak grubu**: şablonu dağıtmak için kullanılacak kaynak grubu. Yeni bir kaynak grubu oluşturun veya varolan bir kaynak grubu abonelikte seçin.
  * **Konum**: şablonu dağıtmak nereye. Varolan bir kaynak grubu seçtiyseniz, bu kaynak grubu konumu kullanılır.
2. **Ayarları**:
  * **SAP sistem kimliği**: SAP sistem kimliği
  * **İşletim sistemi türü**: (Windows veya Linux) dağıtmak istediğiniz işletim sistemi türü.
  * **SAP sistem boyutu**: SAP sistem boyutu.

    Yeni sistem sağlar SAP sayısı. Sistem gerektirir kaç SAP değil eminseniz, SAP teknolojisi iş ortağı veya sistem Tümleştirici isteyin.
  * **Sistem kullanılabilirliğini** (yalnızca üç katmanlı şablon): Sistem kullanılabilirlik.

    Seçin **HA** yüksek kullanılabilirlik yüklemesi için uygun bir yapılandırma için. İki veritabanı sunucuları ve iki sunucu ASCS için oluşturulur.
  * **Depolama türü** (yalnızca iki katmanlı şablon): kullanmak için depolama türü.

    Daha büyük sistemler için yüksek oranda Azure Premium Storage kullanmanızı öneririz. Depolama türleri hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:
      * [SAP DBMS örneği için Azure Premium SSD depolama kullanımı][2367194]
      * [Microsoft Azure depolama] [ dbms-guide-2.3] içinde [SAP NetWeaver için Azure sanal makineleri DBMS dağıtımı][dbms-guide]
      * [Premium Storage: Azure sanal makine iş yükleri için yüksek performanslı depolama][storage-premium-storage-preview-portal]
      * [Microsoft Azure Storage'a giriş][storage-introduction]
  * **Kullanıcı görüntüsü VHD URİ'si** (yalnızca yönetilmeyen disk görüntüsü şablonu): URI özel işletim sistemi yansımasını VHD, örneğin, https://&lt;accountname >.blob.core.windows.net/vhds/userimage.vhd.
  * **Kullanıcı görüntü depolama hesabı** (yalnızca yönetilmeyen disk görüntüsü şablonu): özel işletim sistemi görüntüsünün depolandığı, örneğin, depolama hesabının adını &lt;accountname > https:// içinde&lt;accountname >.blob.core.windows.net/vhds/userimage.vhd.
  * **userImageId** (yalnızca yönetilen disk görüntüsü şablonu): kullanmak istediğiniz Disk yönetilen görüntü kimliği
  * **Yönetici kullanıcı adı** ve **yönetici parolası**: kullanıcı adı ve parola.

    Yeni bir kullanıcı sanal makineye oturum açmak için oluşturulur.
  * **Yeni veya var olan bir alt ağ**: yeni bir sanal ağ ve alt ağ oluşturulur veya mevcut bir alt kullanılan belirler. Şirket içi ağınıza bağlı bir sanal ağ zaten varsa, seçin **varolan**.
  * **Alt ağ kimliği**: olduğu sanal makinelerin bağlanması için alt ağ kimliği. Sanal makine şirket içi ağınıza bağlanmak için kullanılacak VPN ya da ExpressRoute sanal ağınızın alt ağ seçin. Kimliği genellikle şu şekildedir:

    /Subscriptions/&lt;abonelik kimliği > /resourceGroups/&lt;kaynak grubu adı > /providers/Microsoft.Network/virtualNetworks/&lt;sanal ağ adı > /subnets/&lt;alt ağ adı >

3. **Hüküm ve koşullar**:  
    Gözden geçirin ve yasal koşulları kabul edin.

4.  Seçin **satın alma**.

#### <a name="install-the-vm-agent-linux-only"></a>VM Aracısı (yalnızca Linux)
Önceki bölümde açıklanan şablonları kullanmak için Linux Aracısı'nı kullanıcı görüntüde yüklü olması gereken veya dağıtım başarısız olur. Karşıdan yükleyip VM Aracısı kullanıcı resimde açıklandığı gibi [indirme, yükleme ve Azure VM Aracısı'nı etkinleştir][deployment-guide-4.4]. Şablonları kullanmıyorsanız, daha sonra VM Aracısı de yükleyebilirsiniz.

#### <a name="join-a-domain-windows-only"></a>(Yalnızca Windows) etki alanına Katıl
Azure dağıtımınız için bir şirket içi Active Directory veya DNS örneğine bir Azure siteden siteye VPN bağlantısı veya Azure ExpressRoute üzerinden bağlıysa, (Bu adlı *şirketler arası* içinde [Azure sanal makineleri planlama ve uygulama SAP NetWeaver için][planning-guide]), VM bir şirket içi etki alanına katılma, beklenir. Bu adım için ilgili önemli noktalar hakkında daha fazla bilgi için bkz: [bir şirket içi etki alanına (yalnızca Windows) için bir VM katılmak][deployment-guide-4.3].

#### <a name="configure-proxy-settings"></a>Proxy ayarlarını yapılandır
Şirket içi ağınıza nasıl yapılandırıldığına bağlı olarak, VM proxy ayarlamak gerekebilir. VM için bağlıysa, VPN veya ExpressRoute, VM aracılığıyla şirket içi ağınıza Internet erişimine sahip olmayabilir ve gerekli uzantıları indirmesi ve izleme verileri toplamak yükleyemezsiniz. Daha fazla bilgi için bkz: [proxy yapılandırma][deployment-guide-configure-proxy].

#### <a name="configure-monitoring"></a>İzlemeyi Yapılandır
SAP açıklandığı gibi Azure Monitoring uzantısı SAP için ayarlamak, ortamınıza emin olmak için destekleyen [Azure Gelişmiş izleme uzantısı SAP yapılandırma][deployment-guide-4.5]. SAP izleme önkoşulları denetleyin ve SAP çekirdek ve SAP konak Aracısı, gerekli en düşük sürümlerle kaynaklarında listelenen [SAP kaynakları][deployment-guide-2.2].

#### <a name="monitoring-check"></a>Onay izleme
İzleme çalışıp çalışmadığını, açıklandığı gibi denetleyin [denetler ve uçtan uca izleme ayarlamak için sorun giderme][deployment-guide-troubleshooting-chapter].


### <a name="a9a60133-a763-4de8-8986-ac0fa33aa8c1"></a>Senaryo 3: şirket içi VM genelleştirilmiş Azure VHD SAP ile kullanarak taşıma
Bu senaryoda, belirli bir SAP sistemi bir şirket içi ortamından Azure'a taşımak planlayın. Azure DBMS veri ve günlük dosyaları ile işletim sistemi, SAP ikili dosyaları ve sonunda DBMS ikili dosyaları VHD yanı sıra, VHD'leri yükleyerek bunu yapabilirsiniz. Açıklanan senaryoyu aksine [Senaryo 2: özel bir görüntü ile bir VM için SAP dağıtma][deployment-guide-3.3], bu durumda, ana bilgisayar adı, SAP SID tutmak ve SAP kullanıcı hesapları Azure VM'de, şirket içi ortamda yapılandırılmış olduğundan. İşletim sistemi generalize gerekmez. Bu senaryo genellikle burada şirket içi SAP yatay parçası çalışır ve kısmını Azure üzerinde çalışan şirketler arası senaryoları için de geçerlidir.

Bu senaryoda, VM aracısının olup **değil** dağıtımı sırasında otomatik olarak yüklü. VM Aracısı'nı ve Azure Gelişmiş izleme uzantısı SAP SAP NetWeaver Azure üzerinde çalıştırmak için gerekli olduğundan, indirme, yükleme ve sanal makineyi oluşturduktan sonra her iki bileşenin el ile etkinleştirmeniz gerekir.

Azure VM Aracısı hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın.

- - -
> ![Windows][Logo_Windows] Windows
>
> [Azure sanal makine aracısını genel bakış][virtual-machines-windows-agent-user-guide]
>
> ![Linux][Logo_Linux] Linux
>
> [Azure Linux Aracısı Kullanıcı Kılavuzu][virtual-machines-linux-agent-user-guide]
>
>

- - -

Aşağıdaki akış çizelgesi, bir şirket içi VM genelleştirilmiş Azure VHD kullanarak taşıma adımları sırasını göstermektedir:

![VM dağıtımı için bir VM disk kullanarak SAP sistemleri akış çizelgesi][deployment-guide-figure-400]

Disk zaten karşıya ve Azure'da tanımlanan varsa (bkz [Azure sanal makineleri planlama ve uygulama SAP NetWeaver için][planning-guide]), yapmak görevleri açıklanan sonraki birkaç bölümler.

#### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
Azure Portalı aracılığıyla özel bir işletim sistemi diski kullanarak bir dağıtım oluşturmak için yayınlanan SAP şablonunu kullanın [azure hızlı başlangıç şablonlarını GitHub deposunu][azure-quickstart-templates-github]. Ayrıca el ile bir sanal makine PowerShell kullanarak oluşturabilirsiniz.

* [**İki katmanlı yapılandırma (yalnızca bir sanal makine) şablonu** (sap-2-katman-kullanıcı-disk)][sap-templates-2-tier-os-disk]

  İki katmanlı sistemi yalnızca bir sanal makine kullanarak oluşturmak için bu şablonu kullanın.
* [**İki katmanlı yapılandırma (yalnızca bir sanal makine) şablonu - yönetilen Disk** (sap-2-tier-user-disk-md)][sap-templates-2-tier-os-disk-md]

  İki katmanlı sistemi yalnızca bir sanal makine ve yönetilen bir Disk kullanarak oluşturmak için bu şablonu kullanın.

Azure portalında şablon için aşağıdaki parametreleri girin:

1. **Temel kavramları**:
  * **Abonelik**: şablonu dağıtmak için kullanılacak aboneliği.
  * **Kaynak grubu**: şablonu dağıtmak için kullanılacak kaynak grubu. Yeni bir kaynak grubu oluşturun veya varolan bir kaynak grubu abonelikte seçin.
  * **Konum**: şablonu dağıtmak nereye. Varolan bir kaynak grubu seçtiyseniz, bu kaynak grubu konumu kullanılır.
2. **Ayarları**:
  * **SAP sistem kimliği**: SAP sistem kimliği
  * **İşletim sistemi türü**: (Windows veya Linux) dağıtmak istediğiniz işletim sistemi türü.
  * **SAP sistem boyutu**: SAP sistem boyutu.

    Yeni sistem sağlar SAP sayısı. Sistem gerektirir kaç SAP değil eminseniz, SAP teknolojisi iş ortağı veya sistem Tümleştirici isteyin.
  * **Depolama türü** (yalnızca iki katmanlı şablon): kullanmak için depolama türü.

    Daha büyük sistemler için yüksek oranda Azure Premium Storage kullanmanızı öneririz. Depolama türleri hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:
      * [SAP DBMS örneği için Azure Premium SSD depolama kullanımı][2367194]
      * [Microsoft Azure depolama] [ dbms-guide-2.3] içinde [SAP NetWeaver için Azure sanal makine DBMS dağıtımı][dbms-guide]
      * [Premium Storage: Azure sanal makine iş yükleri için yüksek performanslı depolama][storage-premium-storage-preview-portal]
      * [Microsoft Azure Storage'a giriş][storage-introduction]
  * **İşletim sistemi diski VHD URİ'si** (yalnızca yönetilmeyen disk şablonu): özel bir işletim sistemi diski, örneğin, https:// URI'sini&lt;accountname >.blob.core.windows.net/vhds/osdisk.vhd.
  * **İşletim sistemi diski Disk kimliği yönetilen** (yalnızca yönetilen disk şablonu): yönetilen Disk işletim sistemi disk kimliği /subscriptions/92d102f7-81a5-4df7-9877-54987ba97dd9/resourceGroups/group/providers/Microsoft.Compute/disks/WIN
  * **Yeni veya var olan bir alt ağ**: yeni bir sanal ağ ve alt ağ oluşturulur veya mevcut bir alt kullanılan olup olmadığını belirler. Şirket içi ağınıza bağlı bir sanal ağ zaten varsa, seçin **varolan**.
  * **Alt ağ kimliği**: olduğu sanal makinelerin bağlanması için alt ağ kimliği. Sanal makine şirket içi ağınıza bağlanmak için kullanılacak VPN veya Azure ExpressRoute sanal ağınızın alt ağ seçin. Kimliği genellikle şu şekildedir:

    /Subscriptions/&lt;abonelik kimliği > /resourceGroups/&lt;kaynak grubu adı > /providers/Microsoft.Network/virtualNetworks/&lt;sanal ağ adı > /subnets/&lt;alt ağ adı >

3. **Hüküm ve koşullar**:  
    Gözden geçirin ve yasal koşulları kabul edin.

4.  Seçin **satın alma**.

#### <a name="install-the-vm-agent"></a>VM Aracısı yükleme
Önceki bölümde açıklanan şablonları kullanmak için VM Aracısı işletim sistemi disk üzerinde yüklü ya da dağıtım başarısız olur. Karşıdan yükleyip VM Aracısı VM içinde açıklandığı gibi [indirme, yükleme ve Azure VM Aracısı'nı etkinleştir][deployment-guide-4.4].

Önceki bölümde açıklanan şablonları kullanmıyorsanız, daha sonra VM Aracısı da yükleyebilirsiniz.

#### <a name="join-a-domain-windows-only"></a>(Yalnızca Windows) etki alanına Katıl
Azure dağıtımınız için bir şirket içi Active Directory veya DNS örneğine bir Azure siteden siteye VPN bağlantısı veya ExpressRoute aracılığıyla bağlıysa, (Bu adlı *şirketler arası* içinde [Azure sanal makineleri planlama ve uygulama SAP NetWeaver için][planning-guide]), VM bir şirket içi etki alanına katılma, beklenir. Bu görev için konuları hakkında daha fazla bilgi için bkz: [bir şirket içi etki alanına (yalnızca Windows) için bir VM katılmak][deployment-guide-4.3].

#### <a name="configure-proxy-settings"></a>Proxy ayarlarını yapılandır
Şirket içi ağınıza nasıl yapılandırıldığına bağlı olarak, VM proxy ayarlamak gerekebilir. VM için bağlıysa, VPN veya ExpressRoute, VM aracılığıyla şirket içi ağınıza Internet erişimine sahip olmayabilir ve gerekli uzantıları indirmesi ve izleme verileri toplamak yükleyemezsiniz. Daha fazla bilgi için bkz: [proxy yapılandırma][deployment-guide-configure-proxy].

#### <a name="configure-monitoring"></a>İzlemeyi Yapılandır
SAP açıklandığı gibi Azure Monitoring uzantısı SAP için ayarlamak, ortamınıza emin olmak için destekleyen [Azure Gelişmiş izleme uzantısı SAP yapılandırma][deployment-guide-4.5]. SAP izleme önkoşulları denetleyin ve SAP çekirdek ve SAP konak Aracısı, gerekli en düşük sürümlerle kaynaklarında listelenen [SAP kaynakları][deployment-guide-2.2].

#### <a name="monitoring-check"></a>Onay izleme
İzleme çalışıp çalışmadığını, açıklandığı gibi denetleyin [denetler ve uçtan uca izleme ayarlamak için sorun giderme][deployment-guide-troubleshooting-chapter].

## <a name="update-the-monitoring-configuration-for-sap"></a>SAP için izleme yapılandırmasını güncelleştirme
Aşağıdaki senaryolardan birinde SAP İzleme yapılandırmasını güncelleştirin:
* Birleşik Microsoft/SAP ekibi, izleme yeteneklerini genişletir ve daha fazla veya daha az sayaçları ister.
* Microsoft izleme verilerini teslim eden Azure altyapının yeni bir sürümünü kullanıma sunmaktadır ve Azure Gelişmiş izleme uzantısı SAP bu değişiklikleri uyarlanan gerekir.
* Azure VM için ek veri disklerinin bağlayın veya bir veri diski kaldırın. Bu senaryoda, depolama ile ilgili verilerin toplanması güncelleştirin. Ekleyerek veya silerek uç noktaları veya bir VM'ye IP adresleri atayarak yapılandırmanızı değiştirme izleme yapılandırması etkilemez.
* Size, Azure VM boyutu Örneğin, başka bir VM boyutu boyutundan A5 değiştirin.
* Azure VM için yeni ağ arabirimi ekleyin.

İzleme ayarları güncelleştirmek için izleme altyapısının içindeki adımları izleyerek güncelleştirin [Azure Gelişmiş izleme uzantısı SAP yapılandırma][deployment-guide-4.5].

## <a name="detailed-tasks-for-sap-software-deployment"></a>SAP yazılım dağıtımı için ayrıntılı görevleri
Bu bölümde yapılandırma ve dağıtım sürecindeki belirli görevleri yapmak için adımları açıklanmıştır.

### <a name="604bcec2-8b6e-48d2-a944-61b0f5dee2f7"></a>Azure PowerShell cmdlet'lerini dağıtma
1.  Git [Microsoft Azure indirmeleri](https://azure.microsoft.com/downloads/).
2.  Altında **komut satırı araçları**altında **PowerShell**seçin **Windows yükleme**.
3.  İndirilen Dosya (örneğin, WindowsAzurePowershellGet.3f.3f.3fnew.exe) için Microsoft İndirme Yöneticisi iletişim kutusunda seçin **çalıştırmak**.
4.  Microsoft Web Platformu Yükleyicisi (Microsoft Web PI) çalıştırmak için seçin **Evet**.
5.  Bu görünür benzer bir sayfa:

  ![Azure PowerShell cmdlet'leri yükleme sayfası][deployment-guide-figure-500]<a name="figure-5"></a>

6.  Seçin **yükleme**ve Microsoft Yazılım Lisans Koşulları'nı kabul edin.
7.  PowerShell yüklü. Seçin **son** Yükleme Sihirbazı'nı kapatın.

Güncelleştirmeler genellikle her ay güncelleştirilen PowerShell cmdlet'leri için sık sık denetleyin. Güncelleştirmeleri denetlemek için en kolay yolu, adım 5'te gösterilen yükleme sayfasında kadar yukarıdaki yükleme adımlarını yapmaktır. Adım 5'te gösterilen sayfasında cmdlet'leri yayın tarihi ve sürüm sayısı dahil edilir. SAP notta aksi belirtilmediği sürece [1928533] veya SAP Not [2015553], Azure PowerShell cmdlet'lerinin en son sürümüyle çalışmasını öneririz.

Bilgisayarınızda yüklü olan Azure PowerShell cmdlet'lerini sürümünü denetlemek için bu PowerShell komutunu çalıştırın:
```powershell
(Get-Module AzureRm.Compute).Version
```
Sonuç şöyle görünür:

![Azure PowerShell cmdlet sürüm denetiminin sonucu.][deployment-guide-figure-600]
<a name="figure-6"></a>

Bilgisayarınızda yüklü Azure cmdlet sürümü geçerli sürümü ise, Yükleme Sihirbazı'nın ilk sayfasında ekleyerek belirtir **(yüklü)** ürün başlığı için (aşağıdaki ekran görüntüsüne bakın). PowerShell Azure cmdlet'lerini güncel değil. Yükleme Sihirbazı'nı kapatmak için seçin **çıkış**.

![Azure PowerShell cmdlet'lerinin en son sürümü yüklü olmadığını gösteren Azure PowerShell cmdlet'leri yükleme sayfası][deployment-guide-figure-700]
<a name="figure-7"></a>

### <a name="1ded9453-1330-442a-86ea-e0fd8ae8cab3"></a>Azure CLI dağıtma
1.  Git [Microsoft Azure indirmeleri](https://azure.microsoft.com/downloads/).
2.  Altında **komut satırı araçları**altında **Azure komut satırı arabirimi**seçin **yükleme** işletim sisteminizin bağlantı.
3.  İndirilen Dosya (örneğin, WindowsAzureXPlatCLI.3f.3f.3fnew.exe) için Microsoft İndirme Yöneticisi iletişim kutusunda seçin **çalıştırmak**.
4.  Microsoft Web Platformu Yükleyicisi (Microsoft Web PI) çalıştırmak için seçin **Evet**.
5.  Bu görünür benzer bir sayfa:

  ![Azure PowerShell cmdlet'leri yükleme sayfası][deployment-guide-figure-500]<a name="figure-5"></a>

6.  Seçin **yükleme**ve Microsoft Yazılım Lisans Koşulları'nı kabul edin.
7.  Azure CLI yüklenir. Seçin **son** Yükleme Sihirbazı'nı kapatın.

Güncelleştirmeler genellikle her ay güncelleştirilen Azure CLI için sık sık denetleyin. Güncelleştirmeleri denetlemek için en kolay yolu, adım 5'te gösterilen yükleme sayfasında kadar yukarıdaki yükleme adımlarını yapmaktır.

Azure CLI, bilgisayarınızda yüklü sürümünü denetlemek için şu komutu çalıştırın:
```
azure --version
```

Sonuç şöyle görünür:

![Azure CLI sürüm denetiminin sonucu.][deployment-guide-figure-760]
<a name="0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda"></a>

### <a name="31d9ecd6-b136-4c73-b61e-da4a29bbc9cc"></a>Bir şirket içi etki alanına (yalnızca Windows) bir VM ekleme
Burada şirket içi Active Directory ve DNS Azure'da genişletilir, bir şirket içi senaryosunda, SAP VM'ler dağıtırsanız, sanal makineleri bir şirket içi etki alanına katılma beklenir. Bir şirket içi etki alanı ve bir şirket içi etki alanının bir üyesi olması gereken ek yazılım için bir VM katılmak için ayrıntılı adımlar müşteri tarafından değişir. Genellikle, bir VM bir şirket içi etki alanına katılmak için kötü amaçlı yazılımdan koruma yazılımı gibi ek yazılım ve yedekleme ya da izleme yazılımı yüklemeniz gerekir.

Bu senaryoda, ayrıca bir VM, ortamınızdaki bir etki alanına katıldığında Internet proxy ayarlarını zorlanır, Windows yerel sistem hesabını (S-1-5-18) Konuk VM'de aynı proxy ayarlarını sahip olduğundan emin olmanız gerekir. Bir etki alanına etki alanı sistemlerinde uygulandığı Grup İlkesi kullanarak proxy zorlamak için en basit seçenek budur.

### <a name="c7cbb0dc-52a4-49db-8e03-83e7edc2927d"></a>İndirme, yükleme ve Azure VM Aracısı'nı etkinleştir
(Örneğin, sysprep veya Windows Sistem Hazırlama aracı kökenli olmayan bir görüntü) genelleştirilmiş olmayan bir işletim sistemi görüntüsünden dağıtılan sanal makineler için el ile indirin, yükleyin ve Azure VM Aracısı'nı etkinleştirmeniz gerekir.

Azure Marketi'nden VM dağıtırsanız, bu adım gerekli değildir. Azure Marketi'nden görüntüleri Azure VM Aracısı zaten var.

#### <a name="b2db5c9a-a076-42c6-9835-16945868e866"></a>Windows
1.  Azure VM Aracısı'nı indirme:
  1.  Karşıdan [Azure VM Aracısı Yükleyici paketi](https://go.microsoft.com/fwlink/?LinkId=394789).
  2.  VM Aracısı MSI paketini bir kişisel bilgisayar veya sunucu üzerinde yerel olarak depolar.
2.  Azure VM aracısını yükleyin:
  1.  Dağıtılan Azure VM'ye Uzak Masaüstü Protokolü (RDP) kullanarak bağlanın.
  2.  VM üzerinde bir Windows Explorer penceresi açın ve VM Aracısı'nın MSI dosyası için hedef dizini seçin.
  3.  Azure VM Aracısı yükleyici MSI dosyası yerel bilgisayar/sunucunuzdan VM'de VM Aracısı'nın hedef dizini sürükleyin.
  4.  VM MSI dosyasına çift tıklayın.
3.  Şirket içi etki alanına katılmış olan VM'ler için açıklandığı gibi nihai Internet proxy ayarlarını ayrıca VM Windows yerel sistem hesabında (S-1-5-18) uygulanacağını emin olun [proxy yapılandırma][deployment-guide-configure-proxy]. VM Aracısı bu bağlamda çalışır ve Azure'a bağlanabilmesi gerekir.

Kullanıcı etkileşimi olmadan Azure VM Aracısı'nı güncelleştirmek için gereklidir. VM Aracısı otomatik olarak güncelleştirilir ve VM yeniden başlatma gerektirmez.

#### <a name="6889ff12-eaaf-4f3c-97e1-7c9edc7f7542"></a>Linux
Linux için VM Aracısı'nı yüklemek için aşağıdaki komutları kullanın:

* **SUSE Linux Enterprise Server (SLES)**

  ```
  sudo zypper install WALinuxAgent
  ```

* **Red Hat Enterprise Linux (RHEL) veya Oracle Linux**

  ```
  sudo yum install WALinuxAgent
  ```

Azure Linux Aracısı'nı güncelleştirmek için aracının zaten yüklü değilse, açıklanan adımları yapmak [bir VM'de Azure Linux Aracısı Github'dan güncelleştirin][virtual-machines-linux-update-agent].

### <a name="baccae00-6f79-4307-ade4-40292ce4e02d"></a>Proxy ayarlarını yapılandır
Windows'da Proxy'yi yapılandırmak için uygulayacağınız adımlar, Linux proxy yapılandırma biçiminizi farklıdır.

#### <a name="windows"></a>Windows
Proxy ayarları Internet'e erişmek yerel sistem hesabı için doğru ayarlanmış olması gerekir. Proxy ayarlarını Grup İlkesi tarafından ayarlanmamışsa, yerel sistem hesabı için ayarları yapılandırabilirsiniz.

1. Git **Başlat**, girin **gpedit.msc**ve ardından **Enter**.
2. Seçin **Bilgisayar Yapılandırması** > **Yönetim Şablonları** > **Windows bileşenleri** > **Internet Explorer**. Emin olmak ayarın **proxy ayarlarını makine başına (yerine kullanıcı) yapmak** devre dışı veya yapılandırılmamış.
3. İçinde **Denetim Masası**gidin **ağ ve Paylaşım Merkezi** > **Internet Seçenekleri**.
4. Üzerinde **bağlantıları** sekmesine **LAN Ayarları** düğmesi.
5. Clear **ayarları otomatik olarak algıla** onay kutusu.
6. Seçin **AĞINIZ için bir proxy sunucu kullanacak** onay kutusunu işaretleyin ve ardından proxy adresi ve bağlantı noktası girin.
7. Seçin **Gelişmiş** düğmesi.
8. İçinde **özel durumları** kutusunda, IP adresini girin **168.63.129.16**. **Tamam**’ı seçin.

#### <a name="linux"></a>Linux
Konumda bulunan Microsoft Azure Konuk Aracısı, yapılandırma dosyasında doğru proxy yapılandırma \\vb.\\waagent.conf.

Aşağıdaki parametreleri ayarlayın:

1.  **HTTP proxy konağını**. Örneğin, kümesine **proxy.corp.local**.
  ```
  HttpProxy.Host=<proxy host>

  ```
2.  **HTTP proxy bağlantı noktası**. Örneğin, kümesine **80**.
  ```
  HttpProxy.Port=<port of the proxy host>

  ```
3.  Aracıyı yeniden başlatın.

  ```
  sudo service waagent restart
  ```

Proxy ayarları \\vb.\\waagent.conf gerekli VM uzantıları için de geçerlidir. Azure depoları kullanmak istiyorsanız, bu depoları trafiği, şirket içi intranet üzerinden giderek değil emin olun. Kullanıcı tanımlı yollar zorlamalı tüneli etkinleştirmek için bir rota eklediğinizden emin olun oluşturduysanız, trafiği doğrudan Internet'e ve siteden siteye VPN bağlantısı üzerinden değil depoları yönlendirir.

* **SLES**

  Ayrıca, IP adreslerini listelenen için yollar eklemeniz gerekir \\vb.\\regionserverclnt.cfg. Aşağıdaki şekilde bir örnek gösterilmektedir:

  ![Zorlamalı tünel oluşturma][deployment-guide-figure-50]


* **RHEL**

  Ayrıca listelenen konakların IP adresleri için yollar eklemeniz gerekir \\vb.\\yum.repos.d\\rhui yük dengeleyicileri. Örneğin, önceki şekle bakın.

* **Oracle Linux**

  Oracle Linux Azure üzerinde hiçbir depoları vardır. Oracle Linux için kendi depoları yapılandırmak veya ortak depoları kullanmanız gerekir.

Kullanıcı tanımlı yollar hakkında daha fazla bilgi için bkz: [kullanıcı tanımlı yollar ve IP iletimini][virtual-networks-udr-overview].

### <a name="d98edcd3-f2a1-49f7-b26a-07448ceb60ca"></a>SAP için Azure Gelişmiş izleme uzantısı yapılandırma
Ne zaman hazırlıklı VM açıklandığı gibi [VM'ler Azure üzerinde SAP için dağıtım senaryoları][deployment-guide-3], Azure VM Aracısı sanal makineye yüklenir. Sonraki adım Azure Gelişmiş izleme uzantısı Azure uzantısı depo genel Azure veri merkezleri içinde kullanılabilir SAP dağıtmaktır. Daha fazla bilgi için bkz: [Azure sanal makineleri planlama ve uygulama SAP NetWeaver için][planning-guide-9.1].

Yüklemek ve Azure Gelişmiş izleme uzantısı SAP yapılandırmak için PowerShell veya Azure CLI kullanın. Windows makine kullanarak bir Windows veya Linux VM uzantısı yüklemek için bkz: [Azure PowerShell][deployment-guide-4.5.1]. Bir Linux Masaüstü'nü kullanarak bir Linux VM uzantısı yüklemek için bkz: [Azure CLI][deployment-guide-4.5.2].

#### <a name="987cf279-d713-4b4c-8143-6b11589bb9d4"></a>Linux ve Windows VM'ler için Azure PowerShell
PowerShell kullanarak Azure Gelişmiş izleme uzantısı SAP yüklemek için:

1. Azure PowerShell cmdlet'ini en son sürümünü yüklediğinizden emin olun. Daha fazla bilgi için bkz: [dağıtımı Azure PowerShell cmdlet'lerini][deployment-guide-4.1].  
2. Aşağıdaki PowerShell cmdlet’ini çalıştırın.
    Kullanılabilir ortamlar listesi için çalıştırın `commandlet Get-AzureRmEnvironment`. Genel Azure kullanmak istiyorsanız, ortamınızı olan **AzureCloud**. Çin'de Azure için seçin **AzureChinaCloud**.

    ```powershell
    $env = Get-AzureRmEnvironment -Name <name of the environment>
    Connect-AzureRmAccount -Environment $env
    Set-AzureRmContext -SubscriptionName <subscription name>

    Set-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
    ```

Hesap verilerinizi girin ve Azure sanal makineyi tanımlamanız sonra komut dosyasını gerekli uzantıları dağıtır ve gerekli özellikleri sağlar. Bu işlem birkaç dakika sürebilir.
Hakkında daha fazla bilgi için `Set-AzureRmVMAEMExtension`, bkz: [kümesi AzureRmVMAEMExtension][msdn-set-azurermvmaemextension].

![SAP özgü aktarılmadığı Azure cmdlet kümesi AzureRmVMAEMExtension][deployment-guide-figure-900]

`Set-AzureRmVMAEMExtension` Yapılandırması için SAP izleme ana bilgisayarı yapılandırmak için gereken tüm adımları yapar.

Komut dosyası çıkışını aşağıdaki bilgileri içerir:

* İşletim sistemi diski ve tüm ek veri disklerinin için izlemeyi yapılandırıldığını onayı.
* Sonraki iki iletileri belirli bir depolama hesabı için depolama ölçümleri yapılandırmasını doğrulayın.
* Tek satırlık bir çıktı izleme yapılandırmasının gerçek güncelleştirme durumunu verir.
* Çıktısını başka bir satır yapılandırması dağıtılan güncelleştirilmiş veya onaylar.
* Çıktının son satırında bilgilendirme amaçlıdır. İzleme Yapılandırması test seçeneklerinizi gösterir.
* Gelişmiş Azure Monitoring tüm adımları başarıyla çalıştırılıncaya ve Azure altyapısı gerekli verileri sağlar denetlemek için Azure Gelişmiş izleme uzantısı SAP, hazırlık denetimi açıklandığı gibi devam [Gelişmiş Azure izlemesi için SAP için hazırlık denetimi][deployment-guide-5.1].
* İlgili verileri toplamak Azure tanılama 15-30 dakika bekleyin.

#### <a name="408f3779-f422-4413-82f8-c57a23b4fc2f"></a>Linux VM'ler için Azure CLI
Azure Gelişmiş izleme uzantısı SAP için Azure CLI kullanarak yüklemek için:

1. Azure CLI 1.0 açıklandığı gibi yüklemeniz [Azure CLI 1.0 yüklemeyi][azure-cli].
2. Azure hesabınızla oturum açın:

  ```
  azure login
  ```

3. Azure Resource Manager moduna geçin:

  ```
  azure config mode arm
  ```

4. Azure Gelişmiş izlemeyi etkinleştir:

  ```
  azure vm enable-aem <resource-group-name> <vm-name>
  ```

5. Azure Gelişmiş izleme uzantısı Azure Linux VM'de etkin olduğunu doğrulayın. Denetleme olup olmadığını dosya \\var\\lib\\AzureEnhancedMonitor\\PerfCounters bulunmaktadır. Bir komut isteminde varsa Azure Gelişmiş İzleyici tarafından toplanan bilgileri görüntülemek için bu komutu çalıştırın:
```
cat /var/lib/AzureEnhancedMonitor/PerfCounters
```

Çıktı şu şekildedir:
```
2;cpu;Current Hw Frequency;;0;2194.659;MHz;60;1444036656;saplnxmon;
2;cpu;Max Hw Frequency;;0;2194.659;MHz;0;1444036656;saplnxmon;
???
???
```

## <a name="564adb4f-5c95-4041-9616-6635e83a810b"></a>Denetim ve uçtan uca izleme için sorun giderme
Azure VM dağıtılan ve ilgili Azure izleme altyapıyı ayarladıktan sonra Azure Gelişmiş izleme uzantısı'nın tüm bileşenleri beklendiği gibi çalışıp çalışmadığını denetleyin.

Bölümünde açıklandığı gibi Azure Gelişmiş izleme uzantısı SAP için hazırlık denetimi Çalıştır [Azure Gelişmiş izleme uzantısı SAP için hazırlık denetimi][deployment-guide-5.1]. Tüm hazırlık denetimi sonuçları pozitif ve tüm ilgili performans sayaçları Tamam görünürse Azure izleme başarıyla ayarlandı. SAP notları açıklandığı gibi SAP konak Aracısı yükleme işlemine devam edebilirsiniz [SAP kaynakları][deployment-guide-2.2]. Hazırlık denetimi sayaçları eksik olduğunu gösteriyorsa, Azure izleme altyapısının için sistem durumu denetimi bağlantısında açıklandığı şekilde çalıştırabilirsiniz [izleme Azure altyapısı yapılandırması için sistem durumu denetimi][deployment-guide-5.2]. Daha fazla sorun giderme seçenekleri için bkz: [Azure sorun giderme için SAP izleme][deployment-guide-5.3].

### <a name="bb61ce92-8c5c-461f-8c53-39f5e5ed91f2"></a>Azure Gelişmiş izleme uzantısı SAP için hazırlık denetimi
Bu onay SAP uygulamanız içinde görünür tüm performans ölçümleri temel Azure izleme altyapısı tarafından sağlanan emin olur.

#### <a name="run-the-readiness-check-on-a-windows-vm"></a>Bir Windows VM üzerinde hazırlık denetimi Çalıştır

1.  Azure sanal makinede oturum açın (bir yönetici hesabı kullanarak gerekli değildir).
2.  Bir komut istemi penceresi açın.
3.  Komut isteminde Azure Gelişmiş izleme uzantısı SAP için yükleme klasörüne dizini değiştirin: C:\\paketleri\\eklentileri\\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;sürüm >\\bırak

  *Sürüm* izleme uzantısı yolunu değişebilir. Klasörleri birden fazla sürümünü yükleme klasöründe izleme uzantısı için görürseniz, AzureEnhancedMonitoring Windows hizmeti yapılandırmasını denetleyin ve ardından olarak gösterilen klasöre geçin *yürütülebilir dosya yoluna*.

  ![Azure Gelişmiş izleme uzantısı SAP çalışan hizmeti özellikleri][deployment-guide-figure-1000]

4.  Komut istemine **azperflib.exe** hiçbir parametre olmadan.

  > [!NOTE]
  > Azperflib.exe bir döngüde çalışır ve her 60 saniyede toplanan sayaçların güncelleştirir. Döngü sona erdirmek için komut istemi penceresini kapatın.
  >
  >

Azure Gelişmiş izleme uzantısı yüklü değil veya AzureEnhancedMonitoring hizmeti çalışmıyor olabilir, uzantısı düzgün yapılandırılmamış. Uzantı dağıtma hakkında ayrıntılı bilgi için bkz: [Azure izleme altyapısının SAP için sorun giderme][deployment-guide-5.3].

##### <a name="check-the-output-of-azperflibexe"></a>Onay azperflib.exe çıktısı
Tüm Azure performans sayaçları için SAP doldurulmuş Azperflib.exe çıktısını gösterir. Toplanan sayaçlarının listesi sonunda Özet ve sistem durumu gösterge Azure izleme durumunu gösterir.

![Sistem durumu denetimi herhangi bir sorun mevcut olduğunu gösteren azperflib.exe çalıştırarak çıktısı][deployment-guide-figure-1100]
<a name="figure-11"></a>

İçin döndürülen sonuç denetleme **sayaçları toplam** bildirilen boş olarak ve için çıktı **sistem durumu**, yukarıdaki şekilde gösterildiği.

Sonuçta elde edilen değerleri aşağıdaki gibi yorumlar:

| Azperflib.exe sonuç değerleri | Azure sistem durumunu izleme |
| --- | --- |
| **API çağrıları - mevcut değil** | Kullanılabilir değil sayaçları ya da olabilir sanal makine yapılandırması için geçerli veya hatalardır. Bkz: **sistem durumu**. |
| **Sayaçları toplam - boş** |Aşağıdaki iki Azure depolama sayaçları boş olabilir: <ul><li>Depolama alanından Op gecikme okuma sunucu milisaniye</li><li>Depolama alanından Op gecikme okuma E2E milisaniye</li></ul>Diğer tüm sayaçları değerlere sahip olmalıdır. |
| **Sistem durumu** |Yalnızca Tamam IF dönüş durumu **Tamam**. |
| **Tanılama** |Sistem durumu hakkında ayrıntılı bilgiler. |

Varsa **sistem durumu** değeri değil **Tamam**,'ndaki yönergeleri izleyin [izleme Azure altyapısı yapılandırması için sistem durumu denetimi][deployment-guide-5.2].

#### <a name="run-the-readiness-check-on-a-linux-vm"></a>Bir Linux VM üzerinde hazırlık denetimi Çalıştır

1.  SSH kullanarak Azure sanal makinesine bağlanın.

2.  Azure Gelişmiş izleme uzantısı çıktısını denetleyin.

  a.  `more /var/lib/AzureEnhancedMonitor/PerfCounters` öğesini çalıştırın

   **Beklenen sonucu**: performans sayaçları listesi döndürür. Dosya boş olmamalıdır.

 b. `cat /var/lib/AzureEnhancedMonitor/PerfCounters | grep Error` öğesini çalıştırın

   **Beklenen sonucu**: hata olduğu döndürür tek bir çizgi **hiçbiri**, örneğin, **3; config; Hata; 0; 0; yok; 0; 1456416792; tst servercs;**

  c. `more /var/lib/AzureEnhancedMonitor/LatestErrorRecord` öğesini çalıştırın

    **Beklenen sonucu**: boş olarak döndürür veya yok.

Önceki onay başarılı olmadıysa, bu ek denetimler çalıştırın:

1.  Waagent yüklü ve etkin olduğundan emin olun.

  a.  `sudo ls -al /var/lib/waagent/` öğesini çalıştırın

      **Beklenen sonucu**: waagent dizinin içeriğini listeler.

  b.  `ps -ax | grep waagent` öğesini çalıştırın

   **Beklenen sonucu**: için benzer bir giriş görüntüler: `python /usr/sbin/waagent -daemon`

3.   Azure Gelişmiş izleme uzantısı yüklü ve çalışıyor olduğundan emin olun.

  a.  `sudo sh -c 'ls -al /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-*/'` öğesini çalıştırın

    **Beklenen sonucu**: Azure Gelişmiş izleme uzantısı dizinin içeriğini listeler.

  b. `ps -ax | grep AzureEnhanced` öğesini çalıştırın

     **Beklenen sonucu**: için benzer bir giriş görüntüler: `python /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-2.0.0.2/handler.py daemon`

3. SAP konak Aracısı SAP Not açıklandığı gibi yüklemeniz [1031096]ve çıktısını kontrol `saposcol`.

  a.  `/usr/sap/hostctrl/exe/saposcol -d` öğesini çalıştırın

  b.  `dump ccm` öğesini çalıştırın

  c.  Denetleme olup olmadığını **Virtualization_Configuration\Enhanced izleme erişim** ölçümüdür **doğru**.

Yüklü bir SAP NetWeaver ABAP uygulama sunucusu zaten varsa, işlem ST06 açın ve Gelişmiş izleme etkin olup olmadığını denetleyin.

Tüm bu denetimlerin başarısız olursa, ve uzantısı yeniden dağıtma hakkında ayrıntılı bilgi için bkz: [Azure izleme altyapısının SAP için sorun giderme][deployment-guide-5.3].

### <a name="e2d592ff-b4ea-4a53-a91a-e5521edb6cd1"></a>Sistem durumu izleme Azure altyapısı yapılandırmasını denetleyin
Bazı izleme, veri açıklanan test tarafından belirtildiği şekilde doğru şekilde teslim edilmedi [Gelişmiş Azure izlemesi için SAP için hazırlık denetimi][deployment-guide-5.1], çalışma `Test-AzureRmVMAEMExtension` altyapı ve izleme izleme Azure SAP uzantısı olup olmadığını doğru bir şekilde yapılandırıldığını denetlemek için cmdlet.

1.  Bölümünde açıklandığı gibi Azure PowerShell cmdlet'ini en son sürümünü yüklediğinizden emin olun [dağıtımı Azure PowerShell cmdlet'lerini][deployment-guide-4.1].
2.  Aşağıdaki PowerShell cmdlet’ini çalıştırın. Kullanılabilir ortamlar listesi için cmdlet'i çalıştırmak `Get-AzureRmEnvironment`. Genel Azure kullanmayı seçin **AzureCloud** ortamı. Çin'de Azure için seçin **AzureChinaCloud**.
  ```powershell
  $env = Get-AzureRmEnvironment -Name <name of the environment>
  Connect-AzureRmAccount -Environment $env
  Set-AzureRmContext -SubscriptionName <subscription name>
  Test-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
  ```

3.  Hesap verilerinizi girin ve Azure sanal makinesini tanımlayın.

  ![SAP özgü giriş sayfasında Azure cmdlet Test VMConfigForSAP_GUI][deployment-guide-figure-1200]

4. Komut dosyası seçtiğiniz sanal makinenin yapılandırmasını sınar.

  ![SAP için Azure izleme altyapısının başarılı test çıkışı][deployment-guide-figure-1300]

Her sistem durumu denetimi sonucu olduğundan emin olun **Tamam**. Bazı denetimleri değil görüntülerseniz **Tamam**, açıklandığı gibi güncelleştirme cmdlet'i çalıştırmak [Azure Gelişmiş izleme uzantısı SAP yapılandırma][deployment-guide-4.5]. 15 dakika bekleyin ve açıklanan denetimleri [Gelişmiş Azure izlemesi için SAP için hazırlık denetimi] [ deployment-guide-5.1] ve [Azure altyapı yapılandırma izlemelerine yönelik sistem durumu denetimi][deployment-guide-5.2]. Denetimleri hala bazı veya tüm sayaçları ile ilgili bir sorun gösteriyorsa, bkz: [Azure izleme altyapısının SAP için sorun giderme][deployment-guide-5.3].

### <a name="fe25a7da-4e4e-4388-8907-8abc2d33cfd8"></a>Azure izleme altyapısının SAP için sorun giderme

#### <a name="windowslogowindows-azure-performance-counters-do-not-show-up-at-all"></a>![Windows][Logo_Windows] Azure performans sayaçları hiç görünmüyor
AzureEnhancedMonitoring Windows hizmeti Azure performans ölçümleri toplar. Hizmet doğru şekilde yüklenmemiş veya VM ile çalışmıyorsa, performans ölçümleri toplanabilir.

##### <a name="the-installation-directory-of-the-azure-enhanced-monitoring-extension-is-empty"></a>Yükleme dizini Azure Gelişmiş izleme uzantısı'nın boştur

###### <a name="issue"></a>Sorun
Yükleme dizini C:\\paketleri\\eklentileri\\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;sürüm >\\açılan boştur.

###### <a name="solution"></a>Çözüm
Uzantısı yüklü değil. (Daha önce açıklandığı gibi) bu proxy sorunu olup olmadığını belirler. Makineyi yeniden başlatın veya yeniden gerekebilir `Set-AzureRmVMAEMExtension` yapılandırma komut dosyası.

##### <a name="service-for-azure-enhanced-monitoring-does-not-exist"></a>Gelişmiş Azure izlemesi için hizmet yok

###### <a name="issue"></a>Sorun
AzureEnhancedMonitoring Windows hizmeti yok.

Azperflib.exe çıktı bir hata oluşturur:

![Azperflib.exe yürütülmesi Azure Gelişmiş izleme uzantısı hizmeti SAP için çalışmadığını gösterir][deployment-guide-figure-1400]
<a name="figure-14"></a>

###### <a name="solution"></a>Çözüm
Hizmet mevcut değilse Azure Gelişmiş izleme uzantısı SAP doğru şekilde yüklenmedi. Dağıtım senaryonuz için açıklanan adımları kullanarak uzantısı dağıtmanız [VM'ler Azure SAP için dağıtım senaryoları][deployment-guide-3].

Bir saat sonra uzantısı dağıttıktan sonra yeniden Azure performans sayaçlarını Azure VM'yi sağlanan olup olmadığını denetleyin.

##### <a name="service-for-azure-enhanced-monitoring-exists-but-fails-to-start"></a>Gelişmiş Azure izlemesi için hizmeti var, ancak başlatılamıyor

###### <a name="issue"></a>Sorun
AzureEnhancedMonitoring Windows hizmeti var ve etkinleştirilir, ancak başlatılamıyor. Daha fazla bilgi için uygulama olay günlüğünü denetleyin.

###### <a name="solution"></a>Çözüm
Yapılandırması doğru değil. İzleme uzantısı VM için açıklandığı gibi yeniden [Azure Gelişmiş izleme uzantısı SAP yapılandırma][deployment-guide-4.5].

#### <a name="windowslogowindows-some-azure-performance-counters-are-missing"></a>![Windows][Logo_Windows] Bazı Azure performans sayaçları kayboluyor
AzureEnhancedMonitoring Windows hizmeti Azure performans ölçümleri toplar. Hizmet çeşitli kaynaklardan veri alır. Bazı yapılandırma verilerini yerel olarak toplanır ve bazı performans ölçümleri Azure tanılama salt okunurdur. Depolama sayaçları, depolama abonelik düzeyinde, oturum açmasını kullanılır.

SAP Not kullanarak sorun giderme olursa [1999351] sorunu çözümlemek için değil, yeniden `Set-AzureRmVMAEMExtension` yapılandırma komut dosyası. Hemen etkinleştirildikten sonra depolama analytics veya tanılama sayaçları oluşturulmuş olabilir değil çünkü bir saat beklemeniz gerekebilir. Sorun devam ederse, Windows için bileşen OP NT AZR BC veya BC-OP-LNX-AZR Linux sanal makine için bir SAP Müşteri Destek iletisini açın.

#### <a name="linuxlogolinux-azure-performance-counters-do-not-show-up-at-all"></a>![Linux][Logo_Linux] Azure performans sayaçları hiç görünmüyor
Azure performans ölçümlerini bir arka plan programı tarafından toplanır. Arka plan programı çalışmıyorsa, performans ölçümleri toplanabilir.

##### <a name="the-installation-directory-of-the-azure-enhanced-monitoring-extension-is-empty"></a>Gelişmiş Azure Monitoring uzantısı yükleme dizininin boştur

###### <a name="issue"></a>Sorun
Dizin \\var\\lib\\waagent\\ Gelişmiş Azure Monitoring uzantısı için bir alt yok.

###### <a name="solution"></a>Çözüm
Uzantısı yüklü değil. (Daha önce açıklandığı gibi) bu proxy sorunu olup olmadığını belirler. Makineyi yeniden başlatın ve/veya yeniden gerekebilir `Set-AzureRmVMAEMExtension` yapılandırma komut dosyası.

#### <a name="linuxlogolinux-some-azure-performance-counters-are-missing"></a>![Linux][Logo_Linux] Bazı Azure performans sayaçları kayboluyor
Azure'da performans ölçümleri, çeşitli kaynaklardan veri aldığı bir arka plan programı tarafından toplanır. Bazı yapılandırma verilerini yerel olarak toplanır ve bazı performans ölçümleri Azure tanılama salt okunurdur. Depolama sayaçları depolama aboneliğinizi günlüklerde gelmektedir.

Bilinen sorunların eksiksiz ve güncel listesi için bkz. SAP Not [1999351], Gelişmiş Azure izlemesi için SAP için ek sorun giderme bilgileri içeriyor.

SAP Not kullanarak sorun giderme olursa [1999351] yok sorunu çözün, yeniden `Set-AzureRmVMAEMExtension` açıklandığı gibi yapılandırma komut dosyası [Azure Gelişmiş izleme uzantısı SAP yapılandırma][deployment-guide-4.5]. Hemen etkinleştirildikten sonra depolama analytics veya tanılama sayaçları oluşturulmuş olabilir değil çünkü bir saat beklemeniz gerekebilir. Sorun devam ederse, Windows için bileşen OP NT AZR BC veya BC-OP-LNX-AZR Linux sanal makine için bir SAP Müşteri Destek iletisini açın.
