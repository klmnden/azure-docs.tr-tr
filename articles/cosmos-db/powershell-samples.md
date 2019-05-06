---
title: Azure Cosmos DB için Azure PowerShell örnekleri
description: Azure PowerShell Örnekleri - Azure Cosmos DB hesapları oluşturmanıza ve yönetmenize yardımcı olacak betikler.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/08/2019
ms.author: mjbrown
ms.openlocfilehash: 68e845a05f4ebe2d1f25b55c00042c8925c8109e
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65069297"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure PowerShell örnekleri

Aşağıdaki tabloda, çekirdek (SQL) API Azure Cosmos DB'ye yönelik örnek Azure PowerShell betiklerinin bağlantılarını içerir.

| |  |
|---|---|
|**Azure Cosmos hesapları**||
|[Hesap oluşturma](scripts/powershell/sql/ps-account-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos SQL API hesabı oluşturur. |
|[Bir hesabı edinin](scripts/powershell/sql/ps-account-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir Azure Cosmos hesap özelliklerini alır. |
|[Bir bölge Ekle](scripts/powershell/sql/ps-account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir Azure Cosmos hesabı alıp farklı bir bölge konumlar listesine ekleyin. |
|[Yük devretme önceliğini değiştirme](scripts/powershell/sql/ps-account-failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Yük devretme önceliğini el ile yük devretme tetikleyici bir Azure Cosmos hesapla değiştirin. |
|[Etiketleri güncelleştirin](scripts/powershell/sql/ps-account-tags-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos hesabınız için etiketleri güncelleştirin. |
|[Hesap anahtarlarını alma](scripts/powershell/sql/ps-account-key-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos hesabın birincil ve ikincil anahtarları alın. |
|[Hesap anahtarlarını yeniden oluşturma](scripts/powershell/sql/ps-account-key-regenerate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos hesabın birincil ve ikincil anahtarları yeniden oluştur. |
|[Liste bağlantı dizeleri](scripts/powershell/sql/ps-account-connection-string-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos hesabın birincil ve ikincil bağlantı dizelerini alın. |
|[IP Güvenlik Duvarı oluşturma](scripts/powershell/sql/ps-account-firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir Azure Cosmos hesap için bir IP Güvenlik Duvarı oluşturun. |
|[Bir Azure Cosmos hesabını Sil](scripts/powershell/sql/ps-account-delete.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir Azure Cosmos hesabı silin. |
|**Azure Cosmos veritabanları**||
| [Veritabanı oluşturma](scripts/powershell/sql/ps-database-create.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir Azure Cosmos hesabı içinde bir veritabanı oluşturun.|
| [Paylaşılan/veritabanı düzeyinde işleme ile veritabanı oluşturma](scripts/powershell/sql/ps-database-create-shared.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Kapsayıcılarında ile paylaşılan veritabanı düzeyinde aktarım hızı ile bir Azure Cosmos veritabanı oluşturun.|
| [Tüm veritabanlarını listeleyin](scripts/powershell/sql/ps-database-list.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir Azure Cosmos hesabındaki tüm veritabanlarını listeleyin.|
| [Veritabanı Al](scripts/powershell/sql/ps-database-get.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir Azure Cosmos veritabanı özelliklerini alır.|
|**Azure Cosmos kapsayıcıları**||
| [Bir kapsayıcı oluşturma](scripts/powershell/sql/ps-container-create.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Adanmış aktarım hızı ile bir Azure Cosmos kapsayıcısı oluşturun.|
| [Paylaşılan aktarım hızı ile bir kapsayıcı oluşturma](scripts/powershell/sql/ps-container-create-shared.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Diğer kapsayıcılarla veritabanında paylaşılan aktarım hızı ile bir Azure Cosmos kapsayıcısı oluşturun.|
| [Bir kapsayıcı dizini ilke oluşturun](scripts/powershell/sql/ps-container-create-index-custom.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir Azure Cosmos kapsayıcısı olan bir özel dizine ilke oluşturun.|
| [Bir kapsayıcı dizini bir ilke oluşturun](scripts/powershell/sql/ps-container-create-index-none.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Devre dışı dizini İlkesi ile bir Azure Cosmos kapsayıcısı oluşturun.|
| [Benzersiz anahtarlar ve TTL ile bir kapsayıcı oluşturun.](scripts/powershell/sql/ps-container-create-unique-key-ttl.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Kapsayıcı benzersiz bir anahtar kısıtlaması ile ve yaşam süresi yapılandırılmış bir Azure Cosmos oluşturun.|
| [Çakışma çözümü ile bir kapsayıcı oluşturma](scripts/powershell/sql/ps-container-create-conflict-policy.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Yazıcı son WINS ile bir Azure Cosmos kapsayıcısı oluşturma çakışma çözüm ilkesi.|
| [Tüm kapsayıcıları listesi](scripts/powershell/sql/ps-container-list.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir Azure Cosmos veritabanı, tüm kapsayıcıları listesi.|
| [Bir kapsayıcı Al](scripts/powershell/sql/ps-container-get.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir Azure Cosmos veritabanı, bir kapsayıcı için özelliklerini alır.|
| [Kapsayıcı silme](scripts/powershell/sql/ps-container-delete.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir Azure Cosmos veritabanı bir kapsayıcıda silin.|
|||