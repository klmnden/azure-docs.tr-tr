---
title: "Microsoft Azure Data Lake Analytics&quot;e genel bakış | Microsoft Belgeleri"
description: "Data Lake Analytics, konumundan ve boyutundan bağımsız olarak işinizi buluttaki verilerinizden elde edilen öngörülerle desteklemek üzere verileri kullanmanıza olanak tanıyan bir Azure Büyük Veri hizmetidir."
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: 1e1d443a-48a2-47fb-bc00-bf88274222de
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/28/2017
ms.author: edmaca
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 12d6fe834ed2b31a756123351288eec7ba2a72f7
ms.lasthandoff: 03/22/2017


---
# <a name="overview-of-microsoft-azure-data-lake-analytics"></a>Microsoft Azure Data Lake Analytics'e genel bakış
## <a name="what-is-azure-data-lake-analytics"></a>Azure Data Lake Analytics nedir?
Azure Data Lake Analytics, büyük veri analizini kolaylaştırmaya yönelik, isteğe bağlı bir analiz işi hizmetidir. Dağıtılan altyapıyı işletmek yerine, iş yazma, çalıştırma ve yönetme işlemlerine odaklanın. Donanım dağıtma, yapılandırma ve ayarlama işlemleri yerine, verilerinizi dönüştürmek ve değerli öngörüleri ayıklamak için sorgular yazarsınız. Analiz hizmeti sadece ne kadar güce ihtiyacınız olduğunu ayarlayarak her ölçekteki işin üstesinden gelmenizi sağlar. Yalnızca işiniz çalıştırıldığında ücret ödersiniz; bu da hizmeti uygun maliyetli kılar. Analiz hizmeti Azure Active Directory desteği sunarak şirket içi kimlik sisteminizde tümleşik olan erişimi ve rolleri kolayca yönetmenize olanak tanır. Ayrıca, SQL'in avantajlarını kullanıcı kodunun ifade gücüyle birleştiren bir dil olan U-SQL'yi de içerir. U-SQL’in ölçeklenebilir dağıtılmış çalışma zamanı depodaki ve Azure, Azure SQL Veritabanı ve Azure SQL Veri Ambarı kapsamındaki verileri verimli bir şekilde analiz etmenizi sağlar.

## <a name="key-capabilities"></a>Temel işlevler
* **Dinamik ölçeklendirme**
  
    Data Lake Analytics, bulut ölçeği ve performans için geliştirilmiştir.  Kaynakları dinamik olarak sağlar ve terabayt ve hatta eksabayt boyutlarındaki veriler üzerinde analiz gerçekleştirmenize olanak tanır. İş tamamlandığında, kaynakları otomatik olarak kapatır ve yalnızca kullanılan işlemci gücü için ücret ödersiniz. Depolanan verilerin veya kullanılan hesaplamanın boyutunu artırdıkça veya azalttıkça kodu yeniden yazmanız gerekmez. Yalnızca iş mantığınıza odaklanabilir ve odağınızı büyük veri kümelerini nasıl işleyeceğinize ve depolayacağınıza çevirebilirsiniz.
* **Alıştığınız araçları kullanarak daha hızlı geliştirin, hataları ayıklayın ve iyileştirme gerçekleştirin**
  
    Data Lake Analytics, Visual Studio ile derin bir tümleştirmeye sahiptir; böylece kodunuzu çalıştırmak, hatalarını ayıklama veya ayarlamak için alıştığınız araçları kullanabilirsiniz. U-SQL işlerinizin görselleştirmeleri, kodunuzun ölçekli olarak nasıl çalıştırıldığını görmenize olanak tanır; bu sayede, kolayca performans sorunlarını tanımlayabilir ve masrafları iyileştirebilirsiniz.
* **U-SQL: Basit ve alışılmış, güçlü ve genişletilebilir**
  
    Data Lake Analytics, SQL'nin alışılmış, basit ve açıklayıcı yapısını C# dilinin ifade gücüyle genişleten bir sorgu dili olan U-SQL'yi içerir. U-SQL dili, Microsoft içindeki büyük veri sistemlerini destekleyen dağıtılmış çalışma zamanı temelinde geliştirilmiştir. Artık milyonlarca SQL ve .NET geliştiricisi, tüm verilerini sahip oldukları mevcut becerileri kullanarak işleyebilir ve çözümleyebilir.
* **BT yatırımlarınızla sorunsuz şekilde tümleştirilir**
  
    Data Lake Analytics, kimlik, yönetim, güvenlik ve veri deposu için var olan BT yatırımlarınızı kullanabilir. Bu, veri yönetimini basitleştirir ve geçerli veri uygulamalarınızı genişletmeyi kolaylaştırır. Data Lake Analytics, kullanıcı yönetimi ve izinleri için Active Directory ile tümleştirilmiştir ve yerleşik izleme ve denetim olanaklarıyla sağlanır.
* **Ekonomik ve uygun maliyetli**
  
    Data Lake Analytics, büyük veri iş yüklerini çalıştırmaya yönelik uygun maliyetli bir çözümdür. Veri işlendiğinde, iş başına ücret ödersiniz. Hiçbir donanım, lisans veya hizmete özgü destek sözleşmesi gerekli değildir. Sistem, işin başlayıp tamamlanmasıyla otomatik olarak ölçeğini büyütür veya küçültür; bu da asla ihtiyacınızdan fazlası için ücret ödememeniz anlamına gelir.
* **Tüm Azure verilerinizle çalışır**
  
    Azure Data Lake ile çalışmak üzere en iyi hale getirilmiş Data Lake Analytics, büyük veri iş yükleriniz için en yüksek performans, çıktı ve paralelleştirme düzeyi sunar.  Data Lake Analytics ayrıca Azure Blob depolama ve Azure SQL veritabanı ile de çalışabilir.

## <a name="see-also"></a>Ayrıca bkz.
* Başlarken
  
  * [Azure portalını kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
  * [Azure PowerShell'i kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-powershell.md)
  * [Azure .NET SDK'yı kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-net-sdk.md)
  * [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md)
  * [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md)
* U-SQL ve geliştirme
  
  * [Azure Data Lake Analytics işleri için U-SQL pencere işlevlerini kullanma](data-lake-analytics-use-window-functions.md)
  * [Data Lake Analytics işleri için U-SQL kullanıcı tanımlı işleçleri geliştirme](data-lake-analytics-u-sql-develop-user-defined-operators.md)
* Yönetim
  
  * [Azure Data Lake Analytics'i Azure portalını kullanarak yönetme](data-lake-analytics-manage-use-portal.md)
  * [Azure Data Lake Analytics'i Azure PowerShell'i kullanarak yönetme](data-lake-analytics-manage-use-powershell.md)
  * [Azure portalını kullanarak Azure Data Lake Analytics işlerini izleme ve sorun giderme](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
  * [Azure Data Lake Analytics için Tanılama günlüklerine erişme](data-lake-analytics-diagnostic-logs.md)
* Kapsamlı öğretici
  
  * [Azure Data Lake Analytics etkileşimli öğreticilerini kullanma](data-lake-analytics-use-interactive-tutorials.md)
  * [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md)
* Bize düşüncelerinizi iletin
  
  <!-- Fixing broken links for Azure content migration from ACOM to DOCS. I can't find a suitable substitute for what appears to be a link that is no longer available. I am commenting out for now. The author can investigate in the future. Hyperlink text: Comment on our documentation backlog. Referenced file: data-lake-analytics-documentation-backlog.md -->
  * [Özellik isteği gönderin](http://aka.ms/adlafeedback)
  * [Forumlarda yardım alın](http://aka.ms/adlaforums)


