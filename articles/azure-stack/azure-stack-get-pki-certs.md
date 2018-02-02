---
title: "Azure tümleşik yığını systems dağıtımı Azure yığın ortak anahtar altyapısı sertifikalarını oluşturmak | Microsoft Docs"
description: "Azure yığın PKI sertifika dağıtım durumlu işlem Azure tümleşik yığını sistemlerini açıklar."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: jeffgilb
ms.reviewer: ppacent
ms.openlocfilehash: a9f2a882947e07cde0e0505458608f86043b2a67
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="generate-pki-certificates-for-azure-stack-deployment"></a>Azure yığın dağıtımı için PKI sertifikaları oluşturun
Artık bildiğinize göre [PKI sertifikası gereksinimleri](azure-stack-pki-certs.md) Azure yığın dağıtımları için sertifika yetkilisi (CA) tercih ettiğiniz bu sertifikaları almak gerekir. 

## <a name="request-certificates-using-an-inf-file"></a>Bir INF dosyası kullanarak sertifika isteme
Bir sertifika istemek için bir ortak CA veya bir iç CA bir INF dosyası kullanarak yoludur. Windows yerleşik certreq.exe yardımcı programı, bu bölümde açıklandığı gibi bir istek dosyası oluşturmak için sertifika ayrıntı belirten bir INF dosyası kullanabilirsiniz. 

### <a name="sample-inf-file"></a>Örnek INF dosyası 
Örnek sertifika isteği INF dosyasını (dahili veya genel) göndermelerini bir CA'ya bir çevrimdışı sertifika isteği dosyasını oluşturmak için kullanılabilir. INF (isteğe bağlı PaaS hizmetler dahil) gerekli bitiş noktalarının tümü tek bir joker karakter sertifikada kapsar. 

Örnek INF dosyası bu bölge eşit olduğunu varsayar **sea** ve dış FQDN değeri **sea &#46;contoso &#46; com**. Oluşturmadan önce ortamınızı eşleştirmek için bu değerleri değiştirmek bir. Dağıtımınız için INF dosyası. 

    
    [Version] 
    Signature="$Windows NT$"

    [NewRequest] 
    Subject = "C=US, O=Microsoft, L=Redmond, ST=Washington, CN=portal.sea.contoso.com"

    Exportable = TRUE                   ; Private key is not exportable 
    KeyLength = 2048                    ; Common key sizes: 512, 1024, 2048, 4096, 8192, 16384 
    KeySpec = 1                         ; AT_KEYEXCHANGE 
    KeyUsage = 0xA0                     ; Digital Signature, Key Encipherment 
    MachineKeySet = True                ; The key belongs to the local computer account 
    ProviderName = "Microsoft RSA SChannel Cryptographic Provider" 
    ProviderType = 12 
    SMIME = FALSE 
    RequestType = PKCS10
    HashAlgorithm = SHA256

    ; At least certreq.exe shipping with Windows Vista/Server 2008 is required to interpret the [Strings] and [Extensions] sections below

    [Strings] 
    szOID_SUBJECT_ALT_NAME2 = "2.5.29.17" 
    szOID_ENHANCED_KEY_USAGE = "2.5.29.37" 
    szOID_PKIX_KP_SERVER_AUTH = "1.3.6.1.5.5.7.3.1" 
    szOID_PKIX_KP_CLIENT_AUTH = "1.3.6.1.5.5.7.3.2"

    [Extensions] 
    %szOID_SUBJECT_ALT_NAME2% = "{text}dns=*.sea.contoso.com&dns=*.blob.sea.contoso.com&dns=*.queue.sea.contoso.com&dns=*.table.sea.contoso.com&dns=*.vault.sea.contoso.com&dns=*.adminvault.sea.contoso.com&dns=*.dbadapter.sea.contoso.com&dns=*.appservice.sea.contoso.com&dns=*.scm.appservice.sea.contoso.com&dns=api.appservice.sea.contoso.com&dns=ftp.appservice.sea.contoso.com&dns=sso.appservice.sea.contoso.com&dns=adminportal.sea.contoso.com&dns=management.sea.contoso.com&dns=adminmanagement.sea.contoso.com" 
    %szOID_ENHANCED_KEY_USAGE% = "{text}%szOID_PKIX_KP_SERVER_AUTH%,%szOID_PKIX_KP_CLIENT_AUTH%"

    [RequestAttributes]
    

## <a name="generate-and-submit-request-to-the-ca"></a>Oluşturma ve CA isteği gönderin
Aşağıdaki iş akışını özelleştirme ve bir CA'dan sertifika istemek için daha önce oluşturulan örnek INF dosyasını kullanmak nasıl açıklanmaktadır:

1. **Düzenle ve INF dosyasını kaydedin**. Örnek sağlanan kopyalayıp yeni bir metin dosyasına kaydedin. Dosyayı kaydedin ve dağıtımınız eşleşmiyor değerleri dış FQDN ve konu adı yerine bir. INF dosyası.
2. **CertReq kullanarak istek oluşturmak**. Bir Windows bilgisayar kullanarak, yönetici olarak bir komut istemi başlatın ve bir istek (.req) dosyası oluşturmak için aşağıdaki komutu çalıştırın: `certreq -new <yourinffile>.inf <yourreqfilename>.req`.
3. **CA'ya göndermek**. Gönderme. (Dahili veya genel olabilir) Yetkiliniz oluşturulan isteği dosyası.
4. **İçeri aktarın. CER**. CA döndürür bir. CER dosyasını. İstek dosyası ürettiğiniz aynı Windows bilgisayar kullanarak, içe aktarma. CER dosyasını bilgisayar/kişisel deposuna döndürdü. 
5. **Dışarı aktarma ve kopyalayın. PFX için dağıtım klasörleri**. (Özel anahtar dahil) sertifikayı Dışarı Aktar bir. PFX dosyası ve kopyalayın. PFX dosyası açıklanan dağıtım klasörleri için [Azure yığın dağıtım PKI gereksinimleri](azure-stack-pki-certs.md).

## <a name="next-steps"></a>Sonraki adımlar
[Kimlik Tümleştirme](azure-stack-integrate-identity.md)