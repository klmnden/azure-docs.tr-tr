---
title: "Dosyaları ve klasörleri yedeklemek için kullanım Azure Yedekleme aracısı | Microsoft Docs"
description: "Microsoft Azure Yedekleme aracısı Windows dosya ve klasörlerinizi Azure'a yedeklemek için kullanın. Bir kurtarma Hizmetleri kasası oluşturmanız, yedekleme aracısını yüklemek, yedekleme ilkesi tanımlama ve dosya ve klasörleri ilk yedeklemeyi çalıştırın."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "Yedekleme kasası; bir Windows server'ı Yedekle; Yedekleme pencereleri;"
ms.assetid: 7f5b1943-b3c1-4ddb-8fb7-3560533c68d5
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/15/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: b95dc0a83d8e5618effb573353f419e1837d30c5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="back-up-a-windows-server-or-client-to-azure-using-the-resource-manager-deployment-model"></a>Resource Manager dağıtım modelini kullanarak Windows Server veya istemcisini Azure’a yedekleme
> [!div class="op_single_selector"]
> * [Azure portal](backup-configure-vault.md)
> * [Klasik portal](backup-configure-vault-classic.md)
>
>

Bu makalede, Windows Server (veya Windows istemcisi) yedekleme açıklanmaktadır dosya ve klasörleri Azure Resource Manager dağıtım modelini kullanarak yedekleme ile azure'a.

[!INCLUDE [learn-about-deployment-models](../../includes/backup-deployment-models.md)]

![Yedekleme işlemi adımları](./media/backup-configure-vault/initial-backup-process.png)

## <a name="before-you-start"></a>Başlamadan önce
Bir sunucu veya istemci için Azure yedekleme için bir Azure hesabınızın olması gerekir. Yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma
Kurtarma Hizmetleri kasası tüm yedeklemeleri ve zaman içinde oluşturduğunuz kurtarma noktalarını depolayan bir varlıktır. Kurtarma Hizmetleri kasası, korumalı dosyalara ve klasörlere uygulanan yedekleme ilkesini de içerir. Kurtarma Hizmetleri kasası oluşturduğunuzda, aynı zamanda uygun depolama artıklığı seçeneği işaretlemeniz gerekir.

### <a name="to-create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturmak için
1. Önceden yapmadıysanız Azure aboneliğinizi kullanarak [Azure Portal](https://portal.azure.com/)'da oturum açın.
2. Hub menüsünde **Diğer hizmetler**'e tıklayın ve kaynak listesinde **Kurtarma Hizmetleri** yazıp **Kurtarma Hizmetleri kasaları** seçeneğine tıklayın.

    ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    Abonelikte kurtarma hizmetleri kasaları varsa kasalar listelenir.

3. **Kurtarma Hizmetleri kasaları** menüsünde **Ekle**'ye tıklayın.

    ![Kurtarma Hizmetleri Kasası oluşturma 2. adım](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    Kurtarma Hizmetleri kasası dikey penceresi açılır ve sizden bir **Ad**, **Abonelik**, **Kaynak Grubu** ve **Konum** sağlamanızı ister.

    ![Kurtarma Hizmetleri Kasası Oluşturma 3. adım](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. **Ad** alanına, kasayı tanımlayacak kolay bir ad girin. Adın Azure aboneliği için benzersiz olması gerekir. 2 ila 50 karakterden oluşan bir ad yazın. Ad bir harf ile başlamalıdır ve yalnızca harf, rakam ve kısa çizgi içerebilir.

5. **Abonelik** bölümündeki açılır menüyü kullanarak Azure aboneliğini seçin. Yalnızca bir abonelik kullanıyorsanız bu abonelik görüntülenir ve sonraki adıma atlayabilirsiniz. Hangi aboneliğin kullanılacağından emin değilseniz varsayılan (veya önerilen) aboneliği kullanın. Yalnızca kuruluş hesabınızın birden çok Azure aboneliği ile ilişkili olması durumunda birden çok seçenek olur.

6. **Kaynak grubu** bölümünde:

    * Yeni bir Kaynak grubu oluşturmak istiyorsanız **Yeni oluştur**’u seçin.
    Veya
    * **Var olanı kullan**’ı seçin ve açılır menüyü kullanarak mevcut Kaynak gruplarının listesine bakın.

  Kaynak grupları hakkında eksiksiz bilgiler için bkz. [Azure Resource Manager’a genel bakış](../azure-resource-manager/resource-group-overview.md).

7. Kasa için coğrafi bölgeyi seçmek üzere **Konum**'a tıklayın. Bu seçim, yedekleme verilerinizin gönderildiği coğrafi bölgeyi belirler.

8. Kurtarma Hizmetleri kasası dikey penceresinin alt kısmındaki **Oluştur**’a tıklayın.

  Kurtarma Hizmetleri kasasının oluşturulması birkaç dakika sürebilir. Portalın sağ üst kısmından durum bildirimlerini izleyin. Kasanız oluşturulduktan sonra Kurtarma Hizmetleri kasaları listesinde görünür. Birkaç dakika sonra kasayı görmezseniz **Yenile**’ye tıklayın.

  ![Yenile düğmesine tıklayın](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

  Kasanızı Kurtarma Hizmetleri kasaları listesinde gördükten sonra, depolama yedekliliğini ayarlamaya hazır olursunuz.


### <a name="set-storage-redundancy"></a>Küme depolama artıklığı
Bir Kurtarma Hizmetleri kasasını ilk oluşturduğunuzda depolamanın nasıl çoğaltılacağını belirlersiniz.

1. **Kurtarma Hizmetleri kasaları** dikey penceresinden yeni kasaya tıklayın.

    ![Kurtarma Hizmetleri kasası listesinden yeni kasayı seçin](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    Kasayı seçtiğinizde **Kurtarma Hizmetleri kasası** dikey penceresi daralır ve Ayarlar dikey penceresi (*en üstünde kasanın adı bulunur*) ve ile kasa ayrıntıları dikey penceresi açılır.

    ![Yeni kasa için depolama yapılandırmasını görüntüleme](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)

2. Yeni kasanın Ayarlar dikey penceresinde dikey kaydırma çubuğunu kullanarak Yönet bölümüne inin ve **Yedekleme Altyapısı**’na tıklayın.

  Yedekleme Altyapısı dikey penceresi açılır.

3. Yedekleme Altyapısı dikey penceresinde, **Yedekleme Yapılandırması**’na tıklayarak **Yedekleme Yapılandırması** dikey penceresini açın.

  ![Yeni kasa için depolama yapılandırması ayarlama](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)

4. Kasanız için uygun depolama çoğaltma seçeneğini belirleyin.

  ![depolama yapılandırması seçenekleri](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

  Varsayılan olarak, kasanız coğrafi olarak yedekli depolamaya sahiptir. Azure'ı birincil yedek depolama uç noktası olarak kullanıyorsanız, **Coğrafi olarak yedekli** seçeneğini kullanmaya devam edin. Azure’u birincil yedek depolama uç noktası olarak kullanmıyorsanız, Azure depolama maliyetlerini azaltan **Yerel olarak yedekli** seçeneğini belirleyin. [Coğrafi olarak yedekli](../storage/common/storage-redundancy.md#geo-redundant-storage) ve [yerel olarak yedekli](../storage/common/storage-redundancy.md#locally-redundant-storage) depolama seçenekleri hakkında daha fazla bilgiyi [Depolama yedekliliğine genel bakış](../storage/common/storage-redundancy.md) bölümünden edinebilirsiniz.

Bir kasa oluşturduğunuza göre indirme ve Microsoft Azure kurtarma Hizmetleri Aracısı'nı yükleyerek, kasa kimlik bilgilerini indirme ve aracı kasaya kaydetmek için bu kimlik bilgilerini kullanarak dosyaları ve klasörleri yedeklemek için altyapınızı hazırlayın.

## <a name="configure-the-vault"></a>Kasa yapılandırma

1. Kurtarma Hizmetleri kasası dikey penceresinin (yeni oluşturduğunuz kasa için) Başlarken bölümünde **Yedekle**’ye tıklayın, ardından **Yedeklemeye Başlama** dikey penceresinde **Yedekleme hedefi**’ne tıklayın.

  ![Yedekleme hedefi dikey penceresini açma](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

  **Yedekleme Hedefi** dikey penceresi açılır. Kurtarma Hizmetleri kasası önceden yapılandırılmışsa, sonra **yedekleme hedefi** dikey pencere açılır tıkladığınızda **yedekleme** kurtarma Hizmetleri kasası dikey penceresi.

  ![Yedekleme hedefi dikey penceresini açma](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. **İş yükünüz nerede çalışıyor?** açılır menüsünden **Şirket içi**’ni seçin.

  Windows Server veya Windows bilgisayarınız Azure üzerinde olmayan fiziksel bir makine olduğu için **Şirket içi** seçeneğini belirlersiniz.

3. **Neleri yedeklemek istiyorsunuz?** menüsünden **Dosyalar ve klasörler**'i seçin ve **Tamam**'a tıklayın.

  ![Dosya ve klasörleri yedekleme](./media/backup-try-azure-backup-in-10-mins/set-file-folder.png)

  Tamam'a tıkladıktan sonra **Yedekleme hedefi**’nin yanında bir onay işareti görünür ve **Altyapıyı hazırlama** dikey penceresi açılır.

  ![Yedekleme hedefi yapılandırılmıştır, bundan sonra altyapıyı hazırlayın](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. **Altyapıyı hazırlama** dikey penceresinde **Windows Server veya Windows İstemcisi için Aracı'yı indir** seçeneğine tıklayın.

  ![altyapıyı hazırlama](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

  Windows Server Essential kullanıyorsanız Windows Server Essential aracısını indirmeyi seçin. Açılır menü, MARSAgentInstaller.exe dosyasını çalıştırma veya kaydetme seçeneğini sunar.

  ![MARSAgentInstaller iletişim kutusu](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. İndirme açılır penceresinde **Kaydet**'e tıklayın.

  Varsayılan olarak, **MARSagentinstaller.exe** dosyası İndirilenler klasörünüze kaydedilir. Yükleyici tamamlandığında yükleyiciyi çalıştırmak veya klasörü açmak isteyip istemediğinizi soran bir açılır pencere görüntülenir.

  ![altyapıyı hazırlama](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

  Aracıyı yüklemeniz henüz gerekli değildir. Kasa kimlik bilgilerini indirdikten sonra aracıyı yükleyebilirsiniz.

6. **Altyapıyı hazırlama** dikey penceresinde **İndir**'e tıklayın.

  ![kasa kimlik bilgilerini indirme](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

  Kasa kimlik bilgileri, İndirmeler klasörünüze indirilir. Kasa kimlik bilgilerini indirme tamamlandıktan sonra kimlik bilgilerini açmak veya kaydetmek isteyip istemediğinizi soran bir açılır pencere görüntülenir. **Kaydet**’e tıklayın. Yanlışlıkla **Aç**’a tıklarsanız, kasa kimlik bilgilerini açmaya çalışan iletişim kutusu başarısız olur. Kasa kimlik bilgilerini açamazsınız. Sonraki adıma geçin. Kasa kimlik bilgileri İndirmeler klasöründedir.   

  ![kasa kimlik bilgilerini indirme tamamlandı](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-the-agent"></a>Aracıyı yükleme ve kaydetme

> [!NOTE]
> Azure portal üzerinden yedeklemeyi etkinleştirme olanağı henüz mevcut değildir. Dosya ve klasörlerinizi yedeklemek üzere Microsoft Azure Kurtarma Hizmetleri Aracısı'nı kullanın.
>

1. İndirilenler klasöründen (veya diğer kayıtlı konumdan) **MARSagentinstaller.exe** dosyasını bulun ve dosyaya çift tıklayın.

  Yükleyici, Kurtarma Hizmetleri aracısı ayıklama, yükleme ve kaydetme sırasında bir dizi ileti sunar.

  ![Kurtarma Hizmetleri aracısı yükleyici kimlik bilgilerini çalıştırma](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. Microsoft Azure Kurtarma Hizmetleri Aracısı Kurulum Sihirbazı'nı tamamlayın. Sihirbazı tamamlamak için şunları yapmanız gerekir:

  * Yükleme ve önbellek klasörü için bir konum seçin.
  * İnternet'e bağlanmak için bir ara sunucu kullanıyorsanız ara sunucu bilgilerinizi sağlayın.
  * Kimliği doğrulanmış bir ara sunucu kullanıyorsanız kullanıcı adı ve parola bilgilerinizi sağlayın.
  * İndirilen kasa kimlik bilgilerini sağlayın
  * Şifreleme parolasını güvenli bir konuma kaydedin.

  > [!NOTE]
  > Parolayı kaybeder veya unutursanız Microsoft, yedekleme verilerini kurtarmanıza yardımcı olamaz. Dosyayı güvenli bir konuma kaydedin. Bu dosya, bir yedeklemeyi geri yüklemek için gereklidir.
  >
  >

Aracı artık yüklenmiş ve makineniz kasaya kaydedilmiştir. Yedeklemenizi yapılandırıp zamanlamak için hazırsınız.

## <a name="network-and-connectivity-requirements"></a>Ağ ve Bağlantı Gereksinimleri

Makine/proxy sınırlı Internet erişimi, güvenlik duvarı ayarlarını makine/proxy aşağıdaki URL'ler izin verecek şekilde yapılandırıldığından emin olun: <br>
    1. www.msftncsi.com
    2. *.Microsoft.com
    3. *.WindowsAzure.com
    4. *.microsoftonline.com
    5. *.windows.ne


## <a name="create-the-backup-policy"></a>Yedekleme ilkesi oluşturma
Yedekleme kurtarma noktası alınma zamanlaması ve kurtarma noktalarının bekletileceği süre ilkesidir. Microsoft Azure Yedekleme aracısı, dosyalar ve klasörler için yedekleme ilkesi oluşturmak için kullanın.

### <a name="to-create-a-backup-schedule"></a>Bir yedekleme zamanlaması oluşturmak için
1. Microsoft Azure yedekleme Aracısı'nı açın. Bunu, makinenizde **Microsoft Azure Backup** aramasını yaparak bulabilirsiniz.

    ![Azure Backup aracısını başlatma](./media/backup-configure-vault/snap-in-search.png)
2. Yedekleme aracının içinde **Eylemler** bölmesinde tıklatın **yedekleme zamanlaması** yedeklemeyi Zamanlama Sihirbazı başlatmak için.

    ![Windows Server yedeklemesini zamanlama](./media/backup-configure-vault/schedule-first-backup.png)

3. Üzerinde **Başlarken** sayfa zamanlama Yedekleme Sihirbazı'nın tıklatın **sonraki**.
4. Üzerinde **yedeklenecek öğeleri seçin** sayfasında, **öğeleri Ekle**.

  Öğeleri seçin iletişim kutusu açılır.

5. Dosya ve koruyun ve ardından istediğiniz klasörleri seçin **Tamam**.
6. İçinde **yedeklenecek öğeleri seçin** sayfasında, **sonraki**.
7. Üzerinde **yedekleme zamanlamasını belirtin** sayfasında, yedekleme zamanlamasını belirtin ve tıklayın **sonraki**.

    Günlük (en fazla günde üç kez olmak üzere) veya haftalık yedeklemeler zamanlayabilirsiniz.

    ![Windows Server Yedekleme öğeleri](./media/backup-configure-vault/specify-backup-schedule-close.png)

   > [!NOTE]
   > Yedekleme zamanlamasını belirtme konusunda daha fazla bilgi için [Bant altyapınızın yerini alması için Azure Windows Server Backup'ı kullanma](backup-azure-backup-cloud-as-tape.md) makalesine bakın.
   >
   >

8. Üzerinde **bekletme ilkesi seçin** belirli bekletme ilkeleri sayfasında, tıklatın ve yedek kopya için **sonraki**.

    Bekletme İlkesi yedekleme depolanan süreyi belirtir. Tüm yedekleme noktaları için bir "sabit ilke" belirtmek yerine, yedeklemenin gerçekleşme zamanını temel alan farklı bekletme ilkeleri belirtebilirsiniz. Günlük, haftalık, aylık ve yıllık bekletme ilkelerini gereksinimlerinizi karşılayacak şekilde değiştirebilirsiniz.
9. İlk Yedekleme Türünü Seçin sayfasında ilk yedekleme türünü seçin. **Ağ üzerinden otomatik olarak** seçeneğini işaretli bırakın ve ardından **İleri**'ye tıklayın.

    Otomatik olarak ağ üzerinden veya çevrimdışı yedekleme yapabilirsiniz. Bu makalenin sonraki bölümlerinde otomatik olarak yedekleme işlemi açıklanmaktadır. Çevrimdışı yedekleme işlemini tercih ediyorsanız ek bilgi için [Azure Backup'ta çevrimdışı yedekleme iş akışı](backup-azure-backup-import-export.md) makalesini gözden geçirin.
10. Onay sayfasında bilgileri gözden geçirin ve ardından **Son**'a tıklayın.
11. Sihirbaz yedekleme zamanlamasını oluşturduktan sonra **Kapat**'a tıklayın.

### <a name="enable-network-throttling"></a>Ağ azaltmayı etkinleştir
Microsoft Azure Yedekleme aracısı, ağ azaltma sağlar. Veri aktarımı sırasında ağ bant genişliğinin nasıl kullanıldığını denetimleri azaltma. Bu denetim sırasında verilerin yedeklemeniz gerekiyorsa, çalışma saatleri ancak Yedekleme işleminin diğer Internet trafiğine engel olmasını istiyor musunuz yararlı olabilir. Azaltma yedeklemek ve geri yükleme etkinlikleri için geçerlidir.

> [!NOTE]
> Ağ azaltma (hizmet paketleri ile) Windows Server 2008 R2 SP1, Windows Server 2008 SP2 veya Windows 7 üzerinde kullanılabilir değil. Özellik azaltma Azure Backup ağ hizmet kalitesi (QoS) yerel işletim sisteminde ilgilenir. Azure Backup bu işletim sistemlerini korumak için de, QoS bu platformlarda kullanılabilir sürümü Azure Backup ağ azaltma ile çalışmaz. Ağ azaltma kullanılabilir tüm diğer bağlı [desteklenen işletim sistemleri](backup-azure-backup-faq.md).
>
>

**Ağ azaltma etkinleştirmek için**

1. Microsoft Azure Yedekleme aracısı, tıklatın **özelliklerini değiştirme**.

    ![Özelliklerini değiştir](./media/backup-configure-vault/change-properties.png)
2. Üzerinde **azaltma** sekmesine **yedekleme işlemleri için Internet bant genişliği kullanımı daraltmayı etkinleştir** onay kutusu.

    ![Ağ azaltma](./media/backup-configure-vault/throttling-dialog.png)
3. Azaltma etkinleştirdikten sonra sırasında yedek veri aktarımı için izin verilen bant genişliğini belirtin **çalışma saatleri** ve **çalışılmayan saatler**.

    Bant genişliği değerler 512 kilobit / saniye (Kbps) başlar ve 1,023 megabayt (MBps) saniyede gidebilirsiniz. Ayrıca başlangıç belirleyin ve için son **çalışma saatleri**, ve haftanın hangi günleri dikkate alınan iş günlerini. Belirtilen iş saatleri kabul dışında saat saat İş dışı.
4. **Tamam** düğmesine tıklayın.

### <a name="to-back-up-files-and-folders-for-the-first-time"></a>Dosya ve klasörleri ilk kez yedeklemek için
1. Yedekleme aracı tıklatın **Şimdi Yedekle** ağ üzerinden ilk doldurma işlemini tamamlamak için.

    ![Windows Server şimdi yedekle](./media/backup-configure-vault/backup-now.png)
2. Onay sayfasında, Şimdi Yedekle Sihirbazı'nın makineyi yedeklemek için kullanacağı ayarları gözden geçirin. Ardından **Yedekle**'ye tıklayın.
3. Sihirbazı kapatmak için **Kapat**'a tıklayın. Bunu yedekleme işlemi tamamlanmadan önce yaparsanız sihirbaz arka planda çalışmaya devam eder.

İlk yedekleme tamamlandıktan sonra, Yedekleme konsolunda **İş tamamlandı** durumu görünür.

![IR tamamlandı](./media/backup-configure-vault/ircomplete.png)

## <a name="questions"></a>Sorularınız mı var?
Sorularınız varsa veya dahil edilmesini istediğiniz herhangi bir özellik varsa [bize geri bildirim gönderin](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Sonraki adımlar
Sanal makineler veya diğer iş yüklerini yedekleme hakkında ek bilgi için bkz:

* Dosya ve klasörlerinizi yedeklediğinize göre artık [kasa ve sunucularınızı yönetebilirsiniz](backup-azure-manage-windows-server.md).
* Bir yedeklemeyi geri yüklemeniz gerekirse [dosyaları bir Windows makinesine geri yüklemek](backup-azure-restore-windows-server.md) için bu makaleyi kullanın.
