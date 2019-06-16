---
title: 'Verileri dışarı aktarma: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Verileri dışarı aktarma modül sonuçları, Ara veriler ve çalışma verileri bulut depolama konumlarını dışında Azure Machine Learning içine denemelerinizden kaydetmek için Azure Machine Learning hizmetinde kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: c3744803f172edf9fbf2556a12677e8faef370c2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65028328"
---
# <a name="export-data-module"></a>Dışarı aktarma veri Modülü

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Sonuçlar, Ara veriler ve çalışma verileri bulut depolama konumlarını dışında Azure Machine Learning içine denemelerinizden kaydetmek için bu modülü kullanın.

Bu modül, dışarı aktarma veya aşağıdaki bulut veri hizmetlerinden verileriniz kaydedilirken destekler:


- **Dışarı aktarma, Azure Blob depolama alanına**: Verileri Azure Blob hizmetine kaydeder. Blob hizmeti verileri herkese açık şekilde paylaşılan veya güvenli uygulama veri depolarında kaydedildi.

  
## <a name="how-to-configure-export-data"></a>Yapılandırma verilerini dışarı aktar

1. Ekleme **verileri dışarı aktarma** denemenizde arabirimi modülü. Bu modülde bulabilirsiniz **giriş ve çıkış** kategorisi.

2. Connect **verileri dışarı aktarma** dışarı aktarmak istediğiniz verileri içeren modül.

3. Çift **verileri dışarı aktarma** açmak için **özellikleri** bölmesi.

4. İçin **veri hedef**, bulut depolama kaydettiğiniz veri türünü seçin. Diğer tüm özellikler için bu seçeneği herhangi bir değişiklik yaparsanız sıfırlanır. Bu nedenle bu seçeneği ilk seçtiğinizden emin olun!

5. Belirtilen depolama hesabına erişmek için gereken bir hesap adı ve kimlik doğrulama yöntemi sağlar.

    **Dışarı aktarma, Azure Blob depolama alanına** tek seçenek özel Önizleme aşamasındadır. Aşağıdaki modül ayarlama işlemi gösterilmektedir.
    1. Büyük miktarlarda verinin ikili veriler de dahil olmak üzere depolamak için Azure blob hizmetidir. Blob storage'nın iki tür vardır: Genel BLOB'lar ve oturum açma kimlik bilgileri gerektiren BLOB'ları.

    2. İçin **kimlik doğrulama türü**, seçin **genel (SAS)** depolama erişimi bir SAS URL'si aracılığıyla desteklediğini biliyorsanız.

          Bir SAS URL'si, bir Azure depolama yardımcı programını kullanarak oluşturulabilir ve yalnızca sınırlı bir süre için kullanılabilir URL özel türüdür.  Bu kimlik doğrulama ve indirme için gerekli tüm bilgileri içerir.

        İçin **URI**yazın veya yapıştırın hesabı ve ortak blob tanımlayan tam URI.

        Dosya biçimi için CSV ve TSV desteklenir.

    3. Özel hesap seçin **hesabı**ve deneme depolama hesabına yazabilmesi amacıyla hesap adını ve hesap anahtarı sağlayın.

         - **Hesap adı**: Yazın veya verileri kaydetmek istediğiniz hesabın adını yapıştırın. Örneğin, depolama hesabının Tam URL'si `http://myshared.blob.core.windows.net`, yazarsınız `myshared`.

        - **Hesap anahtarı**: Hesapla ilişkili depolama erişim anahtarını yapıştırın.

        -  **Kapsayıcı, dizin veya blob yolu**: Dışarı aktarılan verileri depolanacağı blob adını yazın. Örneğin, adında yeni bir bloba denemenizi sonuçlarını kaydetmek için **results01.csv** kapsayıcısında **Öngörüler** adlı bir hesapta **mymldata**, tam URL'si BLOB olacak `http://mymldata.blob.core.windows.net/predictions/results01.csv`.

            Bu nedenle, alandaki **kapsayıcı, dizin veya blob yolu**, kapsayıcı belirtirsiniz ve blob adı olarak izler: `predictions/results01.csv`

        - Olmayan bir blobun adını belirtirseniz mevcut, Azure blob sizin için oluşturur.

       -  Mevcut bir bloba yazarken, geçerli bir blobun içeriğini özelliğini ayarlayarak yazılmasını belirtebilirsiniz **Azure blob depolama alanına yazma modu**. Varsayılan olarak, bu özellik kümesine **hata**, yani aynı ada sahip mevcut bir blob dosya bulunduğunda, bir hata oluşturdu.


    4. İçin **blob dosyası için dosya biçimi**, hangi veri depolanmalıdır biçimini seçin.

        - **CSV**: Virgülle ayrılmış değerler (CSV), varsayılan depolama biçimi ' dir. Sütun başlıkları birlikte verileri dışarı aktarmak için seçeneğini **yazma blob üst bilgi satırı**.  Azure Machine Learning'de kullanılan virgülle sınırlandırılmış biçimi hakkında daha fazla bilgi için bkz: [CSV'ye Dönüştür](./convert-to-csv.md).

        - **TSV**: Sekmeyle ayrılmış değerler (TSV) biçimi, çok sayıda machine learning araçları ile uyumludur. Sütun başlıkları birlikte verileri dışarı aktarmak için seçeneğini **yazma blob üst bilgi satırı**.  

 
    5. **Önbelleğe alınan sonuçları kullanmak**: Sonuçları blob dosyasına yeniden denemeyi her çalıştırdığınızda kaçınmak istiyorsanız bu seçeneği belirleyin. Modül parametrelerini için başka bir değişiklik varsa, denemeyi modülü, yalnızca ilk çalıştırıldığında sonuçları yazar veya verilerde yapılan değişiklikler olduğunda.

    6. Denemeyi çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 