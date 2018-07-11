---
title: 'Azure Hızlı Başlangıç: PowerShell kullanarak olay akışlarını işleme | Microsoft Docs'
description: Bu hızlı başlangıçta PowerShell ve basit bir .NET uygulaması kullanarak Azure Event Hubs olaylarını gönderme ve alma adımları açıklanmaktadır.
services: event-hubs
author: sethmanheim
manager: timlt
editor: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.date: 06/26/2018
ms.author: sethm
ms.openlocfilehash: 9216372038db7a6f97cfc8034f715b34de08d83c
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37132445"
---
# <a name="quickstart-process-event-streams-using-powershell-and-net-standard"></a>Hızlı başlangıç: PowerShell ve .NET Standard kullanarak olay akışlarını işleme

Azure Event Hubs saniyede milyonlarca olay alıp işleyebilen, ölçeklenebilirlik yüzeyi yüksek bir veri akışı platformu ve veri alma hizmetidir. Bu hızlı başlangıçta Azure PowerShell kullanarak olay hub'ı oluşturma ve .NET Standard SDK'sını kullanarak olay hub'ıyla ileti alışverişi yapma adımları gösterilmektedir.

Bu hızlı başlangıcı tamamlamak bir Azure aboneliğinizin olması gerekir. Aboneliğiniz yoksa başlamadan önce [Ücretsiz hesap oluşturun][] oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için şunlar sahip olduğunuzdan emin olun:

- [Visual Studio 2017 Güncelleştirme 3 (sürüm 15.3, 26730.01)](http://www.visualstudio.com/vs) veya sonraki sürümler.
- [.NET Standard SDK'sı](https://www.microsoft.com/net/download/windows), sürüm 2.0 veya üzeri.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel ortamda kullanıyorsanız bu hızlı başlangıcı tamamlamak için PowerShell'in en son sürümüne sahip olmanız gerekir. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell'i Yükleme ve Yapılandırma](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps?view=azurermps-5.7.0).

## <a name="provision-resources"></a>Kaynak sağlama

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynaklarından oluşan mantıksal bir koleksiyondur ve olay hub'ı oluşturmak için bir kaynak grubuna ihtiyacınız vardır. 

Aşağıdaki örnekte Doğu ABD bölgesinde bir kaynak grubu oluşturulmaktadır. `myResourceGroup` değerini kullanmak istediğiniz kaynak grubu adıyla değiştirin:

```azurepowershell-interactive
New-AzureRmResourceGroup –Name myResourceGroup –Location eastus
```

### <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma

Kaynak grubunuzu oluşturduktan sonra bu kaynak grubu içinde bir Event Hubs ad alanı oluşturun. Event Hubs ad alanı, içinde olay hub'ınızı oluşturabileceğiniz benzersiz bir tam etki alanı adı sağlar. `namespace_name` değerini ad alanınız için benzersiz bir adla değiştirin:

```azurepowershell-interactive
New-AzureRmEventHubNamespace -ResourceGroupName myResourceGroup -NamespaceName namespace_name -Location eastus
```

### <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

Bir Event Hubs ad alanı oluşturduğunuza göre şimdi bu ad alanının içinde bir olay hub'ı oluşturabilirsiniz:

```azurepowershell-interactive
New-AzureRmEventHub -ResourceGroupName myResourceGroup -NamespaceName namespace_name -EventHubName eventhub_name
```

### <a name="create-a-storage-account-for-event-processor-host"></a>Olay İşleyicisi Ana Bilgisayarı için bir depolama hesabı oluşturma

Olay İşleyicisi Ana Bilgisayarı, olay hub’larına ait denetim noktalarını ve paralel alıcıları yöneterek bu olay hub’larından olay almayı basitleştirir. Olay İşleyicisi Ana Bilgisayarı, denetim noktası için bir depolama hesabına ihtiyaç duyar. Bir depolama hesabı oluşturmak anahtarlarını almak için aşağıdaki komutları çalıştırın:

```azurepowershell-interactive
# Create a standard general purpose storage account 
New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name storage_account_name -Location eastus -SkuName Standard_LRS 
e
# Retrieve the storage account key for accessing it
Get-AzureRmStorageAccountKey -ResourceGroupName myResourceGroup -Name storage_account_name
```

### <a name="get-the-connection-string"></a>Bağlantı dizesini alma

Olay hub'ınıza bağlanmak ve olayları işlemek için bir bağlantı dizesi gerekir. Bağlantı dizenizi almak için şu komutu çalıştırın:

```azurepowershell-interactive
Get-AzureRmEventHubKey -ResourceGroupName myResourceGroup -NamespaceName namespace_name -Name RootManageSharedAccessKey
```

## <a name="stream-into-event-hubs"></a>Event Hubs'a akış sağlama

Artık Event Hubs'a akış başlatabilirsiniz. Bu örnekleri [Event Hubs deposundan](https://github.com/Azure/azure-event-hubs) indirebilir veya Git üzerinden kopyalayabilirsiniz

### <a name="ingest-events"></a>Olayları alma

Olay akışını başlatmak için GitHub'dan [SampleSender](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) dosyasını indirin veya aşağıdaki komutu kullanarak [Event Hubs GitHub deposunu](https://github.com/Azure/azure-event-hubs) kopyalayın:

```bash
git clone https://github.com/Azure/azure-event-hubs.git
```

\azure-event-hubs\samples\DotNet\Microsoft.Azure.EventHubs\SampleSender klasörüne gidin ve SampleSender.sln dosyasını Visual Studio'ya yükleyin.

Ardından [Microsoft.Azure.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) Nuget paketini projeye ekleyin.

Program.cs dosyasında aşağıdaki yer tutucuları olay hub'ınızın adı ve bağlantı dizenizle değiştirin:

```C#
private const string EhConnectionString = "Event Hubs connection string";
private const string EhEntityPath = "Event Hub name";

```

Şimdi örneği derleyin ve çalıştırın. Olayların, olay hub'ınıza alındığını görebilirsiniz:

![][3]

### <a name="receive-and-process-events"></a>Olayları alma ve işleme

Şimdi, az önce gönderdiğiniz iletileri alan Olay İşlemcisi Ana Bilgisayarı alıcı örneğini indirin. GitHub'dan [SampleEphReceiver](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) dosyasını indirin veya aşağıdaki komutu kullanarak [Event Hubs GitHub deposunu](https://github.com/Azure/azure-event-hubs) kopyalayın:

```bash
git clone https://github.com/Azure/azure-event-hubs.git
```

\azure-event-hubs\samples\DotNet\Microsoft.Azure.EventHubs\SampleEphReceiver klasörüne gidin ve SampleEphReceiver.sln dosyasını Visual Studio'ya yükleyin.

Ardından [Microsoft.Azure.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) ve [Microsoft.Azure.EventHubs.Processor](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) Nuget paketlerini projenize ekleyin.

Program.cs dosyasında aşağıdaki sabitleri ilgili değerlerle değiştirin:

```C#
private const string EventHubConnectionString = "Event Hubs connection string";
private const string EventHubName = "Event Hub name";
private const string StorageContainerName = "Storage account container name";
private const string StorageAccountName = "Storage account name";
private const string StorageAccountKey = "Storage account key";
```

Şimdi örneği derleyin ve çalıştırın. Olayların örnek uygulamanıza geldiğini görebilirsiniz:

![][4]

Azure portalda belirli bir Event Hubs ad alanı için olayların işleme hızını aşağıdaki şekilde görüntüleyebilirsiniz:

![][5]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıcı tamamladıktan sonra kaynak grubunuzu ve içindeki ad alanını, depolama hesabını ve olay hub'ını silebilirsiniz. `myResourceGroup` değerini oluşturduğunuz kaynak grubunun adıyla değiştirin. 

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Event Hubs ad alanını ve olay hub'ınızdan olay gönderip almak için gereken diğer kaynakları oluşturdunuz. Daha fazla bilgi edinmek için aşağıdaki öğreticiyle devam edin:

> [!div class="nextstepaction"]
> [Event Hubs veri akışları üzerindeki veri anormalliklerini görselleştirme](event-hubs-tutorial-visualize-anomalies.md)

[Ücretsiz hesap oluşturun]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[Install and Configure Azure PowerShell]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[New-AzureRmResourceGroup]: https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup
[fully qualified domain name]: https://wikipedia.org/wiki/Fully_qualified_domain_name
[3]: ./media/event-hubs-quickstart-powershell/sender1.png
[4]: ./media/event-hubs-quickstart-powershell/receiver1.png
[5]: ./media/event-hubs-quickstart-powershell/metrics.png
