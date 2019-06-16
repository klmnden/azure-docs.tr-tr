---
title: Azure IOT Central uygulamanızdaki cihaz verilerinizi analiz edin | Microsoft Docs
description: Azure IOT Central uygulamanızdaki cihaz verilerinizi analiz edin.
author: lmasieri
ms.author: lmasieri
ms.date: 06/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: ffe8b350c1b5cea23aeb65092c7912c6d6c1ed89
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67052963"
---
# <a name="how-to-use-analytics-to-analyze-your-device-data"></a>Cihazınızın verileri analiz etmek için Analytics kullanma

*Bu makale, işleçler, oluşturucular ve yöneticiler için geçerlidir.*

Azure IOT Central, büyük miktarda verileri anlamlı zengin analiz özellikleri sağlar. Başlamak için ziyaret **Analytics** sol gezinti menüsünde.

## <a name="querying-your-data"></a>Verilerinizi sorgulama

Seçmeniz gerekir bir **cihaz kümesi**, ekleme bir **filtre** (isteğe bağlı) seçip bir **süre** kullanmaya başlamak için. İşiniz bittiğinde seçin **sonuçları göster** verilerinizi görselleştirme başlatmak için.

* **Cihaz kümeleri:** A [cihaz kümesi](howto-use-device-sets.md) cihazlarınızı, kullanıcı tanımlı grubudur. Örneğin, tüm Refrigerators Oakland içinde ya da tüm 2.0 Rüzgar turbines rev.

* **Filtreler:** İsteğe bağlı olarak, verileriniz üzerinde geliştirmeniz için arama filtreleri ekleyebilirsiniz. Aynı anda en fazla 10 filtreleri ekleyebilirsiniz. Örneğin, sıcaklık çalıştırılmış olan 60 derecenin Git Oakland içindeki tüm Refrigerators içinde bulun.
* **Zaman aralığı:** Varsayılan olarak veri son 10 dakika ile alıyoruz. Önceden tanımlanmış saat aralıklardan biri için bu değeri değiştirin veya bir özel zaman aralığı seçin.

  ![Analiz sorgusu](media/howto-create-analytics/analytics-query.png)

## <a name="visualizing-your-data"></a>Verilerinizi Görselleştirme

Verilerinizi sorgulanan sonra bu görselleştirme başlatmak mümkün olacaktır. Toplanmış ve daha fazla veri biçimini değiştirme verileri farklı cihaz özelliklerine göre bölmek, Göster/ölçümleri gizle.  

* **Bölme ölçütü:** Cihaz özellikleri tarafından veri bölme için daha fazla ayrıntıya aşağı verilerinizi sağlar. Örneğin, sonuçlarınızı cihaz kimliği veya konuma göre bölebilirsiniz.

* **Ölçümleri:** Aynı anda, cihazlarınız tarafından bildirilen en fazla 10 farklı telemetri öğeleri Göster/Gizle seçebilirsiniz. Ölçümler, sıcaklık ve nem gibi noktalardır.

* **Toplama:** Varsayılan olarak şu veri, ortalama ile toplama, ancak veri toplama şeye kendi gereksinimlerinize uyacak şekilde değiştirmek seçebilirsiniz.

   ![Bölme ölçütü analytics Görselleştirme](media/howto-create-analytics/analytics-splitby.png)

## <a name="interacting-with-your-data"></a>Verilerinizle etkileşim kurma

Sorgu sonuçlarını görselleştirme gereksinimlerinizi karşılayacak şekilde değiştirmek için çeşitli yollar var. Bir graf görünümünden ve ızgara görünümü arasında geçiş, yakınlaştırma ve uzaklaştırma, Veri kümenizi yenileyin ve çizgilerinin nasıl gösterileceğini alter.

* **Kılavuzu Göster:** Sonuçlarınız, tablo biçiminde, böylece her veri noktası için belirli bir değeri görüntülemek kullanılabilir. Bu görünüm, ayrıca erişilebilirlik standartlarını karşılar.
* **Grafiğin göster:** Sonuçlarınızı yukarı tanımlamanıza yardımcı olması için bir satır biçimi veya aşağı eğilimleri ve anormallikleri görüntülenir.

  ![Analiz için kılavuz görünümü gösteriliyor](media/howto-create-analytics/analytics-showgrid.png)

Yakınlaştırma, verileriniz üzerinde giriş sayfasında olanak tanır. Odaklanmak için sonuç kümesinde istediğiniz bir zaman dönemi bulursanız, imlecinizi yakınlaştırmak ve aşağıdaki eylemlerden birini gerçekleştirmek için kullanılabilir denetimleri kullanmak istediğiniz alanı almak için kullanın:

* **Yakınlaştırma:** Bir zaman dönemi seçtikten sonra yakınlaştırma etkinleştirilir ve verilerinize yakınlaştırmak olanak tanır.
* **Uzaklaştır:** Bu denetim, yakınlaştırma, son yakınlaştırma bir düzeyi sağlar. Yakınlaştırma belirttiyseniz, verilerinizi üç kez gerçekleştirilen işlemlerin uzaklaştırma'için bir adım teker teker tekrar açın.
* **Yakınlaştırma sıfırlanması:** Yakınlaştırma düzeyleri çeşitli gerçekleştirdiğiniz sonra özgün Sonuç kümenizi döndürülecek yakınlaştırma sıfırlama denetimini kullanabilirsiniz.

  ![Verileriniz üzerinde yakınlaştırma gerçekleştirin](media/howto-create-analytics/analytics-zoom.png)

Çizgi stili gereksinimlerinizi karşılayacak şekilde değiştirebilirsiniz. Dört seçeneğiniz vardır:

* **Satırı:** Her veri noktası arasında düz bir çizgi.
* **Düzgün:** Her nokta arasında bir eğri çizgi.
* **Adım:** Satır grafikteki her nokta arasında bir adımdır.
* **Dağılım:** Tüm noktaları grafikteki satırları bağlamayı çizilir.

  ![Farklı satır türleri Analytics'te kullanılabilir](media/howto-create-analytics/analytics-linetypes.png)

Son olarak, üç moddan birini seçerek y ekseni verilerinizi düzenleyebilirsiniz:

* **Yığın:** Her bir ölçüm grafiği Yığılmış ve her grafik kendi y ekseni. Yığılmış grafikler seçili birden çok ölçümleri sahip ve farklı bir görünüm Bu ölçümlerin istediğinizde yararlıdır.
* **Yığılmamış:** Her ölçü için bir grafik karşı bir y ekseni çizilme ancak y değerlerini vurgulanan ölçü göre değişir. Yığılmamış grafikleri, birden çok ölçü kaplama ve aynı zaman aralığı için bu ölçümleri arasında desenleri görmek istediğinizde yararlıdır.
* **Y ekseni paylaşılan:** Tüm grafikleri aynı y ekseni paylaşın ve eksen için değerleri değiştirmeyin. Paylaşılan y ekseni grafikleri, bölünmüş tarafından verileri dilimleme sırasında tek bir ölçü aramak istediğinizde kullanışlıdır.

  ![Verilerinizi farklı görselleştirme modları y ekseni düzenleyin](media/howto-create-analytics/analytics-yaxis.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Central uygulamanız için özel analytics oluşturulacağını öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Hazırlama ve bir Node.js uygulaması bağlanın](howto-connect-nodejs.md)