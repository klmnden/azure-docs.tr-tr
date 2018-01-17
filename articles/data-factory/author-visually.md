---
title: "Görsel olarak Azure data factory'leri Yazar | Microsoft Docs"
description: "Görsel olarak Azure Data Factory Yazar öğrenin"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/9/2018
ms.author: shlo
ms.openlocfilehash: 3e67665facba78c4ca8e2317f0323b4c5c02a49c
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="visually-author-data-factories"></a>Görsel olarak yazar veri fabrikaları
Azure veri fabrikası UX deneyimiyle kullanıcılar görsel olarak yazar ve tek satırlık bir kod yazmak zorunda kalmadan kendi veri fabrikası kaynaklarında dağıtın. Bu Kodsuz arabirimi sürükleyin ve bir ardışık düzen tuvalde etkinlikleri bırakın, test çalışmalarını gerçekleştirmek, tekrarlayarak, hata ayıklama ve dağıtma ve ardışık düzen çalışmalarınız izleme sağlar. İki yolla ADF UX Aracı'nı kullanmayı da tercih edebilirsiniz:

1. Doğrudan Data Factory hizmeti ile çalışma
2. VSTS Git tümleştirmesi, işbirliği, kaynak denetimi ya da sürüm oluşturma için yapılandırın

## <a name="authoring-with-data-factory"></a>Data Factory ile yazma
İlk seçenek, doğrudan veri fabrikası moduyla yazma. Bu yaklaşım, değişikliklerinizi JSON varlıklarının depolama depo olmasını bakımından VSTS kod deposu yazma farklıdır ve işbirliği veya sürüm denetimi için optimize edilmiştir.

![Veri Fabrikası yapılandırın](media/author-visually/configure-data-factory.png)

Data Factory modunda, yalnızca 'Yayımla' modunda yoktur. Yaptığınız değişiklikler veri fabrikası hizmetine doğrudan yayımlanır.

![Veri Fabrikası yayımlama](media/author-visually/data-factory-publish.png)

## <a name="authoring-with-vsts-git-integration"></a>VSTS Git Tümleştirmesi ile yazma
Kaynak denetimi ve veri fabrikası hatlarınızı geliştirme sırasında işbirliği için VSTS Git Tümleştirmesi ile geliştirme sağlar. Kullanıcıların bir veri fabrikası kaynak denetimi, işbirliği ve sürüm oluşturma vb. için VSTS Git hesap depo ilişkilendirmek için seçeneğiniz vardır. Tek bir VSTS GIT hesap birden çok depoları olabilir. Ancak, bir VSTS Git deposu yalnızca bir tek veri fabrikası ile ilişkili olabilir. VSTS hesabı ve depo zaten yoksa, oluşturmak [burada](https://docs.microsoft.com/en-us/vsts/accounts/create-account-msa-or-work-student).

### <a name="configure-vsts-git-repo-with-azure-data-factory"></a>VSTS Git deposuna Azure Data Factory ile yapılandırma
Kullanıcılar, iki yöntem data factory ile VSTS GIT deposuna yapılandırabilir.

#### <a name="method-1-lets-get-started-page"></a>Yöntem 1: 'başlayalım' sayfası

'Başlayalım' sayfasına gidin ve 'Kod havuzu Yapılandır' öğesini tıklatın

![Kod deposu yapılandırın](media/author-visually/configure-repo.png)

Buradan, havuz ayarlarının yapılandırılması için bir yan panel görüntülenir.

![Depo ayarlarını yapılandırma](media/author-visually/repo-settings.png)
* **Depo türü**: Visual Studio Team Services Git (şu anda, Github desteklenmiyor.)
* **Visual Studio Team Services hesabı**: hesap adı https://{account adından bulunabilir}. visualstudio.com. VSTS hesabınızda oturum açın [burada](https://www.visualstudio.com/team-services/git/) ve depoları ve projeleri görmek üzere Visual Studio profilinizi erişim
* **ProjectName:** proje adı https://{account name}.visualstudio.com/{project adından bulunabilir}
* **RepositoryName:** depo adı. VSTS projeleri projenizi büyüdükçe, kaynak kodunuzu yönetmek için Git depoları içerir. Yeni bir havuz oluşturabilir veya varolan bir depo zaten projedeki kullanabilirsiniz.
* **Mevcut Veri Fabrikası Kaynakları depoya Al**: Bu kutuyu işaretleyerek, JSON biçiminde ilişkili VSTS GIT deposuna UX tuvalde yazılan, geçerli Veri Fabrikası Kaynakları içeri aktarabilirsiniz. Bu eylem her bir kaynağın ayrı ayrı verir (diğer bir deyişle, bağlı hizmetler ve veri kümeleri ayrı JSONs verilir).    Bu onay kutusunu temizlerseniz, varolan kaynakları Git depoya içeri aktarılmadı.

#### <a name="method-2-from-authoring-canvas"></a>Yöntem 2: Tuvale yazma

'Yazma tuvali', 'Veri fabrikası' açılan menüsünün altında veri fabrikası adı tıklatın. 'Kod deposu yapılandırın.' ı Benzer şekilde **yöntemi 1**, havuz ayarlarını yapılandırmak için bir yan panel görüntülenir. Önceki bölümlerde ayarları hakkında bilgi için bkz.

![Kod deposu 2 yapılandırın](media/author-visually/configure-repo-2.png)

### <a name="version-control"></a>Sürüm denetimi
Sürüm denetimi, ya da için kaynak denetimi sistemleri kod için temel kod ve izleme değişikliklerinin üzerinde işbirliği yapmak, geliştiricilerin olanak sağlar. Kaynak denetimi çok geliştirici projeleri için önemli bir araçtır.

Bir kez data factory ile ilişkili her VSTS Git deposu ana dala sahiptir. Buradan, değişiklik yaparken VSTS Git deposuna erişimi olan her kullanıcının iki seçenek vardır: eşitleme ve yayımlayın.

![Eşitleme yayımlama](media/author-visually/sync-publish.png)

#### <a name="sync"></a>Sync

'Sync' tıkladığınızda, değişiklikleri ana dala yerel dalı için çekme veya değişiklikleri yerel dalınızdan ana dala gönderin.

![Değişiklikler eşitleniyor](media/author-visually/sync-change.png)

#### <a name="publish"></a>Yayımlama
 Veri Fabrikası hizmetine ana dal değişiklikleri yayımlayın.

> [!NOTE]
> **Ana dala Data Factory hizmeti dağıtılan temsili değildir.** Ana dala *gerekir* yayımlanacağı el ile veri fabrikası hizmetine.




## <a name="expression-language"></a>İfade Dili

Kullanıcılar, Azure Data Factory ile desteklenen ifade dili kullanarak özellik değerlerini belirlerken ifadeleri belirtebilirsiniz. Bkz: [ifadeleri ve Azure Data Factory işlevleri](control-flow-expression-language-functions.md) için daha hakkında ifadeleri desteklenir.

İfadeler UX özellik değerlerinde belirtin sözlüğüdür.

![İfade Dili](media/author-visually/expression-language.png)

## <a name="parameters"></a>Parametreler
Kullanıcıların parametreleri ardışık düzen ve veri kümeleri için 'Parameters' sekmesinde belirtebilirsiniz. Ayrıca, parametreleri özelliklerinde kolayca "Dinamik içeriği ekleyin." tuşlarına basarak kullanma

![Dinamik içerik](media/author-visually/dynamic-content.png)

Buradan, var olan bir parametre kullanma veya yeni bir parametre, özellik değeri belirtin.

![Parametreler](media/author-visually/parameters.png)

## <a name="feedback"></a>Geri Bildirim
Bize çeşitli özellikleri veya karşılıklı herhangi bir sorunla (Microsoft) görüşlerinizi bildirmek için 'Geri' simgesine tıklayın.

![Geri Bildirim](media/monitor-visually/feedback.png)

## <a name="next-steps"></a>Sonraki adımlar

İzleme ve ardışık düzen yönetme hakkında bilgi edinmek için bkz: [İzleyici ve ardışık düzen programlı olarak yönetmek](monitor-programmatically.md) makale
