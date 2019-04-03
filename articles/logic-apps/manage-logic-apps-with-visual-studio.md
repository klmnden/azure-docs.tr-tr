---
title: Visual Studio - Azure Logic Apps ile mantıksal uygulamaları yönetme
description: Logic apps ve diğer Azure varlıklarınızdan Visual Studio Cloud Explorer ile yönetme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.custom: mvc
ms.date: 04/02/2019
ms.openlocfilehash: 9654caca5fd4b1f79544ea7303a5d3fff72d22f8
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58862752"
---
# <a name="manage-logic-apps-with-visual-studio"></a>Visual Studio ile mantıksal uygulamaları yönetme

Oluşturabilirseniz de düzenleme, yönetme ve logic apps'te dağıtma <a href="https://portal.azure.com" target="_blank">Azure portalında</a>, kaynak denetimi, farklı sürümler yayımlayabilir ve oluşturmak için logic apps eklemek istediğiniz zaman Visual Studio'yu da kullanabilirsiniz [Azure Kaynak Yöneticisi'ni](../azure-resource-manager/resource-group-overview.md) çeşitli dağıtım ortamları için şablonlar. Visual Studio Cloud Explorer ile bulun ve diğer Azure kaynakları ile birlikte mantıksal uygulamalarınızı yönetin. Örneğin, açın, indirme, düzenleme, çalıştırma, çalıştırma geçmişi, devre dışı bırakma ve zaten dağıtılmış olan etkinleştirme logic apps, Azure portalında görüntülemek. Visual Studio'da Azure Logic Apps ile çalışmaya yeni başladıysanız, bilgi [Visual Studio ile mantıksal uygulamalar oluşturmak nasıl](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

> [!IMPORTANT]
> Azure Portalı'nda, uygulama sürümü, dağıtma veya Visual Studio'dan bir mantıksal uygulama yayımlama üzerine yazar. Tutmak istediğiniz Azure portalında değişiklik yaparsanız, bu nedenle emin olun, [mantıksal uygulamayı Visual Studio'da yenileme](#refresh) dağıtın veya Visual Studio'dan yayımlama sonraki süreden önce Azure portalından.

<a name="requirements"></a>

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Henüz yoksa şu araçları indirip yükleyin: 

  * <a href="https://aka.ms/download-visual-studio" target="_blank">Visual Studio 2019, 2017 veya 2015 - Community sürümü veya üzeri</a>. 
  Bu hızlı başlangıçta ücretsiz olan Visual Studio Community 2017 kullanılmaktadır.

    > [!IMPORTANT]
    > Visual Studio 2019 veya 2017'yi yüklediğinizde, seçtiğinizden emin olun **Azure geliştirme** iş yükü.
    > Daha fazla bilgi için [şekilde Azure hesaplarınızı Visual Studio Cloud Explorer ile ilişkili kaynakları yönetme](https://docs.microsoft.com/visualstudio/azure/vs-azure-tools-resources-managing-with-cloud-explorer?view).
    >
    > Visual Studio 2019, Cloud Explorer Azure portalında mantıksal Uygulama Tasarımcısı açabilirsiniz, ancak henüz ekli mantıksal Uygulama Tasarımcısı açılamıyor.

    Visual Studio 2015 için cloud Explorer'ı yüklemek için [Cloud Explorer'ı Visual Studio Market'ten indir](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVisualStudio2015). 
    Daha fazla bilgi için [Azure hesaplarınızı Visual Studio Cloud Explorer (2015) ile ilişkili kaynakları yönetme](https://docs.microsoft.com/visualstudio/azure/vs-azure-tools-resources-managing-with-cloud-explorer?view=vs-2015).

  * <a href="https://azure.microsoft.com/downloads/" target="_blank">Azure SDK (2.9.1 veya sonrası)</a> 

  * <a href="https://github.com/Azure/azure-powershell#installation" target="_blank">Azure PowerShell</a>

  * Azure Logic Apps araçları istediğiniz Visual Studio sürümü için:

    * <a href="https://aka.ms/download-azure-logic-apps-tools-visual-studio-2019" target="_blank">Visual Studio 2019</a>
    
    * <a href="https://aka.ms/download-azure-logic-apps-tools-visual-studio-2017" target="_blank">Visual Studio 2017</a>
    
    * <a href="https://aka.ms/download-azure-logic-apps-tools-visual-studio-2015" target="_blank">Visual Studio 2015</a>

    Azure Logic Apps Araçlarını doğrudan Visual Studio Market’ten indirip yükleyebilir veya <a href="https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions" target="_blank">bu uzantıyı Visual Studio’nun içinden yükleme</a> hakkında bilgi edinebilirsiniz. 
    Yükleme işlemini tamamladıktan sonra Visual Studio’yu yeniden başlattığınızdan emin olun.

* Katıştırılmış Logic Apps Tasarımcısı kullanılırken web erişimi

  Tasarımcının Azure'da kaynak oluşturması ve mantıksal uygulamanızdaki bağlayıcılardan özellik ve verileri okuması için İnternet bağlantısı gerekir. 
  Örneğin, Dynamics CRM Online bağlayıcısını kullanıyorsanız, tasarımcı CRM örneğinizdeki varsayılan ve özel kullanılabilir özellikleri denetler.

<a name="find-logic-apps-vs"></a>

## <a name="find-your-logic-apps"></a>Logic apps bulun

Visual Studio'da Azure aboneliğinizle ilişkili ve Cloud Explorer'ı kullanarak Azure Portalı'nda dağıtılan tüm mantıksal uygulamalar bulabilirsiniz.

1. Visual Studio'yu açın. Üzerinde **görünümü** menüsünde **Cloud Explorer**.

2. Bulut Gezgini'nde **hesap yönetimi**. Logic apps ile ilişkili Azure aboneliği seçin ve ardından **Uygula**. Örneğin:

   !["Hesap Yönetimi" seçin](./media/manage-logic-apps-with-visual-studio/account-management-select-Azure-subscription.png)

2. Bağlı olup olmadığını tarafından aramakta olduğunuz üzerinde **kaynak grupları** veya **kaynak türleri**, şu adımları izleyin:

   * **Kaynak grupları**: Azure aboneliğiniz kapsamındaki Cloud Explorer bu abonelikle ilişkili olan tüm kaynak gruplarını gösterir. 
   Mantıksal uygulamanızı içeren kaynak grubunu genişletin ve ardından mantıksal uygulamanızı seçin.

   * **Kaynak türleri**: Azure aboneliğiniz kapsamındaki genişletin **Logic Apps**. Cloud Explorer'ı aboneliğinizle ilişkili olan tüm dağıtılan mantıksal uygulamalar görüntüledikten sonra mantıksal uygulamanızı seçin.

<a name="open-designer"></a>

## <a name="open-in-visual-studio"></a>Visual Studio'da aç

Visual Studio'da mantıksal uygulamalar daha önce oluşturulan ve doğrudan Azure portalından veya Azure Resource Manager projeleri Visual Studio ile olarak dağıtılan açabilirsiniz.

1. Cloud Explorer'ı açın ve mantıksal uygulamanızı bulun. 

2. Mantıksal uygulamanın kısayol menüsünde **Logic App Düzenleyicisi ile açın**.

   Bu örnek, logic apps kaynak türü tarafından gösterir, logic apps altında görünecek şekilde **Logic Apps** bölümü.

   ![Azure portalından açık dağıtılan mantıksal uygulama](./media/manage-logic-apps-with-visual-studio/open-logic-app-in-editor.png)

   Mantıksal Uygulama Tasarımcısı'nın altındaki Logic Apps Tasarımcısı'nda açıldıktan sonra seçebileceğiniz **kod görünümü** böylece mantıksal uygulama tanımı temelindeki gözden geçirebilirsiniz. 
   Mantıksal uygulama için bir dağıtım şablonu oluşturmak istiyorsanız, bilgi [bir Azure Resource Manager şablonu indirmek nasıl](#download-logic-app) bu mantıksal uygulama için. Daha fazla bilgi edinin [Resource Manager şablonları](../azure-resource-manager/resource-group-overview.md#template-deployment).

<a name="download-logic-app"></a>

## <a name="download-from-azure"></a>Azure'dan indirin

Logic apps'ten indirebileceğiniz <a href="https://portal.azure.com" target="_blank">Azure portalında</a> ve bunları olarak Kaydet [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) şablonları. Ardından yerel olarak Visual Studio şablonlarıyla düzenleyebilir ve logic apps farklı dağıtım ortamları için özelleştirin. Mantıksal uygulamalar otomatik olarak yüklenmesini *parametreleştiren* tanımlarını içinde [Resource Manager şablonları](../azure-resource-manager/resource-group-overview.md#template-deployment), JavaScript nesne gösterimi (JSON) de kullanın.

1. Visual Studio'daki bulut Gezgini'ni açın ardından bulun ve Azure'dan indirmek istediğiniz bir mantıksal uygulama seçin.

2. Bu uygulamanın kısayol menüsünde **Logic App Düzenleyicisi ile açın**.

   Logic Apps Tasarımcısı açılır ve mantıksal uygulama gösterilir. 
   Mantıksal uygulamanın temel aldığı tanımını ve yapısı, Tasarımcı, alt kısmındaki gözden geçirmek için seçin **kod görünümü**. 

3. Tasarımcı araç çubuğunda **indirme**.

   !["İndir" seçin](./media/manage-logic-apps-with-visual-studio/download-logic-app.png)

4. Bir konumu istendiğinde, bu konuma göz atın ve mantıksal uygulama tanımını için Resource Manager şablonu JSON (.json) dosya biçiminde kaydedin. 

Mantıksal uygulama tanımınızı görünür `resources` Resource Manager şablonu içinde alt. Şimdi mantıksal uygulama tanımını ve Visual Studio ile Resource Manager şablonunu da düzenleyebilirsiniz. Ayrıca, bir Azure Resource Manager projesi olarak Visual Studio çözüm şablonu ekleyebilirsiniz. Hakkında bilgi edinin [Resource Manager projeleri Visual Studio'da logic apps için](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md). 

<a name="refresh"></a>

## <a name="refresh-from-azure"></a>Azure'dan Yenile

Azure portalında mantıksal uygulamanızı düzenlemek ve bu değişiklikleri tutmak istiyorsanız, bu uygulamanın Visual Studio sürümünde bu değişikliklerle yenileme emin olun. 

* Visual Studio'da mantıksal Uygulama Tasarımcısı araç çubuğunda **Yenile**.

  -veya-

* Visual Studio Cloud Explorer, mantıksal uygulamanızın kısayol menüsünü açın ve seçin **Yenile**. 

![Mantıksal uygulama güncelleştirmeleri ile yenileme](./media/manage-logic-apps-with-visual-studio/refresh-logic-app.png)

## <a name="publish-logic-app-updates"></a>Mantıksal uygulama güncelleştirmeleri yayımlama

Mantıksal uygulama güncelleştirmelerinizi Visual Studio'dan azure'a mantıksal Uygulama Tasarımcısı araç çubuğunda dağıtmaya hazır olduğunuzda seçin **Yayımla**.

![Güncelleştirilmiş bir mantıksal uygulama yayımlama](./media/manage-logic-apps-with-visual-studio/publish-logic-app.png)

## <a name="manually-run-your-logic-app"></a>Mantıksal uygulamanızı el ile çalıştırın

Visual Studio'dan azure'da dağıtılan bir mantıksal uygulama el ile tetikleyebilirsiniz. Mantıksal Uygulama Tasarımcısı araç çubuğunda **tetikleyici çalıştırması**.

![El ile mantık uygulaması Çalıştır](./media/manage-logic-apps-with-visual-studio/manually-run-logic-app.png)

## <a name="review-run-history"></a>Çalıştırma geçmişini gözden geçirme

Mantıksal uygulama çalıştırmaları özelliğiyle sorunları tanılayın ve durumu denetlemek için Visual Studio'da girişler ve çıkışlar, bunlar için çalıştırdığı gibi ayrıntıları gözden geçirebilirsiniz.

1. Bulut Gezgini'nde mantıksal uygulamanızın kısayol menüsünü açın ve seçin **açık çalıştırma geçmişini**.

   ![Çalıştırma geçmişi Aç](./media/manage-logic-apps-with-visual-studio/view-run-history.png)

2. Belirli bir çalıştırma ayrıntılarını görüntülemek için bir çift tıklayın. Örneğin:

   ![Ayrıntılı çalıştırma geçmişi](./media/manage-logic-apps-with-visual-studio/view-run-history-details.png)
  
   > [!TIP]
   > Tablo özelliğe göre sıralamak için bu özellik için sütun başlığını seçin. 

3. Adımları, giriş ve çıkışları gözden geçirmek istediğiniz genişletin. Örneğin:

   ![Giriş ve çıkışları her adım için görüntüleyin](./media/manage-logic-apps-with-visual-studio/run-inputs-outputs.png)

## <a name="disable-or-enable-logic-app"></a>Mantıksal uygulama etkinleştirmek veya devre dışı

Mantıksal uygulamanızı silmeden tetikleyici koşul gerçekleştiğinde, sonraki açışınızda tetikleme gelen tetikleyici durdurabilirsiniz. Mantıksal uygulamanızı devre dışı bırakılması, Logic Apps altyapısı oluşturma ve gelecekteki iş akışı örnekleri için mantıksal uygulamanızı çalıştıran engeller.
Bulut Gezgini'nde mantıksal uygulamanızın kısayol menüsünü açın ve seçin **devre dışı**.

![Mantıksal uygulamanızı devre dışı bırak](./media/manage-logic-apps-with-visual-studio/disable-logic-app.png)

> [!NOTE]
> Mantıksal uygulama devre dışı bıraktığınızda, hiçbir yeni çalıştırmaları örneği oluşturulur. Tüm süren ve bunlar, tamamlanması uzun sürebilir bitene kadar çalıştırmalar devam eder. 

İşlemi sürdürmek mantıksal uygulamanız için hazır olduğunuzda, mantıksal uygulamanızı yeniden etkinleştirebilir. Bulut Gezgini'nde mantıksal uygulamanızın kısayol menüsünü açın ve seçin **etkinleştirme**.

![Mantıksal uygulamanızı etkinleştirme](./media/manage-logic-apps-with-visual-studio/enable-logic-app.png)

## <a name="delete-your-logic-app"></a>Mantıksal uygulamanızı silme

Azure portalında Cloud Explorer, mantıksal uygulamanızı silmek için mantıksal uygulamanızın kısayol menüsünü açın ve seçin **Sil**.

![Mantıksal uygulamanızı silme](./media/manage-logic-apps-with-visual-studio/delete-logic-app.png)

> [!NOTE]
> Mantıksal uygulamayı sildiğinizde yeni çalıştırma başlatılmaz. Devam eden ve bekleme durumunda olan tüm çalıştırmalar iptal edilir. Binlerce çalıştırma varsa iptal işleminin tamamlanması zaman alabilir. 

## <a name="troubleshooting"></a>Sorun giderme

Logic Apps Tasarımcısı'nda mantıksal uygulama projenizin açtığınızda, Azure aboneliğinizi seçmek için seçenek alamayabilirsiniz. Bunun yerine, mantıksal uygulamanızı kullanmak istediğiniz bir tane değil bir Azure aboneliği ile açılır. Bir mantıksal uygulama .json dosyasını açın, sonra Visual Studio'yu ilk seçili abonelik gelecekte kullanım için ön belleğe aldığından, bu davranış gerçekleşir. Bu sorunu çözmek için aşağıdaki adımlardan birini deneyin:

* Mantıksal uygulama .json dosyasını yeniden adlandırın. Abonelik önbellek dosyası adına bağlıdır.

* Daha önce seçilen abonelikleri için kaldırmak için *tüm* logic apps, çözümünüz içinde gizli Visual Studio ayarları klasöründe (.vs) çözümünüzün dizini silin. Bu konum abonelik bilgilerinizi depolar.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Visual Studio ile dağıtılan mantıksal uygulamaları yönetme öğrendiniz. Ardından, mantıksal uygulama tanımları dağıtımı için özelleştirme hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [JSON biçiminde mantıksal uygulama tanımları yazma](../logic-apps/logic-apps-author-definitions.md)
