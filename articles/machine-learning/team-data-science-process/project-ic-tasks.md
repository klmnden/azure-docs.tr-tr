---
title: Team Data Science Process içinde tek bir katkıda bulunan görevleri
description: Görev bir veri bilimi takım projesindeki tek bir katkıda bulunan bir ana hat.
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/13/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 6a52907fa6c0e2483479031fbb3d1ad68a121d95
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61043349"
---
# <a name="tasks-for-an-individual-contributor-in-the-team-data-science-process"></a>Team Data Science Process içinde tek bir katkıda bulunan görevleri

Bu konuda anahatları tek bir katkıda görevleri için veri bilimi ekip tamamlanması bekleniyor. Hedefi üzerinde standartlaştırır ekip işbirliği ortamı oluşturmaktır [Team Data Science Process](overview.md) (TDSP). Bu işlemi, standart personel rolleri ve veri bilimi ekibi tarafından işlenen ilişkilendirilen görevlerinin bir özetini görmek [Team Data Science Process rolleri ve görevleri](roles-tasks.md).

Proje bağımsız katılımcıları (TDSP ortamlarını projenin ayarlamayı veri bilimcileri) görevlerini gibi açıklanmamıştır: 

![1](./media/project-ic-tasks/project-ic-1-tdsp-data-scientist.png)

- **GroupUtilities** grubunuz yararlı yardımcı programları grubunun arasında paylaşmak için koruma depodur. 
- **TeamUtilities** takımınız, özellikle takımınız için koruma depodur. 

Bir veri bilimi proje TDSP altında çalıştırmak yönergeler için bkz: [veri bilimi projeleri yürütme](project-execution.md). 

>[AZURE.NOTE] Azure DevOps aşağıdaki yönergeleri kullanarak TDSP takım ortamını ayarlamak için gerekli olan adımları genel çizgileriyle belirtin. Biz, biz Microsoft'ta TDSP nasıl uygulama olduğundan, Azure DevOps ile bu görevleri gerçekleştirmek üzere nasıl belirtin. Grubunuz için başka bir kod barındırma platformu kullanılıyorsa, ekip lideri tarafından genellikle tamamlanması gereken görevler değiştirmeyin. Ancak bu görevleri tamamlamak için yol farklı zordur.


## <a name="repositories-and-directories"></a>Depoları ve dizinler

Bu öğreticide, kısaltılmış depoları ve dizinler için kullanılır. Bu adlar dizinlerini ve depoları işlemleri izlemenizi kolaylaştırır. Bu gösterim (**R** Git depoları ve **D** DSVM'ye yerel dizinler için) aşağıdaki bölümlerde kullanılır:

- **R2**: Azure DevOps grubu sunucunuzda, Grup Yöneticisi ayarlanmış Git GroupUtilities havuzda.
- **R4**: Takımınızın sağlama Git TeamUtilities havuzda ayarlanmış.
- **R5**: Proje deposu, proje lideri tarafından ayarlanmış olan Git üzerinde.
- **D2**: Yerel dizin R2'den kopyalandı.
- **D4**: Yerel dizin R4 kopyalandı.
- **D5**: Yerel dizin R5 kopyalandı.


## <a name="step-0-prerequisites"></a>Adım-0: Önkoşullar

Özetlenen, Grup Yöneticisi atanan görevlerin tamamlayarak önkoşullara uyduğunuzdan [grup yöneticisi görevleri için bir veri bilimi ekip](group-manager-tasks.md). Burada özetlemek gerekirse, aşağıdaki gereksinimleri ekip sağlama görevlerini başlamadan önce karşılanması gerekir: 
- Grup yöneticinizin ayarladığı **GroupUtilities** depo (varsa). 
- Ekip Lideri ayarlanmış **TeamUtilities** depo (varsa).
- Proje deposu, proje lideri ayarladı. 
- Kopyalama kaynağı ve geri proje depoya ayrıcalık ile proje lideri tarafından proje deponuza eklenmiştir.

İkinci **TeamUtilities** depo, önkoşul ekibinizin takım özgü yardımcı programı depo sahip bağlı olarak, isteğe bağlı. Herhangi diğer üç önkoşulları tamamlanmamış olursa, ekip lideri, proje lideri veya yönergelerini takip ederek ayarlamak için temsilciler geçin [ekibine Liderlikte görev bir veri bilimi takım için](team-lead-tasks.md) veya [ Proje için bir veri bilimi ekibi müşteri adayı görevleri](project-lead-tasks.md).

- Git makinenizde yüklü olması gerekir. Bir veri bilimi sanal makinesi (DSVM) kullanıyorsanız, Git önceden yüklenmiş ve hazırsınız demektir. Aksi takdirde bkz [platformları ve araçlarıyla ek](platforms-and-tools.md#appendix).  
- Kullanıyorsanız bir **Windows DSVM**, ihtiyacınız [Git Credential Manager'ı (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) makinenizde yüklü. README.md dosyasında doğru aşağı kaydırın **indirme ve yükleme** tıklayın ve bölüm *en son yükleyicisi*. Bu en son yükleyici sayfasına götürür. .Exe yükleyiciyi buradan indirin ve çalıştırın. 
- Kullanıyorsanız **Linux DSVM'sini**, bir SSH ortak anahtarı üzerinde DSVM oluşturma ve Grup Azure DevOps hizmetlerinizi ekleyin. SSH hakkında daha fazla bilgi için bkz: **oluşturma SSH ortak anahtarı** konusundaki [platformları ve araçlarıyla ek](platforms-and-tools.md#appendix). 
- Takım veya proje müşteri adayı DSVM'ye bağlamak için gereken bazı Azure dosya depolama oluşturduysa, bunları Azure dosya depolama bilgilerini almanız gerekir. 

## <a name="step-1-3-clone-group-team-and-project-repositories-to-local-machine"></a>1-3. adım: Grup ve takım projesi depolarını yerel makinenize kopyalama

Bu bölümde, proje bağımsız katılımcıları ilk üç görevleri tamamlama yönergeleri sağlar: 

- Kopya **GroupUtilities** depoya R2 D2
- Kopya **TeamUtilities** -D4 depoyu R4 
- Kopya **proje** D5 R5 depoya.

Yerel makinenizde bir dizin oluşturma ***C:\GitRepos*** (Windows için) veya ***$home/GitRepos*** (forLinux) ve ardından bu dizine geçin. 

Kopyalama için (işletim sisteminiz için gerektiği şekilde) aşağıdaki komutlardan birini çalıştırın, **GroupUtilities**, **TeamUtilities**, ve **proje** depolarını dizinleri için Yerel Makine: 

**Windows**
    
    git clone <the URL of the GroupUtilities repository>
    git clone <the URL of the TeamUtilities repository>
    git clone <the URL of the Project repository>
    
![2](./media/project-ic-tasks/project-ic-2-clone-three-repo-to-ic.png)

Üç klasör proje dizininiz altında gördüğünüzü onaylayın.

![3](./media/project-ic-tasks/project-ic-3-three-repo-cloned-to-ic.png)

**Linux**
    
    git clone <the SSH URL of the GroupUtilities repository>
    git clone <the SSH URL of the TeamUtilities repository>
    git clone <the SSH URL of the Project repository>

![4](./media/project-ic-tasks/project-ic-4-clone-three-repo-to_ic-linux.png)

Üç klasör proje dizininiz altında gördüğünüzü onaylayın.

![5](./media/project-ic-tasks/project-ic-5-three-repo-cloned-to-ic-linux.png)

## <a name="step-4-5-mount-azure-file-storage-to-your-dsvm-optional"></a>4-5. adım: Azure dosya depolama bağlama için DSVM'ye (isteğe bağlı)

Azure dosya depolama bağlama için DSVM'ye için Bölüm 4'ü yönergelere bakın [ekip sağlama görevleri için bir veri bilimi ekibi](team-lead-tasks.md)

## <a name="next-steps"></a>Sonraki adımlar

Team Data Science Process tarafından tanımlanan görevleri ve rolleri ayrıntılı açıklamaları için bağlantılar şunlardır:

- [Bir veri bilimi takım için Grup yöneticisi görevleri](group-manager-tasks.md)
- [Bir veri bilimi takım için takım sağlama görevleri](team-lead-tasks.md)
- [Proje için bir veri bilimi ekibi müşteri adayı görevleri](project-lead-tasks.md)
- [Bir veri bilimi takım için proje bağımsız katılımcıları](project-ic-tasks.md)

