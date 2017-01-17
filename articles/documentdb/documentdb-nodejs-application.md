---
title: "Node.js öğrenin - DocumentDB Node.js Öğreticisi | Microsoft Belgeleri"
description: "Node.js öğrenin! Öğretici, Azure Web Siteleri&quot;nde barındırılan bir Node.js Express web uygulamasında verileri depolamak ve bunlara erişmek için Microsoft Azure DocumentDB&quot;nin nasıl kullanılacağını açıklar."
keywords: "Uygulama geliştirme, veritabanı öğreticisi, node.js öğrenme, node.js öğreticisi, documentdb, azure, Microsoft azure"
services: documentdb
documentationcenter: nodejs
author: syamkmsft
manager: jhubbard
editor: cgronlun
ms.assetid: 9da9e63b-e76a-434e-96dd-195ce2699ef3
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 12/16/2016
ms.author: syamk
translationtype: Human Translation
ms.sourcegitcommit: a5abaa698de2978e676153832d252cf2bc43e72b
ms.openlocfilehash: cfd2f49a3452e4ad5132f55d269452e436bcecc5


---
# <a name="a-nametoc395783175abuild-a-nodejs-web-application-using-documentdb"></a><a name="_Toc395783175"></a>DocumentDB kullanarak bir Node.js web uygulaması oluşturma
> [!div class="op_single_selector"]
> * [.NET](documentdb-dotnet-application.md)
> * [Node.js](documentdb-nodejs-application.md)
> * [Java](documentdb-java-application.md)
> * [Python](documentdb-python-application.md)
> 
> 

Bu Node.js öğreticisi, Azure Web Siteleri'nde barındırılan bir Node.js Express uygulamasında verileri depolamak ve bunlara erişmek için Azure DocumentDB'nin nasıl kullanılacağını size gösterir. Görevlerin oluşturulmasını, alınmasını ve tamamlanmasını sağlayan basit bir web tabanlı görev yönetimi uygulaması, yani yapılacak işler uygulaması oluşturacaksınız. Görevler, JSON belgeleri olarak Azure DocumentDB'de depolanır. Bu öğretici, uygulamayı oluşturma ve dağıtma konusunda rehberlik yapmaktadır ve her kod parçacığında yapılanlar anlatılmaktadır.

![Bu Node.js öğreticisinde oluşturulan Yapılacaklar Listem uygulamasının ekran görüntüsü](./media/documentdb-nodejs-application/image1.png)

Öğreticiyi tamamlayacak zamanınız yok ve yalnızca tam çözümü mü edinmek istiyorsunuz? Sorun değil, tam örnek çözümü [GitHub][GitHub]'dan edinebilirsiniz. Uygulamanın nasıl çalıştırılacağını belirten talimatlar için [BeniOku](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md) dosyasını okumanız yeterlidir.

## <a name="a-nametoc395783176aprerequisites"></a><a name="_Toc395783176"></a>Önkoşullar
> [!TIP]
> Bu Node.js öğreticisi, Node.js ve Azure Web Siteleri'ni kullanma konusunda biraz deneyim sahibi olduğunuzu varsayar.
> 
> 

Bu makaledeki yönergeleri uygulamadan önce aşağıdakilere sahip olduğunuzdan emin olmanız gerekir:

* Etkin bir Azure hesabı. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).

   OR

   Yerel bir [Azure DocumentDB Öykünücüsü](documentdb-nosql-local-emulator.md) yüklemesi.
* [Node.js][Node.js] sürüm v0.10.29 veya üzeri.
* [Express oluşturucu](http://www.expressjs.com/starter/generator.html) (bunu `npm install express-generator -g` aracılığıyla yükleyebilirsiniz)
* [Git][Git].

## <a name="a-nametoc395637761astep-1-create-a-documentdb-database-account"></a><a name="_Toc395637761"></a>1. Adım: DocumentDB veritabanı hesabı oluşturma
Bir DocumentDB hesabı oluşturarak başlayalım. Zaten bir hesabınız varsa veya bu öğretici için DocumentDB Öykünücüsü’nü kullanıyorsanız [2. Adım: Yeni Node.js uygulaması oluşturma](#_Toc395783178) adımına atlayabilirsiniz.

[!INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

[!INCLUDE [documentdb-keys](../../includes/documentdb-keys.md)]

## <a name="a-nametoc395783178astep-2-learn-to-create-a-new-nodejs-application"></a><a name="_Toc395783178"></a>2. Adım: Yeni bir Node.js uygulaması oluşturmayı öğrenme
Şimdi [Express](http://expressjs.com/) altyapısını kullanarak temel bir Hello World Node.js projesi oluşturmayı öğrenelim.

1. Node.js komut istemi gibi istediğiniz bir terminal uygulamasını açın.
2. Yeni uygulamanın depolanmasını istediğiniz dizine gidin.
3. Express oluşturucuyu kullanarak **todo** adlı yeni bir uygulama oluşturun.
   
        express todo
4. Yeni **todo** dizininizi açın ve bağımlılıkları yükleyin.
   
        cd todo
        npm install
5. Yeni uygulamanızı çalıştırın.
   
        npm start
6. Tarayıcınızda [http://localhost:3000](http://localhost:3000) adresine giderek yeni uygulamanızı görüntüleyebilirsiniz.
   
    ![Node.js öğrenin - Bir tarayıcı penceresinde Hello World uygulamasının ekran görüntüsü](./media/documentdb-nodejs-application/image12.png)

    Ardından, uygulamayı durdurmak için terminal penceresinde CTRL+C tuşlarına basın ve ardından toplu işlemi sonlandırmak için **y** öğesine tıklayın.

## <a name="a-nametoc395783179astep-3-install-additional-modules"></a><a name="_Toc395783179"></a>3. Adım: Ek modülleri yükleme
**Package.json** dosyası, projenin kökünde oluşturulan dosyalardan biridir. Bu dosya, Node.js uygulamanız için gerekli olan ek modüllerin listesini içerir. Daha sonra bu uygulamayı bir Azure Web Sitesi'ne dağıttığınızda uygulamanızı desteklemek amacıyla Azure'a hangi modüllerin yüklenmesi gerektiğini belirlemek için bu dosya kullanılır. Bu öğretici için iki paket daha yüklememiz gerekiyor.

1. Terminale geri dönüp npm aracılığıyla **async** modülünü yükleyin.
   
        npm install async --save
2. Npm aracılığıyla **documentdb** modülünü yükleyin. DocumentDB'nin tüm marifeti bu modülde meydana gelir.
   
        npm install documentdb --save
3. Uygulamanın **package.json** dosyası hızlı bir şekilde denetlendiğinde ek modüller görüntülenmelidir. Bu dosya, uygulamanızı çalıştırırken hangi paketlerin indirilip yükleneceğini Azure'a bildirir. Aşağıdaki örneğe benzemelidir.
   
        {
          "name": "todo",
          "version": "0.0.0",
          "private": true,
          "scripts": {
            "start": "node ./bin/www"
          },
          "dependencies": {
            "async": "^2.1.4",
            "body-parser": "~1.15.2",
            "cookie-parser": "~1.4.3",
            "debug": "~2.2.0",
            "documentdb": "^1.10.0",
            "express": "~4.14.0",
            "jade": "~1.11.0",
            "morgan": "~1.7.0",
            "serve-favicon": "~2.3.0"
          }
        }
   
    Bu düğüm, Düğüme (ve daha sonra Azure’a) uygulamanızın bu ek modüllere bağlı olduğunu bildirir.

## <a name="a-nametoc395783180astep-4-using-the-documentdb-service-in-a-node-application"></a><a name="_Toc395783180"></a>4. Adım: DocumentDB hizmetini bir düğüm uygulamasında kullanma
Böylece tüm ilk kurulum ve yapılandırma işlemleri sona erdi, şimdi burada olma nedenimize dönelim ve Azure DocumentDB'yi kullanarak biraz kod yazalım.

### <a name="create-the-model"></a>Modeli oluşturma
1. Proje dizininde package.json dosyasıyla aynı dizinde **models** adlı yeni bir dizin oluşturun.
2. **models** dizininde **taskDao.js** adında yeni bir dosya oluşturun. Bu dosya, uygulamamız tarafından oluşturulan görevlerin modelini içerir.
3. Aynı **models** dizininde **docdbUtils.js** adlı başka bir yeni dosya oluşturun. Bu dosya, uygulama genelinde kullanacağımız bazı yararlı ve yeniden kullanılabilir kodları içerir. 
4. Aşağıdaki kodu **docdbUtils.js**'ye kopyalayın
   
        var DocumentDBClient = require('documentdb').DocumentClient;
   
        var DocDBUtils = {
            getOrCreateDatabase: function (client, databaseId, callback) {
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id= @id',
                    parameters: [{
                        name: '@id',
                        value: databaseId
                    }]
                };
   
                client.queryDatabases(querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {
                        if (results.length === 0) {
                            var databaseSpec = {
                                id: databaseId
                            };
   
                            client.createDatabase(databaseSpec, function (err, created) {
                                callback(null, created);
                            });
   
                        } else {
                            callback(null, results[0]);
                        }
                    }
                });
            },
   
            getOrCreateCollection: function (client, databaseLink, collectionId, callback) {
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id=@id',
                    parameters: [{
                        name: '@id',
                        value: collectionId
                    }]
                };               
   
                client.queryCollections(databaseLink, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {        
                        if (results.length === 0) {
                            var collectionSpec = {
                                id: collectionId
                            };
   
                            client.createCollection(databaseLink, collectionSpec, function (err, created) {
                                callback(null, created);
                            });
   
                        } else {
                            callback(null, results[0]);
                        }
                    }
                });
            }
        };
   
        module.exports = DocDBUtils;
   
   > [!TIP]
   > createCollection, Koleksiyon için Teklif Türü'nü belirtmek üzere kullanılabilecek bir isteğe bağlı requestOptions parametresi alır. Hiçbir requestOptions.offerType değeri sağlanmazsa Koleksiyon varsayılan Teklif Türü kullanılarak oluşturulur.
   > 
   > DocumentDB Teklif Türleri hakkında daha fazla bilgi için lütfen [DocumentDB'de performans düzeyleri](documentdb-performance-levels.md)'ne başvurun. 
   > 
   > 
5. **docdbUtils.js** dosyasını kaydedin ve kapatın.
6. **taskDao.js** dosyasının başına, yukarıda oluşturduğumuz **DocumentDBClient** ve **docdbUtils.js**'ye başvurmak için aşağıdaki kodu ekleyin:
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var docdbUtils = require('./docdbUtils');
7. Ardından, Task nesnesini tanımlamak ve dışarı aktarmak için kod ekleyeceksiniz. Bu kod Task nesnemizin başlatılmasından ve kullanacağımız Veritabanı ve Belge Koleksiyonunun ayarlanmasından sorumludur.
   
        function TaskDao(documentDBClient, databaseId, collectionId) {
          this.client = documentDBClient;
          this.databaseId = databaseId;
          this.collectionId = collectionId;
   
          this.database = null;
          this.collection = null;
        }
   
        module.exports = TaskDao;
8. Ardından DocumentDB'de depolanan verilerle etkileşimi sağlayan ek yöntemleri Task nesnesinde tanımlamak için aşağıdaki kodu ekleyin.
   
        TaskDao.prototype = {
            init: function (callback) {
                var self = this;
   
                docdbUtils.getOrCreateDatabase(self.client, self.databaseId, function (err, db) {
                    if (err) {
                        callback(err);
                    } else {
                        self.database = db;
                        docdbUtils.getOrCreateCollection(self.client, self.database._self, self.collectionId, function (err, coll) {
                            if (err) {
                                callback(err);
   
                            } else {
                                self.collection = coll;
                            }
                        });
                    }
                });
            },
   
            find: function (querySpec, callback) {
                var self = this;
   
                self.client.queryDocuments(self.collection._self, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {
                        callback(null, results);
                    }
                });
            },
   
            addItem: function (item, callback) {
                var self = this;
   
                item.date = Date.now();
                item.completed = false;
   
                self.client.createDocument(self.collection._self, item, function (err, doc) {
                    if (err) {
                        callback(err);
   
                    } else {
                        callback(null, doc);
                    }
                });
            },
   
            updateItem: function (itemId, callback) {
                var self = this;
   
                self.getItem(itemId, function (err, doc) {
                    if (err) {
                        callback(err);
   
                    } else {
                        doc.completed = true;
   
                        self.client.replaceDocument(doc._self, doc, function (err, replaced) {
                            if (err) {
                                callback(err);
   
                            } else {
                                callback(null, replaced);
                            }
                        });
                    }
                });
            },
   
            getItem: function (itemId, callback) {
                var self = this;
   
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id = @id',
                    parameters: [{
                        name: '@id',
                        value: itemId
                    }]
                };
   
                self.client.queryDocuments(self.collection._self, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {
                        callback(null, results[0]);
                    }
                });
            }
        };
9. **taskDao.js** dosyasını kaydedin ve kapatın. 

### <a name="create-the-controller"></a>Denetleyiciyi oluşturma
1. Projenizin **routes** dizininde **tasklist.js** adlı yeni bir dosya oluşturun. 
2. Aşağıdaki kodu **tasklist.js**'ye ekleyin. Bu kod **tasklist.js** tarafından kullanılan DocumentDBClient ve async modüllerini yükler. Bu, daha önce tanımladığımız **Task** nesnesinin bir örneği olarak geçirilmiş olup **TaskList** işlevi olarak da tanımlanır.
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var async = require('async');
   
        function TaskList(taskDao) {
          this.taskDao = taskDao;
        }
   
        module.exports = TaskList;
3. **showTasks, addTask** ve **completeTasks** için kullanılan yöntemleri ekleyerek **tasklist.js** dosyasına eklemeye devam edin:
   
        TaskList.prototype = {
            showTasks: function (req, res) {
                var self = this;
   
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.completed=@completed',
                    parameters: [{
                        name: '@completed',
                        value: false
                    }]
                };
   
                self.taskDao.find(querySpec, function (err, items) {
                    if (err) {
                        throw (err);
                    }
   
                    res.render('index', {
                        title: 'My ToDo List ',
                        tasks: items
                    });
                });
            },
   
            addTask: function (req, res) {
                var self = this;
                var item = req.body;
   
                self.taskDao.addItem(item, function (err) {
                    if (err) {
                        throw (err);
                    }
   
                    res.redirect('/');
                });
            },
   
            completeTask: function (req, res) {
                var self = this;
                var completedTasks = Object.keys(req.body);
   
                async.forEach(completedTasks, function taskIterator(completedTask, callback) {
                    self.taskDao.updateItem(completedTask, function (err) {
                        if (err) {
                            callback(err);
                        } else {
                            callback(null);
                        }
                    });
                }, function goHome(err) {
                    if (err) {
                        throw err;
                    } else {
                        res.redirect('/');
                    }
                });
            }
        };
4. **tasklist.js** dosyasını kaydedin ve kapatın.

### <a name="add-configjs"></a>Config.js ekleme
1. Proje dizininizde **config.js** adlı yeni bir dosya oluşturun.
2. Aşağıdakileri **config.js**'ye ekleyin. Bu, uygulamamız için gereken yapılandırma ayarlarını ve değerlerini tanımlar.
   
        var config = {}
   
        config.host = process.env.HOST || "[the URI value from the DocumentDB Keys blade on http://portal.azure.com]";
        config.authKey = process.env.AUTH_KEY || "[the PRIMARY KEY value from the DocumentDB Keys blade on http://portal.azure.com]";
        config.databaseId = "ToDoList";
        config.collectionId = "Items";
   
        module.exports = config;
3. **config.js** dosyasında [Microsoft Azure portalındaki](https://portal.azure.com) DocumentDB hesabınızın Anahtarlar dikey penceresinde bulunan değerleri kullanarak HOST ve AUTH_KEY değerlerini güncelleştirin.
4. **config.js** dosyasını kaydedin ve kapatın.

### <a name="modify-appjs"></a>App.js'yi değiştirme
1. Proje dizininde **app.js** dosyasını açın. Bu dosya daha önce Express web uygulaması oluşturulduğu zaman oluşturulmuştur.
2. Aşağıdaki kodu **app.js**'nin üst kısmına ekleyin
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var config = require('./config');
        var TaskList = require('./routes/tasklist');
        var TaskDao = require('./models/taskDao');
3. Bu kod, kullanılacak yapılandırma dosyasını tanımlar ve değerleri bu dosyadan okuyarak kısa bir süre sonra kullanacağımız bazı değişkenlere uygular.
4. **app.js** dosyasında bulunan aşağıdaki iki satırı:
   
        app.use('/', index);
        app.use('/users', users); 
   
      aşağıdaki kod parçacığıyla değiştirin:
   
        var docDbClient = new DocumentDBClient(config.host, {
            masterKey: config.authKey
        });
        var taskDao = new TaskDao(docDbClient, config.databaseId, config.collectionId);
        var taskList = new TaskList(taskDao);
        taskDao.init();
   
        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
        app.set('view engine', 'jade');
5. Bu satırlar, DocumentDB'ye yeni bir bağlantıyla (**config.js**'den okunan değerleri kullanarak) **TaskDao** nesnemizin yeni bir örneğini tanımlar, görev nesnesini başlatır ve ardından form eylemlerini **TaskList** denetleyicimizdeki yöntemlere bağlar. 
6. Son olarak, **app.js** dosyasını kaydedip kapattığınızda işimiz neredeyse bitti demektir.

## <a name="a-nametoc395783181astep-5-build-a-user-interface"></a><a name="_Toc395783181"></a>5. Adım: Kullanıcı arabirimi oluşturma
Artık bir kullanıcının uygulamamızla gerçekte etkileşim kurabilmesi için kullanıcı arabirimini oluşturmaya dönelim. Oluşturduğumuz Express uygulaması, görüntüleme altyapısı olarak **Jade**'i kullanır. Jade hakkında daha fazla bilgi için lütfen [http://jade-lang.com/](http://jade-lang.com/) adresine başvurun.

1. **views** dizinindeki **layout.jade** dosyası diğer **.jade** dosyaları için genel bir şablon olarak kullanılır. Bu adımda, iyi görünümlü bir web sitesi tasarlamayı kolaylaştıran bir araç seti olan [Twitter Bootstrap](https://github.com/twbs/bootstrap)'i kullanmak için bu dosyayı değiştireceksiniz. 
2. **views** klasöründe bulunan **layout.jade** dosyasını açın ve içeriğini aşağıdakilerle değiştirin:
   
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

    Bu, **Jade** altyapısına uygulamamız için bazı HTML'leri işlemesini etkili bir şekilde söyler ve içerik sayfalarımıza düzeni sağlayabileceğimiz **content** adlı bir **blok** oluşturur.
    Bu **layout.jade** dosyasını kaydedin ve kapatın.

3. Şimdi uygulamamız tarafından kullanılacak görünüm olan **index.jade** dosyasını açın ve dosyanın içeriğini aşağıdakilerle değiştirin:
   
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
                       input(type="checkbox", name="#{task.id}", value="#{!task.completed}", checked=task.completed)
             button.btn(type="submit") Update tasks
           hr
           form.well(action="/addtask", method="post")
             label Item Name:
             input(name="name", type="textbox")
             label Item Category:
             input(name="category", type="textbox")
             br
             button.btn(type="submit") Add item
   
    Bu, düzeni genişletir ve daha önce **layout.jade** dosyasında gördüğümüz **content** yer tutucusu için içerik sağlar.
   
    Bu düzende iki HTML formu oluşturduk. 
    İlk form, öğeleri denetleyicimizin **/completetask** yöntemine göndererek güncelleştirmemizi sağlayan bir düğmeyi ve verilerimiz için bir tabloyu içerir.
    İkinci form, yeni bir öğeyi denetleyicimizin **/addtask** yöntemine göndererek oluşturmamızı sağlayan bir düğmeyi ve iki giriş alanını içerir.
   
    Uygulamamızın çalışması için bunlar yeterli olacaktır.
4. **public\stylesheets** dizinindeki **style.css** dosyasını açın ve kodu aşağıdakilerle değiştirin:
   
        body {
          padding: 50px;
          font: 14px "Lucida Grande", Helvetica, Arial, sans-serif;
        }
        a {
          color: #00B7FF;
        }
        .well label {
          display: block;
        }
        .well input {
          margin-bottom: 5px;
        }
        .btn {
          margin-top: 5px;
          border: outset 1px #C8C8C8;
        }
   
    Bu **style.css** dosyasını kaydedin ve kapatın.

## <a name="a-nametoc395783181astep-6-run-your-application-locally"></a><a name="_Toc395783181"></a>6. Adım: Uygulamanızı yerel olarak çalıştırma
1. Uygulamayı yerel bilgisayarınızda test etmek için terminalde `npm start` komutunu çalıştırarak uygulamanızı başlatın ve ardından [http://localhost:3000](http://localhost:3000) tarayıcı sayfanızı yenileyin. Sayfanın aşağıdakine benzer şekilde görünmesi gerekir:
   
    ![Bir tarayıcı penceresinde Yapılacaklar Listem uygulamasının ekran görüntüsü](./media/documentdb-nodejs-application/image18.png)

    > [!TIP]
    > layout.jade veya index.jade dosyasındaki girintilerle ilgili bir hata alırsanız iki dosyanın da ilk iki satırının sola dayalı olduğundan ve öncesinde boşluk bulunmadığından emin olun. İlk iki satırdan önce boşluk varsa silin, iki dosyayı da kaydedin ve tarayıcı pencerenizi yenileyin. 

2. Öğe, Öğe Adı ve Kategori alanlarını kullanarak yeni bir görev girin ve ardından **Öğe Ekle**'ye tıklayın. Bu işlemden sonra DocumentDB içinde bu özelliklere sahip bir belge oluşturulur. 
3. Sayfa, Yapılacaklar listesinde yeni oluşturulan öğeyi görüntülemek üzere güncelleştirilmelidir.
   
    ![Yapılacaklar listesinde yeni bir öğeyi içeren uygulamanın ekran görüntüsü](./media/documentdb-nodejs-application/image19.png)
4. Bir görevi tamamlamak için Tamamla sütunundaki onay kutusunu işaretlemeniz ve ardından **Görevleri güncelleştir**'e tıklamanız yeterlidir. Bu işlem önceden oluşturduğunuz belgeyi güncelleştirir.

5. Uygulamayı durdurmak için terminal penceresinde CTRL+C tuşlarına basın ve ardından toplu işlemi sonlandırmak için **Y** öğesine tıklayın.

## <a name="a-nametoc395783182astep-7-deploy-your-application-development-project-to-azure-websites"></a><a name="_Toc395783182"></a>7. Adım: Uygulama geliştirme projenizi Azure Web Siteleri'ne dağıtma
1. Daha önce yapmadıysanız Azure Web Siteniz için bir git deposunu etkinleştirin. Bunun nasıl yapılacağı hakkındaki yönergeleri [Azure Uygulama Hizmeti’nde Yere l Git Dağıtımı](../app-service-web/app-service-deploy-local-git.md) konu başlığında bulabilirsiniz.
2. Azure Web Sitenizi bir git uzak öğesi olarak ekleyin.
   
        git remote add azure https://username@your-azure-website.scm.azurewebsites.net:443/your-azure-website.git
3. Uzak öğeye ileterek dağıtın.
   
        git push azure master
4. Git birkaç saniye içinde web uygulamanızı yayımlamayı bitirecek ve eserinizi Azure'da çalışırken görebileceğiniz bir tarayıcıyı başlatacak!

    Tebrikler! Azure DocumentDB kullanarak ilk Node.js Express Web Uygulamanızı oluşturdunuz ve bunu Azure Web Siteleri'ne yayımladınız.

    Bu öğreticinin başvuru uygulamasının tamamını indirmek veya incelemek isterseniz [GitHub][GitHub]'dan indirebilirsiniz.

## <a name="a-nametoc395637775anext-steps"></a><a name="_Toc395637775"></a>Sonraki adımlar

* DocumentDB ile ölçek ve performans testi mi yapmak istiyorsunuz? Bkz. [Azure DocumentDB ile Performans ve Ölçek Testi](documentdb-performance-testing.md)
* [Bir DocumentDB hesabını izleme](documentdb-monitor-accounts.md) hakkında bilgi edinin.
* [Query Playground](https://www.documentdb.com/sql/demo)'daki örnek veri kümelerimizde sorgular çalıştırın.
* [DocumentDB belgelerini](https://docs.microsoft.com/en-us/azure/documentdb/) keşfedin.

[Node.js]: http://nodejs.org/
[Git]: http://git-scm.com/
[Github]: https://github.com/Azure-Samples/documentdb-node-todo-app




<!--HONumber=Dec16_HO3-->


