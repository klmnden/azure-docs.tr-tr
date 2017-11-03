---
title: "Power BI çalışma koleksiyonlarında bir veri kaynağına bağlanma | Microsoft Docs"
description: "Power BI çalışma alanı koleksiyonu içinde bir veri kaynağına bağlanmak öğrenin."
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ROBOTS: NOINDEX
ms.assetid: 2a4caeb3-255d-4215-9554-0ca8e3568c13
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/20/2017
ms.author: asaxton
ms.openlocfilehash: 24600c4343e3bfebe14f25020c5a7ba02d15af64
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="connect-to-a-data-source"></a>Bir veri kaynağına bağlanma

İle **Power BI çalışma koleksiyonları**, raporlar, kendi uygulamanıza eklenebilir. Uygulamanıza bir Power BI raporu, rapor tarafından temel alınan veri bağlanır **alma** yapılandırarak veya verilerin bir kopyasını **doğrudan bağlanma** kullanarak veri kaynağı için **DirectQuery**.

> [!IMPORTANT]
> Power BI Çalışma Alanı Koleksiyonları kullanım dışı bırakılmıştır ve Haziran 2018'e kadar veya anlaşmanızda belirtilen süre boyunca kullanılabilecektir. Uygulamanızda kesinti yaşanmaması için Power BI Embedded'a geçirmeyi planlamanız önerilir. Verilerinizi Power BI Embedded'a nasıl taşıyacağınızı öğrenmek için bkz. [Power BI Çalışma Alanı Koleksiyonları'nı Power BI Embedded'a geçirme](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/).

**İçeri Aktarma** ve **DirectQuery**.arasındaki farklar burada yer almaktadır.

| İçeri Aktarma | DirectQuery |
| --- | --- |
| Tablolar, sütunlar *ve veri* içeri aktarılan veya raporun veri kümesine kopyalar. Temel alınan verilerde meydana değişiklikleri görmek için yenileme veya alma, bir tam, güncel veri kümesini yeniden gerekir. |Yalnızca *tablolar ve sütunlar* içeri aktarılan veya raporun veri kümesine kopyalar. Her zaman en güncel verileri görüntülediğiniz. |

Power BI çalışma koleksiyonlarıyla, DirectQuery bulut veri kaynaklarıyla kullanabilirsiniz, ancak veri kaynakları şu anda şirket içi değil.

> [!NOTE]
> Şirket içi veri ağ geçidi Power BI çalışma koleksiyonlarla şu anda desteklenmiyor. Bu, şirket içi veri kaynakları ile DirectQuery kullanamayacağınız anlamına gelir.

## <a name="supported-data-sources"></a>Desteklenen veri kaynakları

**DirectQuery**
* Azure SQL veritabanı
* Azure SQL Veri Ambarı

**İçeri Aktarma**

Tüm Power BI Desktop içinde kullanılabilir veri kaynakları kullanarak içeri aktarabilirsiniz. Şunları yapacaksınız **değil** Power BI çalışma alanı koleksiyonu içinde bu verileri yenileyemezsiniz. Değişiklikleri Power BI çalışma koleksiyonlara PBIX dosyanızı karşıya yüklemek zorunda. Ağ geçidi nedeni budur. 

## <a name="benefits-of-using-directquery"></a>DirectQuery kullanmanın yararları

İki birincil avantajları vardır kullanırken **DirectQuery**:

* **DirectQuery** tüm verileri büyük veri kümeleri, burada, aksi takdirde olacaktır ilk alınacak unfeasible üzerinden görselleştirmeleri oluşturmanıza olanak sağlar.
* Veri değişikliklerini temel alınan veri yenileme gerektirebilir ve bazı raporlar için geçerli verileri görüntülemek için gereken büyük veri aktarımları, yeniden içe aktarılması veri unfeasible yapmadan gerektirebilir. Bunun aksine, **DirectQuery** raporları her zaman güncel verileri kullanın.

## <a name="limitations-of-directquery"></a>DirectQuery sınırlamaları

Kullanımıyla ilgili birkaç sınırlama vardır **DirectQuery**:

* Tüm tablolar tek bir veritabanından gelmelidir.
* Sorgu fazla karmaşık olması durumunda bir hata oluşur. Hatayı gidermek için en az karmaşık olacak şekilde sorguyu yeniden düzenlemeniz gerekir. Sorgu karmaşık olması gerekiyorsa kullanmak yerine verileri içeri aktarmanız gerekir **DirectQuery**.
* İlişki filtreleme, her iki yönde yerine tek bir yön sınırlıdır.
* Bir sütunun veri türünü değiştiremezsiniz.
* Varsayılan olarak, sınırlamalar ölçüler izin DAX ifadeleri yerleştirilir. Bkz: [DirectQuery ve ölçüleri](#measures).

<a name="measures"/>

## <a name="directquery-and-measures"></a>DirectQuery ve ölçüleri
Temel alınan veri kaynağına gönderilen sorguların kabul edilebilir performans sahip emin olmak için sınırlamalar ölçüler üzerinde kullanılan. Kullanırken **Power BI Desktop**, Gelişmiş kullanıcılar seçerek bu sınırlamaya atlamak seçebilirsiniz **Dosya > Seçenekler ve Ayarlar > Seçenekler**. İçinde **seçenekleri** iletişim kutusunda, seçin **DirectQuery**ve seçeneğini **DirectQuery modunda Kısıtlanmamış ölçümlere izin**. Bu seçenek belirlendiğinde, bir ölçü için geçerli olan herhangi bir DAX ifade kullanılabilir. Kullanıcıların farkında olması gerekir; Ancak, ne zaman veriler içeri iyi gerçekleştirmek bazı ifadeleri yavaş neden olabileceğini arka ucuna sorgular zaman içinde kaynak **DirectQuery** modu. 

## <a name="see-also"></a>Ayrıca Bkz.

* [Microsoft Power BI çalışma koleksiyonları ile çalışmaya başlama](get-started.md)
* [Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)

Başka sorunuz mu var? [Power BI Topluluğu'nu deneyin](http://community.powerbi.com/)

