---
title: "Azure DevTest Labs'de aylık tahmini Laboratuvar maliyet eğilim görüntülemek | Microsoft Docs"
description: "Azure DevTest Labs aylık tahmini maliyet eğilim Grafiği hakkında bilgi edinin."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 1f46fdc5-d917-46e3-a1ea-f6dd41212ba4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: b3ad1ead522908d4b41b7cca98d20ac91664998e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="view-the-monthly-estimated-lab-cost-trend-in-azure-devtest-labs"></a>Azure DevTest Labs'de aylık tahmini Laboratuvar maliyet eğilim görüntüleyin
DevTest Labs maliyet yönetim özelliği Laboratuvarınızı maliyetini izlemenize yardımcı olur. Bu makalede nasıl kullanılacağını anlatan **aylık tahmini maliyet eğilimi** geçerli Takvim ayı için geçerli Takvim ayın tahmini maliyet-to-date ve tahmini ayın son maliyet görüntülemek üzere grafik. Bu makalede, Azure portalında aylık tahmini maliyet eğilim grafiği görüntüleme öğrenin.

## <a name="viewing-the-monthly-estimated-cost-trend-chart"></a>Aylık tahmini maliyet eğilim grafiği görüntüleme
Aylık tahmini maliyet eğilim Grafiği görüntülemek için aşağıdaki adımları izleyin: 

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. Seçin **daha Hizmetleri**ve ardından **DevTest Labs** listeden.
3. İstenen Laboratuvar labs listesinden seçin.   
4. Laboratuvar 's dikey penceresinde, seçin **maliyet ayarlarına**.
5. Laboratuvar 's üzerinde **maliyet ayarlarına** dikey penceresinde, select **Laboratuvar maliyet eğilimi**.
6. Aşağıdaki ekran görüntüsünde bir maliyet grafik örneği gösterilmektedir. 
   
    ![Maliyet grafik](./media/devtest-lab-configure-cost-management/graph.png)

**Maliyet tahmini** geçerli Takvim ayın tahmini maliyet-to-date bir değerdir. **Tahmini maliyet** tahmini tüm geçerli Takvim ayı için önceki beş gün boyunca Laboratuvar maliyet kullanılarak hesaplanan maliyetidir.

Maliyet tutarlar sonraki tamsayıya yuvarlanır. Örneğin: 

* 5.01 6 adede kadar yuvarlar 
* 5.50 6 adede kadar yuvarlar
* 5.99 6 adede kadar yuvarlar

Grafik durumları gibi grafikte gördüğünüz maliyetleri olduğundan *tahmini* kullanarak maliyetlerini [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) hızları sunar.
Ayrıca, aşağıda verilmiştir *değil* maliyet hesaplamasında dahil:

* CSP ve Dreamspark abonelik şu anda Azure DevTest Labs kullanır olarak desteklenmez [Azure fatura API'leri](../billing/billing-usage-rate-card-overview.md) maliyet, CSP veya Dreamspark abonelik desteklemiyor Laboratuvar hesaplamak için.
* Teklif ücretlerinizi. Şu anda, biz teklif ücretlerinizi (abonelik altında Microsoft veya Microsoft iş ortakları anlaşılan olduğunu gösterilen) kullanmanız mümkün değildir. Kullandıkça Öde oranları kullanıyoruz.
* Vergiler
* İndirim
* Fatura para birimi. Şu anda, Laboratuvar maliyet yalnızca ABD Doları para birimi görüntülenir.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>İlgili blog gönderileri
* [DevTest Labs kanalındaki maliyetinizi tutmak için iki daha fazla şey](https://blogs.msdn.microsoft.com/devtestlab/2016/06/21/keep-your-cost-on-track/)
* [Neden eşikleri maliyet?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/11/why-cost-thresholds/)

## <a name="next-steps"></a>Sonraki adımlar
Daha sonra deneyin gereken bazı şeyler şunlardır:

* [Laboratuvar ilkeleri tanımlar](devtest-lab-set-lab-policy.md) -Laboratuvarınızı ve Vm'lerini nasıl kullanıldığı yönetmek için kullanılan çeşitli ilkeler ayarlama öğrenin. 
* [Özel görüntü oluşturma](devtest-lab-create-template.md) - bir VM oluşturduğunuzda, özel bir görüntü veya bir Market görüntüsü olabilecek bir taban belirtin. Bu makalede, bir VHD dosyasından özel bir görüntü oluşturmak nasıl gösterilmektedir.
* [Market görüntülerini yapılandırma](devtest-lab-configure-marketplace-images.md) - VMs Azure Market görüntülerini temel oluşturmayı DevTest Labs destekler. Bu makalede, varsa, Azure Market görüntülerini olabilecek belirtmek verilmektedir VM'ler bir laboratuar ortamında oluşturulurken kullanılır.
* [Bir laboratuar ortamında bir VM oluşturma](devtest-lab-add-vm-with-artifacts.md) -temel bir görüntüden bir VM oluşturmak nasıl gösterir (ya da özel veya Market) ve VM yapıları çalışmak nasıl.

