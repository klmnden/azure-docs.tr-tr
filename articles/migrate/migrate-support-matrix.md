---
title: Azure geçişi destek matrisi
description: Destek ayarları ve sınırlamalar için Azure geçişi hizmeti, bir özetini sağlar.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: raynew
ms.openlocfilehash: b2ca1b9118ecc3d112a49bb4c79b413c46fe67cb
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811563"
---
# <a name="azure-migrate-support-matrix"></a>Azure geçişi destek matrisi

Kullanabileceğiniz [Azure geçişi hizmeti](migrate-overview.md) değerlendirmek ve makineleri için Microsoft Azure bulutuna geçirmek için. Bu makalede, genel destek ayarları ve Azure geçişi senaryoları ve dağıtımlar için sınırlamaları özetlenmektedir.


## <a name="azure-migrate-versions"></a>Azure geçişi sürümleri

Azure geçişi hizmeti iki sürümü vardır:

- **Geçerli sürümü**: Yeni Azure geçişi projeleri oluşturabilirsiniz. bu sürümünü kullanarak, şirket içi değerlendirir ve değerlendirmeler ve geçişleri düzenlemek keşfedin. [Daha fazla bilgi edinin](whats-new.md#azure-migrate-new-version).
- **Önceki sürüm**: (Yalnızca değerlendirme şirket içi VMware vm'lerinin destekleniyordu) Azure Geçişi'nin önceki sürümünü kullanan müşteri için artık geçerli sürümünü kullanmanız gerekir. Önceki sürümde, yeni Azure geçişi proje oluşturamaz veya yeni bulma gerçekleştirin.

## <a name="supported-migration-scenarios"></a>Desteklenen geçiş senaryoları

Tabloda, desteklenen geçiş senaryoları özetlenmiştir.

**Dağıtım** | **Ayrıntıları*** 
--- | --- 
**Şirket içi değerlendirmesi** | Şirket içi iş yüklerini ve verileri VMware Vm'leri ve Hyper-V Vm'lerinde çalışan değerlendirin. Azure geçişi Server değerlendirmesi ve Microsoft Data Migration Yardımcısı (DMA) yanı sıra Cloudamize Corent teknik ve Turbonomic sunucu üçüncü taraf araçları kullanarak değerlendirin.
**Şirket içinde azure'a geçiş** | İş yükleri ve VMware Vm'leri, Hyper-V Vm'lerinden, fiziksel sunucuları azure'a AWS/GCP örneklerinde çalıştırıldığı verileri geçirin. Azure geçişi Server değerlendirmesi ve Azure veritabanı geçiş hizmeti (DMS) geçişi ve Carbonite ve CorentTech dahil üçüncü taraf araçları kullanarak olarak yanı sıra.

Özel araç desteği aşağıda özetlenmiştir.

**Araç** | **Değerlendirme/geçiş** | **Ayrıntılar**
--- | --- | ---
Azure geçişi Server değerlendirmesi | Değerlendirme | Server değerlendirmesi için deneme [Hyper-V](tutorial-prepare-hyper-v.md) ve [VMware](tutorial-prepare-vmware.md).
Cloudamize | Değerlendirme | [Daha fazla bilgi edinin](https://www.cloudamize.com/platform#tab-0).
CorentTech | Değerlendirme | [Daha fazla bilgi edinin](https://www.corenttech.com/).
Turbonomic | Değerlendirme | [Daha fazla bilgi edinin](https://turbonomic.com/solutions/technologies/azure-cloud/).
Azure geçişi sunucusu geçişi | Geçiş | Sunucu geçiş için deneme [Hyper-V](tutorial-migrate-hyper-v.md) ve [VMware](tutorial-migrate-vmware.md).
Carbonite | Geçiş | [Daha fazla bilgi edinin](https://www.carbonite.com/data-protection-resources/resource/Datasheet/carbonite-migrate-for-microsoft-azure).
CorentTech | Geçiş | [Daha fazla bilgi edinin](https://www.corenttech.com/).


## <a name="azure-migrate-projects"></a>Azure geçiş projeleri

**Destek** | **Ayrıntılar**
--- | ---
Subscription | Tek bir Azure geçişi projesini bir abonelikte olabilir.
Azure izinleri | Bir Azure geçişi projesi oluşturmak için katkıda bulunan veya sahip izinleri aboneliği gerekir.
VMware Sanal Makineleri  | Tek bir projede en fazla 35.000 VMware Vm'lerini değerlendirin.
Hyper-V Sanal Makineleri | Tek bir projede en fazla 10.000 Hyper-V Vm'lerini değerlendirin.

Bir proje, VMware Vm'lerini hem Hyper-V Vm'lerinden, en fazla değerlendirme sınırları içerebilir.


## <a name="vmware-assessment-and-migration"></a>VMware değerlendirme ve geçiş

[Gözden geçirme](migrate-support-matrix-vmware.md) VMware Vm'leri için Azure geçişi Server değerlendirmesi ve sunucu geçişi destek matrisi.

## <a name="hyper-v-assessment-and-migration"></a>Hyper-V değerlendirme ve geçiş

[Gözden geçirme](migrate-support-matrix-hyper-v.md) Hyper-V Vm'leri için Azure geçişi Server değerlendirmesi ve sunucu geçişi destek matrisi.


## <a name="next-steps"></a>Sonraki adımlar

- [VMware sanal makineleri değerlendirme](tutorial-assess-vmware.md) geçiş.
- [Hyper-V sanal makineleri değerlendirme](tutorial-assess-hyper-v.md) geçiş.

