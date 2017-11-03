---
title: "Oluşturma ve BizTalk Services bir yedeğini geri | Microsoft Docs"
description: "BizTalk Services yedekleme ve geri yüklemeyi içerir. Ne yedeklenir belirlemek ve nasıl oluşturulacağı ve bir yedeğini geri öğrenin. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 59f91173-4683-48df-abd5-41262bfce6df
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: c55d1ab124441c42101b4ad60924a9ea28231408
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="biztalk-services-backup-and-restore"></a>BizTalk Services: Yedekleme ve Geri Yükleme

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk Services yedekleme ve geri yükleme özellikleri içerir. Bu konu, yedekleme ve geri yükleme Klasik Azure portalını kullanarak BizTalk Services açıklar.

BizTalk Services kullanarak yedekleyebilirsiniz [BizTalk Services REST API](http://go.microsoft.com/fwlink/p/?LinkID=325584). 

> [!NOTE]
> Karma bağlantılar, sürüm ne olursa olsun yedeklenmiş değil. Karma bağlantılar yeniden oluşturmanız gerekir.


## <a name="before-you-begin"></a>Başlamadan önce
* Yedekleme ve geri yükleme tüm sürümleri için kullanılamayabilir. Bkz: [BizTalk Services: sürümler grafiği](biztalk-editions-feature-chart.md).
* Klasik Azure portalını kullanarak, bir talep üzerine yedekleme oluşturmak veya zamanlanmış bir yedekleme oluşturun. 
* Yedekleme içeriği aynı BizTalk hizmeti ya da yeni bir BizTalk hizmeti geri yüklenebilir. Aynı adı kullanarak BizTalk hizmeti geri yüklemek için mevcut BizTalk hizmeti silinmelidir ve ad kullanılabilir olmalıdır. BizTalk hizmeti sildikten sonra kullanılabilir olması aynı adı istedik daha uzun zaman alabilir. Kullanılabilir olması aynı adı bekleyemiyorsanız, yeni bir BizTalk hizmeti geri yükleyin.
* BizTalk Services aynı sürümü veya daha yüksek bir sürüme geri yüklenebilir. BizTalk Services daha düşük bir sürüme geri yükleme, yedeklemenin ne zaman alındığı gelen desteklenmiyor.
  
    Örneğin, temel sürümünü kullanarak bir yedekleme Premium sürümüne geri yüklenebilir. Premium Edition'ı kullanarak bir yedekleme, Standard Edition geri yüklenemez.
* EDI denetim numaraları denetim sayıların sürekliliği sağlamak için yedeklenir. Son yedeklemeden sonra işlenen iletileri, bu yedekleme içeriğin geri yinelenen denetim numaraları neden olabilir.
* Bir toplu etkin iletileri varsa, toplu işlem **önce** bir yedekleme çalıştırılıyor. Bir yedekleme (gerekli veya zamanlanmış olarak) oluştururken, toplu iletiler hiçbir zaman saklanır. 
  
    **Bir yedekleme toplu etkin iletilerle alınmışsa, bu iletiler yedeklenmez ve bu nedenle kaybolur.**
* İsteğe bağlı: BizTalk Services Portalı'nda herhangi bir yönetim işlemini durdurun.

## <a name="create-a-backup"></a>Bir yedekleme oluşturun
Bir yedekleme herhangi bir zamanda alınabilir ve sizin tarafınızdan tamamen denetlenir. Bu bölümde Azure Klasik portalı kullanarak yedeklemeler oluşturmak için aşağıdaki adımları listeler de dahil olmak üzere:

[İsteğe bağlı bir yedekleme](#backupnow)

[Yedeklemeyi zamanlama](#backupschedule)

#### <a name="backupnow"></a>İsteğe bağlı bir yedekleme
1. Klasik Azure portalında seçin **BizTalk Services**ve ardından istediğiniz BizTalk hizmeti yedekleme.
2. İçinde **Pano** sekmesine **yedekleme** sayfanın sonundaki.
3. Yedek bir ad girin. Örneğin *myBizTalkService*BU*tarih*.
4. Bir blob depolama hesabı seçin ve yedeklemeyi başlatmak için onay işaretini seçin.

Yedekleme tamamlandıktan sonra bir kapsayıcı girdiğiniz yedek adı ile depolama hesabı oluşturulur. Bu kapsayıcı, BizTalk hizmeti yedekleme yapılandırmanızı içerir.

#### <a name="backupschedule"></a>Yedeklemeyi zamanlama
1. Klasik Azure portalında seçin **BizTalk Services**, yedekleme işini zamanlamak ve ardından istediğiniz BizTalk hizmeti adını seçin **yapılandırma** sekmesi.
2. Ayarlama **yedekleme durumu** için **otomatik**. 
3. Seçin **depolama hesabı** yedeğini depolamak için girin **sıklığı** için yedekleme oluşturmaya ve yedeklemeleri tutmak için ne kadar süre (**bekletme gün**):
   
    ![][AutomaticBU]
   
    **Notlar**     
   
   * İçinde **bekletme gün**, bekletme dönemi yedekleme sıklığı büyük olmalıdır.
   * Seçin **her zaman en az bir yedekleme tutun**, saklama süresi olsa bile.
4. **Kaydet**’i seçin.

Zamanlanmış bir yedekleme işini çalıştırdığında, girdiğiniz depolama hesabında (yedek verileri depolamak için) bir kapsayıcı oluşturur. Kapsayıcının adı adlı *BizTalk hizmeti adı-tarih-saat*. 

BizTalk hizmeti Pano gösterirse bir **başarısız** durumu:

![Zamanlanan son yedekleme durumu][BackupStatus] 

Bağlantı gidermenize yardımcı olması için Yönetim Hizmetleri işlem günlükleri açar. Bkz: [BizTalk Services: işlem günlükleri kullanarak sorun giderme](http://go.microsoft.com/fwlink/p/?LinkId=391211).

## <a name="restore"></a>Geri Yükleme
Azure Klasik portalından veya yedeklerini geri [geri BizTalk hizmeti REST API'si](http://go.microsoft.com/fwlink/p/?LinkID=325582). Bu bölümde, Klasik portalı kullanarak geri yüklemek için adımları listelenir.

#### <a name="before-restoring-a-backup"></a>Bir yedeği geri yüklemeden önce
* Yeni izleme, arşivleme ve depoları izleme BizTalk hizmeti geri yüklenirken girilebilir.
* Aynı EDI çalışma zamanı verileri geri yüklendi. EDI çalışma zamanı yedekleme denetim numaraları depolar. Yedekleme sırayla geri yüklenen denetim numaralarıdır. Son yedeklemeden sonra işlenen iletileri, bu yedekleme içeriğin geri yinelenen denetim numaraları neden olabilir.

#### <a name="restore-a-backup"></a>Yedeği geri yükleme
1. Klasik Azure portalında seçin **yeni** > **uygulama hizmetleri** > **BizTalk hizmeti** > **geri**:
   
    ![Yedeği geri yükleme][Restore]
2. İçinde **yedekleme URL**, klasör simgesini seçin ve BizTalk hizmeti yapılandırma yedeği depolar Azure depolama hesabı'nı genişletin. Kapsayıcısını genişletin ve sağ bölmede, karşılık gelen yedekleme .txt dosyası seçin. 
   <br/><br/>
   Seçin **açık**.
3. Üzerinde **geri yükleme BizTalk hizmeti** want bir **BizTalk hizmeti adı** ve doğrulama **etki alanı URL'si**, **Edition**, ve **bölge** geri yüklenen BizTalk hizmeti için. **Yeni bir SQL veritabanı oluştur** veritabanı izleme için:
   
    ![][RestoreBizTalkService]
   
    İleri okunu seçin.
4. SQL veritabanı adını doğrulayın, bu sunucu için SQL veritabanı oluşturulacağı fiziksel sunucu ve bir kullanıcı adı/parola girin.

    SQL veritabanı sürümü, boyutu ve diğer özellikleri yapılandırmak isteyip istemediğinizi seçin **Gelişmiş veritabanı ayarlarını yapılandır**. 

    İleri okunu seçin.

1. Yeni bir depolama hesabı oluşturun veya BizTalk hizmeti için mevcut bir depolama hesabını girin.
2. Geri yüklemeyi başlatmak için onay işaretini seçin.

Geri yükleme işlemi başarıyla tamamlandığında, yeni bir BizTalk hizmeti askıya alınmış durumda Klasik Azure Portalı'ndaki BizTalk Services sayfasında listelenir.

### <a name="postrestore"></a>Bir yedekleme geri yükledikten sonra
BizTalk hizmeti her zaman içinde geri bir **askıya** durumu. Bu durumda, önce yeni ortam işlevsel dahil olmak üzere yapılandırma değişiklikleri yapabilirsiniz:

* BizTalk hizmeti uygulamalarını Azure BizTalk Services SDK'sını kullanarak oluşturduysanız, geri yüklenen ortamı ile çalışmak için bu uygulamalarda erişim denetimi (ACS) kimlik bilgilerini güncelleştirmeniz gerekebilir.
* Mevcut bir BizTalk hizmeti ortamına çoğaltmak için BizTalk hizmeti geri yükleyin. Bu durumda, bir kaynak FTP klasörü kullanın özgün BizTalk Services Portalı'nda yapılandırılmış anlaşmaları varsa farklı kaynak FTP klasör kullanmak için anlaşmaları yeni geri yüklenen ortamında güncelleştirmeniz gerekebilir. Aksi takdirde, aynı iletiyi çekme çalışılırken iki farklı anlaşmaları olabilir.
* Birden çok BizTalk hizmeti ortamları için geri yüklediyseniz, Visual Studio uygulamaları, PowerShell cmdlet'leri, REST API'leri veya ticari ortak Yönetimi OM API'leri doğru ortamında hedef emin olun.
* Geri yüklenen yeni BizTalk hizmeti ortamda otomatik yedeklemeler yapılandırmak için iyi bir uygulamadır.

Azure Klasik Portalı'nda BizTalk hizmetini başlatmak için geri yüklenen BizTalk hizmeti seçin ve Seç **Sürdür** görev çubuğunda. 

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
Klasik Azure portalında Azure BizTalk Services oluşturmak için şu adrese gidin [BizTalk Services: sağlama kullanarak Azure Klasik portalı](http://go.microsoft.com/fwlink/p/?LinkID=302280). Uygulamalar oluşturmaya başlamak için [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197)’a gidin.

## <a name="see-also"></a>Ayrıca Bkz.
* [BizTalk hizmeti yedekleme](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [BizTalk hizmeti yedekten geri yükleyin](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [BizTalk Services: Geliştirici, temel, standart ve Premium sürümler grafiği](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [BizTalk Services: Klasik portalı kullanarak Azure hazırlama](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [BizTalk Services: Durum Grafiğini hazırlama](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [BizTalk Services: Pano, İzleyici ve Ölçek sekmeleri](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [BizTalk Services: Azaltma](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [BizTalk Services: Verenin Adı ve Verenin Anahtarı](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [Azure BizTalk Services SDK'sını Kullanmaya Nasıl Başlarım](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png

