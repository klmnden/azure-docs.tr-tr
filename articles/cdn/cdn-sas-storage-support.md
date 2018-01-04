---
title: Azure CDN ile SAS kullanarak | Microsoft Docs
description: 
services: cdn
documentationcenter: 
author: dksimpson
manager: 
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2017
ms.author: rli v-deasim
ms.openlocfilehash: de30f4319be75362131f8c8ad71aad57b0528f05
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="using-azure-cdn-with-sas"></a>Azure CDN ile SAS kullanma

Depolama hesabınızın depolama kapsayıcısından içerik sunmanızı, depolama kapsayıcısı için özel erişim vererek kullanıcıların dosyalarınızı nasıl erişebileceğiniz güvenli hale getirmek isteyebilirsiniz. Aksi takdirde, genel erişim verilen bir depolama kapsayıcısı URL'sini bilen herkes tarafından erişilebilir. Erişmek içerik teslim ağı (CDN) izin verdiğiniz bir depolama hesabı korumak için özel depolama kapsayıcıları sınırlı erişim vermek için Azure storage paylaşılan erişim imzası (SAS) özelliğini kullanabilirsiniz.

Bir SAS verir hesap anahtarınızı sokmadan Azure depolama kaynaklarınıza erişim hakları kısıtlı bir URI değil. Bir SAS depolama hesabı anahtarınızı ile güveniyor musunuz istemcilere ancak belirli depolama hesabı kaynaklarına erişimi devretmek istediğiniz sağlayabilir. Bu istemciler için paylaşılan erişim imzası URI dağıtarak, bunları bir kaynağa erişimi belirli bir süre boyunca verin.
 
SAS, başlangıç ve bitiş zamanlarını, izinlerine (okuma/yazma) ve IP aralıkları gibi bir blob için erişim çeşitli parametreleri tanımlamanızı sağlar. Bu makalede, Azure CDN ile birlikte SAS kullanmayı açıklar. SAS ve parametre seçeneklerini oluşturma dahil olmak üzere hakkında daha fazla bilgi için bkz: [kullanarak paylaşılan erişim imzaları (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).

## <a name="setting-up-azure-cdn-to-work-with-storage-sas"></a>Azure CDN SAS depolama ile çalışacak şekilde ayarlama
Aşağıdaki üç seçenekten SAS Azure CDN ile kullanmak için önerilir. Tüm seçenekleri, çalışan bir SAS (önkoşullara bakın) zaten oluşturduğunuzu varsayalım. 
 
### <a name="prerequisites"></a>Önkoşullar
Başlatmak için bir depolama hesabı oluşturun ve ardından, varlık için bir SAS oluşturun. Oluşturabileceğiniz iki tür depolanmış erişim imzaları: SAS hizmet ya da hesap SAS. Daha fazla bilgi için bkz: [paylaşılan erişim imzaları türlerini](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#types-of-shared-access-signatures).

Bir SAS oluşturduktan sonra blob depolama dosyanızın aşağıdaki biçimde bir URL ile erişebilirsiniz:`https://<account>.blob.core.windows.net/<folder>/<file>?sv=<SAS_TOKEN>`
 
Örneğin:
 ```
https://democdnstorage1.blob.core.windows.net/container1/sasblob.txt?sv=2017-04-17&ss=b&srt=co&sp=r&se=2038-01-02T21:30:49Z&st=2018-01-02T13:30:49Z&spr=https&sig=QehoetQFWUEd1lhU5iOMGrHBmE727xYAbKJl5ohSiWI%3D
```

Parametreleri ayarlama hakkında daha fazla bilgi için bkz: [SAS parametre konuları](#sas-parameter-considerations) ve [paylaşılan erişim imzası parametreleri](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#shared-access-signature-parameters).

![CDN SAS ayarları](./media/cdn-sas-storage-support/cdn-sas-settings.png)

### <a name="option-1-using-sas-with-pass-through-to-blob-storage-from-the-cdn"></a>Seçenek 1: SAS BLOB depolamaya CDN ile doğrudan kullanma

Bu seçenek en kolayıdır ve CDN kaynak sunucusuna iletilen yalnızca bir tek SAS belirtecini kullanır. Tarafından desteklenen **verizon'dan Azure CDN**, standart ve Premium profilleri ve **akamai'den Azure CDN**. 
 
1. Bir uç nokta seçin, **kuralları önbelleğe alma**seçeneğini belirleyip **her benzersiz URL'yi önbelleğe** gelen **sorgu dizesini önbelleğe alma** listesi.

    ![CDN önbelleğe alma kuralları](./media/cdn-sas-storage-support/cdn-caching-rules.png)

2. Depolama hesabınıza SA'ları ayarladıktan sonra SAS belirteci ile CDN URL'sine dosyaya erişmek için kullanın. 
   
   Sonuçta elde edilen URL'si aşağıdaki biçime sahiptir:`https://<endpoint>.azureedge.net/<folder>/<file>?sv=<SAS_TOKEN>`

   Örneğin:   
   ```
   https://demoendpoint.azureedge.net/test/demo.jpg/?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
   
3. Önbellek süresi önbelleğe alma kurallarını kullanarak veya ekleyerek ince ayar `Cache-Control` kaynağa üstbilgileri. CDN SAS belirteci düz sorgu dizesi olarak davrandığı için en iyi uygulama olarak noktada veya SAS sona erme süresi sona bir önbelleğe alma süresi ayarlamanız gerekir. Aksi halde, bir dosya SAS etkin olduğundan daha uzun bir süre önbelleğe alınır, SAS sona erme süresi dolduktan sonra dosya CDN kaynak sunucudan erişilebilir olarak olabilir. Bu durum oluşur ve önbelleğe alınmış dosya erişilemez hale getirmek istediğiniz, bir dosyanın önbellekten temizlemek için temizleme işlemi gerçekleştirmeniz gerekir. CDN üzerinde önbellek süresini ayarlama hakkında daha fazla bilgi için bkz: [denetim Azure içerik teslim ağı kuralları önbelleğe alma ile önbelleğe alma davranışı](cdn-caching-rules.md).

### <a name="option-2-hidden-cdn-security-token-using-rewrite-rule"></a>Seçenek 2: yeniden yazma kuralı kullanarak gizli CDN güvenlik belirteci
 
Bu seçenek ile CDN kullanıcı için bir SAS belirteci gerektirmeden kaynak blob depolama güvenliğini sağlayabilirsiniz. Özel erişim kısıtlamaları için dosya gerekmez ancak kullanıcıların doğrudan CDN boşaltma süresini kısaltmak için depolama kaynak erişimini engellemek istiyorsanız bu seçeneği kullanmak isteyebilirsiniz. Bu seçenek yalnızca için kullanılabilir **verizon'dan Azure CDN Premium** profilleri. 
 
1. Kullanım [kurallar altyapısı](cdn-rules-engine.md) bir URL yeniden yazma kuralı oluşturun. Yeni kurallar yaymak için yaklaşık 90 dakika sürebilir.

   ![CDN yönetmek düğmesi](./media/cdn-sas-storage-support/cdn-manage-btn.png)

   ![CDN kurallar altyapısı düğmesi](./media/cdn-sas-storage-support/cdn-rules-engine-btn.png)

   Bu örnek URL yeniden yazma kuralı aşağıdaki desenleri sahiptir:
   
   Kaynak:   
   `/test/demo.jpg`
   
   Hedef:   
   `/test/demo.jpg?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D`

   ![CDN URL yeniden yazma kuralı](./media/cdn-sas-storage-support/cdn-url-rewrite-rule.png)
 
2. Şu biçimde SAS belirteci olmadan, CDN dosyada erişebilirsiniz:`https://<endpoint>.azureedge.net/<folder>/<file>`
 
   Örneğin:   
   `https://demoendpoint.azureedge.net/test/demo.jpg`
       
   Bir SAS belirteci olup olmadığını kullanımından bağımsız olarak herhangi bir CDN uç noktasına erişebildiğinden emin unutmayın. 

3. Önbellek süresi önbelleğe alma kurallarını kullanarak veya ekleyerek ince ayar `Cache-Control` kaynağa üstbilgileri. CDN SAS belirteci düz sorgu dizesi olarak davrandığı için en iyi uygulama olarak noktada veya SAS sona erme süresi sona bir önbelleğe alma süresi ayarlamanız gerekir. Aksi halde, bir dosya SAS etkin olduğundan daha uzun bir süre önbelleğe alınır, SAS sona erme süresi dolduktan sonra dosya CDN kaynak sunucudan erişilebilir olarak olabilir. Bu durum oluşur ve önbelleğe alınmış dosya erişilemez hale getirmek istediğiniz, bir dosyanın önbellekten temizlemek için temizleme işlemi gerçekleştirmeniz gerekir. CDN üzerinde önbellek süresini ayarlama hakkında daha fazla bilgi için bkz: [denetim Azure içerik teslim ağı kuralları önbelleğe alma ile önbelleğe alma davranışı](cdn-caching-rules.md).

### <a name="option-3-using-cdn-security-token-authentication-with-a-rewrite-rule"></a>Seçenek 3: Bir yeniden yazma kuralı ile CDN güvenlik belirteci kimlik doğrulaması kullanma

Bu, en güvenli ve özelleştirilebilir seçenektir. CDN güvenlik belirteci kimlik doğrulaması kullanmak için olmalıdır bir **verizon'dan Azure CDN Premium** profili. İstemci erişimi için CDN güvenlik belirteci ayarlanan güvenlik parametreleri temel alır. Ancak, SAS geçersiz hale gelirse, CDN kaynak sunucusundan içerik düzeltin mümkün olmayacaktır.

1. [CDN güvenlik belirteci oluşturma](https://docs.microsoft.com/azure/cdn/cdn-token-auth#setting-up-token-authentication) ve kurallar altyapısı yol ve CDN uç noktası için kullanıcılarınızın dosya erişebileceğiniz kullanarak etkinleştirin.

   Bir SAS URL'si aşağıdaki biçime sahiptir:   
   `https://<endpoint>.azureedge.net/<folder>/<file>?sv=<SAS_TOKEN>`
 
   Örneğin:   
   ```
   https://demoendpoint.azureedge.net/test/demo.jpg?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
       
   Parametre seçenekleri bir CDN güvenlik belirteci kimlik doğrulaması için bir SAS belirteci parametre seçenekleri farklıdır. Zaman aşımı değeri bir CDN güvenlik belirteci oluştururken kullanmayı seçerseniz, SAS belirteci süre sonu zamanı aynı değere ayarlayın. Bunun yapılması, sona erme zamanı tahmin edilebilir olmasını sağlar. 
 
2. Kullanım [kurallar altyapısı](cdn-rules-engine.md) kapsayıcıdaki tüm blob'lara belirteç erişimi etkinleştirmek için bir URL yeniden yazma kuralı oluşturmak için. Yeni kurallar yaymak için yaklaşık 90 dakika sürebilir.

   Bu örnek URL yeniden yazma kuralı aşağıdaki desenleri sahiptir:
   
   Kaynak:   
   `/test/demo.jpg`
   
   Hedef:   
   `/test/demo.jpg?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D`

   ![CDN URL yeniden yazma kuralı](./media/cdn-sas-storage-support/cdn-url-rewrite-rule.png)

3. SAS yenilediğinizde yeni SAS belirteci kullanmak için Url yeniden yazma kuralı güncelleştirin. 

## <a name="sas-parameter-considerations"></a>SAS parametre konuları

SAS parametreleri CDN ile görünür olduğundan CDN üzerlerinde göre teslim davranışını değiştiremezsiniz. CDN istemci istekleri için kaynak sunucuya CDN yapan isteklerinde tanımlanan parametre kısıtlamalar uygulanır. Bu ayrım SAS parametreleri ayarladığınızda dikkate almak önemlidir. Bu gelişmiş özellikleri gereklidir ve kullanmakta olduğunuz [seçeneği 3](#option-3-using-cdn-security-token-authentication-with-a-rewrite-rule), uygun kısıtlamaları CDN güvenlik belirteci ayarlayın.

| SAS parametre adı | Açıklama |
| --- | --- |
| Başlatma | Blob dosyaya erişmek için CDN başlayabilirsiniz süre. Saat nedeniyle (saat sinyal farklı bileşenler için farklı zamanlarda geldiğinde) eğme, süresi 15 dakika önceki varlık hemen kullanılabilir olmasını istiyorsanız seçin. |
| Son | Saat geçmesi CDN blob dosya artık erişemez. Daha önce CDN önbelleğe alınan dosyaları hala erişilebilir. Dosya süre sonu zamanı denetlemek için uygun sona erme saati CDN güvenlik belirteci ayarlayın veya varlık Temizle. |
| İzin verilen IP adresi | İsteğe bağlı. Kullanıyorsanız **verizon'dan Azure CDN**, tanımlı aralıklar için bu parametreyi ayarlayın [Azure CDN Verizon kenar sunucu IP aralıklarından](https://msdn.microsoft.com/library/mt757330.aspx). Kullanıyorsanız **akamai'den Azure CDN**, IP adreslerini statik olduğundan IP aralıkları parametresi ayarlanamıyor.|
| İzin verilen protokoller | Hesap SAS ile yapılan bir istek için izin verilen protokollerini. HTTPS ayarı önerilir.|

## <a name="see-also"></a>Ayrıca bkz.
- [Paylaşılan erişim imzaları (SAS) kullanma](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
- [Paylaşılan erişim imzası, bölüm 2: Oluşturma ve bir SAS Blob storage'ı kullanma](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2)
- [Azure içerik teslim ağı varlıklar belirteci kimlik doğrulaması ile güvenli hale getirme](https://docs.microsoft.com/azure/cdn/cdn-token-auth)
