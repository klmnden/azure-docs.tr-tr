---
title: 'Uygulama Hizmetleri dağıtın: Azure yığını | Microsoft Docs'
description: App Service Azure Stack'te dağıtma hakkında ayrıntılı kılavuz
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
ms.date: 08/15/2018
ms.author: anwestg
ms.openlocfilehash: 9173dfcbb5f73c2292bce7d28c2dae5dbece79cd
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42060705"
---
# <a name="add-an-app-service-resource-provider-to-azure-stack"></a>Azure Stack'e bir App Service kaynak sağlayıcısı ekleme

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack'te App Service'e dağıtım yapmak, bu makaledeki yönergeleri kullanın.

>[!IMPORTANT]  
>Azure Stack tümleşik sisteminize 1807 güncelleştirmeyi veya Azure App Service 1.3 dağıtmadan önce en son Azure Stack geliştirme Seti'ni (ASDK) dağıtın.

Kullanıcılarınız, web ve API uygulamaları oluşturma olanağı verebilirsiniz. Kullanıcıların bu uygulamaları oluşturmak için gerekir:

 - Ekleme [App Service kaynak sağlayıcısı](azure-stack-app-service-overview.md) bu makalede açıklanan adımları kullanarak Azure Stack dağıtımınıza.
 - App Service kaynak Sağlayıcısı'nı yükledikten sonra teklifler ve planlar içerebilir. Kullanıcıların hizmete almak ve uygulamalar oluşturmaya başlamak için abone olabilirsiniz.

> [!IMPORTANT]  
> ' Deki yönergeleri izlediğinizden emin olun, kaynak sağlayıcı yükleyicisini çalıştırmadan önce [başlamadan önce](azure-stack-app-service-before-you-get-started.md).

## <a name="run-the-app-service-resource-provider-installer"></a>App Service kaynak sağlayıcısı yükleyiciyi çalıştırın

App Service kaynak Sağlayıcısı'nı yükleme, en az bir saat sürer. Kaç rol örnekleri üzerinde gereken sürenin uzunluğunu bağlıdır dağıtın. Dağıtım sırasında aşağıdaki görevleri yükleyiciyi çalıştırır:

 - Belirtilen Azure Stack depolama hesabındaki bir blob kapsayıcısı oluşturun.
 - App Service için bir DNS bölgesi ve girişleri oluşturun.
 - App Service kaynak sağlayıcısını kaydedin.
 - App Service galeri öğelerini kaydedin.

App Service kaynak sağlayıcısı dağıtmak için aşağıdaki adımları izleyin:

1. Appservice.exe Azure Stack yönetici Azure kaynak yönetimi uç noktasını erişebilen bir bilgisayarda yönetici olarak çalıştırın.

2. Seçin **App Service dağıtın veya en son sürüme yükseltme**.

    ![Uygulama hizmeti yükleyicisi][1]

3. Gözden geçirin ve Microsoft yazılım lisans koşullarını kabul edin ve ardından **sonraki**.

4. Gözden geçirin ve üçüncü taraf lisans koşullarını kabul edin ve ardından **sonraki**.

5. App Service bulut yapılandırması bilgilerin doğru olduğundan emin olun. Azure Stack geliştirme Seti'ni (ASDK) dağıtım sırasında varsayılan ayarları kullandıysanız, varsayılan değerleri kabul edebilir. Ancak bu pencerede farklar yansıtacak şekilde değerleri ASDK dağıtılan ya da bir Azure Stack tümleşik sisteminde dağıtma seçenekleri özelleştirdiyseniz, düzenlemeniz gerekir.

   Örneğin, etki alanı soneki mycloud.com kullanırsanız, Azure Stack Kiracı Azure Resource Manager uç noktanız için yönetim değiştirmeniz gerekir. &lt;bölge&gt;. mycloud.com. Bu ayarları gözden geçirin ve ardından **sonraki** ayarları kaydetmek için.

   ![Uygulama hizmeti yükleyicisi][2]

6. App Service yükleyici sonraki sayfada, şu adımları izleyin:

    a. Seçin **Connect** yanındaki **Azure Stack aboneliklerini**.

     - Azure Active Directory (Azure AD) kullanıyorsanız, Azure AD yönetici hesabı ve Azure Stack dağıttığınızda sağladığınız parolayı girin. Seçin **oturum**.
     - Active Directory Federasyon Hizmetleri (AD FS) kullanıyorsanız, yönetici hesabı sağlayın. Örneğin, cloudadmin@azurestack.local. Parolanızı girin ve ardından **oturum**.

   b. İçinde **Azure Stack aboneliklerini**seçin **varsayılan sağlayıcı aboneliği**.

     >[!NOTE]
     >Şu anda, App Service yalnızca dağıtılabilir **varsayılan sağlayıcı aboneliği**.

   c. İçinde **Azure Stack konumları**, için dağıtmakta bölgesine karşılık gelen konum seçin. Örneğin, **yerel** , Azure Stack Geliştirme Seti için dağıtma.

    ![Uygulama hizmeti yükleyicisi][3]

7. Yapılandırdığınız mevcut bir sanal ağa dağıtabileceğiniz artık [aşağıdaki adımları kullanarak](azure-stack-app-service-before-you-get-started.md#virtual-network), ya da yeni bir sanal ağ oluşturup alt ağları App Service yükleyici sağlar. Bir VNet oluşturmak için aşağıdaki adımları izleyin:

   a. Seçin **sanal ağ oluşturma varsayılan ayarlarla**Varsayılanları kabul edin ve ardından **sonraki**.

   b. Alternatif olarak, seçin **mevcut bir VNet ve alt ağları kullan**. Aşağıdaki işlemleri tamamlayın:

     - Seçin **kaynak grubu** , sanal ağınızı içerir.
     - Seçin **sanal ağ** dağıtmak istediğiniz adı.
     - Doğru seçin **alt** gerekli rol alt ağın her biri için değerler.
     - **İleri**’yi seçin.

   ![Uygulama hizmeti yükleyicisi][4]

8. Dosya paylaşımınızın bilgilerini girin ve ardından **sonraki**. Dosya Paylaşımı adresi, tam etki alanı adı (FQDN) veya dosya sunucunuzun IP adresini kullanmanız gerekir. Örneğin, \\\appservicefileserver.local.cloudapp.azurestack.external\websites, veya \\\10.0.0.1\websites.

   >[!NOTE]
   >Yükleyici, devam etmeden önce dosya paylaşımını bağlantısını test etmek çalışır. Ancak, bir sanal ağınız dağıtıyorsanız, bu bağlantı testi dosyası. Size bir uyarı ve bir komut istemi devam etmek için verilir. Fileshare bilgilerin doğru olduğundan, dağıtımın devam edin.

   ![Uygulama hizmeti yükleyicisi][7]

9. App Service yükleyici sonraki sayfada, şu adımları izleyin:

   a. İçinde **Identity Application kimliği** kutusunda, kimlik (Azure AD) için kullanmakta olduğunuz uygulama için GUID girin.

   b. İçinde **Identity Application sertifika dosyası** kutusuna girin (veya göz atın) sertifika dosyasının konumu.

   c. İçinde **Identity Application sertifikası parolası** kutusunda, sertifikanın parolasını girin. Bu sertifikaları oluşturmak için betik kullanıldığında Not bir paroladır.

   d. İçinde **Azure Resource Manager kök sertifika dosyasını** kutusuna girin (veya göz atın) sertifika dosyasının konumu.

   e. **İleri**’yi seçin.

   ![Uygulama hizmeti yükleyicisi][9]

10. Her üç sertifika dosyası kutularının **Gözat** ve uygun sertifika dosyasına gidin. Her sertifika için parola belirtmeniz gerekir. Bu sertifikalar, oluşturduğunuz olanlardır [Oluştur gerekli sertifikalar adımında](azure-stack-app-service-before-you-get-started.md#get-certificates). Seçin **sonraki** tüm bilgileri girdikten sonra.

    | Box | Sertifika dosyası adı örneği |
    | --- | --- |
    | **App Service varsayılan SSL sertifika dosyası** | \_.appservice.local.AzureStack.external.pfx |
    | **App Service API SSL sertifika dosyası** | api.appservice.local.AzureStack.external.pfx |
    | **App Service yayımcı SSL sertifika dosyası** | ftp.appservice.local.AzureStack.external.pfx |

    Sertifikaları oluştururken farklı bir etki alanı soneki kullandıysanız, sertifika dosya adlarını kullanmayın *yerel. AzureStack.external*. Bunun yerine, özel etki alanı bilginizi kullanın.

    ![Uygulama hizmeti yükleyicisi][10]

11. App Service kaynak sağlayıcısı veritabanlarını barındırmak ve ardından seçmek için kullanılan sunucu örneği için SQL Server ayrıntıları girin **sonraki**. Yükleyici SQL bağlantı özelliklerini doğrular.

    > [!NOTE]
    > Yükleyici, devam etmeden önce SQL Server bağlantısını test etmek çalışır. Ancak, bir sanal ağınız dağıtıyorsanız, bu bağlantı testi dosyası. Size bir uyarı ve bir komut istemi devam etmek için verilir. SQL Server bilgilerin doğru olduğundan, dağıtımın devam edin.
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

    ![Uygulama hizmeti yükleyicisi][11]

12. Rol örneği ve SKU seçenekleri gözden geçirin. En az örnek sayısını ve ASDK dağıtımdaki her bir rol için en düşük SKU ile varsayılan değerleri doldurun. Dağıtımınızı planlamanıza yardımcı olacak vCPU ve bellek gereksinimlerinin bir özeti sağlanır. Seçimlerinizi yaptıktan sonra seçin **sonraki**.

    >[!NOTE]
    >İçindeki yönergeleri izleyerek, üretim dağıtımları için [kapasite Azure Stack'te Azure App Service sunucu rolleri için planlama](azure-stack-app-service-capacity-planning.md).

    | Rol | En düşük örnekleri | En az bir SKU | Notlar |
    | --- | --- | --- | --- |
    | Denetleyici | 1 | Standard_a2 = - (2 vCPU, 3584 MB) | Yönetir ve App Service bulut durumu korur. |
    | Yönetim | 1 | Standard_a2 = - (2 Vcpu, 3584 MB) | App Service Azure Resource Manager ve API uç noktaları, portal Uzantıları (yönetici, Kiracı, işlevleri portalına) ve veri hizmeti yönetir. Yük devretme desteklemek için 2 için önerilen örnekleri artar. |
    | Yayımcı | 1 | Standard_A1 - (1 vCPU, 1792 MB) | FTP ve web dağıtımı üzerinden içerik yayımlar. |
    | FrontEnd | 1 | Standard_A1 - (1 vCPU, 1792 MB) | App Service uygulamaları, istekleri yönlendirir. |
    | Paylaşılan çalışan | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Ana web veya API uygulamaları ve Azure işlev uygulamaları. Daha fazla örnek eklemek isteyebilirsiniz. Bir operatör olarak, teklifiniz tanımlayabilir ve herhangi bir SKU katmanı seçin. Katmanları, en az bir vCPU olması gerekir. |

    ![Uygulama hizmeti yükleyicisi][13]

    >[!NOTE]
    >**Windows Server 2016 Core Azure Stack'te Azure App Service ile kullanmak için desteklenen bir platform görüntüsü değil.  Üretim dağıtımları için değerlendirme görüntüleri kullanmayın.**

13. İçinde **Platform görüntüsü seçin** kutusunda, App Service bulut işlem kaynağı sağlayıcısında görüntülerinden dağıtımı Windows Server 2016 sanal makine görüntünüzü seçin. **İleri**’yi seçin.

14. App Service yükleyici sonraki sayfada, şu adımları izleyin:

     a. Çalışan rolü sanal makine yönetici kullanıcı adı ve parola girin.

     b. Diğer roller sanal makine yönetici kullanıcı adı ve parola girin.

     c. **İleri**’yi seçin.

    ![Uygulama hizmeti yükleyicisi][15]

15. App Service yükleyici Özet sayfasında, şu adımları izleyin:

    a. Yaptığınız seçimleri doğrulayın. Değişiklik yapmak için kullanmanız **önceki** düğmelerini önceki sayfalarını ziyaret edin.

    b. Yapılandırmaları doğruysa, onay kutusunu işaretleyin.

    c. Dağıtımı başlatmak için **sonraki**.

    ![Uygulama hizmeti yükleyicisi][16]

16. App Service yükleyici sonraki sayfada, şu adımları izleyin:

    a. Yükleme ilerleme durumunu izleyin. Azure Stack üzerinde App Service varsayılan seçimlere göre dağıtmak için yaklaşık 60 dakika sürer.

    b. Yükleyici başarıyla tamamlandıktan sonra Seç **çıkış**.

    ![Uygulama hizmeti yükleyicisi][17]

## <a name="validate-the-app-service-on-azure-stack-installation"></a>Yükleme Azure Stack üzerinde App Service'te doğrula

1. Azure Stack Yönetim Portalı'nda Git **yönetim - App Service**.

2. Genel bakış sayfasında durumu altında denetleyin **durumu** gösterir **tüm roller hazır**.

    ![App Service Yönetimi](media/azure-stack-app-service-deploy/image12.png)

   >[!IMPORTANT]
   >Mevcut bir sanal ağa dağıtma ve da dosya için bağlanmak için bir iç IP adresi kullanarak, bir giden güvenlik kuralı eklemeniz gerekir. Bu kural, çalışan alt ağ ve dosya sunucusu arasında SMB trafiği sağlar.  Bunu yapmak için Yönetim Portalı'nda WorkersNsg gidin ve aşağıdaki özelliklere sahip bir giden güvenlik kuralı ekleyin:<br>
    >  - Kaynak: tüm
    >  - Kaynak bağlantı noktası aralığı: *
    >  - Hedef: IP adresleri
    >  - Hedef IP adresi aralığı: dosya için IP aralığı
    >  - Hedef bağlantı noktası aralığı: 445
    >  - Protokol: TCP
    >  - Eylem: izin ver
    >  - Öncelik: 700
    >  - Ad: Outbound_Allow_SMB445

## <a name="test-drive-app-service-on-azure-stack"></a>Azure Stack üzerinde test sürüşü App Service

Dağıtma ve App Service kaynak sağlayıcısı kaydetme sonra kullanıcılar, web ve API uygulamaları dağıtabileceğiniz emin olmak için test edin.

>[!NOTE]
>Plandaki Microsoft.Web ad alanı olan bir teklif oluşturun gerekir. Ayrıca teklife abone olan bir kiracı aboneliği gerekir. Daha fazla bilgi için [oluştur Teklif](azure-stack-create-offer.md) ve [plan oluşturma](azure-stack-create-plan.md).
>
>*Gerekir* Azure Stack üzerinde App Service kullanan uygulamalar oluşturmak için bir kiracı aboneliğinizin olması. App Service kaynak sağlayıcısı Yönetim için bir Hizmet Yöneticisi Yönetim Portalı'nda gerçekleştirebileceğiniz görevleri ilgilidir. Bu, kapasite ekleme, dağıtım kaynaklarını yapılandırma ve çalışan katmanları ve SKU'ları ekleme içerir.
>
>Web API ve Azure'ı oluşturmak için uygulamaları İşlevler, Kiracı portalı kullanın ve Kiracı aboneliğinizin olması gerekir.
>

Web uygulaması oluşturmak için aşağıdaki adımları izleyin:

1. Azure Stack Kiracı portalında **yeni** > **Web + mobil** > **Web uygulaması**.

2. Altında **Web uygulaması**, bir ad girin **Web uygulaması**.

3. Altında **kaynak grubu**seçin **yeni**. İçin bir ad girin **kaynak grubu**.

4. Seçin **App Service planı/konumu** > **Yeni Oluştur**.

5. Altında **App Service planı**, için bir ad girin **App Service planı**.

6. Seçin **fiyatlandırma katmanı** > **ücretsiz paylaşılan** veya **paylaşılan paylaşılan** > **seçin**  >  **Tamam** > **oluşturma**.

7. Yeni web uygulaması için bir kutucuk Panoda görüntülenir. Kutucuğu seçin.

8. Üzerinde **Web uygulaması**seçin **Gözat** bu uygulama için varsayılan Web sitesini görüntülemek için.

## <a name="deploy-a-wordpress-dnn-or-django-website-optional"></a>WordPress, DNN ve Django Web sitesi (isteğe bağlı) dağıtma

1. Azure Stack Kiracı portalında **+**, Azure Marketi'nde gidin, Django Web sitesi dağıtma ve daha sonra dağıtımın tamamlanmasını bekleyin. Django web platformu, dosya sistemi tabanlı bir veritabanı kullanır. Bu, SQL veya MySQL gibi herhangi bir ek kaynak sağlayıcıları olmasını gerektirmez.

2. Ayrıca bir MySQL kaynak sağlayıcısı dağıtılan marketten bir WordPress Web sitesi dağıtabilirsiniz. Veritabanı parametrelerini sorulduğunda, kullanıcı adı olarak girin *User1@Server1*, tercih ettiğiniz sunucu adı ve kullanıcı adı.

3. Ayrıca bir SQL Server Kaynak sağlayıcısı dağıtılan marketten bir DNN Web sitesi dağıtabilirsiniz. Veritabanı parametrelerini istendiğinde, kaynak sağlayıcısı için bağlı SQL Server çalıştıran bilgisayardaki bir veritabanı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Diğer de deneyebilirsiniz [platform olarak hizmet (PaaS) Hizmetleri](azure-stack-tools-paas-services.md).

 - [SQL Server Kaynak sağlayıcısı](azure-stack-sql-resource-provider-deploy.md)
 - [MySQL kaynak sağlayıcısı](azure-stack-mysql-resource-provider-deploy.md)

<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[App_Service_Deployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525

<!--Image references-->
[1]: ./media/azure-stack-app-service-deploy/app-service-installer.png
[2]: ./media/azure-stack-app-service-deploy/app-service-azure-stack-arm-endpoints.png
[3]: ./media/azure-stack-app-service-deploy/app-service-azure-stack-subscription-information.png
[4]: ./media/azure-stack-app-service-deploy/app-service-default-VNET-config.png
[5]: ./media/azure-stack-app-service-deploy/app-service-custom-VNET-config.png
[6]: ./media/azure-stack-app-service-deploy/app-service-custom-VNET-config-with-values.png
[7]: ./media/azure-stack-app-service-deploy/app-service-fileshare-configuration.png
[8]: ./media/azure-stack-app-service-deploy/app-service-fileshare-configuration-error.png
[9]: ./media/azure-stack-app-service-deploy/app-service-identity-app.png
[10]: ./media/azure-stack-app-service-deploy/app-service-certificates.png
[11]: ./media/azure-stack-app-service-deploy/app-service-sql-configuration.png
[12]: ./media/azure-stack-app-service-deploy/app-service-sql-configuration-error.png
[13]: ./media/azure-stack-app-service-deploy/app-service-cloud-quantities.png
[14]: ./media/azure-stack-app-service-deploy/app-service-windows-image-selection.png
[15]: ./media/azure-stack-app-service-deploy/app-service-role-credentials.png
[16]: ./media/azure-stack-app-service-deploy/app-service-azure-stack-deployment-summary.png
[17]: ./media/azure-stack-app-service-deploy/app-service-deployment-progress.png