---
title: Azure Key Vault azaltma kılavuzu
description: Anahtar kasası azaltma kaynakların aşırı kullanımı önlemek için eş zamanlı çağrılarının sayısını sınırlar.
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: ''
ms.assetid: 9b7d065e-1979-4397-8298-eeba3aec4792
ms.service: key-vault
ms.workload: identity
ms.topic: article
ms.date: 05/10/2018
ms.author: alleonar
ms.openlocfilehash: 59968f2bccbe2828ebe5fb33c57ed28d4f8509b6
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
ms.locfileid: "34067699"
---
# <a name="azure-key-vault-throttling-guidance"></a>Azure Key Vault azaltma kılavuzu

Azaltma kaynakların aşırı kullanımı önlemek için Azure hizmeti eşzamanlı çağrı sayısı sınırlayan başlatma bir işlemdir. Azure anahtar kasası (AKV), yüksek hacimli isteklerini işlemek için tasarlanmıştır. Bunaltıcı bir istek sayısı ortaya çıkarsa, istemcinin istek azaltma en iyi performans ve güvenilirlik AKV hizmetinin korumaya yardımcı olur.

Azaltma sınırları senaryo göre değişir. Örneğin, büyük hacimli yazma gerçekleştiriyorsanız azaltma olanağı yalnızca okuma gerçekleştirdiğiniz varsa daha yüksek olur.

## <a name="how-does-key-vault-handle-its-limits"></a>Anahtar kasası sınırlarına nasıl işler?

Kaynakların kötüye kullanımı önlemek ve tüm anahtar Kasası'nın istemcileri için hizmet kalitesi sağlamak için anahtar kasası hizmet sınırları vardır. Bir hizmeti eşiği aşıldığında, anahtar kasası herhangi başka bir istek Bu istemciden gelen bir süre için sınırlar. Bu gerçekleştiğinde, anahtar kasası HTTP durum kodu 429 döndürür (çok sayıda istek), ve istekleri başarısız olur. Ayrıca, anahtar kasası tarafından izlenen azaltma sınırları doğrultusunda 429 bir sayı döndürür isteği başarısız oldu. 

Daha yüksek azaltma sınırları için geçerli iş durum varsa, lütfen bizimle iletişime geçin.


## <a name="how-to-throttle-your-app-in-response-to-service-limits"></a>Hizmet sınırları yanıta uygulamanızda kısıtlama nasıl

Aşağıdakiler **en iyi uygulamalar** uygulamanızı azaltma için:
- İstek başına işlem sayısını azaltın.
- İstekleri sıklığını azaltın.
- Hemen yeniden deneme kaçının. 
    - Tüm istekler, kullanım sınırları karşı tahakkuk eder.

Uygulamanızın hata işleme uyguladığınızda, istemci-tarafı azaltma ihtiyacına algılamak için HTTP hata kodunu 429 kullanın. İsteği yeniden HTTP 429 hata koduyla başarısız olursa, bir Azure hizmeti sınırını hala karşılaşıyoruz. Önerilen yöntem azaltma, onu başarılı olana kadar istek yeniden deneniyor istemci kullanmaya devam edin.

### <a name="recommended-client-side-throttling-method"></a>Önerilen istemci-tarafı azaltma yöntemi

HTTP hata kodunu 429, üstel geri alma yaklaşımı kullanarak, istemci azaltma başlayın:

1. Yeniden deneme isteği 1 saniye bekleyin
2. Hala 2 saniye bekleyin kısıtlanan, isteği yeniden deneyin
3. Hala 4 saniye bekleyin kısıtlanan, isteği yeniden deneyin
4. Hala 8 saniye bekleyin kısıtlanan, isteği yeniden deneyin
5. Hala 16 saniye bekleyin kısıtlanan, isteği yeniden deneyin

Bu noktada, HTTP 429 yanıt kodları alamıyorsanız.

## <a name="see-also"></a>Ayrıca bkz.

Microsoft Cloud azaltma daha derin yönlendirmesi için bkz: [azaltma düzeni](https://docs.microsoft.com/azure/architecture/patterns/throttling).

