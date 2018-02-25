---
title: "Raporlar için Azure yedeklemeyi yapılandırma"
description: "Bu makalede, Azure kurtarma Hizmetleri kasası kullanarak yedekleme için Power BI raporları yapılandırma hakkında alınmaktadır."
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 86e465f1-8996-4a40-b582-ccf75c58ab87
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/10/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1c6cc4ba95f440f09f11a93927fd67873f8813e8
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="configure-azure-backup-reports"></a>Azure Backup raporlarını yapılandırma
Bu makalede Azure kurtarma Hizmetleri kasası kullanarak yedekleme için raporlar yapılandırmak ve Power BI'ı kullanarak bu raporlara erişmek için ilgili adımlar açıklanmaktadır. Bu adımları gerçekleştirdikten sonra Power BI için ın tüm raporları görüntülemek için özelleştirme ve raporları oluşturma doğrudan gidebilirsiniz. 

## <a name="supported-scenarios"></a>Desteklenen senaryolar
1. Azure yedekleme raporları, Azure sanal makine yedekleme ve Azure kurtarma Hizmetleri aracısını kullanarak buluta dosya/klasör yedekleme için desteklenmiyor.
2. Azure SQL, DPM ve Azure yedekleme sunucusu için raporlar şu anda desteklenmiyor.
3. Aynı depolama hesabındaki her kasa için yapılandırılmışsa, kasa ve abonelikler arasında raporları görüntüleyebilir. Seçilen depolama hesabı, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
4. Rapor için zamanlanan yenileme sıklığını Power bı'da 24 saattir. Power BI, raporları işlemeye yönelik büyük/küçük harfe en son verileri müşteri depolama hesabında kullanılan raporları geçici yenilenmesini de gerçekleştirebilirsiniz. 
5. Azure yedekleme raporları Ulusal bulutlara şu anda desteklenmemektedir.

## <a name="prerequisites"></a>Önkoşullar
1. Oluşturma bir [Azure depolama hesabı](../storage/common/storage-create-storage-account.md#create-a-storage-account) için raporları yapılandırmak için. Bu depolama hesabını raporları ilgili verileri depolamak için kullanılır.
2. [Power BI hesabınızı oluşturmak](https://powerbi.microsoft.com/landing/signin/) görüntülemek için özelleştirme ve Power BI portalını kullanarak kendi raporlarınızı oluşturun.
3. Kayıt kaynak sağlayıcısı **Microsoft.ınsights** zaten kayıtlı değil, depolama hesabı aboneliği ve ayrıca etkinleştirmek için kurtarma Hizmetleri kasası abonelik ile veri depolama alanına akışını raporlama hesabı. Aynı işlemi gerçekleştirmek için Azure portal gidin > abonelik > kaynak sağlayıcıları ve kaydetmek bu sağlayıcıyı denetleyin. 

## <a name="configure-storage-account-for-reports"></a>Raporlar için depolama hesabı yapılandırma
Azure portalını kullanarak kurtarma Hizmetleri kasası için depolama hesabı yapılandırmak için aşağıdaki adımları kullanın. Bu tek seferlik yapılandırma ve depolama hesabı yapılandırıldıktan sonra doğrudan içerik paketi görüntüleyebilir ve raporları yararlanmak için Power BI gidebilirsiniz.
1. Açık kurtarma Hizmetleri kasası zaten varsa, sonraki adıma geçin. Açık kurtarma Hizmetleri kasanız yoksa ancak Azure portalında, tıklatın **tüm hizmetleri**.

   * Kaynak listesinde **Kurtarma Hizmetleri** yazın.
   * Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Kurtarma Hizmetleri kasaları** seçeneğini gördüğünüzde buna tıklayın.

      ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

     Kurtarma Hizmetleri kasalarının listesi görünür. Kurtarma Hizmetleri kasalarının listesinden bir kasa seçin.

     Seçilen kasa panosu açılır.
2. Kasa altında görüntülenen listeden öğelerinin tıklatın **yedekleme raporları** bölümünde raporlar için depolama hesabı yapılandırmak için izleme ve raporlar.

      ![Select yedekleme raporları menü öğesi 2. adım](./media/backup-azure-configure-reports/backup-reports-settings.PNG)
3. Yedekleme raporları dikey penceresinde **tanılama ayarları** bağlantı. Bu tanılama ayarları verileri müşteri depolama hesabına gönderilmesi için kullanılan kullanıcı arabirimini açar.

      ![Adım 3'in tanılama etkinleştir](./media/backup-azure-configure-reports/backup-azure-configure-reports.png)
4. Bağlantıyı **tanılamayı açın**. Bu depolama hesabı yapılandırmak için kullanıcı arabirimini açar. 

      ![Tanılama adım 4 Aç](./media/backup-azure-configure-reports/enable-diagnostics.png)
5. Ayar adı alanı girin **adı** ve seçin **bir depolama hesabı arşive** veri içinde depolama hesabına akan başlayabileceğini raporlama böylece onay kutusunu işaretleyin.

      ![Adım 5'in tanılama etkinleştir](./media/backup-azure-configure-reports/select-setting-name.png)
6. Depolama hesabı Seçici ve raporlama veri ve tıklatın depolamak için listeden ilgili abonelik ve depolama hesabını seçin **Tamam**.

      ![Depolama hesabı adım 6 seçin](./media/backup-azure-configure-reports/select-subscription-sa.png)
7. Seçin **AzureBackupReport** günlük bölümü altında kutuyu işaretleyin ve raporlama verilerini bu select saklama süresi için kaydırıcıyı taşıyın. Raporlama verilerini depolama hesabındaki bu kaydırıcı kullanılarak seçilen boyunca tutulur.

      ![Depolama hesabı adım 7 kaydetme](./media/backup-azure-configure-reports/save-diagnostic-settings.png)
8. Tüm değişiklikleri gözden geçirin ve tıklatın **kaydetmek** Yukarıdaki şekilde gösterildiği gibi en üstte, düğme. Bu eylem, yaptığınız tüm değişiklikler kaydedilir ve raporlama verilerini depolamak için depolama hesabı artık yapılandırılmıştır sağlar.

9. Tanılama ayarları tablosu artık yeni ayar kasası için etkin gösterilmesi gerekir. Bunu gösterilmez, güncelleştirilmiş ayarı görmek için tabloyu yenileyin.

      ![Tanılama ayarını adım 9 görüntüleme](./media/backup-azure-configure-reports/diagnostic-setting-row.png)

> [!NOTE]
> Depolama hesabı kaydederek raporları yapılandırdıktan sonra aşağıdakileri yapmalısınız **24 saat bekleyin** tamamlamak ilk veri gönderimi için. Bundan sonra yalnızca Power BI içerik paketi Azure Backup almanız gerekir. Başvuru [SSS bölümüne](#frequently-asked-questions) daha ayrıntılı bilgi için. 
>
>

## <a name="view-reports-in-power-bi"></a>Power BI raporları görüntüleme 
Depolama hesabı için yapılandırma kurtarma Hizmetleri kasası kullanarak raporları sonraki veri raporlama için yaklaşık 24 saat içinde akan başlatmak için alır. Depolama hesabı kurma 24 saat sonra Power BI raporları görüntülemek için aşağıdaki adımları kullanın:
1. [Oturum](https://powerbi.microsoft.com/landing/signin/) Power BI için.
2. Tıklatın **Veri Al** tıklatıp **almak** altında **Hizmetleri** içerik paketi Kitaplığı'nda. Belirtilen adımları kullanın [içerik paketi erişmek için Power BI belgelerine](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-packs-services/).

     ![İçerik paketini İçeri Aktar](./media/backup-azure-configure-reports/content-pack-import.png)
3. Tür **Azure Backup** arama çubuğunda ve tıklatın **Şimdi Al**.

      ![İçerik Paketi Al](./media/backup-azure-configure-reports/content-pack-get.png)
4. Yukarıdaki 5. adımda yapılandırılmış depolama hesabı adı girin ve tıklayın **sonraki** düğmesi.

    ![Depolama hesabı adını girin](./media/backup-azure-configure-reports/content-pack-storage-account-name.png)    
5. Bu depolama hesabı için depolama hesabı anahtarı girin. Yapabilecekleriniz [görüntülemek ve depolama erişim tuşlarını kopyalayın](../storage/common/storage-create-storage-account.md#manage-your-storage-account) depolama hesabınız Azure portalında giderek. 

     ![Depolama hesabı girin](./media/backup-azure-configure-reports/content-pack-storage-account-key.png) <br/>
     
6. Tıklatın **oturum** düğmesi. Oturum açma başarılı olduktan sonra size **veri alma** bildirim.

    ![İçerik Paketi içe aktarma](./media/backup-azure-configure-reports/content-pack-importing-data.png) <br/>
    
    Bir süre sonra size **başarı** alma işlemi tamamlandıktan sonra bildirim. Çok miktarda veri depolama hesabındaki ise içerik paketini içeri aktarmak için biraz daha uzun sürebilir.
    
    ![İçeri aktarma başarılı İçerik Paketi](./media/backup-azure-configure-reports/content-pack-import-success.png) <br/>
    
7. Verileri başarıyla içeri aktarıldığında **Azure Backup** içerik paketi, görünür **uygulamaları** Gezinti bölmesinde. Liste Azure Backup Pano, raporlar ve veri kümesi artık yeni içe aktarılan raporlar belirten için sarı bir yıldız gösterir. 

     ![Azure yedekleme İçerik Paketi](./media/backup-azure-configure-reports/content-pack-azure-backup.png) <br/>
     
8. Tıklatın **Azure Backup** panolar altında sabitlenmiş anahtar rapor kümesini gösterir.

      ![Azure yedekleme Panosu](./media/backup-azure-configure-reports/azure-backup-dashboard.png) <br/>
9. Raporları tamamını görüntülemek için herhangi bir rapordaki panosunda tıklayın.

      ![Azure yedekleme işi durumu](./media/backup-azure-configure-reports/azure-backup-job-health.png) <br/>
10. Her bir sekmede, bu alanına raporları görüntülemek için raporları'ı tıklatın.

      ![Azure yedekleme raporları sekmeleri](./media/backup-azure-configure-reports/reports-tab-view.png)


## <a name="frequently-asked-questions"></a>Sık sorulan sorular
1. **Nasıl raporlama verileri, depolama hesabına akan başlayıp başlamadığını denetleyin?**
    
    Yapılandırılmış depolama hesabına gidin ve kapsayıcılarını seçin. Kapsayıcı Öngörüler günlükleri azurebackupreport için bir giriş varsa, veri raporlama akan başlatıldığını gösterir.

2. **Veri gönderimi depolama hesabı ve Power BI içerik paketi Azure Backup sıklığını nedir?**

   0. gün kullanıcılar için bu depolama hesabı veri göndermek için yaklaşık 24 saat sürer. Bu ilk anında compelete olduğunda, veriler aşağıdaki şekilde gösterilen aşağıdaki sıklık yenilenir. 
      * İlgili verileri **işleri, uyarılar, yedekleme öğeleri, kasa, korumalı sunucular ve ilkeleri** müşteri depolama hesabı olarak ve ne zaman oturum için gönderilir.
      * İlgili verileri **depolama** müşteri depolama hesabı için 24 saatte gönderilir.
   
    ![Azure yedekleme raporları veri gönderme sıklığını](./media/backup-azure-configure-reports/reports-data-refresh-cycle.png)

  Power BI bir [zamanlanan yenileme günde bir kez](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/#what-can-be-refreshed). Power BI'da verileri el ile yenileme için içerik paketi gerçekleştirebilirsiniz.

3. **Raporların ne kadar süreyle koruyabilir miyim?** 

   Depolama hesabı yapılandırırken, raporlama veri saklama dönemi (yukarıdaki rapor bölümü için adım 6 yapılandırma depolama hesabındaki kullanarak) depolama hesabındaki seçebilirsiniz. Yanı sıra yapabilecekleriniz [Çözümle raporlarda excel](https://powerbi.microsoft.com/documentation/powerbi-service-analyze-in-excel/) ve bunları gereksinimlerinize göre daha uzun bir bekletme dönemi için kaydedin. 

4. **Tüm verilerimi raporlarında depolama hesabı yapılandırdıktan sonra görüyor?**

   Sonra oluşturulan tüm verileri **"depolama hesabı yapılandırma"** storage hesabına gönderilir ve raporlarda kullanılabilir olacaktır. Ancak, **içinde devam eden işler değil gönderilen** raporlama için. İş tamamlanana veya başarısız sonra raporları gönderilir.

5. **Başka bir depolama hesabı kullanmak üzere yapılandırma, raporları görüntülemek için depolama hesabı ı zaten yapılandırdıysanız, değiştirebilir miyim?** 

   Evet, farklı bir depolama hesabı için işaret edecek şekilde yapılandırmasını değiştirebilirsiniz. Azure Backup içerik paketine bağlanırken yeni yapılandırılan depolama hesabı kullanmanız gerekir. Ayrıca, farklı bir depolama hesabı yapılandırıldıktan sonra yeni verileri bu depolama hesabında akış. Ancak (önce yapılandırmasını değiştirme) daha eski verileri eski depolama hesabında kalması.

6. **Raporları ve abonelikleri kasalarını arasında görüntüleyebilir miyim?** 

   Evet, çapraz kasası raporları görüntülemek için çeşitli kasaları aynı depolama hesabı yapılandırabilirsiniz. Ayrıca, abonelikler arasında kasaları için aynı depolama hesabı yapılandırabilirsiniz. Ardından bu depolama hesabı için Azure Backup Power BI içerik paketi bağlanırken raporları görüntülemek için kullanabilirsiniz. Ancak, seçilen depolama hesabı, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
   
## <a name="troubleshooting-errors"></a>Sorun giderme
| Hata ayrıntıları | Çözüm |
| --- | --- |
| Depolama hesabı için yedekleme raporları, ayarlandıktan sonra **depolama hesabı** göstermeye devam **yapılandırılmamış**. | Depolama hesabı başarıyla yapılandırdıysanız, raporlama verilerinizi içinde rağmen bu sorunu akar. Bu sorunu çözmek için Azure portal gidin > tüm hizmetleri > tanılama ayarlarını > RS kasası > ayarını düzenle. Daha önce yapılandırılan bir ayarı silin ve yeni bir ayar aynı dikey penceresinden oluşturun. Bu zaman alan kümesi **adı** için **hizmet**. Bu, yapılandırılan depolama hesabı göstermesi gerekir. |
|Azure Backup içeri aktardıktan sonra Power BI, hata paketinde içerik **404 kapsayıcısı bulunamadı** gelir. | Bu belgedeki olarak önerilen, bunları Power BI'da doğru şekilde görmek için kurtarma Hizmetleri kasasına raporları yapılandırdıktan sonra 24 saat beklemeniz gerekir. 24 saat önce raporları erişmeye çalışırsa, tam veri henüz geçerli raporları göstermek için mevcut olmadığından bu hatayı alırsınız. |

## <a name="next-steps"></a>Sonraki adımlar
Depolama hesabı ve içeri aktarılan Azure Backup içerik paketi yapılandırdığınıza göre sonraki adım bu raporları özelleştirme ve raporlar oluşturmak için raporlama veri modeli kullanmaktır. Daha fazla ayrıntı için aşağıdaki makalelere bakın.

* [Azure Backup veri modeli raporlama kullanma](backup-azure-reports-data-model.md)
* [Power BI raporları filtreleme](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
* [Power BI raporları oluşturma](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)

