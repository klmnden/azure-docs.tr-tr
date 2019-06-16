---
title: Azure veri Gezgini'nde Sisense kullanarak verileri Görselleştir
description: Bu makalede, Azure Veri Gezgini'ni veri kaynağı olarak Sisense için ayarlama ve veri görselleştirme hakkında bilgi edinin.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 5/29/2019
ms.openlocfilehash: f0840b90e1036c23fa58d94515bfeb035299c07f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66358191"
---
# <a name="visualize-data-from-azure-data-explorer-in-sisense"></a>Azure veri Gezgini'nde Sisense verileri görselleştirin

Sisense yüksek oranda etkileşimli kullanıcı deneyimleri sunan analytics uygulamalar oluşturmanıza olanak sağlayan bir analiz iş zekası platformudur. İş Zekası ve raporlama yazılım Pano erişme ve verileri birkaç tıklamayla birleştirmek izin verir. Yapılandırılmış ve yapılandırılmamış veri kaynağına bağlanın, en az bir betik oluşturma ve kodlama ile birden çok kaynaktan tabloları birleştirme ve etkileşimli web panolar ve raporlar oluşturabilirsiniz. Bu makalede, Azure Veri Gezgini'ni veri kaynağı olarak Sisense için ayarlama ve örnek Küme verilerini görselleştirmek öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede tamamlamak için şunlara ihtiyacınız vardır:

* [İndirme ve yükleme Sisense uygulama](https://documentation.sisense.com/latest/getting-started/download-install.htm) 

* Bir küme oluşturup StormEvents örnek verileri içeren veritabanı. Daha fazla bilgi için [hızlı başlangıç: Bir Azure Veri Gezgini kümesi ile veritabanı oluşturma](create-cluster-database-portal.md) ve [örnek verileri Azure veri Gezgini'ne alma](ingest-sample-data.md).

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

## <a name="connect-to-sisense-dashboards-using-azure-data-explorer-jdbc-connector"></a>Azure Veri Gezgini JDBC Bağlayıcısı'nı kullanarak Sisense panolara bağlanın

1. İndirmek ve kopyalamak için aşağıdaki jar dosyalarının en son sürümleri *... \Sisense\DataConnectors\jdbcdrivers\adx* 

    * 1\.1.jar etkinleştirme
    * adal4j 1.6.0.jar
    * Commons codec 1.10.jar
    * Commons collections4 4.1.jar
    * commons-lang3-3.5.jar
    * gson 2.8.0.jar
    * Ek açıklamalar 1.0 1.jar jcip
    * JSON akıllı 1.3.1.jar
    * lang-tag-1.4.4.jar
    * posta 1.4.7.jar
    * mssql-jdbc-7.2.1.jre8.jar
    * nimbus-jose-jwt-7.0.1.jar
    * oıdc sdk 5.24.1.jar oauth2
    * slf4j API 1.7.21.jar
    
1. Açık **Sisense** uygulama.
1. Seçin **veri** sekmenize **+ ElastiCube** yeni bir ElastiCube modeli oluşturun.
    
    ![ElastiCube seçin](media/sisense/data-select-elasticube.png)

1. İçinde **yeni ElastiCube Model ekleme**, ElastiCube modeli adlandırın ve **Kaydet**.
   
    ![Yeni ElastiCube modeli ekleme](media/sisense/add-new-elasticube-model.png)

1. Seçin **+ veri**.

    ![Veri düğmeyi seçin](media/sisense/select-data.png)

1. İçinde **seçin bağlayıcı** sekmesinin **genel JDBC** bağlayıcı.

    ![JDBC bağlayıcısını seçme](media/sisense/select-connector.png)

1. İçinde **Connect** sekmesinde, aşağıdaki alanlar için tam **genel JDBC** Bağlayıcısı ve select **sonraki**.

    ![JDBC bağlayıcı ayarları](media/sisense/jdbc-connector.png)

    |Alan |Açıklama |
    |---------|---------|
    |Bağlantı Dizesi     |   `jdbc:sqlserver://<cluster_name.region>.kusto.windows.net:1433;database=<database_name>;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.kusto.windows.net;loginTimeout=30;authentication=ActiveDirectoryPassword`      |
    |JDBC Jar'lar klasörü  |    `..\Sisense\DataConnectors\jdbcdrivers\adx`     |
    |Sürücü sınıfı adı    |   `com.microsoft.sqlserver.jdbc.SQLServerDriver`      |
    |Kullanıcı adı   |    AAD kullanıcı adı     |
    |Parola     |   AAD kullanıcı parolası      |

1. İçinde **veri Seç** sekmesinde, arama **Veritabanı Seç** izinleri sahibi ilgili veritabanını seçebilirsiniz. Bu örnekte seçin *test1*.

    ![veritabanı seçin](media/sisense/select-database.png)

1. İçinde *test* (veritabanı adı) bölmesi:
    1. Tablo Önizleme ve tablo sütun adlarını görmek için tablo adını seçin. Gereksiz sütunları kaldırabilirsiniz.
    1. Bu tabloyu tablonun ilgili onay kutusunu seçin. 
    1. **Done** (Bitti) öğesini seçin.

    ![Tablo seçin](media/sisense/select-table-see-columns.png)    

1. Seçin **derleme** kümenizi oluşturmak için. 

    * İçinde **derleme** penceresinde **yapı**.

      ![Pencere oluşturma](media/sisense/build-window.png)

    * Tam ve ardından yapı işlemi tamamlanana kadar bekleyin **derleme başarılı**.

      ![Derleme başarılı](media/sisense/build-succeeded.png)

## <a name="create-sisense-dashboards"></a>Sisense panolar oluşturun

1. İçinde **Analytics** sekmesinde **+**  >  **yeni Pano** bu veri kümesinde panoları oluşturmak için.

    ![Yeni pano](media/sisense/new-dashboard.png)

1. Bir Pano çeker ve **Oluştur**. 

    ![Pano oluşturma](media/sisense/create-dashboard.png)

1. Altında **yeni pencere öğesi**seçin **+ seçin veri** yeni bir pencere öğesi oluşturmak için. 

    ![Alanları StormEvents panoya ekleme](media/sisense/storm-dashboard-add-field.png)  

1. Seçin **+ fazla veri ekleme** grafa ek sütunlar eklemek. 

    ![Grafiğe daha fazla veri ekleme](media/sisense/add-more-data.png)

1. Seçin **+ pencere öğesi** başka bir pencere öğesi oluşturmak için. Panonuz yeniden düzenlemek için pencere öğelerini sürükleyip yeniden açın.

    ![Storm panosu](media/sisense/final-dashboard.png)

Artık görsel analiz ile verilerinizi keşfedin, ek panolar oluşturma ve verileri, iş etkisi yapmak için eyleme geçirilebilen öngörülere dönüştürün.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini için sorgu yazma](write-queries.md)

