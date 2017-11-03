# <a name="azure-service-bus"></a>Azure Service Bus
Bir uygulama veya hizmet ister bulutta ister şirket içinde çalışsın, genellikle diğer uygulamalarla veya hizmetlerle etkileşimde olmalıdır. Bunu yapmak için kapsamlı ve kullanışlı bir yol sağlamak üzere Azure hizmet veri yolu sunar. Bu makalede, hizmetin yapısına ve hizmeti kullanmanız durumunda elde edeceğiniz avantajlara yer verilerek bu teknoloji genel hatlarıyla incelenir.

## <a name="service-bus-fundamentals"></a>Service Bus ile ilgili temel bilgiler
Farklı çözümler, farklı iletişim stilleri gerektirir. Bazı durumlarda, uygulamaların basit bir kuyruk aracılığıyla iletileri almasına veya göndermesine izin vermek en iyi çözümdür. Diğer durumlarda ise sıradan bir kuyruk yeterli değildir; yayımla ve abone ol mekanizmasını içeren bir kuyruk daha faydalıdır. Ve bazı durumlarda, gerçekten gerekli olan tüm uygulamaları &#151;arasında bir bağlantı; kuyruklar gerekli değildir. Service Bus, uygulamalarınızın birçok farklı yöntemle etkileşim izin vererek her üç seçenek sunar.

Service Bus çok kiracılı bir bulut hizmetidir, yani hizmet birçok kullanıcı tarafından paylaşılır. Her kullanıcı (örneğin, uygulama geliştiricisi) bir *ad alanı* oluşturur ve ardından ad alanında ihtiyaç duyacağı iletişim mekanizmasını tanımlar. [Şekil 1](#Fig1) bu sistem gösterilmektedir.

<a name="Fig1"></a>![Azure hizmet veri yolu diyagramı][svc-bus]

**Şekil 1: Service Bus, bulut üzerinden uygulamaları bağlamak için çok kiracılı bir hizmet sunar.**

Ad alanı içinde, dört farklı iletişim mekanizmasının bir veya birden fazla örneğini kullanabilirsiniz. Bu mekanizmaların her biri, uygulamaları farklı yöntemlerle bağlar. Seçenekler şunlardır:

* Tek yönlü iletişime izin veren *kuyruklar*. Her kuyruk, alınana kadar gönderilen iletileri depolayan bir ara hizmet olarak görev yapar (bazen *aracı* olarak adlandırılır). Her ileti tek bir alıcı tarafından alınır.
* *Abonelikler* olmak üzere tek konu başlığı kullanan, tek yönlü iletişim sağlayan *Konu Başlıkları*, birden çok aboneliği barındırabilir. Kuyrukta olduğu gibi konu başlığı, aracı gibi davranır ancak her abonelik isteğe bağlı olarak yalnızca belirli kriterlerle eşleşen iletileri almak için filtre kullanabilir.
* Çift yönlü iletişim sunan *geçişler*. Kuyruk ve konu başlıklarının aksine, bir geçiş yürütülen iletileri depolamaz; yani bir aracı değildir. Bunun yerine iletileri yalnızca hedef uygulamaya gönderir.
* Düşük gecikme süresi ve yüksek güvenilirlikle buluta büyük ölçekte olay ve telemetri girişi sağlayan *Event Hubs*.

Bir kuyruk, konu başlığı, geçiş veya Event Hub oluşturduğunuzda, aynı zamanda bunları adlandırırsınız. Ad alanınız ne olursa olsun, bu ad nesne için benzersiz bir tanıtıcı oluşturur. Uygulamalar bu adı Service Bus'a verir ve ardından birbirleriyle iletişim kurmak için bu kuyruğu, konu başlığını, geçişi veya Event Hub hizmetini kullanır. 

Bu nesnelerden herhangi birini kullanmak için Windows uygulaması Windows Communication Foundation'ı (WCF) kullanabilir. Ayrıca, Windows uygulamaları kuyruklar, konu başlıkları ve Event Hubs için Service Bus tanımlı mesajlaşma API'lerini de kullanabilir. Bu nesnelerin Windows uygulaması olmayan uygulamalar tarafından kullanımını kolaylaştırmak için Microsoft Java, Node.js ve diğer dillere yönelik SDK sunar. Ayrıca kuyruklara, konu başlıklarına ve Event Hubs hizmetine HTTP üzerinden REST API'lerini kullanarak da erişebilirsiniz. 

Service Bus hizmeti bulutta (Microsoft'un Azure veri merkezlerinde) çalışıyor olsa da Service Bus hizmetini kullanan uygulamaların herhangi bir yerde çalışabileceğini kavramak önemlidir. Service Bus hizmetini Azure'da çalışan uygulamaları (örneğin, kendi veri merkezinizde çalışan uygulamalar) bağlamak için kullanabilirsiniz . Ayrıca, bu hizmeti Azure veya başka bulut platformunda çalışan bir uygulama ile şirket içi bir uygulamayı veya tablet ve telefonları bağlamak için de kullanabilirsiniz. Ev aletlerini, sensörleri ve diğer cihazları merkezi bir uygulamaya veya başka bir uygulamaya bağlamak bile mümkündür. Service Bus, neredeyse her yerden erişilebilen bulut tabanlı genel bir iletişim mekanizmasıdır. Service Bus hizmetini kullanım şekliniz uygulamanızın gereksinimlerine göre değişir.

## <a name="queues"></a>Kuyruklar
İki uygulamayı Service Bus kuyruğu kullanarak bağlamaya karar verdiğinizi varsayalım. [Şekil 2](#Fig2) bu durum gösterilir.

<a name="Fig2"></a>![Hizmet veri yolu kuyrukları diyagramı][queues]

**Şekil 2: Service Bus kuyrukları tek yönlü, zaman uyumsuz kuyruğa alma işlemi sunar.**

İşlem oldukça basittir: Bir gönderici Service Bus kuyruğuna ileti gönderir ve daha sonra alıcı bu iletiyi alır. Bir kuyruk olarak yalnızca bir alıcıya sahip [Şekil 2](#Fig2) gösterir ya da birden çok uygulama aynı kuyrukta iletileri okuyabilir. İkinci durumda, her ileti tek bir alıcı tarafından okunur. Çok noktaya yayın yapan bir hizmet için konu başlığı kullanmanız gerekir.

Her ileti iki bölümden oluşur: özellikler kümesi (her biri anahtar/değer çifti olmak üzere) ve ikili ileti gövdesi. Bunların nasıl kullanıldığı, uygulama tarafından gerçekleştirilmeye çalışılan işleme bağlıdır. Örneğin, yakın zamandaki bir satış hakkında bir ileti gönderen uygulama *Seller="Ava"* ve *Amount=10000* özelliklerini içerebilir. İleti gövdesi, satışın imzalı sözleşmesinin taranmış bir görüntüsünü içerebilir veya görüntü mevcut değilse bu kısım boş bırakılır.

Bir alıcı iki farklı şekilde Service Bus kuyruğundaki iletileri okuyabilir. *ReceiveAndDelete* olarak adlandırılan ilk seçenek, kuyruktan iletiyi kaldırır ve anında siler. Bu yöntem basittir ancak alıcı iletiyi işlemeyi tamamlamadan bir çökme gerçekleşirse ileti kaybolur. Kuyruktan kaldırılmış olduğundan başka bir alıcı bu iletiye erişemez. 

*PeekLock* olan ikinci seçenek ise bu soruna çözüm bulmak için tasarlanmıştır. ReceiveAndDelete gibi PeekLock okuma bir iletiyi kuyruktan kaldırır. Ancak iletiyi silmez. Bunun yerine, iletiyi kilitleyerek diğer alıcılar için görünmez yapar ve aşağıdaki olaylardan birinin gerçekleşmesini bekler:

* Alıcı iletiyi başarıyla işlediğinde *Complete* çağrısı yapar ve kuyruk iletiyi siler. 
* Alıcı, iletiyi başarıyla işleyemediğine karar verirse *Abandon* çağrısını yapar. Daha sonra kuyruk iletinin kilidini açar ve iletiyi diğer alıcılar için kullanılabilir hale getirir.
* Ayarlanabilir bir süre içinde alıcı bu yöntemlerin hiçbirini çağırmazsa (varsayılan 60 saniyedir) kuyruk alıcının başarısız olduğunu varsayar. Bu durumda, Abandon, iletinin diğer alıcılar için kullanılabilir hale getirme çağrısı yaptığını sanki gibi davranır.

Burada gerçekleşebilecek şu duruma dikkat edin: Aynı ileti iki kez hatta iki farklı alıcıya teslim edilebilir. Service Bus kuyruklarını kullanan uygulamalar bu duruma karşı hazırlıklı olmalıdır. Yinelenen algılama, her ileti birçok kez okunduğuna bakılmaksızın aynı kalır, varsayılan olarak benzersiz bir MessageID özelliğine sahip kolaylaştırmak için iletiyi bir kuyruktan okunur. 

Kuyruklar birçok durumda oldukça faydalıdır. Kuyruklar sayesinde aynı anda çalışmayan uygulamaların bile iletişim kurmasına olanak sağlanır. Özellikle toplu işlem ve mobil uygulamalarda olmak üzere bu özellik oldukça kullanışlıdır. Ayrıca, birden çok alıcısı bulunan bir kuyruk otomatik olarak yük dengelemesi sunar. Bu durum, gönderilen iletilerin tüm alıcılara dağıtılmasından kaynaklanır.

## <a name="topics"></a>Konu başlıkları
Ne kadar faydalı olsalar da kuyruklar her zaman doğru çözüm değildir. Bazı durumlarda Service Bus konu başlıkları daha faydalıdır. [Şekil 3](#Fig3) bu durum gösterilir.

<a name="Fig3"></a>![Service Bus konuları ve abonelikleri diyagramı][topics-subs]

**Şekil 3: Uygulama, abone uygulamanın belirlediği bir filtreye bağlı olarak Service Bus konu başlığına gönderilen iletilerin bazılarını veya tümünü alabilir.**

Konu başlığı, birçok açıdan kuyruğa benzer. Göndericiler, iletileri kuyruğa gönderdikleri gibi aynı şekilde konu başlığına gönderir ve bu iletiler kuyrukta göründükleri gibi görünür. Büyük farktır konuları her alıcı uygulama tanımlayarak kendi Abonelik Oluştur izin bir *filtre*. Böylece abone yalnızca filtreyle eşleşen iletileri görebilir. Örneğin, [Şekil 3](#Fig3) bir gönderici ve konuyla üç abonelerin her birinin kendi filtrelerine sahip gösterir:

* Abone 1 yalnızca *Seller="Ava"* özelliğini içeren iletileri alır.
* Abone 2 ise *Seller="Ruby"* ve/veya değeri 100.000'den fazla olan *Amount* özelliklerini içeren iletileri alır. Belki de Ruby satış müdürü olduğunu ve bu nedenle kendi satış ve tüm büyük satışları kimin yapar bağımsız olarak görmek istediği.
* Abone 3, filtresini *True* olarak ayarlar ve tüm iletileri alır. Örneğin, bu uygulama bir denetim kaydı tutmakla görevlendirilmiştir ve tüm iletileri görmesi gerekir.

Kuyruklarla olduğu gibi bir konu abonelere ReceiveAndDelete veya PeekLock kullanarak iletileri okuyabilir. Ancak kuyrukların aksine, konu başlığına gönderilen tek bir ileti birden çok abone tarafından alınabilir. Yaygın olarak adlandırılan bu yaklaşım *Yayımla ve abone ol*, birden çok uygulama aynı iletilerinde ilgileniyor durumunda faydalıdır. Her abone, doğru filtreyi tanımlayarak ileti akışının yalnızca görmesi gereken kısmını seçebilir.

## <a name="relays"></a>Geçişler
Hem kuyruklar hem de konu başlıkları, bir aracı yoluyla tek yönlü zaman uyumsuz iletişim sağlar. Trafik akışları sadece tek yöndedir ve göndericiler ile alıcılar arasında doğrudan bağlantı yoktur. Peki bunu istemezseniz çözüm nedir? Uygulamalarınızın iletileri hem göndermesi hem de alması gerektiğini ya da gönderici ile alıcılar arasında doğrudan bağlantı istediğinizi ve iletileri depolamak için aracıya ihtiyacınız olmadığını düşünelim. Bunun gibi adres senaryoları için Service Bus geçişleri, olarak [Şekil 4](#Fig4) gösterir.

<a name="Fig4"></a>![Service Bus geçişi diyagramı][relay]

**Şekil 4: Service Bus geçişi, uygulamalar arasında zaman uyumlu, çift yönlü iletişim sağlar.**

Geçiş kullanımı hakkında akla gelen ilk soru ise şudur: Neden geçiş kullanmalıyım? Kuyruklara ihtiyacım olmasa bile doğrudan etkileşim sağlamak yerine uygulamaların arasındaki iletişimi neden bir bulut hizmeti aracılığıyla sağlamalıyım? Bu soruya verilen cevap ise doğrudan iletişim kurmanın düşündüğünüzden daha zor olmasıdır.

Her ikisi de kurumsal veri merkezlerinde çalışan iki şirket içi uygulama arasında bağlantı kurmak istediğinizi düşünelim. Bu uygulamaların her biri güvenlik duvarının arkasında bulunur ve her veri merkezi de ağ adresi çevirisi (NAT) kullanır. Güvenlik duvarı, birkaçı dışındaki tüm bağlantı noktalarından gelen verileri engeller ve NAT ise uygulamaların çalıştığı makinelerin veri merkezi dışından doğrudan erişebileceğiniz sabit bir IP adresi olmadığını ifade eder. İlave yardım almadan genel İnternet üzerinden bu uygulamaları bağlamak sorun yaratır.

Service Bus geçişi, ihtiyacınız olan yardımı sunar. Bir geçiş aracılığıyla çift yönlü iletişim sağlamak için her uygulama Service Bus içeren bir giden TCP bağlantısı kurar ve bu bağlantıyı açık tutar. İki uygulama arasındaki tüm iletişim bu bağlantılar üzerinden kurulur. Tüm bağlantılar veri merkezi içinden kurulduğundan, güvenlik duvarı her uygulama için gelen trafiğe yeni bağlantı noktaları açmadan izin verir. Ayrıca, her uygulama iletişim boyunca bulutta sabit bir uç noktaya sahip olduğundan, bu çözüm NAT sorununu da ortadan kaldırır.  Uygulamalar, geçiş aracılığıyla veri değişimi yaparak gerçekleşmeleri durumunda iletişimi zorlaştıracak sorunlardan kaçınabilir. 

Service Bus geçişlerini kullanmak için uygulamaların Windows Communication Foundation (WCF) güvenir. Service Bus, Windows uygulamaları için geçişler aracılığıyla etkileşim sağlamayı doğrudan olacak şekilde ayarlayan WCF bağlamalarını sunar. WCF'yi zaten kullanmakta olan uygulamaların genel olarak bu bağlamaların bir tanesini belirtmesi yeterlidir, ardından bir geçiş aracılığıyla birbirleriyle iletişim kurarlar. Öte yandan kuyrukların ve konu başlıklarının aksine, olası durumlarda Windows uygulaması olmayan uygulamalarda geçişlerin kullanılması standart kitaplıklar sağlanmadığından programlama açısından biraz daha fazla çaba sarf edilmesini gerektirir.

Kuyruk ve konu başlıklarının aksine uygulamalar, geçişleri açık bir şekilde oluşturmaz. Bunun yerine, iletileri almak isteyen bir uygulama Service Bus içeren bir TCP bağlantısı kurduğunda otomatik olarak geçiş oluşturulur. Bağlantı kesildiğinde geçiş de silinir. Uygulamanın belirli bir dinleyici tarafından oluşturulan geçişi bulması izin vermek için Service Bus, uygulamaların ada göre belirli bir geçiş yerini olanak veren bir kayıt defteri sunar.

Uygulamalar arasında doğrudan iletişim kurulması gerekiyorsa geçişler doğru çözümdür. Örneğin; check-in kiosk cihazları, mobil cihazlar ve diğer bilgisayarlardan erişilebilmesi gereken ve bir şirket içi veri merkezinde çalışan hava yolu rezervasyon sistemini ele alalım. Bu sistemlerin tümünde çalışan uygulamalar, nerede çalışıyor olurlarsa olsunlar iletişim kurmak için buluttaki Service Bus geçişlerine güvenebilirler.

## <a name="event-hubs"></a>Event Hubs
Event Hubs, saniye başına milyonlarca olayı işleyen ileri düzeyde ölçeklenebilir bir alım sistemidir. Bu sistem, uygulamanızın bağlı cihazlarınız ve uygulamalarınız tarafından üretilen oldukça büyük miktardaki veriyi işlemesine ve analiz etmesine olanak sağlar. Örneğin, bir araba filosundan canlı motor performansı verilerini toplamak için Event Hub hizmetini kullanabilirsiniz. Veriler Event Hubs hizmetinde toplandığında, herhangi bir gerçek zamanlı analitik sağlayıcısı veya depolama kümesi kullanarak bu verileri dönüştürebilir veya depolayabilirsiniz. Event Hubs hakkında daha fazla bilgi için bkz: [Event Hubs'a genel bakış][Event Hubs overview].

## <a name="summary"></a>Özet
Uygulamalar arasında bağlantı kurma her zaman eksiksiz çözüm derlemelerinin bir parçası olmuştur. Bunun yanı sıra, uygulamaların ve hizmetlerin birbirleriyle iletişim kurmasını gerektiren senaryoların sayısı daha fazla uygulamanın ve cihazın İnternet'e bağlanmasıyla artmaktadır. Bu gibi durumlarda istenilen başarıya ulaşmak için kuyrukların, konu başlıklarının, geçişlerin ve Event Hubs hizmetinin kullanılmasına yönelik bulut tabanlı teknolojiler sağlayan Service Bus, bu temel işlevin uygulanmasını kolaylaştırmayı ve daha geniş kapsamda kullanılmasını amaçlar.

[svc-bus]: ./media/hybrid-solutions/SvcBus_01_architecture.png
[queues]: ./media/hybrid-solutions/SvcBus_02_queues.png
[topics-subs]: ./media/hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[relay]: ./media/hybrid-solutions/SvcBus_04_relay.png
[Event Hubs overview]: https://msdn.microsoft.com/library/azure/dn836025.aspx
