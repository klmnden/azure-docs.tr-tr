<properties 
    pageTitle="Node.js öğrenin - DocumentDB Node.js Öğreticisi | Microsoft Azure" 
    description="Node.js öğrenin! Öğretici, Azure Web Siteleri'nde barındırılan bir Node.js Express web uygulamasında verileri depolamak ve bunlara erişmek için Microsoft Azure DocumentDB'nin nasıl kullanılacağını açıklar." 
    keywords="Application development, database tutorial, learn node.js, node.js tutorial, documentdb, azure, Microsoft azure"
    services="documentdb" 
    documentationCenter="nodejs" 
    authors="aliuy" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="hero-article" 
    ms.date="04/18/2016" 
    ms.author="andrl"/>

# <a name="_Toc395783175"></a>DocumentDB kullanarak bir Node.js web uygulaması oluşturma

> [AZURE.SELECTOR]
- [.NET](documentdb-dotnet-application.md)
- [Node.js](documentdb-nodejs-application.md)
- [Java](documentdb-java-application.md)
- [Python](documentdb-python-application.md)

Bu Node.js öğreticisi, Azure Web Siteleri'nde barındırılan bir Node.js Express uygulamasında verileri depolamak ve bunlara erişmek için Azure DocumentDB hizmetinin nasıl kullanılacağını size gösterir.

Bir Azure DocumentDB veritabanı hesabının nasıl sağlanacağını ve Node.js uygulamanızda JSON belgelerinin nasıl depolanacağını öğreneceğiniz aşağıdaki videoyu izleyerek çalışmaya başlamanızı öneririz. 

> [AZURE.VIDEO azure-demo-getting-started-with-azure-documentdb-on-nodejs-in-linux]

Ardından, aşağıdaki soruların yanıtlarını öğreneceğimiz bu Node.js öğreticisine geri dönün:

- Documentdb npm modülünü kullanarak DocumentDB ile nasıl çalışırım?
- Web uygulamasını Azure Web Siteleri'ne nasıl dağıtırım?

Bu veritabanı öğreticisini uygulayarak görevlerin oluşturulmasını, alınmasını ve tamamlanmasını sağlayan basit bir web tabanlı görev yönetimi uygulaması oluşturacaksınız. Görevler, JSON belgeleri olarak Azure DocumentDB'de depolanır.

![Bu Node.js öğreticisinde oluşturulan Yapılacaklar Listem uygulamasının ekran görüntüsü](./media/documentdb-nodejs-application/image1.png)

Öğreticiyi tamamlayacak zamanınız yok ve yalnızca tam çözümü mü edinmek istiyorsunuz? Sorun değil, tam örnek çözümü [GitHub][]'dan edinebilirsiniz.

## <a name="_Toc395783176"></a>Önkoşullar

> [AZURE.TIP] Bu Node.js öğreticisi, Node.js ve Azure Web Siteleri'ni kullanma konusunda biraz deneyim sahibi olduğunuzu varsayar.

Bu makaledeki yönergeleri uygulamadan önce aşağıdakilere sahip olduğunuzdan emin olmanız gerekir:

- Etkin bir Azure hesabı. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).
- [Node.js][] v0.10.29 veya sonraki bir sürümü.
- [Express oluşturucu](http://www.expressjs.com/starter/generator.html) (bunu `npm install express-generator -g` aracılığıyla yükleyebilirsiniz)
- [Git][].

## <a name="_Toc395637761"></a>1. Adım: DocumentDB veritabanı hesabı oluşturma

Bir DocumentDB hesabı oluşturarak başlayalım. Hesabınız zaten varsa [2. Adım: Yeni bir Node.js uygulaması oluşturma](#_Toc395783178) adımına atlayabilirsiniz.

[AZURE.INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

[AZURE.INCLUDE [documentdb-keys](../../includes/documentdb-keys.md)]

## <a name="_Toc395783178"></a>2. Adım: Yeni bir Node.js uygulaması oluşturmayı öğrenme

Şimdi [Express](http://expressjs.com/) altyapısını kullanarak temel bir Hello World Node.js projesi oluşturmayı öğrenelim.

1. Sık kullandığınız terminali açın.

2. Express oluşturucuyu kullanarak **todo** adlı yeni bir uygulama oluşturun.

        express todo

3. Yeni **todo** dizininizi açın ve bağımlılıkları yükleyin.

        cd todo
        npm install

4. Yeni uygulamanızı çalıştırın.

        npm start

5. Tarayıcınızla [http://localhost:3000](http://localhost:3000) adresine giderek yeni uygulamanızı görüntüleyebilirsiniz.

    ![Node.js öğrenin - Bir tarayıcı penceresinde Hello World uygulamasının ekran görüntüsü](./media/documentdb-nodejs-application/image12.png)

## <a name="_Toc395783179"></a>3. Adım: Ek modülleri yükleme

**Package.json** dosyası, projenin kökünde oluşturulan dosyalardan biridir. Bu dosya, Node.js uygulamanız için gerekli olan ek modüllerin listesini içerir. Daha sonra bu uygulamayı bir Azure Web Sitesi'ne dağıttığınızda uygulamanızı desteklemek amacıyla Azure'a hangi modüllerin yüklenmesi gerektiğini belirlemek için bu dosya kullanılır. Bu öğretici için iki paket daha yüklememiz gerekiyor.

1. Terminale geri dönüp npm aracılığıyla **async** modülünü yükleyin.

        npm install async --save

1. Npm aracılığıyla **documentdb** modülünü yükleyin. DocumentDB'nin tüm marifeti bu modülde meydana gelir.

        npm install documentdb --save

3. Uygulamanın **package.json** dosyası hızlı bir şekilde denetlendiğinde ek modüller görüntülenmelidir. Bu dosya, uygulamanızı çalıştırırken hangi paketlerin indirilip yükleneceğini Azure'a bildirir. Aşağıdaki örneğe benzemelidir.

    ![Package.json sekmesinin ekran görüntüsü](./media/documentdb-nodejs-application/image17.png)

       This tells Node (and Azure later) that your application depends on these additional modules.

## <a name="_Toc395783180"></a>4. Adım: DocumentDB hizmetini bir düğüm uygulamasında kullanma

Böylece tüm ilk kurulum ve yapılandırma işlemleri sona erdi, şimdi burada olma nedenimize dönelim ve Azure DocumentDB'yi kullanarak biraz kod yazalım.

### Modeli oluşturma

1. Proje dizininde **models** adlı yeni bir dizin oluşturun.
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
                            
                            var requestOptions = {
                                offerType: 'S1'
                            };
                            
                            client.createCollection(databaseLink, collectionSpec, requestOptions, function (err, created) {
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

> [AZURE.TIP] createCollection, Koleksiyon için Teklif Türü'nü belirtmek üzere kullanılabilecek bir isteğe bağlı requestOptions parametresi alır. Hiçbir requestOptions.offerType değeri sağlanmazsa Koleksiyon varsayılan Teklif Türü kullanılarak oluşturulur.
> DocumentDB Teklif Türleri hakkında daha fazla bilgi için lütfen [DocumentDB'de performans düzeyleri](documentdb-performance-levels.md)'ne başvurun. 
        
3. **docdbUtils.js** dosyasını kaydedin ve kapatın.

4. **taskDao.js** dosyasının başına, yukarıda oluşturduğumuz **DocumentDBClient** ve **docdbUtils.js**'ye başvurmak için aşağıdaki kodu ekleyin:

        var DocumentDBClient = require('documentdb').DocumentClient;
        var docdbUtils = require('./docdbUtils');

4. Ardından, Task nesnesini tanımlamak ve dışarı aktarmak için kod ekleyeceksiniz. Bu kod Task nesnemizin başlatılmasından ve kullanacağımız Veritabanı ve Belge Koleksiyonunun ayarlanmasından sorumludur.

        function TaskDao(documentDBClient, databaseId, collectionId) {
          this.client = documentDBClient;
          this.databaseId = databaseId;
          this.collectionId = collectionId;
        
          this.database = null;
          this.collection = null;
        }
        
        module.exports = TaskDao;

5. Ardından DocumentDB'de depolanan verilerle etkileşimi sağlayan ek yöntemleri Task nesnesinde tanımlamak için aşağıdaki kodu ekleyin.

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

6. **taskDao.js** dosyasını kaydedin ve kapatın. 

### Denetleyiciyi oluşturma

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
 
### Config.js ekleme

1. Proje dizininizde **config.js** adlı yeni bir dosya oluşturun.
2. Aşağıdakileri **config.js**'ye ekleyin. Bu, uygulamamız için gereken yapılandırma ayarlarını ve değerlerini tanımlar.

        var config = {}
        
        config.host = process.env.HOST || "[the URI value from the DocumentDB Keys blade on http://portal.azure.com]";
        config.authKey = process.env.AUTH_KEY || "[the PRIMARY KEY value from the DocumentDB Keys blade on http://portal.azure.com]";
        config.databaseId = "ToDoList";
        config.collectionId = "Items";
        
        module.exports = config;

3. **config.js** dosyasında [Microsoft Azure Portal](https://portal.azure.com)'daki DocumentDB hesabınızın Anahtarlar dikey penceresinde bulunan değerleri kullanarak HOST ve AUTH_KEY değerlerini güncelleştirin:

4. **config.js** dosyasını kaydedin ve kapatın.
 
### App.js'yi değiştirme

1. Proje dizininde **app.js** dosyasını açın. Bu dosya daha önce Express web uygulaması oluşturulduğu zaman oluşturulmuştur.
2. Aşağıdaki kodu **app.js**'nin üst kısmına ekleyin
    
        var DocumentDBClient = require('documentdb').DocumentClient;
        var config = require('./config');
        var TaskList = require('./routes/tasklist');
        var TaskDao = require('./models/taskDao');

3. Bu kod, kullanılacak yapılandırma dosyasını tanımlar ve değerleri bu dosyadan okuyarak kısa bir süre sonra kullanacağımız bazı değişkenlere uygular.
4. **app.js** dosyasında bulunan aşağıdaki iki satırı:

        app.use('/', routes);
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



6. Bu satırlar, DocumentDB'ye yeni bir bağlantıyla (**config.js**'den okunan değerleri kullanarak) **TaskDao** nesnemizin yeni bir örneğini tanımlar, görev nesnesini başlatır ve ardından form eylemlerini **TaskList** denetleyicimizdeki yöntemlere bağlar. 

7. Son olarak, **app.js** dosyasını kaydedip kapattığınızda işimiz neredeyse bitti demektir.
 
## <a name="_Toc395783181"></a>5. Adım: Kullanıcı arabirimi oluşturma

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

4. Şimdi uygulamamız tarafından kullanılacak görünüm olan **index.jade** dosyasını açın ve dosyanın içeriğini aşağıdakilerle değiştirin:

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

5. **public\stylesheets** dizinindeki **style.css** dosyasını açın ve kodu aşağıdakilerle değiştirin:

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

## <a name="_Toc395783181"></a>6. Adım: Uygulamanızı yerel olarak çalıştırma

1. Uygulamanızı yerel makinenizde test etmek için, bir terminalde `npm start` çalıştırarak uygulamanızı başlatın ve aşağıdaki görüntü gibi görünen bir sayfaya sahip olan bir tarayıcıyı başlatın:

    ![Bir tarayıcı penceresinde Yapılacaklar Listem uygulamasının ekran görüntüsü](./media/documentdb-nodejs-application/image18.png)


2. Bilgi girmek için Öğe, Öğe Adı ve Kategori için sağlanan alanları kullanın ve ardından **Öğe Ekle**'ye tıklayın.

3. Sayfa, Yapılacaklar listesinde yeni oluşturulan öğeyi görüntülemek üzere güncelleştirilmelidir.

    ![Yapılacaklar listesinde yeni bir öğeyi içeren uygulamanın ekran görüntüsü](./media/documentdb-nodejs-application/image19.png)

4. Bir görevi tamamlamak için Tamamla sütunundaki onay kutusunu işaretlemeniz ve ardından **Görevleri güncelleştir**'e tıklamanız yeterlidir.

## <a name="_Toc395783182"></a>7. Adım: Uygulama geliştirme projenizi Azure Web Siteleri'ne dağıtma

1. Daha önce yapmadıysanız Azure Web Siteniz için bir git deposunu etkinleştirin. Bunun nasıl yapılacağı hakkındaki yönergeleri [Azure App Service'te GIT kullanarak sürekli dağıtım](../app-service-web/web-sites-publish-source-control.md) konu başlığında bulabilirsiniz.

2. Azure Web Sitenizi bir git uzak öğesi olarak ekleyin.

        git remote add azure https://username@your-azure-website.scm.azurewebsites.net:443/your-azure-website.git

3. Uzak öğeye ileterek dağıtın.

        git push azure master

4. Git birkaç saniye içinde web uygulamanızı yayımlamayı bitirecek ve eserinizi Azure'da çalışırken görebileceğiniz bir tarayıcıyı başlatacak!

## <a name="_Toc395637775"></a>Sonraki adımlar

Tebrikler! Azure DocumentDB kullanarak ilk Node.js Express Web Uygulamanızı oluşturdunuz ve bunu Azure Web Siteleri'ne yayımladınız.

Eksiksiz başvuru uygulaması için kaynak kodu [GitHub][]'dan indirilebilir.

Daha fazla bilgi için bkz. [Node.js Geliştirici Merkezi](https://azure.microsoft.com/develop/nodejs/).

[Node.js]: http://nodejs.org/
[Git]: http://git-scm.com/
[GitHub]: https://github.com/Azure-Samples/documentdb-node-todo-app
 



<!----HONumber=Jun16_HO2-->


