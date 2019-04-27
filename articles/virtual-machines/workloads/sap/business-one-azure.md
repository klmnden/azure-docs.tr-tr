---
title: SAP Business bir on Azure sanal makineleri | Microsoft Docs
description: SAP iş üzerinde Azure.
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
ms.date: 07/15/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 101710b5a57faa37be77ff4b059fa0d494f4e617
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60835660"
---
# <a name="sap-business-one-on-azure-virtual-machines"></a>Azure Sanal Makineler'de SAP Business One
Bu belgede, SAP Business One Azure sanal Makineler'de dağıtma konusunda rehberlik sağlar. Belgeler yükleme belgelerine SAP iş yerine değil. Belgeler, bir iş uygulamaları çalıştırmak Azure altyapısı için temel planlama ve dağıtım yönergeleri kapsamalıdır.

İş bir, iki farklı veritabanında destekler:
- SQL Server - bkz [SAP notu #928839 - Microsoft SQL Server için yayın planlama](https://launchpad.support.sap.com/#/notes/928839)
- SAP HANA - SAP HANA, SAP Business One tam destek matrisi için kullanıma alma [SAP ürün kullanılabilirliği Matrisi](https://support.sap.com/pam)

SQL Server, temel dağıtım konuları açıklandığı gibi ilgili [SAP NetWeaver için Azure sanal makineleri DBMS dağıtım](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms-guide) uygular. SAP HANA için bu belgede konuları anlatılmaktadır.

## <a name="prerequisites"></a>Önkoşullar
Bu kılavuzu kullanmak için aşağıdaki Azure bileşenlerini temel bilgiye ihtiyacınız vardır:

- [Windows Azure sanal makinelerinde](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-manage-vm)
- [Linux üzerinde Azure sanal makineleri](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [PowerShell ile yönetim Azure ağ ve sanal ağlar](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-virtual-network)
- [Azure ağ ve CLI ile sanal ağlar](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure CLI ile Azure disklerini yönetme](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

Bir iş ilgilendiğiniz olsa bile yalnızca belgenin [planlama Azure sanal makineleri ve SAP NetWeaver uygulamasını](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide) iyi bir bilgi kaynağı olabilir.

SAP Business One'ı dağıtma örneği olarak, sahibi olduğunuz varsayılır:

- Bir VM gibi belirli bir altyapı üzerinde SAP HANA yüklemeyle ilgili bilgi sahibi
- Alışık olduğunuz Azure Vm'leri gibi bir altyapı üzerinde SAP Business One uygulama yükleme
- SAP Business One'ı ve seçilen DBMS sistemi işletim aşina
- Hakkında bilginiz altyapısını Azure'a dağıtma

Bu alanlar, bu belgede ele alınmamaktadır.

Azure belgeleri yanı sıra ana SAP, iş için bir başvuru veya bir işletme için SAP central notları olduğu notları, haberdar olmanız gerekir:

- [528296 - bir işletme sürümleri, SAP ve ilgili ürünler için genel bakış Not genel](https://launchpad.support.sap.com/#/notes/528296)
- [2216195 - SAP Business bir 9.2, SAP HANA için sürüm için sürüm güncelleştirmeleri Not](https://launchpad.support.sap.com/#/notes/2216195)
- [2483583 - SAP iş Orta Not bir 9.3](https://launchpad.support.sap.com/#/notes/2483583)
- [2483615 - sürüm güncelleştirmeleri Not SAP iş için bir 9.3](https://launchpad.support.sap.com/#/notes/2483615)
- [2483595 - SAP iş toplu Not bir 9.3 genel sorunlar](https://launchpad.support.sap.com/#/notes/2483595)
- [2027458 - toplu danışmanlık Not SAP HANA-Related konular, SAP Business One, SAP HANA için sürümü için](https://launchpad.support.sap.com/#/notes/2027458)


## <a name="business-one-architecture"></a>İş bir mimarisi
İş bir iki katmanları olan bir uygulamadır:

- 'Fat' istemcisi ile bir istemci katmanı
- Bir kiracı için veritabanı şeması içeren bir veritabanı katmanı

Bileşenler istemci bölümünde çalıştıran ve bölümleri sunucusu bölümün çalıştığını daha iyi bir genel bakış belgelenen [SAP iş bir Yönetici Kılavuzu](https://help.sap.com/http.svc/rc/879bd9289df34a47af838e67d74ea302/9.3/en-US/AdministratorGuide_SQL.pdf) 

İstemci katmanı ve DBMS katmanı arasındaki ağır gecikme süresi kritik etkileşim olduğundan, her iki katmanda Azure'da dağıtırken Azure'da bulunması gerekir. Kullanıcılar daha sonra bir RDS çalıştıran bir veya birden çok sanal makine içine RDS iş bir istemci bileşenleri için hizmet normal.

### <a name="sizing-vms-for-sap-business-one"></a>VM'ler için SAP iş boyutlandırma bir

VM istemci boyutlandırma ile ilgili, kaynak gereksinimlerini belgelenmiştir SAP tarafından belgede [SAP iş bir donanım gereksinimleri Kılavuzu](https://help.sap.com/http.svc/rc/011000358700000244612011e/9.3/en-US/B1_Hardware_Requirements_Guide.pdf). Azure için odaklanın ve belgenin 2.4 bölümde belirtilen gereksinimleri olan hesapla gerekir.

Azure iş bir istemci bileşenlerini ve DBMS konak barındırmak için sanal, yalnızca SAP NetWeaver'ın desteklenen VM'ler izin verilir. Desteklenen Azure Vm'lerinde SAP NetWeaver listesi bulmak için okuma [SAP notu #1928533](https://launchpad.support.sap.com/#/notes/1928533).

SAP HANA için bir iş, yalnızca VM, HANA iş için listelenen DBMS arka uç olarak çalıştırmayı [HANA certifeid Iaas platform listesine](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure%23SAP%20Business%20One) HANA için desteklenir. İş bir istemci bileşenleri için SAP HANA DBMS sistemi olarak daha güçlü bu kısıtlama etkilenmez.

### <a name="operating-system-releases-to-use-for-sap-business-one"></a>SAP Business One için kullanılacak işletim sistemi sürümleri

İlkesi, her zaman en son işletim sistemi sürümleri kullanmak en iyisidir. Özellikle Linux alanında farklı daha yeni ikincil sürümleri Suse ve Red Hat ile yeni Azure işlevi sunulmuştur. Windows tarafında, Windows Server 2016'yı kullanarak kesinlikle önerilir.


## <a name="deploying-infrastructure-in-azure-for-sap-business-one"></a>Altyapısını Azure'a SAP Business One için dağıtma
Sonraki birkaç bölümde, sorgunuzun SAP dağıtımı için altyapı parça.

### <a name="azure-network-infrastructure"></a>Azure ağ altyapısı
Ağ altyapısını Azure'da dağıtmak için ihtiyacınız olup, tek bir iş bir sistem kendiniz dağıttığınız üzerinde bağlıdır. Veya müşteriler için bir iş sistemleri onlarca barındıran bir barındırma olup. Ayrıca olabilir hafif değişiklikler tasarımında olup Azure'a bağlanma şeklinize üzerinde. Farklı olanaklar giderek, birinci tasarım, azure'a bir VPN bağlantısı olan ve Active Directory aracılığıyla genişletmek burada [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design) veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) azure'a.

![İş bir basit ağ yapılandırması](./media/business-one-azure/simple-network-with-VPN.PNG)

Sunulan Basitleştirilmiş yapılandırma denetimi ve yönlendirme sınırını sağlayan birkaç güvenlik örnekleri sunar. İle başlar 

- Yönlendirici/güvenlik duvarı müşteri şirket tarafında.
- Sonraki örnek [Azure ağ güvenlik grubu](https://docs.microsoft.com/azure/virtual-network/security-overview) SAP Business bir yapılandırmanızı çalıştıran Azure sanal ağı için yönlendirmeyi ve güvenlik kuralları tanıtmak için kullanabilirsiniz.
- İş bir istemci kullanıcılarının önlemek için de veritabanı çalıştıran bir iş sunucusu çalıştıran sunucuda görebilirsiniz, iş bir istemci ve sanal ağ içindeki iki farklı alt ağlarda iş tek bir sunucuyu barındırma VM ayırmalısınız.
- İş bir sunucusuna erişimi sınırlamak için iki farklı alt ağlara atanan Azure NSG tekrar kullanırsınız.

Bir Azure ağı yapılandırması daha karmaşık bir sürümü, Azure tabanlı [merkez ve uç mimarisi en iyi belgelenmiş](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke). Merkez ve uç mimarisi desenini ilk Basitleştirilmiş yapılandırma bir şöyle değiştirin:


![Bir iş ile merkez ve uç yapılandırması](./media/business-one-azure/hub-spoke-network-with-VPN.PNG)

Burada kullanıcıları bağlanırken azure'a herhangi bir özel bağlantı olmadan internet üzerinden durumlarda, Azure başvuru mimarisi için belirtilmiştir ilkeleri ile azure'da ağ tasarımı hizalanmalıdır [Azure arasında DMZ ve Internet](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-dmz).

### <a name="business-one-database-server"></a>İş bir veritabanı sunucusu
SQL Server ve SAP HANA veritabanı türü için kullanılabilir. Bağımsız olarak DBMS, belge kimler [SAP iş yükü Azure sanal makineleri DBMS dağıtım konuları](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general) DBMS dağıtımları azure Vm'leri ve ilişkili ağ ve depolama alanlarındaki genel bir anlayış edinmek için konuları.

Özel ve genel veritabanı belgeleri zaten vurgulanmış olsa, kendiniz hakkında bilgi sahibi olmanız gerekir:

- [Azure'daki Windows sanal makinelerin kullanılabilirliğini yönetme](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) ve [Azure Linux sanal makinelerinin kullanılabilirliğini yönetme](https://docs.microsoft.com/azure/virtual-machines/linux/manage-availability)
- [Sanal makineler için SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/)

Bu belgeleri depolama türleri ve yüksek kullanılabilirlik yapılandırması seçimine karar vermenize yardımcı olmalıdır.

İlkesi şunları yapmalısınız:

- Premium SSD üzerinde standart HDD'ler kullanın. Kullanılabilir disk türleri hakkında daha fazla bilgi için bkz: makalemizi [bir disk türü seçin](../../windows/disks-types.md)
- Yönetilmeyen diskler üzerinde Azure yönetilen diskler kullanın
- Disk yapılandırmanızı ile yapılandırılmış, yeterli IOPS ve g/ç aktarım hızı sahip olduğunuzdan emin olun
- Maliyet açısından verimli depolama yapılandırması için/hana/birime veri ve /hana/log birleştirin


#### <a name="sql-server-as-dbms"></a>DBMS olarak SQL Server
SQL Server DBMS bir iş için dağıtmak için belge Git [SAP NetWeaver için SQL Server Azure sanal makineleri DBMS dağıtım](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sqlserver). 

SQL Server için DBMS yan kaba boyutlandırma tahminleri şunlardır:

| Kullanıcı sayısı | vCPU sayısı | Bellek | Örnek VM türleri |
| --- | --- | --- | --- |
| en fazla 20 | 4 | 16 GB | D4s_v3, E4s_v3 |
| en fazla 40 | 8 | 32 GB | D8s_v3, E8s_v3 |
| 80 | 16 | 64 GB | D16s_v3, E16s_v3 |
| en fazla 150 | 32 | 128 GB | D32s_v3, E32s_v3 |

Yukarıda listelenen boyutlandırma hakkında bir fikir başlamak burada vermeniz gerekir. Azure'da bir uyarlama kolaydır bu durumda daha az veya daha fazla kaynak gerekiyor olabilir. VM türleri arasındaki bir değişiklik, yalnızca sanal Makinenin yeniden başlatılması ile mümkündür.

#### <a name="sap-hana-as-dbms"></a>SAP HANA DBMS olarak
SAP HANA, DBMS aşağıdaki bölümleri kullanarak belge ilişkin konular izlemelidir [Azure işlemler kılavuzu üzerinde SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations).

Yüksek kullanılabilirlik ve iş bir Azure için SAP HANA veritabanı geçici olarak olağanüstü durum kurtarma yapılandırmaları için belgelere okumalısınız [Azure sanal makineler için SAP HANA yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview) ve belgeler Bu belgeden işaret.

SAP HANA için yedekleme ve geri yükleme stratejileri, belge okumalıdır [SAP HANA için yedekleme Kılavuzu Azure sanal Makineler'de](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide) ve belgeleri yönlendirmekteydi bu belgeden.

 
### <a name="business-one-client-server"></a>İş bir istemci sunucu
Bu bileşenler için depolama konuları birincil söz konusu değildir. Bununla birlikte, güvenilir bir platforma sahip istiyorsunuz. Bu nedenle, bu VM için Azure Premium depolama için bile temel VHD kullanmalısınız. Verilen veri ile VM boyutlandırma [SAP iş bir donanım gereksinimleri Kılavuzu](https://help.sap.com/http.svc/rc/011000358700000244612011e/9.3/en-US/B1_Hardware_Requirements_Guide.pdf). Azure için odaklanın ve belgenin 2.4 bölümde belirtilen gereksinimleri olan hesapla gerekir. Gereksinimlerini hesaplamak gibi bunları ideal VM bulmak için aşağıdaki belgelere karşılaştırma yapmanız gerekir:

- [Azure'da Windows sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)
- [SAP notu 1928533 #](https://launchpad.support.sap.com/#/notes/1928533)

CPU ve Microsoft tarafından belirtildiği için ihtiyaç duyulan bellek sayısı ile karşılaştırın. Ayrıca ağ aktarım hızı, Vm'leri seçerken göz önünde bulundurun.








