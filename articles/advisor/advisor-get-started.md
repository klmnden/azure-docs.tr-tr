---
title: Azure Advisor'ı kullanmaya başlama | Microsoft Docs
description: Azure Advisor'ı kullanmaya başlayın.
services: advisor
author: kasparks
ms.service: advisor
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/01/2019
ms.author: kasparks
ms.openlocfilehash: f91e48a532a278c95d50775e135ac6379e8d8070
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332065"
---
# <a name="get-started-with-azure-advisor"></a>Azure Advisor’ı kullanmaya başlama

Azure portalı üzerinden Advisor'a erişmek, öneriler alın ve önerileri uygulama hakkında bilgi edinin.

> [!NOTE]
> Azure Danışmanı, kaynakları yeni oluşturulan bulmak için arka planda otomatik olarak çalıştırılır. Uygulamanın üzerinde bu kaynakları öneriler sağlamak için 24 saat sürebilir.

## <a name="get-recommendations"></a>Öneriler alın

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol bölmede **Advisor**.  Sol bölmede Danışman'ı görmüyorsanız tıklayın **tüm hizmetleri**.  Hizmet menüsünü bölmede altında **izleme ve Yönetim**, tıklayın **Advisor**. Danışman Panosu görüntülenir.

   ![Azure portalını kullanarak erişim Azure Danışmanı](./media/advisor-get-started/advisor-portal-menu.png) 

1. Danışman Panosu, tüm seçili abonelikler için önerilerin bir özeti görüntülenir.  Filtresi açılan menüsü, aboneliği kullanmak için görüntülenecek önerileri istediğiniz abonelikleri seçebilirsiniz.

1. Belirli bir kategori için öneriler almak için sekmelerden birine tıklayın: **Yüksek kullanılabilirlik**, **güvenlik**, **performans**, veya **maliyet**. 

   ![Azure Danışmanı Panosu](./media/advisor-overview/advisor-dashboard.png)

## <a name="get-recommendation-details-and-implement-a-solution"></a>Öneri ayrıntılarını almak ve bir çözümü

Öneri Danışmanı – Önerilen Eylemler ve etkilenen kaynaklar – gibi ek ayrıntıları görüntülemek ve öneri çözümü uygulamak için seçin.  

1. Oturum [Azure portalında](https://portal.azure.com)ve ardından açın [Advisor](https://aka.ms/azureadvisordashboard).

1. O kategorideki öneriler listesini görüntülemek veya seçmek için bir öneri kategorisi seçin **tüm** tüm önerileri görmek için sekmesinde.

1. Ayrıntılı olarak gözden geçirmek istediğiniz bir öneriye tıklayın.

1. Öneri ve öneri uygulanacağı kaynakları hakkında bilgileri gözden geçirin.

1. Tıklayarak **önerilen eylem** öneriyi uygulamak için.

## <a name="filter-recommendations"></a>Filtre önerileri

Sizin için önemli olan aşağı inmek için öneriler filtreleyebilirsiniz.  Abonelik, kaynak türü veya öneri durumu göre filtreleyebilirsiniz.  

1. Oturum [Azure portalında](https://portal.azure.com)ve ardından açın [Advisor](https://aka.ms/azureadvisordashboard).

1. Açılır menüleri kullanarak Advisor Panoda aboneliğe, kaynak türü ya da öneri durumu filtrelemek için kullanın.

    ![Advisor arama filtre ölçütü](./media/advisor-get-started/advisor-filters.png)

## <a name="postpone-or-dismiss-recommendations"></a>Erteleyebilir veya önerileri Kapat

1. Oturum [Azure portalında](https://portal.azure.com)ve ardından açın [Advisor](https://aka.ms/azureadvisordashboard).

1. Erteleyebilir veya kapatmak istediğiniz öneriye gidin.

1. Bir öneriye tıklayabilir.

1. Tıklayın **erteleme**. 

1. Bir Ertele'yi zaman aralığını belirtin veya seçin **hiçbir zaman** öneri kapatılamadı.

## <a name="exclude-subscriptions-or-resource-groups"></a>Abonelik veya kaynak grupları Dışla

Kaynak grubu veya abonelik Danışmanı önerilerini almak istediğiniz değil: 'test' kaynakları gibi olabilir.  Yalnızca belirli Abonelikleriniz ve kaynak gruplarınız için öneriler oluşturmak için Advisor yapılandırabilirsiniz.

> [!NOTE]
> Dahil etmek veya bir abonelik veya kaynak grubu Danışmandan hariç tutmak için bir abonelik sahibi olmanız gerekir.  Bir abonelik veya kaynak grubu için gerekli izinleri yoksa, kullanıcı arabiriminde dahil edilecek veya hariç tutulacak seçeneği devre dışı bırakıldı.

1. Oturum [Azure portalında](https://portal.azure.com)ve ardından açın [Advisor](https://aka.ms/azureadvisordashboard).

1. Tıklayın **yapılandırma** eylem çubuğunda.

1. Herhangi bir abonelik veya kaynak grupları için Danışmanı önerilerini almak istiyor musunuz işaretini kaldırın.

    ![Danışman kaynakları örnek yapılandırma](./media/advisor-get-started/advisor-configure-resources.png)

1. Tıklayın **Uygula** düğmesi.

## <a name="configure-low-usage-vm-recommendation"></a>Az kullanılan sanal makine önerisi yapılandırın

Bu yordam, kullanımı düşük sanal makine önerisi için ortalama CPU kullanımı kuralı yapılandırır.

Advisor için 7 gün, sanal makine kullanımını izler ve ardından kullanımı düşük sanal makineleri tanımlar. Sanal makineler, düşük kullanımı, CPU kullanımı % 5'ise veya daha az olarak kabul edilir ve kendi ağ kullanımı % 2'den az veya daha küçük bir sanal makine boyutu tarafından yerleştirilebilecek geçerli iş yükünü.

Az kullanılan sanal makineleri saptamayı daha ısrarlı olmasını istiyorsanız, ortalama CPU kullanımı Kural başına abonelik temelinde ayarlayabilirsiniz.  % 5, % 10, %15 veya % 20 CPU kullanımı kural ayarlanabilir.

> [!NOTE]
> Az kullanılan sanal makineleri tanımlamak için ortalama CPU kullanımı kuralı ayarlamak için bir abonelik olmalıdır *sahibi*.  Bir abonelik veya kaynak grubu için gerekli izinleri yoksa, dahil edilecek veya hariç tutulacak seçeneği kullanıcı arabiriminin devre dışı bırakılır. 

1. Oturum [Azure portalında](https://portal.azure.com)ve ardından açın [Advisor](https://aka.ms/azureadvisordashboard).

1. Tıklayın **yapılandırma** eylem çubuğunda.

1. Tıklayın **kuralları** sekmesi.

1. Ortalama CPU kullanımı kural değiştirebilir ve ardından istediğiniz abonelikleri seçin **Düzenle**.

1. İstenen ortalama CPU kullanımı değeri seçin ve tıklayın **Uygula**.

1. Tıklayın **önerileri Yenile** yeni ortalama CPU kullanımı kural kullanılacak mevcut önerilerinizi güncelleştirilecek. 

   ![Advisor öneri kuralları örnek yapılandırma](./media/advisor-get-started/advisor-configure-rules.png)

## <a name="download-recommendations"></a>Önerileri indir

Danışman önerilerinizi özetini indirin olanak tanır.  Önerileriniz bir PDF veya CSV dosyası olarak indirebilirsiniz.  Önerilerinizi indirme kolayca iş arkadaşlarınızla paylaşmak veya kendi analizinizi öneri veriler üzerinde gerçekleştirmek sağlar.

1. Oturum [Azure portalında](https://portal.azure.com)ve ardından açın [Advisor](https://aka.ms/azureadvisordashboard).

1. Tıklayın **CSV olarak indir** veya **PDF olarak indir** eylem çubuğundaki.

Danışman panosu için uyguladığınız herhangi bir filtre indirme seçeneğini uyar.  Belirli bir öneri kategorisi veya öneri görüntülerken yükleme seçeneğini belirlerseniz, indirilen Özeti yalnızca bu kategori veya öneri bilgilerini içerir. 

## <a name="next-steps"></a>Sonraki adımlar

Advisor hakkında daha fazla bilgi için bkz:

- [Azure Danışmanı giriş](advisor-overview.md)
- [Advisor yüksek kullanılabilirlik önerisi](advisor-high-availability-recommendations.md)
- [Advisor güvenlik önerileri](advisor-security-recommendations.md)
- [Danışmanı performans önerileri](advisor-performance-recommendations.md)
- [Advisor maliyet önerileri](advisor-performance-recommendations.md)
