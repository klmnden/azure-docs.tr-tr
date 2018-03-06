---
title: "Müşteri kullanarak Azure Storage hizmeti şifreleme anahtarları Azure anahtar Kasası'yönetilen | Microsoft Docs"
description: "Müşteri kullanarak veri alma anahtarları yönetildiğinde şifresini çözmek ve Azure depolama hizmeti şifrelemesi özelliği hizmet tarafında Azure Blob Depolama veri depolarken şifrelemek için kullanın."
services: storage
documentationcenter: .net
author: lakasa
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: lakasa
ms.openlocfilehash: 0a05a0d28899cc3db11f8fda8aec5bd6ed9bd5f8
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="storage-service-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Depolama hizmeti müşteri kullanarak şifreleme anahtarları Azure anahtar Kasası'yönetilen

Microsoft Azure korumak ve Kuruluş güvenliği ve uyumluluk taahhüt karşılamak için verilerinizi korumaya yardımcı olmayı taahhüt eder.  REST verilerinizi koruyabilmek için bir depolama hizmeti şifreleme (otomatik olarak depolama birimine yazarken, verileri şifreler ve onu alırken, verilerin şifresini çözer SSE), kullanılacak yoludur. Şifreleme ve şifre çözme otomatik, tamamen saydam ve 256 bit kullanır [AES şifreleme](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), güçlü blok birini şifrelemeleri kullanılabilir.

Microsoft tarafından yönetilen şifreleme anahtarları ile SSE kullanabilir veya kendi şifreleme anahtarlarınızı kullanabilirsiniz. Bu makalede ikinci anlatılmaktadır. Daha fazla SSE veya Microsoft tarafından yönetilen anahtarlar kullanılarak hakkında genel bilgi için [bekleyen veri için depolama hizmeti şifrelemesi](../storage-service-encryption.md).

Kendi şifreleme anahtarlarınızı kullanma olanağı sağlamak için Blob storage için SSE Azure anahtar kasası (AKV) ile tümleşiktir. Kendi şifreleme anahtarları oluşturabilir ve bunları AKV depolamak ya da AKV'ın API'leri, şifreleme anahtarları oluşturmak için kullanabilirsiniz. Yalnızca AKV yönetmek ve anahtarlarınızı denetlemek izin vermez, ayrıca, anahtar kullanımını denetleme sağlar. 

Neden kendi anahtarları oluşturmak istiyor? Daha fazla esneklik, oluşturmak, döndürme, devre dışı bırakın ve erişim denetimleri tanımlamak olanak sağlar. Ayrıca, verilerinizi korumak için kullanılan şifreleme anahtarlarını denetlemek sağlar.

## <a name="sse-with-customer-managed-keys-preview"></a>SSE müşteri yönetilen anahtarlar Önizleme ile

Bu özellik şu anda önizleme sürümündedir. Bu özelliği kullanmak için yeni bir depolama hesabı oluşturmanız gerekir. Yeni bir anahtar kasası oluşturabilir ya da ve anahtar veya bir var olan anahtar kasasını ve anahtar kullanabilirsiniz. Depolama hesabı ve anahtar kasasıyla aynı bölgede olması gerekir, ancak bunlar farklı Aboneliklerde olabilir.

Önizlemede katılmayı başvurun [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com). Biz önizlemede katılmak için özel bir bağlantı sağlar.

Daha fazla bilgi edinmek için bkz [SSS](#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).

> [!IMPORTANT]
> Bu makaledeki adımları izleyerek önce önizleme için kaydolmanız gerekir. Önizleme erişimi olmadan, Portalı'nda bu özelliği etkinleştirmek mümkün olmaz.

## <a name="getting-started"></a>Başlarken
## <a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>1. adım: [yeni depolama hesabı oluşturma](../storage-create-storage-account.md)

## <a name="step-2-enable-encryption"></a>2. adım: Etkinleştirme şifreleme
Depolama hesabı kullanarak için SSE etkinleştirebilirsiniz [Azure portal](https://portal.azure.com). Ayarlar dikey penceresinde depolama hesabı için aşağıdaki çizimde gösterildiği gibi için Blob hizmeti bölümüne bakın ve şifreleme'yi tıklatın.

![Portal gösteren ekran görüntüsü şifreleme seçeneği](./media/storage-service-encryption-customer-managed-keys/ssecmk1.png)
<br/>*Blob hizmeti için SSE etkinleştir*

Program aracılığıyla istiyorsanız, etkinleştirme veya bir depolama hesabında depolama hizmeti şifrelemesi devre dışı bırakmak, kullanabilirsiniz [Azure depolama kaynak sağlayıcısı REST API](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN), [.NET için depolama kaynak sağlayıcısı istemci Kitaplığı](https://docs.microsoft.com/dotnet/api/?redirectedfrom=MSDN), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.0.0), veya [Azure CLI](https://docs.microsoft.com/azure/storage/storage-azure-cli).

"Kendi anahtarınızı kullan" onay kutusunu göremiyorsanız, bu ekranda, Önizleme için onaylanan değil. Bir e-posta Gönder [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) ve onay isteyin.

![Şifreleme Önizleme gösteren portal ekran görüntüsü](./media/storage-service-encryption-customer-managed-keys/ssecmk1.png)

Varsayılan olarak, Microsoft tarafından yönetilen anahtarlar SSE kullanır. Kendi anahtarları kullanmak için kutuyu işaretleyin. Ardından, anahtarınızı URI belirtin veya bir anahtarı ve anahtar kasası seçiciden seçin.

## <a name="step-3-select-your-key"></a>3. adım: anahtarınızı seçin

![Portal gösteren ekran görüntüsü şifrelemeleri anahtar seçeneğinizi kullanın](./media/storage-service-encryption-customer-managed-keys/ssecmk2.png)

![Şifreleme ile anahtar URI seçenek girin gösteren portal ekran görüntüsü](./media/storage-service-encryption-customer-managed-keys/ssecmk3.png)

Depolama hesabı anahtar kasası erişimi yoksa, gerekli anahtar kasası depolama hesaplarına erişim vermek için Azure Powershell kullanarak şu komutu çalıştırabilirsiniz.

![Erişim için anahtar kasasını reddedildi gösteren portal ekran görüntüsü](./media/storage-service-encryption-customer-managed-keys/ssecmk4.png)

Ayrıca, Azure portalında Azure anahtar kasası gidip depolama hesabına erişim izni verme Azure portalı üzerinden erişim izni verebilir.

## <a name="step-4-copy-data-to-storage-account"></a>4. adım: veri depolama alanına kopyalanmaya
Böylece şifreli verileri yeni depolama hesabınızda aktarmak istiyorsanız, başvurmak [adım 3, Başlarken bölümünde kalan verileri için depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption#step-3-copy-data-to-storage-account).

## <a name="step-5-query-the-status-of-the-encrypted-data"></a>5. adım: şifrelenmiş verileri durumunu sorgulama
Şifrelenmiş veriler durumunu sorgulamak için başvurmak [adım 4, Başlarken bölümünde kalan verileri için depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption#step-4-query-the-status-of-the-encrypted-data).

## <a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Sık sorulan bekleyen veri için depolama hizmeti şifrelemesi hakkında sorular
**S: Premium depolama kullanıyorum; Müşteri yönetilen anahtarlarla SSE kullanabilir miyim?**

A: Microsoft tarafından yönetilen ile SSE ve müşteri anahtarları Evet yönetilen standart depolama ve Premium depolama desteklenir. 

**S: yeni depolama hesapları ile SSE Azure PowerShell ve Azure CLI kullanarak etkin müşteri yönetilen anahtarlarla oluşturabiliyorum?**

Y: Evet.

**S: ne kadar daha fazla desteklemediğini Azure depolama maliyeti müşteriyle SSE anahtarları yönetilen etkin mi?**

Y: var. Azure anahtar kasası kullanmak için ilişkili bir maliyet Daha fazla ayrıntı şu adresi ziyaret edin [anahtar kasası fiyatlandırma](https://azure.microsoft.com/en-us/pricing/details/key-vault/). SSE kullanmak için ek bir maliyet yoktur.

**S: şifreleme anahtarlarının erişimi iptal?**

A: Evet erişimi dilediğiniz zaman iptal edebilirsiniz. Anahtarlarınızı erişimi iptal etmek için birkaç yolu vardır. Başvurmak [Azure anahtar kasası PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.0.0) ve [Azure anahtar kasası CLI](https://docs.microsoft.com/cli/azure/keyvault) daha fazla ayrıntı için. Hesap şifreleme anahtarı Azure Storage tarafından erişilemez olduğundan erişimi iptal ediliyor verimli depolama hesabındaki tüm BLOB'lar erişmesini engeller.

**S: bir depolama hesabı ve anahtarı farklı bölgede oluşturabilirim?**

A: Hayır depolama hesabı ve anahtarı kasa anahtarı aynı bölgede olması gerekir. 

**S: SSE müşteri yönetilen anahtarlarla depolama hesabı oluşturulurken etkinleştirebilirim?**

Y: No Depolama hesabı oluşturulurken SSE etkinleştirdiğinizde, yalnızca Microsoft tarafından yönetilen tuşlarını kullanabilirsiniz. Yönetilen müşteri anahtarları kullanmak istiyorsanız, depolama hesabı özellikleri güncelleştirmeniz gerekir. Program aracılığıyla depolama hesabınızı güncelleştirmek için REST veya depolama istemci kitaplıklarından birini kullanın veya hesabı oluşturduktan sonra Azure portalını kullanarak depolama hesabı özellikleri güncelleştirin.

**S: yönetilen anahtarları SSE müşteriyle kullanarak ı şifreleme devre dışı?**

A: Hayır anahtarları yönetilen SSE müşteriyle kullanarak sırasında şifreleme devre dışı bırakılamıyor. Şifreleme devre dışı bırakmak için Microsoft tarafından yönetilen anahtarları kullanarak geçmesi gerekir. Azure portal veya PowerShell kullanarak bunu yapabilirsiniz.

**S: yeni bir depolama hesabı oluşturduğunuzda, varsayılan olarak etkin SSE mi?**

Y: SSE varsayılan olarak etkin değildir; Azure Portalı'nı etkinleştirmek için kullanabilirsiniz. Depolama kaynak sağlayıcısı REST API'sini kullanarak bu özelliği program aracılığıyla da etkinleştirebilirsiniz. 

**Depolama Hesabımı s: şifreleme etkinleştirilemiyor.**

Y: Resource Manager depolama hesabı mı? Klasik depolama hesapları desteklenmez. Müşteri yönetilen anahtarlarla SSE de yalnızca yeni oluşturulan Resource Manager depolama hesaplarında etkinleştirilebilir.

**S: olduğu SSE müşteriyle yalnızca belirli bölgelerine izin anahtarları yönetiliyor?**

Y: SSE yalnızca belirli bölgeler Bu önizleme için Blob storage için kullanılabilir. E-posta [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) kullanılabilirlik ve önizleme ayrıntıları denetlemek için. 

**S: herhangi bir sorununuz veya geri bildirim sağlamak istiyorsanız nasıl ı biriyle iletişim?**

Y: kişi [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) depolama hizmeti şifrelemesi ile ilgili sorunlar için. 

## <a name="next-steps"></a>Sonraki adımlar

*   Güvenlik kapsamlı kümesi hakkında daha fazla bilgi için geliştiricilerin yetenekleri güvenli uygulamaları oluşturmak, bkz: [depolama Güvenlik Kılavuzu](https://docs.microsoft.com/azure/storage/storage-security-guide).
*   Azure anahtar kasası hakkında genel bilgi için bkz: [Azure anahtar kasası nedir](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)?
*   Azure anahtar kasası kullanmaya başlama için bkz: [Azure anahtar kasası ile çalışmaya başlama](../../key-vault/key-vault-get-started.md).
