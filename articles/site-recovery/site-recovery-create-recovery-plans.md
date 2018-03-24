---
title: Oluşturma ve yük devretme ve kurtarma Azure Site kurtarma için kurtarma planları özelleştirme | Microsoft Docs
description: Oluşturma ve Azure Site Recovery kurtarma planlarında özelleştirme hakkında bilgi edinin. Bu makalede, yük devri ve sanal makineleri ve fiziksel sunucuları kurtarma açıklar.
services: site-recovery
documentationcenter: ''
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/21/2018
ms.author: raynew
ms.openlocfilehash: e02672ea76eada2d660b20f91c4417019d4efc97
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="create-and-customize-recovery-plans"></a>Oluşturma ve kurtarma planları özelleştirme

Bu makalede nasıl oluşturulacağı ve bir kurtarma planı özelleştirme [Azure Site Recovery](site-recovery-overview.md). Başlamadan önce [daha fazla bilgi edinin](recovery-plan-overview.md) kurtarma planları hakkında.

## <a name="create-a-recovery-plan"></a>Kurtarma planı oluşturma

1. Kurtarma Hizmetleri kasasına seçin **kurtarma planları (Site Recovery)** > **+ kurtarma planı**.
2. İçinde **oluşturma kurtarma planı**, plan için bir ad belirtin.
3. Bir kaynak ve hedef makinelere plana göre seçip **Resource Manager** dağıtım modeli için. Kaynak konumu, etkinleştirilen makineleri yük devretme ve kurtarma olması gerekir. 

   **Yük devretme** | **Kaynak** | **Hedef** 
   --- | --- | ---
   Azure-Azure arası | Azure bölgesi |Azure bölgesi
   VMware Azure | Yapılandırma sunucusu | Azure
   Fiziksel makineleri azure'a | Yapılandırma sunucusu | Azure   
   Azure VMM tarafından yönetilmeyen Hyper-V  | VMM görünen adı | Azure
   VMM'den Azure'a olmadan Hyper-V | Hyper-V sitesi adı | Azure
   VMM VMM |VMM kolay adı | VMM görünen adı 

   > [!NOTE]
   > Bir kurtarma planı aynı kaynak ve hedef makinelerle içerebilir. VMware ve VMM tarafından yönetilen Hyper-V Vm'lerini aynı planda olamaz. VMware Vm'lerini ve fiziksel sunucuların aynı planında, kaynak yapılandırma sunucusu olduğu olabilir.

2. İçinde **seçin, sanal makineleri öğelerini**, plana eklemek istediğiniz makineler (veya çoğaltma grubu) seçin. Daha sonra, **Tamam**'a tıklayın.
    - Makineler planında varsayılan grubu (Grup 1) eklenir. Yük devretme sonrasında, bu gruptaki tüm makineler aynı anda başlatın.
    - Yalnızca makineleri seçebilirsiniz, belirttiğiniz kaynak ve hedef konumların yazılımında. 
1. Tıklatın **Tamam** planı oluşturmak için.

## <a name="add-a-group-to-a-plan"></a>Bir plana bir Grup Ekle

Ek grupları oluşturun ve böylece bir grup tarafından Gru temelinde farklı bir davranış belirtebilirsiniz makineleri farklı gruplara ekleyin. Örneğin, bir grup makinelerinizde yük devretme sonrasında başlattığınızda veya grubu başına özelleştirilmiş eylemleri belirtin belirtebilirsiniz.

1. İçinde **kurtarma planları**, plana sağ > **Özelleştir**. Varsayılan olarak, bir plan oluşturduktan sonra tüm kendisine eklenmiş makineler varsayılan Grup 1 bulunur.
2. Tıklatın **+ grup**. Varsayılan olarak yeni bir grup içinde eklenir sırayla numaralandırılır. En çok yedi grupları olabilir.
3. Yeni gruba taşımak istediğiniz makinede seçin **grubu Değiştir**ve ardından yeni grubu seçin. Alternatif olarak, grup adına sağ tıklayın > **korumalı öğe**ve makineler grubuna ekleyin. Bir makine veya çoğaltma grubu yalnızca bir kurtarma planı bir gruba ait olabilir.


## <a name="add-a-script-or-manual-action"></a>Bir komut dosyası veya el ile eylemi ekleyin

Bir komut dosyası veya işlemi el ile ekleyerek, bir kurtarma planı özelleştirebilirsiniz. Şunlara dikkat edin:

- Azure'a çoğaltma yapıyorsanız, kurtarma planına Azure Otomasyon çalışma kitabı tümleştirebilirsiniz. [Daha fazla bilgi edinin](site-recovery-runbook-automation.md).
- System Center VMM tarafından yönetilen Hyper-V Vm'lerini çoğaltıyorsanız şirket içi VMM sunucusunda bir komut dosyası oluşturabilir ve kurtarma planında içerir.
- Bir komut dosyası eklediğinizde, yeni bir grup için Eylemler kümesi ekler. Örneğin, Grup 1 öncesi adımları kümesini adıyla oluşturulur *Grup 1: öncesi adımları*. Tüm ön adımları bu kümesi içinde listelenir. Yalnızca dağıtılan bir VMM sunucunuz varsa, birincil sitede bir komut dosyası ekleyebilirsiniz.
- Kurtarma planı çalıştığında işlemi el ile eklerseniz, işlemi el ile eklenen bir noktada durdurur. Bir iletişim kutusu, el ile gerçekleştirilen işlemin tamamlandığını belirtmenizi ister.
- VMM sunucusunda bir komut dosyası oluşturmak için'ndaki yönergeleri izleyin [bu makalede](hyper-v-vmm-recovery-script.md).
- Betikleri ikincil siteye yük devretme sırasında ve yeniden çalışma sırasında ikincil siteden için birincil uygulanabilir. Destek çoğaltma senaryonuza bağlıdır:
    
    **Senaryo** | **Yük devretme** | **İlk duruma döndürme**
    --- | --- | --- 
    Azure-Azure arası  | Runbook | Runbook
    Vmware’den Azure’a | Runbook | NA 
    Azure'a VMM ile Hyper-V | Runbook | Betik
    Azure'da Hyper-V sitesi | Runbook | NA
    İkincil VMM VMM | Betik | Betik

1. Kurtarma planında, hangi eylemini eklenmelidir ve eylem ne zaman gerçekleşeceğini belirtin adımına tıklayın: bir. Bir eylemin gruptaki makineler yük devretme sonrasında, select başlatmadan önce gerçekleşmesi için istiyorsanız **ön eylem eklemek**.
    b. Grup başlangıç makinelerinizde sonra Yük devretme işleminden sonra gerçekleşecek eylemi istiyorsanız seçin **post eylem eklemek**. Eylem konumunu taşımak için seçin **Yukarı Taşı** veya **Aşağı Taşı** düğmeler.
2. İçinde **Ekle eylem**seçin **betik** veya **el ile işlem**.
3. İşlemi el ile eklemek istiyorsanız, aşağıdakileri"bir. Eylem için bir ad yazın ve eylem yönergeleri yazın. Yük devretme çalıştıran kişinin bu yönergeleri görürsünüz.
    b. İşlemi el ile yük devretme (Test, yük devretme, planlanmış yük devretme (uygunsa)) tüm türleri eklemek isteyip istemediğinizi belirtin. Daha sonra, **Tamam**'a tıklayın.
4. Bir komut dosyası eklemek istiyorsanız, aşağıdakileri yapın: bir. VMM komut dosyası ekliyorsanız seçin **VMM komut dosyası için yük devretme**ve içinde **betik yolu** paylaşım göreli yolunu yazın. Örneğin, paylaşım, bulunuyorsa \\ <VMMServerName>\MSSCVMMLibrary\RPScripts, yolunu belirtin: \RPScripts\RPScript.PS1.
    b. Bir Azure Otomasyonu defterini çalıştır ekliyorsanız belirtin **Azure Otomasyon hesabı** , runbook bulunur ve uygun seçin **Azure Runbook betiği**.
5. Kurtarma planı betiği beklendiği gibi çalıştığından emin olmak için bir sınama yük devretmesi çalıştırın.

## <a name="watch-a-video"></a>Bir videoyu izleyin

Bir kurtarma planı nasıl oluşturulacağını gösteren bir video izleyin.


> [!VIDEO https://www.youtube.com/embed/1KUVdtvGqw8]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek [yerine çalıştıran](site-recovery-failover.md).  

    
