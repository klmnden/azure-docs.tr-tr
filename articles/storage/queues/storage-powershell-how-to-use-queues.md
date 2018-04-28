---
title: PowerShell ile Azure kuyruk depolama işlemleri | Microsoft Docs
description: PowerShell ile Azure kuyruk depolama işlemleri gerçekleştirme
services: storage
documentationcenter: storage
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 09/14/2017
ms.author: robinsh
ms.openlocfilehash: bad9f1f3fd5737e865a8f4d1d15ab3d5eb68b4cb
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="perform-azure-queue-storage-operations-with-azure-powershell"></a>Azure PowerShell ile Azure kuyruk depolama işlemleri

Azure Kuyruk depolama, HTTP veya HTTPS kullanan kimlik doğrulaması yapılmış çağrılar aracılığıyla dünyanın her yerinden erişilebilen çok sayıda iletinin depolanması için bir hizmettir. Ayrıntılı bilgi için bkz: [Azure kuyrukları giriş](storage-queues-introduction.md). Bu nasıl yapılır makalesi genel kuyruk depolama işlemlerini kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir kuyruk oluşturma
> * Bir Sıraya alma
> * Bir ileti ekleyin
> * Bir ileti okuma
> * İleti silme 
> * Bir kuyruk silme

Bu nasıl yapılır, Azure PowerShell modülü 3,6 veya sonraki bir sürümü gerektiriyor. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps).

Hiçbir veri düzlemi sıralar için PowerShell cmdlet'leri vardır. Bir ileti okuma veri gerçekleştirmek için düzlemi işlemleri gibi bir ileti eklemeniz ve ileti silme, PowerShell'de gösterilen .NET depolama istemci kitaplığı kullanmak zorunda. Bir ileti nesnesi oluşturun ve sonra ileti üzerinde işlem gerçekleştirmek için AddMessage gibi komutları kullanabilirsiniz. Bu makalede, bunun nasıl yapılacağını gösterir.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

`Connect-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzureRmAccount
```

## <a name="retrieve-list-of-locations"></a>Konumların listesini alma

Kullanmak istediğiniz konumdan emin değilseniz, kullanılabilir konumları listeleyebilirsiniz. Liste görüntülendikten sonra, kullanmak istediğiniz öğeyi bulun. Bu alıştırmada kullanacağı **eastus**. Bu değişken içerisinde nasıl depolanacağını **konumu** gelecekte kullanım için.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) komutu ile yeni bir kaynak grubu oluşturun. 

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Kaynak grubu adı bir değişken gelecekte kullanım için depolar. Bu örnekte, bir kaynak grubu adında *howtoqueuesrg* oluşturulan *eastus* bölge.

```powershell
$resourceGroup = "howtoqueuesrg"
New-AzureRmResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

## <a name="create-storage-account"></a>Depolama hesabı oluştur

Yerel olarak yedekli depolama (LRS) kullanarak standart genel amaçlı depolama hesabı oluşturma [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount). Kullanılacak depolama hesabını tanımlayan depolama hesabı bağlamını alır. Bir depolama hesabı üzerinde hareket ederken, tekrar tekrar kimlik bilgileri sağlama yerine bağlamı başvuru.

```powershell
$storageAccountName = "howtoqueuestorage"
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName Standard_LRS

$ctx = $storageAccount.Context
```

## <a name="create-a-queue"></a>Bir kuyruk oluşturma

Aşağıdaki örnek ilk Azure depolama hesabı adı ve erişim anahtarını içeren depolama hesabı bağlamını kullanarak depolama bağlantı kurar. Ardından, çağıran [yeni AzureStorageQueue](/powershell/module/azure.storage/new-azurestoragequeue) 'queuename' adında bir kuyruk oluşturmak için cmdlet'i.

```powershell
$queueName = "howtoqueue"
$queue = New-AzureStorageQueue –Name $queueName -Context $ctx
```

Azure kuyruk hizmeti için adlandırma kuralları hakkında bilgi için bkz: [adlandırma kuyrukları ve meta verileri](http://msdn.microsoft.com/library/azure/dd179349.aspx).

## <a name="retrieve-a-queue"></a>Bir Sıraya alma

Sorgulama ve belirli bir sıraya veya bir depolama hesabındaki tüm kuyrukların listesini alın. Aşağıdaki örnekler depolama hesabındaki tüm kuyruklar ve belirli bir kuyruğa alma gösterir; Her iki komutlarını kullanmak [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) cmdlet'i.

```powershell
# Retrieve a specific queue
$queue = Get-AzureStorageQueue –Name $queueName –Context $ctx
# Show the properties of the queue
$queue

# Retrieve all queues and show their names
Get-AzureStorageQueue -Context $ctx | select Name
```

## <a name="add-a-message-to-a-queue"></a>Kuyruğa bir ileti Ekle

Sıradaki gerçek iletileri etkileyen işlemleri .NET depolama istemci kitaplığı PowerShell'de gösterilen şekilde kullanın. Kuyruğa bir ileti eklemek için ileti nesnesinin yeni bir örneğini oluşturmak [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) sınıfı. Ardından [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) yöntemini çağırın. Bir CloudQueueMessage bir dizeden (UTF-8 biçiminde) veya bir bayt dizisi oluşturulabilir.

Aşağıdaki örnek, bir ileti kuyruğuna eklemek gösterilmiştir.

```powershell
# Create a new message using a constructor of the CloudQueueMessage class
$queueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage `
  -ArgumentList "This is message 1"
# Add a new message to the queue
$queue.CloudQueue.AddMessage($QueueMessage)

# Add two more messages to the queue 
$queueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage `
  -ArgumentList "This is message 2"
$queue.CloudQueue.AddMessage($QueueMessage)
$queueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage `
  -ArgumentList "This is message 3"
$queue.CloudQueue.AddMessage($QueueMessage)
```

Kullanırsanız [Azure Storage Gezgini](http://storageexplorer.com), Azure hesabınıza bağlanın ve depolama hesabında sıralarını görüntüleyin ve ayrıntıya aşağı sıra üzerinde iletilerini görüntülemek için bir sıra. 

## <a name="read-a-message-from-the-queue-then-delete-it"></a>Bir ileti kuyruktan okunur sonra silin

İleti en iyi deneme ilk olarak ilk çıkar sırayla salt okunurdur. Bu garanti edilmez. Sıradan iletiyi okurken sıraya arayan tüm işlemler için görünmez olur. Bu donanım veya yazılım hatası nedeniyle iletiyi işlemek kodunuzu başarısız olursa, kodunuzun başka bir örneği aynı ileti alma ve yeniden deneyin sağlar.  

Bu **görünmezlik zaman aşımı** ne kadar süreyle yeniden işleme için kullanılabilir olmadan önce ileti görünmez kalır tanımlar. Varsayılan değer 30 saniyedir. 

Kodunuzun bir iletiyi kuyruktan iki adımda okur. Çağırdığınızda [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) sırasındaki sonraki iletiyi elde yöntemi. **GetMessage**’dan dönen bir ileti bu kuyruktaki kod okuyan iletilere karşı görünmez olur. İletiyi kuyruktan kaldırmayı tamamlamak için arama [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) yöntemi. 

Aşağıdaki örnekte, üç kuyruk iletileri üzerinden okuyun sonra 10 saniye bekleyin (görünmezlik zaman aşımı). Bunları çağırarak okuduktan sonra iletilerini silerken yeniden üç iletileri okumak sonra **DeleteMessage**. İletiler silindikten sonra sıra okuma denerseniz, $queueMessage NULL olarak döner.

```powershell
# Set the amount of time you want to entry to be invisible after read from the queue
# If it is not deleted by the end of this time, it will show up in the queue again
$invisibleTimeout = [System.TimeSpan]::FromSeconds(10)

# Read the message from the queue, then show the contents of the message. Read the other two messages, too.
$ queueMessage = $queue.CloudQueue.GetMessage($invisibleTimeout)
$ queueMessage 
$ queueMessage = $queue.CloudQueue.GetMessage($invisibleTimeout)
$ queueMessage 
$ queueMessage = $queue.CloudQueue.GetMessage($invisibleTimeout)
$ queueMessage 

# After 10 seconds, these messages reappear on the queue. 
# Read them again, but delete each one after reading it.
# Delete the message.
$ queueMessage = $queue.CloudQueue.GetMessage($invisibleTimeout)
$ queue.CloudQueue.DeleteMessage($queueMessage)
$ queueMessage = $queue.CloudQueue.GetMessage($invisibleTimeout)
$ queue.CloudQueue.DeleteMessage($queueMessage)
$ queueMessage = $queue.CloudQueue.GetMessage($invisibleTimeout)
$ queue.CloudQueue.DeleteMessage($queueMessage)
```

## <a name="delete-a-queue"></a>Bir kuyruk silme
Bir kuyruk ve içerdiği tüm iletileri silmek için Remove-AzureStorageQueue cmdlet'ini çağırın. Aşağıdaki örnek Remove-AzureStorageQueue cmdlet'ini kullanarak bu alıştırmada kullanılan belirli bir kuyruğa silmek nasıl gösterir.

```powershell
# Delete the queue 
Remove-AzureStorageQueue –Name $queueName –Context $ctx
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu alıştırmada oluşturduğunuz varlıklar tümünün kaldırmak için kaynak grubunu kaldırın. Bu, ayrıca grubun içerdiği tüm kaynakları da siler. Bu durumda, oluşturulan depolama hesabı ve kaynak grubu kaldırır.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Nasıl yapılır bu makalede, temel kuyruk Depolama Yönetimi PowerShell ile ilgili öğrenilen nasıl dahil olmak üzere:

> [!div class="checklist"]
> * Bir kuyruk oluşturma
> * Bir Sıraya alma
> * Bir ileti ekleyin
> * Sonraki iletiyi okuma
> * İleti silme 
> * Bir kuyruk silme

### <a name="microsoft-azure-powershell-storage-cmdlets"></a>Microsoft Azure PowerShell depolama cmdlet'leri
* [Depolama PowerShell cmdlet’leri](/powershell/module/azurerm.storage#storage)

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Depolama Gezgini
* [Microsoft Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
