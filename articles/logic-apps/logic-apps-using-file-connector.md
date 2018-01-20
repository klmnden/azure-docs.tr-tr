---
title: "Şirket içi - Azure Logic Apps dosya sistemlerine | Microsoft Docs"
description: "Şirket içi veri ağ geçidi ve dosya sistemi bağlayıcısı aracılığıyla mantığı uygulama iş akışları için şirket içi dosya sistemleri bağlayın"
keywords: "Şirket içi dosya sistemleri"
services: logic-apps
author: derek1ee
manager: anneta
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2017
ms.author: LADocs; deli
ms.openlocfilehash: 32ab5be41a8dee3b1f2c0b1bde076c0d1a844bdd
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="connect-to-on-premises-file-systems-from-logic-apps-with-the-file-system-connector"></a>Logic apps dosya sistemi bağlayıcı ile şirket içi dosya sistemleri bağlayın

Verileri yönetmek ve güvenli bir şekilde şirket içi kaynaklara erişmek için mantıksal uygulamalarınızı şirket içi veri ağ geçidi kullanabilirsiniz. Bu makalede bu temel örnek senaryosu aracılığıyla şirket içi dosya sisteminde nasıl bağlanabileceği gösterilmektedir: Dropbox'a dosya paylaşımına yüklendikten sonra bir e-posta Gönder dosyasını kopyalayın.

## <a name="prerequisites"></a>Önkoşullar

* En son karşıdan [şirket içi veri ağ geçidi](https://www.microsoft.com/download/details.aspx?id=53127).

* Yükleyin ve en son şirket içi veri ağ geçidi, sürüm 1.15.6150.1 yukarı veya yukarıdaki ayarlayın. Adımları için bkz: [şirket içi veri kaynaklarına bağlanma](http://aka.ms/logicapps-gateway). Bu adımlara devam etmeden önce bir şirket içi makinede ağ geçidi yüklemeniz gerekir.

* Hakkındaki temel bilgileri [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

## <a name="add-trigger-and-actions-for-connecting-to-your-file-system"></a>Tetikleyici ve dosya sistemine bağlanmak için Eylemler ekleme

1. Boş bir mantıksal uygulama oluşturma. Bu tetikleyici ilk adım olarak Ekle: **bir dosya oluşturulduğunda, Dropbox -** 

2. Tetikleyici altında seçin **+ sonraki adım** > **Eylem Ekle**. 

3. Arama kutusuna "dosya sistemi", filtre olarak girin. Dosya sistemi bağlayıcı yönelik tüm eylemler gördüğünüzde seçin **dosya sistemi - dosyası oluşturma** eylem. 

   ![Dosya bağlayıcı arayın](media/logic-apps-using-file-connector/search-file-connector.png)

4. Dosya sisteminizi bir bağlantı zaten yoksa, bir bağlantı oluşturmanız istenir. 

5. Seçin **Connect şirket içi veri ağ geçidi üzerinden**. Bağlantı özellikleri görüntülendiğinde, tabloda belirtildiği gibi bir bağlantı ayarlayın.

   ![Bağlantıyı yapılandır](media/logic-apps-using-file-connector/create-file.png)

   | Ayar | Açıklama |
   | ------- | ----------- |
   | **Kök klasör** | Dosya sistemi için kök klasör belirtin. Bir yerel klasör, şirket içi veri ağ geçidi yüklendi veya klasörü makinenin erişebildiği bir ağ paylaşımı olabilir makinede belirtebilirsiniz. <p>**İpucu:** tüm dosya ile ilgili eylemler için göreli yollar için kullanılan ana üst klasör kök klasörüdür. | 
   | **Kimlik doğrulama türü** | Dosya sistemi tarafından kullanılan kimlik doğrulama türü | 
   | **Kullanıcı Adı** | Kullanıcı adınızı sağlamak {*etki alanı*\\*kullanıcıadı*} önceden yüklenmiş ağ geçidiniz için. | 
   | **Parola** | Önceden yüklenmiş ağ geçidiniz için parolanızı girin. | 
   | **Ağ geçidi** | Daha önce yüklenen ağ geçidi seçin. | 
   ||| 

6. Tüm bağlantı ayrıntıları verdikten sonra Seç **oluşturma**. 

   Logic Apps yapılandırır ve bağlantı düzgün çalıştığından emin olmayı bağlantınızı test eder. 
   Bağlantısı düzgün şekilde ayarlanıp ayarlanmadığını seçenekler için daha önce seçtiğiniz eylem görüntülenir. 
   Dosya sistemi Bağlayıcısı'nı şimdi kullanıma hazırdır.

7. Ayarlanan **dosyası oluştur** şirket içi dosya paylaşımı için dosyaları Dropbox'tan kök klasörüne kopyalamak için eylem.

   ![Dosya eylem oluşturun](media/logic-apps-using-file-connector/create-file-filled.png)

8. Dosya kopyalamak için bu eylemden sonra bir e-posta gönderir ve böylece ilgili kullanıcılar yeni dosya hakkında bilmeniz Outlook eylem ekleyin. Alıcılar, başlık ve e-posta gövdesini girin. 

   İçinde **dinamik içerik** listesinde seçebilirsiniz veri çıkışları dosya Bağlayıcısı'ndan daha fazla ayrıntı için e-posta ekleyebilmek için.

   ![E-posta eylemi Gönder](media/logic-apps-using-file-connector/send-email.png)

9. Mantıksal uygulamanızı kaydedin. Dropbox için bir dosyayı karşıya yükleyerek uygulamanızı test edin. Dosya şirket içi dosya paylaşımına kopyalanmış ve işlemi hakkında bir e-posta almanız gerekir.

Tebrikler, artık, şirket içi dosya sistemine bağlanmak bir çalışma mantıksal uygulama vardır. 

Bağlayıcı sunar, örneğin diğer işlevleri keşfetme deneyin:

- Dosya oluştur
- Klasördeki dosyaları listele
- Dosya ekle
- Dosyayı sil
- Dosya içeriğini al
- Dosya yolunu kullanarak dosya içeriğini al
- Dosya meta verilerini al
- Dosya yolunu kullanarak dosya meta verilerini al
- Kök klasördeki dosyaları listele
- Dosyayı güncelleştir

## <a name="view-the-swagger"></a>Swagger görüntüleyin

Bkz: [ayrıntıları swagger](/connectors/fileconnector/). 

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.

* Azure mantıksal uygulamaları ve bağlayıcıların geliştirmeye yardımcı olmak için oy veya fikir gönderme [Azure Logic Apps kullanıcı sesi site](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Sonraki adımlar

* [Şirket içi verilere bağlanma](../logic-apps/logic-apps-gateway-connection.md) 
* [Mantıksal uygulamalarınızı izleyin](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [B2B senaryolarını için Kurumsal tümleştirme](../logic-apps/logic-apps-enterprise-integration-overview.md)
