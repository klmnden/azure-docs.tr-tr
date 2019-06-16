---
title: Oluşturma ve Azure anahtar kasası - Azure Key Vault için HSM korumalı anahtarlar Aktarım | Microsoft Docs
description: Planlama, oluşturma ve Azure anahtar kasası ile kullanmak için kendi HSM korumalı anahtarlar'ı aktarım yardımcı olması için bu makaleyi kullanın. BYOK olarak da bilinen veya kendi anahtarınızı getirin.
services: key-vault
author: barclayn
manager: barbkess
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: barclayn
ms.openlocfilehash: 1ae94718aa41c58f4d5e397942492ad8ed643ae3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65556213"
---
# <a name="how-to-generate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a>Azure anahtar kasası için nasıl oluşturma ve aktarma HSM korumalı anahtarlar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Ek güvence için Azure anahtar Kasası'nı kullandığınızda alabilir veya HSM sınırını asla terk donanım güvenlik modüllerinde (HSM'ler) anahtarları oluşturun. Bu senaryo, genellikle olarak adlandırılır *kendi anahtarını Getir*, bYok. HSM'ler, FIPS 140-2 Düzey 2 doğrulanmasına sahiptir. Azure Key Vault, anahtarlarınızı korumak için HSM'ler nCipher nShield ailesini kullanır.

Bilgileri, planlama, oluşturma ve Azure anahtar kasası ile kullanmak için kendi HSM korumalı anahtarlar'ı aktarım yardımcı olması için bu konudaki kullanın.

Bu işlev Azure Çin için kullanılamıyor.

> [!NOTE]
> Azure Key Vault hakkında daha fazla bilgi için bkz. [Azure anahtar kasası nedir?](key-vault-whatis.md)  
> Bir anahtar kasası için HSM korumalı anahtarlar oluşturma içeren bir başlangıç öğreticisi için bkz: [Azure anahtar kasası nedir?](key-vault-overview.md).

Oluşturma ve HSM korumalı bir anahtar Internet üzerinden aktarmaktan hakkında daha fazla bilgi için:

* Saldırı yüzeyini azaltan bir çevrimdışı iş istasyonundan, anahtarı üret
* Anahtar ile bir anahtar değişim anahtarı (Azure anahtar kasası Hsm'lerine aktarılmasına kadar şifrelenmiş olarak kalır, KEK), şifreli. Anahtarınızın yalnızca şifrelenmiş sürümü özgün iş istasyonundan bırakır.
* Araç takımı, Kiracı anahtarınızdaki anahtarınızı Azure anahtar kasası güvenlik dünyasına bağlayan özelliklerini ayarlar. Bu sayede Azure anahtar kasası Hsm'lerine almak ve anahtarınızı şifresini sonra yalnızca bu HSM'ler bunu kullanabilirsiniz. Anahtarınız dışarı aktarılamaz. Bu bağlama, nCipher Hsm'leri tarafından zorlanır.
* Anahtarınızı şifrelemek için kullanılan anahtar değişim anahtarı (KEK) Azure anahtar kasası Hsm'lerinin içinde oluşturulur ve dışarı aktarılamaz. HSM'ler KEK Hsm'lerin dışında NET bir sürümü olabilir uygular. Ayrıca, araç takımı KEK'nin dışarı aktarılabilir olmadığı ve nCipher tarafından üretilmiş orijinal bir HSM içinde oluşturulduğu nCipher kanıtlamayı içerir.
* Araç takımı, Azure Key Vault güvenlik Dünyası nCipher tarafından üretilen orijinal bir HSM üzerinde de oluşturulan nCipher kanıtlamayı içerir. Bu kanıtlama için Microsoft orijinal donanım kullandığını kanıtlar.
* Microsoft, ayrı Dünyaları kullanır ve her coğrafi bölgede güvenlik Dünyaları ayırın. Bu ayrım anahtarınızı içinde şifrelendiği bölgedeki veri merkezlerinde yalnızca kullanılabilir sağlar. Örneğin, Avrupalı bir müşterinin bir anahtarı Kuzey Amerika veya Asya'daki veri merkezlerinde kullanılamaz.

## <a name="more-information-about-ncipher-hsms-and-microsoft-services"></a>Daha fazla bilgi hakkında nCipher HSM'ler ve Microsoft Hizmetleri

Güvenlik nCipher bir siber güvenlik çözümleri finansal hizmetler, yüksek teknoloji, üretim, kamu ve teknoloji sektörlerine veri şifreleme ve önde gelen genel sağlayıcısıdır. 40 yıllık tecrübesiyle Kurumsal ve resmi bilgileri ile şifreleme güvenlik çözümleri, nCipher dört enerji ve Havacılık beş en büyük şirketleri tarafından kullanılır. Çözümleri ayrıca 22 NATO ülkeler/bölgeler tarafından kullanılır ve daha yüzde 80'den tüm dünyadaki ödeme işlemlerinin güvenli.

Microsoft, nCipher HSM'ler için resim durumunu geliştirmek için güvenlik ile CISCO. Bu geliştirmeler tanımaktadır anahtarlarınızın denetimi sizin bırakmadan, barındırılan hizmetlere tipik avantajlardan yararlanmanıza olanak tanıyacak. Özellikle, bu geliştirmeler, böylece gerekmez HSM'ler Microsoft yürütebilmektedir. Bir bulut hizmeti olan Azure Key Vault, kuruluşunuzun ani artışları karşılamak üzere kullanımındaki. Aynı zamanda, anahtarınızı Microsoft'un Hsm'leri içerisinde korunur: Anahtar oluşturma ve Microsoft'un Hsm'lerine aktarmak için anahtar yaşam döngüsü denetim korur.

## <a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a>Uygulama kendi anahtarını getir (BYOK) için Azure anahtar kasası

Eğer kendi HSM korumalı anahtar oluşturun ve ardından Azure anahtar Kasası'na aktarma aşağıdaki bilgi ve yordamları kullanın — Getir kendi anahtarı (BYOK) senaryosu.

## <a name="prerequisites-for-byok"></a>BYOK için Önkoşullar

Kendi anahtarını getir (BYOK) için Azure anahtar kasası için bir önkoşul listesi için aşağıdaki tabloya bakın.

| Gereksinim | Daha fazla bilgi |
| --- | --- |
| Azure aboneliği |Bir Azure anahtar kasası oluşturmak için bir Azure aboneliğinizin olması gerekir: [Ücretsiz deneme için kaydolun](https://azure.microsoft.com/pricing/free-trial/) |
| HSM korumalı anahtarları desteklemek için Azure anahtar kasası Premium hizmet katmanı |Azure Key Vault için hizmet katmanları ve özellikler hakkında daha fazla bilgi için bkz. [Azure anahtar kasası fiyatlandırma](https://azure.microsoft.com/pricing/details/key-vault/) Web sitesi. |
| nCipher nShield HSM, akıllı kartlar ve destek yazılımı |Bir donanım güvenlik modülü nCipher erişimi ve nCipher nShield HSM'ler hakkında temel operasyonel bilginiz olmalıdır. Bkz: [nCipher nShield donanım güvenlik modülü](https://www.ncipher.com/products/key-management/cloud-microsoft-azure/how-to-buy) uyumlu modellerin ya da bir yoksa bir HSM satın almak için listesi. |
| Aşağıdaki donanım ve yazılım:<ol><li>Çevrimdışı bir x64 iş istasyonunda en az bir Windows işletim sistemi en az Windows 7 ve nCipher nShield yazılımı sürümü ile 11.50 sürümü.<br/><br/>Bu iş istasyonu Windows 7 çalıştırıyorsa, şunları yapmalısınız [Microsoft .NET Framework 4.5 sürümünü yüklemeniz](https://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</li><li>Internet'e bağlı ve Windows 7'in en az bir Windows işletim sistemi olan bir iş istasyonu ve [Azure PowerShell](/powershell/azure/overview?view=azps-1.2.0) **en düşük sürüm 1.1.0** yüklü.</li><li>Bir USB sürücü veya en az 16 MB boş alanı olan başka bir taşınabilir depolama cihazı.</li></ol> |Güvenlik nedenleriyle ilk iş istasyonunun bir ağa bağlı değilse öneririz. Ancak, bu öneriyi program aracılığıyla zorlanmaz.<br/><br/>Aşağıdaki yönergelerde bu iş istasyonu, bağlantısı kesilmiş iş istasyonu olarak adlandırılır.</p></blockquote><br/>Kiracı anahtarınız bir üretim ağı için ise, ayrıca, araç takımını indirmek için ikinci ve ayrı bir iş istasyonu kullanın ve Kiracı anahtarınızı karşıya öneririz. Ancak test amacıyla Birincisi aynı iş istasyonunu kullanabilirsiniz.<br/><br/>Aşağıdaki yönergelerde bu ikinci iş istasyonu İnternet'e bağlı iş istasyonu olarak adlandırılır.</p></blockquote><br/> |

## <a name="generate-and-transfer-your-key-to-azure-key-vault-hsm"></a>Oluşturma ve anahtarınızı Azure anahtar kasası HSM'ye aktarma

Oluşturma ve anahtarınızı Azure anahtar kasası HSM'ye aktarma beş aşağıdaki adımları kullanın:

* [1. adım: İnternet'e bağlı iş istasyonunuzu hazırlama](#step-1-prepare-your-internet-connected-workstation)
* [2. adım: Bağlantısı kesilmiş iş istasyonunuzu hazırlama](#step-2-prepare-your-disconnected-workstation)
* [3. adım: Anahtarınızı](#step-3-generate-your-key)
* [4. adım: Kiracı anahtarınızı aktarım için hazırlama](#step-4-prepare-your-key-for-transfer)
* [5. adım: Azure Key Vault'a anahtar aktarma](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a>1\. adım: İnternet'e bağlı iş istasyonunuzu hazırlama

Birinci adım için Internet'e bağlı iş istasyonunuzu üzerinde aşağıdaki yordamları gerçekleştirin.

### <a name="step-11-install-azure-powershell"></a>Adım 1.1: Azure PowerShell'i yükleme

İnternet'e bağlı iş istasyonundan indirin ve Azure anahtar Kasası'nı yönetmek için cmdlet'ler içeren Azure PowerShell modülünü yükleyin. Yükleme yönergeleri için bkz. [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview).

### <a name="step-12-get-your-azure-subscription-id"></a>Adım 1.2: Azure abonelik Kimliğinizi alın

Azure PowerShell oturumu başlatın ve aşağıdaki komutu kullanarak Azure hesabınızda oturum açın:

```Powershell
   Connect-AzAccount
```
Açılır tarayıcı penceresinde Azure hesabı kullanıcı adınızı ve parolanızı girin. Ardından, [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription) komutu:

```powershell
   Get-AzSubscription
```
Çıkışı, Azure anahtar kasası için kullanacağınız abonelik Kimliğini bulun. Bu abonelik kimliği daha sonra gerekecektir.

Azure PowerShell penceresini kapatmayın.

### <a name="step-13-download-the-byok-toolset-for-azure-key-vault"></a>1\.3. adım: İçin Azure anahtar kasası BYOK araç takımını indirin

Microsoft Download Center gidin ve [Azure anahtar kasası BYOK araç takımını indirmek](https://www.microsoft.com/download/details.aspx?id=45345) coğrafi bölge veya Azure örneği. İndirme ve karşılık gelen, SHA-256'yı paket karmasını paket adını tanımlamak için aşağıdaki bilgileri kullanın:

- - -
**Amerika Birleşik Devletleri:**

States.zip KeyVault-BYOK-araçları-birleşik

2E8C00320400430106366A4E8C67B79015524E4EC24A2D3A6DC513CA1823B0D4

- - -
**Avrupa:**

Anahtar kasası BYOK araç Europe.zip

9AAA63E2E7F20CF9BB62485868754203721D2F88D300910634A32DFA1FB19E4A

- - -
**Asya için:**

Anahtar kasası BYOK araç AsiaPacific.zip

4BC14059BF0FEC562CA927AF621DF665328F8A13616F44C977388EC7121EF6B5

- - -
**Latin Amerika:**

Anahtar kasası BYOK araç LatinAmerica.zip

E7DFAFF579AFE1B9732C30D6FD80C4D03756642F25A538922DD1B01A4FACB619

- - -
**Japonya:**

Anahtar kasası BYOK araç Japan.zip

3933C13CC6DC06651295ADC482B027AF923A76F1F6BF98B4D4B8E94632DEC7DF

- - -
**Kore:**

Anahtar kasası BYOK araç Korea.zip

71AB6BCFE06950097C8C18D532A9184BEF52A74BB944B8610DDDA05344ED136F

- - -
**Güney Afrika:**

KeyVault-BYOK-Tools-SouthAfrica.zip

C41060C5C0170AAAAD896DA732E31433D14CB9FC83AC3C67766F46D98620784A

- - -
**BAE:**

KeyVault-BYOK-Tools-UAE.zip

FADE80210B06962AA0913EA411DAB977929248C65F365FD953BB9F241D5FC0D3

- - -
**Avustralya:**

Anahtar kasası BYOK araç Australia.zip

CD0FB7365053DEF8C35116D7C92D203C64A3D3EE2452A025223EEB166901C40A

- - -
[**Azure kamu:** ](https://azure.microsoft.com/features/gov/)

KeyVault-BYOK-Tools-USGovCloud.zip

F8DB2FC914A7360650922391D9AA79FF030FD3048B5795EC83ADC59DB018621A

- - -
**ABD DOD:**

KeyVault-BYOK-Tools-USGovernmentDoD.zip

A79DD8C6DFFF1B00B91D1812280207A205442B3DDF861B79B8B991BB55C35263

- - -
**Kanada:**

KeyVault-BYOK-Tools-Canada.zip

61BE1A1F80AC79912A42DEBBCC42CF87C88C2CE249E271934630885799717C7B

- - -
**Almanya:**

Anahtar kasası BYOK araç Germany.zip

5385E615880AAFC02AFD9841F7BADD025D7EE819894AA29ED3C71C3F844C45D6

- - -
**Hindistan:**

KeyVault-BYOK-Tools-India.zip

49EDCEB3091CF1DF7B156D5B495A4ADE1CFBA77641134F61B0E0940121C436C8

- - -
**Fransa:**

Anahtar kasası BYOK araç France.zip

5C9D1F3E4125B0C09E9F60897C9AE3A8B4CB0E7D13A14F3EDBD280128F8FE7DF

- - -
**Birleşik Krallık:**

Anahtar kasası BYOK araç UnitedKingdom.zip

432746BD0D3176B708672CCFF19D6144FCAA9E5EB29BB056489D3782B3B80849

- - -

Azure PowerShell oturumunuzda, indirilen BYOK araç bütünlüğünü doğrulamak için kullanın [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) cmdlet'i.

   ```powershell
   Get-FileHash KeyVault-BYOK-Tools-*.zip
   ```

Araç takımı içerir:

* İle başlayan bir ada sahip olan bir anahtar değişim anahtarı (KEK) paketi **BYOK-KEK - pkg-.**
* İle başlayan bir ada sahip olan bir güvenlik Dünyası paketi **BYOK-SecurityWorld - pkg-.**
* Adlı python betiğini **verifykeypackage.py.**
* Adlı bir komut satırı yürütülebilir dosyası **KeyTransferRemote.exe** ve ilişkili DLL'ler.
* Bir görsel C++ adlı yeniden dağıtılabilir paketi **vcredist_x64.exe.**

Paketi bir USB sürücüye veya başka bir taşınabilir depolama kopyalayın.

## <a name="step-2-prepare-your-disconnected-workstation"></a>2\. adım: Bağlantısı kesilmiş iş istasyonunuzu hazırlama

Bu ikinci adım için bir ağa (İnternet'e veya iç ağınıza) bağlı olmayan bir iş istasyonunda aşağıdaki yordamları gerçekleştirin.

### <a name="step-21-prepare-the-disconnected-workstation-with-ncipher-nshield-hsm"></a>2\.1. adım: Bağlantısı kesilmiş iş istasyonunuzu nCipher nShield HSM ile hazırlama

Bir Windows bilgisayara nCipher destek yazılımını yükleyin ve ardından o bilgisayara nCipher nShield HSM ekleyin.

NCipher araçları yolunuzda olduğundan emin olun ( **%nfast_home%\bin**). Örneğin, aşağıdaki komutu yazın:

  ```cmd
  set PATH=%PATH%;"%nfast_home%\bin"
  ```

Daha fazla bilgi için HSM nShield ile kullanıcı kılavuzuna bakın.

### <a name="step-22-install-the-byok-toolset-on-the-disconnected-workstation"></a>2\.2. adım: BYOK araç takımını, bağlantısı kesilmiş iş istasyonunda yükleyin

USB sürücü veya başka bir taşınabilir depolama BYOK araç takımı paketini kopyalayın ve ardından aşağıdakileri yapın:

1. Dosyaları indirilen paketteki herhangi bir klasöre ayıklayın.
2. Bu klasörden vcredist_x64.exe çalıştırın.
3. Yönergeleri, Visual Studio 2013 için Visual C++ çalışma zamanı bileşenlerini yüklemeyi izleyin.

## <a name="step-3-generate-your-key"></a>3\. adım: Anahtarınızı

Bu üçüncü adım için bağlantısı kesilmiş iş istasyonunda aşağıdaki yordamları gerçekleştirin. Bu adımı tamamlamak için HSM tedarikçinize başlatma modunda olması gerekir. 

### <a name="step-31-change-the-hsm-mode-to-i"></a>Adım 3.1: 'I' HSM modunu değiştirme

Modu değiştirmek için Edge, nCipher nShield kullanıyorsanız: 1. Gerekli modu vurgulamak için Modu düğmesini kullanın. 2. Birkaç saniye içinde basın ve Temizle düğmesine birkaç saniye basılı tutun. Modu değişirse, yeni modun LED yanıp durdurur ve aydınlatılmış kalır. Durum LED birkaç saniye düzensiz flash ve düzenli olarak cihaz hazır olduğunda, ardından yanıp. Aksi takdirde cihaz kalır LED uygun moduyla geçerli modunda aydınlatma.

### <a name="step-32-create-a-security-world"></a>Adım 3.2: Bir güvenlik Dünyası oluşturma

Bir komut istemi başlatın ve nCipher yeni dünya programını çalıştırın.

   ```cmd
    new-world.exe --initialize --cipher-suite=DLf3072s256mRijndael --module=1 --acs-quorum=2/3
   ```

Bu programın oluşturduğu bir **güvenlik Dünyası** % NFAST_KMDATA%\local\world, C:\ProgramData\nCipher\Key Management Data\local klasörüne karşılık gelen dosya. Çekirdek için farklı değerler kullanabilirsiniz, ancak örneğimizde her biri için biri üç boş kart ve PIN girmeniz istenir. Ardından, herhangi iki kart güvenlik dünyasına tam erişim verin. Bu kartların haline **yönetici kart Seti** yeni güvenlik Dünyası için.

> [!NOTE]
> HSM'NİZDE yeni şifre paketi DLf3072s256mRijndael desteklemiyorsa değiştirebilirsiniz--şifre paketi DLf3072s256mRijndael şifre paketi ile--= DLf1024s160mRijndael =

Ardından şunları yapın:

* Dünya dosyasının yedeğini alın. Güvenli ve dünya dosyasını, yönetici kartlarını ve PIN kodlarını korumak ve tek başına birden fazla kart erişimi olduğundan emin olun.

### <a name="step-33-change-the-hsm-mode-to-o"></a>Adım 3.3: HSM moduna '

Modu değiştirmek için Edge, nCipher nShield kullanıyorsanız: 1. Gerekli modu vurgulamak için Modu düğmesini kullanın. 2. Birkaç saniye içinde basın ve Temizle düğmesine birkaç saniye basılı tutun. Modu değişirse, yeni modun LED yanıp durdurur ve aydınlatılmış kalır. Durum LED birkaç saniye düzensiz flash ve düzenli olarak cihaz hazır olduğunda, ardından yanıp. Aksi takdirde cihaz kalır LED uygun moduyla geçerli modunda aydınlatma.

### <a name="step-34-validate-the-downloaded-package"></a>Adım 3.4: İndirilen paketi doğrulama

Bu adım isteğe bağlıdır ancak aşağıdakileri doğrulayabilmeniz böylece önerilir:

* Araç takımında yer anahtar değişim anahtarı bir orijinal nCipher nShield HSM oluşturuldu.
* Araç takımında yer güvenlik Dünyası karması, orijinal nCipher nShield HSM oluşturuldu.
* Anahtar değişim anahtarı aktarılamaz.

> [!NOTE]
> İndirilen paketi doğrulamak için HSM, açık bağlanmalıdır ve üzerindeki güvenlik Dünyası (örneğin, yeni oluşturduğunuz bir tane) sahip olması gerekir.

İndirilen paketi doğrulamak için:

1. Azure örneği veya coğrafi bölgede bağlı olarak aşağıdakilerden birini yazarak verifykeypackage.py betiği çalıştırın:

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
   * Güney Afrika için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-SA-1 -w BYOK-SecurityWorld-pkg-SA-1
   * BAE için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-UAE-1 -w BYOK-SecurityWorld-pkg-UAE-1
   * Avustralya için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
   * İçin [Azure kamu](https://azure.microsoft.com/features/gov/), Azure ABD kamu örneğini kullanır:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
   * ABD kamu savunma Bakanlığı için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USDOD-1 -w BYOK-SecurityWorld-pkg-USDOD-1
   * Kanada:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
   * Almanya için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
   * Hindistan için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
   * Fransa için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-FRANCE-1 -w BYOK-SecurityWorld-pkg-FRANCE-1
   * Birleşik Krallık için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-UK-1 -w BYOK-SecurityWorld-pkg-UK-1

     > [!TIP]
     > NCipher nShield yazılımı, %NFAST_HOME%\python\bin python içerir
     >
     >
2. Doğrulama başarılı gösteren aşağıdaki gördüğünüzü onaylayın: **Sonuç: BAŞARILI**

Bu betik nShield kök anahtarı kadar doğru imzalayan zincirini doğrular. Bu kök anahtarın karması betiğe ekli olduğunu ve değeri **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**. Ayrıca bu değeri ayrı olarak ederek doğrulayabilirsiniz [nCipher Web sitesi](https://www.ncipher.com/products/key-management/cloud-microsoft-azure/validation).

Şimdi yeni bir anahtar oluşturmaya hazırsınız.

### <a name="step-35-create-a-new-key"></a>Adım 3.5: Yeni anahtar oluştur

NCipher nShield kullanarak bir anahtar oluşturmak **generatekey** program.

Anahtarı oluşturmak için aşağıdaki komutu çalıştırın:

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

Bu komutu çalıştırdığınızda, aşağıdaki yönergeleri kullanın:

* Parametre *korumak* değerine ayarlanmalıdır **Modülü**gösterildiği gibi. Bu, modül korumalı bir anahtar oluşturur. BYOK araç takımı, OCS korumalı anahtarları desteklemez.
* Değiştirin *contosokey* için **ident** ve **plainname** ile herhangi bir dize değeri. Yönetim ek yüklerini en aza indirmek ve hataları riskini azaltmak için her ikisi için aynı değeri kullanmanızı öneririz. **İdent** değer yalnızca sayı, tire ve küçük harf içermelidir.
* Bu örnekte pubexp (varsayılan) boş kaldı, ancak belirli bir değer belirtebilirsiniz. Daha fazla bilgi için [nCipher belgeleri.](https://www.ncipher.com/resources/solution-briefs/protect-sensitive-data-rest-and-use-across-premises-and-azure-based)

Bu komut, %NFAST_KMDATA%\local klasörünüzde adı ile bir simgeleştirilmiş anahtar dosyası oluşturur. **key_simple_** çizgidir **ident** komutu belirtildi. Örneğin: **key_simple_contosokey**. Bu dosya şifreli bir anahtar içerir.

Bu simgeleştirilmiş anahtar dosyasını güvenli bir yere yedekleyin.

> [!IMPORTANT]
> Anahtarınızı daha sonra Azure anahtar Kasası'na aktardığınızda, anahtarınızı ve güvenlik dünyanızı güvenle yedeklemeniz çok önemli hale gelir için Microsoft bu anahtarı size geri dışarı aktaramazsınız. İlgili kişi [nCipher](https://www.ncipher.com/about-us/contact-us) rehberlik ve anahtarınızı yedekleme için en iyi uygulamalar için.
>


Anahtarınızı Azure anahtar Kasası'na aktarmak artık hazırsınız.

## <a name="step-4-prepare-your-key-for-transfer"></a>4\. Adım: Kiracı anahtarınızı aktarım için hazırlama

Bu dördüncü adım için bağlantısı kesilmiş iş istasyonunda aşağıdaki yordamları gerçekleştirin.

### <a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a>4\.1. adım: Sınırlı izinlerle anahtarınızın bir kopyasını oluşturma

Yeni bir komut istemi açın ve burada BYOK ZIP dosyasının sıkıştırması açılan geçerli dizine geçin. Anahtarınızı izinleri bir komut istemi'nden azaltmak için Azure örneği veya coğrafi bölgede bağlı olarak aşağıdakilerden birini çalıştırın:

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
* Güney Afrika için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-SA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-SA-1
* BAE için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-UAE-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-UAE-1
* Avustralya için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
* İçin [Azure kamu](https://azure.microsoft.com/features/gov/), Azure ABD kamu örneğini kullanır:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
* ABD kamu savunma Bakanlığı için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1
* Kanada:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
* Almanya için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
* Hindistan için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1
* Fransa için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-FRANCE-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-FRANCE-1
* Birleşik Krallık için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-UK-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-UK-1

Bu komutu çalıştırdığınızda, değiştirin *contosokey* ile aynı belirtilen değere **adım 3.5: Yeni anahtar oluştur** gelen [anahtarınızı](#step-3-generate-your-key) adım.

Güvenlik Dünyası yönetim kartlarınızı takmanız istenir.

Komut tamamlandığında, gördüğünüz **sonucu: Başarı** ve anahtarınızın sınırlı izinlere sahip kopyası, key_xferacıd_ adlı dosyada olan\<contosokey >.

İnceler ACL'leri nCipher nShield Araçları'nı kullanarak aşağıdaki komutları kullanarak:

* aclprint.PY:

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
* kmfile-dump.exe:

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
  Bu komutları çalıştırdıktan sonra belirttiğiniz değerle contosokey değiştirin **adım 3.5: Yeni anahtar oluştur** gelen [anahtarınızı](#step-3-generate-your-key) adım.

### <a name="step-42-encrypt-your-key-by-using-microsofts-key-exchange-key"></a>4\.2. adım: Microsoft'un anahtar değişim anahtarını kullanarak anahtarınızı şifreleme

Azure örneği veya coğrafi bölgede bağlı olarak aşağıdaki komutlardan birini çalıştırın:

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
* Güney Afrika için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-SA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-SA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* BAE için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-UAE-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-UAE-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Avustralya için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* İçin [Azure kamu](https://azure.microsoft.com/features/gov/), Azure ABD kamu örneğini kullanır:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* ABD kamu savunma Bakanlığı için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Kanada:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Almanya için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Hindistan için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Fransa için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-France-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-France-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Birleşik Krallık için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-UK-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-UK-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey

Bu komutu çalıştırdığınızda, aşağıdaki yönergeleri kullanın:

* Değiştirin *contosokey* anahtarı oluşturmak için kullandığınız tanımlayıcıyla **adım 3.5: Yeni anahtar oluştur** gelen [anahtarınızı](#step-3-generate-your-key) adım.
* Değiştirin *Subscriptionıd* anahtar kasanıza içeren Azure abonelik kimliği. Bu değer aldığınız önceden, **adım 1.2: Azure abonelik Kimliğinizi alın** gelen [İnternet'e bağlı iş istasyonunuzu hazırlama](#step-1-prepare-your-internet-connected-workstation) adım.
* Değiştirin *ContosoFirstHSMKey* çıktı dosyanızın adı için kullanılan bir etikete sahip.

Başarıyla tamamlandığında, bu görüntüler **sonucu: Başarı** ve geçerli klasörde şu ada sahip yeni bir dosya yok: KeyTransferPackage -*ContosoFirstHSMkey*.byok

### <a name="step-43-copy-your-key-transfer-package-to-the-internet-connected-workstation"></a>4\.3. adım: Anahtar aktarım paketinizi İnternet'e bağlı iş istasyonuna kopyalama

Çıktı dosyasını (KeyTransferPackage-ContosoFirstHSMkey.byok) önceki adımdaki, İnternet'e bağlı iş istasyonunuzu kopyalamak için bir USB sürücü veya başka bir taşınabilir depolama kullanın.

## <a name="step-5-transfer-your-key-to-azure-key-vault"></a>5\. Adım: Azure Key Vault'a anahtar aktarma

Bu son adım İnternet'e bağlı iş istasyonunda, kullanın [Ekle AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey) cmdlet'i için Azure Key Vault HSM'SİNDE bağlantısı kesilmiş iş istasyonundan kopyaladığınız anahtar aktarma paketini karşıya yüklemek için:

   ```powershell
        Add-AzKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\KeyTransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'
   ```

Karşıya yükleme başarılı olursa, gördüğünüz eklediğiniz anahtar özelliklerini görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

Bu HSM korumalı anahtar, anahtar Kasası'nda artık kullanabilirsiniz. Daha fazla bilgi için bkz. Bu fiyat ve özellik [karşılaştırma](https://azure.microsoft.com/pricing/details/key-vault/).
