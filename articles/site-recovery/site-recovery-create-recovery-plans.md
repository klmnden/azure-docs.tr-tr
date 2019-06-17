---
title: Oluşturma ve Azure Site Recovery ile olağanüstü durum kurtarma için kurtarma planlarını özelleştirme | Microsoft Docs
description: Oluşturma ve Azure Site Recovery hizmetini kullanarak olağanüstü durum kurtarma için kurtarma planları özelleştirme hakkında bilgi edinin.
author: rayne-wiselman
manager: carmonm
services: site-recovery
ms.service: site-recovery
ms.topic: article
ms.date: 05/30/2019
ms.author: raynew
ms.openlocfilehash: 866374df7d3a6973cfc5995afd5cc3c4b0145c48
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66399995"
---
# <a name="create-and-customize-recovery-plans"></a>Oluşturma ve kurtarma planlarını özelleştirme

Bu makalede oluşturma ve bir kurtarma planındaki özelleştirme [Azure Site Recovery](site-recovery-overview.md). Başlamadan önce [daha fazla bilgi edinin](recovery-plan-overview.md) kurtarma planları hakkında.

## <a name="create-a-recovery-plan"></a>Kurtarma planı oluşturma

1. Kurtarma Hizmetleri Kasası'nda seçin **kurtarma planları (Site Recovery)**  >  **+ kurtarma planı**.
2. İçinde **kurtarma planı oluştur**, plan için bir ad belirtin.
3. Kaynak ve hedef makinelerde plana göre seçip **Resource Manager** dağıtım modeli için. Kaynak konumu makineler yük devretme ve kurtarma için etkinleştirilmiş olması gerekir. 

   **Yük devretme** | **Kaynak** | **Hedef** 
   --- | --- | ---
   Azure-Azure arası | Azure bölgesi |Azure bölgesi
   Vmware'den azure'a | Yapılandırma sunucusu | Azure
   Fiziksel makineleri azure'a | Yapılandırma sunucusu | Azure   
   Azure'a VMM tarafından yönetilen Hyper-V  | VMM görünen adı | Azure
   Azure'a VMM olmadan Hyper-V | Hyper-V site adı | Azure
   VMM'den VMM'e |VMM kolay adı | VMM görünen adı 

   > [!NOTE]
   > Bir kurtarma planı ile aynı kaynak ve hedef makine içerebilir. VMware ve VMM tarafından yönetilen Hyper-V Vm'lerini aynı plana olamaz. VMware Vm'leri ve fiziksel sunucuları aynı planında, kaynağın bir yapılandırma sunucusu olduğu olabilir.

2. İçinde **seçin, sanal makineler öğelerini**, plana eklemek istediğiniz makineleri (veya çoğaltma grubunu) seçin. Daha sonra, **Tamam**'a tıklayın.
    - Makineler, varsayılan grup (Grup 1) plana eklenir. Yük devretme işleminden sonra bu gruptaki tüm makinelerin aynı anda başlatın.
    - Yalnızca makineleri seçebilirsiniz, belirtilen kaynak ve hedef konumların de. 
1. Tıklayın **Tamam** planı oluşturun.

## <a name="add-a-group-to-a-plan"></a>Bir plana bir grubu Ekle

Ek grupları oluşturun ve bu makine, farklı gruplara ekleyebilirsiniz, böylece bir grubu tarafından temelinde farklı davranış belirtebilirsiniz. Örneğin, bir gruptaki makinelerin yük devretme işleminden sonra Başlat veya grup başına özelleştirilmiş eylemleri belirtin, belirtebilirsiniz.

1. İçinde **kurtarma planları**, plan sağ tıklayın > **Özelleştir**. Varsayılan olarak, bir plan oluşturduktan sonra tüm ona eklediğiniz makineleri varsayılan Grup 1 bulunur.
2. Tıklayın **+ grup**. Varsayılan olarak yeni bir grup eklenmiş sırada numaralandırılmıştır. En fazla yedi grubunuz olabilir.
3. Yeni grubuna taşımak için istediğiniz makineyi seçin **grubu Değiştir**ve ardından yeni grubu seçin. Alternatif olarak, grup adına sağ tıklayın > **korumalı öğesi**, makineleri gruba ekleyin. Bir makine ya da çoğaltma grubu yalnızca bir kurtarma planında bir gruba ait olabilir.


## <a name="add-a-script-or-manual-action"></a>Bir betik veya el ile eylemi ekleme

Bir kurtarma planı betiği veya el ile gerçekleştirilen eylem ekleyerek özelleştirebilirsiniz. Aşağıdakilere dikkat edin:

- Azure'a çoğaltma yapıyorsanız, kurtarma planına Azure Otomasyonu runbook'ları tümleştirebilirsiniz. [Daha fazla bilgi edinin](site-recovery-runbook-automation.md).
- System Center VMM tarafından yönetilen Hyper-V Vm'lerini çoğaltma yapıyorsanız şirket içi VMM sunucusunda bir komut dosyası oluşturabilir ve kurtarma planında içerir.
- Bir betik eklediğinizde, yeni bir grubu için Eylemler ekler. Örneğin, grubu 1 öncesi adımları adıyla oluşturulur *Grup 1: önceki adımlar*. Önceki tüm adımlar bu kümesi içinde listelenir. Dağıtılan bir VMM sunucusu varsa, birincil sitedeki bir komut dosyası ekleyebilirsiniz.
- Kurtarma planında çalıştığında el ile gerçekleştirilen bir eylem eklerseniz, el ile gerçekleştirilen eylem yerleştirdiğiniz noktada durdurur. Bir iletişim kutusu el ile gerçekleştirilen eylem tamamlandığını belirtmenizi ister.
- VMM sunucusunda bir komut dosyası oluşturmak için yönergeleri izleyin. [bu makalede](hyper-v-vmm-recovery-script.md).
- Betikleri ikincil siteye yük devretme sırasında ve yeniden çalışma sırasında ikincil siteden birincil siteye uygulanabilir. Destek çoğaltma senaryonuza bağlıdır:
    
    **Senaryo** | **Yük devretme** | **İlk duruma döndürme**
    --- | --- | --- 
    Azure-Azure arası  | Runbook | Runbook
    Vmware’den Azure’a | Runbook | NA 
    Azure'a VMM ile Hyper-V | Runbook | Komut Dosyası
    Azure'da Hyper-V sitesi | Runbook | NA
    İkincil VMM VMM'ye | Komut Dosyası | Komut Dosyası

1. Kurtarma planında hangi eylemi eklenmelidir ve eylemi ne zaman gerçekleşmesi gerektiğini belirtin. Adım'a tıklayın:
    1. Gruptaki makinelerin yük devretme sonrasında, select başlamadan gerçekleşecek eylemi istiyorsanız **ön Eylem Ekle**.
    1. Makine grubu başlangıç sonra Yük devretme işleminden sonra gerçekleşecek şekilde eylem istiyorsanız belirleyin **sonraki eylem Ekle**. Eylemin konumuna taşımak, seçmek **Yukarı Taşı** veya **Aşağı Taşı** düğmeleri.
2. İçinde **Ekle eylemini**seçin **betik** veya **el ile gerçekleştirilen eylem**.
3. El ile gerçekleştirilen bir eylem eklemek istiyorsanız, aşağıdakileri yapın:
    1. Eylem için bir ad yazın ve eylem yönergeleri yazın. Yük devretme çalıştıran kişinin, bu yönergeler karşınıza çıkar.
    1. Tüm türleri (Test, yük devretme, (uygunsa) planlı yük devretme) yük devretme için el ile gerçekleştirilen eylem eklemek isteyip istemediğinizi belirtin. Daha sonra, **Tamam**'a tıklayın.
4. Bir komut dosyası eklemek istiyorsanız, aşağıdakileri yapın:
    1. VMM komut ekliyorsanız seçin **yük devretme VMM betiği**hem de **betik yolu** paylaşımına göreli yolunu yazın. Örneğin, paylaşım, bulunuyorsa \\ \<VMMServerName > \MSSCVMMLibrary\RPScripts, yolu belirtin: \RPScripts\RPScript.PS1.
    1. Kitap çalıştıran bir Azure Otomasyonu ekliyorsanız belirtin **Azure Otomasyonu hesabı** , runbook bulunur ve uygun seçin **Azure Runbook betiği**.
5. Kurtarma planı betiği beklendiği gibi çalıştığından emin olmak için bir yük devretme çalıştırın.

## <a name="watch-a-video"></a>Bir video izleyin

Bir kurtarma planı oluşturmak nasıl gösteren bir video izleyin.


> [!VIDEO https://www.youtube.com/embed/1KUVdtvGqw8]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [devretme testlerini çalıştırma](site-recovery-failover.md).  

    
