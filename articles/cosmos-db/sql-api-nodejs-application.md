---
title: Azure Cosmos DB SQL API verileri yönetmek için JavaScript SDK'sını kullanarak bir Node.js web uygulaması derleme
description: Bu Node.js öğreticisi, Azure Web Siteleri'nde barındırılan bir Node.js Express web uygulamasında verileri depolamak ve bunlara erişmek için Microsoft Azure Cosmos DB'nin nasıl kullanılacağını açıklar.
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: sngun
ms.openlocfilehash: 04f634406eacd05e772d2b672cdfb2af6fb42054
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52874490"
---
# <a name="tutorial-build-a-nodejs-web-app-using-javascript-sdk-to-manage-azure-cosmos-db-sql-api-data"></a>Öğretici: Azure Cosmos DB SQL API verilerini yönetmek için JavaScript SDK’sını kullanarak bir Node.js web uygulaması derleme

> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [Java](sql-api-java-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Python](sql-api-python-application.md)
> * [Xamarin](mobile-apps-with-xamarin.md)
> 

Bu Node.js öğreticisi, Azure Web Siteleri'nde barındırılan bir Node.js Express uygulamasını kullanarak Azure Cosmos DB'nin SQL API hesabından veri depolama ve bu verilere erişme adımlarını göstermektedir. Bu öğreticide görev oluşturmanızı, almanızı ve tamamlamanızı sağlayan bir web tabanlı uygulama (Todo uygulaması) derleyeceksiniz. Görevler, JSON belgeleri olarak Azure Cosmos DB'de depolanır. 

Bu öğreticide Azure portalı kullanarak bir Azure Cosmos DB SQL API hesabını oluşturma işlemi gösterilmektedir. Bu işlemin ardından veritabanını ve kapsayıcıyı oluşturup kapsayıcıya öğe eklemek için Node.js SDK'sı ile bir web uygulaması derleyecek ve oluşturacaksınız. Bu öğreticide JavaScript SDK sürüm 2.0 kullanılmaktadır.

İsterseniz tamamlanmış örneği [GitHub][GitHub] üzerinden edinebilir ve uygulamayı çalıştırma yönergelerini içeren [Readme](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md) dosyasını inceleyebilirsiniz.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Azure Cosmos DB hesabı oluşturma
> * Yeni bir Node.js uygulaması oluşturma
> * Uygulamayı Azure Cosmos DB’ye bağlama
> * Uygulamayı Azure'da çalıştırma ve dağıtma

## <a name="_Toc395783176"></a>Önkoşullar

Bu makaledeki yönergeleri izlemeden önce aşağıdaki kaynaklara sahip olduğunuzdan emin olun:

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Node.js][Node.js] sürüm 6.10 veya üzeri.
* [Express oluşturucu](http://www.expressjs.com/starter/generator.html) (Express'i `npm install express-generator -g` aracılığıyla yükleyebilirsiniz)
* [Git][Git]'i yerel iş istasyonunuza yükleyin.

## <a name="_Toc395637761"></a>1. Adım: Azure Cosmos DB hesabı oluşturma
İlk olarak bir Azure Cosmos DB hesabı oluşturalım. Zaten bir hesabınız varsa veya bu öğretici için Azure Cosmos DB Öykünücüsü’nü kullanıyorsanız [2. Adım: Yeni Node.js uygulaması oluşturma](#_Toc395783178) adımına atlayabilirsiniz.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [cosmos-db-keys](../../includes/cosmos-db-keys.md)]

## <a name="_Toc395783178"></a>2. Adım: Yeni bir Node.js uygulaması oluşturma
Şimdi [Express](http://expressjs.com/) altyapısını kullanarak temel bir Hello World Node.js projesi oluşturmayı öğrenelim.

1. Node.js komut istemi gibi istediğiniz bir terminal uygulamasını açın.

1. Yeni uygulamanın depolanmasını istediğiniz dizine gidin.

1. Express oluşturucuyu kullanarak **todo** adlı yeni bir uygulama oluşturun.

   ```bash
   express todo
   ```

1. **todo** dizinini açın ve bağımlılıkları yükleyin.

   ```bash
   cd todo
   npm install
   ```

1. Yeni uygulamayı çalıştırın.

   ```bash
   npm start
   ```

1. Tarayıcınızda [http://localhost:3000](http://localhost:3000) adresine giderek yeni uygulamanızı görüntüleyebilirsiniz.
   
   ![Node.js öğrenin - Bir tarayıcı penceresinde Hello World uygulamasının ekran görüntüsü](./media/sql-api-nodejs-application/cosmos-db-node-js-express.png)

 Uygulamayı durdurmak için terminal penceresinde CTRL+C tuşlarına basın ve ardından toplu işlemi sonlandırmak için **y** öğesini seçin.

## <a name="_Toc395783179"></a>3. Adım: Gerekli modülleri yükleme

**Package.json** dosyası, projenin kökünde oluşturulan dosyalardan biridir. Bu dosya, Node.js uygulamanız için gerekli olan ek modüllerin listesini içerir. Bu uygulamayı Azure'a dağıttığınızda uygulamanızı desteklemek amacıyla Azure'a hangi modüllerin yüklenmesi gerektiğini belirlemek için bu dosya kullanılır. Bu öğretici için iki paket daha yükleyeceksiniz.

1. Terminali açın, npm aracılığıyla **async** modülünü yükleyin.

   ```bash
   npm install async --save
   ```

2. npm aracılığıyla **@azure/cosmos** modülünü yükleyin. 

   ```bash
   npm install @azure/cosmos
   ```

## <a name="_Toc395783180"></a>4. Adım: Node.js uygulamasını Azure Cosmos DB'ye bağlama
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

2. Aşağıdaki kodu **tasklist.js**'ye ekleyin. Bu kod, **tasklist.js** tarafından kullanılan CosmosClient ve async modüllerini yükler. Bu kod, daha önce tanımladığımız **TaskDao** nesnesinin bir örneği olarak geçirilmiş **TaskList** sınıfını da tanımlar:
   
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

2. **app.js** dosyasına aşağıdaki kodu ekleyin. Bu kod, kullanılacak yapılandırma dosyasını tanımlar ve değerleri sonraki bölümlerde kullanacağınız değişkenlere yükler. 
   
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

3. Son olarak, **app.js** dosyasını kaydedip kapatın.

## <a name="_Toc395783181"></a>5. Adım: Kullanıcı arabirimi oluşturma

Şimdi kullanıcının uygulamayla etkileşim kurabilmesi için kullanıcı arabirimini oluşturalım. Önceki bölümlerde oluşturduğumuz Express uygulaması, görüntüleme altyapısı olarak **Jade**'i kullanır. Jade hakkında daha fazla bilgi için bkz. [Jade dili](http://jade-lang.com/).

1. **views** dizinindeki **layout.jade** dosyası diğer **.jade** dosyaları için genel bir şablon olarak kullanılır. Bu adımda, web sitesi tasarlamak için kullanılan bir araç seti olan [Twitter Bootstrap](https://github.com/twbs/bootstrap)'i kullanmak için bu dosyayı değiştireceksiniz.  

2. **views** klasöründe bulunan **layout.jade** dosyasını açın ve içeriğini aşağıdaki kodla değiştirin:

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

    Bu kod, **Jade** altyapısına uygulamamız için bazı HTML'leri işlemesini söyler ve içerik sayfalarımıza düzeni sağlayabileceğimiz **content** adlı bir **blok** oluşturur. **layout.jade** dosyasını kaydedin ve kapatın.

3. Şimdi uygulamamız tarafından kullanılacak görünüm olan **index.jade** dosyasını açın ve dosyanın içeriğini aşağıdaki kodla değiştirin:

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

Bu kod, düzeni genişletir ve daha önce **layout.jade** dosyasında gördüğümüz **content** yer tutucusu için içerik sağlar. Bu düzende iki HTML formu oluşturduk.

İlk form, öğeleri denetleyicinin **/completeTask** yöntemine göndererek güncelleştirmenizi sağlayan bir düğmeyi ve verileriniz için bir tabloyu içerir.
    
İkinci form, yeni bir öğeyi denetleyicinin **/addtask** yöntemine göndererek oluşturmanızı sağlayan bir düğmeyi ve iki giriş alanını içerir. Bunlar uygulamayı çalıştırmak için yeterli olacaktır.

## <a name="_Toc395783181"></a>6. Adım: Uygulamanızı yerel olarak çalıştırma

1. Uygulamayı yerel bilgisayarınızda test etmek için terminalde `npm start` komutunu çalıştırarak uygulamanızı başlatın ve ardından [http://localhost:3000](http://localhost:3000) tarayıcı sayfasını yenileyin. Sayfa, aşağıdaki ekran görüntüsünde gösterildiği gibi görünmelidir:
   
    ![Bir tarayıcı penceresinde Yapılacaklar Listem uygulamasının ekran görüntüsü](./media/sql-api-nodejs-application/cosmos-db-node-js-localhost.png)

    > [!TIP]
    > layout.jade veya index.jade dosyasındaki girintilerle ilgili bir hata alırsanız iki dosyanın da ilk iki satırının sola dayalı olduğundan ve öncesinde boşluk bulunmadığından emin olun. İlk iki satırdan önce boşluk varsa silin, iki dosyayı da kaydedin ve tarayıcı pencerenizi yenileyin. 

2. Öğe, Öğe Adı ve Kategori alanlarını kullanarak yeni bir görev girin ve ardından **Öğe Ekle**'yi seçin. Bu işlem, Azure Cosmos DB içinde bu özelliklere sahip bir belge oluşturur. 

3. Sayfa, Yapılacaklar listesinde yeni oluşturulan öğeyi görüntülemek üzere güncelleştirilmelidir.
   
    ![Yapılacaklar listesinde yeni bir öğeyi içeren uygulamanın ekran görüntüsü](./media/sql-api-nodejs-application/cosmos-db-node-js-added-task.png)

4. Bir görevi tamamlamak için Tamamla sütunundaki onay kutusunu işaretleyin ve ardından **Görevleri güncelleştir**'i seçin. Bu işlem önceden oluşturduğunuz belgeyi güncelleştirir ve görünümden kaldırır.

5. Uygulamayı durdurmak için terminal penceresinde CTRL+C tuşlarına basın ve ardından toplu işlemi sonlandırmak için **Y** öğesini seçin.

## <a name="_Toc395783182"></a>7. Adım: Java uygulamanızı Azure Web Siteleri'ne dağıtma

1. Daha önce yapmadıysanız Azure Web Siteniz için bir git deposunu etkinleştirin. git deposu etkinleştirme yönergelerini [Azure App Service'te Yerel Git Dağıtımı](../app-service/app-service-deploy-local-git.md) konu başlığında bulabilirsiniz.

2. Azure Web Sitenizi bir git uzak öğesi olarak ekleyin.
   
   ```bash
   git remote add azure https://username@your-azure-website.scm.azurewebsites.net:443/your-azure-website.git
   ```

3. Uygulamayı uzak öğeye göndererek dağıtın.
   
   ```bash
   git push azure master
   ```

4. Web uygulamanız birkaç saniye içinde yayımlanır ve bir tarayıcıda başlatılır.

Bu öğreticinin başvuru uygulamasının tamamını indirmek veya incelemek isterseniz [GitHub][GitHub]'dan indirebilirsiniz.

## <a name="_Toc395637775"></a>Sonraki adımlar

Bu öğreticide Azure Cosmos DB SQL API verilerini yönetmek için JavaScript SDK’sını kullanarak bir Node.js web uygulaması derlemeyi öğrendiniz. Şimdi bir sonraki makaleye geçebilirsiniz:

> [!div class="nextstepaction"]
> [Xamarin ve Azure Cosmos DB ile mobil uygulamalar derleme](mobile-apps-with-xamarin.md)


[Node.js]: http://nodejs.org/
[Git]: http://git-scm.com/
[GitHub]: https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-todo-app

