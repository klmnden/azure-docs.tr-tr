---
title: Azure ile Bulutlar arası ölçeklendirme çözümleri oluşturun | Microsoft Docs
description: Azure ile Bulutlar arası ölçeklendirme çözümleri oluşturmayı öğrenin.
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
ms.date: 09/24/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 68021d25e3deab1c6551d29b0febd9f07e144b57
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50095347"
---
# <a name="tutorial-create-cross-cloud-scaling-solutions-with-azure"></a>Öğretici: Azure ile Bulutlar arası ölçeklendirme çözümleri oluşturma

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bir Azure yığını geçiş yapmak için el ile tetiklenen bir işlem barındırılan web uygulaması, trafik Yöneticisi, esnek ve ölçeklenebilir bulut yardımcı programı, yük altında sağlayarak aracılığıyla otomatik olarak ölçeklendirme ile web uygulaması için bir Azure barındırılan sağlamak için bir Bulutlar arası çözümü oluşturmayı öğrenin.

Bu düzende, kiracınızın genel bulutta uygulamanızı çalıştırmak hazır olmayabilir. Ancak, bu uygulama için talep artışlarını işlemek için kendi şirket içi ortamda gereken kapasiteyi korumak iş için ekonomik uygun olmayabilir. Kiracınızın alabilir, şirket içi çözüm ile genel bulut esnekliğinin kullanın.

Bu öğreticide, bir örnek ortama oluşturacaksınız:

> [!div class="checklist"]
> - Bir çok düğümlü web uygulaması oluşturun.
> - Yapılandırma ve sürekli dağıtım (CD) işlem yönetin.
> - Web uygulamasını Azure Stack'te yayımlayın.
> - Bir yayın oluşturun.
> - Dağıtımlarınızı izlemeye yarayan öğrenin.

> [!Tip]  
> ![karma pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack, Azure'nın bir uzantısıdır. Azure Stack çevikliğini ve yenilik bulut bilgi işlem, şirket içi ortamınıza ve hibrit uygulamaları her yerde oluşturup dağıtmayı olanak tanıyan tek hibrit Bulutu sunar.  
> 
> Teknik incelemeyi [karma uygulamaları için tasarım konuları](https://aka.ms/hybrid-cloud-applications-pillars) (yerleştirme, ölçeklenebilirlik, kullanılabilirlik, dayanıklılık, yönetilebilirlik ve güvenlik) yazılım kalitesinin yapı taşları tasarlama, dağıtma ve çalıştırma için gözden geçirmeleri karma uygulamalar. Tasarım konuları, üretim ortamlarında sorunlarını en aza karma uygulama tasarımının en iyi duruma getirme yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

-   Azure aboneliği. Gerekirse, oluşturun bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) başlamadan önce.

- Azure Stack tümleşik sistemi veya Azure Stack geliştirme Seti'ni dağıtımı.
    - Azure Stack, yüklemek için yönergeler bulun [Azure Stack geliştirme Seti'ni yüklemek](/articles/azure-stack/asdk/asdk-install).
    - [https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1](https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1) Bu yüklemenin tamamlanması birkaç saat gerektirebilir.

-   Dağıtma [App Service](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) PaaS Hizmetleri Azure stack'e.

-   [Planı/Teklifler oluşturma](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) Azure Stack ortamında.

-   [Kiracı aboneliği oluşturmak](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) Azure Stack ortamında.

-   Kiracı abonelik içinde bir Web uygulaması oluşturun. Daha sonra kullanmak için yeni Web App URL'si not edin.

-   Kiracı abonelik içinde VSTS sanal makine dağıtın.

-   Windows Server 2016 VM ile .NET 3.5 gereklidir. Bu VM, özel yapı aracısı olarak Azure Stack'te Kiracı aboneliği içinde oluşturulur.

-   [Windows Server 2016 SQL 2017 VM görüntüsüyle](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-vm-image#add-a-vm-image-through-the-portal) Azure Stack Marketi'nde kullanımınıza sunuluyor. Bu görüntü değilse kullanılabilir, ortama eklendiğinden emin olmak için bir Azure Stack operatörü ile çalışır.

## <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler

### <a name="scalability-considerations"></a>Ölçeklenebilirlik konusunda dikkat edilmesi gerekenler

Önemli bir bileşeni, Bulutlar arası ölçeklendirme arasında ortak anında, isteğe bağlı ölçeklendirme teslim olanağı ve şirket içi bulut altyapısı, tutarlı ve güvenilir bir hizmet talebi tarafından belirtilen şekilde kanıtlama.

### <a name="availability-considerations"></a>Kullanılabilirlik konusunda dikkat edilmesi gerekenler

Yerel olarak dağıtılan uygulamaları, şirket içi donanım yapılandırması ve yazılım dağıtımı aracılığıyla yüksek kullanılabilirlik için yapılandırıldığından emin olun.

### <a name="manageability-considerations"></a>Yönetilebilirlik konusunda dikkat edilmesi gerekenler

Bulutlar arası çözüm sorunsuz yönetimi ve ortamlar arasında tanıdık arabirimi sağlar. PowerShell, platformlar arası yönetimi için önerilir.

## <a name="cross-cloud-scaling"></a>Bulutlar arası ölçeklendirme

### <a name="obtain-a-custom-domain-and-configure-dns"></a>Özel bir etki alanı almak ve DNS yapılandırma

Etki alanı için DNS bölge dosyasını güncelleştirin. Azure AD'ye özel etki alanı adı sahipliğini doğrular. Kullanım [Azure DNS](https://docs.microsoft.com/azure/dns/dns-getstarted-portal) , azure'da Azure/Office 365/dış DNS kayıtları için veya, DNS girişini eklemesini [farklı bir DNS kayıt şirketi](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/).

1.  Özel bir etki alanı, ortak bir kayıt şirketi ile kaydedin.

2.  Etki alanına ilişkin etki alanı adı kayıt şirketinde oturum açın. Onaylanmış yönetici DNS güncelleştirmeleri yapmanız gerekebilir. 

3.  Azure AD tarafından sağlanan DNS girişini ekleyerek etki alanının DNS bölge dosyasını güncelleştirin. (DNS girişini posta yönlendirme veya web barındırma davranışları etkilemez.) 

### <a name="create-a-default-multi-node-web-app-in-azure-stack"></a>Azure Stack'te varsayılan çok düğümlü web uygulaması oluşturma

Azure ve Azure Stack Web uygulamasına dağıtmak için karma sürekli tümleştirme ve sürekli dağıtım (CI/CD) ayarlama ve her iki bulut için otomatik anında iletme değiştirir.

> [!Note]  
> Azure Stack çalıştırma (Windows Server ve SQL) ve App Service dağıtımı için genel olarak uygun görüntülerle gereklidir. App Service belgelerini inceleyin "[Azure Stack'te App Service ile çalışmaya başlamadan önce](/articles/azure-stack/azure-stack-app-service-before-you-get-started)" bölümünde Azure Stack operatörü için.

### <a name="add-code-to-visual-studio-team-services-project"></a>Kod eklemek için Visual Studio Team Services projesi

1. Visual Studio Team Services (VSTS) için VSTS üzerinde proje oluşturma haklarına sahip bir hesapla oturum açın.

    Karma CI/CD, hem uygulama kodunda hem de altyapı kodunu uygulayabilirsiniz. Kullanım [Azure Resource Manager şablonları](https://azure.microsoft.com/resources/templates/) hem özel hem de barındırılan buluta yönelik geliştirme için.

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image1.JPG)

2. **Depoyu kopyalama** oluşturarak ve varsayılan bir web uygulamasını açma.

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image2.png)

### <a name="create-self-contained-web-app-deployment-for-app-services-in-both-clouds"></a>Uygulama hizmetleri için kendi içinde bir web uygulaması dağıtımı her iki bulut oluşturma

1.  Düzen **WebApplication.csproj** dosya. Seçin **Runtimeidentifier** ve ekleme **win10 x64**. (Bkz [Self-contained dağıtım](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) belgelerine.) 

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image3.png)

2.  VSTS Takım Gezgini'ni kullanarak kodu iade edin.

3.  Uygulama kodu Visual Studio Team Services'e işaretli olduğunu doğrulayın.

## <a name="create-the-build-definition"></a>Derleme tanımını oluşturun

1. Yapı tanımları oluşturma olanağı onaylamak için VSTS oturum açın.

2. Ekleme **- r win10-x64** kod. Bu.Net Core ile kendi içinde bir dağıtım tetiklemek gereklidir.

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image4.png)

3. Yapı çalıştırın. [Müstakil dağıtım derleme](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) işlem, Azure ve Azure Stack üzerinde çalışabilen yapıtları yayımlar.

## <a name="use-an-azure-hosted-agent"></a>Azure kullanım barındırılan aracı

Vsts'de barındırılan bir aracı kullanmak, web uygulamaları oluşturmak ve dağıtmak için kullanışlı bir seçenektir. Bakımı ve yükseltmeler, sürekli ve kesintisiz geliştirme, test ve dağıtımını etkinleştirme Microsoft Azure tarafından otomatik olarak gerçekleştirilir.

### <a name="manage-and-configure-the-cd-process"></a>CD işlem yapılandırma ve yönetme

Visual Studio Team Services ve Team Foundation Server (TFS) yüksek oranda yapılandırılabilir ve yönetilebilir bir işlem hattı geliştirme gibi birden çok ortama yayınlar için hazırlama, QA ve üretim ortamlarında; sağlayın onay gerektiren belirli aşamalarda dahil.

## <a name="create-release-definition"></a>Yayın tanımı oluşturma

![Alternatif metin](media\azure-stack-solution-cloud-burst\image5.png)

1.  Seçin **yanı sıra** altında yeni bir yayın eklemek için Ekle düğmesine **sürümler sekmesinde** VSO derleme ve yayın sayfasında.

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image6.png)

2. Azure uygulama hizmeti dağıtımının şablonu uygulayın.

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image7.png)

3. Altında yapıt ekleme, Azure bulut derleme uygulaması için yapıt ekleyin.

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image8.png)

4. İşlem hattı sekmesi altında seçin **aşama, görev** ortamının bağlamak ve Azure bulut ortamına değerlerini ayarlayın.

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image9.png)

5. Ayarlama **ortam adı** Azure seçip **abonelik** Azure bulut uç noktası için.

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image10.png)

6. Ortam adı altında gerekli ayarlamak **Azure uygulama hizmeti adı**.

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image11.png)

7. Girin **Hosted VS2017** barındırılan Azure bulut ortamı için aracı kuyruğu altında.

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image12.png)

8. Azure App Service'e dağıtma geçerli menüde **paket veya klasör** ortam için. Seçin **Tamam** için **klasör konumu**.

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image13.png)

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image14.png)

9. Tüm değişiklikleri kaydetmek ve geri dönüp **yayın ardışık düzeni**.

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image15.png)

10. Azure Stack uygulaması için derleme seçerek yeni bir yapıt ekleyin.

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image16.png)

11. Azure uygulama hizmeti dağıtımının uygulayarak bir daha fazla ortam ekleyin.

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image17.png)

12. Azure Stack yeni ortam adı.

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image18.png)

13. Azure Stack ortamı altında bulmak **görev** sekmesi.

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image19.png)

14. Azure Stack uç noktası için bir abonelik seçin.

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image20.png)

15. Azure Stack web uygulaması adı, uygulama hizmeti adı ayarlayın.

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image21.png)

16. Azure Stack Aracısı'nı seçin.

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image22.png)

17. Azure uygulama hizmeti Dağıt altında bölümü geçerli seçin **paket veya klasör** ortam için. Seçin **Tamam** klasör konumuna.

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image23.png)

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image24.png)

18. Değişken sekmesi altında adlı bir değişken ekleyin `VSTS\_ARM\_REST\_IGNORE\_SSL\_ERRORS`, değer olarak ayarlanmış **true**ve Azure Stack için kapsam.

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image25.png)

19. Seçin **sürekli** yapıtları hem de etkin dağıtım tetikleyicisi simgesi **devam eder** dağıtım tetikleyicisi.

    ![Alternatif metin](media\azure-stack-solution-cloud-burst\image26.png)

20. Seçin **dağıtım öncesi** Azure Stack ortamında koşullar simgesi ve tetikleyici kümesine **sürümünden sonra.**

21. Tüm değişiklikleri kaydedin.

> [!Note]  
> Görevler için bazı ayarları otomatik olarak tanımlanmış olabilir [ortam değişkenlerini](https://docs.microsoft.com/vsts/build-release/concepts/definitions/release/variables?view=vsts#custom-variables) bir şablondan bir yayın tanımı oluşturma sırasında. Bu ayarlar görev ayarlarını değiştirilemez; Bunun yerine, üst ortam öğesi bu ayarları düzenleyebilmeniz için seçilmelidir

## <a name="publish-to-azure-stack-via-visual-studio"></a>Visual Studio aracılığıyla Azure stack'e yayımlama

Visual Studio Online (VSTO) derleme, uç noktaları oluşturarak, Azure Stack için Azure hizmet uygulamaları dağıtabilirsiniz. Azure Stack'e bağlanır yapı aracısı VSTS bağlanır.

1.  VSTO için oturum açın ve uygulama ayarları sayfasına gidin.

2.  Üzerinde **ayarları**seçin **güvenlik**.

3.  İçinde **VSTS grupları**seçin **uç noktasını oluşturanlar**.

4.  Üzerinde **üyeleri** sekmesinde **Ekle**.

5.  İçinde **kullanıcılar ve gruplar ekleme**, bir kullanıcı adı girin ve kullanıcının kullanıcılar listesinden seçin.

6.  Seçin **değişiklikleri kaydetmek**.

7.  İçinde **VSTS grupları** listesinden **uç nokta yöneticileri**.

8.  Üzerinde **üyeleri** sekmesinde **Ekle**.

9.  İçinde **kullanıcılar ve gruplar ekleme**, bir kullanıcı adı girin ve kullanıcının kullanıcılar listesinden seçin.

10. Seçin **değişiklikleri kaydetmek**.

Mevcut uç nokta bilgileri, Azure Stack bağlantı VSTS'ye kullanıma hazırdır. Yapı aracısının Azure Stack'te VSTS'den yönergeler alır ve ardından aracıyı Azure Stack ile iletişim için uç nokta bilgileri iletmez.

## <a name="develop-the-application-build"></a>Uygulama derleme geliştirin

> [!Note]  
> Azure Stack çalıştırma (Windows Server ve SQL) ve App Service dağıtımı için genel olarak uygun görüntülerle gereklidir. App Service belgelerini inceleyin "[Azure Stack'te App Service ile çalışmaya başlamadan önce](/articles/azure-stack/azure-stack-app-service-before-you-get-started)" bölümünde Azure Stack operatörü için.

Kullanım [Azure Resource Manager şablonları gibi web](https://azure.microsoft.com/resources/templates/) hem bulutlara dağıtmak için vsts'den uygulama kodu.

### <a name="add-code-to-a-vsts-project"></a>VSTS projesi için kod ekleyin

1.  VSTS'ye Azure Stack üzerinde proje oluşturma haklarına sahip bir hesapla oturum açın. Sonraki ekran görüntüsü yakalamayı HybridCICD projesine bağlanma işlemi gösterilmektedir.

2.  **Depoyu kopyalama** oluşturarak ve varsayılan bir web uygulamasını açma.

#### <a name="create-self-contained-web-app-deployment-for-app-services-in-both-clouds"></a>Uygulama hizmetleri için kendi içinde bir web uygulaması dağıtımı her iki bulut oluşturma

1.  Düzen **WebApplication.csproj** dosya: seçin **Runtimeidentifier** ve win10 x64 ekleyin. Daha fazla bilgi için [müstakil dağıtım](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) belgeleri.

2.  VSTS kodunu denetlemek için Takım Gezgini'ni kullanın.

3.  Uygulama kodu Visual Studio Team Services'e denetlendi onaylayın.

### <a name="create-the-build-definition"></a>Derleme tanımını oluşturun

1.  VSTS derleme tanımı oluşturabilirsiniz bir hesapla oturum açın.

2.  Gidin **Web uygulaması derleme** proje sayfası.

3.  İçinde **bağımsız değişkenleri**, ekleme **- r win10-x64** kod. .Net Core ile kendi içinde bir dağıtım tetiklemek için bu gereklidir.

4.  Yapı çalıştırın. [Müstakil dağıtım derleme](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) işlem, Azure ve Azure Stack üzerinde çalışabilen yapıtları yayımlar.

#### <a name="use-an-azure-hosted-build-agent"></a>Azure kullanım barındırılan derleme aracısı

VSTS'de barındırılan derleme Aracısı'nı kullanarak, oluşturmak ve web uygulamalarını dağıtmak için kullanışlı bir seçenektir. Aracı Bakımı ve yükseltmeler, sürekli ve kesintisiz geliştirme döngüsü sağlayan Microsoft Azure tarafından otomatik olarak gerçekleştirilir.

### <a name="configure-the-continuous-deployment-cd-process"></a>Sürekli dağıtım (CD) işlem yapılandırma

Visual Studio Team Services (VSTS) ve Team Foundation Server (TFS) yüksek oranda yapılandırılabilir ve yönetilebilir bir işlem hattı geliştirme gibi birden çok ortama yayınlar için hazırlama, kalite güvencesi kapsayan (QA) ve üretim sağlar. Bu işlem, belirli bir uygulama yaşam döngüsü aşamalarında onay gerektiren içerebilir.

#### <a name="create-release-definition"></a>Yayın tanımı oluşturma

Bir yayın tanımı oluşturma, uygulamanın son adımı yapı işlemi bağlıdır. Bu yayın tanımı, bir yayın oluşturun ve bir yapı dağıtmak için kullanılır.

1.  VSTS oturum açın ve gidin **derleme ve yayın** projesi için.

2.  Üzerinde **yayınlar** sekmesinde **[+]** ve ardından çekme **Oluştur yayın tanımı**.

3.  Üzerinde **bir şablon seçin**, seçin **Azure uygulama hizmeti dağıtımının**ve ardından **Uygula**.

4.  Üzerinde **yapıt ekleme**, gelen ** kaynak (derleme tanımı), Azure bulut yapı uygulamayı seçin.

5.  Üzerinde **işlem hattı** sekmesinde **1. Aşama**, **1 görev** bağlantı **ortam görevlerini görüntüle**.

6.  Üzerinde **görevleri** sekmesinde, Azure olarak girin **ortam adı** gelen AzureCloud Traders Web EP seçip **Azure aboneliği** listesi.

7.  Girin **Azure uygulama hizmeti adı**, olduğu `northwindtraders` sonraki ekran görüntüsünde.

8.  Aracı aşaması için seçin **Hosted VS2017** gelen **aracı kuyruğu** listesi.

9.  İçinde **Azure App Service'e dağıtma**, geçerli seçin **paket veya klasör** ortam için.

10. İçinde **seçin dosya veya klasör**seçin **Tamam** için **konumu**.

11. Tüm değişiklikleri kaydetmek ve geri dönüp **işlem hattı**.

12. Üzerinde **işlem hattı** sekmesinde **yapıt ekleme**ve **NorthwindCloud Traders-tekne** gelen ** kaynak (derleme tanımı) ** listesi.

13. Üzerinde **bir şablon seçin**, başka bir ortama ekleyin. Çekme **Azure uygulama hizmeti dağıtımının** seçip **Uygula**.

14. Girin `Azure Stack` olarak **ortam adı**.

15. Üzerinde **görevleri** sekmesinde, bulmak ve Azure Stack seçin.

16. Gelen **Azure aboneliği** listesinden **AzureStack Traders tekne EP** Azure Stack uç noktası için.

17. Azure Stack web uygulaması adı olarak girin **uygulama hizmeti adı**.

18. Altında **Aracısı Seçimi**, çekme **AzureStack -b Douglas alanının** gelen **aracı kuyruğu** listesi.

19. İçin **Azure App Service'e dağıtma**, geçerli seçin **paket veya klasör** ortam için. Üzerinde **dosya veya klasörü seçin**seçin **Tamam** klasör **konumu**.

20. Üzerinde **değişkeni** sekmesinde, bulmak adlı değişken `VSTS\_ARM\_REST\_IGNORE\_SSL\_ERRORS`. Değişken değeri ayarlamak **true**ve kapsamı ayarlayın **Azure Stack**.

21. Üzerinde **işlem hattı** sekmesinde **sürekli dağıtım tetikleyicisi** kümesi ve NorthwindCloud Traders-Web yapıt simgesi **sürekli dağıtım tetikleyicisi** için **Etkin**. Aynı şeyi yapmak **NorthwindCloud Traders-tekne** yapıt.

22. Azure Stack ortamı için seçin **dağıtım öncesi koşulları** simge tetikleyiciyi ayarlayın **sürümünden sonra**.

23. Tüm değişiklikleri kaydedin.

> [!Note]  
> Yayın görevleri için bazı ayarları otomatik olarak tanımlanan [ortam değişkenlerini](https://docs.microsoft.com/vsts/build-release/concepts/definitions/release/variables?view=vsts#custom-variables) bir şablondan bir yayın tanımı oluşturma sırasında. Bu ayarlar görev ayarlarını değiştirilemez, ancak üst ortam öğeler değiştirilebilir.

## <a name="create-a-release"></a>Bir yayın oluşturun

1.  Üzerinde **işlem hattı** sekmesini **yayın** listesindeki **yayın oluştur**.

2.  Yayını için bir açıklama girin, doğru yapıtlar seçili olduğunu görmek için kontrol edin ve ardından **Oluştur**. Birkaç dakika sonra yeni yayın oluşturuldu ve yayın adı bir bağlantı gösterilir belirten bir başlık görüntülenir. Sürüm özeti sayfasında görmek için bağlantıyı seçin.

3.  Sürüm özeti sayfasında sürüm hakkındaki ayrıntıları gösterir. Aşağıdaki ekran görüntüsünde "Release-2" için **ortamları** bölümünde gösterildiği **dağıtım durumu** Azure "Sürüyor" olarak ve Azure Stack için durumu başarılı"için". Azure ortamı için dağıtım durumu "Başarılı" için değiştiğinde, yayın onay için hazır olduğunu belirten bir başlık görüntülenir. Ne zaman bir dağıtımı Beklemede veya başarısız oldu, mavi bir **(i)** bilgi simgesi gösterilir. Gecikme veya hatanın nedenini içeren bir açılır pencere için simgesinin üzerine gelin.

4.  Sürümler listesi gibi diğer görünümleri, ayrıca görünen onay belirten bir simge beklemede. Ortam adı ve dağıtımıyla ilgili daha fazla ayrıntı için bu simge açılır gösterir. Sürümler ve sürümler onay bekliyor bkz: Genel ilerlemesi yönetici görmek için kolay bir işlemdir.

## <a name="monitor-and-track-deployments"></a>İzleme ve izleme dağıtımları

1.  Üzerinde **sürüm 2** Özet sayfasında, **günlükleri**. Bir dağıtım sırasında bu sayfa, Aracıdan dinamik günlüğü gösterir. Sol bölmede, her ortam için dağıtım her işlemin durumunu gösterir.

2.  Bir kişi simgesi seçin **eylem** sütun dağıtım onaylayan (veya reddedildi) bakın ve sağladıkları ileti bir dağıtım öncesi veya dağıtım sonrası onayı.

3.  Dağıtım tamamlandıktan sonra tüm günlük dosyasına sağ bölmede görüntülenir. Seçin **adım** gibi tek bir adım için günlük dosyasını görmek için sol bölmedeki **başlatma işi**. Tek tek günlükleri görme olanağı, izlemek ve genel dağıtım bölümlerinde hata ayıklamak kolaylaştırır. **Kaydet** bir adım için günlük dosyasına veya **tüm günlükleri zip olarak indir**.

4.  Açık **özeti** sürüm hakkındaki genel bilgileri görmek için sekmesinde. Bu görünüm, yapı, için dağıtılan ortamları, dağıtım durumu ve sürüm hakkındaki diğer bilgileri hakkındaki ayrıntıları gösterir.

5.  Bir ortam bağlantıyı seçin (**Azure** veya **Azure Stack**) belirli bir ortama dağıtımları bekleyen ve varolan hakkında bilgi için. Bu görünümler, aynı derlemenin iki ortamlara dağıtılan doğrulamak için hızlı bir yol olarak kullanın.

6.  Açık **üretim uygulamasını dağıtmışsınızdır** tarayıcıda. Örneğin, Azure App Services Web sitesi için URL'yi açın [http://[uygulamanızın-adı\]. azurewebsites.net](http:// [your-app-name].azurewebsites.net).

**Azure ve Azure Stack tümleştirmesini Bulutlar arası ölçeklenebilir bir çözüm sağlar.**

Esnek ve sağlam bir çok bulut hizmeti geri veri güvenliği sağlar yukarı ve yedeklilik, kullanılabilirlik, tutarlı ve hızlı, ölçeklenebilir depolama ve dağıtım ve coğrafi uyumlu yönlendirme. Bu el ile tetiklenen işlem önemli verilerin anlık kullanılabilirliği sağlamaya Web barındırılan uygulamalar arasında geçiş yaparken, güvenilir ve verimli yük sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
- Azure bulut desenleri hakkında daha fazla bilgi için bkz: [bulut tasarımı desenleri](https://docs.microsoft.com/azure/architecture/patterns).