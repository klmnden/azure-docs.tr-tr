---
title: "Öğretici: MongoDB için API için Azure Cosmos DB'nin MongoDB çevrimiçi geçirmek için Azure veritabanı geçiş hizmeti kullanın | Microsoft Docs"
description: MongoDB şirket içinden Azure Cosmos DB API için çevrimiçi MongoDB için Azure veritabanı geçiş hizmeti kullanarak geçirmeyi öğrenin.
services: dms
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 07/04/2019
ms.openlocfilehash: 17f1b36ba5d5b699cce621db3917ef92654047ff
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565570"
---
# <a name="tutorial-migrate-mongodb-to-azure-cosmos-dbs-api-for-mongodb-online-using-dms"></a>Öğretici: MongoDB için Azure Cosmos DB API için MongoDB geçişi çevrimiçi DMS kullanarak

MongoDB için API Azure Cosmos DB'nin MongoDB örneğini Bulut veya bir şirket içi veritabanları bir çevrimiçi (en düşük kapalı kalma süresi) geçişi gerçekleştirmek için Azure veritabanı geçiş hizmeti kullanabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
>
> * Azure veritabanı geçiş Hizmeti'nin bir örneğini oluşturun.
> * Azure veritabanı geçiş hizmeti kullanarak bir geçiş projesi oluşturun.
> * Geçişi çalıştırma.
> * Geçişi izleme.
> * Hazır olduğunuzda geçişi tamamlayın.

Bu öğreticide, bir veri kümesinde en az kapalı kalma süresi ile MongoDB için Azure veritabanı geçiş hizmetini kullanarak Azure Cosmos DB API'si için bir Azure sanal Makinesi'nde barındırılan bir MongoDB geçirin. Önceden kurulmuş bir MongoDB kaynağına sahip değilseniz, bkz [yükleyin ve azure'da Windows sanal makinesi üzerinde MongoDB yapılandırma](https://docs.microsoft.com/azure/virtual-machines/windows/install-mongodb).

> [!NOTE]
> Azure veritabanı geçiş hizmeti çevrimiçi bir geçiş gerçekleştirmek için Premium fiyatlandırma katmanını temel alan bir örneği oluşturmanız gerekir.

> [!IMPORTANT]
> En iyi geçiş deneyimi için Microsoft, hedef veritabanı ile aynı Azure bölgesindeki Azure veritabanı geçiş hizmeti örneği oluşturmayı önerir. Bölgeler ve coğrafyalar için geçerli arasında veri taşıma, geçiş işlemlerini yavaşlatabilir.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Bu makalede, MongoDB için API Azure Cosmos DB'nin MongoDB bir çevrimiçi geçiş açıklanır. Çevrimdışı bir geçiş için bkz: [Azure Cosmos DB'nin MongoDB API'si için MongoDB geçişi DMS kullanarak çevrimdışı](tutorial-mongodb-cosmos-db.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

* [Geçiş öncesi tamamlamak](../cosmos-db/mongodb-pre-migration.md) bölüm anahtarını ve dizin oluşturma ilkesini seçme aktarım hızı, tahmin etme gibi adımları.
* [MongoDB hesabı için bir Azure Cosmos DB'nin API'si oluşturma](https://ms.portal.azure.com/#create/Microsoft.DocumentDB).
* Kullanarak şirket içi kaynak sunucularınıza siteden siteye bağlantı sağlayan, Azure Resource Manager dağıtım modeli kullanarak bir Azure sanal ağı (VNet) için Azure veritabanı geçiş hizmeti oluşturma [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction)veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).

    > [!NOTE]
    > Microsoft Ağ eşlemesi ile ExpressRoute kullanıyorsanız, sanal ağ kurulumu sırasında şu Hizmet Ekle [uç noktaları](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) hangi hizmet sağlanacağı alt ağ için:
    >
    > * Hedef veritabanı uç noktası (örneğin, SQL uç noktası, Cosmos DB uç noktası vb.)
    > * Depolama uç noktası
    > * Service bus uç noktası
    >
    > Azure veritabanı geçiş hizmeti internet bağlantısı olmadığı için bu gerekli bir yapılandırmadır.

* VNet ağ güvenlik grubu (NSG) kuralları aşağıdaki iletişim bağlantı noktalarını engelleme emin olun: 53, 443, 445, 9354 ve 20000 10000. Azure VNet NSG trafik filtreleme hakkında daha fazla ayrıntı için bkz [ağ güvenlik grupları ile ağ trafiğini filtreleme](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
* Varsayılan olarak TCP bağlantı noktası olan 27017 kaynak MongoDB sunucusuna erişmek Azure veritabanı geçiş hizmeti, Windows Güvenlik Duvarı'nı açın.
* Kaynak veritabanlarınız önünde bir güvenlik duvarı Gereci kullanırken oluşan kaynak veritabanları geçiş için erişmek Azure veritabanı geçiş hizmeti izin vermek için güvenlik duvarı kuralları eklemeniz gerekebilir.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme

1. Azure portal'da oturum açın, **Tüm hizmetler** seçeneğini belirleyin ve ardından **Abonelikler**'i seçin.

   ![Portal aboneliklerini gösterme](media/tutorial-mongodb-to-cosmosdb-online/portal-select-subscription1.png)

2. Azure veritabanı geçiş hizmeti örneği oluşturun ve ardından istediğiniz aboneliği seçin **kaynak sağlayıcıları**.

    ![Kaynak sağlayıcılarını gösterme](media/tutorial-mongodb-to-cosmosdb-online/portal-select-resource-provider.png)

3. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydetme](media/tutorial-mongodb-to-cosmosdb-online/portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Örnek oluşturma

1. Azure portalda +**Kaynak oluştur**'u seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve açılan listeden **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

    ![Azure Market](media/tutorial-mongodb-to-cosmosdb-online/portal-marketplace.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında **Oluştur**'u seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğini oluşturma](media/tutorial-mongodb-to-cosmosdb-online/dms-create1.png)
  
3. **Geçiş Hizmeti oluşturun** ekranında hizmet için bir ad belirtin, aboneliği ve yeni ya da var olan bir kaynak grubunu seçin.

4. Azure veritabanı geçiş hizmeti örneğini oluşturmak istediğiniz konumu seçin.

5. Mevcut bir VNet seçin veya yeni bir tane oluşturun.

   Sanal ağ erişim kaynak MongoDB örneği ve ' % s'hedef Azure Cosmos DB hesabı için Azure veritabanı geçiş hizmeti sağlar.

   Azure portalında VNet oluşturma hakkında daha fazla bilgi için bkz [Azure portalını kullanarak bir sanal ağ oluşturma](https://aka.ms/DMSVnet).

6. Bir SKU Premium fiyatlandırma katmanı seçin.

    > [!NOTE]
    > Çevrimiçi geçişler yalnızca Premium katmanda kullanılırken desteklenir. Maliyetler ve fiyatlandırma katmanları hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://aka.ms/dms-pricing) bakın.

    ![Azure Veritabanı Geçiş Hizmeti örneği ayarlarını yapılandırma](media/tutorial-mongodb-to-cosmosdb-online/dms-settings3.png)

7. Hizmeti oluşturmak için **Oluştur**’u seçin.

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma

Hizmet oluşturulduktan sonra Azure portaldan bulun, açın ve yeni bir geçiş projesi oluşturun.

1. Azure portalda **Tüm hizmetler**'i seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

    ![Azure veritabanı geçiş hizmeti tüm örneklerini bulun](media/tutorial-mongodb-to-cosmosdb-online/dms-search.png)

2. Üzerinde **Azure veritabanı geçiş hizmetleri** ekranında, adın Azure veritabanı geçiş hizmeti örneği oluşturduğunuz arayın ve sonra örneği seçin.

    Alternatif olarak, Azure veritabanı geçiş hizmeti örneği Azure portalında arama bölmesinden bulabilir.

    ![Azure portalında arama bölmesini kullanma](media/tutorial-mongodb-to-cosmosdb-online/dms-search-portal.png)

3. +**Yeni Geçiş Projesi**'ni seçin.

4. Üzerinde **yeni geçiş projesi** projesi için bir ad belirtin, ekran **kaynak sunucu türü** metin kutusunda **MongoDB**, **hedef sunucu türü**  metin kutusunda **CosmosDB (MongoDB API'SİYLE)** ve ardından **etkinlik türünü seçin**seçin **çevrimiçi veri geçiş [Önizleme]** .

    ![Veritabanı geçiş hizmeti projesi oluşturma](media/tutorial-mongodb-to-cosmosdb-online/dms-create-project1.png)

5. Seçin **Kaydet**ve ardından **oluşturma ve çalıştırma etkinliğinin** projeyi oluşturmak ve geçiş etkinliği çalıştırmak için.

## <a name="specify-source-details"></a>Kaynak ayrıntılarını belirtme

1. Üzerinde **kaynak ayrıntıları** ekranında, kaynak MongoDB sunucu bağlantı ayrıntılarını belirtin.

    Bir kaynağına bağlanmak için üç mod vardır:
   * **Standart mod**, tam etki alanı adı veya IP adresi, bağlantı noktası numarası ve bağlantı kimlik bilgilerini kabul eden.
   * **Bağlantı dizesi modu**, MongoDB bağlantı dizesi kabul eden makalesinde açıklandığı şekilde [bağlantı dizesi URI biçimi](https://docs.mongodb.com/manual/reference/connection-string/).
   * **Verileri Azure depolama biriminden**, bir blob kapsayıcısı SAS URL'si kabul eder. Seçin **Blob BSON dökümleri içeren** blob kapsayıcısını MongoDB tarafından üretilen BSON dökümleri varsa [bsondump aracı](https://docs.mongodb.com/manual/reference/program/bsondump/)ve JSON dosyaları kapsayıcı içeriyorsa, XML'deki seçin.

     Bu seçeneği belirlerseniz, depolama hesabı bağlantı dizesi şu biçimde göründüğünden emin olun:

     ```
     https://blobnameurl/container?SASKEY
     ```

     Ayrıca, Azure depolama türü döküm bilgilerinde bağlı olarak, aşağıdaki ayrıntıları göz önünde bulundurun.

     * BSON aktarımları için veri dosyalarının biçimi collection.bson içeren veritabanları sonra klasörleri içine yerleştirilir şekilde verileri blob kapsayıcısındaki bsondump biçiminde olmalıdır. Meta veri dosyaları (varsa) adı biçimini kullanarak *koleksiyon*. metadata.json.

     * JSON aktarımları için blob kapsayıcısında dosyalarını içeren veritabanları sonra adlı klasörlere yerleştirilmelidir. Veri dosyaları her veritabanı klasörü içinde yerleştirilmelidir bir alt klasör "veri" olarak adlandırılan ve aşağıdaki biçimi kullanarak adlı *koleksiyon*.json. Meta veri dosyaları (varsa) yerleştirilmelidir bir alt klasör "metadata" olarak adlandırılan ve aynı biçim kullanılarak adlı *koleksiyon*.json. Meta veri dosyaları, MongoDB bsondump araç tarafından üretilen gibi aynı biçimde olması gerekir.

    > [!IMPORTANT]
    > Mongo sunucuda otomatik olarak imzalanan bir sertifika kullanmak için önerilmez. Kullanılır, ancak lütfen kullanarak sunucunuza bağlanmak **bağlantı dize modunun** ve bağlantı dizenizi olduğundan emin olun ""
    >
    >```
    >&sslVerifyCertificate=false
    >```

    IP adresi DNS ad çözümlemesi mümkün olmayan durumlar için kullanabilirsiniz.

   ![Kaynak ayrıntılarını belirtme](media/tutorial-mongodb-to-cosmosdb-online/dms-specify-source1.png)

2. **Kaydet**’i seçin.

   > [!NOTE]
   > Kaynak çoğaltma kümesi ve yönlendirici ise kaynak parçalı bir MongoDB küme ise, kaynak sunucunun adresini birincil adresi olmalıdır. Parçalı bir MongoDB kümesi için Azure veritabanı geçiş hizmeti daha fazla makine üzerinde güvenlik duvarını açmak gerektirebilir kümedeki tek tek parça bağlanabilir olması gerekir.

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme

1. Üzerinde **geçiş hedef ayrıntıları** ekranında, önceden sağlanmış Azure Cosmos DB'nin API geçip MongoDB verilerini geçiş yaptığınız MongoDB hesabı için Azure Cosmos DB hesabı, hedef bağlantı ayrıntılarını belirtin.

    ![Hedef ayrıntılarını belirtme](media/tutorial-mongodb-to-cosmosdb-online/dms-specify-target1.png)

2. **Kaydet**’i seçin.

## <a name="map-to-target-databases"></a>Hedef veritabanlarıyla eşleyin

1. Üzerinde **hedef veritabanlarına eşleme** ekranında, kaynak ve hedef veritabanı geçiş eşleyin.

   Hedef veritabanını kaynak veritabanıyla aynı veritabanı adı içeriyorsa, Azure veritabanı geçiş hizmeti hedef veritabanı varsayılan olarak seçer.

   Dize **Oluştur** gösteren Azure veritabanı geçiş hizmeti, hedef veritabanında bulamadı ve hizmet veritabanı oluşturacaktır veritabanı adının yanında görünür.

   Bu noktada geçiş istiyorsanız veritabanı aktarım hızını paylaşın, aktarım hızı RU belirtin. Cosmos DB'de aktarım hızı ve veritabanı düzeyinde veya tek tek her koleksiyon için sağlayabilirsiniz. Aktarım hızı ölçülür [istek birimi](https://docs.microsoft.com/azure/cosmos-db/request-units) (RU). Daha fazla bilgi edinin [Azure Cosmos DB fiyatlandırma](https://azure.microsoft.com/pricing/details/cosmos-db/).

   ![Hedef veritabanlarıyla eşleyin](media/tutorial-mongodb-to-cosmosdb-online/dms-map-target-databases1.png)

2. **Kaydet**’i seçin.

3. Üzerinde **koleksiyon ayarını** ekran, liste koleksiyonları genişletin ve ardından geçirilecek koleksiyonları listesini gözden geçirin.

   Azure veritabanı geçiş hizmeti otomatik Azure Cosmos DB hesabı hedefte mevcut olmayan kaynak MongoDB örneğinde mevcut tüm koleksiyonlar seçer. Zaten veri içeren koleksiyonlar yeniden geçirmek istiyorsanız, bu ekrandaki koleksiyonlar açıkça seçmeniz gerekir.

   Kullanılacak koleksiyonların istediğiniz RU sayısını belirtebilirsiniz. Çoğu durumda, 500 (1000 minimum parçalı koleksiyonlar için) ve 4000 arasında bir değer yeterli olacaktır. Azure veritabanı geçiş hizmeti, koleksiyon boyutunu temel alan akıllı varsayılanlar önerir.

    > [!NOTE]
    > Gerekirse, çalıştırma hızlandırmak için Azure veritabanı geçiş hizmeti, birden çok örneğini kullanarak paralel koleksiyon ve veritabanı geçiş gerçekleştirin.

   Yararlanmak için bir parça anahtarı belirtebilirsiniz [Azure Cosmos DB'de bölümleme](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview) en iyi ölçeklendirilebilirlik için. Gözden geçirmeyi unutmayın [parça/bölüm anahtarının seçilmesi için en iyi yöntemler](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey). Bir bölüm anahtarı yoksa, her zaman kullanabilirsiniz **_kimliği** daha iyi aktarım hızı için parça anahtarı olarak.

   ![Koleksiyonları tabloları seçin](media/tutorial-mongodb-to-cosmosdb-online/dms-collection-setting1.png)

4. **Kaydet**’i seçin.

5. **Geçiş özeti** ekranının **Etkinlik adı** metin kutusunda geçiş etkinliği için bir ad belirtin.

    ![Geçiş özeti](media/tutorial-mongodb-to-cosmosdb-online/dms-migration-summary1.png)

## <a name="run-the-migration"></a>Geçişi çalıştırma

* **Geçişi çalıştır**'ı seçin.

   Geçiş etkinlik penceresi görüntülenir ve **durumu** etkinliğini görüntülenir.

   ![Etkinlik durumu](media/tutorial-mongodb-to-cosmosdb-online/dms-activity-status1.png)

## <a name="monitor-the-migration"></a>Geçişi izleme

* Geçiş etkinlik ekranında seçin **Yenile** ekranı kadar güncelleştirmek için **durumu** geçişini gösterir olarak **Replaying**.

   > [!NOTE]
   > Veritabanı ve koleksiyon düzeyi geçiş ölçümleri ayrıntılarını almak için etkinlik seçebilirsiniz.

   ![Etkinlik durumu yeniden yürüterek](media/tutorial-mongodb-to-cosmosdb-online/dms-activity-replaying.png)

## <a name="verify-data-in-cosmos-db"></a>Verileri Cosmos DB'de doğrula

1. Kaynak MongoDB veritabanına değişiklikleri yapın.
2. COSMOS DB, verileri bir MongoDB kaynak sunucudan çoğaltılır doğrulamak için bağlanın.

    ![Etkinlik durumu yeniden yürüterek](media/tutorial-mongodb-to-cosmosdb-online/dms-verify-data.png)

## <a name="complete-the-migration"></a>Geçişi tamamlama

* Tüm belgeleri kaynağından COSMOS DB hedefte kullanılabilir olduktan sonra seçin **son** Geçişi tamamlamak için geçiş etkinliğe ilişkin bağlam menüsünden.

    Bu eylem bekleyen tüm değişiklikleri yeniden yürüterek tamamlamak ve geçiş işlemi.

    ![Etkinlik durumu yeniden yürüterek](media/tutorial-mongodb-to-cosmosdb-online/dms-finish-migration.png)

## <a name="post-migration-optimization"></a>Geçiş sonrası en iyi duruma getirme

MongoDB için API Azure Cosmos DB'nin MongoDB veritabanına depolanan veriler geçiş yaptıktan sonra Azure Cosmos DB'ye bağlanmak ve verileri yönetin. Dizin oluşturma ilkesini en iyi duruma getirme gibi diğer geçiş sonrası en iyi duruma getirme adımları uygulayın, varsayılan tutarlılık düzeyini güncelleştirmek veya Azure Cosmos DB hesabınız için genel dağıtımı yapılandırma. Daha fazla bilgi için [geçiş sonrası en iyi duruma getirme](../cosmos-db/mongodb-post-migration.md) makalesi.

## <a name="additional-resources"></a>Ek kaynaklar

* [Cosmos DB hizmet bilgileri](https://azure.microsoft.com/services/cosmos-db/)

## <a name="next-steps"></a>Sonraki adımlar

* Microsoft ek senaryolar için geçiş kılavuzunu gözden [veritabanı Geçiş Kılavuzu](https://datamigration.microsoft.com/).
