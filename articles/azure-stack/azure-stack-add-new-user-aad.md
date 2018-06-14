---
title: Azure Active Directory'de yeni bir Azure yığın Kiracı hesap eklemek | Microsoft Docs
description: Microsoft Azure yığın Geliştirme Seti dağıttıktan sonra Kiracı portalı gözatabilirsiniz en az bir Kiracı Kullanıcı hesabı oluşturmanız gerekir.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: a75d5c88-5b9e-4e9a-a6e3-48bbfa7069a7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.openlocfilehash: 590426563936c66b1353f769be138759bb53f58c
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
ms.locfileid: "29553214"
---
# <a name="add-a-new-azure-stack-tenant-account-in-azure-active-directory"></a>Azure Active Directory'de yeni bir Azure yığın Kiracı hesabı Ekle
Sonra [Azure yığın Geliştirme Seti dağıtma](azure-stack-run-powershell-script.md), Kiracı Portalı'nı keşfedin ve test sunar ve planları için bir Kiracı Kullanıcı hesabınızın olması gerekir. Bir kiracı hesap tarafından oluşturabilirsiniz [Azure portalını kullanarak](#create-an-azure-stack-tenant-account-using-the-azure-portal) veya [PowerShell kullanarak](#create-an-azure-stack-tenant-account-using-powershell).

## <a name="create-an-azure-stack-tenant-account-using-the-azure-portal"></a>Azure Portalı'nı kullanarak bir Azure yığın Kiracı hesabı oluşturma
Azure Portalı'nı kullanmak için bir Azure aboneliğinizin olması gerekir.

1. Oturum [Azure](https://portal.azure.com).
2. Microsoft Azure sol gezinti çubuğunda, **Active Directory**.
3. Dizin listesinde Azure yığını için kullanmak istediğiniz dizini tıklatın veya yeni bir tane oluşturun.
4. Bu dizin sayfasında, tıklatın **kullanıcılar**.
5. Tıklatın **Kullanıcı Ekle**.
6. İçinde **Kullanıcı Ekle** sihirbazında **kullanıcı türünü** listesinde, seçin **kuruluşunuzdaki yeni kullanıcı**.
7. İçinde **kullanıcı adı** kullanıcı için bir ad yazın.
8. İçinde  **@**  kutusunda, uygun girişi seçin.
9. İleri okuna tıklayın.
10. İçinde **kullanıcı profili** sihirbaz sayfasında bir **ad**, **Soyadı**, ve **görünen adı**.
11. İçinde **rol** listesinde, seçin **kullanıcı**.
12. İleri okuna tıklayın.
13. Üzerinde **Get geçici parola** sayfasında, **oluşturma**.
14. Kopya **yeni parola**.
15. Microsoft Azure için yeni hesabıyla oturum açın. İstendiğinde parolayı değiştirin.
16. Oturum `https://portal.local.azurestack.external` Kiracı Portalı'nı görmek için yeni hesabı ile.

## <a name="create-an-azure-stack-tenant-account-using-powershell"></a>PowerShell kullanarak bir Azure yığın Kiracı hesabı oluşturma
Bir Azure aboneliğiniz yoksa, bir Kiracı Kullanıcı hesabı eklemek için Azure Portalı'nı kullanamazsınız. Bu durumda, bunun yerine Azure Active Directory için Windows PowerShell modülü kullanabilirsiniz.

> [!NOTE]
> Azure yığın Geliştirme Seti dağıtmak için Microsoft Account (Live ID) kullanıyorsanız, Kiracı hesap oluşturmak için AAD PowerShell kullanamazsınız. 
> 
> 

1. Yükleme [Microsoft Online Services oturum açma Yardımcısı BT uzmanları RTW için](https://www.microsoft.com/en-us/download/details.aspx?id=41950).
2. Yükleme [Azure Active Directory için Windows PowerShell Modülü (64-bit sürümü)](http://go.microsoft.com/fwlink/p/?linkid=236297) ve açın.
3. Aşağıdaki cmdlet'leri çalıştırın:

    ```powershell
    # Provide the AAD credential you use to deploy Azure Stack Development Kit

            $msolcred = get-credential

    # Add a tenant account "Tenant Admin <username>@<yourdomainname>" with the initial password "<password>".

            connect-msolservice -credential $msolcred
            $user = new-msoluser -DisplayName "Tenant Admin" -UserPrincipalName <username>@<yourdomainname> -Password <password>
            Add-MsolRoleMember -RoleName "Company Administrator" -RoleMemberType User -RoleMemberObjectId $user.ObjectId

    ```

1. Microsoft Azure için yeni hesabıyla oturum açın. İstendiğinde parolayı değiştirin.
2. Oturum `https://portal.local.azurestack.external` Kiracı Portalı'nı görmek için yeni hesabı ile.

