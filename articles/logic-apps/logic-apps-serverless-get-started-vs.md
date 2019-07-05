---
title: Visual Studio'da Azure Logic Apps ve Azure işlevleri'ni kullanarak sunucusuz uygulamalar oluşturun
description: Oluşturmanızı, dağıtmanızı ve Visual Studio'da Azure Logic Apps ve Azure işlevleri'ni kullanarak sunucusuz ilk uygulamanızı yönetme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.custom: vs-azure
ms.topic: article
ms.date: 06/20/2019
ms.openlocfilehash: b7af4fc731d01bb666165655baa2f1d6c64d4071
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444862"
---
# <a name="build-your-first-serverless-app-by-using-azure-logic-apps-and-azure-functions-in-visual-studio"></a>Visual Studio'da Azure Logic Apps ve Azure işlevleri'ni kullanarak sunucusuz ilk uygulamanızı oluşturun

Hızlı bir şekilde geliştirin ve sunucusuz araçları ve özellikleri, Azure'da gibi kullanarak bulut uygulamaları dağıtın [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve [Azure işlevleri](../azure-functions/functions-overview.md). Bu makalede, Visual Studio'da bir Azure işlevi çağıran bir mantıksal uygulama kullanan sunucusuz bir uygulama oluşturmaya başlamak gösterilmektedir. Azure sunucusuz çözümler hakkında daha fazla bilgi edinmek için [sunucusuz Azure işlevleri ve Logic Apps ile](../logic-apps/logic-apps-serverless-overview.md).

## <a name="prerequisites"></a>Önkoşullar

Visual Studio'da sunucusuz bir uygulama oluşturmak için ihtiyacınız vardır:

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Aşağıdaki araçlar. Zaten sahip değilseniz, indirin ve yükleyin.

  * [Visual Studio 2019, 2017 veya 2015 (topluluk veya diğer edition)](https://aka.ms/download-visual-studio). 
  Bu hızlı başlangıçta ücretsiz olan Visual Studio Community 2017 kullanılmaktadır.

    > [!IMPORTANT]
    > Visual Studio 2019 veya 2017'yi yüklediğinizde, seçtiğinizden emin olun **Azure geliştirme** iş yükü.

  * [.NET için Microsoft Azure SDK (sürüm 2.9.1 veya sonrası)](https://azure.microsoft.com/downloads/). 
  [Azure SDK for .NET](https://docs.microsoft.com/dotnet/azure/dotnet-tools?view=azure-dotnet) hakkında daha fazla bilgi edinin.

  * [Azure PowerShell](https://github.com/Azure/azure-powershell#installation).

  * Azure Logic Apps araçları istediğiniz Visual Studio sürümü için:

    * [Visual Studio 2019](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2019)

    * [Visual Studio 2017](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2017)

    * [Visual Studio 2015](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2015)
  
    Azure Logic Apps Araçlarını doğrudan Visual Studio Market’ten indirip yükleyebilir veya [bu uzantıyı Visual Studio’nun içinden yükleme](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions) hakkında bilgi edinebilirsiniz. 
    Yükleme işlemini tamamladıktan sonra Visual Studio’yu yeniden başlattığınızdan emin olun.

  * [Azure işlevleri temel araçları](https://www.npmjs.com/package/azure-functions-core-tools) işlevleri yerel olarak hata ayıklama.

* Ekli mantıksal Uygulama Tasarımcısı kullanılırken web erişimi.

  Tasarımcının Azure'da kaynak oluşturması ve mantıksal uygulamanızdaki bağlayıcılardan özellik ve verileri okuması için İnternet bağlantısı gerekir. 
  Örneğin, Dynamics CRM Online bağlayıcısını kullanıyorsanız, tasarımcı CRM örneğinizdeki varsayılan ve özel kullanılabilir özellikleri denetler.

## <a name="create-a-resource-group-project"></a>Bir kaynak grubu projesi oluşturma

Başlamak için oluşturun bir [Azure kaynak grubu projesi](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) sunucusuz uygulamanız için. Azure'da kaynakları oluşturmak bir *kaynak grubu*, düzenleme, yönetme ve kaynaklar için tek bir varlık olarak tüm bir uygulamayı dağıtmak için kullandığınız mantıksal koleksiyonu. Azure'da sunucusuz bir uygulama için Azure Logic Apps ve Azure işlevleri için kaynak grubunuzun kaynakları içerir. [Azure kaynak grupları ve kaynakları](../azure-resource-manager/resource-group-overview.md) hakkında daha fazla bilgi edinin.

1. Visual Studio'yu başlatın ve Azure hesabınızı kullanarak oturum açın.

1. **Dosya** menüsünde **Yeni** > **Proje**’yi seçin.

   ![Visual Studio'da yeni proje oluşturma](./media/logic-apps-serverless-get-started-vs/create-new-project-visual-studio.png)

1. **Yüklü** altında **Visual C#** veya **Visual Basic**’i seçin. Ardından, **bulut** > **Azure kaynak grubu**.

   > [!NOTE]
   > Varsa **bulut** kategori veya **Azure kaynak grubu** proje mevcut değil, Visual Studio için Azure SDK'sını yüklediğinizden emin olun.

   Visual Studio 2019 kullanıyorsanız, aşağıdaki adımları izleyin:

   1. İçinde **yeni bir proje oluşturma** kutusunda **Azure kaynak grubu** ya da görsel için proje şablonu C# veya Visual Basic ve ardından **sonraki**.

   1. Ad ve Azure kaynak grubu için kullanmak istediğiniz diğer proje bilgileri sağlayın. İşiniz bittiğinde **Oluştur**’u seçin.

1. Projenize bir ad ve konum verin ve ardından **Tamam**.

   Visual Studio şablonları listeden bir şablon seçin isteyip istemediğinizi sorar. 
   Böylece, bir mantıksal uygulama ve bir Azure işlevine bir çağrı içeren sunucusuz bir uygulama oluşturabilirsiniz, bu örnek bir Azure Hızlı Başlangıç şablonu kullanır.

   > [!TIP]
   > Boş olmayan istediğiniz bir Azure kaynak grubuna çözümünüzü predeploy senaryolarda kullanabilirsiniz **mantıksal uygulama** şablon, yalnızca bir boş mantıksal uygulama oluşturur.

1. Gelen **bu konumdan şablonlar Göster** listesinden **Azure Hızlı Başlangıç (github.com/Azure/azure-quickstart-templates)** .

1. Arama kutusuna filtreniz olarak "logic-app" girin. Sonuçlar arasından seçim **101-logic-app-and-function-app** şablonu.

   ![Azure Hızlı Başlangıç şablonu seçin](./media/logic-apps-serverless-get-started-vs/select-template.png)

   Visual Studio oluşturur ve kaynak grubu projenizi bir çözümü açar. 
   Seçtiğiniz Azure Hızlı Başlangıç şablonu kaynak grubu projenizi içinde azuredeploy.json adlı bir dağıtım şablonu oluşturur. Bu dağıtım şablonu, bir HTTP isteği tarafından tetiklenip tetiklenmediğini, bir Azure işlevi çağırır ve sonucu olarak bir HTTP yanıtı döndüren basit bir mantıksal uygulama tanımını içerir.

   ![Yeni sunucusuz çözüm](./media/logic-apps-serverless-get-started-vs/create-serverless-solution.png)

1. Ardından, çözümünüzü Azure'a dağıtın. Dağıtım şablonunu açın ve kaynakları gözden geçirin, sunucusuz uygulamanız için önce bunu yapmanız gerekir.

## <a name="deploy-your-solution"></a>Çözümünüzü dağıtın

Visual Studio'da Logic App Tasarımcısı'nda mantıksal uygulamanızı açabilmek için önce zaten Azure'da dağıtılan bir Azure kaynak grubu olması gerekir. Tasarımcı, sonra mantıksal uygulamanızın kaynaklarını ve Hizmetleri için bağlantı oluşturabilirsiniz. Bu görev için Azure portalına Visual Studio'dan çözümünüzü dağıtmak için aşağıdaki adımları izleyin:

1. Çözüm Gezgini'nde, kaynak projenizin kısayol menüsünden seçin **Dağıt** > **yeni**.

   ![Kaynak grubu için yeni bir dağıtımını oluşturun](./media/logic-apps-serverless-get-started-vs/deploy.png)

1. Henüz seçili, Azure aboneliğinizi ve dağıtmak istediğiniz kaynak grubunu seçin. Ardından, **Dağıt**.

   ![Dağıtım ayarları](./media/logic-apps-serverless-get-started-vs/deploy-to-resource-group.png)

1. Varsa **parametreleri Düzenle** kutusu görüntülenirse, mantıksal uygulamanızı ve dağıtım sırasında Azure işlev uygulaması için kullanılacak kaynak adları belirtin ve ardından ayarlarınızı kaydedin. İşlev uygulamanız için genel olarak benzersiz bir ad kullandığınızdan emin olun.

   ![Mantıksal uygulamanızı ve işlev uygulaması için adlar sağlayın](./media/logic-apps-serverless-get-started-vs/logic-function-app-name-parameters.png)

   Visual Studio, belirtilen kaynak grubuna dağıtım başladığında Visual Studio çözümünüzün dağıtım durumu görünür **çıkış** penceresi. 
   Dağıtım tamamlandıktan sonra mantıksal uygulamanızı Azure portalında zaten çalışıyor.

## <a name="edit-your-logic-app-in-visual-studio"></a>Mantıksal uygulamanızı Visual Studio'da Düzenle

Dağıtım sonrasında mantıksal uygulamanızı düzenlemek için Visual Studio'da Logic Apps Tasarımcısı kullanarak mantıksal uygulamanızı açın.

1. Çözüm Gezgini'nde azuredeploy.json dosyasının kısayol menüsünden seçin **açık ile mantıksal Uygulama Tasarımcısı**.

   ![Azuredeploy.JSON Logic Apps Tasarımcısı'nda açın.](./media/logic-apps-serverless-get-started-vs/open-logic-app-designer.png)

   > [!TIP]
   > Bu komut Visual Studio 2019 yoksa, Visual Studio için en son güncelleştirmelere sahip olduğunu denetleyin.

1. Sonra **mantıksal uygulama özellikleri** kutusu görüntülenirse, altında **abonelik**, henüz seçili değilse, Azure aboneliğinizi seçin. Altında **kaynak grubu**çözümünüzün dağıtıldığı konum ve kaynak grubu seçin ve ardından **Tamam**.

   ![Mantıksal uygulama özellikleri](./media/logic-apps-serverless-get-started-vs/logic-app-properties.png)

   Logic Apps Tasarımcısı açılır sonra adımları eklemeye devam etmek veya iş akışını değiştirme ve güncelleştirmelerinizi kaydedin.

   ![Mantıksal Uygulama Tasarımcısı'nda açılan mantıksal uygulama](./media/logic-apps-serverless-get-started-vs/opened-logic-app.png)

## <a name="create-your-azure-functions-project"></a>Azure işlevleri projenizi oluşturun

JavaScript, Python kullanarak işlevi ve işlev projesi oluşturmak için F#, PowerShell, toplu veya Bash adımları izleyin [iş ile Azure işlevleri çekirdek Araçları](../azure-functions/functions-run-local.md). Azure işlevinizi kullanarak geliştirmeyi C# çözümünüz içinde kullanmayı bir C# Sınıf Kitaplığı'ndaki adımları izleyerek [.NET sınıf kitaplığı bir işlev uygulaması olarak Yayımla](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/16/publishing-a-net-class-library-as-a-function-app/).

## <a name="deploy-functions-from-visual-studio"></a>İşlevleri Visual Studio'dan dağıtma

Azuredeploy.json dosyasının değişkenleri tarafından belirtilen Git deposundan çözümünüzdeki sahip herhangi bir Azure işlevleri, dağıtım şablonu dağıtır. Oluşturursanız ve İşlevler projeniz çözümünüzde yazar, proje Git kaynak denetimine (örneğin, GitHub veya Azure DevOps) kontrol edin ve ardından güncelleştirme `repo` değişken böylece şablonu, Azure işleviniz dağıtır.

## <a name="manage-logic-apps-and-view-run-history"></a>Logic apps ve çalıştırma geçmişi görünümü yönetme

Yine de zaten Azure'da dağıtılan logic apps için düzenleme, yönetme, çalıştırma geçmişini görüntülemek ve söz konusu uygulamaları Visual Studio'dan devre dışı bırakmak.

1. Gelen **görünümü** Visual Studio'da Aç menüsünde **Cloud Explorer**.

1. Altında **tüm abonelikleri**, yönetmek ve ardından istediğiniz logic apps ile ilişkili Azure aboneliği seçin **Uygula**.

1. Altında **Logic Apps**, mantıksal uygulamanızı seçin. Bu uygulamanın kısayol menüsünden seçin **Logic App Düzenleyicisi ile açın**.

   > [!TIP]
   > Bu komut Visual Studio 2019 yoksa, Visual Studio için en son güncelleştirmelere sahip olduğunu denetleyin.

Kaynak grubu projenize zaten yayımlanan bir mantıksal uygulama şimdi yükleyebilirsiniz. Bu nedenle, Azure portalında bir mantıksal uygulama başlatıldı olsa da, yine de içeri aktarabilir ve bu uygulamayı Visual Studio'da yönetme. Daha fazla bilgi için [Visual Studio ile mantıksal uygulamaları yönetme](../logic-apps/manage-logic-apps-with-visual-studio.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio ile mantıksal uygulamaları yönetme](manage-logic-apps-with-visual-studio.md)
