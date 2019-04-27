---
title: Excel Web hizmetini kullanma
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio web Hizmetleri, kod yazmaya gerek kalmadan doğrudan Excel'den çağırın kolaylaştırır.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 02/01/2018
ms.openlocfilehash: ef1d8f1a72c5936ff661636c4c51acf439a0a5ea
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60773799"
---
# <a name="consuming-an-azure-machine-learning-studio-web-service-from-excel"></a>Bir Azure Machine Learning Studio Web hizmetini Excel'den kullanma

 Azure Machine Learning Studio web Hizmetleri, kod yazmaya gerek kalmadan doğrudan Excel'den çağırın kolaylaştırır.

Excel 2013 (veya üzeri) veya Excel Online kullanıyorsanız, sonra Excel kullanmanızı öneririz, [Excel eklentisi](excel-add-in-for-web-services.md).



## <a name="steps"></a>Adımlar
Web hizmeti yayımlama. [Öğretici 3: Kredi riski model dağıtma](tutorial-part3-credit-risk-deploy.md) nasıl yapılacağı açıklanır. Şu anda Excel çalışma kitabını özelliği yalnızca tek bir çıkış (diğer bir deyişle, tek bir Puanlama etiketi) sahip istek/yanıt hizmetler için desteklenir. 

Bir web hizmeti oluşturduktan sonra tıklayarak **WEB Hizmetleri** studio sol tarafında bölümüne ve ardından web hizmetini Excel'den kullanma.

**Klasik Web hizmeti**

1. Üzerinde **PANO** web hizmeti için bir satır için sekmesinde **istek/yanıt** hizmeti. Bu hizmet, tek bir çıkış olsaydı, görmelisiniz **Excel çalışma kitabını indirin** söz konusu satırdaki bağlantı.

    ![Studio Web hizmeti portalını kullanarak çalışma kitabını indirin](./media/consuming-from-excel/excellink.png)
2. Tıklayarak **Excel çalışma kitabını indirin**.

**Yeni Web hizmeti**

1. Azure Machine Learning Web hizmetini portalında **Tüket**.
2. Kullanım sayfası, şirket içinde **Web hizmeti tüketimi seçeneklerini** bölümünde, Excel simgesine tıklayın.

**Çalışma kitabını kullanmaya**

1. Çalışma kitabını açın.
2. Bir güvenlik uyarısı görüntülenmez; tıklayarak **düzenlemeyi etkinleştir** düğmesi.

    ![Korumalı Görünüm güvenlik uyarısını kaldırmaya düzenlemeyi etkinleştir](./media/consuming-from-excel/enableeditting.png)
3. Bir güvenlik uyarısı görüntülenir. Tıklayarak **içeriği etkinleştir** makroları elektronik tablonuz üzerinde çalıştırmak için düğme.

    ![Makroları devre dışı bırakma güvenlik uyarısını kapatmak içeriği etkinleştir](./media/consuming-from-excel/enablecontent.png)
4. Makrolar etkinleştirildikten sonra bir tablo oluşturulur. Mavi olan sütunlarda gerekli RRS web hizmeti giriş olarak veya **parametreleri**. RRS hizmet çıktısını Not **tahmin edilen değerler** yeşil. Belirli bir satır için tüm sütunları dolduğunda, çalışma kitabına otomatik olarak Puanlama API'yi çağıran ve puanlanmış sonuçların görüntüler.

    ![Tablo parametresi giriş ve sonuç için tahmin edilen değerler](./media/consuming-from-excel/sampletable.png)
5. Birden fazla satır puanlamak için dolgu verileri ve tahmin edilen değerler ikinci satırı oluşturulur. Aynı anda birden çok satır bile yapıştırabilirsiniz.

Verileri görselleştirme yardımcı olmak için tahmin edilen değerleri (grafikleri power map, koşullu biçimlendirme, vb.) Excel özelliklerinden herhangi birini kullanabilirsiniz.

## <a name="sharing-your-workbook"></a>Çalışma kitabınızı paylaşma
İş için makrolar, API anahtarınızı elektronik bir parçası olmalıdır. Bu çalışma kitabını yalnızca varlık/güvenilir kişiler ile paylaşmalıdır anlamına gelir.

## <a name="automatic-updates"></a>Otomatik güncelleştirmeler
Bu iki durumda bir RRS çağrısıyla yapılır:

1. İlk kez bir satır sahip içerik tümünde kendi **parametreleri**
2. İstediğiniz herhangi bir zaman **parametreleri** tüm olan bir satır değişiklikleri kendi **parametreleri** girildi.