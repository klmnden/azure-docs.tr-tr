---
title: "Bir kurtarma planı Azure Site kurtarma için bir komut dosyası ekleme | Microsoft Docs"
description: "Yeni bir System Center Virtual Machine Manager (VMM) komut dosyası Azure kurtarma planında eklemeye yönelik önkoşulları hakkında bilgi edinin."
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
ms.openlocfilehash: 2e00f812fb35ac9a0cb390fc6a3ba40a8678f8dd
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="add-a-vmm-script-to-a-recovery-plan"></a>Bir kurtarma planı için bir VMM komut dosyası ekleme

Bu makalede bir System Center Virtual Machine Manager (VMM) komut dosyası oluşturma ve bir kurtarma planı eklemek [Azure Site Recovery](site-recovery-overview.md).

Tüm yorumlarınızı ve sorularınızı bu makalenin veya üzerinde altındaki sonrası [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Önkoşullar

Kurtarma planlarınızı PowerShell komut dosyalarını kullanabilirsiniz. Kurtarma planından erişilebilir olması için komut dosyası yazmak ve komut dosyası VMM Kitaplığı'nda yerleştirin. Betik yazarken aşağıdaki noktaları göz önünde bulundurun:

* Böylece özel durumları düzgün biçimde işlenen komut dosyaları try-catch bloklarını kullandığınızdan emin olun.
    - Komut dosyasında bir özel durum oluşursa, çalışan komut dosyasını durdurur ve görev başarısız olarak gösterir.
    - Bir hata oluşursa, komut dosyasını geri kalanı çalıştırmaz.
    - Planlanmamış bir yük devretme çalıştırdığınızda, bir hata oluşursa, kurtarma planı devam eder.
    - Planlanmış bir yük devretme çalıştırdığınızda, bir hata oluşursa, kurtarma planı durdurur. Komut dosyası, beklendiği gibi çalışır onay ve kurtarma planı yeniden çalıştırın düzeltin.
        - `Write-Host` Komutu, bir kurtarma planı betiği çalışmıyor. Kullanırsanız `Write-Host` komutu bir betikte, komut dosyası başarısız olur. Çıktı oluşturmak için sırayla ana komut çalıştıran bir proxy komut dosyası oluşturun. Tüm çıktı çıkışı varsayılır emin olmak için kullanın  **\> \>**  komutu.
        - 600 saniye içinde döndürmezse komut dosyası zaman aşımına uğradı.
        - Herhangi bir şey STDERR yazılmışsa, komut dosyası başarısız olarak sınıflandırılır. Bu bilgiler, komut dosyası yürütme ayrıntılar görüntülenir.

* Bir kurtarma planı betiklerde VMM hizmet hesabı bağlamında çalışır. Bu hesap komut dosyasının bulunduğu uzak paylaşımı için izinleri okuduğundan emin olun. VMM hizmet hesabıyla aynı düzeyde kullanıcı hakları ile çalıştırmak için komut dosyası sınayın.
* VMM cmdlet'lerini Windows PowerShell modülündeki teslim edilir. VMM konsolunu yüklediğinizde modülü yüklenir. Komut dosyanıza modülünü yüklemek için komut dosyasında aşağıdaki komutu kullanın: 

    `Import-Module -Name virtualmachinemanager`

    Daha fazla bilgi için bkz: [Windows PowerShell ve VMM ile çalışmaya başlama](https://technet.microsoft.com/library/hh875013.aspx).
* VMM dağıtımınızda en az bir kitaplık sunucusu olduğundan emin olun. Varsayılan olarak, bir VMM sunucusu kitaplık paylaşım yolunu yerel olarak VMM sunucusunda bulunur. Klasör MSCVMMLibrary adıdır.

  Kitaplık paylaşım yolu uzaktan (veya yerel ancak MSCVMMLibrary ile paylaşılan olup olmadığını) ise, kullanarak paylaşım aşağıdaki gibi yapılandırın \\libserver2.contoso.com\share\ bir örnek olarak:
  
  1. Kayıt Defteri Düzenleyicisi'ni açın ve gidin **HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure Site Recovery\Registration**.

  2. Değeri değiştirme **ScriptLibraryPath** için  **\\\libserver2.contoso.com\share\\**. Tam FQDN'yi belirtin. Paylaşım konumunu izinleri sağlar. Bu paylaşımın kök düğümdür. VMM, bir kök düğüm olup olmadığını denetlemek için Kitaplığı'nda kök düğümüne gidin. Açılan yolun kökü yoludur. Bu değişken kullanmanız yoludur.

  3. Komut dosyası, VMM hizmet hesabıyla aynı düzeyde kullanıcı hakları olan bir kullanıcı hesabı kullanarak test. Bu kullanıcı haklarını kullanarak, tek başına, test edilmiş komut dosyalarının kurtarma planları çalıştırdıkları aynı şekilde çalışmalarını doğrular. VMM sunucusunda, aşağıdaki gibi atlamak için yürütme ilkesini ayarlayın:

     a. Açık **64-bit Windows PowerShell** konsolunu yönetici olarak.
     
     b. Girin **Set-executionpolicy bypass**. Daha fazla bilgi için bkz: [Set-ExecutionPolicy cmdlet'ini kullanarak](https://technet.microsoft.com/library/ee176961.aspx).

     > [!IMPORTANT]
     > Ayarlama **Set-executionpolicy bypass** yalnızca 64-bit PowerShell konsolundaki. Komut dosyaları için 32-bit PowerShell konsolunu ayarlarsanız çalıştırmayın.

## <a name="add-the-script-to-the-vmm-library"></a>VMM kitaplığı komut dosyası ekleme

Bir VMM kaynak site varsa, VMM sunucusunda bir komut dosyası oluşturabilirsiniz. Ardından, komut dosyası kurtarma planınıza dahil.

1. Kitaplık paylaşımında yeni bir klasör oluşturun. Örneğin, \<VMM Sunucu adı > \MSSCVMMLibrary\RPScripts. Kaynak klasöre yerleştirin ve VMM sunucuları hedefleyebilirsiniz.
2. Komut dosyası oluşturun. Örneğin, komut dosyası RPScript adlandırın. Betiği beklendiği gibi çalıştığını doğrulayın.
3. Komut dosyasında yerleştirin \<VMM Sunucu adı > kaynak ve hedef VMM sunucularında \MSSCVMMLibrary klasör.

## <a name="add-the-script-to-a-recovery-plan"></a>Bir kurtarma planı için komut dosyası ekleme

Sanal makineler veya çoğaltma gruplarına bir kurtarma planına eklenen ve planı oluşturduktan sonra komut dosyası grubuna ekleyebilirsiniz.

1. Kurtarma planı açın.
2. İçinde **adım** listesinde, bir öğe seçin. Ardından, şunlardan birini seçin **betik** veya **el ile eylemi**.
3. Komut dosyası ya da önce veya sonra seçili öğe eylem eklenip eklenmeyeceğini belirtin. Komut dosyası konumunu yukarı veya aşağı taşımak için seçin **Yukarı Taşı** ve **Aşağı Taşı** düğmeler.
4. VMM komut dosyası eklerseniz, seçin **VMM komut dosyası için yük devretme**. İçinde **betik yolu**, paylaşımının göreli yolunu girin. Örneğin **\RPScripts\RPScript.PS1**.
5. Bir Azure Otomasyonu runbook'u eklerseniz, runbook bulunduğu Otomasyon hesabı belirtin. Sonra kullanmak istediğiniz Azure runbook komut dosyasını seçin.
6. Betiği beklendiği gibi çalıştığından emin olmak için bir yük devretme testi kurtarma planı yapın.


## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [yerine çalıştıran](site-recovery-failover.md).

