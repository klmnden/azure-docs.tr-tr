---
title: Azure altyapı bütünlüğü
description: Bu makalede, Azure altyapı bütünlüğü yöneliktir.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/06/2018
ms.author: terrylan
ms.openlocfilehash: 24d54fa7a8985a6af58cddfc969b8023485c73c2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60587130"
---
# <a name="azure-infrastructure-integrity"></a>Azure altyapı bütünlüğü

## <a name="software-installation"></a>Yazılım yükleme
Azure ortamında yüklenen tüm yazılım yığınında özel Microsoft Security Development Lifecycle (SDL) işlem oluşturulan bileşenlerdir. İşletim sistemi (OS) görüntüsü ve SQL veritabanı dahil olmak üzere tüm yazılım bileşenleri, değişiklik yönetiminin bir parçası dağıtılan ve yönetim sürecini bırakın. Tüm düğümleri üzerinde çalışan işletim sistemi, Windows Server 2008 veya Windows Server 2012, özelleştirilmiş bir sürümüdür. Tam sürümünü (FC) yürütmek işletim sistemi için düşünüyor role göre yapı denetleyicisi tarafından seçilir. Ayrıca, konak işletim sistemi tüm yetkisiz yazılım bileşenlerini yükleme izin vermez.

Bazı Azure bileşenleri Azure müşterilerinin bir konuk olarak dağıtılan bir konuk işletim sistemi çalıştıran VM.

## <a name="virus-scans-on-builds"></a>Virüs tarama derlemelerinde
Azure yazılım bileşeni (işletim sistemi dahil) derlemeleri Endpoint Protection virüsten koruma Aracı'nı kullanan bir virüs taraması geçmeleri gerekir. Her bir virüs taraması ne taranan gerçekleşen ilişkili derleme dizini ve tarama sonuçlarını bir günlük oluşturur. Virüs taraması azure'daki her bileşeni için derleme kaynak kodu bir parçasıdır. Kod üretim için bir temiz ve başarılı virüs taraması zorunda kalmadan taşınmaz. Herhangi bir sorun belirtilmiştir, yapı'nın dondurulmuş olup ve "yanlış" kod derleme girmiş tanımlamak için Microsoft Security güvenlik takımları gider.

## <a name="closed-and-locked-environment"></a>Kapalı ve kilitli bir ortam
Varsayılan olarak, Azure altyapı düğümleri ve Konuk Vm'leri üzerinde oluşturulan kullanıcı hesaplarını yok. Ayrıca, varsayılan Windows yönetici hesapları da devre dışı bırakılır. Yöneticiler Azure canlı destek uygun kimlik doğrulaması ile bu makinelerinde oturum açmak ve Azure üretim ağı için Acil onarımların yönetme.

## <a name="azure-sql-database-authentication"></a>Azure SQL veritabanı kimlik doğrulaması
Herhangi bir uygulama SQL Server gibi kullanıcı hesabı Yönetimi sıkı bir şekilde denetlenebilir. Azure SQL veritabanı yalnızca SQL Server kimlik doğrulamasını destekler. Bir müşterinin veri güvenlik modelini tamamlar için kullanıcı hesapları ile güçlü parolalar ve belirli bir ile yapılandırılmış hak de kullanılmalıdır.

## <a name="acls-and-firewalls-between-the-microsoft-corporate-network-and-an-azure-cluster"></a>ACL ve Microsoft Kurumsal ağ ve bir Azure kümesi arasındaki güvenlik duvarı
Erişim denetimi listeleri (ACL'ler) ve hizmet platformu ve Microsoft Kurumsal ağ arasındaki güvenlik duvarlarının SQL veritabanı örnekleri Insider yetkisiz erişimden korumak. Ayrıca, yalnızca Microsoft Kurumsal ağdan IP adres aralıklarını kullanıcıların Windows Fabric platform Yönetimi uç nokta erişebilirsiniz.

## <a name="acls-and-firewalls-between-nodes-in-a-sql-database-cluster"></a>ACL ve SQL veritabanı kümedeki düğümler arasındaki güvenlik duvarları
Savunma açma-stratejisinin, bir parçası olarak ek bir koruma olarak ACL ve güvenlik duvarı SQL veritabanı kümedeki düğümler arasında uygulanmıştır. Windows Fabric platform küme içindeki tüm iletişimi yanı sıra tüm çalışan kodu güveniliyor.

## <a name="custom-monitoring-agents"></a>Özel İzleme aracıları
SQL veritabanı, SQL veritabanı küme durumunu izlemek için watchdogs, olarak da bilinen özel izleme aracılarını (MAs) kullanır.

## <a name="web-protocols"></a>Web protokolleri

### <a name="role-instance-monitoring-and-restart"></a>Rolü örneği izleme ve yeniden başlatma
Azure sağlar, tüm dağıtılan, çalışan rolleri (internet'e yönelik web veya çalışan rolleri arka uç işleme) olan kullanıcılar için bunlar sağlanan hizmetler etkili ve verimli bir şekilde teslim emin olmak için sürekli sistem durumu izleme tabidir. Bir rol, yapılıyorsa uygulamanın kritik bir hata veya rol örneği içinde temel alınan bir yapılandırma sorunu bozulursa, FC rol örneği içinde sorun algılar ve bir düzeltme durumu başlatır.

### <a name="compute-connectivity"></a>Bilgi işlem bağlantısı
Azure, dağıtılan uygulama veya hizmet standart web tabanlı protokolleri üzerinden ulaşılabilir olmasını sağlar. Sanal örneğe internet'e yönelik web rollerinin dış internet bağlantısı olan ve doğrudan web kullanıcılar tarafından erişilebilir. Duyarlılık ve çalışan rolleri adına ortak-erişilebilir web rolü sanal örneğe gerçekleştirme işlemleri bütünlüğünü korumak için arka uç işleme çalışan rolleri sanal örneklerini dış Internet bağlantısına sahip ancak olamaz doğrudan web dış kullanıcılar tarafından erişilebilir.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Azure altyapısının güvenliğini sağlamak için yaptığı hakkında daha fazla bilgi için bkz:

- [Azure özellikleri, şirket içi ve fiziksel güvenlik](azure-physical-security.md)
- [Azure altyapı kullanılabilirlik](azure-infrastructure-availability.md)
- [Azure Information sistem bileşenleri ve sınırlar](azure-infrastructure-components.md)
- [Azure ağ mimarisi](azure-infrastructure-network.md)
- [Azure üretim ağı](azure-production-network.md)
- [Azure SQL veritabanı güvenlik özellikleri](azure-infrastructure-sql.md)
- [Azure Üretim Operasyon ve Yönetimi](azure-infrastructure-operations.md)
- [Azure altyapı izleme](azure-infrastructure-monitoring.md)
- [Azure müşteri verilerini koruma](azure-protection-of-customer-data.md)
