---
title: Azure altyapı bütünlüğü
description: Bu makalede, Azure altyapı bütünlüğü yöneliktir.
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
ms.date: 07/06/2018
ms.author: terrylan
ms.openlocfilehash: 867bc66a68bec662153d8336e649cf46df02f101
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37901325"
---
# <a name="azure-infrastructure-integrity"></a>Azure altyapı bütünlüğü

## <a name="software-installation"></a>Yazılım yükleme
Azure ortamında yüklenen tüm yazılım yığınında özel Microsoft'un güvenlik geliştirme yaşam döngüsü (SDL) işlem oluşturulan bileşenlerdir. (İşletim sistemi görüntüleri ve SQL veritabanı dahil) tüm yazılım bileşenleri, değişiklik ve sürüm Yönetimi işleminin bir parçası dağıtılır. Tüm düğümleri üzerinde çalışan işletim sistemi, Windows Server 2008 veya Windows Server 2012, özelleştirilmiş bir sürümüdür. Tam sürümünü, FC yürütmek işletim sistemi için düşünüyor role göre seçilir. Ayrıca, ana bilgisayar işletim sistemi tüm yetkisiz yazılım bileşenlerini yükleme izin vermez.

Bazı Microsoft Azure bileşenlerinin (örneğin, RDFE, Geliştirici Portalı, vb.), Konuk VM konuk işletim sistemi üzerinde çalışan Azure müşterilerinin olarak dağıtılır.

## <a name="virus-scans-on-builds"></a>Virüs tarama derlemelerinde
Azure yazılım bileşeni (işletim sistemi dahil) derlemeleri, Microsoft uç nokta koruma (MEP) virüsten koruma Aracı'nı kullanarak bir virüs taraması gitmek zorunda. Her bir virüs taraması ne taranan gerçekleşen ilişkili derleme dizini ve tarama sonuçlarını bir günlük oluşturur. Virüs taraması azure'daki her bileşeni için derleme kaynak kodu bir parçasıdır. Kod, tarama temiz ve başarılı bir virüs zorunda kalmadan üretime taşınmaz. Belirtilen herhangi bir sorun varsa, yapı dondurulmuş ve ardından "yanlış" kod derleme girmiş tanımlamak için Microsoft Security içinde güvenlik ekibi için geçer.

## <a name="closedlocked-environment"></a>Ortam kapalı/kilitli
Varsayılan olarak, Azure altyapı düğümleri ve Konuk Vm'leri üzerinde oluşturulan herhangi bir kullanıcı hesabı yok. Ayrıca, varsayılan Windows yönetici hesapları da devre dışı bırakılır. Yöneticiler Microsoft Azure canlı destek (WALS) öğesinden kimlik doğrulaması gerçekleştirilmesini – bu makinelerinde oturum açmak ve Azure üretim ağı için Acil onarımların yönetme.

## <a name="microsoft-azure-sql-database-authentication"></a>Microsoft Azure SQL veritabanı kimlik doğrulaması
Herhangi bir uygulama SQL Server gibi kullanıcı hesabı Yönetimi sıkı bir şekilde denetlenebilir. Microsoft Azure SQL veritabanı, yalnızca SQL Server kimlik doğrulamasını destekler. Kullanıcı hesapları ile güçlü parolalar ve yapılandırılmış belirli haklar da Müşteri'nin veri güvenlik modelini tamamlar için kullanılmalıdır.

## <a name="firewallacls-between-msft-corpnet-and-microsoft-azure-cluster"></a>Güvenlik Duvarı/ACL MSFT CorpNet ile Microsoft Azure küme
ACL'ler/Güvenlik Duvarı hizmeti platformu ve MS şirket ağı arasında Microsoft Azure SQL veritabanı Insider yetkisiz erişime karşı koruyun. Ayrıca, yalnızca CorpNet Microsoft gelen IP adresi aralıklarının kullanıcılardan WinFabric platform yönetim uç noktasına erişebilirsiniz.

## <a name="firewallacls-between-nodes-in-an-azure-sql-db-cluster"></a>Bir Azure SQL DB kümedeki düğümler arasında güvenlik duvarı/ACL
Savunma açma-stratejisinin, bir parçası olarak ek bir koruma olarak ACL'leri/güvenlik duvarı uygulanmıştır Microsoft Azure SQL DB kümedeki düğümler arasında. WinFabric platform küme içindeki tüm iletişimi yanı sıra tüm çalışan kodu güveniliyor.

## <a name="custom-mas-watchdogs"></a>Özel MAs (Watchdogs)
Microsoft Azure SQL veritabanı, Microsoft Azure SQL DB küme durumunu izlemek için özel MAs watchdogs adlı kullanır.

## <a name="web-protocols"></a>Web protokolleri

### <a name="role-instance-monitoring-and-restart"></a>Rolü örneği izleme ve yeniden başlatma
Azure, dağıtılan tüm çalışan rolleri (Internet'e yönelik web veya çalışan rolleri arka uç işleme) etkili bir şekilde olduklarından emin olmak için izleme ve verimli bir şekilde, bunlar sağlanmış Hizmetleri sunmaya sürekli sistem durumu tabi olmasını sağlar. Bir rol tarafından barındırılan veya temel alınan yapılandırma sorunu rol örneği içinde olan uygulamada Kritik hata, kötüleşir durumunda Microsoft Azure FC rol örneğinde sorunu algılama ve düzeltme durumu başlatın .

### <a name="compute-connectivity"></a>Bilgi işlem bağlantısı
Azure, dağıtılan uygulama/hizmet standart web tabanlı protokolleri üzerinden ulaşılabilir olmasını sağlar. Internet'e yönelik web rolü sanal örneğe dış Internet bağlantısına sahip ve doğrudan web kullanıcılar tarafından erişilemez. Arka uç işleme çalışan rolü sanal örnekleri dış Internet bağlantısına sahip ancak duyarlılık ve çalışan rolleri adına gerçekleştiren işlemleri bütünlüğünü korumak için doğrudan bir dış web kullanıcı tarafından erişilemez. Genel olarak erişilebilir web rolü sanal örnekleri.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Azure altyapısının güvenliğini sağlamak için yaptığı hakkında daha fazla bilgi için bkz:

- [Azure özellikleri, şirket içi ve fiziksel güvenlik](azure-physical-security.md)
- [Azure altyapı kullanılabilirlik](azure-infrastructure-availability.md)
- [Azure Information sistem bileşenleri ve sınırlar](azure-infrastructure-components.md)
- [Azure ağ mimarisi](azure-infrastructure-network.md)
- [Azure üretim ağı](azure-production-network.md)
- [Microsoft Azure SQL veritabanı güvenlik özellikleri](azure-infrastructure-sql.md)
- [Azure Üretim Operasyon ve Yönetimi](azure-infrastructure-operations.md)
- [Azure altyapısını izleme](azure-infrastructure-monitoring.md)
- [Azure'da müşteri verilerini koruma](azure-protection-of-customer-data.md)
