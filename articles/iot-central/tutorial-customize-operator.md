---
title: Azure IoT Central’da operatör görünümlerini özelleştirme | Microsoft Docs
description: Oluşturucu olarak, Azure IoT Central uygulamanızda operatör görünümlerini özelleştirin.
author: sandeeppujar
ms.author: sandeepu
ms.date: 04/16/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
ms.openlocfilehash: d99b76faf618439e51735d5f1096fd4f1cfd2364
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47038298"
---
# <a name="tutorial-customize-the-azure-iot-central-operators-view"></a>Öğretici: Azure IoT Central operatör görünümünü özelleştirme

Bu öğreticide, bir oluşturucu olarak uygulamanızın operatör görünümünü nasıl özelleştireceğiniz gösterilmektedir. Oluşturucu olarak uygulamada bir değişiklik yaptığınızda, Microsoft Azure IoT Central uygulamasında operatör görünümünün önizlemesini görebilirsiniz.

Bu öğreticide, bağlı klima cihazıyla ilgili bilgileri bir operatöre gösterecek şekilde uygulamayı özelleştireceksiniz. Özelleştirmeleriniz sayesinde operatör, uygulamaya bağlı klima cihazlarını yönetebilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Cihaz panonuzu yapılandırma
> * Cihaz ayarları düzeninizi yapılandırma
> * Cihaz özellikleri düzeninizi yapılandırma
> * Bir operatör olarak cihazın önizlemesini görme
> * Varsayılan giriş sayfanızı yapılandırma
> * Bir operatör olarak varsayılan giriş sayfasının önizlemesini görme

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, önceki iki öğreticiyi tamamlamanız gerekir:

* [Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlama](tutorial-define-device-type.md).
* [Cihazınız için kurallar ve eylemler yapılandırma](tutorial-configure-rules.md).

## <a name="configure-your-device-dashboard"></a>Cihaz panonuzu yapılandırma

Oluşturucu olarak, bir cihaz panosunda hangi bilgilerin gösterileceğini tanımlayabilirsiniz. [Uygulamanızda yeni bir cihaz türü tanımlama](tutorial-define-device-type.md) öğreticisinde, **Bağlı Klima-1** panosuna bir çizgi grafik ve diğer bilgileri eklemiştiniz.

1. **Bağlı Klima** cihaz şablonunu düzenlemek için sol gezinti menüsündeki **Gezgin**’i seçin:

    ![Gezgin sayfası](media/tutorial-customize-operator/explorer.png)

2. Bağlı klima cihazınızın panosunu özelleştirmeye başlamak için **Bağlı Klima (1.0.0)** cihaz şablonunu seçin. [Uygulamanızda yeni bir cihaz türü tanımlama](tutorial-define-device-type.md) öğreticisinde oluşturduğunuz **Bağlı Klima-1** cihazını seçin:

    ![Bağlı klima cihazını seçme](media/tutorial-customize-operator/selectdevice.png)

    **Bağlı Klima-1** gibi bir cihazın içindeyken, temel alınan şablonda değişiklik yapmak için **Şablonu Düzenle**’yi seçebilirsiniz. Daha fazla bilgi için bkz. [Yeni bir cihaz şablonu sürümü oluşturma](howto-version-devicetemplate.md).

3. Panoyu düzenlemek için **Pano**’yu seçin ve **Şablonu Düzenle**’yi belirtin:

    ![Cihaz şablonu pano sayfası](media/tutorial-customize-operator/dashboard.png)

4. Panoya bir KPI kutucuğu eklemek için **KPI**’yı seçin:

    ![KPI ekleme](media/tutorial-customize-operator/addkpi.png)

    KPI’yı tanımlamak için aşağıdaki tabloda yer alan bilgileri kullanın:

    | Ayar     | Değer |
    | ----------- | ----- |
    | Adı        | En fazla sıcaklık |
    | Ölçüm | sıcaklık |
    | Toplama | Maksimum |
    | Zaman aralığı  | Geçen 1 hafta |

5. **Kaydet**'i seçin. Artık KPI kutucuğunu panoda görebilirsiniz:

    ![KPI kutucuğu](media/tutorial-customize-operator/temperaturekpi.png)

6. Panodaki bir kutucuğu taşımak veya yeniden boyutlandırmak için, fare işaretçisini kutucuğun üzerine getirin. Kutucuğu yeni bir konuma sürükleyebilir ya da yeniden boyutlandırabilirsiniz:

    ![Pano düzenini düzenleme](media/tutorial-customize-operator/dashboardlayout.png)

7. Değişiklik yapmayı tamamladığınızda **Bitti**’yi tıklatın.

## <a name="configure-your-settings-layout"></a>Ayar düzeninizi değiştirme

Oluşturucu olarak, cihaz ayarlarının operatör görünümünü de yapılandırabilirsiniz. Operatör bir cihazı yapılandırmak için cihaz ayarları sayfasını kullanır. Örneğin, operatör buzdolabının hedef sıcaklığını ayarlamak için ayarlar sayfasını kullanır.

1. Bağlı klimanızın ayar düzenini değiştirmek için **Ayarlar**’ı seçin ve **Şablonu Düzenle**’yi belirtin:

    ![Ayarlar sayfası](media/tutorial-customize-operator/settings.png)

2. Ayar kutucuklarını taşıyabilir ve yeniden boyutlandırabilirsiniz:

    ![Ayarlar düzenini değiştirme](media/tutorial-customize-operator/settingslayout.png)

3. Değişiklik yapmayı tamamladığınızda **Bitti**’yi tıklatın.

> [!NOTE]
> **Şablonu Düzenle**’de ayarların değerlerini düzenleyemezsiniz.

## <a name="configure-your-properties-layout"></a>Özellikler düzeninizi yapılandırma

Pano ve ayarlara ek olarak cihaz özelliklerinin operatör görünümünü de yapılandırabilirsiniz. Operatör, cihaz meta verilerini yönetmek için cihaz özellikleri sayfasını kullanır. Örneğin, operatör bir cihaz seri numarasını görüntülemek veya üreticinin iletişim bilgilerini güncelleştirmek için özellikler sayfasını kullanır.

1. Bağlı klimanızın ayar düzenini değiştirmek için **Ayarlar**’ı seçin ve **Şablonu Düzenle**’yi belirtin:

    ![Özellikler sayfası](media/tutorial-customize-operator/properties.png)

2. Özellikler alanlarını taşıyabilir ve yeniden boyutlandırabilirsiniz:

    ![Özellikler düzenini değiştirme](media/tutorial-customize-operator/propertieslayout.png)

3. Değişiklik yapmayı tamamladığınızda **Bitti**’yi tıklatın.

> [!NOTE]
> **Şablonu Düzenle** modunda özelliklerin değerlerini düzenleyemezsiniz.

## <a name="preview-the-connected-air-conditioner-device-as-an-operator"></a>Operatör olarak bağlı klima cihazının önizlemesini görme

**Şablonu Düzenle** modunda bir işlecin pano, ayarlar ve özellikler sayfalarını özelleştirebilirsiniz. **Şablonu Düzenle** modunda değilseniz, uygulamayı bir işleç olarak görüntüleyebilirsiniz.

1. Bağlı klima cihazınızı işleç olarak görüntülemek isterseniz, şablonu düzenlemeyi durdurmak için **Bitti**’yi tıklamanız gerekir. Bu cihaz size cihazın operatör görünümünü döndürür.

2. Bu cihazın konumunu güncelleştirmek için konum kutucuğundaki değeri düzenleyin ve **Kaydet**’i seçin:

    ![Özellik değerini düzenleme](media/tutorial-customize-operator/editproperty.png)

3. Bağlı klimanıza bir ayar göndermek için **Ayarlar**’ı seçin, bir kutucuktaki ayar değerini değiştirin ve **Güncelleştir**’i seçin:

    ![Cihaza ayar gönderme](media/tutorial-customize-operator/sendsetting.png)

    Cihaz yeni ayar değerini kabul ettiğinde, ayar kutucukta **eşitlendi** olarak görünür.

4. Operatör olarak, cihaz panosunu oluşturucu tarafından yapılandırıldığı şekilde görüntüleyebilirsiniz:

    ![Cihaz panosunun operatör görünümü](media/tutorial-customize-operator/operatordashboard.png)

## <a name="configure-the-default-home-page"></a>Varsayılan giriş sayfasını yapılandırma

Oluşturucu veya operatör bir Azure IoT Central uygulamasında oturum açtığında bir giriş sayfası görür. Oluşturucu olarak, bu giriş sayfasının içeriğini operatör için en yararlı ve ilgili içeriği dahil edecek şekilde yapılandırabilirsiniz.

1. Varsayılan giriş sayfasını özelleştirmek için **Giriş** sayfasına gidin ve sayfanın sağ üst kısmından **Düzenle**’yi seçin. **Düzenle**’nin seçilmesiyle, Giriş Sayfanıza ekleyebileceğiniz nesnelerin bir listesiyle birlikte sağ taraftan bir panel açılır.

    ![Uygulama Oluşturucu sayfası](media/tutorial-customize-operator/builderhome.png)

2. Giriş sayfasını özelleştirmek için **Kütüphane**’den kutucuk ekleyin. **Bağlantı**’yı seçin ve kuruluşunuzun web sitesinin ayrıntılarını ekleyin. Ardından **Kaydet**'i seçin:

    ![Giriş sayfasına bağlantı ekleme](media/tutorial-customize-operator/addlink.png)

    > [!NOTE]
    > Azure IoT Central uygulamanızdaki sayfalara bağlantılar da ekleyebilirsiniz. Örneğin, bir cihazın panosuna ya da ayarlar sayfasına bağlantı ekleyebilirsiniz.

3. İsteğe bağlı olarak, **Görüntü**’yü seçin ve giriş sayfanızda gösterilecek bir görüntü yükleyin. Görüntünün, üzerine tıkladığınızda gidebileceğiniz bir URL’si olabilir:

    ![Giriş sayfasına görüntü ekleme](media/tutorial-customize-operator/addimage.png)

    Daha fazla bilgi için bkz. [Görüntüleri hazırlama ve Azure IoT Central uygulamanıza yükleme](howto-prepare-images.md).

## <a name="preview-the-default-home-page-as-an-operator"></a>Bir operatör olarak varsayılan giriş sayfasının önizlemesini görme

Ana sayfanın önizlemesini işleç olarak görüntülemek ve daha fazla düzenlememek için, sayfanın sağ üst kısmındaki **Bitti**’yi seçin

![Tasarım Modunu Açma/Kapatma](media/tutorial-customize-operator/operatorviewhome.png)

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

* [Cihazlarınızı izleme (operatör olarak)](tutorial-monitor-devices.md)
* [Uygulamanıza yeni cihaz ekleme (operatör ve cihaz geliştiricisi olarak)](tutorial-add-device.md)
