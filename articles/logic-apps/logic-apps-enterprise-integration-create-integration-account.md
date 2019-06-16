---
title: Tümleştirme hesapları B2B çözümleri - Azure Logic Apps oluşturma ve yönetme | Microsoft Docs
description: Oluşturma, bağlama, taşıma ve kurumsal tümleştirme ve B2B çözümleri Azure Logic Apps ile tümleştirme hesapları Sil
services: logic-apps
documentationcenter: ''
author: ecfan
manager: jeconnoc
editor: ''
ms.assetid: d3ad9e99-a9ee-477b-81bf-0881e11e632f
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 04/30/2018
ms.author: estfan
ms.openlocfilehash: 43ecdafac4f0a5cdc9e619537cdbe2a42ff7fe1b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60999686"
---
# <a name="create-and-manage-integration-accounts-for-b2b-solutions-with-logic-apps"></a>B2B çözümleri logic apps ile tümleştirme hesapları oluşturma ve yönetme

Oluşturabileceğinizi önce [Kurumsal tümleştirme ve B2B çözümleri](../logic-apps/logic-apps-enterprise-integration-overview.md) ile [Azure Logic Apps](../logic-apps/logic-apps-overview.md), burada oluşturulacağı, depolanacağı ve gibi B2B yapıtları yönetmek olan tümleştirme hesabı, önce olmalıdır İş ortakları, sözleşmeler, haritalar, şemalar, sertifikaları ve benzeri ticari. Mantıksal uygulamanız tümleştirme hesabı yapıtları çalışın ve XML doğrulama gibi Logic Apps B2B bağlayıcıları kullanmak için önce şunları yapmalısınız [tümleştirme hesabınızı bağlayın](#link-account) mantıksal uygulamanız için. Bunları bağlamak için hem tümleştirme hesabı ve mantıksal uygulamanızı olmalıdır *aynı* Azure konuma veya bölgeye.

Bu makalede, bu görevlerin nasıl gerçekleştirileceğini gösterir:

* Tümleştirme hesabınızı oluşturun.
* Mantıksal uygulama tümleştirme hesabınızı bağlayın.
* Tümleştirme hesabı, başka bir Azure'a taşımak kaynak grubuna veya aboneliğe.
* Tümleştirme hesabı silin.

Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure hesabınızın kimlik bilgileriyle <a href="https://portal.azure.com" target="_blank">Azure portalında</a> oturum açın.

## <a name="create-integration-account"></a>Tümleştirme hesabı oluşturma

1. Azure ana menüsünden seçin **tüm hizmetleri**. Arama kutusuna filtreniz olarak "tümleştirme hesapları" girin ve seçin **tümleştirme hesapları**.

   ![Tümleştirme hesapları bulun](./media/logic-apps-enterprise-integration-create-integration-account/create-integration-account.png)

2. Altında **tümleştirme hesapları**, seçin **Ekle**.

   ![Tümleştirme hesabı oluşturmak için "Ekle" yi seçin](./media/logic-apps-enterprise-integration-create-integration-account/add-integration-account.png)

3. Tümleştirme hesabı hakkında bilgi sağlar: 

   ![Tümleştirme hesabının ayrıntılarını sağlayın](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-details.png)

   | Özellik | Gerekli | Örnek değer | Açıklama | 
   |----------|----------|---------------|-------------|
   | Name | Evet | Test Tümleştirme hesabı | Tümleştirme hesabı adı. Bu örnekte, belirtilen adı kullanın. | 
   | Abonelik | Evet | <*Azure-subscription-name*> | Kullanılacak Azure aboneliği adı | 
   | Kaynak grubu | Evet | Tümleştirme hesabı rg test | Adı [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) ilgili kaynakların düzenlenmesi için kullanılır. Bu örnekte, belirtilen ada sahip yeni bir kaynak grubu oluşturun. | 
   | Fiyatlandırma Katmanı | Evet | Boş | Kullanmak istediğiniz fiyatlandırma katmanı. Bu örnekte, seçin **ücretsiz**, ancak daha fazla bilgi için [Logic Apps limitler ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md) ve [Logic Apps fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps/). | 
   | Location | Evet | Batı ABD | Bölge tümleştirme hesap bilgilerinizin depolanacağı konumu. Mantıksal uygulamanızı aynı konumu seçin veya bir mantıksal uygulama tümleştirme hesabınız ile aynı konumda oluşturun. | 
   | Log Analytics çalışma alanı | Hayır | Kapalı | Tanılama günlüğüne kaydetme ayarını **Kapalı** durumda bırakın. | 
   ||||| 

4. Hazır olduğunuzda **Panoya sabitle**'yi ve ardından **Oluştur**'u seçin.

   Azure, Azure, genellikle bir dakika içinde tamamlanır, seçili konuma tümleştirme hesabınızı dağıttıktan sonra tümleştirme hesabı açılır.

   ![Azure tümleştirme hesabınızı açar](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-created.png)

Şimdi mantıksal uygulamanızı, tümleştirme hesabı kullanabilmeniz için mantıksal uygulamanızı tümleştirme hesabına bağlamanız gerekir.

<a name="link-account"></a>

## <a name="link-to-logic-app"></a>Mantıksal uygulamanızı bağlayın

İş ortakları, sözleşmeler, haritalar ve şemaları, alım-satım gibi B2B yapıtları içeren bir tümleştirme hesabı için logic apps erişim verilecek mantıksal uygulamanıza tümleştirme hesabınızı bağlamanız gerekir. 

> [!NOTE]
> Tümleştirme hesabı ve mantıksal uygulamanızı aynı bölgede bulunmalıdır.

1. Azure portalında bulun ve mantıksal uygulamanızı açın.

2. Mantıksal uygulama menüsünde, altında **ayarları**seçin **iş akışı ayarları**. İçinde **tümleştirme hesabı seçin** listesinde, mantıksal uygulamanızı bağlamak için tümleştirme hesabı'nı seçin.

   ![Tümleştirme hesabı](./media/logic-apps-enterprise-integration-create-integration-account/linkaccount-2.png)

3. Bağlama bitirmek için seçin **Kaydet**.

   ![Tümleştirme hesabı](./media/logic-apps-enterprise-integration-create-integration-account/linkaccount-3.png)

   Tümleştirme hesabınızı başarıyla bağlandığında, Azure, bir onay iletisi görüntüler. 

   ![Azure başarılı bağlantıyı doğrular.](./media/logic-apps-enterprise-integration-create-integration-account/linkaccount-5.png)

Şimdi mantıksal uygulamanızı tüm yapıtlar yanı sıra XML doğrulama ve kodlama veya kod çözme düz dosya gibi B2B bağlayıcılar, tümleştirme hesabı kullanabilirsiniz.  

## <a name="unlink-from-logic-app"></a>Mantıksal uygulamadan bağlantısını Kaldır

Mantıksal uygulamanız başka bir tümleştirme hesabı'na bağlamak veya artık mantıksal uygulamanız ile bir tümleştirme hesabı kullanmak için Azure kaynak Gezgini üzerinden bağlantıyı silebilirsiniz.

1. Tarayıcınızda, Git <a href="https://resources.azure.com" target="_blank">Azure kaynak Gezgini (https://resources.azure.com)</a>. Aynı Azure kimlik bilgileri ile oturum açtığınızdan emin olun.

   ![Azure Resource Manager](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer.png)

2. Arama kutusunda, mantıksal uygulamanızın adını girin, ardından bulun ve mantıksal uygulamanızı seçin.

   ![Bulun ve mantıksal uygulama seçin](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-find-logic-app.png)

3. Explorer başlık çubuğunda **okuma/yazma**.

   !["Okuma/yazma" modunu açma](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-choose-read-write-mode.png)

4. Üzerinde **veri** sekmesini, **Düzenle**.

   !["Veri" sekmesinde "Düzenle" öğesini seçin.](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-choose-edit.png)

5. Düzenleyicide Bul `integrationAccount` tümleştirme özelliği hesabı ve bu biçime sahip bu özellik silin:

   ```json
   "integrationAccount": {
      "name": "<integration-account-name>",
      "id": "<integration-account-resource-ID>",
      "type": "Microsoft.Logic/integrationAccounts"  
   },
   ```

   Örneğin:

   !["İntegrationAccount" özelliği tanımı bulunamadı](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-delete-integration-account.png)

6. Üzerinde **veri** sekmesini, **Put** yaptığınız değişiklikleri kaydedin. 

   ![Değişiklikleri kaydetmek için "Put" seçin](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-save-changes.png)

7. Azure portalında, mantıksal uygulamanızın **iş akışı ayarları**, kontrol **tümleştirme hesabı** özelliği artık görünür boş.

   ![Tümleştirme hesabı bağlantılı olmayan denetleyin](./media/logic-apps-enterprise-integration-create-integration-account/unlinked-account.png)

## <a name="move-integration-account"></a>Tümleştirme hesabını taşıma

Tümleştirme hesabı, başka bir Azure abonelik veya kaynak grubuna taşıyabilirsiniz.

1. Ana Azure menüsünde **tüm hizmetleri**. Arama kutusuna filtreniz olarak "tümleştirme hesapları" girin ve seçin **tümleştirme hesapları**.

   ![Tümleştirme hesabı bulunamadı](./media/logic-apps-enterprise-integration-create-integration-account/create-integration-account.png)

2. Altında **tümleştirme hesapları**, taşımak istediğiniz tümleştirme hesabı seçin. Üzerinde tümleştirme hesabı menünüzde **ayarları**, seçin **özellikleri**.

   !["Ayarlar" altında "Özellikler" öğesini seçin.](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-properties.png)

3. Azure kaynak grubu veya abonelik, tümleştirme hesabı için değiştirin.

   !["Kaynak grubunu değiştir" veya "Değişiklik abonelik" seçin](./media/logic-apps-enterprise-integration-create-integration-account/change-resource-group-subscription.png)

4. İşiniz bittiğinde, yapıtlar için yeni kaynak kimliklerini tüm betikleri güncelleştirdiğinizden emin olun.  

## <a name="delete-integration-account"></a>Tümleştirme hesabını Sil

1. Ana Azure menüsünde **tüm hizmetleri**. Arama kutusuna filtreniz olarak "tümleştirme hesapları" girin ve seçin **tümleştirme hesapları**.

   ![Tümleştirme hesabı bulunamadı](./media/logic-apps-enterprise-integration-create-integration-account/create-integration-account.png)

2. Altında **tümleştirme hesapları**, silmek istediğiniz tümleştirme hesabı seçin. Tümleştirme hesabı menüsünde **genel bakış**, ardından **Sil**. 

   ![Tümleştirme hesabı'nı seçin. "Genel bakış" sayfasından, "Sil" seçeneğini belirleyin.](./media/logic-apps-enterprise-integration-create-integration-account/delete-integration-account.png)

3. Tümleştirme hesabınızı silmek istediğinizi onaylamak için seçin **Evet**.

   ![Silme işlemini onaylamak için "Evet"'seçeneğini belirleyin.](./media/logic-apps-enterprise-integration-create-integration-account/confirm-delete.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Ticari ortaklar oluşturma](../logic-apps/logic-apps-enterprise-integration-partners.md)
* [Anlaşmaları oluşturma](../logic-apps/logic-apps-enterprise-integration-agreements.md)
