---
title: Excel eklentisi Machine Learning Web Hizmetleri için | Microsoft Docs
description: Azure Machine Learning Web hizmetlerini doğrudan Excel'de herhangi bir kod yazmak zorunda kalmadan nasıl kullanacağınızı.
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.author: yahajiza
manager: hjerez
editor: cgronlun
ms.assetid: 9618079d-502f-4974-a3e2-8f924042a23f
ms.service: machine-learning
ms.component: studio
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 2/1/2018
ms.openlocfilehash: 68e2f72dfd8cc58d42263f4b6378d89304aaaa4d
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34834201"
---
# <a name="excel-add-in-for-azure-machine-learning-web-services"></a>Azure Machine Learning web hizmetleri için Excel Eklentisi
Excel web hizmetleri herhangi bir kod yazmak zorunda kalmadan doğrudan çağırmak kolaylaştırır.

## <a name="steps-to-use-an-existing-web-service-in-the-workbook"></a>Çalışma kitabını bir varolan web hizmetini kullanmak için adımları

1. Açık [örnek Excel dosyası](http://aka.ms/amlexcel-sample-2), Excel eklentisi ve Titanic üzerinde yolcu hakkındaki verileri içerir.
2. Web hizmeti tıklayarak seçin-"Titanic hayatta bir göstergesi olduğu (Excel Eklentisi örneği) [Sonuç]" Bu örnekte.
   
    ![Web hizmetini seçin][01]
3. Bu, olanak sürer **Predıct** bölümü.  Bu çalışma kitabı zaten örnek veri içeriyor, ancak için boş bir çalışma kitabını Excel'de bir hücre seçin ve'ı tıklatın **örnek verileri kullanın**.
4. Üst bilgileri ile verileri seçin ve giriş verilerini aralığı simgesini tıklatın.  "Verilerimin üstbilgileri var" kutunun işaretli olduğundan emin olun.
5. Altında **çıkış**, olması için örneğin "H1" Buraya çıkış istediğiniz hücre sayısını girin.
6. Tıklatın **tahmin**.
   
    ![Bölüm tahmin etme][02]

Bir web hizmetini dağıtma veya var olan bir Web hizmetini kullanın. Bir web hizmeti dağıtma hakkında daha fazla bilgi için bkz: [gözden geçirme adım 5: Azure Machine Learning Web hizmetini dağıtma](walkthrough-5-publish-web-service.md).

Web hizmeti için API anahtarı edinin. Gerçekleştirdiğiniz burada yeni Machine Learning web hizmeti bir Klasik Machine Learning web hizmeti olup yayımlanan Bu eylem bağlıdır.

**Klasik web hizmetini kullanın** 

1. Machine Learning Studio'da tıklatın **WEB Hizmetleri** bölümünde sol bölmede ve web hizmeti seçin.
   
    ![Bir Web hizmeti Studio seçin][04]
2. Web hizmeti API anahtarını kopyalayın.
   
    ![Studio API anahtarı][05]
3. Üzerinde **PANO** web hizmeti için sekmesini tıklatın, **istek/yanıt** bağlantı.
4. Ara **istek URI'si** bölümü.  Kopyalayın ve URL kaydedin.

> [!NOTE]
> Oturumu açmak artık mümkündür [Azure Machine Learning Web Hizmetleri](https://services.azureml.net) Klasik Machine Learning web hizmeti için API anahtarını elde etmek üzere portalı.
> 
> 

**Yeni bir web hizmetini kullanın**

1. İçinde [Azure Machine Learning Web Hizmetleri](https://services.azureml.net) portal tıklatın **Web Hizmetleri**, web hizmetinizi seçin. 
2. Tıklatın **tüketen**.
3. Ara **temel tüketim bilgileri** bölümü. Kopyalayıp kaydedin **birincil anahtar** ve **istek-yanıt** URL.

## <a name="steps-to-add-a-new-web-service"></a>Yeni bir web hizmeti ekleme adımları

1. Bir web hizmetini dağıtma veya var olan bir Web hizmetini kullanın. Bir web hizmeti dağıtma hakkında daha fazla bilgi için bkz: [gözden geçirme adım 5: Azure Machine Learning Web hizmetini dağıtma](walkthrough-5-publish-web-service.md).
2. Tıklatın **tüketen**.
3. Ara **temel tüketim bilgileri** bölümü. Kopyalayıp kaydedin **birincil anahtar** ve **istek-yanıt** URL.
4. Excel'de Git **Web Hizmetleri** bölümüne (içinde olup olmadığını **Predıct** bölümünde, web hizmetleri listesine dönmek için geri oku tıklatın).
   
    ![Web hizmeti seçimi gidin][03]
5. Tıklatın **Web hizmeti Ekle**.
6. Eklenti metin kutusu etiketli Excel'e URL'sini yapıştırın **URL**.
7. API/birincil anahtarı etiketli metin kutusuna yapıştırın **API anahtarı**.
8. **Ekle**'ye tıklayın.
   
    ![Klasik Web hizmeti URL'sini ve API anahtarı.][06]
9. Web hizmeti kullanmak için "varolan web hizmeti kullanmak için adımlar." önceki yönergeleri izleyin

## <a name="sharing-your-workbook"></a>Çalışma kitabınız paylaşımı
Çalışma kitabınız kaydederseniz, eklediğiniz web hizmetleri için API/birincil anahtar da kaydedilir. Bu çalışma kitabını yalnızca güvendiğiniz kişilerle paylaşması gerekir anlamına gelir.

Herhangi bir sorunuz aşağıdaki Açıklama bölümünde veya üzerinde isteyin bizim [Forumu](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).

[01]: ./media/excel-add-in-for-web-services/image1.png
[02]: ./media/excel-add-in-for-web-services/image2.png
[03]: ./media/excel-add-in-for-web-services/image3.png
[04]: ./media/excel-add-in-for-web-services/image4.png
[05]: ./media/excel-add-in-for-web-services/image5.png
[06]: ./media/excel-add-in-for-web-services/image6.png
