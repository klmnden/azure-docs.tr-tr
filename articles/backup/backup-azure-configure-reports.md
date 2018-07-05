---
title: Raporlar için Azure yedeklemeyi yapılandırma
description: Power BI raporları için Azure kurtarma Hizmetleri kasası kullanarak yedekleme yapılandırın.
services: backup
author: adiganmsft
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: adigan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 81653f9125b9cc4411e5cfe358bd602f92c5bf89
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37448375"
---
# <a name="configure-azure-backup-reports"></a>Azure Backup raporlarını yapılandırma
Bu makalede, Azure kurtarma Hizmetleri kasası kullanarak yedekleme raporları yapılandırmak için ve Power BI'ı kullanarak bu raporlara erişmek için adımları hakkında konuşuyor. Bu adımları gerçekleştirdikten sonra tüm raporları görüntülemek için Power BI'da, özelleştirme ve raporlar oluşturmak doğrudan gidebilirsiniz. 

## <a name="supported-scenarios"></a>Desteklenen senaryolar
1. Azure Backup raporları, Azure sanal makine yedekleme ve Azure kurtarma Hizmetleri Aracısı'nı kullanarak bulut dosya/klasör yedekleme için desteklenir.
2. Azure SQL, DPM ve Azure Backup sunucusu için raporları, şu anda desteklenmemektedir.
3. Aynı depolama hesabını her kasa için yapılandırılmışsa, abonelikleri ve kasalar arasında raporları görüntüleyebilirsiniz. Seçili depolama hesabının, Kurtarma Hizmetleri kasasıyla aynı bölgede olmalıdır.
4. Power BI raporları için zamanlanmış yenileme sıklığını 24 saattir. Power BI, rapor işlemeye yönelik büyük/küçük harf en son verileri müşteri depolama hesabındaki kullanıldığı raporları bir geçici yenilenmesini de gerçekleştirebilirsiniz. 
5. Azure Backup raporları Ulusal bulutlarda şu anda desteklenmemektedir.

## <a name="prerequisites"></a>Önkoşullar
1. Oluşturma bir [Azure depolama hesabı](../storage/common/storage-create-storage-account.md#create-a-storage-account) için raporlar yapılandırılamadı. Bu depolama hesabı, raporları ilgili verileri depolamak için kullanılır.
2. [Power BI hesabı oluşturma](https://powerbi.microsoft.com/landing/signin/) görüntülemek için özelleştirme ve Power BI portalını kullanarak kendi raporlarınızı oluşturmak.
3. Kaynak sağlayıcısını kaydetme **Microsoft.insights** abonelik depolama hesabı ve aboneliği etkinleştirmek için kurtarma Hizmetleri kasası ile zaten kayıtlı değilse, veri akışı depolama raporlama hesabı. Aynı işlemi gerçekleştirmek için Azure portalına gidin > abonelik > kaynak sağlayıcıları ve kaydetmek bu sağlayıcıyı denetleyin. 

## <a name="configure-storage-account-for-reports"></a>Raporlar için depolama hesabı yapılandırın
Azure portalını kullanarak bir kurtarma Hizmetleri kasası için depolama hesabı yapılandırmak için aşağıdaki adımları kullanın. Bu tek seferlik bir yapılandırmadır ve depolama hesabı yapılandırıldıktan sonra içerik paketi görüntülemek ve raporlardan yararlanın doğrudan Power BI'a gidebilirsiniz.
1. Açık kurtarma Hizmetleri kasası zaten varsa sonraki adıma geçin. Açık bir kurtarma Hizmetleri kasanız yoksa ancak Azure portalında, tıklayın **tüm hizmetleri**.

   * Kaynak listesinde **Kurtarma Hizmetleri** yazın.
   * Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Kurtarma Hizmetleri kasaları** seçeneğini gördüğünüzde buna tıklayın.

      ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

     Kurtarma Hizmetleri kasalarının listesi görünür. Kurtarma Hizmetleri kasalarının listesinden bir kasa seçin.

     Seçilen kasa panosu açılır.
2. Kasa altında görüntülenen listeden öğeleri tıklayın **Backup raporları** raporlar için depolama hesabı yapılandırmak için izleme ve Raporlar bölümünde.

      ![Select yedekleme raporları menü öğesi 2. adım](./media/backup-azure-configure-reports/backup-reports-settings.PNG)
3. Yedekleme raporları dikey penceresinde tıklayın **tanılama ayarları** bağlantı. Bu tanılama ayarları müşteri depolama hesabına veri göndermek için kullanılan kullanıcı arabirimini açar.

      ![Adım 3 tanılamayı etkinleştir](./media/backup-azure-configure-reports/backup-azure-configure-reports.png)
4. Bağlantıyı tıklatın **tanılamayı Aç**. Bu depolama hesabı yapılandırmak için kullanıcı arabirimini açar. 

      ![Tanılama adım 4 Aç](./media/backup-azure-configure-reports/enable-diagnostics.png)
5. Ayar adı alanına **adı** seçip **bir depolama hesabında arşivle** raporlama verilerini depolama hesabına içeriye başlayabilir, onay kutusunu işaretleyin.

      ![Tanılamayı etkinleştir 5. adım](./media/backup-azure-configure-reports/select-setting-name.png)
6. Depolama hesabı Seçici'yi tıklatın ve raporlama verileri ve tıklatın depolamak için listeden ilgili abonelik ve depolama hesabını seçin **Tamam**.

      ![Depolama hesabı adım 6'i seçin](./media/backup-azure-configure-reports/select-subscription-sa.png)
7. Seçin **AzureBackupReport** günlüğü bölümü altındaki kutuyu ve raporlama verilerini bu seçim saklama süresi için kaydırıcıyı taşıyın. Depolama hesabındaki verileri raporlama kaydırıcıyı kullanarak seçili boyunca tutulur.

      ![Depolama hesabı adım 7 kaydetme](./media/backup-azure-configure-reports/save-diagnostic-settings.png)
8. Tüm değişiklikleri gözden geçirin ve tıklayın **Kaydet** düğmesini üstte, yukarıdaki şekilde gösterildiği gibi. Bu eylem, yaptığınız tüm değişiklikler kaydedilir ve depolama hesabı, artık raporlama verilerini depolamak için yapılandırılmıştır sağlar.

9. Tanılama ayarları tablosu için kasa etkin yeni ayar artık göstermelidir. Bunu görünmez, tablonun güncelleştirilmiş ayarları görmek için yenileyin.

      ![Tanılama ayarını adım 9 görüntüleme](./media/backup-azure-configure-reports/diagnostic-setting-row.png)

> [!NOTE]
> Depolama hesabı kaydederek raporları yapılandırdıktan sonra yapmanız gerekenler **için 24 saat bekleyin** tamamlamak ilk veri gönderimi için. Yalnızca bundan sonra Power bı'da Azure Backup içerik Paketi'ne almanız gerekir. Başvuru [SSS bölümüne](#frequently-asked-questions) daha ayrıntılı bilgi için. 
>
>

## <a name="view-reports-in-power-bi"></a>Power BI raporlarını görüntüle 
Kurtarma Hizmetleri Kasası'nı kullanarak depolama hesabı için yapılandırma raporları sonra akışa başlamak için veri raporlama için yaklaşık 24 saat sürer. Depolama hesabı kurarak 24 saat sonra Power BI'da raporları görüntülemek için aşağıdaki adımları kullanın:
1. [Oturum](https://powerbi.microsoft.com/landing/signin/) Power bı'a.
2. Tıklayın **Veri Al** tıklatıp **alma** altında **Hizmetleri** içerik paketi Kitaplığı'nda. Belirtilen adımları [içerik paketine erişmek için Power BI belgeleri](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-packs-services/).

     ![İçerik paketini içeri aktarma](./media/backup-azure-configure-reports/content-pack-import.png)
3. Tür **Azure Backup** Arama çubuğuna tıklayıp **şimdi edinin**.

      ![İçerik paketini edinin](./media/backup-azure-configure-reports/content-pack-get.png)
4. Yukarıdaki 5. adımda yapılandırılmış depolama hesabı adı girin ve tıklayın **sonraki** düğmesi.

    ![Depolama hesabı adını girin](./media/backup-azure-configure-reports/content-pack-storage-account-name.png)    
5. Bu depolama hesabı için depolama hesabı anahtarını girin. Yapabilecekleriniz [görüntüleme ve depolama erişim anahtarlarını kopyalama](../storage/common/storage-create-storage-account.md#manage-your-storage-account) Azure portalında depolama hesabınıza giderek. 

     ![Depolama hesabı girin](./media/backup-azure-configure-reports/content-pack-storage-account-key.png) <br/>
     
6. Tıklayın **oturum** düğmesi. Oturum açma başarılı olduktan sonra size **veri alma** bildirim.

    ![İçerik paketini içeri aktarma](./media/backup-azure-configure-reports/content-pack-importing-data.png) <br/>
    
    Bir süre sonra size **başarı** içeri aktarma tamamlandıktan sonra bildirim. Çok fazla veri depolama hesabındaki ise içerik paketini içeri aktarmak için biraz daha uzun sürebilir.
    
    ![Başarı içerik paketini içeri aktarma](./media/backup-azure-configure-reports/content-pack-import-success.png) <br/>
    
7. Verileri başarıyla içeri aktarıldıktan sonra **Azure Backup** içerik paketi görünür **uygulamaları** Gezinti bölmesinde. Liste artık yeni içe aktarılan raporlar gösteren sarı bir yıldız ile Azure Backup Pano, raporlar ve veri kümesi gösterir. 

     ![Azure Backup içerik Paketi'ne](./media/backup-azure-configure-reports/content-pack-azure-backup.png) <br/>
     
8. Tıklayın **Azure Backup** Pano anahtar sabitlenmiş rapor kümesini gösterir.

      ![Azure Backup Panosu](./media/backup-azure-configure-reports/azure-backup-dashboard.png) <br/>
9. Raporları kümesinin tamamını görüntülemek için panodaki herhangi bir rapor tıklayın.

      ![Azure yedekleme işi durumu](./media/backup-azure-configure-reports/azure-backup-job-health.png) <br/>
10. Her sekme raporlarında bu raporları görüntülemek için tıklayın.

      ![Azure Backup raporları sekmeler](./media/backup-azure-configure-reports/reports-tab-view.png)


## <a name="frequently-asked-questions"></a>Sık sorulan sorular
1. **Raporlama verilerini depolama hesabına içeriye başlayıp başlamadığını nasıl kontrol edebilirim?**
    
    Yapılandırılmış depolama hesabına gidin ve kapsayıcılar'ı seçin. Kapsayıcı öngörüleri günlükleri azurebackupreport için bir girdi varsa, veri raporlama akan başlatıldığını gösterir.

2. **Depolama hesabı ve Azure Backup içerik Paketi'ne Power bı'daki veri gönderme sıklığı nedir?**

   0. gün kullanıcılar için bu depolama hesabına veri göndermek için yaklaşık 24 saat sürecektir. Bu ilk gönderme tamamlandıktan sonra verileri aşağıdaki şekilde gösterilen şu sıklıkta yenilenir. 
      * İlgili verileri **işleri, uyarılar, yedekleme öğeleri, kasalar, korumalı sunucular ve ilkeleri** müşteri depolama hesabı olarak ve ne zaman günlüğe gönderilir.
      * İlgili verileri **depolama** müşteri depolama hesabı için 24 saatte bir gönderilir.
   
    ![Azure Backup raporları veri gönderme sıklığı](./media/backup-azure-configure-reports/reports-data-refresh-cycle.png)

  Power bı'da bir [zamanlanmış yenileme günde bir kez](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/#what-can-be-refreshed). Power BI'da İçerik Paketi için verileri el ile yenileme gerçekleştirebilirsiniz.

3. **Raporların ne kadar süreyle tutabilir miyim?** 

   Depolama hesabı yapılandırılırken (yukarıdaki Raporlar bölümünde için adım 6 yapılandırma depolama hesabındaki kullanarak) depolama hesabında raporlama veri saklama süresi seçebilirsiniz. Yanı sıra yapabilecekleriniz [analiz raporlarında excel](https://powerbi.microsoft.com/documentation/powerbi-service-analyze-in-excel/) ve bunları ihtiyaçlarınıza göre daha uzun bekletme süreleri için kaydedin. 

4. **Raporlardaki tüm verilerimi depolama hesabını yapılandırdıktan sonra görür müyüm?**

   Sonra oluşturulan tüm veriler **"depolama hesabı yapılandırma"** depolama hesabına gönderilir ve raporlarda kullanılabilir olacak. Ancak, **, devam eden işleri değil itilir** raporlama için. İşin tamamlanana veya başarısız sonra Bu raporlar için gönderilir.

5. **Başka bir depolama hesabı kullanacak şekilde yapılandırma, raporları görüntülemek için depolama hesabı ı zaten yapılandırdıysanız, değiştirebilir miyim?** 

   Evet, farklı bir depolama hesabına işaret edecek şekilde yapılandırmasını değiştirebilirsiniz. Azure Backup içerik Paketi'ne bağlanırken yeni yapılandırılan depolama hesabı kullanmanız gerekir. Ayrıca, farklı bir depolama hesabı yapılandırıldıktan sonra yeni verileri bu depolama hesabına aktarın. Ancak, eski verileri (önce yapılandırmasını değiştirme) hala eski depolama hesabında kalır.

6. **Rapor abonelikleri ve kasalar arasında görüntüleyebilir miyim?** 

   Evet, kasa arası raporları görüntülemek için çeşitli kasaları aynı depolama hesabı yapılandırabilirsiniz. Ayrıca, abonelikler arasında kasaları için aynı depolama hesabı yapılandırabilirsiniz. Ardından bu depolama hesabı Azure Backup içerik Paketi'ne Power bı'da bağlanırken raporları görüntülemek için kullanabilirsiniz. Bununla birlikte, seçilen depolama hesabına, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
   
## <a name="troubleshooting-errors"></a>Hatalarda sorun giderme
| Hata ayrıntıları | Çözüm |
| --- | --- |
| Backup raporları için depolama hesabı ayarlandıktan sonra **depolama hesabı** görüntülenmeye **yapılandırılmadı**. | Depolama hesabı başarıyla yapılandırıldı, raporlama veri içinde rağmen bu sorunu akar. Bu sorunu çözmek için Azure portalına gidin > tüm hizmetler > tanılama Ayarları > RS kasa > ayarını düzenle. Daha önce yapılandırılan bir ayarı silin ve aynı dikey penceresinden yeni bir ayar oluşturun. Bu zaman alan kümesi **adı** için **hizmet**. Bu, yapılandırılmış depolama hesabı göstermelidir. |
|Azure Backup içeri aktardıktan sonra içerik paketiyle Power bı'daki hata **kapsayıcı 404 Bulunamadı** gelir. | Bu belgedeki olarak önerilen, bunları doğru bir şekilde Power BI'da görmek için kurtarma Hizmetleri Kasası'nda raporları yapılandırdıktan sonra 24 saat beklemeniz gerekir. 24 saat önce raporları erişmeye çalışırsanız, tam veri henüz geçerli raporları göstermek için mevcut olmadığından bu hatayı alırsınız. |

## <a name="next-steps"></a>Sonraki adımlar
Depolama hesabı ve içeri aktarılan Azure Backup içerik Paketi'ne yapılandırdığınıza göre sonraki adımda bu raporları özelleştirin ve raporlar oluşturmak için raporlama veri modeli kullanmaktır. Daha fazla ayrıntı için aşağıdaki makalelere bakın.

* [Azure Backup'ın raporlama veri modelini kullanma](backup-azure-reports-data-model.md)
* [Power BI raporlarını filtreleme](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
* [Power BI'da raporlar oluşturma](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)

