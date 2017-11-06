---
title: "Olağanüstü durum kurtarma uygulama kullanarak yedekleme ve geri yükleme Azure API Management'te | Microsoft Docs"
description: "Yedekleme ve olağanüstü durum kurtarma Azure API Management'te gerçekleştirmek için geri yükleme öğrenin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 6f10be3c-f796-4a6c-bacd-7931b6aa82af
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 51ec88ae2a4f10b68c7d74324c3a3a25d2893891
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2017
---
# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>Olağanüstü durum kurtarma hizmeti Yedekleme kullanarak uygulamak ve Azure API Management'te geri yükleme
Yayımlama ve Azure API Yönetimi yoluyla, API'ları yönetmek seçerek birçok hata toleransı ve aksi durumda tasarım, uygulamak ve yönetmek için olurdu altyapı olanaklarından sürüyor. Azure platformu olası hataları bir kısmı maliyetle en büyük bir kısmı azaltır.

Kullanılabilirlik kurtarmak için API Management hizmeti, barındırıldığı bölgeyi etkileyen sorunları hizmetiniz farklı bir bölgede herhangi bir zamanda yeniden oluşturma hazır olmanız gerekir. Kullanılabilirlik hedefleriniz ve kurtarma süresi hedefi bağlı olarak bir yedekleme hizmeti bir veya daha fazla bölgelerde ayırabilir ve bunların yapılandırma ve içerik etkin hizmeti ile eşitlenmiş tutmak deneyin isteyebilirsiniz. Hizmeti yedekleme ve geri yükleme özelliği, olağanüstü durum kurtarma stratejiniz uygulamak için gerekli yapı taşı sağlar.

Bu kılavuz, Azure Resource Manager istekleri kimlik doğrulaması yapmayı ve yedekleme ve geri yükleme, API Management hizmeti örnekleri nasıl gösterir.

> [!NOTE]
> Yedekleme ve olağanüstü durum kurtarma için bir API Management hizmet örneği geri yükleme işlemi, API Management hizmeti örnekleri hazırlama gibi senaryolar için çoğaltmak için de kullanılabilir.
>
> Her yedekleme 30 gün sonra süresi dolar unutmayın. 30 günlük süre sona erdiğinde bir yedeğini geri çalışırsanız, geri yükleme başarısız bir `Cannot restore: backup expired` ileti.
>
>

## <a name="authenticating-azure-resource-manager-requests"></a>Kimlik doğrulama Azure Resource Manager istekleri
> [!IMPORTANT]
> Yedekleme ve geri yükleme için REST API Azure Resource Manager kullanır ve API Management varlıklarınızı yönetmek için farklı kimlik doğrulama mekanizması REST API'leri daha vardır. Bu bölümdeki adımlar, Azure Resource Manager isteklerinin kimlik doğrulaması açıklar. Daha fazla bilgi için bkz: [Azure Resource Manager kimlik doğrulama istekleri](http://msdn.microsoft.com/library/azure/dn790557.aspx).
>
>

Tüm kaynakları Azure Kaynak Yöneticisi'ni kullanarak bunu görevleri Azure aşağıdaki adımları kullanarak Active Directory'e ile kimliğinin doğrulanması gerekir.

* Azure Active Directory Kiracı için bir uygulama ekleyin.
* Eklediğiniz uygulama izinlerini ayarlayın.
* İstekleri için Azure Resource Manager kimlik doğrulaması için belirteç alın.

İlk adım, bir Azure Active Directory uygulaması oluşturmaktır. İçine oturum [Klasik Azure portalı](http://manage.windowsazure.com/) API Management hizmetiniz içeren aboneliğinden örneği ve gidin **uygulamaları** sekmesi, varsayılan olarak Azure Active Directory için.

> [!NOTE]
> Azure Active Directory varsayılan dizin hesabınıza görünür durumda değilse, hesabınız için gerekli izinleri vermek için Azure aboneliği yöneticisine başvurun.

![Azure Active Directory uygulaması oluşturma][api-management-add-aad-application]

Tıklatın **Ekle**, **kuruluşumun geliştirmekte olduğu bir uygulama Ekle**ve seçin **yerel istemci uygulaması**. Açıklayıcı bir ad girin ve İleri okuna tıklayın. Yer tutucu URL girin `http://resources` için **yeniden yönlendirme URI'si**, gerekli bir alandır ancak değer daha sonra kullanılmaz. Uygulamayı kaydetmek için onay kutusuna tıklayın.

Uygulama kaydedildikten sonra tıklatın **Yapılandır**, aşağı kaydırarak **diğer uygulamalara izinler** bölüm ve'ı tıklatın **uygulama eklemek**.

![İzinleri ekleme][api-management-aad-permissions-add]

Seçin **Windows** **Azure Hizmet Yönetimi API'si** ve uygulama eklemek için onay kutusunu işaretleyin.

![İzinleri ekleme][api-management-aad-permissions]

' I tıklatın **izinlere temsilci** yeni eklenen yanında **Windows** **Azure Hizmet Yönetimi API'si** uygulama için kutuyu **erişim Azure Hizmet Yönetimi (Önizleme)**, tıklatıp **kaydetmek**.

![İzinleri ekleme][api-management-aad-delegated-permissions]

Yedekleme oluşturmak ve bunu geri yüklemeyi API'lerini çağırmadan önce bir belirteç almak gereklidir. Aşağıdaki örnek kullanır [Microsoft.IdentityModel.Clients.activedirectory tarafından](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) belirtecini almak için nuget paketi.

```c#
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

Değiştir `{tentand id}`, `{application id}`, ve `{redirect uri}` aşağıdaki yönergeleri kullanarak.

Değiştir `{tenant id}` oluşturduğunuz Azure Active Directory Uygulama Kiracı kimliği. Tıklatarak kimliği erişebilirsiniz **uç noktaları görüntülemek**.

![Uç Noktalar][api-management-aad-default-directory]

![Uç Noktalar][api-management-endpoint]

Değiştir `{application id}` ve `{redirect uri}` kullanarak **istemci kimliği** ve URL'den **yeniden yönlendirme URI'ler** Azure Active Directory uygulamanızın bölümünden **yapılandırma** sekmesi.

![Kaynaklar][api-management-aad-resources]

Belirtilen değerler sonra kod örneği bir belirteç aşağıdaki örneğe benzer şekilde döndürmelidir.

![Belirteç][api-management-arm-token]

Aşağıdaki bölümlerde açıklanan yedekleme ve geri yükleme işlemleri çağırmadan önce yetkilendirme isteği başlığı REST çağrısı için ayarlayın.

```c#
request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);
```

## <a name="step1"></a>Bir API Management hizmeti yedekleyin
Aşağıdaki HTTP isteği bir API Management hizmet sorunu yedeklemek için:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

Burada:

* `subscriptionId`-çalıştığınız API Management hizmeti içeren abonelik kimliğini yedekleme
* `resourceGroupName`-'Api - varsayılan-{hizmeti-bölge}' biçiminde bir dize nerede `service-region` burada API Management hizmeti Azure bölgelerini tanımlayan çalıştığınız yedekleme barındırılan, örn.`North-Central-US`
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

Yedekleme tamamlamak için birden çok dakika sürebilir uzun süren bir işlemdir.  İstek başarılı oldu ve yedekleme işlemi başlatıldı, alırsınız bir `202 Accepted` yanıt durum koduyla bir `Location` üstbilgi.  'GET istekleri URL'ye' olun `Location` işlemi durumunu öğrenmek için üstbilgi. Yedekleme işlemi devam ederken '202 kabul edilen' durum kodunu almaya devam eder. Bir yanıt kodu, `200 OK` yedekleme işlemi başarılı şekilde tamamlandığını gösterir.

Lütfen aşağıdaki kısıtlamalar yedekleme isteği yapılırken unutmayın.

* **Kapsayıcı** istek gövdesinde belirtilen **bulunmalıdır**.
* Yedekleme, sürerken **herhangi bir hizmet yönetim işlemini çalışmamalısınız** SKU yükseltme veya indirgeme, etki alanı adı değişikliği gibi vb.
* Geri yükleme bir **yedekleme yalnızca 30 gün boyunca garanti** oluşturulduktan andan itibaren.
* **Kullanım verileri** analytics raporları oluşturmak için kullanılan **eklenmedi** yedekleme. Kullanım [Azure API Management REST API] [ Azure API Management REST API] analytics raporları güvenli olarak saklamak için düzenli aralıklarla alınamadı.
* Hizmet yedeklemeler gerçekleştirmek sıklığı, kurtarma noktası hedefi etkiler. Bu en aza indirmek için biz düzenli yedeklemeler uygulama yanı sıra API Management hizmetiniz için önemli değişiklikler yaptıktan sonra isteğe bağlı yedeklemeleri gerçekleştirmek öneriyoruz.
* **Değişiklikleri** hizmet yapılandırmasını (örneğin API'leri, ilkeleri, Geliştirici Portalı Görünüm) yedekleme sırasında yapılan işlemdir işleminde **yedekleme işlemine dahil edilmeyen ve bu nedenle kaybolacak**.

## <a name="step2"></a>Bir API Management hizmeti geri yükleme
Önceden oluşturulmuş bir yedek hizmetinden bir API Management geri yüklemek için aşağıdaki HTTP isteği olun:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

Burada:

* `subscriptionId`-içine bir yedeği geri yüklediğiniz API Management hizmeti içeren abonelik kimliği
* `resourceGroupName`-'Api - varsayılan-{hizmeti-bölge}' biçiminde bir dize nerede `service-region` içine bir yedeği geri yüklediğiniz API Management hizmeti barındırıldığı, Azure bölgesi örneğin tanımlar`North-Central-US`
* `serviceName`-hizmet uygulamasına geri yükleniyor, oluşturma sırasında belirtilen API Management adı
* `api-version`-değiştirin`2014-02-14`

İstek gövdesinde, yani Azure depolama hesabı adı, erişim tuşu, blob kapsayıcı adı ve yedek adı yedekleme dosyasının konumunu belirtin:

```
'{  
    storageAccount : {storage account name for the backup},  
    accessKey : {access key for the account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

Değerini `Content-Type` istek üstbilgisi `application/json`.

Geri yüklemeyi tamamlamak için en az 30 dakika sürebileceğini uzun süren bir işlemdir.  İstek başarılı oldu ve geri yükleme işlemi başlatıldı, alırsınız bir `202 Accepted` yanıt durum koduyla bir `Location` üstbilgi.  'GET istekleri URL'ye' olun `Location` işlemi durumunu öğrenmek için üstbilgi. Geri yükleme işlemi devam ederken '202 kabul edilen' almaya devam edeceksiniz durum kodu. Bir yanıt kodu, `200 OK` geri yükleme işlemi başarılı şekilde tamamlandığını gösterir.

> [!IMPORTANT]
> **SKU** uygulamasına geri yükleniyor hizmetinin **eşleşmelidir** SKU yedeklenen hizmet geri yükleniyor.
>
> **Değişiklikleri** yaptığınız geri yükleme sırasında hizmet yapılandırma (örneğin API'leri, ilkeleri, Geliştirici Portalı Görünüm) için işlem devam ediyor **üzerine yazılabilir**.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Yedekleme/geri yükleme işleminin iki farklı izlenecek yol aşağıdaki Microsoft blogları göz atın.

* [Azure API Management hesapları Çoğalt](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/)
  * Bu makalede her katkısı için Gisela için teşekkür ederiz.
* [Azure API Management: Yedekleme ve yapılandırmasını geri yükleme](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
  * Ancak çok ilginç Stuart tarafından ayrıntılı yaklaşım resmi Kılavuzu eşleşmiyor.

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
