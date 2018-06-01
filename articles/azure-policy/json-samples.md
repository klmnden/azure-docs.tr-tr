---
title: İlke şablonu örnekleri
description: Azure İlkesi JSON örnekleri
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 01/17/2018
ms.author: dacoulte
ms.custom: mvc
ms.openlocfilehash: 4b9096c1fb0d9ee74849e259a6e0af2486c5d29b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34195133"
---
# <a name="templates-for-azure-policy"></a>Azure İlkesi şablonları

Aşağıdaki tabloda örnek Azure İlkesi json şablonlarının bağlantıları yer almaktadır. Bu örnekler, [Azure İlkesi örnek deposunda](https://github.com/Azure/azure-policy) bulunur.

| | |
|---|---|
|**İşlem**||
| [Onaylı VM görüntüleri](scripts/allowed-custom-images.md) | Ortamınızda yalnızca onaylı özel görüntülerin dağıtılmasını gerektirir. Onaylanan bir görüntü kimliği dizisi belirtirsiniz. |
| [VM Yönetilen Disk kullanmadığında denetle](scripts/create-vm-managed-disk.md) | Yönetilen diskler kullanmayan bir sanal makine oluşturulduğunda denetler.|
| [Uzantı yoksa denetle](scripts/audit-ext-not-exist.md) | Sanal makine ile bir uzantı dağıtılmadıysa denetler. Dağıtılıp dağıtılmadığını görmek için uzantı yayımcısı ve türünü belirtirsiniz. |
| [Bir Kaynak Grubundan özel VM kullanımına izin verin](scripts/allow-custom-vm-image.md) |  Özel görüntülerin onaylı bir kaynak grubundan gelmesini gerektirir. Onaylı kaynak grubunun adını belirtirsiniz. |
| [Karma kullanım avantajını reddet](scripts/deny-hybrid-use.md) | Azure Hibrit Kullanım Teklifi (AHUB) kullanımını engeller. Şirket içi lisans kullanımına izin vermek istemediğiniz zaman kullanın. |
| [İzin verilmeyen VM Uzantıları](scripts/not-allowed-vm-ext.md) | Belirtilen uzantıların kullanılmasını engeller. Yasaklanan uzantı türlerini içeren bir dizi belirtirsiniz. |
| [Yalnızca belirli bir VM platform görüntüsüne izin ver](scripts/allow-certain-vm-image.md) | Sanal makinelerin belirli bir UbuntuServer sürümü kullanmasını gerektirir. |
| [Yönetilen Disk kullanarak VM oluştur](scripts/use-managed-disk-vm.md) | Sanal makinelerin yönetilen diskler kullanmasını gerektirir.|
|**İzleme**||
| [Tanılama ayarını denetle](scripts/audit-diag-setting.md) | Belirtilen kaynak türlerinde tanılama ayarlarının etkinleştirilip etkinleştirilmediğini denetler. Tanılama ayarlarının etkin olup olmadığını denetlemek için bir kaynak türü dizisi belirtirsiniz. |
|**Ad ve metin kuralları**||
| [Birden çok adı desenine izin ver](scripts/allow-multiple-name-patterns.md) | Birçok ad deseninden birinin kaynaklarda kullanılmasına izin verir. |
| [Benzer desen gerektir](scripts/enforce-like-pattern.md) | Kaynak adlarının bir desen için benzer koşuluna uyduğundan emin olun. |
| [Eşleşme deseni gerektir](scripts/enforce-match-pattern.md) | Kaynak adlarının adlandırma deseniyle eşleştiğinden emin olun. |
| [Etiket eşleştirme deseni gerektir](scripts/enforce-tag-match-pattern.md) | Bir etiket değerinin metin deseniyle eşleştiğinden emin olun. |
|**Ağ**||
| [İzin verilen Application Gateway SKU’ları](scripts/allowed-app-gate-sku.md) | Uygulama ağ geçitlerinin onaylı bir SKU kullanmasını gerektirir. Onaylı bir SKU dizisi belirtirsiniz. |
| [Ağ İzleyicisi bölge için etkin değilse denetle](scripts/net-watch-not-enabled.md) | Belirli bir bölge için ağ izleyicisinin etkinleştirilip etkinleştirilmediğini denetler. Ağ izleyicisinin etkin olup olmadığını denetlemek için bir bölge adı belirtirsiniz. |
| [Her NIC üzerinde NSG X](scripts/nsg-on-nic.md) | Her sanal ağ arabirimiyle belirli bir ağ güvenlik grubunun kullanılmasını gerektirir. Kullanılacak ağ güvenlik grubunun kimliğini belirtirsiniz. |
| [Her alt ağ üzerinde NSG X](scripts/nsg-on-subnet.md) | Her sanal alt ağ ile belirli bir ağ güvenlik grubunun kullanılmasını gerektirir. Kullanılacak ağ güvenlik grubunun kimliğini belirtirsiniz. |
| [İzin verilen Express Route bant genişliği](scripts/allowed-er-band.md) | Express Route’ların belirli bir bant genişliği kümesini kullanmasını gerektirir. Express Route için belirtilebilen bir SKU dizisi belirtirsiniz. |
| [Express Route için izin verilen Eşleme Konumu](scripts/allowed-peering-er.md) | Express Route’ların belirtilen eşleme konumlarını kullanmasını gerektirir. İzin verilen bir eşleme konumu dizisi belirtirsiniz. |
| [İzin verilen Express Route SKU’ları](scripts/allowed-er-skus.md) | Express Routes’un onaylı bir SKU kullanmasını gerektirir. İzin verilen bir SKU dizisi belirtirsiniz. |
| [İzin verilen Load Balancer SKU'ları](scripts/allowed-lb-skus.md) | Yük dengeleyicilerin onaylı bir SKU kullanmasını gerektirir. İzin verilen bir SKU dizisi belirtirsiniz. |
| [ER ağı ile ağ eşlemesi yok](scripts/no-peering-er-net.md) | Bir ağ eşlemesinin belirli bir kaynak grubundaki bir ağ ile ilişkilendirilmesini önler. Merkezi yönetilen ağ altyapısı ile bağlantıyı engellemek için kullanın. İlişkilendirmeyi önlemek için kaynak grubunun adını belirtirsiniz. |
| [Kullanıcı Tanımlı Yol Tablosu Yok](scripts/no-user-def-route-table.md)  |Sanal ağların kullanıcı tanımlı bir yönlendirme tablosu ile dağıtılmasını engeller. |
| [İzin verilen Sanal Ağ Geçidi SKU’ları](scripts/allowed-vn-gate-sku.md) | Sanal ağ geçitlerinin onaylı bir SKU ve ağ geçidi türü kullanmasını gerektirir. Onaylanan bir SKU dizisi ve onaylanan bir ağ geçidi türleri dizisi belirtirsiniz. |
| [VM ağ arabirimlerinde onaylanan alt ağı kullan](scripts/use-approved-subnet-vm-nics.md) | Ağ arabirimlerinin onaylı bir alt ağ kullanmasını gerektirir. Onaylanan alt ağın kimliğini belirtirsiniz. |
| [VM ağ arabirimlerinde onaylanan sanal ağı kullan](scripts/use-approved-vnet-vm-nics.md) | Ağ arabirimlerinin onaylı bir sanal ağ kullanmasını gerektirir. Onaylanan sanal ağın kimliğini belirtirsiniz. |
|**Etiketler**||
| [Fatura Etiketleri İlkesi Girişimi](scripts/billing-tags-policy-init.md) | Maliyet merkezi ve ürün adı için belirtilen etiket değerlerini gerektirir. Gerekli etiketleri uygulamak ve zorlamak için yerleşik ilkeleri kullanır. Etiketler için gereken değerleri belirtin.  |
| [Etiket ve etiket değerini kaynak gruplara zorla](scripts/enforce-tag-rg.md) | Kaynak grubunda bir etiket ve değer gerektirir. Gerekli etiket adını ve değerini belirtirsiniz.  |
|**SQL**||
| [SQL DB Düzeyinde Denetim Ayarını denetle](scripts/audit-sql-db-audit-setting.md) | Bu ayarlar belirli bir ayarla eşleşmiyorsa SQL veritabanı denetim ayarlarını denetler. Denetim ayarlarının etkin veya devre dışı olması gerektiğini gösteren bir değer belirtirsiniz.  |
| [Saydam veri şifreleme durumunu denetle](scripts/audit-trans-data-enc-status.md) | Etkin değilse SQL veritabanında saydam veri şifrelemeyi denetler.  |
| [DB düzeyi tehdit algılama ayarını denetle](scripts/audit-db-threat-det-setting.md) | SQL veritabanı güvenlik uyarısı ilkeleri belirtilen duruma ayarlanmadıysa bu ilkeleri denetler. Tehdit algılamanın etkin mi devre dışı mı olduğunu gösteren bir değer belirtirsiniz.  |
| [SQL Server Düzeyi Denetim Ayarlarını denetle](scripts/audit-sql-ser-leve-audit-setting.md) | Bu ayarlar belirli bir ayarla eşleşmiyorsa SQL sunucusu denetim ayarlarını denetler. Denetim ayarlarının etkin veya devre dışı olması gerektiğini gösteren bir değer belirtirsiniz. |
| [Sunucu düzeyi tehdit algılama ayarını denetle](scripts/audit-sql-ser-threat-det-setting.md) | SQL veritabanı güvenlik uyarısı ilkeleri belirtilen duruma ayarlanmadıysa bu ilkeleri denetler. Tehdit algılamanın etkin mi devre dışı mı olduğunu gösteren bir değer belirtirsiniz.  |
| [Azure Active Directory yöneticisi olmadığında denetle](scripts/audit-no-aad-admin.md) | SQL sunucusuna atanmış bir Azure Active Directory yöneticisi olmadığında denetler. |
| [İzin verilen SQL DB SKU’ları](scripts/allowed-sql-db-skus.md) | SQL veritabanlarının onaylı bir SKU kullanmasını gerektirir. İzin verilen bir SKU kimlikleri dizisi veya izin verilen bir SKU adları dizisi belirtirsiniz. |
|**Depolama**||
| [Depolama Hesapları ve Sanal Makineler için izin verilen SKU'lar](scripts/allowed-skus-storage.md) | Depolama hesapları ve sanal makinelerin onaylı SKU’lar kullanmasını gerektirir. Onaylı SKU’ları güvence altına almak için yerleşik ilkeleri kullanır. Onaylı bir sanal makine SKU’su dizisi ve onaylı bir depolama hesabı SKU’su dizisi belirtirsiniz. |
| [Depolama hesabı için yalnızca HTTPS trafiğini güvence altına alma](scripts/ensure-https-stor-acct.md) | Depolama hesaplarının HTTPS trafiği kullanmasını gerektirir.  |
| [Depolama hesapları için seyrek erişim katmanını reddet](scripts/deny-cool-access-tiering.md) | Blob depolama hesapları için seyrek erişimli depolama katmanının kullanılmasını engeller.  |
| [Depolama dosya şifrelemesini güvence altına alma](scripts/ensure-store-file-enc.md) | Depolama hesapları için dosya şifrelemenin etkinleştirilmesini gerektirir.  |
|**Yerleşik ilke**||
| [İzin verilen konumlar](scripts/allowed-locs.md) | Tüm kaynakların uygun konumlara dağıtılmasını gerektirir. Onaylanan bir konum dizisi belirtirsiniz.  |
| [İzin verilen kaynak türleri](scripts/allowed-res-types.md) | Yalnızca onaylanan kaynak türlerinin dağıtılmasını güvence altına alır. İzin verilen kaynak türü dizisini belirtirsiniz.  |
| [İzin verilen depolama hesabı SKU'ları](scripts/allowed-stor-acct-skus.md) | Depolama hesaplarının onaylı bir SKU kullanmasını gerektirir. Onaylı bir SKU dizisi belirtirsiniz. |
| [Etiketi ve varsayılan değerini uygula](scripts/apply-tag-def-val.md) | İlgili etiket sağlanmadıysa belirli bir etiket adı ve değerini ekler. Uygulanacak etiket adını ve değerini belirtirsiniz.  |
| [SQL Veritabanı şifrelemesini denetleme](scripts/sql-database-encryption-audit.md) | SQL veritabanında saydam veri şifreleme etkin değilse denetler. |
| [SQL Server denetim ayarlarını denetle](scripts/sql-server-audit.md) | Denetim ayarlarının etkin olup olmamasına göre SQL sunucusunu denetler. |
| [Data Lake Store şifrelemesini zorunlu kıl](scripts/enforce-datalakestore-encryption.md) | Şifrelemesi etkin olmayan Data Lake Store hesaplarını reddeder. |
| [Etiketi ve değerini zorla](scripts/enforce-tag-val.md) | Belirli bir etiket adı ve değeri gerektirir. Zorlanacak etiket adını ve değerini belirtirsiniz.  |
| [İzin verilmeyen kaynak türleri](scripts/not-allowed-res-type.md) | Belirtilen kaynak türlerinin dağıtımını yasaklar. Engellenen kaynak türü dizisini belirtirsiniz.  |
| [SQL Server sürüm 12.0 iste](scripts/req-sql-12.md) | SQL sunucularının sürüm 12.0’ı kullanmasını gerektirir.  |
| [Depolama hesabı şifrelemesi iste](scripts/req-store-acct-enc.md) | Blob şifrelemesi kullanmak için depolama hesabı gerektirir.  |
