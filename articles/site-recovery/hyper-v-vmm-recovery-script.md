---
title: Azure Site Recovery kurtarma planında bir betik ekleyin | Microsoft Docs
description: Azure'da bir kurtarma planı için yeni bir System Center Virtual Machine Manager (VMM) komut dosyası eklemek için önkoşullar hakkında bilgi edinin.
services: site-recovery
documentationcenter: ''
author: rajani-janaki-ram
manager: rochakm
editor: ''
ms.assetid: 72408c62-fcb6-4ee2-8ff5-cab1218773f2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/06/2018
ms.author: rajanaki
ms.openlocfilehash: 71991347ffaf036065aae9e1a93b7eb83a14b15c
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37917358"
---
# <a name="add-a-vmm-script-to-a-recovery-plan"></a>VMM komut dosyası için bir kurtarma planı Ekle

Bu makalede System Center Virtual Machine Manager (VMM) bir komut dosyası oluşturma ve bir kurtarma planında eklemek [Azure Site Recovery](site-recovery-overview.md).

Alt bu makalenin veya üzerinde yorum veya sorularınız sonrası [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Önkoşullar

Kurtarma planlarınızda PowerShell betiklerini kullanabilirsiniz. Kurtarma planından erişilebilir olması için betik yazma ve komut dosyasını VMM kitaplığına yerleştirin. Betik yazarken aşağıdaki konuları göz önünde bulundurun:

* Özel durumlar düzgün bir şekilde işlenir böylece betikleri'nın try-catch bloklarını kullandığınızdan emin olun.
    - Betikte bir özel durum oluşursa, betik çalıştıran durdurur ve görev başarısız olarak gösterilir.
    - Bir hata oluşursa, betik geri kalanında çalıştırılmaz.
    - Planlanmamış yük devretme çalıştırdığınızda bir hata oluşursa, kurtarma planı devam eder.
    - Planlı yük devretme çalıştırdığınızda bir hata oluşursa, kurtarma planı durdurur. Betik, beklendiği gibi çalıştığını denetleyin ve kurtarma planı yeniden çalıştırın düzeltin.
        - `Write-Host` Komutu, bir kurtarma planı betikte çalışmaz. Kullanırsanız `Write-Host` komutu bir betikte, komut dosyası başarısız olur. Çıktı oluşturmak için sırayla ana kodunuzu çalıştıran bir proxy betiği oluşturun. Tüm çıkış kullanıma cmdlet'iyle yönetilir sağlamak için kullanın **\> \>** komutu.
        - 600 saniye içinde döndürmezse betik zaman aşımına uğradı.
        - Herhangi bir şey STDERR yazılmışsa, betiği başarısız olarak sınıflandırılır. Bu bilgiler, betik yürütme ayrıntıları görüntülenir.

* Bir kurtarma planı betiklerde VMM hizmet hesabı bağlamında çalıştırın. Bu hesap betiğin bulunduğu uzak paylaşımı için izinleri okuduğundan emin olun. Betiğin VMM hizmet hesabıyla aynı düzeyde kullanıcı hakları ile çalışmasını test edin.
* VMM cmdlet, bir Windows PowerShell modülüne teslim edilir. Modülü, VMM konsolunu yüklediğinizde yüklenir. Modülü, komut dosyanıza yüklemek için betikte aşağıdaki komutu kullanın: 

    `Import-Module -Name virtualmachinemanager`

    Daha fazla bilgi için [VMM ve Windows PowerShell ile çalışmaya başlama](https://technet.microsoft.com/library/hh875013.aspx).
* En az bir kitaplık sunucusu VMM dağıtımınız olduğundan emin olun. Varsayılan olarak, bir VMM sunucusu için kitaplık paylaşım yolu, VMM sunucusunda yerel olarak bulunur. Klasör MSCVMMLibrary adıdır.

  Kitaplık yoluna uzaktan (veya yerel ancak MSCVMMLibrary ile paylaşılan olup olmadığını) ise, kullanarak paylaşım aşağıdaki gibi yapılandırın \\libserver2.contoso.com\share\ örnek olarak:
  
  1. Kayıt Defteri Düzenleyicisi'ni açın ve ardından Git **HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure Site Recovery\Registration**.

  2. Değerini **ScriptLibraryPath** için  **\\\libserver2.contoso.com\share\\**. Tam FQDN'yi belirtin. Konum paylaşımı için izinler sağlayın. Bu paylaşım kök düğümüdür. Vmm'de kök düğümü olup olmadığını denetlemek için Kitaplığı'nda kök düğümüne gidin. Açılır yolun kökünü yoldur. Değişkeninde kullanmanız gereken yolu budur.

  3. Betik, VMM hizmet hesabıyla aynı düzeyde kullanıcı hakları olan bir kullanıcı hesabı kullanarak test edin. Bu kullanıcı haklarını kullanarak, bir tek başına, test edilmiş betiklerin kurtarma planlarıyla, çalıştırıldıkları aynı şekilde doğrular. VMM sunucusunda şu şekilde atlamak için yürütme ilkesini ayarlayın:

     a. Açık **64 bit Windows PowerShell** konsolunu yönetici olarak.
     
     b. Girin **Set-executionpolicy atlama**. Daha fazla bilgi için [Set-ExecutionPolicy cmdlet'ini kullanarak](https://technet.microsoft.com/library/ee176961.aspx).

     > [!IMPORTANT]
     > Ayarlama **Set-executionpolicy atlama** yalnızca 64 bit PowerShell konsolundaki. Komut dosyaları için 32-bit PowerShell konsolunda ayarlarsanız çalıştırmayın.

## <a name="add-the-script-to-the-vmm-library"></a>Betiğin VMM kitaplığına ekleme

Bir VMM kaynak site varsa, VMM sunucusunda bir komut dosyası oluşturabilirsiniz. Ardından, betiği kurtarma planınıza dahil.

1. Kitaplık paylaşımında, yeni bir klasör oluşturun. Örneğin, \<VMM Sunucu adı > \MSSCVMMLibrary\RPScripts. Kaynak klasöre yerleştirin ve VMM sunucuları hedefleyin.
2. Komut dosyası oluşturun. Örneğin, komut dosyası RPScript adlandırın. Komut dosyası beklendiği gibi çalıştığını doğrulayın.
3. Betikte yerleştirin \<VMM Sunucu adı > kaynak ve hedef VMM sunucularında \MSSCVMMLibrary klasör.

## <a name="add-the-script-to-a-recovery-plan"></a>Bir kurtarma planı için komut dosyası Ekle

Kurtarma planına eklenen Vm'leri veya çoğaltma gruplarına ve planı oluşturduktan sonra gruba betiği ekleyebilirsiniz.

1. Kurtarma planını açın.
2. İçinde **adım** listesinde, bir öğe seçin. Ardından, ya da seçin **betik** veya **el ile eylemi**.
3. Betik veya eylem önce veya sonra seçili öğe eklenip eklenmeyeceğini belirtin. Betik konumu yukarı veya aşağı taşımak, seçmek **Yukarı Taşı** ve **Aşağı Taşı** düğmeleri.
4. VMM komut dosyası eklerseniz, seçin **yük devretme VMM betiği**. İçinde **betik yolu**, paylaşıma göreli yolunu girin. Örneğin, **\RPScripts\RPScript.PS1**.
5. Azure Otomasyonu runbook'u eklerseniz, runbook bulunduğu Otomasyon hesabı belirtin. Ardından, kullanmak istediğiniz Azure runbook betiği seçin.
6. Komut dosyası beklendiği gibi çalıştığından emin olmak için bir yük devretme kurtarma planı yapın.


## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [devretme testlerini çalıştırma](site-recovery-failover.md).

