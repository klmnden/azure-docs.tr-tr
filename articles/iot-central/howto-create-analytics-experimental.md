---
title: Azure IOT Central uygulamanızdaki cihaz verilerinizi analiz edin | Microsoft Docs
description: Azure IOT Central uygulamanızdaki cihaz verilerinizi analiz edin.
author: lmasieri
ms.author: lmasieri
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 851a509421924a2ea6607020e7749c10dcbec01d
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55774204"
---
# <a name="how-to-use-analytics-to-analyze-your-device-data"></a>Cihazınızın verileri analiz etmek için Analytics kullanma


*Bu makale, işleçler, oluşturucular ve yöneticiler için geçerlidir.*


Azure IOT Central, büyük miktarda verileri anlamlı zengin analiz özellikleri sağlar. Başlamak için ziyaret **Analytics** sol gezinti menüsünde. 

## <a name="querying-your-data"></a>Verilerinizi sorgulama

Seçmeniz gerekir bir **cihaz kümesi**, ekleme bir **filtre** (isteğe bağlı) seçip bir **süre** kullanmaya başlamak için. İşiniz bittiğinde tıklayarak *sonuçları göster* verilerinizi görselleştirme başlatmak için.


* **Cihaz kümeleri:** A [cihaz kümesi](howto-use-device-sets-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) cihazlarınızı, kullanıcı tanımlı grubudur. Örneğin, tüm Refrigerators Oakland içinde ya da tüm 2.0 Rüzgar turbines rev.

<!---
to-do: confirm if 10 is the max number of filters
to-do: do we need to explain how fiters work?
--->

* **Filtreler:** İsteğe bağlı olarak, verileriniz üzerinde geliştirmeniz için arama filtreleri ekleyebilirsiniz. Aynı anda en fazla 10 filtreleri ekleyebilirsiniz. Örneğin, sıcaklık çalıştırılmış olan 60 derecenin Git Oakland içindeki tüm Refrigerators içinde bulun. 
* **Zaman aralığı:** Varsayılan olarak veri son 10 dakika ile alıyoruz. Önceden tanımlanmış saat aralıklardan biri için bu değeri değiştirin veya bir özel zaman aralığı seçin. 

 ![Analiz sorgusu](media/howto-create-analytics-experimental/analytics-query.png)

## <a name="visualizing-your-data"></a>Verilerinizi Görselleştirme

Verilerinizi sorgulanan sonra bu görselleştirme başlatmak mümkün olacaktır. Toplanmış ve daha fazla veri biçimini değiştirme verileri farklı cihaz özelliklerine göre bölmek, Göster/ölçümleri gizle.  

* **Bölme ölçütü:** Cihaz özellikleri tarafından veri bölme için daha fazla ayrıntıya aşağı verilerinizi sağlar. Örneğin, sonuçlarınızı cihaz kimliği veya konuma göre bölebilirsiniz.
<!---
to-do: confirm if 10 is the max number of measurements
--->
* **Ölçümleri:** Aynı anda, cihazlarınız tarafından bildirilen en fazla 10 farklı telemetri öğeleri Göster/Gizle seçebilirsiniz. Ölçümler, sıcaklık ve nem gibi noktalardır. 
* **Toplama:** Varsayılan olarak şu veri, ortalama ile toplama, ancak veri toplama şeye kendi gereksinimlerinize uyacak şekilde değiştirmek seçebilirsiniz. 

   ![Bölme ölçütü analytics Görselleştirme](media/howto-create-analytics-experimental/analytics-splitby.png)

## <a name="interacting-with-your-data"></a>Verilerinizle etkileşim kurma

Sahip olduğunuz çeşitli şekillerde daha fazla sorgu sonuçlarınız görselleştirme gereksinimlerinizi karşılayacak şekilde değiştirebilirsiniz. Bir graf görünümünden ve ızgara görünümü arasında geçiş, daraltma/genişletme yakınlaştırma, Veri kümenizi yenileyin ve satırları nasıl gösterildiğini alter.

* **Kılavuzu Göster:** Sonuçlarınız, tablo biçiminde, böylece her veri noktası için belirli bir değeri görüntülemek kullanılabilir. Bu görünüm, ayrıca erişilebilirlik standartlarını karşılar. 
* **Grafiğin göster:** Sonuçlarınızı bir kolayca noktaları yukarı ve aşağı doğru eğilimleri ve anormallikleri satır biçiminde görüntülenir. 

 ![Analiz için kılavuz görünümü gösteriliyor](media/howto-create-analytics-experimental/analytics-showgrid.png)

Yakınlaştırma, verileriniz üzerinde odaklanmanıza imkan sağlar. Odaklanmak için sonuç kümesinde istediğiniz bir zaman dönemi bulursanız, imlecinizi yakınlaştırmak ve aşağıdaki eylemlerden birini gerçekleştirmek için kullanılabilir denetimleri kullanmak istediğiniz alanı almak için kullanın:
* **Yakınlaştırma:** Bir zaman dönemi seçtikten sonra yakınlaştırma, etkin ve verileriniz için yakınlaştırın olanak sağlar.
* **Uzaklaştır:** Bu denetim, yakınlaştırma, son yakınlaştırma bir düzeyi sağlar. Örneğin, üç kez verilerinize olduğunuz ekranı yakınlaştırdığınızda, uzaklaştırma sürecek bir kerede tek bir adımda geri.
* **Yakınlaştırma sıfırlanması:** Yakınlaştırma düzeyleri çeşitli gerçekleştirdiğiniz sonra özgün Sonuç kümenizi döndürülecek yakınlaştırma sıfırlama denetimini kullanabilirsiniz. 

 ![Verileriniz üzerinde yakınlaştırma gerçekleştirin](media/howto-create-analytics-experimental/analytics-zoom.png)


Çizgi stili gereksinimlerinizi karşılayacak şekilde değiştirebilirsiniz. Aralarından seçim yapabileceğiniz dört seçeneğiniz vardır:
* **Satırı:** Her veri noktası arasında düz bir çizgi biçimlendirilmiş olmalıdır. 
* **Düzgün:** Her nokta arasında bir eğri çizgi oluşturulmuş
* **Adım:** Grafikteki her nokta arasındaki çizgi adım grafiği oluşturur.
* **Dağılım:** Tüm noktaları grafikteki satırları bağlamayı çizilir. 

 ![Farklı satır türleri Analytics'te kullanılabilir](media/howto-create-analytics-experimental/analytics-linetypes.png)

Son olarak, üç moddan birini seçerek y ekseni verilerinizi düzenleyebilirsiniz:

* **Yığın:** Her bir ölçüm grafiği Yığılmış ve her grafik kendi y ekseni. Yığılmış grafikler seçili birden çok ölçümleri sahip ve farklı bir görünüm Bu ölçümlerin istediğinizde yararlıdır.
* **Yığılmamış:** Her ölçü için bir grafik karşı bir y ekseni çizilme ancak y değerlerini vurgulanan ölçü göre değişir. Yığılmamış grafikleri, birden çok ölçü kaplama ve aynı zaman aralığı için bu ölçümleri arasında desenleri görmek istediğinizde yararlıdır.
* **Y ekseni paylaşılan:** Tüm grafikleri aynı y ekseni paylaşın ve eksen için değerleri değiştirmeyin. Paylaşılan y ekseni grafikleri, bölünmüş tarafından verileri dilimleme sırasında tek bir ölçü aramak istediğinizde kullanışlıdır.

 ![Verilerinizi farklı görselleştirme modları y ekseni düzenleyin](media/howto-create-analytics-experimental/analytics-yaxis.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Central uygulamanız için özel analytics oluşturulacağını öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Hazırlama ve bir Node.js uygulaması bağlanın](howto-connect-nodejs-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)