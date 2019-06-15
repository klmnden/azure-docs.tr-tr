---
title: SQL Veritabanı Uygulaması Geliştirmeye Genel Bakış | Microsoft Belgeleri
description: SQL Veritabanına bağlanan uygulamalar için kullanılabilen bağlantı kitaplıkları ve en iyi uygulamalar hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: genemi
manager: craigg
ms.date: 02/07/2019
ms.openlocfilehash: efb6d932e616ada6b8dfff637af469c16fc2f293
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60723417"
---
# <a name="sql-database-application-development-overview"></a>SQL veritabanı uygulaması geliştirmeye genel bakış

Bu makalede, geliştiricilerin Azure SQL Veritabanı ile bağlantı kurmak üzere kod yazarken dikkat etmesi gereken noktalara yer verilmiştir. Bu makalede, Azure SQL veritabanı (tek veritabanı, elastik havuzlar, yönetilen örnek) tüm dağıtım modelleri için geçerlidir.

> [!TIP]
> Başlarken göz çalışmaya yönelik Kılavuzlar [tek veritabanları](sql-database-single-database-quickstart-guide.md) ve [yönetilen örnekleri](sql-database-managed-instance-quickstart-guide.md) Azure SQL veritabanınızı Kurulum gerekiyorsa.
>

## <a name="language-and-platform"></a>Dil ve platform

Çeşitli kullanabileceğiniz [programlama dilleri ve platformları](sql-database-connect-query.md) bağlanmak ve Azure SQL veritabanı sorgulamak için. Bulabilirsiniz [örnek uygulamalar](https://azure.microsoft.com/resources/samples/?service=sql-database&sort=0) , Azure SQL veritabanı'na bağlanmak için kullanabilirsiniz.

Açık kaynak araçlarla yararlanabileceğiniz [cheetah](https://github.com/wunderlist/cheetah), [sql-cli](https://www.npmjs.com/package/sql-cli), [VS Code](https://code.visualstudio.com/). Ayrıca, Azure SQL Veritabanı [Visual Studio](https://www.visualstudio.com/downloads/) ve [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) gibi Microsoft araçlarıyla birlikte çalışır. Azure portalı, PowerShell de kullanabilirsiniz ve REST API'leri yardımcı ek verimlilik elde edin.

## <a name="authentication"></a>Kimlik Doğrulaması

Azure SQL veritabanına erişim, oturum açma bilgileri ve güvenlik duvarları ile korunur. Azure SQL veritabanı, hem SQL Server destekler ve [Azure Active Directory (AAD) kimlik doğrulaması](sql-database-aad-authentication.md) kullanıcılar ve oturum açma bilgileri. AAD oturum açma bilgileri, yalnızca yönetilen örneği'nde kullanılabilir. 

Daha fazla bilgi edinin [veritabanında erişim ve oturum açma yönetme](sql-database-manage-logins.md).

## <a name="connections"></a>Bağlantılar

İstemci bağlantısı mantığınızda varsayılan zaman aşımını 30 saniye olacak şekilde geçersiz kılın. 15 saniyelik varsayılan değer, internet kullanan bağlantılar için çok kısadır.

[Bağlantı havuzu](https://msdn.microsoft.com/library/8xx3tyca.aspx) kullanıyorsanız programınız etkin olarak kullanmadığında ve yeniden kullanma hazırlığı yapmadığında bağlantıyı kapatın.

Uzun süre çalışan işlemler kaçının, çünkü herhangi bir altyapı ya da bağlantı hatası işlemin geri alınması. Mümkünse, birden çok daha küçük işlem işlemde bölme ve kullanma [performansını artırmak için toplu işleme](sql-database-use-batching-to-improve-performance.md).

## <a name="resiliency"></a>Dayanıklılık

Azure SQL veritabanı burada gerçekleşen geçici hataların altyapının veya Bulut varlıklarını arasındaki iletişimi beklediğiniz bir bulut hizmetidir. Azure SQL veritabanı üzerinde karşılıklı altyapı hatalara dayanıklı olsa da, bu hatalar bağlantınızı etkileyebilir. SQL veritabanına bağlanırken geçici bir hata oluştuğunda, kodunuzu gereken [çağrıyı yeniden denemesi](sql-database-connectivity-issues.md). Yeniden deneme mantığının SQL Veritabanını aynı anda yeniden deneme yapan birden fazla istemciyle boğmaması için geri alma mantığını kullanmasını öneririz. Yeniden deneme mantığı bağlıdır [SQL veritabanı istemci programları için hata iletileri](sql-database-develop-error-messages.md).

Azure SQL veritabanınızı planlı bakım olayları için hazırlanması hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı'nda Azure bakım olayları için planlama](sql-database-planned-maintenance.md).

## <a name="network-considerations"></a>Ağ konuları

- İstemci programınızı barındıran bilgisayarda güvenlik duvarının 1433 numaralı bağlantı noktasından giden TCP iletişimine izin verdiğinden emin olun.  Daha fazla bilgi: [Bir Azure SQL veritabanı güvenlik duvarını](sql-database-configure-firewall-settings.md).
- İstemciniz bir Azure sanal makine (VM) üzerinde çalışırken istemci programınız SQL veritabanına bağlanır, VM'de belirli bağlantı noktası aralıklarını açmanız gerekir. Daha fazla bilgi: [ADO.NET 4.5 ve SQL veritabanı için 1433 dışındaki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md).
- Azure SQL veritabanı istemci bağlantıları, bazen Proxy'yi atlar ve veritabanı ile doğrudan etkileşim. 1433 dışındaki bağlantı noktaları önemli hale gelmiştir. Daha fazla bilgi için [Azure SQL veritabanı bağlantı mimarisi](sql-database-connectivity-architecture.md) ve [ADO.NET 4.5 ve SQL veritabanı için 1433 dışındaki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md).
- Yönetilen örnek için ağ yapılandırma için bkz: [yönetilen örnekleri için ağ yapılandırması](sql-database-howto-managed-instance.md#network-configuration).

## <a name="next-steps"></a>Sonraki adımlar

Tüm [SQL Veritabanı özelliklerini](sql-database-technical-overview.md) keşfedin.
