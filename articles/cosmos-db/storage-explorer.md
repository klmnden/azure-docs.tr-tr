---
title: Azure Depolama Gezgini’nde Azure Cosmos DB’yi Yönetme
description: Azure Depolama Gezgini’nde Azure Cosmos DB’yi yönetmeyi öğrenin.
Keywords: Azure Cosmos DB, Azure Storage Explorer, MongoDB
services: cosmos-db
documentationcenter: ''
author: Jejiang
manager: omafnan
editor: ''
tags: Azure Cosmos DB
ms.assetid: ''
ms.service: cosmos-db
ms.custom: Azure Cosmos DB active
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/20/2018
ms.author: jejiang
ms.openlocfilehash: 18f580f1eae31c9bf3626e100217467bb48ca881
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="manage-azure-cosmos-db-in-azure-storage-explorer-preview"></a>Azure Depolama Gezgini’nde Azure Cosmos DB’yi Yönetme (Önizleme)

Azure Depolama Gezgini’nde Azure Cosmos DB kullanılması, kullanıcıların Azure Cosmos DB varlıklarını yönetmesine, verileri düzenlemesine, saklı yordamların ve tetikleyicilerin yanı sıra Depolama blob’ları ve kuyrukları gibi diğer Azure varlıklarını güncelleştirmesine imkan tanır. Artık farklı Azure varlıklarını aynı aracı kullanarak tek bir yerde yönetebilirsiniz. Azure Depolama Gezgini şu anda SQL, MongoDB, Graf ve Tablo hesaplarını desteklemektedir.

Bu makalede, Depolama Gezgini’ni kullanarak Azure Cosmos DB’yi yönetmeyi öğrenebilirsiniz.


## <a name="prerequisites"></a>Ön koşullar

SQL API<!--or MongoDB API--> için bir Azure Cosmos DB hesabı. Hesabınız yoksa, [Azure Cosmos DB: .NET ve Azure portalı ile bir SQL API'si web uygulaması derleme](create-sql-api-dotnet.md) bölümünde açıklandığı gibi Azure portalından bir hesap oluşturabilirsiniz.

## <a name="installation"></a>Yükleme

Şuradan en yeni Azure Depolama Gezgini bileşenlerini yükleyin: [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) (Windows, Linux ve MAC sürümü artık desteklenmektedir).

## <a name="connect-to-an-azure-subscription"></a>Bir Azure aboneliğine Bağlanma

1. **Azure Depolama Gezgini**’ni yükledikten sonra, aşağıdaki resimde gösterildiği gibi soldaki **eklenti** simgesine tıklayın:
       
   ![Eklenti simgesi](./media/storage-explorer/plug-in-icon.png)
 
2. **Azure Hesabı Ekle**'yi seçip **Oturum açın**'a tıklayın.

   ![Azure aboneliğine bağlanma](./media/storage-explorer/connect-to-azure-subscription.png)

2. **Azure Oturum Açma** iletişim kutusunda **Oturum aç**’ı seçip Azure kimlik bilgilerinizi girin.

    ![Oturum aç](./media/storage-explorer/sign-in.png)

3. Listeden aboneliğinizi seçip **Uygula**’ya tıklayın.

    ![Uygula](./media/storage-explorer/apply-subscription.png)

    Gezgin bölmesi güncelleştirilir ve seçili abonelikteki hesapları gösterir.

    ![Hesap listesi](./media/storage-explorer/account-list.png)

    **Cosmos DB hesabınızı** Azure aboneliğinize başarıyla bağladınız.

## <a name="connect-to-azure-cosmos-db-by-using-a-connection-string"></a>Bağlantı dizesi kullanarak Azure Cosmos DB’ye bağlanma

Azure Cosmos DB’ye bağlanmanın alternatif yollarından biri, bağlantı dizesi kullanmaktır. Bağlantı dizesi kullanarak bağlanmak için aşağıdaki adımları kullanın.

1. Soldaki ağaçtan **Yerel ve Bağlı**’yı bulun ve **Cosmos DB Hesapları**’na sağ tıklayıp **Cosmos DB’ye bağlan...** seçeneğini belirleyin.

    ![Bağlantı dizesiyle Cosmos DB’ye bağlanma](./media/storage-explorer/connect-to-db-by-connection-string.png)

2. Yalnızca SQL ve Tablo API’sini destekler. API seçin, **Bağlantı Dizesini** yapıştırın, **Hesap etiketi** girin, **İleri**’ye tıklayın ve özeti denetleyin, ardından **Bağlan**’a tıklayarak Azure Cosmos DB hesabına bağlanın. Bağlantı dizesini alma hakkında daha fazla bilgi için bkz. [Bağlantı dizelerini edinme](https://docs.microsoft.com/azure/cosmos-db/manage-account#get-the--connection-string).

    ![Connection-string](./media/storage-explorer/connection-string.png)

## <a name="connect-to-azure-cosmos-db-by-using-local-emulator"></a>Yerel öykünücüyü kullanarak Azure Cosmos DB’ye bağlanma
Öykünücü ile bir Azure Cosmos DB’ye bağlanmak için aşağıdaki adımları uygulayın. Şu anda yalnızca SQL hesabı desteklenmektedir.
1. Öykünücüyü yükleyip başlatın. Öykünücünün nasıl yükleneceği hakkında bilgi için bkz. [Cosmos DB Öykünücüsü](https://docs.microsoft.com/en-us/azure/cosmos-db/local-emulator)
2. Soldaki ağaçtan **Yerel ve Bağlı**’yı bulun ve **Cosmos DB Hesapları**’na sağ tıklayıp **Cosmos DB Öykünücüsüne bağlan...** seçeneğini belirleyin.

    ![Öykünücü ile Cosmos DB’ye bağlanma](./media/storage-explorer/emulator-entry.png)

3. Şu anda yalnızca SQL API’si desteklenmektedir. **Bağlantı Dizesini** yapıştırın, **Hesap etiketi** girin, **İleri**’ye tıklayın ve özeti denetleyin, ardından **Bağlan**’a tıklayarak Azure Cosmos DB hesabına bağlanın. Bağlantı dizesini alma hakkında daha fazla bilgi için bkz. [Bağlantı dizelerini edinme](https://docs.microsoft.com/azure/cosmos-db/manage-account#get-the--connection-string).

    ![Öykünücü ile Cosmos DB’ye bağlan iletişim kutusu](./media/storage-explorer/emulator-dialog.png)



## <a name="azure-cosmos-db-resource-management"></a>Azure Cosmos DB kaynak yönetimi

Bir Azure Cosmos DB hesabını aşağıdaki işlemleri gerçekleştirerek yönetebilirsiniz:
* Azure portalında hesabı açın
* Kaynağı Hhızlı Erişim listesine ekleyin
* Kaynakları arayın ve yenileyin
* Veritabanı oluşturma ve silme
* Koleksiyon oluşturma ve silme
* Belgeleri oluşturun, düzenleyin, silin ve filtreleyin
* Saklı yordamları, tetikleyicileri ve kullanıcı tanımlı işlevleri yönetin

### <a name="quick-access-tasks"></a>Hızlı erişim görevleri

Gezgin bölmesindeki bir aboneliğe sağ tıklayarak birçok hızlı eylem görevi gerçekleştirebilirsiniz:

* Bir Azure Cosmos DB hesabına veya veritabanına sağ tıklayın; **Portalda Aç**’ı seçerek kaynağı Azure portalında, tarayıcıda yönetebilirsiniz.

     ![Portalda açma](./media/storage-explorer/open-in-portal.png)

* Ayrıca, **Hızlı Erişim**’e Azure Cosmos DB hesabı, veritabanı, koleksiyonu ekleyebilirsiniz.
* **Buradan Arayın** özelliği, seçili yol altında anahtar sözcük aramayı etkinleştirir.

    ![buradan arayın](./media/storage-explorer/search-from-here.png) 

### <a name="database-and-collection-management"></a>Veritabanı ve koleksiyon yönetimi
#### <a name="create-a-database"></a>Veritabanı oluşturma 
-   Azure Cosmos DB hesabına sağ tıklayın, **Veritabanı Oluştur**’u seçin, veritabanı adını girin ve işlemi tamamlamak için **Enter** tuşuna basın.
       
    ![Veritabanı oluşturma](./media/storage-explorer/create-database.png) 

#### <a name="delete-a-database"></a>Veritabanı silme
- Veritabanına sağ tıklayın, **Veritabanını Sil**’e tıklayıp açılan pencerede **Evet**’e tıklayın. Veritabanı düğümü silinir ve Azure Cosmos DB hesabı otomatik olarak yenilenir.

    ![Veritabanı silme1](./media/storage-explorer/delete-database1.png)  

    ![Veritabanı silme2](./media/storage-explorer/delete-database2.png) 

#### <a name="create-a-collection"></a>Koleksiyon oluşturma
1. Veritabanınıza sağ tıklayın, **Koleksiyon Oluştur**’a tıklayın ve aşağıdaki **Koleksiyon Kimliği**, **Depolama kapasitesi**, vb. bilgileri sağlayın. Ayarlamayı bitirmek için **Tamam**'a tıklayın. 

    ![Koleksiyon oluşturma1](./media/storage-explorer/create-collection.png)

    ![Koleksiyon oluşturma2](./media/storage-explorer/create-collection2.png) 

2. Bölüm anahtarını belirtebilmek için **Sınırsız**’ı seçin ve sonra **Tamam**’a tıklayarak işlemi tamamlayın.

    Bir koleksiyon oluşturulurken bölüm anahtarı kullanılırsa, oluşturma işlemi tamamlandıktan sonra koleksiyondaki bölüm anahtarı değeri değiştirilemez. Bölüm anahtarı ayarlarıyla ilgili bilgi için bkz. [Bölümleme için tasarlama](partition-data.md#designing-for-partitioning).

    ![Bölüm anahtarı](./media/storage-explorer/partitionkey.png)

#### <a name="delete-a-collection"></a>Koleksiyon silme
- Koleksiyona sağ tıklayıp **Koleksiyonu Sil**’e tıklayın ve açılan pencerede **Evet**’e tıklayın. 

    Koleksiyon düğümü silinir ve veritabanı otomatik olarak yenilenir.

    ![Koleksiyonu silme](./media/storage-explorer/delete-collection.png) 

### <a name="document-management"></a>Belge yönetimi

#### <a name="create-and-modify-documents"></a>Belge oluşturma ve değiştirme
- Yeni bir belge oluşturmak için soldaki pencerede **Belgeler**’i açın, **Yeni Belge**’ye tıklayın, sağ bölmede içeriği düzenleyin ve **Kaydet**’e tıklayın. Ayrıca, mevcut bir belgeyi güncelleştirip **Kaydet**’e tıklayabilirsiniz. Değişiklikler **At** seçeneğine tıklanarak atılabilir.

    ![Belge](./media/storage-explorer/document.png)

#### <a name="delete-a-document"></a>Bir belgeyi silme
- Seçili belgeyi silmek için **Sil** düğmesine tıklayın.

#### <a name="query-for-documents"></a>Belgeler için sorgu
- Bir [SQL sorgusu](sql-api-sql-query.md) girip **Uygula**’ya tıklayarak belge filtresini düzenleyin.

    ![Belge Filtresi](./media/storage-explorer/document-filter.png)



### <a name="graph-management"></a>Graf yönetimi

#### <a name="create-and-modify-vertex"></a>Köşe oluşturma ve değiştirme
1. Yeni bir köşe oluşturmak için sol pencereden **Graf**’ı açın, **Yeni Köşe**’ye tıklayın, ardından **Tamam**’a tıklayın.    
2. Mevcut bir köşeyi değiştirmek için sağ bölmede kalem simgesine tıklayın.   

    ![Graf](./media/storage-explorer/vertex.png)

#### <a name="delete-a-graph"></a>Graf silme
- Bir köşeyi silmek için köşe adının yanındaki geri dönüşüm kutusu simgesine tıklayın.

#### <a name="filter-for-graph"></a>Grafik filtresi
- Bir [gremlin sorgusu](gremlin-support.md) girerek grafik filtresini düzenleyin ve sonra **Filtreyi Uygula**’ya tıklayın.

    ![Graf Filtresi](./media/storage-explorer/graph-filter.png)

### <a name="table-management"></a>Tablo yönetimi

#### <a name="create-and-modify-table"></a>Tablo oluşturma ve değiştirme
1. Yeni bir tablo oluşturmak için, sol pencereden **Varlıklar**’ı açın, **Ekle**’ye tıklayın, **Varlık Ekle** iletişim kutusundaki içeriği düzenleyin, **Özellik Ekle** düğmesine tıklayarak özellik ekleyin ve sonra **Ekle**’ye tıklayın.
2. Bir tabloyu değiştirmek için **Düzenle**’ye tıklayın, içeriği değiştirin ve sonra **Güncelleştir**’e tıklayın.

    ![Tablo](./media/storage-explorer/table.png)

#### <a name="import-and-export-table"></a>Tabloyu içeri ve dışarı aktarma
1. İçeri aktarmak için, **İçeri Aktar** düğmesine tıklayın ve mevcut bir tabloyu seçin.
2. Dışarı aktarmak için, **Dışarı Aktar** düğmesine tıklayın ve bir hedef seçin.

    ![Tablo İçeri ve Dışarı Aktarma](./media/storage-explorer/table-import-export.png)

#### <a name="delete-entities"></a>Varlıkları silme
- Varlıkları seçin ve **Sil** düğmesine tıklayın.

    ![Tablo silme](./media/storage-explorer/table-delete.png)

#### <a name="query-table"></a>Sorgu tablosu
- **Sorgu** düğmesine tıklayın, sorgu koşulunu girin ve sonra **Sorguyu Yürüt** düğmesine tıklayın. **Sorguyu Kapat** düğmesine tıklayarak Sorgu bölmesini kapatın.

    ![Tablo Sorgusu](./media/storage-explorer/table-query.png)

### <a name="manage-stored-procedures-triggers-and-udfs"></a>Saklı yordamları, tetikleyicileri ve UDF'leri yönetme
* Saklı yordam oluşturmak için soldaki ağaçta **Saklı Yordam**’a sağ tıklayın, **Saklı Yordam Oluştur**’u seçin, sol tarafa bir ad girin, sağdaki pencerede saklı yordam betiklerini yazın ve sonra **Oluştur**’a tıklayın. 
* Ayrıca, mevcut saklı yordamlara çift tıklayıp güncelleştirmeyi yaptıktan sonra değişikliğinizi **Güncelleştir**’e tıklayarak kaydedebilir veya **At**’a tıklayarak atabilirsiniz.

    ![Saklı yordam](./media/storage-explorer/stored-procedure.png)
* **Tetikleyicilere** ve **UDF**’ye yönelik işlemler, **Saklı Yordamlara** benzer.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Depolama Gezgini’nde Azure Cosmos DB’yi kullanmayı öğrenmek için şu videoyu izleyin: [Azure Depolama Gezgini’nde Azure Cosmos DB kullanma](https://www.youtube.com/watch?v=iNIbg1DLgWo&feature=youtu.be).
* [Depolama Gezgini (Önizleme) ile çalışmaya başlama](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) konusunda Depolama Gezgini hakkında daha fazla bilgi edinin ve daha fazla hizmet bağlayın.

