---
title: 'Microsoft Genomiks: sorun giderme kılavuzu | Microsoft Docs'
titleSuffix: Azure
description: Sorun giderme stratejileri hakkında daha fazla bilgi edinin
keywords: sorun giderme, hata, hata ayıklama
services: microsoft-genomics
author: ruchir
editor: jasonwhowell
ms.author: ruchir
ms.service: genomics
ms.workload: genomics
ms.topic: article
ms.date: 10/29/2018
ms.openlocfilehash: 78084e6beac7b390b1ea1afe888030c5224856b6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60790513"
---
# <a name="troubleshooting-guide"></a>Sorun giderme kılavuzu

İşte birkaç MSGEN'i Microsoft Genomics hizmeti kullanırken karşılaştığınız yaygın sorunlardan bazılarını için sorun giderme ipuçları.

 Sorun giderme için ilgili olmayan SSS için [sık sorulan sorular](frequently-asked-questions-genomics.md).
## <a name="step-1-locate-error-codes-associated-with-the-workflow"></a>1\. adım: İş akışı ile ilişkili hata kodları bulun

İş akışı tarafından ile ilişkili hata iletileri bulabilirsiniz:

1. Komut satırını kullanarak ve yazarak  `msgen status`
2. Standardoutput.txt içeriğini inceleniyor.

### <a name="1-using-the-command-line-msgen-status"></a>1. Komut satırını kullanarak `msgen status`

```bash
msgen status -u URL -k KEY -w ID 
```




Üç gerekli bağımsız değişkenleri şunlardır:

* Adresa URL – API için ana URI
* ANAHTARI - Genomiks hesabı için erişim anahtarı
    * URL'sini ve ANAHTARINI bulmak için Azure portalına gidin ve Microsoft Genomics hesap sayfanızı açın. Altında **Yönetim** başlığı seçin **erişim anahtarları**. Burada, hem API URL'sine ve erişim tuşlarınızı bulun.

  
* Kimliği - iş akışı kimliği
    * İş akışı kimliği türünüz bulunacak `msgen list` komutu. Yapılandırma dosyanızı URL'si ve erişim tuşlarınızı içerir ve bulunduğu msgen'i exe dosyanızın ile aynı konumda olan komut şöyle görünür: 
        
        ```bash
        msgen list -f "config.txt"
        ```
        Bu komutun çıktısı şuna benzeyecektir:
        
        ```bash
            Microsoft Genomics command-line client v0.7.4
                Copyright (c) 2018 Microsoft. All rights reserved.
                
                Workflow List
                -------------
                Total Count  : 1
                
                Workflow ID     : 10001
                Status          : Completed successfully
                Message         :
                Process         : snapgatk-20180730_1
                Description     :
                Created Date    : Mon, 27 Aug 2018 20:27:24 GMT
                End Date        : Mon, 27 Aug 2018 20:55:12 GMT
                Wall Clock Time : 0h 27m 48s
                Bases Processed : 1,348,613,600 (1 GBase)
        ```

  > [!NOTE]
  >  Alternatif olarak, doğrudan URL'sini ve ANAHTARINI girmek yerine yapılandırma dosyasının yolu ekleyebilirsiniz. Yapılandırma dosyası yanı sıra komut satırında bu bağımsız değişkenler dahil ederseniz, komut satırı bağımsız değişkenleri öncelikli olur.  

İş akışı kimliği 1001 ve config.txt dosyasına msgen'i aynı yol yürütülebilir yerleştirilir, komut şöyle görünür:

```bash
msgen status -w 1001 -f "config.txt"
```

### <a name="2--examine-the-contents-of-standardoutputtxt"></a>2.  Standardoutput.txt içeriğini inceleyin 
Çıkış kapsayıcısı için söz konusu iş akışını bulun. MSGEN'i oluşturur `[workflowfilename].logs.zip` her iş akışı yürütme sonrasında klasör. Klasör içeriğini görüntülemek için sıkıştırmasını açın:

* outputFileList.txt - iş akışı sırasında oluşturulan çıktı dosyaları listesi
* StandardError.txt - Bu dosya boş olur.
* StandardOutput.txt - iş akışı çalıştırılırken oluşan hatalar dahil olmak üzere tüm üst düzey durum iletileri günlüğe kaydeder.
* GATK günlük dosyalarını - diğer tüm dosyalarda `logs` klasörü

Sorun giderme için standardoutput.txt içeriğini incelemek ve görünen herhangi bir hata iletisi dikkat edin.


## <a name="step-2-try-recommended-steps-for-common-errors"></a>2\. adım: Sık karşılaşılan sorunlar için önerilen adımları deneyin

Bu bölümde kısaca Microsoft Genomics hizmeti (msgen'i) ve bunları gidermek için kullanabileceğiniz stratejiler yaygın hataları çıkış vurgular. 

Microsoft Genomics hizmetine (msgen'i) hataları aşağıdaki iki tür durum oluşturabilir:

1. İç hizmet hataları: Parametreleri veya giriş dosyaları düzelterek çözülebilir hizmete, iç hata. Bazen iş akışını yeniden göndermeyi bu hataları düzeltebilir.
2. Giriş hataları: Doğru bağımsız değişkenleri kullanarak veya dosya biçimleri düzeltme çözülmüş hataları.

### <a name="1-internal-service-errors"></a>1. İç hizmet hataları

Bir iç hizmet hatası kullanıcı eyleme dönüştürülebilir değil. İş akışını yeniden ancak bu işe yaramazsa, Microsoft Genomics desteğe başvurun.

| Hata iletisi                                                                                                                            | Önerilen sorun giderme adımları                                                                                                                                   |
|------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Bir iç hata oluştu. İş akışını yeniden göndermeyi deneyin. Bu hata yeniden görürseniz, Yardım için Microsoft Genomiks desteğine başvurun. | İş akışını yeniden gönderin. Kişi Microsoft Genomics destek oluşturarak sorun devam ederse, Yardım için destek [bilet](file-support-ticket-genomics.md ). |

### <a name="2-input-errors"></a>2. Giriş hataları

Bu eyleme dönüştürülebilir kullanıcı hatalardır. Dosya ve hata kodu türüne bağlı olarak, Microsoft Genomics hizmeti farklı hata kodları çıkarır. Aşağıda listelenen önerilen sorun giderme adımlarını izleyin.

| Dosya türü | Hata kodu | Hata iletisi                                                                           | Önerilen sorun giderme adımları                                                                                         |
|--------------|------------|-----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| Tüm          | 701        | Okuma [readId] [numberOfBases] tabanları var, ancak sınırı [maxReadLength]           | Bu hatanın en yaygın nedeni, iki okuma bir birleşimi için önde gelen dosyasının bozuk olmasıdır. Giriş dosyalarınızı denetleyin. |
| BAM          | 200        |   '[YourFileName]' dosyası okunamıyor.                                                                                       | BAM dosyası biçimini denetleyin. İş akışı düzgün şekilde biçimlendirilmiş bir dosyayla yeniden gönderin.                                                                           |
| BAM          | 201        |  [File_name] BAM dosyası okunamıyor.                                                                                      |BAM dosyası biçimini denetleyin.  İş akışı, doğru biçimlendirilmiş bir dosya ile gönderin.                                                                            |
| BAM          | 202        | [File_name] BAM dosyası okunamıyor. Çok küçük ve eksik dosya üstbilgisi.                                                                                        | BAM dosyası biçimini denetleyin.  İş akışı, doğru biçimlendirilmiş bir dosya ile gönderin.                                                                            |
| BAM          | 203        |   [File_name] BAM dosyası okunamıyor. Üstbilgi dosyası bozuktu.                                                                                      |BAM dosyası biçimini denetleyin.  İş akışı, doğru biçimlendirilmiş bir dosya ile gönderin.                                                                           |
| BAM          | 204        |    [File_name] BAM dosyası okunamıyor. Üstbilgi dosyası bozuktu.                                                                                     | BAM dosyası biçimini denetleyin.  İş akışı, doğru biçimlendirilmiş bir dosya ile gönderin.                                                                           |
| BAM          | 205        |    [File_name] BAM dosyası okunamıyor. Üstbilgi dosyası bozuktu.                                                                                     | BAM dosyası biçimini denetleyin.  İş akışı, doğru biçimlendirilmiş bir dosya ile gönderin.                                                                            |
| BAM          | 206        |   [File_name] BAM dosyası okunamıyor. Üstbilgi dosyası bozuktu.                                                                                      | BAM dosyası biçimini denetleyin.  İş akışı, doğru biçimlendirilmiş bir dosya ile gönderin.                                                                            |
| BAM          | 207        |  [File_name] BAM dosyası okunamıyor. Dosya uzaklığı [uzaklığı] kesildi.                                                                                       | BAM dosyası biçimini denetleyin.  İş akışı, doğru biçimlendirilmiş bir dosya ile gönderin.                                                                            |
| BAM          | 208        |   BAM dosyası geçersiz. ReadID [Read_Id] [File_name] dosyasında hiçbir dizisi vardır.                                                                                      | BAM dosyası biçimini denetleyin.  İş akışı, doğru biçimlendirilmiş bir dosya ile gönderin.                                                                             |
| FASTQ        | 300        |  FASTQ dosyası okunamıyor. [File_name] bir yeni satır ile sonlanmıyor.                                                                                     | FASTQ dosya biçimi düzeltin ve iş akışını yeniden gönderin.                                                                           |
| FASTQ        | 301        |   [File_name] FASTQ dosyası okunamıyor. FASTQ kaydıdır uzaklık arabellek boyutu büyüktür: [_offset]                                                                                      | FASTQ dosya biçimi düzeltin ve iş akışını yeniden gönderin.                                                                         |
| FASTQ        | 302        |     FASTQ söz dizimi hatası. Dosya [File_name] boş bir satır vardır.                                                                                    | FASTQ dosya biçimi düzeltin ve iş akışını yeniden gönderin.                                                                         |
| FASTQ        | 303        |       FASTQ söz dizimi hatası. [Dosya_adı] dosyasının uzaklığında geçersiz bir başlangıç karakter içeriyor: [_offset] satırdan: [line_type] karakter: [_char]                                                                                  | FASTQ dosya biçimi düzeltin ve iş akışını yeniden gönderin.                                                                         |
| FASTQ        | 304      |  [_ReadID] readID FASTQ sözdizimi hatası.  Toplu işin ilk okuma readID /1 [File_name] dosyasında sonu yok                                                                                       | FASTQ dosya biçimi düzeltin ve iş akışını yeniden gönderin.                                                                         |
| FASTQ        | 305        |  [_ReadID] readID FASTQ sözdizimi hatası. İkinci okuma batch readID [File_name] dosyasında /2 sonu yok                                                                                      | FASTQ dosya biçimi düzeltin ve iş akışını yeniden gönderin.                                                                          |
| FASTQ        | 306        |  [_ReadID] readID FASTQ sözdizimi hatası. Çiftin ilk okuma [File_name] dosyasında /1 biten bir Kimliğe sahip değil                                                                                       | FASTQ dosya biçimi düzeltin ve iş akışını yeniden gönderin.                                                                          |
| FASTQ        | 307        |   [_ReadID] readID FASTQ sözdizimi hatası. ReadID /1 ile bitmiyor veya / 2. [Dosya_adı] dosyasının bir eşleştirilmiş FASTQ dosyası olarak kullanılamaz.                                                                                      |FASTQ dosya biçimi düzeltin ve iş akışını yeniden gönderin.                                                                          |
| FASTQ        | 308        |  FASTQ okuma hatası. Okuma ikisinde'nın farklı bir şekilde yanıt verdi. Doğru FASTQ dosyası seçtiğiniz?                                                                                       | FASTQ dosya biçimi düzeltin ve iş akışını yeniden gönderin.                                                                         |
|        |       |                                                                                        |                                                                           |

## <a name="step-3-contact-microsoft-genomics-support"></a>3\. adım: Microsoft Genomiks desteğe başvurun

İş hataları devam ederseniz veya diğer herhangi bir sorunuz varsa, Azure portalında Microsoft Genomiks desteğe başvurun. Bir destek talebi göndermek nasıl hakkında daha fazla bilgi bulunabilir [burada](file-support-ticket-genomics.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, siz ve Microsoft Genomics hizmeti ile ilgili yaygın sorunları gidermek nasıl öğrendiniz. Daha fazla bilgi ve daha Genel SSS için bkz. [sık sorulan sorular](frequently-asked-questions-genomics.md). 
