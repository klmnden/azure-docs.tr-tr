---
title: "Olağanüstü durum kurtarma uygulama kullanarak yedekleme ve geri yükleme Azure API Management'te | Microsoft Docs"
description: "Yedekleme ve olağanüstü durum kurtarma Azure API Management'te gerçekleştirmek için geri yükleme öğrenin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: apimpm
ms.openlocfilehash: 3fcd2fc4162cfbf549be979e15745934c2e4c6ff
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>Olağanüstü durum kurtarma hizmeti Yedekleme kullanarak uygulamak ve Azure API Management'te geri yükleme

Yayımlama ve Azure API Yönetimi yoluyla, API'ları yönetmek seçerek birçok hata toleransı ve aksi durumda tasarım, uygulamak ve yönetmek için olurdu altyapı olanaklarından sürüyor. Azure platformu olası hataları bir kısmı maliyetle en büyük bir kısmı azaltır.

API Management hizmetiniz barındırıldığı bölgeyi etkileyen kullanılabilirlik sorunlarından kurtarmak üzere hizmetiniz farklı bir bölgede herhangi bir zamanda yeniden oluşturma hazır olmanız gerekir. Kullanılabilirlik hedefleri ve kurtarma süresi hedefi bağlı olarak, bir yedekleme hizmeti bir veya daha fazla bölgelerde ayırabilir ve bunların yapılandırma ve içerik etkin hizmeti ile eşitlenmiş tutmak deneyin isteyebilirsiniz. Hizmeti "Yedekleme ve geri yükleme" özelliği, olağanüstü durum kurtarma stratejiniz uygulamak için gerekli yapı taşı sağlar.

Bu kılavuz, Azure Resource Manager istekleri kimlik doğrulaması yapmayı ve yedekleme ve geri yükleme, API Management hizmeti örnekleri nasıl gösterir.

> [!NOTE]
> Yedekleme ve olağanüstü durum kurtarma için bir API Management hizmet örneği geri yükleme işlemi, API Management hizmeti örnekleri hazırlama gibi senaryolar için çoğaltmak için de kullanılabilir.
>
> Her yedekleme 30 gün sonra süresi dolar. 30 günlük süre sona erdiğinde bir yedeğini geri çalışırsanız, geri yükleme başarısız bir `Cannot restore: backup expired` ileti.
>
>

## <a name="authenticating-azure-resource-manager-requests"></a>Kimlik doğrulama Azure Resource Manager istekleri

> [!IMPORTANT]
> Yedekleme ve geri yükleme için REST API Azure Resource Manager kullanır ve API Management varlıklarınızı yönetmek için farklı kimlik doğrulama mekanizması REST API'leri daha vardır. Bu bölümdeki adımlar, Azure Resource Manager isteklerinin kimlik doğrulaması açıklar. Daha fazla bilgi için bkz: [Azure Resource Manager kimlik doğrulama istekleri](http://msdn.microsoft.com/library/azure/dn790557.aspx).
>
>

Tüm kaynakları Azure Kaynak Yöneticisi'ni kullanarak bunu görevleri Azure aşağıdaki adımları kullanarak Active Directory'e ile kimlik doğrulaması yapılması gerekir:

* Azure Active Directory Kiracı için bir uygulama ekleyin.
* Eklediğiniz uygulama izinlerini ayarlayın.
* İstekleri için Azure Resource Manager kimlik doğrulaması için belirteç alın.

### <a name="create-an-azure-active-directory-application"></a>Azure Active Directory Uygulama oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın. 
2. API Management hizmet örneğinizin içeren aboneliğinden gidin **uygulama kayıtlar** sekmesi.

    > [!NOTE]
    > Azure Active Directory varsayılan dizin hesabınıza görünür durumda değilse, hesabınız için gerekli izinleri vermek için Azure aboneliği yöneticisine başvurun.
3. **Yeni uygulama kaydı**’na tıklayın.

    **Oluşturma** penceresi sağ tarafta görüntülenir. AAD uygulama ilgili bilgileri girdiğiniz olmasıdır.
4. Uygulama için bir ad girin.
5. Uygulama türü için **yerel**.
6. Yer tutucu URL girin `http://resources` için **yeniden yönlendirme URI'si**, gerekli bir alandır ancak değer daha sonra kullanılmaz. Uygulamayı kaydetmek için onay kutusuna tıklayın.
7. **Oluştur**’a tıklayın.

### <a name="add-an-application"></a>Uygulama ekle

1. Uygulama oluşturulduktan sonra tıklatın **ayarları**.
2. Tıklatın **gerekli izinleri**.
3. Tıklatın **+ Ekle**.
4. Tuşuna **bir API seçin**.
5. Seçin **Windows** **Azure Hizmet Yönetimi API**.
6. Tuşuna **seçin**. 

    ![İzin ekle](./media/api-management-howto-disaster-recovery-backup-restore/add-app.png)

7. Tıklatın **izinlere temsilci** yeni eklenen uygulamanın onay kutusunu için **erişim Azure Hizmet Yönetimi (Önizleme)**.
8. Tuşuna **seçin**.

### <a name="configuring-your-app"></a>Uygulamanızı yapılandırma

Yedekleme oluşturmak ve bunu geri yüklemeyi API'lerini çağırmadan önce bir belirteç almak gereklidir. Aşağıdaki örnek kullanır [Microsoft.IdentityModel.Clients.activedirectory tarafından](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) belirtecini almak için NuGet paketi.

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
            var result = authenticationContext.AcquireToken("https://management.azure.com/", {application id}, new Uri({redirect uri});

            if (result == null) {
                throw new InvalidOperationException("Failed to obtain the JWT token");
            }

            Console.WriteLine(result.AccessToken);

            Console.ReadLine();
        }
    }
}
```

Değiştir `{tentand id}`, `{application id}`, ve `{redirect uri}` aşağıdaki yönergeleri kullanarak:

1. Değiştir `{tenant id}` oluşturduğunuz Azure Active Directory Uygulama Kiracı kimliği. Tıklatarak kimliği erişebilirsiniz **uygulama kayıtlar** -> **uç noktaları**.

    ![Uç Noktalar][api-management-endpoint]
2. Değiştir `{application id}` giderek aldığınız değerle **ayarları** sayfası.
3. URL'den Değiştir **yeniden yönlendirme URI'ler** sekme, Azure Active Directory uygulamanızın kullanılabilir.

    Kod örneği değerleri belirlendikten sonra bir belirteç aşağıdaki örneğe benzer şekilde döndürmesi gerekir:

    ![Belirteç][api-management-arm-token]

## <a name="calling-the-backup-and-restore-operations"></a>Yedekleme ve geri yükleme işlemlerini çağırma

Aşağıdaki bölümlerde açıklanan "Yedekleme ve geri yükleme" işlemleri çağırmadan önce yetkilendirme isteği başlığı REST çağrısı için ayarlayın.

```csharp
request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);
```

### <a name="step1"></a>Bir API Management hizmeti yedekleyin
Aşağıdaki HTTP isteği bir API Management hizmet sorunu yedeklemek için:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

Burada:

* `subscriptionId`-Yedekleme çalıştığınız API Management hizmeti içeren abonelik kimliği
* `resourceGroupName`-'Api - varsayılan-{hizmeti-bölge}' biçiminde bir dize nerede `service-region` burada API Management hizmeti Azure bölgelerini tanımlayan denediğiniz yedekleme barındırılan, örneğin,`North-Central-US`
* `serviceName`-API Management hizmet adını, oluşturma sırasında belirtilen bir yedeğini kuran
* `api-version`-değiştirin`2014-02-14`

İstek gövdesinde hedef Azure depolama hesabı adı, erişim tuşu, blob kapsayıcı adı ve yedek adı belirtin:

```
'{  
    storageAccount : {storage account name for the backup},  
    accessKey : {access key for the account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

Değerini `Content-Type` istek üstbilgisi `application/json`.

Yedekleme tamamlamak için birden çok dakika sürebilir uzun süren bir işlemdir.  İstek başarılı oldu ve yedekleme işlemi başlatıldı, aldığınız bir `202 Accepted` yanıt durum koduyla bir `Location` üstbilgi.  'GET istekleri URL'ye' olun `Location` işlemi durumunu öğrenmek için üstbilgi. Yedekleme işlemi devam ederken '202 kabul edilen' durum kodunu almaya devam eder. Bir yanıt kodu, `200 OK` yedekleme işlemi başarılı şekilde tamamlandığını gösterir.

Aşağıdaki kısıtlamalar yedekleme isteği yapılırken unutmayın.

* **Kapsayıcı** istek gövdesinde belirtilen **bulunmalıdır**.
* Yedekleme, sürerken **herhangi bir hizmet yönetim işlemini çalışmamalısınız** SKU yükseltme veya indirgeme, etki alanı adı değişikliği gibi vb.
* Geri yükleme bir **yedekleme yalnızca 30 gün boyunca garanti** oluşturulduktan andan itibaren.
* **Kullanım verileri** analytics raporları oluşturmak için kullanılan **eklenmedi** yedekleme. Kullanım [Azure API Management REST API] [ Azure API Management REST API] analytics raporları güvenli olarak saklamak için düzenli aralıklarla alınamadı.
* Hizmet yedeklemeler gerçekleştirmek sıklığı, kurtarma noktası hedefi etkiler. Bu en aza indirmek için öneri düzenli yedeklemeler uygulama yanı sıra API Management hizmetiniz için önemli değişiklikler yaptıktan sonra isteğe bağlı yedeklemeleri gerçekleştirmek.
* **Değişiklikleri** (örneğin, API, ilkeleri, Geliştirici Portalı Görünüm) hizmet yapılandırmasını yedeklemesi sırasında yapılan işlemdir işleminde **yedekleme işlemine dahil edilmeyen ve bu nedenle kaybolacak**.

### <a name="step2"></a>Bir API Management hizmeti geri yükleme
Önceden oluşturulmuş bir yedek hizmetinden bir API Management geri yüklemek için aşağıdaki HTTP isteği olun:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

Burada:

* `subscriptionId`-içine bir yedeği geri yüklediğiniz API Management hizmeti içeren abonelik kimliği
* `resourceGroupName`-'Api - varsayılan-{hizmeti-bölge}' biçiminde bir dize nerede `service-region` içine bir yedeği geri yüklediğiniz API Management hizmeti barındırıldığı, örneğin, Azure bölgesi tanımlar`North-Central-US`
* `serviceName`-hizmet uygulamasına geri yükleniyor, oluşturma sırasında belirtilen API Management adı
* `api-version`-değiştirin`2014-02-14`

İstek gövdesinde olan yedekleme dosyası konumu, Azure depolama hesabı adı, erişim tuşu, blob kapsayıcı adı ve yedek adı belirtin:

```
'{  
    storageAccount : {storage account name for the backup},  
    accessKey : {access key for the account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

Değerini `Content-Type` istek üstbilgisi `application/json`.

Geri yüklemeyi tamamlamak için en az 30 dakika sürebileceğini uzun süren bir işlemdir. İstek başarılı oldu ve geri yükleme işlemi başlatıldı, aldığınız bir `202 Accepted` yanıt durum koduyla bir `Location` üstbilgi. 'GET istekleri URL'ye' olun `Location` işlemi durumunu öğrenmek için üstbilgi. Geri yükleme işlemi devam ederken '202 kabul edilen' almaya devam durum kodu. Bir yanıt kodu, `200 OK` geri yükleme işlemi başarılı şekilde tamamlandığını gösterir.

> [!IMPORTANT]
> **SKU** uygulamasına geri yükleniyor hizmetinin **eşleşmelidir** SKU yedeklenen hizmet geri yükleniyor.
>
> **Değişiklikleri** yaptığınız geri yükleme sırasında hizmet yapılandırma (örneğin, API'leri, ilkeleri, Geliştirici Portalı Görünüm) için işlem devam ediyor **üzerine yazılabilir**.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Yedekleme/geri yükleme işleminin iki farklı izlenecek yol aşağıdaki Microsoft blogları göz atın.

* [Azure API Management hesapları Çoğalt](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/)
* [Azure API Management: Yedekleme ve yapılandırmasını geri yükleme](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
  * Ancak ilgi çekici Stuart tarafından ayrıntılı yaklaşım resmi Kılavuzu eşleşmiyor.

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2


[Azure API Management REST API]: http://msdn.microsoft.com/library/azure/dn781421.aspx

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
