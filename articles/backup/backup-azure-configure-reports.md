---
title: İçin Azure Backup raporlarını yapılandırma
description: Power BI raporları, bir kurtarma Hizmetleri kasası kullanarak Azure Backup için yapılandırın.
services: backup
author: adigan
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: adigan
ms.openlocfilehash: e3004a44958d75d18d608a2fbed7ccc44a00dc93
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59278834"
---
# <a name="configure-azure-backup-reports"></a>Azure Backup raporlarını yapılandırma
Bu makalede, bir kurtarma Hizmetleri kasası kullanarak Azure Backup için raporları yapılandırmak için izlemeniz gereken adımlar açıklanır. Ayrıca, Power BI'ı kullanarak raporlara erişmek nasıl gösterir. Bu adımları tamamladıktan sonra doğrudan görüntülemek, özelleştirme ve raporlar oluşturmak için Power BI'da gidebilirsiniz.

> [!IMPORTANT]
> 1 Kasım 2018 tarihinden itibaren bazı müşteriler Power BI'da Azure Backup uygulaması verilerini yükleme sırasında sorun yaşayabilir ve "JSON girişinin sonunda fazladan karakterler bulduk. Özel durum, IDataReader arabirimi tarafından tetiklendi." iletisiyle karşılaşabilir.
Bunun nedeni, verilerin depolama hesabına yüklenmesi sırasında kullanılan biçimin değiştirilmesidir.
Lütfen bu sorunu önlemek için en son uygulama (sürüm 1.8) indirin.
>
>

## <a name="supported-scenarios"></a>Desteklenen senaryolar
- Azure Backup raporları için Azure sanal makine yedekleme ve dosya ve klasör desteklenir Azure kurtarma Hizmetleri Aracısı'nı kullanarak buluta yedekleme.
- Azure SQL veritabanı, Azure dosya paylaşımları, Data Protection Manager ve Azure Backup sunucusu için raporlar şu anda desteklenmiyor.
- Aynı depolama hesabını her kasa için yapılandırılmışsa, kasaları ve Aboneliklerde, raporları görüntüleyebilirsiniz. Seçilen depolama hesabına kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
- Power BI raporları için zamanlanmış yenileme sıklığını 24 saattir. Ayrıca, Power BI'da raporlar geçici bir yenileme gerçekleştirebilirsiniz. Bu durumda, en son verileri müşteri depolama hesabındaki raporları oluşturmak için kullanılır.

## <a name="prerequisites"></a>Önkoşullar
- Oluşturma bir [Azure depolama hesabı](../storage/common/storage-quickstart-create-account.md) için raporlar yapılandırılamadı. Bu depolama hesabı, raporları ile ilgili verileri depolamak için kullanılır.
- [Power BI hesabı oluşturma](https://powerbi.microsoft.com/landing/signin/) görüntülemek için özelleştirme ve kendi raporlarınızı Power BI portalı kullanarak oluşturun.
- Kaynak sağlayıcısını kaydetme **Microsoft.insights**, zaten kayıtlı değilse. Raporlama verilerini depolama hesabına akış, abonelik depolama hesabı ve kurtarma Hizmetleri kasası için kullanın. Bu adımı tamamlamak için Azure portalından select gidin **abonelik** > **kaynak sağlayıcıları**ve kaydetmek bu sağlayıcıyı denetleyin.

## <a name="configure-storage-account-for-reports"></a>Raporlar için depolama hesabı yapılandırın
Azure portalını kullanarak bir kurtarma Hizmetleri kasası için depolama hesabı yapılandırmak için aşağıdaki adımları izleyin. Bu tek seferlik bir yapılandırmadır. Depolama hesabını yapılandırdıktan sonra doğrudan içerik paketini görüntüleyebilir ve raporları kullanmak için Power BI'da gidebilirsiniz.

1. Açık kurtarma Hizmetleri kasası zaten varsa sonraki adıma gidin. Açık, Azure portalında kurtarma Hizmetleri kasası yoksa seçin **tüm hizmetleri**.

   * Kaynak listesinde girin **kurtarma Hizmetleri**.
   * Yazmaya başladığınızda liste, girişinize göre filtrelenir. Gördüğünüzde **kurtarma Hizmetleri kasaları**, onu seçin.
   * Kurtarma Hizmetleri kasalarının listesi görünür. Kurtarma Hizmetleri kasalarının listesinden bir kasa seçin.

     Seçilen kasa panosu açılır.
2. Kasa altında altında görünür öğeler listeden **izleme ve raporlar** bölümünden **Backup raporları** raporlar için depolama hesabı yapılandırmak için.

      ![Select yedekleme raporları menü öğesi 2. adım](./media/backup-azure-configure-reports/backup-reports-settings.PNG)
3. Üzerinde **Backup raporları** dikey penceresinde **tanılama ayarları** bağlantı. Bu bağlantıyı açar **tanılama ayarları** bir müşterinin depolama hesabına veri göndermek için kullanılan kullanıcı Arabirimi.

      ![Adım 3 tanılamayı etkinleştir](./media/backup-azure-configure-reports/backup-azure-configure-reports.png)
4. Seçin **tanılamayı Aç** bir depolama hesabı yapılandırmak için kullanılacak bir kullanıcı arabirimini açın.

      ![Tanılama adım 4 Aç](./media/backup-azure-configure-reports/enable-diagnostics.png)
5. İçinde **adı** kutusunda, bir ayar adı girin. Seçin **bir depolama hesabında arşivle** raporlama verilerini depolama hesabına akar başlayabilir, onay kutusunu işaretleyin.

      ![Tanılamayı etkinleştir 5. adım](./media/backup-azure-configure-reports/select-setting-name.png)
6. Seçin **depolama hesabı**, ilgili abonelik ve depolama hesabı raporlama verilerini depolamak için listeden seçip **Tamam**.

      ![Depolama hesabı adım 6'i seçin](./media/backup-azure-configure-reports/select-subscription-sa.png)
7. Altında **günlük** bölümünden **AzureBackupReport** onay kutusu. Bu raporlama verileri için bir saklama süresi seçmek için kaydırıcıyı taşıyın. Depolama hesabındaki verileri raporlama, kaydırıcıyı kullanarak seçilen süre boyunca tutulur.

      ![Depolama hesabı adım 7 kaydetme](./media/backup-azure-configure-reports/save-diagnostic-settings.png)
8. Tüm değişiklikleri gözden geçirin ve seçin **Kaydet**. Bu eylem, yaptığınız tüm değişiklikler kaydedilir ve depolama hesabı şimdi raporlama verilerini depolamak üzere yapılandırıldı sağlar.

9. **Tanılama ayarları** tablo kasa için etkinleştirilen yeni ayar artık gösterir. Görünmüyorsa, tablonun güncelleştirilmiş ayarları görmek için yenileyin.

      ![Tanılama ayarını adım 9 görüntüleme](./media/backup-azure-configure-reports/diagnostic-setting-row.png)

> [!NOTE]
> Depolama hesabı kaydederek raporları yapılandırdıktan sonra *için 24 saat bekleyin* tamamlamak ilk veri gönderimi için. Azure yedekleme uygulamasını Power BI yalnızca bundan sonra içeri aktarın. Daha fazla bilgi için [SSS bölümüne](#frequently-asked-questions).
>
>

## <a name="view-reports-in-power-bi"></a>Power BI raporlarını görüntüle
Bir kurtarma Hizmetleri Kasası'nı kullanarak bir depolama hesabı raporlar için yapılandırdıktan sonra yaklaşık 24 saat içeriye başlatmak raporlama verilerini alır. Bir depolama hesabı kurarak 24 saat sonra Power BI'da raporları görüntülemek için aşağıdaki adımları izleyin.
İsterseniz özelleştirme ve rapor paylaşma, bir çalışma alanı oluşturun ve aşağıdaki adımları uygulayın

1. [Oturum](https://powerbi.microsoft.com/landing/signin/) Power bı'a.
2. **Veri Al**’ı seçin. İçinde **kendi içeriğinizi oluşturmanın daha fazla yolu**seçin **hizmet içerik paketleri**. Bağlantısındaki [bir hizmete bağlanmak için Power BI belgeleri](https://powerbi.microsoft.com/documentation/powerbi-content-packs-services/).

3. İçinde **arama** çubuğunda girin **Azure Backup** seçip **şimdi edinin**.

      ![İçerik paketini edinin](./media/backup-azure-configure-reports/content-pack-get.png)
4. Önceki adımda 5 yapılandırılan depolama hesabı adını girin ve seçin **sonraki**.

    ![Depolama hesabı adını girin](./media/backup-azure-configure-reports/content-pack-storage-account-name.png)    
5. Kimlik doğrulama yöntemi "Anahtarını" kullanarak, bu depolama hesabı için depolama hesabı anahtarını girin. İçin [görüntüleme ve depolama erişim anahtarlarını kopyalama](../storage/common/storage-account-manage.md#access-keys), Azure portalında depolama hesabınıza gidin.

     ![Depolama hesabı girin](./media/backup-azure-configure-reports/content-pack-storage-account-key.png) <br/>

6. Seçin **oturum**. Oturum açma başarılı olduktan sonra bir içeri aktarma verileri bildirim görürsünüz.

    ![İçerik paketini içeri aktarma](./media/backup-azure-configure-reports/content-pack-importing-data.png) <br/>

    Gördüğünüz içeri aktarma tamamlandıktan sonra bir **başarı** bildirim. Depolama hesabındaki veri miktarı büyükse, içerik paketini içeri aktarmak için biraz daha uzun sürebilir.

    ![Başarı içerik paketini içeri aktarma](./media/backup-azure-configure-reports/content-pack-import-success.png) <br/>

7. Veriler başarıyla içeri aktarıldıktan sonra **Azure Backup** içerik paketi görünür **uygulamaları** Gezinti bölmesinde. Altında **panolar**, **raporları**, ve **veri kümeleri**, liste Azure Backup artık gösterir.

8. Altında **panolar**seçin **Azure Backup**, sabitlenmiş anahtar raporlar kümesi gösterir.

      ![Azure Backup Panosu](./media/backup-azure-configure-reports/azure-backup-dashboard.png) <br/>
9. Raporları kümesinin tamamını görüntülemek için Panoda herhangi bir rapor seçin.

      ![Azure yedekleme işi durumu](./media/backup-azure-configure-reports/azure-backup-job-health.png) <br/>
10. Her sekme, bu alanına raporları görüntülemek için raporları seçin.

      ![Azure Backup raporları sekmeler](./media/backup-azure-configure-reports/reports-tab-view.png)


## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="how-do-i-check-if-reporting-data-has-started-flowing-into-a-storage-account"></a>Raporlama verileri bir depolama hesabına akar başlatılmış olup olmadığını nasıl denetlerim?
Yapılandırdığınız depolama hesabına gidin ve kapsayıcılar'ı seçin. Kapsayıcı öngörüleri günlükleri azurebackupreport için bir girdi varsa, veri raporlama akan başlatıldığını gösterir.

### <a name="what-is-the-frequency-of-data-push-to-a-storage-account-and-the-azure-backup-content-pack-in-power-bi"></a>Bir depolama hesabı ve Azure Backup içerik Paketi'ne Power bı'daki veri gönderme sıklığı nedir?
  0. gün kullanıcılar için bunu bir depolama hesabına veri göndermeye ilişkin yaklaşık 24 saat sürer. Bu ilk gönderme tamamlandıktan sonra verileri aşağıdaki şekilde gösterilen sıklıkta yenilenir.

  * İlgili verileri **işleri**, **uyarılar**, **yedekleme öğeleri**, **kasaları**, **korumalı sunucuların**ve  **İlkeleri** gibi ve günlüğe bir müşterinin depolama hesabına gönderilir.

  * İlgili verileri **depolama** 24 saatte bir müşterinin depolama hesabına gönderilir.

       ![Azure Backup raporları veri gönderme sıklığı](./media/backup-azure-configure-reports/reports-data-refresh-cycle.png)

  * Power bı'da bir [zamanlanmış yenileme günde bir kez](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/#what-can-be-refreshed). Power BI'da İçerik Paketi için verileri el ile yenileme gerçekleştirebilirsiniz.

### <a name="how-long-can-i-retain-reports"></a>Raporlar ne kadar süreyle tutabilir miyim?
Bir depolama hesabı yapılandırmak, depolama hesabında bir rapor verileri saklama süresini seçebilirsiniz. İzleme adımı 6 [raporlar için depolama hesabı yapılandırma](backup-azure-configure-reports.md#configure-storage-account-for-reports) bölümü. Ayrıca [raporları Excel'de Çözümle](https://powerbi.microsoft.com/documentation/powerbi-service-analyze-in-excel/) ve bunları ihtiyaçlarınıza göre daha uzun bir bekletme dönemi için kaydedin.

### <a name="will-i-see-all-my-data-in-reports-after-i-configure-the-storage-account"></a>Raporlardaki tüm Verilerimin, depolama hesabı yapılandırabilirim sonra görür müyüm?
 Bir depolama hesabı yapılandırdıktan sonra oluşturulan tüm veriler depolama hesabına gönderilir ve raporlarında kullanılabilir. Raporlama için devam eden işleri gönderilmez. İşin tamamlanana veya başarısız olduktan sonra Bu raporlar için gönderilir.

### <a name="if-i-already-configured-the-storage-account-to-view-reports-can-i-change-the-configuration-to-use-another-storage-account"></a>Depolama hesabı, raporları görüntülemek için yapılandırılmış, başka bir depolama hesabı kullanmak üzere yapılandırma değiştirebilirim?
Evet, farklı bir depolama hesabına işaret edecek şekilde yapılandırmasını değiştirebilirsiniz. Azure Backup içerik Paketi'ne bağlanmak yeni yapılandırılan depolama hesabı kullanın. Ayrıca, farklı bir depolama hesabı yapılandırdıktan sonra yeni veriler bu depolama hesabına akar. (Yapılandırmayı değiştirmemeden önce) daha eski verilere hala eski depolama hesabında kalır.

### <a name="can-i-view-reports-across-vaults-and-subscriptions"></a>Raporları kasaları ve Aboneliklerde görüntüleyebilir miyim?
Evet, kasa arası raporları görüntülemek için çeşitli kasaları aynı depolama hesabı yapılandırabilirsiniz. Ayrıca, abonelikler arasında kasaları için aynı depolama hesabı yapılandırabilirsiniz. Power bı'da raporları görüntülemek için Azure Backup içerik Paketi'ne bağlanırken, bu depolama hesabı kullanabilirsiniz. Seçilen depolama hesabına kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.

## <a name="troubleshooting-errors"></a>Hatalarda sorun giderme
| Hata Ayrıntıları | Çözüm |
| --- | --- |
| Backup raporları için depolama hesabınızı ayarladığınızda **depolama hesabı** görüntülenmeye **yapılandırılmadı**. | Bir depolama hesabı başarıyla yapılandırıldı, raporlama veri, bu sorunu rağmen akar. Bu sorunu çözmek için Azure portal ve select Git **tüm hizmetleri** > **tanılama ayarları** > **kurtarma Hizmetleri kasası**  >  **Ayarını Düzenle**. Daha önce yapılandırılan bir ayarı silin ve yeni bir ayar aynı dikey pencerede oluşturun. İçinde bu sefer **adı** kutusunda **hizmet**. Yapılandırılmış depolama hesabı artık görünür. |
|Azure Backup içerik Paketi'ne Power bı'da bir "kapsayıcı 404 bulunamadı" hatası içeri aktardıktan sonra iletisi görüntülenir. | Daha önce belirtildiği gibi bunları doğru bir şekilde Power BI'da görmek için kurtarma Hizmetleri Kasası'nda raporları yapılandırdıktan sonra 24 saat beklemeniz gerekir. 24 saat önce raporları erişmeye çalışırsanız, tam veri henüz geçerli raporları göstermek için mevcut olmadığından bu hata iletisi görünür. |

## <a name="next-steps"></a>Sonraki adımlar
Depolama hesabı yapılandırın ve Azure Backup içerik paketini içeri aktarma sonra sonraki raporları özelleştirin ve raporlar oluşturmak için bir raporlama veri modelini kullanmak için adımlardır. Daha fazla bilgi için aşağıdaki makalelere bakın.

* [Veri modeli raporlama bir Azure Backup kullanın](backup-azure-reports-data-model.md)
* [Power BI raporları](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
* [Power BI'da raporlar oluşturma](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)
