cmake_minimum_required(VERSION 3.5)

set(ENV{ESP_MATTER_DEVICE_PATH} $ENV{ESP_MATTER_PATH}/device_hal/device/esp32_devkit_c)

set(PROJECT_VER "1.0")
set(PROJECT_VER_NUMBER 1)

set(ESP_MATTER_PATH $ENV{ESP_MATTER_PATH})
set(MATTER_SDK_PATH ${ESP_MATTER_PATH}/connectedhomeip/connectedhomeip)

# This should be done before using the IDF_TARGET variable.
include($ENV{IDF_PATH}/tools/cmake/project.cmake)
include($ENV{ESP_MATTER_DEVICE_PATH}/esp_matter_device.cmake)

set(EXTRA_COMPONENT_DIRS
    "${ESP_MATTER_PATH}/examples/common"
    "${MATTER_SDK_PATH}/config/esp32/components"
    "${ESP_MATTER_PATH}/components"
    "${ESP_MATTER_PATH}/device_hal/device"
    "${CMAKE_CURRENT_SOURCE_DIR}/esp-idf-lib/components/esp_idf_lib_helpers"
    "${CMAKE_CURRENT_SOURCE_DIR}/esp-idf-lib/components/i2cdev"
    "${CMAKE_CURRENT_SOURCE_DIR}/esp-idf-lib/components/sht3x"
    "${CMAKE_CURRENT_SOURCE_DIR}/esp-idf-lib/components/bmp180"
    "${CMAKE_CURRENT_SOURCE_DIR}/esp-idf-lib/components/tsl2591"
    ${extra_components_dirs_append})

project(weather-station)

if(CONFIG_IDF_TARGET_ESP32C2)
    include(relinker)
endif()

idf_build_set_property(CXX_COMPILE_OPTIONS "-std=gnu++17;-Os;-DCHIP_HAVE_CONFIG_H" APPEND)
idf_build_set_property(C_COMPILE_OPTIONS "-Os" APPEND)
# For RISCV chips, project_include.cmake sets -Wno-format, but does not clear various
# flags that depend on -Wformat
idf_build_set_property(COMPILE_OPTIONS "-Wno-format-nonliteral;-Wno-format-security" APPEND)
