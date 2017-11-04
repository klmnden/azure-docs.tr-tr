---
title: "Azure Site Recovery yük devretme ve kurtarma için kurtarma planları oluşturun | Microsoft Docs"
description: "Oluşturma ve yük devri ve sanal makineleri ve fiziksel sunucuları kurtarmak için Azure Site Recovery kurtarma planlarında özelleştirme açıklar"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 72408c62-fcb6-4ee2-8ff5-cab1218773f2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/25/2017
ms.author: raynew
ms.openlocfilehash: 202e0ac8be36e9156ec16fadc1b722f4eb3d1432
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2017
---
# <a name="create-recovery-plans"></a>Kurtarma planları oluşturma


Bu makalede oluşturma ve kurtarma planları özelleştirme hakkında bilgiler sağlanmaktadır [Azure Site Recovery](site-recovery-overview.md).

Tüm yorumlarınızı ve sorularınızı bu makalenin alt kısmında veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda paylaşabilirsiniz.

 Aşağıdakileri yapmak için kurtarma planları oluşturabilirsiniz:

* Birlikte yük devri ve birlikte başlatma makine grupları tanımlayın.
* Model bağımlılıkları birlikte kurtarma gruplandırarak tarafından makineler arasında Grup planlayın. Örneğin, yük devri ve belirli bir uygulamayı oluşturan hale getirmek için tüm VM'ler bu uygulamaya aynı kurtarma planı grup için Grup.
* Bir yük devretme çalıştırın. Bir test, planlanmış veya planlanmamış yük devretme bir kurtarma planı çalıştırabilirsiniz.


## <a name="create-a-recovery-plan"></a>Kurtarma planı oluşturma

1. Tıklatın **kurtarma planlarına** > **kurtarma planı oluşturma**.
   Kurtarma planı ve kaynak ve hedef için bir ad belirtin. Kaynak konumun sanal makineler, yük devretme ve kurtarma için etkinleştirilmiş olması gerekir.

    - VMM çoğaltma VMM için seçin **kaynak türü** > **VMM**ve kaynak ve hedef VMM sunucuları. Tıklatın **Hyper-V** korunan Bulutları görmek için.
    - Azure'a VMM için seçin **kaynak türü** > **VMM**.  Kaynak VMM sunucusunu seçin ve **Azure** hedefi olarak.
    - (VMM olmadan) azure'a Hyper-V çoğaltma için seçin **kaynak türü** > **Hyper-V sitesi**. Site, kaynak olarak seçin ve **Azure** hedefi olarak.
    - VMware VM ya da Azure fiziksel şirket içi sunucu için bir yapılandırma sunucusu, kaynak olarak seçin ve **Azure** hedefi olarak.
    - Bir Azure için Azure kurtarma planı için kaynak ve hedef olarak ikincil bir Azure bölgesi olarak Azure bölgesini seçin. İkincil Azure bölgeleri yalnızca sanal makineler korumalı dosyalardır.
2. İçinde **sanal makine Seç**, varsayılan grubuna (Grup 1) kurtarma planında eklemek istediğiniz sanal makineleri (veya çoğaltma grubu) seçin.

## <a name="customize-and-extend-recovery-plans"></a>Özelleştirme ve kurtarma planları genişletme

Özelleştirme ve kurtarma planları genişletir:

- **Yeni gruplar eklemek**— ek kurtarma planı grubu (en fazla yedi) varsayılan grubuna ekleyin ve sonra kurtarma planı grupların daha fazla makine ya da çoğaltma grupları ekleyin. Grupları, bunları eklediğiniz sırayla numaralandırılır. Yalnızca bir sanal makine ya da çoğaltma grubu bir kurtarma planı grubuna eklenebilir.
- **İşlemi el ile eklemek**— önce veya sonra bir kurtarma planı Grup çalıştırmak el ile yapılan eylemler ekleyebilirsiniz. Kurtarma planı çalıştığında işlemi el ile eklenen bir noktada durdurur. Bir iletişim kutusu, el ile gerçekleştirilen işlemin tamamlandığını belirtmenizi ister.
- **Bir komut dosyası eklemek**— önce veya sonra bir kurtarma planı Grup çalışan komut dosyaları ekleyebilirsiniz. Bir komut dosyası eklediğinizde, yeni bir grup için Eylemler kümesi ekler. Örneğin, Grup 1 öncesi adımları kümesini adıyla oluşturulur: Grup 1: öncesi adımları. Bu küme tüm öncesi adımları listelenir. Dağıtılan bir VMM sunucunuz varsa, birincil sitede yalnızca bir komut dosyası ekleyebilirsiniz.
- **Azure runbook'ları ekleyin**— Azure runbook'ları içeren kurtarma planları genişletebilirsiniz. Örneğin, görevleri otomatikleştirmek için ya da tek adımlı kurtarma oluşturmak için. [Daha fazla bilgi edinin](site-recovery-runbook-automation.md).

## <a name="add-scripts"></a>Komut dosyaları ekleme

Kurtarma planlarınızı PowerShell komut dosyalarını kullanabilirsiniz.

 - Böylece özel durumlar düzgün biçimde işlenen komut dosyaları try-catch bloklarını kullandığınızdan emin olun.
    - Komut dosyasında bir özel durum ise, çalışmayı durdurur ve görev başarısız olarak gösterir.
    - Bir hata oluşursa, betik, herhangi bir kalan parçası çalıştırmaz.
    - Planlanmamış bir yük devretme çalıştırdığınızda, bir hata oluşursa, kurtarma planı devam eder.
    - Planlanmış bir yük devretme çalıştırdığınızda, bir hata oluşursa, kurtarma planı durdurur. Komut dosyası, beklendiği gibi çalışır onay ve kurtarma planı yeniden çalıştırın çözmeniz gerekir.
- Write-Host komutu, bir kurtarma planı betiği çalışmaz ve komut dosyası başarısız olur. Çıktı oluşturmak için sırayla ana komut çalıştıran bir proxy komut dosyası oluşturun. Tüm çıktı kullanarak varsayılır emin olun >> komutu.
  * 600 saniye içinde döndürmezse komut dosyası zaman aşımına uğradı.
  * Herhangi bir şey STDERR yazılır, komut dosyası başarısız olarak sınıflandırılır. Bu bilgiler, komut dosyası yürütme ayrıntılar görüntülenir.

Dağıtımınızda VMM kullanıyorsanız:

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
> Yalnızca 64-bit powershell yürütme İlkesi Atla olarak ayarlamanız gerekir. İçin 32-bit powershell ayarladıysanız, komut dosyaları değil exeute olur.

## <a name="add-a-script-or-manual-action-to-a-plan"></a>Bir plana bir komut dosyası veya el ile eylemi Ekle

Sanal makineleri veya çoğaltma gruplarına eklenmesi ve planı oluşturuldu sonra bir komut dosyası varsayılan kurtarma planı grubuna ekleyebilirsiniz.

1. Kurtarma planı açın.
2. Bir öğeyi tıklatın **adım** listeleyin ve ardından **betik** veya **el ile eylemi**.
3. Komut dosyası veya eylem önce veya sonra seçili öğe eklemek istediğiniz belirtin. Kullanım **Yukarı Taşı** ve **Aşağı Taşı** komut dosyası konumunu yukarı veya Aşağı Taşı düğmeleri,.
4. VMM komut dosyası eklerseniz, seçin **VMM komut dosyası için yük devretme**. İçinde **betik yolu**, paylaşım göreli yolunu yazın. VMM aşağıdaki örnekte, yolunu belirtin: **\RPScripts\RPScript.PS1**.
5. Defterini çalıştır bir Azure Otomasyonu eklerseniz, runbook bulunduğu Azure Otomasyonu hesabı belirtin ve uygun Azure runbook komut dosyasını seçin.
6. Kurtarma planı betiği beklendiği gibi çalıştığından emin olmak için bir yük devretme yapabilirsiniz.


### <a name="add-a-vmm-script"></a>VMM komut ekleme

Bir VMM kaynak site varsa, VMM sunucusunda bir komut dosyası oluşturabilir ve kurtarma planınıza dahil.

1. Kitaplık paylaşımında yeni bir klasör oluşturun. Örneğin, \<Vmmsunucusuadı > \MSSCVMMLibrary\RPScripts. Kaynağında yerleştirin ve VMM sunucuyu hedefleyebilir.
2. (Örneğin RPScript) komut dosyası oluşturabilir ve beklendiği gibi çalıştığını denetleyin.
3. Komut dosyası konuma yerleştirmek \<Vmmsunucusuadı > kaynak ve hedef VMM sunucularında \MSSCVMMLibrary.


## <a name="next-steps"></a>Sonraki adımlar

[Daha fazla bilgi edinin](site-recovery-failover.md) yük devretme işlemleri çalıştırma hakkında.
