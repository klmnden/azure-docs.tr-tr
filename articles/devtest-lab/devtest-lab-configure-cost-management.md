---
title: "Azure DevTest Labs'de aylık tahmini Laboratuvar maliyet eğilim görüntülemek | Microsoft Docs"
description: "Azure DevTest Labs aylık tahmini maliyet eğilim Grafiği hakkında bilgi edinin."
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 1f46fdc5-d917-46e3-a1ea-f6dd41212ba4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/22/2017
ms.author: v-craic
ms.openlocfilehash: 660180fac4f4743bc543b1babf7dbe921bf8c26b
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="view-the-monthly-estimated-lab-cost-trend-in-azure-devtest-labs"></a>Azure DevTest Labs'de aylık tahmini Laboratuvar maliyet eğilim görüntüleyin
DevTest Labs maliyet yönetim özelliği Laboratuvarınızı maliyetini izlemenize yardımcı olur. Bu makalede nasıl kullanılacağını anlatan **aylık tahmini maliyet eğilimi** geçerli Takvim ayı için geçerli Takvim ayın tahmini maliyet-to-date ve tahmini ayın son maliyet görüntülemek üzere grafik. Bu makalede ayrıca daha iyi harcama hedefleri ve eşikleri, erişildiğinde, tetikleyici sonuçları size bildirmek için DevTest Labs ayarlayarak Laboratuvar maliyetlerini yönetmek nasıl gösterir.

## <a name="viewing-the-monthly-estimated-cost-trend-chart"></a>Aylık tahmini maliyet eğilim grafiği görüntüleme
Aylık tahmini maliyet eğilim Grafiği görüntülemek için aşağıdaki adımları izleyin: 

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Gerekirse, seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden. (Laboratuvarınızı altında bir Pano üzerinde zaten görüntülenebilir **tüm kaynakları**).
1. İstenen Laboratuvar labs listesinden seçin.  
1. Laboratuvar 's üzerinde **genel bakış** alanında **yapılandırma ve ilkeleri**.   
1. Solda'nin altında **maliyet izleme**seçin **maliyet eğilimi**.

   Aşağıdaki ekran görüntüsünde bir maliyet grafik örneği gösterilmektedir. 
   
    ![Maliyet grafik](./media/devtest-lab-configure-cost-management/graph.png)

**Maliyet tahmini** geçerli Takvim ayın tahmini maliyet-to-date bir değerdir. **Tahmini maliyet** tahmini tüm geçerli Takvim ayı için önceki beş gün boyunca Laboratuvar maliyet kullanılarak hesaplanan maliyetidir.

Maliyet tutarlar sonraki tamsayıya yuvarlanır. Örneğin: 

* 5.01 6 adede kadar yuvarlar 
* 5.50 6 adede kadar yuvarlar
* 5.99 6 adede kadar yuvarlar

Grafik durumları gibi varsayılan hesap olarak gördüğünüz maliyetleri olduğundan *tahmini* kullanarak maliyetlerini [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) hızları sunar. Grafikler tarafından görüntülenen kendi harcama hedefleri de ayarlayabilirsiniz [Laboratuvarınızı maliyet hedeflerini yönetme.](#managing-cost-targets-for-your-lab)

Ayrıca, aşağıda verilmiştir *değil* maliyet hesaplamasında dahil:

* CSP ve Dreamspark abonelik şu anda Azure DevTest Labs kullanır olarak desteklenmez [Azure fatura API'leri](../billing/billing-usage-rate-card-overview.md) maliyet, CSP veya Dreamspark abonelik desteklemiyor Laboratuvar hesaplamak için.
* Teklif ücretlerinizi. Şu anda (abonelik altında Microsoft veya Microsoft iş ortakları anlaşılan olduğunu gösterilen) İndirim oranları kullanamazsınız. Yalnızca Kullandıkça Öde oranları kullanılır.
* Vergiler
* İndirim
* Fatura para birimi. Şu anda, Laboratuvar maliyet yalnızca ABD Doları para birimi görüntülenir.

## <a name="managing-cost-targets-for-your-lab"></a>Laboratuvarınız için maliyet hedefleri yönetme
DevTest Labs aylık tahmini maliyet eğilim Grafiği sonra görüntüleyebileceğiniz bir harcama hedef ayarlayarak laboratuvarınızda maliyetleri daha iyi yönetmenize olanak tanır. Belirtilen hedef harcama veya Eşiğe ulaşıldığında DevTest Labs Ayrıca, bir bildirim gönderebilirsiniz. 

1. Laboratuvarınızı 's üzerinde **genel bakış** bölmesinde, **yapılandırma ve ilkeleri**.
1. Solda'nin altında **maliyet izleme**seçin **maliyet eğilimi**.

    ![Hedef düğmesi yönetme](./media/devtest-lab-configure-cost-management/cost-trend.png)

1. İçinde **maliyet eğilimi** bölmesinde, **Yönet hedef**.

    ![Hedef düğmesi yönetme](./media/devtest-lab-configure-cost-management/cost-trend-manage-target.png)

1. Yönet hedef bölmesinde istenen harcama hedef ve eşikleri belirtin. Seçilen her eşiği maliyet eğilimi grafik veya bir Web kancası bildirim aracılığıyla bildirilir olup olmadığını da ayarlayabilirsiniz.

    ![Hedef bölmesinde yönetme](./media/devtest-lab-configure-cost-management/cost-trend-manage-target-pane.png)

   - İzlenen maliyet hedefleri istediğiniz zaman aralığını seçin.
      - **Aylık**: Maliyet hedefleri, her ay izlenir.
      - **Sabit**: Maliyet hedefleri başlangıç tarihi ve bitiş tarihi alanları, belirttiğiniz tarih aralığı için izlenir. Genellikle, bu ne kadar süreyle projenizi çalışacak şekilde zamanlanır ile karşılık gelebilir.
   - Belirtin bir **hedef maliyet**. Örneğin, bu, üzerinde bu Laboratuvar tanımladığınız dönemde harcadığı planladığınız ne kadar olabilir.
   - Etkinleştirme veya devre dışı herhangi eşik seçin, istediğiniz bildirilen – % 25 – artışlarla %125 kadar belirtilen **hedef maliyet**.
      - **Bildirim**: Bu eşiğine ulaşıldığında, belirttiğiniz bir Web kancası URL'si bildirilir.
      - **Çizim grafikte**: Bu eşiğine ulaşıldığında sonuçları görüntüleyebilir, maliyet eğilimi grafikte açıklandığı gibi çizilir [aylık tahmini maliyet eğilim grafiği görüntüleme](#viewing-the-monthly-estimated-cost-trend-chart).
   - İsterseniz **bildirim** eşiğine ulaşıldığında, bir Web kancası URL'si belirtmeniz gerekir. Maliyet tümleştirmeler bölmesinde seçin **bir tümleştirme eklemek için burayı tıklatın**.

      Yapılandırma bildirim Bölmesi'nde bir Web kancası URL'si girin ve ardından **Tamam**.

       ![Bildirim bölmesini Yapılandır](./media/devtest-lab-configure-cost-management/configure-notification.png)

      - Belirtirseniz **bildirim**, bir Web kancası URL'si tanımlamanız gerekir.
      - Benzer şekilde, bir Web kancası URL'si tanımlarsanız ayarlamalısınız **bildirim** için **üzerinde** maliyet eşiği bölmesinde.
      - Bir Web kancası burada girme önce oluşturmanız gerekir.  

      Web kancası hakkında daha fazla bilgi için bkz: [bir Web kancası veya API Azure işlevi oluşturma](../azure-functions/functions-create-a-web-hook-or-api-function.md). 
 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>İlgili blog gönderileri
* [DevTest Labs kanalındaki maliyetinizi tutmak için iki daha fazla şey](https://blogs.msdn.microsoft.com/devtestlab/2016/06/21/keep-your-cost-on-track/)
* [Neden eşikleri maliyet?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/11/why-cost-thresholds/)

## <a name="next-steps"></a>Sonraki adımlar
Daha sonra deneyin gereken bazı şeyler şunlardır:

* [Laboratuvar ilkeleri tanımlar](devtest-lab-set-lab-policy.md) -Laboratuvarınızı ve Vm'lerini nasıl kullanıldığı yönetmek için kullanılan çeşitli ilkeler ayarlama öğrenin. 
* [Özel görüntü oluşturma](devtest-lab-create-template.md) - bir VM oluşturduğunuzda, özel bir görüntü veya bir Market görüntüsü olabilecek bir taban belirtin. Bu makalede, bir VHD dosyasından özel bir görüntü oluşturmak nasıl gösterilmektedir.
* [Market görüntülerini yapılandırma](devtest-lab-configure-marketplace-images.md) - VMs Azure Market görüntülerini temel oluşturmayı DevTest Labs destekler. Bu makalede, varsa, Azure Market görüntülerini olabilecek belirtmek verilmektedir VM'ler bir laboratuar ortamında oluşturulurken kullanılır.
* [Bir laboratuar ortamında bir VM oluşturma](devtest-lab-add-vm.md) -temel bir görüntüden bir VM oluşturmak nasıl gösterir (ya da özel veya Market) ve VM yapıları çalışmak nasıl.

