---
title: Azure Blockchain çalışma ekranı akıllı sözleşme tümleştirme düzenleri
description: Akıllı sözleşme tümleştirme desenleri Azure Blockchain çalışma ekranındaki genel bakış.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/1/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: a6a44e30fe58617b43c5491a72fc882015bc9591
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="smart-contract-integration-patterns"></a>Akıllı sözleşme tümleştirme desenleri

Akıllı sözleşmeleri genellikle dış sistemleri ve cihazların ile tümleştirmek için gereken bir iş akışı temsil eder.

Verileri bir dış sistem, hizmeti veya aygıt içermeyen bir dağıtılmış defter hareketleri başlatmak için gerekirse bu iş akışları gereksinimlerini içerir. Bunlar ayrıca dağıtılmış bir muhasebe akıllı sözleşmelerinde kaynaklanan olayları tepki dış sistemler sahip olmanız gerekir.

REST API ve mesajlaşma tümleştirme hem işlemleri bir Azure Blockchain çalışma ekranı uygulamasına dahil edilen akıllı sözleşmeleri dış sistemlerden göndermek, hem de alan değişikliklere dayalı dış sistemler olay bildirimleri göndermek için olanağı sunar. bir uygulama içinde yerleştirin.

Veri tümleştirme senaryoları için Azure Blockchain çalışma ekranı blockchain ve uygulamalar ve akıllı sözleşmeleri hakkındaki meta verileri işlemsel verileri birlikte birleştirme veritabanı görünümleri içerir.

Ayrıca, zinciri veya ortam sağlamak için ilgili olanlar gibi bazı senaryolar belgeleri tümleştirme gerektirebilir. Azure Blockchain çalışma ekranı belgeleri doğrudan işlemek için API çağrılarının belirtmese belgeleri bir Azure Blockchain uygulamaya birleştirilebilir. Bu bölümde Ayrıca bu desene içerir.

Bu bölümde tümleştirmeler bu türlerinin her biri, uçtan uca çözümler gerçekleştirmek için tanımlanan düzenleri içerir.

## <a name="rest-api-based-integration"></a>REST API tabanlı tümleştirme

Azure Blockchain oluşturulan çalışma ekranı web uygulaması içinde özellikleri REST API sunulur. Bu, Azure Blockchain çalışma ekranı karşıya yükleme, yapılandırma ve yönetim işlemleri dağıtılmış bir muhasebe ve uygulama meta verileri ve muhasebe verileri sorgulamak için gönderme uygulamalarının içerir.

REST API öncelikle web, mobil, gibi etkileşimli istemcileri ve bot uygulamalar için kullanılır.

Bu bölümde Dağıtılmış bir muhasebe ve bu verileri Azure Blockchain çalışma ekranı'nın hareketleri hakkında sorgu işlemleri göndermek REST API yönlerini odaklanan desenleri bakar *kapalı zinciri* SQL veritabanı.

### <a name="sending-transactions-to-a-distributed-ledger-from-an-external-system"></a>Dış sistemden dağıtılmış defterine gönderme işlemleri

Azure Blockchain çalışma ekranı REST API üzerinde dağıtılmış bir muhasebe işlemleri yürütmek için kimliği doğrulanmış istekler gönderme olanağı sağlar.

![Dağıtılmış bir muhasebe işlemleri gönderme](media/blockchain-workbench-integration-patterns/send-transactions-ledger.png)

Bu, yukarıda gösterilen işlemi burada kullanarak oluşur:

-   Dış uygulama Azure Active Directory'ye Azure Blockchain çalışma ekranı dağıtımının bir parçası sağlanan kimliğini doğrular.
-   Yetkili kullanıcılar istekleriyle API'sine gönderilebilecek bir taşıyıcı belirteci alır.
-   Dış uygulamalar, taşıyıcı belirteci kullanarak REST API çağrıları yapma.
-   REST API isteği bir ileti olarak paketleri ve Service Bus hizmetine gönderir. Buradan alınan, imzalı ve uygun dağıtılmış muhasebe gönderilir.
-   REST API Azure Blockchain çalışma ekranı SQL isteği kaydetmek ve geçerli sağlama durumu kurmak için DB'ye isteğinde bulunur.
-   SQL DB sağlama durumu döndürür ve API çağrısı adlı dış uygulama Kimliğini döndürür.

### <a name="querying-blockchain-workbench-metadata-and-distributed-ledger-transactions"></a>Blockchain çalışma ekranı meta verileri ve dağıtılmış hareketlerinin sorgulama

Azure Blockchain çalışma ekranı REST API dağıtılmış bir muhasebe akıllı sözleşme yürütülmesine ilgili sorgu ayrıntıları için kimliği doğrulanmış istekler gönderme olanağı sağlar.

![Meta veri sorgulama](media/blockchain-workbench-integration-patterns/querying-metadata.png)

Bu, yukarıda gösterilen işlemi burada kullanarak oluşur:

1. Dış uygulama Azure Active Directory'ye Azure Blockchain çalışma ekranı dağıtımının bir parçası sağlanan kimliğini doğrular.
2. Yetkili kullanıcılar istekleriyle API'sine gönderilebilecek bir taşıyıcı belirteci alır.
3. Dış uygulamalar, taşıyıcı belirteci kullanarak REST API çağrıları yapma.
4. REST API SQL DB gelen istek için verileri sorgular ve istemciye döndürür.

## <a name="messaging-integration"></a>İleti tümleştirme

Tümleştirme Mesajlaşma sistemleri, hizmetleri ve etkileşimli oturum açma olası veya istenmediğinde olduğu cihazlar ile etkileşimi kolaylaştırır. İki tür da işlemleri dağıtılmış bir muhasebe ve işlemler yapılmadığı, muhasebe tarafından sunulan olayları yürütülmesi istek iletileri ileti tümleştirme odaklanır.

İleti tümleştirme yürütülmesine odaklanır ve işlem izleme ve kullanıcı oluşturma, sözleşme oluşturma ve yürütme sözleşmelerinde hareketlerinin ilgili tarafından kullanılır *gözetimsiz* arka uç sistemleri.

Bu bölümde Dağıtılmış bir muhasebe ve temel dağıtılmış defter tarafından kullanıma sunulan olay iletileri temsil eden bu işlemleri Gönder ileti tabanlı API yönlerini odaklanan desenleri bakar.

### <a name="one-way-event-delivery-from-a-smart-contract-to-an-event-consumer"></a>Olay tüketicisi akıllı sözleşme tek yönlü olay teslimi 

Bu senaryoda, bir akıllı sözleşme içinde Örneğin, bir durum değişikliği veya yürütme işleminin belirli bir türde bir olayı oluşur. Bu olay bir aşağı akış tüketicileri olay kılavuzuna ve bu tüketicileri aracılığıyla yayın sonra uygun eylemleri gerçekleştirin.

Bu senaryo bir işlem oluştuğunda bir tüketici uyarı almak ve bir SQL DB veya ortak veri hizmeti bilgi kaydetme gibi eylemleri gerçekleştirebilir olduğunu örneğidir. Çalışma ekranı doldurmak için aşağıdaki aynı düzeni budur kendi *kapalı zinciri* SQL DB.

Başka bir akıllı Sözleşme Sözleşme uygulamasına gittiğinde örneğin belirli bir duruma geçiş durumunda olacak bir *OutOfCompliance*. Bu durum değişikliği olduğunda, bir yöneticinin cep telefonuna gönderilecek bir uyarı tetikleyebilir.

![Tek yönlü olay teslimi](media/blockchain-workbench-integration-patterns/one-way-event-delivery.png)

Bu, yukarıda gösterilen işlemi burada kullanarak oluşur:

-   Akıllı sözleşme yeni bir duruma geçer ve muhasebe bir olay gönderir.
-   Muhasebe alır ve Azure Blockchain çalışma ekranına olay sunar.
-   Azure Blockchain çalışma ekranı defterine olaylarına abone olur ve olay alır.
-   Azure Blockchain çalışma ekranı olayın olay kılavuza abonelere yayımlar.
-   Dış sistemler olay kılavuza abone olduğunuz, ileti kullanabilir ve uygun eylemleri gerçekleştirin.

## <a name="one-way-event-delivery-of-a-message-from-an-external-system-to-a-smart-contract"></a>Tek yönlü olay ileti teslimini dış sistemden bir akıllı Sözleşmesi

Ayrıca, karşıt yönünden akan senaryo vardır. Bu durumda, bir olay algılayıcı tarafından oluşturulan veya bir dış sistem ve olay verilerini bir akıllı sözleşmesine gönderilmelidir.

Teslimat veri Finans Piyasası, örneğin, ticari mallar, hisse senedi veya bonolar fiyatlar akıllı sözleşme buna ortak bir örnektir.

### <a name="direct-delivery-of-an-azure-blockchain-workbench-in-the-expected-format"></a>Beklenen biçimde bir Azure Blockchain ekranının doğrudan teslimat

Bazı uygulamalar Azure Blockchain çalışma ekranı ile tümleştirmek için tasarlanmıştır ve doğrudan oluşturur ve beklenen biçimlerde iletileri göndermek.

![Doğrudan teslimat](media/blockchain-workbench-integration-patterns/direct-delivery.png)

Bu, yukarıda gösterilen işlemi burada kullanarak oluşur:

-   Bir ileti oluşturma Azure Blockchain çalışma ekranı için tetikleyen dış sistemdeki bir olayı oluşur.
-   Dış Sistem bilinen bir biçimde bu iletiyi oluşturmak için yazılan kod vardır ve bu doğrudan Service Bus hizmetine gönderir.
-   Azure Blockchain çalışma ekranı olaylara hizmet yolundan abone olur ve iletiyi alır.
-   Azure Blockchain çalışma ekranı, belirli bir sözleşmeyle dış sistemden veri gönderme defter bir çağrı başlatır.
-   İleti alındığında, sözleşmeyi yeni bir duruma geçer.

### <a name="delivery-of-a-message-in-a-format-unknown-to-azure-blockchain-workbench"></a>Azure Blockchain çalışma ekranı bilinmeyen biçimde bir ileti teslimi

Bazı sistemler Azure Blockchain çalışma ekranı tarafından kullanılan standart biçimlerde iletileri sunmak için değiştirilemez. Bu durumlarda, varolan mekanizmaları ve iletinizi bu sistemler biçimlerinden genellikle kullanılabilir. Özellikle, bu sistemler yerel ileti türlerini beklenen biçimler Mesajlaşma standart birine eşlemek için Logic Apps, Azure işlevleri veya diğer özel kod kullanarak dönüştürülebilir.

![Bilinmeyen ileti biçimi](media/blockchain-workbench-integration-patterns/unknown-message-format.png)

Bu, yukarıda gösterilen işlemi burada kullanarak oluşur:

-   Bir olay bir ileti oluşturulmasını tetikleyen bir dış sistemde oluşur.
-   Bir mantıksal uygulama veya özel kod ileti alma ve standart bir Azure Blockchain çalışma ekranı biçimlendirilmiş iletinin dönüştürmek için kullanılır.
-   Mantıksal uygulama doğrudan Service Bus dönüştürülmüş iletisi gönderir.
-   Azure Blockchain çalışma ekranı olaylara hizmet yolundan abone olur ve iletiyi alır.
-   Azure Blockchain çalışma ekranı sözleşmesindeki belirli bir işlev için dış sistemden veri gönderme defter bir çağrı başlatır.
-   İşlev yürütür ve tipik olarak durumu değiştirir. Durum değişikliği İleri şimdi uygun şekilde yürütülecek diğer işlevlerini etkinleştirmek akıllı sözleşmesindeki yansıtılan iş akışı taşır.

### <a name="transitioning-control-to-an-external-process-and-await-completion"></a>Bir dış denetimine geçiş işlemek ve tamamlanmasını bekleme

Akıllı bir sözleşme iç yürütme ve bir dış işlem dışı ele burada durdurmalısınız senaryo vardır. Dış işlem sonra tamamlandığını, akıllı sözleşme ve yürütme ileti gönderme sonra Akıllı sözleşme içinde devam eder.

#### <a name="transition-to-the-external-process"></a>Dış işlem geçiş

Bu deseni genelde aşağıdaki yaklaşımı kullanılarak uygulanır:

-   Belirli bir durumu akıllı sözleşme geçişleri. Bu durumda, her iki yok veya bir dış sistem istenen bir eylemde kadar işlevleri sınırlı sayıda çalıştırılabilir.
-   Durum değişikliği olayı olarak bir aşağı akış tüketiciye ortaya.
-   Aşağı Akış tüketici olay alır ve dış kod yürütmeyi tetikler.

![Dış işlem için geçiş denetimi](media/blockchain-workbench-integration-patterns/transition-external-process.png)

#### <a name="return-of-control-from-the-smart-contract"></a>Denetim Return akıllı sözleşmeden

Dış Sistem özelleştirme yeteneği bağlı olarak bu olabilir veya Azure Blockchain çalışma ekranı bekliyor standart biçimlerinden birinde iletileri sunmak mümkün olmayabilir. Dış sistemler yeteneklerine bağlı olarak bu iletilerinden birini oluşturmak için aşağıdaki iki dönüş yolları gerçekleştirilecek belirler.

##### <a name="direct-delivery-of-an-azure-blockchain-workbench-in-the-expected-format"></a>Beklenen biçimde bir Azure Blockchain ekranının doğrudan teslimat

![](media/blockchain-workbench-integration-patterns/direct-delivery.png)

Bu modelde, sözleşmenin ve sonraki durumu değişikliği için yukarıdaki iletişimin işlem where -

-   Tamamlama veya belirli bir aşama harici kod yürütmesinde ulaşmasını bağlı bir olay Azure Blockchain çalışma ekranına bağlı Service Bus gönderilir.

-   API beklentilerini için uygun bir ileti yazmak için doğrudan uyarlanan olamaz sistemler için onu dönüştürülür.

-   İletinin içeriğini paketlenir ve belirli bir işlev akıllı sözleşmesindeki gönderilir. Bu dış sistemle ilişkili kullanıcı adına gerçekleştirilir.

-   İşlev yürütür ve tipik olarak durumu değiştirir. Durum değişikliği İleri şimdi uygun şekilde yürütülecek diğer işlevlerini etkinleştirmek akıllı sözleşmesindeki yansıtılan iş akışı taşır.

### 

### <a name="delivery-of-a-message-in-a-format-unknown-to-azure-blockchain-workbench"></a>Azure Blockchain çalışma ekranı bilinmeyen biçimde bir ileti teslimi

![Bilinmeyen ileti biçimi](media/blockchain-workbench-integration-patterns/unknown-message-format.png)

Burada bir ileti standart bir biçimde doğrudan iletişim sözleşmeye gönderilemiyor ve sonraki durumu değişikliği oluşur yukarıdaki Bu modelde işlem yeri:

1.  Tamamlama veya belirli bir aşama harici kod yürütmesinde ulaşmasını bağlı bir olay Azure Blockchain çalışma ekranına bağlı Service Bus gönderilir.
2.  Bir mantıksal uygulama veya özel kod ileti alma ve standart bir Azure Blockchain çalışma ekranı biçimlendirilmiş iletinin dönüştürmek için kullanılır.
3.  Mantıksal uygulama doğrudan Service Bus dönüştürülmüş iletisi gönderir.
4.  Azure Blockchain çalışma ekranı olaylara hizmet yolundan abone olur ve iletiyi alır.
5.  Azure Blockchain çalışma ekranı, belirli bir sözleşmeyle dış sistemden veri gönderme defter bir çağrı başlatır.
6. İletinin içeriğini paketlenir ve belirli bir işlev akıllı sözleşmesindeki gönderilir. Bu dış sistemle ilişkili kullanıcı adına gerçekleştirilir.
7.  İşlev yürütür ve tipik olarak durumu değiştirir. Durum değişikliği İleri şimdi uygun şekilde yürütülecek diğer işlevlerini etkinleştirmek akıllı sözleşmesindeki yansıtılan iş akışı taşır.

## <a name="iot-integration"></a>IOT tümleştirme

Ortak bir tümleştirme senaryosunun akıllı sözleşmesindeki algılayıcılar kaynağından alınan telemetri verilerini eklenmesidir. Algılayıcılar tarafından sunulan veri bağlı olarak, akıllı sözleşmeler bilinçli eylemleri ve sözleşme durumunu değiştirme.

Örneğin, TIP teslim kamyon 110 derece soar kendi sıcaklık olsaydı, TIP verimliliğini etkileyebilecek genel güvenlik sorunu neden olabilir değilse algılandı ve tedarik zinciri kaldırıldı. Bir sürücü için saat başına 100 mil bu otomobil hızlandırılmış, Algılayıcı bilgilerine sigorta kendi sigorta sağlayıcı tarafından iptal edilmesine tetikleyebilir. Araba kiralık araba olduysa, GPS veri sürücüsü tarafından kendi kiralama kapsanmaktadır Coğrafya dışında olduğunuzda belirtmek ve bir cezası gider.

, Bu verileri bir sabit temelinde teslim ve tüm bu verileri bir akıllı sözleşmesine göndermek uygun değil iştir. İkincil bir depoya tüm iletileri sunarken blockchain gönderilen ileti sayısı sınırı tipik bir yaklaşımdır. Örneğin, yalnızca sabit aralıklarla, örneğin, saatte bir ve içerdiği bir değer anlaşılmış dışında düştüğünde alınan iletilerin teslim akıllı sözleşme aralığının bağlıdır. Toleranslar dışında kalan değerler denetimi, veri sözleşmeleri iş mantığına ilgili yürütülen ve alındığını sağlar. Aralık değeri denetleme algılayıcı hala raporlama onaylar. Tüm veriler gönderildiğinde İkincil raporlama deposuna daha geniş raporlama, analizi ve makine öğrenme etkinleştirmek için. Örneğin, GPS sensör okumaları alma dakikada bir akıllı sözleşmesi için gerekli olmayan olsa da, raporları veya eşleme rotaları kullanılacak ilginç veriler sağlayabilir.

Azure platformunda, aygıtlar ile tümleştirme genellikle IOT Hub ile yapılır. IOT Hub özelliği içeriğine göre iletileri yönlendirmenizi sağlar ve yukarıda açıklanan işlevsellik türünü sağlar.

![IOT iletileri](media/blockchain-workbench-integration-patterns/iot.png)

Bu uygulanan için yukarıdaki işlemi bir desen gösterir:

-   Bir aygıt, doğrudan veya bir alan ağ geçidi IOT Hub'ına üzerinden iletişim kurar.
-   IOT Hub iletileri alır ve iletileri yollar kurulan karşı değerlendirir örneğin iletinin içeriği denetleyin. *50 derece büyük bir sıcaklık algılayıcısı bildiriliyor mu?*
-   IOT hub'ı yol için tanımlı bir hizmet veri yolu kriterleri karşılayan iletileri gönderir.
-   Bir mantıksal uygulama ya da başka bir kod IOT hub'ı kurulduğunda Service Bus için rota dinler.
-   Mantıksal uygulama ya da başka bir kod alır ve bilinen bir biçime iletisini dönüştürün.
-   Dönüştürülmüş ileti artık standart bir biçimde için hizmet veri yolu için Azure Blockchain çalışma ekranı gönderilir.
-   Azure Blockchain çalışma ekranı olaylara hizmet yolundan abone olur ve iletiyi alır.
-   Azure Blockchain çalışma ekranı, belirli bir sözleşmeyle dış sistemden veri gönderme defter bir çağrı başlatır.
-   İleti alındığında sözleşme veri değerlendirir ve olabilir yüksek sıcaklık için bu gibi bir durumda değerlendirme sonucunu temel alarak durum değişikliği, durum değişikliği *dışı Uyumluluk*.

## <a name="data-integration"></a>Veri tümleştirmesi

REST ve ileti tabanlı API ek olarak, Azure Blockchain çalışma ekranı Ayrıca uygulama ve sözleşme meta verileri ve bunun yanı sıra dağıtılmış defterleri işlem verileri ile doldurulan bir SQL DB erişim sağlar.

![Veri tümleştirmesi](media/blockchain-workbench-integration-patterns/data-integration.png)

Veri tümleştirme iyi bilinen şöyledir:

-   Azure Blockchain çalışma ekranı uygulamalar, iş akışları, sözleşmeler ve işlemler hakkındaki meta verileri normal işletim davranışını bir parçası olarak depolar.
-   Veritabanı, veritabanı sunucusu adı, veritabanı adı, türü kimlik doğrulaması, oturum açma kimlik bilgileri ve kullanmak için hangi veritabanı görünümler gibi ilgili bilgilerin toplanması kolaylaştırmak için bir veya daha fazla iletişim kutuları bir harici sisteme veya araçlar sağlar.
-   Sorguları aşağı akış tüketim kolaylaştırmak için SQL veritabanı görünümleri dış sistemler, hizmetleri, raporlama, geliştirici araçları ve kurumsal üretkenlik araçları tarafından yazılır.

## <a name="storage-integration"></a>Depolama tümleştirme

Attestable dosyaları birleştirmek için gereken birçok senaryo gerektirebilir. Birden çok nedeniyle, bir blockchain dosyalarda yerleştirme için uygun olacaktır. Bunun yerine, bir ortak bir dosya karşı bir şifreleme karması (örneğin, SHA-256) gerçekleştirmek ve bu karma dağıtılmış bir muhasebe paylaşmak için bir yaklaşımdır. Karma gelecekteki istediğiniz zaman tekrar gerçekleştirmek, aynı sonucu döndürmelidir. Dosya değiştirilirse tek piksel görüntüdeki değiştirilmiş olsa bile, karma farklı bir değer döndürür.

![Depolama tümleştirme](media/blockchain-workbench-integration-patterns/storage-integration.png)

Desen uygulanabilir burada:

-   Bir dış sistem Azure depolama gibi bir depolama mekanizmasını dosyasında devam ettirir.
-   Bir karma dosya veya dosya oluşturulur ve ilişkili meta verileri sahibi, dosyanın bulunduğu, URL için bir tanımlayıcı gibi vb.
-   Karma ve herhangi bir meta veri gönderildiği bir işlev akıllı sözleşmesindeki gibi *FileAdded*
-   Gelecekte, meta verileri ve dosya yeniden karma ve değiştirebilirsiniz defter üzerinde depolanan değerleri karşı karşılaştırılan.

## <a name="prerequisites-for-implementing-integration-patterns-using-the-rest-and-message-apis"></a>REST ve ileti API'ları kullanarak tümleştirme desenleri uygulama için Önkoşullar

REST ya da ileti API kullanarak akıllı sözleşme ile etkileşim özelliği bir dış sistem veya cihaz için kolaylaştırmak için aşağıdaki ortaya çıkması gereken-

1. Konsorsiyumu Azure Active Directory'de Dış Sistem veya aygıt temsil eden bir hesabı oluşturulur.
2. Azure Blockchain çalışma ekranı uygulamanız için uygun akıllı sözleşmelerinin Dış Sistem veya aygıttan olayları kabul etmek için tanımlı işlevlere sahiptir.
3. Akıllı sözleşme için uygulama yapılandırma dosyası sistem veya aygıt olacaktır rolü içeren atanmış.
4. Akıllı sözleşme için uygulama yapılandırma dosyası, bu işlevin tanımlı rol tarafından çağrılabilir bildiren tanımlar.
5. Uygulama yapılandırma dosyasını ve akıllı sözleşmeleri Azure Blockchain çalışma ekranına yüklenir.

Uygulama yüklendikten sonra dış sistem için Azure Active Directory hesabı sözleşme ve ilişkili rol atanır.

## <a name="testing-external-system-integration-flows-prior-to-writing-integration-code"></a>Tümleştirme kod yazma önce Dış Sistem tümleştirme akışları test etme 

Dış sistemlerle tümleştirmek olanağı sağlayan birçok senaryoların anahtar gerekli değildir. Dış sistemlerle tümleştirmek için akıllı sözleşme tasarım önceki veya kod geliştirme paralel doğrulayabilmesi için iyi bir şeydir.

Azure Active Directory (Azure AD) kullanımını önemli ölçüde Geliştirici üretkenliği ve saat değerine hızlandırabilir. Özellikle, bir dış sistem kod tümleşme Önemsiz olmayan bir süre alabilir. Azure AD kullanarak ve otomatik olarak oluşturulmasını UX Azure Blockchain çalışma ekranı tarafından bu çalışma ekranı, dış sistem olarak oturum açın ve UX aracılığıyla bu dış sistemden beklenen değerleri doldurmak geliştiricilerin izin verebilirsiniz Bu hızlı geliştirme ve fikirleri kavram ortamına da öncesinde kanıtı ya da dış sistemler için yazılan tümleştirme koduna paralel, doğrulama sağlar.
