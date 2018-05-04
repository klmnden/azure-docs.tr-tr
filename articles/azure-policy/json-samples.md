---
title: İlke şablonu örnekleri | Microsoft Docs
description: Azure ilke JSON örnekleri
services: azure-policy
documentationcenter: ''
author: DCtheGeek
manager: carmonm
editor: ''
tags: ''
ms.assetid: ''
ms.service: azure-policy
ms.devlang: na
ms.topic: samples
ms.tgt_pltfrm: ''
ms.workload: ''
ms.date: 01/17/2018
ms.author: dacoulte
ms.custom: mvc
ms.openlocfilehash: 3473cb5260773fda0534c4f0aca1db731cce74eb
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2018
---
# <a name="templates-for-azure-policy"></a>Azure ilke şablonları

Aşağıdaki tabloda Azure ilkesi için json şablonları bağlantılarını içerir. Bu örnekler bulunan [Azure ilkesi örnekleri depo](https://github.com/Azure/azure-policy).

| | |
|---|---|
|**İşlem**||
| [Onaylanan VM görüntüleri](scripts/allowed-custom-images.md) | Özel onaylanmış yalnızca görüntüleri ortamınızda dağıtılmasını gerektirir. Onaylanan resim kimlikleri dizisi belirtin. |
| [VM yönetilen Disk kullanmadığında denetleme](scripts/create-vm-managed-disk.md) | Bir sanal makine oluşturulduğunda denetimleri yönetilen diskleri kullanmaz.|
| [Uzantısı yoksa, Denetim](scripts/audit-ext-not-exist.md) | Uzantı sahip bir sanal makine dağıtılmamışsa, denetler. Uzantı yayımcı ve türüne dağıtılan olup olmadığını denetlemek için belirtin. |
| [Bir kaynak grubundan özel VM görüntüsü'izin ver](scripts/allow-custom-vm-image.md) |  Özel resimler onaylanan kaynak grubundan gelen gerektirir. Onaylanan kaynak grubunun adını belirtin. |
| [Karma kullanımı avantajı Reddet](scripts/deny-hybrid-use.md) | Azure karma kullanma Avantajı (AHUB), kullanım engelliyor. Şirket içi lisansları kullanımına izin vermek istediğinizde değil işlevini kullanın. |
| [VM uzantıları izin verilmiyor](scripts/not-allowed-vm-ext.md) | Belirtilen uzantıları kullanımını engeller. Yasaklanan uzantı türlerini içeren bir dizi belirtin. |
| [Yalnızca belirli bir VM platform görüntüsü izin ver](scripts/allow-certain-vm-image.md) | Sanal makineler UbuntuServer belirli bir sürümünü kullanmanız gerekir. |
| [Yönetilen diski kullanarak VM oluşturma](scripts/use-managed-disk-vm.md) | Sanal makineler yönetilen diskleri kullanmanızı gerektirir.|
|**İzleme**||
| [Tanılama ayarını denetleme](scripts/audit-diag-setting.md) | Kaynak türleri için etkinleştirilmemiş tanılama ayarlarını belirtilmişse denetler. Tanılama ayarları etkin olup olmadığını denetlemek için kaynak türleri dizisi belirtin. |
|**Ad ve metin kuralları**||
| [Birden çok adı desenleri izin ver](scripts/allow-multiple-name-patterns.md) | Kaynaklar için kullanılacak birçok adı desenleri birini izin verir. |
| [LIKE desen gerektirir](scripts/enforce-like-pattern.md) | Kaynak adları desenini LIKE koşula emin olun. |
| [Eşleşme deseni gerektirir](scripts/enforce-match-pattern.md) | Kaynak adları adlandırma desenle eşleşen emin olun. |
| [Etiket eşleşme deseni gerektirir](scripts/enforce-tag-match-pattern.md) | Bir etiket değeri metin düzeni eşleştiğinden emin olun. |
|**Ağ**||
| [Uygulama ağ geçidi SKU'ları izin](scripts/allowed-app-gate-sku.md) | Uygulama ağ geçitleri onaylanmış bir SKU kullanmanızı gerektirir. Onaylanan SKU'ları dizisi belirtin. |
| [Ağ İzleyicisi bölge için etkin değilse denetim](scripts/net-watch-not-enabled.md) | Ağ İzleyicisi belirtilen bir bölge için etkin değilse denetler. Ağ İzleyicisi etkin olup olmadığını denetlemek için bölge adını belirtin. |
| [NSG X her NIC üzerinde](scripts/nsg-on-nic.md) | Belirli bir ağ güvenlik grubu ile her sanal ağ arabirimi kullanılır gerektirir. Kullanılacak ağ güvenlik grubu kimliği belirtin. |
| [Her alt ağ üzerinde NSG X](scripts/nsg-on-subnet.md) | Belirli ağ güvenlik grubu ile her bir sanal alt kullanılmasını gerektirir. Kullanılacak ağ güvenlik grubu kimliği belirtin. |
| [İzin verilen hızlı rota bant genişliği](scripts/allowed-er-band.md) | Express yolları belirlenen bant genişlikleri kullanmanızı gerektirir. Hızlı rota için belirtilen SKU'ları dizisi belirtin. |
| [Eşleme konumu hızlı rota için izin verilen](scripts/allowed-peering-er.md) | Belirtilen eşleme konumları Express yollar kullan gerektirir. İzin verilen eşleme konumları dizisi belirtin. |
| [İzin verilen hızlı rota SKU'ları](scripts/allowed-er-skus.md) | Express yollar onaylanmış bir SKU kullanmanızı gerektirir. İzin verilen SKU'lar dizisi belirtin. |
| [Yük Dengeleyici SKU'ları izin](scripts/allowed-lb-skus.md) | Yük Dengeleyici onaylanmış bir SKU kullanmanızı gerektirir. İzin verilen SKU'lar dizisi belirtin. |
| [ER ağ eşlemesi ağ yok](scripts/no-peering-er-net.md) | Belirtilen kaynak grubu bir ağda ilişkili eşliği ağ engelliyor. Merkezi yönetilen ağ altyapısı ile bağlantı engellemek için kullanın. İlişkilendirme önlemek için kaynak grubu adını belirtin. |
| [Hiçbir kullanıcı tanımlı yönlendirme tablosundaki](scripts/no-user-def-route-table.md)  |Sanal ağlar, bir kullanıcı tanımlı yol tablosu ile dağıtılan engelliyor. |
| [Sanal ağ geçidi SKU'ları izin](scripts/allowed-vn-gate-sku.md) | Sanal ağ geçitlerini onaylanmış bir SKU ve ağ geçidi türü kullanmanızı gerektirir. Onaylanan SKU'ları bir dizi ve onaylanan ağ geçidi türleri dizisi belirtin. |
| [Onaylanan alt ağ için VM ağ arabirimleri kullanın](scripts/use-approved-subnet-vm-nics.md) | Ağ arabirimleri onaylanmış bir alt ağ kullanmanızı gerektirir. Onaylanan alt ağ Kimliğini belirtin. |
| [VM ağ arabirimleri için onaylanan vNet kullanın](scripts/use-approved-vnet-vm-nics.md) | Ağ arabirimleri onaylanmış bir sanal ağı kullanmayı gerektirir. Onaylanan sanal ağ Kimliğini belirtin. |
|**Etiketler**||
| [Faturalama etiketleri İlkesi girişimi](scripts/billing-tags-policy-init.md) | Belirtilen etiket değerleri için maliyet merkezi ve ürün adı gerektirir. Gerekli etiketleri uygulamak ve zorlamak için yerleşik ilkeleri kullanır. Etiketler için gereken değerleri belirtin.  |
| [Etiket ve kaynak grupları değerine zorla](scripts/enforce-tag-rg.md) | Bir etiketi ve bir kaynak grubu üzerinde değer gerektirir. Gerekli etiket adını ve değerini belirtin.  |
|**SQL**||
| [SQL DB düzeyi denetim ayarını denetleme](scripts/audit-sql-db-audit-setting.md) | Bu ayarları belirtilmiş bir ayarın eşleşmiyorsa SQL veritabanı denetim ayarları denetler. Denetim ayarlarını etkin veya devre dışı olup olmadığını belirten bir değer belirtin.  |
| [Saydam veri şifreleme durumunu denetleme](scripts/audit-trans-data-enc-status.md) | SQL veritabanı saydam veri şifrelemesi etkin olmadığını denetler.  |
| [DB düzeyi tehdit algılama ayarı denetleme](scripts/audit-db-threat-det-setting.md) | Bu ilkeleri belirli durumda ayarlanmamışsa SQL veritabanı güvenlik uyarısı ilkelerini denetler. Tehdit algılama etkin mi yoksa devre dışı mı olduğunu belirten bir değer belirtin.  |
| [SQL Server düzeyi denetim ayarını denetleme](scripts/audit-sql-ser-leve-audit-setting.md) | Bu ayarları belirtilmiş bir ayarın eşleşmiyorsa SQL server denetim ayarlarını denetler. Denetim ayarlarını etkin veya devre dışı olup olmadığını belirten bir değer belirtin. |
| [Sunucu düzeyinde tehdit algılama ayarı denetleme](scripts/audit-sql-ser-threat-det-setting.md) | Bu ilkeleri belirli durumda ayarlanmamışsa SQL veritabanı güvenlik uyarısı ilkelerini denetler. Tehdit algılama etkin mi yoksa devre dışı mı olduğunu belirten bir değer belirtin.  |
| [Hiçbir Azure Active Directory yönetici denetleme](scripts/audit-no-aad-admin.md) | SQL Server'a atanan hiçbir Azure Active Directory yönetici olduğunda denetim. |
| [SQL DB SKU'ları izin](scripts/allowed-sql-db-skus.md) | SQL veritabanları onaylanmış bir SKU kullanmak gerektirir. İzin verilen SKU kimlikleri bir dizi veya bir dizi izin verilen SKU adını belirtin. |
|**Depolama**||
| [Depolama hesapları ve sanal makineler için izin verilen SKU'ları](scripts/allowed-skus-storage.md) | Depolama hesapları ve sanal makineleri onaylanan SKU'ları kullanmanızı gerektirir. Sağlamak için kullandığı yerleşik ilkeleri SKU'ları onaylanmış. Bir dizi onaylanan sanal makinelerin SKU'ları ve bir dizi onaylanan depolama hesabı SKU'ları belirtin. |
| [Depolama hesabı için yalnızca HTTPS trafiği emin olun](scripts/ensure-https-stor-acct.md) | Depolama hesapları HTTPS trafiği kullanmasını gerektirir.  |
| [Depolama hesapları için katmanlama cool erişimini engelle](scripts/deny-cool-access-tiering.md) | Blob storage hesapları için katmanlama seyrek erişimli erişim kullanımını engeller.  |
| [Depolama dosya şifreleme emin olun](scripts/ensure-store-file-enc.md) | Bu dosya şifreleme depolama hesapları için etkinleştirilmesini gerektirir.  |
|**Yerleşik İlkesi**||
| [İzin verilen konumların](scripts/allowed-locs.md) | Tüm kaynaklar onaylanan konumlara dağıtılmasını gerektirir. Bir dizi onaylanan konumları belirtin.  |
| [İzin verilen kaynak türleri](scripts/allowed-res-types.md) | Yalnızca onaylanan kaynak türleri dağıtılan sağlar. İzin verilen kaynak türleri dizisi belirtin.  |
| [Depolama hesabı SKU'ları izin](scripts/allowed-stor-acct-skus.md) | Depolama hesapları onaylanmış bir SKU kullanmanızı gerektirir. Onaylanan SKU'ları dizisi belirtin. |
| [Etiket ve varsayılan değerini Uygula](scripts/apply-tag-def-val.md) | Bu etiket sağlanmazsa, belirtilen etiket ad ve değer ekler. Uygulama için değer ve etiket adı belirtin.  |
| [SQL veritabanı şifreleme denetleme](scripts/sql-database-encryption-audit.md) | SQL veritabanı saydam veri şifreleme etkin yoksa denetler. |
| [SQL Server denetim ayarlarını denetleme](scripts/sql-server-audit.md) | Denetim ayarlarını etkin temel SQL server denetler. |
| [Data Lake Store şifrelemeyi zorunlu kılma](scripts/enforce-datalakestore-encryption.md) | Şifreleme etkin olmayan herhangi bir Data Lake Store hesabı reddeder. |
| [Etiket ve değerini zorla](scripts/enforce-tag-val.md) | Belirtilen etiket adı ve değeri gerektirir. Zorlamak için değer ve etiket adı belirtin.  |
| [Kaynak türleri izin verilmiyor](scripts/not-allowed-res-type.md) | Belirtilen kaynak türleri dağıtımını engelliyor. Bir dizi engellemek için kaynak türlerini belirtin.  |
| [SQL Server sürüm 12.0 gerektirir](scripts/req-sql-12.md) | SQL sunucu sürümü 12.0 kullanmasını gerektirir.  |
| [Depolama hesabı şifreleme iste](scripts/req-store-acct-enc.md) | Depolama hesabı kullan blob şifreleme gerektirir.  |
