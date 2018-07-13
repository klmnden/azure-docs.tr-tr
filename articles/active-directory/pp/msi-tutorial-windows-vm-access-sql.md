---
title: Azure SQL'e erişmek için bir Windows VM MSI kullanma
description: Öğretici Azure SQL'e erişmek için bir Windows VM yönetilen hizmet kimliği (MSI) kullanma işlemi gösterilmektedir.
services: active-directory
documentationcenter: ''
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
ms.openlocfilehash: 5c3e2af8c6864204a4c373ac4e1c12090389ac32
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39007435"
---
# <a name="use-a-windows-vm-managed-service-identity-msi-to-access-azure-sql"></a>Azure SQL'e erişmek için bir Windows VM yönetilen hizmet kimliği (MSI) kullanma

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Bu öğreticide bir Azure SQL sunucusuna erişim için bir Windows sanal makine (VM) için Yönetilen hizmet kimliği (MSI) kullanma işlemini gösterir. Yönetilen hizmet kimlikleri, Azure tarafından otomatik olarak yönetilir ve Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuza eklemek zorunda kalmadan destekleyen hizmetler için kimlik doğrulaması sağlar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir Windows VM üzerinde MSI etkinleştir 
> * Bir Azure SQL sunucusu, VM erişimi verme
> * VM kimliğini kullanarak bir erişim belirteci alma ve Azure SQL sunucusunu sorgulamak için kullanın

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Windows sanal makine oluşturun

Bu öğretici için yeni bir Windows VM'yi oluştururuz.  Mevcut VM'yi MSI de etkinleştirebilirsiniz.

1.  Tıklayın **kaynak Oluştur** sol üst köşesinde Azure portal'ın üzerinde.
2.  **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. 
3.  Sanal makine bilgilerini girin. **Kullanıcıadı** ve **parola** için kullandığınız kimlik bilgilerini İşte oluşturulan sanal makineye oturum açma.
4.  Uygun seçin **abonelik** sanal makinenin açılır.
5.  Yeni bir seçilecek **kaynak grubu** seçin, sanal makinenizi oluşturmak, **Yeni Oluştur**. İşlem tamamlandığında **Tamam**’a tıklayın.
6.  Sanal makine için boyutu seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. Ayarları sayfasında, varsayılan değerleri koruyun ve tıklayın **Tamam**.

    ![Alt resim metni](../managed-service-identity/media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>Vm'nizde MSI etkinleştir 

VM MSI, kimlik bilgilerini kodunuza koyma gereksinimi olmadan Azure AD'den erişim belirteci alma olanak tanır. MSI etkinleştirmesine VM'niz için yönetilen bir kimlik oluşturmak için Azure'da söyler. Perde MSI etkinleştirmesine iki şeyi yapar: VM'NİZDE MSI VM uzantısı yükler ve Azure Resource Manager'daki MSI sağlar.

1.  Seçin **sanal makine** MSI etkinleştirmek istiyorsanız.  
2.  Sol gezinti çubuğunda Koruma'ya tıklayın **yapılandırma**. 
3.  Gördüğünüz **yönetilen hizmet kimliği**. Kaydolun ve MSI etkinleştirmek için **Evet**, devre dışı bırakmak istiyorsanız seçin No 
4.  Tıkladığınız olun **Kaydet** yapılandırmayı kaydetmek için.  
    ![Alt resim metni](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Denetleyin ve bu VM üzerinde hangi uzantıları doğrulamak isterseniz tıklayın **uzantıları**. MSI, ardından etkinse **ManagedIdentityExtensionforWindows** listesinde görünür.

    ![Alt resim metni](../managed-service-identity/media/msi-tutorial-windows-vm-access-arm/msi-windows-extension.png)

## <a name="grant-your-vm-access-to-a-database-in-an-azure-sql-server"></a>Bir Azure SQL server veritabanında, VM erişimi verme

Artık bir Azure SQL server veritabanında, VM erişimi verebilir.  Bu adım için mevcut bir SQL server'ı kullanın veya yeni bir tane oluşturun.  Yeni Sunucu oluştur ve Azure portalını kullanarak veritabanı için bu izleyin [SQL Azure Hızlı Başlangıç](~/articles/sql-database/sql-database-get-started-portal.md). Ayrıca, Azure PowerShell ve Azure CLI kullanan hızlı başlangıçlar vardır [Azure SQL belgeleri](~/articles/sql-database/index.yml).

VM erişim için bir veritabanı için üç adım vardır:
1.  Azure AD'de grup oluşturma ve VM MSI grubunun bir üyesi olun.
2.  SQL server için Azure AD kimlik doğrulamasını etkinleştirin.
3.  Oluşturma bir **içerilen kullanıcı** veritabanında Azure AD grubunu temsil eder.

> [!NOTE]
> Genellikle doğrudan için VM MSI eşleyen içerilen kullanıcı oluşturursunuz.  Şu anda Azure SQL için içerilen kullanıcı eşlenmiş VM MSI gösteren Azure AD hizmet sorumlusu izin vermez.  Desteklenen bir geçici çözüm olarak, VM MSI bir Azure AD grubunun bir üyesi olun ve ardından içerilen kullanıcı grubu temsil eden veritabanında oluşturun.


### <a name="create-a-group-in-azure-ad-and-make-the-vm-msi-a-member-of-the-group"></a>Azure AD'de grup oluşturma ve VM MSI grubunun bir üyesi olun

Mevcut bir Azure AD grubunu kullanın veya Azure AD PowerShell kullanarak yeni bir tane oluşturun.  

İlk olarak, yükleme [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) modülü. Ardından kullanarak oturum `Connect-AzureAD`, grubu oluşturmak için aşağıdaki komutu çalıştırın ve bir değişkene kaydedin:

```powershell
$Group = New-AzureADGroup -DisplayName "VM MSI access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
```

Çıktı, ayrıca değişkenin değerini inceler aşağıdaki gibi görünür:

```powershell
$Group = New-AzureADGroup -DisplayName "VM MSI access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
$Group
ObjectId                             DisplayName          Description
--------                             -----------          -----------
6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 VM MSI access to SQL
```

Ardından, sanal makinenin MSI grubuna ekleyin.  MSI ihtiyacınız **objectID**, Azure PowerShell kullanarak elde.  İlk olarak, indirme [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Ardından kullanarak oturum `Connect-AzureRmAccount`, ve aşağıdaki komutları çalıştırın:
- Birden fazla varsa, oturum bağlamı istediğiniz Azure aboneliği için ayarlandığından emin olun.
- Azure aboneliğinizdeki kullanılabilir kaynak listesinde, doğru kaynak grubunu ve sanal makine adları içinde doğrulayın.
- İçin uygun değerleri kullanarak, MSI sanal makinenin özelliklerini alma `<RESOURCE-GROUP>` ve `<VM-NAME>`.

```powershell
Set-AzureRMContext -subscription "bdc79274-6bb9-48a8-bfd8-00c140fxxxx"
Get-AzureRmResource
$VM = Get-AzureRmVm -ResourceGroup <RESOURCE-GROUP> -Name <VM-NAME>
```

Çıktı, ayrıca sanal makinenin MSI hizmet sorumlusu nesne kimliği inceler aşağıdaki gibi görünür:
```powershell
$VM = Get-AzureRmVm -ResourceGroup DevTestGroup -Name DevTestWinVM
$VM.Identity.PrincipalId
b83305de-f496-49ca-9427-e77512f6cc64
```

Artık VM MSI grubuna ekleyin.  Bu gibi durumlarda, bir hizmet sorumlusu yalnızca Azure AD PowerShell kullanarak bir gruba ekleyebilirsiniz.  Şu komutu çalıştırın:
```powershell
Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId $VM.Identity.PrincipalId
```

Ayrıca Grup üyeliğini sonradan incelerseniz, çıktı şu şekilde görünür:

```powershell
Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId $VM.Identity.PrincipalId
Get-AzureAdGroupMember -ObjectId $Group.ObjectId

ObjectId                             AppId                                DisplayName
--------                             -----                                -----------
b83305de-f496-49ca-9427-e77512f6cc64 0b67a6d6-6090-4ab4-b423-d6edda8e5d9f DevTestWinVM
```

### <a name="enable-azure-ad-authentication-for-the-sql-server"></a>SQL server için Azure AD kimlik doğrulamasını etkinleştirme

Grubu oluşturulan ve üyelik için VM MSI eklenen göre yapabilecekleriniz [SQL server için Azure AD kimlik doğrulamasını yapılandırma](~/articles/sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-managed-instance) aşağıdaki adımları kullanarak:

1.  Azure portalında **SQL sunucuları** sol gezinti bölmesinden.
2.  Azure AD kimlik doğrulaması için etkin için SQL server'ı tıklatın.
3.  İçinde **ayarları** bölümü dikey pencerenin tıklayın **Active Directory Yöneticisi**.
4.  Komut çubuğunda **yönetici Ayarla**.
5.  Sunucu Yöneticisi yapılması ve bir Azure AD kullanıcı hesabı seçin **seçin.**
6.  Komut çubuğunda **kaydedin.**

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Azure AD grubunu temsil eden veritabanında içerilen kullanıcı oluşturma

Bu sonraki adım için ihtiyacınız olacak [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). Başlamadan önce ayrıca Azure AD tümleştirmesi hakkında arka plan bilgileri için aşağıdaki makaleleri gözden geçirmek yararlı olabilir:

- [Evrensel kimlik doğrulaması ile SQL veritabanı ve SQL veri ambarı'nı (MFA için SSMS desteği)](~/articles/sql-database/sql-database-ssms-mfa-authentication.md)
- [Yapılandırma ve SQL veritabanı veya SQL veri ambarı ile Azure Active Directory kimlik doğrulamasını Yönet](~/articles/sql-database/sql-database-aad-authentication-configure.md)

1.  SQL Server Management Studio’yu başlatın.
2.  İçinde **sunucuya Bağlan** iletişim kutusunda, SQL server'ınızın adı Enter **sunucu adı** alan.
3.  İçinde **kimlik doğrulaması** alanın, Seç **Active Directory - MFA desteğiyle Evrensel**.
4.  İçinde **kullanıcı adı** sunucu yöneticisi olarak, örneğin, belirlediğiniz bir Azure AD hesabı adını girin helen@woodgroveonline.com
5.  **Seçenekler**’e tıklayın.
6.  İçinde **veritabanına bağlan** yapılandırmak istediğiniz sistem dışı veritabanının adını girin.
7.  **Bağlan**'a tıklayın.  Oturum açma işlemini tamamlayın.
8.  İçinde **Nesne Gezgini**, genişletme **veritabanları** klasör.
9.  Bir kullanıcı veritabanı üzerinde sağ tıklatıp **yeni sorgu**.
10.  Sorgu penceresinde aşağıdaki satırı girin ve tıklayın **yürütme** araç çubuğunda:
    
     ```
     CREATE USER [VM MSI access to SQL] FROM EXTERNAL PROVIDER
     ```
    
     İçerilen kullanıcı grubu oluşturma komutu başarıyla tamamlanması.
11.  Sorgu penceresine işaretini kaldırın, aşağıdaki satırı girin ve tıklayın **yürütme** araç çubuğunda:
     
     ```
     ALTER ROLE db_datareader ADD MEMBER [VM MSI access to SQL]
     ```

     Tüm veritabanını okuma yeteneği içerilen kullanıcı verme komutu başarıyla tamamlamanız gerekir.

VM'de çalışan kod artık konumundan MSI bir belirteç almak ve SQL server kimlik doğrulaması için belirteci kullanın.

## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-call-azure-sql"></a>VM kimliğini kullanarak bir erişim belirteci alma ve Azure SQL çağırmak için kullanın 

Azure SQL MSI kullanarak doğrudan erişim belirteçleri kabul edebilir destekleyen Azure AD kimlik doğrulaması, yerel olarak alınır.  Kullandığınız **erişim belirteci** SQL bağlantı oluşturma yöntemi.  Bu, Azure AD ile tümleştirme Azure SQL'ın bir parçasıdır ve bağlantı dizesini kimlik bilgilerini sağlayarak öğesinden farklıdır.

Erişim belirteci kullanarak SQL bağlantı açma bir .net kod örneği aşağıda verilmiştir.  Bu kod, VM MSI uç noktasına erişmek için VM üzerinde çalıştırmanız gerekir.  **.NET framework 4.6** ya da daha yüksek erişim belirteci yöntemini kullanmak için gereklidir.  AZURE SQL SERVERNAME ve veritabanı değerlerini değiştirin uygun şekilde.  Azure SQL için kaynak Kimliğini Not "https://database.windows.net/".

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

Alternatif olarak, yazma ve VM üzerindeki uygulama dağıtmak zorunda kalmadan test uçtan uca kurulumu için hızlı bir yol PowerShell kullanıyor.

1.  Portalda gidin **sanal makineler** ve Windows sanal makinenizi gidin ve buna **genel bakış**, tıklayın **Connect**. 
2.  Girin, **kullanıcıadı** ve **parola** Windows sanal Makineyi oluştururken, eklediğiniz için. 
3.  Sizin oluşturduğunuz bir **Uzak Masaüstü Bağlantısı** sanal makineyle açın **PowerShell** uzak oturumu içinde. 
4.  PowerShell'in kullanarak `Invoke-WebRequest`, Azure SQL için bir erişim belirteci almak için yerel MSI uç noktasına bir istek oluşturun.

    ```powershell
       $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://database.windows.net/"} -Headers @{Metadata="true"}
    ```
    
    Yanıt, bir JSON nesnesinden bir PowerShell nesnesine dönüştürmek. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```

    Erişim belirteci yanıttan ayıklayın.
    
    ```powershell
    $AccessToken = $content.access_token
    ```

5.  Bir SQL server bağlantısı açın. AZURE SQL SERVERNAME ve veritabanı için değerleri değiştirmeyi unutmayın.
    
    ```powershell
    $SqlConnection = New-Object System.Data.SqlClient.SqlConnection
    $SqlConnection.ConnectionString = "Data Source = <AZURE-SQL-SERVERNAME>; Initial Catalog = <DATABASE>"
    $SqlConnection.AccessToken = $AccessToken
    $SqlConnection.Open()
    ```

    Ardından, oluşturun ve sunucuya bir sorgu gönderin.  Tablo için değerini değiştirmeyi unutmayın.

    ```powershell
    $SqlCmd = New-Object System.Data.SqlClient.SqlCommand
    $SqlCmd.CommandText = "SELECT * from <TABLE>;"
    $SqlCmd.Connection = $SqlConnection
    $SqlAdapter = New-Object System.Data.SqlClient.SqlDataAdapter
    $SqlAdapter.SelectCommand = $SqlCmd
    $DataSet = New-Object System.Data.DataSet
    $SqlAdapter.Fill($DataSet)
    ```

Değerini incelemek `$DataSet.Tables[0]` sorgunun sonuçlarını görüntülemek için.  Tebrikler, VM MSI kullanma veritabanı sorgulanan ve kimlik bilgilerini sağlamak için gerek kalmadan!

## <a name="related-content"></a>İlgili içerik

- MSI genel bakış için bkz. [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).
- Daha fazla bilgi edinin [Azure AD kimlik doğrulaması için Azure SQL desteği](~/articles/sql-database/sql-database-aad-authentication.md).
- Daha fazla bilgi edinin [Azure AD kimlik doğrulaması için Azure SQL desteği yapılandırma](~/articles/sql-database/sql-database-aad-authentication-configure.md).
- Daha fazla bilgi edinin [kimlik doğrulaması ve SQL Server'da erişim](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/getting-started-with-database-engine-permissions).

Aşağıdaki yorum bölümünde geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı kullanın.
