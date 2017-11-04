---
title: "Azure Storage için bir bağlantı dizesi yapılandırma | Microsoft Docs"
description: "Bir Azure depolama hesabı için bir bağlantı dizesi yapılandırın. Bir bağlantı dizesi çalışma zamanında uygulamanızdan bir depolama hesabına erişimi kimlik doğrulaması yapmak için gereken bilgileri içerir."
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: ecb0acb5-90a9-4eb2-93e6-e9860eda5e53
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: tamram
ms.openlocfilehash: 192799cb44dc9a56c65a6414c1267c506252fe29
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-azure-storage-connection-strings"></a>Azure Storage bağlantı dizelerini yapılandırma

Bir bağlantı dizesi uygulamanız çalışma zamanında bir Azure depolama hesabındaki verilere erişmek için gereken kimlik doğrulama bilgileri içerir. Bağlantı dizeleri için yapılandırabilirsiniz:

* Azure storage öykünücüsü bağlayın.
* Azure depolama hesabı erişim.
* Paylaşılan erişim imzası (SAS) aracılığıyla Azure içinde belirtilen kaynaklara erişir.

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a>Bağlantı dizenizi depolanması
Azure depolama alanına yapılan istekleri kimlik doğrulaması için çalışma zamanında bağlantı dizesi erişmek uygulamanız gerekir. Bağlantı dizenizi depolamak için birkaç seçeneğiniz vardır:

* Masaüstünde veya bir cihazdaki uygulama çalıştıran bir bağlantı dizesinde depolayabilir bir **app.config** veya **web.config** dosya. Bağlantı dizesine eklemek **AppSettings** bu dosyaları bölümünde.
* Bir Azure bulut hizmetindeki çalışan bir uygulamaya bağlantı dizesinde depolayabilir [Azure hizmet yapılandırma (.cscfg) şema dosyası](https://msdn.microsoft.com/library/ee758710.aspx). Bağlantı dizesine eklemek **ConfigurationSettings** hizmet yapılandırma dosyasının.
* Bağlantı dizenizi doğrudan kodunuzda kullanabilirsiniz. Ancak, çoğu senaryoda yapılandırma dosyasında bağlantı dizenizi depolamanız önerilir.

Bağlantı dizenizi bir yapılandırma dosyasında depolamak depolama öykünücüsünü ve buluttaki bir Azure depolama hesabını arasında geçiş yapmak için bağlantı dizesini güncellemeniz kolaylaştırır. Yalnızca hedef ortamınızı noktası için bağlantı dizesi düzenlemeniz gerekir.

Kullanabileceğiniz [Microsoft Azure Yapılandırma Yöneticisi](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) , uygulamanızın nerede çalıştığına bakmaksızın çalışma zamanında bağlantı dizenizi erişmek için.

## <a name="create-a-connection-string-for-the-storage-emulator"></a>Bir bağlantı dizesi oluşturmak için depolama öykünücüsü
[!INCLUDE [storage-emulator-connection-string-include](../../../includes/storage-emulator-connection-string-include.md)]

Depolama öykünücüsü hakkında daha fazla bilgi için bkz: [geliştirme ve sınama için Azure storage öykünücüsünü kullanma](storage-use-emulator.md).

## <a name="create-a-connection-string-for-an-azure-storage-account"></a>Bir Azure depolama hesabı için bir bağlantı dizesi oluşturma
Azure depolama hesabınız için bir bağlantı dizesi oluşturmak için aşağıdaki biçimi kullanın. (Önerilen) HTTPS üzerinden depolama hesabı bağlanmak istediğiniz veya HTTP belirtmek, yerine `myAccountName` depolama hesabı ve Değiştir adıyla `myAccountKey` hesabının erişim anahtarı ile:

`DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey`

Örneğin, bağlantı dizenizi benzer şekilde görünebilir:

`DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>`

Azure Storage HTTP ve HTTPS bağlantı dizesinde desteklemesine rağmen *HTTPS tavsiye*.

> [!TIP]
> Depolama hesabınızın bağlantı dizeleri bulabilirsiniz [Azure portal](https://portal.azure.com). Gidin **ayarları** > **erişim anahtarları** bağlantı dizeleri için hem birincil ve ikincil erişim tuşlarını görmek için depolama hesabınızın menü dikey penceresinde.
>

## <a name="create-a-connection-string-using-a-shared-access-signature"></a>Paylaşılan erişim imzası kullanarak bir bağlantı dizesi oluşturma
[!INCLUDE [storage-use-sas-in-connection-string-include](../../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="create-a-connection-string-for-an-explicit-storage-endpoint"></a>Bir açık depolama uç nokta için bir bağlantı dizesi oluşturma
Varsayılan uç noktalar kullanmak yerine, bağlantı dizesinde açık hizmet uç noktaları belirtin. Açık bir uç nokta belirten bir bağlantı dizesi oluşturmak için aşağıdaki biçimde protokolü belirtimi (HTTPS (önerilen) veya HTTP) dahil olmak üzere her hizmet için tam Hizmeti uç noktası belirtin:

```
DefaultEndpointsProtocol=[http|https];
BlobEndpoint=myBlobEndpoint;
FileEndpoint=myFileEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
AccountName=myAccountName;
AccountKey=myAccountKey
```

Blob storage uç noktanız eşlenen nerede istediğiniz açık bir uç nokta belirtmek için bir senaryo olduğunda bir [özel etki alanı](../blobs/storage-custom-domain-name.md). Bu durumda, bağlantı dizenizi Blob storage için özel uç noktanızı belirtebilirsiniz. Uygulamanız bunları kullanıyorsa, isteğe bağlı olarak diğer hizmetler için varsayılan uç noktalar belirtebilirsiniz.

Burada, Blob hizmeti için açık bir uç nokta belirten bir bağlantı dizesi örneği verilmiştir:

```
# Blob endpoint only
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
AccountName=storagesample;
AccountKey=<account-key>
```

Bu örnek Blob hizmeti için özel bir etki alanı dahil olmak üzere tüm hizmetleri için açık uç nokta belirtir:

```
# All service endpoints
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
FileEndpoint=https://myaccount.file.core.windows.net;
QueueEndpoint=https://myaccount.queue.core.windows.net;
TableEndpoint=https://myaccount.table.core.windows.net;
AccountName=storagesample;
AccountKey=<account-key>
```

Bir bağlantı dizesi uç nokta değerleri depolama hizmetleri için URI isteği oluşturun ve kodunuzu döndürülen URI'ler form dikte için kullanılır.

Bir depolama uç noktası için özel bir etki alanı eşlenen ve bu uç bağlantı dizesinden atlarsanız, daha sonra kodunuzdan bu hizmetindeki verilere erişmek için bağlantı dizesini kullanmanız mümkün olmaz.

> [!IMPORTANT]
> Hizmet uç noktası değerleri bağlantı dizelerinizi doğru biçimlendirilmiş olmalıdır URI'ler dahil olmak üzere, `https://` (önerilen) veya `http://`. Azure Storage henüz HTTPS özel etki alanları için desteklemediğinden, *gerekir* belirtin `http://` için herhangi bir uç nokta için özel bir etki alanı gösteren URI.
>

### <a name="create-a-connection-string-with-an-endpoint-suffix"></a>Bir uç nokta soneki ile bir bağlantı dizesi oluşturma
Azure Çin veya Azure kamu gibi bölgelerde veya farklı uç nokta sonekleri örnekleriyle depolama hizmeti için bir bağlantı dizesi oluşturmak için aşağıdaki bağlantı dizesi biçimi kullanın. (Önerilen) HTTPS üzerinden depolama hesabı bağlanmak istediğiniz veya HTTP belirtmek, yerine `myAccountName` depolama hesabınızın adıyla değiştirin `myAccountKey` hesap erişim tuşu ve Değiştir ile `mySuffix` URI soneki:

```
DefaultEndpointsProtocol=[http|https];
AccountName=myAccountName;
AccountKey=myAccountKey;
EndpointSuffix=mySuffix;
```

Bir örnek bağlantı dizesi Azure Çin'de depolama hizmetleri aşağıdadır:

```
DefaultEndpointsProtocol=https;
AccountName=storagesample;
AccountKey=<account-key>;
EndpointSuffix=core.chinacloudapi.cn;
```

## <a name="parsing-a-connection-string"></a>Bir bağlantı dizesini ayrıştırma
[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [Geliştirme ve sınama için Azure storage öykünücüsünü kullanma](storage-use-emulator.md)
* [Azure depolama gezginleri](storage-explorers.md)
* [Paylaşılan erişim imzaları (SAS) kullanma](storage-dotnet-shared-access-signature-part-1.md)

