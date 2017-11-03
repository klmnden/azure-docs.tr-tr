---
title: "Windows server ya da iş istasyonu Azure (Klasik modeli) yedekleme | Microsoft Docs"
description: "Yedekleme Windows sunucularını veya istemcilerini Azure yedekleme kasasına. Azure Backup aracısını kullanarak dosyalarınızı ve klasörlerinizi yedekleme kasasına koruma ilgili temel bilgiler geçer."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "Yedekleme kasası; bir Windows server'ı Yedekle; Yedekleme pencereleri;"
ms.assetid: 3b543bfd-8978-4f11-816a-0498fe14a8ba
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: a8daa6a4655b72936b6299c0fa5b80459ffa5da3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="back-up-a-windows-server-or-workstation-to-azure-using-the-classic-portal"></a>Bir Windows server veya iş istasyonu için Azure Klasik portalı kullanarak yedekleme
> [!div class="op_single_selector"]
> * [Klasik portal](backup-configure-vault-classic.md)
> * [Azure portal](backup-configure-vault.md)
>
>

Bu makalede, Azure için ortamınızı hazırlayın ve bir Windows server (veya iş istasyonu) yedeklemek için izlemeniz gereken yordamları açıklanmaktadır. Ayrıca Yedekleme çözümünüzün dağıtma konuları ele alınmaktadır. Azure Backup ilk kez çalışırken düşünüyorsanız, bu makalede hızlı bir şekilde, işleminde size kılavuzluk eder.

Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: Resource Manager ve klasik. Bu makale klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

## <a name="before-you-start"></a>Başlamadan önce
Bir sunucu veya istemci için Azure yedekleme için bir Azure hesabınızın olması gerekir. Yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde.

## <a name="create-a-backup-vault"></a>Yedekleme kasası oluşturma
Bir sunucu veya istemci dosyaları ve klasörleri yedeklemek için verileri depolamak istediğiniz coğrafi bölgede bir yedekleme kasası oluşturmanız gerekir.

> [!IMPORTANT]
> Mart 2017’den itibaren Backup kasaları oluşturmak için klasik portalı kullanamayacaksınız.
>
> Artık Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltebilirsiniz. Ayrıntılı bilgi için [Backup kasasını Kurtarma Hizmetleri kasasına yükseltme](backup-azure-upgrade-backup-to-recovery-services.md) makalesine bakın. Microsoft, Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltmenizi önerir.<br/> **15 Ekim 2017**’den itibaren, artık PowerShell kullanarak Backup kasaları oluşturamayacaksınız. <br/> **1 Kasım 2017'den itibaren**:
>- Yükseltilmemiş olan tüm Backup kasaları Kurtarma Hizmetleri kasalarına otomatik olarak yükseltilecektir.
>- Klasik portalda yedekleme verilerinize erişemeyeceksiniz. Bunun yerine, Kurtarma Hizmetleri kasalarındaki yedekleme verilerinize erişmek için Azure portalını kullanabilirsiniz.
>


## <a name="download-the-vault-credential-file"></a>Kasa kimlik bilgilerini indirin
Şirket içi makineyi verileri Azure'a yedekleyebilirsiniz önce bir yedekleme kasası ile kimliğinizin doğrulanması gerekiyor. Kimlik doğrulaması yoluyla sağlanır *kasa kimlik bilgileri*. Kasa kimlik bilgilerini Klasik Portalı'ndan güvenli bir kanal üzerinden indirilir. Sertifika özel anahtarı, portal veya hizmeti devam etmez.

### <a name="to-download-the-vault-credential-file-to-a-local-machine"></a>Kasa kimlik bilgilerini yerel makineye indirmek için
1. Sol gezinti bölmesinde **kurtarma Hizmetleri**ve ardından oluşturduğunuz yedekleme kasası seçin.

    ![IR tamamlandı](./media/backup-configure-vault-classic/rs-left-nav.png)
2. Hızlı Başlangıç sayfasında, tıklatın **indirme kasa kimlik bilgileri**.

   Klasik Portalı'nı kasa adını ve geçerli tarih bir bileşimini kullanarak bir kasa kimlik bilgisi oluşturur. Kasa kimlik bilgileri dosyası yalnızca kayıt iş akışı sırasında kullanılır ve 48 saat sonra süresi dolar.

   Kasa kimlik bilgilerini portalından indirilebilir.
3. Tıklatın **kaydetmek** için kasa kimlik bilgilerini yerel hesap indirmeler klasörüne indirmek için. Öğesini de seçebilirsiniz **Kaydet** gelen **kaydetmek** menü kasa kimlik bilgileri dosyası için bir konum belirtin.

   > [!NOTE]
   > Kasa kimlik bilgilerini makinenizden erişilebilir bir konuma kaydedilir emin olun. Bir dosya paylaşımı veya Sunucu İleti Bloğu'depolanıyorsa, erişim izniniz olduğunu doğrulayın.
   >
   >

## <a name="download-install-and-register-the-backup-agent"></a>İndirme, yükleme ve yedekleme aracısını Kaydet
Yedekleme kasası oluşturun ve kasa kimlik bilgilerini indirin sonra her Windows makinelerinizi bir aracı yüklü olmalıdır.

### <a name="to-download-install-and-register-the-agent"></a>İndirme, yükleme ve aracı kaydetmek için
1. Tıklatın **kurtarma Hizmetleri**ve ardından bir sunucuyla kaydetmek istediğiniz yedekleme kasasının seçin.
2. Hızlı Başlangıç sayfasında, aracıyı tıklatın **aracısı için Windows Server veya System Center Data Protection Manager veya Windows İstemcisi**. Daha sonra **Kaydet**'e tıklayın.

    ![Aracı Kaydet](./media/backup-configure-vault-classic/agent.png)
3. MARSagentinstaller.exe dosya indirdiği sonra tıklayın **çalıştırmak** (veya çift **MARSAgentInstaller.exe** kayıtlı konumdan).
4. Yükleme klasörü ve aracı için gerekli olan Önbellek klasörü seçin ve ardından **sonraki**. Belirttiğiniz önbellek konumu yedekleme verilerini yüzde 5'en az boş disk alanı olması gerekir.
5. Varsayılan proxy ayarları aracılığıyla Internet'e bağlanmak devam edebilirsiniz.             Proxy Yapılandırması sayfasında Internet'e bağlanmak için bir proxy sunucu kullanıyorsanız seçin **özel proxy ayarlarını kullan** onay kutusunu işaretleyin ve ardından proxy sunucusu ayrıntılarını girin. Doğrulanmış bir proxy kullanıyorsanız, kullanıcı adı ve parola ayrıntılarını girin ve ardından **sonraki**.
6. Tıklatın **yükleme** aracı yüklemesine başlamaya. (Zaten yüklüyse) .NET Framework 4.5 ve Windows PowerShell yedekleme aracısını yükler yüklemeyi tamamlamak için.
7. Aracı yüklendikten sonra tıklayın **devam etmek için kayıt** iş akışı ile devam etmek için.
8. Kasa kimlik sayfasında göz atın ve daha önce indirdiğiniz kasa kimlik bilgilerini seçin.

    Kasa kimlik bilgilerini portaldan İndirildikten sonra yalnızca 48 saat için geçerlidir. Bir hatayla karşılaşırsanız (örneğin, "sağlanan dosyasının süresi dolmuş kasa kimlik bilgileri"), bu sayfayı portalında oturum açın ve kasa kimlik bilgileri dosyasını yeniden indirin.

    Kasa kimlik bilgilerini Kurulum uygulama tarafından erişilebilen bir konumda kullanılabilir olduğundan emin olun. Erişim ile ilgili hatalarla karşılaşırsanız, kasa kimlik bilgilerini aynı makine üzerinde geçici bir konuma kopyalayın ve işlemi yeniden deneyin.

    "Geçersiz kasa kimlik bilgileri sağlandı" gibi bir kasa kimlik bilgileri hata karşılaşırsanız, dosya zarar görmüş veya mu sahip en yeni kimlik ilişkilendirilmemiş kurtarma hizmeti ile. Yeni bir kasa kimlik bilgilerini portaldan indirme işlemi yeniden deneyin. Bir kullanıcı, bu hata ayrıca oluşabilir **indirme kasa kimlik bilgileri** seçeneğinde birkaç kez art arda. Bu durumda, yalnızca son kasa kimlik bilgilerini geçerli değil.
9. Şifreleme Ayarları sayfasında, bir parola oluşturmak veya bir parola girin (en az 16 karakter ile). Parolayı güvenli bir konumda kaydetmeyi unutmayın.
10. **Son**'a tıklayın. Sunucu Kaydetme Sihirbazı'nı server yedekleme ile kaydeder.

    > [!WARNING]
    > Kaybeder veya parolayı unutursanız Microsoft Yedekleme verilerini kurtarmanıza yardımcı olamaz. Şifreleme parolası sahibi ve Microsoft kullandığınız parolayı görünürlüğe sahip değil. Bir kurtarma işlemi sırasında gerekli olacağı için dosyayı güvenli bir konuma kaydedin.
    >
    >

11. Şifreleme anahtarı ayarlandıktan sonra bırakın **Microsoft Azure kurtarma Hizmetleri Aracısı** seçili kutuyu işaretleyin ve ardından **Kapat**.

## <a name="complete-the-initial-backup"></a>İlk yedeklemeyi tamamlamak
İlk yedekleme iki anahtar görev içerir:

* Yedekleme zamanlaması oluşturma
* Dosya ve klasörleri ilk kez yedekleme

Yedekleme İlkesi ilk Yedekleme tamamlandıktan sonra verileri kurtarmanız gerekiyorsa kullanabileceğiniz yedekleme noktaları oluşturur. Yedekleme İlkesi bu tanımladığınız bir zamanlamaya göre yapar.

### <a name="to-schedule-the-backup"></a>Yedeklemeyi zamanlamak için
1. Microsoft Azure yedekleme Aracısı'nı açın. (Bırakılırsa, otomatik olarak açılır **Microsoft Azure kurtarma Hizmetleri Aracısı** sunucuyu Kaydetme Sihirbazı kapatıldığında onay kutusunun.) Bunu, makinenizde **Microsoft Azure Backup** aramasını yaparak bulabilirsiniz.

    ![Azure Backup aracısını başlatma](./media/backup-configure-vault-classic/snap-in-search.png)
2. Yedekleme aracısını tıklatın **yedekleme zamanlaması**.

    ![Windows Server yedeklemesini zamanlama](./media/backup-configure-vault-classic/schedule-backup-close.png)
3. Yedeklemeyi Zamanlama Sihirbazı'nın Başlarken sayfasında **İleri**'ye tıklayın.
4. Yedeklenecek Öğeleri Seçin sayfasında **Öğe Ekle**'ye tıklayın.
5. Yedeklemek istediğiniz dosya ve klasörleri seçin ve ardından **Tamam**'a tıklayın.
6. **İleri**’ye tıklayın.
7. **Yedekleme Zamanlamasını Belirtin** sayfasında, **yedekleme zamanlamasını** belirtin ve **İleri**'ye tıklayın.

    Günlük (en fazla günde üç kez olmak üzere) veya haftalık yedeklemeler zamanlayabilirsiniz.

    ![Windows Server Yedekleme öğeleri](./media/backup-configure-vault-classic/specify-backup-schedule-close.png)

   > [!NOTE]
   > Yedekleme zamanlamasını belirtme konusunda daha fazla bilgi için [Bant altyapınızın yerini alması için Azure Windows Server Backup'ı kullanma](backup-azure-backup-cloud-as-tape.md) makalesine bakın.
   >
   >

8. **Bekletme İlkesi Seçin** sayfasında, yedekleme kopyası için **Bekletme İlkesi**'ni seçin.

    Bekletme ilkesi, yedeklemenin depolanacağı süreyi belirtir. Tüm yedekleme noktaları için bir "sabit ilke" belirtmek yerine, yedeklemenin gerçekleşme zamanını temel alan farklı bekletme ilkeleri belirtebilirsiniz. Günlük, haftalık, aylık ve yıllık bekletme ilkelerini gereksinimlerinizi karşılayacak şekilde değiştirebilirsiniz.
9. İlk Yedekleme Türünü Seçin sayfasında ilk yedekleme türünü seçin. **Ağ üzerinden otomatik olarak** seçeneğini işaretli bırakın ve ardından **İleri**'ye tıklayın.

    Otomatik olarak ağ üzerinden veya çevrimdışı yedekleme yapabilirsiniz. Bu makalenin sonraki bölümlerinde otomatik olarak yedekleme işlemi açıklanmaktadır. Çevrimdışı yedekleme işlemini tercih ediyorsanız ek bilgi için [Azure Backup'ta çevrimdışı yedekleme iş akışı](backup-azure-backup-import-export.md) makalesini gözden geçirin.
10. Onay sayfasında bilgileri gözden geçirin ve ardından **Son**'a tıklayın.
11. Sihirbaz yedekleme zamanlamasını oluşturduktan sonra **Kapat**'a tıklayın.

### <a name="enable-network-throttling-optional"></a>Ağ (isteğe bağlı) azaltmayı etkinleştir
Yedekleme aracısı, ağ azaltma sağlar. Veri aktarımı sırasında ağ bant genişliğinin nasıl kullanıldığını denetimleri azaltma. Bu denetim sırasında verilerin yedeklemeniz gerekiyorsa, çalışma saatleri ancak Yedekleme işleminin diğer Internet trafiğine engel olmasını istiyor musunuz yararlı olabilir. Azaltma yedeklemek ve geri yükleme etkinlikleri için geçerlidir.

**Ağ azaltma etkinleştirmek için**

1. Yedekleme aracısını tıklatın **özelliklerini değiştirme**.

    ![Özelliklerini değiştir](./media/backup-configure-vault-classic/change-properties.png)
2. Üzerinde **azaltma** sekmesine **yedekleme işlemleri için Internet bant genişliği kullanımı daraltmayı etkinleştir** onay kutusu.

    ![Ağ azaltma](./media/backup-configure-vault-classic/throttling-dialog.png)
3. Azaltma etkinleştirdikten sonra sırasında yedek veri aktarımı için izin verilen bant genişliğini belirtin **çalışma saatleri** ve **çalışılmayan saatler**.

    Bant genişliği değerler 512 kilobit / saniye (Kbps) başlar ve 1,023 megabayt (MBps) saniyede gidebilirsiniz. Ayrıca başlangıç belirleyin ve için son **çalışma saatleri**, ve haftanın hangi günleri dikkate alınan iş günlerini. Belirtilen iş saatleri kabul dışında saat saat İş dışı.
4. **Tamam** düğmesine tıklayın.

### <a name="to-back-up-now"></a>Şimdi yedeklemek için
1. Yedekleme aracısını tıklatın **Şimdi Yedekle** ağ üzerinden ilk doldurma işlemini tamamlamak için.

    ![Windows Server şimdi yedekle](./media/backup-configure-vault-classic/backup-now.png)
2. Onay sayfasında, Şimdi Yedekle Sihirbazı'nın makineyi yedeklemek için kullanacağı ayarları gözden geçirin. Ardından **Yedekle**'ye tıklayın.
3. Sihirbazı kapatmak için **Kapat**'a tıklayın. Bunu yedekleme işlemi tamamlanmadan önce yaparsanız sihirbaz arka planda çalışmaya devam eder.

İlk yedekleme tamamlandıktan sonra, Yedekleme konsolunda **İş tamamlandı** durumu görünür.

![IR tamamlandı](./media/backup-configure-vault-classic/ircomplete.png)

## <a name="next-steps"></a>Sonraki adımlar
* Kaydolun bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/).

Sanal makineler veya diğer iş yüklerini yedekleme hakkında ek bilgi için bkz:

* [Iaas Vm'lerini yedekleme](backup-azure-vms-prepare.md)
* [Microsoft Azure yedekleme sunucusu ile azure'a iş yüklerini yedekleme](backup-azure-microsoft-azure-backup.md)
* [Azure ile DPM iş yüklerini dön](backup-azure-dpm-introduction.md)
