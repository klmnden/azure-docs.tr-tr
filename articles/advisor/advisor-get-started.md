---
title: Azure Advisor ile çalışmaya başlama | Microsoft Docs
description: Azure Advisor ile çalışmaya başlayın.
services: advisor
documentationcenter: NA
author: manbeenkohli
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/10/2017
ms.author: makohli
ms.openlocfilehash: cd64515beabec43a5209d62dccf2b55b21702c16
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
ms.locfileid: "30230059"
---
# <a name="get-started-with-azure-advisor"></a>Azure Advisor’ı kullanmaya başlama

Advisor Azure portalı üzerinden erişim, öneriler alın ve önerileri uygulamak öğrenin.

## <a name="get-advisor-recommendations"></a>Danışman’dan öneriler alın

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Sol bölmede **Danışmanı**.  Sol bölmede Danışmanı'nı görmüyorsanız tıklatın **tüm hizmetleri**.  Hizmet menü bölmesinde altında **izleme ve Yönetim**, tıklatın **Danışmanı**.
 Advisor Panosu görüntülenir.

   ![Erişim Azure Azure portalını kullanarak Danışmanı](./media/advisor-get-started/advisor-portal-menu.png) 

4. Advisor Pano önerilerinizi tüm seçili abonelikler için özetini görüntüler.  Abonelik kullanmak için görüntülenecek önerileri istediğiniz abonelikleri açılır filtre seçebilirsiniz.

5. Belirli bir kategorideki önerileri almak için sekmelerden birine tıklayın: **yüksek kullanılabilirlik**, **güvenlik**, **performans**, veya **maliyet**. 

  ![Azure Danışmanı Panosu](./media/advisor-overview/advisor-dashboard.png)

## <a name="get-advisor-recommendation-details-and-implement-a-solution"></a>Advisor öneri ayrıntılarını almak ve bir çözüm uygulama

Advisor – Önerilen Eylemler ve etkilenen kaynakları – gibi ek ayrıntıları görüntülemek ve öneri çözümü uygulamak için bir öneri seçebilirsiniz.  

1. Oturum [Azure portal](https://portal.azure.com)ve ardından açın [Danışmanı](https://aka.ms/azureadvisordashboard).

2. Bu kategoride öneriler listesini görüntülemek veya seçmek için bir öneri kategorisi seçin **tüm** tüm önerilerinizi görüntülemek için.

3. Ayrıntılı olarak gözden geçirmek istediğiniz bir öneri'ı tıklatın.

4. Öneri ve öneri uygulandığı kaynaklar hakkında bilgileri gözden geçirin.

5. Tıklayın **önerilen eylem** öneriyi uygulamayı.

## <a name="filter-advisor-recommendations"></a>Filtre Advisor önerileri

Sizin için en önemli nedir aşağıya doğru incelemek için öneriler filtreleyebilirsiniz.  Abonelik, kaynak türü veya öneri durumu göre filtreleyebilirsiniz.  

1. Oturum [Azure portal](https://portal.azure.com)ve ardından açın [Danışmanı](https://aka.ms/azureadvisordashboard).

2.  Bırakmalar Danışmanı Panoda abonelik, kaynak türü veya öneri durumu göre filtre uygulamak için kullanın.

    ![Advisor arama filtre ölçütü](./media/advisor-get-started/advisor-filters.png)

## <a name="postpone-or-dismiss-advisor-recommendations"></a>Erteleyin veya Advisor önerileri kapatın

1. Oturum [Azure portal](https://portal.azure.com)ve ardından açın [Danışmanı](https://aka.ms/azureadvisordashboard).

2. Erteleyin veya kapatmak istediğiniz öneri gidin.

3. Öneri'ı tıklatın.

4. Tıklatın **erteleyin**. 

5. Bir Ertele'yi zaman aralığını belirtin veya seçin **hiçbir zaman** öneri kapatılamadı.

## <a name="exclude-subscriptions-or-resource-groups-from-advisor"></a>Abonelik veya kaynak grupları danışmanına tut

Kaynak grupları ya da abonelik Danışmanı önerileri almak istediğiniz değil – 'test' kaynakları gibi olabilir.  Yalnızca belirli Abonelikleriniz ve kaynak gruplarınız için öneri oluşturmak amacıyla Danışmanı'nı yapılandırabilirsiniz.

> [!NOTE]
> Eklemek veya bir abonelik veya kaynak grubu danışmanına çıkarmak için bir abonelik sahibi olmalıdır.  Bir abonelik veya kaynak grubu için gerekli izinlere sahip değil, dahil etmek veya dışarıda seçeneğine kullanıcı arabiriminde devre dışı bırakılır.

1. Oturum [Azure portal](https://portal.azure.com)ve ardından açın [Danışmanı](https://aka.ms/azureadvisordashboard).

2. Tıklatın **yapılandırma** eylem çubuğunda.

3. Herhangi bir abonelik veya kaynak grupları Danışmanı önerileri için almak istiyor musunuz seçeneğinin işaretini kaldırın.

    ![Advisor kaynakları örnek yapılandırma](./media/advisor-get-started/advisor-configure-resources.png)

4. Tıklatın **Uygula** düğmesi.

## <a name="configure-the-average-cpu-utilization-rule-for-the-low-usage-virtual-machine-recommendation"></a>Düşük kullanım sanal makine öneri için ortalama CPU kullanımı kuralı yapılandırma

Advisor 14 gün boyunca, sanal makine kullanımını izler ve düşük kullanımı sanal makineleri tanımlar. Sanal makineler, ortalama CPU kullanımı yüzde 5'idir veya daha az ve ağ kullanımını 7 MB veya daha az dört veya daha fazla gün düşük kullanımı sanal makineler olarak kabul edilir.

Düşük kullanım sanal makineleri saptamayı daha agresif olmasını istiyorsanız, abonelik başına temelinde ortalama CPU kullanımı kuralı ayarlayabilirsiniz.  Ortalama CPU kullanımı kuralı % 5, % 10, %15 veya % 20 ayarlayabilirsiniz.

> [!NOTE]
> Düşük kullanım sanal makineleri tanımlamak için ortalama CPU kullanımı kuralı ayarlamak için bir abonelik olmalıdır *sahibi*.  Bir abonelik veya kaynak grubu için gerekli izinlere sahip değil, dahil etmek veya dışarıda seçeneğine kullanıcı arabiriminde devre dışı bırakılacak. 

1. Oturum [Azure portal](https://portal.azure.com)ve ardından açın [Danışmanı](https://aka.ms/azureadvisordashboard).

2. Tıklatın **yapılandırma** eylem çubuğunda.

3. Tıklatın **kuralları** sekmesi.

4. İçin ortalama CPU kullanımı kuralı ayarlayın ve ardından istediğiniz abonelikleri seçin **Düzenle**.

5. İstenen ortalama CPU kullanımı değeri seçin ve tıklayın **Uygula**.

6. Tıklatın **yenileme önerileri** yeni ortalama CPU kullanımı kuralı kullanmak için mevcut önerilerinizi güncelleştirmek için. 

   ![Advisor öneri kuralları örnek yapılandırma](./media/advisor-get-started/advisor-configure-rules.png)

## <a name="download-your-advisor-recommendations"></a>Advisor önerileri indirin

Advisor önerilerinizi özetini indirmek etkinleştirir.  Önerilerinizi PDF dosyası veya bir CSV dosyası olarak indirebilirsiniz.  Önerilerinizi indirme kolayca arkadaşlarınızla paylaşın veya öneri veri üstünde, kendi analiz gerçekleştirmenize imkan sağlar.

1. Oturum [Azure portal](https://portal.azure.com)ve ardından açın [Danışmanı](https://aka.ms/azureadvisordashboard).

2. Tıklatın **CSV olarak indirme** veya **PDF olarak karşıdan** eylem çubuğunda.

Yükleme seçeneği Danışmanı panoya uyguladığınız filtreleri dikkate alır.  Belirli öneri kategori veya öneri görüntülerken yükleme seçeneğini belirlerseniz, indirilen Özeti yalnızca bu kategori veya öneri bilgilerini içerir. 

## <a name="next-steps"></a>Sonraki adımlar

Advisor hakkında daha fazla bilgi için bkz:
* [Azure Danışmanı giriş](advisor-overview.md)
* [Advisor yüksek kullanılabilirlik önerileri](advisor-high-availability-recommendations.md)
* [Advisor güvenlik önerileri](advisor-security-recommendations.md)
-  [Advisor performans önerileri](advisor-performance-recommendations.md)
* [Advisor maliyet önerileri](advisor-performance-recommendations.md)
