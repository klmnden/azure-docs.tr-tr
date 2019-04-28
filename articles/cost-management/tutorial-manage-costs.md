---
title: 'Öğretici: Azure’da Cloudyn ile maliyetleri yönetme | Microsoft Docs'
description: Bu öğreticide, maliyet dağıtımı, ücret hesaplama raporları ve ücret yansıtma raporları kullanarak maliyetleri yönetmeyi öğrenirsiniz.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 03/18/2019
ms.topic: tutorial
ms.service: cost-management
ms.custom: seodec18
manager: benshy
ms.openlocfilehash: df4994475bf10fa902a2902b69b231821601d33b
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62130037"
---
# <a name="tutorial-manage-costs-by-using-cloudyn"></a>Öğretici: Cloudyn kullanarak maliyetleri yönetme

Maliyetlerin etiketler temelinde dağıtımını yaparak Cloudyn'de maliyetleri yönetir ve ücret hesaplama raporları oluşturursunuz. Maliyet dağıtımı işlemi, maliyetleri tüketilen bulut kaynaklarınıza dağıtır. Kaynaklarınızın tümü etiketler kullanılarak kategorilere ayrıldığında maliyetler tam olarak dağıtılır. Maliyetler dağıtıldıktan sonra, panolar ve raporlarla kullanıcılarınıza ücret hesaplama ve ücret yansıtma raporları sağlayabilirsiniz. Bununla birlikte, Cloudyn'i kullanmaya başladığınızda birçok kaynak etiketsiz veya etiketlenemez durumda olabilir.

Örneğin, mühendislik maliyetleri için geri ödeme almak isteyebilirsiniz. Mühendislik ekibinize, kaynak maliyetleri temelinde belirli bir miktar almanız gerektiğini gösterebilmelisiniz. Tüm tüketilen *mühendislik* etiketli kaynaklar için onlara bir rapor gösterebilirsiniz.

Bu makalede, etiketler ve kategoriler bazen eş anlamlı kullanılmıştır. Kategorileri geniş koleksiyonlardır ve birçok şey olabilir. Bunlar iş birimlerini, maliyet merkezlerini, web hizmetlerini veya etiketlenmiş herhangi bir şeyi içerebilir. Birden çok kaynak ve kaynak grubuna aynı etiketi uygulayarak kaynakları kategorilere ayırma ve görüntülemek ve yönetmek sağlayan ad/değer çiftleri faturalandırma bilgileri birleştirilmiş etiketlerdir. Azure portalının önceki sürümlerinde *etiket adı*, *anahtar* olarak adlandırılıyordu. Etiketler tek bir Azure aboneliği için oluşturulur ve bu abonelikte depolanır. AWS etiketleri anahtar/değer çiftlerinden oluşur. Hem Azure'da hem de AWS'de kullanıldığından, Cloudyn'de de *anahtar* terimi kullanılır. Kategori Yöneticisi, etiketleri birleştirmek için anahtarları (etiket adları) kullanır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Maliyetleri dağıtmak için özel etiketler kullanma.
> * Ücret hesaplama ve ücret yansıtma raporları oluşturma.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

- Azure hesabınız olmalıdır.
- Cloudyn için bir deneme kaydına veya ücretli aboneliğe sahip olmanız gerekir.
- Cloudyn portalında [etkinleştirilmemiş hesaplar etkinleştirilmelidir](activate-subs-accounts.md).
- Sanal makinelerinizde [konuk düzeyi izleme](azure-vm-extended-metrics.md) etkinleştirilmelidir.


## <a name="use-custom-tags-to-allocate-costs"></a>Maliyetleri dağıtmak için özel etiketler kullanma

Cloudyn kaynak grubu etiket verilerini Azure'dan alır ve etiket bilgilerini otomatik olarak kaynaklara yayar. Maliyet dağıtımında, kaynak etiketlerine göre maliyeti görebilirsiniz.

Maliyetlerinizi gruplandırmak ve etiketlenmemiş maliyetleri işleyecek kuralları tanımlamak için, Maliyet Dağıtma modelini kullanarak dahili olarak kategorilere ayrılmamış (etiketlenmemiş) kaynaklara uygulanacak kategorileri (etiketleri) tanımlarsınız. Maliyet dağıtma kuralları, bir hizmetin maliyetlerinin başka bir hizmete dağıtılmasına yönelik kaydedilmiş yönergelerinizdir. Bundan sonra, kaynaklarınız oluşturduğunuz modeli seçerek etiketleri/kategorileri *maliyet dağıtma* raporlarında görüntüler.

*Maliyet analizi* raporlarında söz konusu kaynaklar için etiket bilgilerinin gösterilmediğini unutmayın. Ayrıca, maliyet ayırma kullanılarak Cloudyn'de uygulanan etiketler Azure'a gönderilmez. Dolayısıyla bunları Azure portalında görmezsiniz.

Maliyet dağıtımına başladığınızda, ilk olarak bir maliyet modeli kullanıp kapsamı tanımlarsınız. Maliyet modeli maliyetleri değiştirmez, bunları dağıtır. Maliyet modeli oluşturduğunuzda, verilerinizi maliyet varlığı, hesap veya aboneliğe göre ya da birden çok etikete göre segmentlere ayırırsınız. Yaygın kullanılan örnek etiketler arasında faturalama kodu, maliyet merkezi veya grup adı sayılabilir. Etiketler, kuruluşunuz diğer bölümlerine ücret hesaplama ve ücret yansıtma işlemleri gerçekleştirmenize de yardımcı olur.

Özel bir maliyet dağıtma modeli oluşturmak için, raporun menüsünde **Maliyetler** &gt; **Maliyet Yönetimi** &gt; **360° Maliyet Dağıtma**'yı seçin.

![360 maliyet seçtiğiniz bir panoya gösteren örnek](./media/tutorial-manage-costs/cost-allocation-360.png)

**360 Maliyet Dağıtma** sayfasında **Ekle**'yi seçin ve ardından maliyet modeliniz için bir ad ve açıklama girin. Tüm hesapları veya tek tek hesapları seçin. Tek tek hesapları kullanmak istiyorsanız, birden çok bulut hizmeti sağlayıcısından birden çok hesap seçebilirsiniz. Sonra, bulunan ve verilerinizi kategorilere ayıracak etiketleri seçmek için **Kategorilere Ayırma**'ya tıklayın. Modelinize eklemek istediğiniz etiketleri (kategorileri) seçin. Aşağıdaki örnekte **Unit** (Birim) etiketi seçilmiştir.

![Örnek gösteren maliyet modeli kategorilere ayırma](./media/tutorial-manage-costs/cost-model01.png)

Örnekte, 19.680 ABD dolarının kategorilere ayrılmadığı (etiket içermediği) gösteriliyor.

Ardından, **Kategorilere Ayrılmamış Kaynaklar**'ı seçin ve dağıtılmamış maliyetleri bulunan hizmetleri belirtin. Sonra, maliyetleri dağıtmaya yönelik kuralları tanımlayın.

Örneğin, Azure depolama maliyetlerinizi alıp Azure sanal makineleri (VM) arasında eşit olarak dağıtmak isteyebilirsiniz. Bunu yapmak için, **Azure/Depolama** hizmetini seçin, **Kategorilere Göre Orantılı**'yı ve sonra da **Azure/VM**'yi seçin. Ardından **Oluştur**’u seçin.

![Eşit dağıtım için maliyet modeli dağıtma örneği](./media/tutorial-manage-costs/cost-model02.png)



Farklı bir örnekte, tüm Azure ağ maliyetlerinizi kuruluşunuzdaki belirli bir iş birimine dağıtmak isteyebilirsiniz. Bunu yapmak için, **Azure/Ağ** hizmetini seçin ve sonra da **Dağıtma Kuralını Tanımla**'nın altında **Açık Dağıtım**'ı seçin. Ardından, dağıtım yüzdesini 100 olarak ayarlayın ve iş birimini seçin (aşağıdaki resimde **G&amp;A**):

![Belirli bir iş birimine yönelik maliyet modeli dağıtma kuralı örneği](./media/tutorial-manage-costs/cost-model03.png)



Kalan tüm kategorilere ayrılmamış kaynaklar için ek dağıtma kuralları oluşturun.

Dağıtılmamış Amazon Web Services (AWS) ayrılmış örnekleriniz varsa, bunları **Ayrılmış Örnekler** etiketli kategorilere dağıtabilirsiniz.

Kaynakları dağıtmak için yaptığınız seçimler hakkındaki bilgileri görüntülemek için **Özet**'i seçin. Bilgilerinizi kaydetmek ve ek kurallar üzerinde çalışmaya daha sonra devam etmek için **Taslak Olarak Kaydet**'i seçin. İsterseniz, bilgilerinizi kaydetmek ve Cloudyn'in maliyet dağıtma modelinizi işlemeye başlamasını sağlamak için **Kaydet ve Etkinleştir**'i de seçebilirsiniz.

Maliyet modelleri listesinde yeni maliyet modeli **İşleme durumu** ile gösterilir. Cloudyn veritabanının maliyet modelinizi içerecek şekilde güncelleştirilmesi biraz zaman alabilir. İşleme tamamlandığında, durum da **Tamamlandı** olarak güncelleştirilir. Bundan sonra maliyet modelinizdeki verileri **Genişletilmiş Filtreler** &gt; **Maliyet Modeli** altında Maliyet Analizi raporunda görüntüleyebilirsiniz.

### <a name="category-manager"></a>Kategori Yöneticisi

Kategori Yöneticisi, birden çok kategorinin (etiketin ) değerlerini birleştirip yenileri oluşturmanıza yardımcı olan bir veri temizleme aracıdır. Bu, bir kategori seçip mevcut değerleri birleştirecek kuralları oluşturduğunuz, kural tabanlı basit bir araçtır. Örneğin, her ikisi de geliştirme grubunu temsil eden **R&amp;D** ve **dev** için mevcut kategorileriniz olabilir.

Cloudyn portalında, sağ üst kısımdaki dişli simgesine tıklayın ve **Kategori Yöneticisi**'ni seçin. Yeni bir kategori oluşturmak için artı simgesini (**+**) seçin. Kategoriye bir ad girin ve ardından **Anahtarlar**'ın altında, yeni kategoriye eklemek istediğiniz kategori anahtarlarını girin.

Kuralı tanımlarken, VEYA koşuluyla birden çok değer ekleyebilirsiniz. Ayrıca bazı temel dize işlemleri de yapabilirsiniz. Her iki durumda da, **Kural**'ın sağ tarafındaki üç nokta simgesine (**…**) tıklayın.

Yeni kural oluşturmak için, **Kurallar** alanında yeni kuralı oluşturun. Örneğin, **Kurallar**'ın altına **dev** girin ve ardından **Eylemler**'in altına **R&amp;D** girin. İşiniz bittiğinde yeni kategorinizi kaydedin.

Aşağıdaki resimde, **Work-Load** adlı yeni kategori için oluşturulan örnek kurallar gösterilir:

![Yeni iş yükü kategori gösteren örnek](./media/tutorial-manage-costs/category01.png)

### <a name="tag-sources-and-reports"></a>Etiket kaynakları ve raporlar

Cloudyn raporlarında gördüğünüz etiket verileri üç konumdan kaynaklanır:

- Bulut sağlayıcısı kaynak API'leri
- Bulut sağlayıcısı faturalama API'leri
- Aşağıdaki kaynaklardan el ile oluşturulan etiketler:
    - Cloudyn varlık etiketleri - Cloudyn varlıklarına uygulanan kullanıcı tanımlı meta veri etiketleri
    - Kategori Yöneticisi - mevcut etiketlere uygulanan kurallar temelinde yeni etiketler oluşturan bir veri temizleme aracı

Cloudyn maliyet raporlarında bulut sağlayıcısı etiketlerini görüntülemek için, 360 Maliyet Dağıtma kullanarak özel bir maliyet dağıtma modeli oluşturmalısınız. Bunu yapmak için, **Maliyetler** > **Maliyet Yönetimi** > **360 Maliyet Dağıtma** öğesine gidin, istenen etiketleri seçin ve ardından etiketlenmemiş maliyetleri işlemek üzere kurallar tanımlayın. Sonra da yeni maliyet modelini oluşturun. Bunun ardından, Maliyet Dağıtma Analizi'nde raporlarınızı görüntüleyerek Azure kaynak etiketleriniz üzerinde görüntüleme, filtreleme ve sıralama işlemleri yapabilirsiniz.

Azure kaynak etiketleri yalnızca **Maliyetler** > **Maliyet Dağıtma Analizi** raporlarında gösterilir.

Bulut sağlayıcısı faturalandırma etiketleri tüm maliyet raporlarında gösterilir.

Cloudyn varlık etiketleri ve el ile oluşturduğunuz etiketler tüm maliyet raporlarında gösterilir.


## <a name="create-showback-and-chargeback-reports"></a>Ücret hesaplama ve ücret yansıtma raporları oluşturma

Kuruluşların ücret hesaplama ve ücret yansıtma işlemlerini yapmak için kullandıkları yöntemler büyük farklılık gösterir. Bununla birlikte, Cloudyn portalındaki panolar ve raporlardan herhangi birini her iki amaçla da kullanabilirsiniz. Kuruluşunuzdaki herkese kullanıcı erişimi verebilirsiniz; böylelikle, istedikleri zaman panoları ve raporları görüntüleyebilirler. Tüm Maliyet Analizi raporları ücret hesaplamayı destekler, çünkü kullanıcılara tükettikleri kaynakları gösterir. Ayrıca, kuruluşunuzdaki kullanıcıların kendi gruplarına özgü maliyet ve kullanım verilerinde detaya gitmelerine de olanak tanır.

Maliyet dağıtma sonuçlarını görüntülemek için, Maliyet Analizi raporunu açın ve oluşturduğunuz maliyet modelini seçin. Ardından, maliyet modelinde seçilen bir veya birden çok etiketle bir gruplandırma ekleyin.

![Maliyet gösteren bir örnek veri yeni maliyet analizi raporu](./media/tutorial-manage-costs/cost-analysis.png)

Kolaylıkla belirli gruplar tarafından tüketilen belirli hizmetlere odaklanmış raporlar oluşturabilir ve kaydedebilirsiniz. Örneğin, Azure sanal makinelerini yaygın olarak kullanan bir bölümünüz olabilir. Tüketimi ve maliyetleri göstermek için Azure sanal makinelerine göre filtrelenmiş bir rapor oluşturabilirsiniz.

Diğer takımlara anlık veriler sağlamanız gerekiyorsa, herhangi bir raporu PDF veya CSV biçiminde dışarı aktarabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Maliyetleri dağıtmak için özel etiketler kullanma.
> * Ücret hesaplama ve ücret yansıtma raporları oluşturma.



Verilere erişimi denetleme hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Verilere erişimi denetleme](tutorial-user-access.md)
