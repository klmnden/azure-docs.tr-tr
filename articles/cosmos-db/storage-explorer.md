---
title: "Azure Depolama Gezgini’nde Azure Cosmos DB’yi Yönetme"
description: "Azure Depolama Gezgini’nde Azure Cosmos DB’yi yönetmeyi öğrenin."
Keywords: Azure Cosmos DB, Azure Storage Explorer, MongoDB
services: cosmos-db
documentationcenter: 
author: jejiang
manager: omafnan
editor: 
tags: Azure Cosmos DB
ms.assetid: 
ms.service: cosmos-db
ms.custom: Azure Cosmos DB active
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/19/2017
ms.author: Jejiang
ms.openlocfilehash: baa7eee614159f9c6af493aff74b27773471f7d7
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="manage-azure-cosmos-db-in-azure-storage-explorer-preview"></a>Azure Depolama Gezgini’nde Azure Cosmos DB’yi Yönetme (Önizleme)

Azure Depolama Gezgini’nde Azure Cosmos DB kullanılması, kullanıcıların Azure Cosmos DB varlıklarını yönetmesine, verileri düzenlemesine, saklı yordamların ve tetikleyicilerin yanı sıra Depolama blob’ları ve kuyrukları gibi diğer Azure varlıklarını güncelleştirmesine imkan tanır. Artık farklı Azure varlıklarını aynı aracı kullanarak tek bir yerde yönetebilirsiniz. Azure Depolama Gezgini şu anda SQL <!--and MongoDB--> hesaplarını desteklemektedir. Azure Depolama Gezgini, Azure Cosmos DB Yerel Öykünücüsü ile çalışmaz. 

Bu makalede, Depolama Gezgini’ni kullanarak Azure Cosmos DB’yi yönetmeyi öğrenebilirsiniz.


## <a name="prerequisites"></a>Ön koşullar

SQL API <!--or MongoDB API--> için bir Azure Cosmos DB hesabı. Hesabınız yoksa, [Azure Cosmos DB: .NET ve Azure portalı ile bir SQL API'si web uygulaması derleme](create-sql-api-dotnet.md) bölümünde açıklandığı gibi Azure portalından bir hesap oluşturabilirsiniz.

## <a name="installation"></a>Yükleme

Şuradan en yeni Azure Depolama Gezgini bileşenlerini yükleyin: [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) (Windows, Linux ve MAC sürümü artık desteklenmektedir).

## <a name="connect-to-an-azure-subscription"></a>Bir Azure aboneliğine Bağlanma

1. **Azure Depolama Gezgini**’ni yükledikten sonra, aşağıdaki resimde gösterildiği gibi soldaki **eklenti** simgesine tıklayın.
       
   ![Eklenti simgesi](./media/storage-explorer/plug-in-icon.png)
 
2. **Azure Hesabı Ekle**'yi seçip **Oturum açın**'a tıklayın.

   ![Azure aboneliğine bağlanma](./media/storage-explorer/connect-to-azure-subscription.png)

2. **Azure Oturum Açma** iletişim kutusunda **Oturum aç**’ı seçip Azure kimlik bilgilerinizi girin.

    ![Oturum aç](./media/storage-explorer/sign-in.png)

3. Listeden aboneliğinizi seçip **Uygula**’ya tıklayın.

    ![Uygula](./media/storage-explorer/apply-subscription.png)

    Gezgin bölmesi güncelleştirilir ve seçili abonelikteki hesapları gösterir.

    ![Hesap listesi](./media/storage-explorer/account-list.png)

    **Azure Cosmos DB hesabınızı** Azure aboneliğinize başarıyla bağladınız.

## <a name="connect-to-azure-cosmos-db-by-using-a-connection-string"></a>Bağlantı dizesi kullanarak Azure Cosmos DB’ye bağlanma

Azure Cosmos DB’ye bağlanmanın alternatif yollarından biri, bağlantı dizesi kullanmaktır. Bağlantı dizesi kullanarak bağlanmak için aşağıdaki adımları kullanın.

1. Soldaki ağaçtan **Yerel ve Bağlı**’yı bulun ve **Azure Cosmos DB Hesapları**’na sağ tıklayıp **Azure Cosmos DB’ye bağlan...** seçeneğini belirleyin.

    ![Bağlantı dizesiyle Azure Cosmos DB’ye bağlanma](./media/storage-explorer/connect-to-db-by-connection-string.png)

2. Azure Cosmos DB hesabına bağlanmak için Hesap türünüz olarak uygun **Varsayılan Deneyim**’i (<!--either--> **DocumentDB** <!--or **MongoDB**-->) seçin, **Bağlantı Dizenizi** yapıştırın ve **Tamam**’a tıklayın. Bağlantı dizesini alma hakkında daha fazla bilgi için bkz. [Bağlantı dizelerini edinme](https://docs.microsoft.com/azure/cosmos-db/manage-account#get-the--connection-string).

    ![Connection-string](./media/storage-explorer/connection-string.png)

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
Azure Cosmos DB hesabına sağ tıklayın, **Veritabanı Oluştur**’u seçin, veritabanı adını girin ve işlemi tamamlamak için **Enter** tuşuna basın.

![Veritabanı oluşturma](./media/storage-explorer/create-database.png) 

#### <a name="delete-a-database"></a>Veritabanı silme
Veritabanına sağ tıklayın, **Veritabanını Sil**’e tıklayıp açılan pencerede **Evet**’e tıklayın. Veritabanı düğümü silinir ve Azure Cosmos DB hesabı otomatik olarak yenilenir.

![Veritabanı silme1](./media/storage-explorer/delete-database1.png)  

![Veritabanı silme2](./media/storage-explorer/delete-database2.png) 

#### <a name="create-a-collection"></a>Koleksiyon oluşturma
Veritabanınıza sağ tıklayın, **Koleksiyon Oluştur**’a tıklayın ve aşağıdaki **Koleksiyon Kimliği**, **Depolama kapasitesi**, vb. bilgileri sağlayın. Ayarlamayı bitirmek için **Tamam**'a tıklayın. Bölüm anahtarı ayarlarıyla ilgili bilgi için bkz. [Bölümleme için tasarlama](partition-data.md#designing-for-partitioning).

Bir koleksiyon oluşturulurken bölüm anahtarı kullanılırsa, oluşturma işlemi tamamlandıktan sonra koleksiyondaki bölüm anahtarı değeri değiştirilemez.

![Koleksiyon oluşturma1](./media/storage-explorer/create-collection.png)

![Koleksiyon oluşturma2](./media/storage-explorer/create-collection2.png) 

#### <a name="delete-a-collection"></a>Koleksiyon silme
Koleksiyona sağ tıklayıp **Koleksiyonu Sil**’e tıklayın ve açılan pencerede **Evet**’e tıklayın. 

Koleksiyon düğümü silinir ve veritabanı otomatik olarak yenilenir.  

![Koleksiyonu silme](./media/storage-explorer/delete-collection.png) 

### <a name="document-management"></a>Belge yönetimi

#### <a name="create-and-modify-documents"></a>Belge oluşturma ve değiştirme
Yeni bir belge oluşturmak için soldaki pencerede **Belgeler**’i açın, **Yeni Belge**’ye tıklayın, sağ bölmede içeriği düzenleyin ve **Kaydet**’e tıklayın. Ayrıca, mevcut bir belgeyi güncelleştirip **Kaydet**’e tıklayabilirsiniz. Değişiklikler **At** seçeneğine tıklanarak atılabilir.

![Belge](./media/storage-explorer/document.png)

#### <a name="delete-a-document"></a>Bir belgeyi silme
Seçili belgeyi silmek için **Sil** düğmesine tıklayın.
#### <a name="query-for-documents"></a>Belgeler için sorgu
Bir [SQL sorgusu](sql-api-sql-query.md) girip **Uygula**’ya tıklayarak belge filtresini düzenleyin.

![Filtre](./media/storage-explorer/filter.png)

### <a name="manage-stored-procedures-triggers-and-udfs"></a>Saklı yordamları, tetikleyicileri ve UDF'leri yönetme
* Saklı yordam oluşturmak için soldaki ağaçta **Saklı Yordam**’a sağ tıklayın, **Saklı Yordam Oluştur**’u seçin, sol tarafa bir ad girin, sağdaki pencerede saklı yordam betiklerini yazın ve sonra **Oluştur**’a tıklayın. 
* Ayrıca, mevcut saklı yordamlara çift tıklayıp güncelleştirmeyi yaptıktan sonra değişikliğinizi **Güncelleştir**’e tıklayarak kaydedebilir veya **At**’a tıklayarak atabilirsiniz.

![Saklı yordam](./media/storage-explorer/stored-procedure.png)

* **Tetikleyicilere** ve **UDF**’ye yönelik işlemler, **Saklı Yordamlara** yönelik işlemlere benzer.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Depolama Gezgini’nde Azure Cosmos DB’yi kullanmayı öğrenmek için şu videoyu izleyin: [Azure Depolama Gezgini’nde Azure Cosmos DB kullanma](https://www.youtube.com/watch?v=iNIbg1DLgWo&feature=youtu.be).
* [Depolama Gezgini (Önizleme) ile çalışmaya başlama](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) konusunda Depolama Gezgini hakkında daha fazla bilgi edinin ve daha fazla hizmet bağlayın.

