---
title: Hizmet yapılandırması ve profillerini yönetme | Microsoft Docs
description: Hizmet yapılandırmalarını ve profilleri yapılandırma dosyaları ile çalışmayı öğrenin | hangi dağıtım ortamları için ayarları depolar ve bulut Hizmetleri için yayınlama ayarlarını.
services: visual-studio-online
documentationcenter: na
author: ghogen
manager: douge
editor: ''
ms.assetid: 7da8c551-fb06-4057-b5c7-c77f4b39d803
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/11/2017
ms.author: ghogen
ms.openlocfilehash: 411daa8892bee1858c6930dfd8b2b811f164ec5d
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="how-to-manage-service-configurations-and-profiles"></a>Hizmet yapılandırması ve profillerini yönetme
## <a name="overview"></a>Genel Bakış
Bir bulut hizmeti yayımladığınızda, Visual Studio yapılandırma dosyaları iki tür yapılandırma bilgileri depolar: hizmet yapılandırmalarını ve Profiller. Hizmet yapılandırması (.cscfg dosyaları) bir Azure bulut hizmeti için dağıtım ortamları için ayarları depolar. Azure bulut hizmetlerinizi yönetir bu yapılandırma dosyalarını kullanır. Diğer taraftan, profilleri (.azurePubxml dosyaları) deposu bulut Hizmetleri ayarlarını yayımlayın. Bu ayarları Yayımlama Sihirbazı'nı kullanın ve yerel olarak Visual Studio tarafından kullanılan seçtiğiniz öğeye kaydını verilmiştir. Bu konuda, yapılandırma dosyaları her iki türdeki iş açıklanmaktadır.

## <a name="service-configurations"></a>Hizmet yapılandırması
Her dağıtım ortamları için kullanmak üzere birden çok hizmet yapılandırması oluşturabilirsiniz. Örneğin, bir hizmet yapılandırması ve Azure uygulaması ve başka bir hizmet yapılandırması, üretim ortamınız için test çalıştırmak için kullandığınız yerel ortamı oluşturabilirsiniz.

Ekleme, silme, yeniden adlandırın ve gereksinimlerinize göre bu hizmet yapılandırması değiştirme. Aşağıdaki çizimde gösterildiği gibi bu hizmet yapılandırması Visual Studio'dan yönetebilirsiniz.

![Hizmet yapılandırmalarını Yönet](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-service-config.png)

Ayrıca açabilirsiniz **yönetmek yapılandırmaları** rolün özellik sayfaları iletişim kutusu. Bir rol için özellikleri Azure projenizde açmak için bu rol için kısayol menüsünü açın ve ardından **özellikleri**. Üzerinde **ayarları** sekmesinde, genişletin **hizmet yapılandırmasını** listeleyin ve ardından **Yönet** açmak için **yönetmek yapılandırmaları** iletişim kutusu.

### <a name="to-add-a-service-configuration"></a>Bir hizmet yapılandırması eklemek için
1. Çözüm Gezgini'nde Azure projesi için kısayol menüsünü açın ve ardından **yönetmek yapılandırmaları**.
   
    **Hizmet yapılandırmalarını Yönet** iletişim kutusu görüntülenir.
2. Bir hizmet yapılandırması eklemek için varolan bir yapılandırma bir kopyasını oluşturmanız gerekir. Bunu yapmak için ad listesinden kopyalayın ve ardından istediğiniz yapılandırmayı seçin **kopya oluştur**.
3. (İsteğe bağlı) Hizmet yapılandırması farklı bir ad vermek için yeni hizmet yapılandırması adı listesinden seçin ve ardından **yeniden adlandırma**. İçinde **adı** metin kutusuna, bu hizmet yapılandırması kullanın ve ardından istediğiniz adı yazın **Tamam**.
   
    ServiceConfiguration adlı yeni bir hizmet yapılandırma dosyası. [Yeni adı] .cscfg, Çözüm Gezgini'nde Azure projesi eklenir.

### <a name="to-delete-a-service-configuration"></a>Bir hizmet yapılandırması silmek için
1. Çözüm Gezgini'nde Azure projesi için kısayol menüsünü açın ve ardından **yönetmek yapılandırmaları**.
   
    **Hizmet yapılandırmalarını Yönet** iletişim kutusu görüntülenir.
2. Bir hizmet yapılandırması silmek için silmek istediğiniz yapılandırmayı seçin **adı** listeleyin ve ardından **kaldırmak**. Bu yapılandırma silmek istediğiniz doğrulamak için bir iletişim kutusu görüntülenir.
3. **Sil**’i seçin.
   
     Hizmet yapılandırma dosyası, Çözüm Gezgini'nde Azure projeden kaldırılır.

### <a name="to-rename-a-service-configuration"></a>Bir hizmet yapılandırması yeniden adlandırmak için
1. Çözüm Gezgini'nde, Azure projesi için kısayol menüsünü açın ve ardından **yönetmek yapılandırmaları**.
   
    **Hizmet yapılandırmalarını Yönet** iletişim kutusu görüntülenir.
2. Yeni hizmet yapılandırmasından bir hizmet yapılandırması yeniden adlandırmak tercih **adı** listeleyin ve ardından **yeniden adlandırma**. İçinde **adı** metin kutusuna, bu hizmet yapılandırması kullanın ve ardından istediğiniz adı yazın **Tamam**.
   
    Hizmet yapılandırma dosyasının adını Azure projesindeki Çözüm Gezgini'nde değiştirilir.

### <a name="to-change-a-service-configuration"></a>Hizmet yapılandırmasını değiştirmek için
* Hizmet yapılandırmasını değiştirmek istiyorsanız, Azure projesinde değiştirin ve ardından istediğiniz belirli bir rol için kısayol menüsünü açın **özellikleri**. Bkz: [nasıl yapılır: Visual Studio ile Azure bulut hizmeti için rolleri yapılandırmak](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service) daha fazla bilgi için.

## <a name="make-different-setting-combinations-by-using-profiles"></a>Profilleri kullanılarak farklı ayar birleşimleri olun
Bir profil kullanarak otomatik olarak doldurabilirsiniz **Yayımlama Sihirbazı** farklı amaçlar için ayarları farklı bileşimleri ile. Örneğin, hata ayıklama için bir profil olabilir ve yayın için başka bir oluşturur. Bu durumda, **hata ayıklama** profil sahip olabilir **IntelliTrace** etkin ve **hata ayıklama** seçili, yapılandırma ve **sürüm** Profil sahip olabilir **IntelliTrace** devre dışı ve **sürüm** seçili yapılandırma. Farklı profilleri, farklı bir depolama hesabı kullanarak bir hizmet dağıtmak için de kullanabilirsiniz.

Sihirbazı ilk kez çalıştırdığınızda, bir varsayılan profili oluşturulur. Visual Studio profil altında Azure projenize eklenen bir .azurePubXml uzantısına sahip bir dosyada depolar **profilleri** klasör. Daha sonra Sihirbazı'nı çalıştırdığınızda, farklı seçenekler el ile belirtirseniz, dosyayı otomatik olarak güncelleştirir. Aşağıdaki yordamı çalıştırmadan önce önceden bulut hizmetiniz en az bir kez yayımladığınız.

### <a name="to-add-a-profile"></a>Bir profili eklemek için
1. Azure projeniz için kısayol menüsünü açın ve ardından **Yayımla**.
2. Yanına **hedef profil** listesinde **profili Kaydet** düğmesi, aşağıdaki çizimde gösterildiği gibi. Bu profil sizin için oluşturur.
   
    ![Yeni bir profil oluşturma](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/create-new-profile.png)
3. Profil oluşturulduktan sonra seçin **< Yönet... >** içinde **hedef profil** listesi.
   
    **Profillerini yönetme** iletişim kutusu görüntülenirse, aşağıda gösterildiği gibi.
   
    ![Profilleri Yönet iletişim kutusu](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-profiles.png)
4. İçinde **adı** listesinde, bir profil seçin ve ardından **kopya oluştur**.
5. Seçin **Kapat** düğmesi.
   
    Yeni profili hedef profil listesinde görünür.
6. İçinde **hedef profil** listesinde, oluşturduğunuz profil seçin. Yayımlama Sihirbazı ayarları seçtiğiniz profil seçeneklerden doldurulur.
7. Seçin **önceki** ve **sonraki** Yayımlama Sihirbazı'nın her sayfasında görüntülemek ve ardından bu profil ayarlarını özelleştirmek için düğmeler. Bkz: [Azure uygulaması Yayımlama Sihirbazı](http://go.microsoft.com/fwlink/p/?LinkID=623085) bilgi.
8. Ayarları özelleştirme tamamladıktan sonra Seç **sonraki** ayarları sayfasına geri gidin. Profil, bu ayarları kullanarak hizmet yayımladığınızda veya seçerseniz kaydedilir **kaydetmek** profil listesine yanındaki.

### <a name="to-rename-or-delete-a-profile"></a>Yeniden adlandırmak veya bir profilini silmek için
1. Azure projeniz için kısayol menüsünü açın ve ardından **Yayımla**.
2. İçinde **hedef profil** listesinde **Yönet**.
3. İçinde **profillerini yönetme** iletişim kutusu, silmek istediğiniz profili seçin ve ardından **kaldırmak**.
4. Görüntülenen onay iletişim kutusunda seçin **Tamam**.
5. Seçin **Kapat**.

### <a name="to-change-a-profile"></a>Bir profili değiştirmek için
1. Azure projeniz için kısayol menüsünü açın ve ardından **Yayımla**.
2. İçinde **hedef profil** listesinde, değiştirmek istediğiniz profili seçin.
3. Seçin **önceki** ve **sonraki** Yayımlama Sihirbazı her sayfasını görüntülemek ve ardından istediğiniz ayarları değiştirmek için düğmeler. Bkz: [Azure uygulaması Yayımlama Sihirbazı](http://go.microsoft.com/fwlink/p/?LinkID=623085) bilgi.
4. Ayarları değiştirme işlemini tamamladıktan sonra seçin **sonraki** dönmek için **ayarları** sayfası.
5. (İsteğe bağlı) seçin **Yayımla** yeni ayarlar kullanılarak bulut hizmeti yayımlamak için. Şu anda bulut hizmetinizi yayımlayın istemediğiniz ve Yayımlama Sihirbazı kapatırsanız, Visual Studio profiline değişiklikleri kaydetmek isteyip istemediğinizi sorar.

## <a name="next-steps"></a>Sonraki adımlar
Diğer Azure projeniz Visual Studio'dan bölümlerini yapılandırma hakkında bilgi edinmek için [bir Azure projesi yapılandırma](http://go.microsoft.com/fwlink/p/?LinkID=623075)

