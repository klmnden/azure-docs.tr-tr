---
title: En son Azure konuk işletim sistemi sürümleri hakkında bilgi edinin | Microsoft Docs
description: Son sürüm haberleri ve SDK uyumluluk Azure bulut Hizmetleri konuk işletim sistemi için.
services: cloud-services
documentationcenter: na
author: raiye
manager: timlt
editor: ''
ms.assetid: 6306cafe-1153-44c7-8554-623b03d59a34
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 8/3/2018
ms.author: raiye
ms.openlocfilehash: 2ee31e0a2d563ddf2aa63498b4ca280e4da26754
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39524867"
---
# <a name="azure-guest-os-releases-and-sdk-compatibility-matrix"></a>Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi
En son Azure konuk işletim sistemi hakkında güncel bilgiler ile bulut Hizmetleri için sürümleri sağlar. Bu bilgiler bir konuk işletim sistemi devre dışı bırakılmasına sıranız yükseltme yolunuza planlamanıza yardımcı olur. Kullanılacak rollerinizi yapılandırırsanız *otomatik* konuk işletim sistemi güncelleştirmeleri açıklandığı [Azure konuk işletim sistemi güncelleştirme ayarları][Azure Guest OS Update Settings], bu sayfayı okuyun önemli değildir.

> [!IMPORTANT]
> Bu sayfa, bir konuk işletim sistemi üzerinde çalışan Cloud Services web ve çalışan rolleri için geçerlidir. Mevcut **geçerli** Iaas sanal makineleri için.
>
>


> [!TIP]
>  Abone [Konuk işletim sistemi güncelleştirme RSS akışı] tüm konuk işletim sistemi değişiklikleri hakkında ilk bildirim alanlardan almak için.
>
>

> [!IMPORTANT]
> Kasım sunum başlayarak, konuk işletim Sisteminin en son 2 sürümleri, desteklenen ve Azure portalında kullanılabilir olacaktır.
>
>

Konuk işletim sisteminizi güncelleştirin konusunda emin değilseniz? Denetleme [bu] [ cloud updates] uğradı.

## <a name="news-updates"></a>Haber güncelleştirmeleri

###### <a name="august-3-2018"></a>**3 Ağustos 2018**
Temmuz konuk işletim sistemi kullanıma sundu.

###### <a name="july-3-2018"></a>**3 Temmuz 2018**
Haziran konuk işletim sistemi kullanıma sundu.

###### <a name="june-1-2018"></a>**1 Haziran 2018'den**
Olabilir konuk işletim sistemi kullanıma sundu.

###### <a name="may-4-2018"></a>**4 Mayıs 2018**
Nisan konuk işletim sistemi kullanıma sundu.

###### <a name="april-6-2018"></a>**6 Nisan 2018**
Mart konuk işletim sistemi kullanıma sundu.

###### <a name="march-19-2018"></a>**19 Mart 2018**
Şubat konuk işletim sistemi kullanıma sundu.

###### <a name="january-29-2018"></a>**29 Ocak 2018**
Ocak konuk işletim sistemi için işletim sistemi ailesi 2 piyasaya Sürüldü (WA-GUEST-işletim sistemi-2.70_201801-01) & 3 (WA-GUEST-işletim sistemi-3.57_201801-01)

###### <a name="january-4-2018"></a>**4 Ocak 2018**
Ocak konuk işletim sistemi için işletim sistemi ailesi 4 piyasaya Sürüldü (WA-GUEST-işletim sistemi-4.50_201801-01) & 5 (WA-GUEST-işletim sistemi-5.15_201801-01) ve önemli güvenlik düzeltmelerini içerir.  

###### <a name="january-4-2018"></a>**4 Ocak 2018**
Aralık konuk işletim sistemi kullanıma sundu.

###### <a name="december-14-2017"></a>**14 Aralık 2017**
Kasım konuk işletim sistemi kullanıma sundu.

###### <a name="november-8-2017"></a>**8 Kasım 2017**
Ekim konuk işletim sistemi kullanıma sundu.



## <a name="releases"></a>Yayınlar
## <a name="family-5-releases"></a>Ailesi 5 yayınlar
**Windows Server 2016**

.NET framework yüklü: 4.0, 4.5, 4.5.1, 4.5.2, 4.6, 4.6.1, 4.6.2

> [!NOTE]
> İle tarihleri bir * tanımladığınıza göre değişebilir.
>
> İşletim sistemi ailesi 5 RDP parolasını en az 10 karakter uzunluğunda olmalıdır.
>

| Yapılandırma dizesi | Sürüm tarihi | Tarih devre dışı bırak | Süresi dolmuş tarih |
| --- | --- | --- | --- |
| WA-GUEST-OS-5.21_201807-02 |3 Ağustos 2018 |POST 5.23 |TBD |
| WA-GUEST-OS-5.20_201806-01 |3 Temmuz 2018 |POST 5.22 |TBD |
|~~WA-GUEST-OS-5.19_201805-01~~ |1 Haziran 2018'den |3 Ağustos 2018 |TBD |
|~~WA-GUEST-OS-5.18_201804-01~~ |4 Mayıs 2018 |3 Temmuz 2018 |TBD |
|~~WA-GUEST-OS-5.17_201803-01~~ |6 Nisan 2018 |1 Haziran 2018'den|TBD |
|~~WA-GUEST-OS-5.16_201802-01~~ |12 Mart 2018 |4 Mayıs 2018 |TBD |
|~~WA-GUEST-OS-5.15_201801-01~~ |4 Ocak 2018 |6 Nisan 2018 |TBD |
|~~WA-GUEST-OS-5.14_201712-01~~ |4 Ocak 2018 |12 Mart 2018 |TBD |
|~~WA-GUEST-OS-5.13_201711-01~~ |14 Aralık 2017 |4 Ocak 2018|TBD |
|~~WA-GUEST-OS-5.12_201710-02~~ |8 Kasım 2017 |4 Ocak 2018 |TBD |


## <a name="family-4-releases"></a>Ailesi 4 yayınlar
**Windows Server 2012 R2**

.NET framework yüklü: 4.0, 4.5, 4.5.1, 4.5.2

> [!NOTE]
> İle tarihleri bir * tanımladığınıza göre değişebilir
>
>

| Yapılandırma dizesi | Sürüm tarihi | Tarih devre dışı bırak | Süresi dolmuş tarih |
| --- | --- | --- | --- |
| WA-GUEST-OS-4.56_201807-02 |3 Ağustos 2018 |POST 4.58 |TBD |
| WA-GUEST-OS-4.55_201806-01 |3 Temmuz 2018 |POST 4.57 |TBD |
|~~WA-GUEST-OS-4.54_201805-01~~ |1 Haziran 2018'den |3 Ağustos 2018 |TBD |
|~~WA-GUEST-OS-4.53_201804-01~~ |4 Mayıs 2018 |3 Temmuz 2018 |TBD |
|~~WA-GUEST-OS-4.52_201803-01~~ |6 Nisan 2018 |1 Haziran 2018'den |TBD |
|~~WA-GUEST-OS-4.51_201802-01~~ |12 Mart 2018 |4 Mayıs 2018 |TBD |
|~~WA-GUEST-OS-4.50_201801-01~~ |4 Ocak 2018 |6 Nisan 2018 |TBD |
|~~WA-GUEST-OS-4.49_201712-01~~ |4 Ocak 2018 |12 Mart 2018 |TBD |
|~~WA-GUEST-OS-4.48_201711-01~~ |14 Aralık 2017 |4 Ocak 2018 |TBD |
|~~WA-GUEST-OS-4.47_201710-02~~ |8 Kasım 2017 |4 Ocak 2018 |TBD |


## <a name="family-3-releases"></a>Aile 3 yayınlar
**Windows Server 2012**

.NET framework yüklü: 4.0, 4.5, 4.5.1, 4.5.2

> [!NOTE]
> İle tarihleri bir * tanımladığınıza göre değişebilir
>
>

| Yapılandırma dizesi | Sürüm tarihi | Tarih devre dışı bırak | Süresi dolmuş tarih |
| --- | --- | --- | --- |
| WA-GUEST-OS-3.63_201807-02 |3 Ağustos 2018 |POST 3.65 |TBD |
| WA-GUEST-OS-3.62_201806-01 |3 Temmuz 2018 |POST 3.64 |TBD |
|~~WA-GUEST-OS-3.61_201805-01~~ |1 Haziran 2018'den |3 Ağustos 2018 |TBD |
|~~WA-GUEST-OS-3.60_201804-01~~ |4 Mayıs 2018 |3 Temmuz 2018 |TBD |
|~~WA-GUEST-OS-3.59_201803-01~~ |6 Nisan 2018 |1 Haziran 2018'den |TBD |
|~~WA-GUEST-OS-3.58_201802-01~~ |19 Mart 2018 |4 Mayıs 2018 |TBD |
|~~WA-GUEST-OS-3.57_201801-01~~ |29 Ocak 2018 |6 Nisan 2018 |TBD |
|~~WA-GUEST-OS-3.56_201712-01~~ |4 Ocak 2018 |19 Mart 2018 |TBD |
|~~WA-GUEST-OS-3.55_201711-01~~ |14 Aralık 2017 |29 Ocak 2018 |TBD |
|~~WA-GUEST-OS-3.54_201710-02~~ |8 Kasım 2017 |4 Ocak 2018 |TBD |


## <a name="family-2-releases"></a>Ailesi 2 yayınlar
**Windows Server 2008 R2 SP1**

.NET framework yüklü: 3.5, 4.0, 4.5, 4.5.1, 4.5.2

> [!NOTE]
> İle tarihleri bir * tanımladığınıza göre değişebilir
>
>

| Yapılandırma dizesi | Sürüm tarihi | Tarih devre dışı bırak | Süresi dolmuş tarih |
| --- | --- | --- | --- |
| WA-GUEST-OS-2.76_201807-02 |3 Ağustos 2018 |POST 2.78 |TBD |
| WA-GUEST-OS-2.75_201806-01 |3 Temmuz 2018 |POST 2.77 |TBD |
|~~WA-GUEST-OS-2.74_201805-01~~ |1 Haziran 2018'den |3 Ağustos 2018|TBD |
|~~WA-GUEST-OS-2.73_201804-01~~ |4 Mayıs 2018 |3 Temmuz 2018 |TBD |
|~~WA-GUEST-OS-2.72_201803-01~~ |6 Nisan 2018 |1 Haziran 2018'den |TBD |
|~~WA-GUEST-OS-2.71_201802-01~~ |12 Mart 2018 |4 Mayıs 2018 |TBD |
|~~WA-GUEST-OS-2.70_201801-01~~ |29 Ocak 2018 |6 Nisan 2018 |TBD |
|~~WA-GUEST-OS-2.69_201712-01~~ |4 Ocak 2018 |12 Mart 2018 |TBD |
|~~WA-GUEST-OS-2.68_201711-01~~ |14 Aralık 2017 |29 Ocak 2018 |TBD |
|~~WA-GUEST-OS-2.67_201710-02~~ |8 Kasım 2017 |4 Ocak 2018 |TBD |
|~~WA-GUEST-OS-2.66_201709-01~~ |6 Ekim 2017 |14 Aralık 2017 |TBD |
|~~WA-GUEST-OS-2.65_201708-01~~ |24 Ağustos 2017 |14 Aralık 2017 |TBD |


## <a name="msrc-patch-updates"></a>MSRC düzeltme eki güncelleştirmeleri
Aylık her konuk işletim sistemi sürüm ile birlikte gelen düzeltme eklerinin listesini kullanılabilir [burada][patches].

## <a name="sdk-support"></a>SDK desteği
Olsa da [Azure SDK'sı için kullanımdan kaldırma İlkesi] [ retire policy sdk] sürümleri 2.2 yukarıda yalnızca desteklenen, belirli konuk işletim sistemi ailelerini önceki sürümlerinde kullanmanıza izin gösterir. Her zaman en son desteklenen SDK kullanmanız gerekir.

| Konuk işletim sistemi ailesi | Uyumlu SDK sürümleri |
| --- | --- |
| 5 |Sürüm 2.9.5.1+ |
| 4 |2.1 + sürümü |
| 3 |Sürüm 1.8 + |
| 2 |1.3 + sürümü |
| 1 |Sürüm 1.0 + |

## <a name="guest-os-release-information"></a>Konuk işletim sistemi sürüm bilgileri
Konuk işletim sistemi sürümleri için önemli olan üç tarihleri vardır: **yayın** tarihi **devre dışı** tarihi ve **sona erme** tarih. Bu portalda ve konuk işletim sistemi hedefi olarak seçilebilir bir konuk işletim sistemi kullanılabilir olarak kabul edilir. Bir konuk işletim sistemi ulaştığında **devre dışı** tarih, Azure'dan kaldırılır. Ancak, bu konuk işletim sistemi hedefleyen herhangi bir bulut hizmeti hala normal şekilde çalışır.

Pencerenin arasında **devre dışı** tarih ve **sona erme** tarih sağlar bir arabellek ile kolayca geçiş daha yeni bir bir konuk işletim sisteminden. Kullanıyorsanız *otomatik* , konuk işletim sistemi her zaman en son sürümde başlayabilir ve süresinin dolmasını bu konuda endişelenmeniz gerekmez.

Zaman **sona erme** tarihini geçerse, o konuk işletim sistemi kullanmaya devam durduruldu, silinir ya yükseltmek zorunda herhangi bir bulut hizmeti. Daha fazla kullanımdan kaldırma İlkesi hakkında [burada][retirepolicy].

## <a name="guest-os-family-version-explanation"></a>Konuk işletim sistemi ailesi sürüm açıklaması
Konuk işletim sistemi ailesi, yayımlanan Microsoft Windows Server sürümlerinde temel alır. Konuk işletim sistemi Azure Cloud Services'ın üzerinde çalıştığı temel işletim sistemi ' dir. Her konuk işletim sistemi ailesi, sürüm ve sürüm olan sayı.

* **Konuk işletim sistemi ailesi**  
  Bir konuk işletim sistemi temel alan bir Windows Server işletim sistemi sürümü. Örneğin, *aile 3* Windows Server 2012'de bağlıdır.
* **Konuk işletim sistemi sürümü**  
  Belirli bir konuk işletim sistemi ailesi görüntüsüne yanı sıra ilgili [Microsoft Güvenlik Yanıt Merkezi (MSRC)] [ msrc] yeni konuk işletim sistemi sürümü oluşturulur tarihte kullanılabilir olan düzeltme eklerinin. Tüm düzeltme eklerini dahil edilebilir.

    Sayı 0'da başlar ve 1 ile yeni bir güncelleştirme kümesi eklenen her zaman artırın. Sondaki sıfırlar yalnızca gösterilen önemli değilse. Diğer bir deyişle, 2.10 sürüm 2.1 farklı, daha sonraki bir sürümden sürümüdür.
* **Konuk işletim sistemi sürüm**  
  Bir konuk işletim sistemi sürümü sürümündeki. Microsoft, test sırasında sorunları bulursa, bir yeniden yayımlama oluşur; değişiklik gerektirmeden. En son sürümü her zaman herhangi bir önceki yerini alır, veya genel serbest bırakır. Azure portalı, yalnızca belirli bir sürümü için en son sürümünü seçmek kullanıcılar izin verir. Önceki bir sürümü üzerinde çalışan dağıtımları genellikle hatanın önem derecesine bağlı olarak yükseltme zorla değildir.

Aşağıdaki örnekte, 2 seridir, 12 sürümüdür ve "rel2" sürümüdür.

**Konuk işletim sistemi sürüm** - 2,12 rel2

**Bu sürüm için yapılandırma dizesi** -WA-GUEST-OS-2.12_201208-02

Bu bilgiyi, hangi MSRC düzeltme ekleri için söz konusu sürümden dikkate gösteren bir tarih birlikte katıştırılmış bir konuk işletim sistemi için yapılandırma dizesi var. Bu örnekte, Windows Server 2008 R2 için en fazla üretilen ve Ağustos 2012 dahil olmak üzere MSRC düzeltme eki ekleme için ele alındı. Yalnızca özel olarak Windows Server'ın bu sürümü için uygulama düzeltme ekleri dahil edilir. MSRC düzeltme eki Microsoft Office için geçerliyse, bu ürün Windows Server temel görüntü parçası olmadığından, bu dahil edilmez.

## <a name="guest-os-system-update-process"></a>Konuk işletim sistemi güncelleştirme işlemi
Bu sayfa yakında konuk işletim sistemi sürümleri hakkında bilgi içerir. Müşteriler, "Otomatik" Güncelleştirme ayarlarsanız bulut hizmeti rollerinin yeniden çünkü bir sürüm ne zaman gerçekleştiğini öğrenmek istedikleri belirttiniz. Konuk işletim sistemi sürümleri, genellikle en az beş (5) MSRC güncelleştirme her ayın ikinci Salı günü oluşan sürüm gün sonra oluşur. Yeni yayınlar her konuk işletim sistemi ailesi için tüm ilgili MSRC düzeltme içerir.

Microsoft Azure, sürekli güncelleştirmeler yayımlamaktadır. Konuk işletim sistemi gibi işlem hattında yalnızca bir güncelleme var. Bir yayın birçok faktörler tarafından etkilenebilir burada listelemek için çok fazla sayıda. Ayrıca, Azure tam anlamıyla yüz binlerce makineler üzerinde çalışır. Başka bir deyişle, tam tarihi ve saati, rollere ne zaman yeniden vermek mümkün değildir. Sınırlamak veya yeniden başlatma süresi için bir plan üzerinde çalışıyoruz.

Konuk işletim Sisteminin yeni bir sürümü yayımlandığında, Azure'da tamamen yayılması zaman alabilir. Yeni konuk işletim sistemi için hizmet olarak bunlar güncelleştirme etki alanına göre yeniden başlatılır. "Otomatik" güncelleştirmeleri kullanacak şekilde Hizmetleri bir yayın ilk alırsınız. Güncelleştirme tamamlandıktan sonra hizmetinizin Azure portalında listelenen yeni konuk işletim sistemi sürümü görürsünüz. Yeniden yayımlama, bu süre boyunca ortaya çıkabilir. Bazı sürümler uzun süreler boyunca dağıtılabilir ve Otomatik yükseltme yeniden başlatma resmi sürüm tarihinden sonra birçok hafta boyunca gerçekleşmeyebilir. Bir konuk işletim sistemi kullanılabilir duruma geldikten sonra daha sonra açıkça bu sürümü portalından veya yapılandırma dosyanızdan seçebilirsiniz.

Yeniden başlatılır ve Konuk ve konak işletim sistemi güncelleştirmeleri hakkında daha fazla bilgi teknik ayrıntı işaretçileri değerli bilgilere aşırı için post başlıklı MSDN gönderisine bakın [rol örneği yeniden nedeniyle işletim sistemi yükseltmelerini] [ restarts].

El ile konuk işletim sistemi güncelleştirme olup [konuk işletim sistemi kullanımdan kaldırma İlkesi] [ retirepolicy] ek bilgi için.

## <a name="guest-os-supportability-and-retirement-policy"></a>Konuk işletim sistemi Desteklenebilirliği ve kullanımdan kaldırma İlkesi
Konuk işletim sistemi desteklenebilirliği ve kullanımdan kaldırma İlkesi açıklanan [burada][retirepolicy].

[cloud updates]: https://docs.microsoft.com/azure/cloud-services/cloud-services-update-azure-service
[Konuk işletim sistemi güncelleştirme RSS akışı]: https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/master/GuestOS/GuestOSFeed.xml
[Install .NET on a Cloud Service Role]: https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-install-dotnet/?WT.mc_id=azurebg_email_Trans_963_RevisedNET_Update
[Azure Guest OS Update Settings]: cloud-services-how-to-configure-portal.md
[ssl3 announcement]: http://azure.microsoft.com/blog/2014/12/09/azure-security-ssl-3-0-update/
[Microsoft Security Advisory 3009008]: https://technet.microsoft.com/library/security/3009008.aspx
[ssl3-fixit]: http://go.microsoft.com/?linkid=9863266
[MS14-066]: https://technet.microsoft.com/library/security/ms14-066.aspx
[MS14-046]: https://technet.microsoft.com/library/security/ms14-046.aspx
[retire policy sdk]: https://msdn.microsoft.com/library/dn479282.aspx
[server and gos]: https://msdn.microsoft.com/library/dn775043.aspx
[azuresupport]: http://azure.microsoft.com/support/options/
[net install pkg]: http://www.microsoft.com/download/details.aspx?id=42643
[msrc]: https://technet.microsoft.com/security/dn440717.aspx
[update guest os portal]: https://msdn.microsoft.com/library/gg433101.aspx
[update guest os svc]: https://msdn.microsoft.com/library/gg456324.aspx
[restarts]: http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx
[patches]: cloud-services-guestos-msrc-releases.md
[retirepolicy]: cloud-services-guestos-retirement-policy.md
[fam1retire]: cloud-services-guestos-family1-retirement.md
[fix]: https://technet.microsoft.com/library/security/ms17-010.aspx
