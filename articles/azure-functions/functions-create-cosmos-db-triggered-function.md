---
title: "Azure Cosmos DB tarafından tetiklenen bir işlev oluşturma | Microsoft Docs"
description: "Azure İşlevleri kullanarak Azure Cosmos DB’de bir veritabanına veri eklendiğinde çağrılan sunucusuz bir işlev oluşturun."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: bc497d71-75e7-47b1-babd-a060a664adca
ms.service: functions; cosmos-db
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 12/07/2017
ms.author: glenga
ms.custom: 
ms.openlocfilehash: ff0c468179ef7b71151b64426bf2e6701d5032fe
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
---
# <a name="create-a-function-triggered-by-azure-cosmos-db"></a>Azure Cosmos DB tarafından tetiklenen bir işlev oluşturma

Azure Cosmos DB’de veri eklendiğinde veya değiştirildiğinde tetiklenen bir işlev oluşturmayı öğrenin. Azure Cosmos DB hakkında daha fazla bilgi edinmek için bkz. [Azure Cosmos DB: Azure İşlevleri ile sunucusuz veritabanı işlemleri](..\cosmos-db\serverless-computing-database.md).

![Günlüklerde iletiyi görüntüleyin.](./media/functions-create-cosmos-db-triggered-function/quickstart-completed.png)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

+ Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Azure İşlev uygulaması oluşturma

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Ardından, yeni işlev uygulamasında bir işlev oluşturun.

<a name="create-function"></a>

## <a name="create-azure-cosmos-db-trigger"></a>Azure Cosmos DB tetikleyicisi oluşturma

1. İşlev uygulamanızı genişletin ve **İşlevler**'in yanındaki **+** düğmesine tıklayın. Bu, işlev uygulamanızdaki ilk işlevse **Özel işlev**'i seçin. Böylece işlev şablonlarının tamamı görüntülenir.

    ![Azure portalındaki İşlevler hızlı başlangıç sayfası](./media/functions-create-cosmos-db-triggered-function/add-first-function.png)

2. Arama alanına `cosmos` yazıp Azure Cosmos DB tetikleyici şablonunuz için istediğiniz dili seçin.

    ![Azure Cosmos DB tetikleyicisi seçme](./media/functions-create-cosmos-db-triggered-function/select-cosmos-db-trigger-portal.png)

3. Yeni tetikleyiciyi resmin altındaki tabloda belirtilen ayarlarla yapılandırın.

    ![Azure Cosmos DB tarafından tetiklenen işlevi oluşturma](./media/functions-create-cosmos-db-triggered-function/functions-cosmosdb-trigger-settings.png)
    
    | Ayar      | Önerilen değer  | Açıklama                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | **Ad** | Varsayılan | Şablonun önerdiği varsayılan işlev adını kullanın. |
    | **Koleksiyon adı** | Öğeler | İzlenecek koleksiyonun adı. |
    | **Yoksa kira koleksiyonu oluşturun** | İşaretli | Koleksiyon henüz mevcut değil, bu yüzden oluşturun. |
    | **Veritabanı adı** | Görevler | İzlenecek koleksiyonu içeren veritabanının adı. |

4. **Azure Cosmos DB hesap bağlantısı** etiketinin yanından **Yeni**’yi seçin ve mevcut bir Cosmos DB hesabını ya da **+ Yeni oluştur** seçeneğini belirleyin. 
 
    ![Azure Cosmos DB bağlantısını yapılandırma](./media/functions-create-cosmos-db-triggered-function/functions-create-CosmosDB.png)

6. Yeni bir Cosmos DB hesabı oluştururken tabloda belirtilen **Yeni hesap** ayarlarını kullanın.

    | Ayar      | Önerilen değer  | Açıklama                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | **ID** | Veritabanının adı | Azure Cosmos DB veritabanı için benzersiz kimlik  |
    | **API** | SQL (DocumentDB) | Bu konuda belge veritabanı API’si kullanılır.  |
    | **Abonelik** | Azure Aboneliği | Azure Aboneliği  |
    | **Kaynak Grubu** | myResourceGroup |  İşlevi uygulamanızı içeren mevcut kaynak grubunu kullanın. |
    | **Konum**  | WestEurope | İşlev uygulamanıza veya depolanmış belgeleri kullanan diğer uygulamalara yakın olan bir konum seçin.  |

6. Veritabanını oluşturmak için **Tamam**’a tıklayın. Veritabanının oluşturulması birkaç dakika sürebilir. Veritabanı oluşturulduktan sonra, veritabanı bağlantı dizesi bir işlev uygulaması ayarı olarak depolanır. Bu uygulama ayarının adı **Azure Cosmos DB hesap bağlantısına** eklenir. 

7. **Oluştur**’a tıklayarak Azure Cosmos DB tarafından tetiklenen işlevinizi oluşturun. İşlev oluşturulduktan sonra şablon temelli işlev kodu görüntülenir.  

    ![C# dilinde Cosmos DB işlev şablonu](./media/functions-create-cosmos-db-triggered-function/function-cosmosdb-template.png)

    Bu işlev şablonu, günlüklere belge sayısını ve ilk belgenin kimliğini yazar. 

Daha sonra, Azure Cosmos DB hesabınız bağlar ve veritabanında **Görevler** koleksiyonunu oluşturursunuz. 

## <a name="create-the-items-collection"></a>Öğeler koleksiyonunu oluşturma

1. Tarayıcıdaki yeni bir sekmede [Azure portalının](https://portal.azure.com) ikinci bir örneğini açın. 

2. Portalın sol tarafındaki simge çubuğunu genişletin, arama alanına `cosmos` yazıp **Azure Cosmos DB**’yi seçin.

    ![Azure Cosmos DB hizmetini arama](./media/functions-create-cosmos-db-triggered-function/functions-search-cosmos-db.png)

2. Azure Cosmos DB hesabınızı seçin ve ardından **Veri Gezgini**’ni seçin. 
 
3. **Koleksiyonlar** bölümünde **taskDatabase**’i seçip **Yeni Koleksiyon**’u seçin.

    ![Koleksiyon oluşturma](./media/functions-create-cosmos-db-triggered-function/cosmosdb-create-collection.png)

4. **Koleksiyon Ekle** bölümünde, resmin altındaki tabloda gösterilen ayarları kullanın. 
 
    ![taskCollection’ı tanımlama](./media/functions-create-cosmos-db-triggered-function/cosmosdb-create-collection2.png)
 
    | Ayar|Önerilen değer|Açıklama |
    | ---|---|--- |
    | **Veritabanı Kimliği** | Görevler |Yeni veritabanınızın adı. Bu, işlev bağlamanızda tanımlanan adla eşleşmelidir. |
    | **Koleksiyon Kimliği** | Öğeler | Yeni koleksiyonun adı. Bu, işlev bağlamanızda tanımlanan adla eşleşmelidir.  |
    | **Depolama kapasitesi** | Sabit (10 GB)|Varsayılan değeri kullanın. Bu değer, veritabanının depolama kapasitesidir. |
    | **Aktarım hızı** |400 RU| Varsayılan değeri kullanın. Daha sonra gecikme süresini azaltmak isterseniz aktarım hızının ölçeğini artırabilirsiniz. |
    | **[Bölüm anahtarı](../cosmos-db/partition-data.md#design-for-partitioning)** | /kategori|Verileri her bölüme eşit şekilde dağıtan bir bölüm anahtarı. Koleksiyon performansının yüksek olması için doğru bölüm anahtarının seçilmesi önemlidir. | 

1. **Görevler** koleksiyonunu oluşturmak için **Tamam**’a tıklayın. Koleksiyonun oluşturulması biraz zaman alabilir.

İşlev bağlamasında belirtilen koleksiyon oluşturulduktan sonra bu yeni koleksiyona belge ekleyerek işlevi test edebilirsiniz.

## <a name="test-the-function"></a>İşlevi test etme

1. Veri Gezgini’nde yeni **taskCollection** koleksiyonunu genişletin, **Belgeler**’i ve sonra **Yeni Belge**’yi seçin.

    ![taskCollection’da belge oluşturma](./media/functions-create-cosmos-db-triggered-function/create-document-in-collection.png)

2. Yeni belgenin içeriğini aşağıdaki içerikle değiştirip **Kaydet**’i seçin.

        {
            "id": "task1",
            "category": "general",
            "description": "some task"
        }

1. Portalda işlevinizi içeren ilk tarayıcı sekmesine geçin. İşlev günlüklerini genişletin ve yeni belgenin işlevi tetiklediğini doğrulayın. `task1` belge kimliğinin günlüklere yazılıp yazılmadığına bakın. 

    ![Günlüklerde iletiyi görüntüleyin.](./media/functions-create-cosmos-db-triggered-function/functions-cosmosdb-trigger-view-logs.png)

4. (İsteğe bağlı) Belgenize dönerek bir değişiklik yapın ve **Güncelleştir**’e tıklayın. Sonra işlev günlüklerine dönerek güncelleştirmenin işlevi de tetiklediğini doğrulayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB’nizde bir belge eklendiğinde ya da değiştirildiğinde çalışan bir işlev oluşturdunuz.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Azure Cosmos DB tetikleyicileri hakkında daha fazla bilgi için bkz. [Azure İşlevleri için Azure Cosmos DB bağlamaları](functions-bindings-cosmosdb.md).
