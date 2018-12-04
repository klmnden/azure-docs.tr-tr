---
title: Azure Advisor'ı kullanmaya başlama | Microsoft Docs
description: Azure Advisor'ı kullanmaya başlayın.
services: advisor
documentationcenter: NA
author: kasparks
manager: ''
ms.assetid: ''
ms.service: advisor
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/10/2017
ms.author: kasparks
ms.openlocfilehash: 6e66fed21223701cd6c61bd1e903b4e7d7fbe0d0
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52850104"
---
# <a name="get-started-with-azure-advisor"></a>Azure Advisor’ı kullanmaya başlama

Azure portalı üzerinden Advisor'a erişmek, öneriler alın ve önerileri uygulama hakkında bilgi edinin.

## <a name="get-advisor-recommendations"></a>Danışman’dan öneriler alın

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Sol bölmede **Advisor**.  Sol bölmede Danışman'ı görmüyorsanız tıklayın **tüm hizmetleri**.  Hizmet menüsünü bölmede altında **izleme ve Yönetim**, tıklayın **Advisor**.
 Danışman Panosu görüntülenir.

   ![Azure portalını kullanarak erişim Azure Danışmanı](./media/advisor-get-started/advisor-portal-menu.png) 

4. Danışman Panosu, tüm seçili abonelikler için önerilerin bir özeti görüntülenir.  Filtresi açılan menüsü, aboneliği kullanmak için görüntülenecek önerileri istediğiniz abonelikleri seçebilirsiniz.

5. Belirli bir kategori için öneriler almak için sekmelerden birine tıklayın: **yüksek kullanılabilirlik**, **güvenlik**, **performans**, veya **maliyet**. 

  ![Azure Danışmanı Panosu](./media/advisor-overview/advisor-dashboard.png)

## <a name="get-advisor-recommendation-details-and-implement-a-solution"></a>Advisor öneri ayrıntılarını almak ve bir çözümü

Öneri Danışmanı – Önerilen Eylemler ve etkilenen kaynaklar – gibi ek ayrıntıları görüntülemek ve öneri çözümü uygulamak için seçin.  

1. Oturum [Azure portalında](https://portal.azure.com)ve ardından açın [Advisor](https://aka.ms/azureadvisordashboard).

2. O kategorideki öneriler listesini görüntülemek veya seçmek için bir öneri kategorisi seçin **tüm** tüm önerileri görmek için sekmesinde.

3. Ayrıntılı olarak gözden geçirmek istediğiniz bir öneriye tıklayın.

4. Öneri ve öneri uygulanacağı kaynakları hakkında bilgileri gözden geçirin.

5. Tıklayarak **önerilen eylem** öneriyi uygulamak için.

## <a name="filter-advisor-recommendations"></a>Filtre Danışmanı önerileri

Sizin için önemli olan aşağı inmek için öneriler filtreleyebilirsiniz.  Abonelik, kaynak türü veya öneri durumu göre filtreleyebilirsiniz.  

1. Oturum [Azure portalında](https://portal.azure.com)ve ardından açın [Advisor](https://aka.ms/azureadvisordashboard).

2.  Açılır menüleri kullanarak Advisor Panoda aboneliğe, kaynak türü ya da öneri durumu filtrelemek için kullanın.

    ![Advisor arama filtre ölçütü](./media/advisor-get-started/advisor-filters.png)

## <a name="postpone-or-dismiss-advisor-recommendations"></a>Erteleyebilir veya Danışmanı önerilerini Kapat

1. Oturum [Azure portalında](https://portal.azure.com)ve ardından açın [Advisor](https://aka.ms/azureadvisordashboard).

2. Erteleyebilir veya kapatmak istediğiniz öneriye gidin.

3. Bir öneriye tıklayabilir.

4. Tıklayın **erteleme**. 

5. Bir Ertele'yi zaman aralığını belirtin veya seçin **hiçbir zaman** öneri kapatılamadı.

## <a name="exclude-subscriptions-or-resource-groups-from-advisor"></a>Abonelik veya kaynak grupları Danışmandan Dışla

Kaynak grubu veya abonelik Danışmanı önerilerini almak istediğiniz değil: 'test' kaynakları gibi olabilir.  Yalnızca belirli Abonelikleriniz ve kaynak gruplarınız için öneriler oluşturmak için Advisor yapılandırabilirsiniz.

> [!NOTE]
> Dahil etmek veya bir abonelik veya kaynak grubu Danışmandan hariç tutmak için bir abonelik sahibi olmanız gerekir.  Bir abonelik veya kaynak grubu için gerekli izinleri yoksa, kullanıcı arabiriminde dahil edilecek veya hariç tutulacak seçeneği devre dışı bırakıldı.

1. Oturum [Azure portalında](https://portal.azure.com)ve ardından açın [Advisor](https://aka.ms/azureadvisordashboard).

2. Tıklayın **yapılandırma** eylem çubuğunda.

3. Herhangi bir abonelik veya kaynak grupları için Danışmanı önerilerini almak istiyor musunuz işaretini kaldırın.

    ![Danışman kaynakları örnek yapılandırma](./media/advisor-get-started/advisor-configure-resources.png)

4. Tıklayın **Uygula** düğmesi.

## <a name="configure-the-average-cpu-utilization-rule-for-the-low-usage-virtual-machine-recommendation"></a>Kullanımı düşük sanal makine önerisi için ortalama CPU kullanımı kuralı yapılandırma

Advisor için 14 gün, sanal makine kullanımını izler ve ardından kullanımı düşük sanal makineleri tanımlar. Sanal makineler, ortalama CPU kullanımı yüzde 5'idir veya daha az ve ağ kullanımını 7 MB veya daha az dört veya daha fazla gün kullanımı düşük sanal makine olarak kabul edilir.

Az kullanılan sanal makineleri saptamayı daha ısrarlı olmasını istiyorsanız, ortalama CPU kullanımı Kural başına abonelik temelinde ayarlayabilirsiniz.  Ortalama CPU kullanımı kural % 5, % 10, %15 veya % 20 ayarlayabilirsiniz.

> [!NOTE]
> Az kullanılan sanal makineleri tanımlamak için ortalama CPU kullanımı kuralı ayarlamak için bir abonelik olmalıdır *sahibi*.  Bir abonelik veya kaynak grubu için gerekli izinleri yoksa, dahil edilecek veya hariç tutulacak seçeneği kullanıcı arabiriminin devre dışı bırakılır. 

1. Oturum [Azure portalında](https://portal.azure.com)ve ardından açın [Advisor](https://aka.ms/azureadvisordashboard).

2. Tıklayın **yapılandırma** eylem çubuğunda.

3. Tıklayın **kuralları** sekmesi.

4. Ortalama CPU kullanımı kural değiştirebilir ve ardından istediğiniz abonelikleri seçin **Düzenle**.

5. İstenen ortalama CPU kullanımı değeri seçin ve tıklayın **Uygula**.

6. Tıklayın **önerileri Yenile** yeni ortalama CPU kullanımı kural kullanılacak mevcut önerilerinizi güncelleştirilecek. 

   ![Advisor öneri kuralları örnek yapılandırma](./media/advisor-get-started/advisor-configure-rules.png)

## <a name="download-your-advisor-recommendations"></a>Danışman önerilerinizi indirin

Danışman önerilerinizi özetini indirin olanak tanır.  Önerileriniz bir PDF veya CSV dosyası olarak indirebilirsiniz.  Önerilerinizi indirme kolayca iş arkadaşlarınızla paylaşmak veya kendi analizinizi öneri veriler üzerinde gerçekleştirmek sağlar.

1. Oturum [Azure portalında](https://portal.azure.com)ve ardından açın [Advisor](https://aka.ms/azureadvisordashboard).

2. Tıklayın **CSV olarak indir** veya **PDF olarak indir** eylem çubuğundaki.

Danışman panosu için uyguladığınız herhangi bir filtre indirme seçeneğini uyar.  Belirli bir öneri kategorisi veya öneri görüntülerken yükleme seçeneğini belirlerseniz, indirilen Özeti yalnızca bu kategori veya öneri bilgilerini içerir. 

## <a name="next-steps"></a>Sonraki adımlar

Advisor hakkında daha fazla bilgi için bkz:
* [Azure Danışmanı giriş](advisor-overview.md)
* [Advisor yüksek kullanılabilirlik önerisi](advisor-high-availability-recommendations.md)
* [Advisor güvenlik önerileri](advisor-security-recommendations.md)
-  [Danışmanı performans önerileri](advisor-performance-recommendations.md)
* [Advisor maliyet önerileri](advisor-performance-recommendations.md)
