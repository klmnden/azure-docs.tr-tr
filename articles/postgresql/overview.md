---
title: PostgreSQL için Azure Veritabanı ilişkisel veritabanı hizmetine genel bakış
description: PostgreSQL için Azure Veritabanı ilişkisel veritabanı hizmetine genel bir bakış sağlar.
author: jonels-msft
ms.author: jonels
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 05/06/2019
ms.openlocfilehash: f4023fa84215a0319669de0d812d8306b62278e3
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "65073282"
---
# <a name="what-is-azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı nedir?
PostgreSQL için Azure veritabanı, Microsoft bulutunda geliştiriciler için oluşturulmuş ilişkisel veritabanı hizmetidir. Açık kaynak topluluk sürümünü temel alan [PostgreSQL](https://www.postgresql.org/) veritabanı altyapısı ve iki dağıtım seçeneklerinde kullanılabilir: Tek sunucu ve hiper ölçekli (Citus) (Önizleme).

## <a name="azure-database-for-postgresql---single-server"></a>PostgreSQL - tek bir sunucu için Azure veritabanı
Tek sunuculu dağıtım seçeneği sunar:

- Ek ücret ödemeden (% 99,99 SLA) ile yerleşik yüksek kullanılabilirlik
- Kapsamlı kullandıkça öde fiyatlandırması kullanılarak öngörülebilir performans
- Saniyeler içinde gerektikçe dikey ölçeklendirme
- İzleme ve hızlı bir şekilde ölçeklendirme etkisini değerlendirmek için uyarı
- Bekleyen ve hareket halindeki hassas verileri korumaya yönelik güvenlik
- Otomatik yedeklemeler ve 35 güne kadar belirli bir noktaya geri yükleme
- Kurumsal düzeyde güvenlik ve uyumluluk

Tüm bu özellikler neredeyse hiç yönetim gerektirmez ve tümüyle ek ücret ödemeden sağlanır. Hızlı uygulama geliştirmeye ve piyasaya sunma sürenizi kısaltmaya, yerine değerli zamanınızı ve kaynaklarınızı sanal makineleri ve altyapıyı yönetmek için harcama odaklanmanıza olanak sağlar. Yeni beceriler edinmek zorunda kalmadan açık kaynak araçları ve tercih ettiğiniz platform ile geliştirmeye devam edebilirsiniz.

Tek sunuculu dağıtım seçeneği, üç fiyatlandırma katmanı sunar: Temel, genel amaçlı ve bellek için iyileştirilmiş. Her katman veritabanı iş yükünüzü desteklemek için farklı kaynak özellikleri sunar. İlk uygulamanızı aylık birkaç dolar ücretle küçük bir veritabanı üzerinde oluşturabilir ve sonra çözümünüzün gereksinimlerine göre ölçeği ayarlayabilirsiniz. Dinamik ölçeklendirebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine saydam bir şekilde yanıt verebilmesini sağlar. Yalnızca ihtiyacınız olan kaynaklar için ve yalnızca bunlara ihtiyacınız olduğunda ödeme yaparsınız. Bkz: [fiyatlandırma katmanları](concepts-pricing-tiers.md) Ayrıntılar için.

## <a name="azure-database-for-postgresql---hyperscale-citus-preview"></a>PostgreSQL - hiper ölçekli (Citus) (Önizleme) için Azure veritabanı
Hiper ölçekli (Citus) seçeneğini sorguları parçalama kullanarak birden fazla makine arasında yatay olarak ölçeklendirir. Sorgu motoru gelen SQL sorguları daha hızlı yanıt büyük veri kümelerinde bu sunucular arasında parallelizes. Bu daha yüksek ölçek ve performans gerektiren uygulamalar, genellikle yaklaştığı--veya zaten aşan--100 GB veri iş yükleri sunar.

Hiper ölçekli (Citus) dağıtım seçeneği sunar:

- Yatay parçalama kullanarak birden fazla makine arasında ölçeklendirme
- Büyük veri kümelerinde hızlı yanıtlar için bu sunucular arasında sorgu paralelleştirme
- Çok kiracılı uygulamaları, gerçek zamanlı işlem analizi ve yüksek aktarım hızı işlem iş yükleri için mükemmel destek

PostgreSQL, Hiper ölçekli (Citus) üzerinde dağıtılmış sorgular çalıştırabilirsiniz için standart ile oluşturulan uygulamalar [bağlantı kitaplıkları](./concepts-connection-libraries.md) ve küçük değişiklikler.

Hiper ölçekli (Citus) genel Önizleme aşamasındadır ve bu nedenle bir SLA'sı henüz sunmaz unutmayın.

## <a name="data-security"></a>Veri güvenliği
PostgreSQL için Azure veritabanı, Azure veritabanı hizmetlerinde yılda veri güvenliği anlayışına. Erişimi sınırlayan, bekleyen veri ve Hareket halindeki koruyan ve etkinlikleri izlemenize yardımcı olan özellikler var. Azure'ın platform güvenliği hakkında bilgi edinmek için [Azure Güven Merkezi](https://azure.microsoft.com/overview/trusted-cloud/)'ni ziyaret edin.

Hizmet PostgreSQL için Azure veritabanı, bekleyen veri için depolama şifrelemesi kullanır ve FIPS 140-2 uyumludur. Yedeklemeler gibi veriler diskte şifrelenir. Hizmet, Azure depolama şifrelemesi kapsamında yer alan AES 256-bit şifrelemesini kullanır ve anahtarlar sistem tarafından yönetilir. Depolama şifrelemesi her zaman açıktır ve devre dışı bırakılamaz. Varsayılan olarak, hizmet PostgreSQL için Azure veritabanı, güvenli bağlantılar hem ağ üzerinden hem de veritabanı ve istemci uygulaması arasında halindeki için verileri gerektirir.

## <a name="contacts"></a>Kişiler
Herhangi bir sorunuz veya PostgreSQL için Azure veritabanı ile çalışma hakkında daha fazla öneri için bir e-posta PostgreSQL takım için Azure veritabanı'na gönderin. ([ @Ask PostgreSQL için Azure DB](mailto:AskAzureDBforPostgreSQL@service.microsoft.com)). Bu, destek biletlerini yerine genel sorular için adresidir.

Ayrıca, uygun şekilde iletişim şu noktaları göz önünde bulundurun:
- Azure desteği ile iletişime geçin veya hesabınız ile bir sorunu düzeltmek için [Azure portalından bileti](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Görüş bildirmek veya yeni özellikler istemek için [UserVoice](https://feedback.azure.com/forums/597976-azure-database-for-postgresql) aracılığıyla bir giriş oluşturun.

## <a name="next-steps"></a>Sonraki adımlar
- Maliyet karşılaştırmaları ve hesaplayıcıları için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/postgresql/) bakın.
- PostgreSQL için Azure veritabanı sunucunuza ilk oluşturarak başlayın [tek sunucu](./quickstart-create-server-database-portal.md) veya [hiper ölçekli (Citus) (Önizleme)](./quickstart-create-hyperscale-portal.md)
- Python, PHP, Ruby, C ilk uygulamanızı oluşturun\#, Java, Node.js: [Bağlantı kitaplıkları](./concepts-connection-libraries.md)
