---
title: "Rest verileri için Azure Storage hizmeti şifreleme | Microsoft Docs"
description: "Veri alınırken şifresini çözmek ve Azure depolama hizmeti şifrelemesi özelliği hizmet tarafında Azure Blob Depolama veri depolarken şifrelemek için kullanın."
services: storage
author: lakasa
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 03/06/2018
ms.author: lakasa
ms.openlocfilehash: 29f6a38f7a7f0f107dab8c99e750a8dec803f6f0
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Bekleyen Veri için Azure Storage Hizmeti Şifreleme

Azure Storage hizmeti şifreleme (SSE) bekleyen veri için korumak ve Kuruluş güvenliği ve uyumluluk taahhüt karşılamak için verilerinizi korumaya yardımcı olur. Bu özellik ile Azure depolama, verilerinizi öncesinde otomatik olarak şifreler Azure depolama alanına kalıcı yapma ve alma önce şifresini çözer. İşleme şifreleme, rest, şifre çözme ve anahtar yönetimi SSE tarafından sağlanan şifreleme kullanıcılar için saydamdır. Azure depolama alanına yazılan tüm veriler, 256 bit kullanılarak şifrelenir [AES şifreleme](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), güçlü blok birini şifrelemeleri kullanılabilir.

SSE tüm yeni ve var olan depolama hesapları için etkin ve devre dışı bırakılamaz. Varsayılan olarak, verilerinizin güvenliği için kod veya SSE yararlanmak için uygulamaları değiştirmek gerekmez.

 SSE otomatik olarak tüm performans katmanları (standart ve Premium), tüm dağıtım modellerini (Azure Resource Manager ve klasik) ve tüm Azure Storage Hizmetleri (Blob, kuyruk, tablo ve dosya) verileri şifreler. 

Microsoft tarafından yönetilen şifreleme anahtarları ile SSE kullanabilir veya kendi şifreleme anahtarlarınızı kullanabilirsiniz. Kendi anahtarları kullanma hakkında daha fazla bilgi için bkz: [depolama hizmeti müşteri kullanarak şifreleme anahtarları Azure anahtar Kasası'yönetilen](storage-service-encryption-customer-managed-keys.md).

## <a name="view-encryption-settings-in-the-azure-portal"></a>Azure portalında görünümü şifreleme ayarları

Depolama hizmeti şifrelemesi ayarlarını görüntülemek için oturum açın [Azure portal](https://portal.azure.com) ve bir depolama hesabı seçin. Üzerinde **ayarları** dikey penceresinde, şifreleme ayarı'ı tıklatın.

![Portal gösteren ekran görüntüsü şifreleme seçeneği](./media/storage-service-encryption/image1.png)

## <a name="faq-for-storage-service-encryption"></a>Depolama hizmeti şifrelemesi hakkında SSS

**S: Klasik depolama hesabınız sahibim. SSE üzerinde etkinleştirebilirim?**

Y: SSE tüm depolama hesapları (Klasik ve Resource Manager depolama hesapları) için varsayılan olarak etkindir.

**S: nasıl ı veri Klasik depolama Hesabımı şifreleyebilir mi?**

A: varsayılan olarak etkin şifreleme ile yeni veri depolama hizmeti tarafından otomatik olarak şifrelenir. 

**S: mevcut bir Resource Manager depolama hesabını sahibim. SSE üzerinde etkinleştirebilirim?**

Y: SSE varolan tüm Resource Manager depolama hesaplarıyla üzerinde varsayılan olarak etkindir. Bu BLOB'lar, tablolar, dosya ve Kuyruk için desteklenen depolama. 

**S: var olan bir Resource Manager depolama hesabı geçerli verileri şifrelemek ister misiniz?**

Y: ile SSE varsayılan olarak tüm depolama hesaplarının - Klasik ve Resource Manager depolama hesapları etkin. Ancak, zaten mevcut veri şifrelenmez. Var olan verileri şifrelemek için başka bir ad veya başka bir kapsayıcı kopyalayın ve ardından şifrelenmemiş sürümlerini kaldırın. 

**S: yeni depolama hesapları ile Azure PowerShell ve Azure CLI kullanarak etkin SSE oluşturabilir miyim?**

Y: SSE herhangi bir depolama hesabı (Klasik ve Resource Manager hesapları) oluşturma sırasında varsayılan olarak etkindir. Azure PowerShell ve Azure CLI kullanarak hesap özelliklerini doğrulayabilirsiniz.

**S: SSE etkinse, daha fazlasını nasıl Azure depolama maliyeti?**

Y: yoktur ek ücret ödemeden.

**S: şifreleme anahtarları yöneten?**

Y: anahtarları Microsoft tarafından yönetilir.

**S: kendi şifreleme anahtarları kullanmak?**

Y: müşterilerin kendi şifreleme anahtarlarını getirmek özelliklerinin sağlanması üzerinde çalışıyoruz.

**S: şifreleme anahtarlarının erişimi iptal?**

Şu anda A:; anahtarları tam olarak Microsoft tarafından yönetilir.

**S: yeni bir depolama hesabı oluşturduğunuzda, varsayılan olarak etkin SSE mi?**

A: Evet SSE Microsoft Managed tuşlarını kullanarak, tüm depolama hesapları - Azure Resource Manager ve klasik depolama hesapları için varsayılan olarak etkindir. Tüm hizmetleri de - Blob, tablo, kuyruk ve dosya depolama alanı için etkinleştirilir.

**S: nasıl bu Azure Disk Şifrelemesi ' farklı mı?**

Y: azure Disk şifrelemesi, işletim sistemi ve veri diskleri Iaas Vm'leri de şifrelemek için kullanılır. Daha fazla ayrıntı için lütfen ziyaret bizim [depolama Güvenlik Kılavuzu](../storage-security-guide.md).

**S: alırsam ne Azure Disk şifrelemesi my veri disklerde etkinleştirebilirim?**

Y: Bu sorunsuz bir şekilde çalışır. Her iki yöntemle veri şifrelenir.

**S: depolama Hesabımı coğrafi nedenle çoğaltılması için ayarlanır. SSE ile my yedek kopya de şifrelenir mi?**

A: Evet depolama hesabını tüm kopyalarını şifrelenir ve tüm artıklık seçenekleri – yerel olarak yedekli depolama (LRS), bölge olarak yedekli depolama (ZRS), coğrafi olarak yedekli depolama (GRS) ve okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) – desteklenir.

**S: şifreleme depolama Hesabımı devre dışı bırakılamıyor.**

Y: şifreleme varsayılan olarak etkindir ve depolama hesabınız için şifrelemeyi devre dışı bırakmak için hiçbir sağlama yok. 

**S: SSE yalnızca belirli bölgelerine izin?**

Y: SSE tüm hizmetler için tüm bölgelerde kullanılabilir. 

**S: herhangi bir sorununuz veya geri bildirim sağlamak istiyorsanız nasıl ı biriyle iletişim?**

Y: kişi [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) depolama hizmeti şifrelemesi ile ilgili sorunlar için.

## <a name="next-steps"></a>Sonraki Adımlar
Azure depolama birlikte geliştiricilerin güvenli uygulamalar oluşturmasını sağlama güvenlik özellikleri kapsamlı bir kümesini sağlar. Daha fazla ayrıntı için ziyaret [depolama Güvenlik Kılavuzu](../storage-security-guide.md).
