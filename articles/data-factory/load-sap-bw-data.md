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
ms.openlocfilehash: 9458903378576a50db9be92b9377987829e1ba41
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58200166"
---
# <a name="load-data-from-sap-business-warehouse-bw-by-using-azure-data-factory"></a>Azure Data Factory kullanarak SAP Business Warehouse (BW) veri yükleme

Bu makalede Data Factory kullanımına ilişkin bir kılavuz gösterir _veri yükleme açık hub'ı aracılığıyla SAP Business Warehouse (BW) gelen Azure Data Lake depolama Gen2_. Diğer veri kopyalamak için benzer adımları izleyebilirsiniz [havuz veri depolarına desteklenen](copy-activity-overview.md#supported-data-stores-and-formats). 

> [!TIP]
> Başvurmak [SAP BW Open Hub Bağlayıcısı makalesi](connector-sap-business-warehouse-open-hub.md) veri SAP BW genel kopyalama ile ilgili, SAP BW Open Hub tümleştirmesi ve delta ayıklama akışını giriş dahil.

## <a name="prerequisites"></a>Önkoşullar

- **Azure Data Factory (ADF):** Veri Fabrikası yoksa izleyin "[veri fabrikası oluşturma](quickstart-create-data-factory-portal.md#create-a-data-factory)" bölümü oluşturun. 

- **SAP BW Open Hub hedef (OHD) "Veritabanı tablo" olarak hedef türüne sahip.** İzleyin [SAP BW Open Hub hedef yapılandırmaları](#sap-bw-open-hub-destination-configurations) bölümü oluşturun veya ADF ile tümleştirilmesi için mevcut OHD düzgün yapılandırılıp yapılandırılmadığını doğrulamak üzere.

- **SAP BW kullanıcının kullanılan aşağıdaki izinleri olmalıdır:**

  - RFC ve SAP BW için yetkilendirme.
  - İzinleri "**yürütme**"Etkinlik yetkilendirme nesnenin"**S_SDSAUTH**".

- **[Tümleştirme çalışma zamanı barındırma](concepts-integration-runtime.md#self-hosted-integration-runtime) SAP .NET bağlayıcısıyla 3.0 gereklidir**. Yapmanız gereken hazırlıklar ayrıntılı aşağıdadır:

  1. Yükleme ve şirket içinde barındırılan IR sürümü ile kaydetme > = (Aşağıdaki kılavuzda ele) 3.13. 

  2. İndirme [64-bit SAP .NET bağlayıcı 3.0](https://support.sap.com/en/product/connectors/msnet.html) SAP'nin sitesinden ve şirket içinde barındırılan IR makineye yükleyin.  Ne zaman yükleme, "isteğe bağlı kurulum adımlarını" penceresinde seçtiğinizden emin olun "**yükleme derlemeleri GAC'ye**" seçeneği aşağıdaki görüntüde gösterildiği gibi.

     ![SAP .NET Bağlayıcısı'nı Ayarla](media/connector-sap-business-warehouse-open-hub/install-sap-dotnet-connector.png)

## <a name="full-copy-from-sap-bw-open-hub"></a>SAP BW Open Hub'ından tam kopya

Azure Portal'da data factory'nizi Git seçin -> **yazar ve İzleyici** ADF kullanıcı arabirimini ayrı bir sekmede açmak için. 

1. **Başlayalım** sayfasında, Veri Kopyalama aracını açmak için **Veri Kopyala**’yı seçin. 

2. Üzerinde **özellikleri** sayfasında, bir ad belirtin **görev adı** alan ve seçim **sonraki**.

3. Üzerinde **kaynak veri deposu** sayfasında **+ yeni bağlantı oluştur** seçin -> **SAP BW Open Hub** bağlayıcı galerisinden seçin -> **devamet**. "SAP" bağlayıcıları filtrelemek için arama kutusuna yazabilirsiniz.

4. Üzerinde **belirtin SAP BW Open Hub bağlantısı** sayfası 

   ![SAP BW Open bağlı Hub hizmeti oluşturma](media/load-sap-bw-data/create-sap-bw-open-hub-linked-service.png)

   1. Seçin **tümleştirme çalışma zamanı aracılığıyla Bağlan**: mevcut bir şirket içinde barındırılan IR seçmek için aşağı açılan listeye tıklayın ya da şirket içinde barındırılan IR henüz yoksa bir tane oluşturun. 

      Yeni oluşturmak için tıklayın **+ yeni** select türü açılan menü-> **şirket içinde barındırılan** -> belirtin bir **adı** tıklatıp **sonraki** -> seçin **Hızlı kurulum** geçerli makinede yükleme veya izleyin **el ile Kurulum** adımlar vardır.

      Belirtildiği gibi [önkoşulları](#prerequisites), ayrıca sahip olduğunuzdan emin olun **SAP .NET bağlayıcı 3.0** şirket içinde barındırılan IR çalıştığı aynı makinede yüklü.

   2. SAP BW belirtin **sunucu adı**, **sistem numarası**, **istemci kimliği** **dil** (Eğer tr dışında), **kullanıcıadı**, ve **parola**.

   3. Tıklayın **Bağlantıyı Sına** ayarlarını doğrulamak için seçip **son**.

   4. Yeni bir bağlantı oluşturulduğunu görürsünüz. **İleri**’yi seçin.

5. Üzerinde **açık hub'ı seçin hedefleri** sayfasında, SAP BW üzerinde açık Hub hedefleriyle göz atın ve deposundan veri kopyalamayı ve ardından istediğiniz kuralı seçin **sonraki**.

   ![SAP BW Open Hub tablo seçin](media/load-sap-bw-data/select-sap-bw-open-hub-table.png)

6. Gerekirse, filtre belirtin. Açık Hub hedefiniz yalnızca tek bir istek kimliği ile tek bir veri aktarım işlemi (DTP) yürütme verileri içeriyorsa veya emin, DTP tamamlandı ve tüm verileri kopyalamak istediğiniz işaretini kaldırın **hariç son istek**. SAP BW yapılandırmanızı nasıl bu ayarları daha ile ilgili bilgi edinebilirsiniz [SAP BW Open Hub hedef yapılandırmaları](#sap-bw-open-hub-destination-configurations) bölümü. Tıklayın **doğrulama** çift verileri denetlemek için döndürülen, ardından **sonraki**.

   ![SAP BW Open Hub filtresini Yapılandır](media/load-sap-bw-data/configure-sap-bw-open-hub-filter.png)

7. İçinde **hedef veri deposuna** sayfasında **+ yeni bağlantı oluştur**ve ardından **Azure Data Lake depolama Gen2'ye**seçip **devam**.

8. İçinde **belirtin Azure Data Lake Storage bağlantı** sayfası 

   ![ADLS Gen2'ye bağlı hizmeti oluşturma](media/load-sap-bw-data/create-adls-gen2-linked-service.png)

   1. Data Lake depolama Gen2 özellikli hesabınızı "Depolama hesabı adı" aşağı açılan listeden seçin.
   2. Seçin **son** bağlantı oluşturmak için. Sonra **İleri**’yi seçin.

9. İçinde **çıktı dosyasını veya klasörünü seçin** sayfasında çıkış klasör adı olarak "copyfromopenhub" girin ve seçin **sonraki**.

   ![Çıkış klasörü seçin](media/load-sap-bw-data/choose-output-folder.png)

10. İçinde **dosya biçimi ayarını** sayfasında **sonraki** varsayılan ayarları kullanmak için.

    ![Havuz biçimini belirtin](media/load-sap-bw-data/specify-sink-format.png)

11. İçinde **ayarları** sayfasında **performans ayarları**, ayarlayıp **kopyalama paralellik derecesi** gibi SAP BW paralel olarak yüklemek için 5. **İleri**’ye tıklayın.

    ![Kopya ayarlarını yapılandırma](media/load-sap-bw-data/configure-copy-settings.png)

12. İçinde **özeti** sayfasında, ayarları gözden geçirin ve seçin **sonraki**.

13. İçinde **dağıtım** sayfasında **İzleyici** işlem hattını izleme.

    ![Dağıtım sayfası](media/load-sap-bw-data/deployment.png)

14. Soldaki **İzleyici** sekmesinin otomatik olarak seçildiğine dikkat edin. **Eylemleri** sütununda etkinlik çalıştırması ayrıntılarını görüntüleme ve işlem hattını yeniden çalıştırma bağlantılarını içerir:

    ![İşlem hattını izleme](media/load-sap-bw-data/pipeline-monitoring.png)

15. İşlem hattı çalıştırması ile ilişkili etkinlik çalıştırmalarını görüntülemek için seçin **etkinlik çalıştırmalarını görüntüle** bağlantısını **eylemleri** sütun. İşlem hattında yalnızca bir etkinlik (kopyalama etkinliği) olduğundan tek bir girdi görürsünüz. İşlem hattı çalıştırmaları görünümüne dönmek için seçin **işlem hatları** üstündeki bağlantısı. Listeyi yenilemek için **Yenile**’yi seçin.

    ![Etkinlik izleme](media/load-sap-bw-data/activity-monitoring.png)

16. Her bir kopyalama etkinliği için yürütme ayrıntıları izlemek için **ayrıntıları** bağlantısını (gözlük resmi) altında **eylemleri** izleme görünümü etkinlik. Veri kaynağından kopyalanan havuz, veri aktarım hızı, yürütme adımları karşılık gelen süre ve yapılandırmaları kullanılan gibi ayrıntıları izleyebilirsiniz:

    ![Etkinlik ayrıntıları izleme](media/load-sap-bw-data/activity-monitoring-details.png)

17. Gözden geçirme **en fazla istek kimliği** , kopyalanır. İzleme görünümü etkinliğe geri dönün, tıklayın **çıkış** altında **eylemleri**.

    ![Etkinlik çıkışı](media/load-sap-bw-data/activity-output.png)

    ![Etkinlik çıkışı ayrıntıları](media/load-sap-bw-data/activity-output-details.png)

## <a name="incremental-copy-from-sap-bw-open-hub"></a>SAP BW Open Hub'ından artımlı kopya

> [!TIP]
>
> Başvurmak [SAP BW Open Hub Bağlayıcısı delta ayıklama akışı](connector-sap-business-warehouse-open-hub.md#delta-extraction-flow) artımlı veri SAP BW kopyalayın ve en baştan bağlayıcısının ilgili temel bilgileri anlamak için bu makaleyi okuyun ADF açık Hub SAP BW Bağlayıcısı birlikte nasıl çalıştığı hakkında daha fazla bilgi için yapılandırmalar.

Artık, artımlı kopyadan SAP BW Open hub'ı yapılandırmak devam edelim. 

Artımlı kopya göre üst eşik mekanizması kullanıyor **istek kimliği** DTP tarafından SAP BW Open Hub hedef otomatik olarak oluşturulmuş. Bu yaklaşıma yönelik iş akışı şu diyagramda gösterilmiştir:

![Artımlı kopyalama iş akışı](media/load-sap-bw-data/incremental-copy-workflow.png)

ADF kullanıcı arabiriminde **başlayalım** sayfasında **şablondan işlem hattı Oluştur** yerleşik şablonundan yararlanmasına olanak. 

1. Arama adlı şablonu bulun ve "SAP BW" **artımlı kopyalama SAP BW için Azure Data Lake depolama Gen2**. Bu şablon verileri ADLS Gen2 kopyalar, diğer havuz türlerine kopyalamak için buna benzer bir akış daha sonra takip edebilirsiniz.

2. Şablon ana sayfada seçin veya aşağıdaki üç bağlantıları oluşturma ve ardından **bu şablonu kullan** sağ alt köşede.

   - **Azure Blob**: Bu kılavuzda, Azure Blob max kopyalanan istek kimliği üst eşiğin depolanacağı kullanıyoruz
   - **SAP BW Open Hub**: verileri kopyalamak için kaynak. Önceki tam kopya izlenecek ayrıntılı yapılandırmalarda bakın.
   - **ADLS Gen2**: verileri kopyalamak havuzunuzu. Önceki tam kopya izlenecek ayrıntılı yapılandırmalarda bakın.

   ![SAP BW şablondan artımlı kopya](media/load-sap-bw-data/incremental-copy-from-sap-bw-template.png)

3. Bu şablon - üç etkinliklerle bir işlem hattı oluşturur **arama, veri kopyalama ve Web** - ve bunların zincirleme başarı sağlar. İşlem hattı Git **parametreleri** sekmesinde sağlamak için ihtiyacınız olan tüm yapılandırmalar görürsünüz.

   ![SAP BW yapılandırmadan artımlı kopya](media/load-sap-bw-data/incremental-copy-from-sap-bw-pipeline-config.png)

   - **SAPOpenHubDestinationName**: verileri kopyalamak için açık Hub tablo adını belirtin.

   - **ADLSGen2SinkPath**: verileri kopyalamak için hedef ADLS Gen2'ye yolu belirtin. Yol mevcut değilse, yürütme sırasında ADF kopyalama etkinliği oluşturur.

   - **HighWatermarkBlobPath**: Örneğin, üst eşik değerini depolamak için yolunu belirtin `container/path`. 

   - **HighWatermarkBlobName**: Örneğin, üst eşik değerini depolamak için blob adı belirtin `requestIdCache.txt`. Blob depolamanızdaki HighWatermarkBlobPath + HighWatermarkBlobName, karşılık gelen yolda ör "*container/path/requestIdCache.txt*", blob içeriği 0 ile oluşturma. 

      ![Blob içeriği](media/load-sap-bw-data/blob.png)

   - **LogicAppURL**: Bu şablonda Web etkinliği Logic Apps, Blob Depolama alanında üst eşik değerini ayarlamak için çağırmak için kullanırız. Alternatif olarak, depolamak ve değerini güncelleştirmek için saklı yordam etkinliği kullanmak için SQL veritabanı kullanabilirsiniz. 

      Burada, ilk olarak, aşağıdaki gibi bir mantıksal uygulama oluşturmak için ihtiyacınız kopyalayıp **HTTP POST URL'si** Bu alan için. 

      ![Mantıksal uygulama yapılandırması](media/load-sap-bw-data/logic-app-config.png)

      1. Azure portalına gidin -> Yeni bir **Logic Apps** hizmet tıklayın -> **+ boş mantıksal uygulama** gitmek için **Logic Apps Tasarımcısı'nda**.

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

      3. Bir eylem ekleme **blob Oluştur**. "Klasör yolu" ve "Blob adı" için yukarıdaki HighWatermarkBlobPath ve HighWatermarkBlobName yapılandırılan aynı değeri kullanın.

      4. Tıklayın **Kaydet**ve ardından değerini kopyalayın **HTTP POST URL'si** ADF işlem hattında kullanılacak.

4. Tüm değerleri için ADF işlem hattı parametrelerinin verdikten sonra tıklayabilirsiniz **hata ayıklama** -> **son** yapılandırmasını doğrulamak için bir çalıştırma başlatmak için. Ya da seçebilirsiniz **tümünü Yayımla** tüm değişiklikleri yayımlamak için ardından **tetikleyici** çalıştırma yürütülemiyor.

## <a name="sap-bw-open-hub-destination-configurations"></a>SAP BW Open Hub hedef yapılandırmalar

Bu bölüm, SAP BW Open hub'ı bağlayıcı, verileri kopyalamak için ADF içinde kullanmak için gerekli yapılandırmayı SAP BW tarafında tanıtır.

### <a name="configure-delta-extraction-in-sap-bw"></a>SAP BW delta ayıklama yapılandırın

Hem geçmiş kopyalama ve artımlı kopyalama veya yalnızca artımlı kopya gerekiyorsa, SAP BW delta ayıklama yapılandırın.

1. Açık Hub hedef (OHD) oluşturma

   SAP işlem gerekli dönüştürme ve veri aktarım işlemi (DTP) otomatik olarak oluşturan RSA1 içinde OHD oluşturabilirsiniz. Aşağıdaki ayarları kullanın:

   - Nesne türü olabilir. Burada Infocube örnek kullanıyoruz.
   - **Hedef türü:** *Veritabanı tablosu*
   - **Tablonun anahtarı:** *Teknik anahtarı*
   - **Ayıklama:** *Canlı veriler ve tabloya INSERT kayıtlar*

   ![SAP BW OHD delta ayıklama oluşturma](media/load-sap-bw-data/create-sap-bw-ohd-delta.png)

   ![Oluştur-sap-bw-ohd-delta2](media/load-sap-bw-data/create-sap-bw-ohd-delta2.png)

   Paralel çalışan SAP iş işlemleri için DTP sayısını artırmanız:

   ![Oluştur-sap-bw-ohd-delta3](media/load-sap-bw-data/create-sap-bw-ohd-delta3.png)

2. İşlem zincirleri içinde DTP zamanlama

   Bir Delta DTP bir küp için yalnızca ne zaman çalıştığını satırları henüz sıkıştırıldı değildir. Bu nedenle, BW küpü sıkıştırma açık Hub tabloya önce DTP çalışmadığından emin olmanız gerekir. Bunun için en kolay yolu, var olan işlem zincirleri içinde bu DTP tümleşiyor. Aşağıdaki örnekte, ('ohd) DTP adım arasında işlem zincirine eklenir ve ayarlama (toplam değer dökümü) Daralt (küp sıkıştırma).

   ![Oluştur-sap-bw-işlem-zinciri](media/load-sap-bw-data/create-sap-bw-process-chain.png)

### <a name="configure-full-extraction-in-sap-bw"></a>SAP BW tam ayıklama yapılandırın

Delta ayıklama yanı sıra, aynı InfoProvider'ın tam bir ayıklama olmasını isteyebilirsiniz. Kopyalama artımlı gerek kalmadan tam istiyorsanız veya istediğiniz genellikle uygulandığı [delta ayıklama'yeniden eşitleme](#re-sync-delta-extraction).

Aynı OHD için birden fazla DTP olmamalıdır. Bu nedenle, ek OHD sonra delta ayıklama oluşturmanız gerekir.

![Oluştur-sap-bw-ohd-tam](media/load-sap-bw-data/create-sap-bw-ohd-full.png)

Bir tam yük için OHD, delta ayıklama değerinden farklı seçenekleri belirleyin:

- İçinde OHD: "Ayıklama" ayarı olarak "*verileri silme ve ekleme kayıtları*". Aksi takdirde verileri birden çok kez zaman BW işlem zincirindeki DTP yinelenen ayıklanmasını.

- İçinde DTP: "Ayıklama modu" olarak ayarlayın. "*tam*". Yalnızca OHD oluşturulduktan sonra otomatik olarak oluşturulan DTP tam olarak Delta değiştirmeniz gerekir:

   ![Oluştur-sap-bw-ohd-full2](media/load-sap-bw-data/create-sap-bw-ohd-full2.png)

- ADF açık Hub SAP BW Bağlayıcısı: seçeneği devre dışı "*dışlama son istek*". Aksi durumda hiçbir şey ayıklanan. 

Genellikle tam DTP el ile çalıştırın. Veya tam DTP için de bir işlem zinciri oluşturabilir - genellikle, var olan işlem zincirleri bağımsız bir ayrı işlemde zinciri olur. Her iki durumda da gerekir **DTP ADF kopyalama kullanarak ayıklama başlatmadan önce tamamlandı emin**, aksi takdirde, kısmi veri kopyalanır.

### <a name="run-delta-extraction-the-first-time"></a>Delta ayıklama ilk kez çalıştırma

Teknik olarak ilk Delta ayıklama olduğu bir **tam ayıklama**. Not varsayılan ADF açık Hub SAP BW Bağlayıcısı tarafından veri kopyalarken, son istek hariç tutar. Oluncaya kadar sonraki değişim ayıklama ADF kopyalama etkinliği ilk kez oluşması durumunda hiçbir veri çıkartıldığından DTP ayrı bir istek kimliğiyle tabloda değişiklik verilerini oluşturur Bu senaryonun olmaması için iki olası yolu olsa da:

1. "Son istek ilk Delta ayıklama açmak için ilk kez Delta ayıklama başlatmadan önce ilk Delta DTP tamamlandığını emin olmanız gerekir, bu durumda dışarıda bırak" seçeneği devre dışı
2. Delta ayıklama aşağıda açıklandığı gibi yeniden eşitleme için yordamı kullanın.

### <a name="re-sync-delta-extraction"></a>Delta ayıklama yeniden eşitleyin

Delta DTP tarafından kabul edilmez ancak SAP BW küplerini veri birkaç senaryo vardır:

- SAP BW seçmeli silme işlemi (satırlar) herhangi bir filtre koşulu kullanma
- SAP BW isteği silinmesini (Hatalı istek)

SAP açık Hub hedef (içindeki tüm SAP BW desteği paketleri 2015 yılı itibaren) bir veri reyonu denetimindeki verileri hedefi değil. Bu nedenle, OHD verileri değiştirmeden bir küp verilerini silmek mümkündür. Bu durumda, küpün veri ADF verilerle aşağıdaki adımları uygulayarak yeniden eşitlemeniz gerekir:

1. (SAP içinde tam bir DTP kullanarak) tam ayıklama ADF içinde çalıştırın
2. Açık Hub tablodaki tüm satırlar için Delta DTP Sil
3. Delta DTP durumunu ayarlamak için alındı

Bundan sonra izleyen Delta DTPs ve ADF Delta Ayıklamalar düzgün beklendiği gibi çalışır.

Aşağıdaki seçeneği kullanılarak el ile Delta DTP çalıştırarak alındı için Delta DTP durumunu ayarlayabilirsiniz: "*; Veri aktarımı Kaynak delta durumu: Getirilen*".

## <a name="next-steps"></a>Sonraki adımlar

SAP BW Open Hub connector desteği hakkında bilgi edinmek için şu makaleye geçin: 

> [!div class="nextstepaction"]
>[SAP Business Warehouse açık Hub Bağlayıcısı](connector-sap-business-warehouse-open-hub.md)
