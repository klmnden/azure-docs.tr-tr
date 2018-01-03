---
title: "Azure Storage Gezgini DB Azure Cosmos yönetme"
description: "Azure Storage Gezgini Azure Cosmos DB'de yönetmeyi öğrenin."
Keywords: Azure Cosmos DB, Azure Storage Explorer, DocumentDB, MongoDB, DocumentDB
services: cosmos-db
documentationcenter: 
author: Jiaj-Li
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
ms.author: Jiaj-Li
ms.openlocfilehash: 303fcfbda1934e3b29cb8ed06087c560275489e0
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="manage-azure-cosmos-db-in-azure-storage-explorer-preview"></a>Azure Depolama Gezgini (Önizleme) Azure Cosmos DB yönetme

Azure Storage Gezgini Azure Cosmos DB kullanarak kullanıcıların Azure Cosmos DB varlıkları yönetme, verileri işlemek, saklı yordamları ve Tetikleyicileri depolama bloblar ve kuyruklarda olduğu gibi Azure diğer varlıklar yanı sıra güncelleştirme olanak tanır. Şimdi tek bir yerde farklı Azure varlıklarınızı yönetmek için aynı aracı kullanabilirsiniz. Şu anda Azure Storage Gezgini SQL (DocumentDB) ve MongoDB hesaplarını destekler.

Bu makalede, Depolama Gezgini Azure Cosmos DB yönetmek için nasıl kullanılacağını öğrenebilirsiniz.


## <a name="prerequisites"></a>Ön koşullar

Bir SQL (DocumentDB) veya MongoDB veritabanı için bir Azure Cosmos DB hesap. Bir hesabınız yoksa, bir Azure Portalı'nda açıklandığı gibi oluşturabilirsiniz [Azure Cosmos DB: .NET ve Azure portal ile bir DocumentDB API web uygulaması oluşturma](create-documentdb-dotnet.md).

## <a name="installation"></a>Yükleme

Burada yeni Azure Storage Gezgini BITS uygulamasını yüklemeniz: [Azure Storage Gezgini](https://azure.microsoft.com/features/storage-explorer/), Windows, Linux ve MAC sürüm destekliyoruz artık.

## <a name="connect-to-an-azure-subscription"></a>Bir Azure aboneliğine Bağlanma

1. Yükledikten sonra **Azure Storage Gezgini**, tıklatın **eklenti** aşağıdaki görüntüde gösterildiği gibi soldaki simgesi.
       
   ![Simgesine takın](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/plug-in-icon.png)
 
2. Seçin **Azure Hesap Ekle**ve ardından **oturum açma**.

   ![Azure aboneliğine bağlanma](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/connect-to-azure-subscription.png)

2. İçinde **Azure oturum açma** iletişim kutusunda **oturum**ve ardından Azure kimlik bilgilerinizi girin.

    ![Oturum aç](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/sign-in.png)

3. Listeden aboneliğinizi seçin ve ardından **Uygula**.

    ![Başvurun](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/apply-subscription.png)

    Gezgin bölmesini güncelleştirir ve hesapları seçilen abonelikte görüntüler.

    ![Hesap listesi](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/account-list.png)

    Başarılı bir şekilde bağlı, **Azure Cosmos DB hesap** Azure aboneliğinize.

## <a name="connect-to-azure-cosmos-db-by-using-a-connection-string"></a>Bir bağlantı dizesi kullanarak Azure Cosmos Veritabanına bağlanın

Bir Azure Cosmos DB bağlayan alternatif bir yolu, bir bağlantı dizesi kullanmaktır. Bir bağlantı dizesi kullanarak bağlanmak için aşağıdaki adımları kullanın.

1. Bul **yerel ve iliştirildiği** sol ağacında sağ **Azure Cosmos DB hesapları**, seçin **Azure Cosmos DB Bağlan...**

    ![Bağlantı dizesi tarafından Azure Cosmos Veritabanına bağlanın](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/connect-to-db-by-connection-string.png)

2. Uygun seçin **varsayılan deneyimi** hesap türünüz için ya da **DocumentDB** veya **MongoDB**, yapıştırın, **bağlantı dizesi**ve ardından **Tamam** Azure Cosmos DB hesap bağlanmak için. Bağlantı dizesi alma hakkında daha fazla bilgi için bkz: [bağlantı dizesini almak](https://docs.microsoft.com/en-us/azure/cosmos-db/manage-account#get-the--connection-string).

    ![bağlantı dizesi](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/connection-string.png)

## <a name="azure-cosmos-db-resource-management"></a>Azure Cosmos DB kaynak yönetimi

Aşağıdaki işlemleri yaparak bir Azure Cosmos DB hesap yönetebilirsiniz:
* Azure portalında hesabını açın
* Kaynak hızlı erişim listeye ekleyin
* Arama ve yenileme kaynakları
* Veritabanı oluşturma ve silme
* Koleksiyon oluşturma ve silme
* Oluşturma, düzenleme, silme ve belgelere filtre
* Saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler yönetme

### <a name="quick-access-tasks"></a>Hızlı erişim görevleri

Bir abonelikte Gezgini bölmesinde sağ tıklayarak pek çok hızlı eylem görevleri gerçekleştirebilirsiniz:

* Seçebileceğiniz Azure Cosmos DB hesabı veya bir veritabanını sağ tıklatın **portalında açık** ve Azure Portal'da tarayıcıda kaynak yönetin.

     ![Portalı'nda Aç](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/open-in-portal.png)

* Ayrıca ekleyebilirsiniz Azure Cosmos DB hesap, veritabanı, koleksiyon için **hızlı erişim**.
* **Arama buradan** seçilen yolu altında anahtar sözcük aramasını etkinleştirir.

    ![Buradan arama](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/search-from-here.png) 

### <a name="database-and-collection-management"></a>Veritabanı ve koleksiyonu Yönetimi
#### <a name="create-a-database"></a>Veritabanı oluşturma 
Azure Cosmos DB hesabına sağ tıklayın, seçin **Create Database**, giriş veritabanı adı ve tuşuna **Enter** tamamlamak için.

![Veritabanı oluşturma](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/create-database.png) 

#### <a name="delete-a-database"></a>Bir veritabanını silin
Veritabanına sağ tıklayın, **Sil veritabanı**, tıklatıp **Evet** açılır pencerede. Veritabanı düğüm silinir ve Azure Cosmos DB hesabı otomatik olarak yeniler.

![Database1 Sil](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/delete-database1.png)  

![Database2 Sil](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/delete-database2.png) 

#### <a name="create-a-collection"></a>Koleksiyon oluşturma
Veritabanınıza sağ tıklayın, seçin **Koleksiyonu Oluştur**ve ardından aşağıdaki gibi bilgiler **koleksiyon kimliği**, **depolama kapasitesi**, vb. Ayarlamayı bitirmek için **Tamam**'a tıklayın. Bölüm anahtarı ayarları hakkında daha fazla bilgi için bkz: [bölümleme için tasarım](partition-data.md#designing-for-partitioning).

Bölüm anahtarı oluşturma tamamlandıktan sonra bir koleksiyon oluştururken kullanılırsa, koleksiyonda bölüm anahtar değeri değiştirilemez.

![Collection1 oluşturma](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/create-collection.png)

![Collection2 oluşturma](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/create-collection2.png) 

#### <a name="delete-a-collection"></a>Bir koleksiyonu silin
Koleksiyona sağ tıklayın, **Koleksiyonu Sil'i**ve ardından **Evet** açılır pencerede. 

Koleksiyon düğümüyle silinir ve veritabanı otomatik olarak yeniler.  

![Koleksiyonunu silin](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/delete-collection.png) 

### <a name="document-management"></a>Belge Yönetimi

#### <a name="create-and-modify-documents"></a>Oluşturma ve belgeleri değiştirme
Yeni bir belge oluşturmak için açık **belgeleri** sol penceresinde **yeni belge**, sağ bölmede içeriğini düzenleyin ve ardından **kaydetmek**. Ayrıca var olan bir belgeyi güncelleştirin ve ardından **kaydetmek**. Değişiklikleri atılmasını gerektirebilir tıklayarak **atmak**.

![Belge](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/document.png)

#### <a name="delete-a-document"></a>Bir belgeyi silme
Tıklatın **silmek** seçili dosyayı silmek için düğmesini.
#### <a name="query-for-documents"></a>Belgeler için sorgu
Belge Filtresi girerek düzenleyin bir [SQL sorgusu](documentdb-sql-query.md) ve ardından **Uygula**.

![Filtre](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/filter.png)

### <a name="manage-stored-procedures-triggers-and-udfs"></a>Saklı yordamlar, tetikleyiciler ve UDF'lerin yönetme
* Sol ağacında bir saklı yordam oluşturmak için **saklı yordam**, seçin **saklı yordam oluşturma**, sol tarafta bir ad girin, doğru penceresinde saklı yordam komut yazın ve ardından tıklatın **oluşturma**. 
* Var olan saklı yordamları çift güncelleştirme yapma ve ardından düzenleyebilirsiniz **güncelleştirme** tıklayın veya kaydetmek **atmak** değişikliği iptal etmek için.

![Saklı yordam](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/stored-procedure.png)

* İşlemler için **Tetikleyicileri** ve **UDF** için kullanılanlara benzerdir **saklı yordamlar**.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Storage Gezgini Azure Cosmos DB kullanılması hakkında bilgi için aşağıdaki videoyu izleyin: [kullanmak Azure Cosmos DB'de Azure Storage Gezgini](https://www.youtube.com/watch?v=iNIbg1DLgWo&feature=youtu.be).
* Depolama Gezgini hakkında daha fazla bilgi ve daha fazla hizmet bağlanmak [Depolama Gezgini (Önizleme) ile çalışmaya başlama](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-manage-with-storage-explorer).

