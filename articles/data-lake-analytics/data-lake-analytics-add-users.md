---
title: Bir Azure Data Lake Analytics hesabı için kullanıcı ekleme
description: Kullanıcılar, Data Lake Analytics hesabınıza doğru eklemeyi öğrenin
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: db35f16e-1565-4873-a851-bd987accdc58
ms.topic: conceptual
ms.date: 05/24/2018
ms.openlocfilehash: 8323c4e1b236444f55dab826d2567491f5f0f736
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60629331"
---
# <a name="adding-a-user-in-the-azure-portal"></a>Azure portalında bir kullanıcı ekleme

## <a name="start-the-add-user-wizard"></a>Başlangıç Kullanıcı Ekleme Sihirbazı
1. Aracılığıyla, Azure Data Lake Analytics'i açın https://portal.azure.com.
2. Tıklayın **Kullanıcı Ekleme Sihirbazı**.
3. İçinde **Kullanıcı Seç** adımı, eklemek istediğiniz kullanıcıyı bulun. **Seç**'e tıklayın.
4. **rol seçme** adım, çekme **Data Lake Analytics geliştiricisi**. Bu rol, en düşük U-SQL işlerini gönderme/İzleyici/yönetme için gereken izinler kümesini sahiptir. Grup, Azure hizmetlerini yönetmek için tasarlanmamıştır, bu role atayın.
5. İçinde **katalog izinleri seçin** adım, herhangi bir ek veritabanlarını seçin, kullanıcı erişimi gerekir. Ana veritabanı için okuma ve yazma iş göndermek için gereklidir. İşiniz bittiğinde **Tamam**’a tıklayın.
6. Son adım olarak adlandırılan **seçili izinleri atama** Sihirbazı yaptığınız değişiklikleri gözden geçirin. **Tamam** düğmesine tıklayın.


## <a name="configure-acls-for-data-folders"></a>Veri klasörleri için ACL'leri yapılandırma
"R-X" veya "RWX" gerektiğinde, girdi verilerini içeren klasörlerde vermek ve çıktı verilerini.


## <a name="optionally-add-the-user-to-the-azure-data-lake-storage-gen1-role-reader-role"></a>İsteğe bağlı olarak, Azure Data Lake depolama Gen1 rolüne kullanıcı ekleme **okuyucu** rol.
1.  Azure Data Lake depolama Gen1 hesabınızı bulun.
2.  Tıklayarak **kullanıcılar**.
3. **Ekle**'ye tıklayın.
4.  Bu grup atamak için bir Azure RBAC rolü seçin.
5.  Okuyucu rolüne atayın. Bu rol, en düşük Gözat/ADLSGen1 içinde depolanan verileri yönetmek için gereken izinler kümesini sahiptir. Grup, Azure hizmetlerini yönetmek için tasarlanmamıştır, bu role atayın.
6.  Grubun adını yazın.
7.  **Tamam** düğmesine tıklayın.

## <a name="adding-a-user-using-powershell"></a>PowerShell kullanarak bir kullanıcı ekleme

1. Bu kılavuzdaki yönergeleri izleyin: [Azure PowerShell'i yükleme ve yapılandırma işlemini](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
2. İndirme [Ekle AdlaJobUser.ps1](https://github.com/Azure/AzureDataLake/blob/master/Samples/PowerShell/ADLAUsers/Add-AdlaJobUser.ps1) PowerShell Betiği.
3. PowerShell betiğini çalıştırın. 

İşleri göndermek için kullanıcı erişimi vermek için örnek komut, yeni iş meta verileri ve eski meta veriler görünümü görüntüleyin:

`Add-AdlaJobUser.ps1 -Account myadlsaccount -EntityToAdd 546e153e-0ecf-417b-ab7f-aa01ce4a7bff -EntityType User -FullReplication`


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Azure portalını kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
* [Azure PowerShell kullanarak Azure Data Lake Analytics'i yönetme](data-lake-analytics-manage-use-powershell.md)

