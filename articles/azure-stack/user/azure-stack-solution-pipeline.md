---
title: "Azure ve Azure uygulamanızı dağıtmak yığını | Microsoft Docs"
description: "Uygulamaları Azure ve Azure yığını ile karma CI/CD ardışık dağıtmayı öğrenin."
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/25/2017
ms.author: helaw
ms.custom: mvc
ms.openlocfilehash: 83bb401d5d65cd2c34015a1a14673363aeee81d7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-apps-to-azure-and-azure-stack"></a>Azure ve Azure uygulamaları dağıtmak yığını
*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bir karma [sürekli tümleştirme](https://www.visualstudio.com/learn/what-is-continuous-integration/)/[kesintisiz teslim](https://www.visualstudio.com/learn/what-is-continuous-delivery/)(CI/CD) ardışık düzen oluşturma, sınama ve uygulamanızı dağıtmak için birden çok bulut olanak sağlar.  Bu öğreticide, bir karma CI/CD ardışık size nasıl yardımcı olabileceğini öğrenmek için bir örnek ortamı oluşturun:
 
> [!div class="checklist"]
> * Visual Studio Team Services (VSTS) deponuza kod tamamlama dayalı yeni bir yapı başlatır.
> * Kullanıcı kabul testi için otomatik olarak yeni oluşturulan kodunuzu Azure'a dağıtın.
> * Kodunuzu test geçtikten sonra otomatik olarak Azure yığınına dağıtın. 


## <a name="prerequisites"></a>Ön koşullar
Bazı bileşenler bir karma CI/CD ardışık düzen oluşturmak için gerekli ve hazırlamak için biraz zaman alabilir.  Bu bileşenlerden bazıları zaten varsa başlamadan önce gereksinimleri karşıladığınızdan emin olun.

Bu konu ayrıca bazı Azure ve Azure yığın bilgisine sahip olduğunuzu varsayar. Devam etmeden önce daha fazla bilgi edinmek istiyorsanız, bu konularda başlatmak emin olun:

- [Azure giriş](https://docs.microsoft.com/azure/fundamentals-introduction-to-azure)
- [Azure yığın temel kavramları](../azure-stack-key-features.md)

### <a name="azure"></a>Azure
 - Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
 - Oluşturma bir [Web uygulaması](../../app-service/environment/app-service-web-how-to-create-a-web-app-in-an-ase.md)ve için yapılandırma [FTP Yayımlama](../../app-service/app-service-deploy-ftp.md).  Daha sonra kullanılmak üzere yeni Web uygulaması URL'sini not edin.


### <a name="azure-stack"></a>Azure Stack
 - [Azure yığın dağıtmak](../azure-stack-run-powershell-script.md).  Yükleme genellikle tamamlamak için bu nedenle uygun şekilde planlamanız birkaç saat sürer.
 - Dağıtma [uygulama hizmeti](../azure-stack-app-service-deploy.md) Azure yığınına PaaS Hizmetleri.
 - Bir Web uygulaması oluşturma ve için yapılandırma [FTP Yayımlama](../azure-stack-app-service-enable-ftp.md).  Daha sonra kullanılmak üzere yeni Web uygulaması URL'sini not edin.  

### <a name="developer-tools"></a>Geliştirici Araçları
 - Oluşturma bir [VSTS çalışma](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).  Kaydolma işlemi "MyFirstProject." adlı bir proje oluşturur  
 - [Visual Studio 2017 yükleme](https://docs.microsoft.com/visualstudio/install/install-visual-studio) ve [açma VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services#connect-and-share-code-from-visual-studio)
 - Projeye bağlayın ve [yerel olarak kopyalamak](https://www.visualstudio.com/docs/git/gitquickstart).
 - Oluşturma bir [aracı havuzu](https://www.visualstudio.com/docs/build/concepts/agents/pools-queues#creating-agent-pools-and-queues) VSTS içinde.
 - Visual Studio yükleme ve dağıtma bir [VSTS yapı aracısını](https://www.visualstudio.com/docs/build/actions/agents/v2-windows) bir sanal makineye Azure yığında. 
 

## <a name="create-app--push-to-vsts"></a>VSTS itme & Uygulama Oluştur

### <a name="create-application"></a>Uygulama oluşturma
Bu bölümde, basit bir ASP.NET uygulaması oluşturma ve VSTS gönderme.  Bu adımları normal Geliştirici iş akışını temsil eder ve geliştirici araçları ve diller için uyarlanmış. 

1.  Visual Studio'yu açın.
2.  Takım Gezgini alanından ve **çözümleri...**  alanında tıklatın **yeni**.
3.  Seçin **Visual C#** > **Web** > **ASP.NET Web uygulaması (.NET Framework)**.
4.  ' A tıklayın ve uygulama için bir ad **Tamam**.
5.  Sonraki ekranda (Web forms) Varsayılanları tutun ve **Tamam**.

### <a name="commit-and-push-changes-to-vsts"></a>Yürütme ve VSTS değişiklikleri gönderme
1.  Takım Gezgini, Visual Studio kullanarak, açılır seçin ve tıklatın **değişiklikleri**.
2.  Yürütme iletiyi sağlayın ve seçin **Tümünü Yürüt**. İstenebilir çözüm dosyasını kaydetmek için tüm kaydetmek için Evet'i tıklatın.
3.  Tamamlandıktan sonra değişiklikleri projenize eşitlemek Visual Studio sunar. Seçin **eşitleme**.

    ![yürütme tamamlandıktan sonra tamamlama ekran gösteren görüntü](./media/azure-stack-solution-pipeline/image1.png)

4.  Eşitleme sekmesindeki altında *giden*, yeni tamamlama bakın.  Seçin **anında** VSTS değişikliği eşitlenecek.

    ![Görüntü gösteren eşitleme adımları](./media/azure-stack-solution-pipeline/image2.png)

### <a name="review-code-in-vsts"></a>VSTS kod gözden geçirme
Bir kez bir değişiklik kaydedilen ve VSTS için gönderilen VSTS portal kodunuzdan denetleyin.  Seçin **kod**ve ardından **dosyaları** açılan menüden.  Oluşturduğunuz çözüm görebilirsiniz.

## <a name="create-build-definition"></a>Yapı tanımı oluşturun
Derleme işlemi, uygulamanızın nasıl oluşturulur ve kod değişiklikleri her yürütme üzerinde dağıtım için paketlenmiş tanımlar. Bu yapılandırma, uygulamaya bağlı olarak uyarlanmış olabilir ancak Bizim örneğimizde, biz dahil derleme işlemi, bir ASP.NET uygulaması için yapılandırmak için şablonu kullanın.

1.  VSTS alanınıza bir web tarayıcısından oturum açın.
2.  Reklam, seçin **yapı & yayın** ve ardından **derlemeler**.
3.  Tıklatın **+ yeni tanımı**.
4.  Şablonları listesinden seçin **ASP.NET (Önizleme)** seçip **Uygula**.
5.  Değiştirme *MSBuild bağımsız değişkenleri* alanındaki *yapı çözümü* için adım:

    `/p:DeployOnBuild=True /p:WebPublishMethod=FileSystem /p:DeployDefaultTarget=WebPublish /p:publishUrl="$(build.artifactstagingdirectory)\\"`

6.  Seçin **seçenekleri** sekmesini tıklatın ve dağıtılan bir sanal makineye Azure yığında derleme aracısı için aracı sıra seçin. 
7.  Seçin **Tetikleyicileri** sekmesini tıklatın ve etkinleştirme **sürekli tümleştirme**.
7.  Tıklatın **sıraya & Kaydet** ve ardından **kaydetmek** gelen açılır. 

## <a name="create-release-definition"></a>Yayın tanımı oluşturun
Yayın işlem, önceki adımdan derlemeleri bir ortama nasıl dağıtıldığını tanımlar.  Bu öğreticide, size bir Azure Web uygulaması için ASP.NET uygulamamıza FTP ile yayımlayın. Bir yayın Azure yapılandırmak için aşağıdaki adımları kullanın:

1.  VSTS reklam seçin **yapı & yayın** ve ardından **sürümleri**.
2.  Yeşil tıklatın **+ yeni tanımı**.
3.  Seçin **boş** tıklatıp **sonraki**.
4.  Onay kutusunu için *sürekli dağıtım*ve ardından **oluşturma**.

Boş yayın tanımını oluşturdu ve yapı bağlı göre şu adımları Azure ortamı için ekleyin:

1.  Yeşil tıklatın  **+**  görevler eklemek için.
2.  Seçin **tüm**ve ardından listeden ekleyin **FTP Karşıya** seçip **Kapat**.
3.  Seçin **FTP Karşıya** , yeni eklenen görev ve aşağıdaki parametreleri yapılandırın:
    
    | Parametre | Değer |
    | ----- | ----- |
    |Kimlik Doğrulama Yöntemi| Kimlik bilgilerini girin|
    |Sunucu URL'si | Web uygulaması FTP Azure portalından alınan URL'si |
    |Kullanıcı adı | FTP kimlik bilgileri için Web uygulaması oluştururken yapılandırılmış kullanıcı adı |
    |Parola | FTP kimlik bilgileri için Web uygulaması kurarken oluşturduğunuz parola|
    |Kaynak dizini | $(System.DefaultWorkingDirectory)\**\ |
    |Uzak dizin | /Site/wwwroot / |
    |Dosya yolları koru | Etkin (işaretli)|

4.  Tıklatın **Kaydet**

Son olarak, aşağıdaki adımları kullanarak dağıtılmış aracı içeren aracı havuzu kullanmak için yayın tanımı yapılandırın:
1.  Yayın tanımı'nı seçin ve'ı tıklatın **Düzenle**.
2.  Seçin **aracı üzerinde çalıştığı** Orta sütundan.  Sağ sütunda Azure yığın üzerinde çalışan derleme aracısı içeren aracı sıra seçin.  
    ![belirli bir kuyruğa kullanmak için yayın tanımı görüntü gösteren yapılandırması](./media/azure-stack-solution-pipeline/image3.png)


## <a name="deploy-your-app-to-azure"></a>Uygulamanızı Azure'a dağıtma
Bu adım, Azure üzerinde bir Web uygulaması için ASP.NET uygulama dağıtmak için yeni oluşturulan CI/CD hattınızı kullanır. 

1.  Reklam VSTS içinde seçin **yapı & yayın**ve ardından **derlemeler**.
2.  Tıklatın **...**  derleme açıklamasında daha önce oluşturulmuş ve select **sıraya yeni derleme**.
3.  Varsayılanları kabul edin ve tıklayın **Tamam**.  Yapı başlar ve ilerleme durumunu görüntüler.
4.  Yapı tamamlandıktan sonra seçerek durumunu izleyebilir **yapı & yayın** ve seçerek **sürümleri**.
5.  Yapı tamamlandıktan sonra Web uygulaması oluştururken, belirtilen URL'yi kullanarak Web sitesini ziyaret edin.    


## <a name="add-azure-stack-to-pipeline"></a>Boru hattı için Azure yığın ekleme
Azure'a dağıtarak CI/CD hattınızı test ettikten, ardışık düzene Azure yığın ekleme zamanı geldi.  Aşağıdaki adımlarda, yeni bir ortam oluşturmak ve uygulamanızı Azure yığınına dağıtmak için bir FTP Karşıya görev ekleyin.  Ayrıca, "Azure yığın kod sürüme oturum" benzetimini yapmak için bir yol olarak hizmet veren bir yayın onaylayan de ekleyin.  

1.  Yayın tanımı'nda seçin **+ ortama eklemek** ve **oluştur yeni ortam**.
2.  Seçin **boş**, tıklatın **sonraki**.
3.  Seçin **belirli kullanıcıları** ve hesabınızı belirtin.  **Oluştur**’u seçin.
4.  Varolan adını seçip yazarak ortamı yeniden adlandırma *Azure yığın*.
5.  Şimdi, seçim Azure yığın ortamını seçip **görev ekleme**.
6.  Seçin **FTP Karşıya** seçin ve görev **Ekle**seçeneğini belirleyip **Kapat**.


### <a name="configure-ftp-task"></a>FTP görev yapılandırın
Bir yayın oluşturduğunuza göre yayımlama Azure yığında Web uygulaması için gerekli olan adımları yapılandıracaksınız.  Yalnızca Azure için FTP Karşıya görev yapılandırılmış gibi Azure yığını için görev yapılandırın:

1.  Seçin **FTP Karşıya** , yeni eklenen görev ve aşağıdaki parametreleri yapılandırın:
    
    | Parametre | Değer |
    | -----     | ----- |
    |Kimlik Doğrulama Yöntemi| Kimlik bilgilerini girin|
    |Sunucu URL'si | Web uygulaması FTP Azure yığın Portalı'ndan alınan URL'si |
    |Kullanıcı adı | FTP kimlik bilgileri için Web uygulaması oluştururken yapılandırılmış kullanıcı adı |
    |Parola | FTP kimlik bilgileri için Web uygulaması kurarken oluşturduğunuz parola|
    |Kaynak dizini | $(System.DefaultWorkingDirectory)\**\ |
    |Uzak dizin | /Site/wwwroot /|
    |Dosya yolları koru | Etkin (işaretli)|

2.  Tıklatın **Kaydet**

Son olarak, aşağıdaki adımları kullanarak dağıtılmış aracı içeren aracı havuzu kullanmak için yayın tanımı yapılandırın:
1.  Yayın tanımı'nı seçin ve'ı tıklatın **Düzenle**
2.  Seçin **aracı üzerinde çalıştığı** Orta sütundan. Sağ sütunda Azure yığın üzerinde çalışan derleme aracısı içeren aracı sıra seçin.  
    ![belirli bir kuyruğa kullanmak için yayın tanımı görüntü gösteren yapılandırması](./media/azure-stack-solution-pipeline/image3.png)

## <a name="deploy-new-code"></a>Yeni kod dağıtın
Şimdi, Azure yığınına yayımlama son adım ile karma CI/CD ardışık test edebilirsiniz.  Bu bölümde, sitenin altbilgi değiştirin ve ardışık düzeninden dağıtım başlatın.  Tamamlandıktan sonra değişikliklerinizi Azure'a gözden geçirilmek üzere dağıtılan görürsünüz ve yayın onayladıktan sonra Azure yığınına yayımlanır.

1. Visual Studio'da açın *site.master* dosya ve bu satırı değiştirin:
    
    `
        <p>&copy; <%: DateTime.Now.Year %> - My ASP.NET Application</p>
    `

    şu şekilde:

    `
        <p>&copy; <%: DateTime.Now.Year %> - My ASP.NET Application delivered by VSTS, Azure, and Azure Stack</p>
    `
3.  Değişiklikleri ve VSTS eşitleyin.  
4.  VSTS çalışma alanından seçerek yapı durumu kontrol **yapı & yayın** > **derleme**
5.  Devam eden bir derleme görürsünüz.  Durum çift tıklayın ve yapı ilerleme durumunu izleyebilirsiniz.  Sürümünden denetlemek "Tamamlanmış derleme" konsolda görebileceğiniz sonra devam **yapı & yayın** > **yayın**.  Yayın çift tıklayın.
6.  Bir yayın gözden geçirme gerektiren bildirim alırsınız. Web uygulaması URL'si denetleyin ve yeni değişiklikleri mevcut olduğunu doğrulayın.  VSTS sürümde onaylayın.
    ![belirli bir kuyruğa kullanmak için yayın tanımı görüntü gösteren yapılandırması](./media/azure-stack-solution-pipeline/image4.png)
    
7.  Web uygulaması oluştururken, belirtilen URL'yi kullanarak Web sitesini ziyaret ederek Azure yığın yayımlamayı tamamlandığını doğrulayın.
    ![ASP gösteren görüntü. Değiştirilen altbilgi ile nEt uygulama](./media/azure-stack-solution-pipeline/image5.png)


Yeni Karma CI/CD hattınızı bir yapı taşı olarak için diğer karma bulut desenleri artık kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir karma oluşturmak öğrendiniz CI/CD kanal:

> [!div class="checklist"]
> * Başlatır, Visual Studio Team Services (VSTS) deponuza koduna göre yeni bir yapı kaydeder.
> * Kullanıcı kabul testi için otomatik olarak yeni oluşturulan kodunuzu Azure'a dağıtır.
> * Kodunuzu test geçtiğinde Azure yığını tarafından otomatik olarak dağıtır. 

Bir karma CI/CD ardışık sahip olduğunuza göre Azure yığın uygulama geliştirmek nasıl öğrenerek devam edin.

> [!div class="nextstepaction"]
> [Azure Stack için geliştirme](azure-stack-developer.md)


