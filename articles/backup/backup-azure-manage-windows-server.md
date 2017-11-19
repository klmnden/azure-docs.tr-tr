---
title: "Azure kurtarma Hizmetleri kasaları ve sunucuları yönetmek | Microsoft Docs"
description: "Azure kurtarma Hizmetleri kasaları ve sunucuları yönetmek nasıl öğrenmek için bu öğreticiyi kullanın."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: tysonn
ms.assetid: 4eea984b-7ed6-4600-ac60-99d2e9cb6d8a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: markgal
ms.openlocfilehash: 58080d0e045f1825e89287fc421b7e84db36331e
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
---
# <a name="monitor-and-manage-azure-recovery-services-vaults-and-servers-for-windows-machines"></a>Windows makineleri için Azure kurtarma hizmetleri kasaları ile sunucularını izleme ve yönetme

Bu makale Azure portalı ve Microsoft Azure Yedekleme aracısı kullanılabilir yedek izleme ve yönetim görevlerini genel bir bakış içerir. Bu makalede, zaten bir Azure aboneliğiniz varsa ve en az bir kurtarma Hizmetleri kasası oluşturdunuz varsayılır.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]


## <a name="open-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası açın

Kurtarma Hizmetleri kasa Panosu ayrıntıları veya bir kurtarma Hizmetleri kasası özniteliklerini gösterir.

1. Oturum [Azure Portal](https://portal.azure.com/) Azure aboneliğinizi kullanarak.
2. Hub menüsünde **daha Hizmetleri**.

    ![Kurtarma Hizmetleri kasaları adım 1'in açık listesi](./media/backup-azure-manage-windows-server/open-rs-vault-list.png) <br/>

3. Kurtarma Hizmetleri kasası açmak istiyor. İletişim kutusunda yazmaya başlayın **kurtarma Hizmetleri**. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Tıklatın **kurtarma Hizmetleri kasaları** aboneliğinizde kurtarma Hizmetleri kasalarının listesini görüntülemek için.

    ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-azure-manage-windows-server/browse-to-rs-vaults-2.png) <br/>

    Kurtarma Hizmetleri kasalarının listesi açılır.

    ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-azure-manage-windows-server/list-of-rs-vaults.png) <br/>

4. Kasa listesinden açmak istediğiniz kurtarma Hizmetleri kasasının adını seçin. Kurtarma Hizmetleri kasası Pano menüsü açılır.

    ![Kurtarma Hizmetleri kasa Panosu](./media/backup-azure-manage-windows-server/rs-vault-blade.png) <br/>

    Kurtarma Hizmetleri kasası açtığınız, izleme veya yönetim görevlerinden herhangi birini deneyin.

## <a name="monitor-backup-jobs-and-alerts"></a>Yedekleme işleri izleme ve uyarılar

İşlerini ve Uyarıları gördüğünüz kurtarma Hizmetleri kasası panodan izleyin:

* Yedekleme uyarıları ayrıntıları
* Dosyaları ve klasörleri yanı sıra, bulutta korunan Azure sanal makineler
* Azure'da tüketilen toplam depolama alanı
* Yedekleme işi durumu

![Yedekleme Pano görevleri](./media/backup-azure-manage-windows-server/dashboard-tiles.png)

Bu kutucukların her bilgileri'ni tıklatarak ilgili görevleri yöneteceğiniz ilişkili menüsü açılır.

Pano üstten:

* Ayarları - kullanılabilir yedekleme görevleri erişim sağlar.
* Yedekleme - yeni dosyalar ve klasörler (veya Azure VM'ler) kurtarma Hizmetleri kasasına Yedekleme yardımcı olur.
* Bir kurtarma Hizmetleri kasası artık kullanılıyor silerseniz, depolama alanı boşaltmak için - silin. Tüm korumalı sunucuları kasadan silindikten sonra Sil yalnızca etkinleştirilir.

![Yedekleme Pano görevleri](./media/backup-azure-manage-windows-server/dashboard-tasks.png)

## <a name="alerts-for-backups-using-azure-backup-agent"></a>Azure Yedekleme aracısı kullanarak yedeklemeler için uyarılar:
| Uyarı düzeyi | Gönderilen uyarıları |
| --- | --- |
| Kritik |Yedekleme hatası, Kurtarma hatası |
| Uyarı |Yedekleme (< 100 dosyaları bozulması sorunları nedeniyle yedeklenmez ve > 1.000.000 dosyalar başarıyla yedeklendi olduğunda) uyarılarla tamamlandı |
| Bilgilendirme |None |

## <a name="manage-backup-alerts"></a>Yedekleme Uyarıları yönetme
Tıklatın **yedekleme uyarıları** açmak için kutucuğa **yedekleme uyarıları** menü uyarıları ve yönetin.

![Yedekleme uyarıları](./media/backup-azure-manage-windows-server/manage-backup-alerts.png)

Yedekleme uyarıları kutucuğu sayısını gösterir:

* Son 24 saat içinde çözümlenmemiş kritik uyarılar
* Son 24 saat içinde çözümlenmemiş uyarı bildirimleri

Görmek için bağlantıya tıklayın **yedekleme uyarıları** menüsüyle filtrelenmiş görünüm bu uyarıların (kritik veya uyarı).

Yedekleme uyarıları menüsünden:

* Uyarılarınızı ile dahil etmek için uygun bilgileri seçin.

    ![Sütunları seçin](./media/backup-azure-manage-windows-server/choose-alerts-colunms.png)
* Önem derecesi durumu için uyarıları filtrelemek ve başlangıç/bitiş saatleri.

    ![Filtre uyarıları](./media/backup-azure-manage-windows-server/filter-alerts.png)
* Önem derecesi, sıklığı ve alıcıların bildirimlerini yapılandırma gibi uyarıları Aç veya kapat.

    ![Filtre uyarıları](./media/backup-azure-manage-windows-server/configure-notifications.png)

Varsa **başına uyarı** olarak seçilen **bildirim** sıklığı, hiçbir gruplandırma veya e-postaları düşüş ortaya çıkar. Bir bildirim (varsayılan ayar) her uyarı sonuçları ve çözümleme e-posta hemen gönderilir.

Varsa **saatlik Özet** olarak seçilen **bildirim** sıklığı, bir e-posta çözümlenmemiş uyarıları son saat içinde oluşturuldu açıklayan kullanıcıya gönderilir. Saat sonunda çözümleme e-posta gönderilir.

Uyarılar için aşağıdaki önem düzeylerini gönderilebilir:

* Kritik
* Uyarı
* Bilgi

Uyarı ile devre dışı bırak **devre dışı bırak** iş ayrıntıları menü düğmesi. ' I tıklattığınızda devre dışı bırak, çözümleme notları sağlayabilir.

Uyarı ile bir parçası olarak görüntülenmesini istediğiniz sütunları seçin **sütunları seçin** düğmesi.

> [!NOTE]
> Gelen **ayarları** menüsünde seçerek yedekleme Uyarıları yönetme **izleme ve Raporlar > Uyarı ve olayları > Yedekleme uyarıları** ve ardından **filtre** veya  **Bildirimleri Yapılandırma**.
>
>

## <a name="manage-backup-items"></a>Yedekleme öğeleri yönetme
Şirket içi yedeklemeler yönetme Yönetim Portalı'nda kullanıma sunulmuştur. Pano, yedekleme bölümünde **yedekleme öğeleri** döşeme kasaya korumalı yedekleme öğeleri sayısını gösterir.

Tıklatın **dosya klasörleri** yedekleme öğeleri döşeme.

![Yedekleme öğeleri döşeme](./media/backup-azure-manage-windows-server/backup-items-tile.png)

Yedekleme öğeleri menü öğesi listelenen her belirli yedekleme gördüğünüz dosya-klasör olarak ayarlanmış filtre ile açılır.

![Yedekleme öğeleri](./media/backup-azure-manage-windows-server/backup-item-list.png)

Listeden belirli bir yedekleme öğesi seçeneğini belirlerseniz bu öğenin temel ayrıntılarını görebilirsiniz.

> [!NOTE]
> Gelen **ayarları** menüsünde seçerek dosyaları ve klasörleri yönetme **korumalı öğeler > yedekleme öğeleri** seçilerek **dosya klasörleri** açılan menüden.
>
>

![Yedekleme öğeleri ayarları](./media/backup-azure-manage-windows-server/backup-files-and-folders.png)

## <a name="manage-backup-jobs"></a>Yedekleme işlerini yönetme
(Şirket içi sunucunun Azure'a yedekleme olduğunda) hem de şirket içi için yedekleme işleri ve Azure yedeklemeleri panosunda görünür.

Pano yedekleme bölümünde yedekleme işi kutucuğunu işlerinin sayısını gösterir:

* Sürüyor
* Son 24 saat içinde başarısız oldu.

Yedekleme işlerini yönetmek için tıklatın **yedekleme işleri** döşeme, hangi yedekleme işleri menüsü açılır.

![Yedekleme öğeleri ayarları](./media/backup-azure-manage-windows-server/backup-jobs.png)

İle yedekleme işlerini menüde kullanılabilir bilgileri değiştirmek **sütunları seçin** sayfanın üstündeki düğmesi.

Kullanım **filtre** düğmesi dosya ve klasörleri ve Azure sanal makine yedeklemesi arasında seçin.

Yedekleme dosyaları ve klasörleri görmüyorsanız, tıklatın **filtre** düğmesini seçin ve sayfanın üstündeki **dosya ve klasörleri** öğesi türü menüsünde.

> [!NOTE]
> Gelen **ayarları** menüsünde seçerek yedekleme işlerini yönetme **izleme ve raporlama > işleri > yedekleme işleri** seçilerek **dosya klasörleri** aşağı açılan menüsü.
>
>

## <a name="monitor-backup-usage"></a>Yedekleme kullanımını izleme
Pano yedekleme bölümünde yedekleme kullanımı kutucuğu Azure'da tüketilen depolamayı gösterir. Depolama kullanım için sağlanır:

* Kasayla ilişkili bulut LRS depolama kullanımı
* Kasayla ilişkili bulut GRS depolama kullanımı

## <a name="manage-your-production-servers"></a>Üretim sunucularını yönetme
Üretim sunucularınızı yönetmek için tıklatın **ayarları**.

Altında Yönet'i **yedekleme altyapısı > üretim sunucuları**.

Üretim sunucularında menü tüm kullanılabilir üretim sunucuları listesi. Sunucu ayrıntıları açmak için listedeki bir sunucuya tıklayın.

![Korumalı öğeler](./media/backup-azure-manage-windows-server/production-server-list.png)


## <a name="open-the-azure-backup-agent"></a>Azure yedekleme Aracısı'nı açın
Açık **Microsoft Azure Yedekleme aracısı** (, makinenizde arama yaparak Bul *Microsoft Azure yedekleme*).

![Windows Server yedeklemesini zamanlama](./media/backup-azure-manage-windows-server/snap-in-search.png)

Gelen **Eylemler** yedekleme aracısını konsol sağındaki adresinde, aşağıdaki yönetim görevlerini gerçekleştirebilirsiniz:

* Sunucuyu kaydetmek
* Yedekleme zamanlaması
* Şimdi Yedekle
* Özelliklerini değiştir

![Microsoft Azure Yedekleme aracısı konsol Eylemler](./media/backup-azure-manage-windows-server/console-actions.png)

> [!NOTE]
> İçin **verileri kurtarabilirsiniz**, bkz: [bir Windows server veya Windows istemci makinesi dosyaları geri yükle](backup-azure-restore-windows-server.md).
>
>

## <a name="modify-the-backup-schedule"></a>Yedekleme zamanlamasını Değiştir
1. Microsoft Azure yedekleme Aracısı'nı **yedekleme zamanlaması**.

    ![Windows Server yedeklemesini zamanlama](./media/backup-azure-manage-windows-server/schedule-backup.png)
2. İçinde **Yedekleme Zamanlama Sihirbazı** bırakın **yedekleme öğeleri veya saatler için değişiklik yapma** seçeneğe ve tıklatın **sonraki**.

    ![Windows Server yedeklemesini zamanlama](./media/backup-azure-manage-windows-server/modify-or-stop-a-scheduled-backup.png)
3. Ekleme veya öğeleri değiştirmek isteyip istemediğinizi **yedeklenecek öğeleri seçin** ekran tıklatın **öğeleri Ekle**.

    Ayrıca ayarlayabilirsiniz **dışarıda bırakma ayarları** Sihirbazı'nda bu sayfadan. Dosya türleri ekleme yordamı okuma ya da dosyaları hariç tutmak istiyorsanız [dışarıda bırakma ayarları](#manage-exclusion-settings).
4. Dosya ve, önce yedeklemek istediğiniz klasörleri seçin **Tamam**.

    ![Windows Server yedeklemesini zamanlama](./media/backup-azure-manage-windows-server/add-items-modify.png)
5. Belirtin **yedekleme zamanlaması** tıklatıp **sonraki**.

    (En fazla günde 3 kereye) günlük veya haftalık yedeklemeler zamanlayabilirsiniz.

    ![Windows Server Yedekleme öğeleri](./media/backup-azure-manage-windows-server/specify-backup-schedule-modify-close.png)

   > [!NOTE]
   > Yedekleme zamanlaması belirtme açıklanmıştır bu ayrıntılı [makale](backup-azure-backup-cloud-as-tape.md).
   >

6. Seçin **Bekletme İlkesi** tıklatın ve yedek kopya için **sonraki**.

    ![Windows Server Yedekleme öğeleri](./media/backup-azure-manage-windows-server/select-retention-policy-modify.png)
7. Üzerinde **onay** ekranı bilgileri gözden geçirin ve tıklatın **son**.
8. Oluşturma Sihirbazı'nı tamamladıktan sonra **yedekleme zamanlaması**, tıklatın **Kapat**.

    Korumayı değiştirdikten sonra yedeklemeler doğru giderek tetikleyen olduğunu onaylayabilirsiniz **işleri** sekmesi ve değişiklikler yedekleme işleri yansıtılır onaylama.

## <a name="enable-network-throttling"></a>Ağ azaltmayı etkinleştir

Azure Backup aracısını, veri aktarımı sırasında ağ bant genişliğinin nasıl kullanıldığını denetlemenize olanak tanıyan bir azaltma sekmesi sağlar. Bu denetim sırasında verilerin yedeklemeniz gerekiyorsa, çalışma saatleri ancak Yedekleme işleminin diğer internet trafiğine engel olmasını istiyor musunuz yararlı olabilir. Verilerin azaltma aktarımı yedeklemek ve geri yükleme etkinlikleri için geçerlidir.  

Azaltmayı etkinleştirmek için:

1. İçinde **Backup Aracısı**, tıklatın **özelliklerini değiştirme**.
2. Üzerinde ** sekmesini azaltma, seçin **yedekleme işlemleri için Internet bant genişliği kullanımı daraltmayı etkinleştir**.

    ![Ağ azaltma](./media/backup-azure-manage-windows-server/throttling-dialog.png)

    Azaltma etkinleştirdikten sonra sırasında yedek veri aktarımı için izin verilen bant genişliğini belirtin **çalışma saatleri** ve **çalışılmayan saatler**.

    Bant genişliği değerler 512 KB / saniye (Kbps) başlar ve en fazla 1023 MB / saniye (Mbps) gidebilirsiniz. Ayrıca başlangıç belirleyin ve için son **çalışma saatleri**, ve haftanın hangi günleri iş dikkate gün. Belirtilen iş saatleri dışında saat İş dışı saatler olarak kabul edilir.
3. **Tamam** düğmesine tıklayın.

## <a name="manage-exclusion-settings"></a>Dışarıda bırakma ayarları yönetme
1. Açık **Microsoft Azure Yedekleme aracısı** (makinenizi arayarak bulabilirsiniz *Microsoft Azure yedekleme*).

    ![Windows Server yedeklemesini zamanlama](./media/backup-azure-manage-windows-server/snap-in-search.png)
2. Microsoft Azure yedekleme Aracısı'nı **yedekleme zamanlaması**.

    ![Windows Server yedeklemesini zamanlama](./media/backup-azure-manage-windows-server/schedule-backup.png)
3. Yedekleme zamanlaması Sihirbazı'nı bırakın, **yedekleme öğeleri veya saatler için değişiklik yapma** seçeneğe ve tıklatın **sonraki**.

    ![Windows Server yedeklemesini zamanlama](./media/backup-azure-manage-windows-server/modify-or-stop-a-scheduled-backup.png)
4. Tıklatın **dışlama ayarları**.

    ![Windows Server yedeklemesini zamanlama](./media/backup-azure-manage-windows-server/exclusion-settings.png)
5. Tıklatın **dışlama eklemek**.

    ![Windows Server yedeklemesini zamanlama](./media/backup-azure-manage-windows-server/add-exclusion.png)
6. Konum seçin ve ardından **Tamam**.

    ![Windows Server yedeklemesini zamanlama](./media/backup-azure-manage-windows-server/exclusion-location.png)
7. Dosya uzantısını Ekle **dosya türü** alan.

    ![Windows Server yedeklemesini zamanlama](./media/backup-azure-manage-windows-server/exclude-file-type.png)

    .Mp3 uzantı ekleme

    ![Windows Server yedeklemesini zamanlama](./media/backup-azure-manage-windows-server/exclude-mp3.png)

    Başka bir uzantı eklemek için tıklatın **dışlama Ekle** ve (.jpeg uzantı eklemeden) başka bir dosya türü uzantısını girin.

    ![Windows Server yedeklemesini zamanlama](./media/backup-azure-manage-windows-server/exclude-jpg.png)
8. Tüm uzantıları eklediğinizde, tıklatın **Tamam**.
9. Yedeklemeyi zamanlama Sihirbazı tıklayarak devam **sonraki** kadar **onay sayfası**, ardından **son**.

    ![Windows Server yedeklemesini zamanlama](./media/backup-azure-manage-windows-server/finish-exclusions.png)

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
**S1. Yedekleme işi durumunu neden bunu hemen portalında yansıtılan değil Azure Yedekleme aracısı tamamlandı olarak gösterir.**

Y1. Var. 15 dakika yedekleme iş durumu arasında en fazla gecikme adresindeki Azure Yedekleme aracısı hem de Azure portalında yansıtılır.

**Bir yedekleme işi başarısız olduğunda Q.2 ne kadar bir uyarı yükseltmek için sürer?**

Azure yedekleme hatası 20 dakika içinde A.2 bir uyarı oluşturulur.

**S3. Bir e-posta, bildirimleri yapılandırılırsa burada gönderilmez söz konusu var mı?**

Y3. Aşağıdaki durumlarda alındığında uyarı gürültü azaltmak için bildirim gönderilmeyecek:

* Bildirimleri saatlik yapılandırıldıysa ve bir uyarı oluşturulur ve aynı saat içinde çözümlenen
* İşleri iptal edilir.
* Özgün yedekleme işi devam ettiğinden ikinci yedekleme işi başarısız oldu.

## <a name="troubleshooting-monitoring-issues"></a>İzleme sorunlarını giderme
**Sorun:** işleri ve/veya Azure Backup Aracısı uyarıları portalda görünmez.

**Sorun giderme adımlarını:** işlem ```OBRecoveryServicesManagementAgent```, iş ve uyarı verileri Azure yedekleme hizmetine gönderir. Bu işlem bazen durumunda takılıp kalabilir veya kapatın.

1. İşlemi çalışmıyor doğrulamak için açık **Görev Yöneticisi'ni** ve olup olmadığını denetleyin ```OBRecoveryServicesManagementAgent``` işlem çalışıyor.
2. İşlemi çalışmıyor varsayılarak açmak **Denetim Masası** ve hizmetlerin listesini bulun. Başlatma veya yeniden **Microsoft Azure kurtarma Hizmetleri yönetim Aracısı**.

    Daha fazla bilgi için günlüklerine göz atın:<br/>
   `<AzureBackup_agent_install_folder>\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider*`Örneğin:<br/>
   `C:\Program Files\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider0.errlog`

## <a name="next-steps"></a>Sonraki adımlar
* [Windows Server veya Windows İstemcisi Azure'dan geri yükleme](backup-azure-restore-windows-server.md)
* Azure yedekleme hakkında daha fazla bilgi için bkz: [Azure Yedekleme'ye Genel Bakış](backup-introduction-to-azure-backup.md)
* Ziyaret [Azure yedekleme Forumu](http://go.microsoft.com/fwlink/p/?LinkId=290933)
