---
title: Oluşturma ve BizTalk Hizmetleri bir yedekten geri yükleme | Microsoft Docs
description: BizTalk Hizmetleri, yedekleme ve geri yükleme içerir. Ne yedeklenir belirlemek ve oluşturma ve bir yedekleme geri yükleme hakkında bilgi edinin. MABS, WABS
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: 59f91173-4683-48df-abd5-41262bfce6df
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: ee86b9aa2d920668cf036f3e8f8634e9289e8913
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58916878"
---
# <a name="biztalk-services-backup-and-restore"></a>BizTalk Services: Yedekleme ve Geri Yükleme

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk Services yedekleme ve geri yükleme özelliklerini içerir. 

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

> [!NOTE]
> Karma bağlantılar sürüm ne olursa olsun, yedeklenmeyen. Karma bağlantılarınızı yeniden oluşturmanız gerekir.


## <a name="before-you-begin"></a>Başlamadan önce
* Yedekleme ve geri yükleme için tüm sürümlerinde kullanılabilir olmayabilir. Bkz: [BizTalk Services: Sürümler grafiği](biztalk-editions-feature-chart.md).
* Yedekleme içeriği aynı BizTalk hizmeti veya yeni bir BizTalk hizmetine geri yüklenebilir. BizTalk hizmeti aynı adı kullanarak geri yüklemek için mevcut BizTalk hizmeti silinmesi gerekir ve ad kullanılabilir olmalıdır. BizTalk hizmeti sildikten sonra istiyordu kullanılabilir olması için aynı adı çok uzun zaman alabilir. Kullanılabilir olması için aynı adı bekleyemiyorsanız, ardından yeni bir BizTalk hizmetine geri yükleyin.
* BizTalk Hizmetleri aynı sürümü veya daha yüksek bir sürüme geri yüklenebilir. BizTalk Hizmetleri, daha düşük bir sürüme geri yükleme, yedeklemenin ne zaman alındığı gelen desteklenmiyor.
  
    Örneğin, temel sürümünü kullanarak bir yedekleme Premium sürümüne geri yüklenebilir. Premium Edition'ı kullanarak bir yedekleme standart Sürüm'e geri yüklenemez.
* EDI denetim numaralarını denetim numaralarını sürekliliği sağlamak için yedeklenir. Son yedeklemeden sonra iletileri işlenir, bu yedekleme içerik geri yinelenen denetim numaraları neden olabilir.
* Bir batch etkin iletileriniz varsa, toplu işlem **önce** bir yedek çalıştırıyor. Bir yedekleme (gerekli veya zamanlanmış olarak) oluştururken, toplu iletileri hiçbir zaman depolanır. 
  
    **Bir yedekleme ile bir batch etkin iletileriniz alınmışsa, bu iletileri yedeklenmez ve bu nedenle kaybolur.**
* İsteğe bağlı: BizTalk Hizmetleri portalında herhangi bir yönetim işlemini durdurun.

## <a name="create-a-backup"></a>Yedekleme oluşturma
Bir yedekleme herhangi bir zamanda geçen ve tamamen sizin tarafınızdan denetlenir. Bir yedekleme oluşturmak için kullanın [azure'da BizTalk Services'ı yönetmek için REST API](/previous-versions/azure/reference/dn232347(v=azure.100)).

## <a name="restore"></a>Geri Yükleme
Bir yedeklemeyi geri yüklemek için kullanmak [azure'da BizTalk Services'ı yönetmek için REST API](/previous-versions/azure/reference/dn232347(v=azure.100)).

### <a name="postrestore"></a>Bir yedekleme geri yükledikten sonra
BizTalk hizmeti her zaman içinde geri bir **askıya alındı** durumu. Bu durumda, yeni ortam dahil olmak üzere işlevsel hale gelmeden önce herhangi bir yapılandırma değişikliği yapabilirsiniz:

* BizTalk hizmeti uygulamalarını Azure BizTalk Hizmetleri SDK'sını kullanarak oluşturduysanız, geri yüklenen bir ortam ile çalışmanız için bu uygulamaları erişim denetimi (ACS) kimlik bilgilerini güncelleştirmeniz gerekebilir.
* Bir BizTalk hizmet ortamı çoğaltmak için BizTalk hizmeti geri. Bu durumda, bir kaynak FTP klasörü kullanın özgün BizTalk Services Portalı'nda yapılandırılmış anlaşmaları varsa farklı kaynak FTP klasörü kullanmak için yapılan anlaşmaları yeni geri yüklenen ortamında güncelleştirmeniz gerekebilir. Aksi takdirde, iki farklı sözleşmeler aynı iletiyi çekmeye çalışıyor olabilir.
* Birden fazla BizTalk hizmet ortamları sağlamak için geri yüklediyseniz, Visual Studio uygulamalarını, PowerShell cmdlet'leri, REST API'leri veya ticari ortak Yönetimi OM API'leri doğru ortamda hedef emin olun.
* Otomatik yedekleri üzerinde yeni geri yüklenen BizTalk hizmet ortamı yapılandırmak için iyi bir uygulamadır.

## <a name="what-gets-backed-up"></a>Ne yedeklenir
Bir yedek oluşturulduğunda, aşağıdaki öğeler desteklenir:

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH>Yedeklenen öğeleri</TH> 
</tr> 
<tr>
<td colspan="2">
 <strong>Azure BizTalk Hizmetleri portalını</strong></td>
</tr> 
<tr>
<td>Yapılandırma ve çalışma zamanı</td> 
<td>
<ul>
<li>İş ortağı ve profil ayrıntıları</li>
<li>İş ortağı sözleşmeleri</li>
<li>Dağıtılan özel derlemeler</li>
<li>Dağıtılan köprüleri</li>
<li>Sertifikalar</li>
<li>Dağıtılan dönüşümler</li>
<li>İşlem hatları</li>
<li>Oluşturduğunuz ve kaydettiğiniz BizTalk Hizmetleri portalında şablonları</li>
<li>X12 ST01 ve GS01 eşlemeleri</li>
<li>Denetim numaraları (EDI)</li>
<li>AS2 ileti MIC değerleri</li>
</ul>
</td>
</tr> 

<tr>
<td colspan="2">
 <strong>Azure BizTalk hizmeti</strong></td>
</tr> 
<tr>
<td>SSL Sertifikası</td> 
<td>
<ul>
<li>SSL sertifika verileri</li>
<li>SSL sertifika parolası</li>
</ul>
</td>
</tr> 
<tr>
<td>BizTalk hizmeti ayarları</td> 
<td>
<ul>
<li>Ölçek birimi sayısı</li>
<li>Sürüm</li>
<li>Ürün Sürümü</li>
<li>Bölge/veri merkezi</li>
<li>Access Control Service (ACS) ad alanı ve anahtarı</li>
<li>İzleme veritabanı bağlantı dizesi</li>
<li>Depolama hesabı bağlantı dizesi arşivleme</li>
<li>İzleme depolama hesabı bağlantı dizesi</li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2">
 <strong>Ek öğeler</strong></td>
</tr> 
<tr>
<td>Veritabanı izleme</td> 
<td>BizTalk hizmeti oluşturulduğunda, Azure SQL veritabanı sunucusu ve veritabanı izleme adı dahil olmak üzere izleme veritabanı ayrıntıları girilir. İzleme veritabanı otomatik olarak yedeklenmez.
<br/><br/>
<strong>Önemli</strong><br/>
İzleme veritabanı silinir ve veritabanı gereksinimlerini kurtarıldı, önceki yedekleme mevcut olması gerekir. Bir yedek yoksa, izleme veritabanı ve onun veri kurtarılabilir değildir. Bu durumda, aynı veritabanı adında izleme yeni bir veritabanı oluşturun. Coğrafi çoğaltma önerilir.</td>
</tr> 
</table>

## <a name="next"></a>Sonraki
Azure BizTalk hizmetleri oluşturmak için Git [BizTalk Services: Sağlama](https://go.microsoft.com/fwlink/p/?LinkID=302280). Uygulamalar oluşturmaya başlamak için [Azure BizTalk Services](https://go.microsoft.com/fwlink/p/?LinkID=235197)’a gidin.

## <a name="see-also"></a>Ayrıca Bkz.
* [BizTalk hizmeti yedekleme](https://go.microsoft.com/fwlink/p/?LinkID=325584)
* [BizTalk hizmeti yedekten geri yükleyin.](https://go.microsoft.com/fwlink/p/?LinkID=325582)
* [BizTalk Services: Geliştirici, temel, standart ve Premium sürümler grafiği](https://go.microsoft.com/fwlink/p/?LinkID=302279)
* [BizTalk Services: Sağlama](https://go.microsoft.com/fwlink/p/?LinkID=302280)
* [BizTalk Services: Durum grafiğini hazırlama](https://go.microsoft.com/fwlink/p/?LinkID=329870)
* [BizTalk Services: Pano, İzleyici ve ölçek sekmeleri](https://go.microsoft.com/fwlink/p/?LinkID=302281)
* [BizTalk Services: Azaltma](https://go.microsoft.com/fwlink/p/?LinkID=302282)
* [BizTalk Services: Verenin adı ve verenin anahtarı](https://go.microsoft.com/fwlink/p/?LinkID=303941)
* [Azure BizTalk Services SDK'sını Kullanmaya Nasıl Başlarım](https://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png

