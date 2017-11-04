<!--author=SharS last changed: 1/14/2016 -->

> [!NOTE]
> StorSimple bağdaştırıcısını SharePoint KKY yapılandırma için değişiklik yaparken, Domain Admins grubuna ait bir kullanıcı hesabıyla oturum. Ayrıca, merkezi yönetim olarak aynı ana bilgisayarda çalışan bir tarayıcıdan yapılandırma sayfası erişmeniz gerekir.
> 
> 

#### <a name="to-configure-rbs"></a>KKY yapılandırmak için
1. SharePoint Merkezi Yönetim sayfasını açın ve gidin **sistem ayarlarını**. 
2. İçinde **Azure StorSimple** 'yi tıklatın **StorSimple bağdaştırıcısı yapılandırma**.
   
    ![StorSimple bağdaştırıcısını yapılandırın](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS1-include.png) 
3. Üzerinde **StorSimple bağdaştırıcısını yapılandırın** sayfa:
   
   1. Olduğundan emin olun **düzenleme yolu etkinleştir** onay kutusu seçilidir.
   2. Metin kutusuna BLOB deposu Evrensel Adlandırma Kuralı (UNC) yolunu yazın.
      
      > [!NOTE]
      > BLOB Depolama Birimi StorSimple cihazında yapılandırılmış bir iSCSI birim üzerinde barındırılması gerekir.

   3. Tıklatın **etkinleştirmek** her bir uzak depolama için yapılandırmak istediğiniz içerik veritabanı aşağıdaki düğmesine.
      
      > [!NOTE]
      > BLOB deposu tüm web ön uç (WFE) sunucuları tarafından paylaşılan ve erişilebilir olmalıdır ve SharePoint sunucu grubu için yapılandırılan kullanıcı hesabı bu paylaşıma erişiminiz olmalıdır.
      
      ![KKY sağlayıcısını etkinleştirin](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS2-include.png)
      
      Etkinleştirmek veya KKY devre dışı bıraktığınızda, ayrıca aşağıdaki iletiyi görürsünüz.
      
      ![StorSimple bağdaştırıcısı etkinleştirme devre dışı bırakma yapılandırın](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_ConfigureStorSimpleAdapterEnableDisableMessage-include.png)

   4. Tıklatın **güncelleştirme** yapılandırmayı uygulamak için düğmesi. Tıkladığınızda **güncelleştirme** düğmesi tüm WFE sunucularında KKY yapılandırma durumu güncelleştirilir ve tüm Grup KKY etkin olacaktır. Aşağıdaki ileti görüntülenir.
      
      ![Bağdaştırıcı yapılandırma iletisi](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS3-include.png)
      
      > [!NOTE]
      > Bir çok sayıda (200'den büyük) veritabanları bir SharePoint çiftliği için KKY yapılandırıyorsanız, SharePoint Yönetim Merkezi web sayfası zaman aşımı olabilir. Bu gerçekleşirse, sayfayı yenileyin. Bu yapılandırma işlemi etkilemez.

4. Yapılandırmasını doğrulayın:
   
   1. SharePoint Yönetim Merkezi Web sitesinde oturum açın ve gidin **StorSimple bağdaştırıcısını yapılandırın** sayfası.
   2. Girdiğiniz ayarları eşleştiğinden emin olmak için yapılandırma ayrıntılarını kontrol edin. 
5. KKY düzgün çalıştığını doğrulayın:
   
   1. Belge SharePoint'te yükleyin. 
   2. Yapılandırdığınız UNC yoluna göz atın. KKY dizin yapısını oluşturulduğunu ve karşıya yüklenen nesne içeren emin olun.
6. (İsteğe bağlı) Microsoft RBS kullanabileceğiniz `Migrate()` SharePoint için StorSimple cihazı mevcut BLOB İçerik Geçişi ile birlikte gelen PowerShell cmdlet'i. Daha fazla bilgi için bkz: [içine veya SharePoint 2013'te KKY dışında İçerik Geçişi] [ 6] veya [içine veya dışına KKY (SharePoint Foundation 2010) İçerik Geçişi] [7].
7. (İsteğe bağlı) Test yüklemelerinde BLOB dışında içerik veritabanı gibi taşındı doğrulayabilirsiniz: 
   
   1. SQL Management Studio'yu başlatın.
   2. Listblobsındb_2010.SQL veya Listblobsındb_2013.SQL sorgu aşağıdaki gibi çalıştırın.
      
      ```
      **ListBlobsInDB_2013.sql**
      
        USE WSS_Content
        GO
      
        SELECT DocStreams.DocId,
      
               LeafName AS Name,
               Content,
               AllDocs.Size AS OrigSizeOfContent,
               LEN(CAST(Content AS VARBINARY(MAX))) AS SizeOfContentInDB,
               DocStreams.RbsId,
               TimeLastModified
      
        FROM DocStreams
      
             INNER JOIN AllDocs ON DocStreams.DocId = AllDocs.Id
        ORDER BY TimeLastModified DESC
        GO
      
      **ListBlobsInDB_2010.sql**
      
        USE WSS_Content
        GO
      
        SELECT AllDocStreams.Id,
      
               LeafName AS Name,
               Content,
               AllDocs.Size AS OrigSizeOfContent,
               LEN(CAST(Content AS VARBINARY(MAX))) AS SizeOfContentInDB,
               RbsId,
               TimeLastModified
        FROM AllDocStreams
      
             INNER JOIN AllDocs ON AllDocStreams.Id = AllDocs.Id
        ORDER BY TimeLastModified DESC
        GO
      ```
      
      KKY doğru şekilde yapılandırıldıysa, bir NULL değer karşıya ve başarıyla KKY ile externalized herhangi bir nesnenin SizeOfContentInDB Sütun görüntülenmelidir.
8. (İsteğe bağlı) KKY yapılandırmak ve StorSimple cihazı için tüm BLOB içeriğinin taşıma sonra içerik veritabanını cihaza taşıyabilirsiniz. İçerik veritabanını taşımak seçerseniz, birincil bir birim olarak aygıtta içerik veritabanı depolama yapılandırmanızı öneririz. Daha sonra kullanmak için StorSimple cihazı içerik veritabanını taşımak için SQL Server en iyi yöntemler kurdu. 
   
   > [!NOTE]
   > Cihaza içerik veritabanını taşıma (Bunun 5000 veya 7000 Serisi için desteklenmiyor) StorSimple 8000 serisi için yalnızca desteklenir.
   
   StorSimple cihazı ayrı birimlerde içinde BLOB'ları ve içerik veritabanını depolarsanız, aynı birim kapsayıcısı içinde yapılandırmadan öneririz. Bu, bunlar birlikte yedeklenecek olmasını sağlar.
   
   > [!WARNING]
   > KKY etkinleştirmediyseniz, StorSimple cihazı içerik veritabanını taşıma önermiyoruz. Bu sınanmamış bir yapılandırmadır.
   
9. Sonraki adıma gidin: [çöp toplama yapılandırma](#configure-garbage-collection).

[6]: https://technet.microsoft.com/library/ff628254(v=office.15).aspx
[7]: https://technet.microsoft.com/library/ff628255(v=office.14).aspx
