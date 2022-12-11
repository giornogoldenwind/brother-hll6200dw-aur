# brother-hll6200dw-aur

Here are some notes I have for myself while adopting a package on the AUR that
was very inactive.

Printer drivers aren't philosophically deep, but I'd like to spell out some
pieces of knowledge that most AUR users are laconic about.

## Some background

Brother has a proprietary license for the [LPR](https://en.wikipedia.org/wiki/Line_Printer_Daemon_protocol)
driver, which is used for network printing, and an open source license for the
[CUPS](https://en.wikipedia.org/wiki/CUPS) driver, which is all you need if you
will only print locally via a direct USB connection.

Brother makes separate versions of its drivers: `.rpm`/[RPM](https://en.wikipedia.org/wiki/RPM_Package_Manager)
and `.deb`/[DEB](https://en.wikipedia.org/wiki/Deb_%28file_format%29).  Looking
at a handful of Brother printer drivers on the AUR indicates that the preferred
archive format is RPM.

### Clarification

The ArchWiki has [info](https://wiki.archlinux.org/title/Packaging_Brother_printer_drivers)
on packaging Brother printer drivers.  There seems to be a split in approach
between the DEB and RPM drivers.  On one hand, the DEB drivers can be packaged
separately, where the `*-lpr` driver and `*-cups` driver are 2 separate
packages.  This seems much easier to write the `PKGBUILD`, but not as convenient
as having a single AUR package from the RPM.  However, on the other hand, the
RPM needs to be carefully managed with a patch file, since both the LPR and and
CUPS drivers need to be installed simultaneously for the AUR package to function
properly.  (Yet somehow there are plently of counter examples to the latter?)

Due to this, I have stopped trying to make the RPM package work.  Instead, I
will try to make the DEB versions of the drivers work, since that approach
appears much easier for a beginning AUR package contributor.

### 

## How to find the exact download links

1. This is the download preparation [page](https://support.brother.com/g/b/downloadend.aspx?c=us&lang=en&prod=hll6200dw_us_as&os=128&dlid=dlf102429_000&flang=4&type3=561)
   for the CUPS wrapper, before downloading.
2. This is the download landing page after clicking to agree with the EULA:
   ```
   https://support.brother.com/g/b/downloadhowto.aspx?c=us&lang=en&prod=hll6200dw_us_as&os=128&dlid=dlf102429_000&flang=4&type3=561
   ```
   (Exit out of the "Save as..." prompt, if it appears.)
3. Take `dlf102429` from the download ID `dlid=` value (i.e., `dlid=dlf102429`)
   with the archive name of `hll6200dwcupswrapper-3.5.1-1.i386` to get the download
   link:
   ```
   https://download.brother.com/welcome/dlf102429/hll6200dwcupswrapper-3.5.1-1.i386.deb
   ```

(The DEB archive was chosen here, though the same applies if you choose RPM.)

The same method applies to the LPR driver, without loss of generality.

This method is handy, in case the `dlid=` value ever changes between CUPS
wrapper/LPR driver versions.

(I wrote this all out, since the ArchWiki article didn't spell out how to obtain
the exact download URL for each archive, respectively, and I wanted to look this
up if this happens again the next time I update the AUR packages.)

## Improvements

I added the proprietary Brother license as `license-custom-brother.txt` for the
LPR driver and the [GPLv2](https://en.wikipedia.org/wiki/GNU_General_Public_License#Version_2)
license as `license-glp2.txt` for the CUPS driver.

I changed the [checksum](https://en.wikipedia.org/wiki/Checksum) (for file
integrity) from [MD5](https://en.wikipedia.org/wiki/MD5) to
`b2sum`/[BLAKE2](https://en.wikipedia.org/wiki/BLAKE_(hash_function)#BLAKE2), in
accordance to guidelines for making a `PKGBUILD` on the
[ArchWiki](https://wiki.archlinux.org/title/PKGBUILD#Integrity).

### Long note on not using detached GPG signatures for checksums

Of course, I am aware that this should ideally be verified using a detached GPG
signature for these BLAKE2 checksums, but the most stringent GPG users would
then ask for my GPG key to be signed by the [Web of Trust](https://en.wikipedia.org/wiki/Web_of_trust),
which isn't feasible for most people, who can figure out how to use GPG.  I'd 
have to get my signing key signed by well-known people, such as Richard Stallman
or Laura Poitras, and that isn't going to happen.  Plus, cryptography professor
[Matthew D. Green](https://blog.cryptographyengineering.com/2014/08/13/whats-matter-with-pgp/))
and [Moxie Marlinspike](https://moxie.org/2015/02/24/gpg-and-me.html) have
separately written about the worst of every timeline converges with GPG: how
unforgiving GPG is due to its lack of usability and how easy it is to make
mistakes with GPG means that most people (especially those that are
non-technical) should steer clear of GPG; thus, GPG has and is mostly used to
sign software for those under the most stringent security models: such is the
case for Mullvad VPN, for those **who need a VPN (and not anonymity)** and Qubes
OS, as a few examples.  Even Tails doesn't show the GPG instructions anymore in
2022.  GrapheneOS entirely forgoes GPG in its installation process without any
loss of security.

But alas, there are no upstream Brother GPG signing keys or even checksums.

## Credit

Credit goes to Jordan Selanders/ProfessorChill for making `brother-hll6200dw` on
the [AUR](https://aur.archlinux.org/packages/brother-hll6200dw) and inspiring me
to make an AUR package.

Also thanks to the ArchWiki authors who wrote up how to package Brother printer
drivers for the AUR - there may not be a catch-all Brother driver (like in
Debian), for Arch Linux, though at the least installing each printer is probably
more manageable and better than installing a multiiprinter driver package that
includes many other printers you will never use.

## License

This `PKGBUILD` is licensed under the GPLv3 license.  (I will remove this
license if this license if licensing modified versions of `PKGBUILD` is not
appropriate.)
