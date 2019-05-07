---
title: Ayarları yapılandırma
titleSuffix: Azure Cognitive Services
description: Hizmet yapılandırması nasıl ödül hizmet değerlendirir, ne sıklıkta hizmet keşfediyor, modelin ne sıklıkta retrained ve ne kadar veri depolanan içerir.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: bce260d2a6b897d3e45b231df4eecd7ecb31ba31
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65143251"
---
# <a name="personalizer-settings"></a>Personalizer ayarları

Hizmet yapılandırması nasıl ödül hizmet değerlendirir, ne sıklıkta hizmet keşfediyor, modelin ne sıklıkta retrained ve ne kadar veri depolanan içerir.

## <a name="create-personalizer-resource"></a>Personalizer kaynak oluştur

Her geri bildirim döngüsü için Personalizer kaynak oluşturun. 

1. [Azure portalda](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer) oturum açın. Önceki bağlantı açılır **Oluştur** Personlizer hizmetinin sayfası. 
1. Hizmet adınızı girin, fiyatlandırma katmanı ve kaynak grubu aboneliği konumu seçin.
1. Onay seçip **Oluştur**.

## <a name="configure-service-settings-in-the-azure-portal"></a>Azure portalında hizmet ayarlarını yapılandırma

1. [Azure Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer) oturum açın.
1. Personalizer kaynağınızı bulun. 
1. İçinde **kaynak yönetimi** bölümünden **ayarları**.

    Azure portalında ayrılmadan önce kaynak anahtarlarınızdan birini kopyalayın **anahtarları** sayfası. Kullanmak için bu bilgi gerekecektir [Personalizer SDK](https://aka.ms/personalizer-dotnet-sdk-docs).

### <a name="configure-reward-settings-for-the-feedback-loop-based-on-use-case"></a>Kullanım durumunu temel alarak geri bildirim döngüsü ödül ayarlarını yapılandırma

Geri bildirim döngünün kullanımını ödül için hizmetin ayarlarını yapılandırın. Aşağıdaki ayarlarda yapılan değişiklikleri geçerli Personalizer modeli sıfırlama ve verinin son 2 gün ile çağırma:

![Geri bildirim döngüsü ödül ayarlarını yapılandırın](media/settings/configure-model-reward-settings.png)

|Ayar|Amaç|
|--|--|
|Ödül bekleme süresi|Ayarlar, hangi Personalizer sırasında sürenin uzunluğunu derece çağrı ödül değerlerini toplar, derece çağrı andan itibaren başlayan'olmuyor Bu değer, sorarak ayarlanır: "Ne kadar süreyle Personalizer ödül çağrıları için beklemesi gereken?" Bu pencere sonra gelen herhangi bir ödül oturum açmış ancak öğrenme için kullanılmaz.|
|Varsayılan ödül|Hiçbir ödül çağrı aldığında, bir sıra ilişkili ödül bekleme süresi penceresi sırasında Personalizer tarafından çağrı, varsayılan ödül Personalizer atar. Varsayılan ve çoğu senaryoda, varsayılan ödül sıfırdır.|
|Ödül toplama|Birden çok ödül alınırsa aynı dereceye için API çağrısı, bu toplama yöntemi kullanılır: **toplam** veya **erken**. En erken alınan erken puanı alır ve rest atar. Bu, büyük olasılıkla yinelenen çağrıları arasında benzersiz ödül istiyorsanız kullanışlıdır. |

Bu ayarları değiştirdikten sonra seçtiğinizden emin olun **Kaydet**.

### <a name="exploration-setting"></a>İnceleme ayarı 

Kişiselleştirme yeni keşfedin ve kullanıcı davranış değişiklikleri için zaman içinde alternatifleri inceleyerek uyum kuramıyor. **Araştırma** ayarı belirler derece çağrıları yüzde araştırması ile yanıt verdi. 

Bu ayarda yapılan değişiklikler geçerli Personalizer modeli sıfırlama ve verinin son 2 gün ile çağırma.

![İnceleme ayarı, derece çağrıları yüzde araştırması ile yanıt verdi belirler.](media/settings/configure-exploration-setting.png)

Bu ayarı değiştirdikten sonra seçtiğinizden emin olun **Kaydet**.

### <a name="model-update-frequency"></a>Model güncelleştirme sıklığı

**Model güncelleştirme sıklığını** ne sıklıkta yeni Personalizer model retrained ayarlar. 

![Yeni bir Personalizer modeli ne sıklıkta retrained model güncelleştirme sıklığını ayarlar.](media/settings/configure-model-update-frequency-settings.png)

Bu ayarı değiştirdikten sonra seçtiğinizden emin olun **Kaydet**.

### <a name="data-retention"></a>Veri saklama

**Veri saklama süresi** kaç gün, veri günlüklerini Personalizer tutar ayarlar. Veri günlükleri gerçekleştirmek için gerekli olan [çevrimdışı değerlendirmeleri](concepts-offline-evaluation.md), Personalizer etkisini ölçmek ve öğrenme ilke iyileştirmek için kullanılır.

Bu ayarı değiştirdikten sonra seçtiğinizden emin olun **Kaydet**.

## <a name="export-the-personalizer-model"></a>Personalizer modelini dışarı aktarma

İçin kaynak yönetim takımının bölümünden **modeli ve ilke**, model oluşturma ve son güncelleştirme tarihi gözden geçirin ve geçerli modelini dışarı aktarma.

![Geçerli Personalizer modelini dışarı aktarma](media/settings/export-current-personalizer-model.png)

## <a name="import-and-export-learning-policy"></a>İçeri ve dışarı aktarma İlkesi öğrenme

İçin kaynak yönetim takımının bölümünden **modeli ve ilke**, yeni bir öğrenme ilkesi alma veya geçerli öğrenme ilkeyi dışarı aktar.

## <a name="next-steps"></a>Sonraki adımlar

[Personalizer kapsayıcı kullanma](https://go.microsoft.com/fwlink/?linkid=2083923&clcid=0x409)
