---
title: "Azure maliyeti Management kullanarak maliyetlerini yönetme | Microsoft Docs"
description: "Maliyetleri maliyet ayırma ve giderleri ve geri ödeme raporlarını kullanarak yönetin."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 11/06/2017
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 83ddc0cb4227235069b0027a24a52f4d8e818126
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="manage-costs-by-using-azure-cost-management"></a>Azure maliyeti Management kullanarak maliyetlerini yönetme

Maliyetleri yönetmek ve etiketlere göre maliyetleri ayırarak Azure maliyeti yönetim Cloudyn tarafından giderleri raporlar üretir. Maliyet ayırma işlemini maliyetleri tüketilen bulut kaynaklarınıza atar. Maliyetleri tam olarak tüm kaynaklarınızı etiketleriyle kategorilere ayrılır. Maliyetleri tahsis sonra giderleri veya geri ödeme kullanıcılarınız için panolar ve raporlar sağlayabilir. Ancak, maliyet Yönetimi'ni kullanma başlattığınızda birçok kaynağa etiketlenmemiş veya untaggable olabilir.

Örneğin, mühendislik maliyetlerini giderlerin isteyebilirsiniz. Kaynak maliyetler temelinde belirli bir miktar gerekir, mühendislik ekibi gösterebilmek için gerekir. Etiketlenmiş tüketilen kaynakları için bir rapor Göster *mühendislik*.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Maliyetleri ayırmak üzere özel etiketleri kullanın.
> * Giderleri ve geri ödeme raporları oluşturun.

## <a name="use-custom-tags-to-allocate-costs"></a>Maliyetleri ayırmak üzere özel etiketleri kullanın

Maliyet ayırma başlattığınızda, bunu ilk maliyet modelini kullanarak kapsamını tanımlamak şeydir. Maliyet modeli maliyetleri değiştirmez, bunları dağıtır. Maliyet modeli oluşturduğunuzda, verilerinizi birden çok etiket ve maliyet varlık, hesabı veya abonelik tarafından kesiminde. Genel örnek etiketler faturalama kodu, maliyet merkezi veya grup adı içeriyor olabilir. Etiketler de giderleri veya geri ödeme, kuruluşunuzun diğer bölümleri gerçekleştirmenize yardımcı olur.

Bir özel maliyet ayırma modeli oluşturmak için seçin **maliyet** &gt; **Yönetimi maliyeti** &gt; **maliyet ayırma 360 derecelik** raporun menüsünde.

![Maliyet ayırma 360 seçimi](./media/tutorial-manage-costs/cost-allocation-360.png)

Üzerinde **maliyet ayırma 360** sayfasında, **Ekle** ve bir ad ve maliyet modeliniz için açıklama girin. Tüm hesapları veya bireysel hesaplar seçin. Bireysel hesaplar kullanmak istiyorsanız, birden çok bulut hizmeti sağlayıcılarını birden fazla hesap seçebilirsiniz. Bundan sonra öğesini **kategori** maliyet verilerinizi kategorilere ayırma bulunan etiketler seçmek için. Modelinize eklemek istediğiniz etiketleri (kategori) seçin. Aşağıdaki örnekte, **birim** etiketi seçilidir.

![Maliyet örnek modeli kategorilere ayırma](./media/tutorial-manage-costs/cost-model01.png)



Aşağıdaki örnekte, $14,444 (etiketsiz) Kategorilere ayrılmamış gösterilir.

Ardından, **Kategorilere ayrılmamış kaynakları** ve maliyetleri ayrılmamış hizmetleri seçin. Ardından, maliyetleri ayırmak için kurallar tanımlar.

Örneğin, Azure storage maliyetleri alın ve maliyetleri eşit olarak Azure sanal makineleri (VM'ler) dağıtmak isteyebilirsiniz. Bunu yapmak için seçin **Azure/depolama** hizmeti, select **kategoriler orantılı**ve ardından **Azure/VM**. Ardından, seçin **oluşturma**.

![Eşit dağıtım için örnek maliyet modeli ayırma kuralı](./media/tutorial-manage-costs/cost-model02.png)



Farklı bir örnekte, belirli bir departmana, kuruluşunuzdaki tüm Azure ağ maliyetleri ayırmak isteyebilirsiniz. Bunu yapmak için seçin **Azure/ağ** hizmet ve ardından **açık dağıtım**. Sonra dağıtım yüzde 100'e ayarlayın ve iş birimi seçin —**G&amp;A** aşağıdaki görüntüde:

![Belirli bir departman için örnek maliyet modeli ayırma kuralı](./media/tutorial-manage-costs/cost-model03.png)



Tüm kalan Kategorilere ayrılmamış kaynaklar için ek ayırma kurallar oluşturun.

Hiç ayrılmamış Amazon Web Hizmetleri (AWS) ayrılmış örneği varsa, bunları ile etiketli kategoriye atayabilirsiniz **ayrılmış örnekler**.

Maliyetleri ayırmak için yaptığınız seçimler hakkındaki bilgileri görüntülemek için seçin **Özet**. Bilgilerinizi kaydetmek ve daha sonra ek kurallar üzerinde çalışmaya devam etmek için seçin **taslağı olarak Kaydet**. Veya, bilgilerinizi kaydetmek ve maliyet ayırma modelinizi işlemeye başlaması Cloudyn sağlamak için seçin **kaydedin ve etkinleştirme**.

Yeni maliyet modelinizi maliyet modelleri listesini gösterir **durumu işlenirken**. Cloudyn veritabanı maliyet modelinizi güncelleştirilmeden önce biraz zaman alabilir. Durumu güncelleştirilmesi işlem tamamlandığında **tamamlandı**. Altında maliyet analiz raporunda, maliyet modelden sonra verilerini görüntüleyebilir **genişletilmiş filtreleri** &gt; **maliyet modeli**.

### <a name="category-manager"></a>Kategori Yöneticisi

Kategori Yöneticisi Yeni kampanya oluşturmak için birden çok kategori (etiketleri) değerlerini birleştirme yardımcı olan bir veri temizleme bir araçtır. Burada bir kategori seçin ve mevcut değerleri birleştirmek için kurallar oluşturabilirsiniz bir basit kural tabanlı araçtır. Örneğin, mevcut kategorileri olabilir **R&amp;D** ve **geliştirme** geliştirme grubu temsil eden burada her ikisi de.

Dişli simgesini seçin ve sağ üst Cloudyn Portalı'nda **kategori Yöneticisi**. Yeni bir kategori oluşturmak için artı simgesi seçin (**+**). Kategori için ve ardından altında bir ad girin **anahtarları**, yeni kategoride dahil etmek istediğiniz kategori anahtarları girin.

Bir kural tanımladığınızda, bir veya koşulu birden çok değerle ekleyebilirsiniz. Ayrıca, bazı temel dize işlemleri yapabilirsiniz. Her iki durumda da üç nokta simgesini tıklatın (**...** ) sağındaki **kuralı**.

Yeni bir kural tanımlamak için **kuralları** alan, yeni bir kural oluşturun. Örneğin **geliştirme** altında **kuralları** yazıp enter **R&amp;D** altında **Eylemler**. İşiniz bittiğinde, yeni bir kategori kaydedin.

Aşağıdaki resimde adlı yeni bir kategori için oluşturulan kuralların örneği gösterilmektedir **iş yükü**:

![Örnek kategorisi](./media/tutorial-manage-costs/category01.png)



## <a name="create-showback-and-chargeback-reports"></a>Giderleri ve geri ödeme raporları oluşturma

Kuruluşlar giderleri ve geri ödeme gerçekleştirmek için kullandığınız yöntem büyük ölçüde farklılık gösterir. Ancak, panolar ve raporlar hiçbirini Cloudyn Portalı'nda temel olarak her iki amaçla kullanabilirsiniz. Bunlar panolar ve raporlar isteğe bağlı olarak görüntüleyebilmesi için kuruluşunuzdaki herkes için kullanıcı erişim sağlayabilir. Tüm maliyet analiz raporları desteklemek giderleri çünkü bunlar tükettiği kaynaklar kullanıcıları göster. Ve kuruluşunuzdaki kullanıcıların gruba özgü maliyet veya kullanım verileri incelemek kullanıcıların sağlarlar.

Maliyet ayırma sonuçlarını görüntülemek için maliyet analiz raporu açın ve oluşturduğunuz maliyet modeli seçin. Ardından, bir veya daha fazla maliyet modelinde seçili etiketleri gruplandırarak ekleyin.

![Maliyet analizi raporu](./media/tutorial-manage-costs/cost-analysis.png)

Kolayca oluşturabilir ve raporları odaklanan belirli grupları tarafından kullanılan belirli hizmetleri kaydedin. Örneğin, Azure Vm'leri yaygın kullanan bir bölüm olabilir. Kullanım ve maliyetleri göstermek için Azure Vm'lerinde filtrelenmiş bir rapor oluşturabilirsiniz.

Diğer ekipler için anlık görüntü verilerini sağlamak gerekiyorsa, herhangi bir rapordaki PDF veya CSV biçiminde dışarı aktarabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Maliyetleri ayırmak üzere özel etiketleri kullanın.
> * Giderleri ve geri ödeme raporları oluşturun.



Cloudyn ile çalışmaya başlama hakkında daha fazla bilgi edinin ve onun özelliklerini kullanarak Cloudyn belgelere ilerletmek için.

> [!div class="nextstepaction"]
> [Cloudyn belgeleri](https://support.cloudyn.com/hc/)
