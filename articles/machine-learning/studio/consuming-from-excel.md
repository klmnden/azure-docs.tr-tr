---
title: Bir Machine Learning Web hizmeti Excel'den kullanma | Microsoft Docs
description: Bir Azure Machine Learning Web hizmeti Excel'den kullanma
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.author: yahajiza
manager: hjerez
editor: cgronlun
ms.assetid: 3f3cdd2f-1816-487e-ab78-530e01e9788f
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/1/2018
ms.openlocfilehash: 14621e50a397bc1f1922a4c8fae638d6b42ab8ba
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34837074"
---
# <a name="consuming-an-azure-machine-learning-web-service-from-excel"></a>Bir Azure Machine Learning Web Hizmetini Excel’den kullanma
 Azure Machine Learning Studio, web hizmetleri doğrudan kod yazmaya gerek kalmadan Excel çağırmanıza kolaylaştırır.

Excel 2013 (veya üstü) veya Excel Online'da kullandığınız sonra Excel kullanmanızı öneririz [Excel eklentisi](excel-add-in-for-web-services.md).

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="steps"></a>Adımlar
Bir web hizmeti yayımlayın. [Bu sayfayı](walkthrough-5-publish-web-service.md) nasıl yapılacağını açıklar. Şu anda Excel çalışma kitabı özelliği yalnızca tek bir çıktı (diğer bir deyişle, tek bir Puanlama etiketi) sahip istek/yanıt Hizmetleri için desteklenir. 

Bir web hizmeti oluşturduktan sonra tıklayın **WEB Hizmetleri** bölümünde Studio'nun sol tarafta ve Excel'den kullanmak için web hizmeti seçin.

**Klasik Web hizmeti**

1. Üzerinde **PANO** web hizmeti için bir satır için sekme **istek/yanıt** hizmet. Bu hizmet tek bir çıktı olsaydı görmeniz gerekir **karşıdan Excel çalışma kitabı** o satırdaki bağlantı.
   
    ![][1]
2. Tıklayın **karşıdan Excel çalışma kitabı**.

**Yeni Web hizmeti**

1. Azure Machine Learning Web hizmeti Portalı'nda seçin **Tüket**.
2. Tüket sayfasında içinde **Web hizmeti tüketim seçenekleri** bölümünde, Excel simgesine tıklayın.

**Çalışma kitabı kullanma**

1. Çalışma kitabını açın.
2. Bir güvenlik uyarısı görüntülenmez; tıklayın **düzenlemeyi etkinleştir** düğmesi.
   
    ![][2]
3. Bir güvenlik uyarısı görüntülenmez. Tıklayın **içeriği etkinleştir** makroları, elektronik tablo üzerinde çalıştırmak için düğmesi.
   
    ![][3]
4. Makrolar etkinleştirildikten sonra bir tablo oluşturulur. Mavi olan sütunlarda gerekli giriş RRS web hizmeti olarak veya **parametreleri**. RRS hizmet çıktısını Not **ÖNGÖRÜLEN değerleri** yeşil. Belirli bir satır için tüm sütunları dolduğunda, çalışma kitabını otomatik olarak Puanlama API çağrılarının ve puanlanmış sonuçları görüntüler.
   
    ![][4]
5. Birden fazla satır Puanlama amacıyla dolgu ikinci satırın veriler ve tahmin edilen değerler ile oluşturulur. Aynı anda birden çok satır bile yapıştırabilirsiniz.

Verileri görselleştirin yardımcı olmak için tahmin edilen değerlere sahip (grafikleri, güç harita, koşullu biçimlendirme, vb.) Excel özelliklerinden herhangi birini kullanabilirsiniz.    

## <a name="sharing-your-workbook"></a>Çalışma kitabınız paylaşımı
Makrolar çalışmak API anahtarınıza elektronik tablonun parçası olması gerekir. Bu çalışma kitabını yalnızca varlıklar/güvendiğiniz kişilerle paylaşmalıdır anlamına gelir.

## <a name="automatic-updates"></a>Otomatik güncelleştirmeler
Bir RRS çağrı bu iki durumlarda yapılmıştır:

1. İlk kez bir satır var içerik tümünde kendi **parametreleri**
2. Herhangi birini zaman **parametreleri** tümüne sahip olan bir satır değişiklikleri kendi **parametreleri** girildi.

[1]: ./media/consuming-from-excel/excellink.png
[2]: ./media/consuming-from-excel/enableeditting.png
[3]: ./media/consuming-from-excel/enablecontent.png
[4]: ./media/consuming-from-excel/sampletable.png
