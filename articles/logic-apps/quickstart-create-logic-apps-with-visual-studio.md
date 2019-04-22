---
title: Visual Studio - Azure Logic Apps ile otomatik iş akışları oluşturma
description: Azure Logic Apps ve Visual Studio kullanarak görevleri, iş süreçlerini ve kurumsal tümleştirme için iş akışlarını otomatikleştirin
services: logic-apps
ms.service: logic-apps
ms.workload: azure-vs
author: ecfan
ms.author: estfan
ms.topic: quickstart
ms.custom: mvc
ms.reviewer: klam, LADocs
ms.suite: integration
ms.date: 04/02/2019
ms.openlocfilehash: 10ed3ec8b29048a7ede51a6d98e9f1ebb7f44cf6
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58862990"
---
# <a name="quickstart-create-automated-tasks-processes-and-workflows-with-azure-logic-apps---visual-studio"></a>Hızlı Başlangıç: Azure Logic Apps - Visual Studio ile otomatik görevler, süreçleri ve iş akışları oluşturma

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve Visual Studio ile uygulama, veri, sistem ve hizmetleri kurum ve kuruluşlar arasında otomatik hale getiren iş akışları oluşturabilirsiniz. Bu hızlı başlangıçta nasıl tasarım ve Visual Studio'da mantıksal uygulamalar oluşturup bu uygulamaları bulutta azure'a dağıtma bu iş akışları oluşturma gösterilmektedir. Bu görevleri, Azure portalında gerçekleştirebilirsiniz ancak, Visual Studio, kaynak denetimi, farklı sürümler yayımlayabilir ve farklı dağıtım ortamları için Azure Resource Manager şablonları oluşturmak için mantıksal uygulamalarınızı eklemenizi sağlar.

Azure Logic Apps kullanmaya yeni başladıysanız ve yalnızca temel kavramları istiyorsanız, bunun yerine [Azure portalında mantıksal uygulama oluşturmak için hızlı başlangıç](../logic-apps/quickstart-create-first-logic-app-workflow.md) makalesini deneyin. Mantıksal Uygulama Tasarımcısı hem Azure portalında hem de Visual Studio’da benzer şekilde çalışır.

Burada, aynı mantıksal uygulamayı Azure portalı hızlı başlangıcında bu kez Visual Studio ile oluşturursunuz. Bu mantıksal uygulama bir web sitesinin RSS akışını izler ve sitede yayınlanan her yeni öğe için e-posta gönderir. İşlemi tamamladığınızda mantıksal uygulamanız şu yüksek düzeyli iş akışı gibi görünür:

![Tamamlanmış mantıksal uygulama](./media/quickstart-create-logic-apps-with-visual-studio/overview.png)

<a name="prerequisites"></a>

Başlamadan önce bu hızlı başlangıcı takip için bu öğeleri sahip olduğunuzdan emin olun:

* Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Henüz yoksa şu araçları indirip yükleyin:

  * <a href="https://aka.ms/download-visual-studio" target="_blank">Visual Studio 2019, 2017 veya 2015 - Community sürümü veya üzeri</a>. 
  Bu hızlı başlangıçta ücretsiz olan Visual Studio Community 2017 kullanılmaktadır.

    > [!IMPORTANT]
    > Visual Studio 2019 veya 2017'yi yüklediğinizde, seçtiğinizden emin olun **Azure geliştirme** iş yükü.
    > Visual Studio 2019 için Cloud Explorer Azure portalında mantıksal Uygulama Tasarımcısı açabilirsiniz, ancak henüz ekli mantıksal Uygulama Tasarımcısı açılamıyor.

  * <a href="https://azure.microsoft.com/downloads/" target="_blank">.NET için Microsoft Azure SDK (2.9.1 veya sonrası)</a>. <a href="https://docs.microsoft.com/dotnet/azure/dotnet-tools?view=azure-dotnet">Azure SDK for .NET</a> hakkında daha fazla bilgi edinin.

  * <a href="https://github.com/Azure/azure-powershell#installation" target="_blank">Azure PowerShell</a>

  * Azure Logic Apps araçları istediğiniz Visual Studio sürümü için:

    * <a href="https://aka.ms/download-azure-logic-apps-tools-visual-studio-2019" target="_blank">Visual Studio 2019</a>
    
    * <a href="https://aka.ms/download-azure-logic-apps-tools-visual-studio-2017" target="_blank">Visual Studio 2017</a>
    
    * <a href="https://aka.ms/download-azure-logic-apps-tools-visual-studio-2015" target="_blank">Visual Studio 2015</a>
  
    Azure Logic Apps Araçlarını doğrudan Visual Studio Market’ten indirip yükleyebilir veya <a href="https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions" target="_blank">bu uzantıyı Visual Studio’nun içinden yükleme</a> hakkında bilgi edinebilirsiniz. 
    Yükleme işlemini tamamladıktan sonra Visual Studio’yu yeniden başlattığınızdan emin olun.

* Ekli Mantıksal Uygulama Tasarımcısı kullanılırken web erişimi

  Tasarımcının Azure'da kaynak oluşturması ve mantıksal uygulamanızdaki bağlayıcılardan özellik ve verileri okuması için İnternet bağlantısı gerekir. 
  Örneğin, Dynamics CRM Online bağlayıcısını kullanıyorsanız, tasarımcı CRM örneğinizdeki varsayılan ve özel kullanılabilir özellikleri denetler.

* Logic Apps tarafından desteklenen Office 365 Outlook, Outlook.com veya Gmail gibi bir e-posta hesabı. Diğer sağlayıcılar için <a href="https://docs.microsoft.com/connectors/" target="_blank">buradaki bağlayıcı listesini inceleyin</a>. Bu mantıksal uygulama Office 365 Outlook kullanır. Farklı bir sağlayıcı kullanıyorsanız genel adımlar aynıdır, ancak kullanıcı arabirimi biraz farklı olabilir.

## <a name="create-azure-resource-group-project"></a>Azure kaynak grubu projesi oluşturma

Başlamak için bir [Azure Kaynak Grubu projesi](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) oluşturun. [Azure kaynak grupları ve kaynakları](../azure-resource-manager/resource-group-overview.md) hakkında daha fazla bilgi edinin.

1. Visual Studio'yu başlatın ve Azure hesabınızla oturum açın.

1. **Dosya** menüsünde **Yeni** > **Proje**’yi seçin. (Klavye: Ctrl+Shift+N)

   !["Dosya" menüsünde "Yeni" > "Proje" öğesini seçin](./media/quickstart-create-logic-apps-with-visual-studio/create-new-visual-studio-project.png)

1. **Yüklü** altında **Visual C#** veya **Visual Basic**’i seçin. **Bulut** > **Azure Kaynak Grubu**’nu seçin. Projenizi adlandırın, örneğin:

   ![Azure Kaynak Grubu projesi oluşturma](./media/quickstart-create-logic-apps-with-visual-studio/create-azure-cloud-service-project.png)

   > [!NOTE]
   > Varsa **bulut** kategori veya **Azure kaynak grubu** proje mevcut değil, Visual Studio için Azure SDK'sı yüklü olduğundan emin olun.

   Visual Studio 2019 kullanıyorsanız, aşağıdaki adımları izleyin:

   1. İçinde **yeni bir proje oluşturma** kutusunda **Azure kaynak grubu** ya da görsel için proje şablonu C# veya Visual Basic ve **sonraki**.

   1. Kullanmak istediğiniz Azure kaynak grubu ve diğer proje bilgileri için ad belirtin. İşiniz bittiğinde **Oluştur**’u seçin.

1. Şablon listesinden **mantıksal uygulama** şablonu.

   ![Mantıksal Uygulama şablonunu seçme](./media/quickstart-create-logic-apps-with-visual-studio/select-logic-app-template.png)

   Visual Studio projenizi oluşturduktan sonra, Çözüm Gezgini açılır ve çözümünüzü gösterir.

   ![Çözüm Gezgini yeni mantıksal uygulama çözümünü ve dağıtım dosyasını gösterir](./media/quickstart-create-logic-apps-with-visual-studio/logic-app-solution-created.png)

   Çözümünüzde **LogicApp.json** dosyası yalnızca mantıksal uygulamanızın tanımını depolamaz, aynı zamanda dağıtım için ayarlayabileceğiniz bir Azure Resource Manager şablonudur.

## <a name="create-blank-logic-app"></a>Boş mantıksal uygulama oluşturma

Azure Kaynak Grubu projenizi oluşturduktan sonra **Boş Mantıksal Uygulama** şablonundan başlayarak mantıksal uygulamanızı oluşturup derleyin.

1. Çözüm Gezgini'nde **LogicApp.json** dosyasının kısayol menüsünü açın. 
   **Mantıksal Uygulama Tasarımcısı ile Aç**’ı seçin. (Klavye: Ctrl+L)

   ![Mantıksal Uygulama Tasarımcısı ile mantıksal uygulama .json dosyasını açma](./media/quickstart-create-logic-apps-with-visual-studio/open-logic-app-designer.png)

1. **Abonelik** için kullanmak istediğiniz Azure aboneliğini seçin. 
   **Kaynak Grubu** için **Yeni Oluştur...**  öğesini seçerek yeni bir Azure kaynak grubu oluşturun.

   ![Azur aboneliği, kaynak grubu ve kaynak konumu seçme](./media/quickstart-create-logic-apps-with-visual-studio/select-azure-subscription-resource-group-location.png)

   Visual Studio, mantıksal uygulamanızla ilişkili kaynakları ve bağlantıları oluşturup dağıtmak için Azure aboneliğinize ve bir kaynak grubuna ihtiyaç duyar.

   | Ayar | Örnek değer | Açıklama |
   | ------- | ------------- | ----------- |
   | Kullanıcı profili listesi | Contoso <br> jamalhartnett@contoso.com | Varsayılan olarak oturum açmak için kullandığınız hesap |
   | **Abonelik** | Kullandıkça Öde <br> (jamalhartnett@contoso.com) | Azure aboneliğinizin ve ilişkili hesabın adı |
   | **Kaynak Grubu** | MyLogicApp-RG <br> (Batı ABD) | Azure kaynak grubu ve mantıksal uygulamanızın kaynaklarını depolama ve dağıtma konumu |
   | **Konum** | MyLogicApp-RG2 <br> (Batı ABD) | Kaynak grubu konumunu kullanmak istemiyorsanız farklı bir konum |
   ||||

1. Logic Apps Tasarımcısı açılır ve bir tanıtım videosu ile sık kullanılan tetikleyicilerin bulunduğu bir sayfa görüntülenir. 
   Video ve tetikleyicileri kaydırın. **Şablonlar** altında **Boş Mantıksal Uygulama**'yı seçin.

   !["Boş Mantıksal Uygulama" seçme](./media/quickstart-create-logic-apps-with-visual-studio/choose-blank-logic-app-template.png)

## <a name="build-logic-app-workflow"></a>Mantıksal uygulama iş akışı derleme

Sonra, yeni bir RSS akışı öğesi göründüğünde tetiklenen bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts) ekleyin. Her mantıksal uygulama belirli ölçütler karşılandığında başlatılan bir tetikleyici ile başlamalıdır. Tetikleyici her etkinleştirildiğinde Logic Apps altyapısı iş akışınızı çalıştıran bir mantıksal uygulama örneği oluşturur.

1. Mantıksal Uygulama Tasarımcısı’nda arama kutusuna "rss" yazın. Şu tetikleyiciyi seçin: **Akış öğesi yayımlandığında**

   ![Tetikleyici ve eylemler ekleyerek mantıksal uygulamanızı derleme](./media/quickstart-create-logic-apps-with-visual-studio/add-trigger-logic-app.png)

   Tetikleyici artık tasarımcıda görünür:

   ![RSS tetikleyicisi Mantıksal Uygulama Tasarımcısı’nda görünür](./media/quickstart-create-logic-apps-with-visual-studio/rss-trigger-logic-app.png)

1. Mantıksal uygulama oluşturma işlemini tamamlamak için [Azure portalı hızlı başlangıcı](../logic-apps/quickstart-create-first-logic-app-workflow.md#add-rss-trigger) içindeki iş akışı adımlarını izleyin, sonra bu makaleye geri dönün.

   İşiniz bittiğinde mantıksal uygulamanız şu örnekteki gibi görünür:

   ![Tamamlanmış mantıksal uygulama](./media/quickstart-create-logic-apps-with-visual-studio/finished-logic-app.png)

1. Mantıksal uygulamanızı kaydetmek için Visual Studio çözümünüzü kaydedin. (Klavye: Ctrl + S)

Şimdi mantıksal uygulamanızı test edebilmemiz için uygulamanızı Azure’a dağıtın.

## <a name="deploy-logic-app-to-azure"></a>Mantıksal uygulamanızı Azure'a dağıtma

Mantıksal uygulamanızı çalıştırabilmeniz için uygulamanızı yalnızca birkaç adımda Visual Studio’dan Azure'a dağıtın.

1. Çözüm Gezgini'nde projenizin kısayol menüsünden **Dağıt** > **Yeni**'yi seçin. Sorulursa Azure hesabınızla oturum açın.

   ![Mantıksal uygulama dağıtımı oluşturma](./media/quickstart-create-logic-apps-with-visual-studio/create-logic-app-deployment.png)

1. Bu dağıtım için Azure aboneliği, kaynak grubu ve diğer varsayılan ayarları değiştirmeyin. Hazır olduğunuzda **Dağıt**’ı seçin.

   ![Mantıksal uygulamayı Azure kaynak grubuna dağıtma](./media/quickstart-create-logic-apps-with-visual-studio/select-azure-subscription-resource-group-deployment.png)

1. **Parametreleri Düzenle** kutusu görüntülenirse, mantıksal uygulamanın dağıtımda kullanacağı kaynak adını belirtin, ardından ayarlarınızı kaydedin, örneğin:

   ![Mantıksal uygulama için dağıtım adı belirtme](./media/quickstart-create-logic-apps-with-visual-studio/edit-parameters-deployment.png)

   Dağıtım başladığında uygulamanızın dağıtım durumu Visual Studio **Çıktı** penceresinde görünür. 
   Durum görünmezse **Çıktıyı göster** listesini açıp Azure kaynak grubunuzu seçin.

   ![Dağıtım durumu çıktısı](./media/quickstart-create-logic-apps-with-visual-studio/logic-app-output-window.png)

   Seçtiğiniz bağlayıcılar bir şey yazmanızı gerektiriyorsa, arka planda bir PowerShell penceresi açılabilir ve gerekli parolaları veya gizli anahtarları isteyebilir. Bu bilgileri girdikten sonra dağıtım işlemi devam eder.

   ![Dağıtım powershell_window](./media/quickstart-create-logic-apps-with-visual-studio/logic-apps-powershell-window.png)

   Dağıtım tamamlandıktan sonra mantıksal uygulamanız Azure portalında etkindir ve belirttiğiniz zamanlamaya göre (dakikada bir kez) RSS akışını denetler. 
   RSS akışında yeni öğeler olduğunda mantıksal uygulamanız her yeni öğe için bir e-posta gönderir. 
   Aksi takdirde mantıksal uygulamanız yeniden denetlemek için bir sonraki zaman aralığını bekler.

   Örneğin, bu mantıksal uygulamanın gönderdiği örnek e-postalar aşağıda verilmiştir. 
   E-posta gelmezse istenmeyen e-posta klasörüne bakın.

   ![Outlook her yeni RSS öğesi için e-posta gönderir](./media/quickstart-create-logic-apps-with-visual-studio/outlook-email.png)

   Teknik olarak tetikleyici, RSS akışını denetleyip yeni öğeler bulduğunda tetikleyici etkinleşir ve Logic Apps altyapısı, mantıksal uygulama iş akışınızın, iş akışında eylemleri çalıştıran bir örneğini oluşturur.
   Tetikleyici yeni öğeler bulmazsa tetikleyici etkinleşmez ve iş akışı örneğini oluşturma işlemini "atlar".

Tebrikler, Visual Studio ile mantıksal uygulamanızı başarıyla derleyip dağıttınız! Mantıksal uygulamanızı yönetmek ve çalıştırma geçmişini gözden geçirmek için bkz. [Visual Studio ile mantıksal uygulamaları yönetme](../logic-apps/manage-logic-apps-with-visual-studio.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerek kalmadığında mantıksal uygulamanızı ve ilgili kaynakları içeren kaynak grubunu silin.

1. Mantıksal uygulamanızı oluşturmak için kullandığınız hesapla <a href="https://portal.azure.com" target="_blank">Azure portalında</a> oturum açın.

1. Azure menüsünde **Kaynak grupları**'nı seçin.
Mantıksal uygulamanızın kaynak grubunu ve ardından **Genel bakış**'ı seçin.

1. **Genel Bakış** sayfasında **Kaynak grubunu sil**’i seçin. Onay olarak kaynak grubunun adını girip **Sil**’i seçin.

   !["Kaynak grupları" > "Genel bakış" > "Kaynak grubunu sil"](./media/quickstart-create-logic-apps-with-visual-studio/delete-resource-group.png)

1. Visual Studio çözümünü yerel bilgisayarınızdan silin.

## <a name="get-support"></a>Destek alın

* Sorularınız için <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">Azure Logic Apps forumunu</a> ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için <a href="https://aka.ms/logicapps-wish" target="_blank">Logic Apps kullanıcı geri bildirimi sitesini</a> ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede Visual Studio kullanarak mantıksal uygulamanızı derlediniz, dağıttınız ve çalıştırdınız. Visual Studio ile mantıksal uygulamalarda gelişmiş dağıtımı yönetme ve gerçekleştirme hakkında daha fala bilgi almak için şu makalelere bakın:

> [!div class="nextstepaction"]
> * [Visual Studio ile mantıksal uygulamaları yönetme](../logic-apps/manage-logic-apps-with-visual-studio.md)
> * [Visual Studio ile mantıksal uygulamalar için dağıtım şablonları oluşturma](../logic-apps/logic-apps-create-deploy-template.md)
