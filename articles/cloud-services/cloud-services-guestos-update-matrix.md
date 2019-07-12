---
title: En son Azure konuk işletim sistemi sürümleri hakkında bilgi edinin | Microsoft Docs
description: Son sürüm haberleri ve SDK uyumluluk Azure bulut Hizmetleri konuk işletim sistemi için.
services: cloud-services
documentationcenter: na
author: raiye
editor: ''
ms.assetid: 6306cafe-1153-44c7-8554-623b03d59a34
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 7/8/2019
ms.author: raiye
ms.openlocfilehash: 88c3cd0e07e207a8b5ae1c07d39c8829a531c743
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67721128"
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
> Konuk işletim Sisteminin en son 2 sürümleri, desteklenen ve Azure portalında kullanılabilir olacaktır.
>
>

Konuk işletim sisteminizi güncelleştirin konusunda emin değilseniz? Denetleme [bu][cloud updates] uğradı.

## <a name="news-updates"></a>Haber güncelleştirmeleri

###### <a name="july-8-2019"></a>**8 Temmuz 2019**
Haziran konuk işletim sistemi kullanıma sundu.

###### <a name="june-6-2019"></a>**6 Haziran 2019**
Olabilir konuk işletim sistemi kullanıma sundu.

###### <a name="may-7-2019"></a>**7 Mayıs 2019**
Nisan konuk işletim sistemi kullanıma sundu.

###### <a name="march-26-2019"></a>**26 Mart 2019**
Mart konuk işletim sistemi kullanıma sundu.

###### <a name="march-12-2019"></a>**12 Mart 2019**
Şubat konuk işletim sistemi kullanıma sundu.

###### <a name="february-5-2019"></a>**5 Şubat 2019**
Ocak konuk işletim sistemi kullanıma sundu.

###### <a name="january-24-2019"></a>**24 Ocak 2019**
Aile 6 konuk işletim sistemi (Windows Server 2019) kullanıma sundu.

###### <a name="january-7-2019"></a>**7 Ocak 2019**
Aralık konuk işletim sistemi kullanıma sundu.

###### <a name="december-14-2018"></a>**14 Aralık 2018'e**
Kasım konuk işletim sistemi kullanıma sundu.

###### <a name="november-8-2018"></a>**8 Kasım 2018**
Ekim konuk işletim sistemi kullanıma sundu.

###### <a name="october-12-2018"></a>**12 Ekim 2018**
Eylül konuk işletim sistemi kullanıma sundu.

## <a name="releases"></a>Yayınlar

## <a name="family-6-releases"></a>Aile 6 yayınlar
**Windows Server 2019**

.NET framework yüklü: 3.5, 4.7.2, 4.8

> [!NOTE]
> Windows Azure SDK - .NET 3.0 indirilebilir [burada][Windows Azure SDK].
>
>Yükleme adımları:
>1. Lütfen MicrosoftAzureAuthoringTools*.msi eski sürümlerini kaldırın.
>2. Yükleme [.NET - 3.0 için Azure SDK][Windows Azure SDK]
>3. Bilgisayarınızı yeniden başlatın
>4. Yeni bir bulut hizmeti projesi oluşturun ve tek bir çalışan rolü Ekle
>5. İşletim sistemi ailesi 6 olarak değiştirin ve bir paket oluşturun
>6. Paketi, Visual Studio ve Azure portalını kullanarak Azure'a dağıtma
>


| Yapılandırma dizesi | Sürüm tarihi | Tarih devre dışı bırak |
| --- | --- | --- |
| WA-GUEST-OS-6.8_201906-01 |8 Temmuz 2019 |POST 6.10 |
| WA-GUEST-OS-6.7_201905-01 |6 Haziran 2019 |POST 6.9 |
|~~WA-GUEST-OS-6.6_201904-01~~ |7 Mayıs 2019 |8 Temmuz 2019 |
|~~WA-GUEST-OS-6.5_201903-01~~ |26 Mart 2019 |6 Haziran 2019 |
|~~WA-GUEST-OS-6.4_201902-01~~ |12 Mart 2019 |7 Mayıs 2019 |
|~~WA-GUEST-OS-6.3_201901-01~~ |5 Şubat 2019 |26 Mart 2019 |
|~~WA-GUEST-OS-6.2_201812-01~~ |24 Ocak 2019 |12 Mart 2019 |
|~~WA-GUEST-OS-6.1_201811-01~~ |24 Ocak 2019 |5 Şubat 2019 |

## <a name="family-5-releases"></a>Ailesi 5 yayınlar
**Windows Server 2016**

.NET framework yüklü: 3.5, 4.6.2, 4.7.2, 4.8

> [!NOTE]
> İşletim sistemi ailesi 5 RDP parolasını en az 10 karakter uzunluğunda olmalıdır.
>


| Yapılandırma dizesi | Sürüm tarihi | Tarih devre dışı bırak |
| --- | --- | --- |
| WA-GUEST-OS-5.32_201906-01 |8 Temmuz 2019 |POST 5.34 |
| WA-GUEST-OS-5.31_201905-01 |6 Haziran 2019 |POST 5.33 |
|~~WA-GUEST-OS-5.30_201904-01~~ |7 Mayıs 2019 |8 Temmuz 2019 |
|~~WA-GUEST-OS-5.29_201903-01~~ |26 Mart 2019 |6 Haziran 2019 |
|~~WA-GUEST-OS-5.28_201902-01~~ |12 Mart 2019 |7 Mayıs 2019 |
|~~WA-GUEST-OS-5.27_201901-01~~ |5 Şubat 2019 |26 Mart 2019 |
|~~WA-GUEST-OS-5.26_201812-01~~ |7 Ocak 2019 |12 Mart 2019 |
|~~WA-GUEST-OS-5.25_201811-01~~ |14 Aralık 2018'e |5 Şubat 2019 |
|~~WA-GUEST-OS-5.24_201810-01~~ |8 Kasım 2018 |7 Ocak 2019 |
|~~WA-GUEST-OS-5.23_201809-01~~ |12 Ekim 2018 |14 Aralık 2018'e |

## <a name="family-4-releases"></a>Ailesi 4 yayınlar
**Windows Server 2012 R2**

.NET framework yüklü: 3.5, 4.5.1, 4.5.2

| Yapılandırma dizesi | Sürüm tarihi | Tarih devre dışı bırak |
| --- | --- | --- |
| WA-GUEST-OS-4.67_201906-01 |8 Temmuz 2019 |POST 4.69 |
| WA-GUEST-OS-4.66_201905-01 |6 Haziran 2019 |POST 4.68 |
|~~WA-GUEST-OS-4.65_201904-01~~ |7 Mayıs 2019 |8 Temmuz 2019 |
|~~WA-GUEST-OS-4.64_201903-01~~ |26 Mart 2019 |6 Haziran 2019 |
|~~WA-GUEST-OS-4.63_201902-01~~ |12 Mart 2019 |7 Mayıs 2019 |
|~~WA-GUEST-OS-4.62_201901-01~~ |5 Şubat 2019 |26 Mart 2019 |
|~~WA-GUEST-OS-4.61_201812-01~~ |7 Ocak 2019 |12 Mart 2019 |
|~~WA-GUEST-OS-4.60_201811-01~~ |14 Aralık 2018'e |5 Şubat 2019 |
|~~WA-GUEST-OS-4.59_201810-01~~ |8 Kasım 2018 |7 Ocak 2019 |
|~~WA-GUEST-OS-4.58_201809-01~~ |12 Ekim 2018 |14 Aralık 2018'e |

## <a name="family-3-releases"></a>Aile 3 yayınlar
**Windows Server 2012**

.NET framework yüklü: 3.5, 4.5

| Yapılandırma dizesi | Sürüm tarihi | Tarih devre dışı bırak |
| --- | --- | --- |
| WA-GUEST-OS-3.74_201906-01 |8 Temmuz 2019 |POST 3.76 |
| WA-GUEST-OS-3.73_201905-01 |6 Haziran 2019 |POST 3,75 |
|~~WA-GUEST-OS-3.72_201904-01~~ |7 Mayıs 2019 |8 Temmuz 2019 |
|~~WA-GUEST-OS-3.71_201903-01~~ |26 Mart 2019 |6 Haziran 2019 |
|~~WA-GUEST-OS-3.70_201902-01~~ |12 Mart 2019 |7 Mayıs 2019 |
|~~WA-GUEST-OS-3.69_201901-01~~ |5 Şubat 2019 |26 Mart 2019 |
|~~WA-GUEST-OS-3.68_201812-01~~ |7 Ocak 2019 |12 Mart 2019 |
|~~WA-GUEST-OS-3.67_201811-01~~ |14 Aralık 2018'e |5 Şubat 2019 |
|~~WA-GUEST-OS-3.66_201810-01~~ |8 Kasım 2018 |7 Ocak 2019 |
|~~WA-GUEST-OS-3.65_201809-01~~ |12 Ekim 2018 |14 Aralık 2018'e |

## <a name="family-2-releases"></a>Ailesi 2 yayınlar
**Windows Server 2008 R2 SP1**

.NET framework yüklü: 3.5 (2.0 ve 3.0 içerir), 4.5

| Yapılandırma dizesi | Sürüm tarihi | Tarih devre dışı bırak |
| --- | --- | --- |
| WA-GUEST-OS-2.87_201906-01 |8 Temmuz 2019 |POST 2.89 |
| WA-GUEST-OS-2.86_201905-01 |6 Haziran 2019 |POST 2.88 |
|~~WA-GUEST-OS-2.85_201904-01~~ |7 Mayıs 2019 |8 Temmuz 2019 |
|~~WA-GUEST-OS-2.84_201903-01~~ |26 Mart 2019 |6 Haziran 2019 |
|~~WA-GUEST-OS-2.83_201902-01~~ |12 Mart 2019 |7 Mayıs 2019 |
|~~WA-GUEST-OS-2.82_201901-01~~ |5 Şubat 2019 |26 Mart 2019 |
|~~WA-GUEST-OS-2.81_201812-01~~ |7 Ocak 2019 |12 Mart 2019 |
|~~WA-GUEST-OS-2.80_201811-01~~ |14 Aralık 2018'e |5 Şubat 2019 |
|~~WA-GUEST-OS-2.79_201810-01~~ |8 Kasım 2018 |7 Ocak 2019 |
|~~WA-GUEST-OS-2.78_201809-01~~ |12 Ekim 2018 |14 Aralık 2018'e |

## <a name="msrc-patch-updates"></a>MSRC düzeltme eki güncelleştirmeleri
Aylık her konuk işletim sistemi sürüm ile birlikte gelen düzeltme eklerinin listesini kullanılabilir [burada][patches].

## <a name="sdk-support"></a>SDK desteği
Olsa da [Azure SDK'sı için kullanımdan kaldırma İlkesi][retire policy sdk] sürümleri 2.2 yukarıda yalnızca desteklenen, belirli konuk işletim sistemi ailelerini önceki sürümlerinde kullanmanıza izin gösterir. Her zaman en son desteklenen SDK kullanmanız gerekir.

| Konuk işletim sistemi ailesi | Uyumlu SDK sürümleri |
| --- | --- |
| 6 |Sürüm 2.9.6+ |
| 5 |Sürüm 2.9.5.1+ |
| 4 |2\.1 + sürümü |
| 3 |Sürüm 1.8 + |
| 2 |1\.3 + sürümü |
| 1\. |Sürüm 1.0 + |

## <a name="guest-os-release-information"></a>Konuk işletim sistemi sürüm bilgileri
Konuk işletim sistemi sürümleri için önemli olan üç tarihleri vardır: **yayın** tarihi **devre dışı** tarihi ve **sona erme** tarih. Bu portalda ve konuk işletim sistemi hedefi olarak seçilebilir bir konuk işletim sistemi kullanılabilir olarak kabul edilir. Bir konuk işletim sistemi ulaştığında **devre dışı** tarih, Azure'dan kaldırılır. Ancak, bu konuk işletim sistemi hedefleyen herhangi bir bulut hizmeti hala normal şekilde çalışır.

Pencerenin arasında **devre dışı** tarih ve **sona erme** tarih sağlar bir arabellek ile kolayca geçiş daha yeni bir bir konuk işletim sisteminden. Kullanıyorsanız *otomatik* , konuk işletim sistemi her zaman en son sürümde başlayabilir ve süresinin dolmasını bu konuda endişelenmeniz gerekmez.

Zaman **sona erme** tarihini geçerse, o konuk işletim sistemi kullanmaya devam durduruldu, silinir ya yükseltmek zorunda herhangi bir bulut hizmeti. Daha fazla kullanımdan kaldırma İlkesi hakkında [burada][retirepolicy].

## <a name="guest-os-family-version-explanation"></a>Konuk işletim sistemi ailesi sürüm açıklaması
Konuk işletim sistemi ailesi, yayımlanan Microsoft Windows Server sürümlerinde temel alır. Konuk işletim sistemi Azure Cloud Services'ın üzerinde çalıştığı temel işletim sistemi ' dir. Her konuk işletim sistemi ailesi, sürüm ve sürüm olan sayı.

* **Konuk işletim sistemi ailesi**  
  Bir konuk işletim sistemi temel alan bir Windows Server işletim sistemi sürümü. Örneğin, *aile 3* Windows Server 2012'de bağlıdır.
* **Konuk işletim sistemi sürümü**  
  Belirli bir konuk işletim sistemi ailesi görüntüsüne yanı sıra ilgili [Microsoft Güvenlik Yanıt Merkezi (MSRC)][msrc] yeni konuk işletim sistemi sürümü oluşturulur tarihte kullanılabilir olan düzeltme eklerinin. Tüm düzeltme eklerini dahil edilebilir.

    Sayı 0'da başlar ve 1 ile yeni bir güncelleştirme kümesi eklenen her zaman artırın. Sondaki sıfırlar yalnızca gösterilen önemli değilse. Diğer bir deyişle, 2.10 sürüm 2.1 farklı, daha sonraki bir sürümden sürümüdür.
* **Konuk işletim sistemi sürüm**  
  Bir konuk işletim sistemi sürümü sürümündeki. Microsoft, test sırasında sorunları bulursa, bir yeniden yayımlama oluşur; değişiklik gerektirmeden. En son sürümü her zaman herhangi bir önceki yerini alır, veya genel serbest bırakır. Azure portalı, yalnızca belirli bir sürümü için en son sürümünü seçmek kullanıcılar izin verir. Önceki bir sürümü üzerinde çalışan dağıtımları genellikle hatanın önem derecesine bağlı olarak yükseltme zorla değildir.

Aşağıdaki örnekte, 2 seridir, 12 sürümüdür ve "rel2" sürümüdür.

**Konuk işletim sistemi sürüm** - 2,12 rel2

**Bu sürüm için yapılandırma dizesi** -WA-GUEST-OS-2.12_201208-02

Bu bilgiyi, hangi MSRC düzeltme ekleri için söz konusu sürümden dikkate gösteren bir tarih birlikte katıştırılmış bir konuk işletim sistemi için yapılandırma dizesi var. Bu örnekte, Windows Server 2008 R2 için en fazla üretilen ve Ağustos 2012 dahil olmak üzere MSRC düzeltme eki ekleme için ele alındı. Yalnızca özel olarak Windows Server'ın bu sürümü için uygulama düzeltme ekleri dahil edilir. MSRC düzeltme eki Microsoft Office için geçerliyse, bu ürün Windows Server temel görüntü parçası olmadığından, bu dahil edilmez.

## <a name="guest-os-system-update-process"></a>Konuk işletim sistemi güncelleştirme işlemi
Bu sayfa yakında konuk işletim sistemi sürümleri hakkında bilgi içerir. Müşteriler, "Otomatik" Güncelleştirme ayarlarsanız bulut hizmeti rollerinin yeniden çünkü bir sürüm ne zaman gerçekleştiğini öğrenmek istedikleri belirttiniz. Konuk işletim sistemi sürümleri, genellikle 2-3 hafta MSRC güncelleştirme sonra her ayın ikinci Salı günü oluşan sürüm oluşur. Yeni yayınlar her konuk işletim sistemi ailesi için tüm ilgili MSRC düzeltme içerir.

Microsoft Azure, sürekli güncelleştirmeler yayımlamaktadır. Konuk işletim sistemi gibi işlem hattında yalnızca bir güncelleme var. Bir yayın birçok faktörler tarafından etkilenebilir burada listelemek için çok fazla sayıda. Ayrıca, Azure tam anlamıyla yüz binlerce makineler üzerinde çalışır. Başka bir deyişle, tam tarihi ve saati, rollere ne zaman yeniden vermek mümkün değildir. Sınırlamak veya yeniden başlatma süresi için bir plan üzerinde çalışıyoruz.

Konuk işletim Sisteminin yeni bir sürümü yayımlandığında, Azure'da tamamen yayılması zaman alabilir. Yeni konuk işletim sistemi için hizmet olarak bunlar güncelleştirme etki alanına göre yeniden başlatılır. "Otomatik" güncelleştirmeleri kullanacak şekilde Hizmetleri bir yayın ilk alırsınız. Güncelleştirme tamamlandıktan sonra hizmetinizin Azure portalında listelenen yeni konuk işletim sistemi sürümü görürsünüz. Yeniden yayımlama, bu süre boyunca ortaya çıkabilir. Bazı sürümler uzun süreler boyunca dağıtılabilir ve Otomatik yükseltme yeniden başlatma resmi sürüm tarihinden sonra birçok hafta boyunca gerçekleşmeyebilir. Bir konuk işletim sistemi kullanılabilir duruma geldikten sonra daha sonra açıkça bu sürümü portalından veya yapılandırma dosyanızdan seçebilirsiniz.

Yeniden başlatılır ve Konuk ve konak işletim sistemi güncelleştirmeleri hakkında daha fazla bilgi teknik ayrıntı işaretçileri değerli bilgilere aşırı için post başlıklı MSDN gönderisine bakın [rol örneği yeniden nedeniyle işletim sistemi yükseltmelerini][restarts].

El ile konuk işletim sistemi güncelleştirme olup [konuk işletim sistemi kullanımdan kaldırma İlkesi][retirepolicy] ek bilgi için.

## <a name="guest-os-supportability-and-retirement-policy"></a>Konuk işletim sistemi desteklenebilirliği ve kullanımdan kaldırma İlkesi
Konuk işletim sistemi desteklenebilirliği ve kullanımdan kaldırma İlkesi açıklanan [burada][retirepolicy].

[cloud updates]: https://docs.microsoft.com/azure/cloud-services/cloud-services-update-azure-service
[Konuk işletim sistemi güncelleştirme RSS akışı]: https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/master/GuestOS/GuestOSFeed.xml
[Install .NET on a Cloud Service Role]: https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-install-dotnet/?WT.mc_id=azurebg_email_Trans_963_RevisedNET_Update
[Azure Guest OS Update Settings]: cloud-services-how-to-configure-portal.md
[ssl3 announcement]: https://azure.microsoft.com/blog/2014/12/09/azure-security-ssl-3-0-update/
[Microsoft Security Advisory 3009008]: https://technet.microsoft.com/library/security/3009008.aspx
[ssl3-fixit]: https://go.microsoft.com/?linkid=9863266
[MS14-066]: https://technet.microsoft.com/library/security/ms14-066.aspx
[MS14-046]: https://technet.microsoft.com/library/security/ms14-046.aspx
[retire policy sdk]: https://msdn.microsoft.com/library/dn479282.aspx
[server and gos]: https://msdn.microsoft.com/library/dn775043.aspx
[azuresupport]: https://azure.microsoft.com/support/options/
[net install pkg]: https://www.microsoft.com/download/details.aspx?id=42643
[msrc]: https://technet.microsoft.com/security/dn440717.aspx
[update guest os portal]: https://msdn.microsoft.com/library/gg433101.aspx
[update guest os svc]: https://msdn.microsoft.com/library/gg456324.aspx
[restarts]: https://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx
[patches]: cloud-services-guestos-msrc-releases.md
[retirepolicy]: cloud-services-guestos-retirement-policy.md
[fam1retire]: cloud-services-guestos-family1-retirement.md
[fix]: https://technet.microsoft.com/library/security/ms17-010.aspx
[Windows Azure SDK]: https://www.microsoft.com/en-us/download/details.aspx?id=54917
