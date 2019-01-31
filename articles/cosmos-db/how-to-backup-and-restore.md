---
title: Azure Cosmos DB verileri bir yedekten geri yükleme
description: Bu makalede Azure Cosmos DB verileri bir yedekten geri yükleme, verileri geri yüklemek için Azure desteğine başvurma adımları verileri geri yüklendikten sonra gerçekleştirilecek.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: 7f99b6d2f6fc1c6d1c270bd66965d978749ac63f
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55455941"
---
# <a name="restore-data-from-a-backup-in-azure-cosmos-db"></a>Verileri Azure Cosmos DB'de bir yedekten geri yükleyin 

Veritabanınız veya bir kapsayıcı kaza ile silerseniz, şunları yapabilirsiniz [bir destek bileti]( https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) veya [Azure destek çağrısı]( https://azure.microsoft.com/support/options/) çevrimiçi otomatik yedeklemelerden veri geri yükleme. Azure desteği, seçili planlarında yalnızca gibi **standart**, ** geliştirici ve bunları yüksek planlar. Azure desteği ile kullanılabilir değildir **temel** planı. Farklı destek planları hakkında bilgi edinmek için [Azure destek planları](https://azure.microsoft.com/support/plans/) sayfası. 

Yedeklemenin belirli bir anlık görüntüsüne geri yüklemek için Azure Cosmos DB verileri bu anlık görüntü için yedekleme döngüsü boyunca kullanılabilir olmasını gerektirir.

## <a name="request-a-restore"></a>İsteği bir geri yükleme

Bir geri yükleme istemeden önce aşağıdaki ayrıntıları sahip olmalıdır:

* Abonelik Kimliğini hazır olması.

* Nasıl verilerinizi yanlışlıkla silinmiş veya değiştirilen üzerinde bağlı olarak, ek bilgi sağlamak hazırlamanız gerekir. Mevcut olan bilgiler önceden geri yönlü zaman hassas bazı durumlarda verebilirliğinde olabilecek en aza indirmek için olduğunuz önerilir.

* Tüm Azure Cosmos DB hesabı silinirse, silinen hesabı adını vermeniz gerekir. Silinen hesabı olarak aynı ada sahip başka bir hesap oluşturursanız, seçilecek doğru hesabı belirlemek için yardımcı olacağından, Destek ekibiyle paylaşın. Durumu geri yükleme klasörünün oluşturacağı karışıklığı en aza indirir çünkü dosya farklı destek biletlerini silinmiş her hesap için önerilir.

* Bir veya daha fazla veritabanı silindiğinde, Azure Cosmos veritabanı adları yanı sıra, Azure Cosmos hesabı sağlayın ve aynı ada sahip yeni bir veritabanı var olup olmadığını belirtin.

* Bir veya daha fazla kapsayıcı silinirse, Azure Cosmos hesap adı, veritabanı adları ve kapsayıcı adları sağlamanız gerekir. Ve aynı ada sahip bir kapsayıcı olup olmadığını belirtin.

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
