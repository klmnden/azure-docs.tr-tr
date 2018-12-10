---
title: Studio 3T kullanma MongoDB hesabınıza bağlanma (MongoChef)
titleSuffix: Azure Cosmos DB
description: Studio 3t'yi kullanarak Azure Cosmos DB MongoDB API'SİNDE bağlama ve bağlandıktan sonra bir veritabanı, koleksiyon ve belge oluşturma işlemini öğrenin.
keywords: mongochef, studio 3T
services: cosmos-db
author: slyons
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: sclyon
ms.custom: seodec18
ms.openlocfilehash: 5bdcf035f892f1cbdb8bb43579dba547f0ec8bfd
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53135670"
---
# <a name="connect-to-mongodb-account-using-studio-3t-mongochef"></a>Studio 3T kullanma MongoDB hesabınıza bağlanma (MongoChef)

Bir Azure Cosmos DB MongoDB API hesabına bağlanmak için şunları yapmalısınız:

* İndirme ve yükleme [Studio 3T](https://studio3t.com/) (eski adıyla MongoChef da bilinir)
* Azure Cosmos DB sahip [bağlantı dizesi](connect-mongodb-account.md) MongoDB hesabı için bilgiler

## <a name="create-the-connection-in-studio-3t"></a>Studio 3t'yi bağlantı oluşturma
Studio 3T Bağlantı Yöneticisi için Azure Cosmos DB hesabınıza eklemek için aşağıdaki adımları gerçekleştirin:

1. İçindeki yönergeleri kullanarak MongoDB API hesabı için Azure Cosmos DB bağlantı bilgilerini almak [bir MongoDB uygulamasını Azure Cosmos DB'ye bağlanmak](connect-mongodb-account.md) makalesi.

    ![Bağlantı dizesi sayfası ekran görüntüsü](./media/mongodb-mongochef/ConnectionStringBlade.png)
2. Tıklayın **Connect** Bağlantı Yöneticisi'ni açmak için ardından **yeni bağlantı**

    ![Studio 3T Bağlantı Yöneticisi ekran görüntüsü](./media/mongodb-mongochef/ConnectionManager.png)
3. İçinde **yeni bağlantı** penceresi, **sunucu** sekmesinde, Azure Cosmos DB hesabı ana bilgisayar (FQDN) ve bağlantı NOKTASINI girin.

    ![Studio 3T Bağlantı Yöneticisi server sekmesinin Ekran görüntüsü](./media/mongodb-mongochef/ConnectionManagerServerTab.png)
4. İçinde **yeni bağlantı** penceresi, **kimlik doğrulaması** sekmesinde, kimlik doğrulama modu seçme **temel (MONGODB CR veya SCARM-SHA-1)** kullanıcı adı ve parola girin.  Varsayılan kimlik doğrulaması db (Yönetici) kabul edin veya kendi değer sağlayın.

    ![Studio 3T Bağlantı Yöneticisi kimlik doğrulaması sekmesinin Ekran görüntüsü](./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. İçinde **yeni bağlantı** penceresi, **SSL** sekmesinde, onay **bağlanmak için SSL kullan Protokolü** onay kutusunu ve **sunucu otomatik olarak imzalanan SSL sertifikalarını kabul et**  radyo düğmesi.

    ![Studio 3T Bağlantı Yöneticisi SSL sekmesinin Ekran görüntüsü](./media/mongodb-mongochef/ConnectionManagerSSLTab.png)
6. Tıklayın **Test Bağlantısı** bağlantı bilgilerini doğrulamak için düğmeyi **Tamam** yeni bağlantı penceresine dönün ve ardından **Kaydet**.

    ![Studio 3T test bağlantısı penceresinin ekran görüntüsü](./media/mongodb-mongochef/TestConnectionResults.png)

## <a name="use-studio-3t-to-create-a-database-collection-and-documents"></a>Bir veritabanı, koleksiyon ve belgelerin oluşturmak için Studio 3t'yi kullanma
Veritabanı, koleksiyon ve belgelerin Studio 3T kullanma oluşturmak için aşağıdaki adımları gerçekleştirin:

1. İçinde **Bağlantı Yöneticisi**, bağlantı vurgulayıp **Connect**.

    ![Studio 3T Bağlantı Yöneticisi ekran görüntüsü](./media/mongodb-mongochef/ConnectToAccount.png)
2. Konağa sağ tıklayın ve seçin **veritabanı ekleme**.  Bir veritabanı adı girin ve tıklatın **Tamam**.

    ![Studio 3T veritabanı ekle seçeneğinin ekran görüntüsü](./media/mongodb-mongochef/AddDatabase1.png)
3. Veritabanına sağ tıklayın ve seçin **koleksiyon Ekle**.  Bir koleksiyon adı girin ve tıklatın **Oluştur**.

    ![Studio 3T koleksiyon ekle seçeneğinin ekran görüntüsü](./media/mongodb-mongochef/AddCollection.png)
4. Tıklayın **koleksiyon** menü öğesi, ardından **Belge Ekle**.

    ![Studio 3T Belge Ekle menü öğesinin ekran görüntüsü](./media/mongodb-mongochef/AddDocument1.png)
5. Belge Ekle iletişim kutusunda, aşağıdakini yapıştırın ve ardından **Belge Ekle**.

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
6. Bu süre aşağıdaki içeriğe sahip başka bir belge ekleyin:

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
7. Örnek sorgu yürütün. Örneğin, Soyadı 'Andersen' aileleriyle arayabilir ve durumu alanları ve üst öğeleri döndürür.

    ![Mongo Chef sorgu sonuçları ekran görüntüsü](./media/mongodb-mongochef/QueryDocument1.png)

## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB MongoDB API keşfedin [örnekleri](mongodb-samples.md).
