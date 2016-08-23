<properties
    pageTitle="İlk Bakış: Azure sanal makinelerini bir yedekleme kasasıyla koruma | Microsoft Azure"
    description="Azure sanal makinelerini Yedekleme kasasıyla koruyun. Öğretici, Azure'da kasa oluşturma, VM'leri kaydetme, ilke oluşturma ve VM'leri koruma işlemlerini açıklar."
    services="backup"
    documentationCenter=""
    authors="markgalioto"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/29/2016"
    ms.author="markgal; jimpark"/>


# İlk bakış: Azure sanal makinelerini yedekleme

> [AZURE.SELECTOR]
- [İlk bakış: Sanal makineleri bir kurtarma hizmetleri kasasıyla koruma](backup-azure-vms-first-look-arm.md)
- [İlk Bakış: Azure sanal makinelerini bir yedekleme kasasıyla koruma](backup-azure-vms-first-look.md)

Bu öğretici bir Azure sanal makinesini (VM) Azure'daki bir yedekleme kasasına yedeklemeye yönelik adımlar boyunca size yol gösterir. Bu makalede sanal makineleri yedeklemeye yönelik Klasik modeli veya Service Manager dağıtım modeli açıklanmaktadır. Bir sanal makineyi bir Kaynak Grubuna ait Kurtarma Hizmetleri kasasına yedeklemek istiyorsanız bkz. [İlk bakış: Sanal makineleri bir kurtarma hizmetleri kasasıyla koruma](backup-azure-vms-first-look-arm.md). Bu öğreticiyi başarıyla tamamlamak için şu önkoşulların mevcut olması gerekir:

- Azure aboneliğinizde bir VM oluşturmuş olmanız gerekir.
- VM'nin Azure genel IP adreslerine bağlantısı olmalıdır. Ek bilgi için bkz. [Ağ bağlantısı](./backup-azure-vms-prepare.md#network-connectivity).

Bir VM'yi yedeklemek için beş ana adım gerçekleştirilir:  

![birinci adım](./media/backup-azure-vms-first-look/step-one.png) Yedekleme kasası oluşturma veya mevcut bir yedekleme kasası tanımlama. <br/>
![ikinci adım](./media/backup-azure-vms-first-look/step-two.png) Sanal makineleri bulmak ve kaydetmek için Klasik Azure portalını kullanma. <br/>
![üçüncü adım](./media/backup-azure-vms-first-look/step-three.png) VM Aracısı'nı yükleme. <br/>
![dördüncü adım](./media/backup-azure-vms-first-look/step-four.png) Sanal makineleri korumaya yönelik ilkeyi oluşturma. <br/>
![beşinci adım](./media/backup-azure-vms-first-look/step-five.png) Yedeklemeyi çalıştırma.

![VM yedekleme işleminin üst düzey görünümü](./media/backup-azure-vms-first-look/backupazurevm-classic.png)

>[AZURE.NOTE] Azure'da kaynak oluşturmaya ve kaynaklarla çalışmaya yönelik iki dağıtım modeli mevcuttur: [Resource Manager ve Klasik](../resource-manager-deployment-model.md). Bu öğretici, Klasik Azure portalında oluşturulabilen VM'lerle kullanıma yöneliktir. Azure Backup hizmeti Resource Manager temelli sanal makineleri destekler. Sanal makineleri bir kurtarma hizmetleri kasasına yedekleme hakkında bilgi için bkz. [İlk bakış: Sanal makineleri bir kurtarma hizmetleri kasasıyla koruma](backup-azure-vms-first-look-arm.md).



## 1. Adım - Bir VM için yedekleme kasası oluşturma

Yedekleme kasası, zaman içinde oluşturulan tüm yedeklemeleri ve kurtarma noktalarını depolayan bir varlıktır. Yedekleme kasası, yedeklenmekte olan sanal makinelere uygulanan yedekleme ilkelerini de içerir.

1. [Klasik Azure portalında](http://manage.windowsazure.com/) oturum açın.

2. Azure portalının sol alt köşesinde **Yeni**'ye tıklayın

    ![Yeni menüsüne tıklama](./media/backup-azure-vms-first-look/new-button.png)

3. Hızlı Oluşturma sihirbazında **Veri Hizmetleri** > **Kurtarma Hizmetleri** > **Yedekleme Kasası** > **Hızlı Oluştur**'a tıklayın.

    ![Yedekleme kasası oluşturma](./media/backup-azure-vms-first-look/new-vault-wizard-one-subscription.png)

    Sihirbaz sizden **Ad** ve **Bölge** bilgilerini ister. Birden fazla aboneliği yönetiyorsanız aboneliği seçmeye yönelik bir iletişim kutusu görünür.

4. **Ad** alanına, kasayı tanımlayacak kolay bir ad girin. Adın Azure aboneliği için benzersiz olması gerekir.

5. **Bölge** içinde, kasa için coğrafi bölgeyi seçin. Kasa koruduğu sanal makinelerle aynı bölgede **olmalıdır**.

    VM'nizin hangi bölgede bulunduğunu bilmiyorsanız bu sihirbazı kapatın ve Azure hizmetleri listesinde **Virtual Machines**'e tıklayın. Konum sütunu bölgenin adını belirtir. Birden çok bölgede sanal makineniz varsa her bölgede bir yedekleme kasası oluşturun.

6. Sihirbazda **Abonelik** iletişim kutusu yoksa sonraki adıma geçin. Birden çok abonelik ile çalışıyorsanız yeni bir yedekleme kasası ile ilişkilendirmek üzere bir abonelik seçin.

    ![Kasa bildirimi oluşturma](./media/backup-azure-vms-first-look/backup-vaultcreate.png)

7. **Kasa Oluştur**'a tıklayın. Yedekleme kasasının oluşturulması biraz zaman alabilir. Portalın alt kısmındaki durum bildirimlerini izleyin.

    ![Kasa bildirimi oluşturma](./media/backup-azure-vms-first-look/create-vault-demo.png)

    Bir ileti, kasanın başarıyla oluşturulduğunu onaylar. Kasa **Kurtarma Hizmetleri** sayfasında **Etkin** olarak listelenir.

    ![Kasa bildirimi oluşturma](./media/backup-azure-vms-first-look/create-vault-demo-success.png)

8. **Kurtarma Hizmetleri** sayfasındaki kasa listesinde, **Hızlı Başlangıç** sayfasını açmak için oluşturduğunuz kasayı seçin.

    ![Yedekleme kasalarının listesi](./media/backup-azure-vms-first-look/active-vault-demo.png)

9. **Hızlı Başlangıç** sayfasında, depolama çoğaltma seçeneğini açmak için **Yapılandır**'a tıklayın.
    ![Yedekleme kasalarının listesi](./media/backup-azure-vms-first-look/configure-storage.png)

10. **Depolama çoğaltma** seçeneğinde, kasanız için bir çoğaltma seçeneği belirleyin.

    ![Yedekleme kasalarının listesi](./media/backup-azure-vms-first-look/backup-vault-storage-options-border.png)

    Varsayılan olarak, kasanız coğrafi olarak yedekli depolamaya sahiptir. Bu, birincil yedeklemenizse coğrafi olarak yedekli depolamayı seçin. Daha düşük dayanıklılık düzeyinde olan daha uygun maliyetli bir seçenek istiyorsanız yerel olarak yedekli depolamayı seçin. Coğrafi olarak yedekli ve yerel olarak yedekli depolama seçenekleri hakkında daha fazla bilgiyi [Azure Storage çoğaltmaya genel bakış](../storage/storage-redundancy.md) bölümünde edinebilirsiniz.

Kasanız için depolama seçeneğini belirledikten sonra, VM'yi kasa ile ilişkilendirmek için hazır duruma gelirsiniz. İlişkilendirmeyi başlatmak için Azure sanal makinelerini bulun ve kaydedin.

## 2. Adım - Azure sanal makinelerini bulma ve kaydetme
VM'yi bir kasaya kaydetmeden önce yeni VM'leri tanımlamak için bulma işlemini çalıştırın. Bu, bulut hizmeti adı ve bölge gibi ek bilgilerle birlikte, abonelikteki sanal makinelerin listesini döndürür.

1. [Klasik Azure portalında](http://manage.windowsazure.com/) oturum açın

2. Klasik Azure portalında, Kurtarma Hizmetleri kasalarının listesini açmak için **Kurtarma Hizmetleri**'ne tıklayın.
    ![İş yükünü seçme](./media/backup-azure-vms-first-look/recovery-services-icon.png)

3. Kasa listesinden bir VM'nin yedekleneceği kasayı seçin.

    Kasanızı seçtiğinizde kasa **Hızlı Başlangıç** sayfasında açılır

4. Kasa menüsünden **Kayıtlı Öğeler**'e tıklayın.

    ![İş yükünü seçme](./media/backup-azure-vms-first-look/configure-registered-items.png)

5. **Tür** menüsünden **Azure Sanal Makinesi**'ni seçin.

    ![İş yükünü seçme](./media/backup-azure-vms/discovery-select-workload.png)

6. Sayfanın alt kısmındaki **BUL** seçeneğine tıklayın.
    ![Bul düğmesi](./media/backup-azure-vms/discover-button-only.png)

    Bulma işlemi, sanal makinelerin tabloya alınması nedeniyle birkaç dakika sürebilir. Ekranın alt kısmında, işlemin çalıştığını bildiren bir bildirim bulunur.

    ![VM'leri bulma](./media/backup-azure-vms/discovering-vms.png)

    İşlem tamamlandığında bildirim değişir.

    ![Bulma işlemi bitti](./media/backup-azure-vms-first-look/discovery-complete.png)

7. Sayfanın alt kısmındaki **KAYDET** seçeneğine tıklayın.
    ![Kaydet düğmesi](./media/backup-azure-vms-first-look/register-icon.png)

8. **Öğeleri Kaydet** kısayol menüsünde, kaydetmek istediğiniz sanal makineleri seçin.

    >[AZURE.TIP] Birden çok sanal makine aynı anda kaydedilebilir.

    Seçtiğiniz her sanal makine için bir iş oluşturulur.

9. **İşler** sayfasına gitmek için bildirim içinde **İşi Görüntüle**'ye tıklayın.

    ![İşi kaydetme](./media/backup-azure-vms/register-create-job.png)

    Sanal makine ayrıca, kayıt işleminin durumu ile birlikte kayıtlı öğeler listesinde de görünür.

    ![Kayıt durumu 1](./media/backup-azure-vms/register-status01.png)

    İşlem tamamlandığında, durum *kayıtlı* durumunu yansıtacak şekilde değişir.

    ![Kayıt durumu 2](./media/backup-azure-vms/register-status02.png)

## 3. Adım - VM Aracısı'nı sanal makineye yükleme

Backup uzantısının çalışması için Azure VM Aracısı'nın Azure sanal makinesine yüklenmesi gerekir. VM'niz Azure galerisinden oluşturulmuşsa VM Aracısı VM üzerinde zaten mevcuttur. [VM'lerinizi koruma](backup-azure-vms-first-look.md#step-4---protect-azure-virtual-machines) adımına atlayabilirsiniz.

VM'nizin bir şirket içi veri merkezinden geçişi sağlandıysa VM için VM Aracısı büyük olasılıkla yüklü değildir. VM'yi koruma aşamasına geçmeden önce sanal makine üzerinde VM Aracısı'nı yüklemeniz gerekir. VM Aracısı'nı yükleme konusunda ayrıntılı adımlar için bkz. [VM'leri Yedekleme makalesinin VM Aracısı bölümü](backup-azure-vms-prepare.md#vm-agent).


## 4. Adım - Yedekleme ilkesini oluşturma
İlk yedekleme işini tetiklemeden önce, yedekleme anlık görüntülerinin alınma zamanlamasını ayarlayın. Yedekleme anlık görüntülerinin alınma zamanlaması ve bu anlık görüntülerin tutulma süresinin uzunluğu, yedekleme ilkesini oluşturur. Bekletme bilgileri, Üst öğe-orta öğe-alt öğe rotasyon düzenini temel alır.

1. Klasik Azure portalında **Kurtarma Hizmetleri**'nin altındaki yedekleme kasasına gidin ve **Kayıtlı Öğeler**'e tıklayın.
2. Açılan menüden **Azure Sanal Makine**'yi seçin.

    ![Portalda iş yükünü seçme](./media/backup-azure-vms/select-workload.png)

3. Sayfanın alt kısmındaki **KORU** seçeneğine tıklayın.
    ![Koru'ya tıklama](./media/backup-azure-vms-first-look/protect-icon.png)

    **Öğeleri Koruma sihirbazı** görünür ve *yalnızca* kayıtlı ve korumasız sanal makineleri listeler.

    ![Korumayı ölçekli olarak yapılandırma](./media/backup-azure-vms/protect-at-scale.png)

4. Korumak istediğiniz sanal makineleri seçin.

    Aynı ada sahip iki veya daha fazla sanal makine mevcutsa sanal makineleri ayırt etmek için Bulut Hizmeti'ni kullanın.

5. **Korumayı yapılandır** menüsünde, tanımladığınız sanal makineleri korumak için var olan bir ilkeyi seçin veya yeni bir ilke oluşturun.

    Yeni Yedekleme kasaları, kasayla ilişkili bir varsayılan ilke içerir. Bu ilke, her akşam günlük bir anlık görüntü alır ve günlük anlık görüntü 30 gün süreyle tutulur. Her bir yedekleme ilkesi, kendisiyle ilişkili birden çok sanal makineye sahip olabilir. Ancak sanal makine tek seferde yalnızca bir ilkeyle ilişkili olabilir.

    ![Yeni ilke ile koruma](./media/backup-azure-vms/policy-schedule.png)

    >[AZURE.NOTE] Yedekleme ilkesi, zamanlanmış yedeklemeler için bir bekletme düzeni içerir. Var olan bir yedekleme ilkesini seçerseniz sonraki adımda bekletme seçeneklerini değiştiremezsiniz.

6. **Bekletme Aralığı** üzerinde belirli yedekleme noktaları için günlük, haftalık, aylık ve yıllık kapsam tanımlayın.

    ![Sanal makine, kurtarma noktası ile yedeklenir](./media/backup-azure-vms/long-term-retention.png)

    Bekletme ilkesi, bir yedeklemenin depolanacağı süreyi belirtir. Yedeklemenin ne zaman oluşturulduğuna bağlı olarak, farklı bekletme ilkeleri belirtebilirsiniz.

7. **Korumayı Yapılandır** işlerinin listesini görüntülemek için **İşler**'e tıklayın.

    ![Korumayı yapılandır işi](./media/backup-azure-vms/protect-configureprotection.png)

    İlkeyi belirlediğinize göre, artık sonraki adıma geçebilir ve ilk yedeklemeyi çalıştırabilirsiniz.

## 5. Adım - İlk yedekleme

Bir sanal makine bir ilke ile koruma altına alındıktan sonra, bu ilişkiyi **Korumalı Öğeler** sekmesinde görüntüleyebilirsiniz. İlk yedekleme gerçekleşene kadar, **Koruma Durumu** **Korumalı - (ilk yedekleme bekleniyor)** olarak gösterilir. Varsayılan olarak, ilk zamanlanmış yedekleme *ilk yedekleme* olacaktır.

![Yedekleme beklemede](./media/backup-azure-vms-first-look/protection-pending-border.png)

İlk yedeklemeyi hemen başlatmak için:

1. **Korumalı Öğeler** sayfasında, sayfanın alt kısmındaki **Şimdi Yedekle** seçeneğine tıklayın.
    ![Şimdi Yedekle simgesi](./media/backup-azure-vms-first-look/backup-now-icon.png)

    Azure Backup hizmeti, ilk yedekleme işlemi için bir yedekleme işi oluşturur.

2. İşlerin listesini görüntülemek için **İşler**'e tıklayın.

    ![Yedekleme devam ediyor](./media/backup-azure-vms-first-look/protect-inprogress.png)

    İlk yedekleme tamamlandığında sanal makinenin **Korumalı Öğeler** sekmesindeki durumu *Korumalı* olacaktır.

    ![Sanal makine, kurtarma noktası ile yedeklenir](./media/backup-azure-vms/protect-backedupvm.png)

    >[AZURE.NOTE] Sanal makineleri yedekleme işlemi, yerel bir işlemdir. Bir bölgedeki sanal makineleri, başka bir bölgedeki bir yedek kasaya yedekleyemezsiniz. Bu nedenle, yedeklenmesi gereken VM'lerin bulunduğu her Azure bölgesi için bu bölgede en az bir yedekleme kasası oluşturulmalıdır.

## Sonraki adımlar
Bir VM'yi başarıyla yedeklediğinize göre, sonraki birkaç adım ilginizi çekebilir. En mantıklı adım, bir VM'ye veri geri yükleme konusunda bilgi edinmektir. Bununla birlikte, verilerinizin güvenliğini nasıl koruyacağınızı ve maliyetleri nasıl en aza indireceğinizi anlamanıza yardımcı olacak yönetim görevleri mevcuttur.

- [Sanal makinelerinizi yönetme ve izleme](backup-azure-manage-vms.md)
- [Sanal makineleri geri yükleme](backup-azure-restore-vms.md)
- [Sorun giderme rehberi](backup-azure-vms-troubleshoot.md)


## Sorularınız mı var?
Sorularınız varsa veya dahil edilmesini istediğiniz herhangi bir özellik varsa [bize geri bildirim gönderin](http://aka.ms/azurebackup_feedback).



<!--HONumber=Aug16_HO1-->


