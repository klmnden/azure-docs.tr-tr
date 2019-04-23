---
title: Azure Data Factory kopyalama verileri Azure Veri Gezgini
description: Bu konu başlığında, Azure Data Factory kopyalama aracını kullanarak Azure veri Gezgini'ne (yükle) veri alma öğrenin.
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/15/2019
ms.openlocfilehash: 64856d53168a7676cf279da2d8675ce81e1985f7
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60005375"
---
# <a name="copy-data-to-azure-data-explorer-using-azure-data-factory"></a>Azure Veri Gezgini, Azure Data Factory kullanarak veri kopyalama 

Azure Veri Gezgini, büyük hacimli uygulamaları, Web siteleri ve IOT cihazları gibi birçok farklı kaynaktan akış verileri üzerinde gerçek zamanlı analiz için hızlı, tam olarak yönetilen bir veri analiz hizmetidir. Yinelemeli olarak verileri araştırmak ve düzenleri ve anormallikleri ürünlerini geliştirmek için müşteri deneyimlerini geliştirmek için tanımlamak cihazları izlemek ve işlemleri artırın. Yeni soruları keşfedin ve dakikalar içinde yanıt alın. Azure Data Factory, tam olarak yönetilen bulut tabanlı veri tümleştirme hizmetidir. Hizmet, mevcut sisteminizden Azure Veri Gezgini veritabanınızı verilerle doldurmak ve zamandan tasarruf için kullanabileceğiniz analiz çözümlerinizi oluştururken.

Azure Data Factory, verileri Azure veri Gezgini'ne yüklemek için aşağıdaki avantajları sunar:

* **Kolay ayarlama**: Hiçbir gerekli komut ile bir sezgisel beş adımı Sihirbazı.
* **Zengin veri deposu desteği**: Zengin bir şirket içi ve bulut tabanlı veri depoları için yerleşik destek. Tablo ayrıntılı bir listesi için bkz [desteklenen veri depoları](/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats).
* **Güvenli ve uyumlu**: Veriler, HTTPS veya ExpressRoute üzerinden aktarılır. Küresel hizmet varlığını verilerinizi coğrafi sınır hiçbir zaman ayrılmaz sağlar.
* **Yüksek performanslı**: Azure Veri Gezgini içinde 1-GB/sn veri yükleme hızını kadar. Ayrıntılar için bkz [kopyalama etkinliği performansı](/azure/data-factory/copy-activity-performance).

Bu makalede Data Factory-veri kopyalama aracını verileri Amazon S3'ten Azure veri Gezgini'ne yüklemek için nasıl kullanılacağını gösterir. Gibi diğer veri depolarından veri kopyalamak için benzer adımları izleyebilirsiniz [Azure Blob Depolama](/azure/data-factory/connector-azure-blob-storage), [Azure SQL veritabanı](/azure/data-factory/connector-azure-sql-database), [Azure SQL veri ambarı](/azure/data-factory/connector-azure-sql-data-warehouse), [Google BigQuery](/azure/data-factory/connector-google-bigquery),[Oracle](/azure/data-factory/connector-oracle), ve [dosya sistemi](/azure/data-factory/connector-file-system).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Bir Azure Veri Gezgini küme ve veritabanı](create-cluster-database-portal.md)
* Veri kaynağı.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Seçin **kaynak Oluştur** düğmesini (+) portalın sol üst köşesinin > **Analytics** > **Data Factory**.

   ![Yeni bir veri fabrikası oluşturma](media/data-factory-load-data/create-adf.png)

1. İçinde **yeni veri fabrikası** sayfasında, aşağıdaki alanlar için değerler sağlayın ve ardından **Oluştur**.

    ![Yeni veri fabrikası sayfası](media/data-factory-load-data/my-new-data-factory.png)

    **Ayar**  | **Alan açıklaması**
    |---|---|
    | **Ad** | Veri fabrikanızın genel olarak benzersiz bir ad girin. Hatasını alırsanız *"veri fabrikası adı \"LoadADXDemo\" kullanılamıyor"*, veri fabrikasının farklı bir ad girin. Data Factory yapıtlarının adlandırma kuralları için bkz: [Data Factory adlandırma kuralları](/azure/data-factory/naming-rules).|
    | **Abonelik** | Veri fabrikasının oluşturulacağı Azure aboneliğini seçin. |
    | **Kaynak Grubu** | Seçin **Yeni Oluştur** ve yeni bir kaynak grubu adını girin. Seçin **var olanı kullan**, mevcut bir kaynak grubu varsa. |
    | **Sürüm** | Seçin **V2** |
    | **Konum** | Veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depoları, diğer konumlara veya bölgelerde olabilir. |
    | | |

1. Bildirimleri oluşturma işlemini izlemek için araç çubuğundan seçin. Oluşturma işlemi tamamlandıktan sonra oluşturduğunuz veri fabrikasına gidin. **Data Factory** giriş sayfası açılır.

   ![Data factory giriş sayfası](media/data-factory-load-data/data-factory-home-page.png)

1. Seçin **yazar ve İzleyici** uygulamasını ayrı bir sekmede açmak için kutucuğa.

## <a name="load-data-into-azure-data-explorer"></a>Azure veri Gezgini'ne veri yükleme

Birçok türlerinden veri yüklenebilir [veri depoları](/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats) Azure Veri Gezgini içinde. Bu konuda yükleme verileri Amazon S3 ayrıntıları.

Azure Data Factory kullanarak Azure Veri Gezgini içinde veri yüklemenin iki yolu vardır:

* Azure Data Factory kullanıcı arabirimini - [ **Yazar** sekmesi](/azure/data-factory/quickstart-create-data-factory-portal#create-a-data-factory)
* [Azure Data Factory **veri kopyalama** aracı](/azure/data-factory/quickstart-create-data-factory-copy-data-tool) bu makalede kullanılan.

### <a name="copy-data-from-amazon-s3-source"></a>Amazon S3'ten veri kopyalama (kaynak)

1. İçinde **başlayalım** sayfasında **veri kopyalama** veri kopyalama aracını başlatmak için.

   ![Veri kopyalama aracının kutucuğu](media/data-factory-load-data/copy-data-tool-tile.png)

1. İçinde **özellikleri** sayfasında, belirtin **görev adı** seçip **sonraki**.

    ![Kaynak özelliklerini kopyalama](media/data-factory-load-data/copy-from-source.png)

1. İçinde **kaynak veri deposu** sayfasında **+ yeni bağlantı oluştur**.

    ![Kaynak veri bağlantısı oluşturma](media/data-factory-load-data/source-create-connection.png)

1. Seçin **Amazon S3**ve ardından **devam et**

    ![Yeni bağlı hizmet](media/data-factory-load-data/amazons3-select-new-linked-service.png)

1. İçinde **yeni bağlı hizmet (Amazon S3)** sayfasında, aşağıdaki adımları uygulayın:

    ![Amazon S3'bağlı hizmeti belirtin](media/data-factory-load-data/amazons3-new-linked-service-properties.png)

    * Belirtin **adı** , yeni bağlı hizmeti.
    * Seçin **tümleştirme çalışma zamanı aracılığıyla Bağlan** açılır listeden bir değer.
    * Belirtin **erişim anahtarı kimliği** değeri.
    * Belirtin **gizli erişim anahtarı** değeri.
    * Seçin **Bağlantıyı Sına** oluşturduğunuz bağlı hizmet bağlantısını test etmek için.
    * **Son**’u seçin.

1. İçinde **kaynak veri deposu** sayfasında, yeni AmazonS31 bağlantınızı görürsünüz. **İleri**’yi seçin.

   ![Kaynak veri deposu oluşturulan bağlantı](media/data-factory-load-data/source-data-store-created-connection.png)

1. İçinde **girdi dosyasını veya klasörünü seçin** sayfası:

    1. Klasör/kopyalamak istediğiniz dosyaya göz atın. Klasör/dosya seçin.
    1. Kopyalama davranışını gerektiği gibi seçin. Tutun **ikili kopya** seçeneği işaretli değil.
    1. **İleri**’yi seçin.

    ![Girdi dosyasını veya klasörünü seçin](media/data-factory-load-data/source-choose-input-file.png)

1. İçinde **dosya biçimleri ayarları** sayfa Seç'e tıklayın ve dosya için ilgili ayarları **sonraki**.

   ![Girdi dosyasını veya klasörünü seçin](media/data-factory-load-data/source-file-format-settings.png)

### <a name="copy-data-into-azure-data-explorer-destination"></a>Azure Veri Gezgini (hedef) içine veri kopyalama

Azure Veri Gezgini yeni bağlı hizmet, aşağıda belirtilen verileri Azure Veri Gezgini hedef tablosu (havuz) kopyalamak için oluşturulur.

1. İçinde **hedef veri deposuna** kullanabileceğiniz sayfasında, varolan bir veri bağlantısı depolamak veya tıklayarak yeni bir veri deposu belirtmek **+ yeni bağlantı oluştur**.

    ![Hedef veri deposu sayfası](media/data-factory-load-data/destination-create-connection.png)

1. İçinde **yeni bağlı hizmet** sayfasında **Azure Veri Gezgini**ve ardından **devam et**

    ![Azure Veri Gezgini - yeni bağlı hizmet seçin](media/data-factory-load-data/adx-select-new-linked-service.png)

1. İçinde **yeni bağlı hizmet (Azure Veri Gezgini)** sayfasında, aşağıdaki adımları uygulayın:

    ![Yeni bağlı hizmet ADX](media/data-factory-load-data/adx-new-linked-service.png)

   * Seçin **adı** Azure Veri Gezgini için bağlı hizmeti.
   * İçinde **hesap seçme yöntemi**: 
        * Seçin **Azure abonelik** radyo düğmesini seçin ve seçin, **Azure aboneliği** hesabı. Ardından, sizin **küme**. Not açılan kullanıcıya ait kümeleri listeleme olur.
        * Alternatif olarak, seçin **el ile girmek** radyo düğmesini ve girin, **uç nokta**.
    * Belirtin **Kiracı**.
    * Girin **hizmet sorumlusu kimliği**.
    * Seçin **hizmet sorumlusu anahtarı** düğmesine tıklayın ve girin **hizmet sorumlusu anahtarı**.
    * Seçin, **veritabanı** açılan menüden. Alternatif olarak, seçin **Düzenle** onay kutusunu ve veritabanı adınızı girin.
    * Seçin **Bağlantıyı Sına** oluşturduğunuz bağlı hizmet bağlantısını test etmek için. Yeşil bir onay işareti, kurulum için bağlanabildiğini **bağlantı başarılı** görünür.
    * Seçin **son** bağlı hizmeti oluşturma işlemini tamamlamak için.

    > [!NOTE]
    > Hizmet sorumlusu, Azure Veri Gezgini hizmete erişmek için Azure Data Factory tarafından kullanılır. Hizmet sorumlusu için [bir Azure Active Directory (Azure AD) hizmet sorumlusu oluşturma](/azure/azure-stack/azure-stack-create-service-principals#manage-service-principal-for-azure-ad). Kullanmayın **Azure anahtar kasası** yöntemi.

1. **Hedef veri deposuna** açılır. Azure Veri Gezgini, oluşturduğunuz veri bağlantısı kullanılabilir. Seçin **sonraki** bağlantıyı yapılandırmak için.

    ![Hedef veri deposuna ADX](media/data-factory-load-data/destination-data-store.png)

1. İçinde **Tablo eşleme**, hedef tablo adını ayarlayın ve seçin **sonraki**.

    ![Hedef veri kümesi Tablo eşleme](media/data-factory-load-data/destination-dataset-table-mapping.png)

1. İçinde **sütun eşlemesi** sayfası:
    * İlk eşleme göre ADF tarafından gerçekleştirilen [ADF şema eşleme](/azure/data-factory/copy-activity-schema-and-type-mapping)
        * Ayarlama **sütun eşlemelerini** Azure Data Factory hedef tablosu için. Varsayılan eşleme kaynağından ADF hedef tabloya görüntülenir.
        * Sütun eşleştirme tanımlamanız gerekmez sütunların seçimini kaldırın.
    * Bu tablo veri alınan ve Azure veri Gezgini'ne ikinci eşleme gerçekleşir. Eşleme göre gerçekleştirilir [CSV eşleme kurallarını](/azure/kusto/management/mappings#csv-mapping). Kaynak verileri CSV biçiminde başarısız olduysa, ADF tablo biçimine dönüştürülen veriler, bu nedenle, bu aşamada yalnızca ilgili eşleme CSV eşleme bir dikkat edin.
        * Altında **Azure Veri Gezgini (Kusto) havuz özellikleri** ilgili ekleme **alımı eşleme adı** (isteğe bağlı) bu nedenle, sütun eşleme kullanılabilir.
        * Varsa **alımı eşleme adı** belirtilmediyse, "by-name" eşleme sipariş tanımlanan **sütun eşlemelerini** bölümü oluşur. "By-name" Eşleme başarısız olursa, Azure Veri Gezgini (haritalar tarafından konumu varsayılan olarak) "sütuna göre konumu" sırada verilerin alımı çalışacaktır.
    * Seçin **İleri**

    ![Hedef veri kümesi sütun eşleme](media/data-factory-load-data/destination-dataset-column-mapping.png)

1. **Ayarlar** sayfasında:
    * İlgili ayarlamak **hataya dayanıklılık ayarları**.
    * **Performans ayarları**: Etkinleştirme hazırlama geçerli değildir. **Gelişmiş ayarlar** maliyet konuları içerir. Hiçbir özel gerekip gerekmediğini olduğu gibi bırakın.
    * **İleri**’yi seçin.

    ![Veri ayarlarını kopyalama](media/data-factory-load-data/copy-data-settings.png)

1. İçinde **özeti**, ayarları gözden geçirin ve seçin **sonraki**.

    ![Özet verileri kopyalama](media/data-factory-load-data/copy-data-summary.png)

1. İçinde **dağıtım sayfası**:
    * Seçin **İzleyici** geçmek **İzleyici** sekmesini ve (ilerleme durumunu, hataları ve veri akışı) işlem hattının durumunu görürsünüz.
    * Seçin **ardışık düzen** bağlı hizmetler, veri kümeleri ve işlem hatlarını düzenleyebilmek için.
    * Seçin **son** tam kopya veri görevi

    ![Dağıtım sayfası](media/data-factory-load-data/deployment.png)

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [Azure Veri Gezgini bağlayıcı](/azure/data-factory/connector-azure-data-explorer) Azure Data factory'de.

* Bağlı hizmetler, veri kümeleri ve işlem hatlarında düzenleme hakkında daha fazla bilgi [Data Factory kullanıcı arabirimini](/azure/data-factory/quickstart-create-data-factory-portal).

* Hakkında bilgi edinin [Azure Veri Gezgini sorguları](/azure/data-explorer/web-query-data) veri sorgulamak için.
