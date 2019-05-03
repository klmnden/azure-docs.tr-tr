---
title: "HTTP üzerinden Web URL'den içeri aktar: Modül başvurusu"
titleSuffix: Azure Machine Learning service
description: Bir machine learning denemesinden kullanmak için genel bir Web sayfasından veri okumak için HTTP modülü, Azure Machine Learning hizmeti aracılığıyla Web URL'si alma kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 2f0847e9dd90267d985b75be3c3a07ce8fae98a9
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029678"
---
# <a name="import-from-web-url-via-http-module"></a>Web URL HTTP modülü içeri aktarın.

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Bir machine learning denemesinden kullanmak için genel bir Web sayfasından veri okumak için bu modülü kullanın.

Bir web sayfasında yayımlanan veriler aşağıdaki kısıtlamalar geçerlidir:

- Verileri desteklenen biçimlerden birinde olmalıdır: CSV, TSV, ARFF'ye veya SvmLight. Diğer veri hatalara neden olur.
- Kimlik doğrulaması gerekli veya desteklenir. Verilerin genel kullanıma açık olması gerekir. 

Veri almanın iki yolu vardır: veri kaynağı kurma Sihirbazı'nı kullanın veya el ile yapılandırın.

## <a name="use-the-data-import-wizard"></a>Veri İçeri Aktarma Sihirbazı'nı kullanma

1. Ekleme **verileri içeri aktarma** denemenizi modülü. Modül içinde arabiriminde bulabilirsiniz **veri giriş ve çıkış** kategorisi.

2. Tıklayın **veri içeri aktarma sihirbazını başlatma** ve HTTP üzerinden Web URL'si seçin.

3. URL'yi yapıştırın ve bir veri biçimi seçin.

4. Yapılandırma tamamlandığında.

Mevcut bir veri bağlantısı düzenlemek için sihirbazı yeniden başlatın. Sıfırdan yeniden başlatmak zorunda kalmazsınız sihirbazın önceki tüm yapılandırma ayrıntılarını yükler.

## <a name="manually-set-properties-in-the-import-data-module"></a>El ile verileri içeri aktarma modülü kümesi özellikleri

Aşağıdaki adımları el ile içeri aktarma kaynak nasıl yapılandırılacağı açıklanmaktadır.

1. Ekleme [verileri içeri aktarma](import-data.md) denemenizi modülü. Modül içinde arabiriminde bulabilirsiniz **veri giriş ve çıkış** kategorisi.

2. İçin **veri kaynağı**seçin **Web URL'si HTTP üzerinden**.

3. İçin **URL**yüklemek istediğiniz verileri içeren sayfanın tam URL'yi yapıştırın veya yazın.

    URL ve site URL'sini ve dosya adı ve uzantısını yüklemek için gerekli verileri içeren sayfasına ile tam yolu içermesi gerekir.

    Örneğin, makine öğrenimi California Üniversitesi, Irvine deposu Iris veri kümesinden şu sayfaya içerir:

    `http://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data`

4. İçin **veri biçimi**, bir desteklenen veri biçimlerini listeden seçin.

    Her zaman önceden veri biçimini belirlemek için kontrol etmenizi öneririz. UC Irvine sayfanın CSV biçimi kullanır. Diğer desteklenen veri biçimlerini, TSV, ARFF'ye ve SvmLight ' dir.

5. Verileri CSV veya TSV biçimindedir kullanırsanız **dosya üst bilgi satırı içeriyor** kaynak veriler bir üst bilgi satırı içeriyorsa olup olmadığını belirtmek için seçeneği. Üst bilgi satırı sütun adları atamak için kullanılır.

6. Seçin **kullanın, sonuçları önbelleğe** çok değiştirmek için verileri beklemiyoruz veya yeniden yüklemeyi önlemek istiyorsanız verileri her zaman seçenekleri denemeyi çalıştırın.

    Bu seçenek belirlendiğinde, denemeyi modülü çalıştırılır ve bundan sonra veri kümesinin önbelleğe alınmış bir sürümü kullanan veri ilk zaman yükler.

    Veri kümesini deneme kümesinin her yinelemede yeniden yüklemek istiyorsanız, seçimini **kullanın, sonuçları önbelleğe** seçeneği. Parametre herhangi bir değişiklik varsa, sonuçları yeniden ayrıca [verileri içeri aktarma](import-data.md).

7. Denemeyi çalıştırın.

## <a name="results"></a>Sonuçlar

Tamamlandığında, çıkış veri kümesi tıklayıp **Görselleştir** verileri başarıyla içeri aktarıldı görmek için.


## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 