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
ms.date: 03/07/2019
ms.author: spelluru
ms.openlocfilehash: f761af3a5a3f08e4da89d8869aea5d666ecd69d0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60868284"
---
# <a name="track-costs-associated-with-a-lab-in-azure-devtest-labs"></a>Azure DevTest labs'deki bir laboratuvara ile ilişkili maliyetleri izleme
Bu makalede, laboratuvarınız maliyetini izleme konusunda bilgi sağlar. Tahmini görüntüleme gösterir trent geçerli Takvim ayı boyunca Laboratuvar maliyeti. Makalede ayrıca kaynak başına aylık tarih maliyeti laboratuvarda görüntüleme gösterilmektedir.

## <a name="view-the-monthly-estimated-lab-cost-trend"></a>Aylık tahmini Laboratuvar maliyeti eğilimini görüntüleyin 
Bu bölümde, nasıl kullanılacağını öğrenin **aylık tahmini maliyet eğilimi** geçerli Takvim ayı boyunca geçerli Takvim ayki tahmini maliyet başından bu yana ve tahmini aylık son maliyet görüntülemek için grafik. Ayrıca harcama hedeflere ve eşiklere, erişildiğinde, tetikleyici sonuçları size bildirmek için DevTest Labs ayarlayarak Laboratuvar maliyetleri yönetmeyi öğrenin.

Aylık tahmini maliyet eğilimi grafiği görüntülemek için aşağıdaki adımları izleyin: 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
3. Laboratuvarınızı labs listesinden seçin.  
4. Seçin **yapılandırması ve ilkelerini** sol menüsünde.  
4. Seçin **maliyet eğilimi** içinde **Maliyet İzlemesi** sol menüde bölümü. Aşağıdaki ekran görüntüsünde, maliyet grafik örneği gösterilmektedir. 
   
    ![Maliyet grafiği](./media/devtest-lab-configure-cost-management/graph.png)

    **Tahmini maliyet** geçerli Takvim ayki tahmini maliyet başından bu yana bir değerdir. **Öngörülen maliyet** Laboratuvar maliyeti önceki beş gün boyunca kullanılarak hesaplanan tüm geçerli Takvim ayı boyunca tahmini maliyeti olduğundan.

    Ücret tutarları, sonraki tam sayıya yuvarlanır. Örneğin: 

   * en fazla 6 5.01 yuvarlar 
   * en fazla 6 5.50 yuvarlar
   * en fazla 6 5.99 yuvarlar

     Grafiğin üst kısmındaki durumları gibi varsayılan olarak grafikte gördüğünüz ücretlerdir *tahmini* kullanarak maliyetleri [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) hızları sunar. Grafikler tarafından görüntülenen kendi harcama hedefleri de ayarlayabilirsiniz [laboratuvarınız için maliyet hedefleri yönetme.](#managing-cost-targets-for-your-lab)

     Aşağıdaki maliyetlerinin *değil* maliyetini hesaplamaya dahil:

   * CSP ve Dreamspark abonelikleri şu anda desteklenmiyor Azure DevTest Labs kullandıkça [Azure faturalandırma API'leri](../billing/billing-usage-rate-card-overview.md) maliyet, CSP veya Dreamspark abonelikleri desteklemez Laboratuvar hesaplamak için.
   * Teklif ücretlerinizi. Şu anda (aboneliğiniz kapsamındaki, Microsoft veya Microsoft ile iş ortakları anlaşılan olduğunu gösterilen) teklif ücretler kullanamazsınız. Yalnızca Kullandıkça Öde tarifesine göre kullanılır.
   * Vergileri
   * Slevy
   * Faturalandırma, para birimi. Laboratuvar maliyeti şu anda yalnızca ABD Doları para biriminde görüntülenir.

### <a name="managing-cost-targets-for-your-lab"></a>Laboratuvarınız için maliyet hedefleri yönetme
DevTest Labs aylık tahmini maliyet eğilimi grafiği daha sonra görüntüleyebileceğiniz bir harcama hedef ayarlayarak laboratuvarınızda maliyetleri daha iyi yönetmenize olanak tanır. Belirtilen hedef harcama veya Eşiğe ulaşıldığında DevTest Labs Ayrıca, bir bildirim gönderebilirsiniz. 

1. Üzerinde **maliyet eğilimi** sayfasında **Yönet hedef**.

    ![Hedef düğmesi yönetme](./media/devtest-lab-configure-cost-management/cost-trend-manage-target.png)
2. Üzerinde **Yönet hedef** harcama hedef ve eşikleri belirtin. Seçilen her eşiği maliyet eğilimi grafiği veya bir Web kancası bildirim aracılığıyla bildirilir olmadığını de ayarlayabilirsiniz.

    ![Hedef bölmesinde yönetme](./media/devtest-lab-configure-cost-management/cost-trend-manage-target-pane.png)

   - İzlenen maliyet hedefleri istediğiniz bir zaman aralığı seçin.
      - **Aylık**: aylık maliyet hedefleri izlenir.
      - **Sabit**: Maliyet hedefleri, başlangıç ve bitiş tarihleri belirttiğiniz tarih aralığı için izlenir. Genellikle, bu değerleri projeniz ne kadar süre çalışacak şekilde zamanlanırsa temsil eder.
   - Belirtin bir **hedef maliyet**. Örneğin, ne kadar bu Laboratuvar tanımladığınız dönemdeki harcayabileceğiniz planlayın.
   - Etkinleştirme veya devre dışı herhangi bir eşik seçin, istediğiniz bildirilen – – % 25 artışlarla kadar kaynağının % 125, belirtilen **hedef maliyet**.
      - **Bildirim**: Bu eşik geçildikten sonra size belirttiğiniz bir Web kancası URL'si bildirilir.
      - **Grafik çizim**: Bu eşik geçildikten sonra sonuçları görüntüleyebilir, maliyet eğilimi grafiği aylık tahmini maliyet eğilimi grafiği görüntüleme içinde anlatıldığı gibi çizilir.
   - İsterseniz **bildirim** eşiğine ulaşıldığında, bir Web kancası URL'si belirtmeniz gerekir. Maliyet tümleştirmeler alanında seçin **bir tümleştirme eklemek için burayı tıklatın**. Girin bir **Web kancası URL'si** yapılandırma bildirim bölmesi ve ardından **Tamam**.

       ![Bildirim bölmesini Yapılandır](./media/devtest-lab-configure-cost-management/configure-notification.png)

     - Belirtirseniz **bildirim**, bir Web kancası URL'si tanımlamanız gerekir.
     - Benzer şekilde, bir Web kancası URL'si tanımlarsanız, ayarlamalısınız **bildirim** için **üzerinde** maliyet eşiği bölmesinde.
     - Buraya girilmesi önce bir Web kancası oluşturmanız gerekir.  

       Web kancaları hakkında daha fazla bilgi için bkz: [bir Web kancası veya API Azure işlevi oluşturma](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

## <a name="view-cost-by-resource"></a>Kaynak Görünümü maliyeti 
Aylık maliyet eğilimi özellik Labs, geçerli Takvim ayı içinde ne kadar geçirdiklerini sağlar. Ayrıca, son yedi gün içinde harcama temel ay sonuna kadar harcama projeksiyon gösterir. Neden, laboratuvarda harcama eşikleri toplantı olduğunu anlamanıza yardımcı olması için erken aşamalarda, kullanabileceğiniz **kaynağı ile maliyet** ay başından bu yana maliyetini gösteren özelliği **kaynak başına** bir tablodaki.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
3. İstenen Laboratuvar labs listesinden seçin.  
4. Seçin **yapılandırması ve ilkelerini** sol menüsünde.
5. Seçin **kaynağı ile maliyet** içinde **Maliyet İzlemesi** sol menüde bölümü. Bir laboratuvar ile ilişkili her kaynakla ilişkili maliyetleri görürsünüz. 

    ![Kaynağa göre maliyet](./media/devtest-lab-configure-cost-management/cost-by-resource.png)

Bu özellik, maliyet kaynakları kolayca belirlemenize yardımcı olur böylece en çok harcama Laboratuvar azaltmak için eylemleri gerçekleştirebilirsiniz. Örneğin, bir VM maliyetini VM boyutuna bağlıdır. Daha fazla VM'nin boyutunu maliyeti büyüktür. Böyle bir VM boyutu neden gereklidir anlamak için VM sahibinin konuşabilirsiniz. böylece bir VM boyutuna ve sahibi, kolayca bulabilirsiniz ve boyutunu düşürmek için bir fırsat olup olmadığı.

[Otomatik kapatma ilke](devtest-lab-get-started-with-lab-policies.md#set-auto-shutdown) günün belirli bir zamanda Laboratuvar Vm'leri kapatarak maliyetini azaltmak için yardımcı olur. Ancak, bir laboratuvar kullanıcı VM çalıştırma maliyetini artıran kapatma ilkelerin dışına tercih edebilirsiniz. Bu seçimi yaptıysanız otomatik kapatma ilkeleri için uğradı varsa görmek için tabloda bir sanal makine seçebilirsiniz. Bu durumda, neden VM kabul ilke uğradı bulmak için VM sahibinin konuşabilirsiniz.
 
## <a name="next-steps"></a>Sonraki adımlar
Daha sonra deneyin gereken bazı noktalar şunlardır:

* [Laboratuvar ilkeleri tanımlama](devtest-lab-set-lab-policy.md) -Laboratuvarınızı ve Vm'lerini nasıl kullanıldığını yönetmek için çeşitli ilkeler ayarlama hakkında bilgi edinin. 
* [Özel görüntü oluşturma](devtest-lab-create-template.md) - bir VM oluşturduğunuzda, belirttiğiniz bir temel veya özel bir görüntü, hem de bir Market görüntüsü olabilir. Bu makalede, bir VHD dosyasından bir özel görüntü oluşturma işlemini göstermektedir.
* [Market görüntülerini yapılandırma](devtest-lab-configure-marketplace-images.md) - Azure Market görüntüleri temel alan VM'ler oluşturma DevTest Labs destekler. Bu makalede, varsa, Azure Market görüntüleri olabilecek belirtmek verilmektedir bir laboratuvarda VM oluşturulurken kullanılan.
* [Bir laboratuvarda VM oluşturma](devtest-lab-add-vm.md) -bir temel görüntüden VM oluşturma işlemini gösterir (ya da özel veya Market) ve vm'nizde yapıları ile çalışma.

