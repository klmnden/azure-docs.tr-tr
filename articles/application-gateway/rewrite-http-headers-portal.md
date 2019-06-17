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
ms.openlocfilehash: e144214a58f9fe383cf4edd878554792d9d6a6f9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64947159"
---
# <a name="rewrite-http-request-and-response-headers-with-azure-application-gateway---azure-portal"></a>Azure Application Gateway - Azure portalı ile HTTP istek ve yanıt üstbilgileri yeniden yazma

Bu makalede, Azure portalını yapılandırmak için kullanmayı açıklar bir [Application Gateway v2 SKU](<https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant>) yeniden yazma isteklerini ve yanıtlarını HTTP üstbilgileri için örneği.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için bir uygulama ağ geçidi v2 SKU örnek olması gerekir. Üst bilgileri yeniden yazma v1 SKU'da desteklenmiyor. V2 SKU yoksa, oluşturun bir [Application Gateway v2 SKU](https://docs.microsoft.com/azure/application-gateway/tutorial-autoscale-ps) başlamadan önce örneği.

## <a name="create-required-objects"></a>Gerekli nesnelerden oluşturma

HTTP üst bilgisi yeniden yapılandırmak için bu adımları tamamlamak gerekir.

1. HTTP üst bilgisi yeniden yazmak için gerekli olan nesneleri oluşturun:

   - **Eylem yeniden**: İstek, yeniden yazma için istediğinize isteği üst bilgi alanları ve üst bilgileri için yeni değeri belirtmek için kullanılır. Bir ilişkilendirme veya daha fazla bir yeniden yazma eylemi koşullarla yeniden yazma.

   - **Koşulu yeniden**: İsteğe bağlı yapılandırma. HTTP (S) istekleri ve yanıtları içeriğini yeniden yazma koşulları değerlendirin. HTTP (S) istek veya yanıtı yeniden yazma koşulu ile eşleşirse yeniden yazma eylemi meydana gelir.

     Birden fazla koşulu bir eylem ile ilişkilendirirseniz, yalnızca tüm koşulları sağlandığında eylem gerçekleşir. Diğer bir deyişle, bir mantıksal AND işlemi işlemdir.

   - **Kuralı yeniden**: Birden çok yeniden yazma eylemi içeriyor / koşul birleşimleri yeniden yazın.

   - **Kural dizisi**: İçinde yeniden yazma kuralları yürütme sırası belirlemeye yardımcı olur. Bir yeniden yazma kümesinde birden çok yeniden yazma kuralları varsa bu yapılandırma yararlıdır. Bir alt kural sırası değerine sahip bir yeniden yazma kuralı ilk çalıştırır. Yürütme sırası, iki yeniden yazma kuralları için aynı kural dizisi değeri atarsanız, belirleyici değildir.

   - **Kümesi yeniden**: İstek yönlendirme kuralı ile ilişkilendirilecek birden çok yeniden yazma kuralları içerir.

2. Yönlendirme kural kümesi yeniden ekleyin. Yeniden yapılandırma, kaynak dinleyicisi aracılığıyla yönlendirme kuralı eklenir. Temel yönlendirme kuralı kullandığınızda, üstbilgi yeniden yapılandırma kaynağı dinleyici ile ilişkili ise genel üstbilgi yeniden yazma. Yola dayalı kural kullandığınızda, URL yolu haritada üstbilgi yeniden yapılandırma tanımlanır. Bu durumda, yalnızca bir sitenin belirli yolu alanını geçerlidir.

Birden çok HTTP üst bilgisi yeniden yazma kümeleri oluşturabilir ve birden çok dinleyici ayarlamak her yeniden uygulayın. Ancak bir yeniden yazma yalnızca belirli bir dinleyici kümesine uygulayabilirsiniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure hesabınızla [Azure portalında](https://portal.azure.com/) oturum açın.

## <a name="configure-header-rewrite"></a>Üstbilgi yeniden yapılandırma

Bu örnekte, bir arka uç uygulama tarafından gönderilen HTTP yanıtında location üst bilgisini yazarak biz bir yeniden yönlendirme URL'si değiştireceksiniz.

1. Seçin **tüm kaynakları**ve ardından uygulama ağ geçidi'ni seçin.

2. Seçin **yeniden yazar** sol bölmesinde.

3. Seçin **yeniden kümesi**:

   ![Yeniden yazma kümesi Ekle](media/rewrite-http-headers-portal/add-rewrite-set.png)

4. Yeniden yazma küme için bir ad girin ve yönlendirme kuralı ile ilişkilendirin:

   - Kümesinde yeniden yazma için bir ad girin **adı** kutusu.
   - Bir veya daha fazla listelenen kuralları seçin **yönlendirme kuralları ilişkili** listesi. Diğer yeniden yazma kümeleriyle ilişkilendirilmiş henüz kuralları seçebilirsiniz. Zaten başka bir yeniden yazma kümeleri ile ilişkili olan kurallar soluklaştırılır.
   - **İleri**’yi seçin.
   
     ![Ad ve ilişkilendirme ekleyin](media/rewrite-http-headers-portal/name-and-association.png)

5. Bir yeniden yazma kuralı oluşturun:

   - Seçin **yeniden yazma Kuralı Ekle**.

     ![Yeniden yazma Kuralı Ekle](media/rewrite-http-headers-portal/add-rewrite-rule.png)

   - Yeniden yazma kuralı için bir ad girin **yeniden yazma kuralı adı** kutusu. Bir sayı girin **kural dizisi** kutusu.

     ![Yeniden yazma kuralı adı Ekle](media/rewrite-http-headers-portal/rule-name.png)

6. Yalnızca bir başvuru azurewebsites.net içerdiğinde bu örnekte biz location üst bilgisini yeniden. Bunu yapmak için azurewebsites.net yanıt location üst bilgisini içeren olup olmadığını değerlendirmek için bir koşul ekleyin:

   - Seçin **koşul Ekle** seçip kutusu içeren **varsa** genişletmek için yönergeler.

     ![Koşul Ekle](media/rewrite-http-headers-portal/add-condition.png)

   - İçinde **kontrol etmek için değişken türünü** listesinden **HTTP üstbilgisi**.

   - İçinde **üstbilgi türü** listesinden **yanıt**.

   - Bu örnekte biz ortak bir üst bilgisi olan konum üst bilgisi değerlendiriyor çünkü seçin **ortak üstbilgisi** altında **üst bilgi adı**.

   - İçinde **ortak üstbilgisi** listesinden **konumu**.

   - Altında **büyük/küçük harfe**seçin **Hayır**.

   - İçinde **işleci** listesinden **eşittir (=)** .

   - Bir normal ifade deseni belirtin. Bu örnekte, deseni kullanacağız `(https?):\/\/.*azurewebsites\.net(.*)$`.

   - **Tamam**’ı seçin.

     ![Eğer yapılandırma durumu](media/rewrite-http-headers-portal/condition.png)

7. Location üst bilgisini yeniden yazma için bir eylem ekleyin:

   - İçinde **eylem türü** listesinden **ayarlamak**.

   - İçinde **üstbilgi türü** listesinden **yanıt**.

   - Altında **üst bilgi adı**seçin **ortak üstbilgisi**.

   - İçinde **ortak üstbilgisi** listesinden **konumu**.

   - Üstbilgi değeri girin. Bu örnekte, kullanacağız `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` üstbilgi değeri. Bu değer değiştirecek *azurewebsites.net* ile *contoso.com* konum üst bilgisi içinde.

   - **Tamam**’ı seçin.

     ![Eylem ekleme](media/rewrite-http-headers-portal/action.png)

8. Seçin **Oluştur** yeniden oluşturmak için:

   ![Oluştur’u seçin](media/rewrite-http-headers-portal/create.png)

9. Yeniden ayarlama görünümü açılır. Oluşturduğunuz yeniden yazma kümesi yeniden yazma ayarlar listesinde olduğundan emin olun:

   ![Görünümü yeniden ayarlama](media/rewrite-http-headers-portal/rewrite-set-list.png)

## <a name="next-steps"></a>Sonraki adımlar

Bazı ortak kullanım durumları ayarlama hakkında daha fazla bilgi için bkz. [ortak üstbilgisi yeniden senaryoları](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).