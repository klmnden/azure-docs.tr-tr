---
title: "Azure veritabanı PostgreSQL ilişkisel veritabanı hizmeti için genel bakış | Microsoft Docs"
description: "Azure veritabanı genel bir bakış için PostgreSQL ilişkisel veritabanı hizmeti sağlar."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 10/20/2017
ms.openlocfilehash: 5b5da758e966cc5ca536d7b291be74409f02ca73
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2017
---
# <a name="what-is-azure-database-for-postgresql"></a>Azure veritabanı PostgreSQL için nedir?

Azure için PostgreSQL veritabanıdır topluluk açık kaynak sürümüne geliştiriciler için yerleşik Microsoft bulut ilişkisel veritabanı hizmeti [PostgreSQL](https://www.postgresql.org/) veritabanı altyapısı. Genel önizlemede hizmetidir. Azure veritabanı PostgreSQL için sunar:

- Yerleşik yüksek kullanılabilirlik ek ücret ödemeden ile.
- Tahmin edilebilir performans, kapsayıcı Kullandıkça Öde fiyatlandırma kullanma.
- Anında saniye içinde ölçeklendirin.
- Çalışmıyorken hassas verileri ve hareket halinde korumak için güvenli.
- Otomatik yedekleme ve noktası-içinde--geri yükleme 35 güne kadar.
- Kurumsal düzeyde güvenlik ve uyumluluk.

Bu özellikler neredeyse hiçbir yönetim gerektirir ve tüm hiçbir ek ücret sağlanır. Bu özellikler, hızlı uygulama geliştirme ve pazara zamanınızı hızlandırmaya yerine değerli zaman ve sanal makineleri ve altyapıyı yönetmek için kaynak ayırma odaklanmak olanak tanır. Ayrıca, seçtiğiniz platform ve açık kaynaklı araçları ile uygulamanızı geliştirin ve hız ve yeni yetenekler öğrenmek zorunda kalmadan işinizin talep verimliliği ile teslim etmek devam edebilirsiniz. 

Bu makale Azure veritabanı giriş PostgreSQL temel kavramlar ve performans, ölçeklenebilirlik ve yönetilebilirlik ile ilgili özellikler için yazılmıştır. Başlamanıza yardımcı olmak için bu quickstarts bakın:

- [Azure portalını kullanarak PostgreSQL için bir Azure veritabanı oluşturma](quickstart-create-server-database-portal.md)
- [Azure CLI kullanarak PostgreSQL için bir Azure veritabanı oluşturma](quickstart-create-server-database-azure-cli.md)

Azure CLI örnekler kümesi için bkz:

- [Azure veritabanı PostgreSQL için Azure CLI örnekleri](./sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-within-seconds"></a>Performansı ve ölçeği saniye içinde ayarlama
Önizleme'de, iki hizmet katmanları Azure veritabanı için MySQL hizmeti sunar: temel ve standart. Her katman farklı performans ve ağır veritabanı iş yükleri için basit desteklemek için özellikleri sunar. Ayda birkaç Doları küçük bir veritabanı üzerinde ilk uygulamanızı oluşturun ve sonra çözümünüzü ihtiyaçlarını karşılamak üzere ölçeği ayarlayın. Kaynak gereksinimleri hızla değişen şeffaf bir şekilde yanıt vermesi veritabanınızı dinamik ölçeklenebilirlik sağlar. Yalnızca kaynaklar için gerek ve yalnızca ücret ödersiniz gereksinim duyarsınız. Bkz: [fiyatlandırma katmanlarına](concepts-service-tiers.md) Ayrıntılar için.

## <a name="monitoring-and-alerting"></a>İzleme ve uyarı
Nasıl yukarı ve aşağı çevirmek ne zaman karar? Yerleşik performans izleme ve uyarı özellikleri, işlem birimleri temel performans değerlendirmeleri birlikte kullanın. İşlem birimleri ölçeklendirmeyi etkisini hızlı bir şekilde değerlendirmek bu Araçları'nı kullanarak veya aşağı geçerli veya tahmini performans gereksinimlerinize göre. Bkz: [uyarıları](howto-alert-on-metric.md) Ayrıntılar için.

## <a name="keep-your-app-and-business-running"></a>Uygulamanızın ve işinizin hiç kesintiye uğramamasını sağlayın
Azure'nın endüstri lideri genel bir veri merkezleri, Microsoft tarafından yönetilen ağ tarafından desteklenen % 99,99 kullanılabilirlik (Önizleme'de kullanılamaz) hizmet düzeyi sözleşmesi (SLA), uygulamanızın 7/24 çalıştıran tutmaya yardımcı olur. Her Azure veritabanı ile PostgreSQL sunucu için yerleşik güvenlik, hata toleransı ve satın alma veya tasarlama, derleme ve yönetmek için Aksi durumda olması gereken veri koruma yararlanın. PostgreSQL Azure veritabanıyla, her bir hizmet katmanına iş sürekliliği özellikleri ve hale getirmek için kullanabileceğiniz seçenekleri ve çalıştığından ve Kal kapsamlı bir kümesini sunar. Bir veritabanını daha önceki bir durumuna (35 güne kadar) geri döndürmek üzere[ belirli bir noktaya geri yükleme](howto-restore-server-portal.md) işlemini gerçekleştirebilirsiniz. Ayrıca, veritabanlarınızı barındıran veri merkezinde bir kesinti oluşursa, son yedeklemelerini coğrafi olarak yedekli kopyalarından veritabanlarını geri yükleyebilirsiniz.

## <a name="secure-your-data"></a>Verilerinizin güvenliğini sağlama
Azure veritabanı hizmetleri gelenek veri güvenliği PostgreSQL için Azure veritabanına erişimi sınırlamak, çalışmıyorken veri ve hareket halinde korumak ve etkinlikleri izlemenize yardımcı özelliklerle anlayışına sahiptir. Ziyaret [Azure Güven Merkezi](https://www.microsoft.com/TrustCenter/Security/default.aspx) Azure'nın platform güvenliği hakkında bilgi için.

Azure veritabanı PostgreSQL hizmeti için veri çalışmıyorken için depolama şifrelemesi kullanır. Veri yedekleri de dahil olmak üzere (hariç sorgu çalıştırılırken altyapısı tarafından oluşturulan geçici dosyalar) diskte şifrelenir. Azure depolama şifreleme dahil AES 256 bit şifreleme hizmeti kullanıyor ve anahtarları sistem tarafından yönetiliyor. Depolama şifreleme her zaman açıktır ve devre dışı bırakılamaz.

Varsayılan olarak, PostgreSQL hizmeti için Azure veritabanı gerektirecek şekilde yapılandırılmış [SSL bağlantı güvenliği](./concepts-ssl-connection-security.md) veri hareket halinde ağ üzerinden için. Veritabanı sunucunuz ve istemci uygulamalarınız arasında SSL bağlantılarını zorlamayı "ortadaki adam" saldırılarına karşı uygulamanız ile sunucu arasındaki veri akışını şifreleyerek korunmasına yardımcı.  İsteğe bağlı olarak, istemci uygulamanız SSL bağlantısını desteklemiyorsa, veritabanı hizmete bağlanmak için SSL gerektirme devre dışı bırakabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- Bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/postgresql/) maliyet karşılaştırmaları ve hesaplayıcıları için.
- İle çalışmaya başlama [PostgreSQL için ilk Azure veritabanınızı oluşturmaya](./quickstart-create-server-database-portal.md).
- Python, PHP, Ruby, C kullanarak ilk uygulamanızı oluşturma\#, Java, Node.js: [bağlantı kitaplıkları](./concepts-connection-libraries.md)
