---
title: Azure SQL'e erişmek için Windows VM kullanma
description: Windows VM Yönetilen Hizmet Kimliği kullanarak Azure SQL'e işleminde size yol gösteren bir öğretici.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: bryanla
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: daveba
ms.openlocfilehash: ca920a93d754254390a5c5c5a066be3144b47fc7
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41919453"
---
# <a name="tutorial-use-a-windows-vm-managed-service-identity-to-access-azure-sql"></a>Öğretici: Azure SQL'e erişmek için Windows VM Yönetilen Hizmet Kimliği kullanma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bu öğreticide, Azure SQL sunucusuna erişmek için Windows sanal makinesi (VM) için Yönetilen Hizmet Kimliği'ni nasıl kullanacağınız gösterilir. Yönetilen Hizmet Kimlikleri Azure tarafından otomatik olarak yönetilir kodunuza kimlik bilgileri girmenize gerek kalmadan Azure AD kimlik doğrulamasını destekleyen hizmetlerde kimlik doğrulaması yapmanıza olanak tanır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Windows VM'de Yönetilen Hizmet Kimliği'ni etkinleştirme 
> * VM'nize Azure SQL sunucusu için erişim verme
> * VM kimliğini kullanarak erişim belirteci alma ve Azure SQL sunucusunu sorgulamak için bunu kullanma

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda Windows sanal makinesi oluşturma

Bu öğretici için, yeni bir Windows VM oluşturuyoruz.  Yönetilen Hizmet Kimliği'ni var olan bir VM'de de etkinleştirebilirsiniz.

1.  Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** düğmesine tıklayın.
2.  **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. 
3.  Sanal makine bilgilerini girin. Burada oluşturulan **Kullanıcı adı** ve **Parola**, sanal makinede oturum açmak için kullandığınız kimlik bilgileridir.
4.  Açılan listede sanal makine için uygun **Aboneliği** seçin.
5.  İçinde sanal makinenizi oluşturacağınız yeni bir **Kaynak Grubu** seçmek için **Yeni Oluştur**'u seçin. İşlem tamamlandığında **Tamam**’a tıklayın.
6.  VM'nin boyutunu seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. Ayarlar penceresinde varsayılan değerleri koruyun ve **Tamam**'a tıklayın.

    ![Alternatif resim metni](media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-managed-service-identity-on-your-vm"></a>VM'nizde Yönetilen Hizmet Kimliği'ni etkinleştirme 

VM Yönetilen Hizmet Kimliği, kodunuza kimlik bilgileri yerleştirmeniz gerekmeden Azure AD'den erişim belirteçlerini almanıza olanak tanır. Yönetilen Hizmet Kimliği'nin etkinleştirilmesi Azure'a VM'niz için bir yönetilen kimlik oluşturmasını bildirir. Yönetilen Hizmet Kimliği'nin etkinleştirilmesi arka planda iki işlem gerçekleştirir: yönetilen kimliğini oluşturmak için VM'nizi Azure Active Directory'ye kaydeder ve kimliği VM'de yapılandırır.

1.  Yönetilen Hizmet Kimliği'ni etkinleştirmek istediğiniz **Sanal Makine**'yi seçin.  
2.  Sol gezinti çubuğunda **Yapılandırma**'ya tıklayın. 
3.  **Yönetilen Hizmet Kimliği**'ni görürsünüz. Yönetilen Hizmet Kimliği'ni kaydetmek ve etkinleştirmek için **Evet**'i seçin, devre dışı bırakmak istiyorsanız Hayır'ı seçin. 
4.  Yapılandırmayı kaydetmek için **Kaydet**’e tıkladığınızdan emin olun.  
    ![Alternatif resim metni](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

## <a name="grant-your-vm-access-to-a-database-in-an-azure-sql-server"></a>VM'nize Azure SQL sunucusundaki bir veritabanı için erişim verme

Şimdi VM'nize Azure SQL sunucusundaki bir veritabanı için erişim verebilirsiniz.  Bu adımda, mevcut SQL sunucusunu kullanabilir veya yeni bir sunucu oluşturabilirsiniz.  Azure portalını kullanarak yeni sunucu ve veritabanı oluşturmak için bu [Azure SQL hızlı başlangıcını](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) izleyin. [Azure SQL belgeleri](https://docs.microsoft.com/azure/sql-database/) arasında Azure CLI'nin ve Azure PowerShell'in kullanıldığı hızlı başlangıçlar da vardır.

VM'nize veritabanı erişimi verme işleminin üç adımı vardır:
1.  Azure AD'de grup oluşturma ve VM Yönetilen Hizmet Kimliği'ni gruba üye yapma.
2.  SQL sunucusu için Azure AD kimlik doğrulamasını etkinleştirme.
3.  Azure AD grubunu temsil eden veritabanında bir **içerilen kullanıcı** oluşturun.

> [!NOTE]
> Normalde doğrudan VM'nin Yönetilen Hizmet Kimliği'ne eşlenen bir içerilen kullanıcı oluşturabilirsiniz.  Şu anda Azure SQL, VM Yönetilen Hizmet Kimliği'ni temsil eden Azure AD Hizmet Sorumlusunun içerilen kullanıcıyla eşlenmesine izin vermemektedir.  Desteklenen bir geçici çözüm olarak, VM Yönetilen Hizmet Kimliği'ni Azure AD grubuna üye yapın ve ardından grubu temsil eden veritabanında bir içerilen kullanıcı oluşturun.


### <a name="create-a-group-in-azure-ad-and-make-the-vm-managed-service-identity-a-member-of-the-group"></a>Azure AD'de grup oluşturma ve VM Yönetilen Hizmet Kimliği'ni gruba üye yapma

Mevcut Azure AD grubunu kullanabilir veya Azure AD PowerShell kullanarak yeni bir grup oluşturabilirsiniz.  

İlk olarak [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) modülünü yükleyin. Ardından `Connect-AzureAD` kullanarak oturum açın ve aşağıdaki komutu çalıştırarak grubu oluşturup bir değişkene kaydedin:

```powershell
$Group = New-AzureADGroup -DisplayName "VM Managed Service Identity access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
```

Çıkış aşağıdaki gibi görünür ve değişkenin değeri de incelenir:

```powershell
$Group = New-AzureADGroup -DisplayName "VM Managed Service Identity access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
$Group
ObjectId                             DisplayName          Description
--------                             -----------          -----------
6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 VM Managed Service Identity access to SQL
```

Sonraki adımda VM'nin Yönetilen Hizmet Kimliği'ni gruba ekleyin.  Yönetilen Hizmet Kimliği'nin **ObjectId** değerine ihtiyacınız olacaktır. Bunu, Azure PowerShell'i kullanarak alabilirsiniz.  İlk olarak [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)'i indirin. Sonra `Connect-AzureRmAccount` kullanarak oturum açın ve aşağıdaki komutları çalıştırarak şunları yapın:
- Birden çok Azure aboneliğiniz varsa, oturum bağlamının doğru Azure aboneliğine ayarlandığından emin olun.
- Azure aboneliğinizdeki kullanılabilir kaynakları listeleyerek kaynak grubu ve VM adlarının doğruluğundan emin olun.
- `<RESOURCE-GROUP>` ve `<VM-NAME>` için uygun değerleri kullanarak Yönetilen Hizmet Kimliği VM'sinin özelliklerini alın.

```powershell
Set-AzureRMContext -subscription "bdc79274-6bb9-48a8-bfd8-00c140fxxxx"
Get-AzureRmResource
$VM = Get-AzureRmVm -ResourceGroup <RESOURCE-GROUP> -Name <VM-NAME>
```

Çıkış aşağıdaki gibi görünür ve VM Yönetilen Hizmet Kimliği'nin hizmet sorumlusu Object ID değeri de incelenir:
```powershell
$VM = Get-AzureRmVm -ResourceGroup DevTestGroup -Name DevTestWinVM
$VM.Identity.PrincipalId
b83305de-f496-49ca-9427-e77512f6cc64
```

Şimdi VM Yönetilen Hizmet Kimliği'ni gruba ekleyin.  Yalnızca Azure AD PowerShell kullanarak gruba hizmet sorumlusu ekleyebilirsiniz.  Şu komutu çalıştırın:
```powershell
Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId $VM.Identity.PrincipalId
```

Daha sonra grup üyeliğini de incelerseniz, çıkış şöyle görünür:

```powershell
Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId $VM.Identity.PrincipalId
Get-AzureAdGroupMember -ObjectId $Group.ObjectId

ObjectId                             AppId                                DisplayName
--------                             -----                                -----------
b83305de-f496-49ca-9427-e77512f6cc64 0b67a6d6-6090-4ab4-b423-d6edda8e5d9f DevTestWinVM
```

### <a name="enable-azure-ad-authentication-for-the-sql-server"></a>SQL sunucusu için Azure AD kimlik doğrulamasını etkinleştirme

Artık grubu oluşturduğunuza ve VM Yönetilen Hizmet Kimliği'ni üyeliğe eklediğinize göre, aşağıdaki adımları kullanarak [SQL sunucusu için Azure AD kimlik doğrulamasını yapılandırabilirsiniz](/azure/sql-database/sql-database-aad-authentication-configure#provision-an-azure-active-directory-administrator-for-your-azure-sql-server):

1.  Azure portalında, sol gezintiden **SQL sunucuları**'nı seçin.
2.  Azure AD kimlik doğrulaması için etkinleştirilecek SQL sunucusuna tıklayın.
3.  Dikey pencerenin **Ayarlar** bölümünde **Active Directory yöneticisi**'ne tıklayın.
4.  Komut çubuğunda **Yönetici ayarla**'ya tıklayın.
5.  Sunucunun yöneticisi olacak bir Azure AD kullanıcı hesabı seçin ve **Seç**'e tıklayın.
6.  Komut çubuğunda **Kaydet**'e tıklayın.

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Azure AD grubunu temsil eden veritabanında bir içerilen kullanıcı oluşturma

Bu sonraki adım için, [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)'ya (SSMS) ihtiyacınız vardır. Başlamadan önce, Azure Ad tümleştirmesiyle ilgili arka plan bilgileri için aşağıdaki makaleleri gözden geçirmeniz yararlı olabilir:

- [SQL Veritabanı ve SQL Veri Ambarı ile Evrensel Kimlik Doğrulaması (MFA için SSMS desteği)](/azure/sql-database/sql-database-ssms-mfa-authentication)
- [SQL Veritabanı veya SQL Veri Ambarı ile Azure Active Directory kimlik doğrulamasını yapılandırma ve yönetme](/azure/sql-database/sql-database-aad-authentication-configure)

1.  SQL Server Management Studio’yu başlatın.
2.  **Sunucuya Bağlan** iletişim kutusunda, **Sunucu adı** alanına SQL sunucu adınızı girin.
3.  **Kimlik Doğrulaması** alanında, **Active Directory - MFA ile Evrensel desteği**'ni seçin.
4.  **Kullanıcı adı** alanında, sunucu yöneticisi olarak ayarladığınız Azure AD hesabının adını girin (örneğin, helen@woodgroveonline.com)
5.  **Seçenekler**’e tıklayın.
6.  **Veritabanına bağlan** alanında, yapılandırmak istediğiniz sistem dışı veritabanının adını girin.
7.  **Bağlan**'a tıklayın.  Oturum açma işlemini tamamlayın.
8.  **Nesne Gezgini**'nde **Veritabanları** klasörünü genişletin.
9.  Kullanıcı veritabanına sağ tıklayın ve **Yeni sorgu**'ya tıklayın.
10.  Sorgu penceresinde, aşağıdaki satırı girin ve araç çubuğunda **Yürüt**'e tıklayın:
    
     ```
     CREATE USER [VM Managed Service Identity access to SQL] FROM EXTERNAL PROVIDER
     ```
    
     Komutun başarıyla tamamlanması ve grup için içerilen kullanıcıyı oluşturması gerekir.
11.  Sorgu penceresini temizleyin, aşağıdaki satırı girin ve araç çubuğunda **Yürüt**'e tıklayın:
     
     ```
     ALTER ROLE db_datareader ADD MEMBER [VM Managed Service Identity access to SQL]
     ```

     Komutun başarıyla tamamlanması ve içerilen kullanıcıya veritabanının tamamını okuma erişimi vermesi gerekir.

VM'de çalıştırılan kod şimdi Yönetilen Hizmet Kimliği'nden belirteç alabilir ve belirteci kullanarak SQL Server'da kimlik doğrulaması yapabilir.

## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-call-azure-sql"></a>VM kimliğini kullanarak erişim belirteci alma ve Azure SQL çağrısı yapmak için bunu kullanma 

Azure SQL, Azure AD kimlik doğrulamasını yerel olarak desteklediğinden Yönetilen Hizmet Kimliği kullanılarak alınan erişim belirteçlerini doğrudan kabul eder.  SQL bağlantısı oluştururken **erişim belirteci** yöntemini kullanırsınız.  Bu, Azure SQL’in Azure AD tümleştirmesi kapsamındadır ve bağlantı dizesinde kimlik bilgileri sağlama işleminden farklıdır.

Burada, erişim belirteci kullanarak SQL'e bağlantı açan bir .Net kod örneği verilmiştir.  Bu kodun, VM’nin Yönetilen Hizmet Kimliği uç noktasına erişebilmesi için VM üzerinde çalıştırılması gerekir.  Erişim belirteci yöntemini kullanmak için **.Net Framework 4.6** veya üzeri bir sürüm gereklidir.  AZURE-SQL-SERVERNAME ve DATABASE değerlerini uygun şekilde değiştirin.  Azure SQL için kaynak kimliğinin "https://database.windows.net/" olduğuna dikkat edin.

```csharp
using System.Net;
using System.IO;
using System.Data.SqlClient;
using System.Web.Script.Serialization;

//
// Get an access token for SQL.
//
HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://database.windows.net/");
request.Headers["Metadata"] = "true";
request.Method = "GET";
string accessToken = null;

try
{
    // Call Managed Service Identity endpoint.
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

Alternatif olarak, uygulama yazmak ve VM'de dağıtmak zorunda kalmadan uçtan uca kurulumu test etmenin hızlı bir yöntemi PowerShell kullanmaktır.

1.  Portalda, **Sanal Makineler**'e ve Windows sanal makinenize gidin, ardından **Genel Bakış**'ta **Bağlan**'a tıklayın. 
2.  Windows VM'sini oluştururken eklendiğiniz hesabın **Kullanıcı adı** ve **Parola** değerlerini girin. 
3.  Artık sanal makineyle **Uzak Masaüstü Bağlantısı**'nı oluşturduğunuza göre, uzak oturumda **PowerShell**'i açın. 
4.  PowerShell’in `Invoke-WebRequest` komutunu kullanarak, yerel Yönetilen Hizmet Kimliği uç noktasına Azure SQL için erişim belirteci alma isteğinde bulunun.

    ```powershell
       $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fdatabase.windows.net%2F' -Method GET -Headers @{Metadata="true"}
    ```
    
    Yanıtı JSON nesnesinden PowerShell nesnesine dönüştürün. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```

    Yanıttan erişim belirtecini ayıklayın.
    
    ```powershell
    $AccessToken = $content.access_token
    ```

5.  SQL Server'a bir bağlantı açın. AZURE-SQL-SERVERNAME ve DATABASE değerlerini değiştirmeyi unutmayın.
    
    ```powershell
    $SqlConnection = New-Object System.Data.SqlClient.SqlConnection
    $SqlConnection.ConnectionString = "Data Source = <AZURE-SQL-SERVERNAME>; Initial Catalog = <DATABASE>"
    $SqlConnection.AccessToken = $AccessToken
    $SqlConnection.Open()
    ```

    Ardından bir sorgu oluşturup sunucuya gönderin.  TABLE değerini değiştirmeyi unutmayın.

    ```powershell
    $SqlCmd = New-Object System.Data.SqlClient.SqlCommand
    $SqlCmd.CommandText = "SELECT * from <TABLE>;"
    $SqlCmd.Connection = $SqlConnection
    $SqlAdapter = New-Object System.Data.SqlClient.SqlDataAdapter
    $SqlAdapter.SelectCommand = $SqlCmd
    $DataSet = New-Object System.Data.DataSet
    $SqlAdapter.Fill($DataSet)
    ```

Sorgunun sonuçlarını görüntülemek için `$DataSet.Tables[0]` değerini inceleyin.  Tebrikler, VM Yönetilen Hizmet Kimliği'ni kullanarak ve kimlik bilgileri sağlamak zorunda kalmadan veritabanını sorguladınız!

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure SQL Server'a erişmek için Yönetilen Hizmet Kimliği oluşturmayı öğrendiniz.  Azure SQL Server hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
>[Azure SQL Veritabanı hizmeti](/azure/sql-database/sql-database-technical-overview)
