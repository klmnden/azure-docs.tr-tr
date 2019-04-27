---
title: MariaDB için Azure Veritabanı ilişkisel veritabanı hizmetine genel bakış
description: MariaDB için Azure Veritabanı ilişkisel veritabanı hizmetine genel bakış.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: overview
ms.custom: mvc
ms.date: 03/20/2019
ms.openlocfilehash: a5d00c24531099e66afcb6ccf07cfdf99abd28d1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60846251"
---
# <a name="what-is-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanı nedir?

MariaDB için Azure Veritabanı, Microsoft bulutunda sunulan bir ilişkisel veritabanı hizmetidir. MariaDB için Azure veritabanı temel [MariaDB community edition](https://mariadb.org/download/) (GPLv2 lisansı altında kullanılabilir) veritabanı altyapısı, sürüm 10.2.

MariaDB için Azure Veritabanı şunları sağlar:

- Ek ücret ödemeden yerleşik yüksek kullanılabilirlik.
- Kapsamlı kullandıkça öde fiyatlandırması kullanılarak öngörülebilir performans.
- Saniyeler içinde gerektiği gibi ölçeklendirin.
- Bekleyen ve hareket halindeki hassas verileri korumaya yönelik güvenlik.
- Otomatik yedeklemeler ve 35 güne kadar belirli bir noktaya geri yükleme.
- Kurumsal düzeyde güvenlik ve uyumluluk.

Bu özelliklerin yönetilmesine neredeyse hiç gerek yoktur. Bu özellikler ücretsiz sunulur. MariaDB için Azure Veritabanı, uygulamanızı hızla geliştirmenize ve pazara ulaşma sürenizi kısaltmanıza yardımcı olabilir. Değerli vaktinizi ve kaynaklarınızı sanal makine ve altyapı yönetimine ayırmanıza gerek yoktur. Ayrıca dilediğiniz açık kaynak araçlarını ve platformu kullanarak uygulamanızı geliştirmeye devam edebilirsiniz. Yeni beceri edinmeye gerek kalmadan işletmenizin ihtiyaç duyduğu hız ve verimlilikle çalışın.

Performans, ölçeklenebilirlik ve yönetilebilirlik dahil olmak üzere MariaDB için Azure Veritabanı'nın temel kavramları ve özellikleri hakkında daha fazla bilgi edinmek için şu hızlı başlangıçlara bakın:

- [Azure portalı kullanarak MariaDB için Azure Veritabanı sunucusu oluşturma](quickstart-create-mariadb-server-database-using-azure-portal.md)
- [Azure CLI kullanarak MariaDB için Azure Veritabanı sunucusu oluşturma](quickstart-create-mariadb-server-database-using-azure-cli.md)

<!--
For a set of Azure CLI samples, see:
- [Azure CLI samples for Azure Database for MariaDB](sample-scripts-azure-cli.md) 
-->

## <a name="adjust-performance-and-scale-within-seconds"></a>Saniyeler içinde performansı ve ölçeği ayarlama

MariaDB hizmeti için Azure veritabanı, birkaç hizmet katmanı sunar: Temel, genel amaçlı ve bellek için iyileştirilmiş. Her katman, hafiften ağıra kadar tüm iş yüklerini desteklemek üzere farklı performans ve özellikler getirir. İlk uygulamanızı aylık birkaç dolar ücretle küçük bir veritabanı üzerinde oluşturabilir ve sonra çözümünüzün gereksinimlerine göre ölçeği ayarlayabilirsiniz. Dinamik ölçeklendirebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine saydam bir şekilde yanıt verebilmesine yardımcı olur. Yalnızca ihtiyacınız olan kaynaklar için ve yalnızca bunlara ihtiyacınız olduğunda ödeme yaparsınız. Bkz: [fiyatlandırma katmanları](concepts-pricing-tiers.md) Ayrıntılar için.

## <a name="monitoring-and-alerting"></a>İzleme ve uyarı

Ölçeğin ne zaman artırılacağına veya azaltılacağına nasıl karar verirsiniz? MariaDB için Azure Veritabanı'nın yerleşik performans izleme ve uyarı özelliklerini, sanal çekirdekleri temel alan performans değerlendirmeleriyle birlikte kullanabilirsiniz. Bu araçları kullanarak geçerli veya projeye özgü performans ihtiyaçlarınıza göre sanal çekirdeklerin ölçeğini büyütme veya küçültme işlemlerinin etkisini hızla değerlendirebilirsiniz. Ayrıntılar için bkz. [Uyarılar](howto-alert-metric.md).

## <a name="keep-your-app-and-business-running"></a>Uygulamanızın ve işinizin hiç kesintiye uğramamasını sağlayın

Azure'nın sektörde lider % 99,99 kullanılabilirlik SLA'sı, Microsoft tarafından yönetilen veri merkezlerinden oluşan global bir ağda tarafından desteklenir. Ağ, uygulamanızın 7/24 çalışmasına yardımcı olur. MariaDB için Azure Veritabanı'ndaki yerleşik güvenlik, hataya dayanıklılık ve veri koruma özelliklerinden faydalanabilirsiniz. MariaDB için Azure Veritabanı ile, sunucuyu daha önceki bir durumuna (35 güne kadar) döndürüp kurtarmak için belirli bir noktaya geri yükleme işlemini gerçekleştirebilirsiniz.

## <a name="secure-your-data"></a>Verilerinizin güvenliğini sağlama

Azure veritabanı hizmetlerinin sahip olduğu veri güvenliği anlayışı, MariaDB için Azure Veritabanı'nda da devam etmektedir. MariaDB için Azure Veritabanı bekleyen ve hareket halindeki verileri koruyan, erişimi sınırlandıran ve etkinliği izlemenize yardımcı olan özellikler sunmaktadır. Azure'ın platform güvenliği hakkında bilgi edinmek için [Azure Güven Merkezi](https://www.microsoft.com/en-us/trustcenter/security)'ni ziyaret edin.

MariaDB hizmeti için Azure veritabanı, bekleyen veri için depolama şifrelemesi kullanır ve FIPS 140-2 uyumludur. Yedeklenen veriler dahil olmak üzere disk üzerindeki tüm veriler şifrelenir. (Sorgu çalıştırıldığında altyapı tarafından oluşturulan geçici dosyalar, diskte şifrelenmez.) Hizmet, Azure Depolama şifrelemesi kapsamında yer alan AES 256 bit şifrelemesini kullanır. Anahtarlar sistem tarafından yönetilir. Depolama şifrelemesi her zaman açıktır ve devre dışı bırakılamaz.

Varsayılan olarak, MariaDB için Azure Veritabanı hizmeti ağ genelinde hareket halindeki veriler için [SSL bağlantı güvenliğini](./concepts-ssl-connection-security.md) zorunlu tutacak şekilde yapılandırılmıştır. Veritabanı sunucunuzla istemci uygulamalarınız arasında SSL bağlantılarının zorunlu tutulması, sunucuya uygulamanız arasındaki veri akışını şifreleyerek "bağlantıyı izinsiz izleme" saldırılarına karşı korumaya yardımcı olur. İsteğe bağlı olarak, istemci uygulamanızın SSL bağlantısını desteklememesi durumunda veritabanı hizmetinize bağlanmak için SSL gereksinimini devre dışı bırakabilirsiniz.

## <a name="contacts"></a>Kişiler

MariaDB için Azure Veritabanı’yla çalışma hakkındaki sorularınızı veya önerilerinizi [MariaDB için Azure Veritabanı Ekibine](mailto:AskAzureDBforMariaDB@service.microsoft.com) (teknik destek diğer adı değildir) gönderebilirsiniz.

Ayrıca aşağıdaki iletişim noktalarını da kullanabilirsiniz:
- Azure Desteğine başvurmak için Azure portalda [bir destek isteği açın](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Hesabınızla ilgili bir sorun gidermek için Azure portalda bir [destek isteği açın](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
- Görüş bildirmek veya yeni özellik isteği göndermek için [Azure Geri Bildirim Forumlarında](https://feedback.azure.com/forums/915439-azure-database-for-mariadb) bir giriş oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

MariaDB için Azure Veritabanı hakkında giriş bilgilerini incelediğinize göre şu adımlar için hazırsınız:
- Maliyet karşılaştırmaları ve hesaplayıcıları için [fiyatlandırma](https://azure.microsoft.com/pricing/details/mariadb/) sayfasına bakın. 
- [İlk sunucunuzu oluşturarak](quickstart-create-mariadb-server-database-using-azure-portal.md) başlayın.

<!--- - Build your first app using your preferred language: [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md) | [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [Go](./connect-go.md) --->
