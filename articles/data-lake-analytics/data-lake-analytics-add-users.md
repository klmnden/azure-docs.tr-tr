---
title: Bir Azure Data Lake Analytics hesabı için kullanıcı ekleme
description: Kullanıcılar, Data Lake Analytics hesabınızı düzgün eklemeyi öğrenin
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
manager: kfile
editor: jasonwhowell
ms.assetid: db35f16e-1565-4873-a851-bd987accdc58
ms.topic: conceptual
ms.date: 05/24/2018
ms.openlocfilehash: 58b78c7d9769069f36c9f01c2a7650878a6c5ec9
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34717235"
---
# <a name="adding-a-user-in-the-azure-portal"></a>Azure portalında bir kullanıcı ekleme

## <a name="start-the-add-user-wizard"></a>Başlangıç Kullanıcı Ekleme Sihirbazı
1. Aracılığıyla, Azure Data Lake Analytics'i açın https://portal.azure.com.
2. Tıklatın **Kullanıcı Ekleme Sihirbazı**.
3. İçinde **kullanıcı** adımı, eklemek istediğiniz kullanıcıyı bulun. **Seç**'e tıklayın.
4. **Select rol** adım, çekme **Data Lake Analytics Geliştirici**. Bu rol Gönder/izleme/yönetme U-SQL işleri için gereken izinler minimum kümesi vardır. Grup Azure hizmetleri yönetmek için tasarlanmamıştır, bu role atayın.
5. İçinde **seçin katalog izinleri** adım, tüm ek veritabanlarını seçin bu kullanıcının erişimi gerekir. Okuma ve yazma erişimi ana veritabanına iş göndermek için gereklidir. İşiniz bittiğinde **Tamam**’a tıklayın.
6. Son adımda adlı **seçili izinleri atamak** sihirbaz yapacak değişiklikleri gözden geçirin. **Tamam**’a tıklayın.


## <a name="configure-acls-for-data-folders"></a>Veri klasörleri için ACL'leri yapılandırın
"R-X" veya "RWX", giriş verisi içeren klasörlerde gerektiğinde verin ve çıktı verilerini.


## <a name="optionally-add-the-user-to-the-azure-data-lake-store-role-reader-role"></a>İsteğe bağlı olarak, Azure Data Lake Store rolüne kullanıcı ekleme **okuyucu** rol.
1.  Azure Data Lake Store hesabınızı bulun.
2.  Tıklayın **kullanıcılar**.
3. **Ekle**'ye tıklayın.
4.  Bu gruba atamak için bir Azure RBAC rolü seçin.
5.  Okuyucu rolüne atayın. Bu rol Gözat/ADLS içinde depolanan verileri yönetmek için gereken izinler minimum kümesi vardır. Grup Azure hizmetleri yönetmek için tasarlanmamıştır, bu role atayın.
6.  Grubun adını yazın.
7.  **Tamam**’a tıklayın.

## <a name="adding-a-user-using-powershell"></a>PowerShell kullanarak bir kullanıcı ekleme

1. Bu Kılavuzu'ndaki yönergeleri izleyin: [Azure PowerShell'i yükleme ve yapılandırma nasıl](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
2. Karşıdan [Ekle AdlaJobUser.ps1](https://github.com/Azure/AzureDataLake/blob/master/Samples/PowerShell/ADLAUsers/Add-AdlaJobUser.ps1) PowerShell Betiği.
3. PowerShell komut dosyasını çalıştırın. 

İşlerini göndermek için kullanıcı erişimi vermek için örnek komut, yeni iş meta veri ve eski meta veriler görünümü görüntüleyin:

`Add-AdlaJobUser.ps1 -Account myadlsaccount -EntityToAdd 546e153e-0ecf-417b-ab7f-aa01ce4a7bff -EntityType User -FullReplication`


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Azure portalını kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
* [Azure PowerShell kullanarak Azure Data Lake Analytics'i yönetme](data-lake-analytics-manage-use-powershell.md)

