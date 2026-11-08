# Kit layout

The contents of `kit/` go at the very top of the kit, keeping the exact same locations and contents.

| This repository | Inside the kit | Contents |
| --- | --- | --- |
| `kit/Start-PocketGPU.cmd` | Same location | Entry point for automatic setup |
| `kit/Setup-PocketGPU.cmd` | Same location | Entry point for installing everything and configuring it (View -> Ask and install -> iPhone app guide -> Final check) |
| `kit/Run-on-iPhone-GPU.cmd` | Same location | Entry point for launching apps using a iPhone as a GPU |
| `kit/Update-PocketGPU.cmd` | Same location | Entry point for updates |
| `kit/Fix-PocketGPU.cmd` | Same location | Entry point for automatic repair |
| `kit/report.cmd`・`kit/report.ps1` | Same location | Summarizes the PC status into a single text file |
| `kit/pocketgpu.cmd` | Same location | Entry point for launching (for Steam launch options) |
| `kit/pocketgpu_android.cmd`・`kit/check_android.cmd` | Same location | Entry points for Android |
| `kit/Send-Android-Log.cmd` | Same location | Entry point for gathering Android logs into a single zip file. Runs `tools\android-log.ps1` after checking adb and the smartphone via `android\_common.cmd`. Included only when `android\` is part of the kit |
| `kit/Test-Android-Speed.cmd` | Same location | Entry point for testing Android USB speed. Runs `tools\android-speed.ps1` after checking adb and the smartphone via `android\_common.cmd`. Included only when `android\` is part of the kit |
| `kit/android/*.cmd` | `android\` | Starts or stops the Android receiver. `android_app.cmd` is for the PocketGPU app (connects the camera, microphone, speaker, and GPU port). `_common.cmd` handles common parts (locates adb and the smartphone, determines the PC port. If `PGPU_FIXED_PORTS=1`, uses the port as an environment variable without determining it). `_gamemode.cmd` is a request for game mode (performance) called by `start_android.cmd` and `android_app.cmd` after the receiver starts |
| `kit/tools/*.ps1` | `tools\` | Contents of the entry points above (Windows PowerShell 5.1). `pgpu_port.ps1` selects the PC-side port for Android and saves it in `android\port.txt`. `android-log.ps1` is the content of `Send-Android-Log.cmd`, and `android-speed.ps1` is the content of `Test-Android-Speed.cmd` |
| `kit/tools/setup.ps1`・`kit/tools/setup-admin.ps1` | `tools\` | Contents of `Setup-PocketGPU.cmd`. `setup-admin.ps1` is a small component handling items that require administrator privileges (winget iTunes, Vulkan Runtime, Visual C++, starting Apple services) all at once with a single UAC prompt, and is launched exclusively from `setup.ps1`. It accepts only fixed names |
| `kit/tools/fix-data/*.cmd` | `tools\fix-data\` | New items used by Fix-PocketGPU.cmd to replace older entry points |

## Rules

- The three files in `kit/tools/fix-data/` must have **identical contents** to the three files with the same names at the very top of `kit/` (matching byte-for-byte). This is to ensure that the SHA-256 update verification matches after replacing the entry points.
- Line endings must remain as CRLF. `.gitattributes` prevents them from changing during checkout.
- `.ps1` files must be UTF-8 (with BOM), and `.cmd` files must be ASCII only. They can be fixed using `tests/Normalize-Files.ps1`.
- Passphrases, personal signatures, `update.ini`, `kit.ini`, DLLs, EXEs, and IPAs are not included here.

## Not in this repository 

- `iphone_icd.dll`・`iphone_icd.json`・`x86\`・`vklist.exe`
- `PocketGPU.exe`・`extras\` (camera, microphone, subtitle tools, and virtual camera)
- iPhone app (ipa), Android receiver (`android\server_aarch64`), Android app (`android\pocketgpu.apk`)
- `tools\kit.ini`・`tools\update.ini` (individual settings)
- `android\port.txt` (The port selected and remembered by `tools\pgpu_port.ps1` when Windows has reserved a PC port. Created during use. Formatted as `gpu=47300`. Deleting it reverts to the default)
- `README.txt`・`FAQ.txt`・`dxvk\`

These are added by whoever assembles the kit. The tools (`.cmd` and `.ps1` files in `kit/`) simply invoke them.