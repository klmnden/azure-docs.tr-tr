---
title: Azure blob depolama alanına - Azure Logic Apps bağlanma | Microsoft Docs
description: Oluşturma ve Azure Logic Apps ile Azure depolama alanında BLOB'ları yönetme
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 05/21/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 49d08135dee4568d1a9d65ec2d22d17ee3bda2ea
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294688"
---
# <a name="create-and-manage-blobs-in-azure-blob-storage-with-azure-logic-apps"></a>Oluşturma ve Azure Logic Apps ile Azure blob depolama alanındaki BLOB'ları yönetme

Bu makalede nasıl erişmek ve Azure Blob Storage Bağlayıcısı ile bir mantıksal uygulama içinde Azure depolama hesabınızdaki BLOB'lar olarak depolanan dosyalar yönetmek gösterilmektedir. Böylece, görevler ve dosyalarınızı yönetmek için iş akışlarını otomatikleştirmek mantıksal uygulamalar oluşturabilirsiniz. Örneğin, oluşturma, alma, güncelleştirme ve depolama hesabındaki dosyaları silin mantıksal uygulamalar oluşturabilirsiniz.

Bir Azure web sitesinde güncelleştirilir bir aracı olduğunu varsayalım. mantıksal uygulamanız için tetikleyici olarak görev yapan. Bu olay gerçekleştiğinde, mantıksal uygulamanızı bir eylem, blob depolama kapsayıcısını bazı dosyasında güncelleştirme mantıksal uygulamanızı olabilir. 

Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. Logic apps yeniyseniz, gözden [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: ilk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Bağlayıcı özgü teknik bilgi için bkz: <a href="https://docs.microsoft.com/connectors/azureblobconnector/" target="blank">Azure Blob Storage bağlayıcı başvuru</a>.

## <a name="prerequisites"></a>Önkoşullar

* Bir [Azure depolama hesabı ve depolama kapsayıcısı](../storage/blobs/storage-quickstart-blobs-portal.md)

* Burada, Azure blob depolama hesabınıza erişmeniz mantıksal uygulama. Bir Azure Blob Storage tetikleyicisi ile mantıksal uygulamanızı başlatmak için gereken bir [boş mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

<a name="add-trigger"></a>

## <a name="add-blob-storage-trigger"></a>BLOB Depolama tetikleyicisi ekleyin

Azure Logic Apps içinde her mantıksal uygulama başlamalı ve bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts), belirli bir olay olduğunda etkinleşir gerçekleşen veya belirli bir koşul karşılanıyorsa zaman. Her tetikleyici ateşlenir Logic Apps altyapısı bir mantıksal uygulama örneği oluşturur ve uygulamanızın iş akışı çalışmaya başlar.

Bu örnek, bir mantıksal uygulama iş akışı ile nasıl başlatabilirsiniz gösterir **blob eklenir veya (yalnızca özellikleri) değiştirildiğinde Azure Blob Storage -** bir blob'un özelliklerini veya eklenen depolama kapsayıcısında güncelleştirildiğinde tetikleyici. 

1. Azure portal ya da Visual Studio mantığı Uygulama Tasarımcısı'nı açar boş mantıksal uygulama oluşturun. Bu örnek, Azure portalını kullanır.

2. Arama kutusuna "azure blob", filtre olarak girin. Tetikleyiciler listeden istediğiniz Tetikleyici seçin.

   Bu örnek, bu tetikleyici kullanır: **blob eklenir veya (yalnızca özellikleri) değiştirildiğinde Azure Blob Storage -**

   ![Tetikleyici seçin](./media/connectors-create-api-azureblobstorage/azure-blob-trigger.png)

3. Bağlantı ayrıntılarını istenirse [blob depolama bağlantınızı şimdi oluşturmak](#create-connection). Ya da bağlantı zaten varsa, tetikleyici için gerekli bilgileri sağlayın.

   Bu örnekte, kapsayıcı ve izlemek istediğiniz klasörü seçin.

   1. İçinde **kapsayıcı** kutusunda, klasör simgesini seçin.

   2. Klasör listesinde sağ açılı ayraç seçin ( **>** ) ve ardından bulmak istediğiniz klasörü seçin kadar göz atın. 

      ![klasörü seçin](./media/connectors-create-api-azureblobstorage/trigger-select-folder.png)

   3. Aralığı ve klasör değişiklikleri denetlemek için tetikleyici hangi sıklıkta güncelleştirileceğini sıklığını seçin.

4. Tasarımcı araç çubuğunda bitirdiniz seçin **kaydetmek**.

5. Şimdi mantıksal uygulamanızı tetikleyici sonuçlarıyla gerçekleştirmek istediğiniz görevlerle ilgili bir veya daha fazla eylem ekleme devam edin.

<a name="add-action"></a>

## <a name="add-blob-storage-action"></a>BLOB Depolama Eylem Ekle

Azure mantıksal uygulamaları içinde bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) tetikleyicinin veya başka bir eylem izler, iş akışınızı bir adımdır. Bu örnekte, mantıksal uygulama ile başlayan [yineleme tetikleyici](../connectors/connectors-native-recurrence.md).

1. Azure portal ya da Visual Studio mantığı Uygulama Tasarımcısı'nda mantıksal uygulamanızı açın. Bu örnek, Azure portalını kullanır.

2. Tetikleyici veya eylemi altında mantığı Uygulama Tasarımcısı'nda seçin **yeni adım** > **Eylem Ekle**.

   ![Eylem ekleme](./media/connectors-create-api-azureblobstorage/add-action.png) 

   Varolan adımlar arasındaki bir eylem eklemek amacıyla bağlanan oku fareyi hareket ettirin. 
   Artı işaretini seçin (**+**) görünür ve ardından **Eylem Ekle**.

3. Arama kutusuna "azure blob", filtre olarak girin. Eylemler listesinden istediğiniz eylemi seçin.

   Bu eylem Bu örnek kullanır: **Azure Blob Storage - Get blob içeriğinin**

   ![Bir eylem seçin](./media/connectors-create-api-azureblobstorage/azure-blob-action.png) 

4. Bağlantı ayrıntılarını istenirse [artık Azure Blob Storage bağlantınızı oluşturmak](#create-connection). Ya da bağlantı zaten varsa, eylem için gerekli bilgileri sağlayın. 

   Bu örnek için istediğiniz dosyayı seçin.

   1. Gelen **Blob** kutusunda, klasör simgesini seçin.
  
      ![klasörü seçin](./media/connectors-create-api-azureblobstorage/action-select-folder.png)

   2. Bulma ve blob'un üzerinde tabanlı istediğiniz dosyayı seçin **kimliği** numarası. Bu bulma **kimliği** daha önce açıklanan blob depolama tetik tarafından döndürülen blob'un meta verilerde sayı.

5. Tasarımcı araç çubuğunda bitirdiniz seçin **kaydetmek**.
Mantıksal uygulamanızı test etmek için seçilen klasör bir blob içerdiğinden emin olun.

Bu örnek yalnızca bir blob içeriği alır. İçeriği görüntülemek için başka bir bağlayıcı kullanarak blob ile bir dosya oluşturur başka bir eylem ekleyin. Örneğin, blob içeriklerini temel alarak bir dosya oluşturur OneDrive eylemi ekleyin.

<a name="create-connection"></a>

## <a name="connect-to-storage-account"></a>Depolama hesabına bağlanma

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

[!INCLUDE [Create a connection to Azure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcı'nın Swagger dosyası tarafından açıklandığı gibi tetikleyiciler, Eylemler ve sınırları, gibi teknik ayrıntılar için bkz [bağlayıcı başvuru sayfası](/connectors/azureblobconnector/). 

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](http://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcılar](../connectors/apis-list.md)
