---
title: 'Azure Blob depolama alanından içeri aktar: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Bu konuda bir machine learning denemesinden verileri kullanabilmesi için Azure Blob Depolama modülü içeri aktar Azure Machine Learning hizmetinde Azure blob depolama alanından verileri okumak için nasıl kullanılacağını açıklar öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 4ac98516c1a326e1ede09bbb9660113ffd0642a0
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029693"
---
# <a name="import-from-azure-blob-storage-module"></a>Azure Blob Depolama modülünden içeri aktarma

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Bir machine learning denemesinden verileri kullanabilmesi için Azure blob depolama alanından verileri okumak için bu modülü kullanın.  

Azure Blob veri ikili veriler de dahil olmak üzere, büyük miktarlarda depolamak için bir hizmettir. Azure blobları, HTTP veya HTTPS aracılığıyla erişilebilir. Kimlik doğrulaması, blob depolama türüne bağlı olarak gerekli olabilir. 

- Genel BLOB'lar, herkes tarafından ya da bir SAS URL'si olan kullanıcılar tarafından erişilebilir.
- Özel BLOB'ları, bir oturum açma ve kimlik bilgilerini gerektirir.

BLOB depolama alanından alma gerektiren veri kullanan blob'larda depolanacak **blok blobu** biçimi. Blobu'nda depolanan dosyaları, virgülle ayrılmış (CSV) veya sekmeyle ayrılmış (TSV) biçimleri kullanmanız gerekir. Dosyayı okumak, satırlar olarak kayıtlarını ve ilgili özniteliğin başlık bir veri kümesi olarak belleğe yüklenir.


Verilerinizi içeri aktarmadan önce şemayı beklenildiği gibi gittiğinden emin olmak için profilinin kesinlikle öneririz. İçeri aktarma işlemi bazı şema belirlemek için baş satır sayısı tarar, ancak sonraki satırları ek sütunları veya hatalara neden veri içerebilir.



## <a name="manually-set-properties-in-the-import-data-module"></a>El ile verileri içeri aktarma modülü kümesi özellikleri

Aşağıdaki adımları el ile içeri aktarma kaynak nasıl yapılandırılacağı açıklanmaktadır.

1. Ekleme **verileri içeri aktarma** denemenizi modülü. Bu modül de arabiriminde bulabilirsiniz **veri giriş ve çıkış**

2. İçin **veri kaynağı**seçin **Azure Blob Depolama**.

3. İçin **kimlik doğrulama türü**, seçin **ortak (SAS URL)** genel veri kaynağı olarak bilgileri sağlanmadı biliyorsanız. Bir SAS URL'si, bir Azure depolama yardımcı programını kullanarak oluşturabileceğiniz genel erişim için bir zaman sınırı URL'dir.

    Bir seçim **hesabı**.

4. Verilerinizi ise bir **genel** bir SAS URL'si kullanılarak erişilebilen blob indirme ve kimlik doğrulaması için gerekli tüm bilgileri URL dizesini içerdiği için ek kimlik bilgileri gerekmez.

    İçinde **URI** alanına yazın veya hesap ve ortak blob tanımlayan tam URI yapıştırın.



5. Verilerinizi ise bir **özel** hesabı, hesap adı ve anahtarı gibi kimlik bilgilerini sağlamanız gerekir.

    - İçin **hesap adı**erişmek istediğiniz blob içeren hesabının adını yapıştırın veya yazın.

        Örneğin, depolama hesabının Tam URL'si `http://myshared.blob.core.windows.net`, yazarsınız `myshared`.

    - İçin **hesap anahtarı**, hesabıyla ilişkili depolama erişim anahtarını yapıştırın.

        Erişim anahtarı bilmiyorsanız, bölümüne bakın, "Bu makalede Azure depolama hesaplarınızı yönetme": [Azure depolama hesapları hakkında](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

6. İçin **kapsayıcı, dizin veya blob yolu**, almak istediğiniz belirli bir blobu adını yazın.

    Örneğin, adında bir dosya karşıya **data01.csv** kapsayıcıya **trainingdata** adlı bir hesapta **mymldata**, dosya için tam URL şu şekilde olacaktır: `http://mymldata.blob.core.windows.net/trainingdata/data01.txt` .

    Bu nedenle, alandaki **kapsayıcı, dizin veya blob yolu**, şunu yazın: `trainingdata/data01.csv`

    Birden çok dosyayı içeri aktarmak için joker karakterler kullanabilirsiniz `*` (yıldız işareti) veya `?` (soru işareti).

    Örneğin, kapsayıcı varsayılarak `trainingdata` birden çok dosya içeren uyumlu bir biçimde, ile başlayan tüm dosyaları okumak için şu belirtime kullanabilirsiniz `data`ve bunları tek bir veri kümesine birleştirme:

    `trainingdata/data*.csv`

    Kapsayıcı adlarında joker karakterler kullanamaz. Birden çok kapsayıcılardan dosyaları almanız gerekiyorsa, ayrı bir örneğini kullan **Veri Al** her kapsayıcı ve veri kümeleri kullanılarak ardından birleştirme için Modülü [Add Rows](./add-rows.md) modülü.

    > [!NOTE]
    > Seçeneğini seçtiyseniz **kullanın, sonuçları önbelleğe**, kapsayıcı dosyalarında yaptığınız tüm değişiklikler denemede veri yenileme tetiklemez.

7. İçin **Blob dosya biçimi**, Azure Machine Learning veri uygun şekilde işleyebilmesi blob depolanan verilerin biçimini belirten bir seçenek belirleyin. Aşağıdaki biçimleri desteklenir:

    - **CSV**: Virgülle ayrılmış değerler (CSV) içeri ve dışarı aktarma Azure Machine learning'de dosyaları için varsayılan depolama biçimi ' dir. Verilere bir üst bilgi satırı içeriyorsa, seçeneğini belirlediğinizden emin olun **dosya üst bilgi satırı içeriyor**, veya üst bilgi veri satırı kabul edilir.

       

    - **TSV**: Sekmeyle ayrılmış değerler (TSV) çok sayıda machine learning araçları tarafından kullanılan bir biçim var. Verilere bir üst bilgi satırı içeriyorsa, seçeneğini belirlediğinizden emin olun **dosya üst bilgi satırı içeriyor**, veya üst bilgi veri satırı kabul edilir.

       

    - **ARFF'YE**: Bu biçim, Weka araç takımı tarafından kullanılan biçimi dosyalarında almayı destekler. 

   

8. Denemeyi çalıştırın.


## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 