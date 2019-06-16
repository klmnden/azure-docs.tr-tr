---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: f84fe995e65d2b67aaaf4ff9acc4a6a44ce607dc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66155814"
---
> [!NOTE]
> SharePoint KKY yapılandırması için StorSimple bağdaştırıcısı değişiklikler yaparken, Domain Admins grubuna ait bir kullanıcı hesabıyla oturum. Ayrıca, merkezi yönetim olarak aynı konakta çalışan bir tarayıcıdan yapılandırma sayfasında erişmeniz gerekir.
> 
> 

#### <a name="to-configure-rbs"></a>KKY yapılandırmak için
1. SharePoint Merkezi Yönetim sayfasını açın ve gidin **sistem ayarlarını**. 
2. İçinde **Azure StorSimple** bölümünde **StorSimple bağdaştırıcısını yapılandırın**.
   
    ![StorSimple bağdaştırıcısını yapılandırın](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS1-include.png) 
3. Üzerinde **StorSimple bağdaştırıcısını yapılandırın** sayfası:
   
   1. Emin olun **düzenleme yolunu etkinleştirmek** onay kutusu seçilidir.
   2. Metin kutusuna BLOB deposuna Evrensel Adlandırma Kuralı (UNC) yolunu yazın.
      
      > [!NOTE]
      > BLOB Depolama Birimi, StorSimple cihazında bir iSCSI birimi üzerinde barındırılması gerekir.

   3. Tıklayın **etkinleştirme** uzak depolama için yapılandırmak istediğiniz içerik veritabanlarının her biri aşağıda düğmesi.
      
      > [!NOTE]
      > BLOB Depolama tüm web ön uç (WFE) sunucular tarafından paylaşılan ve erişilebilir olmalıdır ve SharePoint sunucu grubu için yapılandırılmış kullanıcı hesabının bu paylaşıma erişiminiz olmalıdır.
      
      ![KKY sağlayıcısını etkinleştirin](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS2-include.png)
      
      Etkinleştirdiğinizde veya KKY devre dışı bıraktığınızda, şu iletiyi görürsünüz.
      
      ![StorSimple bağdaştırıcısı etkinleştirme devre dışı bırakma yapılandırma](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_ConfigureStorSimpleAdapterEnableDisableMessage-include.png)

   4. Tıklayın **güncelleştirme** yapılandırmayı uygulamak için düğme. Tıkladığınızda **güncelleştirme** düğmesi tüm WFE sunucuları üzerinde KKY yapılandırma durumu güncelleştirilir ve tüm Grup KKY etkin olacaktır. Aşağıdaki ileti görüntülenir.
      
      ![Bağdaştırıcı yapılandırma iletisi](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS3-include.png)
      
      > [!NOTE]
      > Çok büyük sayıda (200 ' büyük) veritabanları bir SharePoint çiftliği için KKY yapılandırıyorsanız SharePoint Yönetim Merkezi web sayfası zaman aşımına uğrayabilir. Bu meydana gelirse, sayfayı yenileyin. Bu yapılandırma işlemini etkilemez.

4. Yapılandırmayı doğrulayın:
   
   1. SharePoint Merkezi Yönetim Web sitesine oturum açın ve gidin **StorSimple bağdaştırıcısını yapılandırın** sayfası.
   2. Girdiğiniz ayarları eşleştiğinden emin olmak için yapılandırma ayrıntılarını kontrol edin. 
5. KKY düzgün çalıştığını doğrulayın:
   
   1. Bir belge SharePoint'e yükle. 
   2. Yapılandırdığınız UNC yolu belirtin. KKY dizin yapısı oluşturulduğunu ve karşıya yüklenen nesnesi içerdiğinden emin olun.
6. (İsteğe bağlı) Microsoft RBS kullanabileceğiniz `Migrate()` StorSimple cihazına mevcut BLOB içeriği taşımak için SharePoint dahil olanlar PowerShell cmdlet'i. Daha fazla bilgi için [içine veya dışına SharePoint 2013'te KKY İçerik Geçişi] [ 6] veya [içine veya dışına KKY (SharePoint Foundation 2010) İçerik Geçişi] [7].
7. (İsteğe bağlı) Test yüklemelerinde Blobları gibi içerik veritabanından taşınan doğrulayabilirsiniz: 
   
   1. SQL Management Studio'yu başlatın.
   2. Listblobsındb_2010.SQL veya Listblobsındb_2013.SQL sorgu şu şekilde çalıştırın.
      
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
      
      KKY doğru şekilde yapılandırıldıysa, bir NULL değer karşıya ve başarıyla ile KKY te dış herhangi bir nesne SizeOfContentInDB Sütun görüntülenmelidir.
8. (İsteğe bağlı) KKY yapılandırmak ve StorSimple cihazına tüm BLOB içeriğini taşımak sonra cihaza içerik veritabanını taşıyabilirsiniz. İçerik veritabanını taşımak isterseniz, cihazdaki birincil bir birim olarak içerik veritabanını depolama yapılandırmanızı öneririz. Ardından, SQL Server en iyi yöntemler için StorSimple cihazı içerik veritabanını geçirmek için yerleşik. 
   
   > [!NOTE]
   > Cihaza içerik veritabanını taşıma (Bu 5000 veya 7000 Serisi için desteklenmez) StorSimple 8000 serisi için yalnızca desteklenir.
   
   StorSimple cihazında ayrı birimlerdeki Blobları ve içerik veritabanını depoluyorsanız bunları aynı birim kapsayıcısında yapılandırmanızı öneririz. Bu, bunlar birlikte yedeklenir, sağlar.
   
   > [!WARNING]
   > KKY etkinleştirmediyseniz, StorSimple cihazı için içerik veritabanını taşıma önerilmez. Bu test edilmemiş bir yapılandırmadır.
   
9. Sonraki adıma gidin: [Çöp toplamayı yapılandır](#configure-garbage-collection).

[6]: https://technet.microsoft.com/library/ff628254(v=office.15).aspx
[7]: https://technet.microsoft.com/library/ff628255(v=office.14).aspx
