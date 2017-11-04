---
title: "Bölünmüş birleştirme güvenlik yapılandırması | Microsoft Docs"
description: "X409 ayarlamak esnek ölçek bölme/merge hizmetiyle şifreleme için sertifikalar."
metakeywords: Elastic Database certificates security
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: f9e89c57-61a0-484f-b787-82dae2349cb6
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: torsteng
ms.openlocfilehash: 94a4d5331aa2ed42a81e2e0bf890408f2db98fa7
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="split-merge-security-configuration"></a>Bölünmüş birleştirme güvenlik yapılandırması
Bölünmüş/Merge hizmetini kullanmak için güvenlik doğru şekilde yapılandırmanız gerekir. Hizmet Microsoft Azure SQL veritabanı'nın esnek ölçeklendirme özelliği bir parçasıdır. Daha fazla bilgi için bkz: [esnek ölçek bölme ve birleştirme hizmet öğretici](sql-database-elastic-scale-configure-deploy-split-and-merge.md).

## <a name="configuring-certificates"></a>Sertifikaları yapılandırma
Sertifikalar iki şekilde yapılandırılır. 

1. [SSL sertifikası yapılandırma](#to-configure-the-ssl-certificate)
2. [İstemci sertifikalarını yapılandırmak için](#to-configure-client-certificates) 

## <a name="to-obtain-certificates"></a>Sertifikaları almak için
Ortak sertifika yetkilileri (CA) ya da sertifika elde edilebilir [Windows sertifika hizmeti](http://msdn.microsoft.com/library/windows/desktop/aa376539.aspx). Bu sertifikaları almak için tercih edilen yöntemler şunlardır.

Bu seçenek mevcut değilse, oluşturabileceğiniz **otomatik olarak imzalanan sertifikalar**.

## <a name="tools-to-generate-certificates"></a>Sertifikalarını oluşturmak için Araçlar
* [MakeCert.exe](http://msdn.microsoft.com/library/bfsktky3.aspx)
* [Pvk2pfx.exe](http://msdn.microsoft.com/library/windows/hardware/ff550672.aspx)

### <a name="to-run-the-tools"></a>Araçları çalıştırmak için
* Bir geliştirici komut isteminden Visual stüdyoları için bkz: [Visual Studio komut istemi](http://msdn.microsoft.com/library/ms229859.aspx) 
  
    Yüklü değilse, aşağıdaki adrese gidin:
  
        %ProgramFiles(x86)%\Windows Kits\x.y\bin\x86 
* Gelen WDK almak [Windows 8.1: indirme setleri ve araçları](http://msdn.microsoft.com/windows/hardware/gg454513#drivers)

## <a name="to-configure-the-ssl-certificate"></a>SSL sertifikası yapılandırma
Bir SSL sertifikası, iletişimi şifrelemek ve sunucu kimlik doğrulaması için gereklidir. Aşağıdaki üç senaryodan en uygun seçin ve tüm adımları yürütün:

### <a name="create-a-new-self-signed-certificate"></a>Yeni bir otomatik olarak imzalanan sertifika oluşturma
1. [Otomatik olarak imzalanan sertifika oluşturma](#create-a-self-signed-certificate)
2. [PFX dosyası için otomatik olarak imzalanan SSL sertifika oluşturun.](#create-pfx-file-for-self-signed-ssl-certificate)
3. [Bulut hizmeti için SSL sertifikasını karşıya yükle](#upload-ssl-certificate-to-cloud-service)
4. [Hizmet yapılandırma dosyasında SSL sertifikasını güncelleştir](#update-ssl-certificate-in-service-configuration-file)
5. [SSL sertifika yetkilisi alma](#import-ssl-certification-authority)

### <a name="to-use-an-existing-certificate-from-the-certificate-store"></a>Varolan bir sertifikayı sertifika deposundan kullanmak için
1. [SSL sertifikası, sertifika deposundan dışarı aktarma](#export-ssl-certificate-from-certificate-store)
2. [Bulut hizmeti için SSL sertifikasını karşıya yükle](#upload-ssl-certificate-to-cloud-service)
3. [Hizmet yapılandırma dosyasında SSL sertifikasını güncelleştir](#update-ssl-certificate-in-service-configuration-file)

### <a name="to-use-an-existing-certificate-in-a-pfx-file"></a>Varolan bir sertifikayı bir PFX dosyası kullanmak için
1. [Bulut hizmeti için SSL sertifikasını karşıya yükle](#upload-ssl-certificate-to-cloud-service)
2. [Hizmet yapılandırma dosyasında SSL sertifikasını güncelleştir](#update-ssl-certificate-in-service-configuration-file)

## <a name="to-configure-client-certificates"></a>İstemci sertifikalarını yapılandırmak için
İstemci sertifikalarını hizmet isteklerine kimlik doğrulaması için gereklidir. Aşağıdaki üç senaryodan en uygun seçin ve tüm adımları yürütün:

### <a name="turn-off-client-certificates"></a>İstemci sertifikaları devre dışı bırakma
1. [İstemci sertifikası tabanlı kimlik doğrulamasını devre dışı bırakma](#turn-off-client-certificate-based-authentication)

### <a name="issue-new-self-signed-client-certificates"></a>Yeni otomatik olarak imzalanan istemci sertifikaları
1. [Bir otomatik olarak imzalanan sertifika yetkilisi oluşturma](#create-a-self-signed-certification-authority)
2. [Bulut hizmeti için CA sertifikasını karşıya yükle](#upload-ca-certificate-to-cloud-service)
3. [Hizmet yapılandırma dosyasında CA sertifikasını güncelleştir](#update-ca-certificate-in-service-configuration-file)
4. [İstemci sertifikaları](#issue-client-certificates)
5. [PFX dosyaları için istemci sertifikaları oluşturma](#create-pfx-files-for-client-certificates)
6. [İstemci sertifikası Al](#Import-Client-Certificate)
7. [İstemci sertifikası parmak kopyalayın](#copy-client-certificate-thumbprints)
8. [Hizmet yapılandırma dosyasında izin verilen istemcilerini yapılandırma](#configure-allowed-clients-in-the-service-configuration-file)

### <a name="use-existing-client-certificates"></a>Mevcut istemci sertifikalarını kullanın
1. [CA ortak anahtarı bulma](#find-ca-public-key)
2. [Bulut hizmeti için CA sertifikasını karşıya yükle](#Upload-CA-certificate-to-cloud-service)
3. [Hizmet yapılandırma dosyasında CA sertifikasını güncelleştir](#Update-CA-Certificate-in-Service-Configuration-File)
4. [İstemci sertifikası parmak kopyalayın](#Copy-Client-Certificate-Thumbprints)
5. [Hizmet yapılandırma dosyasında izin verilen istemcilerini yapılandırma](#configure-allowed-clients-in-the-service-configuration-file)
6. [İstemci sertifikası iptal denetimi yapılandırma](#Configure-Client-Certificate-Revocation-Check)

## <a name="allowed-ip-addresses"></a>İzin verilen IP adresi
Hizmet uç noktalarına erişime belirli IP adresi aralıkları için kısıtlanmış olabilir.

## <a name="to-configure-encryption-for-the-store"></a>Depolama alanı için şifrelemeyi yapılandırma
Meta veri deposunda saklanan kimlik bilgilerini şifrelemek için bir sertifika gerekir. Aşağıdaki üç senaryodan en uygun seçin ve tüm adımları yürütün:

### <a name="use-a-new-self-signed-certificate"></a>Yeni bir otomatik olarak imzalanan sertifika kullan
1. [Otomatik olarak imzalanan sertifika oluşturma](#create-a-self-signed-certificate)
2. [PFX dosyası için otomatik olarak imzalanan şifreleme sertifika oluşturun.](#create-pfx-file-for-self-signed-ssl-certificate)
3. [Bulut hizmeti için şifreleme sertifikasını karşıya yükle](#upload-encryption-certificate-to-cloud-service)
4. [Hizmet yapılandırma dosyasında şifreleme sertifikasını güncelleştir](#update-encryption-certificate-in-service-configuration-file)

### <a name="use-an-existing-certificate-from-the-certificate-store"></a>Varolan bir sertifikayı sertifika deposundan kullanın
1. [Şifreleme sertifikası sertifika deposundan dışarı aktarma](#export-encryption-certificate-from-certificate-store)
2. [Bulut hizmeti için şifreleme sertifikasını karşıya yükle](#upload-encryption-certificate-to-cloud-service)
3. [Hizmet yapılandırma dosyasında şifreleme sertifikasını güncelleştir](#update-encryption-certificate-in-service-configuration-file)

### <a name="use-an-existing-certificate-in-a-pfx-file"></a>Varolan bir sertifikayı bir PFX dosyası kullanın
1. [Bulut hizmeti için şifreleme sertifikasını karşıya yükle](#upload-encryption-certificate-to-cloud-service)
2. [Hizmet yapılandırma dosyasında şifreleme sertifikasını güncelleştir](#update-encryption-certificate-in-service-configuration-file)

## <a name="the-default-configuration"></a>Varsayılan yapılandırma
Varsayılan yapılandırma, tüm HTTP uç noktası erişimi reddeder. Bu uç noktalar isteklerine veritabanı kimlik bilgileri gibi hassas bilgileri taşımak beri önerilen, ayar budur.
Varsayılan yapılandırma tüm HTTPS uç noktasını erişmesini sağlar. Bu ayar daha fazla kısıtlanabilir.

### <a name="changing-the-configuration"></a>Yapılandırmayı değiştirme
Grubun geçerli erişim denetim kurallarını ve uç nokta yapılandırılmış  **<EndpointAcls>**  bölümüne **hizmet yapılandırma dosyası**.

    <EndpointAcls>
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpIn" accessControl="DenyAll" />
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="AllowAll" />
    </EndpointAcls>

Bir erişim denetimi grubu kurallarında yapılandırılan bir <AccessControl name=""> hizmet yapılandırma dosyasının. 

Biçim ağ erişim denetimi listelerini belgelerinde açıklanmıştır.
Örneğin, yalnızca IP aralığı HTTPS uç noktasına erişmek için 100.100.0.0 için 100.100.255.255 izin vermek için kuralları şöyle olabilir:

    <AccessControl name="Retricted">
      <Rule action="permit" description="Some" order="1" remoteSubnet="100.100.0.0/16"/>
      <Rule action="deny" description="None" order="2" remoteSubnet="0.0.0.0/0" />
    </AccessControl>
    <EndpointAcls>
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="Restricted" />

## <a name="denial-of-service-prevention"></a>Hizmet önleme reddi
Desteklenen algılamak ve hizmet reddi saldırılarını önlemek için iki farklı mekanizma vardır:

* Uzak ana bilgisayar başına eşzamanlı istek sayısını kısıtlayın (varsayılan olarak kapalıdır)
* Uzak ana bilgisayar başına erişimi oranını kısıtlamak (üzerinde varsayılan olarak)

Bunlar daha fazla dinamik IP Güvenlik IIS'de açıklandığı özellikleri temel alır. Ne zaman bu yapılandırmasını değiştirme aşağıdaki etmenlere dikkat:

* Proxy ve ağ adresi çevirisi aygıtları üzerinden uzak ana bilgisayar bilgilerini davranışını
* Web rolü herhangi bir kaynağa her istek (örneğin yükleme komut dosyaları, görüntüler, vb.) olarak kabul edilir

## <a name="restricting-number-of-concurrent-accesses"></a>Eşzamanlı erişimi sayısını sınırlandırma
Bu davranışı yapılandırmak ayarlar şunlardır:

    <Setting name="DynamicIpRestrictionDenyByConcurrentRequests" value="false" />
    <Setting name="DynamicIpRestrictionMaxConcurrentRequests" value="20" />

DynamicIpRestrictionDenyByConcurrentRequests bu korumayı etkinleştirmek için true olarak değiştirin.

## <a name="restricting-rate-of-access"></a>Erişim kısıtlama oranı
Bu davranışı yapılandırmak ayarlar şunlardır:

    <Setting name="DynamicIpRestrictionDenyByRequestRate" value="true" />
    <Setting name="DynamicIpRestrictionMaxRequests" value="100" />
    <Setting name="DynamicIpRestrictionRequestIntervalInMilliseconds" value="2000" />

## <a name="configuring-the-response-to-a-denied-request"></a>Reddedilen isteğinin yanıtı yapılandırma
Aşağıdaki ayar reddedilen isteğinin yanıtı yapılandırır:

    <Setting name="DynamicIpRestrictionDenyAction" value="AbortRequest" />
Desteklenen diğer değerler için dinamik IP Güvenlik IIS'de belgelere bakın.

## <a name="operations-for-configuring-service-certificates"></a>Hizmet sertifikaları yapılandırma işlemleri
Bu konu yalnızca başvuru amacıyla kullanılır. Lütfen özetlenen yapılandırma adımları izleyin:

* SSL sertifikası yapılandırma
* İstemci sertifikaları yapılandırın

## <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma
Yürütün:

    makecert ^
      -n "CN=myservice.cloudapp.net" ^
      -e MM/DD/YYYY ^
      -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1" ^
      -a sha1 -len 2048 ^
      -sv MySSL.pvk MySSL.cer

Özelleştirmek için:

* -n hizmet URL'si. Joker karakterler ("CN = * .cloudapp .net") ve diğer adları ("CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net") desteklenir.
* -e sertifika sona erme tarihi ile güçlü bir parola oluşturmak ve istendiğinde belirtin.

## <a name="create-pfx-file-for-self-signed-ssl-certificate"></a>Otomatik olarak imzalanan SSL Sertifika PFX dosyası oluşturma
Yürütün:

        pvk2pfx -pvk MySSL.pvk -spc MySSL.cer

Bir parola girin ve sonra bu seçenekler ile sertifika verme:

* Evet, özel anahtarı dışarı aktar
* Tüm genişletilmiş özellikleri Dışarı Aktar

## <a name="export-ssl-certificate-from-certificate-store"></a>SSL sertifikası, sertifika deposundan dışarı aktarma
* Sertifika Bul
* Tıklatın Eylemler -> tüm görevleri Export ->...
* İçinde sertifika verme bir. Bu seçenekler PFX dosyası:
  * Evet, özel anahtarı dışarı aktar
  * Mümkünse sertifika yolundaki tüm sertifikaları dahil et * tüm genişletilmiş özellikleri Dışarı Aktar

## <a name="upload-ssl-certificate-to-cloud-service"></a>Bulut hizmeti için SSL sertifikasını karşıya yükle
Karşıya yükleme, var olan sertifika veya oluşturulabilir. SSL anahtar çifti PFX dosyası:

* Özel anahtar bilgisi koruyan parolayı girin

## <a name="update-ssl-certificate-in-service-configuration-file"></a>Hizmet yapılandırma dosyasında SSL sertifikasını güncelleştir
Bulut hizmetine karşıya sertifikanın parmak izine sahip hizmet yapılandırma dosyasında aşağıdaki ayar parmak izi değerini güncelleştirin:

    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="import-ssl-certification-authority"></a>SSL sertifika yetkilisi alma
Tüm hesap/hizmet ile iletişim kuracak makinesinde aşağıdaki adımları izleyin:

* Çift tıklayın. Windows Gezgini'nde CER dosyasını
* Sertifika iletişim kutusunda, sertifikayı yükle düğmesine...
* Güvenilen Kök Sertifika Yetkilileri deposuna sertifika İçeri Aktar

## <a name="turn-off-client-certificate-based-authentication"></a>İstemci sertifikası tabanlı kimlik doğrulamasını devre dışı bırakma
Yalnızca istemci sertifika tabanlı kimlik doğrulaması desteklenmez ve diğer mekanizmaları (örneğin Microsoft Azure sanal ağı) yerinde olmadıkça devre dışı bırakmasını hizmet uç noktalarına erişimine izin verir.

Hizmet yapılandırma dosyasında özelliği devre dışı bırakmak için false bu ayarları değiştirin:

    <Setting name="SetupWebAppForClientCertificates" value="false" />
    <Setting name="SetupWebserverForClientCertificates" value="false" />

Ardından, SSL sertifikası ile aynı parmak izine CA sertifikası ayarında kopyalayın:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="create-a-self-signed-certification-authority"></a>Bir otomatik olarak imzalanan sertifika yetkilisi oluşturma
Bir sertifika yetkilisi olarak davranacak şekilde otomatik olarak imzalanan bir sertifika oluşturmak için aşağıdaki adımları yürütün:

    makecert ^
    -n "CN=MyCA" ^
    -e MM/DD/YYYY ^
     -r -cy authority -h 1 ^
     -a sha1 -len 2048 ^
      -sr localmachine -ss my ^
      MyCA.cer

Özelleştirmek için

* -e ile sertifika sona erme tarihi

## <a name="find-ca-public-key"></a>CA ortak anahtarı bulma
İstemci sertifikalarının tümünü hizmeti tarafından güvenilen bir sertifika yetkilisi tarafından verilmiş olması gerekir. İstemci kimlik doğrulaması için bulut hizmetine yüklemek için kullanılacak sertifikaları veren sertifika yetkilisine ortak anahtar bulunamıyor.

Ortak anahtar dosyasıyla kullanılabilir durumda değilse, sertifika deposundan dışarı aktarın:

* Sertifika Bul
  * Aynı sertifika yetkilisi tarafından verilen bir istemci sertifikası için arama yapın
* Sertifikayı çift tıklatın.
* Sertifika iletişim kutusunda sertifika yolu sekmesini seçin.
* Yolun CA girişi çift tıklatın.
* Sertifika Özellikleri not alın.
* Kapat **sertifika** iletişim.
* Sertifika Bul
  * Yukarıda belirtilen CA'yı arayın.
* Tıklatın Eylemler -> tüm görevleri Export ->...
* İçinde sertifika verme bir. CER bu seçenekleri:
  * **Hayır, özel anahtarı verme**
  * Tüm sertifikaları mümkünse sertifika yolundaki içerir.
  * Tüm genişletilmiş özellikleri dışarı aktarın.

## <a name="upload-ca-certificate-to-cloud-service"></a>Bulut hizmeti için CA sertifikasını karşıya yükle
Karşıya yükleme, var olan sertifika veya oluşturulabilir. CER dosyasını CA ortak anahtarı ile.

## <a name="update-ca-certificate-in-service-configuration-file"></a>Hizmet yapılandırma dosyasında güncelleştirme CA sertifikası
Bulut hizmetine karşıya sertifikanın parmak izine sahip hizmet yapılandırma dosyasında aşağıdaki ayar parmak izi değerini güncelleştirin:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

Aşağıdaki ayarı değerini aynı parmak izine sahip güncelleştirin:

    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />

## <a name="issue-client-certificates"></a>İstemci sertifikaları
Her hizmete erişmek için yetkili kullanmak özel his/hers için yayımlanan bir istemci sertifikasına sahip olmalıdır ve his/hers kendi özel anahtarını korumak için güçlü bir parola seçmeniz gerekir. 

Aşağıdaki adımları otomatik olarak imzalanan sertifika burada oluşturulan ve saklanan aynı makinede çalıştırılmalıdır:

    makecert ^
      -n "CN=My ID" ^
      -e MM/DD/YYYY ^
      -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.2" ^
      -a sha1 -len 2048 ^
      -in "MyCA" -ir localmachine -is my ^
      -sv MyID.pvk MyID.cer

Özelleştirme:

* -n Bu sertifika ile kimlik doğrulaması yapılacak istemci için bir kimliği olan
* -e ile sertifika sona erme tarihi
* MyID.pvk ve bu istemci sertifikası için benzersiz adlarıyla MyID.cer

Bu komut oluşturulabilir ve bir kez kullanılması bir parola sorar. Güçlü bir parola kullanın.

## <a name="create-pfx-files-for-client-certificates"></a>PFX dosyaları için istemci sertifikaları oluşturma
Her oluşturulan istemci sertifikası için yürütün:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Özelleştirme:

    MyID.pvk and MyID.cer with the filename for the client certificate

Bir parola girin ve sonra bu seçenekler ile sertifika verme:

* Evet, özel anahtarı dışarı aktar
* Tüm genişletilmiş özellikleri Dışarı Aktar
* Bu sertifika yayımlanmaktadır tek verme parola seçmeniz gerekir

## <a name="import-client-certificate"></a>İstemci sertifikası Al
Kendisi için bir istemci sertifikası verilmiş olan her anahtar çifti klasöründe hizmetiyle iletişim kurmak için kullanacağınız makinelerde almanız gerekir:

* Çift tıklayın. Windows Gezgini'nde PFX dosyası
* İçeri aktarma kişisel içine deposundan sertifika ile en az bu seçeneği:
  * Seçili tüm genişletilmiş özellikleri Ekle

## <a name="copy-client-certificate-thumbprints"></a>İstemci sertifikası parmak kopyalayın
Kendisi için bir istemci sertifikası yayımlandığı her parmak izini his/hers almak için şu adımları izlemelisiniz service yapılandırma dosyasına eklenecek sertifikası:

* Certmgr.exe çalıştırın
* Kişisel sekmesini seçin
* Kimlik doğrulaması için kullanılacak istemci sertifikası çift tıklatın
* Açılır sertifikası iletişim kutusunda, Ayrıntılar sekmesini seçin
* Göster tüm görüntülediğinden emin olun
* Parmak izi listede adlı alanı seçin
* Parmak izi değerini kopyalayın ** ilk rakam önünde görünür olmayan Unicode karakterler silme ** tüm alanları Sil

## <a name="configure-allowed-clients-in-the-service-configuration-file"></a>Hizmet yapılandırma dosyasında izin verilen istemcilerini yapılandırma
Hizmet yapılandırma dosyasında aşağıdaki ayarın değerini hizmetine erişim izni olan istemci sertifikalarının parmak izleri virgülle ayrılmış bir listesi ile güncelleştirin:

    <Setting name="AllowedClientCertificateThumbprints" value="" />

## <a name="configure-client-certificate-revocation-check"></a>İstemci sertifikası iptal denetimi yapılandırma
Varsayılan ayar, istemcinin sertifika iptal durumunu için sertifika yetkilisi ile kontrol etmez. İstemci sertifikaları veren sertifika yetkilisini bu tür denetimler destekliyorsa üzerinde denetimleri, etkinleştirmek için X509RevocationMode sıralamasında tanımlanan değerlerden biriyle aşağıdaki ayarını değiştirin:

    <Setting name="ClientCertificateRevocationCheck" value="NoCheck" />

## <a name="create-pfx-file-for-self-signed-encryption-certificates"></a>Otomatik olarak imzalanan şifreleme sertifikaları için PFX dosyası oluşturun
Bir şifreleme sertifikası için yürütün:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Özelleştirme:

    MyID.pvk and MyID.cer with the filename for the encryption certificate

Bir parola girin ve sonra bu seçenekler ile sertifika verme:

* Evet, özel anahtarı dışarı aktar
* Tüm genişletilmiş özellikleri Dışarı Aktar
* Bulut hizmeti için sertifikayı karşıya yüklenirken parola gerekir.

## <a name="export-encryption-certificate-from-certificate-store"></a>Şifreleme sertifikası sertifika deposundan dışarı aktarma
* Sertifika Bul
* Tıklatın Eylemler -> tüm görevleri Export ->...
* İçinde sertifika verme bir. Bu seçenekler PFX dosyası: 
  * Evet, özel anahtarı dışarı aktar
  * Mümkünse sertifika yolundaki tüm sertifikaları dahil et 
* Tüm genişletilmiş özellikleri Dışarı Aktar

## <a name="upload-encryption-certificate-to-cloud-service"></a>Bulut hizmetine şifreleme sertifikasını karşıya yükle
Karşıya yükleme, var olan sertifika veya oluşturulabilir. Şifreleme anahtar çifti PFX dosyası:

* Özel anahtar bilgisi koruyan parolayı girin

## <a name="update-encryption-certificate-in-service-configuration-file"></a>Hizmet yapılandırma dosyasında şifreleme sertifikasını güncelleştir
Bulut hizmetine karşıya sertifikanın parmak izine sahip parmak izi değeri hizmet yapılandırma dosyasında aşağıdaki ayarlardan birini güncelleştirin:

    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="common-certificate-operations"></a>Ortak sertifika işlemleri
* SSL sertifikası yapılandırma
* İstemci sertifikaları yapılandırın

## <a name="find-certificate"></a>Sertifika Bul
Şu adımları uygulayın:

1. MMC.exe çalıştırın.
2. Dosya -> Ekle/Kaldır ek bileşenini...
3. Seçin **Sertifikalar**.
4. **Ekle**'ye tıklayın.
5. Sertifika depolama konumu seçin.
6. **Son**'a tıklayın.
7. **Tamam** düğmesine tıklayın.
8. Genişletme **Sertifikalar**.
9. Sertifika deposu düğümünü genişletin.
10. Sertifika alt düğümünü genişletin.
11. Listeden bir sertifika seçin.

## <a name="export-certificate"></a>Sertifika verme
İçinde **Sertifika Verme Sihirbazı**:

1. **İleri**’ye tıklayın.
2. Seçin **Evet**, ardından **özel anahtarı dışarı**.
3. **İleri**’ye tıklayın.
4. İstenen çıktı dosyası biçimini seçin.
5. İstenen seçenekleri denetleyin.
6. Denetleme **parola**.
7. Güçlü bir parola girin ve onaylayın.
8. **İleri**’ye tıklayın.
9. Yazın veya bir dosya adı sertifikanın depolanacağı yeri göz atın (kullanan bir. PFX uzantılı).
10. **İleri**’ye tıklayın.
11. **Son**'a tıklayın.
12. **Tamam** düğmesine tıklayın.

## <a name="import-certificate"></a>Sertifika İçeri Aktar
Sertifika Alma Sihirbazı'nda:

1. Depolama konumu seçin.
   
   * Seçin **geçerli kullanıcı** geçerli kullanıcı altında çalışan işlemler hizmeti erişecek yalnızca
   * Seçin **yerel makine** bu bilgisayardaki diğer işlemlerin hizmet erişecekse
2. **İleri**’ye tıklayın.
3. Bir dosyadan içeri aktarma, dosya yolunu doğrulayın.
4. İçeri aktarma, bir. PFX dosyası:
   1. Özel anahtarın korunması için parolayı girin
   2. İçeri aktarma seçenekleri seçin
5. "Yerine" sertifikaları aşağıdaki depolama alanına seçin
6. **Gözat**’a tıklayın.
7. İstenen depolama alanı seçin.
8. **Son**'a tıklayın.
   
   * Güvenilen kök sertifika yetkilisi deposunda seçildiyse, tıklatın **Evet**.
9. Tıklatın **Tamam** tüm iletişim Windows.

## <a name="upload-certificate"></a>Sertifikayı karşıya yükleme
İçinde [Azure portalı](https://portal.azure.com/)

1. Seçin **bulut hizmetlerini**.
2. Bulut hizmeti seçin.
3. Üst menüsünde **Sertifikalar**.
4. Alt çubuğunda **karşıya**.
5. Sertifika dosyası seçin.
6. Bu doğruysa bir. PFX dosyası, özel anahtarı parolasını girin.
7. Tamamlandığında, sertifika parmak izini listede yeni girdiyi kopyalayın.

## <a name="other-security-considerations"></a>Diğer güvenlik konuları
HTTPS uç noktası kullanıldığında, bu belgede açıklanan SSL ayarları hizmet ve istemcileri arasındaki iletişimi şifrelemek. Bu veritabanı erişimi için kimlik bilgilerini bu yana önemlidir ve iletişime büyük olasılıkla diğer hassas bilgiler yer alır. Ancak, hizmeti kimlik bilgileri, kendi iç Microsoft Azure aboneliğiniz meta veri depolama alanı için sağlanan Microsoft Azure SQL veritabanı tablolarında dahil olmak üzere, iç durum devam ederse unutmayın. Bu veritabanı aşağıdaki ayarı hizmet yapılandırma dosyanızdaki bir parçası olarak tanımlandı (. CSCFG dosyası): 

    <Setting name="ElasticScaleMetadata" value="Server=…" />

Bu veritabanında depolanan kimlik bilgileri şifrelenir. Ancak, en iyi uygulama, hizmet dağıtımlarınız web ve çalışan rolleri güncel tutulması ve güvenli olarak her ikisi de meta veri veritabanı ve şifreleme ve şifre çözme depolanan kimlik bilgileri için kullanılan sertifikanın erişim içerdiğinden emin olun. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

