include $(DEVDIR)/bsp/mach/bspconfig
export RRSDK_PREPATCH_TARGETS=audio_codecs_checked m3_code_checked
export RRSDK_POSTUNPATCH_TARGETS=cleanup_audio_codecs cleanup_m3_code


ifeq ($(CONFIG_PROPRIETARY_TI_CODECENGINE_DM8168),y)
#Use the new firmware generated for the DSP which supports audio
ifeq ($(CONFIG_PROPRIETARY_EXTERNAL_AUDIO_CODECS_SUPPORT),y)
DSP_FIRMWARE=ezsdk/component-sources/omx_05_02_00_48/bin/dm81xx/bin/ti816x-evm/dm81xx_c6xdsp_debug.xe674
endif

#Use the new firmware generated for the VPSS
ifeq ($(CONFIG_PROPRIETARY_EZSDK_BUILD_FIRMWARE),y)
#Check for DM8168 boards
ifeq ($(CONFIG_PROPRIETARY_TI_CODECENGINE_DM8168),y)
VPSS_FIRMWARE?=ezsdk/component-sources/omx-ti81xx-src_05_02_00_48/bin/dm816xbm/bin/ti816x-evm/dm816xbm_m3vpss_whole_program_debug.xem3
else
#Check for DM8148 boards
VPSS_FIRMWARE?=ezsdk/component-sources/omx-ti81xx-src_05_02_00_48/bin/dm814xbm/bin/ti814x-evm/dm814xbm_m3vpss_whole_program_debug.xem3
endif
endif
endif

ifeq ($(CONFIG_PROPRIETARY_TI_CODECENGINE_DM8148),y)
ifeq ($(CONFIG_PROPRIETARY_EXTERNAL_AUDIO_CODECS_SUPPORT),y)
DSP_FIRMWARE=ezsdk/component-sources/omx_05_02_00_48/bin/dm81xx/bin/ti814x-evm/dm81xx_c6xdsp_debug.xe674
endif
endif

#Firmware to be loaded into the DSP supporting AAC compression
ifeq ($(CONFIG_PROPRIETARY_EXTERNAL_PREBUILT_DSP_FIRMWARE),y)
DSP_FIRMWARE=custom_firmware/dm81xx_c6xdsp_aac.xe674
endif

default_rule: build
audio_codecs_checked:
	$(V) if [ -d ezsdk/component-sources/c674x_aaclcenc_01_00_01_00_elf -a -d ezsdk/component-sources/c674x_mp3dec_01_41_00_00_elf ] ; then \
	     $(ECHO) "  Detected external AAC and MP3 audio codecs, adding support for them." ; \
	     patch -p1 < patches/arch/integrate-aaclenc.patch ; \
	     patch -p1 < patches/arch/integrate-mp3dec.patch ; \
	 fi
	$(V)touch $@

cleanup_audio_codecs:
	$(V) if [ -d ezsdk/component-sources/c674x_aaclcenc_01_00_01_00_elf -a -d ezsdk/component-sources/c674x_mp3dec_01_41_00_00_elf ] ; then \
	        if [ -f audio_codecs_checked ] ; then \
	           patch -R -p1 < patches/arch/integrate-mp3dec.patch ; \
	           patch -R -p1 < patches/arch/integrate-aaclenc.patch ; \
	        fi; \
	 fi
	$(V)rm -rf audio_codecs_checked

m3_code_checked:
	$(V) if [ -e ezsdk/Overlay.Makefile ] ; then \
	     $(ECHO) "  Detected Overlay package installed on EZSDK, building M3 code..." ; \
	     patch -p1 < patches/arch/integrate-overlay.patch ; \
	 fi
	$(V)touch $@

cleanup_m3_code:
	$(V) if [ -e ezsdk/Overlay.Makefile ] ; then \
	        if [ -f m3_code_checked ] ; then \
	           patch -R -p1 < patches/arch/integrate-overlay.patch ; \
	        fi; \
	 fi
	$(V)rm -rf m3_code_checked

