cmake_minimum_required(VERSION 3.13)

project(stm32-ssd1306 LANGUAGES C)

# The library cannot be compiled on its own: it needs the consuming project's
# ssd1306_conf.h, STM32 HAL headers, MCU family define (STM32F4, ...) and
# compile flags. It is therefore exposed as an INTERFACE target whose sources
# are compiled as part of whatever target links against it.
add_library(ssd1306 INTERFACE)
add_library(ssd1306::ssd1306 ALIAS ssd1306)

target_sources(ssd1306 INTERFACE
    ${CMAKE_CURRENT_SOURCE_DIR}/ssd1306/ssd1306.c
    ${CMAKE_CURRENT_SOURCE_DIR}/ssd1306/ssd1306_fonts.c
)

target_include_directories(ssd1306 INTERFACE
    ${CMAKE_CURRENT_SOURCE_DIR}/ssd1306
)

# Visual demos (ssd1306_TestAll() etc.) that run on real hardware. Off by
# default since they pull in large bitmaps and snprintf.
option(SSD1306_INCLUDE_TESTS "Compile the visual demos in ssd1306_tests.c" OFF)
if(SSD1306_INCLUDE_TESTS)
    target_sources(ssd1306 INTERFACE
        ${CMAKE_CURRENT_SOURCE_DIR}/ssd1306/ssd1306_tests.c
    )
endif()
