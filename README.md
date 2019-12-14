Archive of the DNS root zone
============================

This repository is generated by a cron job which frequently obtains a
zone transfer of the DNS root zone, commits any changes, and pushes to
GitHub.

The archive dates back to 2014-03-05. There are likely to be gaps
caused by failures in the cron job. (This archive is a low-effort side
project.)


repository size
---------------

*Warning: this repository is multiple gigabytes in size.*

This repository is so big because most of the changes to the root zone
are updated RRSIG records, containing cryptographic data that does not
compress well.


old journal
-----------

The `journal` subdirectory contains an archived BIND journal file
split into pieces. To re-create the file, run the command:

        cat journal/* | zcat -d > root.jnl

You can use `named-journalprint root.jnl` to examine its contents.
This file contains incremental updates to the root zone covering
serial numbers between 2005072701 and 2014030500. There is [a small
unlucky gap][gap] between the end of the journal and the start of the
main archive.

[gap]: https://lists.dns-oarc.net/pipermail/dns-operations/2019-December/019536.html

Many thanks to [David Malone][dwmal1] for providing this journal.

[dwmal1]: https://www.maths.tcd.ie/~dwmalone/


summarize git log
-----------------

The `summarize` subdirectory contains some helper scripts for
selecting which parts of the root zone are shown in `git log --patch`.
Most of the changes to the root zone are to refresh RRSIG records,
which is not very interesting.

The `summarize/gitconfig` file can be appended to your `.git/config`
file to get it set up. This must be done manually because the
`textconv` option runs scripts and git won't let a repository auto-run
scripts.

The `summarize/gitattributes` file is an example that can be copied to
`.gitattributes` and edited to choose which records you want to see in
diffs. The default in the example `gitattributes` file ignores
RRSIG records.


related work
------------

This `saveroot` repository is a companion to [@diffroot on Twitter][diffroot].
The `diffroot` bot uses [nsdiff][] to tweet summaries of how the root
zone changes.

The scripts for updating this `saveroot` repository live alongside
`diffroot`, which is currently not fit for public consumption because
the repository contains secrets.

[diffroot]: https://twitter.com/diffroot

[nsdiff]: https://dotat.at/prog/nsdiff/


about the root zone
-------------------

[IANA's root zone management page][iana] has more information about
how the DNS root zone is changed.

[iana]: https://www.iana.org/domains/root


license
-------

The scripts in the summarize directory are released under the 0BSD license.

I do not know the exact copyright status of the root zone itself.


future work
-----------

It would be cool to:

  * Reconstruct old root zones from the journal. This covers the
    period when the zone was signed in 2010.

  * Write analysis scripts to extract a log of changes per TLD.

  * Publish a less enormous version of this repository that excludes
    RRSIG records.


feedback
--------

Please send comments / suggestions / contributions to me by email.

> Tony Finch <dot@dotat.at> <fanf2@cam.ac.uk>  
> University of Cambridge
