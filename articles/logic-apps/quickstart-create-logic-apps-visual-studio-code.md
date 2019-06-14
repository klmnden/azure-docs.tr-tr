---
title: Oluşturma ve Visual Studio Code - Azure Logic Apps ile otomatik iş akışları yönetme | Microsoft Docs
description: Hızlı Başlangıç için oluşturma ve Visual Studio code'da (VS Code) JSON ile mantıksal uygulamaları yönetme
services: logic-apps
ms.service: logic-apps
ms.workload: azure-vs
author: ecfan
ms.author: estfan
ms.topic: article
ms.reviewer: klam, deli, LADocs
ms.suite: integration
ms.date: 10/05/2018
ms.openlocfilehash: 0fec590523fa130af2e5670a92914c056df289d1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60512043"
---
# <a name="quickstart-create-and-manage-automated-logic-app-workflows---visual-studio-code"></a>Hızlı Başlangıç: Oluşturma ve otomatik mantıksal uygulama iş akışlarını - Visual Studio Code yönetme

İle [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve Visual Studio Code, oluşturma ve yönetme yardımcı olan mantıksal uygulamalar görevleri, iş akışları ve uygulamaları, verileri, sistemleri ve Hizmetleri kurum ve kuruluşların arasında tümleştirmeye yönelik işlemleri otomatik hale getirin. Bu hızlı nasıl oluşturabileceğinizi ve kod tabanlı bir deneyim aracılığıyla iş akışı Tanım Şeması JavaScript nesne gösterimi (JSON) birlikte çalışarak, mantıksal uygulama iş akışı tanımları Düzenle gösterilir. Ayrıca dağıtılan zaten var olan mantıksal uygulamalar üzerinde çalışabilir <a href="https://docs.microsoft.com/azure/guides/developer/azure-developer-guide" target="_blank">Azure</a> bulutta. 

Aynı görevleri de gerçekleştirebilirsiniz ancak <a href="https://portal.azure.com" target="_blank">Azure portalında</a> ve mantıksal uygulama tanımları ile zaten biliyor ve doğrudan kod içinde çalışmak istediğiniz zaman Visual Studio'da daha hızlı bir şekilde Visual Studio Code'da başlayabilirsiniz. Örneğin, devre dışı bırakma, etkinleştirme, silmek ve önceden oluşturulmuş mantıksal uygulamalar yenileyin. Ayrıca, logic apps ve tümleştirme hesapları Visual Studio Code çalıştırıldığı herhangi bir geliştirme platformu, Linux, Windows ve Mac gibi üzerinde çalışabilir

Bu makalede olarak aynı mantıksal uygulama oluşturabilirsiniz [Azure portalında bir mantıksal uygulama oluşturmaya yönelik hızlı başlangıç](../logic-apps/quickstart-create-first-logic-app-workflow.md), hangi odaklanılmıştır fazla temel kavramlara. Visual Studio Code'da mantıksal uygulama bu örnekteki gibi görünür:

![Tamamlanmış mantıksal uygulama](./media/create-logic-apps-visual-studio-code/overview.png)

Başlamadan önce şunlara sahip olduğundan emin olun:

* Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Hakkında temel bilgilere [mantıksal uygulama iş akışı tanımları](../logic-apps/logic-apps-workflow-definition-language.md) ve JavaScript nesne gösterimi (JSON) kullanır, yapısı 

  Logic Apps kullanmaya yeni başladıysanız, size kılavuzluk hızlı başlangıcı deneyin [Azure portalında ilk mantıksal uygulamanızı oluşturmak nasıl](../logic-apps/quickstart-create-first-logic-app-workflow.md), hangi odaklanılmıştır fazla temel kavramlara. 

* Web erişimi için Azure ve Azure aboneliğinizde oturum açmak için

* Henüz yoksa şu araçları indirip yükleyin: 

  * <a href="https://code.visualstudio.com/" target="_blank">Visual Studio Code sürüm 1.25.1 veya üzeri</a>, ücretsiz olduğu

  * Azure Logic Apps için Visual Studio Code uzantısı

    Bu uzantıyı indirip [Visual Studio Market](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-logicapps) veya doğrudan Visual Studio Code içinde. 
    Yükledikten sonra Visual Studio Code'u yeniden yükleyip emin olun. 

    !["Azure Logic Apps için Visual Studio Code uzantısı" bulun](./media/create-logic-apps-visual-studio-code/find-install-logic-apps-extension.png)

    Uzantı doğru bir şekilde Azure yüklü olduğunu denetlemek için Visual Studio Code araç çubuğunda simgesi görünür. 

    ![Yüklü uzantı](./media/create-logic-apps-visual-studio-code/installed-extension.png)

    Daha fazla bilgi için <a href="https://code.visualstudio.com/docs/editor/extension-gallery" target="_blank">uzantı Marketi</a>. Ayrıca görüntüleyebilir ve bu uzantının açık kaynak sürümü Katkıları ederek gönderme [GitHub üzerinde Visual Studio Code için Azure Logic Apps uzantısı](https://github.com/Microsoft/vscode-azurelogicapps). 

<a name="sign-in-azure"></a>

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

1. Visual Studio Code'u açın. Visual Studio Code araç çubuğunda Azure simgesini seçin. 

   ![Azure simgesini seçin](./media/create-logic-apps-visual-studio-code/open-extension.png)

1. Azure penceresinde altında **Logic Apps**seçin **Azure'da oturum aç**. 

   !["Azure'da oturum açın" seçin](./media/create-logic-apps-visual-studio-code/sign-in-azure.png)

   Artık belirtilen kimlik doğrulaması kodu kullanarak oturum açmanız istenir. 

1. Kimlik doğrulama kodu kopyalayın ve ardından **açın & Kopyala**, yeni bir tarayıcı penceresi açılır.

   ![Oturum açma istemi](./media/create-logic-apps-visual-studio-code/sign-in-prompt.png)

1. Kimlik doğrulama kodunuzu girin. İstendiğinde, **devam**.

   ![Kodu girin](./media/create-logic-apps-visual-studio-code/authentication-code.png)

1. Azure hesabınızı seçin. Oturum açtıktan sonra tarayıcınızı kapatın ve Visual Studio Code için döndürür.

   Azure penceresinde, Logic Apps ve tümleştirme hesapları bölmesinde artık hesabınızdaki Azure abonelik gösterir. 

   ![Abonelik seçme](./media/create-logic-apps-visual-studio-code/select-azure-subscription.png)

   Abonelik görmüyorsanız yanında beklediğiniz **Logic Apps** etiket öğesini **abonelikleri seçin** (filtre simgesi). Bulun ve istediğiniz abonelikleri seçin.

1. Azure aboneliğinizdeki herhangi bir mevcut mantıksal uygulamaları veya tümleştirme hesapları görüntülemek için aboneliğinizi genişletin.

   ![Logic apps ve tümleştirme hesapları görüntüleyin](./media/create-logic-apps-visual-studio-code/existing-logic-apps.png)

<a name="create-logic-app"></a>

## <a name="create-logic-app"></a>Mantıksal uygulama oluşturma

1. Azure aboneliğinizde oturum açmadıysanız için bu makaledeki adımlarda Visual Studio Code içinde izleme [şimdi oturum açın](#sign-in-azure).

1. Aboneliğinizin bağlam menüsünden seçin **Oluştur**.

   !["Oluştur" seçeneğini belirleyin](./media/create-logic-apps-visual-studio-code/create-logic-app.png)

1. Azure kaynak grupları, aboneliğinize gösteren listeden mevcut bir kaynak grubu seçin veya **yeni bir kaynak grubu oluşturma**. 

   Bu örnek, yeni bir kaynak grubu oluşturur:

   ![Yeni kaynak grubu oluşturun](./media/create-logic-apps-visual-studio-code/select-or-create-azure-resource-group.png)

1. Azure kaynak grubunuz için bir ad belirtin ve ardından ENTER tuşuna basın.

   ![Kaynak grubunuzu adlandırın](./media/create-logic-apps-visual-studio-code/enter-name-resource-group.png)

1. Mantıksal uygulamanızın meta verileri kaydedileceği yeri veri merkezi konumunu seçin.

   ![Konum seçin](./media/create-logic-apps-visual-studio-code/select-location.png)

1. Mantıksal uygulamanız için bir ad belirtin ve ardından ENTER tuşuna basın.

   ![Mantıksal uygulamanızı adlandırın](./media/create-logic-apps-visual-studio-code/enter-name-logic-app.png)

   Yeni mantıksal uygulamanızı şimdi Azure penceresinde, Azure aboneliğinizde görünür. Şimdi mantıksal uygulamanızın iş akışı tanımı oluşturmaya başlayabilirsiniz.

1. Mantıksal uygulamanızın kısayol menüsünden seçin **düzenleyicide Aç**. 

   ![Düzenleyicide açık mantıksal uygulama](./media/create-logic-apps-visual-studio-code/open-new-logic-app.png)

   Visual Studio Code, bir mantıksal uygulama iş akışı tanımı şablonu açar (. logicapp.json dosya) mantıksal uygulamanızın iş akışı oluşturma başlamak üzere.

   ![Yeni mantıksal uygulama iş akışı tanımı](./media/create-logic-apps-visual-studio-code/blank-logic-app-workflow-definition.png)

1. Mantıksal uygulama iş akışı tanımı şablon dosyasında, mantıksal uygulamanızın iş akışı tanımı oluşturmaya başlayın. Teknik başvuru için bkz: [Azure Logic Apps iş akışı tanımı dil şeması](../logic-apps/logic-apps-workflow-definition-language.md).

   Bir örnek mantıksal tanımı aşağıda verilmiştir. Genellikle JSON öğeleri alfabetik olarak her bölüm içinde görünür, ancak bu örnekte bu öğeler kabaca mantıksal uygulamanın adımı tasarımcıda görünür sırayla gösterilmektedir.

   ```json
   {
      "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
         "$connections": {
            "defaultValue": {},
            "type": "Object"
         }
      },
      "triggers": {
         "When_a_feed_item_is_published": {
            "recurrence": {
               "frequency": "Minute",
               "interval": 1
            },
            "splitOn": "@triggerBody()?['value']",
            "type": "ApiConnection",
            "inputs": {
               "host": {
                  "connection": {
                     "name": "@parameters('$connections')['rss']['connectionId']"
                  }
               },
               "method": "get",
               "path": "/OnNewFeed",
               "queries": {
                  "feedUrl": "http://feeds.reuters.com/reuters/topNews"
               }
            }
         }
      },
      "actions": {
         "Send_an_email": {
            "runAfter": {},
            "type": "ApiConnection",
            "inputs": {
               "body": {
                  "Body": "Title: @{triggerBody()?['title']}\n\nDate published: @{triggerBody()?['publishDate']}\n\nLink: @{triggerBody()?['primaryLink']}",
                  "Subject": "New RSS item: @{triggerBody()?['title']}",
                  "To": "Sophie.Owen@contoso.com"
               },
               "host": {
                  "connection": {
                     "name": "@parameters('$connections')['outlook']['connectionId']"
                  }
               },
               "method": "post",
               "path": "/Mail"
            }
         }
      },
      "outputs": {}
   }   
   ```

1. İşiniz bittiğinde, mantıksal uygulama tanımı dosyasını kaydedin. Visual Studio Code, Azure aboneliğinize, mantıksal uygulama tanımınızı karşıya onaylamanızı ister seçtiğinizde **karşıya**.

   ![Yeni mantıksal uygulamanızı karşıya yükleyin](./media/create-logic-apps-visual-studio-code/upload-new-logic-app.png)

   Visual Studio Code, mantıksal uygulamanızı Azure'a yayımlar. sonra uygulamanız artık canlı ve çalışan Azure portalında bulabilirsiniz. 

   ![Azure Portalı'nda yayımlanan mantıksal uygulama](./media/create-logic-apps-visual-studio-code/published-logic-app.png)

<a name="edit-logic-app"></a>

## <a name="edit-logic-app"></a>Mantıksal uygulamayı Düzenle

Zaten Azure'da dağıtılan var olan bir mantıksal uygulama üzerinde çalışmak için Visual Studio Code'da bu uygulamanın iş akışı tanım dosyasını açabilirsiniz.

1. Azure aboneliğinizde oturum açmadıysanız için bu makaledeki adımlarda Visual Studio Code içinde izleme [şimdi oturum açın](#sign-in-azure).

1. Azure penceresinde altında **Logic Apps**, Azure aboneliğinizi genişletin ve istediğiniz mantıksal uygulamayı seçin. 

1. Mantıksal uygulamanızın menüden **düzenleyicide Aç**. Veya mantıksal uygulamanızın adının yanında, düzenleme simgesini seçin.

   ![Mevcut mantıksal uygulama için Düzenleyiciyi Aç](./media/create-logic-apps-visual-studio-code/open-editor-existing-logic-app.png)

   Visual Studio Code açar. mantıksal uygulamanızın iş akışı tanımı için logicapp.json dosyası.

   ![Açık mantıksal uygulama iş akışı tanımı](./media/create-logic-apps-visual-studio-code/edit-logic-app-workflow-definition-file.png)

1. Mantıksal uygulamanızın tanımında değişikliklerinizi yapın.

1. İşiniz bittiğinde yaptığınız değişiklikleri kaydedin.

1. Visual Studio Code, Azure aboneliğinizde mantıksal uygulama tanımınızı güncelleştirmek için istediğinde seçin **karşıya**. 

   ![Düzenlemeleriniz karşıya yükleme](./media/create-logic-apps-visual-studio-code/upload-logic-app-changes.png)

## <a name="get-support"></a>Destek alın

* Sorularınız için <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">Azure Logic Apps forumunu</a> ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için <a href="https://aka.ms/logicapps-wish" target="_blank">Logic Apps kullanıcı geri bildirimi sitesini</a> ziyaret edin.

