<properties
   pageTitle="Windows Azure Resource Manager dağıtım modelini kullanarak Azure Backup ile dosya ve klasörleri Windows'dan Azure'a yedeklemeyi öğrenme | Microsoft Azure"
   description="Bir kasa oluşturarak, Kurtarma Hizmetleri aracısını yükleyerek ve dosya ve klasörlerinizi Azure'a yedekleyerek Windows Server verilerini nasıl yedekleyeceğinizi öğrenin."
   services="backup"
   documentationCenter=""
   authors="Jim-Parker"
   manager="jwhit"
   editor=""
   keywords="how to backup; how to back up"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.date="05/10/2016"
   ms.author="jimpark;"/>

# İlk bakış: Resource Manager dağıtım modelini kullanarak Azure Backup ile dosya ve klasörleri Windows Server'dan veya istemcisinden Azure'a yedekleme

Bu makalede, Resource Manager'ı kullanarak Azure Backup ile Windows Server (veya Windows istemcisi) dosya ve klasörlerinizi Azure'a nasıl yedekleyeceğiniz açıklanmaktadır. Bu, size temel işlemler boyunca yol göstermeye yönelik bir öğreticidir. Azure Backup ile çalışmaya başlamak istiyorsanız doğru yerdesiniz.

Azure Backup hakkında daha fazla bilgi edinmek istiyorsanız bu [genel bakışı](backup-introduction-to-azure-backup.md) okuyun.

Dosya ve klasörlerin Azure'a yüklenmesi için şu etkinliklerin gerçekleştirilmesi gerekir:

![1. Adım](./media/backup-try-azure-backup-in-10-mins/step-1.png) Bir Azure aboneliği edinme (zaten mevcut değilse).<br>
![2. Adım](./media/backup-try-azure-backup-in-10-mins/step-2.png) Bir Kurtarma Hizmetleri kasası oluşturma.<br>
![3. Adım](./media/backup-try-azure-backup-in-10-mins/step-3.png) Gerekli dosyaları indirme.<br>
![4. Adım](./media/backup-try-azure-backup-in-10-mins/step-4.png) Kurtarma Hizmetleri aracısını yükleme ve kaydetme.<br>
![5. Adım](./media/backup-try-azure-backup-in-10-mins/step-5.png) Dosya ve klasörlerinizi yedekleme.

![Windows makinenizi Azure Backup ile yedekleme](./media/backup-try-azure-backup-in-10-mins/backup-process.png)

## 1. Adım: Bir Azure aboneliği edinme

Azure aboneliğiniz yoksa istediğiniz Azure hizmetine erişmenizi sağlayan [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## 2. Adım: Bir Kurtarma Hizmetleri kasası oluşturma

Dosya ve klasörlerinizi yedeklemek için, verileri depolamak istediğiniz bölgede bir Kurtarma Hizmetleri kasası oluşturmanız gerekir. Ayrıca, depolama alanınızın nasıl çoğaltılmasını istediğinizi belirlemeniz gerekir.

### Kurtarma Hizmetleri kasası oluşturmak için

1. Önceden yapmadıysanız Azure aboneliğinizi kullanarak [Azure Portal](https://portal.azure.com/)'da oturum açın.

2. Hub menüsünde **Gözat**'a tıklayın ve kaynak listesinde **Kurtarma Hizmetleri** yazıp **Kurtarma Hizmetleri kasalarına** tıklayın.

    ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-try-azure-backup-in-10-mins/browse-to-rs-vaults.png) <br/>

3. **Kurtarma Hizmetleri kasaları** menüsünde **Ekle**'ye tıklayın.

    ![Kurtarma Hizmetleri Kasası oluşturma 2. adım](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    Kurtarma Hizmetleri kasası dikey penceresi açılır ve sizden bir **Ad**, **Abonelik**, **Kaynak grubu** ve **Konum** sağlamanızı ister.

    ![Kurtarma Hizmetleri kasası oluşturma 5. adım](./media/backup-try-azure-backup-in-10-mins/rs-vault-attributes.png)

4. **Ad** alanına, kasayı tanımlayacak kolay bir ad girin.

5. Kullanılabilir abonelik listesini görmek için **Abonelik** seçeneğine tıklayın.

6. Kullanılabilir Kaynak grubu listesini görmek için **Kaynak grubu** seçeneğine, yeni bir Kaynak grubu oluşturmak için de **Yeni** seçeneğine tıklayın.

7. Kasa için coğrafi bölgeyi seçmek üzere **Konum**'a tıklayın. Bu seçim, yedekleme verilerinizin gönderildiği coğrafi bölgeyi belirler.

8. **Oluştur**'a tıklayın.

    Tamamlandıktan sonra kasanızı listede görmüyorsanız **Yenile**'ye tıklayın. Liste yenilendiğinde kasasının adına tıklayın.

### Depolama artıklığını belirlemek için
Bir Kurtarma Hizmetleri kasasını ilk oluşturduğunuzda depolamanın nasıl çoğaltılacağını belirlersiniz.

1. Panoyu açmak için yeni kasaya tıklayın.

2. Kasa panonuzla otomatik olarak açılan **Ayarlar** dikey penceresinde**Yedekleme Altyapısı**'na tıklayın.

3. Yedekleme Altyapısı dikey penceresinde, **Depolama çoğaltma türü** öğesini görüntülemek için **Yedekleme Yapılandırması**'na tıklayın.

    ![Kurtarma Hizmetleri kasası oluşturma 5. adım](./media/backup-try-azure-backup-in-10-mins/backup-infrastructure.png)

4. Kasanız için uygun depolama çoğaltma seçeneğini belirleyin.

    ![Kurtarma hizmetleri kasalarının listesi](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    Varsayılan olarak, kasanız coğrafi olarak yedekli depolamaya sahiptir. Azure'ı birincil yedekleme alanı uç noktası olarak kullanıyorsanız coğrafi olarak yedekli depolamayı kullanmaya devam edin. Azure'ı birincil olmayan yedekleme alanı uç noktası olarak kullanıyorsanız Azure'da veri depolama maliyetini düşürecek olan yerel olarak yedekli depolamayı seçin. [Coğrafi olarak yedekli](../storage/storage-redundancy.md#geo-redundant-storage) ve [yerel olarak yedekli](../storage/storage-redundancy.md#locally-redundant-storage) depolama seçenekleri hakkında daha fazla bilgiyi bu [genel bakış](../storage/storage-redundancy.md) bölümünde edinebilirsiniz.

Kasa oluşturduğunuza göre, artık Microsoft Azure Kurtarma Hizmetleri aracısını ve kasa kimlik bilgilerini indirerek, dosya ve klasörleri yedeklemeye yönelik altyapınızı hazırlayabilirsiniz.

## 3. Adım - Dosyaları indirme

1. Kurtarma Hizmetleri kasası panosunda **Ayarlar**'a tıklayın.

    ![Yedekleme hedefi dikey penceresini açma](./media/backup-try-azure-backup-in-10-mins/settings-button.png)

2. Ayarlar dikey penceresinde **Başlarken > Yedekleme**'ye tıklayın.

    ![Yedekleme hedefi dikey penceresini açma](./media/backup-try-azure-backup-in-10-mins/getting-started-backup.png)

3. Yedekleme dikey penceresinde **Yedekleme hedefi** seçeneğine tıklayın.

    ![Yedekleme hedefi dikey penceresini açma](./media/backup-try-azure-backup-in-10-mins/backup-goal.png)

4. İş yükünüz nerede çalıştırılıyor? menüsünden **Şirket içi**'ni seçin.

5. Neyi yedeklemek istiyorsunuz? menüsünden **Dosyalar ve klasörler**'i seçin ve **Tamam**'a tıklayın.

### Kurtarma Hizmetleri aracısını indirme

1. **Altyapıyı hazırlama** dikey penceresinde **Windows Server veya Windows İstemcisi için Aracı'yı indir** seçeneğine tıklayın.

    ![altyapıyı hazırlama](./media/backup-try-azure-backup-in-10-mins/prepare-infrastructure-short.png)

2. İndirme açılır menüsünde **Kaydet**'e tıklayın. Varsayılan olarak, **MARSagentinstaller.exe** dosyası İndirilenler klasörünüze kaydedilir.

### Kasa kimlik bilgilerini indirme

1. Altyapıyı hazırlama dikey penceresinde **İndir > Kaydet**'e tıklayın.

    ![altyapıyı hazırlama](./media/backup-try-azure-backup-in-10-mins/prepare-infrastructure-download.png)

## 4. Adım: Aracıyı yükleme ve kaydetme

>[AZURE.NOTE] Azure portal üzerinden yedeklemeyi etkinleştirme olanağı yakında sunulacaktır. Şu an için, dosya ve klasörlerinizi yedeklemek üzere Microsoft Azure Kurtarma Hizmetleri Aracısı'nı şirket içi olarak kullanabilirsiniz.

1. İndirilenler klasöründen (veya diğer kayıtlı konumdan) **MARSagentinstaller.exe** dosyasını bulun ve dosyaya çift tıklayın.

2. Microsoft Azure Kurtarma Hizmetleri Aracısı Kurulum Sihirbazı'nı tamamlayın. Sihirbazı tamamlamak için şunları yapmanız gerekir:

    - Yükleme ve önbellek klasörü için bir konum seçin.
    - İnternet'e bağlanmak için bir ara sunucu kullanıyorsanız ara sunucu bilgilerinizi sağlayın.
    - Kimliği doğrulanmış bir ara sunucu kullanıyorsanız kullanıcı adı ve parola bilgilerinizi sağlayın.
    - İndirilen kasa kimlik bilgilerini sağlayın
    - Şifreleme parolasını güvenli bir konuma kaydedin.

    >[AZURE.NOTE] Parolayı kaybeder veya unutursanız Microsoft, yedekleme verilerini kurtarmanıza yardımcı olamaz. Lütfen dosyayı güvenli bir konuma kaydedin. Bu dosya, bir yedeklemeyi geri yüklemek için gereklidir.

Aracı artık yüklenmiş ve makineniz kasaya kaydedilmiştir. Yedeklemenizi yapılandırıp zamanlamak için hazırsınız.

## 5. Adım: Dosya ve klasörlerinizi yedekleme

İlk yedekleme iki anahtar görev içerir:

- Yedeklemeyi zamanlama
- Dosya ve klasörleri ilk kez yedekleme

İlk yedeklemeyi tamamlamak için Microsoft Azure Kurtarma Hizmetleri aracısını kullanırsınız.

### Yedeklemeyi zamanlamak için

1. Microsoft Azure Kurtarma Hizmetleri aracısını açın. Bunu, makinenizde **Microsoft Azure Backup** aramasını yaparak bulabilirsiniz.

    ![Azure Kurtarma Hizmetleri aracısını başlatma](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)

2. Kurtarma Hizmetleri aracısında, **Yedeklemeyi Zamanla**'ya tıklayın.

    ![Windows Server yedeklemesini zamanlama](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)

3. Yedeklemeyi Zamanlama Sihirbazı'nın Başlarken sayfasında **İleri**'ye tıklayın.

4. Yedeklenecek Öğeleri Seçin sayfasında **Öğe Ekle**'ye tıklayın.

5. Yedeklemek istediğiniz dosya ve klasörleri seçin ve ardından **Tamam**'a tıklayın.

6. **Next (İleri)** düğmesine tıklayın.

7. **Yedekleme Zamanlamasını Belirtin** sayfasında, **yedekleme zamanlamasını** belirtin ve **İleri**'ye tıklayın.

    Günlük (en fazla günde üç kez olmak üzere) veya haftalık yedeklemeler zamanlayabilirsiniz.

    ![Windows Server Yedekleme öğeleri](./media/backup-try-azure-backup-in-10-mins/specify-backup-schedule-close.png)

    >[AZURE.NOTE] Yedekleme zamanlamasını belirtme konusunda daha fazla bilgi için [Bant altyapınızın yerini alması için Azure Windows Server Backup'ı kullanma](backup-azure-backup-cloud-as-tape.md) makalesine bakın.

8. **Bekletme İlkesi Seçin** sayfasında, yedekleme kopyası için **Bekletme İlkesi**'ni seçin.

    Bekletme ilkesi, yedeklemenin depolanacağı süreyi belirtir. Tüm yedekleme noktaları için bir "sabit ilke" belirtmek yerine, yedeklemenin gerçekleşme zamanını temel alan farklı bekletme ilkeleri belirtebilirsiniz. Günlük, haftalık, aylık ve yıllık bekletme ilkelerini gereksinimlerinizi karşılayacak şekilde değiştirebilirsiniz.

9. İlk Yedekleme Türünü Seçin sayfasında ilk yedekleme türünü seçin. **Ağ üzerinden otomatik olarak** seçeneğini işaretli bırakın ve ardından **İleri**'ye tıklayın.

    Otomatik olarak ağ üzerinden veya çevrimdışı yedekleme yapabilirsiniz. Bu makalenin sonraki bölümlerinde otomatik olarak yedekleme işlemi açıklanmaktadır. Çevrimdışı yedekleme işlemini tercih ediyorsanız ek bilgi için [Azure Backup'ta çevrimdışı yedekleme iş akışı](backup-azure-backup-import-export.md) makalesini gözden geçirin.

10. Onay sayfasında bilgileri gözden geçirin ve ardından **Son**'a tıklayın.

11. Sihirbaz yedekleme zamanlamasını oluşturduktan sonra **Kapat**'a tıklayın.

### Dosya ve klasörleri ilk kez yedeklemek için

1. Kurtarma Hizmetleri aracısında, ağ üzerinden ilk doldurma işlemini tamamlamak için **Şimdi Yedekle**'ye tıklayın.

    ![Windows Server şimdi yedekle](./media/backup-try-azure-backup-in-10-mins/backup-now.png)

2. Onay sayfasında, Şimdi Yedekle Sihirbazı'nın makineyi yedeklemek için kullanacağı ayarları gözden geçirin. Ardından **Yedekle**'ye tıklayın.

3. Sihirbazı kapatmak için **Kapat**'a tıklayın. Bunu yedekleme işlemi tamamlanmadan önce yaparsanız sihirbaz arka planda çalışmaya devam eder.

İlk yedekleme tamamlandıktan sonra, Yedekleme konsolunda **İş tamamlandı** durumu görünür.

![IR tamamlandı](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## Sorularınız mı var?
Sorularınız varsa veya dahil edilmesini istediğiniz herhangi bir özellik varsa [bize geri bildirim gönderin](http://aka.ms/azurebackup_feedback).

## Sonraki adımlar
- [Windows makinelerini yedekleme](backup-configure-vault.md) konusunda daha fazla bilgi edinin.
- Dosya ve klasörlerinizi yedeklediğinize göre artık [kasa ve sunucularınızı yönetebilirsiniz](backup-azure-manage-windows-server.md).
- Bir yedeklemeyi geri yüklemeniz gerekirse [dosyaları bir Windows makinesine geri yüklemek](backup-azure-restore-windows-server.md) için bu makaleyi kullanın.



<!---HONumber=Jun16_HO2-->


