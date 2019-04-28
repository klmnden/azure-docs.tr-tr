---
title: Azure Cosmos DB verileri bir yedekten geri yükleme
description: Bu makalede Azure Cosmos DB verileri bir yedekten geri yükleme, verileri geri yüklemek için Azure desteğine başvurma adımları verileri geri yüklendikten sonra gerçekleştirilecek.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: 1d886e146e9e18eb735e6f88d2cb2c1a4a472924
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61059647"
---
# <a name="restore-data-from-a-backup-in-azure-cosmos-db"></a>Verileri Azure Cosmos DB'de bir yedekten geri yükleyin 

Veritabanınız veya bir kapsayıcı kaza ile silerseniz, şunları yapabilirsiniz [bir destek bileti]( https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) veya [Azure destek çağrısı]( https://azure.microsoft.com/support/options/) çevrimiçi otomatik yedeklemelerden veri geri yükleme. Azure desteği, seçili planlarında yalnızca gibi **standart**, **Geliştirici**ve bunları yüksek planlar. Azure desteği ile kullanılabilir değildir **temel** planı. Farklı destek planları hakkında bilgi edinmek için [Azure destek planları](https://azure.microsoft.com/support/plans/) sayfası. 

Yedeklemenin belirli bir anlık görüntüsüne geri yüklemek için Azure Cosmos DB verileri bu anlık görüntü için yedekleme döngüsü boyunca kullanılabilir olmasını gerektirir.

## <a name="request-a-restore"></a>İsteği bir geri yükleme

Bir geri yükleme istemeden önce aşağıdaki ayrıntıları sahip olmalıdır:

* Abonelik Kimliğini hazır olması.

* Nasıl verilerinizi yanlışlıkla silinmiş veya değiştirilen üzerinde bağlı olarak, ek bilgi sağlamak hazırlamanız gerekir. Mevcut olan bilgiler önceden geri yönlü zaman hassas bazı durumlarda verebilirliğinde olabilecek en aza indirmek için olduğunuz önerilir.

* Tüm Azure Cosmos DB hesabı silinirse, silinen hesabı adını vermeniz gerekir. Silinen hesabı olarak aynı ada sahip başka bir hesap oluşturursanız, seçilecek doğru hesabı belirlemek için yardımcı olacağından, Destek ekibiyle paylaşın. Durumu geri yükleme klasörünün oluşturacağı karışıklığı en aza indirir çünkü dosya farklı destek biletlerini silinmiş her hesap için önerilir.

* Bir veya daha fazla veritabanı silindiğinde, Azure Cosmos veritabanı adları yanı sıra, Azure Cosmos hesabı sağlayın ve aynı ada sahip yeni bir veritabanı var olup olmadığını belirtin.

* Bir veya daha fazla kapsayıcı silinirse, Azure Cosmos hesap adı, veritabanı adları ve kapsayıcı adları sağlamanız gerekir. Ve aynı ada sahip bir kapsayıcı olup olmadığını belirtin.

* Yanlışlıkla silinmiş veya verilerinizi bozuk, sizinle iletişim kuralım [Azure Destek](https://azure.microsoft.com/support/options/) Azure Cosmos DB takıma yardımcı olur böylece 8 saat içinde verileri yedeklerden geri.
  
  * Veritabanı veya kapsayıcı yanlışlıkla sildiyseniz, ön. der. B veya ön. der. C Azure destek talebinde bulunun. 
  * Yanlışlıkla silinmiş veya kapsayıcı içindeki bazı belgeler bozuk, ön. der. A destek talebinde bulunun. 

Veri bozulması meydana geldiğinde ve bir kapsayıcı içindeki belgeler değiştirilmiş veya silinmiş, **kapsayıcısını olabildiğince çabuk silme**. Kapsayıcıyı silerek, Azure Cosmos DB yedeklerin üzerine yazmasını önleyebilirsiniz. Herhangi bir nedenden dolayı silme işlemini mümkün değilse, bir bilet olabildiğince çabuk dosyası. Azure Cosmos hesap adı, veritabanı adları, koleksiyon adları ek olarak, istediğiniz verileri geri yüklenebilir zaman noktası belirtmeniz gerekir. En iyi kullanılabilir yedekler o anda belirlemek yardımcı olmak mümkün olduğunca kesin olarak önemlidir. UTC saatini belirtmek önemlidir. 

Aşağıdaki ekran görüntüsünde bir container(collection/graph/table) verileri Azure portalını kullanarak geri yüklemek bir destek isteği oluşturma işlemini gösterir. İstek belirlememize yardımcı olmak için verileri silindikten sonra geri yükleme, amacı veri türü gibi ek ayrıntılar zaman sağlar.

![Azure portalını kullanarak bir yedekleme destek isteği oluşturun](./media/how-to-backup-and-restore/backup-support-request-portal.png)

## <a name="post-restore-actions"></a>Geri yükleme sonrası eylemler

Verileri geri yükledikten sonra yeni hesap adıyla ilgili bir bildirim alırsınız (genellikle bir biçiminde olan `<original-name>-restored1`) ve ne zaman hesap geri yüklendi için saat. Dizin oluşturma ilkeleri aynı sağlanan aktarım hızı, geri yüklenen hesabınız ve özgün hesabıyla aynı bölgede yer. Abonelik Yöneticisi veya bir coadmin olan bir kullanıcıyı geri yüklenen hesap görebilirsiniz.

Verileri geri yüklendikten sonra incelemek geri yüklenen hesabındaki verileri doğrulamak ve beklediğiniz sürüm içerdiğinden emin olun. Her şey iyi görünüyor, tekrar özgün kullanarak hesabınızda veri geçirmelisiniz [Azure Cosmos DB değişiklik akışı](change-feed.md) veya [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md).

Hemen veri geçirdikten sonra kapsayıcı veya veritabanı silmeniz önerilir. Geri yüklenen veritabanları veya kapsayıcıları silmezseniz, bunlar istek birimleri, depolama ve çıkış için bir ücret.

## <a name="next-steps"></a>Sonraki adımlar

Ardından aşağıdaki makaleleri kullanarak geri özgün hesabınıza veri geçirme hakkında bilgi edinebilirsiniz:

* İstek, Azure desteği'ne başvurun bir geri yükleme yapmak için [Azure portalından bileti](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
* [Kullanım Cosmos DB değişiklik akışı](change-feed.md) verilerini Azure Cosmos DB'ye taşımak için.

* [Azure Data factory'yi](../data-factory/connector-azure-cosmos-db.md) verilerini Azure Cosmos DB'ye taşımak için.
