---
title: PostgreSQL için Azure Veritabanı ilişkisel veritabanı hizmetine genel bakış
description: PostgreSQL için Azure Veritabanı ilişkisel veritabanı hizmetine genel bir bakış sağlar.
author: rachel-msft
ms.author: raagyema
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 11/14/2018
ms.openlocfilehash: 318778a83c82b0ddb88f8bbd852442ab389fedb3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60680327"
---
# <a name="what-is-azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı nedir?

PostgreSQL için Azure veritabanı, açık kaynak topluluk sürümünü alan geliştiriciler için oluşturulmuş Microsoft bulutunda bir ilişkisel veritabanı hizmetidir [PostgreSQL](https://www.postgresql.org/) veritabanı altyapısı, sürümler 9.5 9.6 ve 10. PostgreSQL için Azure Veritabanı şunları getirir:

- Ek ücret ödemeden yerleşik yüksek kullanılabilirlik
- Kapsamlı kullandıkça öde fiyatlandırması kullanılarak öngörülebilir performans
- Saniyeler içinde gerektiği gibi ölçeklendirin
- Bekleyen ve hareket halindeki hassas verileri korumaya yönelik güvenlik
- Otomatik yedeklemeler ve 35 güne kadar belirli bir noktaya geri yükleme
- Kurumsal düzeyde güvenlik ve uyumluluk

Tüm bu özellikler neredeyse hiç yönetim gerektirmez ve tümüyle ek ücret ödemeden sağlanır. Bu özellikler sayesinde değerli zamanınızı ve kaynaklarınızı sanal makine ve altyapı yönetimi yerine hızlı uygulama geliştirmeye ve piyasaya sunma sürenizi kısaltmaya ayırabilirsiniz. Buna ek olarak, kendi seçtiğiniz açık kaynak araçları ve platformuyla uygulamanızı geliştirmeye devam edebilir, yeni beceriler edinmek zorunda kalmadan işlerinizin gerektirdiği hızla ve verimlilikle kullanıma sunabilirsiniz. 

Bu makale performans, PostgreSQL için Azure Veritabanı'nın ölçeklenebilirlik ve yönetilebilirlikle ilgili temel kavramlarıyla özelliklerine bir giriş niteliğindedir. Başlamanıza yardımcı olacak şu hızlı başlangıçlara bakın:

- [Azure Portal'ı kullanarak PostgreSQL için Azure Veritabanı oluşturma](quickstart-create-server-database-portal.md)
- [Azure CLI aracını kullanarak PostgreSQL için Azure Veritabanı oluşturma](quickstart-create-server-database-azure-cli.md)

Azure CLI örnekleri için bkz:

- [PostgreSQL için Azure Veritabanı Azure CLI örnekleri](./sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-within-seconds"></a>Saniyeler içinde performansı ve ölçeği ayarlama
Azure PostgreSQL için veritabanı hizmeti üç fiyatlandırma katmanı sunar: Temel, genel amaçlı ve bellek için iyileştirilmiş. Her katman veritabanı iş yükünüzü desteklemek için farklı kaynak özellikleri sunar. İlk uygulamanızı aylık birkaç dolar ücretle küçük bir veritabanı üzerinde oluşturabilir ve sonra çözümünüzün gereksinimlerine göre ölçeği ayarlayabilirsiniz. Dinamik ölçeklendirebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine saydam bir şekilde yanıt verebilmesini sağlar. Yalnızca ihtiyacınız olan kaynaklar için ve yalnızca bunlara ihtiyacınız olduğunda ödeme yaparsınız. Bkz: [fiyatlandırma katmanları](concepts-pricing-tiers.md) Ayrıntılar için.

## <a name="monitoring-and-alerting"></a>İzleme ve uyarı
Ölçeğin ne zaman artırılacağına veya azaltılacağına nasıl karar verirsiniz? Yerleşik Azure izleme ve uyarı özelliklerini kullanarak. Bu araçları kullanarak geçerli veya tahmini performans veya depolama ihtiyaçlarınıza göre ölçeği büyütme veya küçültme işlemlerinin etkisini hızla değerlendirebilirsiniz. Ayrıntılar için bkz. [Uyarılar](howto-alert-on-metric.md).

## <a name="keep-your-app-and-business-running"></a>Uygulamanızın ve işinizin hiç kesintiye uğramamasını sağlayın
Azure'ın Microsoft yönetimindeki veri merkezlerinin küresel bir ağı tarafından desteklenen ve endüstri lideri niteliğinde %99,99'luk bir kullanılabilirlik oranı sunan hizmet düzeyi sözleşmesi (SLA), uygulamanızın 7/24 kesintiye uğramamasına yardımcı olur. PostgreSQL için Azure Veritabanı sunucuları sayesinde satın almanız, tasarlamanız, oluşturmanız ve yönetmeniz gerekmeksizin; yerleşik güvenlik, hataya dayanıklılık ve veri koruması olanaklarından yararlanırsınız. PostgreSQL için Azure Veritabanı'ndaki her fiyatlandırma katmanı, uygulamanızı ve işinizi oluşturup çalışır halde kalmasını sağlama amacıyla kullanabileceğiniz kapsamlı bir iş sürekliliği özellikleri ve seçenekleri kümesi sunar. Bir veritabanını daha önceki bir durumuna (35 güne kadar) geri döndürmek üzere[ belirli bir noktaya geri yükleme](howto-restore-server-portal.md) işlemini gerçekleştirebilirsiniz. Ayrıca, veritabanlarınızı barındıran veri merkezinde bir kesinti oluşursa, son yedeklemelerin coğrafi olarak yedekli kopyalarından veritabanlarını geri yükleyebilirsiniz.

## <a name="secure-your-data"></a>Verilerinizin güvenliğini sağlama
Azure veritabanı hizmetlerinin veri güvenliği geleneği, PostgreSQL için Azure Veritabanı'nda da erişimi sınırlayan, bekleyen ve hareket halindeki verileri koruyan ve etkinliği izlemenize yardımcı olan özelliklerle sürdürülür. Azure'ın platform güvenliği hakkında bilgi edinmek için [Azure Güven Merkezi](https://azure.microsoft.com/overview/trusted-cloud/)'ni ziyaret edin.

Hizmet PostgreSQL için Azure veritabanı, bekleyen veri için depolama şifrelemesi kullanır ve FIPS 140-2 uyumludur. Yedekler de dahil olmak üzere veriler diskte şifrelenir (altyapı tarafından sorgular çalıştırılırken oluşturulan geçici dosyalar hariç). Hizmet, Azure depolama şifrelemesi kapsamında yer alan AES 256-bit şifrelemesini kullanır ve anahtarlar sistem tarafından yönetilir. Depolama şifrelemesi her zaman açıktır ve devre dışı bırakılamaz.

Varsayılan olarak, PostgreSQL için Azure Veritabanı hizmeti ağ genelinde hareket halindeki veriler için [SSL bağlantı güvenliğini](./concepts-ssl-connection-security.md) zorunlu tutacak şekilde yapılandırılmıştır. Veritabanı sunucunuzla istemci uygulamalarınız arasında SSL bağlantılarının zorunlu tutulması, sunucuya uygulamanız arasındaki veri akışını şifreleyerek "bağlantıyı izinsiz izleme" saldırılarına karşı korumaya yardımcı olur. İsteğe bağlı olarak, istemci uygulamanızın SSL bağlantısını desteklememesi durumunda veritabanı hizmetinize bağlanmak için SSL zorunluluğunu devre dışı bırakabilirsiniz.

## <a name="contacts"></a>Kişiler
PostgreSQL için Azure Veritabanı’yla çalışma hakkında sorularınız veya önerileriniz varsa, PostgreSQL için Azure Veritabanı Ekibi’ne bir e-posta gönderin ([@Ask PostgreSQL için Azure DB](mailto:AskAzureDBforPostgreSQL@service.microsoft.com)). Bunun bir teknik destek diğer adı olmadığını unutmayın.

Buna ek olarak, aşağıdaki iletişim noktalarını uygun şekilde göz önünde bulundurun:
- Azure Desteği ile iletişim kurmak için [Azure portaldan bir bilet oluşturun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Hesabınızla ilgili bir sorun gidermek için Azure portalda bir [destek isteği](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) oluşturun.
- Görüş bildirmek veya yeni özellikler istemek için [UserVoice](https://feedback.azure.com/forums/597976-azure-database-for-postgresql) aracılığıyla bir giriş oluşturun.

## <a name="next-steps"></a>Sonraki adımlar
- Maliyet karşılaştırmaları ve hesaplayıcıları için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/postgresql/) bakın.
- [İlk PostgreSQL için Azure Veritabanınızı oluşturarak](./quickstart-create-server-database-portal.md) başlangıç yapın.
- Python, PHP, Ruby, C ilk uygulamanızı oluşturun\#, Java, Node.js: [Bağlantı kitaplıkları](./concepts-connection-libraries.md)
