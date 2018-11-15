---
title: Verileri Azure Cosmos DB ile küresel olarak dağıtma | Microsoft Docs
description: Bir Global olarak dağıtılmış çok modelli veritabanı hizmeti olan Azure Cosmos DB genel veritabanlarından kullanarak çok büyük ölçekli coğrafi çoğaltma, çok yöneticili, yük devretme ve veri kurtarma hakkında bilgi edinin.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: mjbrown
ms.openlocfilehash: c90450fa4cc35b460198f5a351a965aee4ea4f4b
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636425"
---
# <a name="build-globally-distributed-applications-with-azure-cosmos-db"></a>Azure Cosmos DB ile küresel olarak dağıtılmış uygulamalar oluşturun

Günümüzün uygulamalarının çoğu birden fazla veri merkezlerinde yapılacak gerektirir. Bu uygulamalar, Global olarak dağıtılmış uygulamalar olarak adlandırılır. Bu uygulamalar her zaman "açık" olduğunda ve bunlar dünya genelinde tüm kullanıcılar için erişilebilir. Genel dağıtım, düşük gecikme süresi, aktarım hızı ve dünyanın dört bir yanındaki yüksek kullanılabilirlik, esnek ölçeklenebilirlik'ı sağlayarak bu uygulamaları tarafından kullanılan veri yönetme sabit bir sorundur. Azure Cosmos DB, düşük gecikme süresi, aktarım hızı, esnek ölçeklenebilirlik, iyi tanımlanmış semantik veri tutarlılığı ve yüksek kullanılabilirlik sağlamak üzere tasarlanmış bir Global olarak dağıtılmış veritabanı hizmetidir. Kısacası, uygulamanızın gereksinimlerine göre her zaman çevrimiçi olmasını gerektirir ve sınırsız ve elastik ölçeklenebilirliğini aktarım hızını ve depolamayı gerekiyorsa dünyanın her yerden hızlı yanıt süresi garanti, Azure Cosmos DB kullanarak uygulamaları oluşturmayı düşünmelisiniz.

Azure Cosmos DB, temel Azure hizmet ve tüm kullanılabilir [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/) varsayılan olarak. Microsoft 54 fazla bölgenin dünyanın dört bir yanındaki Azure veri merkezlerinde çalışır ve müşterilerin büyüyen ihtiyaçlarını karşılamak için bölgesel varlığı genişletmeye devam ediyor. Bir Azure Cosmos hesabı oluşturduğunuzda, hangi bölgelerin içinde dağıtılmalıdır karar verin. Microsoft Azure Cosmos DB çalıştırır 7/24, hizmet, uygulamalarınız üzerinde odaklanabilirsiniz.

Veritabanlarınızı, tüm Azure bölgelerinde genel olarak dağıtılmış ve kullanılabilir olmasını yapılandırabilirsiniz. Gecikme süresini azaltmak için kullanıcılarınızın bulunduğu konumlara veri yakın yerleştirmeniz gerekir. Gerekli bölgelerini seçme çaplı uygulamanız ve kullanıcılarınızın bulunduğu yere bağlıdır. Azure Cosmos DB hesabınızdaki veriler, hesabınızla ilişkili tüm bölgelere şeffaf biçimde çoğaltır. Bu, küresel olarak dağıtılan Azure Cosmos veritabanı ve uygulamanızı okuma ve yerel olarak yazma kapsayıcıları tek bir sistem görüntüsü sağlar. Azure Cosmos DB ile ekleyebilir veya herhangi bir zamanda hesabınızla ilişkili bölgelerle kaldırın. Uygulamanızı duraklatıldı ya da eklemek veya bir bölgeyi kaldırmak için yeniden gerekmez. Her zaman hizmet sunan çok girişli özellikleri nedeniyle yüksek düzeyde kullanılabilir olmaya devam eder.

## <a name="key-benefits-of-global-distribution"></a>Genel dağıtım kilit yararları

**Genel etkin-etkin uygulamalar derleme**: çok yöneticili özelliğiyle, her bir yazma bölgesi (yanı sıra okunabilir olması) bölgedir. Çok yöneticili özelliği de garanti - sınırsız elastik yazma ölçeklenebilirliği, % 99,999, okuma ve yazma tüm dünya ve garantili okumayı/yazmayı hizmet kullanılabilirliğini 10 milisaniye olarak 99. yüzdebirlik.  

İle kullanarak Azure Cosmos DB'nin çok girişli API'lerini uygulamanız en yakın bölgenin farkındadır ve bu bölgeye istekleri gönderebilirsiniz. Herhangi bir yapılandırma değişikliği gerçekleştirmeden en yakın bölgenin tanımlanır. Azure Cosmos DB hesabınızdan bölgeleri ekleyip kaldırırken uygulamanızın dağıtılması gerekmez ve yüksek oranda kullanılabilir olmaya devam eder.

**Yüksek derecede yanıt veren uygulamalar oluşturmayı**: uygulamanızı kolayca neredeyse gerçek zamanlı okuma ve yazma işlemleri, tek haneli milisaniyelik gecikme süreleriyle veritabanınız için seçtiğiniz tüm bölgeler ile gerçekleştirmek için tasarlanmış olabilir.  Azure Cosmos hesap için seçilen tutarlılık düzeyi garanti, azure Cosmos DB bölgeler arasında veri çoğaltma dahili olarak işler.

Birçok uygulama, çok bölgeli (yerel) yazma işlemleri gerçekleştirme olanağı gelen performans geliştirmeleri yararlı olacaktır. Güçlü tutarlılık gerektiren bazı uygulamalar, tüm yazma işlemlerini tek bir bölge için Huni tercih eder. Bu uygulamaları desteklemek için Azure Cosmos DB hem tek bölgede hem de çok bölgeli yapılandırmalarını destekler.

**Yüksek oranda kullanılabilir uygulamalar oluşturmayı**: çalıştıran birden çok bölgede bir veritabanı, veritabanının kullanılabilirliğini artırır. Tek bir bölge kullanılamıyorsa, diğer bölgelere otomatik olarak uygulama isteklerini işler. Azure Cosmos DB, % 99,999 okuma ve yazma kullanılabilirliğini çok bölgeli veritabanları için sunar.

**Bölgesel kesintiler sırasında iş sürekliliği**: Azure Cosmos DB destekleyen [otomatik yük devretme](how-to-manage-database-account.md#automatic-failover) bölgesel bir kesinti sırasında. Üstelik, bölgesel bir kesinti sırasında gecikme süresi, kullanılabilirlik, tutarlılık ve aktarım hızı SLA'lar korumak Azure Cosmos DB eder. Tüm uygulamanızın yüksek oranda kullanılabilir olduğundan emin olun yardımcı olmak için Azure Cosmos DB bölgesel bir kesinti simülasyonu yapma el ile yük devretme API sunar. Bu API kullanarak normal iş sürekliliği tatbikatları gerçekleştirebilirsiniz.

**Global ölçeklenebilirlik okuyup**: çok yöneticili özelliğine sahip esnek bir biçimde ölçeklendirmek okuma ve yazma aktarım hızı tüm dünyada. Çok yöneticili özelliği, uygulamanız bir Azure Cosmos DB veritabanında yapılandırır veya bir kapsayıcı teslim tüm bölgeler arasında ve korunan aktarım hızı garanti [SLA'mali olarak desteklenen](https://aka.ms/acdbsla).

**Birden çok, iyi tanımlanmış tutarlılık modeli**: Azure Cosmos DB'nin çoğaltma Protokolü beş iyi tanımlanmış, pratik ve sezgisel tutarlılık modeli sunmak için tasarlanmıştır. Her bir tutarlılık modelini, tutarlılık ve performans arasında bir denge sahiptir. Bu tutarlılık modeli, Global olarak dağıtılmış uygulamaları kolayca oluşturmanıza olanak sağlar.

## <a id="Next Steps"></a>Sonraki adımlar

Hakkında daha fazla küresel dağıtım için aşağıdaki makalelere bakın:

* [Genel dağıtım - başlık altında](global-dist-under-the-hood.md)
* [Birden çok giriş için istemcileri yapılandırma](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Nasıl bölge Azure Cosmos hesabınızdan Ekle/Kaldır](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [SQL API hesabı için bir özel çakışma çözüm ilkesi oluşturma](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)