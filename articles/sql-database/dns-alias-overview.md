---
title: Azure SQL veritabanı için DNS diğer adı | Microsoft Docs
description: Uygulamalarınızı Azure SQL veritabanı sunucunuzun adını için bir diğer ad bağlanabilirsiniz. Bu arada, diğer her zaman, vb. test edilmesini kolaylaştırmak için işaret SQL veritabanını değiştirebilirsiniz.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: genemi, ayolubek, jrasnick
manager: craigg
ms.date: 06/26/2019
ms.openlocfilehash: bb38f73308fb1eb67be310120cb589cb9412e737
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67461818"
---
# <a name="dns-alias-for-azure-sql-database"></a>Azure SQL veritabanı için DNS diğer adı

Azure SQL veritabanı, bir etki alanı adı sistemi (DNS) sunucusu vardır. PowerShell ve REST API'lerinin kabul [oluşturmak ve DNS diğer adları yönetmek için çağrıları](#anchor-powershell-code-62x) SQL veritabanı sunucu adınız için.

A *DNS diğer adı* Azure SQL veritabanı sunucu adı yerine kullanılabilir. İstemci programları diğer kendi bağlantı dizelerini kullanabilirsiniz. DNS diğer adı, istemci programları farklı sunuculara yönlendirebilirsiniz bir çeviri katmanı sağlar. Bu katman, bulun ve tüm istemcilerine ve bunların bağlantı dizelerini düzenlemek zorunda kalmadan, sorunlar ödemek zorunda kalmamasını sağlar.

Aşağıdaki durumlarda bir DNS diğer adı için yaygın kullanımları şunlardır:

- Bir Azure SQL sunucusunun adını hatırlamak bir kolayca oluşturun.
- İlk geliştirme sırasında test SQL veritabanı sunucusu için diğer adınızı başvurabilir. Uygulamayı Canlı çıktığında, üretim sunucusuna başvurmak için diğer ad değiştirebilirsiniz. Üretim testinden geçişi, veritabanı sunucusuna çeşitli istemciler yapılandırmaları değişiklik gerektirmez.
- Başka bir SQL veritabanı sunucusuna, uygulamanızda yalnızca veritabanı taşınır varsayalım. Burada, diğer birkaç istemcisi yapılandırmalarını değiştirmek zorunda kalmadan değiştirebilirsiniz.
- Bölgesel bir kesinti sırasında farklı bir sunucu ve bölge, veritabanını kurtarmak için coğrafi geri yükleme kullanın. Böylece var olan istemci uygulaması için yeniden bağlanamadı yeni sunucuya işaret edecek şekilde var olan diğer adınızı değiştirebilirsiniz. 

## <a name="domain-name-system-dns-of-the-internet"></a>İnternet etki alanı adı sistemi (DNS)

Internet DNS kullanır. DNS, Azure SQL veritabanı sunucunuzun adını kolay adları çevirir.

## <a name="scenarios-with-one-dns-alias"></a>Bir DNS diğer adı ile senaryoları

Yeni bir Azure SQL veritabanı sunucusuna sisteminizin geçiş etmeniz gerektiğini varsayalım. Her bağlantı dizesinde her istemci programı güncelleştirmek için gereken geçmişte. Ancak, bağlantı dizeleri bir DNS diğer adı kullanırsanız, yalnızca bir diğer ad özelliği artık güncelleştirilmelidir.

Azure SQL veritabanı'nın DNS diğer ad özelliği, aşağıdaki senaryolarda yardımcı olabilir:

### <a name="test-to-production"></a>Üretim için test etme

İstemci programlarından geliştirme başlattığınızda, bir DNS diğer adı, bağlantı dizelerini kullanma sağlayın. Azure SQL veritabanı sunucunuz deneme sürümü için diğer ad noktasının özelliklerini yapmanızı ister.

Daha sonra yeni sisteme üretim ortamında dinamik çıktığında, üretim SQL veritabanı sunucusuna işaret etmesi diğer özellikleri güncelleştirebilirsiniz. Hiçbir değişiklik istemci programları için gereklidir.

### <a name="cross-region-support"></a>Bölgeler arası destek

Bir olağanüstü durum kurtarma için farklı bir coğrafi bölgede SQL veritabanı sunucunuza kaydırma. Bir DNS diğer adı kullanan bir sistem bulun ve tüm istemciler için tüm bağlantı dizelerini güncelleştirmek için gereken önlenebilir. Bunun yerine, artık veritabanınızı barındıran yeni SQL veritabanı sunucusuna başvurmak için bir diğer ad güncelleştirebilirsiniz.

## <a name="properties-of-a-dns-alias"></a>Bir DNS diğer adı özellikleri

Aşağıdaki özellikler, SQL veritabanı sunucunuz için her bir DNS diğer adı için geçerlidir:

- *Benzersiz adı:* Yalnızca sunucu adları olarak oluşturduğunuz her bir diğer ad tüm Azure SQL veritabanı sunucuları arasında benzersizdir.
- *Sunucu gereklidir:* Tam olarak bir sunucusuna başvuran ve sunucu zaten mevcut olmalıdır bir DNS diğer adı oluşturulamıyor. Güncelleştirilmiş bir diğer ad, her zaman tam olarak bir tane mevcut sunucunun başvurmanız gerekir.
  - SQL veritabanı sunucusu düşürdüğünüzde, Azure sistem sunucusuna başvuran tüm DNS diğer adları da bırakır.
- *Herhangi bir bölgesine bağlı değil:* DNS diğer adları bir bölgeye bağlı değil. Herhangi bir DNS diğer adları, herhangi bir coğrafi bölgede bulunan bir Azure SQL veritabanı sunucusuna başvurmak için güncelleştirilebilir.
  - Ancak, başka bir sunucuya başvurmak için bir diğer ad güncelleştirirken, her iki sunucuyu aynı Azure bulunmalıdır *abonelik*.
- *İzinler:* Bir DNS diğer adı'nı yönetmek için kullanıcının olmalıdır *Server Katılımcısı* izinler veya üzeri. Daha fazla bilgi için [Azure portalında rol tabanlı erişim denetimi ile çalışmaya başlama](../role-based-access-control/overview.md).

## <a name="manage-your-dns-aliases"></a>Kendi DNS diğer adları yönetme

REST API'lerini hem PowerShell cmdlet'leri sağlamak, DNS diğer adları programlı olarak yönetmek kullanılabilir.

### <a name="rest-apis-for-managing-your-dns-aliases"></a>Kendi DNS diğer adları yönetmek için REST API'leri

REST API belgelerini, aşağıdaki web konumun kullanılabilir:

- [Azure SQL veritabanı REST API](https://docs.microsoft.com/rest/api/sql/)

Ayrıca, REST API'leri github'da görülebilir:

- [Azure SQL veritabanı sunucusu, DNS diğer adı REST API'leri](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/sql/resource-manager/Microsoft.Sql/preview/2017-03-01-preview/serverDnsAliases.json)

<a name="anchor-powershell-code-62x"/>

#### <a name="powershell-for-managing-your-dns-aliases"></a>Kendi DNS diğer adları yönetmek için PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

PowerShell cmdlet'leri kullanılabilir REST API'larını çağırma.

DNS diğer adları yönetmek için kullanılan PowerShell cmdlet'lerini bir kod örneği, konumunda belgelenmiştir:

- [PowerShell için Azure SQL veritabanı için DNS diğer adı](dns-alias-powershell.md)

Aşağıdaki kod örneğinde kullanılan cmdlet'ler şunlardır:

- [Yeni AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/New-azSqlServerDnsAlias): Azure SQL veritabanı hizmet sistemde yeni bir DNS diğer ad oluşturur. Diğer ad, 1 Azure SQL veritabanı sunucusuna ifade eder.
- [Get-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Get-azSqlServerDnsAlias): Alın ve SQL DB sunucusu 1 atanmış olan tüm DNS diğer adları listesi.
- [Set-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Set-azSqlServerDnsAlias): Diğer adı için yapılandırılmış bir sunucu adı değiştirir, SQL veritabanı sunucusuna 2 1 sunucusundan bakın.
- [Remove-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Remove-azSqlServerDnsAlias): DNS diğer adı, diğer adı kullanarak, 2, SQL DB sunucusundan kaldırın.

## <a name="limitations-during-preview"></a>Önizleme süresince sınırlamaları

Şu anda, bir DNS diğer adı aşağıdaki sınırlamalara sahiptir:

- *En fazla 2 dakika gecikmesi:* Kaldırılan veya güncelleştirilmesi bir DNS diğer adı en fazla 2 dakika sürer.
  - Kısa bir gecikme bağımsız olarak eski sunucunun istemci bağlantılarını başvuran diğer hemen durdurur.
- *DNS araması:* Şimdilik yalnızca yetkili gerçekleştirerek belirli bir DNS diğer adı başvurduğu hangi sunucu olup olmadığını denetleyin şekilde bir [DNS araması](https://docs.microsoft.com/windows-server/administration/windows-commands/nslookup).
- _Tablo denetimi desteklenmez:_ Bir DNS diğer adı olan bir Azure SQL veritabanı sunucusunda kullanamazsınız *tablo denetleme* bir veritabanında etkin.
  - Tablo denetimi kullanım dışı bırakılmıştır.
  - İçin taşıma öneririz [Blob denetimi](sql-database-auditing.md).

## <a name="related-resources"></a>İlgili kaynaklar

- [Azure SQL veritabanı'nda iş sürekliliğine genel bakış](sql-database-business-continuity.md), olağanüstü durum kurtarma dahil olmak üzere.

## <a name="next-steps"></a>Sonraki adımlar

- [PowerShell için Azure SQL veritabanı için DNS diğer adı](dns-alias-powershell.md)
