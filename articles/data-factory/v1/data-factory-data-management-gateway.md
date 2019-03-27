---
title: Veri Yönetimi ağ geçidi Data Factory | Microsoft Docs
description: Şirket içi ve bulut arasında veri taşımak için bir veri ağ geçidi ayarlama. Veri Yönetimi ağ geçidi, Azure Data Factory'de veri taşımak için kullanın.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.assetid: b9084537-2e1c-4e96-b5bc-0e2044388ffd
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: abnarain
robots: noindex
ms.openlocfilehash: 00c8d7cefd7539cd53de8081f44fe861bd063bee
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58487796"
---
# <a name="data-management-gateway"></a>Veri Yönetimi Ağ Geçidi
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [barındırılan tümleştirme çalışma zamanını içinde](../create-self-hosted-integration-runtime.md).

> [!NOTE]
> Veri Yönetimi ağ geçidi artık şirket içinde barındırılan Integration Runtime yeni marka adları verilmiştir.

Veri Yönetimi ağ geçidi, şirket içi ortamınızda kopyalamak için yüklemeniz gereken bir istemci aracısıdır Bulut ve şirket içi veri depoları arasında veri. Data Factory tarafından desteklenen depolarının listelenir şirket içi veri [desteklenen veri kaynakları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) bölümü.

Bu makalede izlenecek yolda tamamlar [şirket içi ile bulut arasında veri taşıma veri depoları](data-factory-move-data-between-onprem-and-cloud.md) makalesi. Bu izlenecek yolda, verileri bir şirket içi SQL Server veritabanından Azure blobuna taşımak için ağ geçidini kullanan bir işlem hattı oluşturun. Bu makalede, veri yönetimi ağ geçidi hakkında ayrıntılı bilgi sağlar.

Birden çok şirket içi makine ile ağ geçidi ile ilişkilendirilmesi yoluyla veri yönetimi ağ geçidi ölçeklendirebilirsiniz. Ölçeklendirebileceğiniz yukarı bir düğümde eşzamanlı olarak çalışabilecek veri taşıma işlerinin sayısını artırarak. Bu özellik, tek bir düğüm ile mantıksal bir ağ geçidi için de kullanılabilir. Bkz: [ölçeklendirme veri yönetimi ağ geçidi Azure Data factory'de](data-factory-data-management-gateway-high-availability-scalability.md) makale Ayrıntılar için.

> [!NOTE]
> Şu anda ağ geçidi veri fabrikasında kopyalama etkinliği, saklı yordam etkinliği yalnızca destekler. Şirket içi veri kaynaklarına erişmek için özel bir etkinlik ağ geçidinden kullanmak mümkün değildir.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="overview"></a>Genel Bakış
### <a name="capabilities-of-data-management-gateway"></a>Veri Yönetimi ağ geçidi özellikleri
Veri Yönetimi ağ geçidi, aşağıdaki özellikleri sağlar:

* Şirket içi veri kaynaklarına model, aynı data factory içinde bulunan veri kaynaklarına Bulut ve veri taşıma.
* Cam izleme ve yönetim ağ geçidi durumu Data Factory sayfasından görünürlük için tek bir bölme vardır.
* Şirket içi veri kaynaklarına erişimi güvenli bir şekilde yönetin.
  * Kurumsal güvenlik duvarı için gereken değişiklik yok. Ağ geçidi, yalnızca açık İnternete giden HTTP tabanlı bağlantılar sağlar.
  * Şirket içi veri depolarınıza sertifikanızla için kimlik bilgilerini şifreler.
* Verileri verimli bir şekilde taşıma - veri aktarılır paralel olarak dayanıklı aralıklı olarak ortaya çıkan ağ sorunları otomatik yeniden deneme mantığı.

### <a name="command-flow-and-data-flow"></a>Komut akışını ve veri akışı
Şirket içi ve bulut arasında veri kopyalamak için kopyalama etkinliğini kullandığınızda, etkinlik bir ağ geçidi bulut geçme veya tam tersi şirket içi veri kaynağından veri aktarımı için kullanır.

Üst düzey veri akışı için ve veri ağ geçidi ile kopyalama adımları özeti aşağıda verilmiştir: ![Ağ geçidi kullanarak veri akışı](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)

1. Veri Geliştirici oluşturur bir ağ geçidi kullanarak bir Azure Data Factory'deki [Azure portalında](https://portal.azure.com) veya [PowerShell cmdlet'i](https://docs.microsoft.com/powershell/module/az.datafactory/).
2. Veri geliştirici, ağ geçidi belirterek bir şirket içi veri deposu için bağlı hizmet oluşturur. Bağlı hizmet oluşturma işleminin bir parçası olarak, kimlik doğrulama türleri ve kimlik bilgilerini belirtmek için kimlik bilgilerini ayarlama uygulama veri geliştiricisi kullanır. Kimlik bilgilerini ayarlama uygulama iletişim bağlantı ve kimlik bilgilerini kaydetmek için ağ geçidi test etmek için veri deposuyla iletişim kurar.
3. Ağ Geçidi kimlik bilgileri bulutta kaydetmeden önce (veri geliştiricisi tarafından sağlanan), ağ geçidi ile ilişkili sertifika ile kimlik bilgilerini şifreler.
4. Data Factory hizmeti ağ geçidi için planlama ve yönetim işlerinin bir paylaşılan Azure service bus kuyruğu kullanan bir denetim kanalı aracılığıyla iletişim kurar. Bir kopyalama etkinliği işi çalıştırmasının başlatılması gerektiğinde, Data Factory istekle birlikte kimlik bilgilerini sıralar. Ağ geçidi, işi kuyruğa yoklama sonra başlatıyor.
5. Ağ geçidi aynı sertifika ile kimlik bilgilerinin şifresini çözer ve uygun kimlik doğrulama türü ve kimlik bilgileri ile şirket içi veri deposu bağlanır.
6. Ağ geçidi verileri bulut depolamaya veya tam tersi veri işlem hattında kopyalama etkinliği nasıl yapılandırıldığına bağlı olarak bir şirket içi depolama alanından kopyalar. Bu adım için ağ geçidi doğrudan Azure Blob Depolama gibi bulut tabanlı depolama hizmetlerinde güvenli (HTTPS) kanal üzerinden iletişim kurar.

### <a name="considerations-for-using-gateway"></a>Ağ geçidi kullanma konuları
* Tek bir veri yönetimi ağ geçidi örneğinde birden çok şirket içi veri kaynakları için kullanılabilir. Ancak, **tek bir ağ geçidinin yalnızca bir Azure data factory'ye bağlı** ve başka bir data factory ile paylaşılamaz.
* Sahip olduğunuz **veri yönetimi ağ geçidi yalnızca bir örneğini** tek bir makinede yüklü. Şirket içi veri kaynaklarına erişmesi gereken iki veri fabrikaları sahip varsayalım, ağ geçitleri iki şirket içi bilgisayara yüklemeniz gerekir. Diğer bir deyişle, bir ağ geçidi için belirli bir veri fabrikası bağlıdır
* **Ağ geçidi veri kaynağı ile aynı makinede olması gerekmez**. Ancak, veri kaynağı yakın ağ geçidi, ağ geçidinin veri kaynağına bağlanmak süreyi azaltır. Bir şirket içi veri kaynağı barındıran farklı bir makinede ağ geçidi yüklemenizi öneririz. Ağ geçidi ve veri kaynağı, farklı makinelerde ağ geçidi kaynaklarının veri kaynağı ile rekabet edemez.
* Sahip olduğunuz **farklı makinelerde aynı şirket içi veri kaynağına bağlanan birden fazla ağ geçidi**. Örneğin, iki veri fabrikaları hizmet veren iki ağ geçidi olabilir ancak aynı şirket içi veri kaynağı ile veri fabrikaları hem kayıtlı.
* Bilgisayar işlevi gören üzerinde yüklü bir ağ geçidi zaten varsa bir **Power BI** senaryo, yükleme bir **Azure Data Factory için ayrı bir ağ geçidi** başka bir makine üzerinde.
* Ağ geçidi kullandığınızda da kullanılmalıdır **ExpressRoute**.
* Veri kaynağı (bir güvenlik duvarının arkasında olan) bir şirket içi veri kaynağı olarak davran kullanırken bile **ExpressRoute**. Ağ Geçidi Hizmeti ve veri kaynağı arasında bağlantı kurmak için kullanın.
* Yapmanız gerekenler **ağ geçidini kullanmak** üzerinde bulutta veri depolama alanı olsa bile bir **Azure Iaas VM**.

## <a name="installation"></a>Yükleme
### <a name="prerequisites"></a>Önkoşullar
* Desteklenen **işletim sistemi** sürümleridir Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2. Veri Yönetimi ağ geçidi etki alanı denetleyicisine yüklenmesi şu anda desteklenmiyor.
* .NET framework 4.5.1 veya üzeri gereklidir. Ağ geçidi bir Windows 7 makinesinde yüklüyorsanız, .NET Framework 4.5 veya sonraki bir sürümü yükleyin. Bkz: [.NET Framework System Requirements](https://msdn.microsoft.com/library/8z6watww.aspx) Ayrıntılar için.
* Önerilen **yapılandırma** ağ geçidi için en az 2 GHz, 4 çekirdek, 8 GB RAM ve 80 GB disk makinedir.
* Konak makine hazırda bekleme, ağ geçidi veri isteklere yanıt vermez. Bu nedenle, uygun bir yapılandırma **güç planı** ağ geçidi yüklemeden önce bilgisayarda. Makine hazırda bekleme için yapılandırılmışsa, ağ geçidi yüklemesi isteyen bir ileti alırsınız.
* Makinede yüklemek ve veri yönetimi ağ geçidi başarıyla yapılandırmak için bir yönetici olması gerekir. Ek kullanıcılar ekleyebilirsiniz **veri yönetimi ağ geçidi kullanıcıları** yerel Windows grubu. Bu grubun üyelerinin kullanabildiği **veri yönetimi ağ geçidi Yapılandırma Yöneticisi'ni** ağ geçidini yapılandırmak için aracı.

Kopyalama etkinliği çalıştırma üzerinde belirli bir sıklıkta gerçekleşecek şekilde makinesinde kaynak kullanımı (CPU, bellek), ayrıca en yüksek ve boş kalma sürelerinde ile aynı deseni izler. Kaynak Kullanımı Yoğun taşınan veri miktarı da bağlıdır. Birden çok kopyası işleri devam ederken, kaynak kullanımı yoğun zamanlarda Yukarı Git bakın.

### <a name="installation-options"></a>Yükleme Seçenekleri
Veri Yönetimi ağ geçidi, aşağıdaki yollarla yüklenebilir:

* Bir MSI Kurulumu paketinden indirerek [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717). MSI, korunan tüm ayarlar ile mevcut veri yönetimi ağ geçidi en son sürüme yükseltmek için de kullanılabilir.
* Tıklayarak **veri ağ geçidi yükleyip** el ile Kurulum altındaki bağlantıyı veya **doğrudan bu bilgisayara yüklemek** hızlı KURULUMUNU altında. Bkz: [şirket içi ile bulut arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale hızlı kurulum kullanarak ilişkin adım adım yönergeler. El ile adım indirme merkezine alır. Sonraki bölümde indirme ve ağ geçidi Yükleme Merkezi'nden yükleme yönergeleri verilmiştir.

### <a name="installation-best-practices"></a>Yükleme için en iyi yöntemler:
1. Böylece makine olmayan hazırda bekleme ağ geçidi için konak makinedeki güç planı yapılandırmak. Konak makine hazırda bekleme, ağ geçidi veri isteklere yanıt vermez.
2. Ağ geçidi ile ilişkili sertifika yedekleyin.

### <a name="install-the-gateway-from-download-center"></a>Ağ geçidi İndirme Merkezi'nden yükleyin
1. Gidin [Microsoft Veri Yönetimi ağ geçidi indirme sayfasına](https://www.microsoft.com/download/details.aspx?id=39717).
2. Tıklayın **indirme**seçin **64-bit** sürüm (32-bit artık desteklenir), tıklatıp **sonraki**.
3. Çalıştırma **MSI** doğrudan veya sabit disk ve çalışma kaydedin.
4. Üzerinde **Hoş Geldiniz** sayfasında, bir **dil** tıklayın **sonraki**.
5. **Kabul** tıklayın ve son kullanıcı lisans sözleşmesi **sonraki**.
6. Seçin **klasör** ağ geçidi yükleyip tıklayın **sonraki**.
7. Üzerinde **yüklenmeye hazır** sayfasında **yükleme**.
8. Tıklayın **son** yüklemeyi tamamlamak için.
9. Anahtarı Azure portalından alın. Adım adım yönergeler için sonraki bölüme bakın.
10. Üzerinde **kayıt ağ geçidi** sayfasının **veri yönetimi ağ geçidi Yapılandırma Yöneticisi'ni** , makine üzerinde çalışan, aşağıdaki adımları uygulayın:
    1. Anahtar metnini yapıştırın.
    2. İsteğe bağlı olarak, tıklayın **Show ağ geçidi anahtarı** anahtar metnini görmek için.
    3. Tıklayın **kaydetme**.

### <a name="register-gateway-using-key"></a>Anahtarını kullanarak ağ geçidini kaydetme
#### <a name="if-you-havent-already-created-a-logical-gateway-in-the-portal"></a>Portalda mantıksal bir ağ geçidi oluşturmadıysanız
Portalda bir ağ geçidi oluşturma ve anahtarı almak için **yapılandırma** izlenecek yolda adımları takip edin sayfasında [şirket içi ile bulut arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makalesi.

#### <a name="if-you-have-already-created-the-logical-gateway-in-the-portal"></a>Portalda mantıksal ağ geçidi oluşturduysanız
1. Azure portalında gidin **Data Factory** sayfasında ve tıklayın **bağlı hizmetler** Döşe.

    ![Veri Fabrikası sayfası](media/data-factory-data-management-gateway/data-factory-blade.png)
2. İçinde **bağlı hizmetler** sayfasında, mantıksal **ağ geçidi** portalda oluşturduğunuz.

    ![mantıksal ağ geçidi](media/data-factory-data-management-gateway/data-factory-select-gateway.png)
3. İçinde **veri ağ geçidi** sayfasında **veri ağ geçidi yükleyip**.

    ![İndirme bağlantısı portalında](media/data-factory-data-management-gateway/download-and-install-link-on-portal.png)
4. İçinde **yapılandırma** sayfasında **yeniden oluşturun anahtar**. Dikkatli bir şekilde okuduktan sonra uyarı iletisi üzerinde Evet'e tıklayın.

    ![Anahtarı yeniden oluşturun](media/data-factory-data-management-gateway/recreate-key-button.png)
5. Anahtar yanındaki Kopyala düğmesine tıklayın. Anahtar, panoya kopyalanır.

    ![Anahtarı kopyalama](media/data-factory-data-management-gateway/copy-gateway-key.png)

### <a name="system-tray-icons-notifications"></a>Sistem Tepsisi Simgeleri / bildirimleri
Aşağıdaki görüntüde gördüğünüz Tepsisi Simgeleri bazıları gösterilmektedir.

![Sistem Tepsisi Simgeleri](./media/data-factory-data-management-gateway/gateway-tray-icons.png)

Sistem tepsisi simgesi/bildirim iletisi İmleç bir açılan pencere ağ geçidi/update işleminde durumuyla ilgili ayrıntıları görürsünüz.

### <a name="ports-and-firewall"></a>Bağlantı noktaları ve güvenlik duvarı
Göz önünde bulundurmanız gereken iki güvenlik duvarı vardır: **Kurumsal güvenlik duvarınız** kuruluşun merkezi yönlendirici üzerinde çalışan ve **Windows Güvenlik Duvarı** ağ geçidinin bulunduğu yerel makinede bir arka plan olarak yapılandırılmış yüklü.

![güvenlik duvarları](./media/data-factory-data-management-gateway/firewalls2.png)

Kurumsal güvenlik duvarınız düzeyinde aşağıdaki etki alanları ve giden bağlantı noktalarını yapılandırmak:

| Etki alanı adları | Bağlantı Noktaları | Açıklama |
| --- | --- | --- |
| *.servicebus.windows.net |443 |Veri taşıma Hizmeti'nde arka ucu ile iletişim kurmak için kullanılır |
| *. core.windows.net |443 |Azure Blob (yapılandırılmışsa) kullanarak hazırlanmış kopya için kullanılan|
| *.frontend.clouddatahub.net |443 |Veri taşıma Hizmeti'nde arka ucu ile iletişim kurmak için kullanılır |
| *.servicebus.windows.net |9350-9354, 5671 |Kopyalama Sihirbazı tarafından kullanılan TCP üzerinden isteğe bağlı bir service bus geçişi |

Windows Güvenlik Duvarı düzeyinde şu giden bağlantı noktaları genellikle etkindir. Etki alanları ve bağlantı noktalarını uygun şekilde yapılandırabilirsiniz, varsa ağ geçidi makinesi üzerinde.

> [!NOTE]
> 1. Kaynağınız üzerinde temel / havuzlarını sahip olabileceğiniz beyaz liste ek etki alanları ve giden bağlantı noktaları, Kurumsal/Windows Güvenlik Duvarı'nda.
> 2. Bazı bulut veritabanları için (örneğin: [Azure SQL veritabanı](https://docs.microsoft.com/azure/sql-database/sql-database-configure-firewall-settings), [Azure Data Lake](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-secure-data#set-ip-address-range-for-data-access), vs.), beyaz liste IP adresini güvenlik duvarı yapılandırmalarını ağ geçidi makinesinde gerekebilir.
>
>

#### <a name="copy-data-from-a-source-data-store-to-a-sink-data-store"></a>Bir kaynak veri deposundan bir havuz veri deposuna veri kopyalamak
Güvenlik duvarı kuralları düzgün bir şekilde şirket güvenlik duvarı, ağ geçidi makinesindeki Windows Güvenlik Duvarı etkinleştirilir ve kendi veri deposu emin olun. Bu kurallar etkinleştirilmesi, ağ geçidinin iki kaynağına bağlanmak ve başarıyla havuz için sağlar. Kopyalama işleminin katılan her veri deposunun kurallarını etkinleştirin.

Örneğin, kopyalama için **bir Azure SQL veritabanı havuz veya bir Azure SQL veri ambarı havuzu bir şirket içi veri deposuna**, aşağıdaki adımları uygulayın:

* Gidene izin ver **TCP** bağlantı noktasında iletişime **1433** Windows Güvenlik Duvarı hem de kurumsal bir güvenlik duvarı için.
* İzin verilen IP adreslerinin listesi için ağ geçidi makinesinin IP adresi eklemek için Azure SQL sunucusunun güvenlik duvarı ayarlarını yapılandırın.

> [!NOTE]
> Güvenlik duvarınızı giden bağlantı noktası 1433 izin vermediği durumlarda ağ geçidi doğrudan Azure SQL erişemiyor. Bu durumda, kullanabilir [hazırlanmış kopya](https://docs.microsoft.com/azure/data-factory/data-factory-copy-activity-performance#staged-copy) SQL Azure veritabanına / SQL Azure DW. Bu senaryoda, veri taşıma işlemi için yalnızca HTTPS (443 numaralı bağlantı noktası) gerekir.
>
>

### <a name="proxy-server-considerations"></a>Proxy server konuları
Kurumsal ağ ortamınızı, internet'e bir proxy sunucusu kullanıyorsa, uygun proxy ayarlarını kullanmak için veri yönetimi ağ geçidi'ni yapılandırın. Proxy ilk kayıt aşamasında ayarlayabilirsiniz.

![Kayıt sırasında küme proxy](media/data-factory-data-management-gateway/SetProxyDuringRegistration.png)

Ağ geçidi, bulut hizmetine bağlanmak için proxy sunucusunu kullanır. Tıklayın **değişiklik** ilk kurulum sırasında bağlantı. Gördüğünüz **proxy ayarı** iletişim.

![Yapılandırma Yöneticisi'ni kullanarak küme proxy](media/data-factory-data-management-gateway/SetProxySettings.png)

Üç yapılandırma seçeneği vardır:

* **Proxy kullanmayın**: Ağ geçidi açıkça her Proxy'yi bulut hizmetlerine bağlanmak için kullanmaz.
* **Sistem Ara sunucu kullanmak**: Ağ geçidi diahost.exe.config ve diawp.exe.config yapılandırılan proxy ayarı kullanır. Proxy diahost.exe.config ve diawp.exe.config yapılandırılmışsa, Ara sunucu üzerinden geçmeden doğrudan ağ geçidi bulut hizmetine bağlanır.
* **Özel ara sunucu kullanmak**: HTTP proxy yapılandırması diahost.exe.config ve diawp.exe.config kullanma yerine ağ geçidi için kullanılacak ayarı yapılandırın. Adres ve bağlantı noktası gereklidir. Kullanıcı adı ve parola, proxy kimlik doğrulama ayarlarına bağlı olarak isteğe bağlıdır. Tüm ayarları Ağ Geçidi kimlik bilgisi sertifikası ile şifrelenir ve ağ geçidi ana makinede yerel olarak depolanır.

Veri Yönetimi ağ geçidi ana bilgisayar hizmeti, güncelleştirilen proxy ayarlarını kaydettikten sonra otomatik olarak başlatır.

Ara sunucu ayarlarını görüntüleme veya güncelleştirme istiyorsanız ağ geçidi başarıyla, kaydedildikten sonra veri yönetimi ağ geçidi Yapılandırma Yöneticisi'ni kullanın.

1. Başlatma **veri yönetimi ağ geçidi Yapılandırma Yöneticisi'ni**.
2. **Ayarlar** sekmesine geçin.
3. Tıklayın **değişiklik** bağlantısını **HTTP Proxy** başlatmak için bölüm **Set HTTP Proxy** iletişim.
4. Tıkladıktan sonra **sonraki** düğmesi, proxy ayarını kaydedin ve ağ geçidi konak hizmetini yeniden başlatmak için izninizi isteyen bir uyarı iletişim kutusu görürsünüz.

Görüntüleyebilir ve Configuration Manager aracını kullanarak HTTP Ara sunucusunu güncelleştirin.

![Yapılandırma Yöneticisi'ni kullanarak küme proxy](media/data-factory-data-management-gateway/SetProxyConfigManager.png)

> [!NOTE]
> NTLM kimlik doğrulaması ile bir proxy sunucusu ayarlayın, ağ geçidi ana bilgisayar hizmeti etki alanı hesabı altında çalışır. Daha sonra etki alanı hesabı için parolayı değiştirirseniz, hizmeti için yapılandırma ayarlarını güncelleştirme ve buna göre yeniden unutmayın. Bu gereksinimi nedeniyle parolanızı sık güncelleştirme gerektirmez proxy sunucusuna erişmek için bir özel etki alanı hesabı kullanmanızı öneririz.
>
>

### <a name="configure-proxy-server-settings"></a>Ara sunucu ayarlarını yapılandırma
Seçerseniz **sistem Ara sunucu kullanmak** HTTP proxy ayarı, ağ geçidi proxy diahost.exe.config ve diawp.exe.config ayarını kullanır. Proxy diahost.exe.config ve diawp.exe.config belirtilirse, Ara sunucu üzerinden geçmeden doğrudan ağ geçidi bulut hizmetine bağlanır. Aşağıdaki yordam diahost.exe.config dosyayı güncelleştirmek için yönergeler sağlar.

1. Dosya Gezgini'nde, özgün dosyasını yedeklemek için C:\Program Files\Microsoft veri yönetimi Gateway\2.0\Shared\diahost.exe.config güvenli bir kopyasını oluşturun.
2. Yönetici olarak çalıştırdığınızdan Notepad.exe başlatın ve "C:\Program Files\Microsoft veri yönetimi Gateway\2.0\Shared\diahost.exe.config. metin dosyasını açın Aşağıdaki kodda gösterildiği gibi varsayılan etiket için system.net bulun:

    ```
    <system.net>
        <defaultProxy useDefaultCredentials="true" />
    </system.net>
    ```

    Ardından, aşağıdaki örnekte gösterildiği gibi proxy sunucusu ayrıntılarının ekleyebilirsiniz:

    ```
    <system.net>
        <defaultProxy enabled="true">
            <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
        </defaultProxy>
    </system.net>
    ```

    Ek özellikler scriptLocation gibi gerekli ayarları belirlemek için proxy etiketin içinde izin verilir. Başvurmak [proxy öğesi (ağ ayarları)](https://msdn.microsoft.com/library/sa91de1e.aspx) söz dizimi hakkında.

    ```
    <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
    ```
3. Özgün konumuna yapılandırma dosyasını kaydedin ve ardından değişiklikleri toplar veri yönetimi ağ geçidi konak hizmetini yeniden başlatın. Hizmetini yeniden başlatmak için: Denetim Masası'ndan ya da hizmetler uygulamasını kullanın **veri yönetimi ağ geçidi Yapılandırma Yöneticisi'ni** > tıklayın **Hizmeti Durdur** düğmesine ve ardından tıklayın **Başlat Hizmet**. Hizmet başlatılmazsa, bir XML etiket sözdizimi yanlış düzenlendiğinde uygulama yapılandırma dosyasına eklendi olasıdır.

> [!IMPORTANT]
> Güncelleştirilecek unutmadığınızdan **hem** diahost.exe.config ve diawp.exe.config.

Bu noktaları ek olarak, ayrıca Microsoft Azure, şirketinizin izin verilenler listesinde olduğundan emin olmak gerekir. Geçerli Microsoft Azure IP adreslerinin listesi indirilebileceğini [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).

#### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a>Olası Belirtiler güvenlik duvarınızdan ve Ara sunucu ilgili sorunlar
Aşağıdaki ayarlara benzer hatalarla karşılaşırsanız, kendi kimliğini doğrulamak için ağ geçidinin veri Fabrikasına bağlanmasını engelleyen güvenlik duvarı veya Ara sunucunun yanlış yapılandırması nedeniyle olasıdır. Güvenlik duvarınızı emin olmak için önceki bölüme bakın ve proxy sunucusu düzgün yapılandırılmış.

1. Ağ geçidini kaydetmek çalıştığınızda şu hatayı alıyorsunuz: "Ağ geçidi anahtarı kaydedilemedi. Ağ geçidi anahtarı kaydettirmeyi yeniden denemeden önce veri yönetimi ağ geçidi bağlı bir durumda ve veri yönetimi ağ geçidi ana bilgisayar hizmetinin başlatıldığından emin olun."
2. Yapılandırma Yöneticisi'ni açın, durum "Bağlantı kesildi" veya "Bağlanıyor" görürsünüz Windows olay günlükleri, "Olay Görüntüleyicisi" görüntülerken > "Uygulama ve hizmet günlükleri" > "Veri yönetimi ağ geçidi", şu hata gibi hata iletilerine bakın: `Unable to connect to the remote server`
   `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`

### <a name="open-port-8050-for-credential-encryption"></a>Kimlik bilgilerinin şifrelenebilmesi için 8050 bağlantı noktasını açın
**Kimlik bilgilerini ayarlama** uygulamanın kullandığı gelen bağlantı noktası **8050** geçiş kimlik bilgilerini Azure portalında bir şirket bağlantılı hizmet ayarladığınızda ağ geçidine. Ağ geçidi Kurulum sırasında varsayılan olarak, ağ geçidi yüklemesi, ağ geçidi makinesinde açılır.

Bir üçüncü taraf güvenlik duvarı kullanıyorsanız, bağlantı noktası 8050 el ile açabilirsiniz. Ağ geçidi Kurulum sırasında güvenlik duvarı sorunu çalıştırırsanız, güvenlik duvarını yapılandırma olmadan ağ geçidi yüklemek için aşağıdaki komutu kullanarak deneyebilirsiniz.

    msiexec /q /i DataManagementGateway.msi NOFIREWALL=1

Ağ geçidi makinesinde 8050 bağlantı noktası açık değil kullanmayı tercih ederseniz kullanarak dışında mekanizmaları kullanma **kimlik bilgilerini ayarlama** uygulama veri deposu kimlik bilgilerini yapılandırmak için. Örneğin, kullanabileceğinizi [yeni AzDataFactoryEncryptValue](https://docs.microsoft.com/powershell/module/az.datafactory/new-azdatafactoryencryptvalue) PowerShell cmdlet'i. Bkz. nasıl veri kimlik bilgilerini depolama kimlik bilgilerini ayarlama ve güvenlik bölümü ayarlanabilir.

## <a name="update"></a>Güncelleştirme
Varsayılan olarak, veri yönetimi ağ geçidi, ağ geçidini daha yeni bir sürümü kullanılabilir olduğunda otomatik olarak güncelleştirilir. Ağ geçidi, tüm zamanlanmış görevlerin tümü tamamlanıncaya kadar güncelleştirilmez. Güncelleştirme işlemi tamamlanana kadar başka bir görev ağ geçidi tarafından işlenir. Güncelleştirme başarısız olursa, ağ geçidi eski sürüme geri alınır.

Zamanlanan güncelleştirme zamanı aşağıdaki konumlarda görürsünüz:

* Ağ geçidi özellikleri sayfasında Azure portalı.
* Veri Yönetimi ağ geçidi Configuration Manager'ın ana sayfası
* Sistem tepsisi bildirimi iletisi.

Veri Yönetimi ağ geçidi Yapılandırma Yöneticisi'nin Giriş sekmesinde güncelleştirme zamanlamasını görüntüler ve son ağ geçidinin yüklü/güncelleştirildi.

![Güncelleştirmeleri zamanlama](media/data-factory-data-management-gateway/UpdateSection.png)

Güncelleştirmeyi hemen yükleyin veya planlanan zamanda otomatik olarak güncelleştirilecek ağ geçidi için bekleyin. Örneğin, aşağıdaki resimde hemen yüklemek için tıklayabileceği güncelleştir düğmesine yanı sıra ağ geçidi Yapılandırma Yöneticisi'nde gösterilen bildirim iletisi gösterilir.

![DMG Configuration Manager'da güncelleştirme](./media/data-factory-data-management-gateway/gateway-auto-update-config-manager.png)

Sistem tepsisi bildirimi iletisinde aşağıdaki görüntüde gösterildiği gibi görünür:

![Sistem tepsisi iletisi](./media/data-factory-data-management-gateway/gateway-auto-update-tray-message.png)

Sistem tepsisindeki güncelleştirme işlemi (el ile veya otomatik) durumunu görürsünüz. Configuration Manager ağ geçidi sonraki zaman başlatıldığında, bir ileti ağ geçidi bağlantısı ile birlikte güncelleştirilmiş bildirim çubuğu görürsünüz [yeni konu nedir](data-factory-gateway-release-notes.md).

### <a name="to-disableenable-auto-update-feature"></a>Otomatik güncelleştirme özelliğini devre dışı bırak/etkinleştir
Devre dışı bırak/otomatik güncelleştirme özelliği aşağıdaki adımları uygulayarak etkinleştirebilirsiniz:

[Tek düğümlü ağ geçidi için]
1. Ağ geçidi makinesinde Windows PowerShell'i başlatın.
2. C:\Program Files\Microsoft tümleştirme Runtime\3.0\PowerShellScript\ klasöre geçin.
3. Özellik (devre dışı bırakın) otomatik güncelleştirmesini etkinleştirmek için aşağıdaki komutu çalıştırın.

    ```powershell
    .\IntegrationRuntimeAutoUpdateToggle.ps1 -off
    ```
4. Yeniden açmak için:

    ```powershell
    .\IntegrationRuntimeAutoUpdateToggle.ps1 -on
    ```
   [Yüksek oranda kullanılabilir ve ölçeklenebilir çok düğümlü gateway için](data-factory-data-management-gateway-high-availability-scalability.md)
1. Ağ geçidi makinesinde Windows PowerShell'i başlatın.
2. C:\Program Files\Microsoft tümleştirme Runtime\3.0\PowerShellScript\ klasöre geçin.
3. Özellik (devre dışı bırakın) otomatik güncelleştirmesini etkinleştirmek için aşağıdaki komutu çalıştırın.

    Yüksek oranda kullanılabilirlik özelliği ile ağ geçidi için ek bir AuthKey param gereklidir.
    ```powershell
    .\IntegrationRuntimeAutoUpdateToggle.ps1 -off -AuthKey <your auth key>
    ```
4. Yeniden açmak için:

    ```powershell
    .\IntegrationRuntimeAutoUpdateToggle.ps1 -on -AuthKey <your auth key>
    ```

## <a name="configuration-manager"></a>Configuration Manager
Ağ geçidini yükledikten sonra veri yönetimi ağ geçidi Yapılandırma Yöneticisi'ni aşağıdaki yollardan birini başlatabilirsiniz:

1. İçinde **arama** penceresinde, tür **veri yönetimi ağ geçidi** bu yardımcı programına erişmek için.
2. Yürütülebilir dosyayı çalıştırmak **ConfigManager.exe** klasöründeki: **C:\Program Files\Microsoft veri yönetimi Gateway\2.0\Shared**

### <a name="home-page"></a>Giriş sayfası
Giriş sayfası aşağıdaki eylemleri gerçekleştirmenize izin verir:

* (VS bulut hizmetine bağlı) ağ geçidinin durumunu görüntüleyin.
* **Kayıt** portalından bir anahtar kullanarak.
* **Durdur** ve başlangıç **veri yönetimi ağ geçidi ana bilgisayar hizmeti** ağ geçidi makinesinde.
* **Güncelleştirmeleri zamanla** gün belirli bir zamanda.
* Ağ geçidi ne zaman son tarihi görüntüleyebilirsiniz **son güncelleştirme**.

### <a name="settings-page"></a>Ayarlar sayfası
Ayarlar sayfasını aşağıdaki eylemleri gerçekleştirmenize izin verir:

* Görüntüleme, değiştirme ve dışarı aktarma **sertifika** ağ geçidi tarafından kullanılır. Bu sertifika, veri kaynağı kimlik bilgilerini şifrelemek için kullanılır.
* Değişiklik **HTTPS bağlantı noktası** uç noktası için. Ağ geçidi, veri kaynağı kimlik bilgilerini ayarlamak için bir bağlantı noktası açar.
* **Durum** uç noktası
* Görünüm **SSL sertifikası** portalı ve ağ geçidi arasında SSL iletişimi için veri kaynakları için kimlik bilgilerini ayarlamak için kullanılır.

### <a name="remote-access-from-intranet"></a>Intranet'ten uzaktan erişim
Bu işlev gelecekteki etkinleştirilecektir. Gelecek güncelleştirmelerde (v3.4 veya üzeri) biz, etkinleştir / gerçekleşen bugün kimlik bilgilerini şifrelemek için PowerShell veya kimlik bilgileri Yöneticisi uygulamasını kullanırken bağlantı noktası (yukarıdaki bölümüne bakın) 8050 kullanarak herhangi bir uzak bağlantı devre dışı bırak olanak tanır.

### <a name="diagnostics-page"></a>Tanılama sayfası
Tanılama sayfası aşağıdaki eylemleri gerçekleştirmenize izin verir:

* Ayrıntılı etkinleştir **günlüğü**, günlükleri Olay Görüntüleyicisi'nde görüntülemek ve bir hata varsa, günlükleri Microsoft'a gönder.
* **Bağlantıyı Sına** bir veri kaynağı.

### <a name="help-page"></a>Yardım sayfası
Yardım sayfasına aşağıdaki bilgileri görüntüler:

* Ağ geçidi kısa açıklaması
* Sürüm numarası
* Çevrimiçi Yardım, gizlilik ve lisans sözleşmesini bağlar.

## <a name="monitor-gateway-in-the-portal"></a>İzleyici ağ geçidi portalında
Azure portalında bir ağ geçidi makinesinde kaynak kullanımı (CPU, bellek, network(in/out), vb.) neredeyse gerçek zamanlı anlık görüntüsünü görüntüleyebilirsiniz.

1. Azure portalında veri fabrikanızın giriş sayfasına gidin ve tıklayın **bağlı hizmetler** Döşe.

    ![Data factory giriş sayfası](./media/data-factory-data-management-gateway/monitor-data-factory-home-page.png)
2. Seçin **ağ geçidi** içinde **bağlı hizmetler** sayfası.

    ![Bağlı hizmetler sayfası](./media/data-factory-data-management-gateway/monitor-linked-services-blade.png)
3. İçinde **ağ geçidi** sayfasında, bellek ve CPU kullanımı ağ geçidi görebilirsiniz.

    ![Ağ geçidi CPU ve bellek kullanımı](./media/data-factory-data-management-gateway/gateway-simple-monitoring.png)
4. Etkinleştirme **Gelişmiş ayarlar** ağ kullanımı gibi daha fazla ayrıntı görmek için.
    
    ![Gelişmiş ağ geçidi için izleme](./media/data-factory-data-management-gateway/gateway-advanced-monitoring.png)

Aşağıdaki tabloda yer alan sütun açıklanmakta **ağ geçidi düğümleri** listesi:

İzleme özelliği | Açıklama
:------------------ | :----------
Ad | Ağ geçidi ile ilişkili düğümleri ve mantıksal ağ geçidi adı. Ağ geçidi üzerinde yüklü olan bir şirket içi Windows makine düğümüdür. Birden fazla düğümü (en fazla dört düğüm) sahip tek bir mantıksal ağ geçidi hakkında daha fazla bilgi için bkz: [veri yönetimi ağ geçidi - yüksek kullanılabilirlik ve ölçeklenebilirlik](data-factory-data-management-gateway-high-availability-scalability.md).
Durum | Mantıksal ağ geçidini ve ağ geçidi düğümleri durumu. Örnek: Çevrimiçi/çevrimdışı/sınırlı/vb. Bu durumlar hakkında daha fazla bilgi için bkz. [ağ geçidi durumu](#gateway-status) bölümü.
Sürüm | Mantıksal ağ geçidi her ağ geçidi düğümü ve sürümü gösterir. Mantıksal ağ geçidi sürümünü grubunda düğüm çoğunluğu sürümüne göre belirlenir. Düğüm varsa, yalnızca mantıksal ağ geçidi işlevi olarak aynı sürüm numarasına sahip mantıksal bir ağ geçidi kurulumunu farklı sürümleriyle düzgün. Başkalarının sınırlı modundaki ve (yalnızca otomatik güncelleştirme başarısız olursa) el ile güncelleştirilmesi gerekir.
Uygun bellek | Bir ağ geçidi düğümü kullanılabilir bellek. Bu değer, neredeyse gerçek zamanlı anlık görüntüsüdür.
CPU kullanımı | Bir ağ geçidi düğümü, CPU kullanımı. Bu değer, neredeyse gerçek zamanlı anlık görüntüsüdür.
Ağ (daraltma/genişletme) | Bir ağ geçidi düğümü, ağ kullanımı. Bu değer, neredeyse gerçek zamanlı anlık görüntüsüdür.
(Çalışan / sınırlama) eşzamanlı işleri | İşleri veya her bir düğümde çalışan görevler sayısı. Bu değer, neredeyse gerçek zamanlı anlık görüntüsüdür. Her düğüm için en fazla eşzamanlı iş sınırı belirtir. Bu değer, makine boyutuna bağlı olarak tanımlanır. Burada CPU/bellek/ağ altında kullanılan, ancak etkinlikler zaman aşımına uğruyor Gelişmiş senaryolarda, eş zamanlı iş yürütme artırabileceğinizi limiti artırabilirsiniz. Bu özellik, bir tek düğümlü ağ geçidi ile (ölçeklenebilirlik ve kullanılabilirlik özelliği etkin olduğunda bile) de kullanılabilir.
Rol | Dağıtıcı ve çalışan rolleri, bir çok düğümlü ağ geçidi - iki tür vardır. Tüm düğümleri, yani tüm işleri yürütmek için kullanılabilirler çalışanlardır. Görevler/işleri bulut hizmetlerinden çekme ve bunları farklı çalışan düğümlerine (kendisi dahil) dağıtmak için kullanılan tek bir dağıtıcı düğüm yok.

Bu sayfada, ağ geçidi iki veya daha fazla düğüm (ölçeği genişletme senaryo) olduğunda daha anlamlı bazı ayarları bakın. Bkz: [veri yönetimi ağ geçidi - yüksek kullanılabilirlik ve ölçeklenebilirlik](data-factory-data-management-gateway-high-availability-scalability.md) bir çok düğümlü ağ geçidini ayarlamadan hakkında ayrıntılar için.

### <a name="gateway-status"></a>Ağ geçidi durumu
Aşağıdaki tabloda, olası durumlar sağlayan bir **ağ geçidi düğümü**:

Durum  | Yorumlar/senaryoları
:------- | :------------------
Çevrimiçi | Düğüm, Data Factory hizmetine bağlı.
Çevrimdışı | Düğümü çevrimdışı durumda.
Yükseltiliyor | Düğüm, otomatik olarak güncelleştirilir.
Sınırlı | Bağlantı sorunundan kaynaklanıyor. HTTP bağlantı noktası 8050 sorunu, service bus bağlantı sorunu veya kimlik bilgileri eşitleme sorunu nedeniyle olabilir.
Etkin Değil | Diğer Çoğunluk düğüm yapılandırmasından farklı bir yapılandırmada düğümüdür.<br/><br/> Diğer düğümlere bağlanamadığında bir düğüm etkin olabilir.

Aşağıdaki tabloda, olası durumlar sağlayan bir **mantıksal ağ geçidi**. Ağ geçidi durumu, ağ geçidi düğümleri durumlar üzerinde bağlıdır.

Durum | Yorumlar
:----- | :-------
Kayıt gerekiyor | Bu mantıksal ağ geçidi için hiçbir düğüm henüz kayıtlı
Çevrimiçi | Ağ geçidi düğümleri çevrimiçi
Çevrimdışı | Çevrimiçi durumu düğümü yok.
Sınırlı | Bu ağ geçidi olarak tüm düğümleri sağlıklı durumda olur. Bu durum bazı düğümü kapalı olabilir bir uyarıdır! <br/><br/>Dağıtıcı/çalışan düğümüyle kimlik eşitleme sorunu nedeniyle olabilir.

## <a name="scale-up-gateway"></a>Ağ geçidi ölçeklendirin
Sayısını yapılandırabilirsiniz **eş zamanlı veri taşıma işleri** şirket içi ve bulut arasında veri taşıma yeteneği artırabileceğinizi düğümünde çalıştırabilirsiniz veri depoları.

Kullanılabilir bellek ve CPU iyi kullanılmaz, ancak boş kapasiteyi 0 ise, bir düğümde çalıştırılabilen eşzamanlı iş sayısını artırarak ölçeği. Ağ geçidinin aşırı yüklendiği etkinlikler zaman aşımına uğruyor. zaman ölçeği isteyebilirsiniz. Bir ağ geçidi düğümü, Gelişmiş ayarları kullanarak bir düğümü için kapasite üst sınırı artırabilirsiniz.

## <a name="troubleshooting-gateway-issues"></a>Ağ geçidi sorunlarını giderme
Bkz: [ağ geçidiyle ilgili sorunları giderme](data-factory-troubleshoot-gateway-issues.md) makale bilgi/veri yönetimi ağ geçidi kullanma ile ilgili sorunları giderme ipuçları için.

## <a name="move-gateway-from-one-machine-to-another"></a>Ağ geçidi bir makineden diğerine taşıyabilirsiniz
Bu bölümde, taşıma ağ geçidi istemci için başka bir makineye adımları bir makineden sağlar.

1. Portalda gidin **Data Factory giriş sayfasında**, tıklatıp **bağlı hizmetler** Döşe.

    ![Veri ağ geçidi bağlantısı](./media/data-factory-data-management-gateway/DataGatewaysLink.png)
2. Ağ geçidi seçin **veri ağ GEÇİTLERİ** bölümünü **bağlı hizmetler** sayfası.

    ![Seçili ağ geçidi ile bağlantılı Hizmetleri sayfası](./media/data-factory-data-management-gateway/LinkedServiceBladeWithGateway.png)
3. İçinde **veri ağ geçidi** sayfasında **veri ağ geçidi yükleyip**.

    ![Ağ geçidi bağlantısını indirin](./media/data-factory-data-management-gateway/DownloadGatewayLink.png)
4. İçinde **yapılandırma** sayfasında **veri ağ geçidi yükleyip**ve veri ağ geçidi makinesine yüklemek için yönergeleri izleyin.

    ![Yapılandır sayfası](./media/data-factory-data-management-gateway/ConfigureBlade.png)
5. Tutun **Microsoft Veri Yönetimi ağ geçidi Yapılandırma Yöneticisi** açın.

    ![Configuration Manager](./media/data-factory-data-management-gateway/ConfigurationManager.png)
6. İçinde **yapılandırma** sayfasında Portalı'nda, **anahtarı yeniden oluşturun** tıklayın ve komut çubuğunda **Evet** için uyarı iletisi. Tıklayın **Kopyala düğmesini** yanındaki anahtarını panoya kopyalar anahtar metin. Ağ geçidini eski makineye yakında anahtarı yeniden oluşturmak gibi çalışmayı durdurur.

    ![Anahtarı yeniden oluşturun](./media/data-factory-data-management-gateway/RecreateKey.png)
7. Yapıştırma **anahtarı** metin kutusuna **ağ geçidi kaydetme** sayfasının **veri yönetimi ağ geçidi Yapılandırma Yöneticisi'ni** makinenizde. (isteğe bağlı) Tıklayın **Show ağ geçidi anahtarı** anahtar metnini görmek için onay kutusunu.

    ![Anahtarı Kopyala ve kaydetme](./media/data-factory-data-management-gateway/CopyKeyAndRegister.png)
8. Tıklayın **kaydetme** ağ geçidi bulut hizmetinde kaydetmek için.
9. Üzerinde **ayarları** sekmesinde **değişiklik** eski ağ geçidi ile kullanılan aynı sertifikayı seçmek için enter **parola**, tıklatıp **Son**.

   ![Sertifika belirtin](./media/data-factory-data-management-gateway/SpecifyCertificate.png)

   Sertifika aşağıdaki adımları uygulayarak eski ağ geçidini dışarı aktarabilirsiniz: eski makinede veri yönetimi ağ geçidi Yapılandırma Yöneticisi'ni başlatın, geçiş **sertifika** sekmesinde **dışarı** düğmesine tıklayın ve yönergeleri izleyin.
10. Ağ geçidi başarılı kayıt sonrasında görmeniz gerekir **kayıt** kümesine **kayıtlı** ve **durumu** kümesine **başlatıldı** Giriş sayfasında ağ geçidi Yapılandırma Yöneticisi.

## <a name="encrypting-credentials"></a>Kimlik bilgilerini şifreleme
Data Factory Düzenleyicisi'nde kimlik bilgilerini şifrelemek için aşağıdaki adımları uygulayın:

1. Şirket Web tarayıcısını **ağ geçidi makinesi**, gitmek [Azure portalında](https://portal.azure.com). Gerekirse veri fabrikanızı arayın, açın data factory'de **DATA FACTORY** sayfasında ve ardından **geliştir ve Dağıt** Data Factory Düzenleyicisi'ni başlatmak için.
2. Mevcut bir tıklayın **bağlı hizmet** JSON tanımına bakın veya bir veri yönetimi ağ geçidi gerektiren bir bağlı hizmet oluşturmak için ağaç görünümünde (örneğin: SQL Server veya Oracle).
3. JSON Düzenleyicisi için **gatewayName** özelliği, ağ geçidinin adını girin.
4. Sunucu adı girin **veri kaynağı** özelliğinde **connectionString**.
5. Veritabanı adını **Initial Catalog** özelliğinde **connectionString**.
6. Tıklayın **şifrele** düğmesine tıklayarak başlatan komut çubuğunda-sonra **kimlik bilgileri Yöneticisi** uygulama. Görmelisiniz **kimlik bilgilerini ayarlama** iletişim kutusu.

    ![Ayar kimlik bilgileri iletişim kutusu](./media/data-factory-data-management-gateway/setting-credentials-dialog.png)
7. İçinde **kimlik bilgilerini ayarlama** iletişim kutusunda, aşağıdaki adımları uygulayın:
   1. Seçin **kimlik doğrulaması** Data Factory hizmetinin veritabanına bağlanmak için kullanmak istediğiniz.
   2. Veritabanına erişimi olan kullanıcının adını girmek için **kullanıcıadı** ayarı.
   3. Kullanıcı için parola girin **parola** ayarı.
   4. Tıklayın **Tamam** kimlik bilgilerini şifrelemek ve iletişim kutusunu kapatın.
8. Görmelisiniz bir **encryptedCredential** özelliğinde **connectionString** şimdi.

    ```JSON
    {
        "name": "SqlServerLinkedService",
        "properties": {
            "type": "OnPremisesSqlServer",
            "description": "",
            "typeProperties": {
                "connectionString": "data source=myserver;initial catalog=mydatabase;Integrated Security=False;EncryptedCredential=eyJDb25uZWN0aW9uU3R",
                "gatewayName": "adftutorialgateway"
            }
        }
    }
    ```
   Ağ geçidi makineden farklı bir makineden portalı erişirseniz, kimlik bilgileri Yöneticisi uygulama ağ geçidi makineye bağlanabildiğinizi emin olmanız gerekir. Uygulama ağ geçidi makinesine bağlanamazsa, bu veri kaynağı için kimlik bilgilerini ayarlama ve veri kaynağı bağlantısını test etmek için izin vermez.

Kullanırken **kimlik bilgilerini ayarlama** uygulama portalı şifreler kimlik bilgileri ile belirtilen sertifika **sertifika** sekmesinde **ağ geçidi Yapılandırma Yöneticisi**  ağ geçidi makinesinde.

Kimlik bilgilerini şifrelemek için API tabanlı bir yaklaşım arıyorsanız kullanabileceğiniz [yeni AzDataFactoryEncryptValue](https://docs.microsoft.com/powershell/module/az.datafactory/new-azdatafactoryencryptvalue) kimlik bilgilerini şifrelemek için PowerShell cmdlet'i. Cmdlet sertifikayı kullanan kimlik bilgilerini şifrelemek üzere kullanmak için bu ağ geçidi yapılandırılmış. Şifrelenmiş kimlik bilgileri ekleme **EncryptedCredential** öğesinin **connectionString** JSON. JSON ile kullandığınız [yeni AzDataFactoryLinkedService](https://docs.microsoft.com/powershell/module/az.datafactory/new-azdatafactorylinkedservice) cmdlet'ini veya Data Factory Düzenleyicisi'nde.

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

Data Factory düzenleyici kullanarak kimlik bilgilerini ayarlamak için daha fazla bir yaklaşım yoktur. Düzenleyiciyi kullanarak bir SQL Server bağlı hizmeti oluşturursunuz ve kimlik bilgilerini düz metin olarak girin, kimlik bilgileri, Data Factory hizmetinin sahip bir sertifika kullanılarak şifrelenir. Sertifika kullanmaz, ağ geçidi kullanmak üzere yapılandırılmıştır. Bu yaklaşım bazı durumlarda biraz daha hızlı olmasını sağlarken daha az güvenlidir. Bu nedenle, bu yaklaşım yalnızca geliştirme/Test amaçları için izlemenizi öneririz.

## <a name="powershell-cmdlets"></a>PowerShell cmdlet'leri
Bu bölümde, oluşturma ve Azure PowerShell cmdlet'lerini kullanarak bir ağ geçidi kaydetme açıklar.

1. Başlatma **Azure PowerShell** Yönetici modunda.
2. Aşağıdaki komutu çalıştırarak ve Azure kimlik bilgilerinizi girerek Azure hesabınızda oturum açın.

    ```powershell
    Connect-AzAccount
    ```
3. Kullanım **yeni AzDataFactoryGateway** cmdlet'i gibi mantıksal bir ağ geçidi oluşturmak için:

    ```powershell
    $MyDMG = New-AzDataFactoryGateway -Name <gatewayName> -DataFactoryName <dataFactoryName> -ResourceGroupName ADF –Description <desc>
    ```
    **Örnek komut ve çıktı**:

    ```
    PS C:\> $MyDMG = New-AzDataFactoryGateway -Name MyGateway -DataFactoryName $df -ResourceGroupName ADF –Description “gateway for walkthrough”

    Name              : MyGateway
    Description       : gateway for walkthrough
    Version           :
    Status            : NeedRegistration
    VersionStatus     : None
    CreateTime        : 9/28/2014 10:58:22
    RegisterTime      :
    LastConnectTime   :
    ExpiryTime        :
    ProvisioningState : Succeeded
    Key               : ADF#00000000-0000-4fb8-a867-947877aef6cb@fda06d87-f446-43b1-9485-78af26b8bab0@4707262b-dc25-4fe5-881c-c8a7c3c569fe@wu#nfU4aBlq/heRyYFZ2Xt/CD+7i73PEO521Sj2AFOCmiI
    ```

1. Azure PowerShell'de klasöre geçin: **C:\\Program Files\Microsoft veri yönetimi Gateway\2.0\PowerShellScript\\**. Çalıştırma **RegisterGateway.ps1** yerel değişkeni ile ilişkili **$Key** aşağıdaki komutta gösterildiği gibi. Bu betik, daha önce oluşturduğunuz mantıksal ağ geçidi kurulu istemci Aracısı kaydeder.

    ```powershell
    PS C:\> .\RegisterGateway.ps1 $MyDMG.Key
    ```
    ```
    Agent registration is successful!
    ```
    Ağ geçidini bir uzak makineye IsRegisterOnRemoteMachine parametresini kullanarak kaydedebilirsiniz. Örnek:

    ```powershell
    .\RegisterGateway.ps1 $MyDMG.Key -IsRegisterOnRemoteMachine true
    ```
2. Kullanabileceğiniz **Get-AzDataFactoryGateway** veri fabrikanızı ağ geçitleri listesini almak için cmdlet. Zaman **durumu** gösterir **çevrimiçi**, ağ geçidiniz hazır anlamına gelir.

    ```powershell        
    Get-AzDataFactoryGateway -DataFactoryName <dataFactoryName> -ResourceGroupName ADF
    ```
   Bir ağ geçidi kullanarak kaldırabilirsiniz **Remove-AzDataFactoryGateway** kullanarak bir ağ geçidi için cmdlet ve güncelleştirme açıklaması **kümesi AzDataFactoryGateway** cmdlet'leri. Data Factory Cmdlet başvurusu söz dizimi ve bu cmdlet'ler hakkında diğer ayrıntılar için bkz.  

### <a name="list-gateways-using-powershell"></a>PowerShell kullanarak listesi ağ geçitleri

```powershell
Get-AzDataFactoryGateway -DataFactoryName jasoncopyusingstoredprocedure -ResourceGroupName ADF_ResourceGroup
```

### <a name="remove-gateway-using-powershell"></a>PowerShell kullanarak ağ geçidini kaldırma

```powershell
Remove-AzDataFactoryGateway -Name JasonHDMG_byPSRemote -ResourceGroupName ADF_ResourceGroup -DataFactoryName jasoncopyusingstoredprocedure -Force
```

## <a name="next-steps"></a>Sonraki adımlar
* Bkz: [şirket içi ile bulut arasında veri taşıma veri depoları](data-factory-move-data-between-onprem-and-cloud.md) makalesi. Bu izlenecek yolda, verileri bir şirket içi SQL Server veritabanından Azure blobuna taşımak için ağ geçidini kullanan bir işlem hattı oluşturun.
