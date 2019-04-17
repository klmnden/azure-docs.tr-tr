---
title: PHP ile Service Bus konu başlıklarını kullanma | Microsoft Docs
description: Azure'da PHP ile Service Bus konu başlıklarını kullanmayı öğrenin.
services: service-bus-messaging
documentationcenter: php
author: axisc
manager: timlt
editor: spelluru
ms.assetid: faaa4bbd-f6ef-42ff-aca7-fc4353976449
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/15/2019
ms.author: aschhab
ms.openlocfilehash: 1ce9c5ddb08f3e81a0f0050048a8afef24e4c625
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59607543"
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-php"></a>PHP ile Service Bus konu başlıklarını ve aboneliklerini kullanma

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Bu makalede hizmet veri yolu konuları ve abonelikleri kullanmayı gösterir. PHP ve kullanım örnekleri yazılır [PHP için Azure SDK'sı](../php-download-sdk.md). Kapsanan senaryolar şunlardır:

- Konuları ve abonelikleri oluşturma 
- Abonelik filtreleri oluşturma 
- Bir konu başlığına ileti gönderme 
- Abonelikten ileti alma
- Konuları ve abonelikleri silme

## <a name="prerequisites"></a>Önkoşullar
1. Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Etkinleştirebilir, [Visual Studio veya MSDN abone Avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) veya kaydolun bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
2. İzleyeceğiniz adımlar [hızlı başlangıç: Bir Service Bus konusu ve konu için Abonelik oluşturmak için Azure portal'ı kullanmanızı](service-bus-quickstart-topics-subscriptions-portal.md) bir Service Bus'ı oluşturmak için **ad alanı** ve **bağlantı dizesi**.

    > [!NOTE]
    > Oluşturacağınız bir **konu** ve **abonelik** kullanarak konuya **PHP** Bu hızlı başlangıçta. 

## <a name="create-a-php-application"></a>PHP uygulaması oluşturma
Azure Blob hizmete erişen bir PHP uygulaması oluşturmak için sınıflar başvurmak için tek gereksinim olmasıdır [PHP için Azure SDK'sı](../php-download-sdk.md) gelen kod içinde. Herhangi bir geliştirme aracı, uygulama veya not defteri oluşturmak için kullanabilirsiniz.

> [!NOTE]
> PHP yüklemenizi de olmalıdır [OpenSSL uzantısı](https://php.net/openssl) yüklenir ve etkinleştirilir.
> 
> 

Bu makalede, yerel olarak PHP uygulaması içinde veya bir Azure web rolü, çalışan rolü veya Web sitesi içinde çalışan kod çağrılabilir service özelliklerini kullanmayı açıklar.

## <a name="get-the-azure-client-libraries"></a>Azure istemci kitaplıkları Al
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Service Bus hizmetini kullanmak için uygulamanızı yapılandırma
Service Bus API'lerine kullanmak için:

1. Otomatik Yükleyici dosyasını kullanarak başvuru [require_once] [ require-once] deyimi.
2. Tüm sınıflar kullanabileceğinize başvuru.

Aşağıdaki örnekte, başvuru ve otomatik yükleyici dosyasını içerecek şekilde gösterilmektedir **ServiceBusService** sınıfı.

> [!NOTE]
> Bu örnekte (ve diğer örnekleri bu makaledeki) Oluşturucusu aracılığıyla Azure için PHP istemci kitaplıkları yüklü olduğunu varsayar. El ile veya bir ARMUTLU paketi olarak kitaplıkların yüklü değilse, başvurmalıdır **WindowsAzure.php** onarıldığında dosya.
> 
> 

```php
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

Aşağıdaki örneklerde, `require_once` ifade her zaman gösterilir, ancak yalnızca örnek yürütmek gereken sınıfların başvurulur.

## <a name="set-up-a-service-bus-connection"></a>Service Bus bağlantı kurma
Service Bus istemci oluşturmak için önce geçerli bir bağlantı dizesi şu biçimde olmalıdır:

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

Burada `Endpoint` genellikle şu biçimdedir `https://[yourNamespace].servicebus.windows.net`.

Herhangi bir Azure hizmeti istemcisi oluşturmak için kullanmanız gerekir `ServicesBuilder` sınıfı. Şunları yapabilirsiniz:

* Doğrudan bağlantı dizesini geçirin.
* Kullanım **CloudConfigurationManager (CCM)** bağlantı dizesi için dış kaynaklardan denetlemek için:
  * Varsayılan olarak, bir dış kaynak - ortam değişkenlerini desteği ile birlikte gelir.
  * `ConnectionStringSource` sınıfını genişleterek yeni kaynaklar ekleyebilirsiniz.

Burada özetlenen örnekler için bağlantı dizesi doğrudan geçirilir.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a>Konu başlığı oluşturma
Service Bus konu başlıklarını yönelik yönetim işlemlerini gerçekleştirebilirsiniz `ServiceBusRestProxy` sınıfı. A `ServiceBusRestProxy` nesnesi aracılığıyla oluşturulduğunda `ServicesBuilder::createServiceBusService` belirteci yönetme izni kapsülleyen bir uygun bir bağlantı dizesi ile fabrika yöntemi.

Aşağıdaki örnek, örneği gösterilmiştir bir `ServiceBusRestProxy` ve çağrı `ServiceBusRestProxy->createTopic` adlı bir konu oluşturmak için `mytopic` içinde bir `MySBNamespace` ad alanı:

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\TopicInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try {
    // Create topic.
    $topicInfo = new TopicInfo("mytopic");
    $serviceBusRestProxy->createTopic($topicInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [!NOTE]
> Kullanabileceğiniz `listTopics` metodunda `ServiceBusRestProxy` nesneleri belirtilen ada sahip bir konu, bir hizmet ad alanı içinde zaten mevcut olup olmadığını denetleyin.
> 
> 

## <a name="create-a-subscription"></a>Abonelik oluşturma
Konu abonelikleri ile de oluşturulur `ServiceBusRestProxy->createSubscription` yöntemi. Abonelikler adlandırılır ve aboneliğin sanal kuyruğuna gönderilen ileti kümesini sınırlayan isteğe bağlı bir filtre içerebilir.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Varsayılan (MatchAll) filtreyle abonelik oluşturma
Yeni bir abonelik oluşturulurken filtre belirtilmezse **MatchAll** filtre (varsayılan) kullanılır. Zaman **MatchAll** filtresinin kullanılacağının, konu başlığında yayımlanan tüm iletiler aboneliğin sanal kuyruğuna yerleştirilir. Aşağıdaki örnekte adlı bir abonelik oluşturur `mysubscription` ve varsayılan **MatchAll** filtre.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\SubscriptionInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try {
    // Create subscription.
    $subscriptionInfo = new SubscriptionInfo("mysubscription");
    $serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

### <a name="create-subscriptions-with-filters"></a>Filtre içeren abonelik oluşturma
Bir konu başlığına gönderilen iletilerden hangilerinin belirli bir konu başlığı aboneliğinde görüneceğini belirlemenize olanak sağlayan filtreler de ayarlayabilirsiniz. En esnek filtre türü, abonelikler tarafından desteklenen [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter), SQL92 kümesini uygular. SQL filtreleri, konu başlığında yayımlanan iletilerin özelliklerinde çalışır. SqlFilters hakkında daha fazla bilgi için bkz: [SqlFilter.SqlExpression özelliği][sqlfilter].

> [!NOTE]
> Bir Abonelikteki her bir kural sonucu iletilerinin aboneliğe eklemek gelen iletileri birbirinden bağımsız olarak işler. Ayrıca, her yeni abonelik varsayılan sahip **kural** tüm iletilerin konu başlığından aboneliğine ekleyen bir filtre ile nesne. Filtrenizle eşleşen iletileri almak için varsayılan kuralı kaldırmanız gerekir. Varsayılan kural kullanarak kaldırabilirsiniz `ServiceBusRestProxy->deleteRule` yöntemi.
> 
> 

Aşağıdaki örnekte adlı bir abonelik oluşturur `HighMessages` ile bir **SqlFilter** , yalnızca özel bir bulunduran iletileri seçen `MessageNumber` özelliği 3'ten büyük. Bkz: [konu başlığına ileti gönderme](#send-messages-to-a-topic) iletileri için özel özellikler ekleme hakkında bilgi.

```php
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

Bu kod başka bir ad gerektirir: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.

Benzer şekilde, aşağıdaki örnekte adlı bir abonelik oluşturur `LowMessages` ile bir `SqlFilter` , yalnızca bulunduran iletileri seçen bir `MessageNumber` özelliğini daha az veya eşit 3.

```php
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

Şimdi ne zaman bir ileti gönderilir `mytopic` konu, bu her zaman girmediklerinden abone alıcılar için `mysubscription` aboneliği ve seçmeli olarak teslim edilen alıcılar abone için `HighMessages` ve `LowMessages` (bağlı olarak abonelikler ileti içeriği).

## <a name="send-messages-to-a-topic"></a>Konu başlığına ileti gönderme
Uygulama çağrılarınızı, bir Service Bus konusuna bir ileti göndermek için `ServiceBusRestProxy->sendTopicMessage` yöntemi. Aşağıdaki kod bir ileti göndermek nasıl gösterir `mytopic` konu içinde daha önce oluşturduğunuz `MySBNamespace` hizmet ad alanı.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\BrokeredMessage;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try {
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message");

    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Service Bus konu başlıklarına gönderilen iletiler örnekleridir [BrokeredMessage] [ BrokeredMessage] sınıfı. [BrokeredMessage] [ BrokeredMessage] nesnelerin bir dizi standart özellikleri ve yöntemleri yanı sıra, uygulamaya özgü özel özellikleri tutmak için kullanılan özellikleri vardır. Aşağıdaki örnek nasıl beş test iletisi göndereceğinizi gösterir `mytopic` daha önce oluşturulan konu. `setProperty` Yöntemi özel bir özellik eklemek için kullanılır (`MessageNumber`) her ileti için. `MessageNumber` Özellik değeri, her ileti üzerinde değişir (gösterildiği gibi alacak abonelikleri belirlemek için bu değeri kullanın [abonelik oluşturma](#create-a-subscription) bölümü):

```php
for($i = 0; $i < 5; $i++){
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message ".$i);

    // Set custom property.
    $message->setProperty("MessageNumber", $i);

    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
```

Service Bus konu başlıkları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Konu başlığında tutulan ileti sayısına ilişkin bir sınır yoktur ancak konu başlığı tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Bu konu başlığı boyutu üst sınırı 5 GB'tır. Kotaları hakkında daha fazla bilgi için bkz: [Service Bus kotaları][Service Bus quotas].

## <a name="receive-messages-from-a-subscription"></a>Abonelikten ileti alma
Abonelikten ileti almak için en iyi yolu kullanmaktır bir `ServiceBusRestProxy->receiveSubscriptionMessage` yöntemi. İleti, iki farklı modda alınabilir: [*ReceiveAndDelete* ve *PeekLock*](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode). **PeekLock** varsayılan değerdir.

[ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) modunu kullanırken alma işlemi tek aşamalıdır. Service Bus abonelikte bir iletiye yönelik okuma isteği aldığında, iletiyi kullanılıyor olarak işaretler ve uygulamaya döndürür. [ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) * modu en basit modeldir ve içinde bir uygulama tolere edebilen bir hata oluştuğunda bir iletinin işlenmemesine senaryolarda en iyi şekilde çalışır. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın. Service Bus iletiyi kullanılıyor olarak işaretlediğinden, uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında ardından onu çökmenin öncesinde kullanılan iletiyi eksik.

Varsayılan [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) bir mesaj modu, atlanan iletilere veremeyen uygulamaları desteklemenin mümkün hale getiren bir iki aşamalı işlemi haline gelir. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan sonra (veya güvenilir bir şekilde işlemek üzere depolar sonra) alınan iletinin geçirerek alma işleminin ikinci aşamasını tamamlar `ServiceBusRestProxy->deleteMessage`. Service Bus gördüğünde `deleteMessage` çağrı, iletiyi kullanılıyor olarak işaretler ve kuyruktan kaldırın.

Aşağıdaki örnek almak ve bir iletiyi kullanarak işlemek nasıl gösterir [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) modu (varsayılan mod). 

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try {
    // Set receive mode to PeekLock (default is ReceiveAndDelete)
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();

    // Get message.
    $message = $serviceBusRestProxy->receiveSubscriptionMessage("mytopic", "mysubscription", $options);

    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";

    /*---------------------------
        Process message here.
    ----------------------------*/

    // Delete message. Not necessary if peek lock is not set.
    echo "Deleting message...<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Nasıl yapılır: uygulama çökmelerini ve Okunmayan iletileri işleme
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Alıcı uygulamanın iletiyi işlemek için herhangi bir nedenle silemiyor sonra çağırabilirsiniz `unlockMessage` yöntemi alınan iletide (yerine `deleteMessage` yöntemi). Service Bus'ın Kuyruktaki iletinin kilidini açmasına ve iletiyi aynı kullanıcı uygulama veya başka bir kullanıcı uygulama tarafından tekrar alınabilir hale neden olur.

Ayrıca kuyrukta kilitlenen iletiye ilişkin bir zaman aşımı yoktur ve uygulama önce iletiyi işleyemezse (örneğin, uygulama çökerse) Service Bus otomatik olarak iletinin kilidini açar ve hale kilit zaman aşımı dolmadan tekrar kullanılabilir.

Uygulama iletiyi ancak önce çökmesi durumunda, `deleteMessage` isteği bildirilmeden, sonra yeniden başlatıldığında ileti uygulamaya yeniden teslim. Genellikle bu tür bir işlem olarak adlandırılır *en az bir kez* diğer bir deyişle, her ileti en az bir kez işlenir işleme; ancak belirli durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işlemeyi kabul etmiyorsa, uygulama geliştiricilerin ek mantık yinelenen ileti teslimine uygulamaları eklemelisiniz. Genellikle kullanılarak gerçekleştirilir `getMessageId` yöntemi iletinin teslim denemeleri arasında sabit kalır.

## <a name="delete-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri silme
Bir konu veya aboneliği silmek için kullanın `ServiceBusRestProxy->deleteTopic` veya `ServiceBusRestProxy->deleteSubscripton` yöntemleri, sırasıyla. Bir konu başlığı silindiğinde bu konu başlığıyla kaydedilen tüm abonelikler de silinir.

Aşağıdaki örnekte adlı konu başlığına silme gösterir `mytopic` ve onun kayıtlı abonelikler.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\ServiceBus\ServiceBusService;
use WindowsAzure\ServiceBus\ServiceBusSettings;
use WindowsAzure\Common\ServiceException;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try {
    // Delete topic.
    $serviceBusRestProxy->deleteTopic("mytopic");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Kullanarak `deleteSubscription` yöntemi, bir abonelik bağımsız olarak silebilirsiniz:

```php
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için [kuyruklar, konular ve abonelikler][Queues, topics, and subscriptions].

[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter
[require-once]: https://php.net/require_once
[Service Bus quotas]: service-bus-quotas.md
