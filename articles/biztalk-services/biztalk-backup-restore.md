---
title: Oluşturma ve BizTalk Services bir yedeğini geri | Microsoft Docs
description: BizTalk Services yedekleme ve geri yüklemeyi içerir. Ne yedeklenir belirlemek ve nasıl oluşturulacağı ve bir yedeğini geri öğrenin. MABS, WABS
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
ms.openlocfilehash: 45365092f5bcd1a8d309c10404a7437c494a8967
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "24102350"
---
# <a name="biztalk-services-backup-and-restore"></a>BizTalk Services: Yedekleme ve Geri Yükleme

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk Services yedekleme ve geri yükleme özellikleri içerir. 

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

> [!NOTE]
> Karma bağlantılar, sürüm ne olursa olsun yedeklenmiş değil. Karma bağlantılar yeniden oluşturmanız gerekir.


## <a name="before-you-begin"></a>Başlamadan önce
* Yedekleme ve geri yükleme tüm sürümleri için kullanılamayabilir. Bkz: [BizTalk Services: sürümler grafiği](biztalk-editions-feature-chart.md).
* Yedekleme içeriği aynı BizTalk hizmeti ya da yeni bir BizTalk hizmeti geri yüklenebilir. Aynı adı kullanarak BizTalk hizmeti geri yüklemek için mevcut BizTalk hizmeti silinmelidir ve ad kullanılabilir olmalıdır. BizTalk hizmeti sildikten sonra kullanılabilir olması aynı adı istedik daha uzun zaman alabilir. Kullanılabilir olması aynı adı bekleyemiyorsanız, yeni bir BizTalk hizmeti geri yükleyin.
* BizTalk Services aynı sürümü veya daha yüksek bir sürüme geri yüklenebilir. BizTalk Services daha düşük bir sürüme geri yükleme, yedeklemenin ne zaman alındığı gelen desteklenmiyor.
  
    Örneğin, temel sürümünü kullanarak bir yedekleme Premium sürümüne geri yüklenebilir. Premium Edition'ı kullanarak bir yedekleme, Standard Edition geri yüklenemez.
* EDI denetim numaraları denetim sayıların sürekliliği sağlamak için yedeklenir. Son yedeklemeden sonra işlenen iletileri, bu yedekleme içeriğin geri yinelenen denetim numaraları neden olabilir.
* Bir toplu etkin iletileri varsa, toplu işlem **önce** bir yedekleme çalıştırılıyor. Bir yedekleme (gerekli veya zamanlanmış olarak) oluştururken, toplu iletiler hiçbir zaman saklanır. 
  
    **Bir yedekleme toplu etkin iletilerle alınmışsa, bu iletiler yedeklenmez ve bu nedenle kaybolur.**
* İsteğe bağlı: BizTalk Services Portalı'nda herhangi bir yönetim işlemini durdurun.

## <a name="create-a-backup"></a>Bir yedekleme oluşturun
Bir yedekleme herhangi bir zamanda alınabilir ve sizin tarafınızdan tamamen denetlenir. Bir yedekleme oluşturmak için kullanmak [Azure BizTalk hizmetlerinin yönetilmesi için REST API](https://msdn.microsoft.com/library/azure/dn232347.aspx).

## <a name="restore"></a>Geri Yükleme
Bir yedeklemeyi geri yüklemek için kullanmak [Azure BizTalk hizmetlerinin yönetilmesi için REST API](https://msdn.microsoft.com/library/azure/dn232347.aspx).

### <a name="postrestore"></a>Bir yedekleme geri yükledikten sonra
BizTalk hizmeti her zaman içinde geri bir **askıya** durumu. Bu durumda, önce yeni ortam işlevsel dahil olmak üzere yapılandırma değişiklikleri yapabilirsiniz:

* BizTalk hizmeti uygulamalarını Azure BizTalk Services SDK'sını kullanarak oluşturduysanız, geri yüklenen ortamı ile çalışmak için bu uygulamalarda erişim denetimi (ACS) kimlik bilgilerini güncelleştirmeniz gerekebilir.
* Mevcut bir BizTalk hizmeti ortamına çoğaltmak için BizTalk hizmeti geri yükleyin. Bu durumda, bir kaynak FTP klasörü kullanın özgün BizTalk Services Portalı'nda yapılandırılmış anlaşmaları varsa farklı kaynak FTP klasör kullanmak için anlaşmaları yeni geri yüklenen ortamında güncelleştirmeniz gerekebilir. Aksi takdirde, aynı iletiyi çekme çalışılırken iki farklı anlaşmaları olabilir.
* Birden çok BizTalk hizmeti ortamları için geri yüklediyseniz, Visual Studio uygulamaları, PowerShell cmdlet'leri, REST API'leri veya ticari ortak Yönetimi OM API'leri doğru ortamında hedef emin olun.
* Geri yüklenen yeni BizTalk hizmeti ortamda otomatik yedeklemeler yapılandırmak için iyi bir uygulamadır.

## <a name="what-gets-backed-up"></a>Ne yedeklenir
Bir yedek oluşturulduğunda, aşağıdaki öğeleri desteklenir:

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH>Yedeklenecek öğe</TH> 
</tr> 
<tr>
<td colspan="2">
 <strong>Azure BizTalk Services portalı</strong></td>
</tr> 
<tr>
<td>Yapılandırma ve çalışma zamanı</td> 
<td>
<ul>
<li>İş ortakları ve profili ayrıntıları</li>
<li>Ortak sözleşmeleri</li>
<li>Dağıtılan özel derlemeler</li>
<li>Dağıtılan köprüleri</li>
<li>Sertifikalar</li>
<li>Dağıtılan dönüşümler</li>
<li>İşlem hatları</li>
<li>Oluşturulan ve BizTalk Services Portalı'nda kaydedilen şablonları</li>
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
<td>SSL sertifikası</td> 
<td>
<ul>
<li>SSL sertifikası verileri</li>
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
<li>Ürün sürümü:</li>
<li>Bölge/veri merkezi</li>
<li>Erişim denetimi Hizmeti'nden (ACS) ad alanı ve anahtarı</li>
<li>Veritabanı bağlantı dizesi izleme</li>
<li>Depolama hesabı bağlantı dizesi arşivleme</li>
<li>Depolama hesabı bağlantı dizesi izleme</li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2">
 <strong>Ek öğeler</strong></td>
</tr> 
<tr>
<td>Veritabanı izleme</td> 
<td>BizTalk hizmeti oluşturduğunuzda, Azure SQL veritabanı sunucusu ve veritabanı izleme adı dahil olmak üzere veritabanı İzleme Ayrıntıları girilir. İzleme veritabanı otomatik olarak yedeklenmez.
<br/><br/>
<strong>Önemli</strong><br/>
İzleme veritabanı silinir ve veritabanı gereksinimlerini kurtarılan, önceki bir yedekleme mevcut olması gerekir. Bir yedek yoksa, izleme veritabanı ve onun verileri kurtarılabilir değildir. Bu durumda, aynı veritabanı adında yeni bir izleme veritabanı oluşturun. Coğrafi çoğaltma önerilir.</td>
</tr> 
</table>

## <a name="next"></a>Sonraki
Azure BizTalk Services oluşturmak için şu adrese gidin [BizTalk Services: Sağlama](http://go.microsoft.com/fwlink/p/?LinkID=302280). Uygulamalar oluşturmaya başlamak için [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197)’a gidin.

## <a name="see-also"></a>Ayrıca Bkz.
* [BizTalk hizmeti yedekleme](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [BizTalk hizmeti yedekten geri yükleyin](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [BizTalk Services: Geliştirici, temel, standart ve Premium sürümler grafiği](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [BizTalk Services: sağlama](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [BizTalk Services: Durum Grafiğini hazırlama](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [BizTalk Services: Pano, İzleyici ve Ölçek sekmeleri](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [BizTalk Services: Azaltma](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [BizTalk Services: Verenin Adı ve Verenin Anahtarı](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [Azure BizTalk Services SDK'sını Kullanmaya Nasıl Başlarım](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png

