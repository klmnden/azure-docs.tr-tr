---
title: "Kodlamak veya kodunu çözmek Azure mantıksal uygulamaları düz dosyalarda | Microsoft Docs"
description: "Logic apps Enterprise tümleştirme paketinde dosya dosya Kodlayıcısı veya kod çözücü kullanma"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 82152dab-c7ad-43df-b721-596559703be8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; mandia
ms.openlocfilehash: 8795687c002282b68ebd1a4fa3fe18a9b102af4a
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="overview-of-enterprise-integration-with-flat-files"></a>Düz dosyalar ile Kurumsal tümleştirme genel bakış

İşletmeden işletmeye (B2B) senaryosunda bir iş ortağına göndermeden önce XML içerik kodlama isteyebilirsiniz. Bir mantıksal uygulama bağlayıcısı kodlama düz dosya, bunu yapmak için kullanabilirsiniz. Oluşturduğunuz mantıksal uygulama kendi XML kaynakları, bir HTTP isteği tetikleyicisi, başka bir uygulama veya çok birinden bile dahil olmak üzere çeşitli içerik alabilir [Bağlayıcılar](../connectors/apis-list.md). Logic apps hakkında daha fazla bilgi için bkz: [logic apps belge](logic-apps-overview.md "Logic apps hakkında daha fazla bilgi").  

## <a name="create-the-flat-file-encoding-connector"></a>Bağlayıcı kodlama düz dosya oluşturma
Mantıksal uygulamanızı bağlayıcıya kodlama düz bir dosya eklemek için aşağıdaki adımları izleyin.

1. Mantıksal uygulama oluşturma ve [tümleştirme hesabınıza bağlamak](logic-apps-enterprise-integration-accounts.md "bir mantıksal uygulama için bir tümleştirme hesabı bağlamak bilgi"). Bu hesap XML verileri kodlamak için kullanacağınız bir şema içeriyor.  
2. Ekleme bir **isteği - olduğunda bir HTTP isteği alındığında** mantıksal uygulamanızı tetikleyiciye.  
   ![Tetikleyici seçmek için ekran görüntüsü](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)    
3. Eylem, aşağıdaki gibi kodlama düz dosya ekleyin:
   
    a. Seçin **artı** oturum.
   
    b. Seçin **Eylem Ekle** bağlantı (artı seçtikten sonra görünür).
   
    c. Arama kutusuna *düz* tüm eylemler için kullanmak istediğiniz bir filtre uygulamak için.
   
    d. Seçin **düz dosya kodlamasını** listesindeki seçeneği.   
   ![Ekran görüntüsü düz dosya kodlamasını seçeneği](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)   
4. Üzerinde **düz dosya kodlamasını** iletişim kutusunda **içerik** metin kutusu.  
   ![İçerik ekran metin kutusu](media/logic-apps-enterprise-integration-flatfile/flatfile-3.png)  
5. Gövde etiketi kodlamak istediğiniz içeriği seçin. Gövde etiketi içerik alanında doldurur.     
   ![Gövde etiketinin ekran görüntüsü](media/logic-apps-enterprise-integration-flatfile/flatfile-4.png)  
6. Seçin **şema adı** liste kutusu ve giriş içeriği kodlamak için kullanmak istediğiniz şema seçin.    
   ![Şema adı ekran liste kutusu](media/logic-apps-enterprise-integration-flatfile/flatfile-5.png)  
7. Çalışmanızı kaydedin.   
   ![Ekran Kaydet simgesi](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)  

Bu noktada, düz dosya kodlama Connector kurulumu tamamlandı. Gerçek dünya uygulamada Salesforce gibi bir iş kolu satır uygulama kodlanmış verileri depolamak isteyebilirsiniz. Veya, bir ticaret için kodlanmış verileri ortak gönderebilirsiniz. Sağlanan diğer bağlayıcıları herhangi birini kullanarak Salesforce veya ticari ortağınızı kodlama eylemin çıkış göndermek için bir eylem kolayca ekleyebilirsiniz.

Artık, HTTP uç noktası için bir istek yapıp istek gövdesinde XML içeriği de dahil olmak üzere Bağlayıcınızı test edebilirsiniz.  

## <a name="create-the-flat-file-decoding-connector"></a>Bağlayıcı kod çözme düz dosya oluşturma

> [!NOTE]
> Bu adımları tamamlamak için tümleştirme dikkate Karşıya zaten bir şema dosyası olması gerekir.

1. Ekleme bir **isteği - olduğunda bir HTTP isteği alındığında** mantıksal uygulamanızı tetikleyiciye.  
   ![Tetikleyici seçmek için ekran görüntüsü](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)    
2. Eylem, aşağıdaki gibi kod çözme düz dosya ekleyin:
   
    a. Seçin **artı** oturum.
   
    b. Seçin **Eylem Ekle** bağlantı (artı seçtikten sonra görünür).
   
    c. Arama kutusuna *düz* tüm eylemler için kullanmak istediğiniz bir filtre uygulamak için.
   
    d. Seçin **düz dosya kod çözme** listesindeki seçeneği.   
   ![Ekran görüntüsü, düz dosya kod çözme seçeneği](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)   
3. Seçin **içerik** denetim. Bu içeriği olarak çözmek için kullanabileceğiniz önceki adımlarda içerikten listesini oluşturur. Dikkat *gövde* gelen HTTP istek içeriği olarak çözecek amacıyla kullanılabilir. Doğrudan kod çözme için içeriği de girebilirsiniz **içerik** denetim.     
4. Seçin *gövde* etiketi. Gövde etiketi konusu artık bildirim **içerik** denetim.
5. İçerik kodunu çözmek için kullanmak istediğiniz şema adını seçin. Aşağıdaki ekran görüntüsü gösterilmektedir *OrderFile* Seçili şema adıdır. Bu şema adı tümleştirme dikkate daha önce yüklenen.
   
   ![İletişim kutusunun ekran görüntüsü, düz dosya kod çözme](media/logic-apps-enterprise-integration-flatfile/flatfile-decode-1.png)    
6. Çalışmanızı kaydedin.  
   ![Ekran Kaydet simgesi](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)    

Bu noktada, düz dosya bağlayıcı kod çözme ayarlama tamamlandı. Gerçek dünya uygulamada Salesforce gibi bir iş kolu satır uygulama kodu çözülmüş verileri depolamak isteyebilirsiniz. Salesforce için kod çözme eylemin çıkış göndermek için bir eylem kolayca ekleyebilirsiniz.

Artık, HTTP uç noktası için istekte ve istek gövdesinde kodunu çözmek istediğiniz XML içeriği de dahil olmak üzere Bağlayıcınızı test edebilirsiniz.  

## <a name="next-steps"></a>Sonraki adımlar
* [Enterprise Integration Pack hakkında daha fazla bilgi](logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin").  

