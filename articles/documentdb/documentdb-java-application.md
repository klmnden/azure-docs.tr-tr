<properties
    pageTitle="DocumentDB kullanarak Java uygulaması geliştirme öğreticisi | Microsoft Azure"
    description="Bu Java web uygulaması öğreticisi, Azure Web Siteleri'nde barındırılan bir Java uygulamasında verileri depolamak ve bunlara erişmek için Azure DocumentDB hizmetinin nasıl kullanılacağını size gösterir."
    keywords="Uygulama geliştirme, veritabanı öğreticisi, java uygulaması, java web uygulaması öğreticisi, documentdb, Azure, Microsoft Azure"
    services="documentdb"
    documentationCenter="java"
    authors="AndrewHoh"
    manager="jhubbard"
    editor="mimig"/>

<tags
    ms.service="documentdb"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-services"
    ms.date="08/24/2016"
    ms.author="anhoh"/>

# DocumentDB kullanarak bir Java web uygulaması oluşturma

> [AZURE.SELECTOR]
- [.NET](documentdb-dotnet-application.md)
- [Node.js](documentdb-nodejs-application.md)
- [Java](documentdb-java-application.md)
- [Python](documentdb-python-application.md)

Bu Java web uygulaması öğreticisi, Azure Web Siteleri'nde barındırılan bir Java uygulamasında verileri depolamak ve bunlara erişmek için [Microsoft Azure DocumentDB](https://portal.azure.com/#gallery/Microsoft.DocumentDB) hizmetinin nasıl kullanılacağını size gösterir. Bu konu başlığında şunları öğreneceksiniz:

- Eclipse'te temel bir JSP uygulaması oluşturma.
- [DocumentDB Java SDK'sını](https://github.com/Azure/azure-documentdb-java) kullanarak Azure DocumentDB hizmetiyle çalışma.

Bu Java uygulaması öğreticisi görevleri oluşturmanızı, almanızı ve aşağıdaki görüntüde gösterilen şekilde tamamlandı olarak işaretlemenizi sağlayan bir web tabanlı görev yönetimi uygulamasını nasıl oluşturacağınızı gösterir. Yapılacaklar listesindeki görevlerin her biri, Azure DocumentDB'de JSON belgeleri olarak depolanır.

![Yapılacaklar Listesi Java uygulamam](./media/documentdb-java-application/image1.png)

> [AZURE.TIP] Bu uygulama geliştirme öğreticisi, Java kullanımına ilişkin deneyim sahibi olduğunuzu varsayar. Java veya [önkoşul araçlarında](#Prerequisites) yeniyseniz GitHub'dan [yapılacaklar](https://github.com/Azure-Samples/documentdb-java-todo-app) projesinin tamamını indirmenizi ve [bu makalenin sonundaki yönergeleri](#GetProject) kullanarak projeyi oluşturmanızı öneririz. Oluşturduktan sonra, proje bağlamında kodu daha iyi kavramak için makaleyi inceleyebilirsiniz.  

##<a id="Prerequisites"></a>Bu Java web uygulaması öğreticisi için önkoşullar
Bu uygulama geliştirme öğreticisine başlamadan önce aşağıdakilere sahip olmanız gerekir:

- Etkin bir Azure hesabı. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).
- [Java Geliştirme Seti (JDK) 7 +](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
- [Java EE Geliştiricileri için Eclipse IDE.](http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunasr1)
- [Java çalışma zamanı ortamı (ör. Tomcat veya Jetty) etkin bir Azure Web Sitesi.](../app-service-web/web-sites-java-get-started.md)

Bu araçları ilk kez yüklüyorsanız coreservlets.com adresindeki [Öğretici: TomCat7'yi yükleme ve Eclipse ile kullanma](http://www.coreservlets.com/Apache-Tomcat-Tutorial/tomcat-7-with-eclipse.html) makalesinin Hızlı Başlangıç bölümünde yükleme işlem için bir adım adım kılavuz mevcuttur.

##<a id="CreateDB"></a>1. Adım: DocumentDB veritabanı hesabı oluşturma

Bir DocumentDB hesabı oluşturarak başlayalım. Hesabınız zaten varsa [2. Adım: Java JSP uygulaması oluşturma](#CreateJSP) adımına atlayabilirsiniz.

[AZURE.INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

[AZURE.INCLUDE [documentdb-keys](../../includes/documentdb-keys.md)]

##<a id="CreateJSP"></a>2. Adım: Java JSP uygulaması oluşturma

JSP uygulaması oluşturmak için:

1. İlk olarak, bir Java projesi oluşturarak başlayacağız. Eclipse'i başlatın, ardından **Dosya**'ya tıklayın, **Yeni**'ye tıklayın ve ardından **Dinamik Web Projesi**'ne tıklayın. Kullanılabilir bir proje olarak **Dinamik Web Projesi**'ni listelenmiş şekilde görmüyorsanız şunu yapın: **Dosya**'ya tıklayın, **Yeni**'ye tıklayın, **Proje**… seçeneğine tıklayın, **Web**'i genişletin, **Dinamik Web Projesi**'ne tıklayın ve **İleri**'ye tıklayın.

    ![JSP Java Uygulaması Geliştirme](./media/documentdb-java-application/image10.png)

2. **Proje adı** kutusuna bir proje adı girin ve **Hedef Çalışma Zamanı** açılan menüsünde isteğe bağlı olarak bir değer seçin (ör. Apache Tomcat v7.0) ve ardından **Son**'a tıklayın. Bir hedef çalışma zamanının seçilmesi, projenizi Eclipse aracılığıyla yerel olarak çalıştırmanızı sağlar.
3. Eclipse'te Proje Gezgini görünümünde projenizi genişletin. **WebContent**'e sağ tıklayın, **Yeni**'ye tıklayın ve ardından **JSP Dosyası**'na tıklayın.
4. **Yeni JSP Dosyası** iletişim kutusunda dosyaya **index.jsp** adını verin. Üst klasörü aşağıdaki resimde gösterildiği gibi **WebContent** olarak tutun ve ardından **İleri**'ye tıklayın.

    ![Yeni bir JSP Dosyası Oluşturma - Java Web Uygulaması Öğreticisi](./media/documentdb-java-application/image11.png)

5. **Select JSP Template (JSP Şablon Seçme)** iletişim kutusunda bu öğreticinin amacı doğrultusunda **New JSP File (html) (Yeni JSP Dosyası (html))** seçeneğini belirleyin ve ardından **Finish (Son)** düğmesine tıklayın.

6. index.jsp dosyası Eclipse'te açıldığında, var olan ** öğesinin içinde** <body>Hello World! (Merhaba Dünya!) ifadesinin görüntülenmesi için metni ekleyin. Güncelleştirilmiş <body> içeriğiniz aşağıdaki kod gibi görünmelidir:

        <body>
            <% out.println("Hello World!"); %>
        </body>

8. index.jsp dosyasını kaydedin.
9. 2. adımda bir hedef çalışma zamanı ayarlarsanız **Proje**'ye ve ardından **Çalıştır**'a tıklayıp JSP uygulamanızı yerel olarak çalıştırabilirsiniz:

    ![Hello World - Java Uygulaması Öğreticisi](./media/documentdb-java-application/image12.png)

##<a id="InstallSDK"></a>3. Adım: DocumentDB Java SDK'sını yükleme ##

[Apache Maven](http://maven.apache.org/), DocumentDB Java SDK'sını ve bağımlılıklarını çekmenin en kolay yolunu sağlar.

Bunu yapmak için aşağıdaki adımları tamamlayarak projenizi bir Maven projesine dönüştürmeniz gerekir:

1. Proje Gezgini'nde projenize sağ tıklayın, **Yapılandır**'a tıklayın, **Maven Projesine Dönüştür**'e tıklayın.
2. **Yeni POM oluştur** penceresinde varsayılanları kabul edin ve **Son**'a tıklayın.
3. **Proje Gezgini**'nde pom.xml dosyasını açın.
4. **Bağımlılıklar** sekmesindeki **Bağımlılıklar** bölmesinde **Ekle**'ye tıklayın.
4. **Bağımlılık Seç** penceresinde aşağıdakileri yapın:
 - **GroupId** kutusuna com.microsoft.azure girin.
 - **Artifact Id** kutusuna azure-documentdb girin.
 - **Sürüm** kutusuna 1.5.1 girin.

    ![DocumentDB Java Uygulaması SDK'sını yükleme](./media/documentdb-java-application/image13.png)

    Alternatif olarak, GroupId ve ArtifactId için bağımlılık XML'sini bir metin düzenleyicisi aracılığıyla doğrudan pom.xml'ye ekleyin:

        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-documentdb</artifactId>
            <version>1.5.1</version>
        </dependency>

5. **Tamam**'a tıkladığınızda Maven DocumentDB Java SDK'sını yükler.
6. Pom.xml dosyasını kaydedin.

##<a id="UseService"></a>4. Adım: DocumentDB hizmetini bir Java uygulamasında kullanma

1. İlk olarak, TodoItem nesnesini tanımlayalım:

        @Data
        @Builder
        public class TodoItem {
            private String category;
            private boolean complete;
            private String id;
            private String name;
        }

    Bu projede oluşturucuyu, alıcıları ve ayarlayıcıları oluşturmak için [Project Lombok](http://projectlombok.org/)'u kullanıyoruz. Alternatif olarak, bu kodu el ile yazabilir veya IDE'nin oluşturmasını sağlayabilirsiniz.

2. DocumentDB hizmetini çağırmak için yeni bir **DocumentClient** örneği oluşturmanız gerekir. Genel olarak, sonraki her istek için yeni bir istemci oluşturmak yerine **DocumentClient**'ı tekrar kullanmak en iyisidir. İstemciyi bir **DocumentClientFactory**'ye sarmalayarak tekrar kullanabiliriz. [1. adımda](#CreateDB) panonuza kopyaladığınız URI ve BİRİNCİL ANAHTAR değerini de yapıştırmanız gereken yer burasıdır. [YOUR\_ENDPOINT\_HERE] yerine URI'nizi ve [YOUR\_KEY\_HERE] yerine BİRİNCİL ANAHTARINIZI girin.

        private static final String HOST = "[YOUR_ENDPOINT_HERE]";
        private static final String MASTER_KEY = "[YOUR_KEY_HERE]";

        private static DocumentClient documentClient;

        public static DocumentClient getDocumentClient() {
            if (documentClient == null) {
                documentClient = new DocumentClient(HOST, MASTER_KEY,
                        ConnectionPolicy.GetDefault(), ConsistencyLevel.Session);
            }

            return documentClient;
        }

3. Şimdi Yapılacaklar öğelerimizi DocumentDB'de kalıcı hale getirmeyi özetlemek için bir Veri Erişim Nesnesi (DAO) oluşturalım.

    Yapılacaklar öğelerini bir koleksiyona kaydetmek için, istemcinin hangi veritabanı ve koleksiyona kalıcı hale getireceğini (kendine bağlantılar tarafından başvurulduğu üzere) bilmesi gerekir. Genel olarak, veritabanına ek gidiş gelişleri önlemek için veritabanı ve koleksiyonu mümkün olduğunda ön belleğe almak en iyisidir.

    Aşağıdaki kod, veritabanımızın ve koleksiyonumuzun varsa nasıl alınacağını veya yoksa yenisinin nasıl oluşturulacağını gösterir:

        public class DocDbDao implements TodoDao {
            // The name of our database.
            private static final String DATABASE_ID = "TodoDB";

            // The name of our collection.
            private static final String COLLECTION_ID = "TodoCollection";

            // The DocumentDB Client
            private static DocumentClient documentClient = DocumentClientFactory
                    .getDocumentClient();

            // Cache for the database object, so we don't have to query for it to
            // retrieve self links.
            private static Database databaseCache;

            // Cache for the collection object, so we don't have to query for it to
            // retrieve self links.
            private static DocumentCollection collectionCache;

            private Database getTodoDatabase() {
                if (databaseCache == null) {
                    // Get the database if it exists
                    List<Database> databaseList = documentClient
                            .queryDatabases(
                                    "SELECT * FROM root r WHERE r.id='" + DATABASE_ID
                                            + "'", null).getQueryIterable().toList();

                    if (databaseList.size() > 0) {
                        // Cache the database object so we won't have to query for it
                        // later to retrieve the selfLink.
                        databaseCache = databaseList.get(0);
                    } else {
                        // Create the database if it doesn't exist.
                        try {
                            Database databaseDefinition = new Database();
                            databaseDefinition.setId(DATABASE_ID);

                            databaseCache = documentClient.createDatabase(
                                    databaseDefinition, null).getResource();
                        } catch (DocumentClientException e) {
                            // TODO: Something has gone terribly wrong - the app wasn't
                            // able to query or create the collection.
                            // Verify your connection, endpoint, and key.
                            e.printStackTrace();
                        }
                    }
                }

                return databaseCache;
            }

            private DocumentCollection getTodoCollection() {
                if (collectionCache == null) {
                    // Get the collection if it exists.
                    List<DocumentCollection> collectionList = documentClient
                            .queryCollections(
                                    getTodoDatabase().getSelfLink(),
                                    "SELECT * FROM root r WHERE r.id='" + COLLECTION_ID
                                            + "'", null).getQueryIterable().toList();

                    if (collectionList.size() > 0) {
                        // Cache the collection object so we won't have to query for it
                        // later to retrieve the selfLink.
                        collectionCache = collectionList.get(0);
                    } else {
                        // Create the collection if it doesn't exist.
                        try {
                            DocumentCollection collectionDefinition = new DocumentCollection();
                            collectionDefinition.setId(COLLECTION_ID);

                            collectionCache = documentClient.createCollection(
                                    getTodoDatabase().getSelfLink(),
                                    collectionDefinition, null).getResource();
                        } catch (DocumentClientException e) {
                            // TODO: Something has gone terribly wrong - the app wasn't
                            // able to query or create the collection.
                            // Verify your connection, endpoint, and key.
                            e.printStackTrace();
                        }
                    }
                }

                return collectionCache;
            }
        }

4. Sonraki adım, TodoItems öğelerini koleksiyonda kalıcı hale getirmek için biraz kod yazmaktır. Bu örnekte TodoItem Eski Basit Java Nesnelerini (POJO'lar) JSON belgelerine seri hale getirmek ve seri durumdan çıkarmak için [Gson](https://code.google.com/p/google-gson/)'u kullanacağız. [Jackson](http://jackson.codehaus.org/) veya kendi özel seri hale getiriciniz de POJO'ları serileştirmek için harika alternatiflerdir.

        // We'll use Gson for POJO <=> JSON serialization for this example.
        private static Gson gson = new Gson();

        @Override
        public TodoItem createTodoItem(TodoItem todoItem) {
            // Serialize the TodoItem as a JSON Document.
            Document todoItemDocument = new Document(gson.toJson(todoItem));

            // Annotate the document as a TodoItem for retrieval (so that we can
            // store multiple entity types in the collection).
            todoItemDocument.set("entityType", "todoItem");

            try {
                // Persist the document using the DocumentClient.
                todoItemDocument = documentClient.createDocument(
                        getTodoCollection().getSelfLink(), todoItemDocument, null,
                        false).getResource();
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return null;
            }

            return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
        }



5. DocumentDB veritabanları ve koleksiyonlarına benzer şekilde belgelere de kendine bağlantılar tarafından başvurulur. Aşağıdaki yardımcı işlevi, belgeleri kendine bağlantı yerine başka bir öznitelik (ör. "id") aracılığıyla almamızı sağlar:

        private Document getDocumentById(String id) {
            // Retrieve the document using the DocumentClient.
            List<Document> documentList = documentClient
                    .queryDocuments(getTodoCollection().getSelfLink(),
                            "SELECT * FROM root r WHERE r.id='" + id + "'", null)
                    .getQueryIterable().toList();

            if (documentList.size() > 0) {
                return documentList.get(0);
            } else {
                return null;
            }
        }

6. Id ile bir TodoItem JSON belgesini alıp ardından bunu bir POJO'ya seri durumdan çıkarmak için 5. adımdaki yardımcı yöntemi kullanabiliriz:

        @Override
        public TodoItem readTodoItem(String id) {
            // Retrieve the document by id using our helper method.
            Document todoItemDocument = getDocumentById(id);

            if (todoItemDocument != null) {
                // De-serialize the document in to a TodoItem.
                return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
            } else {
                return null;
            }
        }

7. DocumentDB SQL kullanarak bir koleksiyonu veya TodoItems listesini almak için DocumentClient'ı da kullanabiliriz:

        @Override
        public List<TodoItem> readTodoItems() {
            List<TodoItem> todoItems = new ArrayList<TodoItem>();

            // Retrieve the TodoItem documents
            List<Document> documentList = documentClient
                    .queryDocuments(getTodoCollection().getSelfLink(),
                            "SELECT * FROM root r WHERE r.entityType = 'todoItem'",
                            null).getQueryIterable().toList();

            // De-serialize the documents in to TodoItems.
            for (Document todoItemDocument : documentList) {
                todoItems.add(gson.fromJson(todoItemDocument.toString(),
                        TodoItem.class));
            }

            return todoItems;
        }

8. DocumentClient ile bir belgeyi güncelleştirmenin birçok yolu vardır. Yapılacaklar listesi uygulamamızda bir TodoItem'ın tamamlandı ve tamamlanmadı durumları arasında geçiş yapabilmek istiyoruz. Belgenin içinde "tamamlandı" özniteliğini güncelleştirerek bunu yapabiliriz:

        @Override
        public TodoItem updateTodoItem(String id, boolean isComplete) {
            // Retrieve the document from the database
            Document todoItemDocument = getDocumentById(id);

            // You can update the document as a JSON document directly.
            // For more complex operations - you could de-serialize the document in
            // to a POJO, update the POJO, and then re-serialize the POJO back in to
            // a document.
            todoItemDocument.set("complete", isComplete);

            try {
                // Persist/replace the updated document.
                todoItemDocument = documentClient.replaceDocument(todoItemDocument,
                        null).getResource();
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return null;
            }

            return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
        }

9. Son olarak, listemizden bir TodoItem'ı silebilmek istiyoruz. Bunu yapmak için daha önce yazmış olduğumuz yardımcı yöntemi kullanarak kendine bağlantıyı alıp ardından istemciye bunu silmesini söyleyebiliriz:

        @Override
        public boolean deleteTodoItem(String id) {
            // DocumentDB refers to documents by self link rather than id.

            // Query for the document to retrieve the self link.
            Document todoItemDocument = getDocumentById(id);

            try {
                // Delete the document by self link.
                documentClient.deleteDocument(todoItemDocument.getSelfLink(), null);
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return false;
            }

            return true;
        }


##<a id="Wire"></a>5. Adım: Java uygulaması geliştirme projesinin geriye kalan kısmını bağlama

Artık eğlenceli kısımları tamamladığımıza göre, geriye sadece hızlı bir kullanıcı arabirimi oluşturmak ve bunu DAO'muza bağlamak kaldı.

1. İlk olarak, DAO'muzu çağırmak için bir denetleyici oluşturmakla başlayalım:

        public class TodoItemController {
            public static TodoItemController getInstance() {
                if (todoItemController == null) {
                    todoItemController = new TodoItemController(TodoDaoFactory.getDao());
                }
                return todoItemController;
            }

            private static TodoItemController todoItemController;

            private final TodoDao todoDao;

            TodoItemController(TodoDao todoDao) {
                this.todoDao = todoDao;
            }

            public TodoItem createTodoItem(@NonNull String name,
                    @NonNull String category, boolean isComplete) {
                TodoItem todoItem = TodoItem.builder().name(name).category(category)
                        .complete(isComplete).build();
                return todoDao.createTodoItem(todoItem);
            }

            public boolean deleteTodoItem(@NonNull String id) {
                return todoDao.deleteTodoItem(id);
            }

            public TodoItem getTodoItemById(@NonNull String id) {
                return todoDao.readTodoItem(id);
            }

            public List<TodoItem> getTodoItems() {
                return todoDao.readTodoItems();
            }

            public TodoItem updateTodoItem(@NonNull String id, boolean isComplete) {
                return todoDao.updateTodoItem(id, isComplete);
            }
        }

    Karmaşık bir uygulamada, denetleyici DAO'nun üstünde karmaşık bir iş mantığı barındırabilir.

2. Ardından, HTTP isteklerini denetleyiciye yönlendirmek için bir servlet oluşturacağız:

        public class TodoServlet extends HttpServlet {
            // API Keys
            public static final String API_METHOD = "method";

            // API Methods
            public static final String CREATE_TODO_ITEM = "createTodoItem";
            public static final String GET_TODO_ITEMS = "getTodoItems";
            public static final String UPDATE_TODO_ITEM = "updateTodoItem";

            // API Parameters
            public static final String TODO_ITEM_ID = "todoItemId";
            public static final String TODO_ITEM_NAME = "todoItemName";
            public static final String TODO_ITEM_CATEGORY = "todoItemCategory";
            public static final String TODO_ITEM_COMPLETE = "todoItemComplete";

            public static final String MESSAGE_ERROR_INVALID_METHOD = "{'error': 'Invalid method'}";

            private static final long serialVersionUID = 1L;
            private static final Gson gson = new Gson();

            @Override
            protected void doGet(HttpServletRequest request,
                    HttpServletResponse response) throws ServletException, IOException {

                String apiResponse = MESSAGE_ERROR_INVALID_METHOD;

                TodoItemController todoItemController = TodoItemController
                        .getInstance();

                String id = request.getParameter(TODO_ITEM_ID);
                String name = request.getParameter(TODO_ITEM_NAME);
                String category = request.getParameter(TODO_ITEM_CATEGORY);
                boolean isComplete = StringUtils.equalsIgnoreCase("true",
                        request.getParameter(TODO_ITEM_COMPLETE)) ? true : false;

                switch (request.getParameter(API_METHOD)) {
                case CREATE_TODO_ITEM:
                    apiResponse = gson.toJson(todoItemController.createTodoItem(name,
                            category, isComplete));
                    break;
                case GET_TODO_ITEMS:
                    apiResponse = gson.toJson(todoItemController.getTodoItems());
                    break;
                case UPDATE_TODO_ITEM:
                    apiResponse = gson.toJson(todoItemController.updateTodoItem(id,
                            isComplete));
                    break;
                default:
                    break;
                }

                response.getWriter().println(apiResponse);
            }

            @Override
            protected void doPost(HttpServletRequest request,
                    HttpServletResponse response) throws ServletException, IOException {
                doGet(request, response);
            }
        }

3. Kullanıcıya gösterilecek bir Web Kullanıcı Arabirimine ihtiyacımız olacak. Daha önce oluşturduğumuz index.jsp'yi yeniden yazalım:

        <html>
        <head>
          <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
          <meta http-equiv="X-UA-Compatible" content="IE=edge;" />
          <title>Azure DocumentDB Java Sample</title>

          <!-- Bootstrap -->
          <link href="//ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css" rel="stylesheet">

          <style>
            /* Add padding to body for fixed nav bar */
            body {
              padding-top: 50px;
            }
          </style>
        </head>
        <body>
          <!-- Nav Bar -->
          <div class="navbar navbar-inverse navbar-fixed-top" role="navigation">
            <div class="container">
              <div class="navbar-header">
                <a class="navbar-brand" href="#">My Tasks</a>
              </div>
            </div>
          </div>

          <!-- Body -->
          <div class="container">
            <h1>My ToDo List</h1>

            <hr/>

            <!-- The ToDo List -->
            <div class = "todoList">
              <table class="table table-bordered table-striped" id="todoItems">
                <thead>
                  <tr>
                    <th>Name</th>
                    <th>Category</th>
                    <th>Complete</th>
                  </tr>
                </thead>
                <tbody>
                </tbody>
              </table>

              <!-- Update Button -->
              <div class="todoUpdatePanel">
                <form class="form-horizontal" role="form">
                  <button type="button" class="btn btn-primary">Update Tasks</button>
                </form>
              </div>

            </div>

            <hr/>

            <!-- Item Input Form -->
            <div class="todoForm">
              <form class="form-horizontal" role="form">
                <div class="form-group">
                  <label for="inputItemName" class="col-sm-2">Task Name</label>
                  <div class="col-sm-10">
                    <input type="text" class="form-control" id="inputItemName" placeholder="Enter name">
                  </div>
                </div>

                <div class="form-group">
                  <label for="inputItemCategory" class="col-sm-2">Task Category</label>
                  <div class="col-sm-10">
                    <input type="text" class="form-control" id="inputItemCategory" placeholder="Enter category">
                  </div>
                </div>

                <button type="button" class="btn btn-primary">Add Task</button>
              </form>
            </div>

          </div>

          <!-- Placed at the end of the document so the pages load faster -->
          <script src="//ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js"></script>
          <script src="//ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js"></script>
          <script src="assets/todo.js"></script>
        </body>
        </html>

4. Son olarak, web kullanıcı arabirimi ile servlet'i birbirine bağlamak için biraz istemci tarafı Javascript'i yazın:

        var todoApp = {
          /*
           * API methods to call Java backend.
           */
          apiEndpoint: "api",

          createTodoItem: function(name, category, isComplete) {
            $.post(todoApp.apiEndpoint, {
                "method": "createTodoItem",
                "todoItemName": name,
                "todoItemCategory": category,
                "todoItemComplete": isComplete
              },
              function(data) {
                var todoItem = data;
                todoApp.addTodoItemToTable(todoItem.id, todoItem.name, todoItem.category, todoItem.complete);
              },
              "json");
          },

          getTodoItems: function() {
            $.post(todoApp.apiEndpoint, {
                "method": "getTodoItems"
              },
              function(data) {
                var todoItemArr = data;
                $.each(todoItemArr, function(index, value) {
                  todoApp.addTodoItemToTable(value.id, value.name, value.category, value.complete);
                });
              },
              "json");
          },

          updateTodoItem: function(id, isComplete) {
            $.post(todoApp.apiEndpoint, {
                "method": "updateTodoItem",
                "todoItemId": id,
                "todoItemComplete": isComplete
              },
              function(data) {},
              "json");
          },

          /*
           * UI Methods
           */
          addTodoItemToTable: function(id, name, category, isComplete) {
            var rowColor = isComplete ? "active" : "warning";

            todoApp.ui_table().append($("<tr>")
              .append($("<td>").text(name))
              .append($("<td>").text(category))
              .append($("<td>")
                .append($("<input>")
                  .attr("type", "checkbox")
                  .attr("id", id)
                  .attr("checked", isComplete)
                  .attr("class", "isComplete")
                ))
              .addClass(rowColor)
            );
          },

          /*
           * UI Bindings
           */
          bindCreateButton: function() {
            todoApp.ui_createButton().click(function() {
              todoApp.createTodoItem(todoApp.ui_createNameInput().val(), todoApp.ui_createCategoryInput().val(), false);
              todoApp.ui_createNameInput().val("");
              todoApp.ui_createCategoryInput().val("");
            });
          },

          bindUpdateButton: function() {
            todoApp.ui_updateButton().click(function() {
              // Disable button temporarily.
              var myButton = $(this);
              var originalText = myButton.text();
              $(this).text("Updating...");
              $(this).prop("disabled", true);

              // Call api to update todo items.
              $.each(todoApp.ui_updateId(), function(index, value) {
                todoApp.updateTodoItem(value.name, value.value);
                $(value).remove();
              });

              // Re-enable button.
              setTimeout(function() {
                myButton.prop("disabled", false);
                myButton.text(originalText);
              }, 500);
            });
          },

          bindUpdateCheckboxes: function() {
            todoApp.ui_table().on("click", ".isComplete", function(event) {
              var checkboxElement = $(event.currentTarget);
              var rowElement = $(event.currentTarget).parents('tr');
              var id = checkboxElement.attr('id');
              var isComplete = checkboxElement.is(':checked');

              // Toggle table row color
              if (isComplete) {
                rowElement.addClass("active");
                rowElement.removeClass("warning");
              } else {
                rowElement.removeClass("active");
                rowElement.addClass("warning");
              }

              // Update hidden inputs for update panel.
              todoApp.ui_updateForm().children("input[name='" + id + "']").remove();

              todoApp.ui_updateForm().append($("<input>")
                .attr("type", "hidden")
                .attr("class", "updateComplete")
                .attr("name", id)
                .attr("value", isComplete));

            });
          },

          /*
           * UI Elements
           */
          ui_createNameInput: function() {
            return $(".todoForm #inputItemName");
          },

          ui_createCategoryInput: function() {
            return $(".todoForm #inputItemCategory");
          },

          ui_createButton: function() {
            return $(".todoForm button");
          },

          ui_table: function() {
            return $(".todoList table tbody");
          },

          ui_updateButton: function() {
            return $(".todoUpdatePanel button");
          },

          ui_updateForm: function() {
            return $(".todoUpdatePanel form");
          },

          ui_updateId: function() {
            return $(".todoUpdatePanel .updateComplete");
          },

          /*
           * Install the TodoApp
           */
          install: function() {
            todoApp.bindCreateButton();
            todoApp.bindUpdateButton();
            todoApp.bindUpdateCheckboxes();

            todoApp.getTodoItems();
          }
        };

        $(document).ready(function() {
          todoApp.install();
        });

5. Harika! Şimdi geriye yalnızca uygulamayı test etmek kaldı. Uygulamayı yerel olarak çalıştırın, ardından öğe adı ve kategoriyi doldurarak ve **Görev Ekle**'ye tıklayarak birkaç Yapılacaklar öğesi ekleyin.

6. Öğe göründükten sonra, onay kutusundaki işareti değiştirip **Görevleri Güncelleştir**'e tıklayarak öğeyi tamamlandı veya tamamlanmadı olarak güncelleştirebilirsiniz.

##<a id="Deploy"></a>6. Adım: Java uygulamanızı Azure Web Siteleri'ne dağıtma

Azure Web Siteleri Java Uygulamalarını dağıtmayı, uygulamanızı bir WAR dosyası olarak dışarı aktarmak ve kaynak denetimi (ör. GIT) veya FTP aracılığıyla karşıya yüklemek kadar basit hale getirir.

1. Uygulamanızı bir WAR olarak dışarı aktarmak için **Proje Gezgini**'nde projenize sağ tıklayın, **Dışarı Aktar**'a tıklayın ve ardından **WAR Dosyası**'na tıklayın.
2. **WAR Dışarı Aktar** penceresinde aşağıdakileri yapın:
 - Web projesi kutusuna azure-documentdb-java-sample metnini girin.
 - Hedef kutusunda WAR dosyasını kaydetmek için bir hedef seçin.
 - **Son**'a tıklayın.

3. Artık elinizde bir WAR dosyası olduğuna göre, bunu Azure Web Sitenizin **webapps** dizinine yüklemeniz yeterlidir. Dosyayı karşıya yükleme konusunda yönergeler için bkz. [Azure'daki Java web sitenize bir uygulama ekleme](../app-service-web/web-sites-java-add-app.md).

    WAR dosyası webapps dizinine yüklendikten sonra, çalışma zamanı ortamı bunu eklemiş olduğunuzu algılar ve otomatik olarak yükler.
4. Tamamlanmış ürününüzü görmek için http://\_SİTENİZİN\_ADI.azurewebsites.net/azure-documentdb-java-sample/ adresine gidin ve görevlerinizi eklemeye başlayın!

##<a id="GetProject"></a>Projeyi GitHub'dan alma

Bu öğreticideki tüm örnekler GitHub'daki [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) projesinde bulunur. todo projesini Eclipse'e aktarmak için [Önkoşullar](#Prerequisites) bölümünde listelenen yazılım ve kaynaklara sahip olduğunuzdan emin olun ve ardından aşağıdakileri yapın:

1. [Proje Lombok](http://projectlombok.org/)'u yükleyin. Lombok projede oluşturucular, alıcılar ve ayarlayıcılar oluşturmak için kullanılır. Lombok.jar dosyasını indirdikten sonra, yüklemek için buna çift tıklayın veya komut satırından yükleyin.
2. Eclipse açıksa kapatın ve Lombok'u yüklemek için yeniden başlatın.
3. Eclipse'te **Dosya** menüsünde **İçeri Aktar**'a tıklayın.
4. **İçeri Aktar** penceresinde **Git**'e tıklayın, **Git Projeleri**'ne tıklayın ve ardından **İleri**'ye tıklayın.
5. **Depo Kaynağı Seçin** ekranında **URI'yi kopyalama**'ya tıklayın.
6. **Kaynak Git Deposu** ekranında **URI** kutusuna https://github.com/Azure-Samples/documentdb-java-todo-app.git adresini girin ve ardından **İleri**'ye tıklayın.
7. **Dal Seçimi** ekranında **master**'ın seçili olduğundan emin olun ve ardından **İleri**'ye tıklayın.
8. **Yerel Hedef** ekranında deponun kopyalanabileceği bir klasör seçmek için **Gözat**'a tıklayın ve ardından **İleri**'ye tıklayın.
9. **Projeleri içeri aktarmada kullanmak için sihirbaz seçin** ekranında **Var olan projeleri içeri aktar**'ın seçili olduğundan emin olun ve ardından **İleri**'ye tıklayın.
10. **Projeleri İçeri Aktar** ekranında **DocumentDB** projesinin seçimini kaldırın ve ardından **Son**'a tıklayın. DocumentDB projesi, daha sonra bağımlılık olarak ekleyeceğimiz DocumentDB Java SDK'sını içerir.
11. **Proje Gezgini**'nde azure-documentdb-java-sample\src\com.microsoft.azure.documentdb.sample.dao\DocumentClientFactory.java konumuna gidin ve HOST ve MASTER_KEY değerlerini DocumentDB hesabınızın URI ve BİRİNCİL ANAHTAR değerleriyle değiştirin ve ardından dosyayı kaydedin. Daha fazla bilgi için bkz. [1. Adım. DocumentDB veritabanı hesabı oluşturma](#CreateDB).
12. **Proje Gezgini**'nde **azure-documentdb-java-sample**'a sağ tıklayın, **Yapı Yolu**'na tıklayın ve ardından **Oluşturma Yolunu Yapılandır**'a tıklayın.
13. **Java Oluşturma Yolu** ekranında sağ bölmedeki **Kitaplıklar** sekmesini seçin ve ardından **Dış JAR'lar Ekle**'ye tıklayın. Lombok.jar dosyasının konumuna gidin, **Aç**'a tıklayın ve ardından **Tamam**'a tıklayın.
14. **Özellikler** penceresini tekrar açmak için 12. adımı kullanın ve ardından sol bölmedeki **Hedeflenen Çalışma Zamanları**'na tıklayın.
15. **Hedeflenen Çalışma Zamanları** ekranında **Yeni**'ye tıklayın, **Apache Tomcat v7.0**'ı seçin ve ardından **Tamam**'a tıklayın.
16. **Özellikler** penceresini tekrar açmak için 12. adımı kullanın ve ardından sol bölmedeki **Proje Modelleri**'ne tıklayın.
17. **Proje Modelleri** ekranında **Dinamik Web Modülü**'nü ve **Java**'yı seçin ve ardından **Tamam**'a tıklayın.
18. Ekranın en altındaki **Sunucular** sekmesinde **Localhost'ta Tomcat v7.0 Sunucusu**'na sağ tıklayın ve ardından **Ekle ve Kaldır**'a tıklayın.
19. **Ekle ve Kaldır** penceresinde **azure-documentdb-java-sample**'ı **Yapılandırılmış** kutusuna taşıyın ve ardından **Son**'a tıklayın.
20. **Sunucu** sekmesinde **Localhost'ta Tomcat v7.0 Sunucusu**'na sağ tıklayın ve ardından **Yeniden Başlat**'a tıklayın.
21. Bir tarayıcıda http://localhost:8080/azure-documentdb-java-sample/ adresine gidin ve görev listenize eklemeye başlayın. Varsayılan bağlantı noktası değerlerinizi değiştirdiyseniz 8080'i seçtiğiniz değere değiştirmeyi unutmayın.
22. Projenizi bir Azure web sitesine dağıtmak için bkz. [6. Adım. Java uygulamanızı Azure Web Siteleri'ne dağıtma](#Deploy).

[1]: media/documentdb-java-application/keys.png



<!--HONumber=ago16_HO5-->


