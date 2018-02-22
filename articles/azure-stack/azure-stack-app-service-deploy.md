---
title: "Uygulama Hizmetleri dağıtma: Azure yığın | Microsoft Docs"
description: "Azure yığın uygulama hizmeti dağıtma hakkında ayrıntılı kılavuz"
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2018
ms.author: brenduns
ms.reviewer: anwestg
ms.openlocfilehash: d4394463be02d067b8228099acd30a0421ce4be9
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="add-an-app-service-resource-provider-to-azure-stack"></a>Azure yığın uygulama hizmeti kaynak Sağlayıcısı Ekle
*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bir Azure yığın bulut operatörü, web ve API uygulamaları oluşturma olanağı, kullanıcılarınızın verebilirsiniz. Bunu yapmak için öncelikle eklemelisiniz [uygulama hizmeti kaynak sağlayıcısı](azure-stack-app-service-overview.md) bu makalede anlatıldığı gibi Azure yığın dağıtımına. Uygulama hizmeti kaynak sağlayıcısı yükledikten sonra teklifleri ve planları içerebilir. Kullanıcılar daha sonra get hizmet ve uygulamalar oluşturmaya başlamak için abone olabilirsiniz.

> [!IMPORTANT]
> Yükleyiciyi çalıştırmadan önce yer alan yönergeleri izlediğinizden emin olun [çalışmaya başlamadan önce](azure-stack-app-service-before-you-get-started.md).
>
>



## <a name="run-the-app-service-resource-provider-installer"></a>Uygulama hizmeti kaynak sağlayıcısı yükleyiciyi çalıştırın

Uygulama hizmeti kaynak sağlayıcısı Azure yığın ortamınıza yükleme saate kadar sürebilir. Bu işlem sırasında yükleyici olur:

* Bir blob kapsayıcısını belirtilen Azure yığın depolama hesabı oluşturun.
* Bir DNS bölgesi ve girişleri için uygulama hizmeti oluşturun.
* Uygulama hizmeti kaynak sağlayıcısı kaydedin.
* Uygulama hizmeti galeri öğeleri kaydedin.

Uygulama hizmeti kaynak sağlayıcısı dağıtmak için aşağıdaki adımları izleyin:

1. Appservice.exe (azurestack\CloudAdmin) yönetici olarak çalıştırın.

2. Tıklatın **Azure yığın bulut üzerinde dağıtmak uygulama hizmeti**.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy/image01.png)

3. Gözden geçirin ve Microsoft Yazılım Lisans Koşulları'nı kabul edin ve ardından **sonraki**.

4. Gözden geçirin ve üçüncü taraf Lisans Koşulları'nı kabul edin ve ardından **sonraki**.

5. Uygulama hizmeti bulut yapılandırma bilgilerinin doğru olduğundan emin olun. Varsayılan ayarları Azure yığın Geliştirme Seti dağıtımı sırasında kullanılan, varsayılan değerleri kabul edebilir. Ancak, Azure yığın dağıtıldığında seçenekleri özelleştirdiyseniz, yansıtmak üzere bu penceresindeki değerleri düzenlemeniz gerekir. Örneğin, etki alanı soneki mycloud.com kullanırsanız, uç noktanız için management.mycloud.com değiştirmeniz gerekir. Bilgilerinizi doğruladıktan sonra tıklatın **sonraki**.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy/image02.png)

6. Sonraki sayfada:
    1. Tıklatın **Bağlan** düğmesine **Azure yığın abonelikleri** kutusu.
        - Azure Active Directory (Azure AD) kullanıyorsanız, Azure AD yönetici hesabı ve Azure yığın dağıtıldığında, verdiğiniz parolayı girin. Tıklatın **oturum**.
        - Active Directory Federasyon Hizmetleri (AD FS) kullanıyorsanız, yönetici hesabı sağlayın. Örneğin, cloudadmin@azurestack.local. Parolanızı girin ve tıklayın **oturum**.
    2. İçinde **Azure yığın abonelikleri** kutusunda, aboneliğinizi seçin.
    3. İçinde **Azure yığın konumu** kutusunda, dağıtımına bölgeyi karşılık gelen konumu seçin. Örneğin, seçin **yerel** varsa Azure yığın Geliştirme Seti dağıtma.
    4. Girin bir **kaynak grubu adı** uygulama hizmeti dağıtımınız için. Varsayılan olarak ayarlanır **APPSERVICE\<bölge\>**.
    5. Girin **depolama hesabı adı** yüklemesinin bir parçası oluşturmak için uygulama hizmeti istiyor. Varsayılan olarak ayarlanır **appsvclocalstor**.
    6. **İleri**’ye tıklayın.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy/image03.png)

7. Dosya Paylaşımı için bilgileri girin ve ardından **sonraki**. Dosya Paylaşımı adresi tam etki alanı adını, dosya sunucunuz örneğin kullanmalıdır \\\appservicefileserver.local.cloudapp.azurestack.external\websites ya da IP adresi, örneğin \\\10.0.0.1\websites.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy/image04.png)

8. Sonraki sayfada:
    1. İçinde **kimlik uygulama kimliği** kutusuna, kimlik (Azure AD) için kullanmakta olduğunuz uygulama için GUID girin.
    2. İçinde **kimlik uygulama sertifika dosyası** kutusuna girin (veya göz atın) sertifika dosyası konumu.
    3. İçinde **kimlik uygulama sertifika parolası** kutusunda, sertifikanın parolasını girin. Sertifikalar oluşturmak üzere komut kullanıldığında Not yapılan bir paroladır.
    4. İçinde **Azure Resource Manager kök sertifika dosyasını** kutusuna girin (veya göz atın) sertifika dosyası konumu.
    5. **İleri**’ye tıklayın.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy/image05.png)

9. Her üç sertifika dosya kutularında, **Gözat** ve uygun sertifika dosyasına gidin. Her sertifika için parola belirtmeniz gerekir. Bu sertifikalar, oluşturduğunuz olanlardır [oluşturma gerekli sertifikaları adım](azure-stack-app-service-deploy.md#create-the-required-certificates). Tıklatın **sonraki** tüm bilgileri girdikten sonra.

    | Box | Sertifika dosyası adı örneği |
    | --- | --- |
    | **Uygulama hizmeti varsayılan SSL sertifika dosyası** | \_.appservice.local.AzureStack.external.pfx |
    | **Uygulama hizmeti API SSL sertifika dosyası** | api.appservice.local.AzureStack.external.pfx |
    | **Uygulama hizmeti yayımcı SSL sertifika dosyası** | ftp.appservice.local.AzureStack.external.pfx |

    Sertifikaları oluşturduğunuzda farklı etki alanı soneki kullandıysanız, sertifika dosya adları kullanmayın *yerel. AzureStack.external*. Bunun yerine, özel etki alanı bilgilerinizi kullanın.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy/image06.png)    

10. Uygulama hizmeti kaynak sağlayıcısı veritabanlarını barındırmak ve ardından için kullanılan sunucu örneği için SQL Server ayrıntılarını girin **sonraki**. Yükleyici SQL bağlantı özelliklerini doğrulama.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy/image07.png)    

11. Rol örneği ve SKU seçenekleri gözden geçirin. Varsayılan örneği ve minimum SKU ASDK dağıtımında her rol için minimum sayısı ile doldurulur. VCPU ve bellek gereksinimlerini özetini dağıtımınızı planlamaya yardımcı olması için sağlanmıştır. Seçimlerinizi yaptıktan sonra tıklatın **sonraki**.

    > [!NOTE]
    > Üretim dağıtımlarında yer alan yönergeleri izleyerek, [kapasite Azure yığınında Azure App Service sunucu rolleri için planlama](azure-stack-app-service-capacity-planning.md).
    >
    >

    | Rol | Minimum örnekleri | Minimum SKU | Notlar |
    | --- | --- | --- | --- |
    | Denetleyici | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Yönetir ve uygulama hizmeti bulut durumunu korur. |
    | Yönetim | 1 | Standard_A2 - (2 Vcpu, 3584 MB) | Uygulama hizmeti Azure Resource Manager ve API uç noktaları, portal Uzantıları (yönetici, Kiracı, işlevleri portalına) ve veri hizmeti yönetir. Yük devretme desteği için 2 için önerilen örnekleri artar. |
    | Yayımcı | 1 | Standard_A1 - (1 vCPU, 1792 MB) | FTP ve web dağıtımı üzerinden içerik yayımlar. |
    | FrontEnd | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Uygulama hizmeti uygulamaları isteklerini yönlendirir. |
    | Paylaşılan çalışan | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Ana web veya API uygulamaları ve Azure işlevleri uygulamalar. Daha fazla örnek eklemek isteyebilirsiniz. Bir operatör olarak teklifinizle tanımlamak ve herhangi bir SKU katmanı seçin. Katman bir vCPU en az olması gerekir. |

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy/image08.png)    

    > [!NOTE]
    > **Windows Server 2016 Core Azure yığında Azure uygulama hizmeti ile kullanılmak üzere desteklenen platform görüntüsü değil**.

12. İçinde **Platform Görüntüsü Seç** kutusunda, uygulama hizmeti bulut bilgi işlem kaynak sağlayıcısındaki kullanılabilir olanlardan dağıtım Windows Server 2016 sanal makine görüntüsü seçin. **İleri**’ye tıklayın.

13. Sonraki sayfada:
     1. Çalışan rolü sanal makine yönetici kullanıcı adını ve parolasını girin.
     2. Diğer roller sanal makine yönetici kullanıcı adını ve parolasını girin.
     3. **İleri**’ye tıklayın.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy/image09.png)    

14. Özet sayfasında:
    1. Yaptığınız seçimleri doğrulayın. Değişiklik yapmak için kullanın **önceki** düğmeleri önceki sayfaları ziyaret edin.
    2. Yapılandırmaları doğruysa, onay kutusunu seçin.
    3. Dağıtımı başlatmak için tıklatın **sonraki**.

    ![Uygulama Hizmeti Yükleyici](media/azure-stack-app-service-deploy/image10.png)    

15. Sonraki sayfada:
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
Web API ve Azure oluşturmak için uygulamaları İşlevler, Kiracı Portalı'nı kullanın ve Kiracı aboneliğinizin olması gerekir.

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
