<properties
    pageTitle="Çok katmanlı .NET uygulaması | Microsoft Azure"
    description="Azure'da katmanlar arasında iletişim sağlamak için Service Bus kuyruklarını kullanan çok katmanlı uygulama geliştirmenize yardımcı olan bir .NET öğreticisi."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="05/27/2016"
    ms.author="sethm"/>

# Azure Service Bus kuyruklarını kullanan çok katmanlı .NET uygulaması

## Giriş

Visual Studio ve .NET için ücretsiz Azure SDK kullanarak Microsoft Azure'a yönelik geliştirme işlemleri oldukça kolaydır. Bu öğretici, yerel ortamınızda çalışan birden çok Azure kaynağı kullanan bir uygulama oluşturmak için sizi adım adım yönlendirir. Öğreticideki adımlar, Azure kullanma konusunda deneyim sahibi olmadığınızı varsayar.

Şunları öğreneceksiniz:

-   Tek bir indirme ve yükleme işlemiyle bilgisayarınızı Azure geliştirmesine elverişli hale getirme.
-   Azure'a yönelik geliştirmeler için Visual Studio'yu kullanma.
-   Web ve çalışan rollerini kullanarak Azure'da çok katmanlı uygulama oluşturma.
-   Service Bus kuyruklarını kullanarak katmanlar arasında iletişim kurma.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

Bu öğreticide, Azure bulut hizmetinde çok katmanlı bir uygulama derleyip çalıştıracaksınız. Ön uç, ASP.NET MVC web rolü olurken, arka uç ise bir Service Bus kuyruğu kullanan çalışan rolü olacaktır. Aynı çok katmanlı uygulamayı, bulut hizmeti yerine bir Azure web sitesine dağıtılan web projesi özelliğine sahip ön uçla da oluşturabilirsiniz. Azure web sitesi ön ucunda gerçekleştirilebilecek farklı işlemler hakkındaki talimatlar için [Sonraki adımlar](#nextsteps) bölümüne bakın. Ayrıca, [.NET bulut/şirket içi karma uygulama](service-bus-dotnet-hybrid-app-using-service-bus-relay.md) öğreticisine de başvurabilirsiniz.

Aşağıdaki ekran görüntüsünde tamamlanan uygulama gösterilir.

![][0]

## Senaryoya genel bakış: roller arası iletişim

İşlemeye yönelik bir sipariş göndermek için web rolünde çalışan ön uç kullanıcı arabirimi bileşeninin çalışan rolünde çalışmakta olan orta katman mantığı ile etkileşim içinde olması gerekir. Bu örnekte, katmanlar arasındaki iletişim için Service Bus aracılı mesajlaşma kullanılır.

Web katmanı ve orta katman arasında aracılı mesajlaşmayı kullanma iki bileşeni birbirinden ayırır. Doğrudan mesajlaşmanın aksine (yani TCP veya HTTP), web katmanı orta katmanla doğrudan bağlantı kurmaz. Bunun yerine çalışma birimlerini iletiler şeklinde, orta katman bu birimleri kullanmaya ve işlemeye hazır olana kadar güvende tutan Service Bus hizmetine gönderir.

Service Bus, kuyruklar ve konu başlıkları olmak üzere aracılı mesajlaşmayı desteklemek için iki varlık sunar. Kuyruklar kısmında, kuyruğa gönderilen her ileti tek bir alıcı tarafından kullanılır. Konu başlıkları ise yayınlanan tüm iletilerin konu başlığına kayıtlı olan bir abonelikte kullanılabilir hale geldiği yayımla/paylaş düzenini destekler. Her abonelik mantıksal olarak kendi ileti kuyruğunu korur. Ayrıca abonelikler, filtreyle eşleşen abonelik kuyruğuna gönderilen ileti kümesini sınırlayan filtre kuralları ile yapılandırabilir. Aşağıdaki örnekte Service Bus kuyrukları kullanılır.

![][1]

Bu iletişim mekanizması, doğrudan mesajlaşma ile karşılaştırıldığında birçok avantaj sunar:

-   **Zamana bağlı ayırma.** Zaman uyumsuz mesajlaşma düzeni sayesinde üreticilerin ve tüketicilerin aynı anda çevrimiçi olması gerekmez. Service Bus, kullanıcı taraf iletileri almaya hazır olana kadar güvenli bir şekilde depolar. Bu özellik, dağıtılan uygulamanın bileşenlerinin isteğe bağlı olarak (örneğin, bakım için) veya bir bileşenin çökmesinden dolayı bağlantısının sistemin tamamını etkilemeden kesilmesine olanak sağlar. Ayrıca, kullanıcı uygulamanın yalnızca günün belirli zamanlarında çevrimiçi olması gerekebilir.

-   **Yük dengeleme.** Birçok uygulamada, sistem yükü zamana göre değişirken çalışmanın her birimi için gereken işleme süresi genel olarak aynıdır. Bir kuyruk aracılığıyla ileti üreticileri ve tüketicileri arasında bağlantı kurmak, kullanıcı uygulamaya (çalışan) en fazla yük yerine yalnızca ortalama yük sağlanması gerektiği anlamına gelir. Gelen yük hacmi değiştikçe kuyruğun derinliği artar ve daralır. Bu işlem, uygulama yükünü sunmak için gereken altyapı miktarı bağlamında doğrudan para tasarrufu sağlar.

-   **Yük dengeleme.** Yük arttıkça kuyruktan okunmak üzere daha fazla çalışan işlemi eklenebilir. Her ileti yalnızca bir çalışan işlemi tarafından işlenir. Ayrıca bu çekme tabanlı yük dengelemesi, çalışan makineler işleme gücü bağlamında farklılık gösterse bile çalışan makinelerin optimum kullanımına olanak sağlar. Çalışan makinelerin işleme gücündeki farklar, her birinin iletileri kendi maksimum hızında çekmesinden kaynaklanır. Bu düzen, genelde *rakip tüketici* düzeni olarak adlandırılır.

    ![][2]

Aşağıdaki bölümlerde, bu mimariyi uygulayan kod ele alınır.

## Geliştirme ortamını ayarlama

Azure uygulamalarını geliştirmeye başlamadan önce, araçları edinip geliştirme ortamınızı ayarlayın.

1.  [Araç ve SDK edinme][] bölümünde .NET için Azure SDK'sını yükleyin.

2.  Kullandığınız Visual Studio çözümü için **SDK'yi yükle**'ye tıklayın. Bu öğreticideki adımlarda Visual Studio 2015 kullanılır.

4.  Yükleyiciyi çalıştırmanız veya kaydetmeniz istendiğinde **Çalıştır**'a tıklayın.

5.  **Web Platformu Yükleyicisi**'nde **Yükle**'ye tıklayın ve kuruluma devam edin.

6.  Kurulum tamamlandığında uygulamayı geliştirmeye başlamak için gereken her şeye sahip olacaksınız. SDK, Visual Studio'da Azure uygulamalarını kolayca geliştirmenize olanak sağlayan araçları içerir. Visual Studio yüklü değilse SDK ücretsiz Visual Studio Express de yükler.

## Service Bus ad alanı oluşturma

İlk adım, bir hizmet ad alanı oluşturmak ve Paylaşılan Erişim İmzası (SAS) anahtarı edinmektir. Ad alanı, Service Bus tarafından kullanıma sunulan her uygulama için bir uygulama sınırı sağlar. Hizmet ad alanı oluşturulduğunda sistem tarafından bir SAS anahtarı oluşturulur. Ad alanı ve SAS anahtarı birleşimi ile Service Bus hizmetinin bir uygulamaya erişim kimliğini doğrulayan kimlik bilgisi sağlanır.

### Klasik Azure portalını kullanarak ad alanı ayarlama

1.  [Klasik Azure portalında][] oturum açın.

2.  Portalın sol gezinti bölmesinde **Service Bus** hizmetine tıklayın.

3.  Portalın alt bölmesinde **Oluştur**'a tıklayın.

    ![][6]

4.  **Yeni bir ad alanı ekle** sayfasında, bir ad alanı adı girin. Adın kullanılabilirliği sistem tarafından hemen kontrol edilir.

    ![][7]

5.  Ad alanındaki adın kullanılabilirliğinden emin olduktan sonra, ad alanınızın barındırılması gereken ülkeyi veya bölgeyi seçin (işlem kaynaklarınızın dağıtıldığı ülkeyle aynı ülkeyi/bölgeyi kullandığınızdan emin olun). Ayrıca, ad alanının **Tür** alanı için **Mesajlaşma** ve **Mesajlaşma Kapsamı** alanı için **Standart** seçeneklerini belirlediğinizden emin olun.

    > [AZURE.IMPORTANT] Uygulamanızı dağıtmak için seçmeyi planladığınız **aynı bölgeyi** seçin. Bunu uygulayarak en iyi performansı alırsınız.

6.  Tamam onay işaretine tıklayın. Artık sistem hizmet ad alanınızı oluşturur ve kullanıma açar. Sistem, hesabınıza yönelik kaynakları sağlarken birkaç dakika beklemeniz gerekebilir.

7.  Ana pencerede, hizmet ad alanı adınıza tıklayın.

8. **Bağlantı Bilgileri**'ne tıklayın.

9.  **Erişim bağlantısı bilgileri** bölmesinde, SAS anahtarını ve anahtar adını içeren bağlantı dizesini bulun.

    ![][35]

10.  Bu kimlik bilgilerini not edin veya panoya kopyalayın.

11. Aynı portal sayfasında, sayfanın üst kısmındaki **Yapılandır** sekmesine tıklayın.

12. **RootManageSharedAccessKey** ilkesinin birincil anahtarını panoya kopyalayın veya Not Defteri'ne yapıştırın. Bu değeri daha sonra bu öğreticide kullanacaksınız.

    ![][36]

## Web rolü oluşturma

Bu bölümde, uygulamanızın ön ucunu derleyin. Öncelikle, uygulamanızda görüntülenecek sayfaları oluşturursunuz.
Daha sonra, Service Bus kuyruğuna öğe gönderen ve kuyruk hakkındaki durum bilgilerini gösteren kodu ekleyin.

### Proje oluşturma

1.  Yönetici ayrıcalıklarını kullanarak Microsoft Visual Studio'yu başlatın. Yönetici ayrıcalıklarıyla Visual Studio'yu başlatmak için **Visual Studio** programının simgesine sağ tıklayın ve ardından **Yönetici olarak çalıştır**'a tıklayın. Bu makalenin sonraki bölümlerinde ele alınan Azure işlem öykünücüsü, Visual Studio'nun yönetici ayrıcalıklarıyla başlatılmasını gerektirir.

    Visual Studio'da, **Dosya** menüsündeki **Yeni** seçeneğine ve ardından **Proje**'ye tıklayın.

2.  **Yüklü Şablonlar**'da **Visual C#** kısmında **Bulut** seçeneğine ve ardından **Azure Bulut Hizmeti**'ne tıklayın. Projeyi **MultiTierApp** olarak adlandırın. Daha sonra, **Tamam**'a tıklayın.

    ![][9]

3.  **.NET Framework 4.5** rollerinden **ASP.NET Web Rolü**'ne çift tıklayın.

    ![][10]

4.  **Azure Bulut Hizmeti çözümü** kısmında **WebRole1** öğesinin üzerine gelin, kurşun kalem simgesine tıklayın ve web rolünü **FrontendWebRole** olarak yeniden adlandırın. Daha sonra, **Tamam**'a tıklayın. ("Frontend" öğesini "FrontEnd" olarak değil de küçük 'e' ile yazdığınızdan emin olun.)

    ![][11]

5.  **Yeni ASP.NET Projesi** iletişim kutusundaki **Bir şablon seçin** listesinde **MVC**'ye tıklayın.

    ![][12]

6. Yine **Yeni ASP.NET projesi** iletişim kutusunda **Kimlik Doğrulamayı Değiştir** düğmesine tıklayın. **Kimlik Doğrulamayı Değiştir** iletişim kutusunda **Kimlik Doğrulama Yok** seçeneğine ve ardından **Tamam**'a tıklayın. Bu öğreticide kullanıcının oturum açmasını gerektirmeyen bir uygulamayı dağıtıyorsunuz.

    ![][16]

7. **Yeni ASP.NET Projesi** iletişim kutusuna geri döndükten sonra projeyi oluşturmak için **Tamam**'a tıklayın.

6.  **Çözüm Gezgini**'ndeki, **FrontendWebRole** projesinde **Başvurular** seçeneğine ve ardından **NuGet Paketlerini Yönet**'e tıklayın.

7.  **Gözat** sekmesine tıklayıp `Microsoft Azure Service Bus` için arama yapın. **Yükle**'ye tıklayın ve kullanım koşullarını kabul edin.

    ![][13]

    Gerekli istemci derlemelerine başvuru oluşturulduğunu ve bazı yeni kod dosyaları eklendiğini unutmayın.

9.  **Çözüm Gezgini**'nde, **Modeller**'e sağ tıklayın ve ardından **Ekle** ile **Sınıf** seçeneklerine tıklayın. **Ad** kutusuna **OnlineOrder.cs** yazın: Daha sonra **Ekle**'ye tıklayın.

### Web rolünüz için kod yazma

Bu bölümde, uygulamanızın görüntülediği çeşitli sayfalar oluşturursunuz.

1.  Visual Studio'daki OnlineOrder.cs dosyasında var olan ad alanı tanımını şu kod ile değiştirin:

    ```
    namespace FrontendWebRole.Models
    {
        public class OnlineOrder
        {
            public string Customer { get; set; }
            public string Product { get; set; }
        }
    }
    ```

2.  **Çözüm Gezgini**'nde **Controllers\HomeController.cs** öğesine çift tıklayın. Service Bus hizmetinin yanı sıra az önce oluşturduğunuz modele yönelik ad alanlarını da içermesi için dosyanın başına aşağıdaki **using** deyimlerini ekleyin.

    ```
    using FrontendWebRole.Models;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    ```

3.  Ayrıca, Visual Studio'daki HomeController.cs dosyasında var olan ad alanı tanımını da aşağıdaki kod ile değiştirin. Bu kod, öğelerin kuyruğa gönderilme işleminin ele alınmasına yönelik yöntemleri içerir.

    ```
    namespace FrontendWebRole.Controllers
    {
        public class HomeController : Controller
        {
            public ActionResult Index()
            {
                // Simply redirect to Submit, since Submit will serve as the
                // front page of this application.
                return RedirectToAction("Submit");
            }
    
            public ActionResult About()
            {
                return View();
            }
    
            // GET: /Home/Submit.
            // Controller method for a view you will create for the submission
            // form.
            public ActionResult Submit()
            {
                // Will put code for displaying queue message count here.
    
                return View();
            }
    
            // POST: /Home/Submit.
            // Controller method for handling submissions from the submission
            // form.
            [HttpPost]
            // Attribute to help prevent cross-site scripting attacks and
            // cross-site request forgery.  
            [ValidateAntiForgeryToken]
            public ActionResult Submit(OnlineOrder order)
            {
                if (ModelState.IsValid)
                {
                    // Will put code for submitting to queue here.
    
                    return RedirectToAction("Submit");
                }
                else
                {
                    return View(order);
                }
            }
        }
    }
    ```

4.  Çalışmanızın o ana kadarki doğruluğunu test etmek için **Derle** menüsünde **Çözümü Derle**'ye tıklayın.

5.  Şimdi daha önceden oluşturduğunuz `Submit()` yöntemi görünümünü oluşturun. `Submit()` yöntemi içinde sağ tıklayın (parametre almayan `Submit()` aşırı yükü) ve ardından **Görünüm Ekle**'yi seçin.

    ![][14]

6.  Görünüm oluşturmanız için bir iletişim kutusu belirir. **Şablon** listesinde **Oluştur** seçeneğini belirleyin. **Model sınıfı** listesinde **OnlineOrder** sınıfına tıklayın.

    ![][15]

7.  **Ekle**'ye tıklayın.

8.  Şimdi, uygulamanızın görüntülenen adını değiştirin. **Çözüm Gezgini**'nde, **Views\Shared\\_Layout.cshtml** dosyasına çift tıklayarak dosyayı Visual Studio düzenleyicisinde açın.

9.  **My ASP.NET Application** uygulamasının tüm örneklerini **LITWARE's Products** olarak değiştirin.

10. **Home**, **About** ve **Contact** bağlantılarını kaldırın. Vurgulanmış kodu silme:

    ![][28]

11. Son olarak, gönderim sayfasını kuyruk hakkındaki bazı bilgileri içerecek şekilde değiştirin. **Çözüm Gezgini**'nde, **Views\Home\Submit.cshtml** dosyasına çift tıklayarak dosyayı Visual Studio düzenleyicisinde açın. `<h2>Submit</h2>` öğesinden sonra aşağıdaki satırı ekleyin. `ViewBag.MessageCount`, şimdilik boştur. Bu alanı daha sonra dolduracaksınız.

    ```
    <p>Current number of orders in queue waiting to be processed: @ViewBag.MessageCount</p>
    ```

12. Şu anda kullanıcı arabiriminizi uyguladınız. Uygulamanızı çalıştırmak ve istediğiniz gibi göründüğünü doğrulamak için **F5**'e basabilirsiniz.

    ![][17]

### Service Bus kuyruğuna öğe göndermek için kod yazma

Şimdi, öğeleri kuyruğa göndermek için bir kod ekleyin. İlk olarak, Service Bus kuyruğunuzun bağlantı bilgilerini içeren bir sınıf oluşturun. Ardından, bağlantınızı Global.aspx.cs üzerinden başlatın. Son olarak, öğeleri gerçekten Service Bus kuyruğuna göndermek için daha önce HomeController.cs dosyasında oluşturduğunuz gönderme kodunu güncelleştirin.

1.  **Çözüm Gezgini**'nde, **FrontendWebRole** projesine sağ tıklayın (role değil, projeye tıklayın). **Ekle** seçeneğine ve ardından **Sınıf**'a tıklayın.

2.  Sınıfı **QueueConnector.cs** olarak adlandırın. Sınıf oluşturmak için **Ekle**'ye tıklayın.

3.  Şimdi, bağlantı bilgilerini yalıtan ve Service Bus kuyruğuna bağlantıyı başlatan kodu ekleyin. QueueConnector.cs öğesinin tüm içeriğini aşağıdaki kodla değiştirin ve `your Service Bus namespace` öğesi (ad alanındaki adınız) ile [Klasik Azure portalında][] "Service Bus ad alanı oluşturma" bölümünün 12. adımında el elde ettiğiniz **birincil anahtarınız olan** `yourKey` öğesi için değerleri girin.

    ```
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    
    namespace FrontendWebRole
    {
        public static class QueueConnector
        {
            // Thread-safe. Recommended that you cache rather than recreating it
            // on every request.
            public static QueueClient OrdersQueueClient;
    
            // Obtain these values from the portal.
            public const string Namespace = "your Service Bus namespace";
    
            // The name of your queue.
            public const string QueueName = "OrdersQueue";
    
            public static NamespaceManager CreateNamespaceManager()
            {
                // Create the namespace manager which gives you access to
                // management operations.
                var uri = ServiceBusEnvironment.CreateServiceUri(
                    "sb", Namespace, String.Empty);
                var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                    "RootManageSharedAccessKey", "yourKey");
                return new NamespaceManager(uri, tP);
            }
    
            public static void Initialize()
            {
                // Using Http to be friendly with outbound firewalls.
                ServiceBusEnvironment.SystemConnectivity.Mode =
                    ConnectivityMode.Http;
    
                // Create the namespace manager which gives you access to
                // management operations.
                var namespaceManager = CreateNamespaceManager();
    
                // Create the queue if it does not exist already.
                if (!namespaceManager.QueueExists(QueueName))
                {
                    namespaceManager.CreateQueue(QueueName);
                }
    
                // Get a client to the queue.
                var messagingFactory = MessagingFactory.Create(
                    namespaceManager.Address,
                    namespaceManager.Settings.TokenProvider);
                OrdersQueueClient = messagingFactory.CreateQueueClient(
                    "OrdersQueue");
            }
        }
    }
    ```

4.  Şimdi **Başlat** yönteminizin çağrıldığından emin olun. **Çözüm Gezgini**'nde **Global.asax\Global.asax.cs** öğesine çift tıklayın.

5.  **Application_Start** yönteminin sonuna aşağıdaki kod satırını ekleyin.

    ```
    FrontendWebRole.QueueConnector.Initialize();
    ```

6.  Son olarak, öğeleri kuyruğa göndermek için daha önce oluşturduğunuz web kodunu güncelleştirin. **Çözüm Gezgini**'nde **Controllers\HomeController.cs** öğesine çift tıklayın.

7.  Kuyrukta bulunan ileti sayısını alması için `Submit()` yöntemini (parametre almayan aşırı yük) aşağıdaki şekilde güncelleştirin.

    ```
    public ActionResult Submit()
    {
        // Get a NamespaceManager which allows you to perform management and
        // diagnostic operations on your Service Bus queues.
        var namespaceManager = QueueConnector.CreateNamespaceManager();
    
        // Get the queue, and obtain the message count.
        var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
        ViewBag.MessageCount = queue.MessageCount;
    
        return View();
    }
    ```

8.  Kuyruğa ilişkin sipariş bilgilerini alması için `Submit(OnlineOrder order)` yöntemini (bir parametre alan aşırı yük) aşağıdaki şekilde güncelleştirin.

    ```
    public ActionResult Submit(OnlineOrder order)
    {
        if (ModelState.IsValid)
        {
            // Create a message from the order.
            var message = new BrokeredMessage(order);
    
            // Submit the order.
            QueueConnector.OrdersQueueClient.Send(message);
            return RedirectToAction("Submit");
        }
        else
        {
            return View(order);
        }
    }
    ```

9.  Şimdi uygulamayı tekrar çalıştırabilirsiniz. Siz her sipariş gönderdiğinizde ileti sayısı da artar.

    ![][18]

## Çalışan rolü oluşturma

Şimdi, sipariş gönderimlerini işleyen çalışan rolünü oluşturacaksınız. Bu örnekte, **Service Bus Kuyruğu İçeren Çalışan Rolü** Visual Studio proje şablonu kullanılır. Gerekli kimlik bilgilerini zaten portaldan almıştınız.

1. Visual Studio'yu Azure hesabınıza bağladığınızdan emin olun.

2.  Visual Studio'da bulunan **Çözüm Gezgini**'ndeki **MultiTierApp** projesi kısmında **Roller**'e çift tıklayın.

3.  **Ekle**'ye ve ardından **Yeni Çalışan Rolü Projesi**'ne tıklayın. **Yeni Çalışan Rolü Projesi** iletişim kutusu görünür.

    ![][26]

4.  **Yeni Rol Projesi Ekle** iletişim kutusunda **Service Bus kuyruğu içeren Çalışan Rolü**'ne tıklayın.

    ![][23]

5.  **Ad** kutusunda projeyi **OrderProcessingRole** olarak adlandırın. Daha sonra **Ekle**'ye tıklayın.

6.  "Service Bus ad alanı oluşturma" bölümünün 9. adımında elde ettiğiniz bağlantı dizesini panoya kopyalayın.

7.  **Çözüm Gezgini**'nde, 5.adımda oluşturduğunuz **OrderProcessingRole** rolüne çift tıklayın (sınıf kısmındakine değil de, **Roller** bölümündeki **OrderProcessingRole** öğesine çift tıkladığınızdan emin olun). Daha sonra **Özellikler**'e tıklayın.

8.  **Özellikler** iletişim kutusunun **Ayarlar** sekmesinde, **Microsoft.ServiceBus.ConnectionString** için **Değer** kutusuna çift tıklayın ve 6.adımda kopyaladığınız uç nokta değerini yapıştırın.

    ![][25]

9.  Kuyruktan işlediğiniz siparişleri temsil etmesi için **OnlineOrder** sınıfı oluşturun. Önceden oluşturduğunuz bir sınıfı yeniden kullanabilirsiniz. **Çözüm Gezgini**'nde, **OrderProcessingRole** sınıfına sağ tıklayın (rol simgesine değil, sınıf simgesine sağ tıklayın). **Ekle**'ye ve **Var Olan Öğe**'ye tıklayın.

10. **FrontendWebRole\Models** alt klasörüne gözatın ve ardından bu projeye eklemek için **OnlineOrder.cs** sınıfına çift tıklayın.

11. **WorkerRole.cs** içindeki `"ProcessingQueue"` olan **QueueName** değişkenin değerini aşağıdaki kodda gösterildiği şekilde `"OrdersQueue"` olarak değiştirin.

    ```
    // The name of your queue.
    const string QueueName = "OrdersQueue";
    ```

12. WorkerRole.cs dosyasının üst kısmına aşağıdaki using deyimini ekleyin.

    ```
    using FrontendWebRole.Models;
    ```

13. `Run()` işlevinde `OnMessage()` çağrısı içinde, `try` yan tümcesinin içeriğini aşağıdaki kodla değiştirin.

    ```
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View the message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```

14. Uygulamayı tamamladınız. Çözüm Gezgini'nde MultiTierApp projesine sağ tıklayarak uygulamanın tamamını test edebilirsiniz. **Başlangıç Projesi Olarak Ayarla**'yı seçip F5'e basın. Çalışan rolü öğeleri kuyruktan işlediğinden ve işlediği öğeleri tamamlanmış olarak işaretlediğinden ileti sayısında artış olmayacağını unutmayın. Azure İşlem Öykünücüsü kullanıcı arabiriminde görüntüleyerek çalışan rolünüzün izleme çıktısını görebilirsiniz. Bu işlemi, görev çubuğunuzdaki bildirim alanında bulunan öykünücü simgesine sağ tıklayıp **İşlem Öykünücüsü Kullanıcı Arabirimini Göster**'i seçerek gerçekleştirebilirsiniz.

    ![][19]

    ![][20]

## Sonraki adımlar  

Service Bus hakkında daha fazla bilgi edinmek için şu kaynaklara bakın:  

* [Azure Service Bus][sbmsdn]  
* [Service Bus hizmeti sayfası][sbwacom]  
* [Service Bus Kuyruklarını Kullanma][sbwacomqhowto]  

Çok katmanlı senaryolar hakkında daha fazla bilgi için bkz.  

* [Depolama Tabloları, Kuyrukları ve Blobları Kullanan .NET Çok Kapsamlı Uygulaması][multitierstorage]  

  [0]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-01.png
  [1]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-100.png
  [2]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-101.png
  [Araç ve SDK edinme]: http://go.microsoft.com/fwlink/?LinkId=271920


  [GetSetting]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.getsetting.aspx
  [Microsoft.WindowsAzure.Configuration.CloudConfigurationManager]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx
  [NamespaceMananger]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx

  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx

  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx

  [EventHubClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx

  [Klasik Azure portalında]: http://manage.windowsazure.com
  [6]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/sb-queues-03.png
  [7]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/sb-queues-04.png
  [9]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-10.png
  [10]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-11.png
  [11]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-02.png
  [12]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-12.png
  [13]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-13.png
  [14]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-33.png
  [15]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-34.png
  [16]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-14.png
  [17]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-36.png
  [18]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-37.png

  [19]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-38.png
  [20]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-39.png
  [23]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRole1.png
  [25]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRoleProperties.png
  [26]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBNewWorkerRole.png
  [28]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-40.png
  [35]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/multi-web-45.png
  [36]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/service-bus-policies.png

  [sbmsdn]: http://msdn.microsoft.com/library/azure/ee732537.aspx  
  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: service-bus-dotnet-how-to-use-queues.md  
  [multitierstorage]: https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36
  


<!---HONumber=Jun16_HO2-->


