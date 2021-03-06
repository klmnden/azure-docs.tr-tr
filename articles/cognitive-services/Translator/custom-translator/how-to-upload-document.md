---
title: Belge - özel Translator karşıya yükleme
titleSuffix: Azure Cognitive Services
description: Belgeyi Karşıya Yükleme özelliğini kullanarak, eğitimleri için paralel bir belge karşıya yükleyebilirsiniz. Paralel belgeleri belgelerin bir diğer çeviri olduğu çiftleridir. Çifti tek bir belgede kaynak dili cümlelerde ve diğer belge Bu cümle hedef dile çevrilen içerir.
author: swmachan
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 948c9628147ae8cf869046b283601b939d35b290
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447919"
---
# <a name="upload-a-document"></a>Bir belgeyi karşıya yükleme

İçinde [özel Translator](https://portal.customtranslator.azure.ai), çeviri Modellerinizi eğitmek için paralel belgeler karşıya yükleyebilirsiniz. [Paralel belgeleri](what-are-parallel-documents.md) belgeleri diğer çeviri olduğu bir çiftleridir. Çifti tek bir belgede kaynak dili cümlelerde ve diğer belge Bu cümle hedef dile çevrilen içerir.

Belgelerinizi karşıya yüklemeden önce gözden [Belge biçimleri ve adlandırma kuralı yönergelerini](document-formats-naming-convention.md) dosyanızı emin olmak için biçim özel Translator içinde desteklenir.

## <a name="how-to-upload-document"></a>Belgeyi Karşıya yüklemek nasıl?

Gelen [özel Translator](https://portal.customtranslator.azure.ai) portal belgeleri sayfasına gitmek için sekmesinde "Belgeler"'a tıklayın.

![Belge yükleme bağlantısı](media/how-to/how-to-upload-1.png)


1.  Belgeler sayfası karşıya yükleme dosyaları düğmesine tıklayın.

    ![Belge sayfasını yükleyin](media/how-to/how-to-upload-2.png)

2.  İletişim hakkında aşağıdaki bilgileri doldurun:

    a.  Belge türü:

    -  Eğitim: Bu belgeler, eğitim kümesi için kullanılır.
    -  Ayarlama: Bu belge kümesini ayarlamak için kullanılır.
    -  Test: Bu belge, kümesi test etmek için kullanılır.
    -  İfade sözlüğü: Bu belge, ifade sözlüğü için kullanılır.
    -  Tümce sözlüğü: Bu belge, cümle sözlük için kullanılacak

    b.  Dil çifti

    c.  Belge geçersiz kılmak bulunmaktadır: Aynı ada sahip tüm mevcut belgeleri üzerine yazmak istiyorsanız bu onay kutusunu seçin.

    d.  Paralel veri veya birleşik giriş verileri için ilgili bölüme doldurun.

    -  Paralel veri:
        -  Kaynak dosya: Kaynak dili dosyasını kullanarak yerel bilgisayarınızdan seçin.
        -  Hedef dosya: Yerel bilgisayarınızdan hedef dile dosyayı seçin.
        -  Belge adı: Paralel dosya karşıya yüklemekte yalnızca kullanılır.

    - Birleşik giriş verileri:
        -  Birleşik giriş dosyası: Yerel bilgisayarınızdan açılan dosyayı seçin. Birleşik giriş dosyanız hem kaynak ve hedef dil tümceleri vardır. [Adlandırma kuralı](document-formats-naming-convention.md) birleşik giriş dosyaları için önemlidir.

    e.  Yükle'yi tıklatın

    ![Belge iletişim karşıya yükleme](media/how-to/how-to-upload-dialog.png)

3.  Bu noktada, biz belgelerinizi işleme ve cümleleri çalışılıyor. "Görünümü karşıya yükleme ilerleme durumu" tıklayabilirsiniz işlemek gibi belgelerinizi durumunu denetlemek için.

    ![Belge işleme iletişim karşıya yükleme](media/how-to/how-to-upload-processing-dialog.png)

4.  Bu sayfa durumu ve karşıya yükleme işleminiz içindeki her dosya için herhangi bir hata görüntülenir. "Geçmiş karşıya yükle" sekmesinde tıklayarak herhangi bir zamanda yükleme durumunu görüntüleyebilirsiniz.

    ![Belge geçmişi iletişim karşıya yükleme](media/how-to/how-to-upload-document-history.png)


## <a name="view-upload-history"></a>Karşıya yükleme Geçmişi Görüntüle

Belge türü, dil çifti, karşıya yükleme geçmişi sayfasında ayrıntılar gibi tüm belgeyi karşıya geçmişini görüntüleyebilirsiniz durumu vb. yükleyin.

1. Gelen [özel Translator](https://portal.customtranslator.azure.ai) portal geçmişini görüntülemek için karşıya yükleme geçmiş sekmesine tıklayın.

    ![Geçmiş sekmesinde karşıya yükleme](media/how-to/how-to-upload-history-1.png)

2. Bu sayfa, son karşıya yükleme durumunu gösterir. Karşıya yükleme görüntüler için en eski en son öğesinden. Her yükleme için belge adı, karşıya yükleme durumu, karşıya yükleme tarihi, karşıya yüklenen dosya sayısı, karşıya dosya türü ve dosya dil çiftinin gösterir.

    ![Geçmiş sayfasını yükleyin](media/how-to/how-to-document-history-2.png)

3. Herhangi bir karşıya yükleme geçmiş kaydının tıklayın. Karşıya yükleme geçmişi Ayrıntıları sayfasında, karşıya yükleme, karşıya yüklenen dosya, dil (varsa herhangi bir hata karşıya yükleme) dosya ve hata iletisinin durumunu bir parçası olarak yüklenmiş dosyalarını görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanım [belge Ayrıntıları sayfası](how-to-view-document-details.md) ayıklanan cümleler listesini gözden geçirebilirsiniz.
- [Bir model eğitip nasıl](how-to-train-model.md).
