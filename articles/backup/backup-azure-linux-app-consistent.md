---
title: "Azure yedekleme: uygulama tutarlı Linux VM'ler yedeklerini | Microsoft Docs"
description: "Linux sanal makinelerinizin Azure uygulamayla tutarlı yedeklemeler oluşturun. Bu makalede, Azure dağıtılan Linux Vm'leri yedekleme için komut dosyası framework yapılandırma açıklanmaktadır. Bu makalede, sorun giderme bilgileri de içerir."
services: backup
documentationcenter: dev-center-name
author: anuragmehrotra
manager: shivamg
keywords: "uygulamayla tutarlı yedekleme; Uygulama tutarlı Azure VM backup; Linux VM yedekleme; Azure yedekleme"
ms.assetid: bbb99cf2-d8c7-4b3d-8b29-eadc0fed3bef
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 1/12/2018
ms.author: anuragm;markgal
ms.openlocfilehash: c2437b4cd90deda3e7239d87837a47a072f52835
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/13/2018
---
# <a name="application-consistent-backup-of-azure-linux-vms"></a>Uygulama tutarlı yedekleme Azure Linux VM'ler

Vm'leriniz yedekleme anlık görüntülerini alırken, uygulama tutarlılığı geri sonra VM'ler önyükleme uygulamalarınızı başlattığınızda anlamına gelir. Uygulama tutarlılığı hayal edebildiğiniz gibi son derece önemlidir. Emin olmak için uygulama tutarlı, uygulamayla tutarlı yedeklemeler almak için Linux öncesi betiğini ve sonrası betik framework kullanabilirsiniz, Linux VM'ler şunlardır. Öncesi betiğini ve sonrası betik framework Azure Resource Manager tarafından dağıtılan Linux sanal makineleri destekler. Uygulama tutarlılığı için komut dosyalarını Service Manager tarafından dağıtılan sanal makineler ya da Windows sanal makineleri desteklemez.

## <a name="how-the-framework-works"></a>Framework nasıl çalışır?

Framework özel öncesi betikleri çalıştırmak için bir seçenek sağlar ve VM anlık görüntülerini alırken sonrası komut dosyaları. Yalnızca VM anlık görüntü ve VM anlık alma hemen sonra çalıştırın sonrası betikleri olabilmesi öncesi betikleri çalıştırın. Öncesi ve sonrası komut dosyalarını VM anlık görüntülerini alırken uygulamanız ve ortamınız, denetleme esnekliği sağlar.

Öncesi komut dosyaları yerel uygulama API'leri, hangi sessiz moda IOs çağırır ve bellek içi içerik diske boşaltır. Bu Eylemler, anlık görüntü uygulama tutarlı olduğundan emin olun. Sonrası komut dosyalarını normal işlemler VM anlık görüntü sonrası sürdürmek uygulama sağlar IOs çözme için yerel uygulama API'leri kullanın.

## <a name="steps-to-configure-pre-script-and-post-script"></a>Öncesi betiğini ve sonrası betik yapılandırma adımları

1. Yedeklemek istediğiniz Linux VM kök kullanıcı olarak oturum açın.

2. Gelen [GitHub](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig), indirme **VMSnapshotScriptPluginConfig.json** ve kopyalayın **/etc/azure** yedeklemek istediğiniz tüm VM'ler için klasör. Varsa **/etc/azure** klasörü mevcut değil, oluşturun.

3. Öncesi betiği ve yedekleme planladığınız tüm sanal makineleri üzerinde uygulamanız için sonrası betiği kopyalayın. Komut dosyaları, VM üzerinde herhangi bir konuma kopyalayabilirsiniz. Komut dosyalarında tam yolunu güncelleştirdiğinizden emin olun **VMSnapshotScriptPluginConfig.json** dosya.

4. Bu dosyalar için aşağıdaki izinleri emin olun:

   - **VMSnapshotScriptPluginConfig.json**: izni "600." Örneğin, yalnızca "root" kullanıcı "Okuma" ve "yazma" izinleri bu dosyaya olmalıdır ve hiçbir kullanıcı "Yürütme izinleri" olması gerekir.

   - **Öncesi betiği**: izni "700."  Örneğin, yalnızca "root" kullanıcı olmalıdır "Okuma", "yazma" ve "Yürütme izinleri bu dosyaya".
  
   - **Sonrası betik** izni "700." Örneğin, yalnızca "root" kullanıcı olmalıdır "Okuma", "yazma" ve "Yürütme izinleri bu dosyaya".

   > [!Important]
   > Çerçeve, kullanıcıların çok fazla güç sağlar. Framework güvenli ve yalnızca "root" kullanıcı kritik JSON ve komut dosyalarını erişimi olduğundan emin olun.
   > Gereksinimleri karşılanmadığı takdirde komut dosyası çalıştırılmadığından, bir dosya sistemi kilitlenme ve tutarsız yedekleme sonuçlanır.
   >

5. Yapılandırma **VMSnapshotScriptPluginConfig.json** burada açıklandığı gibi:
    - **pluginName**: Bu alanı olduğu gibi bırakın ya da komut dosyalarınızı beklendiği gibi çalışmayabilir.

    - **preScriptLocation**: yedeklenecek giderek VM üzerinde öncesi komut dosyasının tam yolunu girin.

    - **postScriptLocation**: yedeklenecek giderek VM'de sonrası komut dosyasının tam yolunu girin.

    - **preScriptParams**: öncesi komut dosyasına geçirilmesi gereken isteğe bağlı parametreleri sağlamak. Tüm parametreleri tırnak işaretleri içinde olmalıdır. Birden çok parametre kullanırsanız, Parametreler virgül ile ayırın.

    - **postScriptParams**: sonrası komut dosyasına geçirilmesi gereken isteğe bağlı parametreleri sağlamak. Tüm parametreleri tırnak işaretleri içinde olmalıdır. Birden çok parametre kullanırsanız, Parametreler virgül ile ayırın.

    - **preScriptNoOfRetries**: öncesi betiği yeniden varsa herhangi bir hata sonlandırmadan önce sayısını ayarlayın. Sıfır anlamına gelir tek deneyin ve bir hata olduğunda hiçbir yeniden deneyin.

    - **postScriptNoOfRetries**: sonrası betik yeniden varsa herhangi bir hata sonlandırmadan önce sayısını ayarlayın. Sıfır anlamına gelir tek deneyin ve bir hata olduğunda hiçbir yeniden deneyin.
    
    - **SaniyeOlarakZamanAşımıSüresi**: tek tek zaman aşımı öncesi komut dosyası ve sonrası komut dosyasını belirtin.

    - **continueBackupOnFailure**: Bu değer ayarlanırsa **doğru** bir dosya sisteminin tutarlı/kilitlenme tutarlı yedekleme öncesi betik, geri dönüş veya başarısız sonrası komut dosyası için Azure Backup istiyorsanız. Bu ayar **false** (dışında bu ayardan bağımsız olarak kilitlenme tutarlı yedekleme geri döner tek disk VM olduğunda) komut dosyası hatası durumunda yedekleme başarısız olur.

    - **fsFreezeEnabled**: dosya sistemi tutarlılığı sağlamak için VM anlık görüntü alırken, Linux fsfreeze adlı olup olmadığını belirtin. Ayarlamak bu ayarı tutma öneririz **true** uygulamanız fsfreeze devre dışı bırakma bir bağımlılık olmadıkça.

6. Komut dosyası framework artık yapılandırılmıştır. VM yedekleme zaten yapılandırılmışsa, sonraki yedekleme betikleri çağırır ve uygulama tutarlı yedekleme tetikler. VM yedekleme yapılandırılmamışsa, kullanarak yapılandırma [kurtarma Hizmetleri kasaları için Azure sanal makineleri yedekleyin.](https://docs.microsoft.com/azure/backup/backup-azure-vms-first-look-arm)

## <a name="troubleshooting"></a>Sorun giderme

Öncesi betiği ve sonrası komut dosyası yazılırken uygun günlüğe kaydetme ekleyin ve komut dosyası sorunları gidermek için komut dosyası günlüklerinizi gözden emin olun. Komut dosyası çalıştırarak sorunlarını yaşamaya devam ediyorsanız, daha fazla bilgi için aşağıdaki tabloya bakın.

| Hata | Hata iletisi | Önerilen eylem |
| ------------------------ | -------------- | ------------------ |
| Öncesi ScriptExecutionFailed |Yedekleme uygulama tutarlı olmayabilir şekilde öncesi komut dosyası bir hata döndürdü.   | Sorunu düzeltmek, komut dosyası için hata günlüklerine bakın.|  
|   POST ScriptExecutionFailed |    Sonrası betik uygulama durumunu etkileyebilecek bir hata döndürdü. |    Sorunu giderin ve uygulama durumunu denetlemek komut dosyanızı için hata günlüklerine bakın. |
| Öncesi ScriptNotFound |  Öncesi komut dosyası belirtilen konumda bulunamadı **VMSnapshotScriptPluginConfig.json** yapılandırma dosyası. |   Yapma emin bu öncesi betiği uygulama tutarlı yedekleme emin olmak için yapılandırma dosyasında belirtilen yolda bulunur.|
| POST ScriptNotFound | Sonrası betiğin belirtilen konumda bulunamadı **VMSnapshotScriptPluginConfig.json** yapılandırma dosyası. |   Mevcut uygulama tutarlı yedekleme emin olmak için yapılandırma dosyasında belirtilen yolda olduğundan olun emin bu sonrası betik.|
| IncorrectPluginhostFile | **Pluginhost** VmSnapshotLinux uzantısı ile birlikte gelen, dosya bozuk, böylece öncesi betiği ve sonrası komut dosyası çalışamaz ve yedekleme uygulama tutarlı olmaz. | Kaldırma **VmSnapshotLinux** uzantısı ve onu otomatik olarak yeniden yüklenir sorunu gidermek için sonraki yedekleme. |
| IncorrectJSONConfigFile | **VMSnapshotScriptPluginConfig.json** dosyasıdır yanlış, bu nedenle öncesi betiği ve sonrası komut dosyası çalışamaz ve yedekleme uygulama tutarlı olmaz. | Kopyadan karşıdan [GitHub](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig) ve yeniden yapılandırın. |
| InsufficientPermissionforPre-komut dosyası | Komut dosyaları çalıştırmak için "root" kullanıcı dosyanın sahibi olmalıdır ve dosya "700" izinleri olması (diğer bir deyişle, yalnızca "sahip" olmalıdır "Okuma", "yazma" ve "Yürütme izinleri"). | "Kök" kullanıcı "sahibi" komut dosyasını ve yalnızca "sahip" "Okuma", "yazma" ve "yürütme" izinleri olduğundan emin olun. |
| InsufficientPermissionforPost-komut dosyası | Komut dosyaları çalıştırmak için kök kullanıcının dosyanın sahibi olması gerekir ve dosyayı "700" izinleri olmalıdır (diğer bir deyişle, yalnızca "sahip" olmalıdır "Okuma", "yazma" ve "Yürütme izinleri"). | "Kök" kullanıcı "sahibi" komut dosyasını ve yalnızca "sahip" "Okuma", "yazma" ve "yürütme" izinleri olduğundan emin olun. |
| Öncesi ScriptTimeout | Betik yürütme işlemi zaman aşımına uğradı uygulama tutarlı yedekleme öncesi. | Komut dosyasını denetleyin ve zaman aşımı artırmak **VMSnapshotScriptPluginConfig.json** konumunda bulunan dosya **/etc/azure**. |
| POST ScriptTimeout | Uygulama tutarlı yedekleme sonrası betik yürütme işlemi zaman aşımına uğradı. | Komut dosyasını denetleyin ve zaman aşımı artırmak **VMSnapshotScriptPluginConfig.json** konumunda bulunan dosya **/etc/azure**. |

## <a name="next-steps"></a>Sonraki adımlar
[Kurtarma Hizmetleri kasasına VM yedeklemeyi yapılandırma](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms)
