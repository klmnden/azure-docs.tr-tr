---
title: Sanal makine değişikliklerini izleme - Azure Event Grid ve Logic Apps | Microsoft Docs
description: Azure Event Grid ve Logic Apps kullanarak sanal makinelerde (VM) yapılandırma değişikliklerini denetleme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: tutorial
ms.date: 05/14/2019
ms.openlocfilehash: 791e38f3d15801166f07234648909e03d800f5c0
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65604499"
---
# <a name="tutorial-monitor-virtual-machine-changes-with-azure-event-grid-and-logic-apps"></a>Öğretici: Azure Event Grid ve Logic Apps ile sanal makine değişikliklerini izleme

Azure kaynaklarında veya üçüncü taraf kaynaklarda belirli olaylar olduğunda otomatik bir [mantıksal uygulama iş akışı](../logic-apps/logic-apps-overview.md) başlatabilirsiniz. Bu kaynaklar bu olayları bir [Azure olay kılavuzuna](../event-grid/overview.md) yayımlayabilir. Olay kılavuzu da bu olayları uç nokta olarak kuyruk, web kancası veya [olay hub’ları](../event-hubs/event-hubs-what-is-event-hubs.md) olan abonelere gönderir. Bir abone olarak, mantıksal uygulamanız sizin herhangi bir kod yazmanıza gerek kalmadan görevleri gerçekleştirmek üzere otomatik iş akışlarını çalıştırmak için olay kılavuzundan bu olayları bekleyebilir.

Örneğin, yayımcıların Azure Event Grid hizmeti üzerinden abonelere gönderebileceği bazı olaylar şunlardır:

* Kaynağı oluşturun, okuyun, güncelleştirin veya silin. Örneğin, Azure aboneliğinizde ücretlendirmeye neden olabilecek ve faturanızı etkileyebilecek değişiklikleri izleyebilirsiniz. 
* Bir Azure aboneliğine kişi ekleyin veya kaldırın.
* Uygulamanız belirli bir eylemi gerçekleştirir.
* Bir kuyrukta yeni bir ileti görüntülenir.

Bu öğreticide, bir sanal makine yapılan değişiklikleri izler ve bu değişiklikler hakkında e-posta gönderen bir mantıksal uygulama oluşturur. Bir Azure kaynağı için bir mantıksal uygulama oluşturduğunuzda, bu kaynaktan olayların bir mantık kılavuzu üzerinden mantıksal uygulamaya akışı yapılır. Öğretici size bu mantıksal uygulamayı oluşturma sürecinde yardımcı olur:

![Genel bakış - Olay kılavuzu ve mantıksal uygulama ile sanal makineyi izleme](./media/monitor-virtual-machine-changes-event-grid-logic-app/monitor-virtual-machine-event-grid-logic-app-overview.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir olay kılavuzundan olayları izleyen bir mantıksal uygulama oluşturun.
> * Özellikle sanal makine değişikliklerini izleyen bir koşul ekleyin.
> * Sanal makineniz değiştiğinde e-posta gönderin.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Office 365 Outlook, Outlook.com veya Gmail gibi bir bildirim göndermek için Logic Apps tarafından desteklenen bir e-posta sağlayıcısından bir e-posta hesabı. Diğer sağlayıcılar için [buradaki bağlayıcı listesini inceleyin](/connectors/). 

  Bu öğreticide, bir Office 365 Outlook hesabı kullanır. Farklı bir e-posta hesabı kullanırsanız genel adımlar aynı kalır, ancak kullanıcı arabiriminiz biraz farklı görünebilir.

* Bir [sanal makine](https://azure.microsoft.com/services/virtual-machines). Zaten yapmadıysanız, bir sanal makine aracılığıyla oluşturmak [VM öğretici oluşturma](../virtual-machines/windows/quick-create-portal.md). Sanal makinenin olayları yayımlaması için, [başka bir işlem yapmanız gerekmez](../event-grid/overview.md).

## <a name="create-blank-logic-app"></a>Boş mantıksal uygulama oluşturma

1. Azure hesabınızın kimlik bilgileriyle [Azure portalında](https://portal.azure.com) oturum açın. 

1. Azure ana menüsünden seçin **kaynak Oluştur** > **tümleştirme** > **mantıksal uygulama**.

   ![Mantıksal uygulama oluşturma](./media/monitor-virtual-machine-changes-event-grid-logic-app/azure-portal-create-logic-app.png)

1. Altında **mantıksal uygulama**, mantıksal uygulamanızla ilgili bilgileri sağlayın. İşiniz bittiğinde **Oluştur**’u seçin.

   ![Mantıksal uygulama ayrıntılarını sağlayın](./media/monitor-virtual-machine-changes-event-grid-logic-app/create-logic-app-for-event-grid.png)

   | Özellik | Önerilen değer | Açıklama |
   | -------- | --------------- | ----------- |
   | **Ad** | <*mantıksal uygulama adı*> | Mantıksal uygulamanız için benzersiz bir ad sağlayın. |
   | **Abonelik** | <*Azure-subscription-name*> | Bu öğreticideki tüm hizmetler için aynı Azure aboneliğini seçin. |
   | **Kaynak grubu** | <*Azure kaynak grubu*> | Bu öğreticideki tüm hizmetler için aynı Azure kaynak grubunu seçin. |
   | **Konum** | <*Azure veri merkezi bölgesi*> | Bu öğreticideki tüm hizmetler için aynı bölgeyi seçin. |
   |||

   Mantıksal uygulamanız için bir Azure kaynağı oluşturdunuz. 

1. Azure mantıksal uygulamanızı dağıttıktan sonra Logic Apps Tasarımcısı'nda giriş içeren bir sayfa video gösterir ve sık kullanılan tetikleyicilerin. Video ve tetikleyicileri kaydırın. 

1. **Şablonlar** bölümünde **Boş Mantıksal Uygulama**'yı seçin.

   ![Mantıksal uygulama şablonunu seçin](./media/monitor-virtual-machine-changes-event-grid-logic-app/choose-logic-app-template.png)

   Logic Apps Tasarımcısı'nda, gösterdiğini [ *Tetikleyicileri* ](../logic-apps/logic-apps-overview.md#logic-app-concepts) mantıksal uygulamanızı başlatmak için kullanabilirsiniz. Her mantıksal uygulama, belirli bir olay gerçekleştiğinde ya da belirli bir koşul karşılandığında tetiklenen bir tetikleyiciyle başlamalıdır. 
   Mantıksal uygulamanızın Tetikleyici etkinleştirildiğinde, Azure Logic Apps, iş akışı örneği oluşturur her zaman çalışır.

## <a name="add-event-grid-trigger"></a>Olay Kılavuzu tetikleyicisi Ekle 

Şimdi sanal makineniz için kaynak grubu izleyen Event Grid tetikleyicisinin ekleyin. 

1. Tasarımcıda arama kutusuna filtreniz olarak "event grid" yazın. Tetikleyiciler listesinden şu tetikleyiciyi seçin: **Bir kaynak olayı gerçekleştiğinde - Azure Event Grid**

   ![Şu tetikleyiciyi seçin: "Bir kaynak olayı"](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger.png)

1. İstendiğinde, Azure Event Grid için Azure hesabı kimlik bilgilerinizle oturum açın. İçinde **Kiracı** doğru Kiracı görünür olup olmadığını kontrol edin, Azure aboneliğinizin ilişkili Azure Active Directory kiracısı gösteren bir liste.

   ![Azure kimlik bilgilerinizle oturum açın](./media/monitor-virtual-machine-changes-event-grid-logic-app/sign-in-event-grid.png)

   > [!NOTE]
   > @outlook.com veya @hotmail.com gibi kişisel bir Microsoft hesabında oturum açtıysanız, Event Grid tetikleyicisi doğru görüntülenmeyebilir. Geçici bir çözüm olarak, [Hizmet Sorumlusu ile bağlan](../active-directory/develop/howto-create-service-principal-portal.md)’ı seçin veya *user-name*@emailoutlook.onmicrosoft.com gibi Azure aboneliğinizle ilişkili bir Azure Active Directory’nin bir üyesi olarak kimlik doğrulaması yapın.

1. Şimdi mantıksal uygulamanızı yayımcı olaylarına kaydedin. Aşağıdaki tabloda belirtildiği gibi olay aboneliğinizin ayrıntılarını sağlayın:

   ![Olay aboneliğinin ayrıntılarını sağlayın](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details-generic.png)

   | Özellik | Gereklidir | Value | Açıklama |
   | -------- | -------- | ----- | ----------- |
   | **Abonelik** | Evet | <*Olay-publisher-Azure-abonelik-name*> | Olay yayımcısı ile ilişkili Azure aboneliği için bir ad seçin. Bu öğreticide, sanal makineniz için Azure abonelik adını seçin. |
   | **Kaynak Türü** | Evet | <*Olay-publisher-Azure-resource-type*> | Olay yayımcısı için kaynak türünü seçin. Mantıksal uygulamanız yalnızca kaynak gruplarını izler. böylece Bu öğretici için bu değeri seçin: <p><p>**Microsoft.Resources.resourceGroups** |
   | **Kaynak Adı** |  Evet | <*Olay-publisher-Azure-resource-name*> | Olay yayımcısı ile ilişkili Azure kaynak adını seçin. Örneğin, bu kaynak bir Event Grid konusu olabilir. Bu öğreticide, sanal makineniz için ilişkili Azure kaynak grubu adını seçin. |
   | **Olay türü öğesi** |  Hayır | <*olay türleri*> | İzlemek istediğiniz bir veya daha fazla belirli olay türleri seçin. Bu öğreticide, bu özelliği boş bırakın. |
   | **Abonelik adı** | Hayır | <*Olay aboneliği adı*> | Olay aboneliğiniz için benzersiz bir ad girin. |
   | İsteğe bağlı ayarları seçin **yeni parametre Ekle**. | Hayır | {açıklamalarına bakın} | * **Önek filtresi**: Bu öğreticide, bu özelliği boş bırakın. Varsayılan davranış tüm değerlerle eşleşir. Ancak filtre olarak bir ön ek dizesi (örneğin belirli bir kaynak için bir yol ve bir parametre) belirtebilirsiniz. <p>* **Sonek filtresi**: Bu öğreticide, bu özelliği boş bırakın. Varsayılan davranış tüm değerlerle eşleşir. Ancak yalnızca belirli dosya türlerini istediğinizde filtre olarak bir sonek dizesi (örneğin dosya adı uzantısı) belirtebilirsiniz. |
   |||

   İşiniz bittiğinde, olay Kılavuzu tetikleyicisi şu örnekteki gibi görünebilir:

   ![Örnek olay Kılavuzu tetikleyicisi ayrıntıları](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details.png)

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin. Mantıksal uygulamanızda bir eylemin ayrıntılarını daraltmak ve gizlemek için, eylemin başlık çubuğunu seçin.

   ![Mantıksal uygulamanızı kaydetme](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save.png)

   Mantıksal uygulamanızı bir olay kılavuzu tetikleyicisiyle kaydettiğinizde, Azure seçtiğiniz kaynağa mantıksal uygulamanız için otomatik olarak bir olay aboneliği oluşturur. Bu nedenle kaynak bir olayı olay kılavuzuna yayımladığında, bu olay kılavuzu otomatik olarak olayı mantıksal uygulamanıza gönderir. Bu olay mantıksal uygulamanızı tetikler ve ardından sonraki adımlarda yapılandıracağınız iş akışının bir örneğini oluşturur ve çalıştırır.

Mantıksal uygulamanız artık canlı ve olay kılavuzundan olayları dinliyor ancak siz eylemleri iş akışına ekleyene kadar herhangi bir işlem yapmayacak. 

## <a name="add-condition"></a>Koşul ekle

Mantıksal uygulama iş akışınızı yalnızca belirli bir olay gerçekleştiğinde çalıştırmak için, sanal makine “write” işlemlerini denetleyen bir koşul ekleyin. Bu koşul true olduğunda, mantıksal uygulamanız size güncelleştirilen sanal makine hakkında ayrıntıları içeren bir e-posta gönderir.

1. Logic Apps Tasarımcısı'nda olay Kılavuzu tetikleyicisi altında seçin **yeni adım**.

   !["Yeni adım" seçin](./media/monitor-virtual-machine-changes-event-grid-logic-app/choose-new-step-condition.png)

1. Arama kutusuna filtreniz olarak "koşul" girin. Eylem listesinden şu eylemi seçin: **Koşul**

   ![Koşul ekle](./media/monitor-virtual-machine-changes-event-grid-logic-app/select-condition.png)

   Logic App Tasarımcısı iş akışınıza koşulun true veya false olmasına bağlı olarak izlenecek eylem yolları dahil boş bir koşul ekler.

   ![Boş koşul](./media/monitor-virtual-machine-changes-event-grid-logic-app/empty-condition.png)

1. Koşul başlığı Yeniden Adlandır `If a virtual machine in your resource group has changed`. Koşulun başlık çubuğundaki üç noktayı seçin (**...** ) düğmesini ve **Yeniden Adlandır**.

   ![Koşulu yeniden adlandırın](./media/monitor-virtual-machine-changes-event-grid-logic-app/rename-condition.png)

1. Olay denetleyen bir koşul oluşturun `body` için bir `data` nesne burada `operationName` özelliğini eşittir `Microsoft.Compute/virtualMachines/write` işlemi. [Event Grid olay şeması](../event-grid/event-schema.md) hakkında daha fazla bilgi edinin.

   1. İlk satırda **Ve** başlığının altındaki sol kutunun içine tıklayın. Görüntülenen dinamik içerik listesinde seçin **ifade**.

      !["İfadesi" seçin](./media/monitor-virtual-machine-changes-event-grid-logic-app/condition-choose-expression.png)

   1. İfade düzenleyicisinde şu ifadeyi girin ve seçin **Tamam**: 

      `triggerBody()?['data']['operationName']`

      Örneğin:

      !["İfadesi" seçin](./media/monitor-virtual-machine-changes-event-grid-logic-app/condition-add-data-operation-name.png)

   1. Ortadaki kutuda **eşittir** işlecini tutun.

   1. Bu değeri doğru kutuya girin:

      `Microsoft.Compute/virtualMachines/write`

   Tamamlanmış koşulunuzu artık şu örnekteki gibi görünür:

   ![Tamamlanmış koşul](./media/monitor-virtual-machine-changes-event-grid-logic-app/complete-condition.png)

1. Mantıksal uygulamanızı kaydedin.

## <a name="send-email-notifications"></a>E-posta bildirimleri gönderme

Şimdi belirtilen koşul true olduğunda bir e-posta almak için bir [*eylem*](../logic-apps/logic-apps-overview.md#logic-app-concepts) ekleyin.

1. Koşulun **True ise** kutusunda **Eylem ekle**’yi seçin.

   ![Koşul true olduğunda kullanılacak eylemi ekleyin](./media/monitor-virtual-machine-changes-event-grid-logic-app/condition-true-add-action.png)

1. Arama kutusuna "filtreniz olarak bir e-posta Gönder" girin. E-posta sağlayıcınıza uygun bağlayıcıyı bulun ve seçin. Ardından bağlayıcı için "e-posta gönder" eylemini seçin. Örneğin: 

   * Azure iş veya okul hesabı için Office 365 Outlook bağlayıcısını seçin. 

   * Kişisel Microsoft hesapları için Outlook.com bağlayıcısını seçin. 

   * Gmail hesapları için Gmail bağlayıcısını seçin. 

   Bu öğreticide Office 365 Outlook bağlayıcısıyla devam eder. 
   Farklı bir sağlayıcı kullanıyorsanız adımlar aynı olacaktır ancak kullanıcı Arabirimi biraz farklı olabilir. 

   !["E-posta gönder" eylemini seçin](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email.png)

1. E-posta sağlayıcınız için zaten bir bağlantınız yoksa, kimliğinizi doğrulamanız istendiğinde e-posta hesabınızda oturum açın.

1. Bu konu başlığı gönderme e-posta başlığına yeniden adlandır: `Send email when virtual machine updated`. 

1. Aşağıdaki tabloda belirtildiği gibi e-posta için ayrıntıları sağlayın:

   ![Boş e-posta eylemi](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-empty-email-action.png)

   > [!TIP]
   > Önceki adımlarda, iş akışınızı sonuçlardan seçmek için dinamik içerik listesinde görünmesi bir düzenleme kutusuna tıklayın veya seçin **dinamik içerik Ekle**. Daha fazla sonuç için seçin **daha fazla bilgi bkz** listesinde her bölüm için. Dinamik içerik listesini kapatın, tercih **dinamik içerik Ekle** yeniden.

   | Özellik | Gereklidir | Value | Açıklama |
   | -------- | -------- | ----- | ----------- |
   | **Alıcı** | Evet | <*Alıcı\@etki alanı*> | Alıcının e-posta adresi girin. Test için kendi e-posta adresinizi kullanabilirsiniz. |
   | **Konu** | Evet | Güncelleştirilen kaynağı: **Konu** | E-posta konusunun içeriğini girin. Bu öğretici için belirtilen metin girin ve seçin olayın **konu** alan. Burada, e-postanızın konusu güncelleştirilen kaynağın (sanal makine) adını içerir. |
   | **Gövde** | Evet | Kaynak grubu: **Konu** <p>Olay türü: **Olay türü**<p>Olay Kimliği: **ID**<p>Zaman: **Olay saati** | E-posta gövdesinin içeriğini girin. Bu öğretici için belirtilen metin girin ve seçin olayın **konu**, **olay türü**, **kimliği**, ve **olay saati** alanları için e-posta, kaynak grubu adı, olay türü, olay zaman damgası ve güncelleştirmesi olay Kimliğini içerir. <p>İçeriğinize boş satır eklemek için Shift + Enter tuşlarını kullanın. |
   ||||

   > [!NOTE] 
   > Bir diziyi temsil eden bir alan seçerseniz, tasarımcı eyleme otomatik olarak diziye başvuran bir **For each** döngüsü ekler. Bu şekilde mantıksal uygulamanız ilgili eylemi dizideki tüm öğeler için gerçekleştirir.

   Şimdi, e-posta eyleminiz bu örnekteki gibi görünebilir:

   ![E-postaya eklenecek çıkışları seçme](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email-details.png)

   Tamamlanmış mantıksal uygulamanız örnekteki gibi görünebilir:

   ![Tamamlanmış mantıksal uygulama](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-completed.png)

1. Mantıksal uygulamanızı kaydedin. Mantıksal uygulamanızda her eylemin ayrıntılarını daraltmak ve gizlemek için, eylemin başlık çubuğunu seçin.

   Mantıksal uygulamanız artık canlıdır ancak herhangi bir işlem gerçekleştirmeden önce sanal makinenizde yapılan değişiklikleri bekler. 
   Mantıksal uygulamanızı şimdi test etmek için sonraki bölüme geçin.

## <a name="test-your-logic-app-workflow"></a>Mantıksal uygulama iş akışınızı test etme

1. Mantıksal uygulamanızın belirtilen olayları alıp almadığını denetlemek için, sanal makinenizi güncelleştirin.

   Örneğin, Azure portalında sanal makinenizi yeniden boyutlandırabilir veya [VM’nizi Azure PowerShell ile yeniden boyutlandırabilirsiniz](../virtual-machines/windows/resize-vm.md).

   Birkaç dakika sonra bir e-posta almanız gerekir. Örneğin:

   ![Sanal makine güncelleştirmesi hakkında e-posta](./media/monitor-virtual-machine-changes-event-grid-logic-app/email.png)

1. Çalıştırmalar gözden geçirin ve tetikleyici geçmişini, mantıksal uygulama menüsünde, mantıksal uygulamanızın seçin **genel bakış**. Bir çalıştırma hakkında daha fazla ayrıntı görüntülemek için bu çalıştırma için satır seçin.

   ![Mantıksal uygulama çalıştırma geçmişi](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history.png)

1. Her bir adımın giriş ve çıkışlarını görüntülemek için gözden geçirmek istediğiniz adımı genişletin. Bu bilgiler mantıksal uygulamanızdaki sorunları tespit etmenize ve gidermenize yardımcı olabilir.

   ![Mantıksal uygulama çalıştırma geçmişi ayrıntıları](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history-details.png)

Tebrikler, bir olay kılavuzuyla kaynak olaylarını izleyen ve bu olaylar gerçekleştiğinde size e-posta gönderen bir mantıksal uygulama oluşturdunuz. Ayrıca, süreçleri otomatik hale getiren iş akışlarını ne kadar kolay oluşturabileceğinizi ve sistemler ile bulut hizmetlerini tümleştirmeyi öğrendiniz.

Olay kılavuzları ve mantıksal uygulamalarla diğer yapılandırma değişikliklerini izleyebilirsiniz, örneğin:

* Bir sanal makine rol tabanlı erişim denetimi (RBAC) haklarını alır.
* Değişiklikler bir ağ arabirimi (NIC) üzerindeki bir ağ güvenlik grubunda (NSG) yapılır.
* Bir sanal makine için diskler eklenir veya kaldırılır.
* Bir sanal makine NIC’sine genel bir IP adresi atanır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide Azure aboneliğinize ücret uygulanmasına neden olan kaynaklar kullanılmakta ve eylemler gerçekleştirilmektedir. Bu nedenle öğreticiyi ve testlerinizi tamamladıktan sonra ücret uygulanmasını istemediğiniz kaynakları devre dışı bırakmayı veya silmeyi unutmayın.

* Çalışmanızı silmeden mantıksal uygulamanızı durdurmak için uygulamanızı devre dışı bırakın. Mantıksal uygulama menüsünde seçin **genel bakış**. Araç çubuğunda **Devre dışı bırak**'ı seçin.

  ![Mantıksal uygulamanızı kapatma](./media/monitor-virtual-machine-changes-event-grid-logic-app/turn-off-disable-logic-app.png)

  > [!TIP]
  > Mantıksal uygulama menüsü görünmüyorsa Azure panosuna dönüp mantıksal uygulamanızı yeniden açmayı deneyin.

* Mantıksal uygulama menüsünde, mantıksal uygulamanız kalıcı olarak silmek için işaretleyin **genel bakış**. Araç çubuğunda **Sil**'i seçin. Mantıksal uygulamanızı silme ve istediğinizi onaylayın **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

* [Event Grid ile özel olaylar oluşturma ve yönlendirme](../event-grid/custom-event-quickstart.md)
