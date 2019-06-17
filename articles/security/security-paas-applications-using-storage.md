---
title: Azure depolama kullanarak PaaS uygulamalarının güvenliğini sağlama | Microsoft Docs
description: Azure depolama güvenliği hakkında PaaS web ve mobil uygulamalarınızın güvenliğini sağlamak için en iyi adımları öğrenin.
services: security
documentationcenter: na
author: TomShinder
manager: barbkess
editor: ''
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/28/2018
ms.author: TomShinder
ms.openlocfilehash: 3ad97c7adb5901c1da1d174d12d5d6a91831cc74
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60445426"
---
# <a name="best-practices-for-securing-paas-web-and-mobile-applications-using-azure-storage"></a>PaaS web ve mobil uygulamalarının Azure depolama kullanarak güvenliğini sağlamaya yönelik en iyi yöntemler
Bu makalede, Azure depolama güvenlik en iyi uygulamaları hizmet olarak platform (PaaS) web ve mobil uygulamalarınızın güvenliğini sağlamak için koleksiyonu ele alır. Bu en iyi Azure ile deneyimimizi ve sizin gibi müşteri deneyimleri türetilmiştir.

Azure depolama yollarla dağıtılacağı ve kullanılacağı mümkün kılar şirket kolayca ulaşılabilir. Azure depolama ile yüksek düzeyde ölçeklenebilirlik ve kullanılabilirlik görece az çabayla ulaşabilirsiniz. Yalnızca temel Azure depolama, Windows ve Linux Azure sanal makineleri için ayrıca büyük dağıtılmış uygulamaları destekleyebilir.

Azure depolama, şu dört hizmeti sunar: BLOB Depolama, tablo depolama, kuyruk depolama ve dosya depolama. Daha fazla bilgi için bkz. [Microsoft Azure Storage'a giriş](../storage/storage-introduction.md).

[Azure depolama Güvenlik Kılavuzu](../storage/common/storage-security-guide.md) Azure depolama ve güvenlikle ilgili ayrıntılı bilgi için harika bir kaynaktır. Bu en iyi yöntemler makalesi yüksek düzeyde bazı güvenlik kılavuzu ve daha fazla bilgi için diğer kaynakları yanı sıra güvenlik kılavuzu bağlantıları bulunan kavramlar yöneliktir.

Bu makalede aşağıdaki en iyi uygulamalar ele alır:

- Paylaşılan erişim imzaları (SAS)
- Rol tabanlı erişim denetimi (RBAC)
- Yüksek değerli veriler için istemci tarafı şifreleme
- Depolama Hizmeti Şifrelemesi


## <a name="use-a-shared-access-signature-instead-of-a-storage-account-key"></a>Paylaşılan erişim imzası bir depolama hesabı anahtarı yerine kullanın.
Erişim denetimi büyük/küçük harf önemlidir. Yardımcı olması için bir depolama hesabı oluşturduğunuzda erişimi denetlemek için Azure depolama, Azure iki adet 512 bit depolama hesabı anahtarları (SAKs) oluşturur. Anahtar yedeklilik düzeyini rutin anahtar döndürme sırasında hizmet kesintilerinden kaçınmak mümkün kılar. 

Depolama erişim anahtarlarını yüksek öncelikli gizlidir ve yalnızca depolama erişim denetimi için sorumlu için erişilebilir olmalıdır. Yanlış kişiler bu anahtarları erişin, bunlar depolama tam denetime sahiptir ve değiştirin, silin veya depolama dosyaları ekleyin. Bu, kötü amaçlı yazılım ve diğer kuruluş veya müşterilerinizin tehlikeye atabilir içerik türlerini içerir.

Yine depolamadaki nesnelere erişim sağlamak için bir yol gerekir. Daha ayrıntılı erişim sağlamak için paylaşılan erişim imzası (SAS) yararlanabilirsiniz. SAS depolama alanındaki belirli nesneleri önceden tanımlı bir zaman aralığı için ve belirli izinleri paylaşmak mümkün kılar. Paylaşılan erişim imzası tanımlamanıza olanak sağlar:

- SAS başlangıç ve sona erme saati de dahil olmak üzere, geçerli aralığı.
- SAS'den izinler. Örneğin, bir blob SAS kullanıcı okuma izni ve bu bloba yazma izinleri, ancak silme izinleri yok.
- İsteğe bağlı bir IP adresi veya IP adresleri, Azure depolama SAS kabul eder. Örneğin, kuruluşunuza ait bir IP adresi aralığı belirtebilirsiniz. Bu, SAS için başka bir ölçü güvenlik sağlar.
- Protokol üzerinden Azure depolama SAS kabul eder. Bu isteğe bağlı bir parametre, HTTPS kullanan istemciler için erişimi kısıtlamak için kullanabilirsiniz.

SAS depolama hesap anahtarlarınızı hemen vermeden paylaşmak istediğiniz şekilde içeriği paylaşmanıza olanak tanır. Her zaman uygulamanızda SAS kullanarak depolama hesap anahtarlarınızı tehlikeye atmadan depolama kaynaklarınızı paylaşmanın güvenli bir yoludur.

Paylaşılan erişim imzası hakkında daha fazla bilgi için bkz: [paylaşılan erişim imzaları kullanma](../storage/common/storage-dotnet-shared-access-signature-part-1.md). 

## <a name="use-role-based-access-control"></a>Rol tabanlı erişim denetimi kullanma
Erişimi yönetmek için başka bir yolu [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) (RBAC). RBAC, çalışanlar gereksinim duydukları tam izinleri vererek üzerinde odaklanmak ile bilinmesi gerekenler ve en az ayrıcalık güvenlik ilkelerine bağlı. Çok fazla İznim saldırganların bir hesap üzerinden kullanıma sunabilirsiniz. Çok az izinleri anlamına gelir çalışanlar işlerini verimli bir şekilde alınamıyor. RBAC, Azure için ayrıntılı erişim yönetimi sunarak bu sorunu gidermeye yardımcı olur. Bu, veri erişimi için güvenlik ilkelerini zorlamak istediğinizde kuruluşlar için zorunludur.

Azure'da yerleşik RBAC rolleri, kullanıcılara ayrıcalıkları atamak için kullanabilirsiniz. Örneğin, depolama hesabı Katılımcısı, depolama hesabı ve klasik depolama hesaplarını yönetmek için Klasik depolama hesabı Katılımcısı rolü yönetmek için gereken bulut operatörleri için kullanın. Sanal ağ veya depolama hesabı bağlı sanal makineleri yönetmek için gereken bulut operatörleri için bunları sanal makine Katılımcısı rolüne ekleyebilirsiniz.

Veri erişim denetimi, RBAC gibi özellikleri kullanarak zorlama kuruluşların kullanıcıları için gerekenden daha fazla ayrıcalık vermiş. Bazı kullanıcılar ilk başta yaratmazlar verilere erişime izin vererek bu veri güvenliğinin aşılmasına yol açabilir.

RBAC bakın hakkında daha fazla bilgi için:

- [RBAC ve Azure portalı kullanarak erişimi yönetme](../role-based-access-control/role-assignments-portal.md)
- [Azure kaynakları için yerleşik roller](../role-based-access-control/built-in-roles.md)
- [Azure Depolama güvenlik kılavuzu](../storage/common/storage-security-guide.md) 

## <a name="use-client-side-encryption-for-high-value-data"></a>Yüksek değerli veriler için istemci tarafı şifreleme kullan
İstemci tarafı şifreleme, program aracılığıyla Azure Storage'a yüklemeden önce Aktarımdaki verileri şifrelemek ve verilerin alınması, program aracılığıyla şifresini çözmek sağlar. Bu Aktarımdaki verileri şifreleme sağlar ancak aynı zamanda bekleyen verilerin şifrelenmesi sağlar. İstemci tarafı şifreleme, veri şifreleme için en güvenli yöntemdir, ancak uygulamanızı programlı değişiklik ve anahtar yönetimi işlemlerini yerleştirdiniz gerektirir.

İstemci tarafı şifreleme şifreleme anahtarlarınızı tek denetime sahip olmanızı sağlar. Oluşturabilir ve kendi şifreleme anahtarlarınızı yönetme. Burada Azure depolama istemci kitaplığı (anahtar şifreleme anahtarı (KEK) kullanılarak şifrelenir) sonra kaydırılan bir içerik şifreleme anahtarı (CEK) oluşturur bir zarf teknik kullanır. KEK anahtar bir tanımlayıcıyla tanımlanır ve asimetrik anahtar çifti veya bir simetrik anahtar ve yerel olarak yönetilebilir veya depolanan [Azure anahtar kasası](../key-vault/key-vault-whatis.md).

İstemci tarafı şifreleme, Java ve .NET depolama istemci kitaplıkları ile oluşturulmuştur. Bkz: [istemci tarafı şifreleme ve Microsoft Azure depolama için Azure anahtar kasası](../storage/storage-client-side-encryption.md) istemci uygulamalar içinde verilerin şifrelenmesi, oluşturma ve kendi şifreleme anahtarlarınızı yönetme hakkında bilgi.

## <a name="enable-storage-service-encryption-for-data-at-rest"></a>Bekleyen veriler için depolama hizmeti Şifrelemesi'ni etkinleştir
Zaman [depolama hizmeti şifrelemesi](../storage/storage-service-encryption.md) etkin dosya depolama için veriler otomatik olarak AES-256 şifreleme kullanılarak şifrelenir. Microsoft, tüm şifreleme, şifre çözme ve anahtar yönetimini işler. Bu özellik, LRS ve GRS yedeklilik türleri için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, PaaS web ve mobil uygulamalarınızın güvenliğini sağlamak için Azure depolama güvenlik en iyi yöntemler koleksiyonu kullanıma sunuldu. PaaS dağıtımlarının güvenliğini sağlama hakkında daha fazla bilgi için bkz:

- [PaaS dağıtımlarının güvenliğini sağlama](security-paas-deployments.md)
- [PaaS web ve Azure App Services'ı kullanarak mobil uygulamalarının güvenliğini sağlama](security-paas-applications-using-app-services.md)
- [Azure PaaS veritabanlarının güvenliğini sağlama](security-paas-applications-using-sql.md)
