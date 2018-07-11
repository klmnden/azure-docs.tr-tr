---
title: 'Öğretici: Azure Maliyet Yönetimi’nde kullanım ve maliyetleri gözden geçirme | Microsoft Docs'
description: Bu öğreticide, eğilimleri takip etmek, verimsizlikleri algılamak ve uyarılar oluşturmak için kullanımı ve maliyetleri gözden geçirirsiniz.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/07/2018
ms.topic: tutorial
ms.service: cost-management
ms.custom: ''
manager: dougeby
ms.openlocfilehash: 5505ec8dd25e5468fad81d4eb26980202425969a
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2018
ms.locfileid: "35629603"
---
<!-- Intent: As a cloud-consuming user, I need to view usage and costs for my cloud resources and services.
-->

# <a name="tutorial-review-usage-and-costs"></a>Öğretici: Kullanımı ve maliyetleri gözden geçirme

Azure Maliyet Yönetimi, eğilimleri takip edebilmeniz, verimsizlikleri algılamanız ve uyarılar oluşturmanız için kullanım ve maliyet bilgilerini gösterir. Tüm kullanım ve maliyet verileri, Cloudyn panoları ve raporlarında gösterilir. Bu öğreticideki örneklerde, pano ve raporları kullanarak kullanım ve maliyetleri gözden geçirme işlemi açıklanmaktadır. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Kullanım ve maliyet eğilimlerini izleme
> * Kullanım verimsizliklerini algılama
> * Olağan dışı harcama veya aşırı harcama uyarıları oluşturma
> * Verileri dışarı aktarma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

- Azure hesabınız olmalıdır.
- Azure Maliyet Yönetimi için bir deneme kaydı veya ücretli aboneliğe sahip olmalısınız.

## <a name="open-the-cloudyn-portal"></a>Cloudyn portalını açın

Tüm kullanım ve maliyet bilgilerini Cloudyn portalında gözden geçirirsiniz. Cloudyn portalını Azure portalından açın veya https://azure.cloudyn.com sayfasına gidip oturum açın.

## <a name="track-usage-and-cost-trends"></a>Kullanım ve maliyet eğilimlerini izleme

Eğilimleri belirlemek için Zaman İçinde raporları ile kullanım maliyetler için harcanan gerçek parayı izleyebilirsiniz. Eğilimlere bakmaya başlamak için Zaman İçinde Gerçek Maliyet raporunu kullanın. Portalın sol üst kısmından **Maliyet** > **Maliyet Analizi** > **Zaman İçinde Gerçek Maliyet**’e tıklayın. Raporu ilk kez açtığınızda, rapor uygulanmış bir grup ya da filtre yoktur.

Örnek bir rapor aşağıda verilmiştir:

![örnek rapor](./media/tutorial-review-usage/actual-cost01.png)

Raporda son 30 gün içinde yapılan tüm harcamalar gösterilir. Yalnızca Azure hizmetlerine ilişkin harcamaları görüntülemek için Hizmet grubunu uygulayın ve ardından tüm Azure hizmetleri için filtreleyin. Aşağıdaki resimde filtrelenmiş hizmetler gösterilmektedir.

![filtrelenmiş hizmetler](./media/tutorial-review-usage/actual-cost02.png)

Yukarıdaki örnekte, 31.08.2017 tarihinden sonra, önceki döneme göre daha az para harcanmıştır. Bu maliyet eğilimi, yaklaşık dokuz gün boyunca çeşitli hizmetler için devam etmektedir. Sonra, ek harcamalar önceki gibi devam etmektedir. Ancak, çok fazla sayıda sütun kullanılması, belirgin bir eğilimi belirsiz hale getirebilir. Diğer görünümlerde gösterilen verileri görmek için rapor görünümünü bir satır veya alan grafiği ile değiştirebilirsiniz. Aşağıdaki resimde eğilim daha net bir şekilde gösterilmektedir.

![raporda eğilim](./media/tutorial-review-usage/actual-cost03.png)

Örnekte, Azure Depolama maliyetinin 31.08.2017’den sonra düştüğü, diğer Azure hizmetlerine yapılan harcamaların ise sabit kaldığı açıkça görülmektedir. Öyleyse, harcamadaki bu azalmanın nedeni nedir? Bu örnekte bazı çalışanlar, işten izin alarak tatile çıkmıştır ve Depolama hizmetini kullanmamıştır.

Kullanım ve maliyet eğilimlerini izleme hakkında öğretici bir video izlemek için bkz. [Azure Maliyet Yönetimi ile bulut faturalama verilerinizi zamanla karşılaştırmalı olarak çözümleme](https://youtu.be/7LsVPHglM0g).

## <a name="detect-usage-inefficiencies"></a>Kullanım verimsizliklerini algılama

İyileştirici raporlar verimliliği artırır, kullanımını en iyi duruma getirir ve bulut kaynaklarınız için harcanan paradan tasarruf etmenin yollarını tanımlar. Bunlar özellikle boştaki veya pahalı VMleri azaltmaya yardımcı olmayı amaçlayan, uygun maliyetli boyutlandırma önerileri için yararlıdır.

Kuruluşları, kaynakları buluta ilk kez taşırken etkileyen yaygın bir sorun, sanallaştırma stratejisidir. Kuruluşlar genellikle şirket içi sanallaştırma ortamı için sanal makine oluştururken kullandıklarına benzer bir yaklaşım kullanırlar. Ayrıca, şirket içi VM’leri buluta olduğu gibi taşıyarak maliyetlerin azaltıldığını varsayarlar. Ancak, bu yaklaşımın maliyetleri azaltma olasılığı düşüktür.

Sorun, mevcut altyapı için zaten ödeme yapılmış olmasıdır. Kullanıcılar isterse, büyük VM’ler oluşturup önemli sonuçlar yaşamadan boşta olsun veya olmasın, çalışır durumda tutabilir. Büyük veya boşta VM'leri buluta taşımak büyük olasılıkla maliyetleri *artırır*. Bulut hizmeti sağlayıcıları ile sözleşmeler yaptığınızda, kaynaklar için maliyet tahsisi önemlidir. Kaynağı tam olarak kullanıp kullanmadığınıza bakılmaksızın, taahhüt ettiğiniz ödemeyi yapmanız gerekir.

Uygun Maliyetli Boyutlandırma Önerileri raporu, VM örnek türü kapasitesini geçmiş CPU ve bellek kullanım verileri ile karşılaştırarak olası yıllık tasarrufları belirler.  

Portalın üst kısmındaki menüde **İyileştirici** > **Boyutlandırma İyileştirmesi** > **Uygun Maliyetli Boyutlandırma Önerileri**’ne tıklayın. Yalnızca Azure VM’lerine bakmak için sağlayıcıya Azure filtresi uygulayın. Örnek bir görüntü aşağıda verilmiştir.

![Azure VM’leri](./media/tutorial-review-usage/sizing01.png)

Bu örnekte, VM örnek türlerini değiştirme önerilerine uyularak 3.114$ tasarruf edilebilirdi. İlk öneri için **Ayrıntılar** altındaki artı sembolüne (+) tıklayın. Burada, ilk öneriye ilişkin ayrıntılar verilmiştir.

![öneri ayrıntıları](./media/tutorial-review-usage/sizing02.png)

**Aday Listesi**’nin yanındaki artı sembolüne tıklarak VM örnek kimliklerini görüntüleyin.

![Aday Listesi](./media/tutorial-review-usage/sizing03.png)

Kullanım verimsizliklerini algılama hakkında bir öğretici videosu izlemek için bkz. [Azure Maliyet Yönetimi’nde VM Boyutunu İyileştirme](https://youtu.be/1xaZBNmV704).

## <a name="create-alerts-for-unusual-spending"></a>Olağan dışı harcama uyarıları oluşturma

Anormal harcama durumları ve fazla harcama riskleri konusunda paydaşları otomatik olarak uyarabilirsiniz. Bütçe ve maliyet eşiklerine göre uyarıları destekleyen raporları kullanarak hızlı ve kolay bir şekilde uyarılar oluşturabilirsiniz.

Herhangi bir Maliyet raporunu kullanarak herhangi bir harcamaya ilişkin uyarı oluşturabilirsiniz. Bu örnekte, Azure VM harcaması toplam bütçenize yaklaştığında bildirim almak için Zaman İçinde Gerçek Maliyet raporunu kullanın. Uyarının oluşturulması için aşağıdaki adımların tümü gereklidir. Portalın üst kısmındaki menüde **Maliyet** > **Maliyet Analizi** > **Zaman İçinde Gerçek Maliyet**’e tıklayın. **Gruplar** seçeneğini **Hizmet** olarak, **Hizmet filtreleme** seçeneğini **Azure/VM** olarak ayarlayın. Raporun sağ üst kısmında **Eylemler**’e tıklayın ve ardından **Rapor zamanla**’yı seçin.

Bu raporu kaydet veya zamanla kutusunda, istediğiniz sıklığı kullanarak kendinize raporu e-posta ile göndermek için **Zamanlama** sekmesini kullanın. **E-posta ile gönder**’i seçtiğinizden emin olun. Kullandığınız tüm etiketler, gruplar ve filtreler, e-posta ile gönderilen rapora dahil edilir. **Eşik** sekmesine tıklayın ve **Gerçek Maliyet ve Eşik** öğesini seçin. 500.000$ toplam bütçeye sahipseniz ve maliyetler bu tutarın yarısına yaklaştığında bildirim almak istediyseniz, 250.000$ değerinde **Kırmızı uyarı** ve 240.000$ değerinde **Sarı uyarı** oluşturun. Girdiğiniz değerlere virgül eklemeyin. Ardından, ardışık olarak verilecek uyarı sayısını seçin. Belirttiğiniz toplam uyarı sayısını aldığınızda, başka bir uyarı gönderilmez. Zamanlanmış raporu kaydedin.

![örnek rapor](./media/tutorial-review-usage/schedule-alert01.png)

Ayrıca, Maliyet Yüzdesi ve Bütçe eşiği ölçümünü kullanarak uyarı oluşturmayı seçebilirsiniz. Bu ölçümü kullanarak, para birimi değerleri yerine bütçe yüzdelerini kullanabilirsiniz.

## <a name="export-data"></a>Verileri dışarı aktarma

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
