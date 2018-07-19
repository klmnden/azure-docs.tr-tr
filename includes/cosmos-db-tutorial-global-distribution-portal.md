---
title: Azure Cosmos DB genel dağıtımı
description: Azure portalında Azure Cosmos DB ile genel olarak verilerin nasıl çoğaltılacağını öğrenin
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 03/26/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: fb9418d47d2888467e1c1c40862833029111b75b
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38747568"
---
Azure Cosmos DB Program Yöneticisi Manager Andrew Liu’nun genel dağıtım işlevini açıkladığı aşağıdaki videodan Azure Cosmos DB genel dağıtımı hakkında bilgi edinebilirsiniz.

>[!VIDEO https://www.youtube.com/embed/1D06yjTVxt8]

Genel veritabanı çoğaltmasının Azure Cosmos DB’de nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Cosmos DB ile verileri genel olarak dağıtma](../articles/cosmos-db/distribute-data-globally.md).

## <a id="addregion"></a>Azure portalını kullanarak genel veritabanı bölgeleri ekleme
Azure Cosmos DB, dünya genelindeki tüm [Azure bölgelerinde][azureregions] kullanılabilir. Veritabanı hesabınız için varsayılan tutarlılık düzeyini seçtikten sonra bir veya daha fazla bölgeyi (tercih ettiğiniz varsayılan tutarlılık düzeyine ve genel dağıtım gereksinimlerine bağlı olarak) ilişkilendirebilirsiniz.

1. [Azure portalında](https://portal.azure.com/), soldaki çubuktan **Azure Cosmos DB** seçeneğine tıklayın.
2. **Azure Cosmos DB** sayfasında, değiştirilecek veritabanı hesabını seçin.
3. Hesap sayfasındaki menüden **Verileri genel olarak çoğaltma** seçeneğine tıklayın.
4. **Verileri genel olarak çoğaltma** sayfasında, haritadaki bölgelere tıklayarak eklenecek veya kaldırılacak bölgeleri seçin ve sonra **Kaydet**’e tıklayın. Bölgeleri eklemenin bir maliyeti yoktur. Daha fazla bilgi için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/) veya [Azure Cosmos DB ile verileri genel olarak dağıtma](../articles/cosmos-db/distribute-data-globally.md) makalesine bakın.
   
    ![Haritadaki bölgelere tıklayarak bölgeler ekleyin veya kaldırın][1]
    
İkinci bir bölge eklediğinizde, portaldaki **Verileri genel olarak çoğaltma** sayfasında **El İle Yük Devretme** seçeneği etkinleştirilir. Yük devretme işlemini test etmek veya birincil yazma bölgesini değiştirmek için bu seçeneği kullanabilirsiniz. Üçüncü bir bölge eklemenizin ardından, okuma için yük devretme sırasını değiştirebilmeniz için aynı sayfada **Yük Devretme Öncelikleri** seçeneği etkinleştirilir.  

### <a name="selecting-global-database-regions"></a>Genel veritabanı bölgelerini seçme
İki veya daha fazla bölge yapılandırma için iki yaygın senaryo vardır:

1. Dünya nerede bulunurlarsa bulunsunlar, son kullanıcılara düşük gecikmeli veri erişimi sunma
2. İş sürekliliği ve olağanüstü durum kurtarma (BCDR) için bölgesel dayanıklılık ekleme

Son kullanıcılara düşük gecikme sunmak için, uygulama kullanıcılarının bulunduğu yere karşılık gelen bölgelerde hem uygulamayı hem de Azure Cosmos DB’yi dağıtmanız önerilir.

BCDR için, [İş sürekliliği ve olağanüstü durum kurtarma (BCDR): Azure Eşlenmiş Bölgeleri][bcdr] makalesinde açıklanan bölge çiftlerine göre bölgeler eklenmesi önerilir.

<!--

## <a id="selectwriteregion"></a>Select the write region

While all regions associated with your Cosmos DB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive the write (insert, upsert, replace, delete) requests. To set the active write region, do the following  


1. In the **Azure Cosmos DB** blade, select the database account to modify.
2. In the account blade, click **Replicate data globally** from the menu.
3. In the **Replicate data globally** blade, click **Manual Failover** from the top bar.
    ![Change the write region under Azure Cosmos DB Account > Replicate data globally > Manual Failover][2]
4. Select a read region to become the new write region, click the checkbox to confirm triggering a failover, and click OK
    ![Change the write region by selecting a new region in list under Azure Cosmos DB Account > Replicate data globally > Manual Failover][3]

--->

<!--Image references-->
[1]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-add-region.png
[2]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-manual-failover-1.png
[3]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-manual-failover-2.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: ../articles/cosmos-db/consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
