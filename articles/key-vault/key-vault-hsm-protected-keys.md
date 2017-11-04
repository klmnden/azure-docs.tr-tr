---
title: "Oluşturma ve Azure anahtar kasası için HSM korumalı anahtarlar Aktarım | Microsoft Docs"
description: "Planlama, oluşturma ve ardından Azure anahtar kasası ile kullanmak üzere kendi HSM korumalı anahtarları aktarma yardımcı olması için bu makaleyi kullanın. BYOK olarak da bilinen veya kendi anahtarını getir."
services: key-vault
documentationcenter: 
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 51abafa1-812b-460f-a129-d714fdc391da
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2017
ms.author: barclayn
ms.openlocfilehash: 6c49b086fd35a855fa8e32fa576c5b52d16f1d04
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="how-to-generate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a>Azure anahtar kasası için nasıl oluşturma ve aktarma HSM korumalı anahtarları
## <a name="introduction"></a>Giriş
Ek güvence için Azure anahtar kasası kullandığınızda alabilir veya donanım güvenlik modülleri (HSM'ler)'hsm sınırını asla bırakın anahtarları oluştur. Bu senaryo, genellikle olarak adlandırılır *kendi anahtarını Getir*, veya BYOK. HSM'ler, FIPS 140-2 Düzey 2 doğrulanmasına sahiptir. Azure anahtar kasası anahtarlarınızı korumak için Thales nShield ailesi Hsm'leri kullanır.

Planlama, oluşturma ve ardından Azure anahtar kasası ile kullanmak üzere kendi HSM korumalı anahtarları aktarma yardımcı olması için bu konudaki bilgileri kullanın.

Bu işlev Azure Çin için kullanılamıyor.

> [!NOTE]
> Azure anahtar kasası hakkında daha fazla bilgi için bkz: [Azure anahtar kasası nedir?](key-vault-whatis.md)  
>
> Bir anahtar kasası için HSM korumalı anahtarlar oluşturma içeren bir başlangıç öğreticisinde için bkz: [Azure anahtar kasası ile çalışmaya başlama](key-vault-get-started.md).
>
>

Oluşturma ve Internet üzerinden bir HSM korumalı anahtara aktarma hakkında daha fazla bilgi için:

* Saldırı yüzeyini azaltan bir çevrimdışı iş istasyonundan, anahtarı oluşturur.
* Anahtar ile bir anahtar değişim anahtarı (Azure anahtar kasası Hsm'lerine aktarılmasına kadar şifrelenmiş olarak kalan KEK), şifrelenir. Anahtarınızın yalnızca şifrelenmiş sürümü özgün iş istasyonundan ayrılır.
* Araç takımı, Kiracı anahtarınızdaki anahtarınızı Azure anahtar kasası güvenlik dünyasına bağlayan özelliklerini ayarlar. Bu nedenle Azure anahtar kasası HSM'ler almak ve anahtarınızı şifresini sonra yalnızca bu HSM'ler bunu kullanabilirsiniz. Anahtarınız dışarı aktarılamaz. Bu bağlama Thales Hsm'leri tarafından zorlanır.
* Anahtarınızı şifrelemek için kullanılan anahtar değişim anahtarı (KEK) Azure anahtar kasası Hsm'lerinin içinde oluşturulur ve dışarı aktarılamaz. HSM'ler kek'in HSM'LERİN HSM'ler dışında NET bir sürümü olabilir uygulayın. Ayrıca, araç takımı KEK'nin dışarı aktarılabilir olmadığı ve Thales tarafından üretilmiş orijinal bir HSM içinde oluşturulduğu Thales kanıtı içerir.
* Araç takımı Azure anahtar kasası güvenlik Dünyası Thales tarafından üretilen orijinal bir HSM üzerinde de oluşturulduğu yönünde Thales kanıtı içerir. Bu kanıtlama size Microsoft orijinal donanım kullandığını kanıtlar.
* Microsoft ve her bir coğrafi bölge güvenlik Dünyaları ayrı ayrı kullanır. Bu ayrım anahtarınızı yalnızca veri merkezleri içinde şifrelediğiniz bölgede kullanılabilir olmasını sağlar. Örneğin, Avrupalı bir müşterinin bir anahtarı Kuzey Amerika veya Asya'daki veri merkezlerinde kullanılamaz.

## <a name="more-information-about-thales-hsms-and-microsoft-services"></a>Thales Hsm'leri ve Microsoft Hizmetleri hakkında daha fazla bilgi
Thales e güvenlikle ilgili bir veri şifreleme ve sanal güvenlik çözümleri finansal hizmetler, yüksek teknoloji, üretim, kamu ve teknoloji sektörlerine başında genel sağlayıcıdır. 40 yıllık korumadaki Kurumsal ve resmi bilgileri ile Thales çözümleri dört enerji ve Havacılık beş büyük şirketler tarafından kullanılır. Çözümleri 22 NATO ülkesi tarafından da kullanılır ve birden fazla yüzde 80'tüm dünyadaki ödeme işlemlerinin güvenli.

Microsoft HSM'ler için resim durumunu geliştirmek için Thales ile iş Birliği yapmaktadır. Bu geliştirmeler, barındırılan hizmet anahtarlarınızı üzerinde bırakmasını denetime sahip olmadan normal avantajlarından yararlanabilmek etkinleştirin. Özellikle, bu geliştirmeler Microsoft'un Hsm'leri gerek yoktur, böylece yönetmesine olanak tanır. Bir bulut hizmeti olarak Azure anahtar kasası, kuruluşunuzun kullanımındaki ani artışları karşılamak üzere ölçek artırabilir. Aynı anda anahtarınızı Microsoft'un Hsm'leri içerisinde korunur: anahtarı oluşturmak ve Microsoft'un Hsm'lerine aktarmak için anahtar yaşam döngüsü üzerinde denetimi korumak.

## <a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a>Uygulama kendi anahtarını getir (BYOK) için Azure anahtar kasası
Eğer kendi HSM korumalı anahtara oluşturmak ve Azure anahtar Kasası'na aktarma aşağıdaki bilgi ve yordamları kullanın: Getir kendi anahtarı (BYOK) senaryosu.

## <a name="prerequisites-for-byok"></a>BYOK için Önkoşullar
Kendi anahtarını getir (BYOK) için Azure anahtar kasası için önkoşul listesi için aşağıdaki tabloya bakın.

| Gereksinim | Daha fazla bilgi |
| --- | --- |
| Azure aboneliği |Azure anahtar kasası oluşturmak için bir Azure aboneliği gerekir: [ücretsiz deneme için kaydolun](https://azure.microsoft.com/pricing/free-trial/) |
| HSM korumalı anahtarları desteklemek için Azure anahtar kasası Premium hizmet katmanı |Azure anahtar kasası için hizmet katmanları ve özellikler hakkında daha fazla bilgi için bkz: [Azure anahtar kasası fiyatlandırma](https://azure.microsoft.com/pricing/details/key-vault/) Web sitesi. |
| Thales HSM, akıllı kartlar ve destek yazılımı |Thales donanım güvenlik modülü ve Thales HSM'ler hakkında temel operasyonel bilginiz erişiminiz olması gerekir. Bkz: [Thales donanım güvenlik modülü](https://www.thales-esecurity.com/msrms/buy) uyumlu modellerin veya bir yoksa bir HSM satın almak için listesi. |
| Aşağıdaki donanım ve yazılım:<ol><li>Çevrimdışı bir x64 en az bir Windows işletim sistemi en az Windows 7 ve Thales nShield yazılımı sürümü ile iş istasyonu sürüm 11.50.<br/><br/>Bu iş istasyonu Windows 7 çalıştırıyorsa, şunları yapmalısınız [Microsoft .NET Framework 4.5 sürümünü yüklemeniz](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</li><li>Internet'e bağlı ve Windows 7'in en az bir Windows işletim sistemi olan bir iş istasyonu ve [Azure PowerShell](/powershell/azure/overview) **en düşük sürüm 1.1.0** yüklü.</li><li>Bir USB sürücü veya en az 16 MB boş alana sahip başka bir taşınabilir depolama cihazı.</li></ol> |Güvenlik nedenleriyle ilk iş istasyonunun bir ağa bağlı değil öneririz. Ancak, bu öneri program aracılığıyla zorlanmaz.<br/><br/>Aşağıdaki yönergelerde bu iş istasyonu için bağlantısı kesik iş istasyonu olarak adlandırılır olduğunu unutmayın.</p></blockquote><br/>Ayrıca, Kiracı anahtarınız bir üretim ağı için ise araç takımını indirmek ve Kiracı anahtarınızı karşıya yüklemek için ikinci ve ayrı bir iş istasyonu kullanmanızı öneririz. Ancak test amacıyla Birincisi aynı iş istasyonunu kullanabilirsiniz.<br/><br/>Aşağıdaki yönergelerde bu ikinci iş istasyonu Internet'e bağlı iş istasyonu olarak bilinir olduğunu unutmayın.</p></blockquote><br/> |

## <a name="generate-and-transfer-your-key-to-azure-key-vault-hsm"></a>Oluşturma ve Azure anahtar kasası HSM anahtarınızı aktarma
Oluşturma ve Azure anahtar kasası HSM anahtarınızı aktarmak için aşağıdaki beş adımları kullanın:

* [1. adım: Internet'e bağlı iş istasyonunuzu hazırlama](#step-1-prepare-your-internet-connected-workstation)
* [2. adım: bağlantısı kesilmiş iş istasyonunuzu hazırlama](#step-2-prepare-your-disconnected-workstation)
* [3. adım: anahtarınızı oluştur](#step-3-generate-your-key)
* [4. adım: anahtarınızı aktarım için hazırlama](#step-4-prepare-your-key-for-transfer)
* [5. adım: anahtarınızı Azure anahtar Kasası'na aktarma](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a>1. adım: Internet'e bağlı iş istasyonunuzu hazırlama
Birinci adım için Internet'e bağlı iş istasyonunuzu üzerinde aşağıdaki yordamları gerçekleştirin.

### <a name="step-11-install-azure-powershell"></a>1.1. adım: Azure PowerShell'i yükleme
Internet'e bağlı iş istasyonundan indirin ve Azure anahtar kasası yönetmek için cmdlet'leri içerir Azure PowerShell modülünü yükleyin. Bu 0.8.13 en az bir sürümünü gerektirir.

Yükleme yönergeleri için bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).

### <a name="step-12-get-your-azure-subscription-id"></a>1.2. adım: Azure abonelik Kimliğinizi alma
Bir Azure PowerShell oturumu başlatın ve aşağıdaki komutu kullanarak Azure hesabınızda oturum açın:

        Add-AzureAccount
Açılır tarayıcı penceresinde Azure hesabı kullanıcı adınızı ve parolanızı girin. Ardından, [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) komutu:

        Get-AzureSubscription
Çıktısını Azure anahtar kasası için kullanacağınız abonelik Kimliğini bulun. Bu abonelik kimliği daha sonra ihtiyacınız olacak.

Azure PowerShell penceresini kapatmayın.

### <a name="step-13-download-the-byok-toolset-for-azure-key-vault"></a>1.3. adım: Azure anahtar kasası için BYOK araç takımı indirme
Microsoft Download Center gidin ve [Azure anahtar kasası BYOK araç takımı indirme](http://www.microsoft.com/download/details.aspx?id=45345) coğrafi bölge veya Azure örneği. Karşıdan yükleme ve onun karşılık gelen SHA-256 paket karmasını paket adını belirlemek için aşağıdaki bilgileri kullanın:

- - -
**Amerika Birleşik Devletleri:**

States.zip KeyVault-BYOK-araçları-birleşik

2E8C00320400430106366A4E8C67B79015524E4EC24A2D3A6DC513CA1823B0D4

- - -
**Avrupa:**

BYOK araçları Europe.zip KeyVault

9AAA63E2E7F20CF9BB62485868754203721D2F88D300910634A32DFA1FB19E4A

- - -
**Asya:**

BYOK araçları AsiaPacific.zip KeyVault

4BC14059BF0FEC562CA927AF621DF665328F8A13616F44C977388EC7121EF6B5

- - -
**Latin Amerika:**

BYOK araçları LatinAmerica.zip KeyVault

E7DFAFF579AFE1B9732C30D6FD80C4D03756642F25A538922DD1B01A4FACB619

- - -
**Japonya:**

BYOK araçları Japan.zip KeyVault

3933C13CC6DC06651295ADC482B027AF923A76F1F6BF98B4D4B8E94632DEC7DF

- - -
**Kore:**

BYOK araçları Korea.zip KeyVault

71AB6BCFE06950097C8C18D532A9184BEF52A74BB944B8610DDDA05344ED136F

- - -
**Avustralya:**

BYOK araçları Australia.zip KeyVault

CD0FB7365053DEF8C35116D7C92D203C64A3D3EE2452A025223EEB166901C40A

- - -
[**Azure kamu:**](https://azure.microsoft.com/features/gov/)

BYOK araçları USGovCloud.zip KeyVault

F8DB2FC914A7360650922391D9AA79FF030FD3048B5795EC83ADC59DB018621A

- - -
**ABD hükümeti savunma Bakanlığı:**

BYOK araçları USGovernmentDoD.zip KeyVault

A79DD8C6DFFF1B00B91D1812280207A205442B3DDF861B79B8B991BB55C35263

- - -
**Kanada:**

BYOK araçları Canada.zip KeyVault

61BE1A1F80AC79912A42DEBBCC42CF87C88C2CE249E271934630885799717C7B

- - -
**Almanya:**

BYOK araçları Germany.zip KeyVault

5385E615880AAFC02AFD9841F7BADD025D7EE819894AA29ED3C71C3F844C45D6

- - -
**Hindistan:**

BYOK araçları India.zip KeyVault

49EDCEB3091CF1DF7B156D5B495A4ADE1CFBA77641134F61B0E0940121C436C8

- - -
**Birleşik Krallık:**

BYOK araçları UnitedKingdom.zip KeyVault

432746BD0D3176B708672CCFF19D6144FCAA9E5EB29BB056489D3782B3B80849

- - -

İndirilen BYOK araç takımı, Azure PowerShell oturumunuzda bütünlüğünü doğrulamak için kullanın [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) cmdlet'i.

    Get-FileHash KeyVault-BYOK-Tools-*.zip

Araç takımı aşağıdakileri içerir:

* İle başlayan bir ada sahip olan bir anahtar değişim anahtarı (KEK) paketi **BYOK-KEK - pkg-.**
* İle başlayan bir ada sahip olan bir güvenlik Dünyası paketi **BYOK-SecurityWorld - pkg-.**
* Adlı bir python komut **verifykeypackage.py.**
* Adlı bir komut satırı yürütülebilir dosyası **KeyTransferRemote.exe** ve ilişkili DLL'ler.
* Adlı bir Visual C++ yeniden dağıtılabilir paketi, **vcredist_x64.exe.**

Paketi bir USB sürücü veya başka bir taşınabilir depolama kopyalayın.

## <a name="step-2-prepare-your-disconnected-workstation"></a>2. adım: bağlantısı kesilmiş iş istasyonunuzu hazırlama
Bu ikinci adım için bir ağa (Internet veya iç ağınıza) bağlı değil iş istasyonunda aşağıdaki yordamları gerçekleştirin.

### <a name="step-21-prepare-the-disconnected-workstation-with-thales-hsm"></a>2.1. adım: bağlantısı kesilmiş iş istasyonunuzu Thales HSM ile hazırlama
Bir Windows bilgisayara nCipher (Thales) destek yazılımını yükleyin ve ardından o bilgisayara bir Thales HSM ekleyin.

Thales araçlarının yolunuzda bulunduğundan emin olun (**%nfast_home%\bin**). Örneğin, aşağıdaki komutu yazın:

        set PATH=%PATH%;"%nfast_home%\bin"

Daha fazla bilgi için Thales HSM ile verilen kullanıcı kılavuzuna bakın.

### <a name="step-22-install-the-byok-toolset-on-the-disconnected-workstation"></a>2.2. adım: BYOK araç takımını bağlantısı kesilmiş iş istasyonunda yükleme
USB sürücü veya başka bir taşınabilir depolama BYOK araç takımı paketini kopyalayın ve sonra aşağıdakileri yapın:

1. İndirilen paketteki dosyaları herhangi bir klasöre ayıklayın.
2. Bu klasörden vcredist_x64.exe çalıştırın.
3. Yönergeleri, Visual Studio 2013 için Visual C++ çalışma zamanı bileşenleri yüklemek için izleyin.

## <a name="step-3-generate-your-key"></a>3. adım: anahtarınızı oluştur
Bu üçüncü adım için bağlantısı kesilmiş iş istasyonunda aşağıdaki yordamları gerçekleştirin. Bu adımı tamamlamak için HSM başlatma modunda olmalıdır. 


### <a name="step-31-change-the-hsm-mode-to-i"></a>3.1. adım: HSM modunu 'I' değiştirme
Thales nShield kenar, modunu değiştirmek için kullanıyorsanız: 1. Gerekli modu vurgulamak için Modu düğmesini kullanın. 2. Birkaç saniye içinde tuşuna basın ve Temizle'yi birkaç saniye basılı tutun. Mod değişirse, yeni modun ışığı yanıp durdurur ve aydınlatılmış kalır. Durum LED birkaç saniye düzensiz flash ve düzenli olarak cihaz hazır olduğunda sonra yanıp sönen. Aksi takdirde geçerli modda, uygun modu ışığı aygıt kalır aydınlatma.

### <a name="step-32-create-a-security-world"></a>3.2. adım: güvenlik Dünyası oluşturma
Bir komut istemi başlatın ve Thales yeni dünya programını çalıştırın.

    new-world.exe --initialize --cipher-suite=DLf1024s160mRijndael --module=1 --acs-quorum=2/3

Bu programın oluşturduğu bir **güvenlik Dünyası** % C:\ProgramData\nCipher\Key Management Data\local klasörüne karşılık gelen NFAST_KMDATA%\local\world dosyası. Çekirdek için farklı değerler kullanabilirsiniz, ancak örneğimizde her biri için üç adet boş kart ve PIN girmeniz istenir. Ardından, herhangi iki kart güvenlik dünyasına tam erişim verin. Bu kartlar hale **yönetici kart Seti** yeni güvenlik Dünyası için.

Ardından şunları yapın:

* Dünya dosyasının yedeğini alın. Güvenli ve dünya dosyasını, yönetici kartlarını ve PIN korumak ve tek bir kişinin birden fazla kart erişimi olduğundan emin olun.

### <a name="step-33-change-the-hsm-mode-to-o"></a>3.3. adım: HSM moduna '
Thales nShield kenar, modunu değiştirmek için kullanıyorsanız: 1. Gerekli modu vurgulamak için Modu düğmesini kullanın. 2. Birkaç saniye içinde tuşuna basın ve Temizle'yi birkaç saniye basılı tutun. Mod değişirse, yeni modun ışığı yanıp durdurur ve aydınlatılmış kalır. Durum LED birkaç saniye düzensiz flash ve düzenli olarak cihaz hazır olduğunda sonra yanıp sönen. Aksi takdirde geçerli modda, uygun modu ışığı aygıt kalır aydınlatma.


### <a name="step-34-validate-the-downloaded-package"></a>3.4. adım: indirilen paketi doğrulama
Bu adım isteğe bağlıdır ancak aşağıdakileri doğrulayabilmeniz böylece önerilir:

* Araç takımında yer alan anahtar değişim anahtarı, orijinal bir Thales HSM'den oluşturulmuştur.
* Araç takımında yer alan güvenlik Dünyası karması, orijinal bir Thales HSM'de oluşturulmuştur.
* Anahtar değişim anahtarı aktarılamaz.

> [!NOTE]
> İndirilen paketi doğrulamak için HSM, açık bağlanmalıdır ve üzerindeki güvenlik Dünyası (örneğin, yeni oluşturduğunuz bir) olması gerekir.
>
>

İndirilen paketi doğrulamak için:

1. Coğrafi bölge veya Azure örneği bağlı olarak aşağıdakilerden birini yazarak verifykeypackage.py betiği çalıştırın:

   * Kuzey Amerika için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
   * Avrupa için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
   * Asya için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
   * Latin Amerika için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
   * Japonya için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
   * Kore için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-KOREA-1 -w BYOK-SecurityWorld-pkg-KOREA-1
   * Avustralya için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
   * İçin [Azure kamu](https://azure.microsoft.com/features/gov/), Azure ABD devlet kurumları örneğini kullanır:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
   * ABD hükümeti savunma Bakanlığı için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USDOD-1 -w BYOK-SecurityWorld-pkg-USDOD-1
   * Kanada için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
   * Almanya için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
   * Hindistan için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
     > [!TIP]
     > Thales yazılımı, %NFAST_HOME%\python\bin python içerir
     >
     >
2. Doğrulama başarılı gösteren aşağıdaki gördüğünüzü onaylayın: **Result: SUCCESS**

Bu komut dosyası, Thales kök anahtarına kadar imzalayan zincirini doğrular. Bu kök anahtarın karması komut dosyasındaki katıştırılır ve değeri **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**. Siz de bu değeri ayrı olarak ziyaret ederek onaylayabilirsiniz [Thales Web sitesini](http://www.thalesesec.com/).

Artık yeni bir anahtar oluşturmaya hazırsınız.

### <a name="step-35-create-a-new-key"></a>3.5. adım: yeni bir anahtar oluşturun
Thales kullanarak bir anahtar oluşturmak **generatekey** program.

Anahtarı oluşturmak için aşağıdaki komutu çalıştırın:

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

Bu komutu çalıştırdığınızda, aşağıdaki yönergeleri kullanın:

* Parametre *korumak* değerine ayarlanmalıdır **Modülü**gösterildiği gibi. Bu, modül korumalı bir anahtar oluşturur. BYOK araç takımı, OCS korumalı anahtarları desteklemez.
* Değerini *contosokey* için **plainname** ve **contosokey** herhangi bir dize değeri ile. Yönetim ek yüklerini en aza indirmek ve hataları riskini azaltmak için her ikisi için aynı değeri kullanmanız önerilir. **Plainname** değeri yalnızca sayı, tire ve küçük harf içermelidir.
* Bu örnekte pubexp (varsayılan) boş kalır, ancak özel değerler belirtebilirsiniz. Daha fazla bilgi için Thales belgelerine bakın.

Bu komut, %NFAST_KMDATA%\local klasörünüzde adı ile bir simgeleştirilmiş anahtar dosyası oluşturur **key_simple_**, ardından **plainname** komutta belirtildi. Örneğin: **key_simple_contosokey**. Bu dosya şifreli bir anahtar içerir.

Bu simgeleştirilmiş anahtar dosyasını güvenli bir konuma yedekleyin.

> [!IMPORTANT]
> Anahtarınızı daha sonra Azure anahtar Kasası'na aktardığınızda, anahtar ve güvenlik dünyanızı güvenli biçimde yedeklemeniz son derece önemli olacak şekilde Microsoft bu anahtarı size geri dışarı aktaramazsınız. Kılavuzu ve anahtarınızı yedekleme için en iyi uygulamalar için Thales başvurun.
>
>

Artık Azure anahtar Kasası'na anahtarınızı aktarmak hazırsınız.

## <a name="step-4-prepare-your-key-for-transfer"></a>4. adım: anahtarınızı aktarım için hazırlama
Dördüncü Bu adım için bağlantısı kesilmiş iş istasyonunda aşağıdaki yordamları gerçekleştirin.

### <a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a>4.1. adım: sınırlı izinlerle anahtarınızın bir kopyasını oluşturma

Yeni bir komut istemi açın ve burada BYOK zip dosyası unzipped konumun geçerli dizini değiştirin. Bir komut isteminden, anahtarı üzerindeki izinleri azaltmak için coğrafi bölge veya Azure örneği bağlı olarak aşağıdakilerden birini çalıştırın:

* Kuzey Amerika için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
* Avrupa için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
* Asya için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
* Latin Amerika için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
* Japonya için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
* Kore için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-KOREA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-KOREA-1
* Avustralya için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
* İçin [Azure kamu](https://azure.microsoft.com/features/gov/), Azure ABD devlet kurumları örneğini kullanır:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
* ABD hükümeti savunma Bakanlığı için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1
* Kanada için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
* Almanya için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
* Hindistan için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1

Bu komutu çalıştırdığınızda, yerini *contosokey* ile aynı belirtilen değere **adım 3.5: yeni bir anahtar oluşturun** gelen [anahtarınızı](#step-3-generate-your-key) adım.

Güvenlik Dünyası yönetim kartlarınızı takmanız istenir.

Komut tamamlandığında, gördüğünüz **sonucu: başarı** ve anahtarınızın sınırlı izinlere sahip kopyası, key_xferacıd_ adlı dosyada olan<contosokey>.

İnceler ACL'ler Thales yardımcı programını kullanarak komutlar:

* aclprint.PY:

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
* kmfile-dump.exe:

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
  Bu komutları çalıştırdığınızda, contosokey, belirttiğiniz değerle değiştirin **adım 3.5: yeni bir anahtar oluşturun** gelen [anahtarınızı](#step-3-generate-your-key) adım.

### <a name="step-42-encrypt-your-key-by-using-microsofts-key-exchange-key"></a>4.2. adım: Microsoft'un anahtar değişim anahtarını kullanarak anahtarınızı şifreleme
Coğrafi bölge veya Azure örneği bağlı olarak aşağıdaki komutlardan birini çalıştırın:

* Kuzey Amerika için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Avrupa için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Asya için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Latin Amerika için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Japonya için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Kore için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-KOREA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-KOREA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Avustralya için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* İçin [Azure kamu](https://azure.microsoft.com/features/gov/), Azure ABD devlet kurumları örneğini kullanır:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* ABD hükümeti savunma Bakanlığı için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Kanada için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Almanya için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Hindistan için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey

Bu komutu çalıştırdığınızda, aşağıdaki yönergeleri kullanın:

* Değiştir *contosokey* anahtarı oluşturmak için kullandığınız tanımlayıcıyla **adım 3.5: yeni bir anahtar oluşturun** gelen [anahtarınızı](#step-3-generate-your-key) adım.
* Değiştir *Subscriptionıd* anahtar kasanızı içeren Azure abonelik kimliği. Bu değer alınan önceden, **adım 1.2: Azure abonelik Kimliğinizi alın** gelen [Internet'e bağlı iş istasyonunuzu hazırlama](#step-1-prepare-your-internet-connected-workstation) adım.
* Değiştir *ContosoFirstHSMKey* , çıktı dosyanızın adı için kullanılan bir etikete sahip.

Bu başarıyla tamamlandığında görüntüler **sonucu: başarı** ve geçerli klasörde şu ada sahip yeni bir dosya yok: KeyTransferPackage -*ContosoFirstHSMkey*.byok

### <a name="step-43-copy-your-key-transfer-package-to-the-internet-connected-workstation"></a>4.3. adım: anahtar aktarım paketinizi Internet'e bağlı iş istasyonuna kopyalama
Bir USB sürücü veya başka bir taşınabilir depolama çıktı dosyasını (KeyTransferPackage-ContosoFirstHSMkey.byok) önceki adımda, Internet'e bağlı iş istasyonunuzu kopyalamak için kullanın.

## <a name="step-5-transfer-your-key-to-azure-key-vault"></a>5. adım: anahtarınızı Azure anahtar Kasası'na aktarma
Bu son adım için Internet'e bağlı iş istasyonunda, kullanın [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) Azure anahtar kasası HSM bağlantısı kesilmiş iş istasyonundan kopyaladığınız anahtar aktarma paketini karşıya yüklemek için cmdlet:

    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\KeyTransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'

Karşıya yükleme başarılı olursa, gördüğünüz görüntülenen eklediğiniz anahtar özelliklerini.

## <a name="next-steps"></a>Sonraki adımlar
Bu gibi durumlarda, bu HSM korumalı anahtara artık anahtar kasanız kullanabilirsiniz. Daha fazla bilgi için bkz: **bir donanım güvenlik modülü (HSM) kullanmak isteyip istemediğinizi** bölümüne [Azure anahtar kasası ile çalışmaya başlama](key-vault-get-started.md) Öğreticisi.
