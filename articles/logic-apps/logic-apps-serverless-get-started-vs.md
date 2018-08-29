---
title: Visual Studio ile sunucusuz uygulamalar oluşturun | Microsoft Docs
description: Derleme, dağıtma ve Azure Logic Apps ve Azure işlevleri'nde Visual Studio ile ilk sunucusuz uygulamanızı yönetme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.custom: vs-azure
ms.topic: article
ms.date: 08/01/2018
ms.openlocfilehash: a69c129d5ae1405462e3a54a24cd2edbad2a86a7
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43126786"
---
# <a name="build-your-first-serverless-app-with-azure-logic-apps-and-azure-functions---visual-studio"></a>Azure Logic Apps ve Azure işlevleri - Visual Studio ile ilk sunucusuz uygulamanızı oluşturun

Hızlı bir şekilde geliştirin ve sunucusuz araçları ve özellikleri Azure'da gibi kullanarak bulut uygulamaları dağıtın [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve [Azure işlevleri](../azure-functions/functions-overview.md). Bu makalede, Visual Studio'da bir Azure işlevi çağıran bir mantıksal uygulama kullanan sunucusuz bir uygulama oluşturmaya başlamak gösterilmektedir. Azure sunucusuz çözümler hakkında daha fazla bilgi edinmek için [sunucusuz Azure işlevleri ve Logic Apps ile](../logic-apps/logic-apps-serverless-overview.md).

## <a name="prerequisites"></a>Önkoşullar

Visual Studio'da sunucusuz bir uygulama oluşturmak için bu öğeler gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* [Visual Studio 2017](https://www.visualstudio.com/vs/) veya Visual Studio 2015 - Community, Professional veya Enterprise

* [Microsoft Azure SDK'sı](https://azure.microsoft.com/downloads/) (2.9.1 veya sonrası)

* [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)

* [Visual Studio 2017 için Azure Logic Apps Araçları](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) veya [Visual Studio 2015 sürümü](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio)

  İndirebilir ve Azure Logic Apps araçlarını doğrudan Visual Studio Market'ten, yükleme veya [bu uzantıyı yüklemeniz öğrenin Visual Studio içinde](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions). 
  Yükleme işlemini tamamladıktan sonra Visual Studio'yu yeniden başlatmanız emin olun. 

* [Azure işlevleri temel araçları](https://www.npmjs.com/package/azure-functions-core-tools) işlevleri yerel olarak hata ayıklama

* Visual Studio'da Logic Apps Tasarımcısı kullanılırken web erişimi katıştırılmış

  Tasarımcının Azure'da kaynak oluşturması ve mantıksal uygulamanızdaki bağlayıcılardan özellik ve verileri okuması için İnternet bağlantısı gerekir. 
  Örneğin, Dynamics CRM Online bağlayıcısını kullanıyorsanız, tasarımcı CRM örneğinizdeki varsayılan ve özel kullanılabilir özellikleri denetler.

## <a name="create-resource-group-project"></a>Kaynak grubu projesi oluşturma

Başlamak için oluşturun bir [Azure kaynak grubu projesi](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) sunucusuz uygulamanız için. Azure'da, tek bir varlık düzenleme, yönetmek için kullandığınız bir mantıksal koleksiyondur bir kaynak grubu içindeki kaynaklar ve tüm bir uygulamayı dağıtma kaynakları oluşturun. Azure'da sunucusuz bir uygulama için Azure Logic Apps ve Azure işlevleri için kaynak grubunuzun kaynakları içerir. [Azure kaynak grupları ve kaynakları](../azure-resource-manager/resource-group-overview.md) hakkında daha fazla bilgi edinin.

1. Visual Studio'yu başlatın ve Azure hesabınızla oturum açın. 

1. **Dosya** menüsünde **Yeni** > **Proje**’yi seçin. 

   ![Visual Studio'da yeni proje oluşturma](./media/logic-apps-serverless-get-started-vs/create-new-project-visual-studio.png)

1. **Yüklü** altında **Visual C#** veya **Visual Basic**’i seçin. **Bulut** > **Azure Kaynak Grubu**’nu seçin.

   Varsa **bulut** kategori veya **Azure kaynak grubu** proje mevcut değil, Visual Studio için Azure SDK'sı yüklü olduğundan emin olun.

1. Projenize bir ad ve konum verin ve ardından **Tamam**. 

   Visual Studio şablon seçmenizi ister. 
   Boş mantıksal uygulama veya diğer şablon ile başlayın, ancak bu örnek, bir mantıksal uygulama ve bir Azure işlevine bir çağrı içeren sunucusuz bir uygulama oluşturmak için bir Azure Hızlı Başlangıç şablonu kullanır.

   Visual Studio'da yalnızca bir mantıksal uygulama oluşturmak için seçin **mantıksal uygulama** şablonu. Bu şablon, bir Azure kaynak grubuna çözümünüzü predeploy gerek kalmadan Logic App Tasarımcısı'nda açılır bir boş mantıksal uygulama oluşturur.

1. Altında **bu konumdan şablonlar Göster**seçin **Azure Hızlı Başlangıç (github/Azure/azure-quickstart-templates)**. 

1. Arama kutusuna filtreniz olarak "logic-app" girin ve bu sunucusuz Hızlı Başlangıç şablonu seçip **Tamam**: **101-logic-app-and-function-app**

   ![Azure Hızlı Başlangıç şablonu seçin](./media/logic-apps-serverless-get-started-vs/select-template.png)

   Visual Studio oluşturur ve kaynak grubu projenizi bir çözümü açar. 
   Seçtiğiniz Hızlı Başlangıç şablonu adlı bir dağıtım şablonu oluşturur `azuredeploy.json` , kaynak grubu projesi içinde. 
   Bu dağıtım şablonu, bir HTTP isteği tetikler, bir Azure işlevi çağırır ve sonucu olarak bir HTTP yanıtı döndüren basit bir mantıksal uygulama tanımını içerir. 
   
   ![Yeni sunucusuz çözüm](./media/logic-apps-serverless-get-started-vs/create-serverless-solution.png)

1. Ardından, dağıtım şablonunu açın ve kaynakları gözden geçirin, sunucusuz uygulamanız için önce çözümünüzü Azure'a dağıtmanız gerekir. 

## <a name="deploy-your-solution"></a>Çözümünüzü dağıtın

Visual Studio'da Logic Apps Tasarımcısı ile mantıksal uygulamanızı açabilmek için önce zaten Azure'da dağıtılan bir Azure kaynak grubu olması gerekir. Tasarımcı, sonra mantıksal uygulamanızın kaynaklarını ve Hizmetleri için bağlantı oluşturabilirsiniz. Bu görev için çözümünüzü Visual Studio'dan Azure portalına dağıtın.

1. Çözüm Gezgini'nde, kaynak projenin kısayol menüsünü açın ve ardından **Dağıt** > **yeni**.

   ![Kaynak grubu için yeni bir dağıtımını oluşturun](./media/logic-apps-serverless-get-started-vs/deploy.png)

1. Zaten seçili değilse, Azure aboneliğinizi ve dağıtmak istediğiniz kaynak grubunu seçin. Seçin **dağıtma**.

   ![Dağıtım ayarları](./media/logic-apps-serverless-get-started-vs/deploy-to-resource-group.png)

1. Varsa **parametreleri Düzenle** kutusu görüntülenirse, mantıksal uygulamanızı ve dağıtım sırasında Azure işlev uygulaması için kullanılacak kaynak adı girin ve ardından ayarlarınızı kaydedin. İşlev uygulamanız için genel olarak benzersiz bir ad kullandığınızdan emin olun.

   ![Mantıksal uygulamanızı ve işlev uygulaması için adlar sağlayın](./media/logic-apps-serverless-get-started-vs/logic-function-app-name-parameters.png)

   Visual Studio, belirtilen kaynak grubuna dağıtım başladığında Visual Studio çözümünüzün dağıtım durumu görünür **çıkış** penceresi. 
   Dağıtım tamamlandıktan sonra mantıksal uygulamanızı Azure portalında zaten çalışıyor.

## <a name="edit-logic-app-in-visual-studio"></a>Mantıksal uygulamayı Visual Studio'da düzenleme

Çözümünüzü kaynak grubunuzun dağıtıldığına göre mantıksal uygulamanızı düzenlemek ve mantıksal uygulamanızı değiştirmek için mantıksal Uygulama Tasarımcısı ile açın.

1. Çözüm Gezgini'nde açın `azuredeploy.json` dosyanın kısayol menüsünü ve ardından **mantıksal Uygulama Tasarımcısı ile Aç**.

   ![Logic Apps Tasarımcısı'nda "azuredeploy.json" açın](./media/logic-apps-serverless-get-started-vs/open-logic-app-designer.png)

1. Sonra **mantıksal uygulama özellikleri** kutusu görünür ve zaten altında seçili değilse **abonelik**, Azure aboneliğinizi seçin. Altında **kaynak grubu**, çözümünüzün dağıtıldığı konum ve kaynak grubu seçin ve ardından **Tamam**.

   ![Mantıksal uygulama özellikleri](./media/logic-apps-serverless-get-started-vs/logic-app-properties.png)

   Logic Apps Tasarımcısı açılır sonra adımları eklemeye devam etmek veya iş akışını değiştirme ve güncelleştirmelerinizi kaydedin.

   ![Mantıksal Uygulama Tasarımcısı'nda açılan mantıksal uygulama](./media/logic-apps-serverless-get-started-vs/opened-logic-app.png)

## <a name="create-azure-functions-project"></a>Azure işlevleri projesi oluşturma

JavaScript, Python, F #, PowerShell, toplu veya Bash ile işlevi ve işlev projesi oluşturmak için makaledeki adımları [iş ile Azure işlevleri çekirdek Araçları](../azure-functions/functions-run-local.md). C# ile Azure işlevinizin içine çözümünüzü geliştirmek için bir C# sınıf kitaplığı makalesindeki adımları izleyerek kullanabileceğiniz [.NET sınıf kitaplığı bir işlev uygulaması olarak Yayımla](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/16/publishing-a-net-class-library-as-a-function-app/).

## <a name="deploy-functions-from-visual-studio"></a>İşlevleri Visual Studio'dan dağıtma

Sahip olduğunuz değişken tarafından belirtilen Git deposundan çözümünüzdeki tüm Azure işlevleri, dağıtım şablonu dağıtır `azuredeploy.json` dosya. Oluşturursanız ve İşlevler projeniz çözümünüzde yazar, Git kaynak denetimi, örneğin, GitHub veya Visual Studio Team Services içinde bu proje denetleyin ve ardından güncelleştirme `repo` değişken böylece şablonu, Azure işleviniz dağıtır.

## <a name="manage-logic-apps-and-view-run-history"></a>Logic apps ve çalıştırma geçmişi görünümü yönetme

Yine de zaten Azure'da dağıtılan mantıksal uygulamalar için düzenleme, yönetme, çalıştırma geçmişini görüntülemek ve söz konusu uygulamaları Visual Studio'dan devre dışı bırakmak. 

1. Gelen **görünümü** Visual Studio'da Aç menüsünde **Cloud Explorer**. 

1. Altında **tüm abonelikleri**, istediğiniz yönetmek ve logic apps ile ilişkili Azure aboneliği seçin **Uygula**.

1. Altında **Logic Apps**, mantıksal uygulamanızı seçin. Bu uygulamanın kısayol menüsünden seçin **Logic App Düzenleyicisi ile açın**. 

Kaynak grubu projenize zaten yayımlanan bir mantıksal uygulama şimdi yükleyebilirsiniz. Bu nedenle Azure portalında bir mantıksal uygulama başlatıldı olsa da, yine de içeri aktarabilir ve bu uygulamayı Visual Studio'da yönetme. Daha fazla bilgi için [Visual Studio ile mantıksal uygulamaları yönetme](../logic-apps/manage-logic-apps-with-visual-studio.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Sunucusuz bir Sosyal Panosu derleme](logic-apps-scenario-social-serverless.md)
* [Visual Studio ile mantıksal uygulamaları yönetme](manage-logic-apps-with-visual-studio.md)
* [Mantıksal uygulama iş akışı tanımı dili](logic-apps-workflow-definition-language.md)