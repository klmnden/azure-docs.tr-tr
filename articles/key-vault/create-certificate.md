---
title: Sertifika Oluşturma yöntemleri
description: Anahtar kasasına bir sertifika oluşturmak için yol.
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e17b4c9b-4ff3-472f-8c9d-d130eb443968
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: alleonar
ms.openlocfilehash: 7b71c6a8daa97300ecf3b37ec6ab47207fece98e
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="certificate-creation-methods"></a>Sertifika Oluşturma yöntemleri

 Bir anahtar kasası (KV) sertifika ya da oluşturulmuş ya bir anahtar kasası alınmış. KV sertifika oluşturulduğunda, özel anahtarı anahtar kasası içinde oluşturulur ve sertifika sahibinin hiçbir zaman kullanıma. Anahtar kasasına bir sertifika oluşturmak için yöntemler şunlardır:  

-   **Kendinden imzalı bir sertifika oluşturun:** bu genel-özel anahtar çifti oluşturur ve bir sertifika ile ilişkilendirin. Sertifika kendi anahtar tarafından imzalanması.  

-    **El ile yeni bir sertifika oluşturun:** bu genel-özel anahtar çifti oluşturur ve bir X.509 sertifika imzalama isteği oluşturun. İmzalama isteği, kayıt yetkilisi ya da sertifika yetkilisi tarafından imzalanmış. Anahtar kasası KV sertifikada tamamlamak için sertifika Beklemede anahtar ile birleştirilebilir imzalı x509 eşleştirin. Bu yöntem daha fazla adım gerektirse de, özel anahtarı içinde oluşturulan ve anahtar Kasası'na kısıtlanmış çünkü bu, büyük güvenlik sağlar. Aşağıdaki diyagramda bu açıklanmıştır.  

![Kendi sertifika yetkilisi ile bir sertifika oluşturun](media/certificate-authority-1.png)  

Aşağıdaki açıklamaları önceki diyagramda yeşil harflerin adımlarda karşılık gelir.

1. Yukarıdaki diyagramda uygulamanızı dahili olarak, anahtar kasasına bir anahtar oluşturarak başlayan bir sertifika oluşturuyor.
2. Anahtar kasası uygulamanız için bir sertifika imzalama isteği (CSR) döndürür.
3. Uygulamanız, seçilen CA CSR geçirir.
4. İle seçilen CA yanıt bir bir X509 sertifika.
5. Yeni sertifika oluşturulmasını bir birleşme ile X509 uygulamanızı tamamlar, CA'ın sertifikadan.

-   **Bir bilinen veren sağlayıcıyla bir sertifika oluşturun:** veren nesne oluşturma işleminin tek seferlik bir görevi gerçekleştirmek bu yöntem gerektirir. Bir veren nesne oluşturulduktan sonra size kasa anahtarı, adı KV sertifika ilkesinde başvurulabilir. Bu tür bir KV sertifika oluşturma isteği kasaya bir anahtar çifti oluşturur ve bir x509 almak için başvurulan veren nesnesinde bilgileri kullanarak veren sağlayıcısı hizmeti ile iletişim sertifika. Sertifika veren hizmetinden alınan ve KV tamamlamak için anahtar çifti ile birleştirilmiş x509 oluşturma sertifika.  

![Bir anahtar kasası ortaklık sertifika yetkilisi ile bir sertifika oluşturun](media/certificate-authority-2.png)  

Aşağıdaki açıklamaları önceki diyagramda yeşil harflerin adımlarda karşılık gelir.

1. Yukarıdaki diyagramda uygulamanızı dahili olarak, anahtar kasasına bir anahtar oluşturarak başlayan bir sertifika oluşturuyor.
2. Anahtar kasası gönderir ve CA SSL sertifika isteği.
3. Uygulamanız, döngü ve bekleme işleminde sertifika tamamlanması için anahtar kasanız için yoklar. Anahtar kasası x509 ile CA'ın yanıtı aldığında sertifika oluşturma işlemi tamamlandıktan sertifika.
4. CA anahtar Kasası'nın SSL sertifika isteği bir X509 ile yanıtlaması SSL sertifikası.
5. X509 birleşme ile yeni sertifika oluşturmayı tamamlar CA sertifikası.

## <a name="asynchronous-process"></a>Zaman uyumsuz işlem
KV sertifika oluşturma zaman uyumsuz bir işlemdir. Bu işlem bir KV sertifika isteği oluşturmak ve 202 (kabul edilen) bir http durum kodunu döndürür. İsteğin durumunu, bu işlemi tarafından oluşturulan bekleyen nesne yoklayarak izlenebilir. Bekleyen nesnesinin tam URI konumu üstbilgisinde döndürülür.  

KV sertifika oluşturma isteği tamamlandığında, bekleyen bir nesnenin durumu "" devam ediyor"tamamlandı" olarak değiştirir ve KV sertifika yeni bir sürümü oluşturulacak. Bu, geçerli sürümü olur.  

## <a name="first-creation"></a>İlk oluşturma
 KV sertifikayı ilk kez oluşturulduğunda, bir adreslenebilir anahtarı ve gizli da oluşturulduğunda, sertifikanın aynı ada sahip. Ad zaten kullanımda olduğundan, işlem 409 (Çakışma) bir http durum kodu ile başarısız olur.
Adreslenebilir anahtarı ve gizli öznitelikleriyle KV sertifika öznitelikleri alın. Adreslenebilir anahtarı ve gizli bu yolla oluşturulan yönetilen anahtarlar ve, yaşam süresi anahtar kasası tarafından yönetilen gizli olarak işaretlenmiş. Yönetilen anahtarları ve gizli anahtarları salt okunurdur. Not: KV sertifikanın süresi dolmadan devre dışı bırakılırsa, karşılık gelen anahtarı ve gizli çalışamaz olur.  

 Bu bir KV sertifika oluşturmak için ilk işlemi ise bir ilke gereklidir.  Bir ilke da ile art arda sağlanabilir ilke kaynağı değiştirmek için işlemleri oluşturun. Bir ilke sağlanmazsa, hizmet ilkesi kaynakta KV sertifika sonraki bir sürümünü oluşturmak için kullanılır. İlerleme durumu, geçerli KV sertifikası ve karşılık gelen adreslenebilir anahtarı ve gizli anahtarı, sonraki bir sürümünü oluşturmak için bir istek iken kalır Not değişmedi.  

## <a name="self-issued-certificate"></a>Kendi kendine sertifikayı
 Kendi kendine verilen bir sertifika oluşturmak için verenin adı olarak Ayarla "Kendi kendine" Sertifika İlkesi Sertifika İlkesi'nden aşağıdaki kod parçacığında gösterildiği gibi.  

```  
"issuer": {  
       "name": "Self"  
    }  

```  

 Verenin adı belirtilmezse, verenin adı "Bilinmeyen" olarak ayarlanır. Veren "Bilinmiyor" olduğunda, sertifika sahibinin el ile bir x509 almak gerekir güncelleştirmesini tercih veren sertifika ve sertifika oluşturma işlemini tamamlamak için nesne bekleyen anahtar kasası sertifikayla birleştirme ortak x509 sertifika.

```  
"issuer": {  
       "name": "Unknown"  
    }  

```  

## <a name="partnered-ca-providers"></a>CA ortaklık sağlayıcıları
Sertifika oluşturma el ile tamamlanan veya kullanarak bir "Self" veren olabilir. Anahtar kasası ayrıca sertifikaları oluşturma işlemini basitleştirmek için belirli veren sağlayıcılarıyla ortakları. Aşağıdaki türde sertifikalar için anahtar kasasını bu iş ortağı veren sağlayıcılarıyla sıralanabilir.  

|Sağlayıcı|Sertifika türü|  
|--------------|----------------------|  
|DigiCert|Anahtar kasası DigiCert OV veya EV SSL sertifikalarıyla sunar|
|GlobalCert|Anahtar kasası GlobalSign OV veya EV SSL sertifikalarıyla sunar|

 Bu sertifikayı verenin sağlayıcıların coğrafi kullanılabilirlik dahil olmak üzere daha fazla bilgi için bkz: [sertifika verenler](/rest/api/keyvault/certificate-issuers.md).

Bir sipariş veren sağlayıcı ile yerleştirildiğinde, uygun veya geçersiz kılma x509 sertifika uzantıları ve sertifika geçerlilik süresi sertifika türüne göre unutmayın.  

 Yetkilendirme: sertifikaları ve oluşturma izni gerektirir.

 ## <a name="see-also"></a>Ayrıca Bkz.
 - [Anahtarları, gizli ve sertifikaları hakkında](about-keys-secrets-and-certificates.md)
 - [İzleme ve yönetme sertifika oluşturma](create-certificate-scenarios.md)
