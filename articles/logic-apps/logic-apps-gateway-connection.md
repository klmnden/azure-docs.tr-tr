---
title: Azure Logic Apps'ten şirket içi veri kaynaklarına erişim | Microsoft Docs
description: Şirket içi veri kaynağına bir şirket içi veri ağ geçidi oluşturarak logic apps'ten bağlanın
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: arthii, LADocs
ms.topic: article
ms.date: 10/01/2018
ms.openlocfilehash: 2b9e1c153c3fa9b17145eb6c3c8f3ed02e3bf40f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60304201"
---
# <a name="connect-to-on-premises-data-sources-from-azure-logic-apps"></a>Azure Logic Apps'ten şirket içi veri kaynaklarına bağlanın

Mantıksal uygulamalarınızı şirket içi veri kaynaklarına erişmek için Azure portalında bir şirket içi veri ağ geçidi kaynağı oluşturun. Logic apps ardından kullanabilirsiniz [şirket içi Bağlayıcılar](../logic-apps/logic-apps-gateway-install.md#supported-connections). Bu makale, Azure ağ geçidi kaynağı oluşturmak nasıl *sonra* , [ağ geçidini, yerel bilgisayarınızda yükleyip](../logic-apps/logic-apps-gateway-install.md). 

> [!TIP]
> Azure sanal ağlarına bağlanmak için oluşturmayı göz önünde bulundurun bir [ *tümleştirme hizmeti ortamı* ](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) yerine. 

Diğer hizmetlerle ağ geçidi kullanma hakkında daha fazla bilgi için şu makalelere bakın:

* [Microsoft Power BI şirket içi veri ağ geçidi](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
* [Microsoft Flow şirket içi veri ağ geçidi](https://flow.microsoft.com/documentation/gateway-manage/)
* [Microsoft PowerApps şirket içi veri ağ geçidi](https://powerapps.microsoft.com/tutorials/gateway-management/)
* [Azure Analysis Services şirket içi veri ağ geçidi](../analysis-services/analysis-services-gateway.md)

## <a name="prerequisites"></a>Önkoşullar

* Seçtiğiniz zaten [data gateway yerel bir bilgisayara yüklenip](../logic-apps/logic-apps-gateway-install.md).

* Ağ geçidi yüklemenizi zaten bir Azure ağ geçidi kaynağı ile ilişkili değil. Ağ geçidi kaynağı oluşturmak ve ağ geçidi yüklemenizi seçtiğinizde gerçekleşen yalnızca bir ağ geçidi kaynağına, ağ geçidi yüklemenizi bağlayabilirsiniz. Bu bağlantı ağ geçidi yüklemesi diğer kaynaklar için kullanılamaz hale getirir.

* Azure portalında oturum açın ve ağ geçidi kaynağı oluşturmak için önceden kullanılmış olan aynı oturum açma hesabı kullandığınızdan emin olun [şirket içi veri ağ geçidi yükleme](../logic-apps/logic-apps-gateway-install.md#requirements) aynı birlikte [Azure aboneliği ](https://docs.microsoft.com/azure/architecture/cloud-adoption-guide/adoption-intro/subscription-explainer) ağ geçidi yüklemek için kullanıldı. Henüz Azure aboneliğiniz yoksa, <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Oluşturma ve Azure portalında, ağ geçidi kaynağı korumak için [Windows hizmet hesabı](../logic-apps/logic-apps-gateway-install.md#windows-service-account) için en az **katkıda bulunan** izinleri. Şirket içi veri ağ geçidi Windows hizmeti olarak çalışır ve kullanacak şekilde ayarlanmış `NT SERVICE\PBIEgwService` için Windows hizmeti oturum açma kimlik bilgileri. 

  > [!NOTE]
  > Windows için şirket içi verilere bağlanmak için kullanılan hesabı hizmet hesabı farklıdır, kaynakları ve Azure'dan iş veya Okul hesabı bulut hizmetlerinde oturum açmak için kullanılan.

## <a name="download-and-install-gateway"></a>Ağ geçidi yükleyip

Bu makaledeki adımlar ile devam etmeden önce ağ geçidiniz yerel bir bilgisayarda zaten yüklü olduğundan emin olun.
Henüz yapmadıysanız, adımları [şirket içi veri ağ geçidi yükleyip](../logic-apps/logic-apps-gateway-install.md). 

<a name="create-gateway-resource"></a>

## <a name="create-azure-resource-for-gateway"></a>Ağ geçidi için bir Azure kaynağı oluşturma

Yerel bilgisayarda ağ geçidini yükledikten sonra ağ geçidiniz için bir Azure kaynağı sonra oluşturabilirsiniz. Bu adım, Azure aboneliğinizle de, ağ geçidi kaynağı ilişkilendirir.

1. <a href="https://portal.azure.com" target="_blank">Azure Portal</a> oturum açın. Aynı Azure iş veya Okul e-posta adresini ağ geçidi yüklemek için kullanılan emin olun.

2. Ana Azure menüsünde **kaynak Oluştur** > 
**tümleştirme** > **şirket içi veri ağ geçidi**.

   !["Şirket içi veri ağ geçidi" bulun](./media/logic-apps-gateway-connection/find-on-premises-data-gateway.png)

3. Üzerinde **bağlantı ağ geçidi Oluştur** sayfasında, ağ geçidi kaynağınızın bu bilgileri sağlayın:

   | Özellik | Açıklama | 
   |----------|-------------|
   | **Ad** | Ağ geçidi kaynak adı | 
   | **Abonelik** | Azure aboneliğinizin adı, mantıksal uygulamanız ile aynı abonelikte olmalıdır. Varsayılan abonelik Azure hesabında oturum açmak için kullanılan temel alır. | 
   | **Kaynak grubu** | Adı [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) ilgili kaynakları düzenlemek için | 
   | **Konum** | Azure, bu konuma sırasında ağ geçidi bulut hizmeti için seçtiğiniz aynı bölgeye sınırlar [ağ geçidi yüklemesi](../logic-apps/logic-apps-gateway-install.md). <p>**Not**: Ağ geçidi bulut hizmeti konumu bu ağ geçidi kaynak konumu eşleştiğinden emin olun. Aksi takdirde, ağ geçidi yüklemesi bir sonraki adımda seçebilmeniz için yüklenen ağ geçitlerini listesinde görünmeyebilir. Ağ geçidi kaynağınızın ve mantıksal uygulamanızın farklı bölgelerdeki kullanabilirsiniz. | 
   | **Yükleme adı** | Ağ geçidi yüklemenizi zaten seçili değilse, daha önce yüklediğiniz ağ geçidi'ni seçin. | 
   | | | 

   Örnek aşağıda verilmiştir:

   ![Şirket içi veri ağ geçidinizi oluşturmak için ayrıntıları sağlayın](./media/logic-apps-gateway-connection/createblade.png)

4. Ağ geçidi kaynağı Azure panonuza eklemek için seçin **panoya Sabitle**. İşiniz bittiğinde **Oluştur**’u seçin.

   Bulma veya ağ geçidi ana Azure menüsünden, dilediğiniz zaman görüntülemek için seçin **tüm hizmetleri**. 
   Arama kutusuna "şirket içi veri ağ geçitleri" girin ve ardından **şirket içi veri ağ geçitleri**.

   !["Şirket içi veri ağ geçitleri" bulun](./media/logic-apps-gateway-connection/find-on-premises-data-gateway-enterprise-integration.png)

<a name="connect-logic-app-gateway"></a>

## <a name="connect-to-on-premises-data"></a>Şirket içi verilere bağlanma

Ağ geçidi kaynağı oluşturmak ve Azure aboneliğinizin bu kaynak ile ilişkilendirmeniz sonra artık bir bağlantı mantıksal uygulamanız ve şirket içi veri kaynağınız arasında ağ geçidi kullanarak oluşturabilirsiniz.

1. Azure portalında oluşturun veya mantıksal Uygulama Tasarımcısı'nda mantıksal uygulamanızı açın.

2. Şirket içi bağlantılarını destekler, örneğin, bir bağlayıcıyı ekleyin **SQL Server**.

3. Artık bağlantı ayarlarınızı ayarlayın:

   1. Seçin **şirket içi veri ağ geçidi üzerinden Bağlan**. 

   2. İçin **ağ geçitleri**, daha önce oluşturduğunuz ağ geçidi kaynağı seçin. 

      Ağ geçidi bağlantısı konumunuz mantıksal uygulamanız ile aynı bölgede bulunmalıdır ancak farklı bir bölgede bir ağ geçidi seçebilirsiniz.

   3. Benzersiz bağlantı adı ve diğer gerekli bilgileri sağlayın. 

      Benzersiz bir bağlantı adı özellikle birden fazla bağlantı oluşturduğunuzda, bağlantıyı daha sonra kolayca belirlemenize yardımcı olur. Uygunsa, tam etki alanı adınız için de içerir.
   
      Örnek aşağıda verilmiştir:

      ![Mantıksal uygulama ve veri ağ geçidi arasında bir bağlantı oluşturun](./media/logic-apps-gateway-connection/blankconnection.png)

   4. İşiniz bittiğinde **Oluştur**’u seçin. 

Ağ geçidi bağlantınızı artık kullanılacak mantıksal uygulamanız için hazırdır.

## <a name="edit-connection"></a>Bağlantıyı Düzenle

Mantıksal uygulamanız için bir ağ geçidi bağlantısı oluşturduktan sonra daha sonra özel bağlantı ayarlarını güncelleştirmek isteyebilirsiniz.

1. Ağ geçidi bağlantınızı bulun:

   * Mantıksal uygulama menüsünde, yalnızca mantıksal uygulamanızın, tüm API bağlantıları altında bulunacak **geliştirme araçları**seçin **API bağlantıları**. 
   
     ![Mantıksal uygulamanıza gidin, "API bağlantıları" seçeneğini belirleyin](./media/logic-apps-gateway-connection/logic-app-find-api-connections.png)

   * Azure aboneliğinizle ilişkili tüm API bağlantıları bulmak için: 

     * Azure ana menüsünden Git **tüm hizmetleri** > **Web** > **API bağlantıları**. 
     * Veya Azure ana menüsünden Git **tüm kaynakları**.

2. Seçin ve ardından ağ geçidi bağlantısı **Düzenle API bağlantısı**.

   > [!TIP]
   > Güncelleştirmelerinizi etkili yoksa, deneyin [ağ geçidi Windows hizmetini durdurup](./logic-apps-gateway-install.md#restart-gateway).

<a name="change-delete-gateway-resource"></a>

## <a name="delete-gateway-resource"></a>Ağ geçidi kaynağı silme

Farklı bir ağ geçidi kaynağı oluşturmak, ağ geçidiniz ile farklı bir kaynak ilişkilendirmek veya ağ geçidi kaynağı kaldırmak için ağ geçidi kaynağı ağ geçidi yüklemesi etkilemeden silebilirsiniz. 

1. Azure ana menüsünden Git **tüm kaynakları**. 

2. Bulma ve ağ geçidi kaynağınızı seçin.

3. Ağ geçidi kaynak menüsünde seçili değilse seçin **şirket içi veri ağ geçidi**. 

4. Kaynak araç çubuğunda **Sil**.

<a name="faq"></a>

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal uygulamanızı güvenli hale getirme](./logic-apps-securing-a-logic-app.md)
* [Yayın örnekleri ve senaryoları için logic apps](./logic-apps-examples-and-scenarios.md)
