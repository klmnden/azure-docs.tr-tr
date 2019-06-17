---
title: Yedekleme ve geri yükleme, Azure API Yönetimi'nde uygulama olağanüstü durum kurtarma'yı kullanarak | Microsoft Docs
description: Yedekleme ve olağanüstü durum kurtarma, Azure API Yönetimi'nde gerçekleştirmek için geri yükleme hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2018
ms.author: apimpm
ms.openlocfilehash: e0c02829a2fef6e281794fdba6c9fb5d9b8a736b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66241711"
---
# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>Olağanüstü durum kurtarma hizmeti Yedekleme kullanarak uygulayın ve Azure API Yönetimi'nde geri yükleme

Yayımlama ve Azure API Management aracılığıyla API'leri yönetmeye, hataya dayanıklılık ve aksi tasarım, uygulamak ve el ile yönetmek olan altyapı yeteneklerinden yönlendiriyoruz. Azure platformu, olası arızaları maliyetinin daha düşük Ücretlerle büyük bir bölümünü azaltır.

API Yönetimi hizmetiniz barındıran bölgeyi etkileyen kullanılabilirlik sorunları gidermek için hizmetinizde başka bir bölgeye herhangi bir zamanda yeniden oluşturmak hazır olun. Kullanılabilirlik ve kurtarma zamanı hedeflerine bağlı olarak, bir veya daha fazla bölgede bir yedekleme hizmeti ayırmak isteyebilirsiniz. Kendi yapılandırma ve içerik etkin hizmeti ile eşitlenmiş tutmak deneyebilir. Hizmeti "Yedekleme ve geri yükleme" özelliği, olağanüstü durum kurtarma stratejiniz uygulamak için gereken Yapı bloğu sağlar.

Bu kılavuz, Azure Resource Manager istek doğrulamanın nasıl gerçekleştirileceğini gösterir. Ayrıca, yedekleme ve geri yükleme, API Management hizmet örneklerini işlemini gösterir.

> [!NOTE]
> Yedekleme ve olağanüstü durum kurtarma için API Management hizmet örneği geri yükleme işlemi, API Management hizmet örneklerini hazırlama gibi senaryolar için çoğaltmak için de kullanılabilir.
>
> Her yedekleme 30 gün sonra süresi dolar. 30 günlük süre sona erdiğinde bir yedekleme geri yükleme girişimi, geri yükleme ile başarısız olur bir `Cannot restore: backup expired` ileti.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="authenticating-azure-resource-manager-requests"></a>Azure Resource Manager kimlik doğrulama istekleri

> [!IMPORTANT]
> Yedekleme ve geri yükleme için REST API, Azure Resource Manager kullanır ve API Management varlıklarınızı yönetmek için farklı kimlik doğrulama mekanizması REST API'lerini daha vardır. Bu bölümdeki adımları, Azure Resource Manager istek doğrulamanın nasıl gerçekleştirileceğini açıklar. Daha fazla bilgi için [Azure Resource Manager kimlik doğrulama istekleri](/rest/api/index).

Kaynakları Azure Resource Manager kullanarak bunu görevlerin tümü, Azure aşağıdaki adımları kullanarak Active Directory ile kimlik doğrulaması yapılması gerekir:

* Azure Active Directory kiracısına uygulama ekleyin.
* Eklediğiniz uygulama izinlerini ayarlayın.
* Azure Resource Manager'a isteklerine kimlik doğrulaması için belirteç alın.

### <a name="create-an-azure-active-directory-application"></a>Bir Azure Active Directory uygulaması oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. API Management hizmet örneğinizin içeren aboneliği kullanarak **uygulama kayıtları** sekmesinde **Azure Active Directory** (Azure Active Directory > Yönet/uygulama kayıtları).

    > [!NOTE]
    > Azure Active Directory varsayılan dizin hesabınıza görünür değilse, hesabınız için gerekli izinleri vermek için Azure aboneliği yöneticisine başvurun.
3. **Yeni uygulama kaydı**’na tıklayın.

    **Oluştur** penceresi sağ tarafta görüntülenir. AAD uygulaması ilgili bilgileri girebileceğiniz olmasıdır.
4. Uygulama için bir ad girin.
5. Uygulama türü için **yerel**.
6. Bir yer tutucu URL'yi girin `http://resources` için **yeniden yönlendirme URI'si**, gerekli bir alandır, ancak değeri daha sonra kullanılmaz. Uygulamayı kaydetmek için onay kutusuna tıklayın.
7. **Oluştur**’a tıklayın.

### <a name="add-an-application"></a>Uygulama ekleme

1. Uygulama oluşturulduktan sonra tıklayın **ayarları**.
2. Tıklayın **gerekli izinler**.
3. Tıklayın **+ Ekle**.
4. Tuşuna **bir API seçin**.
5. Seçin **Windows** **Azure Hizmet Yönetimi API**.
6. Tuşuna **seçin**.

    ![İzin ekleme](./media/api-management-howto-disaster-recovery-backup-restore/add-app.png)

7. Tıklayın **Temsilcili izinler** için yeni eklenen uygulamanın kutuyu **Azure Hizmet Yönetimi (Önizleme) erişim**.
8. Tuşuna **seçin**.
9. Tıklayın **izinleri verin**.

### <a name="configuring-your-app"></a>Uygulamanızı yapılandırma

Yedekleme oluşturmak ve bunu geri yüklemeyi API'leri çağrılmadan önce bir belirteç almanız gerekir. Aşağıdaki örnekte [Microsoft.IdentityModel.Clients.activedirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) belirteci almak için NuGet paketi.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace GetTokenResourceManagerRequests
{
    class Program
    {
        static void Main(string[] args)
        {
            var authenticationContext = new AuthenticationContext("https://login.microsoftonline.com/{tenant id}");
            var result = authenticationContext.AcquireTokenAsync("https://management.azure.com/", "{application id}", new Uri("{redirect uri}"), new PlatformParameters(PromptBehavior.Auto)).Result;

            if (result == null) {
                throw new InvalidOperationException("Failed to obtain the JWT token");
            }

            Console.WriteLine(result.AccessToken);

            Console.ReadLine();
        }
    }
}
```

Değiştirin `{tenant id}`, `{application id}`, ve `{redirect uri}` aşağıdaki yönergeleri kullanarak:

1. Değiştirin `{tenant id}` oluşturduğunuz Azure Active Directory uygulamasının Kiracı kimliği. Kimliği tıklayarak erişebilirsiniz **uygulama kayıtları** -> **uç noktaları**.

    ![Uç Noktalar][api-management-endpoint]
2. Değiştirin `{application id}` giderek aldığınız değerle **ayarları** sayfası.
3. Değiştirin `{redirect uri}` değeriyle **yeniden yönlendirme URI'leri** Azure Active Directory Uygulama sekmesinde.

    Belirtilen değerler sonra kod örneği bir belirteç aşağıdaki örneğe benzer döndürülmesi gerekir:

    ![Belirteç][api-management-arm-token]

    > [!NOTE]
    > Belirteç, belirli bir süre sonra dolabilir. Yeni bir belirteci yeniden üretmek için kod örneği çalıştırın.

## <a name="calling-the-backup-and-restore-operations"></a>Yedekleme ve geri yükleme işlemleri çağırma

REST API'leri [API yönetim hizmeti - yedekleme](/rest/api/apimanagement/2019-01-01/apimanagementservice/backup) ve [API yönetim hizmeti - geri yükleme](/rest/api/apimanagement/2019-01-01/apimanagementservice/restore).

Aşağıdaki bölümlerde açıklanan "Yedekleme ve geri yükleme" işlemleri çağırmadan önce yetkilendirme isteği üst bilgisi, REST çağrısı için ayarlayın.

```csharp
request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);
```

### <a name="step1"> </a>Bir API Yönetimi Hizmeti yedekleme

API Management hizmet soruna aşağıdaki HTTP isteği yedeklemek için:

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}
```

burada:

* `subscriptionId` -API Management hizmeti, yedekleme çalıştığınız tutan aboneliğin kimliği
* `resourceGroupName` -Azure API Yönetimi hizmetiniz kaynak grubunun adı
* `serviceName` -API Management hizmeti adını oluşturulduğu sırada belirtilen yedek bir kopyasını yaparsınız
* `api-version` -değiştirin `2018-06-01-preview`

İstek gövdesinde, hedef Azure depolama hesabı adı, erişim anahtarı, blob kapsayıcı adı ve yedekleme adı belirtin:

```json
{
  "storageAccount": "{storage account name for the backup}",
  "accessKey": "{access key for the account}",
  "containerName": "{backup container name}",
  "backupName": "{backup blob name}"
}
```

Değerini `Content-Type` istek üstbilgisi `application/json`.

Yedekleme tamamlanması bir dakikadan daha fazla sürebilir uzun süren bir işlemdir.  İstek başarılı oldu ve yedekleme işlemi başladı, aldığınız bir `202 Accepted` yanıt durum kodu ile bir `Location` başlığı.  'GET istekleri URL'ye' olun `Location` işlem durumunu öğrenmek için üst bilgi. Yedekleme devam ederken '202 kabul edildi' durum kodu almaya devam eder. Yanıt kodunu `200 OK` yedekleme işlemi başarılı olarak tamamlanmasına gösterir.

Yedekleme isteği yaparken aşağıdaki kısıtlamaları göz önünde bulundurun:

* **Kapsayıcı** istek gövdesinde belirtilen **bulunmalıdır**.
* Yedekleme devam ederken **hizmet yönetimi değişiklikleri önlemek** SKU yükseltme veya düşürme gibi etki alanı adı ve daha fazlasını değiştirin.
* Geri yükleme bir **yedekleme yalnızca 30 gün boyunca garanti** oluşturulduğu andan itibaren.
* **Kullanım verilerini** analiz raporları oluşturmak için kullanılan **bulunmaz** yedekleme. Kullanım [Azure API Management REST API] [ Azure API Management REST API] analiz raporları flooring'in düzenli olarak alınacak.
* Hizmet yedekleme ile gerçekleştirdiğiniz sıklığı, kurtarma noktası hedefi etkiler. Bu en aza indirmek için düzenli yedeklemeler uygulama ve API Management hizmetiniz için değişiklikler yaptıktan sonra isteğe bağlı yedeklemeler gerçekleştirmek öneririz.
* **Değişiklikleri** yedeklemesi sırasında (örneğin, API, ilkeleri ve Geliştirici Portalı görünümünü) hizmeti yapılandırması için yapılan işlemi işlemde olduğu **yedeklemeden hariç tutulması ve kaybolacak**.

### <a name="step2"> </a>API Management hizmeti geri yükleme

API Management hizmeti önceden oluşturulmuş bir yedeklemeden geri yüklemek için aşağıdaki HTTP isteği olun:

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}
```

burada:

* `subscriptionId` -API Management hizmeti ile bir yedekleme geri yükleme tutan aboneliğin kimliği
* `resourceGroupName` -içine bir yedekleme geri Azure API Management hizmeti içeren kaynak grubunun adı
* `serviceName` -API Management hizmet uygulamasına geri yükleniyor, oluşturma zamanında belirtilen adı
* `api-version` -değiştirin `2018-06-01-preview`

İstek gövdesinde yedekleme dosyasının konumunu belirtin. Diğer bir deyişle, Azure depolama hesabı adı, erişim anahtarı, blob kapsayıcı adı ve yedekleme adı ekleyin:

```json
{
  "storageAccount": "{storage account name for the backup}",
  "accessKey": "{access key for the account}",
  "containerName": "{backup container name}",
  "backupName": "{backup blob name}"
}
```

Değerini `Content-Type` istek üstbilgisi `application/json`.

Geri yükleme tamamlamak için en az 30 dakika sürebileceğini uzun süren bir işlemdir. İstek başarılı oldu ve geri yükleme işlemi başladı, aldığınız bir `202 Accepted` yanıt durum kodu ile bir `Location` başlığı. 'GET istekleri URL'ye' olun `Location` işlem durumunu öğrenmek için üst bilgi. Geri yükleme işlemi devam ederken '202 kabul edildi' almaya devam durum kodu. Yanıt kodunu `200 OK` geri yükleme işlemi başarılı olarak tamamlanmasına gösterir.

> [!IMPORTANT]
> **SKU** içine geri yüklenen hizmetin **eşleşmelidir** geri yükleniyor yedekleme hizmetinin SKU.
>
> **Değişiklikleri** yapılan işlemi devam ediyor (örneğin, API, ilkeleri, Geliştirici Portalı görünümünü) hizmeti yapılandırmasını geri yükleme sırasında **yazılabiliyordu**.

<!-- Dummy comment added to suppress markdown lint warning -->

> [!NOTE]
> Yedekleme ve geri yükleme işlemleri de PowerShell ile gerçekleştirilmesi *yedekleme AzApiManagement* ve *geri yükleme-AzApiManagement* komutları sırasıyla.

## <a name="next-steps"></a>Sonraki adımlar

Yedekleme/geri yükleme işleminin farklı izlenecek yollar için aşağıdaki kaynaklara göz atın.

* [Azure API Management hesaplarını çoğaltın](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/)
* [Logic Apps ile API Management Yedekleme ve Geri Yükleme İşlemlerini Otomatikleştirme](https://github.com/Azure/api-management-samples/tree/master/tutorials/automating-apim-backup-restore-with-logic-apps)
* [Azure API Yönetimi: Yedekleme ve geri yükleme Yapılandırması](https://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
  *Fatih tarafından ayrıntılı bir yaklaşım resmi rehberlik eşleşmiyor ancak ilgi çekici değil.*

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2

[Azure API Management REST API]: https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
