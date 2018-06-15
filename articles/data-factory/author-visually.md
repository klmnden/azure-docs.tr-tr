---
title: Azure Data Factory'de görsel geliştirme | Microsoft Docs
description: Azure Data Factory'de Visual yazma kullanmayı öğrenin
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
ms.openlocfilehash: b588fd4b67dbed1e0dc3d4ad2cbd75b462ce311f
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34725151"
---
# <a name="visual-authoring-in-azure-data-factory"></a>Azure Data Factory'de Visual yazma
Görsel olarak yazar ve herhangi bir kod yazmak zorunda kalmadan veri fabrikanızın kaynakları dağıtma Azure Data Factory kullanıcı arabirimi (UX) deneyimi sağlar. Etkinlikleri bir ardışık düzen tuvale sürükleyin, test çalışmalarını gerçekleştirmek, yinelemeli olarak, hata ayıklama ve dağıtabilir ve ardışık düzen çalışmalarınız izleyin. Görsel geliştirme gerçekleştirmek için kullanıcı Deneyimini kullanmak için iki yaklaşım vardır:

- Data Factory hizmeti ile doğrudan yazar.
- İşbirliği, kaynak denetimi veya sürüm oluşturma için Visual Studio Team Services (VSTS) Git Tümleştirmesi ile yazar.

## <a name="author-directly-with-the-data-factory-service"></a>Data Factory hizmeti ile doğrudan Yazar
Data Factory hizmetiyle Visual yazma visual VSTS ile iki şekilde geliştirme farklıdır:

- Data Factory hizmetinin değişikliklerinizi JSON varlıkları depolamak için bir depo yok.
- Data Factory hizmetinin işbirliği veya sürüm denetimi için iyileştirilmemiş.

![Data Factory hizmetinin yapılandırma ](media/author-visually/configure-data-factory.png)

UX kullandığınızda **tuvale yazma** doğrudan Data Factory hizmetiyle, yalnızca yazmak için **tümünü Yayımla** modu kullanılabilir. Yaptığınız tüm değişiklikler veri fabrikası hizmetine doğrudan yayımlanır.

![Mod yayımlama](media/author-visually/data-factory-publish.png)

## <a name="author-with-vsts-git-integration"></a>VSTS Git Tümleştirmesi ile yazar
VSTS Git Tümleştirmesi ile Visual yazma veri fabrikası hatlarınızı çalışmaları için kaynak denetimi ve işbirliği destekler. Kaynak denetimi, işbirliği, sürüm ve benzeri için VSTS Git hesap deposu data factory ilişkilendirebilirsiniz. Tek bir VSTS Git hesap birden çok depoları olabilir, ancak bir VSTS Git deposu yalnızca bir data factory ile ilişkili olabilir. VSTS hesabı veya depo yoksa izleyin [bu yönergeleri](https://docs.microsoft.com/vsts/accounts/create-account-msa-or-work-student) kaynaklarınızı oluşturmak için.

> [!NOTE]
> VSTS GIT deposunda betiğini ve veri dosyalarını depolayabilirsiniz. Bununla birlikte, dosyaları el ile Azure depolama alanına yüklemek zorunda. Data Factory işlem hattı VSTS GIT deposu Azure Storage depolanan komut dosyası veya veri dosyaları otomatik olarak karşıya değil.

### <a name="configure-a-vsts-git-repository-with-azure-data-factory"></a>Azure Data Factory ile VSTS Git deposu yapılandırma
VSTS GIT deposu iki yöntem data factory ile yapılandırabilirsiniz.

#### <a name="method1"></a> Yapılandırma yöntem 1: başlangıç sayfasına şimdi al

Azure Data Factory'de Git **başlayalım** sayfası. Seçin **kod deposu yapılandırın**:

![VSTS kod deposu yapılandırın](media/author-visually/configure-repo.png)

**Deposu ayarları** yapılandırma bölmesinde görünür:

![Kod deposu ayarlarını yapılandırma](media/author-visually/repo-settings.png)

Bölmesinde aşağıdaki VSTS kod deposu ayarlarını gösterir:

| Ayar | Açıklama | Değer |
|:--- |:--- |:--- |
| **Depo türü** | VSTS kod depo türü.<br/>**Not**: GitHub şu anda desteklenmiyor. | Visual Studio Team Services Git |
| **Azure Active Directory** | Azure AD Kiracı adınız. | <your tenant name> |
| **Visual Studio Team Services hesabı** | VSTS hesap adınızı. VSTS hesap adınızı bulabilir `https://{account name}.visualstudio.com`. Yapabilecekleriniz [VSTS hesabınızda oturum açın](https://www.visualstudio.com/team-services/git/) Visual Studio profilinizi erişip depoları ve proje bakın. | \<hesap adınız > |
| **ProjectName** | VSTS projenizin adına. VSTS proje adınızı bulabilir `https://{account name}.visualstudio.com/{project name}`. | \<VSTS proje adı > |
| **RepositoryName** | VSTS kod deposu adınız. VSTS projeleri projenizi büyüdükçe, kaynak kodunuzu yönetmek için Git depoları içerir. Yeni bir havuz oluşturabilir veya projenizde zaten varolan bir havuz kullanabilirsiniz. | \<VSTS kod depo adı > |
| **İşbirliği şube** | Yayımlama için kullanılacak VSTS işbirliği dalı. Varsayılan olarak bu değer `master`. Başka bir şube kaynaklardan yayımlamak istediğiniz durumlarda bu değiştirin. | \<İşbirliği şube adı > |
| **Kök klasör** | Kök klasörünüzde VSTS işbirliği dalı. | \<kök klasör adı > |
| **Mevcut Veri Fabrikası Kaynakları depoya Al** | Mevcut Veri Fabrikası Kaynakları UX'dan alma belirtir **tuvale yazma** VSTS Git deposu içine. Veri Fabrikası Kaynakları ilişkili Git deposu JSON biçiminde aktarmak için kutuyu işaretleyin. Bu eylem her bir kaynağın ayrı ayrı verir (diğer bir deyişle, veri kümeleri ve bağlantılı Hizmetleri ayrı JSONs verilir). Bu kutu işaretli değilse, varolan kaynakları içe aktarılmaz. | Seçili (varsayılan) |

#### <a name="configuration-method-2-ux-authoring-canvas"></a>Yapılandırma yöntem 2: UX tuvale yazma
Azure veri fabrikası UX içinde **tuvale yazma**, veri fabrikası bulun. Seçin **Data Factory** açılır menü ve ardından **yapılandırma kodu depo**.

Bir yapılandırma bölmesinde görünür. Açıklamasında yapılandırma ayarları hakkında daha fazla bilgi için bkz <a href="#method1">yapılandırma yöntemi 1</a>.

![UX yazma kodu deposu ayarlarını yapılandırma](media/author-visually/configure-repo-2.png)

#### <a name="switch-to-a-different-git-repo"></a>Farklı bir Git deposuna geçin

Farklı bir Git deposuna geçiş yapmak için aşağıdaki ekran görüntüsünde gösterildiği gibi veri fabrikası genel bakış sayfasında, sağ üst köşesinde simgesini bulun. Simge göremiyorsanız, yerel tarayıcı önbelleğini temizleyebilirsiniz. Geçerli depoyu ilişkilendirmesini kaldırmak için simgeyi seçin.

Geçerli depoyu ilişkilendirmesini kaldırmanız sonra Git ayarlarınızı farklı depoyu kullanacak şekilde yapılandırabilirsiniz. Ardından, var olan Veri Fabrikası Kaynakları yeni deposuna içe aktarabilirsiniz.

![Geçerli Git deposuna ilişkilendirmesini kaldırın.](media/author-visually/remove-repo.png)

### <a name="use-version-control"></a>Sürüm denetimini kullanma
Sürüm denetim sistemleri (olarak da bilinen _kaynak denetimi_) koda temel yapılan kod ve izleme değişiklikleri üzerinde işbirliği geliştiriciler olanak tanır. Kaynak denetimi çok geliştirici projeleri için önemli bir araçtır.

Data factory ile ilişkili her VSTS Git deposu sahip bir işbirliği dal. (`master` varsayılan işbirliği dalının). Kullanıcılar da oluşturabilir özelliği dalları tıklayarak **+ dalı** ve özellik dalları geliştirme yapın.

![Eşitleniyor veya yayımlama kodunu değiştirme](media/author-visually/sync-publish.png)

Özellik dalında ile özelliği development hazır olduğunuzda tıklayabilirsiniz **oluşturma çekme isteği**. Bu sürecek VSTS nerede yükseltmeniz çekme GIT istekleri kod gözden geçirme ve birleştirme işbirliği dalınızdaki değiştirir. (`master` varsayılandır). Ancak veri fabrikası hizmetine işbirliği dalınızdan yayımlama izin verilir. 

![Yeni bir çekme isteği oluşturun](media/author-visually/create-pull-request.png)

#### <a name="publish-code-changes"></a>Kod değişikliklerini yayımlama
İşbirliği şube değişiklikler birleştirilmiş sonra (`master` varsayılandır), select **Yayımla** kod değişiklikleri ana dala Data Factory hizmetine el ile yayımlamak için.

![Data Factory hizmetinin değişiklikleri yayımlama](media/author-visually/publish-changes.png)

> [!IMPORTANT]
> Ana dala Data Factory hizmeti dağıtılan temsili değildir. Ana dala *gerekir* yayımlanacağı el ile veri fabrikası hizmetine.

## <a name="use-the-expression-language"></a>İfade dili kullanın
Azure Data Factory ile desteklenen ifade dili kullanarak ifadeler için özellik değerlerini belirtebilirsiniz. 

Özellik değerleri için ifadeleri belirleyerek **dinamik içerik Ekle**:

![İfade dili kullanın](media/author-visually/dynamic-content-1.png)

## <a name="use-functions-and-parameters"></a>İşlevler kullanın ve parametreleri

İşlevler kullanın ya da veri fabrikasında ardışık düzen ve veri kümeleri için parametre belirtin **ifade oluşturucusu**:

Desteklenen ifadeler hakkında daha fazla bilgi için bkz: [ifadeleri ve Azure Data Factory işlevleri](control-flow-expression-language-functions.md).

![Dinamik içerik ekleme](media/author-visually/dynamic-content-2.png)

## <a name="provide-feedback"></a>Geri bildirimde bulunma
Seçin **geri bildirim** özellikleri hakkında yorum yapmak veya Microsoft aracı ile ilgili sorunları bildirmek için:

![Geri Bildirim](media/author-visually/provide-feedback.png)

## <a name="next-steps"></a>Sonraki adımlar
Ve hakkında daha fazla izleme ardışık düzen yönetme öğrenmek için bkz [İzleyici ve ardışık düzen programlama yoluyla yönetilmesi](monitor-programmatically.md).
