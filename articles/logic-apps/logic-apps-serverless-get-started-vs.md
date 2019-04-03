---
title: Azure Logic Apps & Visual Studio'da Azure işlevleri ile sunucusuz uygulamalar oluşturun
description: Derleme, dağıtma ve Azure Logic Apps ve Azure işlevleri'nde Visual Studio ile ilk sunucusuz uygulamanızı yönetme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.custom: vs-azure
ms.topic: article
ms.date: 04/02/2019
ms.openlocfilehash: 39b44668a89ce0c77c09a7fa20dc4d95b2164bf4
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58863007"
---
# <a name="build-your-first-serverless-app-with-azure-logic-apps-and-azure-functions---visual-studio"></a>Azure Logic Apps ve Azure işlevleri - Visual Studio ile ilk sunucusuz uygulamanızı oluşturun

Hızlı bir şekilde geliştirin ve sunucusuz araçları ve özellikleri Azure'da gibi kullanarak bulut uygulamaları dağıtın [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve [Azure işlevleri](../azure-functions/functions-overview.md). Bu makalede, Visual Studio'da bir Azure işlevi çağıran bir mantıksal uygulama kullanan sunucusuz bir uygulama oluşturmaya başlamak gösterilmektedir. Azure sunucusuz çözümler hakkında daha fazla bilgi edinmek için [sunucusuz Azure işlevleri ve Logic Apps ile](../logic-apps/logic-apps-serverless-overview.md).

## <a name="prerequisites"></a>Önkoşullar

Visual Studio'da sunucusuz bir uygulama oluşturmak için bu öğeler gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Henüz yoksa şu araçları indirip yükleyin:

  * <a href="https://aka.ms/download-visual-studio" target="_blank">Visual Studio 2019, 2017 veya 2015 - Community sürümü veya üzeri</a>. 
  Bu hızlı başlangıçta ücretsiz olan Visual Studio Community 2017 kullanılmaktadır.

    > [!IMPORTANT]
    > Visual Studio 2019 veya 2017'yi yüklediğinizde, seçtiğinizden emin olun **Azure geliştirme** iş yükü.
    > Visual Studio 2019 için Cloud Explorer Azure portalında mantıksal Uygulama Tasarımcısı açabilirsiniz, ancak henüz ekli mantıksal Uygulama Tasarımcısı açılamıyor.

  * <a href="https://azure.microsoft.com/downloads/" target="_blank">.NET için Microsoft Azure SDK (2.9.1 veya sonrası)</a>. <a href="https://docs.microsoft.com/dotnet/azure/dotnet-tools?view=azure-dotnet">Azure SDK for .NET</a> hakkında daha fazla bilgi edinin.

  * [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)

  * Azure Logic Apps araçları istediğiniz Visual Studio sürümü için:

    * <a href="https://aka.ms/download-azure-logic-apps-tools-visual-studio-2019" target="_blank">Visual Studio 2019</a>

    * <a href="https://aka.ms/download-azure-logic-apps-tools-visual-studio-2017" target="_blank">Visual Studio 2017</a>

    * <a href="https://aka.ms/download-azure-logic-apps-tools-visual-studio-2015" target="_blank">Visual Studio 2015</a>
  
    Azure Logic Apps Araçlarını doğrudan Visual Studio Market’ten indirip yükleyebilir veya <a href="https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions" target="_blank">bu uzantıyı Visual Studio’nun içinden yükleme</a> hakkında bilgi edinebilirsiniz. 
    Yükleme işlemini tamamladıktan sonra Visual Studio’yu yeniden başlattığınızdan emin olun.

  * <a href="https://www.npmjs.com/package/azure-functions-core-tools" target="_blank">Azure işlevleri temel araçları</a> işlevleri yerel olarak hata ayıklama

* Ekli Mantıksal Uygulama Tasarımcısı kullanılırken web erişimi

  Tasarımcının Azure'da kaynak oluşturması ve mantıksal uygulamanızdaki bağlayıcılardan özellik ve verileri okuması için İnternet bağlantısı gerekir. 
  Örneğin, Dynamics CRM Online bağlayıcısını kullanıyorsanız, tasarımcı CRM örneğinizdeki varsayılan ve özel kullanılabilir özellikleri denetler.

## <a name="create-resource-group-project"></a>Kaynak grubu projesi oluşturma

Başlamak için oluşturun bir [Azure kaynak grubu projesi](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) sunucusuz uygulamanız için. Azure'da, tek bir varlık düzenleme, yönetmek için kullandığınız bir mantıksal koleksiyondur bir kaynak grubu içindeki kaynaklar ve tüm bir uygulamayı dağıtma kaynakları oluşturun. Azure'da sunucusuz bir uygulama için Azure Logic Apps ve Azure işlevleri için kaynak grubunuzun kaynakları içerir. [Azure kaynak grupları ve kaynakları](../azure-resource-manager/resource-group-overview.md) hakkında daha fazla bilgi edinin.

1. Visual Studio'yu başlatın ve Azure hesabınızla oturum açın.

1. **Dosya** menüsünde **Yeni** > **Proje**’yi seçin.

   ![Visual Studio'da yeni proje oluşturma](./media/logic-apps-serverless-get-started-vs/create-new-project-visual-studio.png)

1. **Yüklü** altında **Visual C#** veya **Visual Basic**’i seçin. **Bulut** > **Azure Kaynak Grubu**’nu seçin.

   > [!NOTE]
   > Varsa **bulut** kategori veya **Azure kaynak grubu** proje mevcut değil, Visual Studio için Azure SDK'sı yüklü olduğundan emin olun.

   Visual Studio 2019 kullanıyorsanız, aşağıdaki adımları izleyin:

   1. İçinde **yeni bir proje oluşturma** kutusunda **Azure kaynak grubu** ya da görsel için proje şablonu C# veya Visual Basic ve **sonraki**.

   1. Kullanmak istediğiniz Azure kaynak grubu ve diğer proje bilgileri için ad belirtin. İşiniz bittiğinde **Oluştur**’u seçin.

1. Projenize bir ad ve konum verin ve ardından **Tamam**.

   Visual Studio şablonları listeden bir şablon seçin isteyip istemediğinizi sorar. 
   Bu örnek, Azure Hızlı Başlangıç şablonu kullanır, bu nedenle, mantıksal uygulama ve bir Azure işlevine bir çağrı içeren sunucusuz bir uygulama oluşturabilirsiniz.

   > [!TIP]
   > Boş olmayan istediğiniz bir Azure kaynak grubuna çözümünüzü predeploy senaryolarda kullanabilirsiniz **mantıksal uygulama** şablon, yalnızca bir boş mantıksal uygulama oluşturur.

1. Gelen **bu konumdan şablonlar Göster** listesinden **Azure Hızlı Başlangıç (github.com/Azure/azure-quickstart-templates)**.

1. Arama kutusuna filtreniz olarak "logic-app" girin. Sonuçlardan, bu şablonu seçin: **101-Logic-App-and-Function-App**

   ![Azure Hızlı Başlangıç şablonu seçin](./media/logic-apps-serverless-get-started-vs/select-template.png)

   Visual Studio oluşturur ve kaynak grubu projenizi bir çözümü açar. 
   Seçtiğiniz Azure Hızlı Başlangıç şablonu adlı bir dağıtım şablonu oluşturur `azuredeploy.json` , kaynak grubu projesi içinde. Bu dağıtım şablonu, bir HTTP isteği tetikler, bir Azure işlevi çağırır ve sonucu olarak bir HTTP yanıtı döndüren basit bir mantıksal uygulama tanımını içerir.

   ![Yeni sunucusuz çözüm](./media/logic-apps-serverless-get-started-vs/create-serverless-solution.png)

1. Ardından, dağıtım şablonunu açın ve kaynakları gözden geçirin, sunucusuz uygulamanız için önce çözümünüzü Azure'a dağıtmanız gerekir.

## <a name="deploy-your-solution"></a>Çözümünüzü dağıtın

Visual Studio'da Logic Apps Tasarımcısı ile mantıksal uygulamanızı açabilmek için önce zaten Azure'da dağıtılan bir Azure kaynak grubu olması gerekir. Tasarımcı, sonra mantıksal uygulamanızın kaynaklarını ve Hizmetleri için bağlantı oluşturabilirsiniz. Bu görev için çözümünüzü Visual Studio'dan Azure portalına dağıtın.

1. Çözüm Gezgini'nde, kaynak projenizin kısayol menüsünden seçin **Dağıt** > **yeni**.

   ![Kaynak grubu için yeni bir dağıtımını oluşturun](./media/logic-apps-serverless-get-started-vs/deploy.png)

1. Zaten seçili değilse, Azure aboneliğinizi ve dağıtmak istediğiniz kaynak grubunu seçin. Seçin **dağıtma**.

   ![Dağıtım ayarları](./media/logic-apps-serverless-get-started-vs/deploy-to-resource-group.png)

1. Varsa **parametreleri Düzenle** kutusu görüntülenirse, mantıksal uygulamanızı ve dağıtım sırasında Azure işlev uygulaması için kullanılacak kaynak adı girin ve ardından ayarlarınızı kaydedin. İşlev uygulamanız için genel olarak benzersiz bir ad kullandığınızdan emin olun.

   ![Mantıksal uygulamanızı ve işlev uygulaması için adlar sağlayın](./media/logic-apps-serverless-get-started-vs/logic-function-app-name-parameters.png)

   Visual Studio, belirtilen kaynak grubuna dağıtım başladığında Visual Studio çözümünüzün dağıtım durumu görünür **çıkış** penceresi. 
   Dağıtım tamamlandıktan sonra mantıksal uygulamanızı Azure portalında zaten çalışıyor.

## <a name="edit-logic-app-in-visual-studio"></a>Mantıksal uygulamayı Visual Studio'da düzenleme

Çözümünüzü kaynak grubunuzun dağıtıldığına göre mantıksal uygulamanızı düzenlemek ve mantıksal uygulamanızı değiştirme mantıksal Uygulama Tasarımcısı ile açın.

1. Çözüm Gezgini'nde, gelen `azuredeploy.json` dosyanın kısayol menüsünde, select **açık ile mantıksal Uygulama Tasarımcısı**.

   ![Logic Apps Tasarımcısı'nda "azuredeploy.json" açın](./media/logic-apps-serverless-get-started-vs/open-logic-app-designer.png)

1. Sonra **mantıksal uygulama özellikleri** kutusu görünür ve zaten altında seçili değilse **abonelik**, Azure aboneliğinizi seçin. Altında **kaynak grubu**, çözümünüzün dağıtıldığı konum ve kaynak grubu seçin ve ardından **Tamam**.

   ![Mantıksal uygulama özellikleri](./media/logic-apps-serverless-get-started-vs/logic-app-properties.png)

   Logic Apps Tasarımcısı açılır sonra adımları eklemeye devam etmek veya iş akışını değiştirme ve güncelleştirmelerinizi kaydedin.

   ![Mantıksal Uygulama Tasarımcısı'nda açılan mantıksal uygulama](./media/logic-apps-serverless-get-started-vs/opened-logic-app.png)

## <a name="create-azure-functions-project"></a>Azure işlevleri projesi oluşturma

JavaScript, Python, işlevi ve işlev projesi oluşturmak için F#, PowerShell, toplu veya Bash, makaledeki adımları izleyin [iş ile Azure işlevleri çekirdek Araçları](../azure-functions/functions-run-local.md). C# ile Azure işlevinizin içine çözümünüzü geliştirmek için bir C# sınıf kitaplığı makalesindeki adımları izleyerek kullanabileceğiniz [.NET sınıf kitaplığı bir işlev uygulaması olarak Yayımla](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/16/publishing-a-net-class-library-as-a-function-app/).

## <a name="deploy-functions-from-visual-studio"></a>İşlevleri Visual Studio'dan dağıtma

Sahip olduğunuz değişken tarafından belirtilen Git deposundan çözümünüzdeki tüm Azure işlevleri, dağıtım şablonu dağıtır `azuredeploy.json` dosya. Oluşturma ve İşlevler projeniz çözümünüzde yazar, proje Git kaynak denetimine, örneğin, GitHub veya Azure DevOps, denetleyebilir ve ardından güncelleştirme `repo` değişken böylece şablonu, Azure işleviniz dağıtır.

## <a name="manage-logic-apps-and-view-run-history"></a>Logic apps ve çalıştırma geçmişi görünümü yönetme

Yine de zaten Azure'da dağıtılan mantıksal uygulamalar için düzenleme, yönetme, çalıştırma geçmişini görüntülemek ve söz konusu uygulamaları Visual Studio'dan devre dışı bırakmak.

1. Gelen **görünümü** Visual Studio'da Aç menüsünde **Cloud Explorer**.

1. Altında **tüm abonelikleri**, istediğiniz yönetmek ve logic apps ile ilişkili Azure aboneliği seçin **Uygula**.

1. Altında **Logic Apps**, mantıksal uygulamanızı seçin. Bu uygulamanın kısayol menüsünden seçin **Logic App Düzenleyicisi ile açın**.

Kaynak grubu projenize zaten yayımlanan bir mantıksal uygulama şimdi yükleyebilirsiniz. Bu nedenle Azure portalında bir mantıksal uygulama başlatıldı olsa da, yine de içeri aktarabilir ve bu uygulamayı Visual Studio'da yönetme. Daha fazla bilgi için [Visual Studio ile mantıksal uygulamaları yönetme](../logic-apps/manage-logic-apps-with-visual-studio.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio ile mantıksal uygulamaları yönetme](manage-logic-apps-with-visual-studio.md)
