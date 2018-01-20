---
title: "Mantıksal uygulamalarınızı Azure blob depolama bağlayıcı ekleme | Microsoft Docs"
description: "Başlama ve bir mantıksal uygulama Azure blob depolama Bağlayıcısı'nı yapılandırma"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b5dc3f75-6bea-420b-b250-183668d2848d
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/02/2017
ms.author: mandia; ladocs
ms.openlocfilehash: e12669abd41f09d161fab786af29955da54a1633
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="use-the-azure-blob-storage-connector-in-a-logic-app"></a>Bir mantıksal uygulama Azure blob depolama Bağlayıcısı'nı kullanın
Karşıya yükleme, güncelleştirme, almak ve depolama hesabınızda, tüm mantıksal uygulama içinde BLOB'ları silme için Azure Blob Depolama Bağlayıcısı'nı kullanın.  

Azure blob storage ile:

* İş akışınızı yeni projeler karşıya yükleme veya en son güncelleştirilen dosyaları alma oluşturun.
* Eylemler dosya meta verilerini almak için bir dosya, dosyaları kopyala ve daha fazlasını silmek için kullanın. Örneğin, bir aracı bir Azure web sitesinde (bir tetikleyici) güncelleştirildiğinde, ardından bir blob depolama (bir eylem) dosyasında güncelleştirin. 

Bu konuda bir mantıksal uygulama blob depolama Bağlayıcısı'nı kullanmayı gösterir.

Logic Apps hakkında daha fazla bilgi için bkz: [logic apps nedir](../logic-apps/logic-apps-overview.md) ve [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-azure-blob-storage"></a>Azure blob depolama alanına bağlanma
Mantıksal uygulamanızı herhangi bir hizmete erişebilmesi için önce oluşturduğunuz bir *bağlantı* hizmet. Bağlantı bir mantıksal uygulama ile başka bir hizmet arasında bağlantı sağlar. Örneğin, bir depolama hesabına bağlanmak için önce bir blob depolama oluşturmanız *bağlantı*. Bir bağlantı oluşturmak için normalde bağlandığınız hizmete erişmek için kullandığınız kimlik bilgilerini girin. Bu nedenle Azure storage ile bağlantı oluşturmak için depolama hesabınıza kimlik bilgilerini girin. 

#### <a name="create-the-connection"></a>Bağlantı oluşturma
> [!INCLUDE [Create a connection to Azure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]

## <a name="use-a-trigger"></a>Bir tetikleyici kullanın
Bu bağlayıcı hiçbir tetikleyici yok. Bir yineleme tetikleyici, bir HTTP Web kancası tetikleyici, tetikleyici diğer bağlayıcıları ve daha fazla ile kullanılabilir gibi mantıksal uygulamayı başlatmak için diğer Tetikleyicileri kullanın. [Mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) bir örnek sağlar.

## <a name="use-an-action"></a>Bir eylem kullanın
Bir eylem, bir mantıksal uygulama içinde tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir.

1. Artı işaretini seçin. Birkaç seçeneğiniz bkz: **Eylem Ekle**, **bir koşul eklemek**, veya biri **daha fazla** seçenekleri.
   
    ![](./media/connectors-create-api-azureblobstorage/add-action.png)
2. Seçin **Eylem Ekle**.
3. Metin kutusuna, kullanılabilir tüm eylemlerin bir listesini almak için "blob" yazın.
   
    ![](./media/connectors-create-api-azureblobstorage/actions.png) 
4. Bizim örneğimizde seçin **AzureBlob - Get dosya meta veri yolu kullanarak**. Bir bağlantı zaten varsa, ardından **...** Bir dosya (Göster Seçici) düğmesine tıklayarak seçin.
   
    ![](./media/connectors-create-api-azureblobstorage/sample-file.png)
   
    Bağlantı bilgilerini istenirse, bağlantı oluşturmak için ayrıntılarını girin. [Bağlantı oluşturmak](connectors-create-api-azureblobstorage.md#create-the-connection) bu konuda bu özellikleri açıklar. 
   
   > [!NOTE]
   > Bu örnekte, bir dosya meta verileri alın. Meta verileri görmek için başka bir bağlayıcı kullanarak yeni bir dosya oluşturur başka bir eylem ekleyin. Örneğin, meta verileri temel alarak yeni bir "test" dosyası oluşturur OneDrive eylemi ekleyin. 


5. **Kaydet** değişikliklerinizi (sol üst köşesindeki araç). Mantıksal uygulamanızı kaydedilir ve otomatik olarak etkinleştirilir.

> [!TIP]
> [Depolama Gezgini](http://storageexplorer.com/) birden çok depolama hesaplarını yönetmek için harika bir araçtır.

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/azureblobconnector/). 

## <a name="next-steps"></a>Sonraki adımlar
[Mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Logic Apps diğer kullanılabilir bağlayıcılar keşfedin bizim [API'leri listesi](apis-list.md).

