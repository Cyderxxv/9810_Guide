## Welcome to AOSP bringup guide!
## All of the Instructions lines are from my experience while building AOSP for 9810 devices.
   If I messed something up below, please send me a message with what is to fix.
## So after you have synced your favorite ROM sources, it's time to bringup your trees.

1. Common ROMs bringup guide
In your device trees (Example: android_device_samsung_crownlte), it will always have 2 files that you will have to check it properly.
+ AndroidProducts.mk
+ romname_device.mk (Example: lineage_crownlte.mk)
You'll have to edit those files in order to build your ROM. In this example, I'll use LineageOS as the ROM I'm building.
+ In AndroidProducts.mk
     ```bash
        PRODUCT_MAKEFILES := \
        $(LOCAL_DIR)/arrow_crownlte.mk

    COMMON_LUNCH_CHOICES := \
        arrow_crownlte-eng \
        arrow_crownlte-user \
        arrow_crownlte-userdebug
    ```
    - We just need to edit all the "arrow" into "lineage" and leave everything as it should. 
    - And how do we know the 'romname' when editing those? 
    - Just simply, go to ROM sources (Mostly they're on Github), and check the repo name called "vendor_romname". That is the name you'll need to change in your files.

+ In romname_device.mk  
You'll have to change the filename first. In this example, I'm having arrow_crownlte.mk. So in order to build LineageOS, I'll have to change the filename into lineage_crownlte.mk (As I already explain where is the romname for files in above).
    ```bash
        ## Inherit some common Lineage stuff
    $(call inherit-product, vendor/arrow/config/common.mk)

    ## Device identifier, this must come after all inclusions
    'PRODUCT_NAME := arrow_crownlte
    ```
+ You'll probably see those lines. 
    - ```bash
        $(call inherit-product, vendor/arrow/config/common.mk)
      ```
    - In order to build LineageOS as example, we'll have to edit the above line into:
        ```bash
        $(call inherit-product, vendor/lineage/config/common_full_phone.mk)
        ```
    + Explaination:
        - The path "vendor/lineage/config" is the actual ROM's vendor file path. Let's have a look at LineageOS's vendor:
            ```bash
            https://github.com/LineageOS/android_vendor_lineage/tree/lineage-20.0/config
            ```
        - And how about the "common.mk"? In the ROM's vendor, you'll probably see a lot of "common_etc.mk" files at there. And if you ROM have "common_full_phone.mk" file, you'll need to change "common.mk" into "common_full_phone.mk". And if your ROM DOESN'T have it, simply leave at "common.mk". 
        - Or, some ROMS even have "common_full.mk" but not "common_full_phone.mk" in their vendor. You'll have to check their vendor sources to choose the right thing for you :).
+ ```bash
    PRODUCT_NAME := arrow_crownlte
    ```
    - Just simply change "arrow" into "lineage" or whatever your ROM called as I explained above.

### AND YOU'RE GOOD TO GO!