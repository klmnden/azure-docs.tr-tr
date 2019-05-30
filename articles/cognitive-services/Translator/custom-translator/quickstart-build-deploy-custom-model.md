---
title: 'Hızlı Başlangıç: Oluşturmanızı, dağıtmanızı ve özel bir model - özel Translator kullanın'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, özel Translator'ı kullanarak bir çeviri sistemi oluşturma, adım adım sürecinden geçer.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: v-pawal
ms.topic: quickstart
ms.openlocfilehash: aa38bbf211c9d62bcb9051a7ddc7f7d404add585
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66389621"
---
# <a name="quickstart-build-deploy-and-use-a-custom-model-for-translation"></a>Hızlı Başlangıç: Oluşturmanızı, dağıtmanızı ve özel bir model için çeviri kullanın

Bu makalede, bir çeviri sistem özel Translator ile oluşturmak için adım adım yönergeler sağlar.

## <a name="prerequisites"></a>Önkoşullar

1. Kullanılacak [özel Translator](https://portal.customtranslator.azure.ai) Portal, ihtiyaç duyacağınız bir [Microsoft hesabı](https://signup.live.com) veya [Azure AD hesabı](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) (Azure üzerinde barındırılan kuruluş hesabı) oturum açmak için.

2. Azure portal aracılığıyla Translator Text API aboneliği. Çalışma alanınızda özel Translator ile ilişkilendirmek için Translator Text API aboneliği anahtarı gerekir. Bkz: [Translator metin çevirisi API'si için kaydolma](https://docs.microsoft.com/azure/cognitive-services/translator/translator-text-how-to-signup).

3. Yukarıdaki iki sahip olduğunuzda, oturum [özel Translator](https://portal.customtranslator.azure.ai) portalı. Bir kez özel Translator portalında çalışma alanınızla Microsoft Translator Text API aboneliği anahtarınızı burada ilişkilendirebilirsiniz ayarları sayfasına gidin.

## <a name="create-a-project"></a>Proje oluşturma

Özel Translator portal giriş sayfasında, yeni proje tıklayın. İletişim kutusunda, istenen proje adınız, dil çifti ve kategori yanı sıra diğer ilgili alanları girebilirsiniz. Ardından, projenizi kaydedin. Daha fazla bilgi için ziyaret [proje oluştur](how-to-create-project.md).

![Proje oluşturma](media/quickstart/ct-how-to-create-project.png)


## <a name="upload-documents"></a>Belgeyi Karşıya Yükle

Ardından, karşıya yükleme [eğitim](training-and-model.md#training-dataset-for-custom-translator), [ayarlama](training-and-model.md#tuning-dataset-for-custom-translator) ve [test](training-and-model.md#testing-dataset-for-custom-translator) belge kümeleri. Her ikisi de yükleyebilirsiniz [paralel](what-are-parallel-documents.md) ve birleşik belgeleri. Ayrıca yükleyebilirsiniz [sözlük](what-is-dictionary.md).

Belgeler sekmesinden veya belirli bir projenin sayfasından belgeler karşıya yükleyebilirsiniz.

![Belgeyi Karşıya Yükle](media/quickstart/ct-how-to-upload.png)

Belgeler karşıya yüklenirken (Eğitimi, ayarlama veya test) belge türü ve dili çifti seçin. Paralel belgeler karşıya yüklenirken, ayrıca bir belge adı belirtmeniz gerekir. Daha fazla bilgi için ziyaret [karşıya yükleme belge](how-to-upload-document.md).

## <a name="create-a-model"></a>Bir model oluşturma

Gerekli tüm belgeler karşıya yüklendiğinde bir sonraki adım, model oluşturmaktır.

Oluşturduğunuz projeyi seçin. Bu proje ile dil çifti paylaşmak, yüklediğiniz tüm belgeleri görürsünüz. Modelinizde dahil edilmesini istediğiniz belgeleri seçin. Seçebileceğiniz [eğitim](training-and-model.md#training-dataset-for-custom-translator), [ayarlama](training-and-model.md#tuning-dataset-for-custom-translator), ve [test](training-and-model.md#testing-dataset-for-custom-translator) yalnızca verileri eğitim ve özel Translator otomatik olarak izin seçin veya veri ayarlama derleyip test etmeye modeliniz için ayarlar.

![Bir model oluşturma](media/quickstart/ct-how-to-train.png)

İstenen belgelerinizi seçerek bitirdiğinizde, modelinizi oluşturmak ve eğitim başlatmak için Model oluşturma düğmesine basın. Eğitim ve modelleri sekmede eğittiğimize tüm modelleri ayrıntılarını durumunu görebilirsiniz.

Daha fazla bilgi için ziyaret [Model oluşturma](how-to-train-model.md).

## <a name="analyze-your-model"></a>Modelinizi analiz edin

Eğitim başarıyla tamamlandıktan sonra sonuçları inceleyebilir. BLEU puanı, çeviri kalitesini belirten bir unsurdur. "Test" sekmesine gidin ve "Sistem sonuçları" tıklayarak test sağlanan çevirileri özel modelinizi yapılan çevirileri el ile de karşılaştırabilirsiniz Bu çevirileri birkaçını el ile İnceleme sisteminiz tarafından üretilen çeviri kalitesi hakkında fikir verir. Daha fazla bilgi için ziyaret [sistem Test sonuçları](how-to-view-system-test-results.md).

## <a name="deploy-a-trained-model"></a>Eğitilen model dağıtma

Eğitilen model dağıtmaya hazır olduğunuzda "Dağıt" düğmesini tıklatın. Bir dağıtılan model başına proje sahip olabilir ve dağıtımınızın durumunu Durum sütununda görüntüleyebilirsiniz. Daha fazla bilgi için ziyaret [Model dağıtımı](how-to-view-system-test-results.md#deploy-a-model)

![Eğitilen model dağıtma](media/quickstart/ct-how-to-deploy.png)

## <a name="use-a-deployed-model"></a>Dağıtılan bir modelde kullanın

Microsoft Translator dağıtılan modellerinde erişilebilir [CategoryID belirterek Text API V3](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl). Translator metin çevirisi API'si hakkında daha fazla bilgi bulunabilir [API Başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference) Web sayfası.

## <a name="next-steps"></a>Sonraki adımlar

- Git hakkında bilgi edinin [özel Translator çalışma ve projelerinizi yönetin](workspace-and-project.md).
