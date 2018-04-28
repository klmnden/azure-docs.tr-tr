---
title: Azure veritabanı geçiş hizmeti ile ilgili SSS | Microsoft Docs
description: Veritabanı geçişleri gerçekleştirmek üzere Azure veritabanı geçiş hizmeti kullanma hakkında sık sorulan sorular öğrenin.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 01/25/2018
ms.openlocfilehash: 85052e1363ebbfe21cd7d6d5b3720f79cec7c417
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="faq-about-using-the-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti ile ilgili SSS
Bu makalede Azure veritabanı geçiş hizmeti ile ilgili yanıtlar birlikte kullanma hakkında sık sorulan soruların listelenmektedir.

### <a name="q-what-is-azure-database-migration-service"></a>Q. Azure veritabanı geçiş hizmeti nedir?
Azure veritabanı geçiş hizmeti, Azure veri platformları en az kapalı kalma süresi ile birden çok veritabanı kaynakları sorunsuz kümesinden sağlamak için tasarlanmış tam olarak yönetilen bir hizmettir. Hizmet şu anda odaklanan geliştirme çalışmalarını ile genel önizlemede değil:
- Güvenilirlik ve performans.
- Yinelemeli Ayrıca kaynak hedef çifti.
- Uyuşmazlık serbest geçişler sürekli yatırım.

### <a name="q-what-source-target-pairs-does-the-azure-database-migration-service-currently-support"></a>Q. Hangi kaynak hedef çiftleri Azure veritabanı geçiş hizmeti şu anda destekliyor mu?
Genel Önizleme hizmetinde şu anda Azure SQL veritabanı için SQL Server'dan geçişleri destekler ve bu senaryo için Azure veritabanı geçiş hizmeti kullanmaya başlamak için Azure Portalı'nı şimdi gidebilirsiniz. Yönetilen Azure SQL veritabanı örneğine SQL Server ve Oracle Azure SQL veritabanı gibi diğer kaynak hedef çifti ile sınırlı özel Önizleme kullanılabilir. Bu senaryoların sınırlı özel önizleme olarak katılmak için bir fırsat için kaydolun [burada](https://sqldatabase-migrationpreview.azurewebsites.net/).

### <a name="q-how-does-the-azure-database-migration-service-compare-to-other-microsoft-database-migration-tools-such-as-the-database-migration-assistant-dma-or-sql-server-migration-assistant-ssma"></a>Q. Azure veritabanı geçiş hizmeti veritabanı geçiş Yardımcısı (DMA) veya SQL Server Geçiş Yardımcısı (SSMA) gibi diğer Microsoft veritabanı Geçiş Araçları nasıl şekilde karşılaştırma?
Azure veritabanı geçiş hizmeti ölçekte veritabanı geçiş Microsoft Azure için tercih edilen yöntemdir. Diğer Microsoft Azure veritabanı geçiş hizmeti nasıl karşılaştırır hakkında daha fazla ayrıntı için Geçiş Araçları veritabanı ve blog nakil çeşitli senaryolar için hizmetinin kullanılmasıyla ilgili daha fazla önerileri için bkz: [ayrım Microsoft'un veritabanı Geçiş Araçları ve Hizmetleri](https://blogs.msdn.microsoft.com/datamigration/2017/10/13/differentiating-microsofts-database-migration-tools-and-services/).

### <a name="q-how-does-the-azure-database-migration-service-compare-to-the-azure-migrate-offering"></a>Q. Azure veritabanı geçiş hizmeti Azure geçirmek teklifine nasıl karşılaştırmak?
Geçiş Azure hizmeti Azure Iaas için şirket içi sanal makinelerin geçişini ile yardımcı olur. Hizmet geçiş uygunluğu ve performans tabanlı boyutlandırma değerlendirir ve şirket içi sanal makinelerinizi Azure'da çalışan için maliyet tahminleri sağlar. Azure geçirme, şirket içi VM tabanlı iş yüklerini Azure Iaas vm'lerine yükseltme shift geçirilmesi için kullanışlıdır. Ancak, Azure veritabanı geçiş hizmeti Azure geçirmek için Azure SQL Database veya SQL Azure veya Azure SQL veritabanı yönetilen örneği gibi Azure PaaS ilişkisel veritabanı platformlarda sunumu özel veritabanı geçiş hizmeti değildir.

### <a name="q-what-is-a-summary-of-the-steps-required-to-use-the-azure-database-migration-service-to-perform-a-database-migration"></a>Q. Veritabanı geçiş işlemini gerçekleştirmek için Azure veritabanı geçiş hizmeti kullanmak için gerekli olan adımları özetini nedir?
Tipik, basit bir veritabanı geçiş sırasında:
1.  Hedef veritabanı oluşturun.
2.  Veritabanı şeması kullanarak geçirmek [veritabanı geçiş Yardımcısı](https://www.microsoft.com/en-us/download/details.aspx?id=53595).
3.  Azure veritabanı geçiş hizmeti örneği oluşturun.
4.  Kaynak veritabanı, hedef veritabanları ve tablolar geçirmek için belirten bir geçiş projesi oluşturun.
5.  Tam Yük başlatır.
6.  Sonraki doğrulama seçin.
7.  Üretim ortamınıza yeni bulut tabanlı veritabanına el ile geçiş gerçekleştirin. 

### <a name="q-what-are-the-prerequisites-for-using-the-azure-database-migration-service"></a>Q. Azure veritabanı geçiş hizmeti kullanmak için önkoşulları nelerdir?
Azure veritabanı geçiş hizmeti düzgün veritabanı geçiş gerçekleştirirken çalıştığından emin olmak için gereken birkaç önkoşul vardır. Bazı Önkoşullar Önkoşullar belirli bir senaryoyla benzersiz durumdayken service tarafından desteklenen tüm senaryoları (kaynak hedef çiftleri) uygulamak.
Tüm desteklenen geçiş senaryoları arasında ortak olan azure veritabanı geçiş hizmeti Önkoşullar gerek şunlardır:
- Kullanarak, şirket içi kaynak sunucular için siteden siteye bağlantı sağlar Azure Resource Manager dağıtım modelini kullanarak Azure veritabanı geçiş hizmeti için bir VNET oluşturma [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Azure sanal ağ (VNET) ağ güvenlik grubu kuralları blok aşağıdaki iletişim bağlantı noktaları 443, 53, 9354, 445, 12000. Azure VNET NSG trafik filtreleme daha ayrıntılı bilgi için bkz: [filtre ağ güvenlik grupları ile ağ trafiği](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Bir güvenlik duvarı gerecini kaynak veritabanları önünde kullanırken, geçiş için kaynak veritabanlarının erişmek Azure veritabanı geçiş hizmeti izin veren güvenlik duvarı kuralları eklemeniz gerekebilir.
 
Azure veritabanı geçiş hizmeti ilgili eğitimlerine belirli geçiş senaryoları Azure veritabanı geçiş hizmeti ile rekabet için gereken tüm önkoşulları listesi için bkz [belgelerine](https://docs.microsoft.com/azure/dms/dms-overview) üzerinde docs.microsoft.com.

### <a name="q-how-do-i-find-the-ip-address-for-the-azure-database-migration-service-so-that-i-can-create-an-allow-list-for-the-firewall-rules-used-to-access-my-source-database-for-migration"></a>Q. Geçiş için kaynak veritabanını erişmek için kullanılan güvenlik duvarı kuralları için bir izin verilenler oluşturmak için nasıl IP adresi için Azure veritabanı geçiş hizmeti bulabilirim?
Geçiş için kaynak veritabanına erişmek Azure veritabanı geçiş hizmeti veren güvenlik duvarı kuralları eklemeniz gerekebilir. Hizmet için IP adresi dinamik olduğundan, ancak hızlı rota kullanıyorsanız, bu adresi şirket ağınıza tarafından özel olarak atanır. İlişkili ağ arabirimi bulmak için aynı kaynak grubunda sağlanan Azure veritabanı geçiş hizmeti kaynak olarak aramaktır uygun IP adresi tanımlamak için en kolay yolu. Genellikle ağ arabirimi kaynağın adını NIC önek ile başlar ve ardından bir benzersiz karakter ve numara sırası, örnek NIC jj6tnztnmarpsskr82rbndyp. Bu ağ arabirimi kaynak seçerek, izin verilenler listesinde kaynağa genel bakış Azure portal sayfası dahil edilmesi gereken IP adresi görebilirsiniz.

SQL Server izin verilenler listesinde dinleme bağlantı noktası kaynağını eklemeniz gerekebilir. Varsayılan olarak, bağlantı noktası 1433 olduğu, ancak diğer bağlantı noktalarında dinleme için SQL Server Kaynak yapılandırılabilir. Bu durumda, bu bağlantı noktalarına izin verilenler de eklemeniz gerekir. SQL Server dinamik yönetim görünümünü sorgu kullanarak dinlediği bağlantı noktasını belirleyebilirsiniz:

```sql
    SELECT DISTINCT 
        local_tcp_port 
    FROM sys.dm_exec_connections 
    WHERE local_tcp_port IS NOT NULL
```
SQL Server için SQL Server hata günlüğünü sorgulayarak dinlediği bağlantı noktasını da belirleyebilirsiniz:

```sql
    USE master
    GO
    xp_readerrorlog 0, 1, N'Server is listening on' 
    GO
```

### <a name="q-are-there-any-recommendations-for-optimizing-the-performance-of-the-azure-database-migration-service"></a>Q. Azure veritabanı geçiş Hizmeti performansını iyileştirmek için herhangi bir önerimiz var mı?
Hizmetini kullanarak, veritabanı geçiş hızlandırmak için bazı şeyler yapabilir:
- Paralelleştirme ve daha hızlı veri aktarımı için birden çok Vcpu'lar yararlanmak hizmet izin vermek için hizmet örneği oluşturduğunuzda, birden çok CPU genel amaçlı fiyatlandırma katmanı kullanın.
- Geçici olarak ölçek büyütme Azure SQL veritabanı hedef örneğinizi Azure SQL Database, azaltma en aza indirmek için veri geçiş işlemi sırasında Premium katmanına SKU alt düzey SKU'ları kullanırken veri taşıma etkinliklerini etkileyebilir.

### <a name="q-how-do-i-set-up-an-azure-virtual-network"></a>Q. Bir Azure sanal ağı nasıl ayarlarım?
Bir Azure sanal ayarlama işleminde size yol birden çok Microsoft öğreticiler, resmi belge makalesinde görünür [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

### <a name="q-why-is-my-azure-database-migration-service-unavailable-or-stopped"></a>Q. Neden Azure veritabanı geçiş Hizmetim kullanılamıyor veya durdurulmuş mı?
Kullanıcı Azure veritabanı geçiş hizmeti (DMS) açıkça durdurursa veya hizmet 24 saat boyunca etkin değilse, hizmeti durdurulmuş bir olması veya duraklatılmış otomatik. Her durumda, hizmet ve durdurulmuş durumda kullanılamaz olacaktır.  Etkin geçişler sürdürmek için hizmeti yeniden başlatın.

### <a name="q-where-can-i-leave-feedback-about-the-azure-database-migration-service"></a>Q. Burada Azure veritabanı geçiş hizmeti hakkında geri bildirim bırakabilirsiniz?
Sizden duymak istiyoruz. Lütfen herhangi bir geri bildirim gönderin ve / fikirleriniz kullanıcı sesi aracılığıyla Azure veritabanı geçiş hizmeti hakkında [burada](https://feedback.azure.com/forums/906100-azure-database-migration-service).

## <a name="next-steps"></a>Sonraki adımlar
Azure veritabanı geçiş hizmeti ve genel Önizleme sırasında bölgesel kullanılabilirlik genel bakış için bkz: [Azure veritabanı geçiş hizmeti Önizleme nedir](dms-overview.md). 
