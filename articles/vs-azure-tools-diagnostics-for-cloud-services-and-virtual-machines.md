---
title: Azure Cloud Services ve sanal makineler için tanılama ayarlama | Microsoft Docs
description: Azure cloude Hizmetleri ve sanal makineleri (VM'ler) Visual Studio hata ayıklama için tanılama ayarlanacağını öğrenin.
services: visual-studio-online
documentationcenter: na
author: mikejo
manager: douge
editor: ''
ms.assetid: e70cd7b4-6298-43aa-adea-6fd618414c26
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: mikejo
ms.openlocfilehash: 34c667b0a594682e4d099e7bff64bfdb336b850b
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="set-up-diagnostics-for-azure-cloud-services-and-virtual-machines"></a>Azure Cloud Services ve sanal makineler için tanılama ayarlayın
Bir Azure bulut hizmeti ya da sanal makineyi gidermek gerektiğinde, daha kolay Azure tanılama ayarlamak için Visual Studio'yu kullanabilirsiniz. Tanılama sistem verileri ve sanal makineler ve bulut hizmeti çalıştıran sanal makine örnekleri günlük verilerini yakalar. Tanılama veri seçtiğiniz bir depolama hesabı aktarılır. Azure'da oturum Tanılama hakkında daha fazla bilgi için bkz [Azure App Service'te Web uygulamalarını için tanılama günlüğünü etkinleştirme](app-service/web-sites-enable-diagnostic-log.md).

Bu makalede, Visual Studio açın ve Azure tanılama öncesinde ve sonrasında dağıtım ayarlamak için nasıl kullanılacağını gösteriyoruz. Tanılama Azure sanal makinelerinde ayarlanan nasıl, tanılama bilgilerini toplamak için türlerini seçme ve toplandıktan sonra bilgilerini görüntülemek nasıl öğrenin.

Azure tanılama ayarlamak için aşağıdaki seçeneklerden birini kullanabilirsiniz:

* Tanılama ayarlarını değiştirin **tanılama Yapılandırması** Visual Studio'da iletişim kutusu. Ayarlar (Azure SDK 2.4 ve önceki sürümlerinde, dosyanın diagnostics.wadcfg denir) diagnostics.wadcfgx adlı bir dosyaya kaydedilir. Yapılandırma dosyasını doğrudan değiştirebilirsiniz. Dosyayı el ile güncelleştirirseniz, yapılandırma değişiklikleri etkili bulut dağıttığınız sonraki açışınızda Azure hizmet veya hizmet öykünücüsünde çalıştırın.
* Bir bulut hizmeti veya çalıştıran sanal makine için tanılama ayarları değiştirmek için Visual Studio cloud Explorer veya Sunucu Gezgini'ı kullanın.

## <a name="azure-sdk-26-diagnostics-changes"></a>Azure SDK 2.6 tanılama değişiklikleri
Azure SDK 2.6 ve daha sonra Visual Studio projelerinde aşağıdaki değişiklikleri uygulayın:

* Yerel öykünücüsü artık tanılama destekler. Bu tanılama verilerini toplamak ve geliştirmek ve Visual Studio'da test ederken, uygulamanızın doğru izlemeleri oluşturduğundan emin olmak anlamına gelir. Bağlantı dizesi `UseDevelopmentStorage=true` Azure storage öykünücüsü kullanarak Visual Studio bulut hizmeti projenizi çalıştırırken tanılama veri koleksiyonu açık kapatır. Tüm tanılama verilerini geliştirme depolama depolama hesabında toplanır.
* Tanılama depolama hesabı bağlantı dizesi `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` hizmet yapılandırma (.cscfg) dosyasında depolanır. Azure SDK 2.5 tanılama depolama hesabı diagnostics.wadcfgx dosyasında belirtilir.

Bağlantı dizesi, bazı anahtar yollarla Azure SDK 2.6 ve daha sonra Azure SDK 2.4 karşı ve daha önceki sürümlerde farklı şekilde çalışır:

* Azure SDK 2.4 ve önceki sürümlerinde, bağlantı dizesi tanılama günlükleri aktarmak için depolama hesabı bilgilerini almak için bir çalışma zamanı tanılama eklenti tarafından kullanılır.
* Azure SDK 2.6 ve daha sonra Visual Studio tanılama bağlantı dizesi Azure tanılama uzantısını yayımlama sırasında uygun depolama hesap bilgileriyle ayarlamak için kullanır. Bağlantı dizesi, Visual Studio yayımlama sırasında kullanılır farklı hizmet yapılandırması için farklı depolama hesapları tanımlamak için kullanabilirsiniz. Ancak, eklenti tanılama Azure SDK 2.5 sonra kullanılabilir olmadığından, .cscfg dosyası tek başına tanılama uzantı ayarlanamıyor. Visual Studio veya PowerShell gibi araçlar kullanarak uzantı ayrı olarak ayarlamanız gerekir.
* PowerShell kullanarak tanılama uzantı kurma işlemini basitleştirmek için Visual Studio Paketi çıktısını tanılama uzantısını her rol için ortak yapılandırma XML içerir. Visual Studio tanılama bağlantı dizesi ortak yapılandırma depolama hesabı bilgilerini doldurmak için kullanır. Genel yapılandırma dosyaları Uzantıları klasöründe oluşturulur. Genel yapılandırma dosyalarını adlandırma deseni PaaSDiagnostics kullanın. &lt;rol adı\>. PubConfig.xml. Tüm PowerShell tabanlı dağıtımlar, her yapılandırma bir role eşleştirmek için bu deseni kullanabilirsiniz.
* [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) tanılama verilere erişmek için .cscfg dosyasında bağlantı dizesini kullanır. Verilerin görünen **izleme** sekmesi. Bağlantı dizesi, ayrıntılı izleme verileri portalda göstermek için hizmet ayarlamak için gereklidir.

## <a name="migrate-projects-to-azure-sdk-26-and-later"></a>Projeler için Azure SDK 2.6 ve daha sonra geçirme
.Wadcfgx dosyasında belirtilen tanılama depolama hesabı varsa Azure SDK 2.5-Azure SDK 2.6 veya sonraki sürümleri geçirdiğinizde, depolama hesabı bu dosyada kalır. Farklı depolama hesapları farklı depolama yapılandırmaları için kullanma esnekliğini yararlanmak için el ile bağlantı dizesi projenize ekleyin. Azure SDK 2.4 veya daha önceki Azure SDK 2.6 proje geçiş, tanılama bağlantı dizeleri korunur. Ancak, bağlantı dizeleri önceki bölümde açıklanan Azure SDK 2.6 nasıl davranılır değişiklikler unutmayın.

### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Visual Studio tanılama depolama hesabı nasıl belirler
* Tanılama bağlantı dizesi .cscfg dosyasında belirtilmediği takdirde, Visual Studio yayımlama sırasında ve paketlemesi sırasında ortak yapılandırma XML dosyalarını oluşturduğunda tanılama uzantı ayarlamak için kullanır.
* Bir tanılama bağlantı dizesi .cscfg dosyasında belirtilmezse, Visual Studio tanılama uzantısını yayımlama ve genel yapılandırma XML oluşturmak için ayarlamak üzere .wadcfgx dosyasında belirtilen depolama hesabı kullanmaya geri döner Dosyaları paketleme sırasında.
* .Cscfg dosyası tanılama bağlantı dizesinde .wadcfgx dosya depolama hesabında daha önceliklidir. Bir tanılama bağlantı dizesi ise .cscfg dosyasında belirtilen Visual Studio bu bağlantı dizesini kullanır ve .wadcfgx depolama hesabında yok sayar.

### <a name="what-does-the-update-development-storage-connection-strings-check-box-do"></a>"Geliştirme storage bağlantı dizelerini güncelleştir" onay kutusunu ne yapar?
**Güncelleştirme geliştirme storage bağlantı dizelerini tanılama ve önbelleğe alma için Microsoft Azure depolama hesabı kimlik bilgileriyle Microsoft Azure yayımlama sırasında** onay kutusudur herhangi bir geliştirme depolama alanı güncelleştirmek için kolay bir yol Yayımlama sırasında belirttiğiniz Azure depolama hesabıyla hesap bağlantı dizeleri.

Örneğin, bu onay kutusunu ve tanılama bağlantı dizesi seçerseniz belirtir `UseDevelopmentStorage=true`, Azure için proje yayımladığınızda, Visual Studio otomatik olarak güncelleştirir tanılama bağlantı dizesi olarak belirtilen depolama hesabı ile Yayımlama Sihirbazı. Ancak, gerçek depolama hesabı tanılama bağlantı dizesi olarak belirtilmişse, bu hesabı yerine kullanılır.

## <a name="diagnostics-functionality-differences-in-azure-sdk-24-and-earlier-vs-azure-sdk-25-and-later"></a>Tanılama işlevleri farklılıkları Azure SDK 2.4 ve önceki vs. Azure SDK 2,5 ve üzeri
Projenizi Azure SDK 2.4 den ve daha önce Azure SDK 2.5 veya sonraki bir sürüme yükseltiyorsanız, aşağıdaki tanılama işlevleri farklılıkları göz önünde bulundurun:

* **Yapılandırma API'leri dışıdır**. Tanılama program yapılandırması Azure SDK 2.4 bulunan ve önceki ancak Azure SDK 2.5 ve daha sonra kullanım dışı bırakılmıştır. Tanılama yapılandırmanızı kodda şu anda tanımlanmış olması durumunda, bu ayarları çalışmaya devam geçirilen proje tanılama için en baştan yeniden yapılandırmanız gerekir. Tanılama yapılandırması için Azure SDK 2.4 diagnostics.wadcfg dosyasıdır. Tanılama yapılandırması için Azure SDK 2.5 ve daha sonra diagnostics.wadcfgx dosyasıdır.
* **Bulut hizmeti uygulamaları için tanılama yalnızca rol düzeyinde yapılandırılabilir**. Azure SDK 2.5 ve daha sonra bulut hizmet uygulamaları için tanılama örnek düzeyinde ayarlayamazsınız.
* **Uygulamanızı dağıtma her zaman, tanılama yapılandırması güncelleştirilir**. Visual Studio Server Explorer'dan tanılama yapılandırmanızı değiştirirseniz ve uygulamanızı yeniden dağıtın bu eşlik sorunlara neden olabilir.
* **Azure SDK 2.5 ve daha sonra kilitlenme bilgi dökümleri tanılama yapılandırma dosyasında ve kod değil, yapılandırılmış**. Kilitlenme bilgi dökümleri kodda yapılandırılmış varsa, el ile yapılandırma koddan yapılandırma dosyasına aktarmanız gerekir. Kilitlenme bilgi dökümleri geçiş sırasında Azure SDK 2.6 aktarılmaz.

## <a name="turn-on-diagnostics-in-cloud-service-projects-before-you-deploy-them"></a>Bulut hizmeti projelerinde tanılama dağıtmadan önce Aç
Visual Studio öykünücüsü dağıtmadan önce hizmet çalıştırdığınızda, Azure'da çalışan rolleri için tanılama verilerini toplayabilir. Visual Studio tanılama ayarlarında yapılan tüm değişiklikler diagnostics.wadcfgx yapılandırma dosyasında kaydedilir. Bu ayarlar, bulut hizmetinizin dağıttığınızda tanılama verilerini kaydedildiği depolama hesabı belirtin.

[!INCLUDE [cloud-services-wad-warning](../includes/cloud-services-wad-warning.md)]

### <a name="to-turn-on-diagnostics-in-visual-studio-before-deployment"></a>Visual Studio tanılama dağıtmadan önce etkinleştirmek için

1. Rolü için kısayol menüsünden seçin **özellikleri**. Rolün içindeki **özellikleri** iletişim kutusunda **yapılandırma** sekmesi.
2. İçinde **tanılama** bölümünde, olduğundan emin olun **tanılamayı etkinleştir** onay kutusu seçilidir.
   
    ![Tanılamayı etkinleştir seçeneğine erişme](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796660.png)
3. Tanılama verileri için depolama hesabı belirtmek için üç nokta (...) düğmesini seçin.
   
    ![Depolama hesabı belirtin](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796661.png)
4. İçinde **depolama bağlantı dizesi oluştur** iletişim kutusunda, bir Azure aboneliğine Azure storage öykünücüsü kullanarak bağlanmak istediğiniz veya kimlik bilgileri'el ile girilen olup olmadığını belirtin.
   
    ![Depolama hesabı iletişim kutusu](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796662.png)
   
   * Seçerseniz **Microsoft Azure storage öykünücüsü**, bağlantı dizesini ayarlamak `UseDevelopmentStorage=true`.
   * Seçerseniz **aboneliğinizi**, kullanmak istediğiniz Azure aboneliğini seçin ve bir hesap adı girin. Azure Aboneliklerini yönetmek için seçin **hesaplarını yönetme**.
   * Seçerseniz **kimlik bilgileri'el ile girilen**, kullanmak istediğiniz Azure hesabı anahtarı ve adını girin.
5. Görüntülemek için **tanılama Yapılandırması** iletişim kutusunda **yapılandırma**. Dışında **genel** ve **günlük dizinleri**, her sekme toplamak bir tanılama veri kaynağını temsil eder. Varsayılan **genel** sekmesinde aşağıdaki tanılama veri toplama seçeneklerini sunar: **yalnızca hatalar**, **tüm bilgileri**, ve **özel plan**. Varsayılan **yalnızca hatalar** seçenek uyarılar veya izleme iletileri aktarmaz çünkü en az miktarda depolama kullanır. **Tüm bilgileri** seçenek en bilgileri aktarır, en fazla depolama alanı kullanır ve bu nedenle, çok pahalı bir seçenektir.
   
    ![Azure tanılama ve yapılandırma etkinleştir](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)
6. Bu örnek için select **özel plan** toplanan verileri özelleştirebileceğiniz şekilde seçeneği.
7. İçinde **MB Disk kotası** kutusunda Tanılama verileri için depolama hesabınızdaki ayırmak için ne kadar alan ayarlayabilirsiniz. Değiştirme veya varsayılan değerini kabul edin.
8. Her toplamak istediğiniz tanılama verilerini sekmesinde seçin **etkinleştirmek Aktarım, \<oturum türü\>**  onay kutusu. Örneğin, uygulama günlüklerini toplamak istiyorsanız **uygulama günlüklerini** sekmesine **etkinleştirmek uygulama günlüklerini aktarımını** onay kutusunu. Ayrıca, her bir tanılama veri türü tarafından gerekli diğer bilgileri belirtin. Her sekme için yapılandırma bilgileri için bölümüne bakın **tanılama veri kaynaklarını kurmak** bu makalenin ilerisinde yer. 
9. İstediğiniz tüm Tanılama verileri koleksiyonu etkinleştirdikten sonra seçin **Tamam**.
10. Azure bulut hizmeti projenizi Visual Studio'da her zamanki gibi çalıştırın. Uygulamanızı kullanırken, etkin günlük bilgilerini belirttiğiniz Azure depolama hesabı kaydedilir.

## <a name="turn-on-diagnostics-on-azure-virtual-machines"></a>Azure sanal makinelerde tanılama Aç
Visual Studio'da Azure sanal makineleri için tanılama verilerini toplayabilir.

### <a name="to-turn-on-diagnostics-on-azure-virtual-machines"></a>Azure sanal makineler üzerinde tanılamayı etkinleştirmek için

1. Sunucu Gezgini'nde, Azure düğümünü seçin ve henüz bağlıysanız Azure aboneliğinize bağlanma.
2. Genişletme **sanal makineleri** düğümü. Yeni bir sanal makine oluşturun veya varolan bir düğümü seçin.
3. İstediğiniz sanal makine için kısayol menüsünden seçin **yapılandırma**. Sanal makine yapılandırma iletişim kutusu görüntülenir.
   
    ![Bir Azure sanal makine yapılandırma](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796663.png)
4. Henüz yüklü değilse, Microsoft İzleme Aracısı tanılama uzantısını ekleyin. Bu uzantıya sahip Azure sanal makine için Tanılama verileri toplayabilir. Altında **yüklü uzantıları**, **kullanılabilir bir uzantı seçin** aşağı açılan liste kutusu, select **Microsoft İzleme Aracısı tanılama**.
   
    ![Bir Azure sanal makine uzantısını yükleyin](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766024.png)
   
    > [!NOTE]
   > Diğer tanılama uzantıları, sanal makineleriniz için kullanılabilir. Daha fazla bilgi için bkz: [sanal makine uzantıları ve özellikleri Windows için](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-features).
   > 
   > 
5. Uzantı ve görünüm eklemek için kendi **tanılama Yapılandırması** iletişim kutusunda **Ekle**.
6. Bir depolama hesabı belirtmek için işaretleyin **yapılandırma**ve ardından **Tamam**.
   
    Her sekme (dışında **genel** ve **günlük dizinleri**) toplamak bir tanılama veri kaynağını temsil eder.
   
    ![Azure tanılama ve yapılandırma etkinleştir](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)
   
    Varsayılan sekme **genel**, aşağıdaki tanılama veri toplama seçeneklerini sunar: **yalnızca hatalar**, **tüm bilgileri**, ve **özel plan**. Varsayılan seçenek **yalnızca hatalar**, uyarı veya izleme iletileri aktarmaz çünkü en az miktarda depolama alır. **Tüm bilgileri** seçeneği en bilgileri aktarır ve bu nedenle, depolama açısından en ucuz seçeneği oluşur.
7. Bu örnek için select **özel plan** toplanan verileri özelleştirebileceğiniz şekilde seçeneği.
8. **MB Disk kotası** kutusu ayırmak istediğiniz depolama hesabınız için Tanılama verileri ne kadar alan belirtir. İsterseniz varsayılan değeri değiştirebilirsiniz.
9. Her bir sekmede diagnostics veri toplamak istediğiniz seçin, **etkinleştirmek Aktarım, \<oturum türü\>**  onay kutusu.
   
    Örneğin, uygulama günlüklerini toplamak istiyorsanız seçin **etkinleştirmek uygulama günlüklerini aktarımını** onay kutusunu **uygulama günlüklerini** sekmesi. Ayrıca, her bir tanılama veri türü için gerekli diğer bilgileri belirtin. Her sekme için yapılandırma bilgileri için bölümüne bakın **tanılama veri kaynaklarını kurmak** bu makalenin ilerisinde yer.
10. İstediğiniz tüm Tanılama verileri koleksiyonu etkinleştirdikten sonra seçin **Tamam**.
11. Güncelleştirilmiş Projeyi kaydedin.
    
    Bir ileti **Microsoft Azure etkinlik günlüğü** penceresi belirten sanal makine güncelleştirildi.

## <a name="set-up-diagnostics-data-sources"></a>Tanılama veri kaynakları ayarlayın
Tanılama veri toplama etkinleştirdikten sonra toplamak istediğiniz tam olarak hangi veri kaynaklarını ve hangi bilgilerin toplandığı seçebilirsiniz. Sonraki bölümlerde sekmeleri **tanılama Yapılandırması** iletişim kutusu ve her hangi bir yapılandırma seçeneği anlamına gelir.

### <a name="application-logs"></a>Uygulama günlükleri
Uygulama günlüklerini bir web uygulaması tarafından üretilen tanılama bilgilerine sahip. Uygulama günlüklerini yakalama isteyip istemediğinizi seçin **etkinleştirmek uygulama günlüklerini aktarımını** onay kutusu. Artırmak veya uygulama günlüklerini aktarımını depolama hesabınıza arasındaki süreyi azaltmak için değiştirme **aktarım süresi (dak)** değeri. Ayrıca ayarlayarak günlüğünde yakalanan bilgi miktarını değiştirebilirsiniz **günlük düzeyi** değeri. Örneğin, seçin **ayrıntılı** daha fazla bilgi almak veya seçmek üzere **kritik** yalnızca kritik hataları yakalamak için. Uygulama günlüklerini yayar belirli tanılama sağlayıcısı varsa, sağlayıcının GUID ekleyerek günlükleri yakalayabilirsiniz **sağlayıcı GUID** kutusu.

  ![Uygulama günlükleri](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758145.png)

Uygulama günlükleri hakkında daha fazla bilgi için bkz: [Azure App Service'te Web uygulamalarını için tanılama günlüğünü etkinleştirme](app-service/web-sites-enable-diagnostic-log.md).

### <a name="windows-event-logs"></a>Windows olay günlükleri
Windows olay günlüklerini yakalamak için seçin **etkinleştirmek aktarımı, Windows olay günlüklerini** onay kutusu. Artırmak veya olay günlüklerini aktarımını depolama hesabınıza arasındaki süreyi azaltmak için değiştirme **aktarım süresi (dak)** değeri. İzlemek istediğiniz olay türleri için onay kutularını seçin.

![Olay günlükleri](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796664.png)

Azure SDK 2.6 veya sonraki sürümünü kullanıyorsanız ve bir özel veri kaynağı belirtmek istiyorsanız bu alana giriş **\<veri kaynağı adı\>** metin kutusuna ve ardından **Ekle**. Veri kaynağı diagnostics.cfcfg dosyasına eklenir.

Azure SDK 2.5 kullanıyorsanız ve bir özel veri kaynağı belirtmek istiyorsanız, ona ekleyebilirsiniz `WindowsEventLog` diagnostics.wadcfgx bölümünü dosyası gibi aşağıdaki örnekte:

```
<WindowsEventLog scheduledTransferPeriod="PT1M">
   <DataSource name="Application!*" />
   <DataSource name="CustomDataSource!*" />
</WindowsEventLog>
```
### <a name="performance-counters"></a>Performans sayaçları
Performans sayacı bilgileri sistem engelleri bulun ve sistem ve uygulama performansının ayarını yardımcı olabilir. Daha fazla bilgi için bkz: [Azure uygulaması oluşturma ve kullanma performans sayaçları](https://msdn.microsoft.com/library/azure/hh411542.aspx). Performans sayaçlarını yakalamak için seçin **etkinleştirmek performans sayaçları aktarımını** onay kutusu. Artırmak veya olay günlüklerini aktarımını depolama hesabınıza arasındaki süreyi azaltmak için değiştirme **aktarım süresi (dak)** değeri. İzlemek istediğiniz performans sayaçları için onay kutularını seçin.

![Performans sayaçları](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758147.png)

Listede bir performans sayacı izlemek için önerilen sözdizimini kullanarak performans sayacı girin. ve ardından **Ekle**. Sanal makinedeki işletim sistemini izleyebilirsiniz hangi performans sayaçlarını belirler. Sözdizimi hakkında daha fazla bilgi için bkz: [bir sayaç yolu belirtin](https://msdn.microsoft.com/library/windows/desktop/aa373193.aspx).

### <a name="infrastructure-logs"></a>Altyapı günlükleri
Altyapı günlükleri Azure tanılama altyapısı, uzaktan erişim modülü ve RemoteForwarder modülü hakkında bilgiler vardır. Altyapı günlükleri hakkında bilgi toplamak için seçin **etkinleştirmek altyapı günlükleri aktarımını** onay kutusu. Artırmak veya altyapı günlükleri aktarımını depolama hesabınıza arasındaki süreyi azaltmak için değiştirme **aktarım süresi (dak)** değeri.

![Tanılama altyapı günlükleri](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758148.png)

Daha fazla bilgi için bkz: [Azure tanılama kullanarak günlük verileri toplama](https://msdn.microsoft.com/library/azure/gg433048.aspx).

### <a name="log-directories"></a>Günlük dizinleri
Günlük dizinleri Internet Information Services (IIS) istekleri, başarısız olan istekler veya seçtiğiniz klasör için günlük dizinlerinden toplanan verilere sahip. Günlük dizinleri yakalamak için seçin **etkinleştirmek günlük dizinleri aktarımını** onay kutusu. Artırmak veya depolama hesabınız günlükleri aktarımını arasındaki aralığı azaltmak için değiştirme **aktarım süresi (dak)** değeri.

Gibi toplamak istediğiniz günlükleri onay kutularını seçin **IIS günlüklerini** ve **isteği başarısız oldu** günlükleri. Varsayılan depolama kapsayıcısını adları sağlanır, ancak adlarını değiştirebilirsiniz.

Herhangi bir klasörden günlükleri yakalayabilirsiniz. Yoldaki **mutlak dizin günlüğünden** bölümünde ve ardından **Dizin Ekle**. Günlükleri belirtilen kapsayıcılarında yakalanır.

![Günlük dizinleri](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796665.png)

### <a name="etw-logs"></a>ETW günlükleri
Kullanırsanız [Windows için olay izleme](https://msdn.microsoft.com/library/windows/desktop/bb968803\(v=vs.85\).aspx) (ETW) ve ETW günlükleri yakalamak istediğiniz seçin **etkinleştirmek ETW günlükleri aktarımını** onay kutusu. Artırmak veya depolama hesabınız günlükleri aktarımını arasındaki aralığı azaltmak için değiştirme **aktarım süresi (dak)** değeri.

Olaylar, olay kaynakları ve belirttiğiniz olay bildirimleri yakalanır. Bir olay kaynağı belirtmek üzere **olay kaynakları** bölümünde, bir ad girin ve ardından **olay kaynağı Ekle**. Benzer şekilde, bir olay bildiriminde belirtebilirsiniz **olay bildirimlerini** bölümünde ve ardından **ekleme olay bildirimi**.

![ETW günlükleri](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766025.png)

ETW framework ASP.NET sınıflarda üzerinden desteklenen [System.Diagnostics.aspx](https://msdn.microsoft.com/library/system.diagnostics(v=vs.110)) ad alanı. Devralır ve standart genişletir Microsoft.WindowsAzure.Diagnostics ad [System.Diagnostics.aspx](https://msdn.microsoft.com/library/system.diagnostics(v=vs.110)) sınıfları, kullanmayı sağlayan [System.Diagnostics.aspx](https://msdn.microsoft.com/library/system.diagnostics(v=vs.110)) bir günlük olarak Azure ortamı Framework. Daha fazla bilgi için bkz: [Microsoft Azure'da günlüğe kaydetme ve izleme denetimini ele](https://msdn.microsoft.com/magazine/ff714589.aspx) ve [Azure Cloud Services ve sanal makineleri tanılamayı etkinleştirme](cloud-services/cloud-services-dotnet-diagnostics.md).

### <a name="crash-dumps"></a>Kilitlenme bilgi dökümleri
Ne zaman bir rol örneği çöküyor hakkında bilgileri toplamak için seçin **etkinleştirmek kilitlenme dökümleri aktarımını** onay kutusu. (ASP.NET çoğu özel durumları işler olduğundan, bu yalnızca çalışan rolleri için genellikle kullanışlıdır.) Artırmak veya kilitlenme bilgi dökümleri için ayrılan depolama alanı yüzdesini azaltmak için değiştirme **Directory kota (%)** değeri. Burada kilitlenme bilgi dökümleri depolanır ve yakalama istediğinizi seçin depolama kapsayıcısı değiştirebileceğiniz bir **tam** veya **Mini** dökümü.

Şu anda izlenmekte olan işlemler sonraki ekran görüntüsünde listelenir. Yakalamak istediğiniz işlemler için onay kutularını seçin. Başka bir işlem listesine eklemek için işlem adı girin ve ardından **ekleme işlemi**.

![Kilitlenme bilgi dökümleri](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766026.png)

Daha fazla bilgi için bkz: [Microsoft Azure'da günlüğe kaydetme ve izleme denetimini ele](https://msdn.microsoft.com/magazine/ff714589.aspx) ve [Microsoft Azure tanılama bölümü 4: özel günlük bileşenleri ve Azure tanılama 1.3 değişiklikleri](http://justazure.com/microsoft-azure-diagnostics-part-4-custom-logging-components-azure-diagnostics-1-3-changes/).

## <a name="view-the-diagnostics-data"></a>Tanılama verilerini görüntüleyin
Bir bulut hizmeti veya sanal makine için tanılama verilerini derledik sonra görüntüleyebilirsiniz.

### <a name="to-view-cloud-service-diagnostics-data"></a>Bulut hizmeti tanılama verilerini görüntülemek için
1. Normal olarak, bulut hizmetinize dağıtın ve ardından çalıştırın.
2. Depolama hesabınızı Visual Studio'nun oluşturduğu bir raporda ya da tablolardaki tanılama verilerini görüntüleyebilirsiniz. Açık Cloud Explorer veya Sunucu Gezgini, rapor verilerinde açın ve ardından rol için düğümünün kısayol menüsünü görüntülemek için **görünüm tanılama verilerini**.
   
    ![Tanılama verilerini görüntüle](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748912.png)
   
    Kullanılabilir veri gösteren bir rapor görüntülenir.
   
    ![Visual Studio'da Microsoft Azure tanılama raporu](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796666.png)
   
    En son verileri gösterilmeyen transfer dönemi geçmesini bekleyin gerekebilir.
   
    Verileri hemen güncelleştirmek için seçin **yenileme** bağlantı. Otomatik olarak güncelleştirilen veri sağlamak için bir zaman aralığı seçin **otomatik yenileme** aşağı açılan liste kutusu. Hata verileri vermek için seçin **CSV'ye aktar** düğmesine bir Excel çalışma sayfasında açabilirsiniz bir virgülle ayrılmış değer dosyası oluşturun.
   
    Cloud Explorer veya Sunucu Gezgini içinde dağıtımla ilişkili depolama hesabını açın.
3. Tanılama tabloları Tablo Görüntüleyicisi'nde açın ve toplanan verileri gözden geçirin. Özel günlükler ve IIS günlüklerini bir blob kapsayıcısını açabilirsiniz. Aşağıdaki tabloda, tablolar veya farklı günlük dosyaları için verileri içeren blob kapsayıcıları listeler. Bu günlük dosyası için veri yanı sıra tablo girişleri **EventTickCount**, **Deploymentıd**, **rol**, ve **RoleInstance** , hangi sanal makine ve rol verileri oluşturulan tanımlamanıza yardımcı olması için ve ne zaman. 
   
   | Tanılama veri | Açıklama | Konum |
   | --- | --- | --- |
   | Uygulama günlükleri |Kodunuzu yöntemlerini çağırarak oluşturur günlükleri **System.Diagnostics.Trace** sınıfı. |WADLogsTable |
   | Olay günlükleri |Windows olay günlüklerini sanal makinelerde verileri. Windows bu günlüklerinde bilgilerini saklar, ancak uygulamalar ve hizmetler de hata günlüklerini kullanın veya bilgileri günlüğe kaydeder. |WADWindowsEventLogsTable |
   | Performans sayaçları |Sanal makinede kullanılabilir herhangi bir performans sayacı hakkında veri toplar. İşletim sistemi, bellek kullanımı ve işlemci süresi gibi birçok istatistikleri dahil performans sayaçları sağlar. |WADPerformanceCountersTable |
   | Altyapı günlükleri |Tanılama Altyapısı kendisini oluşturulan günlükleri. |WADDiagnosticInfrastructureLogsTable |
   | IIS günlükleri |Kayıt web istekleri günlüğe kaydedilir. Bulut hizmetiniz bir miktarda trafiği alır, bu günlükler uzun olabilir. Toplamak ve yalnızca gerektiğinde bu verileri depolamak için iyi bir fikirdir. |İsteği başarısız oldu blob kapsayıcısı wad-IIS-failedreqlogs, bu dağıtım, rol ve örneği için bir yol altında altında kaydeder bulabilirsiniz. Tam günlükleri wad IIS logfiles altında bulabilirsiniz. Her dosya için girişler WADDirectories tablosunda oluşturulur. |
   | Kilitlenme bilgi dökümleri |İkili bulut hizmetinizin işlemin (genellikle çalışan rolü) sağlar. |wad-crush-dumps blob container |
   | Özel günlük dosyaları |Günlükleri, önceden tanımlanmış veri. |Depolama hesabınızı kodda özel günlük dosyalarının konumu belirtebilirsiniz. Örneğin, bir özel blob kapsayıcısını belirtebilirsiniz. |
4. Herhangi bir türde veriler kesildi, bu verileri için arabellek boyutu artırmayı deneyebilirsiniz türü veya veri depolama hesabınıza aktarımları sanal makineden arasındaki aralığı kısaltmayı.
5. (İsteğe bağlı) Bazen genel depolama maliyetleri azaltmak için depolama hesabından verilerini temizle.
6. Tam bir dağıtım yaptığınızda, diagnostics.cscfg dosyası (.wadcfgx için Azure SDK 2.5) Azure'da güncelleştirilir ve bulut hizmetiniz tanılama yapılandırması değişiklikleri alır. Bunun yerine var olan bir dağıtıma güncelleştirirseniz, .cscfg dosyası Azure'da güncelleştirilmez. Tanılama ayarları, sonraki bölümde yer alan adımları izleyerek, ancak hala değiştirebilirsiniz. Tam dağıtımını gerçekleştirme ve var olan bir dağıtımını güncelleştirme hakkında daha fazla bilgi için bkz: [Azure uygulaması Yayımlama Sihirbazı](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-view-virtual-machine-diagnostics-data"></a>Sanal makine tanılama verilerini görüntülemek için
1. Sanal makine için kısayol menüsünden seçin **tanılama verilerini görüntüle**.
   
    ![Bir Azure sanal makinesinde tanılama verilerini görüntüle](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766027.png)
   
    **Tanılama özeti** iletişim kutusu görüntülenir.
   
    ![Azure sanal makinesi tanılama özeti](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796667.png)  
   
    En son verileri gösterilmeyen transfer dönemi geçmesini bekleyin gerekebilir.
   
    Verileri hemen güncelleştirmek için seçin **yenileme** bağlantı. Otomatik olarak güncelleştirilen veri sağlamak için bir zaman aralığı seçin **otomatik yenileme** aşağı açılan liste kutusu. Hata verileri vermek için seçin **CSV'ye aktar** düğmesine bir Excel çalışma sayfasında açabilirsiniz bir virgülle ayrılmış değer dosyası oluşturun.

## <a name="set-up-cloud-service-diagnostics-after-deployment"></a>Bulut hizmeti tanılama sonra dağıtım ayarlayın.
Zaten çalışmakta olan bir bulut hizmeti ile ilgili bir sorun çalışıyoruz, belirtmediğiniz veri toplamak isteyebilirsiniz, ilk olarak rolü dağıttığınız önce. Bu durumda, Sunucu Gezgininde ayarlarını değiştirerek bu verileri toplama başlatabilirsiniz. Tek bir örnek veya olup açtığınız bağlı olarak bir roldeki tüm örnekleri için tanılama ayarlayabilirsiniz **tanılama Yapılandırması** veya rol örneği için kısayol menüsünden iletişim kutusu. Rol düğümü yapılandırırsanız, yaptığınız değişiklikleri tüm örneklerine uygulanır. Örnek düğüm yapılandırırsanız, yaptığınız tüm değişiklikler yalnızca bu örnek için geçerlidir.

### <a name="to-set-up-diagnostics-for-a-running-cloud-service"></a>Çalışan bir bulut hizmeti için tanılama ayarlamak için
1. Server Explorer'da genişletin **bulut Hizmetleri** düğümünü genişletin ve ardından rolü veya örneği (veya her ikisi de) bulmak için düğüm listesi araştırmak istediğiniz.
   
    ![Tanılama Yapılandır](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748913.png)
2. Bir örnek düğümü ya da rolü düğümü için kısayol menüsünden seçin **güncelleştirme tanılama ayarları**ve ardından toplamak istediğiniz tanılama ayarlarını seçin.
   
    Yapılandırma ayarları hakkında daha fazla bilgi için bkz **tanılama veri kaynaklarını kurmak** bu makalede. Tanılama verilerini görüntüleme hakkında daha fazla bilgi için bkz **tanılama verilerini görüntülemek** bu makalede.
   
    Sunucu Gezgininde veri toplama değiştirirseniz, değişiklikler, bulut hizmeti tam olarak yeniden dağıtın kadar etkin kalır. Varsayılan kullanırsanız yayımlama ayarları, değişikliklerin üzerine yazılmaz. Varsayılan ayar yayımlama mevcut dağıtımı güncelleştirmek için yerine tam yeniden dağıtım yapmak için değil. Ayarları dağıtım sırasında temizleyin emin olmak için Git **Gelişmiş ayarları** Yayımlama Sihirbazı'nda sekmesini tıklatın ve ardından temizlemek **dağıtım güncelleştirme** onay kutusu. Bu onay kutusu temizlenmiş yeniden dağıtırken ayarları bu .wadcfgx (veya .wadcfg) dosyasındaki aracılığıyla olarak geri **özellikleri** Düzenleyicisi rolü. Dağıtımınızı güncelleştirirseniz, Azure önceki ayarlarını korur.

## <a name="troubleshoot-azure-cloud-service-issues"></a>Azure bulut hizmeti sorunlarını giderme
"Meşgul" durumunda, takılmış bir rolü sürekli geri dönüştürüldüğünde veya bir iç sunucu hatası oluşturur gibi bulut hizmeti projeleri sorunlar yaşıyorsanız, Araçlar ve tanılamak ve sorunu gidermek için kullanabileceğiniz teknikler vardır. Ortak sorunlar ve çözümleri belirli örnekleri için ve kavramları ve tanılamak ve bu hataları gidermek için kullanabileceğiniz araçlar genel bakış için bkz: [Azure PaaS işlem tanılama verilerini](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

## <a name="q--a"></a>Soru-Cevap
**Arabellek boyutu nedir ve ne kadar büyük olmalıdır?**

Her sanal makine örneği, ne kadar tanılama verilerini yerel dosya sisteminde depolanabilir kotaları sınırlayın. Ayrıca, her kullanılabilir tanılama veri türü için arabellek boyutu belirtin. Bu arabellek boyutu, veri türü için ayrı bir kota gibi davranır. Genel kota ve kalır bellek miktarını belirlemek için tanılama veri türü için iletişim kutusunun altına bakın. Daha büyük arabellek ya da daha fazla veri türlerini belirtirseniz, genel kota yaklaşımını. Genel kota diagnostics.wadcfg veya .wadcfgx yapılandırma dosyasını değiştirerek değiştirebilirsiniz. Tanılama verileri aynı dosya sistemi, uygulamanızın veri olarak depolanır. Uygulamanız çok miktarda disk alanı kullanıyorsa, genel tanılama kotasından artırmak döndürmemelidir.

**Transfer süresi nedir ve ne kadar süreyle olmalıdır?**

Transfer süresi dolduktan veriler arasında yakalar zaman miktarıdır. Her aktarım süresinden sonra verileri yerel dosya sisteminden bir sanal makinede depolama hesabınızdaki tablolara taşınır. Toplanan veri miktarını transfer dönemi sona önce kotayı aşarsa, eski verileri göz ardı edilir. Verilerinizi arabellek boyutu veya genel kota sınırını aştığı için veri kaybı aktarım süresini azaltmak isteyebilirsiniz.

**Hangi saat dilimi zaman damgaları, misiniz?**

Zaman damgaları, bulut hizmeti barındıran veri merkezinde yerel saat dilimindedir. Günlük tablolarda aşağıdaki üç zaman damgası sütunlarını kullanılır:

* **PreciseTimeStamp**: olayın ETW zaman damgası. Diğer bir deyişle, istemciden olayı günlüğe kaydeden saati.
* **Zaman damgası**: değeri **PreciseTimeStamp** yuvarlatılmış karşıya yükleme frekansı sınır aşağı kaydırın. Karşıya yükleme frekansı 5 dakika ve olay saat 00:17:12, örneğin, zaman damgası 00:15:00 ise.
* **Zaman damgası**: hangi varlık Azure tablosunda oluşturulduğu zaman damgası.

**Maliyetleri nasıl tanılama bilgisi toplarken yönetiyorsunuz?**

Varsayılan ayarları (**günlük düzeyi** kümesine **hata**, ve **Transfer dönemi** kümesine **1 dakika**) maliyetleri en aza indirmek için tasarlanmıştır. Daha fazla tanılama verisi toplama veya transfer dönemi azaltırsanız, işlem maliyetleri artırın. Gereksinim ve artık ihtiyacınız olduğunda veri toplamayı devre dışı bırakmak unutmayın daha fazla veri toplama. Her zaman yeniden çalışma zamanında bile bu makalenin önceki bölümlerinde açıklandığı şekilde etkinleştirebilirsiniz.

**IIS nasıl isteği başarısız oldu günlükleri toplamak?**

Varsayılan olarak IIS istek başarısız günlükleri toplamak değil. IIS web rolünüz için web.config dosyasını düzenleyerek isteği başarısız oldu günlükleri toplamak için ayarlayabilirsiniz.

**İzleme bilgilerini RoleEntryPoint yöntemlerden OnStart gibi alıyorum değil. Ne oldu?**

Yöntemlerinin **RoleEntryPoint** IIS içinde değil WAIISHost.exe bağlamında denir. Yapılandırma bilgilerini izlemeyi etkinleştirir normalde uygulanmaz web.config dosyasında. Bu sorunu çözmek için web rolü projenize .config dosyasına ekleyin ve içeren çıkış bütünleştirilmiş eşleştirilecek dosya adı **RoleEntryPoint** kodu. Varsayılan web rolü projesinde WAIISHost.exe.config .config dosyası adı olmalıdır. Bu dosyayı aşağıdaki satırları ekleyin:

```
<system.diagnostics>
  <trace>
      <listeners>
          <add name “AzureDiagnostics” type=”Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener”>
              <filter type=”” />
          </add>
      </listeners>
  </trace>
</system.diagnostics>
```

İçinde **özellikleri** penceresindeki ayarlayın **çıktı dizinine Kopyala** özelliğine **her zaman Kopyala**.

## <a name="next-steps"></a>Sonraki adımlar
Azure'da oturum Tanılama hakkında daha fazla bilgi için bkz: [Azure Cloud Services ve sanal makineleri tanılamayı etkinleştirme](cloud-services/cloud-services-dotnet-diagnostics.md) ve [Azure App Service'te Web uygulamalarını için tanılama günlüğünü etkinleştirme](app-service/web-sites-enable-diagnostic-log.md).

