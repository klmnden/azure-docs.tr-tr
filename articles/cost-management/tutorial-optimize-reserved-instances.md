---
title: 'Öğretici: Azure’da Cloudyn ile ayrılmış örnek maliyetlerini iyileştirme | Microsoft Docs'
description: Bu öğreticide, Azure ve Amazon Web Services (AWS) için ayrılmış örnek maliyetlerinizi nasıl iyileştireceğinizi öğreneceksiniz.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 03/18/2019
ms.topic: tutorial
ms.service: cost-management
ms.custom: seodec18
manager: benshy
ms.openlocfilehash: a9c6e99d3eff14c364d5eba3e6256382f9df6bc4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61023158"
---
<!-- Intent: As a cloud-consuming administrator, I need to ensure that my reserved instances are optimized for cost and usage
-->

# <a name="tutorial-optimize-reserved-instances"></a>Öğretici: Ayrılmış örnekleri iyileştirme

Bu öğreticide, Cloudyn'in Azure ve Amazon Web Services (AWS) için ayrılmış örnek maliyetlerinizi ve kullanımını iyileştirmenize nasıl yardımcı olabileceğini öğreneceksiniz. Bulut hizmet sağlayıcılarıyla ayrılmış örnek, sanal makinenin gelecekteki kullanımı için önceden taahhütte bulunduğunuz uzun vadeli bir taahhüttür. Ayrıca, sanal makinelerin standart Kullandığın Kadar Öde fiyatlandırma modeline göre önemli bir tasarruf sağlama potansiyeline sahiptir. Potansiyel tasarrufların gerçekleşmesi için ayrılmış örneklerinizi tam kapasiteyle kullanmanız gerekir.

Bu öğreticide, Azure 'un ve AWS Ayrılmış Örneklerin (RI) Cloudyn tarafından nasıl desteklendiği anlatılır. Ayrıca ayrılmış örnek maliyetlerini nasıl iyileştirebileceğiniz de açıklanır. Bunun için öncelikle ayırmalarınızın tümüyle kullanıldığından emin olmalısınız. Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * Azure RI maliyetlerini anlama
> * RI'lerin avantajları hakkında bilgi edinme
> * Azure RI maliyetlerini iyileştirme
> * RI maliyetlerini görüntüleme
> * Azure RI maliyet verimliliğini değerlendirme
> * AWS RI maliyetlerini iyileştirme
> * Önerilen RI'leri satın alma
> * Kullanılmamış ayırmaları değiştirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

- Azure hesabınız olmalıdır.
- Cloudyn için bir deneme kaydına veya ücretli aboneliğe sahip olmanız gerekir.
- Azure veya AWS'de RI satın almış olmalısınız.

## <a name="understand-azure-ri-costs"></a>Azure RI maliyetlerini anlama

Azure Ayrılmış VM Örnekleri satın aldığınızda, gelecekteki kullanım için önceden ödeme yaparsınız. Önceden yapılan ödeme, şu VM'lerin gelecekteki kullanımının maliyetini kapsar:

- belirli bir türde
- belirli bir bölgede
- bir veya üç yıllık bir dönem için
- satın alınan VM miktarı kadar.

Satın aldığınız Azure Ayrılmış VM Örneklerini, Azure Portal'daki [Ayırmalar](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade)'da görebilirsiniz.

_Azure Ayrılmış VM Örneği_ terimi yalnızca fiyatlandırma modeli için geçerlidir. Çalışan VM'lerinizi hiçbir şekilde değiştirmez. Bu terim Azure'a özgüdür ve daha genel anlamıyla _ayrılmış örnek_ veya _ayırma_ olarak da kullanılır. Satın aldığınız ayrılmış örnekler belirli VM'lere uygulanmaz; tüm eşleşen VM'lere uygulanır. Örneğin, satın aldığınız ayırma için seçtiğiniz bölgede çalışan bir VM türünün ayırması olabilir.

Satın alınan ayrılmış örnekler yalnızca temel donanım için geçerlidir. VM'nin yazılım lisanslarını kapsamaz. Örneğin, bir örneği ayırabilirsiniz ve bununla eşleşen ve Windows çalıştıran bir VM'niz olabilir. Ayrılmış örnek yalnızca VM'nin temel maliyetini kapsar. Bu örnekte, tüm gerekli Windows lisanslarının tam fiyatını ödersiniz. İşletim sisteminde veya VM'niz üzerinde çalıştırılan diğer yazılımlarda indirim almak için, [Azure Hibrit Avantajları](https://azure.microsoft.com/pricing/hybrid-benefit)'nı kullanmayı göz önünde bulundurabilirsiniz. Hibrit Avantajları yazılım lisanslarında, ayrılmış örneklerin temel VM'lerde sağladığına benzer türde bir indirim sağlar.

Ayrılmış örnek kullanımı maliyeti doğrudan etkilemez. Diğer bir deyişle, VM'yi %100 CPU kullanımıyla çalıştırmakla %0 CPU kullanımıyla çalıştırmanın etkisi aynıdır: VM'nin gerçek kullanımı için değil VM ayırması için ön ödeme yaparsınız.

Şimdi aşağıdaki resimde, standart isteğe bağlı VM kullanımının, ayrılmış örneklerle ilgili olarak maliyetle ilişkisini görelim:

![İsteğe bağlı maliyetler - ayrılmış örnek maliyetleri karşılaştırması](./media/tutorial-optimize-reserved-instances/azure01.png)



Kırmızı çubuklar ayrılmış örnek satın almasının maliyetini gösterir. Yalnızca tek seferlik bir ücret ödersiniz. VM kullanımı ücretsizdir. Mavi çubuklar aynı VM'nin kullandıkça ödeme veya isteğe bağlı fiyatlandırma modeliyle çalıştırılmasının birikmiş maliyetini gösterir. VM kullanımının yaklaşık yedinci ve sekizinci ayları arasında bir yerde, *başa baş noktası* vardır. Bu örnekte, sekizinci aydan başlayarak tasarruf etmeye başlarsınız.

## <a name="benefits-of-ris"></a>RI'nin avantajları

Her ayrılmış örnek satın alması, belirli bir boyutta ve konumdaki VM için geçerlidir. Örneğin, aşağıdaki resimde gösterildiği gibi D2s\_v3 Batı ABD bölgesinde çalışır:

![Azure ayrılmış örnek ayrıntıları](./media/tutorial-optimize-reserved-instances/azure02.png)

Ayrılmış örnek satın alması, VM'nin ayırma başa baş noktasına ulaşılmasına yetecek kadar saat çalıştırılması durumunda avantajlı hale gelir. VM'nin ayrılmış örneğinizin boyutu ve konumuyla eşleşmesi gerekir. Örneğin, önceki grafikte başa baş noktası yedi buçuk ay dolayındadır. Dolayısıyla, satın almanın avantajlı olması için ayırmayla eşleşen VM en az 7,5 ay \* 30 gün \* 24 saat = 5.400 saat çalıştırılmalıdır. Eşleşen VM 5.400 saatten az çalıştırılırsa, ayırma kullandıkça ödemeden daha pahalı olur.

Başa baş noktası her VM boyutu ve her konum için farklı olabilir. Bu, üzerinde anlaşmaya vardığınız VM kullandıkça ödeme fiyatına da bağlıdır. Satın almadan önce, sizin durumunuz için geçerli olan başa baş noktasını denetlemelisiniz.

Ayırma satın alırken dikkate alınacak bir diğer nokta da ayrıma örneği kapsamıdır. Kapsam, ayırmanın avantajının paylaşılabildiğini veya belirli bir abonelik için geçerli olduğunu belirler. Paylaşılan ayrılmış örnekler tüm aboneliklerinizde ilk bulunan eşleşen VM'lere rastgele uygulanır.

Paylaşılan satın alma kapsamı en esnek kapsamdır ve mümkün olduğunca bunun tercih edilmesi önerilir. Paylaşılan kapsam söz konusu olduğunda, tüm ayrılmış örneklerinizi kullanma şansınız önemli ölçüde artar. Öte yandan, aboneliğin sahibi ayrılmış örnek için ödeme yaparken, Tek Abonelik kapsamında satın almaktan başka seçeneği olmayabilir.

## <a name="optimize-azure-ri-costs"></a>Azure RI maliyetlerini iyileştirme

Cloudyn ayrılmış örnekleri ve Karma Avantajları şu yolla destekler:

- Size fiyatlandırma modelleriyle ilişkilendirilmiş maliyetleri gösterir
- RI kullanımını izler
- RI etkisini değerlendirir
- RI maliyetlerini ilkelerinize uygun olarak ayırır

Ayrılmış örnek satın almadan önce yapmanız gereken ilk işlem, RI satın almasının etkisini değerlendirmektir:

- Size maliyeti ne kadar olacak?
- Ne kadar tasarruf edeceksiniz?
- Başa baş noktası nedir?

Ayrılmış Örnek Satın Alma Etkisi raporu bu soruları yanıtlamaya yardımcı olabilir.

## <a name="assess-azure-ri-cost-effectiveness"></a>Azure RI maliyet verimliliğini değerlendirme

Cloudyn portalında **İyileştirici** > **RI Karşılaştırması**'na gidin ve **Ayrılmış Örnek Satın Alma Etkisi**'ni seçin.

Ayrılmış Örnek Satın Alma Etkisi raporunda bir VM boyutu (Örnek Türü), Konum (Bölge), ayırma dönemi, miktar ve beklenen çalıştırma zamanı seçin. Bundan sonra, satın almanızın tasarruflu olup olmadığını değerlendirebilirsiniz.

Örneğin, Doğu ABD'de DS1\_v2 türünde bir VM için ayırma satın alırsanız ve tüm yıl boyunca 7x24 çalıştırılırsa, yıllık 369,48 ABD Doları tasarruf edebilirsiniz. Başa baş noktası beşinci aya denk gelir. Aşağıdaki resme bakın:

![Azure ayrılmış örnek başa baş noktası](./media/tutorial-optimize-reserved-instances/azure03.png)

Öte yandan, yalnızca zamanın %50'sinde çalıştırılırsa, başa baş noktası 10 aya denk gelir ve yıllık tasarruf yalnızca 49,74 ABD Doları olur. Buradaki bu örnek türü için ayırma satın almak sizin için avantajlı olmayabilir. Aşağıdaki resme bakın:

![Azure Vm'leri için başa baş noktası örneği](./media/tutorial-optimize-reserved-instances/azure04.png)

## <a name="view-ri-costs"></a>RI maliyetlerini görüntüleme

Ayırma satın aldığınızda, bir kerelik ödeme yaparsınız. Cloudyn'de ödemeyi görüntülemenin iki yolu vardır:

- Gerçek Maliyet
- Amorti Edilmiş Maliyet

### <a name="actual-reserved-instance-cost"></a>Gerçek ayrılmış örnek maliyeti

Gerçek Maliyet Analizi ve Zamana Göre Analiz raporlarında, satın alındığı aydan başlayarak ayırma için ödediğiniz tam miktar gösterilir. Bunlar, belirli bir dönemdeki gerçek harcamayı görmenize yardımcı olur.

Cloudyn portalında **Maliyetler** > **Maliyet Analizi**'ne gidin ve ardından **Gerçek Maliyet Analizi**'ni veya **Zamana Göre Gerçek Maliyet**'i seçin. Sonra filtreleri ayarlayın. Örneğin, yalnızca Azure/VM hizmetini filtreleyin ve Kaynak Türü ile Fiyat Modeli'ne göre gruplandırın. Aşağıdaki resme bakın:

![Ayrılmış örnekler gerçek maliyeti örneği](./media/tutorial-optimize-reserved-instances/azure05.png)

Aşağıdaki resimde gösterildiği gibi hizmete göre filtreleyebilir (bu örnekte, **Azure/VM**) ve **Fiyat Modeli** ile **Kaynak Türü**'ne göre gruplandırabilirsiniz:

![Gerçek Maliyet raporu grupları ve fiyat model ve kaynak türüne göre gruplandırılmış filtreleri örneği](./media/tutorial-optimize-reserved-instances/azure06.png)

Ayrıca tek seferlik ücretler, kullanım ücretleri ve lisans ücretleri gibi yaptığınız ödeme türlerini de analiz edebilirsiniz.

### <a name="amortized-reserved-instance-cost"></a>Amorti edilmiş ayrılmış örnek maliyeti

RI satın aldığınızda, satın alma işleminin yapıldığı ay görünen bir ön ödeme yaparsınız. Bu ödeme izleyen faturalarınızda görünmez. Dolayısıyla, aylık kullanıma bakmak yanıltıcı olabilir. Aylık maliyetiniz aslında aylık kullanım artı daha önce yapılmış tek seferlik ücretlerin orantılı (amorti edilmiş) bölümüdür. Amorti Edilmiş Maliyet raporu gerçek resmi elde etmenize yardımcı olabilir.

Amorti edilmiş ayrılmış örnek maliyeti, tek seferlik ayırma ücretinin alınıp ayırma dönemi üzerinden amorti ettirilmesiyle hesaplanır. Gerçek Maliyet raporlarında, tek seferlik ücretler ayırmanın satın alındığı ayda görünür. Dağıtımın gerçek maliyetinde günlük ve aylık harcama gösterilmez. Amorti Edilmiş Maliyet raporlarında dağıtımın zaman içindeki gerçek maliyeti gösterilir.  Amorti edilmiş maliyet raporu, gerçek maliyet eğilimlerinizi görmenin tek yoludur. Bu, gelecekteki harcamalarınızı tahmin etmenin de tek yoludur.

Gerçek Maliyet raporunda, 16 Kasım'daki RI satın alması için 747 ABD Doları tutarında bir sıçrama görürsünüz. Amorti Edilmiş Maliyet raporunda (aşağıdaki resme bakın), 16 Kasım'da kısmi bir günlük maliyet vardır. 17 Kasım'dan başlayarak, 747 ABD Doları/365 = 2,05 ABD Doları tutarındaki amorti edilmiş RI maliyetini görürsünüz. Bu arada, satın alınan ayırmanın kullanılmamış olduğunu fark edebilir ve bu nedenle onu farklı bir VM boyutuna kaydırarak iyileştirebilirsiniz.

Bunu görüntülemek için, **Maliyetler** > **Maliyet Analizi**'ne gidin ve ardından **Amorti Edilmiş Maliyet Analizi**'ni veya **Zamana Göre Amorti Edilmiş Maliyet**'i seçin.

![Amorti edilmiş gösteren örnek rapor ayrılmış örnek maliyeti](./media/tutorial-optimize-reserved-instances/azure07.png)

## <a name="optimize-aws-ri-costs"></a>AWS RI maliyetlerini iyileştirme

Ayrılmış örnekler açık bir taahhüttür. Sürekli VM'leri kullanıyorsanız bunlar kullanışlıdır, çünkü ayrılmış örnekler isteğe bağlı örneklerden daha ucuzdur. Öte yandan bunları yeterli düzeyde kullanılması gerekir. Taahhüt, kaynakları (normal olarak VM'leri) bir veya üç yıl gibi tanımlı bir süre kullanma yönündedir. Satın alma taahhüdünde bulunduğunuzda, ayrılan kaynaklar için önceden ödeme yaparsınız. Bununla birlikte, ayırmada taahhüt ettiğiniz miktarı her zaman tümüyle kullanmayabilirsiniz.

Örneğin, ortamınızı değerlendirebilir ve geçen yıl boyunca sürekli çalışan 20 standart D2 örneğiniz olduğunu saptayabilirsiniz. Bunlar için ayırma satın alabilir ve büyük olasılıkla önemli bir para tasarrufu sağlayabilirsiniz. Farklı bir örnekte, yıl boyunca on MA4 örneği kullanmayı taahhüt etmiş olabilirsiniz. Ama bugüne kadar yalnızca beş tanesini kullanmışsınızdır. Her iki örnek de verimsiz RI kullanımını gösterir. Cloudyn iyileştirme raporlarıyla ayrılmış örnekler için maliyetleri iyileştirmenin iki yolu vardır:

- Geçmiş kullanımınız temelinde ne satın alabileceğinize ilişkin satın alma önerilerini gözden geçirme
- Kullanılmamış ayırmaları değiştirme

_EC2 RI Satın Alma Önerileri_ ve _EC2 Şu Anda Kullanılmamış Durumdaki Ayırmalar_ raporlarını, ayrılmış örnek kullanımınızı ve maliyetlerinizi geliştirmek için kullanırsınız.

## <a name="buy-recommended-ris"></a>Önerilen RI'leri satın alma

Cloudyn isteğe bağlı örnek kullanımını ve potansiyel ayrılmış örnekleri karşılaştırır. Tasarruf olasılıkları bulursa, EC2 Satın Alma Önerileri raporunda önerilerini gösterir.

Portalın üst kısmındaki raporlar menüsünde **İyileştirici** > **Fiyat İyileştirme** > **EC2 RI Satın Alma Önerileri**’ne tıklayın.

Aşağıdaki resimde, rapordaki satın alma önerileri gösterilir.

![Satın alma önerileri EC2 satın alma önerileri raporunda örnek gösteriliyor](./media/tutorial-optimize-reserved-instances/aws01.png)

Bu örnekte, Cloudyn\_A hesabının 32 ayrılmış örnek satın alma önerisi vardır. Tüm satın alma önerilerine uyarsanız, yıllık 137.770 AMD Doları tasarruf etme potansiyeliniz olur. Cloudyn tarafından sağlanan satın alma önerilerinde, çalışan iş yüklerinizde kullanımın tutarlı kalacağının varsayıldığını unutmayın.

Her satın almanın neden önerildiğini açıklayan ayrıntıları görüntülemek için, **Gerekçeler**'in altındaki artı simgesine (**+**) tıklayın. Aşağıda, listedeki ilk öneri için bir örnek verilmiştir.

![Satın alma gerekçe ayrıntıları gösteren örnek](./media/tutorial-optimize-reserved-instances/aws02.png)

Önceki örnekte, iş yükünü isteğe bağlı çalıştırmanın maliyeti yıllık 90.456 ABD Doları tutarındadır. Öte yandan, ayırmayı önceden satın alırsanız, aynı iş yükü 56.592 ABD Doları tutar ve yıllık 33.864 ABD Doları tasarruf etmiş olursunuz.

Satın alma yatırımınızın yaklaşık olarak ne zaman karşılandığını görmek amacıyla yıl içindeki başa baş noktanızı görüntülemek için **EC2 RI Satın Alma Etkisi**'nin yanındaki artı simgesine tıklayın. Aşağıdaki örnekte, yaklaşık olarak satın almayı izleyen sekizinci ayda, isteğe bağlı birikmiş maliyeti RI birikmiş maliyetini aşmaya başlıyor:

![Satın alma etkisi ayrıntıları gösteren örnek](./media/tutorial-optimize-reserved-instances/aws03.png)

Bu noktada tasarruf etmeye başlıyorsunuz.

Satın alma önerisinin doğruluğunu onaylamak için **Zamana Göre Örnekler**'i gözden geçirebilirsiniz. Bu örnekte, son 30 günlük sürede altı örneğin iş yükü için ortalama kullanıldığını görebilirsiniz.

![Zaman içinde örnekleri geçmiş kullanımını gösteren örnek](./media/tutorial-optimize-reserved-instances/aws04.png)

## <a name="modify-unused-reservations"></a>Kullanılmamış ayırmaları değiştirme

Kullanılmamış ayırmalar, birçok bulut kaynağı müşterisinin bilgi işlem ortamında yaygın bir durumdur. Ayırmaları güncel gereksinimlerinize uyacak şekilde değiştirirken kullanılmamış ayırmaların tümüyle kullanıldığından emin olursanız tasarruf sağlayabilirsiniz. Örneğin, Linux üzerinde çalışan standart D3 örnekleri içeren bir aboneliğiniz olabilir. Ayırmayı tümüyle kullanmayacaksınız, örnek türünü değiştirebilirsiniz. Öte yandan, kullanılmamış kaynakları farklı bir ayırmaya veya farklı bir hesaba taşımanız da mümkün olabilir.

AWS belirli kullanılabilirlik alanları ve bölgeleri için ayrılmış örnekler satar. Belirli bir kullanılabilirlik alanı için ayrılmış örnekler satın aldıysanız, ayırmaları alanlar arasında taşıyamazsınız. Öte yandan, bölgesel ayrılmış örnekleri **EC2 Şu Anda Kullanılmamış Durumdaki Ayırmalar** raporunu kullanarak alanlar arasında kolayca taşıyabilirsiniz. Alternatif olarak, bunlarda değişiklik yaparak bölgesel kapsama sahip olmalarını sağlayabilirsiniz; bundan sonra, tüm kullanılabilirlik alanları genelinde eşleşen örneklere uygulanırlar.

Portalın en üstündeki raporlar menüsünde **İyileştirici** > **Verimsizlikler** > **EC2 Şu Anda Kullanılmamış Durumdaki Ayırmalar**'a tıklayın.

Aşağıdaki resimlerde kullanılmamış ayrılmış örnekleri içeren rapor gösterilir.

![Örnek gösteren özetlenmiş kullanılmamış durumdaki ayırmalar hakkında bilgi](./media/tutorial-optimize-reserved-instances/unused-ri01.png)

Belirli bir ayırmanın ayrıntılarını görüntülemek için **Ayrıntılar**'ın altındaki artı simgesine tıklayın.

![Kullanılmamış ayırma ayrıntıları gösteren örnek](./media/tutorial-optimize-reserved-instances/unused-ri02.png)

Önceki örnekte, çeşitli kullanılabilirlik alanlarında toplam 77 kullanılmamış ayırma vardır. İlk ayırmanın 51 örneği kullanılmamıştır. Listede aşağıya doğru bakıldığında, **us-east-1c** kullanılabilirlik alanındaki **m3.2xlarge** örnek türünü kullanarak yapabileceğiniz potansiyel ayırma örneği değişiklikleri vardır.

Listedeki ilk ayırma için **Değiştir**'e tıklayarak, ayırma hakkındaki verilerin gösterildiği **RI'yi Değiştir** sayfasını açın.

![Rezervasyonlar, değiştirebilirsiniz gösteren örnek](./media/tutorial-optimize-reserved-instances/unused-ri03.png)

Değiştirebileceğiniz ayırma örnekleri listelenir. Aşağıdaki örnek resimde, kullanılmamış olan ve değiştirebileceğiniz 51 ayırma vardır ama iki ayırma arasında 54 tane gerekmektedir. Kullanılmamış ayırmalarınızı hepsini kullanacak şekilde değiştirirseniz, dört örnek isteğe bağlı olarak çalıştırılmaya devam edecektir. Bu örnek için, kullanılmamış ayırmalarınızı ilk ayırma 30 ve ikinci ayırma 21 tane kullanacak şekilde bölün.

İlk ayırma girdisi için artı simgesine tıklayın ve **Ayırma miktarı**'nı **30** olarak ayarlayın. İkinci girdi için, ayırma miktarını **21** olarak ayarlayın ve ardından **Uygula**'ya tıklayın.

![Ayırma miktarını değişiklikleri gösteren örnek](./media/tutorial-optimize-reserved-instances/unused-ri04.png)

Ayırma için tüm kullanılmamış örnekleriniz tümüyle kullanılır ve artık 51 örnek isteğe bağlı olarak çalıştırılmaz. Bu örnekte, isteğe bağlı kullanımı önemli ölçüde azaltarak ve zaten ödenmiş olan ayırmaları kullanarak kuruluşunuzun parasını tasarruf etmiş olursunuz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki görevleri başarıyla gerçekleştirdiniz:

> [!div class="checklist"]
> * Azure RI maliyetlerini anladınız
> * RI'lerin avantajları hakkında bilgi edindiniz
> * Azure RI maliyetlerini iyileştirdiniz
> * RI maliyetlerini görüntülediniz
> * Azure RI maliyet verimliliğini değerlendirdiniz
> * AWS RI maliyetlerini iyileştirdiniz
> * Önerilen RI'leri satın aldınız
> * Kullanılmamış ayırmaları değiştirdiniz


Verilere erişimi denetleme hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Verilere erişimi denetleme](tutorial-user-access.md)
