---
title: Portalda Azure Search hizmeti oluşturma | Microsoft Docs
description: Portalda Azure Search hizmeti sağlayın.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: quickstart
ms.date: 07/09/2018
ms.author: heidist
ms.openlocfilehash: 0c7f9807605236a8250d75623d0885730c9945a0
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37950691"
---
# <a name="create-an-azure-search-service-in-the-portal"></a>Portalda Azure Search hizmeti oluşturma

Portalda Azure Search hizmeti oluşturmayı veya sağlamayı öğrenin. 

PowerShell’i mi tercih ediyorsunuz? Azure Resource Manager [hizmet şablonunu](https://azure.microsoft.com/resources/templates/101-azure-search-create/) kullanın. Başlangıç konusunda yardım için [PowerShell ile Azure Search’ü yönetme](search-manage-powershell.md) bölümüne bakarak arka planı öğrenin.

## <a name="subscribe-free-or-paid"></a>Abone olma (ücretsiz veya ücretli)

[Ücretsiz bir Azure hesabı açın](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) ve ücretli Azure hizmetlerini denemek için ücretsiz krediler kullanın. Krediler bittikten sonra hesabı tutun ve Web Siteleri gibi ücretsiz Azure hizmetlerini kullanmaya devam edin. Açıkça ayarlarınızı değiştirip ücretlendirme istemediğiniz sürece kredi kartınız asla ücretlendirilmez.

Alternatif olarak, [MSDN abone avantajlarınızı etkinleştirin](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). MSDN aboneliği size her ay ücretli Azure hizmetleri için kullanabileceğiniz krediler verir. 

## <a name="find-azure-search"></a>Azure Search’ü bulma
1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Sol üst köşedeki artı işaretine ("+ Kaynak Oluştur") tıklayın.
3. **Web** > **Azure Search**’ü seçin.

![](./media/search-create-service-portal/find-search3.png)

## <a name="name-the-service-and-url-endpoint"></a>Hizmet ve URL uç noktasını adlandırma

Hizmet adı, API çağrılarının düzenlendiği URL uç noktasının bir kısmıdır: `https://your-service-name.search.windows.net`. **URL** alanına hizmet adınızı girin. 

Hizmet adı gereksinimleri:
   * search.windows.net ad alanı içinde benzersiz olmalıdır
   * 2 ila 60 karakter uzunluğunda olmalıdır
   * Küçük harfleri, rakamları veya kısa çizgileri ("-") kullanın
   * İlk 2 karakter olarak veya sondaki tek karakter olarak tire ("-") kullanmaktan kaçının
   * Hiçbir yerde art arda tire ("--") kullanmayın

## <a name="select-a-subscription"></a>Abonelik seçme
Birden fazla aboneliğiniz varsa, veri veya dosya depolama hizmetleri de içeren bir abonelik seçin. Azure Search, [*dizin oluşturucular*](search-indexer-overview.md) aracılığıyla dizin oluşturmak için Azure Tablosu ve Blob depolama, SQL Veritabanı ve Azure Cosmos DB’yi otomatik olarak algılayabilir, ancak yalnızca aynı abonelikteki hizmetler için bunu yapar.

## <a name="select-a-resource-group"></a>Kaynak grubu seçme
Kaynak grubu, birlikte kullanılan Azure hizmetleri ve kaynakları koleksiyonudur. Örneğin, bir SQL veritabanının dizinini oluşturmak için Azure Search kullanıyorsanız, her iki hizmet de aynı kaynak grubunun parçası olmalıdır.

> [!TIP]
> Bir kaynak grubu silindiğinde, o kaynak grubunun içindeki hizmetler de silinir. Birden fazla hizmet kullanan prototip projeler için, tüm bunların aynı kaynak grubuna yerleştirilmesi, proje bittikten sonra temizleme işlemini kolaylaştırır. 

## <a name="select-a-hosting-location"></a>Barındırma konumu seçme 
Azure hizmeti olarak Azure Search, dünyanın dört bir yanındaki veri merkezlerinde barındırılabilir. [Fiyatların bölgeye göre değişiklik gösterebileceğini](https://azure.microsoft.com/pricing/details/search/) unutmayın.

## <a name="select-a-pricing-tier-sku"></a>Fiyatlandırma katmanı (SKU) seçme
[Azure Search şu anda birden fazla fiyatlandırma katmanında sunulmaktadır](https://azure.microsoft.com/pricing/details/search/): Ücretsiz, Temel veya Standart. Her katmanın kendi [kapasitesi ve sınırları](search-limits-quotas-capacity.md) vardır. Yönergeler için [Fiyatlandırma katmanı veya SKU seçme](search-sku-tier.md) bölümüne bakın.

Bu kılavuzda, hizmetimiz için Standart katmanı seçtik.

Hizmet oluşturulduktan sonra fiyatlandırma katmanı değiştirilemez. Daha yüksek veya daha düşük bir katmana ihtiyacınız olursa, hizmeti yeniden oluşturmanız gerekir.

## <a name="create-your-service"></a>Sitenizi oluşturma

Her oturum açtığınızda kolay erişim için hizmetinizi panoya sabitlemeyi unutmayın.

![](./media/search-create-service-portal/new-service3.png)

## <a name="scale-your-service"></a>Hizmetinizi ölçeklendirme
Bir hizmetin oluşturulması birkaç dakika (katmana bağlı olarak 15 dakika veya daha fazla) sürebilir. Hizmetiniz sağlandıktan sonra ihtiyaçlarınızı karşılayacak şekilde ölçeklendirilebilir. Azure Search hizmetiniz için Standart katmanı seçtiğinizden hizmetinizi iki boyutta ölçeklendirebilirsiniz: çoğaltmalar ve bölümler. Temel katmanı seçtiyseniz yalnızca çoğaltmalar ekleyebilirsiniz. Ücretsiz hizmeti sağladıysanız ölçek kullanılamaz.

***Bölümler***, hizmetinizin daha fazla belge depolamasına ve daha fazla belgede arama yapmasına olanak sağlar.

***Çoğaltmalar***, hizmetinizin daha yüksek arama sorgusu yükünü işlemesine olanak sağlar.

Kaynak eklemek aylık faturanız artırır. [Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/), fatura konusunda kaynak eklemenin getirdiği sonuçları anlamanıza yardımcı olabilir. Kaynakları yüke göre ayarlayabildiğinizi unutmayın. Örneğin, tam bir ilk dizin oluşturmak için kaynakları artırabilir ve ardından artımlı dizin oluşturmak için daha uygun bir düzeye indirebilirsiniz.

> [!Important]
> Bir hizmetin [salt okunur SLA için 2 çoğaltması ve okuma/yazma SLA’sı için 3 çoğaltması](https://azure.microsoft.com/support/legal/sla/search/v1_0/) olmalıdır.

1. Azure portalında arama hizmeti sayfanıza gidin.
2. Sol gezinti bölmesinde **Ayarlar** > **Ölçek** seçeneklerini belirleyin.
3. Her iki türdeki kaynakları eklemek için kaydırma çubuğunu kullanın.

![](./media/search-create-service-portal/settings-scale.png)

> [!Note] 
> Her katmanın, tek bir hizmette izin verilen toplam Arama Birimi sayısı üzerinde farklı [sınırları](search-limits-quotas-capacity.md) vardır (Çoğaltmalar * Bölümler = Toplam Arama Birimleri).

## <a name="when-to-add-a-second-service"></a>Ne zaman ikinci bir hizmet eklenir?

Müşterilerin çoğu, [doğru kaynak bakiyesini](search-sku-tier.md) sağlayan bir katmanda sağlanan yalnızca bir hizmeti kullanır. Bir hizmet, her bir dizinin diğerinden yalıtıldığı, [seçtiğiniz maksimum katman sınırlarına](search-capacity-planning.md) tabi olan birden fazla dizin barındırabilir. Azure Search’te istekler yalnızca bir dizine yönlendirilerek aynı hizmetteki diğer dizinlerden yanlışlıkla veya kasıtlı olarak veri alınması ihtimalini en aza indirir.

Müşterilerin çoğu yalnızca bir hizmet kullansa da, işletim gereksinimleri arasında aşağıdakiler yer alıyorsa hizmet yedekliliği gerekebilir:

+ Olağanüstü durum kurtarma (veri merkezi kesintisi). Azure Search, bir kesinti olması durumunda anında yük devretme işlevi sağlamaz. Öneriler ve kılavuz için bkz. [Hizmet yönetim](search-manage.md).
+ Çok kiracılı modelleme araştırması, ek hizmetlerin optimum tasarım olduğunu belirlemiştir. Daha fazla bilgi için bkz. [Çoklu kiracı tasarımı](search-modeling-multitenant-saas-applications.md).
+ Genel olarak dağıtılan uygulamalar için, uygulamanızın uluslararası trafiğinin gecikme süresini en aza indirmek amacıyla birden fazla bölgede bir Azure Search örneği gerekebilir.

> [!NOTE]
> Azure Search’te, dizin oluşturma ve sorgulama iş yüklerini ayıramazsınız; bu nedenle ayrılmış iş yükleri için hiçbir zaman birden fazla hizmet oluşturmazsınız. Bir dizin her zaman oluşturulduğu hizmette sorgulanır (bir hizmette bir dizini oluşturup başka bir hizmete kopyalayamazsınız).
>

Yüksek düzeyde kullanılabilirlik için ikinci bir hizmet gerekmez. Aynı hizmette 2 veya daha fazla çoğaltma kullandığınızda sorguların yüksek kullanılabilirliği elde edilir. Çoğaltma güncelleştirmeleri sıralıdır; başka bir deyişle bir hizmet güncelleştirmesi kullanıma sunulduğunda en az biri işlevsel olur. Çalışma süresi hakkında daha fazla bilgi için bkz. [Hizmet Düzeyi Sözleşmeleri](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

## <a name="next-steps"></a>Sonraki adımlar
Bir Azure Search hizmeti sağlandıktan sonra, verilerinizi karşıya yükleyip arayabilmeniz için [bir dizin tanımlamaya](search-what-is-an-index.md) hazır olursunuz. 

> [!div class="nextstepaction"]
> [.NET’te Azure Search kullanma](search-howto-dotnet-sdk.md)
