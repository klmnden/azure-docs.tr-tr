---
title: "Azure yedekleme kasaları ve sunucuları Klasik dağıtım modelini kullanarak Azure yönetmek | Microsoft Docs"
description: "Azure yedekleme kasaları ve sunucuları yönetmek nasıl öğrenmek için bu öğreticiyi kullanın."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: tysonn
ms.assetid: f175eb12-0905-437f-91fd-eaee03ab6e81
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: markgal;
ms.openlocfilehash: 91451b2cdc42ed05ef7c1ba9c66ad5b4b45dd788
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-azure-backup-vaults-and-servers-using-the-classic-deployment-model"></a>Klasik dağıtım modelini kullanarak Azure Backup kasa ve sunucularını yönetme
> [!div class="op_single_selector"]
> * [Resource Manager](backup-azure-manage-windows-server.md)
> * [Klasik](backup-azure-manage-windows-server-classic.md)
>
>

Bu makalede, Klasik Azure portalı ve Microsoft Azure Yedekleme aracısı aracılığıyla sunulan yedekleme yönetim görevlerini genel bir bakış bulabilirsiniz.

> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

> [!IMPORTANT]
> Artık Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltebilirsiniz. Ayrıntılı bilgi için [Backup kasasını Kurtarma Hizmetleri kasasına yükseltme](backup-azure-upgrade-backup-to-recovery-services.md) makalesine bakın. Microsoft, Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltmenizi önerir.<br/> 15 Ekim 2017’den itibaren, PowerShell kullanarak Backup kasaları oluşturamayacaksınız. **1 Kasım 2017’ye kadar**:
>- Yükseltilmemiş olan tüm Backup kasaları Kurtarma Hizmetleri kasalarına otomatik olarak yükseltilecektir.
>- Klasik portalda yedekleme verilerinize erişemeyeceksiniz. Bunun yerine, Kurtarma Hizmetleri kasalarındaki yedekleme verilerinize erişmek için Azure portalını kullanabilirsiniz.
>

## <a name="management-portal-tasks"></a>Yönetim Portalı görevleri
1. Oturum [Yönetim Portalı](https://manage.windowsazure.com).
2. Tıklatın **kurtarma Hizmetleri**, hızlı başlangıç sayfasını görüntülemek için yedekleme kasasının adını tıklatın.

    ![Kurtarma Hizmetleri](./media/backup-azure-manage-windows-server-classic/rs-left-nav.png)

Sayfanın üst kısmındaki hızlı başlangıç seçenekleri belirleyerek, kullanılabilir yönetim görevlerini görebilirsiniz.

![Sekmeleri yönetme](./media/backup-azure-manage-windows-server-classic/qs-page.png)

### <a name="dashboard"></a>Pano
Seçin **Pano** sunucusu için kullanım genel bakış için. **Kullanıma genel bakış** içerir:

* Bulut için kayıtlı Windows sunucuları sayısı
* Bulutta korumalı Azure sanal makine sayısı
* Azure'da tüketilen toplam depolama alanı
* En son işlerin durumunu

Pano alt kısmında, aşağıdaki görevleri gerçekleştirebilirsiniz:

* **Sertifika yönetmek** - sunucuyu kaydetmek ve ardından bu sertifikayı güncelleştirmek için kullanmak için bir sertifika kullanıldıysa. Kasa kimlik bilgileri kullanıyorsanız kullanmayın **Yönet sertifika**.
* **Silme** -geçerli yedekleme kasası siler. Bir yedekleme kasası artık kullanılıyorsa, depolama alanı boşaltmak için silebilirsiniz. **Silme** tüm kayıtlı sunucuları kasadan silindikten sonra yalnızca etkinleştirilir.

![Yedekleme Pano görevleri](./media/backup-azure-manage-windows-server-classic/dashboard-tasks.png)

## <a name="registered-items"></a>Kayıtlı öğeler
Seçin **kayıtlı öğeler** bu kasaya kayıtlı sunucularının adlarını görüntülemek için.

![Kayıtlı öğeler](./media/backup-azure-manage-windows-server-classic/registered-items.png)

**Türü** filtre varsayılanları için Azure sanal makine. Bu kasaya kayıtlı sunucularının adlarını görüntülemek için seçin **Windows server** açılan menüden.

Buradan, aşağıdaki görevleri gerçekleştirebilirsiniz:

* **Yeniden kayda izin** - kullanabileceğiniz bir sunucu için bu seçenek belirlendiğinde, **Kayıt Sihirbazı'nı** yedeklemeyle sunucuyu kaydetmek için şirket içi Microsoft Azure Yedekleme aracısı, ikinci kez Kasa. Sertifika bir hata nedeniyle yeniden kaydetmeniz gerekebilir veya bir sunucusunun derlenmeye sahip.
* **Silme** -bir sunucu yedek Kasası'nı siler. Tüm sunucuyla ilişkili tüm depolanmış veriler hemen silinir.

    ![Kayıtlı öğeleri görevleri](./media/backup-azure-manage-windows-server-classic/registered-items-tasks.png)

## <a name="protected-items"></a>Korumalı öğeler
Seçin **korunan öğeler** sunucularından yedeklenen öğeleri görüntülemek için.

![Korumalı öğeler](./media/backup-azure-manage-windows-server-classic/protected-items.png)

## <a name="configure"></a>Yapılandırma
Gelen **yapılandırma** sekmesini uygun depolama artıklığı seçeneği seçebilirsiniz. Depolama artıklığı seçeneği seçmek için en iyi bir kasası oluşturduktan sonra ve herhangi bir makine için kayıtlı önce sağ saattir.

> [!WARNING]
> Bir öğe Kasası'na kaydedildikten sonra depolama artıklığı seçeneği kilitli ve değiştirilemez.
>
>

![Yapılandırma](./media/backup-azure-manage-windows-server-classic/configure.png)

Bu makalede daha fazla bilgi için bkz: [depolama artıklığı](../storage/common/storage-redundancy.md).

## <a name="microsoft-azure-backup-agent-tasks"></a>Microsoft Azure Yedekleme aracısı görevleri
### <a name="console"></a>Konsol
Açık **Microsoft Azure Yedekleme aracısı** (makinenizi arayarak bulabilirsiniz *Microsoft Azure yedekleme*).

![Yedekleme aracısı](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

Gelen **Eylemler** aşağıdaki yönetim görevlerini gerçekleştirebilirsiniz yedekleme aracısını konsol sağında kullanılabilir:

* Sunucuyu kaydetmek
* Yedekleme zamanlaması
* Şimdi Yedekle
* Özelliklerini değiştir

![Aracı konsol eylemleri](./media/backup-azure-manage-windows-server-classic/console-actions.png)

> [!NOTE]
> İçin **verileri kurtarabilirsiniz**, bkz: [bir Windows server veya Windows istemci makinesi dosyaları geri yükle](backup-azure-restore-windows-server.md).
>
>

### <a name="modify-an-existing-backup"></a>Varolan bir yedeği değiştirme
1. Microsoft Azure yedekleme Aracısı'nı **yedekleme zamanlaması**.

    ![Windows Server yedeklemesini zamanlama](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
2. İçinde **Yedekleme Zamanlama Sihirbazı** bırakın **yedekleme öğeleri veya saatler için değişiklik yapma** seçeneğe ve tıklatın **sonraki**.

    ![Zamanlanmış yedekleme değiştirme](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
3. Ekleme veya öğeleri değiştirmek isteyip istemediğinizi **yedeklenecek öğeleri seçin** ekran tıklatın **öğeleri Ekle**.

    Ayrıca ayarlayabilirsiniz **dışarıda bırakma ayarları** Sihirbazı'nda bu sayfadan. Dosya türleri ekleme yordamı okuma ya da dosyaları hariç tutmak istiyorsanız [dışarıda bırakma ayarları](#exclusion-settings).
4. Dosya ve, önce yedeklemek istediğiniz klasörleri seçin **Tamam**.

    ![Öğe ekle](./media/backup-azure-manage-windows-server-classic/add-items-modify.png)
5. Belirtin **yedekleme zamanlaması** tıklatıp **sonraki**.

    (En fazla günde 3 kereye) günlük veya haftalık yedeklemeler zamanlayabilirsiniz.

    ![Yedekleme zamanlamasını belirtin](./media/backup-azure-manage-windows-server-classic/specify-backup-schedule-modify-close.png)

   > [!NOTE]
   > Yedekleme zamanlaması belirtme açıklanmıştır bu ayrıntılı [makale](backup-azure-backup-cloud-as-tape.md).
   >
   >
6. Seçin **Bekletme İlkesi** tıklatın ve yedek kopya için **sonraki**.

    ![Bekletme İlkesi seçin](./media/backup-azure-manage-windows-server-classic/select-retention-policy-modify.png)
7. Üzerinde **onay** ekranı bilgileri gözden geçirin ve tıklatın **son**.
8. Oluşturma Sihirbazı'nı tamamladıktan sonra **yedekleme zamanlaması**, tıklatın **Kapat**.

    Korumayı değiştirdikten sonra yedeklemeler doğru giderek tetikleyen olduğunu onaylayabilirsiniz **işleri** sekmesi ve değişiklikler yedekleme işleri yansıtılır onaylama.

### <a name="enable-network-throttling"></a>Ağ azaltmayı etkinleştir
Azure Backup aracısını, veri aktarımı sırasında ağ bant genişliğinin nasıl kullanıldığını denetlemenize olanak tanıyan bir azaltma sekmesi sağlar. Bu denetim sırasında verilerin yedeklemeniz gerekiyorsa, çalışma saatleri ancak Yedekleme işleminin diğer internet trafiğine engel olmasını istiyor musunuz yararlı olabilir. Verilerin azaltma aktarımı yedeklemek ve geri yükleme etkinlikleri için geçerlidir.  

Azaltmayı etkinleştirmek için:

1. İçinde **Backup Aracısı**, tıklatın **özelliklerini değiştirme**.
2. Seçin **yedekleme işlemleri için Internet bant genişliği kullanımı daraltmayı etkinleştir** onay kutusu.

    ![Ağ azaltma](./media/backup-azure-manage-windows-server-classic/throttling-dialog.png)
3. Azaltma etkinleştirdikten sonra sırasında yedek veri aktarımı için izin verilen bant genişliğini belirtin **çalışma saatleri** ve **çalışılmayan saatler**.

    Bant genişliği değerler 512 KB / saniye (Kbps) başlar ve en fazla 1023 MB / saniye (Mbps) gidebilirsiniz. Ayrıca başlangıç belirleyin ve için son **çalışma saatleri**, ve haftanın hangi günleri iş dikkate gün. Belirtilen iş saatleri dışında saat İş dışı saatler olarak kabul edilir.
4. **Tamam** düğmesine tıklayın.

## <a name="exclusion-settings"></a>Dışlama ayarları
1. Açık **Microsoft Azure Yedekleme aracısı** (makinenizi arayarak bulabilirsiniz *Microsoft Azure yedekleme*).

    ![Açık Yedekleme aracısı](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)
2. Microsoft Azure yedekleme Aracısı'nı **yedekleme zamanlaması**.

    ![Windows Server yedeklemesini zamanlama](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
3. Yedekleme zamanlaması Sihirbazı'nı bırakın, **yedekleme öğeleri veya saatler için değişiklik yapma** seçeneğe ve tıklatın **sonraki**.

    ![Bir zamanlamayı değiştirmek](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
4. Tıklatın **dışlama ayarları**.

    ![Çıkarılacak öğeleri seçin](./media/backup-azure-manage-windows-server-classic/exclusion-settings.png)
5. Tıklatın **dışlama eklemek**.

    ![Özel durumları ekleme](./media/backup-azure-manage-windows-server-classic/add-exclusion.png)
6. Konum seçin ve ardından **Tamam**.

    ![Dışlama için bir konum seçin](./media/backup-azure-manage-windows-server-classic/exclusion-location.png)
7. Dosya uzantısını Ekle **dosya türü** alan.

    ![Dosya türüne göre Dışla](./media/backup-azure-manage-windows-server-classic/exclude-file-type.png)

    .Mp3 uzantı ekleme

    ![Dosya türü örneği](./media/backup-azure-manage-windows-server-classic/exclude-mp3.png)

    Başka bir uzantı eklemek için tıklatın **dışlama Ekle** ve (.jpeg uzantı eklemeden) başka bir dosya türü uzantısını girin.

    ![Başka bir dosya türü örneği](./media/backup-azure-manage-windows-server-classic/exclude-jpg.png)
8. Tüm uzantıları eklediğinizde, tıklatın **Tamam**.
9. Yedeklemeyi zamanlama Sihirbazı tıklayarak devam **sonraki** kadar **onay sayfası**, ardından **son**.

    ![Dışlama onayı](./media/backup-azure-manage-windows-server-classic/finish-exclusions.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Windows Server veya Windows İstemcisi Azure'dan geri yükleme](backup-azure-restore-windows-server.md)
* Azure yedekleme hakkında daha fazla bilgi için bkz: [Azure Yedekleme'ye Genel Bakış](backup-introduction-to-azure-backup.md)
* Ziyaret [Azure yedekleme Forumu](http://go.microsoft.com/fwlink/p/?LinkId=290933)
