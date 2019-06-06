---
title: Azure vm'lerde SAP kullanmaya başlama | Microsoft Docs
description: Microsoft Azure sanal makineler'de (VM) üzerinde çalışan SAP çözümleri hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: ad8e5c75-0cf6-4564-ae62-ea1246b4e5f2
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/05/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7539244429978f7cf639d2e49c532b5d19da385d
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66733455"
---
# <a name="use-azure-to-host-and-run-sap-workload-scenarios"></a>Barındırma ve çalıştırma SAP iş yükü senaryoları için Azure'ı kullanın

Microsoft Azure'u kullandığınızda, ölçeklenebilir, uyumlu ve kurumsal düzeyde kendini kanıtlamış bir platformda görev açısından kritik SAP iş yüklerinizi ve senaryoları güvenilir bir şekilde çalıştırabilirsiniz. Alırken ölçeklenebilirlik, esneklik ve maliyet tasarrufu Azure. Microsoft ve SAP arasındaki Genişletilmiş iş ortaklığı ile SAP uygulama yelpazesini azure'da geliştirme ve test ile üretim senaryolarında çalıştırabilir ve tam olarak desteklenir. SAP NetWeaver SAP S/4HANA, SAP BI Linux için Windows ve SAP HANA'dan SQL'e, için gelen biz hallederiz.

SAP NetWeaver farklı DBMS Azure senaryolarla barındırma yanı sıra, Azure üzerinde SAP BI gibi diğer SAP iş yükü senaryoları barındırabilirsiniz. 

Benzersiz SAP HANA için Azure, Azure parçalayın ayarlar bir tekliftir. Daha fazla bellek ve SAP HANA gerektiren CPU kaynak zorlu SAP senaryoları barındırmayı etkinleştirmek için Azure müşteri ayrılmış çıplak bilgisayar donanım kullanımını sunar. Bu çözüm, S/4HANA veya diğer SAP HANA iş yükü için en fazla 24 TB (120 TB genişleme) bellek gerektiren SAP HANA dağıtımları kullanın. 

Azure'da SAP iş yükü senaryoları barındırma kimlik tümleştirmesi ve çoklu oturum açma gereksinimlerini de oluşturabilirsiniz. Farklı SAP bileşenlerini bağladıktan ve hizmet olarak yazılım-a-(SaaS) veya hizmet olarak platform (PaaS) teklifleri SAP için Azure Active Directory (Azure AD) kullandığınızda, bu durum ortaya çıkabilir. Bu tür bir tümleştirme ve Azure AD ile çoklu oturum açma senaryoları listesini ve SAP varlıkları açıklanmış ve "AAD SAP kimlik tümleştirmesi ve çoklu oturum açma." bölümünde belgelenen

## <a name="latest-changes"></a>En son değişiklikleri

- ExpressRoute hızlı yolu ve Global erişim sunulmasıyla HANA büyük örnekler için [SAP HANA (büyük örnekler) ağ mimarisi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture) ve ilgili belgeler
- Sürümü [Azure HANA büyük örnekleri Azure Portalı aracılığıyla denetleme](hana-li-portal.md)
- Sürümü [SUSE Linux Enterprise Server SAP uygulamaları için Azure NetApp dosya çubuğunda Azure vm'lerinde SAP NetWeaver için yüksek kullanılabilirlik](high-availability-guide-suse-netapp-files.md)
- Açıklama **Linux işletim sistemi parametresi net.ipv4.tcp_timestamps** ayarları birlikte bir Azure yük dengeleyici






## <a name="sap-hana-on-azure-large-instances"></a>Azure’da SAP HANA (Büyük Örnekler)

Bir dizi belgeleri, SAP HANA azure'da (büyük örnekler) veya kısa, HANA büyük örnekler için yol gösterir. HANA büyük örnekleri aşağıdaki alanlara daha fazla bilgi için bkz:

- [SAP HANA (büyük örnekler) azure'da genel bakış](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [SAP hana (büyük örnekler) azure'da bir mimari](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-architecture)
- [Altyapı ve SAP HANA (büyük örnekler) azure'da bağlantısı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-infrastructure-connectivity)
- [SAP HANA (büyük örnekler) Azure'a yükleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-installation)
- [SAP hana (büyük örnekler) azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery)
- [Sorun giderme ve SAP HANA (büyük örnekler) azure'da izleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/troubleshooting-monitoring)

Sonraki adımlar:

- Okuma [genel bakış ve SAP hana (büyük örnekler) azure'da bir mimari](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)



## <a name="sap-hana-on-azure-virtual-machines"></a>Azure sanal makineler'de SAP HANA
Belgelerinin bu bölümü, SAP HANA farklı yönlerini kapsar. Bir önkoşul olarak Azure Iaas temel hizmetleri sağlayan asıl hizmetlerini Azure ile ilgili bilgi sahibi olması gerekir. Bu nedenle, Azure işlem, depolama ve ağ bilgisi gerekir. Bu konular birçoğu SAP NetWeaver ile ilgili işlenir [Azure Planlama Kılavuzu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide). 

Azure'da HANA hakkında daha fazla bilgi için aşağıdaki makaleleri ve bunların subarticles bakın:

- [Hızlı Başlangıç: Tek örnek SAP hana Azure vm'lerde el ile yükleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-get-started)
- [SAP S/4HANA veya BW/4hana'yı azure'da dağıtın](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h)
- [Azure'da SAP HANA altyapısı yapılandırmaları ve işlemleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations)
- [Azure sanal makineler için SAP HANA yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview)
- [Bir Azure bölgesi içinde SAP HANA kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-one-region)
- [Azure bölgeleri arasında SAP HANA kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions)
- [Azure sanal makineler'de SAP hana yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability)
- [Azure sanal makineler'de SAP HANA için yedekleme Kılavuzu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [SAP HANA dosya düzeyi Azure yedekleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [Depolama anlık görüntülerine dayalı SAP HANA yedeklemesi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)

 

## <a name="sap-netweaver-deployed-on-azure-virtual-machines"></a>Dağıtılan Azure sanal makinelerinde SAP NetWeaver
Bu bölümde, SAP NetWeaver ve azure'da bir iş planlama ve dağıtım belgelerini listelenir. Belge, azure'da bir SAP iş yükü ile temel bilgileri ve HANA olmayan veritabanlarının kullanımını odaklanır. Ayrıca, azure'da HANA yüksek kullanılabilirlik için temel yüksek kullanılabilirlik için makaleler ve belgeler gibi şunlardır:

- [Azure sanal makinelerinde SAP Business One](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/business-one-azure)
- [Azure'da SAP ERP 6.0 için SAP IDES EHP7 SP3'ı dağıtma](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-ides-erp6-erp7-sp3-sql)
- [Microsoft Azure SUSE Linux Vm'lerde SAP NetWeaver'ı çalıştırma](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/suse-quickstart)
- [Azure sanal makineleri planlama ve uygulama için SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide)
- [Azure sanal makineler dağıtım için SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide)
- [Site Recovery kullanarak çok katmanlı bir SAP NetWeaver uygulama dağıtımını koruma](https://docs.microsoft.com/azure/site-recovery/site-recovery-sap)
- [Azure için SAP LaMa bağlayıcısı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/lama-installation)

Azure'da bir SAP iş yükü altında olmayan HANA veritabanları hakkında daha fazla bilgi için bkz:

- [SAP iş yükü Azure sanal makineleri DBMS dağıtım konuları](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)
- [SAP NetWeaver için SQL Server Azure sanal makineleri DBMS dağıtım](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sqlserver)
- [SAP iş yükü için Oracle Azure Sanal Makineler DBMS dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_oracle)
- [SAP iş yükü için IBM DB2 Azure Sanal Makineler DBMS dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_ibm)
- [SAP iş yükü için SAP ASE Azure Sanal Makineler DBMS dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sapase)
- [Azure vm'lerde SAP MaxDB, dinamik önbellek ve içerik sunucusu dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_maxdb)

Azure üzerinde SAP HANA veritabanları hakkında daha fazla bilgi için "Azure sanal makineler'de SAP HANA." bölümüne bakın

Bir SAP iş yükü azure'da yüksek kullanılabilirlik hakkında daha fazla bilgi için bkz:

- [Azure sanal makineleri SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-start)

Bu belgede, çeşitli diğer mimari ve senaryo belgelere işaret eder. Sonraki senaryo belgelerde, dağıtımını ve yapılandırmasını farklı yüksek kullanılabilirlik yöntemleri açıklayan ayrıntılı teknik belgelere bağlantılar sağlanır. Linux ve Windows işletim sistemlerini oluşturmak ve bir SAP NetWeaver iş yükü için yüksek kullanılabilirliği yapılandırma işlemini gösteren farklı belgeleri kapsar.


Azure Active Directory (Azure AD) ve SAP hizmetlerini ve çoklu oturum açma arasında tümleştirme hakkında daha fazla bilgi için bkz:

- [Öğretici: Müşteri için SAP Cloud ile Azure Active Directory Tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-customer-cloud-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP Cloud Platform kimlik doğrulaması ile Azure Active Directory Tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP Cloud Platform ile Azure Active Directory Tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP NetWeaver ile Azure Active Directory Tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-netweaver-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP Business ByDesign ile Azure Active Directory Tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/sapbusinessbydesign-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP HANA ile Azure Active Directory Tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/saphana-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [S/4hana'yı ortamınızı: Fiori Launchpad SAML çoklu oturum açma Azure AD ile](https://blogs.sap.com/2017/02/20/your-s4hana-environment-part-7-fiori-launchpad-saml-single-sing-on-with-azure-ad/)

SAP bileşenleri Azure hizmetlerinin tümleştirme hakkında daha fazla bilgi için bkz:

- [Power BI Desktop’ta SAP HANA kullanma](https://docs.microsoft.com/power-bi/desktop-sap-hana)
- [DirectQuery ve SAP HANA](https://docs.microsoft.com/power-bi/desktop-directquery-sap-hana)
- [SAP BW Connector’ı Power BI Desktop’ta kullanma](https://docs.microsoft.com/power-bi/desktop-sap-bw-connector) 
- [Azure Data Factory, SAP HANA ve Business Warehouse veri tümleştirmesi sunuyor](https://azure.microsoft.com/blog/azure-data-factory-offer-sap-hana-and-business-warehouse-data-integration)




