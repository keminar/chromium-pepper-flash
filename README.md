# chromium-pepper-flash

起因
====
	装gentoo时不小心装成32位系统，安装chrome-binary-plugins时发现只提供了amd64的包，所以要手动安装i386包
	以下是我在gentoo上的操作,其它系统也可以使用，另外firefox也可以用只是配置和so动态库存放的位置不同

flash安装参考
====
	https://forums.gentoo.org/viewtopic-p-7556170.html
	http://blog.sina.com.cn/s/blog_858820890102v63w.html

获取32位包
====
	从 http://download.csdn.net/detail/hurenr/8593695
	下载 google-chrome-stable-27.0.1453.110-202711.i386.rpm

获取64位包
====
	从 https://aur.archlinux.org/packages/chromium-pepper-flash/
	wget https://aur.archlinux.org/cgit/aur.git/snapshot/chromium-pepper-flash.tar.gz
	tar zxf chromium-pepper-flash.tar.gz
	cd chromium-pepper-flash
	more PKGBUILD 找到下载地址

	mkdir tmp
	cd tmp
	wget https://dl.google.com/linux/chrome/rpm/stable/x86_64/google-chrome-stable-51.0.2704.63-1.x86_64.rpm

获取amd64包
====
	emerge -av chrome-binary-plugins 报错,因为我的不是amd的cpu
	more /var/tmp/portage/www-plugins/chrome-binary-plugins-51.0.2704.63/temp/environment

	mkdir tmp3
	cd tmp3/
	wget https://dl.google.com/linux/chrome/deb/pool/main/g/google-chrome-stable/google-chrome-stable_51.0.2704.63-1_amd64.deb

rmp 包解压
====
	rpm2tar google-chrome-stable-51.0.2704.63-1.x86_64.rpm
	tar xf google-chrome-stable-51.0.2704.63-1.x86_64.tar
	ls opt/google/chrome/PepperFlash/

deb 包解压
====
	ar -x google-chrome-stable_51.0.2704.63-1_amd64.deb 
	xz -d data.tar.xz 
	tar xf data.tar 
	ls opt/google/chrome/PepperFlash/

flash 安装
====
	# 为了方便我将上面的包全解压好放在这个git了, 大家可以根据需要直接取用
	sudo mkdir /usr/lib/PepperFlash
	# 如果用我解压好的，这里的opt/google/chrome/PepperFlash 换成git里的目录
	sudo cp opt/google/chrome/PepperFlash/* /usr/lib/PepperFlash/
	sudo chmod 644 /usr/lib/PepperFlash/*

	# 查看版本
	grep version /usr/lib/PepperFlash/manifest.json 
		"version": "11.7.700.203",
	# 写入chromium配置
	sudo vim /etc/chromium/default
		CHROMIUM_FLAGS="--ignore-gpu-blacklist --ppapi-flash-path=/usr/lib/PepperFlash/libpepflashplayer.so  --ppapi-flash-version='11.7.700.203'"


