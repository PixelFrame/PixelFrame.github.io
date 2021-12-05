# WinForm over PowerShell

## Get Started
``` PowerShell
Add-Type -AssemblyName System.Windows.Forms;
[System.Windows.Forms.Application]::EnableVisualStyles()

$MainForm = New-Object System.Windows.Forms.Form

$MainForm.ShowDialog() | Out-Null
```

## Designing

## Data Processing

## High DPI Supporting
``` PowerShell
$PInvokeSetDpiAwareness = @'
using System;
using System.Runtime.InteropServices;

public class Win32API{
    [DllImport("SHCore.dll", SetLastError = true)]
    public static extern bool SetProcessDpiAwareness(uint awareness);
    
    [DllImport("kernel32.dll")]
    public static extern uint GetLastError();
}
'@
Add-Type -TypeDefinition $PInvokeSetDpiAwareness -ErrorAction Stop
# If HRESULT != S_OK
if ($SetDpiResult = [Win32API]::SetProcessDpiAwareness(2)) { Write-Host "Set DPI awareness failed! $SetDpiResult" -ForegroundColor Red }

```

## Embedding Binary
``` PowerShell
$iconBase64
$iconBytes = [Convert]::FromBase64String($Script:iconBase64)
$iconStream = [System.IO.MemoryStream]::new($iconBytes, 0, $iconBytes.Length)
$iconHandle = [System.Drawing.Bitmap]::new($iconStream).GetHIcon()
$MainForm.Icon = [System.Drawing.Icon]::FromHandle($iconHandle)
```

## PS1 to EXE