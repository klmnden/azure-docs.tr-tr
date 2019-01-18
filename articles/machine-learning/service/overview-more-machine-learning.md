---
title: Karşılaştırma makine öğrenme ürün seçenekleri
titleSuffix: Microsoft
description: Makine öğrenmesi modellerinizi derlemek, dağıtmak ve yönetmek için Microsoft’un sunduğu ürün çeşitlerini karşılaştırın. Çözümünüz için hangi ürünün seçileceğine karar verin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: garyericson
ms.author: garye
ms.date: 1/17/2019
ms.openlocfilehash: d5660d1a4d5d2a26af57e2a2411552e3f6267379
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2019
ms.locfileid: "54389138"
---
# <a name="what-are-the-machine-learning-products-at-microsoft"></a>Microsoft'ta makine öğrenimi ürünlerini nelerdir?

Makine öğrenmesi modellerinizi derlemek, dağıtmak ve yönetmek için Microsoft çeşitli ürün seçenekleri sağlar. Bu ürünleri karşılaştırın ve makine öğrenmesi çözümlerinizi en verimli şekilde geliştirmek için size gerekeni seçin.


**Bulut tabanlı seçenekleri**

Aşağıdaki seçenekler, machine learning Azure bulutunda için kullanılabilir.

| Bulut&nbsp;seçenekleri | Nedir? | Bununla neler yapabilirsiniz? |
|-|-|-|
| [Azure Machine Learning hizmeti](#azure-machine-learning-services) | ML için yönetilen bulut hizmeti  | Python ve CLI kullanarak Azure’da modelleri eğitme, dağıtma ve yönetme |
| [Azure Machine Learning Studio](#azure-machine-learning-studio) | Sürükleme&ndash;ve&ndash;ML için görsel arabirim bırakma | Derleme, denemeler yapın ve önceden yapılandırılmış algoritmalar (Python ve R) kullanarak modelleri dağıtma|
| [Azure Databricks](#azure-databricks) | Spark tabanlı analiz platformu | Modelleri ve veri iş akışlarını derleme ve dağıtma |
| [Azure Bilişsel Hizmetler](#azure-cognitive-services) | Önceden derlenmiş AI ve ML modellerine sahip Azure hizmetleri | Uygulamalarınıza akıllı özellikleri kolayca ekleme |
| [Azure Veri Bilimi Sanal Makinesi](#azure-data-science-virtual-machine) | Önceden yüklenmiş veri bilimi araçlarına sahip sanal makine | Önceden yapılandırılmış bir ortamda ML çözümleri geliştirme |

**Şirket içi seçenekleri**

Aşağıdaki seçenekler, şirket içi öğrenme makine için kullanılabilir. Şirket içi sunucular, buluttaki bir sanal makine içinde de çalıştırabilirsiniz.

| Şirket içi&nbsp;seçenekleri | Nedir? | Bununla neler yapabilirsiniz? |
|-|-|-|
| [SQL Server Machine Learning Hizmetleri](#sql-server-machine-learning-services) | SQL'e eklenen analiz altyapısı | SQL Server içinde modelleri derleme ve dağıtma |
| [Microsoft Machine Learning Server](#microsoft-machine-learning-server) | Tahmin analizi için tek başına kurumsal sunucu | R ve Python ile modelleri derleme ve dağıtma |

**Geliştirme araçları**

Aşağıdaki geliştirme araçları, machine learning için kullanılabilir.

| Geliştirme&nbsp;araçları | Nedir? | Bununla neler yapabilirsiniz? |
|-|-|-|
| [ML.NET](#mlnet) | Açık kaynak, platformlar arası ML SDK'sı | .NET uygulamaları için ML çözümlerini geliştirme |
| [Windows ML](#windows-ml) | Windows 10 ML platformu | Windows 10 cihazlarında eğitilen modelleri değerlendirme |







## <a name="azure-machine-learning-service"></a>Azure Machine Learning hizmeti

[Azure Machine Learning hizmeti](overview-what-is-azure-ml.md) eğitmek, dağıtmak ve ML modellerini ölçeğe uygun yönetmek için kullanılan bir tam olarak yönetilen bir bulut hizmetidir. Açık kaynak teknolojilerini tamamen destekler, bu nedenle TensorFlow, PyTorch ve scikit-learn gibi açık kaynak Python paketlerinin on binlercesini kullanabilir. Zengin Araçlar gibi kullanılabilir ayrıca [Azure not defterleri](https://notebooks.azure.com/), [Jupyter not defterleri](http://jupyter.org), veya [Visual Studio Code için Azure Machine Learning](https://aka.ms/vscodetoolsforai) keşfetmek kolay hale getirmek için uzantı verileri dönüştürmek ve ardından eğitmek ve Modellerinizi dağıtın. Azure Machine Learning hizmeti kolaylık, verimlilik ve doğrulukla model oluşturma ve ayarlama işlemini otomatik hale getiren özellikler içerir.

Bulut ölçeğinde Python ve CLI kullanarak ML modellerini eğitmek, dağıtmak ve yönetmek için Azure Machine Learning hizmetini kullanın.

Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](http://aka.ms/AMLFree) bugün.

## <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio

[Azure Machine Learning Studio](../studio/what-is-ml-studio.md), önceden derlenmiş makine öğrenmesi algoritmalarını kullanarak modelleri kolayca ve hızla derlemek, test etmek ve dağıtmak için kullanabildiğiniz, etkileşimli ve görsel bir çalışma alanı getirir. Machine Learning Studio, modelleri özel uygulamalar veya Excel gibi BI araçları tarafından kolayca kullanılabilen web hizmetleri olarak yayımlar.
Programlama gerekmez; veri kümelerini ve analiz modüllerini etkileşimli bir tuvalde birbirine bağlayarak makine öğrenmesi modelinizi oluşturur ve ardından bunu birkaç tıklamayla dağıtırsınız.

Modellerinizi hiç koda gerek kalmadan geliştirmek ve dağıtmak istiyorsanız Machine Learning Studio'yu kullanın.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="azure-databricks"></a>Azure Databricks

[Azure Databricks](/azure/azure-databricks/what-is-azure-databricks), Microsoft Azure bulut hizmetleri platformu için iyileştirilen Apache Spark tabanlı bir analiz platformudur. Databricks, tek tıklamayla kurulum olanağı ve kolaylaştırılmış iş akışlarının yanı sıra veri uzmanları, veri mühendisleri ve iş analistleri arasında işbirliği sağlayan etkileşimli bir çalışma alanı sunmak amacıyla Azure ile tümleştirilmiştir.
Verileri sorgulamak, görselleştirmek ve modellemek için web tabanlı not defterlerinde Python, R, Scala ve SQL kodu kullanın.

Apache Spark üzerinde makine öğrenmesi çözümleri derlerken işbirliği yapmak istiyorsanız Databricks kullanın.

## <a name="azure-cognitive-services"></a>Azure Bilişsel Hizmetler

[Azure Bilişsel Hizmetler](/azure/cognitive-services/welcome) doğal iletişim yöntemlerini kullanarak uygulama derlemenizi sağlayan bir dizi API'dir. Bu API'ler birkaç satırlık kodlarla uygulamalarınızın görmesini, duymasını, konuşmasını, anlamasını ve ihtiyaçları yorumlamasını sağlar. Uygulamalarınıza aşağıdaki gibi akıllı özellikleri kolayca ekleyebilirsiniz: 

- Duygu ve düşünceleri algılama
- Görme ve konuşma Tanıma
- Language understanding (LUIS)
- Bilgi ve arama

Bilişsel Hizmetler'i farklı cihaz ve platformlarda uygulama geliştirmek için kullanın. API'ler sürekli olarak geliştirilir ve kolayca ayarlanabilir.

## <a name="azure-data-science-virtual-machine"></a>Azure Veri Bilimi Sanal Makinesi

[Azure Veri Bilimi Sanal Makinesi](../data-science-virtual-machine/overview.md) veri bilimi için Microsoft Azure bulutunda derlenmiş olan özel bir sanal makine ortamıdır. Gelişmiş analiz için akıllı uygulamalar derlemeye hızlı giriş yapmak için önceden yüklenmiş ve önceden yapılandırılmış birçok popüler veri bilimi araçlarına ve diğer araçlara sahiptir.

Veri Bilimi Sanal Makinesi, Azure Machine Learning hizmetinin hedefi olarak desteklenir.
Hem Windows hem de Linux Ubuntu (Azure Machine hizmeti üzerinde Linux CentOS desteklenmiyor Learning) sürümlerinde kullanılabilir.
Belirli sürüm bilgileri ve dahil olan özelliklerin listesi için bkz. [Azure Veri Bilimi Sanal Makinesi'ne giriş](../data-science-virtual-machine/overview.md).

Veri Bilimi Sanal Makinesini işlerinizi tek bir düğüm üzerinde çalıştırmanız veya barındırmanız gerektiğinde kullanın. İşlemlerinizi tek bir makinede uzaktan ölçeklendirmeniz gerektiğinde de kullanabilirsiniz.

## <a name="sql-server-machine-learning-services"></a>SQL Server Machine Learning Hizmetleri

[SQL Server Microsoft Machine Learning Service](https://docs.microsoft.com/sql/advanced-analytics/r/r-services), SQL Server veritabanlarındaki ilişkisel veriler için R ve Python'da istatistiksel analiz, veri görselleştirme ve tahmine dayalı analiz sağlar. Microsoft'un sağladığı R ve Python kitaplıkları, SQL Server'da paralel olarak ve her ölçekte çalıştırılabilen gelişmiş modelleme ve ML algoritmaları içerir.

SQL Server'daki ilişkisel verilerde yerleşik AI ve tahmine dayalı analiz gerektiğinde SQL Server Machine Learning Services'i kullanın.

## <a name="microsoft-machine-learning-server"></a>Microsoft Machine Learning Sunucusu

[Microsoft Machine Learning Sunucusu](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server) R ve Python işlemlerinin paralel ve dağıtılmış iş yüklerini barındırmak ve yönetmek için kullanılan kurumsal sunucudur. Microsoft Machine Learning Server Linux, Windows, Hadoop ve Apache Spark üzerinde çalışır; ayrıca [HDInsight](https://azure.microsoft.com/services/hdinsight/r-server/) üzerinde de kullanılabilir. [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler), [revoscalepy](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) ve [MicrosoftML paketleri](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package) kullanılarak derlenmiş çözümler için yürütme altyapısı sağlar ve açık kaynak R ve Python'u yüksek performanslı analiz, istatistiksel analiz, makine öğrenmesi ve muazzam büyüklükteki veri kümeleri desteğiyle genişletir. Bu işlevler, sunucuyla birlikte yüklenen özel paketler sayesinde sağlanır. Geliştirme için [Visual Studio için R Araçları](https://www.visualstudio.com/vs/rtvs/) ve [Visual Studio için Python Araçları](https://www.visualstudio.com/vs/python/) gibi IDE'leri kullanabilirsiniz.

Sunucuda R ve Python ile derlenmiş modelleri derlemeniz ve çalıştırmanız veya her ölçekte R ve Python eğitimini Hadoop veya Spark kümesine dağıtmanız gerekiyorsa Microsoft Machine Learning Server kullanın.

## <a name="mlnet"></a>ML.NET

[ML.NET](https://docs.microsoft.com/dotnet/machine-learning/), özel makine öğrenmesi çözümleri derlemenize ve bunları .NET uygulamalarınızla tümleştirmenize olanak tanıyan ücretsiz, açık kaynak ve platformlar arası bir makine öğrenmesi çerçevesidir.

Makine öğrenmesi çözümlerini .NET uygulamalarınızla tümleştirmek istediğinizde ML.NET kullanın.

## <a name="windows-ml"></a>Windows ML

[Windows ML](https://docs.microsoft.com/windows/uwp/machine-learning/), eğitilmiş modelleri Windows 10 cihazlarında yerel olarak değerlendirerek, eğitilmiş makine öğrenmesi modellerini uygulamalarınızda kullanmanıza olanak tanır.

Windows uygulamalarınızın içinde eğitilmiş makine öğrenmesi modelleri kullanmak istediğinizde Windows ML kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- Microsoft tarafından sunulan tüm Yapay Zeka (AI) geliştirme ürünleri hakkında bilgi edinmek için bkz. [Microsoft AI platformu](https://www.microsoft.com/ai)
- AI çözümleri geliştirme konusunda eğitim almak için bkz. [Microsoft AI School](https://aischool.microsoft.com/learning-paths)
