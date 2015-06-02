QEMU 2.2.92 Optimized for Live Migration
Forked from QEMU Orbit Project (for post-copy)


Install with
./configure --target-list=x86_64-softmmu
make
make install


Post-copy needs Kernel with Andrea's Patch

(

copy kernel header file in include/linux and use

./configure --target-list=x86_64-softmmu --extra-cflags="-D__NR_userfaultfd=323"

in case linux-headers are not latest - e.g. kernel 4.1

)


@GOAL
The aim of this project is to automatically choose pre- or post-copy based on a page dirty rate estimate.

A new hmp command dirtyrate has been added that returns the number of pages dirtied since last call to this command.

dirtyrate works from QEMU monitor directly and also from libvirt by using virsh qemu-monitor-command --hmp dirtyrate



@INITIAL MODIFICATIONS

hmp.c
+ hmp_dirtyrate()

hmp.h
+ hmp_dirtyrate();

hmp-commands.hx
+ new command dirtyrate

arch_init.c
+ get_dirty_pages()
+ __get_dirty_pages()
+ __get_dirty_pages();

monitor.h
+ get_dirty_pages();


@TODO
+ add a command dirtyrateps that returns true dirty rate, i.e. the number of pages dirtied per second. 
+ move prototype from monitor.h to somewhere more suitable 
