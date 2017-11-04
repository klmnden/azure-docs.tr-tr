Bu makaledeki örneklerde sürüm 3.0 veya daha sonraki Azure PowerShell sürümünü gerektirir. Sürüm 3.0 veya üstü, yoksa [sürümünüzü güncelleştirme](/powershell/azureps-cmdlets-docs/) PowerShell Galerisi ya da Web Platformu yükleyicisi kullanarak.

Bir *kaynak grubunun* mevcut etiketlerini görmek şunu kullanın:

```powershell
(Get-AzureRmResourceGroup -Name examplegroup).Tags
```

Bu betik aşağıdaki biçimde veri döndürür:

```powershell
Name                           Value
----                           -----
Dept                           IT
Environment                    Test
```

*Kaynak kimliği belirtilmiş bir kaynağın* mevcut etiketlerini görmek için şunu kullanın:

```powershell
(Get-AzureRmResource -ResourceId {resource-id}).Tags
```

*Kaynak kimliği belirtilmiş bir kaynağın* mevcut etiketlerini görmek için şunu da kullanabilirsiniz:

```powershell
(Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
```

*Belirli bir etikete sahip kaynak gruplarını* almak için şunu kullanın:

```powershell
(Find-AzureRmResourceGroup -Tag @{ Dept="Finance" }).Name
```

*Belirli bir etikete sahip kaynakları* almak için şunu kullanın:

```powershell
(Find-AzureRmResource -TagName Dept -TagValue Finance).Name
```

Bir kaynağa veya kaynak grubuna etiket uyguladığınız her durumda ilgili kaynağın veya kaynak grubunun üzerinde mevcut olan etiketlerin üzerine yazarsınız. Bu nedenle, kaynakta veya kaynak grubunda etiketlerin mevcut olup olmamasına bağlı olarak farklı bir yaklaşım kullanmanız gerekir.

*Mevcut etiketi olmayan bir kaynak grubuna* etiket eklemek için şunu kullanın:

```powershell
Set-AzureRmResourceGroup -Name examplegroup -Tag @{ Dept="IT"; Environment="Test" }
```

*Mevcut etiketleri olan bir kaynak grubuna* etiket eklemek için, mevcut etiketleri alın, yeni etiketi ekleyin ve etiketleri yeniden uygulayın:

```powershell
$tags = (Get-AzureRmResourceGroup -Name examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResourceGroup -Tag $tags -Name examplegroup
```

*Mevcut etiketi olmayan bir kaynağa* etiket eklemek için şunu kullanın:

```powershell
$r = Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceId $r.ResourceId -Force
```

*Mevcut etiketi olan bir kaynağa* etiket eklemek için şunu kullanın:

```powershell
$r = Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup
$r.tags += @{Status="Approved"}
Set-AzureRmResource -Tag $r.Tags -ResourceId $r.ResourceId -Force
```

Bir kaynak grubundaki tüm etiketleri *kaynaklardaki mevcut etiketleri korumadan* gruptaki kaynaklara uygulamak için aşağıdaki betiği kullanın:

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups)
{
    Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
}
```

Bir kaynak grubundaki tüm etiketleri *yinelenmeyen kaynaklardaki mevcut etiketleri koruyarak* gruptaki kaynaklara uygulamak için aşağıdaki betiği kullanın:

```powershell
$group = Get-AzureRmResourceGroup "examplegroup"
if ($group.Tags -ne $null) {
    $resources = $group | Find-AzureRmResource
    foreach ($r in $resources)
    {
        $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags
        foreach ($key in $group.Tags.Keys)
        {
            if (($resourcetags) -AND ($resourcetags.ContainsKey($key))) { $resourcetags.Remove($key) }
        }
        $resourcetags += $group.Tags
        Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
    }
}
```

Tüm etiketleri kaldırmak için bir boş karma tablosu geçirin:

```powershell
Set-AzureRmResourceGroup -Tag @{} -Name examplegroup
```
