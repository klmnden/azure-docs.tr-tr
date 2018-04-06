---
title: DNS bölgeleri oluşturma ve kayıt kümeleri .NET SDK kullanarak Azure DNS'de | Microsoft Docs
description: DNS bölgeleri ve kaydı nasıl oluşturulacağını, .NET SDK kullanarak Azure DNS'de ayarlar.
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
ms.assetid: eed99b87-f4d4-4fbf-a926-263f7e30b884
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2016
ms.author: jonatul
ms.openlocfilehash: c0fb0be8da1c0ca48a4d43ea027d30a0bc17fe30
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="create-dns-zones-and-record-sets-using-the-net-sdk"></a>DNS bölgeleri ve .NET SDK kullanarak kayıt kümeleri oluşturma

Oluşturamaz, silemez veya DNS bölgeleri, kayıt kümelerini ve kayıtları .NET DNS Yönetim kitaplığıyla DNS SDK kullanılarak güncelleştirmek için işlemleri otomatik hale getirebilirsiniz. Tam bir Visual Studio projesi kullanılabilir [burada.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)

## <a name="create-a-service-principal-account"></a>Bir hizmet sorumlusu hesabı oluşturun

Genellikle, kendi kullanıcı kimlik bilgilerini yerine adanmış bir hesap Azure kaynaklarına programlı erişim verilir. Bu ayrılmış hesapları 'hizmet sorumlusu' hesapları adı verilir. Azure DNS SDK örnek proje kullanmak için önce bir hizmet sorumlusu hesabı oluşturmak ve doğru izinleri atamak gerekir.

1. İzleyin [bu yönergeleri](../azure-resource-manager/resource-group-authenticate-service-principal.md) (Azure DNS SDK örnek proje parola tabanlı kimlik doğrulama varsayar.) bir hizmet sorumlusu hesabı oluşturmak için
2. Bir kaynak grubu oluşturun ([burada nasıl](../azure-resource-manager/resource-group-template-deploy-portal.md)).
3. Hizmet sorumlusu hesabı kaynak grubunda 'DNS bölge katkıda bulunan' izinleri vermek için Azure RBAC kullanın ([burada nasıl](../active-directory/role-based-access-control-configure.md).)
4. Azure DNS SDK örnek proje kullanıyorsanız, aşağıdaki gibi 'program.cs' dosyasını düzenleyin:

   * Tenantıd, istemci kimliği (hesap kimliği olarak da bilinir), gizliliği (hizmet sorumlusu hesabı parola) ve 1. adımda kullanılan Subscriptionıd için doğru değerleri ekleyin.
   * 2. adımda seçilen kaynak grubu adı girin.
   * Tercih ettiğiniz bir DNS bölgesi ad girin.

## <a name="nuget-packages-and-namespace-declarations"></a>NuGet paketleri ve ad alanı bildirimleri

Azure DNS .NET SDK'yı kullanmak için yüklemeniz gerekir **Azure DNS Yönetimi Kitaplığı** NuGet paketi ve diğer gerekli Azure paketler.

1. İçinde **Visual Studio**, bir proje veya yeni bir proje açın.
2. Git **Araçları** **>** **NuGet Paket Yöneticisi** **>** **çözüm için NuGet paketlerini Yönet...** .
3. Tıklatın **Gözat**, etkinleştirme **INCLUDE yayın öncesi** onay kutusunu ve türü **Microsoft.Azure.Management.Dns** arama kutusuna.
4. Paketi seçin ve **yükleme** Visual Studio projenize eklemek için.
5. Ayrıca aşağıdaki paketleri yüklemek için yukarıdaki işlemi tekrarlayın: **Microsoft.Rest.ClientRuntime.Azure.Authentication** ve **Microsoft.Azure.Management.ResourceManager**.

## <a name="add-namespace-declarations"></a>Ad alanı bildirimleri ekleme

Aşağıdaki ad alanı bildirimleri ekleme

```cs
using Microsoft.Rest.Azure.Authentication;
using Microsoft.Azure.Management.Dns;
using Microsoft.Azure.Management.Dns.Models;
```

## <a name="initialize-the-dns-management-client"></a>DNS yönetim istemcisi başlatılamadı

*DnsManagementClient* DNS bölgeleri ve kayıt kümeleri yönetmek için gerekli özellikler ve yöntemler içerir.  Aşağıdaki kod, hizmet sorumlusu hesabı oturum açtığında ve bir DnsManagementClient nesnesi oluşturur.

```cs
// Build the service credentials and DNS management client
var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
var dnsClient = new DnsManagementClient(serviceCreds);
dnsClient.SubscriptionId = subscriptionId;
```

## <a name="create-or-update-a-dns-zone"></a>Bir DNS bölgesi güncelle

Bir DNS bölgesi oluşturmak için öncelikle bir "Bölgesine" nesne DNS bölge parametrelerini içerecek şekilde oluşturulur. DNS bölgelerini belirli bir bölgede bağlı değildir çünkü konumu 'genel' olarak ayarlıdır. Bu örnekte, bir [Azure Resource Manager 'etiketinin'](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) de bölgesine eklenir.

Gerçekte oluşturmak veya Azure DNS bölgesinde güncelleştirmek için bölge parametrelerini içeren bölge nesnesi geçirilir *DnsManagementClient.Zones.CreateOrUpdateAsyc* yöntemi.

> [!NOTE]
> DnsManagementClient işlemi üç modlarını destekler: eşzamanlı ('CreateOrUpdate'), zaman uyumsuz ('CreateOrUpdateAsync'), ya da erişim HTTP yanıtına ('CreateOrUpdateWithHttpMessagesAsync') ile uyumsuz.  Uygulamanızın gereksinimlerine bağlı olarak bu modlarından birini seçebilirsiniz.

Azure DNS destekler adlı iyimser eşzamanlılık [Etag'ler](dns-getstarted-create-dnszone.md). Bu örnekte belirtme "*" 'If-None-Match için' Azure bir zaten yoksa, bir DNS bölgesi oluşturmak için DNS üstbilgi söyler.  Verilen ada sahip bir bölge verilen kaynak grubunda zaten varsa, çağrı başarısız olur.

```cs
// Create zone parameters
var dnsZoneParams = new Zone("global"); // All DNS zones must have location = "global"

// Create a Azure Resource Manager 'tag'.  This is optional.  You can add multiple tags
dnsZoneParams.Tags = new Dictionary<string, string>();
dnsZoneParams.Tags.Add("dept", "finance");

// Create the actual zone.
// Note: Uses 'If-None-Match *' ETAG check, so will fail if the zone exists already.
// Note: For non-async usage, call dnsClient.Zones.CreateOrUpdate(resourceGroupName, zoneName, dnsZoneParams, null, "*")
// Note: For getting the http response, call dnsClient.Zones.CreateOrUpdateWithHttpMessagesAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*")
var dnsZone = await dnsClient.Zones.CreateOrUpdateAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*");
```

## <a name="create-dns-record-sets-and-records"></a>DNS kayıt kümelerini ve kayıtları oluşturma

DNS kayıtlarını kayıt kümesi yönetilir. Kayıt kümesi, aynı ada ve bir bölge içindeki kayıt türü kayıtlarıyla kümesidir.  Kayıt kümesi, bölge adı göreli değil tam DNS adı adıdır.

Bir kayıt kümesini oluştur veya güncelleştir için bir "Kayıt kümesi" parametreleri nesnesi oluşturulur ve geçirilen *DnsManagementClient.RecordSets.CreateOrUpdateAsync*. DNS bölgeleri ile olmadığından işlem üç modu: eşzamanlı ('CreateOrUpdate'), zaman uyumsuz ('CreateOrUpdateAsync'), veya zaman uyumsuz HTTP yanıtı ('CreateOrUpdateWithHttpMessagesAsync') erişim.

DNS bölgeleri gibi ile kayıt kümeleri üzerinde işlem iyimser eşzamanlılık desteği içerir.  'If-Match' ne 'If-None-Match' belirtilmiş olduğundan, bu örnekte, her zaman kayıt kümesi oluşturulur.  Bu çağrı, bu DNS bölgesinde aynı ada ve kayıt türü ile ayarlamak varolan bir kaydı üzerine yazar.

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

## <a name="get-zones-and-record-sets"></a>Get bölgeleri ve kayıt kümeleri

*DnsManagementClient.Zones.Get* ve *DnsManagementClient.RecordSets.Get* yöntemleri tek başına bölgeleri ve kayıt kümelerini sırasıyla alma. Kayıt kümeleri kendi türü, adı ve bunlar mevcut bölgesi ve kaynak grubu tarafından tanımlanır. Bölgeler, kendi ad ve bunlar mevcut kaynak grubu tarafından tanımlanır.

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
```

## <a name="update-an-existing-record-set"></a>Varolan bir kayıt kümesini güncelleştir

Varolan bir DNS kaydı kümesini güncelleştirmek, ilk kayıt kümesi almak kayıt kümesi içeriği güncelleştirmek için ardından değişiklik gönderin.  Bu örnekte, alınan kayıt kümesinde 'If-Match' parametresinde 'Etag' belirtin. Eşzamanlı bir işlem kayıt zarfında kümesine değiştirdi çağrı başarısız olur.

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

// Add a new record to the local object.  Note that records in a record set must be unique/distinct
recordSet.ARecords.Add(new ARecord("5.6.7.8"));

// Update the record set in Azure DNS
// Note: ETAG check specified, update will be rejected if the record set has changed in the meantime
recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);
```

## <a name="list-zones-and-record-sets"></a>Liste bölgeleri ve kayıt kümeleri

Bölgeler listesinden, kullanmak için *DnsManagementClient.Zones.List...*  belirtilen kaynak grubunun tüm bölgelerde ya da belirli bir Azure aboneliği tüm bölgelerde (kaynak grupları arasında.) listeleme destek yöntemleri Kayıt kümeleri listesinde, kullanmak için *DnsManagementClient.RecordSets.List...*  belirli bir bölgedeki tüm kayıt kümelerinin ya da bu kaydı kümeleri yalnızca belirli bir türdeki listeleme destek yöntemleri.

Bölgeleri listelerken not alın ve sonuçları kayıt kümelerini anlatır.  Aşağıdaki örnek, sonuçları sayfalarıyla yinelemek gösterilmektedir. ('2' yapay küçük sayfa boyutunu, disk belleği zorlamak için kullanılır; uygulamada bu parametre atlanırsa ve varsayılan sayfa boyutu kullanılır.)

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

Karşıdan [Azure DNS .NET SDK örnek proje](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), başka Azure DNS .NET diğer DNS kayıt türleri için örnekler dahil SDK kullanma örnekleri içerir.
