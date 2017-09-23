---
title: "Microsoft Azure Data Lake Analytics'e genel bakış | Microsoft Docs"
description: "Data Lake Analytics, konumundan ve boyutundan bağımsız olarak işinizi buluttaki verilerinizden elde edilen öngörülerle desteklemek üzere verileri kullanmanıza olanak tanıyan bir Azure Büyük Veri hizmetidir."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 1e1d443a-48a2-47fb-bc00-bf88274222de
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/23/2017
ms.author: saveenr
ms.translationtype: Human Translation
ms.sourcegitcommit: cb4d075d283059d613e3e9d8f0a6f9448310d96b
ms.openlocfilehash: 8817b511d779029421491194b50120d51ec9dbad
ms.contentlocale: tr-tr
ms.lasthandoff: 06/26/2017

---
# <a name="overview-of-microsoft-azure-data-lake-analytics"></a>Microsoft Azure Data Lake Analytics'e genel bakış
## <a name="what-is-azure-data-lake-analytics"></a>Azure Data Lake Analytics nedir?
Azure Data Lake Analytics, büyük veri analizini kolaylaştırmaya yönelik, isteğe bağlı bir analiz işi hizmetidir. Dağıtılan altyapıyı işletmek yerine iş yazma, çalıştırma ve yönetme işlemlerine odaklanabilirsiniz. Donanım dağıtma, yapılandırma ve ayarlama işlemleri yerine, verilerinizi dönüştürmek ve değerli öngörüleri ayıklamak için sorgular yazarsınız. Analiz hizmeti sadece ne kadar güce ihtiyacınız olduğunu ayarlayarak her ölçekteki işin üstesinden gelmenizi sağlar. Yalnızca işiniz çalıştırıldığında ücret ödersiniz; bu da hizmeti uygun maliyetli kılar. Analiz hizmeti Azure Active Directory desteği sunarak şirket içi kimlik sisteminizde tümleşik olan erişimi ve rolleri kolayca yönetmenize olanak tanır. Ayrıca, SQL'in avantajlarını kullanıcı kodunun ifade gücüyle birleştiren bir dil olan U-SQL'yi de içerir. U-SQL’in ölçeklenebilir dağıtılmış çalışma zamanı depodaki ve Azure, Azure SQL Veritabanı ve Azure SQL Veri Ambarı kapsamındaki verileri verimli bir şekilde analiz etmenizi sağlar.

## <a name="key-capabilities"></a>Temel işlevler
* **Dinamik ölçeklendirme**
  
    Data Lake Analytics, bulut ölçeği ve performans için geliştirilmiştir.  Kaynakları dinamik olarak sağlar ve terabayt ve hatta eksabayt boyutlarındaki veriler üzerinde analiz gerçekleştirmenize olanak tanır. İş tamamlandığında, kaynakları otomatik olarak kapatır ve yalnızca kullanılan işlemci gücü için ücret ödersiniz. Depolanan verilerin veya kullanılan işlem kaynaklarının boyutunu artırdıkça veya azalttıkça yeniden kod yazmanız gerekmez. Yalnızca iş mantığınıza odaklanabilir ve odağınızı büyük veri kümelerini nasıl işleyeceğinize ve depolayacağınıza çevirebilirsiniz.
* **Alıştığınız araçları kullanarak daha hızlı geliştirin, hataları ayıklayın ve iyileştirme gerçekleştirin**
  
    Data Lake Analytics, Visual Studio ile kapsamlı bir tümleştirmeye sahiptir; bu, kodunuzu çalıştırmak, ayarlamak ve hata ayıklamak için alıştığınız araçları kullanmanıza olanak tanır. U-SQL işlerinizin görselleştirmeleri, kodunuzun ölçekli olarak nasıl çalıştırıldığını görmenize olanak tanır; bu sayede, kolayca performans sorunlarını tanımlayabilir ve masrafları iyileştirebilirsiniz.
* **U-SQL: Basit ve alışılmış, güçlü ve genişletilebilir**
  
    Data Lake Analytics, SQL'nin alışılmış, basit ve açıklayıcı yapısını C# dilinin ifade gücüyle genişleten bir sorgu dili olan U-SQL'yi içerir. U-SQL dili, Microsoft içindeki büyük veri sistemlerini destekleyen dağıtılmış çalışma zamanı temelinde geliştirilmiştir. Artık milyonlarca SQL ve .NET geliştiricisi, tüm verilerini sahip oldukları mevcut becerileri kullanarak işleyebilir ve çözümleyebilir.
* **BT yatırımlarınızla sorunsuz şekilde tümleştirilir**
  
    Data Lake Analytics, kimlik, yönetim, güvenlik ve veri deposu için var olan BT yatırımlarınızı kullanabilir. Bu yaklaşım, veri yönetimini basitleştirir ve geçerli veri uygulamalarınızı genişletmeyi kolaylaştırır. Data Lake Analytics, kullanıcı yönetimi ve izinleri için Active Directory ile tümleştirilmiştir ve yerleşik izleme ve denetim olanaklarıyla sağlanır.
* **Ekonomik ve uygun maliyetli**
  
    Data Lake Analytics, büyük veri iş yüklerini çalıştırmaya yönelik uygun maliyetli bir çözümdür. Veri işlendiğinde, iş başına ücret ödersiniz. Hiçbir donanım, lisans veya hizmete özgü destek sözleşmesi gerekli değildir. Sistem, işin başlayıp tamamlanmasıyla otomatik olarak ölçeğini büyütür veya küçültür; böylece, hiçbir zaman ihtiyacınızdan fazlası için ücret ödemezsiniz.
* **Tüm Azure verilerinizle çalışır**
  
    Azure Data Lake ile çalışmak üzere en iyi hale getirilmiş Data Lake Analytics, büyük veri iş yükleriniz için en yüksek performans, çıktı ve paralelleştirme düzeyi sunar.  Data Lake Analytics ayrıca Azure Blob Depolama ve Azure SQL Veritabanı ile de çalışabilir.

## <a name="next-steps"></a>Sonraki adımlar
 
  * [Azure Portal](data-lake-analytics-get-started-portal.md) | [Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [CLI](data-lake-analytics-get-started-cli2.md) kullanarak Data Lake Analytics ile çalışmaya başlama
  * [Azure Portal](data-lake-analytics-manage-use-portal.md) | [Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [CLI](data-lake-analytics-manage-use-cli.md) | [Azure .NET SDK](data-lake-analytics-manage-use-dotnet-sdk.md) | [Node.js](data-lake-analytics-manage-use-nodejs.md) kullanarak Azure Data Lake Analytics'i yönetme
  * [Azure portalını kullanarak Azure Data Lake Analytics işlerini izleme ve sorun giderme](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md) 

