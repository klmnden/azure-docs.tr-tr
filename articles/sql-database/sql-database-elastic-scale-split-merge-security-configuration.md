---
title: Ayırma-birleştirme güvenliği yapılandırma | Microsoft Docs
description: X409 ayarlamak için esnek ölçeklendirme ayırma/birleştirme hizmetiyle şifreleme için sertifikalar.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: VanMSFT
ms.author: vanto
ms.reviewer: sstein
manager: craigg
ms.date: 12/18/2018
ms.openlocfilehash: 7ca7e653cc42323f4313ef955de40416154b4ecf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60335233"
---
# <a name="split-merge-security-configuration"></a>Ayırma-birleştirme güvenliği yapılandırma

Ayırma/birleştirme hizmetini kullanmak için doğru güvenlik yapılandırmanız gerekir. Hizmet, Microsoft Azure SQL veritabanı'nın esnek ölçeklendirme özelliği bir parçasıdır. Daha fazla bilgi için [esnek ölçek bölme ve birleştirme Service Öğreticisi](sql-database-elastic-scale-configure-deploy-split-and-merge.md).

## <a name="configuring-certificates"></a>Sertifikaları yapılandırma

Sertifikalar iki şekilde yapılandırılır. 

1. [SSL sertifikası yapılandırma](#to-configure-the-ssl-certificate)
2. [İstemci sertifikaları yapılandırma](#to-configure-client-certificates) 

## <a name="to-obtain-certificates"></a>Sertifika edinme

Ortak sertifika yetkilileri (CA) ya da sertifika elde edilebilir [Windows sertifika hizmeti](https://msdn.microsoft.com/library/windows/desktop/aa376539.aspx). Bu sertifikaları almak için tercih edilen yöntemlerdir.

Bu seçenek kullanılabilir değilse, oluşturabileceğiniz **otomatik olarak imzalanan sertifikalar**.

## <a name="tools-to-generate-certificates"></a>Sertifikalarını oluşturmak için Araçlar

* [makecert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx)
* [pvk2pfx.exe](https://msdn.microsoft.com/library/windows/hardware/ff550672.aspx)

### <a name="to-run-the-tools"></a>Araçları çalıştırmak için

* Bir geliştirici komut isteminden Visual Studios için bkz: [Visual Studio komut istemi](https://msdn.microsoft.com/library/ms229859.aspx) 
  
    Yüklü değilse, şuraya gidin:
  
        %ProgramFiles(x86)%\Windows Kits\x.y\bin\x86 
* WDK gelen alma [Windows 8.1: Yükleme setleri ve araçları](https://msdn.microsoft.com/windows/hardware/gg454513#drivers)

## <a name="to-configure-the-ssl-certificate"></a>SSL sertifikası yapılandırma

Bir SSL sertifikası, iletişimi şifrelemek ve sunucu kimlik doğrulaması için gereklidir. Aşağıdaki üç senaryo en uygun seçin ve tüm adımları yürütün:

### <a name="create-a-new-self-signed-certificate"></a>Yeni bir otomatik olarak imzalanan sertifika oluştur

1. [Otomatik olarak imzalanan bir sertifika oluşturun](#create-a-self-signed-certificate)
2. [İçin otomatik olarak imzalanan SSL Sertifika PFX dosyasını oluşturma](#create-pfx-file-for-self-signed-ssl-certificate)
3. [Bulut hizmeti için SSL sertifikasını karşıya yükle](#upload-ssl-certificate-to-cloud-service)
4. [Hizmet yapılandırma dosyasında SSL sertifikasını güncelleştirme](#update-ssl-certificate-in-service-configuration-file)
5. [SSL sertifika yetkilisi içeri aktarma](#import-ssl-certification-authority)

### <a name="to-use-an-existing-certificate-from-the-certificate-store"></a>Sertifika depolama alanından mevcut bir sertifikayı kullanmak için
1. [Sertifika Store ' SSL sertifikasını dışarı aktarma](#export-ssl-certificate-from-certificate-store)
2. [Bulut hizmeti için SSL sertifikasını karşıya yükle](#upload-ssl-certificate-to-cloud-service)
3. [Hizmet yapılandırma dosyasında SSL sertifikasını güncelleştirme](#update-ssl-certificate-in-service-configuration-file)

### <a name="to-use-an-existing-certificate-in-a-pfx-file"></a>Mevcut bir sertifikayı bir PFX dosyasında kullanmak için
1. [Bulut hizmeti için SSL sertifikasını karşıya yükle](#upload-ssl-certificate-to-cloud-service)
2. [Hizmet yapılandırma dosyasında SSL sertifikasını güncelleştirme](#update-ssl-certificate-in-service-configuration-file)

## <a name="to-configure-client-certificates"></a>İstemci sertifikaları yapılandırma
İstemci sertifikaları, istekleri hizmete kimlik doğrulaması için gereklidir. Aşağıdaki üç senaryo en uygun seçin ve tüm adımları yürütün:

### <a name="turn-off-client-certificates"></a>İstemci sertifikaları Kapat
1. [İstemci sertifika tabanlı kimlik doğrulamasını devre dışı Aç](#turn-off-client-certificate-based-authentication)

### <a name="issue-new-self-signed-client-certificates"></a>Yeni otomatik olarak imzalanan istemci sertifikaları
1. [Otomatik olarak imzalanan bir sertifika yetkilisi oluşturma](#create-a-self-signed-certification-authority)
2. [Bulut hizmeti için CA sertifikasını karşıya yükle](#upload-ca-certificate-to-cloud-service)
3. [Hizmet yapılandırma dosyasında CA sertifikasını güncelleştir](#update-ca-certificate-in-service-configuration-file)
4. [İstemci sertifikaları](#issue-client-certificates)
5. [İstemci sertifikaları için PFX dosyaları oluşturma](#create-pfx-files-for-client-certificates)
6. [İstemci sertifika İçeri Aktar](#import-client-certificate)
7. [İstemci sertifikası parmak izleri kopyalayın](#copy-client-certificate-thumbprints)
8. [Hizmet yapılandırma dosyasında izin verilen istemcilerini yapılandırma](#configure-allowed-clients-in-the-service-configuration-file)

### <a name="use-existing-client-certificates"></a>Mevcut istemci sertifikaları kullanın
1. [CA ortak anahtar bulunamadı](#find-ca-public-key)
2. [Bulut hizmeti için CA sertifikasını karşıya yükle](#upload-ca-certificate-to-cloud-service)
3. [Hizmet yapılandırma dosyasında CA sertifikasını güncelleştir](#update-ca-certificate-in-service-configuration-file)
4. [İstemci sertifikası parmak izleri kopyalayın](#copy-client-certificate-thumbprints)
5. [Hizmet yapılandırma dosyasında izin verilen istemcilerini yapılandırma](#configure-allowed-clients-in-the-service-configuration-file)
6. [İstemci sertifika iptal denetimi yapılandırma](#configure-client-certificate-revocation-check)

## <a name="allowed-ip-addresses"></a>İzin verilen IP adresleri
Hizmet uç noktalarına erişimi belirli IP adresleri aralığı için sınırlı olabilir.

## <a name="to-configure-encryption-for-the-store"></a>Store için şifreleme yapılandırmak için
Meta veri deposunda saklanan kimlik bilgilerini şifrelemek için bir sertifika gerekir. Aşağıdaki üç senaryo en uygun seçin ve tüm adımları yürütün:

### <a name="use-a-new-self-signed-certificate"></a>Yeni bir otomatik olarak imzalanan sertifika kullan
1. [Otomatik olarak imzalanan bir sertifika oluşturun](#create-a-self-signed-certificate)
2. [İçin otomatik imzalanan şifreleme sertifika PFX dosyasını oluşturma](#create-pfx-file-for-self-signed-ssl-certificate)
3. [Bulut hizmeti için şifreleme sertifikasını karşıya yükle](#upload-encryption-certificate-to-cloud-service)
4. [Hizmet yapılandırma dosyasında şifreleme sertifikasını güncelleştir](#update-encryption-certificate-in-service-configuration-file)

### <a name="use-an-existing-certificate-from-the-certificate-store"></a>Mevcut bir sertifikayı sertifika deposundan kullanın
1. [Sertifika Store ' şifreleme sertifikasını dışarı aktarma](#export-encryption-certificate-from-certificate-store)
2. [Bulut hizmeti için şifreleme sertifikasını karşıya yükle](#upload-encryption-certificate-to-cloud-service)
3. [Hizmet yapılandırma dosyasında şifreleme sertifikasını güncelleştir](#update-encryption-certificate-in-service-configuration-file)

### <a name="use-an-existing-certificate-in-a-pfx-file"></a>Mevcut bir sertifikayı bir PFX dosyasında kullanmak
1. [Bulut hizmeti için şifreleme sertifikasını karşıya yükle](#upload-encryption-certificate-to-cloud-service)
2. [Hizmet yapılandırma dosyasında şifreleme sertifikasını güncelleştir](#update-encryption-certificate-in-service-configuration-file)

## <a name="the-default-configuration"></a>Varsayılan yapılandırma
Varsayılan yapılandırma, HTTP uç noktasına tüm erişimi engeller. Bu istekler Bu uç noktaları için veritabanı kimlik bilgileri gibi hassas bilgileri gerçekleştirmek beri önerilen, ayardır.
Tüm erişim HTTPS uç noktasına varsayılan yapılandırmasını sağlar. Bu ayar daha da kısıtlanabilir.

### <a name="changing-the-configuration"></a>Yapılandırmayı değiştirme
Geçerli erişim denetim kuralları ve uç nokta grubunun yapılandırılmış  **\<EndpointAcls >** konusundaki **hizmet yapılandırma dosyasını**.

```xml
<EndpointAcls>
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpIn" accessControl="DenyAll" />
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="AllowAll" />
</EndpointAcls>
```

Bir erişim denetimi grubu kurallarında, yapılandırılmış bir \<AccessControl name = "" > hizmet yapılandırma dosyasının. 

Biçim, ağ erişim denetimi listelerini belgelerinde açıklanmıştır.
Örneğin, yalnızca IP HTTPS uç noktasına erişmek için 100.100.0.0 için 100.100.255.255 aralığında izin vermek için kuralları şöyle görünür:

```xml
<AccessControl name="Retricted">
    <Rule action="permit" description="Some" order="1" remoteSubnet="100.100.0.0/16"/>
    <Rule action="deny" description="None" order="2" remoteSubnet="0.0.0.0/0" />
</AccessControl>
<EndpointAcls>
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="Restricted" />
</EndpointAcls>
```

## <a name="denial-of-service-prevention"></a>Önleme hizmet reddi
Algılama ve hizmet reddi saldırılarını önlemek için desteklenen iki farklı mekanizma vardır:

* Uzak ana bilgisayar başına eşzamanlı istek sayısını sınırlayın (varsayılan olarak kapalıdır)
* Uzak ana bilgisayar başına erişim oranını sınırlamak (üzerinde varsayılan olarak)

Bunlar daha fazla IIS dinamik IP güvenlik konusunda belgelenen özellikleri temel alır. Ne zaman bu yapılandırmasının değiştirilmesi aşağıdaki etmenlere dikkat:

* Proxy ve ağ adresi çevirisi cihazlar üzerinde uzak konak bilgilerini davranışı
* Her isteğin web rolünde herhangi bir kaynağa (örneğin yüklenirken komut dosyaları, görüntüler vb.) olarak kabul edilir

## <a name="restricting-number-of-concurrent-accesses"></a>Eş zamanlı erişimi sayısını sınırlandırma
Bu davranış yapılandırdığınız ayarlar şunlardır:

```xml
<Setting name="DynamicIpRestrictionDenyByConcurrentRequests" value="false" />
<Setting name="DynamicIpRestrictionMaxConcurrentRequests" value="20" />
```

DynamicIpRestrictionDenyByConcurrentRequests bu korumayı etkinleştirmek için true olarak değiştirin.

## <a name="restricting-rate-of-access"></a>Erişimi kısıtlama oranı
Bu davranış yapılandırdığınız ayarlar şunlardır:

```xml
<Setting name="DynamicIpRestrictionDenyByRequestRate" value="true" />
<Setting name="DynamicIpRestrictionMaxRequests" value="100" />
<Setting name="DynamicIpRestrictionRequestIntervalInMilliseconds" value="2000" />
```

## <a name="configuring-the-response-to-a-denied-request"></a>Reddedilen bir isteğin yanıtını yapılandırma
Reddedilen bir isteğin yanıtını aşağıdaki ayarı yapılandırır:

```xml
<Setting name="DynamicIpRestrictionDenyAction" value="AbortRequest" />
```

Desteklenen diğer değerler için dinamik IP Güvenlik IIS'de belgelere bakın.

## <a name="operations-for-configuring-service-certificates"></a>Hizmet sertifikaları yapılandırma işlemleri
Bu konu, yalnızca başvuru amacıyla kullanılır. Bölümünde açıklanan yapılandırma adımlarını izleyin:

* SSL sertifikası yapılandırma
* İstemci sertifikaları yapılandırma

## <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma
Yürütün:

    makecert ^
      -n "CN=myservice.cloudapp.net" ^
      -e MM/DD/YYYY ^
      -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1" ^
      -a sha256 -len 2048 ^
      -sv MySSL.pvk MySSL.cer

Özelleştirme için:

* -n ile hizmet URL'si. Joker karakter ("CN = * .cloudapp .net") ve diğer adları ("CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net") desteklenir.
* -e sertifika sona erme tarihi ile güçlü bir parola oluşturmak ve onu istendiğinde belirtin.

## <a name="create-pfx-file-for-self-signed-ssl-certificate"></a>Otomatik olarak imzalanan SSL sertifikası PFX dosyasını oluşturma
Yürütün:

        pvk2pfx -pvk MySSL.pvk -spc MySSL.cer

Parolayı girin ve ardından bu seçeneklere sahip sertifika verme:

* Evet, özel anahtarı dışarı aktar
* Tüm genişletilmiş özellikleri Dışarı Aktar

## <a name="export-ssl-certificate-from-certificate-store"></a>Sertifika deposundan SSL sertifikasını dışarı aktarma
* Sertifika bulunamadı
* Tıklayın Eylemler -> tüm görevler -> dışarı aktar...
* Sertifikayı dışarı aktarma bir. PFX dosyası olan bu seçenekleri:
  * Evet, özel anahtarı dışarı aktar
  * Mümkünse sertifika yolundaki tüm sertifikaları dahil et * tüm genişletilmiş özellikleri Dışarı Aktar

## <a name="upload-ssl-certificate-to-cloud-service"></a>Bulut hizmeti için SSL sertifikasını karşıya yükle
Karşıya yükleme, var olan sertifika veya oluşturulabilir. SSL anahtar çifti ile PFX dosyası:

* Özel anahtar bilgisi koruyan parolayı girin

## <a name="update-ssl-certificate-in-service-configuration-file"></a>Hizmet yapılandırma dosyasında SSL sertifikasını güncelleştirme
Bulut hizmeti için karşıya yüklenen sertifikanın parmak izine sahip hizmet yapılandırma dosyasında aşağıdaki ayarı parmak izi değerini güncelleştirin:

    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="import-ssl-certification-authority"></a>SSL sertifika yetkilisi içeri aktarma
Tüm hesap / hizmetiyle iletişim kuracak makinede aşağıdaki adımları izleyin:

* Çift tıklayın. Windows Gezgini'nde CER dosyası
* Sertifika iletişim kutusunda, sertifikayı yükle düğmesine...
* Güvenilen Kök Sertifika Yetkilileri deposuna sertifika İçeri Aktar

## <a name="turn-off-client-certificate-based-authentication"></a>İstemci sertifika tabanlı kimlik doğrulamasını devre dışı Aç
Yalnızca istemci sertifika tabanlı kimlik doğrulaması desteklenir ve başka mekanizmalar karşılandığından (örneğin, Microsoft Azure sanal ağı) sürece devre dışı bırakılması hizmet uç noktalarına erişimine izin verir.

Bu ayarlar, hizmet yapılandırma dosyasında özelliği devre dışı bırakmak için false değiştirin:

```xml
<Setting name="SetupWebAppForClientCertificates" value="false" />
<Setting name="SetupWebserverForClientCertificates" value="false" />
```

Ardından, CA sertifikası ayarı SSL sertifikası olarak aynı parmak izini kopyalayın:

```xml
<Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />
```

## <a name="create-a-self-signed-certification-authority"></a>Otomatik olarak imzalanan bir sertifika yetkilisi oluşturma
Bir sertifika yetkilisi olarak davranacak şekilde otomatik olarak imzalanan bir sertifika oluşturmak için aşağıdaki adımları uygulayın:

    makecert ^
    -n "CN=MyCA" ^
    -e MM/DD/YYYY ^
     -r -cy authority -h 1 ^
     -a sha256 -len 2048 ^
      -sr localmachine -ss my ^
      MyCA.cer

Bunu özelleştirmek için

* -e ile sertifika sona erme tarihi

## <a name="find-ca-public-key"></a>CA ortak anahtar bulunamadı
Tüm istemci sertifikaları hizmet tarafından güvenilen bir sertifika yetkilisi tarafından verilmiş olmalıdır. İstemci kimlik doğrulaması için bulut hizmetine yüklemek için kullanılacak sertifikaları veren sertifika yetkilisine genel anahtarını bulun.

Genel anahtar dosyasıyla kullanılamıyorsa sertifika deposundan dışarı aktarın:

* Sertifika bulunamadı
  * Aynı sertifika yetkilisi tarafından verilmiş bir istemci sertifikası Ara
* Sertifikaya çift tıklayın.
* Sertifika iletişim kutusunda sertifika yolu sekmesini seçin.
* Yolun CA girişi çift tıklatın.
* Sertifika Özellikleri Not.
* Kapat **sertifika** iletişim.
* Sertifika bulunamadı
  * Yukarıda belirtilen CA'yı arayın.
* Tıklayın Eylemler -> tüm görevler -> dışarı aktar...
* Sertifikayı dışarı aktarma bir. CER bu seçenekleri:
  * **Hayır, özel anahtarı verme**
  * Mümkünse sertifika yolundaki tüm sertifikaları Ekle.
  * Tüm genişletilmiş özellikleri dışarı aktarın.

## <a name="upload-ca-certificate-to-cloud-service"></a>Bulut hizmeti için CA sertifikasını karşıya yükle
Karşıya yükleme, var olan sertifika veya oluşturulabilir. CER dosyası CA ortak anahtara sahip.

## <a name="update-ca-certificate-in-service-configuration-file"></a>Hizmet yapılandırma dosyasında güncelleştirme CA sertifikası
Bulut hizmeti için karşıya yüklenen sertifikanın parmak izine sahip hizmet yapılandırma dosyasında aşağıdaki ayarı parmak izi değerini güncelleştirin:

```xml
<Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />
```

Aşağıdaki ayarı değerini aynı parmak izine sahip güncelleştirin:

```xml
<Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />
```

## <a name="issue-client-certificates"></a>İstemci sertifikaları
Her hizmete erişim yetkisi verilen kendi özel kullanım için bir istemci sertifikasına sahip olmalıdır ve özel anahtarıyla korumak için kendi güçlü bir parola seçmeniz gerekir. 

Aşağıdaki adımları otomatik olarak imzalanan sertifika burada oluşturulan ve depolanan aynı makinede yürütülmelidir:

    makecert ^
      -n "CN=My ID" ^
      -e MM/DD/YYYY ^
      -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.2" ^
      -a sha256 -len 2048 ^
      -in "MyCA" -ir localmachine -is my ^
      -sv MyID.pvk MyID.cer

Özelleştirme:

* Bu sertifika ile kimlik doğrulaması yapılacak istemciye ait bir kimlik ile -n
* -e ile sertifika sona erme tarihi
* MyID.pvk ve bu istemci sertifikası için benzersiz dosya adları ile MyID.cer

Bu komut, oluşturulması ve bir kez kullanılması için bir parola sorar. Güçlü bir parola kullanın.

## <a name="create-pfx-files-for-client-certificates"></a>PFX dosyaları için istemci sertifikaları oluşturma
Her oluşturulan istemci sertifikası için yürütün:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Özelleştirme:

    MyID.pvk and MyID.cer with the filename for the client certificate

Parolayı girin ve ardından bu seçeneklere sahip sertifika verme:

* Evet, özel anahtarı dışarı aktar
* Tüm genişletilmiş özellikleri Dışarı Aktar
* Bu sertifika yayımlanmaktadır tek dışarı aktarma parolası seçmeniz gerekir

## <a name="import-client-certificate"></a>İstemci sertifika İçeri Aktar
Kendisi için bir istemci sertifikası verildi her hizmetiyle iletişim kurmak için kullanacağı makineleri içindeki anahtar çiftinden almanız gerekir:

* Çift tıklayın. Windows Gezgini'nde PFX dosyası
* İçeri aktarma sertifikayı Kişisel depolama ile en az bu seçeneği:
  * Seçili tüm genişletilmiş özellikleri Ekle

## <a name="copy-client-certificate-thumbprints"></a>İstemci sertifikası parmak izleri kopyalayın
Kendisi için bir istemci sertifikası verildi her bir hizmet yapılandırma dosyasına eklenir, sertifikanın parmak izini edinmek için şu adımları uygulamanız gerekir:

* Certmgr.exe çalıştırın
* Kişisel sekmesini seçin.
* Kimlik doğrulaması için kullanılacak istemci sertifikası'na çift tıklayın
* Ayrıntılar sekmesi açılır sertifikası iletişim kutusunda seçin
* Tümünü Göster görüntüleme emin olun
* Parmak izi listesinde adlı bir alan seçin
* Parmak izi değerini kopyalayın
  * İlk basamak önünde görünür olmayan Unicode karakterleri silin
  * Tüm boşlukları Sil

## <a name="configure-allowed-clients-in-the-service-configuration-file"></a>Hizmet yapılandırma dosyasında verilen istemcilerini yapılandırma
Hizmete erişim verilen istemci sertifikaların parmak izleriyle virgülle ayrılmış listesiyle birlikte hizmet yapılandırma dosyasında aşağıdaki ayarın değerini güncelleştirin:

```xml
<Setting name="AllowedClientCertificateThumbprints" value="" />
```

## <a name="configure-client-certificate-revocation-check"></a>İstemci sertifika iptal denetimi yapılandırma
Varsayılan ayar, istemcinin sertifika iptal durumunu için sertifika yetkilisi ile denetlemez. İstemci sertifikaları veren sertifika yetkilisi tür denetimler destekliyorsa denetimleri etkinleştirmek için aşağıdaki ayarı X509RevocationMode numaralandırmada tanımlanan değerlerden biriyle değiştirin:

```xml
<Setting name="ClientCertificateRevocationCheck" value="NoCheck" />
```

## <a name="create-pfx-file-for-self-signed-encryption-certificates"></a>Otomatik olarak imzalanan şifreleme sertifikaları PFX dosyasını oluşturma
Bir şifreleme sertifikası için yürütün:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Özelleştirme:

    MyID.pvk and MyID.cer with the filename for the encryption certificate

Parolayı girin ve ardından bu seçeneklere sahip sertifika verme:

* Evet, özel anahtarı dışarı aktar
* Tüm genişletilmiş özellikleri Dışarı Aktar
* Bulut hizmeti için sertifika karşıya yüklenirken parolaya ihtiyaç duyacaksınız.

## <a name="export-encryption-certificate-from-certificate-store"></a>Şifreleme sertifikası, sertifika deposundan dışarı aktarma
* Sertifika bulunamadı
* Tıklayın Eylemler -> tüm görevler -> dışarı aktar...
* Sertifikayı dışarı aktarma bir. PFX dosyası olan bu seçenekleri: 
  * Evet, özel anahtarı dışarı aktar
  * Mümkünse sertifika yolundaki tüm sertifikaları dahil et 
* Tüm genişletilmiş özellikleri Dışarı Aktar

## <a name="upload-encryption-certificate-to-cloud-service"></a>Bulut hizmeti için şifreleme sertifikasını karşıya yükle
Karşıya yükleme, var olan sertifika veya oluşturulabilir. PFX dosyası şifreleme anahtar çifti ile:

* Özel anahtar bilgisi koruyan parolayı girin

## <a name="update-encryption-certificate-in-service-configuration-file"></a>Hizmet yapılandırma dosyasında şifreleme sertifikasını güncelleştir
Bulut hizmeti için karşıya yüklenen sertifikanın parmak izi ile parmak izi değerini hizmet yapılandırma dosyasında aşağıdaki ayarlardan birini güncelleştirin:

```xml
<Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
```

## <a name="common-certificate-operations"></a>Ortak sertifika işlemleri
* SSL sertifikası yapılandırma
* İstemci sertifikaları yapılandırma

## <a name="find-certificate"></a>Sertifika bulunamadı
Şu adımları uygulayın:

1. MMC.exe çalıştırın.
2. Dosya -> Ekle/Kaldır ek bileşenini...
3. Seçin **sertifikaları**.
4. **Ekle**'ye tıklayın.
5. Sertifika depolama konumu seçin.
6. **Son**'a tıklayın.
7. **Tamam** düğmesine tıklayın.
8. Genişletin **sertifikaları**.
9. Sertifika deposu düğümünü genişletin.
10. Sertifika alt düğümünü genişletin.
11. Listeden bir sertifika seçin.

## <a name="export-certificate"></a>Sertifikayı dışarı aktarma
İçinde **Sertifika Verme Sihirbazı**:

1. **İleri**’ye tıklayın.
2. Seçin **Evet**, ardından **özel anahtarı dışarı**.
3. **İleri**’ye tıklayın.
4. İstenen çıkış dosya biçimini seçin.
5. İstenen seçenekleri denetleyin.
6. Denetleme **parola**.
7. Güçlü bir parola girin ve onaylayın.
8. **İleri**’ye tıklayın.
9. Yazın veya bir dosya adı sertifikanın depolanacağı yeri bulun (kullanan bir. PFX uzantısı).
10. **İleri**’ye tıklayın.
11. **Son**'a tıklayın.
12. **Tamam** düğmesine tıklayın.

## <a name="import-certificate"></a>Sertifikayı içeri aktar
Sertifika Alma Sihirbazı'nda:

1. Depolama konumu seçin.
   
   * Seçin **geçerli kullanıcının** yalnızca geçerli kullanıcı altında çalışan işlemler hizmete erişecek
   * Seçin **yerel makine** bu bilgisayardaki diğer işlemler hizmete erişecek olursa
2. **İleri**’ye tıklayın.
3. Bir dosyadan içe aktarılıyorsa, dosya yolunu doğrulayın.
4. İçe aktarılıyorsa bir. PFX dosyası:
   1. Özel anahtarını koruyan parolayı girin
   2. İçeri aktarma seçenekleri seçin
5. "Yerleştir" sertifikaları aşağıdaki depolama alanına seçin
6. **Gözat**’a tıklayın.
7. İstediğiniz depoyu seçin.
8. **Son**'a tıklayın.
   
   * Güvenilen kök sertifika yetkilisi deposunda seçildiyse, tıklayın **Evet**.
9. Tıklayın **Tamam** tüm iletişim Windows.

## <a name="upload-certificate"></a>Sertifikayı karşıya yükleme
[Azure portalında](https://portal.azure.com/)

1. Seçin **bulut Hizmetleri**.
2. Bulut hizmeti seçin.
3. Üst menüsünde **sertifikaları**.
4. Alt çubuğunda **karşıya**.
5. Sertifika dosyası seçin.
6. Bu doğruysa bir. PFX dosya, özel anahtarı parolasını girin.
7. Tamamlandığında, listedeki yeni girişi sertifika parmak izini kopyalayın.

## <a name="other-security-considerations"></a>Diğer güvenlik konuları
Bu belgede açıklanan SSL ayarları, HTTPS uç noktası kullanıldığında, hizmet ve istemcileri arasındaki iletişimi şifrelemek. Veritabanı erişimi için kimlik bilgilerini bu yana önemli olduğunu ve iletişime olabilecek diğer hassas bilgiler yer alır. Ancak, hizmet kimlik bilgilerini, Microsoft Azure aboneliğiniz meta veri depolama için sağlanan Microsoft Azure SQL veritabanı'nda kendi iç tablolar dahil olmak üzere, iç durumu devam ettiğini unutmayın. Bu veritabanı, aşağıdaki ayarı hizmet yapılandırma dosyanızdaki bir parçası olarak tanımlandı (. CSCFG dosyası): 

```xml
<Setting name="ElasticScaleMetadata" value="Server=…" />
```

Bu veritabanında depolanan kimlik bilgileri şifrelenir. Ancak, en iyi uygulama, hem web hem de çalışan roller hizmet dağıtımlarınıza güncel tutulur ve güvenli olarak hem de meta verileri veritabanı ve şifreleme ve şifre çözme depolanan kimlik bilgileri için kullanılan sertifikanın erişiminiz emin olun. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

