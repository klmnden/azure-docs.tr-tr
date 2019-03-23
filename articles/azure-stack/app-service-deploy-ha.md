---
title: Yüksek oranda kullanılabilir bir yapılandırmada Azure Stack App Service'e dağıtma | Microsoft Docs
description: Azure Stack kullanarak yüksek kullanılabilirliğe sahip yapılandırmaya App Service'te dağıtmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: ''
ms.date: 03/23/2019
ms.author: jeffgilb
ms.reviewer: anwestg
ms.lastreviewed: 03/23/2019
ms.openlocfilehash: 1c105548f19994c4ca0ce161eedcfe11736864c7
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58370032"
---
# <a name="deploy-app-service-in-a-highly-available-configuration"></a>App Service'ı yüksek oranda kullanılabilir bir yapılandırmada dağıtın

Bu makalede, yüksek oranda kullanılabilir bir yapılandırmada Azure Stack için App Service dağıtmak için Azure Stack Market öğesi kullanmayı gösterir. Kullanılabilir Market öğesi ek olarak, bu çözüm ayrıca kullanır [appservice-dosya paylaşımı-sqlserver-ha](https://github.com/Azure/azurestack-quickstart-templates/tree/master/appservice-fileserver-sqlserver-ha) Azure Stack Hızlı Başlangıç şablonu. Bu şablon, App Service kaynak Sağlayıcısı'nı barındırmak için yüksek oranda kullanılabilir bir altyapı oluşturma otomatikleştirir. App Service, ardından bu yüksek oranda kullanılabilir VM altyapısında yüklenir. 

## <a name="deploy-the-highly-available-app-service-infrastructure-vms"></a>Yüksek oranda kullanılabilir App Service altyapısı Vm'leri dağıtma
[Appservice-dosya paylaşımı-sqlserver-ha](https://github.com/Azure/azurestack-quickstart-templates/tree/master/appservice-fileserver-sqlserver-ha) Azure Stack Hızlı Başlangıç şablonu, App Service dağıtımı yüksek oranda kullanılabilir bir yapılandırmaya basitleştirir. Varsayılan sağlayıcı abonelikte dağıtılmalıdır. 

Azure Stack'te özel bir kaynak oluşturmak için kullanıldığında, şablon oluşturur:
- Bir sanal ağ ve gerekli alt ağlar.
- Dosya sunucusu, SQL Server ve Active Directory etki alanı Hizmetleri (AD DS) alt ağları için ağ güvenlik grupları.
- VM diskleri ve bulut tanığı küme için depolama hesapları.
- Özel IP ile SQL VM'ler için bir iç yük dengeleyici için SQL AlwaysOn dinleyicisinde bağlı.
- AD DS, dosya sunucusu kümesi ve SQL kümesi için üç kullanılabilirlik kümeleri.
- İki düğümlü SQL kümesi.
- İki düğümlü dosya sunucusu kümesi.
- İki etki alanı denetleyicisi.

### <a name="required-azure-stack-marketplace-items"></a>Gerekli Azure Stack Market öğesi
Bu şablonu kullanmadan önce aşağıdakilerden emin [Azure Stack Market öğesi](azure-stack-marketplace-azure-items.md) Azure Stack Örneğinizde kullanılabilir:

- Windows Server 2016 Datacenter çekirdek görüntüsü (AD DS ve dosya sunucusu için Vm'leri)
- SQL Server 2016 SP2 Windows Server 2016 (Kurumsal)
- En son SQL Iaas uzantısı 
- En son PowerShell Desired State Configuration uzantısı 

> [!TIP]
> Gözden geçirme [şablon Benioku dosyası](https://github.com/Azure/azurestack-quickstart-templates/tree/master/appservice-fileserver-sqlserver-ha) şablon gereksinimleri ve varsayılan değerleri hakkında daha fazla ayrıntı için GitHub üzerindeki. 

### <a name="deploy-the-app-service-infrastructure"></a>App Service altyapısını dağıtma
Kullanarak bir özel dağıtım oluşturmak için bu bölümdeki adımları kullanın **appservice-dosya paylaşımı-sqlserver-ha** Azure Stack Hızlı Başlangıç şablonu.

1. [!INCLUDE [azs-admin-portal](../../includes/azs-admin-portal.md)]

2. Seçin **\+** **kaynak Oluştur** > **özel**, ardından **şablon dağıtımı**.

   ![Özel şablon dağıtımı](media/app-service-deploy-ha/1.png)


3. Üzerinde **özel dağıtım** dikey penceresinde **şablonu Düzen** > **Hızlı Başlangıç şablonu** ve sonra kullanılabilir özel şablonlar için aşağı açılan listesini kullanın seçin **appservice-dosya paylaşımı-sqlserver-ha** şablon tıklayın **Tamam**, ardından **Kaydet**.

   ![Appservice-dosya paylaşımı-sqlserver-ha Hızlı Başlangıç şablonu seçin](media/app-service-deploy-ha/2.png)

4. Üzerinde **özel dağıtım** dikey penceresinde **parametreleri Düzenle** ve aşağı kaydırma varsayılan şablon değerlerini gözden geçirin. Tüm gerekli parametre bilgilerini sağlayın ve ardından gerekiyorsa bu değerleri değiştirme **Tamam**.<br><br> En az karmaşık parolalar ADMINPASSWORD, FILESHAREOWNERPASSWORD FILESHAREUSERPASSWORD SQLSERVERSERVICEACCOUNTPASSWORD ve SQLLOGINPASSWORD parametrelerini belirtin.
    
   ![Özel dağıtım parametreleri Düzenle](media/app-service-deploy-ha/3.png)

5. Üzerinde **özel dağıtım** dikey penceresinde olun **varsayılan sağlayıcı aboneliği** olarak kullanın ve yeni bir kaynak grubu oluşturun veya mevcut bir kaynak grubunu, özel seçin için abonelik seçildi Dağıtım.<br><br> Ardından, kaynak grubu konumunu seçin (**yerel** ASDK yüklemeleri için) ve ardından **Oluştur**. Şablon dağıtımı başlamadan önce özel dağıtım ayarlarını doğrulanır.

    ![Özel dağıtım oluşturma](media/app-service-deploy-ha/4.png)

6. Yönetim portalında **kaynak grupları** ve ardından kaynak grubunun adı için özel dağıtım oluşturduğunuz (**app-service-ha** Bu örnekte). Tüm dağıtımlar başarıyla tamamladınız emin olmak için dağıtım durumunu görüntüleyin.

   > [!NOTE]
   > Şablon dağıtımı, tamamlanması yaklaşık bir saat sürer.

   [![](media/app-service-deploy-ha/5-sm.png "Şablonu dağıtım durumunu gözden geçirin")](media/app-service-deploy-ha/5-lg.png#lightbox)


### <a name="record-template-outputs"></a>Kayıt şablonu çıkarır
Sonra şablon dağıtım şablon dağıtımı çıkarır kaydı başarıyla tamamlar. App Service yükleyici çalıştırırken bu bilgileri sağlamanız gerekir. 

Bu çıkış değerlerin her birini kayıt emin olun:
- FileSharePath
- FileShareOwner
- FileShareUser
- SqlServer
- SQLuser

Şablon çıkış değerleri bulmak için aşağıdaki adımları izleyin:

1. [!INCLUDE [azs-admin-portal](../../includes/azs-admin-portal.md)]

2. Yönetim portalında **kaynak grupları** ve ardından kaynak grubunun adı için özel dağıtım oluşturduğunuz (**app-service-ha** Bu örnekte). 

3. Tıklayın **dağıtımları** seçip **Microsoft.Template**.

    ![Microsoft.Template dağıtım](media/app-service-deploy-ha/6.png)

4. Seçtikten sonra **Microsoft.Template** dağıtım, select **çıkışları** ve şablon parametresinin çıktısı kaydedin. Bu bilgiler, App Service dağıtımı sırasında gerekli olacaktır.

    ![Çıkış parametresi](media/app-service-deploy-ha/7.png)


## <a name="deploy-app-service-in-a-highly-available-configuration"></a>App Service'ı yüksek oranda kullanılabilir bir yapılandırmada dağıtın
Temel yüksek oranda kullanılabilir bir yapılandırmaya Azure Stack için App Service dağıtmak için bu bölümdeki adımları [appservice-dosya paylaşımı-sqlserver-ha](https://github.com/Azure/azurestack-quickstart-templates/tree/master/appservice-fileserver-sqlserver-ha) Azure Stack Hızlı Başlangıç şablonu. 

App Service kaynak Sağlayıcısı'nı yükledikten sonra teklifler ve planlar içerebilir. Kullanıcıların hizmete almak ve uygulamalar oluşturmaya başlamak için abone olabilirsiniz.

> [!IMPORTANT]
> Kaynak sağlayıcı yükleyicisini çalıştırmadan önce yeni işlevler, düzeltmeler ve dağıtımınızı etkileyebilecek tüm bilinen sorunlar hakkında bilgi edinmek için her App Service sürüm ile birlikte gelen sürüm notlarını okuduğunuzdan emin olun.

### <a name="prerequisites"></a>Önkoşullar
App Service yükleyiciyi çalıştırmadan önce birkaç adım açıklandığı gibi gerekli [App Service ile Azure Stack makaleye başlamadan önce](azure-stack-app-service-before-you-get-started.md):

> [!TIP]
> Açıklanan tüm adımları başlamadan önce makale gereklidir çünkü şablon azure'daki sanal makineleri altyapı sizin için yapılandırır. 

- [App Service yükleyici ve yardımcı betikleri indirin](azure-stack-app-service-before-you-get-started.md#download-the-installer-and-helper-scripts).
- [En son özel betik uzantısı için Azure Stack marketini indirme](azure-stack-app-service-before-you-get-started.md#syndicate-the-custom-script-extension-from-the-marketplace).
- [Gerekli sertifikaları oluşturmak](azure-stack-app-service-before-you-get-started.md#get-certificates).
- Azure Stack için seçtiğiniz kimlik sağlayıcısı dayalı kimliği uygulaması oluşturun. Uygulama kimliği için yapılabilir [Azure AD'ye](azure-stack-app-service-before-you-get-started.md#create-an-azure-active-directory-application) veya [Active Directory Federasyon Hizmetleri](azure-stack-app-service-before-you-get-started.md#create-an-active-directory-federation-services-application) ve uygulama kimliğini kaydedin
- Azure Stack Market'te Windows Server 2016 Datacenter görüntüsü eklediğinizden emin olun. Bu uygulama hizmeti yüklemesi için gereklidir.

### <a name="deploy-app-service-in-highly-available-configuration"></a>App Service'ı yüksek oranda kullanılabilir bir yapılandırmada dağıtın
App Service kaynak Sağlayıcısı'nı yükleme, en az bir saat sürer. Kaç rol örnekleri üzerinde gereken sürenin uzunluğunu bağlıdır dağıtın. Dağıtım sırasında aşağıdaki görevleri yükleyiciyi çalıştırır:

- Belirtilen Azure Stack depolama hesabındaki bir blob kapsayıcısı oluşturun.
- App Service için bir DNS bölgesi ve girişleri oluşturun.
- App Service kaynak sağlayıcısını kaydedin.
- App Service galeri öğelerini kaydedin.

App Service kaynak sağlayıcısı dağıtmak için aşağıdaki adımları izleyin:

1. Daha önce indirilen App Service yükleyiciyi çalıştırın (**appservice.exe**) yönetici olarak Azure Stack yönetici Azure kaynak yönetimi uç noktasına erişebildiğinden bir bilgisayar.

2. Seçin **App Service dağıtın veya en son sürüme yükseltme**.

    ![Dağıtın veya yükseltin](media/app-service-deploy-ha/01.png)

3. Microsoft lisans koşulları kabul edin ve tıklayın **sonraki**.

    ![Microsoft lisans koşulları](media/app-service-deploy-ha/02.png)

4. Microsoft olmayan lisans koşullarını kabul edin ve tıklayın **sonraki**.

    ![Microsoft olmayan lisans koşulları](media/app-service-deploy-ha/03.png)

5. App Service, Azure Stack ortamınıza bulut uç noktası yapılandırmasını sağlar.

    ![App Service bulut uç noktası yapılandırması](media/app-service-deploy-ha/04.png)

6. **Connect** Azure Stack aboneliğine yükleme ve konumu seçin. 

    ![Azure Stack aboneliğine bağlanma](media/app-service-deploy-ha/05.png)

7. Seçin **mevcut bir VNet ve alt ağları kullan** ve **kaynak grubu adı** yüksek oranda kullanılabilir bir şablonu dağıtmak için kullanılan kaynak grubu için.<br><br>Ardından, şablon dağıtımının bir parçası oluşturduğunuz sanal ağı seçin ve ardından açılır liste seçeneklerinden uygun rolü alt ağları seçin. 

    ![Vnet seçimi](media/app-service-deploy-ha/06.png)

8. Daha önce kaydedilen şablon dosya paylaşım yolu ve dosya paylaşımı sahibi parametrelerini bilgilerini çıkarır sağlar. İşiniz bittiğinde tıklayın **sonraki**.

    ![Dosya Paylaşımı çıkış bilgileri](media/app-service-deploy-ha/07.png)

9. App Service yüklemek için kullanılan makine App Service dosya paylaşımı barındırmak için kullanılan dosya sunucusu olarak aynı VNet üzerinde bulunmadığından adını çözümlemek mümkün olmayacaktır. **Bu beklenen bir davranıştır**.<br><br>Dosya Paylaşımı için UNC yolu ve hesap bilgilerini girdiğiniz bilgilerin doğru olduğundan emin olun ve basın **Evet** App Service yüklemeye devam etmek için uyarı iletişim kutusunda.

    ![Beklenen hata iletişim kutusu](media/app-service-deploy-ha/08.png)

    Mevcut bir sanal ağ ve dosya sunucunuza bağlanmak için bir dahili IP adresine dağıtmayı seçerseniz, çalışan alt ağ ve dosya sunucusu arasında SMB trafiği etkinleştirme bir giden güvenlik kuralı eklemeniz gerekir. Yönetim Portalı'nda WorkersNsg gidin ve aşağıdaki özelliklere sahip bir giden güvenlik kuralı ekleyin:
    - Kaynak: Herhangi biri
    - Kaynak bağlantı noktası aralığı: *
    - Hedef: IP Adresleri
    - Hedef IP adresi aralığı: Dosya sunucusu için IP aralığı
    - Hedef bağlantı noktası aralığı: 445
    - Protokol: TCP
    - Eylem: İzin Ver
    - Önceliği: 700
    - Ad: Outbound_Allow_SMB445

10. Identity Application kimliği ve yolunu ve kimlik sertifikası için parola sağlayın ve tıklayın **sonraki**:
    - Uygulama Kimliği sertifikası (biçimi **sso.appservice.local.azurestack.external.pfx**)
    - Azure Resource Manager kök sertifika (**AzureStackCertificationAuthority.cer**)

    ![Kimliği uygulama sertifikası ve kök sertifikası](media/app-service-deploy-ha/008.png)

10. Ardından, aşağıdaki sertifikalar için gerekli kalan bilgileri sağlayın ve tıklayın **sonraki**:
    - Varsayılan Azure Stack SSL sertifikası (biçimi **_.appservice.local.azurestack.external.pfx**)
    - API SSL sertifikası (biçimi **api.appservice.local.azurestack.external.pfx**)
    - Yayımcı sertifikası (biçiminde **ftp.appservice.local.azurestack.external.pfx**) 

    ![Ek yapılandırma sertifikaları](media/app-service-deploy-ha/09.png)

11. Yüksek kullanılabilirlik şablon dağıtım çıkışı gelen SQL Server bağlantı bilgilerini kullanarak SQL Server bağlantı bilgileri sağlayın:

    ![SQL Server bağlantı bilgileri](media/app-service-deploy-ha/10.png)

12. App Service yüklemek için kullanılan makine aynı sanal ağa App Service veritabanlarını barındırmak için kullanılan SQL sunucusu üzerinde bulunmadığından adını çözümlemek mümkün olmayacaktır.  **Bu beklenen bir davranıştır**.<br><br>SQL Server adı ve hesap bilgilerini için girilen bilgilerin doğru olduğundan emin olun ve basın **Evet** App Service yüklemeye devam etmek için. **İleri**’ye tıklayın.

    ![SQL Server bağlantı bilgileri](media/app-service-deploy-ha/11.png)

13. Varsayılan rol yapılandırma değerleri kabul edin veya önerilen değerlerin değiştirip'ı **sonraki**.<br><br>App Service altyapısını rol örnekleri için varsayılan değerleri için yüksek oranda kullanılabilir yapılandırmalara aşağıdaki gibi değiştirilmesi öneririz:

    |Rol|Varsayılan|Yüksek oranda kullanılabilir bir öneri|
    |-----|-----|-----|
    |Denetleyici rolü|2|2|
    |Yönetim Rolü|1|3|
    |Yayımcı rolü|1|3|
    |FrontEnd Rolü|1|3|
    |Paylaşılan çalışan rolü|1|10|
    |     |     |     |

    ![Altyapı rol örneği değerleri](media/app-service-deploy-ha/12.png)

    > [!NOTE]
    > Bu varsayılan değerlerinin değiştirilmesi bu tutoral artışlar App Service'ı yüklemeye yönelik donanım gereksinimleri önerilir. Toplam 26 çekirdek ve 46,592 MB RAM 15 VM'ler için varsayılan 18 çekirdek ve 32.256 MB RAM yerine önerilen 21 Vm'leri desteklemek için gereklidir.

14. App Service altyapısı Vm'leri yüklemek için kullanılacak platform görüntüsünü seçin ve tıklayın **sonraki**:

    ![Platform görüntü seçimi](media/app-service-deploy-ha/13.png)

15. App Service, kullanılması ve altyapı rolü kimlik bilgilerini sağlayın **sonraki**:

    ![Altyapı rolü kimlik bilgileri](media/app-service-deploy-ha/14.png)

16. ' A tıklayın ve App Service dağıtmak için kullanılacak bilgilerin **sonraki** dağıtımına başlamak için. 

    ![Yükleme özetini gözden geçirme](media/app-service-deploy-ha/15.png)

17. App Service dağıtımın ilerleme durumunu gözden geçirin. Bu, üzerinde belirli bir dağıtım yapılandırması ve donanım bağlı olarak bir saat sürebilir. Yükleyici başarıyla tamamlandıktan sonra Seç **çıkış**.

    ![Kurulum Tamamlandı](media/app-service-deploy-ha/16.png)


## <a name="next-steps"></a>Sonraki adımlar

[App Service ölçeğinizi](azure-stack-app-service-add-worker-roles.md). Ortamınızda beklenen talebi karşılamak için ek App Service altyapısını rol çalışanları eklemeniz gerekebilir. Varsayılan olarak, Azure Stack üzerinde App Service ücretsiz ve paylaşılan çalışan katmanları destekler. Diğer çalışan katmanları eklemek için daha fazla çalışan rolü eklemeniz gerekir.

[Dağıtım kaynaklarını yapılandırma](azure-stack-app-service-configure-deployment-sources.md). Ek yapılandırma, GitHub, BitBucket, OneDrive ve DropBox gibi birden çok kaynak denetim sağlayıcılarından isteğe bağlı dağıtım desteklemek için gereklidir.

[App Service yedekleme](app-service-back-up.md). Başarılı bir şekilde dağıtma ve App Service'ı yapılandırdıktan sonra veri kaybını önlemeye ve kurtarma işlemleri sırasında gereksiz hizmet kapalı kalma süresini önlemek için olağanüstü durum kurtarma için gerekli tüm bileşenleri yedeklenir emin olmalısınız.