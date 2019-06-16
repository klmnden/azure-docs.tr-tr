---
title: StorSimple Snapshot Manager nedir? | Microsoft Docs
description: StorSimple Snapshot Manager mimarisini ve özelliklerini açıklar.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: ''
ms.assetid: 6094c31e-e2d9-4592-8a15-76bdcf60a754
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: v-sharos
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3f7436bb63f52c9c2b697c8e7031922ce89d786b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60789629"
---
# <a name="an-introduction-to-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager giriş

## <a name="overview"></a>Genel Bakış
StorSimple Snapshot Manager, veri koruma ve Microsoft Azure StorSimple ortamında yedekleme yönetimini basitleştiren bir Microsoft Yönetim Konsolu (MMC) ek bileşenidir. StorSimple Snapshot Manager ile Microsoft Azure StorSimple veri veri merkezinde hem de bulutta bir tek bir tümleşik depolama çözümü olarak yedekleme süreçlerini basitleştirir ve maliyetlerin düşürülmesi yönetebilirsiniz.

Bu genel bakış StorSimple Snapshot Manager sunar, özelliklerini ve Microsoft Azure StorSimple, rolde açıklanmaktadır. 

SharePoint için StorSimple cihaz, StorSimple Yöneticisi hizmeti, StorSimple Snapshot Manager ve StorSimple bağdaştırıcısını dahil olmak üzere tüm Microsoft Azure StorSimple sistemine genel bakış için bkz. [StorSimple 8000 serisi: bir karma bulut depolama çözümü](storsimple-overview.md). 

> [!NOTE]
> * Microsoft Azure StorSimple sanal (diğer adıyla StorSimple sanal cihazları şirket) dizilerini yönetmek için StorSimple Snapshot Manager'ı kullanamazsınız.
> * StorSimple Cihazınızda StorSimple güncelleştirme 2 yüklemeyi planlıyorsanız, StorSimple Snapshot Manager'ın en son sürümünü indirip yüklemeniz mutlaka **StorSimple güncelleştirme 2'yi yüklemeden önce**. StorSimple Snapshot Manager'ın en son sürümü, geriye dönük uyumludur ve Microsoft Azure StorSimple yayımlanan tüm sürümleri ile çalışır. StorSimple Snapshot Manager'ın önceki sürümü kullanıyorsanız (, yeni sürümü yüklemeden önce önceki sürümü kaldırmanız gerekmez) güncelleştirmeniz gerekir.
> 
> 

## <a name="storsimple-snapshot-manager-purpose-and-architecture"></a>StorSimple Snapshot Manager amacı ve mimarisi
StorSimple Snapshot Manager tutarlı, oluşturmak için kullanabileceğiniz bir Merkezi Yönetim Konsolu sağlar-belirli bir noktaya yedekleme yerel kopyasını ve bulut veri. Örneğin, konsola kullanabilirsiniz:

* Yapılandırma, yedekleme ve birimleri silin.
* Birim gruplarını emin olmak için yapılandırma uygulamayla tutarlı yedeklenen verileri.
* Yedekleme ilkelerini yönetme, böylece verileri önceden belirlenmiş bir zamanlamayla yedeklenir.
* Yerel oluşturmak ve bulutta depolanan ve olağanüstü durum kurtarma için kullanılan anlık görüntüleri bulut.

StorSimple Snapshot Manager konaktaki VSS sağlayıcısı ile kayıtlı uygulamaların listesini getirir. Sonra uygulamayla tutarlı yedeklemeler oluşturmak için bir uygulama tarafından kullanılan birimleri denetler ve birim gruplarını yapılandırmak için önerir. StorSimple Snapshot Manager uygulama tutarlı yedekleme kopyalarını oluşturmak için bu birim gruplarını kullanır. (Tüm ilgili dosyalar ve veritabanları eşitlenir ve gerçek zaman belirli bir noktada uygulama durumunu temsil eden uygulama tutarlılığı bulunur.) 

StorSimple Snapshot Manager yedeklemeleri değişiklikleri yalnızca son yedeklemeden bu yana yakalama artımlı anlık görüntüleri biçiminde. Sonuç olarak, yedeklemeler daha az depolama gerektirir ve oluşturulabilir ve hızlı bir şekilde geri. StorSimple Snapshot Manager anlık görüntüler, uygulama ile tutarlı verileri yakalar emin olmak için Windows Birim Gölge Kopyası Hizmeti (VSS) kullanır. (Daha fazla bilgi için Windows birim gölge kopyası hizmeti bölümü ile tümleştirme için gidin.) StorSimple Snapshot Manager ile yedekleme zamanlamaları oluşturabilir veya gerektiğinde anında yedek alabilir. Yedekleme, StorSimple Snapshot Manager sağlar verileri geri yüklemeniz gerekiyorsa bir katalog yerel veya Bulut anlık görüntüleri seçin. Azure StorSimple veri kullanılabilirliği geri yükleme işlemleri sırasında gecikmeler önleyen, gerektiğinde gereken yalnızca verileri yükler.)

![StorSimple Snapshot Manager mimarisi](./media/storsimple-what-is-snapshot-manager/HCS_SSM_Overview.png)

**StorSimple Snapshot Manager mimarisi** 

## <a name="support-for-multiple-volume-types"></a>Birden çok birim türleri için destek
StorSimple Snapshot Manager'ı yapılandırmak ve birimler aşağıdaki türlerini yedekleme için kullanabilirsiniz: 

* **Temel birimler** – bir temel disk üzerindeki tek bir bölüm temel birimdir. 
* **Basit birimler** – tek bir dinamik diskteki disk alanı içeren bir dinamik birime basit bir birimdir. Basit bir birimi bir disk üzerinde tek bir bölge veya aynı birimde birbirine bağlanmış birden çok bölgede oluşur. (Yalnızca dinamik diskler üzerinde basit birimler oluşturabilirsiniz.) Basit birimler, hataya dayanıklı değildir.
* **Dinamik birimler** – dinamik birim dinamik bir disk üzerinde oluşturulan bir birimdir. Dinamik diskler, bir bilgisayarda bulunan dinamik disklerde bulunan birimler hakkındaki bilgileri izlemek için bir veritabanı kullanın. 
* **Yansıtma ile dinamik birimler** – dinamik birimler yansıtma ile RAID 1 mimarisine oluşturulur. RAID 1 ile yansıtılmış bir kümesini üreten iki veya daha fazla disk aynı veriler yazılır. Okuma isteği, ardından istenen verileri içeren herhangi bir disk tarafından işlenebilir.
* **Küme Paylaşılan birimleri** – Küme Paylaşılan birimleri (CSV), yük devretme kümesi can aynı anda okuma veya yazma aynı disk birden fazla düğüm. Sürücü sahipliğinin veya bağlama, kaldırma ve birim kaldırıldığında bir değişiklik gerektirmeden bir düğümden başka bir düğüme yük devretme hızlı bir şekilde ortaya çıkabilir. 

> [!IMPORTANT]
> Csv ve CSV olmayan aynı anlık karıştırmayın. Csv ve CSV olmayan bir anlık görüntüdeki karıştırma desteklenmiyor. 
> 
> 

Birimin tamamı gruplar geri veya ayrı birimleri Kopyala ve tek tek dosyaları kurtarmak için StorSimple Snapshot Manager'ı kullanabilirsiniz.

* [Birim ve birim gruplarını](#volumes-and-volume-groups) 
* [Yedekleme türleri ve yedekleme ilkeleri](#backup-types-and-backup-policies) 

StorSimple Snapshot Manager özelliklerini ve bunların nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [StorSimple Snapshot Manager kullanıcı arabirimini](storsimple-use-snapshot-manager.md).

## <a name="volumes-and-volume-groups"></a>Birim ve birim gruplarını
Birimler oluşturabilir ve ardından bunları gruplar halinde toplu yapılandırmanız ile StorSimple Snapshot Manager. 

StorSimple Snapshot Manager uygulama tutarlı yedek kopya oluşturmak için birim gruplarını kullanır. Tüm ilgili dosyalar ve veritabanları eşitlenir ve gerçek zaman belirli bir noktada bir uygulamanın durumunu temsil eden uygulama tutarlılığı bulunmaktadır. Birim gruplarını (olarak da bilinen olduğu *tutarlılık grupları*) tahminlerde bir yedekleme veya geri yükleme işi.

Birim gruplarını birim kapsayıcıları ile aynı değildir. Birim kapsayıcısı, bir bulut depolama hesabını ve şifreleme ve bant genişliği tüketimi gibi diğer özniteliklere sahip bir veya daha fazla birimlerini içerir. Tek bir birim kapsayıcısı, 256 adede kadar ölçülü kaynak sağlanan StorSimple birimlerini içerebilir. Birim kapsayıcıları hakkında daha fazla bilgi için Git [, birim kapsayıcıları yönetme](storsimple-manage-volume-containers.md). Birim gruplarını, yedekleme işlemlerini kolaylaştırmak için yapılandırdığınız birimleri koleksiyonlarıdır. Farklı birim kapsayıcılarına ait, bunları bir tek birim grubunda yerleştirin ve ardından bu birim grubu için bir yedekleme ilkesi oluşturma iki birimi seçin, her birim uygun bir depolama hesabı kullanarak uygun birim kapsayıcısında yedeklenir.

> [!NOTE]
> Bir birim grubu tüm birimleri, bir tek bulut hizmeti sağlayıcısından gelmelidir.
> 
> 

## <a name="integration-with-windows-volume-shadow-copy-service"></a>Windows birim gölge kopyası hizmeti ile tümleştirme
StorSimple Snapshot Manager uygulama ile tutarlı verileri yakalamak için Windows Birim Gölge Kopyası Hizmeti (VSS) kullanır. VSS uygulama tutarlılığı artımlı anlık oluşturulmasını koordine etmek için VSS kullanabilen uygulamalarla iletişim kurarak kolaylaştırır. VSS anlık görüntüler alındığında uygulamaların geçici olarak devre dışı veya sessiz, olmasını sağlar. 

StorSimple Snapshot Manager uygulamasını VSS genel NTFS birimlerinin ve SQL Server ile çalışır. İşlemi aşağıdaki gibidir: 

1. Genellikle bir veri yönetimi ve koruma çözümü (gibi StorSimple Snapshot Manager) veya bir yedekleme uygulaması olduğundan, bir istek sahibi, VSS çağırır ve hedef uygulamada yazan yazılım bilgileri toplamak için sorar.
2. VSS yazıcısı bileşeni açıklamasını veri almak için iletişim kurar. Yazıcı yedeklenecek veriler açıklamasını döndürür. 
3. VSS Yazıcı Yedekleme için uygulama hazırlama bildirir. Yazıcı verileri yedekleme için işlem günlüklerini ve benzeri güncelleştirme açık işlemlerin tamamlayarak hazırlar ve sonra VSS'yi bildirir
4. VSS Yazıcı geçici olarak durdurun uygulamanın veri depoları ve gölge kopya oluşturulurken veri birimine yazıldığında emin olmak için yönlendirir. Bu adım, veri tutarlılığı sağlar ve en fazla 60 saniye sürer.
5. VSS gölge kopya oluşturmak için sağlayıcıyı yönlendirir. Yazılım veya donanım tabanlı, olabilen sağlayıcıları, isteğe bağlı olarak bunları gölge kopyalarını oluşturmanız ve mevcut durumda çalışmakta olan birimleri yönetin. Sağlayıcı, gölge kopya oluşturur ve VSS tamamlandığında size bildirir.
6. VSS Yazıcı g/ç sürdürebilirsiniz uygulamaya bildirmek ve g/ç gölge kopya oluşturma sırasında başarıyla duraklatıldı onaylamak için iletişim kurar. 
7. Kopyalama başarılı olduysa, VSS istek sahibine kopyanın konumunu döndürür. 
8. Gölge kopya oluşturulduğu sırada veri yazılmışsa, yedekleme tutarsız olur. VSS gölge kopya siler ve istek sahibine bildirir. İstek sahibine otomatik olarak yedekleme işlemi yineleyin veya daha sonraki bir zamanda yeniden denemek yöneticinize bildirin.

Aşağıdaki resme bakın.

![VSS işlemi](./media/storsimple-what-is-snapshot-manager/HCS_SSM_VSS_process.png)

**Windows birim gölge kopyası hizmeti işlemi** 

## <a name="backup-types-and-backup-policies"></a>Yedekleme türleri ve yedekleme ilkeleri
StorSimple Snapshot Manager ile verileri yedeklemek ve yerel olarak ve bulutta depolayın. Hemen verileri yedeklemek için StorSimple Snapshot Manager kullanmak ya da otomatik olarak yedekler almak için bir zamanlama oluşturmak için bir yedekleme İlkesi kullanabilirsiniz. Yedekleme ilkelerini ayrıca kaç anlık görüntüleri korunur belirtmenize olanak verir. 

### <a name="backup-types"></a>Yedekleme türleri
StorSimple Snapshot Manager'ı yedekleme aşağıdaki türleri oluşturmak için kullanabilirsiniz:

* **Yerel anlık görüntüleri** – StorSimple cihazında depolanan toplu veri, zaman içinde nokta kopyalarını yerel anlık görüntüleridir. Genellikle, bu yedekleme türüne oluşturulabilir ve hızlı bir şekilde geri yüklendi. Yerel bir yedek kopya gibi bir yerel anlık görüntü kullanabilirsiniz.
* **Bulut anlık görüntüleri** – bulut anlık görüntüleri olan bulutta depolanan toplu veri, zaman içinde nokta kopyalarını. Bir bulut anlık görüntüsünü, farklı ve şirket dışı depolama sisteminde çoğaltılan bir anlık görüntü eşdeğerdir. Bulut anlık görüntüleri, olağanüstü durum kurtarma senaryolarda özellikle yararlıdır.

### <a name="on-demand-and-scheduled-backups"></a>İsteğe bağlı hem de zamanlanmış yedeklemeler
StorSimple Snapshot Manager ile hemen oluşturulacak tek seferlik bir yedekleme başlatabilir veya yinelenen yedekleme işlemleri zamanlamak için bir yedekleme İlkesi kullanabilirsiniz.

Bir yedekleme İlkesi, düzenli Yedeklemeler zamanlamak için kullanabileceğiniz otomatik kurallar kümesidir. Bir yedekleme İlkesi belirli bir birim grubu anlık görüntüsünü almak için parametreler ve sıklığı tanımlamanızı sağlar. Başlangıç ve sona erme tarihleri, zamanları, sıklığı ve bekletme gereksinimleri, hem yerel belirtmek için ilkeleri kullanın ve bulut anlık görüntüleri. Tanımladığınız hemen sonra bir ilke uygulanır. 

StorSimple Snapshot Manager'ı yedekleme ilkelerini gerektiğinde yeniden yapılandırın veya yapılandırmak için kullanabilirsiniz. 

Oluşturduğunuz her bir yedekleme ilkesi için aşağıdaki bilgileri yapılandırın:

* **Adı** – seçilen yedekleme İlkesi benzersiz adı.
* **Tür** – yedekleme İlkesi; yerel anlık görüntü veya Bulut anlık görüntüsü türü.
* **Birim grubu** – seçilen yedekleme İlkesi atandığı birim grubu.
* **Bekletme** – korumak için yedek kopya sayısı. Size onay **tüm** kutusunda, birim başına yedek kopyaların sayısı üst sınırına kadar tüm yedekleme kopyası saklanır, bu noktada ilke başarısız olur ve bir hata iletisi oluşturur. Alternatif olarak, bir (1 ile 64 arasında) korumak için yedekleme sayısı belirtebilirsiniz.
* **Tarih** – yedekleme ilkesini ne zaman oluşturulduğu tarih.

Yedekleme ilkelerini yapılandırma hakkında daha fazla bilgi için Git [oluşturmak ve yedekleme ilkelerini yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-manage-backup-policies.md).

### <a name="backup-job-monitoring-and-management"></a>Yedekleme işi izleme ve yönetim
StorSimple Snapshot Manager'ı izlemek ve yaklaşan, zamanlanan ve tamamlanan yedekleme işlerini yönetmek için kullanabilirsiniz. Ayrıca, StorSimple Snapshot Manager en fazla 64 tamamlanmış yedekleme kataloğu sağlar. Katalog bulun ve birimleri veya tek tek dosyaları geri yüklemek için kullanabilirsiniz. 

Yedekleme işlerini izleme hakkında daha fazla bilgi için Git [yedekleme işleri görüntüleme ve yönetme için StorSimple anlık görüntü Yöneticisi'ni kullanın](storsimple-snapshot-manager-manage-backup-jobs.md).

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [StorSimple çözümünüzü yönetmek için StorSimple anlık görüntü Yöneticisi'ni kullanarak](storsimple-snapshot-manager-admin.md).
* İndirme [StorSimple Snapshot Manager](https://www.microsoft.com/download/details.aspx?id=44220).

