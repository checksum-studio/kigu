<p align="center">
  <img src="assets/logo.png" alt="kigu" width="280">
</p>

<p align="center">Peer-to-peer VTuber collabs, minus the green screen.</p>

<p align="center">
  <a href="../../releases/latest"><img src="https://img.shields.io/github/v/release/checksum-studio/kigu?label=release" alt="Latest release"></a>
  <a href="../../releases"><img src="https://img.shields.io/github/downloads/checksum-studio/kigu/total" alt="Downloads"></a>
  <a href="../../releases/latest"><img src="https://img.shields.io/github/release-date/checksum-studio/kigu?display_date=published_at&amp;label=last%20release" alt="Last release date"></a>
  <a href="https://discord.gg/zG5YMVtmtp"><img src="https://img.shields.io/badge/Discord-join-5865F2?logo=discord&amp;logoColor=white" alt="Discord"></a>
</p>

<p align="center">
  <a href="https://ko-fi.com/checksum"><img src="https://ko-fi.com/img/githubbutton_sm.svg" alt="Support kigu on Ko-fi"></a>
</p>

kigu is a VTuber collab app for Windows. It takes your avatar from a Spout app such as VSeeFace, VNyan or Warudo,
connects you with up to 7 partners, and gives OBS one Spout source with everyone in it.

## Download

Get `kigu-win-Setup.exe` from [Releases](../../releases/latest) or [itch.io](https://checksumstudio.itch.io/kigu).
kigu updates itself, and each release lists what changed.

The installer isn't code-signed yet, so Windows may say "Windows protected your PC". Click More info, then Run
anyway. To check your download, compare `Get-FileHash .\kigu-win-Setup.exe` with the release's SHA256SUMS.txt.

## Requirements

- Windows 10 22H2 or Windows 11, 64-bit
- A GPU whose driver supports Vulkan Video H.264 encode and decode. NVIDIA RTX cards are tested. AMD and Intel
  cards with Vulkan Video should work but are less tested.

## Help and bugs

Ask questions on [Discord](https://discord.gg/zG5YMVtmtp). For bugs, search the [open issues](../../issues) first,
then [open a new one](../../issues/new/choose). In kigu, Help > Copy diagnostics saves a zip to attach, with names,
addresses and paths removed.

## Privacy

Video goes directly between you and your partners when your networks allow it. When they don't, it passes through
a relay server run by number 0, the makers of the networking library kigu uses, and stays end-to-end encrypted.
See the [privacy policy](PRIVACY.md).

## Uninstall

To delete your settings as well, first use About > Remove my data in kigu. Then remove kigu in Windows Settings >
Apps.

## License

kigu is free and closed source. This repository holds only its releases and issue tracker. The license is
EULA.txt, included with the download. Tips on [Ko-fi](https://ko-fi.com/checksum) are welcome; they are gifts and
unlock nothing.
