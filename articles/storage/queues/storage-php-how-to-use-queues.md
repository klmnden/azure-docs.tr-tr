---
title: Kuyruk Depolama'yı PHP - Azure depolama kullanma
description: Oluşturmak ve Kuyruklar, silmek için Azure kuyruk depolama hizmetini kullanmayı öğrenin ve Ekle, Al ve iletilerini silin. Örnek PHP ile yazılmıştır.
services: storage
author: mhopkins-msft
ms.service: storage
ms.devlang: php
ms.topic: article
ms.date: 01/11/2018
ms.author: mhopkins
ms.reviewer: cbrooks
ms.subservice: queues
ms.openlocfilehash: 4ff54f9ca20c101de55bec2c7acf914c17bd7709
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65951203"
---
# <a name="how-to-use-queue-storage-from-php"></a>PHP’den Kuyruk depolama kullanma
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz Azure kuyruk depolama hizmetini kullanarak, yaygın senaryoları gerçekleştirmek nasıl gösterir. Örnekleri sınıflardan aracılığıyla yazılır [PHP için Azure depolama istemci Kitaplığı][download]. Kapsanan senaryolar, ekleme, gözatma, alma ve kuyruk iletileri siliniyor yanı sıra oluşturma ve kuyrukları silme içerir.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>PHP uygulaması oluşturma
Sınıflarda başvuran Azure kuyruk depolama erişen bir PHP uygulaması oluşturmak için tek gereksinim olmasıdır [PHP için Azure depolama istemci Kitaplığı] [ download] gelen kod içinde. Uygulamanızı oluşturmak için Not Defteri dahil olmak üzere tüm geliştirme araçlarını kullanabilirsiniz.

Bu kılavuzda, yerel olarak PHP uygulaması içinde veya bir Azure web rolü, çalışan rolü veya Web sitesi içinde çalışan kod çağrılabilir kuyruk depolama hizmeti özelliklerini kullanın.

## <a name="get-the-azure-client-libraries"></a>Azure istemci kitaplıkları alma
### <a name="install-via-composer"></a>Oluşturucu yükleme
1. Adlı bir dosya oluşturun **composer.json** projenizin kökünde ve aşağıdaki kodu ekleyin:
   
    ```json
    {
      "require": {
        "microsoft/azure-storage-queue": "*"
      }
    }
    ```
2. İndirme **[composer.phar] [ composer-phar]** proje kökünüze içinde.
3. Bir komut istemi açın ve proje kökünde aşağıdaki komutu yürütün
   
    ```
    php composer.phar install
    ```

Alternatif olarak Git [Azure Storage PHP istemci Kitaplığı] [ download] kaynak kodu kopyalamak için GitHub üzerindeki.

## <a name="configure-your-application-to-access-queue-storage"></a>Kuyruk depolamaya erişmek için uygulamanızı yapılandırma
Azure kuyruk depolama API'leri kullanmak için yapmanız:

1. Otomatik Yükleyici dosyasını kullanarak başvuru [require_once] deyimi.
2. Kullanabileceğiniz tüm sınıflar başvuru.

Aşağıdaki örnekte, başvuru ve otomatik yükleyici dosyasını içerecek şekilde gösterilmektedir **QueueRestProxy** sınıfı.

```php
require_once 'vendor/autoload.php';
use MicrosoftAzure\Storage\Queue\QueueRestProxy;
```

Aşağıdaki örneklerde, `require_once` ifade her zaman gösterilir, ancak örneğin yürütmek için gerekli olan sınıfları başvurulur.

## <a name="set-up-an-azure-storage-connection"></a>Bir Azure depolama bağlantı kurma
Bir Azure kuyruk depolama istemcisi örneği oluşturmak için öncelikle geçerli bir bağlantı dizesi olması gerekir. Kuyruk hizmeti bağlantı dizesi biçimi aşağıdaki gibidir.

Bir live hizmetine erişmek için:

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

Depolama öykünücüsü erişmek için:

```php
UseDevelopmentStorage=true
```

Bir Azure kuyruk hizmeti istemcisi oluşturmak için kullanmanız gerekir **QueueRestProxy** sınıfı. Aşağıdaki tekniklerden birini kullanabilirsiniz:

* Doğrudan bağlantı dizesini geçirin.
* Ortam değişkenlerini kullanarak Web uygulamanızda bağlantı dizesini depolamak için kullanın. Bkz: [Azure web uygulaması yapılandırma ayarları](../../app-service/configure-common.md) bağlantı dizelerini yapılandırma belge.
Burada özetlenen örnekler için bağlantı dizesi doğrudan geçirilir.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";
$queueClient = QueueRestProxy::createQueueService($connectionString);
```

## <a name="create-a-queue"></a>Bir kuyruk oluşturma
A **QueueRestProxy** nesnesi kullanarak bir kuyruk oluşturmanıza olanak tanıyan **createQueue** yöntemi. Bir kuyruğu oluştururken, kuyruktaki seçeneklerini ayarlayabilirsiniz ancak bunu yaparsanız bu nedenle gerekli değildir. (Aşağıdaki örnekte bir kuyruğa meta verileri ayarlama işlemini gösterir.)

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\CreateQueueOptions;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

// OPTIONAL: Set queue metadata.
$createQueueOptions = new CreateQueueOptions();
$createQueueOptions->addMetaData("key1", "value1");
$createQueueOptions->addMetaData("key2", "value2");

try    {
    // Create queue.
    $queueClient->createQueue("myqueue", $createQueueOptions);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [!NOTE]
> Meta veri anahtarları için büyük/küçük harfe duyarlılık dayanarak doğrulamamalısınız. Tüm anahtarları küçük hizmetten okunur.
> 
> 

## <a name="add-a-message-to-a-queue"></a>Kuyruğa bir ileti ekleyin
Bir kuyruğa ileti eklemek için **QueueRestProxy CreateMessage Nesne ->** . Yöntem, kuyruk adı, ileti metni ve (isteğe bağlı) iletinin seçeneklerini alır.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\CreateMessageOptions;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

try    {
    // Create message.
    $queueClient->createMessage("myqueue", "Hello World!");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="peek-at-the-next-message"></a>Sonraki iletiye gözatın
Bir ileti (ya da iletileri) kuyruğun önündeki kuyruktan kaldırmadan çağırarak Özet **QueueRestProxy -> peekMessages**. Varsayılan olarak, **peekMessage** yöntemi tek bir ileti döndürür, ancak kullanarak bu değeri değiştirebilirsiniz **PeekMessagesOptions -> setNumberOfMessages** yöntemi.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\PeekMessagesOptions;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

// OPTIONAL: Set peek message options.
$message_options = new PeekMessagesOptions();
$message_options->setNumberOfMessages(1); // Default value is 1.

try    {
    $peekMessagesResult = $queueClient->peekMessages("myqueue", $message_options);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$messages = $peekMessagesResult->getQueueMessages();

// View messages.
$messageCount = count($messages);
if($messageCount <= 0){
    echo "There are no messages.<br />";
}
else{
    foreach($messages as $message)    {
        echo "Peeked message:<br />";
        echo "Message Id: ".$message->getMessageId()."<br />";
        echo "Date: ".date_format($message->getInsertionDate(), 'Y-m-d')."<br />";
        echo "Message text: ".$message->getMessageText()."<br /><br />";
    }
}
```

## <a name="de-queue-the-next-message"></a>Sonraki iletiyi sıradan çıkarmak
Kodunuzun bir iletiyi bir kuyruktan iki adımda kaldırır. İlk olarak, çağrı **QueueRestProxy -> listMessages**, getiren iletiyi kuyruktan okuyan herhangi bir kod görünmez. Varsayılan olarak bu ileti 30 saniye görünmez kalır. (İleti bu zaman aralığında değil silinirse, yeniden sıraya görünür duruma gelir.) İletiyi kuyruktan kaldırmayı tamamlamak için çağırmalıdır **QueueRestProxy -> deleteMessage**. Kodunuzun bir iletiyi donanım veya yazılım hatası nedeniyle başarısız olduğunda, kodunuzun başka bir örneği aynı iletiyi alıp yeniden deneyin, bu iki adımlı işlem, bir iletinin sağlar. Kod çağrılarınızı **deleteMessage** ileti işlendikten sonra sağ.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

// Get message.
$listMessagesResult = $queueClient->listMessages("myqueue");
$messages = $listMessagesResult->getQueueMessages();
$message = $messages[0];

/* ---------------------
    Process message.
   --------------------- */

// Get message ID and pop receipt.
$messageId = $message->getMessageId();
$popReceipt = $message->getPopReceipt();

try    {
    // Delete message.
    $queueClient->deleteMessage("myqueue", $messageId, $popReceipt);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="change-the-contents-of-a-queued-message"></a>Kuyruğa alınan iletinin içeriğini değiştirme
Bir ileti yerinde sırasındaki içeriğini çağırarak değiştirebilirsiniz **QueueRestProxy -> updateMessage**. Eğer ileti bir iş görevini temsil ediyorsa, bu özelliği kullanarak iş görevinin durumunu güncelleştirebilirsiniz. Aşağıdaki kod kuyruk iletisini yeni içeriklerle güncelleştirir ve görünürlük zaman aşımını 60 saniye daha uzatır ayarlar. Bu iletiyle ilişkili iş durumunu kaydeder ve istemciye ileti üzerinde çalışmaya devam etmek için başka bir dakika sağlar. Bir işleme adımı donanım veya yazılım arızasından dolayı başarısız olursa baştan başlamanıza gerek kalmadan kuyruk iletilerindeki çok adımlı iş akışlarını izlemek için bu yöntemi kullanabilirsiniz. Genellikle bir yeniden deneme sayacı tutmanı gerekir ve bir ileti *n* seferden daha fazla yeniden denenirse, silebilirsiniz. Bu, her işlendiğinde bir uygulama hatası tetikleyen bir iletiye karşı koruma sağlar.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Get message.
$listMessagesResult = $queueClient->listMessages("myqueue");
$messages = $listMessagesResult->getQueueMessages();
$message = $messages[0];

// Define new message properties.
$new_message_text = "New message text.";
$new_visibility_timeout = 5; // Measured in seconds.

// Get message ID and pop receipt.
$messageId = $message->getMessageId();
$popReceipt = $message->getPopReceipt();

try    {
    // Update message.
    $queueClient->updateMessage("myqueue",
                                $messageId,
                                $popReceipt,
                                $new_message_text,
                                $new_visibility_timeout);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="additional-options-for-de-queuing-messages"></a>İletilerin kuyruktan çıkarılması için ek seçenekler
Bir kuyruktan ileti alma özelleştirebilirsiniz iki yolu vardır. İlk olarak toplu iletiler alabilirsiniz (en fazla 32). İkinci olarak, bir uzun veya kısaysa görünebilirlik zaman aşımı, kodunuzun daha fazla veya daha az zaman her iletiyi tamamen işlemesi için izin ayarlayabilirsiniz. Aşağıdaki kod örneğinde **getMessages** tek çağrıda 16 ileti almak için yöntemi. Kullanarak her iletiyi işleyen sonra bir **için** döngü. Ayrıca her ileti için görünmezlik zaman aşımı beş dakika olarak ayarlanır.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\ListMessagesOptions;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

// Set list message options.
$message_options = new ListMessagesOptions();
$message_options->setVisibilityTimeoutInSeconds(300);
$message_options->setNumberOfMessages(16);

// Get messages.
try{
    $listMessagesResult = $queueClient->listMessages("myqueue",
                                                     $message_options);
    $messages = $listMessagesResult->getQueueMessages();

    foreach($messages as $message){

        /* ---------------------
            Process message.
        --------------------- */

        // Get message Id and pop receipt.
        $messageId = $message->getMessageId();
        $popReceipt = $message->getPopReceipt();

        // Delete message.
        $queueClient->deleteMessage("myqueue", $messageId, $popReceipt);
    }
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="get-queue-length"></a>Kuyruk uzunluğu alma
Bir kuyruktaki ileti sayısı ile ilgili bir tahmin alabilirsiniz. **QueueRestProxy -> getQueueMetadata** yöntemi kuyruk hakkındaki meta verileri döndürmek için kuyruk hizmeti sorar. Çağırma **getApproximateMessageCount** yöntemi döndürülen nesne üzerinde bir kuyrukta ileti sayısını olduğunun sayısını sağlar. İletileri eklenebilir veya kuyruk hizmeti, isteğine yanıt vermeden sonra kaldırılan sayısı yalnızca yaklaşık olmasıdır.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

try    {
    // Get queue metadata.
    $queue_metadata = $queueClient->getQueueMetadata("myqueue");
    $approx_msg_count = $queue_metadata->getApproximateMessageCount();
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

echo $approx_msg_count;
```

## <a name="delete-a-queue"></a>Bir kuyruk silme
Bir kuyruk ve tüm iletileri silmek için çağrı **QueueRestProxy -> deleteQueue** yöntemi.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

try    {
    // Delete queue.
    $queueClient->deleteQueue("myqueue");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="next-steps"></a>Sonraki adımlar
Azure kuyruk depolamanın temellerini öğrendiğinize göre daha karmaşık depolama görevleri hakkında bilgi edinmek için bu bağlantıları izleyin:

* Ziyaret [Azure depolama PHP istemci kitaplığı için API Başvurusu](https://azure.github.io/azure-storage-php/)
* Bkz: [kuyruk Gelişmiş örnek](https://github.com/Azure/azure-storage-php/blob/master/samples/QueueSamples.php).

Daha fazla bilgi için Ayrıca bkz: [PHP Geliştirici Merkezi](https://azure.microsoft.com/develop/php/).

[download]: https://github.com/Azure/azure-storage-php
[require_once]: https://www.php.net/manual/en/function.require-once.php
[Azure Portal]: https://portal.azure.com
[composer-phar]: https://getcomposer.org/composer.phar

