## <a name="sign-in-to-azure"></a>Azure'da oturum açma

`Login-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Login-AzureRmAccount
```

Kullanmak istediğiniz konumu bilmiyorsanız, kullanılabilir konumlarını listeleyebilirsiniz. Liste görüntülendikten sonra kullanmak istediğiniz bir bulun. Bu örnekte **eastus**. Bu bir değişkende saklayın ve tek bir yerde değiştirebilmeniz için değişkeni kullanın.

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

LRS çoğaltma kullanarak standart genel amaçlı depolama hesabı oluşturma [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount), sonra kullanılacak depolama hesabını tanımlayan depolama hesabı bağlamını alır. Bir depolama hesabı üzerinde hareket ederken, tekrar tekrar kimlik bilgileri sağlama yerine bağlamı başvuru. Bu örnek adlı bir depolama hesabı oluşturur *mystorageaccount* ile yerel olarak yedekli depolama ve blob şifreleme etkin.

```powershell
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "mystorageaccount" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob

$ctx = $storageAccount.Context
```
