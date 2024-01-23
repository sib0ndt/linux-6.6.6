# Makefile for MT76x8 combo driver

##############################################################
# Common settings
##############################################################
export MTK_COMBO_CHIP=MT7668
export HIF=sdio
export CONFIG_MTK_WIFI_ONLY=m
WLAN_CHIP_ID := mt7668
CONFIG_MT7668=m

PWD=$(shell pwd)
DRIVER_DIR=$(PWD)

ifeq ($(HIF),)
    HIF=sdio
endif

export CONFIG_MTK_COMBO_WIFI_HIF=$(HIF)
MODULE_NAME := wlan_$(WLAN_CHIP_ID)

export CONFIG_MTK_PREALLOC_MEMORY=n

#Mstar platform need use prealloc, 
#but this need platform insmod prealloc.ko, not suit for all platforms
ifeq "$(UTOPIA)" "true"
export CFG_POWER_OFF_CTRL_SUPPORT=y
ifeq ($(ANDROID),true)
export CONFIG_MTK_PREALLOC_MEMORY=y
endif
endif

WPA3_SUPPORT_VERSION := r-base q-base q-cn r-cn p-cn

ifneq ($(filter $(WPA3_SUPPORT_VERSION), $(MTK_ANDROID_VERSION)),)
$(warning wifi driver MTK_ANDROID_VERSION=$(MTK_ANDROID_VERSION))
export CONFIG_MTK_WPA3_SUPPORT=y
endif

##############################################################
# Platform specific
##############################################################
# Set default platform
PLATFORM:=AML950X2

ifeq ($(TARGET_PRODUCT), rango)
$(warning TARGET_PRODUCT is $(TARGET_PRODUCT))
PLATFORM := AML950X2
endif


ifeq ($(PLATFORM),AML950X2)
$(warning PLATFORM is $(PLATFORM))
LINUX_SRC = $(KERNEL_SRC)
endif


##############################################################
# Compile options
##############################################################
#/***** Common part ******/
# Extend stack limit size to avoid build error
PLATFORM_FLAGS += -Wframe-larger-than=1400

# Define maximum different channels supported for ieee80211_iface_combination setting.
CFG_NUM_DIFFERENT_CHANNELS_STA=1
CFG_NUM_DIFFERENT_CHANNELS_P2P=1

# Define initial driver running mode.
# 0=RUNNING_P2P_MODE, 1=RUNNING_AP_MODE, 2=RUNNING_DUAL_AP_MODE, 3=RUNNING_P2P_AP_MODE
CFG_DRIVER_INITIAL_RUNNING_MODE=3

# Define to enable Android wake_lock
CFG_ENABLE_WAKE_LOCK=0

# For wpa_supplicant w/o MTK priv lib
# y: enable, n: disable
CFG_ANDROID_AOSP_PRIV_CMD=y

CFG_DEFAULT_DBG_LEVEL=0xB

CFG_TX_DIRECT_USB=1

CFG_RX_DIRECT_USB=1

CFG_USB_REQ_TX_DATA_FFA_CNT=6

CFG_USB_REQ_TX_DATA_CNT=2

CFG_USB_REQ_RX_DATA_CNT=4

CFG_ENABLE_EFUSE_MAC_ADDR=1

#CFG_SUPPORT_SINGLE_SKU_LOCAL_DB=0

CFG_SUPPORT_DFS_MASTER=1

CFG_SCAN_CHANNEL_SPECIFIED=1

CFG_SUPPORT_ROAMING=0

# Report all bss networks to cfg80211 when do p2p scan
CFG_P2P_SCAN_REPORT_ALL_BSS=0

# Support to change sta, p2p, ap interface names
# y: enable, n: disable
# eg. insmod wlan_mt7668_usb.ko sta=wlan p2p=p2p ap=ap
CFG_DRIVER_INF_NAME_CHANGE=y

CFG_CHIP_RESET_SUPPORT=0

# 1: Enable SDIO RX Tasklet De-Aggregation
# 0: Disable (default)
CFG_SDIO_RX_AGG_TASKLET=0

# Support disable beacon hint to avoid channel flag be changed.
CFG_SUPPORT_DISABLE_BCN_HINTS=n

#/***** Platform dependent part ******/
ifeq ($(PLATFORM),MT53XX)
CFG_DRIVER_INITIAL_RUNNING_MODE=3
CFG_SUPPORT_SINGLE_SKU_LOCAL_DB=0
endif

ifeq ($(WOW_WAKE_UP), 1)
PLATFORM_FLAGS += -DCFG_SUPPORT_WOW_EINT=1
PLATFORM_FLAGS += -DCFG_SUPPORT_MAGIC_PKT_VENDOR_EVENT=1
endif

#/***** Manage configs into compile options ******/
ifneq ($(CFG_NUM_DIFFERENT_CHANNELS_STA),)
PLATFORM_FLAGS += -DCFG_NUM_DIFFERENT_CHANNELS_STA=$(CFG_NUM_DIFFERENT_CHANNELS_STA)
endif

ifneq ($(CFG_NUM_DIFFERENT_CHANNELS_P2P),)
PLATFORM_FLAGS += -DCFG_NUM_DIFFERENT_CHANNELS_P2P=$(CFG_NUM_DIFFERENT_CHANNELS_P2P)
endif

ifneq ($(CFG_DRIVER_INITIAL_RUNNING_MODE),)
PLATFORM_FLAGS += -DCFG_DRIVER_INITIAL_RUNNING_MODE=$(CFG_DRIVER_INITIAL_RUNNING_MODE)
endif

ifneq ($(CFG_ENABLE_WAKE_LOCK),)
PLATFORM_FLAGS += -DCFG_ENABLE_WAKE_LOCK=$(CFG_ENABLE_WAKE_LOCK)
endif

ifneq ($(CFG_DEFAULT_DBG_LEVEL),)
PLATFORM_FLAGS += -DCFG_DEFAULT_DBG_LEVEL=$(CFG_DEFAULT_DBG_LEVEL)
endif

ifneq ($(CFG_TX_DIRECT_USB),)
PLATFORM_FLAGS += -DCFG_TX_DIRECT_USB=$(CFG_TX_DIRECT_USB)
endif

ifneq ($(CFG_RX_DIRECT_USB),)
PLATFORM_FLAGS += -DCFG_RX_DIRECT_USB=$(CFG_RX_DIRECT_USB)
endif

ifneq ($(CFG_USB_REQ_TX_DATA_FFA_CNT),)
PLATFORM_FLAGS += -DCFG_USB_REQ_TX_DATA_FFA_CNT=$(CFG_USB_REQ_TX_DATA_FFA_CNT)
endif

ifneq ($(CFG_USB_REQ_TX_DATA_CNT),)
PLATFORM_FLAGS += -DCFG_USB_REQ_TX_DATA_CNT=$(CFG_USB_REQ_TX_DATA_CNT)
endif

ifneq ($(CFG_USB_REQ_RX_DATA_CNT),)
PLATFORM_FLAGS += -DCFG_USB_REQ_RX_DATA_CNT=$(CFG_USB_REQ_RX_DATA_CNT)
endif

ifneq ($(CFG_SUPPORT_ROAMING),)
PLATFORM_FLAGS += -DCFG_SUPPORT_ROAMING=$(CFG_SUPPORT_ROAMING)
endif

ifneq ($(CFG_ENABLE_EFUSE_MAC_ADDR),)
PLATFORM_FLAGS += -DCFG_ENABLE_EFUSE_MAC_ADDR=$(CFG_ENABLE_EFUSE_MAC_ADDR)
endif

ifeq ($(CFG_DRIVER_INF_NAME_CHANGE), y)
PLATFORM_FLAGS += -DCFG_DRIVER_INF_NAME_CHANGE
endif

ifeq ($(CFG_ANDROID_AOSP_PRIV_CMD), y)
PLATFORM_FLAGS += -DCFG_ANDROID_AOSP_PRIV_CMD
endif

ifneq ($(CFG_SUPPORT_SINGLE_SKU_LOCAL_DB),)
PLATFORM_FLAGS += -DCFG_SUPPORT_SINGLE_SKU_LOCAL_DB=$(CFG_SUPPORT_SINGLE_SKU_LOCAL_DB)
endif

ifneq ($(CFG_SUPPORT_DFS_MASTER),)
PLATFORM_FLAGS += -DCFG_SUPPORT_DFS_MASTER=$(CFG_SUPPORT_DFS_MASTER)
endif

ifneq ($(CFG_P2P_SCAN_REPORT_ALL_BSS),)
PLATFORM_FLAGS += -DCFG_P2P_SCAN_REPORT_ALL_BSS=$(CFG_P2P_SCAN_REPORT_ALL_BSS)
endif

ifneq ($(CFG_SCAN_CHANNEL_SPECIFIED),)
PLATFORM_FLAGS += -DCFG_SCAN_CHANNEL_SPECIFIED=$(CFG_SCAN_CHANNEL_SPECIFIED)
endif

ifneq ($(CFG_CHIP_RESET_SUPPORT),)
PLATFORM_FLAGS += -DCFG_CHIP_RESET_SUPPORT=$(CFG_CHIP_RESET_SUPPORT)
endif

ifneq ($(CFG_SDIO_RX_AGG_TASKLET),)
PLATFORM_FLAGS += -DCFG_SDIO_RX_AGG_TASKLET=$(CFG_SDIO_RX_AGG_TASKLET)
endif

ifeq ($(CFG_SUPPORT_DISABLE_BCN_HINTS), y)
PLATFORM_FLAGS += -DCFG_SUPPORT_DISABLE_BCN_HINTS
endif

#For adjust channel request interval when ais join net work
ifeq ($(CFG_SUPPORT_ADJUST_JOIN_CH_REQ_INTERVAL),y)
PLATFORM_FLAGS += -DCFG_SUPPORT_ADJUST_JOIN_CH_REQ_INTERVAL
endif

ifeq ($(CFG_SUPPORT_P2P_GO_KEEP_RATE_SETTING), y)
PLATFORM_FLAGS += -DCFG_SUPPORT_P2P_GO_KEEP_RATE_SETTING
endif

ifeq ($(CFG_SUPPORT_P2P_GO_11B_RATE),y)
PLATFORM_FLAGS += -DCFG_SUPPORT_P2P_GO_11B_RATE
endif

ifeq ($(CFG_SUPPORT_P2P_OPEN_SECURITY),y)
PLATFORM_FLAGS += -DCFG_SUPPORT_P2P_OPEN_SECURITY
endif

ifeq ($(CFG_SUPPORT_FWPATH_PREFIX),y)
PLATFORM_FLAGS += -DCFG_SUPPORT_FWPATH_PREFIX
endif

# ---------------------------------------------------
# Compile Options
# ---------------------------------------------------
WLAN_CHIP_LIST:=-UMT6620 -UMT6628 -UMT5931 -UMT6630 -UMT6632
ccflags-y += $(WLAN_CHIP_LIST)

#WLAN_CHIP_ID=$(MTK_COMBO_CHIP)
ifeq ($(WLAN_CHIP_ID),)
    WLAN_CHIP_ID := MT6632
endif

ccflags-y += -w
ccflags-y += -DCFG_SUPPORT_DEBUG_FS=0
ccflags-y += -DWLAN_INCLUDE_PROC
ccflags-y += -DCFG_SUPPORT_AGPS_ASSIST=1
ccflags-y += -DCFG_SUPPORT_TSF_USING_BOOTTIME=1
ccflags-y += -Wno-error
#ccflags-y += -Wno-implicit-function-declaration
#ccflags-y += -Wno-incompatible-pointer-types
ccflags-y:=$(filter-out -U$(WLAN_CHIP_ID),$(ccflags-y))
ccflags-y += -DLINUX -D$(WLAN_CHIP_ID)

CONFIG_MTK_WIFI_MCC_SUPPORT=y
CONFIG_MTK_WPA3_SUPPORT=y
ifeq ($(CONFIG_MTK_WIFI_MCC_SUPPORT), y)
    ccflags-y += -DCFG_SUPPORT_CHNL_CONFLICT_REVISE=0
     $(info DCFG_SUPPORT_CHNL_CONFLICT_REVISE=0)
else
    ccflags-y += -DCFG_SUPPORT_CHNL_CONFLICT_REVISE=1
     $(info DCFG_SUPPORT_CHNL_CONFLICT_REVISE=1)
endif

ifeq ($(CONFIG_MTK_AEE_FEATURE), y)
    ccflags-y += -DCFG_SUPPORT_AEE=1
    $(info DCFG_SUPPORT_AEE=1)
else
    ccflags-y += -DCFG_SUPPORT_AEE=0
    $(info DCFG_SUPPORT_AEE=0)
endif

ifeq ($(CONFIG_MTK_WPA3_SUPPORT), y)
    ccflags-y += -DCFG_SUPPORT_CFG80211_AUTH=1
    ccflags-y += -DCFG_SUPPORT_SAE=1
    ccflags-y += -DCFG_SUPPORT_OWE=1
    ccflags-y += -DCFG_SUPPORT_SUITB=1
    ccflags-y += -DCFG_SUPPORT_CFG80211_QUEUE=1
else
    ccflags-y += -DCFG_SUPPORT_CFG80211_AUTH=0
    ccflags-y += -DCFG_SUPPORT_SAE=0
    ccflags-y += -DCFG_SUPPORT_OWE=0
    ccflags-y += -DCFG_SUPPORT_SUITB=0
    ccflags-y += -DCFG_SUPPORT_CFG80211_QUEUE=0
endif

ifeq ($(MUL_WIFI_HW_COMPATIBLE), true)
    $(warning wifi driver MUL_WIFI_HW_COMPATIBLE=$(MUL_WIFI_HW_COMPATIBLE))
    ccflags-y += -DCFG_SUPPORT_MULTI_DONGLE=1
endif

ifeq ($(MDNS_OFFLOAD), true)
    ccflags-y += -DCFG_SUPPORT_MDNS_OFFLOAD=1
endif
# Disable ASSERT() for user load, enable for others
ifneq ($(TARGET_BUILD_VARIANT),user)
    ccflags-y += -DBUILD_QA_DBG=1
else
    ccflags-y += -DBUILD_QA_DBG=0
endif

ifeq ($(CONFIG_MTK_COMBO_WIFI),y)
    ccflags-y += -DCFG_BUILT_IN_DRIVER=1
	ccflags-y += -DCFG_WPS_DISCONNECT=1
else
    ccflags-y += -DCFG_BUILT_IN_DRIVER=0
endif

ifeq ($(CONFIG_MTK_COMBO_WIFI_HIF), sdio)
	ccflags-y += -D_HIF_SDIO=1
else ifeq ($(CONFIG_MTK_COMBO_WIFI_HIF), pcie)
    ccflags-y += -D_HIF_PCIE=1
else ifeq ($(CONFIG_MTK_COMBO_WIFI_HIF), usb)
	ccflags-y += -D_HIF_USB=1
else
    $(error Unsuppoted HIF=$(CONFIG_MTK_COMBO_WIFI_HIF)!!)
endif

ifneq ($(CFG_CFG80211_VERSION),)
VERSION_STR = $(subst ",,$(subst ., , $(subst -, ,$(subst v,,$(CFG_CFG80211_VERSION)))))
$(info VERSION_STR=$(VERSION_STR))
X = $(firstword $(VERSION_STR))
Y = $(word 2 ,$(VERSION_STR))
Z = $(word 3 ,$(VERSION_STR))
VERSION := $(shell echo "$$(( $X * 65536 + $Y * 256 + $Z))" )
ccflags-y += -DCFG_CFG80211_VERSION=$(VERSION)
$(info DCFG_CFG80211_VERSION=$(VERSION))
endif

$(info HIF=$(CONFIG_MTK_COMBO_WIFI_HIF))

ifeq ($(CONFIG_MTK_PASSPOINT_R1_SUPPORT), y)
    ccflags-y += -DCFG_SUPPORT_PASSPOINT=1
    ccflags-y += -DCFG_HS20_DEBUG=1
    ccflags-y += -DCFG_ENABLE_GTK_FRAME_FILTER=1
else ifeq ($(CONFIG_MTK_PASSPOINT_R2_SUPPORT), y)
    ccflags-y += -DCFG_SUPPORT_PASSPOINT=1
    ccflags-y += -DCFG_HS20_DEBUG=1
    ccflags-y += -DCFG_ENABLE_GTK_FRAME_FILTER=1
else
    ccflags-y += -DCFG_SUPPORT_PASSPOINT=0
    ccflags-y += -DCFG_HS20_DEBUG=0
    ccflags-y += -DCFG_ENABLE_GTK_FRAME_FILTER=0
endif

MTK_MET_PROFILING_SUPPORT = yes
ifeq ($(MTK_MET_PROFILING_SUPPORT), yes)
    ccflags-y += -DCFG_MET_PACKET_TRACE_SUPPORT=1
else
    ccflags-y += -DCFG_MET_PACKET_TRACE_SUPPORT=0
endif

MTK_MET_TAG_SUPPORT = no
ifeq ($(MTK_MET_TAG_SUPPORT), yes)
    ccflags-y += -DMET_USER_EVENT_SUPPORT
    ccflags-y += -DCFG_MET_TAG_SUPPORT=1
else
    ccflags-y += -DCFG_MET_TAG_SUPPORT=0
endif

ifeq ($(CFG_POWER_OFF_CTRL_SUPPORT), y)
   ccflags-y += -DCFG_POWER_OFF_CTRL_SUPPORT=1
endif

ifeq ($(MODULE_NAME),)
	MODULE_NAME := wlan_$(shell echo $(strip $(WLAN_CHIP_ID)) | tr A-Z a-z)_$(CONFIG_MTK_COMBO_WIFI_HIF)
endif
#src = $(LINUX_SRC)/$(M)

ccflags-y += -DDBG=0
ccflags-y += -I$(src)/os -I$(src)/os/linux/include
ccflags-y += -I$(src)/include -I$(src)/include/nic -I$(src)/include/mgmt -I$(src)/include/chips
ifeq ($(CONFIG_MTK_COMBO_WIFI_HIF), sdio)
ccflags-y += -I$(src)/os/linux/hif/sdio/include
else ifeq ($(CONFIG_MTK_COMBO_WIFI_HIF), pcie)
ccflags-y += -I$(src)/os/linux/hif/pcie/include
else ifeq ($(CONFIG_MTK_COMBO_WIFI_HIF), usb)
ccflags-y += -I$(src)/os/linux/hif/usb/include
endif

ifneq ($(PLATFORM_FLAGS), )
    ccflags-y += $(PLATFORM_FLAGS)
endif

ifeq ($(CONFIG_MTK_WIFI_ONLY),$(filter $(CONFIG_MTK_WIFI_ONLY),m y))
obj-$(CONFIG_MTK_WIFI_ONLY) += $(MODULE_NAME).o
else
obj-$(CONFIG_MTK_COMBO_WIFI) += $(MODULE_NAME).o
#obj-y += $(MODULE_NAME).o
endif

# ---------------------------------------------------
# Directory List
# ---------------------------------------------------
COMMON_DIR  := common/
OS_DIR      := os/linux/
ifeq ($(CONFIG_MTK_COMBO_WIFI_HIF), sdio)
HIF_DIR	    := os/linux/hif/sdio/
else ifeq ($(CONFIG_MTK_COMBO_WIFI_HIF), pcie)
HIF_DIR     := os/linux/hif/pcie/
else ifeq ($(CONFIG_MTK_COMBO_WIFI_HIF), usb)
HIF_DIR	    := os/linux/hif/usb/
endif
NIC_DIR     := nic/
MGMT_DIR    := mgmt/
CHIPS       := chips/


# ---------------------------------------------------
# Objects List
# ---------------------------------------------------

COMMON_OBJS :=	$(COMMON_DIR)dump.o \
	       		$(COMMON_DIR)wlan_lib.o \
	       		$(COMMON_DIR)wlan_oid.o \
	       		$(COMMON_DIR)wlan_bow.o

NIC_OBJS := $(NIC_DIR)nic.o \
			$(NIC_DIR)nic_tx.o \
			$(NIC_DIR)nic_rx.o \
			$(NIC_DIR)nic_pwr_mgt.o \
            $(NIC_DIR)nic_rate.o \
			$(NIC_DIR)cmd_buf.o \
			$(NIC_DIR)que_mgt.o \
			$(NIC_DIR)nic_cmd_event.o \
			$(NIC_DIR)nic_umac.o

OS_OBJS :=	$(OS_DIR)gl_init.o \
			$(OS_DIR)gl_kal.o  \
			$(OS_DIR)gl_bow.o \
			$(OS_DIR)gl_wext.o \
			$(OS_DIR)gl_wext_priv.o \
			$(OS_DIR)gl_ate_agent.o \
			$(OS_DIR)gl_qa_agent.o \
			$(OS_DIR)gl_hook_api.o \
			$(OS_DIR)gl_rst.o \
			$(OS_DIR)gl_cfg80211.o \
			$(OS_DIR)gl_proc.o \
			$(OS_DIR)gl_vendor.o \
			$(OS_DIR)platform.o

ifeq ($(CONFIG_KASAN),y)
CFLAGS_gl_wext_priv.o += -fno-inline
endif

MGMT_OBJS := $(MGMT_DIR)ais_fsm.o \
			 $(MGMT_DIR)aaa_fsm.o \
			 $(MGMT_DIR)assoc.o \
			 $(MGMT_DIR)auth.o \
			 $(MGMT_DIR)bss.o \
			 $(MGMT_DIR)cnm.o \
			 $(MGMT_DIR)cnm_timer.o \
			 $(MGMT_DIR)cnm_mem.o \
			 $(MGMT_DIR)hem_mbox.o \
			 $(MGMT_DIR)mib.o \
			 $(MGMT_DIR)privacy.o  \
			 $(MGMT_DIR)rate.o \
			 $(MGMT_DIR)rlm.o \
			 $(MGMT_DIR)rlm_domain.o \
			 $(MGMT_DIR)reg_rule.o \
			 $(MGMT_DIR)rlm_obss.o \
			 $(MGMT_DIR)rlm_protection.o \
			 $(MGMT_DIR)rsn.o \
			 $(MGMT_DIR)saa_fsm.o \
			 $(MGMT_DIR)scan.o \
			 $(MGMT_DIR)scan_fsm.o \
             $(MGMT_DIR)swcr.o \
             $(MGMT_DIR)roaming_fsm.o \
             $(MGMT_DIR)tkip_mic.o \
             $(MGMT_DIR)hs20.o \
             $(MGMT_DIR)tdls.o

CHIPS_OBJS := $(CHIPS)mt6632.o \
			  $(CHIPS)mt7668.o
# ---------------------------------------------------
# P2P Objects List
# ---------------------------------------------------

COMMON_OBJS += $(COMMON_DIR)wlan_p2p.o

NIC_OBJS += $(NIC_DIR)p2p_nic.o

OS_OBJS += $(OS_DIR)gl_p2p.o \
           $(OS_DIR)gl_p2p_cfg80211.o \
           $(OS_DIR)gl_p2p_init.o \
           $(OS_DIR)gl_p2p_kal.o

MGMT_OBJS += $(MGMT_DIR)p2p_dev_fsm.o\
            $(MGMT_DIR)p2p_dev_state.o\
            $(MGMT_DIR)p2p_role_fsm.o\
            $(MGMT_DIR)p2p_role_state.o\
            $(MGMT_DIR)p2p_func.o\
            $(MGMT_DIR)p2p_scan.o\
            $(MGMT_DIR)p2p_ie.o\
            $(MGMT_DIR)p2p_rlm.o\
            $(MGMT_DIR)p2p_assoc.o\
            $(MGMT_DIR)p2p_bss.o\
            $(MGMT_DIR)p2p_rlm_obss.o\
            $(MGMT_DIR)p2p_fsm.o

MGMT_OBJS += $(MGMT_DIR)wapi.o

ifeq ($(CONFIG_MTK_COMBO_WIFI_HIF), sdio)
HIF_OBJS :=  $(HIF_DIR)arm.o \
             $(HIF_DIR)sdio.o \
             $(HIF_DIR)sdio_test_driver_core.o \
             $(HIF_DIR)sdio_test_driver_ops.o
else ifeq ($(CONFIG_MTK_COMBO_WIFI_HIF), pcie)
HIF_OBJS :=  $(HIF_DIR)arm.o \
             $(HIF_DIR)pcie.o
else ifeq ($(CONFIG_MTK_COMBO_WIFI_HIF), usb)
HIF_OBJS :=  $(HIF_DIR)usb.o
endif

HIF_OBJS +=  $(HIF_DIR)hal_api.o

$(MODULE_NAME)-objs  += $(COMMON_OBJS)
$(MODULE_NAME)-objs  += $(NIC_OBJS)
$(MODULE_NAME)-objs  += $(OS_OBJS)
$(MODULE_NAME)-objs  += $(HIF_OBJS)
$(MODULE_NAME)-objs  += $(MGMT_OBJS)
$(MODULE_NAME)-objs  += $(CHIPS_OBJS)

#
# mtprealloc
#
ifeq ($(CONFIG_MTK_PREALLOC_MEMORY), y)
ccflags-y += -DCFG_PREALLOC_MEMORY
ccflags-y += -I$(src)/prealloc/include
MODULE_NAME_PREALLOC = mtprealloc
PREALLOC_OBJS := prealloc/prealloc.o
$(MODULE_NAME_PREALLOC)-objs += $(PREALLOC_OBJS)
obj-m += $(MODULE_NAME_PREALLOC).o
endif

#DRIVER_BUILD_DATE=$(shell date +%Y%m%d%H%M%S)
ccflags-y += -DDRIVER_BUILD_DATE=${date}



##############################################################
# Compile settings
##############################################################

all: modules

modules:
	@echo cross_comple = $(CROSS_COMPILE)
	@echo CFLAGS = $(CFLAGS)
	@echo EXTRA_CFLAGS = $(EXTRA_CFLAGS)
	$(MAKE) -C $(KERNEL_SRC) M=$(shell pwd) MODULE_NAME=$(MODULE_NAME) PLATFORM_FLAGS="$(PLATFORM_FLAGS)" modules

modules_install:
	@$(MAKE) INSTALL_MOD_STRIP=1 M=$(M) -C $(KERNEL_SRC) modules_install
	mkdir -p ${OUT_DIR}/../vendor_lib/modules
	cd ${OUT_DIR}/$(M)/; find -name $(MODULE_NAME).ko -exec cp {} ${OUT_DIR}/../vendor_lib/modules/$(MODULE_NAME).ko \;





driver:
	+make -C $(KSRC) M=$(shell pwd)/$(DRIVER_DIR) PLATFORM_FLAGS="$(PLATFORM_FLAGS)" WLAN_CHIP_ID=$(WLAN_CHIP_ID) modules
ifeq ($(PLATFORM),MSTAR)
	$(STRIP) --strip-unneeded $(MODULE_NAME).ko
endif
ifeq ($(PLATFORM),MT53XX)
ifeq ($(DRIVER_PARTIAL_BUILD), y)
	@cd $(DRIVER_DIR) && $(STRIP) --strip-unneeded $(MODULE_NAME).ko
else
	$(STRIP) --strip-unneeded $(OUT_STA_KO)
ifeq ($(CONFIG_MTK_PREALLOC_MEMORY), y)
	$(STRIP) --strip-unneeded $(OUT_STA_KO_DIR)/$(MODULE_NAME)_prealloc.ko
endif
endif
endif



clean: driver_clean


driver_clean:

.PHONY: all clean driver driver_clean

