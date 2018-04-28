---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/19/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: d407b015699925a6a3ce6ef9108ace1e9900303f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
## <a name="set-up-your-project"></a>Projenizin kurulumunu

Bu bölümde bir Windows Masaüstü .NET uygulaması (XAML) tümleştirme göstermek için yeni bir proje ile oluşturduğunuz *Microsoft ile oturum açma* uygulama bir belirteci gerektiren Web API'leri sorgulayabilmesi.

Bu kılavuz ile oluşturduğunuz uygulama bir grafik ekranında sonuçları görüntülemek için bir alana çağırmak için kullanılan bir düğme ve oturum kapatma düğmesini görüntüler.

> [!NOTE]
> Bunun yerine bu örneği ait Visual Studio projesi indirmeyi tercih ediyorsunuz? [Bir proje indirme](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip)ve geçin [yapılandırma adımı](#register-your-application) kod örneği, yürütmeden önce yapılandırmak için.
>

Uygulamanızı oluşturmak için aşağıdakileri yapın:
1. Visual Studio'da seçin **dosya** > **yeni** > **proje**.
2. Altında **şablonları**seçin **Visual C#**.
3. Seçin **WPF uygulaması** veya **WPF uygulaması**bağlı olarak kullanmakta olduğunuz Visual Studio sürümü.

## <a name="add-msal-to-your-project"></a>MSAL projenize ekleyin
1. Visual Studio'da seçin **Araçları** > **NuGet Paket Yöneticisi**> **Paket Yöneticisi Konsolu**.
2. Paket Yöneticisi konsolu penceresinde, aşağıdaki Azure PowerShell komutunu yapıştırın:

    ```powershell
    Install-Package Microsoft.Identity.Client -Pre
    ```

    > [!NOTE] 
    > Bu komut, Microsoft kimlik doğrulama kitaplığı yükler. MSAL alınırken önbelleğe alma ve Azure Active Directory v2 tarafından korunan API'leri erişmek için kullanılan kullanıcı belirteçleri yenilemeyi işler.
    >

## <a name="add-the-code-to-initialize-msal"></a>MSAL başlatmak için kod ekleme
Bu adımda, MSAL, etkileşim belirteçleri işleme gibi işlemek için bir sınıf oluşturun.

1. Açık *App.xaml.cs* dosyasını bulun ve ardından başvuru için MSAL sınıfına ekleyin:

    ```csharp
    using Microsoft.Identity.Client;
    ```
<!-- Workaround for Docs conversion bug -->

2. Uygulama sınıfı şu güncelleştirin:

    ```csharp
    public partial class App : Application
    {
        //Below is the clientId of your app registration. 
        //You have to replace the below with the Application Id for your app registration
        private static string ClientId = "your_client_id_here";

        public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);

    }
    ```

## <a name="create-the-application-ui"></a>Uygulama kullanıcı Arabirimi oluşturma

Bu bölümde, bir uygulama gibi Microsoft Graph korumalı bir arka uç sunucusu nasıl sorgulayabilir gösterir. 

A *MainWindow.xaml* dosya proje şablonu bir parçası olarak otomatik olarak oluşturulmalıdır. Bu dosyayı açın ve ardından, uygulamanızın değiştirin  *\<kılavuz >* aşağıdaki kodla düğümü:

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

