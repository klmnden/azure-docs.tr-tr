---
title: Özel ses nedir? -Konuşma Hizmetleri
titlesuffix: Azure Cognitive Services
description: Özel ses markanız için tanınan, tür, tek bir ses oluşturmanızı sağlayan çevrimiçi araçları kümesidir. Ses dosyaları ve ilişkili döküm birkaç olan tüm kullanmaya başlamak için alır. Özel bir konuşmayı metne deneyimi oluşturmaya başlamak için aşağıdaki bağlantıları izleyin.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: erhopf
ms.openlocfilehash: 016dcf32f2f846e43362f17bc9f4627113908352
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67075143"
---
# <a name="get-started-with-custom-voice"></a>Özel Ses’i kullanmaya başlama

Özel ses markanız için tanınan, tür, tek bir ses oluşturmanızı sağlayan çevrimiçi araçları kümesidir. Ses dosyaları ve ilişkili döküm birkaç olan tüm kullanmaya başlamak için alır. Bir özel metin okuma deneyimi oluşturmaya başlamak için aşağıdaki bağlantıları izleyin.

## <a name="whats-in-custom-voice"></a>İçinde özel sesli nedir?

Özel ses başlatmadan önce bir Azure hesabı ve konuşma Hizmetleri aboneliği gerekir. Bir hesap oluşturduktan sonra verilerinizi hazırlayın, eğitmek ve Modellerinizi test, ses kalitesi değerlendirmek ve sonuç olarak, özel sesli model dağıtma.

Aşağıdaki diyagramda özel sesli portalını kullanarak bir özel sesli modeli oluşturmaya yönelik adımlar vurgulanır. Daha fazla bilgi için bağlantıları kullanın.

![Özel ses mimarisi diyagramı](media/custom-voice/custom-voice-diagram.png)

1.  [Abone olma ve proje oluşturma](#set-up-your-azure-account) - bir Azure hesabı oluşturun ve bir konuşma Hizmetleri aboneliği oluşturun. Bu birleşik abonelik konuşma metin, metin okuma, konuşma çevirisi ve özel ses portalı erişmenizi sağlar. Ardından, konuşma Hizmetleri aboneliğinize kullanarak ilk özel sesli projenizi oluşturun.

2.  [Verileri karşıya](how-to-custom-voice-create-voice.md#upload-your-datasets) -karşıya veri (ses ve metin) özel sesli portalı veya özel sesli API'sini kullanarak. Portalda, araştırmanıza ve Söyleniş puanları ve sinyal/gürültü oranları değerlendirin. Daha fazla bilgi için [özel ses için veri hazırlama](how-to-custom-voice-prepare-data.md).

3.  [Modelinizi eğitin](how-to-custom-voice-create-voice.md#build-your-custom-voice-model) – verilerinizi bir özel metin okuma sesli modeli oluşturmak için kullanın. Bir modeli farklı dillerde eğitebilirsiniz. Eğitim sonra modelinizi test etme ve sonucuyla memnun kalırsanız, model dağıtabilirsiniz.

4.  [Modelinizi](how-to-custom-voice-create-voice.md#create-and-use-a-custom-voice-endpoint) - metin okuma ses modeliniz için özel bir uç noktası oluşturun ve konuşma sentezi ürünleri, araçları ve uygulamaları için kullanın.

## <a name="set-up-your-azure-account"></a>Azure hesabınızı ayarlama

Özel bir model oluşturmak için özel konuşma tanıma Portalı'nı kullanmadan önce bir konuşma Hizmetleri aboneliği gereklidir. Azure'da bir konuşma Hizmetleri aboneliği oluşturmak için bu yönergeleri izleyin. Azure hesabınız yoksa yeni bir tane için kaydolabilirsiniz.  

Bir Azure hesabı ve bir konuşma Hizmetleri abonelik oluşturduktan sonra özel sesli portalında oturum açın ve aboneliğinizi bağlanmak gerekir.

1. Azure portalından konuşma Hizmetleri abonelik anahtarınızı alın.
2. Oturum [özel sesli portalı](https://aka.ms/custom-voice).
3. Aboneliğinizi seçin ve bir konuşma projesi oluşturun.
4. Başka bir konuşma aboneliğine geçiş yapmak istiyorsanız, üst gezintideki bulunan dişli simgesini kullanın.

> [!NOTE]
> Özel ses hizmeti, 30 günlük ücretsiz deneme sürümü anahtarı desteklemez. Bir F0 veya hizmetini kullanabilmeniz için önce Azure'da oluşturulan S0 anahtarı olması gerekir.

## <a name="how-to-create-a-project"></a>Bir proje oluşturma

Veri, modelleri, testleri ve uç noktaları düzenlenmiştir gibi içerik **projeleri** özel sesli portalında. Her proje, bir ülke/dili ve oluşturmak istediğiniz ses dinleyicilerinin özeldir. Örneğin, bir kadın sesi için İngilizce (en-US) Amerika Birleşik Devletleri'nde kullanan çağrı Merkezi'nin sohbet botlar için bir proje oluşturabilirsiniz.

İlk projenizi oluşturmak için Seç **Text-How-to-Speech/özel sesli** sekmesine ve ardından tıklayın **yeni proje**. Projenizi oluşturmak için Sihirbazı tarafından sağlanan yönergeleri izleyin. Bir projeyi oluşturduktan sonra dört sekmeleri görürsünüz: **Veri**, **eğitim**, **test**, ve **dağıtım**. Sağlanan bağlantıları kullanın [sonraki adımlar](#next-steps) her sekme kullanmayı öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

- [Özel ses verileri hazırlama](how-to-custom-voice.md)
- [Özel ses oluşturma](how-to-custom-voice-create-voice.md)
- [Kılavuzu: Ses örneklerinizi kaydedin](record-custom-voice-samples.md)
