---
title: Azure Blockchain Workbench akıllı sözleşme tümleştirme desenleri
description: Azure Blockchain Workbench akıllı sözleşme tümleştirme desenleri genel bakış.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/1/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 0b0307d167485712e06966dd36fa94e24ef33aa1
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48243265"
---
# <a name="smart-contract-integration-patterns"></a>Akıllı sözleşme tümleştirme desenleri

Nitelikli akıllı anlaşmalar genellikle dış sistemleri ve cihazların ile tümleştirmek için gereken iş akışı temsil eder.

Bu iş akışları bir dış sistem, hizmet ya da cihaz verileri içeren dağıtılmış bir defter işlemler başlatmak için bir gereksinimi gereklidir. Bunlar ayrıca dış sistemler üzerinde dağıtılmış bir defter nitelikli akıllı anlaşmalar kaynaklanan olaylara tepki olması gerekir.

REST API ve mesajlaşma tümleştirme hem işlemleri bir Azure Blockchain Workbench uygulamasına dahil edilen nitelikli akıllı anlaşmalar dış sistemlerden göndermek, hem de dış sistemler alan değişikliklere dayalı olay bildirimleri göndermek için olanağı sunar. bir uygulama içinde yerleştirin.

Veri tümleştirme senaryoları için Azure Blockchain Workbench blok zinciri ve meta verileri, uygulamalar ve nitelikli akıllı anlaşmalar hakkında işlem verilerini bir birleşimini birleştirme veritabanı görünümü kümesi içerir.

Ayrıca, zincir veya medya kaynağı ile ilgili olanlar gibi bazı senaryolarda tümleştirme belgelerinin gerektirebilir. Azure Blockchain Workbench belgeleri doğrudan işlemek için API çağrıları belirtmese belgeleri bir Azure blok zinciri uygulamaya dahil edilebilir. Bu bölüm, ayrıca bu deseni içerir.

Bu bölüm bu tümleştirmeler türlerinin her biri, uçtan uca çözümleri uygulamak için tanımlanmış desenleri içerir.

## <a name="rest-api-based-integration"></a>REST API tabanlı bir tümleştirme

Azure Blockchain Workbench oluşturulan web uygulaması'larında sunulan özellikler, REST API aracılığıyla sunulur. Azure Blockchain Workbench karşıya yükleme, yapılandırma ve yönetim uygulamalarının dağıtılmış bir defter ve uygulama meta verileri ve kayıt defteri verilerini sorgulama işlemleri göndermek, özelliklerini içerir.

REST API, web, mobil, gibi etkileşimli istemciler ve bot uygulamalar için öncelikli olarak kullanılır.

Bu bölümde, dağıtılmış bir defter ve, Azure Blockchain Workbench'ın işlemlerden hakkında veri sorgulama işlemleri göndermek REST API odaklanan düzenlerini inceler *kapalı zinciri* SQL veritabanı.

### <a name="sending-transactions-to-a-distributed-ledger-from-an-external-system"></a>Bir dış sistemden gönderme işlemleri için Dağıtılmış bir defter

Azure Blockchain Workbench REST API işlemleri üzerinde dağıtılmış bir defter yürütmek için kimliği doğrulanmış istekleri gönderme olanağı sağlar.

![Gönderme işlemleri için Dağıtılmış bir defter](./media/integration-patterns/send-transactions-ledger.png)

Bu, yukarıda gösterilen işlemi burada kullanarak oluşur:

-   Dış uygulama, Azure Active Directory'ı Azure Blockchain Workbench dağıtımının bir parçası sağlanan kimliğini doğrular.
-   Yetkili kullanıcılar API'sine isteklerle gönderilebilecek bir taşıyıcı belirteç alır.
-   Dış uygulama, taşıyıcı belirteç kullanarak REST API çağrıları yapma.
-   REST API istek iletisi paketleri ve Service Bus'a gönderir. Buradan alınan, imzalı ve uygun olan dağıtılmış kayıt defteri gönderilir.
-   REST API, Azure Blockchain Workbench SQL istek kaydetmek ve geçerli sağlama durumu'kurmak için DB için bir istek gönderir.
-   SQL DB sağlama durumunu döndürür ve API çağrısı, onu çağıran dış uygulama Kimliğini döndürür.

### <a name="querying-blockchain-workbench-metadata-and-distributed-ledger-transactions"></a>Blockchain Workbench'i meta veri ve dağıtılmış kayıt defteri işlemleri sorgulama

Azure Blockchain Workbench REST API'sine kimliği doğrulanmış istekler için Dağıtılmış bir defter akıllı sözleşme yürütülmesine ilgili sorgu ayrıntıları gönderme olanağı sağlar.

![Meta verileri Sorgulama](./media/integration-patterns/querying-metadata.png)

Bu, yukarıda gösterilen işlemi burada kullanarak oluşur:

1. Dış uygulama, Azure Active Directory'ı Azure Blockchain Workbench dağıtımının bir parçası sağlanan kimliğini doğrular.
2. Yetkili kullanıcılar API'sine isteklerle gönderilebilecek bir taşıyıcı belirteç alır.
3. Dış uygulama, taşıyıcı belirteç kullanarak REST API çağrıları yapma.
4. REST API, SQL DB istekten verileri sorgular ve istemciye döndürür.

## <a name="messaging-integration"></a>Mesajlaşma tümleştirme

Tümleştirme Mesajlaşma sistemleri, hizmetler ve etkileşimli bir oturum açma veya istenmediğinde olduğu cihazlar ile etkileşimi kolaylaştırır. Bu işlem dağıtılmış bir defter ve gerçekleştirilen işlemler, bu kayıt defteri tarafından sunulan olayları yürütülüp istek iletileri iki tür Mesajlaşma tümleştirme odaklanır.

Mesajlaşma tümleştirme yürütülmesine odaklanır ve hareket izleme ve kullanıcı oluşturma, sözleşme oluşturma ve yürütme işlemlerinde sözleşmelerinde ilgili tarafından kullanılır *gözetimsiz* arka uç sistemler.

Bu bölümde işlem dağıtılmış bir defter ve temel alınan dağıtılmış kayıt defteri tarafından kullanıma sunulan olay iletileri temsil eden bu gönderen ileti tabanlı API odaklanan düzenlerini inceler.

### <a name="one-way-event-delivery-from-a-smart-contract-to-an-event-consumer"></a>Olay tüketicisi için akıllı bir sözleşme tek yönlü olay teslimi 

Bu senaryoda, bir akıllı sözleşmesi içinde Örneğin, bir durum değişikliği veya yürütme işleminin belirli bir türdeki bir olayı oluşur. Bu olay bir Event Grid aşağı akış tüketicilere ve bu tüketiciler yayın sonra uygun eylemleri gerçekleştirin.

Bu senaryoya örnek olarak, bir işlem ortaya çıktığında, bir tüketici uyarı almak ve SQL DB veya Common Data Service içinde bilgi kaydetme gibi eylemleri, gerçekleştirebilir, ' dir. Bu doldurmak için Workbench izleyen, aynı modelidir kendi *kapalı zinciri* SQL DB.

Başka bir akıllı bir sözleşme sözleşme girer, örneğin belirli bir duruma geçiş durumunda olacaktır bir *OutOfCompliance*. Bu durum değişikliği olduğunda, bir yöneticinin cep telefonunuza gönderilecek bir uyarı tetikleyebilir.

![Tek yönlü olay teslimi](./media/integration-patterns/one-way-event-delivery.png)

Bu, yukarıda gösterilen işlemi burada kullanarak oluşur:

-   Akıllı sözleşme, yeni bir duruma geçer ve kayıt defteri için bir olay gönderir.
-   Genel muhasebe alır ve Azure Blockchain Workbench'i olay gönderir.
-   Azure Blockchain Workbench defterine olaylara abone olur ve olayı alır.
-   Azure Blockchain Workbench abonelerinin kullanımına Event Grid olayı yayımlar.
-   Dış sistemler için Event Grid abonesiniz, iletiyi kullanmak ve uygun eylemleri gerçekleştirin.

## <a name="one-way-event-delivery-of-a-message-from-an-external-system-to-a-smart-contract"></a>Tek yönlü olay teslimi iletinin bir dış sistemden bir akıllı Sözleşmesi

Ters yönlerde akan bir senaryo yoktur. Bu durumda, bir olay bir algılayıcı tarafından oluşturulan veya bir dış sistem ve olay verilerini akıllı bir sözleşmeyi gönderilmelidir.

Yaygın olarak karşılaşılan örneklerden veri gelen Finans piyasalarına, örneğin, bono, ticari mallar veya hisse senedi fiyatlarını akıllı bir sözleşme teslimatıdır.

### <a name="direct-delivery-of-an-azure-blockchain-workbench-in-the-expected-format"></a>Bir Azure Blockchain Workbench beklenen biçimde doğrudan teslim

Bazı uygulamalar Azure Blockchain Workbench ile tümleştirmek için tasarlanmıştır ve doğrudan oluşturur ve beklenen biçimlerde ileti gönderme.

![Doğrudan teslimat](./media/integration-patterns/direct-delivery.png)

Bu, yukarıda gösterilen işlemi burada kullanarak oluşur:

-   Dış sistemdeki bir ileti oluşturmak için Azure Blockchain Workbench tetikleyen bir olay gerçekleşir.
-   Dış sistemdeki bilinen bir biçimde bu iletiyi oluşturmak için yazılan kod vardır ve bu doğrudan Service Bus'a gönderir.
-   Azure Blockchain Workbench, Service Bus olaylara abone olur ve iletiyi alır.
-   Azure Blockchain Workbench, belirli bir sözleşmeyi dış sistemden veri gönderen bir muhasebe, çağrı başlatır.
-   Bir ileti alındığında, sözleşmeyi yeni bir duruma geçer.

### <a name="delivery-of-a-message-in-a-format-unknown-to-azure-blockchain-workbench"></a>Azure Blockchain Workbench bilinmeyen bir biçimde bir ileti teslimi

Bazı sistemler, Azure Blockchain Workbench tarafından kullanılan standart bir biçimde iletileri ulaştırmak üzere değiştirilemez. Bu durumlarda, mekanizmalar ve ileti biçimlerinden bu sistemler genellikle kullanılabilir. Özellikle, beklenen biçimler Mesajlaşma standart birine eşlemek için Logic Apps, Azure işlevleri veya diğer özel kod kullanarak bu sistemlerin yerel ileti türleri dönüştürülebilir.

![Bilinmeyen ileti biçimi](./media/integration-patterns/unknown-message-format.png)

Bu, yukarıda gösterilen işlemi burada kullanarak oluşur:

-   Bir olay, bir ileti oluşturulmasını tetikler bir dış sistemde gerçekleşir.
-   Bir mantıksal uygulama veya özel kod ileti alma ve standart Azure Blockchain Workbench biçimlendirilmiş bir ileti dönüştürmek için kullanılır.
-   Mantıksal uygulama dönüştürülmüş iletiyi doğrudan Service Bus'a gönderir.
-   Azure Blockchain Workbench, Service Bus olaylara abone olur ve iletiyi alır.
-   Azure Blockchain Workbench, Anlaşmadaki belirli bir işlevi dış sistemden veri gönderme genel bir çağrı başlatır.
-   İşlevi yürütür ve tipik olarak durumu değiştirir. Durum değişikliği diğer işlevleri artık uygun olarak yürütülmesini etkinleştirmek akıllı sözleşmesindeki yansıtılan iş akışını ileriye taşır.

### <a name="transitioning-control-to-an-external-process-and-await-completion"></a>Bir dış denetimine geçiş işlemek ve tamamlanmasını bekler

İç yürütme ve bir dış işlem kapalı el akıllı bir sözleşme burada durdurmalısınız senaryo vardır. Dış işleme sonra tamamlamanız, akıllı sözleşme ve yürütme için bir ileti gönderme ardından içinde akıllı sözleşme devam eder.

#### <a name="transition-to-the-external-process"></a>Dış işleme geçiş

Bu düzen, genellikle aşağıdaki yaklaşımı kullanılarak uygulanır:

-   Belirli bir durumu akıllı sözleşme geçer. Bu durumda, ya da yok veya bir dış sistem istenen bir eylemde kadar sınırlı bir dizi işlev yürütülebilir.
-   Durum değişikliği bir aşağı akış tüketici için bir olay olarak kullanıma sunulur.
-   Aşağı Akış tüketici olayı alır ve dış kod yürütme tetikler.

![Dış işleme geçiş denetimi](./media/integration-patterns/transition-external-process.png)

#### <a name="return-of-control-from-the-smart-contract"></a>Akıllı sözleşmeden denetimin dönüşü

Dış Sistem özelleştirme yeteneği bağlı olarak olabilir veya Azure Blockchain Workbench bekliyor standart biçimlerinden birinde iletileri ulaştırmak üzere mümkün olmayabilir. Dış sistemler yeteneklerine bağlı olarak bu iletilerden biri oluşturmak için aşağıdaki iki dönüş yolları gerçekleştirilecek belirler.

##### <a name="direct-delivery-of-an-azure-blockchain-workbench-in-the-expected-format"></a>Bir Azure Blockchain Workbench beklenen biçimde doğrudan teslim

![](./media/integration-patterns/direct-delivery.png)

Bu modelde, sözleşmenin ve sonraki durumu değişikliği iletişim yukarıdaki gerçekleşir where - işlem

-   Tamamlama veya harici kod yürütme belirli bir kilometre taşı ulaştığınızda, Azure Blockchain Workbench'i bağlı Service Bus olay gönderilir.

-   Bu API için beklentileri uyan ileti yazmak için doğrudan uyarlanabilen olamaz sistemler için dönüştürülür.

-   İleti içeriğini paketlenir ve akıllı Anlaşmadaki belirli bir işleve gönderilen. Bu, dış sistemle ilişkili kullanıcı adına gerçekleştirilir.

-   İşlevi yürütür ve tipik olarak durumu değiştirir. Durum değişikliği diğer işlevleri artık uygun olarak yürütülmesini etkinleştirmek akıllı sözleşmesindeki yansıtılan iş akışını ileriye taşır.

### 

### <a name="delivery-of-a-message-in-a-format-unknown-to-azure-blockchain-workbench"></a>Azure Blockchain Workbench bilinmeyen bir biçimde bir ileti teslimi

![Bilinmeyen ileti biçimi](./media/integration-patterns/unknown-message-format.png)

Bu modelde burada standart bir biçimde bir ileti sözleşmesi iletişim doğrudan gönderilemez ve sonraki durumu değişikliği oluştuktan yukarıdaki işlem yeri:

1.  Tamamlama veya harici kod yürütme belirli bir kilometre taşı ulaştığınızda, Azure Blockchain Workbench'i bağlı Service Bus olay gönderilir.
2.  Bir mantıksal uygulama veya özel kod ileti alma ve standart Azure Blockchain Workbench biçimlendirilmiş bir ileti dönüştürmek için kullanılır.
3.  Mantıksal uygulama dönüştürülmüş iletiyi doğrudan Service Bus'a gönderir.
4.  Azure Blockchain Workbench, Service Bus olaylara abone olur ve iletiyi alır.
5.  Azure Blockchain Workbench, belirli bir sözleşmeyi dış sistemden veri gönderen bir muhasebe, çağrı başlatır.
6. İleti içeriğini paketlenir ve akıllı Anlaşmadaki belirli bir işleve gönderilen. Bu, dış sistemle ilişkili kullanıcı adına gerçekleştirilir.
7.  İşlevi yürütür ve tipik olarak durumu değiştirir. Durum değişikliği diğer işlevleri artık uygun olarak yürütülmesini etkinleştirmek akıllı sözleşmesindeki yansıtılan iş akışını ileriye taşır.

## <a name="iot-integration"></a>IOT tümleştirme

Genel bir tümleştirme senaryosu akıllı bir sözleşme sensörlerden alınan telemetri verilerini eklenmesidir. Sensörleri tarafından sunulan veri bağlı olarak, nitelikli akıllı anlaşmalar bilgiye dayalı eylemleri ve sözleşmenin durumunu değiştirme.

Örneğin, TIP sunan bir kamyon 110 derece oranında artırdı, sıcaklık olsaydı, ilaç verimliliğini etkileyebilir kamu güvenliği soruna neden olabilir değilse algılandı ve tedarik zinciri kaldırıldı. Bir sürücü için saat başına 100 mil kendi araba hızlandırılmış, Algılayıcı bilgilerine bir sigorta, sigorta sağlayıcı tarafından iptal tetikleyebilir. Araba kiralama araba, GPS veri kendi kiralama anlaşması kapsamında bir Coğrafya dışında sürücü gittiğini belirten ve bir ceza ücret alınır.

Bu sensörlerden veri sürekli olarak teslim ve tüm bu verileri akıllı bir sözleşmeyi göndermek uygun değil zorluktur. İkincil bir depoya tüm iletileri sunarken blok zincirine gönderilen ileti sayısını sınırla tipik bir yaklaşımdır. Örneğin, yalnızca sabit aralık, örneğin, saatte bir ve içerdiği değer anlaşılmış dışında düştüğünde alınan iletilerin teslim akıllı bir sözleşme için aralığı temel alır. Toleranslar dışında kalan değerleri denetlemek, veri sözleşme iş mantığına uygun yürütülen ve alındığını sağlar. Aralık değeri denetleme algılayıcı hala raporlama onaylar. Tüm veriler gönderildiğinde bir ikincil raporlama depoya daha geniş raporlama, analiz ve makine öğrenimi etkinleştirmek için. Örneğin, GPS için sensör okumaları alma dakikada akıllı bir sözleşme için gerekli olmayabilir ancak raporları veya eşleme rotaları kullanılacak için ilginç verileri sağlayabilir.

Azure platformunda, cihazlar ile tümleştirme, genellikle IOT Hub ile gerçekleştirilir. IOT Hub, iletilerin içeriğine göre olanağı sağlar ve tür yukarıda açıklanan işlevleri sağlar.

![IOT iletileri](./media/integration-patterns/iot.png)

Bu uygulanır için yukarıda işlemi bir deseni gösterir:

-   Bir cihaz doğrudan veya bir alan ağ geçidi için IOT Hub ile iletişim kurar.
-   IOT Hub iletilerini alır ve iletileri yollar kurulan karşı değerlendirir örneğin iletisinin içeriği denetleyin. *50 derece büyük bir sıcaklık algılayıcısı bildirimde bulunur?*
-   IOT hub'ı ölçütlerine yol için tanımlı bir Service Bus iletileri gönderir.
-   Mantıksal uygulama veya diğer kod rota için IOT Hub'ın oluşturduğu Service Bus'a dinler.
-   Mantıksal uygulama veya diğer kod alır ve ileti bilinen biçimine dönüştürün.
-   Dönüştürülmüş iletiyi artık standart bir biçimde, Service Bus-Azure Blockchain Workbench için gönderilir.
-   Azure Blockchain Workbench, Service Bus olaylara abone olur ve iletiyi alır.
-   Azure Blockchain Workbench, belirli bir sözleşmeyi dış sistemden veri gönderen bir muhasebe, çağrı başlatır.
-   İleti alındığında sözleşmesi veri değerlendirir ve için yüksek bir sıcaklık, değerlendirme, örneğin, sonuca dayanarak durum değişikliği, duruma getirmeniz *dışı Uyumluluk*.

## <a name="data-integration"></a>Veri tümleştirmesi

REST ve ileti tabanlı API ek olarak, Azure Blockchain Workbench Ayrıca, dağıtılan defterler işlem verilerini yanı sıra, uygulama ve sözleşme meta veriler ile doldurulmuş bir SQL veritabanına erişim sağlar.

![Veri tümleştirmesi](./media/integration-patterns/data-integration.png)

Veri tümleştirmesi, iyi bilinen:

-   Azure Blockchain Workbench normal çalışma davranışını bir parçası olarak uygulamaları, iş akışları, sözleşmeler ve işlemler hakkındaki meta verileri depolar.
-   Veritabanı sunucusu adı, veritabanı adı, türü kimlik doğrulaması, oturum açma kimlik bilgileri ve kullanmak için hangi veritabanı görünümleri gibi veritabanı hakkındaki bilgileri koleksiyonunu kolaylaştırmak için bir veya daha fazla iletişim kutuları, dış sistemler veya araçlar sağlar.
-   Sorgular, aşağı akış tüketim kolaylaştırmak için SQL veritabanı görünümlerini karşı dış sistemler, hizmetleri, raporlama, geliştirici araçları ve kurumsal üretkenlik araçları tarafından yazılır.

## <a name="storage-integration"></a>Depolama tümleştirme

Birçok senaryo attestable dosyaları eklemenize gerek gerektirebilir. Birden çok nedeniyle, bir blok zinciri dosyaları yerleştirmek uygun olacaktır. Bunun yerine, bir dosyaya karşı bir şifreleme karması ile (örneğin, SHA-256) gerçekleştirmek ve dağıtılmış bir defter o karma paylaşmak için yaygın bir yaklaşım olan. Gelecekteki istediğiniz zaman yeniden karma gerçekleştirme aynı sonucu döndürmelidir. Dosya değiştirilirse, yalnızca bir piksel görüntüdeki değiştirilirse bile, karma farklı bir değer döndürür.

![Depolama tümleştirme](./media/integration-patterns/storage-integration.png)

Desen uygulanabileceği yeri:

-   Dış sistemdeki bir Azure depolama gibi bir depolama mekanizması dosyasında devam ettirir.
-   Karma dosyası veya dosya ile oluşturulur ve ilişkili meta verileri vb. gibi sahibi, dosyanın bulunduğu, URL için bir tanımlayıcı.
-   Karma ve herhangi bir meta veri gönderildiği bir akıllı sözleşmesini bir işlev gibi *FileAdded*
-   Gelecekte, dosya meta verileri yeniden karma hale ve muhasebe üzerinde depolanan değerleri karşı karşılaştırılır.

## <a name="prerequisites-for-implementing-integration-patterns-using-the-rest-and-message-apis"></a>REST ve ileti API'leri kullanarak tümleştirme desenleri uygulamak için Önkoşullar

Bir dış sistem veya cihaz için REST veya ileti API kullanarak akıllı sözleşme ile etkileşim olanağı kolaylaştırmak için aşağıdakiler olmalıdır-

1. Consortium Azure Active Directory'de Dış Sistem ya da cihaz temsil eden bir hesabı oluşturulur.
2. Azure Blockchain Workbench uygulamanız için uygun akıllı sözleşmelerinin Dış Sistem veya cihazınızdan olayları kabul etmek için tanımlanan işlevleri vardır.
3. Akıllı sözleşmeniz için uygulama yapılandırma dosyasını içeren sistemi veya cihaz rolü atanır.
4. Akıllı sözleşmeniz için uygulama yapılandırma dosyası içinde tanımlanan rol tarafından bu işlev çağrılabilir bildiren tanımlar.
5. Azure Blockchain Workbench için uygulama yapılandırma dosyası ve nitelikli akıllı anlaşmalar yüklenir.

Uygulama yüklendikten sonra Azure Active Directory hesabı dış sistem için sözleşme ve ilişkili rol atanır.

## <a name="testing-external-system-integration-flows-prior-to-writing-integration-code"></a>Dış Sistem tümleştirme akışlarını tümleştirme kod yazmadan önce test etme 

Dış sistemlerle tümleştirme olanağı sağlayarak, birçok senaryo önemli bir gereksinimdir. Dış sistemlerle tümleştirmek için akıllı sözleşme tasarım önceki veya paralel kod geliştirme doğrulayabilmesi için istenen bir durumdur.

Azure Active Directory (Azure AD) kullanımını önemli ölçüde Geliştirici üretkenliği ve saat değeri hızlandırabilirsiniz. Özellikle, bir dış sistem kod tümleştirmesiyle Önemsiz olmayan bir süre beklemeniz gerekebilir. Azure AD kullanarak ve UX Azure Blockchain Workbench tarafından otomatik olarak oluşturulmasını, Workbench bu dış sistem olarak oturum açın ve UX'i üzerinden bu dış sistemden beklenen değerleri doldurmak, geliştiricilerin bu izin verebilirsiniz Bu hızlı bir şekilde geliştirmek ve içeren bir kavram kanıtı ya da öncesinde için ortamında veya dış sistemler için yazılan tümleştirme kodunda paralel doğrulama sağlar.
