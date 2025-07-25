# SPDX-License-Identifier: Apache-2.0

if (CONFIG_BUILD_ONLY_NO_BLOBS)
  message(WARNING "
  ---------------------------------------------------------------------------
  Building only the Bluetooth driver without binary blobs and patches.
  This is only for building (CI) purposes and will not work on a real device.
  ---------------------------------------------------------------------------
  ")
else()

if(CONFIG_DT_HAS_ESPRESSIF_ESP32_BT_HCI_ENABLED)
  zephyr_blobs_verify(MODULE hal_espressif REQUIRED)
endif()

if((CONFIG_DT_HAS_ST_HCI_STM32WBA_ENABLED) OR (CONFIG_DT_HAS_ST_HCI_STM32WB0_ENABLED))
  zephyr_blobs_verify(MODULE hal_stm32 REQUIRED)
endif()

if(CONFIG_DT_HAS_SILABS_BT_HCI_EFR32_ENABLED)
  zephyr_blobs_verify(MODULE hal_silabs REQUIRED)
endif()

if(CONFIG_DT_HAS_INFINEON_CAT1_BLESS_HCI_ENABLED)
  zephyr_blobs_verify(MODULE hal_infineon REQUIRED)
endif()

if(CONFIG_DT_HAS_RENESAS_BT_HCI_DA1469X_ENABLED)
  zephyr_blobs_verify(MODULE hal_renesas REQUIRED)
endif()

endif() # CONFIG_BUILD_ONLY_NO_BLOBS

zephyr_library_sources_ifdef(CONFIG_BT_ESP32       hci_esp32.c)
zephyr_library_sources_ifdef(CONFIG_BT_H4          h4.c)
zephyr_library_sources_ifdef(CONFIG_BT_H5          h5.c)
zephyr_library_sources_ifdef(CONFIG_BT_HCI_IPC     ipc.c)
zephyr_library_sources_ifdef(CONFIG_BT_SPI_ZEPHYR  spi.c)
zephyr_library_sources_ifdef(CONFIG_BT_SPI_BLUENRG hci_spi_st.c)
zephyr_library_sources_ifdef(CONFIG_BT_CYW43XX   h4_ifx_cyw43xxx.c)
zephyr_library_sources_ifdef(CONFIG_BT_CYW208XX  hci_ifx_cyw208xx.c)
zephyr_library_sources_ifdef(CONFIG_BT_STM32_IPM   ipm_stm32wb.c)
zephyr_library_sources_ifdef(CONFIG_BT_STM32WBA    hci_stm32wba.c)
zephyr_library_sources_ifdef(CONFIG_BT_STM32WB0    hci_stm32wb0.c)
zephyr_library_sources_ifdef(CONFIG_BT_SILABS_EFR32 hci_silabs_efr32.c)
zephyr_library_sources_ifdef(CONFIG_BT_SILABS_SIWX91X hci_silabs_siwx91x.c)
zephyr_library_sources_ifdef(CONFIG_BT_PSOC6_BLESS hci_ifx_psoc6_bless.c)
zephyr_library_sources_ifdef(CONFIG_SOC_NRF5340_CPUAPP nrf53_support.c)
zephyr_library_sources_ifdef(CONFIG_BT_AMBIQ_HCI   hci_ambiq.c apollox_blue.c)
zephyr_library_sources_ifdef(CONFIG_BT_DA1453X     hci_da1453x.c)
zephyr_library_sources_ifdef(CONFIG_BT_DA1469X     hci_da1469x.c)
zephyr_library_sources_ifdef(CONFIG_BT_NXP         hci_nxp.c)
zephyr_library_sources_ifdef(CONFIG_BT_H4_NXP_CTLR hci_nxp_setup.c)

if(CONFIG_BT_USERCHAN)
  zephyr_library_sources(userchan.c)
  target_sources(native_simulator INTERFACE userchan_bottom.c)
endif()
