# STM32Fxxx graphics display drivers

Layer chart, examples circuits and settings:
- Lcd_drv.pdf ( https://github.com/RobertoBenjami/stm32_graphics_display_drivers/blob/master/Lcd_drv.pdf )

LCD I/O driver:
- stm32f0: lcd_io_spi (software SPI, hardware SPI, hardware SPI with DMA)
- stm32f0: lcd_io_gpio8 (8 bit paralell without analog resistive touchscreen)
- stm32f1: lcd_io_spi (software SPI, hardware SPI, hardware SPI with DMA)
- stm32f1: lcd_io_gpio8 (8 bit paralell without analog resistive touchscreen)
- stm32f1: lcd_io_gpio16 (16 bit paralell without analog resistive touchscreen)
- stm32f1: lcdts_io_gpio8 (8 bit paralell with analog resistive touchscreen)
- stm32f2: lcd_io_spi (software SPI, hardware SPI, hardware SPI with DMA)
- stm32f2: lcd_io_gpio8 (8 bit paralell without analog resistive touchscreen)
- stm32f2: lcd_io_fsmc8 (8 bit paralell without analog resistive touchscreen + FSMC or FSMC with DMA)
- stm32f2: lcd_io_gpio16 (16 bit paralell without analog resistive touchscreen)
- stm32f2: lcd_io_fsmc16 (16 bit paralell without analog resistive touchscreen + FSMC or FSMC with DMA)
- stm32f2: lcdts_io_gpio8 (8 bit paralell with analog resistive touchscreen)
- stm32f2: lcdts_io_fsmc8 (8 bit paralell with analog resistive touchscreen + FSMC or FSMC with DMA)
- stm32f3: lcd_io_spi (software SPI, hardware SPI, hardware SPI with DMA)
- stm32f3: lcd_io_gpio8 (8 bit paralell without analog resistive touchscreen)
- stm32f3: lcdts_io_gpio8 (8 bit paralell with analog resistive touchscreen)
- stm32f4: lcd_io_spi (software SPI, hardware SPI, hardware SPI with DMA)
- stm32f4: lcd_io_gpio8 (8 bit paralell without analog resistive touchscreen)
- stm32f4: lcd_io_fsmc8 (8 bit paralell without analog resistive touchscreen + FSMC or FSMC with DMA)
- stm32f4: lcd_io_gpio16 (16 bit paralell without analog resistive touchscreen)
- stm32f4: lcd_io_fsmc16 (16 bit paralell without analog resistive touchscreen + FSMC or FSMC with DMA)
- stm32f4: lcdts_io_gpio8 (8 bit paralell with analog resistive touchscreen)
- stm32f4: lcdts_io_fsmc8 (8 bit paralell with analog resistive touchscreen + FSMC or FSMC with DMA)
- stm32f7: lcd_io_gpio8 (8 bit paralell without analog resistive touchscreen)
- stm32l0: lcd_io_spi (software SPI, hardware SPI, hardware SPI with DMA), not tested!
- stm32l0: lcd_io_gpio8 (8 bit paralell without analog resistive touchscreen), not tested!
- stm32l1: lcd_io_spi (software SPI, hardware SPI, hardware SPI with DMA), not tested!
- stm32l1: lcd_io_gpio8 (8 bit paralell without analog resistive touchscreen), not tested!
- stm32l4: lcd_io_spi (software SPI, hardware SPI, hardware SPI with DMA), not tested!
- stm32l4: lcd_io_gpio8 (8 bit paralell without analog resistive touchscreen), not tested!

LCD driver:
- st7735  (SPI mode tested)
- st7783  (8 bit paralell mode tested)
- ili9325 (8 bit paralell mode tested)
- ili9328 (8 bit paralell mode tested)
- ili9341 (SPI, 8 bit and 16 bit paralell mode, SPI with LTDC mode tested)
- ili9486 (8 bit paralell mode tested)
- ili9488 (8 bit paralell mode tested)
- hx8347g (8 bit paralell mode tested)

App:
- LcdSpeedTest: Lcd speed test 
- TouchCalib: Touchscreen calibration program 
- Paint: Arduino paint clone
- JpgViewer: JPG file viewer from SD card or pendrive
- AnalogClock: Analog Clock demo
  (printf: the result, i use the SWO pin for ST-LINK Serial Wire Viewer (SWV). See:examples/Src/syscalls.c)
- 3d filled vector (from https://github.com/cbm80amiga/ST7789_3D_Filled_Vector_Ext)

How to use starting from zero?
- stm32f103c8 - spi: https://www.youtube.com/watch?v=4NZ1VwuQWhw
- stm32f103c8 - gpio8: https://www.youtube.com/watch?v=2oP4vGotuJA
- stm32f103c8 - gpio8 with touchscreen: https://www.youtube.com/watch?v=qyCctzAbD2g
- stm32f407zet - fsmc16, sdcard, jpg: https://www.youtube.com/watch?v=hfeKMZXt2L8
- stm32f429zi discovery - spi-dma, usbhost pendrive, jpg: https://www.youtube.com/watch?v=Qi8CtNJGWFw

1. Create project for Cubemx
- setting the RCC (Crystal/ceramic resonator)
- setting the debug (SYS / serial wire or trace assyn sw)
- setting the timebase source (i like the basic timer for this)
- if FSMC : setting the FSMC (chip select, memory type = LCD, Lcd reg select, data = 8 or 16 bits, timing)
- if SDCARD : setting the SDIO mode, enable the FATFS, FATFS: USE_LFN, MAX_SS = 4096, FS_LOCK = 5, RTC enabled
- if USB pendrive : setting the USB host, setting the USB power, enable the FATFS, USE_LFN, MAX_SS = 4096, FS_LOCK = 5
- if JPG : enabled the LIBJPEG
- setting the clock configuration
- project settings: project name, toolchain = truestudio, stack size = 0x800
- generate source code
2. Truestudio
- open projects from file system
- open main.c
- add USER CODE BEGIN PFP: void mainApp(void);
- add USER CODE BEGIN WHILE: mainApp();
- open main.h
- add USER CODE BEGIN Includes (#include "stm32f1xx_hal.h" or #include "stm32f4xx_hal.h" or ...)
- add 2 new folder for Src folder (App, Lcd)
- copy file(s) from App/... to App
- copy 4 or 7 files from Drivers to Lcd (lcd.h, bmp.h, stm32_adafruit_lcd.h / c, if touch: ts.h, stm32_adafruit_ts.h / c)
- copy Fonts folder to Lcd folder
- copy io driver to Lcd folder (lcd_io_...h / c or lcdts_io...h / c or...)
- copy lcd driver to Lcd folder (st7735.h / c or ili9325.h /c or...)
- if printf to SWO : copy syscalls.c to Src folder
- setting the configuration the io driver header file (pin settings, speed settings etc...)
- setting the LCD configuration (orientation, touchscreen)
- add include path : Src/Lcd
- setting the compile options (Enable paralell build, optimalization)
- compile, run ...

Example (please unzip the app you like):
- f103c8t_app: (stm32f103c8t HAL applications, cubemx, truestudio)
- f103c8t_app_rtos: (stm32f103c8t HAL-FreeRtos applications, cubemx, truestudio)
- f407vet_app: (stm32f407vet HAL-applications, cubemx, truestudio)
- f407vet_app_rtos: (stm32f407vet HAL-FreeRtos applications, cubemx, truestudio)
- f407vet_app_fsmc: (stm32f407vet HAL applications, FSMC, cubemx, truestudio)
- f407vet_app_rtos_fsmc: (stm32f407vet HAL-FreeRtos applications, FSMC, cubemx, truestudio)
- f407vet_app_fsmc16: (stm32f407vet HAL applications, FSMC 16 bit, cubemx, truestudio)
- f407zet_app_fsmc16_extsram: (stm32f407zet HAL applications, FSMC 16 bit, external 1MB SRAM, cubemx, truestudio)

How to adding the SWO support to cheap stlink ?
https://lujji.github.io/blog/stlink-clone-trace/

