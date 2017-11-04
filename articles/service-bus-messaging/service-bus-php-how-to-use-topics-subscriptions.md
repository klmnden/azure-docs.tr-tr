---
title: "PHP ile Service Bus konu başlıklarını kullanma | Microsoft Docs"
description: "Azure'da PHP ile Service Bus konu başlıklarını kullanmayı öğrenin."
services: service-bus-messaging
documentationcenter: php
author: sethmanheim
manager: timlt
editor: 
ms.assetid: faaa4bbd-f6ef-42ff-aca7-fc4353976449
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 10/06/2017
ms.author: sethm
ms.openlocfilehash: 4763b172375668213372e6f4f8fc87431c430e53
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-php"></a>Service Bus konuları ve abonelikleri PHP ile kullanma

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Bu makalede, Service Bus konuları ve abonelikleri nasıl kullanılacağı gösterilmektedir. PHP ve kullanım örnekleri yazılır [PHP için Azure SDK](../php-download-sdk.md). Kapsamdaki senaryolar dahil **konuları ve abonelikleri oluşturma**, **abonelik filtreleri oluşturma**, **konu başlığına ileti gönderme**, **abonelikten ileti alma**, ve **konuları ve abonelikleri silmeyi**.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-php-application"></a>PHP uygulaması oluşturma
Azure Blob hizmete erişen bir PHP uygulaması oluşturmak için yalnızca sınıflarda başvurmak için gereksinimdir [PHP için Azure SDK](../php-download-sdk.md) gelen kodunuzu içinde. Herhangi bir geliştirme aracı, uygulama veya Notepad oluşturmak için kullanabilirsiniz.

> [!NOTE]
> PHP yüklemenizi de olmalıdır [OpenSSL uzantısı](http://php.net/openssl) yüklenir ve etkinleştirilir.
> 
> 

Bu makalede, bir PHP uygulamanızda yerel olarak veya bir Azure web rolü, çalışan rolü veya Web sitesi içinde çalışan kodu çağrılabilir hizmet özelliklerini kullanmayı açıklar.

## <a name="get-the-azure-client-libraries"></a>Azure istemci kitaplıkları Al
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Service Bus hizmetini kullanmak için uygulamanızı yapılandırın
Service Bus API'lerine kullanmak için:

1. Otomatik Yükleyiciden kullanarak dosya başvuru [require_once] [ require-once] deyimi.
2. Kullanabileceğinize sınıfları başvuru.

Aşağıdaki örnek otomatik Yükleyiciden dosya ve başvuru dahil gösterilmektedir **ServiceBusService** sınıfı.

> [!NOTE]
> Bu örnek (ve diğer örnekleri bu makalede) oluşturucu aracılığıyla Azure için PHP istemci kitaplıkları yüklü olduğunu varsayar. El ile veya bir ARMUTLU paketi olarak kitaplıkları yüklediyseniz, başvurmalıdır **WindowsAzure.php** otomatik yükleyici dosyası.
> 
> 

```php
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

Aşağıdaki örneklerde, `require_once` deyimi her zaman gösterilir, ancak yalnızca örnek yürütmek gerekli sınıfları başvurulur.

## <a name="set-up-a-service-bus-connection"></a>Hizmet veri yolu bağlantı kurma
Service Bus istemci örneği oluşturmak için önce geçerli bir bağlantı dizesi şu biçimde olmalıdır:

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

Burada `Endpoint` genellikle biçimidir `https://[yourNamespace].servicebus.windows.net`.

Kullanmalısınız herhangi bir Azure hizmeti istemcisi oluşturmak için `ServicesBuilder` sınıfı. Şunları yapabilirsiniz:

* Bağlantı dizesi doğrudan geçirin.
* kullanmak **CloudConfigurationManager (CCM)** bağlantı dizesi için dış kaynaklardan denetlemek için:
  * Varsayılan olarak, bir dış kaynak - ortam değişkenleri desteği ile birlikte gelir.
  * Genişleterek yeni kaynakları ekleyebilirsiniz `ConnectionStringSource` sınıfı.

Burada özetlenen örnekler için bağlantı dizesi doğrudan geçirilir.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a>Konu başlığı oluşturma
Service Bus konu başlıklarına yönelik yönetim işlemlerini gerçekleştirebilirsiniz `ServiceBusRestProxy` sınıfı. A `ServiceBusRestProxy` nesnesi aracılığıyla yapılandırılmıştır `ServicesBuilder::createServiceBusService` yönetmek için belirteç izinleri yalıtan bir uygun bir bağlantı dizesi ile Üreteç yöntemi.

Aşağıdaki örnek örneği gösterilmektedir bir `ServiceBusRestProxy` ve arama `ServiceBusRestProxy->createTopic` adlandırılan bir konu oluşturmak için `mytopic` içinde bir `MySBNamespace` ad alanı:

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\TopicInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {        
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
> Kullanabileceğiniz `listTopics` yöntemi `ServiceBusRestProxy` nesneleri zaten bir hizmet ad alanında belirtilen ada sahip bir konu var olup olmadığını denetleyin.
> 
> 

## <a name="create-a-subscription"></a>Abonelik oluşturma
Konu aboneliklerini içeren de oluşturulur `ServiceBusRestProxy->createSubscription` yöntemi. Abonelikler adlandırılır ve aboneliğin sanal kuyruğuna gönderilen ileti kümesini sınırlayan isteğe bağlı bir filtre içerebilir.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Varsayılan (MatchAll) filtreyle abonelik oluşturma
**MatchAll** filtresi, yeni bir abonelik oluşturulurken filtre belirtilmeyen durumlarda kullanılan varsayılan filtredir. Zaman **MatchAll** filtre kullanıldığında, konu başlığında yayımlanan tüm iletiler aboneliğin sanal kuyruğuna yerleştirilir. Aşağıdaki örnekte 'mysubscription' adlı bir abonelik oluşturulur ve varsayılan **MatchAll** Filtresi.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\SubscriptionInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
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
Bir konu başlığına gönderilen iletilerden hangilerinin belirli bir konu başlığı aboneliğinde görüneceğini belirlemenize olanak sağlayan filtreler de ayarlayabilirsiniz. Filtre abonelikler tarafından desteklenen en esnek türü [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter), SQL92 alt kümesi uygular. SQL filtreleri, konu başlığında yayımlanan iletilerin özelliklerinde çalışır. SqlFilters hakkında daha fazla bilgi için bkz: [SqlFilter.SqlExpression özelliği][sqlfilter].

> [!NOTE]
> Bir abonelikte her bir kural kendi sonuç iletileri için abonelik ekleme gelen iletileri bağımsız olarak işler. Ayrıca, her yeni abonelik varsayılan sahip **kural** tüm iletileri konusundan aboneliğine ekler bir filtre ile nesne. Filtrenizle eşleşen iletileri almak için varsayılan kuralı kaldırmanız gerekir. Varsayılan kural kullanarak kaldırabilirsiniz `ServiceBusRestProxy->deleteRule` yöntemi.
> 
> 

Aşağıdaki örnek adlı bir abonelik oluşturur `HighMessages` ile bir **SqlFilter** özel iletileri yalnızca seçer `MessageNumber` özelliği 3'ten büyük. Bkz: [konu başlığına ileti gönderme](#send-messages-to-a-topic) iletileri özel özellikler ekleme hakkında bilgi.

```php
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

Bu kod ek bir ad alanı kullanılmasını gerektiren Not: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.

Benzer şekilde, aşağıdaki örnekte adlı bir abonelik oluşturulur `LowMessages` ile bir `SqlFilter` olan iletiler yalnızca seçer bir `MessageNumber` özelliğine daha az veya bu değere eşit 3.

```php
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

Şimdi, ne zaman bir ileti gönderilir `mytopic` konu her zaman teslim alıcılara `mysubscription` aboneliği ve alıcılara teslim seçmeli olarak `HighMessages` ve `LowMessages` (bağlı olarak abonelikleri ileti içeriği).

## <a name="send-messages-to-a-topic"></a>Konu başlığına ileti gönderme
Uygulama çağrılarınızı bir Service Bus konu başlığına bir ileti göndermek için `ServiceBusRestProxy->sendTopicMessage` yöntemi. Aşağıdaki kod bir ileti göndermek nasıl gösterir `mytopic` konu içinde daha önce oluşturduğunuz `MySBNamespace` hizmet ad alanı.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\BrokeredMessage;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
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

Service Bus konu başlıklarına gönderilen iletiler örnekleridir [BrokeredMessage] [ BrokeredMessage] sınıfı. [BrokeredMessage] [ BrokeredMessage] nesnelerinin standart özellikleri ve yöntemleri yanı sıra, uygulamaya özgü özel özellikleri tutmak için kullanılan özellikler kümesi vardır. Aşağıdaki örnek 5 sınama iletileri göndermek nasıl gösterir `mytopic` daha önce oluşturulan konu. `setProperty` Yöntemi bir özel özellik eklemek için kullanılır (`MessageNumber`) her ileti için. Unutmayın `MessageNumber` özellik değeri değişir her ileti için (gösterildiği gibi alacak abonelikleri belirlemek için bu değeri kullanın [abonelik oluşturma](#create-a-subscription) bölümü):

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

Service Bus konu başlıkları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Konu başlığında tutulan ileti sayısına ilişkin bir sınır yoktur ancak konu başlığı tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Bu konu başlığı boyutu üst sınır 5 GB'tır. Kotalar hakkında daha fazla bilgi için bkz: [Service Bus kotaları][Service Bus quotas].

## <a name="receive-messages-from-a-subscription"></a>Abonelikten ileti alma
Abonelikten ileti almak için en iyi yolu bir `ServiceBusRestProxy->receiveSubscriptionMessage` yöntemi. İletileri iki farklı modda alınan: [ *ReceiveAndDelete* ve *PeekLock*](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode). **PeekLock** varsayılan değerdir.

[ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) modunu kullanırken alma işlemi tek aşamalıdır. Service Bus abonelikte bir iletiye yönelik okuma isteği aldığında, iletiyi kullanılıyor olarak işaretler ve uygulamaya döndürür. [ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) * modu en basit modeldir ve senaryoları bir uygulama içinde tolerans bir arıza olması durumunda bir ileti işlenirken değil en iyi şekilde çalışır. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın. Service Bus iletiyi kullanılıyor olarak işaretlenmiş nedeniyle uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında, sonra da çökmenin öncesinde kullanılan iletiyi atlamış olur.

Varsayılan [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) bir ileti alma modu, iletilere veremeyen uygulamaları desteklemenin mümkün kılar bir iki aşamalı işlemi haline gelir. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan sonra (veya sonra işlemek için depoladıktan sonra), alınan ileti geçirerek alma işleminin ikinci aşamasını tamamlar `ServiceBusRestProxy->deleteMessage`. Hizmet veri yolu gördüğünde `deleteMessage` çağrısı, iletiyi kullanılıyor olarak işaretler ve kuyruktan kaldırın.

Aşağıdaki örnek, almak ve bir iletiyi kullanarak işlemek gösterilmiştir [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) modu (varsayılan). 

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
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
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Alıcı uygulamanın iletiyi herhangi bir nedenden dolayı işleyemedi sonra işleyememesi `unlockMessage` alınan iletide yöntemi (yerine `deleteMessage` yöntemi). Bu işlem Service Bus hizmetinin kuyruktaki iletinin kilidini açmasına ve iletiyi aynı veya başka bir kullanıcı uygulama tarafından tekrar alınabilir hale getirmesine neden olur.

Ayrıca kuyrukta kilitlenen iletiye ilişkin bir zaman aşımı vardır ve uygulama önce iletiyi işleyemezse (örneğin, uygulama çökerse) Service Bus otomatik olarak iletinin kilidini açar ve hale kilit zaman aşımı dolmadan yeniden alınabilmesi kullanılabilir.

Uygulama iletiyi ancak önce çökmesi durumunda, `deleteMessage` isteği bildirilmeden, sonra yeniden başlatıldığında ileti uygulamaya tekrar teslim. Bu genellikle adlandırılır *en az bir kez* diğer bir deyişle, her ileti en az bir kez işlenir işleme; ancak belirli durumlarda aynı ileti yeniden teslim. Senaryo yinelenen işlemeyi kabul etmiyorsa, uygulama geliştiricilerinin yinelenen ileti teslimine uygulamalar için ek mantık eklemelisiniz. Bu genellikle kullanılarak elde edilen `getMessageId` iletinin teslimat denemelerinde yöntemi.

## <a name="delete-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri silme
Bir konu veya abonelik silmek için kullanın `ServiceBusRestProxy->deleteTopic` veya `ServiceBusRestProxy->deleteSubscripton` yöntemleri, sırasıyla. Konu başlığı silindiğinde, konu başlığıyla kaydedilen tüm abonelikler de silinir olduğunu unutmayın.

Aşağıdaki örnek, adlandırılan bir konu silmek gösterilmiştir `mytopic` ve kayıtlı abonelikler.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\ServiceBus\ServiceBusService;
use WindowsAzure\ServiceBus\ServiceBusSettings;
use WindowsAzure\Common\ServiceException;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {        
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
Service Bus kuyruklarına öğrendiğinize göre bkz: [kuyruklar, konu başlıkları ve abonelikler] [ Queues, topics, and subscriptions] daha fazla bilgi için.

[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter
[require-once]: http://php.net/require_once
[Service Bus quotas]: service-bus-quotas.md
