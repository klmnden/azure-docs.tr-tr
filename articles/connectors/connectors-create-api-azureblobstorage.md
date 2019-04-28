---
title: Azure blob depolamaya - Azure Logic Apps bağlanma
description: Oluşturma ve Azure Logic Apps ile Azure Depolama'daki blobları yönetme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 05/21/2018
tags: connectors
ms.openlocfilehash: ea3e97db9ec560306788943d92a7670025f38bdc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60958661"
---
# <a name="create-and-manage-blobs-in-azure-blob-storage-with-azure-logic-apps"></a>Oluşturma ve Azure Logic Apps ile Azure blob Depolama'daki blobları yönetme

Bu makalede nasıl erişebilir ve Azure Blob Depolama Bağlayıcısı ile bir mantıksal uygulama içinde Azure depolama hesabınızdaki BLOB olarak depolanan dosyalar yönetme gösterilmektedir. Böylece, görevler ve dosyalarınızı yönetmek için iş akışlarını otomatik hale getiren mantıksal uygulamalar oluşturabilirsiniz. Örneğin, oluşturma, alma, güncelleştirme ve depolama hesabınızda dosyaları silme mantıksal uygulamalar oluşturabilirsiniz.

Bir Azure Web sitesinde güncelleştirilir bir araç olduğunu varsayalım. mantıksal uygulamanızın tetikleyici olarak davranır. Bu olay meydana geldiğinde mantıksal uygulamanızda bir eylem, blob depolama kapsayıcısında bazı dosyasını güncelleştirin, mantıksal uygulama olabilir.

> [!NOTE]
> Logic Apps, doğrudan güvenlik duvarları üzerinden Azure depolama hesaplarınızı bağlama desteklemiyor. Bu depolama hesaplarından erişmeye, burada iki seçenekten birini kullanın:
>
> * Oluşturma bir [tümleştirme hizmeti ortamı](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md), hangi Azure sanal ağdaki kaynaklara bağlanabilir.
>
> * API Management'ı zaten kullanıyorsanız, bu senaryo için bu hizmeti kullanabilirsiniz. Daha fazla bilgi için bkz. [basit Kurumsal tümleştirme mimarisi](https://aka.ms/aisarch).

Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Bağlayıcısı özel teknik bilgiler için bkz. <a href="https://docs.microsoft.com/connectors/azureblobconnector/" target="blank">Azure Blob Depolama Bağlayıcısı başvurusu</a>.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Bir [Azure depolama hesabı ve depolama kapsayıcısı](../storage/blobs/storage-quickstart-blobs-portal.md)

* Mantıksal uygulama Burada, Azure blob depolama hesabınıza erişmeniz gerekir. Mantıksal uygulamanızı bir Azure Blob Depolama tetikleyici ile başlayın, gerek bir [boş mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="add-trigger"></a>

## <a name="add-blob-storage-trigger"></a>BLOB Depolama tetikleyicisi Ekle

Azure Logic Apps'te, her mantıksal uygulama ile başlamalıdır bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts), belirli bir olay harekete geçirilir gerçekleşen veya belirli bir koşul karşılanıyorsa zaman. Her zaman tetikleyici Logic Apps altyapısı bir mantıksal uygulama örneği oluşturur ve uygulamanızın iş akışı çalışmaya başlar.

Bu örnek, bir mantıksal uygulama iş akışı ile nasıl başlatılacağı gösterir **Azure Blob Depolama'da bir blob eklendiğinde veya değiştirildiğinde (yalnızca Özellikler) -** bir blob'un özelliklerini veya eklenen depolama kapsayıcınızda güncelleştirildiğinde tetikleyici. 

1. Azure portalı ya da Visual Studio, mantıksal Uygulama Tasarımcısı açılır bir boş mantıksal uygulama oluşturun. Bu örnek, Azure portalını kullanır.

2. Arama kutusuna filtreniz olarak "azure blob" girin. Tetikleyiciler listesinden istediğiniz tetikleyicisini seçin.

   Bu örnek, bu tetikleyici kullanır: **Bir blob eklendiğinde veya değiştirildiğinde (yalnızca Özellikler), azure Blob Depolama-**

   ![Tetikleyici seçin](./media/connectors-create-api-azureblobstorage/azure-blob-trigger.png)

3. Bağlantı ayrıntıları için istenirse [blob depolama bağlantınızı şimdi oluşturmak](#create-connection). Veya, bağlantınız zaten varsa, tetikleyici için gerekli bilgileri sağlayın.

   Bu örnekte, kapsayıcı ve izlemek istediğiniz klasörü seçin.

   1. İçinde **kapsayıcı** kutusunda, klasör simgesini seçin.

   2. Sağ açılı ayraç klasörü listeden seçin ( **>** ) ve ardından bulmak istediğiniz klasörü seçin kadar göz atın.

      ![Klasör seçin](./media/connectors-create-api-azureblobstorage/trigger-select-folder.png)

   3. Klasördeki değişiklikleri denetlemek için tetikleyici istediğiniz sıklığı için sıklık ve aralığı seçin.

4. Tasarımcı araç çubuğunda, işiniz bittiğinde seçin **Kaydet**.

5. Şimdi mantıksal uygulamanız tetikleme sonuçlarıyla gerçekleştirmek istediğiniz görevleri için bir veya daha fazla eylem ekleyerek devam edin.

<a name="add-action"></a>

## <a name="add-blob-storage-action"></a>BLOB Depolama Eylem Ekle

Azure Logic apps'te bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) akışınıza bir tetikleyici veya başka bir eylem izleyen bir adımdır. Bu örnekte, mantıksal uygulama ile başlar [yinelenme tetikleyicisini](../connectors/connectors-native-recurrence.md).

1. Azure portalı ya da Visual Studio, mantıksal uygulamanızı Logic App Tasarımcısı'nda açın. Bu örnek, Azure portalını kullanır.

2. Mantıksal Uygulama Tasarımcısı, tetikleyici veya eylemi seçin **yeni adım** > **Eylem Ekle**.

   ![Eylem ekleme](./media/connectors-create-api-azureblobstorage/add-action.png) 

   Var olan adımlar arasında bir eylem eklemek için bağlantı okun üzerine fareyi hareket ettirin. 
   Artı işaretini seçin (**+**), görünür ve ardından **Eylem Ekle**.

3. Arama kutusuna filtreniz olarak "azure blob" girin. Eylem listesinden istediğiniz eylemi seçin.

   Bu örnek, bu eylem kullanır: **Azure Blob Depolama - blob içeriğini Al**

   ![Eylem seçin](./media/connectors-create-api-azureblobstorage/azure-blob-action.png) 

4. Bağlantı ayrıntıları için istenirse [artık Azure Blob Depolama bağlantınızı oluşturmak](#create-connection). Veya, bağlantınız zaten varsa, eylem için gerekli bilgileri sağlayın.

   Bu örnekte, istediğiniz dosyayı seçin.

   1. Gelen **Blob** kutusunda, klasör simgesini seçin.
  
      ![Klasör seçin](./media/connectors-create-api-azureblobstorage/action-select-folder.png)

   2. Blob üzerinde tabanlı istediğiniz dosyayı bulup **kimliği** sayı. Bunu bulabilirsiniz **kimliği** blobun meta verilerinde daha önce açıklandığı gibi blob depolama tetikleyicisi tarafından döndürülen sayı.

5. Tasarımcı araç çubuğunda, işiniz bittiğinde seçin **Kaydet**.
Mantıksal uygulamanızı test etmek için seçili klasör bir blob içerdiğinden emin olun.

Bu örnekte, yalnızca bir blob içeriğini alır. İçeriği görüntülemek için bir dosya başka bir bağlayıcı kullanarak blob oluşturur. başka bir eylem ekleyin. Örneğin, blob içeriği temel alınarak bir dosya oluşturur bir OneDrive eylem ekleme.

<a name="create-connection"></a>

## <a name="connect-to-storage-account"></a>Depolama hesabına bağlama

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

[!INCLUDE [Create a connection to Azure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları, bağlayıcının açık API tarafından açıklandığı gibi teknik ayrıntılar için (önceki adıyla Swagger) dosyası, bkz: [bağlayıcının başvuru sayfası](/connectors/azureblobconnector/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
