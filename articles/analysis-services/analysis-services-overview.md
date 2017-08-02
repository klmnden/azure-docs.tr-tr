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
ms.date: 07/03/2017
ms.author: owend
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: 625baa2df8b137779ed846c584a138cc15e89a3f
ms.contentlocale: tr-tr
ms.lasthandoff: 07/26/2017

---
# <a name="what-is-azure-analysis-services"></a>Azure Analysis Services nedir?
![Azure Analysis Services](./media/analysis-services-overview/aas-overview-aas-icon.png)

Azure Analysis Services bulutta kurumsal düzeyde veri modelleme sağlar. Bu tümüyle yönetilen bir hizmet olarak platformdur (PaaS) ve Azure veri platformu hizmetleriyle sıkı bir şekilde tümleştirilmiştir. Büyük ölçüde iyileştirilmiş OLAP veri analizi altyapısıyla, Analysis Services büyük, karmaşık ve çoğunlukla birbirinden ayrı veri kaynakları arasında zengin bir semantik modeli katmanı sağlar. Verileri bir araya getirerek semantik veri modelini tanımlayabilir, modern istemci araçlarındaki zengin, etkileşimli analiz deneyimlerini destekleyen üst düzeyde özelleştirilmiş ve yüksek performanslı bir analiz oluşturabilirsiniz.

![Veri kaynakları](./media/analysis-services-overview/aas-overview-data-sources.png)


Azure Analysis Services’ın Microsoft'un genel BT özellikleriyle nasıl uyumlu olduğunu ve veri modellerinizi buluta taşıyarak nasıl bir avantaj sağlayabileceğinizi öğrenmek için [bu videoyu](https://sec.ch9.ms/ch9/d6dd/a1cda46b-ef03-4cea-8f11-68da23c5d6dd/AzureASoverview_high.mp4) izleyin.

<!--
>[!VIDEO https://channel9.msdn.com/series/Azure-Analysis-Services/Azure-Analysis-Services-overview/player]
>
>
-->

## <a name="built-on-sql-server-analysis-services"></a>SQL Server Analysis Services’ı temel alır
Azure Analysis Services, zaten bildiğiniz SQL Server Analysis Services Enterprise Edition ile uyumludur. Azure Analysis Services, 1200 ve 1400 uyumluluk düzeylerinde tablosal modelleri destekler. Bölümler, satır düzeyinde güvenlik, çift yönlü ilişkiler ve çeviriler gibi özelliklerin tümü desteklenir. Bellek içi ve DirectQuery modları, çok büyük ve karmaşık veri kümeleri üzerinde ışık hızında sorgular anlamına gelir.

Tablosal modeller hızlı geliştirme sunar ve bunlar üst düzeyde özelleştirilebilir. Geliştiriciler için, tablosal modeller model nesnelerini açıklamaya yönelik Tablosal Nesne Modeli'ni (TOM) de içerir. TOM, [Tablosal Model Betik Dili (TMSL)](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference) aracılığıyla JSON’da ve [Microsoft.AnalysisServices.Tabular](https://msdn.microsoft.com/library/microsoft.analysisservices.tabular.aspx) ad alanı aracılığıyla AMO veri tanımlama dilinde kullanıma sunulur.

Tablosal 1400 modellerindeki yeni özellikler Ayrıntı Satırları, Nesne düzeyinde güvenlik, düzensiz hiyerarşiler ve veri bağlantısı için SSDT'de modern bir Verileri Al deneyiminin yanı sıra, diğer birçok geliştirmeyi de destekler. Ayrıca temel alınan model meta verileri aynı olduğundan, mevcut şirket içi tablosal model çözümleri kolayca buluta geçirilebilir.


## <a name="better-with-azure"></a>Azure'la daha iyi
Azure Analysis Services, birçok Azure veri hizmetiyle tümleştirildiğinden gelişmiş analiz çözümleri oluşturmanıza olanak tanır.

Azure Analysis Services; Azure SQL Veritabanı, Azure SQL Veri Ambarı ve Azure Blob depolamasındaki verileri kullanabilir. Azure'da, merkezde SQL veri ambarının ve bunun çevresinde farklı iş gruplarını veya konu alanlarını hedefleyen birden çok BI modelinin bulunduğu bir hub ve bağlı bileşen modeli kullanarak kurumsal veri ambarı çözümleri oluşturabilirsiniz.

Azure Data Factory ile verilerin hareketini ve dönüşümünü düzenleyebilirsiniz. Bu, tüm kurumsal BI/analiz çözümlerinin temel özelliğidir. Azure Analysis Services, verileri modele yükleyen bir etkinlik ekleyerek herhangi bir Azure Data Factory işlem hattıyla tümleştirilebilir. Özel kod kullanarak modellerin basit düzenlemelerini yapmak için Azure Otomasyonu ve Azure İşlevleri de kullanılabilir.

Ayrıca Azure Analysis Services, Azure Active Directory ile sıkı bir tümleşiklik içinde çalışarak kritik verilerinize güvenli, rol tabanlı erişim sağlar.

## <a name="pricing"></a>Fiyatlandırma
Azure Analysis Services; Geliştirici, Temel ve Standart katmanlarında sunulur. Her katmanda, plan maliyetleri işlem gücüne, QPU'lara ve bellek boyutuna göre değişir. Sunucu oluşturduğunuzda, katman içinde bir plan seçersiniz. Aynı katman içinde planları yukarı veya aşağı doğru değişiklik yapabilir veya daha üst bir katmana yükseltebilirsiniz, ama üst katmandan daha alt bir katmana inemezsiniz.

![Katmanı yükseltme](./media/analysis-services-overview/aas-overview-tier-up.png)

Azure portalında veya Set-AzureRmAnalysisServicesServer PowerShell cmdlet'iyle katmanları kısa süre içinde değiştirin. Farklı planlar ve katmanlar hakkında daha fazla bilgi edinmek ve fiyatlandırma hesaplayıcısını kullanarak kendinize uygun planı saptamak için, bkz. [Azure Analysis Services Fiyatlandırması](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="scale-resources"></a>Kaynakları ölçeklendirme
Ölçeği artırın, azaltın veya sunucunuzu duraklatın. Azure portalını kullanın ve PowerShell'i kullanarak kolayca tam denetim sahibi olun. Sadece kullandığınız kadar ödersiniz.

Yeni sunucular oluştururken, planınızı ayarlamak için [New-AzureRmAnalysisServicesServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) cmdlet'ini kullanın. Mevcut sunucularda, planınızı değiştirmek için [Set-AzureRmAnalysisServicesServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver) cmdlet'ini kullanın. Hizmetinizi sürekli kullanmıyor musunuz? Portalı kullanarak veya [Suspend-AzureRmAnalysisServicesServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.analysisservices/suspend-azurermanalysisservicesserver) cmdlet'iyle duraklatabilirsiniz. [Resume-AzureRmAnalysisServicesServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.analysisservices/resume-azurermanalysisservicesserver) cmdlet'iyle yeniden başlatın. Yalnızca sunucunuz etkin olduğunda ödeme yaparsınız.


## <a name="regions"></a>Bölgeler
Azure Analysis Services sunucuları aşağıdaki [Azure bölgelerinde](https://azure.microsoft.com/regions/) oluşturulabilir:

| Kuzey ve Güney Amerika | Avrupa | Asya Pasifik |
|----------|--------|--------------|
|  Güney Brezilya<br> Orta Kanada<br> Doğu ABD 2<br> Orta Kuzey ABD<br> Orta Güney ABD<br> Batı Orta ABD<br> Batı ABD | Kuzey Avrupa<br> Birleşik Krallık Güney<br> Batı Avrupa |   Avustralya Güneybatı<br> Japonya Doğu<br> Güneydoğu Asya<br> Batı Hindistan  |

Sürekli yeni bölgeler eklendiğinden bu liste eksik olabilir. Azure portalında sunucunuzu oluştururken veya Azure Resource Manager şablonlarını kullanarak konum seçersiniz. En iyi performansı elde etmek için, en büyük kullanıcı tabanınıza en yakın konumu seçin. Modellerinizi birden çok bölgedeki yedekli sunuculara dağıtarak [yüksek kullanılabilirliği](analysis-services-bcdr.md) güvence altına alın.

## <a name="get-up-and-running-quickly"></a>Hızla çalışmaya başlayın
Azure portalıyla, birkaç dakikada [sunucu oluşturabilirsiniz](analysis-services-create-server.md). Ayrıca, Azure Resource Manager şablonları ve PowerShell'le, bildirim temelli bir şablon kullanarak sunucuları hazırlayabilirsiniz. Basit bir şablonla, birden çok hizmeti ve bunların yanında depolama hesapları gibi diğer Azure bileşenlerini dağıtabilirsiniz.  Daha fazla bilgi edinmek için bkz. [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../azure-resource-manager/resource-group-template-deploy.md).

Sunucu oluşturduğunuzda, doğrudan Azure portalında bir tablosal model oluşturabilirsiniz. Yeni (önizleme) Web tasarımcısı özelliğiyle, Azure SQL Veritabanı'na, Azure SQL Veri Ambarı veri kaynağına bağlanabilir veya Power BI Masaüstü .pbix dosyasını içeri aktarabilirsiniz. Tablolar arasındaki ilişkiler otomatik olarak oluşturulur ve doğrudan tarayıcınızdan ölçümler oluşturabilir veya model.bim dosyasını json biçiminde düzenleyebilirsiniz.

## <a name="migrate-existing-tabular-models"></a>Mevcut tablosal modelleri geçirme
Zaten şirket içi SQL Server Analysis Services model çözümleriniz varsa, Azure Analysis Services'i önemli değişiklikler olmadan geçirebilirsiniz. Geçirmek için, SSDT kullanarak modelinizi sunucunuza dağıtabilirsiniz. Öte yandan, SSMS'de yedekleme ve geri yüklemeyi veya TMSL'yi de kullanabilirsiniz.

Şirket içi veri kaynaklarınız varsa, [Şirket içi veri ağ geçidini](analysis-services-gateway.md) yükleyip yapılandırmanız gerekir. Önceden yapılandırılmış rolleriniz ve rol üyeleriniz varsa, rolleriniz geçirilir ancak rol üyelerini SSMS veya PowerShell kullanarak yeniden eklemeniz gerekir.


## <a name="data-sources"></a>Veri kaynakları
Azure Analysis Services, kuruluşunuzdaki şirket içi veri kaynaklarına ve buluttaki veri kaynaklarına bağlanmayı destekler. Şirket için ve bulut veri kaynaklarından toplanan verileri birleştirerek karma bir çözüm oluşturun. 

Yeni tablosal 1400 modelleri, M formül sorgu dilini temel alan SSDT'deki modern Veri Al özelliğini kullanır. Veri Al özelliğiyle, daha fazla veri dönüştürme ve karma özelliğine, ayrıca kendi gelişmiş M formül dili sorgularınızı oluşturabilme ve düzenleyebilme olanağına sahip olursunuz. Örneğin, tablosal 1400 modelleriyle Azure Blob Depolama'da veri dosyaları üzerinde modelleme yapabilirsiniz.

Azure Analysis Services; doğrudan Azure SQL Veritabanı, Azure SQL Veri Ambarı, SQL Server, SQL Server Veri Ambarı, Oracle ve Teradata ilişkisel veritabanlarına bağlanmak için [DirectQuery](https://docs.microsoft.com/sql/analysis-services/tabular-models/directquery-mode-ssas-tabular) kullanımını destekler.

Daha fazla bilgi edinmek için bkz. [Azure Analysis Services'ta desteklenen veri kaynakları](analysis-services-datasource.md).

## <a name="use-the-tools-you-already-know"></a>Bildiğiniz araçları kullanın

![BI geliştirici araçları](./media/analysis-services-overview/aas-overview-dev-tools.png)

#### <a name="sql-server-data-tools-ssdt-for-visual-studio"></a>Visual Studio için SQL Server Veri Araçları (SSDT)
Ücretsiz [Visual Studio için SQL Server Veri Araçları (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx) ile modeller geliştirin ve dağıtın. SSDT'de, hızla başlangıç yapıp ilerlemeniz için Analysis Services proje şablonları vardır. SSDT şimdi tablosal 1400 modelleri için karma işlevselliğini ve modern Veri Al veri kaynağı sorgusunu içerir. Power BI Masaüstü ve Excel 2016'daki Veri Al işlevini biliyorsanız, üst düzeyde özelleştirilmiş veri kaynağı sorguları oluşturmanın ne kadar kolay olduğunu zaten biliyor olmalısınız.

Yeni SSDT ve tablosal 1400 modelleriyle, artık çalışma alanı veritabanını barındırmak için Analysis Services'ın yerel bir örneğin yükleme gereği ortadan kalkmıştır. SSDT şimdi kendi tümleşik Analysis Services altyapısını ve veritabanını içerir. Hazır olduğunuzda, sunucularınızı Azure'da doğrudan SSDT'den dağıtın. Üstelik SSDT aylık olarak güncelleştirildiğinden, hemen en son özellikleri kullanmaya başlayabilirsiniz.

#### <a name="sql-server-management-studio"></a>Sql Server Management Studio
[SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)’yu (SSMS) kullanarak sunucularınızı ve model veritabanlarınızı yönetin. Sunucularınıza bulutta bağlayın. TMSL betiklerini doğrudan XMLA sorgu penceresinden çalıştırın ve TMSL betiklerini kullanarak görevleri otomatik hale getirin. Yeni özellikler ve işlevler hızla kullanıma sunulur çünkü SSMS her ay güncelleştirilir.

#### <a name="powershell"></a>PowerShell
Sunucuları oluşturma, sunucu işlemlerini askıya alma veya sürdürme ya da hizmet düzeyini (katman) değiştirme gibi sunucu kaynak yönetimi görevlerinde Azure Resource Manager (AzureRM) cmdlet'leri kullanılır. Rol üyeleri ekleme, işleme veya TMSL betiklerini çalıştırma gibi veritabanlarını yönetmeye yönelik diğer görevlerde, SqlServer modülündeki cmdlet'ler kullanılır. Hem AzureRM hem de SQLServer modülleri [PowerShell galerisinde](https://www.powershellgallery.com/) sağlanır.


## <a name="secure"></a>Güvenlik
![Veri görselleştirmeleri](./media/analysis-services-overview/aas-overview-secure.png)

#### <a name="authentication"></a>Kimlik Doğrulaması
Azure Analysis Services için kullanıcı kimlik doğrulaması, [Azure Active Directory (AAD)](../active-directory/active-directory-whatis.md) tarafından işlenir. Kullanıcılar bir Azure Analysis Services veritabanında oturum açmaya çalışırken, erişmeye çalıştıkları veritabanına erişimi olan bir kuruluş hesabı kimliğini kullanır. Bu kullanıcı kimlikleri, Azure Analysis Services sunucusunun bulunduğu abonelik için varsayılan Azure Active Directory’nin üyesi olmalıdır. Daha fazla bilgi edinmek için bkz. [Kimlik doğrulaması ve kullanıcı izinleri](analysis-services-manage-users.md).

#### <a name="data-security"></a>Veri güvenliği
Azure Analysis Services, Analysis Services veritabanları için depolamayı ve meta verileri kalıcı hale getirmek için Azure Blob depolamayı kullanır. Blob’daki veri dosyaları, Azure Blob Sunucu Tarafı Şifrelemesi (SSE) kullanılarak şifrelenir. Doğrudan Sorgu modu kullanılırken yalnızca meta veriler depolanır. Gerçek verilere veri kaynağından sorgu zamanında erişilir.

#### <a name="on-premises-data-sources"></a>Şirket içi veri kaynakları
[Şirket içi veri ağ geçidi](analysis-services-gateway.md) oluşturulup yapılandırılarak kuruluşunuzdaki şirket içi kaynaklarınızda bulunan verilere güvenli erişim gerçekleştirilir. Ağ geçitleri hem Doğrudan Sorgulama hem de bellek içi modlar için veri erişimi sağlar. Bir Azure Analysis Services modeli şirket içi bir veri kaynağına bağlandığında, şirket içi veri kaynağı için şifrelenen kimlik bilgilerinin yanı sıra bir sorgu oluşturulur. Ağ geçidi bulut hizmeti sorguyu analiz eder ve isteği bir Azure Service Bus’a gönderir. Şirket içi ağ geçidi, bekleyen istek olup olmadığını anlamak için Azure Service Bus’ı yoklar. Daha sonra, ağ geçidi sorguyu alır, kimlik bilgilerinin şifresini çözer ve yürütme için veri kaynağına bağlanır. Daha sonra, sonuçlar veri kaynağından tekrar ağ geçidine, oradan da Azure Analysis Services veritabanına gönderilir.

Azure Analysis Services, [Microsoft Online Services Koşullarına](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) ve [Microsoft Online Services Gizlilik Sözleşmesine](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx) tabidir.
Azure Güvenliği hakkında daha fazla bilgi edinmek için bkz. [Microsoft Güven Merkezi](https://www.microsoft.com/trustcenter/Security/AzureSecurity).

## <a name="client-connections"></a>İstemci bağlantıları
![Veri görselleştirmeleri](./media/analysis-services-overview/aas-overview-clients.png)

Power BI, Excel ve diğer üçüncü taraf araçları gibi modern veri keşif ve görselleştirme araçları, son kullanıcılara modern verileriniz üzerinde üst düzeyde etkileşimli ve görsel açıdan zengin öngörüler sağlar.

İstemciler, Analysis Services sunucularına bağlanmak için MSOLAP, AMO veya ADOMD [istemci kitaplıklarını](analysis-services-data-providers.md) kullanır. Power BI Masaüstü ve Excel gibi Microsoft istemci uygulamaları bu istemci kitaplıklarının üçünü de yükler. Ama sürüme veya güncelleştirme sıklığına bağlı olarak, istemci kitaplıklarının Azure Analysis Services için gereken en son sürümlerde olmayabileceğini unutmayın. Aynı durum AsCmd, TOM, ADOMD.NET gibi özel uygulamalar ve diğer arabirimler için de geçerlidir. Bu uygulamalar için normalde, paketin parçası olarak kitaplıkların el ile yüklenmesi gerekir.


## <a name="get-help"></a>Yardım alın

#### <a name="documentation"></a>Belgeler
Azure Analysis Services’ı ayarlamak ve yönetmek basittir. Sunucu hizmetlerinizi oluşturup yönetmek için ihtiyacınız olan tüm bilgileri burada bulabilirsiniz. Sunucunuza dağıtmak üzere bir veri modeli oluşturma işlemi, şirket içi bir sunucuya dağıtmak üzere bir veri modeli oluşturma işlemiyle aynıdır. [Analysis Services](https://docs.microsoft.com/sql/analysis-services/analysis-services) sayfasında kavramsal makaleler, yordamsal makaleler, öğreticiler ve başvuru makalelerinden oluşan kapsamlı bir kitaplık sunulmaktadır.

#### <a name="videos"></a>Videolar
[Channel 9’da Azure Analysis Services](https://channel9.msdn.com/series/Azure-Analysis-Services) sayfasındaki yararlı videoları inceleyin.

#### <a name="blogs"></a>Bloglar
Her şey çok hızlı gelişiyor. Dilediğiniz zaman [Analysis Services ekip blogunu](https://blogs.msdn.microsoft.com/analysisservices/) ve [Azure blogunu](https://azure.microsoft.com/blog/) ziyaret ederek en son bilgileri öğrenebilirsiniz.

#### <a name="community"></a>Topluluk
Analysis Services’ın canlı bir kullanıcı topluluğu vardır. [Azure Analysis Services forumundaki](https://aka.ms/azureanalysisservicesforum) konuşmalara katılın.

## <a name="feedback"></a>Geri Bildirim
Bir öneriniz veya özellik isteğiniz mi var? [Azure Analysis Services Geri Bildirim](https://aka.ms/azureanalysisservicesfeedback) sayfasında görüşlerinizi belirtmeyi unutmayın.

Belgelerle ilgili önerileriniz mi var? Her makalenin altındaki Livefyre özelliğini kullanarak yorum ekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Artık Azure Analysis Services hakkında daha çok şey bildiğinize göre, çalışmaya başlayabilirsiniz demektir. Azure'da [sunucu oluşturmayı](analysis-services-create-server.md) öğrenin. Sunucunuz hazır olduğunda, tamamen işlevsel bir tablosal model oluşturmayı ve bunu sunucunuza dağıtmayı öğrenmek için [Adventure Works öğreticisinin](tutorials/aas-adventure-works-tutorial.md) adımlarını izleyin.

