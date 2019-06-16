---
title: Azure Cosmos DB etcd API giriş
description: Bu makalede bir Azure Cosmos DB'de etcd API genel bakış ve temel avantajları sağlar.
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: dech
ms.reviewer: sngun
ms.openlocfilehash: 6c7fcb1429438ee024cb226b63cfcdcab05ed9f8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65205798"
---
# <a name="introduction-to-the-azure-cosmos-db-etcd-api-preview"></a>Azure Cosmos DB etcd API'si (Önizleme) giriş

Azure Cosmos DB, Microsoft'un Global olarak dağıtılmış çok modelli veritabanı hizmeti görev açısından kritik uygulamalar için ' dir. Anahtar teslim küresel dağıtım, elastik ölçeklendirmenin aktarım hızı ve depolama, 99. yüzdebirlik dilimde Tek haneli milisaniyelik gecikme süreleri sunar ve garantili yüksek kullanılabilirlik, sektör lideri SLA ile desteklenir.

[Etcd](https://github.com/etcd-io/etcd) dağıtılmış anahtar/değer deposu. İçinde [Kubernetes](https://kubernetes.io/), etcd durumu ve Kubernetes küme yapılandırmasını depolamak için kullanılır. Kullanılabilirlik, güvenilirlik ve performansını etcd sağlayarak, genel küme sistem durumu, ölçeklenebilirlik, elastiklik kullanılabilirliğini ve performansını bir Kubernetes kümesi için önemlidir. 

Azure Cosmos DB'de etcd API için arka uç deposu olarak Azure Cosmos DB kullanmanıza olanak tanır [Azure Kubernetes](../aks/index.yml). Azure Cosmos DB'de etcd API'si şu anda Önizleme aşamasındadır. Azure Cosmos DB etcd kablo protokolünü uygular. API Azure Cosmos DB'de etcd ile geliştiriciler otomatik olarak yüksek oranda güvenilir alırsınız [kullanılabilir](high-availability.md), [Global olarak dağıtılmış](distribute-data-globally.md) Kubernetes. Bu API, geliştiricilerin Kubernetes durum yönetimi üzerinde tam olarak yönetilen bulut yerel PaaS hizmeti ölçeklendirme sağlar. 

> [!NOTE]
> Azure Cosmos DB'de diğer API'ler bir etcd API hesabı Azure portalı, CLI veya SDK'ları aracılığıyla sağlayamazsınız. Yalnızca Resource Manager şablonu dağıtarak bir etcd API hesabı sağlayabilirsiniz; ayrıntılı adımlar için bkz. [sağlama Azure Cosmos DB ile Azure Kubernetes](bootstrap-kubernetes-cluster.md) makalesi. Azure Cosmos DB etcd API'si şu anda sınırlı Önizleme aşamasındadır. Yapabilecekleriniz [Önizleme için kaydolun](https://aka.ms/cosmosetcdapi-signup), kaydolma formu doldurarak.

## <a name="wire-level-compatibility"></a>Hat düzeyinde uyumluluk

Azure Cosmos DB etcd sürüm 3 kablo protokolünü uygular ve sağlayan [ana düğümün](https://kubernetes.io/docs/concepts/overview/components/) API sunucuları Azure Cosmos DB yerel olarak yüklenmiş etcd ortamda yaptığınız gibi kullanın. API etcd TLS karşılıklı kimlik doğrulamayı destekler. 

Aşağıdaki diyagramda, bir Kubernetes kümesinin bileşenleri gösterilmektedir. Küme asıl API sunucusu yerel olarak yüklenmiş etcd yerine Azure Cosmos DB etcd API kullanır. 

![Azure Cosmos DB etcd kablo protokolünü uygulama](./media/etcd-api-introduction/etcd-api-wire-protocol.png)

## <a name="key-benefits"></a>Önemli avantajlar

### <a name="no-etcd-operations-management"></a>Hiçbir etcd operasyon Yönetimi

Bir yerel tam olarak yönetilen bulut hizmeti olan Azure Cosmos DB, Kubernetes geliştiricilerin ayarlama ve yönetme etcd gereksinimini ortadan kaldırır. Azure Cosmos DB'de API etcd ölçeklenebilir, yüksek oranda kullanılabilir, hataya dayanıklı olduğundan ve yüksek performans sunar. Çoğaltma işlemini gerçekleştirme çalışırken, birden fazla düğümde ayarı yükü güncelleştirmeler, güvenlik düzeltme ekleri ve etcd sistem durumu izleme Azure Cosmos DB tarafından işlenir.

### <a name="global-distribution--high-availability"></a>Genel dağıtım ve yüksek kullanılabilirlik 

Etcd API'yi kullanarak Azure Cosmos DB veri okuma için % 99,99 oranında kullanılabilirlik düzeyini garanti eder ve bir tek bölge ve % 99,999 kullanılabilirlik bölgelerinde yazar. 

### <a name="elastic-scalability"></a>Elastik ölçeklenebilirlik

Azure Cosmos DB, okuma için esnek ölçeklenebilirlik sunar ve yazma istekleri farklı bölgeler arasında.
Kubernetes kümesini büyüdükçe, Azure Cosmos DB'de etcd API hesabı, kapalı kalma süresi esnek bir şekilde ölçeklendirir. Etcd veri Kubernetes ana düğümler yerine Azure Cosmos DB'de depolamak, aynı zamanda ana düğüm daha esnek ölçeklendirme sağlar. 

### <a name="security--enterprise-readiness"></a>Güvenlik ve kurumsal hazır olma durumu

Kubernetes geliştiriciler etcd verilerini Azure Cosmos DB'de depolanırken otomatik olarak Al [bekleyen yerleşik şifreleme](database-encryption-at-rest.md), [sertifikaları ve Uyumluluk](compliance.md), ve [yedekleme ve geri yükleme özellikleri](online-backup-and-restore.md) Azure Cosmos DB tarafından desteklenir. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB ile Azure Kubernetes kullanma](bootstrap-kubernetes-cluster.md)
* [Azure Cosmos DB anahtar avantajları](introduction.md)
* [AKS altyapısı Hızlı Başlangıç Kılavuzu](https://github.com/Azure/aks-engine/blob/master/docs/tutorials/quickstart.md)