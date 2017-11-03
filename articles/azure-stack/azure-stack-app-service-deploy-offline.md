---
title: "Uygulama hizmeti çevrimdışı bir ortamda dağıtmak: Azure yığın | Microsoft Docs"
description: "AD FS tarafından güvenliği bağlantısı kesilmiş bir Azure yığın ortamında uygulama hizmeti dağıtma hakkında ayrıntılı yönergeler."
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: anwestg
ms.openlocfilehash: 8ee171708364c3e29476302bef04a715df650b9b
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
---
# <a name="add-an-app-service-resource-provider-to-a-disconnected-azure-stack-environment-secured-by-ad-fs"></a>AD FS tarafından güvenliği bağlantısı kesilmiş bir Azure yığın ortam için bir uygulama hizmeti kaynak Sağlayıcısı Ekle

Bu makaledeki yönergeleri izleyerek yükleyebilirsiniz [uygulama hizmeti kaynak sağlayıcısı](azure-stack-app-service-overview.md) bir Azure yığın ortam için:
- internet'e bağlı değil
- Active Directory Federasyon Hizmetleri (AD FS) tarafından güvenli.

Uygulama hizmeti kaynak sağlayıcısı, çevrimdışı Azure yığın dağıtımına eklemek için bu üst düzey görevleri tamamlamanız gerekir:

1. Tamamlamak [önkoşul adımlarını](azure-stack-app-service-before-you-get-started.md) (Sertifikalar satın alma gibi gerçekleştirebileceğiniz almak için birkaç gün).
2. [İndirme ve yükleme ve yardımcı dosyaları ayıklayın](azure-stack-app-service-before-you-get-started.md) internet'e bağlı bir makineye.
3. Bir çevrimdışı yükleme paketi oluşturun.
4. Appservice.exe yükleyici dosyasını çalıştırın.

## <a name="create-an-offline-installation-package"></a>Bir çevrimdışı yükleme paketi oluşturun.

Uygulama hizmeti bağlantısı kesilmiş bir ortamda dağıtmak için ilk Internet'e bağlı bir makinede çevrimdışı yükleme paketi oluşturmalısınız.

1. Internet'e bağlı bir makinede AppService.exe yükleyiciyi çalıştırın.

2. Tıklatın **Gelişmiş** > **çevrimdışı yükleme paketi oluştur**.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy-offline/image01.png)   

3. Uygulama Hizmeti Yükleyici bir çevrimdışı yükleme paketi oluşturur ve yolunu görüntüler. Tıklayabilirsiniz **Klasör Aç** dosya Gezgini'nde klasörü açın.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy-offline/image02.png)   

4. Yükleyici (AppService.exe) ve çevrimdışı yükleme paketini Azure yığın ana makinenizdeki kopyalayın.

## <a name="complete-the-offline-installation-of-app-service-on-azure-stack"></a>Azure yığın uygulama hizmeti çevrimdışı yüklemesini tamamlayın

1. Bağlantısı kesilmiş Azure yığın ana makinede appservice.exe azurestack\clouadmin çalıştırın.

2. Tıklatın **Gelişmiş** > **tamamlamak çevrimdışı yükleme**.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy-offline/image03.png)   

3. Daha önce oluşturduğunuz çevrimdışı yükleme paketini konumuna göz atın ve ardından **sonraki**.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy-offline/image04.png)   

4. Gözden geçirin ve Microsoft Yazılım Lisans Koşulları'nı kabul edin ve ardından **sonraki**.

5. Gözden geçirin ve üçüncü taraf Lisans Koşulları'nı kabul edin ve ardından **sonraki**.

6. Uygulama hizmeti bulut yapılandırma bilgilerinin doğru olduğundan emin olun. Varsayılan ayarları Azure yığın Geliştirme Seti dağıtımı sırasında kullanılan, varsayılan değerleri kabul edebilir. Ancak, Azure yığın dağıtıldığında seçenekleri özelleştirdiyseniz, yansıtmak üzere bu penceresindeki değerleri düzenlemeniz gerekir. Örneğin, etki alanı soneki mycloud.com kullanırsanız, uç noktanız için management.mycloud.com değiştirmeniz gerekir. Bilgilerinizi doğruladıktan sonra tıklatın **sonraki**.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy/image02.png)

7. Sonraki sayfada:
    1. Tıklatın **Bağlan** düğmesine **Azure yığın abonelikleri** kutusu.
        - Azure Active Directory (Azure AD) kullanıyorsanız, Azure AD yönetici hesabı ve Azure yığın dağıtıldığında, verdiğiniz parolayı girin. Tıklatın **oturum**.
        - Active Directory Federasyon Hizmetleri (AD FS) kullanıyorsanız, yönetici hesabı sağlayın. Örneğin, cloudadmin@azurestack.local. Parolanızı girin ve tıklayın **oturum**.
    2. İçinde **Azure yığın abonelikleri** kutusunda, aboneliğinizi seçin.
    3. İçinde **Azure yığın konumu** kutusunda, dağıtımına bölgeyi karşılık gelen konumu seçin. Örneğin, seçin **yerel** varsa Azure yığın Geliştirme Seti dağıtma.
    4. Girin bir **kaynak grubu adı** uygulama hizmeti dağıtımınız için. Varsayılan olarak ayarlanır **APPSERVICE\<mobil\>**.
    5. Girin **depolama hesabı adı** yüklemesinin bir parçası oluşturmak için uygulama hizmeti istiyor. Varsayılan olarak ayarlanır **appsvclocalstor**.
    6. **İleri**’ye tıklayın.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy/image03.png)

8. Dosya Paylaşımı için bilgileri girin ve ardından **sonraki**. Dosya Paylaşımı adresi tam etki alanı adını, dosya sunucunuz örneğin kullanmalıdır \\\appservicefileserver.local.cloudapp.azurestack.external\websites ya da IP adresi, örneğin \\\10.0.0.1\websites.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy/image04.png)

9. Sonraki sayfada:
    1. İçinde **kimlik uygulama kimliği** kutusuna, kimliği için kullanmakta olduğunuz uygulama için GUID girin.
    2. İçinde **kimlik uygulama sertifika dosyası** kutusuna girin (veya göz atın) sertifika dosyası konumu.
    3. İçinde **kimlik uygulama sertifika parolası** kutusunda, sertifikanın parolasını girin. Sertifikalar oluşturmak üzere komut kullanıldığında Not yapılan bir paroladır.
    4. İçinde **Azure Resource Manager kök sertifika dosyasını** kutusuna girin (veya göz atın) sertifika dosyası konumu.
    5. **İleri**’ye tıklayın.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy/image05.png)

10. Her üç sertifika dosya kutularında, **Gözat** uygun sertifika dosyasına gidin ve bir parola yazın. Bu sertifikalar, oluşturduğunuz olanlardır [oluşturma gerekli sertifikaları adım](azure-stack-app-service-deploy.md). Tıklatın **sonraki** tüm bilgileri girdikten sonra.

    | Box | Sertifika dosyası adı örneği |
    | --- | --- |
    | **Uygulama hizmeti varsayılan SSL sertifika dosyası** | \_. appservice.local.AzureStack.external.pfx |
    | **Uygulama hizmeti API SSL sertifika dosyası** | api.appservice.local.AzureStack.external.pfx |
    | **Uygulama hizmeti yayımcı SSL sertifika dosyası** | ftp.appservice.local.AzureStack.external.pfx |

    Sertifikaları oluşturduğunuzda farklı etki alanı soneki kullandıysanız, sertifika dosya adları kullanmayın *yerel. AzureStack.external*. Bunun yerine, özel etki alanı bilgilerinizi kullanın.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy/image06.png)    

11. Uygulama hizmeti kaynak sağlayıcısı veritabanlarını barındırmak ve ardından için kullanılan sunucu örneği için SQL Server ayrıntılarını girin **sonraki**. Yükleyici SQL bağlantı özelliklerini doğrulama.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy/image07.png)    

12. Rol örneği ve SKU seçenekleri gözden geçirin. Varsayılan örneği ve minimum SKU ASDK dağıtımında her rol için minimum sayısı ile doldurulur. Çekirdek ve bellek gereksinimlerini özetini dağıtımınızı planlamaya yardımcı olması için sağlanmıştır. Seçimlerinizi yaptıktan sonra tıklatın **sonraki**.

     > [!NOTE]
     > Üretim dağıtımlarında yer alan yönergeleri izleyerek, [kapasite Azure yığınında Azure App Service sunucu rolleri için planlama](azure-stack-app-service-capacity-planning.md).
     > 
     >

    | Rol | Minimum örnekleri | Minimum SKU | Notlar |
    | --- | --- | --- | --- |
    | Denetleyici | 1 | Standard_A1 - (1 çekirdek, 1792 MB) | Yönetir ve uygulama hizmeti bulut durumunu korur. |
    | Yönetim | 1 | Standard_A2 - (2 Çekirdek, 3584 MB) | Uygulama hizmeti Azure Resource Manager ve API uç noktaları, portal Uzantıları (yönetici, Kiracı, işlevleri portalına) ve veri hizmeti yönetir. Yük devretme desteği için 2 için önerilen örnekleri artar. |
    | Yayımcı | 1 | Standard_A1 - (1 çekirdek, 1792 MB) | FTP ve web dağıtımı üzerinden içerik yayımlar. |
    | FrontEnd | 1 | Standard_A1 - (1 çekirdek, 1792 MB) | Uygulama hizmeti uygulamaları isteklerini yönlendirir. |
    | Paylaşılan çalışan | 1 | Standard_A1 - (1 çekirdek, 1792 MB) | Ana web veya API uygulamaları ve Azure işlevleri uygulamalar. Daha fazla örnek eklemek isteyebilirsiniz. Bir operatör olarak teklifinizle tanımlamak ve herhangi bir SKU katmanı seçin. Katman bir çekirdek en az olması gerekir. |

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy/image08.png)    

    > [!NOTE]
    > **Windows Server 2016 Core Azure yığında Azure uygulama hizmeti ile kullanılmak üzere desteklenen platform görüntüsü değil**.

13. İçinde **Platform Görüntüsü Seç** kutusunda, uygulama hizmeti bulut bilgi işlem kaynak sağlayıcısındaki kullanılabilir olanlardan dağıtım Windows Server 2016 sanal makine görüntüsü seçin. **İleri**’ye tıklayın.

14. Sonraki sayfada:
     1. Çalışan rolü sanal makine yönetici kullanıcı adını ve parolasını girin.
     2. Diğer roller sanal makine yönetici kullanıcı adını ve parolasını girin.
     3. **İleri**’ye tıklayın.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy/image09.png)    

15. Özet sayfasında:
    1. Yaptığınız seçimleri doğrulayın. Değişiklik yapmak için kullanın **önceki** düğmeleri önceki sayfaları ziyaret edin.
    2. Yapılandırmaları doğruysa, onay kutusunu seçin.
    3. Dağıtımı başlatmak için tıklatın **sonraki**.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy/image10.png)    

16. Sonraki sayfada:
    1. Yükleme ilerleme durumunu izler. Azure yığın uygulama hizmeti varsayılan seçimlere göre dağıtmak için yaklaşık 60 dakika sürer.
    2. Yükleyici başarıyla tamamladıktan sonra **çıkış**.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy/image11.png)    


## <a name="validate-the-app-service-on-azure-stack-installation"></a>Uygulama hizmeti Azure yığın yüklemede doğrula

1. Azure yığın Yönetim Portalı'nda Git **yönetim - uygulama hizmeti**.

2. Durumu altında genel bakışta, denetleyin **durum** gösterir **tüm rolleri hazır**.

    ![Uygulama Hizmeti Yönetimi](media/azure-stack-app-service-deploy/image12.png)    


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

1. Azure yığın Kiracı Portalı'nda tıklatın  **+** Azure Marketi gidin, Django Web dağıtmak ve başarılı tamamlanmasını bekleyin. Django web platformu dosya sistemi tabanlı bir veritabanı kullanır. SQL veya MySQL gibi herhangi bir ek kaynak sağlayıcıları gerektirmez.

2. Bir MySQL kaynak sağlayıcısı ayrıca dağıttıysanız Marketi'nden bir WordPress Web sitesi dağıtabilirsiniz. Veritabanı parametreleri için istendiğinde, kullanıcı adı olarak girin  *User1@Server1* , tercih ettiğiniz sunucu adını ve kullanıcı adı.

3. Ayrıca bir SQL Server Kaynak sağlayıcısı dağıttıysanız, DNN Web sitesi marketten dağıtabilirsiniz. Veritabanı parametreleri için istendiğinde, kaynak sağlayıcısına bağlı SQL Server çalıştıran bilgisayarda bir veritabanı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Ayrıca diğer deneyebilirsiniz [platform olarak hizmet (PaaS) Hizmetleri](azure-stack-tools-paas-services.md).

- [SQL Server Kaynak sağlayıcısı](azure-stack-sql-resource-provider-deploy.md)
- [MySQL kaynak sağlayıcısı](azure-stack-mysql-resource-provider-deploy.md)

<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[App_Service_Deployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
