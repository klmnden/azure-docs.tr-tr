---
title: Azure Cosmos DB için Node.js web uygulaması oluşturma | Microsoft Docs
description: Bu Node.js öğreticisi, Azure Web Siteleri'nde barındırılan bir Node.js Express web uygulamasında verileri depolamak ve bunlara erişmek için Microsoft Azure Cosmos DB'nin nasıl kullanılacağını açıklar.
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: sngun
ms.openlocfilehash: 82711ea96f6b3f8544a411ed1b6636c8473ed7e9
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46957355"
---
# <a name="_Toc395783175"></a>Azure Cosmos DB SQL API verilerini yönetmek için JavaScript SDK’sını kullanarak bir Node.js web uygulaması derleme

> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [Java](sql-api-java-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Python](sql-api-python-application.md)
> * [Xamarin](mobile-apps-with-xamarin.md)
> 

Bu Node.js öğreticisi, Azure Web Siteleri'nde barındırılan bir Node.js Express uygulamasında verileri depolamak ve bunlara erişmek için Azure Cosmos DB'nin SQL API hesabının nasıl kullanılacağını size gösterir. Bu öğreticide görev oluşturmanızı, almanızı ve tamamlamanızı sağlayan basit bir web tabanlı uygulama (Todo uygulaması) derleyeceksiniz. Görevler, JSON belgeleri olarak Azure Cosmos DB'de depolanır. Aşağıda Todo uygulamasının ekran görüntüsü verilmiştir:

![Bu Node.js öğreticisinde oluşturulan Yapılacaklar Listem uygulamasının ekran görüntüsü](./media/sql-api-nodejs-application/cosmos-db-node-js-mytodo.png)

Bu öğreticide Azure portalı kullanarak bir Azure Cosmos DB SQL API hesabını oluşturma işlemi gösterilmektedir. Bu işlemin ardından veritabanını ve kapsayıcıyı oluşturup kapsayıcıya öğe eklemek için Node.js SDK'sı ile bir web uygulaması derleyecek ve oluşturacaksınız. Bu öğreticide JavaScript SDK 2.0 sürümü kullanılmaktadır.

Tamamlanmış örneğe [GitHub][GitHub]'dan da ulaşabilirsiniz. Uygulamanın nasıl çalıştırılacağını belirten talimatlar için [BeniOku](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md) dosyasını okumanız yeterlidir.

## <a name="_Toc395783176"></a>Önkoşullar

Bu makaledeki yönergeleri uygulamadan önce aşağıdakilere sahip olduğunuzdan emin olmanız gerekir:

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Node.js][Node.js] sürüm 6.10 veya üzeri.
* [Express oluşturucu](http://www.expressjs.com/starter/generator.html) (bunu `npm install express-generator -g` aracılığıyla yükleyebilirsiniz)
* [Git][Git].

## <a name="_Toc395637761"></a>1. Adım: Azure Cosmos DB veritabanı hesabı oluşturma
İlk olarak bir Azure Cosmos DB hesabı oluşturalım. Zaten bir hesabınız varsa veya bu öğretici için Azure Cosmos DB Öykünücüsü’nü kullanıyorsanız [2. Adım: Yeni Node.js uygulaması oluşturma](#_Toc395783178) adımına atlayabilirsiniz.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [cosmos-db-keys](../../includes/cosmos-db-keys.md)]

## <a name="_Toc395783178"></a>2. Adım: Yeni bir Node.js uygulaması oluşturma
Şimdi [Express](http://expressjs.com/) altyapısını kullanarak temel bir Hello World Node.js projesi oluşturmayı öğrenelim.

1. Node.js komut istemi gibi istediğiniz bir terminal uygulamasını açın.
2. Yeni uygulamanın depolanmasını istediğiniz dizine gidin.
3. Express oluşturucuyu kullanarak **todo** adlı yeni bir uygulama oluşturun.

   ```bash
   express todo
   ```
4. Yeni **todo** dizininizi açın ve bağımlılıkları yükleyin.

   ```bash
   cd todo
   npm install
   ```
5. Yeni uygulamanızı çalıştırın.

   ```bash
   npm start
   ```

6. Tarayıcınızda [http://localhost:3000](http://localhost:3000) adresine giderek yeni uygulamanızı görüntüleyebilirsiniz.
   
    ![Node.js öğrenin - Bir tarayıcı penceresinde Hello World uygulamasının ekran görüntüsü](./media/sql-api-nodejs-application/cosmos-db-node-js-express.png)

 Uygulamayı durdurmak için terminal penceresinde CTRL+C tuşlarına basın ve ardından toplu işlemi sonlandırmak için **y** öğesine tıklayın.

## <a name="_Toc395783179"></a>3. Adım: Gerekli modülleri yükleme

**Package.json** dosyası, projenin kökünde oluşturulan dosyalardan biridir. Bu dosya, Node.js uygulamanız için gerekli olan ek modüllerin listesini içerir. Daha sonra bu uygulamayı bir Azure Web Sitesi'ne dağıttığınızda uygulamanızı desteklemek amacıyla Azure'a hangi modüllerin yüklenmesi gerektiğini belirlemek için bu dosya kullanılır. Bu öğretici için iki paket daha yüklemeniz gerekiyor.

1. Terminali açın, npm aracılığıyla **async** modülünü yükleyin.

   ```bash
   npm install async --save
   ```

2. npm aracılığıyla **@azure/cosmos** modülünü yükleyin. 

   ```bash
   npm install @azure/cosmos
   ```

## <a name="_Toc395783180"></a>4. Adım: Azure Cosmos DB hizmetini bir Node uygulamasında kullanma
İlk kurulum ve yapılandırma adımlarını tamamladığınıza göre yapılacak işler uygulamasının Azure Cosmos DB ile iletişim kurması için gereken kodu yazabilirsiniz.

### <a name="create-the-model"></a>Modeli oluşturma
1. Projenizin kök dizininde **models** adlı yeni bir dizin oluşturun.  

2. **models** dizininde **taskDao.js** adında yeni bir dosya oluşturun. Bu dosya veritabanını ve kapsayıcıyı oluşturmak için gereken kodların yanı sıra Azure Cosmos DB'de görev okuma, güncelleştirme, oluşturma ve bulma metotlarını tanımlar. 

3. Aşağıdaki kodu **taskDao.js** dosyasına kopyalayın

   ```nodejs
   // @ts-check
   const CosmosClient = require("@azure/cosmos").CosmosClient;
   const debug = require("debug")("todo:taskDao");
   class TaskDao {
     /**
      * Manages reading, adding, and updating Tasks in Cosmos DB
      * @param {CosmosClient} cosmosClient
      * @param {string} databaseId
      * @param {string} containerId
      */
     constructor(cosmosClient, databaseId, containerId) {
       this.client = cosmosClient;
       this.databaseId = databaseId;
       this.collectionId = containerId;

       this.database = null;
       this.container = null;
     }

     async init() {
       debug("Setting up the database...");
       const dbResponse = await this.client.databases.createIfNotExists({
         id: this.databaseId
       });
       this.database = dbResponse.database;
       debug("Setting up the database...done!");
       debug("Setting up the container...");
       const coResponse = await this.database.containers.createIfNotExists({
         id: this.collectionId
       });
       this.container = coResponse.container;
       debug("Setting up the container...done!");
     }

     async find(querySpec) {
       debug("Querying for items from the database");
       if (!this.container) {
         throw new Error("Collection is not initialized.");
       }
       const { result: results } = await this.container.items
        .query(querySpec)
        .toArray();
      return results;
    }

    async addItem(item) {
      debug("Adding an item to the database");
      item.date = Date.now();
      item.completed = false;
      const { body: doc } = await this.container.items.create(item);
      return doc;
    }

    async updateItem(itemId) {
      debug("Update an item in the database");
      const doc = await this.getItem(itemId);
      doc.completed = true;

      const { body: replaced } = await this.container.item(itemId).replace(doc);
      return replaced;
    }

    async getItem(itemId) {
      debug("Getting an item from the database");
      const { body } = await this.container.item(itemId).read();
      return body;
    }
  }

   module.exports = TaskDao;
   ```
4. **taskDao.js** dosyasını kaydedin ve kapatın.  

### <a name="create-the-controller"></a>Denetleyiciyi oluşturma

1. Projenizin **routes** dizininde **tasklist.js** adlı yeni bir dosya oluşturun.  

2. Aşağıdaki kodu **tasklist.js**'ye ekleyin. Bu kod **tasklist.js** tarafından kullanılan CosmosClient ve async modüllerini yükler. Bu, daha önce tanımladığımız **TaskDao** nesnesinin bir örneği olarak geçirilmiş **TaskList** sınıfını da tanımlar:
   
   ```nodejs
   const TaskDao = require("../models/TaskDao");

   class TaskList {
     /**
      * Handles the various APIs for displaying and managing tasks
      * @param {TaskDao} taskDao
     */
    constructor(taskDao) {
    this.taskDao = taskDao;
    }
    async showTasks(req, res) {
      const querySpec = {
        query: "SELECT * FROM root r WHERE r.completed=@completed",
        parameters: [
          {
            name: "@completed",
            value: false
          }
        ]
      };

      const items = await this.taskDao.find(querySpec);
      res.render("index", {
        title: "My ToDo List ",
        tasks: items
      });
    }

    async addTask(req, res) {
      const item = req.body;

      await this.taskDao.addItem(item);
      res.redirect("/");
    }

    async completeTask(req, res) {
      const completedTasks = Object.keys(req.body);
      const tasks = [];

      completedTasks.forEach(task => {
        tasks.push(this.taskDao.updateItem(task));
      });

      await Promise.all(tasks);

      res.redirect("/");
    }
  }

  module.exports = TaskList;
   ```

3. **tasklist.js** dosyasını kaydedin ve kapatın.

### <a name="add-configjs"></a>Config.js ekleme

1. Projenizin kök dizininde **config.js** adlı yeni bir dosya oluşturun. 

2. **config.js** dosyasına aşağıdaki kodu ekleyin. Bu kod, uygulamamız için gereken yapılandırma ayarlarını ve değerlerini tanımlar.
   
   ```nodejs
   const config = {};

   config.host = process.env.HOST || "[the endpoint URI of your Azure Cosmos DB account]";
   config.authKey =
     process.env.AUTH_KEY || "[the PRIMARY KEY value of your Azure Cosmos DB account";
   config.databaseId = "ToDoList";
   config.containerId = "Items";

   if (config.host.includes("https://localhost:")) {
     console.log("Local environment detected");
     console.log("WARNING: Disabled checking of self-signed certs. Do not have this code in production.");
     process.env.NODE_TLS_REJECT_UNAUTHORIZED = "0";
     console.log(`Go to http://localhost:${process.env.PORT || '3000'} to try the sample.`);
   }

   module.exports = config;
   ```

3. **config.js** dosyasında [Microsoft Azure portalındaki](https://portal.azure.com) Azure Cosmos DB hesabınızın Anahtarlar sayfasında bulunan değerleri kullanarak HOST ve AUTH_KEY değerlerini güncelleştirin. 

4. **config.js** dosyasını kaydedin ve kapatın.

### <a name="modify-appjs"></a>App.js'yi değiştirme
1. Proje dizininde **app.js** dosyasını açın. Bu dosya daha önce Express web uygulaması oluşturulduğu zaman oluşturulmuştur.  

2. **app.js** dosyasına aşağıdaki kodu ekleyin. Bu kod, kullanılacak yapılandırma dosyasını tanımlar ve değerleri bu dosyadan okuyarak kısa bir süre sonra kullanacağımız bazı değişkenlere uygular. 
   
   ```nodejs
   const CosmosClient = require("@azure/cosmos").CosmosClient;
   const config = require("./config");
   const TaskList = require("./routes/tasklist");
   const TaskDao = require("./models/taskDao");

   const express = require("express");
   const path = require("path");
   const logger = require("morgan");
   const cookieParser = require("cookie-parser");
   const bodyParser = require("body-parser");

   const app = express();

   // view engine setup
   app.set("views", path.join(__dirname, "views"));
   app.set("view engine", "jade");

   // uncomment after placing your favicon in /public
   //app.use(favicon(path.join(__dirname, 'public', 'favicon.ico')));
   app.use(logger("dev"));
   app.use(bodyParser.json());
   app.use(bodyParser.urlencoded({ extended: false }));
   app.use(cookieParser());
   app.use(express.static(path.join(__dirname, "public")));

   //Todo App:
   const cosmosClient = new CosmosClient({
     endpoint: config.host,
     auth: {
       masterKey: config.authKey
     }
   });
   const taskDao = new TaskDao(cosmosClient, config.databaseId, config.containerId);
   const taskList = new TaskList(taskDao);
   taskDao
     .init(err => {
       console.error(err);
     })
     .catch(err => {
       console.error(err);
       console.error("Shutting down because there was an error settinig up the database.");
       process.exit(1);
     });

   app.get("/", (req, res, next) => taskList.showTasks(req, res).catch(next));
   app.post("/addtask", (req, res, next) => taskList.addTask(req, res).catch(next));
   app.post("/completetask", (req, res, next) => taskList.completeTask(req, res).catch(next));
   app.set("view engine", "jade");

   // catch 404 and forward to error handler
   app.use(function(req, res, next) {
     const err = new Error("Not Found");
     err.status = 404;
     next(err);
   });

   // error handler
   app.use(function(err, req, res, next) {
     // set locals, only providing error in development
     res.locals.message = err.message;
     res.locals.error = req.app.get("env") === "development" ? err : {};

     // render the error page
     res.status(err.status || 500);
     res.render("error");
   });

   module.exports = app;
   ```

3. Son olarak, **app.js** dosyasını kaydedip kapattığınızda işimiz neredeyse bitti demektir.

## <a name="_Toc395783181"></a>5. Adım: Kullanıcı arabirimi oluşturma
Artık bir kullanıcının uygulamamızla gerçekte etkileşim kurabilmesi için kullanıcı arabirimini oluşturmaya dönelim. Oluşturduğumuz Express uygulaması, görüntüleme altyapısı olarak **Jade**'i kullanır. Jade hakkında daha fazla bilgi için lütfen [http://jade-lang.com/](http://jade-lang.com/) adresine başvurun.

1. **views** dizinindeki **layout.jade** dosyası diğer **.jade** dosyaları için genel bir şablon olarak kullanılır. Bu adımda, iyi görünümlü bir web sitesi tasarlamayı kolaylaştıran bir araç seti olan [Twitter Bootstrap](https://github.com/twbs/bootstrap)'i kullanmak için bu dosyayı değiştireceksiniz.  

2. **views** klasöründe bulunan **layout.jade** dosyasını açın ve içeriğini aşağıdakilerle değiştirin:

   ```html
   doctype html
   html
     head
       title= title
       link(rel='stylesheet', href='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css')
       link(rel='stylesheet', href='/stylesheets/style.css')
     body
       nav.navbar.navbar-inverse.navbar-fixed-top
         div.navbar-header
           a.navbar-brand(href='#') My Tasks
       block content
       script(src='//ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js')
       script(src='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js')
   ```

    Bu, **Jade** altyapısına uygulamamız için bazı HTML'leri işlemesini etkili bir şekilde söyler ve içerik sayfalarımıza düzeni sağlayabileceğimiz **content** adlı bir **blok** oluşturur.

    Bu **layout.jade** dosyasını kaydedin ve kapatın.

3. Şimdi uygulamamız tarafından kullanılacak görünüm olan **index.jade** dosyasını açın ve dosyanın içeriğini aşağıdakilerle değiştirin:

   ```html
   extends layout
   block content
        h1 #{title}
        br
        
        form(action="/completetask", method="post")
         table.table.table-striped.table-bordered
            tr
              td Name
              td Category
              td Date
              td Complete
            if (typeof tasks === "undefined")
              tr
                td
            else
              each task in tasks
                tr
                  td #{task.name}
                  td #{task.category}
                  - var date  = new Date(task.date);
                  - var day   = date.getDate();
                  - var month = date.getMonth() + 1;
                  - var year  = date.getFullYear();
                  td #{month + "/" + day + "/" + year}
                  td
                   if(task.completed) 
                    input(type="checkbox", name="#{task.id}", value="#{!task.completed}", checked=task.completed)
                   else
                    input(type="checkbox", name="#{task.id}", value="#{!task.completed}", checked=task.completed)
          button.btn.btn-primary(type="submit") Update tasks
        hr
        form.well(action="/addtask", method="post")
          label Item Name:
          input(name="name", type="textbox")
          label Item Category:
          input(name="category", type="textbox")
          br
          button.btn(type="submit") Add item
   ```

Bu, düzeni genişletir ve daha önce **layout.jade** dosyasında gördüğümüz **content** yer tutucusu için içerik sağlar.
   
Bu düzende iki HTML formu oluşturduk.

İlk form, öğeleri denetleyicimizin **/completeTask** yöntemine göndererek güncelleştirmemizi sağlayan bir düğmeyi ve verilerimiz için bir tabloyu içerir.
    
İkinci form, yeni bir öğeyi denetleyicimizin **/addtask** yöntemine göndererek oluşturmamızı sağlayan bir düğmeyi ve iki giriş alanını içerir.

Uygulamamızın çalışması için bunlar yeterli olacaktır.

## <a name="_Toc395783181"></a>6. Adım: Uygulamanızı yerel olarak çalıştırma
1. Uygulamayı yerel bilgisayarınızda test etmek için terminalde `npm start` komutunu çalıştırarak uygulamanızı başlatın ve ardından [http://localhost:3000](http://localhost:3000) tarayıcı sayfanızı yenileyin. Sayfanın aşağıdakine benzer şekilde görünmesi gerekir:
   
    ![Bir tarayıcı penceresinde Yapılacaklar Listem uygulamasının ekran görüntüsü](./media/sql-api-nodejs-application/cosmos-db-node-js-localhost.png)

    > [!TIP]
    > layout.jade veya index.jade dosyasındaki girintilerle ilgili bir hata alırsanız iki dosyanın da ilk iki satırının sola dayalı olduğundan ve öncesinde boşluk bulunmadığından emin olun. İlk iki satırdan önce boşluk varsa silin, iki dosyayı da kaydedin ve tarayıcı pencerenizi yenileyin. 

2. Öğe, Öğe Adı ve Kategori alanlarını kullanarak yeni bir görev girin ve ardından **Öğe Ekle**'ye tıklayın. Bu işlemden sonra Azure Cosmos DB içinde bu özelliklere sahip bir belge oluşturulur. 
3. Sayfa, Yapılacaklar listesinde yeni oluşturulan öğeyi görüntülemek üzere güncelleştirilmelidir.
   
    ![Yapılacaklar listesinde yeni bir öğeyi içeren uygulamanın ekran görüntüsü](./media/sql-api-nodejs-application/cosmos-db-node-js-added-task.png)
4. Bir görevi tamamlamak için Tamamla sütunundaki onay kutusunu işaretlemeniz ve ardından **Görevleri güncelleştir**'e tıklamanız yeterlidir. Bu işlem önceden oluşturduğunuz belgeyi güncelleştirir ve görünümden kaldırır.

5. Uygulamayı durdurmak için terminal penceresinde CTRL+C tuşlarına basın ve ardından toplu işlemi sonlandırmak için **Y** öğesine tıklayın.

## <a name="_Toc395783182"></a>7. Adım: Uygulama geliştirme projenizi Azure Web Siteleri'ne dağıtma
1. Daha önce yapmadıysanız Azure Web Siteniz için bir git deposunu etkinleştirin. Bunun nasıl yapılacağı hakkındaki yönergeleri [Azure Uygulama Hizmeti’nde Yere l Git Dağıtımı](../app-service/app-service-deploy-local-git.md) konu başlığında bulabilirsiniz.
2. Azure Web Sitenizi bir git uzak öğesi olarak ekleyin.
   
        git remote add azure https://username@your-azure-website.scm.azurewebsites.net:443/your-azure-website.git
3. Uzak öğeye ileterek dağıtın.
   
        git push azure master
4. Git birkaç saniye içinde web uygulamanızı yayımlamayı bitirecek ve eserinizi Azure'da çalışırken görebileceğiniz bir tarayıcıyı başlatacak!

    Tebrikler! Azure Cosmos DB kullanarak ilk Node.js Express Web Uygulamanızı oluşturdunuz ve bunu Azure Web Siteleri'ne yayımladınız.

    Bu öğreticinin başvuru uygulamasının tamamını indirmek veya incelemek isterseniz [GitHub][GitHub]'dan indirebilirsiniz.

## <a name="_Toc395637775"></a>Sonraki adımlar

* Azure Cosmos DB ile ölçek ve performans testi mi yapmak istiyorsunuz? Bkz. [Azure Cosmos DB ile Performans ve Ölçek Testi](performance-testing.md)
* [Azure Cosmos DB hesabını nasıl izleyebileceğinizi](monitor-accounts.md) öğrenin.
* [Query Playground](https://www.documentdb.com/sql/demo)'daki örnek veri kümelerimizde sorgular çalıştırın.
* [Azure Cosmos DB belgelerini](https://docs.microsoft.com/azure/cosmos-db/) keşfedin.

[Node.js]: http://nodejs.org/
[Git]: http://git-scm.com/
[GitHub]: https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-todo-app

