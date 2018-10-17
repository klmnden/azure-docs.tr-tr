---
title: MariaDB için Azure Veritabanı ilişkisel veritabanı hizmetine genel bakış
description: MariaDB için Azure Veritabanı ilişkisel veritabanı hizmetine genel bakış.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: overview
ms.date: 09/24/2018
ms.custom: mvc
ms.openlocfilehash: 3649a173d5707179ca8547a8169b7d308c4f7f1c
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48249171"
---
# <a name="what-is-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanı nedir?
MariaDB için Azure Veritabanı, [MariaDB topluluk sürümü](https://mariadb.org/download/) veritabanı altyapısını temel alan, Microsoft bulutunda bulunan ilişkisel bir veritabanı hizmetidir. Bu hizmet genel önizleme aşamasındadır. MariaDB için Azure Veritabanı şunları sağlar:

- Ek ücret ödemeden yerleşik yüksek kullanılabilirlik.
- Kapsamlı kullandıkça öde fiyatlandırması kullanılarak öngörülebilir performans.
- Saniyeler içinde gerektiği gibi ölçeklendirin.
- Bekleyen ve hareket halindeki hassas verileri korumaya yönelik güvenlik.
- Otomatik yedeklemeler ve 35 güne kadar belirli bir noktaya geri yükleme.
- Kurumsal düzeyde güvenlik ve uyumluluk.

Bu özellikler neredeyse hiç yönetim gerektirmez ve tümüyle ek ücret ödemeden sağlanır. Bunlar sayesinde değerli zamanınızı ve kaynaklarınızı sanal makine ve altyapı yönetimi yerine hızlı uygulama geliştirmeye ve piyasaya sunma sürenizi kısaltmaya ayırabilirsiniz. Buna ek olarak, kendi seçtiğiniz açık kaynak araçları ve platformuyla uygulamanızı geliştirmeye devam edebilir, böylelikle yeni beceriler edinmek zorunda kalmadan işlerinizin gerektirdiği hızla ve verimlilikle kullanıma sunabilirsiniz.

Bu makale performans, ölçeklenebilirlik ve yönetilebilirlik ile ilgili temel MariaDB için Azure Veritabanı kavramlarına ve özelliklerine bir giriş niteliğindedir ve ayrıntıları inceleyebilmeniz için ilgili bağlantılarla desteklenmiştir. Başlamanıza yardımcı olacak şu hızlı başlangıçlara bakın:
- [Azure portalını kullanarak MariaDB için Azure Veritabanı sunucusu oluşturma](quickstart-create-mariadb-server-database-using-azure-portal.md)
- [Azure CLI kullanarak MariaDB için Azure Veritabanı sunucusu oluşturma](quickstart-create-mariadb-server-database-using-azure-cli.md)

<!--
For a set of Azure CLI samples, see:
- [Azure CLI samples for Azure Database for MariaDB](sample-scripts-azure-cli.md) 
-->

## <a name="adjust-performance-and-scale-within-seconds"></a>Saniyeler içinde performansı ve ölçeği ayarlama
Önizlemede, MariaDB için Azure Veritabanı hizmeti çeşitli hizmet katmanları sunar: Temel, Genel Amaçlı ve Bellek için İyileştirilmiş. Her katman, hafiften ağıra kadar tüm iş yüklerini desteklemek üzere farklı performans ve özellikler getirir. İlk uygulamanızı aylık birkaç dolar ücretle küçük bir veritabanı üzerinde oluşturabilir ve sonra çözümünüzün gereksinimlerine göre ölçeği ayarlayabilirsiniz. Dinamik ölçeklendirebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine saydam bir şekilde yanıt verebilmesini sağlar. Yalnızca ihtiyacınız olan kaynaklar için ve yalnızca bunlara ihtiyacınız olduğunda ödeme yaparsınız. Ayrıntılar için bkz. [Fiyatlandırma katmanları](concepts-pricing-tiers.md).

## <a name="monitoring-and-alerting"></a>İzleme ve uyarı
Ne zaman artırılacağına ve ne zaman azaltılacağını nasıl karar verirsiniz? Yerleşik performans izleme ve uyarı özelliklerini, sanal çekirdekleri temel alan performans değerlendirmeleriyle birlikte kullanırsınız. Bu araçları kullanarak geçerli veya projeye özgü performans ihtiyaçlarınıza göre sanal çekirdeklerin ölçeğini büyütme veya küçültme işlemlerinin etkisini hızla değerlendirebilirsiniz. <!--See [Alerts](howto-alert-on-metric.md) for details.-->

## <a name="keep-your-app-and-business-running"></a>Uygulamanızın ve işinizin hiç kesintiye uğramamasını sağlayın
Azure'ın Microsoft yönetimindeki veri merkezlerinin küresel bir ağı tarafından desteklenen ve endüstri lideri niteliğinde %99,99'luk bir kullanılabilirlik oranı sunan hizmet düzeyi sözleşmesi (SLA) (genel önizleme sırasında sunulmaz), uygulamanızın 7/24 kesintiye uğramamasına yardımcı olur. MariaDB için Azure Veritabanı sunucuları sayesinde satın almanız, tasarlamanız, oluşturmanız ve yönetmeniz gerekmeksizin; yerleşik güvenlik, hataya dayanıklılık ve veri koruması olanaklarından yararlanırsınız. MariaDB için Azure Veritabanı ile, sunucuyu daha önceki bir durumuna (35 güne kadar) döndürüp kurtarmak için belirli bir noktaya geri yükleme işlemini gerçekleştirebilirsiniz.

## <a name="secure-your-data"></a>Verilerinizin güvenliğini sağlama
Azure veritabanı hizmetlerinin veri güvenliği geleneği, MariaDB için Azure Veritabanı'nda da erişimi sınırlayan, bekleyen ve hareket halindeki verileri koruyan ve etkinliği izlemenize yardımcı olan özelliklerle sürdürülür. Azure'ın platform güvenliği hakkında bilgi edinmek için [Azure Güven Merkezi](https://www.microsoft.com/en-us/trustcenter/security)'ni ziyaret edin.

MariaDB için Azure Veritabanı hizmeti bekleyen verilerde depolama şifrelemesini kullanır. Yedekler de dahil olmak üzere veriler diskte şifrelenir (altyapı tarafından sorguları çalıştırırken oluşturulan geçici dosyalar hariç). Hizmet, Azure depolama şifrelemesi kapsamında yer alan AES 256-bit şifrelemesini kullanır ve anahtarlar sistem tarafından yönetilir. Depolama şifrelemesi her zaman açıktır ve devre dışı bırakılamaz.

Varsayılan olarak, MariaDB için Azure Veritabanı hizmeti ağ genelinde hareket halindeki veriler için [SSL bağlantı güvenliğini](./concepts-ssl-connection-security.md) zorunlu tutacak şekilde yapılandırılmıştır. Veritabanı sunucunuzla istemci uygulamalarınız arasında SSL bağlantılarının zorunlu tutulması, sunucuya uygulamanız arasındaki veri akışını şifreleyerek "bağlantıyı izinsiz izleme" saldırılarına karşı korumaya yardımcı olur. İsteğe bağlı olarak, istemci uygulamanızın SSL bağlantısını desteklememesi durumunda veritabanı hizmetinize bağlanmak için SSL zorunluluğunu devre dışı bırakabilirsiniz.

## <a name="contacts"></a>Kişiler
MariaDB için Azure Veritabanı’yla çalışma hakkında sorularınız veya önerileriniz varsa, MariaDB için Azure Veritabanı Ekibine bir e-posta gönderin ([@Ask MariaDB için Azure Veritabanı](mailto:AskAzureDBforMariaDB@service.microsoft.com)). Bunun bir teknik destek diğer adı olmadığını unutmayın.

Buna ek olarak, aşağıdaki iletişim noktalarını uygun şekilde göz önünde bulundurun:
- Azure Desteği ile iletişim kurmak için [Azure portaldan bir bilet oluşturun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Hesabınızla ilgili bir sorun gidermek için Azure portalda bir [destek isteği](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) oluşturun.
- Görüş bildirmek veya yeni özellikler istemek için [UserVoice](https://feedback.azure.com/forums/915439-azure-database-for-mariadb) aracılığıyla bir giriş oluşturun.

## <a name="next-steps"></a>Sonraki adımlar
MariaDB için Azure Veritabanı'na giriş niteliğindeki makaleyi okuduğunuza ve "MariaDB için Azure Veritabanı Nedir?" sorusunu yanıtladığınıza göre şunları yapmaya hazırsınız:
- Maliyet karşılaştırmaları ve hesaplayıcıları için fiyatlandırma sayfasına bakın. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/mariadb/)
- İlk sunucunuzu oluşturarak başlayın. [Azure portalını kullanarak MariaDB için Azure Veritabanı sunucusu oluşturma](quickstart-create-mariadb-server-database-using-azure-portal.md)

<!--- - Build your first app using your preferred language: [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md) | [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [Go](./connect-go.md) --->
