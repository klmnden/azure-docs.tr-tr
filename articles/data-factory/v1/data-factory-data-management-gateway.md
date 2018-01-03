---
title: "Veri Fabrikası için veri yönetimi ağ geçidi | Microsoft Docs"
description: "Şirket içi ve bulut arasında veri taşımak için veri ağ geçidi kurun ayarlayın. Veri Yönetimi ağ geçidi Azure Data Factory, verilerinizi taşımak için kullanın."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: b9084537-2e1c-4e96-b5bc-0e2044388ffd
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2017
ms.author: abnarain
robots: noindex
ms.openlocfilehash: af05f407661c2606719e733e373d0dad7bff3230
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="data-management-gateway"></a>Veri Yönetimi Ağ Geçidi
> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [tümleştirmesi çalışma zamanı sürüm 2'kendi kendini barındıran](../create-self-hosted-integration-runtime.md). 

Veri Yönetimi ağ geçidi kopyalamak için şirket içi ortamınızda yüklemelisiniz bir istemci aracısıdır Bulut ve şirket içi veri depoları arasında veri. Data Factory ile desteklenen depoları içinde listelenen şirket içi veri [desteklenen veri kaynakları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) bölümü.

Bu makalede kılavuzda tamamlar [şirket içi ve bulut arasında veri taşıma veri depolarına](data-factory-move-data-between-onprem-and-cloud.md) makalesi. Bu kılavuzda, bir Azure blob için bir şirket içi SQL Server veritabanından veri taşımak için ağ geçidini kullanan bir işlem hattı oluşturun. Bu makalede, veri yönetimi ağ geçidi hakkında ayrıntılı bilgi sağlar. 

Ağ geçidi ile birden çok şirket içi makineler ilişkilendirerek veri yönetimi ağ geçidi ölçeklendirebilirsiniz. Ölçeklendirmek yukarı bir düğümde aynı anda çalıştırabilirsiniz veri taşıma işlerinin sayısını artırarak. Bu özellik, tek bir düğüme sahip mantıksal bir ağ geçidi için de kullanılabilir. Bkz: [ölçeklendirme veri yönetimi ağ geçidi Azure veri fabrikası'nda](data-factory-data-management-gateway-high-availability-scalability.md) Ayrıntılar için makale.

> [!NOTE]
> Şu anda, ağ geçidi yalnızca kopyalama etkinliği ve saklı yordam etkinliği veri fabrikasında destekler. Şirket içi veri kaynaklarına erişmek için ağ geçidi'nden özel bir etkinliği kullanmak mümkün değil.      

## <a name="overview"></a>Genel Bakış
### <a name="capabilities-of-data-management-gateway"></a>Veri Yönetimi ağ geçidi özelliklerini
Veri Yönetimi ağ geçidi aşağıdaki yetenekleri sağlar:

* Şirket içi veri kaynakları modeli ve veri kaynakları aynı data factory içinde Bulut ve veri taşıma.
* Acil Durum İzleme ve ağ geçidi durumu veri fabrikası sayfasından görünürlük ile yönetim için tek bir bölme vardır.
* Şirket içi veri kaynaklarına erişimi güvenli bir şekilde yönetin.
  * Kurumsal Güvenlik Duvarı'nda gerekli değişiklik yok. Ağ geçidi yalnızca Internet açmak için giden HTTP tabanlı bağlantılar sağlar.
  * Sertifikanız ile şirket içi veri depoları için kimlik bilgilerini şifreler.
* Veri verimli bir şekilde hareket – veriler aktarılır paralel olarak otomatik dayanıklı aralıklı ağ sorunları mantığı yeniden deneyin.

### <a name="command-flow-and-data-flow"></a>Komut akışının ve veri akışı
Şirket içi ve bulut arasında veri kopyalamak için kopyalama etkinliği kullandığınızda, etkinlik bir ağ geçidi bulut tersi için şirket içi veri kaynağından veri aktarımı için kullanır.

Kopya veri ağ geçidi ile adımları özetini ve üst düzey veri akışı için şöyledir: ![ağ geçidini kullanarak veri akışı](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)

1. Veri Geliştirici oluşturur bir ağ geçidi kullanarak bir Azure Data Factory'deki [Azure portal](https://portal.azure.com) veya [PowerShell cmdlet'ini](https://msdn.microsoft.com/library/dn820234.aspx).
2. Veri geliştirici, ağ geçidi belirterek bir şirket içi veri deposu için bağlı hizmet oluşturur. Bağlantılı hizmet kurulumunun bir parçası olarak, kimlik doğrulama türleri ve kimlik bilgilerini belirtmek için kimlik bilgilerini ayarlama uygulama veri Geliştirici kullanır.  Kimlik bilgilerini ayarlama uygulama iletişim bağlantı ve kimlik bilgilerini kaydetmek için ağ geçidi test etmek için veri deposuyla iletişim kurar.
3. Ağ Geçidi kimlik bilgileri buluta kaydetmeden önce (veri geliştirici tarafından sağlanan), ağ geçidi ile ilişkili sertifika ile kimlik bilgilerini şifreler.
4. Data Factory Hizmeti'ne zamanlama & işlerin bir paylaşılan Azure service bus kuyruğu kullanan bir denetim kanalı üzerinden Yönetim için ağ geçidi ile iletişim kurar. Bir kopyalama etkinliği iş koparılan gerektiğinde, veri fabrikası kimlik bilgileri ile birlikte isteğini sıraya koyar. Ağ geçidi, sıra yoklama sonra işi başlatır.
5. Ağ geçidi aynı sertifika ile kimlik bilgileri şifresini çözer ve uygun kimlik doğrulama türü ve kimlik bilgileri ile şirket içi veri deposuna bağlanır.
6. Ağ geçidi bulut depolama alanına veya tersi kopyalama etkinliği veri ardışık düzeninde nasıl yapılandırıldığına bağlı olarak bir şirket içi deposundan verileri kopyalar. Bu adım için ağ geçidi doğrudan Azure Blob Depolama gibi bulut tabanlı depolama hizmetleri güvenli (HTTPS) kanal üzerinden iletişim kurar.

### <a name="considerations-for-using-gateway"></a>Ağ geçidini kullanma konuları
* Veri Yönetimi ağ geçidi tek bir örneği birden çok şirket içi veri kaynakları için kullanılabilir. Ancak, **bir tek ağ geçidi örneği için yalnızca bir Azure data factory bağlı** ve başka bir data factory ile paylaşılamaz.
* Sağlayabilirsiniz **veri yönetimi ağ geçidi yalnızca bir örneği** tek bir makinede yüklü. Şirket içi veri kaynaklarına erişmek için gereken iki veri fabrikaları olduğunu varsayalım ağ geçitlerini iki şirket içi bilgisayara yüklemeniz gerekir. Diğer bir deyişle, bir ağ geçidi için belirli veri fabrikası bağlıdır
* **Ağ geçidi veri kaynağı ile aynı makinede olması gerekmez**. Ancak, ağ geçidi yakın veri kaynağına sahip veri kaynağına bağlanmak ağ geçidi için süreyi azaltır. Bir şirket içi veri kaynağı barındıran farklı bir makinedeki ağ geçidi yüklemenizi öneririz. Ağ geçidi ve veri kaynağı farklı makinelerde olduğunda ağ geçidi veri kaynağı ile kaynaklar için rekabet edemez.
* Sağlayabilirsiniz **farklı makinelerde aynı şirket içi veri kaynağına bağlanan birden çok ağ geçidi**. Örneğin, iki veri fabrikaları hizmet veren iki ağ geçidi olabilir ancak aynı şirket içi veri kaynağı ile hem veri fabrikaları kaydedilir.
* Bilgisayar hizmet yüklü bir ağ geçidi zaten varsa bir **Power BI** senaryosu, yükleme bir **Azure Data Factory için ayrı bir ağ geçidi** başka bir makinede.
* Ağ geçidi kullandığınızda da kullanılmalıdır **ExpressRoute**.
* Veri kaynağı (bir güvenlik duvarının arkasında olan) bir şirket içi veri kaynağı olarak davran kullandığınızda bile **ExpressRoute**. Ağ Geçidi Hizmeti ve veri kaynağı arasında bağlantı kurmak için kullanın.
* Yapmanız gerekenler **ağ geçidini kullanmak** veri deposu bulutta üzerinde olsa bile bir **Azure Iaas sanal**.

## <a name="installation"></a>Yükleme
### <a name="prerequisites"></a>Önkoşullar
* Desteklenen **işletim sistemi** sürümleri Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2. Veri Yönetimi ağ geçidi etki alanı denetleyicisinde yükleme şu anda desteklenmiyor.
* .NET framework 4.5.1 veya üzeri gereklidir. Windows 7 makinede ağ geçidi yüklüyorsanız, .NET Framework 4.5 veya üstünü yükleyin. Bkz: [.NET Framework sistem gereksinimleri](https://msdn.microsoft.com/library/8z6watww.aspx) Ayrıntılar için.
* Önerilen **yapılandırma** ağ geçidi için en az 2 GHz, 4 çekirdek, 8 GB RAM ve 80 GB disk makinesidir.
* Konak makine hazırda bekleme, ağ geçidi veri isteklere yanıt vermez. Bu nedenle, uygun bir yapılandırma **güç planı** ağ geçidi'ı yüklemeden önce bilgisayarda. Makine hazırda bekleme için yapılandırılmışsa, ağ geçidi yükleme isteyen bir ileti alırsınız.
* Veri Yönetimi ağ geçidi başarıyla yükleyip için makinede yönetici olması gerekir. Ek kullanıcı eklemek **veri yönetimi ağ geçidi kullanıcıları** yerel Windows grubu. Bu grubun üyeleri kullanabilmek için **veri yönetimi ağ geçidi Yapılandırma Yöneticisi** ağ geçidini yapılandırmak için aracı.

Kopyalama etkinliği çalıştırır belirli frekansında durum gibi makinedeki kaynak kullanımı (CPU, bellek) de en yüksek ve boşta kalma süreleri ile aynı düzeni izler. Kaynak Kullanımı Yoğun bir şekilde de taşınan veri miktarına bağlıdır. Birden çok kopyası işleri devam ederken, kaynak kullanımı yoğun zamanlarda gidebilir bakın.

### <a name="installation-options"></a>Yükleme Seçenekleri
Veri Yönetimi ağ geçidi aşağıdaki yollarla yüklenebilir:

* Bir MSI kurulum paketi yükleyerek [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).  MSI korunan tüm ayarlarla mevcut veri yönetimi ağ geçidi en son sürüme yükseltmek için de kullanılabilir.
* Tıklayarak **veri ağ geçidi yükleyip** bağlantıyı el ile Kurulumu altında veya **doğrudan bu bilgisayar Yükle** EXPRESS Kurulumu altında. Bkz: [şirket içi ve bulut arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makalede hızlı kurulum kullanma hakkında adım adım yönergeler için. El ile Adım Yükleme Merkezi'nden alır.  Karşıdan yükleme ve İndirme Merkezi'nden ağ geçidi için sonraki bölümde yönergelerdir.

### <a name="installation-best-practices"></a>Yükleme için en iyi yöntemler:
1. Makine değil hazırda bekletme güç planı ağ geçidi için konak makinedeki yapılandırın. Konak makine hazırda bekleme, ağ geçidi veri isteklere yanıt vermez.
2. Ağ geçidi ile ilişkili sertifika yedekleyin.

### <a name="install-the-gateway-from-download-center"></a>Ağ geçidi İndirme Merkezi'nden yükleyin
1. Gidin [Microsoft Veri Yönetimi ağ geçidi yükleme sayfası](https://www.microsoft.com/download/details.aspx?id=39717).
2. Tıklatın **karşıdan**, uygun sürümü seçin (**32-bit** vs. **64-bit**), tıklatıp **sonraki**.
3. Çalıştırma **MSI** doğrudan ya da sabit diske ve Çalıştır kaydedin.
4. Üzerinde **Hoş Geldiniz** sayfasında, bir **dil** tıklatın **sonraki**.
5. **Kabul** tıklatın ve son kullanıcı lisans sözleşmesi **sonraki**.
6. Seçin **klasörü** ağ geçidini yükleyin tıklatıp **sonraki**.
7. Üzerinde **yüklenmeye hazır** sayfasında, **yükleme**.
8. Tıklatın **son** yüklemeyi tamamlamak için.
9. Anahtar Azure portalından alın. Adım adım yönergeler için sonraki bölüme bakın.
10. Üzerinde **kayıt ağ geçidi** sayfasında **veri yönetimi ağ geçidi Yapılandırma Yöneticisi** , makinede çalışan, şu adımları uygulayın:
    1. Anahtar metni yapıştırın.
    2. İsteğe bağlı olarak, tıklayın **Göster ağ geçidi anahtarı** anahtar metnini görmek için.
    3. Tıklatın **kaydetmek**.

### <a name="register-gateway-using-key"></a>Anahtarı kullanarak ağ geçidini kaydedin
#### <a name="if-you-havent-already-created-a-logical-gateway-in-the-portal"></a>Portalda mantıksal bir ağ geçidi oluşturmadıysanız
Portalda bir ağ geçidi oluşturma ve anahtarını almak için **yapılandırma** sayfası, izlenecek izleme adımları [şirket içi ve bulut arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale.    

#### <a name="if-you-have-already-created-the-logical-gateway-in-the-portal"></a>Portalda mantıksal ağ geçidi oluşturduysanız
1. Azure portalında gidin **Data Factory** sayfasında ve tıklayın **bağlı hizmetler** döşeme.

    ![Veri Fabrikası sayfası](media/data-factory-data-management-gateway/data-factory-blade.png)
2. İçinde **bağlı hizmetler** sayfasında, mantıksal **ağ geçidi** portalında oluşturulan.

    ![mantıksal ağ geçidi](media/data-factory-data-management-gateway/data-factory-select-gateway.png)  
3. İçinde **veri ağ geçidi** sayfasında, **veri ağ geçidi yükleyip**.

    ![Portalı'nda bağlantı indirin](media/data-factory-data-management-gateway/download-and-install-link-on-portal.png)   
4. İçinde **yapılandırma** sayfasında, **yeniden oluşturun anahtar**. Evet uyarı iletisi dikkatle okuduktan sonra'i tıklatın.

    ![Yeniden anahtar](media/data-factory-data-management-gateway/recreate-key-button.png)
5. Anahtar yanındaki Kopyala düğmesini tıklatın. Anahtar panoya kopyalandı.

    ![Anahtarı kopyalayın](media/data-factory-data-management-gateway/copy-gateway-key.png)

### <a name="system-tray-icons-notifications"></a>Sistem Tepsisi Simgeleri / bildirimleri
Aşağıdaki resimde gördüğünüz Tepsisi Simgeleri bazılarını gösterir.

![Sistem Tepsisi Simgeleri](./media/data-factory-data-management-gateway/gateway-tray-icons.png)

İmleç sistem tepsisi simgesi/bildirim iletisi taşırsanız, ağ geçidi/güncelleştirme işlemini açılan penceresinde durumu hakkında ayrıntılar bakın.

### <a name="ports-and-firewall"></a>Bağlantı noktaları ve güvenlik duvarı
Göz önünde bulundurmanız gereken iki güvenlik duvarı vardır: **Kurumsal Güvenlik Duvarı** kuruluşun merkezi yönlendirici üzerinde çalışan ve **Windows Güvenlik Duvarı** ağ geçidi olduğu yerel makine üzerinde bir arka plan programı olarak yapılandırılmış yüklü.  

![Güvenlik duvarları](./media/data-factory-data-management-gateway/firewalls2.png)

Kurumsal güvenlik duvarı düzeyinde şu etki alanlarına ve giden bağlantı noktalarını yapılandırmak:

| Etki alanı adları | Bağlantı Noktaları | Açıklama |
| --- | --- | --- |
| *. servicebus.windows.net |443, 80 |Veri Taşıma hizmeti arka uç ile iletişim için kullanılan |
| *. core.windows.net |443 |Azure Blob (yapılandırılmışsa) kullanarak hazırlanmış kopyalama için kullanılan|
| *. frontend.clouddatahub.net |443 |Veri Taşıma hizmeti arka uç ile iletişim için kullanılan |
| *. servicebus.windows.net |9350-9354, 5671 |İsteğe bağlı hizmet veri yolu geçişi Kopyalama Sihirbazı tarafından kullanılan TCP üzerinden |


Windows Güvenlik Duvarı düzeyde, bu giden bağlantı noktaları normal şekilde etkinleştirilir. Bağlantı noktaları ve etki alanlarını uygun şekilde yapılandırabilirsiniz, varsa ağ geçidi makinesi üzerinde.

> [!NOTE]
> 1. Kaynağını temel alan / havuzlarını olabilecek beyaz liste ek etki alanları ve giden bağlantı noktaları, Kurumsal/Windows Güvenlik Duvarı'nda.
> 2. Bazı bulut veritabanları için (örneğin: [Azure SQL veritabanı](https://docs.microsoft.com/azure/sql-database/sql-database-configure-firewall-settings), [Azure Data Lake](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-secure-data#set-ip-address-range-for-data-access), vb.), ağ geçidi makinede güvenlik duvarı yapılandırmalarını beyaz liste IP adresini gerekebilir.
>
>


#### <a name="copy-data-from-a-source-data-store-to-a-sink-data-store"></a>Veri kaynağı veri deposundan bir havuz veri deposuna kopyalama
Güvenlik duvarı kurallarının düzgün Kurumsal Güvenlik Duvarı'nda, ağ geçidi bilgisayarında Windows Güvenlik Duvarı etkinleştirilir ve kendisini verileri depolamak emin olun. Bu kurallar etkinleştirme hem kaynağına bağlanmak ve başarıyla havuz için ağ geçidini sağlar. Kopyalama işlemi dahil her veri deposu için kuralları etkinleştirin.

Örneğin, kopyadan için **bir şirket içi veri deposuna bir Azure SQL veritabanı havuz veya bir Azure SQL Data Warehouse havuz**, aşağıdaki adımları uygulayın:

* Gidene izin ver **TCP** iletişim bağlantı noktası **1433** Windows Güvenlik Duvarı ve kurumsal güvenlik duvarı için.
* Ağ geçidi makinenin IP adresinin IP adresine izin listesine eklemek için Azure SQL server'ın güvenlik duvarı ayarlarını yapılandırın.

> [!NOTE]
> Güvenlik duvarını giden bağlantı noktası 1433 izin vermez, ağ geçidi doğrudan Azure SQL erişemiyor. Bu durumda, kullanabilir [hazırlanan kopyalama](https://docs.microsoft.com/azure/data-factory/data-factory-copy-activity-performance#staged-copy) SQL Azure veritabanına / SQL Azure DW. Bu senaryoda, yalnızca HTTPS (443 numaralı bağlantı noktası) için veri taşıma gerektirir.
>
>


### <a name="proxy-server-considerations"></a>Proxy sunucusu hususları
Kurumsal ağ ortamınıza Internet'e erişmek için bir proxy sunucusu kullanıyorsa, uygun proxy ayarlarını kullanmak için veri yönetimi ağ geçidi yapılandırın. İlk kayıt aşamasında proxy ayarlayabilirsiniz.

![Kayıt sırasında kümesi proxy](media/data-factory-data-management-gateway/SetProxyDuringRegistration.png)

Ağ geçidi bulut hizmetine bağlanmak için proxy sunucusunu kullanır. Tıklatın **değişiklik** ilk kurulum sırasında bağlantı. Gördüğünüz **proxy ayarını** iletişim.

![Yapılandırma Yöneticisi'ni kullanarak küme proxy](media/data-factory-data-management-gateway/SetProxySettings.png)

Üç yapılandırma seçeneği vardır:

* **Proxy kullanmayın**: ağ geçidi açıkça kullanmaz her Proxy'yi bulut hizmetlerine bağlanmak için.
* **Sistem proxy kullanacak**: ağ geçidi olarak yapılandırılmış diahost.exe.config ve diawp.exe.config ayarının proxy kullanır.  Proxy diahost.exe.config ve diawp.exe.config yapılandırılmışsa, proxy üzerinden geçmeden doğrudan ağ geçidi bulut hizmetine bağlanır.
* **Özel ara sunucu kullanmak**: diahost.exe.config ve diawp.exe.config yapılandırmalarında kullanmak yerine ağ geçidi için kullanılacak HTTP proxy ayarlarını yapılandır.  Adresi ve bağlantı noktası gereklidir.  Kullanıcı adı ve parola, proxy'nin kimlik doğrulama ayarını bağlı olarak isteğe bağlıdır.  Tüm ayarları Ağ Geçidi kimlik bilgileri sertifikası ile şifrelenir ve ağ geçidi ana makinede yerel olarak depolanır.

Veri Yönetimi ağ geçidi ana bilgisayar hizmeti, güncelleştirilen proxy ayarlarını kaydettikten sonra otomatik olarak başlatır.

Görüntülemek veya proxy ayarlarını güncelleştirmek istiyorsanız, ağ geçidi başarıyla, kaydedildikten sonra veri yönetimi ağ geçidi Yapılandırma Yöneticisi'ni kullanın.

1. Başlatma **veri yönetimi ağ geçidi Yapılandırma Yöneticisi**.
2. Geçiş **ayarları** sekmesi.
3. Tıklatın **değişiklik** bağlamak **HTTP Proxy** başlatmak için bölüm **Set HTTP Proxy** iletişim.  
4. Tıklattıktan sonra **sonraki** düğmesi, proxy ayarı kaydetmek ve ağ geçidi ana bilgisayar hizmeti yeniden başlatmak için izninizi isteyen bir uyarı iletişim kutusu görürsünüz.

Görüntüleyin ve Configuration Manager aracını kullanarak HTTP proxy güncelleştirin.

![Yapılandırma Yöneticisi'ni kullanarak küme proxy](media/data-factory-data-management-gateway/SetProxyConfigManager.png)

> [!NOTE]
> Bir proxy sunucusu NTLM kimlik doğrulaması ile ayarladıysanız, ağ geçidi ana bilgisayar hizmeti etki alanı hesabı altında çalışır. Daha sonra etki alanı hesabı için parolayı değiştirirseniz, hizmeti için yapılandırma ayarlarını güncelleştirin ve uygun şekilde yeniden unutmayın. Bu gereksinim nedeniyle, parolayı sık güncelleştirmeye gerektirmez proxy sunucusuna erişmek için bir özel etki alanı hesabı kullanmak öneririz.
>
>

### <a name="configure-proxy-server-settings"></a>Proxy sunucusu ayarlarını yapılandırın
Seçerseniz **sistem proxy kullanmak** için HTTP Proxy'sini ayarlamayı, ağ geçidi proxy diahost.exe.config ve diawp.exe.config ayarını kullanır.  Proxy diahost.exe.config ve diawp.exe.config belirtilirse, proxy üzerinden geçmeden doğrudan ağ geçidi bulut hizmetine bağlanır. Aşağıdaki yordam diahost.exe.config dosyasını güncelleştirmek için yönergeler sağlar.  

1. Dosya Gezgini'nde, özgün dosyasını yedeklemek için C:\Program Files\Microsoft veri yönetimi Gateway\2.0\Shared\diahost.exe.config güvenli bir kopyasını oluşturun.
2. Yönetici olarak çalıştırarak Notepad.exe başlatın ve metin dosyasını "C:\Program Files\Microsoft veri yönetimi Gateway\2.0\Shared\diahost.exe.config. açın Aşağıdaki kodda gösterildiği gibi varsayılan etiket için system.net bulun:

         <system.net>
             <defaultProxy useDefaultCredentials="true" />
         </system.net>    

   Proxy sunucu ayrıntıları aşağıdaki örnekte gösterildiği gibi daha sonra ekleyebilirsiniz:

         <system.net>
               <defaultProxy enabled="true">
                     <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
               </defaultProxy>
         </system.net>

   Ek özellikler proxy etiketinin içine scriptLocation gibi gerekli ayarları belirtmek için izin verilir. Başvurmak [proxy öğesi (ağ ayarları)](https://msdn.microsoft.com/library/sa91de1e.aspx) sözdizimi hakkında.

         <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
3. Yapılandırma dosyasını özgün konumuna kaydedin, sonra değişiklikleri toplar veri yönetimi ağ geçidi ana bilgisayar hizmeti yeniden başlatın. Hizmetini yeniden başlatmak için: Denetim Masası'ndan ya da hizmetler uygulamasını kullanın **veri yönetimi ağ geçidi Yapılandırma Yöneticisi** > tıklatın **Hizmeti Durdur** düğmesine ve ardından **Başlat Hizmet**. Hizmet başlatılmazsa, hatalı bir XML etiket söz dizimini düzenlendi uygulama yapılandırma dosyasına eklendi olasıdır.

> [!IMPORTANT]
> Güncelleştirilecek unutmadığınızdan **her ikisi de** diahost.exe.config ve diawp.exe.config.  


Bu noktalarının yanı sıra de Microsoft Azure, şirketinizin beyaz olduğundan emin olmanız gerekir. Geçerli Microsoft Azure IP adreslerinin listesi yüklenebilir [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).

#### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a>Güvenlik Duvarı ve proxy sunucusu ile ilgili sorunlar için olası Belirtiler
Aşağıdaki ayarlara benzer hatalarla karşılaşırsanız, kendi kimliğini doğrulamak için ağ geçidi, veri fabrikası bağlanmasını engelleyen güvenlik duvarı veya proxy sunucu hatalı yapılandırma nedeniyle olasıdır. Güvenlik duvarınızın emin olmak için önceki bölüme bakın ve proxy sunucusu doğru şekilde yapılandırılmış.

1. Ağ geçidini kaydetmeyi denediğinizde, aşağıdaki hatayı alırsınız: "ağ geçidi anahtarı kaydedilemedi. Ağ geçidi anahtarı tekrar kaydetmeyi denemeden önce veri yönetimi ağ geçidi bağlı bir durumda ve veri yönetimi ağ geçidi ana bilgisayar hizmetinin başlatıldığından emin olun."
2. Yapılandırma Yöneticisi'ni açın, durum "Bağlantı kesildi" veya "Bağlanıyor." görürsünüz Windows olay günlükleri, "Olay Görüntüleyici" görüntülerken > "Uygulama ve hizmet günlükleri" > "Veri yönetimi ağ geçidi", aşağıdaki hata gibi hata iletilerine bakın:`Unable to connect to the remote server`
   `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`

### <a name="open-port-8050-for-credential-encryption"></a>Kimlik bilgisi şifreleme için 8050 numaralı bağlantı noktasını açın
**Kimlik bilgilerini ayarlama** uygulamanın kullandığı bağlantı noktasına gelen **8050** bir şirket içi bağlı hizmeti Azure portalında ayarladığınızda ağ geçidine geçiş kimlik bilgileri. Ağ geçidi Kurulum sırasında varsayılan olarak, ağ geçidi yüklemesi ağ geçidi makinesinde açar.

Bir üçüncü taraf güvenlik duvarı kullanıyorsanız, bağlantı noktası 8050 el ile açabilirsiniz. Ağ geçidi Kurulum sırasında güvenlik duvarı sorunu yaşayıp çalıştırırsanız, güvenlik duvarı yapılandırması olmadan ağ geçidi yüklemek için aşağıdaki komutu kullanarak deneyebilirsiniz.

    msiexec /q /i DataManagementGateway.msi NOFIREWALL=1

Ağ geçidi bilgisayarında bağlantı noktası 8050 açmamak seçerseniz kullanma dışındaki mekanizmalarını kullanmak **kimlik bilgilerini ayarlama** uygulama veri deposu kimlik bilgilerini yapılandırın. Örneğin, kullanabilirsiniz [yeni AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell cmdlet'i. Bkz: [kimlik bilgilerini ayarlama ve güvenlik](#set-credentials-and-securityy) nasıl veri kimlik bilgilerini depolamak üzerinde bölüm ayarlanabilir.

## <a name="update"></a>Güncelleştirme
Varsayılan olarak, veri yönetimi ağ geçidi, ağ geçidi daha yeni bir sürümü kullanılabilir olduğunda otomatik olarak güncelleştirilir. Tüm zamanlanmış görevlerin tümü tamamlanıncaya kadar ağ geçidi güncelleştirilmez. Güncelleştirme işlemi tamamlanana kadar başka görev ağ geçidi tarafından işlenir. Güncelleştirmesi başarısız olursa, ağ geçidi eski bir sürümüne geri alınır.

Zamanlanmış güncelleştirme zamanı aşağıdaki konumlarda bakın:

* Ağ geçidi Özellikler sayfasında Azure portalı.
* Giriş sayfası, veri yönetimi ağ geçidi Yapılandırma Yöneticisi
* Sistem tepsisi bildirimi iletisi.

Veri Yönetimi ağ geçidi Yapılandırma Yöneticisi'nin Giriş sekmesinde güncelleştirme zamanlamasını görüntüler ve en son ne zaman ağ geçidi yüklü/güncelleştirildi.

![Güncelleştirmeleri zamanlama](media/data-factory-data-management-gateway/UpdateSection.png)

Güncelleştirmeyi hemen yükleyin veya zamanlanan saatte otomatik olarak güncelleştirilecek ağ geçidi için bekleyin. Örneğin, aşağıdaki resimde hemen yüklemek için tıklatabilirsiniz Güncelleştir düğmesini yanı sıra ağ geçidi Yapılandırma Yöneticisi'nde gösterilen bildirim iletisi gösterilir.

![Güncelleştirme DMG Yapılandırma Yöneticisi'nde](./media/data-factory-data-management-gateway/gateway-auto-update-config-manager.png)

Sistem tepsisindeki bildirim iletisi, aşağıdaki görüntüde gösterildiği gibi görünür:

![Sistem tepsisi iletisi](./media/data-factory-data-management-gateway/gateway-auto-update-tray-message.png)

Sistem tepsisindeki güncelleştirme işlemi (el ile veya otomatik) durumunu görebilir. Ağ geçidi Yapılandırma Yöneticisi'ni başlattığınızda başlattığında, bir ileti ağ geçidi bağlantısı ile birlikte güncelleştirilmiş bildirim çubuğunda gördüğünüz [yeni konu nedir](data-factory-gateway-release-notes.md).

### <a name="to-disableenable-auto-update-feature"></a>Devre dışı bırak/otomatik güncelleştir özelliğini etkinleştirmek için
Devre dışı bırak/otomatik güncelleştirme özelliği aşağıdakileri yaparak etkinleştirebilirsiniz:

[Tek düğüm için ağ geçidi]
1. Ağ geçidi bilgisayarında Windows PowerShell'i başlatın.
2. C:\Program Files\Microsoft veri yönetimi Gateway\2.0\PowerShellScript klasörüne geçin.
3. (Devre dışı bırakın) özelliğini otomatik güncelleştirme etkinleştirmek için aşağıdaki komutu çalıştırın.   

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -off
    ```
4. Yeniden açmak için:

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -on  
    ```
[Birden çok düğümlü yüksek oranda kullanılabilir ve ölçeklenebilir için ağ geçidi (Önizleme)](data-factory-data-management-gateway-high-availability-scalability.md)
1. Ağ geçidi bilgisayarında Windows PowerShell'i başlatın.
2. C:\Program Files\Microsoft veri yönetimi Gateway\2.0\PowerShellScript klasörüne geçin.
3. (Devre dışı bırakın) özelliğini otomatik güncelleştirme etkinleştirmek için aşağıdaki komutu çalıştırın.   

    Yüksek kullanılabilirlik özelliği (Önizleme) ile ağ geçidi için fazladan bir AuthKey param gereklidir.
    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -off -AuthKey <your auth key>
    ```
4. Yeniden açmak için:

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -on -AuthKey <your auth key> 
    ```

## <a name="configuration-manager"></a>Configuration Manager
Ağ Geçidi'ni yükledikten sonra veri yönetimi ağ geçidi Yapılandırma Yöneticisi aşağıdaki yollardan biriyle başlatabilirsiniz:

1. İçinde **arama** penceresinde, türü **veri yönetimi ağ geçidi** bu yardımcı program erişmek için.
2. Yürütülebilir dosyayı çalıştırmak **ConfigManager.exe** klasöründeki: **C:\Program Files\Microsoft veri yönetim Gateway\2.0\Shared**

### <a name="home-page"></a>Giriş sayfası
Giriş sayfası aşağıdaki işlemleri yapmanıza olanak sağlar:

* (Bulut hizmetine vb. bağlı.) ağ geçidi durumunu görüntüleyin.
* **Kayıt** portalından bir anahtar kullanarak.
* **Durdur** ve başlangıç **veri yönetimi ağ geçidi ana bilgisayar hizmeti** ağ geçidi bilgisayarında.
* **Zamanlama güncelleştirmeleri** günün belirli bir zamanda.
* Ağ geçidi tarihi zaman görüntülemek **son güncelleştirme**.

### <a name="settings-page"></a>Ayarları sayfası
Ayarlar sayfasını aşağıdaki işlemleri yapmanıza olanak sağlar:

* Görüntüleyin, değiştirin ve dışarı aktarma **sertifika** ağ geçidi tarafından kullanılır. Bu sertifika, veri kaynağı kimlik bilgilerini şifrelemek için kullanılır.
* Değişiklik **HTTPS bağlantı noktası** uç noktası için. Ağ geçidi, veri kaynağı kimlik bilgilerini ayarlamak için bir bağlantı noktası açar.
* **Durum** bitiş noktası
* Görünüm **SSL sertifikası** veri kaynakları için kimlik bilgilerini ayarlamak için portal ve ağ geçidi arasında SSL iletişimi için kullanılır.  

### <a name="remote-access-from-intranet"></a>İntranet uzaktan erişim  
Bu işlev gelecekte etkinleştirilecek. Gelecek güncelleştirmelerde (v3.4 veya sonrası) biz, etkinleştirmek / kimlik bilgilerini şifrelemek için PowerShell veya kimlik bilgileri Yöneticisi uygulamasını kullanırken bağlantı noktası (yukarıdaki bölümüne bakın) 8050 kullanarak bugün gerçekleşen tüm uzak bağlantısını devre dışı olanak tanır. 

### <a name="diagnostics-page"></a>Tanılama sayfası
Tanılama sayfası aşağıdaki işlemleri yapmanıza olanak sağlar:

* Ayrıntılı etkinleştir **günlüğü**Olay Görüntüleyicisi'ndeki günlükleri görüntülemek ve başarısız olduysa günlükleri Microsoft'a gönderebilirsiniz.
* **Bağlantı sınama** bir veri kaynağına.  

### <a name="help-page"></a>Yardım sayfası
Yardım sayfasında aşağıdaki bilgileri görüntüler:  

* Ağ geçidi kısa açıklaması
* Sürüm numarası
* Çevrimiçi Yardım, gizlilik bildirimini ve lisans sözleşmesi bağlantılar.  

## <a name="monitor-gateway-in-the-portal"></a>İzleyici ağ geçidi portalında
Azure portalında bir ağ geçidi makinesinde neredeyse gerçek zamanlı anlık görüntüsünü kaynak kullanımı (CPU, bellek, network(in/out), vb.) görüntüleyebilirsiniz.  

1. Azure portalında veri fabrikanızın giriş sayfasına gidin ve tıklayın **bağlantılı Hizmetleri** döşeme. 

    ![Data factory giriş sayfası](./media/data-factory-data-management-gateway/monitor-data-factory-home-page.png) 
2. Seçin **ağ geçidi** içinde **bağlantılı Hizmetleri** sayfası.

    ![Bağlı hizmetler sayfası](./media/data-factory-data-management-gateway/monitor-linked-services-blade.png)
3. İçinde **ağ geçidi** sayfasında, bellek ve CPU kullanımı ağ geçidi görebilirsiniz.

    ![Ağ geçidi CPU ve bellek kullanımı](./media/data-factory-data-management-gateway/gateway-simple-monitoring.png) 
4. Etkinleştirme **Gelişmiş ayarları** ağ kullanımı gibi daha fazla ayrıntı görmek için.
    
    ![Ağ geçidi için izleme Gelişmiş](./media/data-factory-data-management-gateway/gateway-advanced-monitoring.png)

Aşağıdaki tabloda yer alan sütun açıklanmakta **ağ geçidi düğümleri** listesi:  

İzleme özelliği | Açıklama
:------------------ | :---------- 
Ad | Ağ geçidi ile ilişkili düğümleri ve mantıksal ağ geçidi adı. Ağ geçidinin yüklü olduğu bir şirket içi Windows makine düğümdür. Tek bir mantıksal ağ geçidi birden fazla düğüm (en fazla dört düğüm) sahip hakkında daha fazla bilgi için bkz: [veri yönetimi ağ geçidi - yüksek kullanılabilirlik ve ölçeklenebilirlik](data-factory-data-management-gateway-high-availability-scalability.md).    
Durum | Mantıksal ağ geçidi ve ağ geçidi düğümleri durumu. Örnek: Çevrimiçi/çevrimdışı/sınırlı/vs. Bu durumlar hakkında daha fazla bilgi için bkz: [ağ geçidi durumu](#gateway-status) bölümü. 
Sürüm | Mantıksal ağ geçidi ve her ağ geçidi düğümü sürümünü gösterir. Mantıksal ağ geçidi sürümü grubu düğüm çoğunluğu sürümüne göre belirlenir. Varsa düğümleri mantıksal ağ geçidi kurulumunda yalnızca mantıksal ağ geçidi işlevi aynı sürüm numarasına sahip farklı sürümleriyle düzgün. Başkalarının sınırlı modda ve (yalnızca otomatik güncelleştirmeler başarısız olursa) el ile güncelleştirilmesi gerekir. 
Kullanılabilir bellek | Bir ağ geçidi düğümü kullanılabilir bellek. Bu değer yakın gerçek zamanlı anlık görüntüsüdür. 
CPU kullanımı | Bir ağ geçidi düğümünün CPU kullanımı. Bu değer yakın gerçek zamanlı anlık görüntüsüdür. 
Ağ (çıkış) | Bir ağ geçidi düğümünün ağ kullanımı. Bu değer yakın gerçek zamanlı anlık görüntüsüdür. 
(Çalışan / sınırlamak) eşzamanlı işleri | İşlerini veya her bir düğümde çalışan görevlerin sayısıdır. Bu değer yakın gerçek zamanlı anlık görüntüsüdür. Her düğüm için en fazla eşzamanlı iş sınırı belirtir. Bu değeri makine boyutuna göre tanımlanır. Eşzamanlı iş yürütme burada altında kullanılan CPU/bellek/ağ, ancak etkinlikler zaman aşımına uğrama Gelişmiş senaryolarda ölçeklendirin sınırı artırabilirsiniz. Bu özellik, tek bir düğüm ile ağ geçidi (ölçeklenebilirlik ve kullanılabilirlik özelliği etkin değilse bile) de kullanılabilir.  
Rol | Dağıtıcı ve çalışan rolleri bir çok düğümlü ağ geçidi – de iki tür vardır. Tüm düğümleri, tüm işleri yürütmek için kullanılabilmesi için başka bir deyişle, çalışanlardır. Görevler/işleri bulut hizmetlerinden çekmek ve bunları farklı çalışan düğümlerine (kendisi dahil) gönderme için kullanılan tek bir dağıtıcı düğüm yok.

Bu sayfada, ağ geçidi iki veya daha fazla düğüm (senaryo genişletme) olduğunda, daha anlamlı bazı ayarları bölümüne bakın. Bkz: [veri yönetimi ağ geçidi - yüksek kullanılabilirlik ve ölçeklenebilirlik](data-factory-data-management-gateway-high-availability-scalability.md) bir çok düğümlü ağ geçidi kurun ayarlama hakkında ayrıntılı bilgi için.

### <a name="gateway-status"></a>Ağ geçidi durumu
Aşağıdaki tabloda, olası durumlar sağlayan bir **ağ geçidi düğümü**: 

Durum  | Yorumlar/senaryoları
:------- | :------------------
Çevrimiçi | Düğümü, veri fabrikası hizmetine bağlı.
Çevrimdışı | Çevrimdışı düğümdür.
Yükseltiliyor | Düğüm, otomatik olarak güncelleştirilir.
Sınırlı | Bağlantı sorunundan kaynaklanıyor. HTTP bağlantı noktası 8050 sorunu, hizmet veri yolu bağlantı sorunu veya kimlik bilgisi eşitleme sorunu nedeniyle olabilir. 
Devre dışı | Diğer Çoğunluk düğüm yapılandırmasından farklı bir yapılandırmada düğümdür.<br/><br/> Diğer düğümlere bağlanamadığında bir düğüm etkin olabilir. 


Aşağıdaki tabloda, olası durumlar sağlayan bir **mantıksal ağ geçidi**. Ağ geçidi durumu üzerinde ağ geçidi düğümleri durumlar değişir. 

Durum | Yorumlar
:----- | :-------
Kaydedilmesi gerekiyor | Hiçbir düğümü için bu mantıksal ağ geçidi henüz kayıtlı değil
Çevrimiçi | Ağ geçidi düğümleri çevrimiçi
Çevrimdışı | Çevrimiçi durumu düğümü yok.
Sınırlı | Bu ağ geçidi tüm düğümler sağlıklı durumda. Bu durum bazı düğümü kapalı olabilir bir uyarıdır! <br/><br/>Dağıtıcı/çalışan düğümünde kimlik bilgisi eşitleme sorunu nedeniyle olabilir. 

## <a name="scale-up-gateway"></a>Ağ geçidi kurun ölçeklendirme
Sayısı yapılandırabilirsiniz **eş zamanlı veri taşıma işleri** şirket içi ve bulut arasında veri taşıma özelliği ölçeklendirmek için bir düğüm üzerinde çalışabilir veri depolarına. 

Kullanılabilir bellek ve CPU iyi kullanılmaz, ancak boşta kapasitesi 0 ise, bir düğümde çalıştırılabilen eşzamanlı iş sayısını artırarak ölçeği. Ağ geçidi aşırı yüklendiği için etkinlikler zaman aşımına uğruyor zaman ölçeği isteyebilirsiniz. Bir ağ geçidi düğümü Gelişmiş ayarlarda bir düğüm için maksimum kapasiteyi artırabilir. 
  

## <a name="troubleshooting-gateway-issues"></a>Ağ geçidi sorunlarını giderme
Bkz: [ağ geçidi sorunlarını giderme](data-factory-troubleshoot-gateway-issues.md) bilgi/veri yönetimi ağ geçidi kullanarak sorunları sorun giderme ipuçları için makale.  

## <a name="move-gateway-from-one-machine-to-another"></a>Ağ geçidi bir makineden diğerine taşıma
Bu bölümde taşıma ağ geçidi istemcisini başka bir makineye adımlarını bir makineden sağlar.

1. Portalı'nda gidin **Data Factory giriş sayfasında**, tıklatıp **bağlı hizmetler** döşeme.

    ![Veri ağ geçidi bağlantısı](./media/data-factory-data-management-gateway/DataGatewaysLink.png)
2. Ağ geçidi seçin **DATA GATEWAYS** bölümünü **bağlı hizmetler** sayfası.

    ![Seçili ağ geçidi ile bağlantılı Hizmetler Sayfası](./media/data-factory-data-management-gateway/LinkedServiceBladeWithGateway.png)
3. İçinde **veri ağ geçidi** sayfasında, **veri ağ geçidi yükleyip**.

    ![Ağ geçidi bağlantı indirin](./media/data-factory-data-management-gateway/DownloadGatewayLink.png)
4. İçinde **yapılandırma** sayfasında, **veri ağ geçidi yükleyip**ve veri ağ geçidi makineye yüklemek için yönergeleri izleyin.

    ![Yapılandırma sayfası](./media/data-factory-data-management-gateway/ConfigureBlade.png)
5. Tutmak **Microsoft Veri Yönetimi ağ geçidi Yapılandırma Yöneticisi** açın.

    ![Configuration Manager](./media/data-factory-data-management-gateway/ConfigurationManager.png)    
6. İçinde **yapılandırma** sayfasında Portalı'nda, **yeniden oluşturun anahtar** komut çubuğu ve tıklatın **Evet** uyarı iletisi. Tıklatın **Kopyala düğmesini** anahtarı panoya kopyalar anahtar metin yanındaki. Eski makinedeki ağ geçidi anahtarı yeniden yakında gibi çalışmayı durdurur.  

    ![Yeniden anahtar](./media/data-factory-data-management-gateway/RecreateKey.png)
7. Yapıştır **anahtar** metin kutusuna **ağ geçidini Kaydet'i** sayfasında **veri yönetimi ağ geçidi Yapılandırma Yöneticisi** makinenizde. (isteğe bağlı) Tıklatın **Göster ağ geçidi anahtarı** anahtar metnini görmek için onay kutusunu.

    ![Kopya anahtarı ve kaydetme](./media/data-factory-data-management-gateway/CopyKeyAndRegister.png)
8. Tıklatın **kaydetmek** ağ geçidi bulut Hizmeti'ne kaydolmak için.
9. Üzerinde **ayarları** sekmesini tıklatın, **değişiklik** eski ağ geçidi ile kullanılan aynı sertifikayı seçmek için girin **parola**, tıklatıp **Son**.

   ![Sertifika belirtin](./media/data-factory-data-management-gateway/SpecifyCertificate.png)

   Aşağıdaki adımları uygulayarak eski ağ geçidi'nden bir sertifika verebilirsiniz: veri yönetimi ağ geçidi Yapılandırma Yöneticisi eski makinede başlatma, geçiş **sertifika** sekmesini tıklatın, **verme** düğmesine tıklayın ve yönergeleri izleyin.
10. Ağ geçidi başarılı kayıttan sonra görmeniz gerekir **kayıt** kümesine **kayıtlı** ve **durum** kümesine **başlatıldı** Giriş sayfasında ağ geçidi Yapılandırma Yöneticisi.

## <a name="encrypting-credentials"></a>Kimlik bilgileri şifreleme
Data Factory Düzenleyici'de kimlik bilgilerini şifrelemek için aşağıdaki adımları uygulayın:

1. Web tarayıcısı Başlat **ağ geçidi makinesi**, gitmek [Azure portal](http://portal.azure.com). Gerekirse, veri fabrikası için arama, açık veri fabrikasında **DATA FACTORY** sayfasında ve ardından **Yazar & Dağıt** Data Factory Düzenleyici başlatmak için.   
2. Var olan tıklatın **bağlantılı hizmeti** JSON tanımına bakın veya veri yönetimi ağ geçidi gerektiren bağlı hizmet oluşturma ağaç görünümünde (örneğin: SQL Server veya Oracle).
3. JSON Düzenleyicisi'nde için **gatewayName** özelliği, ağ geçidi adını girin.
4. Sunucu adı **veri kaynağı** özelliğinde **connectionString**.
5. Veritabanı adı **ilk katalog** özelliğinde **connectionString**.    
6. Tıklatın **şifrele** düğmesini tıklatın başlatır komut çubuğunda-sonra **kimlik bilgileri Yöneticisi** uygulama. Görmeniz gerekir **kimlik bilgilerini ayarlama** iletişim kutusu.

    ![Ayar kimlik bilgileri iletişim kutusu](./media/data-factory-data-management-gateway/setting-credentials-dialog.png)
7. İçinde **kimlik bilgilerini ayarlama** iletişim kutusunda, aşağıdaki adımları uygulayın:
   1. Seçin **kimlik doğrulaması** Data Factory hizmeti veritabanına bağlanmak için kullanmak istediğiniz.
   2. Veritabanına erişimi olan kullanıcı adını girmek için **kullanıcıadı** ayarı.
   3. Kullanıcı için parola girin **parola** ayarı.  
   4. Tıklatın **Tamam** kimlik bilgilerini şifrelemek ve iletişim kutusunu kapatın.
8. Görmeniz gerekir bir **encryptedCredential** özelliğinde **connectionString** şimdi.

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
Ağ geçidi makineden farklı bir makineden portal erişirse, kimlik bilgileri Yöneticisi uygulama ağ geçidi makineye bağlanabildiğinizi emin olmanız gerekir. Uygulama ağ geçidi makinesi bağlanamazsa, bu veri kaynağı için kimlik bilgilerini ayarlayın ve veri kaynağına bağlantıyı sınamak için izin vermiyor.  

Kullandığınızda **kimlik bilgilerini ayarlama** uygulama, portal şifreler kimlik bilgilerinin belirtilen sertifika ile **sertifika** sekmesinde **ağ geçidi Yapılandırma Yöneticisi**  ağ geçidi bilgisayarında.

Kimlik bilgilerini şifrelemek için API tabanlı bir yaklaşım arıyorsanız, kullanabileceğiniz [yeni AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) kimlik bilgilerini şifrelemek için PowerShell cmdlet. Cmdlet sertifikayı kullanan bu ağ geçidi kimlik bilgilerini şifrelemek için kullanmak üzere yapılandırılmış. Şifrelenmiş kimlik bilgileri Ekle **EncryptedCredential** öğesinin **connectionString** JSON içinde. JSON ile kullandığınız [New-AzureRmDataFactoryLinkedService](https://msdn.microsoft.com/library/mt603647.aspx) cmdlet'ini veya veri fabrikası Düzenleyicisi'nde.

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

Data Factory düzenleyici kullanarak kimlik bilgilerini ayarlama daha fazla bir yaklaşım vardır. Düzenleyicisi'ni kullanarak bir SQL Server bağlantılı hizmet oluşturun ve kimlik bilgilerini düz metin olarak girerseniz, kimlik bilgileri Data Factory hizmetine sahip bir sertifika kullanılarak şifrelenir. Sertifika kullanmaz, ağ geçidi kullanmak için yapılandırılır. Bu yaklaşım bazı durumlarda biraz daha hızlı olabilir ancak daha az güvenlidir. Bu nedenle, yalnızca geliştirme ve Test amaçları için bu yaklaşım izlemenizi öneririz.

## <a name="powershell-cmdlets"></a>PowerShell cmdlet'leri
Bu bölümde, oluşturma ve Azure PowerShell cmdlet'lerini kullanarak bir ağ geçidi kaydetme açıklar.

1. Başlatma **Azure PowerShell** Yönetici modunda.
2. Aşağıdaki komutu çalıştırarak ve Azure kimlik bilgilerinizi girme Azure hesabınızda oturum açın.

    ```PowerShell
    Login-AzureRmAccount
    ```
3. Kullanım **yeni AzureRmDataFactoryGateway** cmdlet'ini mantıksal bir ağ geçidi gibi oluşturun:

    ```PowerShell
    $MyDMG = New-AzureRmDataFactoryGateway -Name <gatewayName> -DataFactoryName <dataFactoryName> -ResourceGroupName ADF –Description <desc>
    ```
    **Örnek komut ve çıktı**:

    ```
    PS C:\> $MyDMG = New-AzureRmDataFactoryGateway -Name MyGateway -DataFactoryName $df -ResourceGroupName ADF –Description “gateway for walkthrough”

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

1. Azure PowerShell'de klasöre geçin: **C:\Program Files\Microsoft veri yönetimi Gateway\2.0\PowerShellScript\**. Çalıştırma **RegisterGateway.ps1** yerel değişkeni ile ilişkilendirilmiş **$Key** aşağıdaki komutta gösterildiği gibi. Bu komut dosyasını daha önce oluşturduğunuz mantıksal ağ geçidi ile makinenizde yüklü istemci Aracısı kaydeder.

    ```PowerShell
    PS C:\> .\RegisterGateway.ps1 $MyDMG.Key
    ```
    ```
    Agent registration is successful!
    ```
    Uzak makinedeki ağ geçidi IsRegisterOnRemoteMachine parametresini kullanarak kaydedebilirsiniz. Örnek:

    ```PowerShell
    .\RegisterGateway.ps1 $MyDMG.Key -IsRegisterOnRemoteMachine true
    ```
2. Kullanabileceğiniz **Get-AzureRmDataFactoryGateway** data factory'nizi ağ geçitleri listesini almak için cmdlet. Zaman **durum** gösterir **çevrimiçi**, ağ geçidiniz kullanıma hazır olduğu anlamına gelir.

    ```PowerShell        
    Get-AzureRmDataFactoryGateway -DataFactoryName <dataFactoryName> -ResourceGroupName ADF
    ```
Bir ağ geçidi kullanarak kaldırabilirsiniz **Kaldır AzureRmDataFactoryGateway** kullanarak bir ağ geçidi için cmdlet ve güncelleştirme açıklaması **kümesi AzureRmDataFactoryGateway** cmdlet'leri. Data Factory Cmdlet başvurusu sözdizimi ve bu cmdlet'ler hakkında diğer ayrıntılar için bkz.  

### <a name="list-gateways-using-powershell"></a>PowerShell kullanarak listesi ağ geçitleri

```PowerShell
Get-AzureRmDataFactoryGateway -DataFactoryName jasoncopyusingstoredprocedure -ResourceGroupName ADF_ResourceGroup
```

### <a name="remove-gateway-using-powershell"></a>PowerShell kullanarak ağ geçidi kaldırma

```PowerShell
Remove-AzureRmDataFactoryGateway -Name JasonHDMG_byPSRemote -ResourceGroupName ADF_ResourceGroup -DataFactoryName jasoncopyusingstoredprocedure -Force
```


## <a name="next-steps"></a>Sonraki adımlar
* Bkz: [şirket içi ve bulut arasında veri taşıma veri depolarına](data-factory-move-data-between-onprem-and-cloud.md) makalesi. Bu kılavuzda, bir Azure blob için bir şirket içi SQL Server veritabanından veri taşımak için ağ geçidini kullanan bir işlem hattı oluşturun.  
