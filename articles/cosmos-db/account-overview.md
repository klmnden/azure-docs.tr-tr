---
title: Azure Cosmos DB hesapları ile çalışma
description: Bu makalede oluşturma ve Azure Cosmos DB hesapları kullanın
author: rimman
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 03/31/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: da55807d4ca803adf63a1dd2dfe3ce3794cdd509
ms.sourcegitcommit: 09bb15a76ceaad58517c8fa3b53e1d8fec5f3db7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58762608"
---
# <a name="work-with-azure-cosmos-account"></a>Azure Cosmos hesabıyla çalışma

Azure Cosmos DB, bir tam olarak yönetilen platformu-bir hizmet olarak (PaaS) ' dir. Azure Cosmos DB'yi kullanmaya başlamak için başlangıçta, Azure aboneliğinizde bir Azure Cosmos hesabı oluşturmanız gerekir. Azure Cosmos hesabınıza benzersiz bir DNS adı içerir ve bir hesabı, Azure portalı, Azure CLI kullanarak veya farklı dile özgü SDK'ları kullanarak yönetebilir. Daha fazla bilgi için [Azure Cosmos hesabınızı yönetmek nasıl](how-to-manage-database-account.md).

Azure Cosmos hesabı genel dağıtım ve yüksek kullanılabilirlik temel birimidir. Genel veriler ve aktarım hızı Azure bölgelerinde dağıtmaktan, ekleyin ve Azure bölgeleri herhangi bir zamanda Azure Cosmos hesabınızı kaldırın. Tek veya birden çok yazma bölgeleri için Azure Cosmos hesabınızı yapılandırabilirsiniz. Daha fazla bilgi için [Azure Cosmos hesabınıza Azure bölgeleri ekleyip nasıl](how-to-manage-database-account.md). Yapılandırabileceğiniz [varsayılan tutarlılık](consistency-levels.md) Azure Cosmos hesap düzeyi. Azure Cosmos DB, aktarım hızı, düşük gecikme süresi 99. yüzdebirlik dilimde, tutarlılık ve yüksek kullanılabilirlik kapsayan kapsamlı SLA'lar sağlar. Daha fazla bilgi için [Azure Cosmos DB SLA'ları](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/).

Azure Cosmos hesabınızdaki tüm verileri erişimi güvenli bir şekilde yönetmek için kullanabileceğiniz [ana anahtarları](secure-access-to-data.md) hesabınızla ilişkilendirilmiş. Daha fazla verilerinize erişim güvenliğini sağlamak için yapılandırabileceğiniz bir [sanal ağ hizmet uç noktası](vnet-service-endpoint.md) ve [IP Güvenlik Duvarı](firewall-support.md) Azure Cosmos hesabınızda. 

## <a name="elements-in-an-azure-cosmos-account"></a>Bir Azure Cosmos hesabındaki öğeleri

Azure Cosmos DB kapsayıcısı ölçeklenebilirlik temel birimidir. Bir kapsayıcı neredeyse bir sınırsız sağlanan aktarım hızına (RU/sn) ve depolama olabilir. Azure Cosmos DB, şeffaf bir şekilde, sağlanan aktarım hızını ve depolamayı esnek bir şekilde ölçeklendirmek için belirttiğiniz mantıksal bölüm anahtarı kullanarak kapsayıcınızı bölümler. Daha fazla bilgi için [Azure Cosmos kapsayıcıları ve öğeleri ile çalışma](databases-containers-items.md).

Şu anda en fazla 100 Azure Cosmos hesapları bir Azure aboneliği altında oluşturabilirsiniz. Tek bir Azure Cosmos hesap neredeyse sınırsız miktarda veri ve sağlanan aktarım hızı yönetebilirsiniz. Veri ve sağlanan aktarım hızı yönetmek için hesabınız kapsamında ve veritabanı içinde bir veya daha fazla Azure Cosmos veritabanı oluşturabilirsiniz, veya daha fazla kapsayıcı oluşturabilirsiniz. Aşağıdaki resimde, bir Azure Cosmos hesabında öğeleri hiyerarşisini gösterir:

![Bir Azure Cosmos hesabı hiyerarşisi](./media/account-overview/hierarchy.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos hesabınızı ve diğer kavramlar yönetmeyi öğrenin:

* [Nasıl yapılır, Azure Cosmos hesabınızı yönetme](how-to-manage-database-account.md)
* [Genel dağıtım](distribute-data-globally.md)
* [Tutarlılık düzeyleri](consistency-levels.md)
* [Azure Cosmos kapsayıcıları ve öğeleri ile çalışma](databases-containers-items.md)
* [Sanal ağ hizmet uç noktası Azure Cosmos hesabınız için](vnet-service-endpoint.md)
* [Azure Cosmos hesabınız için IP Güvenlik Duvarı](firewall-support.md)
* [Nasıl yapılır, Azure Cosmos hesabınıza Azure bölgeleri ekleyip](how-to-manage-database-account.md)
* [Azure Cosmos DB SLA'lar](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
