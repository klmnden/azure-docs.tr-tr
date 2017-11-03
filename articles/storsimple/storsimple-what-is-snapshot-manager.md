---
title: StorSimple Snapshot Manager nedir? | Microsoft Belgeleri
description: "StorSimple Snapshot Manager, kendi mimarisi ve özelliklerini açıklar."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 6094c31e-e2d9-4592-8a15-76bdcf60a754
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: v-sharos
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 38c197c7bc57110b29b1d8cb789d5b7310823da2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="an-introduction-to-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager giriş

## <a name="overview"></a>Genel Bakış
StorSimple Snapshot Manager, veri koruma ve Microsoft Azure StorSimple ortamda yedekleme yönetimini basitleştirir Microsoft Yönetim Konsolu (MMC) ek bileşenidir. StorSimple Snapshot Manager ile Microsoft Azure StorSimple verileri veri merkezi ve bulut tek bir tümleşik depolama çözümü olarak, bu nedenle yedekleme işlemlerini basitleştirir ve maliyetlerini azaltır yönetebilirsiniz.

Bu genel bakışta StorSimple Snapshot Manager tanıtır, özelliklerini açıklar ve Microsoft Azure StorSimple kendi rolünde açıklar. 

SharePoint için StorSimple cihazı, StorSimple Yöneticisi hizmetiniz, StorSimple Snapshot Manager ve StorSimple bağdaştırıcısı dahil olmak üzere tüm Microsoft Azure StorSimple sistemi genel bir bakış için bkz: [StorSimple 8000 serisi: karma bulut depolama çözümü](storsimple-overview.md). 

> [!NOTE]
> * StorSimple Snapshot Manager, Microsoft Azure StorSimple sanal (olarak da bilinen StorSimple sanal cihazlar şirket) diziler yönetmek için kullanamazsınız.
> * StorSimple Cihazınızda StorSimple güncelleştirme 2 yüklemeyi planlıyorsanız, StorSimple anlık görüntü Yöneticisi'nin en son sürümü karşıdan yükleyip emin olun **StorSimple güncelleştirme 2 yüklemeden önce**. StorSimple anlık görüntü Yöneticisi'nin en son sürümü, geriye dönük olarak uyumludur ve Microsoft Azure StorSimple serbest bırakılmış tüm sürümleri ile çalışır. StorSimple anlık görüntü Yöneticisi'nin önceki sürümü kullanıyorsanız, (, yeni sürümü yüklemeden önce önceki sürümü kaldırmanız gerekmez) güncelleştirmeniz gerekir.
> 
> 

## <a name="storsimple-snapshot-manager-purpose-and-architecture"></a>StorSimple Snapshot Manager amacı ve mimarisi
StorSimple Snapshot Manager sağlar tutarlı, oluşturmak için kullanabileceğiniz merkezi bir Yönetim Konsolu zaman içinde nokta yedek kopyalarını yerel ve bulut veri. Örneğin, konsola kullanabilirsiniz:

* Yapılandırma, yedekleme ve birimleri silin.
* Sağlamak için birim grupları yapılandırmak, yedeklenen verileri uygulamayla tutarlı olan.
* Böylece verilerin belirlenmiş bir zamanlamayla yedeklendiğinden yedekleme ilkelerini yönetin.
* Yerel oluşturun ve bulutta depolanan ve olağanüstü durum kurtarma için kullanılan anlık görüntüleri bulut.

StorSimple Snapshot Manager VSS sağlayıcısını ana bilgisayarda kayıtlı uygulamalar listesini getirir. Sonra uygulamayla tutarlı yedeklemeler oluşturmak için bir uygulama tarafından kullanılan birimleri denetler ve yapılandırmak için birim grupları önerir. StorSimple Snapshot Manager uygulamayla tutarlı yedek kopya oluşturmak için bu birim gruplarını kullanır. (Tüm ilgili dosyaları ve veritabanları eşitlenir ve belirli bir noktada uygulama zaman true durumunu temsil olduğunda uygulama tutarlılığı bulunur.) 

StorSimple Snapshot Manager yedeklemeler yalnızca değişiklikleri son yedeklemeden yakalama artımlı anlık görüntüleri biçiminde. Sonuç olarak, yedeklemeleri daha az depolama alanı gerektirir ve oluşturulabilir ve hızlı bir şekilde geri. StorSimple Snapshot Manager Windows Birim Gölge Kopyası Hizmeti (VSS) anlık görüntüleri uygulama ile tutarlı verileri yakalama emin olmak için kullanır. (Daha fazla bilgi için Windows birim gölge kopyası hizmeti bölüm ile tümleştirme gidin.) StorSimple Snapshot Manager ile yedekleme zamanlamaları oluşturabilir veya gerektiğinde hemen yedek alabilir. Veri yedekleme, StorSimple Snapshot Manager sağlar geri yüklemeniz gerekiyorsa yerel Kataloğu'ndan veya Bulut anlık görüntüleri seçin. Azure StorSimple veri kullanılabilirliği geri yükleme işlemleri sırasında gecikmeler önleyen gerektiğinde, yalnızca gerekli verileri geri yükler.)

![StorSimple Snapshot Manager mimarisi](./media/storsimple-what-is-snapshot-manager/HCS_SSM_Overview.png)

**StorSimple Snapshot Manager mimarisi** 

## <a name="support-for-multiple-volume-types"></a>Birden çok birim türleri için destek
StorSimple anlık görüntü Yöneticisi'ni yapılandırmak ve birimler aşağıdaki türlerini yedekleme için kullanabilirsiniz: 

* **Temel birimleri** – bir temel disk üzerinde tek bir bölüm temel bir birimdir. 
* **Basit birimler** – basit bir birimi tek bir dinamik diskteki disk alanını içeriyor dinamik bir birimdir. Basit bir birimi bir disk üzerinde tek bir bölge veya aynı disk üzerinde birbirine bağlanmış birden çok bölgeye oluşur. (Yalnızca dinamik disklerde basit birimler oluşturabilirsiniz.) Basit birimler hataya dayanıklı değildir.
* **Dinamik birimler** – dinamik bir birim bir dinamik disk üzerinde oluşturulan bir birimdir. Dinamik diskler, bir bilgisayarda dinamik diskler üzerinde bulunan birimlerle ilgili bilgileri izlemek için bir veritabanını kullanın. 
* **Yansıtma ile dinamik birimler** – yansıtma ile dinamik birimler, RAID 1 mimarisine yerleşiktir. RAID 1 ile özdeş veriler yansıtılmış kümesini üreten iki veya daha fazla diske yazılır. Okuma isteği, ardından istenen verileri içeren herhangi bir disk tarafından işlenebilir.
* **Küme Paylaşılan birimleri** – Küme Paylaşılan birimleri (CSV), birden çok düğüm yük devretme kümesi can aynı anda okuma veya yazma aynı diske sahip. Bir düğümden başka bir düğüme yük devretme, hızlı bir şekilde, sürücü sahipliğinin veya takma, kaldırma ve bir birim kaldırma bir değişiklik gerektirmeden ortaya çıkabilir. 

> [!IMPORTANT]
> Csv ve CSV olmayan aynı anlık görüntüdeki karışık kullanmayın. Csv ve CSV olmayan bir anlık görüntü içinde karıştırılması desteklenmiyor. 
> 
> 

StorSimple Snapshot Manager, tüm birim gruplarını geri yüklemek veya ayrı birimleri kopyalamak ve tek tek dosyaları kurtarmak için kullanabilirsiniz.

* [Birim ve birim grupları](#volumes-and-volume-groups) 
* [Yedekleme türleri ve yedekleme ilkeleri](#backup-types-and-backup-policies) 

StorSimple Snapshot Manager özellikleri ve bunların nasıl kullanılacağını hakkında daha fazla bilgi için bkz: [StorSimple Snapshot Manager kullanıcı arabirimini](storsimple-use-snapshot-manager.md).

## <a name="volumes-and-volume-groups"></a>Birim ve birim grupları
Birimler oluşturabilir ve bunları Toplu gruplar halinde yapılandırmanız ile StorSimple Snapshot Manager. 

StorSimple Snapshot Manager uygulamayla tutarlı yedek kopyaları oluşturmak için birim gruplarını kullanır. Tüm ilgili dosyaları ve veritabanları eşitlenir ve gerçek bir uygulamada belirli bir noktada zaman durumunu temsil eden olduğunda uygulama tutarlılığı bulunmaktadır. Birim grupları (olarak da bilinen olduğu *tutarlılık grupları*) tahminlerde bir yedekleme veya geri yükleme işi.

Birim grupları birim kapsayıcıları ile aynı değildir. Birim kapsayıcısı, bulut depolama hesabı ve şifreleme ve bant genişliği tüketimini gibi diğer öznitelikleri paylaşan bir veya daha fazla birimleri içerir. Tek bir birim kapsayıcısı en fazla 256 ölçülü kaynak sağlanan StorSimple birimlerini içerebilir. Birim kapsayıcıları hakkında daha fazla bilgi için Git [birim kapsayıcıları yönetmek](storsimple-manage-volume-containers.md). Birim grupları, yedekleme işlemleri kolaylaştırmak için yapılandırdığınız birimleri koleksiyonlarıdır. Farklı bir birim kapsayıcıları ait, tek bir birimde grubunda yerleştirin ve o birim grubu için bir yedekleme ilkesi oluşturmak iki birim seçerseniz, her birim uygun depolama hesabı kullanarak uygun birimi kapsayıcısında yedeklenecek.

> [!NOTE]
> Bir birim grubundaki tüm birimleri, bir tek bulut hizmeti sağlayıcısını gelmesi gerekir.
> 
> 

## <a name="integration-with-windows-volume-shadow-copy-service"></a>Windows birim gölge kopyası hizmeti ile tümleştirme
StorSimple Snapshot Manager uygulamayla tutarlı verilerini yakalamak için Windows Birim Gölge Kopyası Hizmeti (VSS) kullanır. VSS uygulama tutarlılığı artımlı anlık görüntü oluşturmaya koordine etmek için VSS uyumlu uygulamalarla iletişim kurarak kolaylaştırır. VSS anlık görüntülerinin alınma uygulamaları geçici olarak devre dışı veya deneniyor, olmasını sağlar. 

VSS StorSimple Snapshot Manager uygulaması, SQL Server ve genel NTFS birimleri ile çalışır. İşlem aşağıdaki gibidir: 

1. Genellikle veri yönetimi ve koruma çözümü (StorSimple Snapshot Manager gibi) veya bir yedekleme uygulaması olan, bir istek sahibi VSS çağırır ve hedef uygulamada yazan yazılım hakkında bilgiler toplayın kendisine sorar.
2. VSS yazıcısı bileşeni veri açıklamasını almak için iletişim kurar. Yazıcı verilerin yedeklenmesi için açıklaması döndürür. 
3. VSS uygulama yedekleme için hazırlamak için yazıcı işaret eder. Yazıcı verileri yedekleme açık hareketler tamamlayarak işlem günlüklerinin ve benzeri güncelleştirme hazırlar ve VSS'yi bildirir
4. VSS Yazıcı geçici olarak uygulamanın veri depolarına durdurmak ve gölge kopya oluşturulurken veri birime yazılır emin olmak için bildirir. Bu adım, veri tutarlılığı sağlar ve 60 saniyeden daha uzun sürer.
5. VSS gölge kopya oluşturmak için sağlayıcıyı yönlendirir. Yazılım veya donanım tabanlı, olabilir sağlayıcıları çalışmakta olan ve isteğe bağlı olarak bunları gölge kopyalarını oluşturun birimleri yönetin. Sağlayıcı, gölge kopya oluşturur ve tamamlandığında VSS bildirir.
6. VSS Yazıcı g/ç sürdürebilirsiniz uygulamayı bilgilendirecek ve g/ç gölge kopya oluşturma sırasında başarıyla duraklatıldı onaylamak için iletişim kurar. 
7. Kopyalama başarılı olduysa, VSS istek sahibine kopyanın konumu döndürür. 
8. Gölge kopya oluşturulduğu sırada veri yazıldı, yedekleme tutarsız olacaktır. VSS gölge kopya siler ve istek sahibinin bildirir. İstek sahibi otomatik olarak yedekleme işlemi yineleyin veya daha sonraki bir zamanda yeniden denemek için yöneticinize bildirin.

Aşağıdaki çizime bakın.

![VSS işlemi](./media/storsimple-what-is-snapshot-manager/HCS_SSM_VSS_process.png)

**Windows birim gölge kopyası hizmeti işlemi** 

## <a name="backup-types-and-backup-policies"></a>Yedekleme türleri ve yedekleme ilkeleri
StorSimple Snapshot Manager ile verileri yedeklemek ve yerel olarak hem de bulutta saklayın. StorSimple Snapshot Manager hemen yedekleme için kullanabileceğiniz veya yedek otomatik olarak almak için bir zamanlama oluşturmak için bir yedekleme İlkesi kullanabilirsiniz. Yedekleme ilkeleri kaç anlık görüntü tutulacağını belirtin olanak tanır. 

### <a name="backup-types"></a>Yedekleme türleri
StorSimple Snapshot Manager yedekleme aşağıdaki türleri oluşturmak için kullanabilirsiniz:

* **Yerel anlık görüntüleri** – yerel anlık görüntüler StorSimple cihazında depolanmış birim verilerini zaman içinde nokta kopyalarını değildir. Genellikle, bu yedekleme türü oluşturulabilir ve hızlı bir şekilde geri. Yerel bir yedek kopya gibi yerel bir anlık görüntü kullanabilirsiniz.
* **Bulut anlık görüntüleri** – bulut anlık görüntüleri olan bulutta depolanan birim verilerini zaman içinde nokta kopyalarını. Bir bulut anlık görüntüsü, farklı, şirket dışı depolama sisteminizde çoğaltılan bir anlık görüntü eşdeğerdir. Bulut anlık görüntüleri olağanüstü durum kurtarma senaryolarında özellikle kullanışlıdır.

### <a name="on-demand-and-scheduled-backups"></a>İsteğe bağlı ve zamanlanmış yedeklemeler
StorSimple Snapshot Manager ile hemen oluşturulacak bir kerelik bir yedekleme başlatabilir veya yinelenen yedekleme işlemleri zamanlamak için bir yedekleme İlkesi kullanabilirsiniz.

Bir yedekleme İlkesi düzenli Yedeklemeler zamanlamak için kullanabileceğiniz otomatik kurallar kümesidir. Bir yedekleme İlkesi, bir özel birim grubu anlık görüntülerini almak için parametreler ve sıklığı tanımlamanıza olanak sağlar. Başlangıç ve sona erme tarihleri, kez, sıklığı ve bekletme gereksinimleri, hem yerel belirtmek için ilkeleri kullanabilirsiniz ve bulut anlık görüntüleri. Tanımladığınız hemen sonra bir ilke uygulanır. 

StorSimple Snapshot Manager yapılandırmak veya yedekleme ilkeleri gerektiğinde yeniden yapılandırmak için kullanabilirsiniz. 

Oluşturduğunuz her bir yedekleme ilkesi için aşağıdaki bilgileri yapılandırın:

* **Ad** – seçilen yedekleme ilkesinin benzersiz adı.
* **Tür** – yedekleme İlkesi; yerel anlık görüntü veya Bulut anlık görüntüsü türü.
* **Birim grubu** – seçilen yedekleme İlkesi atanmış birim grubu.
* **Bekletme** – korumak için yedek kopyaların sayısı. Bunu işaretlerseniz **tüm** kutusunda, birim başına yedek kopyaların sayısı üst sınırına kadar tüm yedek kopyaları korunur, bu noktada ilke başarısız olacak ve bir hata iletisi oluşturur. Alternatif olarak, (1 ile 64 arasında) korumak için yedekleme sayısı belirtebilirsiniz.
* **Tarih** – yedekleme İlkesi ne zaman oluşturulduğu tarih.

Yedekleme ilkelerini yapılandırma hakkında daha fazla bilgi için Git [oluşturmak ve yedekleme ilkeleri yönetmek için StorSimple anlık görüntü Yöneticisi'ni kullanın](storsimple-snapshot-manager-manage-backup-policies.md).

### <a name="backup-job-monitoring-and-management"></a>Yedekleme işi izleme ve yönetim
StorSimple Snapshot Manager izlemek ve yaklaşan, zamanlanmış ve tamamlanan yedekleme işlerini yönetmek için kullanabilirsiniz. Ayrıca, StorSimple Snapshot Manager en fazla 64 tamamlanmış yedekleme kataloğu sağlar. Katalog bulmak ve birimleri veya tek tek dosyaları geri yüklemek için kullanabilirsiniz. 

Yedekleme işlerini izleme hakkında daha fazla bilgi için Git [görüntülemek ve yedekleme işlerini yönetmek için StorSimple anlık görüntü Yöneticisi'ni kullanın](storsimple-snapshot-manager-manage-backup-jobs.md).

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [StorSimple çözümünüzün yönetmek için StorSimple anlık görüntü Yöneticisi'ni kullanarak](storsimple-snapshot-manager-admin.md).
* Karşıdan [StorSimple Snapshot Manager](https://www.microsoft.com/download/details.aspx?id=44220).

