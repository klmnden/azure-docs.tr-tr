---
title: Azure Key Vault azaltma kılavuzu
description: Key Vault azaltma, kaynakların aşırı kullanımını önlemek için eş zamanlı çağrılar sayısını sınırlar.
services: key-vault
documentationcenter: ''
author: bryanla
manager: mbaldwin
tags: ''
ms.assetid: 9b7d065e-1979-4397-8298-eeba3aec4792
ms.service: key-vault
ms.workload: identity
ms.topic: conceptual
ms.date: 05/10/2018
ms.author: bryanla
ms.openlocfilehash: f119e4a5b5c5f97848c588636a3a707428abbd5b
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53082535"
---
# <a name="azure-key-vault-throttling-guidance"></a>Azure Key Vault azaltma kılavuzu

Azaltma, kaynakların aşırı kullanımını önlemek için bir Azure hizmetine eş zamanlı çağrı sayısını sınırlayan başlatma bir işlemdir. Azure Key Vault (AKV) büyük hacimde istekleri işlemek için tasarlanmıştır. Büyük bir istek sayısı ortaya çıkarsa, istemcinin istekleri azaltma en iyi performans ve güvenilirlik AKV hizmetinin korumasına yardımcı olur.

Azaltma sınırları senaryoya bağlı olarak değişiklik gösterir. Örneğin, büyük hacimli yazma gerçekleştiriyorsanız için azaltma olanağı, yalnızca okuma işlemi yapıyorsanız daha yüksek olur.

## <a name="how-does-key-vault-handle-its-limits"></a>Key Vault, kendi sınırlarına nasıl işliyor?

Kaynakları kötüye kullanımı önlemesi ve Key Vault'un istemciler için hizmet kalitesi emin olmak için Key Vault hizmet sınırları vardır. Bir hizmeti eşiği aşıldığında, Key Vault istemciden gelen ek istekleri bir süre için sınırlar. Bu durumda, HTTP durum kodu 429 Key Vault döndürür (çok fazla istek), ve istekleri başarısız olur. Ayrıca, Key Vault tarafından izlenen azaltma sınırları doğrultusunda 429 bir sayı döndürür isteği başarısız oldu. 

İşle ilgili geçerli durum azaltma sınırları için varsa, lütfen bizimle iletişime geçin.


## <a name="how-to-throttle-your-app-in-response-to-service-limits"></a>Nasıl kısıtlanacağını uygulamanızın yanıt olarak hizmet sınırları

Aşağıdaki **en iyi uygulamalar** uygulamanızın azaltma için:
- İstek başına işlemlerin sayısını azaltın.
- İstekleri sıklığını azaltın.
- Hemen yeniden deneme kaçının. 
    - Tüm istekler, kullanım sınırlarını karşı tahakkuk eder.

Uygulamanızın hata işleme uygularken, istemci tarafı azaltma ihtiyacına algılamak için HTTP hata kodu 429 kullanın. İstek yine bir HTTP 429 hata koduyla başarısız olursa, bir Azure hizmeti sınırını hala karşılaşıyoruz. Önerilen yöntem azaltma, başarılı olana kadar istek yeniden deneniyor taraflı kullanmaya devam edin.

Üstel geri alma uygulayan kodu aşağıda gösterilmiştir. 
```
     public async Task OnGetAsync()
     {
         Message = "Your application description page.";
         int retries = 0;
         bool retry = false;
         try
         {
             AzureServiceTokenProvider azureServiceTokenProvider = new AzureServiceTokenProvider();
             KeyVaultClient keyVaultClient = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
             var secret = await keyVaultClient.GetSecretAsync("https://<YourKeyVaultName>.vault.azure.net/secrets/AppSecret")
                     .ConfigureAwait(false);
             Message = secret.Value;

             /* The below do while logic is to handle throttling errors thrown by Azure Key Vault. It shows how to do exponential backoff which is the recommended client side throttling*/
             do
             {
                 long waitTime = Math.Min(getWaitTime(retries), 2000000);
                 secret = await keyVaultClient.GetSecretAsync("https://<YourKeyVaultName>.vault.azure.net/secrets/AppSecret")
                     .ConfigureAwait(false);
                 retry = false;
             } 
             while(retry && (retries++ < 10));
         }
         /// <exception cref="KeyVaultErrorException">
         /// Thrown when the operation returned an invalid status code
         /// </exception>
         catch (KeyVaultErrorException keyVaultException)
         {
             Message = keyVaultException.Message;
             if((int)keyVaultException.Response.StatusCode == 429)
                 retry = true;
         }
     }

     // This method implements exponential backoff incase of 429 errors from Azure Key Vault
     private static long getWaitTime(int retryCount)
     {
         long waitTime = ((long)Math.Pow(2, retryCount) * 100L);
         return waitTime;
     }
```

### <a name="recommended-client-side-throttling-method"></a>Önerilen istemci-tarafı azaltma yöntemi

HTTP hata kodu 429 üzerinde bir üstel geri alma yaklaşımı kullanarak istemci azaltma başlayın:

1. 1 saniye, yeniden deneme isteği bekleyin
2. Yine de 2 saniye bekleyin kısıtlanan, isteği yeniden deneyin.
3. Yine de 4 saniye bekleyin kısıtlanan, isteği yeniden deneyin.
4. Yine de 8 saniye bekleyin kısıtlanan, isteği yeniden deneyin.
5. Hala 16 saniye bekleyin kısıtlanan, isteği yeniden deneyin.

Bu noktada, HTTP 429 yanıtı kodları alamıyorsanız.

## <a name="see-also"></a>Ayrıca bkz.

Microsoft Cloud azaltma derin yönlendirmesi için bkz. [kısıtlama düzeni](https://docs.microsoft.com/azure/architecture/patterns/throttling).

