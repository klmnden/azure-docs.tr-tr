---
title: "Azure Maliyet Yönetimi'ni kullanarak maliyetleri yönetme | Microsoft Docs"
description: "Maliyet dağıtımı, ücret hesaplama raporları ve ücret yansıtma raporları kullanarak maliyetleri yönetin."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 01/30/2018
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 804b50d6ba054bbb0eb60b659c98f161ea5272ee
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="manage-costs-by-using-azure-cost-management"></a>Azure Maliyet Yönetimi'ni kullanarak maliyetleri yönetme

Maliyetlerin etiketler temelinde dağıtımını yaparak Cloudyn Azure Maliyet Yönetimi'nde maliyetleri yönetir ve ücret hesaplama raporları oluşturursunuz. Maliyet dağıtımı işlemi, maliyetleri tüketilen bulut kaynaklarınıza dağıtır. Kaynaklarınızın tümü etiketler kullanılarak kategorilere ayrıldığında maliyetler tam olarak dağıtılır. Maliyetler dağıtıldıktan sonra, panolar ve raporlarla kullanıcılarınıza ücret hesaplama ve ücret yansıtma raporları sağlayabilirsiniz. Bununla birlikte, Maliyet Yönetimi'ni kullanmaya başladığınızda birçok kaynak etiketsiz veya etiketlenemez durumda olabilir.

Örneğin, mühendislik maliyetleri için geri ödeme almak isteyebilirsiniz. Mühendislik ekibinize, kaynak maliyetleri temelinde belirli bir miktar almanız gerektiğini gösterebilmelisiniz. Tüm tüketilen *mühendislik* etiketli kaynaklar için onlara bir rapor gösterebilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Maliyetleri dağıtmak için özel etiketler kullanma.
> * Ücret hesaplama ve ücret yansıtma raporları oluşturma.

## <a name="use-custom-tags-to-allocate-costs"></a>Maliyetleri dağıtmak için özel etiketler kullanma

Maliyet dağıtımına başladığınızda, ilk olarak bir maliyet modeli kullanıp kapsamı tanımlarsınız. Maliyet modeli maliyetleri değiştirmez, bunları dağıtır. Maliyet modeli oluşturduğunuzda, verilerinizi maliyet varlığı, hesap veya aboneliğe göre ya da birden çok etikete göre segmentlere ayırırsınız. Yaygın kullanılan örnek etiketler arasında faturalama kodu, maliyet merkezi veya grup adı sayılabilir. Etiketler, kuruluşunuz diğer bölümlerine ücret hesaplama ve ücret yansıtma işlemleri gerçekleştirmenize de yardımcı olur.

Özel bir maliyet dağıtma modeli oluşturmak için, raporun menüsünde **Maliyet** &gt; **Maliyet Yönetimi** &gt; **360° Maliyet Dağıtma**'yı seçin.

![360 Maliyet Dağıtma seçimi](./media/tutorial-manage-costs/cost-allocation-360.png)

**360 Maliyet Dağıtma** sayfasında **Ekle**'yi seçin ve ardından maliyet modeliniz için bir ad ve açıklama girin. Tüm hesapları veya tek tek hesapları seçin. Tek tek hesapları kullanmak istiyorsanız, birden çok bulut hizmeti sağlayıcısından birden çok hesap seçebilirsiniz. Sonra, bulunan ve verilerinizi kategorilere ayıracak etiketleri seçmek için **Kategorilere Ayırma**'ya tıklayın. Modelinize eklemek istediğiniz etiketleri (kategorileri) seçin. Aşağıdaki örnekte **Unit** (Birim) etiketi seçilmiştir.

![Maliyet modeli kategorilere ayırma örneği](./media/tutorial-manage-costs/cost-model01.png)



Örnekte, 14.444 ABD Doları'nın kategorilere ayrılmadığı (etiket içermediği) gösteriliyor.

Ardından, **Kategorilere Ayrılmamış Kaynaklar**'ı seçin ve dağıtılmamış maliyetleri bulunan hizmetleri belirtin. Sonra, maliyetleri dağıtmaya yönelik kuralları tanımlayın.

Örneğin, Azure depolama maliyetlerinizi alıp Azure sanal makineleri (VM) arasında eşit olarak dağıtmak isteyebilirsiniz. Bunu yapmak için, **Azure/Depolama** hizmetini seçin, **Kategorilere Göre Orantılı**'yı ve sonra da **Azure/VM**'yi seçin. Ardından **Oluştur**’u seçin.

![Eşit dağıtım için maliyet modeli dağıtma örneği](./media/tutorial-manage-costs/cost-model02.png)



Farklı bir örnekte, tüm Azure ağ maliyetlerinizi kuruluşunuzdaki belirli bir iş birimine dağıtmak isteyebilirsiniz. Bunu yapmak için, **Azure/Ağ** hizmetini seçin ve sonra da **Açık Dağıtım**'ı seçin. Ardından, dağıtım yüzdesini 100 olarak ayarlayın ve iş birimini seçin (aşağıdaki resimde **G&amp;A**):

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

![Örnek kategori](./media/tutorial-manage-costs/category01.png)

### <a name="tag-sources-and-reports"></a>Etiket kaynakları ve raporlar

Cloudyn raporlarında gördüğünüz etiket verileri üç konumdan kaynaklanır:

- Bulut sağlayıcısı kaynak API'leri
- Bulut sağlayıcısı faturalama API'leri
- Aşağıdaki kaynaklardan el ile oluşturulan etiketler:
    - Cloudyn varlık etiketleri - Cloudyn varlıklarına uygulanan kullanıcı tanımlı meta veri etiketleri
    - Kategori Yöneticisi - mevcut etiketlere uygulanan kurallar temelinde yeni etiketler oluşturan bir veri temizleme aracı

Cloudyn maliyet raporlarında bulut sağlayıcısı etiketlerini görüntülemek için, 360 Maliyet Dağıtma kullanarak özel bir maliyet dağıtma modeli oluşturmalısınız. Bunu yapmak için, **Maliyet** > **Maliyet Yönetimi** > **360 Maliyet Yönetme** öğesine gidin, istenen etiketleri seçin ve ardından etiketlenmemiş maliyetleri işlemek üzere kurallar tanımlayın. Sonra da yeni maliyet modelini oluşturun. Bunun ardından, Maliyet Dağıtma Analizi'nde raporlarınızı görüntüleyerek Azure kaynak etiketleriniz üzerinde görüntüleme, filtreleme ve sıralama işlemleri yapabilirsiniz.

Azure kaynak etiketleri yalnızca **Maliyet Dağıtma Analizi** raporlarında gösterilir.

Bulut sağlayıcısı faturalandırma etiketleri tüm maliyet raporlarında gösterilir.

Cloudyn varlık etiketleri ve el ile oluşturduğunuz etiketler tüm maliyet raporlarında gösterilir.


## <a name="create-showback-and-chargeback-reports"></a>Ücret hesaplama ve ücret yansıtma raporları oluşturma

Kuruluşların ücret hesaplama ve ücret yansıtma işlemlerini yapmak için kullandıkları yöntemler büyük farklılık gösterir. Bununla birlikte, Cloudyn portalındaki panolar ve raporlardan herhangi birini her iki amaçla da kullanabilirsiniz. Kuruluşunuzdaki herkese kullanıcı erişimi verebilirsiniz; böylelikle, istedikleri zaman panoları ve raporları görüntüleyebilirler. Tüm Maliyet Analizi raporları ücret hesaplamayı destekler, çünkü kullanıcılara tükettikleri kaynakları gösterir. Ayrıca, kuruluşunuzdaki kullanıcıların kendi gruplarına özgü maliyet ve kullanım verilerinde detaya gitmelerine de olanak tanır.

Maliyet dağıtma sonuçlarını görüntülemek için, Maliyet Analizi raporunu açın ve oluşturduğunuz maliyet modelini seçin. Ardından, maliyet modelinde seçilen bir veya birden çok etiketle bir gruplandırma ekleyin.

![Maliyet Analizi raporu](./media/tutorial-manage-costs/cost-analysis.png)

Kolaylıkla belirli gruplar tarafından tüketilen belirli hizmetlere odaklanmış raporlar oluşturabilir ve kaydedebilirsiniz. Örneğin, Azure sanal makinelerini yaygın olarak kullanan bir bölümünüz olabilir. Tüketimi ve maliyetleri göstermek için Azure sanal makinelerine göre filtrelenmiş bir rapor oluşturabilirsiniz.

Diğer takımlara anlık veriler sağlamanız gerekiyorsa, herhangi bir raporu PDF veya CSV biçiminde dışarı aktarabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Maliyetleri dağıtmak için özel etiketler kullanma.
> * Ücret hesaplama ve ücret yansıtma raporları oluşturma.



Cloudyn'le çalışmaya başlama ve özelliklerini kullanma hakkında daha fazla bilgi edinmek için, Cloudyn belgelerine ilerleyin.

> [!div class="nextstepaction"]
> [Cloudyn belgeleri](https://support.cloudyn.com/hc/)
