---
title: 'Öğretici: Azure’da Cloudyn ile kullanım ve maliyetleri gözden geçirme | Microsoft Docs'
description: Bu öğreticide, eğilimleri takip etmek, verimsizlikleri algılamak ve uyarılar oluşturmak için kullanımı ve maliyetleri gözden geçirirsiniz.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/20/2019
ms.topic: tutorial
ms.service: cost-management
ms.custom: seodec18
manager: benshy
ms.openlocfilehash: 2b701e618bdcbd50709360c55cfb9ceb68781ebc
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969093"
---
<!-- Intent: As a cloud-consuming user, I need to view usage and costs for my cloud resources and services.
-->

# <a name="tutorial-review-usage-and-costs"></a>Öğretici: Kullanımı ve maliyetleri gözden geçirme

Cloudyn, eğilimleri takip edebilmeniz, verimsizlikleri algılamanız ve uyarılar oluşturmanız için kullanım ve maliyet bilgilerini gösterir. Tüm kullanım ve maliyet verileri, Cloudyn panoları ve raporlarında gösterilir. Bu öğreticideki örneklerde, pano ve raporları kullanarak kullanım ve maliyetleri gözden geçirme işlemi açıklanmaktadır.

Azure Maliyet Yönetimi, Cloudyn'e benzer işlevler sunar. Azure Maliyet Yönetimi, yerel Azure maliyet yönetimi çözümüdür. Maliyet analizi yapmanıza, bütçe oluşturup yönetmenize, verileri dışarı aktarmanıza ve tasarruf önerilerini gözden geçirip gerekli eylemleri gerçekleştirmenize yardımcı olur. Daha fazla bilgi için bkz. [Azure Maliyet Yönetimi](overview-cost-mgt.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Kullanım ve maliyet eğilimlerini izleme
> * Kullanım verimsizliklerini algılama
> * Olağan dışı harcama veya aşırı harcama uyarıları oluşturma
> * Verileri dışarı aktarma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

- Azure hesabınız olmalıdır.
- Cloudyn için bir deneme kaydına veya ücretli aboneliğe sahip olmanız gerekir.

## <a name="open-the-cloudyn-portal"></a>Cloudyn portalını açın

Tüm kullanım ve maliyet bilgilerini Cloudyn portalında gözden geçirirsiniz. Cloudyn portalını Azure portalından açın veya https://azure.cloudyn.com sayfasına gidip oturum açın.

## <a name="track-usage-and-cost-trends"></a>Kullanım ve maliyet eğilimlerini izleme

Eğilimleri belirlemek için Zaman İçinde raporları ile kullanım maliyetler için harcanan gerçek parayı izleyebilirsiniz. Eğilimlere bakmaya başlamak için Zaman İçinde Gerçek Maliyet raporunu kullanın. Portalın sol üst kısmından **Maliyet** > **Maliyet Analizi** > **Zaman İçinde Gerçek Maliyet**’e tıklayın. Raporu ilk kez açtığınızda, rapor uygulanmış bir grup ya da filtre yoktur.

Örnek bir rapor aşağıda verilmiştir:

![Örnek zaman içinde gerçek maliyet raporu](./media/tutorial-review-usage/actual-cost01.png)

Raporda son 30 gün içinde yapılan tüm harcamalar gösterilir. Yalnızca Azure hizmetlerine ilişkin harcamaları görüntülemek için Hizmet grubunu uygulayın ve ardından tüm Azure hizmetleri için filtreleyin. Aşağıdaki resimde filtrelenmiş hizmetler gösterilmektedir.

![Azure Hizmetleri için filtrelenmiş örnek gösteriliyor](./media/tutorial-review-usage/actual-cost02.png)

Yukarıdaki örnekte 29.10.2018 tarihinden itibaren, önceki döneme göre daha az para harcanmıştır. Ancak, çok fazla sayıda sütun kullanılması, belirgin bir eğilimi belirsiz hale getirebilir. Diğer görünümlerde gösterilen verileri görmek için rapor görünümünü bir satır veya alan grafiği ile değiştirebilirsiniz. Aşağıdaki resimde eğilim daha net bir şekilde gösterilmektedir.

![Azalan bir Azure VM maliyeti eğilimini gösteren örnek](./media/tutorial-review-usage/actual-cost03.png)

Bu örnek üzerinden devam edecek olursak Azure VM maliyetlerinin düştüğünü görebilirsiniz. Ayrıca diğer Azure hizmetlerinin maliyetleri de aynı günde düşmeye başlamıştır. Öyleyse, harcamadaki bu azalmanın nedeni nedir? Bu örnekte büyük çaplı bir proje tamamlandığı için çoğu Azure hizmetinin kullanım oranı düşmüştür.

Kullanım ve maliyet eğilimlerini izleme hakkında öğretici bir video izlemek için bkz. [Cloudyn ile bulut faturalama verilerinizi zamanla karşılaştırmalı olarak çözümleme](https://youtu.be/7LsVPHglM0g).

## <a name="detect-usage-inefficiencies"></a>Kullanım verimsizliklerini algılama

İyileştirici raporlar verimliliği artırır, kullanımını en iyi duruma getirir ve bulut kaynaklarınız için harcanan paradan tasarruf etmenin yollarını tanımlar. Bunlar özellikle boştaki veya pahalı VMleri azaltmaya yardımcı olmayı amaçlayan, uygun maliyetli boyutlandırma önerileri için yararlıdır.

Kuruluşları, kaynakları buluta ilk kez taşırken etkileyen yaygın bir sorun, sanallaştırma stratejisidir. Kuruluşlar genellikle şirket içi sanallaştırma ortamı için sanal makine oluştururken kullandıklarına benzer bir yaklaşım kullanırlar. Ayrıca, şirket içi VM’leri buluta olduğu gibi taşıyarak maliyetlerin azaltıldığını varsayarlar. Ancak, bu yaklaşımın maliyetleri azaltma olasılığı düşüktür.

Sorun, mevcut altyapı için zaten ödeme yapılmış olmasıdır. Kullanıcılar isterse, büyük VM’ler oluşturup önemli sonuçlar yaşamadan boşta olsun veya olmasın, çalışır durumda tutabilir. Büyük veya boşta VM'leri buluta taşımak büyük olasılıkla maliyetleri *artırır*. Bulut hizmeti sağlayıcıları ile sözleşmeler yaptığınızda, kaynaklar için maliyet tahsisi önemlidir. Kaynağı tam olarak kullanıp kullanmadığınıza bakılmaksızın, taahhüt ettiğiniz ödemeyi yapmanız gerekir.

Uygun Maliyetli Boyutlandırma Önerileri raporu, VM örnek türü kapasitesini geçmiş CPU ve bellek kullanım verileri ile karşılaştırarak olası yıllık tasarrufları belirler.  

Portalın üst kısmındaki menüde **İyileştirici** > **Boyutlandırma İyileştirmesi** > **Uygun Maliyetli Boyutlandırma Önerileri**’ne tıklayın. Gerekirse bir filtre uygulayarak sonuç sayısını azaltabilirsiniz. Örnek bir görüntü aşağıda verilmiştir.

![Azure Vm'leri için öneri rapor boyutlandırma maliyeti](./media/tutorial-review-usage/sizing01.png)

Bu örnekte, VM örnek türlerini değiştirme önerilerine uyularak 2.382 ABD doları tasarruf edilebilirdi. İlk öneri için **Ayrıntılar** altındaki artı sembolüne (+) tıklayın. Burada, ilk öneriye ilişkin ayrıntılar verilmiştir.

![Öneri ayrıntıları gösteren örnek](./media/tutorial-review-usage/sizing02.png)

**Aday Listesi**’nin yanındaki artı sembolüne tıklarak VM örnek kimliklerini görüntüleyin.

![Yeniden boyutlandırmak için VM adayları listesini gösteren örnek](./media/tutorial-review-usage/sizing03.png)

Kullanım verimsizliklerini algılama hakkında bir öğretici videosu izlemek için bkz. [Cloudyn’de VM Boyutunu İyileştirme](https://youtu.be/1xaZBNmV704).

Azure Maliyet Yönetimi, Azure hizmetleri için maliyet tasarrufu önerileri de sunar. Daha fazla bilgi için [Öğreticisi: Önerilerden maliyetlerini iyileştirme](tutorial-acm-opt-recommendations.md).

## <a name="create-alerts-for-unusual-spending"></a>Olağan dışı harcama uyarıları oluşturma

Uyarıları otomatik olarak anomalileri harcama ve fazla harcama riskleri paydaşlara bildirmek sağlar. Uyarıları destekleyen bütçe ve maliyet eşiklerine raporları kullanarak uyarılar oluşturabilirsiniz.

Bu örnekte **zaman içinde gerçek maliyet** harcamalarınızı bir Azure sanal makinesinde toplam bütçenize yaklaştığında bildirim göndermek için rapor. Bu senaryoda, 20.000 $ toplam bütçeye varsa ve maliyetleri 10.000 ABD Doları ulaştığında maliyetler bütçenize $ 9. 000 ve ek bir uyarının yarısını yaklaştığı olduğunda bir bildirim almak istiyorsanız.

1. Cloudyn portal üst kısmındaki menüden **maliyetleri** > **maliyet analizi** > **zaman içinde gerçek maliyet**.
2. **Gruplar** seçeneğini **Hizmet** olarak, **Hizmet filtreleme** seçeneğini **Azure/VM** olarak ayarlayın.
3. Üst raporun sağ seçin **eylemleri** seçip **rapor zamanla**.
4. Zamanlanan aralıklarla kendinize raporu e-posta göndermek için seçin **zamanlama** sekmesinde **kaydedin veya bu zamanlamayı** rapor iletişim. **E-posta ile gönder**’i seçtiğinizden emin olun. Tüm etiketler, gruplandırma ve filtreleme, kullanım, e-postayla rapora dahil edilir.
5. Seçin **eşiği** sekmesini seçip **gerçek maliyet vs. Eşik** öğesini seçin.
   1. İçinde **kırmızı uyarıya** 10000 eşiği kutusuna girin.
   2. İçinde **sarı uyarı** 9000 eşiği kutusuna girin.
   3. İçinde **ardışık olarak verilecek uyarı sayısını** kutusunda, almak için ardışık olarak verilecek uyarı sayısını girin. Belirttiğiniz toplam uyarı sayısını aldığınızda, başka bir uyarı gönderilmez.
6. **Kaydet**’i seçin.

![Kırmızı ve sarı uyarı eşikleri harcamayı göre gösteren örnek](./media/tutorial-review-usage/schedule-alert01.png)

Ayrıca seçebilirsiniz **maliyet yüzdesi ve. Bütçe** eşiği ölçümünü kullanarak uyarı oluşturmayı. Bu, bütçenizi para birimi değerleri yerine yüzde olarak eşikleri belirlemenizi sağlar.

## <a name="export-data"></a>Verileri dışarı aktar

Raporlar için uyarı oluşturduğunuz gibi raporlardaki verileri de dışarı aktarabilirsiniz. Örneğin Cloudyn hesaplarının veya diğer kullanıcı verilerinin bir listesini dışarı aktarmak isteyebilirsiniz. Dışarı aktarmak için bir raporu açtıktan sonra sağ üst kısmından **Eylemler**'e tıklayın. Gerçekleştirmek isteyebileceğiniz eylemlerden biri, bilgileri indirmek veya yazdırmak için **Tüm rapor verilerini dışarı aktar** eylemidir. Dilerseniz **Rapor zamanla**'yı seçerek raporu e-postayla gönderilecek şekilde zamanlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Kullanım ve maliyet eğilimlerini izleme
> * Kullanım verimsizliklerini algılama
> * Olağan dışı harcama veya aşırı harcama uyarıları oluşturma
> * Verileri dışarı aktarma


Geçmiş verileri kullanarak harcamaları tahmin etme hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Gelecek harcamaları tahmin etme](tutorial-forecast-spending.md)
