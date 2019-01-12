---
title: Çevrimdışı bir ortamda Azure Stack'te App Service'e dağıtma | Microsoft Docs
description: AD FS tarafından güvenliği bağlantısı kesilmiş bir Azure Stack ortamında App Service'e dağıtım yapmak hakkında ayrıntılı yönergeler.
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
ms.date: 01/11/2019
ms.author: anwestg
ms.openlocfilehash: db4c0f2d1197a190b33bd297bb597fd19057d875
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2019
ms.locfileid: "54230348"
---
# <a name="add-an-app-service-resource-provider-to-a-disconnected-azure-stack-environment-secured-by-ad-fs"></a>AD FS tarafından güvenliği bağlantısı kesilmiş bir Azure Stack ortamına bir App Service kaynak sağlayıcısı ekleme

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

> [!IMPORTANT]
> Azure Stack tümleşik sisteminize 1809 güncelleştirmesini veya Azure App Service 1.4 dağıtmadan önce en son Azure Stack geliştirme Seti'ni dağıtın.

Bu makalede bulunan yönergeleri izleyerek yükleyebilirsiniz [App Service kaynak sağlayıcısı](azure-stack-app-service-overview.md) bir Azure Stack ortamı için:

- Internet'e bağlı değil
- Active Directory Federasyon Hizmetleri (AD FS) tarafından güvenli.

 > [!IMPORTANT]
 > Kaynak sağlayıcısını dağıtmadan önce yeni işlevler, düzeltmeler ve dağıtımınızı etkileyebilecek bilinen sorunlar hakkında bilgi edinmek için sürüm notlarını gözden geçirin.
 
App Service kaynak sağlayıcısı çevrimdışı Azure Stack dağıtımınıza eklemek için bu üst düzey görevleri tamamlamanız gerekir:

1. Tamamlamak [önkoşul adımlarını](azure-stack-app-service-before-you-get-started.md) (sertifikaları satın gibi hangi gerçekleştirebileceğiniz almak için birkaç gün).
2. [İndirme ve yükleme ve yardımcı dosyaları ayıklayın](azure-stack-app-service-before-you-get-started.md) Internet'e bağlı bir makinede.
3. Çevrimdışı yükleme paketi oluşturun.
4. Appservice.exe yükleyici dosyasını çalıştırın.

## <a name="create-an-offline-installation-package"></a>Çevrimdışı yükleme paketi oluşturun

App Service bağlantısı kesilmiş bir ortamda dağıtmak için önce Internet'e bağlı bir makinede çevrimdışı yükleme paketi oluşturmalısınız.

1. Internet'e bağlı bir makinede AppService.exe yükleyiciyi çalıştırın.

2. Tıklayın **Gelişmiş** > **çevrimdışı yükleme paketi oluştur**.

    ![Uygulama hizmeti yükleyicisi][1]

3. App Service yükleyici çevrimdışı yükleme paketi oluşturur ve ona yolunu görüntüler. Tıklayabilirsiniz **Klasör Aç** klasörü dosya gezgini'nizi açın.

    ![Uygulama hizmeti yükleyicisi](media/azure-stack-app-service-deploy-offline/image02.png)

4. Yükleyici (AppService.exe) ve çevrimdışı yükleme paketi, Azure Stack ana makinesi için kopyalayın.

## <a name="complete-the-offline-installation-of-app-service-on-azure-stack"></a>Azure Stack üzerinde App Service'nin çevrimdışı yüklemesini tamamlayın

1. Appservice.exe Azure Stack yönetici Azure kaynak yönetimi uç nokta ulaşan bir bilgisayarda bir yönetici olarak çalıştırın.

2. Tıklayın **Gelişmiş** > **tamamlamak çevrimdışı yükleme**.

    ![Uygulama hizmeti yükleyicisi][2]

3. Önceden oluşturduğunuz çevrimdışı yükleme paketinin konumuna göz atın ve ardından **sonraki**.

    ![Uygulama hizmeti yükleyicisi](media/azure-stack-app-service-deploy-offline/image04.png)

4. Gözden geçirin ve Microsoft yazılım lisans koşullarını kabul edin ve ardından **sonraki**.

5. Gözden geçirin ve üçüncü taraf lisans koşullarını kabul edin ve ardından **sonraki**.

6. App Service bulut yapılandırması bilgilerin doğru olduğundan emin olun. Azure Stack geliştirme Seti'ni dağıtım sırasında varsayılan ayarları kullandıysanız, varsayılan değerleri kabul edebilir. Ancak, Azure Stack dağıtılan veya tümleşik bir sistemde dağıtma seçenekleri özelleştirdiyseniz, bu pencerede, değişimi yansıtmak için değerleri düzenlemeniz gerekir. Örneğin, etki alanı soneki mycloud.com kullanırsanız, Azure Stack Kiracı Azure Resource Manager uç noktanız için yönetim değiştirmeniz gerekir. <region>. mycloud.com. Bilgilerinizi onayladıktan sonra tıklayın **sonraki**.

    ![Uygulama hizmeti yükleyicisi][3]

7. Sonraki sayfada:
    1. Tıklayın **Connect** düğmesinin yanındaki **Azure Stack aboneliklerini** kutusu.
        - Yönetici hesabı sağlayın. Örneğin, cloudadmin@azurestack.local. Parolanızı girin ve tıklayın **oturum**.
    2. İçinde **Azure Stack aboneliklerini** kutusunda **varsayılan sağlayıcı aboneliği**.
    
    > [!NOTE]
    > App Service yalnızca dağıtılabilir içine **varsayılan sağlayıcı aboneliği**.
    >
    
    3. İçinde **Azure Stack konumları** kutusunda, için dağıtmakta bölgeyi karşılık gelen konum seçin. Örneğin, **yerel** , Azure Stack Geliştirme Seti için dağıtma.
    4. **İleri**’ye tıklayın.

    ![Uygulama hizmeti yükleyicisi][4]

8. Adım adım yapılandırılan mevcut bir sanal ağa dağıtma seçeneği artık sahip [burada](azure-stack-app-service-before-you-get-started.md#virtual-network), veya bir sanal ağ ve ilişkili alt ağlar oluşturmak App Service yükleyici izin verin.
    1. Seçin **sanal ağ oluşturma varsayılan ayarlarla**Varsayılanları kabul edin ve ardından **sonraki**, veya;
    2. Seçin **mevcut bir VNet ve alt ağları kullan**.
        1. Seçin **kaynak grubu** sanal ağınıza; içeren
        2. Doğru seçin **sanal ağ** ; dağıtmak istediğiniz adı
        3. Doğru seçin **alt** gerekli rol alt ağlar; değerleri
        4. **İleri**’ye tıklayın

    ![Uygulama hizmeti yükleyicisi][5]

9. Dosya paylaşımınızın bilgilerini girin ve ardından **sonraki**. Dosya Paylaşımı adresi, tam etki alanı adı veya dosya sunucunuzun IP adresini kullanmanız gerekir. Örneğin, \\\appservicefileserver.local.cloudapp.azurestack.external\websites, veya \\\10.0.0.1\websites

    > [!NOTE]
    > Yükleyici, devam etmeden önce dosya paylaşımını bağlantısını test etmek çalışır.  Bununla birlikte, mevcut bir sanal ağ dağıtmayı seçerseniz, yükleyici dosya paylaşımına bağlanmak mümkün olmayabilir ve devam etmek isteyip istemediğinizi soran bir uyarı görüntüler.  Dosya Paylaşımı bilgileri doğrulayın ve doğru olup olmadıklarını devam edin.
    >
    >

   ![Uygulama hizmeti yükleyicisi][8]

10. Sonraki sayfada:
    1. İçinde **Identity Application kimliği** kutusunda, kimlik (Azure AD) için kullanmakta olduğunuz uygulama için GUID girin.
    2. İçinde **Identity Application sertifika dosyası** kutusuna girin (veya göz atın) sertifika dosyasının konumu.
    3. İçinde **Identity Application sertifikası parolası** kutusunda, sertifikanın parolasını girin. Bu sertifikaları oluşturmak için betik kullanıldığında Not bir paroladır.
    4. İçinde **Azure Resource Manager kök sertifika dosyasını** kutusuna girin (veya göz atın) sertifika dosyasının konumu.
    5. **İleri**’ye tıklayın.

    ![Uygulama hizmeti yükleyicisi][10]

11. Her üç sertifika dosya kutularında, **Gözat** ve ardından uygun sertifika dosyasına gidin. Her sertifika için parola belirtmeniz gerekir. Bu sertifikalar, oluşturduğunuz olanlardır [Oluştur gerekli sertifikalar adımında](azure-stack-app-service-before-you-get-started.md#get-certificates). Tıklayın **sonraki** tüm bilgileri girdikten sonra.

    | Box | Sertifika dosyası adı örneği |
    | --- | --- |
    | **App Service varsayılan SSL sertifika dosyası** | \_.appservice.local.AzureStack.external.pfx |
    | **App Service API SSL sertifika dosyası** | api.appservice.local.AzureStack.external.pfx |
    | **App Service yayımcı SSL sertifika dosyası** | ftp.appservice.local.AzureStack.external.pfx |

    Sertifikaları oluştururken farklı bir etki alanı soneki kullandıysanız, sertifika dosya adlarını kullanmayın *yerel. AzureStack.external*. Bunun yerine, özel etki alanı bilginizi kullanın.

    ![Uygulama hizmeti yükleyicisi][11]

12. App Service kaynak sağlayıcısı veritabanlarını barındırmak ve ardından kullanılacak sunucu örneği için SQL Server ayrıntıları girin **sonraki**. Yükleyici SQL bağlantı özelliklerini doğrular. **Gerekir** iç IP ya da SQL Server adı için tam etki alanı adı girin.

    > [!NOTE]
    > Yükleyici, devam etmeden önce SQl Server bağlantısını test etmek çalışır.  Bununla birlikte, mevcut bir sanal ağ dağıtmayı seçerseniz, yükleyici SQL Server'a bağlanmak mümkün olmayabilir ve devam etmek isteyip istemediğinizi soran bir uyarı görüntüler.  SQL Server bilgilerini doğrulayın ve doğru olup olmadıklarını devam edin.
    >
    > Azure App Service'ten Azure Stack 1.3 ve üzeri üzerinde yükleyici SQL Server veritabanı kapsama SQL sunucu düzeyinde etkin olduğunu denetler.  Yüklü değilse, şu özel durumla istenir:
    > ```sql
    >    Enable contained database authentication for SQL server by running below command on SQL server (Ctrl+C to copy)
    >    ***********************************************************
    >    sp_configure 'contained database authentication', 1;  
    >    GO  
    >    RECONFIGURE;  
    >    GO
    >    ***********************************************************
    > ```
    > Başvurmak [Azure Stack 1.3 üzerinde Azure App Service için sürüm notları](azure-stack-app-service-release-notes-update-three.md) daha fazla ayrıntı için.
   
   ![Uygulama hizmeti yükleyicisi][12]

13. Rol örneği ve SKU seçenekleri gözden geçirin. Varsayılan örnekleri ve en az bir SKU ASDK dağıtımdaki her bir rol için en düşük sayısı ile doldurulur. Dağıtımınızı planlamanıza yardımcı olacak vCPU ve bellek gereksinimlerinin bir özeti sağlanır. Seçimlerinizi yaptıktan sonra tıklayın **sonraki**.

     > [!NOTE]
     > Üretim dağıtımları için sunulan yönergeleri [kapasite Azure Stack'te Azure App Service sunucu rolleri için planlama](azure-stack-app-service-capacity-planning.md).
     >
     >

    | Rol | En düşük örnekleri | En az bir SKU | Notlar |
    | --- | --- | --- | --- |
    | Denetleyici | 1 | Standard_a2 = - (2 vCPU, 3584 MB) | Yönetir ve App Service bulut durumu korur. |
    | Yönetim | 1 | Standard_a2 = - (2 Vcpu, 3584 MB) | App Service Azure Resource Manager ve API uç noktaları, portal Uzantıları (yönetici, Kiracı, işlevleri portalına) ve veri hizmeti yönetir. Yük devretme desteklemek için 2 için önerilen örnekleri artar. |
    | Yayımcı | 1 | Standard_A1 - (1 vCPU, 1792 MB) | FTP ve web dağıtımı üzerinden içerik yayımlar. |
    | FrontEnd | 1 | Standard_A1 - (1 vCPU, 1792 MB) | App Service uygulamaları, istekleri yönlendirir. |
    | Paylaşılan çalışan | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Ana web veya API uygulamaları ve Azure işlev uygulamaları. Daha fazla örnek eklemek isteyebilirsiniz. Bir operatör olarak, teklifiniz tanımlayabilir ve herhangi bir SKU katmanı seçin. Katmanları, en az bir vCPU olması gerekir. |

    ![Uygulama hizmeti yükleyicisi][14]

    > [!NOTE]
    > **Windows Server 2016 Core Azure Stack'te Azure App Service ile kullanmak için desteklenen bir platform görüntüsü değil.  Üretim dağıtımları için değerlendirme görüntüleri kullanmayın.  Azure Stack'te Azure App Service, Microsoft.Net 3.5.1 SP1 dağıtım için kullanılan görüntüye etkinleştirilmesini gerektirir.   Market'te genel olarak Windows Server 2016 görüntüleri etkin bu özelliğe sahip değildir, bu nedenle oluşturmak ve bir Windows Server 2016 görüntüsü önceden etkinleştirildiğinde kullanmanız gerekir.**

14. İçinde **Platform görüntüsü seçin** kutusunda, bu işlem kaynak sağlayıcısı App Service bulut için kullanılabilir dağıtım Windows Server 2016 sanal makine görüntünüzü seçin. **İleri**’ye tıklayın.

15. Sonraki sayfada:
     1. Çalışan rolü sanal makine yönetici kullanıcı adı ve parola girin.
     2. Diğer roller sanal makine yönetici kullanıcı adı ve parola girin.
     3. **İleri**’ye tıklayın.

    ![Uygulama hizmeti yükleyicisi][16]

16. Özet sayfasında:
    1. Yaptığınız seçimleri doğrulayın. Değişiklik yapmak için kullanmanız **önceki** düğmelerini önceki sayfalarını ziyaret edin.
    2. Yapılandırmaları doğruysa, onay kutusunu işaretleyin.
    3. Dağıtımı başlatmak için tıklatın **sonraki**.

    ![Uygulama hizmeti yükleyicisi][17]

17. Sonraki sayfada:
    1. Yükleme ilerleme durumunu izleyin. Azure Stack üzerinde App Service varsayılan seçimlere göre dağıtmak için yaklaşık 60 dakika sürer.
    2. Yükleyici başarıyla tamamladıktan sonra **çıkış**.

    ![Uygulama hizmeti yükleyicisi][18]

## <a name="validate-the-app-service-on-azure-stack-installation"></a>Yükleme Azure Stack üzerinde App Service'te doğrula

1. Azure Stack Yönetim Portalı'nda Git **yönetim - App Service**.

2. Durumu altında genel bakış, görmek için iade **durumu** görüntüler **tüm roller hazır**.

    ![App Service Yönetimi](media/azure-stack-app-service-deploy/image12.png)

> [!NOTE]
> Mevcut bir sanal ağ ve dosya için bağlanmak için bir iç IP adresi dağıtmayı seçerseniz, çalışan alt ağ ve dosya sunucusu arasında SMB trafiği etkinleştirme bir giden güvenlik kuralı eklemeniz gerekir.  Bunu yapmak için Yönetim Portalı'nda WorkersNsg gidin ve aşağıdaki özelliklere sahip bir giden güvenlik kuralı ekleyin:
> * Kaynak: Herhangi biri
> * Kaynak bağlantı noktası aralığı: *
> * Hedef: IP Adresleri
> * Hedef IP adresi aralığı: Dosya için IP aralığı
> * Hedef bağlantı noktası aralığı: 445
> * Protokol: TCP
> * Eylem: İzin Ver
> * Önceliği: 700
> * Ad: Outbound_Allow_SMB445
>

## <a name="test-drive-app-service-on-azure-stack"></a>Azure Stack üzerinde test sürüşü App Service

Dağıtma ve App Service kaynak sağlayıcısı kaydetme sonra kullanıcılar, web ve API uygulamaları dağıtabileceğiniz emin olmak için test edin.

> [!NOTE]
> Planı içinde Microsoft.Web ad alanı olan bir teklif oluşturun gerekir. Ardından, bu teklife abone olan bir kiracı aboneliğinizin olması gerekir. Daha fazla bilgi için [oluştur Teklif](azure-stack-create-offer.md) ve [plan oluşturma](azure-stack-create-plan.md).
>
> *Gerekir* Azure Stack üzerinde App Service kullanan uygulamalar oluşturmak için bir kiracı aboneliğinizin olması. App Service kaynak sağlayıcısı Yönetim için bir Hizmet Yöneticisi Yönetici portalında tamamlayabilirsiniz yalnızca özellikleri ilgilidir. Bu özellikler, kapasite ekleme, dağıtım kaynaklarını yapılandırma ve çalışan katmanları ve SKU'ları ekleme içerir.
>
> Üçüncü Teknik Önizleme'den itibaren web API ve Azure'ı oluşturmak için uygulamaları İşlevler, Kiracı portalı kullanın ve Kiracı aboneliğinizin olması gerekir.

1. Azure Stack Kiracı portalında **+ kaynak Oluştur** > **Web + mobil** > **Web uygulaması**.

2. Üzerinde **Web uygulaması** dikey penceresinde, bir ad yazın **Web uygulaması** kutusu.

3. Altında **kaynak grubu**, tıklayın **yeni**. Bir ad yazın **kaynak grubu** kutusu.

4. **App Service planı/Konum** > **Yeni Oluştur**'a tıklayın.

5. Üzerinde **App Service planı** dikey penceresinde, bir ad yazın **App Service planı** kutusu.

6. Tıklayın **fiyatlandırma katmanı** > **ücretsiz paylaşılan** veya **paylaşılan paylaşılan** > **seçin**  >   **Tamam** > **oluşturma**.

7. Bir dakikadan yeni web uygulaması için bir kutucuk Panoda görüntülenir. Kutucuğa tıklayın.

8. Üzerinde **Web uygulaması** dikey penceresinde tıklayın **Gözat** bu uygulama için varsayılan Web sitesini görüntülemek için.

## <a name="deploy-a-wordpress-dnn-or-django-website-optional"></a>WordPress, DNN ve Django Web sitesi (isteğe bağlı) dağıtma

1. Azure Stack Kiracı portalında **+**, Azure Marketi'nde gidin, Django Web sitesi dağıtma ve başarıyla tamamlanmasını bekleyin. Django web platformu, dosya sistemi tabanlı bir veritabanı kullanır. Bu, SQL veya MySQL gibi herhangi bir ek kaynak sağlayıcıları olmasını gerektirmez.

2. Ayrıca bir MySQL kaynak sağlayıcısı dağıtılan marketten bir WordPress Web sitesi dağıtabilirsiniz. Veritabanı parametrelerini sorulduğunda, kullanıcı adı olarak girin *User1@Server1*, tercih ettiğiniz sunucu adı ve kullanıcı adı.

3. Ayrıca bir SQL Server Kaynak sağlayıcısı dağıtılan marketten bir DNN Web sitesi dağıtabilirsiniz. Veritabanı parametrelerini istendiğinde, kaynak sağlayıcısı için bağlı SQL Server çalıştıran bilgisayardaki bir veritabanı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Diğer de deneyebilirsiniz [platform olarak hizmet (PaaS) Hizmetleri](azure-stack-tools-paas-services.md).

- [SQL Server Kaynak sağlayıcısı](azure-stack-sql-resource-provider-deploy.md)
- [MySQL kaynak sağlayıcısı](azure-stack-mysql-resource-provider-deploy.md)

<!--Links-->
[Azure_Stack_App_Service_preview_installer]: https://go.microsoft.com/fwlink/?LinkID=717531
[App_Service_Deployment]: https://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: https://go.microsoft.com/fwlink/?LinkId=733525

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
