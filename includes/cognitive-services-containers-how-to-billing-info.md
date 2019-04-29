---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 04/16/2019
ms.openlocfilehash: e92d1c65d9601c23e7e785f07e2de3e43ea6612b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60598813"
---
Kapsayıcıya sorguları için kullanılan bir Azure kaynak fiyatlandırma katmanını faturalandırılır `<ApiKey>`.

Bilişsel hizmetler kapsayıcılar, kullanım ölçümü için fatura uç noktasına bağlı olmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri fatura uç noktası ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor. Bilişsel hizmetler kapsayıcılar, Microsoft müşteri verilerini (örneğin, görüntü veya metin analiz edilen) göndermeyin. 

### <a name="connecting-to-azure"></a>Azure'a bağlanma

Kapsayıcıyı çalıştırmak için fatura bağımsız değişken değerlerini gerekir. Bu değerler, fatura uç noktaya bağlanmak kapsayıcı sağlar. Kapsayıcı yaklaşık her 10 ila 15 dakika kullanım raporları. Azure için izin verilen zaman penceresi içinde kapsayıcı bağlanamazsa, kapsayıcı çalıştırın ancak devam edecek fatura uç noktayı geri yüklenene kadar sorgular hizmet yok. Bağlantı 10 kez aynı zaman aralığında 10-15 dakika denenir. Fatura uç noktasına 10 çalıştığında içinde bağlanamıyorsanız, kapsayıcı çalışmayı durdurur. 

### <a name="billing-arguments"></a>Faturalandırma bağımsız değişkenleri

Aşağıdaki seçeneklerden birini üçü için sırayla geçerli değerlerle belirtilmelidir `docker run` kapsayıcı başlatmak için komut:

| Seçenek | Açıklama |
|--------|-------------|
| `ApiKey` | Fatura bilgileri izlemek için kullanılan Bilişsel Hizmet kaynağı API anahtarı.<br/>Bu seçeneğin değeri, belirtilen sağlanan kaynak için bir API anahtarı ayarlanmalıdır `Billing`. |
| `Billing` | Fatura bilgileri izlemek için kullanılan Bilişsel hizmet kaynak uç noktası.<br/>Bu seçeneğin değeri, sağlanan bir Azure kaynak URI'sini uç noktasına ayarlamanız gerekir.|
| `Eula` | Kapsayıcı lisansını kabul ettiğinizi gösterir.<br/>Bu seçenek değeri ayarlanmalıdır `accept`. |


