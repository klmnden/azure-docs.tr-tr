---
title: "PostgreSQL için Azure Veritabanı ilişkisel veritabanı hizmetine genel bakış | Microsoft Docs"
description: "PostgreSQL için Azure Veritabanı ilişkisel veritabanı hizmetine genel bir bakış sağlar."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 10/20/2017
ms.openlocfilehash: 9aa24dd10ef29c716c05cafeb84e0beb23d50628
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="what-is-azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı nedir?

PostgreSQL için Azure Veritabanı, açık kaynak [PostgreSQL](https://www.postgresql.org/) veritabanı altyapısının topluluk sürümünü temel alan, Microsoft bulutunda geliştiriciler için oluşturulmuş ilişkisel bir veritabanı hizmetidir. Bu hizmet genel önizleme aşamasındadır. PostgreSQL için Azure Veritabanı şunları getirir:

- Ek ücret ödemeden yerleşik yüksek kullanılabilirlik.
- Kapsamlı kullandıkça öde fiyatlandırması kullanılarak öngörülebilir performans.
- Saniyeler içinde hemen ölçeklendirme.
- Bekleyen ve hareket halindeki hassas verileri korumaya yönelik güvenlik.
- Otomatik yedeklemeler ve 35 güne kadar belirli bir noktaya geri yükleme.
- Kurumsal düzeyde güvenlik ve uyumluluk.

Tüm bu özellikler neredeyse hiç yönetim gerektirmez ve tümüyle ek ücret ödemeden sağlanır. Bu özellikler sayesinde değerli zamanınızı ve kaynaklarınızı sanal makine ve altyapı yönetimi yerine hızlı uygulama geliştirmeye ve piyasaya sunma sürenizi kısaltmaya ayırabilirsiniz. Buna ek olarak, kendi seçtiğiniz açık kaynak araçları ve platformuyla uygulamanızı geliştirmeye devam edebilir, yeni beceriler edinmek zorunda kalmadan işlerinizin gerektirdiği hızla ve verimlilikle kullanıma sunabilirsiniz. 

Bu makale performans, PostgreSQL için Azure Veritabanı'nın ölçeklenebilirlik ve yönetilebilirlikle ilgili temel kavramlarıyla özelliklerine bir giriş niteliğindedir. Başlamanıza yardımcı olacak şu hızlı başlangıçlara bakın:

- [Azure Portal'ı kullanarak PostgreSQL için Azure Veritabanı oluşturma](quickstart-create-server-database-portal.md)
- [Azure CLI aracını kullanarak PostgreSQL için Azure Veritabanı oluşturma](quickstart-create-server-database-azure-cli.md)

Azure CLI örnekleri için bkz:

- [PostgreSQL için Azure Veritabanı Azure CLI örnekleri](./sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-within-seconds"></a>Saniyeler içinde performansı ve ölçeği ayarlama
Önizleme'de, PostgreSQL için Azure Veritabanı hizmeti iki hizmet katmanı sunar: Temel ve Standart. Her katman, hafiften ağıra kadar tüm iş yüklerini desteklemek üzere farklı performans ve özellikler getirir. İlk uygulamanızı aylık birkaç dolar ücretle küçük bir veritabanı üzerinde oluşturabilir ve sonra çözümünüzün gereksinimlerine göre ölçeği ayarlayabilirsiniz. Dinamik ölçeklendirebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine saydam bir şekilde yanıt verebilmesini sağlar. Yalnızca ihtiyacınız olan kaynaklar için ve yalnızca bunlara ihtiyacınız olduğunda ödeme yaparsınız. Ayrıntılar için bkz. [Fiyatlandırma katmanları](concepts-service-tiers.md).

## <a name="monitoring-and-alerting"></a>İzleme ve uyarı
Ne zaman artırılacağına ve ne zaman azaltılacağını nasıl karar verirsiniz? Yerleşik performans izleme ve uyarı özelliklerini, İşlem Birimlerini temel alan performans değerlendirmeleriyle birlikte kullanırsınız. Bu araçları kullanarak geçerli veya projeye özgü performans ihtiyaçlarınıza göre İşlem Birimlerinin ölçeğini büyütme veya küçültme işlemlerinin etkisini hızla değerlendirebilirsiniz. Ayrıntılar için bkz. [Uyarılar](howto-alert-on-metric.md).

## <a name="keep-your-app-and-business-running"></a>Uygulamanızın ve işinizin hiç kesintiye uğramamasını sağlayın
Azure'un Microsoft yönetimindeki veri merkezlerinin küresel bir ağı tarafından desteklenen ve endüstri lideri niteliğinde %99,99'luk bir kullanılabilirlik oranı sunan (önizleme aşamasında sağlanmaz) hizmet düzeyi sözleşmesi (SLA), uygulamanızın 7/24 kesintiye uğramamasına yardımcı olur. PostgreSQL için Azure Veritabanı sunucuları sayesinde satın almanız, tasarlamanız, oluşturmanız ve yönetmeniz gerekmeksizin; yerleşik güvenlik, hataya dayanıklılık ve veri koruması olanaklarından yararlanırsınız. PostgreSQL için Azure Veritabanı'ndaki her hizmet katmanı, uygulamanızı ve işinizi oluşturup çalışır halde kalmasını sağlamanız amacıyla kullanabileceğiniz kapsamlı bir iş sürekliliği özellikleri ve seçenekleri kümesi sunar. Bir veritabanını daha önceki bir durumuna (35 güne kadar) geri döndürmek üzere[ belirli bir noktaya geri yükleme](howto-restore-server-portal.md) işlemini gerçekleştirebilirsiniz. Ayrıca, veritabanlarınızı barındıran veri merkezinde bir kesinti oluşursa, son yedeklemelerin coğrafi olarak yedekli kopyalarından veritabanlarını geri yükleyebilirsiniz.

## <a name="secure-your-data"></a>Verilerinizin güvenliğini sağlama
Azure veritabanı hizmetlerinin veri güvenliği geleneği, PostgreSQL için Azure Veritabanı'nda da erişimi sınırlayan, bekleyen ve hareket halindeki verileri koruyan ve etkinliği izlemenize yardımcı olan özelliklerle sürdürülür. Azure'ın platform güvenliği hakkında bilgi edinmek için [Azure Güven Merkezi](https://www.microsoft.com/TrustCenter/Security/default.aspx)'ni ziyaret edin.

PostgreSQL için Azure Veritabanı hizmeti bekleyen verilerde depolama şifrelemesini kullanır. Yedekler de dahil olmak üzere veriler diskte şifrelenir (altyapı tarafından sorguları çalıştırırken oluşturulan geçici dosyalar hariç). Hizmet, Azure depolama şifrelemesi kapsamında yer alan AES 256-bit şifrelemesini kullanır ve anahtarlar sistem tarafından yönetilir. Depolama şifrelemesi her zaman açıktır ve devre dışı bırakılamaz.

Varsayılan olarak, PostgreSQL için Azure Veritabanı hizmeti ağ genelinde hareket halindeki veriler için [SSL bağlantı güvenliğini](./concepts-ssl-connection-security.md) zorunlu tutacak şekilde yapılandırılmıştır. Veritabanı sunucunuzla istemci uygulamalarınız arasında SSL bağlantılarının zorunlu tutulması, sunucuya uygulamanız arasındaki veri akışını şifreleyerek "bağlantıyı izinsiz izleme" saldırılarına karşı korumaya yardımcı olur.  İsteğe bağlı olarak, istemci uygulamanızın SSL bağlantısını desteklememesi durumunda veritabanı hizmetinize bağlanmak için SSL zorunluluğunu devre dışı bırakabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- Maliyet karşılaştırmaları ve hesaplayıcıları için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/postgresql/) bakın.
- [İlk PostgreSQL için Azure Veritabanınızı oluşturarak](./quickstart-create-server-database-portal.md) başlangıç yapın.
- Python, PHP, Ruby, C\#, Java, Node.js'de ilk uygulamanızı oluşturun: [Bağlantı kitaplıkları](./concepts-connection-libraries.md)
