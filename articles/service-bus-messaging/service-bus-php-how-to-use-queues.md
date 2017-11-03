---
title: "PHP ile Service Bus kuyruklarını kullanma | Microsoft Docs"
description: "Azure'da Service Bus kuyruklarını kullanmayı öğrenin. PHP ile yazılan kod örnekleri."
services: service-bus-messaging
documentationcenter: php
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e29c829b-44c5-4350-8f2e-39e0c380a9f2
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 3514812f7f087582035dad5d9a4d620652aa4da9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-service-bus-queues-with-php"></a>PHP ile Service Bus kuyruklarını kullanma
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Bu kılavuz Service Bus kuyruklarını kullanmayı gösterir. PHP ve kullanım örnekleri yazılır [PHP için Azure SDK](../php-download-sdk.md). Kapsamdaki senaryolar dahil **sıra oluşturma**, **ileti gönderme ve alma**, ve **sıraları silme**.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-php-application"></a>PHP uygulaması oluşturma
Azure Blob hizmete erişen bir PHP uygulaması oluşturmak için yalnızca sınıflarda başvuran gereksinimdir [PHP için Azure SDK](../php-download-sdk.md) gelen kodunuzu içinde. Herhangi bir geliştirme aracı, uygulama veya Notepad oluşturmak için kullanabilirsiniz.

> [!NOTE]
> PHP yüklemenizi de olmalıdır [OpenSSL uzantısı](http://php.net/openssl) yüklenir ve etkinleştirilir.
> 
> 

Bu kılavuzda, bir PHP uygulamanızda yerel olarak veya bir Azure web rolü, çalışan rolü veya Web sitesi içinde çalışan kodu çağrılabilir hizmet özelliklerini kullanır.

## <a name="get-the-azure-client-libraries"></a>Azure istemci kitaplıkları Al
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Service Bus hizmetini kullanmak için uygulamanızı yapılandırın
Service Bus kuyruğuna API'leri kullanmak için aşağıdakileri yapın:

1. Otomatik Yükleyiciden kullanarak dosya başvuru [require_once] [ require_once] deyimi.
2. Kullanabileceğinize sınıfları başvuru.

Aşağıdaki örnek otomatik Yükleyiciden dosya ve başvuru dahil gösterilmektedir `ServicesBuilder` sınıfı.

> [!NOTE]
> Bu örnek (ve diğer örnekleri bu makalede) oluşturucu aracılığıyla Azure için PHP istemci kitaplıkları yüklü olduğunu varsayar. El ile veya bir ARMUTLU paketi olarak kitaplıkları yüklediyseniz, başvurmalıdır **WindowsAzure.php** otomatik yükleyici dosyası.
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

Aşağıdaki örneklerde `require_once` deyimi her zaman gösterilecek, ancak yalnızca örnek yürütmek gerekli sınıfları başvurulur.

## <a name="set-up-a-service-bus-connection"></a>Hizmet veri yolu bağlantı kurma
Service Bus istemci örneği oluşturmak için önce geçerli bir bağlantı dizesi şu biçimde olmalıdır:

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

Burada `Endpoint` genellikle biçimidir `[yourNamespace].servicebus.windows.net`.

Kullanmalısınız herhangi bir Azure hizmeti istemcisi oluşturmak için `ServicesBuilder` sınıfı. Şunları yapabilirsiniz:

* Bağlantı dizesi doğrudan geçirin.
* kullanmak **CloudConfigurationManager (CCM)** bağlantı dizesi için dış kaynaklardan denetlemek için:
  * Bir dış kaynak - ortam değişkenleri için destek ile birlikte varsayılan olarak
  * Genişleterek yeni kaynakları ekleyebilirsiniz `ConnectionStringSource` sınıfı

Burada özetlenen örnekler için bağlantı dizesi doğrudan geçirilir.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-queue"></a>Bir kuyruk oluşturma
Service Bus kuyruklarını yönelik yönetim işlemlerini gerçekleştirebilirsiniz `ServiceBusRestProxy` sınıfı. A `ServiceBusRestProxy` nesnesi aracılığıyla yapılandırılmıştır `ServicesBuilder::createServiceBusService` yönetmek için belirteç izinleri yalıtan bir uygun bir bağlantı dizesi ile Üreteç yöntemi.

Aşağıdaki örnek örneği gösterilmektedir bir `ServiceBusRestProxy` ve arama `ServiceBusRestProxy->createQueue` adlı bir kuyruk oluşturmak için `myqueue` içinde bir `MySBNamespace` hizmet ad alanı:

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\QueueInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    $queueInfo = new QueueInfo("myqueue");

    // Create queue.
    $serviceBusRestProxy->createQueue($queueInfo);
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
> Kullanabileceğiniz `listQueues` yöntemi `ServiceBusRestProxy` nesneleri belirtilen ada sahip bir sıra ad alanı içinde olup olmadığını denetle.
> 
> 

## <a name="send-messages-to-a-queue"></a>Kuyruğa ileti gönderme
Uygulama çağrılarınızı bir Service Bus kuyruğuna bir ileti göndermek için `ServiceBusRestProxy->sendQueueMessage` yöntemi. Aşağıdaki kod bir ileti göndermek nasıl gösterir `myqueue` içinde daha önce oluşturduğunuz sıra `MySBNamespace` hizmet ad alanı.

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
    $serviceBusRestProxy->sendQueueMessage("myqueue", $message);
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

İletileri gönderilen (ve öğesinden alınan) hizmeti sıraları örnekleridir Bus [BrokeredMessage] [ BrokeredMessage] sınıfı. [BrokeredMessage] [ BrokeredMessage] nesneler sahip bir dizi standart yöntemleri ve özel uygulamaya özgü özellikler ve rastgele uygulama verileri gövdesi tutmak için kullanılan özellik.

Service Bus kuyrukları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Kuyrukta tutulan ileti sayısına ilişkin bir sınır yoktur ancak kuyruk tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Bu sıra boyutu üst sınır 5 GB'tır.

## <a name="receive-messages-from-a-queue"></a>Kuyruktan ileti alma

Kuyruktan iletileri almak için en iyi yolu bir `ServiceBusRestProxy->receiveQueueMessage` yöntemi. İletileri iki farklı modda alınan: [ *ReceiveAndDelete* ](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) ve [ *PeekLock*](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock). **PeekLock** varsayılan değerdir.

Kullanırken [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) modu, alma bir tek işlemi; diğer bir deyişle, hizmet veri yolu bir kuyrukta İletiye yönelik Okuma isteği aldığında, iletiyi kullanılıyor olarak işaretler ve uygulamaya döndürür. [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) modu, en basit modeldir ve uygulamanın hata oluştuğunda bir iletinin işlenmemesine izin verebileceği senaryolarda en iyi şekilde çalışır. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın. Service Bus iletiyi kullanılıyor olarak işaretlenmiş nedeniyle uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında, sonra da çökmenin öncesinde kullanılan iletiyi atlamış olur.

Varsayılan [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) bir ileti alma modu, iletilere veremeyen uygulamaları desteklemenin mümkün kılar bir iki aşamalı işlemi haline gelir. Service Bus bir istek aldığında, kullanılacak sonraki iletiyi bulur, diğer tüketicilerin iletiyi almasını engellemek için kilitler ve uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan sonra (veya sonra işlemek için depoladıktan sonra), alınan ileti geçirerek alma işleminin ikinci aşamasını tamamlar `ServiceBusRestProxy->deleteMessage`. Hizmet veri yolu gördüğünde `deleteMessage` çağrısı iletiyi kullanılıyor olarak işaretler ve kuyruktan kaldırın.

Aşağıdaki örnek, almak ve bir iletiyi kullanarak işlemek gösterilmiştir [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) modu (varsayılan).

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Set the receive mode to PeekLock (default is ReceiveAndDelete).
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();

    // Receive message.
    $message = $serviceBusRestProxy->receiveQueueMessage("myqueue", $options);
    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";

    /*---------------------------
        Process message here.
    ----------------------------*/

    // Delete message. Not necessary if peek lock is not set.
    echo "Message deleted.<br />";
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

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme

Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Alıcı uygulamanın iletiyi herhangi bir nedenden dolayı işleyemedi sonra işleyememesi `unlockMessage` alınan iletide yöntemi (yerine `deleteMessage` yöntemi). Bu, Service Bus hizmetinin Kuyruktaki iletinin kilidini açmasına ve iletiyi aynı veya başka bir kullanıcı uygulama tarafından tekrar alınabilir hale getirmesine neden olur.

Ayrıca kuyrukta kilitlenen iletiye ilişkin bir zaman aşımı vardır ve uygulama önce iletiyi işleyemezse (örneğin, uygulama çökerse) hizmet veri yolu ileti otomatik olarak kilidini açmasına ve tekrar alınabilir hale kilit zaman aşımı dolmadan.

Uygulama iletiyi ancak önce çökmesi durumunda, `deleteMessage` isteği bildirilmeden, sonra yeniden başlatıldığında ileti uygulamaya tekrar teslim edilir. Bu genellikle adlandırılır *en az bir kez* diğer bir deyişle, her ileti en az bir kez işlenir işleme; ancak belirli durumlarda aynı ileti yeniden teslim. Senaryo yinelenen işlemeyi kabul etmiyorsa, ardından yinelenen ileti teslimine uygulamalar için ek mantık eklemeniz önerilir. Bu genellikle kullanılarak elde edilen `getMessageId` iletinin teslimat denemelerinde yöntemi.

## <a name="next-steps"></a>Sonraki adımlar
Service Bus kuyruklarına öğrendiğinize göre bkz: [kuyruklar, konu başlıkları ve abonelikler] [ Queues, topics, and subscriptions] daha fazla bilgi için.

Daha fazla bilgi için ayrıca ziyaret [PHP Geliştirici Merkezi](https://azure.microsoft.com/develop/php/).

[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[require_once]: http://php.net/require_once


