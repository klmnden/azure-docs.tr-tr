---
title: Sertifika oluşturma yöntemleri
description: Anahtar Kasası'nda bir sertifika oluşturma yolları.
services: key-vault
author: msmbaldwin
manager: barbkess
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: 67720256cfac68c350c800291653a4a0c1d7ee46
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66427825"
---
# <a name="certificate-creation-methods"></a>Sertifika oluşturma yöntemleri

 Bir anahtar Kasası'nı (KV) sertifika oluşturulabilir veya veya bir anahtar kasasının içine aktarılır. KV sertifika oluşturulduğunda özel anahtarı içinde anahtar kasası oluşturulur ve hiç sertifika sahibinin kullanıma sunulan. Anahtar Kasası'nda bir sertifika oluşturmak için yollar şunlardır:  

-   **Kendinden imzalı bir sertifika oluşturun:** Genel-özel anahtar çifti oluşturma ve bir sertifika ile ilişkilendirin. Sertifika kendi anahtarla imzalanacak.  

-    **El ile yeni bir sertifika oluşturun:** Genel-özel anahtar çifti oluşturma ve bir X.509 sertifika imzalama isteği oluşturun. İmzalama isteği, kayıt yetkilisi ya da sertifika yetkilisi tarafından imzalanması. Anahtar Kasası'nda KV sertifika tamamlamak için sertifika bekleyen anahtar ile birleştirilebilir imzalı x509 eşleştirin. Bu yöntem daha fazla adım gerektirse de, özel anahtarı içinde oluşturulur ve anahtar Kasası'na sınırlı olduğundan, daha fazla güvenlik sağlayan verin. Bu, aşağıdaki diyagramda açıklanmıştır.  

![Kendi sertifika yetkilisi ile bir sertifika oluşturun](media/certificate-authority-1.png)  

Aşağıdaki açıklamalar önceki şemada yeşil yitirmiş adımları karşılık gelir.

1. Yukarıdaki diyagramda, uygulamanız dahili olarak, anahtar kasasına bir anahtar oluşturarak başlatan bir sertifika oluşturur.
2. Key Vault, uygulamanız için bir sertifika imzalama isteği (CSR) döndürür.
3. Uygulamanızı seçtiğiniz CA CSR'yi geçirir.
4. Seçtiğiniz CA x X509 ile yanıt veren sertifika.
5. Uygulamanızı yeni sertifika oluşturulmasını birleşmesi ile X509 tamamlandıktan Sertifika yetkilinizden sertifikası.

-   **Bir bilinen veren sağlayıcısı ile bir sertifika oluşturun:** Bu yöntem, bir veren nesne oluşturma tek seferlik bir görev gerektirir. Veren nesne oluşturulduktan sonra size key vault, adını KV sertifika ilkesinde başvurulabilir. Böyle bir KV sertifikası oluşturma isteği kasaya bir anahtar çifti oluşturur ve x x509 almak için başvurulan veren nesnesinde bilgileri kullanarak veren sağlayıcısı hizmeti ile iletişim sertifika. Sertifika veren hizmetinden alınır ve KV tamamlamak için anahtar çifti ile birleştirilmiş x509 oluşturma sertifika.  

![Key Vault iş Birliği yaparak sertifika yetkilisi ile bir sertifika oluşturma](media/certificate-authority-2.png)  

Aşağıdaki açıklamalar önceki şemada yeşil yitirmiş adımları karşılık gelir.

1. Yukarıdaki diyagramda, uygulamanız dahili olarak, anahtar kasasına bir anahtar oluşturarak başlatan bir sertifika oluşturur.
2. Key Vault gönderir ve SSL sertifikası istemek için CA.
3. Uygulamanız, döngü ve bekleme işleminde anahtar kasanız için sertifika tamamlama için yoklar. Key Vault ile x509 CA'ın yanıtı aldığında sertifika oluşturma tamamlandığında, sertifika.
4. CA ile x X509 SSL sertifika isteği Key Vault'un yanıtlaması SSL sertifikası.
5. X509 bir birleşme ile yeni bir sertifika oluşturma işleminiz tamamlandıktan CA sertifikası.

## <a name="asynchronous-process"></a>Zaman uyumsuz işlem
Sertifika oluşturma KV zaman uyumsuz bir işlemdir. Bu işlem bir KV sertifika isteği oluşturmak ve bir 202 (kabul edildi) http durum kodu döndürür. Bu işlemi tarafından oluşturulan bekleyen nesne yoklayarak isteğinin durumu izlenebilir. Bekleyen nesnenin tam URI konum üst bilgisinde döndürülür.  

KV sertifikası oluşturma isteği tamamlandığında, bekleyen bir nesnenin durumu "" devam ediyor"tamamlandı" olarak değiştirir ve KV sertifikanın yeni bir sürüm oluşturulur. Bu, geçerli sürümü olur.  

## <a name="first-creation"></a>İlk oluşturma
 KV sertifikayı ilk kez oluşturulduğunda bir adreslenebilir anahtar ve gizli dizi da oluşturulduğunda, sertifikanın aynı ada sahip. Adı zaten kullanımda, işlem 409 (Çakışma) bir http durum kodu ile başarısız olur.
Adreslenebilir anahtar ve gizli dizi öznitelikleri KV sertifika öznitelikleri alın. Adreslenebilir anahtar ve gizli anahtarı bu şekilde oluşturulan yönetilen anahtarları ve gizli anahtarları, ömürlerinin Key Vault tarafından yönetildiğinden olarak işaretlenir. Yönetilen anahtarları ve gizli anahtarları salt okunurdur. Not: KV sertifika süresi dolana veya sözleşme devre dışı bırakıldı, karşılık gelen anahtar ve gizli dizi çalışmaz hale gelir.  

 Bu bir KV sertifika oluşturmak için ilk işlemi ise bir ilke gereklidir.  Bir ilke ayrıca birbirini izleyen ile sağlanabilir oluşturma işlemleri ilke kaynağı değiştirmek için. Bir ilke sağlanmazsa, hizmet ilkesi kaynakta KV sertifika sonraki bir sürümünü oluşturmak için kullanılır. İlerlemeyi, geçerli KV sertifika ve ilgili adreslenebilir anahtar ve gizli dizi, sonraki bir sürümünü oluşturmak için bir istek olmakla birlikte kalan Not değişmedi.  

## <a name="self-issued-certificate"></a>Kendi kendine verilen sertifika
 Kendi kendine verilen bir sertifika oluşturmak için verenin adı olarak ayarlayın "Kendi kendine" Sertifika İlkesi sertifika ilkesinden aşağıdaki kod parçacığında gösterildiği gibi.  

```  
"issuer": {  
       "name": "Self"  
    }  

```  

 Veren adı belirtilmezse, verenin adı "Bilinmeyen" olarak ayarlayın. Veren "Bilinmeyen" olduğunda, sertifika sahibinin el ile bir x509 istemeniz gerekir kendi tercih ettiğiniz bir verenden sertifika ve sertifika oluşturmayı tamamlamak için nesne bekleyen anahtar kasası sertifikası ile birleştirme ortak x509 sertifika.

```  
"issuer": {  
       "name": "Unknown"  
    }  

```  

## <a name="partnered-ca-providers"></a>İş ortaklığı yaptı CA sağlayıcıları
Sertifika oluşturma el ile tamamlanan veya kullanarak bir "Self" veren olabilir. Key Vault ayrıca sertifikaları oluşturulmasını kolaylaştırmak için belirli veren sağlayıcıları ile iş Birliği yapıyor. Aşağıdaki sertifika türlerini, bu iş ortağı veren sağlayıcıları ile key vault için sıralanabilir.  

|Sağlayıcı|Sertifika türü|  
|--------------|----------------------|  
|DigiCert|Key Vault, DigiCert OV ya da EV SSL sertifikalarıyla sunar.|
|GlobalCert|Key Vault, Globaltrust OV ya da EV SSL sertifikalarıyla sunar. |

 Sertifikayı veren süresi kaynak olarak Azure anahtar kasası (KV) olarak temsil edilen bir varlıktır. KV sertifikanın kaynağı hakkında bilgi sağlamak için kullanılır; Verenin adı, sağlayıcı, kimlik bilgilerini ve diğer yönetimsel ayrıntıları.

Bir sipariş veren sağlayıcıyla yerleştirildiğinde, dikkate veya geçersiz kılma x509 sertifika uzantılarını ve sertifika geçerlilik süresi, sertifika türüne göre dikkat edin.  

 Yetkilendirme: Sertifikalar ve oluşturma izni gerektirir.

## <a name="see-also"></a>Ayrıca Bkz.
 - [Anahtarlara, parolalara ve sertifikalara hakkında](about-keys-secrets-and-certificates.md)
 - [Sertifika oluşturmayı izleme ve yönetme](create-certificate-scenarios.md)
