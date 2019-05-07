---
title: Azure depolama için bir bağlantı dizesi yapılandırma | Microsoft Docs
description: Azure depolama hesabınız için bir bağlantı dizesi yapılandırın. Bir bağlantı dizesi, uygulamanızdan çalışma zamanında bir depolama hesabına erişim yetkisi vermek için gereken bilgileri içerir.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/12/2017
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 7029f07b494630cc1ebe4a2dbfb297e73d85ec5e
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65153183"
---
# <a name="configure-azure-storage-connection-strings"></a>Azure Storage bağlantı dizelerini yapılandırma

Bir bağlantı dizesi, uygulamanız çalışma zamanında bir Azure depolama hesabındaki verilere erişmek için gereken kimlik doğrulama bilgileri içerir. Bağlantı dizeleri için yapılandırabilirsiniz:

* Azure storage öykünücüsü için bağlantı.
* Bir Azure depolama hesabına erişim.
* Paylaşılan erişim imzası (SAS) aracılığıyla azure'da belirtilen kaynaklara erişin.

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a>Bağlantı dizesini depolama
Çalışma zamanında Azure depolama için yapılan istekleri yetkilendirmek için bağlantı dizesi erişmek uygulamanız gerekir. Bağlantı dizenizi depolamak için birkaç seçeneğiniz vardır:

* Masaüstü veya bir cihazda uygulama çalıştıran bir bağlantı dizesinde depolayabilirsiniz bir **app.config** veya **web.config** dosya. Bağlantı dizesi Ekle **AppSettings** bölümünde bu dosyalar.
* Bir Azure bulut hizmetinde çalışan bir uygulamaya bağlantı dizesinde depolayabilirsiniz [Azure hizmet yapılandırma şeması (.cscfg) dosyası](https://msdn.microsoft.com/library/ee758710.aspx). Bağlantı dizesi Ekle **ConfigurationSettings** hizmet yapılandırma dosyasının.
* Kodunuzda doğrudan bağlantı dizenizi kullanabilirsiniz. Ancak, çoğu senaryoda bir yapılandırma dosyasındaki bağlantı dizenizi depolamanızı öneririz.

Bağlantı dizenizi bir yapılandırma dosyasında depolamanız depolama öykünücüsü ve buluttaki bir Azure depolama hesabı arasında geçiş yapmak için bağlantı dizesini güncellemek kolaylaştırır. Yalnızca, bağlantı dizesi için hedef ortamınız işaret edecek şekilde düzenlemeniz gerekebilir.

Kullanabileceğiniz [Microsoft Azure Yapılandırma Yöneticisi](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) , uygulamanızın nerede çalıştığına bakmaksızın zamanında bağlantı dizenizi erişmek için.

## <a name="create-a-connection-string-for-the-storage-emulator"></a>Depolama öykünücüsü için bağlantı dizesi oluşturma
[!INCLUDE [storage-emulator-connection-string-include](../../../includes/storage-emulator-connection-string-include.md)]

Depolama öykünücüsü hakkında daha fazla bilgi için bkz: [geliştirme ve test için Azure depolama öykünücüsü kullanma](storage-use-emulator.md).

## <a name="create-a-connection-string-for-an-azure-storage-account"></a>Azure depolama hesabınız için bir bağlantı dizesi oluşturma
Azure depolama hesabınız için bir bağlantı dizesi oluşturmak için aşağıdaki biçimi kullanın. HTTP veya HTTPS (önerilen) üzerinden depolama hesabına bağlanmak istediğiniz belirtmek, yerine `myAccountName` değiştirme ve depolama hesabı adı ile `myAccountKey` hesabınızın erişim anahtarıyla:

`DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey`

Örneğin, bağlantı dizenizi şuna benzeyebilir:

`DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>`

Azure depolama bağlantı dizesinde, hem HTTP hem de HTTPS desteklese *HTTPS kesinlikle önerilir*.

> [!TIP]
> Depolama hesabınıza ait bağlantı dizeleri bulabilirsiniz [Azure portalında](https://portal.azure.com). Gidin **ayarları** > **erişim anahtarları** bağlantı dizeleri için her iki birincil ve ikincil erişim tuşlarını görmek için depolama hesabınızın menü dikey penceresinde.
>

## <a name="create-a-connection-string-using-a-shared-access-signature"></a>Bir bağlantı dizesi kullanarak bir paylaşılan erişim imzası oluşturma
[!INCLUDE [storage-use-sas-in-connection-string-include](../../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="create-a-connection-string-for-an-explicit-storage-endpoint"></a>Bir açık depolama uç noktası için bir bağlantı dizesi oluşturma
Varsayılan uç noktalarını kullanmak yerine, bağlantı dizesinde açık bir hizmet uç noktalarını belirtebilirsiniz. Açık bir bitiş noktası belirten bir bağlantı dizesi oluşturmak için şu biçimde protokolü belirtimi (HTTPS (önerilen) veya HTTP) dahil olmak üzere, her hizmet için tam hizmet uç noktası belirtin:

```
DefaultEndpointsProtocol=[http|https];
BlobEndpoint=myBlobEndpoint;
FileEndpoint=myFileEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
AccountName=myAccountName;
AccountKey=myAccountKey
```

Blob Depolama uç noktanız eşleştirdik istediğiniz açık bir bitiş noktası belirtin. bir senaryo olduğunda bir [özel etki alanı](../blobs/storage-custom-domain-name.md). Bu durumda, bağlantı dizenizi Blob Depolama için özel uç noktanıza belirtebilirsiniz. Uygulamanız bunları kullanıyorsa, isteğe bağlı olarak diğer hizmetler için varsayılan uç noktalarını belirtebilirsiniz.

Burada, Blob hizmeti için açık bir bitiş noktası belirten bir bağlantı dizesi örneği verilmiştir:

```
# Blob endpoint only
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
AccountName=storagesample;
AccountKey=<account-key>
```

Bu örnek, Blob hizmeti için özel bir etki alanı da dahil olmak üzere tüm hizmetler için açık uç noktalarını belirtir:

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

Bir bağlantı dizesi uç nokta değerleri URI'leri bir depolama hizmetlerine isteği oluşturmak ve kodunuzu iade herhangi bir URI'leri biçiminde dikte için kullanılır.

Depolama uç noktası için özel bir etki alanı eşleştirdik ve bağlantı dizesinden uç noktanın atlarsanız, daha sonra kodunuzdan hizmet verilerine erişim için bu bağlantı dizesini kullanmak mümkün olmayacaktır.

> [!IMPORTANT]
> Hizmet uç noktası, bağlantı dizeleri değerlerde iyi biçimlendirilmiş olmalıdır içeren URI'leri, `https://` (önerilir) veya `http://`. HTTPS özel etki alanları için ' ı desteklemediği henüz Azure depolama için *gerekir* belirtin `http://` tüm uç noktasının özel bir etki alanını işaret eden bir URI.
>

### <a name="create-a-connection-string-with-an-endpoint-suffix"></a>Bir uç nokta son eki ile bir bağlantı dizesi oluşturma
Azure Çin veya Azure devlet kurumları, olduğu gibi bir depolama hizmetine bölgelerde veya farklı bir uç nokta sonekleri ile örnekleri için bir bağlantı dizesi oluşturmak için aşağıdaki bağlantı dizesi biçimi kullanın. HTTP veya HTTPS (önerilen) üzerinden depolama hesabına bağlanmak istediğiniz belirtmek, yerine `myAccountName` depolama hesabınızın adıyla değiştirin `myAccountKey` hesabı erişim anahtarı ve Değiştir `mySuffix` URI soneki:

```
DefaultEndpointsProtocol=[http|https];
AccountName=myAccountName;
AccountKey=myAccountKey;
EndpointSuffix=mySuffix;
```

Azure Çin'de depolama hizmetleri için örnek bir bağlantı dizesi şu şekildedir:

```
DefaultEndpointsProtocol=https;
AccountName=storagesample;
AccountKey=<account-key>;
EndpointSuffix=core.chinacloudapi.cn;
```

## <a name="parsing-a-connection-string"></a>Bağlantı dizesini ayrıştırma
[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [Geliştirme ve test için Azure depolama öykünücüsü kullanma](storage-use-emulator.md)
* [Azure depolama gezginleri](storage-explorers.md)
* [Paylaşılan erişim imzaları (SAS) kullanma](storage-dotnet-shared-access-signature-part-1.md)

