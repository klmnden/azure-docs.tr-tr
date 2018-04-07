---
title: Studio 3T kullanın (MongoChef) Azure Cosmos DB ile | Microsoft Docs
description: Bir Azure Cosmos DB MongoDB API hesabıyla Studio 3T kullanmayı öğrenin
keywords: mongochef, studio 3T
services: cosmos-db
author: AndrewHoh
manager: kfile
documentationcenter: ''
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: anhoh
ms.openlocfilehash: 98f1a83ad2470d4e133167b6c4d140a0aa34e114
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="azure-cosmos-db-use-studio-3t-with-a-mongodb-api-account"></a>Azure Cosmos DB: Studio'yu kullanın 3T MongoDB API Hesapla

Bir Azure Cosmos DB MongoDB API hesabınıza bağlanmak için yapmanız gerekir:

* İndirme ve yükleme [Studio 3T](https://studio3t.com/) (önceki adıyla MongoChef da bilinir)
* Azure Cosmos DB sahip [bağlantı dizesi](connect-mongodb-account.md) MongoDB hesabınızın bilgileri

## <a name="create-the-connection-in-studio-3t"></a>Studio 3T bağlantı oluşturma
Studio 3T Bağlantı Yöneticisi Azure Cosmos DB hesabınızı eklemek için aşağıdaki adımları gerçekleştirin:

1. ' Ndaki yönergeleri kullanarak MongoDB API hesabınıza Azure Cosmos DB bağlantısı bilgilerini almak [Azure Cosmos DB MongoDB uygulamaya bağlama](connect-mongodb-account.md) makalesi.

    ![Bağlantı dizesi sayfasının ekran görüntüsü](./media/mongodb-mongochef/ConnectionStringBlade.png)
2. Tıklatın **Bağlan** Bağlantı Yöneticisi'ni açmak için ardından **yeni bağlantı**

    ![Studio 3T Bağlantı Yöneticisi ekran görüntüsü](./media/mongodb-mongochef/ConnectionManager.png)
3. İçinde **yeni bağlantı** penceresi, **Server** sekmesinde, Azure Cosmos DB hesap ana bilgisayar (FQDN) ve bağlantı NOKTASINI girin.

    ![Studio 3T Bağlantı Yöneticisi'ni sunucu sekmesinin Ekran görüntüsü](./media/mongodb-mongochef/ConnectionManagerServerTab.png)
4. İçinde **yeni bağlantı** penceresi, **kimlik doğrulaması** sekmesinde, kimlik doğrulama modu seçin **Basic (CR MONGODB veya SCARM-SHA-1)** ve kullanıcı adı ve parola girin.  Varsayılan kimlik doğrulama db (Yönetici) kabul edin veya kendi değer sağlayın.

    ![Studio 3T Bağlantı Yöneticisi kimlik doğrulaması sekmesinin Ekran görüntüsü](./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. İçinde **yeni bağlantı** penceresi, **SSL** sekmesi, onay **bağlanmak için SSL kullan Protokolü** onay kutusunu ve **sunucu otomatik olarak imzalanan SSL sertifikalarını kabul et**  radyo düğmesi.

    ![Studio 3T Bağlantı Yöneticisi SSL sekmesinin Ekran görüntüsü](./media/mongodb-mongochef/ConnectionManagerSSLTab.png)
6. Tıklatın **Bağlantıyı Sına** bağlantı bilgilerini doğrulamak için düğmesi **Tamam** yeni bağlantı penceresine geri dönün ve ardından **kaydetmek**.

    ![Studio 3T test bağlantısı penceresinin ekran görüntüsü](./media/mongodb-mongochef/TestConnectionResults.png)

## <a name="use-studio-3t-to-create-a-database-collection-and-documents"></a>Bir veritabanı, koleksiyon ve belge oluşturmak için Studio 3T kullanın
Bir veritabanı, koleksiyon ve Studio 3T kullanarak belgeleri oluşturmak için aşağıdaki adımları gerçekleştirin:

1. İçinde **Bağlantı Yöneticisi**, bağlantı vurgulayıp **Bağlan**.

    ![Studio 3T Bağlantı Yöneticisi ekran görüntüsü](./media/mongodb-mongochef/ConnectToAccount.png)
2. Ana bilgisayarı sağ tıklatın ve seçin **veritabanı ekleme**.  Bir veritabanı adı girin ve tıklayın **Tamam**.

    ![Studio 3T veritabanı ekleme seçeneği ekran görüntüsü](./media/mongodb-mongochef/AddDatabase1.png)
3. Bir veritabanını sağ tıklatın ve seçin **topluluk Ekle**.  Koleksiyon adı sağlayın ve tıklatın **oluşturma**.

    ![Studio 3T topluluk Ekle seçeneği ekran görüntüsü](./media/mongodb-mongochef/AddCollection.png)
4. Tıklatın **koleksiyonu** menü öğesi, ardından **Belge Ekle**.

    ![Studio 3T Belge Ekle menü öğesi ekran görüntüsü](./media/mongodb-mongochef/AddDocument1.png)
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
6. Başka bir belge, bu süre aşağıdaki içeriğe sahip ekleyin:

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
* Azure Cosmos DB MongoDB API'sini keşfedin [örnekleri](mongodb-samples.md).
