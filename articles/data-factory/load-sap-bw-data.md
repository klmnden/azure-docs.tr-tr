---
title: Veri yükleme SAP Business Warehouse Azure Data Factory kullanarak | Microsoft Docs
description: SAP Business Warehouse (BW) veri kopyalamak için Azure Data factory'yi kullanın.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/19/2019
ms.author: jingwang
ms.openlocfilehash: 9a123ed45b5857aa40fc9853a95c528833ba8aa9
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59523197"
---
# <a name="copy-data-from-sap-business-warehouse-by-using-azure-data-factory"></a>Azure Data Factory kullanarak SAP Business Warehouse veri kopyalama

Bu makalede Azure Data Factory için Azure Data Lake depolama Gen2 açık hub'ı aracılığıyla SAP Business Warehouse (BW) gelen verileri kopyalamak için nasıl kullanılacağını gösterir. Benzer bir süreç diğerine verileri kopyalamak için kullanabileceğiniz [havuz veri depolarına desteklenen](copy-activity-overview.md#supported-data-stores-and-formats).

> [!TIP]
> SAP BW Open Hub tümleştirmesi ve delta ayıklama akışı dahil olmak üzere SAP BW veri kopyalama hakkında genel bilgi için bkz. [veri kopyalama SAP Business Warehouse açık hub'ı aracılığıyla gelen Azure Data Factory kullanarak](connector-sap-business-warehouse-open-hub.md).

## <a name="prerequisites"></a>Önkoşullar

- **Azure veri fabrikası**: Yoksa, adımları [veri fabrikası oluşturma](quickstart-create-data-factory-portal.md#create-a-data-factory).

- **SAP BW Open Hub hedef (OHD) hedef türü "Veritabanı tablo" olan**: Bir OHD oluşturmak veya, OHD Data Factory tümleştirme için doğru şekilde yapılandırıldığını denetlemek için bkz: [SAP BW Open Hub hedef yapılandırmaları](#sap-bw-open-hub-destination-configurations) bu makalenin.

- **SAP BW kullanıcı aşağıdaki izinler gerekiyor**:

  - Uzak işlev çağrıları (RFC) için yetkilendirme ve SAP BW.
  - "Yürütme" etkinliğini izinleri **S_SDSAUTH** yetkilendirme nesnesi.

- **A [barındırılan tümleştirme çalışma zamanını (IR)](concepts-integration-runtime.md#self-hosted-integration-runtime) SAP .NET Bağlayıcısı 3.0**. Bu kurulum adımları izleyin:

  1. Yükleyin ve şirket içinde barındırılan tümleştirme çalışma zamanı 3.13 veya sonraki bir sürümü kaydedin. (Bu işlem, bu makalenin sonraki bölümlerinde açıklanmıştır.)

  2. İndirme [Microsoft .NET 3.0 için 64-bit SAP Bağlayıcısı](https://support.sap.com/en/product/connectors/msnet.html) SAP'nin sitesinden ve şirket içinde barındırılan IR ile aynı bilgisayara yükleme Yükleme sırasında seçtiğinizden emin olun **yükleme derlemeleri GAC'ye** içinde **isteğe bağlı kurulum adımlarını** iletişim kutusu, aşağıda gösterildiği olarak:

     ![SAP .NET Bağlayıcısı iletişim kutusu ayarlama](media/connector-sap-business-warehouse-open-hub/install-sap-dotnet-connector.png)

## <a name="do-a-full-copy-from-sap-bw-open-hub"></a>SAP BW Open Hub'ından tam bir kopyasını yapın

Azure portalında veri fabrikanıza gidin. Seçin **yazar ve İzleyici** Data Factory kullanıcı arabirimini ayrı bir sekmede açmak için.

1. Üzerinde **başlayalım** sayfasında **veri kopyalama** veri kopyalama aracını açmak için.

2. Üzerinde **özellikleri** sayfasında, belirtin bir **görev adı**ve ardından **sonraki**.

3. Üzerinde **kaynak veri deposu** sayfasında **+ yeni bağlantı oluştur**. Seçin **SAP BW Open Hub** bağlayıcı galeri ve ardından **devam**. Bağlayıcıları Filtrele için yazabilirsiniz **SAP** arama kutusuna.

4. Üzerinde **belirtin SAP BW Open Hub bağlantısı** sayfasında, yeni bir bağlantı oluşturmak için aşağıdaki adımları izleyin.

   ![SAP BW Open Hub için bağlı hizmet sayfası oluşturma](media/load-sap-bw-data/create-sap-bw-open-hub-linked-service.png)

   1. Gelen **tümleştirme çalışma zamanı aracılığıyla Bağlan** listesinde, var olan bir şirket içinde barındırılan IR seçin Veya, henüz yoksa, oluşturmak seçin.

      Yeni bir şirket içinde barındırılan IR oluşturmak için Seç **+ yeni**ve ardından **şirket içinde barındırılan**. Girin bir **adı**ve ardından **sonraki**. Seçin **hızlı kurulum** geçerli bilgisayarda yüklemeniz veya izleyin **el ile Kurulum** sağlanan adımları.

      Belirtildiği gibi [önkoşulları](#prerequisites), SAP bağlayıcısı için Microsoft .NET 3.0 kendinden konak IR çalıştığı aynı bilgisayara yüklenmiş olduğundan emin olun.

   2. SAP BW doldurun **sunucu adı**, **sistem numarası**, **istemci kimliği** **dil** (Eğer dışında **tr**) , **Kullanıcı adı**, ve **parola**.

   3. Seçin **Bağlantıyı Sına** ayarları doğrulayın ve ardından **son**.

   4. Yeni bir bağlantı oluşturulur. **İleri**’yi seçin.

5. Üzerinde **açık Hub hedefleri seçin** sayfasında, SAP BW içinde kullanılabilir olan açık Hub hedefleri göz atın. Verileri kopyalayın ve ardından OHD seçin **sonraki**.

   ![SAP BW Open Hub hedef tablo seçin](media/load-sap-bw-data/select-sap-bw-open-hub-table.png)

6. Bir gereksinim duyarsanız bir filtre belirtin. OHD yalnızca bir tek istek kimliği ile bir tek veri aktarım işlemi (DTP) yürütme verileri içeren ya da kendi DTP tamamlandıktan ve NET verileri kopyalamak istediğiniz emin **hariç son istek** onay kutusu.

   Bu ayarları hakkında daha fazla bilgi [SAP BW Open Hub hedef yapılandırmaları](#sap-bw-open-hub-destination-configurations) bu makalenin. Seçin **doğrulama** hangi verilerin döndürülecek bir kez daha denetleyin. Sonra **İleri**’yi seçin.

   ![SAP BW Open Hub filtresini Yapılandır](media/load-sap-bw-data/configure-sap-bw-open-hub-filter.png)

7. Üzerinde **hedef veri deposuna** sayfasında **+ yeni bağlantı oluştur** > **Azure Data Lake depolama Gen2**  >   **Devam**.

8. Üzerinde **belirtin Azure Data Lake Storage bağlantı** sayfasında, bir bağlantı oluşturmak için aşağıdaki adımları izleyin.

   ![Bir ADLS Gen2'ye bağlı hizmet sayfası oluşturma](media/load-sap-bw-data/create-adls-gen2-linked-service.png)

   1. Gen2 özellikli Data Lake Storage hesabınızdan seçin **adı** aşağı açılan listesi.
   2. Seçin **son** bağlantı oluşturmak için. Sonra **İleri**’yi seçin.

9. Üzerinde **çıktı dosyasını veya klasörünü seçin** want **copyfromopenhub** çıkış klasörü adı. Sonra **İleri**’yi seçin.

   ![Çıkış klasörü sayfası seçin](media/load-sap-bw-data/choose-output-folder.png)

10. Üzerinde **dosya biçimi ayarını** sayfasında **sonraki** varsayılan ayarları kullanmak için.

    ![Havuz biçimi sayfası belirtin](media/load-sap-bw-data/specify-sink-format.png)

11. Üzerinde **ayarları** sayfasında **performans ayarları**. İçin bir değer girin **kopyalama paralellik derecesi** gibi SAP BW paralel olarak yüklemek için 5. Sonra **İleri**’yi seçin.

    ![Kopya ayarlarını yapılandırma](media/load-sap-bw-data/configure-copy-settings.png)

12. Üzerinde **özeti** sayfasında, ayarları gözden geçirin. Sonra **İleri**’yi seçin.

13. Üzerinde **dağıtım** sayfasında **İzleyici** işlem hattını izleme.

    ![Dağıtım sayfası](media/load-sap-bw-data/deployment.png)

14. Dikkat **İzleyici** sayfanın sol tarafındaki sekmesinde otomatik olarak seçilir. **Eylemleri** sütun etkinliği çalıştırma ayrıntılarını görüntülemek ve işlem hattını yeniden çalıştırma bağlantılarını içerir.

    ![İşlem hattını izleme görünümü](media/load-sap-bw-data/pipeline-monitoring.png)

15. İşlem hattı çalıştırması ile ilişkili etkinlik çalıştırmalarını görüntülemek için seçin **etkinlik çalıştırmalarını görüntüle** içinde **eylemleri** sütun. İşlem hattında yalnızca bir etkinlik (kopyalama etkinliği) olduğundan tek bir girdi görürsünüz. İşlem hattı çalıştırmaları görünümüne geçmek için seçin **işlem hatları** üstündeki bağlantısı. Listeyi yenilemek için **Yenile**’yi seçin.

    ![Ekran etkinliğini izleme](media/load-sap-bw-data/activity-monitoring.png)

16. Her bir kopyalama etkinliği için yürütme ayrıntıları izlemek için **ayrıntıları** bir gözlük simge olan bağlantı **eylemleri** etkinliği izleme görünümünde. Havuz, veri aktarım hızı, yürütme adımları ve süresi kaynağından kopyalanan veri hacmi ve kullanılan yapılandırmalar mevcut ayrıntıları içerir.

    ![Etkinlik ayrıntıları izleme](media/load-sap-bw-data/activity-monitoring-details.png)

17. Görüntülenecek **en fazla istek kimliği**dönün Etkinlik izleme görünümü ve seçin **çıkış** altında **eylemleri**.

    ![Etkinlik çıkış ekranı](media/load-sap-bw-data/activity-output.png)

    ![Etkinlik çıkışı Ayrıntıları görünümü](media/load-sap-bw-data/activity-output-details.png)

## <a name="do-an-incremental-copy-from-sap-bw-open-hub"></a>SAP BW Open hub'dan bir artımlı kopya yapın

> [!TIP]
> Bkz: [SAP BW Open Hub Bağlayıcısı delta ayıklama akışı](connector-sap-business-warehouse-open-hub.md#delta-extraction-flow) nasıl Data factory'de SAP BW açık hub'ı bağlayıcı artımlı veri SAP BW kopyalar öğrenin. Bu makalede temel Bağlayıcı yapılandırması anlamanıza da yardımcı olabilir.

Artık, artımlı kopyadan SAP BW Open hub'ı yapılandırmak devam edelim.

Artımlı kopyalama kullanan temel alan bir "üst eşik" mekanizması **istek kimliği**. Bu kimliği, SAP BW Open Hub hedef DTP tarafından otomatik olarak oluşturulur. Bu iş akışı aşağıdaki diyagramda gösterilmiştir:

![Artımlı kopyalama iş akışı akış çizelgesi](media/load-sap-bw-data/incremental-copy-workflow.png)

Data factory üzerinde **başlayalım** sayfasında **şablondan işlem hattı Oluştur** yerleşik şablonu kullanmak için.

1. Arama **SAP BW** bulmak ve seçmek için **artımlı kopyalama SAP BW için Azure Data Lake depolama Gen2** şablonu. Bu şablon, verileri Azure Data Lake depolama Gen2 kopyalar. Benzer bir iş akışı, diğer havuz türlerine kopyalamak için kullanabilirsiniz.

2. Şablonun ana sayfasında, seçin veya aşağıdaki üç bağlantıları oluşturma ve ardından **bu şablonu kullan** penceresinin sağ alt köşesindeki.

   - **Azure Blob Depolama**: Bu kılavuzda, Azure Blob Depolama, üst eşik depolamak için kullandığımız *en fazla istek kimliği kopyalanan*.
   - **SAP BW Open Hub**: Bu veri kopyalamak için kaynaktır. Önceki tam kopya izlenecek ayrıntılı yapılandırma için bakın.
   - **Azure Data Lake depolama Gen2**: Verileri kopyalamak havuz budur. Önceki tam kopya izlenecek ayrıntılı yapılandırma için bakın.

   ![SAP BW şablondan artımlı kopya](media/load-sap-bw-data/incremental-copy-from-sap-bw-template.png)

3. Bu şablon, aşağıdaki üç etkinliklerle bir işlem hattı oluşturur ve bunları zincirleme başarı yapar: *Arama*, *veri kopyalama*, ve *Web*.

   İşlem hattı Git **parametreleri** sekmesi. Sağlamanız gereken tüm yapılandırmaları görürsünüz.

   ![Artımlı kopyadan SAP BW yapılandırma](media/load-sap-bw-data/incremental-copy-from-sap-bw-pipeline-config.png)

   - **SAPOpenHubDestinationName**: Verileri kopyalamak için açık Hub tablo adını belirtin.

   - **ADLSGen2SinkPath**: Verileri kopyalamak için hedef Azure Data Lake depolama Gen2 yolu belirtin. Yol mevcut değilse Data Factory kopyalama etkinliği yürütülürken bir yol oluşturur.

   - **HighWatermarkBlobPath**: Üst eşik değerini depolamak için yolları belirten `container/path`.

   - **HighWatermarkBlobName**: Üst eşik değerini depolamak için blob adı belirtin `requestIdCache.txt`. BLOB Depolama alanında gibi HighWatermarkBlobPath + HighWatermarkBlobName, karşılık gelen yoluna gidin *container/path/requestIdCache.txt*. Blob içeriği 0 ile oluşturun.

      ![Blob içeriği](media/load-sap-bw-data/blob.png)

   - **LogicAppURL**: Bu şablon Azure Logic Apps, Blob Depolama alanında üst eşik değerini ayarlamak için çağrılacak WebActivity kullanın. Ya da depolamak için Azure SQL veritabanı'nı kullanabilirsiniz. Değerini güncelleştirmek için bir saklı yordam etkinliği kullanın.

      Öncelikle, aşağıda gösterildiği gibi bir mantıksal uygulama oluşturmanız gerekir. Ardından, yapıştırın **HTTP POST URL'si**.

      ![Mantıksal uygulama yapılandırması](media/load-sap-bw-data/logic-app-config.png)

      1. Azure portalına gidin. Yeni bir seçin **Logic Apps** hizmeti. Seçin **+ boş mantıksal uygulama** gitmek için **Logic Apps Tasarımcısı'nda**.

      2. Bir tetikleyici oluşturmak **olduğunda bir HTTP isteği alındığında**. HTTP istek gövdesi aşağıdaki gibi belirtin:

         ```json
         {
            "properties": {
               "sapOpenHubMaxRequestId": {
                  "type": "string"
               },
               "type": "object"
            }
         }
         ```

      3. Ekleme bir **blob Oluştur** eylem. İçin **klasör yolu** ve **Blob adı**, daha önce yapılandırdığınız aynı değerlerinde **HighWatermarkBlobPath** ve **HighWatermarkBlobName**.

      4. **Kaydet**’i seçin. Ardından değerini kopyalayıp **HTTP POST URL'si** Data Factory işlem hattı, kullanılacak.

4. Data Factory işlem hattı parametrelerinin verdikten sonra seçin **hata ayıklama** > **son** yapılandırmasını doğrulamak için bir çalıştırma başlatmak için. Ya da seçin **tümünü Yayımla** değişiklikleri yayımlayın ve ardından **tetikleyici** çalıştırma yürütülemiyor.

## <a name="sap-bw-open-hub-destination-configurations"></a>SAP BW Open Hub hedef yapılandırmalar

Bu bölüm, yapılandırma verileri kopyalamak için veri fabrikasında SAP BW Open hub'ı bağlayıcıyı kullanmak üzere SAP BW tarafı tanıtır.

### <a name="configure-delta-extraction-in-sap-bw"></a>SAP BW delta ayıklama yapılandırın

Geçmiş kopyalama ve artımlı kopyalama veya yalnızca artımlı kopya gerekiyorsa, SAP BW delta ayıklama yapılandırın.

1. Açık Hub hedef oluşturun. SAP işlem gerekli dönüştürme ve veri aktarım işlemi otomatik olarak oluşturan RSA1 içinde OHD oluşturabilirsiniz. Aşağıdaki ayarları kullanın:

   - **ObjectType**: Herhangi bir nesne türü kullanabilirsiniz. Burada kullandığımız **Infocube** örnek olarak.
   - **Hedef türü**: Seçin **veritabanı tablo**.
   - **Tablonun anahtar**: Seçin **teknik anahtar**.
   - **Ayıklama**: Seçin **tabloya veri ve Ekle kayıtları saklamak**.

   ![SAP BW OHD delta ayıklama iletişim kutusu oluşturma](media/load-sap-bw-data/create-sap-bw-ohd-delta.png)

   ![SAP BW OHD delta2 ayıklama iletişim kutusu oluşturma](media/load-sap-bw-data/create-sap-bw-ohd-delta2.png)

   Paralel çalışan SAP iş işlemleri için DTP sayısını artırmanız:

   ![Oluştur-sap-bw-ohd-delta3](media/load-sap-bw-data/create-sap-bw-ohd-delta3.png)

2. İşlem zincirleri içinde DTP zamanlayın.

   Gerekli satırları sıkıştırılmış henüz bir küp için bir delta DTP yalnızca çalışır. BW küpü sıkıştırma açık Hub tabloya önce DTP çalışmadığından emin olun. Bunu yapmanın en kolay yolu, var olan işlem zincirleri içinde DTP tümleştirme sağlamaktır. Aşağıdaki örnekte, ('ohd) DTP arasında işlem zincire eklenen *Ayarla* (toplam değer dökümü) ve *Daralt* (küp sıkıştırma) adımları.

   ![SAP BW işlem zinciri akış grafiği oluşturma](media/load-sap-bw-data/create-sap-bw-process-chain.png)

### <a name="configure-full-extraction-in-sap-bw"></a>SAP BW tam ayıklama yapılandırın

Delta ayıklama yanı sıra, aynı SAP BW InfoProvider'ın tam bir ayıklama isteyebilirsiniz. Bu genellikle kopyalama tam istiyorsanız geçerlidir ancak artımlı olmayan veya istediğiniz [resync delta ayıklama](#resync-delta-extraction).

Aynı OHD için birden fazla DTP sahip olamaz. Bu nedenle, bir ek OHD delta ayıklama önce oluşturmanız gerekir.

![SAP BW OHD tam oluşturma](media/load-sap-bw-data/create-sap-bw-ohd-full.png)

Bir tam yük için OHD, delta ayıklama değerinden farklı seçenekleri seçin:

- OHD içinde: Ayarlama **ayıklama** seçeneğini **verileri silmek ve kayıtları eklemek**. Aksi takdirde, verileri birden çok kez ne zaman, BW işlem zincirindeki DTP yineleyin ayıklanır.

- DTP içinde: Ayarlama **ayıklama modu** için **tam**. Otomatik olarak oluşturulan DTP gelen değiştirmeli **Delta** için **tam** bu görüntüde gösterildiği gibi hemen OHD oluşturulduktan sonra:

   ![SAP BW OHD oluşturmak için "Tam" ayıklama yapılandırılmış iletişim kutusu](media/load-sap-bw-data/create-sap-bw-ohd-full2.png)

- Data Factory içinde açık Hub'ı BW Bağlayıcısı: Devre dışı **dışlama son istek**. Aksi takdirde, hiçbir şey ayıklanır.

Genellikle tam DTP el ile çalıştırın. Veya, bir işlem zincirine için tam DTP oluşturabilirsiniz. Bu genellikle, var olan işlem zincirleri bağımsızdır ayrı zinciri olur. Her iki durumda da *DTP, Data Factory kopyalama kullanarak ayıklama başlamadan önce bittiğini doğrulayın*. Aksi takdirde, yalnızca kısmi veri kopyalanır.

### <a name="run-delta-extraction-the-first-time"></a>Delta ayıklama ilk kez çalıştırma

Teknik olarak ilk delta ayıklama olduğu bir *tam ayıklama*. Varsayılan olarak, SAP BW açık hub'ı bağlayıcı veri kopyalarken son istek hariç tutar. Ayrı bir istek kimliğiyle tabloda değişiklik verilerini sonraki DTP oluşturur kadar ilk delta ayıklama için Data Factory kopyalama etkinliği tarafından hiçbir veri ayıklandı Bu senaryonun olmaması için iki yolu vardır:

- Devre dışı **dışlama son istek** ilk delta ayıklama seçeneği. Delta ayıklama ilk kez başlamadan önce ilk delta DTP tamamlandı olduğundan emin olun.
-  Sonraki bölümde açıklandığı gibi delta ayıklama yeniden eşitlenmesi için yordamı kullanın.

### <a name="resync-delta-extraction"></a>Delta ayıklama yeniden eşitleme

Aşağıdaki senaryolarda, SAP BW küp verilerinde değiştirebilirsiniz ancak DTP delta tarafından kabul edilmez:

- SAP BW seçmeli silme işlemi (herhangi bir filtre koşulu kullanarak satırlar)
- SAP BW isteği silinmesini (Hatalı istek)

SAP açık Hub hedef bir veri reyonu denetimindeki verileri hedef (2015 beri tüm SAP BW desteği paketleri) değil. Bu nedenle, bir küp OHD verileri değiştirmeden verileri silebilirsiniz. Ardından, Data Factory ile veri küpünün resync gerekir:

1. Tam ayıklama (SAP içinde tam bir DTP kullanarak) Data Factory'de çalıştırın.
2. Delta DTP açık Hub tablodaki tüm satırları silin.
3. Delta DTP durumunu ayarlamak için **alındı**.

Bundan sonra tüm sonraki değişim DTPs ve Data Factory delta ayıklamalar beklendiği gibi çalışmayabilir.

Delta DTP durumunu ayarlamak için için **alındı**, delta DTP el ile çalıştırmak için aşağıdaki seçenekleri kullanabilirsiniz:

    *No Data Transfer; Delta Status in Source: Fetched*

## <a name="next-steps"></a>Sonraki adımlar

SAP BW Open Hub connector desteği hakkında bilgi edinin:

> [!div class="nextstepaction"]
>[SAP Business Warehouse açık Hub Bağlayıcısı](connector-sap-business-warehouse-open-hub.md)
