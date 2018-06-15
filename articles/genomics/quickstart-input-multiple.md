---
title: 'Hızlı Başlangıç: Çoklu giriş kullanarak iş akışı gönderme | Microsoft Docs'
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
ms.date: 02/05/2018
ms.openlocfilehash: 7aeb4d5ad939cefcf8300b78b4afcc9d91ca0624
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
ms.locfileid: "29120004"
---
# <a name="submit-a-workflow-using-multiple-inputs-from-the-same-sample"></a>Aynı örnekten birden fazla giriş kullanarak iş akışı gönderme

Bu hızlı başlangıçta giriş dosyanız **aynı örnekten alınan** birden fazla FASTQ veya BAM dosyası olduğunda Microsoft Genomiks hizmetine iş akışı gönderme adımları gösterilmektedir. Örneğin, sıralayıcı üzerinde birden çok şeritte **aynı örneği** çalıştırdıysanız, sıralayıcı her şerit için bir çift FASTQ dosyası çıkarabilir. Hizalama ve varyant aramadan önce bu FASTQ dosyalarını birleştirmek yerine, bu girişlerin tümünü `msgen` istemcisine doğrudan gönderebilirsiniz. `msgen` istemcisinin çıktıları, .bam, .bai, .vcf dosyalarından oluşan **tek bir küme** olur. 

Ancak aynı gönderide FASTQ ve BAM dosyalarını bir arada **kullanamayacağınızı** unutmayın. Ayrıca, birden çok kişiden birden çok FASTQ veya BAM dosyası **gönderemezsiniz**. 

Bu makalede `msgen` istemcisini yükleyip çalıştırdığınız ve Azure Depolama’yı kullanma konusunda bilgi sahibi olduğunuz kabul edilmektedir. Sağlanan örnek verileri kullanarak başarıyla bir iş akışı gönderdiyseniz bu hızlı başlangıca devam etmeye hazırsınız demektir. 


## <a name="multiple-bam-files"></a>Birden fazla BAM dosyası

### <a name="upload-your-input-files-to-azure-storage"></a>Giriş dosyalarınızı Azure depolamaya yükleme
Giriş olarak *reads.bam*, *additional_reads.bam* ve *yet_more_reads.bam* olmak üzere birden fazla BAM dosyasına sahip olduğunuzu ve bunları *myaccount* adlı Azure depolama hesabınıza yüklediğinizi düşünelim. API URL'sine ve erişim anahtarına sahipsiniz. **https://<span></span>myaccount.blob.core<span></span>.windows<span></span>.net<span></span>/outputs<span></span>** içinde iki çıkış olmasını istiyorsunuz.


### <a name="submit-your-job-to-the-msgen-client"></a>İşinizi `msgen` istemcisine gönderme 

Birden fazla BAM dosyasını adlarını --input-blob-name-1 komutuna bağımsız değişken olarak ileterek gönderebilirsiniz. Tüm dosyaların aynı örnekten gelmesi gerektiğini ancak sıralamanın önemli olmadığını unutmayın. Aşağıda Windows ile Unix’te komut satırında ve yapılandırma dosyası kullanılarak gerçekleştirilen örnek gönderimlere yer verilmiştir. Kodun daha anlaşılır olması için satır sonları eklenmiştir:


Windows için:

```
msgen submit ^
  --api-url-base <Genomics API URL> ^
  --access-key <Genomics access key> ^
  --process-args R=b37m1 ^
  --input-storage-account-name myaccount ^
  --input-storage-account-key <storage access key to "myaccount"> ^
  --input-storage-account-container inputs ^
  --input-blob-name-1 reads.bam additional_reads.bam yet_more_reads.bam ^
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
  --input-blob-name-1 reads.bam additional_reads.bam yet_more_reads.bam \
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
input_blob_name_1:                reads.bam additional_reads.bam yet_more_reads.bam
output_storage_account_name:      myaccount
output_storage_account_key:       <storage access key to "myaccount">
output_storage_account_container: outputs
```

`config.txt` dosyasını şu çağrıyla gönderin: `msgen submit -f config.txt`


## <a name="multiple-paired-fastq-files"></a>Birden fazla eşleştirilmiş FASTQ dosyası

### <a name="upload-your-input-files-to-azure-storage"></a>Giriş dosyalarınızı Azure depolamaya yükleme
Giriş olarak *reads_1.fq.gz* ve *reads_2.fq.gz*, *additional_reads_1.fq.gz* ve *additional_reads_2.fq.gz* ile *yet_more_reads_1.fq.gz* ve *yet_more_reads_2.fq.gz* olmak üzere birden fazla eşleştirilmiş FASTQ dosyasına sahip olduğunuzu düşünelim. Bunları *myaccount* adlı Azure depolama hesabınıza yüklediniz. API URL'sine ve erişim anahtarına sahipsiniz. **https://<span></span>myaccount.blob.core<span></span>.windows<span></span>.net<span></span>/outputs<span></span>** içinde iki çıkış olmasını istiyorsunuz.


### <a name="submit-your-job-to-the-msgen-client"></a>İşinizi `msgen` istemcisine gönderme 

Eşleştirilmiş FASTQ dosyalarının aynı örneğe ait olması ve bir arada işlenmesi gerekir.  Dosya adlarının sırası --input-blob-name-1 ve --input-blob-name-2 komutlarına bağımsız değişken olarak ilettiğinizde önemlidir. 

Aşağıda Windows ile Unix’te komut satırında ve yapılandırma dosyası kullanılarak gerçekleştirilen örnek gönderimlere yer verilmiştir. Kodun daha anlaşılır olması için satır sonları eklenmiştir:


Windows için:

```
msgen submit ^
  --api-url-base <Genomics API URL> ^
  --access-key <Genomics access key> ^
  --process-args R=b37m1 ^
  --input-storage-account-name myaccount ^
  --input-storage-account-key <storage access key to "myaccount"> ^
  --input-storage-account-container inputs ^
  --input-blob-name-1 reads_1.fastq.gz additional_reads_1.fastq.gz yet_more_reads_1.fastq.gz ^
  --input-blob-name-2 reads_2.fastq.gz additional_reads_2.fastq.gz yet_more_reads_2.fastq.gz ^
  --output-storage-account-name myaccount ^
  --output-storage-account-key <storage access key to "myaccount"> ^
  --output-storage-account-container outputs
```


Unix için:

```
msgen submit \
  --api-url-base <Genomics API URL> \
  --access-key <Genomics access key> \
  --process-args R=b37m1 \
  --input-storage-account-name myaccount \
  --input-storage-account-key <storage access key to "myaccount"> \
  --input-storage-account-container inputs \
  --input-blob-name-1 reads_1.fastq.gz additional_reads_1.fastq.gz yet_more_reads_1.fastq.gz \
  --input-blob-name-2 reads_2.fastq.gz additional_reads_2.fastq.gz yet_more_reads_2.fastq.gz \
  --output-storage-account-name myaccount \
  --output-storage-account-key <storage access key to "myaccount"> \
  --output-storage-account-container outputs
```


Yapılandırma dosyası kullanmayı tercih ediyorsanız şu bileşenleri dahil etmeniz gerekir:

```
api_url_base:                     <Genomics API URL>
access_key:                       <Genomics access key>
process_args:                     R=b37m1
input_storage_account_name:       myaccount
input_storage_account_key:        <storage access key to "myaccount">
input_storage_account_container:  inputs
input_blob_name_1:                reads_1.fq.gz additional_reads_1.fastq.gz yet_more_reads_1.fastq.gz
input_blob_name_2:                reads_2.fq.gz additional_reads_2.fastq.gz yet_more_reads_2.fastq.gz
output_storage_account_name:      myaccount
output_storage_account_key:       <storage access key to "myaccount">
output_storage_account_container: outputs
```

`config.txt` dosyasını şu çağrıyla gönderin: `msgen submit -f config.txt`

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede birden fazla BAM dosyasını veya eşleştirilmiş FASTQ dosyalarını Azure Depolama'ya yükleyip `msgen` python istemcisi üzerinden Microsoft Genomiks hizmetine bir iş akışı gönderdiniz. İş akışının gönderilmesi ve Microsoft Genomiks hizmetiyle kullanabileceğiniz diğer komutlar hakkında daha fazla bilgi için bkz. [SSS](frequently-asked-questions-genomics.md). 