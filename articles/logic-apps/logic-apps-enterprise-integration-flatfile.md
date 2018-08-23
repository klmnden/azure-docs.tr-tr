---
title: Kodlayın veya Azure logic apps'te düz dosyaları kodlayıp kod çözebilirsiniz | Microsoft Docs
description: Logic apps Kurumsal tümleştirme paketinde dosya Kodlayıcı veya kod çözücü kullanma
services: logic-apps
documentationcenter: .net,nodejs,java
author: divyaswarnkar
manager: jeconnoc
editor: ''
ms.assetid: 82152dab-c7ad-43df-b721-596559703be8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; divswa
ms.openlocfilehash: b13e8da04c984456027f152f5af63cfa6604ddc4
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "42056720"
---
# <a name="overview-of-enterprise-integration-with-flat-files"></a>Düz dosyalar ile Kurumsal tümleştirme genel bakış

İşletmeler arası (B2B) senaryosunda iş ortağına göndermeden önce XML içeriği kodlama isteyebilirsiniz. Bir mantıksal uygulama, bunu yapmak için düz dosya kodlama bağlayıcısını kullanabilirsiniz. Oluşturduğunuz mantıksal uygulama, XML HTTP isteği tetikleyicisi, başka bir uygulama veya daha çok birinden gibi kaynakları, çeşitli içerik alabilir [Bağlayıcılar](../connectors/apis-list.md). Logic apps hakkında daha fazla bilgi için bkz. [logic apps belgelerini](logic-apps-overview.md "Logic apps hakkında daha fazla bilgi edinin").  

## <a name="create-the-flat-file-encoding-connector"></a>Düz dosya kodlama Bağlayıcısı oluşturma
Mantıksal uygulamanızın bağlayıcısına kodlama düz bir dosya eklemek için aşağıdaki adımları izleyin.

1. Mantıksal uygulama oluşturma ve [tümleştirme hesabınıza bağlayın](logic-apps-enterprise-integration-accounts.md "öğrenmek için mantıksal uygulama tümleştirme hesabı bağlamak"). Bu hesap, şemanın XML verileri kodlamak için kullanacağı içerir.  
1. Ekleme bir **isteği - zaman bir HTTP isteği alındığında** mantıksal uygulamanızın tetikleyicisi.  
   ![Tetikleyici seçmek için ekran görüntüsü](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)    
1. Düz dosya eylemi şu şekilde kodlama ekleyin:
   
    a. Seçin **artı** oturum.
   
    b. Seçin **Eylem Ekle** bağlantı (artı seçtikten sonra görünür).
   
    c. Arama kutusuna *düz* tüm eylemler için kullanmak istediğiniz bir filtre uygulamak için.
   
    d. Seçin **düz dosya kodlama** listeden seçeneği.   
   ![Ekran görüntüsü, düz dosya kodlama seçeneği](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)   
1. Üzerinde **düz dosya kodlama** iletişim kutusunda **içerik** metin kutusu.  
   ![Metin kutusuna içerik ekran görüntüsü](media/logic-apps-enterprise-integration-flatfile/flatfile-3.png)  
1. Gövde etiketine kodlamak istediğiniz içeriği seçin. Gövde etiketine içerik alanı doldurur.     
   ![Gövde etiketine ekran görüntüsü](media/logic-apps-enterprise-integration-flatfile/flatfile-4.png)  
1. Seçin **şema adı** liste kutusu ve giriş içeriği kodlamak için kullanmak istediğiniz şema seçin.    
   ![Şema adı ekran liste kutusu](media/logic-apps-enterprise-integration-flatfile/flatfile-5.png)  
1. Çalışmanızı kaydedin.   
   ![Ekran Kaydet simgesi](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)  

Bu noktada, düz dosya kodlama Connector kurulumu tamamlandı. Gerçek bir uygulamada, Salesforce gibi bir iş kolu satır uygulama kodlanmış verileri depolamak isteyebilirsiniz. Veya, bir alım-satım için kodlanmış verileri iş ortağı gönderebilirsiniz. Herhangi biri sağlanan diğer bağlayıcıları kullanarak Salesforce veya ticaret iş ortağı kodlama eylemin çıkış göndermek için bir eylem, bir kolayca ekleyebilirsiniz.

HTTP uç noktasına bir istek yapıp XML içeriği istek gövdesinde dahil olmak üzere Bağlayıcınız artık test edebilirsiniz.  

## <a name="create-the-flat-file-decoding-connector"></a>Düz dosya kodunu çözme Bağlayıcısı oluşturma

> [!NOTE]
> Bu adımları tamamlamak için tümleştirme hesabına zaten karşıya bir şema dosyası olması gerekir.

1. Ekleme bir **isteği - zaman bir HTTP isteği alındığında** mantıksal uygulamanızın tetikleyicisi.  
   ![Tetikleyici seçmek için ekran görüntüsü](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)    
1. Eylem, aşağıdaki gibi kod çözme düz dosya ekleyin:
   
    a. Seçin **artı** oturum.
   
    b. Seçin **Eylem Ekle** bağlantı (artı seçtikten sonra görünür).
   
    c. Arama kutusuna *düz* tüm eylemler için kullanmak istediğiniz bir filtre uygulamak için.
   
    d. Seçin **düz dosya kodu çözme** listeden seçeneği.   
   ![Seçeneği ekran görüntüsü, düz dosya kodu çözme](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)   
1. Seçin **içerik** denetimi. Bu içerik içeriği olarak kodunu çözmek için kullanabileceğiniz önceki adımların listesini oluşturur. Dikkat *gövdesi* gelen HTTP istek içeriği olarak kodunu çözmek için kullanılmak üzere kullanılabilir. Doğrudan kodunu çözmek için içeriği de girebilirsiniz **içeriği** denetimi.     
1. Seçin *gövdesi* etiketi. Gövde etiketini, artık, bildirimi **içerik** denetimi.
1. İçerik kodunu çözmek için kullanmak istediğiniz şema adını seçin. Aşağıdaki ekran görüntüsünde gösterilmektedir *OrderFile* Seçili şema adıdır. Bu şema ad tümleştirme hesabına daha önce yüklenmiş.
   
   ![İletişim kutusunun ekran görüntüsü, düz dosya kodu çözme](media/logic-apps-enterprise-integration-flatfile/flatfile-decode-1.png)    
1. Çalışmanızı kaydedin.  
   ![Ekran Kaydet simgesi](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)    

Bu noktada, düz dosya kodunu çözme bağlayıcı ayarlama tamamlandı. Gerçek bir uygulamada, Salesforce gibi bir iş kolu satır uygulama kodu çözülmüş verileri depolamak isteyebilirsiniz. Salesforce'a kod çözme eylemi, çıkış göndermek için bir eylem, bir kolayca ekleyebilirsiniz.

HTTP uç noktaya istekte ve istek gövdesinde çözmek istediğiniz XML içeriği de dahil olmak üzere Bağlayıcınız artık test edebilirsiniz.  

## <a name="next-steps"></a>Sonraki adımlar
* [Enterprise Integration Pack hakkında daha fazla bilgi](logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin").  

