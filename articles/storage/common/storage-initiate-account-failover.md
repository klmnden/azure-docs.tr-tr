---
title: Bir depolama hesabı yük devretme (Önizleme) - Azure depolama başlatın
description: Depolama hesabınızın birincil uç noktaya kullanılamaz hale gelmesi durumunda, bir hesap yük devretme başlatma hakkında bilgi edinin. Depolama hesabınız için birincil bölgesi halinde gelecek ikincil bölgeye yük devretme güncelleştirir.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 02/11/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: fd8eecbd20446bfde8d3a7467e2982398c3b8c19
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59044972"
---
# <a name="initiate-a-storage-account-failover-preview"></a>Bir depolama hesabı yük devretme (Önizleme) başlatın

Coğrafi olarak yedekli depolama hesabınızın birincil uç noktadan herhangi bir nedenle kullanılamaz duruma gelirse, bir hesap yük devretme (Önizleme) başlatabilirsiniz. Bir hesap yük devretme için depolama hesabınızın birincil uç nokta olacak ikincil uç güncelleştirir. Yük devretme işlemi tamamlandıktan sonra istemciler yeni birincil bölgeye yazma başlayabilirsiniz. Zorlamalı yük devretme, uygulamalarınız için yüksek kullanılabilirliği sürdürmek sağlar.

Bu makalede, Azure portal, PowerShell veya Azure CLI kullanarak depolama hesabınız için bir hesap yük devretmeyi başlatmak gösterilmektedir. Hesap yük devretme hakkında daha fazla bilgi için bkz: [olağanüstü durum kurtarma ve hesabı yük devretme (Önizleme) Azure Depolama'daki](storage-disaster-recovery-guidance.md).

> [!WARNING]
> Bir hesap yük devretme, genellikle bazı veri kaybı ile sonuçlanır. Bir hesap yük devretme etkilerini anlamak ve veri kaybı için hazırlamak üzere gözden [hesap yük devretme işlemi anlamak](storage-disaster-recovery-guidance.md#understand-the-account-failover-process).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Depolama hesabınızdaki bir hesabı yük devretme gerçekleştirmeden önce aşağıdaki adımları gerçekleştirdiğinizden emin olun:

- Hesap yük devretme Önizleme için kaydolun. Kaydetme hakkında daha fazla bilgi için bkz: [önizlemesi hakkında](storage-disaster-recovery-guidance.md#about-the-preview).
- Depolama hesabınızda coğrafi olarak yedekli depolama (GRS) veya okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) kullanacak şekilde yapılandırıldığından emin olun. Coğrafi olarak yedekli depolama hakkında daha fazla bilgi için bkz. [coğrafi olarak yedekli depolama (GRS): Azure depolama için bölgeler arası çoğaltma](storage-redundancy-grs.md). 

## <a name="important-implications-of-account-failover"></a>Hesap yük devretme önemli etkileri

Depolama hesabınız için bir hesap yük devretme başlattığınızda, ikincil uç birincil uç noktaya dönüşür ikincil uç nokta için DNS kayıtlarını güncelleştirilir. Bir yük devretmeyi başlatmadan önce depolama hesabınıza olası etkilerini anladığınızdan emin olun.

Bir yük devretmeyi başlatmadan önce olası veri kaybı kapsamını tahmin etmek için kontrol **son eşitleme zamanı** özelliğini kullanarak `Get-AzStorageAccount` PowerShell cmdlet'ini ve `-IncludeGeoReplicationStats` parametresi. Denetleyin ardından `GeoReplicationStats` hesabınız için özellik. 

Yük devretmeden sonra otomatik olarak yeni bir birincil bölgede yerel olarak yedekli depolama (LRS), depolama hesabı türü dönüştürülür. Coğrafi olarak yedekli depolama (GRS) veya okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) hesabı için yeniden etkinleştirebilirsiniz. Not LRS'den GRS'ye veya RA-GRS'ye dönüştürme ek bir ücret doğurur. Ek bilgi için bkz: [bant genişliği fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/bandwidth/). 

GRS depolama hesabınız için yeniden etkinleştirdikten sonra Microsoft hesabınızdaki veriler, yeni ikincil bölgeye çoğaltma başlar. Çoğaltma zaman çoğaltılan veri miktarına bağlıdır.  

## <a name="azure-portal"></a>Azure portal

Azure portalından bir hesap yük devretmeyi başlatmak için bu adımları izleyin:

1. Depolama hesabınıza gidin.
2. Altında **ayarları**seçin **coğrafi çoğaltma**. Aşağıdaki resimde, bir depolama hesabı coğrafi çoğaltma ve yük devretme durumunu gösterir.

    ![Coğrafi çoğaltma ve yük devretme durumunu gösteren ekran görüntüsü](media/storage-initiate-account-failover/portal-failover-prepare.png)

3. Depolama hesabınız için coğrafi olarak yedekli depolama (GRS) veya okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) yapılandırıldığını doğrulayın. Yüklü değilse, ardından **yapılandırma** altında **ayarları** coğrafi yedekli olacak şekilde hesabınızı güncelleştirmek için. 
4. **Son eşitleme zamanı** özelliği, ne kadar ikincil birincilden arkasındaysa gösterir. **Son eşitleme zamanı** kapsamın deneyimi, veri kaybı tahmini sağlar. yük devretme tamamlandıktan sonra.
5. Seçin **hazırlama (Önizleme) için yük devretme**. 
6. Onay iletişim kutusunda gözden geçirin. Hazır olduğunuzda girin **Evet** onaylayın ve yük devretme başlatın.

    ![Ekran gösteren onay iletişim kutusunda bir hesap yük devretme](media/storage-initiate-account-failover/portal-failover-confirm.png)

## <a name="powershell"></a>PowerShell

Bir hesap yük devretmeyi başlatmak için PowerShell kullanmak için önce 6.0.1 yüklemelisiniz Önizleme modülü. Modülü yüklemek için aşağıdaki adımları izleyin:

1. Azure PowerShell'in önceki yüklemeleri kaldırın:

    - Windows Azure PowerShell'in önceki yüklemeleri kaldırmaya **uygulamalar ve Özellikler** bölümündeki **ayarları**.
    - Tümünü Kaldır **Azure*** modüllerden `%Program Files%\WindowsPowerShell\Modules`.
    
1. PowerShellGet yüklü en son sürümüne sahip olduğunuzdan emin olun. Bir Windows PowerShell penceresi açın ve en son sürümünü yüklemek için aşağıdaki komutu çalıştırın:
 
    ```powershell
    Install-Module PowerShellGet –Repository PSGallery –Force
    ```
1. Kapatın ve PowerShellGet yükledikten sonra PowerShell penceresi açın. 

1. Azure PowerShell'in en son sürümünü yükleyin:

    ```powershell
    Install-Module Az –Repository PSGallery –AllowClobber
    ```

1. Azure AD destekleyen bir Azure depolama Önizleme modülünü yükleyin:
   
    ```powershell
    Install-Module Az.Storage –Repository PSGallery -RequiredVersion 1.1.1-preview –AllowPrerelease –AllowClobber –Force 
    ```
1. PowerShell penceresini kapatıp yeniden açın.
 

Powershell'den bir hesabı yük devretmeyi başlatmak için aşağıdaki komutu yürütün:

```powershell
Invoke-AzStorageAccountFailover -ResourceGroupName <resource-group-name> -Name <account-name> 
```

## <a name="azure-cli"></a>Azure CLI

Bir hesap yük devretmeyi başlatmak için Azure CLI kullanmak için aşağıdaki komutları yürütün:

```cli
az storage account show \ --name accountName \ --expand geoReplicationStats
az storage account failover \ --name accountName
```

## <a name="next-steps"></a>Sonraki adımlar

- [Olağanüstü durum kurtarma ve hesabı yük devretme (Önizleme) Azure Depolama'daki](storage-disaster-recovery-guidance.md)
- [RA-GRS’yi kullanarak yüksek kullanılabilirliğe sahip uygulamalar tasarlama](storage-designing-ha-apps-with-ragrs.md)
- [Öğretici: Blob Depolama ile yüksek oranda kullanılabilir bir uygulama oluşturun](../blobs/storage-create-geo-redundant-storage.md) 
