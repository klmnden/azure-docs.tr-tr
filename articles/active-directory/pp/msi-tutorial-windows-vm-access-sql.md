---
title: "Azure SQL erişmek için bir Windows VM MSI kullanın"
description: "Azure SQL erişmek için bir Windows VM yönetilen hizmet kimliği (MSI) kullanma sürecinde anlatan öğretici."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: skwan
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 6e7b431655d84c6371c62bbab83244ac88391442
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="use-a-windows-vm-managed-service-identity-msi-to-access-azure-sql"></a>Azure SQL erişmek için bir Windows VM yönetilen hizmet kimliği (MSI) kullanın

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Bu öğretici, bir yönetilen hizmet Kimliği'ni (MSI) bir Windows sanal makine (VM) için bir Azure SQL sunucusuna erişmek için nasıl kullanılacağını gösterir. Yönetilen hizmet kimliği Azure tarafından otomatik olarak yönetilir ve Azure AD kimlik doğrulaması, kimlik bilgileri kodunuza eklemek zorunda kalmadan destekleyen hizmetler için kimlik doğrulaması sağlar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir Windows VM MSI etkinleştir 
> * Bir Azure SQL server, VM erişim
> * VM kimliğini kullanarak bir erişim belirteci almak ve bir Azure SQL server'ı sorgulamak için kullanın

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresindeki Azure portalında oturum açın.

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Windows sanal makine oluşturma

Bu öğretici için yeni bir Windows VM oluşturun.  Mevcut bir VM'yi üzerinde MSI de etkinleştirebilirsiniz.

1.  Azure portalının sol üst köşesinde bulunan **Yeni** düğmesine tıklayın.
2.  **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. 
3.  Sanal makine bilgilerini girin. **Kullanıcıadı** ve **parola** için kullandığınız kimlik bilgileri İşte oluşturulan sanal makineye oturum açma.
4.  Uygun seçin **abonelik** sanal makine açılır.
5.  Yeni bir seçmek için **kaynak grubu** içinde sanal makine oluşturmak, seçim **Yeni Oluştur**. İşlem tamamlandığında **Tamam**’a tıklayın.
6.  VM boyutunu seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. Ayarlar sayfasında, Varsayılanları tutun ve tıklatın **Tamam**.

    ![Alt görüntü metin](~/articles/active-directory/media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>MSI VM üzerinde etkinleştir 

Bir VM MSI erişim belirteçleri, kimlik bilgileri kodunuza koyma gereksinimi olmadan Azure AD'den almanızı sağlar. MSI etkinleştirme, VM için yönetilen bir kimlik oluşturmak üzere Azure söyler. Perde arkasında MSI etkinleştirme iki işlemi yapar: MSI VM uzantısı, VM yükler ve MSI Azure Kaynak Yöneticisi'nde sağlar.

1.  Seçin **sanal makine** MSI etkinleştirmek istediğiniz.  
2.  Sol gezinti çubuğunda **yapılandırma**. 
3.  Gördüğünüz **yönetilen hizmet kimliği**. Kaydolun ve MSI etkinleştirmek için seçin **Evet**, devre dışı bırakmak istiyorsanız seçin No 
4.  Tıklattığınız olun **kaydetmek** yapılandırmayı kaydetmek için.  
    ![Alt görüntü metin](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Denetleyin ve bu VM uzantıları doğrulamak isterseniz, tıklatın **uzantıları**. MSI, ardından etkinse **ManagedIdentityExtensionforWindows** listede görüntülenir.

    ![Alt görüntü metin](~/articles/active-directory/media/msi-tutorial-windows-vm-access-arm/msi-windows-extension.png)

## <a name="grant-your-vm-access-to-a-database-in-an-azure-sql-server"></a>Bir Azure SQL server veritabanında, VM erişim

Şimdi bir Azure SQL server veritabanında, VM erişim izni verebilir.  Bu adım için mevcut bir SQL server kullanın veya yeni bir tane oluşturun.  Yeni bir sunucu oluşturmak ve Azure portalını kullanarak veritabanı için bu izleyin [Azure SQL quickstart](~/articles/sql-database/sql-database-get-started-portal.md). Ayrıca Azure CLI ve Azure PowerShell'de kullanın quickstarts olan [Azure SQL belgelerine](~/articles/sql-database/index.yml).

Bir veritabanı, VM erişim verilmesi için üç adım vardır:
1.  Azure AD'de bir grup oluşturun ve VM MSI grubunun bir üyesi yapın.
2.  SQL server için Azure AD kimlik doğrulamasını etkinleştirin.
3.  Oluşturma bir **içerilen kullanıcı** veritabanında Azure AD grubunu temsil eder.

> [!NOTE]
> Normalde doğrudan VM MSI eşleyen içerilen kullanıcı oluşturursunuz.  Şu anda, Azure SQL kapsanan bir kullanıcıya eşlenmesi VM MSI temsil eden Azure AD hizmet sorumlusu izin vermiyor.  Desteklenen bir geçici çözüm olarak VM MSI bir Azure AD grubunun bir üyesi yapın sonra içerilen kullanıcı grubu temsil eden veritabanında oluşturun.


### <a name="create-a-group-in-azure-ad-and-make-the-vm-msi-a-member-of-the-group"></a>Azure AD'de bir grup oluşturun ve VM MSI grubunun bir üyesi yapın

Varolan bir Azure AD grubunu kullanın veya Azure AD PowerShell kullanarak yeni bir tane oluşturun.  

İlk olarak, yükleme [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) modülü. Kullanarak oturum `Connect-AzureAD`, grubu oluşturmak için aşağıdaki komutu çalıştırın ve bir değişkene kaydedin:

```powershell
$Group = New-AzureADGroup -DisplayName "VM MSI access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
```

Çıktı ayrıca değişkenin değerini inceler aşağıdaki gibi görünür:

```powershell
$Group = New-AzureADGroup -DisplayName "VM MSI access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
$Group
ObjectId                             DisplayName          Description
--------                             -----------          -----------
6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 VM MSI access to SQL
```

Ardından, VM'nin MSI grubuna ekleyin.  MSI gereksinim **objectID**, hangi Azure PowerShell kullanarak elde.  İlk olarak, indirme [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Kullanarak oturum `Login-AzureRmAccount`, ve aşağıdaki komutları çalıştırın:
- Birden çok olanları varsa, oturum bağlamı istediğiniz Azure aboneliği için ayarlandığından emin olun.
- Azure aboneliğinizde kullanılabilir kaynaklar listesinde, VM adları ve doğru kaynak grubu içinde doğrulayın.
- İçin uygun değerleri kullanarak MSI sanal makinenin özelliklerini al `<RESOURCE-GROUP>` ve `<VM-NAME>`.

```powershell
Set-AzureRMContext -subscription "bdc79274-6bb9-48a8-bfd8-00c140fxxxx"
Get-AzureRmResource
$VM = Get-AzureRmVm -ResourceGroup <RESOURCE-GROUP> -Name <VM-NAME>
```

Çıktı ayrıca VM MSI hizmet asıl nesne kimliği inceler aşağıdaki gibi görünür:
```powershell
$VM = Get-AzureRmVm -ResourceGroup DevTestGroup -Name DevTestWinVM
$VM.Identity.PrincipalId
b83305de-f496-49ca-9427-e77512f6cc64
```

Şimdi VM MSI grubuna ekleyin.  Bu gibi durumlarda, bir hizmet sorumlusu yalnızca Azure AD PowerShell kullanarak bir gruba ekleyebilirsiniz.  Şu komutu çalıştırın:
```powershell
Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId $VM.Identity.PrincipalId
```

Ayrıca Grup üyeliği daha sonra incelerseniz, çıktı şu şekilde görünür:

```powershell
Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId $VM.Identity.PrincipalId
Get-AzureAdGroupMember -ObjectId $Group.ObjectId

ObjectId                             AppId                                DisplayName
--------                             -----                                -----------
b83305de-f496-49ca-9427-e77512f6cc64 0b67a6d6-6090-4ab4-b423-d6edda8e5d9f DevTestWinVM
```

### <a name="enable-azure-ad-authentication-for-the-sql-server"></a>SQL server için Azure AD kimlik doğrulamasını etkinleştir

Grup oluşturulur ve üyelik VM MSI eklenir, yapabilecekleriniz [SQL server için Azure AD kimlik doğrulamasını yapılandırma](~/articles/sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-server) aşağıdaki adımları kullanarak:

1.  Azure portalında seçin **SQL sunucuları** sol gezinti gelen.
2.  Azure AD kimlik doğrulaması için etkin için SQL server'ı tıklatın.
3.  İçinde **ayarları** bölüm için dikey pencerenin tıklatın **Active Directory Yöneticisi**.
4.  Komut çubuğunda, **ayarlamak yönetici**.
5.  Sunucu Yöneticisi yapılması ve bir Azure AD kullanıcı hesabını seçin **seçin.**
6.  Komut çubuğunda, **kaydedin.**

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Azure AD grubu temsil eden veritabanında içerilen kullanıcı oluşturmak

Bu sonraki adım için size gereken [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). Başlamadan önce ayrıca Azure AD tümleştirme hakkında arka plan bilgileri için aşağıdaki makalelere gözden geçirmek yararlı olabilir:

- [SQL Database ve SQL Data Warehouse (MFA desteği SSMS) ile Evrensel kimlik doğrulaması](~/articles/sql-database/sql-database-ssms-mfa-authentication.md)
- [Yapılandırma ve SQL Database veya SQL Data Warehouse ile Azure Active Directory kimlik doğrulaması yönetme](~/articles/sql-database/sql-database-aad-authentication-configure.md)

1.  SQL Server Management Studio’yu başlatın.
2.  İçinde **sunucuya Bağlan** iletişim kutusunda, SQL server'ınızın adını Enter **sunucu adı** alan.
3.  İçinde **kimlik doğrulaması** alan, select **Active Directory - Evrensel MFA desteğiyle**.
4.  İçinde **kullanıcı adı** alanında, sunucu yöneticisi olarak, örneğin, ayarladığınız Azure AD hesabının adını girinhelen@woodgroveonline.com
5.  **Seçenekler**’e tıklayın.
6.  İçinde **veritabanına bağlan** alanında, yapılandırmak istediğiniz sistem dışı veritabanının adını girin.
7.  **Bağlan**'a tıklayın.  Oturum açma işlemini tamamlayın.
8.  İçinde **Object Explorer**, genişletin **veritabanları** klasör.
9.  Bir kullanıcı veritabanı üzerinde sağ tıklatın ve **yeni sorgu**.
10.  Sorgu penceresinde, aşağıdaki satırı girin ve **yürütme** araç çubuğunda:
    
     ```
     CREATE USER [VM MSI access to SQL] FROM EXTERNAL PROVIDER
     ```
    
     Komut grubu için kapsanan kullanıcı oluşturma başarıyla tamamlanmalıdır.
11.  Sorgu penceresi temizleyin, aşağıdaki satırı girin ve tıklatın **yürütme** araç çubuğunda:
     
     ```
     ALTER ROLE db_datareader ADD MEMBER [VM MSI access to SQL]
     ```

     Komut, başarıyla, kapsanan kullanıcı tüm veritabanını okuma özelliği verme tamamlamanız gerekir.

VM içinde çalışan kod artık MSI bir belirteç almak ve SQL sunucusunda kimlik doğrulamak için belirteci kullanın.

## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-call-azure-sql"></a>VM kimliğini kullanarak bir erişim belirteci alın ve Azure SQL çağırmak için kullanın 

Azure SQL MSI kullanarak doğrudan erişim belirteçleri kabul edebilir destekler Azure AD kimlik doğrulama, yerel olarak alınır.  Kullandığınız **erişim belirteci** SQL bağlantısı oluşturma yöntemi.  Bu Azure AD ile tümleştirme Azure SQL'ın bir parçasıdır ve bağlantı dizesini kimlik bilgileri sağladığını farklıdır.

Aşağıda, bir erişim belirteci kullanarak SQL bağlantı açma .net kod örneği verilmiştir.  Bu kod, VM'de VM MSI uç noktasına erişmek için çalıştırmanız gerekir.  **.NET framework 4.6** ya da daha yüksek erişim belirteci yöntemi kullanmak için gereklidir.  AZURE SQL SERVERNAME ve veritabanı değerlerini değiştirmek uygun şekilde.  Azure SQL kaynak kimliği "https://database.windows.net/" olduğuna dikkat edin.

```csharp
using System.Net;
using System.IO;
using System.Data.SqlClient;
using System.Web.Script.Serialization;

//
// Get an access token for SQL.
//
HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://localhost:50342/oauth2/token?resource=https://database.windows.net/");
request.Headers["Metadata"] = "true";
request.Method = "GET";
string accessToken = null;

try
{
    // Call MSI endpoint.
    HttpWebResponse response = (HttpWebResponse)request.GetResponse();

    // Pipe response Stream to a StreamReader and extract access token.
    StreamReader streamResponse = new StreamReader(response.GetResponseStream()); 
    string stringResponse = streamResponse.ReadToEnd();
    JavaScriptSerializer j = new JavaScriptSerializer();
    Dictionary<string, string> list = (Dictionary<string, string>) j.Deserialize(stringResponse, typeof(Dictionary<string, string>));
    accessToken = list["access_token"];
}
catch (Exception e)
{
    string errorText = String.Format("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
}

//
// Open a connection to the SQL server using the access token.
//
if (accessToken != null) {
    string connectionString = "Data Source=<AZURE-SQL-SERVERNAME>; Initial Catalog=<DATABASE>;";
    SqlConnection conn = new SqlConnection(connectionString);
    conn.AccessToken = accessToken;
    conn.Open();
}
```

Alternatif olarak, yazma ve VM üzerinde bir uygulama dağıtmak zorunda kalmadan uçtan uca Kurulum test etmek için hızlı bir şekilde PowerShell kullanıyor.

1.  Portalı'nda gidin **sanal makineleri** ve Windows sanal makinenizi gidin ve buna **genel bakış**, tıklatın **Bağlan**. 
2.  Girin, **kullanıcıadı** ve **parola** Windows VM oluşturduğunuzda, eklediğiniz için. 
3.  Oluşturduğunuza göre bir **Uzak Masaüstü Bağlantısı** sanal makineyle açmak **PowerShell** uzak oturumda. 
4.  PowerShell'in kullanarak `Invoke-WebRequest`, Azure SQL için bir erişim belirteci almak üzere yerel MSI uç nokta için bir istek olun.

    ```powershell
       $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://database.windows.net/"} -Headers @{Metadata="true"}
    ```
    
    Yanıt için bir PowerShell nesnesi bir JSON nesnesinde dönüştürün. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```

    Erişim belirteci yanıttan ayıklayın.
    
    ```powershell
    $AccessToken = $content.access_token
    ```

5.  SQL server bağlantı açın. AZURE SQL SERVERNAME ve veritabanı için değerleri değiştirmek unutmayın.
    
    ```powershell
    $SqlConnection = New-Object System.Data.SqlClient.SqlConnection
    $SqlConnection.ConnectionString = "Data Source = <AZURE-SQL-SERVERNAME>; Initial Catalog = <DATABASE>"
    $SqlConnection.AccessToken = $AccessToken
    $SqlConnection.Open()
    ```

    Ardından, oluşturun ve bir sorgu sunucuya gönderin.  Tablo için değeri değiştirmek unutmayın.

    ```powershell
    $SqlCmd = New-Object System.Data.SqlClient.SqlCommand
    $SqlCmd.CommandText = "SELECT * from <TABLE>;"
    $SqlCmd.Connection = $SqlConnection
    $SqlAdapter = New-Object System.Data.SqlClient.SqlDataAdapter
    $SqlAdapter.SelectCommand = $SqlCmd
    $DataSet = New-Object System.Data.DataSet
    $SqlAdapter.Fill($DataSet)
    ```

Değerini inceleyin `$DataSet.Tables[0]` sorgu sonuçlarını görüntülemek için.  Tebrikler, bir VM MSI kullanarak veritabanını sorgulama ve kimlik bilgilerini sağlamak için gerek kalmadan!

## <a name="related-content"></a>İlgili içerik

- MSI genel bakış için bkz: [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).
- Daha fazla bilgi edinmek [Azure AD kimlik doğrulaması için Azure SQL desteği](~/articles/sql-database/sql-database-aad-authentication.md).
- Daha fazla bilgi edinmek [Azure AD kimlik doğrulaması için Azure SQL desteği yapılandırma](~/articles/sql-database/sql-database-aad-authentication-configure.md).
- Daha fazla bilgi edinmek [kimlik doğrulama ve erişim SQL Server'daki](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/getting-started-with-database-engine-permissions).

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.
