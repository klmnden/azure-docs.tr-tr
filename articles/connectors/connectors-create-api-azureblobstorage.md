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
ms.date: 06/20/2019
tags: connectors
ms.openlocfilehash: d9c29837e99d327112e6a9d648a5c56cc35e8555
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67296657"
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

Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Bağlayıcısı özel teknik bilgiler için bkz. [Azure Blob Depolama Bağlayıcısı başvurusu](/connectors/azureblobconnector/).

## <a name="limits"></a>Limits

* Varsayılan olarak, Azure Blob Depolama işlemleri okuma veya yazma dosyaları *50 MB veya daha küçük*. En fazla 1024 MB ancak 50 MB'tan büyük dosyaları işlemek için Azure Blob Depolama eylemleri destekleyen [ileti Öbekleme](../logic-apps/logic-apps-handle-large-messages.md). **Get blob içeriği** eylem örtülü olarak kullanan Öbekleme.

* Azure Blob Depolama Tetikleyicileri Öbekleme desteklemez. Dosya içeriği isterken Tetikleyicileri 50 MB üzerinde olan dosyalar seçin veya daha küçük. 50 MB'tan büyük dosyaları almak için bu düzeni izleyin:

  * Dosya özellikleri gibi döndüren Azure Blob Depolama Tetikleyici kullanma **bir blob eklendiğinde veya değiştirildiğinde (yalnızca Özellikler)** .

  * Azure Blob Depolama tetikleyici izleyin **Get blob içeriği** tam dosyasını okur ve örtük olarak kullanıldığı Öbekleme eylem.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Bir [Azure depolama hesabı ve depolama kapsayıcısı](../storage/blobs/storage-quickstart-blobs-portal.md)

* Mantıksal uygulama Burada, Azure blob depolama hesabınıza erişmeniz gerekir. Mantıksal uygulamanızı bir Azure Blob Depolama tetikleyici ile başlayın, gerek bir [boş mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="add-trigger"></a>

## <a name="add-blob-storage-trigger"></a>BLOB Depolama tetikleyicisi Ekle

Azure Logic Apps'te, her mantıksal uygulama ile başlamalıdır bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts), belirli bir olay harekete geçirilir gerçekleşen veya belirli bir koşul karşılanıyorsa zaman. Her zaman tetikleyici Logic Apps altyapısı bir mantıksal uygulama örneği oluşturur ve uygulamanızın iş akışı çalışmaya başlar.

Bu örnek, bir mantıksal uygulama iş akışı ile nasıl başlatılacağı gösterir **bir blob eklendiğinde veya değiştirildiğinde (yalnızca Özellikler)** bir blob'un özelliklerini veya eklenen depolama kapsayıcınızda güncelleştirildiğinde tetikleyici.

1. İçinde [Azure portalında](https://portal.azure.com) veya Visual Studio, mantıksal Uygulama Tasarımcısı açılır bir boş mantıksal uygulama oluşturun. Bu örnek, Azure portalını kullanır.

2. Arama kutusuna filtreniz olarak "azure blob" girin. Tetikleyiciler listesinden istediğiniz tetikleyicisini seçin.

   Bu örnek, bu tetikleyici kullanır: **Bir blob eklendiğinde veya değiştirildiğinde (yalnızca Özellikler)**

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

1. İçinde [Azure portalında](https://portal.azure.com) veya Visual Studio, Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın. Bu örnek, Azure portalını kullanır.

2. Mantıksal Uygulama Tasarımcısı, tetikleyici veya eylemi seçin **yeni adım**.

   ![Eylem ekleme](./media/connectors-create-api-azureblobstorage/add-action.png) 

   Var olan adımlar arasında bir eylem eklemek için bağlantı okun üzerine fareyi hareket ettirin. Artı işaretini seçin ( **+** ) görünür ve seçin **Eylem Ekle**.

3. Arama kutusuna filtreniz olarak "azure blob" girin. Eylem listesinden istediğiniz eylemi seçin.

   Bu örnek, bu eylem kullanır: **BLOB içeriğini Al**

   ![Eylem seçin](./media/connectors-create-api-azureblobstorage/azure-blob-action.png)

4. Bağlantı ayrıntıları için istenirse [artık Azure Blob Depolama bağlantınızı oluşturmak](#create-connection).
Veya, bağlantınız zaten varsa, eylem için gerekli bilgileri sağlayın.

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

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
