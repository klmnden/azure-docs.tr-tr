---
title: "İş akışları şablonlardan - Azure Logic Apps oluşturmak | Microsoft Docs"
description: "Kullanmaya başlama - uygulamaları bağlamak ve veri tümleştirmek için Azure mantıksal uygulama şablonları kullanarak iş akışlarını hızla oluşturun."
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 3656acfb-eefd-4e75-b5d2-73da56c424c9
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: LADocs; klam
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 89272869f7dfaa34cbd2ad32d67dca0955e6158b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-workflow-using-a-pre-built-template-or-pattern-to-get-started-quickly"></a>Hızlıca başlamak için önceden derlenmiş şablon veya desen kullanarak bir iş akışı yapılandırma

## <a name="what-are-logic-app-templates"></a>Mantıksal uygulama şablonları nelerdir
Bir mantıksal uygulama şablonu, hızlı bir şekilde kendi iş akışı oluşturmaya başlamak için kullanabileceğiniz önceden oluşturulmuş mantıksal uygulama ' dir. 

Bu şablonlar, logic apps kullanarak yerleşik çeşitli desenlerini bulmak için iyi bir yoldur. Bu şablon olarak da kullanabilirsiniz- ya da bunları senaryonuza uyacak şekilde değiştirin.

## <a name="overview-of-available-templates"></a>Kullanılabilir şablonlarına genel bakış
Şu anda logic app platform yayımlanan birçok kullanılabilir şablonlar vardır. Bazı örnek kategorileri yanı sıra bunların içinde kullanılan bağlayıcılar türünü aşağıda listelenmiştir.

### <a name="enterprise-cloud-templates"></a>Kurumsal bulut şablonlar
Dynamics CRM, Salesforce, kutusu, Azure Blob ve kurumsal bulut gereksinimleriniz için diğer bağlayıcıları tümleştirmek şablonları. Bazı örnekler ne bunlarla yapılabilir şablonları, müşteri adayları düzenleme ve kurumsal dosya verilerinizi yedeklemeye içerir.

### <a name="enterprise-integration-pack-templates"></a>Kurumsal tümleştirme paketi şablonları
VETER yapılandırmalarını (doğrulamak, ayıklama, dönüştürme, zenginleştirmek ve rota) ardışık EDI belge AS2 üzerinde ve XML için dönüştürme olarak yanı sıra X12 ve AS2 iletisi olarak işleme bir X12 alma.

### <a name="protocol-pattern-templates"></a>Protokol düzeni şablonları
İstek-yanıt gibi Protokolü düzenleri tümleştirmeler yanı sıra HTTP FTP ve SFTP arasında içeren mantıksal uygulamalar, bu şablonları oluşur. Bunlar oldukları gibi veya temel olarak daha karmaşık Protokolü desenleri oluşturmak için kullanın.  

### <a name="personal-productivity-templates"></a>Kişisel üretkenlik şablonları
Kişisel üretkenlik geliştirmeye yardımcı olmak için desenleri günlük anımsatıcıları ayarlamak, önemli iş öğeleri Yapılacaklar listelerine kapatma ve tek bir kullanıcı onayı adım kadar uzun görevleri otomatikleştirmek şablonları içerir.

### <a name="consumer-cloud-templates"></a>Tüketici bulut şablonları
Twitter, boşluk ve e-posta, sosyal medya girişimleri pazarlama güçlendirme sonuçta özellikli gibi sosyal medya Hizmetleri ile tümleştirme basit şablonları. Bu da üretkenliklerini geleneksel yinelenen görevler üzerinde harcanan süre kaydederek yardımcı olabilecek cloudy kopyalama gibi şablonları içerir. 

## <a name="how-to-create-a-logic-app-using-a-template"></a>Bir şablonu kullanarak bir mantıksal uygulama oluşturma
Bir mantıksal uygulama şablonu kullanmaya başlamak için mantığı Uygulama Tasarımcısı'na gidin. Varolan bir mantıksal uygulama açarak Tasarımcı girdiğinizden yoksa, mantıksal Uygulama Tasarımcısı görünümünüzde otomatik olarak yükler. Ancak, yeni bir mantıksal uygulama oluşturuyorsanız, aşağıdaki ekran görüntüsüne bakın.  
 ![](../../includes/media/app-service-logic-templates/template7.png)  

Bu ekranda ya da boş mantıksal uygulama ya da önceden derlenmiş şablon ile başlatmak seçebilirsiniz. Şablonlardan birini seçerseniz, ek bilgiler sağlanır. Bu örnekte, kullandığımız *yeni bir dosya Dropbox'oluşturulduğunda, OneDrive kopyalama* şablonu.  
 ![](../../includes/media/app-service-logic-templates/template2.png)  

Şablon kullanmayı seçerseniz, yalnızca seçin *bu şablonu kullanmak* düğmesi. Tabanlı hesaplarınız oturum açmak için şablonu hangi bağlayıcıların yararlanan istenir. Ya da daha önce bu bağlayıcıların bağlantıyla belirlediğinize göre seçebileceğiniz aşağıda görüldüğü gibi devam eder.  
 ![](../../includes/media/app-service-logic-templates/template3.png)  

Bağlantı oluşturma ve seçtikten sonra *devam*, mantıksal Uygulama Tasarımcısı görünümünde açılır.  
 ![](../../includes/media/app-service-logic-templates/template4.png)  

Birçok şablonlarıyla olduğu gibi yukarıdaki örnekte, bazı zorunlu özellik alanları bağlayıcılar içinde doldurulması; Ancak, bazı hala bir değer doğru mantıksal uygulama dağıtmak için gerektirebilir. Eksik alanların bazılarını girmeden dağıtmayı deneyin, hata iletisi ile bildirilir.

Şablon Görüntüleyicisi dönmek istiyorsanız seçin *şablonları* üst gezinti çubuğu düğmesini. Şablon Görüntüleyicisi geçerek, kaydedilmemiş ilerleme kaybedersiniz. Şablon Görüntüleyicisi'ne geçiş öncesinde bu bildiren bir uyarı iletisi görürsünüz.  
 ![](../../includes/media/app-service-logic-templates/template5.png)  

## <a name="how-to-deploy-a-logic-app-created-from-a-template"></a>Şablondan oluşturulan bir mantıksal uygulama dağıtma
Şablonunuzu yüklü ve istediğiniz tüm değişiklikleri yapılan sonra Kaydet seçeneğini sol üst köşedeki düğmesi. Bu kaydeder ve mantıksal uygulamanızı yayımlar.  
 ![](../../includes/media/app-service-logic-templates/template6.png)  

Daha fazla adım var olan bir mantıksal uygulama şablonu içine ekleyin veya genel düzenlemeler hakkında daha fazla bilgi isterseniz, hakkında daha fazla okuma [mantıksal uygulama oluşturma](../logic-apps/logic-apps-create-a-logic-app.md).

