---
title: Web uygulaması oluşturmak üzere MongoDB için Azure Cosmos DB'nin API’sini kullanma | Microsoft Docs
description: MongoDB için API’yi kullanarak bir çevrimiçi veritabanı web uygulaması oluşturan bir Azure Cosmos DB öğreticisi.
keywords: mongodb örnekleri
services: cosmos-db
author: AndrewHoh
manager: kfile
editor: ''
documentationcenter: ''
ms.assetid: 61a2ab3a-2fc3-4d49-a263-ed87c66628f6
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: anhoh
ms.custom: mvc
ms.openlocfilehash: 76a8e19bacdbde938758bf41ed7f209521f513aa
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2018
---
# <a name="azure-cosmos-db-connect-to-a-mongodb-app-using-net"></a>Azure Cosmos DB: .NET kullanarak bir MongoDB uygulamasına bağlanma

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

Bu öğreticide Azure portalı kullanılarak Azure Cosmos DB hesabı oluşturma, ardından [MongoDB API](mongodb-introduction.md) kullanarak verileri depolamak için veritabanı ve koleksiyon oluşturma işlemleri gösterilir. 

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Azure Cosmos DB hesabı oluşturma 
> * Bağlantı dizenizi güncelleştirme
> * Bir sanal makinede MongoDB uygulaması oluşturma 


## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

İlk olarak Azure portalında bir Azure Cosmos DB hesabı oluşturalım.  

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 
[!INCLUDE [cosmos-db-emulator-mongodb](../../includes/cosmos-db-emulator-mongodb.md)]

> [!TIP]
> * Zaten Azure Cosmos DB hesabınız var mı? Öyleyle, [Visual Studio çözümünüzü ayarlama](#SetupVS) adımına atlayın
> * Azure Cosmos DB Öykünücüsü’nü kullanıyorsanız öykünücünün kurulumunu gerçekleştirmek için lütfen [Azure Cosmos DB Öykünücüsü](local-emulator.md) konusundaki adımları izleyin ve [Visual Studio Çözümünüzü ayarlama](#SetupVS) adımına atlayın. 
>
>

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

1. Azure portalında, **Azure Cosmos DB** sayfasında, MongoDB hesabı için API’yi seçin. 
2. Hesap dikey penceresinin sol çubuğunda, **Hızlı başlangıç**’a tıklayın. 
3. Platformunuzu seçin (*.NET sürücüsü*, *Node.js sürücüsü*, *MongoDB Kabuğu*, *Java sürücüsü*, *Python sürücüsü*). Sürücünüz veya aracınızı listede görmüyorsanız endişelenmeyin, sürekli olarak yeni bağlantı kod parçacıklarını belgelere ekliyoruz. 
4. Kod parçacığını kopyalayıp MongoDB uygulamanıza yapıştırdığınızda hazırsınız.

## <a name="set-up-your-mongodb-app"></a>MongoDB uygulamanızı ayarlama

MongoDB hesabı için bir API’ye bağlanan bir MongoDB uygulamasını (yerel olarak veya bir Azure web uygulamasına yayımlayarak) hızlı bir şekilde ayarlamak için, [Sanal makinede çalışan MongoDB’ye bağlanan Azure web uygulaması oluşturma](../app-service/app-service-web-tutorial-nodejs-mongodb-app.md) öğreticisini en az değişiklikle kullanabilirsiniz.  

1. Bir değişiklik ile öğreticiyi izleyin.  Dal.cs kodunu bununla değiştirin:

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

2. Dal.cs dosyasında aşağıdaki değişkenleri Azure portalının Anahtarlar sayfasındaki hesap ayarlarınıza göre değiştirin:

    ```csharp   
    private string userName = "<your user name>";
    private string host = "<your host>";
    private string password = "<your password>";
    ```

3. Uygulamayı kullanın!

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak Azure portalında bu öğretici tarafından oluşturulan tüm kaynakları silin: 

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın adına tıklayın. 
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdakileri yaptınız:

> [!div class="checklist"]
> * Azure Cosmos DB hesabı oluşturma 
> * Bağlantı dizenizi güncelleştirme
> * Bir sanal makinede MongoDB uygulaması oluşturma

Bir sonraki öğreticiye geçip MongoDB verilerini Azure Cosmos DB’de içeri aktarabilirsiniz.  

> [!div class="nextstepaction"]
> [Azure Cosmos DB’ye MongoDB verileri aktarma](mongodb-migrate.md)

