## <a name="delete-a-recovery-services-vault-powershell"></a>Kurtarma Hizmetleri kasasını silme (PowerShell)

1. Kurtarma hizmetleri kasasını alın

        $vault = Get-AzureRmRecoveryServicesVault -Name "ContosoVault"

2. Kasayı silin

        Remove-AzureRmRecoveryServicesVault -Vault $vault

>[!WARNING]
>
> Yukarıdaki komutu büyük bir dikkatle kullanın, bir kasayı yanlışlıkla silerseniz tüm verileri kaybedersiniz. Bu, kalıcı bir eylemdir ve tersine çevrilmesi mümkün değildir.  




<!--HONumber=Feb17_HO3-->


