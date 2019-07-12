---
title: Azure kurtarma Hizmetleri kasalarını ve sunucularını yönetme
description: İşler ve Azure kurtarma Hizmetleri kasasında uyarılar yönetin.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/08/2019
ms.author: raynew
ms.openlocfilehash: b447290a6910d144703bb796290908d0fc21b924
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705129"
---
# <a name="monitor-and-manage-recovery-services-vaults"></a>Kurtarma Hizmetleri kasalarını izleme ve yönetme

Bu makalede, Kurtarma Hizmetleri kasası kullanmayı açıklar **genel bakış** bir pano, Kurtarma Hizmetleri kasalarını izleme ve yönetme. Listeden bir kurtarma Hizmetleri kasası açtığınızda **genel bakış** seçilen kasa panosu açılır. Pano kasası ile ilgili çeşitli ayrıntılar hakkında bilgi sağlar. Vardır *kutucukları* gösteren: durumu kritik ve uyarı bildirimleri, devam eden ve başarısız yedekleme işleri ve yerel olarak yedekli depolama (LRS) ve kullanılan coğrafi olarak yedekli depolama (GRS) miktarı. Kasa için Azure Vm'lerini yedekleme yapıyorsanız [ **yedekleme ön denetim durumu** kutucuk görüntüler kritik veya uyarı öğeler](https://azure.microsoft.com/blog/azure-vm-backup-pre-checks/). Aşağıdaki görüntü **genel bakış** için Pano **Contoso-vault**. **Yedekleme öğeleri** kasaya kayıtlı dokuz öğeleri kutucuk var. gösterir.

![Kurtarma Hizmetleri kasa Panosu](./media/backup-azure-manage-windows-server/rs-vault-blade.png)

Bu makale için önkoşullardır: bir Azure aboneliğine ve bir kurtarma Hizmetleri kasası, orada olduğu kasa için yapılandırılmış en az bir yedekleme öğesi.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]


## <a name="open-a-recovery-services-vault"></a>Bir kurtarma Hizmetleri kasasını açın

Uyarıları yönetme veya kurtarma Hizmetleri kasası ile ilgili yönetim verileri görüntülemek için kasa açın.

1. Oturum [Azure portalında](https://portal.azure.com/) Azure aboneliğinizi kullanarak.

2. Portalında **tüm hizmetleri**.

   ![1\. kurtarma Hizmetleri kasaları adım listesini açma](./media/backup-azure-manage-windows-server/open-rs-vault-list.png)

3. İçinde **tüm hizmetleri** iletişim kutusuna **kurtarma Hizmetleri**. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Zaman **kurtarma Hizmetleri kasaları** seçeneği göründüğünde, aboneliğinizde kurtarma Hizmetleri kasalarının listesini açmak için tıklayın.

    ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-azure-manage-windows-server/list-of-rs-vaults.png) <br/>

4. Kasalarının listesinden bir kasa açmak için tıklayın, **genel bakış** Pano.

    ![Kurtarma Hizmetleri kasa Panosu](./media/backup-azure-manage-windows-server/rs-vault-blade.png) <br/>

    Genel Bakış Panosu, uyarılar ve yedekleme işi veri sağlamak için kutucuklar kullanır.

## <a name="monitor-backup-jobs-and-alerts"></a>Yedekleme işleri izleme ve uyarılar

Kurtarma Hizmetleri kasası **genel bakış** Pano kutucukları için izleme ve kullanım bilgileri sağlar. Kutucuklar izleme bölümünde görünen kritik ve uyarı bildirimleri ve sürmekte olan ve başarısız işler. Belirli bir uyarıyı veya işi, iş veya uyarı için filtrelenmiş yedekleme uyarıları veya işleri yedekleme menüsünü açmak için tıklayın.

![Yedekleme Pano görevleri](./media/backup-azure-manage-windows-server/monitor-dashboard-tiles-warning.png)

İzleme bölümü sonuçlarını gösteren önceden tanımlanmış **yedekleme uyarıları** ve **yedekleme işleri** sorgular. İzleme kutucuklar hakkında güncel bilgiler sağlar:

* Kritik ve uyarı uyarılar için yedekleme işlerinde (son 24 saat)
* Azure Vm'leri - ön denetim durumu ön denetim durumu hakkında tam bilgi için bkz. [yedekleme öncesi denetim durumu yedekleme blogunda](https://azure.microsoft.com/blog/azure-vm-backup-pre-checks/).
* Yedekleme işleri ilerleme ve (son 24 saat içindeki) başarısız olan işler.

Kullanım kutucuklar sağlar:

* Kasa için yapılandırılan yedekleme öğelerin sayısı.
* Kasa tarafından kullanılan (LRS ve GRS göre ayrılmış olarak) Azure depolama.

Kutucuklar (dışında yedekleme depolama alanı), ilişkili menüsünü açmak için tıklayın. Yukarıdaki görüntüde, yedekleme uyarıları kutucuğuna üç kritik uyarıları gösterir. Kritik uyarılar satır yedekleme uyarıları kutucuğuna tıklayarak, kritik uyarılar için filtrelenmiş yedekleme uyarıları açılır.

![Kritik uyarılar için filtrelenmiş yedekleme uyarıları menüsü](./media/backup-azure-manage-windows-server/critical-backup-alerts.png)

Yukarıdaki görüntüde yedekleme uyarıları menüsünde göre filtrelenir: Durumu etkin olduğu, kritik öneme sahip olduğundan ve zaman önceki 24 saattir.

## <a name="manage-backup-alerts"></a>Yedekleme Uyarıları yönetme

Kurtarma Hizmetleri kasası menüsünde yedekleme uyarılar menüsüne erişmek için tıklayın **yedekleme uyarıları**.

![Yedekleme uyarıları](./media/backup-azure-manage-windows-server/backup-alerts-menu.png)

Yedekleme uyarıları rapor kasa uyarılarını listeler.

![Yedekleme uyarıları](./media/backup-azure-manage-windows-server/backup-alerts.png)

### <a name="alerts"></a>Uyarılar

Yedekleme uyarıları listesi, filtrelenmiş uyarılar için seçilen bilgileri görüntüler. Yedekleme uyarıları menüde kritik veya uyarı uyarıları filtreleyebilirsiniz.

| Uyarı düzeyi | Uyarılar üreten olaylar |
| ----------- | ----------- |
| Kritik | Kritik aldığınız uyarıları: Yedekleme işleri başarısız, Kurtarma işleri başarısız olur, ve ne zaman bir sunucu üzerinde korumayı durdurun, ancak verileri korur.|
| Uyarı | Uyarı aldığınız uyarıları: Yedekleme işleri uyarılarla, örneğin, 100'den daha az dosya bozulması sorunları nedeniyle veya daha büyük olduğunda 1.000.000 dosyalar başarıyla yedeklendi daha yedeklenmez Tamamla). |
| Bilgilendirici | şu anda hiçbir bilgilendirici uyarılar kullanımda olması. |

### <a name="viewing-alert-details"></a>Uyarı ayrıntılarını görüntüleme

Yedekleme uyarılar raporu, her uyarı hakkında sekiz ayrıntıları izler. Kullanım **Sütunları Seç** rapor ayrıntıları düzenlemek için düğme.

![Yedekleme uyarıları](./media/backup-azure-manage-windows-server/backup-alerts.png)

Varsayılan olarak, tüm ayrıntıları dışında **son oluşum zamanı**, raporda görünür.

* Uyarı
* Yedekleme öğesi
* Korumalı sunucu
* Severity
* Duration
* Oluşturma zamanı
* Durum
* Son oluşum zamanı

### <a name="change-the-details-in-alerts-report"></a>Uyarılar raporu ayrıntıları değiştirme

1. Rapor bilgilerini değiştirmek için **yedekleme uyarıları** menüsünde tıklatın **Sütunları Seç**.

   ![Yedekleme uyarıları](./media/backup-azure-manage-windows-server/alerts-menu-choose-columns.png)

   **Sütunları Seç** menüsü açılır.

2. İçinde **Sütunları Seç** menüsünde, raporda görünmesini istediğiniz ayrıntıları seçin.

    ![Sütunları menüsünü seçin](./media/backup-azure-manage-windows-server/choose-columns-menu.png)

3. Tıklayın **Bitti** yaptığınız değişiklikleri kaydedin ve Seç sütun menüyü kapatın.

   Değişiklik, ancak değişiklikleri saklamak istemiyorsanız, tıklayın **sıfırlama** son seçilen döndürmek için yapılandırma kaydedildi.

### <a name="change-the-filter-in-alerts-report"></a>Uyarılar raporu filtrede değiştirme

Kullanım **filtre** durumu, başlangıç ve bitiş zamanı uyarılar önem derecesini değiştirmek için menü.

> [!NOTE]
> Yedekleme uyarıları düzenleme filtre kritik veya uyarı uyarılar genel bakış Panosu'nu kasadaki değişikliğe neden olmaz.
>  

1. Yedekleme uyarıları menüsünde yedekleme uyarıları filtre değiştirmek için tıklayın **filtre**.

   ![Filtre menüsü seçin](./media/backup-azure-manage-windows-server/alerts-menu-choose-filter.png)

   Filtre menüsü görüntülenir.

   ![Filtre menüsü seçin](./media/backup-azure-manage-windows-server/filter-alert-menu.png)

2. Önem derecesi, durum, başlangıç saati veya bitiş zamanı, düzenleyebilir ve **Bitti** yaptığınız değişiklikleri kaydedin.

## <a name="configuring-notifications-for-alerts"></a>Uyarılar için bildirimleri yapılandırma

Bir uyarı veya Kritik Uyarı oluştuğunda e-postalar oluşturmak için bildirimleri yapılandırın. Her bir saat, e-posta uyarıları gönderebilir veya belirli bir uyarı oluştuğunda.

   ![Uyarıları Filtrele](./media/backup-azure-manage-windows-server/configure-notification.png)

Varsayılan olarak, e-posta bildirimleri olan **üzerinde**. Tıklayın **kapalı** e-posta bildirimleri durdurmak için.

Üzerinde **bildirim** denetim öğesini **başına uyarı** , gruplandırma istemiyorsanız veya uyarılar oluşturabilir birçok öğe yok. Her uyarı sonuçları bir bildirim (varsayılan ayar) ve çözüm e-posta hemen gönderilir.

Seçerseniz **saatlik Özet**, son bir saat içinde oluşturulan çözümlenmemiş uyarılar açıklayan alıcılara bir e-posta gönderilir. Saatin sonunda bir çözüm e-posta gönderilir.

E-posta oluşturmak için kullanılan uyarı önem derecesi (kritik veya uyarı) seçin. Şu anda hiçbir bilgi uyarı yok.

## <a name="manage-backup-items"></a>Yedekleme öğeleri yönetme

Kurtarma Hizmetleri kasası, yedekleme verilerini türlerde tutar. [Daha fazla bilgi edinin](backup-overview.md#what-can-i-back-up) hakkında ne yedekleyebilirsiniz. Çeşitli sunucular, bilgisayarlar, veritabanları ve iş yüklerini yönetmek için tıklayın **yedekleme öğeleri** kasa içeriğini görüntülemek için kutucuk.

![Yedekleme öğeleri kutucuğu](./media/backup-azure-manage-windows-server/backup-items.png)

Yedekleme öğeleri, yedekleme yönetim türüne göre düzenlenmiş listesi açılır.

![Yedekleme öğeleri listesi](./media/backup-azure-manage-windows-server/list-backup-items.png)

Belirli türde bir korumalı örnek keşfetmek için Yedekleme Yönetimi Türü sütununda öğesini tıklatın. Örneğin, yukarıdaki resimde, bu kasada korunan iki Azure sanal makineler vardır. Tıklayarak **Azure sanal makine**, bu kasada korunan sanal makinelerin listesi açılır.

![Yedekleme türü listesi](./media/backup-azure-manage-windows-server/list-of-protected-virtual-machines.png)

Sanal makinelerin listesini yararlı verileri içeren: ilişkili kaynak grubunu önceki [yedekleme ön denetim](https://azure.microsoft.com/blog/azure-vm-backup-pre-checks/), son yedekleme durumu ve en son geri yükleme noktası tarihi. Üç nokta simgesine son sütunda ortak görevleri tetiklemek için menüsü açılır. Sağlanan sütunlar, faydalı verileri, her bir yedekleme türü için farklıdır.

![Yedekleme türü listesi](./media/backup-azure-manage-windows-server/ellipsis-menu.png)

## <a name="manage-backup-jobs"></a>Yedekleme işlerini yönetme

**Yedekleme işleri** kutucuğu kasa panosunda, son 24 saat içindeki devam eden veya başarısız olan iş sayısını gösterir. Kutucuk, yedekleme işleri menüsüne bir bakışta sağlar.

![Yedekleme öğeleri ayarları](./media/backup-azure-manage-windows-server/backup-jobs-tile.png)

İşleri hakkında ek ayrıntıları görmek için tıklayın **sürüyor** veya **başarısız** bu durum için filtre yedekleme işleri menüsünü açmak için.

### <a name="backup-jobs-menu"></a>Yedekleme işleri menüsü

**Yedekleme işleri** menü öğesi türü, işlemi, durumu, başlangıç saati ve süresi hakkında bilgileri görüntüler.  

Kasanın ana menüde yedekleme işleri menüsünden açmak için **yedekleme işleri**.

![Yedekleme öğeleri ayarları](./media/backup-azure-manage-windows-server/backup-jobs-menu-item.png)

Yedekleme işlerinin listesi açılır.

![Yedekleme öğeleri ayarları](./media/backup-azure-manage-windows-server/backup-jobs-list.png)

Yedekleme işleri menü son 24 saat için tüm işlemler, tüm yedekleme türlerinin durumunu gösterir. Kullanım **filtre** filtreleri değiştirmek için. Filtreler, aşağıdaki bölümlerde açıklanmıştır.

Filtreleri değiştirmek için:

1. Yedekleme işleri menüsünden kasaya tıklayın **filtre**.

   ![Yedekleme öğeleri ayarları](./media/backup-azure-manage-windows-server/vault-backup-job-menu-filter.png)

    Filtre menüsü açılır.

   ![Yedekleme öğeleri ayarları](./media/backup-azure-manage-windows-server/filter-menu-backup-jobs.png)

2. Filtre ayarları seçin ve tıklayın **Bitti**. Yeni ayarlara göre filtrelenmiş bir listesini yeniler.

#### <a name="item-type"></a>Öğe türü

Korumalı örnek yedekleme Yönetim türü öğesi türüdür. Dört tür vardır. Aşağıdaki listeye bakın. Tüm öğe türleri ya da bir öğe türü görüntüleyebilirsiniz. İki veya üç öğe türleri seçemezsiniz. Kullanılabilir öğe türleri şunlardır:

* Tüm öğe türleri
* Azure sanal makinesi
* Dosyalar ve klasörler
* Azure Storage
* Azure iş yükü

#### <a name="operation"></a>Çalışma

Tek bir işlem veya tüm işlemleri görüntüleyebilirsiniz. İki veya üç işlem seçemezsiniz. Kullanılabilir işlemler şunlardır:

* Tüm İşlemler
* Kaydolma
* Yedeklemeyi yapılandırma
* Backup
* Geri Yükleme
* Yedeklemeyi devre dışı bırak
* Yedekleme verilerini silme

#### <a name="status"></a>Durum

Tüm durum ya da görüntüleyebilirsiniz. İki veya üç durumları seçemezsiniz. Kullanılabilir durum şunlardır:

* Tüm durum
* Tamamlandı
* Sürüyor
* Başarısız
* İptal edildi
* Uyarılarla tamamlandı

#### <a name="start-time"></a>Başlangıç saati

Gün ve sorgu başladığı zaman. 24 saatlik dönemde varsayılandır.

#### <a name="end-time"></a>Bitiş zamanı

Gün ve saati sona erdiğinde sorgu.

### <a name="export-jobs"></a>Dışarı aktarma işleri

Kullanım **dışarı aktarma işleri** tüm işleri menü bilgilerini içeren bir elektronik tablo oluşturun. Elektronik tablo, tüm işlerin bir özetini içeren bir sayfa ve her bir sayfaya her iş için vardır.

İş bilgileri bir elektronik tabloya dışarı aktarmak için tıklayın **dışarı aktarma işleri**. Tarih ve kasa adını kullanarak bir elektronik tablo hizmeti oluşturur, ancak adını değiştirebilirsiniz.

## <a name="monitor-backup-usage"></a>Yedekleme kullanımı izleme

Azure'da kullanılan depolama alanına ve yedekleme depolama alanı kutucuk Panoda gösterir. Depolama alanı kullanımı için sağlanır:

* Kasayla ilişkili bulut LRS depolama kullanımı
* Kasayla ilişkili bulut GRS depolama kullanımı


## <a name="troubleshooting-monitoring-issues"></a>İzleme ile ilgili sorunları giderme

**Sorun:** İşleri ve/veya Azure Backup Aracısı'ndan uyarılar portalda görünmez.

**Sorun giderme adımları:** İşlem ```OBRecoveryServicesManagementAgent```, iş ve uyarı verileri Azure yedekleme hizmetine gönderir. Bu işlem bazen kalmış olur veya kapatma.

1. İşlemi çalışmadığından doğrulamak için açık **Görev Yöneticisi'ni**ve ```OBRecoveryServicesManagementAgent``` çalışıyor.

2. İşlem çalışmıyorsa açın **Denetim Masası**ve hizmetler listesine göz atın. Başlatın veya yeniden **Microsoft Azure kurtarma Hizmetleri yönetim Aracısı**.

    Daha fazla bilgi için günlüklere göz atın:<br/>
   `<AzureBackup_agent_install_folder>\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider*` Örneğin:<br/>
   `C:\Program Files\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider0.errlog`

## <a name="next-steps"></a>Sonraki adımlar
* [Windows Server veya Windows İstemcisi Azure'dan geri yükleme](backup-azure-restore-windows-server.md)
* Azure Backup hakkında daha fazla bilgi için bkz: [Azure Yedekleme'ye Genel Bakış](backup-introduction-to-azure-backup.md)
* Ziyaret [Azure yedekleme Forumu](https://go.microsoft.com/fwlink/p/?LinkId=290933)
