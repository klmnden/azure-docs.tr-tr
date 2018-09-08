---
title: Microsoft Azure Stack doğrulama hizmet olarak yazılım güncelleştirmelerini doğrulama | Microsoft Docs
description: Doğrulama bir hizmet olarak yazılım güncelleştirmelerini Microsoft gelen doğrulama öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 6ef8c0486a694ac44c53375b24893812b10343e4
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44158492"
---
# <a name="validate-software-updates-from-microsoft"></a>Microsoft yazılım güncelleştirmeleri doğrulayın

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Microsoft Azure Stack yazılım güncelleştirmeleri düzenli olarak serbest bırakır. Bu güncelleştirmeler, güncelleştirmeleri çözümleri karşı doğrulamak ve Microsoft'a geri bildirimde genel kullanıma açık yapılan tarifelerindeki ortak mühendislik iş ortaklarının Azure Stack için sağlanır.

## <a name="test-an-existing-solution"></a>Varolan bir çözümü test etme

1. Oturum [doğrulama portalı](https://azurestackvalidation.com).

2. Burada güncelleştirilmiş Microsoft gelen dağıtıldıktan ve seçin için varolan bir çözümü seçin **Başlat** üzerinde **paket doğrulama** Döşe.

    ![Paket doğrulaması](media/image3.png)

3. Doğrulama adını girin.

4. Çözüm üzerinde dağıtım sırasında yüklenen OEM paketin URL'sini girin. Azure blob hizmette depolanan paketi için URL'yi kullanın. Daha fazla bilgi için [günlüklerini depolamak için bir Azure depolama blob oluşturma](azure-stack-vaas-set-up-account.md#create-an-azure-storage-blob-to-store-logs).

5. Seçin **karşıya** , dağıtım yapılandırma dosyası eklemek için. Başvurmak [yeni bir Azure Stack çözüm doğrulama](azure-stack-vaas-validate-solution-new.md) dağıtım yapılandırma dosyanızı karşıya yükleme hakkında bilgi.

6. Dağıtım yapılandırma dosyası ardından olmalıdır doğru ortamı parametre dosyasını ile özelleştirilmiş bkz [ortam parametrelerini](azure-stack-vaas-parameters.md#environment-parameters) ek ayrıntılar için.

    > [!Note]   
    > Dağıtım yapılandırma dosyası, ortak test parametreleri ekleyerek daha fazla özelleştirilebilir. Daha fazla bilgi için [iş akışı ortak parametreleri için bir hizmet olarak Azure Stack doğrulama](azure-stack-vaas-parameters.md)

7. Kullanıcı adı ve parola Kiracı Kullanıcı, Hizmet Yöneticisi ve bulut yönetici el ile girilmesi gerekir.

8. Tanılama günlüklerini depolamak için Azure depolama blobu URL'si sağlayın. Daha fazla bilgi için [günlüklerini depolamak için bir Azure depolama blob oluşturma](azure-stack-vaas-set-up-account.md#create-an-azure-storage-blob-to-store-logs).

    > [!Note]  
    > İş akışı etiketlemek için açıklayıcı bir etiket girilebilir.

10. Seçin **Gönder** iş akışı kaydedin.

Çözümü iş akışı yaklaşık 24 saat boyunca çalışır. Bir bağlantı veya yönerge testleri zamanlama üzerinde ekleyin. Temizleyin Aracı'nda.

Bir doğrulama ilerlemesini izleme hakkında daha fazla bilgi bulmak için bkz: [bir testi izlenecek ](azure-stack-vaas-monitor-test.md).

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında daha fazla bilgi edinmek için [hizmet olarak Azure Stack doğrulama](https://docs.microsoft.com/azure/azure-stack/partner).
