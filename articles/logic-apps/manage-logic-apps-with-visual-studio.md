---
title: Logic apps ile Visual Studio - Azure mantıksal uygulamaları yönetme | Microsoft Docs
description: Mantıksal uygulamalar ve diğer Azure varlıkları Visual Studio bulut Gezgini ile yönetme
author: ecfan
manager: SyntaxC4
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: ''
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: mvc
ms.date: 03/15/2018
ms.author: estfan; LADocs
ms.openlocfilehash: 7914bce6ca71b1b3f00c69fb6f33154f0f52dc7a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="manage-logic-apps-with-visual-studio"></a>Logic apps Visual Studio ile yönetme

Oluşturabilseniz de düzenleme, yönetmek ve logic apps içinde dağıtmak <a href="https://portal.azure.com" target="_blank">Azure portal</a>, kaynak denetimi, farklı sürümlerin yayımlama ve oluşturmak için mantıksal uygulamalar eklemek istediğinizde de Visual Studio kullanabilirsiniz [Azure kaynak Yöneticisi](../azure-resource-manager/resource-group-overview.md) farklı dağıtım ortamları için şablonlar. Visual Studio bulut Gezgini ile bulmak ve diğer Azure kaynaklarınızı birlikte mantıksal uygulamalarınızı yönetin. Örneğin, açın, indirin, düzenlemek, çalıştırmak, yapabilirsiniz çalıştırma geçmişi, devre dışı bırakma ve zaten dağıtılmış olan etkinleştir logic apps Azure portalında görüntüleyin. Visual Studio'da Azure Logic Apps ile çalışmaya yeniyseniz, bilgi [logic apps ile Visual Studio oluşturma](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

> [!IMPORTANT]
> Dağıtma veya bir mantıksal uygulama Visual Studio'dan yayımlamak Azure portalında bu uygulamayı sürümü üzerine yazar. Korumak istediğiniz Azure portalında değişiklik yaparsanız, bu nedenle olduğundan emin olun, [Visual Studio'da mantıksal uygulama yenileme](#refresh) dağıtmak veya Visual Studio'dan yayımlamak sonraki süreden önce Azure portalından.

<a name="requirements"></a>

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Henüz yoksa şu araçları indirip yükleyin: 

  * <a href="https://www.visualstudio.com/downloads" target="_blank">Visual Studio 2017 veya Visual Studio 2015 - Community sürümü veya üzeri</a>. 
  Bu hızlı başlangıçta ücretsiz olan Visual Studio Community 2017 kullanılmaktadır.

  * <a href="https://azure.microsoft.com/downloads/" target="_blank">Azure SDK (2.9.1 veya sonrası)</a> ve <a href="https://github.com/Azure/azure-powershell#installation" target="_blank">Azure PowerShell</a>

  * <a href="https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551" target="_blank">Visual Studio 2017 için Azure Logic Apps Araçları</a> veya <a href="https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio" target="_blank">Visual Studio 2015 sürümü</a> 
  
    Azure Logic Apps Araçlarını doğrudan Visual Studio Market’ten indirip yükleyebilir veya <a href="https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions" target="_blank">bu uzantıyı Visual Studio’nun içinden yükleme</a> hakkında bilgi edinebilirsiniz. 
    Yükleme işlemini tamamladıktan sonra Visual Studio’yu yeniden başlattığınızdan emin olun.

* Katıştırılmış Logic Apps Tasarımcısı'nı kullanırken web erişimi

  Tasarımcının Azure'da kaynak oluşturması ve mantıksal uygulamanızdaki bağlayıcılardan özellik ve verileri okuması için İnternet bağlantısı gerekir. 
  Örneğin, Dynamics CRM Online bağlayıcısını kullanıyorsanız, tasarımcı CRM örneğinizdeki varsayılan ve özel kullanılabilir özellikleri denetler.

<a name="find-logic-apps-vs"></a>

## <a name="find-your-logic-apps"></a>Mantıksal uygulamalarınızı Bul

Visual Studio'da Azure aboneliğinizle ilişkili ve bulut Gezgini'ni kullanarak Azure portalında dağıtılan tüm logic apps bulabilirsiniz.

1. Visual Studio'yu açın. Üzerinde **Görünüm** menüsünde, select **Cloud Explorer**.

2. Cloud Explorer'da seçin **hesap yönetimi**. Logic apps ile ilişkili Azure aboneliğini seçin ve ardından **Uygula**. Örneğin:

   !["Hesap Yönetimi" seçin](./media/manage-logic-apps-with-visual-studio/account-management-select-Azure-subscription.png)

2. Olup tarafından aramakta olduğunuz üzerinde temel **kaynak grupları** veya **kaynak türleri**, şu adımları izleyin:

   * **Kaynak grupları**: Azure aboneliğinize altında bu abonelikle ilişkili tüm kaynak gruplarının Cloud Explorer gösterir. 
   Mantıksal uygulamanızı içeren kaynak grubunu genişletin, sonra mantıksal uygulamanızı seçin.

   * **Kaynak türleri**: Azure aboneliğiniz altında genişletin **Logic Apps**. Cloud Explorer aboneliğinizle ilişkili tüm dağıtılan logic apps gösterir sonra mantıksal uygulamanızı seçin.

<a name="open-designer"></a>

## <a name="open-in-visual-studio"></a>Visual Studio'da aç

Visual Studio'da daha önce oluşturulan ve Azure Portalı aracılığıyla doğrudan ya da Visual Studio ile Azure Resource Manager projeleri olarak dağıtılan logic apps açabilirsiniz.

1. Bulut Gezgini'ni açın ve mantıksal uygulamanızı bulun. 

2. Mantığı uygulamanın kısayol menüsünden seçin **mantığı uygulama Düzenleyicisi ile açık**.

   Mantıksal uygulamalarınızı altında görünmesi için bu örnek, logic apps kaynak türüne göre gösterir. **Logic Apps** bölümü.

  ![Azure Portalı'ndan açık dağıtılan mantıksal uygulama](./media/manage-logic-apps-with-visual-studio/open-logic-app-in-editor.png)

   Mantıksal Uygulama Tasarımcısı'nın altındaki Logic Apps Tasarımcısı'nda açıldıktan sonra seçebileceğiniz **kod görünümü** böylece mantığı uygulama tanımı yapılarını gözden geçirebilirsiniz. 
   Mantıksal uygulama için bir dağıtım şablonu oluşturmak istiyorsanız, bilgi [bir Azure Resource Manager şablonu indirmek nasıl](#download-logic-app) bu mantıksal uygulama için. Daha fazla bilgi edinmek [Resource Manager şablonları](../azure-resource-manager/resource-group-overview.md#template-deployment).

<a name="download-logic-app"></a>

## <a name="download-from-azure"></a>Azure'dan indirin

Mantığı uygulamalardan indirebilirsiniz <a href="https://portal.azure.com" target="_blank">Azure portal</a> ve bunları olarak kaydetmek [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) şablonları. Ardından yerel olarak Visual Studio şablonlarıyla düzenleyebilir ve logic apps farklı dağıtım ortamları için özelleştirin. Mantıksal uygulamalar otomatik olarak yüklenmesini *parameterizes* tanımlarını içinde [Resource Manager şablonları](../azure-resource-manager/resource-group-overview.md#template-deployment), JavaScript nesne gösterimi (JSON) da kullanın.

1. Visual Studio'da bulut Gezgini bulun ve Azure'dan indirmek istediğiniz mantıksal uygulama seçin.

2. Bu uygulamanın kısayol menüsünden seçin **mantığı uygulama Düzenleyicisi ile açık**.

   Mantıksal Uygulama Tasarımcısı'nı açar ve mantıksal uygulama gösterir. 
   Mantıksal uygulama'nın temel tanımı ve Tasarımcısı'nın altındaki yapısı gözden geçirmek için seçin **kod görünümü**. 

3. Tasarımcı araç çubuğunda seçin **karşıdan**.

   !["Yükle" seçin](./media/manage-logic-apps-with-visual-studio/download-logic-app.png)

4. İçin bir konum istendiğinde, bu konuma göz atın ve mantıksal uygulama tanımını Resource Manager şablonu JSON (.json) dosya biçiminde kaydedin. 

Mantıksal uygulama tanımını görünür `resources` Resource Manager şablonu içinde alt bölüm. Şimdi mantıksal uygulama tanımını ve Visual Studio ile Resource Manager şablonu düzenleyebilirsiniz. Şablon bir Azure Resource Manager projesi için Visual Studio çözümü de ekleyebilirsiniz. Hakkında bilgi edinin [Resource Manager projeleri için Visual Studio logic apps](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md). 

<a name="refresh"></a>

## <a name="refresh-from-azure"></a>Azure'dan Yenile

Azure portalda mantıksal uygulamanızı düzenlemek ve bu değişiklikleri korumak istiyorsanız bu değişikliklerle bu uygulamanın sürüm Visual Studio'da Yenile emin olun. 

* Visual Studio'da mantığı Uygulama Tasarımcısı araç çubuğundaki seçin **yenileme**.

  -veya-

* Visual Studio Cloud Explorer mantığı uygulamanızın kısayol menüsünü açın ve seçin **yenileme**. 

![Mantıksal uygulama güncelleştirmeleri ile yenileme](./media/manage-logic-apps-with-visual-studio/refresh-logic-app.png)

## <a name="publish-logic-app-updates"></a>Mantıksal uygulama güncelleştirmeleri yayımlama

Mantıksal uygulama güncelleştirmelerinizi Visual Studio'dan Azure, mantığı Uygulama Tasarımcısı araç çubuğunda dağıtmaya hazır olduğunuzda seçin **Yayımla**.

![Güncelleştirilmiş mantıksal uygulama yayımlama](./media/manage-logic-apps-with-visual-studio/publish-logic-app.png)

## <a name="manually-run-your-logic-app"></a>Elle mantıksal uygulamanızı çalıştırma

Visual Studio'dan azure'da dağıtılan bir mantıksal uygulama el ile tetikleyebilirsiniz. Mantıksal Uygulama Tasarımcısı araç çubuğundaki seçin **tetikleyici çalıştırmak**.

![Mantıksal uygulama el ile çalıştırın](./media/manage-logic-apps-with-visual-studio/manually-run-logic-app.png)

## <a name="review-run-history"></a>Çalıştırma geçmişini gözden geçirme

Durumu denetlemek ve mantığı uygulama çalıştırır sorunları tanılamak için girişleri ve çıkışları olanlar için çalışan Visual Studio gibi ayrıntıları gözden geçirebilirsiniz.

1. Cloud Explorer'da mantığı uygulamanızın kısayol menüsünü açın ve seçin **açık çalıştırma geçmişi**.

   ![Çalıştırma geçmişi Aç](./media/manage-logic-apps-with-visual-studio/view-run-history.png)

2. Belirli bir çalışma ayrıntılarını görüntülemek için bir farklı çalıştır çift tıklayın. Örneğin:

   ![Ayrıntılı çalıştırma geçmişi](./media/manage-logic-apps-with-visual-studio/view-run-history-details.png)
  
   > [!TIP]
   > Tablo özelliğine göre sıralamak için bu özelliği için sütun başlığını seçin. 

3. Adımları, girişleri ve çıkışları gözden geçirmek istediğiniz genişletin. Örneğin:

   ![Görünüm girişleri ve çıkışları her adımı için](./media/manage-logic-apps-with-visual-studio/run-inputs-outputs.png)

## <a name="disable-or-enable-logic-app"></a>Mantıksal uygulama etkinleştirmek veya devre dışı

Mantıksal uygulamanızı silmeden zaman tetikleyici koşul sonraki açışınızda tetikleme gelen tetikleyici durdurabilirsiniz. Mantıksal uygulamanızı devre dışı bırakılması oluşturma ve gelecekteki iş akışı örnekleri için mantıksal uygulamanızı çalıştıran Logic Apps altyapısı engeller.
Cloud Explorer'da mantığı uygulamanızın kısayol menüsünü açın ve seçin **devre dışı**.

![Mantıksal uygulamanızı devre dışı bırak](./media/manage-logic-apps-with-visual-studio/disable-logic-app.png)

Mantıksal uygulamanızı işlemini devam ettirmek hazır olduğunuzda, mantıksal uygulamanızı yeniden etkinleştirebilirsiniz. Cloud Explorer'da mantığı uygulamanızın kısayol menüsünü açın ve seçin **etkinleştirmek**.

![Mantıksal uygulamanızı etkinleştirme](./media/manage-logic-apps-with-visual-studio/enable-logic-app.png)

## <a name="delete-your-logic-app"></a>Mantıksal uygulamanızı silme

Azure Portal'dan Cloud Explorer mantıksal uygulamanızı silmek için mantığı uygulamanızın kısayol menüsünü açın ve seçin **silmek**.

![Mantıksal uygulamanızı silme](./media/manage-logic-apps-with-visual-studio/delete-logic-app.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Visual Studio ile dağıtılan mantığı uygulamaların nasıl yönetileceğini öğrendiniz. Ardından, dağıtım için mantıksal uygulama tanımları özelleştirme hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [JSON içinde Yazar mantıksal uygulama tanımları](../logic-apps/logic-apps-author-definitions.md)
