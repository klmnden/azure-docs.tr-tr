---
title: Azure DevTest Labs'de aylık tahmini Laboratuvar maliyeti eğilimini görüntüleme | Microsoft Docs
description: Azure DevTest Labs aylık tahmini maliyet eğilimi grafiği hakkında bilgi edinin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 1f46fdc5-d917-46e3-a1ea-f6dd41212ba4
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: spelluru
ms.openlocfilehash: e616df772bf11d1247f96c78bea2392252f5e5d0
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51259760"
---
# <a name="view-the-monthly-estimated-lab-cost-trend-in-azure-devtest-labs"></a>Azure DevTest Labs'de aylık tahmini Laboratuvar maliyeti eğilimini görüntüleyin
Maliyet yönetimi özelliği DevTest Labs Laboratuvarınızı maliyetini izlemenize yardımcı olur. Bu makalede nasıl kullanılacağı gösterilmektedir **aylık tahmini maliyet eğilimi** geçerli Takvim ayı boyunca geçerli Takvim ayki tahmini maliyet başından bu yana ve tahmini aylık son maliyet görüntülemek için grafik. Bu makalede ayrıca harcama hedeflere ve eşiklere, erişildiğinde, tetikleyici sonuçları size bildirmek için DevTest Labs ayarlayarak Laboratuvar maliyetlerini daha iyi yönetmek nasıl gösterir.

## <a name="viewing-the-monthly-estimated-cost-trend-chart"></a>Aylık tahmini maliyet eğilimi grafiği görüntüleme
Aylık tahmini maliyet eğilimi grafiği görüntülemek için aşağıdaki adımları izleyin: 

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Gerekirse, seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden. (Laboratuvarınızı zaten altında Panoda görüntülenebilir **tüm kaynakları**).
1. İstenen Laboratuvar labs listesinden seçin.  
1. Laboratuvar'ın **genel bakış** alanında **yapılandırması ve ilkelerini**.   
1. Soldaki altında **maliyet izleme**seçin **maliyet eğilimi**.

   Aşağıdaki ekran görüntüsünde, maliyet grafik örneği gösterilmektedir. 
   
    ![Maliyet grafiği](./media/devtest-lab-configure-cost-management/graph.png)

**Tahmini maliyet** geçerli Takvim ayki tahmini maliyet başından bu yana bir değerdir. **Öngörülen maliyet** Laboratuvar maliyeti önceki beş gün boyunca kullanılarak hesaplanan tüm geçerli Takvim ayı boyunca tahmini maliyeti olduğundan.

Ücret tutarları, sonraki tam sayıya yuvarlanır. Örneğin: 

* en fazla 6 5.01 yuvarlar 
* en fazla 6 5.50 yuvarlar
* en fazla 6 5.99 yuvarlar

Grafiğin üst kısmındaki durumları gibi varsayılan olarak grafikte gördüğünüz ücretlerdir *tahmini* kullanarak maliyetleri [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) hızları sunar. Grafikler tarafından görüntülenen kendi harcama hedefleri de ayarlayabilirsiniz [laboratuvarınız için maliyet hedefleri yönetme.](#managing-cost-targets-for-your-lab)

Ayrıca, aşağıdakiler, *değil* maliyetini hesaplamaya dahil:

* CSP ve Dreamspark abonelikleri şu anda desteklenmiyor Azure DevTest Labs kullandıkça [Azure faturalandırma API'leri](../billing/billing-usage-rate-card-overview.md) maliyet, CSP veya Dreamspark abonelikleri desteklemez Laboratuvar hesaplamak için.
* Teklif ücretlerinizi. Şu anda (aboneliğiniz kapsamındaki, Microsoft veya Microsoft ile iş ortakları anlaşılan olduğunu gösterilen) teklif ücretler kullanamazsınız. Yalnızca Kullandıkça Öde tarifesine göre kullanılır.
* Vergileri
* Slevy
* Faturalandırma, para birimi. Laboratuvar maliyeti şu anda yalnızca ABD Doları para biriminde görüntülenir.

## <a name="managing-cost-targets-for-your-lab"></a>Laboratuvarınız için maliyet hedefleri yönetme
DevTest Labs aylık tahmini maliyet eğilimi grafiği daha sonra görüntüleyebileceğiniz bir harcama hedef ayarlayarak laboratuvarınızda maliyetleri daha iyi yönetmenize olanak tanır. Belirtilen hedef harcama veya Eşiğe ulaşıldığında DevTest Labs Ayrıca, bir bildirim gönderebilirsiniz. 

1. Üzerinde Laboratuvar **genel bakış** bölmesinde **yapılandırması ve ilkelerini**.
1. Soldaki altında **maliyet izleme**seçin **maliyet eğilimi**.

    ![Hedef düğmesi yönetme](./media/devtest-lab-configure-cost-management/cost-trend.png)

1. İçinde **maliyet eğilimi** bölmesinde **Yönet hedef**.

    ![Hedef düğmesi yönetme](./media/devtest-lab-configure-cost-management/cost-trend-manage-target.png)

1. Yönet hedef bölmesinde istediğiniz harcama hedef ve eşikleri belirtin. Seçilen her eşiği maliyet eğilimi grafiği veya bir Web kancası bildirim aracılığıyla bildirilir olmadığını de ayarlayabilirsiniz.

    ![Hedef bölmesinde yönetme](./media/devtest-lab-configure-cost-management/cost-trend-manage-target-pane.png)

   - İzlenen maliyet hedefleri istediğiniz bir zaman aralığı seçin.
      - **Aylık**: aylık maliyet hedefleri izlenir.
      - **Sabit**: Maliyet hedefleri, başlangıç tarihi ve bitiş tarihi alanları belirttiğiniz tarih aralığı için izlenir. Genellikle, bu ne kadar süreyle projenizi çalışacak şekilde zamanlanırsa ile karşılık gelebilir.
   - Belirtin bir **hedef maliyet**. Örneğin, bu bu Laboratuvar harcama tanımladığınız dönemdeki planlıyorsanız ne kadar olabilir.
   - Etkinleştirme veya devre dışı herhangi bir eşik seçin, istediğiniz bildirilen – – % 25 artışlarla kadar kaynağının % 125, belirtilen **hedef maliyet**.
      - **Bildirim**: Bu eşiğine ulaşıldığında, belirttiğiniz bir Web kancası URL'si bildirilir.
      - **Grafik çizim**: Bu eşiğine ulaşıldığında, sonuçları görüntüleyebilir, maliyet eğilimi grafikte açıklandığı gibi çizilir [aylık tahmini maliyet eğilimi grafiği görüntüleme](#viewing-the-monthly-estimated-cost-trend-chart).
   - İsterseniz **bildirim** eşiğine ulaşıldığında, bir Web kancası URL'si belirtmeniz gerekir. Maliyet tümleştirmeler alanında seçin **bir tümleştirme eklemek için burayı tıklatın**.

      Yapılandırma bildirim Bölmesi'nde bir Web kancası URL'sini girin ve ardından **Tamam**.

       ![Bildirim bölmesini Yapılandır](./media/devtest-lab-configure-cost-management/configure-notification.png)

      - Belirtirseniz **bildirim**, bir Web kancası URL'si tanımlamanız gerekir.
      - Benzer şekilde, bir Web kancası URL'si tanımlarsanız, ayarlamalısınız **bildirim** için **üzerinde** maliyet eşiği bölmesinde.
      - Buraya girilmesi önce bir Web kancası oluşturmanız gerekir.  

      Web kancaları hakkında daha fazla bilgi için bkz: [bir Web kancası veya API Azure işlevi oluşturma](../azure-functions/functions-create-a-web-hook-or-api-function.md). 
 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>İlgili blog gönderileri
* [DevTest Labs kanalındaki maliyetinizi tutmak için iki daha fazla şey](https://blogs.msdn.microsoft.com/devtestlab/2016/06/21/keep-your-cost-on-track/)
* [Neden maliyet eşiklerine?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/11/why-cost-thresholds/)

## <a name="next-steps"></a>Sonraki adımlar
Daha sonra deneyin gereken bazı noktalar şunlardır:

* [Laboratuvar ilkeleri tanımlama](devtest-lab-set-lab-policy.md) -Laboratuvarınızı ve Vm'lerini nasıl kullanıldığını yönetmek için çeşitli ilkeler ayarlama hakkında bilgi edinin. 
* [Özel görüntü oluşturma](devtest-lab-create-template.md) - bir VM oluşturduğunuzda, belirttiğiniz bir temel veya özel bir görüntü, hem de bir Market görüntüsü olabilir. Bu makalede, bir VHD dosyasından bir özel görüntü oluşturma işlemini göstermektedir.
* [Market görüntülerini yapılandırma](devtest-lab-configure-marketplace-images.md) - Azure Market görüntüleri temel alan VM'ler oluşturma DevTest Labs destekler. Bu makalede, varsa, Azure Market görüntüleri olabilecek belirtmek verilmektedir bir laboratuvarda VM oluşturulurken kullanılan.
* [Bir laboratuvarda VM oluşturma](devtest-lab-add-vm.md) -bir temel görüntüden VM oluşturma işlemini gösterir (ya da özel veya Market) ve vm'nizde yapıları ile çalışma.

