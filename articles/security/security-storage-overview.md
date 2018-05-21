---
title: Azure Storage ile kullanılan güvenlik özellikleri | Microsoft Docs
description: Bu makalede Azure Storage ile kullanılan Azure güvenlik özellikleri çekirdek genel bir bakış sağlar.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: 9558f1ec0d8ccd83da764a0967fa83d93e1e6a02
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="azure-storage-security-overview"></a>Azure depolama güvenliğine genel bakış
Azure Depolama, müşterilerin ihtiyaçlarını karşılamak üzere sağlamlık, kullanılabilirlik ve ölçeklenebilirliğe dayanan modern uygulamalara yönelik bulut depolama çözümüdür. Azure depolama kapsamlı bir güvenlik özellikleri sağlar. Şunları yapabilirsiniz:

* Depolama hesabı rol tabanlı erişim denetimi (RBAC) ve Azure Active Directory kullanarak güvenli hale getirin.
* Bir uygulama ile Azure arasında Aktarımdaki verileri istemci tarafı şifreleme, HTTPS veya SMB 3.0 kullanarak güvenli hale getirin.
* Azure depolama birimine depolama hizmeti şifrelemesi kullanılarak yazılır olduğunda otomatik olarak şifrelenecek veri kümesi.
* Azure Disk şifrelemesi kullanılarak şifrelenmesi için sanal makineler (VM'ler) tarafından kullanılan kümesi işletim sistemi ve veri diskleri.
* Paylaşılan erişim imzaları (SASs) kullanarak Azure storage'da veri nesneleri atanmış erişim izni.
* Analytics depolama eriştiklerinde birisi kullanarak kimlik doğrulama yöntemini izlemek için kullanın.

Azure Storage güvenlik daha ayrıntılı bir bakış için bkz: [Azure Storage Güvenlik Kılavuzu'nu](../storage/common/storage-security-guide.md). Bu kılavuz, Azure Storage'nın güvenlik özelliklerine içine derinlemesine bakış sağlar. Bu özellikler, depolama hesabı anahtarları, veri şifreleme Aktarımdaki ve rest ve depolama çözümlemeleri içerir.

Bu makalede Azure Storage ile birlikte kullanabileceğiniz Azure güvenlik özelliklerine genel bakış sağlar. Daha fazla bilgi edinmek için makalelerinin bağlantıları her bir özelliğin ayrıntılarını verir.

## <a name="role-based-access-control"></a>Rol Tabanlı Access Control
Rol tabanlı erişim denetimi kullanarak depolama hesabınıza güvenli yardımcı olabilir. Erişimi kısıtlamak temel alarak [bilmeniz](https://en.wikipedia.org/wiki/Need_to_know) ve [en az ayrıcalık](https://en.wikipedia.org/wiki/Principle_of_least_privilege) güvenlik ilkeleri kesinlik temelli veri erişimi için güvenlik ilkelerini zorlamak istiyorsanız kuruluşlar için. Bu erişim haklarını grupları ve belirli bir kapsamda uygulamalara uygun RBAC rolü atayarak verilir. Kullanabileceğiniz [yerleşik RBAC rolleri](../role-based-access-control/built-in-roles.md), ayrıcalıkları kullanıcılara atamak için depolama hesabı katılımcı gibi.

Daha fazla bilgi edinin:

* [Azure Active Directory rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md)

## <a name="delegated-access-to-storage-objects"></a>Depolama nesneleri yetkilendirilmiş erişim
Paylaşılan erişim imzası temsilci depolama hesabınızdaki kaynaklara erişim sağlar. SAS anlamına gelir, bir istemci belirli bir süre için ve belirtilen bir izin kümesi ile sınırlı depolama hesabındaki nesnelere izinleri verebilirsiniz. Hesap erişim tuşlarınızı paylaşmak gerek kalmadan bu sınırlı izinleri verebilirsiniz. 

SAS depolama kaynağı için kimlik doğrulamalı erişim için gerekli tüm bilgileri kendi sorgu parametrelerini kapsayan bir URI değil. SAS ile depolama kaynaklarına erişmek için istemcinin yalnızca uygun Oluşturucusu veya yöntem SAS sağlaması gerekir.

Daha fazla bilgi edinin:

* [SAS modelini anlama](../storage/common/storage-dotnet-shared-access-signature-part-1.md)
* [Oluşturma ve bir SAS Blob storage'ı kullanma](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md)

## <a name="encryption-in-transit"></a>Aktarımdaki şifreleme
Şifreleme Aktarımdaki ağlar üzerinden iletilirken veri koruma, bir mekanizmadır. Azure Storage ile kullanarak verilerin güvenliğini sağlayabilirsiniz:

* [Aktarım düzeyinde şifreleme](../storage/common/storage-security-guide.md#encryption-in-transit), içine veya dışına Azure Storage veri aktarırken HTTPS gibi.
* [Şifreleme wire](../storage/common/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), SMB 3.0 şifreleme gibi Azure için dosya paylaşımları.
* [İstemci tarafı şifreleme](../storage/common/storage-security-guide.md#using-client-side-encryption-to-secure-data-that-you-send-to-storage), depolama alanına transfer önce verileri şifrelemek için ve depolama alanı biterse aktarıldıktan sonra verilerin şifresini çözmek için.

İstemci tarafı şifreleme hakkında daha fazla bilgi edinin:

* [Microsoft Azure depolama için istemci tarafı şifreleme](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
* [Bulut güvenlik denetimleri serisi: Aktarımdaki verileri şifreleme](http://blogs.microsoft.com/cybertrust/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme
Çoğu kuruluş için [bekleyen verileri şifreleme](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) veri gizliliği, uyumluluk ve veri egemenliği doğru zorunlu bir adımdır. Üç Azure özellikleri, bekleme durumundayken verilerin şifrelenmesini sağlar:

* [Depolama hizmeti şifrelemesi](../storage/common/storage-security-guide.md#encryption-at-rest) depolama birimi hizmeti otomatik olarak verileri Azure depolama birimine yazılırken şifrelemeniz istemenizi sağlar.
* [İstemci tarafı şifreleme](../storage/common/storage-security-guide.md#client-side-encryption) bekleyen şifreleme özelliği de sağlar.
* [Azure Disk şifrelemesi](../storage/common/storage-security-guide.md#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) , işletim sistemi ve bir Iaas sanal makineyi kullandığı veri disklerle şifrelemenizi sağlar.

Depolama hizmeti şifrelemesi hakkında daha fazla bilgi edinin:

* [Azure depolama hizmeti şifrelemesi](https://azure.microsoft.com/services/storage/) için kullanılabilir [Azure Blob Depolama](https://azure.microsoft.com/services/storage/blobs/). Diğer Azure depolama türleri hakkında daha fazla bilgi için bkz: [Azure dosyaları](https://azure.microsoft.com/services/storage/files/), [Disk (Premium depolama)](https://azure.microsoft.com/services/storage/premium-storage/), [tablo depolama](https://azure.microsoft.com/services/storage/tables/), ve [kuyruk depolama](https://azure.microsoft.com/services/storage/queues/).
* [Rest verileri için Azure Storage hizmeti şifreleme](../storage/common/storage-service-encryption.md)

## <a name="azure-disk-encryption"></a>Azure Disk Şifrelemesi
Sanal makineler için Azure Disk şifrelemesi adresi Kuruluş güvenliği ve uyumluluk gereksinimlerine yardımcı olur. Anahtar ve içinde denetleyen ilkeler kullanılarak (önyükleme ve veri diskleri dahil), VM diskleri şifreler [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/).

Disk şifrelemesi VM'ler için Linux ve Windows işletim sistemleri için çalışır. Ayrıca, koruma, yönetin ve disk şifreleme anahtarlarınızı kullanımını denetleme yardımcı olmak için anahtar kasası kullanır. VM disklerdeki tüm veriler bekleme durumundayken endüstri standardı şifreleme teknolojisi Azure storage hesaplarınızı kullanılarak şifrelenir. Windows için Disk şifrelemesi çözüm dayanır [Microsoft BitLocker Sürücü Şifrelemesi](https://technet.microsoft.com/library/cc732774.aspx), ve Linux çözüm dayanır [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt).

Daha fazla bilgi edinin:

* [Windows ve Linux Iaas sanal makineler için Azure Disk şifrelemesi](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## <a name="azure-key-vault"></a>Azure Key Vault
Azure Disk şifrelemesi kullanan [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) denetlemek ve disk şifreleme anahtarları ve gizli anahtar kasası aboneliğinizde yönetmenize yardımcı olacak. Ayrıca, sanal makine disklerini tüm verileri Azure Storage REST şifrelenmesini sağlar. Anahtar kasası, anahtarları ve ilke kullanımı denetlemek için kullanmanız gerekir.

Daha fazla bilgi edinin:

* [Azure anahtar kasası nedir?](../key-vault/key-vault-whatis.md)
* [Azure anahtar kasası ile çalışmaya başlama](../key-vault/key-vault-get-started.md)
