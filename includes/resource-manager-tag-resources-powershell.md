AzureRm.Resources modülünün 3.0 sürümünde etiketlerle çalışma yönteminde önemli değişiklikler yapılmıştır. Devam etmeden önce sürümünüzü kontrol edin:

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

Sonuçlarınız 3.0 veya üzerindeki bir sürümü gösteriyorsa bu konudaki örnekler ortamınızda çalışır. 3.0 veya sonraki sürüme sahip değilseniz, bu konu başlığına devam etmeden önce PowerShell Galerisi veya Web Platform Yükleyicisi’ni kullanarak [sürümünüzü güncelleştirin](/powershell/azureps-cmdlets-docs/).

```powershell
Version
-------
3.5.0
```

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
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName examplevnet -ResourceGroupName examplegroup
```

*Mevcut etiketi olan bir kaynağa* etiket eklemek için şunu kullanın:

```powershell
$tags = (Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName examplevnet -ResourceGroupName examplegroup
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

Tüm etiketleri kaldırmak için bir boş karma tablosu geçirin:

```powershell
Set-AzureRmResourceGroup -Tag @{} -Name examplegroup
```



