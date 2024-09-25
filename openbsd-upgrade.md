# OpenBSD upgrade from 6.8 to 6.9 


```
# run as root 
sysupgrade


# if needed 

sysmerge


# rm -rf /usr/bin/podselect \
          /usr/lib/libperl.so.20.0 \
          /usr/libdata/perl5/*/CORE/dquote_inline.h \
          /usr/libdata/perl5/*/Tie \
          /usr/libdata/perl5/*/auto/Tie \
          /usr/libdata/perl5/Pod/Find.pm \
          /usr/libdata/perl5/Pod/InputObjects.pm \
          /usr/libdata/perl5/Pod/ParseUtils.pm \
          /usr/libdata/perl5/Pod/Parser.pm \
          /usr/libdata/perl5/Pod/PlainText.pm \
          /usr/libdata/perl5/Pod/Select.pm \
          /usr/libdata/perl5/pod/perlce.pod \
          /usr/libdata/perl5/unicore/Heavy.pl \
          /usr/libdata/perl5/unicore/lib/Lb/EB.pl \
          /usr/libdata/perl5/unicore/lib/Perl/_PerlNon.pl \
          /usr/libdata/perl5/unicore/lib/Sc/Armn.pl \
          /usr/libdata/perl5/utf8_heavy.pl \
          /usr/share/man/man1/podselect.1 \
          /usr/share/man/man3p/Pod::Find.3p \
          /usr/share/man/man3p/Pod::InputObjects.3p \
          /usr/share/man/man3p/Pod::ParseUtils.3p \
          /usr/share/man/man3p/Pod::Parser.3p \
          /usr/share/man/man3p/Pod::PlainText.3p \
          /usr/share/man/man3p/Pod::Select.3p
          
          
sysclean 

 
pkg_add -u

```
