---
title: Azure depolama istemcisi için güvenli TLS etkinleştirme | Microsoft Docs
description: Azure Depolama'nın istemcide TLS 1.2 etkinleştirmeyi öğrenin.
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
ms.openlocfilehash: 6c313b6015a8a6dcc4ca5befb5fef70b047d0410
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37866534"
---
# <a name="enable-secure-tls-for-azure-storage-client"></a>Azure Depolama istemcisi için güvenli TLS'yi etkinleştirme

Denetim gerektiğinde hizmetlerinizi Azure depolama kullanarak en son uyumluluk ve güvenlik gereksinimleri, SSL 1.0 göre 2.0, 3.0 ve TLS 1.0 uyumlu olmayan bir iletişim protokolü tanınır.

SSL 1.0, 2.0 ve 3.0, savunmasız bulundu. Bunlar RFC tarafından yasaklanmış. TLS 1.0 güvenli blok şifreleme (DES CBC ve RC2 CBC) ve Stream şifre (RC4) kullanmak için güvenli hale gelir. PCI council geçiş daha yüksek TLS sürümlerini de önerilen. Daha fazla bilgi için gördüğünüz [Aktarım Katmanı Güvenliği (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0).

Azure depolama, SSL 3.0 2015 tarihinden itibaren durdurdu ve TLS 1.2 genel HTTPs Uç noktalara kullanır ancak yine de TLS 1.0 ve TLS 1.1 geriye dönük uyumluluk için desteklenir.

Güvenli ve uyumlu Azure depolama bağlantı sağlamak için Azure depolama hizmeti çalıştırmak için istekler göndermeden önce istemci tarafı TLS 1.2 etkinleştirmeniz gerekir.

## <a name="enable-tls-12-in-net-client"></a>TLS 1.2 .NET istemci etkinleştirme

TLS 1.2, işletim sistemi ve .NET Framework sürümünü anlaşmak için istemciyi hem TLS 1.2 desteklemesi gerekir. Daha fazla bilgi [TLS 1.2 desteği](https://docs.microsoft.com/en-us/dotnet/framework/network-programming/tls#support-for-tls-12).

Aşağıdaki örnek, TLS 1.2 .NET istemcinizde etkinleştirme işlemi gösterilmektedir.

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

## <a name="enable-tls-12-in-powershell-client"></a>TLS 1.2 PowerShell istemcisi etkinleştir

Aşağıdaki örnek, TLS 1.2 PowerShell istemcinizde etkinleştirme işlemi gösterilmektedir.

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

TLS 1.2 gerçekten kullanılıp kullanılmadığını doğrulamak için Fiddler'ı kullanabilirsiniz. İstemci ağ trafiği yakalamayı başlatmak için Fiddler'ı açın, ardından örnek yürütün. Ardından örnek yaptığı bağlantı TLS sürümü bulabilirsiniz.

Aşağıdaki ekran görüntüsünde, doğrulama için bir örnektir.

![TLS sürümü fiddler'da doğrulama işleminin ekran görüntüsü](./media/storage-security-tls/storage-security-tls-verify-in-fiddler.png)

## <a name="see-also"></a>Ayrıca bkz.

* [Aktarım Katmanı Güvenliği (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0)
* [Java istemci TLS etkinleştir](https://www.java.com/en/configure_crypto.html)
