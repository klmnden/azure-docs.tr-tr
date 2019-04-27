---
title: Azure cihaz sağlama hizmeti SDK'ları kullanarak cihaz kayıtlarını yönetme | Microsoft Docs
description: IOT Hub cihazı sağlama hizmeti SDK'larını kullanarak hizmeti, cihaz kayıtlarını yönetme
author: yzhong94
ms.author: yizhon
ms.date: 04/04/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: arjmands
ms.openlocfilehash: c73a40e46d86632732454ae16ea4f83e3ffa0281
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60627278"
---
# <a name="how-to-manage-device-enrollments-with-azure-device-provisioning-service-sdks"></a>Azure cihaz sağlama hizmeti SDK'ları ile cihaz kayıtlarını yönetme
A *cihaz kaydı* tek bir cihaz veya bir grubun belirli bir noktada cihaz sağlama Hizmeti'ne kaydolabilecek cihazların bir kayıt oluşturur. Kayıt için istenen IOT hub'ı dahil olmak üzere, bu kayıt işleminin bir parçası olarak cihaz ilk istenen yapılandırmayı içerir. Bu makalede, sağlama hizmetinizin programlama yoluyla Azure IOT sağlama hizmeti SDK'ları kullanarak cihaz kayıtlarını yönetme işlemini göstermektedir.  SDK'ları, Azure IOT SDK'ları ile aynı depodaysa github'da kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar
* Cihaz sağlama hizmeti örneğinizi bağlantı dizesini edinin.
* İçin güvenlik yapılarını cihaz elde [kanıtlama mekanizması](concepts-security.md#attestation-mechanism) kullanılır:
    * [**Güvenilir Platform Modülü (TPM)**](/azure/iot-dps/concepts-security#trusted-platform-module):
        * Bireysel kayıt: Kayıt kimliği ve fiziksel bir cihaz veya TPM Simülatörünü TPM onay anahtarı.
        * Kayıt grubu TPM kanıtlama için geçerli değildir.
    * [**X.509**](/azure/iot-dps/concepts-security):
        * Bireysel kayıt: [Yaprak sertifikayı](/azure/iot-dps/concepts-security) fiziksel CİHAZDAN veya SDK'dan [DICE](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) öykünücüsü.
        * Kayıt grubu: [CA/kök sertifikası](/azure/iot-dps/concepts-security#root-certificate) veya [ara sertifika](/azure/iot-dps/concepts-security#intermediate-certificate)fiziksel bir cihazda cihaz sertifikası oluşturmak için kullanılır.  SDK'sı DICE öykünücüsünden de meydana gelebilir.
* Tam API çağrıları dil farklar nedeniyle farklı olabilir. Lütfen ayrıntılar için GitHub üzerinde sağlanan örnekleri inceleyin:
   * [Sağlama hizmeti istemci Java örnekleri](https://github.com/Azure/azure-iot-sdk-java/tree/master/provisioning/provisioning-samples)
   * [Node.js sağlama hizmeti istemci örnekleri](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/service/samples)
   * [Sağlama hizmeti istemci .NET örnekleri](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/provisioning/service/samples)

## <a name="create-a-device-enrollment"></a>Cihaz kaydı oluşturma
Sağlama hizmeti ile cihazlarınızı kaydedebilirsiniz iki yolu vardır:

* Bir **kayıt grubu** cihazlar tarafından imzalanan X.509 Sertifika, ortak bir kanıtlama mekanizmasını paylaşan bir grup için bir giriş [kök sertifika](https://docs.microsoft.com/azure/iot-dps/concepts-security#root-certificate) veya [ara sertifika ](https://docs.microsoft.com/azure/iot-dps/concepts-security#intermediate-certificate). Aynı kiracıya giden tüm cihazlar veya çok sayıda istenen bir ilk yapılandırmayı paylaşan cihazlar için kayıt grubu kullanılması önerilir. Yalnızca X.509 kanıtlama mekanizması olarak kullanan cihazları kaydedebilmeniz için Not *kayıt grupları*. 

    Bu iş akışını izleyerek SDK'ları ile bir kayıt grubu oluşturabilirsiniz:

    1. Kayıt grubu için X.509 Sertifika kanıtlama mekanizması kullanır.  Hizmet SDK'sı API çağrısı ```X509Attestation.createFromRootCertificate``` kanıtlama için kayıt oluşturmak için kök sertifikası ile.  X.509 Sertifika bir PEM dosyası veya bir dize olarak sağlanır.
    1. Yeni bir ```EnrollmentGroup``` değişken kullanarak ```attestation``` oluşturulan ve benzersiz bir ```enrollmentGroupId```.  Parametreleri gibi isteğe bağlı olarak ayarlayabileceğiniz ```Device ID```, ```IoTHubHostName```, ```ProvisioningStatus```.
    2. Hizmet SDK'sı API çağrısı ```createOrUpdateEnrollmentGroup``` ile arka uç uygulamanıza ```EnrollmentGroup``` kayıt grubu oluşturmak için.

* Bir **bireysel kayıt** kaydedebilir, tek bir cihaz için bir giriş. Bireysel kayıtlar, kanıtlama mekanizması olarak X.509 sertifikalarını veya SAS belirteçlerini (Başlangıç, fiziksel ya da sanal TPM'de) kullanabilir. Benzersiz ilk yapılandırma gerektiren cihazlar için veya kanıtlama mekanizması olarak TPM ya da sanal TPM aracılığıyla SAS belirteçleri yalnızca kullanabileceğiniz cihazlar için bireysel kayıtların kullanılmasını öneririz. Bireysel kayıtlar için istenen IoT hub cihazı kimliği belirtilmiş olabilir.

    Bu iş akışını izleyerek SDK'ları ile bireysel kayıt oluşturabilirsiniz:
    
    1. Seçin, ```attestation``` TPM veya X.509 mekanizması.
        1. **TPM**: Giriş olarak fiziksel bir cihaz veya TPM Simülatörünü onay anahtarını kullanarak hizmet SDK API'si çağırabilirsiniz ```TpmAttestation``` kanıtlama için kayıt oluşturmak için. 
        2. **X.509**: Giriş olarak istemci sertifikası kullanarak hizmet SDK API'si çağırabilirsiniz ```X509Attestation.createFromClientCertificate``` kanıtlama için kayıt oluşturmak için.
    2. Yeni bir ```IndividualEnrollment``` değişken kullanarak ```attestation``` oluşturulan ve benzersiz bir ```registrationId``` Cihazınızda veya TPM Simülatörünü oluşturulan giriş olarak.  Parametreleri gibi isteğe bağlı olarak ayarlayabileceğiniz ```Device ID```, ```IoTHubHostName```, ```ProvisioningStatus```.
    3. Hizmet SDK'sı API çağrısı ```createOrUpdateIndividualEnrollment``` ile arka uç uygulamanıza ```IndividualEnrollment``` bireysel kayıt oluşturmak için.

Bir kayıt başarıyla oluşturduktan sonra cihaz sağlama hizmeti bir kayıt sonucunu döndürür. Bu iş akışı örnekleri gösterilmiştir [daha önce bahsedilen](#prerequisites).

## <a name="update-an-enrollment-entry"></a>Güncelleştirme kayıt girişi

Bir kayıt girişi oluşturduktan sonra kayıt güncelleştirmek isteyebilirsiniz.  İstenen özellik güncelleştirme, kanıtlama yöntemi güncelleştiriliyor veya cihaz erişimini iptal olası senaryolar şunlardır.  Bireysel kayıt ve Grup kaydı için farklı API'leri, ancak kanıtlama mekanizması için fark vardır.

Bu iş akışını izleyerek kayıt girişi güncelleştirebilirsiniz:
* **Bireysel kayıt**:
    1. En son kayıt sağlama hizmetinden hizmet SDK'sı API ile ilk ulaşmak ```getIndividualEnrollment```.
    2. Gerektiği gibi en son kayıt parametresi değiştirin. 
    3. Hizmet SDK'sı API çağrısı kullanarak en son kayıt ```createOrUpdateIndividualEnrollment``` kayıt girişiniz güncelleştirilecek.
* **Grup kaydı**:
    1. En son kayıt sağlama hizmetinden hizmet SDK'sı API ile ilk ulaşmak ```getEnrollmentGroup```.
    2. Gerektiği gibi en son kayıt parametresi değiştirin.
    3. Hizmet SDK'sı API çağrısı kullanarak en son kayıt ```createOrUpdateEnrollmentGroup``` kayıt girişiniz güncelleştirilecek.

Bu iş akışı örnekleri gösterilmiştir [daha önce bahsedilen](#prerequisites).

## <a name="remove-an-enrollment-entry"></a>Bir kayıt girdisini Kaldır

* **Bireysel kayıt** hizmet SDK API'si çağırarak silinebilir ```deleteIndividualEnrollment``` kullanarak ```registrationId```.
* **Grup kaydı** hizmet SDK API'si çağırarak silinebilir ```deleteEnrollmentGroup``` kullanarak ```enrollmentGroupId```.

Bu iş akışı örnekleri gösterilmiştir [daha önce bahsedilen](#prerequisites).

## <a name="bulk-operation-on-individual-enrollments"></a>Bireysel kayıtlar üzerinde toplu işlemi

Toplu işlemin oluşturmak, güncelleştirmek veya bu iş akışını izleyerek birden fazla bireysel kayıtlar kaldırmak için gerçekleştirebilirsiniz:

1. Birden çok içeren bir değişken oluşturma ```IndividualEnrollment```.  Bu değişken her dil için farklı uygulamasıdır.  Ayrıntılı bilgi için github'daki toplu işlem örnek gözden geçirin.
2. Hizmet SDK'sı API çağrısı ```runBulkOperation``` ile bir ```BulkOperationMode``` istenen işlemi ve bireysel kayıtlar değişkeninizin. Dört modları desteklenir: oluşturma, updateIfMatchEtag, güncelleştirme ve silme.

Başarılı bir şekilde bir işlemi gerçekleştirdikten sonra cihaz sağlama hizmeti toplu işleminin sonucunu döndürür.

Bu iş akışı örnekleri gösterilmiştir [daha önce bahsedilen](#prerequisites).
