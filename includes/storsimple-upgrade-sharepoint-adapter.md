<!--author=SharS last changed: 9/17/15-->

### <a name="upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-storsomple-adapter-for-sharepoint"></a>SharePoint 2010 SharePoint 2013'e yükseltin ve sonra SharePoint için StorSomple bağdaştırıcısı yükleyin
> [!IMPORTANT]
> Yükseltme tamamlandıktan ve KKY özelliği yeniden etkinleştirilene kadar KKY dış depolama birimine önceden taşındı herhangi bir dosya kullanılamaz. Kullanıcı etkilerine sınırlamak için herhangi bir yükseltme veya planlı bakım penceresi sırasında yeniden gerçekleştirin.
> 
> 

#### <a name="to-upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-adapter"></a>SharePoint 2010 SharePoint 2013'e yükselttikten sonra bağdaştırıcısı ve yüklemek için
1. SharePoint 2010 grupta externalized BLOB'ları ve KKY etkin olduğu içerik veritabanları için BLOB Depolama yolu unutmayın. 
2. Yükleyin ve yeni SharePoint 2013 grubuna yapılandırın. 
3. Veritabanları, uygulamaları ve site koleksiyonları SharePoint 2010 grubundan yeni SharePoint 2013 grubuna taşıyın. Yönergeler için Git [SharePoint 2013 için yükseltme işlemine genel bakış](https://technet.microsoft.com/library/cc262483.aspx).
4. Yeni gruptaki SharePoint için StorSimple bağdaştırıcısı yükleyin. Git [SharePoint için StorSimple bağdaştırıcısı](#install-the-storsimple-adapter-for-sharepoint) yordamlar.
5. 1. adımda not ettiğiniz bilgileri kullanarak, aynı içerik veritabanları kümesi için KKY etkinleştirmek ve SharePoint 2010 yüklemesinde kullanılan BLOB deposu yolunu girin. Git [yapılandırma KKY](#configure-rbs) yordamlar. Bu adımı tamamladıktan sonra daha önce externalized dosyaları yeni grubundan erişilebilir olması gerekir. 

### <a name="upgrade-the-storsimple-adapter-for-sharepoint"></a>SharePoint için StorSimple bağdaştırıcısı yükseltme
> [!IMPORTANT]
> Bu yükseltme şu nedenlerden dolayı bir planlı bakım penceresi sırasında gerçekleşecek şekilde zamanlamanız gerekir:
> 
> * Daha önce externalized içerik bağdaştırıcı yeniden kadar kullanılamaz.
> * SharePoint için StorSimple bağdaştırıcısı önceki sürümünü kaldırdıktan sonra ancak yeni sürümü yüklemeden önce siteye yüklenen herhangi bir içerik içerik veritabanında depolanır. Yeni bağdaştırıcısı yükledikten sonra bu içeriği için StorSimple cihazı taşımanız gerekir. Microsoft kullanabilirsiniz` RBS Migrate()` PowerShell cmdlet'ini içeriği geçirmeyi SharePoint ile dahil. Daha fazla bilgi için bkz: [içine veya dışına KKY İçerik Geçişi](https://technet.microsoft.com/library/ff628255.aspx). 
> 
> 

#### <a name="to-upgrade-the-storsimple-adapter-for-sharepoint"></a>SharePoint için StorSimple bağdaştırıcısı yükseltmek için
1. SharePoint için StorSimple Bağdaştırıcısı'nın önceki sürümünü kaldırın.
   
   > [!NOTE]
   > Bu otomatik olarak KKY içerik veritabanlarına devre dışı bırakır. Ancak, mevcut BLOB'ları StorSimple aygıtta kalır. KKY devre dışı bırakılır ve içerik veritabanları BLOB taşınmamış, bu BLOB'lar için tüm istekler başarısız olur. 
   > 
   > 
2. SharePoint için yeni StorSimple bağdaştırıcısı yükleyin. Yeni bağdaştırıcısı önceden etkinleştirilmiş veya devre dışı KKY için içerik veritabanları otomatik olarak algılar ve önceki ayarları kullanır.

