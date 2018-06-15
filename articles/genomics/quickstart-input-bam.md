---
title: 'Hızlı Başlangıç: BAM dosyası girişini kullanarak iş akışı gönderme | Microsoft Docs'
titleSuffix: Azure
description: Bu hızlı başlangıçta msgen istemcisini yüklediğiniz ve hizmette örnek verileri başarıyla çalıştırdığınız kabul edilmektedir.
services: microsoft-genomics
author: grhuynh
manager: jhubbard
editor: jasonwhowell
ms.author: grhuynh
ms.service: microsoft-genomics
ms.workload: genomics
ms.topic: quickstart
ms.date: 12/07/2017
ms.openlocfilehash: 9887121593cad4931c86b55012f1c22686adf25f
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
ms.locfileid: "26684526"
---
# <a name="submit-a-workflow-using-a-bam-file-input"></a>BAM dosyası girişini kullanarak iş akışı gönderme

Bu hızlı başlangıçta giriş dosyanız tek bir BAM dosyası olduğunda Microsoft Genomiks hizmetine iş akışı gönderme adımları gösterilmektedir. Bu konu başlığında `msgen` istemcisini yükleyip çalıştırdığınız ve Azure Depolama konusunda bilgi sahibi olduğunuz kabul edilmektedir. Sağlanan örnek verileri kullanarak başarıyla bir iş akışı gönderdiyseniz bu hızlı başlangıca devam etmeye hazırsınız demektir. 

## <a name="set-up-upload-your-bam-file-to-azure-storage"></a>Kurulum: BAM dosyanızı Azure depolamaya yükleme
*reads.bam* adlı tek bir BAM dosyasına sahip olduğunuzu ve bu dosyayı *myaccount* adlı Azure depolama hesabınıza **https://<span></span>myaccount.blob.core<span></span>.windows<span></span>.net<span></span>/inputs/reads<span></span>.bam<span></span>** olarak yüklediğinizi düşünelim. API URL'sine ve erişim anahtarına sahipsiniz. **https://<span></span>myaccount.blob.core<span></span>.windows<span></span>.net<span></span>/outputs<span></span>** içinde iki çıkış olmasını istiyorsunuz.



## <a name="submit-your-job-to-the-msgen-client"></a>İşinizi `msgen` istemcisine gönderme 


Burada `msgen` istemcisine sağlamanız gereken minimum bağımsız değişkenler verilmiştir; kodun daha anlaşılır olması için satır sonları eklenmiştir:

Windows için:

```
msgen submit ^
  --api-url-base <Genomics API URL> ^
  --access-key <Genomics access key> ^
  --process-args R=b37m1 ^
  --input-storage-account-name myaccount ^
  --input-storage-account-key <storage access key to "myaccount"> ^
  --input-storage-account-container inputs ^
  --input-blob-name-1 reads.bam ^
  --output-storage-account-name myaccount ^
  --output-storage-account-key <storage access key to "myaccount"> ^
  --output-storage-account-container outputs
```


Unix için

```
msgen submit \
  --api-url-base <Genomics API URL> \
  --access-key <Genomics access key> \
  --process-args R=b37m1 \
  --input-storage-account-name myaccount \
  --input-storage-account-key <storage access key to "myaccount"> \
  --input-storage-account-container inputs \
  --input-blob-name-1 reads.bam \
  --output-storage-account-name myaccount \
  --output-storage-account-key <storage access key to "myaccount"> \
  --output-storage-account-container outputs
```


Yapılandırma dosyası kullanmayı tercih ediyorsanız şu bileşenleri dahil etmeniz gerekir:

``` config.txt
api_url_base:                     <Genomics API URL>
access_key:                       <Genomics access key>
process_args:                     R=b37m1
input_storage_account_name:       myaccount
input_storage_account_key:        <storage access key to "myaccount">
input_storage_account_container:  inputs
input_blob_name_1:                reads.bam
output_storage_account_name:      myaccount
output_storage_account_key:       <storage access key to "myaccount">
output_storage_account_container: outputs
```

`config.txt` dosyasını şu çağrıyla gönderin: `msgen submit -f config.txt`

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede bir BAM dosyasını Azure Depolama'ya yükleyip `msgen` python istemcisi üzerinden Microsoft Genomiks hizmetine bir iş akışı gönderdiniz. İş akışının gönderilmesi ve Microsoft Genomiks hizmetiyle kullanabileceğiniz diğer komutlar hakkında daha fazla bilgi için bkz. [SSS](frequently-asked-questions-genomics.md). 
