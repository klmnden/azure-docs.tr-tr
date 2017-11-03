---
title: "MySQL ilişkisel veritabanı hizmeti için Azure veritabanı'nın genel bakış | Microsoft Docs"
description: "Azure veritabanı MySQL ilişkisel veritabanı hizmeti için genel bakış."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 10/20/2017
ms.custom: mvc
ms.openlocfilehash: 2a9efdd9285dfa5fca450ede64e5f6ee54cbc72b
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2017
---
# <a name="what-is-azure-database-for-mysql"></a>Azure veritabanı için MySQL nedir?
Azure için MySQL veritabanıdır göre Microsoft bulut ilişkisel veritabanı hizmeti [MySQL Community Edition](https://www.mysql.com/products/community/) veritabanı altyapısı. Genel önizlemede hizmetidir. Azure veritabanı için MySQL sunar:

- Yerleşik yüksek kullanılabilirlik ek ücret ödemeden ile.
- Tahmin edilebilir performans, kapsayıcı Kullandıkça Öde fiyatlandırma kullanma.
- Anında saniye içinde ölçeklendirin.
- Çalışmıyorken hassas verileri ve hareket halinde korumak için güvenli.
- Otomatik yedekleme ve noktası-içinde--geri yükleme 35 güne kadar.
- Kurumsal düzeyde güvenlik ve uyumluluk.

Bu özellikler neredeyse hiçbir yönetim gerektirir ve tüm hiçbir ek ücret sağlanır. Hızlı Uygulama geliştirme ve pazara zamanınızı hızlandırmaya yerine değerli zaman ve sanal makineleri ve altyapıyı yönetmek için kaynak ayırma odaklanmanıza olanak sağlar. Ayrıca, uygulamanızı hızını ve verimliliği ile tüm yeni yetenekler öğrenmek zorunda kalmadan, işletme taleplerine teslim etmek için seçtiğiniz platform ve açık kaynaklı araçları ile geliştirme devam edebilirsiniz.

![Azure veritabanı için MySQL kavramsal diyagramı](media/overview/1-azure-db-for-mysql-conceptual-diagram.png)

Bu makale Azure veritabanı giriş MySQL temel kavramlar ve performans, ölçeklenebilirlik ve yönetilebilirlik ayrıntıları keşfetmek için bağlantılar ile ilgili özellikler için yazılmıştır. Başlamanıza yardımcı olmak için bu quickstarts bakın:
- [Azure portalını kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](quickstart-create-mysql-server-database-using-azure-portal.md)
- [Azure CLI kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](quickstart-create-mysql-server-database-using-azure-cli.md)

Azure CLI örnekler kümesi için bkz:
- [Azure veritabanı için MySQL için Azure CLI örnekleri](sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-within-seconds"></a>Performansı ve ölçeği saniye içinde ayarlama
Önizleme'de, iki hizmet katmanları Azure veritabanı için MySQL hizmeti sunar: temel ve standart. Her katman farklı performans ve ağır veritabanı iş yükleri için basit desteklemek için özellikleri sunar. Ayda birkaç Doları küçük bir veritabanı üzerinde ilk uygulamanızı oluşturun ve sonra çözümünüzü ihtiyaçlarını karşılamak üzere ölçeği ayarlayın. Kaynak gereksinimleri hızla değişen şeffaf bir şekilde yanıt vermesi veritabanınızı dinamik ölçeklenebilirlik sağlar. Yalnızca kaynaklar için gerek ve yalnızca ücret ödersiniz gereksinim duyarsınız. Bkz: [fiyatlandırma katmanlarına](concepts-service-tiers.md) Ayrıntılar için.

## <a name="monitoring-and-alerting"></a>İzleme ve uyarı
Nasıl yukarı ve aşağı çevirmek ne zaman karar? Yerleşik performans izleme ve uyarı özellikleri, işlem birimleri temel performans değerlendirmeleri birlikte kullanın. İşlem birimleri ölçeklendirmeyi etkisini hızlı bir şekilde değerlendirmek bu Araçları'nı kullanarak veya aşağı geçerli veya tahmini performans gereksinimlerinize göre. Bkz: [uyarıları](howto-alert-on-metric.md) Ayrıntılar için.

## <a name="keep-your-app-and-business-running"></a>Uygulamanızın ve işinizin hiç kesintiye uğramamasını sağlayın
Azure'nın endüstri lideri genel bir veri merkezleri, Microsoft tarafından yönetilen ağ tarafından desteklenen % 99,99 kullanılabilirlik hizmet düzeyi sözleşmesi (SLA), uygulamanızın 7/24 çalıştıran tutmaya yardımcı olur. MySQL sunucusu için her Azure veritabanı ile yerleşik güvenlik, hata toleransı ve satın alma veya tasarlama, derleme ve yönetmek için Aksi durumda olması gereken veri koruma yararlanın. MySQL için Azure veritabanı ile bir sunucu a 35 gün kadar daha önceki bir duruma kurtarmak için zaman içinde nokta geri yükleme kullanabilirsiniz.

## <a name="secure-your-data"></a>Verilerinizin güvenliğini sağlama
Azure veritabanı hizmetleri Azure veritabanı için MySQL anlayışına veri güvenliği yılda vardır, erişimi sınırlamak, çalışmıyorken veri ve hareket halinde korumak ve yardımcı özelliklerle etkinliğini izleyin. Ziyaret [Azure Güven Merkezi](https://www.microsoft.com/en-us/TrustCenter/Security/default.aspx) Azure'nın platform güvenliği hakkında bilgi için.

MySQL hizmeti için Azure veritabanına veri çalışmıyorken için depolama şifrelemesi kullanır. Yedeklemeler dahil verileri (hariç sorgu çalıştırılırken altyapısı tarafından oluşturulan geçici dosyalar) diskte şifrelenir. Azure depolama şifreleme dahil AES 256 bit şifreleme hizmeti kullanıyor ve anahtarları sistem tarafından yönetiliyor. Depolama şifreleme her zaman açıktır ve devre dışı bırakılamaz.

Varsayılan olarak, Azure veritabanı için MySQL hizmeti gerektirecek şekilde yapılandırılmış [SSL bağlantı güvenliği](./concepts-ssl-connection-security.md) veri hareket halinde ağ üzerinden için. Veritabanı sunucunuz ve istemci uygulamalarınız arasında SSL bağlantılarını zorlamayı uygulamanız ile sunucu arasındaki veri akışını şifreleyerek "ortadaki adam" saldırılarına karşı korumaya yardımcı olur.  İsteğe bağlı olarak, istemci uygulamanız SSL bağlantısını desteklemiyorsa, veritabanı hizmete bağlanmak için SSL gerektirme devre dışı bırakabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Azure veritabanı giriş MySQL için okuma ve "Ne olduğu Azure veritabanı için MySQL?" sorusunu yanıtladığınıza göre şunları yapmaya hazırsınız:
- Maliyet karşılaştırmaları ve hesaplayıcıları için fiyatlandırma sayfasını bakın. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/mysql/)
- İlk sunucunuzun oluşturarak başlayın. [Azure portalını kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](quickstart-create-mysql-server-database-using-azure-portal.md)
- Tercih ettiğiniz dili kullanarak ilk uygulamanızı oluşturma: [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md)  |  [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [gidin](./connect-go.md)
