---
title: Bir proje oluşturmak nasıl? -Özel Translator
titleSuffix: Azure Cognitive Services
description: Özel Translator içinde bir proje oluşturmak nasıl?
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: v-pawal
ms.topic: conceptual
ms.openlocfilehash: 456860c74810a692b4839e4204ec0b78d5620864
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66383001"
---
# <a name="create-a-project"></a>Proje oluşturma

Bir proje modelleri, belgeler için bir kapsayıcıdır ve test eder. Her proje, otomatik olarak doğru dil çiftiniz olması için bu çalışma alanına yüklenen tüm belgeleri içerir.

Proje oluşturma, bir model oluşturmaya yönelik ilk adımdır.

## <a name="create-a-project"></a>Bir proje oluşturun:

1.  İçinde [özel Translator](https://portal.customtranslator.azure.ai) portal, proje oluştur'a tıklayın.

    ![Proje oluşturma](media/how-to/how-to-create-project.png)

2.  Projeniz hakkında aşağıdaki ayrıntıları iletişim kutusunda girin:

    a.  Proje adı (gerekli): Projenize benzersiz ve anlamlı bir ad verin. Başlık içinde dilleri bahsetmek gerekli değildir.

    b.  Açıklama: Projeyle ilgili kısa bir özeti. Bu açıklama, özel Translator ya da sonuçta elde edilen özel sisteminizi davranışı üzerinde hiçbir etkisi yoktur, ancak, farklı projeler arasında ayırt etmenize yardımcı olabilir.

    c.  Dil çifti (gerekli): Başlangıç ve bitiş çevirme dil seçin.

    d.  Kategori (gerekli): Projeniz için en uygun kategoriyi seçin. Kategori terminoloji ve çevirmek için istediğinize belgelerin stil açıklar.

    e.  Kategori açıklaması: Belirli bir alan veya, çalıştığınız sektör daha iyi tanımlamak için bu alanı kullanın. Örneğin, TIP, kategori ise belirli bir belge, tür ameliyatı yapıyor veya pediatrics ekleyebilirsiniz. Açıklama davranışı özel Translator ya da sonuçta elde edilen özel sisteminizin üzerinde etkisi yoktur.

    f.  Proje etiketi: [Proje etiket](workspace-and-project.md#project-labels) projeleri aynı dil çifti ve kategori ile ayırt eder. En iyi uygulama, bir etiket kullanın *yalnızca* birden çok proje için aynı kategoriye ve aynı dil çifti oluşturup bu projeleri ile farklı CategoryID erişmek istediğiniz planlıyorsanız. Bu alan sistemler yalnızca bir kategori için oluşturuluyorsa kullanmayın. Bir proje etiket gerekli ve dil çiftleri arasında ayrım yapmak yararlı değildir. Birden çok proje için aynı etiketi kullanabilirsiniz.

    ![Projesi oluştur iletişim kutusu](media/how-to/how-to-create-project-dialog.png)

3.  Oluştur’a tıklayın

## <a name="view-project-details"></a>Proje ayrıntılarını görüntüle

Özel Translator giriş sayfası ilk 10 projeleri çalışma alanınızda gösterir. Proje adı, dil çifti, kategori, durum ve BLEU puanı gösterir.

Bir proje seçtikten sonra aşağıdaki proje sayfada görürsünüz:

- CategoryID: CategoryID Workspaceıd, proje etiket ve kategori kodu birleştirilerek oluşturulur. Translator metin API'si ile CategoryID özel çevirileri almak için kullanın.

- Train düğmesi: Başlamak için bu düğmeyi kullanın. bir [modeli](how-to-train-model.md).

- Belgeler düğmesi ekleyin: Bu düğmeyi kullanın [belgeleri karşıya yüklemesine](how-to-upload-document.md).

- Filtre belgeleri düğmesi: Filtre ve belirli belge araması için bu düğmeyi kullanın.

    ![Proje ayrıntılarını görüntüle](media/how-to/how-to-view-project.png)

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi [arama, düzenleme, projesini silmek nasıl](how-to-search-edit-delete-projects.md).
- Bilgi [belgeyi karşıya yükleme](how-to-upload-document.md) çeviri modellerini oluşturmak için.
