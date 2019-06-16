---
title: Azure Data Lake Analytics ilkelerini yönetme
description: Bir Data Lake Analytics hesabının kullanımını denetlemek için ilkeler kullanmayı öğrenin.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: 0a6102d1-7554-4df2-b487-4dae9a7287b6
ms.topic: conceptual
ms.date: 04/30/2018
ms.openlocfilehash: 64095f6706bb978cd33b8fe7833fe4e65fc3b0f8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60813418"
---
# <a name="manage-azure-data-lake-analytics-using-policies"></a>İlkeleri kullanarak Azure Data Lake Analytics'i yönetme

Hesap ilkeleri kullanarak kaynakların bir Azure Data Lake Analytics hesabı şeklini denetleyebilirsiniz kullanılır. Bu ilkeler, Azure Data Lake Analytics'i kullanarak maliyeti denetlemenize olanak tanır. Örneğin, bu ilkelerle hesabı aynı anda kullanabileceğiniz kaç AU sınırlayarak beklenmeyen maliyet artışlarını engelleyebilirsiniz.

## <a name="account-level-policies"></a>Hesap düzeyinde ilkeler

Bu ilkeleri, bir Data Lake Analytics hesabı tüm işler için geçerlidir.

### <a name="maximum-number-of-aus-in-a-data-lake-analytics-account"></a>Bir Data Lake Analytics hesabında en fazla AU sayısını
Bir ilke Data Lake Analytics hesabınızı kullanabilirsiniz analiz birimi (AU) toplam sayısını denetler. Varsayılan değer 250'ye ayarlanır. Örneğin, bu değer 250'ye ayarlanmışsa Avustralya, 250 ile çalışan işiniz olabilir veya 10 işleri çalıştıran 25'ine atanan AU her AU. Gönderilen ek işler çalışan işler tamamlanana kadar kuyruğa alınır. Çalışan işleri bittiğinde çalıştırmak için sıraya alınan iş için AU kurtulurlar.

Data Lake Analytics hesabınız için AU sayısını değiştirmek için:

1. Azure portalında Data Lake Analytics hesabınıza gidin.
2. **Özellikler**'e tıklayın.
3. Altında **maksimum AUs**, bir değer seçmek için kaydırıcıyı taşıyın veya metin kutusuna bir değer girin. 
4. **Kaydet**’e tıklayın.

> [!NOTE]
> En fazla ' % s'varsayılan (250) gerekiyorsa AU portalında **Yardım + Destek** için bir destek isteği gönderin. Data Lake Analytics hesabınızda kullanılabilen AU sayısı artırılabilir.
>

### <a name="maximum-number-of-jobs-that-can-run-simultaneously"></a>En fazla eşzamanlı olarak çalışabilecek iş sayısı
Bir ilke, aynı anda kaç tane işin çalıştırıp denetler. Varsayılan olarak, bu değeri 20'ye ayarlanır. Data Lake Analytics Au'lar kullanılabilir varsa, yeni işleri hemen çalışan işlerin toplam sayısı, bu ilkenin değerini ulaşana kadar çalışmak üzere zamanlanır. Aynı anda çalışan işlerin sayısı ulaştığında (AU kullanılabilirliğine bağlı olarak) bir veya daha fazla çalışan iş tamamlanana kadar sonraki işleri öncelik sırasına göre kuyruğa alınır.

Aynı anda çalışabilecek iş sayısını değiştirmek için:

1. Azure portalında Data Lake Analytics hesabınıza gidin.
2. **Özellikler**'e tıklayın.
3. Altında **en büyük sayı, çalışan işleri**, bir değer seçmek için kaydırıcıyı taşıyın veya metin kutusuna bir değer girin. 
4. **Kaydet**’e tıklayın.

> [!NOTE]
> İşleri, varsayılan (20) sayısından daha fazla portalda çalıştırmanız gerekiyorsa, tıklayın **Yardım + Destek** için bir destek isteği gönderin. Data Lake Analytics hesabınızda aynı anda çalışan işlerin sayısı artırılabilir.
>

### <a name="how-long-to-keep-job-metadata-and-resources"></a>Ne kadar süreyle kalmasını iş meta verileri ve kaynakları 
Kullanıcılarınızın, U-SQL işleri çalıştırdığınızda, tüm ilişkili dosyalar Data Lake Analytics hizmetine korur. İlgili dosyalar, U-SQL betiği, U-SQL betiği, derlenmiş kaynakları ve istatistikleri başvurulan DLL dosyaları içerir. Varsayılan Azure Data Lake Store hesabının /system/ klasöründe dosyalarıdır. Bu ilke, bu kaynakları otomatik olarak silinmeden önce ne kadar süreyle saklanır denetler (varsayılan 30 gündür). Hata ayıklama ve gelecekte yeniden çalıştırmanız işleri için performans ayarlama, bu dosyaları kullanabilirsiniz.

İş meta verileri ve kaynakları korumak için ne kadar süreyle değiştirmek için:

1. Azure portalında Data Lake Analytics hesabınıza gidin.
2. **Özellikler**'e tıklayın.
3. Altında **iş sorgularının korumak için gün**, bir değer seçmek için kaydırıcıyı taşıyın veya metin kutusuna bir değer girin.  
4. **Kaydet**’e tıklayın.

## <a name="job-level-policies"></a>Proje düzeyi ilkeleri

Proje düzeyi ilkeleri, maksimum AUS değerini ve bireysel kullanıcılar (veya belirli güvenlik gruplarının üyesi) bunlar gönderme işlerde ayarlayabileceğiniz önceliğini kontrol edebilirsiniz. Bu ilke, kullanıcılar tarafından gerçekleştirilen giderleri denetlemenizi sağlar. Ayrıca sağlar denetimi zamanlanmış bir iş etkisi yüksek öncelikli üretim projelerde aynı Data Lake Analytics hesabında çalışıyor olabilir.

Data Lake Analytics proje düzeyinde ayarlayabileceğiniz iki ilke sahiptir:

* **İş başına AU sınırı**: Kullanıcılar yalnızca AU bu sayıya sahip bir iş gönderebilirsiniz. Varsayılan olarak, bu sınır AU sınırını hesabı ile aynıdır.
* **Öncelik**: Kullanıcılar yalnızca bu değere eşit veya daha düşük bir önceliğe sahip iş gönderebilirsiniz. Daha yüksek bir sayı daha düşük bir öncelik gösterir. Varsayılan olarak, bu sınır, olası en yüksek öncelikli olduğu 1 olarak ayarlanır.

Her hesap üzerinde ayarlanmış varsayılan bir ilke yoktur. Varsayılan ilke, tüm hesap kullanıcıları için geçerlidir. Belirli kullanıcılar ve gruplar için ek ilkeler ayarlayabilir. 

> [!NOTE]
> Hesap düzeyinde ve proje düzeyinde ilkelerinin eşzamanlı olarak uygulanır.
>

### <a name="add-a-policy-for-a-specific-user-or-group"></a>Belirli bir kullanıcı veya grup için bir ilke Ekle

1. Azure portalında Data Lake Analytics hesabınıza gidin.
2. **Özellikler**'e tıklayın.
3. Altında **iş gönderme sınırları**, tıklayın **ilke Ekle** düğmesi. Sonra seçin veya aşağıdaki ayarları girin:
    1. **İşlem İlkesi adı**: İlkenin amacı hatırlatmak için bir ilke adı girin.
    2. **Kullanıcı veya Grup Seç**: Kullanıcı veya grup bu ilkenin uygulanacağı seçin.
    3. **İş AU sınırını ayarlayın**: Seçilen kullanıcı veya grup için geçerlidir AU sınırını ayarlayın.
    4. **Önceliği sınırını ayarlayın**: Seçilen kullanıcı veya grup için geçerlidir önceliği sınırını ayarlayın.

4. **Tamam**’a tıklayın.

5. Yeni ilkede listelenen **varsayılan** altında ilke tablo **iş gönderme sınırları**. 

### <a name="delete-or-edit-an-existing-policy"></a>Silebilir veya var olan bir ilkeyi düzenleyebilirsiniz.

1. Azure portalında Data Lake Analytics hesabınıza gidin.
2. **Özellikler**'e tıklayın.
3. Altında **iş gönderme sınırları**, düzenlemek istediğiniz ilkenin bulun.
4.  Görmek için **Sil** ve **Düzenle** tablonun en sağdaki sütunda seçenekleri `...`.

## <a name="additional-resources-for-job-policies"></a>İş ilkeleri için ek kaynaklar
* [İlke genel bakış blog gönderisi](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-overview/)
* [Hesap düzeyinde ilkeler blog gönderisi](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-account-level-policy/)
* [Proje düzeyinde ilkeler blog gönderisi](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-job-level-policy/)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Azure portalını kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
* [Azure PowerShell kullanarak Azure Data Lake Analytics'i yönetme](data-lake-analytics-manage-use-powershell.md)

