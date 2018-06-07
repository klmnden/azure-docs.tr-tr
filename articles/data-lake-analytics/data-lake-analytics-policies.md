---
title: Azure Data Lake Analytics ilkelerini yönetme
description: Bir Data Lake Analytics hesabı kullanımını denetlemek için ilkeler kullanmayı öğrenin.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
manager: kfile
editor: jasonwhowell
ms.assetid: 0a6102d1-7554-4df2-b487-4dae9a7287b6
ms.topic: conceptual
ms.date: 04/30/2018
ms.openlocfilehash: 2c074b1a75c5bfef07ffb90e719bd3247a474e63
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34623953"
---
# <a name="manage-azure-data-lake-analytics-using-policies"></a>İlkeleri kullanarak Azure Data Lake Analytics yönetme

Hesap ilkeleri kullanarak, bir Azure Data Lake Analytics hesabı nasıl kaynaklardır denetleyebilirsiniz kullanılır. Bu ilkeler, Azure Data Lake Analytics kullanma maliyetini denetlemenize olanak tanır. Örneğin, bu ilkelerle hesabınız aynı anda kaç Avustralya sınırlayarak beklenmeyen maliyet ani engelleyebilirsiniz.

## <a name="account-level-policies"></a>Hesap düzeyinde ilkeleri

Bu ilkeler, bir Data Lake Analytics hesabı tüm işler için geçerlidir.

### <a name="maximum-number-of-aus-in-a-data-lake-analytics-account"></a>En fazla Avustralya numarasında Data Lake Analytics hesabı
Bir ilke Analytics Data Lake Analytics hesabınızı kullanabilirsiniz birimlerin (Avustralya) toplam sayısını denetler. Varsayılan değer 250'ye ayarlanır. Örneğin, bu değer 250'ye ayarlanmışsa, Avustralya, 250 ile çalışan işiniz olabilir veya 25 ile çalışan 10 işleri atanan Avustralya Avustralya her. Çalışan işler tamamlanana kadar gönderilen ek işler kuyruğa alınır. Çalışan işleri tamamlandığında, Avustralya çalıştırmak için sıraya alınan işleri kaydınızı kurtulurlar.

Data Lake Analytics hesabınızın Avustralya sayısını değiştirmek için:

1. Azure Portal'da Data Lake Analytics hesabınıza gidin.
2. **Özellikler**'e tıklayın.
3. Altında **maksimum Avustralya**, bir değer seçmek için kaydırıcıyı hareket ettirin veya metin kutusuna bir değer girin. 
4. **Kaydet**’e tıklayın.

> [!NOTE]
> Birden çok varsayılan (250) gerekiyorsa Avustralya, Portalı'nda tıklatın **Yardım + Destek** bir destek isteği gönderilemedi. Avustralya Data Lake Analytics hesabınızı kullanılabilir sayısı artırılabilir.
>

### <a name="maximum-number-of-jobs-that-can-run-simultaneously"></a>Aynı anda çalışabilir işi sayısı üst sınırı
Bir ilke, aynı anda kaç işleri çalıştırabilirsiniz denetler. Varsayılan olarak, bu değeri 20'ye ayarlanır. Data Lake Analytics Avustralya kullanılabilir varsa, yeni işleri hemen işlerin toplam sayısı bu ilkenin değere ulaşana kadar çalışmak üzere zamanlanmış. Maksimum sayıda aynı anda çalışabilir işi ulaştığında, sonraki işleri (AU kullanılabilirliğine bağlı olarak) bir veya daha fazla çalışan iş tamamlanana kadar öncelik sırasına göre sıralanır.

Aynı anda çalışabilir işlerinin sayısını değiştirmek için:

1. Azure Portal'da Data Lake Analytics hesabınıza gidin.
2. **Özellikler**'e tıklayın.
3. Altında **en büyük sayı, çalışan işleri**, bir değer seçmek için kaydırıcıyı hareket ettirin veya metin kutusuna bir değer girin. 
4. **Kaydet**’e tıklayın.

> [!NOTE]
> İşlerini, varsayılan (20) sayısından daha portalda çalıştırmanız gerekiyorsa, tıklatın **Yardım + Destek** bir destek isteği gönderilemedi. Data Lake Analytics hesabınızı aynı anda çalışabilir işlerin sayısı artırılabilir.
>

### <a name="how-long-to-keep-job-metadata-and-resources"></a>Ne kadar süreyle kalmasını iş meta veri ve kaynakları 
Kullanıcılarınızın U-SQL işleri çalıştırdığınızda, Data Lake Analytics hizmeti tüm ilişkili dosyaları korur. İlgili dosyalar U-SQL betiği, U-SQL betiği, derlenmiş kaynaklar ve İstatistikler başvurulan DLL dosyaları içerir. Varsayılan Azure Data Lake Store hesabına /system/ klasöründe dosyalarıdır. Bu ilke, bu kaynakları otomatik olarak silinmeden önce ne kadar süreyle depolanır denetler (varsayılan 30 gündür). Hata ayıklama ve gelecekte yeniden işleri için performans ayarlama, bu dosyaları kullanabilirsiniz.

Ne kadar süreyle iş meta veri ve kaynakların korunmasına değiştirmek için:

1. Azure Portal'da Data Lake Analytics hesabınıza gidin.
2. **Özellikler**'e tıklayın.
3. Altında **iş sorguları korumak için gün**, bir değer seçmek için kaydırıcıyı hareket ettirin veya metin kutusuna bir değer girin.  
4. **Kaydet**’e tıklayın.

## <a name="job-level-policies"></a>Proje düzeyi ilkeleri

İş düzeyinde ilkeleriyle maksimum Avustralya ve bireysel kullanıcılar (veya belirli güvenlik gruplarının üyeleri) bunlar gönderme işlere ayarlayabilirsiniz en yüksek öncelik kontrol edebilirsiniz. Bu ilke, kullanıcılar tarafından maliyetleri denetlemenize izin verir. Ayrıca sağlar denetimi zamanlanmış bir iş etkisi aynı Data Lake Analytics hesabı çalıştıran yüksek öncelikli üretim işlerini olabilir.

Data Lake Analytics işi düzeyinde ayarlayabilirsiniz iki ilke vardır:

* **İş başına AU sınırı**: kullanıcılar yalnızca bu Avustralya sayıya sahip işleri gönderme. Varsayılan olarak, bu sınır AU sınırını hesabı ile aynıdır.
* **Öncelik**: kullanıcılar yalnızca bu değere eşit veya değerinden daha düşük bir öncelik sahip işleri gönderme. Daha yüksek bir sayı daha düşük bir öncelik gösterir. Varsayılan olarak, bu sınır, olası en yüksek öncelikli olduğu 1 olarak ayarlanır.

Varsayılan ilke her hesabında kümesi yok. Varsayılan ilke, hesap tüm kullanıcılar için geçerlidir. Ek ilkeler belirli kullanıcılar ve gruplar için ayarlayabilirsiniz. 

> [!NOTE]
> Hesap düzeyinde ilkeler ve iş düzeyinde ilkeleri aynı anda uygulayın.
>

### <a name="add-a-policy-for-a-specific-user-or-group"></a>Belirli bir kullanıcı veya grup için bir ilke ekleme

1. Azure Portal'da Data Lake Analytics hesabınıza gidin.
2. **Özellikler**'e tıklayın.
3. Altında **iş gönderim sınırları**, tıklatın **ilke Ekle** düğmesi. Ardından, seçin veya aşağıdaki ayarları girin:
    1. **İlke adı işlem**: ilke amacı anımsatmak için bir ilke adı girin.
    2. **Kullanıcı veya Grup Seç**: kullanıcı veya grubun bu ilkenin uygulandığı seçin.
    3. **İş AU sınırı**: Seçili kullanıcı veya grup için geçerlidir AU sınırı ayarlar.
    4. **Öncelik sınırı**: Seçili kullanıcı veya grup için geçerli bir öncelik sınırı.

4. **Tamam**’a tıklayın.

5. Yeni ilke listelenen **varsayılan** altında İlkesi tablo **iş gönderim sınırları**. 

### <a name="delete-or-edit-an-existing-policy"></a>Silin veya mevcut bir ilkeyi Düzenle

1. Azure Portal'da Data Lake Analytics hesabınıza gidin.
2. **Özellikler**'e tıklayın.
3. Altında **iş gönderim sınırları**, düzenlemek istediğiniz ilkeyi bulun.
4.  Görmek için **silmek** ve **Düzenle** en sağdaki sütun tablonun seçenekleri `...`.

## <a name="additional-resources-for-job-policies"></a>İş ilkeleri için ek kaynaklar
* [İlke genel bakış blog gönderisi](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-overview/)
* [Hesap düzeyinde ilkeler blog gönderisi](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-account-level-policy/)
* [Proje düzeyi ilkeleri blog gönderisi](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-job-level-policy/)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Azure portalını kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
* [Azure PowerShell kullanarak Azure Data Lake Analytics'i yönetme](data-lake-analytics-manage-use-powershell.md)

