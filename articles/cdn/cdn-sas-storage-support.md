---
title: Azure CDN ile SAS kullanarak | Microsoft Docs
description: Azure CDN, paylaşılan erişim imzası (özel depolama kapsayıcıları sınırlı erişim vermek için SAS) destekler.
services: cdn
documentationcenter: ''
author: dksimpson
manager: ''
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: v-deasim
ms.openlocfilehash: dcae29c49035775cd9ff983bbc99bab06c7f16dc
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="using-azure-cdn-with-sas"></a>Azure CDN ile SAS kullanma

Bir depolama hesabı için Azure içerik teslim ağı (önbellek içeriği için varsayılan olarak kullanmak üzere CDN), depolama kapsayıcıları için URL'leri bilen herhangi ayarladığınızda, yüklediğiniz dosyalarına erişebilir. Depolama hesabınızdaki dosyaları korumak için depolama kapsayıcılardan ortak özel erişim ayarlayabilirsiniz. Bunu yaparsanız, ancak hiç kimsenin dosyalarınıza erişmek kuramaz. 

Özel depolama kapsayıcıları sınırlı erişim vermek istiyorsanız, Azure depolama hesabınızın paylaşılan erişim imzası (SAS) özelliğini kullanabilirsiniz. Bir SAS verir hesap anahtarınızı sokmadan Azure depolama kaynaklarınıza erişim hakları kısıtlı bir URI değil. Bir SAS depolama hesabı anahtarınızı ile güveniyor musunuz istemcilere ancak belirli depolama hesabı kaynaklarına erişimi devretmek istediğiniz sağlayabilir. Bu istemciler için paylaşılan erişim imzası URI dağıtarak, bunları bir kaynağa erişimi belirli bir süre boyunca verin.
 
Bir SAS ile başlangıç ve bitiş zamanlarını, izinlerine (okuma/yazma) ve IP aralıkları gibi bir blob için erişim çeşitli parametreleri tanımlayabilirsiniz. Bu makalede, Azure CDN ile birlikte SAS kullanmayı açıklar. SAS ve parametre seçeneklerini oluşturma dahil olmak üzere hakkında daha fazla bilgi için bkz: [kullanarak paylaşılan erişim imzaları (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).

## <a name="setting-up-azure-cdn-to-work-with-storage-sas"></a>Azure CDN SAS depolama ile çalışacak şekilde ayarlama
Aşağıdaki üç seçenekten SAS Azure CDN ile kullanmak için önerilir. Tüm seçenekleri, çalışan bir SAS (önkoşullara bakın) zaten oluşturduğunuzu varsayalım. 
 
### <a name="prerequisites"></a>Önkoşullar
Başlatmak için bir depolama hesabı oluşturun ve ardından, varlık için bir SAS oluşturun. Oluşturabileceğiniz iki tür depolanmış erişim imzaları: SAS hizmet ya da hesap SAS. Daha fazla bilgi için bkz: [paylaşılan erişim imzaları türlerini](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#types-of-shared-access-signatures).

Bir SAS belirteci ürettiğiniz sonra ekleyerek blob depolama dosyanızı erişebilirsiniz `?sv=<SAS token>` , URL. Bu URL'si aşağıdaki biçime sahiptir: 

`https://<account name>.blob.core.windows.net/<container>/<file>?sv=<SAS token>`
 
Örneğin:
 ```
https://democdnstorage1.blob.core.windows.net/container1/demo.jpg?sv=2017-04-17&ss=b&srt=co&sp=r&se=2038-01-02T21:30:49Z&st=2018-01-02T13:30:49Z&spr=https&sig=QehoetQFWUEd1lhU5iOMGrHBmE727xYAbKJl5ohSiWI%3D
```

Parametreleri ayarlama hakkında daha fazla bilgi için bkz: [SAS parametre konuları](#sas-parameter-considerations) ve [paylaşılan erişim imzası parametreleri](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#shared-access-signature-parameters).

![CDN SAS ayarları](./media/cdn-sas-storage-support/cdn-sas-settings.png)

### <a name="option-1-using-sas-with-pass-through-to-blob-storage-from-azure-cdn"></a>Seçenek 1: SAS ile doğrudan blob depolama Azure CDN kullanarak

Bu seçenek en kolayıdır ve Azure CDN ' kaynak sunucunun geçirilen tek bir SAS belirteci kullanır. Tarafından desteklenen **verizon'dan Azure CDN standart** ve **akamai'den Azure CDN standart** profilleri. 
 
1. Bir uç nokta seçin, **kuralları önbelleğe alma**seçeneğini belirleyip **her benzersiz URL'yi önbelleğe** gelen **sorgu dizesini önbelleğe alma** listesi.

    ![CDN önbelleğe alma kuralları](./media/cdn-sas-storage-support/cdn-caching-rules.png)

2. Depolama hesabınıza SA'ları ayarladıktan sonra dosyaya erişmek için CDN uç noktası ve kaynak sunucu URL'ler ile SAS belirteci kullanmalısınız. 
   
   Sonuçta elde edilen CDN uç noktası URL'si aşağıdaki biçime sahiptir: `https://<endpoint hostname>.azureedge.net/<container>/<file>?sv=<SAS token>`

   Örneğin:   
   ```
   https://demoendpoint.azureedge.net/container1/demo.jpg/?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
   
3. Önbellek süresi önbelleğe alma kurallarını kullanarak veya ekleyerek ince ayar `Cache-Control` kaynak sunucuda üstbilgileri. Azure CDN SAS belirteci düz sorgu dizesi olarak davrandığı için en iyi uygulama olarak noktada veya SAS sona erme süresi sona bir önbelleğe alma süresi ayarlamanız gerekir. Aksi halde, bir dosya SAS etkin olduğundan daha uzun bir süre önbelleğe alınır, SAS sona erme süresi dolduktan sonra dosyayı Azure CDN kaynak sunucudan erişilebilir olarak olabilir. Bu durum ortaya çıkar ve önbelleğe alınmış dosya erişilemez hale getirmek istediğiniz, bir dosyanın önbellekten temizlemek için temizleme işlemi gerçekleştirmeniz gerekir. Azure CDN önbellek süresini ayarlama hakkında daha fazla bilgi için bkz: [denetim Azure CDN kuralları önbelleğe alma ile önbelleğe alma davranışı](cdn-caching-rules.md).

### <a name="option-2-hidden-cdn-sas-token-using-a-rewrite-rule"></a>Seçenek 2: bir yeniden yazma kuralı kullanarak gizli CDN SAS belirteci
 
Bu seçenek yalnızca için kullanılabilir **verizon'dan Azure CDN Premium** profilleri. Bu seçenek ile kaynak sunucuda blob depolama güvenliğini sağlayabilirsiniz. Özel erişim kısıtlamaları için dosya gerekmez ancak kullanıcıların doğrudan Azure CDN boşaltma süresini kısaltmak için depolama kaynak erişimini engellemek istiyorsanız bu seçeneği kullanmak isteyebilirsiniz. Kullanıcıya bilinmiyor SAS belirteci herkes kaynak sunucu, belirtilen kapsayıcıda dosyalara erişmek için gereklidir. Ancak, URL yeniden yazma kuralı nedeniyle CDN uç noktası üzerinde SAS belirteci gerekli değildir.
 
1. Kullanım [kurallar altyapısı](cdn-rules-engine.md) bir URL yeniden yazma kuralı oluşturun. Yeni kurallar yaymak için yaklaşık 90 dakika sürebilir.

   ![CDN yönetmek düğmesi](./media/cdn-sas-storage-support/cdn-manage-btn.png)

   ![CDN kurallar altyapısı düğmesi](./media/cdn-sas-storage-support/cdn-rules-engine-btn.png)

   Aşağıdaki örnek URL yeniden yazma kuralı bir yakalama grubunu ve adlı bir uç nokta ile bir normal ifade deseni kullanır *storagedemo*:
   
   Kaynak:   
   `(/test/.*)`
   
   Hedef:   
   ```
   $1?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```

   ![CDN URL yeniden yazma kuralı](./media/cdn-sas-storage-support/cdn-url-rewrite-rule-option-2.png)

2. Yeni Kural etkin hale geldikten sonra herkes belirtilen kapsayıcıda bunlar URL'de bir SAS belirteci olup olmadığını kullandığınızdan bağımsız olarak CDN uç noktası üzerindeki dosyalara erişebilir. Biçimi şöyledir: `https://<endpoint hostname>.azureedge.net/<container>/<file>`
 
   Örneğin:   
   `https://demoendpoint.azureedge.net/container1/demo.jpg`
       

3. Önbellek süresi önbelleğe alma kurallarını kullanarak veya ekleyerek ince ayar `Cache-Control` kaynak sunucuda üstbilgileri. Azure CDN SAS belirteci düz sorgu dizesi olarak davrandığı için en iyi uygulama olarak noktada veya SAS sona erme süresi sona bir önbelleğe alma süresi ayarlamanız gerekir. Aksi halde, bir dosya SAS etkin olduğundan daha uzun bir süre önbelleğe alınır, SAS sona erme süresi dolduktan sonra dosyayı Azure CDN kaynak sunucudan erişilebilir olarak olabilir. Bu durum ortaya çıkar ve önbelleğe alınmış dosya erişilemez hale getirmek istediğiniz, bir dosyanın önbellekten temizlemek için temizleme işlemi gerçekleştirmeniz gerekir. Azure CDN önbellek süresini ayarlama hakkında daha fazla bilgi için bkz: [denetim Azure CDN kuralları önbelleğe alma ile önbelleğe alma davranışı](cdn-caching-rules.md).

### <a name="option-3-using-cdn-security-token-authentication-with-a-rewrite-rule"></a>Seçenek 3: Bir yeniden yazma kuralı ile CDN güvenlik belirteci kimlik doğrulaması kullanma

Azure CDN güvenlik belirteci kimlik doğrulaması kullanmak için olmalıdır bir **verizon'dan Azure CDN Premium** profili. Bu, en güvenli ve özelleştirilebilir seçenektir. İstemci erişimi için güvenlik belirteci ayarlayın güvenlik parametreleri temel alır. Oluşturulan ve güvenlik belirteci ayarlayın sonra tüm CDN uç nokta URL'lerinin gerekir. Ancak, URL yeniden yazma kuralı nedeniyle CDN uç noktası üzerinde SAS belirteci gerekli değildir. SAS belirteci daha sonra geçersiz hale gelirse, Azure CDN artık kaynak sunucusundan içerik düzeltin mümkün olmayacaktır.

1. [Bir Azure CDN güvenlik belirteci oluşturma](https://docs.microsoft.com/azure/cdn/cdn-token-auth#setting-up-token-authentication) ve kurallar altyapısı yol ve CDN uç noktası için kullanıcılarınızın dosya erişebileceğiniz kullanarak etkinleştirin.

   Bir güvenlik belirteci uç nokta URL'si aşağıdaki biçime sahiptir:   
   `https://<endpoint hostname>.azureedge.net/<container>/<file>?<security_token>`
 
   Örneğin:   
   ```
   https://demoendpoint.azureedge.net/container1/demo.jpg?a4fbc3710fd3449a7c99986bkquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
       
   Parametre seçenekleri bir güvenlik belirteci kimlik doğrulaması için bir SAS belirteci parametre seçenekleri farklıdır. Bir güvenlik belirteci oluştururken bir sona erme zamanı kullanmayı seçerseniz, SAS belirteci süre sonu zamanı aynı değere ayarlamanız gerekir. Bunun yapılması, sona erme zamanı tahmin edilebilir olmasını sağlar. 
 
2. Kullanım [kurallar altyapısı](cdn-rules-engine.md) kapsayıcıdaki tüm blob'lara SAS belirteci erişimi etkinleştirmek için bir URL yeniden yazma kuralı oluşturun. Yeni kurallar yaymak için yaklaşık 90 dakika sürebilir.

   Aşağıdaki örnek URL yeniden yazma kuralı bir yakalama grubunu ve adlı bir uç nokta ile bir normal ifade deseni kullanır *storagedemo*:
   
   Kaynak:   
   `(/test/.*)`
   
   Hedef:   
   ```
   $1&sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```

   ![CDN URL yeniden yazma kuralı](./media/cdn-sas-storage-support/cdn-url-rewrite-rule-option-3.png)

3. SAS yenileyin, yeni SAS belirteci Url yeniden yazma kuralı güncelleştirdiğinizden emin olun. 

## <a name="sas-parameter-considerations"></a>SAS parametre konuları

SAS parametreleri Azure CDN görünür olduğundan Azure CDN üzerlerinde göre teslim davranışını değiştiremezsiniz. Azure CDN istemci istekleri için kaynak sunucu için Azure CDN isteklerinde tanımlanan parametre kısıtlamalar uygulanır. Bu ayrım SAS parametreleri ayarladığınızda dikkate almak önemlidir. Bu gelişmiş özellikleri gereklidir ve kullanmakta olduğunuz [seçeneği 3](#option-3-using-cdn-security-token-authentication-with-a-rewrite-rule), uygun kısıtlamaları Azure CDN güvenlik belirteci ayarlayın.

| SAS parametre adı | Açıklama |
| --- | --- |
| Başlatma | Azure CDN blobu dosyaya erişmek için başlayabilirsiniz süre. Saat nedeniyle (bir saat sinyal farklı bileşenler için farklı zamanlarda geldiğinde) eğme, varlık hemen kullanılabilir olmasını istiyorsanız, bir bekleme süresi 15 dakikadan daha önce seçin. |
| End | Saat geçmesi Azure CDN blob dosya artık erişemez. Daha önce Azure CDN önbelleğe alınan dosyaları hala erişilebilir. Dosya süre sonu zamanı denetlemek için uygun sona erme saati Azure CDN güvenlik belirteci ayarlayın veya varlık Temizle. |
| İzin verilen IP adresi | İsteğe bağlı. Kullanıyorsanız **verizon'dan Azure CDN**, tanımlı aralıklar için bu parametreyi ayarlayın [Azure CDN Verizon kenar sunucu IP aralıklarından](https://msdn.microsoft.com/library/mt757330.aspx). Kullanıyorsanız **akamai'den Azure CDN**, IP adreslerini statik olduğundan IP aralıkları parametresi ayarlanamıyor.|
| İzin verilen protokoller | Hesap SAS ile yapılan bir istek için izin verilen protokollerini. HTTPS ayarı önerilir.|

## <a name="see-also"></a>Ayrıca bkz.
- [Paylaşılan erişim imzaları (SAS) kullanma](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
- [Paylaşılan erişim imzası, bölüm 2: Oluşturma ve bir SAS Blob storage'ı kullanma](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2)
- [Azure içerik teslim ağı varlıklar belirteci kimlik doğrulaması ile güvenli hale getirme](https://docs.microsoft.com/azure/cdn/cdn-token-auth)
