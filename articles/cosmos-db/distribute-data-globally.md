---
title: Verileri Azure Cosmos DB ile küresel olarak dağıtma | Microsoft Docs
description: Bir Global olarak dağıtılmış çok modelli veritabanı hizmeti olan Azure Cosmos DB genel veritabanlarından kullanarak çok büyük ölçekli coğrafi çoğaltma, çok yöneticili, yük devretme ve veri kurtarma hakkında öğrenin.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: mjbrown
ms.openlocfilehash: 6849574b9d16a9d76fffd4d69742c85941300e89
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52679486"
---
# <a name="global-data-distribution-with-azure-cosmos-db"></a>Azure Cosmos DB ile verileri küresel dağıtım

Günümüzün uygulamaları yüksek derecede yanıt veren ve her zaman çevrimiçi olması gerekir. Düşük gecikme süresi ve yüksek kullanılabilirlik elde etmek için bu uygulamaların örnekleri kendi kullanıcılara yakın olan veri merkezlerinde dağıtılması gerekir. Bu uygulamalar genellikle birden çok veri merkezlerinde dağıtılır ve Global olarak dağıtılmış olarak adlandırılır. Global olarak dağıtılmış uygulamaların kullanıcılarına yakın olan verilerin bir kopyasını üzerinde çalışılacak uygulamaları etkinleştirmek için dünyanın herhangi bir veri şeffaf bir şekilde çoğaltabilir Global olarak dağıtılmış bir veritabanı gerekir. 

Azure Cosmos DB, düşük gecikme süresi, aktarım hızı, esnek ölçeklenebilirlik, iyi tanımlanmış semantik veri tutarlılığı ve yüksek kullanılabilirlik sağlamak üzere tasarlanmış bir Global olarak dağıtılmış veritabanı hizmetidir. Uygulamanızı her zaman çevrimiçi olması zorunludur, garantili hızlı yanıt süresi dünyanın herhangi bir yere gerekir ve sınırsız ve elastik ölçeklenebilirliğini aktarım hızı ve depolama gerekir, Kısacası, Azure Cosmos DB kullanarak uygulamaları oluşturmayı düşünün.

Veritabanlarınızı, tüm Azure bölgelerinde genel olarak dağıtılmış ve kullanılabilir olmasını yapılandırabilirsiniz. Gecikme süresini azaltmak için kullanıcılarınızın bulunduğu konumlara veri yakın yerleştirin. Gerekli bölgelerini seçme çaplı uygulamanız ve kullanıcılarınızın bulunduğu yere bağlıdır. Azure Cosmos DB hesabınızdaki veriler, hesabınızla ilişkili tüm bölgelere şeffaf biçimde çoğaltır. Bu, küresel olarak dağıtılan Azure Cosmos veritabanı ve uygulamanızı okuma ve yerel olarak yazma kapsayıcıları tek bir sistem görüntüsü sağlar. 

Azure Cosmos DB ile ekleyebilir veya herhangi bir zamanda hesabınızla ilişkili bölgelerle kaldırın. Uygulamanızı duraklatıldı ya da eklemek veya bir bölgeyi kaldırmak için yeniden gerekmez. Her zaman hizmeti sağlayan birden çok girişe atanması özellikleri nedeniyle yüksek düzeyde kullanılabilir olmaya devam eder.

## <a name="key-benefits-of-global-distribution"></a>Genel dağıtım kilit yararları

**Genel etkin-etkin uygulamalar oluşturun.** Çok yöneticili özelliğiyle, her bir yazma bölgesi bölgedir. Okunabilir durumdadır. Çok yöneticili özelliği de garanti eder:

- Sınırsız esnek ölçeklenebilirlik yazın. 
- % 99,999 okuma ve yazma kullanılabilirliğini tüm dünyada.
- Okuma ve yazma işlemleri sırasında 99. yüzdebirlik dilimde 10 milisaniye cinsinden sunulan garanti edilir.

Azure Cosmos DB birden çok giriş API'leri kullanarak, uygulamanızın en yakın bölge farkındadır. Bu bölgeye istekleri gönderebilirsiniz. Herhangi bir yapılandırma değişikliği gerçekleştirmeden en yakın bölgenin tanımlanır. Azure Cosmos DB hesabınızdan bölgeleri ekleyip kaldırırken uygulamanızın yeniden dağıtmanız gerekmez. Uygulama, yüksek oranda kullanılabilir olmaya devam eder.

**Yüksek derecede yanıt veren uygulamalar oluşturun.** Uygulamanız, gerçek zamanlı okuma ve yazma işlemleri gerçekleştirmek için kolayca tasarlanabilir. Tek basamaklı milisaniyelik gecikme süreleri, veritabanınız için seçtiğiniz tüm bölgelerde karşı kullanabilirsiniz. Azure Cosmos DB bölgeler arasında veri çoğaltma dahili olarak işler. Sonuç olarak, Azure Cosmos DB hesabı için seçilen tutarlılık düzeyi garanti edilir.

Birçok uygulama, çok bölgeli (yerel) yazma işlemleri gerçekleştirme olanağı gelen performans geliştirmeleri yararlanın. Güçlü tutarlılık gerektiren bazı uygulamalar, tüm yazma işlemlerini tek bir bölge için Huni tercih eder. Bu uygulamalar için Azure Cosmos DB, tek bölge ve çok bölgeli yapılandırmaları destekler.

**Yüksek oranda kullanılabilir uygulamalar oluşturun.** Birçok bölgede bir veritabanı çalışan bir veritabanının kullanılabilirliğini artırır. Tek bir bölge kullanılamıyorsa, başka bölgelerde uygulama isteklerini otomatik olarak işler. Azure Cosmos DB, % 99,999 okuma ve yazma kullanılabilirliğini çok bölgeli veritabanları için sunar.

**Bölgesel kesintiler sırasında iş sürekliliği korur.** Azure Cosmos DB destekler [otomatik yük devretme](how-to-manage-database-account.md#automatic-failover) bölgesel bir kesinti sırasında. Bölgesel bir kesinti sırasında gecikme süresi, kullanılabilirlik, tutarlılık ve aktarım hızı SLA'larının korumak Azure Cosmos DB devam eder. Tüm uygulamanızın yüksek oranda kullanılabilir olduğundan emin olun yardımcı olmak için Azure Cosmos DB, el ile yük devretme bölgesel bir kesinti simülasyonu yapma API sunar. Bu API kullanarak normal iş sürekliliği tatbikatları gerçekleştirebilirsiniz.

**Ölçek, okuma ve aktarım hızı genel yazma.** Çok yöneticili özelliğiyle, esnek bir biçimde ölçeklendirmek okuma ve yazma aktarım hızı tüm dünyada. Çok yöneticili özelliğini, uygulamanızı bir Azure Cosmos DB veritabanında yapılandırır ya da tüm bölgeler arasında bir kapsayıcı teslim edilen aktarım hızı garanti eder. Aktarım hızı da tarafından korunan [SLA'mali olarak desteklenen](https://aka.ms/acdbsla).

**Birden fazla iyi tanımlanmış tutarlılık modellerinden seçin.** Azure Cosmos DB çoğaltma Protokolü beş iyi tanımlanmış, pratik ve sezgisel tutarlılık modeli sunar. Her model, tutarlılık ve performans arasında bir denge sahiptir. Bu tutarlılık modeli, Global olarak dağıtılmış uygulamaları kolayca oluşturmak için kullanın.

## <a id="Next Steps"></a>Sonraki adımlar

Hakkında daha fazla küresel dağıtım için aşağıdaki makalelere bakın:

* [Genel dağıtım - başlık altında](global-dist-under-the-hood.md)
* [Birden çok giriş için istemcileri yapılandırma](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Bölge ekleme veya Azure Cosmos DB hesabınızdan kaldırma](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [SQL API hesabı için bir özel çakışma çözüm ilkesi oluşturma](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)
