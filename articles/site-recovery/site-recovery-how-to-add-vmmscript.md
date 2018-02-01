---
title: "Azure Site Recovery kurtarma planında komut dosyaları eklemek nasıl | Microsoft Docs"
description: "VMM'den Azure'a kurtarma planında için yeni bir komut dosyası ekleme önkoşulları açıklar"
services: site-recovery
documentationcenter: 
author: ruturaj
manager: shons
editor: 
ms.assetid: 72408c62-fcb6-4ee2-8ff5-cab1218773f2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/13/2017
ms.author: ruturaj
ms.openlocfilehash: 60c6eebd9323028c63f42bd8a0996e3c0a2a1cf1
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="add-vmm-scripts-to-a-recovery-plan"></a>VMM komut dosyalarının bir kurtarma planına ekleyin


Bu makalede oluşturma ve kurtarma planları için VMM komut dosyaları ekleme hakkında bilgi sağlanmaktadır [Azure Site Recovery](site-recovery-overview.md).

Tüm yorumlarınızı ve sorularınızı bu makalenin alt kısmında veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda paylaşabilirsiniz.

## <a name="prerequisites-of-adding-a-script-to-recovery-plan"></a>Kurtarma planı için bir komut dosyası ekleme önkoşulları

Kurtarma planlarınızı PowerShell komut dosyalarını kullanabilirsiniz. Bunları yazar ve kurtarma planından erişilebilir olmasını VMM Kitaplığı'nda yerleştirin gerekecektir. Aşağıda betik yazarken dikkat edilecek bazı noktalar verilmiştir.

* Böylece özel durumlar düzgün biçimde işlenen komut dosyaları try-catch bloklarını kullandığınızdan emin olun.
    - Komut dosyasında bir özel durum ise, çalışmayı durdurur ve görev başarısız olarak gösterir.
    - Bir hata oluşursa, betik, herhangi bir kalan parçası çalıştırmaz.
    - Planlanmamış bir yük devretme çalıştırdığınızda, bir hata oluşursa, kurtarma planı devam eder.
    - Planlanmış bir yük devretme çalıştırdığınızda, bir hata oluşursa, kurtarma planı durdurur. Komut dosyası, beklendiği gibi çalışır onay ve kurtarma planı yeniden çalıştırın çözmeniz gerekir.
        - Write-Host komutu, bir kurtarma planı betiği çalışmaz ve komut dosyası başarısız olur. Çıktı oluşturmak için sırayla ana komut çalıştıran bir proxy komut dosyası oluşturun. Tüm çıktı kullanarak varsayılır emin olun >> komutu.
        - 600 saniye içinde döndürmezse komut dosyası zaman aşımına uğradı.
        - Herhangi bir şey STDERR yazılır, komut dosyası başarısız olarak sınıflandırılır. Bu bilgiler, komut dosyası yürütme ayrıntılar görüntülenir.

* Bir kurtarma planı betiklerde VMM hizmet hesabı bağlamında çalışır. Bu hesap için komut dosyasının bulunduğu uzak paylaşıma Okuma izinlerine sahip olduğundan emin olun. VMM hizmet hesabı ayrıcalık düzeyinde çalıştırmak için komut dosyası sınayın.
* VMM cmdlet'lerini Windows PowerShell modülündeki teslim edilir. VMM konsolunu yüklediğinizde modülü yüklenir. Komut dosyasında aşağıdaki komutu kullanarak, komut dosyası içine yüklenebilir:
   - Import-Module-Name virtualmachinemanager. [Daha fazla bilgi edinin](https://technet.microsoft.com/library/hh875013.aspx).
* En az bir kitaplık sunucusu VMM dağıtımınızda olduğundan emin olun. Varsayılan olarak, bir VMM sunucusu kitaplık paylaşım yolunu ilişkin klasör adını MSCVMMLibrary VMM sunucusunda yerel olarak bulunur.
    * Paylaşımı kitaplık paylaşım yolu uzaktan (veya yerel ancak MSCVMMLibrary ile paylaşılan) ise, aşağıdaki gibi yapılandırın (kullanarak \\libserver2.contoso.com\share\ bir örnek olarak):
      * Kayıt Defteri Düzenleyicisi'ni açın ve gidin **HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure Site Recovery\Registration**.
      * Değeri düzenlemek **ScriptLibraryPath** ve olarak yerleştirme \\libserver2.contoso.com\share\. Tam FQDN'yi belirtin. Paylaşım konumunu izinleri sağlar. Bu paylaşımın kök düğümü olduğuna dikkat edin. **Bunu denetlemek için kök düğümde VMM kitaplığı göz atabilirsiniz. Açılır yol, yolun kökü olacaktır - bir değişkende kullanmanız gerekecektir**.
      * VMM hizmet hesabıyla aynı izinlere sahip bir kullanıcı hesabıyla betik test ettiğinizden emin olun. Bu, tek başına denetler test betikleri kurtarma planları göründükleri gibi aynı şekilde çalıştırın. VMM sunucusunda şu şekilde atlamak için yürütme ilkesini ayarlayın:
        * Açık **64-bit Windows PowerShell** yükseltilmiş ayrıcalıklar kullanarak konsol.
        * Tür: **Set-executionpolicy bypass**. [Daha fazla bilgi edinin](https://technet.microsoft.com/library/ee176961.aspx).

> [!IMPORTANT]
> Atlama yalnızca 64-bit PowerShell yürütme ilkesini ayarlamanız gerekir. İçin 32-bit PowerShell ayarladıysanız, komut dosyaları değil exeute olur.

## <a name="add-the-script-to-the-vmm-library"></a>VMM kitaplığı komut dosyası ekleme

Bir VMM kaynak site varsa, VMM sunucusunda bir komut dosyası oluşturabilir ve kurtarma planınıza dahil.

1. Kitaplık paylaşımında yeni bir klasör oluşturun. Örneğin, \<Vmmsunucusuadı > \MSSCVMMLibrary\RPScripts. Kaynağında yerleştirin ve VMM sunucuyu hedefleyebilir.
2. (Örneğin RPScript) komut dosyası oluşturabilir ve beklendiği gibi çalıştığını denetleyin.
3. Komut dosyası konuma yerleştirmek \<Vmmsunucusuadı > kaynak ve hedef VMM sunucularında \MSSCVMMLibrary.

## <a name="add-the-script-to-a-recovery-plan"></a>Bir kurtarma planı için komut dosyası ekleme

Sanal makineleri veya çoğaltma gruplarına eklenmesi ve planı oluşturuldu sonra komut dosyasını gruba ekleyebilirsiniz.

1. Kurtarma planı açın.
2. Bir öğeyi tıklatın **adım** listeleyin ve ardından **betik** veya **el ile eylemi**.
3. Komut dosyası veya eylem önce veya sonra seçili öğe eklemek istediğiniz belirtin. Kullanım **Yukarı Taşı** ve **Aşağı Taşı** komut dosyası konumunu yukarı veya Aşağı Taşı düğmeleri,.
4. VMM komut dosyası eklerseniz, seçin **VMM komut dosyası için yük devretme**. İçinde **betik yolu**, paylaşım göreli yolunu yazın. VMM aşağıdaki örnekte, yolunu belirtin: **\RPScripts\RPScript.PS1**.
5. Defterini çalıştır bir Azure Otomasyonu eklerseniz, runbook bulunduğu Azure Otomasyonu hesabı belirtin ve uygun Azure runbook komut dosyasını seçin.
6. Kurtarma planı betiği beklendiği gibi çalıştığından emin olmak için bir sınama yük devretmesi yapın.


## <a name="next-steps"></a>Sonraki adımlar

[Daha fazla bilgi edinin](site-recovery-failover.md) yük devretme işlemleri çalıştırma hakkında.
