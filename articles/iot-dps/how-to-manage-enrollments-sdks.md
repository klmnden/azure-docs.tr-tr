---
title: Azure cihaz sağlama hizmeti SDK'ları kullanarak cihaz kayıtlarını yönetme | Microsoft Docs
description: IOT Hub cihaz sağlama hizmeti SDK'ları kullanarak hizmet cihaz kayıtlarını yönetme
services: iot-dps
keywords: ''
author: yzhong94
ms.author: yizhon
ms.date: 04/04/18
ms.topic: article
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 1ec86d319f529fe63b0924f4cfa0c2be178cd4d8
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="how-to-manage-device-enrollments-with-azure-device-provisioning-service-sdks"></a>Cihaz kayıtlarını Azure cihaz sağlama hizmeti SDK'ları ile yönetme
A *cihaz kaydı* tek bir cihazı veya grubun belirli bir noktada cihaz sağlama hizmeti ile kaydedebilir cihazların bir kayıt oluşturur. İlk istenen yapılandırma istenen IOT hub'ı dahil, kaydın parçası olarak aygıt kaydı içerir. Bu makalede Azure IOT sağlama hizmeti SDK'ları kullanarak programlı olarak sağlama hizmetiniz için cihaz kayıtlarını yönetme gösterilmektedir.  SDK'ları, Azure IOT SDK'ları ile aynı Havuzda github'da kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar
* Bağlantı dizesi, cihaz sağlama hizmeti örneğinin edinin.
* Aygıt için güvenlik yapılar elde [kanıtlama mekanizması](concepts-security.md#attestation-mechanism) kullanılır:
    * [**Güvenilir Platform Modülü (TPM)**](/azure/iot-dps/concepts-security#trusted-platform-module):
        * Tek tek kayıt: kayıt kimliği ve TPM onay anahtarını fiziksel aygıttan veya TPM Simulator.
        * Kayıt grubu TPM kanıtı için geçerli değildir.
    * [**X.509**](/azure/iot-dps/concepts-security):
        * Tek tek kayıt: [Yaprak sertifikası](/azure/iot-dps/concepts-security#leaf-certificate) fiziksel aygıttan veya SDK [BÖLMEK](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) öykünücüsü.
        * Kayıt Grup: [CA/kök sertifikası](/azure/iot-dps/concepts-security#root-certificate) veya [ara sertifika](/azure/iot-dps/concepts-security#intermediate-certificate), fiziksel bir aygıtı Aygıt sertifika üretmek için kullanılan.  Aynı zamanda SDK BÖLMEK öykünücüsünden oluşturulabilir.
* Tam API çağrıları dil farklılıkları nedeniyle farklı olabilir. Lütfen ayrıntılar için Github'da sağlanan örnekleri gözden geçirin:
   * [Java sağlama hizmet istemci örnekleri](https://github.com/Azure/azure-iot-sdk-java/tree/master/provisioning/provisioning-samples)
   * [Node.js sağlama hizmet istemci örnekleri](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/service/samples)
   * [.NET sağlama hizmet istemci örnekleri](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/provisioning/service/samples)

## <a name="create-a-device-enrollment"></a>Cihaz kaydı oluşturma
Aygıtlarınızı sağlama hizmeti ile kaydetmek için iki yolu vardır:

* Bir **kayıt grup** X.509 sertifika tarafından imzalanan ortak bir kanıtlama mekanizması paylaşan aygıtları bir grup için bir giriş [kök sertifika](https://docs.microsoft.com/azure/iot-dps/concepts-security#root-certificate) veya [ara sertifika ](https://docs.microsoft.com/azure/iot-dps/concepts-security#intermediate-certificate). Çok sayıda istenen ilk yapılandırma paylaşan aygıtları veya cihazları bir kayıt grubunu kullanarak aynı Kiracı tüm gitmeyi öneririz. Yalnızca X.509 kanıtlama mekanizması olarak kullanan cihazları kaydedebilmeniz için Not *kayıt grupları*. 

    Bu iş akışını aşağıdaki SDK'ları ile bir kayıt grubu oluşturabilirsiniz:

    1. Kayıt grubu için X.509 kök sertifikası kanıtlama mekanizması kullanır.  Hizmet SDK API çağrısı ```X509Attestation.createFromRootCertificate``` kanıtlama kayıt oluşturmak için kök sertifikası.  X.509 Sertifika PEM dosyasında veya dize olarak sağlanır.
    1. Yeni bir ```EnrollmentGroup``` değişken kullanarak ```attestation``` oluşturulan ve benzersiz bir ```enrollmentGroupId```.  İsteğe bağlı olarak, gibi parametreleri ayarlayabilirsiniz ```Device ID```, ```IoTHubHostName```, ```ProvisioningStatus```.
    2. Hizmet SDK API çağrısı ```createOrUpdateEnrollmentGroup``` ile arka uç uygulamanıza ```EnrollmentGroup``` bir kayıt grubu oluşturmak için.

* Bir **tek tek kayıt** kaydedebilir tek bir cihaz için bir giriş. Tek tek kayıtları kanıtlama mekanizmaları X.509 sertifikalarını veya SAS belirteçlerinden (fiziksel veya sanal TPM) kullanabilir. Benzersiz ilk yapılandırmaları gerektirir cihazlar için veya yalnızca TPM ya da sanal TPM aracılığıyla SAS belirteci kanıtlama mekanizması olarak kullanabileceğiniz cihazlar için tek tek kayıtları kullanmanızı öneririz. Bireysel kayıtlar için istenen IoT hub cihazı kimliği belirtilmiş olabilir.

    Bu iş akışını aşağıdaki SDK'ları ile tek bir kayıt oluşturabilirsiniz:
    
    1. Seçin, ```attestation``` TPM ya da X.509 mekanizması.
        1. **TPM**: fiziksel aygıttan veya TPM Simulator onay anahtarını giriş olarak kullanarak, hizmet SDK API'si çağırabilirsiniz ```TpmAttestation``` kanıtlama kayıt oluşturmak için. 
        2. **X.509**: giriş olarak istemci sertifikası kullanarak hizmeti SDK API'si çağırabilirsiniz ```X509Attestation.createFromClientCertificate``` kanıtlama kayıt oluşturmak için.
    2. Yeni bir ```IndividualEnrollment``` kullanarak değişken ```attestation``` oluşturulan ve benzersiz bir ```registrationId``` aygıtınızda veya TPM Simulator oluşturulan giriş olarak.  İsteğe bağlı olarak, gibi parametreleri ayarlayabilirsiniz ```Device ID```, ```IoTHubHostName```, ```ProvisioningStatus```.
    3. Hizmet SDK API çağrısı ```createOrUpdateIndividualEnrollment``` ile arka uç uygulamanıza ```IndividualEnrollment``` tek tek bir kayıt oluşturmak için.

Bir kayıt başarıyla oluşturduktan sonra cihaz hizmeti sağlama bir kayıt sonucunu döndürür. Bu iş akışı örnekleri gösterilmiştir [daha önce bahsedilen](#prerequisites).

## <a name="update-an-enrollment-entry"></a>Güncelleştirme kayıt girişi

Bir kayıt girişi oluşturduktan sonra kayıt güncelleştirmesi isteyebilirsiniz.  Olası senaryolar istenen özelliği güncelleştirmeyi, kanıtlama yöntemi güncelleştirme veya aygıt erişim hakkının iptali içerir.  Tek tek kayıt ve Grup kayıt için farklı API'leri, ancak kanıtlama mekanizması için fark vardır.

Bu iş akışı izleyen bir kayıt girişi güncelleştirebilirsiniz:
* **Bireysel kayıt**:
    1. En son kayıt sağlama hizmetinden hizmet SDK API'si ile ilk Al ```getIndividualEnrollment```.
    2. Parametresi son kayıt gerektiği gibi değiştirin. 
    3. En son kayıt kullanarak hizmet SDK API çağrısı ```createOrUpdateIndividualEnrollment``` kayıt girişini güncelleştirmek için.
* **Grup kayıt**:
    1. En son kayıt sağlama hizmetinden hizmet SDK API'si ile ilk Al ```getEnrollmentGroup```.
    2. Parametresi son kayıt gerektiği gibi değiştirin.
    3. En son kayıt kullanarak hizmet SDK API çağrısı ```createOrUpdateEnrollmentGroup``` kayıt girişini güncelleştirmek için.

Bu iş akışı örnekleri gösterilmiştir [daha önce bahsedilen](#prerequisites).

## <a name="remove-an-enrollment-entry"></a>Bir kayıt girişi Kaldır

* **Tek tek kayıt** hizmet SDK API'sini çağırarak silinebilir ```deleteIndividualEnrollment``` kullanarak ```registrationId```.
* **Grup kayıt** hizmet SDK API'sini çağırarak silinebilir ```deleteEnrollmentGroup``` kullanarak ```enrollmentGroupId```.

Bu iş akışı örnekleri gösterilmiştir [daha önce bahsedilen](#prerequisites).

## <a name="bulk-operation-on-individual-enrollments"></a>Tek tek kayıtları toplu işlem

Oluştur, Güncelleştir veya bu iş akışını aşağıdaki birden çok tek tek kayıtları kaldırmak için toplu işlemi gerçekleştirebilir:

1. Birden çok içeren bir değişkeni oluşturma ```IndividualEnrollment```.  Bu değişken her dil için farklı uygulamasıdır.  Ayrıntılar için github'da toplu işlem örnek gözden geçirin.
2. Hizmet SDK API çağrısı ```runBulkOperation``` ile bir ```BulkOperationMode``` istenen işlem ve tek tek kayıtları için değişkeni. Dört modları desteklenir: oluşturma, updateIfMatchEtag, güncelleştirme ve silme.

Bir işlem başarılı bir şekilde gerçekleştirdikten sonra aygıtı sağlama hizmeti bir toplu işlem sonucu döndürür.

Bu iş akışı örnekleri gösterilmiştir [daha önce bahsedilen](#prerequisites).
