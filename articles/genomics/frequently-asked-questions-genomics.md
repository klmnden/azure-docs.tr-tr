---
title: 'Microsoft Genomiks: Sık sorulan sorular - SSS | Microsoft Docs'
titleSuffix: Azure
description: Microsoft Genomics hakkında genel soruların müşterilere yanıtlar isteyin.
services: genomics
author: grhuynh
manager: cgronlun
ms.author: grhuynh
ms.service: genomics
ms.topic: article
ms.date: 12/07/2017
ms.openlocfilehash: d36a2c6379a95cc67a55c2cc266ced94b4a0179a
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67672224"
---
# <a name="microsoft-genomics-common-questions"></a>Microsoft Genomiks: Sık sorulan sorular

Bu makalede, Microsoft Genomics ilgili en sık kullanılan sorgular listelenir. Microsoft Genomics hizmeti hakkında daha fazla bilgi için bkz. [Microsoft Genomics nedir?](overview-what-is-genomics.md). Sorun giderme hakkında daha fazla bilgi için bkz. bizim [sorun giderme kılavuzu](troubleshooting-guide-genomics.md). 


## <a name="how-do-i-run-gatk4-workflows-on-microsoft-genomics"></a>Microsoft Genomics GATK4 iş akışlarını nasıl çalıştırırım?
Microsoft Genomics hizmetin config.txt dosyasında belirtmek için işlem_adı `gatk4`. Not, normal fatura ücretlerine göre faturalandırılır.


## <a name="what-is-the-sla-for-microsoft-genomics"></a>Microsoft Genomics için SLA'sı nedir?
% 99,9 bir saat Microsoft Genomics hizmeti iş akışı API isteklerini almak kullanılabilir olacağını garanti ediyoruz. Daha fazla bilgi için [SLA](https://azure.microsoft.com/support/legal/sla/genomics/v1_0/).

## <a name="how-does-the-usage-of-microsoft-genomics-show-up-on-my-bill"></a>Nasıl Microsoft Genomics kullanımı faturamda gösterilir?
İş akışı başına işlenen gigabaz sayısına bağlı olarak Microsoft Genomics faturalandırır. Daha fazla bilgi için [fiyatlandırma](https://azure.microsoft.com/pricing/details/genomics/).


## <a name="where-can-i-find-a-list-of-all-possible-commands-and-arguments-for-the-msgen-client"></a>Tüm olası komutları ve bağımsız değişkenler listesini nereden bulabilirim `msgen` istemci?
Çalıştırarak kullanılabilir komutları ve bağımsız değişkenler tam bir listesini alabilirsiniz `msgen help`. Daha fazla bağımsız değişken olmadan kullanılabilir Yardım listesini gösteren sağlanan varsa bölümlerinde, biri her biri için `submit`, `list`, `cancel`, ve `status`. Belirli bir komut için Yardım almak için şunu yazın `msgen help command`; Örneğin, `msgen help submit` teslim seçeneklerini listeler.

## <a name="what-are-the-most-commonly-used-commands-for-the-msgen-client"></a>Komutlarında kullanılan en yaygın olarak nelerdir `msgen` istemci?
En sık kullanılan komutlar için bağımsız değişkenleri olan `msgen` istemcisi içerir: 

 |**Komutu**          |  **Alan açıklaması** |
 |:--------------------|:-------------         |
 |`list`               |Size gönderilen işlerin bir listesini döndürür. Bağımsız değişkenler için bkz. `msgen help list`.  |
 |`submit`             |Hizmetine bir iş akışı isteği gönderir. Bağımsız değişkenler için bkz. `msgen help submit`.|
 |`status`             |Tarafından belirtilen iş akışı durumunu döndüren `--workflow-id`. Ayrıca bkz: `msgen help status`. |
 |`cancel`             |Tarafından belirtilen iş akışı işleme iptal etmek için bir istek gönderir `--workflow-id`. Ayrıca bkz: `msgen help cancel`. |

## <a name="where-do-i-get-the-value-for---api-url-base"></a>Değeri nereden alabilirim `--api-url-base`?
Azure portalına gidin ve Genomiks hesabı sayfanızı açın. Altında **Yönetim** başlığı seçin **erişim anahtarları**. Burada, hem API URL'sine ve erişim tuşlarınızı bulun.

## <a name="where-do-i-get-the-value-for---access-key"></a>Değeri nereden alabilirim `--access-key`?
Azure portalına gidin ve Genomiks hesabı sayfanızı açın. Altında **Yönetim** başlığı seçin **erişim anahtarları**. Burada, hem API URL'sine ve erişim tuşlarınızı bulun.

## <a name="why-do-i-need-two-access-keys"></a>İki erişim tuşu neden gerekiyor?
İki erişim tuşu ihtiyacınız (yeniden) güncelleştirmek istemeniz durumunda bunları hizmeti kullanımını kesintiye olmadan. Örneğin, ilk anahtarı güncelleştirmek istiyorsanız, tüm yeni iş akışları ikinci anahtarınızı olmalıdır. Daha sonra ilk anahtar güncelleştirmeden önce tamamlamak için ilk tuşu kullanarak tüm iş akışları için bekleyin.

## <a name="do-you-save-my-storage-account-keys"></a>Depolama hesabı anahtarlarımı kaydedebilirim?
Depolama hesabı anahtarınızı, Microsoft Genomics hizmeti Çıkış dosyalarını, giriş dosyaları okuma ve yazma kısa süreli erişim belirteçleri oluşturmak için kullanılır. Varsayılan belirteç süresi 48 saattir. Belirteç süresi değiştirilebilir `-sas/--sas-duration` gönderme komutunun; seçenek değeri saattir.

## <a name="what-genome-references-can-i-use"></a>Hangi genom başvuran kullanabilir miyim?

Bu başvurular desteklenir:

 |Başvuru              | Değeri `-pa/--process-args` |
 |:-------------         |:-------------                 |
 |b37                    | `R=b37m1`                     |
 |hg38                   | `R=hg38m1`                    |      
 |hg38 (alt çözümleme) | `R=hg38m1x`                   |  
 |hg19                   | `R=hg19m1`                    |    

## <a name="how-do-i-format-my-command-line-arguments-as-a-config-file"></a>Bir yapılandırma dosyası olarak nasıl my komut satırı bağımsız değişkenleri biçimlendirme? 

yapılandırma dosyaları şu biçimde msgen'i anlar:
* Tüm seçenekleri anahtarlarından virgül ile ayrılmış değerleri olan anahtar-değer çiftleri olarak sağlanır.
  Boşluk yoksayılır.
* İle başlayan satırlar `#` göz ardı edilir.
* Uzun biçimde herhangi bir komut satırı bağımsız değişkeni için bir anahtar şeridi oluşturma önde gelen, kısa çizgi ve alt çizgi ile kelimeler arasındaki çizgi değiştirerek dönüştürülebilir. Dönüştürme bazı örnekler şunlardır:

  |komut satırı bağımsız değişkeni            | Yapılandırma dosyası satırı |
  |:-------------                   |:-------------                 |
  |`-u/--api-url-base https://url`  | *api_url_base: https://url*    |
  |`-k/--access-key KEY`            | *access_key:Key*              |      
  |`-pa/--process-args R=B37m1`     | *process_args:R-b37m1*        |  

## <a name="next-steps"></a>Sonraki adımlar

Microsoft Genomics kullanmaya başlamak için aşağıdaki kaynakları kullanın:
- İlk iş akışınızı Microsoft Genomics hizmet aracılığıyla çalıştırarak başlayın. [Microsoft Genomics hizmeti üzerinden iş akışı çalıştırma](quickstart-run-genomics-workflow-portal.md)
- Microsoft Genomics hizmeti tarafından işlenmesi için kendi verilerinizi göndermek: [eşleştirilmiş FASTQ](quickstart-input-pair-FASTQ.md) | [BAM](quickstart-input-BAM.md) | [birden fazla FASTQ veya BAM](quickstart-input-multiple.md) 

