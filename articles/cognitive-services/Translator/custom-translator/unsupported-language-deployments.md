---
title: Desteklenmeyen dil dağıtımları - özel Translator
titleSuffix: Azure Cognitive Services
description: Desteklenmeyen dil çiftlerinde özel Translator'ı dağıtma
services: cognitive-services
author: rajdeep-in
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 04/24/2019
ms.author: v-pawal
ms.openlocfilehash: 0938ba2e839be603c557cc2a87dd0c5aa1dfe1e3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66390576"
---
# <a name="unsupported-language-deployments"></a>Desteklenmeyen dil dağıtımları

<!--Custom Translator provides the highest-quality translations possible using the latest techniques in neural machine learning. While Microsoft intends to make neural training available in all languages, there are some limitations that prevent us from being able to offer neural machine translation in all language pairs.-->  

Microsoft Translator Hub'ın yakın zamanda kullanımdan ile Microsoft şu anda hub'ı aracılığıyla dağıtılan tüm modelleri undeploying. Birçoğunuzun olan dil çiftleri özel Translator içinde desteklenmeyen hub'ında dağıtılan modelleri vardır.  Bu durumda kullanıcıların içeriklerini çevirmek için hiçbir sunduğumuz sahip istemezsiniz.

Artık desteklenmeyen Modellerinizi özel Translator aracılığıyla dağıtmanıza olanak tanıyan bir işlem var.  Bu işlem en son V3 API'sini kullanarak içeriği çevirmek devam etmenize olanak tanır.  Bu modeller, bunları dağıtımını kaldırmayı seçin veya dil çifti özel Translator içinde kullanılabilir duruma kadar barındırılacak.  Bu makalede, desteklenmeyen dil çiftleriyle modelleri dağıtma işlemi açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

Sırayla Modellerinizi dağıtım için aday olmak için aşağıdaki ölçütleri karşılaması gerekir:
* Modeli içeren proje hub'dan geçiş aracını kullanarak özel Translator'a geçirilmiştir gerekir.  İşlem geçirme projeleri ve çalışma alanları için bulunabilir [burada](how-to-migrate.md).
* Geçiş gerçekleştiğinde model dağıtılmış durumda olması gerekir.  
* Model dili çiftinin özel Translator içindeki bir desteklenmeyen dili çifti olmalıdır.  Bir dil İngilizce'den ya da desteklenir ancak çifti İngilizce içermez dil çiftleri Desteklenmeyen dil dağıtımlar için bir adaydır.  Örneğin, Hub modeli için bir Fransızca, Almanca dil çifti için bir desteklenmeyen dili çifti bile ancak Fransızca İngilizce ve Almanca olan İngilizce için desteklenen dil çifti olarak kabul edilir.

## <a name="process"></a>Process
Dağıtım için aday niteliği taşıyan Hub'ından modelleri geçirdikten sonra bunları şuraya giderek bulabilirsiniz **ayarları** çalışma alanınızı ve göreceğiniz sayfanın sonuna kadar kaydırma için sayfasına bir **desteklenmiyor Translator Hub eğitimleri** bölümü.  Bu bölümde, yalnızca yukarıda açıklanan önkoşulları karşılaması projeler varsa görüntülenir.

![Hub'ından geçirme](media/unsupported-language-deployments/unsupported-translator-hub-trainings.jpg)

İçinde **desteklenmeyen Translator Hub eğitimleri** Seçimi sayfasında, **eğitimleri istenmemiş** sekmesi dağıtım için uygun olan modeller içerir.  Modelleri dağıtmak ve bir istek göndermek istediğiniz seçin.   30 Nisan dağıtım son tarihinden önce dağıtım için istediğiniz sayıda model seçebilirsiniz.
 
![Hub'ından geçirme](media/unsupported-language-deployments/unsupported-translator-hub-trainings-list.jpg)

Gönderilen sonra model üzerinde kullanılabilir olmayacak **eğitimleri istenmemiş** sekmesini ve bunun yerine görünür **eğitimleri istenen** sekmesi.  İstediğiniz zaman, istenen eğitimleri görüntüleyebilirsiniz.

![Hub'ından geçirme](media/unsupported-language-deployments/request-unsupported-trainings.jpg) 

## <a name="whats-next"></a>Sırada ne var?

Dağıtım için seçtiğiniz modelleri, hub'ı kullanımdan kaldırıldı ve tüm modelleri dağıtılmamış sonra kaydedilir.  Desteklenmeyen model dağıtımı için istekleri göndermek için 24 Mayıs kadar sahip.  Bu noktada, Translator V3 API aracılığıyla erişilebilir Bu modeller üzerinde 15 Haziran dağıtacağız.  Ayrıca, bunlar 1 Temmuz'a kadar V2 API aracılığıyla kullanılabilir.  

Kullanımdan kaldırma Hub denetiminin önemli tarihleri hakkında daha fazla bilgi için [burada](https://www.microsoft.com/translator/business/hub/).
Bir kez dağıtılan, normal barındırma ücretleri geçerli olacaktır.  Bkz: [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/) Ayrıntılar için.  

Standart özel Translator modeller, çok bölgeli barındırma ücretleri uygulanmaz şekilde Hub modelleri yalnızca tek bir bölgede kullanılabilir olacak.  Uygulama dağıtıldıktan sonra Hub modelinizin dağıtımı geri geçirilen özel Translator proje aracılığıyla herhangi bir zamanda mümkün olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir model eğitip](how-to-train-model.md).
- Dağıtılan özel çeviri modeliniz aracılığıyla kullanmaya başlama [Microsoft Translator Text API v3 sürümüne](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl).
