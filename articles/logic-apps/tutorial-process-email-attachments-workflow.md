---
title: "E-postaları ve ekleri - Azure mantıksal uygulamaları işlemek için iş akışları oluşturmak | Microsoft Docs"
description: "Bu öğretici e-postaları ve ekleri Azure mantıksal uygulamaları, Azure Storage ve Azure işlevleri işlemek için otomatik iş akışları oluşturmak nasıl gösterir"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/12/2018
ms.author: LADocs; estfan
ms.openlocfilehash: 210731ce2e792452650b7a92cfc542c78a0e8014
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="process-emails-and-attachments-with-a-logic-app"></a>İşlem e-posta ve ekleri bir mantıksal uygulama ile

Azure mantıksal uygulamaları, iş akışlarını otomatikleştirmek ve Azure Hizmetleri, Microsoft Hizmetleri, diğer hizmet olarak yazılım (SaaS) uygulamaları arasında veri tümleştirmenize yardımcı olur ve şirket içi sistemler. Bu öğretici nasıl oluşturabileceğinizi gösteren bir [mantıksal uygulama](../logic-apps/logic-apps-overview.md) gelen e-posta ve eklerin işler. Bu mantığı uygulama işlemleri o içeriği içeriği Azure depolama alanına kaydeder ve bu içeriği gözden geçirme için bildirimler gönderir. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Ayarlanan [Azure depolama](../storage/common/storage-introduction.md) ve denetimi için Depolama Gezgini, e-postaları ve ekleri kaydedildi.
> * Oluşturma bir [Azure işlevi](../azure-functions/functions-overview.md) , HTML gelen e-postaları kaldırır. Bu öğretici için bu işlevi kullanabilirsiniz kodu içerir.
> * Boş bir mantıksal uygulama oluşturma.
> * E-posta ekleri için izleyen tetikleyici ekleyin.
> * E-posta ekleri yüklü olup olmadığını denetleyen bir koşul ekleyin.
> * Bir e-posta ekleri olduğunda Azure işlevi çağıran bir eylem ekleyin.
> * E-postaları ve ekleri için depolama BLOB'lar oluşturan eylem ekleyin.
> * Gönderdiği e-posta bildirimleri, bir eylem ekleyin.

İşiniz bittiğinde, mantıksal uygulamanızı bu iş akışı yüksek bir düzeyde şuna benzer:

![Üst düzey tamamlanmış mantıksal uygulama](./media/tutorial-process-email-attachments-workflow/overview.png)

Azure aboneliğiniz yoksa başlamadan önce <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

## <a name="prerequisites"></a>Önkoşullar

* Office 365 Outlook, Outlook.com veya Gmail gibi Logic Apps tarafından desteklenen bir e-posta Sağlayıcısı'ndan bir e-posta hesabı. Diğer sağlayıcılar için [bağlayıcılar listesi burada gözden](https://docs.microsoft.com/connectors/).

  Bu mantıksal uygulama bir Office 365 Outlook hesabı kullanır. 
  Farklı bir e-posta hesabı kullanıyorsanız, genel adımlar aynı kalır, ancak UI biraz farklı görünebilir.

* İndirme ve yükleme <a href="http://storageexplorer.com/" target="_blank">Microsoft Azure Storage Gezgini serbest</a>. Bu araç, depolama kapsayıcısı doğru şekilde kurulduğundan emin denetleyin yardımcı olur.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Oturum <a href="https://portal.azure.com" target="_blank">Azure portal</a> Azure hesabı kimlik bilgilerinizle.

## <a name="set-up-storage-to-save-attachments"></a>Ekleri kaydetmek üzere depolama alanı ayarlama

BLOB'ları olarak gelen e-postaları ve ekleri kaydedebilirsiniz bir [Azure depolama kapsayıcısının](../storage/common/storage-introduction.md). 

1. Bir depolama kapsayıcısı oluşturabilmeniz için önce [depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account) bu ayarlara sahip:

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Ad** | attachmentstorageacct | Depolama hesabınız için ad | 
   | **Dağıtım modeli** | Resource Manager | [Dağıtım modeli](../azure-resource-manager/resource-manager-deployment-model.md) kaynak dağıtımı yönetmek için | 
   | **Hesap türü** | Genel amaçlı | [Depolama hesabı türü](../storage/common/storage-introduction.md#types-of-storage-accounts) | 
   | **Performans** | Standart | Bu ayar desteklenen veri türlerini ve verilerini depolamak medya belirtir. Bkz: [türlerde depolama hesapları](../storage/common/storage-introduction.md#types-of-storage-accounts). | 
   | **Çoğaltma** | Yerel olarak yedekli depolama (LRS) | Bu ayar nasıl verilerinizi kopyalanır, depolanan, yönetilen eşitlenir ve belirtir. Bkz: [çoğaltma](../storage/common/storage-introduction.md#replication). | 
   | **Gerekli güvenli aktarımı** | Devre dışı | Bu ayar bağlantıları gelen istekleri için gereken güvenlik belirtir. Bkz: [güvenli aktarımı gerektiren](../storage/common/storage-require-secure-transfer.md). | 
   | **Abonelik** | <*Bilgisayarınızı-Azure-abonelik-adı*> | Azure aboneliğiniz için ad | 
   | **Kaynak grubu** | LA öğretici RG | İçin ad [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) düzenlemek ve ilgili kaynakları yönetmek için kullanılır. <p>**Not:** bir kaynak grubu belirli bir bölgede bulunmaktadır. Öğeleri Bu öğretici tüm bölgelerde kullanılamayabilir rağmen mümkün olduğunda aynı bölgede kullanmayı deneyin. | 
   | **Konum** | Doğu ABD 2 | Bölge, depolama hesabınız hakkında bilgiler depolanacağı konumu | 
   | **Sanal ağları yapılandırma** | Devre dışı | Bu öğretici için tutmak **devre dışı** ayarı. | 
   |||| 

   Aynı zamanda [Azure PowerShell](../storage/common/storage-quickstart-create-storage-account-powershell.md) veya [Azure CLI](../storage/common/storage-quickstart-create-storage-account-cli.md).
  
2. Azure depolama hesabınızın dağıtıldıktan sonra depolama hesabının erişim anahtarı alın:

   1. Depolama alanınızda menüsünde altında hesap **ayarları**, seçin **erişim anahtarları**. 
   2. Bul **key1** altında **varsayılan anahtarları** ve depolama hesabı adı.

      ![Kopyalayın ve depolama hesabı adı ve anahtar kaydedin](./media/tutorial-process-email-attachments-workflow/copy-save-storage-name-key.png)

   Aynı zamanda [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccountkey) veya [Azure CLI](https://docs.microsoft.com/cli/azure/storage/account/keys?view=azure-cli-latest.md#az_storage_account_keys_list). 

3. E-posta ekleri için bir depolama kapsayıcısı oluşturun.
   
   1. Depolama hesabı menünüzde üzerinde **genel bakış** bölmesinde seçin **BLOB'lar** altında **Hizmetleri**, ardından **+ kapsayıcı**.

   2. "Ekleri", kapsayıcı adı girin. Altında **genel erişim düzeyi**seçin **kapsayıcı (kapsayıcılar ve bloblar için anonim okuma erişimini)**ve seçin **Tamam**.

   Aynı zamanda [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontainer), veya [Azure CLI](https://docs.microsoft.com/cli/azure/storage/container?view=azure-cli-latest#az_storage_container_create). 
   İşiniz bittiğinde, hesabınızdaki depolama burada Azure portalında, depolama kapsayıcısı bulabilirsiniz:

   ![Tamamlanmış depolama kapsayıcısı](./media/tutorial-process-email-attachments-workflow/created-storage-container.png)

Ardından, Depolama Gezgini depolama hesabınıza bağlanın.

## <a name="set-up-storage-explorer"></a>Depolama Gezgini ayarlayın

Şimdi, böylece mantıksal uygulamanızı doğru ekleri bloblar, depolama kapsayıcısı olarak kaydeder olduğunu onaylayabilirsiniz Depolama Gezgini depolama hesabınıza bağlanın.

1. Microsoft Azure Depolama Gezgini'ni açın. Depolama Gezgini, Azure depolama bağlantı istediğinde seçin **bir depolama hesabı adı ve anahtar kullanmak** > **sonraki**.
Hiçbir komut istemi görünüyorsa, seçin **hesabı eklemek** explorer araç çubuğunda.

2. Altında **adını ve anahtarını kullanarak ekleme**, depolama hesabı adı ve daha önce kaydettiğiniz erişim anahtarını girin. Seçin **sonraki** > **bağlanmak**.

3. Depolama hesabı ve kapsayıcı doğru depolama Gezgini'nde görünen denetleyin:

   1. Altında **Explorer**, genişletin **(yerel ve iliştirildiği)** > 
    **depolama hesapları** > **attachmentstorageaccount** > 
    **Blob kapsayıcıları**.

   2. "Ekleri" kapsayıcı şimdi göründüğünü doğrulayın. 
   Örneğin:

      ![Depolama Gezgini - depolama kapsayıcısı onaylayın](./media/tutorial-process-email-attachments-workflow/storage-explorer-check-contianer.png)

Ardından, oluşturun bir [Azure işlevi](../azure-functions/functions-overview.md) , HTML gelen e-posta adresinden kaldırır.

## <a name="create-a-function-to-clean-html"></a>HTML temizlemek için bir işlev oluşturun

Şimdi, her gelen e-posta adresinden HTML kaldıran bir Azure işlevi oluşturma adımları tarafından sağlanan kod parçacığını kullanın. Böylece, e-posta içeriği temizleyici ve işlem daha kolay olur. Ardından bu işlev, mantığı uygulamanızdan çağırabilirsiniz.

1. Bir işlev oluşturabilmeniz için önce [bir işlev uygulaması oluşturma](../azure-functions/functions-create-function-app-portal.md) bu ayarlara sahip:

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Uygulama adı** | CleanTextFunctionApp | İşlev uygulamanız için genel benzersiz ve açıklayıcı bir ad | 
   | **Abonelik** | <*Bilgisayarınızı-Azure-abonelik-adı*> | Daha önce kullandığınız aynı Azure aboneliği | 
   | **Kaynak Grubu** | LA öğretici RG | Daha önce kullandığınız aynı Azure kaynak grubu | 
   | **Barındırma planı** | Tüketim Planı | Bu ayar nasıl ayrılır ve işlev uygulamanızı çalıştırmak için gücünün gibi ölçek kaynakları belirler. Bkz: [barındırma planları karşılaştırma](../azure-functions/functions-scale.md). | 
   | **Konum** | Doğu ABD 2 | Daha önce kullandığınız aynı bölge | 
   | **Depolama** | cleantextfunctionstorageacct | İşlev uygulamanız için bir depolama hesabı oluşturun. Yalnızca küçük harf ve sayı kullanın. <p>**Not:** bu depolama hesabını işlevi uygulamalarınızı içerir ve daha önce oluşturulmuş depolama hesabınıza e-posta ekleri için farklıdır. | 
   | **Application Insights** | Kapalı | Uygulama ile izleme kapatır [Application Insights](../application-insights/app-insights-overview.md), ancak bu öğreticide, tutmak **kapalı** ayarı. | 
   |||| 

   İşlev uygulamanızı dağıtım sonrasında otomatik olarak açık değilse, uygulamanızda Bul <a href="https://portal.azure.com" target="_blank">Azure portal</a>. Ana Azure menüsünde, **uygulama hizmetleri**ve işlevi uygulamanızı seçin.

   ![Oluşturulan işlev uygulaması](./media/tutorial-process-email-attachments-workflow/function-app-created.png)

   Varsa **uygulama hizmetleri** değil, Azure menüsünde görünen Git **daha fazla hizmet** yerine. Arama kutusuna, bulmak ve seçmek **işlev uygulamalarının**. Daha fazla bilgi için bkz: [işlevinizi oluşturma](../azure-functions/functions-create-first-azure-function.md).

   Aynı zamanda [Azure CLI](../azure-functions/functions-create-first-azure-function-azure-cli.md), veya [PowerShell ve Resource Manager şablonları](../azure-resource-manager/resource-group-template-deploy.md).

2. Altında **işlev uygulamalarının**, genişletin **CleanTextFunctionApp**seçip **işlevler**. İşlevler araç çubuğunda seçin **+ yeni işlev**.

   ![Yeni işlev oluşturma](./media/tutorial-process-email-attachments-workflow/function-app-new-function.png)

3. Altında **aşağıdaki ya da quickstart gidin bir şablon seçin**seçin **HttpTrigger - C#** işlevi şablonu.

   ![İşlev şablonu seçin](./media/tutorial-process-email-attachments-workflow/function-select-httptrigger-csharp-function-template.png)

4. Altında **işlevinizi ad**, girin ```RemoveHTMLFunction```. Altında **HTTP tetikleyicisini** > **yetki düzeyini**, varsayılan tutmak **işlevi** değer ve seçin **oluşturma**.

   ![İşlevinizi adlandırın](./media/tutorial-process-email-attachments-workflow/function-provide-name.png)

5. Düzenleyici açıldıktan sonra şablonu kodu HTML kaldırır ve sonuçları çağırana döndürür bu kodla değiştirin:

   ``` CSharp
   using System.Net;
   using System.Text.RegularExpressions;

   public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
   {
      log.Info($"HttpWebhook triggered");

      // Parse query parameter
      string emailBodyContent = await req.Content.ReadAsStringAsync();

      // Replace HTML with other characters
      string updatedBody = Regex.Replace(emailBodyContent, "<.*?>", string.Empty);
      updatedBody = updatedBody.Replace("\\r\\n", " ");
      updatedBody = updatedBody.Replace(@"&nbsp;", " ");

      // Return cleaned text
      return req.CreateResponse(HttpStatusCode.OK, new { updatedBody });

   }
   ```

6. İşiniz bittiğinde seçin **kaydetmek**. İşlevinizi test edilmesini seçerseniz **Test** oku altında (**<**) Düzenleyicisinin sağ kenarı simgesine tıklayın. 

   !["Test" bölmesini açın](./media/tutorial-process-email-attachments-workflow/function-choose-test.png)

7. İçinde **Test** bölmesi altında **iste gövde**, bu satırı girin ve seçin **çalıştırmak**.

   ```json
   {"name": "<p><p>Testing my function</br></p></p>"}
   ```

   ![İşlevinizi sınama](./media/tutorial-process-email-attachments-workflow/function-run-test.png)

   **Çıkış** penceresi işlevi bu sonucundan gösterir:

   ```json
   {"updatedBody":"{\"name\": \"Testing my function\"}"}
   ```

İşlevinizi çalıştığını denetledikten sonra mantıksal uygulamanızı oluşturun. Bu öğretici bir işlev oluşturma gösterir, ancak, HTML gelen e-postaları kaldırır, Logic Apps de sahip bir **HTML metin** bağlayıcı.

## <a name="create-your-logic-app"></a>Mantıksal uygulama oluşturma

1. Ana Azure menüsünde, **yeni** > **Kurumsal tümleştirme** > **mantıksal uygulama**.

   ![Mantıksal uygulama oluşturma](./media/tutorial-process-email-attachments-workflow/create-logic-app.png)

2. Altında **oluşturma mantıksal uygulama**, bu mantıksal uygulamanızı gösterilen ve tanımlandığı hakkında bilgi sağlar. İşiniz bittiğinde **Panoya sabitle** > **Oluştur**’u seçin.

   ![Mantıksal uygulama bilgileri sağlayın](./media/tutorial-process-email-attachments-workflow/create-logic-app-settings.png)

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Ad** | LA ProcessAttachment | Mantıksal uygulamanız için ad | 
   | **Abonelik** | <*Bilgisayarınızı-Azure-abonelik-adı*> | Daha önce kullandığınız aynı Azure aboneliği | 
   | **Kaynak grubu** | LA öğretici RG | Daha önce kullandığınız aynı Azure kaynak grubu |
   | **Konum** | Doğu ABD 2 | Daha önce kullandığınız aynı bölge | 
   | **Log Analytics** | Kapalı | Bu öğretici için tutmak **kapalı** ayarı. | 
   |||| 

3. Uygulamanızı Azure dağıtıldıktan sonra Logic Apps Tasarımcısı'nı açar ve video giriş ve ortak mantığı uygulama desenler için şablonlar içeren bir sayfa gösterir. **Şablonlar** bölümünde **Boş Mantıksal Uygulama**'yı seçin.

   ![Boş mantıksal uygulama şablonunu seçin](./media/tutorial-process-email-attachments-workflow/choose-logic-app-template.png)

Ardından, eklemek bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts) eklere sahip gelen e-postalar için dinler. Her mantıksal uygulama ateşlenir belirli bir olay gerçekleştiğinde olur veya ne zaman yeni verileri belirli bir koşulunu tetikleyicisi ile başlamalıdır. Daha fazla bilgi için bkz: [ilk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="monitor-incoming-email"></a>Gelen e-posta izleme

1. "E-posta geldiğinde" designer'ı arama kutusuna girin. Bu tetikleyici için e-posta sağlayıcısı seçin:  **< *e-posta sağlayıcısı bilgisayarınızı*> - Yeni bir e-posta geldiğinde**, örneğin:

   ![E-posta sağlayıcısı için bu Tetikleyici seçin: "Yeni bir e-posta geldiğinde"](./media/tutorial-process-email-attachments-workflow/add-trigger-when-email-arrives.png)

   * Azure iş veya okul hesapları için Office 365 Outlook girişini seçin. 
   * Kişisel Microsoft hesapları için Outlook.com girişini seçin. 

2. Kimlik bilgilerini istendiyse, Logic Apps e-posta hesabınızı bağlanabilmesi e-posta hesabınızda oturum açın.

3. Şimdi yeni e-posta filtrelemek için tetikleyici kullanır ölçütünü sağlayın.

   1. Klasör, aralık ve e-postaları denetleme sıklığı belirtin.

      ![Klasör, aralık ve posta denetleme sıklığı belirtin.](./media/tutorial-process-email-attachments-workflow/set-up-email-trigger.png)

      | Ayar | Değer | Açıklama | 
      | ------- | ----- | ----------- | 
      | **Klasör** | Gelen kutusu | E-posta klasörünüze denetlemek için | 
      | **Aralığı** | 1 | Denetimler bekleme aralıkların sayısı | 
      | **Sıklık** | Dakika | Arasındaki her aralık için zaman birimi denetler | 
      |  |  |  | 
  
   2. Seçin **Gelişmiş Seçenekleri Göster** ve bu ayarları belirtin:

      | Ayar | Değer | Açıklama | 
      | ------- | ----- | ----------- | 
      | **Ekli** | Evet | Yalnızca e-postaları ile eklerini alın. <p>**Not:** tetikleyici bir e-postalar yalnızca yeni iletiler denetleniyor ve konu filtreyle eşleşen yalnızca e-postaları işleme, hesabınızdan kaldırmaz. | 
      | **Ekleri içerir** | Evet | İş akışınız için giriş olarak ekleri almak yerine yalnızca eklerini denetleyin. | 
      | **Konu filtresi** | ```Business Analyst 2 #423501``` | E-posta konusunu bulmak için metin | 
      |  |  |  | 

4. Şu an için tetikleyici ayrıntıları gizlemek için tetikleyici başlık çubuğu içinde Ek Yardım düğmesini tıklatın.

   ![Ayrıntıları gizlemek için Şekil Daralt](./media/tutorial-process-email-attachments-workflow/collapse-trigger-shape.png)

5. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.

   Canlı ancak değil artık herhangi bir şey diğer e-postalarınıza denetleme mantıksal uygulamanızı olur. 
   Ardından, iş akışı devam etmek için ölçüt belirten bir koşul ekleyin.

## <a name="check-for-attachments"></a>Ekleri denetle

1. Tetikleyici altında seçin **+ yeni adım** > **bir koşul eklemek**.

   Koşul şekli görüntülendiğinde varsayılan olarak, parametre listesi veya dinamik içerik listesi görüntülenir ve iş akışı girdi olarak dahil edebilirsiniz önceki adımlardan herhangi bir parametre gösterir. 
   Hangi listesi görüntülenir, tarayıcı genişliğini belirler.

2. Koşulu daha iyi bir açıklama ile yeniden adlandırın.

   1. Koşulunun başlık çubuğunda seçin **üç nokta** (**...** ) düğmesini > **yeniden adlandırma**.

      Örneğin, tarayıcınız dar görünümünde ise:

      ![Koşulu yeniden adlandır](./media/tutorial-process-email-attachments-workflow/condition-rename.png)

      Tarayıcınızı uluslararası görünümünde ise ve dinamik içerik listesi erişimi için üç nokta düğmesini seçerek listeyi kapatın **dinamik içerik eklemek** koşul içinde. 
      
      ![Kapat dinamik içerik listesi](./media/tutorial-process-email-attachments-workflow/close-dynamic-content-list.png)

   2. Bu açıklaması, durumu yeniden adlandırın:```If email has attachments and key subject phrase```

3. Koşul, bir ifade sağlayarak açıklanmaktadır. 

   1. Koşul şeklin içine seçin **Gelişmiş modda Düzenle**.

      ![Gelişmiş modda koşulu Düzenle](./media/tutorial-process-email-attachments-workflow/edit-advanced-mode.png)

   2. Metin kutusuna bu deyim girin:

      ```@equals(triggerBody()?['HasAttachment'], bool('true'))```

      Bu ifade karşılaştırır **HasAttachment** özellik değeri Boolean nesnesi ile Bu öğretici, e-posta olarak tetikleyici gövdesinden ```True```. 
      Her iki değer eşitse, e-posta en az bir ek, koşul geçişleri sahiptir ve iş akışı devam eder.

      Koşulunuz şimdi aşağıdaki gibi görünür:

      ![Koşul ifadesi](./media/tutorial-process-email-attachments-workflow/condition-expression.png)

   3. Seçin **temel modunda Düzenle**. İfadeniz şimdi aşağıda gösterildiği gibi çözer:

      ![Çözümlenen ifade](./media/tutorial-process-email-attachments-workflow/condition-expression-resolved.png)

      > [!NOTE]
      > Bir ifade elle oluşturmak için temel modda çalışmaz ve ifade Oluşturucu ile çalışabilmeniz için açık dinamik listesine sahip. Altında **ifade**, işlevleri seçebilirsiniz. Altında **dinamik içerik**, bu işlevleri kullanmak için parametre alanlar seçebilirsiniz.
      > Bu öğretici, daha sonra el ile ifadeler nasıl oluşturulacağını gösterir.

4. Mantıksal uygulamanızı kaydedin.

### <a name="test-your-condition"></a>Koşulunuz test

Şimdi, koşul düzgün çalıştığını olup olmadığını test edin:

1. Mantıksal uygulamanızı zaten çalışıyor durumda değilse, seçin **çalıştırmak** tasarımcı araç.

   Bu adım, belirtilen zaman aralığı geçirir kadar beklemek zorunda kalmadan mantıksal uygulamanızı el ile başlar. 
   Ancak, test e-posta kutunuzda gelene kadar hiçbir şey olmaz. 

2. Kendiniz Bu ölçütleri karşılayan bir e-posta gönder:

   * Tetikleyici içinde belirtilen metin, e-postanın konu sahip **konu filtre**:```Business Analyst 2 #423501```

   * E-posta adresiniz bir eki içerir. 
   Şu an bir boş metin dosyası oluşturun ve bu dosyayı e-postanıza ekleyin.

   E-posta geldiğinde, mantıksal uygulamanızı ekler ve belirtilen konu metni denetler.
   Koşul geçerse, tetikleyici başlatılır ve bir mantıksal uygulama örneği oluşturun ve iş akışını başlatmak için Logic Apps altyapısı neden olur. 

3. Tetikleyici ve mantıksal uygulama mantığını uygulama menüsünde başarıyla çalıştırıldı aranmasını **genel bakış**.

   ![Tetikleyici ve çalıştırmalarını geçmişini kontrol edin](./media/tutorial-process-email-attachments-workflow/checkpoint-run-history.png)

   Mantıksal uygulamanızı alamadık veya tetiklemek rağmen başarılı bir tetikleyici çalıştırmak, bkz: [mantıksal uygulamanızı sorun giderme](../logic-apps/logic-apps-diagnosing-failures.md).

Ardından, gerçekleştirilecek eylemleri tanımlayın **true ise** dal. E-posta eklerinin birlikte kaydetmek için herhangi bir HTML e-posta gövdesinden kaldırın ve sonra e-posta ve ekleri için depolama kapsayıcısı içinde BLOB'ları oluşturun.

> [!NOTE]
> Mantıksal uygulamanızı bir şey yapmanız gerekmez **false ise** dal bir e-posta ekleri sahip olmadığında. Bu öğreticiyi tamamladıktan sonra ek alıştırma olarak almak istediğiniz tüm uygun eylemi ekleyebileceğiniz **false ise** dal.

## <a name="call-the-removehtmlfunction"></a>RemoveHTMLFunction çağırın

1. Mantıksal uygulama menüsünde, **mantığı Uygulama Tasarımcısı**. İçinde **true ise** dal, seçin **Eylem Ekle**.

2. "Azure işlevleri için" araması yapın ve bu eylem seçin: **Azure işlevleri – bir Azure işlevi seçin**

   ![Bir eylem seçin "Azure işlevleri - bir Azure işlevi seçin"](./media/tutorial-process-email-attachments-workflow/add-action-azure-function.png)

3. Önceden oluşturulmuş işlevi uygulamanızı seçin: **CleanTextFunctionApp**

   ![Azure işlevi uygulamanızı seçin](./media/tutorial-process-email-attachments-workflow/add-action-select-azure-function-app.png)

4. Şimdi, işlevi seçin: **RemoveHTMLFunction**

   ![Azure işlevinizi seçin](./media/tutorial-process-email-attachments-workflow/add-action-select-azure-function.png)

5. Bu açıklama, işlevi şekli yeniden adlandırın:```Call RemoveHTMLFunction to clean email body``` 

6. İşlev şeklinde işlevinizi işlemek için giriş girin. E-posta gövdesi gösterilen ve açıklanan olarak burada belirtin:

   ![Beklenen işlevi için istek gövdesini belirtin](./media/tutorial-process-email-attachments-workflow/add-email-body-for-function-processing.png)

   1. Altında **iste gövde**, bu metin girin: 
   
      ```{ "emailBody": ``` 

      Bu giriş sonraki adımlarda tamamlanana kadar geçersiz JSON hakkında bir hata görüntülenir.
      Bu işlev daha önce test olduğunda bu işlev için belirtilen giriş JavaScript nesne gösterimi (JSON) kullanılır. 
      Bu nedenle, istek gövdesini aynı biçimde çok kullanmanız gerekir. 

   2. Parametre listesi veya dinamik içerik listesi seçin **gövde** altında **yeni bir e-posta geldiğinde**.
   Sonra **gövde** alan, kapanış kuşak ekleyin:```}```

      ![İşleve geçirme için istek gövdesini belirtin](./media/tutorial-process-email-attachments-workflow/add-email-body-for-function-processing2.png)

      Mantıksal uygulama tanımı'nda bu girişi bu biçiminde görünür:

      ```{ "emailBody": "@triggerBody()?['Body']" }```

7. Mantıksal uygulamanızı kaydedin.

Ardından, e-posta gövdesi kaydetmek için depolama kapsayıcısı içinde blob oluşturan eylem ekleyin.

## <a name="create-blob-for-email-body"></a>E-posta gövdesi için BLOB oluşturun

1. Azure işlevi şekil altında seçin **Eylem Ekle**. 

2. Altında **bir eylem seçin**, "blob" için arama yapın ve bu eylem seçin: **Azure Blob Storage – Oluştur blob**

   ![E-posta gövdesi için BLOB oluşturmak için Eylem Ekle](./media/tutorial-process-email-attachments-workflow/create-blob-action-for-email-body.png)

3. Bir Azure depolama hesabı bağlantı yoksa, bir bağlantı depolama hesabınız bu ayarlarla gösterilen ve açıklanan olarak burada oluşturun. İşiniz bittiğinde seçin **oluşturma**.

   ![Depolama hesabı bağlantı oluşturun.](./media/tutorial-process-email-attachments-workflow/create-storage-account-connection-first.png)

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Bağlantı Adı** | AttachmentStorageConnection | Bağlantı için açıklayıcı bir ad | 
   | **Depolama hesabı** | attachmentstorageacct | Ekleri kaydetmek için daha önce oluşturduğunuz depolama hesabı adı | 
   |||| 

4. Yeniden Adlandır **oluşturma blob** bu açıklama eylemiyle:```Create blob for email body```

5. İçinde **oluşturma blob** eylem, bu bilgileri sağlayın ve blob gösterilen ve açıklanan olarak oluşturmak için şu parametreleri seçin:

   ![E-posta gövdesi için BLOB bilgileri sağlayın](./media/tutorial-process-email-attachments-workflow/create-blob-for-email-body.png)

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Klasör yolu** | /Attachments | Daha önce oluşturduğunuz kapsayıcının adını ve yolunu. Ayrıca, göz atın ve bir kapsayıcı seçin. | 
   | **BLOB adı** | **Gelen** alan | E-posta gönderen adı blob adı olarak geçirin. Parametre listesi ya da dinamik içerik listesi, seçin **gelen** altında **yeni bir e-posta geldiğinde**. | 
   | **BLOB içeriğinin** | **İçerik** alan | HTML ücretsiz e-posta gövdesindeki blob içeriği olarak geçirin. Parametre listesi ya da dinamik içerik listesi, seçin **gövde** altında **RemoveHTMLFunction e-posta gövdesi temizlemek için arama**. |
   |||| 

6. Mantıksal uygulamanızı kaydedin. 

### <a name="check-attachment-handling"></a>Onay ek işleme

Şimdi mantıksal uygulamanızı e-postalar, belirtilen yolu işleme olup olmadığını test edin:

1. Mantıksal uygulamanızı zaten çalışıyor durumda değilse, seçin **çalıştırmak** tasarımcı araç.

2. Kendiniz Bu ölçütleri karşılayan bir e-posta gönder:

   * Tetikleyici içinde belirtilen metin, e-postanın konu sahip **konu filtre**:```Business Analyst 2 #423501```

   * E-postanızı en azından bir eki vardır. 
   Şu an bir boş metin dosyası oluşturun ve bu dosyayı e-postanıza ekleyin.

   * E-postanızı biraz örneğin gövdesinde sahiptir: 

     ```
     Testing my logic app
     ```

   Mantıksal uygulamanızı alamadık veya tetiklemek rağmen başarılı bir tetikleyici çalıştırmak, bkz: [mantıksal uygulamanızı sorun giderme](../logic-apps/logic-apps-diagnosing-failures.md).

3. Mantıksal uygulamanızı doğru depolama kapsayıcısı için e-posta kaydedilmiş denetleyin. 

   1. Depolama Gezgini'nde genişletin **(yerel ve iliştirildiği)** > 
    **depolama hesapları** > **attachmentstorageacct (harici)** > 
    **Blob kapsayıcıları** > **ekleri**.

   2. Denetleme **ekleri** e-posta için kapsayıcı. 

      Bu noktada, mantıksal uygulama henüz ekleri işlemiyor olduğundan yalnızca e-posta kapsayıcısında görüntülenir.

      ![Depolama Gezgini için kaydedilen e-posta denetleyin](./media/tutorial-process-email-attachments-workflow/storage-explorer-saved-email.png)

   3. İşiniz bittiğinde, e-posta Depolama Gezgini ile silin.

4. İsteğe bağlı olarak, test etmek için **false ise** dal, hiçbir şey, şu anda hangi mu ölçütlerle eşleşmeyen bir e-posta gönderebilirsiniz.

Ardından, tüm e-posta eklerini işlemek için bir döngü ekleyin.

## <a name="process-attachments"></a>İşlem ekler

Bu mantıksal uygulama kullanan bir **her** her ek e-posta olarak işlemek için döngü.

1. Altında **e-posta gövdesi için Create blob** şekil, seçin **... Daha fazla**ve bu komutu seçin: **Ekle bir her**

   !["İçin her" döngü ekleme](./media/tutorial-process-email-attachments-workflow/add-for-each-loop.png)

2. Bu açıklama ile döngü yeniden adlandırın:```For each email attachment```

3. Şimdi verileri işlemek döngünün belirtin. İçini tıklatın **bir çıktı önceki adımları seçin** kutusu. Parametre listesi veya dinamik içerik listesi seçin **ekleri**. 

   !["Ekleri" seçin](./media/tutorial-process-email-attachments-workflow/select-attachments.png)

   **Ekleri** alan içeren bir e-posta gelen tüm ekleri içeren bir dizi geçirir. 
   **Her** döngüyü oturum dizi geçirilen her bir öğe eylemleri yineler.

4. Mantıksal uygulamanızı kaydedin.

Ardından, her ek bir blob'a kaydeder eylem eklemek, **ekleri** depolama kapsayıcısı.

## <a name="create-blobs-for-attachments"></a>Ekler için BLOB'ları oluşturma

1. İçinde **her** döngü, seçin **Eylem Ekle** böylece üzerinde bulunan her ek gerçekleştirmek için görev belirtebilirsiniz.

   ![Döngü için eylem ekleme](./media/tutorial-process-email-attachments-workflow/for-each-add-action.png)

2. Altında **bir eylem seçin**, "blob" için arama yapın ve sonra bu eylem seçin: **Azure Blob Storage – Oluştur blob**

   ![BLOB oluşturmak için Eylem Ekle](./media/tutorial-process-email-attachments-workflow/create-blob-action-for-attachments.png)

3. Yeniden Adlandır **oluşturma blob 2** bu açıklama eylemiyle:```Create blob for each email attachment```

4. İçinde **her e-posta eki için Create blob** eylem, bu bilgileri sağlayın ve her bir blob gösterilen ve açıklanan olarak oluşturmak için parametreleri seçin:

   ![BLOB bilgileri sağlayın](./media/tutorial-process-email-attachments-workflow/create-blob-per-attachment.png)

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Klasör yolu** | /Attachments | Kapsayıcının adını ve yolunu önceden oluşturulmuş. Ayrıca, göz atın ve bir kapsayıcı seçin. | 
   | **BLOB adı** | **Ad** alan | Parametre listesi ya da dinamik içerik listesi, seçin **adı** blob adı için ek ad geçirmek için. | 
   | **BLOB içeriğinin** | **İçerik** alan | Parametre listesi ya da dinamik içerik listesi, seçin **içerik** ek içerik için blob içeriğinin geçirmek için. |
   |||| 

5. Mantıksal uygulamanızı kaydedin. 

### <a name="check-attachment-handling"></a>Onay ek işleme

Ardından, mantıksal uygulamanızı ekleri, belirtilen yolu işleme olup olmadığını test edin:

1. Mantıksal uygulamanızı zaten çalışıyor durumda değilse, seçin **çalıştırmak** tasarımcı araç.

2. Kendiniz Bu ölçütleri karşılayan bir e-posta gönder:

   * Tetikleyici içinde belirtilen metin, e-postanın konu sahip **konu filtre**:```Business Analyst 2 #423501```

   * E-postanızı en az iki ek sahiptir. 
   Şimdilik, yalnızca iki boş metin dosyası oluşturun ve bu dosyaları e-postanıza ekleyin.

   Mantıksal uygulamanızı alamadık veya tetiklemek rağmen başarılı bir tetikleyici çalıştırmak, bkz: [mantıksal uygulamanızı sorun giderme](../logic-apps/logic-apps-diagnosing-failures.md).

3. Mantıksal uygulamanızı e-posta ve ekleri doğru depolama kapsayıcısı kaydedilmiş denetleyin. 

   1. Depolama Gezgini'nde genişletin **(yerel ve iliştirildiği)** > 
    **depolama hesapları** > **attachmentstorageacct (harici)** > 
    **Blob kapsayıcıları** > **ekleri**.

   2. Denetleme **ekleri** e-posta ve ekleri için kapsayıcı.

      ![Kaydedilen e-posta ve ekleri için onay](./media/tutorial-process-email-attachments-workflow/storage-explorer-saved-attachments.png)

   3. İşiniz bittiğinde e-posta ve ekleri depolama Gezgini'nde silin.

Ardından, böylece e-posta ekleri gözden geçirmek için mantıksal uygulamanızı gönderir bir eylem ekleyin.

## <a name="send-email-notifications"></a>E-posta bildirimleri gönder

1. İçinde **true ise** altında dal **her e-posta eki için** döngü, seçin **Eylem Ekle**. 

   ![Ekle "için her altında" eylemi döngüsü](./media/tutorial-process-email-attachments-workflow/add-action-send-email.png)

2. Altında **bir eylem seçin**, "e-postası gönderme için" araması yapın ve sonra istediğiniz e-posta sağlayıcısı için "e-posta Gönder" eylemini seçin. Belirli bir hizmet için Eylemler listesini filtrelemek için bağlayıcı ilk altında seçebileceğiniz **Bağlayıcılar**.

   ![E-posta sağlayıcınız için "e-posta Gönder" eylemini seçin](./media/tutorial-process-email-attachments-workflow/add-action-select-send-email.png)

   * Azure iş veya okul hesapları için Office 365 Outlook girişini seçin. 
   * Kişisel Microsoft hesapları için Outlook.com girişini seçin. 

3. Kimlik bilgilerini istendiyse, Logic Apps e-posta hesabınız için bir bağlantı oluşturur, böylece e-posta hesabınızda oturum açın.

4. Yeniden Adlandır **bir e-posta Gönder** bu açıklama eylemiyle:```Send email for review```

5. Bu eylem için bilgileri sağlayın ve e-postalara gösterilen ve açıklandığı gibi dahil etmek istediğiniz alanları seçin. Bir düzenleme kutusuna boş satır eklemek için Shift + Enter tuşlarını kullanın.  

   Örneğin, dinamik içerik listesi ile çalışıyorsanız:

   ![E-posta bildirimi gönder](./media/tutorial-process-email-attachments-workflow/send-email-notification.png)

   Beklenen bir alan listede bulamazsanız, seçin **daha fazla** yanına **yeni bir e-posta geldiğinde** dinamik içerik listesinde veya parametre listesi sonunda.

   | Ayar | Değer | Notlar | 
   | ------- | ----- | ----- | 
   | **Alıcı** | <*recipient-email-address*> | Test için kendi e-posta adresinizi kullanabilirsiniz. | 
   | **Konu**  | ```ASAP - Review applicant for position: ``` **Subject** | Dahil etmek istediğiniz e-posta konusu. Parametre listesi ya da dinamik içerik listesi, seçin **konu** altında **yeni bir e-posta geldiğinde**. | 
   | **Gövde** | ```Please review new applicant:``` <p>```Applicant name: ```**Gelen** <p>```Application file location: ```**Yolu** <p>```Application email content: ```**Gövdesi** | E-posta gövdesi için içerik. Parametre listesi ya da dinamik içerik listesi, bu alanları seçin: <p>- **Gelen** altında **yeni bir e-posta geldiğinde** </br>- **Yolu** altında **e-posta gövdesi için Create blob** </br>- **Gövde** altında **RemoveHTMLFunction e-posta gövdesi temizlemek için arama** | 
   |||| 

   Gibi bir dizi içeren bir alanı seçmek için görülüyorsa **içerik**ekleri içeren bir dizi olduğu, Tasarımcısı "için her" çevresine bir daire bu alana başvuran eylemi otomatik olarak ekler. 
   Bu şekilde mantıksal uygulamanız ilgili eylemi dizideki tüm öğeler için gerçekleştirebilir. 
   Döngü kaldırmak için Kaldır alan dizi için taşıma başvuru eylemi döngü dışında elipsleri seçin (**...** ) üzerinde döngünün başlık çubuğunu ve seçin **silmek**.
     
6. Mantıksal uygulamanızı kaydedin. 

Ardından, artık bu gibi görünen mantıksal uygulamanızı test edin:

![Tamamlanmış mantıksal uygulama](./media/tutorial-process-email-attachments-workflow/complete.png)

## <a name="run-your-logic-app"></a>Mantıksal uygulamanızı çalıştırma

1. Kendiniz Bu ölçütleri karşılayan bir e-posta gönder:

   * Tetikleyici içinde belirtilen metin, e-postanın konu sahip **konu filtre**:```Business Analyst 2 #423501```

   * E-posta bir veya daha fazla ekleri sahiptir. 
   Önceki testten boş bir metin dosyası yeniden kullanabilirsiniz. 
   Daha gerçekçi bir senaryo için Sürdür dosyası ekleyin.

   * E-posta gövdesi kopyalayın ve yapıştırın bu metin vardır:

     ```
     Name: Jamal Hartnett   
     
     Street address: 12345 Anywhere Road   
     
     City: Any Town   
     
     State or Country: Any State   
     
     Postal code: 00000   
     
     Email address: jamhartnett@outlook.com   
     
     Phone number: 000-000-0000   
     
     Position: Business Analyst 2 #423501   

     Technical skills: Dynamics CRM, MySQL, Microsoft SQL Server, JavaScript, Perl, Power BI, Tableau, Microsoft Office: Excel, Visio, Word, PowerPoint, SharePoint, and Outlook   

     Professional skills: Data, process, workflow, statistics, risk analysis, modeling; technical writing, expert communicator and presenter, logical and analytical thinker, team builder, mediator, negotiator, self-starter, self-managing  
     
     Certifications: Six Sigma Green Belt, Lean Project Management   
     
     Language skills: English, Mandarin, Spanish   
     
     Education: Master of Business Administration   
     ```

2. Mantıksal uygulamanızı çalıştırın. Mantıksal uygulamanızı başarılı olursa, bu gibi görünen bir e-posta gönderir:

   ![Mantıksal uygulama tarafından gönderilen e-posta bildirimi](./media/tutorial-process-email-attachments-workflow/email-notification.png)

   Tüm e-postaları alamazsanız, e-postanın Önemsiz klasörünü denetleyin. 
   E-posta Önemsiz filtre postalar bu tür yönlendirebilir. 
   Mantıksal uygulamanızın düzgün bir şekilde çalışıp çalışmadığından emin değilseniz bkz. [Mantıksal uygulama sorunlarını giderme](../logic-apps/logic-apps-diagnosing-failures.md).

Tebrikler, artık oluşturduğunuz ve farklı Azure Hizmetleri genelinde görevlerini otomatikleştirir ve bazı özel kod çağrıları bir mantıksal uygulama çalıştırın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, mantıksal uygulama ve ilgili kaynakları içeren kaynak grubunu silin. Azure ana menüde Git **kaynak grupları**ve mantıksal uygulamanız için kaynak grubunu seçin. Seçin **kaynak grubu Sil**. Onay kaynak grubu adı girin ve seçin **silmek**.

![Mantıksal uygulama kaynak grubunu silme](./media/tutorial-process-email-attachments-workflow/delete-resource-group.png)

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](http://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, işler ve Azure Storage ve Azure işlevleri gibi Azure Hizmetleri ile tümleştirerek e-posta eklerini depolayan bir mantıksal uygulama oluşturuldu. Şimdi, logic apps oluşturmak için kullanabileceğiniz diğer bağlayıcıları hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Logic Apps için bağlayıcıları hakkında daha fazla bilgi edinin](../connectors/apis-list.md)