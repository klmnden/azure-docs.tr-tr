---
title: Azure vm'lerde SAP kullanmaya başlama | Microsoft Docs
description: Microsoft Azure sanal makineler'de (VM) üzerinde çalışan SAP çözümleri hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: ad8e5c75-0cf6-4564-ae62-ea1246b4e5f2
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/01/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 40ed06bef45948068e3845e728d9c1d63ed62e71
ms.sourcegitcommit: 09bb15a76ceaad58517c8fa3b53e1d8fec5f3db7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58762812"
---
# <a name="using-azure-for-hosting-and-running-sap-workload-scenarios"></a>Barındırma ve SAP iş yükü senaryoları çalıştırmak için Azure'ı kullanma

Microsoft Azure'ı seçerek, senaryoları ve görev açısından kritik SAP iş yüklerini ölçeklenebilir, uyumlu ve kurumsal düzeyde kendini kanıtlamış platformunda güvenilir bir şekilde çalıştırmak kullanabilirsiniz.  Azure’un sunduğu ölçeklenebilirlik, esneklik ve maliyet tasarrufu olanaklarından yararlanın. Microsoft ve SAP arasındaki Genişletilmiş iş ortaklığı ile SAP uygulama yelpazesini - azure'da geliştirme/test ve üretim senaryolarında çalıştırabilir ve tam olarak desteklenir. Windows, SQL, SAP HANA için SAP S/4HANA, SAP BI Linux'a SAP NetWeaver'nden karşılıyoruz.

SAP NetWeaver farklı DBMS Azure senaryolarla barındırma yanı sıra, farklı barındırabilirsiniz Azure üzerinde SAP BI gibi diğer SAP iş yükü senaryoları. 

Benzersiz SAP HANA için Azure, Azure dışında yarışma ayarlar bir tekliftir. Daha fazla bellek ve CPU kaynağı zorlu SAP HANA, Azure teklifleri içeren SAP senaryoları barındırma müşteri kullanımını etkinleştirmek için en fazla 24 TB (120 TB genişleme) bellek gerektiren SAP HANA dağıtımlarını yürüten amacıyla çıplak bilgisayar donanım ayrılmış S/4HANA veya diğer SAP HANA iş yükü için. Bu benzersiz Azure çözüm SAP hana (büyük örnekler) Azure üzerinde SAP HANA SAP uygulama katmanında veya yerel Azure sanal Makineler'de barındırılan iş yükü donanımlar orta katman ile özel bir çıplak bilgisayar donanım çalıştırmanıza olanak tanır. Bu çözüm, çeşitli belgelerde "SAP HANA (büyük örnekler) azure'da." bölümünde belgelenmiştir   

Azure'da SAP iş yükü senaryoları barındırma kimlik tümleştirmesi ve çoklu oturum açma Azure Activity Directory farklı SAP bileşenleri ile SAP SaaS gereksinimlerini de oluşturabilir veya PaaS sunar. Söz konusu tümleştirmesi ve çoklu oturum açma senaryoları ile Azure Active Directory (AAD) ve SAP varlık listesini açıklanmış ve bölümünde belgelenen "AAD SAP kimlik tümleştirmesi ve çoklu oturum açma."

## <a name="latest-changes"></a>En son değişiklikleri

Sürümü [Azure HANA büyük örnekleri, Azure Portalı aracılığıyla denetleme](hana-li-portal.md)

Sürümü [SUSE Linux Enterprise Server SAP uygulamaları için Azure NetApp dosya çubuğunda Azure vm'lerinde SAP NetWeaver için yüksek kullanılabilirlik](high-availability-guide-suse-netapp-files.md)

Açıklama **Linux işletim sistemi parametresi net.ipv4.tcp_timestamps** ayarları Azure ile birlikte yük dengeleyici

Sürümü [Azure kullanılabilirlik alanları ile SAP iş yükü yapılandırmaları](sap-ha-availability-zones.md)

Sürümü [SAP iş yükü planlama ve dağıtım denetim listesi](sap-deployment-checklist.md)




## <a name="sap-hana-on-sap-hana-on-azure-large-instances"></a>SAP HANA (büyük örnekler) Azure üzerinde SAP HANA

Bir dizi belgeleri (büyük örnekler) Azure üzerinde SAP HANA müşteri adayları veya, HANA büyük örnekleri kısa. Belgeler, HANA büyük örnekleri listelenen alanları kapsar:

- [SAP HANA (büyük örnekler) azure'da genel bakış](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [SAP hana (büyük örnekler) azure'da bir mimari](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-architecture)
- [Altyapı ve SAP HANA (büyük örnekler) azure'da bağlantısı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-infrastructure-connectivity)
- [SAP HANA (büyük örnekler) Azure üzerinde SAP HANA yükleyin](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-installation)
- [Yüksek kullanılabilirlik ve olağanüstü durum kurtarma (büyük örnekler) Azure üzerinde SAP hana](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery)
- [Sorun giderme ve SAP hana (büyük örnekler) azure'da izleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/troubleshooting-monitoring)

Sonraki adımlar:

- Okuma [genel bakış ve SAP hana (büyük örnekler) azure'da bir mimari](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)



## <a name="sap-hana-on-azure-virtual-machines"></a>Azure Sanal Makinelerde SAP HANA
Belgelerinin bu bölümü, SAP HANA farklı yönlerini kapsar. Bir önkoşul olarak Azure Iaas, çoğunlukla Azure işlem, depolama ve ağ bilgisini temel hizmetleri sağlayan asıl hizmetlerini Azure ile ilgili bilgi sahibi olması gerekir. Bu konular birçoğu SAP NetWeaver ilgili işlenir [Azure Planlama Kılavuzu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide). 

Azure'da HANA için özel belgelere Bu makaleler ve bunların subarticles listesinden oluşur:

- [Hızlı Başlangıç: Tek örnek SAP hana Azure vm'lerde el ile yükleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-get-started)
- [SAP S/4HANA veya BW/4hana'yı azure'da dağıtın](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h)
- [Azure'da SAP HANA altyapısı yapılandırmaları ve işlemleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations)
- [Azure sanal makineler için SAP HANA yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview)
- [Bir Azure bölgesi içinde SAP HANA kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-one-region)
- [Azure bölgeleri arasında SAP HANA kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions)
- [Azure sanal makineler'de SAP hana yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability)
- [SAP HANA için yedekleme Kılavuzu Azure sanal Makineler'de](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [SAP HANA dosya düzeyi Azure yedekleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [Depolama anlık görüntülerine dayalı SAP HANA yedeklemesi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)

 

## <a name="sap-netweaver-deployed-on-azure-virtual-machines"></a>Dağıtılan Azure sanal Makineler'de SAP NetWeaver
Bu bölümde, SAP NetWeaver ve azure'da bir iş planlama ve dağıtım belgelerini bulun. Bu bölümdeki belgelere, Azure'da temel bilgileri ve iş yüküyle SAP HANA veritabanlarının kullanım etrafında çoğunlukla odaklanmıştır. Ha makaleler ve belgeler foundation HANA yüksek kullanılabilirlik için azure'da da ise. makale listesi gibi:

- [Azure Sanal Makineler üzerinde SAP Business One](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/business-one-azure)
- [Azure'da SAP ERP 6.0 için SAP IDES EHP7 SP3'ı dağıtma](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-ides-erp6-erp7-sp3-sql)
- [Microsoft Azure SUSE Linux vm'lerde SAP NetWeaver çalıştırma](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/suse-quickstart)
- [Azure sanal makineleri planlama ve uygulama için SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide)
- [Azure sanal makineler dağıtım için SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide)
- [Site Recovery kullanarak çok katmanlı SAP NetWeaver uygulama dağıtımını koruma](https://docs.microsoft.com/azure/site-recovery/site-recovery-sap)
- [Azure için SAP LaMa bağlayıcısı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/lama-installation)

Azure'da SAP iş yükü altında olmayan HANA veritabanları ile ilgili belgeler listesi ister:

- [SAP iş yükü Azure sanal makineleri DBMS dağıtım konuları](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)
- [SAP NetWeaver için SQL Server Azure sanal makineleri DBMS dağıtım](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sqlserver)
- [SAP iş yükü için Oracle Azure Sanal Makineler DBMS dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_oracle)
- [SAP iş yükü için IBM DB2 Azure Sanal Makineler DBMS dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_ibm)
- [SAP iş yükü için SAP ASE Azure Sanal Makineler DBMS dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sapase)
- [SAP MaxDB liveCache ve Azure Vm'leri üzerinde içerik sunucusu dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_maxdb)

Azure üzerinde SAP HANA veritabanları için Azure sanal Makineler'de SAP HANA bölümüne bakın.

Azure'da SAP iş yükü yüksek kullanılabilirlik için giriş belgesidir:

- [Azure sanal makineleri SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-start)

Giriş belgesi, çeşitli diğer mimari ve senaryo belgelere işaret eder. Sonraki senaryo belgeleri, dağıtımını ve yapılandırmasını farklı yüksek kullanılabilirlik yöntemleri açıklayan ayrıntılı teknik belgelere bağlantılar sağlanır. Windows işletim sistemlerinin yanı sıra Linux kapsayıp kurma ve SAP NetWeaver iş yükü için yüksek kullanılabilirlik yapılandırma farklı belgeler.


Azure Active Directory (AAD) ile SAP hizmetleri arasında tümleştirme için ve çoklu oturum açma, gibi belgelerin listesi:

- [Öğretici: Müşteri için SAP Cloud ile Azure Active Directory Tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-customer-cloud-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP Cloud Platform kimlik doğrulaması ile Azure Active Directory Tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP Cloud Platform ile Azure Active Directory Tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP NetWeaver ile Azure Active Directory Tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-netweaver-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP Business ByDesign ile Azure Active Directory Tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/sapbusinessbydesign-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP HANA ile Azure Active Directory Tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/saphana-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [– Fiori Launchpad SAML çoklu oturum açma Azure AD ile S/4hana'yı ortamınızı](https://blogs.sap.com/2017/02/20/your-s4hana-environment-part-7-fiori-launchpad-saml-single-sing-on-with-azure-ad/)

SAP bileşenleri Azure Hizmetleri'nin tümleştirilmesi için belgelerin listesini şuna benzer:

- [Power BI Desktop’ta SAP HANA kullanma](https://docs.microsoft.com/power-bi/desktop-sap-hana)
- [DirectQuery ve SAP HANA](https://docs.microsoft.com/power-bi/desktop-directquery-sap-hana)
- [SAP BW Connector’ı Power BI Desktop’ta kullanma](https://docs.microsoft.com/power-bi/desktop-sap-bw-connector) 
- [Azure Data Factory, SAP HANA ve Business Warehouse veri tümleştirmesi sunuyor](https://azure.microsoft.com/blog/azure-data-factory-offer-sap-hana-and-business-warehouse-data-integration)




