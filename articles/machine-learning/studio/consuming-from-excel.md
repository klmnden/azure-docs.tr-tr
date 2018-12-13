---
title: Excel'de - Azure Machine Learning Studio Web hizmetini kullanma | Microsoft Docs
description: Azure Machine Learning Studio web Hizmetleri, kod yazmaya gerek kalmadan doğrudan Excel'den çağırın kolaylaştırır.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: seodec18
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: 3f3cdd2f-1816-487e-ab78-530e01e9788f
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: ad7eae16c2933790aefba3cee1551be29ee457be
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53276936"
---
# <a name="consuming-an-azure-machine-learning-studio-web-service-from-excel"></a>Bir Azure Machine Learning Studio Web hizmetini Excel'den kullanma

 Azure Machine Learning Studio web Hizmetleri, kod yazmaya gerek kalmadan doğrudan Excel'den çağırın kolaylaştırır.

Excel 2013 (veya üzeri) veya Excel Online kullanıyorsanız, sonra Excel kullanmanızı öneririz, [Excel eklentisi](excel-add-in-for-web-services.md).



## <a name="steps"></a>Adımlar
Web hizmeti yayımlama. [Bu sayfa](walkthrough-5-publish-web-service.md) nasıl yapılacağı açıklanır. Şu anda Excel çalışma kitabını özelliği yalnızca tek bir çıkış (diğer bir deyişle, tek bir Puanlama etiketi) sahip istek/yanıt hizmetler için desteklenir. 

Bir web hizmeti oluşturduktan sonra tıklayarak **WEB Hizmetleri** studio sol tarafında bölümüne ve ardından web hizmetini Excel'den kullanma.

**Klasik Web hizmeti**

1. Üzerinde **PANO** web hizmeti için bir satır için sekmesinde **istek/yanıt** hizmeti. Bu hizmet, tek bir çıkış olsaydı, görmelisiniz **Excel çalışma kitabını indirin** söz konusu satırdaki bağlantı.
   
    ![][1]
2. Tıklayarak **Excel çalışma kitabını indirin**.

**Yeni Web hizmeti**

1. Azure Machine Learning Web hizmetini portalında **Tüket**.
2. Kullanım sayfası, şirket içinde **Web hizmeti tüketimi seçeneklerini** bölümünde, Excel simgesine tıklayın.

**Çalışma kitabını kullanmaya**

1. Çalışma kitabını açın.
2. Bir güvenlik uyarısı görüntülenmez; tıklayarak **düzenlemeyi etkinleştir** düğmesi.
   
    ![][2]
3. Bir güvenlik uyarısı görüntülenir. Tıklayarak **içeriği etkinleştir** makroları elektronik tablonuz üzerinde çalıştırmak için düğme.
   
    ![][3]
4. Makrolar etkinleştirildikten sonra bir tablo oluşturulur. Mavi olan sütunlarda gerekli RRS web hizmeti giriş olarak veya **parametreleri**. RRS hizmet çıktısını Not **tahmin edilen değerler** yeşil. Belirli bir satır için tüm sütunları dolduğunda, çalışma kitabına otomatik olarak Puanlama API'yi çağıran ve puanlanmış sonuçların görüntüler.
   
    ![][4]
5. Birden fazla satır puanlamak için dolgu verileri ve tahmin edilen değerler ikinci satırı oluşturulur. Aynı anda birden çok satır bile yapıştırabilirsiniz.

Verileri görselleştirme yardımcı olmak için tahmin edilen değerleri (grafikleri power map, koşullu biçimlendirme, vb.) Excel özelliklerinden herhangi birini kullanabilirsiniz.    

## <a name="sharing-your-workbook"></a>Çalışma kitabınızı paylaşma
İş için makrolar, API anahtarınızı elektronik bir parçası olmalıdır. Bu çalışma kitabını yalnızca varlık/güvenilir kişiler ile paylaşmalıdır anlamına gelir.

## <a name="automatic-updates"></a>Otomatik güncelleştirmeler
Bu iki durumda bir RRS çağrısıyla yapılır:

1. İlk kez bir satır sahip içerik tümünde kendi **parametreleri**
2. İstediğiniz herhangi bir zaman **parametreleri** tüm olan bir satır değişiklikleri kendi **parametreleri** girildi.

[1]: ./media/consuming-from-excel/excellink.png
[2]: ./media/consuming-from-excel/enableeditting.png
[3]: ./media/consuming-from-excel/enablecontent.png
[4]: ./media/consuming-from-excel/sampletable.png
