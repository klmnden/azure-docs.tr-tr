---
title: Verileri Azure Cosmos DB ile küresel olarak dağıtma
description: Bir Global olarak dağıtılmış çok modelli veritabanı hizmeti olan Azure Cosmos DB genel veritabanlarından kullanarak çok büyük ölçekli coğrafi çoğaltma, çok yöneticili, yük devretme ve veri kurtarma hakkında bilgi edinin.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/18/2019
ms.openlocfilehash: 70ead36e20861026e08e864f438071948c526844
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60889061"
---
# <a name="global-data-distribution-with-azure-cosmos-db---overview"></a>Genel veri dağıtım ile bir Azure Cosmos DB - genel bakış

Günümüzün uygulamaları yüksek derecede yanıt veren ve her zaman çevrimiçi olması gerekir. Düşük gecikme süresi ve yüksek kullanılabilirlik elde etmek için bu uygulamaların örnekleri kendi kullanıcılara yakın olan veri merkezlerinde dağıtılması gerekir. Bu uygulamalar genellikle birden çok veri merkezlerinde dağıtılır ve Global olarak dağıtılmış olarak adlandırılır. Global olarak dağıtılmış uygulamaların kullanıcılarına yakın olan verilerin bir kopyasını üzerinde çalışılacak uygulamaları etkinleştirmek için dünyanın herhangi bir veri şeffaf bir şekilde çoğaltabilir Global olarak dağıtılmış bir veritabanı gerekir. 

Azure Cosmos DB, düşük gecikme süresi, aktarım hızı, esnek ölçeklenebilirlik, iyi tanımlanmış semantik veri tutarlılığı ve yüksek kullanılabilirlik sağlamak üzere tasarlanmış bir Global olarak dağıtılmış veritabanı hizmetidir. Kısacası, uygulamanızı her zaman çevrimiçi olması zorunludur, garantili hızlı yanıt süresi dünyanın herhangi bir yere gerekir ve sınırsız ve elastik ölçeklenebilirliğini aktarım hızı ve depolama gerekir, uygulamanızı Azure Cosmos DB üzerinde oluşturması gerekir.

Veritabanlarınızı, tüm Azure bölgelerinde genel olarak dağıtılmış ve kullanılabilir olmasını yapılandırabilirsiniz. Gecikme süresini azaltmak için kullanıcılarınızın bulunduğu konumlara yakın veri yerleştirin. Gerekli bölgelerini seçme çaplı uygulamanız ve kullanıcılarınızın bulunduğu yere bağlıdır. Cosmos DB verileri Cosmos hesabınızla ilişkili tüm bölgelere şeffaf biçimde çoğaltır. Bu, küresel olarak dağıtılan Azure Cosmos veritabanı ve uygulamanızı okuma ve yerel olarak yazma kapsayıcıları tek bir sistem görüntüsü sağlar. 

Azure Cosmos DB ile ekleyebilir veya herhangi bir zamanda hesabınızla ilişkili bölgelerle kaldırın. Uygulamanızı duraklatıldı ya da eklemek veya bir bölgeyi kaldırmak için yeniden gerekmez. Her zaman yerel olarak hizmet sunan çok girişli özellikleri nedeniyle yüksek düzeyde kullanılabilir olmaya devam eder.

![Yüksek oranda kullanılabilir bir dağıtım topolojisi](./media/distribute-data-globally/deployment-topology.png)

## <a name="key-benefits-of-global-distribution"></a>Genel dağıtım kilit yararları

**Genel etkin-etkin uygulamalar oluşturun.** Kendi özgün çok yöneticili çoğaltma protokolü ile her bölge, hem yazma hem de okuma destekler. Çok yöneticili özelliği de sağlar:

- Sınırsız esnek, yazma ve ölçeklenebilirlik okuyun. 
- % 99,999 okuma ve yazma kullanılabilirliğini tüm dünyada.
- Okuma ve yazma işlemleri sırasında 99. yüzdebirlik dilimde 10 milisaniye cinsinden sunulan garanti edilir.

Azure Cosmos DB çoklu yönlendirmeyi kullanarak API'leri, uygulamanız en yakın bölgenin farkındadır ve bu bölgeye istekleri gönderebilirsiniz. Herhangi bir yapılandırma değişikliği gerçekleştirmeden en yakın bölgenin tanımlanır. İçin ve Azure Cosmos hesabınızdan bölgeleri ekleyip kaldırırken uygulamanızın yeniden ya da duraklatılmış gerekmez, her zaman yüksek oranda kullanılabilir olmaya devam eder.

**Yüksek derecede yanıt veren uygulamalar oluşturun.** Uygulamanızın gerçek zamanlı okuma yapabilirsiniz ve tüm bölgelere yönelik yazma veritabanınız için seçtiniz. Azure Cosmos DB tutarlılık düzeyi garantileri seçtiğiniz düzeyi ile bölgeler arası veri çoğaltma dahili olarak işler.

**Yüksek oranda kullanılabilir uygulamalar oluşturun.** Birden çok bölgede bir veritabanı dünya çapında çalışan bir veritabanının kullanılabilirliğini artırır. Tek bir bölge kullanılamıyorsa, başka bölgelerde uygulama isteklerini otomatik olarak işler. Azure Cosmos DB, % 99,999 okuma ve yazma kullanılabilirliğini çok bölgeli veritabanları için sunar.

**Bölgesel kesintiler sırasında iş sürekliliği korur.** Azure Cosmos DB destekler [otomatik yük devretme](how-to-manage-database-account.md#automatic-failover) bölgesel bir kesinti sırasında. Bölgesel bir kesinti sırasında gecikme süresi, kullanılabilirlik, tutarlılık ve aktarım hızı SLA'larının korumak Azure Cosmos DB devam eder. Tüm uygulamanızın yüksek oranda kullanılabilir olduğundan emin olun yardımcı olmak için Cosmos DB, el ile yük devretme bölgesel bir kesinti simülasyonu yapma API sağlar. Bu API kullanarak normal iş sürekliliği tatbikatları gerçekleştirebilirsiniz.

**Ölçek, okuma ve aktarım hızı genel yazma.** Her bir bölge yazılabilir ve okuma ve yazma işlemleri dünyanın dört bir yanında esnek bir şekilde ölçeklendirmek etkinleştirebilirsiniz. Uygulamanızı bir Azure Cosmos veritabanı veya bir kapsayıcı yapılandırır aktarım hızı, Azure Cosmos hesabınızla ilişkili tüm bölgeler arasında teslim garanti edilir. Sağlanan aktarım hızına göre yukarı sağlanır [SLA'mali olarak desteklenen](https://aka.ms/acdbsla).

**Birden fazla iyi tanımlanmış tutarlılık modellerinden seçin.** Azure Cosmos DB çoğaltma Protokolü beş iyi tanımlanmış, pratik ve sezgisel tutarlılık modeli sunar. Her model, tutarlılık ve performans arasında bir denge sahiptir. Bu tutarlılık modeli, Global olarak dağıtılmış uygulamaları kolayca oluşturmak için kullanın.

## <a id="Next Steps"></a>Sonraki adımlar

Hakkında daha fazla küresel dağıtım için aşağıdaki makalelere bakın:

* [Genel dağıtım - başlık altında](global-dist-under-the-hood.md)
* [Çok yöneticili uygulamalarınızda yapılandırma](how-to-multi-master.md)
* [Birden çok giriş için istemcileri yapılandırma](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Bölge ekleme veya Azure Cosmos DB hesabınızdan kaldırma](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [SQL API hesabı için bir özel çakışma çözüm ilkesi oluşturma](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)