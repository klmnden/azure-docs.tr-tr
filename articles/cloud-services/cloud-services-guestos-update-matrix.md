---
title: En son Azure konuk işletim sistemi sürümleri hakkında bilgi edinin | Microsoft Docs
description: En son sürüm Haberler ve SDK uyumluluk Azure bulut Hizmetleri konuk işletim sistemi için.
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
ms.date: 4/6/2018
ms.author: raiye
ms.openlocfilehash: 1f24db331b3d59eaad54c5c2488e56913261cff2
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="azure-guest-os-releases-and-sdk-compatibility-matrix"></a>Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi
En son Azure konuk işletim sistemi hakkında güncel bilgiler için bulut hizmetlerini yayımları sağlar. Bu bilgiler, bir konuk işletim sistemi devre dışı önce yükseltme yolunuza planlamanıza yardımcı olur. Rollerinizi kullanacak şekilde yapılandırırsanız, *otomatik* konuk işletim sistemi güncelleştirmeleri açıklandığı gibi [Azure konuk işletim sistemi güncelleştirme ayarları][Azure Guest OS Update Settings], bu sayfayı okuyun önemli değildir.

> [!IMPORTANT]
> Bu sayfa bir konuk işletim sistemi üzerinde çalışan bulut Hizmetleri web ve çalışan rolleri için geçerlidir. Mevcut **geçerli** Iaas sanal makineleri için.
>
>


> [!TIP]
>  Abone [konuk işletim sistemi güncelleştirme RSS akışı] tüm konuk işletim sistemi değişikliklerle ilgili en güncel bildirim almak için.
>
>

> [!IMPORTANT]
> Kasım sunum başlayarak, konuk işletim sistemi yalnızca son 2 sürümlerinde desteklenen ve Azure portalında kullanılabilir olacaktır.
>
>

Hangi, bir konuk işletim sistemi değilseniz ya da konuk işletim sistemi iş nasıl Yayımları? Okuma [bu](#how-it-works) bölümü.

## <a name="news-updates"></a>Haber güncelleştirmeleri
###### <a name="april-6-2018"></a>**6 Nisan 2018**
Mart konuk işletim sistemi yayımladı.

###### <a name="march-19-2018"></a>**19 Mart 2018**
Şubat konuk işletim sistemi yayımladı.

###### <a name="january-29-2018"></a>**29 Ocak 2018**
Ocak konuk işletim sistemi için işletim sistemi ailesi 2 serbest (WA-GUEST-işletim sistemi-2.70_201801-01) & 3 (WA-GUEST-OS-3.57_201801-01)

###### <a name="january-4-2018"></a>**4 Ocak 2018**
Ocak konuk işletim sistemi için işletim sistemi aileleri 4 yayımlanan (WA-GUEST-OS-4.50_201801-01) & 5 (WA-KONUK-işletim sistemi-5.15_201801-01) ve önemli güvenlik yamaları içerir.  

###### <a name="january-4-2018"></a>**4 Ocak 2018**
Aralık konuk işletim sistemi yayımladı.

###### <a name="december-14-2017"></a>**14 Aralık 2017**
Kasım konuk işletim sistemi yayımladı.

###### <a name="november-8-2017"></a>**8 Kasım 2017**
Ekim konuk işletim sistemi yayımladı.

###### <a name="october-6-2017"></a>**6 Ekim 2017**
Eylül konuk işletim sistemi yayımladı. Windows Server 2016 Eylül sürüm için netfx3 varsayılan olarak etkindir. Müşteriler Ekle ' dism / online Feature /featurename:netfx3' kendi iş akışının .NET 2.x uygulaması 4.x çalışma zamanı ile çalışacak biçimde gerektirir veya .NET 2.x uygulaması çalıştırdıysanız hata işlenmiş ve .NET 4.x uygulaması çalıştıran kendi ONSTART.

###### <a name="september-14-2017"></a>**14 Eylül 2017**
Eylül konuk işletim sistemi dağıtımı 14 Eylül başlatıyor ve Ekim 9 Tahmini sürümü vardır.

###### <a name="august-24-2017"></a>**24 Ağustos 2017**
Ağustos konuk işletim sistemi yayımladı.

###### <a name="august-3-2017"></a>**3 Ağustos 2017**
Temmuz konuk işletim sistemi yayımladı.

###### <a name="july-19-2017"></a>**19 Temmuz 2017**
Temmuz konuk işletim sistemi dağıtımı Temmuz 19 başlatıyor ve 8 Ağustos tahmini sürümü vardır.


## <a name="releases"></a>Sürümleri
## <a name="family-5-releases"></a>Aile 5 sürümleri
**Windows Server 2016**

.NET framework yüklü: 4.0, 4.5, 4.5.1, 4.5.2, 4.6, 4.6.1, 4.6.2

> [!NOTE]
> İle tarihleri bir * olan değiştirilebilir.
>
> İşletim sistemi ailesi 5 RDP parolası, en az 10 karakter uzunluğunda olmalıdır.
>

| Yapılandırma dizesi | Sürüm tarihi | Tarih devre dışı bırak | Süresi dolmuş tarih |
| --- | --- | --- | --- |
| WA-GUEST-OS-5.17_201803-01 |6 Nisan 2018 |POST 5.19 |TBD |
| WA-GUEST-OS-5.16_201802-01 |12 Mart 2018 |POST 5.18 |TBD |
|~~WA-GUEST-OS-5.15_201801-01~~ |4 Ocak 2018 |6 Nisan 2018 |TBD |
|~~WA-GUEST-OS-5.14_201712-01~~ |4 Ocak 2018 |12 Mart 2018 |TBD |
|~~WA-GUEST-OS-5.13_201711-01~~ |14 Aralık 2017 |4 Ocak 2018|TBD |
|~~WA-GUEST-OS-5.12_201710-02~~ |8 Kasım 2017 |4 Ocak 2018 |TBD |
|~~WA-GUEST-OS-5.11_201709-01~~ |6 Ekim 2017 |14 Aralık 2017 |TBD |
|~~WA-GUEST-OS-5.10_201708-01~~ |24 Ağustos 2017 |14 Aralık 2017 |TBD |
|~~WA-GUEST-OS-5.9_201707-01~~ |3 Ağustos 2017 |8 Kasım 2017 |TBD |
|~~WA-GUEST-OS-5.8_201706-01~~ |7 Temmuz 2017 |6 Ekim 2017 |TBD |
|~~WA-GUEST-OS-5.7_201705-01~~ |5 Haziran 2017 |24 Ağustos 2017 |TBD |
|~~WA-GUEST-OS-5.6_201704-01~~ |9 May 2017 |3 Ağustos 2017 |TBD |
|~~WA-GUEST-OS-5.5_201703-01~~ |10 Nisan 2017 |7 Temmuz 2017 |TBD |
|~~WA-GUEST-OS-5.4_201612-01~~ |10 Ocak 2017 |5 Haziran 2017|TBD |

## <a name="family-4-releases"></a>Aile 4 serbest bırakır
**Windows Server 2012 R2**

.NET framework yüklü: 4.0, 4.5, 4.5.1, 4.5.2

> [!NOTE]
> İle tarihleri bir * olan değiştirilebilir
>
>

| Yapılandırma dizesi | Sürüm tarihi | Tarih devre dışı bırak | Süresi dolmuş tarih |
| --- | --- | --- | --- |
| WA-GUEST-OS-4.52_201803-01 |6 Nisan 2018 |POST 4.54 |TBD |
| WA-GUEST-OS-4.51_201802-01 |12 Mart 2018 |POST 4.53 |TBD |
|~~WA-GUEST-OS-4.50_201801-01~~ |4 Ocak 2018 |6 Nisan 2018 |TBD |
|~~WA-GUEST-OS-4.49_201712-01~~ |4 Ocak 2018 |12 Mart 2018 |TBD |
|~~WA-GUEST-OS-4.48_201711-01~~ |14 Aralık 2017 |4 Ocak 2018 |TBD |
|~~WA-GUEST-OS-4.47_201710-02~~ |8 Kasım 2017 |4 Ocak 2018 |TBD |
|~~WA-GUEST-OS-4.46_201709-01~~ |6 Ekim 2017 |14 Aralık 2017 |TBD |
|~~WA-GUEST-OS-4.45_201708-01~~ |24 Ağustos 2017 |14 Aralık 2017 |TBD |
|~~WA-GUEST-OS-4.44_201707-01~~ |3 Ağustos 2017 |8 Kasım 2017 |TBD |
|~~WA-GUEST-OS-4.43_201706-01~~ |7 Temmuz 2017 |6 Ekim 2017 |TBD |
|~~WA-GUEST-OS-4.42_201705-01~~ |5 Haziran 2017 |24 Ağustos 2017 |TBD |
|~~WA-GUEST-OS-4.41_201704-01~~ |9 May 2017 |3 Ağustos 2017 |TBD |
|~~WA-GUEST-OS-4.40_201703-01~~ |10 Nisan 2017 |7 Temmuz 2017 |TBD |
|~~WA-GUEST-OS-4.39_201612-01~~ |10 Ocak 2017 |5 Haziran 2017 |TBD |

## <a name="family-3-releases"></a>Aile 3 serbest bırakır
**Windows Server 2012**

.NET framework yüklü: 4.0, 4.5, 4.5.1, 4.5.2

> [!NOTE]
> İle tarihleri bir * olan değiştirilebilir
>
>

| Yapılandırma dizesi | Sürüm tarihi | Tarih devre dışı bırak | Süresi dolmuş tarih |
| --- | --- | --- | --- |
| WA-GUEST-OS-3.59_201803-01 |6 Nisan 2018 |POST 3.61 |TBD |
| WA-GUEST-OS-3.58_201802-01 |19 Mart 2018 |POST 3.60 |TBD |
|~~WA-GUEST-OS-3.57_201801-01~~ |29 Ocak 2018 |6 Nisan 2018 |TBD |
|~~WA-GUEST-OS-3.56_201712-01~~ |4 Ocak 2018 |19 Mart 2018 |TBD |
|~~WA-GUEST-OS-3.55_201711-01~~ |14 Aralık 2017 |29 Ocak 2018 |TBD |
|~~WA-GUEST-OS-3.54_201710-02~~ |8 Kasım 2017 |4 Ocak 2018 |TBD |
|~~WA-GUEST-OS-3.53_201709-01~~ |6 Ekim 2017 |14 Aralık 2017 |TBD |
|~~WA-GUEST-OS-3.52_201708-01~~ |24 Ağustos 2017 |14 Aralık 2017 |TBD |
|~~WA-GUEST-OS-3.51_201707-01~~ |3 Ağustos 2017 |8 Kasım 2017 |TBD |
|~~WA-GUEST-OS-3.50_201706-01~~ |7 Temmuz 2017 |6 Ekim 2017 |TBD |
|~~WA-GUEST-OS-3.49_201705-01~~ |5 Haziran 2017 |24 Ağustos 2017 |TBD |
|~~WA-GUEST-OS-3.48_201704-01~~ |9 May 2017 |3 Ağustos 2017 |TBD |
|~~WA-GUEST-OS-3.47_201703-01~~ |10 Nisan 2017 |7 Temmuz 2017 |TBD |
|~~WA-GUEST-OS-3.46_201612-01~~ |10 Ocak 2017 |5 Haziran 2017 |TBD |

## <a name="family-2-releases"></a>Ailesi 2 sürümleri
**Windows Server 2008 R2 SP1**

.NET framework yüklü: 3.5, 4.0, 4.5, 4.5.1, 4.5.2

> [!NOTE]
> İle tarihleri bir * olan değiştirilebilir
>
>

| Yapılandırma dizesi | Sürüm tarihi | Tarih devre dışı bırak | Süresi dolmuş tarih |
| --- | --- | --- | --- |
| WA-GUEST-OS-2.72_201803-01 |6 Nisan 2018 |POST 2.74 |TBD |
| WA-GUEST-OS-2.71_201802-01 |12 Mart 2018 |POST 2,73 |TBD |
|~~WA-GUEST-OS-2.70_201801-01~~ |29 Ocak 2018 |6 Nisan 2018 |TBD |
|~~WA-GUEST-OS-2.69_201712-01~~ |4 Ocak 2018 |12 Mart 2018 |TBD |
|~~WA-GUEST-OS-2.68_201711-01~~ |14 Aralık 2017 |29 Ocak 2018 |TBD |
|~~WA-GUEST-OS-2.67_201710-02~~ |8 Kasım 2017 |4 Ocak 2018 |TBD |
|~~WA-GUEST-OS-2.66_201709-01~~ |6 Ekim 2017 |14 Aralık 2017 |TBD |
|~~WA-GUEST-OS-2.65_201708-01~~ |24 Ağustos 2017 |14 Aralık 2017 |TBD |
|~~WA-GUEST-OS-2.64_201707-01~~ |3 Ağustos 2017 |8 Kasım 2017 |TBD |
|~~WA-GUEST-OS-2.63_201706-01~~ |7 Temmuz 2017 |6 Ekim 2017 |TBD |
|~~WA-GUEST-OS-2.62_201705-01~~ |5 Haziran 2017 |24 Ağustos 2017 |TBD |
|~~WA-GUEST-OS-2.61_201704-01~~ |9 May 2017 |3 Ağustos 2017 |TBD |
|~~WA-GUEST-OS-2.60_201703-01~~ |10 Nisan 2017 |7 Temmuz 2017 |TBD |
|~~WA-GUEST-OS-2.59_201701-01~~ |10 Ocak 2017 |5 Haziran 2017 |TBD |
|~~WA-GUEST-OS-2.58_201612-01~~ |10 Ocak 2017 |9 May 2017|TBD |


## <a name="msrc-patch-updates"></a>MSRC düzeltme eki güncelleştirmeleri
Aylık her konuk işletim sistemi sürüm ile birlikte gelen düzeltme eklerinin listesini kullanılabilir [burada][patches].

## <a name="sdk-support"></a>SDK'sı desteği
Olsa bile [Azure SDK'sı için devre dışı bırakma İlkesi] [ retire policy sdk] 2.2 yukarıda sürümleri yalnızca desteklenen, belirli konuk işletim sistemi aileleri önceki sürümlerinde kullanmanıza izin gösterir. Her zaman en son desteklenen SDK kullanmanız gerekir.

| Konuk işletim sistemi ailesi | Uyumlu SDK sürümleri |
| --- | --- |
| 5 |Sürüm 2.9.5.1+ |
| 4 |Sürüm 2.1 + |
| 3 |Sürüm 1,8 + |
| 2 |Sürüm 1.3 + |
| 1 |Sürüm 1.0 + |

## <a name="guest-os-release-information"></a>Konuk işletim sistemi sürüm bilgileri
Konuk işletim sistemi sürümleri için önemli olan üç tarihleri vardır: **yayın** tarih, **devre dışı** tarihi ve **sona erme** tarih. Portalda olduğunda ve konuk işletim sistemi hedefi olarak seçilen bir konuk işletim sistemi kullanılabilir olarak kabul edilir. Bir konuk işletim sistemi ulaştığında **devre dışı** tarih, Azure'dan kaldırılır. Ancak, bu konuk işletim sistemi hedefleme herhangi bir bulut hizmeti hala normal olarak çalışır.

Pencerenin arasında **devre dışı** tarih ve **sona erme** tarih sağlar, bir arabellekle kolayca geçiş için bir konuk işletim sistemi daha yeni bir tane gelen. Kullanıyorsanız, *otomatik* , konuk işletim sistemi her zaman son sürümü olacak ve zaman aşımına uğramak onu hakkında endişelenmeniz gerekmez.

Zaman **sona erme** tarihini geçerse, bu konuk işletim sistemi kullanmaya devam durduruldu, silinmesi veya yükseltmek için zorunlu herhangi bir bulut hizmeti. Daha fazla bilgiyi kullanımdan kaldırma ilkesiyle ilgili [burada][retirepolicy].

## <a name="guest-os-family-version-explanation"></a>Konuk işletim sistemi ailesi sürümlü açıklama
Konuk işletim sistemi aileleri yayımlanan Microsoft Windows Server sürümlerinde temel alır. Konuk işletim sistemi Azure Cloud Services üzerinde çalışan temel işletim sistemini ' dir. Her konuk işletim sistemi ailesi, sürüm ve sürüm olan sayı.

* **Konuk işletim sistemi ailesi**  
  Bir Windows Server işletim sistemi sürüm konuk işletim sistemi dayanır. Örneğin, *aile 3* Windows Server 2012'de temel alır.
* **Konuk işletim sistemi sürümü**  
  Konuk işletim sistemi ailesi görüntüsüne belirli artı ilgili [Microsoft Güvenlik Yanıt Merkezi (MSRC)] [ msrc] yeni konuk işletim sistemi sürümü üretileceğini tarihte kullanılabilir düzeltme ekleri. Tüm düzeltme eklerini dahil edilebilir.

    Sayı 0'da başlatın ve güncelleştirmeleri yeni bir dizi eklenen her zaman 1 ile artırın. Sondaki sıfırlar yalnızca gösterilen önemli değilse. Diğer bir deyişle, 2.10 sürüm 2.1 farklı, daha sonraki bir sürümden sürümüdür.
* **Konuk işletim sistemi sürümü**  
  Bir konuk işletim sistemi sürümü sürümündeki. Microsoft Test sırasında sorunları bulursa bir yeniden yayımlama oluşur; değişiklikleri gerektirir. En son sürümü her zaman herhangi önceki yerini alır, veya ortak serbest bırakır. Azure portal, yalnızca belirli bir sürümü için en son sürümü almak kullanıcılara izin verir. Dağıtımları önceki bir sürümü çalıştıran genellikle hata önem derecesi bağlı olarak yükseltme zorla değildir.

Aşağıdaki örnekte, 2 ailesi, sürüm 12 ise ve "rel2" sürüm.

**Konuk işletim sistemi sürüm** - 2,12 rel2

**Bu sürüm için yapılandırma dizesi** -WA-GUEST-OS-2.12_201208-02

Bir konuk işletim sistemi için yapılandırma dizesi, bu sürüm için hangi MSRC düzeltme ekleri olarak kabul gösteren bir tarih ile birlikte katıştırılmış aynı bilgiler vardır. Bu örnekte, Windows Server 2008 R2 için en fazla üretilen ve Ağustos 2012 dahil olmak üzere MSRC düzeltme ekleri için içerme ele alındı. Yalnızca özel olarak Windows Server'ın bu sürümü için uygulama düzeltme ekleri dahil edilir. Microsoft Office MSRC düzeltme eki uygular, bu ürün Windows Server temel görüntü parçası olmadığından Örneğin, bunu dahil edilmez.

## <a name="guest-os-system-update-process"></a>Konuk işletim sistemi sistem güncelleştirme işlemi
Bu sayfa yakında konuk işletim sistemi sürümleri hakkında bilgi içerir. Müşteriler için "Otomatik" Güncelleştirme ayarlarsanız bulut hizmeti rollerinin yeniden çünkü bir yayın meydana geldiğinde bilmek isteriz belirttiniz. Konuk işletim sistemi sürümleri genellikle en az beş (5) MSRC güncelleştirdikten sonra her ayın ikinci Salı günü ortaya çıkan sürüm günde bir yapılır. Yeni sürümler tüm ilgili MSRC düzeltme eklerinin her konuk işletim sistemi ailesi için içerir.

Microsoft Azure sürekli güncelleştirmeleri yayımladı. Konuk işletim sistemi yalnızca bir tür ardışık düzeninde güncelleştirmesidir. Bir yayın birçok faktörler tarafından etkilenebilir burada listelemek için fazladır. Ayrıca, Azure tam anlamıyla yüz binlerce makineler üzerinde çalışır. Başka bir deyişle, bir tam tarihi ve saati, rollere ne zaman yeniden vermek mümkün değildir. Sınırlamak veya yeniden başlatmalar saat için bir plan üzerinde çalışıyoruz.

Konuk işletim sisteminin yeni bir sürüm yayımlandığında, tam olarak Azure yayılması zaman alabilir. Hizmetleri, yeni konuk işletim sistemine güncelleştirilir gibi bunlar güncelleştirme etki alanları uygularken yeniden başlatılır. "Otomatik" güncelleştirmeleri kullanacak şekilde Hizmetleri bir yayın ilk alır. Güncelleştirme tamamlandıktan sonra Azure portalında hizmetiniz için listelenen yeni konuk işletim sistemi sürümü görürsünüz. Yeniden yayımlama, bu süre zarfında ortaya çıkabilir. Bazı sürümler uzun süreler boyunca dağıtılabilir ve Otomatik yükseltme yeniden başlatmalar için birçok hafta sonra resmi yayın tarihi gerçekleşmeyebilir. Bir konuk işletim sistemi kullanılabilir olduğunda, daha sonra açıkça sürümünün portalından veya yapılandırma dosyanızda seçebilirsiniz.

Yeniden başlatılır ve daha fazla bilgi teknik ayrıntılar Konuk ve ana bilgisayar işletim sistemi güncelleştirmelerinin işaretçiler hakkında değerli bilgiler içeren büyük bir bölümünü post başlıklı MSDN Web günlüğü postasına bakın [rol örneği yeniden son işletim sistemi yükseltmeleri için][restarts].

Konuk işletim sistemi el ile güncelleştirirseniz bkz [konuk işletim sistemi devre dışı bırakma İlkesi] [ retirepolicy] ek bilgi için.

## <a name="guest-os-supportability-and-retirement-policy"></a>Konuk işletim sistemi desteklenebilirlik ve kullanımdan kaldırma İlkesi
Konuk işletim sistemi desteklenebilirlik ve kullanımdan kaldırma İlkesi açıklandığı [burada][retirepolicy].

[konuk işletim sistemi güncelleştirme RSS akışı]: https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/master/GuestOS/GuestOSFeed.xml
[Install .NET on a Cloud Service Role]: https://azure.microsoft.com/en-us/documentation/articles/cloud-services-dotnet-install-dotnet/?WT.mc_id=azurebg_email_Trans_963_RevisedNET_Update
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
[msrc]: http://www.microsoft.com/security/msrc/default.aspx
[update guest os portal]: https://msdn.microsoft.com/library/gg433101.aspx
[update guest os svc]: https://msdn.microsoft.com/library/gg456324.aspx
[restarts]: http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx
[patches]: cloud-services-guestos-msrc-releases.md
[retirepolicy]: cloud-services-guestos-retirement-policy.md
[fam1retire]: cloud-services-guestos-family1-retirement.md
[fix]: https://technet.microsoft.com/en-us/library/security/ms17-010.aspx
