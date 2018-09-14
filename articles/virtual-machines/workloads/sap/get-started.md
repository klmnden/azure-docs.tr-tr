---
title: Azure vm'lerde SAP kullanmaya başlama | Microsoft Docs
description: Microsoft Azure sanal makineler'de (VM) üzerinde çalışan SAP çözümleri hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: ad8e5c75-0cf6-4564-ae62-ea1246b4e5f2
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/13/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0cf6291cb12366c4f710092c1b36c8cb0b8c14fb
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45578404"
---
# <a name="using-azure-for-hosting-and-running-sap-workload-scenarios"></a>Barındırma ve SAP iş yükü senaryoları çalıştırmak için Azure'ı kullanma

Microsoft Azure, SAP kullanılmaya hazır bir bulut iş ortağı olarak seçerek, senaryoları ve görev açısından kritik SAP iş yüklerini ölçeklenebilir, uyumlu ve kurumsal düzeyde kendini kanıtlamış platformunda güvenilir bir şekilde çalıştırmak kullanabilirsiniz.  Azure’un sunduğu ölçeklenebilirlik, esneklik ve maliyet tasarrufu olanaklarından yararlanın. Microsoft ve SAP arasındaki Genişletilmiş iş ortaklığı ile SAP uygulama yelpazesini - azure'da geliştirme/test ve üretim senaryolarında çalıştırabilir ve tam olarak desteklenir. SAP Netweaver'dan SAP S4/HANA, SAP BI, Linux için Windows, SAP HANA'dan SQL'e, karşılıyoruz.

SAP NetWeaver farklı DBMS Azure senaryolarla barındırma yanı sıra, farklı barındırabilirsiniz Azure üzerinde SAP BI gibi diğer SAP iş yükü senaryoları. Azure yerel sanal makinelerinde SAP NetWeaver dağıtımları ile ilgili belgeler, "Azure sanal makinelerinde SAP NetWeaver." bölümünde bulunabilir

Benzersiz SAP HANA için Azure, Azure dışında yarışma ayarlar benzersiz bir tekliftir. Daha fazla bellek ve CPU kaynağı zorlu SAP HANA, Azure teklifleri içeren SAP senaryoları barındırma müşteri kullanımını etkinleştirmek için 20 TB'a kadar (60 TB ölçek genişletme) için bellek gerektiren SAP HANA dağıtımlarını yürüten amacıyla çıplak bilgisayar donanım ayrılmış. S/4HANA veya diğer SAP HANA iş yükü. Bu benzersiz Azure çözüm SAP hana (büyük örnekler) Azure üzerinde SAP HANA SAP uygulama katmanında veya yerel Azure sanal Makineler'de barındırılan iş yükü donanımlar orta katman ile özel bir çıplak bilgisayar donanım çalıştırmanıza olanak tanır. Bu çözüm, çeşitli belgelerde "SAP HANA (büyük örnekler) azure'da." bölümünde belgelenmiştir   

Azure'da SAP iş yükü senaryoları barındırma kimlik tümleştirmesi ve çoklu oturum açma Azure Activity Directory farklı SAP bileşenleri ile SAP SaaS gereksinimlerini de oluşturabilir veya PaaS sunar. Söz konusu tümleştirmesi ve çoklu oturum açma senaryoları ile Azure Active Directory (AAD) ve SAP varlık listesini açıklanmış ve bölümünde belgelenen "AAD SAP kimlik tümleştirmesi ve çoklu oturum açma."

## <a name="latest-changes"></a>En son değişiklikleri

SAP HANA dinamik Azure Vm'leri için katmanlama etrafında belgeleri

- [Azure'da SAP HANA altyapısı yapılandırmaları ve işlemleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations#sap-hana-dynamic-tiering-20-for-azure-virtual-machines)

Azure sanal makine M128s üzerinde SAP HANA ölçeği genişletilmiş etrafında belgeleri eklenen:

- [Azure'da SAP HANA altyapısı yapılandırmaları ve işlemleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations#configuring-azure-infrastructure-for-sap-hana-scale-out)
- [Bir Azure bölgesi içinde SAP HANA kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-one-region)


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
Belgelerinin bu bölümü, SAP HANA farklı yönlerini kapsar. Bir önkoşul olarak Azure Iaas, çoğunlukla Azure işlem, depolama ve ağ bilgisini temel hizmetleri sağlayan asıl hizmetlerini Azure ile ilgili bilgi sahibi olması gerekir. Çok sayıda Bu konular, SAP NetWeaver ilgili işlenir [Azure Planlama Kılavuzu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide). 

Azure'da HANA için özel belgelere Bu makaleler ve onların alt makaleleri listesinden oluşur:

- [Hızlı Başlangıç: Azure sanal makinelerinde tek örnek SAP hana el ile yükleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-get-started)
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
Bu bölümde, SAP NetWeaver ve azure'da bir iş planlama ve dağıtım belgelerini bulabilirsiniz. Bu bölümdeki belgelere, Azure'da temel bilgileri ve iş yüküyle SAP HANA veritabanlarının kullanım etrafında çoğunlukla odaklanmıştır. Ha makaleler ve belgeler foundation HANA yüksek kullanılabilirlik için azure'da da ise. makale listesi gibi:

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

Azure'da SAP iş yükü yüksek kullanılabilirlik için aşağıdaki belgeleri kullanılabilir:

- [Azure sanal makineleri SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-start)
- [Yüksek kullanılabilirlik mimarisi ve senaryolar için SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios)
- ["Yüksek kullanılabilirliği" bir SAP sistemiyle elde etmek üzere Azure altyapı VM yeniden kullanma](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-higher-availability-architecture-scenarios)
- [Azure'da bir küme paylaşılan disk kullanarak bir Windows Yük devretme kümesinde Küme SAP ASCS/SCS örneği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)
- [SAP ASCS/SCS örneği ile Azure dosya paylaşımı kullanarak bir Windows Yük devretme kümesinde Küme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share)
- [SAP uygulamaları için SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)
- [Azure altyapı SAP yüksek kullanılabilirlik için bir Windows Yük devretme kümesi ve paylaşılan disk SAP ASCS/SCS kullanarak hazırlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-shared-disk)
- [SAP ASCS/SCS örneği için bir Windows Yük devretme kümesi ve dosya paylaşımı kullanarak SAP yüksek kullanılabilirlik için Azure altyapısını hazırlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share)
- [SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde NFS için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs)
- [SLES azure'daki SUSE Linux Enterprise Server üzerinde Pacemaker ayarlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker)
- [https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-installation-wsfc-shared-disk)
- [Azure üzerinde SAP ASCS/SCS örneği için bir Windows Yük devretme kümesi ve dosya paylaşım SAP NetWeaver-yüksek kullanılabilirlik yükleyin](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-installation-wsfc-file-share)
- [SAP uygulamaları için SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)
- [Paylaşılan disk azure'da Windows Server Yük Devretme Kümelemesi ve yüksek kullanılabilirlikle çoklu SID SAP ASCS/SCS örneği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ascs-ha-multi-sid-wsfc-shared-disk)
- [Azure'da Windows Server Yük Devretme Kümelemesi ve dosya paylaşımı ile yüksek kullanılabilirlik çoklu SID SAP ASCS/SCS örneği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ascs-ha-multi-sid-wsfc-file-share)


Azure Active Directory (AAD) ile SAP hizmetleri arasında tümleştirme için ve çoklu oturum açma, gibi belgelerin listesi:

- [Öğretici: Müşteri için SAP bulut Azure Active Directory Tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-customer-cloud-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP Cloud Platform kimlik doğrulaması ile Azure Active Directory Tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP Cloud Platform ile Azure Active Directory Tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP NetWeaver ile Azure Active Directory Tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-netweaver-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP Business ByDesign ile Azure Active Directory Tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/sapbusinessbydesign-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP HANA ile Azure Active Directory Tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/saphana-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [– Fiori Launchpad SAML çoklu oturum açma Azure AD ile S/4hana'yı ortamınızı](https://blogs.sap.com/2017/02/20/your-s4hana-environment-part-7-fiori-launchpad-saml-single-sing-on-with-azure-ad/)

SAP bileşenleri Azure Hizmetleri'nin tümleştirilmesi için belgelerin listesini şuna benzer:

- [Power BI Desktop'ta SAP HANA kullanma](https://docs.microsoft.com/power-bi/desktop-sap-hana)
- [DirectQuery ve SAP HANA](https://docs.microsoft.com/power-bi/desktop-directquery-sap-hana)
- [Power BI Desktop'ta SAP BW bağlayıcısını kullanma](https://docs.microsoft.com/power-bi/desktop-sap-bw-connector) 
- [Azure Data Factory SAP HANA ve Business Warehouse veri tümleştirme sunar.](https://azure.microsoft.com/blog/azure-data-factory-offer-sap-hana-and-business-warehouse-data-integration)




