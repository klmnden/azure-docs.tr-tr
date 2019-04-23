---
title: -Azure SQL veritabanı otomatik ayarlama | Microsoft Docs
description: Azure SQL veritabanı, SQL sorguyu analiz eder ve kullanıcı iş yüküne otomatik olarak uyum sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
manager: craigg
ms.date: 03/06/2019
ms.openlocfilehash: 6e818da29b7ee0d17ebe4f8e523648146973fa63
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59796625"
---
# <a name="automatic-tuning-in-azure-sql-database"></a>Azure SQL veritabanı'nda otomatik ayarlama

Azure SQL veritabanı otomatik ayarlama, yapay ZEKA ve makine öğrenimi göre sürekli performans ayarlama yoluyla en yüksek performans ve kararlı iş yükleri sunar.

Otomatik ayarlama, bir veritabanı üzerinde yürütülen sorgular sürekli olarak izlemek için yerleşik zeka kullanan bir tam olarak yönetilen akıllı performans hizmet ve performanslarını otomatik olarak geliştirir. Bu, veritabanı iş yüklerini değiştirme ve ayar önerileri uygulama uyarlama aracılığıyla dinamik olarak sağlanır. Otomatik ayarlama Azure aracılığıyla yapay ZEKA ile ilgili tüm veritabanlarından yatay olarak öğrenir ve dinamik olarak ayarlama eylemlerinin artırır. Otomatik üzerinde ayarlama ile Azure SQL veritabanı uzun süre çalışan, daha iyi gerçekleştirir.

Azure SQL veritabanı otomatik ayarlama, kararlı ve üst düzey veritabanı iş yüklerinin performans sağlamak için etkinleştirebileceğiniz en önemli özelliklerden biri olabilir.

## <a name="what-can-automatic-tuning-do-for-you"></a>Otomatik ayarlama sizin için neler?

- Azure SQL veritabanları otomatik performans ayarlama
- Otomatik performans kazancı elde edildi doğrulanması
- Otomatik geri alma ve kendi kendine düzeltme
- Ayarlama geçmişi
- Eylem T-SQL betikleri dağıtımlar için ayarlama
- Proaktif iş yükü performansı izleme
- Yüz binlerce veritabanını yeteneğini ölçeği genişletme
- Olumlu etkiyi DevOps kaynaklara ve toplam sahip olma maliyeti

## <a name="safe-reliable-and-proven"></a>Güvenli, güvenilir ve kendini kanıtlamış

Azure SQL'e uygulanan işlemlerde veritabanları için en yoğun iş yüklerinizin performansını tam olarak güvenli değildir. Sistem dikkatli kullanıcı yükleriyle karışmamasına için tasarlanmıştır. Otomatik ayarlama önerileri, yalnızca düşük kullanımı zamanlarda uygulanır. Sistem iş yükü performansını korumak için otomatik ayarlama işlemlerini de geçici olarak devre dışı bırakabilirsiniz. Böyle bir durumda, Azure portalında "Sistem tarafından devre dışı" iletisi gösterilir. Otomatik ayarlama, en yüksek kaynak önceliğe sahip iş yüklerini değerlendirir.

Otomatik ayarlama mekanizmaları olgun ve Azure üzerinde çalışan birkaç milyon veritabanlarında perfected. Uygulanan otomatik ayarlama işlemleri, iş yükü performansına olumlu bir geliştirme olduğundan emin olmak için otomatik olarak doğrulanır. Azaltılmış performansa önerileri dinamik olarak algılandı ve en kısa sürede geri döndürüldü. Kaydedilen ayarlama geçmişi ile her bir Azure SQL veritabanı için yapılan geliştirmeleri ayarlama, düz bir izleme vardır. 

![Nasıl otomatik ayarlama çalışıyor mu](./media/sql-database-automatic-tuning/how-does-automatic-tuning-work.png)

Azure SQL veritabanı otomatik ayarlama, SQL Server otomatik ayarlama altyapısıyla çekirdek mantığını paylaşıyor. Yerleşik zeka mekanizması hakkında ek teknik bilgiler için bkz. [SQL Server otomatik ayarlama](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning).

## <a name="use-automatic-tuning"></a>Otomatik ayarlama kullanın

Otomatik ayarlama, aboneliğinizde etkinleştirilmesi gerekir. Azure portalını kullanarak bir otomatik ayarlama etkinleştirmek için bkz: [otomatik ayarlamayı etkinleştirme](sql-database-automatic-tuning-enable.md).

Otomatik ayarlama otomatik doğrulama performans artışı, dahil olmak üzere ayar önerileri otomatik olarak uygulanması aracılığıyla otonom olarak çalışabilir. 

Daha fazla denetim otomatik ayarlama önerileri uygulaması kapatılabilir ve ayarlama önerileri el ile Azure Portalı aracılığıyla uygulanabilir. Yalnızca otomatik ayarlama önerileri görüntülemek ve betikler ve tercih ettiğiniz araçları el ile uygulamanız için çözümü kullanmak da mümkündür. 

Otomatik ayarlama works genel bir bakış ve tipik kullanım senaryoları için katıştırılmış video bakın:


> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Improve-Azure-SQL-Database-Performance-with-Automatic-Tuning/player]
>

## <a name="automatic-tuning-options"></a>Otomatik ayarlama seçeneklerini

Azure SQL veritabanı'nda kullanılabilir otomatik ayarlama seçeneklerini şunlardır:

| Otomatik ayarlama seçeneği | Tek veritabanı ve havuza veritabanı desteği | Örnek veritabanı desteği |
| :----------------------------- | ----- | ----- |
| **CREATE INDEX** -İş yükünüzün performansını artırabilir, dizinler oluşturan ve sorguların performansını iyileştirildiğini otomatik olarak doğrular dizinleri tanımlar. | Evet | Hayır | 
| **DROP INDEX** -yedekli ve yinelenen dizinleri benzersiz ve uzun süre kullanılmamış dizinleri hariç olmak üzere günlük olarak tanımlar. (> 90 gün). Lütfen şu anda seçeneği bölüm değiştirme ve dizin ipuçlarını kullanarak uygulamaları ile uyumlu olmadığını unutmayın. | Evet | Hayır |
| **SON iyi planı ZORLA** (otomatik plan düzeltme) - önceki iyi planı yavaştır ve azaltılmış planı yerine bilinen son iyi planı kullanan sorgular yürütme planını kullanarak tanımlayan SQL sorguları. | Evet | Evet |

Otomatik ayarlama tanımlayan **CREATE INDEX**, **DROP INDEX**, ve **ZORLA son iyi planı** veritabanınızın performansı iyileştirebilir ve bunları gösterir önerileri [Azure portalında](sql-database-advisor-portal.md)ve bunları üzerinden kullanıma sunan [T-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azuresqldb-current) ve [REST API](https://docs.microsoft.com/rest/api/sql/serverautomatictuning). SON iyi planı ZORLA hakkında daha fazla bilgi için bkz: T-SQL aracılığıyla otomatik ayarlama seçeneklerini yapılandırma [otomatik ayarlama, otomatik plan düzeltme tanıtır](https://azure.microsoft.com/blog/automatic-tuning-introduces-automatic-plan-correction-and-t-sql-management/).

Ya da portalı kullanarak ayar önerileri el ile uygulayabilirsiniz veya otomatik olarak çalışabilen uygulamak için ayar önerileri ayarlama sağlayabilirsiniz. Otonom olarak ayarlama önerileri için geçerli sistem izin vererek avantajları olduğundan, otomatik olarak var. doğrulama iş yükü performansına olumlu bir kazancı var ve algılanan hiçbir önemli bir performans geliştirmesi ise çalışır otomatik ayarlama önerileri geri al. Sıklıkla yürütülen değil önerilerinde tarafından etkilenen sorgular olması durumunda, en fazla 72 doğrulama aşamasını alabileceğine Lütfen dikkat edin. Tasarım saat.

El ile ayarlama uyguladığınızı durumunda öneriler, otomatik performans doğrulama ve ters mekanizmaları kullanılamaz. Ayrıca, el ile uygulanan önerileri 24-48 saat için etkin ve öneriler listesinde gösterilen kalır. Sistem otomatik olarak bunları testimizde önce. Bir öneri daha çabuk kaldırmak istiyorsanız, el ile iptal edebilirsiniz.

Otomatik ayarlama seçeneklerini bağımsız olarak etkinleştirilebilir veya veritabanı başına devre dışı veya bunlar üzerinde SQL veritabanı sunucuları yapılandırılabilir ve sunucudan ayarları devralıyor her bir veritabanına uygulanır. SQL veritabanı sunucularını otomatik ayarlama ayarları için Azure Varsayılanları devralabilir. Şu anda Azure Varsayılanları ayarlandığında FORCE_LAST_GOOD_PLAN etkin CREATE_INDEX etkin ve DROP_INDEX devre dışı bırakıldı.

Otomatik bir sunucuda seçenekleri ayarlama ve üst sunucuya ait veritabanlarına yönelik ayarları devralmayı yapılandırma gibi çok sayıda veritabanı otomatik ayarlama seçeneklerini yönetimini basitleştirir, otomatik ayarlama yapılandırmak için önerilen bir yöntemdir.

## <a name="next-steps"></a>Sonraki adımlar

- İş yükünüz yönetmek için Azure SQL veritabanında otomatik ayarlama etkinleştirmek için bkz: [otomatik ayarlamayı etkinleştirme](sql-database-automatic-tuning-enable.md).
- El ile incelemeniz ve otomatik ayarlama önerileri uygulamak için bkz: [bulun ve performans önerilerini uygulama](sql-database-advisor-portal.md).
- Otomatik ayarlama önerileri görüntüleyin ve uygulamak için T-SQL kullanma bilgi edinmek için [T-SQL otomatik ayarlama yönetme](https://azure.microsoft.com/blog/automatic-tuning-introduces-automatic-plan-correction-and-t-sql-management/).
- E-posta bildirimlerini otomatik ayarlama önerileri için oluşturma hakkında bilgi edinmek için [e-posta bildirimlerini otomatik ayarlama](sql-database-automatic-tuning-email-notifications.md).
- Yerleşik zeka kullanılan otomatik ayarlama hakkında bilgi edinmek için bkz. [yapay zeka, Azure SQL veritabanlarına tabanlarını](https://azure.microsoft.com/blog/artificial-intelligence-tunes-azure-sql-databases/).
- Azure SQL veritabanı ve SQL Server 2017 otomatik ayarlama çalıştığı hakkında bilgi edinmek için [SQL Server otomatik ayarlama](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning).
