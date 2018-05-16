---
title: B2B solutions - Azure Logic Apps için tümleştirme hesapları oluşturma ve yönetme | Microsoft Docs
description: Oluşturma, bağlantı, taşıma ve kurumsal tümleştirme ve B2B çözümleri Azure Logic Apps ile tümleştirme hesapları silme
services: logic-apps
documentationcenter: ''
author: ecfan
manager: cfowler
editor: ''
ms.assetid: d3ad9e99-a9ee-477b-81bf-0881e11e632f
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 04/30/2018
ms.author: estfan
ms.openlocfilehash: e661920974c2b0d28200d4c3d82bd644a7a55395
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="create-and-manage-integration-accounts-for-b2b-solutions-with-logic-apps"></a>B2B çözümlerini logic apps ile tümleştirme hesapları oluşturma ve yönetme

Oluşturabileceğiniz önce [Kurumsal tümleştirme ve B2B çözümleri](../logic-apps/logic-apps-enterprise-integration-overview.md) ile [Azure Logic Apps](../logic-apps/logic-apps-overview.md), burada oluşturmak, depolamak ve B2B yapıları gibi yönetmek olan bir tümleştirme hesap önce olmalıdır İş ortakları, anlaşmalar, haritalar, şemalar, sertifikalar ve benzeri ticari. Mantıksal uygulamanızı tümleştirme hesabınızda yapıları ile çalışır ve Logic Apps B2B bağlayıcıları, XML doğrulama gibi kullanmaya başlamadan önce şunları yapmalısınız [tümleştirme hesabınıza bağlamak](#link-account) mantığı uygulamanıza. Bunları bağlamak için her iki tümleştirme hesabı ve mantıksal uygulamanızı olmalıdır *aynı* Azure konumu veya bölge.

Bu makalede, bu görevlerin nasıl gerçekleştirileceğini gösterir:

* Tümleştirme hesabınızı oluşturun.
* Bir mantıksal uygulama tümleştirmesi hesabınızı bağlayın.
* Tümleştirme hesabınızı taşımak için başka bir Azure kaynak grubu veya abonelik.
* Tümleştirme hesabınızı silin.

Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure hesabınızın kimlik bilgileriyle <a href="https://portal.azure.com" target="_blank">Azure portalında</a> oturum açın.

## <a name="create-integration-account"></a>Tümleştirme hesabı oluşturma

1. Ana Azure menüsünden seçin **tüm hizmetleri**. Arama kutusuna "tümleştirme hesapları", filtre olarak girin ve seçin **tümleştirme hesapları**.

   ![Tümleştirme hesapları bulun](./media/logic-apps-enterprise-integration-create-integration-account/create-integration-account.png)

2. Altında **tümleştirme hesapları**, seçin **Ekle**.

   ![Tümleştirme hesabı oluşturmak için "Ekle" yi seçin](./media/logic-apps-enterprise-integration-create-integration-account/add-integration-account.png)

3. Tümleştirme hesabınız hakkında bilgi sağlar: 

   ![Tümleştirme hesabının ayrıntılarını verin](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-details.png)

   | Özellik | Gerekli | Örnek değer | Açıklama | 
   |----------|----------|---------------|-------------|
   | Ad | Evet | sınama tümleştirme hesabı | Tümleştirme hesabınız için ad. Bu örnekte, belirtilen adı kullanın. | 
   | Abonelik | Evet | <*Azure abonelik adı*> | Kullanılacak Azure aboneliği için ad | 
   | Kaynak grubu | Evet | Test-Tümleştirme-hesap-rg | İçin ad [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) ilgili kaynaklar düzenlemek için kullanılır. Bu örnek için belirtilen ada sahip yeni bir kaynak grubu oluşturun. | 
   | Fiyatlandırma Katmanı | Evet | Ücretsiz | Kullanmak istediğiniz fiyatlandırma katmanı. Bu örnek için select **serbest**, ancak daha fazla bilgi için bkz: [Logic Apps sınırlarını ve yapılandırmasını](../logic-apps/logic-apps-limits-and-config.md) ve [Logic Apps fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps/). | 
   | Konum | Evet | Batı ABD | Bölge tümleştirme hesap bilgilerinizin depolanacağı konumu. Mantıksal uygulamanızı aynı konumu seçin ya da bir mantıksal uygulama tümleştirmesi hesabınız ile aynı konumda oluşturun. Bu örnek için | 
   | Log Analytics | Hayır | Kapalı | Tanılama günlüğüne kaydetme ayarını **Kapalı** durumda bırakın. | 
   ||||| 

4. Hazır olduğunuzda, seçin **panoya Sabitle**ve seçin **oluşturma**.

   Azure, genellikle bir dakika içinde tamamlanır, seçili konuma tümleştirme hesabınızı dağıtıldıktan sonra Azure tümleştirme hesabınızı açar.

   ![Azure tümleştirme hesabınızı açar](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-created.png)

Şimdi, mantıksal uygulamanızı tümleştirme hesabınızı kullanmadan önce mantıksal uygulamanızı tümleştirme hesabını bağlamanız gerekir.

<a name="link-account"></a>

## <a name="link-to-logic-app"></a>Mantıksal uygulama için bağlantı

İş ortakları, anlaşmalar, maps ve şemaları, ticaret gibi B2B yapıları içeren bir tümleştirme hesap logic apps erişim vermek için mantıksal uygulamanızı tümleştirme hesabınızı bağlamanız gerekir. 

> [!NOTE]
> Tümleştirme hesabı ve mantığı uygulamanız aynı bölgede olması gerekir.

1. Azure portalında bulun ve mantıksal uygulamanızı açın.

2. Mantığı uygulamanızın menüsünde altında **ayarları**seçin **iş akışı ayarları**. İçinde **tümleştirme hesabı seçme** listesinde, mantıksal uygulamanızı bağlamak için tümleştirme hesabını seçin.

   ![Tümleştirme hesabınızı seçin](./media/logic-apps-enterprise-integration-create-integration-account/linkaccount-2.png)

3. Bağlama işlemini tamamlamak için seçin **kaydetmek**.

   ![Tümleştirme hesabınızı seçin](./media/logic-apps-enterprise-integration-create-integration-account/linkaccount-3.png)

   Tümleştirme hesabınızı başarıyla bağlandığında, Azure bir onay iletisi görüntüler. 

   ![Azure başarılı bağlantı onaylar](./media/logic-apps-enterprise-integration-create-integration-account/linkaccount-5.png)

Şimdi mantıksal uygulamanızı tüm yapıları yanı sıra XML doğrulama ve kodlama veya kod çözme düz dosya gibi B2B bağlayıcılar tümleştirme hesabınızı kullanabilirsiniz.  

## <a name="unlink-from-logic-app"></a>Mantıksal uygulama ' Kaldır

Mantıksal uygulamanızı başka bir tümleştirme hesabınıza bağlamak veya artık mantıksal uygulamanızı ile tümleştirme hesabını kullanmak için Azure kaynak Gezgini bağlantıyla silebilirsiniz.

1. Tarayıcınızda, Git <a href="https://resources.azure.com" target="_blank">Azure kaynak Gezgini (https://resources.azure.com)</a>. Aynı Azure kimlik bilgileriyle kapattığınızdan emin olun.

   ![Azure Resource Manager](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer.png)

2. Arama kutusuna mantığı uygulamanızın adını girin, sonra bulun ve mantıksal uygulamanızı seçin.

   ![Bulma ve mantıksal uygulama seçin](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-find-logic-app.png)

3. Explorer başlık çubuğunda seçin **okuma/yazma**.

   !["Okuma/yazma" Modu'nu](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-choose-read-write-mode.png)

4. Üzerinde **veri** sekmesinde, seçin **Düzenle**.

   !["Data" sekmesinde "Düzenle" seçin](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-choose-edit.png)

5. Düzenleyicisi'nde Bul `integrationAccount` özelliği tümleştirme için hesap ve bu biçimdedir bu özelliği silin:

   ```json
   "integrationAccount": {
      "name": "<integration-account-name>",
      "id": "<integration-account-resource-ID>",
      "type": "Microsoft.Logic/integrationAccounts"  
   },
   ```

   Örneğin:

   !["İntegrationAccount" özelliği tanımı Bul](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-delete-integration-account.png)

6. Üzerinde **veri** sekmesinde, seçin **Put** yaptığınız değişiklikleri kaydetmek için. 

   ![Değişiklikleri kaydetmek için "Put" seçin](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-save-changes.png)

7. Azure portalında mantığı uygulamanızın altında **iş akışı ayarları**, denetleyin **tümleştirme hesabını** özelliği artık görünür boş.

   ![Tümleştirme hesabını bağlantılı olmayan denetleyin](./media/logic-apps-enterprise-integration-create-integration-account/unlinked-account.png)

## <a name="move-integration-account"></a>Tümleştirme hesabını taşıma

Tümleştirme hesabınız için başka bir Azure abonelik veya kaynak grubu taşıyabilirsiniz.

1. Azure ana menüde seçin **tüm hizmetleri**. Arama kutusuna "tümleştirme hesapları", filtre olarak girin ve seçin **tümleştirme hesapları**.

   ![Tümleştirme hesabınızı Bul](./media/logic-apps-enterprise-integration-create-integration-account/create-integration-account.png)

2. Altında **tümleştirme hesapları**, taşımak istediğiniz tümleştirme hesabı seçin. Tümleştirme üzerinde menüsünde altında hesap **ayarları**, seçin **özellikleri**.

   !["Ayarlar altında", "Özellikler"'i seçin](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-properties.png)

3. Azure kaynak grubu veya abonelik tümleştirme hesabınız için değiştirin.

   !["Kaynak grubu Değiştir" veya "Değişiklik abonelik" seçin](./media/logic-apps-enterprise-integration-create-integration-account/change-resource-group-subscription.png)

4. İşiniz bittiğinde, yapıtları için yeni kaynak kimlikleri tüm betikler güncelleştirdiğinizden emin olun.  

## <a name="delete-integration-account"></a>Tümleştirme hesabını silme

1. Azure ana menüde seçin **tüm hizmetleri**. Arama kutusuna "tümleştirme hesapları", filtre olarak girin ve seçin **tümleştirme hesapları**.

   ![Tümleştirme hesabınızı Bul](./media/logic-apps-enterprise-integration-create-integration-account/create-integration-account.png)

2. Altında **tümleştirme hesapları**, silmek istediğiniz tümleştirme hesabı seçin. Tümleştirme hesap menüsünden seçin **genel bakış**, ardından **silmek**. 

   ![Tümleştirme hesabı seçin. "Genel bakış" sayfasında, "Delete" seçin](./media/logic-apps-enterprise-integration-create-integration-account/delete-integration-account.png)

3. Tümleştirme hesabınızı silmek istediğinizi onaylamak için tercih **Evet**.

   ![Silme işlemini onaylamak için "Evet"'i seçin.](./media/logic-apps-enterprise-integration-create-integration-account/confirm-delete.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Ticari ortaklar oluşturma](../logic-apps/logic-apps-enterprise-integration-partners.md)
* [Anlaşmaları oluşturma](../logic-apps/logic-apps-enterprise-integration-agreements.md)