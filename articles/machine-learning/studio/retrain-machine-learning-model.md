---
title: "Machine Learning modelini çağırma | Microsoft Docs"
description: "Bir model yeniden eğitme ve Azure Machine Learning ile yeni eğitilen modelini kullanmak için Web hizmetini güncelleştirmek hakkında bilgi edinin."
services: machine-learning
documentationcenter: 
author: garyericson
manager: raymondl
editor: 
ms.assetid: d1cb6088-4f7c-4c32-94f2-f7523dad9059
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: raymondl
ms.openlocfilehash: 614517c5b9a54fc11cf0b8f8b6e298b9aec5cf76
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="retrain-a-machine-learning-model"></a>Machine Learning modelini çağırma
Azure Machine Learning makine öğrenimi modellerinin operationalization sürecinin bir parçası olarak, modelinizi eğitilmiş ve kaydedilir. Ardından, predicative bir Web hizmeti oluşturmak için kullanabilirsiniz. Web hizmeti web siteleri, panolar ve mobil uygulamaları tüketilebilir. 

Machine Learning kullanarak oluşturduğunuz modelleri genellikle statik değildir. Yeni veriler kullanılabilir olduğunda ya da kendi veri tüketici API varsa, model retrained gerekir. 

Yeniden eğitme sık oluşabilir. Program yeniden eğitme API özelliğiyle program aracılığıyla yeniden eğitme API'lerini kullanarak modeli yeniden eğitme ve Web hizmeti ile yeni eğitilen model güncelleştirin. 

Bu belge yeniden eğitme işlemini açıklar ve yeniden eğitme API'lerini kullanmayı gösterir.

## <a name="why-retrain-defining-the-problem"></a>Neden yeniden eğitme: sorun tanımlama
Makine öğrenimi eğitim işleminin bir parçası olarak, bir model veri kümesini kullanarak eğitildi. Machine Learning kullanarak oluşturduğunuz modelleri genellikle statik değildir. Yeni veriler kullanılabilir olduğunda ya da kendi veri tüketici API varsa, model retrained gerekir.

Bu senaryolarda, siz veya Apı'lerinizi tüketici kullanıcıların kendi verilerini kullanarak modeli bir kerelik veya normal temelinde yeniden eğitme bir istemci oluşturmak izin vermek için kolay bir yol programlı bir API sağlar. Bunlar daha sonra yeniden eğitme sonuçlarını değerlendirmek ve yeni eğitilen modelini kullanmak için Web hizmeti API'sine güncelleştirin.

> [!NOTE]
> Varolan eğitim denemenizi ve yeni Web hizmeti varsa, aşağıdaki bölümde belirtildiği izlenecek yol aşağıdaki yerine var olan bir Tahmine dayalı Web hizmetini Retrain denetlemek isteyebilirsiniz.
> 
> 

## <a name="end-to-end-workflow"></a>Uçtan uca iş akışı
İşlemi aşağıdaki bileşenleri içerir: A eğitim denemenizi ve Tahmine dayalı denemeye bir Web hizmeti olarak yayımladı. Eğitilen modeli yeniden eğitme etkinleştirmek için eğitim denemenizi çıktısı bir modeli, Web hizmetiyle yayımlanması gerekir. Bu API yeniden eğitme için modeline erişim sağlar. 

Aşağıdaki adımlar, hem yeni hem de klasik Web Hizmetleri için geçerlidir:

İlk Tahmine dayalı Web hizmeti oluşturun:

* Eğitim denemenizi oluşturma
* Tahmine dayalı web deneme oluşturma
* Tahmine dayalı web hizmetini dağıtma

Web hizmeti yeniden eğitme:

* Yeniden eğitme için izin vermek için eğitim denemenizi güncelleştir
* Yeniden eğitme web hizmetini dağıtma
* Modeli yeniden eğitme için toplu yürütme hizmeti kodu kullanın

Önceki bir gözden geçirme adımları için bkz: [yeniden eğitme Machine Learning modellerini programlama aracılığıyla](retrain-models-programmatically.md).

> [!NOTE] 
> Yeni bir web hizmeti dağıtmak için yeterli izinleri olan Abonelikteki, web hizmetini dağıtma olmalıdır. Daha fazla bilgi için [Azure Machine Learning Web Hizmetleri Portalı'nı kullanarak bir Web hizmetini yönetmek](manage-new-webservice.md). 

Klasik Web hizmeti dağıttıysanız:

* Tahmine dayalı Web hizmetinde yeni bir uç noktası oluşturma
* URL düzeltme ve kodu alın
* Retrained modeli yeni uç noktası için düzeltme eki URL'yi kullanın 

Önceki bir gözden geçirme adımları için bkz: [Klasik Web hizmeti yeniden eğitme](retrain-a-classic-web-service.md).

Klasik Web hizmeti yeniden eğitme zorluklar çalıştırırsanız, bkz: [bir Azure Machine Learning Klasik Web hizmeti yeniden eğitme sorun giderme](troubleshooting-retraining-models.md).

Yeni Web hizmeti dağıttıysanız:

* Azure Resource Manager hesabınızda oturum açın
* Web hizmeti tanımının Al
* Web hizmeti tanımının JSON olarak dışarı aktarma
* Başvuru güncelleştirme `ilearner` JSON blob
* Bir Web hizmeti tanımının JSON içe
* Yeni Web hizmeti tanımının Web hizmetiyle güncelleştirme

Önceki bir gözden geçirme adımları için bkz: [makine öğrenme yönetim PowerShell cmdlet'lerini kullanarak yeni Web hizmeti yeniden eğitme](retrain-new-web-service-using-powershell.md).

Klasik Web hizmeti yeniden eğitme yukarı ayarlama işlemi şu adımları içerir:

![Yeniden eğitme işlemine genel bakış][1]

Yeni Web hizmeti yeniden eğitme yukarı ayarlama işlemi şu adımları içerir:

![Yeniden eğitme işlemine genel bakış][7]

## <a name="other-resources"></a>Diğer Kaynaklar
* [Azure Data Factory ile güncelleştirme Azure Machine Learning yeniden eğitme ve modelleri](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
* [Birçok Machine Learning modellerini ve web hizmeti uç noktalarını PowerShell kullanarak bir deneme oluşturma](create-models-and-endpoints-with-powershell.md)
* [AML yeniden eğitme modelleri kullanarak API'lerini](https://www.youtube.com/watch?v=wwjglA8xllg) video gösterir, Azure Machine Learning ile oluşturulan Machine Learning modelleri yeniden eğitme nasıl yeniden eğitme API'lerini ve PowerShell kullanarak.

<!--image links-->
[1]: ./media/retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png

