# Домашние задание по теме "Загрузка системы"

1. Попасть в систему без пароля несколькими способами
2. В системе с LVM переименовать VG
3. Добавить модуль в initrd


### 1.  Попасть в систему без пароля несколькими способами

####    1.1 Дописать init=/bin/sh
        1.1.1 Во время загрузки системы (Centos7) при появлении меню Grub нажимаем клавишу "e"
        1.1.2 Находим строку нчинающеюся с linux16 и дописываем в конце init=/bin/sh
        1.1.3 Нажимаем Ctr+x, загружаемся... 
        1.1.4 Монтируем файловую систему в режиме Read-Write # mount -o remount, rw /
        1.1.5 Далее можно поменять пароль root # passwd root

####    1.2 Дописать rd.break
        1.2.1 Во время загрузки системы (Centos7) при появлении меню Grub нажимаем клавишу "e"
        1.2.2 Находим строку нчинающеюся с linux16 и дописываем в конце rd.break
        1.2.3 Нажимаем Ctr+x, загружаемся...
        1.2.4 Монтируем файловую систему в режиме Read-Write # mount -o remount, rw /sysroot
        1.2.5 Представим директории sysroot в качестве корня # chroot /sysroot
        1.2.6 Далее можно поменять пароль root # passwd root
        1.2.7 Включаем переименование /etc/shadow файла SELinux # touch /.autorelabel


####    1.3 Заменить ro на rw init=/sysroot/bin/sh
        1.3.1 Во время загрузки системы (Centos7) при появлении меню Grub нажимаем клавишу "e"
        1.3.2 Находим строку нчинающеюся с linux16 и меняем ro на rw init=/sysroot/bin/sh
        1.3.3 Нажимаем Ctr+x, загружаемся...
        1.3.4 Представим директории sysroot в качестве корня # chroot /sysroot
        1.3.5 Далее можно поменять пароль root # passwd root
 
### 2.  В системе с LVM переименовать VG
        2.1 Проверяем состояние системы # vgs
         ** VG     #PV #LV #SN Attr   VSize   VFree **
         centos   1   2   0 wz--n- <29.00g 4.00m
        2.2 Переименуем VG # vgrename centos OtusRoot
        ** Volume group "centos" successfully renamed to "OtusRoot" **
        2.3 Сделаем правки в файлах # sed -i 's/centos/OtusRoot/g' {/etc/fstab, /etc/default/grub, /boot/grub2/grub.cfg, /etc/grub.d/*}
        2.4 Пересоздаем initrd image, чтобы он знал новое назвние Volume Group # mkinitrd -f -v /boot/initramfs-$(uname -r).img $(uname -r)


        

    
