---
title: Azure Application Gateway - Azure portalı ile HTTP istek ve yanıt üstbilgileri yeniden | Microsoft Docs
description: Azure portalında bir Azure Application Gateway HTTP üstbilgilerinde isteklerinin ve yanıtlarının ağ geçidi üzerinden geçen yeniden yapılandırmak için kullanmayı öğrenin
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 04/10/2019
ms.author: absha
ms.custom: mvc
ms.openlocfilehash: 6afc07f98905469b06622e7829ec4a215b94845e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60716257"
---
# <a name="rewrite-http-request-and-response-headers-with-azure-application-gateway---azure-portal"></a>Azure Application Gateway - Azure portalı ile HTTP istek ve yanıt üstbilgileri yeniden yazma

Bu makalede yapılandırmak için Azure portalını kullanmayı gösterir bir [Application Gateway v2 SKU](<https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant>) HTTP üstbilgileri istek ve yanıtların yeniden yazma için.

> [!IMPORTANT]
> Otomatik ölçeklendirme yapan ve alanlar arası yedekli uygulama ağ geçidi SKU'su şu anda genel önizleme aşamasındadır. Bu önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

Üst bilgiyi yeniden beri özellik SKU v1 SKU için desteklenmiyor bir uygulama ağ geçidi v2 olması gerekir. V2 SKU yoksa, oluşturun bir [Application Gateway v2 SKU](https://docs.microsoft.com/azure/application-gateway/tutorial-autoscale-ps) başlamadan önce.

## <a name="what-is-required-to-rewrite-a-header"></a>Üstbilgi yeniden yazmak için gerekli nedir

HTTP üst bilgisi yeniden yapılandırmak için ihtiyacınız:

1. Http üstbilgileri yeniden yazmak için gereken yeni nesneleri oluşturun:

   - **Eylem yeniden**: istek ve yeniden yazmak için istediğinize isteği üst bilgi alanları ve özgün üstbilgileri için yazılması gereken yeni değeri belirtmek için kullanılır. Bir veya daha fazla yeniden yazma koşulu bir yeniden yazma eylemi ile ilişkilendirilecek seçebilirsiniz.

   - **Koşulu yeniden**: Bu isteğe bağlı bir yapılandırmadır. bir yeniden yazma koşul eklenirse, HTTP (S) istekleri ve yanıtları içeriğini değerlendirir. HTTP (S) istek veya yanıtı yeniden yazma koşulu ile eşleşen olmadığını yeniden yazma koşulu ile ilişkili yeniden yazma eylemi yürütme kararı bağlı olacaktır. 

     Birden fazla koşullar ile ilişkili bir eylem, ardından eylem yalnızca tüm koşulları, yani karşılandığında yürütülecek, mantıksal bir AND işlemi gerçekleştirilir.

   - **Kuralı yeniden**: yeniden yazma kuralı içeren birden çok yeniden yazma eylemi - koşul birleşimleri yeniden yazın.

   - **Kural dizisi**:, farklı bir yeniden yazma kuralları sırayla yürütülen belirlemeye yardımcı olur. Bu, bir yeniden yazma kümesinde birden çok yeniden yazma kuralları gerektiğinde yararlıdır. Daha düşük kuralı dizisi değeri ile yeniden yazma kuralı önce yürütülen. İki yeniden yazma kuralları için aynı kural dizisi sağlarsanız, yürütme sırası belirleyici olacaktır.

   - **Kümesi yeniden**: İstek yönlendirme kuralı ile ilişkilendirilecek birden çok yeniden yazma kuralları içerir.

2. Bir yönlendirme kuralı ile yeniden eklemek için gerekli olacaktır. Yeniden yapılandırma yönlendirme kuralı aracılığıyla kaynak dinleyicisine bağlı olmasıdır. Temel bir yönlendirme kuralını kullanırken, üstbilgi yeniden yapılandırma kaynağı dinleyici ile ilişkili ve genel üstbilgi yeniden yazma. Yola dayalı kural kullanıldığında, URL yolu haritada üstbilgi yeniden yapılandırma tanımlanır. Bu nedenle, yalnızca bir sitenin belirli bir yol alanı için geçerlidir.

Birden çok http üst bilgisi yeniden yazma kümeleri oluşturabilir ve her bir yeniden yazma kümesi birden çok dinleyici uygulanabilir. Ancak, bir yeniden yazma yalnızca belirli bir dinleyici kümesine uygulayabilirsiniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure hesabınızla [Azure portalında](https://portal.azure.com/) oturum açın.

## <a name="configure-header-rewrite"></a>Üstbilgi yeniden yapılandırma

Bu örnekte biz location üst bilgisini arka uç uygulama tarafından gönderilen http yanıtında yazarak yeniden yönlendirme URL'sini değiştirir. 

1. Seçin **tüm kaynakları**ve ardından uygulama ağ geçidi'ni seçin.

2. Seçin **yeniden yazar** sol menüden.

3. Tıklayarak **+ yeniden kümesi**. 

   ![Yeniden yazma kümesi Ekle](media/rewrite-http-headers-portal/add-rewrite-set.png)

4. Yeniden ayarlamak için bir ad sağlayın ve yönlendirme kuralı ile ilişkilendirin:

   - Kümesinde yeniden yazma adını **adı** metin.
   - Listelenen bir veya daha fazla kuralları seçin **yönlendirme kuralları ilişkili** listesi. Diğer yeniden yazma kümeleri ile ilişkili olan bu kuralları yalnızca seçebilirsiniz. Zaten başka bir yeniden yazma kümeleri ile ilişkili olan kuralları gri.
   - İleri'ye tıklayın.
   
     ![Ad ve ilişkilendirme ekleyin](media/rewrite-http-headers-portal/name-and-association.png)

5. Bir yeniden yazma kuralı oluşturun:

   - Tıklayarak **+ yeniden yazma Kuralı Ekle**.![ Yeniden yazma Kuralı Ekle](media/rewrite-http-headers-portal/add-rewrite-rule.png)
   - Yeniden yazma kuralı yeniden yazma kuralı adı metin kutusuna bir ad ve kural dizisi sağlar.![Kural adı ekleyin](media/rewrite-http-headers-portal/rule-name.png)

6. Yalnızca "azurewebsites.net" başvuru içeriyorsa bu örnekte biz location üst bilgisini yeniden. Bunu yapmak için azurewebsites.net yanıt location üst bilgisini içeren olup olmadığını değerlendirmek için bir koşul ekleyin:

   - Tıklayarak **+ koşul Ekle** bölümle tıklayın **varsa** bunu genişletmek için yönergeleri![ Kural adı ekleyin](media/rewrite-http-headers-portal/add-condition.png)

   - Seçin **HTTP üstbilgisi** gelen **kontrol etmek için değişken türünü** açılır. 

   - Seçin **üstbilgi türü** olarak **yanıt**.

   - Bu örnekte, biz, select ortak bir üstbilgi özelleştirmede konum üst bilgisi değerlendirirsiniz beri **ortak üstbilgisi** radyo düğmesi olarak **üst bilgi adı**.

   - Seçin **konumu** gelen **ortak üstbilgisi** açılır.

   - Seçin **Hayır** olarak **büyük/küçük harfe** ayarı.

   - Seçin **eşittir (=)** gelen **işleci** açılır.

   - Normal ifade deseni girin. Bu örnekte, deseni kullanacağız `(https?):\/\/.*azurewebsites\.net(.*)$` .

   - **Tamam** düğmesine tıklayın.

     ![Location üst bilgisini değiştirin](media/rewrite-http-headers-portal/condition.png)

7. Location üst bilgisini yeniden yazma için bir eylem ekleyin:

   - Seçin **ayarlamak** olarak **eylem türü**.

   - Seçin **yanıt** olarak **üstbilgi türü**.

   - Seçin **ortak üstbilgisi** olarak **üst bilgi adı**.

   - Seçin **konumu** gelen **ortak üstbilgisi** açılır.

   - Üstbilgi değeri girin. Bu örnekte, kullanacağız `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` üstbilgi değeri. Bu değiştirecek *azurewebsites.net* ile *contoso.com* konum üst bilgisi içinde.

   - **Tamam** düğmesine tıklayın.

     ![Location üst bilgisini değiştirin](media/rewrite-http-headers-portal/action.png)

8. Tıklayarak **Oluştur** yeniden oluşturmak için ayarlayın.

   ![Location üst bilgisini değiştirin](media/rewrite-http-headers-portal/create.png)

9. Yeniden yazma kümesi görünümüne yönlendirileceksiniz. Yukarıda oluşturduğunuz yeniden yazma kümesi yeniden yazma ayarlar listesinde var olduğundan emin olun.

   ![Location üst bilgisini değiştirin](media/rewrite-http-headers-portal/rewrite-set-list.png)

## <a name="next-steps"></a>Sonraki adımlar

Çalışmaları kullanın bazı yaygın gerçekleştirmek için gerekli yapılandırma hakkında daha fazla bilgi edinmek için [ortak üstbilgisi yeniden senaryoları](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).

   
