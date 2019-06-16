---
title: MySQL ilişkisel veritabanı hizmeti için Azure veritabanı'nın genel bakış
description: MySQL ilişkisel veritabanı hizmeti için Azure veritabanı genel bakış.
ms.service: mysql
author: ajlam
ms.author: andrela
ms.custom: mvc
ms.topic: conceptual
ms.date: 03/20/2019
ms.openlocfilehash: 2852cab05fab8e15b7e60a22f54cc866d2f0f178
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61226259"
---
# <a name="what-is-azure-database-for-mysql"></a>MySQL için Azure veritabanı nedir?

MySQL için Azure veritabanı, bir ilişkisel veritabanı hizmetidir Microsoft bulutunda temel [MySQL Community Edition](https://www.mysql.com/products/community/) (GPLv2 lisansı altında kullanılabilir) veritabanı altyapısı, sürüm 5.6 ve 5.7. MySQL için Azure veritabanı şunları getirir:

- Ek ücret ödemeden yerleşik yüksek kullanılabilirlik.
- Kapsamlı kullandıkça öde fiyatlandırması kullanılarak öngörülebilir performans.
- Saniyeler içinde gerektiği gibi ölçeklendirin.
- Bekleyen ve hareket halindeki hassas verileri korumaya yönelik güvenlik.
- Otomatik yedeklemeler ve 35 güne kadar belirli bir noktaya geri yükleme.
- Kurumsal düzeyde güvenlik ve uyumluluk.

Bu özellikler neredeyse hiç yönetim gerektirmez ve tümüyle ek ücret ödemeden sağlanır. Bunlar sayesinde değerli zamanınızı ve kaynaklarınızı sanal makine ve altyapı yönetimi yerine hızlı uygulama geliştirmeye ve piyasaya sunma sürenizi kısaltmaya ayırabilirsiniz. Buna ek olarak, kendi seçtiğiniz açık kaynak araçları ve platformuyla uygulamanızı geliştirmeye devam edebilir, böylelikle yeni beceriler edinmek zorunda kalmadan işlerinizin gerektirdiği hızla ve verimlilikle kullanıma sunabilirsiniz.

![Kavramsal diyagram MySQL için Azure veritabanı](media/overview/1-azure-db-for-mysql-conceptual-diagram.png)

Bu makalede, MySQL temel kavramları ve performans, ölçeklenebilirlik ve yönetilebilirlik ayrıntılarını incelemek için bağlantılar ile ilgili özellikler için Azure veritabanı giriş niteliğindedir. Başlamanıza yardımcı olacak şu hızlı başlangıçlara bakın:

- [Azure portalını kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](quickstart-create-mysql-server-database-using-azure-portal.md)
- [Azure CLI kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](quickstart-create-mysql-server-database-using-azure-cli.md)

Azure CLI örnekleri için bkz:

- [MySQL için Azure veritabanı Azure CLI örnekleri](sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-within-seconds"></a>Saniyeler içinde performansı ve ölçeği ayarlama
MySQL hizmeti için Azure veritabanı, birkaç hizmet katmanı sunar: Temel, genel amaçlı ve bellek için iyileştirilmiş. Her katman, hafiften ağıra kadar tüm iş yüklerini desteklemek üzere farklı performans ve özellikler getirir. İlk uygulamanızı aylık birkaç dolar ücretle küçük bir veritabanı üzerinde oluşturabilir ve sonra çözümünüzün gereksinimlerine göre ölçeği ayarlayabilirsiniz. Dinamik ölçeklendirebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine saydam bir şekilde yanıt verebilmesini sağlar. Yalnızca ihtiyacınız olan kaynaklar için ve yalnızca bunlara ihtiyacınız olduğunda ödeme yaparsınız. Bkz: [fiyatlandırma katmanları](concepts-service-tiers.md) Ayrıntılar için.

## <a name="monitoring-and-alerting"></a>İzleme ve uyarı
Ne zaman artırılacağına ve ne zaman azaltılacağını nasıl karar verirsiniz? Yerleşik performans izleme ve uyarı özelliklerini, sanal çekirdekleri temel alan performans değerlendirmeleriyle birlikte kullanırsınız. Bu araçları kullanarak geçerli veya projeye özgü performans ihtiyaçlarınıza göre sanal çekirdeklerin ölçeğini büyütme veya küçültme işlemlerinin etkisini hızla değerlendirebilirsiniz. Ayrıntılar için bkz. [Uyarılar](howto-alert-on-metric.md).

## <a name="keep-your-app-and-business-running"></a>Uygulamanızın ve işinizin hiç kesintiye uğramamasını sağlayın
Azure'ın Microsoft yönetimindeki veri merkezlerinin küresel bir ağı tarafından desteklenen ve endüstri lideri niteliğinde %99,99'luk bir kullanılabilirlik oranı sunan hizmet düzeyi sözleşmesi (SLA), uygulamanızın 7/24 kesintiye uğramamasına yardımcı olur. MySQL için Azure veritabanı, yerleşik güvenlik, hataya dayanıklılık ve satın alın veya tasarlamak, oluşturmak ve yönetmek için Aksi durumda olması gereken veri koruma yararlanırsınız. MySQL için Azure veritabanı ile zaman içinde nokta geri yükleme, bir sunucu a 35 güne kadar önceki bir duruma kurtarmak için kullanabilirsiniz.

## <a name="secure-your-data"></a>Verilerinizin güvenliğini sağlama
Azure veritabanı hizmetlerinin veri güvenliği, MySQL için Azure veritabanı anlayışına sahip, erişimi sınırlayan, bekleyen veri ve Hareket halindeki korumak ve yardımcı olan özelliklerle etkinliğini izleyin. Azure'ın platform güvenliği hakkında bilgi edinmek için [Azure Güven Merkezi](https://www.microsoft.com/en-us/trustcenter/security)'ni ziyaret edin.

MySQL hizmeti için Azure veritabanı, bekleyen veri için depolama şifrelemesi kullanır ve FIPS 140-2 uyumludur. Yedekler de dahil olmak üzere veriler diskte şifrelenir (altyapı tarafından sorguları çalıştırırken oluşturulan geçici dosyalar hariç). Hizmet, Azure depolama şifrelemesi kapsamında yer alan AES 256-bit şifrelemesini kullanır ve anahtarlar sistem tarafından yönetilir. Depolama şifrelemesi her zaman açıktır ve devre dışı bırakılamaz.

Varsayılan olarak, MySQL hizmeti için Azure veritabanı gerektirecek şekilde yapılandırılmış [SSL bağlantı güvenliğini](./concepts-ssl-connection-security.md) halindeki ağ üzerinden veri için. Veritabanı sunucunuzla istemci uygulamalarınız arasında SSL bağlantılarının zorunlu tutulması, sunucuya uygulamanız arasındaki veri akışını şifreleyerek "bağlantıyı izinsiz izleme" saldırılarına karşı korumaya yardımcı olur. İsteğe bağlı olarak, istemci uygulamanızın SSL bağlantısını desteklememesi durumunda veritabanı hizmetinize bağlanmak için SSL zorunluluğunu devre dışı bırakabilirsiniz.

## <a name="contacts"></a>Kişiler
Azure veritabanı'na için MySQL ekibine bir e-posta herhangi bir sorunuz veya MySQL için Azure veritabanı ile çalışma hakkında olabilir öneriler göndermek için ([ @Ask MySQL için Azure DB](mailto:AskAzureDBforMySQL@service.microsoft.com)). Bunun bir teknik destek diğer adı olmadığını unutmayın.

Buna ek olarak, aşağıdaki iletişim noktalarını uygun şekilde göz önünde bulundurun:

- Azure Desteği ile iletişim kurmak için [Azure portaldan bir bilet oluşturun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Hesabınızla ilgili bir sorun gidermek için Azure portalda bir [destek isteği](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) oluşturun.
- Görüş bildirmek veya yeni özellikler istemek için [UserVoice](https://feedback.azure.com/forums/597982-azure-database-for-mysql) aracılığıyla bir giriş oluşturun.

## <a name="next-steps"></a>Sonraki adımlar
MySQL için Azure veritabanı tanıtımını okuyun ve "Ne olduğu Azure veritabanı için MySQL?" sorusunu yanıtladığınıza göre şunları yapmaya hazırsınız:

- Maliyet karşılaştırmaları ve hesaplayıcıları için fiyatlandırma sayfasına bakın. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/mysql/)
- İlk sunucunuzu oluşturarak başlayın. [Azure portalını kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](quickstart-create-mysql-server-database-using-azure-portal.md)
- Tercih ettiğiniz dili kullanarak ilk uygulamanızı oluşturun: [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md) | [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [Go](./connect-go.md)
