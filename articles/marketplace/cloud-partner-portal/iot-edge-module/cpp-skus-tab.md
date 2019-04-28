---
title: Azure IOT Edge modülü SKU'ları | Microsoft Docs
description: SKU'ları için bir IOT Edge modülü oluşturun.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: pbutlerm
ms.openlocfilehash: 370d8160661c1f73124151a3a49d0bb3170dfb77
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60910996"
---
# <a name="iot-edge-module-skus-tab"></a>IOT Edge modülü SKU'ları sekmesi

**SKU'ları** sekmesinde **yeni teklif** sayfası, bir veya daha fazla SKU'ları oluşturma ve bunları yeni teklifinizi ilişkilendirme olanak tanır.  Bir çözümü özellik kümeleri, faturalandırma modelleri veya başka bir özellik tarafından ayırt etmek için farklı SKU'ları kullanabilirsiniz.

## <a name="sku-settings"></a>SKU ayarları

Yeni bir teklif oluşturma başlattığınızda, bir teklifle ilgili SKU'ları yok. Yeni bir SKU'ya oluşturmak için aşağıdaki adımları izleyin:

- Üzerinde **IOT Edge modülleri > Yeni Teklif** sayfasında **SKU'ları** sekmesi.
- SKU'ları seçin **+ yeni SKU** iletişim kutusunu açın.

  ![IOT Edge modülleri için yeni teklif sekmesinde yeni SKU düğmesi](./media/iot-edge-module-skus-tab-new-sku.png)

- Üzerinde **yeni SKU** iletişim kutusunda, SKU için bir tanıtıcı girin ve ardından **Tamam**.
(Aşağıdaki tabloda tanımlayıcı adlandırma kuralları sağlar.)

**SKU'ları** sekmesi yenilendiğini ve SKU yapılandırmak için düzenleme alanları görüntüler. Gerekli alan adına eklenmiş bir yıldız işareti (*) gösterir.

|  **Alan**       |     **Açıklama**                                                          |
|  ---------       |     ---------------                                                          |
| **SKU KİMLİĞİ**       | Bu SKU için tanımlayıcı. Bu ada sahip en fazla 50 karakterden oluşan, küçük harf alfasayısal karakterler veya tire (-), ancak bir kısa çizgi ile bitemez. **Not:** Teklif yayımlanma sonra bu adı değiştiremezsiniz. Adı, ürün URL'lerinde herkese görünür. |

## <a name="sku-details"></a>SKU ayrıntıları

Yapılandırma **SKU ayrıntıları** SKU'nuz Azure Market ve Azure Portal'da Web Siteleri'nde nasıl görüntüleneceğini tanımlamak için.

![IOT Edge modülü sku meta verileri](media/iot-edge-module-skus-tab-metadata.png)

Amaç, içerik ve biçimlendirme altındaki alanlar için aşağıdaki tabloda açıklanmıştır **SKU ayrıntıları**.

|  **Alan**       |     **Açıklama**                                                          |
|  ---------       |     ---------------                                                          |
| **Başlık**        | Bu SKU için başlık. En fazla 50 karakter uzunluğunda. <br/> Azure portalında gösterilir ve varsayılan bir modül adı (olmadan, boşluk ve özel karakterler) olarak kullanılacak ne zaman dağıtılır. Tam olarak bu alanı görüntülendiği görmek için aşağıdaki resimler bakın.|
| **Özet**      | Bu SKU kısa özeti. En fazla 100 karakter uzunluğunda. Yapmak **değil** teklif, SKU özetler.  Bu Özet Azure Marketi'nde gösterilir. Tam olarak bu alanı görüntülendiği görmek için aşağıdaki resimler bakın.|
| **Açıklama**  | Bu SKU kısa açıklaması. En fazla 3000 karakter uzunluğunda. Bu teklif, ancak bu SKU tanımlamaz. Azure Marketi'nde ve Azure portalında gösterilir. Azure portalında Market sekmede tanımlanan teklif açıklayan Market açıklamasında eklenir.  SKU Özet ile aynı olabilir. Tam olarak bu alanı görüntülendiği görmek için aşağıdaki resimler bakın.|
| **Bu SKU Gizle** | Varsayılan ayar tutmak **Hayır**. |

### <a name="sku-example"></a>SKU örnek

 Aşağıdaki örneklerde gösterildiği nasıl SKU **başlık**, **özeti**, ve **açıklama** alanları farklı görünümlerde gösterilir.
 
#### <a name="on-the-azure-marketplace-website"></a>Azure Marketi Web sitesinde:

- SKU ayrıntılarını da ararken:

    ![Azure Marketi Web sitesinde SKU'ları nasıl görüntüleneceğini](media/iot-edge-module-ampdotcom-pdp-plans.png)

#### <a name="on-the-azure-portal-website"></a>Azure Portalı Web sitesinde:

- SKU'ları göz atarken:

    ![#1 Azure portalına göz atarken IOT Edge modülü nasıl gösterilir](media/iot-edge-module-portal-browse.png)

    ![#2 Azure portalına göz atarken IOT Edge modülü nasıl gösterilir](media/iot-edge-module-portal-product-picker.png)

- SKU'ları için arama yaparken:

    ![Azure portalında arama yaparken IOT Edge modülü nasıl gösterilir](media/iot-edge-module-portal-search.png)

- SKU ayrıntılarını da ararken:

    ![Ürün ayrıntılarını portalda ararken IOT Edge modülü nasıl gösterilir](./media/iot-edge-module-portal-pdp.png)

- Modül dağıtırken:
    
    ![Dağıtılan, IOT Edge modülü nasıl gösterilir](./media/iot-edge-module-deployment.png)

## <a name="sku-content"></a>SKU içeriği

Altında **Edge modül görüntüleri**, ihtiyacımız, IOT Edge modülü yüklemek bilgileri sağlayın.

Bize erişim sağlamak, [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) (ACR), IOT Edge modülü, böylece biz de karşıya yükleyin ve bunu onaylamak görüntü içeren. Yayımlandıktan sonra IOT Edge modülü kopyalanır ve Azure Marketi tarafından barındırılan bir ortak container Registry'yi kullanarak dağıtılmış.

Birden fazla platformu hedefleyin ve etiketleri aracılığıyla çeşitli sürümleri sağlamalısınız. Daha fazla bilgi edinin [etiketleri ve sürüm oluşturma "hazırlama, IOT Edge modülü teknik varlıkları"](./cpp-create-technical-assets.md).

![IOT Edge modül görüntüleri](./media/iot-edge-module-skus-tab-acr.png)

Amaç, içeriği, aşağıdaki tabloda açıklanmıştır ve alanların biçimlendirme:

- **Görüntü deposu ayrıntıları**
- **Görüntü sürümü**

|  **Alan**       |     **Açıklama**                                                          |
|  ---------       |     ---------------                                                          |
|  ***Görüntü deposu ayrıntıları***   |  |
| **Abonelik kimliği**        | ACR Azure abonelik kimliği.|
| **Kaynak grubu adı**      | Kaynak grubunu, ACR adıdır.|
| **Kayıt defteri adı**  | ACR kayıt defterinizin adı. Yalnızca kayıt defteri adı, oturum açma sunucu adını kopyalayın (örneğin, olmadan `azurecr.io`.) |
| **Depo adı**  | Depo, IOT Edge modülü içerir, ACR adı. **Not:** Daha sonra adı ayarlandıktan sonra değiştirilemez. Diğer herhangi bir teklif hesabınızda aynı ada sahip olmak için benzersiz bir ad kullanın. |
| **Kullanıcı Adı** | ACR (yönetici kullanıcı adı) ile ilişkili kullanıcı adı. |
| **Parola** | ACR ile ilişkili parola. |
|  ***Görüntü sürümü***   |  |
| **Görüntü etiket veya Özet** | En az içermelidir bir `latest` etiketi ve sürüm etiketi (örneğin, başlayarak `xx.xx.xx-` burada xx, bir sayı). Olmaları gerektiği [bildirim etiketleri](https://github.com/estesp/manifest-tool) birden çok platformu hedefleyecek şekilde. Biz bunları yüklemek için bir bildirim etiketi tarafından başvurulan tüm etiketleri de eklenmelidir. Etiketleri kullanarak bir IOT Edge modülü çeşitli sürümlerini ekleyebilirsiniz. Tüm etiketleri bildirim (dışında `latest`) ile başlamalıdır `X.Y-` veya `X.Y.Z-` X, Y, Z tamsayılar olduğu. Daha fazla bilgi edinin [etiketleri ve sürüm oluşturma "hazırlama, IOT Edge modülü teknik varlıkları"](./cpp-create-technical-assets.md). <br/> Örneğin, bir `latest` etiketi, işaret ettiği noktalarına `1.0.1-linux-x64`, `1.0.1-linux-arm32`,, ve `1.0.1-windows-arm32`bu 6 etiketleri burada eklenmesi gerekiyor. |

### <a name="help-your-customers-launch-your-iot-edge-module-by-using-default-settings"></a>Müşterileriniz, IOT Edge modülü başlatma varsayılan ayarları kullanarak Yardım

IOT Edge modülü dağıtmak için en sık kullanılan ayarları tanımlar. Müşteri dağıtımları, IOT Edge modülü,-hazır Bu varsayılanlar ile Başlat vererek iyileştirin.

![Dağıtım, IOT Edge modülü varsayılan ayarları](./media/iot-edge-module-skus-tab-iot-edge-defaults.png)

Amaç, içeriği, aşağıdaki tabloda açıklanmıştır ve alanların biçimlendirme **varsayılan yollar**, **varsayılan çiftinin istenen özelliklerini**, **varsayılan ortam değişkenlerini**, ve **varsayılan CreateOptions**.

|  **Alan**       |     **Açıklama**                                                          |
|  ---------       |     ---------------                                                          |
| **Varsayılan yollar**        | Her bir varsayılan rota adı ve değeri az 512 karakter olmalıdır. En fazla 5 varsayılan yol tanımlayabilirsiniz. Doğru kullandığınızdan emin olun [yol söz dizimi](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes) , rota değeri. Modülünüzün için başvurmak için olacak varsayılan modül adı kullanın, **SKU başlık** alanları ve özel karakterler olmadan. Henüz bilinen diğer modüllerle başvurmak için kullanma `<FROM_MODULE_NAME>` bu bilgileri güncelleştirmek gerektiğini bildirin, müşterilerin, kapasitelerinde kuralı. Daha fazla bilgi edinin [IOT Edge yönlendiren](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes). <br/> Örneğin, Modülü `ContosoModule` girdiler için dinlediği `ContosoInput` ve çıktı veri `ContosoOutput`, aşağıdaki 2 varsayılan rotaları tanımlamak için mantıklıdır:<br/>-Adı #1: `ToContosoModule`<br/>-#1 değeri:`FROM /messages/modules/<FROM_MODULE_NAME>/outputs/* INTO BrokeredEndpoint("/modules/ContosoModule/inputs/ContosoInput")`<br/>-Adı #2: `FromContosoModuleToCloud`<br/>-#2 değeri: `FROM /messages/modules/ContonsoModule/outputs/ContosoOutput INTO $upstream`<br/>  |
| **Varsayılan çiftinin istenen özelliklerini**      | Her varsayılan çiftinin istenen özelliklerini ad ve değer az 512 karakter olmalıdır. En fazla 5 ad/değer çiftinin istenen özelliklerini tanımlayabilirsiniz. Geçerli JSON, olmayan kaçış, Dizileri olmadan ve 4'ün en fazla iç içe hiyerarşisini çiftinin istenen özelliklerini değerleri olmalıdır. Daha fazla bilgi edinin [çiftinin istenen özelliklerini](https://docs.microsoft.com/azure/iot-edge/module-composition#define-or-update-desired-properties). <br/> Bir modül çiftinin istenen özelliklerini aracılığıyla dinamik olarak yapılandırılabilir yenileyerek destekliyorsa, örneğin, aşağıdaki varsayılan çiftinin istenen özelliği tanımlamak için mantıklıdır:<br/> -Adı #1: `RefreshRate`<br/>-#1 değeri: `60`|
| **Varsayılan ortam değişkenleri**  | Her bir varsayılan ortam değişkenleri adı ve değeri az 512 karakter olmalıdır. En fazla 5 ad/değer ortam değişkenlerini tanımlayabilirsiniz. <br/>Örneğin, bir modül Başlatılmakta olan önce kullanım koşulları kabul etmesini gerektiriyorsa, aşağıdaki ortam değişkenlerini tanımlayabilirsiniz:<br/> -Adı #1: `ACCEPT_EULA`<br/>-#1 değeri: `Y`|
| **Varsayılan createOptions**  | CreateOptions az 512 karakter olmalıdır. Atlanan geçerli JSON olmalıdır. Daha fazla bilgi edinin [createOptions](https://docs.microsoft.com/azure/iot-edge/module-composition#configure-modules). <br/> Bir modül gerektiriyorsa bir bağlantı noktası, örneğin, bağlama aşağıdaki createOptions tanımlayabilirsiniz:<br/>  `"HostConfig":{"PortBindings":{"5012/tcp":[{"HostPort":"5012"}]}`|

<br/> Seçin **Kaydet** SKU ayarlarınızı kaydetmek için. 

## <a name="next-steps"></a>Sonraki adımlar

Kullanım [Marketi sekmesinden](./cpp-marketplace-tab.md) teklifiniz için bir Market açıklama oluşturmak için.
