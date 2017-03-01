AzureRm.Resources modülünün 3.0 sürümünde etiketlerle çalışma yönteminde önemli değişiklikler yapılmıştır. Devam etmeden önce sürümünüzü kontrol edin:

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

Sonuçlarınız 3.0 veya üzerindeki bir sürümü gösteriyorsa bu konudaki örnekler ortamınızda çalışır. 3.0 veya sonraki sürüme sahip değilseniz bu konuya devam etmeden önce PowerShell Galerisi veya Web Platform Yükleyicisi’ni kullanarak [sürümünüzü güncelleştirin](/powershell/azureps-cmdlets-docs/).

```powershell
Version
-------
3.5.0
```

Bir kaynağa veya kaynak grubuna etiket uyguladığınız her durumda ilgili kaynağın veya kaynak grubunun üzerinde mevcut olan etiketlerin üzerine yazarsınız. Bu nedenle, kaynakta veya kaynak grubunda korumak istediğiniz etiketlerin mevcut olup olmamasına bağlı olarak farklı bir yaklaşım kullanmanız gerekir. Etiketleri uygulayacağınız yer:

* mevcut etiketler içeren bir kaynak grubu.

  ```powershell
  Set-AzureRmResourceGroup -Name TagTestGroup -Tag @{ Dept="IT"; Environment="Test" }
  ```

* mevcut etiketler içermeyen bir kaynak grubu.

  ```powershell
  $tags = (Get-AzureRmResourceGroup -Name TagTestGroup).Tags
  $tags += @{Status="Approved"}
  Set-AzureRmResourceGroup -Tag $tags -Name TagTestGroup
  ```

* mevcut etiketler içermeyen bir kaynak.

  ```powershell
  Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName storageexample -ResourceGroupName TagTestGroup -ResourceType Microsoft.Storage/storageAccounts
  ```

* mevcut etiketler içeren bir kaynak.

  ```powershell
  $tags = (Get-AzureRmResource -ResourceName storageexample -ResourceGroupName TagTestGroup).Tags
  $tags += @{Status="Approved"}
  Set-AzureRmResource -Tag $tags -ResourceName storageexample -ResourceGroupName TagTestGroup -ResourceType Microsoft.Storage/storageAccounts
  ```

Bir kaynak grubundaki tüm etiketleri **kaynaklardaki mevcut etiketleri korumadan** gruptaki kaynaklara uygulamak için aşağıdaki betiği kullanın:

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force } 
}
```

Bir kaynak grubundaki tüm etiketleri **yinelenmeyen kaynaklardaki mevcut etiketleri koruyarak** gruptaki kaynaklara uygulamak için aşağıdaki betiği kullanın:

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    if ($g.Tags -ne $null) {
        $resources = Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName 
        foreach ($r in $resources)
        {
            $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags
            foreach ($key in $g.Tags.Keys)
            {
                if ($resourcetags.ContainsKey($key)) { $resourcetags.Remove($key) }
            }
            $resourcetags += $g.Tags
            Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
        }
    }
}
```

Tüm etiketleri kaldırmak için bir boş karma tablosu geçirin.

```powershell
Set-AzureRmResourceGroup -Tag @{} -Name TagTestGgroup
```

Belirli bir etikete sahip kaynak gruplarını almak için `Find-AzureRmResourceGroup` cmdlet'ini kullanın.

```powershell
(Find-AzureRmResourceGroup -Tag @{ Dept="Finance" }).Name 
```

Belirli bir etiket ve değere sahip tüm kaynakları almak için `Find-AzureRmResource` cmdlet’ini kullanın.

```powershell
(Find-AzureRmResource -TagName Dept -TagValue Finance).Name
```

