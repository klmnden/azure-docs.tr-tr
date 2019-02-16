---
title: Azure veri fabrikası veri akışı havuz dönüştürme eşlemesi
description: Azure veri fabrikası veri akışı havuz dönüştürme eşlemesi
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: 795b8072bbd9b248f982d061d699f490b1b63b17
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56272181"
---
# <a name="azure-data-factory-mapping-data-flow-sink-transformation"></a>Azure veri fabrikası veri akışı havuz dönüştürme eşlemesi

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Havuz seçenekleri](media/data-flow/windows1.png "havuzu 1")

Veri akışı dönüşümünüzü tamamlandığında, bir hedef veri kümesine dönüştürülmüş veri havuzu. Havuz dönüşümünde hedef çıktı verileri için kullanmak istediğiniz veri kümesi tanımı seçebilirsiniz.

Bir ortak gelen verileri değiştirmek için hesap ve şema kayması için hesap tanımlı bir şeması çıkış veri kümesinde olmayan bir klasör için çıktı verilerini havuz uygulamadır. Ayrıca tüm sütun değişikliklerini kaynaklarınızı "İzin ver şema değişikliklerini" kaynak ve sonra eşleme seçerek havuz içindeki tüm alanlarda hesabının.

Üzerine, ekleme veya bir veri kümesine yönelik veri akışı başarısız seçebilirsiniz.

Tüm gelen alanları havuz "eşleme" de seçebilirsiniz. Hedef havuz istediğiniz alanları seçin istiyorsanız veya hedefteki alanların adlarını değiştirmek istiyorsanız, "eşleme" için "Kapat"'i seçin ve sonra çıktı alanlarını eşlemek için eşleme sekmesine tıklayın:

![Havuz seçenekleri](media/data-flow/sink2.png "2 Havuz")

## <a name="output-to-one-file"></a>Çıktıyı bir dosyaya
Azure depolama blobu veya Data Lake havuz türlerinde bir klasöre dönüştürülmüş verileri çıkarır. Spark, kullanılan bölümleme düzenini temel alarak bölümlenmiş çıkış veri dosyaları oluşturacağını havuz Dönüştür. Bölümleme düzeni "En iyi duruma getir" sekmesinde tıklayarak ayarlayabilirsiniz. Çıkış tek bir dosyada birleştirmek için ADF esnetmek istiyorsanız "Tek bölüm" radyo düğmesine tıklayın.

![Havuz seçenekleri](media/data-flow/opt001.png "havuz seçenekleri")

## <a name="blob-storage-folder"></a>BLOB Depolama klasörü
Blob Store için veri Bağlantılarınızdaki indirme, bir blob seçin *klasör* olarak, hedef klasör yolu bir dosya değil. ADF veri akışı çıktı dosyaları bu klasörde oluşturacaktır.

![Klasör yolu](media/data-flow/folderpath.png "klasör yolu")

## <a name="optional-azure-sql-data-warehouse-sink"></a>İsteğe bağlı bir Azure SQL veri ambarı havuzu

Veri akışı için ADW havuz veri kümesi, erken bir beta sunacağımız. Bu, bir kopyalama etkinliği, işlem hattınızda ekleme gerek kalmadan doğrudan Azure SQL DW veri akışı içinde dönüştürülen veri yerleşmesi olanak tanır.

Herhangi diğer ADF ardışık düzeni için ADW kimlik bilgilerinizi içeren bir bağlı hizmeti ile olduğu gibi bir ADW veri kümesi oluşturarak başlayın ve bağlanmak istediğiniz veritabanını seçin. Tablo adında mevcut bir tabloyu seçin veya gelen alanlarda yapmanız için otomatik olarak oluşturmak için veri akışı istediğiniz tablonun adını yazın.

![Havuz seçenekleri](media/data-flow/adw3.png "3 Havuz")

Havuz dönüştürmesi duyulduğundan (ADW şu anda yalnızca bir havuz olarak desteklenen) ADW hazırlama verileri Polybase için ADW yüklemek için kullanmak istediğiniz depolama hesabını yanı sıra oluşturduğunuz veri kümesi seçersiniz. Yol alanı biçimi şöyledir: "containername/foldername".

![Havuz seçenekleri](media/data-flow/adw1.png "4 Havuz")

### <a name="save-policy"></a>İlkeyi Kaydet

Üzerine yazma varsa tabloyu kesmek, ardından yeniden oluşturun ve verileri yükleme. Ekleme yeni satır ekleyin. Veri kümesini tablo adını tabloda hiç ADW hedefte mevcut değilse, veri akışı tablosu oluşturun ve sonra veri yükleme.

"Otomatik eşleme" işaretini kaldırırsanız, hedef tablonuz alanları el ile eşleyebilirsiniz.

![Havuz ADW seçenekleri](media/data-flow/adw2.png "adw havuz")

### <a name="max-concurrent-connections"></a>Maksimum eşzamanlı bağlantıları

Verilerinizi Azure veritabanı bağlantınız yazarken, en fazla eşzamanlı bağlantı havuzu dönüşümünde ayarlayabilirsiniz.

![Bağlantı Seçenekleri](media/data-flow/maxcon.png "bağlantıları")
