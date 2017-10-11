---
title: "Azure Güvenlik Merkezi'nde disk şifrelemesi uygulamak | Microsoft Docs"
description: "Bu belgede Azure Güvenlik Merkezi öneriyi uygulamayı gösterilmiştir ** disk şifreleme ** uygulayın."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 6cc7824a-8d6b-4a5f-ab40-e3bbaebc4a91
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 67cff664f3723b2194ecd1519729cca17069d07f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="apply-disk-encryption-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde disk şifrelemesi Uygula
Azure Güvenlik Merkezi, Azure Disk şifrelemesi kullanılarak şifrelenmiş değil Windows veya Linux VM diskiniz varsa, disk şifrelemesi uygulamanızı önerir. Disk şifrelemesi, Windows ve Linux Iaas VM diskleri şifrelemek olanak tanır.  Şifreleme hem işletim sistemi hem de VM’nizin üzerindeki veri birimleri için önerilir.

Disk şifrelemesi kullanılır endüstri standardı [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) Windows özelliğidir ve [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux özelliğidir. Bu özellikleri korumak ve verilerinizi korumak ve Kuruluş güvenliği ve uyumluluğu taahhüt karşılamak için işletim sistemi ve veri şifreleme sağlar. Disk şifrelemesi ile tümleştirildiğinde [Azure anahtar kasası](https://azure.microsoft.com/documentation/services/key-vault/) denetlemek ve disk şifreleme anahtarları ve gizli anahtar kasası aboneliğinizde yönetmenize yardımcı olmak için while bekleyen VM disklerdeki tüm veriler şifrelenir sağlayarak, [Azure Storage](https://azure.microsoft.com/documentation/services/storage/).

> [!NOTE]
> Azure Disk şifrelemesi aşağıdaki Windows server işletim sistemlerinde - Windows Server 2008 R2, Windows Server 2012 ve Windows Server 2012 R2 desteklenir. Disk şifrelemesi aşağıdaki Linux sunucusu işletim sistemlerinde - Ubuntu ve CentOS, SUSE ve SUSE Linux Enterprise Server (SLES) desteklenir.
>
>

## <a name="implement-the-recommendation"></a>Öneriyi uygulamayı
1. İçinde **önerileri** dikey penceresinde, select **disk şifrelemesi uygulamak**.
2. İçinde **disk şifrelemesi uygulamak** dikey penceresinde, Disk şifrelemesi önerilir VM'ler listesini görürsünüz.
3. Şifreleme için bu Vm'lere uygulamak için yönergeleri izleyin.

![][1]

Güvenlik Merkezi tarafından şifreleme gerektiği belirlenen Azure Virtual Machines şifreleme için aşağıdaki adımları öneririz:

* Azure PowerShell'i yükleyip yapılandırın. Bu Azure Virtual Machines şifreleme için gereken önkoşulları ayarlama gerekli PowerShell komutlarını çalıştırmanıza olanak sağlar.
* Edinin ve Azure Disk şifrelemesi önkoşulları Azure PowerShell betiğini çalıştırın.
* Sanal makinelerinizi şifreleyin.

[Bir Azure sanal Makine'yi şifreleme](security-center-disk-encryption.md) Bu adımlarda size yol gösterir.  Bu konu, disk şifrelemesi'ni yapılandırma istemci makine gibi Windows 10 kullandığınızı varsayar.

Azure sanal makineler için kullanılabilir birçok yaklaşım vardır. Azure PowerShell veya Azure CLI konusunda zaten bilgiliyseniz alternatif yaklaşımlar kullanmayı tercih edebilirsiniz. Bu diğer yaklaşımlar hakkında bilgi edinmek için [Azure disk şifrelemesi](../security/azure-security-disk-encryption.md).

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede Güvenlik Merkezi öneri "Uygula disk şifrelemesi." uygulamak nasıl oluşturulacağını gösterir Disk şifrelemesi hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Azure anahtar kasası ile şifreleme ve anahtar yönetimi](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (video, 36 min 39 sn)--korumak ve verilerinizi korumaya yardımcı olmak için disk şifreleme Yönetimi Iaas Vm'leri ve Azure anahtar kasası için kullanmayı öğrenin.
* [Bir Azure sanal Makine'yi şifreleme](security-center-disk-encryption.md) (belge)--Azure Virtual Machines şifreleme öğrenin.
* [Azure disk şifrelemesi](../security/azure-security-disk-encryption.md) (belge)--disk şifrelemesi Windows ve Linux VM'ler için etkinleştirmeyi öğrenin.

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) --güvenlik ilkeleri yapılandırmayı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) --Azure kaynaklarınızı sağlığını izlemek öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) --nasıl önerilerin Azure kaynaklarınızı korumanıza yardımcı öğrenin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) --Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

<!--Image references-->
[1]: ./media/security-center-apply-disk-encryption/apply-disk-encryption.png
