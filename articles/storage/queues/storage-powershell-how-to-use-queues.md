---
title: PowerShell - Azure depolama ile Azure kuyruk depolama işlemleri
description: PowerShell ile Azure kuyruk depolama işlemleri gerçekleştirme
services: storage
author: mhopkins-msft
ms.service: storage
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: mhopkins
ms.reviewer: cbrooks
ms.subservice: queues
ms.openlocfilehash: 6e8640b136c52f500de010f842ab73678acdce4f
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65991349"
---
# <a name="perform-azure-queue-storage-operations-with-azure-powershell"></a>Azure PowerShell ile Azure kuyruk depolama işlemleri

Azure kuyruk depolama, çok sayıda herhangi bir HTTP veya HTTPS aracılığıyla dünyanın erişilebilen iletileri depolamak için kullanılan bir hizmettir. Ayrıntılı bilgi için bkz. [Azure kuyruklara giriş](storage-queues-introduction.md). Bu nasıl yapılır makalesi genel kuyruk depolama işlemleri kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
>
> * Kuyruk oluştur
> * Bir kuyruğa alma
> * Bir ileti ekleyin
> * Bir ileti okuma
> * İleti silme
> * Bir kuyruk silme

Bu nasıl yapılır, Azure PowerShell modülü Az 0.7 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps).

Hiçbir veri düzlemi için kuyrukları için PowerShell cmdlet'leri vardır. Bir ileti okuma gerçekleştirmek veri düzlemi işlemleri gibi bir ileti ekleyin ve ileti silme, PowerShell'de sunulur gibi .NET depolama istemcisi kitaplığı kullanmak zorunda. Bir ileti nesnesi oluşturun ve sonra bu ileti üzerinde işlem gerçekleştirmek için AddMessage gibi komutları kullanabilirsiniz. Bu makalede bunu nasıl yapacağınız gösterilmektedir.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>Oturum açın: Azure

`Connect-AzAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

## <a name="retrieve-list-of-locations"></a>Konumların listesini alma

Kullanmak istediğiniz konumdan emin değilseniz, kullanılabilir konumları listeleyebilirsiniz. Liste görüntülendikten sonra, kullanmak istediğiniz öğeyi bulun. Bu alıştırmada kullanacağı **eastus**. Bu değişkende Store **konumu** gelecekte kullanım için.

```powershell
Get-AzLocation | select Location
$location = "eastus"
```

## <a name="create-resource-group"></a>Kaynak grubu oluştur

Bir kaynak grubu oluşturun [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutu.

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Kaynak grubu adı, gelecekte kullanım için bir değişkende Store. Bu örnekte, adlı bir kaynak grubu *howtoqueuesrg* oluşturulur *eastus* bölge.

```powershell
$resourceGroup = "howtoqueuesrg"
New-AzResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

## <a name="create-storage-account"></a>Depolama hesabı oluştur

Yerel olarak yedekli depolama (LRS) kullanarak bir genel amaçlı standart depolama hesabı oluşturma [yeni AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount). Kullanılacak depolama hesabını tanımlayan depolama hesabı bağlamını alın. Depolama hesabında bir işlem gerçekleştirirken, kimlik bilgilerini tekrar tekrar sağlamak yerine bağlama başvurursunuz.

```powershell
$storageAccountName = "howtoqueuestorage"
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName Standard_LRS

$ctx = $storageAccount.Context
```

## <a name="create-a-queue"></a>Kuyruk oluştur

Aşağıdaki örnek, ilk Azure depolama hesabı adını ve erişim anahtarını içeren depolama hesabı bağlamını kullanarak depolama bağlantı kurar. Ardından, çağıran [yeni AzStorageQueue](/powershell/module/az.storage/New-AzStorageQueue) cmdlet'ini 'queuename' adında bir kuyruk oluşturun.

```powershell
$queueName = "howtoqueue"
$queue = New-AzStorageQueue –Name $queueName -Context $ctx
```

Azure kuyruk hizmeti için adlandırma kuralları hakkında bilgi için bkz: [adlandırma kuyrukları ve meta verileri](https://msdn.microsoft.com/library/azure/dd179349.aspx).

## <a name="retrieve-a-queue"></a>Bir kuyruğa alma

Sorgulamak ve belirli bir kuyruğa veya bir depolama hesabındaki tüm kuyrukların listesini alır. Aşağıdaki örnekler, depolama hesabındaki tüm kuyrukları ve belirli bir kuyruğa alma göstermektedir; Her iki komutları [Get-AzStorageQueue](/powershell/module/az.storage/Get-AzStorageQueue) cmdlet'i.

```powershell
# Retrieve a specific queue
$queue = Get-AzStorageQueue –Name $queueName –Context $ctx
# Show the properties of the queue
$queue

# Retrieve all queues and show their names
Get-AzStorageQueue -Context $ctx | select Name
```

## <a name="add-a-message-to-a-queue"></a>Kuyruğa bir ileti ekleyin

Gerçek iletileri etkileyen işlemleri .NET depolama istemci kitaplığı, PowerShell'de gösterilen kullanın. Bir kuyruğa ileti eklemek için yeni bir ileti nesnesi örneğini oluşturma [Microsoft.Azure.Storage.Queue.CloudQueueMessage](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.queue._cloud_queue_message) sınıfı. Ardından [AddMessage](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.queue._cloud_queue.addmessage) yöntemini çağırın. Bir dizeden (UTF-8 biçiminde) veya bir bayt dizisine bir CloudQueueMessage oluşturulabilir.

Aşağıdaki örnek, bir ileti kuyruğuna eklemek gösterilmiştir.

```powershell
# Create a new message using a constructor of the CloudQueueMessage class
$queueMessage = New-Object -TypeName "Microsoft.Azure.Storage.Queue.CloudQueueMessage,$($queue.CloudQueue.GetType().Assembly.FullName)" `
  -ArgumentList "This is message 1"
# Add a new message to the queue
$queue.CloudQueue.AddMessageAsync($QueueMessage)

# Add two more messages to the queue
$queueMessage = New-Object -TypeName "Microsoft.Azure.Storage.Queue.CloudQueueMessage,$($queue.CloudQueue.GetType().Assembly.FullName)" `
  -ArgumentList "This is message 2"
$queue.CloudQueue.AddMessageAsync($QueueMessage)
$queueMessage = New-Object -TypeName "Microsoft.Azure.Storage.Queue.CloudQueueMessage,$($queue.CloudQueue.GetType().Assembly.FullName)" `
  -ArgumentList "This is message 3"
$queue.CloudQueue.AddMessageAsync($QueueMessage)
```

Kullanırsanız [Azure Depolama Gezgini](https://storageexplorer.com), Azure hesabınıza bağlanın ve Kuyruklar depolama hesabında görüntüleyebilir ve detaya gitme kuyrukta iletileri görüntülemek için bir kuyruğun içine.

## <a name="read-a-message-from-the-queue-then-delete-it"></a>Kuyruktan bir ileti okuma ardından silin

En iyi deneme ilk-giren ilk çıkar sırayla ileti okunur. Bu garanti edilmez. Kuyruktan ileti okurken sıraya isteyen tüm diğer işlemler için görünmez hale gelir. Bu iletiyi donanım veya yazılım arızasından dolayı kodunuzun başarısız olursa, kodunuzun başka bir örneği aynı iletiyi alıp yeniden deneyin, sağlar.  

Bu **görünmezlik zaman aşımı** yeniden işleme için kullanılabilir olmadan önce ileti ne kadar süreyle görünmez kalır tanımlar. Varsayılan değer 30 saniyedir.

Kodunuzun bir iletiyi kuyruktan iki adımda okur. Çağırdığınızda [Microsoft.Azure.Storage.Queue.CloudQueue.GetMessage](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.getmessage) kuyrukta sonraki iletiyi elde yöntemi. **GetMessage**’dan dönen bir ileti bu kuyruktaki kod okuyan iletilere karşı görünmez olur. İletiyi kuyruktan kaldırmayı tamamlamak için çağrı [Microsoft.Azure.Storage.Queue.CloudQueue.DeleteMessage](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.deletemessage) yöntemi.

Aşağıdaki örnekte, üç kuyruk iletileri okumak ardından 10 saniye bekleyin (görünmezlik zaman aşımı). Çağırarak okuduğunuzda iletilerini silme yeniden üç iletileri okumak sonra **DeleteMessage**. Kuyruk iletileri silindikten sonra okumak çalışırsanız $queueMessage NULL olarak döndürülür.

```powershell
# Set the amount of time you want to entry to be invisible after read from the queue
# If it is not deleted by the end of this time, it will show up in the queue again
$invisibleTimeout = [System.TimeSpan]::FromSeconds(10)

# Read the message from the queue, then show the contents of the message. Read the other two messages, too.
$queueMessage = $queue.CloudQueue.GetMessageAsync($invisibleTimeout,$null,$null)
$queueMessage.Result
$queueMessage = $queue.CloudQueue.GetMessageAsync($invisibleTimeout,$null,$null)
$queueMessage.Result
$queueMessage = $queue.CloudQueue.GetMessageAsync($invisibleTimeout,$null,$null)
$queueMessage.Result

# After 10 seconds, these messages reappear on the queue.
# Read them again, but delete each one after reading it.
# Delete the message.
$queueMessage = $queue.CloudQueue.GetMessageAsync($invisibleTimeout,$null,$null)
$queueMessage.Result
$queue.CloudQueue.DeleteMessageAsync($queueMessage.Result.Id,$queueMessage.Result.popReceipt)
$queueMessage = $queue.CloudQueue.GetMessageAsync($invisibleTimeout,$null,$null)
$queueMessage.Result
$queue.CloudQueue.DeleteMessageAsync($queueMessage.Result.Id,$queueMessage.Result.popReceipt)
$queueMessage = $queue.CloudQueue.GetMessageAsync($invisibleTimeout,$null,$null)
$queueMessage.Result
$queue.CloudQueue.DeleteMessageAsync($queueMessage.Result.Id,$queueMessage.Result.popReceipt)
```

## <a name="delete-a-queue"></a>Bir kuyruk silme

Bir kuyruk ve içerdiği tüm iletileri silmek için Remove-AzStorageQueue cmdlet'ini çağırın. Aşağıdaki örnek Remove-AzStorageQueue cmdlet'ini kullanarak bu alıştırmada kullanılan belirli bir kuyruğa silme işlemini gösterir.

```powershell
# Delete the queue
Remove-AzStorageQueue –Name $queueName –Context $ctx
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu alıştırmada oluşturduğunuz varlıkların tümünü kaldırmak için kaynak grubunu kaldırın. Bu, ayrıca grubun içerdiği tüm kaynakları da siler. Bu durumda, oluşturduğunuz depolama hesabına ve kaynak grubunu kaldırır.

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Nasıl yapılır bu makalede PowerShell ile temel kuyruk Depolama Yönetimi hakkında bilgi öğrendiniz da dahil olmak üzere:

> [!div class="checklist"]
>
> * Kuyruk oluştur
> * Bir kuyruğa alma
> * Bir ileti ekleyin
> * Sonraki iletiyi okuyun
> * İleti silme
> * Bir kuyruk silme

### <a name="microsoft-azure-powershell-storage-cmdlets"></a>Microsoft Azure PowerShell depolama cmdlet'leri

* [Depolama PowerShell cmdlet’leri](/powershell/module/az.storage)

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Depolama Gezgini

* [Microsoft Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
