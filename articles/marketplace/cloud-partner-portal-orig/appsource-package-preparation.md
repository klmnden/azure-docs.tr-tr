---
title: AppSource paket Hazırlık | Azure Market
description: Hazırlama ve AppSource paketleri oluşturmak nasıl Explanaion.
services: Azure, Marketplace, Cloud Partner Portal,
author: pbutlerm
manager: Ricardo.Villalobos
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: ff822e87bfec5daa161172c0d47975eb06cc2808
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64935639"
---
# <a name="appsource-package-preparation"></a>AppSource paketini hazırlama

Solution.zip dosyasına ek olarak, ihtiyacınız olacak bir **AppSource paket**. Bu, çözümünüzün müşterilerin CRM ortamına dağıtma işlemini otomatik hale getirmek için gerekli tüm varlıkları içeren bir .zip dosyasıdır. **AppSource paket** aşağıdaki bileşenlere sahiptir

* Paket dağıtıcısı için paket
* **Content_Types.xml** kullandığınız varlıklar dosyayla
* Uygulamaya özgü verileri olan XML dosyası
* Yönetim merkezinde, dökümle görüntüler 32 x 32 logosu
* Lisans koşullarını, gizlilik ilkesi

Aşağıdaki adımlar, AppSource paketinizi oluşturmanıza yardımcı olur.

## <a name="a-create-a-package-for-the-package-deployer"></a>a. Paket dağıtıcısı için paket oluşturma

Paket dağıtıcısı için paket Appsource'ta paketin bir parçasıdır.

Paket dağıtıcısı için paket oluşturmak için aşağıdaki yönergeleri kullanın: [ https://msdn.microsoft.com/library/dn688182.aspx ](https://msdn.microsoft.com/library/dn688182.aspx). Tamamlandığında, varlıklar paketiniz oluşur:

1. Paket klasörüne: tüm çözümleri, yapılandırma verilerini, düz dosyalar ve içerik paketiniz için içerir. _Not: Aşağıdaki örnekte "PkgFolder" adlı paket klasörde varsayacağız_
2. DLL: Derleme, paket için özel kod içerir. _Not: Aşağıdaki örnekte bu dosya, "MicrosoftSample.dll." adlı varsayacağız_

Şimdi, adlı bir dosya oluşturmanız gerekir "**Content_Types.xml**" Bu dosya tüm paketinizin bir parçası olan varlıklar uzantıları listelenir. Dosya için kod örneği aşağıda verilmiştir.

    <?xml version="1.0" encoding="utf-8"?>
        <Types xmlns="http://schemas.openxmlformats.org/package/2006/content-types">
        <Default Extension="xml" ContentType="application/octet-stream" />
        <Default Extension="xaml" ContentType="application/octet-stream" />
        <Default Extension="dll" ContentType="application/octet-stream" />
        <Default Extension="zip" ContentType="application/octet-stream" />
        <Default Extension="jpg" ContentType="application/octet-stream" />
        <Default Extension="gif" ContentType="application/octet-stream" />
        <Default Extension="png" ContentType="application/octet-stream" />
        <Default Extension="htm" ContentType="application/octet-stream" />
        <Default Extension="html" ContentType="application/octet-stream" />
        <Default Extension="db" ContentType="application/octet-stream" />
        <Default Extension="css" ContentType="application/octet-stream" />
    </Types>

Son adım, aşağıdaki tek bir dosyada zip sağlamaktır. Bu çağrı **package.zip**. Yer alır

1. PkgFolder (klasörü içindeki her şey dahil)
2. dll
3. **Content_Types.xml**

Package.zip oluşturma adımları:

1. Paket klasörünüze **Content_Types.xml** dosyası ve bir dizine PackageName.dll.

![CRMScreenShot2](media/CRMScreenShot2.png)

1. Tüm öğeleri klasörü seçin, sağ tıklayın ve göndermek için sıkıştırılmış (ZIP) klasörü seçin

![CRMScreenShot3](media/CRMScreenShot3.png)

1. Package.zip için adı değiştirin

![CRMScreenShot4](media/CRMScreenShot4.png)

## <a name="b-create-an-appsource-package"></a>b. AppSource paketi oluşturma

AppSource paket birkaç ek dosyalar gerektirir.

1. jpg (32 x 32 çözümleme)
2. HTML (HTML biçimli dosya)
3. **Content_Types.xml** (yukarıda aynı)
4. xml

İnput.xml yönelik örnek kod aşağıda verilmiştir. Tanımlarında bkz tablonun altındaki.

    <PvsPackageData>
        <ProviderName>Microsoft</ProviderName>
        <PackageFile>package.zip</PackageFile>
        <SolutionAnchorName>SampleSolution.zip</SolutionAnchorName>
        <StartDate>01/01/2016</StartDate>
        <EndDate>01/01/2021</EndDate>
        <SupportedCountries>US,CA</SupportedCountries>
        <LearnMoreLink>https://www.microsoft.com</LearnMoreLink>
        <Locales>
        <PackageLocale Code="1033" IsDefault="true">
        <Logo>logo32x32.png</Logo>
        <Terms>
        <PackageTerm File="TermsOfUse.html" />
        </Terms>
        </PackageLocale>
        </Locales>
    </PvsPackageData>
 
**Burada:**

|Alan|Ayrıntılar|
|---|---|
|ProviderName|Kimin geldiğini çözümüdür. Microsoft ekibi, bu Microsoft olmalıdır.|
|PackageFile |Paket dağıtıcısı varlıklar ile birlikte bir içerik daraltılmış\_types.xml dosya. Bu ZIP dosyasını, paket dağıtıcısı derleme ve paket dağıtıcısı varlıklar ile klasörü içermelidir. Diğer bir deyişle, package.zip|
|SolutionAnchorName |Görünen ad ve açıklama çözüm varlıkları için kullanılan paket dağıtıcısı çözüm zip dosyasının adı.|
| startDate| Bu çözüm paketine kullanılabilir olacak tarihtir. AA/GG/YYYY biçiminde olan|
|endDate|Bu çözüm paketine kullanılabilir olmasını durduracak tarihtir. AA/GG/YYYY biçiminde olan |
|SupportedCountries |Bu paketi görürsünüz ülkeler/bölgeler, virgülle ayrılmış listesini budur. Tüm geçerli Ülke Kodları listesi için Çevrimiçi Hizmetleri ile görüşün. Zaman bu yazılmasını listenin şöyleydi: AE, AL, AM, SANİYE BAŞINA AO, AR, AT, AU, AZ, BA, BB, BD, OLMASI, BG, BH, BM, BN, BO, BR TARAFINDAN CA, CH, CI, CL, CM, ORTAK, CR, CV, FA, CY, CZ, DE, DK,, DZ, AB, EE, ÖRN, ES, FI, FR, GB, GE, GH, GR, GT, HK , HN, İK, HU, KİMLİĞİ, IE, IL, IN, IQ, OLDUĞU GİBİ BU, JM, JO, JP, L, KG, KN, KR, KW, KY, KZ, LB, LK, LT, LU, LV, LY, MA, MC, MD, BANA, MK, MN, AY, MT, MU, MX, MY, NG, NI, NL, HAYIR, NZ, OM, PA, PE, PH , PK, PL, ÇEKME İSTEĞİ, PS, PT, KOPYALA, QA, RO, RS, RU, RW, SA, SE, SG, SI, SK, SN, SV, TH, TM, TN, TR, TT, TW, UA, ABD, UY, UZ, KALDIR, VI, VN, ZA, ZW |
|LearnMoreLink | Bu paket için daha fazla bilgi sayfası URL'si. |
|Yerel ayarlar|Bu düğüm tercih edilen çözümde UX'i desteklemek istediğiniz her bir UX dilin bir örneği Bu düğüm, yerel ayar, logo ve her bir dilin koşullarını tanımlayan alt öğeleri içerir|
|Yerel ayarlar: PackageLocale.Code|Bu düğüm için dilin LCID. Örnek: İngilizce (ABD) 1033'tür|
|Yerel ayarlar: PackageLocale.IsDefault|Bu varsayılan dili olduğunu gösterir. Bu sonbaharda kullanılan müşteri tarafından seçmiş UX dil kullanılabilir değilse, dil yedekleyin.|
|Yerel ayarlar: Logo|Bu logo, bu paket için kullanmak istiyorsanız. Simge boyutu 32 x 32'dir. PNG ve JPG izin verilen biçimler:|
|Yerel ayarlar: koşulları: PackageTerm.File|Bu lisans koşullarına içeren HTML belge dosya adıdır.|

İşte burada görüntülenir:

![CRMScreenShot5](media/CRMScreenShot5.png)

Son adım, aşağıdaki tek bir dosyada zip sağlamaktır.

1. zip (daha önce oluşturduğunuz)
2. **Content_Types.xml**
3. xml
4. PNG
5. html

![CRMScreenShot6](media/CRMScreenShot6.png)

Uygulamanız için uygun dosyayı yeniden adlandırın. Şirketinizin adını ve uygulama adı dahil tercih ederiz. Örneğin: **_Microsoft_SamplePackage.zip**.
