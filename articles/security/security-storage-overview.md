---
title: Azure depolama ile kullanılan güvenlik özellikleri | Microsoft Docs
description: Bu makalede, Azure depolama ile kullanılabilir Azure güvenlik özelliklerini çekirdek genel bir bakış sağlar.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/01/2019
ms.author: terrylan
ms.openlocfilehash: ec0e8ae1bf657cda59f3d133db23106436e184e3
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62120901"
---
# <a name="azure-storage-security-overview"></a>Azure depolama güvenliğine genel bakış

Azure Depolama, müşterilerin ihtiyaçlarını karşılamak üzere sağlamlık, kullanılabilirlik ve ölçeklenebilirliğe dayanan modern uygulamalara yönelik bulut depolama çözümüdür. Azure depolama, kapsamlı güvenlik özellikleri sağlar. Şunları yapabilirsiniz:

* Depolama hesabı rol tabanlı erişim denetimi (RBAC) ve Azure Active Directory kullanarak güvenli hale getirin.
* Bir uygulama ve Azure arasında aktarımda istemci tarafı şifreleme, HTTPS ve SMB 3.0 kullanarak güvenli hale getirin.
* Azure depolama için depolama hizmeti şifrelemesi kullanarak yazıldığı zaman otomatik olarak şifrelenmiş verileri ayarlayın.
* Azure Disk şifrelemesi kullanılarak şifrelenmiş sanal makineleri (VM'ler) tarafından kullanılan Küme işletim sistemi ve veri diskleri.
* Paylaşılan erişim imzaları (SASs) kullanarak Azure depolama veri nesnelerini temsilci erişimi verin.
* Analiz, depolama eriştiklerinde kişinin kullandığı kimlik doğrulama yöntemini izlemek için kullanın.

Azure depolama güvenlik daha ayrıntılı bilgi için bkz: [Azure depolama Güvenlik Kılavuzu](../storage/common/storage-security-guide.md). Bu kılavuz, Azure depolama güvenlik özellikleri hakkında ayrıntılı bir inceleme sunar. Bu özellikler, depolama hesabı anahtarları, veri şifreleme Aktarımdaki ve rest ve depolama analizi içerir.


Bu makalede, Azure depolama için kullanabileceğiniz Azure güvenlik özelliklerine genel bakış sağlar. Daha fazla bilgi için makalelerin bağlantıları her özelliğin ayrıntılarını verir.

## <a name="role-based-access-control"></a>Rol Tabanlı Access Control

Rol tabanlı erişim denetimi kullanarak depolama hesabınızın güvenliğini sağlayabilirsiniz. Erişimi kısıtlama temel alarak [bilmeniz gereken](https://en.wikipedia.org/wiki/Need_to_know) ve [en az ayrıcalık](https://en.wikipedia.org/wiki/Principle_of_least_privilege) güvenlik ilkeleri, veri erişimi için güvenlik ilkelerini zorlamak istediğinizde kuruluşlar için zorunlu. Bu erişim hakları, gruplara ve uygulamalara belirli bir kapsama uygun RBAC rolü atanarak verilir. Kullanabileceğiniz [yerleşik RBAC rolleri](../role-based-access-control/built-in-roles.md), ayrıcalıkları kullanıcılara atamak için depolama hesabı katılımcısı gibi.

Daha fazla bilgi edinin:

* [Azure Active Directory rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md)

## <a name="delegated-access-to-storage-objects"></a>Depolama nesneleri temsilci erişimi

Paylaşılan erişim imzası, depolama hesabınızdaki kaynaklara temsilci erişimi sağlar. SAS, belirli bir süre için ve belirli bir izin kümesi ile bir istemci, depolama hesabınızdaki nesnelere sınırlı verebilirsiniz anlamına gelir. Hesap erişim anahtarlarınızı paylaşmak zorunda kalmadan bu sınırlı izinler verebilirsiniz.

SAS depolama kaynak kimliği doğrulanmış erişim için gerekli tüm bilgileri sorgu parametrelerini kapsayan bir URI'dir. SAS ile depolama kaynaklarına erişmek için istemcinin yalnızca uygun oluşturucuyu veya yöntem SAS sağlaması gerekir.

Daha fazla bilgi edinin:

* [SAS modelini anlama](../storage/common/storage-dotnet-shared-access-signature-part-1.md)
* [Oluşturma ve bir SAS ile Blob Depolama kullanma](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md)

## <a name="encryption-in-transit"></a>Aktarım sırasında şifreleme

Aktarım sırasında şifreleme ağlar üzerinden iletilirken, veri koruma, bir mekanizmadır. Azure depolama ile kullanarak veri güvenliğini sağlayabilirsiniz:

* [Aktarım düzeyinde şifreleme](../storage/common/storage-security-guide.md#encryption-in-transit), HTTPS, Azure depolama içine veya dışına veri aktarımı gibi.
* [Şifreleme wire](../storage/common/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), SMB 3.0 şifreleme gibi için Azure dosya paylaşımları.
* [İstemci tarafı şifreleme](../storage/common/storage-security-guide.md#using-client-side-encryption-to-secure-data-that-you-send-to-storage), verileri şifrelemenizi depolama alanına aktarılır ve depolama alanınız aktarıldıktan sonra verilerin şifresini çözmek için.

İstemci tarafı şifreleme hakkında daha fazla bilgi edinin:

* [Microsoft Azure depolama istemci tarafı şifreleme](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
* [Bulut güvenliği serisi denetler: Aktarımdaki verileri şifreleme](https://cloudblogs.microsoft.com/microsoftsecure/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme

Birçok kuruluşta [bekleyen verileri şifreleme](https://cloudblogs.microsoft.com/microsoftsecure/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) veri gizlilik, uyumluluk ve veri egemenliği doğru zorunlu bir adımdır. Üç Azure özellikleri, bekleyen verileri şifrelenmesi sağlar:

* [Depolama hizmeti şifrelemesi](../storage/common/storage-security-guide.md#encryption-at-rest) her zaman etkindir ve depolama hizmeti verileri Azure depolama alanına yazarken otomatik olarak şifreler.
* [İstemci tarafı şifreleme](../storage/common/storage-security-guide.md#client-side-encryption) bekleyen şifreleme özelliği de sağlar.
* [Azure Disk şifrelemesi](../storage/common/storage-security-guide.md#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) kullanan bir Iaas sanal makinesine veri diski ve işletim sistemi diskleri şifrelemenizi sağlar.

Depolama hizmeti şifrelemesi hakkında daha fazla bilgi edinin:

* [Azure depolama hizmeti şifrelemesi](https://azure.microsoft.com/services/storage/) kullanılabilir [Azure Blob Depolama](https://azure.microsoft.com/services/storage/blobs/). Diğer Azure depolama türleri hakkında daha fazla bilgi için bkz: [Azure dosyaları](https://azure.microsoft.com/services/storage/files/), [tablo depolama](https://azure.microsoft.com/services/storage/tables/), ve [kuyruk depolama](https://azure.microsoft.com/services/storage/queues/).
* [Bekleyen veri için Azure depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption.md)

## <a name="azure-disk-encryption"></a>Azure Disk Şifrelemesi

Sanal makineler için Azure Disk şifrelemesi, Kurumsal güvenlik ve uyumluluk gereksinimlerine yardımcı olur. Sanal makine disklerinizi (önyükleme ve veri diskleri dahil) anahtarları ve, denetim ilkeleri kullanarak şifreler [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/).

Sanal makineler için disk şifrelemesi, Linux ve Windows işletim sistemlerinde çalışır. Ayrıca korumanıza, yönetin ve disk Şifreleme anahtarlarınızın kullanımını denetlemenize yardımcı olmak için Key Vault'u kullanır. Sanal makine disklerinizi tüm verileri bekleme sırasında endüstri standardı şifreleme teknolojisini Azure depolama hesaplarınızda kullanılarak şifrelenir. Windows için Disk şifrelemesi çözümü dayanır [Microsoft BitLocker Sürücü Şifrelemesi](https://technet.microsoft.com/library/cc732774.aspx), ve Linux çözüm dayanır [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt).

Daha fazla bilgi edinin

* [Windows ve Linux Iaas sanal makineler için Azure Disk şifrelemesi](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## <a name="firewalls-and-virtual-networks"></a>Güvenlik duvarları ve sanal ağlar

Azure depolama, depolama hesaplarınız için güvenlik duvarı kurallarını etkinleştirmenize izin verir. Etkinleştirildikten sonra diğer Azure hizmetlerinden gelen istekleri dahil olmak üzere veri, gelen istekleri engeller. Özel durumlara izin verecek şekilde yapılandırabilirsiniz. Güvenlik duvarı kuralları, var olan depolama hesaplarını veya oluşturma zamanı sırasında etkinleştirilebilir.

Bu işlevsellik, belirli bir ağa izin kümesi, depolama hesaplarınıza güvenli hale getirmek için kullanmanız gerekir.

Azure depolama hakkında daha fazla bilgi için güvenlik duvarları ve sanal ağlar makalesini gözden geçirin [Azure depolama güvenlik duvarlarını yapılandırın ve sanal ağlar](../storage/common/storage-network-security.md)

## <a name="azure-data-box"></a>Azure Data Box

Data Box, Data Box Disk ve Data Box Heavy cihazları, ağ seçeneğinin olmadığı durumlarda Azure’a büyük boyutlu veri aktarmanıza yardımcı olur. Çevrimdışı veri aktarımı cihazların, kuruluşunuz ve Azure veri merkezi arasında gönderilir. Bunlar aktarım sırasında verilerinizin korunmasına yardımcı olmak için AES şifrelemesi kullanır ve karşıya yükleme sonrası temizlik işlemine tabi tutularak verileriniz cihazdan silinir.

Data Box Edge ve Data Box Gateway, verilerin siteniz ile Azure arasında yönetilmesi için ağ depolama geçitleri olarak çalışan çevrimiçi veri aktarımı ürünleridir. Şirket içi bir ağ cihazı olan Data Box Edge, Azure’ın içine ve dışına veri aktarımı gerçekleştirmesinin yanı sıra verileri işlemek için yapay zeka (AI) özellikli uç işlemini kullanır. Data Box Gateway, depolama ağ geçidi özelliklerine sahip sanal bir gereçtir.

Daha fazla bilgi edinin:

* [Azure Data Box](https://azure.microsoft.com/services/storage/databox/)
* [Azure Data Box Edge](../databox-online/data-box-edge-overview.md)
* [Azure veri kutusu ağ geçidi](..//databox-online/data-box-gateway-overview.md)

## <a name="advanced-threat-protection"></a>Gelişmiş Tehdit Koruması

Azure depolama, ek bir erişim veya depolama hesabınıza yararlanma olağan dışı ve zararlı olabilecek girişimleri algılar güvenlik zekası katmanı için Gelişmiş tehdit koruması sağlar. Şüpheli okuma için Gelişmiş tehdit koruması izleyiciler Azure depolama, tanılama günlüklerini yazma veya silme istekleri Blob Depolama.

Gelişmiş tehdit koruması uyarıları görüntülenebilir [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/). Azure Güvenlik Merkezi şüpheli etkinliği ayrıntıları algılandı ve olası tehdit düzeltilmesi için Eylemler önerir sağlar.

Daha fazla bilgi edinin:

* [Azure depolama Gelişmiş tehdit koruması genel bakış](../storage/common/storage-advanced-threat-protection.md)

## <a name="azure-key-vault"></a>Azure Key Vault

Azure Disk şifrelemesi kullanır [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) denetlemenize ve disk şifreleme anahtarlarını ve gizli anahtar kasası aboneliğinizi yönetmenize yardımcı olmak için. Ayrıca, sanal makine disklerini tüm veriler Azure depolama alanındaki bekleyen şifrelenmesini sağlar. Key Vault, anahtarları ve ilke kullanımı denetlemek için kullanmanız gerekir.

Daha fazla bilgi edinin

* [Azure anahtar kasası nedir?](../key-vault/key-vault-overview.md)
