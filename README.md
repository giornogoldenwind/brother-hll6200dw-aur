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

## Credit

Credit goes to Jordan Selanders/ProfessorChill for creating this AUR package.

## License

This `PKGBUILD` is licensed under the GPLv3 license.  (I will remove this
license if this license if licensing modified versions of `PKGBUILD` is not
appropriate.)
