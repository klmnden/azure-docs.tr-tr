---
title: Azure Cosmos DB'de çok bölgeli dağıtımlar maliyetini en iyi duruma getirme
description: Bu makalede, Azure Cosmos DB'de çok bölgeli dağıtımlar, maliyetleri yönetmek açıklanmaktadır.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: rimman
ms.openlocfilehash: 478714f48782adb138f1ed803d53c81ec48f2efd
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65967280"
---
# <a name="optimize-multi-region-cost-in-azure-cosmos-db"></a>Azure Cosmos DB çok bölgeli maliyetini en iyi duruma getirme

Ekleme ve bölgeler, Azure Cosmos hesabınıza dilediğiniz zaman kaldırın. Çeşitli Azure Cosmos veritabanları ve kapsayıcılar için yapılandırdığınız aktarım hızı, hesabınızla ilişkili her bölgede ayrılmış. Saat başına aktarım hızı sağlanmış, varsa, Azure Cosmos hesabınız için tüm veritabanlarını ve kapsayıcılar arasında yapılandırılan RU/sn Topla `T` ve veritabanı hesabınızla ilişkili Azure bölgelerinin sayısı `N`, toplam sağlanan aktarım hızı belirli bir saat Cosmos hesabınız için eşittir:

1. `T x N RU/s` Azure Cosmos hesabınızı bir tek bir yazma bölgesi ile yapılandırılmışsa. 

1. `T x (N+1) RU/s` Azure Cosmos hesabınızı yazma işleyebilen tüm bölgeler ile yapılandırılmışsa. 

Tek bir yazma bölgesi ile sağlanan aktarım hızı maliyeti $0.008/ saat başına 100 RU/sn ve sağlanan aktarım hızı birden çok yazılabilir bölge ile maliyetleri 0,016 $/ saat başına 100 RU/sn. Daha fazla bilgi için bkz: Azure Cosmos DB [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/).

## <a name="costs-for-multiple-write-regions"></a>Birden çok yazma bölgeleri için maliyet

Çok yöneticili bir sistemde arttıkça net kullanılabilir RU'ları için yazma `N` zaman nerede `N` yazma bölgeleri sayısıdır. Tek bölge yazma aksine her bölge artık yazılabilir olduğundan ve çakışma desteklemelidir. İş yükü yazıcılar için miktarını arttı. Gerçekleştirilecek açısından, planlama maliyetinden `M` yazma dünya çapında değerinde RU/s, M sağlamak gerekir `RUs` bir kapsayıcı veya veritabanı düzeyinde. Yazma işlemleri için bunları gerçekleştirmek için kullanın ve gibi gibi çok bölgesini daha sonra ekleyebilirsiniz `M` RU değerinde dünya çapında yazar. 

### <a name="example"></a>Örnek

Batı ABD'deki kapsayıcı sahip göz önünde bulundurun 10 K RU/sn aktarım hızı ile sağlanan ve 1 TB veri bu ay depolar. Varsayalım üç bölgesi: Doğu ABD, Kuzey Avrupa ve Doğu Asya, eklemek istediğiniz her aynı depolama ve aktarım hızı ve, kapsayıcılara dört tüm bölgelerde küresel olarak dağıtılan uygulamanızdan yazma olanağı. (31 gün varsayılarak) aylık toplam faturanız ayda aşağıdaki gibidir:

|**Öğesi**|**Kullanım (aylık)**|**Oranı**|**Aylık maliyet**|
|----|----|----|----|
|(Birden çok yazma bölgeleri) Batı ABD'deki kapsayıcı için aktarım hızı faturası |10 K RU/sn * 24 * 31 |0,016 başına saat başına 100 RU/sn |$1,190.40 |
|3 ek bölge - Doğu ABD, Kuzey Avrupa ve Doğu Asya (birden çok yazma bölgeleri) için aktarım hızı faturası |(3 + 1) * 10 K RU/sn * 24 * 31 |0,016 başına saat başına 100 RU/sn |$4,761.60 |
|Batı ABD’deki kapsayıcı için depolama faturası |100 GB |0,25 DOLAR/GB |$25 |
|3 ek bölge (Doğu ABD, Kuzey Avrupa ve Doğu Asya) için depolama faturası |3 * 1 TB |0,25 DOLAR/GB |$75 |
|**Toplam**|||**$6,052** |

## <a name="improve-throughput-utilization-on-a-per-region-basis"></a>İşleme kullanımı geliştirmemizi bir bölge temelinde

Verimsiz kullanımı varsa, örneğin, bir veya daha fazla altında kullanılan veya bölgeler aşırı kullanılan, aktarım hızı kullanımı artırmak için aşağıdaki adımları alabilir:  

1. Sağlanan aktarım hızına (RU) yazma bölgesi içinde ilk iyileştirmek ve ardından en fazla RU'ları okuma bölgelerinde değişiklik akışı okuma bölgesi vb. ' kullanarak kullanabilmesine emin olun. 

2. Birden çok yazma bölgeleri okuma ve yazma işlemleri, Azure Cosmos hesapla ilişkili tüm bölgeler arasında ölçeklendirilebilir. 

3. Bölgelerdeki etkinliğini ve isteğe bağlı olarak, okuma ve yazma aktarım hızı bölgeleri ekleyip.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde Azure Cosmos DB'de maliyet iyileştirmesi hakkında daha fazla bilgi edinmek için sonraki geçebilirsiniz:

* Daha fazla bilgi edinin [en iyi duruma getirme için geliştirme ve test etme](optimize-dev-test.md)
* Daha fazla bilgi edinin [Azure Cosmos DB faturanızı anlama](understand-your-bill.md)
* Daha fazla bilgi edinin [aktarım hızı maliyeti en iyi duruma getirme](optimize-cost-throughput.md)
* Daha fazla bilgi edinin [depolama maliyetini en iyi duruma getirme](optimize-cost-storage.md)
* Daha fazla bilgi edinin [okuma ve yazma işlemleri maliyetini en iyi duruma getirme](optimize-cost-reads-writes.md)
* Daha fazla bilgi edinin [sorguları maliyetini en iyi duruma getirme](optimize-cost-queries.md)

