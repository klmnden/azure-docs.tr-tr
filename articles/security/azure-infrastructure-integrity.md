---
title: Azure altyapı bütünlüğü
description: Bu makalede, Azure altyapı bütünlüğü giderir.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 18d7fab30c0bbaa5292fe5d3003b7f2309b34d3b
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102689"
---
# <a name="integrity-of-azure-infrastructure"></a>Azure altyapı bütünlüğü   

## <a name="software-installation"></a>Yazılım yükleme
Azure ortamında yüklenen tüm yazılım yığınında Microsoft Security Development Lifecycle (SDL) işlem yerleşik özel bileşenleridir. Tüm yazılım bileşenleri (işletim sistemi görüntüleri ve SQL veritabanı dahil), değişiklik ve yayın Yönetimi işleminin bir parçası dağıtılır. Tüm düğümler üzerinde çalışan işletim sistemi Windows Server 2008 veya Windows Server 2012, özelleştirilmiş bir sürümüdür. Tam sürümünü yürütmek işletim sistemi için oranla role göre FC tarafından seçilir. Ayrıca, ana bilgisayar işletim sistemi herhangi yetkisiz yazılım bileşenlerini yükleme izin vermiyor.

Bazı Microsoft Azure bileşenlerinin (örneğin, RDFE, Geliştirici Portalı, vb.) Konuk konuk işletim sisteminde çalışan VM üzerinde Azure müşteri olarak dağıtılır.

## <a name="virus-scans-on-builds"></a>Derlemeleri virüs taramaları
Azure yazılım (işletim sistemi dahil) bileşeni derlemeleri Microsoft Endpoint Protection (MEP) virüsten koruma Aracı'nı kullanarak bir virüs taraması gitmek zorunda. Her virüs taraması ne taranan ayrıntılı ilişkili derleme dizini ve tarama sonuçlarını bir günlük oluşturur. Virüs tarama Azure içinde her bileşen için derleme kaynak kodu bir parçasıdır. Kod, tarama temiz ve başarılı bir virüs gerek kalmadan üretime taşınmaz. Not ettiğiniz herhangi bir sorun varsa, yapı dondurulmuş ve güvenlik takımlar "yanlış" kod derleme girmiş belirlemek için Microsoft Security içinde gider.

## <a name="closedlocked-environment"></a>Kapalı ve kilitli ortamı
Varsayılan olarak, Azure altyapı düğümleri ve Konuk VM'ler üzerinde oluşturulan herhangi bir kullanıcı hesabı yok. Ayrıca, varsayılan Windows yönetici hesapları da devre dışı bırakılır. Yöneticiler Microsoft Azure canlı destek (WALS) gelen düzgün bir kimlik doğrulaması ile – bu makinelerine oturum ve Acil Durum Onarım için Azure üretim ağı yönetme.

## <a name="microsoft-azure-sql-database-authentication"></a>Microsoft Azure SQL veritabanı kimlik doğrulaması
Herhangi bir uygulama SQL Server'ın gibi kullanıcı hesabı Yönetimi sıkı bir şekilde denetlenmesi gerekir. Microsoft Azure SQL veritabanı yalnızca SQL Server kimlik doğrulamasını destekler. Kullanıcı hesapları ile güçlü parolalar ve belirli bir ile yapılandırılmış hakları da Müşteri'nin veri güvenlik modelini tamamlar için kullanılmalıdır.

## <a name="firewallacls-between-msft-corpnet-and-microsoft-azure-cluster"></a>Güvenlik Duvarı/MSFT CorpNet ve Microsoft Azure küme arasında ACL'leri
Hizmet Platform ve MS şirket ağı arasında ACL'leri/güvenlik duvarı Microsoft Azure SQL veritabanı Insider yetkisiz erişimden koruyun. Ayrıca, yalnızca IP adres aralıklarını Microsoft CorpNet kullanıcılardan WinFabric platform Yönetimi uç noktasına erişebilirsiniz.

## <a name="firewallacls-between-nodes-in-an-azure-sql-db-cluster"></a>Güvenlik Duvarı/bir Azure SQL DB kümedeki düğümler arasında ACL'leri
Savunma bileşenini-stratejisinin, bir parçası olarak ek bir koruma olarak ACL'ler/güvenlik duvarı uygulanmıştır Microsoft Azure SQL DB kümedeki düğümler arasında. WinFabric platform küme içindeki tüm iletişimi yanı sıra tüm çalışan kodu güvenilmiyor.

## <a name="custom-mas-watchdogs"></a>Özel MAs (Watchdogs)
Microsoft Azure SQL veritabanı, Microsoft Azure SQL DB küme durumunu izlemek için özel MAs watchdogs adlı kullanır.

## <a name="web-protocols"></a>Web protokolleri

### <a name="role-instance-monitoring-and-restart"></a>Rol örneği izleme ve yeniden başlatma
Azure, dağıtılan tüm çalışan rolleri (Internet'e yönelik web veya arka uç işleme çalışan rolleri) etkili bir şekilde olduklarından emin olmak için izleme ve verimli bir şekilde, bunlar sağlanmış Hizmetleri ileterek sürekli sistem durumu tabi olmasını sağlar. Bir rol, rol örneği içinde barındırılan veya temel alınan yapılandırma sorunu olan uygulamada Kritik hata tarafından kötü, olur durumunda Microsoft Azure FC rol örneği içinde sorunu tespit ve düzeltme durumu başlatma .

### <a name="compute-connectivity"></a>Bağlantı işlem
Azure dağıtılan uygulama/hizmet standart web tabanlı protokolleri üzerinden erişilebilir olmasını sağlar. Internet'e yönelik web rolü sanal örneklerini dış Internet bağlantısına sahip olur ve doğrudan web kullanıcılar tarafından erişilebilir. Arka uç işleme çalışan rolü sanal örnekleri dış Internet bağlantısına sahip ancak duyarlılık ve çalışan rolleri adına gerçekleştirmek işlemleri bütünlüğünü korumak için doğrudan bir dış web kullanıcı tarafından erişilemiyor Genel olarak erişilebilir web rolü sanal örneği.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Azure altyapı güvenli hale getirmek için yaptığı hakkında daha fazla bilgi için bkz:

- [Azure olanakları, şirket içi ve fiziksel güvenlik](azure-physical-security.md)
- [Azure altyapı kullanılabilirliği](azure-infrastructure-availability.md)
- [Azure Information sistem bileşenleri ve sınırlar](azure-infrastructure-components.md)
- [Azure ağ mimarisi](azure-infrastructure-network.md)
- [Azure üretim ağı](azure-production-network.md)
- [Microsoft Azure SQL veritabanı güvenlik özellikleri](azure-infrastructure-sql.md)
- [Azure üretim işlemleri ve Yönetimi](azure-infrastructure-operations.md)
- [Azure altyapı izleme](azure-infrastructure-monitoring.md)
- [Azure müşteri verileri koruma](azure-protection-of-customer-data.md)
