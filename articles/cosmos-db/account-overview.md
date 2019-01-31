---
title: Azure Cosmos DB hesapları ile çalışma
description: Bu makalede oluşturma ve Azure Cosmos DB hesapları kullanın
author: dharmas-cosmos
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: cbc11e2fc54ecffbea22a66354f334f3562e3339
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55459222"
---
# <a name="work-with-azure-cosmos-account"></a>Azure Cosmos hesabıyla çalışma

Azure Cosmos DB, bir tam olarak yönetilen platformu-bir hizmet olarak (PaaS) ' dir. Azure Cosmos DB'yi kullanmaya başlamak için başlangıçta, Azure aboneliğinizde bir Azure Cosmos hesabı oluşturmanız gerekir. Azure Cosmos hesabınıza benzersiz bir DNS adı içerir ve bir hesabı, Azure portalı, Azure CLI kullanarak veya farklı dile özgü SDK'ları kullanarak yönetebilir. Daha fazla bilgi için [Azure Cosmos hesabınızı yönetmek nasıl](how-to-manage-database-account.md).

Azure Cosmos hesabı genel dağıtım ve yüksek kullanılabilirlik temel birimidir. Genel veriler ve aktarım hızı Azure bölgelerinde dağıtmaktan, ekleyin ve Azure bölgeleri herhangi bir zamanda Azure Cosmos hesabınızı kaldırın. Tek veya birden çok yazma bölgeleri için Azure Cosmos hesabınızı yapılandırabilirsiniz. Daha fazla bilgi için [Azure Cosmos hesabınıza Azure bölgeleri ekleyip nasıl](how-to-manage-database-account.md). Yapılandırabileceğiniz [varsayılan tutarlılık](consistency-levels.md) Azure Cosmos hesap düzeyi. Azure Cosmos DB, aktarım hızı, düşük gecikme süresi 99. yüzdebirlik dilimde, tutarlılık ve yüksek kullanılabilirlik kapsayan kapsamlı SLA'lar sağlar. Daha fazla bilgi için [Azure Cosmos DB SLA'ları](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/).

Azure Cosmos hesabınızdaki tüm verileri erişimi güvenli bir şekilde yönetmek için hesabınızla ilişkili ana anahtarları kullanabilirsiniz. Verilerinizi daha güvenli erişimi için bir sanal ağ hizmet uç noktası ve IP Güvenlik Duvarı, Azure Cosmos hesabınızda yapılandırabilirsiniz. 

## <a name="elements-in-an-azure-cosmos-account"></a>Bir Azure Cosmos hesabındaki öğeleri

Azure Cosmos DB kapsayıcısı ölçeklenebilirlik temel birimidir. Bir kapsayıcı neredeyse bir sınırsız sağlanan aktarım hızına (RU/sn) ve depolama olabilir. Azure Cosmos DB, şeffaf bir şekilde, sağlanan aktarım hızını ve depolamayı esnek bir şekilde ölçeklendirmek için belirttiğiniz mantıksal bölüm anahtarı kullanarak kapsayıcınızı bölümler. Daha fazla bilgi için [Azure Cosmos kapsayıcıları ve öğeleri ile çalışma](databases-containers-items.md).

Şu anda en fazla 100 Azure Cosmos hesapları bir Azure aboneliği altında oluşturabilirsiniz. Tek bir Azure Cosmos hesap neredeyse sınırsız miktarda veri ve sağlanan aktarım hızı yönetebilirsiniz. Veri ve sağlanan aktarım hızı yönetmek için hesabınız kapsamında ve veritabanı içinde bir veya daha fazla Azure Cosmos veritabanı oluşturabilirsiniz, veya daha fazla kapsayıcı oluşturabilirsiniz. Aşağıdaki resimde, bir Azure Cosmos hesabında öğeleri hiyerarşisini gösterir:

![Bir Azure Cosmos hesabı hiyerarşisi](./media/account-overview/hierarchy.png)

## <a name="next-steps"></a>Sonraki adımlar

Şimdi Azure Cosmos hesabınızı yönetin veya Azure Cosmos DB ile ilişkili diğer kavramlar hakkında bilgi edinmek için geçebilirsiniz:

* [Nasıl yapılır, Azure Cosmos hesabınızı yönetme](how-to-manage-database-account.md)
* [Genel dağıtım](distribute-data-globally.md)
* [Tutarlılık düzeyleri](consistency-levels.md)
* [Azure Cosmos kapsayıcıları ve öğeleri ile çalışma](databases-containers-items.md)
* [Sanal ağ hizmet uç noktası Azure Cosmos hesabınız için](vnet-service-endpoint.md)
* [Azure Cosmos hesabınız için IP Güvenlik Duvarı](firewall-support.md)
* [Nasıl yapılır, Azure Cosmos hesabınıza Azure bölgeleri ekleyip](how-to-manage-database-account.md)
* [Azure Cosmos DB SLA'lar](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
