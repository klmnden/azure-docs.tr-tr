---
title: "Kullanım ve Azure maliyeti yönetim maliyetlerini gözden geçirin. | Microsoft Docs"
description: "Kullanım ve eğilimlerini izlemek, verimsiz algılamak ve Uyarıları oluşturmak için maliyetlerini gözden geçirin."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 36ebffb41211e443cc1619df46f50247945cc57c
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2017
---
# <a name="review-usage-and-costs"></a>Gözden geçirme kullanım ve maliyetler

Azure maliyeti Yönetimi Cloudyn tarafından kullanımını gösterir ve maliyetleri eğilimleri, izleyebilmesi verimsiz algılamak ve uyarılar oluşturabilir. Tüm kullanım ve maliyet verilerini Cloudyn panolar ve raporlar görüntülenir. Bu öğretici örneklerde, kullanım ve panoları ve raporları kullanarak maliyetlerini gözden geçirerek olsa yol. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Kullanımı izlemek ve eğilimleri maliyet
> * Kullanım verimsiz Algıla
> * Olağan dışı harcama veya overspending için uyarı oluşturma



## <a name="open-the-cloudyn-portal"></a>Cloudyn portalını açın

Tüm kullanım ve Cloudyn portal maliyetlerini gözden geçirin. Azure portalından Cloudyn portalını açın veya https://app.cloudyn.com için gidin ve oturum açın.

## <a name="track-usage-and-cost-trends"></a>Kullanımı izlemek ve eğilimleri maliyet

Kullanım ve eğilimleri belirlemek için zaman içinde raporlarla maliyetleri için harcanan gerçek para izler. Eğilimleri aramaya başlamak için gerçek zamanlı maliyet raporunu kullanın. Portal üstündeki raporları menüsünde **maliyet** > **maliyet analizi** > **gerçek zamanlı maliyet**. Raporun ilk açtığınızda, hiçbir grup veya filtreleri için uygulanır.

Bir örnek raporu şöyledir:

![örnek raporu](./media/tutorial-review-usage/actual-cost01.png)

Bu rapor, son 30 gün içinde tüm harcama gösterir. Yalnızca Azure Hizmetleri için harcama görüntülemek için hizmet grubuna Uygula ve tüm Azure Hizmetleri için filtre. Aşağıdaki resimde filtrelenmiş hizmetleri gösterir.

![filtrelenmiş Hizmetleri](./media/tutorial-review-usage/actual-cost02.png)

Önceki örnekte küçük para 2017-08-31'den önce başlangıç harcandığını. Bu maliyet eğilimi yaklaşık dokuz gün için çeşitli hizmetler için devam eder. Sonra Ek harcama önceki gibi devam eder. Ancak, çok fazla sayıda sütun belirgin bir eğilim soyutlamaması. Diğer görünümlerinde görüntülenen verileri görmek için bir satır veya alan grafiği rapor görünümü değiştirebilirsiniz. Aşağıdaki resimde daha net bir şekilde eğilimi gösterir.

![Rapor eğilimi](./media/tutorial-review-usage/actual-cost03.png)

Örnekte, açıkça Azure Storage diğer Azure hizmetlerinde harcama düzeyi kalan sırada bırakılan 2017-08-31 başlangıç maliyeti görürsünüz. Bu nedenle, bu azaltma harcama içinde nedenini? Bu örnekte, bazı çalışanların iş çıktığınızda tatilde olan ve depolama hizmeti kullanmadı.

Kullanım ve maliyet eğilimleri izleme hakkında öğretici bir video izlemek için bkz: [veri Azure maliyeti Yönetimi Cloudyn tarafından süresiyle ve fatura, bulut çözümleme](https://youtu.be/7LsVPHglM0g).

## <a name="detect-usage-inefficiencies"></a>Kullanım verimsiz Algıla

İyileştirici raporları verimliliğini artırır, kullanımını en iyi duruma ve, bulut kaynaklarınızı harcanan paradan tasarruf yolları tanımlayın. Bunlar, düşük maliyetli boyutlandırma önerileri boşta veya pahalı VM azaltmaya yardımcı olmak için amaçlanan özellikle yararlı olur.

Bunlar başlangıçta kaynakları buluta taşıdığınızda, kuruluşların etkileyen bir ortak kendi sanallaştırma stratejisini sorunudur. Bunlar genellikle şirket içi sanallaştırma ortamı için sanal makineler oluşturmak için kullanılan benzer bir yaklaşım kullanın. Ve olarak buluta, şirket içi Vm'leri taşıyarak maliyetleri azaltılır varsayın-değil. Ancak, bu yaklaşım maliyetlerini azaltmak olası değildir.

Varolan altyapılarını için zaten ödenmiş sorunudur. Kullanıcılar oluşturma ve bunların beğendiğinizi çalıştıran büyük VM'ler tutmak — boşta veya değil ve çok az sonucu. Büyük veya boşta VM'ler buluta taşımak için büyük olasılıkla *artırmak* maliyetleri. Bulut hizmeti sağlayıcıları ile anlaşmalara girdiğinizde maliyet ayırma kaynakları için önemlidir. Neler olup, kaynak tam olarak kullanıp için yürüttükten için ödemeniz gerekir.

Maliyet etkili boyutlandırma önerileri raporu VM örneği türü Kapasite geçmiş CPU ve bellek kullanımı verileri karşılaştırarak olası yıllık tasarrufları tanımlar.  

Portal üstündeki raporları menüsünde **iyileştirici** > **fiyatlandırma iyileştirme** > **maliyet etkili boyutlandırma önerileri**. Yalnızca Azure Vm'leri aramak için Azure sağlayıcıya filtreleyin. Burada, örnek görüntüyü verilmiştir.

![Azure VM’leri](./media/tutorial-review-usage/sizing01.png)

Bu örnekte, $3,114 VM örneği türlerini değiştirmek için öneriler izleyerek kaydedilmiş. Artı (+) altında simgesini **ayrıntıları** ilk öneri için. Burada, ilk öneri hakkında ayrıntılar verilmiştir.

![öneri ayrıntıları](./media/tutorial-review-usage/sizing02.png)

VM örneği kimlikleri yanındaki artı simgesini tıklatarak görüntülemek **listesi, aday**.

![Aday listesi](./media/tutorial-review-usage/sizing03.png)

Kullanım verimsiz algılama hakkında öğretici bir video izlemek için bkz: [Cloudyn tarafından Yönetimi maliyeti Azure VM boyutu en iyi duruma getirme](https://youtu.be/1xaZBNmV704).

## <a name="create-alerts-for-unusual-spending"></a>Olağan dışı harcama için uyarı oluşturma

Harcama anormalliklerini ve overspending riskler için otomatik olarak Paydaşlar uyarabilir. Destek uyarıları bütçeye bağlı ve eşikleri maliyet raporları kullanarak uyarıları hızla ve kolayca oluşturabilirsiniz.

Kullanan tüm harcama herhangi bir Maliyet raporu için bir uyarı oluşturabilir. Bu örnekte, gerçek zamanlı maliyeti raporu Azure VM harcama toplam bütçenizi yaklaştığında sizi bilgilendirmek üzere kullanın. Portal üstündeki raporları menüsünde **maliyet** > **maliyet analizi** > **gerçek zamanlı maliyet**. Ayarlamak **grupları** için **hizmet** ve **filtresi hizmet üzerinde** için **Azure/VM**. Üst raporu sağ tıklatın **Eylemler** ve ardından **zamanlama rapor**.

Kullanım **zamanlama** kendiniz raporu istediğiniz sıklığı kullanarak, bir e-posta göndermek için sekme. Gruplandırma ve kullanılan filtre tüm etiketleri, e-postayla gönderilmiş rapora dahil edilir. Tıklatın **eşik** sekmesini'nü seçip **gerçek maliyet vs. Eşik**. Maliyetleri yarısı yakınında olduğunda toplam bütçe $500.000 ve, istediği bildirim olsaydı oluşturma bir **kırmızı bir uyarı** $250,000 adresindeki ve **sarı uyarı** $240,000 adresindeki. Ardından, ardışık uyarıların sayısını seçin. Belirttiğiniz uyarıların toplam sayısı aldığınızda, hiçbir ek uyarı gönderilir. Zamanlanmış rapor kaydedin.

![örnek raporu](./media/tutorial-review-usage/schedule-alert01.png)

Ayrıca maliyet yüzdesini vs seçebilirsiniz. Uyarılar oluşturmak için eşik ölçüm bütçe. Bu ölçüm kullanarak para birimi değerleri yerine bütçe yüzdeleri kullanabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Kullanımı izlemek ve eğilimleri maliyet
> * Kullanım verimsiz Algıla
> * Olağan dışı harcama veya overspending için uyarı oluşturma


Veri erişimini denetleme hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Veri erişimi denetleme](tutorial-user-access.md)
