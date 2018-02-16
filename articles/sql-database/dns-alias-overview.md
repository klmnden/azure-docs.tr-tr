---
title: "Azure SQL veritabanı için DNS diğer adı | Microsoft Docs"
description: "Uygulamalarınızı Azure SQL veritabanı sunucunuzun adını için diğer ad bağlanabilir. Bu sırada, diğer dilediğiniz zaman, vb. sınama kolaylaştırmak için işaret SQL veritabanını değiştirebilirsiniz."
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: DNS alias
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: 
ms.date: 02/05/2018
ms.reviewer: genemi;ayolubek
ms.author: dmalik
ms.openlocfilehash: b216bd933f9096f46aee2b719394f9df65adc769
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="dns-alias-for-azure-sql-database"></a>Azure SQL veritabanı için DNS diğer adı

Azure SQL veritabanı bir etki alanı adı sistemi (DNS) sunucusu vardır. PowerShell ve REST API'leri kabul [oluşturmak ve DNS diğer adları yönetmek için çağrıları](#anchor-powershell-code-62x) SQL veritabanı sunucusu adı için.

A *DNS diğer adı* Azure SQL veritabanı sunucusu adı yerine kullanılabilir. İstemci programları diğer kendi bağlantı dizelerini kullanabilirsiniz. DNS diğer adı, istemci programları farklı sunuculara yönlendirebilirsiniz bir çeviri katmanı sağlar. Bu katman, bulma ve tüm istemcileri ve onların bağlantı dizelerini düzenlemek zorunda kalmadan, sorunlar ödemek zorunda kalmamasını sağlar.

Aşağıdaki durumlarda bir DNS diğer adı için yaygın kullanımları şunlardır:

- Bir Azure SQL Server için adı anımsaması kolay bir oluşturun.
- İlk geliştirme sırasında diğer adınız bir SQL veritabanı sunucusu testine başvurabilir. Uygulama Canlı gittiğinde, üretim sunucusuna başvurmak için diğer ad değiştirebilirsiniz. Üretim test geçiş veritabanı sunucusuna bağlanın çeşitli istemciler yapılandırmaları kendilerine herhangi bir değişiklik gerektirmez.
- Uygulamanızdaki yalnızca veritabanı başka bir SQL veritabanı sunucusuna taşınır varsayalım. Burada birkaç istemcisi yapılandırmalarını değiştirmek zorunda kalmadan diğer adı değiştirebilirsiniz.

#### <a name="domain-name-system-dns-of-the-internet"></a>Internet etki alanı adı sistemi (DNS)

Internet DNS kullanır. DNS, kolay adlar Azure SQL veritabanı sunucunuzun adını çevirir.

## <a name="scenarios-with-one-dns-alias"></a>Bir DNS diğer adı ile senaryoları

Yeni bir Azure SQL veritabanı sunucusuna, sisteminizin geçiş yapmanız varsayalım. Geçmişte bulmak ve her istemci programı her bağlantı dizesinde güncelleştirmek için gerekli. Ancak şimdi, bağlantı dizeleri bir DNS diğer adı kullanırsanız, yalnızca bir diğer ad özelliği güncelleştirilmesi gerekir.

Azure SQL veritabanı'nın DNS diğer ad özelliği aşağıdaki senaryolarda yardımcı olabilir:

#### <a name="test-to-production"></a>Üretim için test

İstemci programları geliştirmeye başlayın, bunları bir DNS diğer adı, bağlantı dizeleri kullanmak vardır. Azure SQL veritabanı sunucunuzun deneme sürümü için diğer ad noktasının özelliklerini olun.

Daha sonra yeni sistem üretimde Canlı gittiğinde, üretim SQL veritabanı sunucusuna işaret etmek için diğer ad özelliklerini güncelleştirebilirsiniz. Hiçbir değişiklik istemci programları için gereklidir.

#### <a name="cross-region-support"></a>Çapraz bölge desteği

Bir olağanüstü durum kurtarma için farklı bir coğrafi bölge SQL veritabanı sunucunuzun shift. Bir sistem için bir DNS diğer adı kullanarak daha bulmak ve tüm istemciler için tüm bağlantı dizelerini güncelleştirmek için gereken önlenebilir. Bunun yerine, artık, veritabanını barındıran yeni SQL veritabanı sunucusuna başvurmak için bir diğer ad güncelleştirebilirsiniz.




## <a name="properties-of-a-dns-alias"></a>Bir DNS diğer adı özellikleri

Aşağıdaki özellikleri her DNS diğer adı, SQL veritabanı sunucusu için geçerlidir:

- *Benzersiz bir ad:* oluşturduğunuz her diğer adın tüm Azure SQL veritabanı sunucuları arasında benzersiz olduğundan, yalnızca sunucu olarak adlardır.

- *Sunucu gereklidir:* bir DNS diğer adı oluşturulamıyor sürece tam olarak bir sunucuya başvuruda ve sunucu zaten mevcut olmalıdır. Güncelleştirilmiş bir diğer ad, tam olarak bir var olan sunucu her zaman başvurmalıdır.
    - Bir SQL veritabanı sunucusu bıraktığınızda Azure sistem sunucusuna başvuran tüm DNS diğer adları da bırakır.

- *Herhangi bir bölgeye bağlanmamış:* DNS diğer adları bir bölgeye bağlı değil. DNS diğer adlar, herhangi bir coğrafi bölgede bulunan bir Azure SQL veritabanı sunucusuna başvurmak için güncelleştirilebilir.
    - Ancak, başka bir sunucuya başvurmak için bir diğer ad güncelleştirirken, her iki sunucuyu aynı Azure bulunmalıdır *abonelik*.

- *İzinleri:* bir DNS diğer adı yönetmek için kullanıcının olmalıdır *sunucu katkıda bulunan* izinleri ya da daha yüksek. Daha fazla bilgi için bkz: [Azure portalında rol tabanlı erişim denetimi ile çalışmaya başlama](../active-directory/role-based-access-control-what-is.md).





## <a name="manage-your-dns-aliases"></a>DNS diğer adları yönetme

Program aracılığıyla DNS diğer adları yönetmenize olanak tanıyan REST API'leri ve PowerShell cmdlet'leri kullanılabilir.

#### <a name="rest-apis-for-managing-your-dns-aliases"></a>DNS diğer adları yönetmek için REST API'leri

<!-- TODO
??2 "soon" in the following live sentence, is not the best situation.
TODO update this subsection very soon after REST API docu goes live.
Dev = Magda Bojarska
Comment as of:  2018-01-26
-->

REST API belgelerine aşağıdaki web konumu kullanılabilir:
- [Azure SQL veritabanı REST API'si](https://docs.microsoft.com/rest/api/sql/)

Ayrıca, REST API'leri, GitHub üzerinde görülebilir:
- [Azure SQL veritabanı sunucusu, DNS diğer adı REST API'leri](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/sql/resource-manager/Microsoft.Sql/preview/2017-03-01-preview/serverDnsAliases.json)

<a name="anchor-powershell-code-62x"/>

#### <a name="powershell-for-managing-your-dns-aliases"></a>DNS diğer adları yönetmek için PowerShell

PowerShell cmdlet'leri kullanılabilir REST API'larını çağırma.

Bir kod örneği, DNS diğer adları yönetmek için kullanılan PowerShell cmdlet'lerinin en belgelenmiştir:
- [Azure SQL veritabanı için DNS diğer adı için PowerShell](dns-alias-powershell.md)


Aşağıdaki kod örneğinde kullanılan cmdlet'ler şunlardır:
- [AzureRMSqlServerDNSAlias yeni](https://docs.microsoft.com/powershell/module/AzureRM.Sql/New-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): Azure SQL veritabanı hizmetinin sistemde yeni bir DNS diğer adı oluşturur. Diğer Azure SQL veritabanı sunucusuna 1 başvuruyor.
- [Get-AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Get-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): almak ve 1 SQL DB sunucusuna atanan tüm DNS diğer adları listesi.
- [Set-AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Set-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): diğer adı için yapılandırılmış sunucu adını değiştirir, SQL veritabanı sunucusuna 2 1 sunucusundan bakın.
- [Remove-AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Remove-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): Remove the DNS alias from SQL DB server 2, by using the name of the alias.


Önceki cmdlet'ler eklenmiştir **AzureRM.Sql** 5.1.1 modülü sürümünden başlayarak modülü.




## <a name="limitations-during-preview"></a>Önizleme sırasında sınırlamaları

Şu anda bir DNS diğer adı aşağıdaki sınırlamalara sahiptir:

- *2 dakikalık gecikme:* kaldırıldı veya güncelleştirilmesi bir DNS diğer adı en çok 2 dakika sürer.
    - Herhangi kısa bir gecikme bağımsız olarak, istemci bağlantıları eski sunucusuna başvuran diğer hemen durdurur.

- *DNS araması:* belirli bir DNS diğer adı başvurduğu hangi gerçekleştirerek sunucusudur denetlemek için yalnızca yetkili yolu şu an bir [DNS araması](https://docs.microsoft.com/windows-server/administration/windows-commands/nslookup).

- *[Tablo denetim desteklenmiyor](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md):* sahip bir Azure SQL veritabanı sunucusuna bir DNS diğer adı kullanılamıyor *tablo denetim* bir veritabanında etkin.
    - Tablo denetim kullanım dışıdır.
    - İçin taşıma öneririz [Blob denetimi](sql-database-auditing.md).




## <a name="related-resources"></a>İlgili kaynaklar

- [Azure SQL Database iş sürekliliğine genel bakış](sql-database-business-continuity.md), olağanüstü durum kurtarma dahil.



## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL veritabanı için DNS diğer adı için PowerShell](dns-alias-powershell.md)

