---
title: Visual Studio kullanarak Azure Uygulama Sihirbazı yayımlama | Microsoft Docs
description: Visual Studio Azure uygulaması Yayımlama Sihirbazı çeşitli ayarları yapılandırma konusunda bilgi edinin
services: visual-studio-online
documentationcenter: na
author: ghogen
manager: douge
editor: ''
ms.assetid: 7d8f1ac9-e439-47e0-a183-0642c4ea1920
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2017
ms.author: ghogen
ms.openlocfilehash: 980809bc62f7766971ea4753e1cfb165aa1cffc2
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="using-the-visual-studio-publish-azure-application-wizard"></a>Visual Studio kullanarak yayımla Azure Uygulama Sihirbazı

Visual Studio'da bir web uygulaması geliştirme sonra kullanarak bu uygulamaya Azure bulut hizmeti yayımlayabilirsiniz **Azure uygulamasını Yayımla** Sihirbazı.

> [!Note]
> Bu makalede, web siteleri için bulut Hizmetleri dağıtma hakkında ' dir. Web sitelerine dağıtma hakkında daha fazla bilgi için bkz: [bir Azure Web sitesinin nasıl dağıtılacağı](https://social.msdn.microsoft.com/Search/windowsazure?query=How%20to%20Deploy%20an%20Azure%20Web%20Site&Refinement=138&ac=4#refinementChanges=117&pageNumber=1&showMore=false).

## <a name="accessing-the-publish-azure-application-wizard"></a>Azure uygulaması Yayımlama Sihirbazı'na erişim

Sahip olduğunuz Visual Studio projesi türüne bağlı olarak iki yolla Azure uygulamasını Yayımla Sihirbazı'nı erişebilir.

**Bir Azure bulut hizmeti projesi varsa:**

1. Oluşturun veya bir Azure bulut hizmeti projesini Visual Studio'da açın.

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve bağlam menüsünden seçin **Yayımla**.

**Azure için etkinleştirilmemiş bir web uygulaması projesi varsa:**

1. Oluşturun veya bir Azure bulut hizmeti projesini Visual Studio'da açın.

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve bağlam menüsünden seçin **Dönüştür** > **Azure bulut hizmeti projesine dönüştürme**. 

1. İçinde **Çözüm Gezgini**, yeni oluşturulan Azure projesine sağ tıklayın ve bağlam menüsünden seçin **Yayımla**.

## <a name="sign-in-page"></a>Oturum açma sayfası

![Oturum açma sayfası](./media/vs-azure-tools-publish-azure-application-wizard/sign-in.png)

**Hesap** - bir hesap seçin veya seçin **Hesap Ekle** hesabı açılır listesinde.

**Aboneliğinizi seçin** -dağıtımınız için kullanılacak aboneliği seçin.

## <a name="settings-page---common-settings-tab"></a>Ayarları Sayfası - Genel Ayarları sekmesi

![Genel Ayarlar](./media/vs-azure-tools-publish-azure-application-wizard/settings-common-settings.png)

**Bulut hizmeti** -açılan listeyi kullanarak ya da var olan bir bulut hizmeti ya da seçin seçin  **&lt;Yeni Oluştur >**ve bir bulut hizmeti oluşturun. Her bir bulut hizmeti için parantez içinde veri merkezi görüntüler. Veri depolama hesabı (Gelişmiş) için veri merkezi konum olarak aynı olması bulut hizmeti için konum merkezi önerilir.

**Ortam** -seçin **üretim** veya **hazırlama**. Uygulamanızı bir sınama ortamında dağıtmak istiyorsanız, hazırlık ortamı seçin. 

**Derleme Yapılandırması** -seçin **hata ayıklama** veya **sürüm**.

**Hizmet yapılandırması** -seçin **bulut** veya **yerel**.

**Tüm roller için Uzak Masaüstü'nü etkinleştirin** -uzaktan hizmete bağlanmak istiyorsanız bu seçeneği belirleyin. Bu seçenek, öncelikle sorun giderme amacıyla kullanılır. Daha fazla bilgi için bkz: [etkinleştirmek Uzak Masaüstü bağlantısı için Visual Studio kullanarak Azure Cloud Services rolünde](cloud-services/cloud-services-role-enable-remote-desktop-visual-studio.md).

**Tüm web rolleri için Web dağıtımı etkinleştirmek** -web dağıtım hizmeti etkinleştirmek için bu seçeneği belirleyin. Da seçmelisiniz **tüm roller için Uzak Masaüstü'yi etkinleştirmek** seçeneği bu özelliği kullanın. Daha fazla bilgi için bkz: [yayımlama Visual Studio kullanarak bir bulut hizmeti](vs-azure-tools-publishing-a-cloud-service.md).

## <a name="settings-page---advanced-settings-tab"></a>Ayarları sayfası - Gelişmiş Ayarları sekmesi

![Gelişmiş ayarlar](./media/vs-azure-tools-publish-azure-application-wizard/settings-advanced-settings.png)

**Dağıtım etiketi** -varsayılan adı kabul edin veya seçtiğiniz bir ad girin. Dağıtım etiketi tarihi eklemek için onay kutusunu seçili bırakın. 

**Depolama hesabı** -bu dağıtım için kullanılacak depolama hesabını seçin **&lt;Yeni Oluştur > bir depolama hesabı oluşturmak için. Her Depolama hesabı için parantez içinde veri merkezi görüntüler. Depolama hesabı için veri merkezi konumu (ortak ayarları) bulut hizmeti için veri merkezi konumu ile aynı olduğunu önerilir.

Azure depolama hesabı uygulama dağıtımı için paketi depolar. Uygulama dağıtıldıktan sonra paketi depolama hesabından kaldırılır.

**Hata durumunda dağıtımı Sil** -yayımlama sırasında hatalarla karşılaşılırsa silinmiş dağıtım sağlamak için bu seçeneği belirleyin. Bulut hizmetiniz için sabit bir sanal IP adresi korumak istiyorsanız, bu işaretinin kaldırılması gerekir.

**Dağıtım güncelleştirme** -güncelleştirilmiş bileşenleri yalnızca dağıtmak istiyorsanız bu seçeneği belirleyin. Bu dağıtım türünde tam bir dağıtıma göre daha hızlı olabilir. Bulut hizmetiniz için sabit bir sanal IP adresi korumak istiyorsanız, bu denetlenmesi. 

**Dağıtım güncelleştirmesi - ayarları** -bu iletişim kutusunu daha fazla nasıl güncelleştirilmesi rolleri istediğinizi belirtmek için kullanılır. Seçerseniz **Artımlı güncelleştirme**, uygulamanızın her örneği güncelleştirilir birbiri ardından uygulamayı her zaman kullanılabilir olmasını sağlamak. Seçerseniz **eşzamanlı güncelleştirme**, uygulamanızın hepsinin aynı anda güncelleştirilir. Eşzamanlı güncelleştirme hızlıdır ancak hizmetiniz güncelleştirme işlemi sırasında kullanılamayabilir.

![Dağıtım ayarları](./media/vs-azure-tools-publish-azure-application-wizard/deployment-settings.png)

**IntelliTrace'i etkinleştirme** -IntelliTrace etkinleştirmek isteyip istemediğinizi belirtin. IntelliTrace ile Azure içinde çalıştığında bir rol örneği için kapsamlı hata ayıklama bilgileri oturum açabilir. Bir sorunun nedenini bulmak gerekiyorsa, Azure'da çalışıyormuş gibi kodunuzu Visual Studio'dan adım için IntelliTrace günlüklerini kullanabilirsiniz. IntelliTrace'i kullanma hakkında daha fazla bilgi için bkz: [yayımlanan Azure bulut hizmeti Visual Studio ve IntelliTrace ile hata ayıklama](./vs-azure-tools-intellitrace-debug-published-cloud-services.md).

**Profil oluşturma etkinleştir** -performans profili oluşturma etkinleştirmek isteyip istemediğinizi belirtin. Visual Studio profil oluşturucu, bulut hizmetinizin nasıl çalışacağını hesaplama yönlerini ayrıntılı analizini almanızı sağlar. Visual Studio profil oluşturucu kullanma hakkında daha fazla bilgi için bkz: [bir Azure bulut Hizmeti performansını test etme](./vs-azure-tools-performance-profiling-cloud-services.md).

**Uzaktan hata ayıklayıcı tüm roller için etkinleştirme** -uzaktan hata ayıklamayı etkinleştirmek isteyip istemediğinizi belirtin. Visual Studio kullanarak bulut hizmetlerinde hata ayıklama ile ilgili daha fazla bilgi için bkz: [bir Azure bulut hizmeti ya da sanal makineye Visual Studio'da hata ayıklama](./vs-azure-tools-debug-cloud-services-virtual-machines.md).

## <a name="diagnostics-settings-page"></a>Tanılama Ayarları sayfası

![Tanılama ayarları](./media/vs-azure-tools-publish-azure-application-wizard/diagnostic-settings.png)

Tanılama, bir Azure bulut hizmeti (veya Azure sanal makine) gidermek sağlar. Tanılama hakkında daha fazla bilgi için bkz: [yapılandırma tanılama Azure Cloud Services ve sanal makineler için](./vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md). Application Insights hakkında daha fazla bilgi için bkz: [Application Insights nedir?](./application-insights/app-insights-overview.md).

## <a name="summary-page"></a>Özet sayfası

![Özet](./media/vs-azure-tools-publish-azure-application-wizard/summary.png)

**Hedef profil** -seçtiğiniz ayarları bir yayımlama profili oluşturmayı seçebilirsiniz. Örneğin, bir test ortamı için bir profil ve üretim için başka oluşturabilirsiniz. Bu profili kaydetmek üzere seçim yapın **kaydetmek** simgesi. Sihirbaz profili oluşturur ve Visual Studio projesinde kaydeder. Profil adı değiştirmek için açık **hedef profil** listeleyin ve ardından  **&lt;Yönet... &gt;**.

   > [!Note]
   > Visual Studio'da Çözüm Gezgini'nde yayımlama profili görüntülenir ve profil ayarları .azurePubxml uzantılı bir dosyaya yazılır. Ayarları XML etiketleri öznitelik olarak kaydedilir.

## <a name="publishing-your-application"></a>Uygulamanızı yayımlama

Projenizin dağıtımı için tüm ayarları yapılandırdıktan sonra seçeneğini **Yayımla** iletişim kutusunun altındaki. İşlem durumu izleyebilirsiniz **çıkış** Visual Studio'daki.

## <a name="next-steps"></a>Sonraki adımlar

- [Geçiş ve bir Azure bulut hizmeti Visual Studio'dan bir Web uygulaması yayımlama](./vs-azure-tools-migrate-publish-web-app-to-cloud-service.md)

- [Bir Azure bulut hizmeti yayımlamak için Visual Studio kullanmayı öğrenin](./vs-azure-tools-publishing-a-cloud-service.md)

- [Yayımlanan Azure bulut hizmeti Visual Studio ve IntelliTrace ile hata ayıklama](./vs-azure-tools-intellitrace-debug-published-cloud-services.md)

- [Bir Azure bulut Hizmeti performansını test etme](./vs-azure-tools-performance-profiling-cloud-services.md)

- [Azure bulut Hizmetleri ve sanal makineler için tanılama yapılandırma](./vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).

- [Application Insights nedir?](./application-insights/app-insights-overview.md)