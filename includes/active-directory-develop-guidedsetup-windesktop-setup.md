---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: ce95e8d0249a886e031e3ae0fe9dd8e20804f391
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59498403"
---
## <a name="set-up-your-project"></a>Projenizi ayarlama

Bu bölümde ile nasıl tümleştireceğinizi Windows Masaüstü .NET uygulaması (XAML) göstermek için yeni bir proje oluşturma *Microsoft ile oturum açma* uygulama bir belirteç gerektiren bir Web API sorgulama yapabilirsiniz.

Çağrı grafı, ekrandaki sonuçları göstermek için bir alan için kullanılan bir düğme ve oturum kapatma düğmesi ile bu kılavuz, oluşturduğunuz uygulamayı görüntüler.

> [!NOTE]
> Bunun yerine bu örneği ait Visual Studio projeyi indirmek tercih ediyorsunuz? [Bir projeyi indirirken](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/msal3x.zip)ve atlamak [yapılandırma adımı](#register-your-application) yürütmeden önce onun kod örneği yapılandırmak için.
>

Uygulamanızı oluşturmak için aşağıdakileri yapın:

1. Visual Studio'da **dosya** > **yeni** > **proje**.
2. Altında **şablonları**seçin **Visual C#**.
3. Seçin **WPF uygulaması (.NET Framework)** kullanmakta olduğunuz Visual Studio sürümü bağlı olarak.

## <a name="add-msal-to-your-project"></a>MSAL projenize ekleyin.

1. Visual Studio’da **Araçlar** > **NuGet Paket Yöneticisi**> **Paket Yöneticisi Konsolu**’nu seçin.
2. Paket Yöneticisi konsolu penceresinde, aşağıdaki Azure PowerShell komutu yapıştırın:

    ```powershell
    Install-Package Microsoft.Identity.Client -Pre
    ```

    > [!NOTE] 
    > Bu komut, Microsoft kimlik doğrulama kitaplığı yükler. MSAL alınırken, önbelleğe alma ve Azure Active Directory v2.0 tarafından korunan API'lerine erişmek için kullanılan kullanıcı belirteçleri yenileme işleme
    >

## <a name="add-the-code-to-initialize-msal"></a>MSAL başlatmak için kodu ekleyin

Bu adımda, belirteçlerin işleme gibi MSAL, etkileşim işlemek için bir sınıf oluşturun.

1. Açık *App.xaml.cs* dosya ve başvuru için MSAL sınıfı ekleyin:

    ```csharp
    using Microsoft.Identity.Client;
    ```
   <!-- Workaround for Docs conversion bug -->

2. Uygulama sınıfına aşağıdaki gibi güncelleştirin:

    ```csharp
    public partial class App : Application
    {
        static App()
        {
            _clientApp = PublicClientApplicationBuilder.Create(ClientId)
                .WithAuthority(AzureCloudInstance.AzurePublic, Tenant)
                .Build();
        }

        // Below are the clientId (Application Id) of your app registration and the tenant information. 
        // You have to replace:
        // - the content of ClientID with the Application Id for your app registration
        // - Te content of Tenant by the information about the accounts allowed to sign-in in your application:
        //   - For Work or School account in your org, use your tenant ID, or domain
        //   - for any Work or School accounts, use `organizations`
        //   - for any Work or School accounts, or Microsoft personal account, use `common`
        //   - for Microsoft Personal account, use consumers
        private static string ClientId = "0b8b0665-bc13-4fdc-bd72-e0227b9fc011";

        private static string Tenant = "common";

        private static IPublicClientApplication _clientApp ;

        public static IPublicClientApplication PublicClientApp { get { return _clientApp; } }
    }
    ```

## <a name="create-the-application-ui"></a>Uygulama kullanıcı Arabirimi oluşturma

Bu bölümde, Microsoft Graph gibi korumalı bir arka uç sunucusu uygulama nasıl sorgulayabilirsiniz gösterilir. 

A *MainWindow.xaml* dosyası, proje şablonunun bir parçası olarak otomatik olarak oluşturulmalıdır. Bu dosyayı açın ve ardından uygulamanızın değiştirin  *\<kılavuz >* aşağıdaki kodla düğüm:

```xml
<Grid>
    <StackPanel Background="Azure">
        <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
            <Button x:Name="CallGraphButton" Content="Call Microsoft Graph API" HorizontalAlignment="Right" Padding="5" Click="CallGraphButton_Click" Margin="5" FontFamily="Segoe Ui"/>
            <Button x:Name="SignOutButton" Content="Sign-Out" HorizontalAlignment="Right" Padding="5" Click="SignOutButton_Click" Margin="5" Visibility="Collapsed" FontFamily="Segoe Ui"/>
        </StackPanel>
        <Label Content="API Call Results" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="ResultText" TextWrapping="Wrap" MinHeight="120" Margin="5" FontFamily="Segoe Ui"/>
        <Label Content="Token Info" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="TokenInfoText" TextWrapping="Wrap" MinHeight="70" Margin="5" FontFamily="Segoe Ui"/>
    </StackPanel>
</Grid>
```
