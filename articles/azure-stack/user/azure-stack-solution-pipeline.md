---
title: Azure ve Azure uygulamanızı dağıtmak yığını | Microsoft Docs
description: Uygulamaları Azure ve Azure yığını ile karma CI/CD ardışık dağıtmayı öğrenin.
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
ms.date: 05/15/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 41e6f64ada7c95674cc2573048eef8afc83e4385
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34604361"
---
# <a name="tutorial-deploy-apps-to-azure-and-azure-stack"></a>Öğretici: Azure ve Azure yığın uygulama dağıtma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bir uygulamayı Azure ve Azure kullanılarak bir karma sürekli tümleştirme/sürekli teslim (CI/CD) ardışık düzeni yığını dağıtmayı öğrenin.

Bu öğreticide, bir örnek ortama oluşturursunuz:

> [!div class="checklist"]
> * Visual Studio Team Services (VSTS) deponuza kod tamamlama dayalı yeni bir yapı başlatır.
> * Otomatik olarak kullanıcı kabul testi için genel Azure uygulamanızı dağıtın.
> * Kodunuzu test geçtiğinde, otomatik olarak Azure yığınına uygulamayı dağıtın.

## <a name="benefits-of-the-hybrid-delivery-build-pipe"></a>Karma teslimat yararları kanal oluşturma

Sürekliliği, güvenlik ve güvenilirlik uygulama dağıtımı için anahtar öğeleridir. Bu, kuruluşunuz için önemli ve Geliştirme ekibiniz için kritik önemdedir. Bir karma CI/CD ardışık düzen, şirket içi ortamınız ve genel bulut üzerinden yapı kanallar birleştirmek sağlar. Bir karma teslim modelini de uygulamanız değiştirmeden dağıtım konumlarında değiştirmenize olanak verir.

Karma yaklaşımı kullanmanın diğer avantajları şunlardır:

* Şirket içi Azure yığın ortamınıza ve Azure genel bulutunda tutarlı bir dizi geliştirme araçları koruyabilirsiniz.  Ortak bir araç kümesi CI/CD modelleri ve yöntemleri uygulamak kolaylaştırır.
* Uygulamaları ve Hizmetleri Azure veya Azure yığın dağıtılmış birbirinin yerine kullanılabilir ve aynı kodu her iki konumda da çalıştırabilirsiniz. Şirket içi ve genel bulut özellikleri ve yetenekleri avantajından yararlanabilirsiniz.

CI ve CD hakkında daha fazla bilgi edinmek için:

* [Sürekli Tümleştirme nedir?](https://www.visualstudio.com/learn/what-is-continuous-integration/)
* [Kesintisiz teslim nedir?](https://www.visualstudio.com/learn/what-is-continuous-delivery/)

## <a name="prerequisites"></a>Önkoşullar

Bileşenleri karma CI/CD işlem hattı oluşturmak için bir yerde olması gerekir. Aşağıdaki bileşenler hazırlamak için zaman alır:

* Bir Azure OEM/donanım iş ortağı üretim Azure yığın dağıtabilirsiniz. Tüm kullanıcılar Azure yığın Geliştirme Seti (ASDK) dağıtabilirsiniz.
* Bir Azure yığın işleç de gerekir: uygulama hizmeti dağıtmak, planları ve teklifleri oluşturmak, bir kiracı aboneliği oluşturmak ve Windows Server 2016 görüntü ekleyin.

>[!NOTE]
>Dağıtılan bu bileşenlerin bazıları zaten varsa, Bu öğretici başlamadan önce tüm gereksinimlerini karşıladığından emin olun.

Bu öğretici, Azure ve Azure yığın bazı temel bilgiye sahip olduğunuzu varsayar. Öğretici başlamadan önce daha fazla bilgi için aşağıdaki makaleyi okuyun:

* [Azure giriş](https://azure.microsoft.com/overview/what-is-azure/)
* [Azure yığın temel kavramları](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

### <a name="azure-requirements"></a>Azure gereksinimleri

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
* Oluşturma bir [Web uygulaması](https://docs.microsoft.com/azure/app-service/app-service-web-overview) azure'da. Web uygulama URL'sini Not olun, öğreticide kullanmanız gerekir.

### <a name="azure-stack-requirements"></a>Azure yığın gereksinimleri

* Bir Azure tümleşik yığını sistem kullanın veya Azure yığın Geliştirme Seti (ASDK) dağıtabilirsiniz. ASDK dağıtmak için:
    * [Öğreticisi: Yükleyicisi'ni kullanarak ASDK dağıtmak](https://docs.microsoft.com/azure/azure-stack/asdk/asdk-deploy) ayrıntılı dağıtım yönergeleri sağlar.
    * Kullanım [ConfigASDK.ps1](https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1 ) ASDK dağıtım sonrası adımları otomatikleştirmek için PowerShell komut dosyası.

    > [!Note]
    > ASDK yüklemeyi tamamlamak için bu nedenle uygun şekilde planlamanız yaklaşık yedi saat sürer.

 * Dağıtma [uygulama hizmeti](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) Azure yığınına PaaS Hizmetleri.
 * Oluşturma [planı/teklifleri](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) Azure yığınında.
 * Oluşturma bir [Kiracı aboneliği](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) Azure yığınında.
 * Bir Web uygulaması Kiracı aboneliği oluşturun. Daha sonra kullanmak için yeni Web uygulaması URL'si not edin.
 * VSTS sanal makineyi Kiracı abonelikte dağıtın.
* Windows Server 2016 resmi ile .NET 3.5 bir sanal makine (VM) için belirtin. Bu VM, Azure yığında özel derleme aracısı olarak oluşturulacak.

### <a name="developer-tool-requirements"></a>Geliştirici aracı gereksinimleri

* Oluşturma bir [VSTS çalışma](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services). Kayıt işlemini adlı bir proje oluşturur **MyFirstProject**.
* [Visual Studio 2017 yükleme](https://docs.microsoft.com/visualstudio/install/install-visual-studio) ve [açma VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).
* Projenize bağlanmak ve [yerel olarak kopyalamak](https://www.visualstudio.com/docs/git/gitquickstart).

 > [!Note]
 > Azure yığın ortamınızı Windows Server ve SQL Server'ı çalıştırmak için Dağıtılmış doğru görüntüleri gerekir. Dağıtılan uygulama hizmeti de olması gerekir.

## <a name="prepare-the-private-build-and-release-agent-for-visual-studio-team-services-integration"></a>Özel derleme ve sürüm aracı Visual Studio Team Services tümleştirme için hazırlama

### <a name="prerequisites"></a>Önkoşullar

Visual Studio Team Services (VSTS) bir hizmet sorumlusu kullanarak Azure Resource Manager karşı doğrular. VSTS olmalıdır **katkıda bulunan** Azure yığın abonelik sağlama kaynaklara rol.

Aşağıdaki adımlar, hangi kimlik doğrulamasını yapılandırmak için gerekli olan açıklamaktadır:

1. Bir hizmet sorumlusu oluşturun veya var olan bir hizmet sorumlusunu kullanın.
2. Kimlik doğrulama anahtarları için hizmet sorumlusu oluşturun.
3. Hizmet asıl adı (Katkıda bulunanlar rolünün parçası olarak SPN) izin vermek için rol tabanlı erişim denetimi aracılığıyla Azure yığın aboneliği doğrulayın.
4. Yeni bir hizmet tanımı içinde VSTS Azure yığın uç noktaları ve SPN bilgileri kullanarak oluşturun.

### <a name="create-a-service-principal"></a>Bir hizmet sorumlusu oluşturma

Başvurmak [hizmet sorumlusu oluşturma](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) bir hizmet sorumlusu oluşturmak ve ardından yönergeleri **Web App/API** uygulama türü için.

### <a name="create-an-access-key"></a>Erişim anahtarı oluşturma

Bir hizmet asıl anahtar kimlik doğrulaması için gereklidir. Bir anahtar oluşturmak için aşağıdaki adımları kullanın.

1. Azure Active Directory'deki **Uygulama kayıtları**'nda uygulamanızı seçin.

    ![Uygulamayı seçin](media\azure-stack-solution-hybrid-pipeline\000_01.png)

2. Değerini not edin **uygulama kimliği**. Hizmet uç noktası VSTS yapılandırırken bu değeri kullanır.

    ![Uygulama Kimliği](media\azure-stack-solution-hybrid-pipeline\000_02.png)

3. Kimlik doğrulama anahtarını oluşturmak için **Ayarlar**'ı seçin.

    ![Uygulama ayarlarını düzenleme](media\azure-stack-solution-hybrid-pipeline\000_03.png)

4. Kimlik doğrulama anahtarını oluşturmak için **Anahtarlar**'ı seçin.

    ![Anahtar ayarlarını yapılandırma](media\azure-stack-solution-hybrid-pipeline\000_04.png)

5. Anahtar için bir açıklama sağlayın ve anahtar süresini ayarlayın. İşiniz bittiğinde **Kaydet**’i seçin.

    ![Anahtar açıklaması ve süresi](media\azure-stack-solution-hybrid-pipeline\000_05.png)

    Anahtar, anahtar kaydettikten sonra **değeri** görüntülenir. Bu değer daha sonra alınamıyor çünkü bu değer kopyalayın. Sağladığınız **anahtar değerini** uygulaması olarak oturum açmak için uygulama kimliği. Anahtarı, uygulamanızın alabileceği bir konumda depolayın.

    ![Anahtar değeri](media\azure-stack-solution-hybrid-pipeline\000_06.png)

### <a name="get-the-tenant-id"></a>Kiracı Kimliğinizi alma

Hizmet uç noktası yapılandırmasının bir parçası olarak VSTS gerektirir **Kiracı kimliği** karşılık gelen AAD dizinine Azure yığın damganız dağıtılır. Kiracı kimliği edinmek için aşağıdaki adımları kullanın

1. **Azure Active Directory**'yi seçin.

    ![Azure Active Directory Kiracı için](media\azure-stack-solution-hybrid-pipeline\000_07.png)

2. Kiracı kimliğini almak için Azure AD kiracınızda **Özellikler**'i seçin.

    ![Kiracı özellikleri görüntüle](media\azure-stack-solution-hybrid-pipeline\000_08.png)

3. **Dizin kimliği**'ni kopyalayın. Bu değer kiracı kimliğinizdir.

    ![Dizin Kimliği](media\azure-stack-solution-hybrid-pipeline\000_09.png)

### <a name="grant-the-service-principal-rights-to-deploy-resources-in-the-azure-stack-subscription"></a>Azure yığın aboneliğindeki kaynaklar dağıtmak için hizmet asıl haklar

Aboneliğinizde kaynaklara erişmek için bir rol uygulamaya atamanız gerekir. Uygulama için en iyi izinler rolünü karar verin. Kullanılabilir rolleri hakkında bilgi edinmek için [RBAC: yerleşik roller](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

Abonelik, kaynak grubu ya da kaynak düzeyinde kapsamı ayarlayabilirsiniz. Daha düşük düzeyde kapsam devralınan izinleri. Örneğin, bir kaynak grubu için okuyucu rolüne uygulamaya ekleme kaynak grubunu ve tüm kaynaklarını okuyabilir anlamına gelir.

1. Uygulamaya atamak istediğiniz kapsam düzeyine gidin. Örneğin, abonelik kapsamında bir rol atamak için seçin **abonelikleri**.

    ![Abonelik seç](media\azure-stack-solution-hybrid-pipeline\000_10.png)

2. İçinde **abonelik**, Visual Studio Enterprise seçin.

    ![Visual Studio Enterprise](media\azure-stack-solution-hybrid-pipeline\000_11.png)

3. Visual Studio Enterprise seçin **erişim denetimi (IAM)**.

    ![Erişim denetimi (IAM)](media\azure-stack-solution-hybrid-pipeline\000_12.png)

4. **Add (Ekle)** seçeneğini belirleyin.

    ![Ekle](media\azure-stack-solution-hybrid-pipeline\000_13.png)

5. İçinde **izinleri eklemek**, rolü seçin, uygulamaya atamak istediğiniz. Bu örnekte, **sahibi** rol.

    ![Sahip rolü](media\azure-stack-solution-hybrid-pipeline\000_14.png)

6. Varsayılan olarak, Azure Active Directory uygulamaları kullanılabilir seçenekleri görüntülenmiyor. Uygulamanızı bulmak için adını sağlamalısınız **seçin** aramak için alan. Uygulamayı seçin.

    ![Uygulama arama sonucu](media\azure-stack-solution-hybrid-pipeline\000_16.png)

7. Seçin **kaydetmek** rol atama tamamlamak için. Listenin uygulamanızda bu kapsam için bir rolüne atanan kullanıcıların bakın.

### <a name="role-based-access-control"></a>Rol Tabanlı Access Control

Azure rol tabanlı erişim denetimi (RBAC) Azure için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, kullanıcıların işlerini yapmak için gereken erişim düzeyini denetleyebilirsiniz. Rol tabanlı erişim denetimi hakkında daha fazla bilgi için bkz: [Azure abonelik kaynaklarına erişimi yönetme](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal?toc=%252fazure%252factive-directory%252ftoc.json).

### <a name="vsts-agent-pools"></a>VSTS aracısı havuzları

Her bir aracının ayrı ayrı yönetmek yerine, aracıları aracı havuzu halinde düzenleyebilirsiniz. Bir aracı havuzu paylaşım sınır havuzda tüm aracılar için tanımlar. VSTS içinde takım projeleri arası bir aracı havuzu paylaşabileceği anlamına gelir VSTS hesabına aracısı havuzları kapsamlıdır. Aracı havuzları hakkında daha fazla bilgi için bkz: [aracısı havuzları oluşturmak ve kuyruklar](https://docs.microsoft.com/vsts/build-release/concepts/agents/pools-queues?view=vsts).

### <a name="add-a-personal-access-token-pat-for-azure-stack"></a>Kişisel erişim belirteci (PAT) için Azure yığın ekleme

Bir kişisel erişim VSTS erişmek için belirteci oluşturun.

1. VSTS hesabınızda oturum açın ve hesabınızın profil adını seçin.
2. Seçin **Güvenliği Yönet** erişim belirteci oluşturma sayfası.

    ![Kullanıcı oturum açma](media\azure-stack-solution-hybrid-pipeline\000_17.png)

    ![Ekip projesi seçin](media\azure-stack-solution-hybrid-pipeline\000_18.png)

    ![Kişisel erişim belirteci Ekle](media\azure-stack-solution-hybrid-pipeline\000_18a.png)

    ![Belirteç oluştur](media\azure-stack-solution-hybrid-pipeline\000_18b.png)

3. Belirteç kopyalayın.

    > [!Note]
    > Belirteç bilgileri kaydedin. Bu bilgiler saklanmaz ve web sayfasını çıktığınızda yeniden gösterilmeyecektir.

    ![Kişisel erişim belirteci](media\azure-stack-solution-hybrid-pipeline\000_19.png)

### <a name="install-the-vsts-build-agent-on-the-azure-stack-hosted-build-server"></a>Azure yığında VSTS yapı aracısını yükleme yapı sunucu barındırılan

1. Yapı Azure yığın konakta dağıtılan sunucunuza bağlanın.
2. Karşıdan yükleme ve dağıtma derleme aracısı kişisel kullanarak bir hizmet olarak erişim belirteci (PAT) ve VM yönetici farklı çalıştır hesabı.

    ![Derleme Aracısı'nı indirme](media\azure-stack-solution-hybrid-pipeline\010_downloadagent.png)

3. Ayıklanan yapı aracısının klasöre gidin. Çalıştırma **run.cmd** yükseltilmiş bir komut istemi dosyasından.

    ![Ayıklanan derleme aracısı](media\azure-stack-solution-hybrid-pipeline\000_20.png)

    ![Derleme aracısı kaydedin](media\azure-stack-solution-hybrid-pipeline\000_21.png)

4. Run.cmd sona erdiğinde, yapı aracısı klasörüne ek dosyalarla güncelleştirilir. Ayıklanan içeriğin bulunduğu klasöre aşağıdaki gibi görünmelidir:

    ![Yapı klasörü Güncelleştirme Aracısı](media\azure-stack-solution-hybrid-pipeline\009_token_file.png)

    VSTS klasöründeki aracı görebilirsiniz.

## <a name="endpoint-creation-permissions"></a>Uç nokta oluşturma izinleri

Visual Studio Online (VSTO) yapı uç noktaları oluşturarak Azure yığınına Azure hizmet uygulamaları dağıtabilirsiniz. VSTS Azure yığınına bağlayan derleme aracısı bağlanır.

![VSTO NorthwindCloud örnek uygulama](media\azure-stack-solution-hybrid-pipeline\012_securityendpoints.png)

1. VSTO oturum açın ve uygulama ayarları sayfasına gidin.
2. Üzerinde **ayarları**seçin **güvenlik**.
3. İçinde **VSTS grupları**seçin **Endpoint oluşturucuları**.

    ![NorthwindCloud Endpoint oluşturucuları](media\azure-stack-solution-hybrid-pipeline\013_endpoint_creators.png)

4. Üzerinde **üyeleri** sekmesine **Ekle**.

    ![Üye ekleme](media\azure-stack-solution-hybrid-pipeline\014_members_tab.png)

5. İçinde **kullanıcılar ve gruplar ekleme**, bir kullanıcı adı girin ve kullanıcının kullanıcılar listesinden seçin.
6. Seçin **değişiklikleri kaydetmek**.
7. İçinde **VSTS grupları** listesinde **Endpoint Yöneticiler**.

    ![NorthwindCloud Endpoint yöneticileri](media\azure-stack-solution-hybrid-pipeline\015_save_endpoint.png)

8. Üzerinde **üyeleri** sekmesine **Ekle**.
9. İçinde **kullanıcılar ve gruplar ekleme**, bir kullanıcı adı girin ve kullanıcının kullanıcılar listesinden seçin.
10. Seçin **değişiklikleri kaydetmek**.

Uç nokta bilgileri var, Azure yığın bağlantı VSTS kullanılmaya hazırdır. Derleme aracısı Azure yığınında VSTS yönergeleri alır ve ardından aracıyı Azure yığını ile iletişim için uç nokta bilgileri iletir.

![Yapı Aracısı](media\azure-stack-solution-hybrid-pipeline\016_save_changes.png)

## <a name="develop-your-application-build"></a>Uygulama yapınızın geliştirin

Öğreticinin bu bölümünde, gerekir:

* Kod VSTS projeye ekleyin.
* Kendi içinde bulunan web uygulama dağıtımı oluşturun.
* Sürekli dağıtım işlemi yapılandırma

> [!Note]
 > Azure yığın ortamınızı Windows Server ve SQL Server'ı çalıştırmak için Dağıtılmış doğru görüntüleri gerekir. Dağıtılan uygulama hizmeti de olması gerekir. Azure yığın işleci gereksinimleri için uygulama hizmeti belgelerini "Önkoşullar" bölümü gözden geçirin.

Karma CI/CD hem uygulama kodunda hem de altyapı kodu uygulayabilirsiniz. Kullanım [web gibi Azure Resource Manager şablonları ](https://azure.microsoft.com/resources/templates/) Bulutlar hem de dağıtmak için VSTS uygulama kodundan.

### <a name="add-code-to-a-vsts-project"></a>VSTS projeye kod ekleme

1. VSTS Azure yığında proje oluşturma haklarına sahip bir hesapla oturum açın. Sonraki ekran yakalama HybridCICD projesine bağlanmak nasıl gösterir.

    ![Bir projeye bağlayın](media\azure-stack-solution-hybrid-pipeline\017_connect_to_project.png)

2. **Depoyu kopyalayın** oluşturarak ve varsayılan web uygulaması açma.

    ![Kopya deposu](media\azure-stack-solution-hybrid-pipeline\018_link_arm.png)

### <a name="create-self-contained-web-app-deployment-for-app-services-in-both-clouds"></a>Bulutlar hem de uygulama hizmetleri için kendi içinde bulunan web uygulama dağıtımı oluşturma

1. Düzenleme **WebApplication.csproj** dosya: seçin **Runtimeidentifier** ve ardından ekleyin `win10-x64.` daha fazla bilgi için bkz: [müstakil dağıtım](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) belgeler.

    ![Runtimeidentifier yapılandırın](media\azure-stack-solution-hybrid-pipeline\019_runtimeidentifer.png)

2. VSTS kod denetlemek için Takım Gezgini kullanın.

3. Uygulama kodu Visual Studio Team Services denetlendi onaylayın.

### <a name="create-the-build-definition"></a>Yapı tanımı oluşturun

1. VSTS yapı tanımı oluşturabilirsiniz bir hesap ile oturum açın.
2. Gidin **Web uygulaması derleme** projesi için sayfa.

3. İçinde **bağımsız değişkenleri**, ekleme **- r win10-x64** kodu. .Net ile kendi içinde bulunan bir dağıtımı tetiklemek için bu gereklidir çekirdek.

    ![Bağımsız değişken yapı tanımına ekleyin](media\azure-stack-solution-hybrid-pipeline\020_publish_additions.png)

4. Yapı çalıştırın. [Müstakil dağıtım yapı](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) işlemi, Azure ve Azure yığın çalışabilir yapıları yayımlar.

### <a name="use-an-azure-hosted-build-agent"></a>Derleme aracısı kullanım Azure barındırılan

VSTS içinde barındırılan yapı aracısını kullanarak oluşturmak ve web uygulamalarını dağıtmak için kullanışlı bir seçenektir. Aracı Bakımı ve yükseltmeler, sürekli ve kesintisiz geliştirme döngüsü sağlayan Microsoft Azure tarafından otomatik olarak gerçekleştirilir.

### <a name="configure-the-continuous-deployment-cd-process"></a>Sürekli dağıtım (CD) işlemi yapılandırma

Visual Studio Team Services (VSTS) ve Team Foundation Server (TFS) yüksek oranda yapılandırılabilir ve yönetilebilir bir ardışık düzen geliştirme gibi birden çok ortamlara sürümleri için hazırlama, kalite güvence (QA) ve üretim sağlar. Bu işlem, uygulama yaşam döngüsü belirli aşamalarında onayları gerektiren içerebilir.

### <a name="create-release-definition"></a>Yayın tanımı oluşturun

Bir yayın tanımı oluşturma son adım, uygulamanızda yapı işlemi bağlıdır. Bu sürüm tanımı, bir yayın oluşturmak ve bir yapı dağıtmak için kullanılır.

1. VSTS oturum açın ve gidin **derleme ve sürüm** projeniz için.
2. Üzerinde **sürümleri** sekmesine  **\[ +]** ve ardından çekme **oluşturma yayın tanımı**.

   ![Yayın tanımı oluşturun](media\azure-stack-solution-hybrid-pipeline\021a_releasedef.png)

3. Üzerinde **bir şablon seçin**, seçin **Azure uygulama hizmeti dağıtımı**ve ardından **Uygula**.

    ![Şablonu uygula](media\azure-stack-solution-hybrid-pipeline\102.png)

4. Üzerinde **ekleyin yapıt**, gelen **kaynak (derleme tanımı)** aşağı açılır menüsünde, Azure bulut yapı uygulamayı seçin.

    ![Yapıt ekleme](media\azure-stack-solution-hybrid-pipeline\103.png)

5. Üzerinde **ardışık düzen** sekmesine **1 aşaması**, **1 görev** bağlantı **görüntülemek ortam görevleri**.

    ![Ardışık Düzen görünümü görevleri](media\azure-stack-solution-hybrid-pipeline\104.png)

6. Üzerinde **görevleri** sekmesinde, Azure olarak girin **ortam adı** gelen AzureCloud Traders Web EP seçip **Azure aboneliği** aşağı açılan liste.

    ![Ortam değişkenlerini belirleme](media\azure-stack-solution-hybrid-pipeline\105.png)

7. Girin **Azure uygulama hizmeti adını**, sonraki ekran görüntüsünde "northwindtraders" olduğu.

    ![Uygulama hizmeti adı](media\azure-stack-solution-hybrid-pipeline\106.png)

8. Aracı aşaması için seçin **barındırılan VS2017** gelen **Aracısı sırası** aşağı açılan liste.

    ![Barındırılan Aracısı](media\azure-stack-solution-hybrid-pipeline\107.png)

9. İçinde **Azure uygulama hizmeti Dağıt**, geçerli seçin **paket veya klasör** ortamı için.

    ![Paket veya klasörü seçin](media\azure-stack-solution-hybrid-pipeline\108.png)

10. İçinde **seçin dosya veya klasör**seçin **Tamam** için **konumu**.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\109.png)

11. Tüm değişiklikleri kaydetmek ve geri dönüp **ardışık düzen**.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\110.png)

12. Üzerinde **ardışık düzen** sekmesine **ekleyin yapıt**ve seçin **NorthwindCloud Traders-tekne** gelen **kaynak (yapı tanımı)** aşağı açılan liste.

    ![Yeni Yapı Ekle](media\azure-stack-solution-hybrid-pipeline\111.png)

13. Üzerinde **bir şablon seçin**, başka bir ortama ekleyin. Çekme **Azure uygulama hizmeti dağıtımı** ve ardından **Uygula**.

    ![Şablonu seçin](media\azure-stack-solution-hybrid-pipeline\112.png)

14. "Azure yığın" olarak girin **ortam adı**.

    ![Ortam adı](media\azure-stack-solution-hybrid-pipeline\113.png)

15. Üzerinde **görevleri** sekmesinde, bulmak ve Azure yığını seçin.

    ![Azure yığın ortamı](media\azure-stack-solution-hybrid-pipeline\114.png)

16. Gelen **Azure aboneliği** aşağı açılan listesinde, "AzureStack Traders tekne EP" Azure yığın uç noktası için seçin.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\115.png)

17. Azure yığın web uygulaması adı olarak girin **uygulama hizmet adı**.

    ![Uygulama hizmeti adı](media\azure-stack-solution-hybrid-pipeline\116.png)

18. Altında **Aracısı Seçimi**, "AzureStack - bDouglas Fir" seçim **Aracısı sırası** aşağı açılan liste.

    ![Aracı seçin](media\azure-stack-solution-hybrid-pipeline\117.png)

19. İçin **Azure uygulama hizmeti Dağıt**, geçerli seçin **paket veya klasör** ortamı için. Üzerinde **dosya veya klasör seç**seçin **Tamam** klasörü için **konumu**.

    ![Paket ya da klasörü seçin](media\azure-stack-solution-hybrid-pipeline\118.png)

    ![Konum Onayla](media\azure-stack-solution-hybrid-pipeline\119.png)

20. Üzerinde **değişkeni** sekmesinde, adlı değişken Bul **VSTS_ARM_REST_IGNORE_SSL_ERRORS**. Değişken değeri ayarlamak **true**, kapsamı ayarlanmış ve **Azure yığın**.

    ![Değişken yapılandırın](media\azure-stack-solution-hybrid-pipeline\120.png)

21. Üzerinde **ardışık düzen** sekmesine **sürekli dağıtım tetikleyici** NorthwindCloud Traders-Web yapı ve kümesi simgesi **sürekli dağıtım tetikleyici** için **Etkin**.  "NorthwindCloud Traders tekne" yapı için aynı işlevi görür.

    ![Küme sürekli dağıtım tetikleyici](media\azure-stack-solution-hybrid-pipeline\121.png)

22. Azure yığın ortamı için seçin **dağıtım öncesi koşullar** simge kümesine tetikleyici **sürümünden sonra**.

    ![Küme dağıtım öncesi koşullar tetikleyici](media\azure-stack-solution-hybrid-pipeline\122.png)

23. Yaptığınız tüm değişiklikleri kaydedin.

> [!Note]
> Bırakma görevleri için bazı ayarları otomatik olarak tanımlanmış olabilir [ortam değişkenleri](https://docs.microsoft.com/vsts/build-release/concepts/definitions/release/variables?view=vsts#custom-variables) bir şablondan bir yayın tanımı oluşturduğunuzda. Bu ayarlar görev ayarlarında değiştirilemez. Ancak, bu ayarları üst ortam öğeleri düzenleyebilirsiniz.

## <a name="create-a-release"></a>Sürüm oluşturma

Yayın tanımı için yapılan değişiklikleri tamamladığınıza göre dağıtım başlangıç zamanı geldi. Bunu yapmak için yayın tanımı sürüm oluşturun. Bir yayın otomatik olarak oluşturulabilir; Örneğin, sürekli dağıtım tetikleyici yayın tanımı'nda ayarlanır. Bu kaynak kodun değiştirilmesiyle yeni bir yapı başlar anlamına gelir ve bundan, yeni bir sürüm. Ancak, bu bölümde, yeni bir sürüm el ile oluşturacaksınız.

1. Üzerinde **ardışık düzen** sekmesi, açık **yayın** aşağı açılan listesinde ve seçin **sürüm oluşturma**.

    ![Sürüm oluşturma](media\azure-stack-solution-hybrid-pipeline\200.png)

2. Yayın için bir açıklama girin, doğru yapıları seçildiğini denetleyin ve ardından **oluşturma**. Birkaç dakika sonra yeni sürüm oluşturulduğunu ve yayın adı bir bağlantı görüntülenir belirten bir başlık görüntülenir. Yayın Özet sayfasını görmek için bağlantıya seçin.

    ![Sürüm oluşturma başlığı](media\azure-stack-solution-hybrid-pipeline\201.png)

3. Yayın Özet sayfası sürümü hakkında ayrıntıları gösterir. "Yayın-2" için aşağıdaki ekran görüntüsünde **ortamları** bölümünde gösterir **dağıtım durumu** "Sürüyor" ve Azure yığın durumu olarak Azure "başarılı için". Azure ortamı için dağıtım durumu "Başarılı" değiştiğinde, yayın onay için hazır olduğunu belirten bir başlık görüntülenir. Ne zaman bir dağıtımı Beklemede veya başarısız oldu, mavi **(i)** bilgi simgesi gösterilir. Gecikme veya hatanın nedenini içeren bir açılır pencere görmek için simgenin üzerine getirin.

    ![Sürüm Özet sayfası](media\azure-stack-solution-hybrid-pipeline\202.png)

Listesi gibi diğer görünümlere serbest, ayrıca onay bekliyor belirten bir simge görüntülenir. Bu simgenin açılır ortam adı ve dağıtımıyla ilgili daha fazla ayrıntı gösterir. Bir yönetici görmek için sürümleri ve sürümler onay bekliyor bkz. genel ilerlemesi kolay bir işlemdir.

### <a name="monitor-and-track-deployments"></a>İzleyicisi ve izleme dağıtımları

Bu bölümde, nasıl görüntülemek ve tüm dağıtımlar izlemek gösterir. İki Azure App Services Web sitelerini dağıtmak için yayın iyi bir örnek sağlar.

1. "Yayın-2" Özet sayfasında, seçin **günlükleri**. Bir dağıtımı sırasında bu sayfayı Aracısı'ndan Canlı günlüğünü gösterir. Sol bölmede her ortam için dağıtımdaki her bir işlemin durumunu gösterir.

    Bir kişinin simgesi seçebilirsiniz **eylem** dağıtım onaylayan (veya reddeden) bakın ve sağladıkları ileti bir dağıtım öncesi veya dağıtım sonrası onayı sütun.

2. Dağıtım tamamlandıktan sonra tüm günlük dosyasını sağ bölmede görüntülenir. Seçebilirsiniz **adım** sol bölmesinde "İşi başlatılamıyor" gibi tek bir adım için günlük dosyasına bakın. Tek tek günlükleri görme olanağı, izleme ve genel dağıtım bölümlerini hata ayıklamak kolaylaştırır. Ayrıca **kaydetmek** bir adım için günlük dosyasına veya **zip tüm günlükleri indirmek**.

    ![Yayın günlükleri](media\azure-stack-solution-hybrid-pipeline\203.png)

3. Açık **Özet** sürümü hakkında genel bilgi görmek için sekmesini. Bu görünüm, yapı, için dağıtılan ortamları, dağıtım durumu ve sürümü hakkında diğer bilgi ayrıntılarını gösterir.

4. Bir ortam bağlantısını seçin (**Azure** veya **Azure yığın**) belirli bir ortam için dağıtımları bekleyen ve varolan hakkında bilgi için. Bu görünümler, aynı yapı için her iki ortama dağıtılan doğrulamak için hızlı bir yolu olarak kullanabilirsiniz.

5. Açık **üretim uygulama** tarayıcınızda. Örneğin, Azure App Services Web sitesi için URL'yi açın `http://[your-app-name].azurewebsites.net`.

## <a name="next-steps"></a>Sonraki adımlar

* Azure bulut desenleri hakkında daha fazla bilgi için bkz: [bulut tasarım modeli](https://docs.microsoft.com/azure/architecture/patterns).
