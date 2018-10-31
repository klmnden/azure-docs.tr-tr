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
ms.openlocfilehash: 68ac603964593892dfcbc3488b4e6f2c91a0eb92
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50238283"
---
# <a name="global-data-distribution-with-azure-cosmos-db"></a>Azure Cosmos DB ile verileri küresel dağıtım

Günümüzde uygulamaların birçoğu, çalıştırmak veya amaçlarını, çok büyük ölçekte çalıştırmak için sahip. Bunlar her zaman açıktır ve kullanıcılar dünyanın farklı yerlerindeki erişebilir. Yüksek performanslı ve yüksek kullanılabilirlik sağlarken bu uygulamaları tarafından kullanılan verileri genel dağıtımını yönetmek bir sabit bir sorundur. Cosmos DB, baştan yukarı bu zorluklar karşılamak için tasarlanmış bir Global olarak dağıtılmış veritabanı hizmetidir.

Cosmos DB, temel Azure hizmet aşamasındadır ve tüm kullanılabilir [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/) varsayılan olarak. Microsoft, 50'den fazla bölgede dünyanın dört bir yanındaki Azure veri merkezleri işletmektedir ve müşterilerin büyüyen ihtiyaçlarını karşılamak üzere genişletmeye devam ediyor. Microsoft, Cosmos DB çalıştırır 7/24, hizmet, uygulamalarınız üzerinde odaklanabilirsiniz. Bir Cosmos DB hesabı oluşturduğunuzda, hangi bölgelerin içinde dağıtılmalıdır karar verin.

Cosmos DB müşteriler, Global olarak dağıtılmış ve bulunan her yerden 1'den 50'den fazla Azure bölgeleri için veritabanlarını yapılandırabilirsiniz. Daha düşük gecikme süresi için daha yakın veriler, kullanıcının konumuna kadar kaç bölgeleri karar yerleştirin ve uygulamanız ve kullanıcılarınızın bulunduğu konumlara erişmek, genel üzerinde çalıştırmak için hangi bölgeyi bağlıdır. Cosmos DB, Cosmos hesabınızdaki tüm verileri yapılandırılmış tüm bölgelere şeffaf biçimde çoğaltır. Cosmos DB, Cosmos veritabanı ve kapsayıcıları tek bir sistem görüntüsü sağlar, böylece uygulamanız okuyabilir ve yerel olarak yazabilirsiniz. Cosmos DB ile ekleyebilir veya herhangi bir zamanda hesabınızla ilişkili bölgelerle kaldırın. Uygulamanızı duraklatıldı ya da imzalanmasını gerekli değildir ve veri hizmeti sağlayan çok girişli özellikleri sayesinde her zaman hizmet yüksek oranda kullanılabilir olmaya devam eder.

## <a name="key-benefits-of-global-distribution"></a>Genel dağıtım kilit yararları

**Genel etkin-etkin uygulamaları kolayca oluşturmak**: çok yöneticili özelliğiyle, her bölge (okunabilir olmasının yanı sıra) yazılabilir olduğundan, sınırsız elastik etkinleştirme yazma ölçeklenebilirliği, % 99,999 okuma ve yazma kullanılabilirliğini, tüm dünyada ve Hızlı Okuma ve yazma işlemleri, 99. yüzdebirlik dilimde 10 milisaniye cinsinden sunulan garanti edilir.  

Kullanarak Azure Cosmos DB'nin çok girişli API'lerini uygulamanızı her zaman en yakın bölgenin bilir ve istekleri bu bölgeye gönderir. Herhangi bir yapılandırma değişikliği gerçekleştirmeden en yakın bölgenin tanımlanır. Cosmos DB hesabınızdan bölgeleri ekleyip kaldırırken uygulamanızın dağıtılması gerekmez ve yüksek oranda kullanılabilir olmaya devam eder.

**Yüksek derecede yanıt veren uygulamalar oluşturmayı**: uygulamanızı kolayca neredeyse gerçek zamanlı okuma ve yazma işlemleri, tek basamaklı milisaniyelik gecikme süresi ile gerçekleştirmek için tasarlanmış olabilir, veritabanınızı seçtiğiniz karşı tüm bölgeler.  Azure Cosmos DB, Cosmos hesap için seçilen tutarlılık düzeyi garantileri şekilde bölgeler arası veri çoğaltma dahili olarak işler.

Birçok uygulama, çok bölgeli (yerel) yazma işlemleri gerçekleştirme olanağı gelen performans geliştirmeleri yararlı olacaktır. Güçlü tutarlılık gerektiren olanlar gibi bazı uygulamalar, tüm yazma işlemlerini tek bir bölge için Huni tercih eder. Cosmos DB, yapılandırmaları, tek bölge ve çok bölgeli destekler.

**Yüksek oranda kullanılabilir uygulamalar oluşturmayı**: çalıştıran birden çok bölgede bir veritabanı, veritabanının kullanılabilirliğini artırır. Tek bir bölge kullanılamıyorsa, diğer bölgelere otomatik olarak uygulama isteklerini işler. Azure Cosmos DB, % 99,999 okuma ve yazma kullanılabilirliğini çok bölgeli veritabanları için sunar.

**Bölgesel kesintiler sırasında iş sürekliliği**: Azure Cosmos DB destekleyen [otomatik yük devretme](how-to-manage-database-account.md#enable-automatic-failover-for-your-cosmos-account) bölgesel bir kesinti sırasında. Üstelik, bölgesel bir kesinti sırasında Cosmos DB, gecikme süresi, kullanılabilirlik, tutarlılık ve aktarım hızı SLA'lar korumak devam eder. Bütün uygulamanızı yüksek oranda kullanılabilir olduğundan emin olmak için Azure Cosmos DB bölgesel bir kesinti simülasyonu yapma el ile yük devretme API sunar. Bu API kullanarak normal iş sürekliliği tatbikat gerçekleştirme ve hazır.

**Global ölçeklenebilirlik okuyup**: çok yöneticili özelliğine sahip esnek bir biçimde ölçeklendirmek okuma ve yazma aktarım hızı tüm dünyada. Çok yöneticili özelliği, uygulamanız bir Azure Cosmos DB veritabanında yapılandırır veya bir kapsayıcı teslim tüm bölgeler arasında ve korunan aktarım hızı garanti [SLA'mali olarak desteklenen](https://aka.ms/acdbsla).

**Birden çok, iyi tanımlanmış tutarlılık modeli**: Azure Cosmos DB'nin çoğaltma Protokolü beş iyi tanımlanmış, pratik sunmak üzere tasarlanmıştır ve her tutarlılık ve performans arasında NET bir denge modeller sezgisel tutarlılık. Bu tutarlılık modeli doğru Global olarak dağıtılmış uygulamaları kolayca oluşturmanıza olanak sağlar.

## <a id="Next Steps"></a>Sonraki adımlar

Hakkında daha fazla küresel dağıtım için aşağıdaki makalelere bakın:

* [Genel dağıtım - başlık altında](global-dist-under-the-hood.md)
* [Birden çok giriş için istemcileri yapılandırma](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Nasıl bölge veritabanınızdan Ekle/Kaldır](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [SQL API hesabı için bir özel çakışma çözüm ilkesi oluşturma](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)