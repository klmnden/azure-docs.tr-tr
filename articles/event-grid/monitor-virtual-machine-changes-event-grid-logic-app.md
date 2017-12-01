---
title: "Sanal makine değişikliklerini - Azure olay kılavuz & Logic Apps izleme | Microsoft Docs"
description: "Azure olay kılavuz ve Logic Apps kullanarak sanal makineleri (VM'ler) yapılandırma değişiklikleri denetleyin"
keywords: "Logic apps, olay kılavuzları, sanal makine, VM"
services: logic-apps
author: ecfan
manager: anneta
ms.assetid: 
ms.workload: logic-apps
ms.service: logic-apps
ms.topic: article
ms.date: 11/30/2017
ms.author: LADocs; estfan
ms.openlocfilehash: df1e19b772b41064aff1f345dee93813f0c21c73
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="monitor-virtual-machine-changes-with-azure-event-grid-and-logic-apps"></a>Azure olay kılavuz ve Logic Apps ile sanal makine değişikliklerini izleme

Bir Otomatik Başlat [mantığı uygulama iş akışı](../logic-apps/logic-apps-what-are-logic-apps.md) belirli olaylar olduğunda gerçekleşir Azure kaynakları veya üçüncü taraf kaynakları. Bu kaynaklar, bu olaylarla yayımlayabilirsiniz bir [Azure olay kılavuz](../event-grid/overview.md). Sırayla bu olayları olay kılavuz sıralar, Web kancalarını, abonelere iter veya [olay hub'ları](../event-hubs/event-hubs-what-is-event-hubs.md) uç noktalar olarak. Bir abone olarak mantıksal uygulamanızı bu olayları olay grid -, herhangi bir kod yazmadan görevler için otomatik iş akışları çalıştırmadan önce bekleyebilirsiniz.

Örneğin, yayımcılar Azure olay kılavuz hizmeti aracılığıyla abonelere gönderebilirsiniz bazı olaylar şunlardır:

* Oluşturma, okuma, güncelleştirme veya kaynak silme. Örneğin, Azure aboneliğinizde üzerinde ücretlendirme ve faturanızı etkileyen değişiklikleri izleyebilirsiniz. 
* Ekleyebilir veya bir kişinin Azure aboneliğinden kaldırabilirsiniz.
* Uygulamanızı belirli bir eylemi gerçekleştirir.
* Bir kuyruktaki yeni bir ileti görüntülenir.

Bu öğretici bir sanal makine yapılan değişiklikleri izler ve bu değişiklikleri hakkında e-posta gönderen bir mantıksal uygulama oluşturur. Bir Azure kaynağı için bir olay aboneliği ile bir mantıksal uygulama oluşturduğunuzda, olayları olay kılavuz aracılığıyla bu kaynaktan mantıksal uygulama akışı. Öğretici, bu mantıksal uygulama oluşturma sürecinde yardımcı olur:

![Genel Bakış - olay kılavuz ve mantığı uygulama ile yeni bir sanal makine İzleyicisi](./media/monitor-virtual-machine-changes-event-grid-logic-app/monitor-virtual-machine-event-grid-logic-app-overview.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir olay kılavuz olayları izler bir mantıksal uygulama oluşturun.
> * Özellikle sanal makine değişiklikleri denetleyen bir koşul ekleyin.
> * Sanal makineniz değiştiğinde e-posta gönderin.

## <a name="prerequisites"></a>Ön koşullar

* Bir e-posta hesabından [Azure mantıksal uygulamaları tarafından desteklenen herhangi bir e-posta sağlayıcısına](../connectors/apis-list.md)Office 365 Outlook, Outlook.com veya bildirim göndermek için Gmail gibi. Bu öğreticide Office 365 Outlook kullanılmaktadır.

* A [sanal makine](https://azure.microsoft.com/services/virtual-machines). Zaten yapmadıysanız, bir sanal makine üzerinden oluşturmak bir [VM öğretici oluşturma](https://docs.microsoft.com/azure/virtual-machines/). Sanal makine olayları, yayımlama yapmak için [başka bir şey yapmanız gerekmez](../event-grid/overview.md).

## <a name="create-a-logic-app-that-monitors-events-from-an-event-grid"></a>Bir olay kılavuz olayları izler bir mantıksal uygulama oluşturma

İlk olarak, bir mantıksal uygulama oluşturma ve kaynak grubu, sanal makine için izleyen olay kılavuz tetikleyicisi ekleyin. 

1. [Azure Portal](https://portal.azure.com) oturum açın. 

2. Ana Azure menü sol üst köşesinden seçin **yeni** > **Kurumsal tümleştirme** > **mantıksal uygulama**.

   ![Mantıksal uygulama oluşturma](./media/monitor-virtual-machine-changes-event-grid-logic-app/azure-portal-create-logic-app.png)

3. Aşağıdaki tabloda belirtilen ayarlarla mantıksal uygulamanızı oluşturun:

   ![Mantıksal uygulama ayrıntılarını sağlayın](./media/monitor-virtual-machine-changes-event-grid-logic-app/create-logic-app-for-event-grid.png)

   | Ayar | Önerilen değer | Açıklama | 
   | ------- | --------------- | ----------- | 
   | **Ad** | *{mantığı-uygulamanızın-adı}* | Mantıksal uygulama için benzersiz bir ad girin. | 
   | **Abonelik** | *{your-Azure-} aboneliği* | Bu öğreticide tüm hizmetler için aynı Azure aboneliğini seçin. | 
   | **Kaynak grubu** | *{your-Azure-resource-group}* | Bu öğreticide tüm hizmetler için aynı Azure kaynak grubu seçin. | 
   | **Konum** | *{your-Azure-bölgesindeki zaman}* | Tüm hizmetler için aynı bölgede Bu öğreticide seçin. | 
   | | | 

4. Hazır olduğunuzda, seçin **panoya Sabitle**ve seçin **oluşturma**.

   Mantıksal uygulamanız için bir Azure kaynağı oluşturdunuz. 
   Azure mantıksal uygulamanızı dağıttıktan sonra Logic Apps Tasarımcısı'nda hızlı bir başlangıç yapmanıza yardımcı olacak ortak desen şablonları gösterilir.

   > [!NOTE] 
   > Seçtiğinizde, **panoya Sabitle**, mantıksal uygulamanızı Logic Apps Tasarımcısı'nda otomatik olarak açılır. Aksi takdirde, el ile bulabilir ve mantıksal uygulamanızı açın.

5. Şimdi mantıksal uygulama şablonu seçin. Altında **şablonları**, seçin **boş mantıksal uygulama** mantıksal uygulamanızı sıfırdan oluşturabilirsiniz.

   ![Mantıksal uygulama şablonunu seçin](./media/monitor-virtual-machine-changes-event-grid-logic-app/choose-logic-app-template.png)

   Logic Apps Tasarımcısı'nı şimdi, gösterir [ *Bağlayıcılar* ](../connectors/apis-list.md) ve [ *Tetikleyicileri* ](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) , mantıksal uygulama ve ayrıca eylemleri başlatmak için kullanabilirsiniz, görevleri gerçekleştirmek için bir tetikleyici sonra ekleyebilirsiniz. Bir tetikleyici bir mantıksal uygulama örneği oluşturur ve logic app akışınızı başlatan bir olaydır. 
   Mantıksal uygulamanızı bir tetikleyici ilk öğe gerekir.

6. Arama kutusuna "olay kılavuz", filtre olarak girin. Bu tetikleyici seçin: **Azure olay Kılavuzu - kaynak olayı**

   ![Bu tetikleyici seçin: "Azure olay Kılavuzu - kaynak olay"](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger.png)

7. İstendiğinde, Azure olay kılavuza Azure kimlik bilgilerinizle oturum açın.

   ![Azure kimlik bilgilerinizle oturum](./media/monitor-virtual-machine-changes-event-grid-logic-app/sign-in-event-grid.png)

   > [!NOTE]
   > Kişisel bir Microsoft hesabıyla oturum gibi kapattığınızdan varsa @outlook.com veya @hotmail.com, olay kılavuz tetikleyici düzgün görünmeyebilir. Geçici bir çözüm olarak seçin [Connect ile hizmet sorumlusu](../azure-resource-manager/resource-group-create-service-principal-portal.md), veya örneğin, Azure aboneliğinizle ilişkili Azure Active Directory'nun bir üyesi olarak kimlik doğrulaması *kullanıcı adı* @emailoutlook.onmicrosoft.com.

8. Şimdi mantıksal uygulamanızı yayımcı olaylara abone olma. Aşağıdaki tabloda belirtildiği gibi olay aboneliğinizin ayrıntılarını verin:

   ![Ayrıntılar için olay abonelik sağlayın](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details-generic.png)

   | Ayar | Önerilen değer | Açıklama | 
   | ------- | --------------- | ----------- | 
   | **Abonelik** | *{sanal makine-Azure-abonelik}* | Olay publisher'ın Azure aboneliğini seçin. Bu öğretici için sanal makine için Azure aboneliğini seçin. | 
   | **Kaynak Türü** | Microsoft.Resources.resourceGroups | Olay publisher'ın kaynak türü seçin. Yalnızca kaynak grupları mantıksal uygulamanızı izler şekilde Bu öğretici için belirtilen değer seçin. | 
   | **Kaynak adı** | *{sanal-makine-resource-grup-adı}* | Publisher'ın kaynak adı seçin. Bu öğretici için sanal makine için kaynak grubu adını seçin. | 
   | İsteğe bağlı ayarlarını seçin **Gelişmiş Seçenekleri Göster**. | *{açıklamalarına bakın}* | * **Filtre önek**: Bu öğretici için bu ayarı boş bırakın. Varsayılan davranış tüm değerleri eşleşir. Bununla birlikte, örneğin, bir yol ve belirli bir kaynak için bir parametre bir filtre olarak önek dizesi belirtebilirsiniz. <p>* **Sonek filtre**: Bu öğretici için bu ayarı boş bırakın. Varsayılan davranış tüm değerleri eşleşir. Ancak, yalnızca belirli dosya türlerini istediğinizde bir filtre, örneğin, bir dosya adı uzantısı, sonek dizesi belirtebilirsiniz.<p>* **Abonelik adı**: olay aboneliğiniz için benzersiz bir ad sağlayın. |
   | | | 

   İşiniz bittiğinde, olay kılavuz tetikleyicisi bu örnekteki gibi görünebilir:
   
   ![Örnek olay kılavuz tetikleyici ayrıntıları](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details.png)

9. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin. Daraltma ve gizleme mantıksal uygulamanızı bir eylemin Ayrıntılar için eylemin başlık çubuğu seçin.

   ![Mantıksal uygulamanızı kaydetme](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save.png)

   Mantıksal uygulamanızı bir olay kılavuz tetikleyicisi ile kaydettiğinizde, Azure mantıksal uygulamanıza, seçilen kaynak için bir olay aboneliği otomatik olarak oluşturur. Bu nedenle, kaynağı olay kılavuza bir olay yayımlandığında, bu olay kılavuz otomatik olarak olay mantıksal uygulamanızı iter. Bu olay mantıksal uygulamanızı tetikler sonra oluşturur ve sonraki adımları tanımladığınız iş akışı örneğini çalıştırır.

Mantıksal uygulamanız artık canlı ve olay kılavuzdan olayları dinler, ancak Eylemler iş akışı için eklenene kadar hiçbir şey yapmaz. 

## <a name="add-a-condition-that-checks-for-virtual-machine-changes"></a>Sanal makine değişiklikleri denetleyen bir koşul Ekle

Yalnızca belirli bir olay gerçekleştiğinde mantığı uygulama akışınızı çalıştırmak için sanal makine için "yazma işlemlerini" denetleyen bir koşul ekleyin. Bu koşul true olduğunda, mantıksal uygulamanızı güncelleştirilmiş sanal makineye ilişkin ayrıntıları içeren e-posta gönderir.

1. Mantıksal Uygulama Tasarımcısı'nda olay kılavuz tetikleyici altında seçin **yeni adım** > **bir koşul eklemek**.

   ![Mantıksal uygulamanız için bir koşul Ekle](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-add-condition-step.png)

   Mantıksal Uygulama Tasarımcısı'nı koşul true veya false olup göre izlemek için eylem yolları dahil olmak üzere iş akışınıza, boş bir koşul ekler.

   ![Boş koşulu](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-add-empty-condition.png)

2. İçinde **koşulu** kutusunda, seçin **Gelişmiş modda Düzenle**.
Bu ifade girin:

   `@equals(triggerBody()?['data']['operationName'], 'Microsoft.Compute/virtualMachines/write')`

   Koşulunuz şimdi aşağıdaki gibi görünür:

   ![Boş koşulu](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-expression.png)

   Bu ifade olayı denetler `body` için bir `data` nesne nerede `operationName` özelliği `Microsoft.Compute/virtualMachines/write` işlemi. 
   Daha fazla bilgi edinmek [olay kılavuz olay şema](../event-grid/event-schema.md).

3. Koşul için bir açıklama sağlayın, tercih **üç nokta** (**...** ) düğmesini koşul şekil üzerinde sonra seçin **yeniden adlandırma**.

   > [!NOTE] 
   > Bu öğreticide daha sonra örnekler de adımları mantığı uygulama iş akışı için açıklamalar sağlar.

4. Artık seçim **temel modunda Düzenle** böylece ifade gösterildiği gibi otomatik olarak çözer:

   ![Mantıksal uygulama durumu](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-1.png)

5. Mantıksal uygulamanızı kaydedin.

## <a name="send-email-when-your-virtual-machine-changes"></a>Sanal makineniz değiştiğinde e-posta Gönder

Şimdi ekleyin bir [ *eylem* ](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) böylece belirtilen koşulun doğru olması durumunda, bir e-posta alırsınız.

1. Koşulunun içinde **true ise** kutusunda, seçin **Eylem Ekle**.

   ![Koşul true olduğunda için Eylem Ekle](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-2.png)

2. Arama kutusuna "e-posta", filtre olarak girin. E-posta sağlayıcınıza uygun bağlayıcıyı bulun ve seçin. Ardından bağlayıcı için "e-posta gönder" eylemini seçin. Örneğin: 

   * Azure iş veya okul hesabı için Office 365 Outlook bağlayıcısını seçin. 
   * Kişisel Microsoft hesapları için Outlook.com bağlayıcısını seçin. 
   * Gmail hesapları için Gmail bağlayıcısını seçin. 

   İşleme Office 365 Outlook bağlayıcısıyla devam edeceğiz. 
   Farklı bir sağlayıcı kullandığınızda adımlar aynı olacaktır ancak kullanıcı arabirimi farklı olabilir. 

   !["E-posta Gönder" eylemini seçin](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email.png)

3. E-posta sağlayıcınız için bir bağlantı zaten yoksa, kimlik doğrulaması için sorulduğunda e-posta hesabınızda oturum açın.

4. Aşağıdaki tabloda belirtildiği gibi e-posta için bilgileri sağlayın:

   ![Boş bir e-posta eylemi](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-empty-email-action.png)

   > [!TIP]
   > İş akışınızda kullanılabilir alanları seçmek için bir düzenleme kutusuna böylece tıklattıktan **dinamik içerik** açılır liste veya seçin **dinamik içerik eklemek**. Daha fazla alan için seçin **daha fazla** listedeki her bölüm için. Kapatmak için **dinamik içerik** listesinde, seçin **dinamik içerik eklemek**.

   | Ayar | Önerilen değer | Açıklama | 
   | ------- | --------------- | ----------- | 
   | **Alıcı** | *{Alıcı e-posta adresi}* |Alıcının e-posta adresi girin. Test için kendi e-posta adresinizi kullanabilirsiniz. | 
   | **Konu** | Güncelleştirilmiş kaynak: **konu**| E-posta konusunun içeriğini girin. Bu öğretici için önerilen metin girin ve olayın **konu** alan. Burada, e-posta konusu güncelleştirilmiş bir kaynak (sanal makine) adını içerir. | 
   | **Gövde** | Kaynak grubu: **konu** <p>Olay türü: **olay türü**<p>Olay Kimliği: **kimliği**<p>Süre: **olay süresi** | E-posta gövdesinin içeriğini girin. Bu öğretici için önerilen metin girin ve olayın **konu**, **olay türü**, **kimliği**, ve **olay süresi** alanları için e-posta, kaynak grubu adı, olay türü, olay zaman damgası ve güncelleştirmesi olay Kimliğini içerir. <p>Boş satırlar, içeriği eklemek için SHIFT + Enter tuşuna basın. | 
   | | | 

   > [!NOTE] 
   > Tasarımcı otomatik olarak ekler, bir dizi temsil eden bir alan seçerseniz, bir **her** dizi başvuran eylem çevresine daire. Bu şekilde mantıksal uygulamanız ilgili eylemi dizideki tüm öğeler için gerçekleştirir.

   Şimdi, e-posta eyleminizi aşağıdaki örnekte olduğu gibi görünebilir:

   ![E-postayla içerecek şekilde çıkışları seçin](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email-details.png)

   Ve tamamlanmış mantıksal uygulamanızı bu örnekteki gibi görünmelidir:

   ![Tamamlanmış mantıksal uygulama](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-completed.png)

5. Mantıksal uygulamanızı kaydedin. Daraltma ve gizleme mantıksal uygulamanızı her eylemin Ayrıntılar için eylemin başlık çubuğu seçin.

   ![Mantıksal uygulamanızı kaydetme](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save-completed.png)

   Mantıksal uygulamanızı artık canlı, ancak hiçbir şey yapmadan önce sanal makinenize değişiklikler bekler. 
   Mantıksal uygulamanızı şimdi test etmek için sonraki bölüme geçin.

## <a name="test-your-logic-app-workflow"></a>Mantıksal uygulama akışınızı test

1. Mantıksal uygulamanızı belirtilen olayları alınırken olduğunu denetlemek için sanal makinenizi güncelleştirin. 

   Örneğin, sanal makinenizi Azure portalında boyutlandırabilirsiniz veya [Azure PowerShell ile VM'yi yeniden boyutlandırın](../virtual-machines/windows/resize-vm.md). 

   Birkaç dakika sonra bir e-posta almanız gerekir. Örneğin:

   ![Sanal makine güncelleştirmesi hakkında e-posta](./media/monitor-virtual-machine-changes-event-grid-logic-app/email.png)

2. Çalışmaları gözden geçirin ve logic app menünüzde mantıksal uygulamanızı geçmişini tetiklemek tercih **genel bakış**. Bir çalıştırma hakkında daha fazla bilgiye ulaşmak için çalıştırmayla ilgili satırı seçin.

   ![Mantıksal uygulama geçmişi çalıştırır](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history.png)

3. Her bir adımın giriş ve çıkışlarını görüntülemek için gözden geçirmek istediğiniz adımı genişletin. Bu bilgiler mantıksal uygulamanızdaki sorunları tespit etmenize ve gidermenize yardımcı olabilir.
 
   ![Mantıksal uygulama geçmiş ayrıntıları çalıştırın](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history-details.png)

Tebrikler, oluşturduğunuz ve bir olay kılavuz aracılığıyla kaynak olaylarını izler ve olaylar oluştuğunda e-postalar bir mantıksal uygulama çalıştırın. Ayrıca, sistemleri tümleştirme süreçlerini otomatikleştirmek ve bulut Hizmetleri iş akışları ne kadar kolay oluşturabilirsiniz öğrendiniz.

Örneğin olay kılavuzları ve logic apps ile diğer yapılandırma değişikliklerini izleyebilirsiniz:

* Bir sanal makine rol tabanlı erişim denetimi (RBAC) hakları alır.
* Değişiklikler, bir ağ arabiriminde (NIC) bir ağ güvenlik grubu (NSG) yapılır.
* Sanal makinesi için diskler eklenir veya kaldırılır.
* Bir sanal makineye NIC genel bir IP adresi atanır

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici kaynaklarını kullanır ve Azure aboneliğinize üzerinde ücretlendirme eylemleri gerçekleştirir. Bu nedenle bu öğreticiyi işiniz bittiğinde ve test, olduğundan emin olun, devre dışı veya burada ücretlendirme istemediğiniz tüm kaynakları silmesini.

* Çalışmanızı silmeden mantıksal uygulamanızı durdurmak için uygulamanızı devre dışı bırakın. Mantıksal uygulama menüsünden **Genel Bakış**'ı seçin. Araç çubuğunda **Devre dışı bırak**'ı seçin.

  ![Mantıksal uygulamanızı kapatma](./media/monitor-virtual-machine-changes-event-grid-logic-app/turn-off-disable-logic-app.png)

  > [!TIP]
  > Mantıksal uygulama menüsü görünmüyorsa Azure panosuna dönüp mantıksal uygulamanızı yeniden açmayı deneyin.

* Mantıksal uygulama menüsünde, mantıksal uygulamanızı kalıcı olarak silmek mi seçin **genel bakış**. Araç çubuğunda **Sil**'i seçin. Mantıksal uygulamanızı silmek istediğinizi onaylayın ve **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Oluşturma ve rota olay kılavuzuna özel olaylar](../event-grid/custom-event-quickstart.md)
