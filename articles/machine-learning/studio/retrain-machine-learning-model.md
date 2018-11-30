---
title: Azure Machine Learning Studio'da model - yeniden eğitme | Microsoft Docs
description: Modeli yeniden eğitme ve Azure Machine Learning'de yeni eğitim modeli kullanmak için Web hizmetini güncelleştirmek hakkında bilgi edinin.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=yahajiza, author=YasinMSFT)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: d1cb6088-4f7c-4c32-94f2-f7523dad9059
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.openlocfilehash: 1f2505eda232c518ccb24b32fad91fb36dd11b01
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52317934"
---
# <a name="retrain-an-azure-machine-learning-studio-model"></a>Bir Azure Machine Learning Studio modeli yeniden eğitme
Azure Machine Learning, machine learning modellerini, operasyonel hale getirme sürecinin bir parçası olarak, modelinizi eğitilmiş ve kaydedilir. Ardından, bir Tahmine dayalı Web hizmeti oluşturmak için kullanabilirsiniz. Web hizmeti web siteleri, panolar ve mobil uygulamalarda tüketilebilir. 

Machine Learning kullanarak oluşturduğunuz modelleri genellikle statik değildir. Yeni veriler kullanılabilir olduğunda ya da kendi veri tüketici API'si varsa, model eğitilebileceği gerekir. 

Yeniden eğitme sık gerçekleşebilir. Programlı olarak yeniden eğitme API özelliğiyle, program aracılığıyla yeniden eğitme API'lerini kullanarak modeli yeniden eğitme ve Web hizmeti ile yeni eğitilen modeli güncelleştirmek. 

Bu belge, yeniden eğitme işlemini açıklar ve yeniden eğitme API'lerini kullanmayı gösterir.

## <a name="why-retrain-defining-the-problem"></a>Neden yeniden eğitme: sorunu tanımlama
Makine öğrenimi eğitim işleminin bir parçası olarak, bir veri kümesini kullanarak bir model çalıştırılır. Machine Learning kullanarak oluşturduğunuz modelleri genellikle statik değildir. Yeni veriler kullanılabilir olduğunda ya da kendi veri tüketici API'si varsa, model eğitilebileceği gerekir.

Bu senaryolarda, programlı bir API, veya tüketici apı'lerinizi tek seferlik veya normal temelinde kendi verilerini kullanarak modeldeki eğitebilir bir istemci oluşturmak izin vermek için kullanışlı bir yol sağlar. Bunlar daha sonra yeniden eğitme sonuçları değerlendirin ve yeni eğitilen modelini kullanmak için Web hizmeti API'si güncelleştirin.

> [!NOTE]
> Bir eğitim denemesini mevcut ve yeni Web hizmeti varsa, aşağıdaki bölümde belirtildiği izlenecek yolu takip yerine var olan bir Tahmine dayalı Web hizmeti Retrain denetlemek isteyebilirsiniz.
> 
> 

## <a name="end-to-end-workflow"></a>Uçtan uca iş akışı
İşlem aşağıdaki bileşenleri içerir: bir eğitim denemesini ve öngörücü bir denemeye bir Web hizmeti olarak yayımlandı. Eğitilen bir modeli yeniden eğitme etkinleştirmek için eğitim denemesini eğitilen bir modelin çıktısı ile bir Web hizmeti olarak yayımlanması gerekir. Bu API yeniden eğitme için modele erişim sağlar. 

Hem yeni hem de klasik Web Hizmetleri için aşağıdaki adımları uygulayın:

İlk Tahmine dayalı Web hizmeti oluşturun:

* Eğitim denemenizi oluşturma
* Tahmine dayalı web deneme oluşturma
* Tahmine dayalı web hizmetini dağıtma

Web hizmetini yeniden eğitme:

* Yeniden eğitme için izin vermek için eğitim denemesini güncelleştir
* Yeniden eğitme web hizmetini dağıtma
* Modeli yeniden eğitme için toplu yürütme hizmeti kodu kullanın

Önceki bir gözden geçirme adımları için bkz: [yeniden eğitme Machine Learning modellerini programlama aracılığıyla](retrain-models-programmatically.md).

> [!NOTE] 
> Yeni bir web hizmetini dağıtmak için yeterli olan aboneliği, web hizmetini dağıtma olması gerekir. Daha fazla bilgi edinmek, [Azure Machine Learning Web Hizmetleri portalını kullanarak bir Web hizmetini yönetme](manage-new-webservice.md). 

Bir Klasik Web hizmetini dağıttıysanız:

* Tahmine dayalı Web hizmetinde yeni bir uç nokta oluşturma
* URL düzeltme ve kodu alma
* Yeni uç nokta retrained modeli işaret edecek şekilde düzeltme eki uygulama URL'sini kullanın. 

Önceki bir gözden geçirme adımları için bkz: [bir Klasik Web hizmetini yeniden eğitme](retrain-a-classic-web-service.md).

Klasik Web hizmetini yeniden eğitme zorluklarla çalıştırırsanız, bkz. [bir Azure Machine Learning Klasik Web hizmetini yeniden eğitme sorun giderme](troubleshooting-retraining-models.md).

Yeni Web hizmetini dağıttıysanız:

* Azure Resource Manager hesabınızda oturum açın
* Web servis tanımı Al
* Web hizmet tanımı JSON olarak dışarı aktarma
* Başvuru güncelleştirme `ilearner` JSON blob
* Web hizmet tanımı JSON dosyasını içe
* Yeni Web hizmeti tanımıyla Web hizmetini güncelleştirmek

Önceki bir gözden geçirme adımları için bkz: [Machine Learning Yönetimi PowerShell cmdlet'lerini kullanarak bir yeni Web hizmetini yeniden eğitme](retrain-new-web-service-using-powershell.md).

İşlem için bir Klasik Web hizmetini yeniden eğitme yukarı ayarlamak için aşağıdaki adımları içerir:

![Yeniden eğitme işlemine genel bakış][1]

Yeni Web hizmeti olarak yeniden eğitme yukarı ayarlama işlemi aşağıdaki adımları içerir:

![Yeniden eğitme işlemine genel bakış][7]

## <a name="other-resources"></a>Diğer Kaynaklar
* [Azure Data Factory ile yeniden eğitme ve güncelleştirme Azure Machine Learning modelleri](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
* [Çok sayıda Machine Learning modeli ve web hizmet uç noktaları, PowerShell kullanarak bir denemeden oluşturun.](create-models-and-endpoints-with-powershell.md)
* [AML yeniden eğitme modelleri kullanarak API'leri](https://www.youtube.com/watch?v=wwjglA8xllg) video gösterir, Azure Machine Learning'de oluşturulan Machine Learning modelleri yeniden eğitme nasıl yeniden eğitme API'lerini ve PowerShell kullanarak.

<!--image links-->
[1]: ./media/retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png

