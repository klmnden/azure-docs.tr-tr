---
title: "Microsoft Genomics: Sık sorulan sorular | Microsoft Docs"
titleSuffix: Azure
description: "Microsoft Genomics hakkında genel sorular müşteriler yanıtlarını isteyin."
services: microsoft-genomics
author: grhuynh
manager: jhubbard
editor: jasonwhowell
ms.author: grhuynh
ms.service: microsoft-genomics
ms.workload: genomics
ms.topic: article
ms.date: 12/07/2017
ms.openlocfilehash: 2077eeb5177b07c458476ae900f81b72e35f0dc3
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="microsoft-genomics-common-questions"></a>Microsoft Genomics: Sık sorulan sorular

Bu makalede Microsoft Genomics sahip olabileceğiniz sorguları ilişkili üst listelenmektedir. Microsoft Genomics hizmeti hakkında daha fazla bilgi için bkz: [Microsoft Genomics nedir?](overview-what-is-genomics.md) 


## <a name="what-is-the-sla-for-microsoft-genomics"></a>Microsoft Genomics SLA nedir?
Saat Microsoft Genomics hizmeti % 99,9 oranda iş akışı API istekleri almak kullanılabilir olacağını garanti ediyoruz. Daha fazla bilgi için bkz: [SLA](https://azure.microsoft.com/support/legal/sla/genomics/v1_0/).

## <a name="how-does-the-usage-of-microsoft-genomics-show-up-on-my-bill"></a>Nasıl Microsoft Genomics kullanımını faturamı üzerinde görünmesini?
İş akışı işlenen gigabases sayısına dayalı Microsoft Genomics faturaları. Daha fazla bilgi için bkz: [fiyatlandırma](https://azure.microsoft.com/pricing/details/genomics/).


## <a name="where-can-i-find-a-list-of-all-possible-commands-and-arguments-for-the-msgen-client"></a>Tüm olası komutları ve bağımsız değişkenler listesini nereden bulabilirim `msgen` istemci?
Çalıştırarak kullanılabilir komutlar ve bağımsız değişkenler tam bir listesi elde edebilirsiniz `msgen help`. Kullanılabilir Yardım listesini gösterir sağlanan ek bağımsız değişkenler varsa bölümler, her biri için bir tane `submit`, `list`, `cancel`, ve `status`. Belirli bir komut için Yardım almak için yazın `msgen help command`; Örneğin, `msgen help submit` tüm teslim seçeneklerini listeler.

## <a name="what-are-the-most-commonly-used-commands-for-the-msgen-client"></a>Komutları için kullanılan en yaygın olarak nelerdir `msgen` istemci?
En sık kullanılan komutlar için bağımsız değişkenler `msgen` istemcisi içerir: 

 |**Komutu**          |  **Alan açıklaması** |
 |:--------------------|:-------------         |
 |`list`               |Gönderdiğiniz işlerin bir listesini döndürür. Bağımsız değişkenler için bkz: `msgen help list`.  |
 |`submit`             |Bir iş akışı isteği hizmetine gönderir. Bağımsız değişkenler için bkz: `msgen help submit`.|
 |`status`             |Belirtilen iş akışının durumunu döndürür `--workflow-id`. Ayrıca bkz. `msgen help status`. |
 |`cancel`             |Belirtilen iş akışı işlenmesini iptal etmek için bir istek gönderir `--workflow-id`. Ayrıca bkz. `msgen help cancel`. |

## <a name="where-do-i-get-the-value-for---api-url-base"></a>Burada değeri alıyorum `--api-url-base`?
Azure Portalı'na gidin ve Genomics hesap sayfanıza açın. Altında **Yönetim** başlığını seçin **erişim anahtarları**. Burada, API URL'si ve erişim tuşlarınızı bulun.

## <a name="where-do-i-get-the-value-for---access-key"></a>Burada değeri alıyorum `--access-key`?
Azure Portalı'na gidin ve Genomics hesap sayfanıza açın. Altında **Yönetim** başlığını seçin **erişim anahtarları**. Burada, API URL'si ve erişim tuşlarınızı bulun.

## <a name="why-do-i-need-two-access-keys"></a>İki erişim tuşu neden gerekiyor mu?
(Yeniden) güncelleştirmek isteyebileceğiniz iki erişim tuşu gereken hizmet kullanımını kesintiye olmadan bunları. Örneğin, ilk anahtar güncelleştirmek istediğiniz. Bu durumda, tüm yeni iş akışları ikinci anahtar kullanmaya geçin. Daha sonra ilk anahtar kullanarak zaten çalışan iş akışları tamamlanana kadar bekleyin. Ancak bundan sonra anahtar güncelleştirin.

## <a name="do-you-save-my-storage-account-keys"></a>My depolama hesabı anahtarlarını kaydedebilirim?
Depolama hesabı anahtarınızı Microsoft Genomics hizmetinin çıktı dosyaları, giriş dosyalarınızın okuma ve yazma kısa vadeli erişim belirteçleri oluşturmak için kullanılır. Varsayılan belirteç süresi 48 saattir. Belirteç süresi değiştirilebilir `-sas/--sas-duration` Gönder komutunu; seçenek değeri saattir.

## <a name="what-genome-references-can-i-use"></a>Hangi genome başvuruyor kullanabilir miyim?

Bu başvurular desteklenir:
 |Başvuru              | Değeri`-pa/--process-args` |
 |:-------------         |:-------------                 |
 |b37                    | `R=b37m1`                     |
 |hg38                   | `R=hg38m1`                    |      
 |hg38 (alt çözümleme) | `R=hg38m1x`                   |  
 |hg19                   | `R=hg19m1`                    |    

## <a name="how-do-i-format-my-command-line-arguments-as-a-config-file"></a>Bir yapılandırma dosyası olarak nasıl my komut satırı bağımsız değişkenleri biçimlendirmek? 

yapılandırma dosyaları şu biçimde msgen anlar:
* Tüm seçenekleri anahtar-değer çiftleri olarak anahtarları virgülle ayrılmış değerler sağlanır.
Boşluk göz ardı edilir.
* İle başlayan satırlar `#` göz ardı edilir.
* Uzun biçiminde herhangi bir komut satırı bağımsız değişkende bir anahtara başında tire çıkarma ve alt çizgi ile sözcükler arasındaki çizgi değiştirme tarafından dönüştürülebilir. Bazı dönüştürme örnekler şunlardır:

 |Komut satırı bağımsız değişkeni            | Yapılandırma dosyası satırı |
 |:-------------                   |:-------------                 |
 |`-u/--api-url-base https://url`  | *api_url_base:https://URL*    |
 |`-k/--access-key KEY`            | *access_key:Key*              |      
 |`-pa/--process-args R=B37m1`     | *process_args:R-b37m1*        |  

## <a name="next-steps"></a>Sonraki adımlar

Microsoft Genomics ile çalışmaya başlamak için aşağıdaki kaynakları kullanın:
- Microsoft Genomics hizmeti aracılığıyla ilk iş akışınızın çalıştırarak başlayın. [Microsoft Genomics hizmeti aracılığıyla bir iş akışını çalıştırma](quickstart-run-genomics-workflow-portal.md)
- Microsoft Genomics hizmeti tarafından işlemek için kendi verilerinizi gönderme: [FASTQ eşleştirilmiş](quickstart-input-pair-FASTQ.md) | [BAM](quickstart-input-BAM.md) | [birden çok FASTQ veya BAM](quickstart-input-multiple.md) 

