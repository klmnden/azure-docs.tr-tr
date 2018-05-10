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
ms.date: 05/07/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 49a80805c976e5584bb158965583a03eda68cc46
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="tutorial-deploy-apps-to-azure-and-azure-stack"></a>Öğretici: Azure ve Azure uygulamaları dağıtmak yığını

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bir karma sürekli tümleştirme/sürekli teslim (CI/CD) ardışık düzen oluşturma, sınama ve uygulamanızı dağıtmak için birden çok bulut sağlar.  Bu öğreticide, bir örnek ortamı için yapı:
 
> [!div class="checklist"]
> * Visual Studio Team Services (VSTS) deponuza kod tamamlama dayalı yeni bir yapı başlatır.
> * Otomatik olarak yeni oluşturulan kodunuzu kullanıcı kabul testi için genel Azure dağıtın.
> * Kodunuzu test geçtikten sonra otomatik olarak Azure yığınına dağıtın.

### <a name="about-the-hybrid-delivery-build-pipe"></a>Karma teslimat hakkında kanal oluşturma

Uygulama dağıtım sürekliliği, güvenlik ve güvenilirlik, kuruluşunuz için önemli ve Geliştirme ekibiniz için kritik. Karma CI/CD ardışık düzen ile şirket içi ortamınız ve genel bulut üzerinden hatlarınızı birleştirebilir. Uygulamanızı geçmeden konumunu değiştirebilirsiniz.

Bu yaklaşım geliştirme araçları tutarlı bir dizi korumanıza olanak sağlar. Azure genel Bulut ve şirket içi Azure yığın ortamınız genelinde tutarlı araçları CI/CD geliştirme uygulama uygulamak için ne kadar kolay olduğunu gösterir. Uygulamaları ve Hizmetleri Azure veya Azure yığın dağıtılmış birbirinin yerine kullanılabilir ve aynı kodu her iki konumda şirket içi, genel bulut özellikleri ve yetenekleri yararlanarak çalıştırabilirsiniz.

Daha fazla bilgi edinin:
 - [Sürekli Tümleştirme nedir?](https://www.visualstudio.com/learn/what-is-continuous-integration/)
 - [Kesintisiz teslim nedir?](https://www.visualstudio.com/learn/what-is-continuous-delivery/)


## <a name="prerequisites"></a>Önkoşullar

Karma CI/CD işlem hattı oluşturmak için birkaç bileşenleri sahip olmak gerekir. Bunlar hazırlamak için biraz zaman alabilir.
 
 - Bir Azure OEM/donanım iş ortağı üretim Azure yığın dağıtabileceğiniz ve tüm kullanıcıların bir Azure yığın Geliştirme Seti (ASDK) dağıtabilirsiniz. 
 - Bir Azure yığın işleç Ayrıca uygulama hizmeti dağıtmak, planları ve teklifleri oluşturmak, bir kiracı aboneliği oluşturmak ve Windows Server 2016 görüntü eklemeniz gerekir.

Bu bileşenlerden bazıları zaten varsa başlamadan önce gereksinimleri karşıladığınızdan emin olun.

Bu konu ayrıca bazı Azure ve Azure yığın bilgisine sahip olduğunuzu varsayar. Devam etmeden önce daha fazla bilgi edinmek istiyorsanız, bu konularda başlatmak emin olun:


Bu öğretici, ayrıca bazı Azure ve Azure yığın bilgisine sahip olduğunuzu varsayar. 

Devam etmeden önce daha fazla bilgi edinmek istiyorsanız, bu konularda başlatabilirsiniz:
 - [Azure giriş](https://azure.microsoft.com/overview/what-is-azure/)
 - [Azure yığın temel kavramları](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

### <a name="azure"></a>Azure

 - Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

 - Oluşturma bir [Web uygulaması](https://docs.microsoft.com/azure/app-service/app-service-web-overview) azure'da. Daha sonra kullanılmak üzere yeni Web uygulaması URL'sini not edin.

Azure Stack
 - Bir Azure tümleşik yığını sistem kullanın veya Azure yığın Geliştirme Seti (ASDK) aşağıda bağlı dağıtın:
    - Konumundaki ASDK dağıtma hakkında ayrıntılı yönergeler bulabilirsiniz "[Öğreticisi: Yükleyicisi'ni kullanarak ASDK dağıtmak](https://docs.microsoft.com/azure/azure-stack/asdk/asdk-deploy)"
    - Çoğu, aşağıdaki PowerShell Betiği ile ASDK dağıtım sonrası adımları otomatikleştirmek [ConfigASDK.ps1](https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1 ).

    > [!note]  
    > ASDK yükleme yedi bir tamamlamak için bu nedenle uygun şekilde planlamanız saat sürer.

 - Dağıtma [uygulama hizmeti](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) Azure yığınına PaaS Hizmetleri. 
 - Oluşturma [planı/teklifleri](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) Azure yığın ortamında. 
 - Oluşturma [Kiracı aboneliği](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) Azure yığın ortamında. 
 - Kiracı abonelik içindeki bir Web uygulaması oluşturun. Daha sonra kullanmak için yeni Web uygulaması URL'si not edin.
 - VSTS sanal makine hala Kiracı aboneliği içinde dağıtın.
 - Windows Server 2016 VM ile .NET 3.5 gerekiyor. Bu VM, Azure yığında özel derleme aracısı olarak oluşturulacak. 

### <a name="developer-tools"></a>Geliştirici araçları

 - Oluşturma bir [VSTS çalışma](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services). Kayıt işlemini adlı bir proje oluşturur **MyFirstProject**.
 - [Visual Studio 2017 yükleme](https://docs.microsoft.com/visualstudio/install/install-visual-studio) ve [açma VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).
 - Projeye bağlayın ve [yerel olarak kopyalamak](https://www.visualstudio.com/docs/git/gitquickstart).
 
 > [!note]  
 > Dağıtılan uygulama hizmeti (Windows Server ve SQL) çalıştıran ve dağıtılmış uygun görüntülerle Azure yığın gerekir.
 
## <a name="prepare-the-private-build-and-release-agent-for-visual-studio-team-services-integration"></a>Özel derleme ve sürüm aracı Visual Studio Team Services tümleştirme için hazırlama

### <a name="prerequisites"></a>Önkoşullar

Visual Studio Team Services (VSTS) bir hizmet sorumlusu kullanarak Azure Resource Manager karşı doğrular. Bir Azure yığın Abonelikteki sağlama kaynaklarına olması VSTS için katkıda bulunan durumu gerektirir.

Bu tür kimlik doğrulamasını etkinleştirmek için yapılandırılması gereken üst düzey adımları şunlardır:

1. Hizmet sorumlusu oluşturulmalıdır veya mevcut bir kullanılabilir.
2. Kimlik doğrulama anahtarları için hizmet sorumlusu oluşturulması gerekir.
3. Azure yığın abonelik SPN katkıda bulunanlar rolünün bir parçası olarak izin vermek için rol tabanlı erişim denetimi doğrulanması gerekiyor.
4. VSTS yeni bir hizmet tanımında SPN bilgi yanı sıra Azure yığın uç noktaları kullanılarak oluşturulması gerekir.

### <a name="service-principal-creation"></a>Hizmet sorumlusu oluşturma

Başvurmak [hizmet sorumlusu oluşturma](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) yönergeleri bir hizmet sorumlusu oluşturma ve uygulama türü için Web uygulaması/API'ı seçin.

### <a name="access-key-creation"></a>Erişim anahtarı oluşturma

Bir hizmet asıl anahtar kimlik doğrulaması için gereklidir, bir anahtar oluşturmak için bu bölümdeki adımları izleyin.


1. Azure Active Directory'deki **Uygulama kayıtları**'nda uygulamanızı seçin.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\000_01.png)

2.  Değerini not edin **uygulama kimliği**. Hizmet uç noktası VSTS yapılandırırken bu değeri kullanır.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\000_02.png)

3. Kimlik doğrulama anahtarını oluşturmak için **Ayarlar**'ı seçin.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\000_03.png)

4. Kimlik doğrulama anahtarını oluşturmak için **Anahtarlar**'ı seçin.
 
    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\000_04.png)

5. Anahtar için bir açıklama ve süre sağlayın. İşiniz bittiğinde **Kaydet**’i seçin.
 
    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\000_05.png)

    Anahtar kaydedildikten sonra, anahtarın değeri görüntülenir. Bu değeri kopyalayın çünkü daha sonra anahtarı alamazsınız. Sağladığınız **anahtar değerini** uygulaması olarak oturum açmak için uygulama kimliği. Anahtarı, uygulamanızın alabileceği bir konumda depolayın.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\000_06.png)

### <a name="get-tenant-id"></a>Kiracı kimliğini alma

Hizmet uç noktası yapılandırmasının bir parçası olarak VSTS gerektirir **Kiracı kimliği** karşılık gelen Azure yığın damganız dağıtılmış olan bir AAD dizini. Kiracı kimliğini toplamak için bu bölümdeki adımları izleyin

1. **Azure Active Directory**'yi seçin.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\000_07.png)

2. Kiracı kimliğini almak için Azure AD kiracınızda **Özellikler**'i seçin.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\000_08.png)
 
3. **Dizin kimliği**'ni kopyalayın. Bu değer kiracı kimliğinizdir.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\000_09.png)

### <a name="grant-the-service-principal-rights-to-deploy-resources-in-the-azure-stack-subscription"></a>Azure yığın aboneliğindeki kaynaklar dağıtmak için hizmet asıl haklar 

Aboneliğinizde kaynaklara erişmek için bir rol uygulamaya atamanız gerekir. Uygulama için doğru izinlere rolünü karar verin. Kullanılabilir rolleri hakkında bilgi edinmek için [RBAC: yerleşik roller](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

Abonelik, kaynak grubu ya da kaynak düzeyinde kapsamı ayarlayabilirsiniz. Daha düşük düzeyde kapsam devralınan izinleri. Örneğin, bir kaynak grubu için okuyucu rolüne uygulamaya ekleme kaynak grubu ve içerdiği tüm kaynaklar okuyabilir anlamına gelir.

1. Uygulamaya atamak istediğiniz kapsam düzeyine gidin. Örneğin, abonelik kapsamında bir rol atamak için seçin **abonelikleri**. Bunun yerine, bir kaynak grubu veya kaynak seçebilirsiniz.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\000_10.png)

2. Seçin **abonelik** (kaynak grubu veya kaynak) uygulamaya atayın.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\000_11.png)

3. Seçin **erişim denetimi (IAM)**.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\000_12.png)

4. **Add (Ekle)** seçeneğini belirleyin.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\000_13.png)

5. Uygulamaya atamak istediğiniz rolü seçin. Aşağıdaki resimde gösterildiği **sahibi** rol.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\000_14.png)

6. Varsayılan olarak, Azure Active Directory uygulamaları kullanılabilir seçenekleri görüntülenmiyor. Uygulamanızı bulmak için şunları yapmanız gerekir **ad** arama alanında. Bunu seçin.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\000_16.png)

7. Seçin **kaydetmek** rol atama tamamlamak için. Listenin uygulamanızda bu kapsam için bir rolüne atanan kullanıcıların bakın.

### <a name="role-based-access-control"></a>Rol Tabanlı Access Control

Azure rol tabanlı erişim denetimi (RBAC) Azure için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, yalnızca kullanıcıların işlerini yapmak için gereksinim duyduğu erişim miktarını verebilirsiniz. Rol tabanlı erişim denetimi hakkında daha fazla bilgi için bkz: [Azure abonelik kaynaklarına erişimi yönetme](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal?toc=%252fazure%252factive-directory%252ftoc.json).

### <a name="vsts-agent-pools"></a>VSTS aracısı havuzları

Her bir aracının ayrı ayrı yönetmek yerine, aracıları aracı havuzu halinde düzenleyin. Bir aracı havuzu paylaşım sınır havuzda tüm aracılar için tanımlar. VSTS içinde aracısı havuzları VSTS hesabına kapsamındaki; Bu nedenle bir aracı havuzu ekip projeleri arasında paylaşabilirsiniz. Daha fazla bilgi ve VSTS aracısı havuzları oluşturma hakkında bir öğretici için bkz: [aracısı havuzları oluşturmak ve kuyruklar](https://docs.microsoft.com/vsts/build-release/concepts/agents/pools-queues?view=vsts).

### <a name="add-a-personal-access-token-pat-for-azure-stack"></a>Kişisel erişim belirteci (PAT) için Azure yığın ekleme

1. VSTS hesabınızda oturum açın ve hesabınızın profil adını seçin.
2. Seçin **Güvenliği Yönet** erişim belirteci oluşturma sayfası.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\000_17.png)

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\000_18.png)

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\000_18a.png)

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\000_18b.png)

3. Belirteç kopyalayın.
    
    > [!note]  
    > Belirteç bilgileri edinin. Bunu yeniden bu ekran bırakarak sonra gösterilmez. 
    
    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\000_19.png)
    

### <a name="install-the-vsts-build-agent-on-the-azure-stack-hosted-build-server"></a>Azure yığında VSTS yapı aracısını yükleme yapı sunucu barındırılan

1.  Yapı Azure yığın konakta dağıtılan sunucunuza bağlanın.

2.  Karşıdan yükleme ve dağıtma derleme aracısı kişisel kullanarak bir hizmet olarak erişim belirteci (PAT) ve VM yönetici farklı çalıştır hesabı.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\010_downloadagent.png)

3. Ayıklanan derleme aracısı klasörüne gidin. Çalıştırma **run.cmd** yükseltilmiş bir komut istemi dosyasından. 

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\000_20.png)

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\000_21.png)

4.  Run.cmd tamamladıktan sonra ayıklanan içeriğin bulunduğu klasöre aşağıdaki gibi görünmelidir:

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\009_token_file.png)

    VSTS klasöründeki aracı artık görebilirsiniz.

## <a name="endpoint-creation-permissions"></a>Uç nokta oluşturma izinleri

VSTO derlemeleri yığına Azure hizmet uygulamaları dağıtabilmek için kullanıcıları uç noktaları oluşturabilirsiniz. VSTS Azure yığın ile bağlanan derleme aracısı bağlanır. 

![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\012_securityendpoints.png)

1. Üzerinde **ayarları** menüsünde, select **güvenlik**.
2. İçinde **VSTS grupları** solda, select listesi **Endpoint oluşturucuları**. 

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\013_endpoint_creators.png)

3. Üzerinde **üyeleri** sekmesine **+ Ekle**. 

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\014_members_tab.png)

4. Bir kullanıcı adı yazın ve listeden kullanıcı seçin.
5. Tıklatın **değişiklikleri kaydetmek**.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\015_save_endpoint.png)

6. İçinde **VSTS grupları** solda, select listesi **Endpoint Yöneticiler**.
7. Üzerinde **üyeleri** sekmesine **+ Ekle**.
8. Bir kullanıcı adı yazın ve listeden kullanıcı seçin.
9. Tıklatın **değişiklikleri kaydetmek**.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\016_save_changes.png)

    Derleme aracısı Azure yığınında Azure yığını ile iletişim için uç nokta bilgileri iletir VSTS gelen yönergeleri kazanır. 
    
    Azure yığın bağlantı VSTS artık hazırdır.

## <a name="develop-your-application"></a>Uygulamanızı geliştirin

Web uygulaması Azure ve Azure yığın dağıtmak için karma CI/CD ayarlama ve Bulutlar hem de otomatik itme değiştirir.

> [!note]  
> Dağıtılan uygulama hizmeti (Windows Server ve SQL) çalıştıran ve dağıtılmış uygun görüntülerle Azure yığın gerekir. Azure yığın işleci gereksinimleri için uygulama hizmeti belgelerini "Önkoşullar" bölümü gözden geçirin.

### <a name="add-code-to-vsts-project"></a>VSTS projesi için kod ekleme

1. Visual Studio için Azure yığında proje oluşturma haklarına sahip bir hesapla oturum açın.

    Karma CI/CD hem uygulama kodunda hem de altyapı kodu uygulayabilirsiniz. Kullanım [web gibi Azure Resource Manager şablonları ](https://azure.microsoft.com/resources/templates/) uygulama kodundan VSTS Bulutlar hem de.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\017_connect_to_project.png)

2. **Depoyu kopyalayın** oluşturarak ve varsayılan web uygulaması açma.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\018_link_arm.png)

### <a name="create-self-contained-web-app-deployment-for-app-services-in-both-clouds"></a>Bulutlar hem de uygulama hizmetleri için kendi içinde bulunan web uygulama dağıtımı oluşturma

1. Düzenle **WebApplication.csproj** dosya: seçin **Runtimeidentifier** ve ekleme `win10-x64.` daha fazla bilgi için bkz: [kendi içinde bulunan dağıtım](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) belgeleri .

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\019_runtimeidentifer.png)

2. Kod Takım Gezgini'ni kullanarak VSTS denetleyin.

3. Uygulama kodu Visual Studio Team Services işaretli olduğunu doğrulayın. 

### <a name="create-the-build-definition"></a>Yapı tanımı oluşturun

1. VSTS derleme tanımları oluşturma yeteneği onaylamak için oturum açın.

2. Ekleme **- r win10-x64** kodu. .Net ile kendi içinde bulunan bir dağıtımı tetiklemek için gereken budur çekirdek. 

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\020_publish_additions.png)

3. Yapı çalıştırın. [Müstakil dağıtım yapı](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) işlemi, Azure ve Azure yığın çalışabilir yapıları yayımlar.

### <a name="using-an-azure-hosted-agent"></a>Bir Azure barındırılan aracı kullanma

VSTS içinde barındırılan bir aracı kullanarak, derleme ve web uygulamaları dağıtmak için kullanışlı bir seçenektir. Bakımı ve yükseltmeler, sürekli, kesintisiz geliştirme, test ve dağıtım etkinleştirme Microsoft Azure tarafından otomatik olarak gerçekleştirilir.

### <a name="manage-and-configure-the-continuous-deployment-cd-process"></a>Yönetme ve sürekli dağıtımı (CD) işlemi yapılandırma

Visual Studio Team Services (VSTS) ve Team Foundation Server (TFS) yüksek oranda yapılandırılabilir ve yönetilebilir bir ardışık düzen geliştirme gibi birden çok ortamlara sürümleri için hazırlama, QA ve üretim ortamlarını sağlar; belirli aşamalarda onayları gerektiren dahil olmak üzere.

### <a name="create-release-definition"></a>Yayın tanımı oluşturun

![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\021a_releasedef.png)

1. Seçin  **\[ +]** altında yeni bir sürüm eklemek için **sürümler sekmesini** VSO derleme ve sürüm sayfasında.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\102.png)

2. Uygulama **Azure uygulama hizmeti dağıtımı** şablonu.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\103.png)

3. Ekleyin yapıt aşağı açılır menüsünde altında **ekleyin yapıt** Azure bulut yapı uygulama.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\104.png)

4. Ardışık Düzen sekmesinde seçin **aşaması**, **görev** bağlantılandırın ortamını ve Azure bulut ortamı değerlerini ayarlayın.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\105.png)

5. Ayarlama **ortam adı** ve Azure seçin **abonelik** Azure bulut uç noktası için.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\106.png)

6. Ortam adı altında gerekli ayarlamak **Azure uygulama hizmeti adını**.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\107.png)

7. Girin **barındırılan VS2017** barındırılan Azure bulut ortamı için aracı sıra altında.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\108.png)

8. Azure uygulama hizmeti Dağıt menüde geçerli seçin **paket veya klasör** ortamı için. Seçin **Tamam** için **klasör konumu**.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\109.png)

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\110.png)

9. Tüm değişiklikleri kaydetmek ve geri dönüp **yayın ardışık düzen**.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\111.png)

10. Ekleme bir **yeni yapı** Azure yığın uygulama için derleme seçme.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\112.png)

11. Bir daha fazla ortam uygulama Ekle **Azure uygulama hizmeti dağıtımı**.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\113.png)

12. Yeni ortam adı **Azure yığın**.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\114.png)

13. Azure yığın ortamı altında Bul **görev** sekmesi.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\115.png)

14. Seçin **abonelik** Azure yığın uç noktası için.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\116.png)

15. Azure yığın web uygulaması adı olarak ayarlanmış **uygulama hizmet adı**.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\117.png)

16. Seçin **Azure yığın Aracısı**.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\118.png)

17. Azure uygulama hizmeti Dağıt altında bölümü seçmek geçerli **paket veya klasör** ortamı için. İçin Tamam'ı seçin **klasör konumu**.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\119.png)

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\120.png)

18. Değişken sekmesi altında adlı bir değişken Ekle **VSTS_ARM_REST_IGNORE_SSL_ERRORS**, değer olarak ayarlanmış **true**ve için kapsam **Azure yığın**.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\121.png)

19. Seçin **sürekli** dağıtımı tetiklemek hem yapıları simgesine ve devam eder dağıtım tetikleyici etkinleştirin.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\122.png)

20. Seçin **dağıtım öncesi** koşullar simgesi Azure yığın ortamını ve tetikleyici kümesine **sürümünden sonra**.

21. Tüm değişiklikleri kaydedin.

> [!note]  
> Görevler için bazı ayarları otomatik olarak tanımlanmış olabilir [ortam değişkenleri](https://docs.microsoft.com/vsts/build-release/concepts/definitions/release/variables?view=vsts#custom-variables) bir şablondan bir yayın tanımı oluşturduğunuzda. Bu ayarlar görev ayarlarında değiştirilemez; Bunun yerine, bu ayarları düzenlemek için üst ortam öğesi seçmelisiniz.

## <a name="create-a-release"></a>Sürüm oluşturma

Yayın tanımı için yapılan değişiklikleri tamamladığınıza göre dağıtım başlangıç zamanı geldi. Bunu yapmak için yayın tanımı sürüm oluşturun. Bir yayın otomatik olarak oluşturulabilir; Örneğin, sürekli dağıtım tetikleyici yayın tanımı'nda ayarlanır. Bu kaynak kodun değiştirilmesiyle yeni bir yapı başlar anlamına gelir ve bundan, yeni bir sürüm. Ancak, bu bölümde, yeni bir sürüm el ile oluşturacaksınız.

1. Açık **yayın** aşağı açılan listesinde ve seçin **sürüm oluşturma**.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\200.png)
 
2. Yayın için bir açıklama girin, doğru yapıları seçilir ve ardından denetleyin **oluşturma**. Birkaç dakika sonra yeni sürüm oluşturulduğunu belirten bir başlık görüntülenir. Bağlantı (yayın adı) seçin.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\201.png)
 
3. Yayın Özet sayfasını gösteren yayın ayrıntılarını açar. İçinde **ortamları** bölümünde, "Başarılı" "IN ilerlemesini" Değiştir "Kalite güvence" ortam dağıtım durumunu görürsünüz ve bu noktada, yayın şimdi onay bekliyor belirten bir başlık görüntülenir. Bilgi simgesi, bir ortam için bir dağıtım bekliyor ya da başarısız oldu, mavi (i) gösterilir. Bu nedenle içeren bir açılır pencere görmek için gelin.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\202.png)

Sürümler, listesi gibi diğer görünümleri de görüntü onay belirten bir simge beklemede. Simgenin üzerine ortam adı ve daha fazla ayrıntı içeren bir açılır pencere gösterir. Bu, hangi sürümlerinin tüm sürümleri genel ilerlemesi yanı sıra, onay bekleyen görmek bir yöneticinin kolaylaştırır.

### <a name="monitor-and-track-deployments"></a>İzleyicisi ve izleme dağıtımları

Bu bölümde, nasıl izleyeceğiniz görürsünüz ve dağıtımları - iki Azure App Services Web sitelerine - Bu örnekte önceki bölümde oluşturduğunuz sürümünden izleyebilirsiniz.

1. Özet sayfasında sürüm seçin **günlükleri** bağlantı. Dağıtım işlemi gerçekleşirken, bu sayfayı dinamik günlüğü aracısından ve sol bölmede, göstergesidir her ortam için dağıtım işlemi her bir işlemin durumunu gösterir.

    Simgeyi seçin **eylem** sütun kimin ayrıntılarını görmek dağıtım öncesi veya dağıtım sonrası bir onayı için onaylayan (veya reddeden) dağıtım ve ileti bu kullanıcı tarafından sağlanan.

2. Dağıtım tamamlandıktan sonra tüm günlük dosyasını sağ bölmede görüntülenir. Birini seçin **işlem adımları** yalnızca günlük dosyası içeriği bu adım için göstermek için sol bölmedeki. Bu, izleme ve tek tek parçaları genel dağıtım hatalarını ayıklamak kolaylaştırır. Alternatif olarak, ayrı günlük dosyalarına veya günlük dosyalarının zip simgeler ve sayfasındaki bağlantıları indirir.

    ![Alternatif metin](media\azure-stack-solution-hybrid-pipeline\203.png)
 
3. Açık **Özet** yayın genel ayrıntılarını görmek için sekmesini. Derleme ve dağıtılan - dağıtım durumunu ve sürümü hakkında diğer bilgi ile birlikte ortamları ayrıntılarını gösterir.

4. Her birini seçin **ortam bağlantılar** bekleyen dağıtımların o belirli bir ortama ve varolan hakkında daha fazla ayrıntı görmek için. Bu sayfa, aynı yapı için her iki ortama dağıtılan doğrulamak için kullanabilirsiniz.

5. Açık **üretim uygulama** , Gözat içinde. Örneğin, bir Azure App Services Web sitesi URL'den `http://[your-app-name].azurewebsites.net`.

## <a name="next-steps"></a>Sonraki adımlar

- Azure bulut desenleri hakkında daha fazla bilgi için bkz: [bulut tasarım modeli](https://docs.microsoft.com/azure/architecture/patterns).
