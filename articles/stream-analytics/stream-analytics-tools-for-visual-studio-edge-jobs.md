---
title: Stream Analytics Veri kutusu Edge işleri, Visual Studio için Azure Stream Analytics araçları
description: Bu makalede, yazma, hata ayıklama ve Stream Analytics Veri kutusu Edge işlerinizi Visual Studio için Stream Analytics araçlarını kullanarak oluşturmak açıklar.
services: stream-analytics
author: su-jie
ms.author: sujie
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 242fb2daebfe9eb6e5a0c73c2c4c0e91a3131032
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66304152"
---
# <a name="develop-stream-analytics-data-box-edge-jobs-using-visual-studio-tools"></a>Visual Studio Araçları'nı kullanarak Stream Analytics Veri kutusu Edge işlerini geliştirme

Bu öğreticide, Visual Studio için Stream Analytics araçları kullanmayı öğrenin. Yazma, hata ayıklama ve Stream Analytics Veri kutusu Edge işlerinizi oluşturma konusunda bilgi edinin. Oluşturup test işi sonra cihazlara dağıtmak için Azure portalına gidebilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları ihtiyacınız vardır:

* Yükleme [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/), [Visual Studio 2015](https://www.visualstudio.com/vs/older-downloads/), veya [Visual Studio 2013 güncelleştirme 4](https://www.microsoft.com/download/details.aspx?id=45326). Enterprise (Ultimate/Premium), Professional ve Community sürümleri desteklenir. Express sürümü desteklenmez.  

* İzleyin [yükleme yönergeleri](stream-analytics-tools-for-visual-studio-edge-jobs.md) Visual Studio için Stream Analytics araçları yüklemek için.
 
## <a name="create-a-stream-analytics-data-box-edge-project"></a>Stream Analytics Veri kutusu Edge proje oluşturma 

Visual Studio'dan seçin **dosya** > **yeni** > **proje**. Gidin **şablonları** soldaki listesi > genişletin **Azure Stream Analytics** > **Stream Analytics Edge**  >   **Azure Stream Analytics Edge uygulama**. Seçin ve proje için bir ad, konum ve çözüm adı sağlayın **Tamam**.

![Yeni Stream Analytics Veri kutusu Edge proje Visual Studio'da](./media/stream-analytics-tools-for-visual-studio-edge-jobs/new-stream-analytics-edge-project.png)

Proje oluşturulur, sonra gidin **Çözüm Gezgini** klasör hiyerarşisini görüntülemek için.

![Çözüm Gezgini görünümü, Stream Analytics Veri kutusu Edge işi](./media/stream-analytics-tools-for-visual-studio-edge-jobs/edge-project-in-solution-explorer.png)

 
## <a name="choose-the-correct-subscription"></a>Doğru aboneliği seçin

1. Uygulamanızı Visual Studio'dan **görünümü** menüsünde **Sunucu Gezgini**.  

2. Sağ tıklayın **Azure** > seçin **Microsoft Azure aboneliğine bağlanma** > ve ardından Azure hesabınızla oturum açın.

## <a name="define-inputs"></a>Girişleri tanımlayın

1. Gelen **Çözüm Gezgini**, genişletme **girişleri** düğüm adlı bir giriş görmeniz **EdgeInput.json**. Ayarlarını görüntülemek için çift tıklayın.  

2. Kümesine kaynak türü **veri Stream**. Ardından kaynak kümesine **Edge hub'ı**, olay serileştirme biçimi için **Json**ve kodlamayı **UTF8**. İsteğe bağlı olarak adlandırabilirsiniz **giriş diğer adı**, şimdi bu örnekte olduğu gibi bırakın. Giriş diğer adı yeniden adlandır durumunda, sorgunun tanımlarken, belirtilen adı kullanın. Ayarları kaydetmek için **Kaydet**’i seçin.  
   ![Stream Analytics işi giriş yapılandırması](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-input-configuration.png)
 


## <a name="define-outputs"></a>Çıkışlar tanımlayın

1. Gelen **Çözüm Gezgini**, genişletme **çıkışları** düğüm adlı bir çıktı görmeniz **EdgeOutput.json**. Ayarlarını görüntülemek için çift tıklayın.  

2. Seçmek için havuz ayarladığınızdan emin olun **Edge hub'ı**, olay serileştirme biçimi kümesine **Json**ayarlayın kodlamasını **UTF8**ve biçimini ayarlayın **dizi**. İsteğe bağlı olarak adlandırabilirsiniz **çıkış diğer adı**, şimdi bu örnekte olduğu gibi bırakın. Çıkış diğer adı yeniden adlandır durumunda, sorgunun tanımlarken, belirtilen adı kullanın. Ayarları kaydetmek için **Kaydet**’i seçin. 
   ![Stream Analytics işi çıktı yapılandırma](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-output-configuration.png)
 
## <a name="define-the-transformation-query"></a>Dönüşüm sorgusunu tanımlama

Stream Analytics Veri kutusu Edge ortamlara dağıtılan Stream Analytics işleri desteği çoğu [Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/azure/stream-analytics/reference/stream-analytics-query-language-reference?f=255&MSPPError=-2147217396). Ancak, şu işlemler için Stream Analytics Veri kutusu Edge işleri henüz desteklenmez: 


|**Kategori**  | **Komutu**  |
|---------|---------|
|Jeo-uzamsal işleçler |<ul><li>CreatePoint</li><li>CreatePolygon</li><li>CreateLineString</li><li>ST_DISTANCE</li><li>ST_WITHIN</li><li>ST_OVERLAPS</li><li>ST_INTERSECTS</li></ul> |
|Diğer işleçler | <ul><li>BÖLÜMÜ</li><li>ÜZERİNDEN ZAMAN DAMGASI</li><li>DISTINCT</li><li>Expression parametresinde COUNT işleci</li><li>Tarih ve saat işlevleri'nde mikrosaniye ölçeğinde</li><li>JavaScript UDA'ın (Bu özellik, işleri bulutta dağıtılan için Önizleme aşamasında.)</li></ul>   |

Portalda bir Stream Analytics Veri kutusu Edge işi oluşturduğunuzda, derleyici otomatik olarak desteklenen bir işleç kullanmıyorsanız, sizi uyarır.

Visual Studio'dan, sorgu Düzenleyicisi'nde aşağıdaki dönüşüm sorgusunu tanımlayın (**script.asaql dosya**)

```sql
SELECT * INTO EdgeOutput
FROM EdgeInput 
```

## <a name="test-the-job-locally"></a>İş yerel olarak test etme

Yerel olarak, sorguyu sınamak için örnek verileri yüklemeniz gerekir. Kayıt verileri yükleyerek örnek veri alabilirsiniz [GitHub deposu](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/Registration.json) ve yerel bilgisayarınıza kaydedin. 

1. Örnek verileri karşıya yüklemek için sağ tıklayın **EdgeInput.json** seçin ve dosya **yerel giriş Ekle**  

2. Açılır pencerede > **Gözat** örnek verileri, yerel yolu > seçin **Kaydet**.
   ![Visual Studio'da yerel giriş yapılandırma](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-local-input-configuration.png)
 
3. Adlı bir dosya **local_EdgeInput.json** girişleri klasörünüze otomatik olarak eklenir.  
4. yerel olarak çalıştırmak veya Azure'a gönderin. Sorguyu test etmek için seçin **yerel olarak çalıştırma**.  
   ![Stream Analytics işi Visual Studio'da Çalıştırma Seçenekleri](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-visual-stuidio-run-options.png)
 
5. Komut İstemi penceresini işinin durumunu gösterir. İş başarıyla çalıştırıldığında, proje klasörü yolu "Visual Studio 2015\Projects\MyASAEdgejob\MyASAEdgejob\ASALocalRun\2018-02-23-11-31-42" içinde "2018-02-23-11-31-42" gibi görünen bir klasör oluşturur. Yerel klasörde sonuçlarını görüntülemek için klasör yoluna gidin:

   Ayrıca, Azure portalında oturum açın ve iş oluşturulduğunu doğrulayın. 

   ![Stream Analytics işi sonuç klasörü](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-job-result-folder.png)

## <a name="submit-the-job-to-azure"></a>İşi azure'a Gönder

1. İşi azure'a göndermeden önce Azure aboneliğinize bağlanmanız gerekir. Açık **Sunucu Gezgini** > sağ tıklayarak **Azure** > **Microsoft Azure aboneliğine bağlanma** > Azure aboneliğinizde oturum açın.  

2. İşi azure'a göndermek için sorgu Düzenleyicisi gidin > seçin **azure'a Gönder**.  

3. Bir pencere açılır. Var olan bir Stream Analytics Veri kutusu Edge işi güncelleştirme veya yeni bir tane oluşturmak bu seçeneği seçin. Var olan bir işi güncelleştirdiğinizde, bu senaryoda tüm iş yapılandırması, yerini alır, yeni bir iş yayınlayacaksınız. Seçin **yeni bir Azure Stream Analytics işi oluşturma** > şuna benzer işiniz için bir ad girin **MyASAEdgeJob** > gerekli seçin **abonelik**, **Kaynak grubu**, ve **konumu** > seçin **gönderme**.

   ![Stream Analytics işi, Visual Studio'dan Azure'a gönderme](./media/stream-analytics-tools-for-visual-studio-edge-jobs/submit-stream-analytics-job-to-azure.png)
 
   Şimdi, Stream Analytics Veri kutusu Edge işi oluşturuldu. Başvurabilirsiniz [IOT Edge öğretici işlerinizi](stream-analytics-edge.md) cihazlarınıza dağıtmayı öğrenin. 

## <a name="manage-the-job"></a>İşi Yönet 

İş ve iş diyagramı Sunucu Gezgini'nden durumunu görüntüleyebilirsiniz. Gelen **Stream Analytics** içinde **Sunucu Gezgini**, abonelik ve Stream Analytics Veri kutusu Edge işi dağıttığınız kaynak grubunu genişletin. Durumundaki MyASAEdgejob görüntüleyebileceğiniz **oluşturulan**. Proje düğümünü genişletin ve iş görünümünü açmak için çift tıklayın.

![Sunucu Gezgini proje yönetimi seçenekleri](./media/stream-analytics-tools-for-visual-studio-edge-jobs/server-explorer-options.png)
 
İş Görünümü penceresini yenileme işi, işin silinmesi ve Azure Portalı'ndan iş açma gibi işlemler sağlar.

![İş diyagramı ve diğer Visual Studio seçenekleri](./media/stream-analytics-tools-for-visual-studio-edge-jobs/job-diagram-and-other-options.png) 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure IOT Edge hakkında daha fazla bilgi](../iot-edge/about-iot-edge.md)
* [Öğretici IOT Edge üzerinde ASA](../iot-edge/tutorial-deploy-stream-analytics.md)
* [Bu anketi kullanarak ekibine geri bildirim gönderin](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2czagZ-i_9Cg6NhAZlH9ypUMjNEM0RDVU9CVTBQWDdYTlk0UDNTTFdUTC4u) 
