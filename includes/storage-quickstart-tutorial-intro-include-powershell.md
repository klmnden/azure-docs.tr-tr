## <a name="sign-in-to-azure"></a>Azure'da oturum açma

`Login-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Login-AzureRmAccount
```

Kullanmak istediğiniz konumdan emin değilseniz, kullanılabilir konumları listeleyebilirsiniz. Liste görüntülendikten sonra, kullanmak istediğiniz öğeyi bulun. Bu örnekte **eastus** kullanılmıştır. Bunu bir değişkende depolayın ve tek bir yerde değiştirebilmek için değişkeni kullanın.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) ile yeni bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

```powershell
$resourceGroup = "myResourceGroup"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

LRS çoğaltma kullanarak standart genel amaçlı depolama hesabı oluşturma [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount), sonra kullanılacak depolama hesabını tanımlayan depolama hesabı bağlamını alır. Bir depolama hesabı üzerinde hareket ederken, tekrar tekrar kimlik bilgileri sağlama yerine bağlamı başvuru. Bu örnek adlı bir depolama hesabı oluşturur *mystorageaccount* (varsayılan olarak etkindir) yerel olarak yedekli storage(LRS) ve blob şifrelemesi ile.

```powershell
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "mystorageaccount" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context
```
