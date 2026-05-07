*0. Install required software  (To be updated)*
* Follow AOSP site and install required software: https://source.android.com/docs/setup/start/requirements#install-packages
* Install additional software for this build: sudo pip3 install --upgrade meson ninja mako PyYAML

*1. Download and sync source*
* export my_working_dir=$PWD
* mkdir -pv $my_working_dir/mydroid && cd $my_working_dir/mydroid
* repo init -u https://android.googlesource.com/platform/manifest -b android-17.0.0_r1 --depth=1
* mkdir -pv $my_working_dir/mydroid/.repo/local_manifests/
* git clone https://github.com/tranchikha/android_local_manifest
* cd android_local_manifest && git checkout android17
* cp -rf local_manifest.xml $my_working_dir/mydroid/.repo/local_manifests/
* cd $my_working_dir/mydroid
* repo sync

*2. Build*
* source build/envsetup.sh
* lunch rpi4_atablet-cp31-userdebug
* make systemimage vendorimage creatbootimg -j8 # Note: Change j8 to j1 if your PC has small RAM

*2. Tips for saving build time for next build with ccache (only do below steps at the first Android build time)*
* sudo apt-get install -y ccache
* export USE_CCACHE=1
* mkdir -pv <my_working_dir>/MYCCACHE_DIR (Create the folder on the disk which has more than 50GB available to use ccache)
* export CCACHE_DIR=<my_working_dir>/MYCCACHE_DIR/ (Update <my_working_dir> by your real path)
* export CCACHE_EXEC=/usr/bin/ccache
* ccache -M 50G


*3. Tips for building Android AOSP with low RAM PC (my case: RAM 32GB, Swap 30GB, Android AOSP build thread is killed)*

Android AOSP only consumes much memory when performing first build or update *.bp files to create build/soong files.

1. Increase swap from 30 GB to 50GB. Refer: https://askubuntu.com/questions/178712/how-to-increase-swap-space (Use sudo)
2. Change heap configuration to metalava. Refer: https://github.com/verNANDo57/android_build_soong/commit/ffc8846a01fcfc20d6cf8ca701ef73d99f15acad
3. Set Java heap (put to ~/.bashrc. So that you don't need to re-run next time): export _JAVA_OPTIONS="-Xmx16g"
