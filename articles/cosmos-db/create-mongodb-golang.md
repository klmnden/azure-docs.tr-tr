---
title: "Azure Cosmos DB: Golang ve Azure portalıyla bir MongoDB API'si konsol uygulaması oluşturma | Microsoft Docs"
description: "Azure Cosmos DB’ye bağlanmak ve veritabanını sorgulamak için kullanabileceğiniz bir Golang kod örneği sunar"
services: cosmos-db
author: Durgaprasad-Budhwani
manager: jhubbard
editor: mimig1
ms.service: cosmos-db
ms.topic: quickstart
ms.date: 07/21/2017
ms.author: mimig
ms.openlocfilehash: cb123107178f5e7c0207524c19331a6fa4658739
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cosmos-db-build-a-mongodb-api-console-app-with-golang-and-the-azure-portal"></a>Azure Cosmos DB: Golang ve Azure portalıyla bir MongoDB API'si konsol uygulaması oluşturma

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz.

Bu hızlı başlangıçta, [Golang](https://golang.org/) dilinde yazılmış mevcut bir [MongoDB](https://docs.microsoft.com/azure/cosmos-db/mongodb-introduction) uygulamasını kullanma ve MongoDB istemci bağlantılarını destekleyen Azure Cosmos DB veritabanınıza bağlama işlemi gösterilmektedir.

Diğer bir deyişle, Golang uygulamanız yalnızca MongoDB API’lerini kullanarak bir veritabanına bağlandığını bilir. Verilerin Azure Cosmos DB'de depolandığı uygulamaya açıkça gösterilir.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun. 

  [!INCLUDE [cosmos-db-emulator-mongodb](../../includes/cosmos-db-emulator-mongodb.md)]

- [Go](https://golang.org/dl/) ve [Go](https://golang.org/) dilinde temel bilgi düzeyi.
- Bir IDE: Jetbrains tarafından sağlanan [Gogland](https://www.jetbrains.com/go/), Microsoft tarafından sağlanan [Visual Studio Code](https://code.visualstudio.com/) veya [Atom](https://atom.io/). Bu öğreticide Goglang’i kullanıyorum.

<a id="create-account"></a>
## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Örnek uygulamayı kopyalayın ve gerekli paketleri yükleyin.

1. GOROOT\src klasörünün (varsayılan olarak C:\Go\ konumundadır) içinde CosmosDBSample adlı bir klasör oluşturun.
2. Git bash gibi bir git terminal penceresinde aşağıdaki komutu çalıştırarak örnek depoyu CosmosDBSample klasörüne kopyalayın. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-golang-getting-started.git
    ```
3.  Mgo paketini edinmek için aşağıdaki komutu çalıştırın. 

    ```
    go get gopkg.in/mgo.v2
    ```

[Mgo](http://labix.org/mgo) sürücüsü (*mango* olarak telaffuz edilir), standart Go deyimlerini izleyen çok basit bir API altında zengin ve kendini kanıtlamış bir dizi özelliği uygulayan [Go diline](http://golang.org/) yönelik bir [MongoDB](http://www.mongodb.org/) sürücüsüdür.

<a id="connection-string"></a>

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin.

1. Go uygulaması için gerekli olan bağlantı dizesi verilerini görüntülemek için sol gezinti menüsünde **Hızlı başlangıç**’a ve ardından **Diğer**’e tıklayın.

2. Goglang’de GOROOT\CosmosDBSample dizinindeki main.go dosyasını açın ve aşağıdaki ekran görüntüsünde gösterildiği gibi Azure portalından edinilen bağlantı dizesi bilgilerini kullanarak aşağıdaki kod satırlarını güncelleştirin. 

    Veritabanı adı, Azure portalı bağlantı dizesi bölmesindeki **Konak** değerinin ön ekidir. Aşağıdaki resimde gösterilen hesap için Veritabanı adı golang-coach şeklindedir.

    ```go
    Database: "The prefix of the Host value in the Azure portal",
    Username: "The Username in the Azure portal",
    Password: "The Password in the Azure portal",
    ```

    ![Azure portalında bağlantı dizesi bilgilerini gösteren Hızlı başlangıç bölmesi, Diğer sekmesi](./media/create-mongodb-golang/cosmos-db-golang-connection-string.png)

3. Main.go adlı dosyayı kaydedin.

## <a name="review-the-code"></a>Kodu gözden geçirin

Main.go dosyasında gerçekleşen işlemleri hızlıca gözden geçirelim. 

### <a name="connecting-the-go-app-to-azure-cosmos-db"></a>Azure Cosmos DB’yi kullanarak Go uygulamasına bağlanma

Azure Cosmos DB, SSL kullanan MongoDB’yi destekler. SSL kullanan bir MongoDB’ye bağlanmak için [mgo.DialInfo](http://gopkg.in/mgo.v2#DialInfo)’da **DialServer** işlevini tanımlamanız ve bağlantıyı gerçekleştirmek için [tls.*Dial*](http://golang.org/pkg/crypto/tls#Dial) işlevini kullanmanız gerekir.

Aşağıdaki Golang kod parçacığı, Go uygulaması ile Azure Cosmos DB MongoDB API’si arasında bağlantı kurar. *DialInfo* sınıfı, bir MongoDB kümesiyle oturum bağlantısı kurmak için gereken seçenekleri barındırır.

```go
// DialInfo holds options for establishing a session with a MongoDB cluster.
dialInfo := &mgo.DialInfo{
    Addrs:    []string{"golang-couch.documents.azure.com:10255"}, // Get HOST + PORT
    Timeout:  60 * time.Second,
    Database: "database", // It can be anything
    Username: "username", // Username
    Password: "Azure database connect password from Azure Portal", // PASSWORD
    DialServer: func(addr *mgo.ServerAddr) (net.Conn, error) {
        return tls.Dial("tcp", addr.String(), &tls.Config{})
    },
}

// Create a session which maintains a pool of socket connections
// to our Azure Cosmos DB MongoDB database.
session, err := mgo.DialWithInfo(dialInfo)

if err != nil {
    fmt.Printf("Can't connect to mongo, go error %v\n", err)
    os.Exit(1)
}

defer session.Close()

// SetSafe changes the session safety mode.
// If the safe parameter is nil, the session is put in unsafe mode, 
// and writes become fire-and-forget,
// without error checking. The unsafe mode is faster since operations won't hold on waiting for a confirmation.
// 
session.SetSafe(&mgo.Safe{})
```

SSL bağlantısı yoksa **mgo.Dial()** yöntemi kullanılır. SSL bağlantısı için **mgo.DialWithInfo()** yöntemi gerekir.

Oturum nesnesinin oluşturulması için **DialWIthInfo{}** nesnesinin bir örneği kullanılır. Oturum bağlantısı kurulduktan sonra aşağıdaki kod parçacığını kullanarak koleksiyona erişebilirsiniz:

```go
collection := session.DB(“database”).C(“package”)
```

<a id="create-document"></a>

### <a name="create-a-document"></a>Belge oluşturma

```go
// Model
type Package struct {
    Id bson.ObjectId  `bson:"_id,omitempty"`
    FullName      string
    Description   string
    StarsCount    int
    ForksCount    int
    LastUpdatedBy string
}

// insert Document in collection
err = collection.Insert(&Package{
    FullName:"react",
    Description:"A framework for building native apps with React.",
    ForksCount: 11392,
    StarsCount:48794,
    LastUpdatedBy:"shergin",

})

if err != nil {
    log.Fatal("Problem inserting data: ", err)
    return
}
```

### <a name="query-or-read-a-document"></a>Bir belgeyi sorgulama veya okuma

Azure Cosmos DB, her bir koleksiyonda depolanan JSON belgeleri için zengin sorguların gerçekleştirilmesini destekler. Aşağıdaki örnek kod, koleksiyonunuzdaki belgeler için çalıştırabileceğiniz bir sorguyu gösterir.

```go
// Get a Document from the collection
result := Package{}
err = collection.Find(bson.M{"fullname": "react"}).One(&result)
if err != nil {
    log.Fatal("Error finding record: ", err)
    return
}

fmt.Println("Description:", result.Description)
```


### <a name="update-a-document"></a>Bir belgeyi güncelleştirme

```go
// Update a document
updateQuery := bson.M{"_id": result.Id}
change := bson.M{"$set": bson.M{"fullname": "react-native"}}
err = collection.Update(updateQuery, change)
if err != nil {
    log.Fatal("Error updating record: ", err)
    return
}
```

### <a name="delete-a-document"></a>Bir belgeyi silme

Azure Cosmos DB, JSON belgelerini silmeyi destekler.

```go
// Delete a document
query := bson.M{"_id": result.Id}
err = collection.Remove(query)
if err != nil {
   log.Fatal("Error deleting record: ", err)
   return
}
```
    
## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Goglang’da GOPATH’inizin (**Dosya**, **Ayarlar**, **Go**, **GOPATH** altındadır) gopkg’nin yüklendiği ve varsayılan olarak USERPROFILE\go olan konumu içermesi gerekir. 
2. Uygulamayı çalıştırdıktan sonra belgeyi görebilmeniz için 91.-96. satırlar arasında yer alan ve belgeyi silen satırları açıklama satırı yapın.
3. Goglang’de **Çalıştır**’a ve ardından **'Main.go’yu derle ve çalıştır'ı çalıştır**’a tıklayın.

    Uygulama işlemi tamamlar ve [Belge oluşturma](#create-document)’da oluşturulan belgenin açıklamasını görüntüler.
    
    ```
    Description: A framework for building native apps with React.
    
    Process finished with exit code 0
    ```

    ![Uygulamanın çıktısını gösteren Goglang](./media/create-mongodb-golang/goglang-cosmos-db.png)
    
## <a name="review-your-document-in-data-explorer"></a>Veri Gezgini’nde belgenizi gözden geçirin

Belgenizi Veri Gezgini’nde görmek için Azure portalına dönün.

1. Sol gezinti menüsünde **Veri Gezgini (Önizleme)** seçeneğine tıklayın, **golang-coach**, **paket** seçeneğini genişletin ve **Belgeler**’e tıklayın. **Belgeler** sekmesinde \_id seçeneğine tıklayarak belgeyi sağ bölmede görüntüleyin. 

    ![Yeni oluşturulan belgeyi görüntüleyen Veri Gezgini](./media/create-mongodb-golang/golang-cosmos-db-data-explorer.png)
    
2. Daha sonra belgeyle satır içi çalışabilir ve **Güncelleştir**’e tıklayarak belgeyi kaydedebilirsiniz. Belgeyi silme veya yeni belgeler ya da sorgular oluşturma seçeneğiniz de vardır.

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak Azure portalında bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silin:

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın adına tıklayın. 
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure Cosmos DB hesabı oluşturmayı ve MongoDB API’sini kullanarak bir Golang uygulamasını çalıştırmayı öğrendiniz. Şimdi Cosmos DB hesabınıza ek veri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [MongoDB API'si için Azure Cosmos DB’ye veri aktarma](mongodb-migrate.md)
