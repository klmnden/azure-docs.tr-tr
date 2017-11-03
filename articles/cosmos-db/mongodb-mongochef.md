---
title: "Azure Cosmos DB MongoChef kullanın | Microsoft Docs"
description: "Bir Azure Cosmos DB ile MongoChef kullanmayı öğrenin: API MongoDB hesabı"
keywords: mongochef
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: anhoh
ms.openlocfilehash: 54c9799bd646b827f602e2ea2f9a15a4fc853f00
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-mongochef-with-an-azure-cosmos-db-api-for-mongodb-account"></a>Bir Azure Cosmos DB ile MongoChef kullanın: API MongoDB hesabı

Bir Azure Cosmos DB'ye bağlanmasına: API MongoDB hesabı için yapmanız gerekir:

* İndirme ve yükleme [MongoChef](http://3t.io/mongochef)
* Azure Cosmos DB sahip: API MongoDB hesabı için [bağlantı dizesi](connect-mongodb-account.md) bilgileri

## <a name="create-the-connection-in-mongochef"></a>İçinde MongoChef bağlantı oluşturma
Azure Cosmos DB eklemek için: API MongoDB hesabına MongoChef Bağlantı Yöneticisi için aşağıdaki adımları gerçekleştirin.

1. Azure Cosmos DB almak: API MongoDB bağlantı bilgilerini yönergeleri kullanarak [burada](connect-mongodb-account.md).

    ![Bağlantı dizesi dikey penceresi ekran görüntüsü](./media/mongodb-mongochef/ConnectionStringBlade.png)
2. Tıklatın **Bağlan** Bağlantı Yöneticisi'ni açmak için ardından **yeni bağlantı**

    ![MongoChef Bağlantı Yöneticisi ekran görüntüsü](./media/mongodb-mongochef/ConnectionManager.png)
3. İçinde **yeni bağlantı** penceresi, **Server** sekmesinde, Azure Cosmos DB ana bilgisayar (FQDN) girin: API MongoDB hesabı ve bağlantı noktası.

    ![MongoChef Bağlantı Yöneticisi'ni sunucu sekmesinin Ekran görüntüsü](./media/mongodb-mongochef/ConnectionManagerServerTab.png)
4. İçinde **yeni bağlantı** penceresi, **kimlik doğrulaması** sekmesinde, kimlik doğrulama modu seçin **standart (CR MONGODB veya SCARM-SHA-1)** ve kullanıcı adı ve parola girin.  Varsayılan kimlik doğrulama db (Yönetici) kabul edin veya kendi değer sağlayın.

    ![MongoChef Bağlantı Yöneticisi kimlik doğrulaması sekmesinin Ekran görüntüsü](./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. İçinde **yeni bağlantı** penceresi, **SSL** sekmesi, onay **bağlanmak için SSL kullan Protokolü** onay kutusunu ve **sunucu otomatik olarak imzalanan SSL sertifikalarını kabul et**  radyo düğmesi.

    ![MongoChef Bağlantı Yöneticisi SSL sekmesinin Ekran görüntüsü](./media/mongodb-mongochef/ConnectionManagerSSLTab.png)
6. Tıklatın **Bağlantıyı Sına** bağlantı bilgilerini doğrulamak için düğmesi **Tamam** yeni bağlantı penceresine geri dönün ve ardından **kaydetmek**.

    ![MongoChef test bağlantısı penceresinin ekran görüntüsü](./media/mongodb-mongochef/TestConnectionResults.png)

## <a name="use-mongochef-to-create-a-database-collection-and-documents"></a>Bir veritabanı, koleksiyon ve belge oluşturmak için MongoChef kullanın
Bir veritabanı, koleksiyon ve MongoChef kullanarak belgeleri oluşturmak için aşağıdaki adımları gerçekleştirin.

1. İçinde **Bağlantı Yöneticisi**, bağlantı vurgulayıp **Bağlan**.

    ![MongoChef Bağlantı Yöneticisi ekran görüntüsü](./media/mongodb-mongochef/ConnectToAccount.png)
2. Konağı sağ tıklatın ve seçin **veritabanı ekleme**.  Bir veritabanı adı girin ve tıklayın **Tamam**.

    ![MongoChef veritabanı ekleme seçeneği ekran görüntüsü](./media/mongodb-mongochef/AddDatabase1.png)
3. Veritabanını sağ tıklatın ve seçin **topluluk Ekle**.  Koleksiyon adı sağlayın ve tıklatın **oluşturma**.

    ![MongoChef topluluk Ekle seçeneğini ekran görüntüsü](./media/mongodb-mongochef/AddCollection.png)
4. Tıklatın **koleksiyonu** menü öğesi, ardından **Belge Ekle**.

    ![MongoChef Belge Ekle menü öğesi ekran görüntüsü](./media/mongodb-mongochef/AddDocument1.png)
5. Belge Ekle iletişim kutusunda, aşağıdaki yapıştırın ve ardından **Belge Ekle**.

        {
        "_id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
               { "firstName": "Thomas" },
               { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "isRegistered": true
        }
6. Başka bir belge, bu süre aşağıdaki içeriğe sahip ekleyin.

        {
        "_id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam",
                 "givenName": "Jesse",
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            {
                "familyName": "Miller",
                 "givenName": "Lisa",
                 "gender": "female",
                 "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
        }
7. Bir örnek sorgu yürütün. Örneğin, ailesi Soyadı 'Andersen' ile arayın ve üst ve durum alanları döndürür.

    ![Mongo Chef sorgu sonuçları ekran görüntüsü](./media/mongodb-mongochef/QueryDocument1.png)

## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB keşfedin: API MongoDB için [örnekleri](mongodb-samples.md).
