---
title: "Visual Studio'da bağlı hizmetler kullanarak Mobile Services ekleme | Microsoft Docs"
description: "Visual Studio bağlı Hizmetleri Ekle iletişim kutusunu kullanarak mobil hizmetler ekleme"
services: visual-studio-online
documentationcenter: na
author: mlhoop
manager: douge
editor: 
ms.assetid: 75c3cb93-88e1-476d-a416-f34caa3608e3
ms.service: visual-studio-online
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: mobile
ms.date: 12/16/2015
ms.author: mlearned
ms.openlocfilehash: d185fdafebad56f8970e390b2a0672c3fb84df8f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="adding-mobile-services-by-using-visual-studio-connected-services"></a>Visual Studio bağlı Hizmetleri kullanılarak Mobile Services ekleme
Visual Studio 2015 ile Azure Mobile Services kullanmaya bağlanabilir **bağlı hizmet Ekle** iletişim. Tüm C# istemci uygulamaları, tüm JavaScript uygulaması veya platformlar arası Cordova uygulaması bağlanabilir. Bağlandıktan sonra oluşturmak ve verilere erişmek, özel API'leri ve zamanlanmış işler oluşturmak veya anında iletme bildirimleri için destek eklemek.  Bağlı hizmetler işlemi, tüm uygun başvuruları ve bağlantı kodu ekler. Ayrıca Azure AD gibi popüler kimlik düzenleri, çeşitli kimlik doğrulaması için yerleşik destek yararlanabilirsiniz Facebook, Twitter ve Microsoft Accounts.

## <a name="supported-project-types"></a>Desteklenen proje türleri
> [!NOTE]
> Visual Studio 2015'te bağlı Hizmetleri Ekle iletişim kutusunu kullanarak bir Windows Evrensel (Windows 10) projeleri için Azure Mobile Services ekleme desteklenmiyor. Projeniz için NuGet Paket Yöneticisi'ni kullanarak uygun paketlerini yükleyerek Azure Mobile Services ekleyebilirsiniz.
> 
> 

Aşağıdaki proje türleri için Azure Mobile Services bağlanmak için bağlantılı Hizmetler iletişim kutusunu kullanabilirsiniz.

* .NET Windows 8.1 mağazası, telefon ve evrensel uygulama projeleri
* JavaScript Windows 8.1 mağazası, telefon ve evrensel uygulama projeleri
* Apache Cordova için Visual Studio Araçları kullanılarak oluşturulan projeleri

## <a name="connect-to-azure-mobile-services-using-the-add-connected-services-dialog"></a>Azure Mobile Services bağlı Hizmetleri Ekle iletişim kutusunu kullanarak bağlan
1. Bir Azure hesabı olduğundan emin olun. Azure hesabınız yoksa [ücretsiz denemeye](http://go.microsoft.com/fwlink/?LinkId=518146) kaydolabilirsiniz.
2. Açık **bağlı Hizmetleri Ekle** iletişim kutusu.
   
   * .NET uygulamaları için projenizi Visual Studio'da açın, bağlam menüsünü açın **başvuruları** Çözüm Gezgini'nde, düğümünü ve ardından **bağlı hizmet Ekle**
     
        ![Azure mobil hizmetine bağlanılıyor](./media/vs-azure-tools-connected-services-add-mobile-services/IC797635.png)
   * Apache Cordova uygulaması projeleri için projenizi Visual Studio'da açın, Çözüm Gezgini'nde proje düğümüne bağlam menüsünü açın ve ardından **bağlı hizmet Ekle**.
3. İçinde **bağlı hizmet Ekle** iletişim kutusunda, seçin **Azure Mobile Services**ve ardından **yapılandırma** düğmesi. Önceden yapmadıysanız Azure oturum istenebilir.
   
    ![Bir Azure mobil hizmeti ekleme](./media/vs-azure-tools-connected-services-add-mobile-services/IC797636.png)
4. İçinde **Azure Mobile Services** iletişim kutusunda, yoksa mevcut bir mobil hizmeti seçin. Yeni bir Azure mobil hizmeti oluşturmanız gerekiyorsa, bunu yapmak için aşağıdaki yordamı izleyin. Aksi halde sonraki adıma geçin.
   
    Yeni bir mobil hizmet hesabı oluşturmak için:
   
   1. seçin ** hizmet oluşturma ** iletişim kutusunun altındaki bağlantıyı.
       ![Yeni mobil bağlı hizmet ekleme](./media/vs-azure-tools-connected-services-add-mobile-services/IC797637.png)
   2. Üzerinde **mobil hizmet Oluştur** iletişim kutusu, bir JavaScript arka uç mobil hizmet ya da bir .NET arka uç mobil hizmetinden seçebilirsiniz **çalışma zamanı** aşağı açılan liste. 
      
       ![Bir mobil hizmet oluşturma](./media/vs-azure-tools-connected-services-add-mobile-services/IC797638.png)
      
       Basit ve güçlü bir JavaScript arka uç hizmeti. Bir JavaScript arka uç mobil hizmet oluşturmak, sunucu tarafı JavaScript kodu bulutta saklanır, ancak Sunucu Gezgini ya da Azure Yönetim Portalı'nı kullanarak sunucu komut dosyalarını düzenleyebilirsiniz. 
      
       Bir .NET arka uç mobil hizmet gücünden ve Entity Framework ile Web API ve esneklik sağlar. Bir .NET arka uç mobil hizmet oluşturursanız, proje sizin için oluşturulur ve çözümünüze eklenir. 
   3. Seçin **bölge** Burada, mobil hizmet istediğiniz ve ardından sunucu için bir kullanıcı adı ve parola girin.
   4. Gerekli tüm bilgileri girdikten sonra Seç **oluşturma** mobil hizmet oluşturmak için düğmesi.
   5. Yeni mobil hizmet üzerinde hizmet listesinde görüntülenmelidir **Azure Mobile Services** iletişim kutusu. Listede yeni mobil hizmeti seçin ve ardından **Ekle** düğmesi hizmet projenize ekleyin.
5. Görünür alma başlangıç sayfasını inceleyin ve projenizin nasıl değiştirildiği bulun. Bağlı hizmet eklediğinizde Başlarken sayfa tarayıcınızda görüntülenir. Önerilen sonraki adımlar ve kod örnekleri gözden geçirin veya hangi başvuruları projenize eklendi ve kod ve yapılandırma dosyalarınızı nasıl değiştirildi görmek için ne sayfasına geçin.
6. Kod örnekleri bir kılavuz olarak kullanarak, mobil hizmetinize erişmek için kod yazmaya başlayın!

## <a name="how-your-project-is-modified"></a>Projenizi nasıl değiştirilir
Visual Studio, projenizin nasıl değiştirdiğini proje türüne göre değişir. C# istemci uygulamaları için bkz: [ne – C# projeleri](http://go.microsoft.com/fwlink/p/?LinkId=513119). JavaScript istemci uygulamaları için bkz: [ne – JavaScript projeleri](http://go.microsoft.com/fwlink/p/?LinkId=513120). Cordova uygulamaları için bkz: [ne – Cordova projeleri](http://go.microsoft.com/fwlink/p/?LinkId=513116).

## <a name="next-steps"></a>Sonraki adımlar
Soru sorun ve Yardım alın: 

* [MSDN forumu: Azure mobil hizmetler](https://social.msdn.microsoft.com/forums/azure/home?forum=azuremobile)
* [Microsoft Azure ekibi blogu Azure mobil hizmetler](https://azure.microsoft.com/blog/topics/mobile/)
* [Azure.microsoft.com adresindeki Azure mobil hizmetler](https://azure.microsoft.com/services/mobile-services/)
* [Azure.microsoft.com adresindeki Azure Mobile Services belgeleri](https://azure.microsoft.com/documentation/services/mobile-services/)

