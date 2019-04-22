---
title: Azure veri Gezgini'nde Grafana kullanarak verileri Görselleştir
description: Bu nasıl yapılır makalesinde, Azure Veri Gezgini, Grafana için veri kaynağı olarak ayarlayın ve ardından örnek Küme verilerini görselleştirmek öğrenin.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 12/05/2018
ms.openlocfilehash: 188cb310cfc13fe2fc41ba3e01deb01068c0184d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59048325"
---
# <a name="visualize-data-from-azure-data-explorer-in-grafana"></a>Grafana Azure veri Gezgini'nde verileri görselleştirin

Grafana, sorgu ve veri görselleştirme, ardından oluşturup görselleştirmelerinizi üzerinde temel panoları paylaşma sağlayan bir analiz platformudur. Grafana sağlayan bir Azure Veri Gezgini *eklentisi*, bağlanma ve verileri Azure Veri Gezgini görselleştirmenizi sağlar. Bu makalede, Azure Veri Gezgini, Grafana için veri kaynağı olarak ayarlayın ve ardından örnek Küme verilerini görselleştirmek öğrenin.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl tamamlamak için şunlara ihtiyacınız vardır:

* [Grafana sürüm 5.3.0 veya sonraki](https://docs.grafana.org/installation/) işletim sisteminiz için

* [Azure Veri Gezgini eklentisini](https://grafana.com/plugins/grafana-azure-data-explorer-datasource/installation) Grafana için

* StormEvents örnek veriler içeren bir kümesi. Daha fazla bilgi için [hızlı başlangıç: Bir Azure Veri Gezgini kümesi ile veritabanı oluşturma](create-cluster-database-portal.md) ve [örnek verileri Azure veri Gezgini'ne alma](ingest-sample-data.md).

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

## <a name="configure-the-data-source"></a>Veri kaynağını yapılandırma

Azure Veri Gezgini, Grafana için veri kaynağı olarak yapılandırmak için aşağıdaki adımları gerçekleştirin. Şu adımları Bu bölümde daha ayrıntılı konulara değineceğiz:

1. Bir Azure Active Directory (Azure AD), hizmet sorumlusu oluşturun. Hizmet sorumlusu tarafından Grafana, Azure Veri Gezgini hizmete erişmek için kullanılır.

1. Azure AD hizmet sorumlusu ekleme *görüntüleyiciler* Azure Veri Gezgini veritabanı rolü.

1. Azure AD hizmet sorumlusu bilgilerini temel Grafana bağlantı özelliklerini belirleyin ve ardından bağlantıyı test edin.

### <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Hizmet sorumlusu oluşturabilirsiniz [Azure portalında](#azure-portal) veya bu adı kullanıyor [Azure CLI](#azure-cli) komut satırı deneyimi. Hangi yöntemin ne olursa olsun, sonraki adımlarda kullanacağınız dört bağlantı özellikleri için değerleri almak oluşturulduktan sonra kullandığınız.

#### <a name="azure-portal"></a>Azure portal

1. Hizmet sorumlusu oluşturmak için yönergeleri izleyin. [Azure portal belgeleri](/azure/active-directory/develop/howto-create-service-principal-portal).

    1. İçinde [uygulamanızı bir role atama](/azure/active-directory/develop/howto-create-service-principal-portal#assign-the-application-to-a-role) bölümünde, bir rol atamak **okuyucu** Azure Veri Gezgini kümenize.

    1. İçinde [oturum açma için değerleri alma](/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) bölümünde, adımları ele üç özellik değerleri kopyalayın: **Dizin kimliği** (Kiracı kimliği), **uygulama kimliği**, ve **parola**.

1. Azure portalında **abonelikleri** sonra hizmet sorumlusu oluşturduğunuz abonelik kimliği kopyalayın.

    ![Abonelik kimliği - portal](media/grafana/subscription-id-portal.png)

#### <a name="azure-cli"></a>Azure CLI

1. Bir hizmet sorumlusu oluşturun. Uygun bir kapsamı ve rol türü `reader`.

    ```azurecli
    az ad sp create-for-rbac --name "https://{UrlToYourGrafana}:{PortNumber}" --role "reader" \
                             --scopes /subscriptions/{SubID}/resourceGroups/{ResourceGroupName}
    ```

    Daha fazla bilgi için [Azure, Azure CLI ile hizmet sorumlusu oluşturma](/cli/azure/create-an-azure-service-principal-azure-cli).

1. Komut aşağıdaki gibi ayarlayın bir sonuç döndürür. Üç özellik değerleri kopyalayın: **AppID**, **parola**, ve **Kiracı**.

    ```json
    {
      "appId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
      "displayName": "{UrlToYourGrafana}:{PortNumber}",
      "name": "https://{UrlToYourGrafana}:{PortNumber}",
      "password": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
      "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
    }
    ```

1. Aboneliklerinizin listesini alın.

    ```azurecli
    az account list --output table
    ```

    Uygun bir abonelik kimliği kopyalayın.

    ![Abonelik kimliği - CLI](media/grafana/subscription-id-cli.png)

### <a name="add-the-service-principal-to-the-viewers-role"></a>Görüntüleyiciler rolüne hizmet sorumlusu ekleme

Hizmet sorumlusuna sahip olduğunuza göre ona ekleme *görüntüleyiciler* Azure Veri Gezgini veritabanı rolü. Bu görevi altında gerçekleştirebilirsiniz **izinleri** Azure portalında veya altında **sorgu** yönetim komutunu kullanarak.

#### <a name="azure-portal---permissions"></a>Azure portalı - izinleri

1. Azure portalında Veri Gezgini Azure kümenize gidin.

1. İçinde **genel bakış** bölümünde, StormEvents örnek verilerle bir veritabanı seçin.

    ![veritabanı seçin](media/grafana/select-database.png)

1. Seçin **izinleri** ardından **ekleme**.

    ![Veritabanı izinleri](media/grafana/database-permissions.png)

1. Altında **veritabanı izinleri eklemek**seçin **Görüntüleyicisi** rolündeki **seçin sorumluları**.

    ![Veritabanı izinleri ekleme](media/grafana/add-permission.png)

1. Oluşturduğunuz hizmet sorumlusu arayın (sorumlu örnekte gösterildiği **mb grafana**). Asıl seçip **seçin**.

    ![Azure portalında izinleri Yönet](media/grafana/new-principals.png)

1. **Kaydet**’i seçin.

    ![Azure portalında izinleri Yönet](media/grafana/save-permission.png)

#### <a name="management-command---query"></a>Yönetim command - sorgu

1. Azure portalında Veri Gezgini Azure kümenize gidip seçin **sorgu**.

    ![Sorgu](media/grafana/query.png)

1. Sorgu penceresinde aşağıdaki komutu çalıştırın. Uygulama Kimliğini kullanın ve Azure portalı ya da CLI Kiracı kimliği.

    ```kusto
    .add database {TestDatabase} viewers ('aadapp={ApplicationID};{TenantID}')
    ```

    Komut aşağıdaki gibi ayarlayın bir sonuç döndürür. Bu örnekte, ilk satır veritabanında varolan bir kullanıcı içindir ve yeni eklenen hizmet sorumlusu ikinci satırdır.

    ![Sonuç kümesi](media/grafana/result-set.png)

### <a name="specify-properties-and-test-the-connection"></a>Özellikleri belirtin ve bağlantıyı test edin

Atanan hizmet sorumlusu ile *görüntüleyiciler* artık rol özellikleri Grafana Örneğinizde belirtin ve Azure Veri Gezgini için bağlantıyı sınayın.

1. Grafana sol taraftaki menüde, dişli simgesini seçip **veri kaynakları**.

    ![Veri kaynakları](media/grafana/data-sources.png)

1. Seçin **veri kaynağı Ekle**.

1. Üzerinde **veri kaynakları / yeni** sayfasında, veri kaynağı için bir ad girin ve ardından türü seçin **Azure Veri Gezgini Datasource**.

    ![Bağlantı adı ve türü](media/grafana/connection-name-type.png)

1. Kümenizin adını form https://{ClusterName} girin. {Region}. kusto.windows.net. Azure portal veya CLI diğer değerleri girin. Aşağıdaki resimde bir eşleme için aşağıdaki tabloya bakın.

    ![Bağlantı özellikleri](media/grafana/connection-properties.png)

    | Grafana UI | Azure portal | Azure CLI |
    | --- | --- | --- |
    | Abonelik Kimliği | ABONELİK KİMLİĞİ | SubscriptionId |
    | Kiracı Kimliği | Dizin Kimliği | kiracı |
    | İstemci Kimliği | Uygulama Kimliği | appId |
    | Gizli anahtar | Parola | password |
    | | | |

1. Seçin **Kaydet ve Test**.

    Test başarılı olursa, sonraki bölüme gidin. Herhangi bir sorunla karşılaşırsanız Grafana ve gözden geçirme önceki adımlarda belirtilen değerleri kontrol edin.

## <a name="visualize-data"></a>Verileri görselleştirme

Azure Veri Gezgini, Grafana için veri kaynağı olarak yapılandırma bitirdikten sonra artık, verileri görselleştirme zamanı gelmiş demektir. Basit bir örneği aşağıda göstereceğiz ancak yapabileceğiniz çok daha fazla. Bakarak öneririz [Azure Veri Gezgini için sorgu yazma](write-queries.md) karşı örnek veri kümesini çalıştırmak için diğer sorgularının örnekleri için.

1. Grafana sol taraftaki menüde artı simgesini seçip **Pano**.

    ![Pano oluşturma](media/grafana/create-dashboard.png)

1. Altında **Ekle** sekmesinde **Graph**.

    ![Graf Ekle](media/grafana/add-graph.png)

1. Graf panelinde seçin **bölmesi başlığı** ardından **Düzenle**.

    ![Düzen paneli](media/grafana/edit-panel.png)

1. Bölmenin en altında seçin **veri kaynağı** ardından, yapılandırdığınız veri kaynağını seçin.

    ![Veri kaynağı seçme](media/grafana/select-data-source.png)

1. Aşağıdaki sorguda sorgu bölmesinde Kopyala ardından seçin **çalıştırma**. Sorgu olayların sayısı, örnek veri kümesi için günlük demetlerine.

    ```kusto
    StormEvents
    | summarize event_count=count() by bin(StartTime, 1d)
    ```

    ![Sorgu çalıştırma](media/grafana/run-query.png)

1. Varsayılan olarak son altı saat verileri kapsamlı olduğundan, grafik herhangi bir sonuç göstermez. Üst menüden **son 6 saat**.

    ![Son altı saat](media/grafana/last-six-hours.png)

1. 2007'de, bizim StormEvents örnek veri kümesinde bulunan yıl kapsayan özel bir aralık belirtin. **Uygula**’yı seçin.

    ![Özel bir tarih aralığı](media/grafana/custom-date-range.png)

    Şimdi grafik, verileri güne göre kümelenmiş 2007'den gösterir.

    ![Tamamlanan grafiği](media/grafana/finished-graph.png)

1. Üst menüde Kaydet'i seçin simgesi: ![Kaydet simgesine](media/grafana/save-icon.png).

## <a name="next-steps"></a>Sonraki adımlar

[Azure Veri Gezgini için sorgu yazma](write-queries.md)

[Öğretici: Azure Power BI veri Gezgini'nde verileri görselleştirin](visualize-power-bi.md)