---
title: DNS bölgeleri oluşturma ve Azure .NET SDK kullanarak DNS kayıt kümeleri | Microsoft Docs
description: .NET SDK kullanarak Azure DNS'te DNS bölgelerini ve kayıt kümelerini oluşturmak nasıl.
services: dns
documentationcenter: na
author: vhorne
manager: jeconnoc
ms.assetid: eed99b87-f4d4-4fbf-a926-263f7e30b884
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2016
ms.author: victorh
ms.openlocfilehash: a06d629087e853c2578e6d35a2ea90c5a8eff840
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60308952"
---
# <a name="create-dns-zones-and-record-sets-using-the-net-sdk"></a>DNS bölgelerini ve kayıt kümelerini .NET SDK kullanarak oluşturma

Oluşturma, silme veya DNS Yönetim .NET kitaplığı ile DNS SDK'sını kullanarak DNS bölgelerini, kayıt kümeleri ve kayıtları güncelleştirmek için işlemleri otomatik hale getirebilirsiniz. Tam Visual Studio projesi kullanılabilir [burada.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)

## <a name="create-a-service-principal-account"></a>Bir hizmet sorumlusu hesabı oluşturun

Genellikle, kendi kullanıcı kimlik bilgileri yerine özel bir hesap Azure kaynaklarına programlı erişim izni verilir. Bu ayrılmış hesaplarla 'hizmet sorumlusu' hesaplar olarak adlandırılır. Azure DNS SDK'sı kodunuzla kullanmak için öncelikle bir hizmet sorumlusu hesabı oluşturun ve doğru izinleri atamak gerekir.

1. İzleyin [bu yönergeleri](../active-directory/develop/howto-authenticate-service-principal-powershell.md) (Azure DNS SDK'sı kodunuzla varsayar parola tabanlı kimlik doğrulama.) bir hizmet sorumlusu hesabını oluşturma
2. Bir kaynak grubu oluşturun ([burada nasıl](../azure-resource-manager/resource-group-template-deploy-portal.md)).
3. Azure RBAC hizmet asıl hesabı kaynak grubu 'DNS bölgesi katkıda bulunanı' izinleri vermek için kullanın ([burada nasıl](../role-based-access-control/role-assignments-portal.md).)
4. Azure DNS SDK'sı örnek proje kullanıyorsanız, 'program.cs' dosyası şu şekilde düzenleyin:

   * Eklemek için doğru değerleri `tenantId`, `clientId` (diğer adıyla hesap kimliği), `secret` (hizmet sorumlusu hesabı parola) ve `subscriptionId` 1. adımda kullanılır.
   * 2. adımda seçilen kaynak grubu adı girin.
   * Tercih ettiğiniz bir DNS bölge adı girin.

## <a name="nuget-packages-and-namespace-declarations"></a>NuGet paketleri ve ad alanı bildirimi

Azure DNS .NET SDK'sını kullanmak için yüklemeniz gerekir **Azure DNS Yönetimi Kitaplığı** NuGet paketi ve diğer gerekli Azure paketleri.

1. İçinde **Visual Studio**, bir proje veya yeni bir proje açın.
2. Git **Araçları** **>** **NuGet Paket Yöneticisi** **>** **için NuGet paketlerini Yönet Çözüm Ekle...** .
3. Tıklayın **Gözat**, etkinleştirme **INCLUDE yayın öncesi** onay kutusunu ve türü **Microsoft.Azure.Management.Dns** kartındaki arama kutusuna.
4. Paketi seçin ve tıklayın **yükleme** Visual Studio projenize eklemek için.
5. Ayrıca aşağıdaki paketleri yüklemek için yukarıdaki işlemi tekrarlayın: **Microsoft.Rest.ClientRuntime.Azure.Authentication** ve **Microsoft.Azure.Management.ResourceManager**.

## <a name="add-namespace-declarations"></a>Ad alanı bildirimleri ekleme

Aşağıdaki ad alanı bildirimleri ekleme

```cs
using Microsoft.Rest.Azure.Authentication;
using Microsoft.Azure.Management.Dns;
using Microsoft.Azure.Management.Dns.Models;
```

## <a name="initialize-the-dns-management-client"></a>DNS Yönetimi istemci başlatma

`DnsManagementClient` DNS bölgelerini ve kayıt kümelerini yönetmek için gerekli olan özellikleri ve yöntemleri içerir.  Aşağıdaki kod, hizmet sorumlusu hesabı kaydeder ve oluşturur bir `DnsManagementClient` nesne.

```cs
// Build the service credentials and DNS management client
var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
var dnsClient = new DnsManagementClient(serviceCreds);
dnsClient.SubscriptionId = subscriptionId;
```

## <a name="create-or-update-a-dns-zone"></a>Oluşturun veya bir DNS bölgesini güncelleştirme

Bir DNS bölgesi oluşturmak için öncelikle bir "Bölge" nesne DNS bölge parametrelerini içerecek şekilde oluşturulur. DNS bölgelerinin belirli bir bölgeye bağlı değildir çünkü konumu 'genel' olarak ayarlıdır. Bu örnekte, bir [Azure Resource Manager 'tag'](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) ayrıca bölgesine eklenir.

Aslında oluşturmak veya Azure DNS bölgesini güncelleştirme için bölge parametrelerini içeren bölge nesnesi geçirilir `DnsManagementClient.Zones.CreateOrUpdateAsyc` yöntemi.

> [!NOTE]
> DnsManagementClient üç çalışma modunu destekler: zaman uyumlu ('CreateOrUpdate'), zaman uyumsuz ('CreateOrUpdateAsync'), veya zaman uyumsuz HTTP yanıtı ('CreateOrUpdateWithHttpMessagesAsync') erişim.  Uygulama gereksinimlerinize bağlı olarak Bu modun herhangi birinde seçebilirsiniz.

Azure DNS adında iyimser eşzamanlılık destekler [Etag'ler](dns-getstarted-create-dnszone.md). Bu örnekte belirtme "*" 'If-None-Match için' üst bilgisi bir zaten mevcut değilse bir DNS bölgesi oluşturmak için Azure DNS söyler.  Belirtilen kaynak grubunda zaten belirtilen adda bir bölge çağrı başarısız olur.

```cs
// Create zone parameters
var dnsZoneParams = new Zone("global"); // All DNS zones must have location = "global"

// Create an Azure Resource Manager 'tag'.  This is optional.  You can add multiple tags
dnsZoneParams.Tags = new Dictionary<string, string>();
dnsZoneParams.Tags.Add("dept", "finance");

// Create the actual zone.
// Note: Uses 'If-None-Match *' ETAG check, so will fail if the zone exists already.
// Note: For non-async usage, call dnsClient.Zones.CreateOrUpdate(resourceGroupName, zoneName, dnsZoneParams, null, "*")
// Note: For getting the http response, call dnsClient.Zones.CreateOrUpdateWithHttpMessagesAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*")
var dnsZone = await dnsClient.Zones.CreateOrUpdateAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*");
```

## <a name="create-dns-record-sets-and-records"></a>DNS kayıt kümeleri ve kayıtları oluşturma

DNS kayıtlarını kayıt kümesi yönetilir. Kayıt kümesi, kayıtları bir bölge içerisindeki kayıt türü ve aynı ada sahip bir kümesidir.  Bölge adı göreli değil tam DNS adı kayıt kümesinin adıdır.

Oluşturun veya bir kayıt kümesi güncelleştirmek için "Kayıt" parametreleri nesnesi oluşturulur ve geçirilen `DnsManagementClient.RecordSets.CreateOrUpdateAsync`. DNS bölgeleri ile olduğundan işlem üç moddan: zaman uyumlu ('CreateOrUpdate'), zaman uyumsuz ('CreateOrUpdateAsync'), veya zaman uyumsuz HTTP yanıtı ('CreateOrUpdateWithHttpMessagesAsync') erişim.

DNS bölgesi gibi ile kayıt kümeleri üzerinde işlem iyimser eşzamanlılık için destek içerir.  'If-Match' ne 'If-None-Match' belirtilmiş olduğundan, bu örnekte, her zaman kayıt kümesi oluşturulur.  Bu çağrı, bu DNS bölgesinde aynı ada ve bir kayıt türü ile Ayarla varolan bir kaydı üzerine yazar.

```cs
// Create record set parameters
var recordSetParams = new RecordSet();
recordSetParams.TTL = 3600;

// Add records to the record set parameter object.  In this case, we'll add a record of type 'A'
recordSetParams.ARecords = new List<ARecord>();
recordSetParams.ARecords.Add(new ARecord("1.2.3.4"));

// Add metadata to the record set.  Similar to Azure Resource Manager tags, this is optional and you can add multiple metadata name/value pairs
recordSetParams.Metadata = new Dictionary<string, string>();
recordSetParams.Metadata.Add("user", "Mary");

// Create the actual record set in Azure DNS
// Note: no ETAG checks specified, will overwrite existing record set if one exists
var recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSetParams);
```

## <a name="get-zones-and-record-sets"></a>Bölgeler ve kayıt kümelerini Al

`DnsManagementClient.Zones.Get` Ve `DnsManagementClient.RecordSets.Get` yöntemleri almak bireysel bölgeleri ve kayıt kümeleri, sırasıyla. Kayıt kümeleri, kendi türü, adı ve bunlar mevcut bölge ve kaynak grubu tarafından tanımlanır. Bölgeler, kullanıcının adını ve bunlar mevcut bir kaynak grubu tarafından tanımlanır.

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
```

## <a name="update-an-existing-record-set"></a>Mevcut bir kayıt kümesi güncelleştirmesi

Mevcut bir DNS kayıt kümesi güncelleştirmesi, ilk kayıt kümesini almak ve ardından kayıt kümesi içeriği güncelleştirmek için değişikliğin ardından gönderin.  Bu örnekte biz alınan kayıt kümesindeki 'If-Match' parametresinde 'Etag' belirtin. Eşzamanlı bir işlem kaydı sırada kümesi değiştirildi çağrı başarısız olur.

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

// Add a new record to the local object.  Note that records in a record set must be unique/distinct
recordSet.ARecords.Add(new ARecord("5.6.7.8"));

// Update the record set in Azure DNS
// Note: ETAG check specified, update will be rejected if the record set has changed in the meantime
recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);
```

## <a name="list-zones-and-record-sets"></a>Bölgelerini listeleme ve kayıt kümeleri

Alanları listesinde, kullanmak için *DnsManagementClient.Zones.List...*  belirtilen kaynak grubundaki tüm bölgeleri ya da belirli bir Azure aboneliğindeki tüm bölgeleri (kaynak grupları arasında.) listeleme desteği yöntemleri Kayıt kümeleri listesinde, kullanmak için *DnsManagementClient.RecordSets.List...*  belirli bir bölgedeki tüm kayıt kümelerini veya bu kayıt kümeleri yalnızca belirli bir türdeki listeleme desteği yöntemleri.

Bölgeleri listelerken not edin ve sonuçları kayıt kümeleri sayfalandırılmış.  Aşağıdaki örnek, sonuçları sayfalar arasında gezinmek gösterilmektedir. (Bir '2' sınırlarına küçük sayfa boyutunu, disk belleği zorlamak için kullanılır; uygulamada bu parametre kullanılmaz ve varsayılan sayfa boyutu kullanılır.)

```cs
// Note: in this demo, we'll use a very small page size (2 record sets) to demonstrate paging
// In practice, to improve performance you would use a large page size or just use the system default
int recordSets = 0;
var page = await dnsClient.RecordSets.ListAllInResourceGroupAsync(resourceGroupName, zoneName, "2");
recordSets += page.Count();

while (page.NextPageLink != null)
{
    page = await dnsClient.RecordSets.ListAllInResourceGroupNextAsync(page.NextPageLink);
    recordSets += page.Count();
}
```

## <a name="next-steps"></a>Sonraki adımlar

İndirme [Azure DNS .NET SDK'sı kodunuzla](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), Azure DNS .NET örnekleri için diğer DNS kayıt türlerini dahil olmak üzere SDK kullanma hakkında daha fazla örnekleri içerir.
