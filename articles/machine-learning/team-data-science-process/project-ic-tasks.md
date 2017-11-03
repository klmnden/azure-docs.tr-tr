---
title: "Bir tek tek katkıda bulunan için - Azure veri bilimi işlemi görevleri ekip | Microsoft Docs"
description: "Veri bilimi takım projesi üzerinde tek tek bir katkıda bulunan görevlerde ana hattı."
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: bradsev;
ms.openlocfilehash: bbe691174409202a8fd9602a69e764f0a8e2816b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="individual-contributor-tasks"></a>Tek tek katkıda bulunan görevleri

Bu konuda anahatları tek tek katkıda görevleri tamamlamak için kendi veri bilimi ekibi bekleniyordu. Amaç üzerinde standartlaştıran ekip işbirliği ortamı belirtmektir [takım veri bilimi işlemi](overview.md) (TDSP). Bu işlem üzerinde Standartlaştırma personel rolleri ve veri bilimi ekibi tarafından işlenen ilişkilendirilen görevlerinin bir özetini görmek [takım veri bilimi işlemi rolleri ve görevleri](roles-tasks.md).

Proje tek tek Katkıda Bulunanlar (Proje TDSP ortamını ayarlamak için veri bilimcilerine), görevleri aşağıda gösterilen: 

![1](./media/project-ic-tasks/project-ic-1-tdsp-data-scientist.png)

- **GroupUtilities** grubunuzun yararlı yardımcı programları grubun tamamını arasında paylaşmak için koruma depodur. 
- **TeamUtilities** ekibinizin özellikle ekibiniz için koruma depodur. 

Bir veri bilimi projesi TDSP altında çalıştırmak yönergeler için bkz: [veri bilimi projeleri yürütme](project-execution.md). 

>[AZURE.NOTE] Visual Studio Team Services (VSTS) aşağıdaki yönergeleri kullanarak TDSP takım ortamı kurmak için gerekli adımları ana hatlarını vermektedir. Biz, biz Microsoft'taki TDSP nasıl uygulamak olduğundan bu görevlerin VSTS ile nasıl gerçekleştirileceğini belirtin. Grubunuz için başka bir kod barındırma platformu kullanılıyorsa, ekip lideri tarafından genellikle tamamlanması gereken görevler değiştirmeyin. Ancak bu görevleri tamamlamak için yol farklı olacak.


## <a name="repositories-and-directories"></a>Depoları ve dizinler

Bu öğretici depoları ve dizinleri kısaltılmış adlarını kullanır. Bu adları depoları ve dizinler arasında işlemleri izlemek kolaylaştırır. Bu gösterim (**R** Git depoları için ve **D** , DSVM yerel dizinleri için) aşağıdaki bölümlerde kullanılır:

- **R2**: GroupUtilities depo, Grup Yöneticisi VSTS Grup sunucunuz üzerinde ayarlanmış Git üzerinde.
- **R4**: Takım sağlama ayarlanmış Git deposunu TeamUtilities.
- **R5**: Project, proje lideri tarafından ayarlanmış Git deposunu.
- **D2**: yerel dizin R2'den kopyalanamıyor.
- **D4**: yerel dizin R4 kopyalanabilir.
- **D5**: yerel dizin R5 kopyalanabilir.


## <a name="step-0-prerequisites"></a>Step-0: Önkoşullar

Özetlenen, Grup Yöneticisi atanan görevleri tamamlayarak önkoşullara uyduğunuzdan [grup yöneticisi görevleri için bir veri bilimi ekibi](group-manager-tasks.md). Burada özetlemek için aşağıdaki gereksinimleri ekip sağlama görevleri başlamadan önce karşılanması gerekir: 
- Grup Yöneticisi ayarlanmış **GroupUtilities** deposu (varsa). 
- Ekip Lideri ayarlanmış **TeamUtilities** deposu (varsa).
- Proje lideri proje depoyu ayarladı. 
- Öğesinden kopyalayın ve geri proje depoya gönderme için ayrıcalığına sahip, proje lideri tarafından proje deponuza eklenmiştir.

İkinci **TeamUtilities** , önkoşul depodur ekibinizin bir takım özgü yardımcı programı deposu olup bağlı olarak isteğe bağlıdır. Herhangi diğer üç önkoşulları değil tamamlanmış, ekip lideri, proje lideri veya yönergelerini izleyerek ayarlamak için kendi temsilciler başvurun [veri bilimi ekibi Ekip Lideri görevlerde](team-lead-tasks.md) veya [ Proje veri bilimi ekibi için sağlama görevleri](project-lead-tasks.md).

- Makinenizde Git yüklenmesi gerekir. Bir veri bilimi sanal makine (DSVM) kullanıyorsanız, Git önceden yüklenmiş ve hazırsınız. Aksi takdirde bkz [platformları ve araçlarına ek](platforms-and-tools.md#appendix).  
- Kullanıyorsanız bir **Windows DSVM**, olmasına gerek [Git kimlik bilgisi Yöneticisi (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) makinenize yüklü. README.md dosyasında doğru aşağı kaydırın **yükleyip** 'ye tıklayın *son yükleyici*. Bu son yükleyici sayfasına götürür. .Exe yükleyici buradan indirin ve çalıştırın. 
- Kullanıyorsanız **Linux DSVM**, bir SSH ortak anahtarı, DSVM üzerinde oluşturun ve Grup VSTS sunucunuzu ekleyin. SSH hakkında daha fazla bilgi için bkz: **oluşturma SSH ortak anahtarı** bölümüne [platformları ve araçlarına ek](platforms-and-tools.md#appendix). 
- Takım ve/veya proje sağlama, DSVM bağlamak için gereken bazı Azure dosya depolama oluşturduysa, bunları Azure dosya depolama bilgi almanız gerekir. 

## <a name="step-1-3-clone-group-team-and-project-repositories-to-local-machine"></a>1-3. adım: Grup, ekip ve yerel makineye proje depoları kopyalama

Bu bölümde, proje tek tek Katkıda Bulunanlar, ilk üç görevleri tamamlama yönergeler sağlar: 

- Kopya **GroupUtilities** D2 R2 depoya
- Kopya **TeamUtilities** D4 R4 depoya 
- Kopya **proje** D5 R5 depoya.

Yerel makinenizde bir dizin oluşturun ***C:\GitRepos*** (Windows için) veya ***$home/GitRepos*** (forLinux) ve ardından bu dizine geçin. 

Kopyalama (hangisi uygunsa, işletim sistemi için) aşağıdaki komutlardan birini çalıştırın, **GroupUtilities**, **TeamUtilities**, ve **proje** depoları dizinleri için Yerel Makine: 

**Windows**
    
    git clone <the URL of the GroupUtilities repository>
    git clone <the URL of the TeamUtilities repository>
    git clone <the URL of the Project repository>
    
![2](./media/project-ic-tasks/project-ic-2-clone-three-repo-to-ic.png)

Proje dizini altında üç klasör gördüğünüzü onaylayın.

![3](./media/project-ic-tasks/project-ic-3-three-repo-cloned-to-ic.png)

**Linux**
    
    git clone <the SSH URL of the GroupUtilities repository>
    git clone <the SSH URL of the TeamUtilities repository>
    git clone <the SSH URL of the Project repository>

![4](./media/project-ic-tasks/project-ic-4-clone-three-repo-to_ic-linux.png)

Proje dizini altında üç klasör gördüğünüzü onaylayın.

![5](./media/project-ic-tasks/project-ic-5-three-repo-cloned-to-ic-linux.png)

## <a name="step-4-5-mount-azure-file-storage-to-your-dsvm-optional"></a>4-5. adım: bağlama Azure file Storage'a, DSVM (isteğe bağlı)

Bağlama Azure dosya depolama için DSVM bölüm 4'ndaki yönergeleri bkz [veri bilimi ekibi için takım sağlama görevleri](team-lead-tasks.md)

## <a name="next-steps"></a>Sonraki adımlar

Burada, rolleri ve görevleri takım veri bilimi işlem tarafından tanımlanan daha ayrıntılı açıklamaları bağlantıları verilmiştir:

- [Bir veri bilimi ekibi için Grup yöneticisi görevleri](group-manager-tasks.md)
- [Bir veri bilimi ekibi için takım sağlama görevleri](team-lead-tasks.md)
- [Proje veri bilimi ekibi için sağlama görevleri](project-lead-tasks.md)
- [Proje veri bilimi ekibi için tek tek Katkıda Bulunanlar](project-ic-tasks.md)

