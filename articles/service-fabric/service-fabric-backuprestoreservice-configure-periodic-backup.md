---
title: Azure Service fabric'te düzenli yedekleme yapılandırması anlama | Microsoft Docs
description: Service Fabric'in düzenli yedekleme ve geri yükleme, uygulama verilerinin düzenli veri yedeklemeyi etkinleştirme özelliği.
services: service-fabric
documentationcenter: .net
author: hrushib
manager: chackdan
editor: hrushib
ms.assetid: FAA45B4A-0258-4CB3-A825-7E8F70F28401
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/01/2019
ms.author: hrushib
ms.openlocfilehash: b1b36ed5197aeb056c70200a49e09cc777d66d0b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66237350"
---
# <a name="understanding-periodic-backup-configuration-in-azure-service-fabric"></a>Azure Service fabric'te düzenli yedekleme yapılandırması anlama

Durum bilgisi olan Reliable services ve Reliable Actors düzenli yedekleme, aşağıdaki adımlardan oluşur:

1. **Yedekleme ilkelerini oluşturulmasını**: Bu adımda, bir veya daha fazla yedekleme ilkeleri gereksinimlerine bağlı olarak oluşturulur.

2. **Yedeklemeyi etkinleştirme olanağı**: Bu adımda oluşturulan yedekleme ilkeleri ilişkilendirmek **1. adım** gerekli varlıklara _uygulama_, _hizmet_, veya bir _bölüm_.

## <a name="create-backup-policy"></a>Yedekleme ilkesi oluşturma

Bir yedekleme İlkesi, aşağıdaki yapılandırmaları oluşur:

* **Veri kaybı geri yükleme otomatik**: Bir veri kaybı olayından bölüm deneyimleri durumunda otomatik olarak en son kullanılabilir yedek kullanarak geri yüklemeyi tetikleyecek belirtir.

* **En fazla artımlı yedeklemeler**: En fazla iki tam yedeklemeler arasında yapılacak artımlı yedeklemelerin sayısını tanımlar. Artımlı yedeklemeler en fazla boyut üst sınırı belirtin. Belirtilen sayıda artımlı yedeklemeler aşağıdaki koşullardan biri tamamlanmadan önce bir tam yedekleme alınmış olabilir

    1. Birincil haline gelmiştir olduğundan çoğaltma hiçbir zaman bir tam yedekleme duruma getirdi.

    2. Bazı günlük kayıtlarını olduğundan, son yedeklemeden kesildi.

    3. Çoğaltma MaxAccumulatedBackupLogSizeInMB sınırı geçirildi.

* **Yedekleme zamanlaması**: Düzenli yedeklemeler almak sıklığı ve zaman. Bir belirtilen zaman aralığı veya bir sabit anında yinelenen günlük / haftalık yedeklemeler zamanlayabilirsiniz.

    1. **Yedekleme zamanlaması sıklığı tabanlı**: Bu zamanlama türü sabit aralıklarla veri yedek almak için gereken ise kullanılması gerekir. İki ardışık yedeklemeler arasındaki istenen zaman aralığını ISO8601 biçimini kullanarak tanımlanır. Yedekleme zamanlaması sıklığı tabanlı dakikalık bir aralığı çözüm destekler.
        ```json
        {
            "ScheduleKind": "FrequencyBased",
            "Interval": "PT10M"
        }
        ```

    2. **Zamana bağlı bir yedekleme zamanlaması**: Bu zamanlama türü gün veya haftanın belirli zamanlarda veri yedek almak için gereken ise kullanılması gerekir. Zamanlama sıklık türü, günlük veya haftalık ya da olabilir.
        1. **_Günlük_ zamana bağlı bir yedekleme zamanlaması**: Bu zamanlama türü kullanılmalıdır gereksinim kimliği günün belirli zamanlarında verileri yedekleyin. Bu belirtmek için ayarlayın `ScheduleFrequencyType` için _günlük_; ve `RunTimes` istenen saat ISO8601 biçiminde günde bir listesi için belirtilen süre yanı sıra tarih göz ardı edilir. Örneğin, `0001-01-01T18:00:00` temsil _6: 00'dan_ her gün, tarih bölümü yok sayılıyor _0001-01-01_. Aşağıdaki örnekte tetikleyici günlük yedekleme için yapılandırmayı gösterir _9: 00'da_ ve _6: 00'dan_ günlük.

            ```json
            {
                "ScheduleKind": "TimeBased",
                "ScheduleFrequencyType": "Daily",
                "RunTimes": [
                  "0001-01-01T09:00:00Z",
                  "0001-01-01T18:00:00Z"
                ]
            }
            ```

        2. **_Haftalık_ zamana bağlı bir yedekleme zamanlaması**: Bu zamanlama türü kullanılmalıdır gereksinim kimliği günün belirli zamanlarında verileri yedekleyin. Bu belirtmek için ayarlayın `ScheduleFrequencyType` için _haftalık_; set `RunDays` yedekleme gerektiğinde tetiklenen ve ayarlamak için haftada bir gün listesi `RunTimes` istenen saat ISO8601 biçiminde günde bir listesi için belirtilen tarih saati ile birlikte yoksayılacak. Bir haftanın gün listesi ne zaman düzenli yedekleme tetikleyin. Aşağıdaki örnekte tetikleyici günlük yedekleme için yapılandırmayı gösterir _9: 00'da_ ve _6: 00'dan_ Pazartesi-Cuma sırasında.

            ```json
            {
                "ScheduleKind": "TimeBased",
                "ScheduleFrequencyType": "Weekly",
                "RunDays": [
                   "Monday",
                   "Tuesday",
                   "Wednesday",
                   "Thursday",
                   "Friday"
                ],
                "RunTimes": [
                  "0001-01-01T09:00:00Z",
                  "0001-01-01T18:00:00Z"
                ]
            }
            ```

* **Yedekleme depolama**: Yedeklemeleri yüklenecek konumu belirtir. Depolama, olması ya da Azure blob depolama veya dosya paylaşımı.
    1. **Azure blob depolama**: Azure yedeklemeleri oluşturulan depolamak için gereken olduğunda bu depolama türü seçilmelidir. Her ikisi de _tek başına_ ve _Azure tabanlı_ kümeleri, bu depolama türü kullanabilirsiniz. Bu depolama türü için açıklama bağlantı dizesini ve yedeklemeleri yüklenmek üzere gereken yere kapsayıcının adı gerektirir. Ardından belirtilen ada sahip kapsayıcı mevcut değilse, bir yedekleme karşıya yükleme sırasında oluşturulan.
        ```json
        {
            "StorageKind": "AzureBlobStore",
            "FriendlyName": "Azure_storagesample",
            "ConnectionString": "<Put your Azure blob store connection string here>",
            "ContainerName": "BackupContainer"
        }
        ```

    2. **Dosya Paylaşımı**: Bu depolama türü için seçilmelidir _tek başına_ şirket içi verileri depolamak için gereken olduğunda kümeleri yedekleme. Bu depolama türü için açıklama yedeklemeleri yüklenmek üzere gereken yere dosya paylaşım yolu gerektirir. Dosya paylaşımına erişim aşağıdaki seçeneklerden birini kullanarak yapılandırılabilir.
        1. _Tümleşik Windows kimlik doğrulaması_, Service Fabric kümesine ait olan tüm bilgisayarların dosya paylaşımına erişim burada sağlanır. Bu durumda, yapılandırmak için alanları ayarlayın _dosya paylaşımı_ tabanlı Yedekleme depolama.

            ```json
            {
                "StorageKind": "FileShare",
                "FriendlyName": "Sample_FileShare",
                "Path": "\\\\StorageServer\\BackupStore"
            }
            ```

        2. _Kullanıcı adı ve parola kullanarak koruma dosya paylaşımı_, dosya paylaşımına erişimi belirli kullanıcılara nereden sağlanır. Dosya Paylaşımı depolama belirtimi, aynı zamanda İkincil kullanıcı adı ve birincil kullanıcı adı ile kimlik doğrulaması başarısız olursa fall sonradan kimlik bilgilerini sağlamak için ikincil parola ve birincil parolayı belirtmek için yeteneği sağlar. Bu durumda, yapılandırmak için alanları ayarlayın _dosya paylaşımı_ tabanlı Yedekleme depolama.

            ```json
            {
                "StorageKind": "FileShare",
                "FriendlyName": "Sample_FileShare",
                "Path": "\\\\StorageServer\\BackupStore",
                "PrimaryUserName": "backupaccount",
                "PrimaryPassword": "<Password for backupaccount>",
                "SecondaryUserName": "backupaccount2",
                "SecondaryPassword": "<Password for backupaccount2>"
            }
            ```

> [!NOTE]
> Depolama güvenilirliği karşıladığından veya yedekleme verilerinin Güvenilirlik gereksinimleri aşıyor emin olun.
>

* **Bekletme İlkesi**: Yapılandırılmış depolama yedekleri tutma ilkesini belirtir. Yalnızca temel bekletme ilkesi desteklenir.
    1. **Temel Bekletme İlkesi**: Artık gerekli yedekleme dosyaları kaldırarak en iyi depolama kullanımı sağlamak için bu bekletme ilkesi sağlar. `RetentionDuration` Yedekleme depolama alanında saklanması gereken zaman aralığını ayarlamanıza imkan belirtilebilir. `MinimumNumberOfBackups` yedeklemeleri belirtilen sayıda bakılmadan, her zaman korunur emin olmak için belirtilebilen isteğe bağlı bir parametredir `RetentionDuration`. Yedeklemeler için korumak için yapılandırma aşağıdaki örnekte gösterilmiştir _10_ gün ve aşağıdaki Git yedeklerini izin vermiyor _20_.

        ```json
        {
            "RetentionPolicyType": "Basic",
            "RetentionDuration" : "P10D",
            "MinimumNumberOfBackups": 20
        }
        ```

## <a name="enable-periodic-backup"></a>Dönemsel yedeklemeyi etkinleştirme
Veri yedekleme gereksinimlerini karşılamak için yedekleme İlkesi tanımladıktan sonra yedekleme İlkesi uygun şekilde ya da ile ilişkilendirilmesi gereken bir _uygulama_, veya _hizmet_, veya bir _bölüm_.

### <a name="hierarchical-propagation-of-backup-policy"></a>Yedekleme ilkesinin hiyerarşik yayma
Service Fabric'te uygulama, hizmet ve bölümler arasında bir ilişki açıklandığı gibi hiyerarşik [uygulama modeli](./service-fabric-application-model.md). Yedekleme İlkesi ilişkilendirilebilir ya da bir _uygulama_, _hizmet_, veya bir _bölüm_ hiyerarşideki. Yedekleme İlkesi, bir sonraki düzeye hiyerarşik olarak yayar. Yalnızca bir yedekleme ilkesi oluşturulur ve ilişkili olduğu varsayılırsa bir _uygulama_, tümüne ait tüm durum bilgisi olan bölümleri _güvenilir durum bilgisi olan hizmetler_ ve _Reliable Actors_ , _uygulama_ yedekleme İlkesi kullanılarak yedeklenen. Veya yedekleme İlkesi ile ilişkili bir _güvenilir durum bilgisi olan hizmet_, tüm bölümleri yedekleme İlkesi kullanılarak yedeklenen.

### <a name="overriding-backup-policy"></a>Yedekleme ilkesini geçersiz kılma
Veri yedekleme özelliğiyle aynı yedekleme zamanlaması olduğu belirli hizmetler dışında uygulamanın tüm hizmetler için gerekli gereken daha yüksek sıklık zamanlamaya veya farklı bir depolama hesabı için yedekleme sürüyor veri yedekleme sahip olduğu bir senaryoda olabilir veya Dosya Paylaşımı. Bu senaryolara yönelik olarak, hizmet ve bölüm kapsamda geçersiz kılma yayılan ilke birime yedekleme geri yükleme hizmeti sağlar. Yedekleme İlkesi olduğunda, ilişkili _hizmet_ veya _bölüm_, onu yayılan yedekleme ilkesi varsa geçersiz kılar.

### <a name="example"></a>Örnek

Bu örnekte iki uygulamalarla Kurulum _MyApp_A_ ve _MyApp_B_. Uygulama _MyApp_A_ iki güvenilir durum bilgisi olan hizmeti içeren _SvcA1_ & _SvcA3_ve bir Reliable Actor hizmetinin _ActorA2_. _SvcA1_ sırasında üç bölüm içeren _ActorA2_ ve _SvcA3_ iki bölüm içerir.  Uygulama _MyApp_B_ üç güvenilir durum bilgisi olan hizmeti içeren _SvcB1_, _SvcB2_, ve _SvcB3_. _SvcB1_ ve _SvcB2_ iki bölümler her hata içeren _SvcB3_ üç bölüm içerir.

Bu uygulamaların veri yedekleme gereksinimleri gibi olduğunu varsayar

1. MyApp_A
    1. Tüm tüm bölümleri için verilerin günlük yedeğini _güvenilir durum bilgisi olan hizmetler_ ve _Reliable Actors_ uygulamaya ait. Yedekleme verilerini karşıya yükleme konumuna _BackupStore1_.

    2. Bir hizmet _SvcA3_, veri yedekleme saatte gerektirir.

    3. Bölümü cinsinden veri boyutu _SvcA1_P2_ beklenenden fazla ve yedekleme verilerini farklı depolama konumuna depolanacak _BackupStore2_.

2. MyApp_B
    1. 8: 00'da tüm bölümlerin her Pazar verilerin yedeğini oluşturma _SvcB1_ hizmeti. Yedekleme verilerini karşıya yükleme konumuna _BackupStore1_.

    2. Her gün saat 8: 00'da bölümü için verilerin yedeğini oluşturma _SvcB2_P1_. Yedekleme verilerini karşıya yükleme konumuna _BackupStore1_.

Bu veri yedekleme gereksinimlerini karşılamak için yedekleme ilkelerini BP_1 BP_5 oluşturulur ve yedekleme gibi etkindir.
1. MyApp_A
    1. Yedekleme ilkesi oluşturma _BP_1_, 24 sıklığı ayarlandığı sıklığa dayalı yedekleme zamanlaması ile saat. ve yedekleme depolama konumu kullanmak üzere yapılandırılmış depolama _BackupStore1_. Bu ilkeyi etkinleştirmek için uygulama _MyApp_A_ kullanarak [uygulama yedeklemeyi etkinleştir](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-enableapplicationbackup) API. Bu eylem, yedekleme İlkesi'ni kullanarak veri yedekleme etkinleştirir _BP_1_ tüm bölümleri için _güvenilir durum bilgisi olan hizmetler_ ve _Reliable Actors_ uygulamayaait _MyApp_A_.

    2. Yedekleme ilkesi oluşturma _BP_2_, yedekleme zamanlaması sıklığı 1 olarak ayarlandığı sıklığa dayalı ile saat. ve yedekleme depolama konumu kullanmak üzere yapılandırılmış depolama _BackupStore1_. Hizmet için bu ilkeyi etkinleştirmek _SvcA3_ kullanarak [hizmet yedeklemeyi etkinleştir](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-enableservicebackup) API. Bu eylem yayılan ilkesini geçersiz kılar _BP_1_ açıkça yedekleme ilkesi etkinleştirilir _BP_2_ hizmetinin tüm bölümler için _SvcA3_ Yedekleme kullanarak verileri yedekleme için önde gelen İlke _BP_2_ bu bölümler için.

    3. Yedekleme ilkesi oluşturma _BP_3_, 24 sıklığı ayarlandığı sıklığa dayalı yedekleme zamanlaması ile saat. ve yedekleme depolama konumu kullanmak üzere yapılandırılmış depolama _BackupStore2_. Bölüm için bu ilkeyi etkinleştirmek _SvcA1_P2_ kullanarak [bölüm yedeklemeyi etkinleştir](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-enablepartitionbackup) API. Bu eylem yayılan ilkesini geçersiz kılar _BP_1_ açıkça yedekleme ilkesi etkinleştirilir _BP_3_ bölümü için _SvcA1_P2_.

2. MyApp_B
    1. Yedekleme ilkesi oluşturma _BP_4_haftalık zamanlama sıklık türü ayarlandığı zaman tabanlı Yedekleme zamanlaması ile çalışma günleri pazar için ayarlanır ve çalışma zamanları, 8:00:00 ile ayarlanır. Yedekleme depolama konumu kullanmak üzere yapılandırılmış depolama _BackupStore1_. Hizmet için bu ilkeyi etkinleştirmek _SvcB1_ kullanarak [hizmet yedeklemeyi etkinleştir](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-enableservicebackup) API. Bu eylem, yedekleme İlkesi'ni kullanarak veri yedekleme etkinleştirir _BP_4_ hizmetinin tüm bölümler için _SvcB1_.

    2. Yedekleme ilkesi oluşturma _BP_5_, zamanlama sıklık türü olduğu zamana dayalı yedekleme zamanlaması ile günlük ayarlayın ve çalışma zamanları, 8:00:00 ile ayarlanır. Yedekleme depolama konumu kullanmak üzere yapılandırılmış depolama _BackupStore1_. Bölüm için bu ilkeyi etkinleştirmek _SvcB2_P1_ kullanarak [bölüm yedeklemeyi etkinleştir](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-enablepartitionbackup) API. Bu eylem, yedekleme İlkesi'ni kullanarak veri yedekleme etkinleştirir _BP_5_ bölümü için _SvcB2_P1_.

Aşağıdaki diyagramda açıkça etkin yedekleme ilkelerini gösterir ve yedekleme ilkelerini yayılır.

![Service Fabric uygulama hiyerarşisi][0]

## <a name="disable-backup"></a>Yedeklemeyi devre dışı bırak
Yedekleme verilerini gerek olduğunda yedekleme ilkelerini devre dışı bırakılabilir. Yedekleme İlkesi, etkin bir _uygulama_ yalnızca aynı anda devre dışı bırakılabilir _uygulama_ kullanarak [devre dışı uygulama yedekleme](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-disableapplicationbackup) API, yedekleme İlkesi bir etkin_hizmet_ aynı anda devre dışı bırakılabilir _hizmet_ kullanarak [hizmeti yedekleme devre dışı](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-disableservicebackup) API ve yedekleme İlkesi, etkin bir _bölüm_ aynı anda devre dışı bırakılabilir _bölüm_ kullanarak [bölüm Yedekleme devre dışı](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-disablepartitionbackup) API.

* Yedekleme ilkesi için devre dışı bir _uygulama_ güvenilir durum bilgisi olan hizmet veya güvenilir aktör bölümleri için yedekleme ilkesini yayılmasını sonucu olarak gerçekleştirilecek tüm düzenli veri yedeklemeleri durdurur.

* Yedekleme ilkesi için devre dışı bir _hizmet_ bölümlerinden biri için bu yedekleme İlkesi yayılmasını sonucu olarak gerçekleştirilecek tüm düzenli veri yedeklemeleri durdurur _hizmet_.

* Yedekleme ilkesi için devre dışı bir _bölüm_ tüm düzenli veri yedekleme yedekleme İlkesi bölüm en nedeniyle gerçekleşen işlemleri durdurur.

* Yedekleme için bir entity(application/service/partition) devre dışı bırakma sırasında `CleanBackup` ayarlanabilir _true_ yapılandırılmış depolama alanındaki tüm yedekleri silmek için.
    ```json
    {
        "CleanBackup": true 
    }
    ```

## <a name="suspend--resume-backup"></a>Askıya alma ve yedeklemeyi Sürdür
Belirli bir durum verilerinin düzenli yedeklemesi geçici askıya alınması talep edebilir. Bu durumda, gereksinim, bağlı olarak API, kullanılabilir yedekleme askıya bir _uygulama_, _hizmet_, veya _bölüm_. Düzenli yedekleme askıya alma, uygulamanın hiyerarşi uygulandığı noktasından alt ağacı içinde geçişlidir. 

* Ne zaman askıya alınma uygulandığında bir _uygulama_ kullanarak [uygulama yedekleme askıya](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-suspendapplicationbackup) API, ardından tüm bölümler altında bu uygulama ve Hizmetleri için veri düzenli yedekleme askıya alınır.

* Ne zaman askıya alınma uygulanır bir _hizmet_ kullanarak [yedekleme hizmeti askıya alma](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-suspendservicebackup) API, ardından tüm bölümler altında bu hizmet için veri düzenli yedekleme askıya alınır.

* Ne zaman askıya alınma uygulanan en bir _bölüm_ kullanarak [bölüm Yedekleme askıya](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-suspendpartitionbackup) API, ardından bölümler altında bu hizmet askıya alır veri düzenli yedeklemesi için askıya alınır.

Askıya alma gereksinimini bittikten sonra düzenli yedekleme ilgili sürdürme yedekleme API'sini kullanarak döndürülebilir. Düzenli yedekleme gerekir sürdürüldü aynı anda _uygulama_, _hizmet_, veya _bölüm_ burada bu askıya alındı.

* Askıya alma, uygulandıysa bir _uygulama_, kullanarak sürdürülmesini sonra [uygulama yedeklemeyi Sürdür](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-resumeapplicationbackup) API. 

* Askıya alma, uygulandıysa bir _hizmet_, kullanarak sürdürülmesini sonra [hizmet yedeklemeyi Sürdür](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-resumeservicebackup) API.

* Askıya alma, uygulandıysa bir _bölüm_, kullanarak sürdürülmesini sonra [bölüm yedeklemeyi Sürdür](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-resumepartitionbackup) API.

### <a name="difference-between-suspend-and-disable-backups"></a>Askıya alma ve devre dışı yedeklemeler arasındaki fark
Devre dışı yedekleme, yedekleme artık belirli bir uygulama, hizmet veya bölüm için gerekli olduğunda kullanılmalıdır. Bir true olması da var olan tüm yedeklemeler de silinir anlamına gelir devre dışı yedekleme isteği temiz yedeklemeleri parametresi ile birlikte çağırabilirsiniz. Ancak, askıya alma burada bir istediği yedeklemeleri, yerel disk tam olduğunda gibi geçici olarak devre dışı bırakmayı veya yedekleme karşıya yükleme vb. bilinen ağ sorunu nedeniyle başarısız senaryolarda kullanılır. 

Yalnızca bir düzeyinde devre dışı bırakma çağrılabilir olsa önceden yedekleme için açıkça askıya alma, şu anda yedekleme için ya da doğrudan etkinleştirilen herhangi bir düzeyde veya devralma yoluyla ancak uygulanabilir etkin / hiyerarşisi. Örneğin, bir uygulama düzeyinde yedekleme etkinleştirilirse, bir devre dışı bırakma çalıştırabilirsiniz ancak yalnızca uygulama düzeyinde askıya uygulama, hizmet veya uygulama altında bölüm çağrılabilir. 

## <a name="auto-restore-on-data-loss"></a>Veri kaybı otomatik geri yükleme
Hizmet bölüm beklenmeyen hatalar nedeniyle veri kaybedebilirsiniz. Örneğin, bir bölüm (birincil çoğaltma dahil) için iki tanesi üç çoğaltmalar için disk bozuk silinebilen veya.

Service Fabric bölümü veri kaybına olduğunu algıladığında, bu çağırır `OnDataLossAsync` arabirim yöntemi bölüme ve veri kaybı dışında olması gereken eylemi gerçekleştirmek için bölüm bekliyor. Bölüm en etkili bir yedekleme ilkesi varsa, bu durumda `AutoRestoreOnDataLoss` bayrağı ayarlanmış `true` ardından geri yüklemeyi otomatik olarak bu bölüm için en son kullanılabilir yedek kullanarak tetiklenen.

## <a name="get-backup-configuration"></a>Yedekleme yapılandırmasını alma
Ayrı API yedekleme yapılandırması bilgileri almak kullanılabilir yapılan bir _uygulama_, _hizmet_, ve _bölüm_ kapsam. [Uygulama yedekleme yapılandırma bilgilerini alma](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getapplicationbackupconfigurationinfo), [hizmeti yedekleme yapılandırma bilgi al](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getservicebackupconfigurationinfo), ve [bölüm Yedekleme yapılandırma bilgi al](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackupconfigurationinfo) API'leri sırasıyla şunlardır. Esas olarak, bu API'ler, geçerli yedekleme İlkesi, yedekleme ilkesi uygulanan ve yedekleme askıya alma ayrıntıları olduğu kapsamı döndürür. Bu API'leri döndürülen sonuçları hakkında kısa bir açıklaması verilmiştir.

- Uygulama yedekleme yapılandırması bilgileri: uygulama ve Hizmetleri ve bölümleri uygulamaya ait tüm düzenlenmiştir ilkeleri uygulanan yedekleme ilkesini ayrıntılarını sağlar. Uygulama askıya alma bilgilerini de içerir ve Hizmetleri ve bölümler.

- Hizmeti yedekleme yapılandırması bilgileri: etkili bir yedekleme İlkesi hizmetine ve kapsamda ayrıntılarını sağlar, bu ilkenin uygulandığı ve bölümleri düzenlenmiştir tüm ilkeleri. Ayrıca, hizmet ve bölümleri için askıya alma bilgilerini içerir.

- Yedekleme yapılandırması bilgileri bölümü: bölüm ve bu ilkenin uygulandığı kapsam geçerli yedekleme ilkesinin ayrıntılarını sağlar. Ayrıca, bölümler için askıya alma bilgileri içerir.

## <a name="list-available-backups"></a>Kullanılabilir yedekleri Listele

Kullanılabilir yedekler, yedekleme listesi API'sini alma kullanarak listelenebilir. API çağrısının sonucu, geçerli yedekleme ilkesinde yapılandırılan yedekleme depolama, kullanılabilir tüm yedeklemeler ilgili yedekleme bilgilerini öğeleri içerir. Farklı çeşitlerini bu API, bir uygulama, hizmet veya bölüm ait listesi kullanılabilir yedekler için sağlanır. Bu API'ler için destek alma _son_ kullanılabilir yedek yürürlükteki tüm bölümlerin veya yedeklerini filtreleme temel alarak _başlangıç tarihi_ ve _bitiş tarihi_.

Bu API'ler ayrıca sonuçlarını sayfalandırma destekler, _maxresults bağımsız değişkenini_ parametresi, sıfır olmayan pozitif tamsayı olarak ayarlanırsa sonra API maksimum döndürür _maxresults bağımsız değişkenini_ yedekleme bilgisi öğeleri. Var olması durumunda, daha fazla yedekleme bilgilerini öğe daha kullanılabilir _maxresults bağımsız değişkenini_ değeri bir devamlılık belirteci verilir. Geçerli devamlılık belirteci parametresi sonraki almak için kullanılan sonuçlarını ayarlayın. Geçerli devamlılık belirteci değeri sonraki API çağrısına geçirilen zaman API sonraki sonuç kümesini döndürür. Tüm kullanılabilir sonuçları döndürdüğünde devamlılık belirteci yanıtına dahil edilir.

Desteklenen türevleri hakkında kısa bilgiler verilmiştir.

- [Uygulama yedekleme listesini alma](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getapplicationbackuplist): Belirli bir Service Fabric uygulaması ait her bölüm için mevcut yedekleme listesini döndürür.

- [Hizmet yedekleme listesini alma](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getservicebackuplist): Service Fabric hizmet ait her bölüm için mevcut yedekleme listesini döndürür.
 
- [Bölüm yedekleme listesini alma](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackuplist): Yedekleme için belirtilen bölüm kullanılabilir listesini döndürür.

## <a name="next-steps"></a>Sonraki adımlar
- [Yedekleme geri yükleme REST API Başvurusu](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)

[0]: ./media/service-fabric-backuprestoreservice/BackupPolicyAssociationExample.png
