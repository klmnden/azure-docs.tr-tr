---
title: "Bir web uygulaması oluşturmak için Azure Cosmos veritabanı API MongoDB için kullanma | Microsoft Docs"
description: "API MongoDB için kullanarak bir çevrimiçi veritabanı web uygulaması oluşturan bir Azure Cosmos DB Öğreticisi."
keywords: "mongodb örnekleri"
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 61a2ab3a-2fc3-4d49-a263-ed87c66628f6
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: anhoh
ms.custom: mvc
ms.openlocfilehash: a0598d32b5bad240c0a5d77a6e19285115a9f6b0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cosmos-db-connect-to-a-mongodb-app-using-net"></a>Azure Cosmos DB: Bir MongoDB uygulamasına .NET kullanarak bağlanma

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

Bu öğretici Azure portalını kullanarak bir Azure Cosmos DB hesabının nasıl oluşturulacağı ve bir veritabanı ve koleksiyonu kullanarak verileri depolamak için nasıl oluşturulacağını gösterir [MongoDB API](mongodb-introduction.md). 

Bu öğretici, aşağıdaki görevleri içerir:

> [!div class="checklist"]
> * Azure Cosmos DB hesabı oluşturma 
> * Bağlantı dizenizi güncelleştirme
> * Bir sanal makinede bir MongoDB uygulaması oluşturma 


## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

Azure portalında bir Azure Cosmos DB hesabı oluşturarak başlayalım.  

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 
[!INCLUDE [cosmos-db-emulator-mongodb](../../includes/cosmos-db-emulator-mongodb.md)]

> [!TIP]
> * Zaten Azure Cosmos DB hesabınız var mı? Bu durumda, İleri için atlayabilirsiniz [, Visual Studio çözümü ayarlama](#SetupVS)
> * Bir Azure DocumentDB hesabına sahip miydiniz? Bu nedenle, hesabınızı şimdi bir Azure Cosmos DB hesabı ise ve size atlayabilirsiniz [, Visual Studio çözümü ayarlama](#SetupVS).  
> * Azure Cosmos DB öykünücüsü kullanıyorsanız, lütfen bölümündeki adımları izleyin [Azure Cosmos DB öykünücüsü](local-emulator.md) öykünücü kurulması ve için İleri atlayabilirsiniz [, Visual Studio çözümünü kurmak](#SetupVS). 
>
>

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

1. Azure portalında içinde **Azure Cosmos DB** sayfasında, API MongoDB hesabı seçin. 
2. Hesap dikey pencerenin sol çubuğunda tıklayın **Hızlı Başlangıç**. 
3. Platformunuzu seçin (*.NET sürücü*, *Node.js sürücü*, *MongoDB Kabuk*, *Java sürücü*, *Python sürücü*). Sürücü veya listelenen aracı görmüyorsanız, endişelenmeyin, biz sürekli olarak daha fazla bağlantı kod parçacıkları belge. 
4. Kopyala ve Yapıştır MongoDB uygulamanızı ve kod parçacığını başlamaya hazır olursunuz.

## <a name="set-up-your-mongodb-app"></a>MongoDB uygulamanızı ayarlayın

Kullanabileceğiniz [bir sanal makinede çalışan MongoDB bağlandığı Azure web uygulaması oluşturma](../app-service/app-service-web-tutorial-nodejs-mongodb-app.md) eğitmen, hızlı bir şekilde MongoDB uygulamanın kurulumunu en az değişiklik (yerel olarak ya da veya bir Azure web uygulamasına yayımlanan) bağlanan bir API MongoDB hesabı için.  

1. Bir değişiklik, öğreticiyi izleyin.  Dal.cs kod bu ile değiştirin:

    ```csharp   
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using MyTaskListApp.Models;
    using MongoDB.Driver;
    using MongoDB.Bson;
    using System.Configuration;
    using System.Security.Authentication;
   
    namespace MyTaskListApp
    {
        public class Dal : IDisposable
        {
            //private MongoServer mongoServer = null;
            private bool disposed = false;
   
            // To do: update the connection string with the DNS name
            // or IP address of your server. 
            //For example, "mongodb://testlinux.cloudapp.net
            private string connectionString = "mongodb://localhost:27017";
            private string userName = "<your user name>";
            private string host = "<your host>";
            private string password = "<your password>";
   
            // This sample uses a database named "Tasks" and a 
            //collection named "TasksList".  The database and collection 
            //will be automatically created if they don't already exist.
            private string dbName = "Tasks";
            private string collectionName = "TasksList";
   
            // Default constructor.        
            public Dal()
            {
            }
   
            // Gets all Task items from the MongoDB server.        
            public List<MyTask> GetAllTasks()
            {
                try
                {
                    var collection = GetTasksCollection();
                    return collection.Find(new BsonDocument()).ToList();
                }
                catch (MongoConnectionException)
                {
                    return new List<MyTask>();
                }
            }
   
            // Creates a Task and inserts it into the collection in MongoDB.
            public void CreateTask(MyTask task)
            {
                var collection = GetTasksCollectionForEdit();
                try
                {
                    collection.InsertOne(task);
                }
                catch (MongoCommandException ex)
                {
                    string msg = ex.Message;
                }
            }
   
            private IMongoCollection<MyTask> GetTasksCollection()
            {
                MongoClientSettings settings = new MongoClientSettings();
                settings.Server = new MongoServerAddress(host, 10255);
                settings.UseSsl = true;
                settings.SslSettings = new SslSettings();
                settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;
   
                MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
                MongoIdentityEvidence evidence = new PasswordEvidence(password);
   
                settings.Credentials = new List<MongoCredential>()
                {
                    new MongoCredential("SCRAM-SHA-1", identity, evidence)
                };
   
                MongoClient client = new MongoClient(settings);
                var database = client.GetDatabase(dbName);
                var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                return todoTaskCollection;
            }
   
            private IMongoCollection<MyTask> GetTasksCollectionForEdit()
            {
                MongoClientSettings settings = new MongoClientSettings();
                settings.Server = new MongoServerAddress(host, 10255);
                settings.UseSsl = true;
                settings.SslSettings = new SslSettings();
                settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;
   
                MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
                MongoIdentityEvidence evidence = new PasswordEvidence(password);
   
                settings.Credentials = new List<MongoCredential>()
                {
                    new MongoCredential("SCRAM-SHA-1", identity, evidence)
                };
                MongoClient client = new MongoClient(settings);
                var database = client.GetDatabase(dbName);
                var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                return todoTaskCollection;
            }
   
            # region IDisposable
   
            public void Dispose()
            {
                this.Dispose(true);
                GC.SuppressFinalize(this);
            }
   
            protected virtual void Dispose(bool disposing)
            {
                if (!this.disposed)
                {
                    if (disposing)
                    {
                    }
                }
   
                this.disposed = true;
            }
   
            # endregion
        }
    }
    ```

2. Hesap ayarlarınızı Azure portalında anahtarları sayfasından başına Dal.cs dosyasında aşağıdaki değişkenleri değiştirin:

    ```csharp   
    private string userName = "<your user name>";
    private string host = "<your host>";
    private string password = "<your password>";
    ```

3. Uygulama kullanın!

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak Azure portalında bu öğretici tarafından oluşturulan tüm kaynakları silin: 

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın adına tıklayın. 
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdakileri yaptığınızdan:

> [!div class="checklist"]
> * Azure Cosmos DB hesabı oluşturma 
> * Bağlantı dizenizi güncelleştirme
> * Bir sanal makinede bir MongoDB uygulaması oluşturma

Sonraki öğretici devam ve Azure Cosmos DB MongoDB verilerinizi alın.  

> [!div class="nextstepaction"]
> [Azure Cosmos DB’ye MongoDB verileri aktarma](mongodb-migrate.md)

