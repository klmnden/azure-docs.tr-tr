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
ms.openlocfilehash: 7f2c242d7040413598864222efdf06843eddc7d9
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50959503"
---
# <a name="global-data-distribution-with-azure-cosmos-db"></a>Azure Cosmos DB ile verileri küresel dağıtım

Günümüzde uygulamaların birçoğu, dünya ölçeğinde çalıştırın. Bu uygulamalar her zaman açık ve tüm dünya genelinde kullanıcılar tarafından erişilebilir. Yüksek performanslı ve yüksek kullanılabilirlik sağlarken bu uygulamaları tarafından kullanılan verileri genel dağıtımını yönetmek bir sabit bir sorundur. Azure Cosmos DB, yüksek performanslı ve yüksek kullanılabilirlik sağlamak üzere tasarlanmış bir Global olarak dağıtılmış veritabanı hizmetidir. Bu nedenlerle, Azure Cosmos DB Bu gerçek zamanlı uygulamalar için idealdir.

Cosmos DB, temel Azure hizmet aşamasındadır ve tüm kullanılabilir [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/) varsayılan olarak. Microsoft, 50'den fazla bölgede dünyanın dört bir yanındaki Azure veri merkezleri işletmektedir ve müşterilerin büyüyen ihtiyaçlarını karşılamak üzere genişletmeye devam ediyor. Bir Cosmos DB hesabı oluşturduğunuzda, hangi bölgelerin içinde dağıtılmalıdır karar verin. Microsoft, Cosmos DB çalıştırır 7/24, hizmet, uygulamalarınız üzerinde odaklanabilirsiniz.

50'den fazla Azure bölgelerinden birini Global olarak dağıtılmış ve kullanılabilir olmasını veritabanlarınızı yapılandırabilirsiniz. Daha düşük gecikme süresi için daha yakın veriler kullanıcının bulunduğu yere yerleştirmeniz gerekir. Gerekli bölgelerini seçme çaplı uygulamanız ve kullanıcılarınızın bulunduğu yere bağlıdır. Cosmos DB hesabınızdaki verilerin yapılandırılmış tüm bölgelere şeffaf biçimde çoğaltır. Bu, Cosmos veritabanı ve tarafından uygulamanızı okuyabilir ve yerel olarak yazabilirsiniz kapsayıcıları tek bir sistem görüntüsü sağlar. Cosmos DB ile ekleyebilir veya herhangi bir zamanda hesabınızla ilişkili bölgelerle kaldırın. Uygulamanızı duraklatıldı ya da eklemek veya bir bölgeyi kaldırmak için yeniden gerekmez. Her zaman hizmet sunan çok girişli özellikleri nedeniyle yüksek düzeyde kullanılabilir olmaya devam eder.

## <a name="key-benefits-of-global-distribution"></a>Genel dağıtım kilit yararları

**Genel etkin-etkin uygulamalar derleme**: çok yöneticili özelliğiyle, her bir yazma bölgesi (yanı sıra okunabilir olması) bölgedir. Çok yöneticili özelliği de garanti - sınırsız elastik yazma ölçeklenebilirliği, % 99,999, okuma ve yazma tüm dünya ve garantili okumayı/yazmayı hizmet kullanılabilirliğini 10 milisaniye olarak 99. yüzdebirlik.  

İle kullanarak Azure Cosmos DB'nin çok girişli API'lerini uygulamanız en yakın bölgenin farkındadır ve bu bölgeye istekleri gönderebilirsiniz. Herhangi bir yapılandırma değişikliği gerçekleştirmeden en yakın bölgenin tanımlanır. Cosmos DB hesabınızdan bölgeleri ekleyip kaldırırken uygulamanızın dağıtılması gerekmez ve yüksek oranda kullanılabilir olmaya devam eder.

**Yüksek derecede yanıt veren uygulamalar oluşturmayı**: uygulamanızı kolayca neredeyse gerçek zamanlı okuma ve yazma işlemleri, tek haneli milisaniyelik gecikme süreleriyle veritabanınız için seçtiğiniz tüm bölgeler ile gerçekleştirmek için tasarlanmış olabilir.  Cosmos hesap için seçilen tutarlılık düzeyi garanti, azure Cosmos DB bölgeler arasında veri çoğaltma dahili olarak işler.

Birçok uygulama, çok bölgeli (yerel) yazma işlemleri gerçekleştirme olanağı gelen performans geliştirmeleri yararlı olacaktır. Güçlü tutarlılık gerektiren bazı uygulamalar, tüm yazma işlemlerini tek bir bölge için Huni tercih eder. Bu uygulamayı desteklemek için Cosmos DB hem tek bölgede hem de çok bölgeli yapılandırmalarını destekler.

**Yüksek oranda kullanılabilir uygulamalar oluşturmayı**: çalıştıran birden çok bölgede bir veritabanı, veritabanının kullanılabilirliğini artırır. Tek bir bölge kullanılamıyorsa, diğer bölgelere otomatik olarak uygulama isteklerini işler. Azure Cosmos DB, % 99,999 okuma ve yazma kullanılabilirliğini çok bölgeli veritabanları için sunar.

**Bölgesel kesintiler sırasında iş sürekliliği**: Azure Cosmos DB destekleyen [otomatik yük devretme](how-to-manage-database-account.md#enable-automatic-failover-for-your-cosmos-account) bölgesel bir kesinti sırasında. Üstelik, bölgesel bir kesinti sırasında Cosmos DB, gecikme süresi, kullanılabilirlik, tutarlılık ve aktarım hızı SLA'lar korumak devam eder. Tüm uygulamanızın yüksek oranda kullanılabilir olduğundan emin olun yardımcı olmak için Azure Cosmos DB bölgesel bir kesinti simülasyonu yapma el ile yük devretme API sunar. Bu API kullanarak normal iş sürekliliği tatbikatları gerçekleştirebilirsiniz.

**Global ölçeklenebilirlik okuyup**: çok yöneticili özelliğine sahip esnek bir biçimde ölçeklendirmek okuma ve yazma aktarım hızı tüm dünyada. Çok yöneticili özelliği, uygulamanız bir Azure Cosmos DB veritabanında yapılandırır veya bir kapsayıcı teslim tüm bölgeler arasında ve korunan aktarım hızı garanti [SLA'mali olarak desteklenen](https://aka.ms/acdbsla).

**Birden çok, iyi tanımlanmış tutarlılık modeli**: Azure Cosmos DB'nin çoğaltma Protokolü beş iyi tanımlanmış, pratik ve sezgisel tutarlılık modeli sunmak için tasarlanmıştır. Her bir tutarlılık modelini, tutarlılık ve performans arasında bir denge sahiptir. Bu tutarlılık modeli, Global olarak dağıtılmış uygulamaları kolayca oluşturmanıza olanak sağlar.

## <a id="Next Steps"></a>Sonraki adımlar

Hakkında daha fazla küresel dağıtım için aşağıdaki makalelere bakın:

* [Genel dağıtım - başlık altında](global-dist-under-the-hood.md)
* [Birden çok giriş için istemcileri yapılandırma](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Nasıl bölge veritabanınızdan Ekle/Kaldır](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [SQL API hesabı için bir özel çakışma çözüm ilkesi oluşturma](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)