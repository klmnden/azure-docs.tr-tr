---
title: Azure Backup MARS Aracısı ile Windows makinelerini yedekleme
description: Windows makineleri yedeklemek için Azure Backup Microsoft Kurtarma Hizmetleri (MARS) Aracısı'nı kullanın.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: raynew
ms.openlocfilehash: 7a1bd6da68b49481429709c7e4fd37dd5c07ae2c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60810184"
---
# <a name="back-up-windows-machines-with-the-azure-backup-mars-agent"></a>Azure Backup MARS Aracısı ile Windows makinelerini yedekleme

Bu makalede kullanarak Windows makinelerini yedekleme açıklanmaktadır [Azure Backup](backup-overview.md) hizmeti ve Microsoft Azure kurtarma Hizmetleri (MARS) aracısı, Azure Backup aracısı olarak da bilinir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Önkoşulları doğrulayın ve bir kurtarma Hizmetleri kasası oluşturun.
> * MARS Aracısını indirip ayarlama
> * Bir yedekleme İlkesi ve zamanlama oluşturun.
> * Geçici yedekleme gerçekleştirme.

## <a name="about-the-mars-agent"></a>MARS Aracısı hakkında

MARS Aracısı Azure Backup tarafından dosyaları, klasörleri ve sistem durumu yedekleme şirket içi makinelerin ve Azure sanal makinelerini bir yedekleme Azure kurtarma Hizmetleri kasasına yedeklemek için kullanılır. Aracıyı aşağıdaki gibi çalıştırabilirsiniz:

- Böylece bunlar doğrudan Azure yedekleme kurtarma Hizmetleri kasası için yedekleme Aracısı'nı doğrudan şirket içi Windows makineler üzerinde çalıştırın.
- Aracı Azure VM'de belirli dosyaları ve klasörleri yedeklemek için Windows (yan yana Azure VM yedekleme uzantısı ile) çalıştıran VM'ler çalıştırır.
- Bir Microsoft Azure Backup sunucusu (MABS) veya bir System Center Data Protection - Manager (DPM) sunucu Aracısı'nı çalıştırın. Bu senaryoda, MABS/DPM makineler ve iş yüklerini yedeklemek ve MARS Aracısı'nı kullanarak azure'daki bir kasaya ardından MABS/DPM yedekler.
Neleri yedekleyebilir, aracının yüklü olduğu bağlıdır.

> [!NOTE]
> Azure VM'lerin yedeklenmesi için birincil yöntem bir Azure Backup uzantısı VM'de kullanmaktır. Bu, VM'nin tamamını yedekler. Yükleme ve VM'de belirli dosyaları ve klasörleri yedeklemek istiyorsanız MARS Aracısı uzantısı ile birlikte kullanmak isteyebilirsiniz. [Daha fazla bilgi edinin](backup-architecture.md#architecture-direct-backup-of-azure-vms).

![Yedekleme işleminin adımları](./media/backup-configure-vault/initial-backup-process.png)

## <a name="before-you-start"></a>Başlamadan önce

- [Bilgi nasıl](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-server-machines-or-azure-vm-files-or-folders) Azure Backup, MARS Aracısı Windows makinelerle yedekler.
- [Hakkında bilgi edinin](backup-architecture.md#architecture-back-up-to-dpmmabs) MARS Aracısı bir ikincil MABS veya DPM sunucusunda çalışan yedekleme mimarisi.
- [Gözden geçirme](backup-support-matrix-mars-agent.md) ne desteklenir ve MARS Aracısı ile ne yedeklenebilir.
- Yedeklemek istediğiniz makinelerde internet erişimi doğrulayın.
- Bir sunucu veya istemcisini Azure'a yedekleme için bir Azure hesabınızın olması gerekir. Yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde.

### <a name="verify-internet-access"></a>İnternet erişimi doğrulayın

Makinenize sınırlı internet erişimi, güvenlik duvarı ayarlarını makinede veya proxy bu URL'ler izin verdiğinden emin olun ve IP adresi:

**URL'leri**

- www\.msftncsi.com
- *.Microsoft.com
- *.WindowsAzure.com
- *.microsoftonline.com
- *.windows.net

**IP adresi**

- 20.190.128.0/18
- 40.126.0.0/18


## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Kurtarma Hizmetleri kasası, tüm yedeklemeleri ve zaman içinde oluşturduğunuz kurtarma noktalarını depolar ve yedeklenen makinelere uygulanan yedekleme ilkesini de içerir. Şu şekilde bir kasa oluşturun:

1. Oturum [Azure portalı](https://portal.azure.com/) Azure aboneliğinizi kullanarak.
2. Arama alanına yazın **kurtarma Hizmetleri** tıklatıp **kurtarma Hizmetleri kasaları**.

    ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png)

3. Üzerinde **kurtarma Hizmetleri kasaları** menüsünü tıklatın **+ Ekle**.

    ![Kurtarma Hizmetleri Kasası oluşturma 2. adım](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

4. **Ad** alanına, kasayı tanımlayacak kolay bir ad girin.

   - Adın Azure aboneliği için benzersiz olması gerekir.
   - Bu, 2 ile 50 karakter içerebilir.
   - Bir harf ile başlamalı ve yalnızca harf, rakam ve kısa çizgi içerebilir.

5. Azure aboneliği, kaynak grubu ve kasa oluşturulması gereken coğrafi bölgeyi seçin. Yedekleme verileri kasaya gönderilir. Sonra **Oluştur**’a tıklayın.

    ![Kurtarma Hizmetleri Kasası Oluşturma 3. adım](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

   - Bu, kasasının oluşturulması biraz zaman alabilir.
   - Portalın sağ üst alandaki durum bildirimlerini izleyin. Birkaç dakika sonra kasayı görmezseniz, tıklayın **Yenile**.

     ![Yenile düğmesine tıklayın](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)

### <a name="set-storage-redundancy"></a>Küme depolama yedekliliği

Azure yedekleme kasası için depolama otomatik olarak işler. Bu depolamanın nasıl çoğaltılacağını belirtmenize gerek.

1. **Kurtarma Hizmetleri kasaları** dikey penceresinden yeni kasaya tıklayın. Altında **ayarları** bölümünde **özellikleri**.
2. İçinde **özellikleri**altında **yedekleme yapılandırması**, tıklayın **güncelleştirme**.

3. Depolama çoğaltma türü seçin ve tıklayın **Kaydet**.

      ![Yeni kasa için depolama yapılandırması ayarlama](./media/backup-try-azure-backup-in-10-mins/recovery-services-vault-backup-configuration.png)

- Bu, Azure'ı birincil yedek depolama uç noktası olarak kullanıyorsanız, devam varsayılan öneririz **coğrafi olarak yedekli** ayarı.
- Azure’u birincil yedek depolama uç noktası olarak kullanmıyorsanız, Azure depolama maliyetlerini azaltan **Yerel olarak yedekli** seçeneğini belirleyin.
- Daha fazla bilgi edinin [coğrafi](../storage/common/storage-redundancy-grs.md) ve [yerel](../storage/common/storage-redundancy-lrs.md) yedeklilik.

## <a name="download-the-mars-agent"></a>MARS Aracısı'nı indirme

Yedeklemek istediğiniz makineleri yükleme için MARS Aracısı'nı indirin.

- Tüm makinelerde Aracısı'nı zaten yüklediyseniz, en son sürümü çalıştırdığınızdan emin olun.
- En son sürümü Portalı'nda ya da kullanarak bir [doğrudan indirme](https://aka.ms/azurebackup_agent)

1. Bir kasadaki altında **Başlarken**, tıklayın **yedekleme**.

    ![Yedekleme hedefi dikey penceresini açma](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

2. İçinde **iş yükünüz çalıştığı?** seçin **şirket içi**. Bir Azure sanal makinesine MARS Aracısı yükleme isteseniz bile, bu seçenek seçmelisiniz.
3. İçinde **neleri yedeklemek istiyorsunuz?** seçin **dosya ve klasörleri** ve/veya **sistem durumu**. Diğer birkaç seçenek vardır, ancak ikincil yedekleme sunucusu çalıştırıyorsanız, yalnızca bu desteklenir. Tıklayın **altyapıyı hazırlama**.

      ![Dosya ve klasörleri yedekleme](./media/backup-try-azure-backup-in-10-mins/set-file-folder.png)

4. Üzerinde **altyapıyı hazırlama**altında **yükleme kurtarma Hizmetleri Aracısı**, MARS Aracısı'nı indirin.

    ![altyapıyı hazırlama](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

5. İndirme açılır penceresinde **Kaydet**'e tıklayın. Varsayılan olarak, **MARSagentinstaller.exe** dosyası İndirilenler klasörünüze kaydedilir.

6. Şimdi Denetle **zaten indirme veya en son kurtarma Hizmetleri Aracısı'nı kullanarak**ve ardından kasa kimlik bilgilerini indirin.

    ![kasa kimlik bilgilerini indirme](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

7. **Kaydet**’e tıklayın. Dosya indirilenler klasörünüze indirilir. Kasa kimlik bilgileri dosyası açılamıyor.

## <a name="install-and-register-the-agent"></a>Aracıyı yükleme ve kaydetme

1. Çalıştırma **MARSagentinstaller.exe** yedeklemek istediğiniz makineleri dosyası.
2. MARS Aracısı Kurulum Sihirbazı'ndaki > **yükleme ayarları**, önbelleğe yönelik kullanmak için aracı ve bir konuma yüklemek istediğiniz yeri belirtin. Ardından **İleri**'ye tıklayın.
   - Azure Backup, önbellek Azure'a göndermeden önce veri anlık görüntülerini depolamak için kullanır.
   - Önbellek konumunu yedeklemeniz veri boyutunun en az % 5'si kadar boş disk alanı olmalıdır.

     ![MARS Sihirbazı yükleme ayarları](./media/backup-configure-vault/mars1.png)

2. İçinde **Proxy Yapılandırması**, bir Windows makinede çalışan Aracısı internet'e nasıl bağlanacağını belirtin. Ardından **İleri**'ye tıklayın.

   - Özel bir kullanıyorsanız, proxy Ara sunucu ayarlarını ve gerekirse kimlik bilgilerini belirtin.
   - Aracıyı erişimi olması gerektiğini unutmayın [bu URL'leri](#verify-internet-access).

     ![MARS Sihirbazı internet erişimi](./media/backup-configure-vault/mars2.png)

3. İçinde **yükleme** Önkoşul denetimi gözden geçirin ve tıklayın **yükleme**.
4. Aracı yüklendikten sonra tıklayın **kayıt işlemine geç**.
5. İçinde **Kaydetme Sihirbazı'nı** > **kasa kimliği**göz atın ve indirdiğiniz kimlik bilgileri dosyasını seçin. Ardından **İleri**'ye tıklayın.

    ![Register - kasa kimlik bilgileri](./media/backup-configure-vault/register1.png)

6. İçinde **şifreleme ayarı**, şifrelemek ve makine için yedekleme şifresini çözmek için kullanılacak bir parola belirtin.

    - Şifreleme parolasını güvenli bir konuma kaydedin.
    - Kaybeder veya parolayı unutursanız Microsoft Yedekleme verilerini kurtarmanıza yardımcı olamaz. Dosyayı güvenli bir konuma kaydedin. Bir yedeklemeyi geri yüklemek için ihtiyacınız.

7. Tıklayın **son**. Aracı artık yüklenmiş ve makineniz kasaya kaydedilmiştir. Yedeklemenizi yapılandırıp zamanlamak için hazırsınız.

## <a name="create-a-backup-policy"></a>Bir yedekleme ilkesi oluşturma

Ne zaman veri kurtarma noktaları oluşturmak için anlık yedekleme ilkesini belirtir ve kurtarma noktalarını Beklet ne kadar.

- MARS Aracısı'nı kullanarak bir yedekleme İlkesi yapılandırmanız.
- Azure yedekleme, otomatik olarak gün ışığından yararlanma saatine göre (DST) dikkate almaz. Bu, gerçek zaman ve zamanlanmış yedekleme zamanı arasında bazı tutarsızlık neden olabilir.

Bir ilke şu şekilde oluşturun:

1. Her makineye MARS Aracısı'nı açın. Bunu, makinenizde **Microsoft Azure Backup** aramasını yaparak bulabilirsiniz.
2. İçinde **eylemleri**, tıklayın **yedeklemeyi zamanla**.

    ![Windows Server yedeklemesini zamanlama](./media/backup-configure-vault/schedule-first-backup.png)

3. Yedeklemeyi zamanlama Sihirbazı'nda > **Başlarken**, tıklayın **sonraki**.
4. İçinde **yedeklenecek öğeleri seçin**, tıklayın **öğeleri Ekle**.
5. İçinde **öğeleri seçin**, yedeklemek istediğinizi seçin. Daha sonra, **Tamam**'a tıklayın.
6. İçinde **yedeklenecek öğeleri seçin** sayfasında **sonraki**.
7. İçinde **yedekleme zamanlamasını belirtin** sayfasında, günlük veya haftalık yedeklemeler almak istediğiniz zaman belirtin. Ardından **İleri**'ye tıklayın.

    - Bir yedek bir kurtarma noktası oluşturulur.
    - Ortamınızda oluşturulan kurtarma noktalarının sayısını, yedekleme zamanlamanızı bağlıdır.

1. Günde üç kez kadar günlük yedeklemeler zamanlayabilirsiniz. Örneğin, ekran görüntüsü iki günlük yedeklemeler, bir gece yarısı ve 18: 00 teker gösterir.

    ![Günlük Zamanlama](./media/backup-configure-vault/day-schedule.png)

9. Haftalık yedeklemeler da çalıştırabilirsiniz. Örneğin, ekran görüntüsü alternatif her Pazar & Çarşamba 09:30:00 ve 01: 00'da gerçekleştirilen yedeklemeleri gösterir.

    ![Haftalık Zamanlama](./media/backup-configure-vault/week-schedule.png)

8. Üzerinde **bekletme ilkesi seçin** sayfasında, geçmiş verilerinizin kopyalarını nasıl depoladığınız belirtin. Ardından **İleri**'ye tıklayın.

   - Saklama ayarları hangi kurtarma noktalarının depolanması gerekir ve ne kadar bunlar için saklanması gerektiğini belirtin.
   - Örneğin, bir günlük saklama ayarı ayarladığınızda, günlük bekletme için belirtilen zamanda, belirli gün sayısı için en son kurtarma noktası korunur gösterir. Veya başka bir örnek olarak, her ayın 30 üzerinde oluşturulan kurtarma noktası 12 ay süreyle saklanması gerektiğini belirtmek için aylık bekletme ilkesi belirtebilirsiniz.
   - Günlük ve haftalık kurtarma noktası bekletme süresi, genellikle yedekleme zamanlaması ile örtüşür. Yedekleme zamanlamaya göre tetiklendiğinde yedekleme tarafından oluşturulan kurtarma noktası günlük veya bekletme ilkesi haftalık belirtilen süre için depolandığı anlamına.
   - Örneğin, aşağıdaki ekran görüntüsünde:
     - Gece yarısı ve 18: 00 günlük yedeklemeler yedi gün boyunca tutulur.
     - Bir Cumartesi gece yarısı ve 18: 00'üzerinde alınan yedeklemeler için 4 hafta tutulur.
     - Cumartesi gece yarısı ve 18: 00 ayın son haftanın günleri alınan yedeklemeler 12 ay boyunca tutulur. -Mart Ayının Son haftasından Bu, bir Cumartesi günleri alınan yedeklemeler 10 yıl boyunca tutulur.

   ![Bekletme örneği](./media/backup-configure-vault/retention-example.png)

11. İçinde **ilk yedekleme türünü** ağ üzerinden veya çevrimdışı ilk yedekleme, almak üzere nasıl belirtin. Ardından **İleri**'ye tıklayın.

10. İçinde **onay**, bilgileri gözden geçirin ve ardından **son**.
11. Sihirbaz yedekleme zamanlamasını oluşturduktan sonra **Kapat**'a tıklayın.

### <a name="perform-the-initial-backup-offline"></a>Çevrimdışı ilk yedekleme gerçekleştirin

Bir ilk otomatik olarak ağ üzerinden veya çevrimdışı çalıştırabileceğiniz yedekleyin. İlk yedekleme için çevrimdışı dengeli dağıtım, büyük miktarlarda veri aktarımı için ağ bant genişliği çok sayıda gerektiren varsa yararlıdır. Çevrimdışı bir aktarım şu şekilde yapabilirsiniz:

1. Bir hazırlama konumu için yedekleme verilerini Yaz
2. Bir veya daha fazla SATA diskleri için hazırlama konumu verileri kopyalamak için AzureOfflineBackupDiskPrep Aracı'nı kullanın.
3. Aracı, Azure içeri aktarma işi oluşturur. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/storage/common/storage-import-export-service) Azure içeri ve dışarı aktarma hakkında.
4. SATA diskleri için Azure veri merkezi gönderirsiniz.
5. Veri merkezinde, disk verilerini, bir Azure depolama hesabına kopyalanır.
6. Azure Backup verileri depolama hesabından kasasına kopyalar ve artımlı yedeklemeler zamanlanır.

[Daha fazla bilgi edinin](backup-azure-backup-import-export.md) çevrimdışı dengeli dağıtım hakkında.

### <a name="enable-network-throttling"></a>Ağ kapasitesi azaltmayı etkinleştirme

Ağ kapasitesi azaltma etkinleştirerek ağ bant genişliği MARS aracısı tarafından nasıl kullanıldığını kontrol edebilirsiniz. Azaltma, iş saatlerinde yedekleme ancak ne kadar bant yedeklemesi için kullanıldığını kontrol etmek istediğiniz ve etkinlik geri yüklemek istiyorsanız yararlıdır.

- Azure yedekleme ağı kullanan azaltma [hizmet kalitesi (QoS)](https://docs.microsoft.com/windows-server/networking/technologies/qos/qos-policy-top) yerel işletim sisteminin üzerinde.
- Ağ kısıtlama için yedekleme, Windows Server 2008 R2 ve üzeri ve Windows 7 ve sonraki sürümlerde kullanılabilir. İşletim sistemlerini en son hizmet paketleri çalıştırıyor olmalıdır.

Ağ azaltmayı şu şekilde etkinleştirin:

1. MARS Aracısı tıklayın **özelliklerini değiştirme**.
2. Üzerinde **azaltma** sekmesinde, onay **yedekleme işlemleri için internet bant genişliği kullanımını azaltmayı etkinleştir**.

    ![Ağ kapasitesi azaltma](./media/backup-configure-vault/throttling-dialog.png)
3. İzin verilen bant genişliği, çalışma sırasında ve iş saatleri dışında belirtin. Bant genişliği değer 512 KB/sn ile başlayın ve en fazla 1,023 MB/sn gidin. Daha sonra, **Tamam**'a tıklayın.

## <a name="run-an-ad-hoc-backup"></a>Geçici bir yedeklemeyi çalıştırma

1. MARS Aracısı tıklayın **Şimdi Yedekle**. Bu, ağ üzerinden ilk çoğaltmayı devre dışı başlatıyor.

    ![Windows Server şimdi yedekle](./media/backup-configure-vault/backup-now.png)

2. İçinde **onay**, ayarları gözden geçirin ve tıklayın **yedekleme**.
3. Sihirbazı kapatmak için **Kapat**'a tıklayın. Yedekleme tamamlanmadan önce bunu yaparsanız sihirbaz arka planda çalışmaya devam eder.

İlk yedekleme tamamlandıktan sonra, Yedekleme konsolunda **İş tamamlandı** durumu görünür.

## <a name="next-steps"></a>Sonraki adımlar

[Bilgi edinmek için nasıl](backup-azure-restore-windows-server.md) dosyaları geri yükleme.
