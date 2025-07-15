# AutoUpdate-Spicetify

Automatically keep [Spicetify](https://github.com/spicetify/spicetify-cli) up to date with this hourly scheduled task.  
No more opening a terminal to type `spicetify update` when this script checks for updates silently in the background.


<H2>Installation</H2>

  Download the [AutoUpdate‑Spicetify.ps1](https://github.com/SlawSimulation/AutoUpdate-Spicetify/blob/main/Scripts/AutoUpdate-Spicetify.ps1) file and place it in this folder on your C:\ drive:
  
  ```powershell
  C:\Scripts
  ```
  
  ## Run PowerShell as Administrator and paste this script, then press enter.
  
  ```powershell
  # Recreate SpicetifyAutoUpdate task from scratch (Hourly, for 27 years)
  
  $taskName = 'SpicetifyAutoUpdate'
  $script   = 'C:\Scripts\AutoUpdate-Spicetify.ps1'
  
  # Remove old task if it exists
  if (Get-ScheduledTask -TaskName $taskName -ErrorAction SilentlyContinue) {
      Unregister-ScheduledTask -TaskName $taskName -Confirm:$false
  }
  
  # Define the new action
  $action   = New-ScheduledTaskAction -Execute 'powershell.exe' `
                                       -Argument "-NoProfile -WindowStyle Hidden -ExecutionPolicy Bypass -File `"$script`""
  
  # Hourly trigger (for 27 years / 9999 days)
  $trigger  = New-ScheduledTaskTrigger -Once -At (Get-Date).Date.AddMinutes(1) `
                                       -RepetitionInterval (New-TimeSpan -Hours 1) `
                                       -RepetitionDuration (New-TimeSpan -Days 9999)
  
  # Task settings
  $settings = New-ScheduledTaskSettingsSet -ExecutionTimeLimit (New-TimeSpan -Minutes 15) `
                                           -MultipleInstances IgnoreNew `
                                           -RunOnlyIfNetworkAvailable
  
  # Register the new task
  Register-ScheduledTask -TaskName $taskName `
                         -Action    $action `
                         -Trigger   $trigger `
                         -Settings  $settings `
                         -RunLevel  Highest `
                         -User      "$env:USERNAME"

  ```



## Contributing

If you would like to add other update scripts—for example, ones that run for only one year or update less frequently to give users more options, please request contributor access. Once accepted feel free to add the files to the repository.

## License

[MIT](https://choosealicense.com/licenses/mit/)
