---
title: Azure'da sürekli tümleştirme (Team Services) ile bir Service Fabric uygulaması dağıtma | Microsoft Docs
description: Bu öğreticide Visual Studio Team Services kullanarak bir Service Fabric uygulaması için nasıl sürekli tümleştirme ve dağıtım ayarlayacağınız gösterilir.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/13/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: f3cc4f518278cca915e40bd691c6a7674219916e
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37109401"
---
# <a name="tutorial-deploy-an-application-with-cicd-to-a-service-fabric-cluster"></a>Öğretici: Service Fabric kümesine CI/CD ile uygulama dağıtma

Bu öğretici bir serinin dördüncü bölümüdür ve Visual Studio Team Services kullanarak bir Azure Service Fabric uygulamasına nasıl sürekli tümleştirme ve dağıtım ayarlayacağınızı açıklar.  Mevcut bir Service Fabric uygulaması gereklidir; örnek olarak [.NET uygulaması oluşturma](service-fabric-tutorial-create-dotnet-app.md) bölümünde oluşturulan uygulama kullanılır.

Serinin üçüncü bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Projenize kaynak denetimi ekleme
> * Team Services’de derleme tanımı oluşturma
> * Team Services’de yayın tanımı oluşturma
> * Uygulamayı otomatik olarak dağıtma ve yükseltme

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [.NET Service Fabric uygulaması oluşturma](service-fabric-tutorial-create-dotnet-app.md)
> * [Uygulamayı uzak kümeye dağıtma](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [Bir ASP.NET Core ön uç hizmetine HTTPS uç noktası ekleme](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
> * Visual Studio Team Services'i kullanarak CI/CD'yi yapılandırma
> * [Uygulama için izleme ve tanılamayı ayarlama](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun
* **Azure geliştirme** ve **ASP.NET ve web geliştirme** iş yükleriyle [Visual Studio 2017’yi yükleyin](https://www.visualstudio.com/).
* [Service Fabric SDK'yı yükleyin](service-fabric-get-started.md)
* Azure’da Windows Service Fabric kümesi oluşturun; örneğin, [bu öğreticiyi izleyin](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
* [Team Services hesabı](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services) oluşturun.

## <a name="download-the-voting-sample-application"></a>Voting örnek uygulamasını indirme

[Bu öğretici serisinin birinci kısmında](service-fabric-tutorial-create-dotnet-app.md) Voting örnek uygulamasını oluşturmadıysanız, indirebilirsiniz. Komut penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın.

```git
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="prepare-a-publish-profile"></a>Yayımlama profili hazırlama

[Uygulamayı oluşturduğunuza](service-fabric-tutorial-create-dotnet-app.md) ve [uygulamayı Azure’a dağıttığınıza](service-fabric-tutorial-deploy-app-to-party-cluster.md) göre, artık sürekli tümleştirme ayarlamaya hazırsınız.  İlk olarak, uygulamanızda Team Services içinde yürütülen dağıtım işleminin kullanacağı yayımlama profilini hazırlayın.  Yayımlama profili, daha önce oluşturduğunuz kümeyi hedefleyecek şekilde yapılandırılmalıdır.  Visual Studio’yu başlatın ve mevcut Service Fabric uygulaması projesini açın.  **Çözüm Gezgini**'nde uygulamaya sağ tıklayın ve **Yayımla...** öğesini seçin.

Sürekli tümleştirme iş akışınızda kullanmak üzere uygulama projenizin içinde bir hedef profil seçin (örneğin Bulut).  Küme bağlantısı uç noktasını belirtin.  Team Services’deki her dağıtımda uygulamanızın yükseltilmesi için **Uygulamayı Yükselt** onay kutusunu işaretleyin.  Ayarları yayımlama profiline kaydetmek için **Kaydet** bağlantısına tıklayın ve ardından **İptal**’e tıklayarak iletişim kutusunu kapatın.

![Gönderim profili][publish-app-profile]

## <a name="share-your-visual-studio-solution-to-a-new-team-services-git-repo"></a>Visual Studio çözümünüzü yeni bir Team Services Git deposunda paylaşma

Derlemeler oluşturabilmek için uygulamanızın kaynak dosyalarını Team Services’deki bir takım projesinde paylaşın.

Visual Studio’nun sağ alt köşesindeki durum çubuğunda **Kaynak Denetimi’ne Ekle** -> **Git**’i seçerek projeniz için yeni bir yerel Git deposu oluşturun.

**Takım Gezgini**’ndeki **Gönderim** görünümünde **Visual Studio Team Services’e Gönder**’in altında yer alan **Git Deposunda Yayımla** düğmesini seçin.

![Git deposunu gönderme][push-git-repo]

E-postanızı doğrulayın ve **Team Services Etki Alanı** açılır listesinde hesabınızı seçin. Deponuzun adını girin ve **Depoyu yayımla**’yı seçin.

![Git deposunu gönderme][publish-code]

Depoyu yayımlamak, hesabınızda yerel depoyla aynı adda yeni bir takım projesi oluşturur. Depoyu mevcut takım projesinde oluşturmak için, **Depo** adının yanındaki **Gelişmiş**’e tıklayın ve bir takım projesi seçin. **Web üzerinde görüntüleyin**’i seçerek kodunuzu web’de görüntüleyebilirsiniz.

## <a name="configure-continuous-delivery-with-vsts"></a>VSTS ile Sürekli Teslimi Yapılandırma

Team Services derleme tanımı, sırayla yürütülen bir dizi derleme adımından oluşturulmuş bir iş akışını açıklar. Service Fabric kümenize dağıtmak üzere Service Fabric uygulama paketini ve diğer yapıtları üreten bir derleme tanımı oluşturun. [Team Services derleme tanımları](https://www.visualstudio.com/docs/build/define/create) hakkında daha fazla bilgi edinin. 

Team Services yayın tanımı, kümeye uygulama paketi dağıtan bir iş akışını açıklar. Derleme tanımı ve yayın tanımı birlikte kullanıldığında kaynak dosyalardan başlayıp kümenizde çalışan bir uygulamada biten iş akışının tamamını yürütür. Team Services [yayın tanımları](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition) hakkında daha fazla bilgi edinin.

### <a name="create-a-build-definition"></a>Derleme tanımı oluşturma

Web tarayıcısını açın ve şu adresteki yeni takım projenize gidin: [https://&lt;myaccount&gt;.visualstudio.com/Voting/Voting%20Team/_git/Voting](https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting).

**Derleme ve Yayın** sekmesini, ardından **Derlemeler**’i ve **+ Yeni tanım**’ı seçin.  **Şablon seç** alanında **Azure Service Fabric Uygulaması** şablonunu seçin ve **Uygula**'ya tıklayın.

![Derleme şablonu seçme][select-build-template]

**Görevler**’de **Aracı kuyruğu** olarak "Hosted VS2017" girin.

![Görevleri seçme][save-and-queue]

**Tetikleyiciler**’in altında **Tetikleyici durumu**’nu ayarlayarak sürekli tümleştirmeyi etkinleştirin.  Derlemeyi el ile başlatmak için **Kaydet ve kuyruğa al**’ı seçin.

![Tetikleyicileri seçme][save-and-queue2]

Derlemeler gönderme veya iade işlemleriyle de tetiklenir. Derlemenizin ilerleme durumunu denetlemek için **Derlemeler** sekmesine geçin.  Derlemenin başarıyla yürütüldüğünü doğruladıktan sonra, uygulamanızı kümeye dağıtan bir yayın tanımı belirleyin.

### <a name="create-a-release-definition"></a>Yayın tanımı oluşturma

**Derleme ve Yayın** sekmesini, ardından **Yayınlar**’ı ve **+ Yeni tanım**’ı seçin.  **Şablon seç** alanında, listeden **Azure Service Fabric Dağıtımı** şablonunu ve sonra da **Uygula**'yı seçin.

![Yayın şablonunu seçme][select-release-template]

Yeni küme bağlantısı eklemek için **Görevler**->**Ortam 1** ve sonra da **+Yeni**'yi seçin.

![Küme bağlantısı ekleme][add-cluster-connection]

**Yeni Service Fabric Bağlantısı ekle** görünümünde **Sertifika Tabanlı** veya **Azure Active Directory** kimlik doğrulamasını seçin.  Bağlantı adı olarak "mysftestcluster" ve küme uç noktası olarak "tcp://mysftestcluster.southcentralus.cloudapp.azure.com:19000" (veya dağıtım yaptığınız kümenin uç noktası) belirtin.

Sertifika tabanlı kimlik doğrulaması için, kümeyi oluştururken kullanılan sunucu sertifikasının **Sunucu sertifikası parmak izi**'ni ekleyin.  **İstemci sertifikası** alanında, istemci sertifika dosyasının base-64 kodlamasını ekleyin. Sertifikanın bu base-64 kodlamalı gösterimini nasıl alacağınızı öğrenmek için bu alanın yardım açılan kutusuna bakın. Ayrıca sertifika için **Parola** ekleyin.  Ayrı bir istemci sertifikanız yoksa, küme veya sunucu sertifikasını kullanabilirsiniz.

Azure Active Directory kimlik bilgileri için, kümeyi oluştururken kullanılan sunucu sertifikasının **Sunucu sertifikası parmak izi**'ni ve ayrıca **Kullanıcı adı** ve **Parola** alanlarına kümeye bağlanırken kullanmak istediğiniz kimlik bilgilerini girin.

**Ekle**'ye tıklayarak küme bağlantısını kaydedin.

Ardından, yayın tanımının derlemeden çıkışı bulabilmesi için işlem hattına bir derleme yapıtı ekleyin. **İşlem Hattı**'nı ve **Yapıtlar**->**+Ekle**'yi seçin.  **Kaynak (Derleme tanımı)** alanında, daha önce oluşturmuş olduğunuz derleme tanımını seçin.  **Ekle**’ye tıklayarak derleme yapıtını kaydedin.

![Yapıt ekleme][add-artifact]

Derleme tamamlandığında otomatik olarak bir yayın oluşturulması için sürekli dağıtım tetikleyicisini etkinleştirin. Yapıttaki şimşek simgesine tıklayın, tetikleyiciyi etkinleştirin ve **Kaydet**'e tıklayarak yayın tanımını kaydedin.

![Tetikleyici etkinleştirme][enable-trigger]

Yayını el ile oluşturmak için **+Yayın** -> **Yayın Oluştur** -> **Oluştur**'u seçin.  Dağıtımın başarılı olduğunu ve uygulamanın kümede çalıştığını doğrulayın.  Bir web tarayıcısı açın ve [http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/) sayfasına gidin.  Uygulama sürümünü not alın (bu örnekte "1.0.0.20170616.3").

## <a name="commit-and-push-changes-trigger-a-release"></a>Değişiklikleri işleme ve gönderme, yayını tetikleme

Team Services'te yapılan bazı kod değişikliklerini denetleyerek sürekli tümleştirme işlem hattının çalıştığını doğrulayın.

Siz kodunuzu yazarken, değişiklikleriniz Visual Studio tarafından otomatik olarak izlenir. Sağ alt kısımdaki durum çubuğunda bekleyen değişiklikler simgesini (![Beklemede][pending]) seçerek değişiklikleri yerel Git deponuza işleyin.

Takım Gezgini'ndeki **Değişiklikler** görünümünde, güncelleştirmenizi açıklayan bir ileti ekleyin ve değişikliklerinizi işleyin.

![Tümünü işleme][changes]

Yayımlanmamış değişiklikler durum çubuğu simgesini (![Yayımlanmamış değişiklikler][unpublished-changes]) veya Takım Gezgini'nde Eşitleme görünümünü seçin. Team Services/TFS'de kodunu güncelleştirmek için **Gönder**'i seçin.

![Değişiklikleri gönderme][push]

Değişikliklerin Team Services'e gönderilmesi otomatik olarak derlemeyi tetikler.  Derleme tanımı başarıyla tamamlandığında, otomatik olarak bir yayın oluşturulur ve kümede uygulamayı yükseltme işlemini başlatır.

Derlemenizin ilerleme durumunu denetlemek için, Visual Studio'nun **Takım Gezgini**'nde **Derlemeler** sekmesine geçin.  Derlemenin başarıyla yürütüldüğünü doğruladıktan sonra, uygulamanızı kümeye dağıtan bir yayın tanımı belirleyin.

Dağıtımın başarılı olduğunu ve uygulamanın kümede çalıştığını doğrulayın.  Bir web tarayıcısı açın ve [http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/) sayfasına gidin.  Uygulama sürümünü not alın (bu örnekte "1.0.0.20170815.3").

![Service Fabric Explorer][sfx1]

## <a name="update-the-application"></a>Uygulamayı güncelleştirme

Uygulamada kod değişikliklerini yapın.  Önceki adımları izleyerek değişiklikleri kaydedin ve işleyin.

Uygulamanın yükseltmesi başladığında, Service Fabric Explorer'da yükseltmenin ilerleme durumunu izleyebilirsiniz:

![Service Fabric Explorer][sfx2]

Uygulama yükseltmesi birkaç dakika sürebilir. Yükseltme tamamlandığında, uygulama bir sonraki sürümde çalışıyor olacaktır.  Bu örnekte "1.0.0.20170815.4".

![Service Fabric Explorer][sfx3]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Projenize kaynak denetimi ekleme
> * Derleme tanımı oluşturma
> * Yayın tanımı oluşturma
> * Uygulamayı otomatik olarak dağıtma ve yükseltme

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Uygulama için izleme ve tanılamayı ayarlama](service-fabric-tutorial-monitoring-aspnet.md)

<!-- Image References -->
[publish-app-profile]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishAppProfile.png
[push-git-repo]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishGitRepo.png
[publish-code]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishCode.png
[select-build-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectBuildTemplate.png
[save-and-queue]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SaveAndQueue.png
[save-and-queue2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SaveAndQueue2.png
[select-release-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectReleaseTemplate.png
[set-continuous-integration]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SetContinuousIntegration.png
[add-cluster-connection]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddClusterConnection.png
[add-artifact]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddArtifact.png
[enable-trigger]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/EnableTrigger.png
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
