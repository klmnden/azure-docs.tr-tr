---
title: OneDrive - Azure Logic Apps'i bağlama | Microsoft Docs
description: Karşıya yükleme ve dosyalarını OneDrive REST API'leri ve Azure Logic Apps ile yönetme
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 10/18/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 840a010f8606387a250552d884621a96d0031f90
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62106236"
---
# <a name="get-started-with-the-onedrive-connector"></a>OneDrive Bağlayıcısı ile çalışmaya başlama
Karşıya yükleme dahil olmak üzere, dosyalarınızı yönetmek için Onedrive'a bağlanın, almak, dosyaları ve diğer silin. 

OneDrive ile: 

* Onedrive'da dosyaları depolayarak, iş akışınızı oluşturun veya var olan dosyaları onedrive'daki güncelleştirebilirsiniz. 
* Tetikleyiciler, bir dosya oluşturulduğunda veya OneDrive'ınıza içinde güncelleştirilmiş iş akışınızı başlatmak için kullanın.
* Eylemler bir dosya oluşturun ve bir dosyayı silmek için kullanın. Örneğin, ek (tetikleyici) ile yeni bir Office 365 e-posta geldiğinde (bir eylem) Onedrive'da yeni bir dosya oluşturun.

Bu makalede, bir mantıksal uygulama OneDrive Bağlayıcısı'nı kullanma işlemi gösterilmektedir ve tetikleyiciler ve Eylemler de listeler.

Logic Apps hakkında daha fazla bilgi için bkz: [mantıksal uygulamalar nedir](../logic-apps/logic-apps-overview.md) ve [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-onedrive"></a>Onedrive'a bağlanma
Mantıksal uygulamanızı herhangi bir hizmete erişebilmeniz için önce ilk oluşturmak bir *bağlantı* hizmeti. Bir bağlantı, bir mantıksal uygulama ile başka bir hizmet arasında bağlantı sağlar. Örneğin, Onedrive'a bağlanmak için önce bir OneDrive ihtiyacınız *bağlantı*. Bir bağlantı oluşturmak için normalde bağlanmak istediğiniz hizmete erişmek için kullandığınız kimlik bilgilerini girin. Bu nedenle, OneDrive ile bağlantı oluşturmak için OneDrive hesabınızda kimlik bilgilerini girin.

### <a name="create-the-connection"></a>Bağlantı oluşturma
> [!INCLUDE [Steps to create a connection to OneDrive](../../includes/connectors-create-api-onedrive.md)]
> 
> 

## <a name="use-a-trigger"></a>Tetikleyici kullanma
Bir tetikleyici bir mantıksal uygulamada tanımlanan iş akışını başlatmak için kullanılan bir olaydır. Tetikleyiciler "hizmet bir aralığı ve istediğiniz sıklığı yoklama". [Tetikleyiciler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Mantıksal uygulamada, Tetikleyiciler, bir listesini almak için "onedrive" yazın:  
   
    ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Seçin **bir dosya değiştirildiğinde**. Ardından bir bağlantı zaten varsa, bir klasör seçmek için seçiciyi Göster düğmesini seçin.
   
    ![](./media/connectors-create-api-onedrive/sample-folder.png)
   
    Oturum açmanız istenirse, oturum bağlantısı oluşturmak için ayrıntıları girin. [Bağlantı oluşturma](connectors-create-api-onedrive.md#create-the-connection) adımlar bu makalede listelenmektedir. 
   
   > [!NOTE]
   > Bu örnekte, mantıksal uygulama güncelleştirilir seçtiğiniz klasöre bir dosya çalıştırır. Bu tetikleyiciyi sonuçlarını görmek için bir e-posta gönderen başka bir eylem ekleyin. Örneğin, Office 365 Outlook ekleyin *bir e-posta* bir dosya güncelleştirildiğinde size e-posta eylem. 

3. Seçin **Düzenle** ayarlayın ve düğme **sıklığı** ve **aralığı** değerleri. Örneğin, her 15 dakikada yoklamak için tetikleyici istiyorsanız ayarlayın **sıklığı** için **dakika**, ayarlayıp **aralığı** için **15**. 
   
    ![](./media/connectors-create-api-onedrive/trigger-properties.png)
4. **Kaydet** değişikliklerinizi (sol üst köşesindeki araç). Mantıksal uygulamanızı kaydedilir ve otomatik olarak etkinleştirilir.

## <a name="use-an-action"></a>Bir eylem kullanın
Bir eylem, bir mantıksal uygulamada tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir. [Eylemler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Artı işaretini seçin. Birkaç seçenek görürsünüz: **Eylem Ekle**, **koşul Ekle**, veya biri **daha fazla** seçenekleri.
   
    ![](./media/connectors-create-api-onedrive/add-action.png)
2. Seçin **Eylem Ekle**.
3. Metin kutusuna, kullanılabilir tüm eylemlerin bir listesini almak için "onedrive" yazın.
   
    ![](./media/connectors-create-api-onedrive/onedrive-actions.png) 
4. Bizim örneğimizde seçin **OneDrive - dosya oluşturma**. Bir bağlantı zaten varsa, ardından **klasör yolu** dosyanın yerleştirileceği girin **dosya adı**ve **dosya içeriği** istediğiniz:  
   
    ![](./media/connectors-create-api-onedrive/sample-action.png)
   
    Ardından bağlantı bilgileri istenirse, bağlantı oluşturmak için ayrıntıları girin. [Bağlantı oluşturma](connectors-create-api-onedrive.md#create-the-connection) bu makalede bu özellikleri açıklar. 
   
   > [!NOTE]
   > Bu örnekte, bir OneDrive klasörüne yeni bir dosya oluştururuz. Çıkış başka bir tetikleyici, OneDrive dosyası oluşturmak için kullanabilirsiniz. Örneğin, Office 365 Outlook ekleyin *yeni bir e-posta geldiğinde* tetikleyici. OneDrive'ı ekleme *dosya oluştur* ekler ve Content-Type kullanan eylemi Onedrive'da yeni dosya oluşturmak için bir ForEach içindeki alanları. 
   > 
   > ![](./media/connectors-create-api-onedrive/foreach-action.png)

5. **Kaydet** değişikliklerinizi (sol üst köşesindeki araç). Mantıksal uygulamanızı kaydedilir ve otomatik olarak etkinleştirilir.


## <a name="connector-specific-details"></a>Özel bağlayıcı ayrıntıları

Tetikleyiciler ve eylemlerin swagger'da tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınırlama Bkz [bağlayıcı ayrıntıları](/connectors/onedriveconnector/).

## <a name="more-connectors"></a>Daha fazla bağlayıcı
Geri Git [API listesi](apis-list.md).