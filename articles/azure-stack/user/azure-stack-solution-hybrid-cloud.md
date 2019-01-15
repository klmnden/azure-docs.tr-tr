---
title: Azure ve Azure Stack ile hibrit bulut dağıtın | Microsoft Docs
description: Azure'da ve Azure Stack'te hibrit bir bulutla dağıtmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/18/2018
ms.author: mabrigg
ms.reviewer: anajod
ms.openlocfilehash: 1629c4b62fb04e057c38261a33fd3bc759b279c1
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2019
ms.locfileid: "54267411"
---
# <a name="tutorial-deploy-a-hybrid-cloud-solution-with-azure-and-azure-stack"></a>Öğretici: Azure ve Azure Stack ile karma bulut çözümü dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu öğreticide, Azure genel bulutunda ve Azure Stack özel bulut kullanan bir karma bulut çözümü dağıtma işlemini göstermektedir.

Bir hibrit bulut çözümü kullanarak, bir özel bulut uyumluluk avantajlarından genel bulutun ölçeklenebilirliği ile birleştirebilirsiniz. Ayrıca, geliştiricilerinizin Microsoft geliştiricisi ekosistem avantajlarından yararlanın ve becerilerini Bulut ve şirket içi ortamlara Uygula.

## <a name="overview-and-assumptions"></a>Genel bakış ve varsayımlar

Bir genel Bulut ve özel bir bulut aynı web uygulamasını dağıtma geliştiricilerinin bir iş akışı ayarlamak için bu öğreticiyi izleyebilirsiniz. Bu uygulama, özel bulut üzerinde barındırılan bir olmayan Internet yönlendirilebilir bir ağa erişmek mümkün olacaktır. Bu web uygulamalarına izlenir ve trafiğin bir ani değişiklik olduğunda, bir program Genel buluta trafiği yönlendirmek için DNS kayıtları değiştirir. Trafik ani önce düzeyine düştüğünde trafik özel buluta yönlendirilir.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> - Karma bağlı SQL Server veritabanı sunucusu dağıtın.
> - Genel bir Azure web uygulamasında bir karma ağına bağlayın.
> - DNS, Bulutlar arası ölçeklendirme için yapılandırın.
> - Bulutlar arası ölçeklendirmeye yönelik SSL sertifikaları yapılandırın.
> - Yapılandırın ve web uygulaması dağıtın.
> - Traffic Manager profili oluşturur ve Bulutlar arası ölçeklendirme için yapılandırır.
> - Application ınsights'ı izleme ve artan trafiği için uyarı ayarlama.
> - Küresel Azure ve Azure Stack arasında geçiş yapma otomatik trafiği yapılandırın.

### <a name="assumptions"></a>Varsayımlar

Bu öğreticide, genel Azure ve Azure Stack temel bir bilgi sahibi olduğunuzu varsayar. Bu öğreticiye başlamadan önce daha fazla bilgi edinmek istiyorsanız, aşağıdaki makaleleri gözden geçirin:

 - [Azure'a giriş](https://azure.microsoft.com/overview/what-is-azure/)
 - [Azure Stack temel kavramları](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

Bu öğretici ayrıca bir Azure aboneliğine sahip olduğunuzu varsayar. Bir aboneliğiniz yoksa, şunları yapabilirsiniz [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce aşağıdaki gereksinimleri karşılayabilecek emin olun:

- Bir Azure Stack geliştirme Seti'ni (ASDK) veya bir Azure Stack tümleşik sisteminde bir abonelik. Bir Azure Stack geliştirme Seti'ni dağıtmak için yönergeleri izleyin. [Yükleyicisi'ni kullanarak ASDK dağıtma](../asdk/asdk-deploy.md).
- Azure Stack yüklemenizi aşağıdakilerin yüklü olması:
  - Azure App Service. Azure App Service ortamınıza yapılandırmak ve dağıtmak, Azure Stack operatörü ile çalışır. Bu öğreticide, App Service en az bir (1) kullanılabilir bir adanmış çalışan rolünün olması gerekir.
  - Bir Windows Server 2016 görüntüsü
  - Microsoft SQL Server görüntüsü ile bir Windows Server 2016
  - Uygun planlar ve teklifler
 - Web uygulamanız için bir etki alanı adı. Bir etki alanı adı yoksa, bir GoDaddy Bluehost ve InMotion gibi bir etki alanı sağlayıcısından satın alabilirsiniz.
- Etki alanınızın LetsEncrypt gibi güvenilen bir sertifika yetkilisinden SSL sertifikası.
- Bir SQL Server veritabanı ile iletişim kurar ve Application Insights'ı destekleyen bir web uygulaması. İndirebileceğiniz [dotnetcore-sqldb-tutorial](https://github.com/Azure-Samples/dotnetcore-sqldb-tutorial) github'dan örnek uygulaması.
- Bir Azure sanal ağ ve Azure Stack sanal ağ arasında bir hibrit ağ. Ayrıntılı yönergeler için bkz. [Azure Stack ve Azure ile karma bulut bağlantısı yapılandırma](azure-stack-solution-hybrid-connectivity.md).

- Azure Stack'te karma sürekli tümleştirme/sürekli dağıtım (CI/CD) işlem hattı ile özel bir derleme aracısı. Ayrıntılı yönergeler için bkz. [karma bulut kimliği ile Azure'da ve Azure Stack'te uygulamaları yapılandır](azure-stack-solution-hybrid-identity.md)

## <a name="deploy-a-hybrid-connected-sql-server-database-server"></a>Karma bağlı SQL Server veritabanı sunucusunu dağıtma

1. Azure Stack kullanıcı portalında oturum açın.

2. Üzerinde **Pano**seçin **Market**.

    ![Azure Stack Market](media/azure-stack-solution-hybrid-cloud/image1.png)

3. İçinde **Market**seçin **işlem**ve ardından **daha fazla**. Altında **daha fazla**seçin **ücretsiz SQL Server Lisansı: Windows Server üzerinde SQL Server 2017 Developer** görüntü.

    ![Bir sanal makine görüntüsü seçme](media/azure-stack-solution-hybrid-cloud/image2.png)

4. Üzerinde **ücretsiz SQL Server Lisansı: Windows Server üzerinde SQL Server 2017 Developer** seçin **Oluştur**.

5. Üzerinde **temelleri > temel ayarları yapılandırma**, sağlayan bir **adı** sanal makine (VM) için bir **kullanıcı adı** SQL Server SA ve **parola** sa.  Gelen **abonelik** aşağı açılan listesinde, dağıtım yaptığınız aboneliği seçin. İçin **kaynak grubu**, kullanın **seçin varolan** ve Azure Stack web uygulamanızla aynı kaynak grubunda VM yerleştirin.

    ![VM için temel ayarları yapılandırma](media/azure-stack-solution-hybrid-cloud/image3.png)

6. Altında **boyutu**, sanal Makineniz için bir boyut seçin. Bu öğreticide, A2_Standard veya bir DS2_V2_Standard öneririz.

7. Altında **Ayarları > isteğe bağlı Özellikleri Yapılandır**, aşağıdaki ayarları yapılandırın:

    - **Depolama hesabı**. Bir gereksinim duyarsanız, yeni bir hesap oluşturun.
    - **Sanal ağ**

      > [!Important]  
      > SQL Server sanal Makinenizi aynı sanal ağ VPN ağ geçitleri üzerinde dağıtıldığından emin olun.

    - **Genel IP adresi**. Varsayılan ayarları kullanabilirsiniz.
    - **Ağ güvenlik grubu** (NSG). Yeni bir NSG oluşturun.
    - **Uzantıları ve izleme**. Varsayılan ayarları koruyun.
    - **Tanılama depolama hesabı**. Bir gereksinim duyarsanız, yeni bir hesap oluşturun.
    - Seçin **Tamam** yapılandırmanızı kaydetmek için.

    ![İsteğe bağlı özellikleri yapılandırma](media/azure-stack-solution-hybrid-cloud/image4.png)

1. Altında **SQL Server ayarları**, aşağıdaki ayarları yapılandırın:
   - İçin **SQL Bağlantısı**seçin için **genel (Internet)**.
   - İçin **bağlantı noktası**, varsayılan değeri değiştirmeyin **1433**.
   - İçin **SQL kimlik doğrulaması**seçin **etkinleştirme**.

     > [!Note]  
     > SQL kimlik doğrulaması'nı etkinleştirdiğinizde, otomatik olarak yapılandırdığınız "SQLAdmin" bilgilerle doldurmak **Temelleri**.

   - Ayarların geri kalanı için varsayılan değerleri koruyun. **Tamam**’ı seçin.

    ![SQL Server ayarlarını yapılandırma](media/azure-stack-solution-hybrid-cloud/image5.png)

9. Üzerinde **özeti**, sanal makine yapılandırmasını gözden geçirin ve ardından **Tamam** dağıtımı başlatmak için.

    ![Yapılandırma Özeti](media/azure-stack-solution-hybrid-cloud/image6.png)

10. Yeni bir VM oluşturmak için bir süre sürer. Sanal makineleriniz durumunu görüntüleyebilirsiniz **sanal makineler**.

    ![Sanal makineler](media/azure-stack-solution-hybrid-cloud/image7.png)

## <a name="create-web-apps-in-azure-and-azure-stack"></a>Azure ve Azure Stack Web uygulamaları oluşturun

Çalıştıran ve yöneten bir web uygulamasını Azure App Service basitleştirir. App Service, Azure Stack Azure ile tutarlı olduğundan, her iki ortamlarda çalıştırabilirsiniz. Uygulamanızı barındırmak için App Service'ı kullanacaksınız.

### <a name="create-web-apps"></a>Web uygulamaları oluşturun

1. Yönergeleri izleyerek, Azure'da bir web uygulaması oluşturma [bir Azure App Service planında yönetme](https://docs.microsoft.com/azure/app-service/app-service-plan-manage#create-an-app-service-plan). Web uygulaması aynı abonelik ve kaynak grubu, hibrit ağ koyduğunuzdan emin olun.

2. Azure Stack'te (1) önceki adımı yineleyin.

### <a name="add-route-for-azure-stack"></a>Azure Stack için rota Ekle

Azure Stack üzerinde App Service'te kullanıcıların uygulamanıza erişmesine izin vermek için genel Internet'ten yönlendirilebilir olması gerekir. Azure Stack Internet'ten erişilebiliyorsa, bir URL ya da genel kullanıma yönelik IP adresi Azure Stack web uygulamasının not edin.

Bir ASDK kullanıyorsanız, yapabilecekleriniz [bir statik NAT eşlemesi yapılandırma](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-vpn-connection-one-node#configure-the-nat-virtual-machine-on-each-azure-stack-development-kit-for-gateway-traversal) App Service, sanal ortamın dışında kullanıma sunmak için.

### <a name="connect-a-web-app-in-azure-to-a-hybrid-network"></a>Bir web uygulamasını azure'da bir karma ağa bağlanma

Azure'da ön uç web uygulamaları ve Azure Stack'te SQL Server veritabanı arasında bağlantı sağlamak için web uygulaması Azure ve Azure Stack arasında karma ağa bağlı olmalıdır. Bağlantıyı etkinleştirmek için aşağıdakiler:

- Noktadan siteye bağlantı yapılandırma
- Web uygulamasını yapılandırma
- Azure Stack'te yerel ağ geçidi değiştirin.

### <a name="configure-the-azure-virtual-network-for-point-to-site-connectivity"></a>Azure sanal ağı için noktadan siteye bağlantı yapılandırma

Azure hibrit ağ tarafında sanal ağ geçidi, Azure App Service ile tümleştirmek noktadan siteye bağlantıları izin vermeniz gerekir.

1. Azure'da sanal ağ geçidi sayfasına gidin. Altında **ayarları**seçin **noktadan siteye yapılandırma**.

    ![Noktadan siteye seçeneği](media/azure-stack-solution-hybrid-cloud/image8.png)

2. Seçin **Şimdi Yapılandır** noktadan siteye yapılandırma.

    ![Noktadan siteye yapılandırma başlatma](media/azure-stack-solution-hybrid-cloud/image9.png)

3. Üzerinde **noktadan siteye** yapılandırma sayfasında, kullanmak istediğiniz özel IP adresi aralığını girin **adres havuzu**.

   > [!Note]  
   > Belirttiğiniz aralığın bileşenlerde genel Azure'da veya Azure Stack'te hibrit ağ alt ağları tarafından zaten kullanılan adres aralıklardan herhangi biriyle çakışmayacak emin olun.

   Altında **tünel türü**, onay kutusunu temizleyin **Ikev2 VPN**. Seçin **Kaydet** noktadan siteye yapılandırmayı tamamlamak için.

   ![Noktadan siteye ayarları](media/azure-stack-solution-hybrid-cloud/image10.png)

### <a name="integrate-the-azure-app-service-application-with-the-hybrid-network"></a>Azure App Service uygulamasını karma ağ ile tümleştirme

1. Azure sanal ağı uygulamaya bağlanmak için yönergeleri izleyin. [VNet tümleştirmesi etkinleştiriliyor](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet#enabling-vnet-integration).

2. Gidin **ayarları** web uygulamasını barındıran bir App Service planınız için. İçinde **ayarları**seçin **ağ**.

    ![Ağı yapılandırma](media/azure-stack-solution-hybrid-cloud/image11.png)

3. İçinde **VNET tümleştirmesi**seçin **yönetmek için buraya tıklayın**.

    ![VNET tümleştirmesi yönetme](media/azure-stack-solution-hybrid-cloud/image12.png)

4. Yapılandırmak istediğiniz sanal ağ seçin. Altında **için IP adresleri VNET'e YÖNLENDİRİLMİŞ**, Azure sanal ağı, Azure Stack VNet ve noktadan siteye adres alanlarının IP adresi aralığını girin. Seçin **Kaydet** doğrulamak ve bu ayarları kaydedin.

    ![Yönlendirmek için IP adresi aralıkları](media/azure-stack-solution-hybrid-cloud/image13.png)

App Service'nın Azure Vnet ile nasıl tümleştirildiği hakkında daha fazla bilgi için bkz: [uygulamanızı bir Azure sanal ağ ile tümleştirme](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet).

### <a name="configure-the-azure-stack-virtual-network"></a>Azure Stack sanal ağı yapılandırma

Yerel ağ geçidi Azure Stack sanal ağ trafiği yönlendirmek için App Service noktadan siteye adres aralığından yapılandırılması gerekir.

1. Azure Stack'te gidin **yerel ağ geçidi**. **Ayarlar** altında **Yapılandırma**'yı seçin.

    ![Ağ geçidi yapılandırma seçeneği](media/azure-stack-solution-hybrid-cloud/image14.png)

2. İçinde **adres alanı**, Azure.l seçin sanal ağ geçidi için noktadan siteye adres aralığını girin **Kaydet** doğrulamak ve bu yapılandırmayı kaydetmek için.

    ![Noktadan siteye adres alanı](media/azure-stack-solution-hybrid-cloud/image15.png)

## <a name="configure-dns-for-cross-cloud-scaling"></a>Bulutlar arası ölçeklendirme için DNS yapılandırma

Kullanıcılar, Bulutlar arası uygulamalar için doğru DNS yapılandırarak, web uygulamanızın genel Azure ve Azure Stack örnekleri erişebilir. Yükü artırır veya azaltır, Bu öğretici için DNS yapılandırmasını ayrıca Azure Traffic Manager trafiği yönlendirme sağlar.

Bu öğreticide Azure DNS, DNS yönetmek için kullanılır. (App Service etki alanları çalışmaz.)

### <a name="create-subdomains"></a>Alt etki alanları oluşturma

Traffic Manager DNS CNAME üzerinde bağımlı olduğundan, doğru trafiği Uç noktalara yönlendirmek için bir alt etki alanı gereklidir. DNS kayıtlarını ve etki alanı eşlemesi hakkında daha fazla bilgi için bkz: [Traffic Manager ile etki alanlarını eşleme](https://docs.microsoft.com/azure/app-service/web-sites-traffic-manager-custom-domain-name)

Azure için uç nokta da, kullanıcıların bir alt etki alanı oluşturacaksınız web uygulamanıza erişmek için kullanabilirsiniz. Bu öğretici için kullanabileceğiniz **app.northwind.com**, ancak kendi etki alanını temel alarak bu değer için özelleştirme yapmanız gerekir.

Azure Stack uç noktası için bir A kaydı bir alt etki alanı oluşturmak gerekir. Kullanabileceğiniz **azurestack.northwind.com**.

### <a name="configure-a-custom-domain-in-azure"></a>Azure'da özel bir etki alanını yapılandırma

1. Ekleme **app.northwind.com** tarafından Azure web uygulaması için ana bilgisayar adı [Azure App Service için bir CNAME eşlemesi](https://docs.microsoft.com/Azure/app-service/app-service-web-tutorial-custom-domain#map-a-cname-record).

### <a name="configure-custom-domains-in-azure-stack"></a>Azure Stack'te özel etki alanlarını yapılandırma

1. Ekleme **azurestack.northwind.com** ana bilgisayar adı için Azure Stack web uygulaması tarafından [Azure App Service için bir A kaydını eşleme](https://docs.microsoft.com/Azure/app-service/app-service-web-tutorial-custom-domain#map-an-a-record). App Service uygulaması için Internet yönlendirilebilir IP adresini kullanın.

2. Ekleme **app.northwind.com** ana bilgisayar adı için Azure Stack web uygulaması tarafından [Azure App Service için bir CNAME eşlemesi](https://docs.microsoft.com/Azure/app-service/app-service-web-tutorial-custom-domain#map-a-cname-record). Önceki adımda (1) için CNAME hedefi olarak yapılandırılmış bir ana bilgisayar adı kullanın.

## <a name="configure-ssl-certificates-for-cross-cloud-scaling"></a>Bulutlar arası ölçeklendirmeye yönelik SSL sertifikaları yapılandırma

Web uygulamanız tarafından toplanan hassas verileri aktarım için ve SQL veritabanında bekleyen güvenli olmasını sağlamak gerekir.

Tüm gelen trafik için SSL sertifikaları kullanmak için Azure ve Azure Stack web uygulamalarınızın yapılandıracaksınız.

### <a name="add-ssl-to-azure-and-azure-stack"></a>SSL ekleyin, Azure ve Azure Stack

Azure'da SSL eklemek için:

1. SSL sertifikası elde oluşturduğunuz alt etki alanı için geçerli olduğundan emin olun. (Joker karakterli sertifikalar kullanmak sorun değildir.)

2. Azure'da yönergeleri **web uygulamanızı hazırlama** ve **SSL sertifikanızı bağlama** bölümlerini [Azure Web Apps'e mevcut özel bir SSL sertifikası bağlama](https://docs.microsoft.com/Azure/app-service/app-service-web-tutorial-custom-ssl) makaleler. Seçin **SNI tabanlı SSL** olarak **SSL türü**.

3. Tüm trafiği HTTPS bağlantı noktasına yönlendirin. Bölümündeki yönergeleri **HTTPS zorlama** bölümünü [Azure Web Apps'e mevcut özel bir SSL sertifikası bağlama](https://docs.microsoft.com/Azure/app-service/app-service-web-tutorial-custom-ssl) makalesi.

Azure Stack için SSL eklemek için:

- Azure'da kullanılan 1-3. adımları tekrarlayın.

## <a name="configure-and-deploy-the-web-application"></a>Yapılandırma ve web uygulaması dağıtma

Rapor telemetri doğru Application Insights örneği uygulama koduna yapılandırın ve web uygulamaları ile doğru bağlantı dizelerini yapılandırma. Application Insights hakkında daha fazla bilgi için bkz: [Application Insights nedir?](https://docs.microsoft.com/azure/application-insights/app-insights-overview)

### <a name="add-application-insights"></a>Application Insights Ekleme

1. Microsoft Visual Studio ile web uygulamanızı açın.

2. [Application Insights ekleme](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core#add-application-insights-telemetry) projenize Application Insights web trafiğini artış veya Azalış uyarılar oluşturmak için kullandığı telemetri iletmek için.

### <a name="configure-dynamic-connection-strings"></a>Dinamik bağlantı dizelerini yapılandırma

Web uygulamasının her örneği, SQL veritabanına bağlanmak için farklı bir yöntem kullanır. Uygulamayı azure'da SQL Server sanal makinesi (VM) özel IP adresini ve Azure Stack'te uygulama SQL Server VM'nin genel IP adresini kullanır.

> [!Note]  
> Bir Azure Stack tümleşik sisteminde, genel IP adresini Internet yönlendirilebilir olmamalıdır. Bir Azure Stack geliştirme Seti'ni (ASDK üzerinde), genel IP adresini ASDK yönlendirilebilir değildir.

Uygulamanın her örneği için farklı bir bağlantı dizesi geçirmek için App Service ortam değişkenlerini kullanabilirsiniz.

1. Uygulamayı Visual Studio'da açın.

2. Startup.cs dosyasını açın ve aşağıdaki kod bloğunu bulun:

    ```C#
    services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlite("Data Source=localdatabase.db"));
    ```

3. Önceki kod bloğu appsettings.json dosyasında tanımlanan bir bağlantı dizesi kullanan aşağıdaki kodla değiştirin:

    ```C#
    services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("MyDbConnection")));
     // Automatically perform database migration
     services.BuildServiceProvider().GetService<MyDatabaseContext>().Database.Migrate();
    ```

### <a name="configure-app-service-application-settings"></a>App Service uygulama ayarları yapılandırma

1. Azure ve Azure Stack için bağlantı dizeleri oluşturun. Dizeleri, kullanılan IP adreslerini hariç eşit olmalıdır.

2. Azure ve Azure Stack, uygun bir bağlantı dizesi Ekle [bir uygulama ayarı olarak](https://docs.microsoft.com/azure/app-service/web-sites-configure) web uygulamasındaki kullanarak `SQLCONNSTR\_` adı ön eki olarak.

3. **Kaydet** web uygulaması ayarları ve uygulamayı yeniden başlatın.

## <a name="enable-automatic-scaling-in-global-azure"></a>Genel Azure'da otomatik ölçeklendirmeyi etkinleştir

Bir App Service ortamında web uygulamanızı oluştururken bir örnek ile başlar. Otomatik olarak daha fazla bilgi işlem kaynakları uygulamanız için sağlamak için örnekleri eklemek için ölçeği genişletebilirsiniz. Benzer şekilde, otomatik olarak ölçeklendirmek ve örnek sayısını azaltmak uygulama ihtiyaçlarınızı.

> [!Note]  
> Bir App Service ölçek genişletme ve ölçek yapılandırma planı olması gerekir. Bir plana sahip değilseniz, sonraki adımları uygulamaya başlamadan önce oluşturun.

### <a name="enable-automatic-scale-out"></a>Otomatik ölçek genişletme etkinleştir

1. Azure'da ölçeği genişletme ve ardından istediğiniz site için App Service planı bulmak **ölçeği genişletme (App Service planı)**.

    ![Ölçeği genişletme](media/azure-stack-solution-hybrid-cloud/image16.png)

2. Seçin **etkinleştirmek otomatik ölçeklendirme**.

    ![Otomatik ölçeklendirmeyi etkinleştir](media/azure-stack-solution-hybrid-cloud/image17.png)

3. İçin bir ad girin **otomatik ölçeklendirme ayarı adı**. İçin **varsayılan** otomatik ölçek kuralı, select **ölçek dayalı bir ölçüme göre**. Ayarlama **örnek limitleri** için **en az: 1**, **en fazla: 10**, ve **varsayılan: 1**.

    ![Otomatik ölçeklendirmeyi yapılandırma](media/azure-stack-solution-hybrid-cloud/image18.png)

4. Seçin **+ alınabilecek**.

5. İçinde **ölçüm kaynağı**seçin **geçerli kaynak**. Kural için aşağıdaki ölçütleri ve eylemleri kullanın.

**Ölçütler**

1. Altında **zaman toplama** seçin **ortalama**.

2. Altında **ölçüm adı**seçin **CPU yüzdesi**.

3. Altında **işleci**seçin **büyüktür**.

   - Ayarlama **eşiği** için **50**.
   - Ayarlama **süresi** için **10**.

**Eylem**

1. Altında **işlemi**seçin **sayıyı şu kadar Artır**.

2. Ayarlama **örnek sayısı** için **2**.

3. Ayarlama **soğuma** için **5**.

4. **Add (Ekle)** seçeneğini belirleyin.

5. Seçin **+ alınabilecek**.

6. İçinde **ölçüm kaynağı**seçin **geçerli kaynak.**

   > [!Note]  
   > App Service planının adı/GUID, geçerli kaynak içerir ve **kaynak türü** ve **kaynak** açılır listede gri.

### <a name="enable-automatic-scale-in"></a>Otomatik ölçek kümesinde etkinleştirin

Trafiğini azaltır, Azure web uygulamasına otomatik olarak maliyetlerini azaltmak için etkin örnek sayısını azaltabilirsiniz. Bu eylem uygulama kullanıcılar üzerindeki etkiyi en aza indirmek için ölçek genişletme daha az agresiftir.

1. Gidin **varsayılan** ölçek genişletme koşulunun seçin **+ alınabilecek**. Kural için aşağıdaki ölçütleri ve eylemleri kullanın.

**Ölçütler**

1. Altında **zaman toplama** seçin **ortalama**.

2. Altında **ölçüm adı**seçin **CPU yüzdesi**.

3. Altında **işleci**seçin **küçüktür**.

   - Ayarlama **eşiği** için **30**.
   - Ayarlama **süresi** için **10**.

**Eylem**

1. Altında **işlemi**seçin **sayıyı şu kadar Azalt**.

   - Ayarlama **örnek sayısı** için **1**.
   - Ayarlama **soğuma** için **5**.

2. **Add (Ekle)** seçeneğini belirleyin.

## <a name="create-a-traffic-manager-profile-and-configure-cross-cloud-scaling"></a>Bir Traffic Manager profili oluşturun ve Bulutlar arası ölçeklendirmeyi yapılandırma

Azure Traffic Manager profili oluşturacak ve sonra uç noktaları, Bulutlar arası ölçeklendirmeyi etkinleştirmek için yapılandırın.

### <a name="create-traffic-manager-profile"></a>Traffic Manager profili oluşturma

1. Seçin **kaynak oluştur**
2. Seçin **ağ**
3. Seçin **Traffic Manager profili** ve aşağıdakileri yapılandırın:

   - İçinde **adı**, profiliniz için bir ad girin. Bu ad **gerekir** trafficmanager.net bölgesinde benzersiz olması ve yeni bir DNS adı (örneğin, northwindstore.trafficmanager.net) oluşturmak için kullanılır.
   - İçin **yönlendirme yöntemi**seçin **ağırlıklı**.
   - İçin **abonelik**, bu profilde oluşturmak istediğiniz aboneliği seçin.
   - İçinde **kaynak grubu**, bu profili için yeni bir kaynak grubu oluşturun.
   - **Kaynak grubu konumu** alanında kaynak grubunun konumunu seçin. Bu ayar, kaynak grubunun konumunu ifade eder ve genel olarak dağıtılacak Traffic Manager profilini etkilemez.

4. **Oluştur**’u seçin.

    ![Traffic Manager profili oluşturma](media/azure-stack-solution-hybrid-cloud/image19.png)

 Traffic Manager profilinizin genel dağıtımı tamamlandığında, kaynaklar altında oluşturduğunuz kaynak grubunda listesinde gösterilir.

### <a name="add-traffic-manager-endpoints"></a>Traffic Manager uç noktalarını ekleme

1. Traffic Manager profilini oluşturduğunuz arayın. (Profil için kaynak grubu için geldiyseniz, profili seçin.)

2. İçinde **Traffic Manager profili**altında **ayarları**seçin **uç noktaları**.

3. **Add (Ekle)** seçeneğini belirleyin.

4. İçinde **uç noktası ekleme**, Azure Stack için aşağıdaki ayarları kullanın:

   - İçin **türü**seçin **dış uç noktası**.
   - Girin bir **adı** için bu endpoint.
   - İçin **tam etki alanı adı (FQDN) veya IP** Azure Stack web uygulamanız için dış URL'yi girin.
   - İçin **ağırlık**, varsayılan değeri değiştirmeyin **1**. Bu ağırlığı tüm trafiğin sağlıksız olması durumunda bu uç noktaya gitmesiyle sonuçlanır.
   - Bırakın **devre dışı olarak Ekle** seçeneği işaretli değil.

5. Seçin **Tamam** Azure Stack uç nokta kaydetmek için.

Sonraki Azure uç noktası yapılandıracaksınız.

1. Üzerinde **Traffic Manager profili**seçin **uç noktaları**.
2. Seçin **+ Ekle**.
3. Üzerinde **uç noktası ekleme**, Azure için aşağıdaki ayarları kullanın:

   - İçin **türü**seçin **Azure uç noktası**.
   - Girin bir **adı** için bu endpoint.
   - İçin **hedef kaynak türü**seçin **App Service**.
   - İçin **hedef kaynak**seçin **uygulama hizmeti seçin** aynı abonelikte bulunan Web uygulamalarının bir listesini görmek için.
   - **Kaynak** bölümünde ilk uç nokta olarak kullanmak istediğiniz uygulama hizmetini seçin.
   - İçin **ağırlık**seçin **2**. Bu, tüm trafiğin birincil uç noktaya sağlam değil veya tetiklendiğinde trafiği yeniden yönlendiren bir kural/uyarısı varsa bu uç noktaya gitmesiyle sonuçlanır.
   - Bırakın **devre dışı olarak Ekle** seçeneği işaretli değil.

4. Seçin **Tamam** Azure uç noktasını kaydetmek için.

Her iki bitiş noktası yapılandırıldıktan sonra içinde listelendikleri **Traffic Manager profili** seçtiğinizde **uç noktaları**. Aşağıdaki ekran görüntüsünde Bu örnek iki uç nokta, her biri için durumu ve yapılandırma bilgileri gösterir.

![Uç Noktalar](media/azure-stack-solution-hybrid-cloud/image20.png)

## <a name="set-up-application-insights-monitoring-and-alerting"></a>Uygulama izleme ve uyarı ınsights'ı ayarlama

Azure Application Insights uygulamanızı izlemek ve yapılandırdığınız koşullara göre uyarı gönder imkan tanır. Bazı örnekler şunlardır: uygulama kullanılamaz, hataları yaşıyor veya performans sorunlarını gösteriliyor.

Application Insights ölçümleri, Uyarılar oluşturmak için kullanacaksınız. Bu uyarılar tetiklediğinizde ölçeklemek için örnek otomatik olarak Azure yığını ölçeğini genişletmek için Azure geçin ve sonra Azure'a yedekleme, Web uygulamalarınızın yığın.

### <a name="create-an-alert-from-metrics"></a>Ölçümleri bir uyarı oluştur

Bu öğretici için kaynak grubuna gidin ve ardından açmak için Application Insights örneği **Application Insights**.

![Application Insights](media/azure-stack-solution-hybrid-cloud/image21.png)

Bu görünüm uyarı genişletme ve ölçeği uyarıda oluşturmak için kullanacaksınız.

### <a name="create-the-scale-out-alert"></a>Ölçeği genişletme uyarı oluşturma

1. Altında **yapılandırma**seçin **uyarılar (Klasik)**.
2. Seçin **ölçüm uyarısı Ekle (Klasik)**.
3. İçinde **Kuralı Ekle**, aşağıdakileri yapılandırın:

   - İçin **adı**, girin **Azure Bulutuna veri bloğu**.
   - A **açıklama** isteğe bağlıdır.
   - Altında **kaynak**, **uyar**seçin **ölçümleri**.
   - Altında **ölçütleri**, aboneliğiniz, Traffic Manager profilinizin ve Traffic Manager profilinin adı için kaynak grubu için kaynak seçin.

4. İçin **ölçüm**seçin **istek hızı**.
5. İçin **koşul**seçin **büyüktür**.
6. İçin **eşiği**, girin **2**.
7. İçin **süresi**seçin **son 5 dakika üzerinden**.
8. Altında **şu şekilde bildir**:
   - Onay için **e-posta sahipleri, Katkıda Bulunanlar ve okuyucular**.
   - E-posta adresinizi girin **ek yönetici email(s)**.

9. Menü çubuğunda, seçin **Kaydet**.

### <a name="create-the-scale-in-alert"></a>Ölçek uyarısı oluşturma

1. Altında **yapılandırma**seçin **uyarılar (Klasik)**.
2. Seçin **ölçüm uyarısı Ekle (Klasik)**.
3. İçinde **Kuralı Ekle**, aşağıdakileri yapılandırın:

   - İçin **adı**, girin **Azure Stack uygulamasına geri ölçek**.
   - A **açıklama** isteğe bağlıdır.
   - Altında **kaynak**, **uyar**seçin **ölçümleri**.
   - Altında **ölçütleri**, aboneliğiniz, Traffic Manager profilinizin ve Traffic Manager profilinin adı için kaynak grubu için kaynak seçin.

4. İçin **ölçüm**seçin **istek hızı**.
5. İçin **koşul**seçin **küçüktür**.
6. İçin **eşiği**, girin **2**.
7. İçin **süresi**seçin **son 5 dakika üzerinden**.
8. Altında **şu şekilde bildir**:
   - Onay için **e-posta sahipleri, Katkıda Bulunanlar ve okuyucular**.
   - E-posta adresinizi girin **ek yönetici email(s)**.

9. Menü çubuğunda, seçin **Kaydet**.

Aşağıdaki ekran görüntüsü yakalamayı ölçek genişletme ve ölçek için uyarılar gösterilir.

   ![Uyarılar (klasik)](media/azure-stack-solution-hybrid-cloud/image22.png)

## <a name="redirect-traffic-between-azure-and-azure-stack"></a>Azure Stack ile Azure arasındaki trafiği yeniden yönlendirme

Azure ve Azure Stack arasında geçiş yapma, Web uygulaması trafiğinizi, el ile veya Otomatik geçiş yapılandırabilirsiniz.

### <a name="configure-manual-switching-between-azure-and-azure-stack"></a>Azure Stack ve Azure arasında elle değiştirme yapılandırın

Web sitenizi eşiklerce ulaştığında bir uyarı alırsınız. El ile Azure'a trafiği yönlendirmek için aşağıdaki adımları kullanın.

1. Azure portalında Traffic Manager profilinizi seçin.

    ![Traffic Manager uç noktaları](media/azure-stack-solution-hybrid-cloud/image20.png)

2. Seçin **uç noktaları**.
3. Seçin **Azure uç noktası**.
4. Altında **durumu** seçin **etkin**ve ardından **Kaydet**.

    ![Azure uç noktası etkinleştirin](media/azure-stack-solution-hybrid-cloud/image23.png)

5. Üzerinde **uç noktaları** seçmek için Traffic Manager profili, **dış uç noktası**.
6. Altında **durumu** seçin **devre dışı bırakılmış**ve ardından **Kaydet**.

    ![Azure Stack uç noktayı devre dışı](media/azure-stack-solution-hybrid-cloud/image24.png)

Uç noktaları yapılandırıldıktan sonra uygulama trafiği Azure genişleme web uygulamanız Azure Stack web uygulaması yerine gider.

 ![Değiştirilen uç noktaları](media/azure-stack-solution-hybrid-cloud/image25.png)

Azure Stack'e akışı tersine çevirmek için önceki adımları kullanın:

- Azure Stack uç noktayı etkinleştirme
- Azure uç noktası devre dışı bırak

### <a name="configure-automatic-switching-between-azure-and-azure-stack"></a>Azure ve Azure Stack arasında otomatik geçiş yapılandırın

Uygulamanız çalışıyorsa Application Insights izleme kullanabilirsiniz bir [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) Azure işlevleri tarafından sağlanan ortam.

Bu senaryoda, Application Insights'ın çağıran bir işlev uygulaması Web kancası kullanmak için yapılandırabilirsiniz. Bu uygulama, otomatik olarak etkinleştirir veya bir uç nokta bir uyarıya yanıt olarak devre dışı bırakır.

Aşağıdaki adımlar otomatik trafik geçişini yapılandırmak için bir kılavuz olarak kullanın.

1. Bir Azure işlev uygulaması oluşturun.
2. HTTP ile tetiklenen bir işlev oluşturun.
3. Resource Manager, Web uygulamaları ve Traffic Manager için Azure SDK'ları alma.
4. Kod geliştirme:

   - Azure aboneliğiniz için kimlik doğrulaması.
   - Traffic Manager uç noktaları trafiği Azure'a veya Azure Stack değiştirir bir parametreyi kullanın.

5. Kodunuzu kaydedin ve eklemek için uygun parametrelerle işlev uygulamanızın URL'sine **Web kancası** Application Insights uyarı kuralı ayarları bölümü.
6. Bir Application Insights uyarı tetiklendiğinde, trafiği otomatik olarak yönlendirilir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure bulut desenleri hakkında daha fazla bilgi için bkz: [bulut tasarımı desenleri](https://docs.microsoft.com/azure/architecture/patterns).
