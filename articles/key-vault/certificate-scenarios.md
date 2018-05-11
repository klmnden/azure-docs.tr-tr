---
title: Anahtar kasası sertifikalar ile çalışmaya başlama
description: Aşağıdaki senaryolarda anahtar Kasası'nın sertifika yönetim hizmeti, ilk sertifikanızı anahtar kasanızı oluşturmak için gereken ek adımlar da dahil olmak üzere birincil kullanımlarını bazıları ana hatlarıyla gösterilir.
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: a788b958-3acb-4bb6-9c94-4776852aeea1
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: alleonar
ms.openlocfilehash: 5f3b8a7b9c7bf582ebc2fac2be8ff55134fbc6f2
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-key-vault-certificates"></a>Anahtar kasası sertifikalar ile çalışmaya başlama
Aşağıdaki senaryolarda anahtar Kasası'nın sertifika yönetim hizmeti, ilk sertifikanızı anahtar kasanızı oluşturmak için gereken ek adımlar da dahil olmak üzere birincil kullanımlarını bazıları ana hatlarıyla gösterilir.

Aşağıda özetlenmiştir:
- İlk anahtar kasası sertifikanızı oluşturma
- Anahtar kasası ile ortaklık bir sertifika yetkilisi ile bir sertifika oluşturma
- Anahtar kasası ile bir sertifika yetkilisi ile bir sertifika oluşturma ortaklık değil
- Sertifika alma

## <a name="certificates-are-complex-objects"></a>Karmaşık nesneler sertifikalar
Sertifika bir anahtar kasası sertifika olarak birbirine bağlı üç birbiriyle ilişkili kaynakları oluşur; Meta veriler, bir anahtarı ve gizlilik sertifika.


![Sertifikaları karmaşıktır](media/azure-key-vault.png)


## <a name="creating-your-first-key-vault-certificate"></a>İlk anahtar kasası sertifikanızı oluşturma  
 Bir sertifika bir anahtar kasası (KV) içinde oluşturulmadan önce önkoşul adım 1 ve 2 gerekir başarılı bir şekilde gerçekleştirilir ve bu kullanıcı için bir anahtar kasası bulunmalıdır / kuruluş.  

**1. adım** -sertifika yetkilisi (CA) sağlayıcıları  
-   Devreye alma BT yöneticisi, PKI yönetim veya herkesin belirli bir şirket için (örneğin, CA'ları ile hesaplarını yönetme Contoso), anahtar kasası sertifikaların kullanılması için bir önkoşuldur.  
    Aşağıdaki CA anahtar kasası ile geçerli partnered sağlayıcıları şunlardır:  
    -   DigiCert - anahtar kasası OV SSL DigiCert sertifikalarla sunar.  
    -   GlobalSign - anahtar kasası OV SSL GlobalSign sertifikalarla sunar.  
    -   WoSign - anahtar kasası teklifleri OV SSL veya WoSign EV SSL sertifikalarıyla WoSign portalındaki WoSign hesabındaki müşteri tarafından yapılandırılan ayar temel.  

**2. adım** -Hesap Yöneticisi kaydetmek için anahtar kasası tarafından kullanılacak kimlik bilgileri bir CA sağlayıcısı oluşturur için yenileyin ve anahtar kasası aracılığıyla SSL sertifikaları kullanır.

**3. adım** -CA hesabıyla yönetici ya da doğrudan bağlı olarak CA sertifikaları sahibi Contoso çalışan (anahtar kasası kullanıcı) ile birlikte bir Contoso yönetici bir sertifika alabilirsiniz.  

-   Bir anahtar kasası için bir ekleme kimlik bilgisi işlemi oluşturarak başlayın bir [veren sertifika](https://docs.microsoft.com/rest/api/keyvault/certificate-issuers) kaynak. 
    -   Örn MyDigiCertIssuer  
        -   Sağlayıcı  
        -   Kimlik bilgileri – CA hesabı kimlik bilgileri. Her CA'ın kendi belirli veri yok.  

     CA sağlayıcılarıyla hesapları oluşturma hakkında daha fazla bilgi için ilgili post bakın [anahtar kasası blog](http://aka.ms/kvcertsblog).  

**Adım 3.1** - ayarlayın [sertifika kişiler](https://docs.microsoft.com/rest/api/keyvault/certificate-contacts) bildirimler için. Anahtar kasası kullanıcı sorumlu budur. Anahtar kasası, bu adımı uygulamaz.  

Not - Bu süreçte adım 3.1, bir kez bir işlemdir.  

## <a name="creating-a-certificate-with-a-ca-partnered-with-key-vault"></a>Anahtar kasası ile ortaklık bir CA ile sertifika oluşturma

![Bir anahtar kasası ortaklık sertifika yetkilisi ile bir sertifika oluşturun](media/certificate-authority-2.png)

**Adım 4** -aşağıdaki açıklamaları önceki diyagramda yeşil numaralı adımları karşılık gelir.  
  (1) - Yukarıdaki diyagramda uygulamanızı dahili olarak, anahtar kasasına bir anahtar oluşturarak başlayan bir sertifika oluşturma.  
  (2) - anahtar kasası gönderir bir SSL sertifikası istemek için CA.  
  (3) - uygulamanızı yoklar, döngü ve bekleme işleminde sertifika tamamlanması için anahtar kasanız için. Anahtar kasası x509 ile CA'ın yanıtı aldığında sertifika oluşturma işlemi tamamlandıktan sertifika.  
  (4) - CA anahtar Kasası'nın SSL sertifika isteği bir X509 ile yanıtlaması SSL sertifikası.  
  (5) - X509 birleşme ile yeni sertifika oluşturmayı tamamlar CA sertifikası.  

  Anahtar kasası kullanıcı – ilke belirterek bir sertifika oluşturur

  -   Gerektiği şekilde tekrarlayın  
  -   İlke kısıtlamaları  
      -   X509 özellikleri  
      -   Anahtar özellikleri  
      -   Sağlayıcı başvuru - > örn. MyDigiCertIssure  
      -   Yenileme bilgilerinizi - > örn. süre sonu önce 90 gün  

  - Sertifika oluşturma işlemi genellikle zaman uyumsuz bir işlemdir ve anahtar kasanızı oluşturma sertifika işlemi durumu için yoklama içerir.  
[Alma sertifika işlemi](https://docs.microsoft.com/en-us/rest/api/keyvault/getcertificateoperation) -durum: tamamlandı için hata bilgileri ile başarısız oldu veya diğer iptal  
            -Oluşturmak için gecikme nedeniyle, iptal etme işlemi başlatılabilir. İptal edebilir veya etkili olmayabilir.  

## <a name="import-a-certificate"></a>Sertifika alma  
 Alternatif olarak – bir sertifika anahtar kasasını – PFX veya PEM içeri aktarılabilir.  

 PEM biçimi hakkında daha fazla bilgi için sertifikalar bölümüne bakın [anahtarları, gizli ve sertifikaları hakkında](about-keys-secrets-and-certificates.md).  

 Sertifika içeri aktarma – bir PEM veya PFX diskte olması ve bir özel anahtara sahip gerektirir. 
-   Belirtmeniz gerekir: kasa adı ve sertifika adı (ilke isteğe bağlı)

-   PEM / PFX dosyalarını KV ayrıştırma ve sertifika ilkesi doldurmak için kullanabileceğiniz öznitelikleri içerir. Bir sertifika ilkesi zaten belirtilmişse KV PFX verileri eşleştirmek deneyecek / PEM dosyası.  

-   İçeri aktarma son olduktan sonra sonraki işlemleri yeni kullanacağı İlkesi (yeni sürümler).  

-   Başka bir işlem olduğunda bir sona erme bildirimi gönder anahtar kasası yapacağı ilk şey olur. 

-   Ayrıca, kullanıcı içeri aktarma sırasındaki işlev, ancak hiçbir bilgi alma: Burada belirtilen Varsayılanları içeriyor İlkesi düzenleyebilirsiniz. Örn veren bilgisi yok  

## <a name="creating-a-certificate-with-a-ca-not-partnered-with-key-vault"></a>Anahtar kasası ile ortaklık olmayan bir CA ile sertifika oluşturma  
 Bu yöntem, kuruluşunuz kendi seçtiğiniz bir CA ile çalışabilirsiniz anlamı anahtar Kasası'nın ortaklık sağlayıcıları daha diğer CA'ları ile çalışma sağlar.  

![Kendi sertifika yetkilisi ile bir sertifika oluşturun](media/certificate-authority-1.png)  

 Aşağıdaki adım açıklamaları önceki diyagramda yeşil harflerin adımlarda karşılık gelir.  

  (1) - Yukarıdaki diyagramda uygulamanızı dahili olarak, anahtar kasasına bir anahtar oluşturarak başlar bir sertifika oluşturma.  

  (2) - anahtar kasası uygulamanız için bir sertifika imzalama isteği (CSR) döndürür.  

  (3) - uygulamanız, seçilen CA CSR geçirir.  

  (4) - bir X509 ile seçilen CA yanıt sertifika.  

  (5) - bir birleşme ile yeni sertifika oluşturma X509, uygulamanızın tamamlandıktan, CA'ın sertifikadan.

## <a name="see-also"></a>Ayrıca Bkz.
- [Sertifika işlemleri](/rest/api/keyvault/certificate-operations.md)
- [Anahtarlar, gizli diziler ve sertifikalar hakkında](about-keys-secrets-and-certificates.md)
