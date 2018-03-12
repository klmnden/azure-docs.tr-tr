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
ms.openlocfilehash: 316c35fa4b04bdb251c2b9ae14f6b32f4e4bf939
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2018
---
# <a name="overview-of-microsoft-azure-data-lake-analytics"></a>Microsoft Azure Data Lake Analytics'e genel bakış

## <a name="what-is-azure-data-lake-analytics"></a>Azure Data Lake Analytics nedir?
Azure Data Lake Analytics, büyük verileri kolaylaştıran, isteğe bağlı bir analiz işi hizmetidir. Donanım dağıtma, yapılandırma ve ayarlama işlemleri yerine, verilerinizi dönüştürmek ve değerli öngörüleri ayıklamak için sorgular yazarsınız. Analiz hizmeti sadece ne kadar güce ihtiyacınız olduğunu ayarlayarak her ölçekteki işin üstesinden gelmenizi sağlar. Yalnızca işiniz çalıştırıldığında ücret ödersiniz; bu da hizmeti uygun maliyetli kılar. Analiz hizmetinin desteği içerisinde, SQL’in avantajlarını kesinlik temelli kodun gücüyle birleştiren dil U-SQL de bulunur. U-SQL sayesinde Data Lake Store, Azure’da SQL Server, Azure SQL Veritabanı ve Azure SQL Veri Ambarı arasındaki verileri analiz edebilirsiniz.

## <a name="dynamic-scaling"></a>Dinamik ölçeklendirme
  
Data Lake Analytics, bulut ölçeği ve performans için geliştirilmiştir.  Kaynakları dinamik olarak sağlar ve terabayt ve hatta eksabayt boyutlarındaki veriler üzerinde analiz gerçekleştirmenize olanak tanır. İş tamamlandığında, kaynakları otomatik olarak kapatır ve yalnızca kullanılan işlemci gücü için ücret ödersiniz. Depolanan verilerin veya kullanılan işlem kaynaklarının boyutunu artırdıkça veya azalttıkça yeniden kod yazmanız gerekmez. Yalnızca iş mantığınıza odaklanabilir ve odağınızı büyük veri kümelerini nasıl işleyeceğinize ve depolayacağınıza çevirebilirsiniz.

## <a name="develop-faster-debug-and-optimize-smarter-using-familiar-tools"></a>Alıştığınız araçları kullanarak daha hızlı geliştirin, hataları ayıklayın ve akıllı iyileştirmeler gerçekleştirin
  
Data Lake Analytics, Visual Studio ile kapsamlı bir tümleştirmeye sahiptir; bu, kodunuzu çalıştırmak, ayarlamak ve hata ayıklamak için alıştığınız araçları kullanmanıza olanak tanır. U-SQL işlerinizin görselleştirmeleri, kodunuzun ölçekli olarak nasıl çalıştırıldığını görmenize olanak tanır; bu sayede, kolayca performans sorunlarını tanımlayabilir ve masrafları iyileştirebilirsiniz.


## <a name="u-sql-simple-and-familiar-powerful-and-extensible"></a>U-SQL: Basit ve alışılmış, güçlü ve genişletilebilir
  
Data Lake Analytics, SQL'nin alışılmış, basit ve açıklayıcı yapısını C# dilinin ifade gücüyle genişleten bir sorgu dili olan U-SQL'yi içerir. U-SQL dili, Microsoft içindeki büyük veri sistemlerini destekleyen dağıtılmış çalışma zamanı temelinde geliştirilmiştir. Artık milyonlarca SQL ve .NET geliştiricisi, tüm verilerini sahip oldukları mevcut becerileri kullanarak işleyebilir ve çözümleyebilir.

## <a name="integrates-seamlessly-with-your-it-investments"></a>BT yatırımlarınızla sınırsız şekilde tümleştirilir
  
Data Lake Analytics, kimlik, yönetim, güvenlik ve veri deposu için var olan BT yatırımlarınızı kullanabilir. Bu yaklaşım, veri yönetimini basitleştirir ve geçerli veri uygulamalarınızı genişletmeyi kolaylaştırır. Data Lake Analytics, kullanıcı yönetimi ve izinleri için Active Directory ile tümleştirilmiştir ve yerleşik izleme ve denetim olanaklarıyla sağlanır.

## <a name="affordable-and-cost-effective"></a>Ekonomik ve uygun maliyetli

Data Lake Analytics, büyük veri iş yüklerini çalıştırmaya yönelik uygun maliyetli bir çözümdür. Veri işlendiğinde, iş başına ücret ödersiniz. Hiçbir donanım, lisans veya hizmete özgü destek sözleşmesi gerekli değildir. Sistem, işin başlayıp tamamlanmasıyla otomatik olarak ölçeğini büyütür veya küçültür; böylece, hiçbir zaman ihtiyacınızdan fazlası için ücret ödemezsiniz. [Maliyetleri denetleme ve tasarruf etme hakkında daha fazla bilgi edinin](https://1drv.ms/f/s!AvdZLquGMt47h213Hg3rhl-Tym1c).
    
## <a name="works-with-all-your-azure-data"></a>Tüm Azure verilerinizle çalışır
  
Data Lake Analytics, Azure Data Lake Store ile çalışarak daha yüksek performans, aktarım hızı ve paralelleştirme sağlar; ayrıca Azure Depolama blobları, Azure SQL Veritabanı ve Azure Veri Ambarı ile çalışır.

## <a name="next-steps"></a>Sonraki adımlar
 
  * [Azure Portal](data-lake-analytics-get-started-portal.md) | [Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [CLI](data-lake-analytics-get-started-cli2.md) kullanarak Data Lake Analytics ile çalışmaya başlama
  * [Azure Portal](data-lake-analytics-manage-use-portal.md) | [Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [CLI](data-lake-analytics-manage-use-cli.md) | [Azure .NET SDK](data-lake-analytics-manage-use-dotnet-sdk.md) | [Node.js](data-lake-analytics-manage-use-nodejs.md) kullanarak Azure Data Lake Analytics'i yönetme
  * [Data Lake Analytics ile maliyetleri denetleme ve tasarruf etme](https://1drv.ms/f/s!AvdZLquGMt47h213Hg3rhl-Tym1c)
