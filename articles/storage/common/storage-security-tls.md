---
title: Azure Storage istemcisi için güvenli TLS etkinleştirmek | Microsoft Docs
description: Azure Storage istemcisinde TLS 1.2 etkinleştirmeyi öğrenin.
services: storage
documentationcenter: na
author: fhryo-msft
manager: cbrooks
editor: fhryo-msft
ms.assetid: ''
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 06/25/2018
ms.author: fryu
ms.openlocfilehash: 5c21df2b3bdeee6ac7c3956fe1cafa4f947dd6dd
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036437"
---
# <a name="enable-secure-tls-for-azure-storage-client"></a>Azure Storage istemcisi için güvenli TLS etkinleştir

Denetim gerektiğinde hizmetlerinizi Azure Storage kullanarak en son uyumluluk ve güvenlik gereksinimleri, SSL 1.0 göre 2.0, 3.0 ve TLS 1.0 uyumsuz iletişim protokolü tanınır.

SSL 1.0, 2.0 ve 3.0, savunmasız bulundu. Bunlar RFC tarafından yasaklanmış. TLS 1.0 güvensiz blok şifreleme (DES CBC ve RC2 CBC) ve akış şifre (RC4) kullanarak güvenli hale gelir. PCI council de geçiş daha yüksek TLS sürümleri için önerilen. Daha fazla ayrıntı için gördüğünüz [Aktarım Katmanı Güvenliği (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0).

Azure depolama 2015 tarihinden itibaren SSL 3.0 durdurdu ve TLS 1.2 genel HTTPs Uç noktalara kullanır ancak TLS 1.0 ve TLS 1.1 geriye dönük uyumluluk için hala desteklenmektedir.

Azure Storage güvenli ve uyumlu bağlantı sağlamak için Azure depolama hizmeti çalıştırmak için istek göndermeden önce istemci tarafı TLS 1.2 etkinleştirmeniz gerekir.

## <a name="enable-tls-12-in-net-client"></a>TLS 1.2 .NET istemcisinde etkinleştir

İstemcinin TLS 1.2, işletim sistemi ve .NET Framework sürümünü anlaşmak için her ikisini de TLS 1.2 desteklemesi gerekir. İçinde daha fazla ayrıntı görmek [TLS 1.2 desteği](https://docs.microsoft.com/en-us/dotnet/framework/network-programming/tls#support-for-tls-12).

Aşağıdaki örnek, TLS 1.2 .NET istemciniz etkinleştirmek gösterilmiştir.

```csharp

    static void EnableTls12()
    {
        // Enable TLS 1.2 before connecting to Azure Storage
        System.Net.ServicePointManager.SecurityProtocol = System.Net.SecurityProtocolType.Tls12;

        // Connect to Azure Storage
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName={yourstorageaccount};AccountKey={yourstorageaccountkey};EndpointSuffix=core.windows.net");
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        CloudBlobContainer container = blobClient.GetContainerReference("foo");
        container.CreateIfNotExists();
    }

```

## <a name="enable-tls-12-in-powershell-client"></a>TLS 1.2 PowerShell istemcisinde etkinleştir

Aşağıdaki örnek, TLS 1.2 PowerShell istemciniz etkinleştirmek gösterilmiştir.

```powershell

# Enable TLS 1.2 before connecting to Azure Storage
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12;

$resourceGroup = "{YourResourceGropuName}"
$storageAccountName = "{YourStorageAccountNme}"
$prefix = "foo"

# Connect to Azure Storage
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroup -Name $storageAccountName
$ctx = $storageAccount.Context
$listOfContainers = Get-AzureStorageContainer -Context $ctx -Prefix $prefix
$listOfContainers

```

## <a name="verify-tls-12-connection"></a>TLS 1.2 bağlantısını doğrulama

TLS 1.2 gerçekte kullanılıp kullanılmayacağını doğrulamak için fiddler'ı kullanabilirsiniz. İstemci ağ trafiğini yakalama başlatmak için fiddler'ı açın, ardından örnek yürütün. Ardından örnek yaptığı bağlantı TLS sürümü bulabilirsiniz.

Aşağıdaki ekran görüntüsünde, doğrulama için bir örnektir.

![TLS sürüm fiddler'da doğrulama işleminin ekran görüntüsü](./media/storage-security-tls/storage-security-tls-verify-in-fiddler.png)

## <a name="see-also"></a>Ayrıca bkz.

* [Aktarım Katmanı Güvenliği (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0)
