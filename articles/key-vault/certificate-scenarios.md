---
title: Key Vault sertifikalar ile çalışmaya başlama
description: Aşağıdaki senaryolardan birkaç Key Vault'un sertifika yönetim hizmeti, ilk sertifikayı anahtar kasanızı oluşturmak için gereken ek adımları dahil olmak üzere birincil kullanımlarını özetler.
services: key-vault
author: msmbaldwin
manager: barbkess
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: 805f11d57a635f4e73309d025e185049b511570b
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427847"
---
# <a name="get-started-with-key-vault-certificates"></a>Key Vault sertifikalar ile çalışmaya başlama
Aşağıdaki senaryolardan birkaç Key Vault'un sertifika yönetim hizmeti, ilk sertifikayı anahtar kasanızı oluşturmak için gereken ek adımları dahil olmak üzere birincil kullanımlarını özetler.

Aşağıda özetlenmiştir:
- İlk, anahtar kasası sertifikası oluşturma
- Key Vault ile ortaklık ile bir sertifika yetkilisinden bir sertifika oluşturma
- Key Vault ile bir sertifika yetkilisinden bir sertifika oluşturma ortaklık değil
- Sertifika alma

## <a name="certificates-are-complex-objects"></a>Karmaşık nesneler sertifikalardır
Sertifikalar, anahtar kasası sertifikası olarak birbirine bağlı üç birbiriyle ilişkili kaynakların oluşur; Sertifika meta verileri, bir anahtar ve gizli dizi.


![Sertifikaları karmaşıktır](media/azure-key-vault.png)


## <a name="creating-your-first-key-vault-certificate"></a>İlk, anahtar kasası sertifikası oluşturma  
 Bir sertifika bir anahtar kasası (KV) içinde oluşturulmadan önce önkoşul adımlarını 1 ve 2 gerekir başarıyla elde edilebilir ve bu kullanıcı için bir anahtar kasası mevcut olmalıdır / kuruluş.  

**1. adım** -sertifika yetkilisi (CA) sağlayıcıları  
-   Kolaylaşmasına BT yöneticisi, PKI yönetim veya herkesin belirli bir şirket için (ör. CA ' ları ile hesapları yönetme Contoso) anahtar kasası sertifikaların kullanılması için önkoşuldur.  
    Aşağıdaki CA'lar anahtar kasası ile iş ortaklığı yaptı geçerli sağlayıcıları şunlardır:  
    -   DigiCert - Key Vault OV-SSL sertifikalarıyla DigiCert sunar.  
    -   Globaltrust - Key Vault OV-SSL sertifikalarıyla Globaltrust sunar.  

**2. adım** -Hesap Yöneticisi kimlik bilgilerini kaydetmek için Key Vault tarafından kullanılacak bir CA sağlayıcısı oluşturur için yenileyin ve SSL sertifikaları Key Vault aracılığıyla kullanır.

**3. adım** -Yöneticisi veya CA ile doğrudan hesabından bir Contoso Yöneticisi, sertifikalar, CA'ın bağlı olarak sahip bir Contoso çalışanı (Key Vault kullanıcı) ile birlikte bir sertifika edinebilirsiniz.  

- Bir anahtar kasası tarafından bir Ekle kimlik bilgisi işleme başlamak [sertifikayı veren ayarlama](/rest/api/keyvault/setcertificateissuer/setcertificateissuer) kaynak. Sertifikayı veren süresi kaynak olarak Azure anahtar kasası (KV) olarak temsil edilen bir varlıktır. KV sertifikanın kaynağı hakkında bilgi sağlamak için kullanılır; Verenin adı, sağlayıcı, kimlik bilgilerini ve diğer yönetimsel ayrıntıları.
  - Örn. MyDigiCertIssuer  
    -   Sağlayıcı  
    -   Kimlik bilgileri – CA hesabı kimlik bilgileri. Her CA'ın kendi belirli veri vardır.  

    CA sağlayıcılarıyla hesapları oluşturma hakkında daha fazla bilgi için ilgili gönderiye bakın [Key Vault blog](https://aka.ms/kvcertsblog).  

**Adım 3.1** -ayarlanan [sertifika kişileri](/rest/api/keyvault/setcertificatecontacts/setcertificatecontacts) bildirimleri. Key Vault kullanıcı sorumlu budur. Key Vault, bu adımı uygulamaz.  

Not - Bu süreçte adım 3.1, tek seferlik bir işlemdir.  

## <a name="creating-a-certificate-with-a-ca-partnered-with-key-vault"></a>Key Vault ile iş Birliği yaparak bir CA ile bir sertifika oluşturma

![Key Vault iş Birliği yaparak sertifika yetkilisi ile bir sertifika oluşturma](media/certificate-authority-2.png)

**4. adım** -aşağıdaki açıklamalar önceki şemada yeşil numaralı adımları karşılık gelir.  
  (1) - Yukarıdaki diyagramda, dahili olarak, anahtar kasasına bir anahtar oluşturarak başlatan bir sertifika uygulama oluşturuyor.  
  (2) - Key Vault gönderen bir SSL sertifikası istemek için CA.  
  (3) - uygulamanızı yoklar, döngü ve bekleme işleminde anahtar kasanız için sertifika tamamlama için. Key Vault ile x509 CA'ın yanıtı aldığında sertifika oluşturma tamamlandığında, sertifika.  
  (4) - Key Vault'un SSL sertifika isteği x X509 ile CA yanıtlar SSL sertifikası.  
  (5) - X509 bir birleşme ile yeni bir sertifika oluşturma işleminiz tamamlandıktan CA sertifikası.  

  Key Vault kullanıcı – ilke belirterek bir sertifika oluşturur

  -   Gerektiği şekilde yineleyin  
  -   İlke kısıtlamaları  
      -   X509 özellikleri  
      -   Anahtar özellikleri  
      -   Sağlayıcı başvuru - > örn. MyDigiCertIssure  
      -   Yenileme bilgilerinizi - > örn. 90 gün kaldığında  

  - Bir sertifika oluşturma işlemi, genellikle zaman uyumsuz bir işlemdir ve anahtar kasanız oluşturma sertifika işlemi durumu için yoklama içerir.  
[Sertifika işlemi Al](/rest/api/keyvault/getcertificateoperation/getcertificateoperation)  
      -   Durum: hata bilgileri ile başarısız oldu veya iptal edildi tamamlandı  
      -   Oluşturmak için gecikme nedeniyle iptal etme işlemi başlatılabilir. İptal edebilir veya etkili olmayabilir.  

## <a name="import-a-certificate"></a>Sertifika alma  
 Alternatif olarak – bir sertifika Key Vault'a – PFX veya PEM içeri aktarılabilir.  

 Sertifikaları bölümünü PEM biçimi hakkında daha fazla bilgi için bkz. [anahtarlara, parolalara ve sertifikalara hakkında](about-keys-secrets-and-certificates.md).  

 Sertifika içeri aktarma – PEM veya PFX diskte ve bir özel anahtara sahip gerektirir. 
-   Belirtmeniz gerekir: kasa adı ve sertifika adı (ilke isteğe bağlı)

-   PEM / PFX dosyalarını içeren öznitelikleri KV ayrıştırabilir ve sertifika ilkesi doldurmak için kullanın. Bir sertifika ilkesi zaten belirtilmişse KV PFX verilerden eşleşecek şekilde çalışacaktır / PEM dosyası.  

-   İçeri aktarma son eklendiğinde, sonraki işlemleri yeni kullanacağı İlkesi (yeni sürümler).  

-   Başka işlem olduğunda, Key Vault yapacağı ilk şey bir süre sonu bildirimi gönderme olur. 

-   Ayrıca, kullanıcı içeri aktarma sırasında işlevsel olan ancak hiçbir bilgi alma: Burada belirtilen varsayılan değerleri içerir ilkeyi düzenleyebilir. Örn. veren bilgisi yok  

### <a name="formats-of-import-we-support"></a>İçeri aktarma destekliyoruz biçimleri
Aşağıdaki içeri aktarma türü PEM dosyası biçimini destekliyoruz. Bir PKCS # kodlanmış 8 aşağıdaki olan şifresiz anahtar ile birlikte tek bir PEM kodlu sertifika

---BAŞLANGIÇ SERTİFİKA------SON SERTİFİKA---

---BAŞLANGIÇ ÖZEL ANAHTARI------SON ÖZEL ANAHTARI---

Sertifika birleştirme işleminde 2 tabanlı PEM biçimleri destekliyoruz. Tek bir PKCS #8 kodlanmış sertifika ya da birleştirebilir veya base64 ile kodlanmış P7B dosyası. ---BAŞLANGIÇ SERTİFİKA------SON SERTİFİKA---

Şu anda EC anahtarları PEM biçiminde desteklemiyoruz.

## <a name="creating-a-certificate-with-a-ca-not-partnered-with-key-vault"></a>Key Vault ile iş Birliği yaparak değil bir CA ile bir sertifika oluşturma  
 Bu yöntem, Key Vault'un iş ortaklığı yaptı sağlayıcıları, kuruluşunuz kendi tercih ettiğiniz bir CA ile çalışabilir, yani daha diğer CA'ları ile çalışma sağlar.  

![Kendi sertifika yetkilisi ile bir sertifika oluşturun](media/certificate-authority-1.png)  

 Aşağıdaki adım açıklamalar önceki şemada yeşil yitirmiş adımları karşılık gelir.  

  (1) - Yukarıdaki diyagramda, dahili olarak, anahtar kasasına bir anahtar oluşturarak başlar bir sertifika uygulama oluşturuyor.  

  (2) - Key Vault, uygulamanız için bir sertifika imzalama isteği (CSR) döndürür.  

  (3) - uygulamanızı seçtiğiniz CA CSR'yi geçirir.  

  (4) - seçtiğiniz CA x X509 ile yanıt veren sertifika.  

  (5) - yeni sertifika oluşturulmasını birleşmesi ile X509 uygulamanızı tamamlandıktan Sertifika yetkilinizden sertifikası.

## <a name="see-also"></a>Ayrıca Bkz.

- [Anahtarlar, gizli diziler ve sertifikalar hakkında](about-keys-secrets-and-certificates.md)
