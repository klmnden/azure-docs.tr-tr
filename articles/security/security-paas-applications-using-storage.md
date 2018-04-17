---
title: Azure Storage kullanarak PaaS uygulamalarının güvenliğini sağlama | Microsoft Docs
description: " Azure Storage güvenliği hakkında bilgi edinme PaaS web ve mobil uygulamaların güvenliğini sağlamaya yönelik en iyi uygulamalar. "
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: ''
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomShinder
ms.openlocfilehash: 9d4251e61b60d8da6ce5072ba66aeaedb60cb33a
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="securing-paas-web-and-mobile-applications-using-azure-storage"></a>PaaS web ve mobil uygulamaları Azure Storage kullanarak güvenli hale getirme
Bu makalede, PaaS web ve mobil uygulamaların güvenliğini sağlamak için Azure Storage en iyi güvenlik yöntemleri topluluğu tartışın. Bu en iyi uygulamaları Azure ile deneyimi bizim ve kendiniz gibi müşterilerin deneyimleri türetilir.

[Azure Storage Güvenlik Kılavuzu'nu](../storage/common/storage-security-guide.md) Azure Storage ve güvenlik hakkında ayrıntılı bilgi için mükemmel bir kaynaktır.  Bu makalede yüksek düzeyde güvenlik kılavuzu ve daha fazla bilgi için diğer kaynakları yanı sıra Güvenlik Kılavuzu'nu bağlantılar bulunan kavramların bazıları giderir.

## <a name="azure-storage"></a>Azure Storage
Azure dağıtma ve depolama şekillerde kullanma olanaklı kılar içi kolayca ulaşılabilir. Azure storage ile ölçeklenebilirlik ve kullanılabilirlik nispeten az çaba ile yüksek düzeyde ulaşabilirsiniz. Yalnızca Azure depolama foundation, Windows ve Linux Azure sanal makineler için de büyük dağıtılmış uygulamaları destekleyebilir.

Azure Storage şu dört hizmeti sunar: Blob Storage, Table Storage, Kuyruk depolama ve File Storage. Daha fazla bilgi için bkz: [Microsoft Azure Storage'a giriş](../storage/storage-introduction.md).

## <a name="best-practices"></a>En iyi uygulamalar
Bu makalede aşağıdaki en iyi yöntemleri ele alır:

- Erişim Koruması:
   - Paylaşılan Erişim İmzaları (SAS)
   - Yönetilen disk
   - Rol Tabanlı Erişim Denetimi (RBAC)

- Depolama şifrelemesi:
   - Yüksek değerli veri için istemci tarafı şifrelemesi
   - Sanal makineler (VM'ler) için Azure Disk şifrelemesi
   - Depolama Hizmeti Şifrelemesi

## <a name="access-protection"></a>Erişimi Koruması
### <a name="use-shared-access-signature-instead-of-a-storage-account-key"></a>Bir depolama hesabı anahtarı yerine paylaşılan erişim imzası kullanın

Genellikle Windows Server veya Linux sanal makineleri çalıştıran bir Iaas çözümündeki dosyaları açığa çıkması ve erişim denetimi düzenekleri kullanarak değiştirme tehditlerinden korunur. Windows üzerinde kullanacağınız [erişim denetimi listeleri (ACL)](../virtual-network/virtual-networks-acl.md) ve büyük olasılıkla kullandığınız Linux üzerinde [chmod](https://en.wikipedia.org/wiki/Chmod). Esas olarak, tam olarak kendi veri merkezinizde bir sunucudaki dosyalarının bugün koruma varsa yapmayı tercih budur.

PaaS farklıdır. Microsoft Azure'da dosyalarını depolamak için en yaygın yollarından biri kullanmaktır [Azure Blob Depolama](../storage/storage-dotnet-how-to-use-blobs.md). Bir Blob Depolama ve diğer dosya depolama arasında dosya g/ç ve dosya g/ç ile gelen koruma yöntemleri farktır.

Erişim denetimi önemlidir. Yardımcı olması için Azure storage, sistem erişimi denetleme iki 512 bit depolama hesabı anahtarları (SAKs) ne zaman oluşturur, [depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md). Anahtar artıklık düzeyini rutin anahtar döndürme sırasında hizmet kesme önlemek mümkün hale getirir.

Depolama erişim tuşlarını yüksek öncelikli gizli olan ve yalnızca depolama erişim denetimi için sorumlu için erişilebilir olmalıdır. Yanlış kişilerin bu anahtara erişim kazanmak depolama tam denetime sahiptir ve değiştirmek, silmek veya dosyaları depolama alanına ekleyin. Bu, kötü amaçlı yazılım ve diğer kuruluşunuzun veya müşterilerinize tehlikeye atabilir içerik türlerini içerir.

Hala depolama nesnelerine erişim sağlamak için bir yönteme ihtiyacınız vardır. Daha ayrıntılı erişim sağlamak için özelliklerden yararlanabilirsiniz [paylaşılan erişim imzası](../storage/common/storage-dotnet-shared-access-signature-part-1.md) (SAS). SAS depolama belirli nesneleri paylaşımı ön tanımlı bir zaman aralığı için ve özel izinleri mümkün hale getirir. Paylaşılan erişim imzası tanımlamanıza olanak sağlar:

- SAS başlangıç saati ve sona erme saati dahil geçerli aralık.
- SAS tarafından verilen izinler. Örneğin, bir blob SAS kullanıcı okuma izni ve o blob yazma izinleri, ancak izinleri silmemeniz.
- İsteğe bağlı bir IP adresi veya IP adresleri, Azure Storage SAS kabul eder. Örneğin, kuruluşunuza ait bir IP adresi aralığı belirtebilirsiniz. Bu, SA'ları için başka bir ölçüye güvenlik sağlar.
- Protokol üzerinden Azure Storage SAS kabul eder. Bu isteğe bağlı parametre HTTPS kullanan istemciler için erişimi kısıtlamak için kullanabilirsiniz.

SAS içerik uzakta depolama hesabı tuşlarınızı vermeden paylaşmak istediğiniz şekilde paylaşmanıza olanak tanır. Her zaman, uygulamanızda SAS kullanarak, depolama hesabı anahtarlarını ödün vermeden depolama kaynaklarınıza paylaşmak için güvenli bir yoludur.

Daha fazla bilgi için bkz: [paylaşılan erişim imzaları kullanma](../storage/common/storage-dotnet-shared-access-signature-part-1.md) (SAS). Olası riskleri ve bu riskleri azaltmak için öneriler hakkında daha fazla bilgi için bkz: [en iyi yöntemler SAS kullanırken](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

### <a name="use-managed-disks-for-vms"></a>VM'ler için yönetilen diskleri kullanın

Seçtiğinizde [Azure yönetilen diskleri](../storage/storage-managed-disks-overview.md), Azure VM diskleriniz için kullandığınız depolama hesapları yönetir. Yapmanız gereken tek şey disk (Premium ya da standart) ve disk boyutunu seçin; Azure storage rest yapın. Aksi takdirde, birden çok depolama hesaplarına gerekli sahip ölçeklenebilirlik sınırları hakkında endişelenmeniz gerekmez.

Daha fazla bilgi için bkz: [yönetilen ve yönetilmeyen premium diskler hakkında sık sorulan sorular](../storage/storage-faq-for-disks.md).

### <a name="use-role-based-access-control"></a>Rol tabanlı erişim denetimi kullanma

Daha önce hesabı depolama hesabı anahtarınızı sokmadan diğer istemcilere depolama hesabındaki nesnelere sınırlı erişim vermek için paylaşılan erişim imzası (SAS) kullanma açıklanmaktadır. Bazen, depolama hesabınız karşı belirli bir işlemle ilişkili riskleri SAS avantajlarından daha ağır basar. Bazen diğer yollarla erişimi yönetmek üzere basittir.

Erişimi yönetmek için başka bir yolu kullanmaktır [Azure rol tabanlı erişim denetimi](../role-based-access-control/overview.md) (RBAC). RBAC, çalışanlar gereksinim duydukları tam izinleri veriyorsanız, odaklanmasına ile bilme gereğini ve en az ayrıcalık güvenlik ilkelerine göre. Çok fazla izinler saldırganlar bir hesaba getirebilir. Çok az izinleri anlamına gelir çalışanlar verimli bir şekilde işlerini alınamıyor. RBAC, Azure için ayrıntılı erişim yönetimi sunarak bu sorunu gidermeye yardımcı olur. Bu, veri erişimi için güvenlik ilkelerini zorlamak istiyorsanız kuruluşlar için zorunludur.

Azure ayrıcalıkları kullanıcılara atamak için yerleşik RBAC rollerinde yararlanabilirsiniz. Depolama hesabı katkıda bulunan depolama hesapları ve klasik depolama hesaplarını yönetmek için Klasik depolama hesabı katkıda bulunan rolü yönetmek için gereken bulut işleçlerini kullanmayı düşünün. Bulut operatörleri, gereksinim VM'ler ancak bağlı sanal ağ veya depolama hesabı değil yönetmek için sanal makine Katılımcısı rolüne eklemeyi düşünün.

Veri erişim denetimi RBAC gibi özellikler yararlanarak zorlamaz kuruluşlar kendi kullanıcıları için gerekenden daha fazla ayrıcalık vermiş. Bazı kullanıcılar ilk başta olmamalıdır verilere erişime izin vererek bu verilerin güvenliğinin aşılmasına neden olabilir.

RBAC bakın hakkında daha fazla bilgi için:

- [Azure rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md)
- [Azure rol tabanlı erişim denetimi için yerleşik roller](../role-based-access-control/built-in-roles.md)
- [Azure depolama Güvenlik Kılavuzu](../storage/common/storage-security-guide.md) depolama hesabınızı RBAC ile güvenli konusunda ayrıntılı bilgi için

## <a name="storage-encryption"></a>Depolama şifrelemesi
### <a name="use-client-side-encryption-for-high-value-data"></a>İstemci tarafı şifreleme yüksek değerli verileri için kullanın

İstemci tarafı şifreleme program aracılığıyla Azure Storage'a yüklemeden önce Aktarımdaki verileri şifrelemek ve verilerin depolama alanından alınırken program aracılığıyla şifresini çözmek etkinleştirir.  Bu Aktarımdaki verileri şifreleme sağlar ancak aynı zamanda kalan verileri şifrelemesini sağlar.  İstemci tarafı şifreleme, veri şifreleme için en güvenli yöntemdir, ancak uygulamanıza programlı değişiklikleri yapın ve anahtar yönetimi işlemleri yerleştirdiniz gerektirir.

İstemci tarafı şifreleme şifreleme anahtarlarınızı üzerinde tek denetim sahibi sağlar.  Oluştur ve kendi şifreleme anahtarlarınızı yönetin.  İstemci tarafı şifreleme yere Azure storage istemci kitaplığı (anahtar şifreleme anahtarı (KEK) kullanılarak şifrelenir) sonra kaydırılan bir içerik şifreleme anahtarı (CEK) oluşturur bir zarf teknik kullanır. KEK anahtar bir tanımlayıcıyla tanımlanır ve asimetrik anahtar çifti ya da bir simetrik anahtar olması ve yerel olarak yönetilebilir veya depolanan [Azure anahtar kasası](../key-vault/key-vault-whatis.md).

İstemci tarafı şifreleme, Java ve .NET depolama istemcisi kitaplıklarını olarak oluşturulmuştur.  Bkz: [istemci tarafı şifreleme ve Microsoft Azure depolama için Azure anahtar kasası](../storage/storage-client-side-encryption.md) istemci uygulamalar içinde verilerin şifrelenmesi ve oluşturma ve kendi şifreleme anahtarlarınızı yönetme hakkında bilgi için.

### <a name="azure-disk-encryption-for-vms"></a>VM'ler için Azure Disk şifrelemesi
Azure Disk şifrelemesi, Windows ve Linux Iaas sanal makine disklerinizi şifrelemek yardımcı olan bir özelliktir. Azure Disk şifrelemesi endüstri standart BitLocker özelliği, Windows ve Linux işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için DM-Crypt özelliği yararlanır. Çözüm denetlemek ve disk şifreleme anahtarları ve gizli anahtar kasası aboneliğinizde yönetmenize yardımcı olmak için Azure anahtar kasası ile tümleşiktir. Çözüm, aynı zamanda sanal makine disklerdeki tüm veriler Azure depolama alanınızı bekleyen şifrelenmesini sağlar.

Bkz: [Windows ve Linux Iaas VM'ler için Azure Disk şifrelemesi](azure-security-disk-encryption.md).

### <a name="storage-service-encryption"></a>Depolama Hizmeti Şifrelemesi
Zaman [depolama hizmeti şifrelemesi](../storage/storage-service-encryption.md) dosya depolama etkin için verileri otomatik olarak AES 256 şifreleme kullanılarak şifrelenir. Microsoft, tüm şifreleme, şifre çözme ve anahtar yönetimi işler. Bu özellik LRS ve GRS artıklık türleri için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede bir koleksiyonuna PaaS web ve mobil uygulamaların güvenliğini sağlamak için Azure Storage en iyi güvenlik uygulamalarını kullanıma sunuldu. PaaS dağıtımları güvenliğini sağlama hakkında daha fazla bilgi için bkz:

- [PaaS dağıtımlarının güvenliğini sağlama](security-paas-deployments.md)
- [PaaS web ve mobil uygulamaları Azure App Services kullanarak güvenli hale getirme](security-paas-applications-using-app-services.md)
- [Azure PaaS veritabanlarında güvenliğini sağlama](security-paas-applications-using-sql.md)
