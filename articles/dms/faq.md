---
title: Azure veritabanı geçiş hizmeti ile ilgili SSS | Microsoft Docs
description: Veritabanı geçişleri gerçekleştirmek için Azure veritabanı geçiş hizmeti kullanma hakkında sık sorulan sorular hakkında bilgi edinin.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 05/24/2019
ms.openlocfilehash: 856eee294eaa1426bc7c06661ac62ed0f9824dcb
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66225340"
---
# <a name="faq-about-using-the-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti kullanma hakkında SSS

Bu makalede, Azure veritabanı geçiş hizmeti ilgili yanıtları ile birlikte kullanma hakkında sık sorulan soruların listelenmektedir.

### <a name="q-what-is-azure-database-migration-service"></a>S. Azure veritabanı geçiş hizmeti nedir?

Azure veritabanı geçiş hizmeti, birden çok veritabanı kaynağını sorunsuz geçiş için en düşük kapalı kalma süresi ile Azure Data platformlarına sağlamak için tasarlanmış tam olarak yönetilen bir hizmettir. Hizmet şu anda sürmekte olan geliştirme çalışmalarını odaklanan ile kullanıma sunulmuştur:

* Güvenilirlik ve performans.
* Kaynak-hedef çiftlerinin yinelemeli eklenmesi.
* Sorunsuz geçişler sürekli yatırım.

### <a name="q-what-source-target-pairs-does-the-azure-database-migration-service-currently-support"></a>S. Hangi kaynak-hedef çiftlerinin Azure veritabanı geçiş hizmeti şu anda destekliyor mu?

Hizmet şu anda çeşitli geçiş senaryolarını destekler. Her bir kullanılabilir geçiş senaryosu durumunu tam listesi için bkz [Azure veritabanı geçiş hizmeti tarafından desteklenen geçiş senaryoları durumunu](https://docs.microsoft.com/azure/dms/resource-scenario-status). Diğer geçiş senaryoları Önizleme aşamasındadır ve DMS Önizleme sitesi aracılığıyla ADAYLIK gönderme gerektirir. Senaryoları Önizleme aşamasındadır ve bu teklifleri biri katılmayı kaydolmak için tam bir listesi için bkz. [DMS Önizleme site](https://aka.ms/dms-preview/).

### <a name="q-how-does-the-azure-database-migration-service-compare-to-other-microsoft-database-migration-tools-such-as-the-database-migration-assistant-dma-or-sql-server-migration-assistant-ssma"></a>S. Nasıl Azure veritabanı geçiş hizmeti veritabanı geçiş Yardımcısı (DMA) veya SQL Server Geçiş Yardımcısı (SSMA) gibi diğer Microsoft veritabanı geçiş araçları arasındaki fark nedir?

Azure veritabanı geçiş hizmeti, uygun ölçekte Microsoft azure'a veritabanı geçişi için tercih edilen yöntemdir. Veritabanı Geçiş Araçları nasıl diğer Microsoft Azure veritabanı geçiş hizmeti karşılaştırır üzerinde daha fazla ayrıntı için ve Web günlüğü yayınlama çeşitli senaryolar için hizmetinin kullanılmasıyla ilgili öneriler için bkz. [farklılaştırılması Microsoft'un veritabanı Geçiş Araçları ve Hizmetleri](https://blogs.msdn.microsoft.com/datamigration/2017/10/13/differentiating-microsofts-database-migration-tools-and-services/).

### <a name="q-how-does-the-azure-database-migration-service-compare-to-the-azure-migrate-offering"></a>S. Nasıl Azure veritabanı geçiş hizmeti, Azure geçişi teklife arasındaki fark nedir?

Azure geçişi hizmeti ile Azure Iaas için şirket içi sanal makinelerin geçişini yardımcı olur. Hizmet, geçiş uygunluğunu ve performans tabanlı boyutlandırmayı değerlendirir ve şirket içi sanal makinelerinizi Azure'da çalıştırmak için maliyet tahminleri sağlar. Azure geçişi, şirket içi sanal makine tabanlı iş yüklerinin Azure ıaas'ye geçirilmesi lift-and-shift ile taşıma için kullanışlıdır. Ancak, Azure veritabanı geçiş hizmeti farklı olarak, Azure geçişi sunan Azure SQL veritabanı, SQL Azure veya Azure SQL veritabanı yönetilen örneği gibi Azure PaaS ilişkisel veritabanı platformlar için bir özel veritabanı geçiş hizmeti değildir.

### <a name="q-what-versions-of-sql-server-does-the-azure-database-migration-service-support-as-a-source"></a>S. SQL Server'ın hangi sürümleri Azure veritabanı geçiş hizmeti, bir kaynak olarak destekliyor mu?

Azure veritabanı geçiş hizmeti, SQL Server'dan Geçiş sırasında SQL Server 2005, SQL Server 2017 ile destekler.

### <a name="q-what-is-a-summary-of-the-steps-required-to-use-the-azure-database-migration-service-to-perform-a-database-migration"></a>S. Veritabanı geçişi gerçekleştirmek için Azure veritabanı geçiş hizmeti kullanmak için gerekli adımları özetini nedir?

Bir normal, basit bir veritabanı geçiş sırasında:

1. Bir hedef veritabanları oluşturun.
2. Veritabanı şeması kullanarak geçirmek [veritabanı geçiş Yardımcısı'nı](https://www.microsoft.com/en-us/download/details.aspx?id=53595).
3. Azure Veritabanı Geçiş Hizmeti örneği oluşturma.
4. Kaynak veritabanları, hedef veritabanları ve geçirilecek tabloları belirten bir geçiş projesi oluşturun.
5. Tam Yük başlatın.
6. Sonraki doğrulama seçin.
7. Üretim ortamınızın yeni bulut tabanlı veritabanına el ile bir geçiş gerçekleştirin.

### <a name="q-what-are-the-prerequisites-for-using-the-azure-database-migration-service"></a>S. Azure veritabanı geçiş hizmeti kullanmak için Önkoşullar nelerdir?

Azure veritabanı geçiş Hizmeti'nin veritabanı geçişlerini gerçekleştirirken sorunsuz çalıştığından emin olmak için gereken birkaç önkoşul vardır. Bazı Önkoşullar, diğer ön koşulları için belirli bir senaryoya benzersiz çalışırken service tarafından desteklenen tüm senaryolarda (kaynak-hedef çiftlerinin) arasında geçerlidir.

Tüm desteklenen geçiş senaryoları arasında ortak olan azure veritabanı geçiş hizmeti Önkoşullar dahil etme gereksinimi:

* Kullanarak şirket içi kaynak sunucularınıza siteden siteye bağlantı sağlar Azure Resource Manager dağıtım modelini kullanarak sanal ağ için Azure veritabanı geçiş hizmeti oluşturma [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [ VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
* Azure sanal ağınız (VNet) emin ağ güvenlik grubu kurallarını yoksa aşağıdaki Engelle iletişim bağlantı noktası 443, 53, 9354, 445, 12000. Azure VNet NSG trafik filtreleme hakkında daha fazla ayrıntı için bkz [ağ güvenlik grupları ile ağ trafiğini filtreleme](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
* Kaynak veritabanlarınız önünde bir güvenlik duvarı Gereci kullanırken oluşan kaynak veritabanları geçiş için erişmek Azure veritabanı geçiş hizmeti izin vermek için güvenlik duvarı kuralları eklemeniz gerekebilir.

Azure veritabanı geçiş hizmetini kullanarak belirli bir geçiş senaryoları mücadele etmek için gereken tüm önkoşulları listesi için Azure veritabanı geçiş hizmeti ilgili öğreticilerde bakın [belgeleri](https://docs.microsoft.com/azure/dms/dms-overview) üzerinde docs.microsoft.com.

### <a name="q-how-do-i-find-the-ip-address-for-the-azure-database-migration-service-so-that-i-can-create-an-allow-list-for-the-firewall-rules-used-to-access-my-source-database-for-migration"></a>S. Böylece geçiş için kaynak Veritabanım erişmek için kullanılan güvenlik duvarı kuralları için bir izin verilenler listesi oluşturmak nasıl IP adresi için Azure veritabanı geçiş hizmeti bulabilirim?

Geçiş için kaynak veritabanınıza erişmek Azure veritabanı geçiş hizmeti sağlayan güvenlik duvarı kuralları eklemeniz gerekebilir. Hizmeti için IP adresi dinamik, ancak Express Route kullanıyorsanız, bu adresi şirket ağınıza tarafından özel olarak atanır. İlişkili ağ arabirimi bulmak için aynı kaynak grubunda sağlanan Azure veritabanı geçiş Hizmeti'nin kaynak olarak aramaktır uygun IP adresini tanımlamak için en kolay yolu. Genellikle ağ arabirimi kaynağın adını NIC ön Ekle başlayan ve ardından bir benzersiz karakter ve sayı dizisi, örneğin NIC jj6tnztnmarpsskr82rbndyp. Bu ağ arabirimi kaynağı'nı seçerek kaynak genel bakış Azure portal sayfasındaki izin verilenler listesine dahil edilmesi gereken IP adresini görebilirsiniz.

İzin verilenler listesinde SQL sunucusunun dinleme yaptığı bağlantı noktası kaynağını dahil etmek gerekebilir. Varsayılan olarak, ancak diğer bağlantı noktalarında dinlemek için SQL Server yapılandırılabilir kaynak bağlantı noktası 1433 güçlendirin. Bu durumda, bu bağlantı noktalarına izin verilenler listesine de eklemeniz gerekir. SQL Server dinamik yönetim görünümünde sorgu kullanarak dinlediği bağlantı noktasını belirleyebilirsiniz:

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

### <a name="q-are-there-any-recommendations-for-optimizing-the-performance-of-the-azure-database-migration-service"></a>S. Azure veritabanı geçiş Hizmeti performansını iyileştirmek için herhangi bir önerimiz var mı?

Hizmetini kullanarak veritabanı geçişinizi hızlandırmak için bazı şeyler yapabilirsiniz:

* Hizmet örneğinizi paralelleştirme ve daha hızlı veri aktarımı için birden çok Vcpu yararlanmak izin verecek şekilde oluşturduğunuzda, birden çok CPU genel amaçlı fiyatlandırma katmanı kullanın.
* Geçici olarak ölçeği artırma, azaltma Azure SQL veritabanı en aza indirmek için veri geçiş işlemi sırasında Premium katmanına SKU, Azure SQL veritabanı hedef örneği düşük düzeyli SKU'ları kullanırken, veri taşıma etkinliklerini etkileyebilir.

### <a name="q-how-do-i-set-up-an-azure-virtual-network"></a>S. Bir Azure sanal ağı nasıl ayarlayabilirim?

Size bir Azure sanal ağı işleminde yol birden çok Microsoft öğretici sırasında resmi belgelerine makalesinde görünür [Azure sanal ağı](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

### <a name="q-why-is-my-azure-database-migration-service-unavailable-or-stopped"></a>S. Neden benim Azure veritabanı geçiş hizmeti kullanılamıyor veya durduruldu olduğunu?

Kullanıcının Azure veritabanı geçiş hizmeti (DMS) açıkça durdurur veya hizmet 24 saatlik bir süre için etkin değilse, hizmeti durdurulmuş bir olması veya duraklatılmış otomatik. Her durumda, hizmet kullanılamıyor ve durdurulmuş durumda olacaktır.  Etkin geçişlerini devam ettirmek için hizmeti yeniden başlatın.

### <a name="q-where-can-i-leave-feedback-about-azure-database-migration-service"></a>S. Azure veritabanı geçiş hizmeti hakkında geri bildirim nereden bırakın?

Görüşlerinizi almak isteriz. Lütfen Geri bildirimlerinizi gönderin ve / fikirleri sahip kullanıcı sesi aracılığıyla Azure veritabanı geçiş hizmeti hakkında [burada](https://feedback.azure.com/forums/906100-azure-database-migration-service), veya adresinden ekibine başvurarak [isteyin Azure veritabanı geçişlerini](mailto:AskAzureDatabaseMigrations@service.microsoft.com).

## <a name="next-steps"></a>Sonraki adımlar

Bölgesel kullanılabilirlik ve Azure veritabanı geçiş Hizmeti'nin genel bakış için bkz [Azure veritabanı geçiş hizmeti nedir](dms-overview.md).
