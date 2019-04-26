---
title: PHP ile Service Bus kuyruklarını kullanma | Microsoft Docs
description: Azure'da Service Bus kuyruklarını kullanmayı öğrenin. PHP'de yazılan kod örneklerini.
services: service-bus-messaging
documentationcenter: php
author: axisc
manager: timlt
editor: spelluru
ms.assetid: e29c829b-44c5-4350-8f2e-39e0c380a9f2
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/10/2019
ms.author: aschhab
ms.openlocfilehash: 55eee839e24db2ad96eb635adc488e9a119c5907
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60201729"
---
# <a name="how-to-use-service-bus-queues-with-php"></a>PHP ile Service Bus kuyruklarını kullanma
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Bu öğreticide, bir Service Bus kuyruğundaki iletileri alıp ileti göndermek için PHP uygulamalarının nasıl oluşturulacağını öğrenin. 

## <a name="prerequisites"></a>Önkoşullar
1. Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Etkinleştirebilir, [MSDN abone Avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) veya kaydolun bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
2. Çalışmak için bir kuyruk yoksa, izleyeceğiniz adımlar [Service Bus kuyruğuna oluşturmak için Azure portalını kullanın](service-bus-quickstart-portal.md) makalenin bir kuyruk oluşturun.
    1. Hızlı Okuma **genel bakış** Service Bus **kuyrukları**. 
    2. Hizmet veri yolu oluşturma **ad alanı**. 
    3. Alma **bağlantı dizesi**. 

        > [!NOTE]
        > Oluşturacağınız bir **kuyruk** Bu öğreticide PHP kullanarak Service Bus ad alanında. 
3. [PHP için Azure SDK](../php-download-sdk.md)

## <a name="create-a-php-application"></a>PHP uygulaması oluşturma
Sınıflarda başvuran Azure Blob hizmete erişen bir PHP uygulaması oluşturmak için tek gereksinim olmasıdır [PHP için Azure SDK'sı](../php-download-sdk.md) gelen kod içinde. Herhangi bir geliştirme aracı, uygulama veya not defteri oluşturmak için kullanabilirsiniz.

> [!NOTE]
> PHP yüklemenizi de olmalıdır [OpenSSL uzantısı](https://php.net/openssl) yüklenir ve etkinleştirilir.

Bu kılavuzda, yerel olarak PHP uygulaması içinde veya bir Azure web rolü, çalışan rolü veya Web sitesi içinde çalışan kod çağrılabilen hizmet özelliklerini kullanır.

## <a name="get-the-azure-client-libraries"></a>Azure istemci kitaplıkları Al
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Service Bus hizmetini kullanmak için uygulamanızı yapılandırma
Service Bus kuyruğu API'leri kullanmak için aşağıdakileri yapın:

1. Otomatik Yükleyici dosyasını kullanarak başvuru [require_once] [ require_once] deyimi.
2. Tüm sınıflar kullanabileceğinize başvuru.

Aşağıdaki örnekte, başvuru ve otomatik yükleyici dosyasını içerecek şekilde gösterilmektedir `ServicesBuilder` sınıfı.

> [!NOTE]
> Bu örnekte (ve diğer örnekleri bu makaledeki) Oluşturucusu aracılığıyla Azure için PHP istemci kitaplıkları yüklü olduğunu varsayar. El ile veya bir ARMUTLU paketi olarak kitaplıkların yüklü değilse, başvurmalıdır **WindowsAzure.php** onarıldığında dosya.
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

Aşağıdaki örneklerde `require_once` ifade her zaman gösterilir, ancak yalnızca örnek yürütmek gereken sınıfların başvurulur.

## <a name="set-up-a-service-bus-connection"></a>Service Bus bağlantı kurma
Service Bus istemci oluşturmak için önce geçerli bir bağlantı dizesi şu biçimde olmalıdır:

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

Burada `Endpoint` genellikle şu biçimdedir `[yourNamespace].servicebus.windows.net`.

Herhangi bir Azure hizmeti istemcisi oluşturmak için kullanmanız gerekir `ServicesBuilder` sınıfı. Şunları yapabilirsiniz:

* Doğrudan bağlantı dizesini geçirin.
* Kullanım **CloudConfigurationManager (CCM)** bağlantı dizesi için dış kaynaklardan denetlemek için:
  * Varsayılan olarak, bir dış kaynak - ortam değişkenleri için destek gelir
  * Genişleterek yeni kaynaklar ekleyebilirsiniz `ConnectionStringSource` sınıfı

Burada özetlenen örnekler için bağlantı dizesi doğrudan geçirilir.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-queue"></a>Bir kuyruk oluşturma
Service Bus kuyruklarına yönelik yönetim işlemlerini gerçekleştirebilirsiniz `ServiceBusRestProxy` sınıfı. A `ServiceBusRestProxy` nesnesi aracılığıyla oluşturulduğunda `ServicesBuilder::createServiceBusService` belirteci yönetme izni kapsülleyen bir uygun bir bağlantı dizesi ile fabrika yöntemi.

Aşağıdaki örnek, örneği gösterilmiştir bir `ServiceBusRestProxy` ve çağrı `ServiceBusRestProxy->createQueue` adında bir kuyruk oluşturmak için `myqueue` içinde bir `MySBNamespace` hizmet ad alanı:

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
> Kullanabileceğiniz `listQueues` metodunda `ServiceBusRestProxy` nesneleri belirtilen adda bir kuyruk, bir ad alanı içinde zaten mevcut olup olmadığını denetleyin.
> 
> 

## <a name="send-messages-to-a-queue"></a>Kuyruğa ileti gönderme
Uygulama çağrılarınızı, bir Service Bus kuyruğuna bir ileti göndermek için `ServiceBusRestProxy->sendQueueMessage` yöntemi. Aşağıdaki kod bir ileti göndermek nasıl gösterir `myqueue` daha önce oluşturulan kuyruk `MySBNamespace` hizmet ad alanı.

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

Gönderilen (ve öğesinden alınan) hizmeti kuyrukları örnekleridir Bus iletileri [BrokeredMessage] [ BrokeredMessage] sınıfı. [BrokeredMessage] [ BrokeredMessage] nesnelerin bir dizi standart yöntemleri ve özel uygulamaya özgü özellikler ve rastgele uygulama verileri gövdesi tutmak için kullanılan özellikleri vardır.

Service Bus kuyrukları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Kuyrukta tutulan ileti sayısına ilişkin bir sınır yoktur ancak kuyruk tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Bu kuyruk boyutu üst sınırı 5 GB'tır.

## <a name="receive-messages-from-a-queue"></a>Bir kuyruktan ileti alma

En iyi yolu, bir kuyruktan ileti almak için kullanılacak olan bir `ServiceBusRestProxy->receiveQueueMessage` yöntemi. İleti, iki farklı modda alınabilir: [*ReceiveAndDelete* ](/dotnet/api/microsoft.servicebus.messaging.receivemode) ve [ *PeekLock*](/dotnet/api/microsoft.servicebus.messaging.receivemode#Microsoft_ServiceBus_Messaging_ReceiveMode_PeekLock). **PeekLock** varsayılan değerdir.

Kullanırken [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) modunda almak bir tek işlem; diğer bir deyişle, Service Bus kuyruk iletiye yönelik Okuma isteği aldığında, iletiyi kullanılıyor olarak işaretler ve uygulamaya döndürür. [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) modu, en basit modeldir ve uygulamanın hata oluştuğunda bir iletinin işlenmemesine izin verebileceği senaryolarda en iyi şekilde çalışır. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın. Service Bus iletiyi kullanılıyor olarak işaretleyeceğinden, uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında ardından onu çökmenin öncesinde kullanılan iletiyi atlamış olur.

Varsayılan [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode#Microsoft_ServiceBus_Messaging_ReceiveMode_PeekLock) bir mesaj modu, atlanan iletilere veremeyen uygulamaları desteklemenin mümkün hale getiren bir iki aşamalı işlemi haline gelir. Service Bus bir istek aldığında, kullanılacak sonraki iletiyi bulur, diğer tüketicilerin iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan sonra (veya güvenilir bir şekilde işlemek üzere depolar sonra) alınan iletinin geçirerek alma işleminin ikinci aşamasını tamamlar `ServiceBusRestProxy->deleteMessage`. Service Bus gördüğünde `deleteMessage` çağrı, bu iletiyi kullanılıyor olarak işaretler ve kuyruktan kaldırın.

Aşağıdaki örnek almak ve bir iletiyi kullanarak işlemek nasıl gösterir [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode#Microsoft_ServiceBus_Messaging_ReceiveMode_PeekLock) modu (varsayılan mod).

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

Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Alıcı uygulamanın iletiyi işlemek için herhangi bir nedenle silemiyor sonra çağırabilirsiniz `unlockMessage` yöntemi alınan iletide (yerine `deleteMessage` yöntemi). Bu, Service Bus'ın Kuyruktaki iletinin kilidini açmasına ve iletiyi aynı kullanıcı uygulama veya başka bir kullanıcı uygulama tarafından tekrar alınabilir hale neden olur.

Ayrıca kuyrukta kilitlenen iletiye ilişkin bir zaman aşımı yoktur ve uygulama önce iletiyi işleyemezse (örneğin, uygulama çökerse) Service Bus otomatik olarak iletinin kilidini açmasına ve hale kilit zaman aşımı dolmadan tekrar kullanılabilir.

Uygulama iletiyi ancak önce çökmesi durumunda, `deleteMessage` isteği bildirilmeden, sonra yeniden başlatıldığında ileti uygulamaya yeniden teslim edilebilir. Buna genellikle denir *en az bir kez* diğer bir deyişle, her ileti en az bir kez işlenir işleme; ancak belirli durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işlemeyi kabul etmiyorsa, sonra ek mantık yinelenen ileti teslimine uygulamaları ekleyerek önerilir. Bu genellikle kullanılmasıdır `getMessageId` yöntemi iletinin teslim denemeleri arasında sabit kalır.

## <a name="next-steps"></a>Sonraki adımlar
Service Bus kuyruklarına ilişkin temel bilgileri öğrendiğinize göre artık bkz [kuyruklar, konular ve abonelikler] [ Queues, topics, and subscriptions] daha fazla bilgi için.

Ayrıca daha fazla bilgi için ziyaret [PHP Geliştirici Merkezi](https://azure.microsoft.com/develop/php/).

[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[require_once]: https://php.net/require_once


