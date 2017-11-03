---
title: "Power BI çalışma alanı koleksiyonu nedir?"
description: "Power BI Embedded, özel çözümler derleme gerek kalmaması Power BI raporları web veya mobil uygulamaları tümleştirmenize olanak sağlar."
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ROBOTS: NOINDEX
ms.assetid: 03649b72-b7d7-40ca-b077-12356d72d4f3
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/20/2017
ms.author: asaxton
ms.openlocfilehash: 7df172895bb926f1715370b941964e2c29ab393d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="what-are-power-bi-workspace-collections"></a>Power BI çalışma alanı koleksiyonu nedir?

İle **Power BI çalışma koleksiyonları**, Power BI raporları sağ web veya mobil uygulamaları tümleştirebilirsiniz.

![Uygulama diyagramı](media/what-are-power-bi-workspace-collections/what-is.png)

> [!IMPORTANT]
> Power BI Çalışma Alanı Koleksiyonları kullanım dışı bırakılmıştır ve Haziran 2018'e kadar veya anlaşmanızda belirtilen süre boyunca kullanılabilecektir. Uygulamanızda kesinti yaşanmaması için Power BI Embedded'a geçirmeyi planlamanız önerilir. Verilerinizi Power BI Embedded'a nasıl taşıyacağınızı öğrenmek için bkz. [Power BI Çalışma Alanı Koleksiyonları'nı Power BI Embedded'a geçirme](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/).

Power BI çalışma koleksiyonlarıdır bir **Azure hizmeti** ISV'ler ve uygulama geliştiriciler uygulamalarını içinde yüzey Power BI veri deneyimlerinde sağlar. Bir geliştirici olarak uygulamaları oluşturuncaya ve bu uygulamaları kendi kullanıcılar ve ayrı bir dizi özellik sahiptir. Bu uygulamalar ayrıca grafikleri ve şimdi göre Microsoft Power BI çalışma alanı koleksiyonu kapatılabilir raporları gibi bazı yerleşik veri öğeleri için ortaya çıkabilir. Uygulamanızın kullanmak üzere bir Power BI hesabı gerekmez. Önceden, olduğu gibi uygulamanıza oturum açma ve görüntülemek ve herhangi bir ek lisans gerek kalmadan deneyimi raporlama Power BI ile etkileşim devam edebilirsiniz.

## <a name="licensing-for-microsoft-power-bi-workspace-collections"></a>Microsoft Power BI çalışma koleksiyonlar için lisanslama

İçinde **Microsoft Power BI çalışma koleksiyonları** için Power BI sorumluluğu son kullanıcı lisans kullanım modeli.  Bunun yerine, **oturumları** görsel tüketen uygulama geliştiricisi tarafından satın ve bu kaynakları sahibi aboneliği için ücretlendirilirsiniz. 

## <a name="microsoft-power-bi-workspace-collections-conceptual-model"></a>Microsoft Power BI çalışma koleksiyonları kavramsal model

![Çalışma alanı koleksiyonlarıyla uygulama akışı](media/what-are-power-bi-workspace-collections/model.png)

Herhangi diğer hizmetinde gibi Azure kaynakları için Power BI çalışma koleksiyonlar üzerinden sağlanan [Azure Resource Manager API'leri](https://msdn.microsoft.com/library/mt712306.aspx). Bu durumda, sağlama olan kaynaktır bir **Power BI çalışma alanı koleksiyonu**.

## <a name="workspace-collection"></a>Çalışma alanı koleksiyonu

A **çalışma alanı koleksiyonu** 0 veya daha çok içeren üst düzey Azure kaynakları için kapsayıcıdır **çalışma alanları**.  A **çalışma** **koleksiyonu** tüm standart Azure özelliklerinin yanı sıra aşağıdakileri içerir:

* **Erişim tuşları** – güvenli bir şekilde Power BI (sonraki bölümde açıklanmıştır) API'ları çağrılırken kullanılan anahtarları.
* **Kullanıcıların** – Azure Active Directory (AAD) kullanıcılar Power BI çalışma alanı koleksiyonu Azure portalı üzerinden veya Azure Kaynak Yöneticisi API'si yönetmek için yönetici haklarına sahip.
* **Bölge** – sağlama bir parçası olarak bir **çalışma alanı koleksiyonu**, içinde sağlanacak bir bölge seçin. Daha fazla bilgi için bkz: [Azure bölgeleri](https://azure.microsoft.com/regions/).

## <a name="workspace"></a>Çalışma alanı

A **çalışma** , bir Power BI veri kümelerini ve raporları aşağıdakileri içeren içerik, kapsayıcısıdır. A **çalışma** ilk oluşturduğunuz sırada boştur. Power BI Desktop kullanarak içerik yazmak ve, çalışma alanı kullanarak PBIX'i program aracılığıyla dağıtacaksınız [Power BI içeri aktarma API'si](https://msdn.microsoft.com/library/mt711504.aspx). Program aracılığıyla da kümenizi oluşturun ve sonra Power BI Desktop kullanmak yerine, uygulamanızda raporlar oluşturun.

## <a name="using-workspace-collections-and-workspaces"></a>Çalışma alanı koleksiyonu ve çalışma alanlarını kullanma

**Çalışma alanı koleksiyonları** ve **çalışma alanları** kullanılan ve oluşturduğunuz uygulama tasarımını hangi şekilde en iyi sığacak düzenlenmiş içeriğinin kapsayıcılardır. Bunların içeriklerini düzenleme birçok farklı yolu olacaktır. Bir çalışma alanı tüm içeriği yerleştirin ve sonra daha sonra uygulama belirteçleri daha fazla içerik müşterilerinizin arasında ayırabilir seçebilirsiniz. Bunlar arasında bazı ayrımı olmasını sağlamak tüm müşterilerinizin ayrı çalışma alanlarında koymak seçebilirsiniz. Veya kullanıcılar bölge yerine müşteri göre düzenlemek seçebilirsiniz. Bu esnek tasarım içeriği düzenlemek için en iyi yolu seçmenize olanak sağlar.

## <a name="cached-datasets"></a>Önbelleğe alınmış veri kümeleri

Önbelleğe alınan veri kümelerini kullanılabilir.  Ancak, içine yüklendikten sonra önbelleğe alınan veriler yenilenemiyor **Microsoft Power BI çalışma koleksiyonları**. Önbelleğe alınan bir veri kümesi, veri DirectQuery kullanmak yerine Power BI Desktop'a içe anlamına gelir.

## <a name="authentication-and-authorization-with-app-tokens"></a>Kimlik doğrulama ve yetkilendirme uygulama belirteçleri ile

**Microsoft Power BI çalışma koleksiyonları** tüm gerekli kullanıcı kimlik doğrulaması ve yetkilendirme gerçekleştirmek için uygulamanıza erteler. Son kullanıcılarınızın Azure Active Directory (Azure AD) müşterileri olmasını açık gereksinimi yoktur.  Bunun yerine, uygulamanız için ifade eder **Microsoft Power BI çalışma koleksiyonları** kullanarak bir Power BI raporu oluşturmak için yetkilendirme **uygulama kimlik doğrulama belirteçleri (uygulama belirteçleri)**.  Bunlar **uygulama belirteçleri** bir rapor oluşturmak, uygulamanız istediğinde gerektiği şekilde oluşturulur.

![Uygulama belirteci kullanım diyagramı](media/what-are-power-bi-workspace-collections/app-tokens.png)

**Uygulama kimlik doğrulama belirteçleri (uygulama belirteçleri)** karşı kimlik doğrulaması için kullanılan **Microsoft Power BI çalışma koleksiyonları**.  Üç tür vardır **uygulama belirteçleri**:

1. Yeni bir sağlama sırasında kullanılan belirteçleri - sağlama **çalışma** içine bir **çalışma alanı koleksiyonu**
2. Geliştirme belirteçleri - çağrıları doğrudan yapılırken kullanılan **Power BI REST API'leri**
3. Katıştırılmış IFRAME içinde bir rapor oluşturmak için çağrıları yapılırken kullanılan belirteçleri - katıştırma

Bu belirteçler etkileşimlerinizi çeşitli aşamaları için kullanılan **Microsoft Power BI çalışma koleksiyonları**.  Belirteçler için Power BI uygulamanızdan izinleri verebilirsiniz şekilde tasarlanmıştır. Daha fazla bilgi için bkz: [uygulama belirteci akışı](app-token-flow.md).

## <a name="create-or-edit-reports-within-your-application"></a>Oluşturma veya düzenleme uygulamanızdaki raporları

Şimdi, varolan raporları düzenleyin veya Power BI Desktop kullanmaya gerek kalmadan doğrudan uygulamanızda yeni raporlar oluşturun. Bu, bir veri kümesi içinde çalışma alanınızı bulunmasını gerektirir.

## <a name="see-also"></a>Ayrıca bkz.

[Microsoft Power BI çalışma koleksiyonları senaryoları](scenarios.md)  
[Microsoft Power BI çalışma koleksiyonları ile çalışmaya başlama](get-started.md)  
[Bir örnek ile kullanmaya başlama](get-started-sample.md)  
[Rapor ekleme](embed-report.md)  
[Power BI Çalışma Alanı Koleksiyonları ile kimlik doğrulaması ve yetkilendirme](app-token-flow.md)  
[JavaScript Örnek Ekleme](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[Powerbı CSharp Git deposu](https://github.com/Microsoft/PowerBI-CSharp)  
[Powerbı düğümlü Git deposu](https://github.com/Microsoft/PowerBI-Node)  

Başka sorunuz mu var? [Power BI Topluluğu'nu deneyin](http://community.powerbi.com/)
