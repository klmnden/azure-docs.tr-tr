---
title: Azure IOT Central uygulamanız için özel analizler oluşturun | Microsoft Docs
description: İşleç, Azure IOT Central uygulamanız için özel analytics oluşturma.
author: lmasieri
ms.author: lmasieri
ms.date: 08/26/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 0a78c534605b6eab08d5b12674689f0459e80b26
ms.sourcegitcommit: 2b2129fa6413230cf35ac18ff386d40d1e8d0677
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43247105"
---
# <a name="how-to-use-analytics-to-analyze-your-device-data"></a>Cihazınızın verileri analiz etmek için Analytics kullanma

Microsoft Azure IOT Central, büyük miktarda verileri anlamlı zengin analiz özellikleri sağlar. Başlamak için ziyaret **Analytics** sol gezinti menüsünde. 

  ![Analytics IOT Central gitme](media\howto-create-analytics\analytics-navigation.png)

## <a name="querying-your-data"></a>Verilerinizi sorgulama

Seçmeniz gerekir bir **cihaz kümesi**, ekleme bir **filtre** (isteğe bağlı) seçip bir **süre** kullanmaya başlamak için. İşiniz bittiğinde tıklayarak *sonuçları göster* verilerinizi görselleştirme başlatmak için.


* **Cihaz kümeleri:** A [cihaz kümesi](howto-use-device-sets.md) cihazlarınızı, kullanıcı tanımlı grubudur. Örneğin, tüm Refrigerators Oakland içinde ya da tüm 2.0 Rüzgar turbines rev.

<!---
to-do: confirm if 10 is the max number of filters
to-do: do we need to explain how fiters work?
--->

* **Filtreler:** isteğe bağlı olarak, verileriniz üzerinde odaklanmanıza imkan aramanıza filtre ekleyebilirsiniz. Aynı anda en fazla 10 filtreleri ekleyebilirsiniz. Örneğin, sıcaklık çalıştırılmış olan 60 derecenin Git Oakland içindeki tüm Refrigerators içinde bulun. 
* **Zaman aralığı:** varsayılan olarak veri son 10 dakika ile alıyoruz. Önceden tanımlanmış saat aralıklardan biri için bu değeri değiştirin veya bir özel zaman aralığı seçin. 

 ![Analiz sorgusu](media\howto-create-analytics\analytics-query.png)

## <a name="visualizing-your-data"></a>Verilerinizi Görselleştirme

Verilerinizi sorgulanan sonra bu görselleştirme başlatmak mümkün olacaktır. Toplanmış ve daha fazla veri biçimini değiştirme verileri farklı cihaz özelliklerine göre bölmek, Göster/ölçümleri gizle.  

* **Bölme ölçütü:** veri cihaz özelliklerine göre bölme sayesinde daha fazla Detaya Gitmeyi aşağı verilerinizi. Örneğin, sonuçlarınızı cihaz kimliği veya konuma göre bölebilirsiniz.
<!---
to-do: confirm if 10 is the max number of measurements
--->
* **Ölçümler:** teker teker, cihazlarınız tarafından bildirilen en fazla 10 farklı telemetri öğeleri Göster/Gizle seçebilirsiniz. Ölçümler, sıcaklık ve nem gibi noktalardır. 
* **Toplama:** varsayılan olarak şu veri, ortalama ile toplama, ancak veri toplamayı şeye kendi gereksinimlerinize uyacak şekilde değiştirmek isteyebilirsiniz. 

   ![Analytics Görselleştirme](media\howto-create-analytics\analytics-visualize.png) <br/><br/>
   ![Bölme ölçütü analytics Görselleştirme](media\howto-create-analytics\analytics-splitby.png)

## <a name="interacting-with-your-data"></a>Verilerinizle etkileşim kurma

Sahip olduğunuz çeşitli şekillerde daha fazla sorgu sonuçlarınız görselleştirme gereksinimlerinizi karşılayacak şekilde değiştirebilirsiniz. Bir graf görünümünden ve ızgara görünümü arasında geçiş, daraltma/genişletme yakınlaştırma, Veri kümenizi yenileyin ve satırları nasıl gösterildiğini alter.

* **Kılavuzu Göster:** sonuçlarınızı tablo biçimi, her veri noktası için belirli bir değeri görüntülemek etkinleştirmek için kullanıma sunulacak. Bu görünüm, ayrıca erişilebilirlik standartlarını karşılar. 
* **Grafiğin göster:** sonuçlarınızı bir kolayca noktaları yukarı ve aşağı doğru eğilimleri ve anormallikleri satır biçiminde görüntülenir. 

 ![Analiz için kılavuz görünümü gösteriliyor](media\howto-create-analytics\analytics-showgrid.png)

Yakınlaştırma, verileriniz üzerinde odaklanmanıza imkan sağlar. Bir zaman dönemi bulursanız, sonuç kümesinde odaklanmak yakınlaştırmak ve aşağıdaki eylemlerden birini gerçekleştirmek için kullanılabilir denetimleri kullanmak istediğiniz alanı, imleç tutamaçları kullanarak ister misiniz:
* **Yakınlaştırma:** bir zaman dönemi seçtikten sonra yakınlaştırma, etkin ve verileriniz için yakınlaştırın olanak sağlar.
* **Uzaklaştır:** bu denetim, yakınlaştırma, son yakınlaştırma bir düzeyi sağlar. Örneğin, üç kez verilerinize olduğunuz ekranı yakınlaştırdığınızda, uzaklaştırma sürecek bir kerede tek bir adımda geri.
* **Yakınlaştırma sıfırlanması:** çeşitli yakınlaştırma düzeyleri gerçekleştirdiğiniz sonra özgün Sonuç kümenizi döndürülecek yakınlaştırma sıfırlama denetimini kullanabilirsiniz. 

 ![Verileriniz üzerinde yakınlaştırma gerçekleştirin](media\howto-create-analytics\analytics-zoom.png)


Çizgi stili gereksinimlerinizi karşılayacak şekilde değiştirebilirsiniz. Aralarından seçim yapabileceğiniz dört seçeneğiniz vardır:
* **Satır:** her veri noktası arasında düz bir çizgi biçimlendirilmiş olmalıdır. 
* **Düzgün:** her nokta arasında bir eğri çizgi oluşturulmuş
* **Adım:** grafikteki her nokta arasındaki çizgi adım grafiği oluşturur
* **Dağılım:** bağlamayı satırları Grafikteki tüm noktaları çizilir. 

 ![Farklı satır türleri Analytics'te kullanılabilir](media\howto-create-analytics\analytics-linetypes.png)

Son olarak, üç moddan birini seçerek y ekseni verilerinizi düzenleyebilirsiniz:

* **Yığılmış:** her bir ölçüm grafiği, yığılmış ve grafiklerin her biri kendi y ekseni sahip. Yığılmış grafikler seçili birden çok ölçümleri sahip ve farklı bir görünüm Bu ölçümlerin istediğinizde yararlıdır.
* **Yığılmamış:** bir grafik her ölçü bir y ekseni karşı çizilme ancak y değerlerinin değiştirilmesi için vurgulanan ölçüyü temel alarak. Yığılmamış grafikleri, birden çok ölçü kaplama ve aynı zaman aralığı için bu ölçümleri arasında desenleri görmek istediğinizde yararlıdır.
* **Paylaşılan ekseni:** grafikleri aynı y ekseni paylaşın ve eksen için değerleri değiştirmeyin. Paylaşılan y ekseni grafikleri, bölünmüş tarafından verileri dilimleme sırasında tek bir ölçü aramak istediğinizde kullanışlıdır.

 ![Verilerinizi farklı görselleştirme modları y ekseni düzenleyin](media\howto-create-analytics\analytics-yaxis.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Central uygulamanız için özel analytics oluşturulacağını öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Hazırlama ve bir Node.js uygulaması bağlanın](howto-connect-nodejs.md)