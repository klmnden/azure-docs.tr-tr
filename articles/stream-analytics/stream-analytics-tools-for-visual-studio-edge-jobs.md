---
title: Edge işleri, Visual Studio için Azure Stream Analytics araçları
description: Bu makalede, yazma, hata ayıklama ve Stream Analytics Edge işlerinizi Visual Studio için Stream Analytics araçlarını kullanarak oluşturmak açıklar.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/13/2018
ms.openlocfilehash: 5dc90a1334b525c02be3eae2985900ab07cf2e05
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43696257"
---
# <a name="develop-stream-analytics-edge-jobs-using-visual-studio-tools"></a>Visual Studio Araçları'nı kullanarak Stream Analytics Edge işlerini geliştirme

Bu öğreticide, yazma, hata ayıklama ve Stream Analytics Edge işlerinizi oluşturmak için Visual Studio için Stream Analytics araçları kullanmayı öğrenin. Oluşturup test işi sonra cihazlara dağıtmak için Azure portalına gidebilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları ihtiyacınız vardır:

* Yükleme [Visual Studio 2017](https://www.visualstudio.com/downloads/), [Visual Studio 2015](https://www.visualstudio.com/vs/older-downloads/), veya [Visual Studio 2013 güncelleştirme 4](https://www.microsoft.com/download/details.aspx?id=45326). Enterprise (Ultimate/Premium), Professional ve Community sürümleri desteklenir. Express sürümü desteklenmez.  

* İzleyin [yükleme yönergeleri](stream-analytics-tools-for-visual-studio-edge-jobs.md) Visual Studio için Stream Analytics araçları yüklemek için.
 
## <a name="create-a-stream-analytics-edge-project"></a>Stream Analytics Edge proje oluşturma 

Visual Studio'dan seçin **dosya** > **yeni** > **proje**. Gidin **şablonları** soldaki listesi > genişletin **Azure Stream Analytics** > **Stream Analytics Edge**  >   **Azure Stream Analytics Edge uygulama**. Seçin ve proje için bir ad, konum ve çözüm adı sağlayın **Tamam**.

![Yeni uç projesi](./media/stream-analytics-tools-for-visual-studio-edge-jobs/new-edge-project.png)

Proje oluşturulur, sonra gidin **Çözüm Gezgini** klasör hiyerarşisini görüntülemek için.

![Çözüm Gezgini görünümü](./media/stream-analytics-tools-for-visual-studio-edge-jobs/edge-project-in-solution-explorer.png)

 
## <a name="choose-the-correct-subscription"></a>Doğru aboneliği seçin

1. Uygulamanızı Visual Studio'dan **görünümü** menüsünde **Sunucu Gezgini**.  

2. Sağ tıklayın **Azure** > seçin **Microsoft Azure aboneliğine bağlanma** > ve ardından Azure hesabınızla oturum açın.

## <a name="define-inputs"></a>Girişleri tanımlayın

1. Gelen **Çözüm Gezgini**, genişletme **girişleri** düğüm adlı bir giriş görmeniz **EdgeInput.json**. Ayarlarını görüntülemek için çift tıklayın.  

2. Kaynak türü olarak ayarlandığından emin olun **veri Stream** > kaynak ayarlandığında **Edge hub'ı** > olay serileştirme biçimi kümesine **Json** > ve kodlama için **UTF8**. İsteğe bağlı olarak adlandırabilirsiniz **giriş diğer adı**, şimdi bu örnekte olduğu gibi bırakın. Giriş diğer adı yeniden adlandır durumunda, sorgunun tanımlarken, belirtilen adı kullanın. Ayarları kaydetmek için **Kaydet**’i seçin.  
   ![Giriş yapılandırma](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-input-configuration.png)
 


## <a name="define-outputs"></a>Çıkışlar tanımlayın

1. Gelen **Çözüm Gezgini**, genişletme **çıkışları** düğüm adlı bir çıktı görmeniz **EdgeOutput.json**. Ayarlarını görüntülemek için çift tıklayın.  

2. Havuz seçmek için ayarlandığından emin olun **Edge hub'ı** > olay serileştirme biçimi kümesine **Json** > ve kodlama ayarlandığında **UTF8** > ve biçim ayarı  **Dizi**. İsteğe bağlı olarak adlandırabilirsiniz **çıkış diğer adı**, şimdi bu örnekte olduğu gibi bırakın. Çıkış diğer adı yeniden adlandır durumunda, sorgunun tanımlarken, belirtilen adı kullanın. Ayarları kaydetmek için **Kaydet**’i seçin. 
   ![Çıkış yapılandırması](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-output-configuration.png)
 
## <a name="define-the-transformation-query"></a>Dönüşüm sorgusunu tanımlama

Stream Analytics işleri Edge ortamlarında desteği çoğu [Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/azure/stream-analytics/reference/stream-analytics-query-language-reference?f=255&MSPPError=-2147217396), Edge işleri için aşağıdaki işlemleri henüz desteklenmemektedir ancak: 


|**Kategori**  | **Komut**  |
|---------|---------|
|Jeo-uzamsal işleçler |<ul><li>CreatePoint</li><li>CreatePolygon</li><li>CreateLineString</li><li>ST_DISTANCE</li><li>ST_WITHIN</li><li>ST_OVERLAPS</li><li>ST_INTERSECTS</li></ul> |
|Diğer işleçler | <ul><li>BÖLÜMÜ</li><li>ÜZERİNDEN ZAMAN DAMGASI</li><li>FARKLI</li><li>Expression parametresinde COUNT işleci</li><li>Tarih ve saat işlevleri'nde mikrosaniye ölçeğinde</li><li>JavaScript UDA'ın (Bu özellik, işleri bulutta dağıtılan için Önizleme aşamasında.)</li></ul>   |

Edge işi portalda oluşturduğunuzda, derleyici otomatik olarak desteklenen bir işleç kullanmıyorsanız uyarır.

Visual Studio'dan, sorgu Düzenleyicisi'nde aşağıdaki dönüşüm sorgusunu tanımlayın (**script.asaql dosya**)

```sql
SELECT * INTO EdgeOutput
FROM EdgeInput 
```

## <a name="test-the-job-locally"></a>İş yerel olarak test etme

Yerel olarak, sorguyu sınamak için örnek verileri yüklemeniz gerekir. Kayıt verileri yükleyerek örnek veri alabilirsiniz [GitHub deposu](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/Registration.json) ve yerel bilgisayarınıza kaydedin. 

1. Örnek verileri karşıya yükleme için > sağ tıklayarak **EdgeInput.json** Dosya > seçin **yerel giriş Ekle**  

2. Açılır pencerede > **Gözat** örnek verileri, yerel yolu > seçin **Kaydet**.
   ![Yerel giriş yapılandırma](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-local-input-configuration.png)
 
3. Adlı bir dosya **local_EdgeInput.json** girişleri klasörünüze otomatik olarak eklenir.  
4. yerel olarak çalıştırmak veya Azure'a gönderin. Sorguyu test edin > seçin **yerel olarak çalıştırma**.  
   ![Çalıştırma Seçenekleri](./media/stream-analytics-tools-for-visual-studio-edge-jobs/run-options.png)
 
5. Komut İstemi penceresini işinin durumunu gösterir. İş başarıyla çalıştırıldığında, proje klasörü yolu "Visual Studio 2015\Projects\MyASAEdgejob\MyASAEdgejob\ASALocalRun\2018-02-23-11-31-42" içinde "2018-02-23-11-31-42" gibi görünen bir klasör oluşturur. Yerel klasörde sonuçlarını görüntülemek için klasör yoluna gidin:

   Ayrıca, Azure portalında oturum açın ve iş oluşturulduğunu doğrulayın. 

   ![Sonuç klasörü](./media/stream-analytics-tools-for-visual-studio-edge-jobs/result-folder.png)

## <a name="submit-the-job-to-azure"></a>İşi azure'a Gönder

1. İşi azure'a göndermeden önce Azure aboneliğinize bağlanmanız gerekir. Açık **Sunucu Gezgini** > sağ tıklayarak **Azure** > **Microsoft Azure aboneliğine bağlanma** > Azure aboneliğinizde oturum açın.  

2. İşi azure'a göndermek için sorgu Düzenleyicisi gidin > seçin **azure'a Gönder**.  

3. Var olan bir Edge işi güncelleştirme veya yeni bir tane oluşturmak seçebileceğiniz bir pencere açılır. Var olan bir işi güncelleştirdiğinizde, bu senaryoda tüm iş yapılandırması, yerini alır, yeni bir iş yayımlar. Seçin **yeni bir Azure Stream Analytics işi oluşturma** > şuna benzer işiniz için bir ad girin **MyASAEdgeJob** > gerekli seçin **abonelik**, **Kaynak grubu**, ve **konumu** > seçin **gönderme**.

   ![Azure'a Gönder](./media/stream-analytics-tools-for-visual-studio-edge-jobs/submit-to-azure.png)
 
   Stream Analytics Edge işiniz oluşturulduktan sonra başvurabilirsiniz [IOT Edge öğretici işlerinizi](stream-analytics-edge.md) cihazlarınıza dağıtma hakkında bilgi edinmek için. 

## <a name="manage-the-job"></a>İşi Yönet 

İş ve iş diyagramı Sunucu Gezgini'nden durumunu görüntüleyebilirsiniz. Gelen **Sunucu Gezgini** > **Stream Analytics** > abonelik ve Edge işi dağıttığınız kaynak grubunu genişletin > durumundakiMyASAEdgejobgörüntüleyebilirsiniz. **Oluşturulan**. Proje düğümünü genişletin ve iş görünümünü açmak için çift tıklayın.

![Sunucu Gezgini seçenekleri](./media/stream-analytics-tools-for-visual-studio-edge-jobs/server-explorer-options.png)
 
İş Görünümü penceresini iş Azure portal vb. açma işini silme işi yenileme gibi işlemleri sağlar.

![İş diyagramı ve diğer seçenekleri](./media/stream-analytics-tools-for-visual-studio-edge-jobs/job-diagram-and-other-options.png) 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure IOT Edge hakkında daha fazla bilgi](../iot-edge/about-iot-edge.md)
* [Öğretici IOT Edge üzerinde ASA](../iot-edge/tutorial-deploy-stream-analytics.md)
* [Bu anketi kullanarak ekibine geri bildirim gönderin](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2czagZ-i_9Cg6NhAZlH9ypUMjNEM0RDVU9CVTBQWDdYTlk0UDNTTFdUTC4u) 
