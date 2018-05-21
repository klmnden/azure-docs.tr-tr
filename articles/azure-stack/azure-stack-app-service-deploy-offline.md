---
title: Çevrimdışı bir ortamda Azure yığın uygulama hizmeti dağıtma | Microsoft Docs
description: AD FS tarafından güvenliği bağlantısı kesilmiş bir Azure yığın ortamında uygulama hizmeti dağıtma hakkında ayrıntılı yönergeler.
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: anwestg
ms.openlocfilehash: 5b4281de4a6c2efee8e96f98a3cd46fec191fe22
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="add-an-app-service-resource-provider-to-a-disconnected-azure-stack-environment-secured-by-ad-fs"></a>AD FS tarafından güvenliği bağlantısı kesilmiş bir Azure yığın ortam için bir uygulama hizmeti kaynak Sağlayıcısı Ekle

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

> [!IMPORTANT]
> Azure tümleşik yığını sisteminizi 1804 güncelleştirmesini veya Azure App Service 1.2 dağıtmadan önce en son Azure yığın Geliştirme Seti dağıtın.
>
>

Bu makaledeki yönergeleri izleyerek yükleyebilirsiniz [uygulama hizmeti kaynak sağlayıcısı](azure-stack-app-service-overview.md) bir Azure yığın ortam için:

- Internet'e bağlı değil
- Active Directory Federasyon Hizmetleri (AD FS) tarafından güvenli.

Uygulama hizmeti kaynak sağlayıcısı, çevrimdışı Azure yığın dağıtımına eklemek için bu üst düzey görevleri tamamlamanız gerekir:

1. Tamamlamak [önkoşul adımlarını](azure-stack-app-service-before-you-get-started.md) (Sertifikalar satın alma gibi gerçekleştirebileceğiniz almak için birkaç gün).
2. [İndirme ve yükleme ve yardımcı dosyaları ayıklayın](azure-stack-app-service-before-you-get-started.md) Internet'e bağlı bir makineye.
3. Bir çevrimdışı yükleme paketi oluşturun.
4. Appservice.exe yükleyici dosyasını çalıştırın.

## <a name="create-an-offline-installation-package"></a>Bir çevrimdışı yükleme paketi oluşturun.

Uygulama hizmeti bağlantısı kesilmiş bir ortamda dağıtmak için ilk Internet'e bağlı bir makinede çevrimdışı yükleme paketi oluşturmalısınız.

1. Internet'e bağlı bir makinede AppService.exe yükleyiciyi çalıştırın.

2. Tıklatın **Gelişmiş** > **çevrimdışı yükleme paketi oluştur**.

    ![Uygulama Hizmeti Yükleyici][1]

3. Uygulama Hizmeti Yükleyici bir çevrimdışı yükleme paketi oluşturur ve yolunu görüntüler. Tıklayabilirsiniz **Klasör Aç** dosya Gezgini'nde klasörü açın.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy-offline/image02.png)

4. Yükleyici (AppService.exe) ve çevrimdışı yükleme paketini Azure yığın ana makinenizdeki kopyalayın.

## <a name="complete-the-offline-installation-of-app-service-on-azure-stack"></a>Azure yığın uygulama hizmeti çevrimdışı yüklemesini tamamlayın

1. Appservice.exe Azure yığın yönetici Azure kaynak yönetimi uç nokta ulaşabileceği bir bilgisayardan yönetici olarak çalıştırın.

2. Tıklatın **Gelişmiş** > **tamamlamak çevrimdışı yükleme**.

    ![Uygulama Hizmeti Yükleyici][2]

3. Daha önce oluşturduğunuz çevrimdışı yükleme paketini konumuna göz atın ve ardından **sonraki**.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy-offline/image04.png)

4. Gözden geçirin ve Microsoft Yazılım Lisans Koşulları'nı kabul edin ve ardından **sonraki**.

5. Gözden geçirin ve üçüncü taraf Lisans Koşulları'nı kabul edin ve ardından **sonraki**.

6. Uygulama hizmeti bulut yapılandırma bilgilerinin doğru olduğundan emin olun. Varsayılan ayarları Azure yığın Geliştirme Seti dağıtımı sırasında kullanılan, varsayılan değerleri kabul edebilir. Ancak, Azure yığın dağıtılan veya tümleşik bir sistemde dağıtılırken seçenekleri özelleştirdiyseniz, yansıtmak üzere bu penceresindeki değerleri düzenlemeniz gerekir. Örneğin, etki alanı soneki mycloud.com kullanırsanız, Azure yığın Kiracı Azure Resource Manager uç noktanız için yönetim değiştirmeniz gerekir. <region>. mycloud.com. Bilgilerinizi doğruladıktan sonra tıklatın **sonraki**.

    ![Uygulama Hizmeti Yükleyici][3]

7. Sonraki sayfada:
    1. Tıklatın **Bağlan** düğmesine **Azure yığın abonelikleri** kutusu.
        - Yönetici hesabınız sağlar. Örneğin, cloudadmin@azurestack.local. Parolanızı girin ve tıklayın **oturum**.
    2. İçinde **Azure yığın abonelikleri** kutusunda **varsayılan sağlayıcı abonelik**.
    3. İçinde **Azure yığın konumu** kutusunda, dağıtımına bölgeyi karşılık gelen konumu seçin. Örneğin, seçin **yerel** varsa Azure yığın Geliştirme Seti dağıtma.
    4. **İleri**’ye tıklayın.

    ![Uygulama Hizmeti Yükleyici][4]

8. Şimdi adımları yapılandırıldığı gibi mevcut bir sanal ağı dağıtmak için seçeneğiniz vardır [burada](azure-stack-app-service-before-you-get-started.md#virtual-network), veya bir sanal ağ ve ilişkili alt ağları oluşturmak uygulama hizmeti yükleyici izin verin.
    1. Seçin **oluşturma VNet varsayılan ayarlarla**, Varsayılanları kabul edin ve ardından **sonraki**, veya;
    2. Seçin **mevcut VNet ve alt ağları kullanın**.
        1. Seçin **kaynak grubu** sanal ağınızı; içerir
        2. Doğru seçin **sanal ağ** ; dağıtmak istediğiniz ad
        3. Doğru seçin **alt** gerekli rol alt ağın; her biri için değerler
        4. **İleri**’ye tıklayın

    ![Uygulama Hizmeti Yükleyici][5]

9. Dosya Paylaşımı için bilgileri girin ve ardından **sonraki**. Dosya Paylaşımı adresi, tam etki alanı adı veya dosya sunucunuzun IP adresini kullanması gerekir. Örneğin, \\\appservicefileserver.local.cloudapp.azurestack.external\websites, veya \\\10.0.0.1\websites.

> [!NOTE]
> Yükleyici paylaşımına devam etmeden önce bağlantısını test etme girişiminde bulunur.  Ancak, varolan bir sanal ağı dağıtmak seçerseniz, yükleyici için dosya paylaşımı bağlanabiliyor olmayabilir ve devam etmek isteyip istemediğinizi soran bir uyarı görüntüler.  Dosya Paylaşımı bilgilerini doğrulayın ve doğru olup olmadıklarını devam edin.
>
>

   ![Uygulama Hizmeti Yükleyici][8]

10. Sonraki sayfada:
    1. İçinde **kimlik uygulama kimliği** kutusuna, kimlik (Azure AD) için kullanmakta olduğunuz uygulama için GUID girin.
    2. İçinde **kimlik uygulama sertifika dosyası** kutusuna girin (veya göz atın) sertifika dosyası konumu.
    3. İçinde **kimlik uygulama sertifika parolası** kutusunda, sertifikanın parolasını girin. Sertifikalar oluşturmak üzere komut kullanıldığında Not yapılan bir paroladır.
    4. İçinde **Azure Resource Manager kök sertifika dosyasını** kutusuna girin (veya göz atın) sertifika dosyası konumu.
    5. **İleri**’ye tıklayın.

    ![Uygulama Hizmeti Yükleyici][10]

11. Her üç sertifika dosya kutularında, **Gözat** ve ardından uygun sertifika dosyasına gidin. Her sertifika için parola belirtmeniz gerekir. Bu sertifikalar, oluşturduğunuz olanlardır [oluşturma gerekli sertifikaları adım](azure-stack-app-service-before-you-get-started.md#get-certificates). Tıklatın **sonraki** tüm bilgileri girdikten sonra.

    | Box | Sertifika dosyası adı örneği |
    | --- | --- |
    | **Uygulama hizmeti varsayılan SSL sertifika dosyası** | \_.appservice.local.AzureStack.external.pfx |
    | **Uygulama hizmeti API SSL sertifika dosyası** | api.appservice.local.AzureStack.external.pfx |
    | **Uygulama hizmeti yayımcı SSL sertifika dosyası** | ftp.appservice.local.AzureStack.external.pfx |

    Sertifikaları oluşturduğunuzda farklı etki alanı soneki kullandıysanız, sertifika dosya adları kullanmayın *yerel. AzureStack.external*. Bunun yerine, özel etki alanı bilgilerinizi kullanın.

    ![Uygulama Hizmeti Yükleyici][11]

12. Uygulama hizmeti kaynak sağlayıcısı veritabanlarını barındırmak ve ardından için kullanılan sunucu örneği için SQL Server ayrıntılarını girin **sonraki**. Yükleyici SQL bağlantı özelliklerini doğrulama. **Gerekir** iç IP ya da SQL Server adı tam etki alanı adını girin.

> [!NOTE]
> Yükleyici devam etmeden önce SQl Server bağlantısını test etme girişiminde bulunur.  Ancak, varolan bir sanal ağı dağıtmak seçerseniz, yükleyici SQL Server'a bağlanmak kuramamış olabilir ve devam etmek isteyip istemediğinizi soran bir uyarı görüntüler.  SQL Server bilgilerini doğrulayın ve doğru olup olmadıklarını devam edin.
>
>
   
   ![Uygulama Hizmeti Yükleyici][12]

13. Rol örneği ve SKU seçenekleri gözden geçirin. Varsayılan örneği ve minimum SKU ASDK dağıtımında her rol için minimum sayısı ile doldurulur. VCPU ve bellek gereksinimlerini özetini dağıtımınızı planlamaya yardımcı olması için sağlanmıştır. Seçimlerinizi yaptıktan sonra tıklatın **sonraki**.

     > [!NOTE]
     > Üretim dağıtımları için yönergeleri [kapasite Azure yığınında Azure App Service sunucu rolleri için planlama](azure-stack-app-service-capacity-planning.md).
     >
     >

    | Rol | Minimum örnekleri | Minimum SKU | Notlar |
    | --- | --- | --- | --- |
    | Denetleyici | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Yönetir ve uygulama hizmeti bulut durumunu korur. |
    | Yönetim | 1 | Standard_A2 - (2 Vcpu, 3584 MB) | Uygulama hizmeti Azure Resource Manager ve API uç noktaları, portal Uzantıları (yönetici, Kiracı, işlevleri portalına) ve veri hizmeti yönetir. Yük devretme desteği için 2 için önerilen örnekleri artar. |
    | Yayımcı | 1 | Standard_A1 - (1 vCPU, 1792 MB) | FTP ve web dağıtımı üzerinden içerik yayımlar. |
    | FrontEnd | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Uygulama hizmeti uygulamaları isteklerini yönlendirir. |
    | Paylaşılan çalışan | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Ana web veya API uygulamaları ve Azure işlevleri uygulamalar. Daha fazla örnek eklemek isteyebilirsiniz. Bir operatör olarak teklifinizle tanımlamak ve herhangi bir SKU katmanı seçin. Katman bir vCPU en az olması gerekir. |

    ![Uygulama Hizmeti Yükleyici][14]

    > [!NOTE]
    > **Windows Server 2016 Core Azure yığında Azure uygulama hizmeti ile kullanılmak üzere desteklenen platform görüntüsü değil.  Değerlendirme görüntüleri üretim dağıtımları için kullanmayın.**

14. İçinde **Platform Görüntüsü Seç** kutusunda, uygulama hizmeti bulut bilgi işlem kaynak sağlayıcısındaki kullanılabilir olanlardan dağıtım Windows Server 2016 sanal makine görüntüsü seçin. **İleri**’ye tıklayın.

15. Sonraki sayfada:
     1. Çalışan rolü sanal makine yönetici kullanıcı adını ve parolasını girin.
     2. Diğer roller sanal makine yönetici kullanıcı adını ve parolasını girin.
     3. **İleri**’ye tıklayın.

    ![Uygulama Hizmeti Yükleyici][16]

16. Özet sayfasında:
    1. Yaptığınız seçimleri doğrulayın. Değişiklik yapmak için kullanın **önceki** düğmeleri önceki sayfaları ziyaret edin.
    2. Yapılandırmaları doğruysa, onay kutusunu seçin.
    3. Dağıtımı başlatmak için tıklatın **sonraki**.

    ![Uygulama Hizmeti Yükleyici][17]

17. Sonraki sayfada:
    1. Yükleme ilerleme durumunu izler. Azure yığın uygulama hizmeti varsayılan seçimlere göre dağıtmak için yaklaşık 60 dakika sürer.
    2. Yükleyici başarıyla tamamladıktan sonra **çıkış**.

    ![Uygulama Hizmeti Yükleyici][18]

## <a name="validate-the-app-service-on-azure-stack-installation"></a>Uygulama hizmeti Azure yığın yüklemede doğrula

1. Azure yığın Yönetim Portalı'nda Git **yönetim - uygulama hizmeti**.

2. Durumu altında genel bakışta, denetleyin **durum** gösterir **tüm rolleri hazır**.

    ![Uygulama Hizmeti Yönetimi](media/azure-stack-app-service-deploy/image12.png)
    
> [!NOTE]
> Varolan bir sanal ağı ve bir iç IP adresine, DosyaSunucusu conenct dağıtmak isterseniz, SMB trafiğini çalışan alt ağı ve DosyaSunucusu arasında etkinleştirme bir giden güvenlik kuralı eklemeniz gerekir.  Bunu yapmak için yönetim portalında WorkersNsg gidin ve aşağıdaki özelliklere sahip bir giden güvenlik kuralı ekleyin:
> * Kaynak: tüm
> * Kaynak bağlantı noktası aralığı: *
> * Hedef: IP adresleri
> * Hedef IP adresi aralığı:, DosyaSunucusu için IP aralığı
> * Hedef bağlantı noktası aralığı: 445
> * Protokol: TCP
> * Eylem: izin ver
> * Öncelik: 700
> * Ad: Outbound_Allow_SMB445
>

## <a name="test-drive-app-service-on-azure-stack"></a>Azure yığın uygulama hizmeti sürücüsünde test

Dağıtma ve uygulama hizmeti kaynak sağlayıcısı kaydetme sonra kullanıcıların web uygulamalarını ve API uygulamaları dağıtabilirsiniz emin olmak için test edin.

> [!NOTE]
> Planı içinde Microsoft.Web ad alanına sahip bir teklif oluşturmanız gerekir. Bu teklif için abone olan bir kiracı aboneliğinizin olması gerekir. Daha fazla bilgi için bkz: [oluşturma teklif](azure-stack-create-offer.md) ve [plan oluşturma](azure-stack-create-plan.md).
>
*Gerekir* Azure yığın uygulama hizmeti kullanan uygulamalar oluşturmak için bir kiracı aboneliği sahip. Bir Hizmet Yöneticisi Yönetim Portalı'ndan tamamlayabilirsiniz yalnızca özellikleri için uygulama hizmeti kaynak sağlayıcısı yönetim ilişkilidir. Bu özellikler kapasite ekleme, dağıtım kaynaklarını yapılandırmak ve çalışan katmanı ve SKU'ları ekleme içerir.
>
Üçüncü technical preview sürümünden itibaren web API ve Azure oluşturmak için uygulamaları İşlevler, Kiracı Portalı'nı kullanın ve Kiracı aboneliğinizin olması gerekir.

1. Azure yığın Kiracı Portalı'nda tıklatın **yeni** > **Web + mobil** > **Web uygulaması**.

2. Üzerinde **Web uygulaması** dikey penceresinde, bir ad yazın **Web uygulaması** kutusu.

3. Altında **kaynak grubu**, tıklatın **yeni**. Bir ad yazın **kaynak grubu** kutusu.

4. **App Service planı/Konum** > **Yeni Oluştur**'a tıklayın.

5. Üzerinde **uygulama hizmeti planı** dikey penceresinde, bir ad yazın **uygulama hizmeti planı** kutusu.

6. Tıklatın **fiyatlandırma katmanı** > **serbest paylaşılan** veya **paylaşılan paylaşılan** > **seçin**  >   **Tamam** > **oluşturma**.

7. Bir dakika altında yeni web uygulaması için bir kutucuğa panosunda görüntülenir. Kutucuğa tıklayın.

8. Üzerinde **Web uygulaması** dikey penceresinde tıklatın **Gözat** bu uygulama için varsayılan Web sitesini görüntülemek için.

## <a name="deploy-a-wordpress-dnn-or-django-website-optional"></a>Bir WordPress, DNN ya da Django Web sitesi (isteğe bağlı) dağıtma

1. Azure yığın Kiracı Portalı'nda tıklatın **+** Azure Marketi gidin, Django Web dağıtmak ve başarılı tamamlanmasını bekleyin. Django web platformu dosya sistemi tabanlı bir veritabanı kullanır. SQL veya MySQL gibi herhangi bir ek kaynak sağlayıcıları gerektirmez.

2. Bir MySQL kaynak sağlayıcısı ayrıca dağıttıysanız Marketi'nden bir WordPress Web sitesi dağıtabilirsiniz. Veritabanı parametreleri için istendiğinde, kullanıcı adı olarak girin *User1@Server1*, tercih ettiğiniz sunucu adını ve kullanıcı adı.

3. Ayrıca bir SQL Server Kaynak sağlayıcısı dağıttıysanız, DNN Web sitesi marketten dağıtabilirsiniz. Veritabanı parametreleri için istendiğinde, kaynak sağlayıcısına bağlı SQL Server çalıştıran bilgisayarda bir veritabanı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Ayrıca diğer deneyebilirsiniz [platform olarak hizmet (PaaS) Hizmetleri](azure-stack-tools-paas-services.md).

- [SQL Server Kaynak sağlayıcısı](azure-stack-sql-resource-provider-deploy.md)
- [MySQL kaynak sağlayıcısı](azure-stack-mysql-resource-provider-deploy.md)

<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[App_Service_Deployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525

<!--Image references-->
[1]: ./media/azure-stack-app-service-deploy-offline/app-service-exe-advanced-create-package.png
[2]: ./media/azure-stack-app-service-deploy-offline/app-service-exe-advanced-complete-offline.png
[3]: ./media/azure-stack-app-service-deploy-offline/app-service-azure-stack-arm-endpoints.png
[4]: ./media/azure-stack-app-service-deploy-offline/app-service-azure-stack-subscription-information.png
[5]: ./media/azure-stack-app-service-deploy-offline/app-service-default-VNET-config.png
[6]: ./media/azure-stack-app-service-deploy-offline/app-service-custom-VNET-config.png
[7]: ./media/azure-stack-app-service-deploy-offline/app-service-custom-VNET-config-with-values.png
[8]: ./media/azure-stack-app-service-deploy-offline/app-service-fileshare-configuration.png
[9]: ./media/azure-stack-app-service-deploy-offline/app-service-fileshare-configuration-error.png
[10]: ./media/azure-stack-app-service-deploy-offline/app-service-identity-app.png
[11]: ./media/azure-stack-app-service-deploy-offline/app-service-certificates.png
[12]: ./media/azure-stack-app-service-deploy-offline/app-service-sql-configuration.png
[13]: ./media/azure-stack-app-service-deploy-offline/app-service-sql-configuration-error.png
[14]: ./media/azure-stack-app-service-deploy-offline/app-service-cloud-quantities.png
[15]: ./media/azure-stack-app-service-deploy-offline/app-service-windows-image-selection.png
[16]: ./media/azure-stack-app-service-deploy-offline/app-service-role-credentials.png
[17]: ./media/azure-stack-app-service-deploy-offline/app-service-azure-stack-deployment-summary.png
[18]: ./media/azure-stack-app-service-deploy-offline/app-service-deployment-progress.png
