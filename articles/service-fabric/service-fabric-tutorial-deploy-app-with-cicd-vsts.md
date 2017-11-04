---
title: "Bir Azure Service Fabric uygulaması sürekli tümleştirme (Team Services) ile dağıtma | Microsoft Docs"
description: "Sürekli tümleştirme ve Visual Studio Team Services kullanarak bir Service Fabric uygulaması için dağıtım ayarlama öğrenin.  Azure Service Fabric kümede bir uygulamayı dağıtın."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: d0f67b1a63c36e878ed5f7c9aa1c45267e5b156c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-an-application-with-cicd-to-a-service-fabric-cluster"></a>Bir uygulamayı CI/CD ile bir Service Fabric kümesi dağıtma
Bu öğretici üç bir serinin bir parçasıdır ve sürekli tümleştirme ve Visual Studio Team Services kullanarak bir Azure Service Fabric uygulaması için dağıtım nasıl ayarlanacağını açıklar.  Var olan bir Service Fabric uygulaması gereklidir, uygulamayı oluşturduğunuz [bir .NET uygulaması oluşturma](service-fabric-tutorial-create-dotnet-app.md) bir örnek olarak kullanılır.

Bölümünde dizisinin üç bilgi nasıl yapılır:

> [!div class="checklist"]
> * Kaynak denetimi projenize ekleyin
> * Yapı tanımı Team Services içinde oluşturma
> * Team Services içinde bir yayın tanımı oluşturun
> * Otomatik olarak dağıtma ve uygulama yükseltme

Bu öğretici serisinde öğrenin nasıl yapılır:
> [!div class="checklist"]
> * [Bir .NET Service Fabric uygulaması oluşturma](service-fabric-tutorial-create-dotnet-app.md)
> * [Uzak bir küme için uygulama dağıtma](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * CI/CD Visual Studio Team Services kullanarak yapılandırma
> * [İzleme ve tanılama uygulama için ayarlama](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce:
- Bir Azure aboneliğiniz yoksa, oluşturma bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Visual Studio 2017 yükleme](https://www.visualstudio.com/) yükleyip **Azure geliştirme** ve **ASP.NET ve web geliştirme** iş yükleri.
- [Service Fabric SDK yükleme](service-fabric-get-started.md)
- Service Fabric uygulaması, örneğin oluşturun [Bu öğreticiyi izleyerek](service-fabric-tutorial-create-dotnet-app.md). 
- Azure üzerinde bir Windows Service Fabric kümesi tarafından örneğin oluşturma [Bu öğreticiyi izleyerek](service-fabric-tutorial-create-cluster-azure-ps.md)
- Oluşturma bir [Team Services hesabı](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).

## <a name="download-the-voting-sample-application"></a>Oylama örnek uygulamayı indirin
Oylama örnek uygulama yapı içinde değil, [Bu öğretici seri birini Kısım](service-fabric-tutorial-create-dotnet-app.md), yükleyebilirsiniz. Bir komut penceresinde örnek uygulama depoyu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="prepare-a-publish-profile"></a>Bir yayımlama profili hazırlama
Artık [bir uygulama oluşturulan](service-fabric-tutorial-create-dotnet-app.md) ve [uygulamayı Azure'a dağıtılan](service-fabric-tutorial-deploy-app-to-party-cluster.md), sürekli tümleştirme kurup hazırsınız demektir.  İlk olarak, bir yayımlama profili kullanmak için uygulama içinde Team Services içinde yürütür dağıtım işlemi tarafından hazırlayın.  Yayımlama profili, daha önce oluşturduğunuz küme hedeflemek için yapılandırılmalıdır.  Visual Studio'yu başlatın ve var olan bir Service Fabric uygulaması projesini açın.  İçinde **Çözüm Gezgini**, uygulamayı sağ tıklatın ve seçin **Yayımla...** .

Sürekli Tümleştirme iş akışınız için kullanmak için örneğin bulut uygulaması projenize içinden bir hedef profil seçin.  Küme bağlantısı uç noktası belirtin.  Denetleme **uygulama yükseltme** onay kutusunu böylece her dağıtımda Team Services için uygulamanızı yükseltir.  Tıklatın **kaydetmek** yayımlama profili için ayarları kaydedin ve ardından köprü **iptal** iletişim kutusunu kapatmak için.  

![Anında iletme profili][publish-app-profile]

## <a name="share-your-visual-studio-solution-to-a-new-team-services-git-repo"></a>Visual Studio çözümünüz için yeni bir Team Services Git deposuna paylaşma
Derlemeleri oluşturmak için uygulama Kaynak dosyalarınız Team Services takım projesine paylaşır.  

Projeniz için yeni bir yerel Git deposu seçerek oluşturun **eklemek için kaynak denetimi** -> **Git** Visual Studio sağ alt köşesindeki durum çubuğunda. 

İçinde **anında** görünümünde **Takım Gezgini**seçin **yayımlama Git deposuna** altında düğmesini **anında Visual Studio Team Services**.

![Git deposuna Gönder][push-git-repo]

E-postanızı doğrulayın ve hesabınızı seçin **Team Services etki alanı** açılır. Depo adınızı girin ve **Yayımla depo**.

![Git deposuna Gönder][publish-code]

Depodaki yayımlama hesabınızda yerel depoyu aynı ada sahip yeni bir takım projesi oluşturur. Var olan bir takım projesinde deposu oluşturmak için tıklatın **Gelişmiş** yanına **depo** ad ve bir takım projesini seçin. Web üzerinde kodunuzu seçerek görüntüleyebilirsiniz **Web'de bakın**.

## <a name="configure-continuous-delivery-with-vsts"></a>Kesintisiz teslim VSTS ile yapılandırma
Bir Team Services yapı tanımı sırayla yürütülen derleme adımları kümesinden oluşan bir iş akışını açıklar. Yapı tanımı bir Service Fabric uygulama paketini ve bir Service Fabric kümesi dağıtmak için diğer yapıları üreten oluşturabilirsiniz. Daha fazla bilgi edinmek [Team Services yapı tanımları](https://www.visualstudio.com/docs/build/define/create). 

Bir Team Services sürüm tanımı bir küme için bir uygulama paketi dağıtan bir iş akışını açıklar. Birlikte kullanıldığında, kümenizde çalışan bir uygulama ile biten kaynak dosyaları ile başlayan tüm iş akışı derleme tanımı ve yayın tanımının yürütün. Team Services hakkında daha fazla bilgi [yayın tanımları](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).

### <a name="create-a-build-definition"></a>Yapı tanımı oluşturma
Bir web tarayıcısı açın ve yeni takım projenizin gidin: https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting. 

Seçin **yapı & yayın** sekmesinde, ardından **derlemeler**, ardından **+ yeni tanımı**.  İçinde **bir şablon seçin**seçin **Azure Service Fabric uygulaması** şablonu ve tıklatın **Uygula**. 

![Yapı şablonunu seçin][select-build-template] 

.NET Core projesi oylama uygulaması içerir, böylece bağımlılıkları geri yükleyen bir görev ekleyin. İçinde **görevleri** görünümü, select **+ görev eklemek** sayfanın sol. Arama "Komut satırındaki komut satırı görevi bulun ve ardından için" **Ekle**. 

![Görev ekleme][add-task] 

Yeni görev "dotnet.exe Çalıştır" girin **görünen adı**, "dotnet.exe" **aracı**ve "geri" **bağımsız değişkenleri**. 

![Yeni görev][new-task] 

İçinde **Tetikleyicileri** görüntülemek için tıklatın **Bu tetikleyici etkinleştirmek** altında geçiş **sürekli tümleştirme**. 

Seçin **sıraya & Kaydet** ve "Barındırılan VS2017" olarak girin **Aracısı sırası**. Seçin **sıra** bir yapı el ile başlatmak için.  Ayrıca Tetikleyiciler İtme veya iade oluşturur.

Yapı ilerleme durumunu denetlemek için geçin **derlemeler** sekmesi.  Yapı başarılı bir şekilde çalıştığından emin olun sonra uygulamanızı bir kümeye dağıtan bir yayın tanımı tanımlayın. 

### <a name="create-a-release-definition"></a>Bir yayın tanımı oluşturun  

Seçin **yapı & yayın** sekmesinde, ardından **sürümleri**, ardından **+ yeni tanımı**.  İçinde **oluşturma yayın tanımı**seçin **Azure Service Fabric dağıtımı** tıklatın ve listesindeki şablondan **sonraki**.  Seçin **yapı** kaynak, onay **sürekli dağıtım** ve'ı tıklatın **oluşturma**. 

İçinde **ortamları** görüntülemek için tıklatın **Ekle** sağındaki **küme bağlantısı**.  Küme için bir bağlantı adı "mysftestcluster", "tcp://mysftestcluster.westus.cloudapp.azure.com:19000" bir küme uç noktası ve Azure Active Directory veya sertifika kimlik bilgilerini belirtin. Azure Active Directory kimlik bilgileri kümeye bağlanmak için kullanmak istediğiniz kimlik bilgilerini tanımlamak **kullanıcıadı** ve **parola** alanları. Sertifika tabanlı kimlik doğrulaması için istemci sertifika dosyası Base64 kodlaması tanımlamak **istemci sertifikası** alan.  Bu değer alma konusunda bilgi için bu alanı açılır yardımına bakın.  Sertifikanız parola korumalıysa parolayı tanımlayın **parola** alan.  Tıklatın **kaydetmek** yayın tanımını kaydetmek için.

![Küme Bağlantısı Ekle][add-cluster-connection] 

Tıklatın **aracı üzerinde çalıştığı**seçeneğini belirleyip **barındırılan VS2017** için **dağıtım sırası**. Tıklatın **kaydetmek** yayın tanımını kaydetmek için.

![Aracısı'nı çalıştırın][run-on-agent]

Seçin **+ yayın** -> **oluşturma yayın** -> **oluşturma** el ile bir sürüm oluşturmak için.  Dağıtımı başarılı oldu ve uygulama kümede çalışır durumda olduğunu doğrulayın.  Bir web tarayıcısı açın ve gidin [http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).  Bu örnekte "1.0.0.20170616.3" olan uygulama sürümü unutmayın. 

## <a name="commit-and-push-changes-trigger-a-release"></a>Yürütme ve değişiklikleri gönderin, sürüm tetikleme
Sürekli Tümleştirme ardışık düzen Team Services için bazı kod değişiklikleri denetleyerek çalıştığını doğrulamak için.    

Kodunuzu yazarken değişikliklerinizi Visual Studio tarafından otomatik olarak izlenir. Bekleyen değişiklikleri simgesi ('i seçerek yerel Git deponuzu değişiklikleri![Beklemede][pending]) sağ alttaki durum çubuğunda.

Üzerinde **değişiklikleri** Takım Gezgini'nde görüntüleme, güncelleştirmeyi açıklayan bir ileti ekleyin ve değişikliklerinizi uygulamak.

![Tümünü Yürüt][changes]

Yayımdan değişiklikleri durum çubuğu simgesini seçin (![yayımlanmamış değişiklikleri][unpublished-changes]) veya Takım Gezgini'nde eşitleme görünümü. Seçin **anında** kodunuzu Team Services/TFS güncelleştirmek için.

![Değişiklikleri gönderme][push]

Değişiklikleri Team Services için otomatik olarak gönderilmesi bir yapı tetikler.  Yapı tanımı başarıyla tamamlandığında, bir yayın otomatik olarak oluşturulur ve uygulama küme üzerinde yükseltmeyi başlatır.

Yapı ilerleme durumunu denetlemek için geçin **derlemeler** sekmesinde **Takım Gezgini** Visual Studio.  Yapı başarılı bir şekilde çalıştığından emin olun sonra uygulamanızı bir kümeye dağıtan bir yayın tanımı tanımlayın.

Dağıtımı başarılı oldu ve uygulama kümede çalışır durumda olduğunu doğrulayın.  Bir web tarayıcısı açın ve gidin [http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).  Bu örnekte "1.0.0.20170815.3" olan uygulama sürümü unutmayın.

![Service Fabric Explorer][sfx1]

## <a name="update-the-application"></a>Uygulamayı güncelleştirme
Kod değişiklikleri uygulama yapın.  Kaydet ve önceki adımları izleyerek bu değişiklikleri uygulayın.

Uygulama yükseltmesini başladıktan sonra Service Fabric Explorer'da yükseltme ilerleme durumunu izleyebilirsiniz:

![Service Fabric Explorer][sfx2]

Uygulama yükseltme birkaç dakika sürebilir. Yükseltme tamamlandıktan sonra uygulama sonraki sürümünü çalıştıran.  Bu örnekte "1.0.0.20170815.4".

![Service Fabric Explorer][sfx3]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Kaynak denetimi projenize ekleyin
> * Yapı tanımı oluşturma
> * Bir yayın tanımı oluşturun
> * Otomatik olarak dağıtma ve uygulama yükseltme

Sonraki öğretici ilerleyin:
> [!div class="nextstepaction"]
> [İzleme ve tanılama uygulama için ayarlama](service-fabric-tutorial-monitoring-aspnet.md) 


<!-- Image References -->
[publish-app-profile]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishAppProfile.png
[push-git-repo]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishGitRepo.png
[publish-code]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishCode.png
[select-build-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectBuildTemplate.png
[add-task]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddTask.png
[new-task]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewTask.png
[set-continuous-integration]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SetContinuousIntegration.png
[add-cluster-connection]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddClusterConnection.png
[sfx1]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX1.png
[sfx2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX2.png
[sfx3]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX3.png
[pending]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Pending.png
[changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Changes.png
[unpublished-changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/UnpublishedChanges.png
[push]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Push.png
[continuous-delivery-with-VSTS]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/VSTS-Dialog.png
[new-service-endpoint]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpoint.png
[new-service-endpoint-dialog]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpointDialog.png
[run-on-agent]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/RunOnAgent.png