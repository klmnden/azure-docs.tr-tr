---
title: Azure CDN ile SAS kullanarak | Microsoft Docs
description: Azure CDN, paylaşılan erişim imzası (özel depolama kapsayıcıları sınırlı erişim vermek için SAS) destekler.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: magattus
ms.openlocfilehash: 7edf0a9f8d4eb4c01b6d80fd82a1061b6cbb1e35
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60324173"
---
# <a name="using-azure-cdn-with-sas"></a>Azure CDN ile SAS kullanma

Bir depolama hesabı için Azure Content Delivery ağ (içeriği önbelleğe almak için varsayılan olarak kullanmak üzere CDN), depolama kapsayıcıları için URL'leri bilen herhangi ayarladığınızda, yüklediğiniz dosyalara erişebilirsiniz. Depolama hesabınızda dosyaları korumak için özel depolama kapsayıcıları Genelden erişimini ayarlayabilirsiniz. Bunu yaparsanız, ancak hiç dosyalarınıza erişmek mümkün olacaktır. 

Özel depolama kapsayıcılarına sınırlı erişim vermek istiyorsanız, Azure depolama hesabınızın Paylaşılan Erişim İmzası (SAS) özelliğini kullanabilirsiniz. Bir SAS, hesap anahtarınızı açığa çıkarmadan Azure Depolama kaynaklarınıza kısıtlı erişim hakları veren bir URI'dir. Bir SAS ile depolama hesabı anahtarınızı güvenmiyorsanız istemcilere ancak belirli depolama hesabı kaynaklarına erişimi devretmek istediğiniz sağlayabilir. Bu istemcilere paylaşılan erişim imzası URI'si dağıtarak, bunları bir kaynak için belirtilen bir süre için erişim.
 
Bir SAS ile başlangıç ve bitiş zamanlarını izinleri (okuma/yazma) ve IP aralıkları gibi bir bloba erişim çeşitli parametrelerin tanımlayabilirsiniz. Bu makalede, Azure CDN ile birlikte SAS kullanmayı açıklar. Bunu ve parametre seçeneklerini nasıl oluşturulacağı da dahil olmak üzere, SAS hakkında daha fazla bilgi için bkz. [paylaşılan erişim imzaları (SAS) kullanma](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).

## <a name="setting-up-azure-cdn-to-work-with-storage-sas"></a>Azure CDN, SAS depolamasıyla çalışmayı ayarlama
Azure CDN ile SAS kullanmak için aşağıdaki üç seçenek önerilir. Tüm seçenekleri, çalışan bir SAS (önkoşullara bakın) zaten oluşturduğunuzu varsayalım. 
 
### <a name="prerequisites"></a>Önkoşullar
Başlamak için depolama hesabı oluşturma ve ardından varlığınız için bir SAS oluşturun. Oluşturabileceğiniz iki tür saklı erişim imzaları: SAS hizmet veya hesap SAS ise bir. Daha fazla bilgi için [paylaşılan erişim imzaları türleri](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#types-of-shared-access-signatures).

Bir SAS belirteci ürettiğiniz sonra ekleyerek, blob depolama dosyanızı erişebilirsiniz `?sv=<SAS token>` URL'niz için. Bu URL'si aşağıdaki biçime sahiptir: 

`https://<account name>.blob.core.windows.net/<container>/<file>?sv=<SAS token>`
 
Örneğin:
 ```
https://democdnstorage1.blob.core.windows.net/container1/demo.jpg?sv=2017-07-29&ss=b&srt=co&sp=r&se=2038-01-02T21:30:49Z&st=2018-01-02T13:30:49Z&spr=https&sig=QehoetQFWUEd1lhU5iOMGrHBmE727xYAbKJl5ohSiWI%3D
```

Ayar parametreleri hakkında daha fazla bilgi için bkz. [SAS parametresi konuları](#sas-parameter-considerations) ve [paylaşılan erişim imzası parametreleri](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#shared-access-signature-parameters).

![CDN SAS ayarları](./media/cdn-sas-storage-support/cdn-sas-settings.png)

### <a name="option-1-using-sas-with-pass-through-to-blob-storage-from-azure-cdn"></a>1. seçenek: SAS ile doğrudan blob depolama alanına Azure CDN kullanma

Bu seçenek en basit olduğundan ve Azure CDN from kaynak sunucuya geçirilir tek bir SAS belirteci kullanır.
 
1. Bir uç nokta seçin, **önbelleğe alma kuralları**, ardından **her benzersiz URL'yi önbelleğe al** gelen **sorgu dizesini önbelleğe alma** listesi.

    ![CDN önbelleğe alma kuralları](./media/cdn-sas-storage-support/cdn-caching-rules.png)

2. Depolama hesabınızdaki SA'ları ayarladıktan sonra dosyaya erişmek için SAS belirteci ile CDN uç noktası ve kaynak sunucu URL'leri kullanmalısınız. 
   
   Sonuçta elde edilen CDN uç nokta URL'si aşağıdaki biçime sahiptir: `https://<endpoint hostname>.azureedge.net/<container>/<file>?sv=<SAS token>`

   Örneğin:   
   ```
   https://demoendpoint.azureedge.net/container1/demo.jpg/?sv=2017-07-29&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
   
3. Önbelleğe alma süresi, önbelleğe alma kuralları kullanarak veya ekleyerek ince ayar `Cache-Control` kaynak sunucudaki üst bilgileri. Azure CDN, SAS belirteci düz bir sorgu dizesi olarak davrandığı için en iyi uygulama, ya da SAS süre önce sona bir önbelleğe alma süresi ayarlamanız gerekir. Aksi takdirde, bir dosya SAS etkin olan daha uzun bir süre için önbelleğe alınmışsa SAS süre geçtikten sonra dosya Azure CDN kaynak sunucudan erişilebilir olarak olabilir. Bu durum oluşursa, ve, önbelleğe alınan dosyanızı erişilemez hale getirmek istediğiniz bir dosyanın önbellekten temizlemek için temizleme işlemi gerçekleştirmeniz gerekir. Azure CDN'de önbelleğe alma süresi ayarlama hakkında daha fazla bilgi için bkz: [denetimi Azure CDN önbelleğe alma kuralları ile önbelleğe alma davranışını](cdn-caching-rules.md).

### <a name="option-2-hidden-cdn-sas-token-using-a-rewrite-rule"></a>2. seçenek: Bir yeniden yazma kuralı kullanarak gizli CDN SAS belirteci
 
Bu seçenek yalnızca kullanılabilir **verizon'dan Azure CDN Premium** profilleri. Bu seçenek belirtilmişse, kaynak sunucuda blob depolama güvenliğini sağlayabilirsiniz. Dosya için özel erişim kısıtlamaları gerekmez ancak kullanıcıların doğrudan Azure CDN boşaltma sürelerini geliştirmek için depolama kaynak erişimini engellemek istiyorsanız bu seçeneği kullanmak isteyebilirsiniz. Kaynak sunucu, belirtilen kapsayıcıdaki dosyalara erişen herkesin kullanıcıya bilinmiyor, SAS belirteci gereklidir. Ancak, URL yeniden yazma kuralı nedeniyle CDN uç noktasında bir SAS belirteci gerekli değildir.
 
1. Kullanım [kurallar altyapısı](cdn-rules-engine.md) bir URL yeniden yazma kuralı oluşturun. Yeni kurallar yaymak için 4 saat yararlanın.

   ![CDN'yi yönetmek düğmesi](./media/cdn-sas-storage-support/cdn-manage-btn.png)

   ![Düğme CDN kural altyapısı](./media/cdn-sas-storage-support/cdn-rules-engine-btn.png)

   Aşağıdaki örnek URL yeniden yazma kuralı bir yakalama grubu ve adlı bir uç nokta ile yalnızca bir normal ifade deseni kullanılmaktadır *sasstoragedemo*:
   
   Kaynak:   
   `(container1\/.*)`
   
   Hedef:   
   ```
   $1?sv=2017-07-29&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
   ![Kural - sol CDN URL yeniden yazma](./media/cdn-sas-storage-support/cdn-url-rewrite-rule.png)
   ![CDN URL yeniden yazma kuralı - sağ](./media/cdn-sas-storage-support/cdn-url-rewrite-rule-option-4.png)

2. Yeni kuralın etkin hale geldikten sonra herkes, URL'de bir SAS belirteci olup olmadığını kullanıyorsanız bağımsız olarak CDN uç noktasında belirtilen kapsayıcıdaki dosyalara erişebilirsiniz. Biçim şu şekildedir: `https://<endpoint hostname>.azureedge.net/<container>/<file>`
 
   Örneğin:   
   `https://sasstoragedemo.azureedge.net/container1/demo.jpg`
       

3. Önbelleğe alma süresi, önbelleğe alma kuralları kullanarak veya ekleyerek ince ayar `Cache-Control` kaynak sunucudaki üst bilgileri. Azure CDN, SAS belirteci düz bir sorgu dizesi olarak davrandığı için en iyi uygulama, ya da SAS süre önce sona bir önbelleğe alma süresi ayarlamanız gerekir. Aksi takdirde, bir dosya SAS etkin olan daha uzun bir süre için önbelleğe alınmışsa SAS süre geçtikten sonra dosya Azure CDN kaynak sunucudan erişilebilir olarak olabilir. Bu durum oluşursa, ve, önbelleğe alınan dosyanızı erişilemez hale getirmek istediğiniz bir dosyanın önbellekten temizlemek için temizleme işlemi gerçekleştirmeniz gerekir. Azure CDN'de önbelleğe alma süresi ayarlama hakkında daha fazla bilgi için bkz: [denetimi Azure CDN önbelleğe alma kuralları ile önbelleğe alma davranışını](cdn-caching-rules.md).

### <a name="option-3-using-cdn-security-token-authentication-with-a-rewrite-rule"></a>Seçenek 3: Bir yeniden yazma kuralı ile CDN güvenlik belirteci kimlik doğrulamasını kullanma

Azure CDN güvenlik belirteci kimlik doğrulamasını kullanmak için olmalıdır bir **verizon'dan Azure CDN Premium** profili. Bu, en güvenli ve özelleştirilebilir seçenektir. İstemci erişimi güvenlik belirteci üzerinde ayarladığınız güvenlik parametreleri temel alır. Oluşturulan ve güvenlik belirtecini ayarlayın sonra tüm CDN uç nokta URL'leri gerekecektir. Ancak, URL yeniden yazma kuralı nedeniyle CDN uç noktasında bir SAS belirteci gerekli değildir. SAS belirteci daha sonra geçersiz hale gelirse, Azure CDN artık kaynak sunucusundan içerik düzeltin mümkün olacaktır.

1. [Bir Azure CDN güvenlik belirteci oluşturmak](https://docs.microsoft.com/azure/cdn/cdn-token-auth#setting-up-token-authentication) ve kurallar altyapısı yol ve CDN uç noktası için kullanıcılarınızın dosya erişebileceğiniz kullanarak etkinleştirin.

   Bir güvenlik belirteç uç noktası URL'si aşağıdaki biçime sahiptir:   
   `https://<endpoint hostname>.azureedge.net/<container>/<file>?<security_token>`
 
   Örneğin:   
   ```
   https://sasstoragedemo.azureedge.net/container1/demo.jpg?a4fbc3710fd3449a7c99986bkquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
       
   Parametre seçenekleri bir güvenlik belirteci kimlik doğrulaması için bir SAS belirteci parametre seçeneklerini farklıdır. Bir güvenlik belirteci oluşturduğunuzda, sona erme süresini kullanmayı seçerseniz, SAS belirteci süre sonu zamanı aynı değere ayarlamanız gerekir. Bunun yapılması, sona erme süresini tahmin edilebilir olmasını sağlar. 
 
2. Kullanım [kurallar altyapısı](cdn-rules-engine.md) SAS belirteci kapsayıcıdaki tüm blob'lara erişimi etkinleştirmek için bir URL yeniden yazma kuralı oluşturun. Yeni kurallar yaymak için 4 saat yararlanın.

   Aşağıdaki örnek URL yeniden yazma kuralı bir yakalama grubu ve adlı bir uç nokta ile yalnızca bir normal ifade deseni kullanılmaktadır *sasstoragedemo*:
   
   Kaynak:   
   `(container1\/.*)`
   
   Hedef:   
   ```
   $1&sv=2017-07-29&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
   ![Kural - sol CDN URL yeniden yazma](./media/cdn-sas-storage-support/cdn-url-rewrite-rule.png)
   ![CDN URL yeniden yazma kuralı - sağ](./media/cdn-sas-storage-support/cdn-url-rewrite-rule-option-4.png)

3. SAS yenileme, Url yeniden yazma Kuralı'nı yeni bir SAS belirteci ile güncelleştirdiğinizden emin olun. 

## <a name="sas-parameter-considerations"></a>SAS parametresi konuları

Azure CDN, SAS parametreleri için Azure CDN görünür olmadığından, bunları temel alan teslim davranışını değiştiremezsiniz. Azure CDN, Azure CDN istemciden gelen isteklere için kaynak sunucuya yapan isteklerinden tanımlanan parametre kısıtlamalar geçerlidir. Bu ayrım, SAS parametreleri ayarladığınızda dikkate almak önemlidir. Bu özellikler Gelişmiş gereklidir ve kullanmakta olduğunuz [seçeneği 3](#option-3-using-cdn-security-token-authentication-with-a-rewrite-rule), uygun kısıtlamaları Azure CDN güvenlik belirteci üzerinde güncel olarak ayarlayın.

| SAS parametre adı | Açıklama |
| --- | --- |
| Başlatma | Azure CDN blob dosyasına erişmek için başlayabilirsiniz süre. Saat nedeniyle (bir saat sinyal farklı bileşenleri için farklı zamanlarda geldiğinde) eğriltmek, varlık hemen kullanılabilir olmasını istiyorsanız, daha önce 15 dakika seçin. |
| Bitiş | Saat sonra Azure CDN blob dosyası artık erişemez. Daha önce Azure cdn'de önbelleğe alınan dosyalar hala erişilebilir. Dosya süre sonu zamanı denetlemek için Azure CDN güvenlik belirteci üzerinde güncel uygun sona erme saati ayarlamak veya varlık temizleme. |
| İzin verilen IP adresleri | İsteğe bağlı. Kullanıyorsanız **verizon'dan Azure CDN**, tanımlanan aralıklar için bu parametreyi ayarlayın [Azure CDN from Verizon uç sunucu IP aralıkları](/azure/cdn/cdn-pop-list-api). Kullanıyorsanız **akamai'den Azure CDN**, IP adreslerini statik olduğundan IP aralıkları parametresi ayarlanamıyor.|
| İzin verilen protokoller | Hesap SAS'si ile yapılan bir istek için izin verilen protokoller:. HTTPS ayarı önerilir.|

## <a name="next-steps"></a>Sonraki adımlar

SAS hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
- [Paylaşılan erişim imzaları (SAS) kullanma](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
- [Paylaşılan erişim imzaları, bölüm 2: Oluşturma SAS ve Blob Depolama ile kullanma](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2)

Belirteç kimlik doğrulaması ayarlama hakkında daha fazla bilgi için bkz. [belirteç kimlik doğrulaması ile güvenli hale getirme Azure Content Delivery Network varlıklarının](https://docs.microsoft.com/azure/cdn/cdn-token-auth).
