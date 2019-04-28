---
title: Team Data Science Process roller ve görevler
description: Anahat anahtar bileşenleri, personel rolleri ve ilişkili görevleri bir veri bilimi takım projesi için.
author: marktab
manager: cgronlun
editor: cgronlun
services: machine-learning
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 09/04/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 05fc742bba535ea3968e60cd0f40c80b812c09fd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61043079"
---
# <a name="team-data-science-process-roles-and-tasks"></a>Team Data Science Process roller ve görevler

Team Data Science Process, Tahmine dayalı analiz çözümlerini ve akıllı uygulamaları etkili bir şekilde oluşturmak için yapılandırılmış bir Metodoloji sağlayan Microsoft tarafından geliştirilen bir çerçevedir. Bu makalede anahtar personel rolleri özetlemekte ve bu işlemle ilgili Standartlaştırma veri bilimi tarafından işlenen ilişkilendirilen görevlerinin takım.

Tüm veri bilimi grubu, veri bilimi ekipleri ve projeleri için TDSP ortamı ayarlama konusunda yönergeler sağlayan öğreticiler bu giriş bağlar.
Bu öğreticiler, Azure DevOps kullanma hakkında ayrıntılı kılavuz sağlar. Azure DevOps, platform kodu barındırma ve depoları Yönet ekip görevlerini yönetmek ve erişimi denetlemek için Çevik planlama aracı sağlar.

Kendi kod barındırma ve Çevik planlama aracı TDSP uygulamak için bu bilgileri kullanabilirsiniz.

## <a name="structures-of-data-science-groups-and-teams"></a>Veri bilimi grupları ve takım yapıları

Veri bilimi işlevleri kuruluşlarda, genellikle aşağıdaki hiyerarşi içinde düzenlenebilir:

1. ***Veri bilimi grubu/sn***

2. ***Veri bilimi takım/sn içinde grup/sn***

Böyle bir yapıda, Grup ve takım liderleri vardır. Genellikle bir veri bilimi proje proje liderleri (için proje yönetimi ve İdaresi görevleri) ve veri uzmanları veya mühendisleri oluşan bir veri bilimi ekip tarafından yapılır (bireysel katkıda bulunanlar veya teknik personel) kullanan veri bilimi yürütülecek ve projenin parçaları mühendislik verileri. Yürütme öncesinde, Kurulum ve idare gerçekleştirilir grubu, takım veya proje liderleri tarafından.

## <a name="definition-of-four-tdsp-roles"></a>Dört TDSP rol tanımı
Yukarıdaki varsayımıyla, takım personel için dört ayrı roller vardır:

1. ***Grup Yöneticisi***. Grup Yöneticisi, bir kuruluştaki tüm veri bilimi biriminin yöneticisidir. Bir veri bilimi birimi, birden çok takıma, her biri ayrı iş sıralamasına koydunuz birden çok veri bilimi proje üzerinde çalışıyor olabilir. Grup Yöneticisi görevlerini bir yedek için temsilci, ancak rol ile ilişkili görevleri değiştirmeyin.

2. ***Ekip Lideri***. Ekip Lideri, kurumsal bir veri bilimi birimindeki ekibi yönetiyor. Takım birden çok veri bilimcileri oluşur. Yalnızca az sayıda veri uzmanları ile veri bilimi birimi için Grup Yöneticisi ve ekibine Liderlikte aynı kişiye olabilir.

3. ***Proje lideri***. Bir proje lideri, belirli bir veri bilimi proje üzerinde tek tek veri bilimcileri günlük etkinliklerini yönetir.

4. ***Bireysel katılımcı proje***. Veri uzmanı, iş analisti, veri mühendisi, Mimarı, vs. Tek tek projeye katkıda bulunan bir veri bilimi proje yürütür.


> [!NOTE]
> Kurumsal bir yapıda, bağlı olarak birden fazla rol tek bir kişi oynayabilir veya bir rolde çalışan birden fazla kişi olması olabilir. Sık, küçük işletmelerin ya da kuruluşların personel, veri bilimi kuruluş içindeki az sayıda durum da olabilir.

## <a name="tasks-to-be-completed-by-four-personnel"></a>Dört personeli tarafından tamamlanacak görevler

Aşağıdaki resimde, benimseme ve Microsoft tarafından conceptualized olarak Team Data Science Process uygulama rolü tarafından personel için üst düzey görevler gösterilmektedir.

![Rolleri ve görevleri genel bakış](./media/roles-tasks/overview-tdsp-top-level.png)

Bu şema ve her TDSP rolüne atanmış olan görevleri aşağıdaki, ayrıntılı anahattını sizin Sorumluluklarınız kuruluştaki göre uygun eğitmene seçmenize yardımcı olmalıdır.

> [!NOTE]
> Aşağıdaki yönergeler, nasıl bir TDSP ortamı ayarlayın ve diğer Azure DevOps veri bilimi görevleri tamamlamak adımları göstermektedir. Biz ne Microsoft'ta TDSP uygulamak için kullanıyoruz olduğu için Azure DevOps ile bu görevleri gerçekleştirmek üzere nasıl belirtin. Azure DevOps, yönetim görevlerini izleme iş öğelerinin tümleştirerek işbirliği yapılmasını kolaylaştırır ve yardımcı programlar, paylaşmak için kullanılan bir kod barındırma hizmeti sürümleri düzenlemek ve rol tabanlı güvenlik sağlar. TDSP tarafından özetlenen görevleri uygulamak için tercih ederseniz, diğer platformları seçebilirsiniz. Ancak, platforma bağlı olarak, Azure DevOps ' yararlanılarak bazı özellikleri kullanılamayabilir.
>
>Buradaki yönergeleri de [veri bilimi sanal makinesi (DSVM)](https://aka.ms/dsvm) Azure bulut analizi Masaüstü gibi çeşitli Microsoft yazılım ve Azure ile tümleşik ve önceden yapılandırılmış çeşitli popüler veri bilimi araçları ile Hizmetler. TDSP uygulamak için DSVM veya herhangi bir geliştirme ortamı'nı kullanabilirsiniz.


## <a name="group-manager-tasks"></a>Grup yöneticisi görevleri

Aşağıdaki görevler, Grup Yöneticisi (veya belirlenen TDSP Sistem Yöneticisi) TDSP benimsemeye tamamlanır:

- Oluşturma bir **grup hesabı** barındırma Platformu (GitHub, Git, Azure DevOps ve diğerleri gibi) bir kod üzerinde
- Oluşturma bir **proje şablonu depo** grup hesabı ve bu proje şablonu deposundan Microsoft TDSP ekibi tarafından geliştirilen çekirdek. Microsoft gelen TDSP projesi şablon deposu
    - sağlar bir **standartlaştırılmış dizin yapısı** dizinleri veri, kodu ve belgeler için de dahil olmak üzere
    - takımına **standart şablonların** bir verimli veri bilimi işlemi rehberlik edecek.
- Oluşturma bir **yardımcı programı depo**ve Microsoft TDSP ekibiniz tarafından geliştirilen yardımcı programı deposundan temel. Microsoft TDSP yardımcı programı depodan sağlar
    - bir veri Bilimcisi çalışması daha verimli de dahil olmak üzere yardımcı programları modelleme ve raporlama temel yanı sıra, etkileşimli veri keşfi, analiz ve raporlama yapmak için kullanışlı araçlar kümesi.
- Ayarlanan **güvenlik denetimi İlkesi** grubu hesabınızda bu iki depolar.

Ayrıntılı adım adım yönergeler için bkz: [grup yöneticisi görevleri için bir veri bilimi ekip](group-manager-tasks.md).


## <a name="team-lead-tasks"></a>Ekip Lideri görevleri

Ekibine Liderlikte (veya atanan proje yöneticisi) TDSP benimsemek için aşağıdaki görevler tamamlanır:

- Azure DevOps sürüm oluşturma ve işbirliği için kod ana bilgisayar platformu olarak seçilir, oluşturun bir **proje** grubun Azure DevOps Hizmetleri. Aksi takdirde, bu görevi atlanabilir.
- Oluşturma **proje şablonu depo** proje ve grup proje şablonu deposundan ayarlamanız, grup yöneticisi veya yönetici temsilcisi tarafından çekirdek altında.
- Oluşturma **takım yardımcı programı depo**, ekibe özgü yardımcı programları depoya ekleyin.
- (İsteğe bağlı) Oluşturma **[Azure dosya depolama](https://azure.microsoft.com/services/storage/files/)** takımın tamamı için yararlı olabilecek veri varlıkları depolamak için kullanılacak. Diğer takım üyeleri bu paylaşılan bulut dosya depolama, analiz Masaüstü bağlayabilirsiniz.
- (İsteğe bağlı) Azure dosya depolama bağlama **veri bilimi sanal makinesi** (DSVM) takım sağlama ve veri varlıklarını ekleyin.
- Ayarlanan **güvenlik denetimi** göre takım üyeleri ekleme ve kendi ayrıcalıklarını yapılandırın.

Ayrıntılı adım adım yönergeler için bkz: [ekibine Liderlikte görev bir veri bilimi takım için](team-lead-tasks.md).


## <a name="project-lead-tasks"></a>Proje lideri görevleri

Proje TDSP benimsemek için müşteri adayı aşağıdaki görevler tamamlanır:

- Oluşturma bir **projesinin deposuna** projesi altındaki ve proje şablonu depodan temel.
- (İsteğe bağlı) Oluşturma **Azure dosya depolama** projenin veri varlıkları depolamak için kullanılacak.
- (İsteğe bağlı) Azure dosya depolama bağlama **veri bilimi sanal makinesi** (DSVM) projenin sağlama ve proje veri varlıklarını ekleyin.
- Ayarlanan **güvenlik denetimi** ile proje üyeleri ekleme ve kendi ayrıcalıklarını yapılandırın.

Ayrıntılı adım adım yönergeler için bkz: [bir veri bilimi takım projesi neden görevlerde](project-lead-tasks.md).

## <a name="project-individual-contributor-tasks"></a>Proje bireysel katılımcı görevleri

Proje bireysel TDSP kullanan veri bilimi proje yürütmek için katkıda bulunan (genellikle bir veri Bilimcisi) aşağıdaki görevler tamamlanır:

- Kopya **projesinin deposuna** proje lideri ayarlayın.
- (İsteğe bağlı) Paylaşılan bağlama **Azure dosya depolama** takımın ve projedeki kendi **veri bilimi sanal makinesi** (DSVM).
- Proje yürütülemiyor.


Kolaylaşmasına üzerine bir proje için ayrıntılı adım adım yönergeler için bkz: [bir veri bilimi takım için proje bağımsız katılımcıları](project-ic-tasks.md).


## <a name="data-science-project-execution"></a>Veri bilimi proje yürütme

Uygun kümesi yönergeleri takip ederek, veri uzmanları, proje lideri ve takım liderleri, tüm görevler ve bir proje gereken aşamaları, baştan kendi sonuna izlemek için iş öğeleri oluşturabilirsiniz. Git kullanarak da veri uzmanları arasında işbirliğini teşvik eder ve sürüm denetimli ve tüm proje üyeleri tarafından paylaşılan proje yürütme sırasında oluşturulan yapıtların olmasını sağlar.

Proje yürütme için sağlanan yönergeleri hem de iş öğeleri ve git depoları, Azure DevOps üzerinde olan proje varsayımına dayanarak geliştirilmiştir. Azure DevOps için her ikisini de kullanarak proje depolarınızın Git dallarıyla çalışma öğelerinizi bağlamanıza olanak sağlar. Bu şekilde, bir iş öğesi için yapılan kolayca izleyebilirsiniz.

Aşağıdaki şekilde TDSP kullanarak proje yürütme için bu iş akışını açıklar.

![Tipik veri bilimi proje yürütme](./media/roles-tasks/overview-project-execute.png)

İş akışı, üç etkinliklerine gruplandırılabilir adımları içerir:

- Sprint planlama (Proje sağlama)
- Yapıtları git dallanma iş öğeleri (veri bilimi insanı) yönelik geliştirme
- Kod gözden geçirme ve dalları (Proje sağlama veya diğer ekip üyeleri) ana dal ile birleştirme

Proje yürütme iş akışı hakkında ayrıntılı adım adım yönergeler için bkz. [veri bilimi projeleri yürütülmesini](project-execution.md).

## <a name="project-structure"></a>Proje yapısı

Bunu kullanın [proje şablonu depo](https://github.com/Azure/Azure-TDSP-ProjectTemplate) verimli proje yürütme ve işbirliğini desteklemek için. Bu depo, kendi TDSP proje için kullanabileceğiniz bir standartlaştırılmış dizin yapısını ve belge şablonları sunar.

## <a name="next-steps"></a>Sonraki adımlar

Team Data Science Process tarafından tanımlanan görevleri ve rolleri daha detaylı açıklamalar'ı keşfedin:

- [Bir veri bilimi takım için Grup yöneticisi görevleri](group-manager-tasks.md)
- [Bir veri bilimi takım için takım sağlama görevleri](team-lead-tasks.md)
- [Proje için bir veri bilimi ekibi müşteri adayı görevleri](project-lead-tasks.md)
- [Bir veri bilimi takım için proje bağımsız katılımcıları](project-ic-tasks.md)
