---
title: Azure Data Factory'de görsel yazma | Microsoft Docs
description: Azure Data Factory'de görsel yazma kullanmayı öğrenin
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/01/2018
ms.author: shlo
ms.openlocfilehash: 655a6ab2960047cde50bec2953015283ca8577f0
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39214872"
---
# <a name="visual-authoring-in-azure-data-factory"></a>Azure Data Factory'de görsel yazma
Görsel olarak yazma ve herhangi bir kod yazmak zorunda kalmadan, veri fabrikanızın kaynakları dağıtma Azure Data Factory kullanıcı arabirimi deneyimi (UX) sağlar. Etkinlikler bir işlem hattı tuvaline sürükleyin, test çalıştırmaları yapın, yinelemeli olarak, hata ayıklama ve dağıtabilir ve işlem hattı çalıştırmalarınızı izleyin. Görsel yazma gerçekleştirmek için kullanıcı Deneyimini kullanarak iki yaklaşım vardır:

- Data Factory hizmeti ile doğrudan yazar.
- İşbirliği, kaynak denetimi ve sürüm oluşturma için Visual Studio Team Services (VSTS) Git tümleştirmesiyle yazar.

## <a name="author-directly-with-the-data-factory-service"></a>Data Factory hizmeti ile doğrudan Yazar
Data Factory hizmetine görsel yazma visual VSTS ile iki şekilde geliştirme farklıdır:

- Data Factory hizmeti JSON varlıklar için yaptığınız değişiklikleri depolamak için bir depo içermez.
- Data Factory hizmetinin birlikte çalışma veya sürüm denetimi için iyileştirilmedi.

![Data Factory hizmetinin yapılandırma ](media/author-visually/configure-data-factory.png)

UX kullandığınızda **yazma tuvalinde** doğrudan Data Factory hizmetine, yalnızca yazar için **tümünü Yayımla** modu kullanılabilir. Yaptığınız değişiklikleri doğrudan Data Factory hizmetinde yayımlanır.

![Yayımlama modu](media/author-visually/data-factory-publish.png)

## <a name="author-with-vsts-git-integration"></a>VSTS Git tümleştirmesiyle yazma
VSTS Git tümleştirmesiyle görsel yazma data factory işlem hatlarınızı çalışma için kaynak denetimi ve işbirliği destekler. Veri fabrikası, bir kaynak denetimi, işbirliği, sürüm oluşturma ve benzeri için VSTS Gıt hesap deposu ile ilişkilendirebilirsiniz. Tek bir VSTS Gıt hesapta birden çok deposu olabilir, ancak VSTS Gıt deponuzda yalnızca bir data factory ile ilişkili olabilir. Bir VSTS hesabı veya depo yoksa izleyin [bu yönergeleri](https://docs.microsoft.com/vsts/accounts/create-account-msa-or-work-student) kaynaklarınızı oluşturmak için.

> [!NOTE]
> VSTS GIT deposunda betiğini ve veri dosyaları depolayabilir. Ancak, dosyaları Azure depolama alanına el ile karşıya yüklemeniz gerekir. Data Factory işlem hattı, Azure Depolama'ya bir VSTS GIT deponuzda saklanan kod veya veri dosyaları otomatik olarak yüklenmez.

### <a name="configure-a-vsts-git-repository-with-azure-data-factory"></a>Azure Data Factory ile VSTS Gıt deponuzda yapılandırın
VSTS GIT deponuzda iki yöntem bir data factory ile yapılandırabilirsiniz.

#### <a name="method1"></a> Yapılandırma yöntemine 1: başlayalım sayfası

Azure Data Factory'de Git **başlayalım** sayfası. Seçin **kod deposunu yapılandırma**:

![VSTS kod deposunu yapılandırma](media/author-visually/configure-repo.png)

**Depo ayarları** yapılandırma bölmesi görünür:

![Kod deposu ayarlarını yapılandırma](media/author-visually/repo-settings.png)

Bölmesinde aşağıdaki VSTS kod depo ayarları gösterir:

| Ayar | Açıklama | Değer |
|:--- |:--- |:--- |
| **Depo türü** | VSTS kod deposu türü.<br/>**Not**: GitHub şu anda desteklenmiyor. | Visual Studio Team Services Git |
| **Azure Active Directory** | Azure AD Kiracı adı. | <your tenant name> |
| **Visual Studio Team Services hesabı** | VSTS hesabı adı. VSTS hesabı adınızı bulabilirsiniz `https://{account name}.visualstudio.com`. Yapabilecekleriniz [VSTS hesabınızda oturum açın](https://www.visualstudio.com/team-services/git/) Visual Studio profilinize erişmek ve projeleri ve depoları bakın. | <your account name> |
| **projectName** | VSTS proje adı. VSTS projenizin adına konumunda bulabilirsiniz `https://{account name}.visualstudio.com/{project name}`. | <your VSTS project name> |
| **RepositoryName** | VSTS kod deponuzun adını. Projeniz büyüdükçe, kaynak kodunuzu yönetmek için Git depoları VSTS projeleri içerir. Yeni bir havuz oluşturabilir veya projenizde zaten olan mevcut bir depoyu kullanın. | <your VSTS code repository name> |
| **Birlikte çalışma dalı** | Yayımlama için kullanılacak olan, VSTS işbirliği dal. Varsayılan olarak, olduğu `master`. Kaynakları başka bir daldan yayımlamak istemeniz durumunda bu değiştirin. | <your collaboration branch name> |
| **Kök klasör** | Kök klasör VSTS işbirliği dalınızdaki. | <your root folder name> |
| **Mevcut Data Factory kaynaklarını depoya İçeri Aktar** | Mevcut data factory kaynaklarını UX'dan içeri aktarmak etkinleştirilip etkinleştirilmeyeceğini belirtir **yazma tuvalinde** VSTS Gıt deponuzda içine. JSON biçiminde ilişkili Git deposu, data factory kaynaklarını almak için kutusunu seçin. Bu eylem her kaynak ayrı ayrı verir (diğer bir deyişle, veri kümeleri ve bağlı hizmetler ayrı Json'lerini aktarılır). Bu kutusu seçili değilse, varolan kaynakları içe aktarılmaz. | Seçili (varsayılan) |

#### <a name="configuration-method-2-ux-authoring-canvas"></a>Yapılandırma yöntemine 2: UX yazma tuvali
Azure Data Factory UX içinde **yazma tuvalinde**, veri fabrikanızın bulun. Seçin **Data Factory** öğelerine tıklayın ve ardından **kod deposunu Yapılandır**.

Bir yapılandırma bölmesi görüntülenir. Yapılandırma ayarları hakkında daha fazla ayrıntı için bkz: açıklamasında <a href="#method1">yapılandırma yöntemi 1</a>.

![UX yazmak için kod deposu ayarlarını yapılandırma](media/author-visually/configure-repo-2.png)

#### <a name="switch-to-a-different-git-repo"></a>Farklı bir Git deposuna geçin

Farklı bir Git deposuna geçiş yapmak için aşağıdaki ekran görüntüsünde gösterildiği gibi Data Factory'ye genel bakış sayfasında, sağ üst köşesindeki simgeyi bulun. Simge göremiyorsanız, yerel tarayıcınızın önbelleğini temizleyin. Geçerli depo ilişkilendirmesini kaldırmak için simgeyi seçin.

Geçerli depo ilişkilendirmesini kaldırdıktan sonra farklı bir depoyu kullanmanız için Git ayarlarınızı yapılandırabilirsiniz. Daha sonra mevcut Data Factory kaynaklarını depoya yeni içeri aktarabilirsiniz.

![Geçerli bir Git deposu ile ilişkisini kaldırın.](media/author-visually/remove-repo.png)

### <a name="use-version-control"></a>Sürüm denetimi kullanın
Sürüm denetimi sistemlerinden (olarak da bilinen _kaynak denetimi_) kod için temel yapılan kod ve değişiklikleri izleyin üzerinde işbirliği geliştiricilerin olanak sağlar. Kaynak denetimi çok geliştirme projeleri için önemli bir araçtır.

Data factory ile ilişkili her VSTS Gıt deponuzda bir işbirliği dalı yok. (`master` varsayılan işbirliği dalı). Kullanıcılar ayrıca oluşturabilir özellik dalları tıklayarak **+ yeni dal** ve özellik dalları geliştirme yapın.

![Eşitleniyor veya yayımlama kodu değiştirin](media/author-visually/sync-publish.png)

Özellik dalınızda özellik geliştirmeye hazır olduğunuzda tıklayabilirsiniz **çekme isteği oluştur**. Bu sizi yönlendirir VSTS GİT'e olduğu yükseltmeniz çekme istekleri, kod incelemesi ve birleştirme işbirliği dalınızda değişiklik. (`master` varsayılandır). Yalnızca, işbirliği dalından Data Factory hizmetinde yayımlamak için izin verilir. 

![Yeni çekme isteği oluştur](media/author-visually/create-pull-request.png)

#### <a name="publish-code-changes"></a>Kod değişiklikleri Yayımla
İşbirliği dala değişiklikleri birleştirdiğiniz sonra (`master` varsayılandır) seçeneğini **Yayımla** el ile kod değişikliklerinizi ana daldaki Data Factory hizmetine yayımlamak.

![Değişiklikleri Data Factory hizmetinde yayımlayın](media/author-visually/publish-changes.png)

> [!IMPORTANT]
> Ana dal Data Factory hizmetinde dağıtılan temsili değildir. Ana dal *gerekir* yayımlanacağı el ile Data Factory hizmetinde yayımlayın.

## <a name="use-the-expression-language"></a>İfade dili kullanın
Azure Data Factory tarafından desteklenen bir ifade dilini kullanarak ifadeleri için özellik değerlerini belirtebilirsiniz. 

Özellik değerleri için ifadeleri belirleyerek **dinamik içerik Ekle**:

![İfade dili kullanın](media/author-visually/dynamic-content-1.png)

## <a name="use-functions-and-parameters"></a>İşlevlerini kullanın ve parametreleri

İşlevlerini kullanın ya da Data Factory'de işlem hatlarını ve veri kümeleri için parametreleri belirtin **ifade oluşturucu**:

Desteklenen ifadeler hakkında daha fazla bilgi için bkz: [ifadeler ve İşlevler Azure Data factory'de](control-flow-expression-language-functions.md).

![Dinamik İçerik Ekle](media/author-visually/dynamic-content-2.png)

## <a name="provide-feedback"></a>Geri bildirimde bulunma
Seçin **geri bildirim** özellikleri hakkında yorum yapmak veya Microsoft aracı ile ilgili sorunları bildirmek için:

![Geri Bildirim](media/author-visually/provide-feedback.png)

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla ilgili izleme ve işlem hatlarını yönetmek öğrenmek için bkz [izleme ve işlem hatlarını programlama yoluyla yönetme](monitor-programmatically.md).
