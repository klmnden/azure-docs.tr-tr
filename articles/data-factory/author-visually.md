---
title: Azure Data Factory'de görsel yazma | Microsoft Docs
description: Azure Data Factory'de görsel yazma kullanmayı öğrenin
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/09/2019
author: sharonlo101
ms.author: shlo
ms.reviewer: ''
manager: craigg
ms.openlocfilehash: b32e4abebdfb93312c60a25ca8b483f071e5e73c
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65507808"
---
# <a name="visual-authoring-in-azure-data-factory"></a>Azure Data Factory'de görsel yazma
Görsel olarak yazma ve herhangi bir kod yazmak zorunda kalmadan, veri fabrikanızın kaynakları dağıtma Azure Data Factory kullanıcı arabirimi deneyimi (UX) sağlar. Etkinlikler bir işlem hattı tuvaline sürükleyin, test çalıştırmaları yapın, yinelemeli olarak, hata ayıklama ve dağıtabilir ve işlem hattı çalıştırmalarınızı izleyin. Görsel yazma gerçekleştirmek için kullanıcı Deneyimini kullanarak iki yaklaşım vardır:

- Data Factory hizmeti ile doğrudan yazar.
- İşbirliği, kaynak denetimi ve sürüm oluşturma için Azure depoları Git tümleştirmesiyle yazar.

## <a name="author-directly-with-the-data-factory-service"></a>Data Factory hizmeti ile doğrudan Yazar
Data Factory hizmetine görsel yazma görsel iki yolla Git tümleştirmesiyle yazma öğesinden farklıdır:

- Data Factory hizmeti JSON varlıklar için yaptığınız değişiklikleri depolamak için bir depo içermez.
- Data Factory hizmetinin birlikte çalışma veya sürüm denetimi için iyileştirilmedi.

![Data Factory hizmetinin yapılandırma](media/author-visually/configure-data-factory.png)

UX kullandığınızda **yazma tuvalinde** doğrudan Data Factory hizmetine, yalnızca yazar için **tümünü Yayımla** modu kullanılabilir. Yaptığınız değişiklikleri doğrudan Data Factory hizmetinde yayımlanır.

![Yayımlama modu](media/author-visually/data-factory-publish.png)

## <a name="author-with-azure-repos-git-integration"></a>Azure depoları Git tümleştirmesiyle yazma
Azure depoları Git tümleştirmesiyle görsel yazma data factory işlem hatlarınızı çalışma için kaynak denetimi ve işbirliği destekler. Kaynak denetimi, işbirliği, sürüm oluşturma ve benzeri için bir Azure depoları Git kuruluş deposuna bir veri fabrikası ilişkilendirebilirsiniz. Tek bir Azure depoları Git kuruluştaki birden çok deposu olabilir, ancak bir Azure depoları Git deposu yalnızca bir data factory ile ilişkili olabilir. Bir Azure depoları kuruluş ya da depo yoksa izleyin [bu yönergeleri](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization-msa-or-work-student) kaynaklarınızı oluşturmak için.

> [!NOTE]
> Bir Azure depoları Git deposunda betiğini ve veri dosyaları depolayabilir. Ancak, dosyaları Azure depolama alanına el ile karşıya yüklemeniz gerekir. Data Factory işlem hattı, Azure depolama için bir Azure depoları Git deposunda depolanan kod veya veri dosyaları otomatik olarak yüklenmez.

### <a name="configure-an-azure-repos-git-repository-with-azure-data-factory"></a>Azure Data Factory ile bir Azure depoları Git deposu yapılandırma
Bir Azure depoları Git deposu ile veri fabrikası iki yöntemleri yapılandırabilirsiniz.

#### <a name="method1"></a> Yapılandırma yöntemine 1 (Azure depoları Git deposu): Başlayalım sayfası

Azure Data Factory'de Git **başlayalım** sayfası. Seçin **kod deposunu yapılandırma**:

![Bir Azure depoları kod deposunu yapılandırma](media/author-visually/configure-repo.png)

**Depo ayarları** yapılandırma bölmesi görünür:

![Kod deposu ayarlarını yapılandırma](media/author-visually/repo-settings.png)

Bölmesinde, aşağıdaki Azure depoları kod depo ayarları gösterir:

| Ayar | Açıklama | Değer |
|:--- |:--- |:--- |
| **Depo türü** | Azure depoları kod deposu türü.<br/> | Azure depoları Git |
| **Azure Active Directory** | Azure AD Kiracı adı. | `<your tenant name>` |
| **Azure depoları kuruluş** | Azure depoları kuruluşunuzun adı. Azure depoları kuruluş adınızı bulabilirsiniz `https://{organization name}.visualstudio.com`. Yapabilecekleriniz [Azure depoları kuruluşunuz oturum](https://www.visualstudio.com/team-services/git/) Visual Studio profilinize erişmek ve projeleri ve depoları bakın. | `<your organization name>` |
| **projectName** | Azure depoları proje adı. Azure depoları proje adınızı bulabilirsiniz `https://{organization name}.visualstudio.com/{project name}`. | `<your Azure Repos project name>` |
| **RepositoryName** | Azure depoları kod deponuzun adını. Projeniz büyüdükçe, kaynak kodunuzu yönetmek için Git depoları Azure depoları projeleri içerir. Yeni bir havuz oluşturabilir veya projenizde zaten olan mevcut bir depoyu kullanın. | `<your Azure Repos code repository name>` |
| **Birlikte çalışma dalı** | Yayımlama için kullanılan Azure depoları işbirliği dalınızı. Varsayılan olarak, olduğu `master`. Kaynakları başka bir daldan yayımlamak istemeniz durumunda bu ayarı değiştirin. | `<your collaboration branch name>` |
| **Kök klasör** | Kök klasör Azure depoları işbirliği dalınızdaki. | `<your root folder name>` |
| **Mevcut Data Factory kaynaklarını depoya İçeri Aktar** | Mevcut data factory kaynaklarını UX'dan içeri aktarmak etkinleştirilip etkinleştirilmeyeceğini belirtir **yazma tuvalinde** Azure depoları Git deponuzla. JSON biçiminde ilişkili Git deposu, data factory kaynaklarını almak için kutusunu seçin. Bu eylem her kaynak ayrı ayrı verir (diğer bir deyişle, veri kümeleri ve bağlı hizmetler ayrı Json'lerini aktarılır). Bu kutusu seçili değilse, varolan kaynakları içe aktarılmaz. | Seçili (varsayılan) |

#### <a name="configuration-method-2-azure-repos-git-repo-ux-authoring-canvas"></a>Yapılandırma yöntemine 2 (Azure depoları Git deposu): Yazma tuvali UX
Azure Data Factory UX içinde **yazma tuvalinde**, veri fabrikanızın bulun. Seçin **Data Factory** öğelerine tıklayın ve ardından **kod deposunu Yapılandır**.

Bir yapılandırma bölmesi görüntülenir. Yapılandırma ayarları hakkında daha fazla ayrıntı için bkz: açıklamasında <a href="#method1">yapılandırma yöntemi 1</a>.

![UX yazmak için kod deposu ayarlarını yapılandırma](media/author-visually/configure-repo-2.png)

### <a name="use-a-different-azure-active-directory-tenant"></a>Farklı bir Azure Active Directory kiracısı kullanma

Bir Azure depoları Git deposu içinde farklı bir Azure Active Directory kiracısı oluşturabilirsiniz. Farklı bir belirtmek için Azure AD kiracısına sahip kullanmakta olduğunuz Azure aboneliği için yönetici izinlerine sahip olması.

### <a name="use-your-personal-microsoft-account"></a>Kişisel Microsoft hesabınızı kullanın

Git tümleştirmesi için kişisel bir Microsoft hesabı kullanmak için kuruluşunuzun Active Directory ile kişisel Azure deponuzu bağlayabilirsiniz.

1. Kişisel Microsoft hesabınız, kuruluşunuzun Active Directory'ye konuk olarak ekleyin. Daha fazla bilgi için bkz. [ekleme Azure Active Directory B2B işbirliği kullanıcılarını Azure portalında](../active-directory/b2b/add-users-administrator.md).

2. Kişisel Microsoft hesabınızla Azure portalında oturum açın. Ardından, kuruluşunuzun Active Directory geçin.

3. Şimdi kişisel deponuza gördüğünüz Azure DevOps bölümüne gidin. Depo seçin ve Active Directory ile bağlanın.

Bu yapılandırma adımları sonra Data Factory kullanıcı arabiriminde Git tümleştirmesi ayarladığınızda kişisel deponuza kullanılabilir.

Kuruluşunuzun Active Directory ile Azure depoları bağlanma hakkında daha fazla bilgi için bkz. [Azure DevOps kuruluşunuz Azure Active Directory'ye bağlamanın](/azure/devops/organizations/accounts/connect-organization-to-azure-ad).

### <a name="switch-to-a-different-git-repo"></a>Farklı bir Git deposuna geçin

Farklı bir Git deposuna geçiş yapmak için aşağıdaki ekran görüntüsünde gösterildiği gibi Data Factory'ye genel bakış sayfasında, sağ üst köşesindeki simgeyi bulun. Simge göremiyorsanız, yerel tarayıcınızın önbelleğini temizleyin. Geçerli depo ilişkilendirmesini kaldırmak için simgeyi seçin.

Geçerli depo ilişkilendirmesini kaldırdıktan sonra farklı bir depoyu kullanmanız için Git ayarlarınızı yapılandırabilirsiniz. Daha sonra mevcut Data Factory kaynaklarını depoya yeni içeri aktarabilirsiniz.

![Geçerli bir Git deposu ile ilişkilendirmesini Kaldır](media/author-visually/remove-repo.png)

### <a name="use-version-control"></a>Sürüm denetimi kullanın
Sürüm denetimi sistemlerinden (olarak da bilinen _kaynak denetimi_) kod için temel yapılan kod ve değişiklikleri izleyin üzerinde işbirliği geliştiricilerin olanak sağlar. Kaynak denetimi çok geliştirme projeleri için önemli bir araçtır.

Data factory ile ilişkili her Azure depoları Git deposuna bir işbirliği dalı yok. (`master` varsayılan işbirliği dalı). Kullanıcılar ayrıca oluşturabilir özellik dalları tıklayarak **+ yeni dal** ve özellik dalları geliştirme yapın.

![Eşitleniyor veya yayımlama kodu değiştirin](media/author-visually/sync-publish.png)

Özellik dalınızda özellik geliştirmeye hazır olduğunuzda tıklayabilirsiniz **çekme isteği oluştur**. Bu eylem, Azure depoları Git çekme istekleri, burada yükseltebilirsiniz, kod incelemeleri ve işbirliği dalınızdaki değişiklikleri Birleştir alır. (`master` varsayılandır). Yalnızca, işbirliği dalından Data Factory hizmetinde yayımlamak için izin verilir. 

![Yeni çekme isteği oluştur](media/author-visually/create-pull-request.png)

### <a name="configure-publishing-settings"></a>Yayımlama ayarlarını yapılandırma

Diğer bir deyişle, burada Resource Manager şablonları kaydedilir - dal yayımlama dal - yapılandırmak için Ekle bir `publish_config.json` işbirliği dalın kök klasörüne dosya. Data Factory, bu dosyayı okur, alan için arama yapar `publishBranch`ve (zaten yoksa), sağlanan değer ile yeni bir dal oluşturur. Sonra tüm Resource Manager şablonları belirtilen konuma kaydeder. Örneğin:

```json
{
    "publishBranch": "factory/adf_publish"
}
```

Git modundan yayımladığınızda, Data Factory beklediğiniz Yayımla dal aşağıdaki ekran görüntüsünde gösterildiği gibi kullandığını doğrulayabilirsiniz:

![Doğru yayımlama dal onaylayın](media/author-visually/configure-publish-branch.png)

Yeni bir yayımlama dal belirttiğinizde, Data Factory önceki yayımlama dal silmez. Önceki Yayınla dalı uzaktan istiyorsanız, el ile silin.

Yalnızca veri fabrikası okur `publish_config.json` Fabrika yüklediğinde, dosya. Portalda yüklenen Fabrika zaten varsa, etkili, değişiklik yapmak için tarayıcıyı yenileyin.

### <a name="publish-code-changes"></a>Kod değişiklikleri Yayımla
İşbirliği dala değişiklikleri birleştirdiğiniz sonra (`master` varsayılandır) seçeneğini **Yayımla** el ile kod değişikliklerinizi ana daldaki Data Factory hizmetine yayımlamak.

![Değişiklikleri Data Factory hizmetinde yayımlayın](media/author-visually/publish-changes.png)

> [!IMPORTANT]
> Ana dal Data Factory hizmetinde dağıtılan temsili değildir. Ana dal *gerekir* yayımlanacağı el ile Data Factory hizmetinde yayımlayın.

### <a name="advantages-of-git-integration"></a>Git tümleştirmesi avantajları

-   **Kaynak denetimi**. Data factory iş yüklerinizi önemli olduğunda, aşağıdaki gibi birçok kaynak denetimi Avantajdan yararlanmak için Git fabrikanızı tümleştirmek istersiniz:
    -   Değişiklik izleme/denetim olanağı.
    -   Hatanın değişiklikleri geri al olanağı.
-   **Kısmi kaydeder**. Fabrikanızı içinde çok sayıda değişiklik yaptığınız gibi normal Canlı modda, değişiklikleriniz taslak olarak hazır olmayan veya bilgisayarın kilitlenmesi durumda değişikliklerinizi kaybetmek istemediğiniz için kaydedilemiyor olduğunu fark edeceksiniz. Git Tümleştirmesi ile artımlı olarak değişikliklerinizi kaydetmeye devam etmek ve yalnızca hazır olduğunuzda factory'de yayımlamak. İçin tatmininizi değişikliklerinizi test kadar Git için iş hazırlama bir yer olarak görev yapar.
-   **İşbirliği ve Denetim**. Birden çok takım üyeleri için aynı Fabrika katılan varsa, kodunuza bir kod incelemesi süreci birbirleriyle işbirliği yapmak isteyebilirsiniz. Her katılımcı factory'ye factory'ye dağıtırsınız iznine sahip olacak şekilde, Fabrika de ayarlayabilirsiniz. Takım üyeleri yalnızca Git aracılığıyla değişiklik yapmak için izin verilmiyor olabilir ancak yalnızca belirli kişiler takım "Yayımla" Fabrika değişiklikleri izin verilir.
-   **Farkları gösteren**. Git modunda hakkındadır yükü iyi farkı görmek bilgi almak için Fabrika yayımlanmış. Bu fark, tüm kaynakları/değişiklik/eklenen/silinen factory'nize yayımlanan son Time alındı varlıkları gösterir. Bu fark üzerinde bağlı olarak, ya da devam ederek yayımlama ile daha fazla veya geri dönün ve değişikliklerinizi denetleyin ve ardından daha sonra tekrar deneyin.
-   **Daha iyi bir CI/CD**. Git modu kullanıyorsanız, yayın işlem hattınızı otomatik olarak geliştirme fabrikada yapılan değişiklikler olduğundan hemen tetiklemek için yapılandırabilirsiniz. Resource Manager şablonunda parametreler olarak kullanılabilir olan özellikleri Fabrikanızdaki özelleştirmek alın. Yalnızca gerekli özellikleri olarak parametreler kümesi tutun ve her şeyi başka sabit kodlanmış olması için yararlı olabilir.
-   **Daha iyi performans**. Kaynakları Git yüklenir çünkü bir ortalama fabrikası normal Canlı modda Git modunda hızlı 10 x kez yükler.

### <a name="best-practices-for-git-integration"></a>Git tümleştirmesi için en iyi uygulamalar

-   **İzinleri**. Genellikle Fabrika güncelleştirmek için izinlere sahip için tüm takım üyelerini istemezsiniz.
    -   Tüm takım üyelerini data factory'ye Okuma izinlerine sahip.
    -   Yalnızca bir grup kişi factory'ye ve için "Data Factory Katılımcısı" rol Fabrika üzerinde bir parçası olarak gerek yayımlamak için izin verilmelidir.
    -   Kaynak denetimi en iyi yöntemler de doğrudan iadeler işbirliği dalına izin vermeyecek şekilde biridir. Bu gereksinim, her iade bir çekme isteği işlem sırasında geçebileceği hataları önler.
-   **Modlar arasında geçiş yapmak**.
    -    Git modunda olduğunda, öncelikle Canlı modda yapılan tüm değişiklikleri değil görülme çünkü Git'e geri değiştirdiğinizde, İleri ve geri Canlı moduna geçmek için önermemekteyiz. Git modunda kendisini değişiklikleri yapın ve ardından bunları bir kullanıcı Arabirimi yayımlama deneyin.
    -   Benzer şekilde, bunlar doğrudan Canlı Fabrika için sağlanan değişiklikleri uygulayarak aynı etkiyi elde gibi herhangi bir veri fabrikası powershell cmdlet'leri, kullanmayın.
-   **Azure Key vault'tan parolalarını**.
    -   Herhangi bir bağlantı dizelerini veya DataFactory bağlı hizmetler için parolaları depolamak için AzureKeyVault kullanarak öneririz.
    -   Biz gibi gizli bilgiler Git'te (güvenlik nedeniyle), böylece tüm bağlı hizmetler değişiklikler hemen Canlı factory'ye yayımlanır depolamayın. Değişiklikleri sınanmamış gibi bu hemen yayımlama bazen, git'in amacını boşa çıkarır, istenildiği gibi.
    -   Sonuç olarak, tüm gizli bağlı hizmetlerinden tabanlı Azure Key Vault'u kullanın getirilmesi gerekir.
    -   Anahtar kasası kullanmanın diğer avantajları bazıları CICD, gizli dizileri sırasında Resource Manager şablon dağıtımı sağlamanıza yapmayarak kolaylaştırır emin olur.

## <a name="author-with-github-integration"></a>GitHub Tümleştirmesi ile içerik oluşturma

GitHub Tümleştirmesi ile görsel yazma data factory işlem hatlarınızı çalışma için kaynak denetimi ve işbirliği destekler. Veri fabrikası, bir GitHub hesabı deposu için kaynak denetimi, işbirliği ve sürüm oluşturma ile ilişkilendirebilirsiniz. Tek bir GitHub hesabı birden çok deposu olabilir, ancak bir GitHub deposuna tek bir data factory ile ilişkili olabilir. Bir GitHub hesabı ya da depo yoksa izleyin [bu yönergeleri](https://github.com/join) kaynaklarınızı oluşturmak için.

Her iki genel GitHub Data Factory ile GitHub tümleştirmesini destekler (diğer bir deyişle, [ https://github.com ](https://github.com)) ve GitHub Enterprise. Uzun okuma ve yazma izni GitHub deponuza gibi genel ve özel GitHub depoları Data Factory ile kullanabilirsiniz.

GitHub deposunu yapılandırmak için kullandığınız Azure aboneliği için yönetici izinlerine sahip olmanız.

Dokuz dakikalık bir giriş ve bu özelliği için şu videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Azure-Data-Factory-visual-tools-now-integrated-with-GitHub/player]

### <a name="limitations"></a>Sınırlamalar

- Bir GitHub deposunda betiğini ve veri dosyaları depolayabilir. Ancak, dosyaları Azure depolama alanına el ile karşıya yüklemeniz gerekir. Data Factory işlem hattı, Azure Depolama'ya bir GitHub deposu içinde depolanan kod veya veri dosyaları otomatik olarak yüklenmez.

- Microsoft Edge tarayıcısında GitHub Enterprise 2.14.0 eski bir sürümü ile çalışmaz.

- Veri faktörü visual geliştirme araçları ile GitHub tümleştirmesini yalnızca veri fabrikası genel kullanıma sunulan sürümünde çalışır.

### <a name="configure-a-public-github-repository-with-azure-data-factory"></a>Azure Data Factory ile bir ortak GitHub deposu yapılandırma

Bir GitHub deposu ile veri fabrikası iki yöntemleri yapılandırabilirsiniz.

**Yapılandırma yöntemine 1 (genel deponun): Başlayalım sayfası**

Azure Data Factory'de Git **başlayalım** sayfası. Seçin **kod deposunu yapılandırma**:

![Veri Fabrikası başlangıç sayfası](media/author-visually/github-integration-image1.png)

 **Depo ayarları** yapılandırma bölmesi görünür:

![GitHub depo ayarları](media/author-visually/github-integration-image2.png)

Bölmesinde, aşağıdaki Azure depoları kod depo ayarları gösterir:

| **Ayar**                                              | **Açıklama**                                                                                                                                                                                                                                                                                                                                                                                                                   | **Değer**          |
|----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|
| **Depo türü**                                      | Azure depoları kod deposu türü.                                                                                                                                                                                                                                                                                                                                                                                             | GitHub             |
| **GitHub hesabı**                                       | GitHub hesabı adı. Bu ad https'den bulunabilir:\//github.com/{account adı} / {depo adı}. Bu sayfaya giden GitHub hesabınızı GitHub OAuth kimlik bilgilerini girmenizi ister.                                                                                                                                                                                                                                               |                    |
| **RepositoryName**                                       | GitHub kod deponuzun adını. Git depoları, kaynak kodunuzu yönetmek için GitHub hesaplarında bulunur. Yeni bir havuz oluşturabilir veya hesabınızda zaten varolan bir depo kullanın.                                                                                                                                                                                                                              |                    |
| **Birlikte çalışma dalı**                                 | Yayımlama için kullanılan, GitHub işbirliği dal. Varsayılan olarak, ana. Kaynakları başka bir daldan yayımlamak istemeniz durumunda bu ayarı değiştirin.                                                                                                                                                                                                                                                               |                    |
| **Kök klasör**                                          | Kök klasör GitHub işbirliği dalınızdaki.                                                                                                                                                                                                                                                                                                                                                                             |                    |
| **Mevcut Data Factory kaynaklarını depoya İçeri Aktar** | Mevcut data factory kaynaklarını UX'dan içeri aktarmak etkinleştirilip etkinleştirilmeyeceğini belirtir **yazma tuvalinde** içine bir GitHub deposu. JSON biçiminde ilişkili Git deposu, data factory kaynaklarını almak için kutusunu seçin. Bu eylem her kaynak ayrı ayrı verir (diğer bir deyişle, veri kümeleri ve bağlı hizmetler ayrı Json'lerini aktarılır). Bu kutusu seçili değilse, varolan kaynakları içe aktarılmaz. | Seçili (varsayılan) |
| **Dal kaynağını içeri aktarmak için**                       | Veri Fabrikası Kaynakları (işlem hatları, veri kümeleri, bağlı hizmetler vb.) hangi dala içe aktarılacağını belirler. Aşağıdaki dalları biri kaynakların içeri aktarabilirsiniz: bir. İşbirliği b. Yeni c oluşturun. Var Olanı Kullan                                                                                                                                                                                                     |                    |

#### <a name="configuration-method-2-public-repo-ux-authoring-canvas"></a>Yapılandırma yöntemine 2 (ortak depo): Yazma tuvali UX

Azure Data Factory UX içinde **yazma tuvalinde**, veri fabrikanızın bulun. Seçin **Data Factory** öğelerine tıklayın ve ardından **kod deposunu Yapılandır**.

Bir yapılandırma bölmesi görüntülenir. Yapılandırma ayarları hakkında daha fazla ayrıntı için bağlantısındaki açıklamalara bakın *yapılandırma yöntemi 1* yukarıda.

### <a name="configure-a-github-enterprise-repository-with-azure-data-factory"></a>Azure Data Factory ile bir GitHub Enterprise deposunu yapılandırma

GitHub Enterprise depo ile veri fabrikası iki yöntemleri yapılandırabilirsiniz.

#### <a name="configuration-method-1-enterprise-repo-lets-get-started-page"></a>Yapılandırma yöntemine 1 (Kurumsal depo): Başlayalım sayfası

Azure Data Factory'de Git **başlayalım** sayfası. Seçin **kod deposunu yapılandırma**:

![Veri Fabrikası başlangıç sayfası](media/author-visually/github-integration-image1.png)

 **Depo ayarları** yapılandırma bölmesi görünür:

![GitHub depo ayarları](media/author-visually/github-integration-image3.png)

Bölmesinde, aşağıdaki Azure depoları kod depo ayarları gösterir:

| **Ayar**                                              | **Açıklama**                                                                                                                                                                                                                                                                                                                                                                                                                   | **Değer**          |
|----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|
| **Depo türü**                                      | Azure depoları kod deposu türü.                                                                                                                                                                                                                                                                                                                                                                                             | GitHub             |
| **GitHub Enterprise kullanın**                                | GitHub Enterprise seçmek için onay kutusu                                                                                                                                                                                                                                                                                                                                                                                              |                    |
| **GitHub Enterprise URL'si**                                | GitHub Enterprise kök URL'si. Örneğin, https://github.mydomain.com                                                                                                                                                                                                                                                                                                                                                          |                    |
| **GitHub hesabı**                                       | GitHub hesabı adı. Bu ad https'den bulunabilir:\//github.com/{account adı} / {depo adı}. Bu sayfaya giden GitHub hesabınızı GitHub OAuth kimlik bilgilerini girmenizi ister.                                                                                                                                                                                                                                               |                    |
| **RepositoryName**                                       | GitHub kod deponuzun adını. Git depoları, kaynak kodunuzu yönetmek için GitHub hesaplarında bulunur. Yeni bir havuz oluşturabilir veya hesabınızda zaten varolan bir depo kullanın.                                                                                                                                                                                                                              |                    |
| **Birlikte çalışma dalı**                                 | Yayımlama için kullanılan, GitHub işbirliği dal. Varsayılan olarak, ana. Kaynakları başka bir daldan yayımlamak istemeniz durumunda bu ayarı değiştirin.                                                                                                                                                                                                                                                               |                    |
| **Kök klasör**                                          | Kök klasör GitHub işbirliği dalınızdaki.                                                                                                                                                                                                                                                                                                                                                                             |                    |
| **Mevcut Data Factory kaynaklarını depoya İçeri Aktar** | Mevcut data factory kaynaklarını UX'dan içeri aktarmak etkinleştirilip etkinleştirilmeyeceğini belirtir **yazma tuvalinde** içine bir GitHub deposu. JSON biçiminde ilişkili Git deposu, data factory kaynaklarını almak için kutusunu seçin. Bu eylem her kaynak ayrı ayrı verir (diğer bir deyişle, veri kümeleri ve bağlı hizmetler ayrı Json'lerini aktarılır). Bu kutusu seçili değilse, varolan kaynakları içe aktarılmaz. | Seçili (varsayılan) |
| **Dal kaynağını içeri aktarmak için**                       | Veri Fabrikası Kaynakları (işlem hatları, veri kümeleri, bağlı hizmetler vb.) hangi dala içe aktarılacağını belirler. Aşağıdaki dalları biri kaynakların içeri aktarabilirsiniz: bir. İşbirliği b. Yeni c oluşturun. Var Olanı Kullan                                                                                                                                                                                                     |                    |

#### <a name="configuration-method-2-enterprise-repo-ux-authoring-canvas"></a>Yapılandırma yöntemine 2 (Kurumsal depo): Yazma tuvali UX

Azure Data Factory UX içinde **yazma tuvalinde**, veri fabrikanızın bulun. Seçin **Data Factory** öğelerine tıklayın ve ardından **kod deposunu Yapılandır**.

Bir yapılandırma bölmesi görüntülenir. Yapılandırma ayarları hakkında daha fazla ayrıntı için bağlantısındaki açıklamalara bakın *yapılandırma yöntemi 1* yukarıda.

## <a name="use-the-expression-language"></a>İfade dili kullanın
Azure Data Factory tarafından desteklenen bir ifade dilini kullanarak ifadeleri için özellik değerlerini belirtebilirsiniz.

Özellik değerleri için ifadeleri belirleyerek **dinamik içerik Ekle**:

![İfade dili kullanın](media/author-visually/dynamic-content-1.png)

## <a name="use-functions-and-parameters"></a>İşlevlerini kullanın ve parametreleri

İşlevlerini kullanın ya da Data Factory'de işlem hatlarını ve veri kümeleri için parametreleri belirtin **ifade oluşturucu**:

Desteklenen ifadeler hakkında daha fazla bilgi için bkz: [ifadeler ve İşlevler Azure Data factory'de](control-flow-expression-language-functions.md).

![Dinamik İçerik Ekle](media/author-visually/dynamic-content-2.png)

## <a name="provide-feedback"></a>Geri bildirim gönder
Seçin **geri bildirim** özellikleri hakkında yorum yapmak veya Microsoft aracı ile ilgili sorunları bildirmek için:

![Geri Bildirim](media/author-visually/provide-feedback.png)

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla ilgili izleme ve işlem hatlarını yönetmek öğrenmek için bkz [izleme ve işlem hatlarını programlama yoluyla yönetme](monitor-programmatically.md).
