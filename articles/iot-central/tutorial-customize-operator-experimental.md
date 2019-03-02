---
title: Azure IoT Central’da operatör görünümlerini özelleştirme | Microsoft Docs
description: Oluşturucu olarak, Azure IoT Central uygulamanızda operatör görünümlerini özelleştirin.
author: sandeeppujar
ms.author: sandeepu
ms.date: 01/29/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
ms.openlocfilehash: 0494e9ef2a4f7adddd615dbf7882c9f2b4b81982
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57213220"
---
# <a name="tutorial-customize-the-azure-iot-central-operators-view-new-ui-design"></a>Öğretici: Azure IOT Central işlecin görünümü (yeni kullanıcı Arabirimi tasarımı) özelleştirme

Bu öğreticide, bir oluşturucu olarak uygulamanızın operatör görünümünü nasıl özelleştireceğiniz gösterilmektedir. Oluşturucu olarak uygulamada bir değişiklik yaptığınızda, Microsoft Azure IoT Central uygulamasında operatör görünümünün önizlemesini görebilirsiniz.

Bu öğreticide, bağlı klima cihazıyla ilgili bilgileri bir operatöre gösterecek şekilde uygulamayı özelleştireceksiniz. Özelleştirmeleriniz sayesinde operatör, uygulamaya bağlı klima cihazlarını yönetebilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Cihaz panonuzu yapılandırma
> * Cihaz ayarları düzeninizi yapılandırma
> * Cihaz özellikleri düzeninizi yapılandırma
> * Bir operatör olarak cihazın önizlemesini görme
> * Varsayılan uygulama panonuza yapılandırın
> * Önizleme operatör olarak varsayılan uygulama Panosu

[!INCLUDE [iot-central-experimental-note](../../includes/iot-central-experimental-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce, önceki iki öğreticiyi tamamlamanız gerekir:

* [Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlama](tutorial-define-device-type-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).
* [Cihazınız için kurallar ve eylemler yapılandırma](tutorial-configure-rules-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).

## <a name="configure-your-device-dashboard"></a>Cihaz panonuzu yapılandırma

Oluşturucu olarak, bir cihaz panosunda hangi bilgilerin gösterileceğini tanımlayabilirsiniz. İçinde [uygulamanızda yeni bir cihaz türü tanımlayan](tutorial-define-device-type-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) Öğreticisi, eklediğiniz bir çizgi grafik ve diğer bilgileri **bağlı bir klima** Pano.

1. Düzenlenecek **bağlı bir klima** cihaz şablonu seçin **cihaz şablonları** sol gezinti menüsünde:

    ![Cihaz şablonlarını](media/tutorial-customize-operator-experimental/devicetemplates.png)

2. Cihaz panonuzu özelleştirmek için tıklatın **bağlı klima (1.0.0)** oluşturduğunuz cihaz şablonu cihaz [uygulamanızda yeni bir cihaz türü tanımlayan](tutorial-define-device-type-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) öğretici.

3. Pano düzenlemek için seçin **Pano** sekmesi.

4. Panoya Ana Performans Göstergesi (KPI) kutucuğu eklemek için **KPI**'yı seçin:

    KPI’yı tanımlamak için aşağıdaki tabloda yer alan bilgileri kullanın:

    | Ayar     | Değer |
    | ----------- | ----- |
    | Ad        | En fazla sıcaklık |
    | Zaman aralığı  | Geçen 1 hafta |
    | Ölçüm Türü | Telemetri |
    | Ölçüm | sıcaklık |
    | Toplama | Maksimum |
    | Görünürlük  | Etkin |

    ![KPI ekleme](media/tutorial-customize-operator-experimental/addkpi.png)

5. **Kaydet**’e tıklayın. Artık KPI kutucuğunu panoda görebilirsiniz:

    ![KPI kutucuğu](media/tutorial-customize-operator-experimental/temperaturekpi.png)

6. Panodaki bir kutucuğu taşımak veya yeniden boyutlandırmak için, fare işaretçisini kutucuğun üzerine getirin. Kutucuğu yeni bir konuma sürükleyin ya da yeniden boyutlandırın.

## <a name="configure-your-settings-layout"></a>Ayar düzeninizi değiştirme

Oluşturucu olarak, cihaz ayarlarının operatör görünümünü de yapılandırabilirsiniz. Operatörün cihaz Ayarlar sekmesinde bir cihaz yapılandırmak için kullanır. Örneğin, operatöre bağlı klima için hedef sıcaklık için Ayarlar sekmesinde kullanır.

1. Bağlı klima için ayarları düzenlemek için seçin **ayarları** sekmesi.

2. Ayar kutucuklarını taşıyabilir ve yeniden boyutlandırabilirsiniz:

    ![Ayarlar düzenini değiştirme](media/tutorial-customize-operator-experimental/settingslayout.png)

## <a name="configure-your-properties-layout"></a>Özellikler düzeninizi yapılandırma

Pano ve ayarlara ek olarak cihaz özelliklerinin operatör görünümünü de yapılandırabilirsiniz. Operatör, cihaz meta verilerini yönetmek için cihaz Özellikleri sekmesi kullanır. Örneğin, operatörün cihaz seri numarasını görüntülemek veya üreticisi için kişi ayrıntılarını güncelleştirmek için Özellikler sekmesinde kullanır.

1. Bağlı klima için özelliklerini düzenlemek için seçin **özellikleri** sekmesi.

2. Özellikler alanlarını taşıyabilir ve yeniden boyutlandırabilirsiniz:

    ![Özellikler düzenini değiştirme](media/tutorial-customize-operator-experimental/propertieslayout.png)

## <a name="preview-the-device"></a>Cihaz önizlemesi

Kullandığınız **cihaz şablonları** Pano, ayarları ve özellikleri sekme için bir işleç özelleştirmek için sayfa. Kullandığınız **Device Explorer** sayfasını görüntüleyin ve cihaz şablonu kullanın.

1. Görüntüleyebilir ve bir operatör olarak bağlı bir klima şablonu kullanmak için gidin **Device Explorer** sayfasında ve bu IOT Central, şablondan oluşturulan sanal cihazı seçin:

    ![Görüntüleme ve cihaz şablonu kullanma](media/tutorial-customize-operator-experimental/usetemplate.png)

2. Bu cihazın konumunu güncelleştirmek için seçin **özellikleri** ve konum kutucuk değeri düzenleyin. Ardından **Kaydet**:

    ![Özellik değerini düzenleme](media/tutorial-customize-operator-experimental/editproperty.png)

3. Bağlı klimanıza bir ayar göndermek için **Ayarlar**’ı seçin, bir kutucuktaki ayar değerini değiştirin ve **Güncelleştir**’i seçin:

    ![Cihaza ayar gönderme](media/tutorial-customize-operator-experimental/sendsetting.png)

    Cihaz yeni ayar değerini kabul ettiğinde, ayar kutucukta **eşitlendi** olarak görünür.

4. Operatör olarak, cihaz panosunu oluşturucu tarafından yapılandırıldığı şekilde görüntüleyebilirsiniz:

    ![Cihaz panosunun operatör görünümü](media/tutorial-customize-operator-experimental/operatordashboard.png)

## <a name="configure-the-default-dashboard"></a>Varsayılan Pano yapılandırın

Bir oluşturucu veya işleci bir Azure IOT Central uygulamasına oturum açtığında, uygulama Panosu görürler. Bir oluşturucu, bir işleç için en faydalı ve ilgili içerik için varsayılan Pano içeriğini yapılandırabilirsiniz.

> [!NOTE]
> Kullanıcılar ayrıca kendi kişisel panolar oluşturabilir ve varsayılan olarak seçin.

1. Varsayılan uygulama Panosu özelleştirmek için gezinme **Pano** sayfasında ve tıklayın **Düzenle** üst sayfanın sağ. Panoya ekleyebilirsiniz nesnelerin bir kitaplığı ile bir paneli görüntülenir.

    ![Pano sayfası](media/tutorial-customize-operator-experimental/builderhome.png)

2. Pano özelleştirmek için kutucukları ekleme **Kitaplığı**. **Bağlantı**’yı seçin ve kuruluşunuzun web sitesinin ayrıntılarını ekleyin. Ardından **Kaydet**'i seçin:

    ![Bağlantıyı panoya ekleme](media/tutorial-customize-operator-experimental/addlink.png)

    > [!NOTE]
    > Azure IoT Central uygulamanızdaki sayfalara bağlantılar da ekleyebilirsiniz. Örneğin, bir cihazın panosuna ya da ayarlar sayfasına bağlantı ekleyebilirsiniz.

3. İsteğe bağlı olarak, **görüntü** ve Panonuzda görüntülemek için bir görüntü yükleyin. Bir görüntü olduğu ulaşmanıza tıkladığınızda bir URL olabilir:

    ![Panoya resim ekleme](media/tutorial-customize-operator-experimental/addimage.png)

    Daha fazla bilgi için bkz. [Görüntüleri hazırlama ve Azure IoT Central uygulamanıza yükleme](howto-prepare-images-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).

## <a name="preview-the-dashboard"></a>Pano Önizleme

Uygulama Panosu operatör olarak önizlemesini görüntülemek için seçin **Bitti** üst sayfanın sağ.

![Tasarım Modunu Açma/Kapatma](media/tutorial-customize-operator-experimental/operatorviewhome.png)

Oluşturucu olarak ayarladığınız URL’lere gitmek için bağlantıya ve görüntü kutucuklarına tıklayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, uygulamanın operatör görünümünü özelleştirme hakkında bilgi edindiniz.

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Cihaz panonuzu yapılandırma
> * Cihaz ayarları düzeninizi yapılandırma
> * Cihaz özellikleri düzeninizi yapılandırma
> * Bir operatör olarak cihazın önizlemesini görme
> * Varsayılan giriş sayfanızı yapılandırma
> * Bir operatör olarak varsayılan giriş sayfasının önizlemesini görme

Uygulamanın operatör görünümünü özelleştirmeyi öğrendiğinize göre, önerilen sonraki adımlar şunlardır:

* [Cihazlarınızı izleme (operatör olarak)](tutorial-monitor-devices-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
* [Uygulamanıza yeni cihaz ekleme (operatör ve cihaz geliştiricisi olarak)](tutorial-add-device-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
