---
title: Azure Analysis Services nedir | Microsoft Docs
description: "Azure’da büyük Analysis Services resmini görün."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 83d7a29c-57ae-4aa0-8327-72dd8f00247d
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 05/26/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 34726377836d00d484ca01edb098f6c7cbfa9dbf
ms.contentlocale: tr-tr
ms.lasthandoff: 06/17/2017


---
<a id="what-is-azure-analysis-services" class="xliff"></a>

# Azure Analysis Services nedir?
![Azure Analysis Services](./media/analysis-services-overview/aas-overview-aas-icon.png)

Microsoft SQL Server Analysis Services hizmetindeki kendini kanıtlamış analitik altyapıyı temel alan Azure Analysis Services, bulutta kurumsal düzeyde veri modelleme olanağı sağlar. 

Azure Analysis Services’ın Microsoft'un genel BT özellikleriyle nasıl uyumlu olduğunu ve veri modellerinizi buluta taşıyarak nasıl bir avantaj sağlayabileceğinizi öğrenmek için bu videoyu izleyin.


>[!VIDEO https://channel9.msdn.com/series/Azure-Analysis-Services/Azure-Analysis-Services-overview/player]
>
>


<a id="built-on-sql-server-analysis-services" class="xliff"></a>

## SQL Server Analysis Services’ı temel alır
Azure Analysis Services, zaten bildiğiniz SQL Server Analysis Services Enterprise Edition ile uyumludur. Azure Analysis Services, 1200 veya üzeri uyumluluk düzeylerinde tablosal modelleri destekler. DirectQuery, bölümler, satır düzeyinde güvenlik, çift yönlü ilişkiler ve çeviriler gibi özelliklerin tümü desteklenir.

<a id="use-the-tools-you-already-know" class="xliff"></a>

## Bildiğiniz araçları kullanın
![BI geliştirici araçları](./media/analysis-services-overview/aas-overview-dev-tools.png)

Azure Analysis Services için veri modelleri oluştururken, SQL Server Analysis Services için kullandığınız araçların aynılarını kullanırsınız. [SQL Server Veri Araçları](https://msdn.microsoft.com/library/mt204009.aspx)’nı (SSDT) kullanarak model yazın ve dağıtın. [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)’yu (SSMS) kullanarak sunucularınızı ve model veritabanlarınızı yönetin. Ayrıca, [Powershell](analysis-services-powershell.md) ve [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) şablonlarını kullanarak görevleri otomatikleştirin 

<a id="supports-the-latest-features" class="xliff"></a>

## En son özellikleri destekler
Azure Analysis Services, 1200 ve 1400 Önizlemesi uyumluluk düzeylerinde tablosal modelleri destekler.

**Tablolu 1200**: İlk olarak SQL Server 2016 Analysis Services’a dahil edilen 1200, tablolar, sütunlar ve ilişkiler gibi model nesnelerini tanımlamak için Tablosal Nesne Modeli’ni (TOM) kullanıma sunmuştur. Tablosal Model Nesnesi, [Tablosal Model Betik Dili (TMSL)](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference) aracılığıyla JSON’da ve [Microsoft.AnalysisServices.Tabular](https://msdn.microsoft.com/library/microsoft.analysisservices.tabular.aspx) ad alanı aracılığıyla AMO veri tanımlama dilinde kullanıma sunulur.

**Tablosal 1400 Önizlemesi**: Ayrıntı Satırları, Nesne düzeyinde güvenlik, düzensiz hiyerarşi desteği ve veri bağlantısı için modern bir Verileri Al deneyimi için destek eklemenin yanı sıra diğer birçok geliştirme sunar. En yeni özelliklerin tamamından yararlanmak için en son [SQL Server Veri Araçları (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx) sürümünü kullanmanız gerekir. Tablosal 1400 hala önizleme aşamasında olduğundan, her şey çok hızlı gelişiyor. Son haberleri öğrenmek için [blog gönderimize](https://azure.microsoft.com/blog/1400-models-in-azure-as/) bakın.

<a id="data-sources" class="xliff"></a>

## Veri kaynakları
Azure’daki sunuculara dağıtılan veri modelleri, kuruluşunuzdaki şirket içi veri kaynaklarına veya buluttaki veri kaynaklarına bağlanmayı destekler. Şirket için ve bulut veri kaynaklarından toplanan verileri birleştirerek karma bir BI çözümü oluşturun.

Sunucunuz bulutta olduğundan, bulut veri kaynaklarınıza sorunsuzca bağlanabilirsiniz. Şirket içi veri kaynaklarına bağlanırken, [Şirket içi veri ağ geçidi](analysis-services-gateway.md) sayesinde buluttaki sunucunuzla hızlı, güvenli bağlantılar kurabilirsiniz. Hangi şirket içi veri kaynaklarının desteklendiği hakkında daha fazla bilgi edinmek için bkz. [Azure Analysis Services’da desteklenen veri kaynakları](analysis-services-datasource.md).


<a id="explore-your-data-from-anywhere" class="xliff"></a>

## Verilerinizi her yerden keşfedin
Neredeyse her yerden sunucularınıza bağlanabilir ve verileri alabilirsiniz. Azure Analysis Services; Power BI Desktop, Excel, özel uygulamalar ve tarayıcı tabanlı araçlardan bağlanmayı destekler.

![Veri görselleştirmeleri](./media/analysis-services-overview/aas-overview-visualization.png)


<a id="secure" class="xliff"></a>

## Güvenlik
<a id="user-authentication" class="xliff"></a>

#### Kullanıcı kimlik doğrulaması
Azure Analysis Services için kullanıcı kimlik doğrulaması, [Azure Active Directory (AAD)](../active-directory/active-directory-whatis.md) tarafından işlenir. Kullanıcılar bir Azure Analysis Services veritabanında oturum açmaya çalışırken, erişmeye çalıştıkları veritabanına erişimi olan bir kuruluş hesabı kimliğini kullanır. Bu kullanıcı kimlikleri, Azure Analysis Services sunucusunun bulunduğu abonelik için varsayılan Azure Active Directory’nin üyesi olmalıdır. AAD ile şirket içi Active Directory arasında [dizin tümleştirmesi](https://technet.microsoft.com/library/jj573653.aspx) gerçekleştirmek, kullanıcılarınızın bir Azure Analysis Services veritabanına erişmesini sağlamanın harika bir yoludur, ancak tüm senaryolarda gerekli değildir.

Kullanıcılar, hesaplarının kullanıcı asıl adı (UPN) ve parolası ile oturum açar. Şirket içi bir Active Directory ile eşitleme gerçekleştirilirken kullanıcının UPN’si genellikle kuruluş e-posta adresidir.

Azure Analysis Services sunucu kaynağının yönetimiyle ilgili izinler, Azure aboneliğinizde kullanıcılar rollere atanarak işlenir. Varsayılan olarak, abonelik yöneticilerinin Azure’daki sunucu kaynağı için sahip izinleri vardır. Azure Resource Manager kullanılarak daha fazla kullanıcı eklenebilir.

<a id="data-security" class="xliff"></a>

#### Veri güvenliği
Azure Analysis Services, Analysis Services veritabanları için depolamayı ve meta verileri kalıcı hale getirmek için Azure Blob depolamayı kullanır. Blob’daki veri dosyaları, Azure Blob Sunucu Tarafı Şifrelemesi (SSE) kullanılarak şifrelenir. Doğrudan Sorgu modu kullanılırken yalnızca meta veriler depolanır. Gerçek verilere veri kaynağından sorgu zamanında erişilir.

<a id="on-premises-data-sources" class="xliff"></a>

#### Şirket içi veri kaynakları
[Şirket içi veri ağ geçidi](analysis-services-gateway.md) oluşturulup yapılandırılarak kuruluşunuzdaki şirket içi kaynaklarınızda bulunan verilere güvenli erişim gerçekleştirilebilir. Ağ geçitleri hem Doğrudan Sorgulama hem de bellek içi modlar için veri erişimi sağlar. Bir Azure Analysis Services modeli şirket içi bir veri kaynağına bağlandığında, şirket içi veri kaynağı için şifrelenen kimlik bilgilerinin yanı sıra bir sorgu oluşturulur. Ağ geçidi bulut hizmeti sorguyu analiz eder ve isteği bir Azure Service Bus’a gönderir. Şirket içi ağ geçidi, bekleyen istek olup olmadığını anlamak için Azure Service Bus’ı yoklar. Daha sonra, ağ geçidi sorguyu alır, kimlik bilgilerinin şifresini çözer ve yürütme için veri kaynağına bağlanır. Daha sonra, sonuçlar veri kaynağından tekrar ağ geçidine, oradan da Azure Analysis Services veritabanına gönderilir.

Azure Analysis Services, [Microsoft Online Services Koşullarına](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) ve [Microsoft Online Services Gizlilik Sözleşmesine](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx) tabidir.
Azure Güvenliği hakkında daha fazla bilgi edinmek için bkz. [Microsoft Güven Merkezi](https://www.microsoft.com/trustcenter/Security/AzureSecurity).

<a id="get-help" class="xliff"></a>

## Yardım alın

<a id="documentation" class="xliff"></a>

### Belgeler
Azure Analysis Services’ı ayarlamak ve yönetmek basittir. Bir sunucuyu oluşturup yönetmek için ihtiyacınız olan tüm bilgileri burada bulabilirsiniz. Sunucunuza dağıtmak üzere bir veri modeli oluşturma işlemi, şirket içi bir sunucuya dağıtmak üzere bir veri modeli oluşturma işlemiyle aynıdır. [Analysis Services](https://docs.microsoft.com/sql/analysis-services/analysis-services) sayfasında kavramsal makaleler, yordamsal makaleler, öğreticiler ve başvuru makalelerinden oluşan kapsamlı bir kitaplık sunulmaktadır.

<a id="videos" class="xliff"></a>

### Videolar
[Channel 9’da Azure Analysis Services](https://channel9.msdn.com/series/Azure-Analysis-Services) sayfasındaki yararlı videoları inceleyin.

<a id="blogs" class="xliff"></a>

### Bloglar
Her şey çok hızlı gelişiyor. Dilediğiniz zaman [Analysis Services ekip blogunu](https://blogs.msdn.microsoft.com/analysisservices/) ve [Azure blogunu](https://azure.microsoft.com/blog/) ziyaret ederek en son bilgileri öğrenebilirsiniz.

<a id="community" class="xliff"></a>

### Topluluk
Analysis Services’ın canlı bir kullanıcı topluluğu vardır. [Azure Analysis Services forumundaki](https://aka.ms/azureanalysisservicesforum) konuşmalara katılın.

<a id="feedback" class="xliff"></a>

## Geri Bildirim
Bir öneriniz veya özellik isteğiniz mi var? [Azure Analysis Services Geri Bildirim](https://aka.ms/azureanalysisservicesfeedback) sayfasında görüşlerinizi belirtmeyi unutmayın.

Belgelerle ilgili önerileriniz mi var? Her makalenin altındaki Livefyre özelliğini kullanarak yorum ekleyebilirsiniz.

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar
Artık Azure Analysis Services hakkında daha çok şey bildiğinize göre, çalışmaya başlayabilirsiniz demektir. Azure’da [sunucu oluşturma](analysis-services-create-server.md) ve bu sunucuya [tablosal model dağıtma](analysis-services-deploy.md) işlemlerinin nasıl yapılacağını öğrenin.

