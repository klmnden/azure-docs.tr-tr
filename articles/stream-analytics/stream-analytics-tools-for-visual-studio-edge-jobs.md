---
title: Sürekli tümleştirme ve dağıtım işlemini ayarlamak için Stream Analytics Visual Studio araçları kullanın | Microsoft Docs
description: Stream Analytics geliştirme Öğreticisi yazmak, hata ayıklama ve akış analizi kenar işleri oluşturmak Visual Studio Araçları.
keywords: Visual studio, NuGet, DevOps, kenar işler, akış analizi
documentationcenter: ''
services: stream-analytics
author: su-jie
manager: ''
ms.assetid: ''
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/13/2018
ms.author: sujie
ms.openlocfilehash: c6e1d0693035ef343e20cee4b09f0669e089afee
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="develop-stream-analytics-edge-jobs-by-using-visual-studio-tools"></a>Visual Studio araçları kullanarak Stream Analytics kenar işleri geliştirin

Bu öğreticide, yazar, hata ayıklama ve akış analizi kenar işleriniz oluşturmak için Visual Studio için Stream Analytics araçlarını kullanmayı öğrenin. Oluşturma ve test işi sonra cihazlara dağıtmak için Azure portalına gidebilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşullar gerekir:

* Yükleme [Visual Studio 2017](https://www.visualstudio.com/downloads/), [Visual Studio 2015](https://www.visualstudio.com/vs/older-downloads/), veya [Visual Studio 2013 güncelleştirme 4](https://www.microsoft.com/download/details.aspx?id=45326). Enterprise (Ultimate/Premium), Professional ve topluluk sürümleri desteklenir. Express sürümü desteklenmiyor.  

* İzleyin [yükleme yönergeleri](stream-analytics-tools-for-visual-studio-edge-jobs.md) Visual Studio için Stream Analytics araçlarını yüklemek için.
 
## <a name="create-a-stream-analytics-edge-project"></a>Stream Analytics kenar projesi oluşturma 

Visual Studio'dan seçin **dosya** > **yeni** > **proje**. Gidin **şablonları** soldaki listesi > genişletin **Azure akış analizi** > **Stream Analytics kenar**  >   **Azure Stream Analytics kenar uygulama**. Adını, konumunu ve çözüm için bir proje ve select ad **Tamam**.

![Yeni sınır projesi](./media/stream-analytics-tools-for-visual-studio-edge-jobs/new-edge-project.png)

Proje oluşturulduğunu sonra gidin **Çözüm Gezgini** klasör hiyerarşisini görüntülemek için.

![Çözüm Gezgini görünümü](./media/stream-analytics-tools-for-visual-studio-edge-jobs/edge-project-in-solution-explorer.png)

 
## <a name="choose-the-correct-subscription"></a>Doğru abonelik seçin

1. Visual Studio'da **Görünüm** menüsünde, select **Sunucu Gezgini**.  

2. Sağ tıklayın **Azure** > seçin **Microsoft Azure aboneliğine bağlanma** > ve Azure hesabınızla oturum açın.

## <a name="define-inputs"></a>Girdi tanımlama

1. Gelen **Çözüm Gezgini**, genişletin **girişleri** düğümü adlı bir girdi görmeniz gerekir **EdgeInput.json**. Ayarlarını görüntülemek için çift tıklayın.  

2. Kaynak türü olarak ayarlandığından emin olun **veri akışı** > kaynak ayarlanmış **kenar Hub** > olayı seri hale getirme biçimi kümesine **Json** > ve kodlama içinayarlanmış**UTF8**. İsteğe bağlı olarak adlandırabilirsiniz **giriş diğer adı**, şimdi bu örnekte olduğu gibi bırakın. Giriş diğer adı yeniden adlandırmak durumda sorgu tanımlarken, belirtilen adı kullanın. Ayarları kaydetmek için **Kaydet**’i seçin.  
   ![Giriş yapılandırma](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-input-configuration.png)
 


## <a name="define-outputs"></a>Çıkış tanımlayın

1. Gelen **Çözüm Gezgini**, genişletin **çıkışları** düğümü adlı bir çıktı görmeniz gerekir **EdgeOutput.json**. Ayarlarını görüntülemek için çift tıklayın.  

2. Havuz seçmek için ayarlandığından emin olun **kenar Hub** > olayı seri hale getirme biçimi kümesine **Json** > ve kodlama ayarlanmış **UTF8** > ve biçim ayarlanmış  **Dizi**. İsteğe bağlı olarak adlandırabilirsiniz **çıkış diğer adları**, şimdi bu örnekte olduğu gibi bırakın. Çıkış diğer yeniden adlandırmak durumda sorgu tanımlarken, belirtilen adı kullanın. Ayarları kaydetmek için **Kaydet**’i seçin. 
   ![Çıktı yapılandırma](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-output-configuration.png)
 
## <a name="define-the-transformation-query"></a>Dönüştürme sorgusunu tanımlayın

Akış analizi işleri kenar ortamlarında destek çoğunu [Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/azure/stream-analytics/reference/stream-analytics-query-language-reference?f=255&MSPPError=-2147217396), ancak aşağıdaki işlemleri kenar işleri henüz desteklenmiyor: 


|**Kategori**  | **Komut**  |
|---------|---------|
|Jeo-uzamsal işleçleri |<ul><li>CreatePoint</li><li>CreatePolygon</li><li>CreateLineString</li><li>ST_DISTANCE</li><li>ST_WITHIN</li><li>ST_OVERLAPS</li><li>ST_INTERSECTS</li></ul> |
|Diğer işleçleri | <ul><li>BÖLÜM</li><li>ÜZERİNDEN ZAMAN DAMGASI</li><li>DISTINCT</li><li>COUNT işleci ifade parametresi</li><li>Tarih ve saat işlevleri milisaniyeye</li><li>JavaScript UDA'yı (hala bulutta dağıtılan işleri için Önizleme'de bu özellik kullanılabilir)</li></ul>   |

Portalda bir kenar işi oluşturduğunuzda, derleyici otomatik olarak desteklenen bir işleç kullanmıyorsanız sizi uyarır.

Visual Studio'dan sorgu Düzenleyicisi'nde aşağıdaki dönüştürme sorgu tanımlayın (**script.asaql dosya**)

```sql
SELECT * INTO EdgeOutput
FROM EdgeInput 
```

## <a name="test-the-job-locally"></a>Test işi yerel olarak

Yerel olarak sorguyu sınamak için örnek verileri yüklemeniz gerekir. Kayıt verileri yükleyerek örnek verileri alabilirsiniz [GitHub deposunu](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/Registration.json) ve yerel bilgisayarınıza kaydedin. 

1. Örnek verileri yüklemek için > sağ tıklayın **EdgeInput.json** Dosya > seçin **yerel giriş Ekle**  

2. Açılır pencerede > **Gözat** örnek verileri yerel yolu > seçin **kaydetmek**.
   ![Yerel giriş yapılandırma](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-local-input-configuration.png)
 
3. Adlı bir dosya **local_EdgeInput.json** girişleri klasörünüze otomatik olarak eklenir.  
4. yerel olarak çalıştırma veya Azure'a gönderin. Sorguyu sınamak için > seçin **yerel olarak çalıştır**.  
   ![Çalıştırma Seçenekleri](./media/stream-analytics-tools-for-visual-studio-edge-jobs/run-options.png)
 
5. Komut İstemi penceresini işinin durumunu gösterir. İş başarıyla çalıştırıldığında, proje klasör yolu "Visual Studio 2015\Projects\MyASAEdgejob\MyASAEdgejob\ASALocalRun\2018-02-23-11-31-42" "2018-02-23-11-31-42" gibi görünüyor bir klasör oluşturur. Yerel klasörde sonuçları görüntülemek için klasör yoluna gidin:

   Ayrıca, Azure portalında oturum açın ve iş oluşturulduğunu doğrulayın. 

   ![Sonuç klasörü](./media/stream-analytics-tools-for-visual-studio-edge-jobs/result-folder.png)

## <a name="submit-the-job-to-azure"></a>İşi olarak Azure'da Gönder

1. İşi olarak Azure'da göndermeden önce Azure aboneliğinizin bağlanmanız gerekir. Açık **Sunucu Gezgini** > sağ tıklayın **Azure** > **Microsoft Azure aboneliğine bağlanma** > Azure aboneliğinizde oturum açın.  

2. Azure işi göndermek için sorgu Düzenleyicisi gidin > seçin **Azure Gönder**.  

3. Var olan bir kenar işi güncelleştirmek veya yeni bir tane oluşturmak seçebileceğiniz bir açılır pencere açılır. Varolan bir projeyi güncelleştirdiğinizde, bu senaryoda tüm iş yapılandırması, yerini alır, yeni bir iş yayımlar. Seçin **yeni bir Azure Stream Analytics işi oluşturmak** > şuna benzer işiniz için bir ad girin **MyASAEdgeJob** > gerekli seçin **abonelik**, **Kaynak grubu**, ve **konumu** > seçin **gönderme**.

   ![Azure'a Gönder](./media/stream-analytics-tools-for-visual-studio-edge-jobs/submit-to-azure.png)
 
   Stream Analytics Edge işinizi oluşturulan artık başvurabilirsiniz [IOT kenar öğretici işleri çalıştırma](stream-analytics-edge.md) aygıtlarınıza dağıtma hakkında bilgi edinmek için. 

## <a name="manage-the-job"></a>İş yönetimi 

İş ve Sunucu Gezgini proje diyagramdan durumunu görüntüleyebilirsiniz. Gelen **Sunucu Gezgini** > **Stream Analytics** > abonelik ve kenar iş dağıttığınız kaynak grubunu genişletin > durumundakiMyASAEdgejobgörüntüleyebilirsiniz **Oluşturulan**. İş düğümünü genişletin ve iş görünümünde açmak için çift tıklayın.

![Sunucu Gezgini seçenekleri](./media/stream-analytics-tools-for-visual-studio-edge-jobs/server-explorer-options.png)
 
Proje Görünümü penceresi Azure portal vb. işi açma işini silme işi, yenileme gibi işlemleri sağlar.

![İş diyagramı ve diğer seçenekleri](./media/stream-analytics-tools-for-visual-studio-edge-jobs/job-diagram-and-other-options.png) 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure IOT kenar hakkında daha fazla bilgi](../iot-edge/how-iot-edge-works.md)
* [ASA IOT kenar öğretici hakkında](../iot-edge/tutorial-deploy-stream-analytics.md)
* [Bu Anketi kullanılarak ekibine geri bildirim gönderin](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2czagZ-i_9Cg6NhAZlH9ypUMjNEM0RDVU9CVTBQWDdYTlk0UDNTTFdUTC4u) 
