<properties
    pageTitle="Microsoft Azure Stack POC'ye bağlanma | Microsoft Azure"
    description="Hizmet yöneticisi veya kiracı olarak Azure Stack POC'ye nasıl bağlanılacağını öğrenin."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/01/2016"
    ms.author="erikje"/>


# Azure Stack POC sanal makinesinde oturum açma

Azure Stack POC sanal makinesinde

- [**Hizmet yöneticisi**](#log-in-as-a-service-administrator) olarak oturum açabilir; böylece kaynak sağlayıcılarını, kiracı tekliflerini, planları, hizmetleri, kota ve fiyatlandırmayı yönetebilirsiniz.

veya

- [**Kiracı**](#log-in-as-a-tenant) olarak oturum açabilir; böylece Web Apps, depolama ve sanal makineler gibi abone olduğunuz hizmetleri sağlayabilir, izleyebilir ve yönetebilirsiniz.

## Hizmet yöneticisi olarak oturum açma

1.  Azure Stack POC fiziksel makinesinde oturum açın.

2.  **ClientVM.AzureStack.local.rdp** masaüstü simgesine çift tıklayarak istemci sanal makinesi için bir Uzak Masaüstü Bağlantısı açın.
 
    ![](media/azure-stack-connect-azure-stack/clientvmazurestacklocalicon.png)
    
    Bu otomatik olarak, dağıtım betiği tarafından oluşturulan AzureStack\\AzureStackUser hesabını kullanır. **Enter the password for the built-in administrator** (Yerleşik yönetici için parolayı girin) isteminde betik işleminin 5. adımında sağladığınız yönetici parolasını kullanın.

3.  ClientVM.AzureStack.local masaüstünde, **Microsoft Azure Stack POC Portal** simgesine (https://portal.azurestack.local/) çift tıklayarak [portalı](azure-stack-key-features.md#portal) açın.

    ![](media/azure-stack-connect-azure-stack/microsoftazurestackpocprtalicon.png)

4.  Hizmet yöneticisi hesabını kullanarak oturum açın.

## Kiracı olarak oturum açma

Bir hizmet yöneticisi; kiracılarının kullanabileceği planları, teklifleri ve abonelikleri test etmek için kiracı olarak oturum açabilir.
Kiracı hesabınız yoksa oturum açmadan önce [Kiracı hesabı oluşturun](azure-stack-add-new-user-aad.md).

1.  Azure Stack fiziksel makinesinde oturum açın.

2.  **ClientVM.AzureStack.local.rdp** masaüstü simgesine çift tıklayarak istemci sanal makinesi için bir Uzak Masaüstü Bağlantısı açın. 

    ![](media/azure-stack-connect-azure-stack/clientvmazurestacklocalicon.png)

    Bu otomatik olarak, dağıtım betiği tarafından oluşturulan AzureStack\\AzureStackUser hesabını kullanır. **Enter the password for the built-in administrator** (Yerleşik yönetici için parolayı girin) isteminde betik işleminin 5. adımında sağladığınız yönetici parolasını kullanın.

3.  ClientVM.AzureStack.local masaüstünde, **Microsoft Azure Stack POC Portal** simgesine (https://portal.azurestack.local/) çift tıklayarak [portalı](azure-stack-key-features.md#portal) açın.

    ![](media/azure-stack-connect-azure-stack/microsoftazurestackpocprtalicon.png)

4.  Kiracı hesabı kullanarak oturum açın.

RDP, fiziksel Microsoft Azure POC konağına erişebilecek kullanıcı sayısını kısıtlayabilir. Birden çok kullanıcının erişebilmesine olanak tanımak için bkz. [Birden çok eş zamanlı kullanıcı bağlantısını etkinleştirme](azure-stack-enable-multiple-concurrent-users.md).

## Sonraki adımlar

[İlk görevler](azure-stack-first-scenarios.md)



<!--HONumber=Sep16_HO3-->


