---
title: Azure Geçişi'nde yenilikler | Microsoft Docs
description: Azure Geçişi hizmetine genel bir bakış sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 06/10/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 2c3bc596076f3ec4f9d41f0da819ddd386fee63c
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809998"
---
# <a name="whats-new-in-azure-migrate"></a>Azure Geçişi'nde yenilikler

[Azure geçişi](migrate-services-overview.md) keşfedin, değerlendirin ve sunucular, uygulamalar ve veriler, Microsoft Azure bulutuna geçirmek için yardımcı olur. Bu makalede, Azure Geçişi'ndeki yeni özellikler özetlenmektedir.



## <a name="azure-migrate-new-version"></a>Azure geçişi yeni sürümü

Azure Geçişi'nın yeni sürümünü Temmuz 2019 ' yayınlanmıştır. 

- **Geçerli (yeni) sürümü**: Bu sürüm, Azure geçişi projeleri oluşturma, şirket içi makineleri bulma ve değerlendirme ve geçiş düzenlemek için kullanın. 
- **Önceki sürüm**: (Yalnızca değerlendirme şirket içi VMware vm'lerinin destekleniyordu) Azure Geçişi'nin önceki sürümünü kullanan müşteri için artık geçerli sürümünü kullanmanız gerekir. Önceki sürümde, artık yeni Azure geçişi projeler oluşturabilir, veya yeni bulma gerçekleştirin. Var olan projeleri erişmeye devam edebilirsiniz. Azure portalında bunu yapmanın > tüm hizmetleri, Azure geçişi için arama yapın. Azure geçişi Bildirimlerde eski Azure geçişi projesine erişebilmek için bir bağlantı yoktur.


## <a name="azure-migrate-features"></a>Azure geçişi özellikleri

Yeni sürümü, Azure geçişi, birçok yeni özellik sağlar:


- **Birleşik geçiş platform**: Azure geçişi, artık tek bir merkezden yönetin, yönetmek ve bir geliştirilmiş dağıtım akışı ve portal deneyimi ile Azure'a geçiş yolculuğunuza izlemek için tek bir portal sağlar.
- **Değerlendirme ve Geçiş Araçları**: Azure geçişi, yerel araçlar sağlar ve diğer Azure hizmetleriyle yanı sıra bağımsız yazılım satıcısı (ISV) araçları ile tümleşir. [Daha fazla bilgi edinin](migrate-services-overview.md#isv-integration) ISV tümleştirmesi hakkında.
- **Azure geçişi değerlendirmesi**: Azure geçişi Server değerlendirmesi Aracı'nı kullanarak, VMware Vm'leri ve Hyper-V Vm'lerini Azure'a geçiş için değerlendirebilirsiniz. Ayrıca, diğer Azure Hizmetleri ve ISV araçlarını kullanarak geçiş için değerlendirebilirsiniz.
- **Azure geçişi geçiş**: Azure geçişi sunucu geçiş aracını kullanarak şirket içi VMware Vm'leri geçirebileceğiniz ve Vm'leri Hyper-V Vm'lerini Azure'a yanı sıra fiziksel sunucuları, diğer sanallaştırılmış sunucular ve private/public bulut. Ayrıca, ISV araçları kullanarak Azure'a geçirebilirsiniz.
- **Azure geçişi Gereci**: Azure geçişi bulması için basit bir gereç dağıtır ve değerlendirmesi, VMware Vm'leri ve Hyper-V sanal makinelerini şirket.
    - Bu gereç aracısız geçiş için Azure geçişi Server değerlendirmesi ve Azure geçişi sunucusu geçişi tarafından kullanılır.
    - Gereç, sunucu meta verileri ve performans verilerini, değerlendirme ve geçiş amaçları doğrultusunda sürekli olarak bulur.  
- **VMware VM geçişi**:  Azure geçişi sunucusu geçişi birkaç yöntem geçirmek için yerinde VMware Vm'lerini azure'a sağlar.  Azure geçişi Gereci ve çoğaltma Gereci kullanır ve her VM'de bir aracı dağıtan bir aracı tabanlı geçişi kullanarak aracısız bir geçiş geçiş yapmak istiyorsanız. [Daha fazla bilgi edinin](server-migrate-overview.md)
 - **Değerlendirme ve geçiş veritabanı**: Azure Geçişi'nden şirket içi veritabanları Azure veritabanı geçiş Yardımcısı'nı kullanarak Azure'a geçiş için değerlendirebilirsiniz. Azure veritabanı geçiş hizmetini kullanarak veritabanlarını geçirebilirsiniz.
- **Web uygulaması geçiş**: Bir genel uç nokta URL'si ile Azure App Service kullanarak web uygulamaları değerlendirebilirsiniz. Dahili .NET uygulamalarının geçiş için indirin ve App Service Migration Yardımcısı'nı çalıştırın. 
- **Veri kutusu**: Azure Geçişi'ndeki Azure Data Box'ı kullanarak Azure'da çevrimdışı büyük miktarlarda veri aktarın.


## <a name="next-steps"></a>Sonraki adımlar

- Azure Geçişi fiyatlandırması hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/azure-migrate/).
- Azure Geçişi hakkında [sık sorulan soruları gözden geçirin](resources-faq.md).
- Değerlendirmek için öğreticilerimize deneyin [VMware Vm'lerini](tutorial-assess-vmware.md) ve [Hyper-V Vm'lerini](tutorial-assess-hyper-v.md).
