---
title: "Cihaz kayıtlarını Azure cihaz sağlama hizmeti SDK'ları ile yönetme | Microsoft Docs"
description: "IOT Hub cihaz sağlama hizmeti hizmeti SDK'ları ile cihaz kayıtlarını yönetme"
services: iot-dps
keywords: 
author: yzhong94
ms.author: yizhon
ms.date: 12/01/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: arjmands
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 82da49924e71a38ca557f244f2830e1da45826b1
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="how-to-manage-device-enrollments-with-azure-device-provisioning-service-sdks"></a>Cihaz kayıtlarını Azure cihaz sağlama hizmeti SDK'ları ile yönetme
A *cihaz kaydı* tek bir cihazı veya grubun belirli bir noktada cihaz sağlama hizmeti ile kaydedebilir cihazların bir kayıt oluşturur. İlk istenen yapılandırma istenen IOT hub'ı dahil, kaydın parçası olarak aygıt kaydı içerir. Bu makalede Azure IOT sağlama hizmeti SDK'ları kullanarak programlı olarak sağlama hizmetiniz için cihaz kayıtlarını yönetme gösterilmektedir.  SDK'ları, Azure IOT SDK'ları ile aynı Havuzda github'da kullanılabilir.

## <a name="samples"></a>Örnekler
Bu makale Azure IOT sağlama hizmeti SDK'ları kullanarak programlı olarak sağlama hizmetiniz için cihaz kayıtları yönetmek için üst düzey kavramlarını gözden geçirir.  Tam API çağrıları dil farklılıkları nedeniyle farklı olabilir.  Lütfen ayrıntılar için Github'da sağladığımız örnekleri gözden geçirin:
* [Java sağlama hizmet istemci örnekleri](https://github.com/Azure/azure-iot-sdk-java/tree/master/provisioning/provisioning-samples)
* [Node.js sağlama hizmet istemci örnekleri](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/service/samples)

## <a name="prerequisites"></a>Önkoşullar
* Bağlantı dizesinden bir cihaz sağlama hizmet örneği
* Cihaz güvenlik yapıları:
    * [**TPM**](https://docs.microsoft.com/en-us/azure/iot-dps/concepts-security):
        * Tek tek kayıt: kayıt kimliği ve TPM onay anahtarını fiziksel aygıttan veya TPM Simulator.
        * Kayıt grubu TPM kanıtı için geçerli değildir.
    * [**X.509**](https://docs.microsoft.com/en-us/azure/iot-dps/concepts-security):
        * Tek tek kayıt: [Yaprak sertifikası](https://docs.microsoft.com/en-us/azure/iot-dps/concepts-security#leaf-certificate) fiziksel aygıttan veya BÖLMEK öykünücüsü.
        * Kayıt Grup: [kök sertifika](https://docs.microsoft.com/en-us/azure/iot-dps/concepts-security#root-certificate) veya [ara sertifika](https://docs.microsoft.com/en-us/azure/iot-dps/concepts-security#intermediate-certificate), fiziksel bir aygıtı Aygıt sertifika üretmek için kullanılan.  Aynı zamanda BÖLMEK Öykünücüsünden oluşturulabilir.

## <a name="create-a-device-enrollment"></a>Cihaz kaydı oluşturma

Aygıtlarınızı sağlama hizmeti ile kaydetmek için iki yolu vardır:

* Bir **kayıt grup** X.509 sertifika tarafından imzalanan ortak bir kanıtlama mekanizması paylaşan aygıtları bir grup için bir giriş [kök sertifika](https://docs.microsoft.com/en-us/azure/iot-dps/concepts-security#root-certificate) veya [ara sertifika ](https://docs.microsoft.com/en-us/azure/iot-dps/concepts-security#intermediate-certificate). Çok sayıda istenen ilk yapılandırmasını paylaşan cihazlar için veya cihazlar için bir kayıt grubunu kullanarak aynı Kiracı tüm gitmeyi öneririz. Yalnızca X.509 kanıtlama mekanizması olarak kullanan cihazları kaydedebilmeniz için Not *kayıt grupları*. 

    Bu iş akışını aşağıdaki SDK'ları ile bir kayıt grubu oluşturabilirsiniz:

    1. Kayıt grubu için X.509 kök sertifikası kanıtlama mekanizması kullanır.  Hizmet SDK API çağrısı ```X509Attestation.createFromRootCertificate``` kanıtlama kayıt oluşturmak için kök sertifikası.  X.509 Sertifika PEM dosyasında veya dize olarak sağlanır.
    1. Yeni bir ```EnrollmentGroup``` değişken kullanarak ```attestation``` oluşturulan ve benzersiz bir ```enrollmentGroupId```.  İsteğe bağlı olarak, gibi parametreleri ayarlayabilirsiniz ```Device ID```, ```IoTHubHostName```, ```ProvisioningStatus```.
    2. Hizmet SDK API çağrısı ```createOrUpdateEnrollmentGroup``` ile arka uç uygulamanıza ```EnrollmentGroup``` bir kayıt grubu oluşturmak için.

* Bir **tek tek kayıt** kaydedebilir tek bir cihaz için bir giriş. Tek tek kayıtları kanıtlama mekanizmaları X.509 sertifikalarını veya SAS belirteçlerinde (gerçek veya sanal TPM) kullanabilir. Benzersiz başlangıç yapılandırmasını gerektiren cihazlar için veya yalnızca TPM ya da sanal TPM aracılığıyla SAS belirteci kanıtlama mekanizması olarak kullanabileceğiniz cihazlar için tek tek kayıtları kullanmanızı öneririz. Tek tek kayıtları, belirtilen istenen IOT hub cihaz kimliği olabilir.

    Bu iş akışını aşağıdaki SDK'ları ile tek bir kayıt oluşturabilirsiniz:
    
    1. Seçin, ```attestation``` TPM ya da X.509 mekanizması.
        1. **TPM**: fiziksel aygıttan veya TPM Simulator onay anahtarını giriş olarak kullanarak, hizmet SDK API'si çağırabilirsiniz ```TpmAttestation``` kanıtlama kayıt oluşturmak için. 
        2. **X.509**: giriş olarak istemci sertifikası kullanarak hizmeti SDK API'si çağırabilirsiniz ```X509Attestation.createFromClientCertificate``` kanıtlama kayıt oluşturmak için.
    2. Yeni bir ```IndividualEnrollment``` kullanarak değişken ```attestation``` oluşturulan ve benzersiz bir ```registrationId``` aygıtınızda veya TPM Simulator oluşturulan giriş olarak.  İsteğe bağlı olarak, gibi parametreleri ayarlayabilirsiniz ```Device ID```, ```IoTHubHostName```, ```ProvisioningStatus```.
    3. Hizmet SDK API çağrısı ```createOrUpdateIndividualEnrollment``` ile arka uç uygulamanıza ```IndividualEnrollment``` tek tek bir kayıt oluşturmak için.

Bir kayıt başarıyla oluşturduktan sonra cihaz hizmeti sağlama bir kayıt sonucunu döndürür.

Bu iş akışı içinde gösterilen [örnekleri](#samples).

## <a name="update-an-enrollment-entry"></a>Güncelleştirme kayıt girişi

Bir kayıt girişi oluşturduktan sonra kayıt güncelleştirmesi isteyebilirsiniz.  Olası senaryolar istenen özelliği güncelleştirmeyi, kanıtlama yöntemi güncelleştirme veya aygıt erişim hakkının iptali içerir.  Tek tek kayıt ve Grup kayıt için farklı API'leri, ancak kanıtlama mekanizması için fark vardır.

Bu iş akışı izleyen bir kayıt girişi güncelleştirebilirsiniz:
* **Tek tek kayıt**:
    1. En son kayıt sağlama hizmetinden hizmet SDK API'si ile ilk Al ```getIndividualEnrollment```.
    2. Parametresi son kayıt gerektiği gibi değiştirin. 
    3. En son kayıt kullanarak hizmet SDK API çağrısı ```createOrUpdateIndividualEnrollment``` kayıt girişini güncelleştirmek için.
* **Grup kayıt**:
    1. En son kayıt sağlama hizmetinden hizmet SDK API'si ile ilk Al ```getEnrollmentGroup```.
    2. Parametresi son kayıt gerektiği gibi değiştirin.
    3. En son kayıt kullanarak hizmet SDK API çağrısı ```createOrUpdateEnrollmentGroup``` kayıt girişini güncelleştirmek için.

Bu iş akışı içinde gösterilen [örnekleri](#samples).

## <a name="remove-an-enrollment-entry"></a>Bir kayıt girişi Kaldır

* **Tek tek kayıt** hizmet SDK API'sini çağırarak silinebilir ```deleteIndividualEnrollment``` kullanarak ```registrationId```.
* **Grup kayıt** hizmet SDK API'sini çağırarak silinebilir ```deleteEnrollmentGroup``` kullanarak ```enrollmentGroupId```.

Bu iş akışı içinde gösterilen [örnekleri](#samples).

## <a name="bulk-operation-on-individual-enrollments"></a>Tek tek kayıtları toplu işlem

Oluştur, Güncelleştir veya bu iş akışını aşağıdaki birden çok tek tek kayıtları kaldırmak için toplu işlemi gerçekleştirebilir:

1. Birden çok içeren bir değişkeni oluşturma ```IndividualEnrollment```.  Bu değişken her dil için farklı uygulamasıdır.  Ayrıntılar için github'da toplu işlem örnek gözden geçirin.
2. Hizmet SDK API çağrısı ```runBulkOperation``` ile bir ```BulkOperationMode``` istenen işlem ve tek tek kayıtları için değişkeni. Dört modları desteklenir: oluşturma, updateIfMatchEtag, güncelleştirme ve silme.

Bir işlem başarılı bir şekilde gerçekleştirdikten sonra aygıtı sağlama hizmeti bir toplu işlem sonucu döndürür.

Bu iş akışı içinde gösterilen [örnekleri](#samples).
