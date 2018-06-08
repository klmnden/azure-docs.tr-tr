---
title: Ekip veri bilimi işlemi roller ve görevler - Azure | Microsoft Docs
description: Anahat anahtar bileşenleri, personel rolleri ve veri bilimi takım projesi için ilişkili görevleri.
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: deguhath
ms.openlocfilehash: 275c1d1dad9030da776900c4e2b97152c8d2d581
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34839141"
---
# <a name="team-data-science-process-roles-and-tasks"></a>Takım veri bilimi işlemi roller ve görevler

Takım veri bilimi işlemi Tahmine dayalı analiz çözümleri ve akıllı uygulamaları verimli bir şekilde oluşturmak için yapılandırılmış bir Metodoloji sağlayan Microsoft tarafından geliştirilen bir çerçevedir. Bu makalede anahtar personel rolleri özetler ve bu işlemi Standartlaştırma veri bilimi tarafından işlenen ilişkilendirilen görevlerinin ekip. 

Tüm veri bilimi grubu, veri bilimi ekipleri ve projeleri TDSP ortamını ayarlama konusunda yönergeler sağlamak öğreticileri bu giriş bağlantılar. Visual Studio Team Services (VSTS) ekip görevleri yönetmek, erişimi denetlemek ve depoları yönetmek için bizim platform kodu barındırma ve Çevik planlama aracı eğitimlerine kullanma hakkında ayrıntılı kılavuz sunuyoruz. 

Kendi kod barındırma ve Çevik planlama aracı TDSP uygulamak için bu bilgileri kullanmak mümkün olacaktır. 

## <a name="structures-of-data-science-groups-and-teams"></a>Veri bilimi grupları ve takımlar yapıları
Kuruluşlar, veri bilimi işlevleri genellikle aşağıdaki hiyerarşi içinde düzenlenebilir:

1. ***Veri bilimi Grup/s***

2. ***Veri bilimi takım/s Grup/s içinde***

Bu tür bir yapı Grup bulunur ve müşteri adayları ekip. Genellikle, bir veri bilimi Proje Proje müşteri adayları (proje yönetimi ve idare görevler) ve veri bilimcilerine veya mühendisleri oluşan bir veri bilimi ekibi tarafından yapılır (tek tek katkıda bulunanlar / teknik personel) kullanan veri bilimi yürütülecek ve proje bölümlerini mühendislik veri. Yürütme önce Kurulum ve idare grup, ekip veya proje müşteri adayları tarafından gerçekleştirilir.

## <a name="definition-of-four-tdsp-roles"></a>Dört TDSP rol tanımı
Yukarıdaki varsayımıyla, biz takım personelimizin için dört farklı rolleri belirttiğiniz:

1. ***Grup Yöneticisi***. Grup Yöneticisi, bir kuruluştaki tüm veri bilimi biriminin yöneticisidir. Bir veri bilimi birimi, her biri ayrı iş verticals birden çok veri bilimi proje üzerinde çalıştığı birden çok ekibin olabilir. Grup Yöneticisi görevlerini bir yedek için temsilci, ancak rol ile ilişkili görevleri değiştirmeyin.

2. ***Ekip sağlama***. Bir takım lideri kuruluş veri bilimi biriminde ekibi yönetiyor. Bir takım birden çok veri bilimcilerine oluşur. Az sayıda veri bilimcilerine ile veri bilimi birimi için aynı kişi Grup Yöneticisi'ni ve Team neden olabilir.

3. ***Proje sağlama***. Proje lideri ayrı veri bilimcilerine belirli veri bilimi projedeki günlük etkinliklerini yönetir.

4. ***Proje bağımsız katılımcı***. Veri Bilimcisi, iş analisti, veri mühendislik, Mimarı, vs. Proje tek tek katkıda bulunan bir veri bilimi proje yürütür. 


**[AZURE.NOTE]**: Bağlı olarak bir kuruluş yapısında, tek bir kişinin birden fazla rol oynayabilir veya bir rol üzerinde çalışan birden fazla kişi olabilir. Bu durumda küçük şirketler veya kuruluşlar küçük sayıda veri bilimi kuruluşlarındaki personeli ile sık olabilir.

## <a name="tasks-to-be-completed-by-four-personnel"></a>Dört personeli tarafından tamamlanması gereken görevler

Aşağıdaki resimde, benimsenmesi ve Microsoft tarafından conceptualized olarak ekip veri bilimi işlemi uygulama rol tarafından personeli için üst düzey görevler gösterilmektedir. 

![Rolleri ve görevleri genel bakış](./media/roles-tasks/overview-tdsp-top-level.png)

Bu şemayı ve TDSP her role atanmış olan görevleri aşağıdaki, daha ayrıntılı özetini sorumluluklarınızın kuruluştaki göre uygun öğretici seçmenize yardımcı olmalıdır.

>[AZURE.NOTE] Aşağıdaki yönergelerde yer TDSP ortamı kurma ve diğer veri bilimi Visual Studio Team Services (VSTS) içinde görevlerinin yönelik adımları gösterir. Biz, ne TDSP Microsoft'taki uygulamak için kullanıyoruz olduğundan bu görevlerin VSTS ile nasıl gerçekleştirileceğini belirtin. Yönetim görevlerini izleme çalışma öğeleri ile tümleştirerek işbirliği VSTS kolaylaştırır ve yardımcı programları paylaşmak için kullanılan bir kod barındırma hizmeti sürümleri düzenlemek ve rol tabanlı güvenlik sağlar. Diğer platformlar tarafından TDSP özetlenen görevleri uygulamak için tercih ederseniz seçebilir. Ancak, platform bağlı olarak, VSTS nden bazı özellikler kullanılamayabilir. 
>
>Ayrıca kullanırız [veri bilimi sanal makine (DSVM)](http://aka.ms/dsvm) Azure üzerinde önceden yapılandırılmış ve çeşitli Microsoft yazılım ve Azure Hizmetleri ile tümleşik çeşitli popüler veri bilimi araçlarıyla analytics Masaüstü gibi bulut. TDSP uygulamak için DSVM veya başka bir geliştirme ortamı'nı kullanabilirsiniz. 


## <a name="group-manager-tasks"></a>Grup yöneticisi görevleri

Aşağıdaki görevler, Grup Yöneticisi'ni (veya belirlenen TDSP Sistem Yöneticisi tarafından) TDSP benimsemeyi tamamlanır:

- Oluşturma bir **grup hesabı** barındırma Platformu (örneğin, Github, Git, VSTS veya diğerleri) kodu
- Oluşturma bir **proje şablonu deposu** grubu hesabı ve bu proje şablonu depodan Microsoft TDSP ekibi tarafından geliştirilen çekirdek. Microsoft TDSP proje şablonu depodan sağlar bir **standartlaştırılmış dizin yapısını** bir dizi sağlar ve verileri, kodu ve belgeler için dizinler dahil **standartlaştırılmışBelgeşablonları** verimli veri bilimi işlem kılavuzluk edecek. 
- Oluşturma bir **yardımcı programı depo**ve Microsoft TDSP ekibiniz tarafından geliştirilen yardımcı programı depodan çekirdek. Microsoft TDSP yardımcı programı depodan veri Bilimcisi iş daha verimli, etkileşimli veri keşfi, analiz ve raporlama için ve modelleme ve raporlama taban çizgisi için yardımcı programlar da dahil olmak üzere hale getirmek için faydalı araçlar kümesi sağlar.
- Ayarlanan **güvenlik denetimi İlkesi** , bu iki depolarının Grup hesabınızda.  

Ayrıntılı adım adım yönergeler için bkz: [grup yöneticisi görevleri için bir veri bilimi ekibi](group-manager-tasks.md). 


## <a name="team-lead-tasks"></a>Ekip sağlama görevleri

Aşağıdaki görevler, ekip lideri (veya belirlenen takım projesi yöneticinize) TDSP benimsemeyi tamamlanır:

- VSTS sürüm oluşturma ve işbirliği için kod ana bilgisayar platformu olarak seçtiğiniz oluşturmazsanız, bir **takım projesi** grubun VSTS sunucusunda. Aksi takdirde, bu görev atlanabilir.
- Oluşturma **takım projesi şablonu deposu** takım projesi ve grup proje şablonu depodan ayarlayın, grup yöneticisi veya yönetici temsilcisi tarafından çekirdek altında. 
- Oluşturma **takım yardımcı programı depo**, takım özgü yardımcı programları depoya ekleyin. 
- (İsteğe bağlı) Oluşturma **[Azure dosya depolama](https://azure.microsoft.com/services/storage/files/)** tüm ekip için yararlı olabilecek veri varlıkları depolamak için kullanılacak. Diğer ekip üyelerinin bu paylaşılan bulut dosya deposu analytics masaüstlerine bağlayabilir.
- (İsteğe bağlı) Azure dosya depolama birimine bağlama **veri bilimi sanal makine** (DSVM) ekibin sağlama ve veri varlıklarını ekleyin.
- Ayarlanan **güvenlik denetimi** göre takım üyeleri ekleme ve kendi ayrıcalıklarını yapılandırın. 

Ayrıntılı adım adım yönergeler için bkz: [veri bilimi ekibi Ekip Lideri görevlerde](team-lead-tasks.md).  


## <a name="project-lead-tasks"></a>Proje sağlama görevleri

Aşağıdaki görevler, proje TDSP benimsemek için sağlama tarafından tamamlanır:

- Oluşturma bir **proje deposu** onu ekibinden takım projesi ve çekirdek altında şablonu deposu proje. 
- (İsteğe bağlı) Oluşturma **Azure dosya depolama** projenin veri varlıkları depolamak için kullanılacak. 
- (İsteğe bağlı) Azure dosya depolama birimine bağlama **veri bilimi sanal makine** (DSVM) projenin sağlama ve proje veri varlıklarının ekleyin.
- Ayarlanan **güvenlik denetimi** göre proje üye ekleme ve kendi ayrıcalıklarını yapılandırın. 

Ayrıntılı adım adım yönergeler için bkz: [veri bilimi takım projesi sağlama görevlerde](project-lead-tasks.md). 

## <a name="project-individual-contributor-tasks"></a>Proje bağımsız katılımcı görevleri

Aşağıdaki görevler, bir proje tek tek TDSP kullanarak veri bilimi proje yürütmek için katkıda bulunanlar tarafından (genellikle veri Bilimcisi) tamamlanır:

- Kopya **proje deposu** tarafından proje lideri ayarlayın. 
- (İsteğe bağlı) Paylaşılan bağlama **Azure dosya depolama** takım projesi üzerinde ve bunların **veri bilimi sanal makine** (DSVM).
- Proje yürütülemiyor. 

 
Ortamınızın bir proje üzerine için ayrıntılı adım adım yönergeler için bkz: [projesine veri bilimi ekibi için tek tek katkıda bulunanlar](project-ic-tasks.md). 


## <a name="data-science-project-execution"></a>Veri bilimi proje yürütme
 
İlgili ayarlama yönergeleri izleyerek, veri bilimcileri, proje lideri ve ekip müşteri adayları, tüm görevleri ve bir projenin ihtiyaç duyduğu aşamaları kendi baştan kendi sonuna izlemek için iş öğeleri oluşturabilirsiniz. Git kullanarak da veri bilimcilerine arasında işbirliği yükseltir ve proje yürütme sırasında oluşturulan yapıları denetlenen ve tüm proje üyeleri tarafından paylaşılan sürüm olmasını sağlar.

Proje yürütme için sağlanan yönergeleri hem de iş öğeleri ve git depoları üzerinde VSTS olan proje varsayımına temelinde geliştirilmiştir. VSTS her ikisi için kullanarak, iş öğelerini proje depoları Git dalları ile bağlantı sağlar. Bu şekilde, bir iş öğesi için yapılan kolayca izleyebilir. 

Aşağıdaki şekilde, bu iş akışı TDSP kullanılarak proje yürütme için özetlenmektedir.

![Genel veri bilimi proje yürütme](./media/roles-tasks/overview-project-execute.png)

İş akışı üç etkinliklerine gruplandırılabilir adımları içerir:

- Sprint (Proje sağlama) planlama
- İş öğeleri (veri Bilimcisi) gidermek için git dallar yapıları geliştirme
- Kod gözden geçirme ve ana dalları (Proje sağlama veya diğer takım üyeleri) dal birleştirme

Proje yürütme iş akışı hakkında ayrıntılı adım adım yönergeler için bkz: [veri bilimi projeleri yürütülmesi](project-execution.md).

## <a name="next-steps"></a>Sonraki adımlar

Burada, rolleri ve görevleri takım veri bilimi işlem tarafından tanımlanan daha ayrıntılı açıklamaları bağlantıları verilmiştir:

- [Bir veri bilimi ekibi için Grup yöneticisi görevleri](group-manager-tasks.md)
- [Bir veri bilimi ekibi için takım sağlama görevleri](team-lead-tasks.md)
- [Proje veri bilimi ekibi için sağlama görevleri](project-lead-tasks.md)
- [Proje veri bilimi ekibi için tek tek Katkıda Bulunanlar](project-ic-tasks.md)