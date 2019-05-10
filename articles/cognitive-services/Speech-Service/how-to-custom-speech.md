---
title: Özel konuşma - konuşma Hizmetleri ile çalışmaya başlama
titlesuffix: Azure Cognitive Services
description: Özel konuşma tanıma, değerlendirmek ve uygulamalarınızı, araçları ve ürünleri için Microsoft'un Konuşmayı metne doğruluğunu artırmak izin veren çevrimiçi araç kümesidir. Başlamak için geçen birkaç test ses dosyaları değildir. Özel bir konuşmayı metne deneyimi oluşturmaya başlamak için aşağıdaki bağlantıları izleyin.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: ab33feb1ffdbced193afaba8f52719b3c215652f
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65511092"
---
# <a name="what-is-custom-speech"></a>Özel konuşma tanıma nedir?

[Özel konuşma](https://aka.ms/custom-speech) değerlendirmek ve uygulamalarınızı, araçları ve ürünleri için Microsoft'un Konuşmayı metne doğruluğunu artırmak izin veren çevrimiçi araçları kümesidir. Başlamak için geçen birkaç test ses dosyaları değildir. Özel bir konuşmayı metne deneyimi oluşturmaya başlamak için aşağıdaki bağlantıları izleyin.

## <a name="whats-in-custom-speech"></a>Özel konuşma dilinde nedir?

Özel Konuşma ile herhangi bir şey yapmak için önce bir Azure hesabı ve konuşma Hizmetleri aboneliği gerekir. Bir hesap başladıktan sonra verilerinizi hazırlayın, eğitmek ve Modellerinizi test, tanıma kalitesini denetlemek, doğruluk değerlendirmek ve nihai olarak dağıtabilir ve özel konuşma metin modeli kullanır.

Bu diyagramda özel konuşma tanıma portalı parçaları vurgular. Her bir adım hakkında daha fazla bilgi için aşağıdaki bağlantıları kullanın.

![Özel konuşma tanıma portalı farklı bileşenleri vurgulanmaktadır.](./media/custom-speech/custom-speech-overview.png)

1. [Abone olma ve proje oluşturma](#set-up-your-azure-account) - bir Azure hesabı oluşturun ve konuşma Hizmetleri olma. Bu birleşik abonelik konuşma metin, metin okuma, konuşma çevirisi ve özel konuşma tanıma portalı erişmenizi sağlar. Ardından, konuşma Hizmetleri aboneliğinize kullanarak ilk özel konuşma projenizi oluşturun.

2. [Test verileri yükleyerek](how-to-custom-speech-test-data.md) -Microsoft'un konuşma uygulamalar, Araçlar ve ürünlerle sunan metne değerlendirmek için karşıya yükleme test verilerini (ses dosyaları).

3. [Tanıma kalite İnceleme](how-to-custom-speech-inspect-data.md) -özel konuşma tanıma portalı karşıya yüklenen ses kayıttan yürütme ve konuşma tanıma kalitesini test verilerini incelemek için kullanın. Nicel ölçümlerin için bkz: [verileri İnceleme](how-to-custom-speech-inspect-data.md).

4. [Doğruluk değerlendirmek](how-to-custom-speech-evaluate-data.md) -konuşma metin model doğruluğunu değerlendirin. Özel konuşma tanıma portalı sağlayacak bir *Word hata oranı*, ek eğitim gerekli olup olmadığını belirlemek için kullanılabilir. Doğrulukla memnun kalırsanız, konuşma tanıma hizmeti API'lerini doğrudan kullanabilirsiniz. Göreli bir ortalama, 5-%20, doğruluğunu artırmak istiyorsanız, kullanın **eğitim** portalında İnsan etiketli dökümleri ve ilgili metin gibi ek eğitim verileri yüklemek için sekmesinde.

5. [Modeli eğitme](how-to-custom-speech-train-model.md) - yazılı dökümler (10-1000 saat) sağlayarak konuşma metin modelinizi doğruluğunu artırmak ve ilgili metin (< 200 MB) ses yanı sıra test verileri. Bu veriler Konuşmayı metne modeli eğitmek için yardımcı olur. Eğitim sonra yeniden test et, sonucuyla memnun kalırsanız, modelinizi dağıtabilirsiniz.

6. [Modeli dağıtma](how-to-custom-speech-deploy-model.md) - konuşma metin modeliniz için özel bir uç noktası oluşturma ve uygulamalar, Araçlar veya ürünleri kullanın.

## <a name="set-up-your-azure-account"></a>Azure hesabınızı ayarlama

Özel bir model oluşturmak için özel konuşma tanıma Portalı'nı kullanmadan önce bir konuşma Hizmetleri aboneliği gereklidir. Standart bir konuşma Hizmetleri aboneliği oluşturmak için aşağıdaki yönergeleri izleyin: [Konuşma abonelik oluşturma](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#new-azure-account).

> [!NOTE]
> Lütfen standart (S0) abonelikleri oluşturduğunuzdan emin olun, ücretsiz deneme sürümü (F0) abonelikleri desteklenmez.

Bir Azure hesabı ve bir konuşma Hizmetleri abonelik oluşturduktan sonra özel konuşma portalında oturum açın ve aboneliğinizi bağlanmak gerekir.

1. Azure portalından konuşma Hizmetleri abonelik anahtarınızı alın.
2. Oturum açma için [özel konuşma tanıma portalı](https://aka.ms/custom-speech).
3. Bir konuşma projesi oluşturma ve üzerinde çalışmak istediğiniz aboneliği seçin.
4. Aboneliğinizi değiştirmek istiyorsanız, kullanın **dişli** simgesi bulunan üst gezintideki.

## <a name="how-to-create-a-project"></a>Bir proje oluşturma

Veri, modelleri, testleri ve uç noktaları düzenlenmiştir gibi içerik **projeleri** özel konuşma tanıma Portalı'nda. Her proje için bir etki alanı ve ülke/dil özeldir. Örneğin, Amerika Birleşik Devletleri İngilizce kullanan çağrı merkezi için bir proje oluşturabilirsiniz.

İlk projenizi oluşturmak için Seç **konuşma metin/özel konuşma**, ardından **yeni proje**. Projenizi oluşturmak için Sihirbazı tarafından sağlanan yönergeleri izleyin. Bir projeyi oluşturduktan sonra Dört sekmeden görmeniz gerekir: **Veri**, **test**, **eğitim**, ve **dağıtım**. Sağlanan bağlantıları kullanın [sonraki adımlar](#next-steps) her sekme kullanmayı öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

* [Hazırlama ve test, verileri](how-to-custom-speech-test-data.md)
* [Verilerinizi denetleyin](how-to-custom-speech-inspect-data.md)
* [Verilerinizi değerlendirin](how-to-custom-speech-evaluate-data.md)
* [Modelinizi eğitin](how-to-custom-speech-train-model.md)
* [Modelinizi dağıtma](how-to-custom-speech-deploy-model.md)
