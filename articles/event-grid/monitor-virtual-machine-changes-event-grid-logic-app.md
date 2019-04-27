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
ms.date: 01/12/2019
ms.openlocfilehash: e735c9773971a4c594c32e9ae29eeb295c32810c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60824813"
---
# <a name="tutorial-monitor-virtual-machine-changes-with-azure-event-grid-and-logic-apps"></a>Öğretici: Azure Event Grid ve Logic Apps ile sanal makine değişikliklerini izleme

Azure kaynaklarında veya üçüncü taraf kaynaklarda belirli olaylar olduğunda otomatik bir [mantıksal uygulama iş akışı](../logic-apps/logic-apps-overview.md) başlatabilirsiniz. Bu kaynaklar bu olayları bir [Azure olay kılavuzuna](../event-grid/overview.md) yayımlayabilir. Olay kılavuzu da bu olayları uç nokta olarak kuyruk, web kancası veya [olay hub’ları](../event-hubs/event-hubs-what-is-event-hubs.md) olan abonelere gönderir. Bir abone olarak, mantıksal uygulamanız sizin herhangi bir kod yazmanıza gerek kalmadan görevleri gerçekleştirmek üzere otomatik iş akışlarını çalıştırmak için olay kılavuzundan bu olayları bekleyebilir.

Örneğin, yayımcıların Azure Event Grid hizmeti üzerinden abonelere gönderebileceği bazı olaylar şunlardır:

* Kaynağı oluşturun, okuyun, güncelleştirin veya silin. Örneğin, Azure aboneliğinizde ücretlendirmeye neden olabilecek ve faturanızı etkileyebilecek değişiklikleri izleyebilirsiniz. 
* Bir Azure aboneliğine kişi ekleyin veya kaldırın.
* Uygulamanız belirli bir eylemi gerçekleştirir.
* Bir kuyrukta yeni bir ileti görüntülenir.

Bu öğretici bir sanal makinedeki değişiklikleri izleyen ve bu değişiklikler hakkında e-posta gönderen bir mantıksal uygulama oluşturur. Bir Azure kaynağı için bir mantıksal uygulama oluşturduğunuzda, bu kaynaktan olayların bir mantık kılavuzu üzerinden mantıksal uygulamaya akışı yapılır. Öğretici size bu mantıksal uygulamayı oluşturma sürecinde yardımcı olur:

![Genel bakış - Olay kılavuzu ve mantıksal uygulama ile sanal makineyi izleme](./media/monitor-virtual-machine-changes-event-grid-logic-app/monitor-virtual-machine-event-grid-logic-app-overview.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir olay kılavuzundan olayları izleyen bir mantıksal uygulama oluşturun.
> * Özellikle sanal makine değişikliklerini izleyen bir koşul ekleyin.
> * Sanal makineniz değiştiğinde e-posta gönderin.

## <a name="prerequisites"></a>Önkoşullar

* Bildirim göndermek için [Azure Logic Apps tarafından desteklenen](../connectors/apis-list.md) Office 365 Outlook, Outlook.com veya Gmail gibi bir e-posta sağlayıcıdan alınmış e-posta hesabı. Bu öğreticide Office 365 Outlook kullanılmaktadır.

* Bir [sanal makine](https://azure.microsoft.com/services/virtual-machines). Zaten yapmadıysanız, bir [VM oluşturma öğreticisi](https://docs.microsoft.com/azure/virtual-machines/) kullanarak bir sanal makine oluşturun. Sanal makinenin olayları yayımlaması için, [başka bir işlem yapmanız gerekmez](../event-grid/overview.md).

## <a name="create-a-logic-app-that-monitors-events-from-an-event-grid"></a>Bir olay kılavuzundan olayları izleyen bir mantıksal uygulama oluşturma

İlk olarak, bir mantıksal uygulama oluşturun ve sanal makineniz için kaynak grubunu izleyen bir Event Grid tetikleyicisi ekleyin. 

1. [Azure Portal](https://portal.azure.com) oturum açın. 

2. Azure ana menüsünde sol üst köşeden **Kaynak oluştur** > **Kurumsal Tümleştirme** > **Mantıksal Uygulama**'yı seçin.

   ![Mantıksal uygulama oluşturma](./media/monitor-virtual-machine-changes-event-grid-logic-app/azure-portal-create-logic-app.png)

3. Aşağıdaki tabloda yer alan özelliklerle mantıksal uygulamanızı oluşturun:

   ![Mantıksal uygulama ayrıntılarını sağlayın](./media/monitor-virtual-machine-changes-event-grid-logic-app/create-logic-app-for-event-grid.png)

   | Ayar | Önerilen değer | Açıklama | 
   | ------- | --------------- | ----------- | 
   | **Ad** | *{mantıksal-uygulamanızın-adı}* | Mantıksal uygulama için benzersiz bir ad girin. | 
   | **Abonelik** | *{Azure-aboneliğiniz}* | Bu öğreticideki tüm hizmetler için aynı Azure aboneliğini seçin. | 
   | **Kaynak grubu** | *{Azure-kaynak-grubunuz}* | Bu öğreticideki tüm hizmetler için aynı Azure kaynak grubunu seçin. | 
   | **Konum** | *{Azure-bölgeniz}* | Bu öğreticideki tüm hizmetler için aynı bölgeyi seçin. | 
   | | | 

4. Hazır olduğunuzda **Panoya sabitle**'yi ve ardından **Oluştur**'u seçin.

   Mantıksal uygulamanız için bir Azure kaynağı oluşturdunuz. 
   Azure mantıksal uygulamanızı dağıttıktan sonra Logic Apps Tasarımcısı'nda hızlı bir başlangıç yapmanıza yardımcı olacak ortak desen şablonları gösterilir.

   > [!NOTE] 
   > **Panoya sabitle**’yi seçtiğinizde, mantıksal uygulama otomatik olarak Logic Apps Tasarımcısı’nda açılır. Aksi takdirde mantıksal uygulamanızı kendiniz bulup açabilirsiniz.

5. Şimdi bir mantıksal uygulama şablonu seçin. Mantıksal uygulamanızı sıfırdan oluşturabilmek için **Şablonlar**'ın altından **Boş Mantıksal Uygulama**'yı seçin.

   ![Mantıksal uygulama şablonunu seçin](./media/monitor-virtual-machine-changes-event-grid-logic-app/choose-logic-app-template.png)

   Logic Apps Tasarımcısı artık mantıksal uygulamanızı başlatmak için kullanabileceğiniz [*bağlayıcılar*](../connectors/apis-list.md) ve [*tetikleyiciler*](../logic-apps/logic-apps-overview.md#logic-app-concepts) ile görevleri gerçekleştirmek için bir tetikleyiciden sonra ekleyebileceğiniz eylemleri gösterir. Tetikleyici, bir mantıksal uygulama örneği oluşturan ve mantıksal uygulama iş akışınızı başlatan bir olaydır. 
   Mantıksal uygulamanızın ilk öğesinin bir tetikleyici olması gerekir.

6. Arama kutusuna filtreniz olarak "olay kılavuzu" yazın. Şu tetikleyiciyi seçin: **Azure Event Grid - bir kaynak olayı**

   ![Şu tetikleyiciyi seçin: "Azure Event Grid - bir kaynak olayı"](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger.png)

7. İstendiğinde, Azure kimlik bilgilerinizle Azure Event Grid oturumu açın.

   ![Azure kimlik bilgilerinizle oturum açın](./media/monitor-virtual-machine-changes-event-grid-logic-app/sign-in-event-grid.png)

   > [!NOTE]
   > @outlook.com veya @hotmail.com gibi kişisel bir Microsoft hesabında oturum açtıysanız, Event Grid tetikleyicisi doğru görüntülenmeyebilir. Geçici bir çözüm olarak, [Hizmet Sorumlusu ile bağlan](../active-directory/develop/howto-create-service-principal-portal.md)’ı seçin veya *user-name*@emailoutlook.onmicrosoft.com gibi Azure aboneliğinizle ilişkili bir Azure Active Directory’nin bir üyesi olarak kimlik doğrulaması yapın.

8. Şimdi mantıksal uygulamanızı yayımcı olaylarına kaydedin. Aşağıdaki tabloda belirtildiği gibi olay aboneliğinizin ayrıntılarını sağlayın:

   ![Olay aboneliğinin ayrıntılarını sağlayın](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details-generic.png)

   | Ayar | Önerilen değer | Açıklama | 
   | ------- | --------------- | ----------- | 
   | **Abonelik** | *{virtual-machine-Azure-subscription}* | Olay yayımcısının Azure aboneliğini seçin. Bu öğretici için, sanal makinenizin Azure aboneliğini seçin. | 
   | **Kaynak Türü** | Microsoft.Resources.resourceGroups | Olay yayımcısının kaynak türünü seçin. Bu öğretici için, mantıksal uygulamanızın yalnızca kaynak gruplarını izlemesi için belirtilen değeri seçin. | 
   | **Kaynak Adı** | *{virtual-machine-resource-group-name}* | Yayımcının kaynak adını seçin. Bu öğretici için, sanal makineniz için kaynak grubunun adını seçin. | 
   | İsteğe bağlı ayarları için **Gelişmiş seçenekleri göster**’i seçin. | *{açıklamalara bakın}* | * **Önek filtresi**: Bu öğretici için bu ayarı boş bırakın. Varsayılan davranış tüm değerlerle eşleşir. Ancak filtre olarak bir ön ek dizesi (örneğin belirli bir kaynak için bir yol ve bir parametre) belirtebilirsiniz. <p>* **Sonek filtresi**: Bu öğretici için bu ayarı boş bırakın. Varsayılan davranış tüm değerlerle eşleşir. Ancak yalnızca belirli dosya türlerini istediğinizde filtre olarak bir sonek dizesi (örneğin dosya adı uzantısı) belirtebilirsiniz.<p>* **Abonelik adı**: Olay aboneliğiniz için benzersiz bir ad girin. |
   | | | 

   İşiniz bittiğinde, olay kılavuzu tetikleyiciniz bu örnekteki gibi görünebilir:
   
   ![Örnek olay kılavuzu tetikleyici ayrıntıları](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details.png)

9. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin. Mantıksal uygulamanızda bir eylemin ayrıntılarını daraltmak ve gizlemek için, eylemin başlık çubuğunu seçin.

   ![Mantıksal uygulamanızı kaydetme](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save.png)

   Mantıksal uygulamanızı bir olay kılavuzu tetikleyicisiyle kaydettiğinizde, Azure seçtiğiniz kaynağa mantıksal uygulamanız için otomatik olarak bir olay aboneliği oluşturur. Bu nedenle kaynak bir olayı olay kılavuzuna yayımladığında, bu olay kılavuzu otomatik olarak olayı mantıksal uygulamanıza gönderir. Bu olay mantıksal uygulamanızı tetikler ve ardından sonraki adımlarda yapılandıracağınız iş akışının bir örneğini oluşturur ve çalıştırır.

Mantıksal uygulamanız artık canlı ve olay kılavuzundan olayları dinliyor ancak siz eylemleri iş akışına ekleyene kadar herhangi bir işlem yapmayacak. 

## <a name="add-a-condition-that-checks-for-virtual-machine-changes"></a>Sanal makine değişikliklerini izleyen bir koşul ekleme

Mantıksal uygulama iş akışınızı yalnızca belirli bir olay gerçekleştiğinde çalıştırmak için, sanal makine “write” işlemlerini denetleyen bir koşul ekleyin. Bu koşul true olduğunda, mantıksal uygulamanız size güncelleştirilen sanal makine hakkında ayrıntıları içeren bir e-posta gönderir.

1. Logic Apps Tasarımcısı’nda olay kılavuzu tetikleyicisinin altında **Yeni adım** > **Koşul ekle**’yi seçin.

   ![Mantıksal uygulamanıza koşul ekleyin](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-add-condition-step.png)

   Logic App Tasarımcısı iş akışınıza koşulun true veya false olmasına bağlı olarak izlenecek eylem yolları dahil boş bir koşul ekler.

   ![Boş koşul](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-add-empty-condition.png)

2. **Koşul** kutusunda, **Gelişmiş modda düzenle**’yi seçin.
Şu ifadeyi girin:

   `@equals(triggerBody()?['data']['operationName'], 'Microsoft.Compute/virtualMachines/write')`

   Koşulunuz şu örneğe benzer şekilde görünür:

   ![Boş koşul](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-expression.png)

   Bu ifade, `operationName` özelliği `Microsoft.Compute/virtualMachines/write` işlemi olduğunda olayın `body` öğesini bir `data` için denetler. 
   [Event Grid olay şeması](../event-grid/event-schema.md) hakkında daha fazla bilgi edinin.

3. Koşul için bir açıklama sağlamak için, koşul şeklindeki **üç nokta** (**...**) düğmesini seçin ve ardından **Yeniden adlandır**’ı seçin.

   > [!NOTE] 
   > Bu öğreticideki sonraki örneklerde mantıksal uygulama iş akışındaki adımlar için açıklamalar da sağlanmaktadır.

4. Şimdi, ifadenin otomatik olarak aşağıdaki gibi çözümlenmesi için **Temel modda düzenle**’yi seçin:

   ![Mantıksal uygulama koşulu](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-1.png)

5. Mantıksal uygulamanızı kaydedin.

## <a name="send-email-when-your-virtual-machine-changes"></a>Sanal makineniz değiştiğinde e-posta gönderme

Şimdi belirtilen koşul true olduğunda bir e-posta almak için bir [*eylem*](../logic-apps/logic-apps-overview.md#logic-app-concepts) ekleyin.

1. Koşulun **True ise** kutusunda **Eylem ekle**’yi seçin.

   ![Koşul true olduğunda kullanılacak eylemi ekleyin](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-2.png)

2. Arama kutusuna filtreniz olarak "e-posta" yazın. E-posta sağlayıcınıza uygun bağlayıcıyı bulun ve seçin. Ardından bağlayıcı için "e-posta gönder" eylemini seçin. Örneğin: 

   * Azure iş veya okul hesabı için Office 365 Outlook bağlayıcısını seçin. 
   * Kişisel Microsoft hesapları için Outlook.com bağlayıcısını seçin. 
   * Gmail hesapları için Gmail bağlayıcısını seçin. 

   İşleme Office 365 Outlook bağlayıcısıyla devam edeceğiz. 
   Farklı bir sağlayıcı kullandığınızda adımlar aynı olacaktır ancak kullanıcı arabirimi farklı olabilir. 

   !["E-posta gönder" eylemini seçin](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email.png)

3. E-posta sağlayıcınız için zaten bir bağlantınız yoksa, kimliğinizi doğrulamanız istendiğinde e-posta hesabınızda oturum açın.

4. Aşağıdaki tabloda belirtildiği gibi e-posta için ayrıntıları sağlayın:

   ![Boş e-posta eylemi](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-empty-email-action.png)

   > [!TIP]
   > İş akışınızda kullanılabilir alanlardan seçim yapmak için, düzenleme kutusuna tıklayarak **Dinamik içerik** listesini açın veya **Dinamik içerik ekle**'yi seçin. Daha fazla alan için, listedeki her bölüm için **Daha fazla göster**’i seçin. **Dinamik içerik** listesini kapatmak için, **Dinamik içerik ekle**’yi seçin.

   | Ayar | Önerilen değer | Açıklama | 
   | ------- | --------------- | ----------- | 
   | **Alıcı** | *{recipient-email-address}* |Alıcının e-posta adresi girin. Test için kendi e-posta adresinizi kullanabilirsiniz. | 
   | **Konu** | Güncelleştirilen kaynağı: **Konu**| E-posta konusunun içeriğini girin. Bu öğreticide önerilen metni girin ve olayın **Konu** alanını seçin. Burada, e-postanızın konusu güncelleştirilen kaynağın (sanal makine) adını içerir. | 
   | **Gövde** | Kaynak grubu: **Konu** <p>Olay türü: **Olay türü**<p>Olay Kimliği: **ID**<p>Zaman: **Olay saati** | E-posta gövdesinin içeriğini girin. Bu öğretici için, e-postanızın güncelleştirme için grup adı, olay türü, olay zaman damgası ve olay kimliğini içermesi için önerilen metni girin ve olay için **Konu**, **Olay Türü**, **Kimlik** ve **Olay Zamanı** alanlarını seçin. <p>İçeriğinize boş satır eklemek için Shift + Enter tuşlarını kullanın. | 
   | | | 

   > [!NOTE] 
   > Bir diziyi temsil eden bir alan seçerseniz, tasarımcı eyleme otomatik olarak diziye başvuran bir **For each** döngüsü ekler. Bu şekilde mantıksal uygulamanız ilgili eylemi dizideki tüm öğeler için gerçekleştirir.

   Şimdi, e-posta eyleminiz bu örnekteki gibi görünebilir:

   ![E-postaya eklenecek çıkışları seçme](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email-details.png)

   Tamamlanmış mantıksal uygulamanız örnekteki gibi görünebilir:

   ![Tamamlanmış mantıksal uygulama](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-completed.png)

5. Mantıksal uygulamanızı kaydedin. Mantıksal uygulamanızda her eylemin ayrıntılarını daraltmak ve gizlemek için, eylemin başlık çubuğunu seçin.

   ![Mantıksal uygulamanızı kaydetme](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save-completed.png)

   Mantıksal uygulamanız artık canlıdır ancak herhangi bir işlem gerçekleştirmeden önce sanal makinenizde yapılan değişiklikleri bekler. 
   Mantıksal uygulamanızı şimdi test etmek için sonraki bölüme geçin.

## <a name="test-your-logic-app-workflow"></a>Mantıksal uygulama iş akışınızı test etme

1. Mantıksal uygulamanızın belirtilen olayları alıp almadığını denetlemek için, sanal makinenizi güncelleştirin. 

   Örneğin, Azure portalında sanal makinenizi yeniden boyutlandırabilir veya [VM’nizi Azure PowerShell ile yeniden boyutlandırabilirsiniz](../virtual-machines/windows/resize-vm.md). 

   Birkaç dakika sonra bir e-posta almanız gerekir. Örneğin:

   ![Sanal makine güncelleştirmesi hakkında e-posta](./media/monitor-virtual-machine-changes-event-grid-logic-app/email.png)

2. Mantıksal uygulamanız için çalıştırmaları ve tetikleyici geçmişini gözden geçirmek için **Genel Bakış**'ı seçin. Bir çalıştırma hakkında daha fazla bilgiye ulaşmak için çalıştırmayla ilgili satırı seçin.

   ![Mantıksal uygulama çalıştırma geçmişi](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history.png)

3. Her bir adımın giriş ve çıkışlarını görüntülemek için gözden geçirmek istediğiniz adımı genişletin. Bu bilgiler mantıksal uygulamanızdaki sorunları tespit etmenize ve gidermenize yardımcı olabilir.
 
   ![Mantıksal uygulama çalıştırma geçmişi ayrıntıları](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history-details.png)

Tebrikler, bir olay kılavuzuyla kaynak olaylarını izleyen ve bu olaylar gerçekleştiğinde size e-posta gönderen bir mantıksal uygulama oluşturdunuz. Ayrıca, süreçleri otomatik hale getiren iş akışlarını ne kadar kolay oluşturabileceğinizi ve sistemler ile bulut hizmetlerini tümleştirmeyi öğrendiniz.

Olay kılavuzları ve mantıksal uygulamalarla diğer yapılandırma değişikliklerini izleyebilirsiniz, örneğin:

* Bir sanal makine rol tabanlı erişim denetimi (RBAC) haklarını alır.
* Değişiklikler bir ağ arabirimi (NIC) üzerindeki bir ağ güvenlik grubunda (NSG) yapılır.
* Bir sanal makine için diskler eklenir veya kaldırılır.
* Bir sanal makine NIC’sine genel bir IP adresi atanır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide Azure aboneliğinize ücret uygulanmasına neden olan kaynaklar kullanılmakta ve eylemler gerçekleştirilmektedir. Bu nedenle öğreticiyi ve testlerinizi tamamladıktan sonra ücret uygulanmasını istemediğiniz kaynakları devre dışı bırakmayı veya silmeyi unutmayın.

* Çalışmanızı silmeden mantıksal uygulamanızı durdurmak için uygulamanızı devre dışı bırakın. Mantıksal uygulama menüsünden **Genel Bakış**'ı seçin. Araç çubuğunda **Devre dışı bırak**'ı seçin.

  ![Mantıksal uygulamanızı kapatma](./media/monitor-virtual-machine-changes-event-grid-logic-app/turn-off-disable-logic-app.png)

  > [!TIP]
  > Mantıksal uygulama menüsü görünmüyorsa Azure panosuna dönüp mantıksal uygulamanızı yeniden açmayı deneyin.

* Mantıksal uygulamanızı kalıcı olarak silmek için, mantıksal uygulama menüsünde **Genel Bakış**’ı seçin. Araç çubuğunda **Sil**'i seçin. Mantıksal uygulamanızı silmek istediğinizi onaylayın ve **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Event Grid ile özel olaylar oluşturma ve yönlendirme](../event-grid/custom-event-quickstart.md)
