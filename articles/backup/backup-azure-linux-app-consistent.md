---
title: 'Azure yedekleme: uygulamayla tutarlı Linux VM yedekleri'
description: Azure'da Linux sanal makinelerin uygulamayla tutarlı yedeklemeler oluşturun. Bu makalede, Azure tarafından dağıtılan Linux sanal makinelerini yedekleme için betik framework yapılandırma açıklanmaktadır. Bu makale, sorun giderme bilgileri de içerir.
services: backup
author: anuragmehrotra
manager: shivamg
keywords: uygulamayla tutarlı yedekleme; uygulamayla tutarlı Azure VM yedeklemesi; Linux VM yedekleme; Azure yedekleme
ms.service: backup
ms.topic: conceptual
ms.date: 1/12/2018
ms.author: anuragm
ms.openlocfilehash: a81c0b9c87db85771fcecab87c6b9ac88dcbd472
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60641135"
---
# <a name="application-consistent-backup-of-azure-linux-vms"></a>Azure Linux sanal makinelerinin uygulamayla tutarlı yedekleme

Sanal makinelerinizi yedekleme anlık görüntülerini çekerken uygulama tutarlılığı, uygulamalarınızın geri sonra VM'lerin önyükleme başlattığınızda anlamına gelir. Uygulama tutarlılığı hayal edebileceğiniz gibi son derece önemlidir. Emin olmak için uygulamayla tutarlı Linux betik öncesi ve betik sonrası çerçeve uygulamayla tutarlı yedeklemeler almak için kullanabileceğiniz Linux Vm'lerinizi olan. Betik öncesi ve betik sonrası çerçeve Azure Resource Manager tarafından dağıtılan Linux sanal makineleri destekler. Uygulama tutarlılığı için betikler, Service Manager ile dağıtılan sanal makineleri veya Windows sanal makineleri desteklemez.

## <a name="how-the-framework-works"></a>Framework nasıl çalışır?

Framework özel öncesi komut dosyaları çalıştırmak için bir seçenek sağlar ve VM anlık görüntüleri alırken sonrası komut dosyaları. VM anlık görüntüsü ve VM anlık hemen sonra çalıştırılan sonrası betikler yapmadan önce ön betiklerini çalıştırın. Öncesi ve sonrası komut dosyalarını uygulamanız ve ortamınız, VM anlık görüntüleri alırken denetleme esnekliği sağlar.

Ön betiklerini IOs yerel uygulama hangi sessiz moda alın API'leri çağırmak ve bellek içi içeriğini diske boşaltır. Bu Eylemler, uygulamayla tutarlı anlık görüntü olduğundan emin olun. Sonrası betikler, normal işlemleri VM anlık görüntü sonrası devam etmek uygulamayı etkinleştirir IOs çözme yerel uygulama API'leri kullanın.

## <a name="steps-to-configure-pre-script-and-post-script"></a>Betik öncesi ve betik sonrası yapılandırma adımları

1. Yedeklemek istediğiniz Linux VM için kök kullanıcı olarak oturum açın.

2. Gelen [GitHub](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig), indirme **VMSnapshotScriptPluginConfig.json** ve kopyalayıp **/etc/azure** yedeklemek istediğiniz tüm VM'lerin klasörü. Varsa **/etc/azure** klasörü mevcut değil, oluşturun.

3. Betik öncesi ve sonrası betik uygulamanız yedeklemeyi planladığınız tüm sanal makineler için kopyalayın. Betikler, VM üzerinde herhangi bir konuma kopyalayabilirsiniz. Komut dosyalarında tam yolunu güncelleştirdiğinizden emin olun **VMSnapshotScriptPluginConfig.json** dosya.

4. Bu dosyalar için şu izinler olduğundan emin olun:

   - **VMSnapshotScriptPluginConfig.json**: "600" izni Örneğin, yalnızca "Kök" kullanıcısı, "Okuma" ve "yazma" izinleri bu dosyaya olmalıdır ve kullanıcı yok "Yürütme izinleri" sahip olmalıdır.

   - **Ön betik dosyası**: "700" izni  Örneğin, yalnızca "Kök" kullanıcının olmalıdır "Okuma", "yazma" ve "Yürütme izinleri bu dosyaya".

   - **Son betik** izni "700." Örneğin, yalnızca "Kök" kullanıcının olmalıdır "Okuma", "yazma" ve "Yürütme izinleri bu dosyaya".

   > [!Important]
   > Framework kullanıcılara çok güç sağlar. Framework güvenli ve yalnızca "Kök" kullanıcının kritik JSON ve komut dosyaları erişimi olduğundan emin olun.
   > Gereksinimleri karşılanmadığı takdirde betik çalıştırılmadığından, bir dosya sistemi kilitlenme ve tutarsız yedekleme ile sonuçlanır.
   >

5. Yapılandırma **VMSnapshotScriptPluginConfig.json** burada açıklandığı gibi:
    - **pluginName**: Bu alanı olduğu gibi bırakın ya da komut dosyalarınızı beklendiği gibi çalışmayabilir.

    - **preScriptLocation**: Yedeklenecek geçiyor VM'de öncesi komut dosyasının tam yolunu sağlayın.

    - **postScriptLocation**: Yedeklenecek geçiyor VM'de sonrası komut dosyasının tam yolunu sağlayın.

    - **preScriptParams**: Öncesi betiğe geçirilmesi gereken isteğe bağlı parametreleri sağlamak. Tüm parametreler, tırnak işaretleri içinde olmalıdır. Birden çok parametre kullanırsanız, Parametreler virgül ile ayırın.

    - **postScriptParams**: Sonrası betiğe geçirilmesi gereken isteğe bağlı parametreleri sağlamak. Tüm parametreler, tırnak işaretleri içinde olmalıdır. Birden çok parametre kullanırsanız, Parametreler virgül ile ayırın.

    - **preScriptNoOfRetries**: Ön betik bir hata varsa sonlandırmadan önce denenmesini sayısını ayarlayın. Sıfır anlamına gelir tek deneyin ve bir hata olursa yeniden deneme yok.

    - **postScriptNoOfRetries**:  Varsa herhangi bir hata sonlandırmadan önce sonrası betiği yeniden denenmesi gerektiğini sayısını ayarlayın. Sıfır anlamına gelir tek deneyin ve bir hata olursa yeniden deneme yok.

    - **Timeoutınseconds**: Betik öncesi ve sonrası komut dosyası (en yüksek değer 1800 olabilir) için tek bir zaman aşımı belirtin.

    - **continueBackupOnFailure**: Bu değer kümesine **true** bir dosya sistemi tutarlı/kilitlenme tutarlı yedekleme öncesi betik, geri dönüş veya başarısız sonrası komut dosyası için Azure Backup istiyorsanız. Bu ayar **false** (dışında bu ayardan bağımsız olarak kilitlenmeyle tutarlı yedek için geri kalan tek disk VM olduğunda) betiği başarısız olması durumunda yedekleme başarısız olur.

    - **fsFreezeEnabled**: VM anlık görüntüsü, dosya sistemi tutarlılığı güvence altına alırken, Linux fsfreeze adlı olup olmadığını belirtin. Tutma ayarlamak bu ayarı öneririz **true** uygulamanızı fsfreeze devre dışı bırakma bağımlılık sahip olmadığı sürece.

6. Betik framework artık yapılandırılmıştır. VM yedeklemesi zaten yapılandırılmışsa, sonraki yedekleme betiklerini çağırır ve uygulamayla tutarlı yedekleme tetikler. Sanal makine yedekleme yapılandırılmamışsa kullanarak yapılandırma [Azure sanal makinelerini kurtarma Hizmetleri kasalarına yedekleme.](https://docs.microsoft.com/azure/backup/backup-azure-vms-first-look-arm)

## <a name="troubleshooting"></a>Sorun giderme

Ön betik ve son betik yazarken uygun günlük kaydı ekleyin ve betik sorunları düzeltmek için betik günlüklerini gözden geçirme emin olun. Betikleri çalıştırma sorunları hala varsa, daha fazla bilgi için aşağıdaki tabloya bakın.

| Hata | Hata iletisi | Önerilen eylem |
| ------------------------ | -------------- | ------------------ |
| Öncesi ScriptExecutionFailed |Ön betik bir hata döndürdüğünden yedekleme, uygulamayla tutarlı olmayabilir.   | Sorunu düzeltmek betiğinizin hata günlüklerine bakın.|  
|   POST ScriptExecutionFailed |    Son betik uygulama durumunu etkileyebilecek bir hata döndürdü. |    Uygulama durumunu denetleyin ve sorunu düzeltmek betiği için hata günlüklerine bakın. |
| Öncesi ScriptNotFound |  Belirtilen konumda ön betik bulunamadı **VMSnapshotScriptPluginConfig.json** yapılandırma dosyası. |   Uygulamayla tutarlı yedekleme sağlamak için yapılandırma dosyasında belirtilen yolda mevcut olduğundan emin olun, ön betik olun.|
| POST ScriptNotFound | Sonrası betiğin belirtilen konumda bulunamadı **VMSnapshotScriptPluginConfig.json** yapılandırma dosyası. |   Uygulamayla tutarlı yedekleme sağlamak için yapılandırma dosyasında belirtilen yolda mevcut olduğundan emin olun, son betik olun.|
| IncorrectPluginhostFile | **Pluginhost** VmSnapshotLinux uzantısı ile birlikte gelen, dosya, bozuk, betik öncesi ve sonrası betik çalıştıramaz ve yedekleme, uygulamayla tutarlı olmayacaktır. | Kaldırma **VmSnapshotLinux** uzantısı ve otomatik olarak yeniden sorunu düzeltmek üzere sonraki yedeklemeyle birlikte. |
| IncorrectJSONConfigFile | **VMSnapshotScriptPluginConfig.json** dosyasıdır yanlış, bu nedenle ön betik ve sonrası komut dosyası çalıştırılamaz ve yedekleme, uygulamayla tutarlı olmayacaktır. | Kopyadan indirme [GitHub](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig) ve yeniden yapılandırın. |
| InsufficientPermissionforPre betiği | Komut dosyaları çalıştırmak için "Kök" kullanıcısı dosyanın sahibi olmalıdır ve dosya "700" izniniz olmalıdır (diğer bir deyişle, yalnızca "sahibi" olmalıdır "Okuma", "yazma" ve "Yürütme izinleri"). | "Kök" kullanıcısı betik dosyasının "sahibi" ve yalnızca "sahip" "Okuma", "yazma" ve "yürütme" izinleri olduğundan emin olun. |
| InsufficientPermissionforPost betiği | Komut dosyaları çalıştırmak için kök kullanıcı dosyanın sahibi olmalıdır ve dosya "700" izniniz olmalıdır (diğer bir deyişle, yalnızca "sahibi" olmalıdır "Okuma", "yazma" ve "Yürütme izinleri"). | "Kök" kullanıcısı betik dosyasının "sahibi" ve yalnızca "sahip" "Okuma", "yazma" ve "yürütme" izinleri olduğundan emin olun. |
| Öncesi ScriptTimeout | Zaman aşımına uğradı uygulamayla tutarlı yedekleme öncesi betik yürütme. | Betiği denetleyin ve zaman aşımı artırmak **VMSnapshotScriptPluginConfig.json** konumunda bulunan dosya **/etc/azure**. |
| POST ScriptTimeout | Uygulamayla tutarlı yedekleme sonrası betik yürütme zaman aşımına uğradı. | Betiği denetleyin ve zaman aşımı artırmak **VMSnapshotScriptPluginConfig.json** konumunda bulunan dosya **/etc/azure**. |

## <a name="next-steps"></a>Sonraki adımlar
[Bir kurtarma Hizmetleri kasasına VM yedeklemeyi yapılandırma](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms)
