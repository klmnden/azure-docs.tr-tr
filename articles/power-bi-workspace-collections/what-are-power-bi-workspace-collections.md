---
title: Power BI Çalışma Alanı Koleksiyonları nedir?
description: Power BI Embedded, Power BI raporlarına özel çözümler oluşturmanız gerekmez, web veya mobil uygulamalarınızla tümleştirmenize olanak sağlar.
services: power-bi-embedded
author: rkarlin
ROBOTS: NOINDEX
ms.assetid: 03649b72-b7d7-40ca-b077-12356d72d4f3
ms.service: power-bi-embedded
ms.topic: article
ms.workload: powerbi
ms.date: 09/20/2017
ms.author: rkarlin
ms.openlocfilehash: 7f23856363b337a361f329ed54e2152842faf26e
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64705136"
---
# <a name="what-are-power-bi-workspace-collections"></a>Power BI Çalışma Alanı Koleksiyonları nedir?

İle **Power BI çalışma alanı koleksiyonları**, Power BI raporlarını doğrudan, web veya mobil uygulamalarınızla tümleştirebilirsiniz.

![Uygulama diyagramı](media/what-are-power-bi-workspace-collections/what-is.png)

> [!IMPORTANT]
> Power BI Çalışma Alanı Koleksiyonları kullanım dışı bırakılmıştır ve Haziran 2018'e kadar veya anlaşmanızda belirtilen süre boyunca kullanılabilecektir. Uygulamanızda kesinti yaşanmaması için Power BI Embedded'a geçirmeyi planlamanız önerilir. Verilerinizi Power BI Embedded'a nasıl taşıyacağınızı öğrenmek için bkz. [Power BI Çalışma Alanı Koleksiyonları'nı Power BI Embedded'a geçirme](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/).

Power BI çalışma alanı koleksiyonları olan bir **Azure hizmeti** ISV ve uygulama geliştiricilerine uygulamalarıyla yüzey Power BI veri deneyimleri sağlar. Bir geliştirici olarak uygulamalar derlediyseniz ve söz konusu uygulamaların kendi kullanıcıları ve ayrı bir özellik kümesini sahip. Bu uygulamaları, grafikler ve artık Microsoft Power BI çalışma alanı koleksiyonları tarafından desteklenen raporlar gibi bazı yerleşik veri öğeleri için de oluşabilir. Uygulamanızı kullanmak için bir Power BI hesabı gerekmez. Uygulamanızı olduğu gibi önce oturum açın ve raporlama deneyimi ek bir lisans gerek kalmadan Power BI ile etkileşime girmeye yönelik devam edebilirsiniz.

## <a name="licensing-for-microsoft-power-bi-workspace-collections"></a>Microsoft Power BI çalışma alanı koleksiyonları için lisanslama

İçinde **Microsoft Power BI çalışma alanı koleksiyonları** için Power BI sorumluluğu son kullanıcı lisans, kullanım modeli.  Bunun yerine, **oturumları** görsellerin tüketiyor uygulama geliştiricisi tarafından satın alınır ve bu kaynaklara sahip abonelik için ücretlendirilir. 

## <a name="microsoft-power-bi-workspace-collections-conceptual-model"></a>Microsoft Power BI çalışma alanı koleksiyonları'kavramsal model

![Çalışma alanı koleksiyonları ile uygulama akışı](media/what-are-power-bi-workspace-collections/model.png)

Herhangi bir hizmeti gibi Azure, Power BI çalışma alanı koleksiyonları için kaynaklar üzerinden sağlanan [Azure Resource Manager API'leri](https://msdn.microsoft.com/library/mt712306.aspx). Bu durumda, sağlama kaynağı olan bir **Power BI çalışma alanı koleksiyonu**.

## <a name="workspace-collection"></a>Çalışma alanı koleksiyonu

A **çalışma alanı koleksiyonu** 0 veya daha fazlasını içeren üst düzey Azure kaynakları için kapsayıcıdır **çalışma alanları**.  A **çalışma** **koleksiyon** tüm standart Azure özelliklerinin yanı sıra şunları içerir:

* **Erişim anahtarları** – anahtarları güvenli bir şekilde Power BI (bir sonraki bölümde açıklanmıştır) API'lerini çağırma kullanılır.
* **Kullanıcılar** – Power BI çalışma alanı koleksiyonu Azure portalından veya Azure Resource Manager API'si yönetmek için yönetici haklarına sahip kullanıcılar Azure Active Directory (AAD).
* **Bölge** – sağlama işleminin parçası olarak bir **çalışma alanı koleksiyonu**, içinde sağlanacak bir bölge seçebilirsiniz. Daha fazla bilgi için [Azure bölgeleri](https://azure.microsoft.com/regions/).

## <a name="workspace"></a>Çalışma alanı

A **çalışma** bir Power BI'ın aşağıdakileri içeren veri kümeleri ve raporlar içeriği kapsayıcıdır. A **çalışma** ilk oluşturduğunuzda boştur. Power BI Desktop kullanarak içerik yazma ve program aracılığıyla kullanarak çalışma alanınıza PBIX dağıtacaksınız [Power BI API'yi içeri aktar](https://msdn.microsoft.com/library/mt711504.aspx). Programlama yoluyla da Veri kümenizi oluşturabilir ve ardından Power BI Desktop'ı kullanmak yerine, uygulamanızın içinde raporlar oluşturun.

## <a name="using-workspace-collections-and-workspaces"></a>Çalışma alanı koleksiyonları ve çalışma alanlarını kullanma

**Çalışma alanı koleksiyonları** ve **çalışma alanları** kullanılan ve oluşturuyor olduğunuz uygulama tasarımını hangi en iyi şekilde sığacak düzenlenmiş içeriğinin kapsayıcılardır. İçindeki içerik düzenleme birçok farklı yolu vardır. Bir çalışma alanı içinde paylaşılacak tüm içerikleri ve ardından daha sonra uygulama belirteçlerini daha fazla içerik müşterileriniz arasında alt bölümlere ayırmak için tercih edebilirsiniz. Böylece bunlar arasında bazı ayrımı tüm müşterilerinizin ayrı çalışma alanlarında koymak seçebilirsiniz. Veya kullanıcılar bölgelere göre yerine müşteriye göre düzenlemek tercih edebilirsiniz. Bu esnek tasarım içeriği düzenlemek için en iyi yolu seçmenize olanak sağlar.

## <a name="cached-datasets"></a>Önbelleğe alınmış veri kümeleri

Önbelleğe alınmış veri kümeleri kullanılabilir.  Ancak, içine yüklendi sonra önbelleğe alınan verilerin yenilenemiyor **Microsoft Power BI çalışma alanı koleksiyonları**. Önbellekteki veri kümesini veri DirectQuery kullanmak yerine Power BI Desktop'a aktarılmaz anlamına gelir.

## <a name="authentication-and-authorization-with-app-tokens"></a>Kimlik doğrulaması ve yetkilendirme ile uygulama belirteçleri

**Microsoft Power BI çalışma alanı koleksiyonları** gerçekleştirmek için gerekli kullanıcı kimlik doğrulaması ve yetkilendirme uygulamanıza erteler. Son kullanıcılarınızın Azure Active Directory (Azure AD) müşterileri olmasını açık gereksinimi yoktur.  Bunun yerine, uygulamanız için uygulamayı yoğunlaştıracaklardır **Microsoft Power BI çalışma alanı koleksiyonları** kullanarak Power BI raporu oluşturmak için yetkilendirme **uygulama kimlik doğrulama belirteçleri (uygulama belirteçlerini)**.  Bunlar **uygulama belirteçlerini** uygulamanıza bir rapor oluşturmak istediğinde gerektiğinde oluşturulur.

![Uygulama belirteç kullanımı diyagramı](media/what-are-power-bi-workspace-collections/app-tokens.png)

**Uygulama kimlik doğrulama belirteçleri (uygulama belirteçlerini)** karşı kimlik doğrulaması için kullanılan **Microsoft Power BI çalışma alanı koleksiyonları**.  Üç tür vardır **uygulama belirteçlerini**:

1. Yeni bir sağlanırken kullanılan belirteçleri - sağlama **çalışma** içine bir **çalışma alanı koleksiyonu**
2. Geliştirme belirteçleri - doğrudan çağrıları yapılırken kullanılan **Power BI REST API'leri**
3. Ekleme belirteçleri - embedded iframe içindeki bir raporu işlemek için çağrıları yapılırken kullanılan

Bu belirteçler etkileşimlerinizi çeşitli aşamaları için kullanılan **Microsoft Power BI çalışma alanı koleksiyonları**.  Belirteçler, Power BI için uygulamanızdan izinleri verebilirsiniz şekilde tasarlanmıştır. Daha fazla bilgi için [uygulama belirteci akışı](app-token-flow.md).

## <a name="create-or-edit-reports-within-your-application"></a>Uygulamanızda rapor oluşturabilmeniz veya düzenleyebilmeniz

Artık, mevcut raporları düzenlemek veya Power BI Desktop kullanmaya gerek kalmadan doğrudan uygulamanızda yeni raporlar oluşturabilirsiniz. Bu, bir veri kümesi, bir çalışma alanı içinde bulunmasını gerektirir.

## <a name="see-also"></a>Ayrıca bkz.

[Microsoft Power BI çalışma alanı koleksiyonları senaryoları](scenarios.md)  
[Microsoft Power BI çalışma alanı koleksiyonları ile çalışmaya başlama](get-started.md)  
[Bir örnek ile kullanmaya başlama](get-started-sample.md)  
[Rapor ekleme](embed-report.md)  
[Power BI Çalışma Alanı Koleksiyonları ile kimlik doğrulaması ve yetkilendirme](app-token-flow.md)  
[JavaScript Örnek Ekleme](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[Power BI-CSharp Git deposu](https://github.com/Microsoft/PowerBI-CSharp)  
[Power BI düğümlü Git deposu](https://github.com/Microsoft/PowerBI-Node)  

Başka sorunuz mu var? [Power BI Topluluğu'nu deneyin](https://community.powerbi.com/)
