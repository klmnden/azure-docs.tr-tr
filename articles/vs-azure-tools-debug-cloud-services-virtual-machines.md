---
title: "Bir Azure bulut hizmeti ya da sanal makineye Visual Studio'da hata ayıklama | Microsoft Docs"
description: "Bir bulut hizmeti ya da sanal makine Visual Studio'da hata ayıklama"
services: visual-studio-online
documentationcenter: na
author: mikejo
manager: ghogen
editor: 
ms.assetid: 945e06e0-2100-41af-b218-72347367ddab
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/11/2016
ms.author: mikejo
ms.openlocfilehash: a303e080bc847daf023eed2e9ba1ffc31e340160
ms.sourcegitcommit: b83781292640e82b5c172210c7190cf97fabb704
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="debugging-an-azure-cloud-service-or-virtual-machine-in-visual-studio"></a>Bir Azure bulut hizmeti ya da sanal makineye Visual Studio'da hata ayıklama
Visual Studio Azure bulut Hizmetleri ve sanal makineleri hata ayıklama için farklı seçenekler sunar.

## <a name="debug-your-cloud-service-on-your-local-computer"></a>Bulut hizmetinizi yerel bilgisayarınızda hata ayıklama
Zamandan tasarruf edebilirsiniz ve Azure kullanarak para işlem öykünücüsü, bulut hizmeti yerel makinede hata ayıklamak için. Dağıtmadan önce bir hizmet yerel olarak hata ayıklama tarafından, güvenilirlik ve performans işlem zaman için ödeme olmadan artırabilir. Ancak, bazı hatalar yalnızca bir bulut hizmeti Azure'da çalıştırdığınızda oluşabilecek kendisi. Hizmetinizi yayımlama ve ardından bir rol örneği için hata ayıklayıcı ekleyin uzaktan hata ayıklama etkinleştirirseniz, bu hatalarını ayıklayabilirsiniz.

Öykünücü Azure işlem hizmeti canlandırır ve böylece test ve dağıtmadan önce bulut hizmetinde hata ayıklama, yerel bir ortamda çalışır. Öykünücüsü rolü örneklerinizi yaşam döngüsü işler ve yerel depolama gibi sanal kaynaklara erişim sağlar. Hata ayıklama veya hizmetinizi Visual Studio'dan çalıştırdığınızda otomatik olarak bir arka plan uygulaması olarak öykünücüyü başlatır ve ardından hizmetinizi öykünücüsünü dağıtır. Öykünücü, yerel ortamınızda çalıştığında hizmetinizi görüntülemek için kullanabilirsiniz. Tam veya öykünücü express sürümü çalıştırabilirsiniz. (Azure 2.3 ile başlayarak, emulator express sürümü varsayılandır.) Bkz: [çalıştırın ve bir bulut hizmeti yerel olarak hata ayıklama öykünücüsü kullanarak](vs-azure-tools-emulator-express-debug-run.md).

### <a name="to-debug-your-cloud-service-on-your-local-computer"></a>Bulut hizmetinizi yerel bilgisayarınızda hata ayıklamak için
1. Menü çubuğunda seçin **hata ayıklama**, **hata ayıklamayı Başlat** Azure bulut hizmeti projenizi çalıştırmak için. Alternatif olarak, F5 tuşuna basın. İşlem öykünücüsü başlayarak bir ileti görürsünüz. Öykünücü başladığında, sistem tepsisi simgesi, onaylar.

    ![Sistem tepsisindeki Azure öykünücüsü](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC783828.png)
2. Bildirim alanında Azure simgesi için kısayol menüsünü açarak için işlem öykünücüsü kullanıcı arabirimini görüntülemek ve ardından **Göster işlem öykünücüsü kullanıcı Arabiriminde**.

    Sol bölmede UI işlem öykünücüsü ve her hizmet çalışan rolü örneklerini şu anda dağıtılan hizmetleri gösterir. Hizmet veya sağ bölmede yaşam döngüsü, günlüğe kaydetme ve tanılama bilgilerini görüntülemek için rolleri seçebilirsiniz. Odağı dahil penceresinin üst kenar boşluğunda yerleştirirseniz sağ bölmede doldurmak için genişletir.
3. Komutları seçme adım uygulaması aracılığıyla **hata ayıklama** menü ve kodunda kesme noktalarını ayarlama. Hata ayıklayıcı uygulama adım adım, bölmeleri uygulamanın geçerli durumuyla güncellenir. Hata ayıklama durdurduğunuzda, uygulama dağıtımı silinir. Uygulamanızı bir web rolü içerir ve web tarayıcısı başlatmak için başlatma eylemi özelliği ayarladınız, Visual Studio web uygulamanızı tarayıcıda başlatır. Hizmet yapılandırmasının bir rolün örnekleri sayısını değiştirirseniz, bulut hizmetini durdurmak ve ardından bu yeni rol örneklerini ayıklayabilirsiniz böylece hata ayıklamayı yeniden başlatın.

    **Not:** çalıştıran veya hizmetinizi hata ayıklama durdurduğunuzda, yerel işlem öykünücüsü ve depolama öykünücüsü durduruldu değil. Bunları açıkça bildirim alanından durdurmanız gerekir.

## <a name="debug-a-cloud-service-in-azure"></a>Azure bulut hizmetinde hata ayıklama
Böylece gerekli Hizmetleri (örneğin, msvsmon.exe) rolü örneklerinizi çalıştırmak sanal makinelerde yüklü olduğundan, bulut hizmetinizin dağıttığınızda bir bulut hizmeti uzak makinede hata ayıklamak için bu işlevselliği açıkça etkinleştirmelisiniz. Hizmet yayımlandığında uzaktan hata ayıklamayı etkinleştirme kaydetmedi verirseniz uzaktan hata ayıklama etkin durumdayken hizmeti yeniden yayımlamanız gerekir.

Uzaktan hata ayıklama için bir bulut hizmeti etkinleştirirseniz, performans sergiler veya ek ücrete tabi değildir. Hizmeti kullanan istemciler olumsuz yönde etkilenebilir nedeniyle bir üretim hizmette uzaktan hata ayıklama kullanmamalısınız.

> [!NOTE]
> Bir bulut hizmeti Visual Studio'dan yayımladığınızda, etkinleştirebilirsiniz **IntelliTrace** .NET Framework 4 veya .NET Framework 4.5 hedef herhangi bir rol, hizmetindeki için. Kullanarak **IntelliTrace**, bir rol örneği geçmişte oluşan olaylarla inceleyin ve bu süre bağlamından yeniden oluşturun. Bkz: [yayımlanan bulut hizmeti IntelliTrace ve Visual Studio ile hata ayıklama](http://go.microsoft.com/fwlink/?LinkID=623016) ve [kullanarak IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx).
>
>

### <a name="to-enable-remote-debugging-for-a-cloud-service"></a>Uzak bir bulut hizmeti için hata ayıklamayı etkinleştirmek için
1. Azure projesi için kısayol menüsünü açın ve ardından **Yayımla**.
2. Seçin **hazırlama** ortamı ve **hata ayıklama** yapılandırma.

    Bu yalnızca bir kılavuz olarak sağlanmıştır. Bir üretim ortamında test ortamınızı çalıştırmayı seçebilirsiniz. Ancak, üretim ortamına uzaktan hata ayıklama devre dışı bırakırsanız kullanıcılar olumsuz yönde etkileyebilir. Yayın yapılandırma seçebilirsiniz, ancak daha kolay hata ayıklama hata ayıklama yapılandırmasını sağlar.

    ![Hata ayıklama yapılandırmasını seçin](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746717.gif)
3. Her zamanki adımları izleyin, ancak seçin **etkinleştirmek uzaktan hata ayıklayıcı tüm rolleri için** onay kutusunu **Gelişmiş ayarları** sekmesi.

    ![Hata ayıklama yapılandırması](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746718.gif)

### <a name="to-attach-the-debugger-to-a-cloud-service-in-azure"></a>Azure bulut hizmetinde hata ayıklayıcı iliştirmek için
1. Sunucu Gezgini'nde, bulut hizmetinizin düğümünü genişletin.
2. Ekleyin ve ardından istediğiniz rol ya da rol örneği için kısayol menüsünü açın **Attach hata ayıklayıcı**.

    Bir rolü hata ayıklama, bu rolünün her örneği için Visual Studio hata ayıklayıcısı ekler. Hata ayıklayıcı kod satırını çalıştırır ve bu kesme noktası tüm koşulları karşılayan ilk rol örneği için bir kesme noktası kesintiye uğrar. Bir örneği, yalnızca o örneği ve yalnızca bu belirli örnek kod satırını çalıştırır ve kesme noktası'nın koşullara uyan bir kesme noktası sayfasındaki sonlarını hata ayıklayıcı ekler hata ayıklama durumunda.

    ![Hata ayıklayıcıyı Ekle](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746719.gif)
3. Hata ayıklayıcı örneğine ekler sonra her zamanki gibi hata ayıklama. Hata ayıklayıcı rolünüz için uygun ana bilgisayar işlemi için otomatik olarak ekler. Rolü nedir bağlı olarak, hata ayıklayıcı w3wp.exe, WaWorkerHost.exe veya WaIISHost.exe ekler. Hata ayıklayıcısı ekli olduğu işlem doğrulamak için Sunucu Gezgininde örneği düğümünü genişletin. Bkz: [Azure rol mimarisi](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) Azure işlemleri hakkında daha fazla bilgi.

    ![Kod türünü seç iletişim kutusu](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)
4. Hata ayıklayıcısı ekli olduğu işlemleri belirlemek için işlemler iletişim kutusunda, menü çubuğu, hata ayıklama, Windows, işlemleri seçme açın. (Klavye: Ctrl + Alt + Z) Belirli bir işlemin ayırmak için kısayol menüsünü açın ve ardından **ayırma işlemi**. Ya da örneği düğümü Sunucu Gezgini işlemi bulun, kısayol menüsünü açın ve ardından **ayırma işlemi**.

    ![Hata ayıklama işlemleri](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC690787.gif)

> [!WARNING]
> Kesme noktaları uzak uzun durur kaçının hata ayıklama. Azure yanıt olarak birkaç dakikadan uzun süre için durduruldu ve trafiği için bu örneği göndermeye durdurur işlem değerlendirir. Uzun süre durdurursanız msvsmon.exe işleminden ayırır.
>
>

Hata Ayıklayıcı'dan tüm işlemler, örneği veya rol ayırmak için rol veya örneğinin hatalarını ayıkladığınız ve ardından kısayol menüsünü açın **Detach hata ayıklayıcı**.

## <a name="limitations-of-remote-debugging-in-azure"></a>Azure'da uzaktan hata ayıklama sınırlamaları
Azure SDK 2.3 uzaktan hata ayıklama aşağıdaki sınırlamalara sahiptir.

* Uzaktan hata ayıklama etkin durumdayken, herhangi bir rolü 25'ten fazla örneğe sahip bir bulut hizmeti yayımlanamıyor.
* Hata ayıklayıcı 30400 için 30424, 31400 için 31424 ve 32400 için 32424 bağlantı noktalarını kullanır. Bu bağlantı noktaları birini kullanmayı denerseniz, hizmetinizi yayımlama yükleyemezsiniz ve aşağıdaki hata iletilerinden birini Azure için etkinlik günlüğünde görüntülenir:

  * .Cscfg dosyası .csdef dosyası karşı doğrulanırken bir hata oluştu.
    Uç nokta ayrılmış bağlantı noktası aralığı 'range' Microsoft.WindowsAzure.Plugins.RemoteDebugger.Connector 'rolü' rolünün zaten tanımlı bir bağlantı veya aralığı ile çakışıyor.
  * Ayırma başarısız oldu. Lütfen daha sonra yeniden deneyin, VM boyutunu veya rol örneklerinin sayısını azaltmayı deneyin veya farklı bir bölgeye dağıtmayı deneyin.

## <a name="debugging-azure-virtual-machines"></a>Hata ayıklama Azure sanal makineler
Visual Studio'da Sunucu Gezgini kullanarak Azure sanal makinelerde çalışan programlar ayıklayabilirsiniz. Bir Azure sanal makinesi üzerinde uzaktan hata ayıklama etkinleştirdiğinizde, Azure sanal makineye uzaktan hata ayıklama uzantısı yükler. Ardından, sanal makinede işlemlerin ekleyin ve normal olarak hata ayıklama.

> [!NOTE]
> Azure Kaynak Yöneticisi yığınından oluşturulan sanal makineleri uzaktan Visual Studio 2015'te bulut Gezgini'ni kullanarak hata ayıklaması yapılabilir. Daha fazla bilgi için bkz: [bulut Gezgini ile Azure kaynaklarını yönetme](http://go.microsoft.com/fwlink/?LinkId=623031).
>
>

### <a name="to-debug-an-azure-virtual-machine"></a>Bir Azure sanal makinesi hata ayıklamak için
1. Sunucu Gezgini'nde, sanal makine düğümünü genişletin ve hata ayıklamak istediğiniz sanal makine düğümünü seçin.
2. Bağlam menüsünü açın ve seçin **etkinleştirmek hata ayıklama**. Sanal makinede hata ayıklamayı etkinleştirmek istiyorsanız emin olup olmadığınız sorulduğunda **Evet**.

    Azure uzaktan hata ayıklama uzantısı hata ayıklamayı etkinleştirmek için sanal makineye yükler.

    ![Sanal makine hata ayıklamayı etkinleştir komutu](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Azure etkinlik günlüğü](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)
3. Uzaktan hata ayıklama uzantısı yüklemeyi tamamladıktan sonra sanal makinenin bağlam menüsünü açın ve seçin **hata ayıklayıcı Ekle...**

    Azure sanal makinede işlemlerin bir listesini alır ve bunları ekleme işlemi iletişim kutusu gösterilir.

    ![Hata ayıklayıcısı Ekle komutu](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)
4. İçinde **ekleme işlemi için** iletişim kutusunda **seçin** yalnızca istediğiniz hata ayıklamak için kod türlerini göstermek için sonuçları listesini sınırlandırmak için. 32 veya 64-yönetilen bit kod, yerel kod veya her ikisini ayıklayabilirsiniz.

    ![Kod türünü seç iletişim kutusu](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)
5. Sanal makinede hata ayıklama ve ardından istediğiniz işlemleri seçin **Attach**. Örneğin, bir web uygulaması sanal makinede hata ayıklama istiyorsanız w3wp.exe işlemi seçebilirsiniz. Bkz: [hata ayıklama bir veya daha fazla işlemleri Visual Studio'da](https://msdn.microsoft.com/library/jj919165.aspx) ve [Azure rol mimarisi](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) daha fazla bilgi için.

## <a name="create-a-web-project-and-a-virtual-machine-for-debugging"></a>Bir web projesi ve hata ayıklama için bir sanal makine oluşturma
Azure projenizi yayımlamadan önce hata ayıklama ve test senaryoları destekler ve, test ve programları izleme yükleyebileceğiniz bir kapsanan ortamında test etmek yararlı olabilir. Bunu yapmak için bir uzaktan bir sanal makinede uygulamanızın hatalarını ayıklama yoldur.

Visual Studio ASP.NET projeleri uygulama test etmek için kullanabileceğiniz yararlı bir sanal makine oluşturmak için bir seçenek sunar. Sanal makine PowerShell, Uzak Masaüstü'nü ve WebDeploy gibi sık gerekli uç noktaları içerir.

### <a name="to-create-a-web-project-and-a-virtual-machine-for-debugging"></a>Bir web projesi ve hata ayıklama için bir sanal makine oluşturmak için
1. Visual Studio'da yeni bir ASP.NET Web uygulaması oluşturun.
2. Yeni ASP.NET projesi iletişim kutusunda, Azure bölümünde seçin **sanal makine** açılır liste kutusunda. Bırakın **uzak kaynaklar Oluştur** onay kutusu seçili. Seçin **Tamam** devam etmek için.

    **Azure'da sanal makine oluşturma** iletişim kutusu görüntülenir.

    ![ASP.NET web projesi oluştur iletişim kutusu](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746723.png)

    **Not:** zaten oturum açtıysanız, Azure hesabınızda oturum açmak için istenir.

1. Sanal makine için çeşitli ayarları seçin ve ardından **Tamam**. Bkz: [sanal makineleri](http://go.microsoft.com/fwlink/?LinkId=623033) daha fazla bilgi için.

    Sanal makine adı için DNS adını girin adı olacaktır.

    ![Sanal makine oluştur Azure iletişim kutusu](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746724.png)

    Azure sanal makine ve ardından hükümleri oluşturur ve Uzak Masaüstü'nü ve Web dağıtımı gibi uç noktaları yapılandırır
2. Sanal makine tam olarak yapılandırıldıktan sonra sunucu Gezgini'nde sanal makinenin düğümünü seçin.
3. Bağlam menüsünü açın ve seçin **etkinleştirmek hata ayıklama**. Sanal makinede hata ayıklamayı etkinleştirmek istiyorsanız emin olup olmadığınız sorulduğunda **Evet**.

    Azure uzaktan hata ayıklama uzantısı hata ayıklamayı etkinleştirmek için sanal makineye yükler.

    ![Sanal makine hata ayıklamayı etkinleştir komutu](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Azure etkinlik günlüğü](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)
4. Kısmında özetlendiği gibi projenizin Yayımla [nasıl yapılır: bir Web projesi kullanarak tek tıklamayla yayımlama Visual Studio'da dağıtmak](https://msdn.microsoft.com/library/dd465337.aspx). Sanal makinede üzerinde hata ayıklamak istediğiniz çünkü **ayarları** sayfasında **Web'i Yayımla** seçin **hata ayıklama** yapılandırma olarak. Bu hata ayıklarken kod semboller kullanılabilir olduğundan emin olur.

    ![Yayımlama ayarları](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718349.png)
5. İçinde **dosya yayımlama seçeneği**seçin **hedefteki ek dosyaları Kaldır** proje daha erken bir zamanda zaten dağıtıldıysa.
6. Sunucu Gezgini'nde, sanal makinenin bağlam menüsünde projeyi yayımlar sonra seçin **hata ayıklayıcı Ekle...**

    Azure sanal makinede işlemlerin bir listesini alır ve bunları ekleme işlemi iletişim kutusu gösterilir.

    ![Hata ayıklayıcısı Ekle komutu](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)
7. İçinde **ekleme işlemi için** iletişim kutusunda **seçin** yalnızca istediğiniz hata ayıklamak için kod türlerini göstermek için sonuçları listesini sınırlandırmak için. 32 veya 64-yönetilen bit kod, yerel kod veya her ikisini ayıklayabilirsiniz.

    ![Kod türünü seç iletişim kutusu](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)
8. Sanal makinede hata ayıklama ve ardından istediğiniz işlemleri seçin **Attach**. Örneğin, bir web uygulaması sanal makinede hata ayıklama istiyorsanız w3wp.exe işlemi seçebilirsiniz. Bkz: [hata ayıklama bir veya daha fazla işlemleri Visual Studio'da](https://msdn.microsoft.com/library/jj919165.aspx) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
* Kullanım **IntelliTrace** bir yayın sunucusundan günlük çağrıları ve olayları toplamak için. Bkz: [yayımlanan bulut hizmeti IntelliTrace ve Visual Studio ile hata ayıklama](http://go.microsoft.com/fwlink/?LinkID=623016).
* Kullanım **Azure tanılama** rolleri geliştirme ortamında veya Azure çalıştırıp çalıştırmadığınızı ayrıntılı bilgileri rolleri, kod çalıştırılmasını günlüğe kaydetmek için. Bkz: [Azure tanılama kullanarak günlük verileri toplama](http://go.microsoft.com/fwlink/p/?LinkId=400450).
